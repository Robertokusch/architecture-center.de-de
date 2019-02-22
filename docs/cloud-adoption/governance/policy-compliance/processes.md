---
title: 'CAF: Überwachen und Erzwingen der Einhaltung von Richtlinien'
description: Wie stellen Sie sicher, dass eingerichtete Richtlinien eingehalten werden?
author: BrianBlanchard
ms.date: 1/4/2019
ms.openlocfilehash: 9066f33c8baa183476a9632e82d6eb960d03752c
ms.sourcegitcommit: 273e690c0cfabbc3822089c7d8bc743ef41d2b6e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/08/2019
ms.locfileid: "55900755"
---
<!-- markdownlint-disable MD026 -->

# <a name="what-processes-can-help-ensure-policy-adherence"></a>Mit welchen Verfahren kann die Einhaltung von Richtlinien besser sichergestellt werden?

<!---
I've defined policies, I've provided an architecture guide. Now how do I monitor adherence to policy? If there is a violation, how do I enforce the policy?
--->

Nachdem Sie Ihre Cloudrichtlinienanweisungen eingerichtet und ein erstes Entwurfshandbuch geschrieben haben, müssen Sie eine Strategie entwickeln, die sicherstellt, dass die Cloudbereitstellung konform zu Ihren Richtlinienanforderungen bleibt. Diese Strategie muss die fortlaufenden Überprüfungs- und Kommunikationsprozesse Ihres Cloudgovernanceteams einschließen, Kriterien für Handlungen bei Richtlinienverstößen festlegen und die Anforderungen für automatisierte Überwachungs- und Compliancesysteme definieren, die Verstöße erkennen und Aktionen zur Problembehebung auslösen.

Beispiele für die Einpassung eines Prozesses zur Einhaltung von Richtlinien in die Cloudgovernanceplanung finden Sie in den Abschnitten zu Unternehmensrichtlinien unter [Nützliche Governance-Vorgehensweisen](../journeys/overview.md).

## <a name="prioritize-policy-adherence-processes"></a>Priorisieren von Prozessen zur Einhaltung von Richtlinien

Wie viel Aufwand ist bei der Entwicklung von Prozessen erforderlich, um Ihre Richtlinienziele zu unterstützen? Abhängig von der Größe und Reife Ihrer Cloudbereitstellung können der erforderliche Aufwand zum Einrichten von Prozessen für die Einhaltung und die damit verbundenen Kosten stark variieren.

Für kleine Bereitstellungen, die das Entwickeln und Testen von Ressourcen umfassen, sind die Richtlinienanforderungen möglicherweise einfach und erfordern nur die Behandlung weniger dedizierter Ressourcen. Eine ausgereifte, unternehmenskritische Cloudbereitstellung mit hohen Anforderungen an Sicherheit und Leistung erfordert andererseits eventuell ein Team von Mitarbeitern, umfassende interne Prozesse und benutzerdefinierte Überwachungstools, um Ihre Richtlinienziele zu unterstützen.

Als ersten Schritt bei der Definition Ihrer Strategie zur Einhaltung von Richtlinien sollten Sie auswerten, wie die nachfolgend beschriebenen Prozesse Ihre Anforderungen unterstützen können. Ermitteln Sie, wie viel Aufwand sich als Investition in diese Prozesse lohnt, und stellen Sie dann anhand dieser Informationen realistische Budget- und Personalpläne auf, um diese Anforderungen zu erfüllen.

## <a name="establish-cloud-governance-team-processes"></a>Einrichten von Prozessen für das Cloudgovernanceteam

Vor dem Definieren von Auslösern für die Erzwingung der Einhaltung von Richtlinien müssen Sie die allgemeinen Prozesse einrichten, die Ihr Team verwenden soll. Außerdem müssen Sie festlegen, wie Informationen freigegeben und zwischen IT-Mitarbeiter und dem Cloudgovernanceteam eskaliert werden sollen.

### <a name="assign-cloud-governance-team-members"></a>Zuweisen von Mitgliedern zum Cloudgovernanceteam

Wer soll die laufende Unterstützung für die Einhaltung von Richtlinien übernehmen und Probleme im Zusammenhang mit Richtlinien behandeln, die beim Bereitstellen und im Betrieb Ihrer Cloudressourcen auftreten? Die Größe und Zusammensetzung Ihres Cloudgovernanceteams ist von der Komplexität Ihrer Richtlinienanforderungen sowie den Budget- und Personalprioritäten, die Sie für die Einhaltung von Richtlinien eingeräumt haben, abhängig.

Wählen Sie Teammitglieder aus, die über Fachwissen in den Bereichen Ihrer definierten Richtlinienanweisungen verfügen. Für erste Testbereitstellungen kann dies auf wenige Systemadministratoren beschränkt sein, die für die Einrichtung der Governancegrundlagen verantwortlich sind. Wenn die Entwicklungsstufe Ihrer Bereitstellungen und die Komplexität Ihrer Richtlinien sowie deren Integration in die größeren Richtlinienanforderungen Ihres Unternehmens zunehmen, muss Ihr Cloudgovernanceteam angepasst werden, um immer kompliziertere Richtlinienanforderungen zu unterstützen.

Mit zunehmender Reife Ihrer Governanceprozesse sollten Sie die Mitglieder des Cloudunterstützungsteams regelmäßig überprüfen, um sicherzustellen, dass die neuesten Richtlinienanforderungen weiterhin behandelt werden können. Finden Sie Mitgliedern Ihrer IT-Abteilung mit passenden Erfahrungen oder Interesse an bestimmten Governancebereichen, und fügen Sie sie Ihren Teams dauerhaft oder ad-hoc nach Bedarf hinzu.

