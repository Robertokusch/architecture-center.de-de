---
title: Schützen von Windows-Webanwendungen für Branchen mit Regulierung
description: Bewährtes Szenario für die Erstellung einer sicheren Webanwendung mit mehreren Ebenen mithilfe von Windows Server in Azure, für die Skalierungsgruppen, Application Gateway und Lastenausgleichsmodule verwendet werden.
author: iainfoulds
ms.date: 07/11/2018
ms.openlocfilehash: 3572f215d9134a6650d76e1b14458226334c6f42
ms.sourcegitcommit: c49aeef818d7dfe271bc4128b230cfc676f05230
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/11/2018
ms.locfileid: "44389277"
---
# <a name="secure-windows-web-application-for-regulated-industries"></a>Schützen von Windows-Webanwendungen für Branchen mit Regulierung

Dieses Beispielszenario gilt für Branchen mit Regulierung, in denen Anwendungen mit mehreren Ebenen geschützt werden müssen. In diesem Szenario stellt eine Front-End-ASP.NET-Anwendung eine sichere Verbindung mit einem geschützten Microsoft SQL Server-Back-End-Cluster her.

Zu den Beispielszenarien für Anwendungen gehören die Ausführung von Anwendungen für Operationssäle, Patiententermine und -aufzeichnungen oder Medikamentenabholung und -bestellung auf Rezeptbasis. Bisher mussten Organisationen für diese Zwecke lokale Legacyanwendungen und -dienste betreiben. Da diese Windows Server-Anwendungen auf sichere und skalierbare Weise in Azure bereitgestellt werden können, können Organisationen ihre Bereitstellungen modernisieren und die lokalen Betriebskosten und den Verwaltungsaufwand reduzieren.

## <a name="related-use-cases"></a>Verwandte Anwendungsfälle

Erwägen Sie dieses Szenario für folgende Anwendungsfälle:

* Modernisieren von Anwendungsbereitstellungen in einer sicheren Cloudumgebung
* Reduzieren der Verwaltung von lokalen Legacyanwendungen und -diensten
* Verbessern der Gesundheitsversorgung für Patienten und der Benutzerfreundlichkeit neuer Anwendungsplattformen

## <a name="architecture"></a>Architecture

![Übersicht über die Architektur der Azure-Komponenten von Windows Server-Anwendungen mit mehreren Ebenen für Branchen mit Regulierung][architecture]

In diesem Szenario geht es um eine Anwendung mit mehreren Ebenen für Branchen mit Regulierung, für die ASP.NET und Microsoft SQL Server verwendet werden. Die Daten durchlaufen das Szenario wie folgt:

1. Benutzer greifen auf die ASP.NET-Front-End-Anwendung für Branchen mit Regulierung über ein Azure Application Gateway zu.
2. Das Application Gateway verteilt Datenverkehr auf VM-Instanzen in einer Azure-VM-Skalierungsgruppe.
3. Die ASP.NET-Anwendung stellt eine Verbindung mit dem Microsoft SQL Server-Cluster auf einer Back-End-Ebene über einen Azure Load Balancer her. Diese SQL Server-Back-End-Instanzen befinden sich in einem separaten virtuellen Azure-Netzwerk, die durch Netzwerksicherheitsgruppen-Regeln zur Beschränkung des Datenverkehrsflusses geschützt sind.
4. Das Lastenausgleichsmodul verteilt SQL Server-Datenverkehr auf VM-Instanzen in einer anderen VM-Skalierungsgruppe.
5. Azure Blob Storage dient als Cloudzeuge für den SQL Server-Cluster auf der Back-End-Ebene.  Die Verbindung aus dem VNET wird mit einem VNET-Dienstendpunkt für Azure Storage aktiviert.

### <a name="components"></a>Komponenten

