---
title: Muster „Gatewayrouting“
titleSuffix: Cloud Design Patterns
description: Leiten Sie Anforderungen an mehrere Dienste mit einem einzelnen Endpunkt weiter.
keywords: Entwurfsmuster
author: dragon119
ms.date: 06/23/2017
ms.topic: design-pattern
ms.service: architecture-center
ms.subservice: cloud-fundamentals
ms.custom: seodec18
ms.openlocfilehash: e5c93c98a562e790d547d08fdf312c973cfceed8
ms.sourcegitcommit: 1b50810208354577b00e89e5c031b774b02736e2
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/23/2019
ms.locfileid: "54487227"
---
# <a name="gateway-routing-pattern"></a><span data-ttu-id="1d26e-104">Muster „Gatewayrouting“</span><span class="sxs-lookup"><span data-stu-id="1d26e-104">Gateway Routing pattern</span></span>

<span data-ttu-id="1d26e-105">Leiten Sie Anforderungen an mehrere Dienste mit einem einzelnen Endpunkt weiter.</span><span class="sxs-lookup"><span data-stu-id="1d26e-105">Route requests to multiple services using a single endpoint.</span></span> <span data-ttu-id="1d26e-106">Dieses Muster ist hilfreich, wenn Sie mehrere Dienste auf einem einzigen Endpunkt verfügbar machen möchten und die Weiterleitung an den geeigneten Dienst basierend auf der Anforderung erfolgen soll.</span><span class="sxs-lookup"><span data-stu-id="1d26e-106">This pattern is useful when you wish to expose multiple services on a single endpoint and route to the appropriate service based on the request.</span></span>

## <a name="context-and-problem"></a><span data-ttu-id="1d26e-107">Kontext und Problem</span><span class="sxs-lookup"><span data-stu-id="1d26e-107">Context and problem</span></span>

<span data-ttu-id="1d26e-108">Wenn ein Client mehrere Dienste nutzen muss, kann es schwierig sein, einen separaten Endpunkt für jeden Dienst einzurichten und dafür zu sorgen, dass der Client jeden Endpunkt verwaltet.</span><span class="sxs-lookup"><span data-stu-id="1d26e-108">When a client needs to consume multiple services, setting up a separate endpoint for each service and having the client manage each endpoint can be challenging.</span></span> <span data-ttu-id="1d26e-109">Eine E-Commerce-Anwendung stellt beispielsweise Dienste für Folgendes bereit: Suche, Reviews, Warenkorb, Auftragsabschluss und Auftragsverlauf.</span><span class="sxs-lookup"><span data-stu-id="1d26e-109">For example, an e-commerce application might provide services such as search, reviews, cart, checkout, and order history.</span></span> <span data-ttu-id="1d26e-110">Jeder Dienst verwendet eine andere API, mit der der Client interagieren muss, und der Client muss jeden Endpunkt kennen, um eine Verbindung mit den Diensten herstellen zu können.</span><span class="sxs-lookup"><span data-stu-id="1d26e-110">Each service has a different API that the client must interact with, and the client must know about each endpoint in order to connect to the services.</span></span> <span data-ttu-id="1d26e-111">Wenn sich eine API ändert, muss auch der Client aktualisiert werden.</span><span class="sxs-lookup"><span data-stu-id="1d26e-111">If an API changes, the client must be updated as well.</span></span> <span data-ttu-id="1d26e-112">Wenn Sie einen Dienst in zwei oder mehr separate Dienste umgestalten, muss der Code sowohl im Dienst als auch im Client geändert werden.</span><span class="sxs-lookup"><span data-stu-id="1d26e-112">If you refactor a service into two or more separate services, the code must change in both the service and the client.</span></span>

## <a name="solution"></a><span data-ttu-id="1d26e-113">Lösung</span><span class="sxs-lookup"><span data-stu-id="1d26e-113">Solution</span></span>

<span data-ttu-id="1d26e-114">Platzieren Sie ein Gateway vor einer Reihe von Anwendungen, Diensten oder Bereitstellungen.</span><span class="sxs-lookup"><span data-stu-id="1d26e-114">Place a gateway in front of a set of applications, services, or deployments.</span></span> <span data-ttu-id="1d26e-115">Implementieren Sie ein Layer-7-Routing für die Anwendung, um Anforderungen an die geeigneten Instanzen weiterzuleiten.</span><span class="sxs-lookup"><span data-stu-id="1d26e-115">Use application Layer 7 routing to route the request to the appropriate instances.</span></span>

