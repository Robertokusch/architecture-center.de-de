---
title: 'Framework für die Cloudeinführung (CAF): Prozesse für die Compliance der Ressourcenkonsistenzrichtlinie'
titleSuffix: Microsoft Cloud Adoption Framework for Azure
ms.service: architecture-center
ms.subservice: enterprise-cloud-adoption
ms.custom: governance
ms.date: 02/11/2019
description: Prozesse für die Compliance der Ressourcenkonsistenzrichtlinie
author: BrianBlanchard
ms.openlocfilehash: bfe241dd7aa723dc5a8c0109a07c86de98418690
ms.sourcegitcommit: c053e6edb429299a0ad9b327888d596c48859d4a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/20/2019
ms.locfileid: "58245131"
---
# <a name="resource-consistency-policy-compliance-processes"></a>Prozesse für die Compliance der Ressourcenkonsistenzrichtlinie

In diesem Artikel wird ein Ansatz für Prozesse zur Einhaltung von Richtlinien beschrieben, die die [Ressourcenkonsistenz](./overview.md) regeln. Effektive Governance der Cloudressourcenkonsistenz beginnt mit wiederkehrenden manuellen Prozessen, die den Zweck haben, operative Ineffizienz zu identifizieren, die Verwaltung von bereitgestellten Ressourcen zu verbessern und minimale Unterbrechungen von unternehmenskritischen Workloads sicherzustellen. Diese manuellen Prozesse werden ergänzt durch Überwachung, Automatisierung und den Einsatz von Tools, um den Governance-Overhead zu reduzieren und schneller auf Abweichungen von der Richtlinie reagieren zu können.

## <a name="planning-review-and-reporting-processes"></a>Planungs-, Prüfungs- und Berichterstellungsprozesse

Cloudplattformen bieten eine ganze Reihe von Verwaltungstools und -funktionen zum Organisieren, Bereitstellen, Skalieren und Minimieren von Ausfallzeiten. Der Einsatz dieser Tools zur effektiven Strukturierung und dem Betrieb Ihrer Cloudbereitstellungen auf Weisen, die potenzielle Risiken verringern, erfordert wohl durchdachte Prozesse und Richtlinien, zusätzlich zu einer engen Zusammenarbeit mit IT-Betriebsteams und Projektbeteiligten des Unternehmens.

Nachstehend sind eine Reihe von Beispielprozessen ausgeführt, die häufig im Rahmen der Disziplin „Ressourcenkonsistenz“ verwendet werden. Beim Planen der Prozesse, die Ihnen die fortgesetzte Aktualisierung der Ressourcenkonsistenzrichtlinie aufgrund von geschäftlichen Änderungen und Feedback von den Entwicklungs- und IT-Teams (deren Aufgabe es ist, den Leitfaden in die Tat umzusetzen) erlauben, können Sie diese Beispiele als Ausgangspunkt verwenden.

**Anfängliche Risikobewertung und Planung**: Im Rahmen Ihrer erstmaligen Einführung der Disziplin „Ressourcenkonsistenz“ identifizieren Sie Ihre Kerngeschäftsrisiken und -toleranzen im Zusammenhang mit dem Betrieb und der IT-Verwaltung. Verwenden Sie diese Informationen, um spezifische technische Risiken mit Mitgliedern Ihrer IT-Teams und Workloadbesitzer zu besprechen, um einen grundlegenden Satz von Ressourcenkonsistenzrichtlinien zur Verringerung dieser Risiken zu entwickeln, um Ihre anfängliche Governance-Strategie zu formulieren.

**Bereitstellungsplanung**: Führen Sie vor der Bereitstellung von Ressourcen eine Überprüfung durch, um alle neuen Betriebsrisiken zu identifizieren. Stellen Sie Ressourcenanforderungen und erwartete Bedarfsmuster auf, und identifizieren Sie Anforderungen an die Skalierbarkeit und potenzielle Möglichkeiten zur Optimierung der Verwendung. Stellen Sie außerdem sicher, dass Sicherungs- und Wiederherstellungspläne vorhanden sind.

