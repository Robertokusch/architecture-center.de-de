---
title: Batchbewertung mit R-Modellen in Azure
description: Führen Sie die Batchbewertung mit R-Modellen mit Azure Batch und einem Dataset durch, das auf Einzelhandelsverkaufs-Prognosen basiert.
author: njray
ms.date: 03/29/2019
ms.topic: reference-architecture
ms.service: architecture-center
ms.subservice: reference-architecture
ms.custom: azcat-ai
ms.openlocfilehash: 72769cf078596f0312a1f4293205dda5a086ef41
ms.sourcegitcommit: 579c39ff4b776704ead17a006bf24cd4cdc65edd
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/17/2019
ms.locfileid: "59639902"
---
# <a name="batch-scoring-of-r-machine-learning-models-on-azure"></a>Batchbewertung von R-Machine Learning-Modellen in Azure

Diese Referenzarchitektur zeigt, wie Sie die Batchbewertung mit R-Modellen mit Azure Batch ausführen. Das Szenario basiert auf Einzelhandelsverkaufs-Prognosen, aber diese Architektur kann für jedes Szenario verallgemeinert werden, das die Generierung von Vorhersagen auf einem großen Scaler mithilfe von R-Modellen erfordert. Eine Referenzimplementierung für diese Architektur ist auf [GitHub][github] verfügbar.

![Architekturdiagramm][0]

**Szenario:** Eine Supermarktkette muss Produktverkäufe für das bevorstehende Quartal prognostizieren. Die Vorhersage ermöglicht dem Unternehmen, seine Lieferkette besser zu verwalten und sicherzustellen, die Nachfrage nach Produkten in allen Filialen zu erfüllen. Das Unternehmen aktualisiert seine Prognosen jede Woche, wenn neue Verkaufsdaten aus der vorherigen Woche verfügbar sind und die Produktmarketingstrategie für das nächste Quartal festgelegt ist. Quantilvorhersagen werden generiert, um die Ungewissheit bei einzelnen Umsatzprognosen einzuschätzen.

Die Verarbeitung umfasst die folgenden Schritte:

1. Eine Azure-Logik-App löst einmal pro Woche die Vorhersagengenerierung aus.

1. Die Logik-App startet eine Azure-Containerinstanz, auf der der Planer-Docker-Container ausgeführt wird, der die Bewertungsaufträge im Batch-Cluster startet.

1. Bewertungsaufträge werden parallel die Knoten des Clusters übergreifend ausgeführt. Jeder Knoten:

    1. Ruft das Worker-Docker-Image aus Docker Hub ab und startet einen Container.

    1. Liest die Eingabedaten und vortrainierten R-Modelle aus Azure Blob Storage.

    1. Bewertet die Daten, um die Prognosen zu erstellen.

    1. Schreibt die Prognoseergebnisse in Blob Storage.

Die folgende Abbildung zeigt die vorhergesagten Verkäufe für vier Produkte (SKUs) in einer Filiale. Die schwarze Linie ist der Umsatzverlauf, die gestrichelte Linie ist die Medianvorhersage (q50), das rosa Band stellt das 25. und 75. Quantil dar und das blaue Band das fünfte und 95. Quantil.

![Verkaufsprognosen][1]

## <a name="architecture"></a>Architecture

Diese Architektur umfasst die folgenden Komponenten.

[Azure Batch][batch] wird verwendet, um Aufträge zur Vorhersagengenerierung parallel auf einem Cluster von virtuellen Computern auszuführen. Vorhersagen werden mithilfe vorab trainierter Machine Learning-Modelle getroffen, die in R implementiert sind. Azure Batch kann die Anzahl der VMs basierend auf der Anzahl der Aufträge, die an den Cluster übermittelt werden, automatisch skalieren. Auf jedem Knoten wird ein R-Skript zum Bewerten von Daten und Generieren von Vorhersagen in einem Docker-Container ausgeführt.

[Azure Blob Storage][blob] wird zum Speichern der Eingabedaten, der vorab trainierten Machine Learning-Modelle und der Prognoseergebnisse verwendet. Blob Storage bietet sehr kostengünstigen Speicher für die Leistung, die diese Workload erfordert.

