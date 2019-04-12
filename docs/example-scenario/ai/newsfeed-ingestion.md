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
# <a name="mass-ingestion-and-analysis-of-news-feeds-on-azure"></a><span data-ttu-id="5987c-103">Massenerfassung und -analyse von Newsfeeds in Azure</span><span class="sxs-lookup"><span data-stu-id="5987c-103">Mass ingestion and analysis of news feeds on Azure</span></span>

<span data-ttu-id="5987c-104">In diesem Beispielszenario wird eine Pipeline für die Massenerfassung und echtzeitnahe Analyse von Dokumenten unter Verwendung öffentlicher RSS-Newsfeeds beschrieben.</span><span class="sxs-lookup"><span data-stu-id="5987c-104">This example scenario describes a pipeline for mass ingestion and near real-time analysis of documents using public RSS news feeds.</span></span>  <span data-ttu-id="5987c-105">Dazu wird Azure Cognitive Services verwendet, um nützliche Erkenntnisse mithilfe von Textübersetzung, Gesichts- und Stimmungserkennung zu liefern.</span><span class="sxs-lookup"><span data-stu-id="5987c-105">It uses Azure Cognitive Services to offer useful insights including text translation, facial recognition, and sentiment detection.</span></span>

<span data-ttu-id="5987c-106">Dieses Szenario enthält Beispiele für Nachrichtenfeeds in [Englisch][english], [Russisch][russian] und [Deutsch][german], aber Sie können es ganz einfach auch für andere RSS-Feeds nutzen.</span><span class="sxs-lookup"><span data-stu-id="5987c-106">This scenario contains examples for [English][english], [Russian][russian], and [German][german] news feeds, but you can easily extend it to other RSS feeds.</span></span> <span data-ttu-id="5987c-107">Um die Bereitstellung zu erleichtern, basieren Erfassung, Verarbeitung und Analyse der Daten vollständig auf Azure-Diensten.</span><span class="sxs-lookup"><span data-stu-id="5987c-107">For ease of deployment, the data collection, processing, and analysis are based entirely on Azure services.</span></span>

## <a name="relevant-use-cases"></a><span data-ttu-id="5987c-108">Relevante Anwendungsfälle</span><span class="sxs-lookup"><span data-stu-id="5987c-108">Relevant use cases</span></span>

<span data-ttu-id="5987c-109">Obwohl dieses Szenario auf der Verarbeitung von RSS-Feeds basiert, ist es für alle Dokumente, Websites oder Artikel relevant, für die Folgendes gewünscht wird:</span><span class="sxs-lookup"><span data-stu-id="5987c-109">While this scenario is based on processing of RSS feeds, it's relevant to any document, website, or article where you would need to:</span></span>

* <span data-ttu-id="5987c-110">Übersetzen von Text in die Sprache Ihrer Wahl</span><span class="sxs-lookup"><span data-stu-id="5987c-110">Translate any text to the language of choice.</span></span>
* <span data-ttu-id="5987c-111">Bestimmen von Schlüsselbegriffen, Entitäten und Benutzerstimmung in digitalen Inhalten</span><span class="sxs-lookup"><span data-stu-id="5987c-111">Find key phrases, entities, and user sentiment in digital content.</span></span>
* <span data-ttu-id="5987c-112">Erkennen von Objekten, Text und Orientierungspunkten in Bildern, die mit einem digitalen Artikel verknüpft sind</span><span class="sxs-lookup"><span data-stu-id="5987c-112">Detect objects, text, and landmarks in images associated with a digital article.</span></span>
* <span data-ttu-id="5987c-113">Erkennen von Personen anhand ihres Geschlechts und Alters in Bildern in digitalen Inhalten</span><span class="sxs-lookup"><span data-stu-id="5987c-113">Detect people by their gender and age in any image associated with digital content.</span></span>

## <a name="architecture"></a><span data-ttu-id="5987c-114">Architecture</span><span class="sxs-lookup"><span data-stu-id="5987c-114">Architecture</span></span>

![][architecture]

