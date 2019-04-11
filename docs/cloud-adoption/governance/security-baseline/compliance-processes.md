---
title: 'CAF: Sicherheitsbaseline: Prozesse für Richtlinienkonformität'
titleSuffix: Microsoft Cloud Adoption Framework for Azure
ms.service: architecture-center
ms.subservice: enterprise-cloud-adoption
ms.custom: governance
ms.date: 02/11/2019
description: 'Sicherheitsbaseline: Prozesse für Richtlinienkonformität'
author: BrianBlanchard
ms.openlocfilehash: 4002791808a0e02fed9bf8564e5851b3b6360221
ms.sourcegitcommit: c053e6edb429299a0ad9b327888d596c48859d4a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/20/2019
ms.locfileid: "58243651"
---
# <a name="security-baseline-policy-compliance-processes"></a>Sicherheitsbaseline: Prozesse für Richtlinienkonformität

In diesem Artikel wird ein Ansatz für Prozesse zur Einhaltung von Richtlinien beschrieben, die die [Sicherheitsbaseline](./overview.md) regeln. Eine wirkungsvolle Governance der Cloudsicherheit beginnt mit sich wiederholenden manuellen Prozessen, die zum Erkennen von Sicherheitsrisiken und zum Festlegen der Richtlinien entwickelt wurden, die diese Risiken minimieren sollen. Dies erfordert die regelmäßige Einbeziehung des Cloud Governance-Teams sowie interessierter Geschäfts- und IT-Beteiligter, um Richtlinien zu überprüfen und zu aktualisieren und die Einhaltung von Richtlinien sicherzustellen. Darüber hinaus können viele laufende Überwachungs- und Durchsetzungsprozesse automatisiert oder durch Werkzeuge ergänzt werden, um den Kontrolloverhead zu reduzieren und schneller auf Abweichungen von der Richtlinie reagieren zu können.

## <a name="planning-review-and-reporting-processes"></a>Planungs-, Prüfungs- und Berichterstellungsprozesse

Die besten Sicherheitsbaseline-Tools in der Cloud sind nur so gut wie die Prozesse und Richtlinien, die sie unterstützen. Nachstehend sind eine Reihe von Beispielprozessen ausgeführt, die häufig im Rahmen der Disziplin „Sicherheitsbaseline“ verwendet werden. Beim Planen der Prozesse, die Ihnen die fortlaufende Aktualisierung der Sicherheitsrichtlinie aufgrund von geschäftlichen Änderungen und Feedback von den Sicherheits- und IT-Teams (deren Aufgabe es ist, den Governanceleitfaden in die Tat umzusetzen) ermöglichen, können Sie diese Beispiele als Ausgangspunkt verwenden.

**Anfängliche Risikobewertung und Planung**: Als Teil Ihrer erstmaligen Einführung der Disziplin „Sicherheitsbaseline“ identifizieren Sie Ihre Kerngeschäftsrisiken und Toleranzen im Zusammenhang mit der Cloudsicherheit. Verwenden Sie diese Informationen, um spezifische technische Risiken mit Mitgliedern Ihrer IT- und Sicherheitsteams zu besprechen, und entwickeln Sie einen grundlegenden Satz von Sicherheitsrichtlinien zur Minimierung dieser Risiken, um Ihre erste Governancestrategie zu formulieren.

**Bereitstellungsplanung**: Führen Sie vor der Bereitstellung einer Workload oder Ressource eine Sicherheitsüberprüfung aus, um mögliche neue Risiken zu identifizieren und sicherzustellen, dass alle Anforderungen von Zugriffs- und Datensicherheitsrichtlinien erfüllt werden.

**Bereitstellungstests**: Im Rahmen des Bereitstellungsprozesses für eine Workload oder Ressource ist das Cloud Governance-Team zusammen mit den Teams für die Unternehmenssicherheit für die Überprüfung der Bereitstellung verantwortlich, um die Einhaltung der Sicherheitsrichtlinie zu gewährleisten.

