---
title: Architekturstil „Web-Warteschlange-Worker“
titleSuffix: Azure Application Architecture Guide
description: In diesem Artikel werden die Vorteile, Herausforderungen und bewährten Methoden für Architekturen vom Typ „Web-Warteschlange-Worker“ in Azure beschrieben.
author: MikeWasson
ms.date: 04/10/2019
ms.topic: guide
ms.service: architecture-center
ms.subservice: reference-architecture
ms.custom: seojan19
ms.openlocfilehash: 974b8b8595d6d9333552c41dfe1f3f2af848d264
ms.sourcegitcommit: 579c39ff4b776704ead17a006bf24cd4cdc65edd
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/18/2019
ms.locfileid: "59740390"
---
# <a name="web-queue-worker-architecture-style"></a><span data-ttu-id="88482-103">Architekturstil „Web-Warteschlange-Worker“</span><span class="sxs-lookup"><span data-stu-id="88482-103">Web-Queue-Worker architecture style</span></span>

<span data-ttu-id="88482-104">Die Hauptkomponenten dieser Architektur sind ein **Web-Front-End**, mit dem Clientanforderungen bereitgestellt werden, und ein **Worker**, der ressourcenintensive Aufgaben, Workflows mit langer Ausführungsdauer oder Batchaufträge durchführt.</span><span class="sxs-lookup"><span data-stu-id="88482-104">The core components of this architecture are a **web front end** that serves client requests, and a **worker** that performs resource-intensive tasks, long-running workflows, or batch jobs.</span></span>  <span data-ttu-id="88482-105">Das Web-Front-End kommuniziert mit dem Worker über eine **Nachrichtenwarteschlange**.</span><span class="sxs-lookup"><span data-stu-id="88482-105">The web front end communicates with the worker through a **message queue**.</span></span>

![Logisches Diagramm der Architektur „Web-Warteschlange-Worker“](./images/web-queue-worker-logical.svg)

<span data-ttu-id="88482-107">Andere Komponenten, die häufig in diese Architektur eingebunden werden, sind:</span><span class="sxs-lookup"><span data-stu-id="88482-107">Other components that are commonly incorporated into this architecture include:</span></span>

- <span data-ttu-id="88482-108">Eine oder mehrere Datenbanken</span><span class="sxs-lookup"><span data-stu-id="88482-108">One or more databases.</span></span>
- <span data-ttu-id="88482-109">Ein Cache zum Speichern von Werten aus der Datenbank, um schnelle Lesevorgänge zu ermöglichen</span><span class="sxs-lookup"><span data-stu-id="88482-109">A cache to store values from the database for quick reads.</span></span>
- <span data-ttu-id="88482-110">CDN zur Bereitstellung von statischem Inhalt</span><span class="sxs-lookup"><span data-stu-id="88482-110">CDN to serve static content</span></span>
- <span data-ttu-id="88482-111">Remotedienste, z.B. E-Mail- oder SMS-Dienst</span><span class="sxs-lookup"><span data-stu-id="88482-111">Remote services, such as email or SMS service.</span></span> <span data-ttu-id="88482-112">(häufig von Drittanbietern bereitgestellt)</span><span class="sxs-lookup"><span data-stu-id="88482-112">Often these are provided by third parties.</span></span>
- <span data-ttu-id="88482-113">Identitätsanbieter für die Authentifizierung</span><span class="sxs-lookup"><span data-stu-id="88482-113">Identity provider for authentication.</span></span>

