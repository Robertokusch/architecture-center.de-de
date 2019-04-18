---
title: Microservices-Referenzimplementierung für Azure Kubernetes Service
description: Diese Referenzimplementierung veranschaulicht bewährte Methoden für eine Microservices-Architektur.
author: MikeWasson
ms.date: 02/26/2019
ms.topic: guide
ms.service: architecture-center
ms.subservice: reference-architecture
ms.custom: microservices
ms.openlocfilehash: 17e275e5b5f45233f7467192402cb28fce35c57b
ms.sourcegitcommit: 579c39ff4b776704ead17a006bf24cd4cdc65edd
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/17/2019
ms.locfileid: "59068904"
---
# <a name="designing-a-microservices-architecture"></a>Entwerfen einer Microservices-Architektur

Microservices haben sich zu einem beliebten Architekturstil für die Erstellung robuster, hochgradig skalierbarer, unabhängig bereitstellbarer Cloudanwendungen entwickelt, die sich schnell weiterentwickeln lassen. Damit Microservices nicht nur ein Schlagwort bleiben, erfordern sie allerdings eine andere Herangehensweise an die Gestaltung und Erstellung von Anwendungen.

Diese Artikelreihe beschäftigt sich mit der Erstellung und dem Betrieb einer Microservices-Architektur in Azure. Dabei werden folgende Themen behandelt:

- [Kommunikation zwischen Diensten](./interservice-communication.md)
- [API-Design](./api-design.md)
- [API-Gateways](./gateway.md)
- [Überlegungen zu Daten](./data-considerations.md)
- [Entwurfsmuster](./patterns.md)

## <a name="prerequisites"></a>Voraussetzungen

Machen Sie sich vor der Lektüre dieser Artikel ggf. zunächst mit Folgendem vertraut:

- [Einführung in Microservices-Architekturen](../introduction.md): Informieren Sie sich über die Vorteile und Herausforderungen im Zusammenhang mit Microservices sowie darüber, wann diese Art von Architektur verwendet werden sollte.
- [Verwenden der Domänenanalyse zur Modellierung von Microservices](../model/domain-analysis.md): Hier wird ein domänenbasierter Ansatz für die Modellierung von Microservices vermittelt.

## <a name="reference-implementation"></a>Referenzimplementierung

Zur Veranschaulichung bewährter Methoden für eine Microservices-Architektur haben wir als Referenzimplementierung eine Drohnenlieferungsanwendung erstellt. Diese Implementierung wird unter Verwendung von Azure Kubernetes Service (AKS) in Kubernetes ausgeführt. Die Referenzimplementierung finden Sie auf [GitHub][drone-ri].

![Architektur der Drohnenlieferungsanwendung](../images/drone-delivery.png)

## <a name="scenario"></a>Szenario

Fabrikam, Inc. führt einen Drohnenlieferdienst ein. Das Unternehmen verfügt über eine Drohnenflotte. Unternehmen registrieren sich bei dem Dienst, und Benutzer können eine Drohne anfordern, die auszuliefernde Waren abholt. Wenn ein Kunde einen Abholtermin festlegt, weist ein Back-End-System eine Drohne zu und teilt dem Benutzer die voraussichtliche Lieferzeit mit. Während der Durchführung der Lieferung kann der Kunde die Position der Drohne nachverfolgen, und die voraussichtliche Ankunftszeit wird kontinuierlich aktualisiert.

Dieses Szenario beinhaltet eine recht komplizierte Domäne. Zu den Anliegen des Unternehmens zählen unter anderem die Zeitplanung für die Drohnen, die Nachverfolgung von Paketen, die Verwaltung von Benutzerkonten sowie die Speicherung und Analyse von Verlaufsdaten. Darüber hinaus ist Fabrikam an einer schnellen Marktreife interessiert und möchte dann zeitnah neue Features und Funktionen hinzufügen. Die Anwendung muss cloudfähig sein und über ein hohes Servicelevelziel (Service Level Objective, SLO) verfügen. Fabrikam erwartet außerdem erhebliche Unterschiede bei den Datenspeicher- und Abfrageanforderungen der verschiedenen Systemkomponenten. Aufgrund dieser Überlegungen hat sich Fabrikam bei seiner Drohnenlieferungsanwendung für eine Microservices-Architektur entschieden.

> [!NOTE]
> Hilfreiche Informationen zur Wahl einer Microservices-Architektur oder eines anderen Architekturstils finden Sie im [Azure-Anwendungsarchitekturleitfaden](../../guide/index.md).

Unsere Referenzimplementierung verwendet Kubernetes mit [Azure Kubernetes Service (AKS)](/azure/aks/). Viele der allgemeinen architekturbezogenen Entscheidungen und Herausforderungen gelten jedoch für alle Containerorchestratoren (einschließlich [Azure Service Fabric](/azure/service-fabric/)).

<!-- links -->

[drone-ri]: https://github.com/mspnp/microservices-reference-implementation/tree/v0.1.0-orig

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Choosing a compute option for microservices](./compute-options.md) (Auswählen einer Computeoption für Microservices)