---
title: Batchbewertung von Python-Modellen in Azure
description: Erstellen Sie mit Azure Machine Learning Service eine skalierbare Lösung für die parallele Batchbewertung von Modellen nach einem Zeitplan.
author: njray
ms.date: 01/30/2019
ms.topic: reference-architecture
ms.service: architecture-center
ms.subservice: reference-architecture
ms.custom: azcat-ai, AI
ms.openlocfilehash: 9341b9e4c17025e9623902a6202076c352b237b9
ms.sourcegitcommit: 579c39ff4b776704ead17a006bf24cd4cdc65edd
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/17/2019
ms.locfileid: "59640547"
---
# <a name="batch-scoring-of-python-machine-learning-models-on-azure"></a>Batchbewertung von Python-Modellen für Machine Learning in Azure

Diese Referenzarchitektur veranschaulicht, wie Sie mit Azure Machine Learning Service eine skalierbare Lösung für die parallele Batchbewertung vieler Modelle nach einem Zeitplan erstellen. Die Lösung kann als Vorlage genutzt und für unterschiedliche Probleme generalisiert werden.

Eine Referenzimplementierung für diese Architektur ist auf [GitHub][github] verfügbar.

![Batchbewertung von Python-Modellen in Azure](./_images/batch-scoring-python.png)

**Szenario:** Diese Lösung überwacht den Betrieb einer großen Zahl von Geräten unter einer IoT-Einstellung, wobei jedes Gerät fortlaufend Sensormesswerte sendet. Es wird vorausgesetzt, dass jedem Gerät vorab trainierte Modelle für die Anomalieerkennung zugeordnet sind, um Folgendes vorherzusagen: Entspricht eine Reihe von Messwerten, die für ein vordefiniertes Zeitintervall aggregiert werden, einer Anomalie? In der Praxis kann dies ein Datenstrom mit Sensormesswerten sein, die gefiltert und aggregiert werden müssen, bevor sie für das Training oder für Echtzeitbewertungen verwendet werden. Der Einfachheit halber wird für diese Lösung beim Ausführen von Bewertungsaufträgen dieselbe Datendatei verwendet.

Diese Referenzarchitektur ist für Workloads ausgelegt, die anhand eines Zeitplans ausgelöst werden. Die Verarbeitung umfasst die folgenden Schritte:

1. Senden von Sensormesswerten für die Erfassung in Azure Event Hubs
2. Durchführen der Datenstromverarbeitung und Speichern der Rohdaten
3. Senden der Daten an einen Machine Learning-Cluster, der für die Durchführung von Aufgaben bereit ist. Auf jedem Knoten im Cluster wird ein Bewertungsauftrag für einen bestimmten Sensor ausgeführt. 
4. Ausführen der Bewertungspipeline, über die die Bewertungsaufträge mit Machine Learning-Python-Skripts parallel ausgeführt werden. Die Pipeline wird erstellt und veröffentlicht, und es wird ein Zeitplan für die Ausführung nach einem vordefinierten Zeitintervall erstellt.
5. Generieren von Vorhersagen und Speichern im Blobspeicher zur späteren Verwendung

## <a name="architecture"></a>Architecture

Diese Architektur umfasst die folgenden Komponenten:

[Azure Event Hubs][event-hubs]: Mit diesem Dienst für die Nachrichtenerfassung können Millionen von Ereignismeldungen pro Sekunde erfasst werden. In dieser Architektur senden Sensoren einen Datenstrom an den Event Hub.

[Azure Stream Analytics][stream-analytics]: Ein Modul für die Ereignisverarbeitung. Ein Stream Analytics-Auftrag liest die Datenströme aus dem Event Hub und führt die Datenstromverarbeitung durch.

[Azure SQL-Datenbank][sql-database]: Daten der Sensormesswerte werden in SQL-Datenbank geladen. SQL ist eine vertraute Möglichkeit, die verarbeiteten gestreamten Daten (tabellarisch und strukturiert) zu speichern, aber es können auch andere Datenspeicher verwendet werden.