**Bereitstellungstests**: Im Rahmen der Bereitstellung ist das Cloud Gvernance-Team in Zusammenarbeit mit Ihren Cloudbetriebsteams für die Überprüfung der Bereitstellung verantwortlich, um die Compliance der Ressourcenkonsistenzrichtlinie zu überprüfen.

**Jährliche Planung**: Führen Sie jährlich eine allgemeine Überprüfung der Ressourcenkonsistenzstrategie durch. Ermitteln Sie zukünftige Pläne zur Erweiterung des Unternehmens oder Prioritäten, und aktualisieren Sie Strategien zur Einführung der Cloud, um einen potenziellen Risikoanstieg oder andere sich entwickelnde Ressourcenkonsistenzanforderungen zu identifizieren. Nutzen Sie diese Zeit auch, um die neuesten bewährten Methoden für die Cloudressourcenkonsistenz zu überprüfen, und integrieren Sie diese in Ihre Richtlinien und Überprüfungsprozesse.

**Vierteljährliche Prüfung und Planung**: Überprüfen Sie vierteljährlich die operationalen Daten und Vorfallsberichte, um alle Änderungen zu identifizieren, die an der Ressourcenkonsistenzrichtlinie vorgenommen werden müssen. Überprüfen Sie im Rahmen dieses Prozesses Änderungen bei der Ressourcennutzung und Leistung, um Ressourcen zu identifizieren, erhöhte oder reduzierte Ressourcenzuordnung erfordern, und identifizieren Sie alle Workloads oder Ressourcen, die für die Außerbetriebnahme in Frage kommen.

Dieser Planungsprozess ist auch ein guter Zeitpunkt, um die aktuellen Mitglieder Ihres Cloud Governance-Teams auf Wissenslücken im Zusammenhang mit neuen oder sich entwickelnden Richtlinien und Risiken bezüglich der Ressourcenkonsistenz als Disziplin zu überprüfen. Laden Sie relevante IT-Mitarbeiter ein, an Überprüfungen und Planungen teilzunehmen, entweder als zeitlich begrenzte technische Berater oder als ständige Mitglieder Ihres Teams.

**Aus- und Weiterbildung**: Bieten Sie zweimonatlich Schulungen an, um sicherzustellen, dass IT-Mitarbeiter und Entwickler auf dem neuesten Stand bei Richtlinienanforderungen und Anleitungen für die Ressourcenkonsistenz sind. Im Rahmen dieses Prozesses überprüfen und aktualisieren Sie alle Unterlagen oder anderen Schulungsmaterialien, um sicherzustellen, dass sie mit den neuesten Unternehmensrichtlinien übereinstimmen.

**Monatliche Überprüfungen und Berichte**: Führen Sie monatlich eine Überprüfung für alle Cloudbereitstellungen durch, um sicherzustellen, dass sie weiterhin mit den Ressourcenkonsistenzrichtlinien übereinstimmen. Überprüfen Sie diesbezügliche Aktivitäten mit IT-Mitarbeitern, und identifizieren Sie etwaige Konformitätsprobleme, die noch nicht im Rahmen des laufenden Überwachungs- und Durchsetzungsprozesses behandelt wurden. Als Ergebnis dieser Überprüfungsprozess wird ein Bericht für das Cloudstrategieteam und jedes Cloudeinführungsteam erstellt, um die allgemeine Leistung und Einhaltung der Richtlinie zu kommunizieren. Der Bericht wird auch für prüfungsbezogene und rechtliche Zwecke gespeichert.

## <a name="ongoing-monitoring-processes"></a>Fortlaufende Überwachungsprozesse

Die Ermittlung, ob Ihre Ressourcenkonsistenzstrategie für die Beschleunigung der Bereitstellung erfolgreich ist, hängt von der Transparenz des aktuellen und vergangenen Zustands Ihrer Cloudinfrastruktur ab. Ohne die Möglichkeit, die relevanten Metriken und Daten im Zusammenhang mit der Integrität und den Aktivitäten Ihrer Cloudumgebung zu analysieren, können Sie keine Veränderungen bei den Risiken und keine Verstöße gegen Ihre Risikotoleranzen erkennen. Die oben beschriebenen laufenden Kontrollprozesse erfordern qualitativ hochwertige Daten, um sicherzustellen, dass die Richtlinie so geändert werden kann, dass Ihre Cloudressourcennutzung optimiert und die Gesamtleistung von in der Cloud gehosteten Workloads verbessert werden kann.

