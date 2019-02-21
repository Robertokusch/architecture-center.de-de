---
title: 'CAF: Passen Sie die Entwurfshandbücher an die Richtlinie an.'
description: Wie passen Sie Entwurfshandbücher an die Richtlinie an?
author: BrianBlanchard
ms.date: 01/04/2019
ms.openlocfilehash: 77a35597585e5f58967ea79d3c7b0fa17b6ab80e
ms.sourcegitcommit: 273e690c0cfabbc3822089c7d8bc743ef41d2b6e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/08/2019
ms.locfileid: "55900736"
---
<!---
I've established policies. How to help developers adopt these policies?
Draft an architecture design guide.

[Aspirational statement] If you're using Azure, you can use one of ours as a starting point. The choose one of the following 6 as a starting point and mold it to fit your policies.
--->

<!-- markdownlint-disable MD026 -->

# <a name="how-do-you-align-design-guides-with-policy"></a><span data-ttu-id="61a7b-103">Wie passen Sie Entwurfshandbücher an die Richtlinie an?</span><span class="sxs-lookup"><span data-stu-id="61a7b-103">How do you align design guides with policy?</span></span>

<span data-ttu-id="61a7b-104">Nachdem Sie [Cloudrichtlinien definiert](define-policy.md) haben, die auf Ihren [identifizierten Risiken](understanding-business-risk.md) basieren, müssen Sie handlungsrelevante Leitfäden generieren, die an diese Richtlinien für Ihre IT-Mitarbeiter und Entwickler angepasst sind, damit sie sich darauf beziehen können.</span><span class="sxs-lookup"><span data-stu-id="61a7b-104">After you've [defined cloud policies](define-policy.md) based on your [identified risks](understanding-business-risk.md), you'll need to generate actionable guidance that aligns with these policies for your IT staff and developers to refer to.</span></span> <span data-ttu-id="61a7b-105">Das Entwerfen eines Entwurfshandbuch für Cloud-Governance ermöglicht es Ihnen, bestimmte strukturelle, technologische und prozessuale Optionen anzugeben, die auf den Richtlinienanweisungen basieren, die Sie für jede der fünf Governance-Disziplinen generiert haben.</span><span class="sxs-lookup"><span data-stu-id="61a7b-105">Drafting a cloud governance design guide allows you to specify specific structural, technological, and process choices based on the policy statements you generated for each of the five governance disciplines.</span></span>

<span data-ttu-id="61a7b-106">Ein Entwurfshandbuch zur Cloud-Governance sollte die Architekturoptionen und Entwurfsmuster für jede der zentralen Infrastrukturkomponenten von Cloudbereitstellungen einrichten, die Ihre Richtlinienanforderungen am besten erfüllen.</span><span class="sxs-lookup"><span data-stu-id="61a7b-106">A cloud governance design guide should establish the architecture choices and design patterns for each of the core infrastructure components of cloud deployments that best meet your policy requirements.</span></span> <span data-ttu-id="61a7b-107">Neben diesen sollten Sie eine allgemeine Erläuterung der Technologie, Tools und Prozesse liefern, die jede dieser Entwurfsentscheidungen unterstützen werden.</span><span class="sxs-lookup"><span data-stu-id="61a7b-107">Alongside these you should provide a high-level explanation of the technology, tools, and processes that will support each of these design decisions.</span></span>

<span data-ttu-id="61a7b-108">Obwohl Ihre Risikoanalyse und Richtlinienanweisungen möglicherweise zu einem gewissen Grad unabhängig von der Cloudplattform sein können, sollte Ihr Entwurfshandbuch plattformspezifische Implementierungsdetails für Ihre IT und Entwickler enthalten.</span><span class="sxs-lookup"><span data-stu-id="61a7b-108">Although your risk analysis and policy statements may, to some degree, be cloud platform agnostic, your design guide should provide platform-specific implementation details that your IT and dev.</span></span> <span data-ttu-id="61a7b-109">Konzentrieren Sie sich auf die Architektur, Tools und Features Ihrer gewählten Plattform, wenn Sie Ihre Entwurfsentscheidung treffen und einen Leitfaden bereitstellen.</span><span class="sxs-lookup"><span data-stu-id="61a7b-109">Focus on the architecture, tools, and features of your chosen platform when making design decision and providing guidance.</span></span>

