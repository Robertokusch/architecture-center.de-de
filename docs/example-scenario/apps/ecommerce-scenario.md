---
title: E-Commerce-Front-End
titleSuffix: Azure Example Scenarios
description: Hosten Sie eine E-Commerce-Website in Azure.
author: masonch
ms.date: 07/13/2018
ms.custom: fasttrack
ms.openlocfilehash: f07c21b8eb9d812b9831abe8f2e4f6d131893df2
ms.sourcegitcommit: 7d9efe716e8c9e99f3fafa9d0213d48c23d9713d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/09/2019
ms.locfileid: "54160807"
---
# <a name="an-e-commerce-front-end-on-azure"></a>Ein E-Commerce-Front-End in Azure

In diesem Beispielszenario wird die Implementierung eines E-Commerce-Front-Ends mit Azure-PaaS-Tools (Platform-as-a-Service) Schritt für Schritt beschrieben. Für viele E-Commerce-Websites müssen Anforderungen in Bezug auf die Saisonalität und die Variabilität des Datenverkehrs erfüllt werden. Wenn die Nachfrage nach Ihren Produkten oder Diensten steigt (vorhersehbar oder unvorhersehbar), können Sie mit PaaS-Tools automatisch eine größere Anzahl von Kunden verwalten und mehr Transaktionen durchführen. Darüber hinaus kommt Ihnen bei diesem Szenario der Cloudvorteil zugute, indem Sie nur für die genutzte Kapazität zahlen.

In diesem Dokument werden verschiedene Azure-PaaS-Komponenten und -Aspekte beschrieben, die für die Bereitstellung der E-Commerce-Anwendung *Relecloud Concerts*, einer Onlineplattform für Konzerttickets, gelten.

## <a name="relevant-use-cases"></a>Relevante Anwendungsfälle

Zu den weiteren relevanten Anwendungsfällen zählen:

- Erstellen einer Anwendung mit elastischer Skalierung zur Verarbeitung von variierenden Benutzerzahlen (Bursts) zu unterschiedlichen Zeiten
- Erstellen einer Anwendung für den Betrieb mit Hochverfügbarkeit in unterschiedlichen Azure-Regionen weltweit

## <a name="architecture"></a>Architecture

![Beispielarchitektur eines Szenarios für eine E-Commerce-Anwendung][architecture]

Bei diesem Szenario geht es um den Kauf von Tickets über eine E-Commerce-Website. Die Daten durchlaufen das Szenario wie folgt:

1. Azure Traffic Manager leitet die Anforderung eines Benutzers an die E-Commerce-Website weiter, die in Azure App Service gehostet wird.
2. Per Azure CDN werden statische Bilder und Inhalte für den Benutzer bereitgestellt.
3. Der Benutzer meldet sich über einen Azure Active Directory B2C-Mandanten an der Anwendung an.
4. Der Benutzer sucht mit Azure Search nach Konzerten.
5. Die Website ruft Konzertdetails aus Azure SQL-Datenbank ab.
6. Die Website verweist auf Bilder von gekauften Tickets in Blob Storage.
7. Ergebnisse von Datenbankabfragen werden in Azure Redis Cache zwischengespeichert, um die Leistung zu erhöhen.
8. Der Benutzer übermittelt Ticketbestellungen und Konzertbewertungen, die in die Warteschlange eingereiht werden.
9. Azure Functions verarbeitet die Bezahlung der Bestellungen und die Konzertbewertungen.
10. Über Cognitive Services wird eine Analyse der Konzertbewertung durchgeführt, um die Stimmung zu ermitteln (positiv oder negativ).
11. Über Application Insights werden Leistungsmetriken für die Überwachung der Integrität der Webanwendung bereitgestellt.

### <a name="components"></a>Komponenten

