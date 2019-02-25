---
title: 'Framework für die Cloudeinführung (Cloud Adoption Framework, CAF): Worum handelt es sich bei digitalen Ressourcen?'
titleSuffix: Microsoft Cloud Adoption Framework for Azure
description: Worum handelt es sich bei digitalen Ressourcen?
author: BrianBlanchard
ms.date: 12/10/2018
ms.topic: guide
ms.service: architecture-center
ms.subservice: enterprise-cloud-adoption
ms.openlocfilehash: d23bdbdd98226e8b9cdb9bbb5fefa5a918a498d8
ms.sourcegitcommit: 273e690c0cfabbc3822089c7d8bc743ef41d2b6e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/08/2019
ms.locfileid: "55898424"
---
<!-- markdownlint-disable MD026 -->

# <a name="caf-what-is-a-digital-estate"></a><span data-ttu-id="1017f-103">Framework für die Cloudeinführung (Cloud Adoption Framework, CAF): Worum handelt es sich bei digitalen Ressourcen?</span><span class="sxs-lookup"><span data-stu-id="1017f-103">CAF: What is a digital estate?</span></span>

<span data-ttu-id="1017f-104">Digitale Ressourcen finden sich in jedem modernen Unternehmen.</span><span class="sxs-lookup"><span data-stu-id="1017f-104">Every modern company has some form of digital estate.</span></span> <span data-ttu-id="1017f-105">Eine digitale Ressource ähnelt einer physischen Ressource und ist ein abstrakter Verweis auf eine Reihe konkreter eigener Ressourcen.</span><span class="sxs-lookup"><span data-stu-id="1017f-105">Much like a physical estate, a digital estate is an abstract reference to a number of tangible, owned assets.</span></span> <span data-ttu-id="1017f-106">Im digitalen Bereich handelt es sich bei diesen Ressourcen unter anderem um virtuelle Computer (VMs), Server, Anwendungen und Daten.</span><span class="sxs-lookup"><span data-stu-id="1017f-106">In a digital estate, those assets are comprised of virtual machines (VMs), servers, applications, data, and so on.</span></span> <span data-ttu-id="1017f-107">Digitale Ressourcen sind also im Grunde alle IT-Ressourcen, die Unternehmensprozessen und unterstützenden Vorgängen zugrunde liegen.</span><span class="sxs-lookup"><span data-stu-id="1017f-107">Essentially, a digital estate is the collection of IT assets that power business processes and supporting operations.</span></span>

<span data-ttu-id="1017f-108">Die Relevanz digitaler Ressourcen wird insbesondere bei der Planung und Durchführung digitaler Transformationen deutlich.</span><span class="sxs-lookup"><span data-stu-id="1017f-108">The importance of a digital estate is most obvious during planning and execution of Digital Transformation efforts.</span></span> <span data-ttu-id="1017f-109">Während des Transformationsprozesses werden digitale Ressourcen von Cloudstrategieteams verwendet, um die Geschäftsergebnisse mit Releaseplänen und technischen Aufgaben abzustimmen.</span><span class="sxs-lookup"><span data-stu-id="1017f-109">During transformation journeys, the digital estate is how Cloud Strategy teams map the business outcomes to release plans and technical efforts.</span></span> <span data-ttu-id="1017f-110">Dazu werden zunächst die digitalen Ressourcen ermittelt und gemessen, die die Organisation aktuell besitzt.</span><span class="sxs-lookup"><span data-stu-id="1017f-110">That all starts with an inventory and measurement of the digital assets the organization owns today.</span></span>

## <a name="how-can-a-digital-estate-be-measured"></a><span data-ttu-id="1017f-111">Wie können digitale Ressourcen gemessen werden?</span><span class="sxs-lookup"><span data-stu-id="1017f-111">How can a digital estate be measured?</span></span>

