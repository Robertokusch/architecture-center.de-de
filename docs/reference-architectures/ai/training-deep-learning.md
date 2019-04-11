---
title: Verteiltes Trainieren von Deep Learning-Modellen in Azure
description: Diese Referenzarchitektur zeigt, wie Sie unter Verwendung von Azure Batch AI ein verteiltes Training von Deep Learning-Modellen in mehreren Clustern mit GPU-fähigen virtuellen Computern durchführen.
author: njray
ms.date: 01/14/19
ms.custom: azcat-ai
ms.openlocfilehash: 0e63ed24c1e19333e7c9d48d531a7ddc778f3d86
ms.sourcegitcommit: c053e6edb429299a0ad9b327888d596c48859d4a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/20/2019
ms.locfileid: "58242391"
---
# <a name="distributed-training-of-deep-learning-models-on-azure"></a>Verteiltes Trainieren von Deep Learning-Modellen in Azure

Diese Referenzarchitektur zeigt, wie Sie ein verteiltes Training von Deep Learning-Modellen in mehreren Clustern mit GPU-fähigen virtuellen Computern durchführen. Bei dem Szenario handelt es sich zwar um eine Bildklassifizierung, die Lösung kann jedoch auch für andere Deep Learning-Szenarien (etwa für die Segmentierung oder Objekterkennung) generalisiert werden.

Eine Referenzimplementierung für diese Architektur ist auf [GitHub][github] verfügbar.

![Architektur für verteiltes Deep Learning][0]

**Szenario:** Bildklassifizierung ist eine weit verbreitete Technik in Verbindung mit maschinellem Sehen und wird häufig durch Trainieren eines auf dem Faltungsprinzip basierenden neuronalen Netzes (Convolutional Neural Network, CNN) umgesetzt. Bei besonders umfangreichen Modellen mit großen Datasets kann das Training mit einer einzelnen GPU Wochen oder Monate dauern. Manchmal sind die Modelle sogar so groß, dass gar keine sinnvollen Batchgrößen für die GPU möglich sind. In solchen Fällen lässt sich die Trainingsdauer mithilfe eines verteilten Trainings verkürzen.

In diesem spezifischen Szenario wird ein [ResNet50-CNN-Modell][resnet] unter Verwendung von [Horovod][horovod] mit dem [Imagenet-Dataset][imagenet] sowie mit synthetischen Daten trainiert. Die Referenzimplementierung veranschaulicht die Bewältigung dieser Aufgabe anhand von drei der beliebtesten Deep Learning-Frameworks: TensorFlow, Keras und PyTorch.

Für das verteilte Trainieren von Deep Learning-Modellen gibt es verschiedene Möglichkeiten. Hierzu zählen unter anderem Ansätze mit parallelen Daten und parallelem Modell auf der Grundlage von synchronen oder asynchronen Aktualisierungen. Das gängigste Szenario ist derzeit der Ansatz mit parallelen Daten und synchronen Aktualisierungen. Dieser Ansatz lässt sich am einfachsten implementieren und ist in den meisten Fällen ausreichend.

Beim verteilten Trainieren mit parallelen Daten und synchronen Aktualisierungen wird das Modell auf *n* Hardwaregeräten repliziert. Ein Minibatch mit Trainingsbeispielen wird auf *n* Mikrobatches aufgeteilt. Jedes Gerät führt die Vorwärts- und Rückwärtsvorgänge für einen Mikrobatch aus. Nach Abschluss des Prozesses gibt das jeweilige Gerät die Aktualisierungen an die anderen Geräte weiter. Die Werte werden verwendet, um die aktualisierten Gewichtungen des gesamten Minibatchs zu berechnen, und die Gewichtungen werden modellübergreifend synchronisiert. Dieses Szenario wird in [diesem GitHub-Repository][github] behandelt.

![Verteiltes Trainieren mit parallelen Daten][1]

