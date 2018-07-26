---
title: Implementieren einer Hub-Spoke-Netzwerktopologie mit gemeinsamen Diensten in Azure
description: Es wird beschrieben, wie Sie eine Hub-Spoke-Netzwerktopologie mit gemeinsamen Diensten in Azure implementieren.
author: telmosampaio
ms.date: 06/19/2018
pnp.series.title: Implement a hub-spoke network topology with shared services in Azure
pnp.series.prev: hub-spoke
ms.openlocfilehash: 283251d5b11f76985405410c5c237e5a64ee98fe
ms.sourcegitcommit: 71cbef121c40ef36e2d6e3a088cb85c4260599b9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/14/2018
ms.locfileid: "39060794"
---
# <a name="implement-a-hub-spoke-network-topology-with-shared-services-in-azure"></a>Implementieren einer Hub-Spoke-Netzwerktopologie mit gemeinsamen Diensten in Azure

Diese Referenzarchitektur baut auf der [Hub-Spoke][guidance-hub-spoke]-Referenzarchitektur auf, um gemeinsame Dienste in den Hub einzubinden, die von allen Spokes genutzt werden können. Als ersten Schritt zur Migration eines Rechenzentrums zur Cloud und Erstellung eines [virtuellen Rechenzentrums] müssen Sie zunächst die Dienste für Identität und Sicherheit freigeben. Anhand dieser Referenzarchitektur wird veranschaulicht, wie Sie Ihre Active Directory-Dienste aus Ihrem lokalen Rechenzentrum auf Azure ausdehnen und ein virtuelles Netzwerkgerät (Network Virtual Appliance, NVA) hinzufügen, das in einer Hub-Spoke-Topologie als Firewall fungieren kann.  [**Stellen Sie diese Lösung bereit**](#deploy-the-solution).

![[0]][0]

*Laden Sie eine [Visio-Datei][visio-download] mit dieser Architektur herunter.*

Diese Topologie bietet unter anderem folgende Vorteile:

* **Kosteneinsparungen** durch die Zentralisierung von Diensten, die von mehreren Workloads (z.B. Network Virtual Appliances, kurz NVAs, und DNS-Servern) an einem einzigen Standort gemeinsam genutzt werden können
* **Umgehung von Abonnementbeschränkungen** durch die Herstellung von Peeringverbindungen zwischen VNETs verschiedener Abonnements und dem zentralen Hub
* **Trennung der Belange** zwischen zentralen IT-Vorgängen (SecOps, InfraOps) und Workloads (DevOps)

Typische Einsatzmöglichkeiten für diese Architektur sind Folgende:

* Workloads, die in verschiedenen Umgebungen wie Entwicklungs-, Test- und Produktionsumgebungen eingesetzt werden und gemeinsame Dienste wie DNS, IDS, NTP oder AD DS erfordern Gemeinsame Dienste werden im Hub-VNET platziert, während die einzelnen Umgebungen in einem Spoke bereitgestellt werden, um die Isolation beizubehalten.
* Workloads, bei denen keine Konnektivität untereinander bestehen muss, die jedoch Zugriff auf gemeinsame Dienste erfordern
* Unternehmen, die auf eine zentrale Steuerung von Sicherheitsmechanismen (z.B. eine Firewall im Hub als DMZ) und eine getrennte Verwaltung von Workloads in den einzelnen Spokes angewiesen sind

## <a name="architecture"></a>Architektur

Die Architektur umfasst die folgenden Komponenten.

* **Lokales Netzwerk**. Ein in einer Organisation betriebenes privates lokales Netzwerk.

* **VPN-Gerät**: Ein Gerät oder ein Dienst, der externe Konnektivität mit dem lokalen Netzwerk bereitstellt. Bei dem VPN-Gerät kann es sich um ein Hardwaregerät oder eine Softwarelösung wie den Routing- und RAS-Dienst (RRAS) unter Windows Server 2012 handeln. Eine Liste der unterstützten VPN-Appliances und Informationen zur Konfiguration ausgewählter VPN-Geräte für die Verbindung mit Azure finden Sie unter [Informationen zu VPN-Geräten und IPsec-/IKE-Parametern für VPN-Gatewayverbindungen zwischen Standorten][vpn-appliance].

* **VPN-Gateway für ein virtuelles Netzwerk oder ExpressRoute-Gateway**: Über das Gateway für virtuelle Netzwerke kann das VNET zur Konnektivität mit Ihrem lokalen Netzwerk eine Verbindung mit dem VPN-Gerät oder eine ExpressRoute-Verbindung herstellen. Weitere Informationen finden Sie unter [Verbinden eines lokalen Netzwerks mit einem virtuellen Microsoft Azure-Netzwerk][connect-to-an-Azure-vnet].

> [!NOTE]
> In den Bereitstellungsskripts für diese Referenzarchitektur werden ein VPN-Gateway für die Konnektivität und ein VNET in Azure verwendet, um Ihr lokales Netzwerk zu simulieren.

* **Hub-VNET**: Das Azure-VNET, das als Hub in der Hub-Spoke-Topologie verwendet wird. Der Hub ist der zentrale Konnektivitätspunkt für Ihr lokales Netzwerk, mit dem Sie Dienste hosten, die von den verschiedenen in den Spoke-VNETs gehosteten Workloads genutzt werden können.

* **Gatewaysubnetz**: Die Gateways für die virtuellen Netzwerke befinden sich im selben Subnetz.

* **Subnetz für gemeinsame Dienste**: Ein Subnetz im Hub-VNET, das für das Hosten von Diensten verwendet wird, die von allen Spokes gemeinsam genutzt werden können (z.B. DNS oder AD DS).

* **DMZ-Subnetz**: Ein Subnetz im Hub-VNET zum Hosten von NVAs, die als Sicherheitseinrichtungen, z.B. Firewalls, dienen können.

* **Spoke-VNETs**: Ein oder mehrere Azure-VNETs, die als Spokes in der Hub-Spoke-Topologie verwendet werden. Spokes können verwendet werden, um Workloads in ihren eigenen VNETs, die getrennt von anderen Spokes verwaltet werden, zu isolieren. Jede Workload kann mehrere Schichten umfassen, wobei mehrere Subnetze über Azure Load Balancer verbunden sind. Weitere Informationen zur Anwendungsinfrastruktur finden Sie unter [Ausführen von Windows-VM-Workloads][windows-vm-ra] und [Ausführen von Linux-VM-Workloads][linux-vm-ra].

* **VNET-Peering**: Über eine [Peeringverbindung][vnet-peering] können zwei VNETs in derselben Azure-Region miteinander verbunden werden. Peeringverbindungen sind nicht-transitive Verbindungen zwischen VNETs mit niedrigen Latenzen. Sobald eine Peeringverbindung hergestellt wurde, tauschen die VNETs ohne Einsatz eines Routers Datenverkehr über den Azure-Backbone aus. In einer Hub-Spoke-Netzwerktopologie wird durch VNET-Peering eine Verbindung zwischen dem Hub und den einzelnen Spokes hergestellt.

> [!NOTE]
> In diesem Artikel werden ausschließlich [Resource Manager](/azure/azure-resource-manager/resource-group-overview)-Bereitstellungen behandelt, Sie können jedoch auch eine Verbindung zwischen einem klassischen VNET und einem Resource Manager-VNET im selben Abonnement herstellen. Auf diese Weise können Ihre Spokes klassische Bereitstellungen hosten und trotzdem von gemeinsamen Diensten im Hub profitieren.

## <a name="recommendations"></a>Empfehlungen

Alle Empfehlungen für die [Hub-Spoke][guidance-hub-spoke]-Referenzarchitektur gelten auch für die Referenzarchitektur für gemeinsame Dienste. 

Außerdem gelten die folgenden Empfehlungen für die meisten Szenarien mit gemeinsamen Diensten. Sofern Sie keine besonderen Anforderungen haben, die Vorrang haben, sollten Sie diese Empfehlungen befolgen.

### <a name="identity"></a>Identity

Die meisten Organisationen großer Unternehmen verfügen im lokalen Rechenzentrum über eine AD DS-Umgebung (Active Directory Domain Services). Um die Verwaltung von Assets zu ermöglichen, die aus Ihrem lokalen Netzwerk nach Azure verschoben werden und von AD DS abhängig sind, ist es ratsam, AD DS-Domänencontroller in Azure zu hosten.

Falls Sie Gruppenrichtlinienobjekte nutzen, die Sie für Azure und Ihre lokale Umgebung separat steuern möchten, sollten Sie für jede Azure-Region einen anderen AD-Standort verwenden. Ordnen Sie Ihre Domänencontroller in einem zentralen VNET (Hub) an, auf das abhängige Workloads zugreifen können.

### <a name="security"></a>Sicherheit

Wenn Sie Workloads aus Ihrer lokalen Umgebung nach Azure verschieben, müssen einige dieser Workloads auf VMs gehostet werden. Aus Gründen der Konformität müssen Sie für Datenverkehr, der diese Workloads durchläuft, unter Umständen Einschränkungen erzwingen. 

Sie können virtuelle Netzwerkgeräte in Azure verwenden, um unterschiedliche Arten von Sicherheits- und Leistungsdiensten zu hosten. Wenn Sie derzeit mit einem bestimmten Satz von lokalen Geräten vertraut sind, ist es ratsam, jeweils die gleichen virtualisierten Geräte in Azure zu verwenden (falls zutreffend).

> [!NOTE]
> Für die Bereitstellungsskripts dieser Referenzarchitektur wird eine Ubuntu-VM mit aktivierter IP-Weiterleitung verwendet, um ein virtuelles Netzwerkgerät zu imitieren.

## <a name="considerations"></a>Überlegungen

### <a name="overcoming-vnet-peering-limits"></a>Umgehung von VNET-Peeringbeschränkungen

Stellen Sie sicher, dass Sie die [Begrenzung der Anzahl von VNET-Peerings pro VNET][vnet-peering-limit] in Azure einhalten. Falls Sie mehr Spokes als die maximal zulässige Anzahl benötigen, sollten Sie eventuell eine Hub-Spoke-Hub-Spoke-Topologie erstellen, bei der die erste Ebene von Spokes auch als Hub fungiert. Im folgenden Diagramm wird diese Vorgehensweise veranschaulicht.

![[3]][3]

Berücksichtigen Sie auch die Dienste, die im Hub gemeinsam genutzt werden, um sicherzustellen, dass sich der Hub für eine größere Anzahl von Spokes skalieren lässt. Wenn Ihr Hub beispielsweise Firewalldienste bereitstellt, sollten Sie beim Hinzufügen mehrerer Spokes die Bandbreitenbeschränkungen Ihrer Firewalllösung berücksichtigen. Es wird empfohlen, einige dieser gemeinsamen Dienste auf eine zweite Hubebene zu verlagern.

## <a name="deploy-the-solution"></a>Bereitstellen der Lösung

Eine Bereitstellung für diese Architektur ist auf [GitHub][ref-arch-repo] verfügbar. Die Bereitstellung erstellt die folgenden Ressourcengruppen in Ihrem Abonnement:

- hub-adds-rg
- hub-nva-rg
- hub-vnet-rg
- onprem-vnet-rg
- spoke1-vnet-rg
- spoke2-vent-rg

Die Vorlagenparameterdateien beziehen sich auf diese Namen, d.h. wenn Sie sie ändern, müssen Sie die Parameterdateien entsprechend aktualisieren.

### <a name="prerequisites"></a>Voraussetzungen

[!INCLUDE [ref-arch-prerequisites.md](../../../includes/ref-arch-prerequisites.md)]

### <a name="deploy-the-simulated-on-premises-datacenter-using-azbb"></a>Bereitstellen des simulierten lokalen Rechenzentrums mit azbb

In diesem Schritt wird das simulierte lokale Datencenter als Azure-VNET bereitgestellt.

1. Navigieren Sie zum Ordner `hybrid-networking\shared-services-stack\` des GitHub-Repositorys.

2. Öffnen Sie die Datei `onprem.json` . 

3. Suchen Sie nach allen Instanzen von `Password` und `adminPassword`. Geben Sie in den Parametern Werte für den Benutzernamen und das Kennwort ein, und speichern Sie die Datei. 

4. Führen Sie den folgenden Befehl aus:

   ```bash
   azbb -s <subscription_id> -g onprem-vnet-rg -l <location> -p onprem.json --deploy
   ```
5. Warten Sie, bis die Bereitstellung abgeschlossen ist. Bei dieser Bereitstellung werden ein virtuelles Netzwerk, ein virtueller Windows-Computer und ein VPN-Gateway erstellt. Die Erstellung des VPN-Gateways kann mehr als 40 Minuten in Anspruch nehmen.

### <a name="deploy-the-hub-vnet"></a>Bereitstellen des Hub-VNet

In diesem Schritt wird das Hub-VNET bereitgestellt und mit dem simulierten lokalen VNET verbunden.

1. Öffnen Sie die Datei `hub-vnet.json` . 

2. Suchen Sie nach `adminPassword`, und geben Sie in den Parametern einen Benutzernamen und ein Kennwort ein. 

3. Suchen Sie nach allen Instanzen von `sharedKey`, und geben Sie einen Wert für einen freigegebenen Schlüssel ein. Speichern Sie die Datei .

   ```bash
   "sharedKey": "abc123",
   ```

4. Führen Sie den folgenden Befehl aus:

   ```bash
   azbb -s <subscription_id> -g hub-vnet-rg -l <location> -p hub-vnet.json --deploy
   ```

5. Warten Sie, bis die Bereitstellung abgeschlossen ist. Bei dieser Bereitstellung werden ein virtuelles Netzwerk, ein virtueller Computer, ein VPN-Gateway und eine Verbindung mit dem im vorherigen Abschnitt erstellten Gateway erstellt. Die Erstellung des VPN-Gateways kann mehr als 40 Minuten in Anspruch nehmen.

### <a name="deploy-ad-ds-in-azure"></a>Bereitstellen von AD DS in Azure

In diesem Schritt werden AD DS-Domänencontroller in Azure bereitgestellt.

1. Öffnen Sie die Datei `hub-adds.json` .

2. Suchen Sie nach allen Instanzen von `Password` und `adminPassword`. Geben Sie in den Parametern Werte für den Benutzernamen und das Kennwort ein, und speichern Sie die Datei. 

3. Führen Sie den folgenden Befehl aus:

   ```bash
   azbb -s <subscription_id> -g hub-adds-rg -l <location> -p hub-adds.json --deploy
   ```
  
Dieser Bereitstellungsschritt kann einige Minuten dauern, da die beiden virtuellen Computer der Domäne hinzugefügt werden, die im simulierten lokalen Datencenter gehostet wird, und AD DS darauf installiert wird.

### <a name="deploy-the-spoke-vnets"></a>Bereitstellen der Spoke-VNets

In diesem Schritt werden die Spoke-VNETs bereitgestellt.

1. Öffnen Sie die Datei `spoke1.json` .

2. Suchen Sie nach `adminPassword`, und geben Sie in den Parametern einen Benutzernamen und ein Kennwort ein. 

3. Führen Sie den folgenden Befehl aus:

   ```bash
   azbb -s <subscription_id> -g spoke1-vnet-rg -l <location> -p spoke1.json --deploy
   ```
  
4. Wiederholen Sie die Schritte 1 und 2 für die Datei `spoke2.json`.

5. Führen Sie den folgenden Befehl aus:

   ```bash
   azbb -s <subscription_id> -g spoke2-vnet-rg -l <location> -p spoke2.json --deploy
   ```

### <a name="peer-the-hub-vnet-to-the-spoke-vnets"></a>Verknüpfen des Hub-VNETs mit den Spoke-VNETs

Führen Sie den folgenden Befehl aus, um eine Peeringverbindung zwischen dem Hub-VNET und den Spoke-VNETs herzustellen:

```bash
azbb -s <subscription_id> -g hub-vnet-rg -l <location> -p hub-vnet-peering.json --deploy
```

### <a name="deploy-the-nva"></a>Bereitstellen des NVA

In diesem Schritt wird ein NVA im Subnetz `dmz` bereitgestellt.

1. Öffnen Sie die Datei `hub-nva.json` .

2. Suchen Sie nach `adminPassword`, und geben Sie in den Parametern einen Benutzernamen und ein Kennwort ein. 

3. Führen Sie den folgenden Befehl aus:

   ```bash
   azbb -s <subscription_id> -g hub-nva-rg -l <location> -p hub-nva.json --deploy
   ```

### <a name="test-connectivity"></a>Testen der Konnektivität 

Testen Sie die Konnektivität zwischen der simulierten lokalen Umgebung und dem Hub-VNET.

1. Suchen Sie im Azure-Portal die VM namens `jb-vm1` in der `onprem-jb-rg`-Ressourcengruppe.

2. Klicke Sie auf `Connect`, um eine Remotedesktopsitzung mit der VM zu öffnen. Verwenden Sie das Kennwort, das Sie in der `onprem.json`-Parameterdatei angegeben haben.

3. Öffnen Sie auf der VM eine PowerShell-Konsole, und verwenden Sie das `Test-NetConnection`-Cmdlet, um sicherzustellen, dass Sie eine Verbindung mit der Jumpbox-VM im Hub-VNet herstellen können.

   ```powershell
   Test-NetConnection 10.0.0.68 -CommonTCPPort RDP
   ```
Die Ausgabe sollte in etwa wie folgt aussehen:

```powershell
ComputerName     : 10.0.0.68
RemoteAddress    : 10.0.0.68
RemotePort       : 3389
InterfaceAlias   : Ethernet 2
SourceAddress    : 192.168.1.000
TcpTestSucceeded : True
```

> [!NOTE]
> Standardmäßig lassen Windows Server-VMs in Azure keine ICMP-Antworten zu. Wenn Sie `ping` zum Testen der Konnektivität nutzen möchten, müssen Sie für jede VM ICMP-Datenverkehr in der erweiterten Windows-Firewall aktivieren.

Wiederholen Sie die gleichen Schritte, um die Konnektivität mit den Spoke-VNETs zu testen:

```powershell
Test-NetConnection 10.1.0.68 -CommonTCPPort RDP
Test-NetConnection 10.2.0.68 -CommonTCPPort RDP
```


<!-- links -->

[azure-cli-2]: /azure/install-azure-cli
[azbb]: https://github.com/mspnp/template-building-blocks/wiki/Install-Azure-Building-Blocks
[guidance-hub-spoke]: ./hub-spoke.md
[azure-vpn-gateway]: /azure/vpn-gateway/vpn-gateway-about-vpngateways
[best-practices-security]: /azure/best-practices-network-securit
[connect-to-an-Azure-vnet]: https://technet.microsoft.com/library/dn786406.aspx
[guidance-expressroute]: ./expressroute.md
[guidance-vpn]: ./vpn.md
[linux-vm-ra]: ../virtual-machines-linux/index.md
[hybrid-ha]: ./expressroute-vpn-failover.md
[naming conventions]: /azure/guidance/guidance-naming-conventions
[resource-manager-overview]: /azure/azure-resource-manager/resource-group-overview
[virtuellen Rechenzentrums]: https://aka.ms/vdc
[vnet-peering]: /azure/virtual-network/virtual-network-peering-overview
[vnet-peering-limit]: /azure/azure-subscription-service-limits#networking-limits
[vpn-appliance]: /azure/vpn-gateway/vpn-gateway-about-vpn-devices
[windows-vm-ra]: ../virtual-machines-windows/index.md

[visio-download]: https://archcenter.blob.core.windows.net/cdn/hybrid-network-hub-spoke.vsdx
[ref-arch-repo]: https://github.com/mspnp/reference-architectures
[0]: ./images/shared-services.png "Topologie mit gemeinsamen Diensten in Azure"
[3]: ./images/hub-spokehub-spoke.svg "Hub-Spoke-Hub-Spoke-Topologie in Azure"
[ARM-Templates]: https://azure.microsoft.com/documentation/articles/resource-group-authoring-templates/
