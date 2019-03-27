---
title: 'Framework für die Cloudeinführung (CAF): Inwiefern ändert sich die geschäftlichen Risiken in der Cloud?'
description: Verstehen des Geschäftsrisikos während der Migration
author: BrianBlanchard
ms.date: 10/10/2018
ms.openlocfilehash: 458474f3c94c5df4f7ffef439095adf138f33d78
ms.sourcegitcommit: c053e6edb429299a0ad9b327888d596c48859d4a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/20/2019
ms.locfileid: "58246491"
---
<!-- markdownlint-disable MD026 -->

# <a name="how-does-business-risk-change-in-the-cloud"></a>Inwiefern ändert sich die geschäftlichen Risiken in der Cloud?

Ein Überblick über die Geschäftsrisiken ist einer der wichtigsten Aspekte bei der Cloudtransformation. Das Risiko bestimmt die Richtlinien und beeinflusst die Anforderungen an deren Überwachung und Durchsetzung. Das Risiko wirkt sich stark auf die Verwaltung der digitalen Umgebung aus, ob lokal oder in der Cloud.

<!-- markdownlint-enable MD026 -->

## <a name="relativity-of-risk"></a>Relativität von Risiken

Risiken sind relativ. Ein kleines Unternehmen mit ein paar IT-Ressourcen in einem geschlossenen Gebäude ist einem sehr geringen Risiko ausgesetzt. Fügen Sie Benutzer und eine Internetverbindung mit Zugriff auf diese Ressourcen hinzu, und das Risiko wächst. Wenn dieses kleine Unternehmen Fortune 500-Status erlangt, steigen die Risiken exponentiell an. Im gleichen Maße wie sich Umsatz, Geschäftsprozesse, Anzahl der Mitarbeiter und IT-Ressourcen anhäufen, wachsen und verschmelzen die Risiken. Bei IT-Ressourcen, die zum Generieren von Umsatz dienen, besteht die konkrete Gefahr, dass dieser Umsatzstrom im Falle eines Ausfalls zum Erliegen kommt. Jeder Moment Ausfallzeit führt zu Verlusten. Ebenso wächst mit der Datenmenge das Risiko, dass Kunden geschädigt werden.

In der herkömmlichen lokalen Welt befassen sich Teams für IT-Governance mit der Bewertung dieser Risiken. Sie erstellen Prozesse, um das Risiko zu mindern. Sie stellen Systeme bereit, um sicherzustellen, dass Minderungsmaßnahmen erfolgreich sind und implementiert werden. Dies gleicht die Risiken aus, die für den Betrieb in einer verbundenen, modernen Geschäftsumgebung unvermeidbar sind.

## <a name="understanding-business-risks-in-the-cloud"></a>Grundlegendes zu Geschäftsrisiken in der Cloud

Während einer Transformation sind dieselben relativen Risiken zu beobachten.

* In der frühen Experimentierphase werden einige Ressourcen mit wenigen oder gar keinen relevanten Daten bereitgestellt. Das Risiko ist gering.
* Wenn die erste Workload bereitgestellt wird, steigt das Risiko ein wenig an. Dieses Risiko kann einfach verringert werden, indem Sie eine Anwendung mit von Natur aus geringem Risiko und einer geringen Benutzeranzahl auswählen.
* Während weitere Workloads online geschaltet werden, ändern sich die Risiken mit jedem neuen Release. Neue Apps gehen online, Risiken ändern sich.
* Wenn ein Unternehmen die ersten 10 bis 20 Anwendungen online schaltet, liegt ein wesentlich anderes Risikoprofil vor, als wenn die tausendste Anwendung in die Produktion in der Cloud wechselt.

Die Ressourcen, die sich in der herkömmlichen lokalen Umgebung angesammelt haben, haben sich wahrscheinlich im Laufe der Zeit angehäuft. Das Unternehmen und das IT-Team reiften wahrscheinlich in ähnlicher Weise. Dieses parallele Wachstum bringt häufig einige unnötige Richtlinien mit sich.

Während einer Cloudtransformation haben sowohl das Unternehmen als auch das IT-Team die Möglichkeit, diese Richtlinien zurückzusetzen und mit der gewachsenen Erfahrung neue zu erstellen.

<!-- markdownlint-disable MD026 -->

