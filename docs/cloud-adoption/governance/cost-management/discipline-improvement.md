---
title: 'Framework für die Cloudeinführung (Cloud Adoption Framework, CAF): Verbesserung der Disziplin „Kostenmanagement“'
titleSuffix: Microsoft Cloud Adoption Framework for Azure
ms.service: architecture-center
ms.subservice: enterprise-cloud-adoption
ms.custom: governance
ms.date: 02/11/2019
description: Verbesserung der Disziplin „Kostenmanagement“
author: BrianBlanchard
ms.openlocfilehash: 34975d195a95b1a85ada96efe8c76a6138385ec1
ms.sourcegitcommit: 273e690c0cfabbc3822089c7d8bc743ef41d2b6e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/08/2019
ms.locfileid: "55902162"
---
# <a name="cost-management-discipline-improvement"></a>Verbesserung der Disziplin „Kostenmanagement“

Mit der Disziplin „Kostenmanagement“ wird versucht, die wichtigsten Geschäftsrisiken im Zusammenhang mit den Kosten für das Hosting von cloudbasierten Workloads anzugehen. Innerhalb der fünf Disziplinen der Cloudgovernance wird das Kostenmanagement für die Kontrolle der Kosten und Nutzung von Cloudressourcen eingesetzt, mit dem Ziel, einen geplanten Kostenzyklus zu erstellen und zu verwalten.

