---
title: Definieren von Anforderungen für resiliente Azure-Anwendungen
description: Übersicht über die Erfassung von Verfügbarkeits- und Resilienzanforderungen für Azure-Anwendungen
author: MikeWasson
ms.date: 04/10/2019
ms.topic: article
ms.service: architecture-center
ms.subservice: cloud-design-principles
ms.openlocfilehash: 2af2ce4200c285abfda1395862d21efc3c17e577
ms.sourcegitcommit: 579c39ff4b776704ead17a006bf24cd4cdc65edd
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/17/2019
ms.locfileid: "59642860"
---
# <a name="developing-requirements-for-resilient-azure-applications"></a>Entwickeln von Anforderungen für resiliente Azure-Anwendungen

Das Integrieren von *Resilienz* (Wiederherstellung bei Fehlern) und *Verfügbarkeit* (Ausführung in einem fehlerfreien Zustand ohne signifikante Ausfallzeit) in Ihre Apps beginnt mit der Erfassung von Anforderungen. Beispielsweise der Fragen: Wie viel Ausfallzeit ist akzeptabel? Welche Kosten fallen bei potenziellen Ausfallzeiten für Ihr Unternehmen an? Welche Verfügbarkeitsanforderungen stellen Ihre Kunden? Wie viel investieren Sie, um die Hochverfügbarkeit Ihrer Anwendung sicherzustellen? Welche Risiken stehen den Kosten gegenüber?

## <a name="identify-distinct-workloads"></a>Identifizieren verschiedener Workloads

Cloudlösungen bestehen typischerweise aus mehreren *Anwendungsworkloads*. Eine Workload ist eine bestimmte Funktion oder Aufgabe, die in Bezug auf die Geschäftslogik und die Datenspeicheranforderungen logisch von anderen Aufgaben getrennt ist. Eine E-Commerce-App kann beispielsweise die folgenden Workloads aufweisen:

- Suchen in einem Produktkatalog
- Erstellen und Nachverfolgen von Aufträgen
- Anzeigen von Empfehlungen

Jede Workload hat andere Anforderungen hinsichtlich Verfügbarkeit, Skalierbarkeit, Datenkonsistenz und Notfallwiederherstellung. Treffen Sie Ihre Geschäftsentscheidungen, indem Sie Kosten und Risiko für jede Workload abwägen.

Teilen Sie zudem Workloads nach Servicelevelziel auf. Wenn ein Dienst wichtige und weniger wichtige Workloads umfasst, verwalten Sie sie unterschiedlich, und legen Sie die Dienstfunktionen und die Anzahl der benötigten Instanzen entsprechend den jeweiligen Anforderungen an die Verfügbarkeit fest.

## <a name="plan-for-usage-patterns"></a>Planen der Verwendungsmuster

Ermitteln Sie Unterschiede bei den Anforderungen während kritischer und nicht kritischer Zeiträume. Gibt es bestimmte kritische Zeiträume, in denen das System verfügbar sein muss? Eine Anwendung für Steuererklärungen darf beispielsweise keinesfalls kurz vor dem Einreichungsdatum ausfallen, und bei einem Videostreamingdienst sollte es während eines Liveereignisses keine Verzögerungen geben. In diesen Situationen sollten Sie die Kosten gegen das Risiko abwägen.

- Um Betriebszeiten sicherzustellen und Vereinbarungen zum Servicelevel (Service Level Agreements, SLAs) in kritischen Zeiträumen einzuhalten, planen Sie Redundanz über mehrere Regionen hinweg ein, falls eine Region ausfällt, selbst wenn dies zu Mehrkosten führt.
- Während nicht kritischer Zeiträume können Sie Ihre Anwendung dagegen in einer einzelnen Region ausführen, um Kosten zu sparen.
- In einigen Fällen können Sie zusätzliche Kosten durch den Einsatz moderner serverloser Techniken mit verbrauchsbasierter Abrechnung reduzieren.

