---
title: Bildklassifizierung für Versicherungsansprüche in Azure
description: Bewährtes Szenario zur Integration einer Bildverarbeitung in Ihre Azure-Anwendungen.
author: david-stanford
ms.date: 07/05/2018
ms.openlocfilehash: 361a88234fd9ed918ab7664893f86666b4328b8c
ms.sourcegitcommit: 71cbef121c40ef36e2d6e3a088cb85c4260599b9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/14/2018
ms.locfileid: "39060828"
---
# <a name="image-classification-for-insurance-claims-on-azure"></a><span data-ttu-id="2338b-103">Bildklassifizierung für Versicherungsansprüche in Azure</span><span class="sxs-lookup"><span data-stu-id="2338b-103">Image classification for insurance claims on Azure</span></span>

<span data-ttu-id="2338b-104">Dieses Beispielszenario richtet sich an Unternehmen, die eine Bildverarbeitung benötigen.</span><span class="sxs-lookup"><span data-stu-id="2338b-104">This example scenario is applicable for businesses that need to process images.</span></span>

<span data-ttu-id="2338b-105">Mögliche Anwendungsbereiche wären etwa die Klassifizierung von Bildern für eine Modewebsite, die Analyse von Text und Bildern für Versicherungsansprüche oder die Interpretation von Telemetriedaten aus Screenshots von Spielen.</span><span class="sxs-lookup"><span data-stu-id="2338b-105">Potential applications include classifying images for a fashion website, analyzing text and images for insurance claims, or understanding telemetry data from game screenshots.</span></span> <span data-ttu-id="2338b-106">In der Vergangenheit mussten sich Unternehmen in der Regel ausführlich mit Machine Learning-Modellen vertraut machen, die Modelle trainieren und die Bilder schließlich durch ihren benutzerdefinierten Prozess schleusen, um die Daten aus den Bildern zu extrahieren.</span><span class="sxs-lookup"><span data-stu-id="2338b-106">Traditionally, companies would need to develop expertise in machine learning models, train the models, and finally run the images through their custom process to get the data out of the images.</span></span>

<span data-ttu-id="2338b-107">Durch die Verwendung von Azure-Diensten wie Maschinelles Sehen-API und Azure Functions müssen Unternehmen keine einzelnen Server mehr verwalten und können gleichzeitig ihre Kosten senken und von der Erfahrung profitieren, über die Microsoft im Bereich der Bildverarbeitung mit Cognitive Services verfügt.</span><span class="sxs-lookup"><span data-stu-id="2338b-107">By using Azure services such as the Computer Vision API and Azure Functions, companies can eliminate the need to manage individual servers, while reducing costs and leveraging the expertise that Microsoft has already developed around processing images with Cognitive services.</span></span> <span data-ttu-id="2338b-108">In diesem Szenario geht es speziell um die Bildverarbeitung.</span><span class="sxs-lookup"><span data-stu-id="2338b-108">This scenario specifically addresses an image processing scenario.</span></span> <span data-ttu-id="2338b-109">Sollten Sie andere KI-Anforderungen haben, ziehen Sie ggf. die Verwendung der vollständigen Suite von [Cognitive Services][cognitive-docs] in Betracht.</span><span class="sxs-lookup"><span data-stu-id="2338b-109">If you have different AI needs, consider the full suite of [Cognitive Services][cognitive-docs].</span></span>

## <a name="related-use-cases"></a><span data-ttu-id="2338b-110">Verwandte Anwendungsfälle</span><span class="sxs-lookup"><span data-stu-id="2338b-110">Related use cases</span></span>

<span data-ttu-id="2338b-111">Erwägen Sie dieses Szenario für folgende Anwendungsfälle:</span><span class="sxs-lookup"><span data-stu-id="2338b-111">Consider this scenario for the following use cases:</span></span>

* <span data-ttu-id="2338b-112">Klassifizieren von Bildern auf einer Modewebsite</span><span class="sxs-lookup"><span data-stu-id="2338b-112">Classify images on a fashion website.</span></span>
* <span data-ttu-id="2338b-113">Klassifizieren von Telemetriedaten aus Screenshots von Spielen</span><span class="sxs-lookup"><span data-stu-id="2338b-113">Classify telemetry data from screenshots of games.</span></span>

## <a name="architecture"></a><span data-ttu-id="2338b-114">Architektur</span><span class="sxs-lookup"><span data-stu-id="2338b-114">Architecture</span></span>

![Architektur für intelligente Apps: maschinelles Sehen][architecture-computer-vision]

