---
title: Datenverwaltungsmuster
titleSuffix: Cloud Design Patterns
description: Die Datenverwaltung ist das wichtigste Element von Cloudanwendungen und wirkt sich auf die meisten Qualitätsattribute aus. Meist werden Daten aus Gründen der Leistung, Skalierbarkeit oder Verfügbarkeit an verschiedenen Speicherorten und auf mehreren Servern gehostet, und dies kann eine ganze Reihe von Herausforderungen mit sich bringen. Beispielsweise muss die Konsistenz der Daten aufrechterhalten werden, und normalerweise müssen Daten zwischen unterschiedlichen Speicherorten synchronisiert werden.
keywords: Entwurfsmuster
author: dragon119
ms.date: 06/23/2017
ms.topic: design-pattern
ms.service: architecture-center
ms.subservice: cloud-fundamentals
ms.custom: seodec18
ms.openlocfilehash: 25571a431836656856ed3f299455dfdb94ae3477
ms.sourcegitcommit: c053e6edb429299a0ad9b327888d596c48859d4a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/20/2019
ms.locfileid: "58243051"
---
# <a name="data-management-patterns"></a>Datenverwaltungsmuster

[!INCLUDE [header](../../_includes/header.md)]

Die Datenverwaltung ist das wichtigste Element von Cloudanwendungen und wirkt sich auf die meisten Qualitätsattribute aus. Meist werden Daten aus Gründen der Leistung, Skalierbarkeit oder Verfügbarkeit an verschiedenen Speicherorten und auf mehreren Servern gehostet, und dies kann eine ganze Reihe von Herausforderungen mit sich bringen. Beispielsweise muss die Konsistenz der Daten aufrechterhalten werden, und normalerweise müssen Daten zwischen unterschiedlichen Speicherorten synchronisiert werden.

|                        Muster                         |                                                                  Zusammenfassung                                                                  |
|--------------------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------|
|            [Cache-Aside](../cache-aside.md)            |                                            Daten bei Bedarf aus einem Datenspeicher in einen Cache laden                                             |
|                   [CQRS](../cqrs.md)                   |                    Mithilfe separater Schnittstellen Vorgänge trennen, die Daten von Vorgängen zur Aktualisierung von Daten lesen                     |
|         [Ereignisherkunftsermittlung](../event-sourcing.md)         |               Einen nur zum Anfügen vorgesehenen Speicher verwenden, um die vollständige Serie von Ereignissen aufzuzeichnen, die an Daten in einer Domäne ausgeführte Aktionen beschreiben               |
|            [Indextabelle](../index-table.md)            |                         Erstellen Sie Indizes für die Felder im Datenspeicher, auf die häufig von Abfragen verwiesen wird.                          |
|      [Materialisierte Sicht](../materialized-view.md)      | Voraufgefüllte Sichten für die Daten in einem oder mehreren Datenspeichern generieren, wenn die Daten für erforderliche Abfragevorgänge nicht ideal formatiert sind |
|               [Sharding](../sharding.md)               |                                    Einen Datenspeicher in einen Satz horizontaler Partitionen oder Shards unterteilen                                     |
| [Hosten von statischen Inhalten](../static-content-hosting.md) |                   Stellen Sie statische Inhalte in einem cloudbasierten Speicherdienst bereit, der die Inhalte direkt an den Client übermitteln kann.                    |
|              [Valet-Schlüssel](../valet-key.md)              |                 Ein Token oder einen Schlüssel verwenden, das bzw. der Clients eingeschränkten direkten Zugriff auf eine bestimmte Ressource oder einen bestimmten Dienst bietet                 |
