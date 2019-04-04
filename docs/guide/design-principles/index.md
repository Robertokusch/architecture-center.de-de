---
title: Entwurfsprinzipien für Azure-Anwendungen
titleSuffix: Azure Application Architecture Guide
description: Entwurfsprinzipien für Azure-Anwendungen
author: MikeWasson
ms.date: 08/30/2018
ms.topic: guide
ms.service: architecture-center
ms.subservice: reference-architecture
ms.custom: seojan19
ms.openlocfilehash: 8aab710ef6ffde493b80810750d2c0bc299ffaa6
ms.sourcegitcommit: c053e6edb429299a0ad9b327888d596c48859d4a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/20/2019
ms.locfileid: "58344256"
---
# <a name="ten-design-principles-for-azure-applications"></a><span data-ttu-id="a0ca4-103">Zehn Entwurfsprinzipien für Azure-Anwendungen</span><span class="sxs-lookup"><span data-stu-id="a0ca4-103">Ten design principles for Azure applications</span></span>

<span data-ttu-id="a0ca4-104">Befolgen Sie die folgenden Entwurfsprinzipien, um die Skalierbarkeit, Resilienz und Verwaltbarkeit Ihrer Anwendung zu optimieren.</span><span class="sxs-lookup"><span data-stu-id="a0ca4-104">Follow these design principles to make your application more scalable, resilient, and manageable.</span></span>

<span data-ttu-id="a0ca4-105">**[Entwurf mit Blick auf Selbstreparatur](self-healing.md)**:</span><span class="sxs-lookup"><span data-stu-id="a0ca4-105">**[Design for self healing](self-healing.md)**.</span></span> <span data-ttu-id="a0ca4-106">In einem verteilten System kann es zu Ausfällen kommen.</span><span class="sxs-lookup"><span data-stu-id="a0ca4-106">In a distributed system, failures happen.</span></span> <span data-ttu-id="a0ca4-107">Entwerfen Sie Ihre Anwendung so, dass sie bei Ausfällen eine Selbstreparatur durchführt.</span><span class="sxs-lookup"><span data-stu-id="a0ca4-107">Design your application to be self healing when failures occur.</span></span>

<span data-ttu-id="a0ca4-108">**[Herstellen von Redundanz für alle Anwendungskomponenten](redundancy.md)**:</span><span class="sxs-lookup"><span data-stu-id="a0ca4-108">**[Make all things redundant](redundancy.md)**.</span></span> <span data-ttu-id="a0ca4-109">Schaffen Sie Redundanz in Ihrer Anwendung, um Ausfälle einzelner Komponenten zu verhindern.</span><span class="sxs-lookup"><span data-stu-id="a0ca4-109">Build redundancy into your application, to avoid having single points of failure.</span></span>

<span data-ttu-id="a0ca4-110">**[Minimieren der Koordinierung](minimize-coordination.md)**:</span><span class="sxs-lookup"><span data-stu-id="a0ca4-110">**[Minimize coordination](minimize-coordination.md)**.</span></span> <span data-ttu-id="a0ca4-111">Minimieren Sie die Koordinierung zwischen Anwendungsdiensten, um Skalierbarkeit zu erzielen.</span><span class="sxs-lookup"><span data-stu-id="a0ca4-111">Minimize coordination between application services to achieve scalability.</span></span>

<span data-ttu-id="a0ca4-112">**[Ausrichtung des Entwurfs auf horizontale Skalierung](scale-out.md)**: Entwerfen Sie Ihre Anwendung so, dass sie durch Hinzufügen oder Entfernen neuer Instanzen je nach Bedarf horizontal skaliert werden kann.</span><span class="sxs-lookup"><span data-stu-id="a0ca4-112">**[Design to scale out](scale-out.md)**. Design your application so that it can scale horizontally, adding or removing new instances as demand requires.</span></span>

<span data-ttu-id="a0ca4-113">**[Umgehung von Beschränkungen durch Partitionierung](partition.md)**:</span><span class="sxs-lookup"><span data-stu-id="a0ca4-113">**[Partition around limits](partition.md)**.</span></span> <span data-ttu-id="a0ca4-114">Setzen Sie Partitionierung ein, um Datenbank-, Netzwerk- und Computebeschränkungen zu umgehen.</span><span class="sxs-lookup"><span data-stu-id="a0ca4-114">Use partitioning to work around database, network, and compute limits.</span></span>

<span data-ttu-id="a0ca4-115">**[Entwurf mit Blick auf den Betrieb](design-for-operations.md)**:</span><span class="sxs-lookup"><span data-stu-id="a0ca4-115">**[Design for operations](design-for-operations.md)**.</span></span> <span data-ttu-id="a0ca4-116">Setzen Sie es sich beim Entwurf Ihre Anwendung als Ziel, dem Betriebsteam die benötigten Verwaltungstools zur Verfügung zu stellen.</span><span class="sxs-lookup"><span data-stu-id="a0ca4-116">Design your application so that the operations team has the tools they need.</span></span>

<span data-ttu-id="a0ca4-117">**[Verwendung verwalteter Dienste](managed-services.md)**:</span><span class="sxs-lookup"><span data-stu-id="a0ca4-117">**[Use managed services](managed-services.md)**.</span></span> <span data-ttu-id="a0ca4-118">Verwenden Sie nach Möglichkeit Platform as a Service (PaaS) statt Infrastructure-as-a-Service (IaaS).</span><span class="sxs-lookup"><span data-stu-id="a0ca4-118">When possible, use platform as a service (PaaS) rather than infrastructure as a service (IaaS).</span></span>

<span data-ttu-id="a0ca4-119">**[Verwendung des idealen Datenspeichers](use-the-best-data-store.md)**:</span><span class="sxs-lookup"><span data-stu-id="a0ca4-119">**[Use the best data store for the job](use-the-best-data-store.md)**.</span></span> <span data-ttu-id="a0ca4-120">Wählen Sie die Speichertechnologie, die am besten für Ihre Daten und den vorgesehenen Einsatzzweck geeignet ist.</span><span class="sxs-lookup"><span data-stu-id="a0ca4-120">Pick the storage technology that is the best fit for your data and how it will be used.</span></span>

<span data-ttu-id="a0ca4-121">**[Entwurf mit Blick auf die Entwicklung](design-for-evolution.md)**:</span><span class="sxs-lookup"><span data-stu-id="a0ca4-121">**[Design for evolution](design-for-evolution.md)**.</span></span> <span data-ttu-id="a0ca4-122">Erfolgreiche Anwendungen ändern sich im Laufe der Zeit.</span><span class="sxs-lookup"><span data-stu-id="a0ca4-122">All successful applications change over time.</span></span> <span data-ttu-id="a0ca4-123">Ein evolutionärer Entwurf ist für einen kontinuierlichen Innovationsstrom von entscheidender Bedeutung.</span><span class="sxs-lookup"><span data-stu-id="a0ca4-123">An evolutionary design is key for continuous innovation.</span></span>

<span data-ttu-id="a0ca4-124">**[Ausrichtung des Entwurfs auf die Unternehmensanforderungen](build-for-business.md)**:</span><span class="sxs-lookup"><span data-stu-id="a0ca4-124">**[Build for the needs of business](build-for-business.md)**.</span></span> <span data-ttu-id="a0ca4-125">Jede Entwurfsentscheidung muss durch eine geschäftliche Anforderung gerechtfertigt sein.</span><span class="sxs-lookup"><span data-stu-id="a0ca4-125">Every design decision must be justified by a business requirement.</span></span>
