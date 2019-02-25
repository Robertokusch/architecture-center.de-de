---
title: 'CAF: Kleine bis mittlere Unternehmen – anfängliche Unternehmensrichtlinie hinter der Governancestrategie'
titleSuffix: Microsoft Cloud Adoption Framework for Azure
ms.service: architecture-center
ms.subservice: enterprise-cloud-adoption
ms.custom: governance
ms.date: 2/11/2019
description: Kleine bis mittlere Unternehmen – anfängliche Unternehmensrichtlinie hinter der Governancestrategie
author: BrianBlanchard
ms.openlocfilehash: 4f49d0aae2c6ab5a5c8feba669cbbb904af2773b
ms.sourcegitcommit: 273e690c0cfabbc3822089c7d8bc743ef41d2b6e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/08/2019
ms.locfileid: "55900784"
---
# <a name="small-to-medium-enterprise-initial-corporate-policy-behind-the-governance-strategy"></a><span data-ttu-id="0fe16-103">Kleine bis mittlere Unternehmen: Anfängliche Unternehmensrichtlinie hinter der Governancestrategie</span><span class="sxs-lookup"><span data-stu-id="0fe16-103">Small-to-medium enterprise: Initial corporate policy behind the governance strategy</span></span>

<span data-ttu-id="0fe16-104">Die folgende Unternehmensrichtlinie definiert eine anfängliche Governanceposition, die der Ausgangspunkt für diese Governance Journey ist.</span><span class="sxs-lookup"><span data-stu-id="0fe16-104">The following corporate policy defines an initial governance position, which is the starting point for this journey.</span></span> <span data-ttu-id="0fe16-105">In diesem Artikel werden Risiken in einem frühen Stadium, anfängliche Richtlinienanweisungen und frühe Prozesse zum Durchsetzen von Richtlinienanweisungen definiert.</span><span class="sxs-lookup"><span data-stu-id="0fe16-105">This article defines early-stage risks, initial policy statements, and early processes to enforce policy statements.</span></span>

> [!NOTE]
><span data-ttu-id="0fe16-106">Die Unternehmensrichtlinie ist kein technisches Dokument, gibt aber den Anstoß zu vielen technischen Entscheidungen.</span><span class="sxs-lookup"><span data-stu-id="0fe16-106">The corporate policy is not a technical document, but it drives many technical decisions.</span></span> <span data-ttu-id="0fe16-107">Die in der [Übersicht](./overview.md) beschriebene Governance-MVP ist letztlich von dieser Richtlinie abgeleitet.</span><span class="sxs-lookup"><span data-stu-id="0fe16-107">The governance MVP described in the [overview](./overview.md) ultimately derives from this policy.</span></span> <span data-ttu-id="0fe16-108">Vor der Implementierung eines Governance-MVP sollte Ihre Organisation eine Unternehmensrichtlinie entwickeln, die auf Ihren eigenen Zielen und geschäftlichen Risiken basiert.</span><span class="sxs-lookup"><span data-stu-id="0fe16-108">Before implementing a governance MVP, your organization should develop a corporate policy based on your own objectives and business risks.</span></span>

## <a name="cloud-governance-team"></a><span data-ttu-id="0fe16-109">Cloudgovernanceteam</span><span class="sxs-lookup"><span data-stu-id="0fe16-109">Cloud Governance team</span></span>

<span data-ttu-id="0fe16-110">In dieser Schilderung besteht das Cloudgovernanceteam aus zwei Systemadministratoren, die die Notwendigkeit einer Governance erkannt haben.</span><span class="sxs-lookup"><span data-stu-id="0fe16-110">In this narrative, the Cloud Governance team is comprised of two systems administrators who have recognized the need for governance.</span></span> <span data-ttu-id="0fe16-111">In den nächsten Monaten übernehmen sie den Auftrag, die Governance der Cloudpräsenz des Unternehmens zu bereinigen, also als Cloudverwalter zu agieren.</span><span class="sxs-lookup"><span data-stu-id="0fe16-111">Over the next several months, they will inherit the job of cleaning up the governance of the company’s cloud presence, earning them the title of Cloud Custodians.</span></span> <span data-ttu-id="0fe16-112">In zukünftigen Entwicklungen wird sich diese Position wahrscheinlich ändern.</span><span class="sxs-lookup"><span data-stu-id="0fe16-112">In subsequent evolutions, this title will likely change.</span></span>

