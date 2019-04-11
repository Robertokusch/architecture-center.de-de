---
title: 'Framework für die Cloudeinführung (Cloud Adoption Framework, CAF): Was ist die Datenklassifizierung?'
titleSuffix: Microsoft Cloud Adoption Framework for Azure
ms.service: architecture-center
ms.subservice: enterprise-cloud-adoption
ms.custom: governance
ms.date: 02/11/2019
description: Was ist die Datenklassifizierung?
author: BrianBlanchard
ms.openlocfilehash: bb40745b02e4ad6d7faa7054bf62443964f6784b
ms.sourcegitcommit: c053e6edb429299a0ad9b327888d596c48859d4a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/20/2019
ms.locfileid: "58245141"
---
<!-- markdownlint-disable MD026 -->

# <a name="what-is-data-classification"></a><span data-ttu-id="533cd-103">Was ist die Datenklassifizierung?</span><span class="sxs-lookup"><span data-stu-id="533cd-103">What is data classification?</span></span>

<span data-ttu-id="533cd-104">Dieser Artikel ist eine Einführung in das allgemeine Thema „Datenklassifizierung“.</span><span class="sxs-lookup"><span data-stu-id="533cd-104">This is an introductory article on the general topic of Data Classification.</span></span> <span data-ttu-id="533cd-105">Die Datenklassifizierung ist ein sehr häufiger Ausgangspunkt für die gesamte Governance.</span><span class="sxs-lookup"><span data-stu-id="533cd-105">Data classification is a very common starting point for all governance.</span></span>

## <a name="business-risks-and-governance"></a><span data-ttu-id="533cd-106">Geschäftsrisiken und Governance</span><span class="sxs-lookup"><span data-stu-id="533cd-106">Business risks and governance</span></span>

<span data-ttu-id="533cd-107">In den meisten Organisationen können die Hauptgründe für eine Investition in Governance auf drei Geschäftsrisiken reduziert werden:</span><span class="sxs-lookup"><span data-stu-id="533cd-107">In most organizations, the primary reasons for investing in governance can be reduced to three business risks:</span></span>

* <span data-ttu-id="533cd-108">Haftung im Zusammenhang mit Datenpannen</span><span class="sxs-lookup"><span data-stu-id="533cd-108">Liability associated with data breaches</span></span>
* <span data-ttu-id="533cd-109">Geschäftsunterbrechung infolge von Ausfällen</span><span class="sxs-lookup"><span data-stu-id="533cd-109">Interruption to the business from outages</span></span>
* <span data-ttu-id="533cd-110">Nicht geplante bzw. unerwartete Ausgaben</span><span class="sxs-lookup"><span data-stu-id="533cd-110">Unplanned or unexpected spending</span></span>

<span data-ttu-id="533cd-111">Es gibt viele Varianten dieser drei Geschäftsrisiken.</span><span class="sxs-lookup"><span data-stu-id="533cd-111">There are many variants of these three business risks.</span></span> <span data-ttu-id="533cd-112">Die hier aufgeführten Risiken sind jedoch die gängigsten.</span><span class="sxs-lookup"><span data-stu-id="533cd-112">However, these tend to be the most common.</span></span>

## <a name="understand-then-mitigate"></a><span data-ttu-id="533cd-113">Verstehen, dann mindern</span><span class="sxs-lookup"><span data-stu-id="533cd-113">Understand then mitigate</span></span>

<span data-ttu-id="533cd-114">Bevor ein Risiko gemindert werden kann, muss es verstanden werden.</span><span class="sxs-lookup"><span data-stu-id="533cd-114">Before any risk can be mitigated, it must be understood.</span></span> <span data-ttu-id="533cd-115">Im Fall der Haftung bei Datenpannen beginnt dieses Verstehen mit der Datenklassifizierung.</span><span class="sxs-lookup"><span data-stu-id="533cd-115">In the case of data breach liability, that understanding starts with data classification.</span></span> <span data-ttu-id="533cd-116">Datenklassifizierung ist der Prozess, in dem jedem Objekt in einer digitalen Ressource ein Metadatenmerkmal zugeordnet wird, das den mit diesem Objekt verknüpften Datentyp identifiziert.</span><span class="sxs-lookup"><span data-stu-id="533cd-116">Data classification is the process of associating a meta data characteristic to every asset in a digital estate, which identifies the type of data associated with that asset.</span></span>

<span data-ttu-id="533cd-117">Microsoft empfiehlt, dass es zu einem Objekt, das als möglicher Kandidat für Migration in oder Bereitstellung für die Cloud identifiziert wurde, dokumentierte Metadaten zum Aufzeichnen der Datenklassifizierung, geschäftlichen Bedeutung und Verantwortung für Abrechnung geben sollte.</span><span class="sxs-lookup"><span data-stu-id="533cd-117">Microsoft suggests that any asset which has been identified as a potential candidate for migration or deployment to the cloud should have documented meta data to record the data classification, business criticality, and billing responsibility.</span></span> <span data-ttu-id="533cd-118">Diese drei Aspekte der Klassifizierung können zum Verstehen und Mindern von Risiken beitragen.</span><span class="sxs-lookup"><span data-stu-id="533cd-118">These three points of classification can go a long way to understanding and mitigating risks.</span></span>

