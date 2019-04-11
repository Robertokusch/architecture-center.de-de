---
title: 'Framework für die Cloudeinführung (Cloud Adoption Framework, CAF): Rationalisieren der digitalen Ressourcen'
titleSuffix: Microsoft Cloud Adoption Framework for Azure
ms.service: architecture-center
ms.subservice: enterprise-cloud-adoption
ms.custom: governance
description: Bewerten Sie Ihre digitalen Ressourcen, um zu ermitteln, wie sich diese am besten in der Cloud hosten lassen.
author: BrianBlanchard
ms.date: 12/10/2018
ms.topic: guide
ms.openlocfilehash: d10e990192f2b52a2a9560c430d4bd34fb361a01
ms.sourcegitcommit: c053e6edb429299a0ad9b327888d596c48859d4a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/20/2019
ms.locfileid: "58242531"
---
# <a name="rationalize-the-digital-estate"></a>Rationalisieren der digitalen Ressourcen

Bei der Rationalisierung der Cloud werden Ressourcen evaluiert, um die beste Vorgehensweise für das Hosten in der Cloud zu ermitteln. Nachdem eine [Vorgehensweise](approach.md) ermittelt und der [Bestand](inventory.md) zusammengestellt wurde, kann die Rationalisierung beginnen. Unter [5 Rs of rationalization](5-rs-of-rationalization.md) (Die fünf R der Rationalisierung) werden die häufigsten Rationalisierungsoptionen beschrieben.

## <a name="traditional-view-of-rationalization"></a>Herkömmlicher Rationalisierungsansatz

Die Rationalisierung ist leicht verständlich, wenn Sie sich den herkömmlichen Rationalisierungsansatz wie eine komplexe Entscheidungsstruktur vorstellen. Jedes Element der digitalen Ressourcen durchläuft einen Prozess, an dessen Ende eine von fünf möglichen Antworten („5 R“) steht. Für kleinere Ressourcenumgebungen funktioniert dieser Prozess gut. Bei größeren Ressourcenumgebungen ist er aber nicht sehr effizient und kann zu erheblichen Verzögerungen führen. Wir sehen uns den Prozess nun an, um den Grund dafür zu verdeutlichen. Anschließend stellen wir ein effizienteres Modell vor.

**Bestand**: Ein umfassender Ressourcenbestand, z.B. mit Anwendungen, Software, Hardware, Betriebssystemen und Systemleistungsmetriken, ist erforderlich, um mithilfe von herkömmlichen Modellen eine vollständige Rationalisierung zu erzielen.

**Quantitative Analyse**: In der Entscheidungsstruktur bilden quantitative Fragen die Grundlage der ersten Entscheidungsebene. Häufig gestellte Fragen sind: Wird die Ressource derzeit genutzt? Wenn ja, ist sie optimiert, und hat sie die richtige Größe? Welche Abhängigkeiten bestehen zwischen Ressourcen? Diese Fragen sind für die Klassifizierung des Bestands von entscheidender Bedeutung.

**Qualitative Analyse**: Für die nächsten Entscheidungen wird menschliche Intelligenz in Form einer qualitativen Analyse benötigt. Häufig gelten diese Fragen nur speziell für die Lösung und können nur von Projektbeteiligten des Unternehmens und Hauptbenutzern beantwortet werden. Aufgrund dieser zu treffenden Entscheidungen wird der Prozess normalerweise verzögert, und es kommt zu einer erheblichen allgemeinen Verlangsamung. Bei dieser Analyse werden in der Regel 40 bis 80 Vollzeitmitarbeiter-Stunden pro Anwendung verbraucht. Eine Anleitung für die Erstellung einer Liste mit Fragen zur qualitativen Analyse finden Sie unter [Ansätze für die Planung digitaler Ressourcen](approach.md).

**Rationalisierungsentscheidung**: In den Händen eines erfahrenen Rationalisierungsteams können anhand der qualitativen und quantitativen Daten klare Entscheidungen getroffen werden. Leider ist die Einstellung und Schulung von Teams mit viel Erfahrung im Bereich der Rationalisierung ein teurer bzw. mehrere Monate dauernder Prozess.

## <a name="rationalization-at-enterprise-scale"></a>Rationalisierung auf Unternehmensebene