**Jährliche Planung**: Führen Sie jährlich eine allgemeine Überprüfung der Strategie für die Sicherheitsbaseline durch. Ermitteln Sie zukünftige Prioritäten des Unternehmens und aktualisierte Cloud-Einführungsstrategien, um ein potenziell höheres Risiko und andere sich ergebende Sicherheitsanforderungen zu identifizieren. Nutzen Sie diese Zeit auch, um die neuesten Best Practices für die Sicherheitsbaseline zu überprüfen und in Ihre Richtlinien und Überprüfungsprozesse zu integrieren.

**Vierteljährliche Prüfung und Planung**: Überprüfen Sie vierteljährlich die Sicherheitsüberwachungsdaten und Vorfallsberichte, um alle Änderungen zu identifizieren, die an der Sicherheitsrichtlinie vorgenommen werden müssen. Überprüfen Sie im Rahmen dieses Prozesses die aktuelle Landschaft der Cybersicherheit, um neuen Bedrohungen proaktiv zuvorzukommen und die Richtlinie entsprechend zu aktualisieren. Gleichen Sie nach Abschluss der Überprüfung die Entwurfsleitlinien an die aktualisierte Richtlinie an.

Dieser Planungsprozess ist auch ein guter Zeitpunkt, um die aktuelle Zusammensetzung Ihres Cloud Governance-Teams auf Wissenslücken im Zusammenhang mit neuen oder zu entwickelnden Richtlinien und Risiken im Bezug auf die Sicherheit zu überprüfen. Laden Sie relevante IT-Mitarbeiter ein, an Überprüfungen und Planungen teilzunehmen, entweder als zeitlich begrenzte technische Berater oder als ständige Mitglieder Ihres Teams.

**Aus- und Weiterbildung**: Bieten Sie zweimonatlich Schulungen an, um sicherzustellen, dass IT-Mitarbeiter und Entwickler auf dem neuesten Stand der Sicherheitsrichtlinien sind. Überprüfen und aktualisieren Sie im Rahmen dieses Prozesses alle Unterlagen, Anleitungen oder sonstige Schulungsmaterialien, um sicherzustellen, dass sie mit den neuesten Richtlinienanweisungen im Unternehmen übereinstimmen.

**Monatliche Überprüfungen und Berichte**: Führen Sie monatlich eine Überprüfung für alle Cloudbereitstellungen durch, um sicherzustellen, dass sie weiterhin mit den Sicherheitsrichtlinien übereinstimmen. Überprüfen Sie sicherheitsbezogene Aktivitäten mit IT-Mitarbeitern, und identifizieren Sie etwaige Konformitätsprobleme, die noch nicht im Rahmen des laufenden Überwachungs- und Durchsetzungsprozesses behandelt wurden. Als Ergebnis dieser Überprüfung wird ein Bericht für das Cloudstrategieteam und jedes Cloudeinführungsteam erstellt, um die allgemeine Einhaltung der Richtlinie zu kommunizieren. Der Bericht wird außerdem für prüfungsbezogene und rechtliche Zwecke gespeichert.

## <a name="ongoing-monitoring-processes"></a>Fortlaufende Überwachungsprozesse

Die Ermittlung, ob Ihre Strategie für die Sicherheitsgovernance erfolgreich ist, hängt von der Transparenz des aktuellen und vergangenen Zustands Ihrer Cloudinfrastruktur ab. Ohne die Möglichkeit, die relevanten Metriken und Daten im Zusammenhang mit der Sicherheitsintegrität und den Aktivitäten der Cloudressourcen zu analysieren, können Sie keine Veränderungen bei den Risiken und keine Verstöße gegen Ihre Risikotoleranzen erkennen. Für die oben genannten laufenden Governanceprozesse sind Qualitätsdaten erforderlich, um eine entsprechende Änderung der Richtlinie zu gewährleisten und Ihre Infrastruktur besser vor wechselnden Bedrohungen und Sicherheitsanforderungen zu schützen.

