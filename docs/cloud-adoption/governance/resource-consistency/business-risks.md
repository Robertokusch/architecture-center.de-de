---
title: 'Framework für die Cloudeinführung (CAF): Ressourcenkonsistenz: Motivationen und Geschäftsrisiken'
titleSuffix: Microsoft Cloud Adoption Framework for Azure
ms.service: architecture-center
ms.subservice: enterprise-cloud-adoption
ms.custom: governance
ms.date: 02/11/2019
description: 'Ressourcenkonsistenz: Motivationen und Geschäftsrisiken'
author: alexbuckgit
ms.openlocfilehash: 19e0d761e4afa3473099bde2edc960c8b9eadb79
ms.sourcegitcommit: c053e6edb429299a0ad9b327888d596c48859d4a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/20/2019
ms.locfileid: "58242461"
---
# <a name="resource-consistency-motivations-and-business-risks"></a>Ressourcenkonsistenz: Motivationen und Geschäftsrisiken

In diesem Artikel werden die Gründe beschrieben, warum Kunden typischerweise eine Disziplin „Ressourcenkonsistenz“ in ihre Cloud Governance-Strategie integrieren. Darüber hinaus werden einige Beispiele für mögliche Geschäftsrisiken aufgeführt, die zu Richtlinienanweisungen führen können.

<!-- markdownlint-disable MD026 -->

## <a name="is-resource-consistency-relevant"></a>Ist Ressourcenkonsistenz relevant?

Wenn es um die Bereitstellung von Ressourcen und Workloads geht, bietet die Cloud eine höhere Agilität und Flexibilität als die meisten herkömmlichen lokalen Rechenzentren. Allerdings sind diese potenziellen, cloudbasierten Vorteile auch an mögliche Verwaltungsnachteile gekoppelt, die den Erfolg Ihrer Cloudeinführung ernsthaft gefährden können. Welche Ressourcen haben Sie bereitgestellt? Welche Teams besitzen welche Ressourcen? Verfügen Sie über genügend Ressourcen, die eine Workload unterstützen? Woher wissen Sie, ob Workloads fehlerfrei sind?

Ressourcenkonsistenz ist entscheidend, um sicherzustellen, dass Ressourcen konsistent und reproduzierbar bereitgestellt, aktualisiert und konfiguriert werden, und dass Dienstunterbrechungen minimiert und in möglichst kurzer Zeit behoben werden.

Die Disziplin „Ressourcenkonsistenz“ beschäftigt sich mit der Identifizierung und Korrektur von geschäftlichen Risiken im Zusammenhang mit den operationalen Aspekten Ihrer Cloudbereitstellung. Die Ressourcenkonsistenz umfasst die Überwachung der Anwendungs-, Workload- und Ressourcenleistung. Darüber hinaus umfasst sie die Aufgaben, die zur Erfüllung von Skalierungsanforderungen erforderlich sind, sowie zur Korrektur von Verstößen gegen die Vereinbarung zum Leistungsservicelevel (Service Level Agreement, SLA) und zur proaktiven Vermeidung von SLA-Leistungsverstößen durch automatisierte Wartung.

Anfängliche Testbereitstellungen erfordern möglicherweise nicht viel, das über die Einführung einiger oberflächlicher Benennungs- und Markierungsstandards hinausgeht, um Ihre Anforderungen an die Ressourcenkonsistenz zu unterstützen. Mit zunehmender Reife Ihrer Cloudeinführung und der Bereitstellung komplizierterer und stärker unternehmenskritischer Ressourcen steigt die Notwendigkeit zur Investition in die Disziplin „Ressourcenkonsistenz“ schnell an.

## <a name="business-risk"></a>Geschäftsrisiken

Die Disziplin „Ressourcenkonsistenz“ befasst sich mit den operationalen Kerngeschäftsrisiken. Arbeiten Sie mit Ihren Geschäfts- und IT-Teams zusammen, um diese Risiken zu identifizieren und auf Relevanz zu überwachen, um sie bei der Planung und Implementierung Ihrer Cloudbereitstellungen zu berücksichtigen.

Die Risiken werden je nach Organisation unterschiedlich sein, aber die folgenden allgemeinen Risiken können als Ausgangspunkt für Diskussionen innerhalb Ihres Cloud Governance-Teams verwendet werden:

- **Unnötige Betriebskosten**. Veraltete oder nicht verwendete Ressourcen oder Ressourcen, die in Zeiten geringen Bedarfs überdimensioniert sind, verursachen unnötige zusätzliche Betriebskosten.
- **Unterdimensionierte Ressourcen**. Ressourcen, nach denen eine höhere Nachfrage herrscht als erwartet, können zu Unterbrechungen des Geschäftsbetriebs führen, weil Cloudressourcen von der Nachfrage überfordert werden.
- **Verwaltungsineffizienzen**. Das Fehlen konsistenter Benennungs- und Markierungsmetadaten, die Ressourcen zugeordnet sind, können dazu führen, dass IT-Mitarbeiter Schwierigkeiten haben, Ressourcen für Verwaltungsaufgaben zu finden, oder den Besitz und Abrechnungsinformationen im Zusammenhang mit den Ressourcen zu identifizieren. Dies führt zu Ineffizienzen bei der Verwaltung, die Kosten erhöhen und die IT-Reaktionsfähigkeit bei Dienstunterbrechungen oder anderen Betriebsproblemen verlangsamen können.
- **Unterbrechung des Geschäftsbetriebs**. Dienstausfälle, die Verstöße gegen die vereinbarten Vereinbarungen zum Servicelevel (SLA) Ihrer Organisation nach sich ziehen, können zu Geschäftsverlusten oder anderen finanziellen Auswirkungen auf Ihr Unternehmen führen.

## <a name="next-steps"></a>Nächste Schritte

Dokumentieren Sie mit der [Cloudverwaltungsvorlage](./template.md) die Geschäftsrisiken, die wahrscheinlich durch den aktuellen Cloudeinführungsplan entstehen.

Sobald ein Verständnis für realistische Geschäftsrisiken hergestellt ist, besteht der nächste Schritt darin, die Risikotoleranz des Unternehmens zu dokumentieren sowie die Indikatoren und Schlüsselmetrik zur Überwachung dieser Toleranz zu erfassen.

> [!div class="nextstepaction"]
> [Grundlegendes zu Indikatoren, Metriken und Risikotoleranz](./metrics-tolerance.md)