- [Azure CDN][docs-cdn] stellt statische zwischengespeicherte Inhalte von Standorten bereit, die sich in der Nähe der Benutzer befinden, um die Latenz zu reduzieren.
- Mit [Azure Traffic Manager][docs-traffic-manager] wird die Verteilung von Benutzerdatenverkehr für Dienstendpunkte in unterschiedlichen Azure-Regionen gesteuert.
- [App Services – Web-Apps][docs-webapps] hostet Webanwendungen und ermöglicht die automatische Skalierung und Bereitstellung von Hochverfügbarkeit, ohne eine Infrastruktur verwalten zu müssen.
- [Azure Active Directory – B2C][docs-b2c] ist ein Identitätsverwaltungsdienst, mit dem Sie die Kundenregistrierung und -anmeldung anpassen und steuern und die Kundenprofile in einer Anwendung verwalten können.
- In [Speicherwarteschlangen][docs-storage-queues] wird eine große Zahl von Warteschlangennachrichten gespeichert, auf die mit einer Anwendung zugegriffen werden kann.
- [Functions][docs-functions] sind serverlose Computeoptionen, mit denen Anwendungen bedarfsgesteuert ausgeführt werden können, ohne dass die Infrastruktur verwaltet werden muss.
- Für [Cognitive Services – Standpunktanalyse][docs-sentiment-analysis] werden Machine Learning-APIs von Microsoft verwendet, und Entwickler können Anwendungen problemlos intelligente Features wie Emotions- und Videoerkennung, Gesichtserkennung, Spracherkennung, maschinelles Sehen und Sprachverständnis hinzufügen.
- [Azure Search][docs-search] ist eine Search-as-a-Service-Cloudlösung, die umfangreiche Suchfunktionen für private, heterogene Inhalte in Web- und Unternehmensanwendungen sowie in mobilen Anwendungen ermöglicht.
- [Storage-Blobs][docs-storage-blobs] sind für die Speicherung großer Mengen von unstrukturierten Daten, z.B. Text oder Binärdaten, optimiert.
- Per [Redis Cache][docs-redis-cache] wird eine Verbesserung der Leistung und Skalierbarkeit von Systemen erzielt, die stark von Back-End-Datenspeichern abhängig sind. Daten, auf die häufig zugegriffen wird, werden vorübergehend in schnellen Speicher kopiert, der in der Nähe der Anwendung angeordnet ist.
- [SQL-Datenbank][docs-sql-database] ist ein relationaler verwalteter Datenbankdienst in Microsoft Azure für allgemeine Zwecke, der Strukturen wie relationale Daten, JSON, räumliche Daten und XML unterstützt.
- [Application Insights][docs-application-insights] ist für die ständige Verbesserung der Leistung und Benutzerfreundlichkeit ausgelegt. Leistungsabweichungen werden mit integrierten Analysetools automatisch erkannt, um besser zu verstehen zu können, wofür Benutzer eine App verwenden.

### <a name="alternatives"></a>Alternativen

Es sind noch viele andere Technologien zum Erstellen einer kundenorientierten Anwendung verfügbar, bei der es um den E-Commerce-Bereich mit Bedarfsabhängigkeit geht. Dies gilt sowohl für das Front-End der Anwendung als auch für die Datenschicht.

Weitere Optionen für die Webebene und Funktionen sind:

- [Service Fabric][docs-service-fabric]: Eine Plattform für die Erstellung von verteilten Komponenten, die davon profitieren, dass sie über einen Cluster mit einem hohen Kontrollgrad bereitgestellt und ausgeführt werden. Service Fabric kann auch zum Hosten von Containern verwendet werden.
- [Azure Kubernetes Service][docs-kubernetes-service]: Eine Plattform zum Erstellen und Bereitstellen von containerbasierten Lösungen, die als eine einzelne Implementierung einer Microservicearchitektur verwendet werden können. Dies ermöglicht eine flexible Nutzung der unterschiedlichen Komponenten der Anwendung, damit unabhängig bedarfsabhängig skaliert werden kann.
- [Azure Container Instances][docs-container-instances]: Ermöglicht eine schnelle Bereitstellung und Ausführung von Containern mit einem kurzen Lebenszyklus. Container werden hier bereitgestellt, um einen schnellen Verarbeitungsauftrag auszuführen (etwa die Verarbeitung einer Nachricht oder die Durchführung einer Berechnung). Nach Abschluss des Vorgangs wird die Bereitstellung dann so schnell wie möglich wieder aufgehoben.
- Anstelle einer Speicherwarteschlange kann auch [Service Bus][service-bus] verwendet werden.

Andere Optionen für die Datenschicht:

- [Cosmos DB](/azure/cosmos-db/introduction): Die global verteilte Datenbank von Microsoft mit mehreren Modellen. Dieser Dienst bietet eine Plattform für die Ausführung anderer Datenmodelle. Hierzu zählen etwa Mongo DB, Cassandra, Graphdaten oder ein einfacher Tabellenspeicher.

## <a name="considerations"></a>Überlegungen

