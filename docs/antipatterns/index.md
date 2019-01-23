---
title: Leistungsbezogene Antimuster für Cloudanwendungen
titleSuffix: Azure Architecture Center
description: Häufige Vorgehensweisen, die wahrscheinlich zu Skalierbarkeitsproblemen führen.
author: dragon119
ms.date: 06/05/2017
ms.topic: article
ms.service: architecture-center
ms.subservice: cloud-fundamentals
ms.custom: seodec18
ms.openlocfilehash: 212930368942728fc0be0c9b2af1a90293906b39
ms.sourcegitcommit: 1b50810208354577b00e89e5c031b774b02736e2
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/23/2019
ms.locfileid: "54483053"
---
# <a name="performance-antipatterns-for-cloud-applications"></a>Leistungsbezogene Antimuster für Cloudanwendungen

Ein *leistungsbezogenes Antimuster* ist eine häufige Vorgehensweise, die wahrscheinlich zu Skalierbarkeitsproblemen führt, wenn eine Anwendung überlastet ist.

Ein häufiges Szenario: Eine Anwendung reagiert während der Leistungstests gut. Dann wird sie in der Produktionsumgebung eingesetzt und beginnt damit, echte Workloads zu verarbeiten. Ab diesem Zeitpunkt wird die Leistung unzureichend – die Anwendung lehnt Benutzeranforderungen ab, bleibt hängen oder gibt Ausnahmen aus. Jetzt sieht sich das Entwicklungsteam mit zwei Fragen konfrontiert:

- Warum hat sich dieses Verhalten beim Testen nicht gezeigt?
- Wie beheben wir es?

Die Antwort auf die erste Frage ist einfach. Es ist sehr schwierig, in einer Testumgebung echte Benutzer, ihre Verhaltensmuster und den Umfang an Verarbeitungsaufgaben zu simulieren, den die Benutzer ausführen werden. Der einzig wirklich sichere Weg, um zu verstehen, wie sich ein System unter Last verhält, besteht darin, es in der Produktionsumgebung zu beobachten. Verstehen Sie uns nicht falsch: Wir empfehlen nicht, Leistungstests zu überspringen. Leistungstests sind von entscheidender Bedeutung, um Grundwerte für Leistungsmetriken zu erhalten. Sie müssen sich jedoch darauf vorbereiten, Ihr Livesystem zu beobachten und ggf. dort auftretende Leistungsprobleme zu beheben.

Die Antwort auf die zweite Frage – wie lässt sich das Problem beheben? – ist weniger einfach. Hier spielt eine Vielzahl von Faktoren eine Rolle, und zuweilen manifestiert sich ein Problem erst unter bestimmten Umständen. Instrumentierung und Protokollierung sind entscheidend, um die Ursache eines Problems zu finden – aber Sie müssen auch wissen, wonach Sie suchen.

Basierend auf dem Feedback, das wir von Microsoft Azure-Kunden erhalten, haben wir einige der häufigsten Leistungsprobleme ermittelt, die von Kunden in ihren Produktionsumgebungen beobachtet werden. Für jedes Antimuster beschreiben wir, warum das Antimuster typischerweise auftritt, sowie die Symptome des Antimusters und Techniken zum Lösen des Problems. Wir stellen auch Beispielcode bereit, der sowohl das Antimuster als auch die vorgeschlagene Lösung veranschaulicht.

Einige dieser Antimuster mögen allzu offensichtlich erscheinen, wenn Sie die Beschreibungen lesen, treten aber häufiger auf, als Sie vielleicht glauben. Manchmal erbt eine Anwendung ein Design, das lokal wunderbar funktioniert hat, sich aber nicht in die Cloud skalieren lässt. Es kann auch passieren, dass eine Anwendung mit einem sehr sauberen Design entworfen wird, sich aber das ein oder andere Antimuster einschleicht, wenn neue Features hinzugefügt werden. Unabhängig von den Gründen – dieser Leitfaden hilft Ihnen dabei, diese Antimuster zu finden und zu beheben.

Hier finden Sie die Liste der Antimuster, die wir identifiziert haben:

| Antimuster | BESCHREIBUNG |
|-------------|-------------|
| [Ausgelastete Datenbank][BusyDatabase] | Das Auslagern zu vieler Verarbeitungsprozesse an einen Datenspeicher. |
| [Ausgelastetes Front-End][BusyFrontEnd] | Das Verschieben ressourcenintensiver Tasks in Hintergrundthreads. |
| [Zu viele E/A-Vorgänge][ChattyIO] | Das kontinuierliche Senden vieler kleiner Netzwerkanforderungen. |
| [Irrelevante Abrufe][ExtraneousFetching] | Das Abrufen einer größeren Datenmenge als erforderlich, was zu unnötigen E/A-Vorgängen führt. |
| [Ungeeignete Instanziierung][ImproperInstantiation] | Wiederholtes Erstellen und Zerstören von Objekten, die für die gemeinsame Nutzung und Wiederverwendung entworfen wurden. |
| [Monolithische Persistenz][MonolithicPersistence] | Verwenden des gleichen Datenspeichers für Daten mit sehr unterschiedlichen Nutzungsmustern. |
| [Kein Caching][NoCaching] | Daten werden nicht zwischengespeichert. |
| [Synchrone E/A-Vorgänge][SynchronousIO] | Blockieren des aufrufenden Threads, während E/A-Vorgänge abgeschlossen werden. |

[BusyDatabase]: ./busy-database/index.md
[BusyFrontEnd]: ./busy-front-end/index.md
[ChattyIO]: ./chatty-io/index.md
[ExtraneousFetching]: ./extraneous-fetching/index.md
[ImproperInstantiation]: ./improper-instantiation/index.md
[MonolithicPersistence]: ./monolithic-persistence/index.md
[NoCaching]: ./no-caching/index.md
[SynchronousIO]: ./synchronous-io/index.md