Diese Architektur kann auch für den Ansatz mit parallelem Modell und asynchronen Aktualisierungen verwendet werden. Beim verteilten Trainieren mit parallelem Modell wird das Modell auf *n* Hardwaregeräte aufgeteilt, wobei jedes Gerät über einen Teil des Modells verfügt. In der einfachsten Implementierung kann jedes Gerät über eine Schicht des Netzwerks verfügen, und Informationen werden während der Vorwärts- und Rückwärtsphase zwischen Geräten ausgetauscht. Größere neuronale Netze können zwar auf diese Weise trainiert werden, dies geht jedoch zulasten der Leistung, da Geräte ständig darauf warten, dass andere Geräte entweder die Vorwärts- oder die Rückwärtsphase abschließen. Bei einigen erweiterten Techniken wird mithilfe synthetischer Gradienten versucht, dieses Problem zumindest teilweise zu entschärfen.

Das Training umfasst folgende Schritte:

1. Erstellen von Skripts, die im Cluster ausgeführt werden und Ihr Modell trainieren, und anschließendes Übertragen in den Dateispeicher

1. Schreiben der Daten in Blob Storage

1. Erstellen eines Batch AI-Dateiservers und Herunterladen der Daten aus Blob Storage

1. Erstellen der Docker-Container für das jeweilige Deep Learning-Framework und Übertragen der Container in eine Containerregistrierung (Docker Hub)

1. Erstellen eines Batch AI-Pools, der auch den Batch AI-Dateiserver einbindet

1. Übermitteln von Aufträgen. Jeder Auftrag ruft mittels Pullvorgang das entsprechende Docker-Image sowie die Skripts ab.

1. Nach Abschluss des Auftrags: Schreiben aller Ergebnisse in den Dateispeicher

## <a name="architecture"></a>Architecture

Diese Architektur umfasst die folgenden Komponenten.

**[Azure Batch AI][batch-ai]** bildet das Herzstück dieser Architektur und sorgt für eine bedarfsgerechte Skalierung der Ressourcen. Der Batch AI-Dienst unterstützt Sie beim Bereitstellen und Verwalten von VM-Clustern, beim Planen von Aufträgen, beim Erfassen der Ergebnisse, beim Skalieren der Ressourcen, beim Behandeln von Fehlern sowie beim Erstellen des erforderlichen Speichers. Er unterstützt GPU-fähige virtuelle Computer für Deep Learning-Workloads. Für Batch AI sind ein Python SDK und eine Befehlszeilenschnittstelle (Command-Line Interface, CLI) verfügbar.

> [!NOTE]
> Der Azure Batch AI-Dienst wird im März 2019 eingestellt. Die skalierbaren Trainings- und Bewertungsfunktionen dieses Diensts sind nun in [Azure Machine Learning Service][amls] verfügbar. Diese Referenzarchitektur wird demnächst für die Verwendung von Machine Learning aktualisiert, wodurch ein verwaltetes Computeziel namens [Azure Machine Learning Compute][aml-compute] zum Trainieren, Bereitstellen und Bewerten von Machine Learning-Modellen zur Verfügung steht.

**[Blob Storage][azure-blob]** wird zum Bereitstellen der Daten verwendet. Die Daten werden im Rahmen des Trainings auf einen Batch AI-Dateiserver heruntergeladen.

**[Azure Files][files]** wird zum Speichern der Skripts, Protokolle und Endergebnisse des Trainings verwendet. File Storage eignet sich sehr gut zum Speichern von Protokollen und Skripts, ist aber nicht so leistungsfähig wie Blob Storage und sollte daher nicht für datenintensive Aufgaben verwendet werden.

**[Batch AI-Dateiserver][batch-ai-files]** ist eine NFS-Freigabe mit einem einzelnen Knoten und wird in dieser Architektur zum Speichern der Trainingsdaten verwendet. Batch AI erstellt eine NFS-Freigabe und bindet sie auf dem Cluster ein. Batch AI-Dateiserver sind die empfohlene Methode, um Daten mit dem nötigen Durchsatz für den Cluster bereitzustellen.

