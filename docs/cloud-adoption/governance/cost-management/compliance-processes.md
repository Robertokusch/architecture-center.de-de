---
title: 'CAF: Kostenmanagement: Prozesse für Richtlinienkonformität'
titleSuffix: Microsoft Cloud Adoption Framework for Azure
ms.service: architecture-center
ms.subservice: enterprise-cloud-adoption
ms.custom: governance
ms.date: 02/11/2019
description: 'Kostenmanagement: Prozesse für Richtlinienkonformität'
author: BrianBlanchard
ms.openlocfilehash: 62547f4af8a873536499c35f58d323b03b828a74
ms.sourcegitcommit: c053e6edb429299a0ad9b327888d596c48859d4a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/20/2019
ms.locfileid: "58241174"
---
# <a name="cost-management-policy-compliance-processes"></a>Kostenmanagement: Prozesse für Richtlinienkonformität

In diesem Artikel wird eine Methode zum Erstellen von Prozessen erläutert, die eine Governancedisziplin für das [Kostenmanagement](./overview.md) unterstützen. Die wirksame Governance der Cloud beginnt mit sich wiederholenden manuellen Prozessen zur Unterstützung der Richtlinieneinhaltung. Dies erfordert die regelmäßige Einbeziehung des Cloudgovernanceteams sowie interessierter Geschäftsbeteiligter, um Richtlinien zu überprüfen und zu aktualisieren und die Einhaltung von Richtlinien sicherzustellen. Darüber hinaus können viele laufende Überwachungs- und Durchsetzungsprozesse automatisiert oder durch Werkzeuge ergänzt werden, um den Kontrolloverhead zu reduzieren und schneller auf Abweichungen von der Richtlinie reagieren zu können.

## <a name="planning-review-and-reporting-processes"></a>Planungs-, Prüfungs- und Berichterstellungsprozesse

Die besten Tools für das Kostenmanagement in der Cloud sind nur so gut wie die Prozesse und Richtlinien, durch die sie unterstützt werden. Nachstehend sind eine Reihe von Beispielprozessen ausgeführt, die häufig im Rahmen der Disziplin „Kostenmanagement“ verwendet werden. Beim Planen der Prozesse, die Ihnen die fortgesetzte Aktualisierung der Kostenrichtlinie aufgrund von geschäftlichen Änderungen und Feedback von den Geschäftsteams gemäß dem Kostengovernanceleitfaden erlauben, können Sie diese Beispiele als Ausgangspunkt verwenden.

**Anfängliche Risikobewertung und Planung**: Im Rahmen der ersten Einführung der Disziplin „Kostenmanagement“ identifizieren Sie Ihre Kerngeschäftsrisiken und Toleranzen im Zusammenhang mit den Cloudkosten. Verwenden Sie diese Informationen, um budget- und kostenbezogene Risiken mit Mitgliedern Ihrer Geschäftsteams zu besprechen und einen grundlegenden Satz von Richtlinien zur Minimierung dieser Risiken zu entwickeln, um Ihre erste Governancestrategie zu formulieren.

**Bereitstellungsplanung**: Legen Sie vor der Bereitstellung von Ressourcen ein prognostiziertes, auf erwarteter Cloudzuweisung basierendes Budget fest. Stellen Sie sicher, dass Besitzer- und Kostenabrechnungsinformationen für die Bereitstellung dokumentiert sind.  

**Jährliche Planung**: Führen Sie jährlich eine Rollupanalyse für alle bereitgestellten und bereitzustellenden Ressourcen durch. Richten Sie die Budgets an Geschäftseinheiten, Abteilungen, Teams und anderen entsprechenden Einheiten aus, um eine Einführung nach dem Self-Service-Prinzip zu ermöglichen. Stellen Sie sicher, dass der Leiter jeder Abrechnungseinheit das Budget kennt und Ausgaben nachverfolgen kann.

Dies ist der Zeitpunkt für eine Vorabverpflichtung oder einen Kauf im voraus, um Rabatte zu maximieren. Es ist ratsam, die jährliche Budgetierung mit dem Geschäftsjahr des Cloudanbieters abzustimmen, um weiteren Nutzen aus Jahresend-Rabattoptionen zu ziehen.

**Vierteljährliche Planung**: Überprüfen Sie Budgets einmal pro Quartal mit jedem Abrechnungseinheitenleiter, um Vorhersage und tatsächliche Ausgaben abzustimmen. Wenn Änderungen des Plans oder unerwartete Ausgabenmuster auftreten, stimmen Sie das Budget darauf ab und weisen Sie es neu zu.

