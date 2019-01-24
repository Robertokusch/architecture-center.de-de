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
# <a name="availability-patterns"></a>Verfügbarkeitsmuster

[!INCLUDE [header](../../_includes/header.md)]

Als Verfügbarkeit ist der Zeitanteil definiert, in dem ein System funktioniert und erreichbar ist. Die Verfügbarkeit wird durch Systemfehler, Infrastrukturprobleme, böswillige Angriffe und die Systemauslastung beeinflusst. Sie wird üblicherweise als Prozentsatz der Betriebszeit gemessen. Bei Cloudanwendungen verfügen Benutzer normalerweise über eine Vereinbarung zum Servicelevel (SLA). Dies bedeutet, dass Anwendungen so entworfen und implementiert werden müssen, dass die Verfügbarkeit maximiert wird.

|                            Muster                             |                                                           Zusammenfassung                                                            |
|----------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------|
| [Überwachung des Integritätsendpunkts](../health-endpoint-monitoring.md) | Implementieren Sie Funktionsprüfungen in einer Anwendung, auf die externe Tools in regelmäßigen Abständen über verfügbar gemachte Endpunkte zugreifen können. |
|  [Warteschlangenbasierter Lastenausgleich](../queue-based-load-leveling.md)  | Verwenden Sie eine Warteschlange, die als Puffer zwischen einem Task und einem von diesem aufgerufenen Dienst fungiert, um unregelmäßig auftretende hohe Lasten aufzufangen.  |
|                 [Drosselung](../throttling.md)                 |   Steuern Sie den Verbrauch der von einer Anwendungsinstanz, einem einzelnen Mandanten oder einem gesamten Dienst verwendeten Ressourcen.    |
