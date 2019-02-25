---
title: 'Framework für die Cloudeinführung (Cloud Adoption Framework, CAF): Leitfaden zur architekturbezogenen Entscheidungsfindung'
titleSuffix: Microsoft Cloud Adoption Framework for Azure
ms.service: architecture-center
ms.subservice: enterprise-cloud-adoption
ms.custom: governance
ms.date: 02/11/2019
description: Lernen Sie die Leitfäden zur architekturbezogenen Entscheidungsfindung kennen.
author: rotycenh
ms.openlocfilehash: b43047b5fc5b636bc84b9f28ec24846730e63672
ms.sourcegitcommit: 273e690c0cfabbc3822089c7d8bc743ef41d2b6e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/08/2019
ms.locfileid: "55901957"
---
# <a name="architectural-decision-guides"></a>Leitfaden zur architekturbezogenen Entscheidungsfindung

In den Leitfäden zur architekturbezogenen Entscheidungsfindung im Framework für die Cloudeinführung werden Muster und Modelle für das Erstellen von Entwurfsanleitungen für Cloud Governance beschrieben. Jeder Leitfaden zur Entscheidungsfindung konzentriert sich auf eine zentrale Infrastrukturkomponente von Cloudbereitstellungen und listet potenzielle Muster oder Modelle auf, die zur Unterstützung bestimmter Cloudbereitstellungsszenarien dienen.

Wenn Sie beginnen, Cloud Governance in Ihrer Organisation zu etablieren, liefern umsetzbare Governance-Anweisungen einen grundlegenden Fahrplan. Diese Anleitungen enthalten jedoch Annahmen über Anforderungen und Prioritäten, die möglicherweise nicht mit denen Ihrer Organisation übereinstimmen.
Diese Leitfäden zur Entscheidungsfindung ergänzen die exemplarischen Anleitungen zur Governance, indem sie alternative Muster und Modelle bereitstellen, die Ihnen helfen, die in der exemplarischen Entwurfsanleitung getroffenen Entscheidungen zum Architekturentwurf an Ihre eigenen Anforderungen anzupassen.

## <a name="design-guidance-categories"></a>Kategorien von Entwurfsleitfäden

Jede der folgenden Kategorien stellt eine grundlegende Technologie aller Cloudbereitstellungen dar. Untersuchen Sie für jede Option in den beispielhaften Entwurfsanleitungen, die nicht Ihren Anforderungen entspricht, die Optionen im entsprechenden nachstehenden Abschnitt, um ein Muster oder Modell auszuwählen, das Ihren Erfordernissen besser entspricht.

[Abonnements](./subscriptions/overview.md): Planen Sie das Abonnementmodell und die Kontenstruktur Ihrer Cloudbereitstellung entsprechend den Besitz-, Abrechnungs- und Verwaltungskonzepten Ihrer Organisation.

[Identität](./identity/overview.md): Integrieren Sie cloudbasierte Identitätsdienste in Ihre vorhandenen Identitätsressourcen, um die Zugriffssteuerung in Ihrer IT-Umgebung zu unterstützen.

[Richtlinienerzwingung](./policy-enforcement/overview.md): Definieren und erzwingen Sie unternehmensweit geltende Regeln für Ressourcen und Workloads, die Sie in der Cloud bereitstellen.

[Ressourcenkonsistenz](./resource-consistency/overview.md): Stellen Sie sicher, dass die Bereitstellung und Organisation Ihrer cloudbasierten Ressourcen auf die Erfüllung von Ressourcenverwaltungs- und Richtlinienanforderungen ausgerichtet ist.

[Kategorisierung von Ressourcen](./resource-tagging/overview.md): Organisieren Sie Ihre cloudbasierten Ressourcen, um Abrechnungsmodelle, Cloudabrechnungsarten, Verwaltung, Zugriffssteuerung und betriebliche Effizienz zu unterstützen. Die Kategorisierung von Ressourcen erfordert ein einheitliches und überlegt organisiertes Benennungs- und Metadatendschema.

[Softwaredefinierte Netzwerke](./software-defined-network/overview.md): Stellen Sie sichere Workloads schnell in der Cloud bereit, indem Sie die Funktionen für virtualisierte Netzwerke schnell bereitstellen und modifizieren. Softwaredefinierte Netzwerke (SDNs) können agile Workflows unterstützen, Ressourcen isolieren und cloudbasierte Systeme in Ihre bestehende IT-Infrastruktur integrieren.

[Verschlüsselung](./encryption/overview.md): Schützen Sie Ihre sensiblen Daten durch Verschlüsselung, ein wichtiger Aspekt für Sicherheit im Rahmen einer Cloudbereitstellung.

[Protokolle und Berichterstellung](./log-and-report/overview.md): Überwachen Sie von cloudbasierten Ressourcen generierte Protokolldaten. Die Analyse von Daten liefert integritätsbezogene Erkenntnisse über den Status von Workloads hinsichtlich Betrieb, Wartung und Richtlinienerzwingung.

## <a name="next-steps"></a>Nächste Schritte

Erfahren Sie, wie Abonnements und Konten als Grundlage einer Cloudbereitstellung dienen.

> [!div class="nextstepaction"]
> [Abonnementmodell](subscriptions/overview.md)
