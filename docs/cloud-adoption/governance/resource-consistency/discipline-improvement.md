---
title: 'CAF: Verbesserung der Disziplin „Ressourcenkonsistenz“'
titleSuffix: Microsoft Cloud Adoption Framework for Azure
ms.service: architecture-center
ms.subservice: enterprise-cloud-adoption
ms.custom: governance
ms.date: 02/11/2019
description: Verbesserung der Disziplin „Ressourcenkonsistenz“
author: BrianBlanchard
ms.openlocfilehash: bc81b894d46266c52291c53dba5532ab2ab6b860
ms.sourcegitcommit: c053e6edb429299a0ad9b327888d596c48859d4a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/20/2019
ms.locfileid: "58241571"
---
# <a name="resource-consistency-discipline-improvement"></a>Verbesserung der Disziplin „Ressourcenkonsistenz“

Die Disziplin „Ressourcenkonsistenz“ konzentriert sich auf Möglichkeiten zum Festlegen von Richtlinien, die sich auf die Verwaltung des Betriebs einer Umgebung, Anwendung oder Workload beziehen. Innerhalb der fünf Disziplinen der Cloudgovernance umfasst die Ressourcenkonsistenz die Überwachung der Anwendungs-, Workload- und Ressourcenleistung. Darüber hinaus umfasst sie die Aufgaben, die zur Erfüllung von Skalierungsanforderungen erforderlich sind, sowie zur Korrektur von Verstößen gegen die Vereinbarung zum Leistungsservicelevel (Service Level Agreement, SLA) und zur proaktiven Vermeidung von SLA-Verstößen durch automatisierte Wartung.

