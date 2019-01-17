---
title: Erstellen von sicheren Web-Apps mit Windows-VMs
titleSuffix: Azure Example Scenarios
description: Erstellen Sie mithilfe von Windows Server eine sichere Webanwendung mit mehreren Ebenen in Azure, für die Skalierungsgruppen, Application Gateway und Lastenausgleichsmodule verwendet werden.
author: iainfoulds
ms.date: 12/06/2018
ms.custom: seodec18
ms.openlocfilehash: c5c1d4468df72edd989f2ab8f303781b26d50017
ms.sourcegitcommit: 71ee0859e19fe58416b4c0056d67f2f34dd9ca0a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/11/2019
ms.locfileid: "54211987"
---
# <a name="building-secure-web-applications-with-windows-virtual-machines-on-azure"></a>Erstellen von sicheren Webanwendungen mit virtuellen Windows-Computern in Azure

Dieses Szenario fungiert als Architektur- und Entwurfsleitfaden für die Ausführung sicherer Webanwendungen mit mehreren Ebenen in Microsoft Azure. In diesem Beispiel stellt eine ASP.NET-Anwendung eine sichere Verbindung mit einem geschützten Microsoft SQL Server-Back-End-Cluster mit virtuellen Computern her.

Bisher mussten Organisationen lokale Legacyanwendungen und -dienste betreiben, um eine sichere Infrastruktur bereitzustellen. Durch die sichere Bereitstellung dieser Windows Server-Anwendungen in Azure können Organisationen ihre Bereitstellungen modernisieren und die lokalen Betriebskosten und den Verwaltungsaufwand reduzieren.

## <a name="relevant-use-cases"></a>Relevante Anwendungsfälle

Im Anschluss finden Sie einige Beispiele für dieses Szenario:

- Modernisieren von Anwendungsbereitstellungen in einer sicheren Cloudumgebung
- Verringern des Verwaltungsaufwands für lokale Legacyanwendungen und -dienste
- Verbessern der Gesundheitsversorgung für Patienten und der Benutzerfreundlichkeit neuer Anwendungsplattformen

## <a name="architecture"></a>Architecture

![Übersicht über die Architektur der Azure-Komponenten von Windows Server-Anwendungen mit mehreren Ebenen für Branchen mit Regulierung][architecture]

Dieses Szenario zeigt eine Front-End-Webanwendung, die eine Verbindung mit einer Back-End-Datenbank (beide unter Windows Server 2016) herstellt. Die Daten durchlaufen das Szenario wie folgt:

1. Benutzer greifen über eine Azure Application Gateway-Instanz auf die ASP.NET-Front-End-Anwendung zu.
2. Das Application Gateway verteilt Datenverkehr auf VM-Instanzen in einer Azure-VM-Skalierungsgruppe.
3. Die Anwendung stellt über einen Azure Load Balancer eine Verbindung mit dem Microsoft SQL Server-Cluster auf einer Back-End-Ebene her. Diese SQL Server-Back-End-Instanzen befinden sich in einem separaten virtuellen Azure-Netzwerk, die durch Netzwerksicherheitsgruppen-Regeln zur Beschränkung des Datenverkehrsflusses geschützt sind.
4. Das Lastenausgleichsmodul verteilt SQL Server-Datenverkehr auf VM-Instanzen in einer anderen VM-Skalierungsgruppe.
5. Azure Blob Storage fungiert als [Cloudzeuge][cloud-witness] für den SQL Server-Cluster auf der Back-End-Ebene. Die Verbindung aus dem VNET wird mit einem VNET-Dienstendpunkt für Azure Storage aktiviert.

### <a name="components"></a>Komponenten

