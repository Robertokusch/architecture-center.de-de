---
title: Microservices-Referenzimplementierung für Azure Kubernetes Service
description: Diese Referenzimplementierung veranschaulicht bewährte Methoden für eine Microservices-Architektur.
author: MikeWasson
ms.date: 02/26/2019
ms.topic: guide
ms.service: architecture-center
ms.subservice: reference-architecture
ms.custom: microservices
ms.openlocfilehash: 17e275e5b5f45233f7467192402cb28fce35c57b
ms.sourcegitcommit: 0a8a60d782facc294f7f78ec0e9033e3ee16bf4a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/08/2019
ms.locfileid: "59068904"
---
# <a name="designing-a-microservices-architecture"></a><span data-ttu-id="4b765-103">Entwerfen einer Microservices-Architektur</span><span class="sxs-lookup"><span data-stu-id="4b765-103">Designing a microservices architecture</span></span>

<span data-ttu-id="4b765-104">Microservices haben sich zu einem beliebten Architekturstil für die Erstellung robuster, hochgradig skalierbarer, unabhängig bereitstellbarer Cloudanwendungen entwickelt, die sich schnell weiterentwickeln lassen.</span><span class="sxs-lookup"><span data-stu-id="4b765-104">Microservices have become a popular architectural style for building cloud applications that are resilient, highly scalable, independently deployable, and able to evolve quickly.</span></span> <span data-ttu-id="4b765-105">Damit Microservices nicht nur ein Schlagwort bleiben, erfordern sie allerdings eine andere Herangehensweise an die Gestaltung und Erstellung von Anwendungen.</span><span class="sxs-lookup"><span data-stu-id="4b765-105">To be more than just a buzzword, however, microservices require a different approach to designing and building applications.</span></span>

<span data-ttu-id="4b765-106">Diese Artikelreihe beschäftigt sich mit der Erstellung und dem Betrieb einer Microservices-Architektur in Azure.</span><span class="sxs-lookup"><span data-stu-id="4b765-106">In this set of articles, we explore how to build and run a microservices architecture on Azure.</span></span> <span data-ttu-id="4b765-107">Dabei werden folgende Themen behandelt:</span><span class="sxs-lookup"><span data-stu-id="4b765-107">Topics include:</span></span>

- [<span data-ttu-id="4b765-108">Kommunikation zwischen Diensten</span><span class="sxs-lookup"><span data-stu-id="4b765-108">Interservice communication</span></span>](./interservice-communication.md)
- [<span data-ttu-id="4b765-109">API-Design</span><span class="sxs-lookup"><span data-stu-id="4b765-109">API design</span></span>](./api-design.md)
- [<span data-ttu-id="4b765-110">API-Gateways</span><span class="sxs-lookup"><span data-stu-id="4b765-110">API gateways</span></span>](./gateway.md)
- [<span data-ttu-id="4b765-111">Überlegungen zu Daten</span><span class="sxs-lookup"><span data-stu-id="4b765-111">Data considerations</span></span>](./data-considerations.md)
- [<span data-ttu-id="4b765-112">Entwurfsmuster</span><span class="sxs-lookup"><span data-stu-id="4b765-112">Design patterns</span></span>](./patterns.md)

## <a name="prerequisites"></a><span data-ttu-id="4b765-113">Voraussetzungen</span><span class="sxs-lookup"><span data-stu-id="4b765-113">Prerequisites</span></span>

<span data-ttu-id="4b765-114">Machen Sie sich vor der Lektüre dieser Artikel ggf. zunächst mit Folgendem vertraut:</span><span class="sxs-lookup"><span data-stu-id="4b765-114">Before reading these articles, you might start with the following:</span></span>