<span data-ttu-id="5987c-115">Die Daten durchlaufen die Lösung wie folgt:</span><span class="sxs-lookup"><span data-stu-id="5987c-115">The data flows through the solution as follows:</span></span>

1. <span data-ttu-id="5987c-116">Ein RSS-Nachrichtenfeed fungiert als Generator, der Daten aus einem Dokument oder Artikel abruft.</span><span class="sxs-lookup"><span data-stu-id="5987c-116">An RSS news feed acts as the generator that obtains data from a document or article.</span></span> <span data-ttu-id="5987c-117">Beispielsweise enthalten Daten in einem Artikel typischerweise einen Titel, eine Zusammenfassung des Inhalts der Nachricht und mitunter Bilder.</span><span class="sxs-lookup"><span data-stu-id="5987c-117">For example, with an article, data typically includes a title, a summary of the original body of the news item, and sometimes images.</span></span>

2. <span data-ttu-id="5987c-118">Ein Generator- oder Erfassungsprozess fügt den Artikel und alle zugehörigen Bilder in eine Azure Cosmos DB-[Sammlung][collection] ein.</span><span class="sxs-lookup"><span data-stu-id="5987c-118">A generator or ingestion process inserts the article and any associated images into an Azure Cosmos DB [Collection][collection].</span></span>

3. <span data-ttu-id="5987c-119">Eine Benachrichtigung löst eine Erfassungsfunktion in Azure Functions aus, die den Artikeltext in CosmosDB und die Artikelbilder (falls vorhanden) in Azure Blob Storage speichert.</span><span class="sxs-lookup"><span data-stu-id="5987c-119">A notification triggers an ingest function in Azure Functions that stores the article text in CosmosDB and the article images (if any) in Azure Blob Storage.</span></span>  <span data-ttu-id="5987c-120">Der Artikel wird dann an die nächste Warteschlange übergeben.</span><span class="sxs-lookup"><span data-stu-id="5987c-120">The article is then passed to the next queue.</span></span>

4. <span data-ttu-id="5987c-121">Eine Übersetzungsfunktion wird durch das Warteschlangenereignis ausgelöst.</span><span class="sxs-lookup"><span data-stu-id="5987c-121">A translate function is triggered by the queue event.</span></span> <span data-ttu-id="5987c-122">Sie verwendet die [Textübersetzungs-API][translate-text] von Azure Cognitive Services, um die Sprache zu erkennen, den Text bei Bedarf zu übersetzen und die Stimmung, Schlüsselbegriffe und Entitäten in Text und Titel zu sammeln.</span><span class="sxs-lookup"><span data-stu-id="5987c-122">It uses the [Translate Text API][translate-text] of Azure Cognitive Services to detect the language, translate if necessary, and collect the sentiment, key phrases, and entities from the body and the title.</span></span> <span data-ttu-id="5987c-123">Der Artikel wird anschließend an die nächste Warteschlange übergeben.</span><span class="sxs-lookup"><span data-stu-id="5987c-123">Then it passes the article to the next queue.</span></span>

5. <span data-ttu-id="5987c-124">Im Artikel in der Warteschlange wird eine Erkennungsfunktion ausgelöst.</span><span class="sxs-lookup"><span data-stu-id="5987c-124">A detect function is triggered from the queued article.</span></span> <span data-ttu-id="5987c-125">Sie verwendet den Dienst [Maschinelles Sehen][vision], um Objekte, Orientierungspunkte und geschriebene Wörter im zugehörigen Bild zu erkennen, und leitet den Artikel dann an die nächste Warteschlange weiter.</span><span class="sxs-lookup"><span data-stu-id="5987c-125">It uses the [Computer Vision][vision] service to detect objects, landmarks, and written words in the associated image, then passes the article to the next queue.</span></span>