Dieser Prozess ist schon für eine digitale Ressourcenumgebung mit 50 virtuellen Computern zeitaufwändig und anspruchsvoll. Sie können sich sicherlich vorstellen, wie hoch dann der Aufwand für die Unternehmenstransformation in einer Umgebung mit Tausenden von virtuellen Computern und Hunderten von Anwendungen ist. Der erforderliche Mitarbeiteraufwand kann sich leicht auf mehr als 1.500 Vollzeitmitarbeiter-Stunden und neun Monate Planung belaufen.

Die vollständige Rationalisierung ist der gewünschte Endzustand und sicherlich ein anzustrebendes Ziel, aber bezogen auf die erforderliche Zeit und Energie ergibt sich nur selten eine gute Rendite.

Falls die Rationalisierung eine hohe Bedeutung für finanzielle Entscheidungen hat, kann die Inanspruchnahme eines auf Cloudrationalisierung spezialisierten Dienstleistungsunternehmens erwägt werden, um den Prozess zu beschleunigen. Die Durchführung einer vollständigen Rationalisierung kann trotzdem noch teuer und zeitaufwändig sein und zur Verzögerung der Transformation oder von Geschäftsergebnissen führen.

Im weiteren Verlauf dieses Artikels wird ein alternativer Ansatz beschrieben, der als inkrementelle Rationalisierung bezeichnet wird.

## <a name="incremental-rationalization"></a>Inkrementelle Rationalisierung

Die vollständige Rationalisierung von umfassenden digitalen Ressourcen ist risikoanfällig, und aufgrund der Komplexität kann es zu Verzögerungen kommen. Die Annahme beim inkrementellen Ansatz lautet, dass die Belastung für das Unternehmen durch verzögerte Entscheidungen gestaffelt werden kann, um das Risiko von Engpässen zu vermeiden. Bei diesem Ansatz wird im Laufe der Zeit ein natürliches Modell für die Entwicklung der erforderlichen Prozesse und der Benutzeroberfläche erstellt, damit fundierte Rationalisierungsentscheidungen auf effizientere Weise getroffen werden können.

### <a name="inventory-reduce-discovery-data-points"></a>Bestand: Reduzieren von Ermittlungsdatenpunkten

Nur sehr wenige Organisationen investieren die Zeit, Energie und Kosten, die erforderlich sind, um einen präzisen Echtzeitbestand für die digitalen Ressourcen zu vorzuhalten. Verlust, Diebstahl, Aktualisierungszyklen und der Einstellungsprozess von Mitarbeitern sind häufige Gründe, um die ausführliche Nachverfolgung der Ressourcen auf Endbenutzergeräten zu rechtfertigen. Die Rendite (Return on Investment, ROI), die sich aus der Verwaltung eines präzisen Servers und Anwendungsbestands in einem herkömmlichen lokalen Datencenter ergibt, ist aber häufig nur gering. Die meisten IT-Organisationen müssen dringendere Probleme als die Nachverfolgung der Nutzung von festen Ressourcen in einem Datencenter lösen.

Bei einer Cloudtransformation korreliert der Bestand direkt mit den Betriebskosten. Für eine korrekte Planung werden genaue Bestandsdaten benötigt. Unglücklicherweise können Entscheidungen aufgrund der aktuell verfügbaren Optionen zum Scannen von Umgebungen um Wochen oder Monate verzögert werden, wenn der gesamte Bestand gescannt und katalogisiert werden soll. Zum Glück gibt es einige Tricks, mit denen die Datensammlung beschleunigt werden kann.

Das Agent-basierte Scannen wird am häufigsten als Verzögerungsgrund genannt. Die stabilen Daten, die für eine herkömmliche Rationalisierung benötigt werden, sind häufig von Daten abhängig, die nur erfasst werden können, indem auf jeder Ressource ein Agent ausgeführt wird. Diese Abhängigkeit von Agents führt oft zu einer Verlangsamung des Prozesses, weil unter Umständen Rückmeldungen aus den Bereichen Sicherheit, Betrieb und Verwaltung erforderlich sind.

Bei einem inkrementellen Rationalisierungsprozess kann eine Lösung ohne Agents für eine erste Ermittlung genutzt werden, um schneller erste Entscheidungen treffen zu können. Je nach Komplexitätsebene in der Umgebung ist unter Umständen trotzdem noch eine Lösung mit Agents erforderlich, aber sie ist nicht mehr Teil des kritischen Pfads zur Weiterentwicklung des Unternehmens.

### <a name="quantitative-analysis-streamline-decisions"></a>Quantitative Analyse: Optimieren von Entscheidungen