Dieser vierteljährliche Planungsprozess ist auch ein guter Zeitpunkt, um die aktuelle Zusammensetzung Ihres Cloudgovernanceteams auf Wissenslücken im Zusammenhang mit aktuellen oder zukünftigen Geschäftsplänen zu überprüfen. Laden Sie relevante Mitarbeiter und Workloadbesitzer ein, an Überprüfungen und Planungen teilzunehmen, entweder als zeitlich begrenzte Berater oder als ständige Mitglieder Ihres Teams.

**Aus- und Weiterbildung**: Bieten Sie zweimonatlich Trainings an, um sicherzustellen, dass Geschäfts- und IT-Mitarbeiter auf dem neuesten Stand der Richtlinienanforderungen für das Kostenmanagement sind. Im Rahmen dieses Prozesses überprüfen und aktualisieren Sie alle Unterlagen, Anleitungen oder andere Trainingsmaterialien, um sicherzustellen, dass sie mit den neuesten Unternehmensrichtlinien übereinstimmen.

**Monatliche Berichterstellung**: Berichten Sie monatlich über das Verhältnis zwischen tatsächlichen Ausgaben und Vorhersage. Unterrichten Sie Abrechnungsleiter über unerwartete Abweichungen.

Diese grundlegenden Prozesse tragen dazu bei, Ausgaben abzustimmen und eine Grundlage für die Disziplin „Kostenmanagement“ zu bilden.

## <a name="ongoing-monitoring-processes"></a>Fortlaufende Überwachungsprozesse

Eine erfolgreiche Governancestrategie für das Kostenmanagement hängt vom Einblick in vergangene, aktuelle und geplante zukünftige cloudbezogene Ausgaben ab. Ohne die Möglichkeit, die relevanten Metriken und Daten Ihrer bestehenden Kosten zu analysieren, können Sie keine Veränderungen bei den Risiken und keine Verstöße gegen Ihre Risikotoleranzen erkennen. Für die oben genannten laufenden Governanceprozesse sind Qualitätsdaten erforderlich, um eine entsprechende Änderung der Richtlinie zu gewährleisten und Ihre Infrastruktur vor wechselnden Geschäftsanforderungen und wechselnder Cloudnutzung besser zu schützen.

Stellen Sie sicher, dass Ihre IT-Teams automatisierte Systeme für die Überwachung Ihrer Cloudausgaben und zur Verwendung für nicht geplante Abweichungen von erwarteten Kosten implementiert haben. Richten Sie Berichterstellungs- und Warnsysteme ein, um die schnelle Erkennung und Minderung potenzieller Verstöße gegen Richtlinien sicherzustellen.

## <a name="compliance-violation-triggers-and-enforcement-actions"></a>Auslöser bei Complianceverstößen und Durchsetzungsmaßnahmen

Wenn Verstöße festgestellt werden, sollten Sie Durchsetzungsmaßnahmen ergreifen, um eine erneute Einhaltung der Richtlinien zu erreichen. Sie können die meisten Auslöser bei Verstößen mit den unter [Kostenmanagement-Toolkette für Azure](toolchain.md) beschriebenen Tools automatisieren.

Hier einige Beispiele für Trigger:

* Monatliche Budgetabweichungen: Erörtern Sie Abweichungen bei den monatlichen Ausgaben, die das Verhältnis zwischen Vorhersage und tatsächlichen Ausgaben um 20% überschreiten, mit dem Abrechnungseinheitenleiter. Zeichnen Sie Auflösungen und Änderungen in der Prognose auf.
* Geschwindigkeit der Einführung: Jede Abweichung auf Abonnementebene, die 20% überschreitet, löst eine Prüfung mit dem Abrechnungseinheitenleiter aus. Zeichnen Sie Auflösungen und Änderungen in der Prognose auf.

## <a name="next-steps"></a>Nächste Schritte

Dokumentieren Sie mit der [Vorlage Cloudverwaltung](./template.md) die Prozesse und Auslöser, die sich an dem aktuellen Cloudeinführungsplan orientieren.

Anleitungen zur Ausführung von Cloudverwaltungsrichtlinien in Abstimmung mit Einführungsplänen finden Sie im Artikel über die Disziplin „Kostenmanagement“.

> [!div class="nextstepaction"]
> [Verbesserung der Disziplin „Kostenmanagement“](./discipline-improvement.md)
