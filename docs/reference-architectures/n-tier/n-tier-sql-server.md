---
title: n-schichtige Anwendung mit SQL Server
description: Vorgehensweise zur Implementierung einer mehrschichtigen Architektur in Azure für Verfügbarkeit, Sicherheit, Skalierbarkeit und Verwaltbarkeit.
author: MikeWasson
ms.date: 05/03/2018
pnp.series.title: Windows VM workloads
pnp.series.next: multi-region-application
pnp.series.prev: multi-vm
ms.openlocfilehash: 0f170f2fbcbbfeace53db199cb5d3949415b5546
ms.sourcegitcommit: a5e549c15a948f6fb5cec786dbddc8578af3be66
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/06/2018
ms.locfileid: "33673592"
---
# <a name="n-tier-application-with-sql-server"></a>n-schichtige Anwendung mit SQL Server

Diese Referenzarchitektur zeigt, wie Sie für eine n-schichtige Anwendung konfigurierte virtuelle Computer und ein entsprechendes virtuelles Netzwerk mit SQL Server unter Windows für die Datenebene bereitstellen. [**So stellen Sie diese Lösung bereit**.](#deploy-the-solution) 

![[0]][0]

*Laden Sie eine [Visio-Datei][visio-download] mit dieser Architektur herunter.*

## <a name="architecture"></a>Architecture 

Die Architektur besteht aus den folgenden Komponenten:

* **Ressourcengruppe:** [Ressourcengruppen][resource-manager-overview] dienen zum Gruppieren von Ressourcen, damit sie nach Lebensdauer, Besitzer oder anderen Kriterien verwaltet werden können.

* **Virtuelles Netzwerk (VNet) und Subnetze:** Jeder virtuelle Azure-Computer wird in einem virtuellen Netzwerk (VNet) bereitgestellt, das in mehrere Subnetze segmentiert werden kann. Erstellen Sie für jede Schicht ein separates Subnetz. 

* **NSGs:** Verwenden Sie [Netzwerksicherheitsgruppen][nsg] (NSGs), um den Netzwerkdatenverkehr im VNET zu beschränken. In der hier gezeigten 3-schichtigen Architektur akzeptiert die Datenbankschicht beispielsweise keinen Datenverkehr vom Web-Front-End, sondern nur von der Unternehmensschicht und dem Verwaltungssubnetz.

* **Virtuelle Computer:** Empfehlungen zum Konfigurieren von virtuellen Computern finden Sie unter [Run a Windows VM on Azure](./windows-vm.md) (Ausführen eines virtuellen Windows-Computers in Azure) sowie unter [Run a Linux VM on Azure](./linux-vm.md) (Ausführen eines virtuellen Linux-Computers in Azure).

* **Verfügbarkeitsgruppen:** Erstellen Sie eine [Verfügbarkeitsgruppe][azure-availability-sets] für jede Schicht, und stellen Sie mindestens zwei VMs in jeder Schicht bereit. Dies berechtigt die VMs zu einer höheren [Vereinbarung zum Servicelevel (SLA)][vm-sla] für VMs. 

* **VM-Skalierungsgruppe** (nicht im Bild): Eine [VM-Skalierungsgruppe][vmss] stellt eine Alternative zur Verwendung einer Verfügbarkeitsgruppe dar. Mit einer Skalierungsgruppe lassen sich die virtuellen Computer einer Ebene ganz einfach horizontal hochskalieren – entweder manuell oder automatisch auf der Grundlage vordefinierter Regeln.

* **Azure Load Balancer-Instanzen:** Die [Load Balancer-Instanzen][load-balancer] verteilen eingehende Internetanforderungen an die VM-Instanzen. Verwenden Sie einen [öffentlichen Load Balancer][load-balancer-external], um eingehenden Internetdatenverkehr auf die Webschicht zu verteilen, und einen [internen Load Balancer][load-balancer-internal], um Netzwerkdatenverkehr von der Webschicht auf die Unternehmensschicht zu verteilen.

* **Öffentliche IP-Adresse:** Für den Empfang von Internetdatenverkehr benötigt der öffentliche Load Balancer eine öffentliche IP-Adresse.

* **Jumpbox:** Wird auch als [geschützter Host] bezeichnet. Dies ist eine geschützte VM im Netzwerk, die von Administratoren zum Herstellen der Verbindung mit anderen VMs verwendet wird. Die Jumpbox verfügt über eine NSG, die Remotedatenverkehr nur von öffentlichen IP-Adressen zulässt, die in einer Liste mit sicheren Absendern aufgeführt sind. Die NSG sollte Remotedesktop-Datenverkehr (RDP) zulassen.

* **SQL Server Always On-Verfügbarkeitsgruppe:** Stellt Hochverfügbarkeit in der Datenschicht durch Replikation und Failover bereit.

* **AD DS-Server (Active Directory Domain Services):** Die SQL Server-AlwaysOn-Verfügbarkeitsgruppen werden einer Domäne hinzugefügt, um die WSFC-Technologie (Windows Server-Failovercluster) für Failover zu aktivieren. 

* **Azure DNS:** [Azure DNS][azure-dns] ist ein Hostingdienst für DNS-Domänen, der die Namensauflösung unter Verwendung der Microsoft Azure-Infrastruktur durchführt. Durch das Hosten Ihrer Domänen in Azure können Sie Ihre DNS-Einträge mithilfe der gleichen Anmeldeinformationen, APIs, Tools und Abrechnung wie für die anderen Azure-Dienste verwalten.

## <a name="recommendations"></a>Empfehlungen

Ihre Anforderungen können von der hier beschriebenen Architektur abweichen. Verwenden Sie diese Empfehlungen als Startpunkt. 

### <a name="vnet--subnets"></a>VNET/Subnetze

Legen Sie bei der Erstellung des VNET fest, wie viele IP-Adressen Ihre Ressourcen in jedem Subnetz benötigen. Geben Sie mithilfe der [CIDR] eine Subnetzmaske und einen VNET-Adressbereich an, der für die erforderlichen IP-Adressen groß genug ist. Verwenden Sie einen Adressraum, der in die standardmäßigen [privaten IP-Adressblöcke][private-ip-space] 10.0.0.0/8, 172.16.0.0/12 und 192.168.0.0/16 fällt.

Wählen Sie einen Adressbereich, der sich nicht mit Ihrem lokalen Netzwerk überschneidet, für den Fall, dass Sie später ein Gateway zwischen dem VNET und dem lokalen Netzwerk einrichten müssen. Sobald Sie das VNET erstellt haben, können Sie den Adressbereich nicht mehr ändern.

Entwerfen Sie Subnetze unter Berücksichtigung der Funktionalität und Sicherheitsanforderungen. Alle VMs innerhalb derselben Schicht oder Rolle sollten im selben Subnetz platziert werden, was eine Sicherheitsbegrenzung darstellen kann. Weitere Informationen zum Entwerfen von VNETs und Subnetzen finden Sie unter [Planen und Entwerfen von Azure Virtual Networks][plan-network].

### <a name="load-balancers"></a>Load Balancer

Machen Sie die virtuellen Computer nicht direkt über das Internet verfügbar, sondern weisen Sie jedem virtuellen Computer stattdessen eine private IP-Adresse zu. Clients verwenden für die Verbindungsherstellung die IP-Adresse des öffentlichen Load Balancers.

Definieren Sie Lastenausgleichsregeln, um Netzwerkdatenverkehr an die virtuellen Computer weiterzuleiten. Um beispielsweise HTTP-Datenverkehr zu aktivieren, erstellen Sie eine Regel, die Port 80 aus der Front-End-Konfiguration Port 80 im Back-End-Adresspool zuordnet. Wenn ein Client eine HTTP-Anforderung an Port 80 sendet, wählt der Lastenausgleich mithilfe eines [Hashalgorithmus][load-balancer-hashing], der die Quell-IP-Adresse enthält, eine Back-End-IP-Adresse aus. Auf diese Weise werden die Clientanforderungen auf die virtuellen Computer verteilt.

### <a name="network-security-groups"></a>Netzwerksicherheitsgruppen

Verwenden Sie NSG-Regeln, um den Datenverkehr zwischen den Schichten zu beschränken. In der oben gezeigten 3-schichtigen Architektur kommuniziert die Internetschicht beispielsweise nicht direkt mit der Datenbankschicht. Um dies zu erzwingen, sollte die Datenbankschicht eingehenden Datenverkehr aus dem Subnetz der Internetschicht blockieren.  

1. Lehen Sie sämtlichen eingehenden Datenverkehr aus dem VNet ab. (Verwenden Sie den `VIRTUAL_NETWORK`-Tag in der Regel.) 
2. Lassen Sie eingehenden Datenverkehr aus dem Subnetz der Unternehmensschicht zu.  
3. Lassen Sie eingehenden Datenverkehr aus dem Subnetz der Datenbankschicht zu. Diese Regel ermöglicht die Kommunikation zwischen den virtuellen Datenbankcomputern, die für die Replikation und das Failover der Datenbank erforderlich ist.
4. Lassen Sie RDP-Datenverkehr (Port 3389) aus dem Subnetz der Jumpbox zu. Diese Regel erlaubt Administratoren, über die Jumpbox eine Verbindung mit der Datenbankschicht herzustellen.

Erstellen Sie die Regeln 2&ndash;4 mit einer höheren Priorität als die erste Regel, damit sie Vorrang haben.


### <a name="sql-server-always-on-availability-groups"></a>SQL Server Always On-Verfügbarkeitsgruppen

Um Hochverfügbarkeit von SQL Server zu erzielen, werden [AlwaysOn-Verfügbarkeitsgruppen][sql-alwayson] empfohlen. Bei früheren Versionen als Windows Server 2016 erfordern AlwaysOn-Verfügbarkeitsgruppen einen Domänencontroller, und alle Knoten in der Verfügbarkeitsgruppe müssen sich in der gleichen AD-Domäne befinden.

Andere Schichten stellen über einen [Verfügbarkeitsgruppenlistener][sql-alwayson-listeners] eine Verbindung mit der Datenbank her. Mit dem Listener kann ein SQL-Client eine Verbindung herstellen, ohne den Namen der physischen SQL Server-Instanz zu kennen. VMs, die auf die Datenbank zugreifen, müssen Mitglied der Domäne sein. Der Client (in diesem Fall eine andere Schicht) verwendet DNS, um den Namen des virtuellen Netzwerks des Listeners in IP-Adressen aufzulösen.

Konfigurieren Sie die SQL Server-Always On-Verfügbarkeitsgruppe wie folgt:

1. Erstellen Sie einen WSFC-Cluster (Windows Server Failover Clustering), eine SQL Server Always On-Verfügbarkeitsgruppe und ein primäres Replikat. Weitere Informationen finden Sie unter [Erste Schritte mit Always On-Verfügbarkeitsgruppen (SQL Server)][sql-alwayson-getting-started]. 
2. Erstellen Sie ein internes Lastenausgleichsmodul mit einer statischen privaten IP-Adresse.
3. Erstellen Sie einen Verfügbarkeitsgruppenlistener, und ordnen Sie den DNS-Namen des Listeners der IP-Adresse eines internen Lastenausgleichsmoduls zu. 
4. Erstellen Sie eine Lastenausgleichsregel für den SQL Server-Überwachungsport (standardmäßig TCP-Port 1433). Die Lastenausgleichsregel muss *Floating IP*, auch als „Direct Server Return“ bezeichnet, aktivieren. Dies bewirkt, dass die VM direkt auf den Client antwortet, was eine direkte Verbindung mit dem primären Replikat ermöglicht.
  
   > [!NOTE]
   > Wenn die Floating IP aktiviert ist, muss die Portnummer des Front-Ends mit der des Back-Ends in der Lastenausgleichsregel übereinstimmen.
   > 
   > 

Wenn ein SQL-Client versucht, eine Verbindung herzustellen, leitet das Lastenausgleichsmodul die Verbindungsanforderung an das primäre Replikat weiter. Bei einem Failover zu einem anderen Replikat leitet das Lastenausgleichsmodul nachfolgende Anforderungen automatisch an ein neues primäres Replikat weiter. Weitere Informationen finden Sie unter [Konfigurieren eines ILB-Listeners für AlwaysOn-Verfügbarkeitsgruppen in Azure][sql-alwayson-ilb].

Bei einem Failover werden bestehende Clientverbindungen geschlossen. Nachdem das Failover abgeschlossen ist, werden neue Verbindungen an das neue primäre Replikat weitergeleitet.

Wenn Ihre Anwendung deutlich mehr Lesezugriffe als Schreibzugriffe tätigt, können Sie einige der schreibgeschützten Abfragen in ein sekundäres Replikat auslagern. Weitere Informationen finden Sie unter [Verwenden eines Listeners zum Herstellen einer Verbindung mit einem schreibgeschützten sekundären Replikat (schreibgeschütztes Routing)][sql-alwayson-read-only-routing].

Testen Sie Ihre Bereitstellung, indem Sie [ein manuelles Failover der Verfügbarkeitsgruppe erzwingen][sql-alwayson-force-failover].

### <a name="jumpbox"></a>Jumpbox

Verweigern Sie den RDP-Zugriff über das öffentliche Internet auf die VMs, auf denen die Anwendungsworkload ausgeführt wird. Der gesamte RDP-Zugriff auf diese VMs muss stattdessen über die Jumpbox erfolgen. Ein Administrator meldet sich bei der Jumpbox und von der Jumpbox aus dann bei der anderen VM an. Die Jumpbox lässt RDP-Datenverkehr aus dem Internet zu, jedoch nur von bekannten, sicheren IP-Adressen.

Die Jumpbox hat sehr geringe Leistungsanforderungen. Wählen Sie daher einen virtuellen Computer mit geringer Größe. Erstellen Sie eine [öffentliche IP-Adresse] für die Jumpbox. Platzieren Sie die Jumpbox in dasselbe VNET wie die anderen VMs, jedoch in ein separates Verwaltungssubnetz.

Fügen Sie zum Schutz der Jumpbox eine NSG-Regel hinzu, die ausschließlich RDP-Verbindungen von einer sicheren Gruppe öffentlicher IP-Adressen zulässt. Konfigurieren Sie die NSGs für die anderen Subnetze, um RDP-Datenverkehr aus dem Verwaltungssubnetz zuzulassen.

## <a name="scalability-considerations"></a>Überlegungen zur Skalierbarkeit

[VM-Skalierungsgruppen][vmss] ermöglichen die Bereitstellung und Verwaltung einer Gruppe identischer virtueller Computer. Skalierungsgruppen unterstützen die automatische Skalierung basierend auf Leistungsmetriken. Mit zunehmender Last auf den virtuellen Computern werden dem Lastenausgleich automatisch zusätzliche virtuelle Computer hinzugefügt. Erwägen Sie die Verwendung von Skalierungsgruppen, wenn Sie VMs schnell horizontal hochskalieren müssen oder eine automatische Skalierung benötigen.

Es gibt zwei grundlegende Methoden für das Konfigurieren von virtuellen Computern in einer Skalierungsgruppe:

- Verwenden Sie die Erweiterungen, um den virtuellen Computer zu konfigurieren, nachdem er bereitgestellt wurde. Bei diesem Ansatz dauert das Starten neuer VM-Instanzen länger als bei virtuellen Computern ohne Erweiterungen.

- Stellen Sie einen [verwalteten Datenträger](/azure/storage/storage-managed-disks-overview) mit einem benutzerdefinierten Datenträgerimage bereit. Diese Option kann möglicherweise schneller bereitgestellt werden. Allerdings müssen Sie das Abbild auf dem neuesten Stand halten.

Weitere Überlegungen finden Sie unter [Überlegungen zum Entwurf von Skalierungsgruppen][vmss-design].

> [!TIP]
> Wenn Sie eine Lösung für die automatische Skalierung verwenden, testen Sie diese im Voraus mit Workloads auf Produktionsebene.

Jedes Azure-Abonnement verfügt über Standardeinschränkungen, zu denen auch eine maximale Anzahl von virtuellen Computern pro Region gehört. Sie können den Grenzwert erhöhen, indem Sie eine Supportanfrage einreichen. Weitere Informationen finden Sie unter [Grenzwerte für Azure-Abonnements, -Dienste und -Kontingente sowie allgemeine Beschränkungen][subscription-limits].

## <a name="availability-considerations"></a>Überlegungen zur Verfügbarkeit

Wenn Sie keine VM-Skalierungsgruppen verwenden, platzieren Sie virtuelle Computer auf der gleichen Ebene in einer Verfügbarkeitsgruppe. Erstellen Sie zur Unterstützung der [Verfügbarkeits-SLA für Azure-VMs][vm-sla] mindestens zwei VMs in der Verfügbarkeitsgruppe. Weitere Informationen finden Sie unter [Verwalten der Verfügbarkeit virtueller Computer][availability-set]. 

Der Lastenausgleich verwendet [Integritätstests][health-probes] zur Überwachung der Verfügbarkeit von VM-Instanzen. Wenn eine Instanz bei einem Test nicht innerhalb eines Timeoutzeitraums erreicht werden kann, beendet der Lastenausgleich das Senden von Datenverkehr an diesen virtuellen Computer. Der Lastenausgleich führt aber weiterhin Tests durch, und wenn der virtuelle Computer wieder erreichbar ist, sendet der Lastenausgleich auch wieder Datenverkehr an diese VM.

Empfehlungen für Integritätstests durch den Lastenausgleich:

* Die Tests können für HTTP oder TCP durchgeführt werden. Wenn auf Ihren virtuellen Computern ein HTTP-Server ausgeführt wird, erstellen Sie einen HTTP-Test. Erstellen Sie andernfalls einen TCP-Test.
* Geben Sie für einen HTTP-Test den Pfad zu einem HTTP-Endpunkt an. Der Test überprüft, ob von diesem Pfad eine HTTP-200-Antwort empfangen wird. Dies kann den Stammpfad („/“) oder ein Endpunkt zur Integritätsüberwachung sein, der benutzerdefinierte Logik zum Überprüfen der Integrität der Anwendung implementiert. Der Endpunkt muss anonyme HTTP-Anforderungen zulassen.
* Der Test wird von einer [bekannten][health-probe-ip] IP-Adresse (168.63.129.16) gesendet. Stellen Sie sicher, dass Datenverkehr zu oder von dieser IP-Adresse in keiner Firewallrichtlinie und keinen NSG-Regeln (Netzwerksicherheitsgruppe) blockiert wird.
* Verwenden Sie die [Protokolle der Integritätstests][health-probe-log], um den Status der Integritätstests anzuzeigen. Aktivieren Sie die Protokollierung für jeden Lastenausgleich im Azure-Portal. Die Protokolle werden in Azure Blob Storage geschrieben. Die Protokolle zeigen, wie viele virtuelle Computer am Back-End aufgrund fehlerhafter Testantworten keinen Netzwerkdatenverkehr empfangen.

Wenn Sie eine höhere Verfügbarkeit benötigen, als die [Azure-SLA für VMs][vm-sla] bietet, replizieren Sie ggf. die Anwendung über zwei Regionen, und verwenden Sie den Azure Traffic Manager für das Failover. Weitere Informationen finden Sie unter [Multi-region N-tier application for high availability][multi-dc] (N-schichtige Anwendung in mehreren Regionen für Hochverfügbarkeit).  

## <a name="security-considerations"></a>Sicherheitshinweise

Virtuelle Netzwerke stellen in Azure eine Isolationsbegrenzung für Datenverkehr dar. Virtuelle Computer in einem VNET können nicht direkt mit virtuellen Computern in einem anderen VNET kommunizieren. VMs im selben VNET können miteinander kommunizieren, sofern Sie den Datenverkehr nicht durch die Erstellung von [Netzwerksicherheitsgruppen][nsg] (NSGs) beschränken. Weitere Informationen finden Sie unter [Microsoft Cloud Services und Netzwerksicherheit][network-security].

Für eingehenden Internetdatenverkehr definieren die Lastenausgleichsregeln, welcher Datenverkehr an das Back-End weitergeleitet wird. Allerdings unterstützen Lastenausgleichsregeln keine IP-Sicherheitslisten. Wenn Sie also einer Sicherheitsliste bestimmte öffentliche IP-Adressen hinzufügen möchten, fügen Sie dem Subnetz eine NSG hinzu.

Für die Erstellung einer DMZ zwischen dem Internet und dem virtuellen Azure-Netzwerk sollten Sie eventuell eine virtuelle Netzwerkappliance (Network Virtual Appliance, NVA) hinzufügen. NVA ist ein Oberbegriff für eine virtuelle Appliance, die netzwerkbezogene Aufgaben wie Erstellung von Firewalls, Paketüberprüfung, Überwachung und benutzerdefiniertes Routing ausführen kann. Weitere Informationen finden Sie unter [Implementieren einer DMZ zwischen Azure und dem Internet][dmz].

Verschlüsseln Sie sensible ruhende Daten, und verwalten Sie die Datenbankverschlüsselungsschlüssel mithilfe von [Azure Key Vault][azure-key-vault]. Key Vault kann Verschlüsselungsschlüssel in Hardwaresicherheitsmodulen (HSMs) speichern. Weitere Informationen finden Sie unter [Konfigurieren der Azure Key Vault-Integration für SQL Server auf virtuellen Azure-Computern (Resource Manager)][sql-keyvault]. Es empfiehlt sich außerdem, Anwendungsgeheimnisse wie Datenbankverbindungszeichenfolgen in Key Vault zu speichern.

## <a name="deploy-the-solution"></a>Bereitstellen der Lösung

Eine Bereitstellung für diese Referenzarchitektur ist auf [GitHub][github-folder] verfügbar. 

### <a name="prerequisites"></a>Voraussetzungen

1. Klonen oder Forken Sie das GitHub-Repository [Referenzarchitekturen][ref-arch-repo], oder laden Sie die entsprechende ZIP-Datei herunter.

2. Vergewissern Sie sich, dass Azure CLI 2.0 auf Ihrem Computer installiert ist. Um die Befehlszeilenschnittstelle zu installieren, befolgen Sie die Anweisungen unter [Installieren von Azure CLI 2.0][azure-cli-2].

3. Installieren Sie das npm-Paket mit den [Azure Bausteinen][azbb].

   ```bash
   npm install -g @mspnp/azure-building-blocks
   ```

4. Melden Sie sich über eine Eingabeaufforderung, eine bash-Eingabeaufforderung oder die PowerShell-Eingabeaufforderung bei Ihrem Azure-Konto an. Verwenden Sie dazu die unten aufgeführten Befehle, und befolgen Sie die Anweisungen.

   ```bash
   az login
   ```

### <a name="deploy-the-solution-using-azbb"></a>Bereitstellen der Lösung mit azbb

Um die Windows-VMs für eine n-schichtige Anwendungsreferenzarchitektur bereitzustellen, führen Sie die folgenden Schritte aus:

1. Navigieren Sie zum `virtual-machines\n-tier-windows`-Ordner für das Repository, das Sie oben in Schritt 1 der Voraussetzungen als Klon erstellt haben.

2. Die Parameterdatei gibt einen Standard-Administratorbenutzernamen und ein Standardkennwort für jede VM in der Bereitstellung an. Sie müssen diese vor der Bereitstellung der Referenzarchitektur ändern. Öffnen Sie die `n-tier-windows.json`-Datei, und ersetzen Sie jedes Feld **adminUsername** und **adminPassword** durch neue Einstellungen.
  
   > [!NOTE]
   > Während dieser Bereitstellung werden sowohl in den **VirtualMachineExtension**-Objekten als auch in den **extensions**-Einstellungen für einige der **VirtualMachine**-Objekte mehrere Skripts ausgeführt. Für einige dieser Skripts sind der Administratorbenutzername und das Kennwort erforderlich, die Sie soeben geändert haben. Wir empfehlen Ihnen, diese Skripts zu überprüfen, um sicherzustellen, dass Sie die richtigen Anmeldeinformationen angegeben haben. Wenn Sie nicht die richtigen Anmeldeinformationen angegeben haben, tritt bei der Bereitstellung möglicherweise ein Fehler auf.
   > 
   > 

Speichern Sie die Datei .

3. Stellen Sie die Referenzarchitektur mithilfe des Befehlszeilentools **azbb** bereit, wie unten dargestellt.

   ```bash
   azbb -s <your subscription_id> -g <your resource_group_name> -l <azure region> -p n-tier-windows.json --deploy
   ```

Weitere Informationen zum Bereitstellen dieser Beispielreferenzarchitektur mithilfe von Azure-Bausteinen finden Sie im [GitHub-Repository][git].


<!-- links -->
[dmz]: ../dmz/secure-vnet-dmz.md
[multi-dc]: multi-region-sql-server.md
[n-tier]: n-tier.md
[azbb]: https://github.com/mspnp/template-building-blocks/wiki/Install-Azure-Building-Blocks
[azure-administration]: /azure/automation/automation-intro
[azure-availability-sets]: /azure/virtual-machines/virtual-machines-windows-manage-availability#configure-each-application-tier-into-separate-availability-sets
[azure-cli]: /azure/virtual-machines-command-line-tools
[azure-cli-2]: https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest
[azure-dns]: /azure/dns/dns-overview
[azure-key-vault]: https://azure.microsoft.com/services/key-vault
[geschützter Host]: https://en.wikipedia.org/wiki/Bastion_host
[CIDR]: https://en.wikipedia.org/wiki/Classless_Inter-Domain_Routing
[chef]: https://www.chef.io/solutions/azure/
[git]: https://github.com/mspnp/template-building-blocks
[github-folder]: https://github.com/mspnp/reference-architectures/tree/master/virtual-machines/n-tier-windows
[lb-external-create]: /azure/load-balancer/load-balancer-get-started-internet-portal
[lb-internal-create]: /azure/load-balancer/load-balancer-get-started-ilb-arm-portal
[load-balancer-external]: /azure/load-balancer/load-balancer-internet-overview
[load-balancer-internal]: /azure/load-balancer/load-balancer-internal-overview
[nsg]: /azure/virtual-network/virtual-networks-nsg
[operations-management-suite]: https://www.microsoft.com/server-cloud/operations-management-suite/overview.aspx
[plan-network]: /azure/virtual-network/virtual-network-vnet-plan-design-arm
[private-ip-space]: https://en.wikipedia.org/wiki/Private_network#Private_IPv4_address_spaces
[öffentliche IP-Adresse]: /azure/virtual-network/virtual-network-ip-addresses-overview-arm
[puppet]: https://puppetlabs.com/blog/managing-azure-virtual-machines-puppet
[ref-arch-repo]: https://github.com/mspnp/reference-architectures
[sql-alwayson]: https://msdn.microsoft.com/library/hh510230.aspx
[sql-alwayson-force-failover]: https://msdn.microsoft.com/library/ff877957.aspx
[sql-alwayson-getting-started]: https://msdn.microsoft.com/library/gg509118.aspx
[sql-alwayson-ilb]: /azure/virtual-machines/windows/sql/virtual-machines-windows-portal-sql-alwayson-int-listener
[sql-alwayson-listeners]: https://msdn.microsoft.com/library/hh213417.aspx
[sql-alwayson-read-only-routing]: https://technet.microsoft.com/library/hh213417.aspx#ConnectToSecondary
[sql-keyvault]: /azure/virtual-machines/virtual-machines-windows-ps-sql-keyvault
[vm-sla]: https://azure.microsoft.com/support/legal/sla/virtual-machines
[vnet faq]: /azure/virtual-network/virtual-networks-faq
[wsfc-whats-new]: https://technet.microsoft.com/windows-server-docs/failover-clustering/whats-new-in-failover-clustering
[Nagios]: https://www.nagios.org/
[Zabbix]: http://www.zabbix.com/
[Icinga]: http://www.icinga.org/
[visio-download]: https://archcenter.blob.core.windows.net/cdn/vm-reference-architectures.vsdx
[0]: ./images/n-tier-sql-server.png "n-schichtige Architektur mit Microsoft Azure"
[resource-manager-overview]: /azure/azure-resource-manager/resource-group-overview 
[vmss]: /azure/virtual-machine-scale-sets/virtual-machine-scale-sets-overview
[load-balancer]: /azure/load-balancer/load-balancer-get-started-internet-arm-cli
[load-balancer-hashing]: /azure/load-balancer/load-balancer-overview#load-balancer-features
[vmss-design]: /azure/virtual-machine-scale-sets/virtual-machine-scale-sets-design-overview
[subscription-limits]: /azure/azure-subscription-service-limits
[availability-set]: /azure/virtual-machines/virtual-machines-windows-manage-availability
[health-probes]: /azure/load-balancer/load-balancer-overview#load-balancer-features
[health-probe-log]: /azure/load-balancer/load-balancer-monitor-log
[health-probe-ip]: /azure/virtual-network/virtual-networks-nsg#special-rules
[network-security]: /azure/best-practices-network-security
