---
title: 'Framework für die Cloudeinführung (Cloud Adoption Framework, CAF): Governance in Microsoft CAF für Azure'
ms.service: architecture-center
ms.subservice: enterprise-cloud-adoption
ms.custom: governance
ms.date: 02/11/2019
description: Governance in Microsoft CAF für Azure
author: BrianBlanchard
ms.openlocfilehash: b7a0739a42a27def34955577acffc7ab7292ac50
ms.sourcegitcommit: c053e6edb429299a0ad9b327888d596c48859d4a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/20/2019
ms.locfileid: "58247371"
---
# <a name="governance-in-the-microsoft-caf-for-azure"></a>Governance in Microsoft CAF für Azure

Die Cloud schafft neue Paradigmen in Bezug auf die Technologien, die das Unternehmen unterstützen. Diese neuen Paradigmen bewirken auch Veränderungen in der Art und Weise, wie diese Technologien eingeführt, verwaltet und gesteuert werden. Wenn ganze Datencenter mit einer Codezeile aus einem unbeaufsichtigten Prozess zerstört und neu erstellt werden können, müssen wir die traditionellen Ansätze überdenken. Dies gilt gleichermaßen für die Governance.

Für Unternehmen mit vorhandenen Richtlinien für lokale IT-Umgebungen sollte Cloud Governance diese Richtlinien ergänzen. Der Grad der Integration von Unternehmensrichtlinien zwischen lokalen Systemen und der Cloud hängt jedoch von der Reife der Cloud Governance und der digitalen Infrastruktur in der Cloud ab. Mit der Entwicklung der Cloudinfrastruktur im Lauf der Zeit werden auch Cloud Governance-Prozesse und -Richtlinien weiterentwickelt.

Die Anleitungen in diesem Abschnitt zum CAF dienen zwei Zwecken:

* Bereitstellen von umsetzungsfähigen Kundenlösungen, die gemeinsame Erfahrungen darstellen, die Kunden häufig machen. Jede dieser Erfahrungen umfasst Geschäftsrisiken, Unternehmensrichtlinien zur Risikominderung und Designrichtlinien für die Implementierung technischer Lösungen. Die Designrichtlinien sind zwangsläufig spezifisch für Azure. Alle anderen Orientierungshilfen in diesen Lösungen könnten im Rahmen eines cloudagnostischen oder Multi-Cloud-Ansatzes angewendet werden.
* Unterstützung der Leser beim Erstellen personalisierter Governancelösungen, die einer Vielzahl von Geschäftsanforderungen gerecht werden können, einschließlich der Verwaltung mehrerer öffentlicher Clouds, durch detaillierte Anleitungen zur Entwicklung von Unternehmensrichtlinien, Prozessen und Tools.

Dieser Inhalt richtet sich an das Cloud Governance-Team. Es ist auch für Cloudarchitekten relevant, die eine solide Grundlage für Cloud Governance entwickeln müssen.

## <a name="audience"></a>Zielgruppe

Die Inhalte im CAF beeinflussen das Geschäft, die Technologie und die Kultur von Unternehmen. Dieser Abschnitt zum CAF interagiert intensiv mit IT-Sicherheit, IT-Governance, der Finanzabteilung, den Branchenführen sowie den Netzwerk-, Identitäts- und Cloudeinführungsteams. Es gibt verschiedene Abhängigkeiten von diesen Rollen, die einen moderierenden Ansatz der Cloudarchitekten unter Verwendung dieser Leitlinien erfordern. Die Zusammenarbeit mit diesen Teams kann eine einmalige Aufgabe sein, aber in einigen Fällen wird sie zu wiederkehrenden Interaktionen mit diesen anderen Rollen führen.

Der Cloudarchitekt dient als innovative Führungsperson und Vermittler, um diese Zielgruppen zusammenzubringen. Der Inhalt dieser Sammlung von Anleitungen ist darauf ausgelegt, dem Cloudarchitekten die richtige Kommunikation mit der richtigen Zielgruppe zu erleichtern und die erforderlichen Entscheidungen anzustoßen. Von der Cloud unterstützte Unternehmenstransformationen hängen von der Rolle des Cloudarchitekten ab, Entscheidungen im gesamten Geschäfts- und IT-Bereich anzuleiten.

**Spezialisierungen des Cloudarchitekten in diesem Abschnitt:** Jeder Abschnitt des CAF für die Einführung der Cloud stellt eine andere Spezialisierung oder Variante der Cloudarchitektenrolle dar. Dieser Abschnitt des CAF richtet sich an Cloudarchitekten mit einer Leidenschaft für die Verringerung oder Bereinigung technischer Risiken. Viele Cloudanbieter bezeichnen diese Spezialisten als Cloudverwalter, wir bevorzugen allerdings die Begriffe „Cloudwächter“ oder kollektiv „Cloud Governance-Team“. In jeder umsetzbaren Kundenlösung zeigen die Artikel, wie sich die Zusammensetzung und Rolle des Cloud Governance-Teams im Lauf der Zeit ändern kann.

## <a name="using-this-guide"></a>Verwenden dieses Leitfadens

Für Leser, die diesem Leitfaden von Anfang bis Ende folgen, werden diese Inhalte dazu beitragen, eine robuste Cloud Governance-Strategie parallel zur Cloudimplementierung zu entwickeln. Der Leitfaden führt den Leser durch die Theorie und Implementierung einer solchen Strategie.

Einen Crashkurs zur Theorie und zum schnellen Zugang zur Azure-Implementierung finden Sie in der [Übersicht über nützliche Governance-Vorgehensweisen](./journeys/overview.md). Durch diese Anleitung kann der Leser klein anfangen und seine Governanceanforderungen parallel zu den Bemühungen zur Cloudeinführung weiterentwickeln.

## <a name="next-steps"></a>Nächste Schritte

Machen Sie sich mit den nützlichen Governance-Vorgehensweisen vertraut.

> [!div class="nextstepaction"]
> [Nützliche Governancevorgehensweisen](./journeys/overview.md)