[Azure Machine Learning Service][amls]: Machine Learning ist ein Clouddienst für das bedarfsorientierte Trainieren, Bewerten, Bereitstellen und Verwalten von Machine Learning-Modellen. Im Kontext der Batchbewertung wird bei Machine Learning bedarfsabhängig ein Cluster mit virtuellen Computern erstellt, der über eine Option für die automatische Skalierung verfügt, und jeder Knoten führt einen Bewertungsauftrag für einen bestimmten Sensor aus. Die Bewertungsaufträge werden parallel als Python-Skriptschritte ausgeführt, die von Machine Learning in eine Warteschlange eingereiht und verwaltet werden. Diese Schritte sind Teil einer Machine Learning-Pipeline, die erstellt und veröffentlicht wird und für die die Ausführung nach einem vordefinierten Zeitintervall geplant wird.

[Azure Blob Storage][storage]: Blobcontainer werden zum Speichern der vorab trainierten Modelle, der Daten und der Ausgabevorhersagen verwendet. Die Modelle werden im Notebook [01_create_resources.ipynb][create-resources] in den Blobspeicher hochgeladen. Diese Modelle vom Typ [Einklassige SVM][one-class-svm] werden mit Daten trainiert, die Werte unterschiedlicher Sensoren für unterschiedliche Geräte repräsentieren. Bei dieser Lösung wird davon ausgegangen, dass die Datenwerte während eines festen Zeitintervalls aggregiert werden.

[Azure Container Registry][acr]: Das [Python-Skript][pyscript] für die Bewertung wird in Docker-Containern ausgeführt, die auf jedem Knoten des Clusters erstellt werden. Hiermit werden die relevanten Sensordaten gelesen und Vorhersagen generiert und im Blobspeicher gespeichert.

## <a name="performance-considerations"></a>Überlegungen zur Leistung

Für Python-Standardmodelle sind CPUs, die zum Bewältigen der Workload ausreichen, in der Regel akzeptabel. In dieser Architektur werden CPUs verwendet. Bei [Deep Learning-Workloads][deep] bieten GPUs aber eine deutlich bessere Leistung als CPUs. In der Regel wird ein großer Cluster mit CPUs benötigt, um eine vergleichbare Leistung zu erzielen.

### <a name="parallelizing-across-vms-versus-cores"></a>Vergleich des Parallelisierens auf VMs und Kernen

Beim Ausführen von Bewertungsprozessen für viele Modelle im Batchmodus müssen die Aufträge VM-übergreifend parallelisiert werden. Zwei Ansätze sind möglich:

* Erstellen eines größeren Clusters mit kostengünstigen VMs

* Erstellen eines kleineren Clusters mit Hochleistungs-VMs, die jeweils über mehr Kerne verfügen

Im Allgemeinen ist das Bewerten von Python-Standardmodellen nicht so anspruchsvoll wie das Bewerten von Deep Learning-Modellen. Mit einem kleinen Cluster sollte es möglich sein, eine große Zahl von in der Warteschlange befindlichen Modellen effizient zu verarbeiten. Sie können die Anzahl von Clusterknoten erhöhen, wenn die Größe der Datasets zunimmt.

Der Einfachheit halber wird in diesem Szenario für einen Machine Learning-Pipelineschritt nur eine Bewertungsaufgabe übermittelt. Es kann aber auch effizienter sein, mehrere Datenblöcke innerhalb desselben Pipelineschritts zu bewerten. Schreiben Sie in diesen Fällen benutzerdefinierten Code, mit dem mehrere Datasets eingelesen werden und das entsprechende Bewertungsskript während eines einzelnen Schritts ausgeführt wird.

## <a name="management-considerations"></a>Überlegungen zur Verwaltung

- **Überwachen von Aufträgen**: Es ist wichtig, den Status von ausgeführten Aufträgen zu überwachen, aber es kann eine ziemliche Herausforderung darstellen, einen gesamten Cluster mit aktiven Knoten zu überwachen. Verwenden Sie für die Untersuchung des Zustands der Knoten im Cluster das [Azure-Portal][portal], um den [Machine Learning-Arbeitsbereich][ml-workspace] zu verwalten. Wenn ein Knoten inaktiv oder ein Auftrag fehlgeschlagen ist, werden die Fehlerprotokolle im Blobspeicher gespeichert und sind auch über den Abschnitt „Pipelines“ zugänglich. Verbinden Sie zum Durchführen einer umfassenderen Überwachung Protokolle mit [Application Insights][app-insights], oder führen Sie separate Prozesse aus, um den Status des Clusters und der zugehörigen Aufträge abzufragen.
- **Protokollierung**: Machine Learning Service protokolliert alle stdout/stderr-Vorgänge für das zugeordnete Azure Storage-Konto. Verwenden Sie ein Speichernavigationstool, z.B. [Azure Storage-Explorer][explorer], um die Protokolldateien leicht anzeigen zu können.