<span data-ttu-id="88482-114">Web und Worker sind jeweils zustandslos.</span><span class="sxs-lookup"><span data-stu-id="88482-114">The web and worker are both stateless.</span></span> <span data-ttu-id="88482-115">Der Sitzungsstatus kann in einem verteilten Cache zwischengespeichert werden.</span><span class="sxs-lookup"><span data-stu-id="88482-115">Session state can be stored in a distributed cache.</span></span> <span data-ttu-id="88482-116">Alle Vorgänge mit langer Ausführungsdauer werden asynchron vom Worker durchgeführt.</span><span class="sxs-lookup"><span data-stu-id="88482-116">Any long-running work is done asynchronously by the worker.</span></span> <span data-ttu-id="88482-117">Der Worker kann über Nachrichten in der Warteschlange ausgelöst oder nach einem Zeitplan für die Batchverarbeitung ausgeführt werden.</span><span class="sxs-lookup"><span data-stu-id="88482-117">The worker can be triggered by messages on the queue, or run on a schedule for batch processing.</span></span> <span data-ttu-id="88482-118">Der Worker ist eine optionale Komponente.</span><span class="sxs-lookup"><span data-stu-id="88482-118">The worker is an optional component.</span></span> <span data-ttu-id="88482-119">Wenn keine Vorgänge mit langer Ausführungsdauer vorhanden sind, kann der Worker weggelassen werden.</span><span class="sxs-lookup"><span data-stu-id="88482-119">If there are no long-running operations, the worker can be omitted.</span></span>

<span data-ttu-id="88482-120">Das Front-End kann aus einer Web-API bestehen.</span><span class="sxs-lookup"><span data-stu-id="88482-120">The front end might consist of a web API.</span></span> <span data-ttu-id="88482-121">Auf Clientseite kann die Web-API von einer Single-Page-Anwendung, die AJAX-Aufrufe durchführt, oder von einer nativen Clientanwendung genutzt werden.</span><span class="sxs-lookup"><span data-stu-id="88482-121">On the client side, the web API can be consumed by a single-page application that makes AJAX calls, or by a native client application.</span></span>

## <a name="when-to-use-this-architecture"></a><span data-ttu-id="88482-122">Einsatzmöglichkeiten für diese Architektur</span><span class="sxs-lookup"><span data-stu-id="88482-122">When to use this architecture</span></span>

<span data-ttu-id="88482-123">Die Architektur „Web-Warteschlange-Worker“ wird häufig mit verwalteten Computediensten implementiert (entweder Azure App Service oder Azure Cloud Services).</span><span class="sxs-lookup"><span data-stu-id="88482-123">The Web-Queue-Worker architecture is typically implemented using managed compute services, either Azure App Service or Azure Cloud Services.</span></span>

<span data-ttu-id="88482-124">Ziehen Sie diese Art von Architektur in folgenden Fällen in Betracht:</span><span class="sxs-lookup"><span data-stu-id="88482-124">Consider this architecture style for:</span></span>

- <span data-ttu-id="88482-125">Anwendungen mit einer relativ einfachen Domäne</span><span class="sxs-lookup"><span data-stu-id="88482-125">Applications with a relatively simple domain.</span></span>
- <span data-ttu-id="88482-126">Anwendungen mit Workflows mit langer Ausführungsdauer oder Batchvorgängen</span><span class="sxs-lookup"><span data-stu-id="88482-126">Applications with some long-running workflows or batch operations.</span></span>
- <span data-ttu-id="88482-127">Bei Verwendung von verwalteten Diensten anstelle von IaaS (Infrastructure-as-a-Service).</span><span class="sxs-lookup"><span data-stu-id="88482-127">When you want to use managed services, rather than infrastructure as a service (IaaS).</span></span>

## <a name="benefits"></a><span data-ttu-id="88482-128">Vorteile</span><span class="sxs-lookup"><span data-stu-id="88482-128">Benefits</span></span>

- <span data-ttu-id="88482-129">Relativ einfache Architektur, die leicht zu verstehen ist</span><span class="sxs-lookup"><span data-stu-id="88482-129">Relatively simple architecture that is easy to understand.</span></span>
- <span data-ttu-id="88482-130">Einfache Bereitstellung und Verwaltung</span><span class="sxs-lookup"><span data-stu-id="88482-130">Easy to deploy and manage.</span></span>
- <span data-ttu-id="88482-131">Klare Trennung von Zuständigkeiten</span><span class="sxs-lookup"><span data-stu-id="88482-131">Clear separation of concerns.</span></span>
- <span data-ttu-id="88482-132">Das Front-End ist vom Worker per asynchronem Messaging entkoppelt</span><span class="sxs-lookup"><span data-stu-id="88482-132">The front end is decoupled from the worker using asynchronous messaging.</span></span>
- <span data-ttu-id="88482-133">Front-End und Worker können unabhängig voneinander skaliert werden</span><span class="sxs-lookup"><span data-stu-id="88482-133">The front end and the worker can be scaled independently.</span></span>

