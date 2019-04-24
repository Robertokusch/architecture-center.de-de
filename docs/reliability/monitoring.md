---
title: Überwachen der Anwendungsintegrität für Zuverlässigkeit in Azure
description: Verwenden der Überwachung zur Verbesserung der Zuverlässigkeit von Anwendungen in Azure
author: MikeWasson
ms.date: 04/10/2019
ms.topic: article
ms.service: architecture-center
ms.subservice: cloud-design-principles
ms.openlocfilehash: 46f9f3fb623a2facf4b9f9c6ec57376ae59e41dc
ms.sourcegitcommit: 579c39ff4b776704ead17a006bf24cd4cdc65edd
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/17/2019
ms.locfileid: "59642867"
---
# <a name="monitoring-application-health-for-reliability-in-azure"></a>Überwachen der Anwendungsintegrität für Zuverlässigkeit in Azure

Die Überwachung und die Diagnose sind für die Resilienz von entscheidender Bedeutung. Wenn ein Fehler auftritt, müssen Sie wissen, *dass*, *wann* und *warum* etwas fehlgeschlagen ist.

*Überwachung* ist nicht dasselbe wie *Fehlererkennung*. Es kann beispielsweise sein, dass Ihre Anwendung einen vorübergehenden Fehler erkennt und einen Wiederholungsversuch durchführt, ohne dass es zu Ausfallzeit kommt. Es sollte aber auch der Wiederholungsvorgang protokolliert werden, damit Sie die Fehlerrate überwachen können, um ein Gesamtbild der Anwendungsintegrität zu erhalten.

Sie können sich den Überwachungs- und Diagnoseprozess als Pipeline mit vier einzelnen Phasen vorstellen: Instrumentierung, Sammlung und Speicherung, Analyse und Diagnose sowie Visualisierung und Warnungen.

## <a name="instrumentation"></a>Instrumentierung

Es ist nicht praktikabel, die Anwendung direkt zu überwachen, daher ist die Instrumentierung der Schlüssel. Ein großes verteiltes System kann auf Dutzenden von virtuellen Computern (VMs) ausgeführt werden, die im Laufe der Zeit hinzugefügt und entfernt werden. Auch kann eine Cloudanwendung eine Reihe von Datenspeichern verwenden, und eine einzelne Benutzeraktion kann mehrere Subsysteme umfassen.

Stellen Sie die umfangreiche Instrumentation bereit:

- Stellen Sie für Fehler, die wahrscheinlich, aber noch nicht aufgetreten sind, ausreichend Daten bereit, um die Ursache zu ermitteln, das Problem zu entschärfen und sicherzustellen, dass das System verfügbar bleibt.
- Für bereits aufgetretene Fehler sollte die Anwendung eine geeignete Fehlermeldung an den Benutzer zurückgeben und versuchen, die Ausführung trotz eingeschränkter Funktionalität fortzusetzen.

Überwachungssysteme sollten umfassende Details erfassen, damit Anwendungen effizient wiederhergestellt werden können und Designer und Entwickler das System bei Bedarf so anpassen können, dass sich die Situation nicht wiederholt.

Die Rohdaten für die Überwachung können aus einer Vielzahl von Quellen stammen, einschließlich:

- Anwendungsprotokolle, wie die vom [Azure Application Insights](/azure/azure-monitor/app/app-insights-overview)-Dienst generierten Protokolle.
- Betriebssystemleistungsmetriken, die von [Azure-Überwachungs-Agents](/azure/azure-monitor/platform/agents-overview) erfasst werden.
- [Azure-Ressourcen](/azure/azure-monitor/platform/metrics-supported), z.B. von Azure Monitor erfasste Metriken.
- [Azure Service Health](/azure/service-health/service-health-overview), das ein Dashboard bereitstellt, mit dem Sie aktive Ereignisse nachverfolgen können.
- In die Azure-Plattform integrierte [Azure AD-Protokolle](/azure/active-directory/reports-monitoring/howto-integrate-activity-logs-with-log-analytics) .

