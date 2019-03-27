---
title: 'CAF: Tools für die Beschleunigung der Bereitstellung in Azure'
description: Tools für die Beschleunigung der Bereitstellung in Azure
titleSuffix: Microsoft Cloud Adoption Framework for Azure
ms.service: architecture-center
ms.subservice: enterprise-cloud-adoption
ms.custom: governance
ms.date: 02/11/2019
author: BrianBlanchard
ms.openlocfilehash: cd00f2fa132f5f177ccc12f61be8b5342b71197d
ms.sourcegitcommit: c053e6edb429299a0ad9b327888d596c48859d4a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/20/2019
ms.locfileid: "58247421"
---
# <a name="deployment-acceleration-tools-in-azure"></a>Tools für die Beschleunigung der Bereitstellung in Azure

[Beschleunigung der Bereitstellung](overview.md) ist eine der [fünf Disziplinen der Cloud Governance](../governance-disciplines.md). Diese Disziplin konzentriert sich auf das Festlegen von Richtlinien, mit denen die Ressourcenkonfiguration bzw. -bereitstellung gesteuert wird. Innerhalb der fünf Cloud Governance-Disziplinen bezieht sich die Konfigurationsgovernance auf Bereitstellung, Konfigurationsanpassung und Strategien für die Hochverfügbarkeit und Notfallwiederherstellung. Zu diesem Zweck können manuelle Aktivitäten oder vollständig automatisierte DevOps-Aktivitäten zum Einsatz kommen. In beiden Fällen blieben die Richtlinien größtenteils gleich.

Cloudverwalter, -überwacher und -architekten mit Interesse an der Governance werden wahrscheinlich viel Zeit in die Disziplin „Beschleunigung der Bereitstellung“ investieren, da mit dieser Disziplin Richtlinien und Anforderungen für mehrere Cloudanpassungsaktivitäten festgeschrieben werden. Die Tools in dieser Toolkette sind wichtig für das Cloud Governance-Team und sollten im Lernpfad des Teams eine hohe Priorität einnehmen.

Im Folgenden ist eine Liste der Azure-Tools aufgeführt, die zur Ausreifung der Richtlinien und Prozesse, die diese Governance-Disziplin unterstützen, beitragen können.

|  | Azure Policy | Azure-Verwaltungsgruppen | Azure Resource Manager-Vorlagen | Azure Blueprint | Azure Resource Graph | Azure Cost Management |
|---------|---------|---------|---------|---------|---------|---------|
|Unternehmensrichtlinien implementieren     |Ja |Nein   |Nein   |Nein  | Nein  |Nein  |
|Richtlinien zwischen Abonnements anwenden     |Erforderlich |Ja  |Nein   |Nein  | Nein  |Nein  |
|Definierte Ressourcen bereitstellen     |Nein  |Nein   |Ja  |Nein  | Nein  |Nein  |
|Vollständig konforme Umgebungen erstellen      |Erforderlich |Erforderlich  |Erforderlich  |Ja | Nein  |Nein  |
|Richtlinien überprüfen      |Ja |Nein   |Nein   |Nein  | Nein  |Nein  |
|Azure-Ressourcen abfragen      |Nein  |Nein   |Nein   |Nein  |Ja |Nein  |
|Kosten von Ressourcen berichten      |Nein  |Nein   |Nein   |Nein  |Nein  |Ja |

Im Folgenden finden Sie zusätzliche Tools, die ggf. für die Umsetzung spezieller Ziele im Zusammenhang mit der Beschleunigung der Bereitstellung erforderlich sind. Häufig kommen diese Tools außerhalb des Governance-Teams zum Einsatz, werden aber immer noch als Aspekt der Disziplin „Beschleunigung der Bereitstellung“ betrachtet.

|  |Azure-Portal  |Azure Resource Manager-Vorlagen  |Azure Policy  | Azure DevOps | Azure Backup | Azure Site Recovery |
|---------|---------|---------|---------|---------|---------|---------|
|Manuelle Bereitstellung (einzelne Ressource)     | Ja | Ja  | Nein   | Nicht effizient | Nein  | Ja |
|Manuelle Bereitstellung (vollständige Umgebung)     | Nicht effizient | Ja | Nein   | Nicht effizient | Nein  | Ja |
|Automatische Bereitstellung (vollständige Umgebung)     | Nein   | Ja  | Nein  | Ja  | Nein | Ja |
|Konfiguration einzelner Ressource aktualisieren     | Ja | Ja | Nicht effizient | Nicht effizient | Nein  | Ja – während der Replikation |
|Konfiguration vollständiger Umgebung aktualisieren     | Nicht effizient | Ja | Ja | Ja  | Nein  | Ja – während der Replikation |
|Konfigurationsabweichung verwalten     | Nicht effizient | Nicht effizient | Ja  | Ja  | Nein  | Ja – während der Replikation |
|Automatische Pipeline zum Bereitstellen von Code und Konfigurieren von Ressourcen (DevOps) erstellen     | Nein  | Nein  | Nein  | Ja | Nein  | Nein  |
|Datenwiederherstellung bei Ausfall oder SLA-Verletzung     | Nein  | Nein  | Nein  | Ja | Ja | Ja |
|Anwendungs- und Datenwiederherstellung bei Ausfall oder SLA-Verletzung     | Nein  | Nein  | Nein  | Ja | Nein | Ja |

Abgesehen von den oben erwähnten nativen Azure-Tools verwenden Kunden häufig Tools von Drittanbietern, um die Beschleunigung der Bereitstellung und DevOps-Bereitstellungen zu vereinfachen.
