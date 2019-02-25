---
title: 'CAF: Verbesserung der Disziplin „Sicherheitsbaseline“'
titleSuffix: Microsoft Cloud Adoption Framework for Azure
ms.service: architecture-center
ms.subservice: enterprise-cloud-adoption
ms.custom: governance
ms.date: 02/11/2019
description: Verbesserung der Disziplin „Sicherheitsbaseline“
author: BrianBlanchard
ms.openlocfilehash: 28a971f56c9f8ada1d184bdc1cb3dbb9a17c3507
ms.sourcegitcommit: 273e690c0cfabbc3822089c7d8bc743ef41d2b6e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/08/2019
ms.locfileid: "55901901"
---
# <a name="security-baseline-discipline-improvement"></a>Verbesserung der Disziplin „Sicherheitsbaseline“

Die Disziplin „Sicherheitsbaseline“ konzentriert sich auf Möglichkeiten zum Festlegen von Richtlinien, die das Netzwerk, Ressourcen und vor allem die Daten schützen, die sich in der Lösung eines Cloudanbieters befinden werden. Innerhalb der fünf Disziplinen von Cloud Governance umfasst „Sicherheitsbaseline“ die Klassifizierung des digitalen Bestands und der Daten. Darüber hinaus sind die Dokumentation zu Risiken, Geschäftstoleranz und Lösungsstrategien in Zusammenhang mit der Sicherheit von Daten, Ressourcen und Netzwerk enthalten. Aus technischer Sicht umfasst dies auch die Beteiligung an Entscheidungen in Bezug auf [Verschlüsselung](../../decision-guides/encryption/overview.md), [Netzwerkanforderungen](../../decision-guides/software-defined-network/overview.md), [Hybrididentitätsstrategien](../../decision-guides/identity/overview.md) und [Prozesse](compliance-processes.md) zur Entwicklung von Richtlinien für die Cloudsicherheitsbaseline.