<span data-ttu-id="1d26e-116">Bei diesem Muster muss die Clientanwendung nur einen einzigen Endpunkt kennen und mit diesem kommunizieren.</span><span class="sxs-lookup"><span data-stu-id="1d26e-116">With this pattern, the client application only needs to know about and communicate with a single endpoint.</span></span> <span data-ttu-id="1d26e-117">Wenn ein Dienst konsolidiert oder aufgeteilt wird, muss der Client nicht unbedingt aktualisiert werden.</span><span class="sxs-lookup"><span data-stu-id="1d26e-117">If a service is consolidated or decomposed, the client does not necessarily require updating.</span></span> <span data-ttu-id="1d26e-118">Der Client kann weiterhin Anforderungen an das Gateway senden, es ändert sich nur die Weiterleitung.</span><span class="sxs-lookup"><span data-stu-id="1d26e-118">It can continue making requests to the gateway, and only the routing changes.</span></span>

<span data-ttu-id="1d26e-119">Mit einem Gateway können Sie auch die Back-End-Dienste von den Clients abstrahieren, sodass Clientaufrufe einfach bleiben, aber Änderungen in den Back-End-Diensten hinter dem Gateway möglich sind.</span><span class="sxs-lookup"><span data-stu-id="1d26e-119">A gateway also lets you abstract backend services from the clients, allowing you to keep client calls simple while enabling changes in the backend services behind the gateway.</span></span> <span data-ttu-id="1d26e-120">Clientaufrufe können an jeden Dienst weitergeleitet werden, der das erwartete Clientverhalten verarbeiten muss. So können Sie Dienste hinter dem Gateway hinzufügen, teilen und neu anordnen, ohne den Client ändern zu müssen.</span><span class="sxs-lookup"><span data-stu-id="1d26e-120">Client calls can be routed to whatever service or services need to handle the expected client behavior, allowing you to add, split, and reorganize services behind the gateway without changing the client.</span></span>

![Diagramm des Musters „Gatewayrouting“](./_images/gateway-routing.png)

<span data-ttu-id="1d26e-122">Dieses Muster kann auch bei der Bereitstellung helfen, da Sie bestimmen können, wie Updates für die Benutzer bereitgestellt werden.</span><span class="sxs-lookup"><span data-stu-id="1d26e-122">This pattern can also help with deployment, by allowing you to manage how updates are rolled out to users.</span></span> <span data-ttu-id="1d26e-123">Wenn eine neue Version Ihres Diensts bereitgestellt wird, kann diese Bereitstellung parallel zur vorhandenen Version erfolgen.</span><span class="sxs-lookup"><span data-stu-id="1d26e-123">When a new version of your service is deployed, it can be deployed in parallel with the existing version.</span></span> <span data-ttu-id="1d26e-124">Durch Routing können Sie steuern, welche Version des Diensts den Clients präsentiert wird. So können Sie flexibel verschiedene Releasestrategien für Updates nutzen – als inkrementelle, parallele oder vollständige Rollouts.</span><span class="sxs-lookup"><span data-stu-id="1d26e-124">Routing lets you control what version of the service is presented to the clients, giving you the flexibility to use various release strategies, whether incremental, parallel, or complete rollouts of updates.</span></span> <span data-ttu-id="1d26e-125">Sollten nach der Bereitstellung eines neuen Diensts Probleme auftreten, können Sie einfach eine Konfigurationsänderung auf dem Gateway vornehmen, ohne dass Clients beeinträchtigt werden.</span><span class="sxs-lookup"><span data-stu-id="1d26e-125">Any issues discovered after the new service is deployed can be quickly reverted by making a configuration change at the gateway, without affecting clients.</span></span>

## <a name="issues-and-considerations"></a><span data-ttu-id="1d26e-126">Probleme und Überlegungen</span><span class="sxs-lookup"><span data-stu-id="1d26e-126">Issues and considerations</span></span>

- <span data-ttu-id="1d26e-127">Der Gatewaydienst kann ein Single Point of Failure darstellen.</span><span class="sxs-lookup"><span data-stu-id="1d26e-127">The gateway service may introduce a single point of failure.</span></span> <span data-ttu-id="1d26e-128">Stellen Sie sicher, dass es auf Ihre Verfügbarkeitsanforderungen ausgelegt ist.</span><span class="sxs-lookup"><span data-stu-id="1d26e-128">Ensure it is properly designed to meet your availability requirements.</span></span> <span data-ttu-id="1d26e-129">Berücksichtigen Sie bei der Implementierung die Aspekte Resilienz und Fehlertoleranz.</span><span class="sxs-lookup"><span data-stu-id="1d26e-129">Consider resiliency and fault tolerance capabilities when implementing.</span></span>
- <span data-ttu-id="1d26e-130">Der Gatewaydienst kann einen Engpass darstellen.</span><span class="sxs-lookup"><span data-stu-id="1d26e-130">The gateway service may introduce a bottleneck.</span></span> <span data-ttu-id="1d26e-131">Stellen Sie sicher, dass das Gateway leistungsfähig genug ist, um die Last zu verarbeiten, und dass es sich problemlos gemäß Ihren Wachstumserwartungen skalieren lässt.</span><span class="sxs-lookup"><span data-stu-id="1d26e-131">Ensure the gateway has adequate performance to handle load and can easily scale in line with your growth expectations.</span></span>
- <span data-ttu-id="1d26e-132">Führen Sie einen Auslastungstest für das Gateway durch, um sicherzustellen, dass keine kaskadierenden Ausfälle bei den Diensten auftreten.</span><span class="sxs-lookup"><span data-stu-id="1d26e-132">Perform load testing against the gateway to ensure you don't introduce cascading failures for services.</span></span>
- <span data-ttu-id="1d26e-133">Das Gatewayrouting erfolgt auf Ebene 7.</span><span class="sxs-lookup"><span data-stu-id="1d26e-133">Gateway routing is level 7.</span></span> <span data-ttu-id="1d26e-134">Es kann auf IP, Port, Header oder URL basieren.</span><span class="sxs-lookup"><span data-stu-id="1d26e-134">It can be based on IP, port, header, or URL.</span></span>