- <span data-ttu-id="4b765-115">[Einführung in Microservices-Architekturen](../introduction.md):</span><span class="sxs-lookup"><span data-stu-id="4b765-115">[Introduction to microservices architectures](../introduction.md).</span></span> <span data-ttu-id="4b765-116">Informieren Sie sich über die Vorteile und Herausforderungen im Zusammenhang mit Microservices sowie darüber, wann diese Art von Architektur verwendet werden sollte.</span><span class="sxs-lookup"><span data-stu-id="4b765-116">Understand the benefits and challenges of microservices, and when to use this style of architecture.</span></span>
- <span data-ttu-id="4b765-117">[Verwenden der Domänenanalyse zur Modellierung von Microservices](../model/domain-analysis.md):</span><span class="sxs-lookup"><span data-stu-id="4b765-117">[Using domain analysis to model microservices](../model/domain-analysis.md).</span></span> <span data-ttu-id="4b765-118">Hier wird ein domänenbasierter Ansatz für die Modellierung von Microservices vermittelt.</span><span class="sxs-lookup"><span data-stu-id="4b765-118">Learn a domain-driven approach to modeling microservices.</span></span>

## <a name="reference-implementation"></a><span data-ttu-id="4b765-119">Referenzimplementierung</span><span class="sxs-lookup"><span data-stu-id="4b765-119">Reference implementation</span></span>

<span data-ttu-id="4b765-120">Zur Veranschaulichung bewährter Methoden für eine Microservices-Architektur haben wir als Referenzimplementierung eine Drohnenlieferungsanwendung erstellt.</span><span class="sxs-lookup"><span data-stu-id="4b765-120">To illustrate best practices for a microservices architecture, we created a reference implementation that we call the Drone Delivery application.</span></span> <span data-ttu-id="4b765-121">Diese Implementierung wird unter Verwendung von Azure Kubernetes Service (AKS) in Kubernetes ausgeführt.</span><span class="sxs-lookup"><span data-stu-id="4b765-121">This implementation runs on Kubernetes using Azure Kubernetes Service (AKS).</span></span> <span data-ttu-id="4b765-122">Die Referenzimplementierung finden Sie auf [GitHub][drone-ri].</span><span class="sxs-lookup"><span data-stu-id="4b765-122">You can find the reference implementation on [GitHub][drone-ri].</span></span>

![Architektur der Drohnenlieferungsanwendung](../images/drone-delivery.png)

## <a name="scenario"></a><span data-ttu-id="4b765-124">Szenario</span><span class="sxs-lookup"><span data-stu-id="4b765-124">Scenario</span></span>

<span data-ttu-id="4b765-125">Fabrikam, Inc. führt einen Drohnenlieferdienst ein.</span><span class="sxs-lookup"><span data-stu-id="4b765-125">Fabrikam, Inc. is starting a drone delivery service.</span></span> <span data-ttu-id="4b765-126">Das Unternehmen verfügt über eine Drohnenflotte.</span><span class="sxs-lookup"><span data-stu-id="4b765-126">The company manages a fleet of drone aircraft.</span></span> <span data-ttu-id="4b765-127">Unternehmen registrieren sich bei dem Dienst, und Benutzer können eine Drohne anfordern, die auszuliefernde Waren abholt.</span><span class="sxs-lookup"><span data-stu-id="4b765-127">Businesses register with the service, and users can request a drone to pick up goods for delivery.</span></span> <span data-ttu-id="4b765-128">Wenn ein Kunde einen Abholtermin festlegt, weist ein Back-End-System eine Drohne zu und teilt dem Benutzer die voraussichtliche Lieferzeit mit.</span><span class="sxs-lookup"><span data-stu-id="4b765-128">When a customer schedules a pickup, a backend system assigns a drone and notifies the user with an estimated delivery time.</span></span> <span data-ttu-id="4b765-129">Während der Durchführung der Lieferung kann der Kunde die Position der Drohne nachverfolgen, und die voraussichtliche Ankunftszeit wird kontinuierlich aktualisiert.</span><span class="sxs-lookup"><span data-stu-id="4b765-129">While the delivery is in progress, the customer can track the location of the drone, with a continuously updated ETA.</span></span>

