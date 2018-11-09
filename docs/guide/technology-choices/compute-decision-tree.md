---
title: Entscheidungsstruktur für Azure-Computedienste
description: Ein Flussdiagramm zur Auswahl eines Computediensts
author: MikeWasson
ms.date: 11/03/2018
ms.openlocfilehash: cb074272b8d00a71223d8c5755ef8cde3a3f2592
ms.sourcegitcommit: 225251ee2dd669432a9c9abe3aa8cd84d9e020b7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/04/2018
ms.locfileid: "50981979"
---
# <a name="decision-tree-for-azure-compute-services"></a><span data-ttu-id="9fe1c-103">Entscheidungsstruktur für Azure-Computedienste</span><span class="sxs-lookup"><span data-stu-id="9fe1c-103">Decision tree for Azure compute services</span></span>

<span data-ttu-id="9fe1c-104">Azure bietet eine Vielzahl von Möglichkeiten zum Hosten Ihres Anwendungscodes.</span><span class="sxs-lookup"><span data-stu-id="9fe1c-104">Azure offers a number of ways to host your application code.</span></span> <span data-ttu-id="9fe1c-105">Der Begriff *Compute* bezieht sich auf das Hostingmodell für die Computeressourcen, auf denen Ihre Anwendung ausgeführt wird.</span><span class="sxs-lookup"><span data-stu-id="9fe1c-105">The term *compute* refers to the hosting model for the computing resources that your application runs on.</span></span> <span data-ttu-id="9fe1c-106">Das folgende Flussdiagramm unterstützt Sie bei der Auswahl eines Computediensts für Ihre Anwendung.</span><span class="sxs-lookup"><span data-stu-id="9fe1c-106">The following flowchart will help you to choose a compute service for your application.</span></span> <span data-ttu-id="9fe1c-107">Das Flussdiagramm führt Sie durch eine Reihe wichtiger Entscheidungskriterien, um eine Empfehlung zu erzielen.</span><span class="sxs-lookup"><span data-stu-id="9fe1c-107">The flowchart guides you through a set of key decision criteria to reach a recommendation.</span></span> 

<span data-ttu-id="9fe1c-108">**Betrachten Sie dieses Flussdiagramm als Ausgangspunkt.**</span><span class="sxs-lookup"><span data-stu-id="9fe1c-108">**Treat this flowchart as a starting point.**</span></span> <span data-ttu-id="9fe1c-109">Da jede Anwendung besondere Anforderungen aufweist, betrachten Sie die Empfehlung als Ausgangspunkt.</span><span class="sxs-lookup"><span data-stu-id="9fe1c-109">Every application has unique requirements, so use the recommendation as a starting point.</span></span> <span data-ttu-id="9fe1c-110">Führen Sie dann eine ausführlichere Auswertung durch, bei der Sie z.B. folgende Aspekte betrachten:</span><span class="sxs-lookup"><span data-stu-id="9fe1c-110">Then perform a more detailed evaluation, looking at aspects such as:</span></span>
 
