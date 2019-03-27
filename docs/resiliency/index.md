---
title: Entwerfen robuster Anwendungen für Azure
description: 'Es wird beschrieben, wie Sie in Azure robuste Anwendungen mit Hochverfügbarkeit und Notfallwiederherstellung erstellen.'
author: MikeWasson
ms.date: 12/18/2018
ms.topic: article
ms.service: architecture-center
ms.subservice: cloud-design-principles
ms.custom: resiliency
---

# <a name="designing-resilient-applications-for-azure"></a>Entwerfen robuster Anwendungen für Azure

In einem verteilten System kommt es unweigerlich zu Fehlern. Hardware kann ausfallen. Im Netzwerk können vorübergehende Fehler auftreten. In seltenen Fällen kann ein gesamter Dienst oder eine Region ausfallen, aber auch hierfür muss eine Planung vorhanden sein.

Das Erstellen einer zuverlässigen Anwendung in der Cloud unterscheidet sich vom Erstellen einer zuverlässigen Anwendung in einer Unternehmensumgebung. Bisher haben Sie höherwertige Hardware gekauft, um zentral hochzuskalieren, aber in einer Cloudumgebung müssen Sie stattdessen horizontal hochskalieren. Die Kosten für Cloudumgebungen werden niedrig gehalten, indem Standardhardware eingesetzt wird. Das Ziel besteht nicht darin, Fehler ganz zu verhindern, sondern die Auswirkungen eines Fehlers im System zu minimieren.

Dieser Artikel enthält eine Übersicht über die Erstellung von robusten Anwendungen in Microsoft Azure. Zuerst werden der Begriff *Resilienz* und die dazugehörigen Konzepte definiert. Anschließend wird der Prozess zur Erreichung von Resilienz beschrieben. Hierzu wird ein strukturierter Ansatz für die Lebensdauer einer Anwendung verwendet – vom Entwurf und der Implementierung über die Bereitstellung bis zum Betrieb.

<!-- markdownlint-disable MD026 -->

## <a name="what-is-resiliency"></a>Was ist Resilienz?

<!-- markdownlint-enable MD026 -->

Bei der **Resilienz** (Robustheit) geht es um die Möglichkeit, nach Ausfällen für ein System eine Wiederherstellung durchzuführen und die Betriebsbereitschaft sicherzustellen. Es geht nicht um das *Vermeiden* von Ausfällen, sondern um das *Reagieren* auf Ausfälle, um Ausfallzeiten oder Datenverluste zu vermeiden. Das Ziel besteht bei der Resilienz darin, für die Anwendung nach einem Ausfall wieder die volle Funktionsfähigkeit herzustellen.

Zwei wichtige Aspekte der Resilienz sind Hochverfügbarkeit und Notfallwiederherstellung.

- **Hochverfügbarkeit** ist die Möglichkeit, die Anwendung ohne signifikante Ausfallzeit in einem fehlerfreien Zustand weiterzubetreiben. Mit „fehlerfreier Zustand“ ist gemeint, dass die Anwendung reagiert und Benutzer eine Verbindung mit der Anwendung herstellen und damit interagieren können.  
- Die **Notfallwiederherstellung** (Disaster Recovery, DR) ist die Möglichkeit, nach seltenen, schwerwiegenderen Vorfällen (dauerhafte Ausfälle größeren Ausmaßes, z.B. eine Dienstunterbrechung in einer gesamten Region) eine Wiederherstellung durchzuführen. Die Notfallwiederherstellung umfasst die Datensicherung und -archivierung und kann auch einen manuellen Eingriff beinhalten, z.B. das Wiederherstellen einer Datenbank aus einer Sicherung.

Sie können sich die Hochverfügbarkeit und Notfallwiederherstellung beispielsweise wie folgt vorstellen: Die Notfallwiederherstellung beginnt, wenn ein Fehler so große Auswirkungen hat, dass diese von der Einrichtung für Hochverfügbarkeit nicht mehr bewältigt werden können.  

Beim Ausführen der Entwurfsschritte für die Resilienz müssen Sie sich über Ihre Anforderungen an die Verfügbarkeit im Klaren sein. Wie viel Ausfallzeit ist akzeptabel? Dies ist teilweise eine Kostenfunktion. Welche Kosten fallen bei potenziellen Ausfallzeiten für Ihr Unternehmen an? Wie viel Geld sollten Sie investieren, um für die Anwendung die Hochverfügbarkeit sicherzustellen? Außerdem müssen Sie definieren, was die Verfügbarkeit der Anwendung bedeutet. Gilt die Anwendung beispielsweise als „ausgefallen“, wenn ein Kunde eine Bestellung senden kann, diese aber vom System nicht innerhalb des normalen Zeitrahmens verarbeitet werden kann? Berücksichtigen Sie außerdem die Wahrscheinlichkeit eines bestimmten Ausfalltyps und die Kosteneffektivität einer Lösungsstrategie.

Ein weiterer häufig verwendeter Begriff ist **Geschäftskontinuität** (Business Continuity, BC). Hierbei geht es um die Möglichkeit, auch bei bzw. nach widrigen Umständen, z.B. einer Naturkatastrophe oder einem Dienstausfall, wichtige Geschäftsfunktionen aufrechterhalten zu können. Die Geschäftskontinuität erstreckt sich auf den gesamten Betrieb des Unternehmens, z.B. physische Einrichtungen, Personal, Kommunikation, Transport und IT. In diesem Artikel geht es um Cloudanwendungen, aber die Resilienzplanung muss in Bezug auf alle Anforderungen der Geschäftskontinuität durchgeführt werden.

**Datensicherung** ist ein wichtiger Teil der Notfallwiederherstellung. Wenn für die zustandslosen Komponenten einer Anwendung ein Fehler auftritt, können Sie sie immer wieder neu bereitstellen. Falls aber Daten verloren gehen, kann für das System kein stabiler Zustand mehr hergestellt werden. Daten müssen gesichert werden, und zwar idealerweise in einer anderen Region, falls ein Katastrophenfall eine gesamte Region betrifft.

Die Sicherung unterscheidet sich von der *Datenreplikation*. Die Datenreplikation umfasst das Kopieren von Daten nahezu in Echtzeit, sodass das System schnell ein Failover zu einem Replikat durchführen kann. Viele Datenbankensysteme unterstützen die Replikation. Beispielsweise verfügt SQL Server über Unterstützung für SQL Server Always On-Verfügbarkeitsgruppen. Mit der Datenreplikation kann reduziert werden, wie lange die Wiederherstellung nach einem Ausfall dauert, indem sichergestellt wird, dass immer ein Replikat der Daten verfügbar ist. Die Datenreplikation bietet aber keinen Schutz vor menschlichen Fehlern. Falls Daten aufgrund eines menschlichen Fehlers beschädigt werden, werden sie einfach auf die Replikate kopiert. Aus diesem Grund benötigen Sie für Ihre Strategie für die Notfallwiederherstellung eine langfristige Sicherungslösung.

## <a name="process-to-achieve-resiliency"></a>Prozess zur Erreichung von Resilienz

Resilienz ist kein Add-On. Sie muss in das System integriert und im Betrieb umgesetzt werden. Dieses allgemeine Modell dient hierbei als Grundlage:

1. **Definieren** Sie Ihre Verfügbarkeitsanforderungen basierend auf den geschäftlichen Anforderungen.
2. **Entwerfen** Sie die Anwendung mit Blick auf Resilienz. Beginnen Sie mit einer Architektur, die auf bewährten Methoden basiert, und identifizieren Sie dann die möglichen Fehlerpunkte dieser Architektur.
3. **Implementieren** Sie die Strategien für die Erkennung von Fehlern und die anschließende Wiederherstellung.
4. **Testen** Sie die Implementierung, indem Sie Fehler simulieren und erzwungene Failover auslösen.
5. **Stellen Sie die Anwendung in der Produktion bereit**, indem Sie einen zuverlässigen und wiederholbaren Prozess verwenden.
6. **Überwachen** Sie die Anwendung, um Fehler zu erkennen. Indem Sie das System überwachen, können Sie die Integrität der Anwendung messen und bei Bedarf auf Vorfälle reagieren.
7. **Reagieren** Sie, wenn es zu Fehlern kommt, für die manuelle Eingriffe erforderlich sind.

Im weiteren Verlauf dieses Artikels werden diese Schritte ausführlicher beschrieben.

## <a name="define-your-availability-requirements"></a>Definieren der Verfügbarkeitsanforderungen

Die Resilienzplanung beginnt mit den geschäftlichen Anforderungen. Hier sind einige Ansätze beschrieben, die für die Resilienz in diesem Zusammenhang geeignet sind.

