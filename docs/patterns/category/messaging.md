---
title: Nachrichtenmuster
titleSuffix: Cloud Design Patterns
description: Die verteilte Struktur von Cloudanwendungen erfordert eine Messaginginfrastruktur, die die Komponenten und Dienste – idealerweise lose gekoppelt – miteinander verbindet, um die Skalierbarkeit zu maximieren. Asynchrones Messaging ist weit verbreitet und bietet viele Vorteile, beinhaltet aber auch Herausforderungen, beispielsweise die Reihenfolge von Nachrichten, die Verwaltung von nicht verarbeitbaren Nachrichten, Idempotenz und vieles mehr.
keywords: Entwurfsmuster
author: dragon119
ms.date: 12/07/2018
ms.custom: seodec18
ms.openlocfilehash: 4619d30c152f050f3f95aee3f9983b8fe85911ed
ms.sourcegitcommit: 680c9cef945dff6fee5e66b38e24f07804510fa9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/04/2019
ms.locfileid: "54011326"
---
# <a name="messaging-patterns"></a><span data-ttu-id="19271-105">Nachrichtenmuster</span><span class="sxs-lookup"><span data-stu-id="19271-105">Messaging patterns</span></span>

[!INCLUDE [header](../../_includes/header.md)]

<span data-ttu-id="19271-106">Die verteilte Struktur von Cloudanwendungen erfordert eine Messaginginfrastruktur, die die Komponenten und Dienste – idealerweise lose gekoppelt – miteinander verbindet, um die Skalierbarkeit zu maximieren.</span><span class="sxs-lookup"><span data-stu-id="19271-106">The distributed nature of cloud applications requires a messaging infrastructure that connects the components and services, ideally in a loosely coupled manner in order to maximize scalability.</span></span> <span data-ttu-id="19271-107">Asynchrones Messaging ist weit verbreitet und bietet viele Vorteile, beinhaltet aber auch Herausforderungen, beispielsweise die Reihenfolge von Nachrichten, die Verwaltung von nicht verarbeitbaren Nachrichten, Idempotenz und vieles mehr.</span><span class="sxs-lookup"><span data-stu-id="19271-107">Asynchronous messaging is widely used, and provides many benefits, but also brings challenges such as the ordering of messages, poison message management, idempotency, and more.</span></span>

| <span data-ttu-id="19271-108">Muster</span><span class="sxs-lookup"><span data-stu-id="19271-108">Pattern</span></span> | <span data-ttu-id="19271-109">Zusammenfassung</span><span class="sxs-lookup"><span data-stu-id="19271-109">Summary</span></span> |
| ------- | ------- |
| [<span data-ttu-id="19271-110">Konkurrierende Consumer</span><span class="sxs-lookup"><span data-stu-id="19271-110">Competing Consumers</span></span>](../competing-consumers.md) | <span data-ttu-id="19271-111">Mehreren gleichzeitigen Consumern die Verarbeitung von Nachrichten ermöglichen, die auf dem gleichen Messagingkanal empfangen werden</span><span class="sxs-lookup"><span data-stu-id="19271-111">Enable multiple concurrent consumers to process messages received on the same messaging channel.</span></span> |
| [<span data-ttu-id="19271-112">Pipes und Filter</span><span class="sxs-lookup"><span data-stu-id="19271-112">Pipes and Filters</span></span>](../pipes-and-filters.md) | <span data-ttu-id="19271-113">Unterteilen einer Aufgabe, die komplexe Verarbeitungsvorgänge ausführt, in eine Reihe wiederverwendbarer separater Elemente</span><span class="sxs-lookup"><span data-stu-id="19271-113">Break down a task that performs complex processing into a series of separate elements that can be reused.</span></span> |
| [<span data-ttu-id="19271-114">Prioritätswarteschlange</span><span class="sxs-lookup"><span data-stu-id="19271-114">Priority Queue</span></span>](../priority-queue.md) | <span data-ttu-id="19271-115">Priorisieren Sie an Dienste gesendete Anforderungen, sodass Anforderungen mit einer höheren Priorität schneller empfangen und verarbeitet werden als Anforderungen mit einer niedrigeren Priorität.</span><span class="sxs-lookup"><span data-stu-id="19271-115">Prioritize requests sent to services so that requests with a higher priority are received and processed more quickly than those with a lower priority.</span></span> |
| [<span data-ttu-id="19271-116">Publisher-Subscriber</span><span class="sxs-lookup"><span data-stu-id="19271-116">Publisher-Subscriber</span></span>](../publisher-subscriber.md) | <span data-ttu-id="19271-117">Ermöglichen Sie einer Anwendung die asynchrone Ankündigung von Ereignissen für mehrere interessierte Consumer, ohne die Absender mit den Empfängern zu koppeln.</span><span class="sxs-lookup"><span data-stu-id="19271-117">Enable an application to announce events to multiple interested consumers aynchronously, without coupling the senders to the receivers.</span></span> |
| [<span data-ttu-id="19271-118">Warteschlangenbasierter Lastenausgleich</span><span class="sxs-lookup"><span data-stu-id="19271-118">Queue-Based Load Leveling</span></span>](../queue-based-load-leveling.md) | <span data-ttu-id="19271-119">Verwenden Sie eine Warteschlange, die als Puffer zwischen einem Task und einem von diesem aufgerufenen Dienst fungiert, um unregelmäßig auftretende hohe Lasten aufzufangen.</span><span class="sxs-lookup"><span data-stu-id="19271-119">Use a queue that acts as a buffer between a task and a service that it invokes in order to smooth intermittent heavy loads.</span></span> |
| [<span data-ttu-id="19271-120">Scheduler-Agent-Supervisor</span><span class="sxs-lookup"><span data-stu-id="19271-120">Scheduler Agent Supervisor</span></span>](../scheduler-agent-supervisor.md) | <span data-ttu-id="19271-121">Koordinieren Sie eine Reihe von Aktionen in einer verteilten Gruppe von Diensten und anderen Remoteressourcen.</span><span class="sxs-lookup"><span data-stu-id="19271-121">Coordinate a set of actions across a distributed set of services and other remote resources.</span></span> |
