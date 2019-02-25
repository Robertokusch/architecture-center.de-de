---
title: 'Framework für die Cloudeinführung (Cloud Adoption Framework, CAF): Welche geschäftlichen Risiken sind mit einer Cloudtransformation verbunden?'
description: Erläuterung der geschäftlichen Risiken bei einer Cloudtransformation
author: BrianBlanchard
ms.date: 10/10/2018
ms.openlocfilehash: bfd91da42d20a85004debc6b767a1482ba3e158d
ms.sourcegitcommit: 273e690c0cfabbc3822089c7d8bc743ef41d2b6e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/08/2019
ms.locfileid: "55902082"
---
# <a name="evaluating-risk-tolerance"></a>Evaluieren der Risikotoleranz

Jede Geschäftsentscheidung führt zu neuen Risiken. Jede Investition in beliebigen Bereichen bringt ein Verlustrisiko mit sich. Neue Produkte oder Dienste bergen das Risiko eines Marktversagens. Durch Änderungen an aktuellen Produkten oder Diensten kann sich der Marktanteil verringern. Die Cloudtransformation ist keine Wunderlösung für die alltäglichen Geschäftsrisiken. Im Gegenteil – durch verbundene Lösungen (Cloud oder lokal) werden neue Risiken eingeführt. Die Bereitstellung von Ressourcen für mit dem Netzwerk verbundene Einrichtungen erweitert zudem das Profil potenzieller Bedrohungen, indem Sicherheitslücken einer viel größeren, globalen Community zugänglich gemacht werden. Glücklicherweise sind Cloudanbieter mit den geänderten, erhöhten und zusätzlichen Risiken vertraut. Sie investieren in hohem Maße in die Minderung dieser Risiken im Interesse ihrer Kunden.

Dieser Artikel beschäftigt sich nicht vorrangig mit Cloudrisiken. Er befasst sich vielmehr mit den geschäftlichen Risiken, die mit den verschiedenen Formen der Cloudtransformation in Zusammenhang stehen. Im weiteren Verlauf dieses Artikels verlagert sich der Fokus auf die Diskussion von Möglichkeiten zum Verstehen der Risikotoleranz des Unternehmens.

<!-- markdownlint-disable MD026 -->

## <a name="what-business-risks-are-associated-with-a-cloud-transformation"></a>Welche geschäftlichen Risiken sind mit einer Cloudtransformation verbunden?

Leider basieren die wahren geschäftlichen Risiken auf den Hintergründen und der Durchführung bestimmter Transformationen. Glücklicherweise gibt eine Reihe sehr weit verbreiteter Risiken, die als Gesprächsgrundlage für das Verständnis spezifischer, personalisierter Risiken dienen kann.

** Bevor Sie weiterlesen, sollten Sie wissen, dass jedes dieser Risiken entschärft werden kann. Das Ziel dieses Artikels besteht darin, die Leser zu informieren und vorzubereiten, damit sich die Diskussion zur Risikominderung effektiver gestaltet. **