<span data-ttu-id="2338b-116">Dieses Szenario umfasst die Back-End-Komponenten einer webbasierten oder mobilen Anwendung.</span><span class="sxs-lookup"><span data-stu-id="2338b-116">This scenario covers the back-end components of a web or mobile application.</span></span> <span data-ttu-id="2338b-117">Die Daten durchlaufen das Szenario wie folgt:</span><span class="sxs-lookup"><span data-stu-id="2338b-117">Data flows through the scenario as follows:</span></span>

1. <span data-ttu-id="2338b-118">Azure Functions fungiert als API-Schicht.</span><span class="sxs-lookup"><span data-stu-id="2338b-118">Azure Functions acts as the API layer.</span></span> <span data-ttu-id="2338b-119">Diese APIs ermöglichen es der Anwendung, Bilder hochzuladen und Daten aus Cosmos DB abzurufen.</span><span class="sxs-lookup"><span data-stu-id="2338b-119">These APIs enable the application to upload images and retrieve data from Cosmos DB.</span></span>

2. <span data-ttu-id="2338b-120">Wenn ein Bild über einen API-Aufruf hochgeladen wird, wird es in Blob Storage gespeichert.</span><span class="sxs-lookup"><span data-stu-id="2338b-120">When an image is uploaded via an API call, it's stored in Blob storage.</span></span>

3. <span data-ttu-id="2338b-121">Das Hinzufügen von neuen Dateien zu Blob Storage löst eine Event Grid-Benachrichtigung aus, die an eine Azure-Funktion gesendet wird.</span><span class="sxs-lookup"><span data-stu-id="2338b-121">Adding new files to Blob storage triggers an EventGrid notification to be sent to an Azure Function.</span></span>

4. <span data-ttu-id="2338b-122">Azure Functions sendet einen Link für die neu hochgeladene Datei zur Analyse an die Maschinelles Sehen-API.</span><span class="sxs-lookup"><span data-stu-id="2338b-122">Azure Functions sends a link to the newly uploaded file to the Computer Vision API to analyze.</span></span>

5. <span data-ttu-id="2338b-123">Nachdem die Daten von der Maschinelles Sehen-API zurückgegeben wurden, erstellt Azure Functions einen Eintrag in Cosmos DB, um die Ergebnisse der Analyse zusammen mit den Bildmetadaten zu speichern.</span><span class="sxs-lookup"><span data-stu-id="2338b-123">Once the data has been returned from the Computer Vision API, Azure Functions makes an entry in Cosmos DB to persist the results of the analysis alongside the image metadata.</span></span>

### <a name="components"></a><span data-ttu-id="2338b-124">Komponenten</span><span class="sxs-lookup"><span data-stu-id="2338b-124">Components</span></span>

* <span data-ttu-id="2338b-125">[Maschinelles Sehen-API][computer-vision-docs] ist Teil der Cognitive Services-Suite und dient zum Abrufen von Informationen zu den einzelnen Bildern.</span><span class="sxs-lookup"><span data-stu-id="2338b-125">[Computer Vision API][computer-vision-docs] is part of the Cognitive Services suite and is used to retrieve information about each image.</span></span>

* <span data-ttu-id="2338b-126">[Azure Functions][functions-docs] stellt die Back-End-API für die Webanwendung sowie die Ereignisverarbeitung für hochgeladene Bilder bereit.</span><span class="sxs-lookup"><span data-stu-id="2338b-126">[Azure Functions][functions-docs] provides the backend API for the web application, as well as the event processing for uploaded images.</span></span>

* <span data-ttu-id="2338b-127">[Event Grid][eventgrid-docs] löst ein Ereignis aus, wenn ein neues Bild in Blob Storage hochgeladen wird.</span><span class="sxs-lookup"><span data-stu-id="2338b-127">[Event Grid][eventgrid-docs] triggers an event when a new image is uploaded to blob storage.</span></span> <span data-ttu-id="2338b-128">Das Bild wird dann mit Azure Functions verarbeitet.</span><span class="sxs-lookup"><span data-stu-id="2338b-128">The image is then processed with Azure functions.</span></span>

* <span data-ttu-id="2338b-129">[Blob Storage][storage-docs] speichert alle in die Webanwendung hochgeladenen Bilddateien sowie alle statischen Dateien, die von der Webanwendung genutzt werden.</span><span class="sxs-lookup"><span data-stu-id="2338b-129">[Blob Storage][storage-docs] stores all of the image files that are uploaded into the web application, as well any static files that the web application consumes.</span></span>

