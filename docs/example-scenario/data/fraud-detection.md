---
title: Betrugsermittlung in Echtzeit
titleSuffix: Azure Example Scenarios
description: Erkennen Sie betrügerische Aktivitäten mithilfe von Azure Event Hubs und Stream Analytics in Echtzeit.
author: alexbuckgit
ms.date: 07/05/2018
ms.topic: example-scenario
ms.service: architecture-center
ms.subservice: example-scenario
social_image_url: /azure/architecture/example-scenario/data/media/architecture-fraud-detection.png
ms.openlocfilehash: b10838635cb592eb93d35ce745832c55a6daae8b
ms.sourcegitcommit: 3b15d65e7c35a19506e562c444343f8467b6a073
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/25/2019
ms.locfileid: "54908340"
---
# <a name="real-time-fraud-detection-on-azure"></a>Betrugserkennung in Echtzeit in Azure

Dieses Beispielszenario ist relevant für Organisationen, die Daten in Echtzeit analysieren müssen, um betrügerische Transaktionen oder andere anomale Aktivitäten zu erkennen.

Ein mögliches Anwendungsgebiet ist etwa die Erkennung betrügerischer Kreditkartenaktivitäten oder Mobiltelefonanrufe. Bei herkömmlichen onlinebasierten Analysesystemen kann es mehrere Stunden dauern, bis die Daten transformiert und analysiert wurden, um anomale Aktivitäten zu erkennen.

Mit den vollständig verwalteten Azure-Diensten wie Event Hubs und Stream Analytics müssen Unternehmen keine einzelnen Server verwalten und können gleichzeitig ihre Kosten senken und von der Erfahrung profitieren, über die Microsoft bei der cloudbasierten Datenerfassung sowie bei Analysen in Echtzeit verfügt. In diesem Szenario geht es speziell um die Erkennung betrügerischer Aktivitäten. Informationen zu anderen Datenanalysen finden Sie bei Bedarf in der Liste verfügbarer [Azure Analytics-Dienste][product-category].

Dieses Beispiel ist Teil einer umfassenderen Datenverarbeitungsarchitektur und -strategie. Weitere Optionen für diesen Aspekt einer Gesamtarchitektur werden weiter unten in diesem Artikel erläutert.

## <a name="relevant-use-cases"></a>Relevante Anwendungsfälle

Zu den weiteren relevanten Anwendungsfällen zählen:

- Erkennen betrügerischer Mobiltelefonanrufe in Telekommunikationsszenarien
- Identifizieren betrügerischer Kreditkartentransaktionen für Banken
- Identifizieren betrügerischer Einkäufe in Einzelhandels- oder E-Commerce-Szenarien

## <a name="architecture"></a>Architecture

![Übersicht über die Architektur der Azure-Komponenten für ein Szenario zur Betrugserkennung in Echtzeit][architecture]

Dieses Szenario umfasst die Back-End-Komponenten einer Pipeline für Echtzeitanalysen. Die Daten durchlaufen das Szenario wie folgt:

1. Metadaten von Mobiltelefonanrufen werden aus dem Quellsystem an eine Azure Event Hubs-Instanz gesendet.
2. Ein Stream Analytics-Auftrag wird gestartet, der Daten über die Event Hub-Quelle empfängt.
3. Der Stream Analytics-Auftrag führt eine vordefinierte Abfrage aus, um den Eingabedatenstrom zu transformieren und auf der Grundlage eines Algorithmus für betrügerische Transaktionen zu analysieren. Diese Abfrage verwendet ein rollierendes Fenster, um den Datenstrom in unterschiedliche Zeiteinheiten zu unterteilen.
4. Der Stream Analytics-Auftrag schreibt den transformierten Datenstrom, der erkannte betrügerische Anrufe darstellt, in eine Ausgabesenke in Azure Blob Storage.

### <a name="components"></a>Komponenten

