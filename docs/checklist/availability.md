---
title: "Checkliste für die Verfügbarkeit"
description: "Checkliste, die Hinweise zu Überlegungen hinsichtlich der Verfügbarkeit während des Entwurfs bereitstellt."
author: dragon119
ms.date: 03/24/2017
ms.custom: checklist
ms.openlocfilehash: 14e6cd6f25f613ea9793b5cf6c4f1abe6f405b74
ms.sourcegitcommit: b0482d49aab0526be386837702e7724c61232c60
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/14/2017
---
# <a name="availability-checklist"></a>Checkliste für die Verfügbarkeit
[!INCLUDE [header](../_includes/header.md)]

## <a name="application-design"></a>Anwendungsentwurf
* **Vermeiden Sie alle einzelnen Fehlerquellen.** Alle Komponenten, Dienste, Ressourcen und Serverinstanzen sollten in mehreren Instanzen bereitgestellt werden, um zu verhindern, dass sich eine einzelne Fehlerquelle auf die Verfügbarkeit auswirkt. Dies gilt auch für die Authentifizierungsmechanismen. Entwerfen Sie die Anwendung so, dass sie für die Verwendung mehrerer Instanzen konfiguriert werden kann, automatisch Fehler ermittelt und die Anforderungen bei einem Ausfall an einwandfreie Instanzen weiterleitet, wenn die Plattform dies nicht automatisch tut.
* **Teilen Sie die Workload nach den unterschiedlichen SLA auf.** Wenn ein Dienst wichtige und weniger wichtige Workloads umfasst, verwalten Sie sie unterschiedlich, und legen Sie die Dienstfunktionen und die Anzahl von Instanzen entsprechend den jeweiligen Anforderungen an die Verfügbarkeit fest.
* **Minimieren und erkennen Sie Abhängigkeiten des Dienstes.** Minimieren Sie die Anzahl der verschiedenen verwendeten Dienste, wenn möglich, und stellen Sie sicher, dass Sie alle im System vorhandenen Abhängigkeiten von Funktionen und Diensten kennen. Dazu gehört die Art dieser Abhängigkeiten und die Auswirkungen eines Ausfalls oder einer Leistungsbeeinträchtigung auf die Gesamtleistung der Anwendung. Microsoft garantiert für die meisten Dienste eine Verfügbarkeit von mindestens 99,9 %. Dies bedeutet jedoch, dass jeder zusätzliche Dienst, von dem eine Anwendung abhängig ist, die SLA für die Gesamtverfügbarkeit des Systems potenziell um 0,1 % verringert.
* **Entwerfen Sie Aufgaben und Nachrichten möglichst so, dass sie idempotent (sicher wiederholbar) sind**, damit doppelte Anforderungen keine Probleme verursachen. Beispielsweise kann ein Dienst als Consumer fungieren, der Nachrichten von anderen Teilen des Systems, die als Producer fungieren, als Anforderungen behandelt. Fällt der Consumer nach der Verarbeitung der Nachricht, aber vor der Bestätigung ihrer Verarbeitung aus, kann ein Producer eine Wiederholungsanforderung senden, die von einer anderen Instanz des Consumers verarbeitet werden kann. Aus diesem Grund sollten Consumer und die Vorgänge, die sie ausführen, idempotent sein, damit die Wiederholung eines zuvor ausgeführten Vorgangs nicht zu ungültigen Ergebnissen führt. Dies kann bedeuten, dass doppelte Nachrichten erkannt werden müssen oder dass die Konsistenz durch einen optimistischen Konfliktlösungsansatz gewährleistet sein muss.
* **Verwenden Sie einen Nachrichtenmakler, der für wichtige Transaktionen hohe Verfügbarkeit implementiert.** In vielen Szenarien, in denen Aufgaben initiiert werden oder auf Remotedienste zugegriffen wird, werden die Anweisungen zwischen der Anwendung und dem Zieldienst durch Nachrichten übergeben. Um eine optimale Leistung zu erzielen, sollte die Anwendung in der Lage sein, eine Nachricht zu senden und anschließend mit der Verarbeitung weiterer Anforderungen fortzufahren, ohne auf eine Antwort warten zu müssen. Um die Nachrichtenübermittlung zu gewährleisten, sollte das Messaging-System hohe Verfügbarkeit bieten. Azure Service Bus-Nachrichtenwarteschlangen implementieren eine *Mindestens-einmal-Semantik*. Dies bedeutet, dass keine an eine Warteschlange gesendete Nachricht verloren geht, obwohl unter bestimmten Umständen Nachrichten mehrfach übermittelt werden können. Wenn die Nachrichtenverarbeitung idempotent ist (siehe vorheriger Punkt), sollte die wiederholte Übermittlung kein Problem darstellen.
* **Entwerfen Sie Anwendungen, die ihre Anforderungen ordnungsgemäß herabstufen**, wenn Ressourcenlimits erreicht werden, und leiten Sie entsprechende Maßnahmen ein, um die Auswirkungen auf die Benutzer zu minimieren. In einigen Fällen kann die Auslastung der Anwendung die Kapazität von einem oder mehreren Teilen überschreiten und zu verringerter Verfügbarkeit und fehlgeschlagenen Verbindungen führen. Dieses Problem lässt sich durch Skalierung entschärfen, wobei jedoch eine durch andere Faktoren wie z. B. die Ressourcenverfügbarkeit oder Kosten bedingte Grenze erreicht werden kann. Entwerfen Sie die Anwendung so, dass sie in einer solchen Situation automatisch ordnungsgemäß herabgestuft werden kann. Wenn beispielsweise in einem E-Commerce-System das Subsystem zur Auftragsverarbeitung stark ausgelastet (oder sogar vollständig ausgefallen) ist, kann es vorübergehend deaktiviert werden, während andere Funktionen (z. B. zum Durchsuchen des Produktkatalogs) weiterhin ausgeführt werden können. Es kann zweckmäßig sein, die Anforderungen, die an ein ausgefallenes Subsystem gesendet werden, aufzuschieben, z. B. indem zugelassen wird, dass die Kunden Bestellungen absenden, diese jedoch für die spätere Verarbeitung gespeichert werden, wenn das Subsystem für Bestellungen wieder verfügbar ist.
* **Verarbeiten Sie schnelle Änderungen der Auslastung ordnungsgemäß.** Die meisten Anwendungen müssen im Laufe der Zeit unterschiedlich starke Auslastungen handhaben können, z. B. Auslastungsspitzen am Morgen in einer Geschäftsanwendung oder wenn ein neues Produkt in einer E-Commerce-Website veröffentlicht wird. Die automatische Skalierung kann bei der Bewältigung der Last hilfreich sein, es kann jedoch einige Zeit dauern, bis weitere Instanzen online geschaltet sind und Anforderungen verarbeiten. Verhindern Sie, dass die Anwendung durch einen plötzlichen und unvorhergesehenen Anstieg der Aktivität überfordert wird. Entwerfen Sie die Anwendung deshalb so, dass Anforderungen an Dienste in Warteschlangen eingefügt werden und dass die Anwendung ordnungsgemäß herabgestuft wird, wenn die Warteschlangen ihre Kapazitätsgrenze fast erreicht haben. Stellen Sie sicher, dass unter normalen Bedingungen ausreichend Leistung und Kapazität zur Verfügung steht, um die Warteschlangen zu leeren und die ausstehenden Anforderungen zu verarbeiten. Weitere Informationen finden Sie unter [Warteschlangenbasiertes Lastenausgleichsmuster](https://msdn.microsoft.com/library/dn589783.aspx).

## <a name="deployment-and-maintenance"></a>Bereitstellung und Wartung
* **Stellen Sie für jeden Dienst mehrere Rolleninstanzen bereit.** Microsoft garantiert die Verfügbarkeit von Diensten, die Sie erstellen und bereitstellen, aber diese Garantien sind nur gültig, wenn Sie mindestens zwei Instanzen jeder Rolle im Dienst bereitstellen. Dadurch kann eine Rolle nicht verfügbar sein, während die andere aktiv bleibt. Dies ist besonders wichtig, wenn Sie Updates auf einem Live-System bereitstellen müssen, ohne die Aktivitäten der Clients zu unterbrechen. Instanzen können außer Betrieb genommen und einzeln aktualisiert werden, während die anderen weiterhin online bleiben.
* **Hosten Sie Anwendungen in verschiedenen Rechenzentren.** Es ist zwar extrem unwahrscheinlich, aber möglich, dass ein gesamtes Rechenzentrum durch ein Ereignis, z. B. eine Naturkatastrophe oder ein Ausfall des Internets, offline geschaltet wird. Wichtige Geschäftsanwendungen sollten in mehreren Rechenzentren gehostet werden, um maximale Verfügbarkeit zu gewährleisten. Dies kann zudem die Wartezeit für lokale Benutzer verringern und bietet zusätzliche Flexibilität beim Aktualisieren von Anwendungen.
* **Automatisieren und testen Sie Bereitstellungs- und Wartungsaufgaben.** Verteilte Anwendungen bestehen aus mehreren Teilen, die zusammenarbeiten müssen. Die Bereitstellung sollte daher mithilfe getesteter und bewährter Mechanismen automatisiert werden, z. B. mit Skripts und Bereitstellungsanwendungen. Diese Skripts und Bereitstellungsanwendungen können die Konfiguration aktualisieren und überprüfen und den Bereitstellungsprozess automatisieren.  Automatisierte Verfahren sollten auch verwendet werden, um eine gesamte Anwendung oder Teile von Anwendungen zu aktualisieren. Es ist wichtig, alle diese Prozesse vollständig zu testen, um sicherzustellen, dass Fehler nicht zu zusätzlichen Ausfallzeit führen. Alle Bereitstellungstools müssen geeignete Sicherheitseinschränkungen zum Schutz der bereitgestellten Anwendung aufweisen. Definieren und erzwingen Sie Bereitstellungsrichtlinien sorgfältig, und minimieren Sie den Bedarf an manuellen Eingriffen.
* **Ziehen Sie gegebenenfalls die Verwendung von Staging- und Produktionsfunktionen der Plattform in Betracht.** Mithilfe von Azure Cloud Services Staging- und Produktionsumgebungen können Anwendungen z. B. sofort durch einen Austausch der virtuellen IP-Adresse (VIP-Swap bzw. VIP-Austausch) gewechselt werden. Wenn Sie jedoch lieber lokal oder verschiedene Versionen der Anwendung gleichzeitig bereitstellen und die Benutzer schrittweise migrieren möchten, dann können Sie möglicherweise den VIP-Swap-Vorgang nicht einsetzen.
* **Übernehmen Sie Konfigurationsänderungen, möglichst ohne die Instanz neu zu starten** . In vielen Fällen können die Konfigurationseinstellungen für eine Azure-Anwendung oder einen Dienst geändert werden, ohne dass die Rolle neu gestartet werden muss. Rollen machen Ereignisse verfügbar, die behandelt werden können, um Konfigurationsänderungen zu erkennen und auf Komponenten innerhalb der Anwendung anzuwenden. Einige Änderungen an den Kernplattformeinstellungen erfordern jedoch einen Neustart der Rolle. Optimieren Sie beim Erstellen von Komponenten und Diensten die Verfügbarkeit, und minimieren Sie Ausfallzeiten zu, indem Sie sie so entwerfen, dass die Übernahme von Änderungen an den Konfigurationseinstellungen keinen Neustart der gesamten Anwendung erfordert.
* **Verwenden Sie während Aktualisierungen Upgradedomänen, um Ausfallzeiten zu vermeiden.** Azure-Servereinheiten, wie z. B. Web- und Workerrollen, werden Upgradedomänen zugewiesen. In Upgradedomänen werden Rolleninstanzen so in Gruppen zusammengefasst, dass bei der Ausführung von parallelen Updates jede Rolle in der Upgradedomäne nacheinander beendet, aktualisiert und neu gestartet wird. So können die Auswirkungen auf die Verfügbarkeit der Anwendung minimiert werden. Sie können angeben, wie viele Upgradedomänen für einen Dienst erstellt werden sollen, wenn der Dienst bereitgestellt wird.
  
  > [!NOTE]
  > Rollen werden auch auf Fehlerdomänen verteilt, die jeweils in Bezug auf Serverrack, Stromversorgung und Kühlung von anderen Fehlerdomänen halbwegs unabhängig sind, um das Risiko eines Ausfalls zu minimieren, der sich auf alle Rolleninstanzen auswirkt. Diese Verteilung erfolgt automatisch, und Sie können sie nicht steuern.
  > 
  > 
* **Konfigurieren Sie Verfügbarkeitsgruppen für virtuelle Azure-Computer.** Durch die Platzierung von zwei oder mehr virtuellen Computern in der gleichen Verfügbarkeitsgruppe wird garantiert, dass diese virtuellen Computer nicht in derselben Fehlerdomäne bereitgestellt werden. Um die Verfügbarkeit zu maximieren, sollten Sie mehrere Instanzen aller wichtigen in Ihrem System verwendeten virtuellen Computer erstellen und diese Instanzen in derselben Verfügbarkeitsgruppe platzieren. Wenn Sie mehrere virtuelle Computer ausführen, die unterschiedlichen Zwecken dienen, erstellen Sie für jeden virtuellen Computer eine Verfügbarkeitsgruppe. Fügen Sie die Instanzen der einzelnen virtuellen Computer anschließend den einzelnen Verfügbarkeitsgruppen hinzu. Wenn Sie z. B. separate virtuelle Computer erstellt haben, die als Webserver und Berichtsserver fungieren, dann erstellen Sie eine Verfügbarkeitsgruppe für den Webserver und eine andere Verfügbarkeitsgruppe für den Berichtsserver. Fügen Sie Instanzen des virtuellen Webserver-Computers der Webserver-Verfügbarkeitsgruppe hinzu, und fügen Sie Instanzen des virtuellen Berichtsserver-Computers der Berichtsserver-Verfügbarkeitsgruppe hinzu.

## <a name="data-management"></a>Datenverwaltung

* **Führen Sie die Georeplikation für Daten in Azure Storage durch.** Daten in Azure Storage werden in einem Datencenter automatisch repliziert. Verwenden Sie zur Erzielung einer noch höheren Verfügbarkeit „Georedundanten Speicher mit Lesezugriff“ (Read-access geo-redundant storage, RA-GRS). Hierbei werden Ihre Daten in eine sekundäre Region repliziert, und es wird Lesezugriff auf die Daten am sekundären Standort gewährt. Die Daten bleiben auch bei einem umfassenden regionalen Ausfall oder Katastrophenfall erhalten. Weitere Informationen finden Sie unter [Azure Storage-Replikation](/azure/storage/storage-redundancy).
* **Führen Sie die Georeplikation für Datenbanken durch.** Für Azure SQL-Datenbank und Cosmos DB wird jeweils die Georeplikation unterstützt, sodass Sie sekundäre Datenbankreplikate in anderen Regionen konfigurieren können. Sekundäre Datenbanken stehen für Abfragen und Failover zur Verfügung, wenn ein Rechenzentrum ausgefallen ist oder keine Verbindung mit der primären Datenbank möglich ist. Weitere Informationen finden Sie unter [Failovergruppen und aktive Georeplikation](/azure/sql-database/sql-database-geo-replication-overview) (SQL-Datenbank) und [Wie werden Daten mit Azure Cosmos DB global verteilt?](/azure/cosmos-db/distribute-data-globally).
* **Verwenden Sie optimistische Parallelität und letztendliche Konsistenz**, wenn möglich. Transaktionen, die den Zugriff auf Ressourcen durch Sperren (pessimistische Parallelität) blockieren, können die Leistung beeinträchtigen und die Verfügbarkeit erheblich reduzieren. Diese Probleme können insbesondere in verteilten Systemen akut werden. In vielen Fällen lässt sich die Wahrscheinlichkeit, dass miteinander in Konflikt stehende Aktualisierungen auftreten, durch sorgfältige Planung und Techniken, wie z. B. die Partitionierung, minimieren. Wo Daten repliziert oder aus einem separat aktualisierten Speicher gelesen werden, sind die Daten erst zum Schluss konsistent. Die Vorteile überwiegen aber in der Regel bei weitem gegenüber den Auswirkungen, die die Verwendung von Transaktionen zur Gewährleistung der sofortigen Konsistenz auf die Verfügbarkeit haben.
* **Verwenden Sie regelmäßige Sicherungen und die Zeitpunktwiederherstellung**, und stellen Sie sicher, dass diese der RPO (Recovery Point Objective) entspricht. Sichern Sie regelmäßig und automatisch Daten, die nicht an anderer Stelle gespeichert werden, und stellen Sie sicher, dass Sie die Daten und die Anwendung selbst nach einem Ausfall zuverlässig wiederherstellen können. Die Datenreplikation ist keine Sicherungsfunktion, weil Fehler und Inkonsistenzen, die durch einen Ausfall, durch Fehler oder schädliche Vorgänge eingeführt werden, über alle Speicher hinweg repliziert werden. Der Sicherungsvorgang muss sicher sein, sodass die Daten während der Übertragung und Speicherung geschützt sind. Datenbanken oder Teile eines Datenspeichers können in der Regel mithilfe von Transaktionsprotokollen in einem Zustand wiederhergestellt werden, den sie zu einem früheren Zeitpunkt hatten. Microsoft Azure bietet eine Sicherungsfunktion für Daten, die in Azure SQL-Datenbank gespeichert sind. Die Daten werden in ein Sicherungspaket in Azure Blob Storage exportiert und können zur Ablage an einem sicheren lokalen Speicherort heruntergeladen werden.
* **Aktivieren Sie die Option für hohe Verfügbarkeit, um eine sekundäre Kopie eines Azure Redis Cache zu verwalten.** Bei Verwendung von Azure Redis Cache verwenden Sie die Option „Standard“, um eine sekundäre Kopie des Inhalts zu verwalten. Weitere Informationen finden Sie unter [Erstellen eines Caches in Azure Redis Cache](https://msdn.microsoft.com/library/dn690516.aspx).

## <a name="errors-and-failures"></a>Fehler und Ausfälle
* **Führen Sie das Konzept der Zeitüberschreitung ein.** Dienste und Ressourcen können möglicherweise nicht mehr verfügbar sein, sodass bei Anforderungen Fehler auftreten. Stellen Sie sicher, dass Sie Timeouts festlegen, die für den jeweiligen Dienst oder die Ressource und auch für den Client geeignet sind, der auf diese zugreift. (In einigen Fällen kann es zweckmäßig sein, ein längeres Zeitlimit für eine bestimmte Clientinstanz festzulegen, je nach Kontext und den weiteren Aktionen, die der Client ausführt.) Sehr kurze Timeouts können zu übermäßig vielen Wiederholungsvorgängen bei Diensten und Ressourcen mit höherer Latenz führen.  Sehr lange Timeouts können zu einer Blockade führen, wenn sich eine große Anzahl von Anforderungen in der Warteschlange befindet und darauf wartet, dass ein Dienst oder eine Ressource reagiert.
* **Wiederholen Sie die fehlgeschlagenen Vorgänge, die durch vorübergehende Fehler verursacht wurden.** Entwerfen Sie eine Strategie für wiederholte Zugriffsversuche auf alle Dienste und Ressourcen, die nicht grundsätzlich eine automatische Verbindungswiederherstellung unterstützen. Verwenden Sie eine Strategie mit einer Verzögerung zwischen den Wiederholungsversuchen, die mit zunehmender Fehleranzahl zunimmt, um eine Überlastung der Ressource zu verhindern und ihr ermöglichen, sich ordnungsgemäß wiederherzustellen und die in der Warteschlange enthaltenen Anforderungen zu verarbeiten. Fortwährende Wiederholungsversuche mit sehr kurzen Verzögerungen verschlimmern das Problem wahrscheinlich eher.
* **Beenden Sie das Senden von Anforderungen, um kaskadierende Fehler zu vermeiden**, wenn Remotedienste nicht verfügbar sind. Es gibt möglicherweise Situationen, in denen vorübergehende oder andere Fehler, deren Schweregrad von einer teilweisen Unterbrechung der Verbindungen bis zum vollständigen Ausfall eines Diensts reichen kann, viel länger als erwartet anhalten, bis zum normalen Betrieb zurückgekehrt werden kann. Wenn ein Dienst stark ausgelastet ist, kann ein Fehler in einem Teil des Systems zu kaskadierenden Fehlern und dazu führen, dass viele Vorgänge blockiert werden, während sie Sperren für kritische Systemressourcen halten, wie z. B. Arbeitsspeicher, Threads und Datenbankverbindungen. Anstatt kontinuierlich einen Vorgang zu wiederholen, der wahrscheinlich fehlschlägt, sollte die Anwendung schnell akzeptieren, dass der Vorgang fehlgeschlagen ist und diesen Fehler ordnungsgemäß behandeln. Sie können das Circuit Breaker-Muster verwenden, um für definierte Zeiträume Anforderungen für bestimmte Vorgänge abzulehnen. Weitere Informationen finden Sie unter [Circuit Breaker Pattern](../patterns/circuit-breaker.md) (Schutzschaltermuster).
* **Erstellen oder nutzen Sie mehrere Komponenten** , um die Auswirkungen des Ausfalls oder der Nichtverfügbarkeit eines Dienstes zu minimieren. Entwerfen Sie die Anwendungen möglichst so, dass sie mehrere Instanzen nutzen, ohne den Betrieb und die vorhandenen Verbindungen zu beeinträchtigen. Um die Verfügbarkeit zu maximieren, verwenden Sie mehrere Instanzen, teilen Sie die Anforderungen zwischen diesen Instanzen auf, erkennen Sie ausgefallene Instanzen, und vermeiden Sie das Senden von Anforderungen an ausgefallene Instanzen.
* **Weichen Sie, wenn möglich, auf einen anderen Dienst oder Workflow aus**. Wenn z. B. beim Schreiben in die SQL-Datenbank ein Fehler auftritt, speichern Sie die Daten temporär im Blobspeicher. Stellen Sie eine Möglichkeit bereit, die Schreibvorgänge im Blobspeicher mit der SQL-Datenbank zu wiederholen, wenn der Dienst wieder verfügbar ist. In einigen Fällen kann für einen fehlgeschlagenen Vorgang eine alternative Aktion vorhanden sein, die das weitere Funktionieren der Anwendung ermöglicht, selbst wenn eine Komponente oder ein Dienst ausfällt. Wenn möglich, sollten Fehler erkannt und Anforderungen an andere Dienste, die eine geeignete alternative Funktionalität bieten, oder an Ersatzinstanzen oder Instanzen mit reduzierter Funktionalität weitergeleitet werden. So können wichtige Vorgänge aufrechterhalten werden, während der primäre Dienst offline ist.

## <a name="monitoring-and-disaster-recovery"></a>Überwachung und Notfallwiederherstellung
* **Stellen Sie ein umfangreiches Instrumentarium für mögliche Ausfälle und Fehlerereignisse bereit**, um diese Vorkommnisse IT-Mitarbeitern zu melden. Stellen Sie für Fehler, die wahrscheinlich, aber noch nicht aufgetreten sind, ausreichend Daten bereit, um die Mitarbeiter in die Lage zu versetzen, die Ursache zu ermitteln, das Problem zu entschärfen und sicherzustellen, dass das System verfügbar bleibt. Für bereits aufgetretene Fehler sollte die Anwendung eine geeignete Fehlermeldung an den Benutzer zurückgeben und versuchen, die Ausführung trotz eingeschränkter Funktionalität fortzusetzen. In jedem Fall sollte das Überwachungssystem umfassende Details erfassen, damit die Mitarbeiter eine schnelle Wiederherstellung durchführen können und damit Designer und Entwickler ggf. das System ändern können, um ein erneutes Auftreten der Probleme zu vermeiden.
* **Überwachen Sie den Systemzustand, indem Sie Prüffunktionen implementieren.** Eine Beeinträchtigung der Integrität und Leistung einer Anwendung kann über längere Zeit unbemerkt bleiben, bis ein Fehler auftritt. Implementieren Sie Prüfpunkte oder Funktionen, die regelmäßig von außerhalb der Anwendung ausgeführt werden Diese Überprüfungen können ganz einfach sein, z. B. das Messen der Antwortzeit für die Anwendung als Ganzes, für einzelne Teile der Anwendung, für einzelne von der Anwendung verwendete Dienste oder für einzelne Komponenten. Überprüfungsfunktionen können Prozesse ausführen, um sicherzustellen, dass sie gültige Ergebnisse erzeugen, die Latenzzeit messen und die Verfügbarkeit überprüfen sowie Informationen aus dem System extrahieren.
* **Testen Sie regelmäßig alle Failover- und Fallback-Systeme** , um sicherstellen, dass sie verfügbar sind und erwartungsgemäß funktionieren. Änderungen an Systemen und Vorgängen können die Failover- und Fallback-Funktionen beeinträchtigen, die Auswirkungen werden aber möglicherweise erst dann erkannt, wenn das System ausfällt oder überlastet wird. Führen Sie Tests aus, bevor diese Funktionen benötigt werden, um ein echtes Problem zur Laufzeit zu kompensieren.
* **Testen Sie die Überwachungssysteme.** Sowohl automatische Failover- und Fallbacksysteme als auch die manuelle Visualisierung von Systemintegrität und -leistung mithilfe von Dashboards hängen von einer ordnungsgemäß funktionierenden Überwachung und Instrumentation ab. Wenn diese Elemente ausfallen, wichtige Informationen fehlen oder falsche Daten liefern, kann ein Operator möglicherweise nicht erkennen, dass das System instabil oder fehlerhaft ist.
* **Verfolgen Sie den Fortschritt von Workflows mit langer Laufzeit** , und wiederholen Sie Vorgänge im Fehlerfall. Workflows mit langer Laufzeit umfassen häufig mehrere Schritte. Stellen Sie sicher, dass jeder Schritt unabhängig ist und wiederholt werden kann, um das Risiko zu minimieren, dass der gesamte Workflow rückgängig gemacht werden muss oder mehrere ausgleichende Transaktionen ausgeführt werden müssen. Überwachen und verwalten Sie den Status Workflows mit langer Ausführungszeit, indem Sie ein Muster wie das [Scheduler Agent Supervisor Pattern](https://msdn.microsoft.com/library/dn589780.aspx)(Scheduler-Agent-Supervisor-Muster) implementieren.
* **Planen Sie die Wiederherstellung im Notfall.** Erstellen Sie einen akzeptierten und vollständig getesteten Plan für die Wiederherstellung nach einer beliebigen Art von Fehler, der sich auf die Systemverfügbarkeit auswirken kann. Wählen Sie eine Architektur für eine Notfallwiederherstellung mit mehreren Standorten, die für unternehmenskritische Anwendungen bestimmt ist. Identifizieren Sie einen bestimmten Besitzer des Plans für die Notfallwiederherstellung, einschließlich Automation und Testing. Stellen Sie sicher, dass der Plan gut dokumentiert ist, und automatisieren Sie den Prozess so weit wie möglich. Richten Sie eine Sicherungsstrategie für alle Referenz- und Transaktionsdaten ein, und testen Sie die Wiederherstellung dieser Sicherungen regelmäßig. Schulen Sie Betriebspersonal in Bezug auf die Ausführung des Plans, und führen Sie regelmäßig Notfallsimulationen durch, um den Plan zu überprüfen und zu verbessern.