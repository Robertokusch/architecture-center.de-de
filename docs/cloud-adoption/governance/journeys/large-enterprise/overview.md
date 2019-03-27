---
title: 'CAF: Governance Journey für große Unternehmen'
titleSuffix: Microsoft Cloud Adoption Framework for Azure
ms.service: architecture-center
ms.subservice: enterprise-cloud-adoption
ms.custom: governance
ms.date: 02/11/2019
description: Governance Journey für große Unternehmen
author: BrianBlanchard
ms.openlocfilehash: 689b600524c3be6c505ade8c5aaa447d772c6e35
ms.sourcegitcommit: c053e6edb429299a0ad9b327888d596c48859d4a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/20/2019
ms.locfileid: "58245241"
---
# <a name="caf-large-enterprise-governance-journey"></a><span data-ttu-id="d62bd-103">CAF: Governance Journey für große Unternehmen</span><span class="sxs-lookup"><span data-stu-id="d62bd-103">CAF: Large enterprise governance journey</span></span>

## <a name="best-practice-overview"></a><span data-ttu-id="d62bd-104">Übersicht über bewährte Methoden</span><span class="sxs-lookup"><span data-stu-id="d62bd-104">Best practice overview</span></span>

<span data-ttu-id="d62bd-105">Diese Governance Journey folgt den Erfahrungen eines fiktiven Unternehmens in verschiedenen Phasen der Governancereife.</span><span class="sxs-lookup"><span data-stu-id="d62bd-105">This governance journey follows the experiences of a fictional company through various stages of governance maturity.</span></span> <span data-ttu-id="d62bd-106">Sie basiert auf echten Kundenlösungen.</span><span class="sxs-lookup"><span data-stu-id="d62bd-106">It is based on real customer journeys.</span></span> <span data-ttu-id="d62bd-107">Die empfohlenen bewährten Methoden basieren auf den Einschränkungen und Anforderungen des fiktiven Unternehmens.</span><span class="sxs-lookup"><span data-stu-id="d62bd-107">The suggested best practices are based on the constraints and needs of the fictional company.</span></span>

<span data-ttu-id="d62bd-108">Als Schnellstartpunkt definiert diese Übersicht ein auf bewährten Methoden basierendes Minimum Viable Product (MVP) für die Governance.</span><span class="sxs-lookup"><span data-stu-id="d62bd-108">As a quick starting point, this overview defines a minimum viable product (MVP) for governance based on best practices.</span></span> <span data-ttu-id="d62bd-109">Sie enthält außerdem Links zu einigen Governanceentwicklungen, die weitere bewährte Methoden hinzufügen, wenn neue Geschäfts- oder technische Risiken entstehen.</span><span class="sxs-lookup"><span data-stu-id="d62bd-109">It also provides links to some governance evolutions that add further best practices as new business or technical risks emerge.</span></span>

> [!WARNING]
> <span data-ttu-id="d62bd-110">Dieses MVP ist ein Baselineausgangspunkt, der auf einer Reihe von Annahmen basiert.</span><span class="sxs-lookup"><span data-stu-id="d62bd-110">This MVP is a baseline starting point, based on a set of assumptions.</span></span> <span data-ttu-id="d62bd-111">Auch dieser minimale Satz bewährter Methoden basiert auf Unternehmensrichtlinien, die von beispiellosen geschäftlichen Risiken und Risikotoleranzen bestimmt werden.</span><span class="sxs-lookup"><span data-stu-id="d62bd-111">Even this minimal set of best practices is based on corporate policies driven by unique business risks and risk tolerances.</span></span> <span data-ttu-id="d62bd-112">Um festzustellen, ob diese Annahmen auf Sie zutreffen, lesen Sie die [längere Schilderung](./narrative.md), die diesem Artikel folgt.</span><span class="sxs-lookup"><span data-stu-id="d62bd-112">To see if these assumptions apply to you, read the [longer narrative](./narrative.md) that follows this article.</span></span>

### <a name="governance-best-practice"></a><span data-ttu-id="d62bd-113">Bewährte Governancemethode</span><span class="sxs-lookup"><span data-stu-id="d62bd-113">Governance best practice</span></span>

<span data-ttu-id="d62bd-114">Diese bewährte Methode dient als Grundlage, auf der eine Organisation schnell und konsistent Governancesicherungen für mehrere Azure-Abonnements hinzufügen kann.</span><span class="sxs-lookup"><span data-stu-id="d62bd-114">This best practice serves as a foundation that an organization can use to quickly and consistently add governance guardrails across multiple Azure subscriptions.</span></span>

### <a name="resource-organization"></a><span data-ttu-id="d62bd-115">Ressourcenorganisation</span><span class="sxs-lookup"><span data-stu-id="d62bd-115">Resource organization</span></span>

<span data-ttu-id="d62bd-116">Die folgende Abbildung zeigt die Governance-MVP-Hierarchie zum Organisieren von Ressourcen.</span><span class="sxs-lookup"><span data-stu-id="d62bd-116">The following diagram shows the governance MVP hierarchy for organizing resources.</span></span>

![Ressourcenorganisationsdiagramm](../../../_images/governance/resource-organization.png)