<span data-ttu-id="61a7b-110">Während Cloud-Entwurfshandbücher einige der technischen Details berücksichtigen sollten, die mit den einzelnen Infrastrukturkomponenten verbunden sind, sollen sie keine umfangreichen technischen Dokumente oder Spezifikationen sein.</span><span class="sxs-lookup"><span data-stu-id="61a7b-110">While cloud design guides should take into account some of the technical details associated with each infrastructure component, they are not meant to be extensive technical documents or specifications.</span></span> <span data-ttu-id="61a7b-111">Stellen Sie sicher, dass Ihre Handbücher alle Ihre Richtlinienanweisungen behandeln, und geben Sie die Entwurfsentscheidungen in einem Format an, das für Ihre Mitarbeiter einfach zu verstehen ist und für Bezugnahmen verwendet werden kann.</span><span class="sxs-lookup"><span data-stu-id="61a7b-111">Make sure your guides address all of your policy statements and clearly state design decisions in a format easy for staff to understand and reference.</span></span>

<!-- markdownlint-enable MD033 -->

## <a name="using-the-actionable-governance-journeys"></a><span data-ttu-id="61a7b-112">Verwenden der handlungsrelevanten Governance Journeys</span><span class="sxs-lookup"><span data-stu-id="61a7b-112">Using the actionable governance journeys</span></span>

<span data-ttu-id="61a7b-113">Wenn Sie planen, die Azure-Plattform für Ihre Cloudeinführung zu nutzen, bietet das CAF [Governance Journeys](../journeys/overview.md), um den inkrementellen Ansatz des CAF Governance-Modells zu veranschaulichen.</span><span class="sxs-lookup"><span data-stu-id="61a7b-113">If you're planning to use the Azure platform for your cloud adoption, the CAF provides [governance journeys](../journeys/overview.md) illustrating the incremental approach of the CAF governance model.</span></span> <span data-ttu-id="61a7b-114">Diese beschreibenden Journeys decken einen Bereich von allgemeinen Einführungsszenarien ab, darunter die Geschäftsrisiken, Toleranzanforderungen und Richtlinienanweisungen, die in die Erstellung eines Minimum Viable Product (MVP) für die Governance einbezogen wurden.</span><span class="sxs-lookup"><span data-stu-id="61a7b-114">These narrative journeys cover a range of common adoption scenarios, including the business risks, tolerance requirements, and policy statements that went into creating a governance minimum viable product (MVP).</span></span> <span data-ttu-id="61a7b-115">Diese Journeys stellen eine Synthese aus realen Benutzererfahrungen beim Prozess der Cloudeinführung in Azure dar.</span><span class="sxs-lookup"><span data-stu-id="61a7b-115">These journeys represent a synthesis of real-world customer experience of the cloud adoption process in Azure.</span></span>

<span data-ttu-id="61a7b-116">Während es bei jeder Cloudeinführung eindeutige Ziele, Prioritäten und Herausforderungen gibt, sollten diese Beispiele eine gute Vorlage für die Umwandlung Ihrer Richtlinie in einen Leitfaden liefern.</span><span class="sxs-lookup"><span data-stu-id="61a7b-116">While every cloud adoption has unique goals, priorities, and challenges, these samples should provide a good template for converting your policy into guidance.</span></span> <span data-ttu-id="61a7b-117">Wählen Sie das Ihrer Situation am besten entsprechende Szenario als Ausgangspunkt aus, und passen Sie es an Ihre spezifischen Richtlinienanforderungen an.</span><span class="sxs-lookup"><span data-stu-id="61a7b-117">Pick the closest scenario to your situation as a starting point, and mold it to fit your specific policy needs.</span></span>

## <a name="next-steps"></a><span data-ttu-id="61a7b-118">Nächste Schritte</span><span class="sxs-lookup"><span data-stu-id="61a7b-118">Next steps</span></span>

<span data-ttu-id="61a7b-119">Nach Abschluss des Entwurfsleitfadens richten Sie Prozesse zur Einhaltung von Richtlinien ein, um die Richtlinienkonformität sicherzustellen.</span><span class="sxs-lookup"><span data-stu-id="61a7b-119">With design guidance in place, establish policy adherence processes to ensure policy compliance.</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="61a7b-120">Prozesse zur Einhaltung von Richtlinien</span><span class="sxs-lookup"><span data-stu-id="61a7b-120">Policy adherence processes</span></span>](processes.md)