### <a name="decompose-by-workload"></a>Aufteilen von Workloads

Viele Cloudlösungen bestehen aus mehreren Anwendungsworkloads. Der Begriff „Workload“ steht in diesem Kontext für eine diskrete Funktion oder Computingaufgabe, die von anderen Aufgaben in Bezug auf die Geschäftslogik und die Datenspeicheranforderungen logisch getrennt werden kann. Eine E-Commerce-App kann beispielsweise die folgenden Workloads enthalten:

- Suchen in einem Produktkatalog
- Erstellen und Nachverfolgen von Aufträgen
- Anzeigen von Empfehlungen

Für diese Workloads gelten unter Umständen unterschiedliche Anforderungen in Bezug auf die Verfügbarkeit, Skalierbarkeit, Datenkonsistenz und Notfallwiederherstellung. Im Hinblick auf das Kosten-Risiko-Verhältnis sind geschäftliche Entscheidungen zu treffen.

Berücksichtigen Sie außerdem die Nutzungsmuster. Gibt es bestimmte kritische Zeiträume, in denen das System verfügbar sein muss? Beispiele: Ein Dienst für Steuererklärungen darf kurz vor dem Einreichungsdatum nicht ausfallen, ein Dienst für Videostreams muss während eines großen Sportereignisses stabil bleiben usw. Während dieser kritischen Zeiträume verwenden Sie ggf. redundante Bereitstellungen in mehreren Regionen, damit für die Anwendung ein Failover durchgeführt werden kann, wenn eine Region ausfällt. Aber da die Bereitstellung in mehreren Regionen potenzial mit höheren Kosten verbunden ist, kann es ratsam sein, die Anwendung zu weniger kritischen Zeiten nur in einer Region auszuführen. In einigen Fällen können die zusätzlichen Kosten mithilfe von modernen serverlosen Techniken verringert werden, die verbrauchsbasierte Abrechnung verwenden, sodass Ihnen nicht ausgelastete Computeressourcen nicht in Rechnung gestellt werden.

### <a name="rto-and-rpo"></a>RTO und RPO

Zwei wichtige Metriken, die berücksichtigt werden sollten, sind Recovery Time Objective (RTO) und Recovery Point Objective (RPO), da sie sich auf Notfallwiederherstellung beziehen.

- **Recovery Time Objective** (RTO) ist der maximal zulässige Zeitraum, in dem eine Anwendung nach einem Vorfall nicht verfügbar sein darf. Wenn RTO bei Ihnen 90 Minuten beträgt, müssen Sie dazu in der Lage sein, die Anwendung innerhalb von 90 Minuten nach Beginn eines Notfalls wieder in den Ausführungszustand zu versetzen. Falls für RTO ein sehr niedriger Wert angesetzt ist, führen Sie ggf. in einer zweiten regionalen Bereitstellung ständig eine Aktiv/Passiv-Konfiguration im Standby aus, um vor einem regionalen Ausfall geschützt zu sein. In einigen Fällen können Sie eine Aktiv/Aktiv-Konfiguration bereitstellen, um sogar einen noch niedrigeren RTO-Wert zu erzielen.

- **Recovery Point Objective** (RPO) ist die maximale Dauer eines Datenverlusts, die während eines Notfalls zulässig ist. Wenn Sie Daten beispielsweise in einer einzelnen Datenbank ohne Replikation in anderen Datenbanken speichern und stündliche Sicherungen durchführen, können Daten für bis zu eine Stunde verloren gehen.

RTO und RPO sind nicht funktionsbezogene Anforderungen eines Systems und sollten von Geschäftsanforderungen vorgegeben werden. Um diese Werte abzuleiten empfiehlt es sich, eine Risikobewertung durchzuführen und sich der Kosten für Ausfallzeiten oder Datenverluste genau bewusst zu sein.

### <a name="mttr-and-mtbf"></a>MTTR und MTBF

Zwei weitere gängige Measures für Verfügbarkeit sind MTTR (Mean Time To Recover, mittlere Reparaturzeit) und MTBF (Mean Time Between Failure, mittlere Betriebsdauer zwischen Ausfällen). Diese Measures werden in der Regel intern von Dienstanbietern verwendet, um zu ermitteln, wo Clouddiensten Redundanz hinzuzufügen ist und welche SLAs für Kunden bereitzustellen sind.

**Mean Time to Recover** (MTTR) ist der durchschnittliche Zeitraum, der zur Wiederherstellung einer Komponente nach einem Ausfall erforderlich ist. MTTR ist ein empirischer Fakt über eine Komponente. Based auf der MTTR der einzelnen Komponenten können Sie die MTTR einer gesamten Anwendung schätzen. Das Erstellen von Anwendungen aus mehreren Komponenten mit niedrigen MTTR-Werten sorgt für eine Anwendung mit einer niedrigen Gesamt-MTTR, die nach Ausfällen schnell wiederherzustellen ist.

**Mean Time Between Failures** (MTBF) ist die Laufzeit, die für eine Komponente verhältnismäßig zwischen Ausfällen zu erwarten ist. Mit dieser Metrik können Sie berechnen, wie häufig ein Dienst nicht verfügbar sein wird. Eine unzuverlässige Komponente hat einen geringen MTBF-Wert, was zu einem geringen SLA-Wert für die jeweilige Komponente führt. Allerdings können die Auswirkungen eines niedrigen MTBF-Werts gemindert werden, indem mehrere Instanzen der Komponente bereitgestellt werden, zwischen denen Failover implementiert wird.

> [!NOTE]
> Wenn EINER der MTTR-Werte von Komponenten in einer Einrichtung mit hoher Verfügbarkeit den RTO-Wert des Systems überschreitet, führt ein Fehler im System zu einer nicht akzeptablen Geschäftsunterbrechung. Das Wiederherstellen des Systems innerhalb des definierten RTO-Zeitraums ist nicht möglich.

### <a name="slas"></a>SLAs

In Azure wird in der [Vereinbarung zum Servicelevel (SLA)][sla] die garantierte Verfügbarkeit und Konnektivität beschrieben, die Microsoft zusichert. Wenn die Vereinbarung zum Servicelevel für einen bestimmten Dienst 99,9% beträgt, können Sie erwarten, dass der Dienst 99,9% der Zeit verfügbar ist.

> [!NOTE]
> Die Vereinbarung zum Servicelevel von Azure enthält auch Bestimmungen zum Erhalt einer Gutschrift, falls die Vereinbarung nicht erfüllt wird, sowie bestimmte Definitionen zur „Verfügbarkeit“ jedes Diensts. Dieser Aspekt der Vereinbarung zum Servicelevel fungiert als Durchsetzungsrichtlinie.

Sie sollten eigene Ziel-SLAs für jede Workload Ihrer Lösung definieren. Anhand einer Vereinbarung zum Servicelevel kann ausgewertet werden, ob die Architektur die geschäftlichen Anforderungen erfüllt. Führen Sie einen Vorgang zur Abhängigkeitszuordnung durch, um interne und externe Abhängigkeiten zu identifizieren, z. B. Active Directory oder Drittanbieterdienste wie einen Zahlungsanbieter oder E-Mail-Messagingdienst. Achten Sie besonders auf etwaige externe Abhängigkeiten, bei denen es sich um Single Points of Failure handeln kann oder die zu Engpässen während eines Ereignisses führen können. Wenn für eine Workload beispielsweise eine Betriebszeit von 99,99% erforderlich ist, dafür aber eine Abhängigkeit von einem Dienst mit einer Vereinbarung zum Servicelevel mit 99,9% besteht, kann dieser Dienst im System kein Single Point of Failure sein. Eine Lösung hierfür ist die Verwendung eines Fallbackpfads für den Fall, dass der Dienst ausfällt, oder das Treffen von anderen Maßnahmen zur Wiederherstellung nach einem Ausfall des Diensts.

In der folgenden Tabelle sind die potenziellen kumulativen Ausfallzeiten für verschiedene SLA-Ebenen angegeben.

| SLA | Ausfallzeit pro Woche | Ausfallzeit pro Monat | Ausfallzeit pro Jahr |
| --- | --- | --- | --- |
| 99 % |1,68 Stunden |7,2 Stunden |3,65 Tage |
| 99,9 % |10,1 Minuten |43,2 Minuten |8,76 Stunden |
| 99,95 % |5 Minuten |21,6 Minuten |4,38 Stunden |
| 99,99 % |1,01 Minuten |4,32 Minuten |52,56 Minuten |
| 99,999% |6 Sekunden |25,9 Sekunden |5,26 Minuten |