<span data-ttu-id="d62bd-118">Jede Anwendung sollte im richtigen Bereich der Verwaltungsgruppen-, Abonnement- und Ressourcengruppenhierarchie bereitgestellt werden.</span><span class="sxs-lookup"><span data-stu-id="d62bd-118">Every application should be deployed in the proper area of the management group, subscription, and resource group hierarchy.</span></span> <span data-ttu-id="d62bd-119">Während der Bereitstellungsplanung erstellt das Cloudgovernanceteam die erforderlichen Knoten in der Hierarchie, um die für die Cloudeinführung zuständigen Teams die Arbeit zu ermöglichen.</span><span class="sxs-lookup"><span data-stu-id="d62bd-119">During deployment planning, the Cloud Governance team will create the necessary nodes in the hierarchy to empower the cloud adoption teams.</span></span>

1. <span data-ttu-id="d62bd-120">Eine Verwaltungsgruppe für die einzelnen Geschäftseinheiten mit einer detaillierten Hierarchie, die die Geografie und dann den Umgebungstyp (Produktion, nicht Produktion) wiedergibt.</span><span class="sxs-lookup"><span data-stu-id="d62bd-120">A management group for each business unit with a detailed hierarchy that reflects geography then environment type (Production, Non-Production).</span></span>
2. <span data-ttu-id="d62bd-121">Ein Abonnement für jede eindeutige Kombination von Geschäftseinheit, Geografie, Umgebung und „Kategorisierung der Anwendung“.</span><span class="sxs-lookup"><span data-stu-id="d62bd-121">A subscription for each unique combination of business unit, geography, environment, and "Application Categorization."</span></span>
3. <span data-ttu-id="d62bd-122">Eine separate Ressourcengruppe für jede Anwendung.</span><span class="sxs-lookup"><span data-stu-id="d62bd-122">A separate resource group for each application.</span></span>
4. <span data-ttu-id="d62bd-123">Konsistente Benennung sollte auf jeder Ebene dieser Gruppierungshierarchie angewendet werden.</span><span class="sxs-lookup"><span data-stu-id="d62bd-123">Consistent nomenclature should be applied at each level of this grouping hierarchy.</span></span>

![Ressourcenorganisationsdiagramm für große Unternehmen](../../../_images/governance/large-enterprise-resource-organization.png)

<span data-ttu-id="d62bd-125">Diese Muster bieten Raum für Wachstum, ohne die Hierarchie unnötig zu verkomplizieren.</span><span class="sxs-lookup"><span data-stu-id="d62bd-125">These patterns provide room for growth without complicating the hierarchy unnecessarily.</span></span>

[!INCLUDE [governance-of-resources](../../../../../includes/cloud-adoption/governance/governance-of-resources.md)]

## <a name="governance-evolutions"></a><span data-ttu-id="d62bd-126">Governanceentwicklungen</span><span class="sxs-lookup"><span data-stu-id="d62bd-126">Governance evolutions</span></span>

<span data-ttu-id="d62bd-127">Sobald dieses MVP bereitgestellt ist, können zusätzliche Ebenen der Governance schnell in die Umgebung integriert werden.</span><span class="sxs-lookup"><span data-stu-id="d62bd-127">Once this MVP has been deployed, additional layers of governance can be quickly incorporated into the environment.</span></span> <span data-ttu-id="d62bd-128">Einige Möglichkeiten, das MVP zu entwickeln, sind:</span><span class="sxs-lookup"><span data-stu-id="d62bd-128">Here are some ways to evolve the MVP to meet specific business needs:</span></span>

- [<span data-ttu-id="d62bd-129">Sicherheitsbaseline für geschützte Daten</span><span class="sxs-lookup"><span data-stu-id="d62bd-129">Security Baseline for protected data</span></span>](./security-baseline-evolution.md)
- [<span data-ttu-id="d62bd-130">Ressourcenkonfigurationen für unternehmenskritische Anwendungen</span><span class="sxs-lookup"><span data-stu-id="d62bd-130">Resource configurations for mission-critical applications</span></span>](./resource-consistency-evolution.md)
- [<span data-ttu-id="d62bd-131">Steuerelemente für das Kostenmanagement</span><span class="sxs-lookup"><span data-stu-id="d62bd-131">Controls for Cost Management</span></span>](./cost-management-evolution.md)
- [<span data-ttu-id="d62bd-132">Steuerelemente für die Multi-Cloud-Entwicklung</span><span class="sxs-lookup"><span data-stu-id="d62bd-132">Controls for multi-cloud evolution</span></span>](./multi-cloud-evolution.md)

<!-- markdownlint-disable MD026 -->

## <a name="what-does-this-best-practice-do"></a><span data-ttu-id="d62bd-133">Wozu dient diese bewährte Methode?</span><span class="sxs-lookup"><span data-stu-id="d62bd-133">What does this best practice do?</span></span>

