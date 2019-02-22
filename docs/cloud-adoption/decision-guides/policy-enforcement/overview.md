---
title: 'CAF: Leitfaden zur Entscheidungsfindung für die Richtlinienerzwingung'
titleSuffix: Microsoft Cloud Adoption Framework for Azure
ms.service: architecture-center
ms.subservice: enterprise-cloud-adoption
ms.custom: governance
ms.date: 02/11/2019
description: Informationen zu Abonnements für die Richtlinienerzwingung als zentrale Entwurfspriorität bei Azure-Migrationen.
author: rotycenh
ms.openlocfilehash: 372926453ee4ae0502250e9b69fe8a0ea94f0ffe
ms.sourcegitcommit: 273e690c0cfabbc3822089c7d8bc743ef41d2b6e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/08/2019
ms.locfileid: "55900743"
---
# <a name="policy-enforcement-decision-guide"></a>Leitfaden zur Entscheidungsfindung für die Richtlinienerzwingung

Die Definition von organisationsweiten Richtlinien ist nur wirksam, wenn eine Möglichkeit besteht, sie in Ihrer gesamten Organisation durchzusetzen. Ein wichtiger Aspekt bei der Planung einer Cloudmigration ist die Festlegung, wie die in der Cloudplattform enthaltenen Tools am besten mit Ihren vorhandenen IT-Prozessen kombiniert werden können, um die Richtlinienkonformität in der gesamten Cloudinfrastruktur zu maximieren.

![Abbildung der Optionen zur Richtlinienerzwingung mit zunehmender Komplexität entsprechend den nachstehenden weiterführenden Links](../../_images/discovery-guides/discovery-guide-policy-enforcement.png)