Stellen Sie sicher, dass Ihre IT-Teams automatisierte Überwachungssysteme für die Cloudinfrastruktur implementiert haben, welche die entsprechenden Protokolldaten erfassen, die Sie für die Risikobewertung benötigen. Überwachen Sie diese Systeme proaktiv, um eine schnelle Erkennung und Eindämmung potenzieller Richtlinienverletzungen zu gewährleisten und sicherzustellen, dass bei Ihrer Überwachungsstrategie Ihre Betriebsanforderungen berücksichtigt werden.

## <a name="violation-triggers-and-enforcement-actions"></a>Auslöser bei Verstößen und Durchsetzungsmaßnahmen

Da bei der Compliance der Ressourcenkonsistenzrichtlinie das Risiko einer Unterbrechung kritischer Dienste oder der Entstehung übermäßiger Kosten besteht, sollte das Cloud Governance-Team Einblick in Non-Compliance-Vorfälle haben. Stellen Sie sicher, dass die IT-Mitarbeiter für die Meldung solcher Probleme an Mitglieder des Governance-Teams über eindeutige Eskalationspfade verfügen, mit denen sich die Behebung von Richtlinienproblemen optimal identifizieren und überprüfen lässt.  

Wenn Verstöße festgestellt werden, sollten Sie so schnell wie möglich Maßnahmen ergreifen, um eine erneute Einhaltung der Richtlinien zu erreichen. Ihr IT-Team kann die meisten Auslöser bei Verstößen mit den unter [Toolkette der Ressourcenkonsistenz für Azure](toolchain.md) beschriebenen Tools automatisieren.

Die folgenden Auslöser und Durchsetzungsmaßnahmen sind Beispiele, auf die Sie bei der Planung der Verwendung von Überwachungsdaten zur Behebung von Richtlinienverstößen verweisen können:

- Überdimensionierte Ressource erkannt: Ressourcen, bei denen erkannt wird, dass sie weniger als 60 % der CPU- oder Arbeitsspeicherkapazität nutzen, sollten automatisch zentral herunterskaliert werden oder die Bereitstellung von Ressourcen aufheben, um Kosten zu senken.
- Unterdimensionierte Ressource erkannt: Ressourcen, bei denen erkannt wird, dass sie mehr als 80 % der CPU- oder Arbeitsspeicherkapazität nutzen, sollten automatisch zentral hochskaliert werden oder zusätzliche Ressourcen bereitstellen, um zusätzliche Kapazitäten bereitzustellen.
- Ressourcenerstellung ohne Tags: Jede Anforderungen zum Erstellen einer Ressource ohne erforderliche Metatags wird automatisch abgelehnt.
- Ausfall kritischer Ressource erkannt: IT-Mitarbeiter werden bei allen erkannten Ausfällen unternehmenskritischer Ressourcen benachrichtigt. Wenn sich der Ausfall nicht sofort beheben lässt, eskalieren Mitarbeiter das Problem und benachrichtigen Besitzer von Workloads und das Cloud-Governance-Team. Das Cloud-Governance-Team verfolgt das Problem bis zu seiner Lösung und aktualisiert die Anleitung, falls eine Richtlinienüberarbeitung erforderlich ist, um zukünftige Vorfälle zu verhindern.

## <a name="next-steps"></a>Nächste Schritte

Dokumentieren Sie mit der Vorlage [Cloudverwaltung](./template.md) die Prozesse und Auslöser, die sich an dem aktuellen Cloudeinführungsplan orientieren.

Anleitungen zur Ausführung von Cloudverwaltungsrichtlinien in Abstimmung mit Einführungsplänen finden Sie im Artikel über Verbesserungen von Disziplinen.

> [!div class="nextstepaction"]
> [Verbesserung der Disziplin „Ressourcenkonsistenz“](./discipline-improvement.md)