### <a name="reviews-and-policy-iteration"></a>Iterationen von Überprüfungen und Richtlinien

Wenn zusätzliche Ressourcen bereitgestellt werden, muss das Cloudgovernanceteam sicherstellen, dass die neuen Workloads oder Ressourcen die Richtlinienanforderungen erfüllen. Planen Sie ein Treffen mit den für die Bereitstellung neuer Ressourcen verantwortlichen Teams, um die Ausrichtung an Ihren Entwurfsleitfäden zu besprechen.

Bewerten Sie mit zunehmender Größe Ihrer gesamten Bereitstellung regelmäßig neue potenzielle Risiken, und aktualisieren Sie die Richtlinienanweisungen und Entwurfsleitfäden nach Bedarf. Planen Sie regelmäßige Überprüfungszyklen in allen fünf Governancedisziplinen, um sicherzustellen, dass die Richtlinie aktuell ist und eingehalten wird.

### <a name="education"></a>Fortbildung

Zur Einhaltung der Richtlinien müssen die IT-Mitarbeiter und Entwickler die Richtlinienanforderungen, die ihre Zuständigkeitsbereiche betreffen, verstehen. Planen Sie Ressourcen für die Dokumentation von Entscheidungen und Anforderungen ein, und informieren Sie alle relevanten Teams über Entwurfshandbücher, die Ihre Richtlinienanforderungen unterstützen.

Wenn sich Richtlinien ändern, aktualisieren Sie regelmäßig die Dokumentation und Schulungsmaterialien, und stellen Sie sicher, dass in den Schulungen der relevanten IT-Mitarbeiter aktualisierte Anforderungen und Anleitungen angesprochen werden.  

### <a name="establish-escalation-paths"></a>Einrichten von Eskalationspfaden

Wer soll benachrichtigt werden, wenn eine Ressource nicht mehr konform ist? An wen sollen sich IT-Mitarbeiter wenden, wenn sie ein Konformitätsproblem entdecken? Stellen Sie sicher, dass der Eskalationsprozess für das Cloudgovernanceteam klar definiert ist. Stellen Sie sicher, dass diese Kommunikationskanäle immer aktuell bleiben und Änderungen bei den Mitarbeitern und in der Organisation berücksichtigen.

## <a name="violation-triggers-and-actions"></a>Auslöser und Aktionen bei Verstößen

Nachdem Sie Ihr Cloudgovernanceteam und dessen Prozesse definiert haben, müssen Sie explizit definieren, was als Einhaltungsverstoß angesehen wird und die Aktionen auslöst, und was diese Aktionen umfassen sollen.

### <a name="define-triggers"></a>Definieren von Auslösern

Überprüfen Sie für jede Ihrer Richtlinienanweisungen die Anforderungen, um festzustellen, was eine Richtlinienverletzung ausmacht. Generieren Sie Auslöser anhand der Informationen, die Sie bereits im Rahmen der Richtliniendefinition eingerichtet haben.

* Risikotoleranz – erstellen Sie Auslöser bei Verstößen basierend auf Metriken und Risikoindikatoren, die Sie im Rahmen Ihrer [Risikotoleranzanalyse](risk-tolerance.md) eingerichtet haben.
* Definierte Richtlinienanforderungen – Richtlinienanweisungen können Anforderungen an SLA (Service Level Agreement), Business Continuity und Disaster Recovery (BRCD) und Leistung enthalten, die als Grundlage für Konformitätsauslöser verwendet werden sollten.

### <a name="define-actions"></a>Definieren von Aktionen

Jeder Verletzungsauslöser sollte eine entsprechende Aktion aufweisen. Ausgelöste Aktionen sollten immer einen entsprechenden IT-Mitarbeiter oder ein Mitglied des Cloudgovernanceteams benachrichtigen, wenn ein Verstoß auftritt. Diese Benachrichtigung kann in Abhängigkeit vom Typ und Schweregrad der erkannten Verletzung zu einer manuellen Überprüfung des Konformitätsproblems führen oder einen zuvor festgelegten Problembehebungsprozess auslösen.

Einige Beispiele für Auslöser und Aktionen bei Verstößen:

| Cloudgovernancedisziplin | Beispielauslöser | Beispielaktion |
|-----------------------------|----------------|---------------|
| Cost Management | Monatliche Cloudausgaben sind um mehr als 20 % höher als erwartet. | Benachrichtigung des Leiters der Abrechnungsabteilung, der eine Überprüfung der Ressourcennutzung beginnt. |
| Sicherheitsbaseline | Erkennung einer verdächtigen Benutzeranmeldeaktivität. | Benachrichtigung des IT-Sicherheitsteams und Deaktivierung des verdächtigen Benutzerkontos. |
| Ressourcenkonsistenz | Die CPU-Auslastung für eine Workload übersteigt 90 %. | Benachrichtigung des IT-Betriebsteam und horizontales Hochskalieren mit zusätzlichen Ressourcen zum Verarbeiten der Last. |

## <a name="monitoring-and-compliance-automation"></a>Automatisierung der Überwachung und Einhaltung

Nachdem Sie Ihre Auslöser und Aktionen für Verstöße definiert haben, können Sie damit beginnen, optimale Protokollierungs- und Berichterstattungstools und andere Features der Cloudplattform zu planen, die bei der Automatisierung Ihrer Strategie für Überwachung und Einhaltung von Richtlinien helfen.

Hinweise zur Auswahl des besten Überwachungsmusters für Ihre Bereitstellung finden Sie im Thema [Leitfaden zur Entscheidungsfindung für Protokollierung und Berichterstellung](../../decision-guides/log-and-report/overview.md) des CAF.
