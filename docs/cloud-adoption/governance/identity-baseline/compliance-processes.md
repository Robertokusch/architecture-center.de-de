---
title: 'CAF: Identitätsbaseline: Prozesse für Richtlinienkonformität'
titleSuffix: Microsoft Cloud Adoption Framework for Azure
ms.service: architecture-center
ms.subservice: enterprise-cloud-adoption
ms.custom: governance
ms.date: 02/11/2019
description: 'Identitätsbaseline: Prozesse für Richtlinienkonformität'
author: BrianBlanchard
ms.openlocfilehash: ef973a788b49d6818e9647601a16a772106b53eb
ms.sourcegitcommit: c053e6edb429299a0ad9b327888d596c48859d4a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/20/2019
ms.locfileid: "58242571"
---
# <a name="identity-baseline-policy-compliance-processes"></a>Identitätsbaseline: Prozesse für Richtlinienkonformität

In diesem Artikel wird ein Ansatz für Prozesse zur Einhaltung von Richtlinien beschrieben, die die [Identitätsbaseline](./overview.md) regeln. Eine wirkungsvolle Identitätskontrolle (Governance) beginnt mit wiederkehrenden manuellen Prozessen zur Steuerung von Übernahme und Überarbeitungen der Identitätsrichtlinien. Dies erfordert die regelmäßige Einbeziehung des Cloud Governance-Teams sowie interessierter Geschäfts- und IT-Beteiligter, um Richtlinien zu überprüfen und zu aktualisieren und die Einhaltung von Richtlinien sicherzustellen. Darüber hinaus können viele laufende Überwachungs- und Durchsetzungsprozesse automatisiert oder durch Werkzeuge ergänzt werden, um den Kontrolloverhead zu reduzieren und schneller auf Abweichungen von der Richtlinie reagieren zu können.

## <a name="planning-review-and-reporting-processes"></a>Planungs-, Prüfungs- und Berichtsprozesse

Identitätsverwaltungstools bieten Funktionen und Features, die Benutzerverwaltung und Zugriffssteuerung innerhalb einer Cloudbereitstellung erheblich unterstützen. Sie erfordern aber auch gut durchdachte Prozesse und Richtlinien, um die Ziele Ihres Unternehmens zu unterstützen. Nachstehend sind eine Reihe von Beispielprozessen ausgeführt, die häufig im Rahmen der Disziplin „Identitätsbaseline“ verwendet werden. Beim Planen der Prozesse, die Ihnen die fortgesetzte Aktualisierung der Identitätsrichtlinie aufgrund von geschäftlichen Änderungen und Feedback von den IT-Teams (deren Aufgabe es ist, den Governanceleitfaden in die Tat umzusetzen) erlauben, können Sie diese Beispiele als Ausgangspunkt verwenden.

**Anfängliche Risikobewertung und Planung**: Als Teil Ihrer erstmaligen Einführung der Disziplin „Identitätsbaseline“ identifizieren Sie Ihre Kerngeschäftsrisiken und Toleranzen im Zusammenhang mit der Identitätsverwaltung in der Cloud. Verwenden Sie diese Informationen, um spezifische technische Risiken mit Mitgliedern Ihrer IT-Teams zu besprechen, die für die Verwaltung von Identitätsdiensten verantwortlich sind, und entwickeln Sie einen grundlegenden Satz von Sicherheitsrichtlinien zur Minimierung dieser Risiken, um Ihre erste Governancestrategie zu formulieren.

**Bereitstellungsplanung**: Überprüfen Sie vor jeder Bereitstellung den Zugriffsbedarf für alle Workloads, und entwickeln Sie eine Zugriffssteuerungsstrategie, die auf die etablierte Corporate Identity-Richtlinie abgestimmt ist. Dokumentieren Sie alle Lücken zwischen dem Bedarf und der aktuellen Richtlinie, um festzustellen, ob Aktualisierungen der Richtlinie erforderlich sind, und ändern Sie die Richtlinie bei Bedarf.

**Bereitstellungstests**: Im Rahmen der Bereitstellung ist das Cloud Governance-Team in Zusammenarbeit mit den für Identitätsdienste zuständigen IT-Teams für die Überprüfung der Bereitstellung zur Überprüfung der Einhaltung der Identitätsrichtlinien verantwortlich.

**Jährliche Planung**: Führen Sie jährlich eine allgemeine Überprüfung der Identitätsverwaltungsstrategie durch. Untersuchen Sie geplante Änderungen an der Umgebung für Identitätsdienste und aktualisierte Cloudeinführungsstrategien, um potenzielle Risiken zu identifizieren oder aktuelle Muster der Identitätsinfrastruktur zu ändern. Nutzen Sie diese Zeit auch, um die neuesten bewährten Methoden für die Identitätsverwaltung zu überprüfen und in Ihre Richtlinien und Überprüfungsprozesse zu integrieren.

**Vierteljährliche Planung**: Führen Sie vierteljährlich eine allgemeine Überprüfung der Überwachungsdaten der Identitäts- und Zugriffssteuerung durch, und treffen Sie sich mit Cloudeinführungsteams, um potenzielle neue Risiken oder betriebliche Anforderungen zu identifizieren, die ggf. eine Aktualisierung der Identitätsrichtlinie oder Änderungen der Zugriffssteuerungsstrategie erforderlich machen.