## <a name="establish-availability-and-recovery-metrics"></a>Ermitteln von Verfügbarkeits- und Wiederherstellungsmetriken

Erstellen Sie Baselinewerte für zwei Metriksätze als Teil des Anforderungsprozesses. Mithilfe des ersten Satzes können Sie ermitteln, wo Sie Redundanz zu Clouddiensten hinzufügen und welche SLAs Sie Kunden zur Verfügung stellen sollten. Der zweite Satz hilft Ihnen bei der Planung Ihrer Notfallwiederherstellung.

### <a name="availability-metrics"></a>Verfügbarkeitsmetriken

Verwenden Sie diese Maßnahmen, um Redundanz einzuplanen und Kunden-SLAs festzulegen.

- **Mean Time to Recover (MTTR)** ist der durchschnittliche Zeitraum, der zur Wiederherstellung einer Komponente nach einem Ausfall erforderlich ist.
- **Mean Time Between Failures (MTBF)** ist die Laufzeit, die für eine Komponente realistisch zwischen Ausfällen zu erwarten ist.

### <a name="recovery-metrics"></a>Wiederherstellungsmetriken

Führen Sie zum Ermitteln dieser Werte eine Risikobewertung durch, und machen Sie sich mit den Kosten und Risiken von Downtime und Datenverlusten vertraut. Diese sind nicht funktionsbezogene Anforderungen eines Systems und sollten von Geschäftsanforderungen vorgegeben werden.

- **Recovery Time Objective (RTO)** ist der maximal zulässige Zeitraum, in dem eine Anwendung nach einem Vorfall nicht verfügbar ist.
- **Recovery Point Objective (RPO)** ist die maximale Dauer eines Datenverlusts, die während eines Notfalls zulässig ist.

Wenn in einer Hochverfügbarkeitsumgebung der MTTR-Wert *einer beliebigen* kritischen Komponente über der RTO des Systems liegt, führt ein Fehler im System unter Umständen zu einer nicht akzeptablen Störung des Geschäftsbetriebs. Mit anderen Worten: Das System kann nicht innerhalb des definierten RTO-Zeitraums wiederhergestellt werden.

## <a name="determine-workload-availability-targets"></a>Bestimmen der Verfügbarkeitsziele für Workloads

Definieren Sie eigene Ziel-SLAs für die einzelnen Workloads in Ihrer Lösung, um bestimmen zu können, ob die Architektur die geschäftlichen Anforderungen erfüllt.

### <a name="consider-cost-and-complexity"></a>Berücksichtigen von Kosten und Komplexität

Sofern alle anderen Faktoren identisch sind, ist eine höhere Verfügbarkeit immer vorzuziehen. Aber je näher der angestrebte Wert bei 100% liegt, desto höher werden Kosten und Komplexität. Eine Betriebszeit von 99,99% entspricht etwa fünf Minuten Gesamtausfallzeit pro Monat. Ist das Erreichen einer weiteren Neun hinter dem Komma die zusätzliche Komplexität und die Mehrkosten wert? Die Antwort hängt von Ihren geschäftlichen Anforderungen ab.

Hier sind einige andere Aspekte aufgeführt, die für das Definieren einer Vereinbarung zum Servicelevel gelten:

- Wenn Sie 99,99% erreichen möchten, können Sie sich bei der Wiederherstellung nach Ausfällen nicht auf manuelle Eingriffe verlassen. Die Anwendung muss eine Selbstdiagnose und Selbstreparatur durchführen können.
- Im Bereich über 99,99% stellt es eine große Herausforderung dar, Ausfälle schnell genug zu erkennen, um die SLA-Anforderungen zu erfüllen.
- Betrachten Sie das Zeitfenster, auf das sich Ihre Vereinbarung zum Servicelevel bezieht. Je kleiner das Fenster, desto enger die Toleranzen. Es ist nicht sinnvoll, Ihre Vereinbarung zum Servicelevel in Bezug auf die stündliche oder tägliche Betriebszeit zu definieren.
- Betrachten Sie die MTBF- und MTTR-Messungen. Je höher Ihre SLA, desto häufiger kann der Dienst ausfallen und desto schneller muss der Dienst wiederhergestellt werden.
- Holen Sie die Zustimmung Ihrer Kunden zu den Verfügbarkeitszielen jedes Teils Ihrer Anwendung ein, und dokumentieren Sie dies. Andernfalls entspricht Ihr Entwurf möglicherweise nicht den Kundenerwartungen.

