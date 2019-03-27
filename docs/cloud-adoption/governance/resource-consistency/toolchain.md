---
title: 'CAF: Ressourcenkonsistenztools in Azure'
titleSuffix: Microsoft Cloud Adoption Framework for Azure
ms.service: architecture-center
ms.subservice: enterprise-cloud-adoption
ms.custom: governance
ms.date: 02/11/2019
description: Ressourcenkonsistenztools in Azure
author: BrianBlanchard
ms.openlocfilehash: 68503289f60fbb3682264ff39546ca7b7700cef5
ms.sourcegitcommit: c053e6edb429299a0ad9b327888d596c48859d4a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/20/2019
ms.locfileid: "58241581"
---
# <a name="resource-consistency-tools-in-azure"></a>Ressourcenkonsistenztools in Azure

[Ressourcenkonsistenz](overview.md) ist eine der [fünf Disziplinen der Cloud-Governance](../governance-disciplines.md). Dieser Disziplin konzentriert sich auf Möglichkeiten zum Einrichten von Richtlinien, die sich auf die Verwaltung des Betriebs einer Umgebung, Anwendung oder Workload beziehen. Innerhalb der fünf Disziplinen der Cloud-Governance umfasst die Ressourcenkonsistenz die Überwachung der Anwendungs-, Workload- und Ressourcenleistung. Darüber hinaus umfasst sie die Aufgaben, die zur Erfüllung von Skalierungsanforderungen erforderlich sind, sowie zur Korrektur von Leistungs-SLA-Verstößen und zur proaktiven Vermeidung von Leistungs-SLA-Verstößen durch automatisierte Korrektur.

Im Folgenden finden Sie eine Liste der Azure-Tools, die bei der Verfeinerung der Richtlinien und Prozesse beitragen können, die diese Governance-Disziplin unterstützen.

|    | [Azure-Portal](https://azure.microsoft.com/features/azure-portal/)  | [Azure Resource Manager](/azure/azure-resource-manager/resource-group-overview)  | [Azure Blueprint](/azure/governance/blueprints/overview) | [Azure Automation](/azure/automation/automation-intro) | [Azure AD](/azure/active-directory/fundamentals/active-directory-whatis) |
|---------|---------|---------|---------|---------|---------|
| Bereitstellen von Ressourcen                             | Ja | Ja | Ja | Ja | Nein   |
| Verwalten von Ressourcen                             | Ja | Ja | Ja | Ja | Nein   |
| Bereitstellen von Ressourcen mithilfe von Vorlagen             | Nein   | Ja | Nein  | Ja | Nein   |
| Orchestrierte Umgebungsbereitstellung          | Nein   | Nein   | Ja | Nein   | Nein   |
| Definieren von Ressourcengruppen                       | Ja | Ja | Ja | Nein   | Nein   |
| Verwalten von Workload- und Kontobesitzern           | Ja | Ja | Ja | Nein   | Nein   |
| Verwalten des bedingten Zugriffs auf Ressourcen       | Ja | Ja | Ja | Nein   | Nein   |
| Konfigurieren von Benutzern mit rollenbasierter Zugriffssteuerung (RBAC)                         | Ja | Nein   | Nein   | Nein   | Ja |
| Zuweisen von Rollen und Berechtigungen zu Ressourcen | Ja | Ja | Ja | Nein  | Ja |
| Definieren von Abhängigkeiten zwischen Ressourcen        | Nein   | Ja | Ja | Nein   | Nein   |
| Anwenden der Zugriffssteuerung                         | Ja | Ja | Ja | Nein  | Ja |
| Bewerten der Verfügbarkeit und Skalierbarkeit          | Nein   | Nein   | Nein   | Ja | Nein   |
| Anwenden von Tags auf Ressourcen                      | Ja | Ja | Ja | Nein   | Nein   |
| Zuweisen von Azure Policy-Regeln                    | Ja | Ja | Ja | Nein   | Nein   |
| Planen von Ressourcen für die Notfallwiederherstellung         | Ja | Ja | Ja | Nein   | Nein   |
| Anwenden der automatisierten Korrektur                  | Nein   | Nein   | Nein   | Ja | Nein   |
| Verwalten der Abrechnung                               | Ja | Nein   | Nein   | Nein   | Nein   |

Zusammen mit diesen Ressourcenkonsistenztools und -funktionen müssen Sie Ihre bereitgestellten Ressourcen auf Leistungs- und Integritätsprobleme überwachen. [Azure Monitor](/azure/azure-monitor/overview) ist die-Standardlösung für Überwachung und Berichterstellung in Azure. Azure Monitor bietet eine Reihe von einzelnen Funktionen, die Sie verwenden können, um Ihre Cloudressourcen zu überwachen, und die folgende Liste zeigt, welche Funktion es Ihnen erlaubt, gängige Überwachungsanforderungen zu erfüllen.

|                                                    | [Azure-Portal](https://azure.microsoft.com/features/azure-portal/) | [Application Insights](/azure/application-insights/app-insights-overview) | [Log Analytics](/azure/azure-monitor/log-query/log-query-overview) | [Azure Monitor-REST-API](/rest/api/monitor/) |
|----------------------------------------------------|--------------|----------------------|---------------|------------------------|
| Protokollieren von Telemetriedaten von VMs                 | Nein            | Nein                    | Ja           | Nein                      |
| Protokollieren von Telemetriedaten von virtuellen Netzwerken              | Nein            | Nein                    | Ja           | Nein                      |
| Protokollieren von Telemetriedaten von PaaS-Diensten                   | Nein            | Nein                    | Ja           | Nein                      |
| Protokoll von Telemetriedaten von Anwendungen                     | Nein            | Ja                  | Nein             | Nein                      |
| Konfigurieren von Berichten und Warnungen                       | Ja          | Nein                    | Nein             | Ja                    |
| Planen regelmäßiger Berichte oder benutzerdefinierter Analysen        | Nein            | Nein                    | Nein             | Nein                      |
| Visualisieren und Analysieren von Protokoll- und Leistungsdaten     | Ja          | Nein                    | Nein             | Nein                      |
| Integrieren in lokale oder Drittanbieterlösung für die Überwachung     | Nein            | Nein                    | Nein             | Ja                    |

Bei der Planung Ihrer Bereitstellung müssen Sie berücksichtigen, wo Protokolldaten gespeichert werden und wie Sie cloudbasierte [Berichts- und Überwachungsdienste](../../decision-guides/log-and-report/overview.md) in Ihre bestehenden Prozesse und Tools integrieren.

> [!NOTE]
> Organisationen verwenden auch DevOps-Tools von Drittanbietern, um Workloads und Ressourcen zu überwachen. Weitere Informationen finden Sie unter [Integrationen von DevOps-Tools](https://azure.microsoft.com/products/devops-tool-integrations/).

# <a name="next-steps"></a>Nächste Schritte

Erfahren Sie, wie Sie [Richtliniendefinitionen](/azure/governance/policy/) in Azure erstellen, zuweisen und verwalten.
