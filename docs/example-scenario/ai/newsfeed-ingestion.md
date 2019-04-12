---
title: Massenerfassung und -analyse von Newsfeeds in Azure
description: Erstellen Sie nur mit Azure-Diensten (etwa Azure Cosmos DB und Azure Cognitive Services) eine Pipeline für die Erfassung und Analyse von Texten, Bildern, Stimmung und anderen Daten aus RSS-Newsfeeds.
author: njray
ms.date: 2/1/2019
ms.custom: azcat-ai, AI
social_image_url: /azure/architecture/example-scenario/ai/media/mass-ingestion-newsfeeds-architecture.png
ms.topic: example-scenario
ms.service: architecture-center
ms.subservice: example-scenario
ms.openlocfilehash: e1bc636103753d474b545a6e3e9118b2ef302b34
ms.sourcegitcommit: ea97ac004c38c6b456794c1a8eef29f8d2b77d50
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/26/2019
ms.locfileid: "58489255"
---
# <a name="mass-ingestion-and-analysis-of-news-feeds-on-azure"></a>Massenerfassung und -analyse von Newsfeeds in Azure

In diesem Beispielszenario wird eine Pipeline für die Massenerfassung und echtzeitnahe Analyse von Dokumenten unter Verwendung öffentlicher RSS-Newsfeeds beschrieben.  Dazu wird Azure Cognitive Services verwendet, um nützliche Erkenntnisse mithilfe von Textübersetzung, Gesichts- und Stimmungserkennung zu liefern.

Dieses Szenario enthält Beispiele für Nachrichtenfeeds in [Englisch][english], [Russisch][russian] und [Deutsch][german], aber Sie können es ganz einfach auch für andere RSS-Feeds nutzen. Um die Bereitstellung zu erleichtern, basieren Erfassung, Verarbeitung und Analyse der Daten vollständig auf Azure-Diensten.

## <a name="relevant-use-cases"></a>Relevante Anwendungsfälle

Obwohl dieses Szenario auf der Verarbeitung von RSS-Feeds basiert, ist es für alle Dokumente, Websites oder Artikel relevant, für die Folgendes gewünscht wird:

* Übersetzen von Text in die Sprache Ihrer Wahl
* Bestimmen von Schlüsselbegriffen, Entitäten und Benutzerstimmung in digitalen Inhalten
* Erkennen von Objekten, Text und Orientierungspunkten in Bildern, die mit einem digitalen Artikel verknüpft sind
* Erkennen von Personen anhand ihres Geschlechts und Alters in Bildern in digitalen Inhalten

## <a name="architecture"></a>Architecture

![][architecture]

Die Daten durchlaufen die Lösung wie folgt:

1. Ein RSS-Nachrichtenfeed fungiert als Generator, der Daten aus einem Dokument oder Artikel abruft. Beispielsweise enthalten Daten in einem Artikel typischerweise einen Titel, eine Zusammenfassung des Inhalts der Nachricht und mitunter Bilder.

2. Ein Generator- oder Erfassungsprozess fügt den Artikel und alle zugehörigen Bilder in eine Azure Cosmos DB-[Sammlung][collection] ein.

3. Eine Benachrichtigung löst eine Erfassungsfunktion in Azure Functions aus, die den Artikeltext in CosmosDB und die Artikelbilder (falls vorhanden) in Azure Blob Storage speichert.  Der Artikel wird dann an die nächste Warteschlange übergeben.

4. Eine Übersetzungsfunktion wird durch das Warteschlangenereignis ausgelöst. Sie verwendet die [Textübersetzungs-API][translate-text] von Azure Cognitive Services, um die Sprache zu erkennen, den Text bei Bedarf zu übersetzen und die Stimmung, Schlüsselbegriffe und Entitäten in Text und Titel zu sammeln. Der Artikel wird anschließend an die nächste Warteschlange übergeben.

5. Im Artikel in der Warteschlange wird eine Erkennungsfunktion ausgelöst. Sie verwendet den Dienst [Maschinelles Sehen][vision], um Objekte, Orientierungspunkte und geschriebene Wörter im zugehörigen Bild zu erkennen, und leitet den Artikel dann an die nächste Warteschlange weiter.

6. Im Artikel in der Warteschlange wird eine Gesichtserkennungsfunktion ausgelöst. Sie verwendet die [Gesichtserkennungs-API von Azure][face], um Geschlecht und Alter von Gesichtern im zugehörigen Bild zu erkennen, und leitet den Artikel dann an die nächste Warteschlange weiter.

7. Wenn alle Funktionen abgeschlossen sind, wird die Benachrichtigungsfunktion ausgelöst. Sie lädt die für den Artikel verarbeiteten Datensätzen und untersucht sie auf gewünschte Ergebnisse. Bei einem Fund wird der Inhalt markiert und eine Benachrichtigung an das System Ihrer Wahl gesendet.

