---
title: Entscheidungsstruktur für Azure-Computedienste
description: Ein Flussdiagramm zur Auswahl eines Computediensts
author: MikeWasson
ms.date: 06/13/2018
ms.openlocfilehash: 60bb84d4bf210888d3d43498db043b6e452f6a80
ms.sourcegitcommit: 26b04f138a860979aea5d253ba7fecffc654841e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/19/2018
ms.locfileid: "36206726"
---
# <a name="decision-tree-for-azure-compute-services"></a><span data-ttu-id="e45cc-103">Entscheidungsstruktur für Azure-Computedienste</span><span class="sxs-lookup"><span data-stu-id="e45cc-103">Decision tree for Azure compute services</span></span>

<span data-ttu-id="e45cc-104">Azure bietet eine Vielzahl von Möglichkeiten zum Hosten Ihres Anwendungscodes.</span><span class="sxs-lookup"><span data-stu-id="e45cc-104">Azure offers a number of ways to host your application code.</span></span> <span data-ttu-id="e45cc-105">Der Begriff *Compute* bezieht sich auf das Hostingmodell für die Computeressourcen, auf denen Ihre Anwendung ausgeführt wird.</span><span class="sxs-lookup"><span data-stu-id="e45cc-105">The term *compute* refers to the hosting model for the computing resources that your application runs on.</span></span> <span data-ttu-id="e45cc-106">Das folgende Flussdiagramm unterstützt Sie bei der Auswahl eines Computediensts für Ihre Anwendung.</span><span class="sxs-lookup"><span data-stu-id="e45cc-106">The following flowchart will help you to choose a compute service for your application.</span></span> <span data-ttu-id="e45cc-107">Das Flussdiagramm führt Sie durch eine Reihe wichtiger Entscheidungskriterien, um eine Empfehlung zu erzielen.</span><span class="sxs-lookup"><span data-stu-id="e45cc-107">The flowchart guides you through a set of key decision criteria to reach a recommendation.</span></span> 

<span data-ttu-id="e45cc-108">**Betrachten Sie dieses Flussdiagramm als Ausgangspunkt.**</span><span class="sxs-lookup"><span data-stu-id="e45cc-108">**Treat this flowchart as a starting point.**</span></span> <span data-ttu-id="e45cc-109">Da jede Anwendung besondere Anforderungen aufweist, betrachten Sie die Empfehlung als Ausgangspunkt.</span><span class="sxs-lookup"><span data-stu-id="e45cc-109">Every application has unique requirements, so use the recommendation as a starting point.</span></span> <span data-ttu-id="e45cc-110">Führen Sie dann eine ausführlichere Auswertung durch, bei der Sie z.B. folgende Aspekte betrachten:</span><span class="sxs-lookup"><span data-stu-id="e45cc-110">Then perform a more detailed evaluation, looking at aspects such as:</span></span>
 
