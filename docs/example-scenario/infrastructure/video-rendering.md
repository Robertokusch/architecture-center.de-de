---
title: 3D-Videorendering in Azure
description: Führen Sie native HPC-Workloads in Azure mit dem Azure Batch-Dienst aus.
author: adamboeglin
ms.date: 07/13/2018
ms.openlocfilehash: 1206fa7d931fca635118929d433abe232ec5ca9a
ms.sourcegitcommit: b2a4eb132857afa70201e28d662f18458865a48e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/05/2018
ms.locfileid: "48818626"
---
# <a name="3d-video-rendering-on-azure"></a>3D-Videorendering in Azure

3D-Videorendering ist ein zeitaufwendiger Prozess, der sehr viel CPU-Zeit beansprucht. Auf einem einzelnen Computer kann das Generieren einer Videodatei aus statischen Medienobjekten Stunden oder sogar Tage dauern – je nach Länge und Komplexität des produzierten Videos. Viele Unternehmen erwerben entweder teure Highend-Computer für diese Aufgaben oder investieren in große Renderfarmen, an die Aufträge übermittelt werden können. Bei Nutzung von Azure Batch ist diese hohe Leistung aber für Sie verfügbar, wenn Sie sie benötigen, und wird heruntergefahren, wenn Sie sie nicht benötigen – ohne jegliche Kapitalinvestitionen.

Mit Batch erhalten Sie unabhängig davon, ob Sie Windows Server- oder Linux-Computeknoten wählen, eine einheitliche Verwaltungsoberfläche und Auftragsplanung. Für Batch können Sie Ihre vorhandenen Windows- oder Linux-Anwendungen nutzen, z.B. AutoDesk Maya und Blender, um in Azure umfangreiche Renderaufträge auszuführen.

## <a name="relevant-use-cases"></a>Relevante Anwendungsfälle

Erwägen Sie dieses Szenario für ähnliche Anwendungsfälle:

* 3D-Modellierung
* VFX-Rendering (Visual FX)
* Videotranscodierung
* Bildverarbeitung, Farbkorrektur und Größenänderung

## <a name="architecture"></a>Architektur

![Architekturübersicht über die Komponenten einer cloudbasierten HPC-Lösung mit Azure Batch][architecture]

Dieses Szenario zeigt einen Workflow, der Azure Batch verwendet. Die Daten fließen wie folgt:

1. Hochladen der Eingabedateien und der Anwendungen, mit denen diese Dateien verarbeitet werden, in Ihr Azure Storage-Konto
2. Erstellen eines Batch-Pools mit Computeknoten in Ihrem Batch-Konto, eines Auftrags zum Ausführen der Workload im Pool und von Aufgaben im Auftrag
3. Herunterladen von Eingabedateien und der Anwendungen in Batch
4. Überwachen der Aufgabenausführung
5. Hochladen der Aufgabenausgabe
6. Herunterladen der Ausgabedateien

Zur Vereinfachung dieses Prozesses können Sie auch die [Batch-Plug-Ins für Maya & 3ds Max][batch-plugins] verwenden.

### <a name="components"></a>Komponenten

Azure Batch basiert auf der folgenden Azure-Technologie:

* [Virtuelle Netzwerke](/azure/virtual-network/virtual-networks-overview) werden sowohl für den Hauptknoten als auch für die Computeressourcen verwendet.
* [Azure Storage-Konten](/azure/storage/common/storage-introduction) werden für die Synchronisierung und Datenaufbewahrung verwendet.
* [VM-Skalierungsgruppen][vmss] werden von CycleCloud für Computeressourcen genutzt.

## <a name="considerations"></a>Überlegungen

### <a name="machine-sizes-available-for-azure-batch"></a>Verfügbare Computergrößen für Azure Batch

Die meisten Renderingkunden entscheiden sich zwar für Ressourcen mit hoher CPU-Leistung, für andere Workloads mit VM-Skalierungsgruppen werden virtuelle Computer aber ggf. anders ausgewählt und sind von verschiedenen Faktoren abhängig:

* Ist die ausgeführte Anwendung speichergebunden?
* Müssen für die Anwendung GPUs verwendet werden? 
* Gilt für die Auftragstypen eine hohe Parallelität, oder ist für eng gekoppelte Aufträge eine Infiniband-Konnektivität erforderlich?
* Ist für den Zugriff auf Speicher auf den Computeknoten eine hohe E/A-Geschwindigkeit erforderlich?

Azure verfügt über ein breites Spektrum an VM-Größen, mit denen alle oben beschriebenen Anwendungsanforderungen erfüllt werden können. Einige gelten speziell für HPC, aber auch die kleinsten Größen können genutzt werden, um eine effektive Rasterimplementierung zu erreichen:

