---
title: Skalierbare Auftragsverarbeitung in Azure
description: Beispielszenario für die Erstellung einer hoch skalierbaren Pipeline für die Auftragsverarbeitung per Azure Cosmos DB.
author: alexbuckgit
ms.date: 07/10/2018
ms.openlocfilehash: 541b5e9f523c64bc55526e4e2dffc57a5212e67f
ms.sourcegitcommit: 71cbef121c40ef36e2d6e3a088cb85c4260599b9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/14/2018
ms.locfileid: "39060981"
---
# <a name="scalable-order-processing-on-azure"></a>Skalierbare Auftragsverarbeitung in Azure

Dieses Beispielszenario ist für Organisationen relevant, die eine hoch skalierbare und robuste Architektur für die Verarbeitung von Onlineaufträgen benötigen. Zu den potenziellen Anwendungsbereichen gehören E-Commerce und Points of Sale im Einzelhandel, Auftragserfüllung und Bestandsreservierung und -nachverfolgung. 

In diesem Szenario wird ein Event Sourcing-Ansatz (Ereignisherkunftsermittlung) verwendet, indem über Microservices ein funktionierendes Programmiermodell implementiert wird. Jeder Microservice wird als Datenstromprozessor behandelt, und die gesamte Geschäftslogik wird mithilfe von Microservices implementiert. Dieser Ansatz ermöglicht Hochverfügbarkeit sowie Resilienz, Georeplikation und eine hohe Leistung.

Die Verwendung von verwalteten Azure-Diensten, z.B. Cosmos DB und HDInsight, kann zur Kostenreduzierung beitragen, indem das Wissen von Microsoft in Bezug auf die global verteilte Datenspeicherung und der damit verbundene Datenabruf auf Cloudebene genutzt werden. Dieses Szenario bezieht sich speziell auf den Bereich E-Commerce bzw. Einzelhandel. Falls Sie über andere Anforderungen an Datendienste verfügen, hilft Ihnen die Liste mit den verfügbaren [vollständig verwalteten intelligenten Datenbankdiensten in Azure][product-category] weiter.

## <a name="related-use-cases"></a>Verwandte Anwendungsfälle

Erwägen Sie dieses Szenario für folgende Anwendungsfälle:

* Back-End-Systeme für E-Commerce oder Points of Sale im Einzelhandel
* Systeme für die Bestandsverwaltung
* Systeme für die Auftragserfüllung
* Weitere Integrationsszenarien, die für eine Pipeline zur Auftragsverarbeitung relevant sind

## <a name="architecture"></a>Architektur

![Beispielarchitektur für eine skalierbare Pipeline zur Auftragsverarbeitung][architecture-diagram]

Anhand dieser Architektur werden die wichtigsten Komponenten einer Pipeline für die Auftragsverarbeitung ausführlich beschrieben. Die Daten durchlaufen das Szenario wie folgt:

1. Ereignisnachrichten gelangen über kundenorientierte Anwendungen in das System (synchron per HTTP) und in verschiedene Back-End-Systeme (asynchron per Apache Kafka). Diese Nachrichten werden an eine Pipeline für die Befehlsverarbeitung übergeben.
2. Jede Ereignisnachricht wird erfasst und über einen Befehlsprozessor-Microservice einer definierten Gruppe von Befehlen zugeordnet. Der Befehlsprozessor ruft alle aktuellen Status, die für die Ausführung des Befehls relevant sind, aus einer Ereignisdatenstrom-Momentaufnahmedatenbank ab. Der Befehl wird dann ausgeführt, und die Ausgabe des Befehls wird als neues Ereignis ausgegeben.
3. Jedes Ereignis, das für einen Befehl ausgegeben wird, wird per Cosmos DB in eine Ereignisdatenstrom-Datenbank committet.
4. Für jede Datenbankeinfügung und jedes Update, die bzw. das für die Ereignisdatenstrom-Datenbank committet wird, wird vom Cosmos DB-Änderungsfeed ein Ereignis ausgelöst. Nachgeschaltete Systeme können alle Ereignisthemen abonnieren, die für das jeweilige System relevant sind.
5. Alle Ereignisse aus dem Cosmos DB-Änderungsfeed werden außerdem an einen Microservice für Momentaufnahmen-Ereignisdatenströme gesendet, über den alle von Ereignissen verursachten Statusänderungen berechnet werden, die aufgetreten sind. Der neue Status wird dann für die in Cosmos DB gespeicherte Ereignisdatenstrom-Momentaufnahmedatenbank committet.  Die Momentaufnahmedatenbank stellt eine global verteilte Datenquelle mit geringer Latenz für den aktuellen Status aller Datenelemente bereit. Die Ereignisdatenstrom-Datenbank umfasst eine vollständige Aufzeichnung aller Ereignisnachrichten, die die Architektur durchlaufen haben, um robuste Szenarien für Tests, Problembehandlung und Notfallwiederherstellung zu ermöglichen.  