## <a name="microsofts-data-classification"></a><span data-ttu-id="533cd-119">Datenklassifizierung bei Microsoft</span><span class="sxs-lookup"><span data-stu-id="533cd-119">Microsoft's data classification</span></span>

<span data-ttu-id="533cd-120">In der nachstehenden Liste sind die von Microsoft verwendeten Klassifizierungen aufgeführt.</span><span class="sxs-lookup"><span data-stu-id="533cd-120">The following is a list of classifications Microsoft uses.</span></span> <span data-ttu-id="533cd-121">Je nach Ihrer Branche oder vorhandenen Sicherheitsanforderungen gibt es in Ihrer Organisation vielleicht bereits Standards für Datenklassifizierungen.</span><span class="sxs-lookup"><span data-stu-id="533cd-121">Depending on your industry or existing security requirements, data classifications standards may already exist within your organization.</span></span> <span data-ttu-id="533cd-122">Wenn es keinen Standard gibt, können Sie gerne diese Beispielklassifizierung verwenden, um Ihre digitalen Ressourcen und Ihr Risikoprofil besser zu verstehen.</span><span class="sxs-lookup"><span data-stu-id="533cd-122">If no standard exists, we welcome you to use this sample classification, to help you better understand your digital estate and risk profile.</span></span>  

* <span data-ttu-id="533cd-123">**Nicht geschäftlich:** Daten aus Ihrem Privatleben, die nicht zu Microsoft gehören</span><span class="sxs-lookup"><span data-stu-id="533cd-123">**Non-Business:** Data from your personal life that does not belong to Microsoft</span></span>
* <span data-ttu-id="533cd-124">**Öffentlich:** Geschäftsdaten, die frei verfügbar sind und für den öffentlichen Gebrauch genehmigt werden</span><span class="sxs-lookup"><span data-stu-id="533cd-124">**Public:** Business data that is freely available and approved for public consumption</span></span>
* <span data-ttu-id="533cd-125">**Allgemein:** Geschäftsdaten, die nicht für eine öffentliche Zielgruppe bestimmt sind</span><span class="sxs-lookup"><span data-stu-id="533cd-125">**General:** Business data that is not meant for a public audience</span></span>
* <span data-ttu-id="533cd-126">**Vertraulich:** Geschäftsdaten, die Microsoft Schaden zufügen könnten, wenn sie übermäßig freigegeben werden</span><span class="sxs-lookup"><span data-stu-id="533cd-126">**Confidential:** Business data that could cause harm to Microsoft if over-shared</span></span>
* <span data-ttu-id="533cd-127">**Streng vertraulich:** Geschäftsdaten, die Microsoft großen Schaden zufügen könnten, wenn sie übermäßig freigegeben werden</span><span class="sxs-lookup"><span data-stu-id="533cd-127">**Highly Confidential:** Business data that would cause extensive harm to Microsoft if over-shared</span></span>

## <a name="tagging-data-classification-in-azure"></a><span data-ttu-id="533cd-128">Kennzeichnen der Datenklassifizierung in Azure</span><span class="sxs-lookup"><span data-stu-id="533cd-128">Tagging data classification in Azure</span></span>

<span data-ttu-id="533cd-129">Jeder Cloud-Dienstanbieter sollte einen Mechanismus zum Aufzeichnen von Metadaten zu einem Objekt anbieten.</span><span class="sxs-lookup"><span data-stu-id="533cd-129">Every cloud provider should offer a mechanism for recording metadata about any asset.</span></span> <span data-ttu-id="533cd-130">Metadaten sind sehr wichtig für die Verwaltung von Objekten in der Cloud.</span><span class="sxs-lookup"><span data-stu-id="533cd-130">Metadata is vital to managing assets in the cloud.</span></span> <span data-ttu-id="533cd-131">Im Fall von Azure sind Ressourcentags die empfohlene Vorgehensweise für Metadatenspeicher.</span><span class="sxs-lookup"><span data-stu-id="533cd-131">In the case of Azure, resource tags are the suggested approach for metadata storage.</span></span> <span data-ttu-id="533cd-132">Zusätzliche Informationen zu Tags für Ressourcen in Azure finden Sie im Artikel zu [Verwenden von Tags zum Organisieren Ihrer Azure-Ressourcen](/azure/azure-resource-manager/resource-group-using-tags).</span><span class="sxs-lookup"><span data-stu-id="533cd-132">For additional information on resource tagging in Azure, see the article on [Using tags to organize your Azure resources](/azure/azure-resource-manager/resource-group-using-tags).</span></span>

## <a name="next-steps"></a><span data-ttu-id="533cd-133">Nächste Schritte</span><span class="sxs-lookup"><span data-stu-id="533cd-133">Next steps</span></span>

<span data-ttu-id="533cd-134">Wenden Sie Datenklassifizierungen während einer der handlungsrelevanten Governance Journeys an.</span><span class="sxs-lookup"><span data-stu-id="533cd-134">Apply data classifications during one of the actionable governance journeys.</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="533cd-135">Nützliche Governance-Vorgehensweisen</span><span class="sxs-lookup"><span data-stu-id="533cd-135">Begin an Actionable Governance Journey</span></span>](../journeys/overview.md)