- [Azure Application Gateway][appgateway-docs] ist ein Layer-7-Lastenausgleichsmodul für Webdatenverkehr, das anwendungsorientiert ist und Datenverkehr basierend auf spezifischen Routingregeln verteilen kann. Per App Gateway kann auch die SSL-Abladung zur Verbesserung der Webserverleistung verarbeitet werden.
- Mit [Azure Virtual Network][vnet-docs] können Ressourcen, z.B. VMs, auf sichere Weise miteinander, im Internet und mit lokalen Netzwerken kommunizieren. Virtuelle Netzwerke ermöglichen Isolation und Segmentierung, die Filterung und Weiterleitung von Datenverkehr und die Verbindungsherstellung zwischen Standorten. In diesem Szenario werden zwei virtuelle Netzwerke in Kombination mit den entsprechenden NSGs verwendet, um eine [demilitarisierte Zone][dmz] (DMZ) bereitzustellen und für die Isolation der Anwendungskomponenten zu sorgen. Per VNET-Peering werden die beiden Netzwerke verbunden.
- Mit [Azure-VM-Skalierungsgruppen][scaleset-docs] können Sie eine Gruppe von identischen virtuellen Computern mit Lastenausgleich erstellen und verwalten. Die Anzahl von VM-Instanzen kann automatisch erhöht oder verringert werden, wenn sich der Bedarf ändert, oder es kann ein Zeitplan festgelegt werden. In diesem Szenario werden zwei separate virtuelle VM-Skalierungsgruppen verwendet: eine für die ASP.NET-Front-End-Anwendungsinstanzen und eine für die VM-Instanzen des SQL Server-Back-End-Clusters. PowerShell DSC (Desired State Configuration, Konfiguration des gewünschten Zustands) oder die benutzerdefinierte Azure-Skripterweiterung können verwendet werden, um die VM-Instanzen mit der erforderlichen Software und den Konfigurationseinstellungen bereitzustellen.
- [Azure-Netzwerksicherheitsgruppen][nsg-docs] enthalten eine Liste mit Sicherheitsregeln, die ein- oder ausgehenden Netzwerkdatenverkehr basierend auf IP-Adresse, Port und Protokoll (für die Quelle bzw. das Ziel) zulassen oder ablehnen. Die virtuellen Netzwerke in diesem Szenario sind durch Netzwerksicherheitsgruppen-Regeln geschützt, mit denen der Datenverkehrsfluss zwischen den Anwendungskomponenten eingeschränkt wird.
- Per [Azure Load Balancer][loadbalancer-docs] wird der eingehende Datenverkehr gemäß den Regeln und Integritätstests verteilt. Ein Lastenausgleichsmodul sorgt für niedrige Latenzen und einen hohen Durchsatz und kann eine Skalierung auf Millionen von Datenflüssen für alle TCP- und UDP-Anwendungen durchführen. In diesem Szenario wird ein internes Lastenausgleichsmodul genutzt, um Datenverkehr von der Front-End-Anwendungsebene auf den SQL Server-Back-End-Cluster zu verteilen.
- [Azure Blob Storage][cloudwitness-docs] fungiert als Cloudzeugenstandort für den SQL Server-Cluster. Dieser Zeuge wird für Clustervorgänge und Entscheidungen verwendet, für die eine zusätzliche Abstimmung zum Treffen der Quorumentscheidung erforderlich ist. Bei der Nutzung von Cloudzeugen ist es nicht mehr erforderlich, dass eine zusätzliche VM als herkömmlicher Dateifreigabenzeuge fungiert.

### <a name="alternatives"></a>Alternativen

- Da die Infrastruktur nicht vom Betriebssystem abhängig ist, kann sowohl Linux als auch Windows verwendet werden.

- [SQL Server für Linux][sql-linux] kann den Back-End-Datenspeicher ersetzen.

- [Cosmos DB](/azure/cosmos-db/introduction) ist eine weitere Alternative für den Datenspeicher.

## <a name="considerations"></a>Überlegungen

### <a name="availability"></a>Verfügbarkeit

Die VM-Instanzen in diesem Szenario werden für [Verfügbarkeitszonen](/azure/availability-zones/az-overview) übergreifend bereitgestellt. Jede Zone besteht aus mindestens einem Rechenzentrum, dessen Stromversorgung, Kühlung und Netzwerkbetrieb unabhängig funktionieren. Jede aktivierte Region verfügt mindestens über drei Verfügbarkeitszonen. Diese zonenübergreifende Verteilung von VM-Instanzen sorgt für Hochverfügbarkeit auf den Anwendungsebenen.

Die Datenbankebene kann so konfiguriert werden, dass Always On-Verfügbarkeitsgruppen konfiguriert werden. Mit dieser SQL Server-Konfiguration wird eine primäre Datenbank in einem Cluster mit bis zu acht sekundären Datenbanken konfiguriert. Falls für die primäre Datenbank ein Problem besteht, führt der Cluster ein Failover auf eine der sekundären Datenbanken aus, damit die Anwendung weiterhin verfügbar ist. Weitere Informationen finden Sie unter [Übersicht über Always On-Verfügbarkeitsgruppen für SQL Server][sqlalwayson-docs].

