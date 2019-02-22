---
title: 'CAF: Metriken, Indikatoren und Risikotoleranz in der Kostenverwaltung'
titleSuffix: Microsoft Cloud Adoption Framework for Azure
ms.service: architecture-center
ms.subservice: enterprise-cloud-adoption
ms.custom: governance
ms.date: 02/11/2019
description: Erläuterung der Kostenverwaltung in Bezug auf Cloudgovernance
author: BrianBlanchard
ms.openlocfilehash: 76e6b1b32dd862322f6cafd9aa63c6c4f79f4f5d
ms.sourcegitcommit: 273e690c0cfabbc3822089c7d8bc743ef41d2b6e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/08/2019
ms.locfileid: "55902026"
---
# <a name="cost-management-metrics-indicators-and-risk-tolerance"></a>Metriken, Indikatoren und Risikotoleranz in der Kostenverwaltung

Dieser Artikel soll Ihnen bei der Quantifizierung der Geschäftsrisikotoleranz im Zusammenhang mit der Kostenverwaltung helfen. Das Definieren von Metriken und Indikatoren ermöglicht das Erstellen eines Geschäftsszenarios, mit dem Sie verstärkt auf die Ausgereiftheit der Disziplin der Kostenverwaltung setzen können.

## <a name="metrics"></a>Metriken

Die Kostenverwaltung befasst sich in der Regel mit Metriken im Zusammenhang mit Kosten. Im Rahmen der Risikoanalyse sollten Sie Daten zu Ihren aktuellen und geplanten Ausgaben für cloudbasierte Workloads sammeln, um zu ermitteln, wie hoch das Risiko ist und wie wichtig Investitionen in die Kostengovernance für Ihre Cloudeinführungsstrategie sind.

Es folgen Beispiele für nützliche Metriken, die Sie sammeln sollten, um die Risikotoleranz innerhalb der Disziplin der Sicherheitsbaseline zu bewerten:

- Jährliche Ausgaben: Die jährlichen Gesamtkosten für Dienste, die von einem Cloudanbieter bereitgestellt werden
- Monatliche Ausgaben: Die monatlichen Gesamtkosten für Dienste, die von einem Cloudanbieter bereitgestellt werden
- Verhältnis zwischen Prognose und tatsächlichen Ausgaben: Das Verhältnis zwischen den prognostizierten und den tatsächlichen Ausgaben (monatlich oder jährlich)
- Verhältnis der Einführungsgeschwindigkeit (MOM): Der Prozentsatz des Deltas zwischen den monatlichen Cloudkosten
- Akkumulierte Kosten: Akkumulierte Ausgaben auf Tagesbasis ab Monatsbeginn
- Ausgabentrends: Ausgabentrend in Relation zum Budget

## <a name="risk-tolerance-indicators"></a>Risikotoleranzindikatoren

In frühen Bereitstellungen, z.B. Dev/Test oder experimentellen ersten Workloads, ist die Kostenverwaltung wahrscheinlich mit vergleichsweise geringem Risiko verbunden. Wenn weitere Ressourcen bereitgestellt werden, steigt das Risiko, und die Toleranz im Unternehmens für Risiken nimmt wahrscheinlich ab. Darüber hinaus steigt das Risiko, und nimmt die Toleranz ab, wenn weitere Cloudeinführungsteams die Möglichkeit erhalten, Ressourcen in der Cloud zu konfigurieren oder bereitzustellen. Im Gegensatz dazu sind zum Erweitern einer Disziplin der Kostenverwaltung Personen aus der Cloudeinführungsphase erforderlich, die innovativere neue Technologien bereitstellen.

In den frühen Phasen der Cloudeinführung erarbeiten Sie zusammen mit Ihrem Unternehmen eine Baseline für Risikotoleranz. Nachdem Sie eine Basislinie haben, müssen Sie die Kriterien festlegen, die eine Investition in die Disziplin der Kostenverwaltung auslösen würden. Diese Kriterien sind wahrscheinlich für jede Organisation unterschiedlich.

Nachdem Sie [Geschäftsrisiken](./business-risks.md) ermittelt haben, identifizieren Sie gemeinsam mit dem Unternehmen Benchmarks, anhand derer Sie Auslöser identifizieren können, die diese Risiken potenziell erhöhen können. Es folgen einige Beispiele für den Vergleich von Metriken wie den oben genannten mit Ihrer Risikotoleranz-Baseline als Indikator, dass das Unternehmen weiter in die Kostenverwaltung investieren muss.

- Engagementgesteuert (am häufigsten): Ein Unternehmen, das sich vorgenommen hat, in diesem Jahr X Mio. USD für einen Cloudanbieter auszugeben. Es erfordert eine Kostenverwaltungsdisziplin, um sicherzustellen, dass das Unternehmen seine Ausgabenziele nicht um mehr als 20 % überschreitet und mindestens 90 % dieser Vorgabe verwendet.
- Prozentsatzauslöser: Ein Unternehmen mit Cloudausgaben, die für die Produktionssysteme stabil sind. Wenn sich dieser Wert um mehr als X % ändert, ist eine Kostenverwaltungsdisziplin eine sinnvolle Investition.
- Auslöser bei Überdimensionierung: Ein Unternehmen, das glaubt, dass die bereitgestellten Lösungen überdimensioniert sind. Kostenverwaltung ist eine Investition mit hoher Priorität, bis ein vernünftiger Abgleich zwischen Bereitstellung und Ressourcennutzung nachgewiesen werden kann.
- Auslöser bei monatlichen Ausgaben: Ein Unternehmen, das monatlich mehr als X Tausend USD ausgibt und dies als Kostenobergrenze ansieht. Wenn die Ausgaben diesen Betrag in einem bestimmten Monat übersteigen, muss eine Investition in Kostenverwaltung stattfinden.
- Auslöser bei jährlichen Ausgaben: Ein Unternehmen mit einem Budget für IT-Forschung und -Entwicklung, das Ausgaben von X Tausend USD pro Jahr für Cloudexperimente zulässt. Möglicherweise werden Produktionsworkloads in der Cloud ausgeführt, diese werden aber weiter als experimentelle Lösungen angesehen, wenn das Budget diesen Betrag nicht überschreitet. Sobald es weiter ansteigt, muss das Budget als Produktionsinvestition behandelt werden, und Ausgaben müssen strikt überwacht werden.
- OpEx-Ablehnung (selten): Das Unternehmen steht OpEx sehr negativ gegenüber, und es müssen Kostenverwaltungskontrollen eingerichtet werden, bevor eine Dev/Test-Workload bereitgestellt werden kann.

## <a name="next-steps"></a>Nächste Schritte

Dokumentieren Sie mit der Vorlage [Cloudverwaltung](./template.md) die Metriken und Toleranzindikatoren, die sich an dem aktuellen Cloudeinführungsplan orientieren.

Erstellen Sie anhand der Risiken und Toleranzen einen Prozess, mit dem Sie die Einhaltung der Richtlinie für die Kostenverwaltung steuern und kommunizieren.

> [!div class="nextstepaction"]
> [Festlegen von Prozessen für Richtlinienkonformität](compliance-processes.md)
