---
title: 'CAF: Beschleunigung der Bereitstellung – Prozesse für die Richtlinienkonformität'
description: Beschleunigung der Bereitstellung – Prozesse für die Richtlinienkonformität
titleSuffix: Microsoft Cloud Adoption Framework for Azure
ms.service: architecture-center
ms.subservice: enterprise-cloud-adoption
ms.custom: governance
ms.date: 02/11/2019
author: alexbuckgit
ms.openlocfilehash: d469c3ba6fb53c5412c615886d449d9b59c2378e
ms.sourcegitcommit: c053e6edb429299a0ad9b327888d596c48859d4a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/20/2019
ms.locfileid: "58246621"
---
# <a name="deployment-acceleration-policy-compliance-processes"></a>Beschleunigung der Bereitstellung – Prozesse für die Richtlinienkonformität

In diesem Artikel wird ein Ansatz für die Prozesse zur Einhaltung der Richtlinien beschrieben, welche die [Beschleunigung der Bereitstellung](./overview.md) steuern. Eine wirkungsvolle Governance für die Cloudkonfiguration beginnt mit sich wiederholenden manuellen Prozessen, die zum Erkennen von Problemen und zum Festlegen der Richtlinien entwickelt wurden, die diese Risiken minimieren sollen. Sie können diese Prozesse jedoch auch automatisieren und mit Tools unterstützen, um den durch die Governance bedingten Mehraufwand zu reduzieren und eine schnellere Reaktion auf Abweichungen zu ermöglichen.

## <a name="planning-review-and-reporting-processes"></a>Planungs-, Prüfungs- und Berichtsprozesse

Die besten Tools für die Beschleunigung der Bereitstellung in der Cloud sind nur so gut wie die Prozesse und Richtlinien, durch die sie unterstützt werden. Nachstehend sind eine Reihe von Beispielprozessen aufgeführt, die häufig im Rahmen der Disziplin „Sicherheitsbaseline“ verwendet werden. Beim Planen der Prozesse, die Ihnen die fortlaufende Aktualisierung der Sicherheitsrichtlinie aufgrund von geschäftlichen Änderungen und Feedback von den Sicherheits- und IT-Teams (die dafür verantwortlich sind, den Governanceleitfaden in die Tat umzusetzen) ermöglichen, können Sie diese Beispiele als Ausgangspunkt verwenden.

**Erste Risikobewertung und Planung**: Identifizieren Sie im Rahmen der Ersteinführung der Disziplin „Beschleunigung der Bereitstellung“ die zentralen geschäftlichen Risiken und Toleranzen im Zusammenhang mit der Bereitstellung Ihrer Geschäftsanwendungen. Besprechen Sie anhand dieser Informationen mit Mitgliedern des IT Operations-Teams und des Sicherheitsteams die speziellen technischen Risiken, und entwickeln Sie allgemeine Bereitstellungs- und Konfigurationsrichtlinien, um diese Risiken zu minimieren und Ihre anfängliche Governancestrategie zu formulieren.

**Bereitstellungsplanung**: Führen Sie vor der Bereitstellung einer Ressource eine Sicherheitsüberprüfung aus, um mögliche neue Risiken zu identifizieren und sicherzustellen, dass alle Anforderungen von Zugriffs- und Datensicherheitsrichtlinien erfüllt werden.

**Bereitstellungstests**: Im Rahmen des Ressourcenbereitstellungsprozesses ist das Cloud Governance-Team zusammen mit den Teams für die Unternehmenssicherheit für die Überprüfung der Bereitstellung verantwortlich, um die Einhaltung der Sicherheitsrichtlinie zu gewährleisten.