### <a name="availability"></a>Verfügbarkeit

- Erwägen Sie beim Erstellen Ihrer Cloudanwendung die Nutzung der [typischen Entwurfsmuster für die Verfügbarkeit][design-patterns-availability].
- Sehen Sie sich die Aspekte zur Verfügbarkeit in der entsprechenden [Referenzarchitektur für App Service-Webanwendungen][app-service-reference-architecture] an.
- Weitere Aspekte zur Verfügbarkeit finden Sie im Azure Architecture Center in der [Checkliste zur Verfügbarkeit][availability].

### <a name="scalability"></a>Skalierbarkeit

- Beachten Sie beim Erstellen einer Cloudanwendung die [typischen Entwurfsmuster für die Skalierbarkeit][design-patterns-scalability].
- Sehen Sie sich die Aspekte zur Skalierbarkeit in der entsprechenden [Referenzarchitektur für App Service-Webanwendungen][app-service-reference-architecture] an.
- Weitere Skalierbarkeitsthemen finden Sie im Azure Architecture Center in der [Checkliste für die Skalierbarkeit][scalability].

### <a name="security"></a>Sicherheit

- Erwägen Sie die Nutzung der [typischen Entwurfsmuster für die Sicherheit][design-patterns-security], wo dies möglich ist.
- Sehen Sie sich die Aspekte zur Sicherheit in der entsprechenden [Referenzarchitektur für App Service-Webanwendungen][app-service-reference-architecture] an.
- Erwägen Sie die Einhaltung eines [sicheren Entwicklungslebenszyklus][secure-development], um Entwickler beim Erstellen von besser geschützter Software und Erfüllen von Konformitätsanforderungen in Bezug auf die Sicherheit zu unterstützen, während gleichzeitig die Entwicklungskosten reduziert werden.
- Sehen Sie sich die Architekturvorlage für [die Azure-Konformität mit PCI-DSS][pci-dss-blueprint] an.

### <a name="resiliency"></a>Resilienz

- Erwägen Sie die Nutzung des [Modus „Trennschalter“][circuit-breaker], um eine korrekte Fehlerbehandlung zu ermöglichen, falls ein Teil der Anwendung nicht verfügbar sein sollte.
- Sehen Sie sich die [typischen Entwurfsmuster für die Resilienz][design-patterns-resiliency] an, und erwägen Sie die Implementierung, falls dies möglich ist.
- Im Azure Architecture Center sind verschiedene [empfohlene Vorgehensweisen für App Service][resiliency-app-service] aufgeführt.
- Erwägen Sie die Nutzung der aktiven [Georeplikation][sql-geo-replication] für die Datenschicht und des [georedundanten][storage-geo-redudancy] Speichers für Images und Warteschlangen.
- Eine ausführlichere Beschreibung der [Resilienz][resiliency] finden Sie im entsprechenden Artikel im Azure Architecture Center.

## <a name="deploy-the-scenario"></a>Bereitstellen des Szenarios

In [diesem Tutorial][end-to-end-walkthrough] erfahren Sie Schritt für Schritt, wie Sie die einzelnen Komponenten bereitstellen. Außerdem enthält dieses Tutorial eine .NET-Beispielanwendung, über die eine einfache Anwendung zum Kaufen von Tickets ausgeführt wird. Darüber hinaus ist eine Resource Manager-Vorlage vorhanden, mit der die Bereitstellung der meisten Azure-Ressourcen automatisiert werden kann.

## <a name="pricing"></a>Preise

Hier können Sie die Betriebskosten für dieses Szenario ermitteln. Alle Dienste sind im Kostenrechner vorkonfiguriert. Wenn Sie wissen möchten, welche Kosten für Ihren spezifischen Anwendungsfall entstehen, passen Sie die entsprechenden Variablen an Ihren voraussichtlichen Datenverkehr an.

Auf der Grundlage des zu erwartenden Datenverkehrsaufkommens haben wir drei exemplarische Kostenprofile erstellt:

- [Klein:][small-pricing] Dieses Preisbeispiel stellt die Komponenten dar, die zum Erstellen des Ausgangs für eine Instanz auf niedriger Produktionsebene erforderlich sind. Hierbei gehen wir von einer geringen Anzahl von Benutzern (wenige Tausend pro Monat) aus. Die App nutzt eine einzelne Instanz einer Standard-Web-App, die für die automatische Skalierung ausreicht. Alle anderen Komponenten werden auf einen Basic-Tarif skaliert, um die Kosten möglichst gering zu halten und trotzdem sicherzustellen, dass SLA-Support und ausreichend Kapazität vorhanden ist, um eine Workload auf Produktionsebene zu verwenden.
- [Mittel:][medium-pricing] Dieses Preisbeispiel stellt die Komponenten einer Bereitstellung mittlerer Größe dar. Hier gehen wir davon aus, dass ca. 100.000 Benutzer das System im Laufe eines Monats verwenden. Der erwartete Datenverkehr wird über eine einzelne App-Dienstinstanz mit einem mittleren Standard-Tarif verarbeitet. Außerdem werden dem Rechner mittlere Cognitive Services-Tarife und Suchdiensttarife hinzugefügt.
- [Groß:][large-pricing] Dieses Preisbeispiel stellt eine Anwendung mit hohen Anforderungen und einer Größenordnung von mehreren Millionen Benutzern pro Monat sowie mit Daten im Terabyte-Bereich dar. Auf dieser Nutzungsebene mit hoher Leistung müssen Web-Apps im Premium-Tarif in mehreren Regionen bereitgestellt werden, und der Einsatz von Traffic Manager ist erforderlich. Die Daten umfassen Folgendes: Speicher, Datenbanken und CDN (konfiguriert für Daten im Terabyte-Bereich).

## <a name="related-resources"></a>Zugehörige Ressourcen

- [Referenzarchitektur für Webanwendungen für mehrere Regionen][multi-region-web-app]
- [Referenz zu eShop in Containern – Beispiel][microservices-ecommerce]

<!-- links -->
[architecture]: ./media/architecture-ecommerce-scenario.png
[small-pricing]: https://azure.com/e/90fbb6a661a04888a57322985f9b34ac
[medium-pricing]: https://azure.com/e/38d5d387e3234537b6859660db1c9973
[large-pricing]: https://azure.com/e/f07f99b6c3134803a14c9b43fcba3e2f
[app-service-reference-architecture]: ../../reference-architectures/app-service-web-app/basic-web-app.md
[availability]: /azure/architecture/checklist/availability
[circuit-breaker]: /azure/architecture/patterns/circuit-breaker
[design-patterns-availability]: /azure/architecture/patterns/category/availability
[design-patterns-resiliency]: /azure/architecture/patterns/category/resiliency
[design-patterns-scalability]: /azure/architecture/patterns/category/performance-scalability
[design-patterns-security]: /azure/architecture/patterns/category/security
[docs-application-insights]: /azure/application-insights/app-insights-overview
[docs-b2c]: /azure/active-directory-b2c/active-directory-b2c-overview
[docs-cdn]: /azure/cdn/cdn-overview
[docs-container-instances]: /azure/container-instances/
[docs-kubernetes-service]: /azure/aks/
[docs-functions]: /azure/azure-functions/functions-overview
[docs-redis-cache]: /azure/redis-cache/cache-overview
[docs-search]: /azure/search/search-what-is-azure-search
[docs-service-fabric]: /azure/service-fabric/
[docs-sentiment-analysis]: /azure/cognitive-services/welcome
[docs-sql-database]: /azure/sql-database/sql-database-technical-overview
[docs-storage-blobs]: /azure/storage/blobs/storage-blobs-introduction
[docs-storage-queues]: /azure/storage/queues/storage-queues-introduction
[docs-traffic-manager]: /azure/traffic-manager/traffic-manager-overview
[docs-webapps]: /azure/app-service/app-service-web-overview
[end-to-end-walkthrough]: https://github.com/Azure/fta-customerfacingapps/tree/master/ecommerce/articles
[microservices-ecommerce]: https://github.com/dotnet-architecture/eShopOnContainers
[multi-region-web-app]: /azure/architecture/reference-architectures/app-service-web-app/multi-region
[pci-dss-blueprint]: /azure/security/blueprints/payment-processing-blueprint
[resiliency-app-service]: /azure/architecture/checklist/resiliency-per-service#app-service
[resiliency]: /azure/architecture/checklist/resiliency
[scalability]: /azure/architecture/checklist/scalability
[secure-development]: https://www.microsoft.com/SDL/process/design.aspx
[service-bus]: /azure/service-bus-messaging/
[sql-geo-replication]: /azure/sql-database/sql-database-geo-replication-overview
[storage-geo-redudancy]: /azure/storage/common/storage-redundancy-grs
