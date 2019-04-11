---
title: 'CAF: Leitfaden zur Entscheidungsfindung für die Richtlinienerzwingung'
titleSuffix: Microsoft Cloud Adoption Framework for Azure
ms.service: architecture-center
ms.subservice: enterprise-cloud-adoption
ms.custom: governance
ms.date: 02/11/2019
description: Informationen zu Abonnements für die Richtlinienerzwingung als zentrale Entwurfspriorität bei Azure-Migrationen.
author: rotycenh
ms.openlocfilehash: 8e1b447c36051da14231c4f93da463e6f7d96e76
ms.sourcegitcommit: c053e6edb429299a0ad9b327888d596c48859d4a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/20/2019
ms.locfileid: "58242621"
---
# <a name="policy-enforcement-decision-guide"></a>Leitfaden zur Entscheidungsfindung für die Richtlinienerzwingung

Die Definition von organisationsweiten Richtlinien ist nur wirksam, wenn eine Möglichkeit besteht, sie in Ihrer gesamten Organisation durchzusetzen. Ein wichtiger Aspekt bei der Planung einer Cloudmigration ist die Festlegung, wie die in der Cloudplattform enthaltenen Tools am besten mit Ihren vorhandenen IT-Prozessen kombiniert werden können, um die Richtlinienkonformität in der gesamten Cloudinfrastruktur zu maximieren.

![Abbildung der Optionen zur Richtlinienerzwingung mit zunehmender Komplexität entsprechend den nachstehenden weiterführenden Links](../../_images/discovery-guides/discovery-guide-policy-enforcement.png)