Sofern alle anderen Faktoren identisch sind, ist eine höhere Verfügbarkeit natürlich immer vorzuziehen. Aber wenn eine noch höhere Verfügbarkeit angestrebt wird, erhöhen sich auch die Kosten und die Komplexität, um dies zu erreichen. Eine Betriebszeit von 99,99% entspricht einer Gesamtausfallzeit von ca. 5 Minuten pro Monat. Rechtfertigt die Erreichung von 99,999% die zusätzliche Komplexität und die höheren Kosten? Die Antwort hängt von Ihren geschäftlichen Anforderungen ab.

Hier sind einige andere Aspekte aufgeführt, die für das Definieren einer Vereinbarung zum Servicelevel gelten:

- Wenn Sie 99,99% erreichen möchten, können Sie sich bei der Wiederherstellung nach Ausfällen wahrscheinlich nicht auf manuelle Eingriffe verlassen. Die Anwendung muss eine Selbstdiagnose und Selbstreparatur durchführen können.
- Im Bereich über 99,99% stellt es eine große Herausforderung dar, Ausfälle schnell genug zu erkennen, um die SLA-Anforderungen zu erfüllen.
- Betrachten Sie das Zeitfenster, auf das sich Ihre Vereinbarung zum Servicelevel bezieht. Je kleiner das Fenster, desto enger die Toleranzen. Vermutlich ist es nicht sinnvoll, Ihre Vereinbarung zum Servicelevel in Bezug auf die stündliche oder tägliche Betriebszeit zu definieren.
- Betrachten Sie die MTBF- und MTTR-Messungen. Je niedriger Ihre SLA, desto häufiger kann der Dienst ausfallen und desto schneller muss der Dienst wiederhergestellt werden.

### <a name="composite-slas"></a>Zusammengesetzte SLAs

Angenommen, eine App Service-Web-App führt Schreibvorgänge in eine Azure SQL-Datenbank durch. Zum Zeitpunkt der Erstellung dieses Artikels verfügen diese Azure-Dienste über die folgenden SLAs:

- App Service-Web-Apps = 99,95%
- SQL-Datenbank = 99,99%

![Zusammengesetzte Vereinbarung zum Servicelevel](./images/sla1.png)

Welche maximale Ausfallzeit erwarten Sie für diese Anwendung? Wenn einer der Dienste ausfällt, kommt es zu einem Ausfall der gesamten Anwendung. Im Allgemeinen ist die Wahrscheinlichkeit, dass jeder Dienst ausfällt, eine unabhängige Größe, und die zusammengesetzte Vereinbarung zum Servicelevel für diese Anwendung ist 99,95% &times; 99,99% = 99,94%. Dieser Wert ist niedriger als bei einzelnen SLAs. Dies ist nicht verwunderlich, da eine Anwendung, die von mehreren Diensten abhängig ist, über mehr potenzielle Fehlerpunkte verfügt.

Andererseits können Sie die zusammengesetzte Vereinbarung zum Servicelevel verbessern, indem Sie unabhängige Fallbackpfade erstellen. Wenn SQL-Datenbank beispielsweise nicht verfügbar ist, können Transaktionen zur späteren Verarbeitung in eine Warteschlange eingereiht werden.

![Zusammengesetzte Vereinbarung zum Servicelevel](./images/sla2.png)

Bei diesem Aufbau ist die Anwendung auch dann noch verfügbar, wenn keine Verbindung mit der Datenbank hergestellt werden kann. Dies ist aber nicht mehr der Fall, wenn die Datenbank und die Warteschlange gleichzeitig ausfallen. Der erwartete Zeitprozentwert für einen gleichzeitigen Ausfall ist 0,0001 &times; 0,001, sodass die zusammengesetzte Vereinbarung zum Servicelevel für diesen kombinierten Pfad wie folgt lautet:  

- Datenbank ODER Warteschlange = 1,0 &minus; (0,0001 &times; 0,001) = 99,99999%

Für die gesamte zusammengesetzte Vereinbarung zum Servicelevel gilt:

- Web-App UND (Datenbank ODER Warteschlange) = 99,95% &times; 99,99999% = 99,95% (ungefährer Wert)

Dieser Ansatz hat aber auch Nachteile. Die Anwendungslogik ist komplexer, es fallen Kosten für die Warteschlange an, und ggf. müssen Probleme mit der Datenkonsistenz gelöst werden.

**SLA für Bereitstellung in mehreren Regionen**. Ein weiteres Verfahren für Hochverfügbarkeit ist die Bereitstellung der Anwendung in mehr als einer Region und die Verwendung von Azure Traffic Manager zum Durchführen eines Failovers, wenn die Anwendung in einer Region ausfällt. Für eine Bereitstellung in mehreren Regionen wird die zusammengesetzte Vereinbarung zum Servicelevel wie folgt berechnet:

*N* steht für die zusammengesetzte SLA für die in einer Region bereitgestellten Anwendung. *R* steht für die Anzahl von Regionen, in denen die Anwendung bereitgestellt wird. Die erwartete Wahrscheinlichkeit, dass die Anwendung in beiden Regionen gleichzeitig ausfällt, ist ((1 &minus; N) ^ R).

Beispiel: Wenn die SLA für die einzelne Region 99,95% ist, gilt:

- Kombinierte SLA für zwei Regionen: (1 &minus; (1 &minus; 0,9995) ^ 2) = 99,999975 %
- Kombinierte SLA für vier Regionen: (1 &minus; (1 &minus; 0,9995) ^ 4) = 99,999999 %

Außerdem müssen Sie noch die [Vereinbarung zum Servicelevel für Traffic Manager][tm-sla] einbinden. Zum Zeitpunkt der Erstellung dieses Artikels verfügt die Vereinbarung zum Servicelevel für Traffic Manager über den Wert 99,99%.

Zudem wird das Failover in Aktiv/Passiv-Konfigurationen nicht sofort durchgeführt, sodass es während eines Failovers zu Ausfallzeiten kommen kann. Informationen hierzu finden Sie unter [Traffic Manager-Endpunktüberwachung und -failover][tm-failover].

Der berechnete SLA-Wert ist ein nützlicher Anhaltspunkt, aber er hat in Bezug auf die Verfügbarkeit keine umfassende Aussagekraft. Häufig kann eine Anwendung korrekt herabgestuft werden, wenn ein nicht kritischer Pfad ausfällt. Stellen Sie sich eine Anwendung vor, in der ein Katalog mit Büchern angezeigt wird. Wenn die Anwendung das Miniaturbild für den Buchdeckel nicht abrufen kann, wird ggf. ein Platzhalterbild angezeigt. In diesem Fall wird die Betriebszeit der Anwendung durch das fehlende Abrufen des Bilds nicht reduziert, obwohl für Benutzer eine Beeinträchtigung besteht.  

## <a name="design-for-resiliency"></a>Entwurf mit Blick auf Resilienz

In der Entwurfsphase ist es ratsam, eine Fehlermodusanalyse (Failure Mode Analysis, FMA) durchzuführen. Das Ziel einer FMA ist die Identifizierung von möglichen Fehlerpunkten, und es wird definiert, wie die Anwendung auf diese Fehler reagiert.

- Wie erkennt die Anwendung diese Art von Fehler?
- Wie reagiert die Anwendung auf diese Art von Fehler?
- Wie protokollieren und überwachen Sie diese Art von Fehler?

Weitere Informationen zum FMA-Prozess mit spezifischen Empfehlungen für Azure finden Sie im [Azure-Leitfaden zur Resilienz: Fehlermodusanalyse][fma].

### <a name="example-of-identifying-failure-modes-and-detection-strategy"></a>Beispiel für die Identifizierung von Fehlermodi und eine Erkennungsstrategie

**Fehlerpunkt:** Aufruf eines externen Webdiensts bzw. einer API.

| Fehlermodus | Erkennungsstrategie |
| --- | --- |
| Dienst nicht verfügbar |HTTP 5xx |
| Drosselung |HTTP 429 (Zu viele Anforderungen) |
| Authentication |HTTP 401 (Nicht autorisiert) |
| Langsame Reaktion |Timeout der Anforderung |

### <a name="redundancy-and-designing-for-failure"></a>Redundanz und Ausrichten des Entwurfs auf Fehler

Fehler und Ausfälle können mit unterschiedlichen Auswirkungen verbunden sein. Einige Hardwarefehler, z.B. ein ausgefallener Datenträger, wirken sich ggf. nur auf einen einzelnen Hostcomputer aus. Ein fehlerhafter Netzwerkswitch kann sich auf ein gesamtes Serverrack auswirken. Weniger häufig treten Fehler auf, die zu Störungen für ein gesamtes Rechenzentrum führen, z.B. zu einem Stromausfall im Rechenzentrum. In selten Fällen kann es vorkommen, dass eine gesamte Region nicht mehr verfügbar ist.