- <span data-ttu-id="9fe1c-111">Funktionsumfang</span><span class="sxs-lookup"><span data-stu-id="9fe1c-111">Feature set</span></span>
- [<span data-ttu-id="9fe1c-112">Diensteinschränkungen</span><span class="sxs-lookup"><span data-stu-id="9fe1c-112">Service limits</span></span>](/azure/azure-subscription-service-limits)
- [<span data-ttu-id="9fe1c-113">Kosten</span><span class="sxs-lookup"><span data-stu-id="9fe1c-113">Cost</span></span>](https://azure.microsoft.com/pricing/)
- [<span data-ttu-id="9fe1c-114">SLA</span><span class="sxs-lookup"><span data-stu-id="9fe1c-114">SLA</span></span>](https://azure.microsoft.com/support/legal/sla/)
- [<span data-ttu-id="9fe1c-115">Regionale Verfügbarkeit</span><span class="sxs-lookup"><span data-stu-id="9fe1c-115">Regional availability</span></span>](https://azure.microsoft.com/global-infrastructure/services/)
- <span data-ttu-id="9fe1c-116">Entwicklerökosystem und Teamkenntnisse</span><span class="sxs-lookup"><span data-stu-id="9fe1c-116">Developer ecosystem and team skills</span></span>
- [<span data-ttu-id="9fe1c-117">Kriterien für die Auswahl einer Azure-Compute-Option</span><span class="sxs-lookup"><span data-stu-id="9fe1c-117">Compute comparison tables</span></span>](./compute-comparison.md)

<span data-ttu-id="9fe1c-118">Wenn Ihre Anwendung aus mehreren Workloads besteht, bewerten Sie jede Workload getrennt.</span><span class="sxs-lookup"><span data-stu-id="9fe1c-118">If your application consists of multiple workloads, evaluate each workload separately.</span></span> <span data-ttu-id="9fe1c-119">Eine vollständige Lösung kann zwei oder mehr Computedienste umfassen.</span><span class="sxs-lookup"><span data-stu-id="9fe1c-119">A complete solution may incorporate two or more compute services.</span></span>

<span data-ttu-id="9fe1c-120">Weitere Informationen zu den Optionen für das Hosten von Containern in Azure finden Sie unter https://azure.microsoft.com/overview/containers/.</span><span class="sxs-lookup"><span data-stu-id="9fe1c-120">For more information about your options for hosting containers in Azure, see https://azure.microsoft.com/overview/containers/.</span></span>

## <a name="flowchart"></a><span data-ttu-id="9fe1c-121">Flussdiagramm</span><span class="sxs-lookup"><span data-stu-id="9fe1c-121">Flowchart</span></span>

![](../images/compute-decision-tree.svg)

## <a name="definitions"></a><span data-ttu-id="9fe1c-122">Definitionen</span><span class="sxs-lookup"><span data-stu-id="9fe1c-122">Definitions</span></span>

- <span data-ttu-id="9fe1c-123">**Lift und Shift** ist eine Strategie zum Migrieren einer Workload zur Cloud ohne Überarbeitung der Anwendung oder Änderung des Codes.</span><span class="sxs-lookup"><span data-stu-id="9fe1c-123">**Lift and shift** is a strategy for migrating a workload to the cloud without redesigning the application or making code changes.</span></span> <span data-ttu-id="9fe1c-124">Dies wird auch als *erneutes Hosten* bezeichnet.</span><span class="sxs-lookup"><span data-stu-id="9fe1c-124">Also called *rehosting*.</span></span> <span data-ttu-id="9fe1c-125">Weitere Informationen finden Sie im [Azure-Migrationscenter](https://azure.microsoft.com/migration/).</span><span class="sxs-lookup"><span data-stu-id="9fe1c-125">For more information, see [Azure migration center](https://azure.microsoft.com/migration/).</span></span>

- <span data-ttu-id="9fe1c-126">**Cloudoptimierung** ist eine Strategie für die Migration zur Cloud durch Umgestaltung einer Anwendung, um cloudeigene Features und Funktionen zu nutzen.</span><span class="sxs-lookup"><span data-stu-id="9fe1c-126">**Cloud optimized** is a strategy for migrating to the cloud by refactoring an application to take advantage of cloud-native features and capabilities.</span></span>

## <a name="next-steps"></a><span data-ttu-id="9fe1c-127">Nächste Schritte</span><span class="sxs-lookup"><span data-stu-id="9fe1c-127">Next steps</span></span>

<span data-ttu-id="9fe1c-128">Weitere Aspekte, die berücksichtigt werden sollten, finden Sie unter [Kriterien für die Auswahl einer Azure-Compute-Option](./compute-comparison.md).</span><span class="sxs-lookup"><span data-stu-id="9fe1c-128">For additional criteria to consider, see [Criteria for choosing an Azure compute service](./compute-comparison.md).</span></span>