Bei jedem Verarbeitungsschritt schreibt die Funktion die Ergebnisse in Azure Cosmos DB. Schließlich können die Daten nach Belieben verwendet werden. Sie können damit beispielsweise Geschäftsprozesse verbessern, neue Kunden ausfindig machen oder Probleme mit der Kundenzufriedenheit aufdecken.

### <a name="components"></a>Komponenten

Bei diesem Beispiel werden die Azure-Komponenten in der folgenden Liste verwendet.

* [Azure Storage][storage] dient zum Speichern von unbearbeiteten Bild- und Videodateien, die einem Artikel zugeordnet sind. Ein sekundäres Speicherkonto wird mit Azure App Service erstellt, das zum Hosten des Azure-Funktionscodes und von Protokollen dient.

* [Azure Cosmos DB][cosmos-db] enthält Nachverfolgungsinformationen zu Text, Bildern und Videos im Artikel. Die Ergebnisse der Cognitive Services-Schritte werden ebenfalls hier gespeichert.

* [Azure Functions][functions] führt den Funktionscode aus, der verwendet wird, um auf Warteschlangennachrichten zu reagieren und den eingehenden Inhalt zu transformieren. [Azure App Service][aas] hostet den Funktionscode und verarbeitet die Datensätze seriell. Dieses Szenario umfasst fünf Funktionen: Erfassung, Transformation, Objekterkennung, Gesichtserkennung und Benachrichtigung.

* [Azure Service Bus][service-bus] hostet die Azure Service Bus-Warteschlangen, die von den Funktionen verwendet werden.

* [Azure Cognitive Services][acs] liefert die KI für die Pipeline basierend auf Implementierungen des Diensts [Maschinelles Sehen][vision], der [Gesichtserkennungs-API][face] und des maschinellen Übersetzungsdiensts [Text übersetzen ][translate-text].

* [Azure Application Insights][aai] bietet Analysen, die Ihnen helfen, Probleme zu diagnostizieren und die Funktionalität Ihrer Anwendung zu verstehen.

### <a name="alternatives"></a>Alternativen

* Anstatt ein Muster zu verwenden, das auf Warteschlangenbenachrichtigungen und Azure Functions basiert, können Sie für diesen Datenfluss ein anderes Muster verwenden. So können beispielsweise [Azure Service Bus-Themen][topics] verwendet werden, um die verschiedenen Teile des Artikels im Gegensatz zur in diesem Beispiel durchgeführten seriellen Verarbeitung parallel zu verarbeiten. Weitere Informationen finden Sie unter [Warteschlangen und Themen][queues-topics].

* Verwenden Sie [Azure Logic App][logic-app], um den Funktionscode zu implementieren, und implementieren Sie Sperren auf Datensatzebene wie [Redlock][redlock] (was für die Parallelverarbeitung erforderlich ist, bis Azure Cosmos DB [teilweise Dokumentaktualisierungen][partial] unterstützt). Weitere Informationen finden Sie unter [Vergleich zwischen Azure Functions und Azure Logic Apps][compare].

* Implementieren Sie diese Architektur mit angepassten KI-Komponenten anstatt mit bestehenden Azure-Diensten. Erweitern Sie beispielsweise die Pipeline um ein benutzerdefiniertes Modell, das bestimmte Personen in einem Bild erkennt, im Gegensatz zu den in diesem Beispiel gesammelten allgemeinen Daten zu Anzahl, Geschlecht und Alter von Personen. Um angepasste Machine Learning- oder KI-Modelle mit dieser Architektur zu verwenden, erstellen Sie die Modelle als RESTful-Endpunkte, damit sie von Azure Functions aufgerufen werden können.

* Verwenden Sie anstelle von RSS-Feeds einen anderen Eingabemechanismus. Nutzen Sie mehrere Generatoren oder Erfassungsprozesse, um Daten in Azure Cosmos DB und Azure Storage zu übertragen.

## <a name="considerations"></a>Überlegungen

Der Einfachheit halber werden in diesem Beispielszenario nur einige der verfügbaren APIs und Dienste von Azure Cognitive Services verwendet. So kann beispielsweise Text in Bildern mit der [Textanalyse-API][text-analytics] analysiert werden. Als Zielsprache wird in diesem Szenario Englisch angenommen, aber Sie können die Eingabe in eine beliebige [unterstützte Sprache][language] Ihrer Wahl ändern.

### <a name="scalability"></a>Skalierbarkeit

Die Skalierung von Azure Functions hängt von dem von Ihnen verwendeten [Hostingplan][plan] ab. Diese Lösung geht von einem [Verbrauchstarif][plan-c] aus, bei dem den Funktionen bei Bedarf automatisch Rechenleistung zugewiesen wird. Kosten fallen nur an, wenn Ihre Funktionen ausgeführt werden. Eine weitere Option ist ein [Azure App Service][plan-aas]-Plan, mit dem Sie zwischen Tarifen skalieren können, um eine unterschiedliche Menge von Ressourcen zu verteilen.