- [Azure Event Hubs][docs-event-hubs] ist eine Echtzeitstreaming-Plattform und ein Ereigniserfassungsdienst, der pro Sekunde Millionen von Ereignissen empfangen und verarbeiten kann. Event Hubs kann Ereignisse, Daten oder Telemetriedaten, die von verteilter Software und verteilten Geräten erzeugt wurden, verarbeiten und speichern. In diesem Szenario empfängt Event Hubs alle Anrufmetadaten, die auf betrügerische Aktivitäten analysiert werden sollen.
- [Azure Stream Analytics][docs-stream-analytics] ist eine Ereignisverarbeitungsengine, die große Datenmengen analysieren kann, die von Geräten und anderen Quellen gestreamt werden. Zur Identifizierung von Mustern und Beziehungen wird zudem das Extrahieren von Informationen aus Datenströmen unterstützt. Diese Muster können weitere Downstreamaktionen auslösen. In diesem Szenario transformiert Stream Analytics den Eingabedatenstrom aus Event Hubs, um betrügerische Anrufe zu identifizieren.
- [Blob Storage](/azure/storage/blobs/storage-blobs-introduction) wird in diesem Szenario zum Speichern der Ergebnisse des Stream Analytics-Auftrags verwendet.

## <a name="considerations"></a>Überlegungen

### <a name="alternatives"></a>Alternativen

Für Echtzeiterfassung von Nachrichten, Datenspeicherung, Datenstromverarbeitung und die Speicherung von Analysedaten sowie für Analysen und Berichte stehen zahlreiche Technologieoptionen zur Verfügung. Eine Übersicht über diese Optionen, die zugehörigen Funktionen und wichtige Auswahlkriterien finden Sie unter [Big Data-Architekturen: Echtzeitverarbeitung](/azure/architecture/data-guide/technology-choices/real-time-ingestion) im Azure-Datenarchitekturleitfaden.

Von verschiedenen Machine Learning-Diensten in Azure können zudem komplexere Algorithmen für die Betrugserkennung generiert werden. Eine Übersicht über diese Optionen finden Sie im [Azure-Datenarchitekturleitfaden](../../data-guide/index.md) unter [Auswählen einer Machine Learning-Technologie in Azure](/azure/architecture/data-guide/technology-choices/data-science-and-machine-learning).

### <a name="availability"></a>Verfügbarkeit

Azure Monitor bietet einheitliche Benutzeroberflächen für die übergreifende Überwachung verschiedener Azure-Dienste. Weitere Informationen finden Sie unter [Überwachen von Azure-Anwendungen und -Ressourcen](/azure/monitoring-and-diagnostics/monitoring-overview). Event Hubs und Stream Analytics sind jeweils mit Azure Monitor verknüpft.

Weitere Überlegungen zur Verfügbarkeit finden Sie im Azure Architecture Center in der [Checkliste für die Verfügbarkeit][availability].

### <a name="scalability"></a>Skalierbarkeit

Die Komponenten dieses Szenarios sind für die Erfassung mit Hyperskalierung sowie für hochgradig parallelisierte Echtzeitanalysen konzipiert. Azure Event Hubs ist hochgradig skalierbar und kann pro Sekunde Millionen von Ereignissen mit geringer Wartezeit empfangen und verarbeiten. Event Hubs kann die Anzahl von Durchsatzeinheiten bei Bedarf [automatisch zentral hochskalieren](/azure/event-hubs/event-hubs-auto-inflate). Azure Stream Analytics kann große Mengen von Streamingdaten aus zahlreichen Quellen analysieren. Sie können Stream Analytics zentral hochskalieren, indem Sie die Anzahl von [Streamingeinheiten](/azure/stream-analytics/stream-analytics-streaming-unit-consumption) erhöhen, die für die Ausführung Ihres Streamingauftrags zugeteilt sind.

Allgemeine Informationen zur Entwicklung skalierbarer Lösungen finden Sie im Azure Architecture Center in der [Checkliste für die Skalierbarkeit][scalability].

### <a name="security"></a>Sicherheit

Azure Event Hubs verwendet zum Schutz der Daten ein [Authentifizierungs- und Sicherheitsmodell][docs-event-hubs-security-model], das auf einer Kombination aus SAS-Token (Shared Access Signature) und Ereignisherausgebern basiert. Ein Ereignisherausgeber definiert einen virtuellen Endpunkt für einen Event Hub. Der Herausgeber kann nur zum Senden von Nachrichten an einen Event Hub verwendet werden. Es ist nicht möglich, von einem Herausgeber Nachrichten zu empfangen.

