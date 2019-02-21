---
title: 'Mainframemigration: Mythen und Fakten'
description: Migrieren von Anwendungen aus Mainframe-Umgebungen zu Azure, einer bewährten, hoch verfügbaren und skalierbaren Infrastruktur für Systeme, die derzeit auf Mainframes ausgeführt werden.
author: njray
ms.date: 12/27/2018
ms.openlocfilehash: bcad01ec044d2d802b055e328a9496aae7b33311
ms.sourcegitcommit: 273e690c0cfabbc3822089c7d8bc743ef41d2b6e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/08/2019
ms.locfileid: "55901473"
---
# <a name="mainframe-myths-and-facts"></a>Mainframe: Mythen und Fakten

Mainframes ragen aus der historischen Entwicklung von Computern deutlich heraus und besitzen weiterhin ihre Berechtigung für hochspezifische Workloads. Man ist sich allgemein einige, dass Mainframes eine bewährte Plattform mit schon lange etablierten Betriebsverfahren darstellen, die sie zu zuverlässigen und robusten Umgebungen machen. Software wird basierend auf der Nutzung ausgeführt, gemessen in Millionen Anweisungen/Sek. (MIPS), und es stehen umfassende Nutzungsberichte zur Verfügung für eventuelle Gebührenrückerstattungen.

Die Zuverlässigkeit, Verfügbarkeit und Verarbeitungsleistung von Mainframes haben beinahe mythische Ausmaße angenommen. Um die Mainframe-Workloads zu evaluieren, die am besten für Azure geeignet sind, sollten Sie zuerst die Mythen von den Fakten trennen.

## <a name="myth-mainframes-never-go-down-and-have-a-minimum-of-five-9s-of-availability"></a>Mythos: Mainframes fallen nie aus und besitzen eine Verfügbarkeit von mindestens fünf 9en (99,999 %).

Mainframe-Hardware und -Betriebssysteme werden als zuverlässig und stabil angesehen. Aber die Realität sieht so aus, dass Ausfallzeiten für Wartung und Neustarts (auch als „Initial Program Load“ oder IPLs bezeichnet) eingeplant werden müssen. Wenn Sie diese Aufgaben berücksichtigen, besitzt eine Mainframelösung oft eher über eine Verfügbarkeit von zwei oder drei 9en, was der von Intel-basierten High-End-Servern entspricht.

Mainframes bleiben auch anfällig für Notfälle und Katastrophen wie jeder andere Server auch und erfordern unterbrechungsfreie Stromversorgungssysteme (UPS), um diese Arten von Ausfällen abzufangen.

## <a name="myth-mainframes-have-limitless-scalability"></a>Mythos: Mainframes besitzen unbegrenzte Skalierbarkeit.

Die Skalierbarkeit eines Mainframes hängt von der Kapazität seiner Systemsoftware ab, z. B. dem Kundeninformations-Kontrollsystem (Customer Informationen Control System, CICS) und der Kapazität neuer Instanzen von Mainframe-Engines und -Speicher. Einige große Unternehmen, die Mainframes verwenden, haben ihr CICS hinsichtlich der Leistung angepasst und sind ansonsten den Möglichkeiten des größten verfügbaren Mainframes entwachsen.

## <a name="myth-intel-based-servers-are-not-as-powerful-as-mainframes"></a>Mythos: Intel-basierte Servern sind nicht so leistungsfähig wie Mainframes.

Die neuen Intel-basierten Systeme mit hoher Core-Dichte haben so viel Rechenkapazität wie Mainframes.

## <a name="myth-the-cloud-cannot-accommodate-mission-critical-applications-for-large-companies-such-as-financial-institutions"></a>Mythos: Die Cloud kann unternehmenskritische Anwendungen für große Unternehmen wie Finanzinstitute nicht aufnehmen.

Es mag zwar vereinzelte Fälle geben, in denen Cloudlösungen nicht ausreichen, aber dies liegt normalerweise daran, dass die Anwendungsalgorithmen nicht verteilt werden können. Diesen wenigen Beispiele sind die Ausnahmen, nicht die Regel.

## <a name="summary"></a>Zusammenfassung

Azure bietet im Vergleich dazu eine alternative Plattform, die in der Lage ist, gleichwertige Mainframefunktionen und -features bereitzustellen, und dies bei wesentlich geringeren Kosten. Darüber hinaus sind die Gesamtbetriebskosten (TCO) des Kostenmodells der Cloud, das abonnementbasiert und nutzungsabhängig ist, weit weniger kostspielig als Mainframecomputer.

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Wechseln von Mainframes zu Azure](migration-strategies.md)
