---
title: Entwurfsmuster für Microservices
description: Entwurfsmuster zum Implementieren einer stabilen Microservices-Architektur.
author: MikeWasson
ms.date: 02/25/2019
ms.topic: guide
ms.service: architecture-center
ms.subservice: reference-architecture
ms.custom: microservices
ms.openlocfilehash: 8cdbf95c753770910fa4a94384c9809db9fda746
ms.sourcegitcommit: c053e6edb429299a0ad9b327888d596c48859d4a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/20/2019
ms.locfileid: "58243571"
---
# <a name="design-patterns-for-microservices"></a>Entwurfsmuster für Microservices

Microservices dienen zur Beschleunigung von Anwendungsreleases. Hierzu wird die Anwendung in kleine autonome Dienste zerlegt, die unabhängig voneinander bereitgestellt werden können. Eine Microservices-Architektur ist jedoch auch mit gewissen Herausforderungen verbunden. Die hier gezeigten Entwurfsmuster können zur Bewältigung dieser Herausforderungen beitragen.

![Entwurfsmuster für Microservices](../images/microservices-patterns.png)

[**Ambassador**](../../patterns/ambassador.md) kann verwendet werden, um allgemeine Clientkonnektivitätsaufgaben – etwa Überwachung, Protokollierung, Weiterleitung und Sicherheit (beispielsweise TLS) – sprachunabhängig auszulagern. Ambassador-Dienste werden häufig als Sidecar bereitgestellt (wie weiter unten erläutert).

[**Antibeschädigungsebene**](../../patterns/anti-corruption-layer.md) implementiert eine Fassade zwischen neuen und älteren Anwendungen, um zu gewährleisten, dass das Design einer neuen Anwendung nicht durch Abhängigkeiten von Legacysystemen eingeschränkt wird.

[**Back-Ends für Front-Ends**](../../patterns/backends-for-frontends.md) erstellt separate Back-End-Dienste für verschiedene Arten von Clients (etwa für Desktopclients und mobile Clients). Dadurch müssen die gegensätzlichen Anforderungen verschiedener Clienttypen nicht von einem einzelnen Back-End-Dienst verarbeitet werden. Dieses Muster ermöglicht die Trennung clientspezifischer Aspekte und trägt so zur Vereinfachung der einzelnen Microservices bei.

[**Bulkhead**](../../patterns/bulkhead.md) isoliert kritische Ressourcen wie Verbindungspool, Arbeitsspeicher und CPU für die einzelnen Workloads oder Dienste. Bei Verwendung von Bulkheads kann eine einzelne Workload (oder ein einzelner Dienst) nicht sämtliche Ressourcen für sich beanspruchen und sie anderen Workloads oder Diensten entziehen. Dieses Muster verhindert kaskadierende Fehler durch einen einzelnen Dienst und erhöht so die Resilienz des Systems.

[**Gatewayaggregation**](../../patterns/gateway-aggregation.md) fasst Anforderungen für mehrere einzelne Microservices in einer einzelnen Anforderung zusammen und verringert so die Kommunikation zwischen Consumern und Diensten.

[**Gatewayabladung**](../../patterns/gateway-offloading.md) ermöglicht es den einzelnen Microservices, gemeinsam genutzte Dienstfunktionen (etwa die Verwendung von SSL-Zertifikaten) an ein API-Gateway auszulagern.

[**Gatewayrouting**](../../patterns/gateway-routing.md) leitet Anforderungen für mehrere Microservices unter Verwendung eines einzelnen Endpunkts weiter, sodass Consumer nicht mehrere separate Endpunkte verwalten müssen.

[**Sidecar**](../../patterns/sidecar.md) stellt Hilfskomponenten einer Anwendung zwecks Isolation und Kapselung als separaten Container oder Prozess bereit.

[**Einschnürung**](../../patterns/strangler.md) unterstützt die schrittweise Umgestaltung einer Anwendung, indem nach und nach bestimmte Funktionsbestandteile durch neue Dienste ersetzt werden.

Den vollständigen Katalog mit Cloudentwurfsmustern im Azure Architecture Center finden Sie unter [Cloudentwurfsmuster](../../patterns/index.md).