Unabhängig vom Ansatz zur Ermittlung des Bestands kann die quantitative Analyse als Grundlage für verschiedene erste Entscheidungen und Annahmen dienen. Dies ist vor allem beim Identifizieren der ersten Workload der Fall, oder wenn das Ziel der Rationalisierung ein allgemeiner Kostenvergleich ist. Bei einem Prozess zur inkrementellen Rationalisierung beschränken die Teams für die Cloudstrategie und Cloudeinführung die [fünf R der Rationalisierung](5-rs-of-rationalization.md) auf zwei kurze Entscheidungen und wenden nur diese quantitativen Faktoren an, um die Analyse zu optimieren und die Menge der für die Weiterentwicklung benötigten Anfangsdaten zu reduzieren.

Wenn sich eine Organisation beispielsweise mitten in einer IaaS-Migration zur Cloud befindet, kann davon ausgegangen werden, dass die meisten Workloads entweder zurückgezogen oder neu gehostet werden.

### <a name="qualitative-analysis-temporary-assumptions"></a>Qualitative Analyse: Vorübergehende Annahmen

Indem die Anzahl von potenziellen Ergebnissen reduziert wird, ist es einfacher, eine erste Entscheidung zum zukünftigen Zustand einer Ressource zu treffen. Indem die Anzahl von Optionen verringert wird, verringert sich auch die Anzahl von Fragen, die vom Unternehmen in dieser frühen Phase beantwortet werden müssen.

Wenn im obigen Beispiel die Optionen in Bezug auf das erneute Hosten oder Zurückziehen begrenzt sind, muss sich das Unternehmen während der Rationalisierung nur eine Frage stellen, nämlich ob das Zurückziehen durchgeführt werden soll.

„Die Analyse hat ergeben, dass diese Ressource derzeit von keinem Benutzer aktiv verwendet wird. Stimmt dies, oder haben wir etwas übersehen?“ Eine Ja/Nein-Frage dieser Art kann mit einer qualitativen Analyse normalerweise deutlich einfacher beantwortet werden.

Mit diesem optimierten Ansatz werden Baselines, Finanzpläne, eine Strategie und eine Richtung festgelegt. Bei späteren Aktivitäten durchläuft jede Ressource dann eine weitere Rationalisierung und qualitative Analyse, um weitere Optionen zu evaluieren. Alle Annahmen, die bei dieser ersten Rationalisierung getroffen werden, werden vor der Implementierung auf ihre Richtigkeit überprüft.

## <a name="challenging-assumptions"></a>Hinterfragen der Annahmen

Das Ergebnis des vorherigen Abschnitts ist eine grobe Rationalisierung mit vielen Annahmen. Als Nächstes werden einige dieser Annahmen hinterfragt.

### <a name="retiring-assets"></a>Außerbetriebnehmen Ressourcen

In einer herkömmlichen lokalen Umgebung führt das Hosten kleiner, ungenutzter Ressourcen nur selten zu höheren jährlichen Kosten. Mit einigen Ausnahmen werden die Kosteneinsparungen, die sich aus dem Beschneiden und Außerbetriebnehmen dieser Ressourcen ergeben, durch den Vollzeitmitarbeiter-Aufwand für das Analysieren und Außerbetriebnehmen der jeweiligen Ressource wieder zunichtegemacht.

Bei der Umstellung auf ein Cloudabrechnungsmodell können durch das Außerbetriebnehmen von Ressourcen erhebliche Einsparungen bei den jährlichen Betriebskosten und beim Anschubaufwand für die Migration erzielt werden.

Es ist nicht ungewöhnlich, dass Organisationen mindestens 20% ihrer digitalen Ressourcen außer Betrieb nehmen, nachdem sie eine quantitative Analyse durchgeführt haben. Es wird empfohlen, eine weitere qualitative Analyse durchzuführen, bevor die Entscheidung in Bezug auf eine Maßnahme dieser Art getroffen wird. Nach der Bestätigung der Entscheidung kann die Außerbetriebnahme dieser Ressourcen zur ersten guten Rendite bei der Cloudmigration führen. In vielen Fällen ist dies einer der größten Faktoren zur Kosteneinsparung. Daher wird empfohlen, dass das Cloudstrategie-Team die Überprüfung und Außerbetriebnahme der Ressourcen parallel zur Erstellungsphase des Migrationsprozesses überwacht, um schnell finanzielle Erfolge zu erzielen.

### <a name="program-adjustments"></a>Anpassungen des Programms

