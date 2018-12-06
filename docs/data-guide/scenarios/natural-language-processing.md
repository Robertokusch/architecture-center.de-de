---
title: Verarbeitung natürlicher Sprache
description: ''
author: zoinerTejada
ms.date: 02/12/2018
ms.openlocfilehash: 32f2a5e3a0baddc765fb36ccc42fe2626faa5eba
ms.sourcegitcommit: e7e0e0282fa93f0063da3b57128ade395a9c1ef9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/05/2018
ms.locfileid: "52901438"
---
# <a name="natural-language-processing"></a>Verarbeitung natürlicher Sprache

Die Verarbeitung natürlicher Sprache (Natural Language Processing, NLP) wird für Aufgaben wie die Standpunktanalyse, Textgegenstandserkennung, Sprachenerkennung, Schlüsselbegriffserkennung und Dokumentkategorisierung verwendet.

![](./images/nlp-pipeline.png)

## <a name="when-to-use-this-solution"></a>Verwendung dieser Lösung

NLP kann zum Klassifizieren von Dokumenten verwendet werden, beispielsweise zur Kennzeichnung von Dokumenten als „vertraulich“ oder „Spam“. Die Ausgabe der NLP kann zur anschließenden Verarbeitung oder Suche verwendet werden. Eine weitere Verwendungsmöglichkeit der NLP ist die Zusammenfassung von Text durch Identifizieren der im Dokument vorhandenen Entitäten. Diese Entitäten können auch verwendet werden, um Dokumente mit Schlüsselwörtern zu markieren, sodass sie anhand des Inhalts gesucht und abgerufen werden können. Entitäten können zu Themen kombiniert und mit Zusammenfassungen versehen werden, in denen die wichtigen Themen in jedem Dokument beschrieben werden. Anhand der erkannten Themen können die Dokumente zur Navigation kategorisiert oder verwandte Dokumente für ein ausgewähltes Thema aufgelistet werden. Ein weiterer Verwendungszweck für die NLP ist die Standpunktbewertung von Texten, um die positive oder negative Stimmung eines Dokuments zu bewerten. Bei diesen Ansätzen werden viele Techniken der Verarbeitung natürlicher Sprache verwendet, beispielsweise: 

- **Tokenizer**. Aufteilung des Textes in Wörter oder Ausdrücke.
- **Wortstammerkennung und Lemmatisierung**. Normalisierung von Wörtern, sodass unterschiedliche Formen dem kanonischen Wort mit der gleichen Bedeutung entsprechen. Beispielweise werden „Ausführung“ und „ausgeführt“ dem Wort „ausführen“ zugeordnet. 
- **Entitätsextraktion**. Identifizieren von Themen im Text.
- **Wortarterkennung**. Erkennung von Text als Verb, Nomen, Partizip, Verbalphrase usw.
- **Erkennung von Satzgrenzen**. Erkennung von vollständigen Sätzen innerhalb von Textabschnitten.

Bei der Verwendung der NLP zum Extrahieren von Informationen und Einblicken aus Freitext sind meist die in Objektspeicher wie Azure Storage oder Azure Data Lake Store gespeicherten unformatierten Dokumente der Ausgangspunkt. 

## <a name="challenges"></a>Herausforderungen

- Die Verarbeitung einer Sammlung von Freitextdokumenten ist in der Regel rechen-, ressourcen- und zeitintensiv.
- Ohne ein standardisiertes Dokumentformat kann es sehr schwierig sein, durchweg genaue Ergebnisse zu erzielen, wenn die Freitextverarbeitung zum Extrahieren bestimmter Fakten aus einem Dokument verwendet wird. Wenn Sie beispielsweise eine Textdarstellung einer Rechnung haben, kann es schwierig sein, einen Prozess zu erstellen, der die Rechnungsnummer und das Rechnungsdatum für Rechnungen von einer beliebigen Anzahl von Lieferanten korrekt extrahiert.

## <a name="architecture"></a>Architecture

In einer NLP-Lösung wird die Freitextverarbeitung für Dokumente ausgeführt, die Textabschnitte enthalten. Die allgemeine Architektur kann auf [Batchverarbeitung](../big-data/batch-processing.md) oder [Echtzeit-Datenstromverarbeitung](../big-data/real-time-processing.md) beruhen.

Die tatsächliche Verarbeitung variiert je nach gewünschtem Ergebnis. Im Hinblick auf die Pipeline kann die NLP jedoch in einem Batch oder in Echtzeit angewendet werden kann. Beispielsweise kann die Standpunktanalyse für Textblöcke verwendet werden, um eine Stimmungspunktzahl zu erhalten. Die Verarbeitung kann in diesem Fall durch Ausführen eines Batchprozesses für Daten im Speicher oder in Echtzeit anhand von kleineren, durch einen Messagingdienst übermittelten Datenblöcken erfolgen.

## <a name="technology-choices"></a>Auswahl der Technologie

- [Verarbeitung natürlicher Sprache](../technology-choices/natural-language-processing.md)