* [Azure Application Gateway][appgateway-docs] ist ein Layer-7-Lastenausgleichsmodul für Webdatenverkehr, das anwendungsorientiert ist und Datenverkehr basierend auf spezifischen Routingregeln verteilen kann. Per App Gateway kann auch die SSL-Abladung zur Verbesserung der Webserverleistung verarbeitet werden.
* Mit [Azure Virtual Network][vnet-docs] können Ressourcen, z.B. VMs, auf sichere Weise miteinander, im Internet und mit lokalen Netzwerken kommunizieren. Virtuelle Netzwerke ermöglichen Isolation und Segmentierung, die Filterung und Weiterleitung von Datenverkehr und die Verbindungsherstellung zwischen Standorten. In diesem Szenario werden zwei virtuelle Netzwerke in Kombination mit den entsprechenden NSGs verwendet, um eine [demilitarisierte Zone][dmz] (DMZ) bereitzustellen und für die Isolation der Anwendungskomponenten zu sorgen. Per VNET-Peering werden die beiden Netzwerke verbunden.
* Mit [Azure-VM-Skalierungsgruppen][scaleset-docs] können Sie eine Gruppe von identischen virtuellen Computern mit Lastenausgleich erstellen und verwalten. Die Anzahl von VM-Instanzen kann automatisch erhöht oder verringert werden, wenn sich der Bedarf ändert, oder es kann ein Zeitplan festgelegt werden. In diesem Szenario werden zwei separate virtuelle VM-Skalierungsgruppen verwendet: eine für die ASP.NET-Front-End-Anwendungsinstanzen und eine für die VM-Instanzen des SQL Server-Back-End-Clusters. PowerShell DSC (Desired State Configuration, Konfiguration des gewünschten Zustands) oder die benutzerdefinierte Azure-Skripterweiterung können verwendet werden, um die VM-Instanzen mit der erforderlichen Software und den Konfigurationseinstellungen bereitzustellen.
* [Azure-Netzwerksicherheitsgruppen][nsg-docs] enthalten eine Liste mit Sicherheitsregeln, die ein- oder ausgehenden Netzwerkdatenverkehr basierend auf IP-Adresse, Port und Protokoll (für die Quelle bzw. das Ziel) zulassen oder ablehnen. Die virtuellen Netzwerke in diesem Szenario sind durch Netzwerksicherheitsgruppen-Regeln geschützt, mit denen der Datenverkehrsfluss zwischen den Anwendungskomponenten eingeschränkt wird.
* Per [Azure Load Balancer][loadbalancer-docs] wird der eingehende Datenverkehr gemäß den Regeln und Integritätstests verteilt. Ein Lastenausgleichsmodul sorgt für niedrige Latenzen und einen hohen Durchsatz und kann eine Skalierung auf Millionen von Datenflüssen für alle TCP- und UDP-Anwendungen durchführen. In diesem Szenario wird ein internes Lastenausgleichsmodul genutzt, um Datenverkehr von der Front-End-Anwendungsebene auf den SQL Server-Back-End-Cluster zu verteilen.
* [Azure Blob Storage][cloudwitness-docs] fungiert als Cloudzeugenstandort für den SQL Server-Cluster. Dieser Zeuge wird für Clustervorgänge und Entscheidungen verwendet, für die eine zusätzliche Abstimmung zum Treffen der Quorumentscheidung erforderlich ist. Bei der Nutzung von Cloudzeugen ist es nicht mehr erforderlich, dass eine zusätzliche VM als herkömmlicher Dateifreigabenzeuge fungiert.

### <a name="alternatives"></a>Alternativen

* *nix/Windows kann problemlos durch verschiedene andere Betriebssysteme ersetzt werden, da die Infrastruktur nicht vom Betriebssystem abhängig ist.

* [SQL Server für Linux][sql-linux] kann den Back-End-Datenspeicher ersetzen.

* [Cosmos DB][cosmos] ist eine weitere Alternative für den Datenspeicher.

## <a name="considerations"></a>Überlegungen

### <a name="availability"></a>Verfügbarkeit

Die VM-Instanzen in diesem Szenario werden für Verfügbarkeitszonen übergreifend bereitgestellt. Jede Zone besteht aus mindestens einem Rechenzentrum, dessen Stromversorgung, Kühlung und Netzwerkbetrieb unabhängig funktionieren. Mindestens drei Zonen sind in allen aktivierten Regionen verfügbar. Diese zonenübergreifende Verteilung von VM-Instanzen sorgt für Hochverfügbarkeit auf den Anwendungsebenen. Weitere Informationen finden Sie unter [Was sind Verfügbarkeitszonen in Azure?][azureaz-docs].

Die Datenbankebene kann so konfiguriert werden, dass Always On-Verfügbarkeitsgruppen konfiguriert werden. Mit dieser SQL Server-Konfiguration wird eine primäre Datenbank in einem Cluster mit bis zu acht sekundären Datenbanken konfiguriert. Falls für die primäre Datenbank ein Problem besteht, führt der Cluster ein Failover auf eine der sekundären Datenbanken aus, damit die Anwendung weiterhin verfügbar ist. Weitere Informationen finden Sie unter [Übersicht über Always On-Verfügbarkeitsgruppen für SQL Server][sqlalwayson-docs].

Weitere Verfügbarkeitsthemen finden Sie im Azure Architecture Center in der [Checkliste für die Verfügbarkeit][availability].

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

**Voraussetzungen:**

