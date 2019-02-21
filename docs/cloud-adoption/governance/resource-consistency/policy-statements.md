---
title: 'Framework für die Cloudeinführung (CAF): Beispiele für Richtlinienanweisungen der Ressourcenkonsistenz'
titleSuffix: Microsoft Cloud Adoption Framework for Azure
ms.service: architecture-center
ms.subservice: enterprise-cloud-adoption
ms.custom: governance
ms.date: 02/11/2019
description: Beispiele für Richtlinienanweisungen der Ressourcenkonsistenz
author: BrianBlanchard
ms.openlocfilehash: 9217ae73b0edf5b40bedac1cdc961b7a34da87d1
ms.sourcegitcommit: 273e690c0cfabbc3822089c7d8bc743ef41d2b6e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/08/2019
ms.locfileid: "55901470"
---
# <a name="resource-consistency-sample-policy-statements"></a>Beispiele für Richtlinienanweisungen der Ressourcenkonsistenz

Einzelne Cloudrichtlinienanweisungen sind Anleitungen für den Umgang mit bestimmten Risiken, die während Ihres Risikobewertungsprozesses identifiziert wurden. Diese Anweisungen sollten eine präzise Zusammenfassung der Risiken sowie der Pläne für den Umgang mit diesen bereitstellen. Jede Anweisungsdefinition sollte diese Informationen enthalten:

- Technisches Risiko: Eine Zusammenfassung des Risikos, das diese Richtlinie behandelt.
- Richtlinienanweisung: Eine klare, zusammenfassende Erläuterung der Anforderungen der Richtlinie.
- Entwurfsoptionen: Direkt umsetzbare Empfehlungen, Spezifikationen oder anderen Anleitungen, die IT-Teams und Entwickler bei der Implementierung der Richtlinie verwenden können.

Die folgenden Beispielrichtlinienanweisungen behandeln eine Reihe von allgemeinen Geschäftsrisiken im Zusammenhang mit der Ressourcenkonsistenz und sollen Ihnen als Beispiele dienen, auf die Sie sich beim Entwerfen von Richtlinienanweisungen beziehen können, um die Anforderungen Ihrer eigenen Organisation zu erfüllen. Beachten Sie, dass diese Beispiele nicht restriktiv gemeint sind, und dass es immer potenziell mehrere Richtlinienoptionen für den Umgang mit einem spezifischen Risiko gibt. Arbeiten Sie eng mit den Geschäfts- und IT-Teams zusammen, um die besten Richtlinienlösungen für Ihre einzigartige Gruppe von Risiken zu identifizieren.

## <a name="tagging"></a>Kennzeichnung (Tagging)

**Technisches Risiko**: Ohne die Zuordnung geeigneter Metadatentags zu den bereitgestellten Ressourcen kann der IT-Betrieb die Unterstützung oder Optimierung von Ressourcen auf Grundlage der erforderlichen Vereinbarung zum Servicelevel, der Wichtigkeit von Geschäftsvorgängen oder der Betriebskosten nicht priorisieren. Dies kann zur fehlerhaften Zuordnung von IT-Ressourcen und potenziellen Verzögerungen bei der Behebung von Vorfällen führen.

**Richtlinienanweisung**: Die folgenden Richtlinien werden implementiert:

- Bereitgestellte Ressourcen sollten mit den folgenden Werten gekennzeichnet werden: Kosten, Kritikalität, SLA und Umgebung.
- Mit Governancetools muss die Kennzeichnung (Tagging) im Hinblick auf Kosten, Kritikalität, SLA, Anwendung und Umgebung überprüft werden. Alle Werte müssen sich an vordefinierten Werten ausrichten, die vom Governance-Team verwaltet werden.

**Potenzielle Entwurfsoptionen**: In Azure werden für die meisten Ressourcentypen [Standardmetadatentags der Form „Name-Wert“](/azure/azure-resource-manager/resource-group-using-tags) unterstützt. [Azure Policy](/azure/governance/policy/overview) wird verwendet, um bestimmte Tags im Rahmen der Erstellung von Ressourcen zu erzwingen.

## <a name="ungoverned-subscriptions"></a>Ungeregelte Abonnements

**Technisches Risiko**: Das beliebige Erstellen von Abonnements und Verwaltungsgruppen kann zu isolierten Abschnitten Ihrer Cloudumgebung führen, die nicht ordnungsgemäß Ihren Governance-Richtlinien unterliegen.

**Richtlinienanweisung**: Für die Erstellung neuer Abonnements oder Verwaltungsgruppen für unternehmenskritische Anwendungen oder geschützte Daten ist eine Überprüfung durch das Cloud Governance-Team erforderlich. Genehmigte Änderungen werden in eine ordnungsgemäße Blaupausenzuweisung integriert.

**Potenzielle Entwurfsoptionen**: Schränken Sie den administrativen Zugriffs auf die [Azure-Verwaltungsgruppen](/azure/governance/management-groups/) Ihrer Organisation auf nur genehmigte Governance-Teammitglieder ein, die das Erstellen von Abonnements und den Zugriffssteuerungsprozess kontrollieren.

## <a name="manage-updates-to-virtual-machines"></a>Verwalten von Aktualisierungen für virtuelle Computer

