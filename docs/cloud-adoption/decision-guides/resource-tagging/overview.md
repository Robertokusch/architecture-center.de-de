---
title: 'Framework für die Cloudeinführung (CAF): Leitfaden zur Endscheidungsfindung für Ressourcenorganisation und -markierung'
titleSuffix: Microsoft Cloud Adoption Framework for Azure
ms.service: architecture-center
ms.subservice: enterprise-cloud-adoption
ms.custom: governance
ms.date: 02/11/2019
description: Erfahren Sie mehr über Ressourcenorganisation und -markierung als Hauptdienst in Azure-Migrationen.
author: rotycenh
ms.openlocfilehash: e7f39fe31dc809286cdb45b857f1d7370e3bf9d4
ms.sourcegitcommit: c053e6edb429299a0ad9b327888d596c48859d4a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/20/2019
ms.locfileid: "58241661"
---
# <a name="resource-organization-and-tagging-decision-guide"></a>Leitfaden zur Endscheidungsfindung für Ressourcenorganisation und -markierung

Die Organisation cloudbasierter Ressourcen ist eine der wichtigsten Aufgaben für die IT-Abteilung, wenn Ihre Bereitstellungen nicht extrem einfach aufgebaut sind. Die Organisation Ihrer Ressourcen erfüllt drei Hauptaufgaben:

- **Ressourcenverwaltung**. Ihre IT-Teams müssen Ressourcen, die bestimmten Workloads, Umgebungen, Besitzergruppen oder anderen wichtigen Informationen zugeordnet sind, schnell finden. Die Organisation von Ressourcen ist wichtig für die Zuweisung organisatorischer Rollen und Zugriffsberechtigungen für die Ressourcenverwaltung.
- **Automatisierung**. Neben einer Vereinfachung der Ressourcenverwaltung für die IT-Abteilung ermöglicht Ihnen ein angemessenes Organisationsschema die Nutzung der Automatisierung im Rahmen der Ressourcenerstellung, der Betriebsüberwachung und der Erstellung von DevOps-Prozessen.
- **Rechnungsstellung**. Um Unternehmensgruppen auf den Verbrauch von Cloudressourcen aufmerksam zu machen, muss die IT-Abteilung verstehen, welche Workloads und Teams welche Ressourcen verwenden. Um Ansätze wie z.B. die verbrauchsbasierte Kostenzuteilung zu unterstützen, müssen Cloudressourcen dem Besitz und der Nutzung entsprechend organisiert werden.

## <a name="tagging-decision-guide"></a>Leitfaden zur Entscheidungsfindung für Markierungen

![Abbildung der Markierungsoptionen mit zunehmender Komplexität entsprechend den nachstehenden weiterführenden Links](../../_images/discovery-guides/discovery-guide-tagging.png)