* <span data-ttu-id="2338b-130">[Cosmos DB][cosmos-docs] speichert Metadaten zu den einzelnen hochgeladenen Bildern sowie die Verarbeitungsergebnisse der Maschinelles Sehen-API.</span><span class="sxs-lookup"><span data-stu-id="2338b-130">[Cosmos DB][cosmos-docs] stores metadata about each image that is uploaded, including the results of the processing from Computer Vision API.</span></span>

## <a name="alternatives"></a><span data-ttu-id="2338b-131">Alternativen</span><span class="sxs-lookup"><span data-stu-id="2338b-131">Alternatives</span></span>

* <span data-ttu-id="2338b-132">[Custom Vision Service:][custom-vision-docs]</span><span class="sxs-lookup"><span data-stu-id="2338b-132">[Custom Vision Service][custom-vision-docs].</span></span> <span data-ttu-id="2338b-133">Die Maschinelles Sehen-API gibt eine Reihe [taxonomiebasierter Kategorien][cv-categories] zurück.</span><span class="sxs-lookup"><span data-stu-id="2338b-133">The Computer Vision API returns a set of [taxonomy-based categories][cv-categories].</span></span> <span data-ttu-id="2338b-134">Falls Sie Informationen verarbeiten müssen, die nicht von der Maschinelles Sehen-API zurückgegeben werden, ziehen Sie ggf. die Verwendung von Custom Vision Service in Betracht. Dieser Dienst ermöglicht die Erstellung benutzerdefinierter Bildklassifizierungen.</span><span class="sxs-lookup"><span data-stu-id="2338b-134">If you need to process information that isn't returned by the Computer Vision API, consider the Custom Vision Service, which lets you build custom image classifiers.</span></span>

* <span data-ttu-id="2338b-135">[Azure Search:][azure-search-docs]</span><span class="sxs-lookup"><span data-stu-id="2338b-135">[Azure Search][azure-search-docs].</span></span> <span data-ttu-id="2338b-136">Wenn in Ihrem Szenario die Metadaten abgefragt werden müssen, um Bilder zu finden, die bestimmten Kriterien entsprechen, empfiehlt sich ggf. die Verwendung von Azure Search.</span><span class="sxs-lookup"><span data-stu-id="2338b-136">If your use case involves querying the metadata to find images that meet specific criteria, consider using Azure Search.</span></span> <span data-ttu-id="2338b-137">[Cognitive Search][cognitive-search] (momentan in der Vorschauphase) ermöglicht die nahtlose Integration dieses Workflows.</span><span class="sxs-lookup"><span data-stu-id="2338b-137">Currently in preview, [Cognitive search][cognitive-search] seamlessly integrates this workflow.</span></span>

## <a name="considerations"></a><span data-ttu-id="2338b-138">Überlegungen</span><span class="sxs-lookup"><span data-stu-id="2338b-138">Considerations</span></span>

### <a name="scalability"></a><span data-ttu-id="2338b-139">Skalierbarkeit</span><span class="sxs-lookup"><span data-stu-id="2338b-139">Scalability</span></span>

<span data-ttu-id="2338b-140">Bei den Komponenten dieses Szenarios handelt es sich größtenteils um verwaltete Dienste mit automatischer Skalierung.</span><span class="sxs-lookup"><span data-stu-id="2338b-140">For the most part all of the components of this scenario are managed services that will automatically scale.</span></span> <span data-ttu-id="2338b-141">Es gibt jedoch ein paar Ausnahmen: Die maximale Anzahl von Azure Functions-Instanzen ist auf 200 beschränkt.</span><span class="sxs-lookup"><span data-stu-id="2338b-141">A couple notable exceptions: Azure Functions has a limit of a maximum of 200 instances.</span></span> <span data-ttu-id="2338b-142">Sollten Ihre Skalierungsanforderungen über diesen Grenzwert hinausgehen, erwägen Sie die Verwendung mehrerer Regionen oder App-Pläne.</span><span class="sxs-lookup"><span data-stu-id="2338b-142">If you need to scale beyond, consider multiple regions or app plans.</span></span>

<span data-ttu-id="2338b-143">Bei Cosmos DB erfolgt keine automatische Skalierung der bereitgestellten Anforderungseinheiten (Request Units, RUs).</span><span class="sxs-lookup"><span data-stu-id="2338b-143">Cosmos DB doesn’t auto-scale in terms of provisioned request units (RUs).</span></span>  <span data-ttu-id="2338b-144">Einen Leitfaden für die Schätzung Ihrer Anforderungen finden Sie in unserer Dokumentation unter [Anforderungseinheiten][request-units].</span><span class="sxs-lookup"><span data-stu-id="2338b-144">For guidance on estimating your requirements see [request units][request-units] in our documentation.</span></span> <span data-ttu-id="2338b-145">Zur optimalen Nutzung der Skalierung in Cosmos DB empfiehlt sich zudem ein Blick auf [Partitionsschlüssel][partition-key].</span><span class="sxs-lookup"><span data-stu-id="2338b-145">To fully take advantage of the scaling in Cosmos DB you should also take a look at [partition keys][partition-key].</span></span>

