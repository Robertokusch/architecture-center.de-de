---
title: 'CAF: Beschleunigung der Bereitstellung – Beispiele für Richtlinienanweisungen'
titleSuffix: Microsoft Cloud Adoption Framework for Azure
ms.service: architecture-center
ms.subservice: enterprise-cloud-adoption
ms.custom: governance
ms.date: 02/11/2019
description: Beschleunigung der Bereitstellung – Beispiele für Richtlinienanweisungen
author: alexbuckgit
ms.openlocfilehash: 4f7d59e6653c29db03f966d1c7105524b72586fc
ms.sourcegitcommit: 273e690c0cfabbc3822089c7d8bc743ef41d2b6e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/08/2019
ms.locfileid: "55901826"
---
# <a name="deployment-acceleration-sample-policy-statements"></a>Beschleunigung der Bereitstellung – Beispiele für Richtlinienanweisungen

Einzelne Cloudrichtlinienanweisungen sind Anleitungen für den Umgang mit bestimmten Risiken, die bei Ihrem Risikobewertungsprozess identifiziert wurden. Diese Anweisungen sollten eine präzise Zusammenfassung der Risiken sowie der Pläne für den Umgang mit diesen bereitstellen. Jede Anweisungsdefinition sollte diese Informationen enthalten:

- **Technisches Risiko**: Eine Zusammenfassung des Risikos, das diese Richtlinie behandelt.
- **Richtlinienanweisung**: Eine klare, zusammenfassende Erläuterung der Richtlinienanforderungen.
- **Entwurfsoptionen**: Direkt umsetzbare Empfehlungen, Spezifikationen oder andere Anleitungen, die IT-Teams und Entwickler bei der Implementierung der Richtlinie verwenden können.

Die folgenden Beispielrichtlinienanweisungen behandeln eine Reihe von allgemeinen Geschäftsrisiken im Zusammenhang mit der Konfiguration und sollen Ihnen als Beispiele dienen, auf die Sie sich beim Entwerfen von Richtlinienanweisungen beziehen können, um die Anforderungen Ihrer eigenen Organisation zu erfüllen. Beachten Sie, dass diese Beispiele nicht restriktiv gemeint sind, und dass es immer potenziell mehrere Richtlinienoptionen für den Umgang mit einem bestimmten Risiko gibt. Arbeiten Sie eng mit den Geschäfts- und IT-Teams zusammen, um die besten Richtlinienlösungen für Ihre spezielle Risikogruppe zu identifizieren.

## <a name="reliance-on-manual-deployment-or-configuration-of-systems"></a>Verwenden der manuellen Bereitstellung oder Konfiguration von Systemen

**Technisches Risiko**: Wenn Sie sich bei der Bereitstellung oder Konfiguration auf manuelle Eingriffe verlassen, steigt nicht nur die Fehlerwahrscheinlichkeit. Es leiden auch Wiederholbarkeit und Vorhersagbarkeit von Systembereitstellung und -konfiguration. In der Regel werden Systemressourcen auf diesem Weg auch langsamer bereitgestellt.

**Richtlinienanweisung**: Alle Ressourcen, die in der Cloud bereitgestellt werden, sollten nach Möglichkeit mithilfe von Vorlagen oder Automatisierungsskripts bereitgestellt werden.

**Potenzielle Entwurfsoptionen**: [Azure Resource Manager-Vorlagen](/azure/azure-resource-manager/resource-group-overview#template-deployment) stellen für die Bereitstellung von Ressourcen in Azure das Konzept „Infrastructure-as-Code“ bereit. Die [Azure-Bausteine](https://github.com/mspnp/template-building-blocks/wiki) enthalten ein Befehlszeilentool und eine Reihe von Resource Manager-Vorlagen, die die Bereitstellung von Azure-Ressourcen vereinfachen sollen.

## <a name="lack-of-visibility-into-system-issues"></a>Mangelnde Transparenz von Systemproblemen

**Technisches Risiko**: Aufgrund unzureichender Überwachungs- und Diagnosemöglichkeiten für Geschäftssysteme können die Mitarbeiter im Betrieb Probleme vor einem Systemausfall nicht identifizieren und beseitigen. Der Zeitaufwand für das ordnungsgemäße Beheben eines Ausfalls steigt damit erheblich.

**Richtlinienanweisung**: Die folgenden Richtlinien werden implementiert:

- Wichtige Metriken und Diagnosemaßnahmen werden für alle Produktionssysteme und Komponenten identifiziert. Überwachungs- und Diagnosetools werden auf diese Systeme angewendet und regelmäßig von Mitarbeitern im Betrieb überwacht.
- Der Betrieb kann auch den Einsatz von Überwachungs- und Diagnosetools in produktionsfernen Umgebungen wie Staging und Qualitätskontrolle erwägen, um Systemprobleme bereits zu identifizieren, bevor sie in der Produktionsumgebung auftreten.

**Potenzielle Entwurfsoptionen**: [Azure Monitor](/azure/azure-monitor/) mit Log Analytics und Application Insights stellt Tools zum Sammeln und Analysieren von Telemetriedaten bereit, damit Sie die Leistung Ihrer Anwendungen besser verstehen und proaktiv Probleme identifizieren können, die sich auf die Anwendungen und die zugehörigen Ressourcen auswirken.

## <a name="configuration-security-reviews"></a>Sicherheitsüberprüfungen der Konfiguration

**Technisches Risiko**: Im Laufe der Zeit kann sich durch neue Sicherheitsbedrohungen oder -risiken das Risiko eines unberechtigten Zugriffs auf sichere Ressourcen erhöhen.

**Richtlinienanweisung**: Cloud Governance-Prozesse müssen eine vierteljährliche Überprüfung durch Konfigurationsverwaltungsteams umfassen, um böswillige Akteure oder Nutzungsmuster zu identifizieren, die durch die Konfiguration der Cloudressourcen verhindert werden sollten.

**Potenzielle Entwurfsoptionen**: Etablieren Sie vierteljährliche Besprechungen zur Sicherheitsüberprüfung, an denen sowohl Mitglieder des Governance-Teams als auch IT-Mitarbeiter teilnehmen, die für die Konfiguration von Anwendungen und Ressourcen in der Cloud verantwortlich sind. Überprüfen Sie vorhandene Sicherheitsdaten und -metriken, um Lücken in der aktuellen Richtlinie für die Beschleunigung der Bereitstellung und bei den entsprechenden Tools zu definieren. Aktualisieren Sie dann die Richtlinie, um neue Risiken zu minimieren.

## <a name="next-steps"></a>Nächste Schritte

Verwenden Sie die in diesem Artikel erwähnten Beispiele als Ausgangspunkt für die Entwicklung von Richtlinien, die bestimmte Geschäftsrisiken behandeln, die Ihren Plänen für die Einführung der Cloud entsprechen.

Laden Sie die Vorlage [Identitätsbaseline](template.md) herunter, um mit der Entwicklung eigener, benutzerdefinierter Richtlinienanweisungen im Zusammenhang mit der Identitätsverwaltung zu beginnen.

Um die Einführung dieser Disziplin zu beschleunigen, wählen Sie [Nützliche Governance-Vorgehensweisen](../journeys/overview.md) aus, die die höchste Übereinstimmung mit Ihrer Umgebung haben. Ändern Sie dann den Entwurf, um Ihre speziellen Entscheidungen für Unternehmensrichtlinien zu integrieren.

> [!div class="nextstepaction"]
> [Nützliche Governance-Vorgehensweisen](../journeys/overview.md)