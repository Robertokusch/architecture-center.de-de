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
# <a name="messaging-patterns"></a>Nachrichtenmuster

[!INCLUDE [header](../../_includes/header.md)]

Die verteilte Struktur von Cloudanwendungen erfordert eine Messaginginfrastruktur, die die Komponenten und Dienste – idealerweise lose gekoppelt – miteinander verbindet, um die Skalierbarkeit zu maximieren. Asynchrones Messaging ist weit verbreitet und bietet viele Vorteile, beinhaltet aber auch Herausforderungen, beispielsweise die Reihenfolge von Nachrichten, die Verwaltung von nicht verarbeitbaren Nachrichten, Idempotenz und vieles mehr.

| Muster | Zusammenfassung |
| ------- | ------- |
| [Konkurrierende Consumer](../competing-consumers.md) | Mehreren gleichzeitigen Consumern die Verarbeitung von Nachrichten ermöglichen, die auf dem gleichen Messagingkanal empfangen werden |
| [Pipes und Filter](../pipes-and-filters.md) | Unterteilen einer Aufgabe, die komplexe Verarbeitungsvorgänge ausführt, in eine Reihe wiederverwendbarer separater Elemente |
| [Prioritätswarteschlange](../priority-queue.md) | Priorisieren Sie an Dienste gesendete Anforderungen, sodass Anforderungen mit einer höheren Priorität schneller empfangen und verarbeitet werden als Anforderungen mit einer niedrigeren Priorität. |
| [Publisher-Subscriber](../publisher-subscriber.md) | Ermöglichen Sie einer Anwendung die asynchrone Ankündigung von Ereignissen für mehrere interessierte Consumer, ohne die Absender mit den Empfängern zu koppeln. |
| [Warteschlangenbasierter Lastenausgleich](../queue-based-load-leveling.md) | Verwenden Sie eine Warteschlange, die als Puffer zwischen einem Task und einem von diesem aufgerufenen Dienst fungiert, um unregelmäßig auftretende hohe Lasten aufzufangen. |
| [Scheduler-Agent-Supervisor](../scheduler-agent-supervisor.md) | Koordinieren Sie eine Reihe von Aktionen in einer verteilten Gruppe von Diensten und anderen Remoteressourcen. |