Ein Unternehmen führt selten nur einen Transformationsprozess allein durch. Die Wahl zwischen Kostensenkung, Marktwachstum und neuen Umsatzchancen ist meist keine Ja/Nein-Entscheidung. Daher ist es ratsam, dass das Cloudstrategie-Team mit der IT-Abteilung zusammenarbeitet, um Ressourcen paralleler Transformationsprozesse zu ermitteln, die sich außerhalb des primären Transformationsprozesses befinden.

Für das in diesem Artikel verwendete Beispiel für die IaaS-Migration gilt Folgendes:

- Bitten Sie das DevOps-Team, Ressourcen zu identifizieren, die bereits Teil einer Bereitstellungsautomatisierung sind, und entfernen Sie diese aus dem Hauptmigrationsplan.

- Bitten Sie die für Daten sowie für Forschung und Entwicklung zuständigen Teams, Ressourcen zu identifizieren, die als Basis für neue Umsatzchancen dienen, und diese aus dem Hauptmigrationsplan zu entfernen.

Diese programmbezogene qualitative Analyse kann schnell durchgeführt werden und sorgt für eine Vereinheitlichung über mehrere Migrationsbacklogs hinweg.

Einige Ressourcen müssen unter Umständen noch einige Zeit wie erneut gehostete Ressourcen behandelt werden, für die die Rationalisierung dann erst später nach der ersten Migration erfolgt.

## <a name="selecting-the-first-workload"></a>Auswählen der ersten Workload

Die Implementierung der ersten Workload ist für das Testen und den Lernprozess von entscheidender Bedeutung. Es ist die erste Möglichkeit, die Chancen für Umsatzwachstum darzustellen und zu verinnerlichen.

### <a name="business-criteria"></a>Geschäftliche Kriterien

Identifizieren Sie eine Workload, die von einem Mitglied der Geschäftseinheit des Cloudstrategie-Teams unterstützt wird, um die geschäftliche Transparenz sicherzustellen. Es sollte sich vorzugsweise um eine Workload handeln, an der das Team beteiligt ist und für die eine hohe Motivation für die Verlagerung in die Cloud besteht.

### <a name="technical-criteria"></a>Technische Kriterien

Wählen Sie eine Workload aus, die nur über wenige Abhängigkeiten verfügt und als kleine Ressourcengruppe verschoben werden kann. Es ist ratsam, eine Workload mit einem definierten Testpfad auszuwählen, um die Validierung zu vereinfachen.

Die erste Workload wird häufig in einer Experimentierumgebung ohne Betriebs- oder Governance-Kapazität bereitgestellt. Es ist sehr wichtig, eine Workload auszuwählen, die nicht mit geschützten Daten interagiert.

### <a name="qualitative-analysis"></a>Qualitative Analyse

Die Teams für die Cloudeinführung und Cloudstrategie können zusammenarbeiten, um diese kleine Workload zu analysieren. Hierbei wird eine kontrollierte Möglichkeit zum Erstellen und Testen der Kriterien für die qualitative Analyse geschaffen. Aufgrund des geringeren Umfangs können die betroffenen Benutzer überwacht werden, um innerhalb eines Zeitraums von maximal einer Woche eine ausführliche qualitative Analyse durchzuführen. Informationen zu häufigen Faktoren bei der qualitativen Analyse finden Sie unter dem jeweiligen Rationalisierungsziel im Artikel [5 Rs of Rationalization](5-rs-of-rationalization.md) (Die fünf R der Rationalisierung).

### <a name="migration"></a>Migration

Parallel zur laufenden Rationalisierung kann das Team für die Cloudeinführung damit beginnen, die kleine Workload zu migrieren, um den Lernprozess in den folgenden wichtigen Bereichen zu fördern:

- Stärken der Fähigkeiten in Bezug auf die Plattform des Cloudanbieters
- Definieren der erforderlichen Kerndienste (und Azure-Standards) für die langfristige Vision
- Entwickeln eines besseren Verständnisses, wie Vorgänge im Verlauf der Transformation weiterentwickelt werden müssen
- Verstehen der inhärenten geschäftlichen Risiken und die damit verbundene Toleranz des Unternehmens
- Festlegen einer Baseline oder eines Produkts mit den Governance-Mindestanforderungen gemäß der Risikotoleranz des Unternehmens

## <a name="release-planning"></a>Releaseplanung

Während das Team für die Cloudeinführung die Migration oder Implementierung der ersten Workload durchführt, kann das Team für die Cloudstrategie mit dem Priorisieren der restlichen Anwendungen bzw. Workloads beginnen.

