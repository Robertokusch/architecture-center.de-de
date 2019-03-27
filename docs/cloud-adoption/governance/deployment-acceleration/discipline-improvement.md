---
title: 'Framework für die Cloudeinführung (Cloud Adoption Framework, CAF): Verbesserung der Disziplin „Beschleunigung der Bereitstellung“'
titleSuffix: Microsoft Cloud Adoption Framework for Azure
ms.service: architecture-center
ms.subservice: enterprise-cloud-adoption
ms.custom: governance
ms.date: 02/11/2019
description: Verbesserung der Disziplin „Beschleunigung der Bereitstellung“
author: alexbuckgit
ms.openlocfilehash: 98192c777d8866efb01544737e8cabea6354c4d7
ms.sourcegitcommit: c053e6edb429299a0ad9b327888d596c48859d4a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/20/2019
ms.locfileid: "58243721"
---
# <a name="deployment-acceleration-discipline-improvement"></a>Verbesserung der Disziplin „Beschleunigung der Bereitstellung“

Bei der Disziplin „Beschleunigung der Bereitstellung“ geht es darum, Richtlinien einzurichten, die sicherstellen, dass Ressourcen konsistent und wiederholbar bereitgestellt und konfiguriert werden und während des gesamten Lebenszyklus richtlinienkonform sind. Innerhalb der fünf Disziplinen der Cloudgovernance umfasst die Beschleunigung der Bereitstellung Entscheidungen zu Folgendem: Automatisierung von Bereitstellungen, Quellsteuerung von Bereitstellungsartefakten, Überwachung bereitgestellter Ressourcen zur Aufrechterhaltung des gewünschten Zustands und Überwachung von Complianceproblemen.