* Datenschutzrisiko: Das Risiko Nummer 1 im Zusammenhang mit jeder Transformation ist der Schutz von Daten. Im heutigen digitalen Geschäftszeitalter sind Daten das neue Öl. Sie treiben die Wirtschaft an, wärmen das Büro und stellen die Kunden zufrieden. Gibt es jedoch ein Leck, ist das Ergebnis gleichermaßen katastrophal. Alle Änderungen an der Speicherung, Verarbeitung oder Verwendung der Daten stellen ein Risiko dar. Cloudtransformationen sorgen für umfangreiche Änderungen im Bereich der Datenverwaltung, daher sollte das Risiko nicht auf die leichte Schulter genommen werden. [Sicherheitbaseline](../security-baseline/overview.md), [Datenklassifizierung](./what-is-data-classification.md) und [inkrementelle Rationalisierung](../../digital-estate/rationalize.md#incremental-rationalization) können jeweils bei der Minderung dieses Risikos hilfreich sein.

* Risiko in Bezug auf Vorgänge und Kundenerlebnis: Geschäftsvorgänge und Kundenerlebnis hängen stark von technischen Vorgängen ab. Cloudtransformationen führen zu Änderungen in technischen Vorgängen (Technical Operations, TechOps). In einigen Organisationen ist die Veränderung gering und leicht anzupassen. In anderen Organisationen sind für TechOps-Änderungen möglicherweise neue Tools, neue Fachkräfte oder neue Supportansätze erforderlich. Je umfangreicher die Änderung, desto größer die potenziellen Auswirkungen auf Geschäftsvorgänge und Kundenerlebnis. Eine Minderung dieses Risikos kann durch Einbeziehung des Unternehmens in die Transformationsplanung erzielt werden. Die Abschnitte „Releaseplanung“ und „Auswählen der ersten Workload“ im Artikel [Inkrementelle Rationalisierung](../../digital-estate/rationalize.md#incremental-rationalization) erörtern Möglichkeiten zur Auswahl von Workloads für Transformationsprojekte. Die Rolle des Unternehmens bei dieser Aktivität besteht darin, das Risiko für Geschäftsvorgänge von den Änderungen bis hin zu priorisierten Workloads zu kommunizieren. Wird die IT bei der Auswahl von Workloads unterstützt, die geringere Auswirkungen auf Vorgänge haben, verringert sich das Gesamtrisiko.

* Kostenrisiko: In der Cloud verändern sich die Kostenmodelle. Diese Änderung kann zu Risiken im Zusammenhang mit Kostenüberschreitungen oder erhöhten Kosten für verkaufte Waren führen, insbesondere bei direkt damit verbundenen Betriebsausgaben. Wenn das Unternehmen eng mit der IT zusammenarbeitet, ist es möglich, in Bezug auf die Kosten und die von verschiedenen Geschäftseinheiten, Programmen, Projekten usw. genutzten Dienste für Transparenz zu sorgen. Unter [Cost Management] finden Sie Beispiele, wie Unternehmen und IT sich zu diesem Thema zusammenschließen können.

Oben sind einige der gängigsten von Kunden genannten Risiken aufgeführt. Das Cloudgovernanceteam und die Cloudeinführungsteams können mit der Entwicklung eines Risikoprofils beginnen, während Workloads migriert und für den Einsatz in der Produktion vorbereitet werden. Richten Sie sich auf Gespräche ein, um Risiken basierend auf dem gewünschten Geschäftsergebnis und Transformationsaufwand zu definieren, zu präzisieren und zu verringern.

## <a name="understanding-risk-tolerance"></a>Verstehen der Risikotoleranz

Die Identifikation von Risiken ist ein relativ direkter Prozess. Risiken gehören branchenübergreifend zum Geschäft. Allerdings ist die Risikobereitschaft extrem individuell ausgeprägt. An diesem Punkt laufen Gespräche zwischen Unternehmen und IT tendenziell auseinander. Die beiden Gesprächspartner sprechen im Wesentlichen eine andere Sprache. Die folgenden Vergleiche und Fragen sollen als Einstieg für Diskussionen dienen, die es beiden Parteien ermöglichen, die Risikobereitschaft besser zu verstehen und einzuschätzen.

## <a name="simple-use-case-for-comparison"></a>Einfacher Anwendungsfall zum Vergleich

Um die Risikotoleranz zu verstehen, untersuchen wir die Kundendaten. Wenn ein Unternehmen in einer beliebigen Branche Kundendaten auf einem ungesicherten Server ablegt, bleibt das Risiko, ob diese Daten kompromittiert oder gestohlen werden, mehr oder weniger das gleiche. Die Art der Daten und die Toleranz von Unternehmen für dieses Risiko variieren jedoch gewaltig.

* Unternehmen im Gesundheits- und Finanzwesen in den Vereinigten Staaten unterliegen strikten Konformitätsanforderungen von Drittanbietern. Es wird vorausgesetzt, dass personenbezogene Informationen (PII) oder gesundheitsbezogene Daten extrem vertraulich behandelt werden. Für diese Arten von Unternehmen gibt es schwerwiegende Konsequenzen, wenn sie an dem oben beschriebenen Risikoszenario beteiligt sind. Ihre Toleranz wird dementsprechend extrem gering sein. Alle innerhalb oder außerhalb des Netzwerks veröffentlichten Kundendaten müssen durch diese Drittanbieter-Konformitätsrichtlinien verwaltet werden.
* Ein Gamingunternehmen, dessen Kundendaten sich auf einen Benutzernamen, die Spielzeiten und die Bestenlisten beschränken, wird wahrscheinlich keine erheblichen Konsequenzen erwarten, wenn es das oben beschriebene riskante Verhalten zeigt. Wenn ungeschützte Daten gefährdet sind, bleiben die Auswirkungen dieses Risikos gering. Daher ist die Risikotoleranz in diesem Fall hoch.
* Ein mittelständisches Unternehmen, das Tausenden von Kunden einen Teppichreinigungsdienst anbietet, fällt zwischen diese beiden Toleranzextreme. Die Kundendaten sind möglicherweise ausführlicher und enthalten Details wie Adresse oder Telefonnummer. Beides kann als personenbezogene Informationen betrachtet und sollte geschützt werden. Allerdings gibt es möglicherweise keine spezifische Governanceanforderung, die vorschreibt, dass die Daten abgesichert werden müssen. Aus der IT-Perspektive ist die Antwort einfach: Die Daten müssen geschützt werden. Aus geschäftlicher Sicht ist die Sache möglicherweise nicht so einfach. Das Unternehmen benötigt weitere Details, bevor es ein Maß an Toleranz für dieses Risiko ermitteln kann.

Der nächste Abschnitt enthält einige Beispielfragen, anhand derer das Unternehmen ein Maß an Risikotoleranz für den obigen und für andere Anwendungsfälle festlegen kann.

## <a name="risk-tolerance-questions"></a>Fragen zur Risikotoleranz

In diesem Abschnitt werden Fragen zum Gesprächseinstieg in drei Kategorien aufgeführt: Auswirkungen von Datenverlusten, Wahrscheinlichkeit des Datenverlusts und Kosten der Lösung. Wenn Unternehmen und IT sich gemeinsam mit jedem dieser Bereiche befassen, kann die Entscheidung zur Risikominderung und zur Gesamttoleranz für ein bestimmtes Risiko leicht getroffen werden.

**Auswirkungen von Datenverlusten**: Fragen zum Ermitteln der Auswirkungen eines Risikos. Diese Fragen können schwierig (sogar unmöglich) zu beantworten sein. Ideal ist es, die Auswirkungen in Zahlen auszudrücken, aber oft reicht die Diskussion allein bereits aus, um die Risikotoleranz zu verstehen. Auch Spannen sind zulässig, insbesondere dann, wenn sie mit den Annahmen präsentiert werden, anhand derer sie ermittelt wurden.

* Werden Drittanbieter-Konformitätsanforderungen durch dieses Risiko verletzt?
* Verletzt dieses Risiko interne Unternehmensrichtlinien?
* Kann dieses Risiko Kunden oder Marktanteile kosten? Falls ja, lassen sich diese Auswirkungen quantifizieren?
* Kann dieses Risiko zu negativen Kundenerlebnissen führen? Beeinträchtigen diese Erlebnisse wahrscheinlich den Umsatz oder die Ertragsrealisierung?
* Kann dieses Risiko eine gesetzliche Haftung herbeiführen? Falls ja, gibt es hierfür einen Präzendenzfall für zu leistenden Schadenersatz?
* Können die Geschäftsvorgänge durch dieses Risiko zum Erliegen kommen? Falls ja, wie lang wäre der Betriebsausfall?
* Kann dieses Risiko die Geschäftsvorgänge verlangsamen? Falls ja, inwieweit und für wie lange?
* Ist dieses Risiko zu diesem Zeitpunkt in der Transformation einmalig, oder kann es sich wiederholen?
* Nimmt die Häufigkeit des Risikos im Verlauf der Transformation zu oder ab?
* Nimmt die Wahrscheinlichkeit des Risikos im Laufe der Zeit zu oder ab?
* Ist das Risiko von der Natur her zeitkritisch? Geht das Risiko vorüber, oder wird es schlimmer, wenn es nicht behandelt wird?

Diese grundlegenden Fragen führen zu vielen weiteren Fragen. Nachdem Sie einen konstruktiven Dialog geführt haben, wird empfohlen, die relevanten Risiken aufzuzeichnen und nach Möglichkeit zu beziffern.

**Kosten für die Lösung**: Fragen zum Ermitteln der Kosten für eine Minderung oder Beseitigung des Risikos. Diese Fragen können sehr direkt sein, insbesondere, wenn sie geballt präsentiert werden.

* Gibt es eine klare Lösung? Was kostet sie?
* Gibt es mehrere Optionen für die Beseitigung oder Minderung dieses Risikos? In welchem Bereich liegen die Kosten für diese Lösungen?
* Was wird vom Unternehmen benötigt, um eine optimale, klare Lösung auswählen zu können?
* Was wird vom Unternehmen benötigt, um die Kosten zu überprüfen?
* Welche weiteren Vorteile können sich aus der Lösung ergeben, die dieses Risiko mindern würden?

Die technischen Lösungen, die zur Risikominderung erforderlich sind, werden durch diese Fragen sehr vereinfacht dargestellt. Die Fragen kommunizieren diese Lösungen jedoch in einer Weise, die dem Unternehmen einen schnellen Einstieg in den Entscheidungsprozess ermöglicht.

**Wahrscheinlichkeit von Datenverlusten**: Fragen zum Ermitteln der Wahrscheinlichkeit, dass der Risikofall eintritt. Dieser Bereich ist am schwierigsten zu beziffern. Stattdessen wird empfohlen, das Cloudgovernanceteam basierend auf den zugrunde liegenden Daten Kategorien festlegen zu lassen, mit deren Hilfe die Wahrscheinlichkeit vermittelt werden kann. Die folgenden Fragen können beim Erstellen von für das Team sinnvollen Kategorien hilfreich sein.

* Wurden in Bezug auf die Wahrscheinlichkeit, dass dieses Risiko eintritt, Recherchen betrieben?
* Kann der Anbieter Referenzen oder Statistiken zur Wahrscheinlichkeit der Auswirkungen vorlegen?
* Gibt es andere Unternehmen im relevanten Sektor oder in der entsprechenden Branche, die von diesem Risiko betroffen waren?
* Gibt es allgemein andere Unternehmen, die von diesem Risiko betroffen waren?
* Ist dieses Risiko einzigartig und bezieht sich nur auf etwas, das in diesem Unternehmen mangelhaft gehandhabt wurde?

Nach Beantwortung dieser und weiterer Fragen nach Ermessen des Cloudgovernanceteams werden sich voraussichtlich Wahrscheinlichkeitsgruppen herauskristallisieren. Im Folgenden werden einige Gruppierungsbeispiele für den Einstieg aufgeführt:

* Keine Anhaltspunkte: Es gab noch nicht genügend Untersuchungen, um die Wahrscheinlichkeit zu ermitteln.
* Geringes Risiko: Gemäß dem aktuellen Stand der Forschung wird das Risiko wahrscheinlich nicht eintreten.
* Zukünftiges Risiko: Die aktuelle Wahrscheinlichkeit lautet „Geringes Risiko“. Bei fortgesetzter Einführung wird jedoch eine neue Analyse ausgelöst.
* Mittleres Risiko: Es ist wahrscheinlich, dass sich das Risiko auf das Unternehmen auswirkt.
* Hohes Risiko: Im Lauf der Zeit wird es immer weniger wahrscheinlich, dass das Unternehmen die Auswirkungen dieses Risikos vermeiden kann.
* Abnehmendes Risiko: Das Risiko ist mittel bis hoch. Die Wahrscheinlichkeit von Auswirkungen lässt sich jedoch durch Maßnahmen in IT oder Unternehmen reduzieren.

**Ermitteln der Toleranz:**

Die drei oben aufgeführten Fragengruppen sollten genügend Stoff zum Ermitteln der anfänglichen Toleranzen bieten. Wenn Risiko und Wahrscheinlichkeit niedrig und die Kosten zur Minderung hoch sind, wird das Unternehmen kaum in die Risikominderung investieren. Sind Risiko und Wahrscheinlichkeit hoch, wird das Unternehmen sicherlich eine Investition erwägen, solange die Kosten nicht die potenziellen Risiken überschreiten.

## <a name="next-steps"></a>Nächste Schritte

Diese Art des Gesprächs kann Unternehmen und IT dabei unterstützen, die Toleranz effektiver auszuwerten. Dialoge dieser Art können während der Erstellung von MVP-Richtlinien und bei inkrementellen Richtlinienüberprüfungen geführt werden.

> [!div class="nextstepaction"]
> [Unternehmensrichtlinie definieren](./define-policy.md)