Die meisten Azure-Dienste verfügen über Metriken und Diagnosen, die Sie konfigurieren können, um die Ursache von Problemen zu analysieren und zu ermitteln. Weitere Informationen finden Sie unter [Von Azure Monitor gesammelte Überwachungsdaten](/azure/azure-monitor/platform/data-collection).

## <a name="collection-and-storage"></a>Sammlung und Speicherung

Rohdaten der Instrumentierung können an verschiedenen Orten und in unterschiedlichen Formaten gespeichert werden, z.B.:

- Anwendungsnachverfolgungsprotokolle
- IIS-Protokolle
- Leistungsindikatoren

Diese unterschiedlichen Quellen werden gesammelt, zusammengefasst und in zuverlässige Datenspeicher in Azure eingefügt, z.B. Application Insights, Azure Monitor-Metriken, Service Health, Speicherkonten und Azure Log Analytics.

## <a name="analysis-and-diagnosis"></a>Analyse und Diagnose

Analysieren Sie die in diesen Datenspeichern zusammengefassten Daten, um Probleme zu beheben und eine Gesamtansicht der Anwendungsintegrität bereitzustellen. Im Allgemeinen können Sie die Daten in Application Insights and Log Analytics mit Hilfe von Kusto-Abfragen [suchen und analysieren](/azure/azure-monitor/log-query/log-query-overview) oder vorkonfigurierte Diagramme mithilfe von  [Verwaltungslösungen](/azure/azure-monitor/insights/solutions-inventory) anzeigen. Alternativ können Sie Azure Advisor verwenden, um Empfehlungen anzuzeigen, bei denen der Schwerpunkt auf [Resilienz](/azure/advisor/advisor-high-availability-recommendations) und [Leistung](/azure/advisor/advisor-performance-recommendations) liegt.

## <a name="visualization-and-alerts"></a>Visualisierung und Warnungen

Präsentieren Sie Telemetriedaten in einem Format, anhand dessen der Operator Probleme oder Trends einfach und schnell erkennen kann, z.B. in Form eines Dashboards oder einer E-Mail-Benachrichtigung.

Rufen Sie mithilfe von  [Azure-Dashboards](/azure/azure-portal/azure-portal-dashboards) eine vollständige Stapelansicht des Anwendungszustands ab, um eine konsolidierte Ansicht der Überwachungsdiagramme aus Application Insights, Log Analytics, Azure Monitor-Metriken und Service Health zu erstellen. Und verwenden Sie  [Azure Monitor-Warnungen](/azure/azure-monitor/platform/alerts-overview), um Benachrichtigungen über Service Health, Ressourcenintegrität, Azure Monitor-Metriken, Protokolle in Log Analytics und Application Insights zu erstellen.

Weitere Informationen zu Überwachung und Diagnose finden Sie unter [Überwachung und Diagnose](../best-practices/monitoring.md).

## <a name="health-probes-and-check-functions"></a>Integritätstests und Überprüfungsfunktionen

Eine Beeinträchtigung der Integrität und Leistung einer Anwendung kann über längere Zeit unbemerkt bleiben, bis ein Fehler in der Anwendung auftritt.

Implementieren Sie Prüfpunkte oder Überprüfungsfunktionen, und führen Sie diese regelmäßig von außerhalb der Anwendung aus. Diese Überprüfungen können ganz einfach sein, z.B. das Messen der Antwortzeit für die Anwendung als Ganzes, für einzelne Teile der Anwendung, für bestimmte, von der Anwendung verwendete Dienste oder für einzelne Komponenten.

Überprüfungsfunktionen können Prozesse ausführen, um sicherzustellen, dass sie gültige Ergebnisse erzeugen, die Latenzzeit messen und die Verfügbarkeit überprüfen sowie Informationen aus dem System extrahieren.