Eines der wichtigsten Verfahren, mit dem für eine Anwendung die Resilienz sichergestellt werden kann, ist die Redundanz. Sie müssen diese Redundanz aber beim Entwerfen der Anwendung einplanen. Außerdem richtet sich der jeweils benötigte Redundanzgrad nach Ihren Geschäftsanforderungen – nicht für jede Anwendung ist eine regionsübergreifende Redundanz als Schutz vor einem regionalen Ausfall erforderlich. In der Regel muss ein Kompromiss zwischen höherer Redundanz und Zuverlässigkeit und einer höheren Kostensumme und Komplexität gefunden werden.  

Azure verfügt über eine Reihe von Features, mit denen für eine Anwendung für alle Fehlerebenen Redundanz erzielt werden kann – von einer einzelnen VM bis hin zu einer gesamten Region.

![Azure-Resilienzfeatures](./images/redundancy.svg)

**Einzelne VM**: Azure bietet eine [Betriebszeit-SLA](https://azure.microsoft.com/support/legal/sla/virtual-machines) für einzelne virtuelle Computer. (Der virtuelle Computer muss Storage Premium für alle Betriebssystem-Datenträger und Datenträger verwenden.) Sie können zwar einen höheren SLA-Grad erzielen, indem Sie zwei oder mehr VMs ausführen, aber für einige Workloads kann eine einzelne VM zuverlässig genug sein. Für Produktionsworkloads empfehlen wir jedoch aus Redundanzgründen die Verwendung von mindestens zwei virtuellen Computern.

**Verfügbarkeitsgruppen**: Stellen Sie als Schutz vor lokalen Hardwarefehlern, z.B. ein Ausfall eines Datenträgers oder Netzwerkswitchs, in einer Verfügbarkeitsgruppe zwei oder mehr VMs bereit. Eine Verfügbarkeitsgruppe besteht aus mindestens zwei *Fehlerdomänen*, die eine Stromquelle und einen Netzwerkswitch gemeinsam nutzen. VMs in einer Verfügbarkeitsgruppe sind auf die Fehlerdomänen verteilt. Wenn ein Hardwarefehler eine Fehlerdomäne betrifft, kann der Netzwerkdatenverkehr so weiterhin an die VMs in den anderen Fehlerdomänen weitergeleitet werden. Weitere Informationen zu Verfügbarkeitsgruppen finden Sie unter [Verwalten der Verfügbarkeit virtueller Windows-Computer in Azure](/azure/virtual-machines/windows/manage-availability).

**Verfügbarkeitszonen**.  Eine Verfügbarkeitszone ist eine physisch getrennte Zone in einer Azure-Region. Jede Verfügbarkeitszone verfügt über eine eigene Stromquelle, ein Netzwerk und eine Kühlung. Die Bereitstellung von VMs über Verfügbarkeitszonen hinweg dient dem Schutz einer Anwendung vor Ausfällen, die ein gesamtes Rechenzentrum betreffen. Nicht alle Regionen unterstützen Verfügbarkeitszonen. Eine Liste der unterstützten Regionen und Dienste finden Sie unter [Was sind Verfügbarkeitszonen in Azure?](/azure/availability-zones/az-overview)

Wenn Sie Verfügbarkeitszonen in Ihrer Bereitstellung verwenden möchten, überprüfen Sie zuerst, dass Ihre Anwendungsarchitektur und Codebasis diese Konfiguration unterstützen. Wenn Sie COTS-Software bereitstellen, wenden Sie sich an den Softwareanbieter und führen Sie angemessene Tests durch, bevor Sie in der Produktionsumgebung bereitstellen. Eine Anwendung muss in der Lage sein, während eines Ausfalls innerhalb der konfigurierten Zone einen Zustand beizubehalten Datenverluste zu verhindern. Die Anwendung muss die Ausführung in einer elastischen und verteilten Infrastruktur ohne in der Codebasis angegebene hartcodierte Infrastrukturkomponenten unterstützen.

**Azure Site Recovery:**  Replizieren Sie virtuelle Azure-Computer in einer anderen Azure-Region, um Ihre BCDR-Anforderungen (Business Continuity und Disaster Recovery) zu erfüllen. Sie können regelmäßige DR-Drills ausführen, um sicherzustellen, dass die Complianceanforderungen erfüllt werden. Der virtuelle Computer wird mit den angegebenen Einstellungen in der ausgewählten Region repliziert, sodass Sie Ihre Anwendungen bei Ausfällen in der Quellregion wiederherstellen können. Weitere Informationen finden Sie unter [Einrichten der Notfallwiederherstellung in einer sekundären Azure-Region für einen virtuellen Azure-Computer][site-recovery]. Erwägen Sie die RTO- und RPO-Werte hier für Ihre Lösung, und stellen Sie beim Testen sicher, dass die Wiederherstellungszeit und der Wiederherstellungspunkt für Ihre Anforderungen geeignet sind.

**Regionspaare**: Um eine Anwendung vor einem regionalen Ausfall zu schützen, können Sie sie in mehreren Regionen bereitstellen, indem Sie Azure Traffic Manager zum Verteilen von Internetdatenverkehr auf die verschiedenen Regionen verwenden. Jede Azure-Region ist mit einer anderen Region gekoppelt. Zusammen bilden sie ein [Regionspaar](/azure/best-practices-availability-paired-regions). Mit Ausnahme von „Brasilien, Süden“ befinden sich die Regionen der Regionspaare immer innerhalb des gleichen geografischen Gebiets, um steuerliche und rechtliche Anforderungen an den Speicherort von Daten zu erfüllen.

Berücksichtigen Sie beim Entwerfen einer Anwendung für mehrere Regionen, dass die Netzwerklatenz für mehrere Regionen höher als innerhalb einer Region ist. Wenn Sie beispielsweise eine Datenbank replizieren, um das Failover zu ermöglichen, sollten Sie die synchrone Datenreplikation innerhalb einer Region und die asynchrone Datenreplikation für mehrere Regionen verwenden.

Stellen Sie beim Auswählen von gekoppelten Regionen sicher, dass beide Regionen über die erforderlichen Azure-Dienste verfügen. Eine Liste mit Diensten nach Region finden Sie unter [Verfügbare Produkte nach Region](https://azure.microsoft.com/global-infrastructure/services/). Es ist auch sehr wichtig, dass Sie die richtige Bereitstellungstopologie für die Notfallwiederherstellung auswählen. Dies gilt besonders, wenn Sie über kurze RPO/RTO-Zeiträume verfügen. Wählen Sie entweder eine Aktiv/Passiv-Topologie (vollständiges Replikat) oder eine Aktiv/Aktiv-Topologie aus, um sicherzustellen, dass die Failoverregion eine ausreichende Kapazität für die Unterstützung Ihrer Workload aufweist. Beachten Sie hierbei, dass diese Bereitstellungstopologien zu einer erhöhten Komplexität und höheren Kosten führen können, da Ressourcen in der sekundären Region vorab bereitgestellt werden und sich ggf. im Leerlauf befinden. Weitere Informationen finden Sie unter [Bereitstellungstopologien für die Notfallwiederherstellung][deployment-topologies].

| &nbsp; | Verfügbarkeitsgruppe | Verfügbarkeitszone | Azure Site Recovery/Gekoppelte Region |
|--------|------------------|-------------------|---------------|
| Fehlerumfang | Rack | Datacenter | Region |
| Routinganforderung | Load Balancer | Zonenübergreifender Lastenausgleich | Traffic Manager |
| Netzwerklatenz | Sehr niedrig | Niedrig | Mittel bis hoch |
| Virtuelles Netzwerk  | VNet | VNet | Regionsübergreifendes VNet-Peering |

## <a name="implement-resiliency-strategies"></a>Implementieren von Resilienzstrategien

Dieser Abschnitt enthält eine Übersicht über einige häufig verwendete Resilienzstrategien. Die meisten davon sind nicht auf eine bestimmte Technologie beschränkt. In den Beschreibungen in diesem Abschnitt sind jeweils die grundlegenden Ideen jedes Verfahrens mit Links zu weiteren Informationen zusammengefasst.

**Wiederholen des Vorgangs bei vorübergehenden Fehlern.** Vorübergehende Fehler können durch eine kurze Trennung der Netzwerkverbindung, einen Verlust der Verbindung zur Datenbank oder eine Zeitüberschreitung bei zu hoher Auslastung eines Diensts verursacht werden. Häufig kann ein vorübergehender Fehler einfach gelöst werden, indem versucht wird, die Anforderung erneut zu senden. Für viele Azure-Dienste werden vom Client-SDK automatische Wiederholungen so implementiert, dass dies für den Aufrufer transparent ist. Informationen hierzu finden Sie unter [Anleitung zu dienstspezifischen Wiederholungsmechanismen][retry-service-specific guidance].

Jeder Wiederholungsversuch trägt zur Gesamtwartezeit bei. Außerdem können zu viele fehlgeschlagene Anforderungsversuche zu einem Engpass führen, wenn sich ausstehende Anforderungen in der Warteschlange ansammeln. Diese blockierten Anforderungen können kritische Systemressourcen belegen, z.B. Arbeitsspeicher, Threads, Datenbankverbindungen usw., sodass es zu kaskadierenden Fehlern kommen kann. Verlängern Sie die Verzögerungszeit zwischen den einzelnen Wiederholungsversuchen, und begrenzen Sie die Gesamtzahl von fehlgeschlagenen Anforderungen, um dies zu verhindern.

![Diagramm der Wiederholungsversuche](./images/retry.png)

**Übergreifender Lastenausgleich für Instanzen.** Aus Gründen der Skalierbarkeit sollte eine Cloudanwendung horizontal hochskaliert werden können, indem mehr Instanzen hinzugefügt werden. Mit diesem Ansatz wird auch die Resilienz verbessert, da fehlerhafte Instanzen aus der Rotation herausgenommen werden können. Beispiel: 

- Ordnen Sie mindestens zwei VMs hinter einem Lastenausgleich an. Mit dem Lastenausgleichsmodul wird Datenverkehr auf alle VMs verteilt. Informationen hierzu finden Sie unter [Run load-balanced VMs for scalability and availability][ra-multi-vm] (Ausführen von VMs mit Lastenausgleich, um Skalierbarkeit und Verfügbarkeit zu erzielen).
- Führen Sie für eine Azure App Service-App das zentrale Hochskalieren auf mehrere Instanzen durch. App Service verteilt die Last automatisch auf die Instanzen. Informationen hierzu finden Sie unter [Basic web application][ra-basic-web] (Einfache Webanwendung).
- Verwenden Sie [Azure Traffic Manager][tm], um Datenverkehr auf eine Gruppe von Endpunkten zu verteilen.

**Replizieren von Daten.** Das Replizieren von Daten ist eine allgemeine Strategie zum Verarbeiten von nicht vorübergehenden Fehlern in einem Datenspeicher. Viele Speichertechnologien verfügen über eine integrierte Replikation, z.B. Azure Storage, Azure SQL-Datenbank, Cosmos DB und Apache Cassandra. Es ist wichtig, dass sowohl der Lese- als auch der Schreibpfad berücksichtigt wird. Je nach Speichertechnologie sind ggf. mehrere beschreibbare Replikate vorhanden (oder ein einzelnes beschreibbares Replikat und mehrere schreibgeschützte Replikate).

Zur Maximierung der Verfügbarkeit können Replikate in mehreren Regionen angeordnet werden. Hiermit wird aber die Wartezeit beim Replizieren der Daten erhöht. Normalerweise wird das regionsübergreifende Replizieren asynchron durchgeführt. Dies ist mit einem Modell der letztlichen Konsistenz und potenziellem Datenverlust bei einem Ausfall eines Replikats verbunden.

Sie können mithilfe von [Azure Site Recovery][site-recovery] virtuelle Azure-Computer aus einer Region in einer anderen Region replizieren. Site Recovery repliziert Daten fortlaufend in der Zielregion. Bei einem Ausfall am primären Standort können Sie ein Failover auf den sekundären Standort ausführen.

**Stufen Sie Funktionalität korrekt herab**. Wenn ein Dienst ausfällt und kein Failoverpfad vorhanden ist, kann die Anwendung unter Umständen korrekt herabgestuft werden und trotzdem noch eine akzeptable Benutzerfreundlichkeit bieten. Beispiel: 

- Reihen Sie ein Arbeitselement zur späteren Verarbeitung in eine Warteschlange ein.
- Geben Sie einen geschätzten Wert zurück.
- Verwenden Sie lokal zwischengespeicherte Daten.
- Zeigen Sie eine Fehlermeldung für den Benutzer an. (Diese Option ist besser, als wenn die Anwendung nicht mehr auf Anforderungen reagiert.)

**Drosseln von Benutzern mit hohem Volumen.** Es kann vorkommen, dass eine geringe Zahl von Benutzern für die Erzeugung sehr hoher Lasten verantwortlich ist. Dies kann negative Auswirkungen auf andere Benutzer haben und die Gesamtverfügbarkeit Ihrer Anwendung reduzieren.

Wenn von einem einzelnen Client übermäßig viele Anforderungen ausgehen, kann die Anwendung den Client ggf. für einen bestimmten Zeitraum drosseln. Während des Drosselungszeitraums verweigert die Anwendung einige oder alle Anforderungen dieses Clients (je nach Drosselungsstrategie). Der Schwellenwert für die Drosselung richtet sich unter Umständen nach der Dienstebene des Kunden.

Die Drosselung muss nicht bedeuten, dass der Client in böser Absicht gehandelt hat, sondern nur, dass das Dienstkontingent überschritten wurde. In einigen Fällen kann es vorkommen, dass ein Consumer das Kontingent ständig überschreitet oder sich auf andere Weise unangemessen verhält. Sie können dann weitere Maßnahmen treffen und den Benutzer blockieren. Normalerweise wird hierzu ein API-Schlüssel oder ein IP-Adressbereich blockiert. Weitere Informationen finden Sie unter [Throttling Pattern][throttling-pattern] (Drosselungsmuster).

**Verwenden eines Schutzschalters.** Mit einem [Schutzschaltermuster][circuit-breaker-pattern] kann verhindert werden, dass von einer Anwendung wiederholt versucht wird, einen Vorgang auszuführen, der mit hoher Wahrscheinlichkeit fehlschlägt. Der Schutzschalter umschließt Aufrufe an einen Dienst und verfolgt die Anzahl kürzlich aufgetretener Fehler nach. Überschreitet die Fehleranzahl einen Schwellenwert, gibt der Schutzschalter einen Fehlercode zurück, ohne den Dienst aufzurufen. Dadurch hat der Dienst Zeit zur Wiederherstellung.

**Verwenden eines Belastungsausgleichs zum Ausgleichen von Datenverkehrsspitzen.**
Für Anwendungen kann es zu plötzlichen Datenverkehrsspitzen kommen, die von Diensten auf dem Back-End nicht mehr bewältigt werden können. Wenn ein Back-End-Dienst nicht schnell genug auf Anforderungen reagieren kann, kann dies dazu führen, dass Anforderungen in eine Warteschlange eingereiht werden (Rückstau) oder die Anwendung vom Dienst gedrosselt wird. Sie können eine Warteschlange als Puffer verwenden, um dies zu verhindern. Wenn ein neues Arbeitselement vorhanden ist, reiht die Anwendung ein Arbeitselement für die asynchrone Ausführung in die Warteschlange ein, anstatt sofort den Back-End-Dienst aufzurufen. Die Warteschlange fungiert als Puffer, um Lastspitzen zu glätten. Weitere Informationen finden Sie unter [Queue-Based Load Leveling Pattern][load-leveling-pattern] (Warteschlangenbasiertes Belastungsausgleichsmuster).

**Isolieren von kritischen Ressourcen.** Fehler in einem Subsystem können ggf. zu kaskadierenden Fehlern werden und Fehler in anderen Teilen der Anwendung verursachen. Dies kann passieren, wenn ein Fehler dazu führt, dass einige Ressourcen, z.B. Threads oder Sockets, nicht rechtzeitig freigegeben werden und die Ressourcen erschöpft sind.

Um dies zu vermeiden, können Sie ein System in isolierte Gruppen partitionieren, damit ein Ausfall in einer Partition nicht zum Ausfall des gesamten Systems führt. Dieses Verfahren wird auch als [Bulkhead-Muster][bulkhead-pattern] (Trennwandmuster) bezeichnet.

Beispiele:

- Partitionieren Sie eine Datenbank (z.B. nach Mandant), und weisen Sie einen separaten Pool mit Webserverinstanzen für jede Partition zu.  
- Verwenden Sie separate Threadpools, um Aufrufe von unterschiedlichen Diensten zu isolieren. Auf diese Weise werden kaskadierende Fehler verhindert, wenn einer der Dienste ausfällt. Ein Beispiel finden Sie in der [Hystrix-Bibliothek][hystrix] von Netflix.
- Verwenden Sie [Container][containers], um die Ressourcen zu begrenzen, die für ein bestimmtes Subsystem verfügbar sind.

![Diagramm des Bulkhead-Musters](./images/bulkhead.png)

**Anwenden von ausgleichenden Transaktionen.** Mit einer [ausgleichenden Transaktion][compensating-transaction-pattern] werden die Auswirkungen einer anderen abgeschlossenen Transaktion rückgängig gemacht. In einem verteilten System kann es sehr schwierig sein, eine hohe Transaktionskonsistenz zu erzielen. Ausgleichende Transaktionen sind eine Möglichkeit, Konsistenz zu erreichen, indem eine Reihe von kleineren einzelnen Transaktionen verwendet wird, die in jedem Schritt rückgängig gemacht werden können.

Beim Buchen einer Reise kann ein Kunde beispielsweise einen Mietwagen, ein Hotelzimmer und einen Flug reservieren. Wenn einer dieser Schritte nicht erfolgreich ist, schlägt der gesamte Vorgang fehl. Anstatt eine einzelne verteilte Transaktion für den gesamten Vorgang zu verwenden, können Sie für jeden Schritt eine ausgleichende Transaktion definieren. Beispielsweise können Sie die Reservierung stornieren, um die Reservierung eines Mietwagens rückgängig zu machen. Ein Koordinator führt die einzelnen Schritte aus, um den gesamten Vorgang abzuschließen. Falls ein Schritt fehlschlägt, wendet der Koordinator ausgleichende Transaktionen an, um abgeschlossene Schritte rückgängig zu machen.

## <a name="test-for-resiliency"></a>Testen der Resilienz

Im Allgemeinen können Sie die Resilienz nicht auf die gleiche Weise wie die Anwendungsfunktionalität testen (per Ausführung von Komponententests usw.). Stattdessen müssen Sie testen, wie sich die End-to-End-Workload unter Fehlerbedingungen verhält, die nur zeitweise auftreten.

Der Testvorgang ist ein iterativer Prozess. Testen Sie die Anwendung, messen Sie das Ergebnis, analysieren und beheben Sie alle aufgetretenen Fehler, und wiederholen Sie den Vorgang.

**Fault Injection-Tests**: Testen Sie die Resilienz des Systems bei Fehlern, indem Sie entweder echte Fehler auslösen oder diese simulieren. Hier sind einige häufige Fehlerszenarien aufgeführt, die getestet werden können:

- Herunterfahren von VM-Instanzen
- Absturz von Prozessen
- Ablauf von Zertifikaten
- Ändern von Zugriffsschlüsseln
- Herunterfahren des DNS-Diensts auf Domänencontrollern
- Begrenzen von verfügbaren Systemressourcen, z.B. RAM oder Anzahl von Threads
- Aufheben der Bereitstellung von Datenträgern
- Erneutes Bereitstellen eines virtuellen Computers

Messen Sie die Wiederherstellungszeiten, und stellen Sie sicher, dass Ihre geschäftlichen Anforderungen erfüllt werden. Testen Sie auch Kombinationen von Fehlermodi. Vergewissern Sie sich, dass es nicht zu kaskadierenden Fehlern kommt und dass diese isoliert behandelt werden.

Dies ist ein weiterer Grund, warum es wichtig ist, mögliche Fehlerpunkte während der Entwurfsphase zu analysieren. Die Ergebnisse dieser Analyse sollten in Ihren Testplan einfließen.

**Auslastungstests**: Auslastungstests sind sehr wichtig zur Identifizierung von Fehlern, die nur bei hoher Auslastung auftreten, z.B. eine Überlastung der Back-End-Datenbank oder eine Drosselung des Diensts. Führen Sie Tests für Spitzenlasten durch, indem Sie Produktionsdaten oder synthetische Daten verwenden, die den Produktionsdaten möglichst genau ähneln. Hierbei soll ermittelt werden, wie sich die Anwendung unter realen Bedingungen verhält.

**Übungen zur Notfallwiederherstellung.** Es reicht nicht aus, einen guten Notfallwiederherstellungsplan aufzustellen. Sie müssen ihn auch in regelmäßigen Abständen testen, um sicherzustellen, dass der Wiederherstellungsplan funktioniert, wenn es darauf ankommt. Für virtuelle Azure-Computer können Sie [Azure Site Recovery][site-recovery] zum Replizieren sowie zum [Ausführen von Notfallwiederherstellungsübungen][site-recovery-test-failover] verwenden, ohne Produktionsanwendungen oder die laufende Replikation zu beeinträchtigen.

## <a name="deploy-using-reliable-processes"></a>Bereitstellen mithilfe zuverlässiger Prozesse

Nachdem eine Anwendung in der Produktion bereitgestellt wurde, sind Updates eine mögliche Fehlerquelle. Im schlimmsten Fall kann ein fehlerhaftes Update zu Ausfallzeiten führen. Der Bereitstellungsprozess muss vorhersagbar und wiederholbar sein, um dies zu vermeiden. Die Bereitstellung umfasst auch die Bereitstellung von Azure-Ressourcen und -Anwendungscode und das Anwenden von Konfigurationseinstellungen. Ein Update kann alle drei Vorgänge oder einen Teil davon abdecken.

Der entscheidende Punkt ist, dass manuelle Bereitstellungen fehleranfällig sind. Daher wird empfohlen, einen automatisierten idempotenten Prozess zu verwenden, den Sie bedarfsabhängig und bei Fehlern auch erneut durchführen können.

- Zum Automatisieren der Bereitstellung von Azure-Ressourcen können Sie [Terraform][terraform], [Ansible][ansible], [Chef][chef], [Puppet][puppet], [PowerShell][powershell], [CLI][cli] oder [Azure Resource Manager-Vorlagen][template-deployment] verwenden.
- Verwenden Sie die [Azure Automation-Konfiguration für den gewünschten Zustand][dsc] (Desired State Configuration, DSC) zum Konfigurieren von VMs. Für Linux-VMs können Sie [Cloud-init][cloud-init] verwenden.
- Sie können die Anwendungsbereitstellung mit [Azure DevOps Services][azure-devops-services] oder [Jenkins][jenkins] automatisieren.

Zwei Konzepte für die robuste Bereitstellung sind *Infrastruktur als Code* und *unveränderliche Infrastruktur*.

- Bei **Infrastruktur als Code** wird Code zum Bereitstellen und Konfigurieren der Infrastruktur verwendet. Für Infrastruktur als Code kann ein deklarativer Ansatz oder ein imperativer Ansatz genutzt werden (oder eine Kombination). Resource Manager-Vorlagen sind ein Beispiel für einen deklarativen Ansatz. PowerShell-Skripts sind ein Beispiel für einen imperativen Ansatz.
- Bei **unveränderlicher Infrastruktur** geht es darum, die Infrastruktur nicht mehr zu ändern, nachdem sie in der Produktion bereitgestellt wurde. Andernfalls kann es zu einem Zustand kommen, in dem Ad-hoc-Änderungen angewendet wurden. In diesem Fall ist es schwierig, die genauen Änderungen zu erkennen und Entscheidungen für das System zu treffen.

Eine andere Frage ist, wie das Rollout für ein Anwendungsupdate durchgeführt werden soll. Wir empfehlen die Nutzung von Verfahren wie eine Blaugrün-Bereitstellung oder Canary-Releases, bei denen Updates unter starker Kontrolle übertragen werden, um mögliche negative Auswirkungen einer fehlerhaften Bereitstellung zu verringern.

- Die [Blaugrün-Bereitstellung][blue-green] ist ein Verfahren, bei dem ein Update in einer Produktionsumgebung separat von der Liveanwendung bereitgestellt wird. Wechseln Sie für das Routing von Datenverkehr nach der Überprüfung der Bereitstellung dann zur aktualisierten Version. Für Azure App Service-Web-Apps wird dies beispielsweise durch [Stagingslots][staging-slots] ermöglicht.
- [Canary-Releases][canary-release] ähneln Blaugrün-Bereitstellungen. Anstatt für den gesamten Datenverkehr auf die aktualisierte Version umzustellen, führen Sie für das Update den Rollout für einen kleinen Prozentsatz der Benutzer durch, indem Sie einen Teil des Datenverkehrs an die neue Bereitstellung weiterleiten. Falls ein Problem auftritt, können Sie den Vorgang stoppen und wieder auf die alte Bereitstellung zurückgreifen. Wenn alles in Ordnung ist, können Sie mehr Datenverkehr an die neue Version weiterleiten, bis die Umstellung für den gesamten Datenverkehr erfolgt ist.

Stellen Sie unabhängig vom gewählten Ansatz sicher, dass Sie einen Rollback auf die letzte als fehlerfrei bekannte Bereitstellung durchführen können, falls die neue Version nicht funktioniert. Sorgen Sie auch dafür, dass Sie über eine Strategie zum Ausführen eines Rollbacks von Datenbankänderungen und anderer Änderungen abhängiger Dienste verfügen. Wenn Fehler auftreten, muss in den Anwendungsprotokollen angegeben werden, welche Version den Fehler verursacht hat.

## <a name="monitor-to-detect-failures"></a>Überwachen zur Fehlererkennung

Die Überwachung ist für die Resilienz von entscheidender Bedeutung. Wenn ein Fehler auftritt, müssen Sie darüber informiert werden, und Sie müssen über Erkenntnisse zur Fehlerursache verfügen.

Die Überwachung eines umfangreichen verteilten Systems stellt eine erhebliche Herausforderung dar. Angenommen, eine Anwendung wird auf einigen Dutzend VMs ausgeführt. Es ist nicht effizient, sich einzeln nacheinander an jeder VM anzumelden und sich die Protokolldateien anzusehen, um die Problembehandlung durchzuführen. Die Anzahl von VM-Instanzen ist außerdem wahrscheinlich nicht statisch. VMs werden hinzugefügt und entfernt, wenn die Anwendung horizontal herunter- und hochskaliert wird, und gelegentlich kann es zu einem Ausfall einer Instanz kommen, sodass eine erneute Bereitstellung erforderlich ist. Außerdem können für eine typische Cloudanwendung mehrere Datenspeicher verwendet werden (Azure Storage, SQL-Datenbank, Cosmos DB, Redis Cache), und eine einzelne Benutzeraktion kann auf mehrere Subsysteme verteilt sein.

Sie können sich den Überwachungsprozess als Pipeline mit mehreren einzelnen Phasen vorstellen:

![Zusammengesetzte Vereinbarung zum Servicelevel](./images/monitoring.png)

- **Instrumentierung**: Die Rohdaten für die Überwachung stammen aus vielen unterschiedlichen Quellen, z. B. [Anwendungsprotokollen](/azure/application-insights/app-insights-overview?toc=/azure/azure-monitor/toc.json), [Betriebssystem-Leistungsmetriken](/azure/azure-monitor/platform/agents-overview), [Azure-Ressourcen](/azure/monitoring-and-diagnostics/monitoring-supported-metrics?toc=/azure/azure-monitor/toc.json), [Azure-Abonnements](/azure/service-health/service-health-overview) und [Azure-Mandanten](/azure/active-directory/reports-monitoring/howto-integrate-activity-logs-with-log-analytics). Die meisten Azure-Dienste machen [Metriken](/azure/azure-monitor/platform/data-collection) verfügbar, die Sie konfigurieren können, um die Ursache von Problemen zu analysieren und zu ermitteln.
- **Sammlung und Speicherung**: Rohdaten der Instrumentierung können an verschiedenen Orten und in unterschiedlichen Formaten (z.B. Ablaufverfolgungsprotokolle von Anwendungen, IIS-Protokolle, Leistungsindikatoren) vorgehalten werden. Diese unterschiedlichen Quellen werden gesammelt, zusammengefasst und in zuverlässige Datenspeicher eingefügt, z. B. Application Insights, Azure Monitor-Metriken, Service Health, Speicherkonten und Log Analytics.
- **Analyse und Diagnose**: Nachdem die Daten in diesen unterschiedlichen Datenspeichern zusammengefasst wurden, können sie analysiert werden, um die Problembehandlung durchzuführen und eine Gesamtansicht der Anwendungsintegrität bereitzustellen. Im Allgemeinen können Sie in Application Insights und Log Analytics nach den Daten suchen, indem Sie [Kusto-Abfragen](/azure/log-analytics/log-analytics-queries) verwenden. Azure Advisor stellt Empfehlungen bereit, bei denen der Schwerpunkt auf [Resilienz](/azure/advisor/advisor-high-availability-recommendations) und [Optimierung](/azure/advisor/advisor-performance-recommendations) liegt.
- **Visualisierung und Warnungen**: In dieser Phase werden Telemetriedaten so dargestellt, dass ein Bediener Probleme oder Trends schnell erkennen kann. Beispiele hierfür sind Dashboards oder E-Mail-Warnungen. Mit [Azure-Dashboards](/azure/azure-portal/azure-portal-dashboards) können Sie eine zentrale Ansicht mit Überwachungsgraphen erstellen, die aus Application Insights, Log Analytics, Azure Monitor-Metriken und Service Health stammen. Mit [Azure Monitor-Warnungen](/azure/monitoring-and-diagnostics/monitoring-overview-alerts?toc=/azure/azure-monitor/toc.json) können Sie Warnungen zur Dienstintegrität und Ressourcenintegrität erstellen.

Die Überwachung ist nicht dasselbe wie die Fehlererkennung. Es kann beispielsweise sein, dass Ihre Anwendung einen vorübergehenden Fehler erkennt und einen Wiederholungsversuch durchführt, ohne dass es zu Ausfallzeit kommt. Es sollte aber auch der Wiederholungsvorgang protokolliert werden, damit Sie die Fehlerrate überwachen können, um ein Gesamtbild der Anwendungsintegrität zu erhalten.

Anwendungsprotokolle sind eine wichtige Quelle für Diagnosedaten. Bewährte Methoden für die Anwendungsprotokollierung sind:

- Protokollieren in der Produktion: Andernfalls fehlen Ihnen die Erkenntnisse in dem Bereich, für den Sie diese am dringendsten benötigen.
- Protokollieren von Ereignissen an den Dienstgrenzen: Binden Sie eine Korrelations-ID ein, die über Dienstgrenzen hinweg gilt. Wenn eine Transaktion mehrere Dienste durchläuft und einer davon ausfällt, können Sie anhand der Korrelations-ID ermitteln, warum es zum Transaktionsfehler gekommen ist.
- Verwenden Sie die semantische Protokollierung, die auch als strukturierte Protokollierung bezeichnet wird. Unstrukturierte Protokolle erschweren die Automatisierung des Verbrauchs und der Analyse von Protokolldaten, die auf Cloudebene erforderlich ist.
- Verwenden der asynchronen Protokollierung: Andernfalls kann das Protokollierungssystem selbst zu einem Fehler der Anwendung führen, wenn es zu einem Stau von Anforderungen und somit zu einer Blockade kommt, während auf das Schreiben eines Protokollierungsereignisses gewartet wird.
- Die Anwendungsprotokollierung ist nicht dasselbe wie die Überwachung per Auditing. Das Auditing muss unter Umständen aus Konformitätsgründen oder zur Einhaltung von Bestimmungen durchgeführt werden. Daher müssen die Überwachungsdatensätze vollständig sein, und es ist nicht zulässig, beim Verarbeiten von Transaktionen Datensätze zu verwerfen. Wenn für eine Anwendung das Auditing erforderlich ist, sollte dies von der Diagnoseprotokollierung getrennt werden.

Weitere Informationen zur Überwachung und Diagnose finden Sie unter [Anleitung zur Überwachung und Diagnose][monitoring-guidance].

## <a name="respond-to-failures"></a>Reagieren auf Fehler

In den vorherigen Abschnitten wurden Strategien für die automatisierte Wiederherstellung beschrieben, die für die Hochverfügbarkeit wichtig sind. Aber in einigen Fällen sind manuelle Eingriffe erforderlich.

- **Warnungen**. Überwachen Sie Ihre Anwendung auf Warnsignale, die proaktive Eingriffe nötig machen. Wenn Sie beispielsweise sehen, dass Ihre Anwendung von SQL-Datenbank oder Cosmos DB ständig gedrosselt wird, müssen Sie ggf. Ihre Datenbankkapazität erhöhen oder Ihre Abfragen optimieren. In diesem Beispiel sollte Ihre Telemetrie auch dann eine Warnung auslösen, wenn die Anwendung Drosselungsfehler transparent behandelt, damit Sie Maßnahmen treffen können. Es wird empfohlen, Warnungen für Azure-Ressourcenmetriken und -Diagnoseprotokolle anhand der Dienstgrenzwerte und Kontingentschwellenwerte zu konfigurieren. Es ist ratsam, Warnungen für Metriken einzurichten, da hiermit im Vergleich zu Diagnoseprotokollen eine geringere Latenz verbunden ist. Darüber hinaus kann in Azure über die [Ressourcenintegrität](/azure/service-health/resource-health-checks-resource-types) standardmäßig der Integritätsstatus bereitgestellt werden, um die Diagnose in Bezug auf die Drosselung von Azure-Diensten zu unterstützen.
- **Failover**. Konfigurieren Sie eine Strategie für die Notfallwiederherstellung Ihrer Anwendung. Die geeignete Strategie richtet sich jeweils nach Ihren SLAs. Für viele Szenarien reicht eine Aktiv/Passiv-Implementierung aus. Weitere Informationen finden Sie unter [Bereitstellungstopologien für die Notfallwiederherstellung](./disaster-recovery-azure-applications.md#deployment-topologies-for-disaster-recovery). Die meisten Azure-Dienste ermöglichen entweder ein manuelles oder ein automatisiertes Failover. Verwenden Sie in einer IaaS-Anwendung beispielsweise [Azure Site Recovery](/azure/site-recovery/azure-to-azure-architecture) für die Web- und Logikebene und [SQL-Always On-Verfügbarkeitsgruppen](/azure/virtual-machines/windows/sql/virtual-machines-windows-portal-sql-availability-group-dr) für die Datenbankebene. [Traffic Manager](/azure/traffic-manager/traffic-manager-overview) ermöglicht automatisiertes Failover über mehrere Regionen.
- **Testen der Betriebsbereitschaft**: Führen Sie einen Test auf Betriebsbereitschaft durch – sowohl für das Failover in die sekundäre Region als auch für das Failback in die primäre Region. Viele Azure-Dienste unterstützen manuelle Failover oder Testfailover für Übungen zur Notfallwiederherstellung. Alternativ können Sie einen Ausfall simulieren, indem Sie Dienste herunterfahren oder entfernen.
- **Prüfen der Datenkonsistenz**: Wenn in einem Datenspeicher ein Fehler auftritt, kann es zu Inkonsistenzen bei den Daten kommen, sobald der Speicher wieder verfügbar ist. Dies gilt besonders, wenn die Daten repliziert wurden. Sehen Sie sich für Azure-Dienste, die eine regionsübergreifende Replikation ermöglichen, den RTO- und RPO-Wert an, um sich mit dem zu erwartenden Datenverlust bei einem Ausfall vertraut zu machen. Lesen Sie die SLAs für Azure-Dienste, um sich darüber zu informieren, ob regionsübergreifende Failover manuell initiiert werden können oder ob dies von Microsoft durchgeführt wird. Bei einigen Diensten entscheidet Microsoft, wann das Failover durchgeführt wird. Microsoft versieht die Wiederherstellung von Daten in der primären Region unter Umständen mit Prioritäten, sodass ein Failover in eine sekundäre Region nur ausgeführt wird, wenn Daten in der primären Region als nicht wiederherstellbar angesehen werden. Dieses Modell kommt beispielsweise bei [georedundantem Speicher](/azure/storage/common/storage-redundancy-grs) und bei [Key Vault](/azure/key-vault/key-vault-disaster-recovery-guidance) zur Anwendung.
- **Wiederherstellen einer Sicherung**: In einigen Szenarien ist die Wiederherstellung aus einer Sicherung nur innerhalb derselben Region möglich. Dies ist für die [Sicherung von Azure-VMs](/azure/backup/backup-azure-vms-first-look-arm) der Fall. Andere Azure-Dienste stellen georeplizierte Sicherungen bereit, z. B. [geografische Redis Cache-Replikate](/azure/redis-cache/cache-how-to-geo-replication). Der Zweck von Sicherungen besteht darin, einen Schutz vor dem versehentlichen Löschen oder Beschädigen von Daten zu bieten und für die Anwendung eine frühere funktionierende Version wiederherzustellen. Sicherungen können zwar in einigen Fällen als Lösung für die Notfallwiederherstellung dienen, aber der Umkehrschluss trifft nicht immer zu: Die Notfallwiederherstellung bietet keinen Schutz vor dem versehentlichen Löschen oder Beschädigen von Daten.  

Dokumentieren und testen Sie Ihren Plan für die Notfallwiederherstellung. Werten Sie die geschäftlichen Auswirkungen von Anwendungsausfällen aus. Automatisieren Sie den Prozess so weit wie möglich, und dokumentieren Sie alle manuellen Schritte, z.B. manuelles Failover oder die Datenwiederherstellung aus Sicherungen. Testen Sie Ihren Prozess für die Notfallwiederherstellung regelmäßig, um den Plan zu überprüfen und zu verbessern. Richten Sie Warnungen für die Azure-Dienste ein, die von Ihrer Anwendung genutzt werden.

## <a name="summary"></a>Zusammenfassung

In diesem Artikel wurde die Resilienz aus einer ganzheitlichen Perspektive beschrieben, und es wurden einige besondere Anforderungen der Cloud erläutert. Hierzu gehörten die verteilte Struktur des Cloud Computing, die Nutzung von Standardhardware und das Auftreten von vorübergehenden Netzwerkfehlern.

Hier sind noch einmal die wichtigsten Punkte dieses Artikels aufgeführt:

- Resilienz führt zu einer höheren Verfügbarkeit und geringeren mittleren Zeit bis zur Wiederherstellung nach Ausfällen.
- Zur Erreichung von Resilienz in der Cloud sind andere Verfahren als bei herkömmlichen lokalen Lösungen erforderlich.
- Resilienz ist kein Zufallsprodukt. Sie muss von Beginn an gezielt entworfen und integriert werden.
- Resilienz betrifft alle Bereiche des Anwendungslebenszyklus – von der Planung und Codierung bis zum Betrieb.
- Testen und überwachen Sie!

<!-- links -->

[blue-green]: https://martinfowler.com/bliki/BlueGreenDeployment.html
[canary-release]: https://martinfowler.com/bliki/CanaryRelease.html
[circuit-breaker-pattern]: ../patterns/circuit-breaker.md
[compensating-transaction-pattern]: ../patterns/compensating-transaction.md
[containers]: https://en.wikipedia.org/wiki/Operating-system-level_virtualization
[dsc]: /azure/automation/automation-dsc-overview
[contingency-planning-guide]: https://nvlpubs.nist.gov/nistpubs/Legacy/SP/nistspecialpublication800-34r1.pdf
[fma]: ./failure-mode-analysis.md
[hystrix]: https://medium.com/netflix-techblog/introducing-hystrix-for-resilience-engineering-13531c1ab362
[jmeter]: https://jmeter.apache.org/
[load-leveling-pattern]: ../patterns/queue-based-load-leveling.md
[monitoring-guidance]: ../best-practices/monitoring.md
[ra-basic-web]: ../reference-architectures/app-service-web-app/basic-web-app.md
[ra-multi-vm]: ../reference-architectures/virtual-machines-windows/multi-vm.md
[checklist]: ../checklist/resiliency.md
[retry-pattern]: ../patterns/retry.md
[retry-service-specific guidance]: ../best-practices/retry-service-specific.md
[sla]: https://azure.microsoft.com/support/legal/sla/
[throttling-pattern]: ../patterns/throttling.md
[tm]: https://azure.microsoft.com/services/traffic-manager/
[tm-failover]: /azure/traffic-manager/traffic-manager-monitoring
[tm-sla]: https://azure.microsoft.com/support/legal/sla/traffic-manager
[site-recovery]: /azure/site-recovery/azure-to-azure-quickstart/
[site-recovery-test-failover]: /azure/site-recovery/azure-to-azure-tutorial-dr-drill/
[site-recovery-failover]: /azure/site-recovery/azure-to-azure-tutorial-failover-failback/
[deployment-topologies]: ./disaster-recovery-azure-applications.md#deployment-topologies-for-disaster-recovery
[bulkhead-pattern]: ../patterns/bulkhead.md
[terraform]: /azure/virtual-machines/windows/infrastructure-automation#terraform
[ansible]: /azure/virtual-machines/windows/infrastructure-automation#ansible
[chef]: /azure/virtual-machines/windows/infrastructure-automation#chef
[puppet]: /azure/virtual-machines/windows/infrastructure-automation#puppet
[template-deployment]: /azure/azure-resource-manager/resource-group-overview#template-deployment
[cloud-init]: /azure/virtual-machines/windows/infrastructure-automation#cloud-init
[azure-devops-services]: /azure/virtual-machines/windows/infrastructure-automation#azure-devops-services
[jenkins]: /azure/virtual-machines/windows/infrastructure-automation#jenkins
[staging-slots]: /azure/app-service/deploy-staging-slots
[powershell]: /powershell/azure/overview
[cli]: /cli/azure