6. <span data-ttu-id="5987c-126">Im Artikel in der Warteschlange wird eine Gesichtserkennungsfunktion ausgelöst.</span><span class="sxs-lookup"><span data-stu-id="5987c-126">A face function is triggered is triggered from the queued article.</span></span> <span data-ttu-id="5987c-127">Sie verwendet die [Gesichtserkennungs-API von Azure][face], um Geschlecht und Alter von Gesichtern im zugehörigen Bild zu erkennen, und leitet den Artikel dann an die nächste Warteschlange weiter.</span><span class="sxs-lookup"><span data-stu-id="5987c-127">It uses the [Azure Face API][face] service to detect faces for gender and age in the associated image, then passes the article to the next queue.</span></span>

7. <span data-ttu-id="5987c-128">Wenn alle Funktionen abgeschlossen sind, wird die Benachrichtigungsfunktion ausgelöst.</span><span class="sxs-lookup"><span data-stu-id="5987c-128">When all functions are complete, the notify function is triggered.</span></span> <span data-ttu-id="5987c-129">Sie lädt die für den Artikel verarbeiteten Datensätzen und untersucht sie auf gewünschte Ergebnisse.</span><span class="sxs-lookup"><span data-stu-id="5987c-129">It loads the processed records for the article and scans them for any results you want.</span></span> <span data-ttu-id="5987c-130">Bei einem Fund wird der Inhalt markiert und eine Benachrichtigung an das System Ihrer Wahl gesendet.</span><span class="sxs-lookup"><span data-stu-id="5987c-130">If found, the content is flagged and a notification is sent to the system of your choice.</span></span>

<span data-ttu-id="5987c-131">Bei jedem Verarbeitungsschritt schreibt die Funktion die Ergebnisse in Azure Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="5987c-131">At each processing step, the function writes the results to Azure Cosmos DB.</span></span> <span data-ttu-id="5987c-132">Schließlich können die Daten nach Belieben verwendet werden.</span><span class="sxs-lookup"><span data-stu-id="5987c-132">Ultimately, the data can be used as desired.</span></span> <span data-ttu-id="5987c-133">Sie können damit beispielsweise Geschäftsprozesse verbessern, neue Kunden ausfindig machen oder Probleme mit der Kundenzufriedenheit aufdecken.</span><span class="sxs-lookup"><span data-stu-id="5987c-133">For example, you can use it to enhance business processes, locate new customers, or identify customer satisfaction issues.</span></span>

### <a name="components"></a><span data-ttu-id="5987c-134">Komponenten</span><span class="sxs-lookup"><span data-stu-id="5987c-134">Components</span></span>

<span data-ttu-id="5987c-135">Bei diesem Beispiel werden die Azure-Komponenten in der folgenden Liste verwendet.</span><span class="sxs-lookup"><span data-stu-id="5987c-135">The following list of Azure components is used in this example.</span></span>

* <span data-ttu-id="5987c-136">[Azure Storage][storage] dient zum Speichern von unbearbeiteten Bild- und Videodateien, die einem Artikel zugeordnet sind.</span><span class="sxs-lookup"><span data-stu-id="5987c-136">[Azure Storage][storage] is used to hold raw image and video files associated with an article.</span></span> <span data-ttu-id="5987c-137">Ein sekundäres Speicherkonto wird mit Azure App Service erstellt, das zum Hosten des Azure-Funktionscodes und von Protokollen dient.</span><span class="sxs-lookup"><span data-stu-id="5987c-137">A secondary storage account is created with Azure App Service and is used to host the Azure Function code and logs.</span></span>

* <span data-ttu-id="5987c-138">[Azure Cosmos DB][cosmos-db] enthält Nachverfolgungsinformationen zu Text, Bildern und Videos im Artikel.</span><span class="sxs-lookup"><span data-stu-id="5987c-138">[Azure Cosmos DB][cosmos-db] holds article text, image, and video tracking information.</span></span> <span data-ttu-id="5987c-139">Die Ergebnisse der Cognitive Services-Schritte werden ebenfalls hier gespeichert.</span><span class="sxs-lookup"><span data-stu-id="5987c-139">The results of the Cognitive Services steps are also stored here.</span></span>