Allgemeine Informationen zur Entwicklung sicherer Lösungen finden Sie in der [Dokumentation zur Azure-Sicherheit][security].

### <a name="resiliency"></a>Resilienz

Allgemeine Informationen zur Entwicklung robuster Lösungen finden Sie unter [Entwerfen robuster Anwendungen für Azure][resiliency].

## <a name="deploy-the-scenario"></a>Bereitstellen des Szenarios

In [diesem Tutorial][tutorial] erfahren Sie Schritt für Schritt, wie Sie die einzelnen Komponenten des Szenarios manuell bereitstellen. Das Tutorial beinhaltet auch eine .NET-Clientanwendung, mit der Sie exemplarische Anrufmetadaten generieren und an eine Event Hub-Instanz senden können.

## <a name="pricing"></a>Preise

Zur Ermittlung der Betriebskosten für dieses Szenario sind alle Dienste im Kostenrechner vorkonfiguriert. Wenn Sie wissen möchten, welche Kosten für Ihren spezifischen Anwendungsfall entstehen, passen Sie die entsprechenden Variablen an Ihre voraussichtliche Datenmenge an.

Auf der Grundlage des zu erwartenden Datenverkehrsaufkommens haben wir drei exemplarische Kostenprofile erstellt:

- [Klein:][small-pricing] Verarbeitung von einer Million Ereignissen über eine einzelne Standardstreamingeinheit pro Monat.
- [Mittel:][medium-pricing] Verarbeitung von 100 Millionen Ereignissen über fünf Standardstreamingeinheiten pro Monat.
- [Groß:][large-pricing] Verarbeitung von 999 Millionen Ereignissen über 20 Standardstreamingeinheiten pro Monat.

## <a name="related-resources"></a>Zugehörige Ressourcen

Bei komplexeren Betrugserkennungsszenarien kann die Verwendung eines Machine Learning-Modells von Vorteil sein. Informationen zu Szenarien mit Machine Learning Server finden Sie unter [Fraud Detection][r-server-fraud-detection] (Betrugserkennung). Weitere Lösungsvorlagen mit Machine Learning Server finden Sie unter [Solution templates for Machine Learning Server and Microsoft R Server 9.1/9.2][docs-r-server-sample-solutions] (Lösungsvorlagen für Machine Learning Server und Microsoft R Server 9.1/9.2). Eine Beispiellösung mit Azure Data Lake Analytics finden Sie unter [Using Azure Data Lake and R for Fraud Detection][technet-fraud-detection] (Verwenden von Azure Data Lake und R für die Betrugserkennung).

<!-- links -->
[product-category]: https://azure.microsoft.com/product-categories/analytics/
[tutorial]: /azure/stream-analytics/stream-analytics-real-time-fraud-detection
[small-pricing]: https://azure.com/e/74149ec312c049ccba79bfb3cfa67606
[medium-pricing]: https://azure.com/e/4fc94f7376de484d8ae67a6958cae60a
[large-pricing]: https://azure.com/e/7da8804396f9428a984578700003ba42
[architecture]: ./media/architecture-fraud-detection.png
[docs-event-hubs]: /azure/event-hubs/event-hubs-what-is-event-hubs
[docs-event-hubs-security-model]: /azure/event-hubs/event-hubs-authentication-and-security-model-overview
[docs-stream-analytics]: /azure/stream-analytics/stream-analytics-introduction
[docs-r-server-sample-solutions]: /machine-learning-server/r/sample-solutions
[r-server-fraud-detection]: https://microsoft.github.io/r-server-fraud-detection/
[technet-fraud-detection]: https://blogs.technet.microsoft.com/machinelearning/2017/06/28/using-azure-data-lake-and-r-for-fraud-detection/
[availability]: /azure/architecture/checklist/availability
[scalability]: /azure/architecture/checklist/scalability
[resiliency]: ../../resiliency/index.md
[security]: /azure/security/
