---
title: 'CAF: Sicherheitsbaseline: Motivationen und Geschäftsrisiken'
titleSuffix: Microsoft Cloud Adoption Framework for Azure
ms.service: architecture-center
ms.subservice: enterprise-cloud-adoption
ms.custom: governance
ms.date: 02/11/2019
description: 'Sicherheitsbaseline: Motivationen und Geschäftsrisiken'
author: BrianBlanchard
ms.openlocfilehash: 8407ed358e5862e466176096ee6a82ad792027cb
ms.sourcegitcommit: c053e6edb429299a0ad9b327888d596c48859d4a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/20/2019
ms.locfileid: "58247311"
---
# <a name="security-baseline-motivations-and-business-risks"></a>Sicherheitsbaseline: Motivationen und Geschäftsrisiken

In diesem Artikel werden die Gründe beschrieben, warum Kunden typischerweise eine Disziplin „Sicherheitsbaseline“ in ihre Cloud Governance-Strategie integrieren. Darüber hinaus werden einige Beispiele für mögliche Geschäftsrisiken aufgeführt, die zu Richtlinienanweisungen führen können.

<!-- markdownlint-disable MD026 -->

## <a name="is-a-security-baseline-relevant"></a>Ist eine Sichertheitsbaseline relevant?

Sicherheit ist ein Hauptanliegen jeder IT-Organisation. Cloudbereitstellungen weisen viele der gleichen Sicherheitsrisiken auf wie Workloads, die in herkömmlichen lokalen Rechenzentren gehostet werden. Aufgrund der Art der öffentlichen Cloudplattformen, bei denen kein direktes Eigentum an der physischen Hardware zum Speichern und Ausführen Ihrer Workloads besteht, erfordert die Cloudsicherheit eigene Richtlinien und Prozesse.

Eines der wichtigsten Merkmale, durch die sich Cloud-Sicherheitsgovernance von herkömmlichen Sicherheitsrichtlinien unterscheidet, ist die einfache Erstellung von Ressourcen, die möglicherweise zu Sicherheitsrisiken führen, wenn die Sicherheit vor der Bereitstellung nicht berücksichtigt wird. Durch die Flexibilität, die Technologien wie [Software Defined Networking (SDN)](../../decision-guides/software-defined-network/overview.md) für eine schnelle Änderung Ihrer cloudbasierten Netzwerktopologie bieten, kann sich auch leicht die gesamte Angriffsfläche Ihres Netzwerks auf unvorhergesehene Weise ändern. Cloudplattformen bieten auch Tools und Funktionen, die Ihre Sicherheitsfunktionen auf solche Weise verbessern können, wie es in lokalen Umgebungen nicht immer möglich ist.

Wie viel Sie in Sicherheitsrichtlinien und -prozesse investieren, hängt größtenteils von der Art Ihrer Cloudbereitstellung ab. Anfängliche Testbereitstellungen benötigen möglicherweise nur die grundlegendsten Sicherheitsrichtlinien, während eine unternehmenskritische Workload komplexere und umfangreichere Sicherheitsanforderungen erfordert. Alle Bereitstellungen müssen auf einer bestimmten Ebene in die Disziplin eingebunden sein.

Die Disziplin „Sicherheitsbaseline“ umfasst die Unternehmensrichtlinien und manuellen Prozesse, die Sie einführen können, um Ihre Cloudbereitstellung vor Sicherheitsrisiken zu schützen.

> [!NOTE]
>Obwohl es wichtig ist, die [Identitätsbaseline](../identity-baseline/overview.md) im Kontext der Sicherheitsbaseline zu verstehen und deren Beziehung zur Zugriffssteuerung zu kennen, ist unter den [fünf Disziplinen von Cloud Governance](../overview.md) die [Identitätsbaseline](../identity-baseline/overview.md) als eine eigene, von der Sicherheitsbaseline getrennte Disziplin aufgeführt.

## <a name="business-risk"></a>Geschäftsrisiken

Die Disziplin „Sicherheitsbaseline“ befasst sich mit den wichtigsten sicherheitsbezogenen Geschäftsrisiken. Arbeiten Sie mit Ihrem Unternehmen zusammen, um diese Risiken zu identifizieren und auf Relevanz zu überwachen, um sie bei der Planung und Implementierung Ihrer Cloudbereitstellungen zu berücksichtigen.

Die Risiken werden je nach Organisation unterschiedlich sein, aber die folgenden allgemeinen sicherheitsbezogenen Risiken können als Ausgangspunkt für Diskussionen innerhalb Ihres Cloud Governance-Teams verwendet werden:

- **Datenpanne**. Unbeabsichtigte Offenlegung oder Verlust sensibler, in der Cloud gehosteter Daten kann zum Verlust von Kunden, vertraglichen Problemen oder rechtlichen Konsequenzen führen.
- **Dienstunterbrechung**. Ausfälle und andere Leistungsprobleme aufgrund unsicherer Infrastrukturen führen zu Unterbrechungen des normalen Betriebs und können Produktivitäts- oder Umsatzeinbußen zur Folge haben.

## <a name="next-steps"></a>Nächste Schritte

Dokumentieren Sie mit der [Cloudverwaltungsvorlage](./template.md) die Geschäftsrisiken, die wahrscheinlich durch den aktuellen Cloudeinführungsplan entstehen.

Sobald ein Verständnis für realistische Geschäftsrisiken hergestellt ist, besteht der nächste Schritt darin, die Risikotoleranz des Unternehmens zu dokumentieren sowie die Indikatoren und Schlüsselmetrik zur Überwachung dieser Toleranz zu erfassen.

> [!div class="nextstepaction"]
> [Grundlegendes zu Indikatoren, Metriken und Risikotoleranz](./metrics-tolerance.md)