**[Docker-Hub][docker]** wird zum Speichern des Docker-Images benutzt, das Batch AI zum Ausführen des Trainings verwendet. Der Docker-Hub wurde für diese Architektur ausgewählt, weil er benutzerfreundlich und das Standardimagerepository für Docker-Benutzer ist. [Azure Container Registry][acr] kann ebenfalls verwendet werden.

## <a name="performance-considerations"></a>Überlegungen zur Leistung

Azure bietet vier [GPU-fähige VM-Typen][gpu], die zum Trainieren von Deep Learning-Modellen geeignet sind. Im Anschluss finden Sie Informationen zu den verschiedenen Preisen und zur jeweiligen Geschwindigkeit (in aufsteigender Reihenfolge):

| **Azure-VM-Serie** | **NVIDIA-GPU** |
|---------------------|----------------|
| NC                  | K80            |
| ND                  | P40            |
| NCv2                | P100           |
| NCv3                | V100           |

Es empfiehlt sich, das Training zunächst zentral zu skalieren, bevor Sie eine horizontale Skalierung durchführen. Versuchen Sie es also beispielsweise erst mit einer einzelnen V100-Instanz, bevor Sie einen Cluster mit K80-Instanzen verwenden.

Das folgende Diagramm zeigt die Leistungsunterschiede für verschiedene GPU-Typen auf der Grundlage von [Benchmarktests][benchmark], die mit TensorFlow und Horovod in Batch AI durchgeführt wurden. Das Diagramm zeigt den Durchsatz von 32 GPU-Clustern für verschiedene Modelle mit unterschiedlichen GPU-Typen und MPI-Versionen. Die Modelle wurden in TensorFlow 1.9 implementiert.

![Durchsatzergebnisse für TensorFlow-Modelle in GPU-Clustern][2]

Jede VM-Serie aus der obigen Tabelle verfügt über eine Konfiguration mit InfiniBand. Verwenden Sie die InfiniBand-Konfigurationen beim Ausführen von verteilten Trainings, um die Kommunikation zwischen Knoten zu beschleunigen. InfiniBand erhöht außerdem die Skalierungseffizienz des Trainings für die Frameworks, die dafür geeignet sind. Ausführliche Informationen finden Sie im [Benchmarkvergleich][benchmark] für InfiniBand.

Batch AI kann Blob Storage zwar mithilfe des Adapters [blobfuse][blobfuse] einbinden, wir raten jedoch davon ab, Blob Storage auf diese Weise für verteilte Trainings zu verwenden, da die Leistung für den erforderlichen Durchsatz nicht ausreicht. Verschieben Sie die Daten stattdessen auf einen Batch AI-Dateiserver, wie im Architekturdiagramm dargestellt.

## <a name="scalability-considerations"></a>Überlegungen zur Skalierbarkeit

Bei der Skalierungseffizienz des verteilten Trainings werden aufgrund des Zusatzaufwands für das Netzwerk niemals 100 Prozent erreicht: Die Synchronisierung des gesamten Modells zwischen Geräten wird zu einem Engpass. Somit eignen sich verteilte Trainings am besten für umfangreiche Modelle, die nicht mit einer sinnvollen Batchgröße auf einer einzelnen GPU trainiert werden können, oder für Probleme, die sich nicht durch eine einfache, parallele Verteilung des Modells lösen lassen.

Verteilte Trainings sollten nicht für Hyperparameter-Suchvorgänge verwendet werden. Die Skalierungseffizienz wirkt sich auf die Leistung aus und macht einen verteilten Ansatz weniger effizient als das separate Trainieren mehrerer Modellkonfigurationen.

Eine Möglichkeit zur Steigerung der Skalierungseffizienz ist die Erhöhung der Batchgröße. Dabei ist jedoch Vorsicht geboten, da die Erhöhung der Batchgröße ohne Anpassung der anderen Parameter letztendlich die Leistung des Modells beeinträchtigen kann.