Wechseln Sie zu: [Baselinenamenskonventionen](#baseline-naming-conventions) | [Ressourcenmarkierungsmuster](#resource-tagging-patterns) | [Namens- und Markierungsrichtlinie](#naming-and-tagging-policy) | [Weitere Informationen](#learn-more)

Ihr Markierungsansatz kann einfach oder komplex sein, wobei der Schwerpunkt von der Unterstützung der IT-Teams, die die Cloudworkloads verwalten, bis zur Integration von Informationen zu sämtlichen Aspekten des gesamten Unternehmen reichen kann.

Ein IT-orientierter Markierungsfokus (etwa Markierungen auf der Grundlage von Workload, Funktion oder Umgebung) macht die Überwachung von Ressourcen weniger komplex und vereinfacht Entscheidungen auf der Grundlage operativer Anforderungen erheblich.

Bei Markierungsschemas mit geschäftlichem Fokus (beispielsweise Buchhaltung, Unternehmensbesitz oder geschäftliche Bedeutung) muss unter Umständen mehr Zeit in die Erstellung von Markierungsstandards investiert werden, die die Geschäftsinteressen widerspiegeln und diese Standards langfristig bewahren. Allerdings ist das Ergebnis dieses Prozesses ein Markierungssystem, das eine verbesserte Erfassung der Kosten und des Werts von IT-Ressourcen für das gesamte Unternehmen bietet. Diese Verknüpfung des geschäftlichen Werts einer Ressource mit den Betriebskosten ist einer der ersten Schritte, um die Wahrnehmung der IT als Kostenfaktor in Ihrer Organisation zu verändern.

## <a name="baseline-naming-conventions"></a>Baselinenamenskonventionen

Eine standardisierte Namenskonvention ist der Ausgangspunkt für die Organisation Ihrer in der Cloud gehosteten Ressourcen. Mit einem ordnungsgemäß strukturierten Benennungssystem können Sie Ressourcen sowohl zu Verwaltungs- als auch zu Abrechnungszwecken schnell identifizieren. Wenn in anderen Teilen Ihrer Organisation bereits IT-Namenskonventionen vorhanden sind, wägen Sie ab, ob Sie Ihre Namenskonventionen für die Cloud darauf abstimmen oder separate cloudbasierte Standards einrichten möchten.

Beachten Sie auch, dass unterschiedliche Azure-Ressourcentypen unterschiedliche [Benennungsanforderungen](../../../best-practices/naming-conventions.md#naming-rules-and-restrictions) aufweisen. Ihre Namenskonventionen müssen mit diesen Benennungsanforderungen kompatibel sein.

## <a name="resource-tagging-patterns"></a>Ressourcenmarkierungsmuster

Für eine komplexere Organisation als eine konsistente Namenskonvention bereitstellen kann, unterstützen Cloudplattformen die Möglichkeit zum Markieren von Ressourcen.

*Markierungen* sind Metadatenelemente, die den Ressourcen zugeordnet werden. Markierungen bestehen aus Paaren aus Schlüssel-Wert-Zeichenfolgen. Welche Werte Sie in diese Paare einschließen, bleibt Ihnen überlassen, aber die Anwendung einer konsistenten Menge globaler Markierungen im Rahmen einer umfassenden Benennungs- und Markierungsrichtlinie ist ein wesentlicher Bestandteil einer allgemein gültigen Governancerichtlinie.

Im Folgenden sind einige Beispiele für allgemeine Markierungsmuster aufgeführt:

<!-- markdownlint-disable MD033 -->

| Markierungstyp | Beispiele | BESCHREIBUNG |
|-----|-----|-----|
| Funktionen            | app = catalogsearch1 <br/>tier = web <br/>webserver = apache<br/>env = prod <br/>env = staging <br/>env = dev                 | Kategorisieren Sie Ressourcen in Bezug auf ihren Zweck innerhalb einer Workload, auf die Umgebung, in der sie bereitgestellt werden, oder auf andere funktionsbezogene oder operative Details.                                 |
| Classification        | confidentiality=private<br/>sla = 24hours                                 | Klassifiziert eine Ressource danach, wie sie verwendet wird und welche Richtlinien für sie gelten.                               |
| Buchhaltung            | department = finance <br/>project = catalogsearch <br/>region = northamerica | Ermöglicht die Zuordnung der Ressource zu bestimmten Gruppen innerhalb einer Organisation zu Abrechnungszwecken. |
| Partnerschaft           | owner = jsmith <br/>contactalias = catsearchowners<br/>stakeholders = user1;user2;user3<br/>                       | Enthält Informationen dazu, welche Personen (außerhalb der IT) mit der Ressource verknüpft oder in anderer Form von ihr betroffen sind.                      |
| Zweck               | businessprocess=support<br/>businessimpact=moderate<br/>revenueimpact=high   | Richtet Ressourcen an Geschäftsfunktionen aus, um Investitionsentscheidungen besser zu unterstützen.  |

<!-- markdownlint-enable MD033 -->

## <a name="naming-and-tagging-policy"></a>Namens- und Markierungsrichtlinie

Ihre Benennungs- und Markierungsrichtlinie wird sich im Laufe der Zeit weiterentwickeln. Es ist jedoch entscheidend, dass Sie Ihre wesentlichen organisatorischen Prioritäten zu Beginn einer Cloudmigration festlegen. Erwägen Sie im Rahmen Ihres Planungsprozesses sorgfältig die folgenden Fragen:

- Wie können Ihre Benennungs- und Markierungsrichtlinien am besten in vorhandene Benennungs- und Organisationsrichtlinien innerhalb Ihrer Organisation integriert werden?
- Werden Sie ein Abrechnungssystem mit verbrauchsbasierter Kostenzuteilung implementieren? Müssen Sie Buchhaltungsinformationen für Abteilungen, Geschäftsbereiche und Teams bereitstellen, die über eine einfache Aufschlüsselung auf der Abonnementebene hinausgehen?
- Welche Markierungsinformationen werden für alle Ressourcen benötigt? Die Implementierung welcher Markierungsinformationen wird den einzelnen Teams überlassen?
- Muss die Markierung Details wie Anforderungen zur Einhaltung gesetzlicher Bestimmungen für eine Ressource darstellen? Wie sieht es mit betriebsbezogenen Details wie Anforderungen an die Betriebszeiten, Zeitpläne für Patches oder Sicherheitsanforderungen aus?

## <a name="learn-more"></a>Weitere Informationen

Weitere Informationen zur Benennung und Markierung in Azure finden Sie unter:

- [Namenskonventionen für Azure-Ressourcen](../../../best-practices/naming-conventions.md). In dieser Anleitung auf der Website mit Azure-Cloudgrundlagen finden Sie empfohlene Namenskonventionen für Azure-Ressourcen.
- [Verwenden von Tags zum Organisieren von Azure-Ressourcen](/azure/azure-resource-manager/resource-group-using-tags?toc=/azure/billing/TOC.json). Sie können Tags in Azure sowohl auf der Ressourcengruppenebene als auch auf der Ebene einzelner Ressourcen anwenden. So können Sie Abrechnungsberichte anhand der angewendeten Markierungen unterschiedlich detailliert gestalten.

## <a name="next-steps"></a>Nächste Schritte

Erfahren Sie, wie die Verschlüsselung zum Absichern von Daten in Cloudumgebungen verwendet wird.

> [!div class="nextstepaction"]
> [Verschlüsselung](../encryption/overview.md)