## <a name="long-running-workflow-failures"></a>Fehler in zeitintensiven Workflows

Zeitintensive Workflows umfassen häufig mehrere Schritte, die jeweils unabhängig voneinander sein sollten.

Verfolgen Sie den Fortschritt zeitintensiver Prozesse, und um das Risiko zu minimieren, dass der gesamte Workflow rückgängig gemacht werden muss oder mehrere ausgleichende Transaktionen ausgeführt werden müssen.

>[!TIP]
> Überwachen und verwalten Sie den Status von zeitintensiven Workflows, indem Sie ein Muster wie das [Scheduler-Agent-Supervisor](../patterns/scheduler-agent-supervisor.md)-Muster implementieren.

## <a name="application-logs"></a>Anwendungsprotokolle

Anwendungsprotokolle sind eine wichtige Quelle für Diagnosedaten. Um einen Einblick zu erhalten, wenn es am nötigsten ist, befolgen Sie die bewährten Methoden für die Anwendungsprotokollierung.

### <a name="log-data-in-the-production-environment"></a>Protokollieren von Daten in der Produktionsumgebung

Erfassen Sie zuverlässige Telemetriedaten, während die Anwendung in der Produktionsumgebung ausgeführt wird, damit Sie über ausreichende Informationen verfügen, um die Ursache von Problemen im Produktionsstatus zu diagnostizieren.

### <a name="log-events-at-service-boundaries"></a>Protokollieren von Ereignissen an Dienstgrenzen

Binden Sie eine Korrelations-ID ein, die über Dienstgrenzen hinweg gilt. Wenn eine Transaktion mehrere Dienste durchläuft und einer davon ausfällt, können Sie anhand der Korrelations-ID Anforderungen über Ihre Anwendung hinweg verfolgen und ermitteln, warum es zum Transaktionsfehler gekommen ist.

### <a name="use-semantic-structured-logging"></a>Verwenden der semantischen (strukturierten) Protokollierung

Strukturierte Protokolle erleichtern die Automatisierung des Verbrauchs und der Analyse von Protokolldaten, was besonders wichtig auf Cloudebene ist. Im Allgemeinen wird empfohlen, Azure-Ressourcenmetriken und Diagnosedaten in einem Log Analytics-Arbeitsbereich und nicht in einem Speicherkonto zu speichern. Auf diese Weise können Sie die gewünschten Daten mit Kusto-Abfragen schnell und in einem strukturierten Format abrufen. Sie können auch Azure Monitor-APIs und Azure Log Analytics-APIs verwenden.

### <a name="use-asynchronous-logging"></a>Verwenden der asynchronen Protokollierung

Synchrone Protokollierungsvorgänge blockieren manchmal Ihren Anwendungscode und führen zu Sicherungsanforderungen beim Schreiben von Protokollen. Verwenden Sie die asynchrone Protokollierung, um die Verfügbarkeit während der Anwendungsprotokollierung aufrecht zu erhalten.

### <a name="separate-application-logging-from-auditing"></a>Trennen der Anwendungsprotokollierung von der Überwachung

Überwachungsdatensätze werden üblicherweise aufgrund von Compliance- oder regulatorischen Anforderungen gepflegt und müssen vollständig sein. Um abgebrochene Transaktionen zu vermeiden, verwalten Sie Überwachungsprotokolle getrennt von Diagnoseprotokollen.

## <a name="remote-call-statistics"></a>Statistiken zu Remoteaufrufen

Sie sollten Statistiken zu Remoteaufrufen in Echtzeit nachverfolgen und melden und eine einfache Möglichkeit bieten, diese Informationen zu überprüfen, damit das Betriebsteam über unmittelbare Einblicke in die Integrität Ihrer Anwendung verfügt. Fassen Sie Metriken zu Remoteaufrufen wie Latenz, Durchsatz und Fehler in den 99 und 95 Quantilen zusammen.