## <a name="storage-considerations"></a>Speicheraspekt

Beim Trainieren von Deep Learning-Modellen wird oftmals der Speicherort der Daten außer Acht gelassen. Ist der Speicher den Anforderungen der GPUs nicht gewachsen, geht dies unter Umständen zulasten der Trainingsleistung.

Batch AI unterstützt verschiedenste Speicherlösungen. In dieser Architektur wird ein Batch AI-Dateiserver verwendet, da er das beste Verhältnis zwischen Benutzerfreundlichkeit und Leistung bietet. Die beste Leistung erzielen Sie, wenn Sie die Daten lokal laden. Dies kann sich jedoch als umständlich erweisen, da alle Knoten die Daten aus Blob Storage herunterladen müssen, was bei dem ImageNet-Dataset mehrere Stunden dauern kann. Eine andere empfehlenswerte Option ist [Azure Premium Blob Storage][blob] (eingeschränkte Public Preview).

Binden Sie für verteilte Trainings weder Blob noch File Storage als Datenspeicher ein. Diese Optionen sind zu langsam und beeinträchtigen die Trainingsleistung.

## <a name="security-considerations"></a>Sicherheitshinweise

### <a name="restrict-access-to-azure-blob-storage"></a>Beschränken des Zugriffs auf Azure Blob Storage

In dieser Architektur werden [Speicherkontoschlüssel][security-guide] für den Zugriff auf Blob Storage verwendet. Für noch mehr Kontrolle und Schutz sollten Sie stattdessen Shared Access Signatur (SAS) verwenden. Dadurch wird der Zugriff auf die gespeicherten Objekte eingeschränkt, ohne dass die Kontoschlüssel hartcodiert oder als Klartext gespeichert werden müssen. Mit SAS können Sie außerdem sicherstellen, dass das Speicherkonto über eine ordnungsgemäße Governance verfügt und der Zugriff nur ausgewählten Personen gewährt wird.

Stellen Sie in Szenarien mit sensibleren Daten sicher, dass alle Ihre Speicherschlüssel geschützt sind, weil diese Schlüssel den Vollzugriff auf alle Ein- und Ausgabedaten der Workload ermöglichen.

### <a name="encrypt-data-at-rest-and-in-motion"></a>Verschlüsseln von ruhenden und übertragenen Daten

In Szenarien mit sensiblen Daten müssen ruhende Daten (Daten im Speicher) verschlüsselt werden. Bei Datenübertragungen müssen die Daten mithilfe von SSL geschützt werden. Weitere Informationen finden Sie im [Azure Storage-Sicherheitsleitfaden][security-guide].

### <a name="secure-data-in-a-virtual-network"></a>Schützen von Daten in einem virtuellen Netzwerk

Bei Produktionsbereitstellungen empfiehlt es sich ggf., den Batch AI-Cluster in einem Subnetz eines von Ihnen angegebenen virtuellen Netzwerks bereitzustellen. Dadurch können die Computeknoten im Cluster sicher mit anderen virtuellen Computern oder mit einem lokalen Netzwerk kommunizieren. Sie können auch [Dienstendpunkte][endpoints] mit Blob Storage verwenden, um den Zugriff über ein virtuelles Netzwerk zu ermöglichen, oder ein NFS mit einem einzelnen Knoten innerhalb des virtuellen Netzwerks mit Batch AI.

## <a name="monitoring-considerations"></a>Aspekte der Überwachung

Beim Ausführen Ihres Auftrags ist es wichtig, den Fortschritt zu überwachen und zu überprüfen, ob alles wie erwartet funktioniert. Es kann jedoch eine Herausforderung sein, über einen Cluster von aktiven Knoten hinweg zu überwachen.