* <span data-ttu-id="5987c-140">[Azure Functions][functions] führt den Funktionscode aus, der verwendet wird, um auf Warteschlangennachrichten zu reagieren und den eingehenden Inhalt zu transformieren.</span><span class="sxs-lookup"><span data-stu-id="5987c-140">[Azure Functions][functions] executes the function code used to respond to queue messages and transform the incoming content.</span></span> <span data-ttu-id="5987c-141">[Azure App Service][aas] hostet den Funktionscode und verarbeitet die Datensätze seriell.</span><span class="sxs-lookup"><span data-stu-id="5987c-141">[Azure App Service][aas] hosts the function code and processes the records serially.</span></span> <span data-ttu-id="5987c-142">Dieses Szenario umfasst fünf Funktionen: Erfassung, Transformation, Objekterkennung, Gesichtserkennung und Benachrichtigung.</span><span class="sxs-lookup"><span data-stu-id="5987c-142">This scenario includes five functions: Ingest, Transform, Detect Object, Face, and Notify.</span></span>

* <span data-ttu-id="5987c-143">[Azure Service Bus][service-bus] hostet die Azure Service Bus-Warteschlangen, die von den Funktionen verwendet werden.</span><span class="sxs-lookup"><span data-stu-id="5987c-143">[Azure Service Bus][service-bus] hosts the Azure Service Bus queues used by the functions.</span></span>

* <span data-ttu-id="5987c-144">[Azure Cognitive Services][acs] liefert die KI für die Pipeline basierend auf Implementierungen des Diensts [Maschinelles Sehen][vision], der [Gesichtserkennungs-API][face] und des maschinellen Übersetzungsdiensts [Text übersetzen ][translate-text].</span><span class="sxs-lookup"><span data-stu-id="5987c-144">[Azure Cognitive Services][acs] provides the AI for the pipeline based on implementations of the [Computer Vision][vision] service, [Face API][face], and [Translate Text][translate-text] machine translation service.</span></span>

* <span data-ttu-id="5987c-145">[Azure Application Insights][aai] bietet Analysen, die Ihnen helfen, Probleme zu diagnostizieren und die Funktionalität Ihrer Anwendung zu verstehen.</span><span class="sxs-lookup"><span data-stu-id="5987c-145">[Azure Application Insights][aai] provides analytics to help you diagnose issues and to understand functionality of your application.</span></span>

### <a name="alternatives"></a><span data-ttu-id="5987c-146">Alternativen</span><span class="sxs-lookup"><span data-stu-id="5987c-146">Alternatives</span></span>

* <span data-ttu-id="5987c-147">Anstatt ein Muster zu verwenden, das auf Warteschlangenbenachrichtigungen und Azure Functions basiert, können Sie für diesen Datenfluss ein anderes Muster verwenden.</span><span class="sxs-lookup"><span data-stu-id="5987c-147">Instead of using a pattern based on queue notification and Azure Functions, use another pattern for this data flow.</span></span> <span data-ttu-id="5987c-148">So können beispielsweise [Azure Service Bus-Themen][topics] verwendet werden, um die verschiedenen Teile des Artikels im Gegensatz zur in diesem Beispiel durchgeführten seriellen Verarbeitung parallel zu verarbeiten.</span><span class="sxs-lookup"><span data-stu-id="5987c-148">For example, [Azure Service Bus Topics][topics] can be used to processes the various parts of the article in parallel as opposed to the serial processing done in this example.</span></span> <span data-ttu-id="5987c-149">Weitere Informationen finden Sie unter [Warteschlangen und Themen][queues-topics].</span><span class="sxs-lookup"><span data-stu-id="5987c-149">For more information, compare [queues and topics][queues-topics].</span></span>