### <a name="power-of-ten"></a>Zehn Anwendungen

Beim herkömmlichen Rationalisierungsansatz wird versucht, das Unmögliche möglich zu machen. Glücklicherweise wird zum Starten eines Transformationsprozesses meist nicht ein separater Plan für jede Anwendung benötigt. Bei einem inkrementellen Modell ist der Ansatz „Zehn Anwendungen“ ein guter Ausgangspunkt. Hierbei wählt das Team für die Cloudstrategie die ersten zehn Anwendungen für die Migration aus. Diese zehn Workloads sollten ein Mischung aus einfachen und komplexen Workloads darstellen.

### <a name="building-the-first-backlogs"></a>Erstellen des ersten Backlogs

Die Teams für die Cloudeinführung und Cloudstrategie können bei der qualitativen Analyse für die ersten zehn Workloads zusammenarbeiten. Hierbei werden das erste mit Priorität versehene Migrationsbacklog und das erste mit Priorität versehene Releasebacklog erstellt. Bei diesem Ansatz können die Teams den Prozess gemeinsam durchlaufen und haben genügend Zeit, um einen geeigneten Prozess für die qualitative Analyse zu erstellen.

### <a name="maturing-the-process"></a>Weiterentwickeln des Prozesses

Nachdem sich die beiden Teams auf die Kriterien für die qualitative Analyse geeignet haben, kann die Bewertung zu einer Aufgabe bei jeder Iteration werden. Zur Erreichung eines Konsens in Bezug auf die Bewertungskriterien werden normalerweise zwei bis drei Releases benötigt.

Nachdem die Bewertung in die inkrementellen Ausführungsprozesse der Migration verschoben wurde, kann das Team für die Cloudeinführung die Bewertung und die Architektur schneller durchlaufen. An diesem Punkt wird das Team für die Cloudstrategie außerdem abstrahiert, um den Zeitaufwand zu reduzieren. Darüber hinaus kann sich das Team für die Cloudstrategie auf das Priorisieren der Anwendungen konzentrieren, die noch nicht in einem bestimmten Release enthalten sind. So kann eine enge Abstimmung auf sich ändernde Marktbedingungen sichergestellt werden.

Nicht alle priorisierten Anwendungen sind bereit für die Migration. Die Sequenzierung ändert sich wahrscheinlich, wenn das Team eine eingehendere qualitative Analyse durchführt und Geschäftsereignisse und Abhängigkeiten ermittelt, die eine erneute Priorisierung des Backlogs erforderlich machen. Für einige Releases wird ggf. eine geringe Anzahl von Workloads gruppiert. Andere können auch nur eine Workload enthalten.

Das Team für die Cloudeinführung führt wahrscheinlich Iterationen durch, bei denen sich keine vollständige Workloadmigration ergibt. Je kleiner die Workload und je kleiner die Anzahl von Abhängigkeiten ist, desto höher ist die Wahrscheinlichkeit, dass eine Workload in einen einzelnen Sprint bzw. eine Iteration passt. Aus diesem Grund wird empfohlen, die ersten Anwendungen im Releasebacklog klein zu halten und auf wenige externe Abhängigkeiten zu beschränken.

## <a name="end-state"></a>Endzustand

Im Laufe der Zeit führen das Team für die Cloudeinführung und das Team für die Cloudstrategie gemeinsam eine vollständige Rationalisierung des Bestands durch. Bei diesem inkrementellen Ansatz können die Teams den Rationalisierungsprozess aber immer schneller abarbeiten. Außerdem ergeben sich aus dem Transformationsprozess eher greifbare Geschäftsergebnisse, ohne dass zuerst ein hoher Analyseaufwand anfällt.

In einigen Fällen ist das Finanzmodell ggf. zu eng gefasst, um ohne weitere Rationalisierungsmaßnahmen eine Entscheidung zur weiteren Vorgehensweise treffen zu können. Wenn dies so ist, ist unter Umständen eher ein herkömmlicher Rationalisierungsansatz erforderlich.

## <a name="next-steps"></a>Nächste Schritte

Das Ergebnis eines Rationalisierungsprozesses ist ein priorisiertes Backlog mit allen Ressourcen, die von der jeweiligen Transformation betroffen sein werden. Dieses Backlog kann dann als Grundlage für Kostenmodelle von Clouddiensten dienen.

> [!div class="nextstepaction"]
> [Align cost models with the digital estate](calculate.md) (Ausrichten von Kostenmodellen auf die digitalen Ressourcen)