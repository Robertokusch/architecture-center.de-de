---
title: 'Framework für die Cloudeinführung (Cloud Adoption Framework, CAF): Ansätze für die Planung digitaler Ressourcen'
titleSuffix: Microsoft Cloud Adoption Framework for Azure
ms.service: architecture-center
ms.subservice: enterprise-cloud-adoption
ms.custom: governance
description: Beschreibt einige Ansätze für die Planung digitaler Ressourcen
author: BrianBlanchard
ms.date: 12/10/2018
ms.topic: guide
ms.openlocfilehash: 8b6c680cf477ea188cbd263ecc04389d6daaed89
ms.sourcegitcommit: 273e690c0cfabbc3822089c7d8bc743ef41d2b6e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/08/2019
ms.locfileid: "55898016"
---
# <a name="approaches-to-digital-estate-planning"></a>Ansätze für die Planung digitaler Ressourcen

Die Planung digitaler Ressourcen kann je nach gewünschten Ergebnissen und Größe der vorhandenen Ressourcen auf unterschiedlichste Weise erfolgen. Zudem ist in Bezug auf den verwendeten Ansatz eine ganze Reihe von Optionen verfügbar. Es ist wichtig, die Erwartungen in Bezug auf den Ansatz in den frühen Planungszyklen festzulegen. Unklare Erwartungen führen häufig zu Verzögerungen bei weiteren Bestandserfassungen. In diesem Artikel werden drei Ansätze für die Analyse beschrieben.

## <a name="workload-driven-approach"></a>Workloadorientierter Ansatz

Beim Top-Down-Bewertungsansatz werden die Sicherheitsaspekte bewertet, beispielsweise Anforderungen im Hinblick auf die Kategorisierung von Daten (nach hoher, mittlerer oder niedriger Auswirkung auf das Unternehmen), die Konformität, die Datenhoheit und das Sicherheitsrisiko. Dieser Ansatz bewertet die allgemeine Komplexität der Architektur unter Berücksichtigung von Aspekten wie Authentifizierung, Datenstruktur, Anforderungen bezüglich der Wartezeit, Abhängigkeiten und Lebenserwartung der Anwendung. Außerdem misst der Top-Down-Ansatz die operativen Anforderungen der Anwendung, z. B. Servicelevels, Integration, Wartungsfenster, Überwachung und Erkenntnisse. Nachdem all diese Aspekte analysiert und einbezogen wurden, erhalten Sie ein Ergebnis bzw. Maß, das die relative Schwierigkeit der Migration der Anwendung zu den einzelnen Cloudplattformen wiedergibt: IaaS, PaaS und SaaS.

Die Top-Down-Bewertung beurteilt außerdem den finanziellen Nutzen der Anwendung, z. B. die betriebliche Effizienz, die Gesamtkosten, die Rendite und alle anderen zweckmäßigen finanziellen Metriken. Darüber hinaus fließen auch die Saisonalität der Anwendung (Jahreszeiten mit Bedarfsspitzen) und die allgemeine Computeauslastung in die Bewertung ein. Zudem werden die unterstützten Benutzertypen (gelegentliche Nutzer/Experten, immer/gelegentlich angemeldete Benutzer) und somit die erforderliche Skalierbarkeit und Elastizität überprüft. Abschließend werden bei der Bewertung die möglichen Anforderungen der Anwendung an die Geschäftskontinuität und Resilienz sowie Abhängigkeiten für die Ausführung der Anwendung im Fall einer Dienstunterbrechung geprüft.

> [!TIP]
> Dieser Ansatz erfordert Gespräche mit geschäftlichen und technischen Projektbeteiligten sowie individuelles Feedback zu ihren Erfahrungen. Die Verfügbarkeit der entscheidenden Personen ist das größte Risiko für die zeitliche Planung. Die Tatsache, dass die Datenquellen auf konkreten Daten beruhen, erschwert die Erstellung genauer Kosten- oder Zeitschätzungen. Planen Sie Zeitpläne frühzeitig, und überprüfen Sie alle erfassten Daten.

## <a name="asset-driven-approach"></a>Ressourcenorientierter Ansatz