Wechseln Sie zu: [Grundlegende empfohlene Vorgehensweisen](#baseline-recommended-practices) | [Überwachung der Richtlinienkonformität](#policy-compliance-monitoring) | [Richtlinienerzwingung](#policy-enforcement) | [ Organisationsübergreifende Richtlinie](#cross-organization-policy) | [Automatisierte Durchsetzung](#automated-enforcement)

Mit zunehmender Größe Ihrer Cloudumgebung müssen Richtlinien für ein größeres Spektrum von Ressourcen und Abonnements verwaltet und erzwungen werden. Die Vergrößerung Ihrer Umgebung sowie die steigenden Richtlinienanforderungen Ihrer Organisation machen eine Erweiterung Ihrer Richtlinienerzwingungsprozesse erforderlich, um eine konsistente Einhaltung der Richtlinien sowie eine schnelle Erkennung von Verstößen zu gewährleisten.

Für kleinere Cloudumgebungen sind in der Regel die von der Plattform bereitgestellten Mechanismen für die Richtlinienerzwingung ausreichend. Größere Bereitstellungen rechtfertigen ein breiteres Erzwingungsspektrum und erfordern ggf. komplexere Erzwingungsmechanismen mit Bereitstellungsstandards und Ressourcengruppierung/-strukturierung sowie die Integration der Richtlinienerzwingung in Ihre Protokollierungs- und Berichterstellungssysteme.

Bei der Bestimmung des Umfangs Ihrer Richtlinienerzwingungsprozesse sind in erster Linie die [Cloud Governance-Anforderungen](/azure/architecture/cloud-adoption/governance/overview) Ihrer Organisation, die Größe und Art Ihrer Cloudumgebung sowie die Darstellung Ihrer Organisation in Ihrem [Abonnemententwurf](../subscriptions/overview.md) ausschlaggebend. Die Vergrößerung Ihrer Umgebung sowie der gestiegene Bedarf für eine zentrale Verwaltung der Richtlinienerzwingung können eine Erweiterung des Erzwingungsspektrums rechtfertigen.

## <a name="baseline-recommended-practices"></a>Grundlegende empfohlene Vorgehensweisen

Bei Einzelabonnements und einfachen Cloudbereitstellungen lassen sich viele Unternehmensrichtlinien mithilfe nativer Ressourcen- und Abonnementfeatures der Azure-Plattform erzwingen. Die konsistente Verwendung der Muster aus den [CAF-Leitfäden zur Entscheidungsfindung](../overview.md) trägt zur Etablierung einer grundlegenden Richtlinienkonformität ohne spezielle Investitionen in die Richtlinienerzwingung bei.

Beispiel: 

- Mit [Bereitstellungsvorlagen](../resource-consistency/overview.md) können Ressourcen mit standardisierter Struktur und Konfiguration zur Verfügung gestellt werden.
- Mit [Markierungs- und Benennungsstandards](../resource-tagging/overview.md) können Vorgänge organisiert und Abrechnungs- und Geschäftsanforderungen unterstützt werden.
- Einschränkungen für die Datenverkehrsverwaltung und das Netzwerk können über [Software-Defined Networking](../software-defined-network/overview.md) implementiert werden.
- Über die [rollenbasierte Zugriffssteuerung](../identity/overview.md) können Ihre Cloudressourcen gesichert und isoliert werden.

Überprüfen Sie bei Ihrer Planung der Erzwingung von Cloudrichtlinien zunächst, wie sich durch Anwendung der in diesen Leitfäden beschriebenen Standardmuster die in Ihrer Organisation geltenden Anforderungen erfüllen lassen.

## <a name="policy-compliance-monitoring"></a>Überwachung der Richtlinienkonformität

Ein erster Schritt, der über die einfache Nutzung der Richtlinienerzwingungsmechanismen der Azure-Plattform hinausgeht, besteht darin, die Überprüfung der Einhaltung von Organisationsrichtlinien für cloudbasierte Anwendungen und Dienste zu ermöglichen. Dies beinhaltet unter anderem die Implementierung von Benachrichtigungsfunktionen, um Verantwortliche auf nicht mehr konforme Ressourcen aufmerksam zu machen.  Die wirksame [Protokollierung und Berichterstellung](../log-and-report/overview.md) des Konformitätsstatus Ihrer Cloudworkloads ist ein wichtiger Bestandteil einer Unternehmensstrategie zur Richtlinienerzwingung.

Wenn Ihre Cloudinfrastruktur wächst, können Sie mithilfe zusätzlicher Tools, z.B. [Azure Security Center](/azure/security-center/), die integrierte Sicherheit und Bedrohungserkennung verwenden und die zentrale Richtlinienverwaltung sowie Warnungen für Ihre lokalen und Cloudressourcen nutzen.

## <a name="policy-enforcement"></a>Durchsetzung von Richtlinien

In Azure können Konfigurationseinstellungen und Ressourcenerstellungsregeln auf der Verwaltungsgruppen-, Abonnement- oder Ressourcengruppenebene angewendet werden, um die Richtlinieneinhaltung sicherzustellen.

[Azure Policy](/azure/governance/policy/overview) ist ein Azure-Dienst zum Erstellen, Zuweisen und Verwalten von Richtlinien. Mit diesen Richtlinien werden unterschiedliche Regeln und Auswirkungen für Ihre Ressourcen erzwungen, damit diese stets mit Ihren Unternehmensstandards und Vereinbarungen zum Servicelevel konform bleiben. Mit Azure Policy werden Ihre Ressourcen auf Nichteinhaltung der zugewiesenen Richtlinien überprüft. Angenommen, Sie möchten beispielsweise die SKU-Größe von virtuellen Computern in der Umgebung beschränken. Nachdem eine entsprechende Richtlinie implementiert wurde, wird die Konformität der neuen und vorhandenen Ressourcen dahin gehend geprüft. Mit der richtigen Richtlinie können vorhandene Ressourcen in Konformität gebracht werden.

## <a name="cross-organization-policy"></a>Organisationsübergreifende Richtlinie

Wenn Ihre Cloudumgebung wächst und viele Abonnements umfasst, die eine Richtlinienerzwingung erforderlich machen, benötigen Sie eine Erzwingungsstrategie für die gesamte Cloudumgebung, um die Richtlinienkonsistenz zu gewährleisten.

Die Richtlinie muss in Ihrem [Abonnemententwurf](../subscriptions/overview.md) berücksichtigt werden, da sie sich auf Ihre Organisationsstruktur bezieht. Zusätzlich zur Berücksichtigung der komplexen Organisation in Ihrem Abonnemententwurf können [Azure-Verwaltungsgruppen](../subscriptions/overview.md#management-groups) verwendet werden, um Azure Policy-Regeln in mehreren Abonnements zuzuweisen.

## <a name="automated-enforcement"></a>Automatisierte Erzwingung

Während standardisierte Bereitstellungsvorlagen in kleinerem Umfang wirksam sind, ermöglicht [Azure Blueprints](/azure/governance/blueprints/overview) eine umfassende standardisierte Bereitstellung und Bereitstellungsorchestrierung von Azure-Lösungen. Workloads in mehreren Abonnements können mit konsistenten Richtlinieneinstellungen für alle erstellten Ressourcen bereitgestellt werden.

Für IT-Umgebungen mit Integration von lokalen und Cloudressourcen müssen Sie möglicherweise Protokollierungs- und Berichterstellungssysteme verwenden, um Funktionen für die Hybridüberwachung bereitzustellen. Operative Überwachungssysteme von Drittanbietern oder benutzerdefinierte operative Überwachungssysteme umfassen möglicherweise weitere Funktionen zur Richtlinienerzwingung. Bei größeren oder schon länger bestehenden Cloudumgebungen sollten Sie sich Gedanken dazu machen, wie Sie diese Systeme am besten mit Ihren Cloudressourcen integrieren.

## <a name="next-steps"></a>Nächste Schritte

Erfahren Sie, wie Sie unter Verwendung der Ressourcenkonsistenz Cloudbereitstellungen zur Unterstützung des Abonnemententwurfs und der Governanceziele organisieren und standardisieren.

> [!div class="nextstepaction"]
> [Ressourcenkonsistenz](../resource-consistency/overview.md)