Weitere Informationen zur Verfügbarkeit finden Sie im Azure Architecture Center in der [Checkliste für die Verfügbarkeit][availability].

### <a name="scalability"></a>Skalierbarkeit

In diesem Szenario werden VM-Skalierungsgruppen für die Front-End- und Back-End-Komponenten verwendet. Mit Skalierungsgruppen kann die Anzahl von VM-Instanzen, die auf der Front-End-Anwendungsebene ausgeführt werden, als Reaktion auf eine veränderte Kundennachfrage oder basierend auf einem definierten Zeitplan automatisch skaliert werden. Weitere Informationen finden Sie unter [Übersicht über die automatische Skalierung mit VM-Skalierungsgruppen][vmssautoscale-docs].

Weitere Skalierbarkeitsthemen finden Sie im Azure Architecture Center in der [Checkliste für die Skalierbarkeit][scalability].

### <a name="security"></a>Sicherheit

Der gesamte Datenverkehr von virtuellen Netzwerken fließt über die Front-End-Anwendungsebene und wird mit Netzwerksicherheitsgruppen geschützt. Anhand von Regeln wird der Datenverkehrsfluss eingeschränkt, sodass nur die VM-Instanzen der Front-End-Anwendungsebene auf die Back-End-Datenbankebene zugreifen können. Es ist kein ausgehender Internetdatenverkehr von der Datenbankebene zulässig. Es werden keine Ports für die direkte Remoteverwaltung geöffnet, um die Angriffsfläche zu reduzieren. Weitere Informationen finden Sie unter [Azure-Netzwerksicherheitsgruppen][nsg-docs].

Zeigen Sie die Anleitung zur Bereitstellung von PCI-DSS 3.2 (Payment Card Industry Data Security Standards) über eine [konforme Infrastruktur][pci-dss] an. Allgemeine Informationen zur Entwicklung sicherer Szenarien finden Sie in der [Dokumentation zur Azure-Sicherheit][security].

### <a name="resiliency"></a>Resilienz

In Kombination mit der Nutzung von Verfügbarkeitszonen und VM-Skalierungsgruppen werden in diesem Szenario Azure Application Gateway und ein Lastenausgleichsmodul verwendet. Über diese beiden Netzwerkkomponenten wird Datenverkehr auf die verbundenen VM-Instanzen verteilt, und es werden Integritätstests eingebunden, mit denen sichergestellt wird, dass Datenverkehr nur auf fehlerfreie VMs verteilt wird. Zwei Application Gateway-Instanzen werden in einer Aktiv-Passiv-Konfiguration konfiguriert, und es wird ein zonenredundantes Lastenausgleichsmodul verwendet. Mit dieser Konfiguration verfügen die Netzwerkressourcen und die Anwendung über Resilienz gegenüber Problemen, die andernfalls zu einer Störung des Datenverkehrs und zu Auswirkungen auf den Zugriff durch Endbenutzer führen würden.

Allgemeine Informationen zur Entwicklung robuster Szenarien finden Sie unter [Entwerfen robuster Anwendungen für Azure][resiliency].

## <a name="deploy-the-scenario"></a>Bereitstellen des Szenarios

### <a name="prerequisites"></a>Voraussetzungen

- Sie benötigen ein bestehendes Azure-Konto. Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) erstellen, bevor Sie beginnen.

- Sie benötigen eine Domäne in Azure Active Directory (AD) Domain Services, um einen SQL Server-Cluster in der Back-End-Skalierungsgruppe bereitstellen zu können.

### <a name="deploy-the-components"></a>Bereitstellen der Komponenten

Führen Sie die unten angegebenen Schritte aus, um die Kerninfrastruktur für dieses Szenario mit einer Azure Resource Manager-Vorlage bereitzustellen.

<!-- markdownlint-disable MD033 -->

1. Wählen Sie die Schaltfläche **Deploy to Azure** (In Azure bereitstellen):<br><a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2Fmspnp%2Fsolution-architectures%2Fmaster%2Finfrastructure%2Fregulated-multitier-app.json" target="_blank"><img src="https://azuredeploy.net/deploybutton.png"/></a>
2. Warten Sie, bis die Vorlagenbereitstellung im Azure-Portal geöffnet wurde, und führen Sie anschließend folgende Schritte aus:
   - Erstellen Sie über **Neu erstellen** eine neue Ressourcengruppe, und geben Sie einen Namen (beispielsweise *myWindowsscenario*) in das Textfeld ein.
   - Wählen Sie im Dropdownfeld **Standort** eine Region aus.
   - Geben Sie einen Benutzernamen und ein sicheres Kennwort für die VM-Skalierungsgruppeninstanzen an.
   - Lesen Sie die allgemeinen Geschäftsbedingungen, und aktivieren Sie dann das Kontrollkästchen **Ich stimme den oben genannten Geschäftsbedingungen zu**.
   - Klicken Sie auf die Schaltfläche **Kaufen**.