Der ressourcenorientierte Ansatz liefert einen Plan, der auf den Ressourcen beruht, die die Migration einer Anwendung unterstützen. Bei diesem Ansatz werden statistische Nutzungsdaten aus einer Datenbank für die Konfigurationsverwaltung (Configuration Management Database, CMDB) oder anderen Tools zur Infrastrukturbewertung gepullt. Der Ansatz setzt in der Regel ein IaaS-Bereitstellungsmodell als Basis voraus. Bei diesem Prozess wertet die Analyse die Attribute jeder Ressource aus: Arbeitsspeicher, Anzahl von Prozessoren (CPU-Kerne), Speicherplatz für das Betriebssystem, Datenlaufwerke, Netzwerkschnittstellenkarten (NICs), IPv6, Netzwerklastenausgleich, Clustering, Version des Betriebssystems, Version der Datenbank (falls erforderlich), unterstützte Domänen, Komponenten oder Softwarepakete von Drittanbietern usw. Die bei diesem Ansatz inventarisierten Ressourcen werden dann zur Gruppierung und Abhängigkeitszuordnung mit Workloads oder Anwendungen abgeglichen.

> [!TIP]
> Dieser Ansatz erfordert eine umfassende Quelle von statistischen Nutzungsdaten. Der Zeitpunkt der Überprüfung des Bestands und Erfassung von Daten stellt das größte Risiko bei der zeitlichen Planung dar. Bei den Datenquellen auf niedriger Ebene können Abhängigkeiten zwischen Ressourcen oder Anwendungen fehlen. Planen Sie mindestens einen Monat für die Überprüfung des Bestands ein. Überprüfen Sie Abhängigkeiten vor der Bereitstellung.

## <a name="incremental-approach"></a>Inkrementeller Ansatz

Wie beim Framework für die Cloudeinführung wird ausdrücklich ein inkrementeller Ansatz empfohlen. Im Fall der Planung digitaler Ressourcen ergibt sich daraus der folgende aus mehreren Phasen bestehende Prozess:

- Anfängliche Kostenanalyse: Falls eine finanzielle Validierung erforderlich ist, beginnen Sie mit dem oben beschriebenen ressourcenorientierten Ansatz, um eine erste Kostenberechnung ohne Erklärung für sämtliche digitalen Ressourcen durchzuführen. Dadurch erhalten Sie eine Benchmark für das Worst-Case-Szenario.

- Migrationsplanung: Nachdem ein Cloudstrategie-Team zugewiesen wurde, erstellen Sie mit einem workloadbasierten Ansatz ein anfängliches Migrationsbacklog, das einzig auf ihrem kollektiven Wissen und den Erkenntnissen aus Gesprächen mit einer begrenzten Anzahl von Projektbeteiligten beruht. Mit diesem Ansatz erhalten Sie schnell eine einfache Workloadbewertung, auf deren Grundlage Sie die Zusammenarbeit fördern können.

- Releaseplanung: Bei jedem neuen Release wird das Migrationsbacklog gelöscht, und die Prioritäten werden erneut auf die wichtigsten geschäftlichen Auswirkungen ausgerichtet. Während dieses Prozesses werden die nächsten fünf bis zehn Workloads als priorisierte Releases ausgewählt. An diesem Punkt würde das für die Cloudstrategie zuständige Team Zeit in die Durchführung eines umfassenden workloadorientierten Ansatzes investieren. Wird diese Bewertung bis zur Abstimmung eines Releases verzögert, kommt dies den Projektbeteiligten im Hinblick auf ihren Zeitaufwand entgegen. Außerdem werden die vollständige Analyse und der damit einhergehende Aufwand hinausgezögert, bis die Ergebnisse der ersten Aktivitäten für das Unternehmen erkennbar sind.

- Ausführungsanalyse: Vor der Migration, Modernisierung oder Replikation sollte jede Ressource einzeln und als Teil eines gemeinsamen Releases bewertet werden. Zu diesem Zeitpunkt können die Daten aus dem ursprünglichen ressourcenorientierten Ansatz untersucht werden, um genaue Angaben zur Dimensionierung und zu den betrieblichen Einschränkungen sicherzustellen.

> [!TIP]
> Dieser inkrementelle Ansatz ermöglicht eine optimierte Planung und führt schneller zu Ergebnissen. Es ist sehr wichtig, dass alle Beteiligten den Ansatz der verzögerten Entscheidungsfindung verstehen. Ebenso wichtig ist es, dass die in den einzelnen Phasen getroffenen Annahmen dokumentiert werden, damit keine Informationen verloren gehen.

## <a name="next-steps"></a>Nächste Schritte

Nachdem ein Ansatz ausgewählt wurde, kann der Bestand erfasst werden.

> [!div class="nextstepaction"]
> [Gather inventory data](inventory.md) (Erfassen von Bestandsdaten)