## <a name="transient-exceptions-and-retries"></a>Vorübergehende Ausnahmen und Wiederholungen

Verfolgen Sie die Anzahl der vorübergehenden Ausnahmen und Wiederholungen im Zeitverlauf, um Probleme oder Ausfälle in der Wiederholungslogik Ihrer Anwendung aufzudecken. Ein Trend zunehmender Ausnahmen im Zeitverlauf weist darauf hin, dass der Dienst möglicherweise ein Problem hat und ausfallen kann. Weitere Informationen finden Sie unter [Anleitung zu dienstspezifischen Wiederholungsmechanismen](../best-practices/retry-service-specific.md).

## <a name="early-warning-system"></a>Frühwarnsystem

Überwachen Sie Ihre Anwendung auf Warnsignale, die möglicherweise proaktive Eingriffe nötig machen. Tools, die die allgemeine Integrität der Anwendung und ihrer Abhängigkeiten bewerten, helfen Ihnen, schnell zu erkennen, wenn ein System oder seine Komponenten plötzlich nicht mehr verfügbar sind. Verwenden Sie diese Tools, um ein Frühwarnsystem zu implementieren.

1. Identifizieren Sie Key Performance Indicators für die Integrität Ihrer Anwendung, z.B. vorübergehende Ausnahmen und Latenz von Remoteaufrufen.
1. Legen Sie Schwellenwerte auf Ebenen fest, auf denen Probleme identifiziert werden, bevor sie kritisch werden und eine Wiederherstellung erfordern.
1. Senden Sie eine Warnung an Operatoren, wenn der Schwellenwert erreicht wird.