### <a name="components"></a>Komponenten

* [Cosmos DB][docs-cosmos-db] ist die global verteilte Datenbank von Microsoft mit mehreren Modellen. Hiermit können Ihre Lösungen den Durchsatz und die Speicherung für eine beliebige Zahl von geografischen Regionen elastisch und unabhängig skalieren. Azure Cosmos DB bietet Ihnen mit umfassenden Vereinbarungen zum Servicelevel (Service Level Agreements, SLAs) Durchsatz-, Wartezeit-, Verfügbarkeits- und Konsistenzgarantien. In diesem Szenario wird Cosmos DB für die Speicherung von Ereignisdatenströmen und Momentaufnahmen verwendet, und die Features des Cosmos DB-Änderungsfeeds werden genutzt, um für Datenkonsistenz und die Wiederherstellung nach Fehlern zu sorgen. 
* [Apache Kafka in HDInsight][docs-kafka] ist eine Implementierung eines verwalteten Diensts von Apache Kafka. Hierbei handelt es sich um eine verteilte Open-Source-Streamingplattform für die Erstellung von Datenpipelines und Anwendungen mit Echtzeitstreaming. Kafka verfügt auch über Nachrichtenbrokerfunktionen, die einer Nachrichtenwarteschlange zum Veröffentlichen und Abonnieren von benannten Datenströmen ähneln. In diesem Szenario wird Kafka genutzt, um eingehende und nachgeschaltete Ereignisse in der Pipeline für die Auftragsverarbeitung zu verarbeiten. 

## <a name="considerations"></a>Überlegungen

Für die Echtzeiterfassung von Nachrichten, Datenspeicherung, Datenstromverarbeitung und Speicherung von Analysedaten sowie für Analysen und Berichte stehen zahlreiche Technologieoptionen zur Verfügung. Eine Übersicht über diese Optionen, ihre Funktionen und wichtige Auswahlkriterien finden Sie im [Azure-Datenarchitekturleitfaden](/azure/architecture/data-guide/) unter [Auswählen einer Technologie für die Echtzeiterfassung von Nachrichten in Azure](/azure/architecture/data-guide/technology-choices/real-time-ingestion).

Microservices haben sich zu einem beliebten Architekturstil für die Erstellung robuster, hochgradig skalierbarer, unabhängig bereitstellbarer Cloudanwendungen entwickelt, die sich schnell weiterentwickeln lassen. Für Microservices ist in Bezug auf das Entwerfen und Erstellen von Anwendungen ein anderer Ansatz erforderlich. Tools wie Docker, Kubernetes, Azure Service Fabric und Nomad ermöglichen die Entwicklung von Architekturen, die auf Microservices basieren. Eine Anleitung zur Erstellung und Ausführung einer auf Microservices basierenden Architektur finden Sie im Azure Architecture Center unter [Entwerfen von Microservices in Azure](/azure/architecture/microservices/).

### <a name="availability"></a>Verfügbarkeit

Mit dem Event Sourcing-Ansatz dieses Szenarios können Systemkomponenten lose gekoppelt und unabhängig voneinander bereitgestellt werden. Cosmos DB ermöglicht [Hochverfügbarkeit][docs-cosmos-db-regional-failover], und Organisationen können damit Nachteile im Hinblick auf Konsistenz, Verfügbarkeit und Leistung ausgleichen – jeweils mit [entsprechenden Garantien][docs-cosmos-db-guarantees]. Apache Kafka in HDInsight ist ebenfalls auf [Hochverfügbarkeit][docs-kafka-high-availability] ausgelegt.