<span data-ttu-id="1017f-112">Die Messung digitaler Ressourcen hängt von den gewünschten Geschäftsergebnissen ab.</span><span class="sxs-lookup"><span data-stu-id="1017f-112">The measurement of a digital estate changes depending on the desired business outcomes.</span></span>

- <span data-ttu-id="1017f-113">**Infrastrukturmigrationen:**</span><span class="sxs-lookup"><span data-stu-id="1017f-113">**Infrastructure migrations**.</span></span> <span data-ttu-id="1017f-114">Wenn sich eine Organisation auf interne Aspekte konzentriert und beispielsweise ihre Kosten, Betriebsprozesse, Flexibilität oder andere Betriebsaspekte optimieren möchte, liegt der Schwerpunkt bei den digitalen Ressourcen auf virtuellen Computern, Servern und unterstützenden Workloads.</span><span class="sxs-lookup"><span data-stu-id="1017f-114">When an organization is inward facing and seeks to optimize cost, operational processes, agility, or other aspects of optimizing operations, the digital estate focuses on VMs, servers, and supporting workloads.</span></span>

- <span data-ttu-id="1017f-115">**Anwendungsinnovationen:**</span><span class="sxs-lookup"><span data-stu-id="1017f-115">**Application innovation**.</span></span> <span data-ttu-id="1017f-116">Bei kundenorientierten Transformationen sieht es etwas anders aus.</span><span class="sxs-lookup"><span data-stu-id="1017f-116">For customer-obsessed transformations, the lens is a bit different.</span></span> <span data-ttu-id="1017f-117">Hier liegt der Schwerpunkt auf den Anwendungen, APIs und Transaktionsdaten, die die Kunden unterstützen.</span><span class="sxs-lookup"><span data-stu-id="1017f-117">The focus should be placed in the applications, APIs, and transactional data that support the customers.</span></span> <span data-ttu-id="1017f-118">Virtuelle Computer und Network Appliances sind hier meist weniger relevant.</span><span class="sxs-lookup"><span data-stu-id="1017f-118">VMs and network appliances are often of less focus.</span></span>

- <span data-ttu-id="1017f-119">**Datenbasierte Innovationen:**</span><span class="sxs-lookup"><span data-stu-id="1017f-119">**Data-driven innovation**.</span></span> <span data-ttu-id="1017f-120">Angesichts der starken Digitalisierung des Markts ist es heutzutage schwer, ein neues Produkt oder einen neuen Dienst ohne solides Datenfundament einzuführen.</span><span class="sxs-lookup"><span data-stu-id="1017f-120">In today's digitally driven market, it's difficult to launch a new product or service without a strong foundation in data.</span></span> <span data-ttu-id="1017f-121">Bei einer Transformation zur Chancenverbesserung liegt der Schwerpunkt mehr auf den Datensilos innerhalb der gesamten Organisation.</span><span class="sxs-lookup"><span data-stu-id="1017f-121">During disruptive transformation, the focus is more on the silos of data across the organization.</span></span>

<span data-ttu-id="1017f-122">Wenn eine Organisation die wichtigste Form der Transformation versteht, wird die Planung digitaler Ressourcen deutlich einfacher.</span><span class="sxs-lookup"><span data-stu-id="1017f-122">Once an organization understands the most important form of transformation, digital estate planning becomes much easier to manage.</span></span>

> [!TIP]
> <span data-ttu-id="1017f-123">Jede Art von Transformation kann mit jeder der drei Ansichten gemessen werden.</span><span class="sxs-lookup"><span data-stu-id="1017f-123">Each type of transformation could be measured with any of the three views.</span></span> <span data-ttu-id="1017f-124">Darüber hinaus führen Unternehmen in der Regel alle drei Transformationen parallel aus.</span><span class="sxs-lookup"><span data-stu-id="1017f-124">Further, it is common for companies to execute all three transformations in parallel.</span></span> <span data-ttu-id="1017f-125">Unternehmensführung und Cloudstrategieteam sollten sich hinsichtlich der Transformation, die für den Erfolg des Unternehmens am wichtigsten ist, unbedingt abstimmen.</span><span class="sxs-lookup"><span data-stu-id="1017f-125">It is highly suggested that the leadership and Cloud Strategy team be firmly aligned regarding the transformation that is most important for business success.</span></span> <span data-ttu-id="1017f-126">Dieses Verständnis bildet die Grundlage für eine gemeinsame Sprache und gemeinsame Metriken für mehrere Initiativen.</span><span class="sxs-lookup"><span data-stu-id="1017f-126">That understanding will serve as the basis for common language and metrics across multiple initiatives.</span></span>