<span data-ttu-id="2338b-146">Bei NoSQL-Datenbanken gehen Verfügbarkeit, Skalierbarkeit und Partitionierung häufig zulasten der Konsistenz (im Sinne des CAP-Theorems).</span><span class="sxs-lookup"><span data-stu-id="2338b-146">NoSQL databases frequently trade consistency (in the sense of the CAP theorem) for availability, scalability and partition.</span></span>  <span data-ttu-id="2338b-147">Im Falle von Schlüssel-Wert-Datenmodellen, wie sie in diesem Szenario verwendet werden, kann die Transaktionskonsistenz meist vernachlässigt werden, da die meisten Vorgänge per Definition atomisch sind.</span><span class="sxs-lookup"><span data-stu-id="2338b-147">However, in the case of key-value data models which is used in this scenario, transaction consistency is rarely needed as most operations are by definition atomic.</span></span> <span data-ttu-id="2338b-148">Weitere Informationen zur [Wahl des richtigen Datenspeichers](../../guide/technology-choices/data-store-overview.md) finden Sie im Architecture Center.</span><span class="sxs-lookup"><span data-stu-id="2338b-148">Additional guidance to [Choose the right data store](../../guide/technology-choices/data-store-overview.md) is available in the architecture center.</span></span>

<span data-ttu-id="2338b-149">Allgemeine Informationen zur Entwicklung skalierbarer Lösungen finden Sie im Azure Architecture Center in der [Checkliste für die Skalierbarkeit][scalability].</span><span class="sxs-lookup"><span data-stu-id="2338b-149">For general guidance on designing scalable solutions, see the [scalability checklist][scalability] in the Azure Architecture Center.</span></span>

### <a name="security"></a><span data-ttu-id="2338b-150">Sicherheit</span><span class="sxs-lookup"><span data-stu-id="2338b-150">Security</span></span>

<span data-ttu-id="2338b-151">[Verwaltete Dienstidentitäten][msi] (Managed Service Identities, MSIs) ermöglichen den Zugriff auf andere interne Ressourcen Ihres Kontos und werden Ihren Azure-Funktionen zugewiesen.</span><span class="sxs-lookup"><span data-stu-id="2338b-151">[Managed service identities][msi] (MSI) are used to provide access to other resources internal to your account and then assigned to your Azure Functions.</span></span> <span data-ttu-id="2338b-152">Lassen Sie nur den Zugriff auf die erforderlichen Ressourcen in diesen Identitäten zu, um sicherzustellen, dass für Ihre Funktionen (und möglicherweise für Ihre Kunden) keine zusätzlichen Elemente verfügbar gemacht werden.</span><span class="sxs-lookup"><span data-stu-id="2338b-152">Only allow access to the requisite resources in those identities to ensure that nothing extra is exposed to your functions (and potentially to your customers).</span></span>  

<span data-ttu-id="2338b-153">Allgemeine Informationen zur Entwicklung sicherer Lösungen finden Sie in der [Dokumentation zur Azure-Sicherheit][security].</span><span class="sxs-lookup"><span data-stu-id="2338b-153">For general guidance on designing secure solutions, see the [Azure Security Documentation][security].</span></span>

### <a name="resiliency"></a><span data-ttu-id="2338b-154">Resilienz</span><span class="sxs-lookup"><span data-stu-id="2338b-154">Resiliency</span></span>

<span data-ttu-id="2338b-155">Da es sich bei allen Komponenten in diesem Szenario um verwaltete Komponenten handelt, ist deren Resilienz auf regionaler Ebene automatisch gewährleistet.</span><span class="sxs-lookup"><span data-stu-id="2338b-155">All of the components in this scenario are managed, so at a regional level they are all resilient automatically.</span></span>

<span data-ttu-id="2338b-156">Allgemeine Informationen zur Entwicklung robuster Lösungen finden Sie unter [Entwerfen robuster Anwendungen für Azure][resiliency].</span><span class="sxs-lookup"><span data-stu-id="2338b-156">For general guidance on designing resilient solutions, see [Designing resilient applications for Azure][resiliency].</span></span>