## <a name="challenges"></a><span data-ttu-id="88482-134">Herausforderungen</span><span class="sxs-lookup"><span data-stu-id="88482-134">Challenges</span></span>

- <span data-ttu-id="88482-135">Ohne eine sorgfältige Vorgehensweise beim Entwerfen können das Front-End und der Worker zu großen, monolithischen Komponenten werden, die schwierig zu verwalten und zu aktualisieren sind.</span><span class="sxs-lookup"><span data-stu-id="88482-135">Without careful design, the front end and the worker can become large, monolithic components that are difficult to maintain and update.</span></span>
- <span data-ttu-id="88482-136">Es können versteckte Abhängigkeiten vorhanden sein, wenn das Front-End und der Worker Datenschemas oder Codemodule gemeinsam nutzen.</span><span class="sxs-lookup"><span data-stu-id="88482-136">There may be hidden dependencies, if the front end and worker share data schemas or code modules.</span></span>

## <a name="best-practices"></a><span data-ttu-id="88482-137">Bewährte Methoden</span><span class="sxs-lookup"><span data-stu-id="88482-137">Best practices</span></span>

- <span data-ttu-id="88482-138">Machen Sie für den Client eine sorgfältig entworfene API verfügbar.</span><span class="sxs-lookup"><span data-stu-id="88482-138">Expose a well-designed API to the client.</span></span> <span data-ttu-id="88482-139">Siehe [API-Design][api-design].</span><span class="sxs-lookup"><span data-stu-id="88482-139">See [API design best practices][api-design].</span></span>
- <span data-ttu-id="88482-140">Führen Sie die automatische Skalierung durch, um Änderungen der Last zu bewältigen.</span><span class="sxs-lookup"><span data-stu-id="88482-140">Autoscale to handle changes in load.</span></span> <span data-ttu-id="88482-141">Siehe [Autoscaling][autoscaling] (Automatische Skalierung).</span><span class="sxs-lookup"><span data-stu-id="88482-141">See [Autoscaling best practices][autoscaling].</span></span>
- <span data-ttu-id="88482-142">Speichern Sie halbstatische Daten zwischen.</span><span class="sxs-lookup"><span data-stu-id="88482-142">Cache semi-static data.</span></span> <span data-ttu-id="88482-143">Siehe [Caching][caching].</span><span class="sxs-lookup"><span data-stu-id="88482-143">See [Caching best practices][caching].</span></span>
- <span data-ttu-id="88482-144">Verwenden Sie ein CDN zum Hosten von statischem Inhalt.</span><span class="sxs-lookup"><span data-stu-id="88482-144">Use a CDN to host static content.</span></span> <span data-ttu-id="88482-145">Siehe [Content Delivery Network][cdn].</span><span class="sxs-lookup"><span data-stu-id="88482-145">See [CDN best practices][cdn].</span></span>
- <span data-ttu-id="88482-146">Verwenden Sie „Polyglot Persistence“, falls zutreffend.</span><span class="sxs-lookup"><span data-stu-id="88482-146">Use polyglot persistence when appropriate.</span></span> <span data-ttu-id="88482-147">Siehe [Use the best data store for the job][polyglot] (Verwenden des besten Datenspeichers für den Auftrag).</span><span class="sxs-lookup"><span data-stu-id="88482-147">See [Use the best data store for the job][polyglot].</span></span>
- <span data-ttu-id="88482-148">Partitionieren Sie Daten, um die Skalierbarkeit zu verbessern, Konflikte zu reduzieren und die Leistung zu optimieren.</span><span class="sxs-lookup"><span data-stu-id="88482-148">Partition data to improve scalability, reduce contention, and optimize performance.</span></span> <span data-ttu-id="88482-149">Siehe [Data partitioning][data-partition] (Datenpartitionierung).</span><span class="sxs-lookup"><span data-stu-id="88482-149">See [Data partitioning best practices][data-partition].</span></span>

## <a name="web-queue-worker-on-azure-app-service"></a><span data-ttu-id="88482-150">Web-Warteschlange-Worker in Azure App Service</span><span class="sxs-lookup"><span data-stu-id="88482-150">Web-Queue-Worker on Azure App Service</span></span>

