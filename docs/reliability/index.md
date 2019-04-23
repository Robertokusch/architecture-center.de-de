---
title: Entwerfen zuverlässiger Azure-Anwendungen
description: Einführung in die Erstellung zuverlässiger und hoch verfügbarer Azure-Anwendungen
author: MikeWasson
ms.date: 04/10/2019
ms.topic: article
ms.service: architecture-center
ms.subservice: cloud-design-principles
ms.custom: ''
ms.openlocfilehash: 16c1daeed9b1d79f33051eb69571f1574bf9be4d
ms.sourcegitcommit: 579c39ff4b776704ead17a006bf24cd4cdc65edd
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/17/2019
ms.locfileid: "59644009"
---
# <a name="designing-reliable-azure-applications"></a>Entwerfen zuverlässiger Azure-Anwendungen

Die Erstellung einer zuverlässigen Anwendung in der Cloud unterscheidet sich von der herkömmlichen Anwendungsentwicklung. Früher haben Sie wahrscheinlich höherwertige Hardware angeschafft, um zentral hochzuskalieren. In einer Cloudumgebung wird jedoch nicht zentral hochskaliert, sondern horizontal. Es geht nicht darum, Fehler vollständig zu verhindern, sondern darum, die Auswirkungen einer einzelnen fehlerhaften Komponente zu minimieren.

Zuverlässige Anwendungen zeichnen sich durch Folgendes aus:

- **Resilienz:** Sie lassen sich nach einem Fehler ordnungsgemäß wiederherstellen und bis zur vollständigen Wiederherstellung mit minimaler Downtime und minimalen Datenverlusten weiterverwenden.
- **Hochverfügbarkeit (High Availability, HA):** Sie werden bestimmungsgemäß in einem fehlerfreien Zustand ohne nennenswerte Downtime ausgeführt.

Entwickler, die eine zuverlässige Anwendung erstellen möchten, müssen verstehen, wie diese Elemente zusammenarbeiten und welche Auswirkungen sie auf die Kosten haben. Dies kann Ihnen dabei helfen, zu bestimmen, wie viel Downtime akzeptabel ist, welche potenziellen Kosten für Ihr Unternehmen entstehen und welche Funktionen während einer Wiederherstellung benötigt werden.

Dieser Artikel bietet einen kurzen Überblick über die Integration von Zuverlässigkeit in die einzelnen Schritte des Entwurfsprozesses für Azure-Anwendungen. Die einzelnen Abschnitte enthalten jeweils einen Link zu einem Artikel, in dem ausführlich erläutert wird, wie Sie Zuverlässigkeit in diesen speziellen Schritt integrieren. Zuverlässigkeitsaspekte für einzelne Azure-Dienste finden Sie in der [Checkliste für Resilienz für bestimmte Azure-Dienste](../checklist/resiliency-per-service.md).

## <a name="build-for-reliability"></a>Erstellen mit Zuverlässigkeit

In diesem Abschnitt werden sechs Schritte für die Erstellung einer zuverlässigen Azure-Anwendung beschrieben. Jeder Schritt enthält einen Link zu einem Abschnitt mit einer ausführlicheren Beschreibung des Prozesses und der Bedingungen.

