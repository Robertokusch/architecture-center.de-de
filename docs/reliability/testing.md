---
title: Testen von Azure-Anwendungen auf Resilienz und Verfügbarkeit
description: Ansätze zum Testen der Zuverlässigkeit von Anwendungen in Azure
author: MikeWasson
ms.date: 04/10/2019
ms.topic: article
ms.service: architecture-center
ms.subservice: cloud-design-principles
ms.openlocfilehash: cf33a859540810279b1cb85be5a6fb2ebe4500dc
ms.sourcegitcommit: 579c39ff4b776704ead17a006bf24cd4cdc65edd
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/17/2019
ms.locfileid: "59642861"
---
# <a name="testing-azure-applications-for-resiliency-and-availability"></a>Testen von Azure-Anwendungen auf Resilienz und Verfügbarkeit

Um die Resilienz zu testen, sollten Sie überprüfen, wie sich die End-to-End-Workload unter zeitweiligen Fehlerbedingungen verhält.

Führen Sie Tests in der Produktion mithilfe von synthetischen und echten Benutzerdaten aus. Test und Produktion sind selten identisch, daher ist es wichtig, Ihre Anwendung mithilfe einer [Blau/Grün](https://martinfowler.com/bliki/BlueGreenDeployment.html)- oder einer [Canary](https://martinfowler.com/bliki/CanaryRelease.html)-Bereitstellung in der Produktion zu testen. Auf diese Weise testen Sie die Anwendung unter realen Bedingungen und können daher sicher sein, dass sie bei der vollständigen Bereitstellung erwartungsgemäß funktioniert.

Schließen Sie dies in Ihren Testplan ein:

- Automatisierte Tests vor der Bereitstellung
- Fault Injection-Tests
- Lastspitzentests
- Tests der Notfallwiederherstellung
- Tests von Drittanbieterdiensten

Der Testvorgang ist ein iterativer Prozess. Testen Sie die Anwendung, messen Sie das Ergebnis, analysieren und beheben Sie alle aufgetretenen Fehler, und wiederholen Sie den Vorgang.

## <a name="perform-fault-injection-testing"></a>Durchführen von Fault-Injection-Tests

Überprüfen Sie die Resilienz des Systems bei Fehlern mithilfe von Fault Injection, indem Sie entweder echte Fehler auslösen oder diese simulieren. Hier sind einige Strategien für das Hervorrufen von Fehlern:

- Herunterfahren von virtuellen Computerinstanzen
- Absturz von Prozessen
- Ablauf von Zertifikaten
- Ändern von Zugriffsschlüsseln
- Herunterfahren des DNS-Diensts auf Domänencontrollern
- Begrenzen von verfügbaren Systemressourcen, z.B. RAM oder Anzahl von Threads
- Aufheben der Bereitstellung von Datenträgern
- Erneutes Bereitstellen eines virtuellen Computers

Ihr Testplan sollte über allgemeine Fehlerszenarien hinaus mögliche Ausfallpunkte umfassen, die während der Entwurfsphase erkannt wurden:

- Testen Sie Ihre Anwendung in einer Umgebung, die der Produktionsumgebung so ähnlich wie möglich ist
- Testen Sie Fehler in Kombination
- Messen Sie die Wiederherstellungszeiten, und stellen Sie sicher, dass Ihre geschäftlichen Anforderungen erfüllt werden
- Vergewissern Sie sich, dass es nicht zu kaskadierenden Fehlern kommt und dass diese isoliert behandelt werden

Weitere Informationen zu Fehlerszenarien finden Sie unter [Failure and disaster recovery for Azure applications](./disaster-recovery.md) (Wiederherstellung bei einem Fehler oder Notfall für Azure-Anwendungen).

## <a name="test-under-peak-loads"></a>Testen unter Lastspitzen

Auslastungstests sind sehr wichtig zur Identifizierung von Fehlern, die nur bei hoher Auslastung auftreten, z.B. eine Überlastung der Back-End-Datenbank oder eine Drosselung des Diensts. Führen Sie Tests für Lastspitzen und einen vorhersehbaren Anstieg der Spitzenauslastung durch, indem Sie Produktionsdaten oder synthetische Daten verwenden, die den Produktionsdaten möglichst genau ähneln. Hierbei soll ermittelt werden, wie sich die Anwendung unter realen Bedingungen verhält.

## <a name="conduct-disaster-recovery-drills"></a>Durchführen von Übungen zur Notfallwiederherstellung

Gehen Sie von einem guten Plan für die Notfallwiederherstellung aus, und testen Sie ihn regelmäßig, um sicherzustellen, dass er funktioniert.

Für virtuelle Azure-Computer können Sie ohne Beeinträchtigung von Produktionsanwendungen oder laufenden Replikationen [Azure Site Recovery](/azure/site-recovery/azure-to-azure-quickstart/) zum Replizieren sowie zum Ausführen von [Notfallwiederherstellungsübungen](/azure/site-recovery/azure-to-azure-tutorial-dr-drill/) verwenden.

### <a name="failover-and-failback-testing"></a>Failover- und Failbacktests

Testen Sie Failover und Failback, um zu überprüfen, ob die abhängigen Dienste Ihrer Anwendung während der Notfallwiederherstellung in synchronisierter Weise wieder zur Verfügung gestellt werden. Änderungen an Systemen und Vorgängen können die Failover- und Fallback-Funktionen beeinträchtigen, die Auswirkungen werden aber möglicherweise erst dann erkannt, wenn das System ausfällt oder überlastet wird. Testen Sie die Failoverfunktionen *bevor* sie benötigt werden, um ein Liveproblem zu kompensieren. Achten Sie außerdem darauf, dass Failover und Failback von abhängigen Diensten in der richtigen Reihenfolge erfolgen.

Wenn Sie virtuelle Computer mit [Azure Site Recovery](/azure/site-recovery/) replizieren, führen Sie immer wieder Wiederherstellungsübungen in Form eines Testfailovers durch, um Ihre Replikationsstrategie zu überprüfen. Ein Testfailover hat keinerlei Auswirkungen auf die laufende VM-Replikation oder Ihre Produktionsumgebung. Weitere Informationen finden Sie unter [Durchführen eines Notfallwiederherstellungsverfahrens in Azure](/azure/site-recovery/site-recovery-test-failover-to-azure).

### <a name="simulation-testing"></a>Simulationstests

Bei Simulationstests werden kleine, aus der Realität entnommene Situationen erstellt. Simulationen führen die Wirksamkeit der Lösungen im Wiederherstellungsplan vor Augen und lassen alle Probleme hervortreten, die nicht angemessen berücksichtigt wurden.

Folgen Sie beim Durchführen von Simulationstests bewährten Methoden:

- Führen Sie Simulationen in einer Weise durch, die den tatsächlichen Geschäftsbetrieb nicht stört, sich aber wie eine echte Situation anfühlt.
- Achten Sie darauf, dass sie simulierten Szenarien vollständig kontrollierbar sind. Selbst wenn der Wiederherstellungsplan fehlzuschlagen scheint, können Sie den Normalzustand wiederherstellen, ohne dass Schäden entstehen.
- Informieren Sie das Management über Art und Zeitpunkt der durchzuführenden Simulationsübungen. Ihr Plan muss den Zeitrahmen und die Ressourcen, die während der Simulation betroffen sind, detailliert beschreiben.

## <a name="test-health-probes-for-load-balancers-and-traffic-managers"></a>Testen von Integritätstest für Load Balancer und Traffic Manager

Konfigurieren und testen Sie Integritätstest für Ihre Lastenausgleichsmodule und Traffic Manager. Stellen Sie sicher, dass Ihr Integritätsendpunkt die kritischen Teile des Systems überprüft und in geeigneter Weise reagiert.

- Bei [Azure Traffic Manager](/azure/traffic-manager/traffic-manager-overview/) bestimmt der Integritätstest, ob ein Failover auf eine andere Region erforderlich ist. Ihr Integritätsendpunkt sollten alle kritischen Abhängigkeiten überprüfen, die innerhalb der gleichen Region bereitgestellt sind.
- Bei [Azure Load Balancer](/azure/load-balancer/load-balancer-overview/) bestimmt der Integritätstest, ob eine VM aus der Rotation entfernt werden soll. Der Integritätsendpunkt sollte die Integrität der VM melden. Nehmen Sie keine weiteren Schichten oder externen Dienste auf. Andernfalls verursacht ein Fehler, der außerhalb der VM auftritt, dass der Lastenausgleich die VM aus der Rotation entfernt.

Eine Anleitung für die Implementierung der Integritätsüberwachung in Ihrer Anwendung finden Sie unter [Muster für Überwachung der Integrität von Endpunkten](../patterns/health-endpoint-monitoring.md).

## <a name="test-monitoring-systems"></a>Testen der Überwachungssysteme

Nehmen Sie Überwachungssysteme in Ihren Testplan auf. Automatisierte Failover- und Failbacksysteme hängen von der korrekten Funktion von Überwachung und Instrumentierung ab. Dashboards zur Visualisierung von System- und Operatorwarnungen zur Integrität sind ebenfalls auf genaue Überwachung und Instrumentierung angewiesen. Wenn diese Elemente ausfallen, wichtige Informationen fehlen oder falsche Daten liefern, kann ein Operator möglicherweise nicht erkennen, dass das System instabil oder fehlerhaft ist.

## <a name="include-third-party-services-in-test-scenarios"></a>Einbeziehen von Drittanbieterdiensten in Testszenarien

Wenn Ihre Anwendung Abhängigkeiten von den Diensten von Drittanbietern aufweist, identifizieren Sie, wo und wie Fehler in diesen Diensten auftreten können und welche Auswirkungen diese Fehler auf die Anwendung haben. Bedenken Sie das SLA (Service-Level Agreement) für den Drittanbieterdienst und seine möglichen Auswirkungen auf Ihren Plan zur Notfallwiederherstellung.

Der Dienst eines Drittanbieters stellt möglicherweise keine Überwachungs- und Diagnosefunktionen zur Verfügung. Daher ist es wichtig, dass Ihre Aufrufe dieser Dienste protokolliert werden und über einen eindeutigen Bezeichner mit der Integritäts- und Diagnoseprotokollierung Ihrer Anwendung korreliert werden. Weitere Informationen zu bewährten Verfahren für Überwachung und Diagnose finden Sie unter [Anleitung zur Überwachung und Diagnose](../best-practices/monitoring.md).

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Bereitstellen für hohe Resilienz und Verfügbarkeit](./deploy.md)
