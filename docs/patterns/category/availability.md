---
title: Verfügbarkeitsmuster
titleSuffix: Cloud Design Patterns
description: Als Verfügbarkeit ist der Zeitanteil definiert, in dem ein System funktioniert und erreichbar ist. Die Verfügbarkeit wird durch Systemfehler, Infrastrukturprobleme, böswillige Angriffe und die Systemauslastung beeinflusst. Sie wird üblicherweise als Prozentsatz der Betriebszeit gemessen. Bei Cloudanwendungen verfügen Benutzer normalerweise über eine Vereinbarung zum Servicelevel (SLA). Dies bedeutet, dass Anwendungen so entworfen und implementiert werden müssen, dass die Verfügbarkeit maximiert wird.
keywords: Entwurfsmuster
author: dragon119
ms.date: 06/23/2017
ms.topic: design-pattern
ms.service: architecture-center
ms.subservice: cloud-fundamentals
ms.custom: seodec18
ms.openlocfilehash: 897bc3c87beb23770220ef0fc3c5c4394d192f2a
ms.sourcegitcommit: 1b50810208354577b00e89e5c031b774b02736e2
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/23/2019
ms.locfileid: "54486794"
---
# <a name="availability-patterns"></a><span data-ttu-id="275a0-107">Verfügbarkeitsmuster</span><span class="sxs-lookup"><span data-stu-id="275a0-107">Availability patterns</span></span>

[!INCLUDE [header](../../_includes/header.md)]

<span data-ttu-id="275a0-108">Als Verfügbarkeit ist der Zeitanteil definiert, in dem ein System funktioniert und erreichbar ist.</span><span class="sxs-lookup"><span data-stu-id="275a0-108">Availability defines the proportion of time that the system is functional and working.</span></span> <span data-ttu-id="275a0-109">Die Verfügbarkeit wird durch Systemfehler, Infrastrukturprobleme, böswillige Angriffe und die Systemauslastung beeinflusst.</span><span class="sxs-lookup"><span data-stu-id="275a0-109">It will be affected by system errors, infrastructure problems, malicious attacks, and system load.</span></span> <span data-ttu-id="275a0-110">Sie wird üblicherweise als Prozentsatz der Betriebszeit gemessen.</span><span class="sxs-lookup"><span data-stu-id="275a0-110">It is usually measured as a percentage of uptime.</span></span> <span data-ttu-id="275a0-111">Bei Cloudanwendungen verfügen Benutzer normalerweise über eine Vereinbarung zum Servicelevel (SLA). Dies bedeutet, dass Anwendungen so entworfen und implementiert werden müssen, dass die Verfügbarkeit maximiert wird.</span><span class="sxs-lookup"><span data-stu-id="275a0-111">Cloud applications typically provide users with a service level agreement (SLA), which means that applications must be designed and implemented in a way that maximizes availability.</span></span>

|                            <span data-ttu-id="275a0-112">Muster</span><span class="sxs-lookup"><span data-stu-id="275a0-112">Pattern</span></span>                             |                                                           <span data-ttu-id="275a0-113">Zusammenfassung</span><span class="sxs-lookup"><span data-stu-id="275a0-113">Summary</span></span>                                                            |
|----------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------|
| [<span data-ttu-id="275a0-114">Überwachung des Integritätsendpunkts</span><span class="sxs-lookup"><span data-stu-id="275a0-114">Health Endpoint Monitoring</span></span>](../health-endpoint-monitoring.md) | <span data-ttu-id="275a0-115">Implementieren Sie Funktionsprüfungen in einer Anwendung, auf die externe Tools in regelmäßigen Abständen über verfügbar gemachte Endpunkte zugreifen können.</span><span class="sxs-lookup"><span data-stu-id="275a0-115">Implement functional checks in an application that external tools can access through exposed endpoints at regular intervals.</span></span> |
|  [<span data-ttu-id="275a0-116">Warteschlangenbasierter Lastenausgleich</span><span class="sxs-lookup"><span data-stu-id="275a0-116">Queue-Based Load Leveling</span></span>](../queue-based-load-leveling.md)  | <span data-ttu-id="275a0-117">Verwenden Sie eine Warteschlange, die als Puffer zwischen einem Task und einem von diesem aufgerufenen Dienst fungiert, um unregelmäßig auftretende hohe Lasten aufzufangen.</span><span class="sxs-lookup"><span data-stu-id="275a0-117">Use a queue that acts as a buffer between a task and a service that it invokes in order to smooth intermittent heavy loads.</span></span>  |
|                 [<span data-ttu-id="275a0-118">Drosselung</span><span class="sxs-lookup"><span data-stu-id="275a0-118">Throttling</span></span>](../throttling.md)                 |   <span data-ttu-id="275a0-119">Steuern Sie den Verbrauch der von einer Anwendungsinstanz, einem einzelnen Mandanten oder einem gesamten Dienst verwendeten Ressourcen.</span><span class="sxs-lookup"><span data-stu-id="275a0-119">Control the consumption of resources used by an instance of an application, an individual tenant, or an entire service.</span></span>    |