* <span data-ttu-id="5987c-150">Verwenden Sie [Azure Logic App][logic-app], um den Funktionscode zu implementieren, und implementieren Sie Sperren auf Datensatzebene wie [Redlock][redlock] (was für die Parallelverarbeitung erforderlich ist, bis Azure Cosmos DB [teilweise Dokumentaktualisierungen][partial] unterstützt).</span><span class="sxs-lookup"><span data-stu-id="5987c-150">Use [Azure Logic App][logic-app] to implement the function code and implement record-level locking such as [Redlock][redlock] (needed for parallel processing until Azure Cosmos DB supports [partial document updates][partial]).</span></span> <span data-ttu-id="5987c-151">Weitere Informationen finden Sie unter [Vergleich zwischen Azure Functions und Azure Logic Apps][compare].</span><span class="sxs-lookup"><span data-stu-id="5987c-151">For more information, [compare Functions and Logic Apps][compare].</span></span>

* <span data-ttu-id="5987c-152">Implementieren Sie diese Architektur mit angepassten KI-Komponenten anstatt mit bestehenden Azure-Diensten.</span><span class="sxs-lookup"><span data-stu-id="5987c-152">Implement this architecture using customized AI components rather than existing Azure services.</span></span> <span data-ttu-id="5987c-153">Erweitern Sie beispielsweise die Pipeline um ein benutzerdefiniertes Modell, das bestimmte Personen in einem Bild erkennt, im Gegensatz zu den in diesem Beispiel gesammelten allgemeinen Daten zu Anzahl, Geschlecht und Alter von Personen.</span><span class="sxs-lookup"><span data-stu-id="5987c-153">For example, extend the pipeline using a customized model that detects certain people in an image as opposed to the generic people count, gender, and age data collected in this example.</span></span> <span data-ttu-id="5987c-154">Um angepasste Machine Learning- oder KI-Modelle mit dieser Architektur zu verwenden, erstellen Sie die Modelle als RESTful-Endpunkte, damit sie von Azure Functions aufgerufen werden können.</span><span class="sxs-lookup"><span data-stu-id="5987c-154">To use customized machine learning or AI models with this architecture, build the models as RESTful endpoints so they can be called from Azure Functions.</span></span>

* <span data-ttu-id="5987c-155">Verwenden Sie anstelle von RSS-Feeds einen anderen Eingabemechanismus.</span><span class="sxs-lookup"><span data-stu-id="5987c-155">Use a different input mechanism instead of RSS feeds.</span></span> <span data-ttu-id="5987c-156">Nutzen Sie mehrere Generatoren oder Erfassungsprozesse, um Daten in Azure Cosmos DB und Azure Storage zu übertragen.</span><span class="sxs-lookup"><span data-stu-id="5987c-156">Use multiple generators or ingestion processes to feed Azure Cosmos DB and Azure Storage.</span></span>

## <a name="considerations"></a><span data-ttu-id="5987c-157">Überlegungen</span><span class="sxs-lookup"><span data-stu-id="5987c-157">Considerations</span></span>

<span data-ttu-id="5987c-158">Der Einfachheit halber werden in diesem Beispielszenario nur einige der verfügbaren APIs und Dienste von Azure Cognitive Services verwendet.</span><span class="sxs-lookup"><span data-stu-id="5987c-158">For simplicity, this example scenario uses only a few of the available APIs and services from Azure Cognitive Services.</span></span> <span data-ttu-id="5987c-159">So kann beispielsweise Text in Bildern mit der [Textanalyse-API][text-analytics] analysiert werden.</span><span class="sxs-lookup"><span data-stu-id="5987c-159">For example, text in images can be analyzed using the [Text Analytics API][text-analytics].</span></span> <span data-ttu-id="5987c-160">Als Zielsprache wird in diesem Szenario Englisch angenommen, aber Sie können die Eingabe in eine beliebige [unterstützte Sprache][language] Ihrer Wahl ändern.</span><span class="sxs-lookup"><span data-stu-id="5987c-160">The target language in this scenario is assumed to be English, but you can change the input to any [supported language][language] of your choice.</span></span>

### <a name="scalability"></a><span data-ttu-id="5987c-161">Skalierbarkeit</span><span class="sxs-lookup"><span data-stu-id="5987c-161">Scalability</span></span>