## <a name="what-is-a-business-risk-mvp"></a>Was ist ein Geschäftsrisiko-MVP?

**Minimum Viable Product** ist ein Standardausdruck der Branche, der die kleinste Einheit eines Produkts definiert, die einen spürbaren Wert erzeugen kann. In einem Geschäftsrisiko-MVP beginnt das Team mit der Annahme, dass einige Ressourcen in einer Cloudumgebung bereitgestellt werden. Zu diesem Zeitpunkt ist noch unbekannt, um welche Ressourcen es sich handelt. Außerdem ist unbekannt, welche Arten von Daten von diesen Ressourcen verarbeitet werden.

Das Cloudgovernanceteam kann für den ungünstigsten Fall vorsorgen und der Cloud jede erdenkliche Richtlinie zuordnen. Dies ist nicht empfehlenswert, aber es ist eine Option.

Im Gegensatz dazu kann das Team einen MVP-Ansatz wählen und einen Ausgangspunkt sowie eine Reihe von Annahmen definieren, die für die meisten bzw. alle Ressourcen gelten.
Im Folgenden finden Sie einige extrem grundlegende Beispiele:

* Alle Ressourcen könnten beendet werden (durch Fehler, Versehen oder Wartung).
* Alle Ressourcen könnten zu viele Ausgaben generieren.
* Alle Ressourcen könnten durch schwache Kennwörter kompromittiert werden.
* Alle Assets, deren geöffnete Ports vom Internet aus zugänglich sind, könnten kompromittiert werden.

Die oben aufgeführten Beispiele sollen die MVP-Geschäftsrisiken als Theorie aufstellen. Die tatsächliche Liste ist für jede Umgebung individuell.
Sobald das Geschäftsrisiko-MVP eingerichtet wurde, können sie in [Richtlinien](overview.md) umgewandelt werden, um die einzelnen Risiken zu mindern.

<!-- markdownlint-enable MD026 -->

## <a name="incremental-risk-mitigation"></a>Inkrementelle Risikominderung

Wenn von einem Geschäftsrisiko-MVP ausgegangen wird, kann die Governance parallel zur geplanten Bereitstellung (statt parallel zum Unternehmenswachstum) reifen. Dies ist eine wesentlich stabileres Modell für die Governanceentwicklung. Bei jeder Iteration werden neue Ressourcen repliziert und bereitgestellt. Mit jedem Release werden die Workloads für die Höherstufung in die Produktion vorbereitet. Natürlich kann das relative Risiko mit jedem Zyklus wachsen.

Wenn das Cloudgovernanceteam parallel zu den Cloudeinführungsteams arbeitet, kann auch der Anstieg von Unternehmensrisiken aufgefangen werden. Jede bereitgestellte Ressource kann ganz einfach nach Risiko klassifiziert werden. Dokumente zur Datenklassifizierung können parallel zum Staging erstellt werden. Risikoprofil und Angriffspunkte können ebenso dokumentiert werden. Im Lauf der Zeit besitzt die gesamte Organisation eine sehr klare Übersicht über die Geschäftsrisiken.

Bei jeder Iteration kann das Cloudgovernanceteam mit dem Cloudstrategieteam zusammenarbeiten, um neue Risiken, Minderungsstrategien, Kompromisse und potenzielle Kosten schnell zu kommunizieren. So können Unternehmensbeteiligte und IT-Experten zusammen ausgereifte, fundierte Entscheidungen treffen. Diese Entscheidungen fließen dann in die Richtlinienentwicklung ein. Bei Bedarf rufen die Richtlinienänderungen neue Arbeitselemente für die Entwicklung wichtiger Infrastruktursysteme hervor. Wenn Änderungen an bereitgestellten Systemen erforderlich sind, haben Cloudeinführungsteams genügend Zeit für Änderungen, während das Unternehmen die bereitgestellten Systeme testet und einen Einführungsplan für Benutzer entwickelt.

Dieser Ansatz reduziert Risiken auf ein Mindestmaß und ermöglicht dem Team gleichzeitig eine schnelle Weiterentwicklung. Darüber hinaus stellt er sicher, dass Risiken sofort erkannt und vor der Bereitstellung behoben werden.

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Evaluieren der Risikotoleranz](./risk-tolerance.md)