* Sie benötigen ein bestehendes Azure-Konto. Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) erstellen, bevor Sie beginnen.
* Sie benötigen eine Domäne in Azure Active Directory (AD) Domain Services, um einen SQL Server-Cluster in der Back-End-Skalierungsgruppe bereitstellen zu können.

Führen Sie die unten angegebenen Schritte aus, um die Kerninfrastruktur für dieses Szenario mit einer Azure Resource Manager-Vorlage bereitzustellen.

1. Wählen Sie die Schaltfläche **Deploy to Azure** (In Azure bereitstellen):<br><a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2Fmspnp%2Fsolution-architectures%2Fmaster%2Finfrastructure%2Fregulated-multitier-app%2Fazuredeploy.json" target="_blank"><img src="http://azuredeploy.net/deploybutton.png"/></a>
2. Warten Sie, bis die Vorlagenbereitstellung im Azure-Portal geöffnet wurde, und führen Sie anschließend folgende Schritte aus:
   * Erstellen Sie über **Neu erstellen** eine neue Ressourcengruppe, und geben Sie einen Namen (beispielsweise *myWindowsscenario*) in das Textfeld ein.
   * Wählen Sie im Dropdownfeld **Standort** eine Region aus.
   * Geben Sie einen Benutzernamen und ein sicheres Kennwort für die VM-Skalierungsgruppeninstanzen an.
   * Lesen Sie die allgemeinen Geschäftsbedingungen, und aktivieren Sie dann das Kontrollkästchen **Ich stimme den oben genannten Geschäftsbedingungen zu**.
   * Klicken Sie auf die Schaltfläche **Kaufen**.

Der Bereitstellungsvorgang kann zwischen 15 und 20 Minuten dauern.

## <a name="pricing"></a>Preise

Zur Ermittlung der Betriebskosten für dieses Szenario sind alle Dienste im Kostenrechner vorkonfiguriert.  Wenn Sie wissen möchten, welche Kosten für Ihren spezifischen Anwendungsfall entstehen, passen Sie die entsprechenden Variablen an Ihren voraussichtlichen Datenverkehr an.

Wir haben basierend auf der Anzahl von VM-Instanzen, über die Ihre Anwendungen ausgeführt werden, drei Beispielkostenprofile bereitgestellt.

* [Klein][small-pricing]: Dieses Preisbeispiel entspricht zwei Front-End- und zwei Back-End-VM-Instanzen.
* [Mittel][medium-pricing]: Dieses Preisbeispiel entspricht 20 Front-End- und fünf Back-End-VM-Instanzen.
* [Groß][large-pricing]: Dieses Preisbeispiel entspricht 100 Front-End- und 10 Back-End-VM-Instanzen.

## <a name="related-resources"></a>Zugehörige Ressourcen

In diesem Szenario wurde eine VM-Back-End-Skalierungsgruppe verwendet, über die ein Microsoft SQL Server-Cluster ausgeführt wird. Azure Cosmos DB kann auch als skalierbare und sichere Datenbankebene für die Anwendungsdaten verwendet werden. Ein [Azure-VNET-Dienstendpunkt][vnetendpoint-docs] ermöglicht es Ihnen, Ihre kritischen Azure-Dienstressourcen auf Ihre virtuellen Netzwerke zu beschränken und so zu schützen. In diesem Szenario können Sie mit VNET-Endpunkten Datenverkehr zwischen der Front-End-Anwendungsebene und Cosmos DB schützen. Weitere Informationen zu Cosmos DB finden Sie unter [Übersicht über Azure Cosmos DB][azurecosmosdb-docs].

Sie können auch eine eingehende [Referenzarchitektur für eine generische Anwendung mit n-Schichten mit SQL Server][ntiersql-ra] anzeigen.

<!-- links -->
[appgateway-docs]: /azure/application-gateway/overview
[architecture]: ./media/regulated-multitier-app/architecture-regulated-multitier-app.png
[autoscaling]: /azure/architecture/best-practices/auto-scaling
[availability]: /architecture/checklist/availability
[azureaz-docs]: /azure/availability-zones/az-overview
[azurecosmosdb-docs]: /azure/cosmos-db/introduction
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
[cosmos]: /azure/cosmos-db/
[sql-linux]: /sql/linux/sql-server-linux-overview?view=sql-server-linux-2017

[small-pricing]: https://azure.com/e/711bbfcbbc884ef8aa91cdf0f2caff72
[medium-pricing]: https://azure.com/e/b622d82d79b34b8398c4bce35477856f
[large-pricing]: https://azure.com/e/1d99d8b92f90496787abecffa1473a93