Dieser Planungsprozess ist auch ein guter Zeitpunkt, um die aktuelle Zusammensetzung Ihres Cloud Governance-Teams auf Wissenslücken im Zusammenhang mit neuen oder zu entwickelnden Richtlinien sowie Identitätsrisiken zu überprüfen. Laden Sie relevante IT-Mitarbeiter ein, an Überprüfungen und Planungen teilzunehmen, entweder zeitlich begrenzt als technische Berater oder als ständige Mitglieder Ihres Teams.  

**Aus- und Weiterbildung**: Bieten Sie zweimonatlich Trainingssitzungen an, um sicherzustellen, dass IT-Mitarbeiter und Entwickler auf dem neuesten Stand der Identitätsrichtlinien sind. Im Rahmen dieses Prozesses überprüfen und aktualisieren Sie alle Unterlagen, Anleitungen oder andere Trainingsmaterialien, um sicherzustellen, dass sie mit den neuesten Unternehmensrichtlinien übereinstimmen.

**Monatliche Überprüfungen und Berichte**: Führen Sie monatlich eine Überprüfung für alle Cloudbereitstellungen durch, um sicherzustellen, dass sie weiterhin mit den Identitätsrichtlinien übereinstimmen. Überprüfen Sie dabei den Benutzerzugriff bei geschäftlichen Änderungen, um zu gewährleisten, dass Benutzer den erforderlichen Zugriff auf Cloudressourcen erhalten und Zugriffsstrategien wie RBAC konsequent eingehalten werden. Identifizieren Sie alle privilegierten Konten und dokumentieren Sie deren Zweck. Dieser Überprüfungsprozess erstellt einen Bericht für das Cloudstrategieteam und jedes Cloudeinführungsteam mit Details zur allgemeinen Einhaltung der Richtlinien. Der Bericht wird auch für prüfungsbezogene und rechtliche Zwecke gespeichert.

## <a name="ongoing-monitoring-processes"></a>Fortlaufende Überwachungsprozesse

Ob Ihre Identitätsverwaltungsstrategie erfolgreich ist, hängt von der Transparenz des aktuellen und vergangenen Zustands Ihrer Identitätssysteme ab. Ohne die Möglichkeit, die relevanten Metriken und Daten Ihrer Cloudbereitstellung zu analysieren, können Sie keine Veränderungen bei den Risiken und keine Verstöße gegen Ihre Risikotoleranzen erkennen. Die oben beschriebenen laufenden Kontrollprozesse erfordern qualitativ hochwertige Daten, um sicherzustellen, dass die Richtlinie an die sich ändernden Bedürfnisse Ihres Unternehmens angepasst werden kann.

Stellen Sie sicher, dass Ihre IT-Teams automatisierte Überwachungssysteme für Ihre Identitätsdienste implementiert haben, die die Protokolle und Prüfungsinformationen erfassen, die Sie zur Risikobewertung benötigen. Überwachen Sie diese Systeme proaktiv, um eine schnelle Erkennung und Eindämmung potenzieller Richtlinienverletzungen zu gewährleisten und sicherzustellen, dass alle Änderungen an Ihrer Identitätsinfrastruktur in Ihrer Überwachungsstrategie berücksichtigt werden.

## <a name="violation-triggers-and-enforcement-actions"></a>Auslöser bei Verstößen und Durchsetzungsmaßnahmen

Verstöße gegen die Identitätsrichtlinie können zu unbefugtem Zugriff auf sensible Daten und schwerwiegenden Störungen unternehmenskritischer Anwendungen und Dienste führen. Wenn Verstöße festgestellt werden, sollten Sie so schnell wie möglich Maßnahmen ergreifen, um eine erneute Einhaltung der Richtlinien zu erreichen. Ihr IT-Team kann die meisten Auslöser bei Verstößen mit den in der [Identitätsbaseline-Toolkette](toolchain.md) beschriebenen Tools automatisieren.

Die folgenden Auslöser und Durchsetzungsmaßnahmen sind Beispiele, auf die Sie bei der Planung der Verwendung von Überwachungsdaten zur Behebung von Richtlinienverstößen verweisen können:

- Verdächtige Aktivität erkannt: Benutzeranmeldungen, die von anonymen Proxy-IP-Adressen oder unbekannten Standorten erkannt werden, oder aufeinanderfolgende Anmeldungen von unmöglich entfernten geografischen Standorten können auf eine mögliche Kontoverletzung oder einen bösartigen Zugriffsversuch hinweisen. Die Anmeldung wird blockiert, bis die Benutzeridentität überprüft und das Kennwort zurückgesetzt werden kann.
- Kompromittierte Anmeldeinformationen von Benutzern: Konten, deren Benutzernamen und Kennwörter ins Internet gelangt sind, werden deaktiviert, bis die Benutzeridentität überprüft und das Kennwort zurückgesetzt werden kann.
- Unzureichende Zugriffssteuerung erkannt: Alle geschützten Objekte, bei denen die Zugriffsbeschränkungen nicht den Sicherheitsanforderungen entsprechen, werden so lange gesperrt, bis sie die Complianceanforderungen erfüllen.

## <a name="next-steps"></a>Nächste Schritte

Dokumentieren Sie mit der Vorlage [Cloudverwaltung](./template.md) die Prozesse und Auslöser, die sich an dem aktuellen Cloudeinführungsplan orientieren.

Anleitungen zur Ausführung von Cloudverwaltungsrichtlinien in Abstimmung mit Einführungsplänen finden Sie im Artikel über Verbesserungen von Disziplinen.

> [!div class="nextstepaction"]
> [Verbesserung der Disziplin „Identitätsbaseline“](./discipline-improvement.md)