Die Batch AI-Dateiserver können über das Azure-Portal oder über die [Azure-Befehlszeilenschnittstelle][cli] und das Python SDK verwaltet werden. Um einen Eindruck vom Gesamtzustand des Clusters zu bekommen, navigieren Sie im Azure-Portal zu **Batch AI**, und untersuchen Sie den Zustand der Clusterknoten. Wenn ein Knoten inaktiv ist oder bei einem Auftrag ein Fehler auftritt, werden die Fehlerprotokolle in Blob Storage gespeichert und stehen auch im Azure-Portal unter **Aufträge** zur Verfügung.

Erweitern Sie die Überwachung, indem Sie Protokolle mit [Azure Application Insights][ai] verknüpfen oder separate Prozesse ausführen, die den Zustand des Batch AI-Clusters und der dazugehörigen Aufträge abrufen.

Batch AI protokolliert automatisch alle stdout/stderr-Ereignisse im entsprechenden Blob Storage-Konto. Speichernavigationstools wie der [Azure Storage-Explorer][storage-explorer] vereinfachen die Navigation durch die Protokolldateien.

Die Protokolle für die einzelnen Aufträge können auch gestreamt werden. Ausführliche Informationen zu dieser Option finden Sie in den Entwicklungsschritten auf [GitHub][github].

## <a name="deployment"></a>Bereitstellung

Die Referenzimplementierung dieser Architektur ist auf [GitHub][github] verfügbar. Führen Sie die dort beschriebenen Schritte aus, um ein verteiltes Training von Deep Learning-Modellen in mehreren Clustern mit GPU-fähigen virtuellen Computern durchzuführen.

## <a name="next-steps"></a>Nächste Schritte

Diese Architektur gibt ein trainiertes, in Blob Storage gespeichertes Modell aus. Dieses Modell kann für Echtzeitbewertungen oder für Batchbewertungen operationalisiert werden. Weitere Informationen finden Sie in den folgenden Referenzarchitekturen:

- [Echtzeitbewertung von Python-Modellen (scikit-learn) und Deep Learning-Modellen in Azure][real-time-scoring]
- [Batchbewertung in Azure für Deep Learning-Modelle][batch-scoring]

[0]: ./_images/distributed_dl_architecture.png
[1]: ./_images/distributed_dl_flow.png
[2]: ./_images/distributed_dl_tests.png
[acr]: /azure/container-registry/container-registry-intro
[ai]: /azure/application-insights/app-insights-overview
[aml-compute]: /azure/machine-learning/service/how-to-set-up-training-targets#amlcompute
[amls]: /azure/machine-learning/service/overview-what-is-azure-ml
[azure-blob]: /azure/storage/blobs/storage-blobs-introduction
[batch-ai]: /azure/batch-ai/overview
[batch-ai-files]: /azure/batch-ai/resource-concepts#file-server
[batch-scoring]: /azure/architecture/reference-architectures/ai/batch-scoring-deep-learning
[benchmark]: https://github.com/msalvaris/BatchAIHorovodBenchmark
[blob]: https://azure.microsoft.com/en-gb/blog/introducing-azure-premium-blob-storage-limited-public-preview/
[blobfuse]: https://github.com/Azure/azure-storage-fuse
[cli]: https://github.com/Azure/BatchAI/blob/master/documentation/using-azure-cli-20.md
[docker]: https://hub.docker.com/
[endpoints]: /azure/storage/common/storage-network-security?toc=%2fazure%2fvirtual-network%2ftoc.json#grant-access-from-a-virtual-network
[files]: /azure/storage/files/storage-files-introduction
[github]: https://github.com/Azure/DistributedDeepLearning/
[gpu]: /azure/virtual-machines/windows/sizes-gpu
[horovod]: https://github.com/uber/horovod
[imagenet]: http://www.image-net.org/
[real-time-scoring]: /azure/architecture/reference-architectures/ai/realtime-scoring-python
[resnet]: https://arxiv.org/abs/1512.03385
[security-guide]: /azure/storage/common/storage-security-guide
[storage-explorer]: /azure/vs-azure-tools-storage-manage-with-storage-explorer?tabs=windows
[tutorial]: https://github.com/Azure/DistributedDeepLearning