<span data-ttu-id="88482-151">In diesem Abschnitt wird eine empfohlene Architektur vom Typ „Web-Warteschlange-Worker“ beschrieben, für die Azure App Service verwendet wird.</span><span class="sxs-lookup"><span data-stu-id="88482-151">This section describes a recommended Web-Queue-Worker architecture that uses Azure App Service.</span></span>

![Physisches Diagramm der Architektur „Web-Warteschlange-Worker“](./images/web-queue-worker-physical.png)

- <span data-ttu-id="88482-153">Das Front-End wird als Azure App Service-Web-App, der Worker wird als Azure Functions-App implementiert.</span><span class="sxs-lookup"><span data-stu-id="88482-153">The front end is implemented as an Azure App Service web app, and the worker is implemented as an Azure Functions app.</span></span> <span data-ttu-id="88482-154">Die Web-App und Funktions-App werden beide einem App Service-Plan zugeordnet, über den die VM-Instanzen bereitgestellt werden.</span><span class="sxs-lookup"><span data-stu-id="88482-154">The web app and the function app are both associated with an App Service plan that provides the VM instances.</span></span>

- <span data-ttu-id="88482-155">Sie können entweder Azure Service Bus- oder Azure Storage-Warteschlangen als Nachrichtenwarteschlange verwenden.</span><span class="sxs-lookup"><span data-stu-id="88482-155">You can use either Azure Service Bus or Azure Storage queues for the message queue.</span></span> <span data-ttu-id="88482-156">(Im Diagramm ist eine Azure Storage-Warteschlange dargestellt.)</span><span class="sxs-lookup"><span data-stu-id="88482-156">(The diagram shows an Azure Storage queue.)</span></span>

- <span data-ttu-id="88482-157">Bei Azure Redis Cache werden der Sitzungszustand und andere Daten gespeichert, für die der Zugriff mit geringer Wartezeit erforderlich ist.</span><span class="sxs-lookup"><span data-stu-id="88482-157">Azure Redis Cache stores session state and other data that needs low latency access.</span></span>

- <span data-ttu-id="88482-158">Azure CDN wird verwendet, um statischen Inhalt zwischenzuspeichern, z.B. Images, CSS oder HTML.</span><span class="sxs-lookup"><span data-stu-id="88482-158">Azure CDN is used to cache static content such as images, CSS, or HTML.</span></span>

- <span data-ttu-id="88482-159">Wählen Sie für die Speicherung die Speichertechnologie aus, mit der die Anforderungen der Anwendung am besten erfüllt werden.</span><span class="sxs-lookup"><span data-stu-id="88482-159">For storage, choose the storage technologies that best fit the needs of the application.</span></span> <span data-ttu-id="88482-160">Sie können auch mehrere Speichertechnologien („Polyglot Persistence“) verwenden.</span><span class="sxs-lookup"><span data-stu-id="88482-160">You might use multiple storage technologies (polyglot persistence).</span></span> <span data-ttu-id="88482-161">Zur Verdeutlichung sind im Diagramm die Komponenten Azure SQL-Datenbank und Azure Cosmos DB dargestellt.</span><span class="sxs-lookup"><span data-stu-id="88482-161">To illustrate this idea, the diagram shows Azure SQL Database and Azure Cosmos DB.</span></span>

<span data-ttu-id="88482-162">Weitere Informationen finden Sie unter [Improve scalability in a web application][scalable-web-app] (Verbessern der Skalierbarkeit in einer Webanwendung).</span><span class="sxs-lookup"><span data-stu-id="88482-162">For more details, see [App Service web application reference architecture][scalable-web-app].</span></span>

### <a name="additional-considerations"></a><span data-ttu-id="88482-163">Zusätzliche Überlegungen</span><span class="sxs-lookup"><span data-stu-id="88482-163">Additional considerations</span></span>

