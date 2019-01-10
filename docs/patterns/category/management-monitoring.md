---
title: Verwaltungs- und Überwachungsmuster
titleSuffix: Cloud Design Patterns
description: Cloudanwendungen werden in einem Remoterechenzentrum ausgeführt, in dem Sie keine vollständige Kontrolle über die Infrastruktur oder – in manchen Fällen – das Betriebssystem haben. Die Verwaltung und Überwachung kann daher schwieriger sein als bei einer lokalen Bereitstellung. Anwendungen müssen Laufzeitinformationen verfügbar machen, die Administratoren und Betreiber zum Verwalten und Überwachen des Systems verwenden können. Zudem müssen sie sich ändernde Geschäftsanforderungen und Anpassungen unterstützen, ohne dass die Anwendung beendet oder erneut bereitgestellt werden muss.
keywords: Entwurfsmuster
author: dragon119
ms.date: 06/23/2017
ms.custom: seodec18
ms.openlocfilehash: fc75a3a56323b61651b9e840068a3117b31cf203
ms.sourcegitcommit: 680c9cef945dff6fee5e66b38e24f07804510fa9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/04/2019
ms.locfileid: "54009354"
---
# <a name="management-and-monitoring-patterns"></a>Verwaltungs- und Überwachungsmuster

Cloudanwendungen werden in einem Remoterechenzentrum ausgeführt, in dem Sie keine vollständige Kontrolle über die Infrastruktur oder – in manchen Fällen – das Betriebssystem haben. Die Verwaltung und Überwachung kann daher schwieriger sein als bei einer lokalen Bereitstellung. Anwendungen müssen Laufzeitinformationen verfügbar machen, die Administratoren und Betreiber zum Verwalten und Überwachen des Systems verwenden können. Zudem müssen sie sich ändernde Geschäftsanforderungen und Anpassungen unterstützen, ohne dass die Anwendung beendet oder erneut bereitgestellt werden muss.

|                              Muster                               |                                                              Zusammenfassung                                                              |
|--------------------------------------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------|
|                   [Ambassador](../ambassador.md)                   |                 Erstellen Sie Hilfsdienste, die im Auftrag von Consumerdiensten oder -anwendungen Netzwerkanforderungen senden.                 |
|        [Antikorruptionsschicht](../anti-corruption-layer.md)        |                       Implementieren Sie eine „Fassade“ oder Adapterebene zwischen einer modernen Anwendung und einem älteren System.                       |
| [Externer Konfigurationsspeicher](../external-configuration-store.md) |                Konfigurationsinformationen aus dem Anwendungsbereitstellungspaket an einen zentralen Speicherort verschieben                |
|          [Gatewayaggregation](../gateway-aggregation.md)          |                          Aggregieren Sie mithilfe eines Gateways mehrere einzelne Anforderungen in einer einzigen Anforderung.                           |
|           [Gatewayabladung](../gateway-offloading.md)           |                              Lagern Sie gemeinsam genutzte oder spezielle Dienstfunktionen an einen Gatewayproxy aus.                              |
|              [Gatewayrouting](../gateway-routing.md)              |                                   Leiten Sie Anforderungen an mehrere Dienste mit einem einzelnen Endpunkt weiter.                                    |
|   [Überwachung des Integritätsendpunkts](../health-endpoint-monitoring.md)   |   Implementieren Sie funktionale Prüfungen in einer Anwendung, auf die externe Tools in regelmäßigen Abständen über verfügbar gemachte Endpunkte zugreifen können.    |
|                      [Sidecar](../sidecar.md)                      |         Komponenten einer Anwendung zwecks Isolation und Kapselung in einem separaten Prozess oder Container bereitstellen          |
|                    [Strangler](../strangler.md)                    | Migrieren Sie ein älteres System inkrementell, indem Sie bestimmte Teile der Funktionalität nach und nach durch neue Anwendungen und Dienste ersetzen. |