<!-- markdownlint-enable MD033 -->

Der Bereitstellungsvorgang kann zwischen 15 und 20 Minuten dauern.

## <a name="pricing"></a>Preise

Zur Ermittlung der Betriebskosten für dieses Szenario sind alle Dienste im Kostenrechner vorkonfiguriert. Wenn Sie wissen möchten, welche Kosten für Ihren spezifischen Anwendungsfall entstehen, passen Sie die entsprechenden Variablen an Ihren voraussichtlichen Datenverkehr an.

Wir haben basierend auf der Anzahl von VM-Instanzen, über die Ihre Anwendungen ausgeführt werden, drei Beispielkostenprofile bereitgestellt.

- [Klein][small-pricing]: Dieses Preisbeispiel entspricht zwei Front-End- und zwei Back-End-VM-Instanzen.
- [Mittel][medium-pricing]: Dieses Preisbeispiel entspricht 20 Front-End- und fünf Back-End-VM-Instanzen.
- [Groß][large-pricing]: Dieses Preisbeispiel entspricht 100 Front-End- und 10 Back-End-VM-Instanzen.

## <a name="related-resources"></a>Zugehörige Ressourcen

In diesem Szenario wurde eine VM-Back-End-Skalierungsgruppe verwendet, über die ein Microsoft SQL Server-Cluster ausgeführt wird. Cosmos DB kann auch als skalierbare und sichere Datenbankebene für die Anwendungsdaten verwendet werden. Ein [Azure-VNET-Dienstendpunkt][vnetendpoint-docs] ermöglicht es Ihnen, Ihre kritischen Azure-Dienstressourcen auf Ihre virtuellen Netzwerke zu beschränken und so zu schützen. In diesem Szenario können Sie mit VNET-Endpunkten Datenverkehr zwischen der Front-End-Anwendungsebene und Cosmos DB schützen. Weitere Informationen finden Sie in der [Übersicht zu Azure Cosmos DB](/azure/cosmos-db/introduction).

Ausführlichere Informationen zur Implementierung finden Sie in der [Referenzarchitektur für n-schichtige Anwendungen mit SQL Server][ntiersql-ra].

<!-- links -->
[appgateway-docs]: /azure/application-gateway/overview
[architecture]: ./media/architecture-regulated-multitier-app.png
[autoscaling]: /azure/architecture/best-practices/auto-scaling
[availability]: ../../checklist/availability.md
[cloudwitness-docs]: /windows-server/failover-clustering/deploy-cloud-witness
[loadbalancer-docs]: /azure/load-balancer/load-balancer-overview
[nsg-docs]: /azure/virtual-network/security-overview
[ntiersql-ra]: /azure/architecture/reference-architectures/n-tier/n-tier-sql-server
[resiliency]: /azure/architecture/resiliency/
[security]: /azure/security/
[scalability]: /azure/architecture/checklist/scalability
[scaleset-docs]: /azure/virtual-machine-scale-sets/overview
[sqlalwayson-docs]: /sql/database-engine/availability-groups/windows/overview-of-always-on-availability-groups-sql-server
[vmssautoscale-docs]: /azure/virtual-machine-scale-sets/virtual-machine-scale-sets-autoscale-overview
[vnet-docs]: /azure/virtual-network/virtual-networks-overview
[vnetendpoint-docs]: /azure/virtual-network/virtual-network-service-endpoints-overview
[pci-dss]: /azure/security/blueprints/pcidss-iaaswa-overview
[dmz]: /azure/virtual-network/virtual-networks-dmz-nsg
[sql-linux]: /sql/linux/sql-server-linux-overview?view=sql-server-linux-2017
[cloud-witness]: /windows-server/failover-clustering/deploy-cloud-witness
[small-pricing]: https://azure.com/e/711bbfcbbc884ef8aa91cdf0f2caff72
[medium-pricing]: https://azure.com/e/b622d82d79b34b8398c4bce35477856f
[large-pricing]: https://azure.com/e/1d99d8b92f90496787abecffa1473a93