- <span data-ttu-id="88482-164">Nicht jede Transaktion muss über die Warteschlange und den Worker in den Speicher verlaufen.</span><span class="sxs-lookup"><span data-stu-id="88482-164">Not every transaction has to go through the queue and worker to storage.</span></span> <span data-ttu-id="88482-165">Das Web-Front-End kann einfache Lese- und Schreibvorgänge direkt durchführen.</span><span class="sxs-lookup"><span data-stu-id="88482-165">The web front end can perform simple read/write operations directly.</span></span> <span data-ttu-id="88482-166">Worker sind für ressourcenintensive Aufgaben oder Workflows mit langer Ausführungsdauer ausgelegt.</span><span class="sxs-lookup"><span data-stu-id="88482-166">Workers are designed for resource-intensive tasks or long-running workflows.</span></span> <span data-ttu-id="88482-167">In einigen Fällen benötigen Sie unter Umständen gar keinen Worker.</span><span class="sxs-lookup"><span data-stu-id="88482-167">In some cases, you might not need a worker at all.</span></span>

- <span data-ttu-id="88482-168">Verwenden Sie die integrierte App Service-Funktion für die automatische Skalierung, um für die Anzahl von VM-Instanzen das horizontale Hochskalieren durchzuführen.</span><span class="sxs-lookup"><span data-stu-id="88482-168">Use the built-in autoscale feature of App Service to scale out the number of VM instances.</span></span> <span data-ttu-id="88482-169">Verwenden Sie die automatische Skalierung nach Zeitplan, wenn die Last der Anwendung vorhersagbaren Mustern folgt.</span><span class="sxs-lookup"><span data-stu-id="88482-169">If the load on the application follows predictable patterns, use schedule-based autoscale.</span></span> <span data-ttu-id="88482-170">Falls die Last nicht vorhersagbar ist, sollten Sie auf Metriken basierende Regeln für die automatische Skalierung nutzen.</span><span class="sxs-lookup"><span data-stu-id="88482-170">If the load is unpredictable, use metrics-based autoscaling rules.</span></span>

- <span data-ttu-id="88482-171">Erwägen Sie, die Web-App und Funktions-App in getrennten App Service-Plänen anzuordnen.</span><span class="sxs-lookup"><span data-stu-id="88482-171">Consider putting the web app and the function app into separate App Service plans.</span></span> <span data-ttu-id="88482-172">Auf diese Weise können sie unabhängig voneinander skaliert werden.</span><span class="sxs-lookup"><span data-stu-id="88482-172">That way, they can be scaled independently.</span></span>

- <span data-ttu-id="88482-173">Verwenden Sie separate App Service-Pläne für die Produktion und für das Testen.</span><span class="sxs-lookup"><span data-stu-id="88482-173">Use separate App Service plans for production and testing.</span></span> <span data-ttu-id="88482-174">Wenn Sie denselben Plan verwenden, bedeutet dies sonst, dass Ihre Tests auf den VMs für die Produktion durchgeführt werden.</span><span class="sxs-lookup"><span data-stu-id="88482-174">Otherwise, if you use the same plan for production and testing, it means your tests are running on your production VMs.</span></span>

- <span data-ttu-id="88482-175">Verwenden Sie Bereitstellungsslots, um die Bereitstellungen zu verwalten.</span><span class="sxs-lookup"><span data-stu-id="88482-175">Use deployment slots to manage deployments.</span></span> <span data-ttu-id="88482-176">So können Sie eine aktualisierte Version in einem Stagingslot bereitstellen und dann zur neuen Version wechseln.</span><span class="sxs-lookup"><span data-stu-id="88482-176">This lets you to deploy an updated version to a staging slot, then swap over to the new version.</span></span> <span data-ttu-id="88482-177">Außerdem können Sie zurück zur vorherigen Version wechseln, falls ein Problem mit dem Update auftritt.</span><span class="sxs-lookup"><span data-stu-id="88482-177">It also lets you swap back to the previous version, if there was a problem with the update.</span></span>

<!-- links -->

[api-design]: ../../best-practices/api-design.md
[autoscaling]: ../../best-practices/auto-scaling.md
[caching]: ../../best-practices/caching.md
[cdn]: ../../best-practices/cdn.md
[data-partition]: ../../best-practices/data-partitioning.md
[polyglot]: ../design-principles/use-the-best-data-store.md
[scalable-web-app]: ../../reference-architectures/app-service-web-app/scalable-web-app.md