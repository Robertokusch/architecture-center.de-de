---
title: 'Framework für die Cloudeinführung (CAF): Beispiele für Cost Management-Richtlinienanweisungen'
titleSuffix: Microsoft Cloud Adoption Framework for Azure
ms.service: architecture-center
ms.subservice: enterprise-cloud-adoption
ms.custom: governance
ms.date: 01/04/2019
description: Beispiele für Cost Management-Richtlinienanweisungen
author: BrianBlanchard
ms.openlocfilehash: 0a81ab8472a97c57f449f2ab2903a69fd35cb45a
ms.sourcegitcommit: c053e6edb429299a0ad9b327888d596c48859d4a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/20/2019
ms.locfileid: "58243281"
---
# <a name="cost-management-sample-policy-statements"></a>Beispiele für Cost Management-Richtlinienanweisungen

Einzelne Cloudrichtlinienanweisungen sind Anleitungen für den Umgang mit bestimmten Risiken, die während Ihres Risikobewertungsprozesses identifiziert wurden. Diese Anweisungen sollten eine präzise Zusammenfassung der Risiken sowie der Pläne für den Umgang mit diesen bereitstellen. Jede Anweisungsdefinition sollte diese Informationen enthalten:

- Geschäftsrisiko: eine Zusammenfassung des Risikos, das diese Richtlinie behandelt.
- Richtlinienanweisung: eine klare, zusammenfassende Erläuterung der Anforderungen der Richtlinie.
- Entwurfsoptionen: direkt umsetzbare Empfehlungen, Spezifikationen oder andere Anleitungen, die IT-Teams und Entwickler bei der Implementierung der Richtlinie verwenden können.

Die folgenden Beispielrichtlinienanweisungen behandeln eine Reihe von allgemeinen kostenbezogenen Geschäftsrisiken und sollen Ihnen als Beispiele dienen, auf die Sie sich beim Entwerfen von eigentlichen Richtlinienanweisungen beziehen können, um die Anforderungen Ihrer eigenen Organisation zu erfüllen. Diese Beispiele sind nicht restriktiv gemeint, und es gibt immer potenziell mehrere Richtlinienoptionen für den Umgang mit einem einzelnen identifizierten Risiko. Arbeiten Sie eng mit den Geschäfts- und IT-Teams zusammen, um die besten Richtlinienlösungen für Ihre besonderen kostenbezogene Risiken zu identifizieren.  

## <a name="future-proofing"></a>Zukunftsfähigkeit

**Geschäftsrisiko**. Aktuelle Kriterien, die eine Investition in eine Cost Management-Disziplin vonseiten des Governanceteams nicht rechtfertigen. Sie erwarten jedoch eine solche Investition in der Zukunft.

**Richtlinienanweisung**. Sie müssen alle in der Cloud bereitgestellten Ressourcen einer Abrechnungseinheit und einer Anwendung/Workload zuordnen und die Benennungsstandards erfüllen. Diese Richtlinie stellt sicher, dass künftige Cost Management-Bemühungen effektiv verlaufen.

**Entwurfsoptionen**. Informationen zum Einrichten einer zukunftssicheren Grundlage finden Sie in der Diskussionen zum Erstellen einer Governance-MVP in den [nützlichen Governancevorgehensweisen](../journeys/overview.md), die im CAF-Leitfaden enthalten sind.

## <a name="budget-overruns"></a>Kostenüberschreitung

**Geschäftsrisiko:** Die Self-Service-Bereitstellung ruft das Risiko einer Budgetüberschreitung hervor.

**Richtlinienanweisung:** Jede Cloudbereitstellung muss einer Abrechnungseinheit mit genehmigtem Budget und einem Mechanismus für Budgetlimits zugeordnet werden.

**Entwurfsoptionen:** In Azure kann das Budget mit [Azure Cost Management](/azure/cost-management/manage-budgets) gesteuert werden.

## <a name="underutilization"></a>Unterauslastung

**Geschäftsrisiko:** Das Unternehmen hat eine Vorauszahlung für Clouddienste geleistet oder ist eine Verpflichtung zur jährlichen Ausgabe eines bestimmten Betrags eingegangen. Es besteht das Risiko, dass der vereinbarte Betrag nicht genutzt wird, was zu einem Investitionsverlust führt.

**Richtlinienanweisung:** Jede Abrechnungseinheit mit einem zugeordneten Cloudbudget trifft sich jährlich, um Budgets festzulegen, vierteljährlich, um Budgets anzupassen, und monatlich, um Zeit für die Überprüfung der geplanten Ausgaben im Vergleich zu den tatsächlichen Ausgaben zu reservieren. Besprechen Sie alle Abweichungen, die größer als 20 % sind, monatlich mit der Leiter der Abrechnungseinheit. Weisen Sie zu Nachverfolgungszwecken alle Ressourcen einer Abrechnungseinheit zu.

