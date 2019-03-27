---
title: 'CAF: Governancelösung für kleine bis mittlere Unternehmen'
titleSuffix: Microsoft Cloud Adoption Framework for Azure
ms.service: architecture-center
ms.subservice: enterprise-cloud-adoption
ms.custom: governance
ms.date: 02/11/2019
description: Governancelösung für kleine bis mittlere Unternehmen
author: BrianBlanchard
ms.openlocfilehash: a3e078845038a12977e7be5affbf22708411069f
ms.sourcegitcommit: c053e6edb429299a0ad9b327888d596c48859d4a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/20/2019
ms.locfileid: "58245161"
---
# <a name="small-to-medium-enterprise-governance-journey"></a>Governancelösung für kleine bis mittlere Unternehmen

## <a name="best-practice-overview"></a>Übersicht über bewährte Methoden

Diese Governance Journey folgt den Erfahrungen eines fiktiven Unternehmens in verschiedenen Phasen der Governancereife. Sie basiert auf echten Kundenlösungen. Die empfohlenen bewährten Methoden basieren auf den Einschränkungen und Anforderungen des fiktiven Unternehmens.

Als Schnellstartpunkt definiert diese Übersicht ein auf bewährten Methoden basierendes Minimum Viable Product (MVP) für die Governance. Sie enthält außerdem Links zu einigen Governanceentwicklungen, die weitere bewährte Methoden hinzufügen, wenn neue Geschäfts- oder technische Risiken entstehen.

> [!WARNING]
> Dieses MVP ist ein Baselineausgangspunkt, der auf einer Reihe von Annahmen basiert. Auch dieser minimale Satz bewährter Methoden basiert auf Unternehmensrichtlinien, die von beispiellosen geschäftlichen Risiken und Risikotoleranzen bestimmt werden. Um festzustellen, ob diese Annahmen auf Sie zutreffen, lesen Sie die [längere Schilderung](./narrative.md), die diesem Artikel folgt.

## <a name="governance-best-practice"></a>Bewährte Governancemethode

Diese bewährte Methode dient als Grundlage, auf der eine Organisation schnell und konsistent Governancesicherungen für mehrere Azure-Abonnements hinzufügen kann.

### <a name="resource-organization"></a>Ressourcenorganisation

Die folgende Abbildung zeigt die Governance-MVP-Hierarchie zum Organisieren von Ressourcen.

![Ressourcenorganisationsdiagramm](../../../_images/governance/resource-organization.png)

Jede Anwendung sollte im richtigen Bereich der Verwaltungsgruppen-, Abonnement- und Ressourcengruppenhierarchie bereitgestellt werden. Während der Bereitstellungsplanung erstellt das Cloud-Governance-Team die erforderlichen Knoten in der Hierarchie, um die Cloudeinführungsteams zu befähigen.  

1. Eine Verwaltungsgruppe für jeden Typ von Umgebung (z.B. Produktion, Entwicklung und Tests).
2. Ein Abonnement für jede „Anwendungskategorisierung“.
3. Eine separate Ressourcengruppe für jede Anwendung.
4. Konsistente Benennung sollte auf jeder Ebene dieser Gruppierungshierarchie angewendet werden.

Dies ist ein Beispiel für die Anwendung dieses Musters:

![Ressourcenorganisationsbeispiel für ein mittelgroßes Unternehmen](../../../_images/governance/mid-market-resource-organization.png)

Diese Muster bieten Raum für Wachstum, ohne die Hierarchie unnötig zu verkomplizieren.

[!INCLUDE [governance-of-resources](../../../../../includes/cloud-adoption/governance/governance-of-resources.md)]

## <a name="governance-evolutions"></a>Governanceentwicklungen

Sobald dieses MVP bereitgestellt ist, können zusätzliche Ebenen der Governance schnell in die Umgebung integriert werden. Einige Möglichkeiten, das MVP zu entwickeln, sind:

- [Sicherheitsbaseline für geschützte Daten](./security-baseline-evolution.md)
- [Ressourcenkonfigurationen für unternehmenskritische Anwendungen](./resource-consistency-evolution.md)
- [Steuerelemente für das Kostenmanagement](./cost-management-evolution.md)
- [Steuerelemente für die Multi-Cloud-Entwicklung](./multi-cloud-evolution.md)

<!-- markdownlint-disable MD026 -->

## <a name="what-does-this-best-practice-do"></a>Wozu dient diese bewährte Methode?

Im MVP sind Methoden und Tools für die Disziplin der [Beschleunigung der Bereitstellung](../../deployment-acceleration/overview.md) festgelegt, um schnell Unternehmensrichtlinien anzuwenden. Insbesondere verwendet das MVP Azure Blueprints, Azure Policy und Azure-Verwaltungsgruppen, um einige grundlegende Unternehmensrichtlinien anzuwenden, wie in der Lösung für das fiktive Unternehmen definiert. Diese Unternehmensrichtlinien werden mithilfe von Resource Manager-Vorlagen und Azure-Richtlinien angewandt, um eine sehr kleine Baseline für Identität und Sicherheit festzulegen.

![Beispiel für ein inkrementelles Governance-MVP](../../../_images/governance/governance-mvp.png)

## <a name="evolving-the-best-practice"></a>Weiterentwicklung der bewährten Methode

Im Lauf der Zeit wird dieses Governance-MVP verwendet, um die Governancemethoden weiterzuentwickeln. Mit fortschreitender Einführung wächst das geschäftliche Risiko. Verschiedene Disziplinen im CAF-Governancemodell werden zur Verringerung dieser Risiken weiterentwickelt. Spätere Artikel dieser Reihe erläutern die Weiterentwicklung der Unternehmensrichtlinie, die sich auf das fiktive Unternehmen auswirkt. Diese Weiterentwicklungen erfolgen in drei Disziplinen:

- Kostenverwaltung, indem die Einführung skaliert wird.
- Sicherheitsbaseline, indem geschützte Daten bereitgestellt werden.
- Ressourcenkonsistenz, indem die IT-Abteilung beginnt, unternehmenskritische Workloads zu unterstützen.

![Beispiel für ein inkrementelles Governance-MVP](../../../_images/governance/governance-evolution.png)

## <a name="next-steps"></a>Nächste Schritte

Da Sie jetzt mit dem Governance-MVP vertraut sind und eine Vorstellung von zukünftigen Weiterentwicklungen der Governance haben, lesen Sie die unterstützende Lösung, um zusätzliche Kontextinformationen zu erhalten.

> [!div class="nextstepaction"]
> [Lesen Sie die unterstützende Lösung](./narrative.md)
