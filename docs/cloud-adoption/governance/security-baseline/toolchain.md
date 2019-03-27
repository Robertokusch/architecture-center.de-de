---
title: 'CAF: Tools für Sicherheitsbaseline in Azure'
titleSuffix: Microsoft Cloud Adoption Framework for Azure
ms.service: architecture-center
ms.subservice: enterprise-cloud-adoption
ms.custom: governance
ms.date: 02/11/2019
description: Erläuterung der Tools zur Vereinfachung einer verbesserten Sicherheitsbaseline in Azure
author: BrianBlanchard
ms.openlocfilehash: b316626c8ad717514f7f592abefa0f33a92afdca
ms.sourcegitcommit: c053e6edb429299a0ad9b327888d596c48859d4a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/20/2019
ms.locfileid: "58243171"
---
# <a name="security-baseline-tools-in-azure"></a>Tools für Sicherheitsbaseline in Azure

[Sicherheitsbaseline](overview.md) ist eine der [fünf Disziplinen von Cloud Governance](../governance-disciplines.md). Diese Disziplin konzentriert sich auf Möglichkeiten zum Festlegen von Richtlinien, die das Netzwerk, Ressourcen und vor allem die Daten schützen, die sich in der Lösung eines Cloudanbieters befinden werden. Innerhalb der fünf Disziplinen von Cloud Governance umfasst „Sicherheitsbaseline“ die Klassifizierung des digitalen Bestands und der Daten. Darüber hinaus sind die Dokumentation zu Risiken, Geschäftstoleranz und Lösungsstrategien in Zusammenhang mit der Sicherheit von Daten, Ressourcen und Netzwerk enthalten. Aus technischer Sicht umfasst dies auch die Beteiligung an Entscheidungen in Bezug auf [Verschlüsselung](../../decision-guides/encryption/overview.md), [Netzwerkanforderungen](../../decision-guides/software-defined-network/overview.md), [Hybrididentitätsstrategien](../../decision-guides/identity/overview.md) und Tools für [automatisiertes Erzwingen](../../decision-guides/policy-enforcement/overview.md) von Sicherheitsrichtlinien für alle [Ressourcengruppen](../../decision-guides/resource-consistency/overview.md).

Es folgt eine Liste von Azure-Tools, die zu ausgereiften Richtlinien und Prozessen beitragen können, die Sicherheitsbaseline unterstützen.

|                                                            | [Azure-Portal](https://azure.microsoft.com/features/azure-portal/) / [Resource Manager](/azure/azure-resource-manager/resource-group-overview)  | [Azure Key Vault](/azure/key-vault)  | [Azure AD](/azure/active-directory/fundamentals/active-directory-whatis) | [Azure Policy](/azure/governance/policy/overview) | [Azure Security Center](/azure/security-center/security-center-intro) | [Azure Monitor](/azure/azure-monitor/overview) |
|------------------------------------------------------------|---------------------------------|-----------------|----------|--------------|-----------------------|---------------|
| Anwenden von Zugriffssteuerungen auf Ressourcen und Ressourcenerstellung   | Ja                             | Nein              | Ja      | Nein            | Nein                     | Nein             |
| Sichere virtuelle Netzwerke                                    | Ja                             | Nein               | Nein        | Ja          | Nein                     | Nein             |
| Verschlüsseln virtueller Laufwerke                                     | Nein                               | Ja             | Nein        | Nein            | Nein                     | Nein             |
| Verschlüsseln von PaaS-Speicher und -Datenbanken                         | Nein                               | Ja             | Nein        | Nein            | Nein                     | Nein             |
| Verwalten von Hybrididentitätsdiensten                            | Nein                               | Nein               | Ja      | Nein            | Nein                     | Nein             |
| Beschränken zulässiger Ressourcentypen                         | Nein                               | Nein               | Nein        | Ja          | Nein                     | Nein             |
| Erzwingen georegionaler Einschränkungen                          | Nein                               | Nein               | Nein        | Ja          | Nein                     | Nein             |
| Überwachen der Sicherheitsintegrität von Netzwerken und Ressourcen          | Nein                               | Nein               | Nein        | Nein            | Ja                   | Ja           |
| Erkennen schädlicher Aktivität                                  | Nein                               | Nein               | Nein        | Nein            | Ja                   | Ja           |
| Präventives Erkennen von Sicherheitsrisiken                        | Nein                               | Nein               | Nein        | Nein            | Ja                   | Nein             |
| Konfigurieren von Sicherung und Notfallwiederherstellung                     | Ja                             | Nein               | Nein        | Nein            | Nein                     | Nein             |

Eine vollständige Liste der Azure-Sicherheitstools und -dienste finden Sie unter [Bei Azure verfügbare Sicherheitsdienste und -technologien](/azure/security/azure-security-services-technologies).

Es kommt auch sehr häufig vor, dass Kunden Drittanbietertools zur Vereinfachung von Aktivitäten für Sicherheitsbaseline verwenden. Weitere Informationen finden Sie im Artikel [Integrieren von Sicherheitslösungen in Azure Security Center](/azure/security-center/security-center-partner-integration).

Außer den Sicherheitstools enthält das [Microsoft Trust Center](https://www.microsoft.com/trustcenter/guidance/risk-assessment) umfassende Anweisungen, Berichte und zugehörige Dokumentation, mit denen Sie die Risikobewertungen im Rahmen Ihres Migrationsplanungsprozesses ausführen können.