Ziehen Sie die Verwendung von [Microsoft System Center 2016](https://www.microsoft.com/cloud-platform/system-center) oder Drittanbietertools in Betracht, um Überwachungsfunktionen bereitzustellen. Die meisten Überwachungslösungen verfolgen wichtige Leistungsindikatoren und die Verfügbarkeit von Diensten. [Azure Resource Health](/azure/service-health/resource-health-checks-resource-types) stellt einige integrierte Integritätsstatusüberprüfungen bereit, die dabei helfen können, die Drosselung von Azure-Diensten zu diagnostizieren.

## <a name="subscription-and-service-limitations"></a>Abonnements und Diensteinschränkungen

Bei Azure-Abonnements gelten Grenzwerte für bestimmte Ressourcentypen. Hierzu zählt beispielsweise die Anzahl von Ressourcengruppen, Kernen und Speicherkonten. Um sicherzustellen, dass Ihre Anwendung die Einschränkungen für Azure-Abonnements nicht überschreitet, erstellen Sie Warnungen, die für Dienste ausgelöst werden, deren Grenzen und Kontingente demnächst erreicht werden.

Erstellen Sie dabei Warnungen für die folgenden Abonnementgrenzen.

### <a name="individual-services"></a>Einzelne Dienste

Für einzelne Azure-Dienste gelten Nutzungsgrenzwerte für Speicher, Durchsatz, die Anzahl der Verbindungen, Anforderungen pro Sekunde und weitere Metriken. Ihre Anwendung schlägt fehl, wenn sie versucht, Ressourcen außerhalb dieser Grenzen zu nutzen, was zu Dienstdrosselung und möglichen Ausfallzeiten führt.

Abhängig vom spezifischen Dienst und den Anforderungen Ihrer Anwendung können Sie häufig durch zentrales Hochskalieren (z.B. die Auswahl eines anderen Tarifs) oder horizontales Skalieren (Hinzufügen neuer Instanzen) unterhalb dieser Grenzwerte bleiben.

### <a name="azure-storage-scalability-and-performance-targets"></a>Skalierbarkeits- und Leistungsziele für Azure Storage

Azure erlaubt eine maximale Anzahl von Speicherkonten pro Abonnement. Wenn Ihre Anwendung mehr Speicherkonten erfordert, als derzeit in Ihrem Abonnement verfügbar sind, müssen Sie ein neues Abonnement mit zusätzlichen Speicherkonten erstellen. Weitere Informationen finden Sie unter [Grenzwerte für Azure-Abonnements, -Dienste und -Kontingente sowie allgemeine Beschränkungen](/azure/azure-subscription-service-limits/#storage-limits).

Wenn Sie diese Skalierbarkeits- und Leistungsziele für Azure Storage überschreiten, wird für Ihre Anwendung der Speicher eingeschränkt. Weitere Informationen finden Sie unter [Skalierbarkeits- und Leistungsziele für Azure Storage](/azure/storage/storage-scalability-targets/).

### <a name="scalability-targets-for-virtual-machine-disks"></a>Skalierbarkeitsziele für Festplatten virtueller Computer

Eine Azure-IaaS-VM (Infrastructure-as-a-Service) unterstützt das Anfügen von mehreren Datenträgern. Dies ist abhängig von verschiedenen Faktoren wie der VM-Größe und des Speicherkontotyps. Wenn Ihre Anwendung die Skalierbarkeitsziele für VM-Datenträger überschreitet, stellen Sie zusätzliche Speicherkonten für die VM-Datenträger bereit. Weitere Informationen finden Sie unter [Skalierbarkeits- und Leistungsziele für Azure Storage](/azure/storage/storage-scalability-targets/#scalability-targets-for-virtual-machine-disks).

### <a name="vm-size"></a>Größe des virtuellen Computers

Wenn die tatsächlichen Werte von CPU, Arbeitsspeicher, Datenträger und E/A Ihrer VMs sich den Grenzen der VM-Größe nähern, kann es in Ihrer Anwendung zu Kapazitätsproblemen kommen. Um die Probleme zu beheben, erhöhen Sie die VM-Größe. VM-Größen werden unter [Größen für virtuelle Computer in Azure](/azure/virtual-machines/virtual-machines-windows-sizes/?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) beschrieben.

Wenn Ihre Workload im Zeitverlauf schwankt, sollten Sie die Verwendung von VM-Skalierungsgruppen in Betracht ziehen, um die Anzahl der VM-Instanzen automatisch zu skalieren. Andernfalls müssen Sie die VM-Anzahl manuell erhöhen oder verringern. Weitere Informationen finden Sie unter [Übersicht über VM-Skalierungsgruppen](/azure/virtual-machine-scale-sets/virtual-machine-scale-sets-overview/).

### <a name="azure-sql-database"></a>Azure SQL-Datenbank

Wenn Ihre Azure SQL-Datenbankebene die Anforderungen Ihrer Anwendung an die Datenbanktransaktionseinheiten (DTU) nicht verarbeiten kann, wird die Datenverwendung eingeschränkt. Weitere Informationen zur Auswahl des korrekten Dienstplans finden Sie unter [Azure SQL-Datenbankkaufmodelle](/azure/sql-database/sql-database-service-tiers/).

## <a name="third-party-services"></a>Dienste von Drittanbietern

Wenn Ihre Anwendung Abhängigkeiten von den Diensten von Drittanbietern aufweist, identifizieren Sie, wie Fehler in diesen Diensten auftreten können und welche Auswirkungen diese Fehler auf die Anwendung haben.

Der Dienst eines Drittanbieters weist möglicherweise keine Überwachungs- und Diagnosefunktionen auf. Protokollieren Sie Aufrufe an diese Dienste, und korrelieren Sie sie unter Verwendung eines eindeutigen Bezeichners mit der Integrität und der Diagnoseprotokollierung Ihrer Anwendung. Weitere Informationen zu bewährten Verfahren für Überwachung und Diagnose finden Sie unter [Anleitung zur Überwachung und Diagnose](../best-practices/monitoring.md).

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Planen der Wiederherstellung im Notfall](./disaster-recovery.md)