* [HPC-VM-Größen][compute-hpc] Aufgrund der CPU-Bindung des Renderns empfiehlt Microsoft normalerweise die Verwendung von virtuellen Azure-Computern der H-Serie. Diese Art von virtuellem Computer ist speziell auf Highend-Computeranforderungen ausgelegt und verfügt über vCPU-Größen mit 8 oder 16 Kernen sowie über DDR4-Arbeitsspeicher, temporären SSD-Speicher und Haswell E5 Intel-Technologie.
* [GPU-VM-Größen][compute-gpu] GPU-optimierte VM-Größen sind für spezialisierte virtuelle Computer mit einzelnen oder mehreren NVIDIA-GPUs verfügbar. Diese Größen sind für rechenintensive, grafikintensive und visualisierungsorientierte Workloads vorgesehen.
* Die Größen NC, NCv2, NCv3 und ND sind für rechen- und netzwerkintensive Anwendungen und Algorithmen wie CUDA- und OpenCL-basierte Anwendungen und Simulationen sowie für KI und Deep Learning optimiert. NV-Größen sind für Remotevisualisierung, Streaming, Spiele, Codierung und VDI-Szenarien mit Frameworks wie OpenGL und DirectX optimiert und konzipiert.
* [Arbeitsspeicheroptimierte VM-Größen][compute-memory] Wenn mehr Arbeitsspeicher erforderlich ist, ermöglichen die arbeitsspeicheroptimierten VM-Größen ein höheres Arbeitsspeicher/CPU-Verhältnis.
* [Universelle VM-Größen][compute-general] Es sind auch universelle VM-Größen mit einem ausgewogenen Verhältnis von CPU zu Arbeitsspeicher verfügbar.

### <a name="alternatives"></a>Alternativen

Falls Sie eine bessere Kontrolle über Ihre Renderingumgebung in Azure oder eine Hybridimplementierung benötigen, kann CycleCloud-Computing die Orchestrierung eines IaaS-Rasters in der Cloud unterstützen. Indem die gleiche zugrunde liegende Azure-Technologie wie für Azure Batch verwendet wird, wird das Erstellen und Warten eines IaaS-Rasters zu einem effizienten Prozess. Unter dem folgenden Link finden Sie weitere Informationen zu den Entwurfsprinzipien:

Eine vollständige Übersicht über alle HPC-Lösungen, die für Sie in Azure verfügbar sind, finden Sie im Artikel [HPC-, Batch- und Big Compute-Lösungen mit Azure-VMs][hpc-alt-solutions].

### <a name="availability"></a>Verfügbarkeit

Die Überwachung der Azure Batch-Komponenten ist über unterschiedliche Dienste, Tools und APIs möglich. Weitere Informationen zur Überwachung finden Sie im Artikel [Überwachen von Batch-Lösungen][batch-monitor].

### <a name="scalability"></a>Skalierbarkeit

Pools eines Azure Batch-Kontos können entweder per manuellem Eingriff oder automatisch mit einer auf Azure Batch-Metriken basierenden Formel skaliert werden. Weitere Informationen zur Skalierbarkeit finden Sie im Artikel [Erstellen einer Formel für die automatische Skalierung von Computeknoten in einem Batch-Pool][batch-scaling].

### <a name="security"></a>Sicherheit

Allgemeine Informationen zur Entwicklung sicherer Lösungen finden Sie in der [Dokumentation zur Azure-Sicherheit][security].

### <a name="resiliency"></a>Resilienz

Da in Azure Batch derzeit keine Failoverfunktion verfügbar ist, empfehlen wir die folgenden Schritte, um die Verfügbarkeit auch bei einem ungeplanten Ausfall sicherzustellen:

* Erstellen eines Azure Batch-Kontos an einem anderen Azure-Standort mit einem anderen Speicherkonto
* Erstellen der gleichen Knotenpools mit dem gleichen Namen und null zugeordneten Knoten
* Sicherstellen, dass Anwendungen erstellt und auf das alternative Speicherkonto aktualisiert werden
* Hochladen von Eingabedateien und Übermitteln von Aufträgen an das andere Azure Batch-Konto

## <a name="deploy-this-scenario"></a>Bereitstellen dieses Szenarios

### <a name="creating-an-azure-batch-account-and-pools-manually"></a>Manuelles Erstellen eines Azure Batch-Kontos und der Pools

In diesem Szenario wird die Funktionsweise von Azure Batch veranschaulicht und Azure Batch Labs als SaaS-Beispiellösung verwendet, die für Ihre eigenen Kunden weiterentwickelt werden kann:

[Azure Batch – Masterclass][batch-labs-masterclass]

### <a name="deploying-the-example-scenario-using-an-azure-resource-manager-template"></a>Bereitstellen des Beispielszenarios mit einer Azure Resource Manager-Vorlage

Über die Vorlage wird Folgendes bereitgestellt:

* Ein neues Azure Batch-Konto
* Ein Speicherkonto
* Einen Knotenpool, der dem Batch-Konto zugeordnet ist
* Knotenpool wird für die Verwendung von A2 v2-VMs mit Canonical Ubuntu-Images konfiguriert
* Der Knotenpool enthält anfänglich null virtuelle Computer, und zum Hinzufügen von virtuellen Computern ist eine manuelle Skalierung erforderlich.