Azure Monitor bietet einheitliche Benutzeroberflächen für die übergreifende Überwachung verschiedener Azure-Dienste. Weitere Informationen finden Sie unter [Überwachen von Azure-Anwendungen und -Ressourcen](/azure/monitoring-and-diagnostics/monitoring-overview). Event Hubs und Stream Analytics sind jeweils mit Azure Monitor verknüpft. 

Weitere Aspekte zur Verfügbarkeit finden Sie in der [Checkliste für die Verfügbarkeit][availability].

### <a name="scalability"></a>Skalierbarkeit

Kafka in HDInsight ermöglicht die [Konfiguration der Speicherung und Skalierbarkeit](/azure/hdinsight/kafka/apache-kafka-scalability) für Kafka-Cluster. Mit Cosmos DB erhalten Sie eine schnelle und vorhersagbare Leistung und können [nahtlos skalieren](/azure/cosmos-db/partition-data), wenn Ihre Anwendung wächst.
Die auf Microservices basierende Event Sourcing-Architektur dieses Szenarios vereinfacht außerdem das Skalieren Ihres Systems und das Erweitern der Funktionalität.

Weitere Aspekte zur Skalierbarkeit finden Sie im Azure Architecture Center in der [Checkliste für die Skalierbarkeit][scalability].

### <a name="security"></a>Sicherheit

Über das [Cosmos DB-Sicherheitsmodell](/azure/cosmos-db/secure-access-to-data) werden Benutzer authentifiziert, und der Zugriff auf die entsprechenden Daten und Ressourcen wird gewährt. Weitere Informationen finden Sie unter [Azure Cosmos DB-Datenbanksicherheit](/en-us/azure/cosmos-db/database-security).

Allgemeine Informationen zur Entwicklung sicherer Lösungen finden Sie in der [Dokumentation zur Azure-Sicherheit][security].

### <a name="resiliency"></a>Resilienz

Die Event Sourcing-Architektur und die zugehörige Technologie in diesem Beispielszenario sorgen für eine hohe Resilienz gegenüber dem Auftreten von Fehlern. Allgemeine Informationen zur Entwicklung robuster Lösungen finden Sie unter [Entwerfen robuster Anwendungen für Azure][resiliency].

## <a name="pricing"></a>Preise

Zur Untersuchung der Betriebskosten für diese Lösung sind alle Dienste im Kostenrechner vorkonfiguriert.  Wenn Sie wissen möchten, welche Kosten für Ihr jeweiliges Szenario entstehen, passen Sie die entsprechenden Variablen an Ihre voraussichtliche Datenmenge an. In diesem Szenario sind in den Beispielpreisen nur Cosmos DB und ein Kafka-Cluster für die Verarbeitung von Ereignissen enthalten, die über den Cosmos DB-Änderungsfeed ausgelöst werden. Ereignisprozessoren und Microservices für Ursprungssysteme und andere nachgeschaltete Systeme sind nicht enthalten. Die Kosten hierfür sind stark von der Anzahl und Skalierung dieser Dienste sowie von der Technologie abhängig, die für deren Implementierung gewählt wurde.

Die Währung von Azure Cosmos DB ist die Anforderungseinheit (Request Unit, RU). Mit Anforderungseinheiten ist es nicht mehr erforderlich, Kapazitäten für Lese- und Schreibvorgänge zu reservieren oder CPU, Arbeitsspeicher und IOPS bereitzustellen. Azure Cosmos DB unterstützt verschiedene APIs mit unterschiedlichen Vorgängen, die von einfachen Lese- und Schreibvorgängen bis hin zu komplexen Graphabfragen reichen. Da die Anforderungen sich unterscheiden, wird ihnen eine normalisierte Menge von Anforderungseinheiten zugewiesen, die auf dem Rechenaufwand basiert, der für die Verarbeitung der Anforderung erforderlich ist. Die Anzahl von Anforderungseinheiten, die für Ihre Lösung erforderlich sind, richtet sich nach der Datenelementgröße und der Anzahl von Lese- und Schreibvorgängen der Datenbank pro Sekunde. Weitere Informationen finden Sie unter [Anforderungseinheiten in Azure Cosmos DB](/azure/cosmos-db/request-units). Diese geschätzten Preise basieren auf der Ausführung von Cosmos DB in zwei Azure-Regionen.