### <a name="identify-dependencies"></a>Ermitteln von Abhängigkeiten

Führen Sie Übungen zur Abhängigkeitszuordnung durch, um interne und externe Abhängigkeiten zu identifizieren. Beispiele umfassen Abhängigkeiten in Bezug auf Sicherheit oder Identität, wie Active Directory, oder Drittanbieterdienste, wie ein Zahlungsanbieter oder ein E-Mail-Messagingdienst.

Achten Sie besonders auf externe Abhängigkeiten, bei denen es sich um einen Single Points of Failure handeln kann oder die zu Engpässen führen können. Wenn für eine Workload eine Betriebszeit von 99,99% erforderlich ist, dafür aber eine Abhängigkeit von einem Dienst mit einer Vereinbarung zum Servicelevel mit 99,9% besteht, kann dieser Dienst im System kein Single Point of Failure sein. Eine Lösung hierfür ist die Verwendung eines Fallbackpfads für den Fall, dass der Dienst ausfällt. Alternativ können Sie andere Maßnahmen zur Wiederherstellung nach einem Ausfall des Diensts ergreifen.

In der folgenden Tabelle sind die potenziellen kumulativen Ausfallzeiten für verschiedene SLA-Ebenen angegeben.

| **SLA** | **Ausfallzeit pro Woche** | **Ausfallzeit pro Monat** | **Ausfallzeit pro Jahr** |
|---------|-----------------------|------------------------|-----------------------|
| 99 %     | 1,68 Stunden            | 7,2 Stunden              | 3,65 Tage             |
| 99,9 %   | 10,1 Minuten          | 43,2 Minuten           | 8,76 Stunden            |
| 99,95 %  | 5 Minuten             | 21,6 Minuten           | 4,38 Stunden            |
| 99,99 %  | 1,01 Minuten          | 4,32 Minuten           | 52,56 Minuten         |
| 99,999% | 6 Sekunden             | 25,9 Sekunden           | 5,26 Minuten          |

## <a name="understand-service-level-agreements"></a>Grundlegendes zu Vereinbarungen zum Servicelevel

