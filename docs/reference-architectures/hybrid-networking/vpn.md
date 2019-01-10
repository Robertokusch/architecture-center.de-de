---
title: Verbinden eines lokalen Netzwerks mit Azure über ein VPN
titleSuffix: Azure Reference Architectures
description: Hier wird erläutert, wie Sie eine sichere Netzwerkarchitektur zwischen Standorten implementieren, die ein virtuelles Azure-Netzwerk und ein lokales Netzwerk umfasst, die über ein VPN verbunden sind.
author: RohitSharma-pnp
ms.date: 10/22/2018
ms.openlocfilehash: 63e919041e4a8c7c9f90a49de05effc48cb743d8
ms.sourcegitcommit: 1f4cdb08fe73b1956e164ad692f792f9f635b409
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/08/2019
ms.locfileid: "54110909"
---
# <a name="connect-an-on-premises-network-to-azure-using-a-vpn-gateway"></a>Verbinden eines lokalen Netzwerks mit Azure über ein VPN-Gateway

Diese Referenzarchitektur zeigt, wie Sie ein lokales Netzwerk auf Azure ausdehnen, indem Sie ein VPN (virtuelles privates Netzwerk) zwischen Standorten verwenden. Der Datenverkehr zwischen dem lokalen Netzwerk und dem virtuellen Azure-Netzwerk (VNet) wird durch einen IPSec-VPN-Tunnel übertragen. [**Stellen Sie diese Lösung bereit**](#deploy-the-solution).

![Hybrides Netzwerk, das die lokale Infrastruktur und die Azure-Infrastruktur umfasst](./images/vpn.png)

*Laden Sie eine [Visio-Datei][visio-download] mit dieser Architektur herunter.*

## <a name="architecture"></a>Architecture

Die Architektur umfasst die folgenden Komponenten.

- **Lokales Netzwerk**. Ein in einer Organisation betriebenes privates lokales Netzwerk.

- **VPN-Gerät**. Ein Gerät oder ein Dienst, das bzw. der externe Konnektivität mit dem lokalen Netzwerk bereitstellt. Bei dem VPN-Gerät kann es sich um ein Hardwaregerät oder eine Softwarelösung, z.B. den Routing- und RAS-Dienst unter Windows Server 2012, handeln. Eine Liste unterstützter VPN-Geräte und Informationen zu ihrer Konfiguration für die Verbindung mit einem Azure-VPN-Gateway finden Sie in den Anweisungen für das ausgewählte Gerät unter [Informationen zu VPN-Geräten für VPN-Gatewayverbindungen zwischen Standorten][vpn-appliance].

- **Virtuelles Netzwerk (VNET)**. Die Cloudanwendung und die Komponenten des Azure-VPN-Gateways befinden sich im selben [VNet][azure-virtual-network].

- **Azure-VPN-Gateway**. Der Dienst [VPN Gateway][azure-vpn-gateway] ermöglicht dem VNet, über ein VPN-Gerät eine Verbindung mit dem lokalen Netzwerk herzustellen. Weitere Informationen finden Sie unter [Verbinden eines lokalen Netzwerks mit einem Microsoft Azure Virtual Network][connect-to-an-Azure-vnet]. Das VPN-Gateway enthält die folgenden Elemente:

  - **Gateway des virtuellen Netzwerks**. Eine Ressource, die dem VNet ein virtuelles VPN-Gerät bereitstellt. Dieses ist für die Weiterleitung des Datenverkehrs vom lokalen Netzwerk zum VNet zuständig.
  - **Gateway des lokalen Netzwerks**. Eine Abstraktion des lokalen VPN-Geräts. Netzwerkdatenverkehr aus der Cloudanwendung zum lokalen Netzwerk wird durch dieses Gateway geleitet.
  - **Verbindung**. Die Verbindung verfügt über Eigenschaften, die den Verbindungstyp (IPSec) und den Schlüssel angeben, der für das lokale VPN-Gerät freigegeben wird, um Datenverkehr zu verschlüsseln.
  - **Gatewaysubnetz**. Das virtuelle Netzwerkgateway befindet sich in einem eigenen Subnetz, das verschiedenen Anforderungen unterliegt, die im Abschnitt „Empfehlungen“ weiter unten beschrieben werden.

- **Cloudanwendung**. Die in Azure gehostete Anwendung. Sie kann mehrere Schichten umfassen, wobei mehrere Subnetze über Azure Load Balancer verbunden sind. Weitere Informationen zur Anwendungsinfrastruktur finden Sie unter [Ausführen von Windows-VM-Workloads][windows-vm-ra] und [Ausführen von Linux-VM-Workloads][linux-vm-ra].

- **Interner Load Balancer**. Netzwerkverkehr aus dem VPN-Gateway wird über einen internen Load Balancer an die Cloudanwendung weitergeleitet. Der Load Balancer befindet sich im Front-End-Subnetz der Anwendung.

## <a name="recommendations"></a>Empfehlungen

Die folgenden Empfehlungen gelten für die meisten Szenarios. Sofern Sie keine besonderen Anforderungen haben, die Vorrang haben, sollten Sie diese Empfehlungen befolgen.

### <a name="vnet-and-gateway-subnet"></a>VNet und Gatewaysubnetz

Erstellen Sie ein Azure VNet mit einem Adressraum, der für alle erforderlichen Ressourcen ausreichend groß ist. Stellen Sie sicher, dass der VNet-Adressraum genügend Platz für Wachstum bietet, falls in Zukunft weitere VMs benötigt werden. Der Adressraum des VNet darf sich nicht mit dem des lokalen Netzwerks überschneiden. Im obigen Diagramm wird beispielsweise der Adressraum 10.20.0.0/16 für das VNet verwendet.

Erstellen Sie ein Subnetz mit dem Namen *GatewaySubnet* mit dem Adressbereich „/27“. Dieses Subnetz ist für das Gateway für virtuelle Netzwerke erforderlich. Durch Zuweisung von 32 Adressen zu diesem Subnetz wird eine künftige Überschreitung von Beschränkungen der Gatewaygröße verhindert. Vermeiden Sie außerdem, dieses Subnetz in der Mitte des Adressraums zu platzieren. Eine empfehlenswerte Vorgehensweise besteht darin, den Adressraum für das Gatewaysubnetz am oberen Ende des VNet-Adressraums festzulegen. In dem im Diagramm gezeigten Beispiel wird „10.20.255.224/27“ verwendet.  Es folgt eine schnelle Methode zur [CIDR]-Berechnung:

1. Legen Sie die variablen Bits im Adressraum des VNet bis zu den Bits, die vom Gatewaysubnetz verwendet werden, auf 1 und dann die restlichen Bits auf 0 fest.
2. Konvertieren Sie die resultierenden Bits in Dezimalzahlen, und drücken Sie sie als Adressraum aus, wobei die Präfixlänge auf die Größe des Subnetzes des Gateways festgelegt wird.

Beispielsweise ändert sich der IP-Adressbereich „10.20.0.0.0/16“ eines VNet nach Anwenden von Schritt 1 oben in „10.20.0b11111111.0b11100000“.  Nach der Konvertierung in Dezimalzahlen und deren Ausdruck als Adressraum ergibt sich „10.20.255.255.224/27“.

> [!WARNING]
> Stellen Sie im Gatewaysubnetz keine VMs bereit. Weisen Sie außerdem diesem Subnetz keine Netzwerksicherheitsgruppe (NSG) zu, da das Gateway sonst nicht mehr funktioniert.
>

### <a name="virtual-network-gateway"></a>Gateway des virtuellen Netzwerks

Ordnen Sie dem Gateway für virtuelle Netzwerke eine öffentliche IP-Adresse zu.

Erstellen Sie das Gateway für virtuelle Netzwerke im Gatewaysubnetz, und weisen Sie ihm die neu zugeordnete öffentliche IP-Adresse zu. Verwenden Sie den Gatewaytyp, der Ihren Anforderungen am ehesten entspricht und der von Ihrem VPN-Gerät unterstützt wird:

- Erstellen Sie ein [richtlinienbasiertes Gateway ][policy-based-routing], wenn Sie die Weiterleitung von Anforderungen anhand von Richtlinienkriterien wie Adresspräfixen genau steuern müssen. Richtlinienbasierte Gateways arbeiten mit statischem Routing und funktionieren nur mit Verbindungen zwischen Standorten.

- Erstellen Sie ein [routenbasiertes Gateway][route-based-routing], wenn Sie eine Verbindung mit dem lokalen Netzwerk über RRAS herstellen, Verbindungen mit mehreren Standorten oder regionsübergreifende Verbinden unterstützen oder VNet-zu-VNet-Verbindungen implementieren (einschließlich Routen, die mehrere VNets durchqueren). Routenbasierte Gateways verwenden dynamisches Routing, um den Datenverkehr zwischen Netzwerken zu lenken. Sie können Ausfälle im Netzwerkpfad besser verkraften als statische Routen, weil sie alternative Routen ausprobieren können. Routenbasierte Gateways können auch den Verwaltungsaufwand reduzieren, da Routen nicht manuell aktualisiert werden müssen, wenn sich Netzwerkadressen ändern.

Eine Liste der unterstützten VPN-Geräte finden Sie unter [Informationen zu VPN-Geräten für VPN Gateway-Verbindungen zwischen Standorten][vpn-appliances].

> [!NOTE]
> Wenn Sie nach Erstellung des Gateways die Gatewaytypen ändern möchten, müssen Sie das Gateway löschen und neu erstellen.
>

Wählen Sie die Azure VPN Gateway SKU, die Ihren Durchsatzanforderungen am ehesten entspricht. Weitere Informationen finden Sie unter [Gateway-SKUs][azure-gateway-skus].

> [!NOTE]
> Die SKU „Basic“ ist nicht mit Azure ExpressRoute kompatibel. Sie können [die SKU ändern][changing-SKUs], nachdem das Gateway erstellt wurde.
>

Gebühren fallen basierend auf der Bereitstellungs- und Verfügbarkeitsdauer des Gateways an. Siehe [VPN Gateway – Preise][azure-gateway-charges].

Erstellen Sie Routingregeln für das Gatewaysubnetz, die eingehenden Anwendungsdatenverkehr vom Gateway zum internen Load Balancer leiten, anstatt Anforderungen direkt an die Anwendungs-VMs weiterleiten zu lassen.

### <a name="on-premises-network-connection"></a>Lokale Netzwerkverbindung

Erstellen Sie ein Gateway für das lokale Netzwerk. Geben Sie die öffentliche IP-Adresse des lokalen VPN-Geräts und den Adressraum des lokalen Netzwerks an. Beachten Sie, dass das lokale VPN-Gerät über eine öffentliche IP-Adresse verfügen muss, auf die das lokale Netzwerkgateway in Azure VPN Gateway zugreifen kann. Das VPN-Gerät darf sich nicht hinter einem Gerät für Netzwerkadressenübersetzung (Network Address Translation, NAT) befinden.

Richten Sie für das virtuelle und das lokale Netzwerkgateway eine Verbindung zwischen Standorten ein. Wählen Sie den Typ der Verbindung zwischen Standorten (IPSec) aus, und geben Sie den gemeinsam verwendeten Schlüssel an. Die Verschlüsselung zwischen Standorten mit dem Azure-VPN-Gateway basiert auf dem IPSec-Protokoll, wobei zur Authentifizierung vorinstallierte Schlüssel verwendet werden. Sie können den Schlüssel angeben, wenn Sie das Azure-VPN-Gateway erstellen. Sie müssen das VPN-Gerät, das lokal ausgeführt wird, mit demselben Schlüssel konfigurieren. Andere Authentifizierungsmechanismen werden derzeit nicht unterstützt.

Stellen Sie sicher, dass die lokale Routinginfrastruktur so konfiguriert ist, dass Anforderungen für Adressen im Azure VNet an das VPN-Gerät weitergeleitet werden.

Öffnen Sie im lokalen Netzwerk alle Ports, die von der Cloudanwendung benötigt werden.

Testen Sie die Verbindung, um zu überprüfen, ob:

- Das lokale VPN-Gerät den Datenverkehr über das Azure-VPN-Gateway ordnungsgemäß an die Cloudanwendung weiterleitet.
- Das VNet den Datenverkehr ordnungsgemäß zum lokalen Netzwerk zurückleitet.
- Unzulässiger Datenverkehr in beide Richtungen ordnungsgemäß blockiert wird.

## <a name="scalability-considerations"></a>Überlegungen zur Skalierbarkeit

Sie können eine begrenzte vertikale Skalierbarkeit erreichen, indem Sie von den VPN Gateway SKUs „Basic“ oder „Standard“ zur VPN SKU „High Performance“ wechseln.

Für VNets, die ein hohes Volumen an VPN-Datenverkehr erwarten, sollten Sie das Verteilen der verschiedenen Workloads in separate kleinere VNets und die Konfiguration eines VPN-Gateways für jedes einzelne davon in Betracht ziehen.

Das VNet kann entweder horizontal oder vertikal partitioniert werden. Verschieben Sie zum horizontalen Partitionieren einige VM-Instanzen von jeder Ebene in Subnetze des neuen VNet. Das Ergebnis ist, dass jedes VNet dieselbe Struktur und Funktionalität aufweist. Um eine vertikale Partitionierung vorzunehmen, müssen Sie jede Ebene neu gestalten, um die Funktionalität in verschiedene logische Bereiche zu unterteilen (z.B. Auftragsbearbeitung, Fakturierung, Kundenbetreuung usw.). Jeder Funktionsbereich kann dann in einem eigenen VNet platziert werden.

Die Replikation eines lokalen Active Directory-Domänencontrollers im VNet und die Implementierung von DNS im VNet kann dazu beitragen, einen Teil des sicherheitsrelevanten und administrativen Datenverkehrs zu reduzieren, der von den lokalen Systemen in die Cloud fließt. Weitere Informationen finden Sie unter [Erweitern von Active Directory Domain Services (AD DS) auf Azure][adds-extend-domain].

## <a name="availability-considerations"></a>Überlegungen zur Verfügbarkeit

Wenn Sie sicherstellen müssen, dass das lokale Netzwerk für das Azure-VPN-Gateway verfügbar bleibt, implementieren Sie für das lokale VPN-Gateway einen Failovercluster.

Wenn Ihre Organisation über mehrere lokale Standorte verfügt, richten Sie [Verbindungen dieser Standorte][vpn-gateway-multi-site] mit einem oder mehreren Azure VNets ein. Dieser Ansatz erfordert dynamisches (routenbasiertes) Routing. Stellen Sie daher sicher, dass das lokale VPN-Gateway dies unterstützt.

Weitere Informationen zu Vereinbarungen zum Servicelevel finden Sie unter [SLA für VPN Gateway][sla-for-vpn-gateway].

## <a name="manageability-considerations"></a>Überlegungen zur Verwaltbarkeit

Überwachen Sie von lokalen VPN-Geräten stammende Diagnoseinformationen. Dieser Vorgang hängt von den Funktionen ab, die das VPN-Gerät bietet. Wenn Sie z.B. den Routing- und RAS-Dienst unter Windows Server 2012 verwenden, wählen Sie die [RRAS-Protokollierung][rras-logging].

Verwenden Sie die [Azure VPN Gateway-Diagnose][gateway-diagnostic-logs] zum Erfassen von Informationen zu Verbindungsproblemen. Diese Protokolle können verwendet werden, um Informationen wie Quelle und Ziele von Verbindungsanforderungen nachzuverfolgen, welches Protokoll verwendet wurde und wie die Verbindung aufgebaut wurde (oder warum der Versuch fehlgeschlagen ist).

Überwachen Sie die Betriebsprotokolle des Azure-VPN-Gateways mithilfe der Überwachungsprotokolle, die im Azure-Portal verfügbar sind. Für das lokale Netzwerkgateway, das Azure-Netzwerkgateway und die Verbindung sind separate Protokolle verfügbar. Diese Informationen können verwendet werden, um jegliche Änderungen am Gateway nachzuverfolgen, und sind ggf. nützlich, wenn ein zuvor funktionierendes Gateway aus irgendeinem Grund nicht mehr funktioniert.

![Überwachungsprotokolle im Azure-Portal](../_images/guidance-hybrid-network-vpn/audit-logs.png)

Überwachen Sie Verbindungen, und verfolgen Sie Verbindungsfehlerereignisse nach. Sie können ein Überwachungspaket wie [Nagios][nagios] verwenden, um diese Informationen zu erfassen und zu melden.

## <a name="security-considerations"></a>Sicherheitshinweise

Generieren Sie für jedes VPN-Gateway einen anderen gemeinsam verwendeten Schlüssel. Wählen Sie einen sicheren gemeinsam verwendeten Schlüssel, um Brute-Force-Angriffe abzuwehren.

> [!NOTE]
> Derzeit können Sie Azure Key Vault nicht für das Vorinstallieren von Schlüsseln für das Azure-VPN-Gateway nutzen.
>

Stellen Sie sicher, dass das lokale VPN-Gerät eine Verschlüsselungsmethode verwendet, die [mit dem Azure-VPN-Gateway kompatibel][vpn-appliance-ipsec] ist. Für richtlinienbasiertes Routing unterstützt das Azure-VPN-Gateway die Verschlüsselungsalgorithmen AES256, AES128 und 3DES. Routenbasierte Gateways unterstützen AES256 und 3DES.

Wenn sich Ihr lokales VPN-Gerät in einem Umkreisnetzwerk (DMZ) befindet, das eine Firewall zwischen dem Umkreisnetzwerk und dem Internet aufweist, müssen Sie möglicherweise [zusätzliche Firewallregeln][additional-firewall-rules] konfigurieren, um die VPN-Verbindung zwischen Standorten zuzulassen.

Wenn die Anwendung im VNet Daten an das Internet sendet, erwägen Sie [die Implementierung der Tunnelerzwingung][forced-tunneling], um den gesamten internetgebundenen Datenverkehr durch das lokale Netzwerk zu leiten. Auf diese Weise können Sie aus der lokalen Infrastruktur ausgehende Anforderungen der Anwendung überprüfen.

> [!NOTE]
> Die Tunnelerzwingung kann die Konnektivität mit Azure-Diensten (z.B. Azure Storage) und dem Windows-Lizenz-Manager beeinträchtigen.
>

## <a name="deploy-the-solution"></a>Bereitstellen der Lösung

**Voraussetzungen** Sie müssen über eine lokale Infrastruktur verfügen, die bereits mit einer geeigneten Netzwerkappliance konfiguriert ist.

Führen Sie die folgenden Schritte aus, um die Lösung bereitzustellen.

<!-- markdownlint-disable MD033 -->

1. Klicken Sie auf diese Schaltfläche:<br><a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2Fmspnp%2Freference-architectures%2Fmaster%2Fhybrid-networking%2Fvpn%2Fazuredeploy.json" target="_blank"><img src="https://azuredeploy.net/deploybutton.png"/></a>
2. Warten Sie, bis der Link im Azure-Portal geöffnet wird, und gehen Sie dann folgendermaßen vor:
   - Der Name der **Ressourcengruppe** ist bereits in der Parameterdatei definiert. Wählen Sie also **Neu erstellen**, und geben Sie im Textfeld `ra-hybrid-vpn-rg` ein.
   - Wählen Sie im Dropdownfeld **Standort** die Region aus.
   - Lassen Sie die Textfelder für den **Vorlagenstamm-URI** bzw. **Parameterstamm-URI** unverändert.
   - Überprüfen Sie die allgemeinen Geschäftsbedingungen, und aktivieren Sie dann das Kontrollkästchen **Ich stimme den oben genannten Geschäftsbedingungen zu**.
   - Klicken Sie auf die Schaltfläche **Kaufen**.
3. Warten Sie, bis die Bereitstellung abgeschlossen ist.

<!-- markdownlint-enable MD033 -->

Informationen zum Behandeln von Verbindungsproblemen finden Sie unter [Behandeln von Problemen mit einer VPN-Hybridverbindung](./troubleshoot-vpn.md).

<!-- links -->

[adds-extend-domain]: ../identity/adds-extend-domain.md
[windows-vm-ra]: ../virtual-machines-windows/index.md
[linux-vm-ra]: ../virtual-machines-linux/index.md

[azure-cli]: /azure/virtual-machines-command-line-tools
[azure-virtual-network]: /azure/virtual-network/virtual-networks-overview
[vpn-appliance]: /azure/vpn-gateway/vpn-gateway-about-vpn-devices
[azure-vpn-gateway]: https://azure.microsoft.com/services/vpn-gateway/
[azure-gateway-charges]: https://azure.microsoft.com/pricing/details/vpn-gateway/
[azure-gateway-skus]: /azure/vpn-gateway/vpn-gateway-about-vpngateways#gwsku
[connect-to-an-Azure-vnet]: https://technet.microsoft.com/library/dn786406.aspx
[vpn-gateway-multi-site]: /azure/vpn-gateway/vpn-gateway-multi-site
[policy-based-routing]: https://en.wikipedia.org/wiki/Policy-based_routing
[route-based-routing]: https://en.wikipedia.org/wiki/Static_routing
[sla-for-vpn-gateway]: https://azure.microsoft.com/support/legal/sla/vpn-gateway/
[additional-firewall-rules]: https://technet.microsoft.com/library/dn786406.aspx#firewall
[nagios]: https://www.nagios.org/
[changing-SKUs]: https://azure.microsoft.com/blog/azure-virtual-network-gateway-improvements/
[gateway-diagnostic-logs]: https://blogs.technet.microsoft.com/keithmayer/2016/10/12/step-by-step-capturing-azure-resource-manager-arm-vnet-gateway-diagnostic-logs/
[rras-logging]: https://www.petri.com/enable-diagnostic-logging-in-windows-server-2012-r2-routing-and-remote-access
[forced-tunneling]: /azure/vpn-gateway/vpn-gateway-about-forced-tunneling
[vpn-appliances]: /azure/vpn-gateway/vpn-gateway-about-vpn-devices
[visio-download]: https://archcenter.blob.core.windows.net/cdn/hybrid-network-architectures.vsdx
[vpn-appliance-ipsec]: /azure/vpn-gateway/vpn-gateway-about-vpn-devices#ipsec-parameters
[azure-cli]: /cli/azure/install-azure-cli
[CIDR]: https://en.wikipedia.org/wiki/Classless_Inter-Domain_Routing
