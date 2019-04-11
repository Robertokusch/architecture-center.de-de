---
title: 'Framework für die Cloudeinführung (Cloud Adoption Framework, CAF): Große Unternehmen: anfängliche Unternehmensrichtlinie hinter der Governancestrategie'
titleSuffix: Microsoft Cloud Adoption Framework for Azure
ms.service: architecture-center
ms.subservice: enterprise-cloud-adoption
ms.custom: governance
ms.date: 02/11/2019
description: 'Große Unternehmen: anfängliche Unternehmensrichtlinie hinter der Governancestrategie'
author: BrianBlanchard
ms.openlocfilehash: 51b709b3ae6d704a229a08611a3cbc0da974c6c2
ms.sourcegitcommit: c053e6edb429299a0ad9b327888d596c48859d4a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/20/2019
ms.locfileid: "58241145"
---
# <a name="large-enterprise-initial-corporate-policy-behind-the-governance-strategy"></a>Große Unternehmen: Anfängliche Unternehmensrichtlinie hinter der Governancestrategie

Die folgende Unternehmensrichtlinie definiert die anfängliche Governanceposition, die der Ausgangspunkt für diese Lösung ist. In diesem Artikel werden Risiken in einem frühen Stadium, anfängliche Richtlinienanweisungen und frühe Prozesse zum Erzwingen von Richtlinienanweisungen definiert.

> [!NOTE]
>Die Unternehmensrichtlinie ist kein technisches Dokument, gibt aber den Anstoß zu vielen technischen Entscheidungen. Das in der [Übersicht](./overview.md) beschriebene Governance-MVP ist letztlich von dieser Richtlinie abgeleitet. Vor der Implementierung eines Governance-MVP sollte Ihre Organisation eine Unternehmensrichtlinie entwickeln, die auf Ihren eigenen Zielen und geschäftlichen Risiken basiert.

## <a name="cloud-governance-team"></a>Cloudgovernanceteam

Der CIO hatte kürzlich eine Besprechung mit dem IT Governance-Team, um die Geschichte der Richtlinien für personenbezogene Informationen und der unternehmenskritischen Richtlinien zu verstehen und die Auswirkungen zu untersuchen, die eine Änderung dieser Richtlinien mit sich bringen würde. Dabei wurde auch das Gesamtpotenzial der Cloud für IT und das Unternehmen erörtert.

Nach der Besprechung baten zwei Mitglieder des IT-Governance-Teams um Erlaubnis, die Anstrengungen zur Cloudplanung zu erforschen und zu unterstützen. In Anerkennung der Notwendigkeit von Governance und der Möglichkeit, Schatten-IT zu begrenzen, fand diese Idee die Unterstützung des Leiters der IT-Governance. Mit diesem Schritt wurde das Cloud Governance-Team aus der Taufe gehoben. Im Lauf der nächsten Monate wird es die Bereinigung vieler Fehler – aus der Governance-Perspektive – übernehmen, die während der Erkundung in der Cloud gemacht wurden. Das wird ihm den Spitznamen der Cloudverwahrer eintragen. Auf späteren Entwicklungsstufen lässt sich an diesem Weg ablesen, wie sich seine Rolle im Lauf der Zeit ändert.

[!INCLUDE [business-risk](../../../../../includes/cloud-adoption/governance/business-risks.md)]

## <a name="tolerance-indicators"></a>Toleranzindikatoren

Die aktuelle Risikotoleranz ist hoch, und das Interesse an der Investition in die Cloud-Governance ist gering. Daher fungieren die Toleranzindikatoren als Frühwarnsystem, um die Investition von Zeit und Energie auszulösen. Falls die nachfolgend aufgeführten Indikatoren festgestellt werden, ist es ratsam, die Governancestrategie weiterzuentwickeln.

- Kostenverwaltung: Das Ausmaß der Bereitstellung übersteigt 1.000 Ressourcen in der Cloud, oder die monatlichen Ausgaben überschreiten 10.000 USD pro Monat.
- Identitätsbaseline: Einbeziehung von Anwendungen mit Legacy- oder Drittanbieteranforderungen für MFA (Multi-Factor Authentication, mehrstufige Authentifizierung).
- Sicherheitsbaseline: Die Aufnahme der geschützten Daten in definierte Cloudeinführungspläne.
- Ressourcenkonsistenz: Die Aufnahme der unternehmenskritischen Anwendungen in definierte Cloudeinführungspläne.

[!INCLUDE [policy-statements](../../../../../includes/cloud-adoption/governance/policy-statements.md)]

## <a name="next-steps"></a>Nächste Schritte

Diese Unternehmensrichtlinie bereitet das Cloudgovernanceteam auf die Implementierung des Governance-MVP vor, das die Grundlage für die Einführung bildet. Der nächste Schritt ist das Implementieren dieses MVP.

> [!div class="nextstepaction"]
> [Beschreibung der bewährten Methode](./best-practice-explained.md)