Dieser Artikel beschreibt einige potenzielle Aufgaben, die Ihr Unternehmen ausführen kann, um die Disziplin „Beschleunigung der Bereitstellung“ besser erstellen und weiterentwickeln zu können. Diese Aufgaben lassen sich in verschiedene Phasen der Implementierung einer Cloudlösung unterteilen: Planung, Erstellung, Einführung und Betrieb. Diese Phasen werden dann durchlaufen und ermöglichen die Entwicklung eines [inkrementellen Ansatzes für die Cloudgovernance](../journeys/overview.md#an-incremental-approach-to-cloud-governance).

![Vier Phasen der Einführung](../../_images/adoption-phases.png)

*Abbildung 1: Einführungsphasen des inkrementellen Ansatzes für die Cloudgovernance.*

Es ist unmöglich, die Anforderungen aller Unternehmen in einem einzigen Dokument zu berücksichtigen. Daher werden in diesem Artikel die empfohlenen mindestens auszuführenden Aktivitäten sowie Beispiele für potenzielle Aktivitäten für jede Phase des Weiterentwicklungsprozesses für die Governance beschrieben. Ziel dieser Aktivitäten ist es, Sie beim Aufbau eines [Richtlinien-MVP](../journeys/overview.md#an-incremental-approach-to-cloud-governance) und bei der Einrichtung eines Frameworks für die inkrementelle Weiterentwicklung der Richtlinie zu unterstützen. Ihr Cloudgovernance-Team muss entscheiden, wie viel in diese Aktivitäten investiert werden soll, um Ihre Funktionen für die Governance der Identitätsbaseline zu verbessern.

> [!CAUTION]
> Weder die in diesem Artikel beschriebenen mindestens erforderlichen noch die potenziellen Aktivitäten sind auf bestimmte Unternehmensrichtlinien oder Complianceanforderungen von Drittanbietern ausgerichtet. Dieser Leitfaden soll bei Gesprächen über die Ausrichtung beider Anforderungen auf ein Cloudgovernancemodell helfen.

## <a name="planning-and-readiness"></a>Planung und Bereitschaft

Diese Entwicklungsphase der Governance überbrückt die Lücke zwischen Geschäftsergebnissen und umsetzbaren Strategien. Während dieses Prozesses definiert das Führungsteam bestimmte Metriken, ordnet diese Metriken dem digitalen Bestand zu und beginnt mit der Planung der gesamten Migration.

**Mindestens empfohlene Aktivitäten**:

- Bewerten Sie die Optionen Ihrer [Toolkette für die Beschleunigung der Bereitstellung](toolchain.md), und implementieren Sie eine Hybridstrategie, die für Ihre Organisation geeignet ist.
- Entwerfen Sie ein Dokument mit Architekturrichtlinien, und geben Sie dieses an die wichtigsten Beteiligten weiter.
- Schulen und beteiligen Sie die Personen und Teams, die von diesen Architekturrichtlinien betroffen sind.
- Schulen Sie Entwicklungsteams und IT-Mitarbeiter, damit diese die DevSecOps-Prinzipien und -Strategien und die Bedeutung vollständig automatisierter Bereitstellungen in der Disziplin „Beschleunigung der Bereitstellung“ genau verstehen.

**Potenzielle Aktivitäten**:

- Definieren Sie Rollen und Zuweisungen, die die Beschleunigung der Bereitstellung in der Cloud regeln.

## <a name="build-and-pre-deployment"></a>Aufbau und Aufgaben vor der Bereitstellung

**Mindestens empfohlene Aktivitäten**:

- Führen Sie vollständig automatisierte Bereitstellungen bei neuen cloudbasierten Anwendungen frühzeitig im Entwicklungsprozess ein. Diese Investition verbessert die Zuverlässigkeit für Ihre Testprozesse und stellt die Konsistenz über all Ihre Entwicklungs-, Qualitätssicherungs- und Produktionsumgebungen hinweg sicher.
- Speichern Sie alle Bereitstellungsartefakte, z.B. Bereitstellungsvorlagen oder Konfigurationsskripts, auf einer Plattform für die Quellcodeverwaltung wie GitHub oder Azure DevOps.
- Ziehen Sie die Durchführung eines Pilottests in Betracht, bevor Sie Ihre [Toolkette für die Beschleunigung der Bereitstellung](toolchain.md) implementieren, um sicherzustellen, dass diese Ihre Bereitstellungen so weit wie möglich optimiert. Wenden Sie Feedback aus den Pilottests während der Phase vor der Bereitstellung an, und wiederholen Sie die Tests ggf.
- Bewerten Sie die logische und physische Architektur Ihrer Anwendungen, und identifizieren Sie Möglichkeiten, um die Bereitstellung von Anwendungsressourcen zu automatisieren oder Teile der Architektur mithilfe anderer cloudbasierter Ressourcen zu verbessern.
- Aktualisieren Sie das Dokument mit Architekturrichtlinien mit den Plänen für die Bereitstellung und die Akzeptanz durch Benutzer, und geben Sie dieses an die wichtigsten Beteiligten weiter.
- Setzen Sie die Schulung der Personen und Teams fort, die von den Architekturrichtlinien am meisten betroffen sind.

**Potenzielle Aktivitäten**:

- Definieren Sie eine CI/CD-Pipeline (Continuous Integration/Continuous Deployment), um die Freigabe von Updates für Ihre Anwendungen in Ihren Entwicklungs-, Qualitätssicherungs- und Produktionsumgebungen vollständig zu verwalten.

## <a name="adopt-and-migrate"></a>Einführen und Migrieren

Migration ist ein inkrementeller Prozess, bei dem der Schwerpunkt auf der Verlagerung, dem Testen und der Übernahme von Anwendungen oder Workloads in einem vorhandenen digitalen Bestand liegt.

**Mindestens empfohlene Aktivitäten**:

- Migrieren Sie Ihre [Toolkette für die Beschleunigung der Bereitstellung](toolchain.md) aus der Entwicklung in die Produktion.
- Aktualisieren Sie das Dokument mit Architekturrichtlinien, und geben Sie dieses an die wichtigsten Beteiligten weiter.
- Entwickeln Sie Schulungsmaterialien und Dokumentation, Materialien zum Bekanntmachen der Migration, Incentives und weitere Programme, um die Akzeptanz durch die Entwickler und die IT-Abteilung zu unterstützen.

**Potenzielle Aktivitäten**:

- Überprüfen Sie, ob die in der Erstellungsphase und der Phase vor der Bereitstellung definierten Best Practices ordnungsgemäß ausgeführt werden.
- Stellen Sie vor der Freigabe sicher, dass jede Anwendung oder Workload der Strategie für die Beschleunigung der Bereitstellung entspricht.

## <a name="operate-and-post-implementation"></a>Betrieb und Aufgaben nach der Implementierung

Nachdem die Transformation abgeschlossen ist, müssen Governance und Betrieb während des natürlichen Lebenszyklus einer Anwendung oder Workload bestehen bleiben. In dieser Entwicklungsphase der Governance geht es um die Aktivitäten, die üblicherweise ausgeführt werden, nachdem die Lösung implementiert wurde und der Transformationszyklus sich zu stabilisieren beginnt.

**Mindestens empfohlene Aktivitäten**:

- Passen Sie Ihre [Toolkette für die Beschleunigung der Bereitstellung](toolchain.md) an die Änderungen der Identitätsanforderungen Ihrer Organisation an.
- Automatisieren Sie Benachrichtigungen und Berichte, damit Sie bei potenziellen Konfigurationsproblemen oder Bedrohungen gewarnt werden.
- Überwachen Sie die Anwendungs- und Ressourcennutzung, und erstellen Sie entsprechende Berichte.
- Erstellen Sie Berichte zu Metriken nach der Bereitstellung, und geben Sie diese an die Beteiligten weiter.
- Überarbeiten Sie die Architekturrichtlinien, um zukünftige Einführungsprozesse zu unterstützen.
- Sorgen Sie für die regelmäßige Schulung und Kommunikation mit den betroffenen Personen und Teams, um die ununterbrochene Einhaltung der Architekturrichtlinien sicherzustellen.

**Potenzielle Aktivitäten**:

- Konfigurieren Sie ein Überwachungs- und Berichterstellungstool für die Konfiguration des gewünschten Zustands.
- Überprüfen Sie die Konfigurationstools und -skripts regelmäßig, um Prozesse zu verbessern und allgemeine Probleme zu identifizieren.
- Arbeiten Sie mit den Entwicklungs-, Betriebs- und Sicherheitsteams zusammen, um DevSecOps-Methoden weiterzuentwickeln und ineffiziente Organisationssilos aufzubrechen.

## <a name="next-steps"></a>Nächste Schritte

Sie kennen jetzt das Konzept der Identitätsgovernance in der Cloud. Als Nächstes sollten Sie die [Toolkette für die Identitätsbaseline](toolchain.md) untersuchen, um die Azure-Tools und -Features zu identifizieren, die Sie bei der Entwicklung der Governancedisziplin „Identitätsbaseline“ auf der Azure-Plattform benötigen.

> [!div class="nextstepaction"]
> [Identitätsbaseline-Toolkette für Azure](toolchain.md)