In Azure wird in der [Vereinbarung zum Servicelevel (SLA)](https://azure.microsoft.com/en-us/support/legal/sla/) die garantierte Betriebszeit und Konnektivität beschrieben, die Microsoft zusichert. Wenn die Vereinbarung zum Servicelevel für einen bestimmten Dienst 99,9 % beträgt, können Sie erwarten, dass der Dienst 99,9 % der Zeit verfügbar ist. Unterschiedliche Dienste verfügen über verschiedene SLAs.

Die Vereinbarung zum Servicelevel von Azure enthält auch Bestimmungen zum Erhalt einer Gutschrift, falls die Vereinbarung nicht erfüllt wird, sowie bestimmte Definitionen zur *Verfügbarkeit* jedes Diensts. Dieser Aspekt der Vereinbarung zum Servicelevel fungiert als Durchsetzungsrichtlinie.

### <a name="composite-slas"></a>Zusammengesetzte SLAs

*Zusammengesetzte SLAs* umfassen mehrere Dienste, die eine Anwendung mit jeweils unterschiedlichen Verfügbarkeitsebenen unterstützen. Angenommen, eine App Service-Web-App führt Schreibvorgänge in eine Azure SQL-Datenbank durch. Zum Zeitpunkt der Erstellung dieses Artikels verfügen diese Azure-Dienste über die folgenden SLAs:

- App Service-Web-Apps = 99,95%
- SQL-Datenbank = 99,99%

Welche maximale Ausfallzeit erwarten Sie für diese Anwendung? Wenn einer der Dienste ausfällt, kommt es zu einem Ausfall der gesamten Anwendung. Die Wahrscheinlichkeit, dass jeder Dienst ausfällt, ist eine unabhängige Größe, und die zusammengesetzte Vereinbarung zum Servicelevel für diese Anwendung ist 99,95% x 99,99% = 99,94%. Dieser Wert ist niedriger als bei einzelnen SLAs. Dies ist nicht verwunderlich, da eine Anwendung, die von mehreren Diensten abhängig ist, über mehr potenzielle Fehlerpunkte verfügt.

Sie können die zusammengesetzte Vereinbarung zum Servicelevel verbessern, indem Sie unabhängige Fallbackpfade erstellen. Wenn SQL-Datenbank beispielsweise nicht verfügbar ist, können Transaktionen zur späteren Verarbeitung in eine Warteschlange eingereiht werden.

![Zusammengesetzte Vereinbarung zum Servicelevel](_images/composite-sla.png)

Bei diesem Aufbau ist die Anwendung auch dann noch verfügbar, wenn keine Verbindung mit der Datenbank hergestellt werden kann. Dies ist aber nicht mehr der Fall, wenn die Datenbank und die Warteschlange gleichzeitig ausfallen. Der erwartete Zeitprozentwert für einen gleichzeitigen Ausfall ist 0,0001 x 0,001, sodass die zusammengesetzte Vereinbarung zum Servicelevel für diesen kombinierten Pfad wie folgt lautet:

- Datenbank *oder* Warteschlange = 1,0 - (0,0001 x 0,001) = 99,99999%

Für die gesamte zusammengesetzte Vereinbarung zum Servicelevel gilt:

- Web-App *und* (Datenbank *oder* Warteschlange) = 99,95% x 99,99999% = \~99,95% (ungefährer Wert)

Dieser Ansatz hat auch Nachteile. Die Anwendungslogik ist komplexer, es fallen Kosten für die Warteschlange an, und ggf. müssen Probleme mit der Datenkonsistenz gelöst werden.

### <a name="slas-for-multiregion-deployments"></a>SLAs für Bereitstellung in mehreren Regionen

*SLAs für Bereitstellung in mehreren Regionen* umfassen eine Hochverfügbarkeitstechnik für die Bereitstellung der Anwendung in mehr als einer Region und die Verwendung von Azure Traffic Manager zum Durchführen eines Failovers, wenn die Anwendung in einer Region ausfällt.

Die zusammengesetzte Vereinbarung zum Servicelevel für eine Bereitstellung in mehreren Regionen wird wie folgt berechnet:

- *N* steht für die zusammengesetzte Vereinbarung zum Servicelevel für die Anwendung, die in einer Region bereitgestellt wird.
- *R* ist die Anzahl der Regionen, in denen die Anwendung bereitgestellt wird.

Die erwartete Wahrscheinlichkeit, dass die Anwendung in beiden Regionen gleichzeitig ausfällt, ist ((1 − N) \^ R). Beispiel: Wenn die SLA für die einzelne Region 99,95% ist, gilt:

- Kombinierte SLA für zwei Regionen: (1 − (1 − 0,9995) \^ 2) = 99,999975%
- Kombinierte SLA für vier Regionen: (1 − (1 − 0,9995) \^ 4)  = 99,999999%

Die [SLA für Traffic Manager](https://azure.microsoft.com/en-us/support/legal/sla/traffic-manager/v1_0/) ist ein weiterer Faktor. Das Failover in Aktiv/Passiv-Konfigurationen nicht sofort durchgeführt, sodass es während eines Failovers zu Ausfallzeiten kommen kann. Informationen hierzu finden Sie unter [Traffic Manager-Endpunktüberwachung und -failover](/azure/traffic-manager/traffic-manager-monitoring).

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Entwickeln einer Architektur für Resilienz und Verfügbarkeit](./architect.md)
