---
title: Virtuelle Linux-Desktops mit Citrix
titleSuffix: Azure Example Scenarios
description: Erstellen Sie mithilfe von Citrix eine VDI-Umgebung für Linux-Desktops in Azure.
author: miguelangelopereira
ms.date: 09/12/2018
ms.topic: example-scenario
ms.service: architecture-center
ms.subservice: example-scenario
ms.custom: fasttrack, Linux
social_image_url: /azure/architecture/example-scenario/infrastructure/media/azure-citrix-sample-diagram.png
ms.openlocfilehash: e59e785df80efc9de134d27664ba1581a61dd1d1
ms.sourcegitcommit: 579c39ff4b776704ead17a006bf24cd4cdc65edd
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/17/2019
ms.locfileid: "59640276"
---
# <a name="linux-virtual-desktops-with-citrix"></a>Virtuelle Linux-Desktops mit Citrix

Dieses Beispielszenario gilt für jede Branche, in der eine VDI-Umgebung (Virtual Desktop Infrastructure) für Linux-Desktops benötigt wird. VDI bezieht sich auf die Ausführung eines Benutzerdesktops innerhalb eines virtuellen Computers, der sich auf einem Server im Datencenter befindet. Der Kunde in diesem Szenario möchte eine Citrix-basierte Lösung für seine VDI-Anforderungen verwenden.

Organisationen verfügen häufig über heterogene Umgebungen, in denen Mitarbeiter mehrere Geräte und Betriebssysteme verwenden. In einer solchen Umgebung ist es unter Umständen nicht immer einfach, konsistenten Zugriff auf Anwendungen zu bieten und gleichzeitig die Sicherheit der Umgebung zu gewährleisten. Mit einer VDI-Lösung für Linux-Desktops kann Ihre Organisation den Zugriff unabhängig davon ermöglichen, welches Gerät oder Betriebssystem der Endbenutzer verwendet.

Hier sind einige Vorteile dieses Szenarios aufgeführt:

- Die gemeinsame Nutzung virtueller Linux-Desktops sorgt für eine höhere Rendite, da mehr Benutzer Zugriff auf die gleiche Infrastruktur haben. Durch die Konsolidierung von Ressourcen in einer zentralisierten VDI-Umgebung müssen die Geräte der Benutzer weniger leistungsfähig sein.
- Die Leistung ist unabhängig vom Gerät des Endbenutzers konsistent.
- Benutzer können von jedem Gerät aus auf Linux-Anwendungen zugreifen (auch von Linux-fremden Geräten).
- Vertrauliche Daten können im Azure-Rechenzentrum für alle verteilten Mitarbeiter geschützt werden.

## <a name="relevant-use-cases"></a>Relevante Anwendungsfälle

Dieses Szenario eignet sich für den folgenden Anwendungsfall:

- Bereitstellen von sicherem Zugriff auf geschäftskritische spezialisierte Linux-VDI-Desktops über Linux-basierte und Linux-fremde Geräte

## <a name="architecture"></a>Architecture