Wechseln Sie zu: [Grundlegende empfohlene Vorgehensweisen](#baseline-recommended-practices) | [Überwachung der Richtlinienkonformität](#policy-compliance-monitoring) | [Richtlinienerzwingung](#policy-enforcement) | [ Organisationsübergreifende Richtlinie](#cross-organization-policy) | [Automatisierte Durchsetzung](#automated-enforcement)

Wenn Ihre Cloudinfrastruktur wächst, sehen Sie sich mit der entsprechenden Notwendigkeit konfrontiert, Richtlinien in einem größeren Array von Ressourcen, Abonnements und Mandanten zu verwalten und zu erzwingen. Je größer die Infrastruktur, desto komplexer müssen die Durchsetzungsmechanismen sein, um eine konsequente Einhaltung der Richtlinien und eine schnelle Erkennung von Verstößen sicherzustellen. Über die Plattform bereitgestellte Mechanismen zur Richtlinienerzwingung auf Ressourcen- oder Abonnementebene sind normalerweise ausreichend für kleinere Cloudbereitstellungen, bei größeren Cloudbereitstellungen müssen dagegen möglicherweise komplexere Mechanismen genutzt werden, z.B. Bereitstellungsstandards, Gruppierung und Organisation von Ressourcen sowie Integration der Richtlinienerzwingung über Ihre Protokollierungs- und Berichterstellungssysteme.

Der entscheidende Punkt bei der Auswahl der Komplexität Ihrer Strategie zur Richtlinienerzwingung liegt in erster Linie in der Anzahl der Abonnements oder Mandanten, die in Ihrem [Abonnemententwurf](../subscriptions/overview.md) erforderlich sind. Auch der Umfang der für die verschiedenen Benutzerrollen in der Cloudinfrastruktur festgelegten Zugriffsrechte kann sich auf diese Entscheidungen auswirken.

## <a name="baseline-recommended-practices"></a>Grundlegende empfohlene Vorgehensweisen

Bei Bereitstellungen mit Einzelabonnements und einfachen Cloudbereitstellungen können viele Unternehmensrichtlinien mithilfe von Funktionen erzwungen werden, die in den meisten Cloudplattformen nativ sind. Selbst bei dieser relativ geringen Komplexität der Bereitstellung kann durch die konsequente Verwendung der in den [CAF-Leitfäden zur Entscheidungsfindung](../overview.md) erläuterten Muster ein Basisniveau der Richtlinienkonformität festgesetzt werden.

Beispiel: 

- Mit [Bereitstellungsvorlagen](../resource-consistency/overview.md) können Ressourcen mit standardisierter Struktur und Konfiguration zur Verfügung gestellt werden.
- Mit [Markierungs- und Benennungsstandards](../resource-tagging/overview.md) können Vorgänge organisiert und Abrechnungs- und Geschäftsanforderungen unterstützt werden.
- Einschränkungen für die Datenverkehrsverwaltung und das Netzwerk können über [Software-Defined Networking](../software-defined-network/overview.md) implementiert werden.
- Über die [rollenbasierte Zugriffssteuerung](../identity/overview.md) können Ihre Cloudressourcen gesichert und isoliert werden.

Überprüfen Sie bei Ihrer Planung der Erzwingung von Cloudrichtlinien zunächst, wie sich durch Anwendung der in diesen Leitfäden beschriebenen Standardmuster die in Ihrer Organisation geltenden Anforderungen erfüllen lassen.

## <a name="policy-compliance-monitoring"></a>Überwachung der Richtlinienkonformität

Ein weiterer wichtiger Faktor, selbst bei relativ kleinen Cloudbereitstellungen, ist die Möglichkeit der Überprüfung, ob cloudbasierte Anwendungen und Dienste mit der organisationsweiten Richtlinie konform sind, damit die Verantwortlichen umgehend benachrichtigt werden, wenn eine Ressource nicht mehr konform ist. Die wirksame [Protokollierung und Berichterstellung](../log-and-report/overview.md) des Konformitätsstatus Ihrer Cloudworkloads ist ein wichtiger Bestandteil einer Unternehmensstrategie zur Richtlinienerzwingung.

Wenn Ihre Cloudinfrastruktur wächst, können Sie mithilfe zusätzlicher Tools, z.B. [Azure Security Center](/azure/security-center/), die integrierte Sicherheit und Bedrohungserkennung verwenden und die zentrale Richtlinienverwaltung sowie Warnungen für Ihre lokalen und Cloudressourcen nutzen.

## <a name="policy-enforcement"></a>Durchsetzung von Richtlinien

Zudem können Sie durch Anwendung von Konfigurationseinstellungen und Ressourcenerstellungsregeln auf Abonnementebene die Richtlinienerzwingung sicherstellen.

[Azure Policy](/azure/governance/policy/overview) ist ein Azure-Dienst zum Erstellen, Zuweisen und Verwalten von Richtlinien. Mit diesen Richtlinien werden unterschiedliche Regeln und Auswirkungen für Ihre Ressourcen erzwungen, damit diese stets mit Ihren Unternehmensstandards und Vereinbarungen zum Servicelevel konform bleiben. Mit Azure Policy werden Ihre Ressourcen auf Nichteinhaltung der zugewiesenen Richtlinien überprüft. Angenommen, Sie möchten beispielsweise die SKU-Größe von virtuellen Computern in der Umgebung beschränken. Nachdem eine entsprechende Richtlinie implementiert wurde, wird die Konformität der neuen und vorhandenen Ressourcen dahin gehend geprüft. Mit der richtigen Richtlinie können vorhandene Ressourcen in Konformität gebracht werden.

## <a name="cross-organization-policy"></a>Organisationsübergreifende Richtlinie

Wenn Ihre Cloudinfrastruktur wächst und viele Abonnements umfasst, die eine Richtlinienerzwingung erforderlich machen, müssen Sie sich auf eine mandantenweite Durchsetzungsstrategie konzentrieren, um die Übereinstimmung mit der Richtlinie sicherzustellen.

Die Richtlinie muss in Ihrem [Abonnemententwurf](../subscriptions/overview.md) berücksichtigt werden, da sie sich auf Ihre Organisationsstruktur bezieht. Zusätzlich zur Berücksichtigung der komplexen Organisation in Ihrem Abonnemententwurf können [Azure-Verwaltungsgruppen](../subscriptions/overview.md#management-groups) verwendet werden, um Azure Policy-Regeln in mehreren Abonnements zuzuweisen.

## <a name="automated-enforcement"></a>Automatisierte Erzwingung

Während standardisierte Bereitstellungsvorlagen in kleinerem Umfang wirksam sind, ermöglicht [Azure Blueprints](/azure/governance/blueprints/overview) eine umfassende standardisierte Bereitstellung und Bereitstellungsorchestrierung von Azure-Lösungen. Workloads in mehreren Abonnements können mit konsistenten Richtlinieneinstellungen für alle erstellten Ressourcen bereitgestellt werden.

Für IT-Umgebungen mit Integration von lokalen und Cloudressourcen müssen Sie möglicherweise Protokollierungs- und Berichterstellungssysteme verwenden, um Funktionen für die Hybridüberwachung bereitzustellen. Operative Überwachungssysteme von Drittanbietern oder benutzerdefinierte operative Überwachungssysteme umfassen möglicherweise weitere Funktionen zur Richtlinienerzwingung. Bei komplexen Cloudinfrastrukturen sollten Sie überlegen, wie Sie diese Systeme am besten mit Ihren Cloudressourcen integrieren.

## <a name="next-steps"></a>Nächste Schritte

Erfahren Sie, wie Sie unter Verwendung der Ressourcenkonsistenz Cloudbereitstellungen zur Unterstützung des Abonnemententwurfs und der Governanceziele organisieren und standardisieren.

> [!div class="nextstepaction"]
> [Ressourcenkonsistenz](../resource-consistency/overview.md)