<span data-ttu-id="d62bd-134">Im MVP sind Methoden und Tools für die Disziplin der [Beschleunigung der Bereitstellung](../../deployment-acceleration/overview.md) festgelegt, um schnell Unternehmensrichtlinien anzuwenden.</span><span class="sxs-lookup"><span data-stu-id="d62bd-134">In the MVP, practices and tools from the [Deployment Acceleration](../../deployment-acceleration/overview.md) discipline are established to quickly apply corporate policy.</span></span> <span data-ttu-id="d62bd-135">Insbesondere verwendet das MVP Azure Blueprints, Azure Policy und Azure-Verwaltungsgruppen, um einige grundlegende Unternehmensrichtlinien anzuwenden, wie in der Schilderung für das fiktive Unternehmen definiert.</span><span class="sxs-lookup"><span data-stu-id="d62bd-135">In particular, the MVP uses Azure Blueprints, Azure Policy, and Azure management groups to apply a few basic corporate policies, as defined in the narrative for this fictional company.</span></span> <span data-ttu-id="d62bd-136">Diese Unternehmensrichtlinien werden mithilfe von Azure Resource Manager-Vorlagen und Azure-Richtlinien angewandt, um eine sehr kleine Baseline für Identität und Sicherheit festzulegen.</span><span class="sxs-lookup"><span data-stu-id="d62bd-136">Those corporate policies are applied using Azure Resource Manager templates and Azure policies to establish a very small baseline for identity and security.</span></span>

![Beispiel für ein inkrementelles Governance-MVP](../../../_images/governance/governance-mvp.png)

## <a name="evolving-the-best-practice"></a><span data-ttu-id="d62bd-138">Weiterentwicklung der bewährten Methode</span><span class="sxs-lookup"><span data-stu-id="d62bd-138">Evolving the best practice</span></span>

<span data-ttu-id="d62bd-139">Im Lauf der Zeit wird dieses Governance-MVP verwendet, um die Governancemethoden weiterzuentwickeln.</span><span class="sxs-lookup"><span data-stu-id="d62bd-139">Over time, this governance MVP will be used to evolve the governance practices.</span></span> <span data-ttu-id="d62bd-140">Mit fortschreitender Einführung wächst das geschäftliche Risiko.</span><span class="sxs-lookup"><span data-stu-id="d62bd-140">As adoption advances, business risk grows.</span></span> <span data-ttu-id="d62bd-141">Verschiedene Disziplinen im CAF-Governancemodell werden zur Verringerung dieser Risiken weiterentwickelt.</span><span class="sxs-lookup"><span data-stu-id="d62bd-141">Various disciplines within the CAF governance model will evolve to mitigate those risks.</span></span> <span data-ttu-id="d62bd-142">Spätere Artikel dieser Reihe erläutern die Weiterentwicklung der Unternehmensrichtlinie, die sich auf das fiktive Unternehmen auswirkt.</span><span class="sxs-lookup"><span data-stu-id="d62bd-142">Later articles in this series discuss the evolution of corporate policy affecting the fictional company.</span></span> <span data-ttu-id="d62bd-143">Diese Weiterentwicklung erfolgt drei Disziplinen übergreifend:</span><span class="sxs-lookup"><span data-stu-id="d62bd-143">These evolutions happen across three disciplines:</span></span>

- <span data-ttu-id="d62bd-144">Identitätsbaseline, indem Migrationsabhängigkeiten in der Schilderung weiterentwickelt werden.</span><span class="sxs-lookup"><span data-stu-id="d62bd-144">Identity Baseline, as migration dependencies evolve in the narrative</span></span>
- <span data-ttu-id="d62bd-145">Kostenmanagement, wenn die Einführung skaliert wird.</span><span class="sxs-lookup"><span data-stu-id="d62bd-145">Cost Management, as adoption scales.</span></span>
- <span data-ttu-id="d62bd-146">Sicherheitsbaseline, indem geschützte Daten bereitgestellt werden.</span><span class="sxs-lookup"><span data-stu-id="d62bd-146">Security Baseline, as protected data is deployed.</span></span>
- <span data-ttu-id="d62bd-147">Ressourcenkonsistenz, indem die IT-Abteilung beginnt, unternehmenskritische Workloads zu unterstützen.</span><span class="sxs-lookup"><span data-stu-id="d62bd-147">Resource Consistency, as IT Operations begins supporting mission-critical workloads.</span></span>

![Beispiel für ein inkrementelles Governance-MVP](../../../_images/governance/governance-evolution-large.png)

## <a name="next-steps"></a><span data-ttu-id="d62bd-149">Nächste Schritte</span><span class="sxs-lookup"><span data-stu-id="d62bd-149">Next steps</span></span>

<span data-ttu-id="d62bd-150">Da Sie jetzt mit dem Governance-MVP vertraut sind und eine Vorstellung von zukünftigen Weiterentwicklungen der Governance haben, lesen Sie die unterstützende Lösung, um zusätzliche Kontextinformationen zu erhalten.</span><span class="sxs-lookup"><span data-stu-id="d62bd-150">Now that you’re familiar with the governance MVP and have an idea of the governance evolutions to follow, read the supporting narrative for additional context.</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="d62bd-151">Lesen Sie die unterstützende Lösung</span><span class="sxs-lookup"><span data-stu-id="d62bd-151">Read the supporting narrative</span></span>](./narrative.md)
