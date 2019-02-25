---
title: 'Framework für die Cloudeinführung (Cloud Adoption Framework, CAF): Leitfaden zur Entscheidungsfindung bei der Ressourcenkonsistenz'
titleSuffix: Microsoft Cloud Adoption Framework for Azure
ms.service: architecture-center
ms.subservice: enterprise-cloud-adoption
ms.custom: governance
ms.date: 02/11/2019
description: Erfahren Sie mehr zur Ressourcenkonsistenz bei der Planung von Azure-Migrationen.
author: rotycenh
ms.openlocfilehash: 8170bfd09218a451e086a57e0631b7e567eb2b82
ms.sourcegitcommit: 273e690c0cfabbc3822089c7d8bc743ef41d2b6e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/08/2019
ms.locfileid: "55901540"
---
# <a name="caf-resource-consistency-decision-guide"></a>Framework für die Cloudeinführung (Cloud Adoption Framework, CAF): Leitfaden zur Entscheidungsfindung bei der Ressourcenkonsistenz

Das [Azure-Abonnementmodell](../subscriptions/overview.md) bestimmt, wie Sie Ihre Cloudressourcen in Bezug auf die allgemeine Struktur Ihrer Organisation organisieren. Darüber hinaus hängt die Integration Ihrer bestehenden IT-Verwaltungsstandards und Ihrer Organisationsrichtlinien davon ab, wie Sie Cloudressourcen innerhalb eines Abonnements bereitstellen und organisieren.

Die verfügbaren Tools zur Umsetzung Ihrer Modells für die Bereitstellung, Gruppierung und Verwaltung von Ressourcen sind je nach Cloudplattform unterschiedlich. Allgemein bietet jede Lösung die folgenden Features:

- Einen logischen Gruppierungsmechanismus unterhalb der Abonnement- oder Kontoebene.
- Die Fähigkeit, Ressourcen mit APIs programmgesteuert bereitzustellen.
- Vorlagen für die Erstellung standardisierter Bereitstellungen.
- Die Möglichkeit zum Bereitstellen von Richtlinienregeln auf Abonnement-, Konto- und Ressourcengruppenebene.

![Abbildung der Konsistenzoptionen mit zunehmender Komplexität entsprechend den nachstehenden weiterführenden Links](../../_images/discovery-guides/discovery-guide-resource-consistency.png)