<span data-ttu-id="5987c-162">Die Skalierung von Azure Functions hängt von dem von Ihnen verwendeten [Hostingplan][plan] ab.</span><span class="sxs-lookup"><span data-stu-id="5987c-162">Azure Functions scaling depends on the [hosting plan][plan] you use.</span></span> <span data-ttu-id="5987c-163">Diese Lösung geht von einem [Verbrauchstarif][plan-c] aus, bei dem den Funktionen bei Bedarf automatisch Rechenleistung zugewiesen wird.</span><span class="sxs-lookup"><span data-stu-id="5987c-163">This solution assumes a [Consumption plan][plan-c], in which compute power is automatically allocated to the functions when required.</span></span> <span data-ttu-id="5987c-164">Kosten fallen nur an, wenn Ihre Funktionen ausgeführt werden.</span><span class="sxs-lookup"><span data-stu-id="5987c-164">You pay only when your functions are running.</span></span> <span data-ttu-id="5987c-165">Eine weitere Option ist ein [Azure App Service][plan-aas]-Plan, mit dem Sie zwischen Tarifen skalieren können, um eine unterschiedliche Menge von Ressourcen zu verteilen.</span><span class="sxs-lookup"><span data-stu-id="5987c-165">Another option is to use an [Azure App Service][plan-aas] plan, which allows you to scale between tiers to allocate a different amount of resources.</span></span>

<span data-ttu-id="5987c-166">Der wichtigste Punkt bei Azure Cosmos DB ist, dass Ihre Workload zu ungefähr gleichen Teilen auf eine ausreichend große Anzahl von [Partitionsschlüsseln][keys] verteilt wird.</span><span class="sxs-lookup"><span data-stu-id="5987c-166">With Azure Cosmos DB, the key is to distribute your workload roughly evenly among a sufficiently large number of [partition keys][keys].</span></span> <span data-ttu-id="5987c-167">Es gibt keine Begrenzung der Gesamtdatenmenge, die ein Container speichern kann, oder des gesamten [ Durchsatzes][throughput], den ein Container unterstützen kann.</span><span class="sxs-lookup"><span data-stu-id="5987c-167">There's no limit to the total amount of data that a container can store or to the total amount of [throughput][throughput] that a container can support.</span></span>

### <a name="management-and-logging"></a><span data-ttu-id="5987c-168">Verwaltung und Protokollierung</span><span class="sxs-lookup"><span data-stu-id="5987c-168">Management and logging</span></span>

<span data-ttu-id="5987c-169">Diese Lösung nutzt [Application Insights][aai], um Leistungs- und Protokollierungsinformationen zu sammeln.</span><span class="sxs-lookup"><span data-stu-id="5987c-169">This solution uses [Application Insights][aai] to collect performance and logging information.</span></span> <span data-ttu-id="5987c-170">Eine Instanz von Application Insights wird erstellt, wobei die Bereitstellung in der Ressourcengruppe erfolgt, die den anderen für diese Bereitstellung benötigten Diensten entspricht.</span><span class="sxs-lookup"><span data-stu-id="5987c-170">An instance of Application Insights is created with the deployment in the same resource group as the other services needed for this deployment.</span></span>

<span data-ttu-id="5987c-171">So zeigen Sie die von der Lösung generierten Protokolle an</span><span class="sxs-lookup"><span data-stu-id="5987c-171">To view the logs generated by the solution:</span></span>

1. <span data-ttu-id="5987c-172">Navigieren Sie im [Azure-Portal][portal] zur Ressourcengruppe, die Sie für die Bereitstellung erstellt haben.</span><span class="sxs-lookup"><span data-stu-id="5987c-172">Go to [Azure portal][portal] and navigate to the resource group created for the deployment.</span></span>

2. <span data-ttu-id="5987c-173">Klicken Sie auf die **Application Insights**-Instanz.</span><span class="sxs-lookup"><span data-stu-id="5987c-173">Click the **Application Insights** instance.</span></span>

3. <span data-ttu-id="5987c-174">Navigieren Sie im Abschnitt **Application Insights** zu **Untersuchen\\Suchen**, und durchsuchen Sie die Daten.</span><span class="sxs-lookup"><span data-stu-id="5987c-174">From the **Application Insights** section, navigate to **Investigate\\Search** and search the data.</span></span>

