---
title: 'CAF: Verbesserung der Disziplin „Identitätsbaseline“'
titleSuffix: Microsoft Cloud Adoption Framework for Azure
ms.service: architecture-center
ms.subservice: enterprise-cloud-adoption
ms.custom: governance
ms.date: 02/11/2019
description: Verbesserung der Disziplin „Identitätsbaseline“
author: BrianBlanchard
ms.openlocfilehash: c96a638af549782fec22b2068c9b4943df4b943a
ms.sourcegitcommit: 273e690c0cfabbc3822089c7d8bc743ef41d2b6e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/08/2019
ms.locfileid: "55900749"
---
# <a name="identity-baseline-discipline-improvement"></a>Verbesserung der Disziplin „Identitätsbaseline“

Die Disziplin „Identitätsbaseline“ konzentriert sich auf Möglichkeiten zur Erstellung von Richtlinien, die die Konsistenz und Kontinuität von Benutzeridentitäten gewährleisten, unabhängig davon, welcher Cloudanbieter die Anwendung oder Workload hostet. Innerhalb der fünf Disziplinen der Cloudgovernance umfasst die Identitätsbaseline Entscheidungen in Bezug auf die [Hybrididentitätsstrategie](../../decision-guides/identity/overview.md), die Auswertung und Erweiterung von Identitätsrepositorys, die Implementierung des einmaligen Anmeldens (gleiche Anmeldung), die Überprüfung und die Überwachung im Hinblick auf unbefugte Nutzung oder böswillige Akteure. In einigen Fällen kann sie auch Entscheidungen in Bezug auf Modernisierung, Konsolidierung oder Integration mehrerer Identitätsanbieter beinhalten.