<a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2Fmspnp%2Fsolution-architectures%2Fmaster%2Fhpc%2Fbatchcreatewithpools.json" target="_blank">
    <img src="https://azuredeploy.net/deploybutton.png"/>
</a>

Weitere Informationen zu Resource Manager-Vorlagen finden Sie [hier][azure-arm-templates].

## <a name="pricing"></a>Preise

Die Kosten für die Nutzung von Azure Batch richten sich nach den VM-Größen, die für die Pools verwendet werden, sowie nach der Dauer der Zuordnung und Ausführung der virtuellen Computer. Für die Erstellung eines Azure Batch-Kontos fallen keine Kosten an. Speicher und ausgehende Daten sollten berücksichtigt werden, da hierfür weitere Kosten anfallen.

Hier sind Beispiele für Kosten angegeben, die für einen Auftrag anfallen können, der innerhalb von acht Stunden abgeschlossen wird, indem eine andere Zahl von Servern genutzt wird:

* 100 virtuelle Computer mit Hochleistungs-CPU: [Kostenschätzung][hpc-est-high]

  100 x H16m (16 Kerne, 225 GB RAM, 512 GB Storage Premium), 2 TB Blobspeicher, 1 TB ausgehender Datenverkehr

* 50 virtuelle Computer mit Hochleistungs-CPU: [Kostenschätzung][hpc-est-med]

  50 x H16m (16 Kerne, 225 GB RAM, 512 GB Storage Premium), 2 TB Blobspeicher, 1 TB ausgehender Datenverkehr

* 10 virtuelle Computer mit Hochleistungs-CPU: [Kostenschätzung][hpc-est-low]

  10 x H16m (16 Kerne, 225 GB RAM, 512 GB Storage Premium), 2 TB Blobspeicher, 1 TB ausgehender Datenverkehr

### <a name="pricing-for-low-priority-vms"></a>Preise für virtuelle Computer mit niedriger Priorität

Azure Batch unterstützt auch die Verwendung virtueller Computer mit niedriger Priorität in den Knotenpools, was zu erheblichen Kosteneinsparungen führen kann. Weitere Informationen sowie einen Preisvergleich zwischen virtuellen Standardcomputern und virtuellen Computern mit niedriger Priorität finden Sie unter [Batch – Preise][batch-pricing].

> [!NOTE] 
> Virtuelle Computer mit niedriger Priorität sind nur für bestimmte Anwendungen und Workloads geeignet.

## <a name="related-resources"></a>Zugehörige Ressourcen

[Azure Batch – Übersicht][batch-overview]

[Azure Batch-Dokumentation][batch-doc]

[Verwenden von Containern in Azure Batch][batch-containers]

<!-- links -->
[architecture]: ./media/architecture-video-rendering.png
[resource-groups]: /azure/azure-resource-manager/resource-group-overview
[security]: /azure/security/
[resiliency]: /azure/architecture/resiliency/
[scalability]: /azure/architecture/checklist/scalability
[vmss]: /azure/virtual-machine-scale-sets/overview
[storage]: https://azure.microsoft.com/services/storage/
[batch]: https://azure.microsoft.com/services/batch/
[batch-arch]: https://azure.microsoft.com/solutions/architecture/big-compute-with-azure-batch/
[compute-hpc]: /azure/virtual-machines/windows/sizes-hpc
[compute-gpu]: /azure/virtual-machines/windows/sizes-gpu
[compute-compute]: /azure/virtual-machines/windows/sizes-compute
[compute-memory]: /azure/virtual-machines/windows/sizes-memory
[compute-general]: /azure/virtual-machines/windows/sizes-general
[compute-storage]: /azure/virtual-machines/windows/sizes-storage
[compute-acu]: /azure/virtual-machines/windows/acu
[compute=benchmark]: /azure/virtual-machines/windows/compute-benchmark-scores
[hpc-est-high]: https://azure.com/e/9ac25baf44ef49c3a6b156935ee9544c
[hpc-est-med]: https://azure.com/e/0286f1d6f6784310af4dcda5aec8c893
[hpc-est-low]: https://azure.com/e/e39afab4e71949f9bbabed99b428ba4a
[batch-labs-masterclass]: https://github.com/azurebigcompute/BigComputeLabs/tree/master/Azure%20Batch%20Masterclass%20Labs
[batch-scaling]: /azure/batch/batch-automatic-scaling
[hpc-alt-solutions]: /azure/virtual-machines/linux/high-performance-computing?toc=%2fazure%2fbatch%2ftoc.json
[batch-monitor]: /azure/batch/monitoring-overview
[batch-pricing]: https://azure.microsoft.com/pricing/details/batch/
[batch-doc]: /azure/batch/
[batch-overview]: https://azure.microsoft.com/services/batch/
[batch-containers]: https://github.com/Azure/batch-shipyard
[azure-arm-templates]: /azure/azure-resource-manager/resource-group-overview#template-deployment
[batch-plugins]: /azure/batch/batch-rendering-service#options-for-submitting-a-render-job