## <a name="cost-considerations"></a>Kostenbetrachtung

Die teuersten Komponenten, die in dieser Referenzarchitektur genutzt werden, sind die Computeressourcen. Die Größe des Computeclusters wird je nach den Aufträgen in der Warteschlange zentral hoch- und herunterskaliert. Aktivieren Sie die automatische Skalierung programmgesteuert über das Python SDK, indem Sie die Bereitstellungskonfiguration der Computeumgebung ändern. Sie können auch die [Azure CLI][cli] verwenden, um die Parameter für die automatische Skalierung des Clusters festzulegen.

Konfigurieren Sie für Aufträge, die nicht direkt verarbeitet werden müssen, die Formel für die automatische Skalierung so, dass der Standardzustand (Minimum) ein Cluster von null Knoten ist. Bei dieser Konfiguration hat der Cluster anfangs null Knoten. Er skaliert nur dann zentral hoch, wenn er Aufträge in der Warteschlange erkennt. Wenn die Batchbewertung maximal einige Male am Tag ausgeführt wird, können Sie mithilfe dieser Einstellung eine erhebliche Kosteneinsparung erzielen.

Die automatische Skalierung ist ggf. nicht für Batchaufträge geeignet, die zeitlich zu nahe beieinander liegen. Für die benötigte Zeit zum Erstellen und Entfernen eines Clusters fallen ebenfalls Kosten an. Wenn eine Batchworkload also nur wenige Minuten nach dem Ende des vorherigen Auftrags startet, ist es ggf. kostengünstiger, den Cluster permanent auszuführen – also auch in der Zeit zwischen den Aufträgen. Dies hängt davon ab, ob die Ausführung von Bewertungsprozessen mit hoher Häufigkeit (z.B. jede Stunde) oder geringerer Häufigkeit (z.B. einmal pro Monat) geplant ist.

## <a name="deployment"></a>Bereitstellung

Befolgen Sie die Schritte im Abschnitt [GitHub-Repository][github], um diese Referenzarchitektur bereitzustellen.

[acr]: /azure/container-registry/container-registry-intro
[ai]: /azure/application-insights/app-insights-overview
[aml-compute]: /azure/machine-learning/service/how-to-set-up-training-targets#amlcompute
[amls]: /azure/machine-learning/service/overview-what-is-azure-ml
[automatic-scaling]: /azure/batch/batch-automatic-scaling
[azure-files]: /azure/storage/files/storage-files-introduction
[cli]: /cli/azure
[create-resources]: https://github.com/Microsoft/AMLBatchScoringPipeline/blob/master/01_create_resources.ipynb
[deep]: /azure/architecture/reference-architectures/ai/batch-scoring-deep-learning
[event-hubs]: /azure/event-hubs/event-hubs-geo-dr
[explorer]: https://azure.microsoft.com/en-us/features/storage-explorer/
[github]: https://github.com/Microsoft/AMLBatchScoringPipeline
[one-class-svm]: http://scikit-learn.org/stable/modules/generated/sklearn.svm.OneClassSVM.html
[portal]: https://portal.azure.com
[ml-workspace]: /azure/machine-learning/studio/create-workspace
[python-script]: https://github.com/Azure/BatchAIAnomalyDetection/blob/master/batchai/predict.py
[pyscript]: https://github.com/Microsoft/AMLBatchScoringPipeline/blob/master/scripts/predict.py
[storage]: /azure/storage/blobs/storage-blobs-overview
[stream-analytics]: /azure/stream-analytics/
[sql-database]: /azure/sql-database/
[app-insights]: /azure/application-insights/app-insights-overview