Der wichtigste Punkt bei Azure Cosmos DB ist, dass Ihre Workload zu ungefähr gleichen Teilen auf eine ausreichend große Anzahl von [Partitionsschlüsseln][keys] verteilt wird. Es gibt keine Begrenzung der Gesamtdatenmenge, die ein Container speichern kann, oder des gesamten [ Durchsatzes][throughput], den ein Container unterstützen kann.

### <a name="management-and-logging"></a>Verwaltung und Protokollierung

Diese Lösung nutzt [Application Insights][aai], um Leistungs- und Protokollierungsinformationen zu sammeln. Eine Instanz von Application Insights wird erstellt, wobei die Bereitstellung in der Ressourcengruppe erfolgt, die den anderen für diese Bereitstellung benötigten Diensten entspricht.

So zeigen Sie die von der Lösung generierten Protokolle an

1. Navigieren Sie im [Azure-Portal][portal] zur Ressourcengruppe, die Sie für die Bereitstellung erstellt haben.

2. Klicken Sie auf die **Application Insights**-Instanz.

3. Navigieren Sie im Abschnitt **Application Insights** zu **Untersuchen\\Suchen**, und durchsuchen Sie die Daten.

### <a name="security"></a>Sicherheit

Azure Cosmos DB verwendet über das von Microsoft bereitgestellte C\# SDK eine geschützte Verbindung und SAS-Signatur. Es gibt keine weiteren extern zugänglichen Oberflächenbereiche. Erfahren Sie mehr über [bewährte Methoden][db-practices] für die Sicherheit von Azure Cosmos DB.

## <a name="pricing"></a>Preise

Die geschätzten täglichen Kosten, um diese Bereitstellung verfügbar zu halten, liegen bei etwa 20 US-Dollar, ohne dass Daten das System durchlaufen.

Azure Cosmos DB ist leistungsstark, verursacht aber die meisten [Kosten][db-cost] in dieser Bereitstellung. Sie können eine andere Speicherlösung verwenden, indem Sie den bereitgestellten Azure Functions-Code umgestalten.

Die Preise für Azure Functions variieren je nach verwendetem [Plan][function-plan].

## <a name="deploy-the-scenario"></a>Bereitstellen des Szenarios

> [!NOTE]
> Sie benötigen ein bestehendes Azure-Konto. Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto][free] erstellen, bevor Sie beginnen.

Der gesamte Code für dieses Szenario steht im [GitHub][github]-Repository zur Verfügung. Dieses Repository enthält den Quellcode, der zum Erstellen der Generatoranwendung verwendet wird, die die Pipeline für diese Demo mit Daten versorgt.

[architecture]: ./media/mass-ingestion-newsfeeds-architecture.png
[aai]: /azure/azure-monitor/app/app-insights-overview
[aas]: https://azure.microsoft.com/try/app-service/
[acs]: https://azure.microsoft.com/services/cognitive-services/directory/
[collection]: /rest/api/cosmos-db/collections
[compare]: /azure/azure-functions/functions-compare-logic-apps-ms-flow-webjobs#compare-azure-functions-and-azure-logic-apps
[cosmos-db]: /azure/cosmos-db/introduction
[db-cost]: https://azure.microsoft.com/pricing/details/cosmos-db/
[db-practices]: /azure/cosmos-db/database-security
[db-collection]: /azure/cosmos-db/databases-containers-items
[english]: https://www.nasa.gov/rss/dyn/breaking_news.rss
[face]: /azure/cognitive-services/face/overview
[free]: https://azure.microsoft.com/free/?WT.mc_id=A261C142F
[functions]: /azure/azure-functions/functions-overview
[function-plan]: /azure/azure-functions/functions-scale
[german]: http://www.bamf.de/SiteGlobals/Functions/RSS/DE/Feed/RSSNewsfeed_Meldungen
[github]: https://github.com/Azure/cognitive-services
[keys]: /azure/cosmos-db/partition-data
[language]: /azure/cognitive-services/translator/reference/v3-0-languages
[logic-app]: /azure/logic-apps/logic-apps-overview
[queues-topics]: /azure/service-bus-messaging/service-bus-queues-topics-subscriptions
[partial]: https://feedback.azure.com/forums/263030-azure-cosmos-db/suggestions/6693091-be-able-to-do-partial-updates-on-document
[plan]: /azure/azure-functions/functions-scale
[plan-aas]: /azure/azure-functions/functions-scale#app-service-plan
[plan-c]: /azure/azure-functions/functions-scale#consumption-plan
[portal]: http://portal.azure.com
[redlock]: https://redis.io/topics/distlock
[russian]: http://government.ru/all/rss/
[service-bus]: /azure/service-bus-messaging/
[storage]: /azure/storage/common/storage-account-overview 
[throughput]: /azure/cosmos-db/scaling-throughput
[topics]: /azure/service-bus-messaging/service-bus-dotnet-how-to-use-topics-subscriptions
[text-analytics]: /azure/cognitive-services/text-analytics/
[translate-text]: /azure/cognitive-services/translator/translator-info-overview
[vision]: /azure/cognitive-services/computer-vision/home