[!INCLUDE [business-risk](../../../../../includes/cloud-adoption/governance/business-risks.md)]

## <a name="tolerance-indicators"></a><span data-ttu-id="0fe16-113">Toleranzindikatoren</span><span class="sxs-lookup"><span data-stu-id="0fe16-113">Tolerance indicators</span></span>

<span data-ttu-id="0fe16-114">Die aktuelle Risikotoleranz ist hoch, und das Interesse an der Investition in die Cloudgovernance ist niedrig.</span><span class="sxs-lookup"><span data-stu-id="0fe16-114">The current tolerance for risk is high and the appetite for investing in cloud governance is low.</span></span> <span data-ttu-id="0fe16-115">Daher fungieren die Toleranzindikatoren als Frühwarnsystem, um weitere Investitionen von Zeit und Energie auszulösen.</span><span class="sxs-lookup"><span data-stu-id="0fe16-115">As such, the tolerance indicators act as an early warning system to trigger more investment of time and energy.</span></span> <span data-ttu-id="0fe16-116">Wenn die nachfolgend aufgeführten Indikatoren festgestellt werden, sollten Sie die Governancestrategie weiterentwickeln.</span><span class="sxs-lookup"><span data-stu-id="0fe16-116">If and when the following indicators are observed, you should evolve the governance strategy.</span></span>

- <span data-ttu-id="0fe16-117">Kostenmanagement: Das Ausmaß der Bereitstellung übersteigt 100 Ressourcen in der Cloud, oder die monatlichen Ausgaben überschreiten 1.000 USD pro Monat.</span><span class="sxs-lookup"><span data-stu-id="0fe16-117">Cost Management: The scale of deployment exceeds 100 assets to the cloud, or monthly spending exceeds $1,000 USD per month.</span></span>
- <span data-ttu-id="0fe16-118">Sicherheitsbaseline: Die Aufnahme der geschützten Daten in definierte Cloudeinführungspläne.</span><span class="sxs-lookup"><span data-stu-id="0fe16-118">Security Baseline: Inclusion of protected data in defined cloud adoption plans.</span></span>
- <span data-ttu-id="0fe16-119">Ressourcenkonsistenz: Die Aufnahme der unternehmenskritischen Anwendungen in definierte Cloudeinführungspläne.</span><span class="sxs-lookup"><span data-stu-id="0fe16-119">Resource Consistency: Inclusion of any mission-critical applications in defined cloud adoption plans.</span></span>

[!INCLUDE [policy-statements](../../../../../includes/cloud-adoption/governance/policy-statements.md)]

## <a name="next-steps"></a><span data-ttu-id="0fe16-120">Nächste Schritte</span><span class="sxs-lookup"><span data-stu-id="0fe16-120">Next steps</span></span>

<span data-ttu-id="0fe16-121">Diese Unternehmensrichtlinie bereitet das Cloudgovernanceteam auf die Implementierung des Governance-MVP vor, das die Grundlage für die Einführung bildet.</span><span class="sxs-lookup"><span data-stu-id="0fe16-121">This corporate policy prepares the Cloud Governance team to implement the governance MVP, which will be the foundation for adoption.</span></span> <span data-ttu-id="0fe16-122">Der nächste Schritt ist das Implementieren dieses MVP.</span><span class="sxs-lookup"><span data-stu-id="0fe16-122">The next step is to implement this MVP.</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="0fe16-123">Beschreibung der bewährten Methode</span><span class="sxs-lookup"><span data-stu-id="0fe16-123">Best practice explained</span></span>](./best-practice-explained.md)