Dieser Artikel beschreibt einige potenzielle Aufgaben, die Ihr Unternehmen ausführen kann, um die Disziplin „Sicherheitsbaseline“ besser erstellen und weiterentwickeln zu können. Diese Aufgaben lassen sich in verschiedene Phasen der Implementierung einer Cloudlösung unterteilen: Planung, Erstellung, Übernahme und Betrieb. Diese Phasen werden dann durchlaufen und ermöglichen die Entwicklung eines [inkrementellen Ansatzes für Cloud Governance](../journeys/overview.md#an-incremental-approach-to-cloud-governance).

![Vier Phasen der Übernahme](../../_images/adoption-phases.png)

*Abbildung 1: Übernahmephasen des inkrementellen Ansatzes für Cloud Governance.*

Es ist unmöglich, die Anforderungen aller Unternehmen in einem einzigen Dokument zu berücksichtigen. Daher werden in diesem Artikel die empfohlenen mindestens auszuführenden Aktivitäten sowie Beispiele für potenzielle Aktivitäten für jede Phase des Weiterentwicklungsprozesses für die Governance beschrieben. Ziel dieser Aktivitäten ist, Sie beim Aufbau eines [Richtlinien-MVP](../journeys/overview.md#an-incremental-approach-to-cloud-governance) und bei der Einrichtung eines Frameworks für die inkrementelle Weiterentwicklung der Richtlinie zu unterstützen. Ihr Cloud Governance-Team muss entscheiden, wie viel in diese Aktivitäten investiert werden soll, um Ihre Funktionen für die Governance der Sicherheitsbaseline zu verbessern.

> [!CAUTION]
> Weder die in diesem Artikel beschriebenen mindestens erforderlichen noch die potenziellen Aktivitäten sind auf bestimmte Unternehmensrichtlinien oder Complianceanforderungen von Drittanbietern ausgerichtet. Diese Anleitung soll die Diskussionen erleichtern, die zur Anpassung beider Anforderungen an ein Cloud Governance-Modell beitragen.

## <a name="planning-and-readiness"></a>Planung und Bereitschaft

Diese Reifephase der Governance überbrückt die Lücke zwischen Geschäftsergebnissen und umsetzbaren Strategien. Während dieses Prozesses definiert das Führungsteam bestimmte Metriken, ordnet diese Metriken dem digitalen Bestand zu und beginnt mit der Planung der gesamten Migration.

**Mindestens empfohlene Aktivitäten**:

- Bewerten Ihrer Optionen für die [Sicherheitsbaseline-Toolkette](toolchain.md).
- Entwerfen Sie ein Dokument mit Architekturrichtlinien, und geben Sie dieses an die wichtigsten Beteiligten weiter.
- Schulen und beteiligen Sie die Personen und Teams, die von diesen Architekturrichtlinien betroffen sind.
- Fügen Sie priorisierte Sicherheitsaufgaben Ihrem Migrationsbacklog hinzu.

**Potenzielle Aktivitäten**:

- Definieren Sie ein Datenklassifizierungsschema.
- Führen Sie einen Planungsprozess für digitalen Bestand durch, um die aktuellen IT-Ressourcen zu inventarisieren, die Ihren Geschäftsprozessen und unterstützenden Vorgängen zugrunde liegen.
- Führen Sie eine [Richtlinienüberprüfung](../../governance/policy-compliance/what-is-a-cloud-policy-review.md) durch, um den Prozess der Modernisierung bestehender IT-Sicherheitsrichtlinien des Unternehmens einzuleiten, und definieren Sie MVP-Richtlinien für bekannte Risiken.
- Überprüfen Sie die Sicherheitsrichtlinien für Ihre Cloudplattform. Die entsprechenden Angaben für Azure finden Sie auf der [Microsoft Service Trust Platform](https://www.microsoft.com/trustcenter/stp/default.aspx).
- Bestimmen Sie, ob Ihre Richtlinie für die Sicherheitsbaseline einen [Security Development Lifecycle](https://www.microsoft.com/securityengineering/sdl/) beinhaltet.
- Bewerten Sie Geschäftsrisiken in Bezug auf Netzwerk, Daten und Ressourcen auf der Grundlage der nächsten ein bis drei Releases, und bemessen Sie die Toleranz Ihres Unternehmens gegenüber diesen Risiken.
- Lesen Sie den Bericht zu den [wichtigsten Trends bei der Cybersicherheit](https://www.microsoft.com/security/operations/security-intelligence-report) von Microsoft, um einen Überblick über die aktuelle Sicherheitslandschaft zu erhalten.
- Ziehen Sie die Entwicklung einer Rolle vom Typ [Sicherheits-DevOps](https://www.microsoft.com/en-us/securityengineering/devsecops) in Ihrer Organisation in Betracht.

<!-- "en-us" location is required for the URL above. -->

## <a name="build-and-pre-deployment"></a>Aufbau und Aufgaben vor der Bereitstellung

Für die erfolgreiche Migration einer Umgebung muss eine Reihe von technischen und nicht technischen Voraussetzungen erfüllt sein. Bei diesem Prozess geht es primär um die Entscheidungen, die Bereitschaft und die grundlegende Infrastruktur, die eine Migration vorantreiben.

**Mindestens empfohlene Aktivitäten**:

- Implementieren Sie Ihre [Sicherheitsbaseline-Toolkette](toolchain.md) in einer Phase vor der Bereitstellung.
- Aktualisieren Sie das Dokument mit Architekturrichtlinien, und geben Sie dieses an die wichtigsten Beteiligten weiter.
- Implementieren Sie Sicherheitsaufgaben in Ihrem priorisierten Migrationsbacklog.
- Entwickeln Sie Schulungsmaterialien und Dokumentation, Materialien zum Bekanntmachen der Migration, Incentives und weitere Programme, um die Akzeptanz durch Benutzer zu unterstützen.

**Potenzielle Aktivitäten**:

- Bestimmen Sie die [Verschlüsselungsstrategie](../../decision-guides/encryption/overview.md) Ihrer Organisation für in der Cloud gehostete Daten.
- Bewerten Sie die [Identitätsstrategie](../../decision-guides/identity/overview.md) Ihrer Cloudbereitstellung. Bestimmen Sie, wie Ihre cloudbasierte Identitätslösung mit lokalen Identitätsanbietern koexistieren oder in diese integriert werden kann.
- Bestimmen Sie Richtlinien für die Netzwerkgrenze für Ihren [Software Defined Networking (SDN)](../../decision-guides/software-defined-network/overview.md)-Entwurf, um sichere virtualisierte Netzwerkfunktionen zu gewährleisten.
- Bewerten Sie die Richtlinien für den [Zugriff mit den geringsten Rechten](/azure/active-directory/users-groups-roles/roles-delegate-by-task) Ihrer Organisation, und verwenden Sie aufgabenbasierte Rollen, um den Zugriff auf bestimmte Ressourcen zu ermöglichen.
- Wenden Sie Sicherheits- und Überwachungsmechanismen für alle Clouddienste und virtuellen Computer an.
- Automatisieren Sie [Sicherheitsrichtlinien](../../decision-guides/policy-enforcement/overview.md), wo dies möglich ist.
- Überprüfen Sie Ihre Richtlinie für die Sicherheitsbaseline, und bestimmen Sie, ob Sie Ihre Pläne gemäß den Anleitungen für Best Practices, wie sie z.B. im [Security Development Lifecycle](https://www.microsoft.com/securityengineering/sdl/) angegeben sind, ändern müssen.

## <a name="adopt-and-migrate"></a>Übernehmen und Migrieren

Migration ist ein inkrementeller Prozess, bei dem der Schwerpunkt auf der Verlagerung, dem Testen und der Übernahme von Anwendungen oder Workloads in einem vorhandenen digitalen Bestand liegt.

**Mindestens empfohlene Aktivitäten**:

- Migrieren Sie Ihre [Sicherheitsbaseline-Toolkette](toolchain.md) von der Phase vor der Bereitstellung in die Produktionsphase.
- Aktualisieren Sie das Dokument mit Architekturrichtlinien, und geben Sie dieses an die wichtigsten Beteiligten weiter.
- Entwickeln Sie Schulungsmaterialien und Dokumentation, Materialien zum Bekanntmachen der Migration, Incentives und weitere Programme, um die Akzeptanz durch Benutzer zu unterstützen.

**Potenzielle Aktivitäten**:

- Überprüfen Sie die neuesten Informationen zur Sicherheitsbaseline und Bedrohungen, um neue Geschäftsrisiken zu identifizieren.
- Bemessen Sie die Toleranz Ihrer Organisation gegenüber neuen Sicherheitsrisiken, die auftreten können.
- Identifizieren Sie Abweichungen von der Richtlinie, und erzwingen Sie Korrekturen.
- Passen Sie die Automatisierung der Sicherheit und Zugriffssteuerung an, um maximale Richtlinienkonformität zu gewährleisten.  
- Überprüfen Sie, ob die in der Erstellungsphase und der Phase vor der Bereitstellung definierten Best Practices ordnungsgemäß umgesetzt werden.
- Überprüfen Sie Ihre Richtlinien für den Zugriff mit den geringsten Rechten, und passen Sie Zugriffssteuerungen an, um die Sicherheit zu maximieren.
- Testen Sie Ihre Sicherheitsbaseline-Toolkette anhand Ihrer Workloads, um Sicherheitsrisiken zu identifizieren und zu beheben.

## <a name="operate-and-post-implementation"></a>Betrieb und Aufgaben nach der Implementierung

Sobald die Transformation abgeschlossen ist, müssen Governance und Betrieb während des natürlichen Lebenszyklus einer Anwendung oder Workload bestehen bleiben. In dieser Phase der Ausgereiftheit der Governance geht es um die Aktivitäten, die üblicherweise ausgeführt werden, nachdem die Lösung implementiert wurde und der Transformationszyklus sich zu stabilisieren beginnt.

**Mindestens empfohlene Aktivitäten**:

- Überprüfen und/oder optimieren Sie Ihre [Sicherheitsbaseline-Toolkette](toolchain.md).
- Passen Sie Benachrichtigungen und Berichte an, damit Sie bei potenziellen Sicherheitsproblemen gewarnt werden.
- Optimieren Sie die Architekturrichtlinien, um zukünftige Übernahmeprozesse zu unterstützen.
- Informieren und schulen Sie die betroffenen Teams regelmäßig, um die kontinuierliche Einhaltung der Architekturrichtlinien sicherzustellen.

**Potenzielle Aktivitäten**:

- Ermitteln Sie Muster und Verhalten für Ihre Workloads, und konfigurieren Sie Ihre Überwachungs- und Berichtstools, um ungewöhnliche Aktivitäten, Zugriffe oder Ressourcennutzung zu identifizieren und darüber zu informieren.
- Aktualisieren Sie kontinuierlich Ihre Richtlinien für die Überwachung und Berichterstellung, um die neuesten Sicherheitsrisiken, Exploits und Angriffe zu erkennen.
- Stellen Sie Verfahren bereit, um nicht autorisierten Zugriff schnell zu stoppen und Ressourcen zu deaktivieren, die möglicherweise durch einen Angreifer kompromittiert wurden.
- Überprüfen Sie regelmäßig die neuesten Best Practices für Sicherheit, und wenden Sie nach Möglichkeit Empfehlungen für Ihre Sicherheitsrichtlinie, Automatisierung und Überwachungsfunktionen an.

## <a name="next-steps"></a>Nächste Schritte

Nachdem Sie nun das Konzept von Cloud-Sicherheitsgovernance verstanden haben, erfahren Sie mehr darüber, [welche Anleitungen für Sicherheit und Best Practices von Microsoft](azure-security-guidance.md) für Azure bereitgestellt werden.

> [!div class="nextstepaction"]
> [Informationen zum Sicherheitsleitfaden für Azure](azure-security-guidance.md)
> [Einführung in Azure Security](/azure/security/azure-security)
> [Informationen zur Protokollierung, Berichterstellung und Überwachung](../../decision-guides/log-and-report/overview.md)
