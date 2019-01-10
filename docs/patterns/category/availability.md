---
title: Verfügbarkeitsmuster
titleSuffix: Cloud Design Patterns
description: Als Verfügbarkeit ist der Zeitanteil definiert, in dem ein System funktioniert und erreichbar ist. Die Verfügbarkeit wird durch Systemfehler, Infrastrukturprobleme, böswillige Angriffe und die Systemauslastung beeinflusst. Sie wird üblicherweise als Prozentsatz der Betriebszeit gemessen. Bei Cloudanwendungen verfügen Benutzer normalerweise über eine Vereinbarung zum Servicelevel (SLA). Dies bedeutet, dass Anwendungen so entworfen und implementiert werden müssen, dass die Verfügbarkeit maximiert wird.
keywords: Entwurfsmuster
author: dragon119
ms.date: 06/23/2017
ms.custom: seodec18
ms.openlocfilehash: d256ddd39ca3e8a452162b7b75d67f26f7bb22c2
ms.sourcegitcommit: 680c9cef945dff6fee5e66b38e24f07804510fa9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/04/2019
ms.locfileid: "54009813"
---
# <a name="availability-patterns"></a><span data-ttu-id="c65df-107">Verfügbarkeitsmuster</span><span class="sxs-lookup"><span data-stu-id="c65df-107">Availability patterns</span></span>

[!INCLUDE [header](../../_includes/header.md)]

<span data-ttu-id="c65df-108">Als Verfügbarkeit ist der Zeitanteil definiert, in dem ein System funktioniert und erreichbar ist.</span><span class="sxs-lookup"><span data-stu-id="c65df-108">Availability defines the proportion of time that the system is functional and working.</span></span> <span data-ttu-id="c65df-109">Die Verfügbarkeit wird durch Systemfehler, Infrastrukturprobleme, böswillige Angriffe und die Systemauslastung beeinflusst.</span><span class="sxs-lookup"><span data-stu-id="c65df-109">It will be affected by system errors, infrastructure problems, malicious attacks, and system load.</span></span> <span data-ttu-id="c65df-110">Sie wird üblicherweise als Prozentsatz der Betriebszeit gemessen.</span><span class="sxs-lookup"><span data-stu-id="c65df-110">It is usually measured as a percentage of uptime.</span></span> <span data-ttu-id="c65df-111">Bei Cloudanwendungen verfügen Benutzer normalerweise über eine Vereinbarung zum Servicelevel (SLA). Dies bedeutet, dass Anwendungen so entworfen und implementiert werden müssen, dass die Verfügbarkeit maximiert wird.</span><span class="sxs-lookup"><span data-stu-id="c65df-111">Cloud applications typically provide users with a service level agreement (SLA), which means that applications must be designed and implemented in a way that maximizes availability.</span></span>

|                            <span data-ttu-id="c65df-112">Muster</span><span class="sxs-lookup"><span data-stu-id="c65df-112">Pattern</span></span>                             |                                                           <span data-ttu-id="c65df-113">Zusammenfassung</span><span class="sxs-lookup"><span data-stu-id="c65df-113">Summary</span></span>                                                            |
|----------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------|
| [<span data-ttu-id="c65df-114">Überwachung des Integritätsendpunkts</span><span class="sxs-lookup"><span data-stu-id="c65df-114">Health Endpoint Monitoring</span></span>](../health-endpoint-monitoring.md) | <span data-ttu-id="c65df-115">Implementieren Sie Funktionsprüfungen in einer Anwendung, auf die externe Tools in regelmäßigen Abständen über verfügbar gemachte Endpunkte zugreifen können.</span><span class="sxs-lookup"><span data-stu-id="c65df-115">Implement functional checks in an application that external tools can access through exposed endpoints at regular intervals.</span></span> |
|  [<span data-ttu-id="c65df-116">Warteschlangenbasierter Lastenausgleich</span><span class="sxs-lookup"><span data-stu-id="c65df-116">Queue-Based Load Leveling</span></span>](../queue-based-load-leveling.md)  | <span data-ttu-id="c65df-117">Verwenden Sie eine Warteschlange, die als Puffer zwischen einem Task und einem von diesem aufgerufenen Dienst fungiert, um unregelmäßig auftretende hohe Lasten aufzufangen.</span><span class="sxs-lookup"><span data-stu-id="c65df-117">Use a queue that acts as a buffer between a task and a service that it invokes in order to smooth intermittent heavy loads.</span></span>  |
|                 [<span data-ttu-id="c65df-118">Drosselung</span><span class="sxs-lookup"><span data-stu-id="c65df-118">Throttling</span></span>](../throttling.md)                 |   <span data-ttu-id="c65df-119">Steuern Sie den Verbrauch der von einer Anwendungsinstanz, einem einzelnen Mandanten oder einem gesamten Dienst verwendeten Ressourcen.</span><span class="sxs-lookup"><span data-stu-id="c65df-119">Control the consumption of resources used by an instance of an application, an individual tenant, or an entire service.</span></span>    |