## <a name="how-can-a-financial-model-be-updated-to-reflect-the-digital-estate"></a><span data-ttu-id="1017f-127">Wie kann ein Finanzmodell aktualisiert werden, um die digitalen Ressourcen zu berücksichtigen?</span><span class="sxs-lookup"><span data-stu-id="1017f-127">How can a financial model be updated to reflect the digital estate?</span></span>

<span data-ttu-id="1017f-128">Eine Analyse der digitalen Ressourcen ist bei Cloudeinführungsaktivitäten hilfreich.</span><span class="sxs-lookup"><span data-stu-id="1017f-128">An analysis of the digital estate will drive cloud adoption activities.</span></span> <span data-ttu-id="1017f-129">Des Weiteren liefert sie Cloudkostenmodelle, die in Finanzmodelle einbezogen werden können, und trägt so zur Steigerung der Rendite (Return On Investment, ROI) bei.</span><span class="sxs-lookup"><span data-stu-id="1017f-129">It will also inform financial models by providing cloud costing models, which will in turn drive the return on investment (ROI).</span></span>

<span data-ttu-id="1017f-130">Gehen Sie für die Analyse digitaler Ressourcen wie folgt vor:</span><span class="sxs-lookup"><span data-stu-id="1017f-130">To complete the digital estate analysis, perform the following steps:</span></span>

1. [<span data-ttu-id="1017f-131">Bestimmen Sie den Analyseansatz.</span><span class="sxs-lookup"><span data-stu-id="1017f-131">Determine analysis approach</span></span>](approach.md)
1. [<span data-ttu-id="1017f-132">Erfassen Sie den aktuellen Bestand.</span><span class="sxs-lookup"><span data-stu-id="1017f-132">Collect current state inventory</span></span>](inventory.md)
1. [<span data-ttu-id="1017f-133">Rationalisieren Sie die digitalen Ressourcen.</span><span class="sxs-lookup"><span data-stu-id="1017f-133">Rationalize the assets in the digital estate</span></span>](rationalize.md)
1. [<span data-ttu-id="1017f-134">Gleichen Sie zur Preisberechnung Ressourcen mit Cloudangeboten ab.</span><span class="sxs-lookup"><span data-stu-id="1017f-134">Align assets to cloud offerings to calculate pricing</span></span>](calculate.md)

<span data-ttu-id="1017f-135">Finanzmodelle und Migrationsbacklogs können geändert werden, um die rationalisierten Ressourcen und die entsprechenden Preisangaben zu berücksichtigen.</span><span class="sxs-lookup"><span data-stu-id="1017f-135">Financial models and migration backlogs can be modified to reflect the rationalized and priced estate.</span></span>

## <a name="next-steps"></a><span data-ttu-id="1017f-136">Nächste Schritte</span><span class="sxs-lookup"><span data-stu-id="1017f-136">Next steps</span></span>

<span data-ttu-id="1017f-137">Bevor Sie mit der Planung für digitale Ressourcen beginnen, müssen Sie sich zunächst für einen Ansatz entscheiden.</span><span class="sxs-lookup"><span data-stu-id="1017f-137">Before digital estate planning can begin, you must determine what approach to use.</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="1017f-138">Ansätze für die Planung eines digitalen Umfelds</span><span class="sxs-lookup"><span data-stu-id="1017f-138">Approaches to digital estate planning</span></span>](approach.md)