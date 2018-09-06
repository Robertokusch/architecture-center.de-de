---
title: Verwenden des besten Datenspeichers für den Auftrag
description: Wählen Sie die Speichertechnologie aus, die sich am besten für Ihre Daten und den vorgesehenen Einsatzzweck eignet.
author: MikeWasson
ms.date: 08/30/2018
ms.openlocfilehash: 25839f5a749881f415c923db5497984d32b8ac91
ms.sourcegitcommit: ae8a1de6f4af7a89a66a8339879843d945201f85
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/31/2018
ms.locfileid: "43326090"
---
# <a name="use-the-best-data-store-for-the-job"></a>Verwenden des besten Datenspeichers für den Auftrag

## <a name="pick-the-storage-technology-that-is-the-best-fit-for-your-data-and-how-it-will-be-used"></a>Wählen Sie die Speichertechnologie aus, die sich am besten für Ihre Daten und den vorgesehenen Einsatzzweck eignet.

Die Tage, in denen Sie einfach all Ihre Daten in einer großen relationalen SQL-Datenbank gespeichert haben, sind vorbei. Relationale Datenbanken eignen sich ausgezeichnet für ihre Einsatzzwecke – Bereitstellen von ACID-Garantien (Atomarität, Konsistenz, Isolation, Dauerhaftigkeit) für Transaktionen mit relationalen Daten. Allerdings sind sie auch mit einigen Nachteilen verbunden:

- Abfragen erfordern möglicherweise teure Joins.
- Daten müssen normalisiert werden und einem vordefinierten Schema entsprechen (Schema-on-Write).
- Sperrkonflikte können die Leistung beeinträchtigen.

In umfangreichen Lösungen ist es wahrscheinlich, dass eine einzige Datenspeichertechnologie nicht alle Anforderungen erfüllen kann. Es gibt Alternativen zu relationalen Datenbanken, beispielsweise Schlüssel-Wert-Speicher, Dokumentdatenbanken, Datenbanken für Suchmaschinen, Zeitreihendatenbanken, auf Spaltenfamilien basierende Datenbanken und Diagrammdatenbanken. Jede dieser Alternativen bietet Vor- und Nachteile, und je nach Datentyp ist die eine oder die andere Alternative besser geeignet. 

Einen Produktkatalog sollten Sie z.B. in einer Dokumentdatenbank wie Cosmos DB speichern, die ein flexibles Schema zulässt. In diesem Fall ist jede Produktbeschreibung ein eigenständiges Dokument. Um Abfragen im gesamten Katalog durchführen zu können, können Sie den Katalog indizieren und den Index in Azure Search speichern. Für den Produktbestand eignet sich eine SQL-Datenbank, da die Daten ACID-Garantien erfordern.

Denken Sie daran, dass zu den Daten mehr gehört als nur die dauerhaft gespeicherten Anwendungsdaten. Daten umfassen auch Anwendungsprotokolle, Ereignisse, Nachrichten und Caches.

## <a name="recommendations"></a>Empfehlungen

**Verwenden Sie nicht für alle Daten eine relationale Datenbank**. Wählen Sie andere Datenspeicher aus, wenn diese sich besser eignen. Weitere Informationen finden Sie unter [Auswählen des richtigen Datenspeichers][data-store-overview].

**Nutzen Sie mehrsprachige Persistenz**. In umfangreichen Lösungen ist es wahrscheinlich, dass eine einzige Datenspeichertechnologie nicht alle Anforderungen erfüllen kann. 

**Berücksichtigen Sie den Datentyp**. Einige Beispiele: Speichern Sie Transaktionsdaten in SQL, JSON-Dokumente in einer Dokumentdatenbank, Telemetriedaten in einer Zeitreihendatenbank, Anwendungsprotokolle in Elasticsearch und Blobs in Azure Blob Storage.

**Geben Sie der Verfügbarkeit den Vorzug vor (strikter) Konsistenz**. Das CAP-Theorem impliziert, dass in einem verteilten System Kompromisse hinsichtlich Verfügbarkeit und Konsistenz eingegangen werden müssen. (Netzwerkpartitionen,– die dritte Seite des CAP-Theorems – lassen sich nie ganz vermeiden.) Häufig lässt sich durch Einsatz eines Modells der *letztlichen Konsistenz* eine höhere Verfügbarkeit erzielen. 

**Berücksichtigen Sie die Kompetenz des Entwicklungsteams**. Die mehrsprachige Persistenz bietet Vorteile, kann aber auch zu weit gehen. Die Anwendung einer neuen Datenspeichertechnologie erfordert neue Kenntnisse und Fähigkeiten. Das Entwicklungsteam muss wissen, wie sich die Technologie am besten einsetzen lässt. Das Team muss verstehen, welche Nutzungsmuster sich für die Technologie eignen, und wissen, wie Abfragen und Leistung optimiert werden, usw. Beziehen Sie diesen Aspekt in Ihre Überlegungen zu Speichertechnologien ein. 

**Verwenden Sie ausgleichende Transaktionen**. Eine Nebenwirkung der mehrsprachigen Persistenz ist, dass eine einzelne Transaktion möglicherweise Daten in mehrere Speicher schreibt. Wenn etwas nicht funktioniert, verwenden Sie ausgleichende Transaktionen, um alle bereits abgeschlossenen Schritte rückgängig zu machen.

**Sehen Sie sich Kontextgrenzen an**. Der Begriff *Kontextgrenze* stammt aus dem Bereich „Domain-Driven Design“. Eine Kontextgrenze ist eine explizite Grenze um ein Domänenmodell und definiert die Teile der Domäne, für die das Modell gilt. Idealerweise wird eine Kontextgrenze einer Subdomäne der Geschäftsdomäne zugeordnet. Die Kontextgrenzen in Ihrem System bieten sich für mehrsprachige Persistenz an. „Produkte“ treten sowohl in der Subdomäne mit dem Produktkatalog als auch der Subdomäne mit dem Produktbestand auf – es ist allerdings sehr wahrscheinlich, dass beide Subdomänen unterschiedliche Anforderungen an die Speicherung, Aktualisierung und Abfrage von Produkten aufweisen.

[data-store-overview]: ../technology-choices/data-store-overview.md