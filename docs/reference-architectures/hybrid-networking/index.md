---
title: Verbinden eines lokalen Netzwerks mit Azure
titleSuffix: Azure Reference Architectures
description: Dieser Artikel vergleicht Referenzarchitekturen zum Herstellen einer Verbindung zwischen einem lokalen Netzwerk und Azure.
author: telmosampaio
ms.date: 07/02/2018
ms.openlocfilehash: f13249f225ad7ab5072de2b2175cdc2ffb6d0074
ms.sourcegitcommit: 680c9cef945dff6fee5e66b38e24f07804510fa9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/04/2019
ms.locfileid: "54011054"
---
# <a name="choose-a-solution-for-connecting-an-on-premises-network-to-azure"></a>Auswählen einer Lösung zum Herstellen einer Verbindung zwischen einem lokalen Netzwerk und Azure

Dieser Artikel vergleicht Optionen zum Herstellen einer Verbindung zwischen einem lokalen Netzwerk und einem virtuellen Azure-Netzwerk (VNET). Für jede Option ist eine detailliertere Referenzarchitektur verfügbar.

## <a name="vpn-connection"></a>VPN-Verbindung

Ein [VPN Gateway](/azure/vpn-gateway/vpn-gateway-about-vpngateways) ist eine Art von Gateway für virtuelle Netzwerke, mit dem verschlüsselter Datenverkehr zwischen einem virtuellen Azure-Netzwerk und einem lokalen Standort gesendet wird. Der verschlüsselte Datenverkehr wird über das öffentliche Internet übertragen.

Diese Architektur eignet sich für hybride Anwendungen, bei denen wahrscheinlich nur wenig Datenverkehr zwischen der lokalen Hardware und der Cloud stattfindet. Sie ist auch dann geeignet, wenn Sie eine geringfügig erhöhte Latenz in Kauf nehmen können, um von der Flexibilität und der Verarbeitungsleistung der Cloud zu profitieren.

### <a name="benefits"></a>Vorteile

- Diese Architektur lässt sich leicht konfigurieren.

### <a name="challenges"></a>Herausforderungen

- Es ist ein lokales VPN-Gerät erforderlich.
- Microsoft garantiert zwar für jedes VPN Gateway eine Verfügbarkeit von 99,9 %, diese [SLA](https://azure.microsoft.com/support/legal/sla/vpn-gateway/) gilt aber nur für das VPN Gateway, nicht für Ihre Netzwerkverbindung zum Gateway.
- Eine VPN-Verbindung über Azure VPN Gateway unterstützt zurzeit eine Bandbreite von maximal 1,25 GBit/s. Möglicherweise müssen Sie Ihr virtuelles Azure-Netzwerk über mehrere VPN-Verbindungen hinweg partitionieren, wenn Sie damit rechnen, diesen Durchsatz zu überschreiten.

### <a name="reference-architecture"></a>Referenzarchitektur

- [Hybridnetzwerk mit VPN-Gateway](./vpn.md)

<!-- markdownlint-disable MD024 -->

## <a name="azure-expressroute-connection"></a>Azure ExpressRoute-Verbindung

[ExpressRoute](/azure/expressroute/)-Verbindungen nutzen eine dedizierte private Verbindung über einen Drittanbieter für die Konnektivität. Die private Verbindung erweitert Ihr lokales Netzwerk auf Azure.

Diese Architektur eignet sich für hybride Anwendungen, die umfangreiche geschäftskritische Workloads ausführen, für die ein hohes Maß an Skalierbarkeit erforderlich ist.

### <a name="benefits"></a>Vorteile

- Es steht eine wesentlich höhere Bandbreite zur Verfügung – bis zu 10 GBit/s, je nach Konnektivitätsanbieter.
- Die Architektur unterstützt eine dynamische Skalierung der Bandbreite, um in Zeiträumen mit geringerer Nachfrage die Kosten zu senken. Allerdings steht diese Option nicht bei allen Konnektivitätsanbietern zur Verfügung.
- Möglicherweise erhält Ihre Organisation direkten Zugriff auf landesweite Clouds – dies hängt vom Konnektivitätsanbieter ab.
- Es gilt für die gesamte Verbindung eine Verfügbarkeits-SLA von 99,9 %.

### <a name="challenges"></a>Herausforderungen

- Die Einrichtung kann sehr komplex sein. Zum Erstellen einer ExpressRoute-Verbindung müssen Sie mit einem Drittanbieter für die Konnektivität zusammenarbeiten. Der Anbieter ist für die Bereitstellung der Netzwerkverbindung verantwortlich.
- Die Architektur erfordert lokale Router mit hoher Bandbreite.

### <a name="reference-architecture"></a>Referenzarchitektur

- [Hybridnetzwerk mit ExpressRoute](./expressroute.md)

## <a name="expressroute-with-vpn-failover"></a>ExpressRoute mit VPN-Failover

Diese Option kombiniert die beiden vorherigen: Unter normalen Bedingungen wird eine ExpressRoute-Verbindung verwendet, aber im Fall eines Konnektivitätsverlusts in der ExpressRoute-Leitung erfolgt ein Failover auf eine VPN-Verbindung.

Diese Architektur eignet sich für hybride Anwendungen, die sowohl die höhere Bandbreite von ExpressRoute als auch Netzwerkkonnektivität mit hoher Verfügbarkeit benötigen.

### <a name="benefits"></a>Vorteile

- Bei einem Ausfall der ExpressRoute-Leitung ist eine hohe Verfügbarkeit gewährleistet, auch wenn die Fallbackverbindung über ein Netzwerk mit geringerer Bandbreite erfolgt.

### <a name="challenges"></a>Herausforderungen

- Die Konfiguration ist komplex. Sie müssen sowohl eine VPN-Verbindung als auch eine ExpressRoute-Leitung einrichten.
- Es sind redundante Hardwarekomponenten (VPN-Geräte) und eine redundante Azure VPN-Gatewayverbindung erforderlich, wofür Gebühren anfallen.

### <a name="reference-architecture"></a>Referenzarchitektur

- [Hybridnetzwerk mit ExpressRoute und VPN-Failover](./expressroute-vpn-failover.md)

<!-- markdownlint-disable MD024 -->

## <a name="hub-spoke-network-topology"></a>Hub-Spoke-Netzwerktopologie

Eine Hub-Spoke-Netzwerktopologie ist eine Möglichkeit zur Isolierung von Workloads während Dienste wie Identität und Sicherheit gemeinsam verwendet werden. Bei einem Hub handelt es sich um ein virtuelles Netzwerk (VNET) in Azure, das als zentraler Konnektivitätspunkt für Ihr lokales Netzwerk fungiert. Bei den Spokes handelt es sich um VNETs, die eine Peeringverbindung mit dem Hub herstellen. Gemeinsame Dienste werden im Hub bereitgestellt, während einzelne Workloads als Spokes bereitgestellt werden.

### <a name="reference-architectures"></a>Referenzarchitekturen

- [Hub-Spoke-Topologie](./hub-spoke.md)
- [Hub-Spoke mit gemeinsamen Diensten](./shared-services.md)