<span data-ttu-id="4b765-130">Dieses Szenario beinhaltet eine recht komplizierte Domäne.</span><span class="sxs-lookup"><span data-stu-id="4b765-130">This scenario involves a fairly complicated domain.</span></span> <span data-ttu-id="4b765-131">Zu den Anliegen des Unternehmens zählen unter anderem die Zeitplanung für die Drohnen, die Nachverfolgung von Paketen, die Verwaltung von Benutzerkonten sowie die Speicherung und Analyse von Verlaufsdaten.</span><span class="sxs-lookup"><span data-stu-id="4b765-131">Some of the business concerns include scheduling drones, tracking packages, managing user accounts, and storing and analyzing historical data.</span></span> <span data-ttu-id="4b765-132">Darüber hinaus ist Fabrikam an einer schnellen Marktreife interessiert und möchte dann zeitnah neue Features und Funktionen hinzufügen.</span><span class="sxs-lookup"><span data-stu-id="4b765-132">Moreover, Fabrikam wants to get to market quickly and then iterate quickly, adding new functionality and capabilities.</span></span> <span data-ttu-id="4b765-133">Die Anwendung muss cloudfähig sein und über ein hohes Servicelevelziel (Service Level Objective, SLO) verfügen.</span><span class="sxs-lookup"><span data-stu-id="4b765-133">The application needs to operate at cloud scale, with a high service level objective (SLO).</span></span> <span data-ttu-id="4b765-134">Fabrikam erwartet außerdem erhebliche Unterschiede bei den Datenspeicher- und Abfrageanforderungen der verschiedenen Systemkomponenten.</span><span class="sxs-lookup"><span data-stu-id="4b765-134">Fabrikam also expects that different parts of the system will have very different requirements for data storage and querying.</span></span> <span data-ttu-id="4b765-135">Aufgrund dieser Überlegungen hat sich Fabrikam bei seiner Drohnenlieferungsanwendung für eine Microservices-Architektur entschieden.</span><span class="sxs-lookup"><span data-stu-id="4b765-135">All of these considerations lead Fabrikam to choose a microservices architecture for the Drone Delivery application.</span></span>

> [!NOTE]
> <span data-ttu-id="4b765-136">Hilfreiche Informationen zur Wahl einer Microservices-Architektur oder eines anderen Architekturstils finden Sie im [Azure-Anwendungsarchitekturleitfaden](../../guide/index.md).</span><span class="sxs-lookup"><span data-stu-id="4b765-136">For help in choosing between a microservices architecture and other architectural styles, see the [Azure Application Architecture Guide](../../guide/index.md).</span></span>

<span data-ttu-id="4b765-137">Unsere Referenzimplementierung verwendet Kubernetes mit [Azure Kubernetes Service (AKS)](/azure/aks/).</span><span class="sxs-lookup"><span data-stu-id="4b765-137">Our reference implementation uses Kubernetes with [Azure Kubernetes Service](/azure/aks/) (AKS).</span></span> <span data-ttu-id="4b765-138">Viele der allgemeinen architekturbezogenen Entscheidungen und Herausforderungen gelten jedoch für alle Containerorchestratoren (einschließlich [Azure Service Fabric](/azure/service-fabric/)).</span><span class="sxs-lookup"><span data-stu-id="4b765-138">However, many of the high-level architectural decisions and challenges will apply to any container orchestrator, including [Azure Service Fabric](/azure/service-fabric/).</span></span>

<!-- links -->

[drone-ri]: https://github.com/mspnp/microservices-reference-implementation/tree/v0.1.0-orig

## <a name="next-steps"></a><span data-ttu-id="4b765-139">Nächste Schritte</span><span class="sxs-lookup"><span data-stu-id="4b765-139">Next steps</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="4b765-140">Auswählen einer Computeoption</span><span class="sxs-lookup"><span data-stu-id="4b765-140">Choose a compute option</span></span>](./compute-options.md)