Wechseln Sie zu: [Grundlegende Gruppierung](#basic-grouping) | [Bereitstellungskonsistenz](#deployment-consistency) | [Richtlinienkonsistenz](#policy-consistency) | [Hierarchische Konsistenz](#hierarchical-consistency) | [Automatisierte Konsistenz](#automated-consistency)

Entscheidungen zur Bereitstellung und Gruppierung von Ressourcen werden in erster Linie von folgenden Faktoren beeinflusst: Größe des digitalen Umfelds nach der Migration, geschäftliche oder umgebungsbedingte Komplexität, die nicht genau in Ihr bestehendes Abonnementmodell passt, oder die Notwendigkeit, Governance im Laufe der Zeit nach der Bereitstellung von Ressourcen zu erzwingen. Komplexere Modelle für die Gruppierung von Ressourcen erfordern einen erhöhten Aufwand, um eine präzise Gruppierung zu gewährleisten, was zu einer Erhöhung des Zeitaufwands für Change Management und Nachverfolgung führt.

## <a name="basic-grouping"></a>Einfache Gruppierung

In Azure sind [Ressourcengruppen](/azure/azure-resource-manager/resource-group-overview#resource-groups) ein zentraler Mechanismus der Ressourcenorganisation, um Ressourcen innerhalb eines Abonnements logisch zu gruppieren.

Ressourcengruppen fungieren als Container für Ressourcen mit einem gemeinsamen Lebenszyklus oder gemeinsamen Verwaltungseinschränkungen wie Richtlinien oder Anforderungen an die rollenbasierte Zugriffssteuerung (RBAC). Ressourcengruppen dürfen nicht geschachtelt sein, und Ressourcen dürfen nur zu einer einzigen Ressourcengruppe gehören. Einige Aktionen gelten für alle Ressourcen in einer Ressourcengruppe. Durch das Löschen einer Ressourcengruppe werden beispielsweise alle Ressourcen aus dieser Gruppe entfernt. Zum Erstellen von Ressourcengruppen gibt es gängige Muster, die üblicherweise in zwei Kategorien aufgeteilt sind:

- Herkömmliche IT-Workloads: Werden meist nach Elementen innerhalb des gleichen Lebenszyklus gruppiert (z.B. Anwendung). Eine Gruppierung nach Anwendung ermöglicht die Verwaltung einzelner Anwendungen.
- Agile IT-Workloads: Konzentrieren sich auf Cloudanwendungen, die externen Kunden zugänglich sind. Diese Ressourcengruppen spiegeln häufig die Funktionsebenen der Bereitstellung (z.B. Webschicht, App-Schicht) und der Verwaltung wider.

## <a name="deployment-consistency"></a>Bereitstellungskonsistenz

Aufbauend auf dem grundlegenden Mechanismus zur Gruppierung von Ressourcen bieten die meisten Cloudplattformen ein System zur Verwendung von Vorlagen für die Bereitstellung Ihrer Ressourcen in der Cloudumgebung. Sie können Vorlagen verwenden, um konsistente Organisations- und Benennungskonventionen bei der Bereitstellung von Workloads zu erstellen und diese Aspekte der Ressourcenbereitstellung und des Verwaltungskonzepts zu erzwingen.

[Azure Resource Manager-Vorlagen](/azure/azure-resource-manager/resource-group-overview#template-deployment) ermöglichen Ihnen, Ihre Ressourcen unter Verwendung einer vorgegebenen Konfigurations- und Ressourcengruppenstruktur wiederholt in konsistentem Zustand bereitzustellen. Resource Manager-Vorlagen helfen Ihnen, eine Reihe von Standards als Grundlage für Ihre Bereitstellungen zu definieren.

Beispielsweise können Sie eine Standardvorlage für die Bereitstellung einer Webserver-Workload nutzen, die zwei virtuelle Computer als Webserver enthält, kombiniert mit einem Lastenausgleich zur Verarbeitung des Datenverkehrs zwischen den Servern. Sie können diese Vorlage anschließend wiederverwenden, um strukturell identische Bereitstellungen immer dann zu erstellen, wenn eine neue Webserver-Workload erforderlich ist, wobei nur der Bereitstellungsname und die zugehörigen IP-Adressen geändert werden.

Beachten Sie, dass Sie diese Vorlagen auch programmgesteuert bereitstellen und in Ihre CI-/CD-Systeme integrieren können.

## <a name="policy-consistency"></a>Richtlinienkonsistenz

Um sicherzustellen, dass Governancerichtlinien bei der Erstellung von Ressourcen befolgt werden, gehört es zum Konzept von Ressourcengruppen, beim Bereitstellen von Ressourcen eine gemeinsame Konfiguration zu verwenden.

Durch die Kombination aus Ressourcengruppen und standardisierten Resource Manager-Vorlagen können Sie Standards dahingehend erzwingen, welche Einstellungen in einer Bereitstellung erforderlich sind und welche [Azure-Richtlinienregeln](/azure/governance/policy/overview) für die einzelnen Ressourcengruppen oder Ressourcen gelten sollen.

So kann es beispielsweise erforderlich sein, dass alle virtuellen Computer, die in Ihrem Abonnement bereitgestellt werden, mit einem gemeinsamen Subnetz verbunden sind, das von Ihrem zentralen IT-Team verwaltet wird. Sie können eine Standardvorlage für die Bereitstellung von Workload-VMs erstellen, die eine separate Ressourcengruppe für die Workload erstellt und die erforderlichen VMs in dieser bereitstellt. Diese Ressourcengruppe weist eine Richtlinienregel auf, gemäß der nur Netzwerkschnittstellen innerhalb der Ressourcengruppe mit dem gemeinsamen Subnetz verbunden werden dürfen.

Eine ausführlichere Erläuterung der Erzwingung Ihrer Richtlinienentscheidungen innerhalb einer Cloudbereitstellung finden Sie unter [Richtlinienerzwingung](../policy-enforcement/overview.md).

## <a name="hierarchical-consistency"></a>Hierarchische Konsistenz

Wenn die Größe Ihres Cloudumfelds wächst, müssen Sie möglicherweise komplexere Governance-Anforderungen unterstützen, als dies mit der Unternehmens-, Abteilungs-, Konten- und Abonnementhierarchie des Azure Enterprise Agreement möglich ist. Ressourcengruppen ermöglichen Ihnen, zusätzliche Hierarchieebenen innerhalb Ihrer Organisation zu unterstützen, indem Sie Azure Policy-Regeln und Zugriffssteuerungen auf Ressourcengruppenebene anwenden.

[Azure-Verwaltungsgruppen](../subscriptions/overview.md#management-groups) können komplexere Organisationsstrukturen unterstützen, indem Ihre Enterprise Agreement-Struktur mit einer alternativen Hierarchie überlagert wird. Dadurch können Abonnements und die darin enthaltenen Ressourcen Zugriffssteuerungs- und Richtlinienerzwingungsmechanismen unterstützen, die so organisiert sind, dass sie den Anforderungen Ihrer Organisation entsprechen.

## <a name="automated-consistency"></a>Automatisierte Konsistenz

Bei großen Cloudbereitstellungen wird globale Governance immer wichtiger und komplexer. Es ist von entscheidender Bedeutung, auf Governance bezogene Vorgaben bei der Bereitstellung von Ressourcen automatisch um- und durchzusetzen sowie aktualisierte Anforderungen für bestehende Bereitstellungen zu erfüllen.

[Azure Blueprints](/azure/governance/blueprints/overview) ermöglichen Organisationen das Unterstützen globaler Governance in großen Cloudumgebungen in Azure. Diese Blaupausen gehen über die von den Standardvorlagen von Azure Resource Manager bereitgestellten Funktionen hinaus und dienen zum Erstellen kompletter Bereitstellungsorchestrierungen, die in der Lage sind, Ressourcen bereitzustellen und Richtlinienregeln zu befolgen. Die Blaupausen unterstützen die Versionsverwaltung, die Möglichkeit, Aktualisierungen für alle Abonnements vorzunehmen, in denen eine Blaupause verwendet wurde, und die Möglichkeit, bereitgestellte Abonnements zu sperren, um die unbefugte Erstellung und Änderung von Ressourcen zu verhindern.

Diese Bereitstellungspakete ermöglichen IT- und Entwicklungsteams, schnell neue Workloads und Netzwerkressourcen bereitzustellen, die den sich ändernden Anforderungen von Unternehmensrichtlinien entsprechen. Blaupausen können auch in CI-/CD-Pipelines integriert werden, um überarbeitete Governancestandards auf Bereitstellungen anzuwenden, sobald diese aktualisiert werden.

## <a name="next-steps"></a>Nächste Schritte

Erfahren Sie, wie die Ressourcenbenennung und -kategorisierung zur weiteren Organisation und Verwaltung Ihrer Cloudressourcen verwendet werden.

> [!div class="nextstepaction"]
> [Ressourcenbenennung und -kategorisierung](../resource-tagging/overview.md)