- <span data-ttu-id="e45cc-111">Funktionsumfang</span><span class="sxs-lookup"><span data-stu-id="e45cc-111">Feature set</span></span>
- [<span data-ttu-id="e45cc-112">Diensteinschränkungen</span><span class="sxs-lookup"><span data-stu-id="e45cc-112">Service limits</span></span>](/azure/azure-subscription-service-limits)
- [<span data-ttu-id="e45cc-113">Kosten</span><span class="sxs-lookup"><span data-stu-id="e45cc-113">Cost</span></span>](https://azure.microsoft.com/pricing/)
- [<span data-ttu-id="e45cc-114">SLA</span><span class="sxs-lookup"><span data-stu-id="e45cc-114">SLA</span></span>](https://azure.microsoft.com/support/legal/sla/)
- [<span data-ttu-id="e45cc-115">Regionale Verfügbarkeit</span><span class="sxs-lookup"><span data-stu-id="e45cc-115">Regional availability</span></span>](https://azure.microsoft.com/global-infrastructure/services/)
- <span data-ttu-id="e45cc-116">Entwicklerökosystem und Teamkenntnisse</span><span class="sxs-lookup"><span data-stu-id="e45cc-116">Developer ecosystem and team skills</span></span>
- [<span data-ttu-id="e45cc-117">Kriterien für die Auswahl einer Azure-Compute-Option</span><span class="sxs-lookup"><span data-stu-id="e45cc-117">Compute comparison tables</span></span>](./compute-comparison.md)

<span data-ttu-id="e45cc-118">Wenn Ihre Anwendung aus mehreren Workloads besteht, bewerten Sie jede Workload getrennt.</span><span class="sxs-lookup"><span data-stu-id="e45cc-118">If your application consists of multiple workloads, evaluate each workload separately.</span></span> <span data-ttu-id="e45cc-119">Eine vollständige Lösung kann zwei oder mehr Computedienste umfassen.</span><span class="sxs-lookup"><span data-stu-id="e45cc-119">A complete solution may incorporate two or more compute services.</span></span>

## <a name="flowchart"></a><span data-ttu-id="e45cc-120">Flussdiagramm</span><span class="sxs-lookup"><span data-stu-id="e45cc-120">Flowchart</span></span>

![](../images/compute-decision-tree.svg)

## <a name="definitions"></a><span data-ttu-id="e45cc-121">Definitionen</span><span class="sxs-lookup"><span data-stu-id="e45cc-121">Definitions</span></span>

- <span data-ttu-id="e45cc-122">**Greenfield** beschreibt ein völlig neues Softwareprojekt, das von Grund auf neu erstellt wird.</span><span class="sxs-lookup"><span data-stu-id="e45cc-122">**Greenfield** describes a software project that is completely new and built from scratch.</span></span> <span data-ttu-id="e45cc-123">Es enthält keinen Legacycode.</span><span class="sxs-lookup"><span data-stu-id="e45cc-123">It does not include legacy code.</span></span> 

- <span data-ttu-id="e45cc-124">**Brownfield** beschreibt ein Softwareprojekt, das auf einer vorhandenen Anwendung basiert.</span><span class="sxs-lookup"><span data-stu-id="e45cc-124">**Brownfield** describes a software project that builds on an existing application.</span></span> <span data-ttu-id="e45cc-125">Es enthält unter Umständen Legacycode oder -frameworks.</span><span class="sxs-lookup"><span data-stu-id="e45cc-125">It may inherit legacy code or frameworks.</span></span>

- <span data-ttu-id="e45cc-126">**Lift und Shift** ist eine Strategie zum Migrieren einer Workload zur Cloud ohne Überarbeitung der Anwendung oder Änderung des Codes.</span><span class="sxs-lookup"><span data-stu-id="e45cc-126">**Lift and shift** is a strategy for migrating a workload to the cloud without redesigning the application or making code changes.</span></span> <span data-ttu-id="e45cc-127">Dies wird auch als *erneutes Hosten* bezeichnet.</span><span class="sxs-lookup"><span data-stu-id="e45cc-127">Also called *rehosting*.</span></span> <span data-ttu-id="e45cc-128">Weitere Informationen finden Sie im [Azure-Migrationscenter](https://azure.microsoft.com/migration/).</span><span class="sxs-lookup"><span data-stu-id="e45cc-128">For more information, see [Azure migration center](https://azure.microsoft.com/migration/).</span></span>

- <span data-ttu-id="e45cc-129">**Cloudoptimierung** ist eine Strategie für die Migration zur Cloud durch Umgestaltung einer Anwendung, um cloudeigene Features und Funktionen zu nutzen.</span><span class="sxs-lookup"><span data-stu-id="e45cc-129">**Cloud optimized** is a strategy for migrating to the cloud by refactoring an application to take advantage of cloud-native features and capabilities.</span></span>

## <a name="next-steps"></a><span data-ttu-id="e45cc-130">Nächste Schritte</span><span class="sxs-lookup"><span data-stu-id="e45cc-130">Next steps</span></span>

<span data-ttu-id="e45cc-131">Weitere Aspekte, die berücksichtigt werden sollten, finden Sie unter [Kriterien für die Auswahl einer Azure-Compute-Option](./compute-comparison.md).</span><span class="sxs-lookup"><span data-stu-id="e45cc-131">For additional criteria to consider, see [Criteria for choosing an Azure compute service](./compute-comparison.md).</span></span>