1. [**Definieren von Anforderungen:**](#define-requirements) Entwickeln Sie Verfügbarkeits- und Wiederherstellungsanforderungen auf der Grundlage zerlegter Workloads und geschäftlicher Anforderungen.
1. [**Verwenden von bewährten Methoden für die Architektur:**](#use-architectural-best-practices) Implementieren Sie bewährte Verfahren, identifizieren Sie potenzielle Schwachstellen in der Architektur, und ermitteln Sie, wie die Anwendung auf Fehler reagiert.
1. [**Testen mit Simulationen und erzwungenen Failovern:**](#test-with-simulations-and-forced-failovers) Simulieren Sie Ausfälle, lösen Sie erzwungene Failover aus, und testen Sie die Erkennung und die Wiederherstellung nach solchen Fehlern.
1. [**Konsistentes Bereitstellen der Anwendung:**](#deploy-the-application-consistently) Verwenden Sie für die Veröffentlichung in der Produktionsumgebung zuverlässige und wiederholbare Prozesse.
1. [**Überwachen der Anwendungsintegrität:**](#monitor-application-health) Erkennen Sie Fehler, überwachen Sie Indikatoren für potenzielle Fehler, und bestimmen Sie die Integrität Ihrer Anwendungen.
1. [**Reagieren auf Fehler und Notfälle:**](#respond-to-failures-and-disasters) Erkennen Sie auftretende Fehler, und bestimmen Sie geeignete Maßnahmen auf der Grundlage bewährter Strategien.

## <a name="define-requirements"></a>Definieren von Anforderungen

Bestimmen Sie Ihre geschäftlichen Anforderungen, und entwickeln Sie einen geeigneten Zuverlässigkeitsplan. Beachten Sie Folgendes:

- **Ermitteln Sie Workloads und Nutzung:** Eine *Workload* ist eine bestimmte Funktion oder Aufgabe, die in Bezug auf die Geschäftslogik und die Datenspeicheranforderungen logisch von anderen Aufgaben getrennt ist. Jede Workload hat andere Anforderungen hinsichtlich Verfügbarkeit, Skalierbarkeit, Datenkonsistenz und Notfallwiederherstellung.
- **Planen Sie für Nutzungsmuster:** Auch *Nutzungsmuster* sind für die Anforderungen relevant. Ermitteln Sie Unterschiede bei den Anforderungen während kritischer und nicht kritischer Zeiträume. Eine Anwendung für Steuererklärungen darf beispielsweise keinesfalls kurz vor dem Einreichungsdatum ausfallen. Planen Sie Redundanz über mehrere Regionen hinweg, um die Verfügbarkeit auch dann zu gewährleisten, wenn eine der Regionen ausfällt. Während nicht kritischer Zeiträume können Sie Ihre Anwendung dagegen in einer einzelnen Region ausführen, um Kosten zu sparen.
- **Richten Sie die Verfügbarkeitsmetriken *MTTR* (Mean Time To Recover) und *MTBF* (Mean Time Between Failures) ein:** MTTR ist die durchschnittliche Zeit, die nach einem Fehler zur Wiederherstellung einer Komponente erforderlich ist. MTBF ist die Laufzeit, die für eine Komponente realistisch zwischen Ausfällen zu erwarten ist. Bestimmen Sie anhand dieser Measures, wo Redundanz erforderlich ist und welche Vereinbarungen zum Servicelevel (Service Level Agreements, SLAs) für Kunden geeignet sind.  
- **Richten Sie die Wiederherstellungsmetriken RTO (Recovery Time Objective) und RPO (Recovery Point Objective) ein:** *RTO* ist der maximal zulässige Zeitraum, in dem eine Anwendung nach einem Vorfall nicht verfügbar sein darf. *RPO* ist der maximale Zeitraum, in dem Datenverluste bei einem Notfall akzeptabel sind. Führen Sie zum Ermitteln dieser Werte eine Risikobewertung durch, und machen Sie sich mit den Kosten und Risiken von Downtime oder Datenverlusten in Ihrer Organisation vertraut.
    > [!NOTE]
    > Wenn in einer Hochverfügbarkeitsumgebung der MTTR *einer beliebigen* kritischen Komponente über dem RTO des Systems liegt, führt ein Fehler im System unter Umständen zu einer nicht akzeptablen Störung des Geschäftsbetriebs. Mit anderen Worten: Das System kann nicht innerhalb des definierten RTO-Zeitraums wiederhergestellt werden.
- **Bestimmen Sie die Verfügbarkeitsziele für die Workloads:** Definieren Sie Ziel-SLAs für jede Workload, um sicherzustellen, dass die Anwendungsarchitektur Ihre geschäftlichen Anforderungen erfüllt. Berücksichtigen Sie dabei neben Anwendungsabhängigkeiten auch die Kosten und die Komplexität der Erfüllung von Verfügbarkeitsanforderungen.
- **Machen Sie sich mit Vereinbarungen zum Servicelevel vertraut:** In Azure beschreibt die SLA die von Microsoft zugesicherte Verfügbarkeit und Konnektivität. Wenn die Vereinbarung zum Servicelevel für einen bestimmten Dienst 99,9 Prozent beträgt, können Sie erwarten, dass der Dienst 99,9 Prozent der Zeit verfügbar ist.  

    Definieren Sie eigene Ziel-SLAs für die einzelnen Workloads in Ihrer Lösung, um bestimmen zu können, ob die Architektur die geschäftlichen Anforderungen erfüllt. Ein Beispiel: Wenn für eine Workload eine Betriebszeit von 99,99 Prozent erforderlich ist, die Workload aber von einem Dienst mit einer SLA mit 99,9 Prozent abhängig ist, darf dieser Dienst im System kein Single Point of Failure sein.

Weitere Informationen zum Entwickeln von Anforderungen für zuverlässige Anwendungen finden Sie unter [Developing requirements for resilient Azure applications](./requirements.md) (Entwickeln von Anforderungen für resiliente Azure-Anwendungen).

## <a name="use-architectural-best-practices"></a>Verwenden von bewährten Methoden für die Architektur

Konzentrieren Sie sich während der Architekturphase auf die Implementierung von Verfahren, die Ihre geschäftlichen Anforderungen erfüllen, Schwachstellen aufdecken und das Fehlerspektrum minimieren.

- **Führen Sie eine Fehlermodusanalyse (FMA) durch:** Mit der FMA lässt sich Resilienz bereits zu einem frühen Zeitpunkt der Entwurfsphase in eine Anwendung integrieren. Damit können Sie die Arten von Fehlern, die in Ihrer Anwendung auftreten können, sowie potenzielle Auswirkungen und mögliche Wiederherstellungsstrategien bestimmen.
- **Erstellen Sie einen Redundanzplan:** Der Redundanzbedarf für die einzelnen Workloads hängt jeweils von Ihren geschäftlichen Anforderungen ab und fließt in die Gesamtkosten Ihrer Anwendung ein. 
- **Entwickeln Sie einen auf Skalierbarkeit ausgelegten Entwurf:** Eine Cloudanwendung muss skalierbar sein, um auf Veränderungen bei der Nutzung reagieren zu können. Beginnen Sie mit einzelnen Komponenten, und gestalten Sie die Anwendung nach Möglichkeit so, dass sie automatisch auf Laständerungen reagiert. Achten Sie bei Ihrem Entwurf auf Skalierungsgrenzwerte, um später eine problemlose Erweiterung zu ermöglichen.
- **Berücksichtigen Sie Abonnement- und Dienstanforderungen bei der Planung:** Unter Umständen benötigen Sie zusätzliche Abonnements, um genügend Ressourcen für Ihre geschäftlichen Anforderungen in puncto Speicher, Verbindungen, Durchsatz und Ähnlichem bereitstellen zu können. 
- **Verwenden Sie einen Lastenausgleich für die Verteilung von Anforderungen:** Ein Lastenausgleich verteilt die Anforderungen Ihrer Anwendung an fehlerfreie Dienstinstanzen, indem fehlerhafte Instanzen aus der Rotation entfernt werden.
- **Implementieren Sie Resilienzstrategien:** Bei der *Resilienz* (Robustheit) geht es um die Möglichkeit, nach Ausfällen für ein System eine Wiederherstellung durchzuführen und die Betriebsbereitschaft sicherzustellen. Implementieren Sie [Resilienzmuster](../patterns/category/resiliency.md), indem Sie beispielsweise kritische Ressourcen isolieren, ausgleichende Transaktionen verwenden und nach Möglichkeit asynchrone Vorgänge ausführen.
- **Integrieren Sie Verfügbarkeitsanforderungen in Ihren Entwurf:** *Verfügbarkeit* ist die Zeit, in der Ihr System funktioniert und erreichbar ist. Stellen Sie sicher, dass die Anwendungsverfügbarkeit mit Ihrer Vereinbarung zum Servicelevel in Einklang steht. Vermeiden Sie also beispielsweise Single Points of Failure, zerlegen Sie Workloads nach Servicelevelziel, und drosseln Sie Benutzer mit hohem Volumen.
- **Verwalten Sie Ihre Daten:** Die Art der verwendeten Datenspeicherung, Datensicherung und Datenreplizierung hat erhebliche Auswirkungen.

  - **Wählen Sie Replikationsmethoden für Ihre Anwendungsdaten:** Ihre Anwendungsdaten werden in unterschiedlichen Datenspeichern gespeichert und haben möglicherweise unterschiedliche Verfügbarkeitsanforderungen. Bewerten Sie die Replikationsmethoden und Standorte für die einzelnen Arten von Datenspeichern, um sicherzustellen, dass sie Ihren Anforderungen genügen.
  - **Dokumentieren und testen Sie Ihre Failover- und Failbackprozesse:** Dokumentieren Sie klare Anweisungen für das Failover auf einen neuen Datenspeicher, und testen Sie sie regelmäßig, um sicherzustellen, dass sie fehlerfrei und nachvollziehbar sind.
  - **Schützen Sie Ihre Daten:** Sichern und überprüfen Sie Daten regelmäßig, und achten Sie darauf, dass kein einzelnes Benutzerkonto sowohl auf Produktions- als auch auf Sicherungsdaten zugreifen kann.
  - **Entwickeln Sie einen Plan für die Datenwiederherstellung:** Stellen Sie sicher, dass Ihre Sicherungs- und Replikationsstrategie die erforderlichen Datenwiederherstellungszeiten für Ihre Servicelevelanforderungen bietet. Berücksichtigen Sie alle Arten von Daten, die von Ihrer Anwendung genutzt werden (einschließlich Referenzdaten und Datenbanken).

Weitere Informationen zum Entwerfen zuverlässiger Anwendungen finden Sie unter [Architecting Azure applications for resiliency and availability](./architect.md) (Entwerfen resilienter und verfügbarer Azure-Anwendungen).

## <a name="test-with-simulations-and-forced-failovers"></a>Testen mit Simulationen und erzwungenen Failovern

Um die Zuverlässigkeit zu testen, müssen Sie ermitteln, wie sich die End-to-End-Workload unter vorübergehenden Fehlerbedingungen verhält.

- **Testen Sie gängige Fehlerszenarien, indem Sie entweder echte Fehler auslösen oder Fehler simulieren:** Verwenden Sie Fault Injection-Tests, um gängige Szenarien (einschließlich Fehlerkombinationen) und die Wiederherstellungszeit zu testen.
- **Identifizieren Sie Fehler, die nur unter Last auftreten:** Führen Sie Tests bei Spitzenlast durch – entweder mit Produktionsdaten oder mit synthetischen Daten, die den Produktionsdaten möglichst genau ähneln. So können Sie ermitteln, wie sich die Anwendung in der Praxis verhält.
- **Führen Sie Übungen zur Notfallwiederherstellung durch:** Entwickeln Sie einen Notfallwiederherstellungsplan, und testen Sie ihn regelmäßig, um sicherzustellen, dass er funktioniert.
- **Führen Sie Failover- und Failbacktests durch:** Stellen Sie sicher, dass Failover- und Failbackvorgänge für die abhängigen Dienste Ihrer Anwendung in der richtigen Reihenfolge ausgeführt werden.
- **Führen Sie Simulationstests durch:** Mithilfe von Tests für realistische Szenarien lassen sich ggf. Probleme identifizieren, die behoben werden müssen. Die Szenarien müssen kontrollierbar sein und dürfen die Geschäftstätigkeit des Unternehmens nicht beeinträchtigen. Informieren Sie die Geschäftsführung über geplante Simulationstests.
- **Führen Sie Integritätstests durch:** Konfigurieren Sie Integritätstests für Lastenausgleichsmodule und Traffic Manager, um kritische Systemkomponenten zu überprüfen. Testen Sie sie, um sicherzustellen, dass sie korrekt reagieren.
- **Testen Sie Überwachungssysteme:** Vergewissern Sie sich, dass Überwachungssysteme zuverlässig kritische Informationen und korrekte Daten liefern, um potenzielle Fehler erkennen zu können.
- **Beziehen Sie Dienste von Drittanbietern in Testszenarien ein:** Testen Sie potenzielle Fehlerquellen, die auf Störungen bei Drittanbieterdiensten zurückzuführen sind, sowie die Wiederherstellung.

Der Testvorgang ist ein iterativer Prozess. Testen Sie die Anwendung, messen Sie das Ergebnis, analysieren und beheben Sie mögliche Fehler, und wiederholen Sie den Vorgang.

Weitere Informationen zum Testen der Anwendungszuverlässigkeit finden Sie unter [Testing Azure applications for resiliency and availability](./testing.md) (Testen der Resilienz und Verfügbarkeit von Azure-Anwendungen).

## <a name="deploy-the-application-consistently"></a>Konsistentes Bereitstellen der Anwendung

*Bereitstellen* beinhaltet das Bereitstellen von Azure-Ressourcen und Anwendungscode sowie das Anwenden von Konfigurationseinstellungen. Ein Update kann alle drei Aufgaben oder nur einen Teil davon umfassen.

Nachdem eine Anwendung in der Produktionsumgebung bereitgestellt wurde, sind Updates eine mögliche Fehlerquelle. Minimieren Sie Fehler durch planbare und wiederholbare Bereitstellungsprozesse.

- **Automatisieren Sie den Bereitstellungsprozess für Ihre Anwendung:** Automatisieren Sie so viele Prozesse wie möglich.
- **Gestalten Sie Ihren Releaseprozess so, dass ein Höchstmaß an Verfügbarkeit erreicht wird:** Sollte es für Ihren Releaseprozess erforderlich sein, dass Dienste während der Bereitstellung offline geschaltet werden, ist die Anwendung erst wieder verfügbar, wenn die Dienste wieder online sind. Nutzen Sie Staging- und Produktionsfeatures der Plattform. Arbeiten Sie bei der Updatebereitstellung mit Blau-Grün- oder Canary-Releases, um im Falle eines Fehlers schnell ein Rollback für das Update ausführen zu können.
- **Bereiten Sie einen Rollbackplan für die Bereitstellung vor:** Entwerfen Sie einen Rollbackprozess, um bei einem Bereitstellungsfehler zu einer letzten als funktionierend bekannten Version zurückkehren und die Downtime minimieren zu können.
- **Protokollieren und überwachen Sie Bereitstellungen:** Bei Verwendung von Bereitstellungstechniken mit Staging werden in der Produktionsumgebung mehrere Versionen Ihrer Anwendung ausgeführt. Implementieren Sie eine stabile Protokollierungsstrategie, um so viele versionsspezifische Informationen wie möglich zu erfassen.
- **Dokumentieren Sie den Releaseprozess der Anwendung:** Definieren und dokumentieren Sie den Releaseprozess eindeutig, und stellen Sie sicher, dass er für das gesamte Betriebsteam verfügbar ist.

Weitere Informationen zur Anwendungszuverlässigkeit und -bereitstellung finden Sie unter [Deploying Azure applications for resiliency and availability](./deploy.md) (Bereitstellen resilienter, verfügbarer Azure-Anwendungen).

## <a name="monitor-application-health"></a>Überwachen der Anwendungsintegrität

Implementieren Sie in Ihrer Anwendung bewährte Überwachungs- und Benachrichtigungsmethoden, um Fehler zu erkennen und einen Bediener zu informieren, damit dieser den Fehler behebt.

- **Implementieren Sie Integritätstests und Überprüfungsfunktionen:** Führen Sie diese regelmäßig von außerhalb der Anwendung aus, um Verschlechterungen bei der Anwendungsintegrität und -leistung zu ermitteln.
- **Überprüfen Sie Workflows mit langer Laufzeit:** Wenn Probleme frühzeitig erkannt werden, ist es seltener erforderlich, ein Rollback für den gesamten Workflow oder mehrere ausgleichende Transaktionen auszuführen.
- **Nutzen Sie Anwendungsprotokolle:**

  - Protokollieren Sie Anwendungen in der Produktionsumgebung und an den Dienstgrenzen.
  - Nutzen Sie semantische und asynchrone Protokollierung.
  - Trennen Sie Anwendungsprotokolle von Überwachungsprotokollen.

- **Erfassen Sie Statistikdaten für Remoteaufrufe, und geben Sie die Daten an das Anwendungsteam weiter:** Fassen Sie Remoteaufrufmetriken wie Wartezeit, Durchsatz und Fehler im 99. und 95. Perzentil zusammen, um Ihrem Betriebsteam einen unmittelbaren Einblick in die Integrität der Anwendung zu geben. Führen Sie statistische Analysen von Metriken durch, um Fehler zu erkennen, die in den einzelnen Perzentilen auftreten.
- **Verfolgen Sie vorübergehende Ausnahmen und Wiederholungsversuche in einem geeigneten Zeitrahmen nach:** Ein Trend zunehmender Ausnahmen im Zeitverlauf weist darauf hin, dass der Dienst möglicherweise ein Problem hat und ausfallen kann.
- **Richten Sie ein Frühwarnsystem ein:** Identifizieren Sie Key Performance Indicators (KPIs) für die Integrität Ihrer Anwendung (etwa vorübergehende Ausnahmen und die Wartezeit bei Remoteaufrufen), und legen Sie jeweils entsprechende Schwellenwerte fest. Senden Sie eine Warnung an Operatoren, wenn der Schwellenwert erreicht wird.
- **Halten Sie die Grenzwerte von Azure-Abonnements ein:** Bei Azure-Abonnements gelten Grenzwerte für bestimmte Ressourcentypen. Hierzu zählt beispielsweise die Anzahl von Ressourcengruppen, Kernen und Speicherkonten. Behalten Sie Ihre Nutzung von Ressourcentypen im Auge.
- **Überwachen Sie Dienste von Drittanbietern:** Protokollieren Sie Ihre Aufrufe, und korrelieren Sie sie unter Verwendung eines eindeutigen Bezeichners mit der Integrität und der Diagnoseprotokollierung Ihrer Anwendung.
- **Schulen Sie mehrere Bediener in der Überwachung der Anwendung und in der Ausführung manueller Wiederherstellungsschritte:** Stellen Sie sicher, dass immer mindestens ein geschulter Bediener aktiv ist.

Weitere Informationen zur Überwachung der Anwendungszuverlässigkeit finden Sie unter [Monitoring application health for reliability in Azure](./monitoring.md) (Ermitteln der Zuverlässigkeit durch Überwachen der Anwendungsintegrität in Azure).

## <a name="respond-to-failures-and-disasters"></a>Reagieren auf Fehler und Notfälle

Erstellen Sie einen Wiederherstellungsplan, der die Wiederherstellung von Daten, Netzwerkausfälle, Fehler abhängiger Dienste und regionsweite Dienstunterbrechungen abdeckt. Beziehen Sie Ihre virtuellen Computer, Ihren Speicher, Ihre Datenbanken sowie andere Azure-Plattformdienste in Ihre Wiederherstellungsstrategie ein.

- **Entwickeln Sie einen Plan für die Interaktion mit dem Azure-Support:** Richten Sie proaktiv einen Prozess für die Kontaktaufnahme mit dem Azure-Support ein.
- **Dokumentieren und testen Sie Ihren Plan für die Notfallwiederherstellung:** Erstellen Sie einen Notfallwiederherstellungsplan unter Berücksichtigung der geschäftlichen Auswirkungen von Anwendungsfehlern. Automatisieren Sie den Wiederherstellungsprozess so weit wie möglich, und dokumentieren Sie sämtliche manuellen Schritte. Testen Sie Ihren Prozess für die Notfallwiederherstellung regelmäßig, um den Plan zu überprüfen und zu verbessern.
- **Führen Sie bei Bedarf ein manuelles Failover aus:** Für manche Systeme kann kein automatisches Failover ausgeführt werden. In diesen Fällen ist ein manuelles Failover erforderlich. Testen Sie die Betriebsbereitschaft, wenn für eine Anwendung ein Failover auf eine sekundäre Region ausgeführt wird. Vergewissern Sie sich vor einem Failback, dass die primäre Region fehlerfrei und wieder für den Empfang von Datenverkehr bereit ist. Bestimmen Sie die eingeschränkte Anwendungsfunktionalität und wie die App Benutzer über vorübergehende Probleme informiert.
- **Treffen Sie Vorkehrungen für Anwendungsfehler:** Treffen Sie Vorkehrungen für verschiedene Fehler – einschließlich solche, die nicht automatisch behandelt werden können, die Funktionalität einschränken oder dazu führen, dass die Anwendung nicht mehr verfügbar ist. Die Anwendung muss Benutzer über vorübergehende Probleme informieren.
- **Wiederherstellung nach einer Datenbeschädigung:** Überprüfen Sie Daten nach einem Fehler in einem Datenspeicher auf Inkonsistenzen, sobald der Speicher wieder verfügbar ist (insbesondere, wenn die Daten repliziert wurden). Stellen Sie für beschädigte Daten eine Sicherung wieder her.
- **Wiederherstellung nach einem Netzwerkausfall:** Die Anwendung kann möglicherweise lokal mit zwischengespeicherten Daten und eingeschränkter Funktionalität ausgeführt werden. Falls nicht, müssen Sie entweder die Downtime der Anwendung in Kauf nehmen oder ein Failover auf eine andere Region ausführen. Speichern Sie Ihre Daten an einem alternativen Speicherort, bis die Verbindung wiederhergestellt wurde.
- **Wiederherstellung nach einem Fehler eines abhängigen Diensts:** Bestimmen Sie, welche Funktionen weiterhin zur Verfügung stehen und wie die Anwendung reagieren soll.
- **Wiederherstellung nach einer regionsweiten Dienstunterbrechung:** Regionsweite Dienstunterbrechungen sind zwar selten, Sie sollten aber dennoch über eine entsprechende Strategie verfügen – insbesondere für kritische Anwendungen. Möglicherweise können Sie die Anwendung in einer anderen Region erneut bereitstellen oder den Datenverkehr weiterverteilen.

Weitere Informationen zum Umgang mit Fehlern und zur Notfallwiederherstellung finden Sie unter [Failure and disaster recovery for Azure applications](./disaster-recovery.md) (Wiederherstellung bei einem Fehler oder Notfall für Azure-Anwendungen).

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Developing requirements for resilient Azure applications](./requirements.md) (Ausarbeiten von Anforderungen für resiliente Anwendungen)