[![](./media/azure-citrix-sample-diagram.png "Architekturdiagramm")](./media/azure-citrix-sample-diagram.png#lightbox)

Dieses Beispielszenario veranschaulicht, wie Sie dem Unternehmensnetzwerk Zugriff auf die virtuellen Linux-Desktops gewähren:

- Zwischen der lokalen Umgebung und Azure wird eine ExpressRoute-Verbindung hergestellt, um eine schnelle und zuverlässige Cloudverbindung zu erhalten.
- Für VDI wird die Citrix XenDesktop-Lösung bereitgestellt.
- CitrixVDA wird unter Ubuntu (oder einer anderen unterstützten Distribution) ausgeführt.
- Azure-Netzwerksicherheitsgruppen wenden die korrekten Netzwerk-ACLs an.
- Citrix ADC (NetScaler) übernimmt die Veröffentlichung und den Lastenausgleich für alle Citrix-Dienste.
- Active Directory Domain Services wird verwendet, um die Citrix-Server in die Domäne einzubinden. VDA-Server werden nicht in die Domäne eingebunden.
- Die hybride Azure-Dateisynchronisierung ermöglicht die Verwendung von gemeinsam genutztem Speicher innerhalb der gesamten Lösung. So ist beispielsweise die Verwendung in Remote-/Privatlösungen möglich.

Für dieses Szenario werden folgende SKUs verwendet:

- Citrix ADC (NetScaler): 2x D4s v3 mit [NetScaler 12.0 VPX Standard Edition (200 MBit/s, nutzungsbasiertes Image)](https://azuremarketplace.microsoft.com/pt-br/marketplace/apps/citrix.netscalervpx-120?tab=PlansAndPrice)
- Citrix License Server: 1x D2s v3
- Citrix VDA: 4x D8s v3
- Citrix Storefront: 2x D2s v3
- Citrix Delivery Controller: 2x D2s v3
- Domänencontroller: 2x D2sv3
- Azure-Dateiserver: 2x D2sv3

> [!NOTE]
> Alle Lizenzen (mit Ausnahme von NetScaler) sind BYOL-Lizenzen (Bring-Your-Own-License).

### <a name="components"></a>Komponenten

- Mit [Azure Virtual Network](/azure/virtual-network/virtual-networks-overview) können Ressourcen (beispielsweise virtuelle Computer) sicher untereinander sowie mit dem Internet und mit lokalen Netzwerken kommunizieren. Virtuelle Netzwerke ermöglichen Isolation und Segmentierung, die Filterung und Weiterleitung von Datenverkehr und die Verbindungsherstellung zwischen Standorten. In diesem Szenario wird für alle Ressourcen ein einzelnes virtuelles Netzwerk verwendet.
- [Azure-Netzwerksicherheitsgruppen](/azure/virtual-network/security-overview) enthalten eine Liste mit Sicherheitsregeln, die ein- oder ausgehenden Netzwerkdatenverkehr basierend auf IP-Adresse, Port und Protokoll (für die Quelle bzw. das Ziel) zulassen oder ablehnen. Die virtuellen Netzwerke in diesem Szenario sind durch Netzwerksicherheitsgruppen-Regeln geschützt, mit denen der Datenverkehrsfluss zwischen den Anwendungskomponenten eingeschränkt wird.
- [Azure Load Balancer](/azure/application-gateway/overview) verteilt eingehenden Datenverkehr auf der Grundlage von Regeln und Integritätstests. Ein Lastenausgleichsmodul sorgt für niedrige Latenzen und einen hohen Durchsatz und kann eine Skalierung auf Millionen von Datenflüssen für alle TCP- und UDP-Anwendungen durchführen. In diesem Szenario wird ein interner Lastenausgleich verwendet, um Datenverkehr für Citrix NetScaler zu verteilen.
- Die [hybride Azure-Dateisynchronisierung](https://github.com/MicrosoftDocs/azure-docs/edit/master/articles/storage/files/storage-sync-files-planning.md) wird für den gesamten gemeinsam genutzten Speicher verwendet. Der Speicher wird mithilfe der hybriden Dateisynchronisierung auf zwei Dateiservern repliziert.
- [Azure SQL-Datenbank](/azure/sql-database/sql-database-technical-overview) ist eine relationale DBaaS-Lösung (Database-as-a-Service) und basiert auf der neuesten stabilen Version der Microsoft SQL Server-Datenbank-Engine. Sie wird zum Hosten von Citrix-Datenbanken verwendet.
- Mit [ExpressRoute](/azure/expressroute/expressroute-introduction) können Sie Ihre lokalen Netzwerke über eine private Verbindung, die von einem Konnektivitätsanbieter bereitgestellt wird, auf die Cloud von Microsoft ausdehnen.
- Active Directory Domain Services wird für Verzeichnisdienste und für die Benutzerauthentifizierung verwendet.
- [Azure-Verfügbarkeitsgruppen](/azure/virtual-machines/windows/tutorial-availability-sets) sorgen dafür, dass die von Ihnen in Azure bereitgestellten virtuellen Computer auf mehrere isolierte Hardwareknoten in einem Cluster verteilt werden. Dadurch wird sichergestellt, dass sich Hardware- oder Softwarefehler in Azure nur auf einen Teil Ihrer VMs auswirken und die Lösung insgesamt verfügbar und betriebsbereit bleibt.
- [Citrix ADC (NetScaler)](https://www.citrix.com/products/citrix-adc) ist ein Controller zur Anwendungsbereitstellung (Application Delivery Controller), der anwendungsspezifische Datenverkehrsanalysen durchführt, um Netzwerkdatenverkehr (Layer 4 bis Layer 7) für Webanwendungen intelligent zu verteilen, zu optimieren und zu schützen.
- [Citrix StoreFront](https://www.citrix.com/products/citrix-virtual-apps-and-desktops/citrix-storefront.html) ist ein Store für Unternehmens-Apps, der die Sicherheit verbessert und Bereitstellungen vereinfacht. Er verfügt über eine moderne, einzigartige und nahezu native Benutzeroberfläche für Citrix Receiver auf jeder Plattform. StoreFront erleichtert die Verwaltung von Umgebungen mit Citrix Virtual Apps und Desktops für mehrere Standorte und Versionen.
- [Citrix License Server](https://www.citrix.com/buy/licensing/overview.html) verwaltet die Lizenzen für Citrix-Produkte.
- [Citrix XenDesktops VDA](https://docs.citrix.com/en-us/citrix-virtual-apps-desktops-service) ermöglicht Verbindungen mit Anwendungen und Desktops. Der VDA wird auf dem Computer installiert, der die Anwendungen oder virtuellen Desktops für den Benutzer ausführt. Er ermöglicht es dem Computer, sich bei Bereitstellungscontrollern zu registrieren und die HDX-Verbindung (High Definition eXperience) mit einem Benutzergerät zu verwalten.
- [Citrix Delivery Controller](https://docs.citrix.com/en-us/xenapp-and-xendesktop/7-15-ltsr/manage-deployment/delivery-controllers) ist die serverseitige Komponente für die Verwaltung des Benutzerzugriffs sowie für die Aushandlung und Optimierung von Verbindungen. Controller stellen auch die Computererstellungsdienste für die Erstellung von Desktop- und Serverimages bereit.

### <a name="alternatives"></a>Alternativen

- Es gibt mehrere Partner mit VDI-Lösungen, die in Azure unterstützt werden. Hierzu zählen beispielsweise VMware und Workspot. Diese spezielle Beispielarchitektur basiert auf einem bereitgestellten Projekt, das Citrix verwendet.
- Citrix bietet einen Clouddienst, der einen Teil dieser Architektur abstrahiert. Somit ist es eine mögliche Alternative für diese Lösung. Weitere Informationen finden Sie unter [Citrix Cloud](https://www.citrix.com/products/citrix-cloud).

## <a name="considerations"></a>Überlegungen

- Machen Sie sich mit den [Linux-Anforderungen von Citrix](https://docs.citrix.com/en-us/linux-virtual-delivery-agent/current-release/system-requirements) vertraut.
- Die Wartezeit kann sich auf die gesamte Lösung auswirken. Führen Sie für eine Produktionsumgebung entsprechende Tests durch.
- Je nach Szenario sind für die Lösung unter Umständen virtuelle Computer mit GPUs für den VDA erforderlich. Bei der vorliegenden Lösung wird davon ausgegangen, dass keine GPU benötigt wird.

### <a name="availability-scalability-and-security"></a>Verfügbarkeit, Skalierbarkeit und Sicherheit

- Dieses Beispiel ist für Hochverfügbarkeit für alle Rollen (mit Ausnahme des Lizenzservers) konzipiert. Da die Umgebung noch 30 Tage funktioniert, auch wenn der Lizenzserver offline ist, ist auf diesem Server keine zusätzliche Redundanz erforderlich.
- Alle Server, die ähnliche Rollen bereitstellen, müssen in [Verfügbarkeitsgruppen](/azure/virtual-machines/windows/manage-availability#configure-multiple-virtual-machines-in-an-availability-set-for-redundancy) bereitgestellt werden.
- Dieses Beispielszenario enthält keine Funktionen für die Notfallwiederherstellung. [Azure Site Recovery](/azure/site-recovery/site-recovery-overview) wäre ggf. eine gute Ergänzung für diesen Entwurf.
- Erwägen Sie, die VM-Instanzen in diesem Szenario übergreifend für [Verfügbarkeitszonen](/azure/availability-zones/az-overview) bereitzustellen. Jede Verfügbarkeitszone besteht aus mindestens einem Rechenzentrum, dessen Stromversorgung, Kühlung und Netzwerkbetrieb unabhängig funktionieren. Jede aktivierte Region verfügt mindestens über drei Verfügbarkeitszonen. Diese zonenübergreifende Verteilung von VM-Instanzen sorgt für Hochverfügbarkeit auf den Anwendungsebenen. Weitere Informationen finden Sie unter [Was sind Verfügbarkeitszonen in Azure?](/azure/availability-zones/az-overview). Sie können auch [VPN- und ExpressRoute-Gateways in Azure-Verfügbarkeitszonen bereitstellen](/azure/vpn-gateway/about-zone-redundant-vnet-gateways).
- Für eine Bereitstellungsverwaltungslösung in einer Produktionsumgebung sollten Funktionen wie [Sicherung](/azure/backup/backup-introduction-to-azure-backup), [Überwachung](/azure/monitoring-and-diagnostics/monitoring-overview) und [Updateverwaltung](/azure/automation/automation-update-management) implementiert werden.
- Dieses Beispiel eignet sich für etwa 250 gleichzeitige Benutzer (ca. 50 bis 60 pro VDA-Server) mit gemischter Verwendung. Dies hängt jedoch stark von der Art der verwendeten Anwendungen ab. Für Produktionsumgebungen werden ausgiebige Auslastungstests empfohlen.

## <a name="deployment"></a>Bereitstellung

Informationen zur Bereitstellung finden Sie in der offiziellen [Citrix-Dokumentation](https://docs.citrix.com/en-us/citrix-virtual-apps-desktops/install-configure.html).

## <a name="pricing"></a>Preise

- Die Citrix XenDesktop-Lizenzen sind in den Gebühren für Azure-Dienste nicht enthalten.
- Die Citrix NetScaler-Lizenz ist in einem Modell mit nutzungsbasierter Bezahlung enthalten.
- Durch die Nutzung reservierter Instanzen verringern sich die Computekosten für die Lösung erheblich.
- ExpressRoute-Kosten sind nicht enthalten.

## <a name="next-steps"></a>Nächste Schritte

- Informationen zur Planung und Bereitstellung finden Sie [hier](https://docs.citrix.com/en-us/citrix-virtual-apps-desktops/install-configure) in der Citrix-Dokumentation.
- Die von Citrix bereitgestellten Resource Manager-Vorlagen zur Bereitstellung von Citrix ADC (NetScaler) in Azure finden Sie [hier](https://github.com/citrix/netscaler-azure-templates).
