---
title: Bildklassifizierung für Versicherungsansprüche
titleSuffix: Azure Example Scenarios
description: Integrieren Sie Bildverarbeitung in Ihre Azure-Anwendungen.
author: david-stanford
ms.date: 07/05/2018
ms.topic: example-scenario
ms.service: architecture-center
ms.subservice: example-scenario
ms.openlocfilehash: 2630a2a353b2fb5fd6e77e49c7f2027b00503ea6
ms.sourcegitcommit: 1b50810208354577b00e89e5c031b774b02736e2
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/23/2019
ms.locfileid: "54484883"
---
# <a name="image-classification-for-insurance-claims-on-azure"></a>Bildklassifizierung für Versicherungsansprüche in Azure

Dieses Szenario richtet sich an Unternehmen, die eine Bildverarbeitung benötigen.

Mögliche Anwendungsbereiche wären etwa die Klassifizierung von Bildern für eine Modewebsite, die Analyse von Text und Bildern für Versicherungsansprüche oder die Interpretation von Telemetriedaten aus Screenshots von Spielen. In der Vergangenheit mussten sich Unternehmen in der Regel ausführlich mit Machine Learning-Modellen vertraut machen, die Modelle trainieren und die Bilder schließlich durch ihren benutzerdefinierten Prozess schleusen, um die Daten aus den Bildern zu extrahieren.