Dieser Artikel beschreibt die möglichen Aufgaben, die Ihr Unternehmen zum Erstellen und Weiterentwickeln Ihrer Kostenmanagement-Disziplin ausführen kann. Diese Aufgaben lassen sich in verschiedene Phasen der Implementierung einer Cloudlösung unterteilen: Planung, Erstellung, Einführung und Betrieb. Diese Phasen werden dann durchlaufen und ermöglichen die Entwicklung eines [inkrementellen Ansatzes für die Cloudgovernance](../journeys/overview.md#an-incremental-approach-to-cloud-governance).

![Vier Phasen der Einführung](../../_images/adoption-phases.png)

*Abbildung 1: Einführungsphasen des inkrementellen Ansatzes für die Cloudgovernance.*

Es ist unmöglich, die Anforderungen aller Unternehmen in einem einzigen Dokument zu berücksichtigen. Daher werden im vorliegenden Artikel die empfohlenen mindestens auszuführenden Aktivitäten sowie Beispiele für potenzielle Aktivitäten für jede Phase des Weiterentwicklungsprozesses für die Governance beschrieben. Ziel dieser Aktivitäten ist es, Sie beim Aufbau eines [Richtlinien-MVP](../journeys/overview.md#an-incremental-approach-to-cloud-governance) und bei der Einrichtung eines Frameworks für die inkrementelle Weiterentwicklung der Richtlinie zu unterstützen. Ihr Cloudgovernanceteam muss entscheiden, wie viel in diese Aktivitäten investiert werden soll, um Ihre Funktionen für die Governance des Kostenmanagements zu verbessern.

> [!CAUTION]
> Weder die in diesem Artikel beschriebenen mindestens erforderlichen noch die potenziellen Aktivitäten sind auf bestimmte Unternehmensrichtlinien oder Complianceanforderungen von Drittanbietern ausgerichtet. Dieser Leitfaden soll bei Gesprächen über die Ausrichtung beider Anforderungen auf ein Cloudgovernancemodell helfen.

## <a name="planning-and-readiness"></a>Planung und Bereitschaft

Diese Entwicklungsphase der Governance überbrückt die Lücke zwischen Geschäftsergebnissen und umsetzbaren Strategien. Während dieses Prozesses definiert das Führungsteam bestimmte Metriken, ordnet diese Metriken dem digitalen Bestand zu und beginnt mit der Planung der gesamten Migration.

**Mindestens empfohlene Aktivitäten**:

* Bewerten Sie Ihre Optionen für eine [Kostenmanagement-Toolkette](toolchain.md).
* Entwerfen Sie ein Dokument mit Architekturrichtlinien, und geben Sie dieses an die wichtigsten Beteiligten weiter.
* Schulen und beteiligen Sie die Personen und Teams, die von diesen Architekturrichtlinien betroffen sind.

**Potenzielle Aktivitäten**:

* Sorgen Sie für finanzielle Entscheidungen, die die geschäftliche Begründung für Ihre Cloudstrategie unterstützen.
* Überprüfen Sie Lernmetriken, die Sie für Berichte zur erfolgreichen Zuteilung der finanziellen Mittel verwenden.
* Informieren Sie sich genau über das gewünschte Abrechnungsmodell für die Cloud, das die Art und Weise der Kostenverteilung beeinflusst.
* Machen Sie sich mit dem Plan für den digitalen Bestand vertraut, und überprüfen Sie, ob die Kostenerwartungen korrekt sind.
* Bewerten Sie verschiedene Kaufoptionen, um zu ermitteln, ob eine nutzungsbasierte Bezahlung oder eine Vorabverpflichtung durch Erwerb eines Enterprise Agreement sinnvoller ist.
* Richten Sie geschäftliche Ziele an geplanten Budgets aus, und passen Sie ggf. Finanzierungspläne an.
* Entwickeln Sie einen Mechanismus zur Berichterstellung für Ziele und Budget, um technische und geschäftliche Beteiligte am Ende jedes Kostenzyklus zu benachrichtigen.

## <a name="build-and-pre-deployment"></a>Aufbau und Aufgaben vor der Bereitstellung

Für die erfolgreiche Migration einer Umgebung muss eine Reihe von technischen und nichttechnischen Voraussetzungen erfüllt werden. Bei diesem Prozess geht es primär um die Entscheidungen, die Bereitschaft und die grundlegende Infrastruktur, die eine Migration vorantreiben.

**Mindestens empfohlene Aktivitäten**:

* Implementieren Sie Ihre [Kostenmanagement-Toolkette](toolchain.md) in einer Phase vor der Bereitstellung.
* Aktualisieren Sie das Dokument mit Architekturrichtlinien, und geben Sie dieses an die wichtigsten Beteiligten weiter.
* Entwickeln Sie Schulungsmaterialien und Dokumentation, Materialien zum Bekanntmachen der Migration, Incentives und weitere Programme, um die Akzeptanz durch die Benutzer zu unterstützen.
* Ermitteln Sie, ob der Bedarf an zu erwerbenden Komponenten zu Ihrem Budget und Ihren Zielen passt.

**Potenzielle Aktivitäten**:

* Richten Sie Ihre Budgetpläne an der [Abonnementstrategie](../../decision-guides/subscriptions/overview.md) aus, die Ihr grundlegendes Besitzmodell definiert.
* Nutzen Sie Ihre [Strategie für die Konsistenz von Ressourcen](../../decision-guides/resource-consistency/overview.md), um Architektur- und Kostenrichtlinien im Lauf der Zeit durchzusetzen.
* Ermitteln Sie eventuelle Kostenanomalien, die sich auf Ihre Einführungs- und Migrationspläne auswirken können.

## <a name="adopt-and-migrate"></a>Einführen und Migrieren

Migration ist ein inkrementeller Prozess, bei dem der Schwerpunkt auf der Verlagerung, dem Testen und der Übernahme von Anwendungen oder Workloads in einem vorhandenen digitalen Bestand liegt.

**Mindestens empfohlene Aktivitäten**:

* Migrieren Sie Ihre [Kostenmanagement-Toolkette](toolchain.md) von der Phase vor der Bereitstellung in die Produktionsphase.
* Aktualisieren Sie das Dokument mit Architekturrichtlinien, und geben Sie dieses an die wichtigsten Beteiligten weiter.
* Entwickeln Sie Schulungsmaterialien und Dokumentation, Materialien zum Bekanntmachen der Migration, Incentives und weitere Programme, um die Akzeptanz durch die Benutzer zu unterstützen.

**Potenzielle Aktivitäten**:

* Implementieren Sie Ihr Cloudabrechnungsmodell.
* Stellen Sie sicher, dass Ihr Budget Ihre tatsächlichen Ausgaben bei jedem Release widerspiegelt, und passen Sie es nach Bedarf an.
* Überwachen Sie Änderungen in Budgetplänen, und besprechen Sie diese mit den Beteiligten, wenn zusätzliche Genehmigungen erforderlich sind.
* Aktualisieren Sie die Architekturrichtlinien mit den Änderungen, um die tatsächlichen Kosten widerzuspiegeln.

## <a name="operate-and-post-implementation"></a>Betrieb und Aufgaben nach der Implementierung

Sobald die Transformation abgeschlossen ist, müssen Governance und Betrieb während des natürlichen Lebenszyklus einer Anwendung oder Workload bestehen bleiben. In dieser Phase der Ausgereiftheit der Governance geht es um die Aktivitäten, die üblicherweise ausgeführt werden, nachdem die Lösung implementiert wurde und der Transformationszyklus beginnt, sich zu stabilisieren.

**Mindestens empfohlene Aktivitäten**:

* Passen Sie Ihre [Kostenmanagement-Toolkette](toolchain.md) an die Änderungen an den Kostenmanagementanforderungen Ihrer Organisation an.
* Erwägen Sie die Automatisierung von Benachrichtigungen und Berichten, um die tatsächlichen Ausgaben widerzuspiegeln.
* Optimieren Sie die Architekturrichtlinien, um zukünftige Einführungsprozesse zu unterstützen.
* Schulen Sie die betroffenen Teams regelmäßig, um die kontinuierliche Einhaltung der Architekturrichtlinien sicherzustellen.

**Potenzielle Aktivitäten**:

* Bewerten Sie das Cloudbusiness einmal im Vierteljahr, um einen Bericht über den Nutzen für das Geschäft und die zugehörigen Kosten zu erstellen.
* Passen Sie die Pläne vierteljährlich an, um Änderungen an den tatsächlichen Ausgaben widerzuspiegeln.
* Ermitteln Sie die finanzielle Ausrichtung an der Gewinn- und Verlustrechnung für Abonnements von Geschäftseinheiten.
* Analysieren Sie monatlich die Berichtsmethoden von Beteiligten für Nutzen und Kosten.
* Bewerten Sie nicht genutzte Assets, und bestimmen Sie, ob diese weiter im Bestand bleiben sollten.
* Ermitteln Sie falsche Ausrichtungen sowie Anomalien zwischen Plan und tatsächlichen Ausgaben.
* Unterstützen Sie die für die Cloudeinführung und Cloudstrategie zuständigen Teams, indem Sie diese Anomalien beseitigen.

## <a name="next-steps"></a>Nächste Schritte

Sie kennen jetzt das Konzept der Cloudidentitätsgovernance. Als Nächstes sollten Sie die [Kostenmanagement-Toolkette](toolchain.md) untersuchen, um die Azure-Tools und -Features zu identifizieren, die Sie bei der Entwicklung der Governancedisziplin „Kostenmanagement“ auf der Azure-Plattform benötigen.

> [!div class="nextstepaction"]
> [Kostenmanagement-Toolkette für Azure](toolchain.md)