Dieser Artikel beschreibt einige potenzielle Aufgaben, die Ihr Unternehmen ausführen kann, um die Disziplin „Identitätsbaseline“ besser erstellen und weiterentwickeln zu können. Diese Aufgaben lassen sich in verschiedene Phasen der Implementierung einer Cloudlösung unterteilen: Planung, Erstellung, Übernahme und Betrieb. Diese Phasen werden dann durchlaufen und ermöglichen die Entwicklung eines [inkrementellen Ansatzes für die Cloudgovernance](../journeys/overview.md#an-incremental-approach-to-cloud-governance).

![Vier Phasen der Einführung](../../_images/adoption-phases.png)

*Abbildung 1: Einführungsphasen des inkrementellen Ansatzes für die Cloudgovernance.*

Es ist unmöglich, die Anforderungen aller Unternehmen in einem einzigen Dokument zu berücksichtigen. Daher werden in diesem Artikel die empfohlenen mindestens auszuführenden Aktivitäten sowie Beispiele für potenzielle Aktivitäten für jede Phase des Weiterentwicklungsprozesses für die Governance beschrieben. Ziel dieser Aktivitäten ist es, Sie beim Aufbau eines [Richtlinien-MVP](../journeys/overview.md#an-incremental-approach-to-cloud-governance) und bei der Einrichtung eines Frameworks für die inkrementelle Weiterentwicklung der Richtlinie zu unterstützen. Ihr Cloudgovernanceteam muss entscheiden, wie viel in diese Aktivitäten investiert werden soll, um Ihre Funktionen für die Governance der Identitätsbaseline zu verbessern.

> [!CAUTION]
> Weder die in diesem Artikel beschriebenen mindestens erforderlichen noch die potenziellen Aktivitäten sind auf bestimmte Unternehmensrichtlinien oder Complianceanforderungen von Drittanbietern ausgerichtet. Diese Anleitung soll die Diskussionen erleichtern, die zur Ausrichtung beider Anforderungen mit einem Cloudgovernancemodell beitragen.

## <a name="planning-and-readiness"></a>Planung und Bereitschaft

Diese Entwicklungsphase der Governance überbrückt die Lücke zwischen Geschäftsergebnissen und umsetzbaren Strategien. Während dieses Prozesses definiert das Führungsteam bestimmte Metriken, ordnet diese Metriken dem digitalen Bestand zu und beginnt mit der Planung der gesamten Migration.

**Mindestens empfohlene Aktivitäten:**

* Bewerten Sie die Optionen Ihrer [Toolkette für die Identitätsbaseline](toolchain.md), und implementieren Sie eine Hybridstrategie, die für Ihre Organisation geeignet ist.
* Entwerfen Sie ein Dokument mit Architekturrichtlinien, und geben Sie dieses an die wichtigsten Beteiligten weiter.
* Schulen und beteiligen Sie die Personen und Teams, die von diesen Architekturrichtlinien betroffen sind.

**Potenzielle Aktivitäten:**

* Definieren Sie Rollen und Zuweisungen, die die Identitäts- und Zugriffsverwaltung in der Cloud steuern.
* Definieren Sie Ihre lokalen Gruppen, und ordnen Sie sie entsprechenden cloudbasierten Rollen zu.
* Inventarisieren Sie Identitätsanbieter (einschließlich datenbankgestützter Identitäten, die in benutzerdefinierten Anwendungen verwendet werden).
* Berücksichtigen Sie Optionen zur Konsolidierung oder Integration von Identitätsanbietern bei vorhandener Duplizierung, um die gesamte Identitätslösung zu vereinfachen.
* Bewerten Sie die Hybridkompatibilität vorhandener Identitätsanbieter.
* Prüfen Sie für Identitätsanbieter, die nicht hybridkompatibel sind, Konsolidierungs- oder Ersetzungsoptionen.

## <a name="build-and-pre-deployment"></a>Erstellung und Aufgaben vor der Bereitstellung

Für die erfolgreiche Migration einer Umgebung muss eine Reihe von technischen und nichttechnischen Voraussetzungen erfüllt werden. Bei diesem Prozess geht es primär um die Entscheidungen, die Bereitschaft und die grundlegende Infrastruktur, die eine Migration vorantreiben.

**Mindestens empfohlene Aktivitäten:**

* Ziehen Sie die Durchführung eines Pilottests in Betracht, bevor Sie Ihre [Toolkette für die Identitätsbaseline](toolchain.md) implementieren, um sicherzustellen, dass die Benutzerfreundlichkeit so weit wie möglich vereinfacht wird.
* Wenden Sie das Feedback aus Pilottests vor der Bereitstellung an. Wiederholen Sie die Tests, bis die Ergebnisse zufriedenstellend sind.
* Ergänzen Sie das Dokument mit Architekturrichtlinien um Pläne für die Bereitstellung und die Übernahme durch Benutzer, und geben Sie dieses an die wichtigsten Beteiligten weiter.
* Ziehen Sie die Einrichtung eines Early Adopter-Programms und die Einführung für eine begrenzte Anzahl von Benutzern in Betracht.
* Setzen Sie die Schulung der Personen und Teams fort, die von den Architekturrichtlinien am meisten betroffen sind.

**Potenzielle Aktivitäten:**

* Prüfen Sie die logische und physische Architektur, und legen Sie eine [Hybrididentitätsstrategie](../../decision-guides/identity/overview.md) fest.
* Ordnen Sie Richtlinien für die Identitäts- und Zugriffsverwaltung zu, z.B. Zuweisungen von Anmelde-IDs, und wählen Sie die geeignete Authentifizierungsmethode für Azure AD aus.
  * Aktivieren Sie bei einem vorhandenen Verbund Mandanteneinschränkungen für Administratorkonten.
* Integrieren Sie Ihre lokalen und Cloudverzeichnisse.
* Ziehen Sie die Verwendung der folgenden Zugriffsmodelle in Betracht:
  * [Zugriff mit den geringsten Rechten](/windows-server/identity/ad-ds/plan/security-best-practices/implementing-least-privilege-administrative-models)
  * [Privileged Identity Management](/azure/active-directory/privileged-identity-management/pim-configure)
* Legen Sie alle vor der Integration erforderlichen Details endgültig fest, und machen Sie sich mit den [bewährten Methoden für die Identitätsverwaltung](/azure/security/azure-security-identity-management-best-practices) vertraut.
  * Aktivieren der einzelnen Identität, des einmaligen Anmeldens (SSO) oder des nahtlosen einmaligen Anmeldens
  * Konfigurieren der mehrstufigen Authentifizierung (MFA) für Administratoren
  * Bei Bedarf Konsolidieren oder Integrieren von Identitätsanbietern
  * Implementieren der für die zentralisierte Verwaltung von Identitäten erforderlichen Tools
  * Aktivieren des Just-In-Time-Zugriffs (JIT) und der Warnungen für Rollenänderungen
  * Durchführen einer Risikoanalyse der wichtigsten Administratoraktivitäten für die Zuweisung zu integrierten Rollen
  * Eventuelle aktualisierte Einführung einer sichereren Authentifizierung für alle Benutzer
  * Aktivieren von Privileged Identity Management (PIM) für JIT-Zugriff (mit zeitlich begrenzter Aktivierung) für zusätzliche Administratorrollen
  * Trennen von Benutzerkonten und globalen Administratorkonten (um sicherzustellen, dass Administratoren nicht versehentlich E-Mails öffnen oder Programme ausführen, die mit ihren globalen Administratorkonten verknüpft sind)

## <a name="adopt-and-migrate"></a>Übernehmen und Migrieren

Die Migration ist ein inkrementeller Prozess, bei dem der Schwerpunkt auf der Verlagerung, dem Testen und der Übernahme von Anwendungen oder Workloads in einem vorhandenen digitalen Bestand liegt.

**Mindestens empfohlene Aktivitäten:**

* Migrieren Sie Ihre [Toolkette für die Identitätsbaseline](toolchain.md) aus der Entwicklung in die Produktion.
* Aktualisieren Sie das Dokument mit Architekturrichtlinien, und geben Sie dieses an die wichtigsten Beteiligten weiter.
* Entwickeln Sie Schulungsmaterialien und Dokumentation, Materialien zum Bekanntmachen der Migration, Incentives und weitere Programme, um die Akzeptanz durch Benutzer zu unterstützen.

**Potenzielle Aktivitäten:**

* Überprüfen Sie, ob die in der Erstellungsphase und der Phase vor der Bereitstellung definierten Best Practices ordnungsgemäß umgesetzt werden.
* Überprüfen und/oder optimieren Sie Ihre [Hybrididentitätsstrategie](../../decision-guides/identity/overview.md).
* Stellen Sie vor der Freigabe sicher, dass jede Anwendung oder Workload noch der Identitätsstrategie entspricht.
* Vergewissern Sie sich, dass das einmalige Anmelden (SSO) und das nahtlose einmalige Anmelden für Ihre Anwendungen wie erwartet funktionieren.
* Verringern oder entfernen Sie wenn möglich alternative Identitätsspeicher.
* Überprüfen Sie die Notwendigkeit von Identitätsspeichern in Apps oder Datenbanken. Identitäten, die außerhalb eines geeigneten Identitätsanbieters (von Erstanbietern oder Drittanbietern) liegen, können ein Risiko für die Anwendung und die Benutzer darstellen.
* Aktivieren Sie den bedingten Zugriff für [lokale Verbundanwendungen](/azure/active-directory/active-directory-device-registration-on-premises-setup).
* Verteilen Sie Identitäten auf globale Regionen in mehreren Hubs mit Synchronisierung zwischen den Regionen.
* Richten Sie einen Verbund mit zentraler rollenbasierter Zugriffssteuerung (RBAC) ein.

## <a name="operate-and-post-implementation"></a>Betrieb und Aufgaben nach der Implementierung

Nachdem die Transformation abgeschlossen ist, müssen Governance und Betrieb während des natürlichen Lebenszyklus einer Anwendung oder Workload bestehen bleiben. In dieser Entwicklungsphase der Governance geht es um die Aktivitäten, die üblicherweise ausgeführt werden, nachdem die Lösung implementiert wurde und der Transformationszyklus sich zu stabilisieren beginnt.

**Mindestens empfohlene Aktivitäten:**

* Passen Sie Ihre [Toolkette für die Identitätsbaseline](toolchain.md) an die Änderungen der Identitätsanforderungen Ihrer Organisation an.
* Automatisieren Sie Benachrichtigungen und Berichte, damit Sie bei potenziellen Bedrohungen gewarnt werden.
* Überwachen Sie den Fortschritt bei der Systemnutzung und Benutzerakzeptanz, und erstellen Sie entsprechende Berichte.
* Erstellen Sie Berichte zu Metriken nach der Bereitstellung, und geben Sie sie an die Beteiligten weiter.
* Optimieren Sie die Architekturrichtlinien, um zukünftige Übernahmeprozesse zu unterstützen.
* Informieren und schulen Sie die betroffenen Teams regelmäßig, um die kontinuierliche Einhaltung der Architekturrichtlinien sicherzustellen.

**Potenzielle Aktivitäten:**

* Führen Sie regelmäßige Überprüfungen der Identitätsrichtlinien und deren Einhaltung durch.
* Führen Sie regelmäßig Überprüfungen auf böswillige Akteure und Datenschutzverletzungen durch, insbesondere im Zusammenhang mit Identitätsbetrug, z.B. mögliche Übernahmen von Administratorkonten.
* Konfigurieren Sie ein Überwachungs- und Berichterstellungstool.
* Ziehen Sie eine engere Integration in Sicherheits- und Betrugspräventionssysteme in Betracht.
* Überprüfen Sie regelmäßig die Zugriffsrechte für Benutzer oder Rollen mit erhöhten Berechtigungen.
  * Identifizieren Sie jeden Benutzer, der zum Aktivieren von Administratorrechten berechtigt ist.
* Überprüfen Sie das Onboarding, das Offboarding und die Prozesse zum Aktualisieren von Anmeldeinformationen.
* Untersuchen Sie den zunehmenden Grad der Automatisierung und Kommunikation zwischen IAM-Modulen (Identity & Access Management).
* Ziehen Sie die Implementierung eines DevSecOps-Ansatzes (Development Security Operations) in Betracht.
* Führen Sie eine Auswirkungsanalyse durch, um die Ergebnisse bei Kosten, Sicherheit und Benutzerakzeptanz zu messen.
* Erstellen Sie in regelmäßigen Abständen Auswirkungsberichte, in denen die Änderungen bei vom System erstellten Metriken aufgezeigt und die Geschäftsauswirkungen der [Hybrididentitätsstrategie](../../decision-guides/identity/overview.md) abgeschätzt werden.
* Richten Sie die in [Azure Security Center](/azure/security-center/security-center-intro) empfohlene integrierte Überwachung ein.

## <a name="next-steps"></a>Nächste Schritte

Sie kennen jetzt das Konzept der Identitätsgovernance in der Cloud. Als Nächstes sollten Sie die [Toolkette für die Identitätsbaseline](toolchain.md) untersuchen, um die Azure-Tools und -Features zu identifizieren, die Sie bei der Entwicklung der Governancedisziplin „Identitätsbaseline“ auf der Azure-Plattform benötigen.

> [!div class="nextstepaction"]
> [Identitätsbaseline-Toolkette für Azure](toolchain.md)