Durch die Verwendung von Azure-Diensten wie Maschinelles Sehen-API und Azure Functions müssen Unternehmen keine einzelnen Server mehr verwalten und können gleichzeitig ihre Kosten senken und von der Erfahrung profitieren, über die Microsoft im Bereich der Bildverarbeitung mit Cognitive Services verfügt. In diesem Beispielszenario geht es speziell um einen Anwendungsfall mit Bildverarbeitung. Sollten Sie andere KI-Anforderungen haben, ziehen Sie ggf. die Verwendung der vollständigen Suite von [Cognitive Services](/azure/#pivot=products&panel=ai) in Betracht.

## <a name="relevant-use-cases"></a>Relevante Anwendungsfälle

Zu den weiteren relevanten Anwendungsfällen zählen:

- Klassifizieren von Bildern auf einer Modewebsite
- Klassifizieren von Telemetriedaten aus Screenshots von Spielen

## <a name="architecture"></a>Architecture

![Architektur für Bildklassifizierung][architecture]

Dieses Szenario umfasst die Back-End-Komponenten einer webbasierten oder mobilen Anwendung. Die Daten durchlaufen das Szenario wie folgt:

1. Die API-Schicht wird mithilfe von Azure Functions erstellt. Diese APIs ermöglichen es der Anwendung, Bilder hochzuladen und Daten aus Cosmos DB abzurufen.
2. Wenn ein Bild über einen API-Aufruf hochgeladen wird, wird es in Blob Storage gespeichert.
3. Das Hinzufügen von neuen Dateien zu Blob Storage löst eine Event Grid-Benachrichtigung aus, die an eine Azure-Funktion gesendet wird.
4. Azure Functions sendet einen Link für die neu hochgeladene Datei zur Analyse an die Maschinelles Sehen-API.
5. Nachdem die Daten von der Maschinelles Sehen-API zurückgegeben wurden, erstellt Azure Functions einen Eintrag in Cosmos DB, um die Ergebnisse der Analyse zusammen mit den Bildmetadaten zu speichern.

### <a name="components"></a>Komponenten

- [Maschinelles Sehen-API](/azure/cognitive-services/computer-vision/home) ist Teil der Cognitive Services-Suite und dient zum Abrufen von Informationen zu den einzelnen Bildern.
- [Azure Functions](/azure/azure-functions/functions-overview) stellt die Back-End-API für die Webanwendung sowie die Ereignisverarbeitung für hochgeladene Bilder bereit.
- [Event Grid](/azure/event-grid/overview) löst ein Ereignis aus, wenn ein neues Bild in Blob Storage hochgeladen wird. Das Bild wird dann mit Azure Functions verarbeitet.
- [Blob Storage](/azure/storage/blobs/storage-blobs-introduction) speichert alle in die Webanwendung hochgeladenen Bilddateien sowie alle statischen Dateien, die von der Webanwendung genutzt werden.
- [Cosmos DB](/azure/cosmos-db/introduction) speichert Metadaten zu den einzelnen hochgeladenen Bildern sowie die Verarbeitungsergebnisse der Maschinelles Sehen-API.

## <a name="alternatives"></a>Alternativen

- [Custom Vision Service](/azure/cognitive-services/custom-vision-service/home). Die Maschinelles Sehen-API gibt eine Reihe [taxonomiebasierter Kategorien][cv-categories] zurück. Falls Sie Informationen verarbeiten müssen, die nicht von der Maschinelles Sehen-API zurückgegeben werden, ziehen Sie ggf. die Verwendung von Custom Vision Service in Betracht. Dieser Dienst ermöglicht die Erstellung benutzerdefinierter Bildklassifizierungen.
- [Azure Search](/azure/search/search-what-is-azure-search). Wenn in Ihrem Szenario die Metadaten abgefragt werden müssen, um Bilder zu finden, die bestimmten Kriterien entsprechen, empfiehlt sich ggf. die Verwendung von Azure Search. [Cognitive Search](/azure/search/cognitive-search-concept-intro) (momentan in der Vorschauphase) ermöglicht die nahtlose Integration dieses Workflows.

## <a name="considerations"></a>Überlegungen

### <a name="scalability"></a>Skalierbarkeit

Bei den Komponenten dieses Szenarios handelt es sich größtenteils um verwaltete Dienste mit automatischer Skalierung. Wichtige Ausnahmen sind: Azure Functions ist auf maximal 200 Instanzen begrenzt. Sollten Ihre Skalierungsanforderungen über diesen Grenzwert hinausgehen, erwägen Sie die Verwendung mehrerer Regionen oder App-Pläne.

Bei Cosmos DB erfolgt keine automatische Skalierung der bereitgestellten Anforderungseinheiten (Request Units, RUs). Einen Leitfaden für die Schätzung Ihrer Anforderungen finden Sie in unserer Dokumentation unter [Anforderungseinheiten](/azure/cosmos-db/request-units). Machen Sie sich zur optimalen Nutzung der Skalierung in Cosmos DB mit der Funktionsweise von [Partitionsschlüsseln](/azure/cosmos-db/partition-data) in Cosmos DB vertraut.

Bei NoSQL-Datenbanken gehen Verfügbarkeit, Skalierbarkeit und Partitionierung häufig zulasten der Konsistenz (im Sinne des CAP-Theorems). In diesem Beispielszenario wird ein Schlüssel-Wert-Datenmodell verwendet, sodass die Transaktionskonsistenz meist vernachlässigt werden kann, da die meisten Vorgänge per Definition atomisch sind. Weitere Informationen zur [Wahl des richtigen Datenspeichers](../../guide/technology-choices/data-store-overview.md) finden Sie im Azure Architecture Center. Wenn für Ihre Implementierung eine hohe Konsistenz erforderlich ist, können Sie [Ihre Konsistenzebene](/azure/cosmos-db/consistency-levels) in CosmosDB wählen.

Allgemeine Informationen zur Entwicklung skalierbarer Lösungen finden Sie im Azure Architecture Center in der [Checkliste für die Skalierbarkeit][scalability].

### <a name="security"></a>Sicherheit

[Verwaltete Identitäten für Azure-Ressourcen][msi] ermöglichen den Zugriff auf andere interne Ressourcen Ihres Kontos und werden Ihrer Azure Functions-Instanz zugewiesen. Lassen Sie nur den Zugriff auf die erforderlichen Ressourcen in diesen Identitäten zu, um sicherzustellen, dass für Ihre Funktionen (und möglicherweise für Ihre Kunden) keine zusätzlichen Elemente verfügbar gemacht werden.

Allgemeine Informationen zur Entwicklung sicherer Lösungen finden Sie in der [Dokumentation zur Azure-Sicherheit][security].

### <a name="resiliency"></a>Resilienz

Da es sich bei allen Komponenten in diesem Szenario um verwaltete Komponenten handelt, ist deren Resilienz auf regionaler Ebene automatisch gewährleistet.

Allgemeine Informationen zur Entwicklung robuster Lösungen finden Sie unter [Entwerfen robuster Anwendungen für Azure][resiliency].

## <a name="pricing"></a>Preise

Zur Ermittlung der Betriebskosten für dieses Szenario sind alle Dienste im Kostenrechner vorkonfiguriert. Wenn Sie wissen möchten, welche Kosten für Ihren spezifischen Anwendungsfall entstehen, passen Sie die entsprechenden Variablen an Ihren voraussichtlichen Datenverkehr an.

Auf der Grundlage des Datenverkehrs (und unter der Annahme, dass alle Bilder 100 KB groß sind) haben wir drei exemplarische Kostenprofile erstellt:

- [Klein][small-pricing]: Dieses Preisbeispiel entspricht der Verarbeitung von &lt; 5.000 Bildern pro Monat.
- [Mittel][medium-pricing]: Dieses Preisbeispiel entspricht der Verarbeitung von 500.000 Bildern pro Monat.
- [Groß][large-pricing]: Dieses Preisbeispiel entspricht der Verarbeitung von 50 Millionen Bildern pro Monat.

## <a name="related-resources"></a>Zugehörige Ressourcen

Einen geführten Lernpfad finden Sie unter [Erstellen einer serverlosen Web-App in Azure][serverless].

Informieren Sie sich vor dem Bereitstellen dieses Beispielszenarios in einer Produktionsumgebung über die empfohlenen Methoden zum [Optimieren der Leistung und Zuverlässigkeit von Azure Functions][functions-best-practices].

<!-- links -->
[architecture]: ./media/architecture-intelligent-apps-image-processing.png
[small-pricing]: https://azure.com/e/f9b59d238b43423683db73f4a31dc380
[medium-pricing]: https://azure.com/e/7c7fc474db344b87aae93bc29ae27108
[large-pricing]: https://azure.com/e/cbadbca30f8640d6a061f8457a74ba7d
[cognitive-search]: /azure/search/cognitive-search-concept-intro
[serverless]: /azure/functions/tutorial-static-website-serverless-api-with-database
[cv-categories]: /azure/cognitive-services/computer-vision/home#the-86-category-concept
[resiliency]: /azure/architecture/resiliency/
[security]: /azure/security/
[scalability]: /azure/architecture/checklist/scalability
[functions-best-practices]: /azure/azure-functions/functions-best-practices
[msi]: /azure/app-service/app-service-managed-service-identity