### <a name="security"></a><span data-ttu-id="5987c-175">Sicherheit</span><span class="sxs-lookup"><span data-stu-id="5987c-175">Security</span></span>

<span data-ttu-id="5987c-176">Azure Cosmos DB verwendet über das von Microsoft bereitgestellte C\# SDK eine geschützte Verbindung und SAS-Signatur.</span><span class="sxs-lookup"><span data-stu-id="5987c-176">Azure Cosmos DB uses a secured connection and shared access signature through the C\# SDK provided by Microsoft.</span></span> <span data-ttu-id="5987c-177">Es gibt keine weiteren extern zugänglichen Oberflächenbereiche.</span><span class="sxs-lookup"><span data-stu-id="5987c-177">There are no other externally facing surface areas.</span></span> <span data-ttu-id="5987c-178">Erfahren Sie mehr über [bewährte Methoden][db-practices] für die Sicherheit von Azure Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="5987c-178">Learn more about security [best practices][db-practices] for Azure Cosmos DB.</span></span>

## <a name="pricing"></a><span data-ttu-id="5987c-179">Preise</span><span class="sxs-lookup"><span data-stu-id="5987c-179">Pricing</span></span>

<span data-ttu-id="5987c-180">Die geschätzten täglichen Kosten, um diese Bereitstellung verfügbar zu halten, liegen bei etwa 20 US-Dollar, ohne dass Daten das System durchlaufen.</span><span class="sxs-lookup"><span data-stu-id="5987c-180">The estimated daily cost to keep this deployment available is approximately \$20 U.S. with no data moving through the system.</span></span>

<span data-ttu-id="5987c-181">Azure Cosmos DB ist leistungsstark, verursacht aber die meisten [Kosten][db-cost] in dieser Bereitstellung.</span><span class="sxs-lookup"><span data-stu-id="5987c-181">Azure Cosmos DB is powerful but incurs the greatest [cost][db-cost] in this deployment.</span></span> <span data-ttu-id="5987c-182">Sie können eine andere Speicherlösung verwenden, indem Sie den bereitgestellten Azure Functions-Code umgestalten.</span><span class="sxs-lookup"><span data-stu-id="5987c-182">You can use another storage solution by refactoring the Azure Functions code provided.</span></span>

<span data-ttu-id="5987c-183">Die Preise für Azure Functions variieren je nach verwendetem [Plan][function-plan].</span><span class="sxs-lookup"><span data-stu-id="5987c-183">Pricing for Azure Functions varies depending on the [plan][function-plan] it runs in.</span></span>

## <a name="deploy-the-scenario"></a><span data-ttu-id="5987c-184">Bereitstellen des Szenarios</span><span class="sxs-lookup"><span data-stu-id="5987c-184">Deploy the scenario</span></span>

> [!NOTE]
> <span data-ttu-id="5987c-185">Sie benötigen ein bestehendes Azure-Konto.</span><span class="sxs-lookup"><span data-stu-id="5987c-185">You must have an existing Azure account.</span></span> <span data-ttu-id="5987c-186">Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto][free] erstellen, bevor Sie beginnen.</span><span class="sxs-lookup"><span data-stu-id="5987c-186">If you don't have an Azure subscription, create a [free account][free] before you begin.</span></span>

<span data-ttu-id="5987c-187">Der gesamte Code für dieses Szenario steht im [GitHub][github]-Repository zur Verfügung.</span><span class="sxs-lookup"><span data-stu-id="5987c-187">All the code for this scenario is available in the [GitHub][github] repository.</span></span> <span data-ttu-id="5987c-188">Dieses Repository enthält den Quellcode, der zum Erstellen der Generatoranwendung verwendet wird, die die Pipeline für diese Demo mit Daten versorgt.</span><span class="sxs-lookup"><span data-stu-id="5987c-188">This repository contains the source code used to build the generator application that feeds the pipeline for this demo.</span></span>

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