**Technisches Risiko**: Virtuelle Computer (VMs), die nicht auf dem neuesten Stand hinsichtlich neuester Updates und Softwarepatches sind, sind anfällig für Sicherheits- oder Leistungsprobleme, die zu Dienstunterbrechungen führen können.

**Richtlinienanweisung**: Governancetools müssen erzwingen, dass automatische Updates für alle bereitgestellten VMs aktiviert sind. Verstöße müssen von Betriebsmanagementteams überprüft und in Übereinstimmung mit den Betriebsrichtlinien beseitigt werden. Ressourcen, die nicht automatisch aktualisiert werden, müssen in Prozesse des IT-Betriebs einbezogen werden.

**Potenzielle Entwurfsoptionen**: Für in Azure gehostete VMs können konsistente Updateverwaltung bereitstellen, indem Sie die [Updateverwaltungslösung in Azure Automation](/azure/automation/automation-update-management) verwenden.

## <a name="deployment-compliance"></a>Bereitstellungscompliance

**Technisches Risiko**: Bereitstellungsskripts und Automatisierungstools, die vom Cloud Governance-Team nicht vollständig geprüft wurden, können zu Ressourcenbereitstellungen führen, die gegen Richtlinien verstoßen.

**Richtlinienanweisung**: Die folgenden Richtlinien werden implementiert:

- Bereitstellungstools müssen vom Cloud Governance-Team genehmigt werden, um eine kontinuierliche Governance für bereitgestellte Ressourcen sicherzustellen.
- Bereitstellungsskripts müssen in einem zentralen Repository aufbewahrt werden, das für das Cloud Governance-Team zur regelmäßigen Überprüfung und Überwachung zugänglich ist.

**Potenzielle Entwurfsoptionen**: Die konsistente Verwendung von [Azure Blueprints](/azure/governance/blueprints/) zum Verwalten automatisierter Bereitstellungen ermöglicht konsistente Bereitstellungen von Azure-Ressourcen, die die Governance-Standards und -Richtlinien Ihrer Organisation einhalten.

## <a name="monitoring"></a>Überwachung

**Technisches Risiko**: Nicht ordnungsgemäß implementierte oder inkonsistent instrumentierte Überwachung kann die Erkennung von Integritätsproblemen bei Workloads oder anderen Verstößen gegen die Richtliniencompliance verhindern.

**Richtlinienanweisung**: Die folgenden Richtlinien werden implementiert:

- Mit den Governancetools muss überprüft werden, ob alle Ressourcen, die in einem Zusammenhang mit unternehmenskritischen Anwendungen oder geschützten Daten stehen, in die Überwachung auf Ressourcenschwund und -optimierung einbezogen sind.
- Ferner muss mit den Governancetools überprüft werden, ob für alle unternehmenskritischen Anwendungen oder geschützten Daten Protokolldaten mit dem passenden Protokolliergrad erfasst werden.

**Potenzielle Entwurfsoptionen**: [Azure Monitor](/azure/azure-monitor/overview) ist der Standardüberwachungsdienst der Azure-Plattform, und konsistente Überwachung kann durch die Verwendung von [Azure Blueprints](/azure/governance/blueprints/) beim Bereitstellen von Ressourcen erzwungen werden.

## <a name="disaster-recovery"></a>Notfallwiederherstellung

**Technisches Risiko**: Ressourcenfehler, -löschungen oder -beschädigungen können zu Unterbrechungen unternehmenskritischer Anwendungen oder Dienste sowie dem Verlust vertraulicher Daten führen.

**Richtlinienanweisung**: Für alle unternehmenskritischen Anwendungen und geschützten Daten müssen Sicherungs- und Wiederherstellungslösungen implementiert sein, um die geschäftlichen Auswirkungen von Ausfällen oder Systemfehlern zu minimieren.

**Potenzielle Entwurfsoptionen**: Der [Azure Site Recovery]-Dienst bietet Funktionen für Sicherung, Wiederherstellung und Replikation, die die Ausfalldauer in Business Continuity und Notfallwiederherstellungsszenarien (BCDR) minimieren sollen.

## <a name="next-steps"></a>Nächste Schritte

Verwenden Sie die in diesem Artikel erwähnten Beispiele als Ausgangspunkt für die Entwicklung von Richtlinien, die bestimmte geschäftliche Risiken behandeln, die Ihren Plänen für die Einführung der Cloud entsprechen.

Um mit der Entwicklung Ihrer eigenen, benutzerdefinierten Richtlinienanweisungen mit Bezug auf die Ressourcenkonsistenz zu beginnen, laden Sie die [Ressourcenkonsistenzvorlage](template.md) herunter.

Um die Einführung dieser Disziplin zu beschleunigen, wählen Sie [Nützliche Governance-Vorgehensweisen](../journeys/overview.md) aus, die die höchste Übereinstimmung mit Ihrer Umgebung haben. Ändern Sie dann das Design, um Ihre spezifischen Entscheidungen für Unternehmensrichtlinien zu integrieren.

> [!div class="nextstepaction"]
> [Nützliche Governance-Vorgehensweisen](../journeys/overview.md)