Auf Grundlage der zu erwartenden Aktivitätsmenge haben wir drei exemplarische Kostenprofile erstellt:

* [Klein][small-pricing]: Entspricht 5 reservierten RUs mit einem 1-TB-Datenspeicher in Cosmos DB und einem kleinen Kafka-Cluster (D3 v2).
* [Mittel][medium-pricing]: Entspricht 50 reservierten RUs mit einem 10-TB-Datenspeicher in Cosmos DB und einem mittelgroßen Kafka-Cluster (D4 v2).
* [Groß][large-pricing]: Entspricht 500 reservierten RUs mit einem 30-TB-Datenspeicher in Cosmos DB und einem großen Kafka-Cluster (D5 v2).

## <a name="related-resources"></a>Zugehörige Ressourcen

Dieses Beispielszenario basiert auf einer umfassenderen Version dieser Architektur, die von [Jet.com](https://jet.com) für die Pipeline zur End-to-End-Auftragsverarbeitung des Unternehmens erstellt wurde. Weitere Informationen finden Sie unter dem [technischen Kundenprofil von jet.com][source-document] und in der [jet.com-Präsentation auf der Build 2018][source-presentation]. 

Weitere verwandte Ressourcen:
* _[Designing Data-Intensive Applications](https://dataintensive.net/)_ (Entwerfen von datenintensiven Anwendungen) von Martin Kleppmann (O'Reilly Media, 2017).
* _[Domain Modeling Made Functional: Tackle Software Complexity with Domain-Driven Design and F#](https://pragprog.com/book/swdddf/domain-modeling-made-functional)_ (Funktionelle Domänenmodellierung: Lösen der Komplexität von Software mit Domain-Driven Design und F#) von Scott Wlaschin (Pragmatic Programmers LLC, 2018).
* Andere [Cosmos DB-Anwendungsfälle][docs-cosmos-db-use-cases]
* [Echtzeit-Verarbeitungsarchitektur](/azure/architecture/data-guide/big-data/real-time-processing) im [Azure-Datenarchitekturleitfaden](/azure/architecture/data-guide/)

<!-- links -->
[product-category]: https://azure.microsoft.com/product-categories/databases/
[source-document]: https://customers.microsoft.com/en-us/story/jet-com-powers-innovative-e-commerce-engine-on-azure-in-less-than-12-months
[source-presentation]: https://channel9.msdn.com/events/Build/2018/BRK3602
[small-pricing]: https://azure.com/e/3d43949ffbb945a88cc0a126dc3a0e6e
[medium-pricing]: https://azure.com/e/1f1e7bf2a6ad4f7799581211f4369b9b
[large-pricing]: https://azure.com/e/75207172ece94cf6b5fb354a2252b333
[architecture-diagram]: ./images/architecture-diagram-cosmos-db.png
[docs-cosmos-db]: /azure/cosmos-db
[docs-cosmos-db-change-feed]: /azure/cosmos-db/change-feed
[docs-cosmos-db-regional-failover]: /azure/cosmos-db/regional-failover
[docs-cosmos-db-guarantees]: /azure/cosmos-db/distribute-data-globally#AvailabilityGuarantees
[docs-cosmos-db-use-cases]: /azure/cosmos-db/use-cases
[docs-kafka]: /azure/hdinsight/kafka/apache-kafka-introduction
[docs-kafka-high-availability]: /azure/hdinsight/kafka/apache-kafka-high-availability
[docs-event-hubs]: /azure/event-hubs/event-hubs-what-is-event-hubs
[docs-stream-analytics]: /azure/stream-analytics/stream-analytics-introduction
[docs-blob-storage]: /azure/storage/blobs/storage-blobs-introduction
[availability]: /azure/architecture/checklist/availability
[scalability]: /azure/architecture/checklist/scalability
[resiliency]: /azure/architecture/patterns/category/resiliency/
[security]: /azure/security/