**Entwurfsoptionen:**

- In Azure kann der Vergleich zwischen geplanten Ausgaben und tatsächlichen Ausgaben über [Azure Cost Management](/azure/cost-management/quick-acm-cost-analysis) verwaltet werden.
- Es gibt mehrere Optionen für die Gruppierung von Ressourcen nach Abrechnungseinheit. In Azure muss ein [Ressourcenkonsistenzmodell](../../decision-guides/resource-consistency/overview.md) in Verbindung mit dem Governanceteam ausgewählt und auf alle Ressourcen angewendet werden.

## <a name="overprovisioned-assets"></a>Überdimensionierte Ressourcen

**Geschäftsrisiko:** In herkömmlichen lokalen Rechenzentren ist es üblich, Ressourcen mit zusätzlicher Kapazität bereitzustellen, um ein Wachstum in ferner Zukunft einzuplanen. Die Cloud kann schneller als herkömmliche Ausstattung skaliert werden. Die Preise von Ressourcen in der Cloud beruhen zudem auf ihrer technischen Kapazität. Es besteht das Risiko, dass Cloudausgaben gemäß der alten lokalen Praxis künstlich aufgebläht werden.

**Richtlinienanweisung:** Alle in der Cloud bereitgestellten Ressourcen müssen in einem Programm registriert werden, das die Nutzung überwachen und zusätzliche Kapazitäten mit mehr als 50 % Auslastung melden kann. Eine in der Cloud bereitgestellte Ressource muss auf logische Weise gruppiert oder markiert werden, damit Mitglieder des Governanceteams den Workloadbesitzer in Bezug auf eine Optimierung überdimensionierter Ressourcen einbeziehen können.

**Entwurfsoptionen:**

- In Azure kann [Azure Advisor](/azure/advisor/advisor-cost-recommendations) Optimierungsempfehlungen bereitstellen.
- Es gibt mehrere Optionen für die Gruppierung von Ressourcen nach Abrechnungseinheit. In Azure muss ein [Ressourcenkonsistenzmodell](../../decision-guides/resource-consistency/overview.md) in Verbindung mit dem Governanceteam ausgewählt und auf alle Ressourcen angewendet werden.

## <a name="overoptimization"></a>Übermäßige optimierung

**Geschäftsrisiko:** Eine effektive Kostenverwaltung kann tatsächlich zu neuen Risiken führen. Die Optimierung von Ausgaben verhält sich umgekehrt zur Systemleistung. Bei der Kostensenkung besteht das Risiko, dass die Ausgaben zu eng begrenzt werden und sich dadurch das Benutzererlebnis verschlechtert.

**Richtlinienanweisung:** Jede Ressource, die sich direkt auf die Benutzerfreundlichkeit auswirkt, muss durch Gruppieren oder Markieren identifiziert werden. Vor der Optimierung einer Ressource, die die Benutzerfreundlichkeit betrifft, muss das Cloudgovernanceteam die Optimierung basierend auf Nutzungstrends anpassen, die nicht weniger als 90 Tage umfassen dürfen. Dokumentieren Sie alle saisonalen oder ereignisbedingten Bursts bei der Optimierung von Ressourcen.

**Entwurfsoptionen:**

- In Azure können die [Insights-Features von Azure Monitor](/azure/azure-monitor/insights/vminsights-performance) Sie bei der Analyse der Systemauslastung unterstützen.
- Es gibt mehrere Optionen für die Gruppierung und Markierung von Ressourcen nach Rollen. In Azure sollten Sie ein [Ressourcenkonsistenzmodell](../../decision-guides/resource-consistency/overview.md) in Verbindung mit dem Governanceteam auswählen und auf alle Ressourcen anwenden.

## <a name="next-steps"></a>Nächste Schritte

Verwenden Sie die in diesem Artikel erwähnten Beispiele als Ausgangspunkt für die Entwicklung von Richtlinien, die bestimmte Geschäftsrisiken behandeln, die Ihren Plänen für die Einführung der Cloud entsprechen.

Laden Sie die Vorlage [Cost Management](template.md) herunter, um mit der Entwicklung eigener, benutzerdefinierter Richtlinienanweisungen im Zusammenhang mit Cost Management zu beginnen.

Um die Einführung dieser Disziplin zu beschleunigen, wählen Sie die [nützliche Governance-Vorgehensweise](../journeys/overview.md) aus, die am besten zu Ihrer Umgebung passt. Ändern Sie dann den Entwurf, um Ihre speziellen Entscheidungen für Unternehmensrichtlinien zu integrieren.

> [!div class="nextstepaction"]
> [Nützliche Governancevorgehensweisen](../journeys/overview.md)