**Jährliche Planung**: Führen Sie eine jährliche Überprüfung der Strategie für die Beschleunigung der Bereitstellung auf höchster Ebene aus. Ermitteln Sie zukünftige Prioritäten des Unternehmens und aktualisierte Cloud-Einführungsstrategien, um ein potenziell höheres Risiko und neue Konfigurationsanforderungen bzw. sonstige Möglichkeiten zu identifizieren. Nutzen Sie diesen Zeitpunkt auch, um die neuesten bewährten Methoden für die Beschleunigung der Bereitstellung zu überprüfen und in Ihre Richtlinien und Überprüfungsprozesse zu integrieren.

**Vierteljährliche Prüfung und Planung**: Überprüfen Sie vierteljährlich die Sicherheitsüberwachungsdaten und Vorfallsberichte, um alle Änderungen zu identifizieren, die an der Richtlinie zur Beschleunigung der Bereitstellung vorgenommen werden müssen. Überprüfen Sie im Rahmen dieses Prozesses die aktuelle Landschaft der Cybersicherheit, um neuen Bedrohungen proaktiv zuvorzukommen und die Richtlinie entsprechend zu aktualisieren. Gleichen Sie nach Abschluss der Überprüfung die Entwurfsleitlinien für Anwendungen und Systeme an die aktualisierte Richtlinie an.

Dieser Planungsprozess ist auch ein guter Zeitpunkt, um die aktuelle Zusammensetzung Ihres Cloud Governance-Teams auf Wissenslücken im Zusammenhang mit neuen oder in der Entwicklung befindlichen Richtlinien und Risiken bei DevOps und der Beschleunigung der Bereitstellung zu überprüfen. Laden Sie maßgebliche IT-Mitarbeiter zur Teilnahme an Überprüfungen und Planungen ein, entweder zeitlich begrenzt als technische Berater oder als ständige Mitglieder Ihres Teams.

**Aus- und Weiterbildung**: Bieten Sie zweimonatlich Schulungen an, um sicherzustellen, dass IT-Mitarbeiter und Entwickler hinsichtlich Strategie und Anforderungen im Zusammenhang mit der Beschleunigung der Bereitstellung auf dem neuesten Stand sind. Überprüfen und aktualisieren Sie im Rahmen dieses Prozesses alle Unterlagen, Anleitungen oder sonstige Schulungsmaterialien, um sicherzustellen, dass sie mit den neuesten Richtlinienanweisungen im Unternehmen übereinstimmen.

**Monatliche Überprüfungen und Berichte**: Führen Sie monatlich eine Überprüfung aller Cloudbereitstellungen durch, um eine fortgesetzte Übereinstimmung mit den Konfigurationsrichtlinien zu gewährleisten. Überprüfen Sie sicherheitsbezogene Aktivitäten mit IT-Mitarbeitern, und identifizieren Sie etwaige Konformitätsprobleme, die noch nicht im Rahmen des laufenden Überwachungs- und Durchsetzungsprozesses behandelt wurden. Als Ergebnis dieser Überprüfungsprozess wird ein Bericht für das Cloudstrategieteam und jedes Cloudeinführungsteam erstellt, um die allgemeine Einhaltung der Richtlinie zu kommunizieren. Der Bericht wird außerdem für prüfungsbezogene und rechtliche Zwecke gespeichert.

## <a name="ongoing-monitoring-processes"></a>Laufende Überwachungsprozesse

Die Ermittlung, ob Ihre Governancestrategie für die Beschleunigung der Bereitstellung erfolgreich ist, hängt von der Transparenz des aktuellen und vergangenen Zustands Ihrer Cloudinfrastruktur ab. Ohne die Möglichkeit, die relevanten Metriken und Daten im Zusammenhang mit der Sicherheitsintegrität und den Aktivitäten der Cloudressourcen zu analysieren, können Sie keine Veränderungen bei den Risiken und keine Verstöße gegen Ihre Risikotoleranzen erkennen. Für die oben genannten laufenden Governanceprozesse sind Qualitätsdaten erforderlich, um eine entsprechende Änderung der Richtlinie zu gewährleisten und Ihre Infrastruktur vor wechselnden Bedrohungen und Risiken durch falsch konfigurierte Ressourcen zu schützen.

