---
title: N-schichtige Windows-Anwendung mit SQL Server
titleSuffix: Azure Reference Architectures
description: Implementieren Sie eine mehrschichtige Architektur in Azure für Verfügbarkeit, Sicherheit, Skalierbarkeit und Verwaltbarkeit.
author: MikeWasson
ms.date: 11/12/2018
ms.topic: reference-architecture
ms.service: architecture-center
ms.subservice: reference-architecture
ms.openlocfilehash: cf12d27ccaebb9845ada4d4a437e9889ea3325f2
ms.sourcegitcommit: 1b50810208354577b00e89e5c031b774b02736e2
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/23/2019
ms.locfileid: "54485399"
---
# <a name="windows-n-tier-application-on-azure-with-sql-server"></a>n-schichtige Windows-Anwendung in Azure mit SQL Server

Diese Referenzarchitektur zeigt, wie Sie für eine [n-schichtige](../../guide/architecture-styles/n-tier.md) Anwendung konfigurierte virtuelle Computer und ein entsprechendes virtuelles Netzwerk mit SQL Server unter Windows für die Datenschicht bereitstellen. [**Stellen Sie diese Lösung bereit**](#deploy-the-solution).

![N-Ebenen-Architektur mit Microsoft Azure](./images/n-tier-sql-server.png)

*Laden Sie eine [Visio-Datei][visio-download] mit dieser Architektur herunter.*

## <a name="architecture"></a>Architecture

Die Architektur besteht aus den folgenden Komponenten:

- **Ressourcengruppe**. [Ressourcengruppen][resource-manager-overview] dienen zum Gruppieren von Ressourcen, damit sie nach Lebensdauer, Besitzer oder anderen Kriterien verwaltet werden können.

- **Virtuelles Netzwerk (VNET) und Subnetze:** Jede Azure-VM wird in einem virtuellen Netzwerk (VNET) bereitgestellt, das in Subnetze segmentiert werden kann. Erstellen Sie für jede Schicht ein separates Subnetz.

- **Anwendungsgateway:** [Azure Application Gateway](/azure/application-gateway/) ist ein Load Balancer auf Schicht 7 (Anwendungsschicht). Bei dieser Architektur werden HTTP-Anforderungen an das Web-Front-End geleitet. Mit Application Gateway wird auch eine [Web Application Firewall](/azure/application-gateway/waf-overview) (WAF) bereitgestellt, mit der die Anwendung vor häufig auftretenden Exploits und Sicherheitsrisiken geschützt wird.

- **NSGs**. Verwenden Sie [Netzwerksicherheitsgruppen][nsg] (NSGs), um den Netzwerkdatenverkehr im VNET zu beschränken. In der hier gezeigten dreischichtigen Architektur akzeptiert die Datenbankschicht beispielsweise keinen Datenverkehr vom Web-Front-End, sondern nur von der Unternehmensschicht und dem Verwaltungssubnetz.

- **Azure DDoS Protection**. Obwohl die Azure-Plattform Schutz vor verteilten Denial-of-Service-Angriffen (DDoS) beinhaltet, empfehlen wir die Verwendung der Dienstebene [DDoS Protection Standard][ddos], die erweiterte Funktionen für die DDoS-Entschärfung bietet. Weitere Informationen finden Sie unter [Sicherheitshinweise](#security-considerations).

- **Virtuelle Computer:** Empfehlungen zum Konfigurieren von virtuellen Computern finden Sie unter [Run a Windows VM on Azure](./windows-vm.md) (Ausführen eines virtuellen Windows-Computers in Azure) sowie unter [Run a Linux VM on Azure](./linux-vm.md) (Ausführen eines virtuellen Linux-Computers in Azure).

- **Verfügbarkeitsgruppen**: Erstellen Sie eine [Verfügbarkeitsgruppe][azure-availability-sets] für jede Schicht, und stellen Sie mindestens zwei VMs in jeder Schicht bereit. Dadurch ist die VM für eine höhere [Vereinbarung zum Servicelevel (SLA)][vm-sla] qualifiziert.

- **Lastenausgleichsmodule**. Verwenden Sie [Azure Load Balancer][load-balancer] zum Verteilen von Netzwerkdatenverkehr von der Webebene auf die Geschäftsebene und von der Geschäftsebene an SQL Server.

- **Öffentliche IP-Adresse:** Es ist eine öffentliche IP-Adresse erforderlich, damit die Anwendung Internetdatenverkehr empfangen kann.

- **Jumpbox**: Wird auch als [geschützter Host] bezeichnet. Dies ist eine geschützte VM im Netzwerk, die von Administratoren zum Herstellen der Verbindung mit anderen VMs verwendet wird. Die Jumpbox verfügt über eine NSG, bei der Remotedatenverkehr nur von öffentlichen IP-Adressen zugelassen wird, die in einer Liste mit sicheren Adressen enthalten sind. Die NSG sollte Remotedesktop-Datenverkehr (RDP) zulassen.

- **SQL Server Always On-Verfügbarkeitsgruppe**. Stellt Hochverfügbarkeit in der Datenschicht durch Replikation und Failover bereit. Verwendet WSFC-Technologie (Windows Server-Failovercluster) für Failover.

- **AD DS-Server (Active Directory Domain Services):** Die Computerobjekte für den Failovercluster und die zugehörigen Clusterrollen werden in Active Directory Domain Services (AD DS) erstellt.

- **Cloudzeuge**. Bei einem Failovercluster muss mindestens die Hälfte der Knoten ausgeführt werden (Quorum). Enthält der Cluster nur zwei Knoten, kann eine Netzwerkpartition dazu führen, dass sich beide Knoten als Masterknoten betrachten. In diesem Fall benötigen Sie einen *Zeugen*, um den Konflikt zu lösen und ein Quorum festzulegen. Ein Zeuge ist eine Ressource (beispielsweise ein freigegebener Datenträger), der den Ausschlag für das Quorum gibt. Ein Cloudzeuge ist ein Zeugentyp, der Azure Blob Storage nutzt. Weitere Informationen zum Konzept des Quorums finden Sie unter [Understanding cluster and pool quorum](/windows-server/storage/storage-spaces/understand-quorum) (Grundlegendes zu Cluster- und Poolquoren). Weitere Informationen zu Cloudzeugen finden Sie unter [Deploy a cloud witness for a Failover Cluster](/windows-server/failover-clustering/deploy-cloud-witness) (Bereitstellen eines Cloudzeugen für einen Failovercluster).

- **Azure DNS:** [Azure DNS][azure-dns] ist ein Hostingdienst für DNS-Domänen. Er ermöglicht eine Namensauflösung mithilfe der Microsoft Azure-Infrastruktur. Durch das Hosten Ihrer Domänen in Azure können Sie Ihre DNS-Einträge mithilfe der gleichen Anmeldeinformationen, APIs, Tools und Abrechnung wie für die anderen Azure-Dienste verwalten.

## <a name="recommendations"></a>Empfehlungen

Ihre Anforderungen können von der hier beschriebenen Architektur abweichen. Verwenden Sie diese Empfehlungen als Startpunkt.

### <a name="vnet--subnets"></a>VNET/Subnetze

Legen Sie bei der Erstellung des VNET fest, wie viele IP-Adressen Ihre Ressourcen in jedem Subnetz benötigen. Geben Sie mithilfe der [CIDR] eine Subnetzmaske und einen VNET-Adressbereich an, der für die erforderlichen IP-Adressen groß genug ist. Verwenden Sie einen Adressraum, der in die standardmäßigen [privaten IP-Adressblöcke][private-ip-space] 10.0.0.0/8, 172.16.0.0/12 und 192.168.0.0/16 fällt.

Wählen Sie einen Adressbereich, der sich nicht mit Ihrem lokalen Netzwerk überschneidet, für den Fall, dass Sie später ein Gateway zwischen dem VNET und dem lokalen Netzwerk einrichten müssen. Sobald Sie das VNET erstellt haben, können Sie den Adressbereich nicht mehr ändern.

Entwerfen Sie Subnetze unter Berücksichtigung der Funktionalität und Sicherheitsanforderungen. Alle VMs innerhalb derselben Schicht oder Rolle sollten im selben Subnetz platziert werden, was eine Sicherheitsbegrenzung darstellen kann. Weitere Informationen zum Entwerfen von VNETs und Subnetzen finden Sie unter [Planen und Entwerfen von Azure Virtual Networks][plan-network].

### <a name="load-balancers"></a>Load Balancer

Machen Sie die VMs nicht direkt über das Internet verfügbar. Weisen Sie stattdessen jeder VM eine private IP-Adresse zu. Clients stellen über die öffentliche IP-Adresse eine Verbindung her, die dem Application Gateway zugeordnet ist.

Definieren Sie Lastenausgleichsregeln, um Netzwerkdatenverkehr an die virtuellen Computer weiterzuleiten. Um HTTP-Datenverkehr zuzulassen, ordnen Sie beispielsweise den Port 80 in der Front-End-Konfiguration dem Port 80 im Back-End-Adresspool zu. Wenn ein Client eine HTTP-Anforderung an Port 80 sendet, wählt der Lastenausgleich mithilfe eines [Hashalgorithmus][load-balancer-hashing], der die Quell-IP-Adresse enthält, eine Back-End-IP-Adresse aus. So werden Clientanforderungen auf alle VMs im Back-End-Adresspool verteilt.

### <a name="network-security-groups"></a>Netzwerksicherheitsgruppen

Verwenden Sie NSG-Regeln, um den Datenverkehr zwischen den Schichten zu beschränken. In der oben gezeigten dreischichtigen Architektur kommuniziert die Internetschicht nicht direkt mit der Datenbankschicht. Um dies zu erzwingen, sollte die Datenbankschicht eingehenden Datenverkehr aus dem Subnetz der Internetschicht blockieren.

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

Wenn ein SQL-Client versucht, eine Verbindung herzustellen, leitet das Lastenausgleichsmodul die Verbindungsanforderung an das primäre Replikat weiter. Bei einem Failover zu einem anderen Replikat leitet der Lastenausgleich neue Anforderungen automatisch an ein neues primäres Replikat weiter. Weitere Informationen finden Sie unter [Konfigurieren eines ILB-Listeners für AlwaysOn-Verfügbarkeitsgruppen in Azure][sql-alwayson-ilb].

Bei einem Failover werden bestehende Clientverbindungen geschlossen. Nachdem das Failover abgeschlossen ist, werden neue Verbindungen an das neue primäre Replikat weitergeleitet.

Wenn Ihre Anwendung deutlich mehr Lesezugriffe als Schreibzugriffe tätigt, können Sie einige der schreibgeschützten Abfragen in ein sekundäres Replikat auslagern. Weitere Informationen finden Sie unter [Verwenden eines Listeners zum Herstellen einer Verbindung mit einem schreibgeschützten sekundären Replikat (schreibgeschütztes Routing)][sql-alwayson-read-only-routing].

Testen Sie Ihre Bereitstellung, indem Sie [ein manuelles Failover der Verfügbarkeitsgruppe erzwingen][sql-alwayson-force-failover].

### <a name="jumpbox"></a>Jumpbox

Verweigern Sie den RDP-Zugriff (Remotedesktopprotokoll) über das öffentliche Internet auf die VMs, auf denen die Anwendungsworkload ausgeführt wird. Der gesamte RDP-Zugriff auf diese VMs muss stattdessen über die Jumpbox erfolgen. Ein Administrator meldet sich bei der Jumpbox und von der Jumpbox aus dann bei der anderen VM an. Die Jumpbox lässt RDP-Datenverkehr aus dem Internet zu, jedoch nur von bekannten, sicheren IP-Adressen.

Die Jumpbox hat sehr geringe Leistungsanforderungen. Wählen Sie daher einen virtuellen Computer mit geringer Größe. Erstellen Sie eine [öffentliche IP-Adresse] für die Jumpbox. Platzieren Sie die Jumpbox in dasselbe VNET wie die anderen VMs, jedoch in ein separates Verwaltungssubnetz.

Fügen Sie zum Schutz der Jumpbox eine NSG-Regel hinzu, die ausschließlich RDP-Verbindungen von einer sicheren Gruppe öffentlicher IP-Adressen zulässt. Konfigurieren Sie die NSGs für die anderen Subnetze, um RDP-Datenverkehr aus dem Verwaltungssubnetz zuzulassen.

## <a name="scalability-considerations"></a>Überlegungen zur Skalierbarkeit

Erwägen Sie für die Internet- und Unternehmensschichten die Verwendung von [VM-Skalierungsgruppen][vmss], anstatt separate VMs in einer Verfügbarkeitsgruppe bereitzustellen. Eine Skalierungsgruppe erleichtert die Bereitstellung und Verwaltung einer Gruppe identischer VMs und ermöglicht die automatische Skalierung der VMs auf der Grundlage von Leistungsmetriken. Mit zunehmender Last auf den virtuellen Computern werden dem Lastenausgleich automatisch zusätzliche virtuelle Computer hinzugefügt. Erwägen Sie die Verwendung von Skalierungsgruppen, wenn Sie VMs schnell horizontal hochskalieren müssen oder eine automatische Skalierung benötigen.

Es gibt zwei grundlegende Methoden für das Konfigurieren von virtuellen Computern in einer Skalierungsgruppe:

- Verwenden Sie Erweiterungen, um die VM nach ihrer Bereitstellung zu konfigurieren. Bei diesem Ansatz dauert das Starten neuer VM-Instanzen länger als bei virtuellen Computern ohne Erweiterungen.

- Stellen Sie einen [verwalteten Datenträger](/azure/storage/storage-managed-disks-overview) mit einem benutzerdefinierten Datenträgerimage bereit. Diese Option kann möglicherweise schneller bereitgestellt werden. Allerdings müssen Sie das Image auf dem neuesten Stand halten.

Weitere Informationen finden Sie unter [Überlegungen zum Entwurf von Skalierungsgruppen][vmss-design].

> [!TIP]
> Wenn Sie eine Lösung für die automatische Skalierung verwenden, testen Sie diese im Voraus mit Workloads auf Produktionsebene.

Jedes Azure-Abonnement verfügt über Standardeinschränkungen, zu denen auch eine maximale Anzahl von virtuellen Computern pro Region gehört. Sie können den Grenzwert erhöhen, indem Sie eine Supportanfrage einreichen. Weitere Informationen finden Sie unter [Grenzwerte für Azure-Abonnements, -Dienste und -Kontingente sowie allgemeine Beschränkungen][subscription-limits].

## <a name="availability-considerations"></a>Überlegungen zur Verfügbarkeit

Wenn Sie keine VM-Skalierungsgruppen verwenden, platzieren Sie VMs für die gleiche Schicht in einer Verfügbarkeitsgruppe. Erstellen Sie zur Unterstützung der [Verfügbarkeits-SLA für Azure-VMs][vm-sla] mindestens zwei VMs in der Verfügbarkeitsgruppe. Weitere Informationen finden Sie unter [Verwalten der Verfügbarkeit virtueller Computer][availability-set]. Skalierungsgruppen verwenden automatisch *Platzierungsgruppen*, die als eine implizite Verfügbarkeitsgruppe fungieren.

Der Lastenausgleich verwendet [Integritätstests][health-probes] zur Überwachung der Verfügbarkeit von VM-Instanzen. Kann eine Instanz bei einem Test nicht innerhalb des jeweiligen Zeitlimits erreicht werden, beendet der Lastenausgleich das Senden von Datenverkehr an diese VM. Der Lastenausgleich führt aber weiterhin Tests durch, und wenn der virtuelle Computer wieder erreichbar ist, sendet der Lastenausgleich auch wieder Datenverkehr an diese VM.

Empfehlungen für Integritätstests durch den Lastenausgleich:

- Die Tests können für HTTP oder TCP durchgeführt werden. Wenn auf Ihren virtuellen Computern ein HTTP-Server ausgeführt wird, erstellen Sie einen HTTP-Test. Erstellen Sie andernfalls einen TCP-Test.
- Geben Sie für einen HTTP-Test den Pfad zu einem HTTP-Endpunkt an. Der Test überprüft, ob von diesem Pfad eine HTTP-200-Antwort empfangen wird. Bei diesem Pfad kann es sich um den Stammpfad („/“) oder einen Endpunkt zur Integritätsüberwachung handeln, der benutzerdefinierte Logik zum Überprüfen der Integrität der Anwendung implementiert. Der Endpunkt muss anonyme HTTP-Anforderungen zulassen.
- Der Test wird von einer [bekannten][health-probe-ip] IP-Adresse (168.63.129.16) gesendet. Blockieren Sie den an oder von diese(r) IP-Adresse gesendeten Datenverkehr nicht in Firewallrichtlinien oder NSG-Regeln.
- Verwenden Sie die [Protokolle der Integritätstests][health-probe-log], um den Status der Integritätstests anzuzeigen. Aktivieren Sie die Protokollierung für jeden Lastenausgleich im Azure-Portal. Die Protokolle werden in Azure Blob Storage geschrieben. Die Protokolle zeigen, wie viele VMs aufgrund von fehlerhaften Testantworten keinen Netzwerkdatenverkehr empfangen.

Wenn Sie eine höhere Verfügbarkeit benötigen, als die [Azure-SLA für VMs][vm-sla] bietet, replizieren Sie ggf. die Anwendung über zwei Regionen, und verwenden Sie den Azure Traffic Manager für das Failover. Weitere Informationen finden Sie unter [Multi-region N-tier application for high availability][multi-dc] (N-schichtige Anwendung in mehreren Regionen für Hochverfügbarkeit).

## <a name="security-considerations"></a>Sicherheitshinweise

Virtuelle Netzwerke stellen in Azure eine Isolationsbegrenzung für Datenverkehr dar. VMs in einem VNET können nicht direkt mit VMs in einem anderen VNET kommunizieren. VMs im selben VNET können miteinander kommunizieren, sofern Sie den Datenverkehr nicht durch die Erstellung von [Netzwerksicherheitsgruppen][nsg] (NSGs) beschränken. Weitere Informationen finden Sie unter [Microsoft Cloud Services und Netzwerksicherheit][network-security].

**DMZ**. Für die Erstellung einer DMZ zwischen dem Internet und dem virtuellen Azure-Netzwerk sollten Sie eventuell eine virtuelle Netzwerkappliance (Network Virtual Appliance, NVA) hinzufügen. NVA ist ein Oberbegriff für eine virtuelle Appliance, die netzwerkbezogene Aufgaben wie Erstellung von Firewalls, Paketüberprüfung, Überwachung und benutzerdefiniertes Routing ausführen kann. Weitere Informationen finden Sie unter [Implementieren einer DMZ zwischen Azure und dem Internet][dmz].

**Verschlüsselung**. Verschlüsseln Sie sensible ruhende Daten, und verwalten Sie die Datenbankverschlüsselungsschlüssel mithilfe von [Azure Key Vault][azure-key-vault]. Key Vault kann Verschlüsselungsschlüssel in Hardwaresicherheitsmodulen (HSMs) speichern. Weitere Informationen finden Sie unter [Konfigurieren der Azure Key Vault-Integration für SQL Server auf virtuellen Azure-Computern (Resource Manager)][sql-keyvault]. Es empfiehlt sich außerdem, Anwendungsgeheimnisse wie Datenbankverbindungszeichenfolgen in Key Vault zu speichern.

**DDoS Protection**. Die Azure-Plattform stellt DDoS Protection Basic-Schutz standardmäßig bereit. Dieser Basisschutz ist dafür ausgelegt, die Azure-Infrastruktur als Ganzes zu schützen. Obwohl der DDoS Protection Basic-Schutz automatisch aktiviert ist, empfehlen wir die Verwendung von [DDoS Protection Standard][ddos]. DDoS Protection Standard wendet basierend auf den Mustern des Netzwerkdatenverkehrs Ihrer Anwendung eine adaptive Optimierung an, um Bedrohungen zu erkennen. Dadurch können DDoS-Angriffe entschärft werden, die von den infrastrukturweiten DDoS-Richtlinien möglicherweise nicht erkannt werden. DDoS Protection Standard stellt über Azure Monitor auch Warnungen, Telemetrie und Analysen bereit. Weitere Informationen finden Sie unter [Azure DDoS Protection – empfohlene Methoden und Referenzarchitekturen][ddos-best-practices].

## <a name="deploy-the-solution"></a>Bereitstellen der Lösung

Eine Bereitstellung für diese Referenzarchitektur ist auf [GitHub][github-folder] verfügbar. Die gesamte Bereitstellung, die die Ausführung der Skripts zum Konfigurieren von Active Directory Domain Services (AD DS), des Windows Server-Failoverclusters und der SQL Server-Verfügbarkeitsgruppe umfasst, kann bis zu zwei Stunden dauern.

### <a name="prerequisites"></a>Voraussetzungen

[!INCLUDE [ref-arch-prerequisites.md](../../../includes/ref-arch-prerequisites.md)]

### <a name="deployment-steps"></a>Bereitstellungsschritte

1. Führen Sie den folgenden Befehl aus, um eine Ressourcengruppe zu erstellen:

    ```azurecli
    az group create --location <location> --name <resource-group-name>
    ```

2. Führen Sie den folgenden Befehl aus, um ein Speicherkonto für den Cloudzeugen zu erstellen:

    ```azurecli
    az storage account create --location <location> \
      --name <storage-account-name> \
      --resource-group <resource-group-name> \
      --sku Standard_LRS
    ```

3. Navigieren Sie zum Ordner `virtual-machines\n-tier-windows` des GitHub-Repositorys für Referenzarchitekturen.

4. Öffnen Sie die Datei `n-tier-windows.json` .

5. Suchen Sie nach allen Instanzen von „witnessStorageBlobEndPoint“, und ersetzen Sie den Platzhaltertext durch den Namen des Speicherkontos aus Schritt 2.

    ```json
    "witnessStorageBlobEndPoint": "https://[replace-with-storageaccountname].blob.core.windows.net",
    ```

6. Führen Sie den folgenden Befehl aus, um die Kontoschlüssel für das Speicherkonto aufzulisten:

    ```azurecli
    az storage account keys list \
      --account-name <storage-account-name> \
      --resource-group <resource-group-name>
    ```

    Die Ausgabe sollte wie im folgenden Beispiel aussehen. Kopieren Sie den Wert von `key1`.

    ```json
    [
    {
        "keyName": "key1",
        "permissions": "Full",
        "value": "..."
    },
    {
        "keyName": "key2",
        "permissions": "Full",
        "value": "..."
    }
    ]
    ```

7. Suchen Sie in der Datei `n-tier-windows.json` nach allen Instanzen von „witnessStorageAccountKey“, und fügen Sie den Kontoschlüssel ein.

    ```json
    "witnessStorageAccountKey": "[replace-with-storagekey]"
    ```

8. Suchen Sie in der Datei `n-tier-windows.json` nach allen Vorkommen von `[replace-with-password]` und `[replace-with-sql-password]`, und ersetzen Sie sie durch ein sicheres Kennwort. Speichern Sie die Datei .

    > [!NOTE]
    > Wenn Sie den Administratorbenutzernamen ändern, müssen Sie auch die `extensions`-Blöcke in der JSON-Datei aktualisieren.

9. Führen Sie den folgenden Befehl aus, um die Architektur bereitzustellen:

    ```azurecli
    azbb -s <your subscription_id> -g <resource_group_name> -l <location> -p n-tier-windows.json --deploy
    ```

Weitere Informationen zum Bereitstellen dieser Beispielreferenzarchitektur mithilfe von Azure-Bausteinen finden Sie im [GitHub-Repository][git].

## <a name="next-steps"></a>Nächste Schritte

- [Microsoft Learn-Modul: Übersicht über die n-schichtige Architektur](/learn/modules/n-tier-architecture/)

<!-- links -->
[dmz]: ../dmz/secure-vnet-dmz.md
[multi-dc]: multi-region-sql-server.md
[n-tier]: n-tier.md
[azure-availability-sets]: /azure/virtual-machines/virtual-machines-windows-manage-availability#configure-each-application-tier-into-separate-availability-sets
[azure-dns]: /azure/dns/dns-overview
[azure-key-vault]: https://azure.microsoft.com/services/key-vault
[geschützter Host]: https://en.wikipedia.org/wiki/Bastion_host
[CIDR]: https://en.wikipedia.org/wiki/Classless_Inter-Domain_Routing
[ddos]: /azure/virtual-network/ddos-protection-overview
[ddos-best-practices]: /azure/security/azure-ddos-best-practices
[git]: https://github.com/mspnp/template-building-blocks
[github-folder]: https://github.com/mspnp/reference-architectures/tree/master/virtual-machines/n-tier-windows
[nsg]: /azure/virtual-network/virtual-networks-nsg
[plan-network]: /azure/virtual-network/virtual-network-vnet-plan-design-arm
[private-ip-space]: https://en.wikipedia.org/wiki/Private_network#Private_IPv4_address_spaces
[öffentliche IP-Adresse]: /azure/virtual-network/virtual-network-ip-addresses-overview-arm
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
[visio-download]: https://archcenter.blob.core.windows.net/cdn/vm-reference-architectures.vsdx
[resource-manager-overview]: /azure/azure-resource-manager/resource-group-overview
[vmss]: /azure/virtual-machine-scale-sets/virtual-machine-scale-sets-overview
[load-balancer]: /azure/load-balancer/
[load-balancer-hashing]: /azure/load-balancer/load-balancer-overview#load-balancer-features
[vmss-design]: /azure/virtual-machine-scale-sets/virtual-machine-scale-sets-design-overview
[subscription-limits]: /azure/azure-subscription-service-limits
[availability-set]: /azure/virtual-machines/virtual-machines-windows-manage-availability
[health-probes]: /azure/load-balancer/load-balancer-overview#load-balancer-features
[health-probe-log]: /azure/load-balancer/load-balancer-monitor-log
[health-probe-ip]: /azure/virtual-network/virtual-networks-nsg#special-rules
[network-security]: /azure/best-practices-network-security