Stellen Sie sicher, dass Ihre Sicherheits- und IT-Teams automatisierte Überwachungssysteme für die Cloudinfrastruktur implementiert haben, welche die entsprechenden Protokolldaten erfassen, die Sie für die Risikobewertung benötigen. Überwachen Sie diese Systeme proaktiv, um eine schnelle Erkennung und Eindämmung potenzieller Richtlinienverletzungen zu gewährleisten und sicherzustellen, dass bei Ihrer Überwachungsstrategie die Sicherheitsanforderungen berücksichtigt werden.

## <a name="violation-triggers-and-enforcement-actions"></a>Auslöser bei Verstößen und Durchsetzungsmaßnahmen

Da bei einer Nichteinhaltung der Sicherheitsrichtlinien das Risiko einer Offenlegung kritischer Daten und Unterbrechung wichtiger Dienste besteht, sollte das Cloud Governance-Team Einblick in schwerwiegende Richtlinienverstöße haben. Stellen Sie sicher, dass die IT-Mitarbeiter für die Meldung von Sicherheitsproblemen an Mitglieder des Governance-Teams über eindeutige Eskalationspfade verfügen, mit denen sich die Behebung von Richtlinienproblemen optimal identifizieren und überprüfen lässt.  

Wenn Verstöße festgestellt werden, sollten Sie so schnell wie möglich Maßnahmen ergreifen, um eine erneute Einhaltung der Richtlinien zu erreichen. Ihr IT-Team kann die meisten Auslöser bei Verstößen mit den in der [Sicherheitsbaseline-Toolkette für Azure](toolchain.md) beschriebenen Tools automatisieren.

Die folgenden Auslöser und Durchsetzungsmaßnahmen sind Beispiele, auf die Sie bei der Planung der Verwendung von Überwachungsdaten zur Behebung von Richtlinienverstößen verweisen können:

- Zunahme der Angriffe erkannt: Wenn bei einer Ressource eine Zunahme der Brute-Force- oder DDoS-Angriffe um 25 % auftritt, besprechen Sie dies mit dem IT-Sicherheitspersonal und dem Workloadbesitzer, um Abhilfemaßnahmen zu bestimmen. Verfolgen Sie das Problem, und aktualisieren Sie die Anleitung, falls eine Richtlinienüberarbeitung erforderlich ist, um zukünftige Vorfälle zu verhindern.
- Nicht klassifizierte Daten erkannt: Der externe Zugriff auf eine Datenquelle ohne geeignete Klassifizierung von Datenschutz, Sicherheit oder geschäftlichen Auswirkungen wird verweigert, bis die Klassifizierung vom Datenbesitzer angewendet und das entsprechende Datenschutzniveau umgesetzt wird.
- Problem der Sicherheitsintegrität erkannt: Deaktivieren Sie den Zugriff auf alle virtuellen Computer (VMs), für die bekannte Sicherheitsrisiken beim Zugriff oder durch Schadsoftware bestehen, bis geeignete Patches oder Sicherheitssoftware installiert werden können. Aktualisieren Sie die Richtlinienanweisungen, um alle neu erkannten Bedrohungen zu berücksichtigen.
- Netzwerksicherheitsrisiko erkannt: Der Zugriff auf eine Ressource, die nicht ausdrücklich durch die Netzwerkzugriffsrichtlinien als zulässig eingestuft ist, sollte eine Warnung an das IT-Sicherheitspersonal und den Besitzer der jeweiligen Workload auslösen. Verfolgen Sie das Problem, und aktualisieren Sie die Anleitung, falls eine Richtlinienüberarbeitung erforderlich ist, um zukünftige Vorfälle zu minimieren.

## <a name="next-steps"></a>Nächste Schritte

Dokumentieren Sie mit der Vorlage [Cloudverwaltung](./template.md) die Prozesse und Auslöser, die sich an dem aktuellen Cloudeinführungsplan orientieren.

Anleitungen zur Ausführung von Cloudverwaltungsrichtlinien in Abstimmung mit Einführungsplänen finden Sie im Artikel über Verbesserungen von Disziplinen.

> [!div class="nextstepaction"]
> [Verbesserung der Disziplin „Sicherheitsbaseline“](./discipline-improvement.md)