Stellen Sie sicher, dass Ihre Sicherheits- und IT-Teams automatisierte Überwachungssysteme für die Cloudinfrastruktur implementiert haben, welche die entsprechenden Protokolldaten erfassen, die Sie für die Risikobewertung benötigen. Überwachen Sie diese Systeme proaktiv, um eine schnelle Erkennung und Eindämmung potenzieller Richtlinienverletzungen zu gewährleisten und sicherzustellen, dass bei Ihrer Überwachungsstrategie alle Bereitstellungs- und Konfigurationsanforderungen berücksichtigt werden.

## <a name="violation-triggers-and-enforcement-actions"></a>Auslöser bei Verstößen und Durchsetzungsmaßnahmen

Da bei einer Nichteinhaltung von Konfigurationsrichtlinien das Risiko einer Unterbrechung wichtiger Dienste besteht, sollte das Cloud Governance-Team Einblick in schwerwiegende Richtlinienverstöße haben. Stellen Sie sicher, dass die IT-Mitarbeiter für die Meldung von Konfigurationskonformitätsproblemen an Mitglieder des Governance-Teams über eindeutige Eskalationspfade verfügen, mit denen sich die Behebung von Richtlinienproblemen optimal identifizieren und überprüfen lässt.  

Wenn Verstöße festgestellt werden, sollten Sie so schnell wie möglich Maßnahmen ergreifen, um eine erneute Einhaltung der Richtlinie zu erreichen. Ihr IT-Team kann die meisten Auslöser bei Verstößen mit den unter [Tools für die Beschleunigung der Bereitstellung in Azure](toolchain.md) beschriebenen Tools automatisieren.

Die folgenden Auslöser und Durchsetzungsmaßnahmen sind Beispiele, die Sie bei der Diskussion über die Verwendung von Überwachungsdaten zur Behebung von Richtlinienverstößen verwenden können:

- **Unerwartete Änderungen in der Konfiguration erkannt**. Wenn sich die Konfiguration einer Ressource unerwartet ändert, arbeiten Sie mit IT-Mitarbeitern und Workloadbesitzern zusammen, um die Hauptursache zu identifizieren und einen Plan zur Problembehandlung zu entwickeln.
- **Konfiguration der neuen Ressourcen entspricht nicht der Richtlinie.** Arbeiten Sie mit DevOps-Teams und Workloadbesitzern zusammen, um die Richtlinien für die Beschleunigung der Bereitstellung beim Projektstart zu überprüfen, damit jeder Beteiligte die relevanten Anforderungen der Richtlinie versteht.
- **Fehler bei der Bereitstellung oder Konfigurationsprobleme führen zu Verzögerungen in Projektzeitplänen.** Arbeiten Sie mit Entwicklungsteams und Workloadbesitzern zusammen, um sicherzustellen, dass das Team versteht, wie die Bereitstellung von cloudbasierten Ressourcen hinsichtlich Konsistenz und Wiederholbarkeit automatisiert werden kann. Vollautomatische Bereitstellungen sollten früh im Entwicklungszyklus gefordert sein &mdash; eine Umsetzung zu einem späteren Zeitpunkt im Entwicklungszyklus führt in der Regel zu unerwarteten Problemen und Verzögerungen.

## <a name="next-steps"></a>Nächste Schritte

Dokumentieren Sie mit der Vorlage [Cloudverwaltung](./template.md) die Prozesse und Auslöser, die sich an dem aktuellen Cloudeinführungsplan orientieren.

Einen Leitfaden zur Ausführung von Cloudverwaltungsrichtlinien in Abstimmung mit Einführungsplänen finden Sie im Artikel über die Verbesserung von Disziplinen.

> [!div class="nextstepaction"]
> [Verbesserung der Disziplin „Beschleunigung der Bereitstellung“](./discipline-improvement.md)