## <a name="pricing"></a><span data-ttu-id="2338b-157">Preise</span><span class="sxs-lookup"><span data-stu-id="2338b-157">Pricing</span></span>

<span data-ttu-id="2338b-158">Zur Ermittlung der Betriebskosten für dieses Szenario sind alle Dienste im Kostenrechner vorkonfiguriert.</span><span class="sxs-lookup"><span data-stu-id="2338b-158">To explore the cost of running this scenario, all of the services are pre-configured in the cost calculator.</span></span> <span data-ttu-id="2338b-159">Wenn Sie wissen möchten, welche Kosten für Ihren spezifischen Anwendungsfall entstehen, passen Sie die entsprechenden Variablen an Ihren voraussichtlichen Datenverkehr an.</span><span class="sxs-lookup"><span data-stu-id="2338b-159">To see how the pricing would change for your particular use case, change the appropriate variables to match your expected traffic.</span></span>

<span data-ttu-id="2338b-160">Auf der Grundlage des Datenverkehrs (und unter der Annahme, dass alle Bilder 100 KB groß sind) haben wir drei exemplarische Kostenprofile erstellt:</span><span class="sxs-lookup"><span data-stu-id="2338b-160">We have provided three sample cost profiles based on amount of traffic (we assume all images are 100kb in size):</span></span>

* <span data-ttu-id="2338b-161">[Klein:][pricing] Verarbeitung von &lt; 5.000 Bildern pro Monat.</span><span class="sxs-lookup"><span data-stu-id="2338b-161">[Small][pricing]: this correlates to processing &lt; 5000 images a month.</span></span>
* <span data-ttu-id="2338b-162">[Mittel:][medium-pricing] Verarbeitung von 500.000 Bildern pro Monat.</span><span class="sxs-lookup"><span data-stu-id="2338b-162">[Medium][medium-pricing]: this correlates to processing 500,000 images a month.</span></span>
* <span data-ttu-id="2338b-163">[Groß:][large-pricing] Verarbeitung von 50 Millionen Bildern pro Monat.</span><span class="sxs-lookup"><span data-stu-id="2338b-163">[Large][large-pricing]: this correlates to processing 50 million images a month.</span></span>

## <a name="related-resources"></a><span data-ttu-id="2338b-164">Zugehörige Ressourcen</span><span class="sxs-lookup"><span data-stu-id="2338b-164">Related Resources</span></span>

<span data-ttu-id="2338b-165">Einen geführten Lernpfad für dieses Szenario finden Sie unter [Build a serverless web app in Azure][serverless] (Erstellen einer serverlosen Web-App in Azure).</span><span class="sxs-lookup"><span data-stu-id="2338b-165">For a guided learning path of this scenario, see [Build a serverless web app in Azure][serverless].</span></span>  

<span data-ttu-id="2338b-166">Machen Sie sich vor der Bereitstellung in einer Produktionsumgebung mit den [bewährten Methoden][functions-best-practices] für Azure Functions vertraut.</span><span class="sxs-lookup"><span data-stu-id="2338b-166">Before putting this in a production environment, review the Azure Functions [best practices][functions-best-practices].</span></span>

<!-- links -->
[pricing]: https://azure.com/e/f9b59d238b43423683db73f4a31dc380
[medium-pricing]: https://azure.com/e/7c7fc474db344b87aae93bc29ae27108
[large-pricing]: https://azure.com/e/cbadbca30f8640d6a061f8457a74ba7d
[functions-docs]: /azure/azure-functions/
[computer-vision-docs]: /azure/cognitive-services/computer-vision/home
[storage-docs]: /azure/storage/
[azure-search-docs]: /azure/search/
[cognitive-search]: /azure/search/cognitive-search-concept-intro
[architecture-computer-vision]: ./media/architecture-computer-vision.png
[serverless]: /azure/functions/tutorial-static-website-serverless-api-with-database
[cosmos-docs]: /azure/cosmos-db/
[eventgrid-docs]: /azure/event-grid/
[cognitive-docs]: /azure/#pivot=products&panel=ai
[custom-vision-docs]: /azure/cognitive-services/Custom-Vision-Service/home
[cv-categories]: /azure/cognitive-services/computer-vision/home#the-86-category-concept
[resiliency]: /azure/architecture/resiliency/
[security]: /azure/security/
[scalability]: /azure/architecture/checklist/scalability
[functions-best-practices]: /azure/azure-functions/functions-best-practices
[msi]: /azure/app-service/app-service-managed-service-identity
[request-units]: /azure/cosmos-db/request-units
[partition-key]: /azure/cosmos-db/partition-data