Dieser Artikel beschreibt einige potenzielle Aufgaben, die Ihr Unternehmen ausführen kann, um die Disziplin „Ressourcenkonsistenz“ besser erstellen und weiterentwickeln zu können. Diese Aufgaben lassen sich in verschiedene Phasen der Implementierung einer Cloudlösung unterteilen: Planung, Erstellung, Einführung und Betrieb. Diese Phasen werden dann durchlaufen und ermöglichen die Entwicklung eines [inkrementellen Ansatzes für die Cloudgovernance](../journeys/overview.md#an-incremental-approach-to-cloud-governance).

![Vier Phasen der Einführung](../../_images/adoption-phases.png)

*Abbildung 1: Einführungsphasen des inkrementellen Ansatzes für die Cloudgovernance.*

Es ist unmöglich, die Anforderungen aller Unternehmen in einem einzigen Dokument zu berücksichtigen. Daher werden in diesem Artikel die empfohlenen mindestens auszuführenden Aktivitäten sowie Beispiele für potenzielle Aktivitäten für jede Phase des Weiterentwicklungsprozesses für die Governance beschrieben. Ziel dieser Aktivitäten ist, Sie beim Aufbau eines [Richtlinien-MVP](../journeys/overview.md#an-incremental-approach-to-cloud-governance) und bei der Einrichtung eines Frameworks für die inkrementelle Weiterentwicklung der Richtlinie zu unterstützen. Ihr Cloudgovernanceteam muss entscheiden, wie viel in diese Aktivitäten investiert werden soll, um Ihre Funktionen für die Governance der Ressourcenkonsistenz zu verbessern.

> [!CAUTION]
> Weder die in diesem Artikel beschriebenen mindestens erforderlichen noch die potenziellen Aktivitäten sind auf bestimmte Unternehmensrichtlinien oder Complianceanforderungen von Drittanbietern ausgerichtet. Dieser Leitfaden soll bei Gesprächen über die Ausrichtung beider Anforderungen auf ein Cloudgovernancemodell helfen.

## <a name="planning-and-readiness"></a>Planung und Bereitschaft

Diese Entwicklungsphase der Governance überbrückt die Lücke zwischen Geschäftsergebnissen und umsetzbaren Strategien. Während dieses Prozesses definiert das Führungsteam bestimmte Metriken, ordnet diese Metriken dem digitalen Bestand zu und beginnt mit der Planung der gesamten Migration.

**Mindestens empfohlene Aktivitäten**:

* Werten Sie Ihre [Ressourcenkonsistenz-Toolkette](toolchain.md) aus.
* Lernen Sie die Lizenzanforderungen für Ihre Cloudstrategie kennen.
* Entwerfen Sie ein Dokument mit Architekturrichtlinien, und geben Sie dieses an die wichtigsten Beteiligten weiter.
* Machen Sie sich mit dem Ressourcen-Manager vertraut, den Sie zum Bereitstellen, Verwalten und Überwachen aller Ressourcen für Ihre Lösung als Gruppe verwenden.
* Schulen und beteiligen Sie die Personen und Teams, die von diesen Architekturrichtlinien betroffen sind.
* Fügen Sie priorisierte Ressourcenbereitstellungsaufgaben Ihrem Migrationsbacklog hinzu.

**Potenzielle Aktivitäten**:

* Arbeiten Sie mit den Geschäftsbeteiligten und/oder Ihrem Cloudstrategieteam zusammen, um den gewünschten Cloudabrechnungsansatz und die Kostenrechnungsmethoden in Ihren Geschäftseinheiten und Ihrer gesamten Organisation kennenzulernen.
* Definieren Sie Ihre Anforderungen zur [Überwachung und Durchsetzung von Richtlinien](compliance-processes.md).
* Überprüfen Sie den Geschäftswert sowie die Kosten für Ausfallzeiten, um Anforderungen für Wartungsrichtlinie und SLA zu definieren.
* Bestimmen Sie, ob Sie eine [einfache Workload](./governance-simple-workload.md)- oder [Multi-Team](./governance-multiple-teams.md)-Governancestrategie für Ihre Ressourcen bereitstellen.
* Bestimmen Sie die Skalierbarkeitsanforderungen für Ihre geplanten Workloads.

## <a name="build-and-pre-deployment"></a>Aufbau und Aufgaben vor der Bereitstellung

Für die erfolgreiche Migration einer Umgebung muss eine Reihe von technischen und nicht technischen Voraussetzungen erfüllt sein. Bei diesem Prozess geht es primär um die Entscheidungen, die Bereitschaft und die grundlegende Infrastruktur, die eine Migration vorantreiben.

**Mindestens empfohlene Aktivitäten**:

* Implementieren Sie Ihre [Ressourcenkonsistenz-Toolkette](toolchain.md) in einer Phase vor der Bereitstellung.
* Aktualisieren Sie das Dokument mit Architekturrichtlinien, und geben Sie dieses an die wichtigsten Beteiligten weiter.
* Implementieren Sie priorisierte Ressourcenbereitstellungsaufgaben in Ihrem priorisierten Migrationsbacklog.
* Entwickeln Sie Schulungsmaterialien und Dokumentation, Materialien zum Bekanntmachen der Migration, Incentives und weitere Programme, um die Akzeptanz durch die Benutzer zu unterstützen.

**Potenzielle Aktivitäten**:

* Entscheiden Sie sich für eine [Abonnemententwurfsstrategie](../../decision-guides/subscriptions/overview.md), und wählen Sie die Abonnementmuster aus, die am besten zu Ihren Organisations- und Workloadanforderungen passen.
* Nutzen Sie eine Strategie für die [Konsistenz von Ressourcen](../../decision-guides/resource-consistency/overview.md), um Architekturrichtlinien im Lauf der Zeit durchzusetzen.
* Implementieren Sie [Ressourcenbenennung und Markierungsstandards](../../decision-guides/resource-tagging/overview.md) für Ihre Ressourcen entsprechend Ihren Unternehmens- und Kostenabrechnungsanforderungen.
* Setzen Sie zum Erstellen proaktiver Point-in-Time-Governance Bereitstellungsvorlagen und Automatisierung ein, um bei der Bereitstellung von Ressourcen und Ressourcengruppen häufig verwendete Konfigurationen und eine konsistente Gruppierungsstruktur durchzusetzen.
* Legen Sie ein Berechtigungsmodell der geringsten Berechtigung fest, bei dem Benutzer standardmäßig nicht über Berechtigungen verfügen.
* Bestimmen Sie, wer in Ihrer Organisation welche Workload und welches Konto besitzt, und wer Zugriff zum Warten oder Ändern dieser Ressourcen benötigt. Definieren Sie Cloudrollen und -zuständigkeiten, die diesen Anforderungen entsprechen, und verwenden Sie diese Rollen als Grundlage für die Zugriffssteuerung.
* Definieren Sie Abhängigkeiten zwischen Ressourcen.
* Implementieren Sie automatisierte Ressourcenskalierung entsprechend der Anforderungen, die in der Planungsphase definiert wurden.
* Steuern Sie die Zugriffsleistung, um die Qualität der empfangenen Dienste zu messen.
* Erwägen Sie die Bereitstellung einer [Richtlinie](/azure/governance/policy/overview), um die SLA-Durchsetzung mithilfe von Konfigurationseinstellungen und Ressourcenerstellungsregeln zu verwalten.

## <a name="adopt-and-migrate"></a>Einführen und Migrieren

Migration ist ein inkrementeller Prozess, bei dem der Schwerpunkt auf der Verlagerung, dem Testen und der Übernahme von Anwendungen oder Workloads in einem vorhandenen digitalen Bestand liegt.

**Mindestens empfohlene Aktivitäten**:

* Migrieren Sie Ihre [Ressourcenkonsistenz-Toolkette](toolchain.md) von der Phase vor der Bereitstellung in die Produktionsphase.
* Aktualisieren Sie das Dokument mit Architekturrichtlinien, und geben Sie dieses an die wichtigsten Beteiligten weiter.
* Entwickeln Sie Schulungsmaterialien und Dokumentation, Materialien zum Bekanntmachen der Migration, Incentives und weitere Programme, um die Akzeptanz durch die Benutzer zu unterstützen.
* Migrieren Sie bestehende automatisierte Wartungskripte oder -tools zur Unterstützung definierter SLA-Anforderungen.

**Potenzielle Aktivitäten**:

* Vervollständigen und testen Sie Überwachungs- und Berichterstellungsdaten mit der lokalen, Cloudgateway- oder Hybridlösung Ihrer Wahl.
* Bestimmen Sie, ob Änderungen an der SLA oder Verwaltungsrichtlinie für Ressourcen vorgenommen werden müssen.
* Verbessern Sie operative Aufgaben durch Implementierung der Abfragefunktionen, um effizient Ressourcen in Ihrer Cloudumgebung zu suchen.
* Richten Sie Ressourcen an sich ändernden Geschäfts- und Governanceanforderungen aus.
* Stellen Sie sicher, dass Ihre virtuellen Computer, virtuellen Netzwerke und Speicherkonten bei jedem Release den eigentlichen Anforderungen an den Ressourcenzugriff entsprechen, und nehmen Sie notwendige Anpassungen vor.
* Stellen Sie sicher, dass die automatische Skalierung von Ressourcen den Zugriffsanforderungen entspricht.
* Überprüfen Sie den Benutzerzugriff auf Ressourcen, Ressourcengruppen und Azure-Abonnements, und passen Sie bei Bedarf Zugriffssteuerungen an.
* Überwachen Sie Änderungen in Ressourcenzugriffsplänen, und besprechen Sie diese mit den Beteiligten, wenn zusätzliche Genehmigungen erforderlich sind.
* Aktualisieren Sie die Architekturrichtlinien mit den Änderungen, um die tatsächlichen Kosten widerzuspiegeln.
* Bestimmen Sie, ob in Ihrer Organisation eine transparentere finanzielle Ausrichtung an G&V-Rechnungen für Geschäftseinheiten erforderlich ist.
* Implementieren Sie für globale Unternehmen Ihre Anforderungen an SLA-Konformität oder Datenhoheit.
* Stellen Sie für die Aggregation der Cloud eine Gatewaylösung zu einem Cloudanbieter bereit.
* Verknüpfen Sie für Tools, die keine Hybrid- oder Gatewayoptionen zulassen, die Überwachung eng mit einem operativen Überwachungstool.

## <a name="operate-and-post-implementation"></a>Betrieb und Aufgaben nach der Implementierung

Nachdem die Transformation abgeschlossen ist, müssen Governance und Betrieb während des natürlichen Lebenszyklus einer Anwendung oder Workload bestehen bleiben. In dieser Entwicklungsphase der Governance geht es um die Aktivitäten, die üblicherweise ausgeführt werden, nachdem die Lösung implementiert wurde und der Transformationszyklus sich zu stabilisieren beginnt.

**Mindestens empfohlene Aktivitäten**:

* Passen Sie Ihre [Ressourcenkonsistenz-Toolkette](toolchain.md) basierend auf Aktualisierungen der veränderten Anforderungen Ihres Unternehmens an das Kostenmanagement an.
* Erwägen Sie die Automatisierung von Benachrichtigungen und Berichten, um die tatsächliche Ressourcennutzung widerzuspiegeln.
* Optimieren Sie die Architekturrichtlinien, um zukünftige Einführungsprozesse zu unterstützen.
* Schulen Sie die betroffenen Teams regelmäßig, um die kontinuierliche Einhaltung der Architekturrichtlinien sicherzustellen.

**Potenzielle Aktivitäten**:

* Passen Sie die Pläne vierteljährlich an, um Änderungen an den tatsächlichen Ressourcen widerzuspiegeln.
* Wenden Sie bei zukünftigen Bereitstellungen Governanceanforderungen automatisch an, und setzen Sie sie durch.
* Werten Sie unterbeanspruchte Ressourcen aus, und bestimmen Sie, ob diese weiter im Bestand bleiben sollten.
* Ermitteln Sie falsche Ausrichtungen sowie Anomalien zwischen geplanter und tatsächlicher Ressourcennutzung.
* Unterstützen Sie die für die Cloudeinführung und Cloudstrategie zuständigen Teams, indem Sie diese Anomalien beseitigen.
* Bestimmen Sie, ob Änderungen an der Ressourcenkonsistenz für Abrechnung und SLAs erforderlich sind.
* Werten Sie Protokollierungs- und Überwachungstools aus, um festzustellen, ob Ihre lokale, Cloudgateway- oder Hybridlösung angepasst werden muss.
* Bestimmen Sie für Geschäftseinheiten und geografisch verteilte Gruppen, ob Ihre Organisation weitere Cloudverwaltungsfunktionen (z.B. [Azure-Verwaltungsgruppen](/azure/governance/management-groups/)) erwägen sollte, um besser die zentralisierte Richtlinie anzuwenden und SLA-Anforderungen zu erfüllen.

## <a name="next-steps"></a>Nächste Schritte

Nachdem Sie sich nun mit dem Konzept der Cloudressourcenkontrolle vertraut gemacht haben, können Sie sich über das [Verwalten des Ressourcenzugriffs](azure-resource-access.md) in Azure informieren. Dies ist die Vorbereitung auf das Entwerfen eines Governancemodells für eine [einzelne Workload](governance-simple-workload.md) oder für [mehrere Teams](governance-multiple-teams.md).

> [!div class="nextstepaction"]
> [Erfahren Sie mehr über den Zugriff auf Ressourcen in Azure](azure-resource-access.md)
> [Erfahren Sie mehr über SLAs für Azure](https://azure.microsoft.com/support/legal/sla/)
> [Erfahren Sie mehr über Protokollierung, Berichterstellung und Überwachung](../../decision-guides/log-and-report/overview.md)