[Azure Container Instances][aci] ermöglicht serverloses Compute nach Bedarf. In diesem Fall wird eine Containerinstanz nach einem Zeitplan bereitgestellt, um Batchaufträge auszulösen, die die Prognosen generieren. Die Batchaufträge werden von einem R-Skript unter Verwendung des [DoAzureParallel][doAzureParallel]-Pakets ausgelöst. Die Containerinstanz wird automatisch beendet, sobald die Aufträge abgeschlossen sind.

[Azure Logic Apps][logic-apps] löst den gesamten Workflow durch Bereitstellung der Containerinstanzen nach einem Zeitplan aus. Ein Azure Container Instances-Connector in Logic Apps ermöglicht, dass eine Instanz bei einer Reihe von Auslöseereignissen bereitgestellt werden kann.

## <a name="performance-considerations"></a>Überlegungen zur Leistung

### <a name="containerized-deployment"></a>Containerbereitstellung

Bei dieser Architektur werden alle R-Skripts in [Docker](https://www.docker.com/)-Containern ausgeführt. Dadurch wird sichergestellt, dass die Skripts jedes Mal in einer konsistenten Umgebung, mit der gleichen R-Version und den gleichen Paketversionen ausgeführt werden. Für den Planer- und Worker-Container werden separate Docker-Images verwendet, da jeder einen anderen Satz von R-Paketabhängigkeiten hat.

Azure Container Instances bietet eine serverlose Umgebung zum Ausführen des Planer-Containers. Der Planer-Container führt ein R-Skript aus, das die einzelnen in einem Azure Batch-Cluster ausgeführten Bewertungsaufträge auslöst.

Jeder Knoten des Batch-Clusters führt den Worker-Container aus, der das Bewertungsskript ausführt.

### <a name="parallelizing-the-workload"></a>Parallelisierung der Workload

Erwägen Sie bei der Batchbewertung von Daten mit R-Modellen, wie Sie die Workload parallelisieren. Die Eingabedaten müssen irgendwie partitioniert werden, damit der Bewertungsvorgang auf die Clusterknoten verteilt werden kann. Probieren Sie unterschiedliche Ansätze aus, um die beste Wahl für die Verteilung Ihrer Workload zu ermitteln. Berücksichtigen Sie auf Fall-zu-Fall-Basis Folgendes:

- Wie viele Daten in den Arbeitsspeicher eines einzelnen Knotens geladen und dort verarbeitet werden können.
- Den Mehraufwand, jeden einzelnen Batchauftrag zu starten.
- Den Mehraufwand, die R-Modelle zu laden.

In dem für dieses Beispiel verwendeten Szenario sind die Modellobjekte umfangreich, und es dauert nur wenige Sekunden, um eine Vorhersage für die einzelnen Produkte zu generieren. Aus diesem Grund können Sie die Produkte gruppieren und einen einzelnen Batchauftrag pro Knoten ausführen. Eine Schleife innerhalb jedes Auftrags generiert sequenziell Vorhersagen für die Produkte. Es zeigt sich, dass diese bestimmte Workload mit dieser Methode am effizientesten parallelisiert werden kann. Sie vermeidet den zusätzlichen Aufwand, viele kleinere Batchaufträge zu starten und die R-Modelle wiederholt zu laden.

Ein alternativer Ansatz ist, einen Batchauftrag pro Produkt auszulösen. Azure Batch bildet automatisch eine Warteschlange mit Aufträgen und übermittelt sie zur Ausführung an den Cluster, sobald Knoten verfügbar sind. Passen Sie mit der [automatischen Skalierung][autoscale] die Anzahl der Knoten im Cluster der Anzahl der Aufträge an. Dieser Ansatz ergibt mehr Sinn, wenn es relativ lange dauert, jeden Bewertungsvorgang auszuführen, sodass der Aufwand, die Aufträge zu starten und die Modellobjekte erneut zu laden, gerechtfertigt ist. Dieser Ansatz ist auch einfacher zu implementieren und bietet Ihnen die Flexibilität, die automatische Skalierung zu verwenden – ein wichtiger Aspekt, wenn die Größe der gesamten Workload im Voraus nicht bekannt ist.

## <a name="monitoring-and-logging-considerations"></a>Überlegungen zur Überwachung und Protokollierung

### <a name="monitoring-azure-batch-jobs"></a>Überwachen von Azure Batch-Aufträgen

Überwachen und beenden Sie Batchaufträge vom Bereich **Aufträge** des Batchkontos im Azure-Portal aus. Überwachen Sie den Batchcluster einschließlich des Status der einzelnen Knoten vom Bereich **Pools** aus.

### <a name="logging-with-doazureparallel"></a>Protokollierung mit doAzureParallel

Das doAzureParallel-Paket erfasst automatisch Protokolle aller stdout-/stderr-Vorgänge für jeden an Azure Batch übermittelten Auftrag. Diese Protokolle finden Sie im beim Setup erstellten Speicherkonto. Verwenden Sie zu ihrer Anzeige ein Speichernavigationstool wie z.B. den [Azure Storage-Explorer][storage-explorer] oder das Azure-Portal.

Um Batchaufträge während der Entwicklung schnell zu debuggen, drucken Sie Protokolle in Ihrer lokalen R-Sitzung mit der [getJobFiles][getJobFiles]-Funktion von doAzureParallel aus.

## <a name="cost-considerations"></a>Kostenbetrachtung

Die in dieser Referenzarchitektur genutzten Computeressourcen sind die teuersten Komponenten. Für dieses Szenario wird ein Cluster mit fester Größe erstellt, wenn der Auftrag ausgelöst wird, und dann nach Abschluss des Auftrags heruntergefahren. Kosten fallen nur an, während die Clusterknoten gestartet, ausgeführt oder heruntergefahren werden. Dieser Ansatz eignet sich für ein Szenario, in dem die zum Generieren der Vorhersagen benötigten Computeressourcen von Job zu Job relativ konstant bleiben.

In Szenarien, in denen die zum Ausführen des Auftrags erforderliche Computekapazität nicht im Voraus bekannt ist, kann es sinnvoller sein, die automatische Skalierung zu verwenden. Bei diesem Ansatz wird die Größe des Clusters abhängig von der Größe des Auftrags nach oben oder unten skaliert. Azure Batch unterstützt eine Reihe von Formeln für die automatische Skalierung, die Sie festlegen können, wenn Sie den Cluster mit der [doAzureParallel][doAzureParallel]-API definieren.

Für einige Szenarien ist die Zeit zwischen Aufträgen möglicherweise zu kurz, um den Cluster herunterzufahren und wieder zu starten. Behalten Sie in diesen Fällen die Ausführung des Clusters zwischen Aufträgen nach Möglichkeit bei.

Azure Batch und doAzureParallel unterstützen die Verwendung von VMs mit niedriger Priorität. Diese virtuellen Computer bieten einen erheblichen Rabatt, doch besteht das Risiko, das andere Workloads mit höherer Priorität sich diese VMs aneignen. Die Verwendung dieser virtuellen Computer ist daher für entscheidende Produktionsworkloads nicht zu empfehlen. Sie sind jedoch sehr nützlich für experimentelle oder Entwicklungsworkloads.

## <a name="deployment"></a>Bereitstellung

Befolgen Sie die Schritte im [GitHub-Repository][github], um diese Referenzarchitektur bereitzustellen.

[0]: ./_images/batch-scoring-r-models.png
[1]: ./_images/sales-forecasts.png
[aci]: /azure/container-instances/container-instances-overview
[autoscale]: /azure/batch/batch-automatic-scaling
[batch]: /azure/batch/batch-technical-overview
[blob]: /azure/storage/blobs/storage-blobs-introduction
[doAzureParallel]: https://github.com/Azure/doAzureParallel/blob/master/docs/32-autoscale.md
[getJobFiles]: /azure/machine-learning/service/how-to-train-ml-models
[github]: https://github.com/Azure/RBatchScoring
[logic-apps]: /azure/logic-apps/logic-apps-overview
[storage-explorer]: /azure/vs-azure-tools-storage-manage-with-storage-explorer?tabs=windows