## <a name="when-to-use-this-pattern"></a><span data-ttu-id="1d26e-135">Verwendung dieses Musters</span><span class="sxs-lookup"><span data-stu-id="1d26e-135">When to use this pattern</span></span>

<span data-ttu-id="1d26e-136">Verwenden Sie dieses Muster in folgenden Fällen:</span><span class="sxs-lookup"><span data-stu-id="1d26e-136">Use this pattern when:</span></span>

- <span data-ttu-id="1d26e-137">Ein Client muss mehrere Dienste nutzen, auf die hinter einem Gateway zugegriffen werden kann.</span><span class="sxs-lookup"><span data-stu-id="1d26e-137">A client needs to consume multiple services that can be accessed behind a gateway.</span></span>
- <span data-ttu-id="1d26e-138">Sie möchten Clientanwendungen durch Nutzung eines einzigen Endpunkts vereinfachen.</span><span class="sxs-lookup"><span data-stu-id="1d26e-138">You wish to simplify client applications by using a single endpoint.</span></span>
- <span data-ttu-id="1d26e-139">Sie müssen Anforderungen von extern adressierbaren Endpunkten an interne virtuelle Endpunkte weiterleiten – beispielsweise müssen Sie Ports auf einem virtuellen Computer verfügbar machen, um virtuelle IP-Adressen zu gruppieren.</span><span class="sxs-lookup"><span data-stu-id="1d26e-139">You need to route requests from externally addressable endpoints to internal virtual endpoints, such as exposing ports on a VM to cluster virtual IP addresses.</span></span>

<span data-ttu-id="1d26e-140">Dieses Muster eignet sich wahrscheinlich nicht, wenn Sie über eine einfache Anwendung verfügen, die nur einen oder zwei Dienste nutzt.</span><span class="sxs-lookup"><span data-stu-id="1d26e-140">This pattern may not be suitable when you have a simple application that uses only one or two services.</span></span>

## <a name="example"></a><span data-ttu-id="1d26e-141">Beispiel</span><span class="sxs-lookup"><span data-stu-id="1d26e-141">Example</span></span>

<span data-ttu-id="1d26e-142">Der folgende Code ist eine einfache Beispielkonfigurationsdatei für einen Server, der Anforderungen für Anwendungen, die sich in verschiedenen virtuellen Verzeichnissen befinden, an verschiedene Computer im Back-End weiterleitet. Als Router wird nginx verwendet.</span><span class="sxs-lookup"><span data-stu-id="1d26e-142">Using Nginx as the router, the following is a simple example configuration file for a server that routes requests for applications residing on different virtual directories to different machines at the back end.</span></span>

```console
server {
    listen 80;
    server_name domain.com;

    location /app1 {
        proxy_pass http://10.0.3.10:80;
    }

    location /app2 {
        proxy_pass http://10.0.3.20:80;
    }

    location /app3 {
        proxy_pass http://10.0.3.30:80;
    }
}
```

## <a name="related-guidance"></a><span data-ttu-id="1d26e-143">Verwandte Leitfäden</span><span class="sxs-lookup"><span data-stu-id="1d26e-143">Related guidance</span></span>

- [<span data-ttu-id="1d26e-144">Muster „Back-Ends für Front-Ends“</span><span class="sxs-lookup"><span data-stu-id="1d26e-144">Backends for Frontends pattern</span></span>](./backends-for-frontends.md)
- [<span data-ttu-id="1d26e-145">Gatewayaggregationsmuster</span><span class="sxs-lookup"><span data-stu-id="1d26e-145">Gateway Aggregation pattern</span></span>](./gateway-aggregation.md)
- [<span data-ttu-id="1d26e-146">Muster „Gatewayabladung“</span><span class="sxs-lookup"><span data-stu-id="1d26e-146">Gateway Offloading pattern</span></span>](./gateway-offloading.md)
