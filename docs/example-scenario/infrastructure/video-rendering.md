---
title: 3D-Videorendering in Azure
description: Ausführen von nativen HPC-Workloads in Azure mit dem Azure Batch-Dienst
author: adamboeglin
ms.date: 07/13/2018
ms.openlocfilehash: b3af0641642d7ec4b022e8c96f51693eeb0adee4
ms.sourcegitcommit: 71cbef121c40ef36e2d6e3a088cb85c4260599b9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/14/2018
ms.locfileid: "39061001"
---
# <a name="3d-video-rendering-on-azure"></a><span data-ttu-id="81f4e-103">3D-Videorendering in Azure</span><span class="sxs-lookup"><span data-stu-id="81f4e-103">3D video rendering on Azure</span></span>

<span data-ttu-id="81f4e-104">Das 3D-Rendering ist ein zeitaufwändiger Prozess, für dessen Durchführung eine erhebliche Menge an CPU-Zeit erforderlich ist.</span><span class="sxs-lookup"><span data-stu-id="81f4e-104">3D rendering is a time consuming process that requires a significant amount of CPU time co complete.</span></span>  <span data-ttu-id="81f4e-105">Auf einem einzelnen Computer kann das Generieren einer Videodatei aus statischen Medienobjekten Stunden oder sogar Tage dauern – je nach Länge und Komplexität des produzierten Videos.</span><span class="sxs-lookup"><span data-stu-id="81f4e-105">On a single machine, the process of generating a video file from static assets can take hours or even days depending on the length and complexity of the video you are producing.</span></span>  <span data-ttu-id="81f4e-106">Viele Unternehmen erwerben entweder teure Highend-Computer für diese Aufgaben oder investieren in große Renderfarmen, an die Aufträge übermittelt werden können.</span><span class="sxs-lookup"><span data-stu-id="81f4e-106">Many companies will purchase either expensive high end desktop computers to perform these tasks, or invest in large render farms that they can submit jobs to.</span></span>  <span data-ttu-id="81f4e-107">Bei Nutzung von Azure Batch ist diese hohe Leistung aber für Sie verfügbar, wenn Sie sie benötigen, und wird heruntergefahren, wenn Sie sie nicht benötigen – ohne jegliche Kapitalinvestitionen.</span><span class="sxs-lookup"><span data-stu-id="81f4e-107">However, by taking advantage of Azure Batch, that power is available to you when you need it and shuts itself down when you don't, all without any capital investment.</span></span>

<span data-ttu-id="81f4e-108">Mit Batch erhalten Sie unabhängig davon, ob Sie Windows Server- oder Linux-Computeknoten wählen, eine einheitliche Verwaltungsoberfläche und Auftragsplanung.</span><span class="sxs-lookup"><span data-stu-id="81f4e-108">Batch gives you a consistent management experience and job scheduling, whether you select Windows Server or Linux compute nodes.</span></span> <span data-ttu-id="81f4e-109">Für Batch können Sie Ihre vorhandenen Windows- oder Linux-Anwendungen nutzen, z.B. AutoDesk Maya und Blender, um in Azure umfangreiche Renderaufträge auszuführen.</span><span class="sxs-lookup"><span data-stu-id="81f4e-109">With Batch, you can use your existing Windows or Linux applications, including AutoDesk Maya and Blender, to run large-scale render jobs in Azure.</span></span>

## <a name="related-use-cases"></a><span data-ttu-id="81f4e-110">Verwandte Anwendungsfälle</span><span class="sxs-lookup"><span data-stu-id="81f4e-110">Related use cases</span></span>

<span data-ttu-id="81f4e-111">Erwägen Sie dieses Szenario für ähnliche Anwendungsfälle:</span><span class="sxs-lookup"><span data-stu-id="81f4e-111">Consider this scenario for these similar use cases:</span></span>

* <span data-ttu-id="81f4e-112">3D-Modellierung</span><span class="sxs-lookup"><span data-stu-id="81f4e-112">3D Modeling</span></span>
* <span data-ttu-id="81f4e-113">VFX-Rendering (Visual FX)</span><span class="sxs-lookup"><span data-stu-id="81f4e-113">Visual FX (VFX) Rendering</span></span>
* <span data-ttu-id="81f4e-114">Videotranscodierung</span><span class="sxs-lookup"><span data-stu-id="81f4e-114">Video transcoding</span></span>
* <span data-ttu-id="81f4e-115">Bildverarbeitung, Farbkorrektur und Größenänderung</span><span class="sxs-lookup"><span data-stu-id="81f4e-115">Image processing, color correction, & resizing</span></span>

## <a name="architecture"></a><span data-ttu-id="81f4e-116">Architektur</span><span class="sxs-lookup"><span data-stu-id="81f4e-116">Architecture</span></span>

![Architekturübersicht über die Komponenten einer cloudbasierten HPC-Lösung mit Azure Batch][architecture]

<span data-ttu-id="81f4e-118">In diesem Beispielszenario wird der Workflow bei Verwendung von Azure Batch beschrieben, und die Daten fließen wie folgt:</span><span class="sxs-lookup"><span data-stu-id="81f4e-118">This sample scenario covers the workflow when using Azure Batch, the data flows as follows:</span></span>

1. <span data-ttu-id="81f4e-119">Hochladen der Eingabedateien und der Anwendungen, mit denen diese Dateien verarbeitet werden, in Ihr Azure Storage-Konto</span><span class="sxs-lookup"><span data-stu-id="81f4e-119">Upload input files and the applications to process those files to your Azure Storage account</span></span>
2. <span data-ttu-id="81f4e-120">Erstellen eines Batch-Pools mit Computeknoten in Ihrem Batch-Konto, eines Auftrags zum Ausführen der Workload im Pool und von Aufgaben im Auftrag</span><span class="sxs-lookup"><span data-stu-id="81f4e-120">Create a Batch pool of compute nodes in your Batch account, a job to run the workload on the pool, and tasks in the job.</span></span>
3. <span data-ttu-id="81f4e-121">Herunterladen von Eingabedateien und der Anwendungen in Batch</span><span class="sxs-lookup"><span data-stu-id="81f4e-121">Download input files and the applications to Batch</span></span>
4. <span data-ttu-id="81f4e-122">Überwachen der Aufgabenausführung</span><span class="sxs-lookup"><span data-stu-id="81f4e-122">Monitor task execution</span></span>
5. <span data-ttu-id="81f4e-123">Hochladen der Aufgabenausgabe</span><span class="sxs-lookup"><span data-stu-id="81f4e-123">Upload task output</span></span>
6. <span data-ttu-id="81f4e-124">Herunterladen von Ausgabedateien</span><span class="sxs-lookup"><span data-stu-id="81f4e-124">Download output files</span></span>

<span data-ttu-id="81f4e-125">Zur Vereinfachung dieses Prozesses können Sie auch die [Batch-Plug-Ins für Maya & 3ds Max][batch-plugins] verwenden.</span><span class="sxs-lookup"><span data-stu-id="81f4e-125">To simplify this process, you could also use the [Batch Plugins for Maya & 3ds Max][batch-plugins]</span></span>

### <a name="components"></a><span data-ttu-id="81f4e-126">Komponenten</span><span class="sxs-lookup"><span data-stu-id="81f4e-126">Components</span></span>

<span data-ttu-id="81f4e-127">Azure Batch basiert auf der folgenden Azure-Technologie:</span><span class="sxs-lookup"><span data-stu-id="81f4e-127">Azure Batch builds upon the following Azure technologies:</span></span>

* <span data-ttu-id="81f4e-128">[Ressourcengruppen][resource-groups] sind logische Container für Azure-Ressourcen.</span><span class="sxs-lookup"><span data-stu-id="81f4e-128">[Resource Groups][resource-groups] is a logical container for Azure resources.</span></span>
* <span data-ttu-id="81f4e-129">[Virtuelle Netzwerke][vnet] werden sowohl für den Hauptknoten als auch für die Computeressourcen verwendet.</span><span class="sxs-lookup"><span data-stu-id="81f4e-129">[Virtual Networks][vnet] are used to for both the Head Node and Compute resources</span></span>
* <span data-ttu-id="81f4e-130">[Storage][storage]-Konten werden für die Synchronisierung und Datenaufbewahrung verwendet.</span><span class="sxs-lookup"><span data-stu-id="81f4e-130">[Storage][storage] accounts are used for the synchronisation and data retention</span></span>
* <span data-ttu-id="81f4e-131">[Virtual Machine Scale Sets][vmss] (VM-Skalierungsgruppen) werden von CycleCloud für Computeressourcen genutzt.</span><span class="sxs-lookup"><span data-stu-id="81f4e-131">[Virtual Machine Scale Sets][vmss] are utilised by CycleCloud for compute resources</span></span>

## <a name="considerations"></a><span data-ttu-id="81f4e-132">Überlegungen</span><span class="sxs-lookup"><span data-stu-id="81f4e-132">Considerations</span></span>

### <a name="machine-sizes-available-for-azure-batch"></a><span data-ttu-id="81f4e-133">Verfügbare Computergrößen für Azure Batch</span><span class="sxs-lookup"><span data-stu-id="81f4e-133">Machine Sizes available for Azure Batch</span></span>
<span data-ttu-id="81f4e-134">Die meisten Renderingkunden entscheiden sich zwar für Ressourcen mit einer hohen CPU-Leistung, aber für andere Workloads mit Verwendung von VM Scale Sets werden virtuelle Computer ggf. anders ausgewählt und sind von verschiedenen Faktoren abhängig:</span><span class="sxs-lookup"><span data-stu-id="81f4e-134">While most rendering customers whill choose resources with high CPU power, other workloads using VM Scale Sets may choose VM's differently and will depend on a number of factors:</span></span>
  - <span data-ttu-id="81f4e-135">Ist die ausgeführte Anwendung speichergebunden?</span><span class="sxs-lookup"><span data-stu-id="81f4e-135">Is the Application being run memory bound?</span></span>
  - <span data-ttu-id="81f4e-136">Müssen für die Anwendung GPUs verwendet werden?</span><span class="sxs-lookup"><span data-stu-id="81f4e-136">Does the Application need to use GPU's?</span></span> 
  - <span data-ttu-id="81f4e-137">Gilt für die Auftragstypen eine hohe Parallelität, oder ist für eng gekoppelte Aufträge eine Infiniband-Konnektivität erforderlich?</span><span class="sxs-lookup"><span data-stu-id="81f4e-137">Are the job types embarassingly parallel or require Infiniband connectivity for tightly coupled jobs?</span></span>
  - <span data-ttu-id="81f4e-138">Erzwingen einer hohen E/A-Geschwindigkeit für die Speicherung auf den Computeknoten</span><span class="sxs-lookup"><span data-stu-id="81f4e-138">Require fast I/O to Storage on the Compute Nodes</span></span>

<span data-ttu-id="81f4e-139">Azure verfügt über einen weiten Bereich von VM-Größen, mit denen alle oben beschriebenen Anwendungsanforderungen erfüllt werden können. Einige gelten speziell für HPC, aber auch die kleinsten Größen können genutzt werden, um für eine effektive Rasterimplementierung zu sorgen:</span><span class="sxs-lookup"><span data-stu-id="81f4e-139">Azure has a wide range of VM sizes that can address each and every one of the above application requirements, some are specific to HPC, but even the smallest sizes can be utilised to provide an effective grid implementation:</span></span>

  - <span data-ttu-id="81f4e-140">[HPC-VM-Größen][compute-hpc] Aufgrund der CPU-Bindung des Renderns empfiehlt Microsoft normalerweise die Verwendung von Azure-VMs der H-Serie.</span><span class="sxs-lookup"><span data-stu-id="81f4e-140">[HPC VM sizes][compute-hpc] Due to the CPU bound nature of rendering, Microsoft typically suggests the Azure H-Series VM's.</span></span>  <span data-ttu-id="81f4e-141">Diese VMs sind speziell auf Highend-Computeranforderungen ausgelegt und verfügen über vCPU-Größen mit 8 oder 16 Kernen sowie DDR4-Arbeitsspeicher, temporären SSD-Speicher und Haswell E5 Intel-Technologie.</span><span class="sxs-lookup"><span data-stu-id="81f4e-141">These are built specifically available for high end computational needs, have 8 and 16 core vCPU sizes available, and feature DDR4 memory, SSD temporary storage and Haswell E5 Intel technology.</span></span>
  - <span data-ttu-id="81f4e-142">[GPU-VM-Größen][compute-gpu] GPU-optimierte VM-Größen sind für spezialisierte virtuelle Computer mit einzelnen oder mehreren NVIDIA-GPUs verfügbar.</span><span class="sxs-lookup"><span data-stu-id="81f4e-142">[GPU VM sizes][compute-gpu] GPU optimized VM sizes are specialized virtual machines available with single or multiple NVIDIA GPUs.</span></span> <span data-ttu-id="81f4e-143">Diese Größen sind für rechenintensive, grafikintensive und visualisierungsorientierte Workloads vorgesehen.</span><span class="sxs-lookup"><span data-stu-id="81f4e-143">These sizes are designed for compute-intensive, graphics-intensive, and visualization workloads.</span></span>
    - <span data-ttu-id="81f4e-144">Die Größen NC, NCv2, NCv3 und ND sind für rechen- und netzwerkintensive Anwendungen und Algorithmen wie CUDA- und OpenCL-basierte Anwendungen und Simulationen sowie für KI und Deep Learning optimiert.</span><span class="sxs-lookup"><span data-stu-id="81f4e-144">NC, NCv2, NCv3, and ND sizes are optimized for compute-intensive and network-intensive applications and algorithms, including CUDA and OpenCL-based applications and simulations, AI, and Deep Learning.</span></span> <span data-ttu-id="81f4e-145">NV-Größen sind für Remotevisualisierung, Streaming, Spiele, Codierung und VDI-Szenarien mit Frameworks wie OpenGL und DirectX optimiert und konzipiert.</span><span class="sxs-lookup"><span data-stu-id="81f4e-145">NV sizes are optimized and designed for remote visualization, streaming, gaming, encoding, and VDI scenarios utilizing frameworks such as OpenGL and DirectX.</span></span>
  - <span data-ttu-id="81f4e-146">[Arbeitsspeicheroptimierte VM-Größen][compute-memory] Wenn mehr Arbeitsspeicher erforderlich ist, ermöglichen die arbeitsspeicheroptimierten VM-Größen ein höheres Arbeitsspeicher/CPU-Verhältnis.</span><span class="sxs-lookup"><span data-stu-id="81f4e-146">[Memory optmised VM sizes][compute-memory] When more memory is required, the memory optimized VM sizes offer a higher memory-to-CPU ratio.</span></span>
  - <span data-ttu-id="81f4e-147">[Universelle VM-Größen][compute-general] Es sind auch universelle VM-Größen mit einem ausgewogenen Verhältnis von CPU zu Arbeitsspeicher verfügbar.</span><span class="sxs-lookup"><span data-stu-id="81f4e-147">[General purposes VM sizes][compute-general] General purpose VM sizes are also available and provide balanced CPU-to-memory ratio.</span></span>

### <a name="alternatives"></a><span data-ttu-id="81f4e-148">Alternativen</span><span class="sxs-lookup"><span data-stu-id="81f4e-148">Alternatives</span></span>

<span data-ttu-id="81f4e-149">Falls Sie eine bessere Kontrolle über Ihre Renderingumgebung in Azure oder eine Hybridimplementierung benötigen, kann CycleCloud-Computing die Orchestrierung eines IaaS-Rasters in der Cloud unterstützen.</span><span class="sxs-lookup"><span data-stu-id="81f4e-149">If you require more control over your rendering environment in Azure or need a hybrid implementation, then CycleCloud computing can help orchestrate an IaaS grid in the cloud.</span></span> <span data-ttu-id="81f4e-150">Indem die gleiche zugrunde liegende Azure-Technologie wie für Azure Batch verwendet wird, wird das Erstellen und Warten eines IaaS-Rasters zu einem effizienten Prozess.</span><span class="sxs-lookup"><span data-stu-id="81f4e-150">Using the same underlying Azure technologies as Azure Batch, it makes building and maintaining an IaaS grid an efficient process.</span></span> <span data-ttu-id="81f4e-151">Unter dem folgenden Link finden Sie weitere Informationen zu den Entwurfsprinzipien:</span><span class="sxs-lookup"><span data-stu-id="81f4e-151">To find out more and learn about the design principles, please use the following link:</span></span>

<span data-ttu-id="81f4e-152">Eine vollständige Übersicht über alle HPC-Lösungen, die für Sie in Azure verfügbar sind, finden Sie im Artikel [HPC-, Batch- und Big Compute-Lösungen mit Azure-VMs][hpc-alt-solutions].</span><span class="sxs-lookup"><span data-stu-id="81f4e-152">For a complete overview of all the HPC solutions that are available to you in Azure, please see the article [HPC, Batch, and Big Compute solutions using Azure VMs][hpc-alt-solutions]</span></span>

### <a name="availability"></a><span data-ttu-id="81f4e-153">Verfügbarkeit</span><span class="sxs-lookup"><span data-stu-id="81f4e-153">Availability</span></span>

<span data-ttu-id="81f4e-154">Die Überwachung der Azure Batch-Komponenten ist über unterschiedliche Dienste, Tools und APIs verfügbar.</span><span class="sxs-lookup"><span data-stu-id="81f4e-154">Monitoring of the Azure Batch components is available through a range of services, tools and APIs.</span></span> <span data-ttu-id="81f4e-155">Weitere Informationen hierzu finden Sie im Artikel [Überwachen von Batch-Lösungen][batch-monitor].</span><span class="sxs-lookup"><span data-stu-id="81f4e-155">This is discussed further in the [Monitor Batch solutions][batch-monitor] article.</span></span>

### <a name="scalability"></a><span data-ttu-id="81f4e-156">Skalierbarkeit</span><span class="sxs-lookup"><span data-stu-id="81f4e-156">Scalability</span></span>

<span data-ttu-id="81f4e-157">Pools eines Azure Batch-Kontos können entweder per manuellem Eingriff oder mit einer Formel basierend auf Azure Batch-Metriken automatisch skaliert werden.</span><span class="sxs-lookup"><span data-stu-id="81f4e-157">Pools within an Azure Batch account can either scale through manual intervention or, by using an formula based on Azure Batch metrics, be scaled automatically.</span></span> <span data-ttu-id="81f4e-158">Weitere Informationen hierzu finden Sie im Artikel [Create an automatic scaling formula for scaling nodes in a Batch pool][batch-scaling] (Erstellen einer Formel für die automatische Skalierung zum Skalieren von Knoten in einem Batch-Pool).</span><span class="sxs-lookup"><span data-stu-id="81f4e-158">For more information on this, please see the article [Create an automatic scaling formula for scaling nodes in a Batch pool][batch-scaling].</span></span>

### <a name="security"></a><span data-ttu-id="81f4e-159">Sicherheit</span><span class="sxs-lookup"><span data-stu-id="81f4e-159">Security</span></span>

<span data-ttu-id="81f4e-160">Allgemeine Informationen zur Entwicklung sicherer Lösungen finden Sie in der [Dokumentation zur Azure-Sicherheit][security].</span><span class="sxs-lookup"><span data-stu-id="81f4e-160">For general guidance on designing secure solutions, see the [Azure Security Documentation][security].</span></span>

### <a name="resiliency"></a><span data-ttu-id="81f4e-161">Resilienz</span><span class="sxs-lookup"><span data-stu-id="81f4e-161">Resiliency</span></span>

<span data-ttu-id="81f4e-162">Da in Azure Batch derzeit keine Failoverfunktion verfügbar ist, empfehlen wir die folgenden Schritte, um sicherzustellen, dass bei einem ungeplanten Ausfall für Verfügbarkeit gesorgt ist:</span><span class="sxs-lookup"><span data-stu-id="81f4e-162">While there is currently no failover capability in Azure Batch, we recommend using the following steps to ensure availbility in case of an unplanned outage:</span></span>

* <span data-ttu-id="81f4e-163">Erstellen eines Azure Batch-Kontos an einem anderen Azure-Standort mit einem anderen Speicherkonto</span><span class="sxs-lookup"><span data-stu-id="81f4e-163">Create a Azure Batch account in an alternate Azure location with an alternate Storage Account</span></span>
* <span data-ttu-id="81f4e-164">Erstellen der gleichen Knotenpools mit dem gleichen Namen und null zugeordneten Knoten</span><span class="sxs-lookup"><span data-stu-id="81f4e-164">Create the same node pools with the same name, with 0 nodes allocated</span></span>
* <span data-ttu-id="81f4e-165">Sicherstellen, dass Anwendungen erstellt und auf das alternative Speicherkonto aktualisiert werden</span><span class="sxs-lookup"><span data-stu-id="81f4e-165">Ensure Applications are created and updated to the alternate Storage Account</span></span>
* <span data-ttu-id="81f4e-166">Hochladen von Eingabedateien und Übermitteln von Aufträgen an das andere Azure Batch-Konto</span><span class="sxs-lookup"><span data-stu-id="81f4e-166">Upload input files and submit jobs to the alternate Azure Batch account</span></span>

## <a name="deploy-this-scenario"></a><span data-ttu-id="81f4e-167">Bereitstellen dieses Szenarios</span><span class="sxs-lookup"><span data-stu-id="81f4e-167">Deploy this scenario</span></span>

### <a name="creating-an-azure-batch-account-and-pools-manually"></a><span data-ttu-id="81f4e-168">Manuelles Erstellen eines Azure Batch-Kontos und der Pools</span><span class="sxs-lookup"><span data-stu-id="81f4e-168">Creating an Azure Batch account and pools manually</span></span>

<span data-ttu-id="81f4e-169">In diesem Beispielszenario wird veranschaulicht, wie Azure Batch funktioniert, und Azure Batch Labs wird als SaaS-Beispiellösung verwendet, die für Ihre eigenen Kunden entwickelt werden kann:</span><span class="sxs-lookup"><span data-stu-id="81f4e-169">This sample scenario will provide help in learning how Azure Batch works while showcasing Azure Batch Labs as an example SaaS solution that can be developed for your own customers:</span></span>

<span data-ttu-id="81f4e-170">[Azure Batch – Masterclass][batch-labs-masterclass]</span><span class="sxs-lookup"><span data-stu-id="81f4e-170">[Azure Batch Masterclass][batch-labs-masterclass]</span></span>

### <a name="deploying-the-sample-scenario-using-an-azure-resource-manager-arm-template"></a><span data-ttu-id="81f4e-171">Bereitstellen des Beispielszenarios mit einer ARM-Vorlage (Azure Resource Manager)</span><span class="sxs-lookup"><span data-stu-id="81f4e-171">Deploying the sample scenario using an Azure Resource Manager (ARM) template</span></span>

<span data-ttu-id="81f4e-172">Über die Vorlage wird Folgendes bereitgestellt:</span><span class="sxs-lookup"><span data-stu-id="81f4e-172">The template will deploy:</span></span>
  - <span data-ttu-id="81f4e-173">Ein neues Azure Batch-Konto</span><span class="sxs-lookup"><span data-stu-id="81f4e-173">A new Azure Batch account</span></span>
  - <span data-ttu-id="81f4e-174">Ein Speicherkonto</span><span class="sxs-lookup"><span data-stu-id="81f4e-174">A storage account</span></span>
  - <span data-ttu-id="81f4e-175">Einen Knotenpool, der dem Batch-Konto zugeordnet ist</span><span class="sxs-lookup"><span data-stu-id="81f4e-175">A node pool associated with the batch account</span></span>
  - <span data-ttu-id="81f4e-176">Knotenpool wird für die Verwendung von A2 v2-VMs mit Canonical Ubuntu-Images konfiguriert</span><span class="sxs-lookup"><span data-stu-id="81f4e-176">The node pool will be configured to use A2 v2 VMs with Canonical Ubuntu images</span></span>
  - <span data-ttu-id="81f4e-177">Knotenpool enthält anfänglich 0 VMs, und zum Hinzufügen von VMs ist eine manuelle Skalierung erforderlich</span><span class="sxs-lookup"><span data-stu-id="81f4e-177">The node pool will contain 0 VMs initially and will require scaling manually to add VMs</span></span>

<a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2Fmspnp%2Fsolution-architectures%2Fmaster%2Fhpc%2Fbatchcreatewithpools.json" target="_blank">
    <img src="http://azuredeploy.net/deploybutton.png"/>
</a>

<span data-ttu-id="81f4e-178">[Weitere Informationen zu ARM-Vorlagen][azure-arm-templates]</span><span class="sxs-lookup"><span data-stu-id="81f4e-178">[Learn more about ARM templates][azure-arm-templates]</span></span>

## <a name="pricing"></a><span data-ttu-id="81f4e-179">Preise</span><span class="sxs-lookup"><span data-stu-id="81f4e-179">Pricing</span></span>

<span data-ttu-id="81f4e-180">Die Kosten für die Nutzung von Azure Batch richten sich nach den VM-Größen, die für die Pools verwendet werden, und nach der Dauer der Zuordnung und Ausführung. Für die Erstellung eines Azure Batch-Kontos werden keine Kosten berechnet.</span><span class="sxs-lookup"><span data-stu-id="81f4e-180">The cost of using Azure Batch will depend on the VM sizes that are used for the pools and how long these are allocated and running, there is no cost associated with an Azure Batch account creation.</span></span> <span data-ttu-id="81f4e-181">Speicher und ausgehende Daten sollten ebenfalls berücksichtigt werden, da hierfür weitere Kosten anfallen.</span><span class="sxs-lookup"><span data-stu-id="81f4e-181">Storage and data egress should also be taken into account as these will apply additional costs.</span></span>

<span data-ttu-id="81f4e-182">Hier sind Beispiele für Kosten angegeben, die für einen Auftrag anfallen können, der innerhalb von acht Stunden abgeschlossen wird, indem eine andere Zahl von Servern genutzt wird:</span><span class="sxs-lookup"><span data-stu-id="81f4e-182">The following are examples of costs that could be incurred for a job that completes in 8 hours using a different number of servers:</span></span>


- <span data-ttu-id="81f4e-183">100 VMs mit Hochleistungs-CPU: [Kostenschätzung][hpc-est-high]</span><span class="sxs-lookup"><span data-stu-id="81f4e-183">100 High Performance CPU VMs: [Cost Estimate][hpc-est-high]</span></span>

  <span data-ttu-id="81f4e-184">100 x H16m (16 Kerne, 225 GB RAM, Storage Premium 512 GB), 2 TB Blobspeicher, 1 TB ausgehender Datenverkehr</span><span class="sxs-lookup"><span data-stu-id="81f4e-184">100 x H16m (16 cores, 225GB RAM, Premium Storage 512GB), 2 TB Blob Storage, 1 TB egress</span></span>

- <span data-ttu-id="81f4e-185">50 VMs mit Hochleistungs-CPU: [Kostenschätzung][hpc-est-med]</span><span class="sxs-lookup"><span data-stu-id="81f4e-185">50 High Performance CPU VMs: [Cost Estimate][hpc-est-med]</span></span>

  <span data-ttu-id="81f4e-186">50 x H16m (16 Kerne, 225 GB RAM, Storage Premium 512 GB), 2 TB Blobspeicher, 1 TB ausgehender Datenverkehr</span><span class="sxs-lookup"><span data-stu-id="81f4e-186">50 x H16m (16 cores, 225GB RAM, Premium Storage 512GB), 2 TB Blob Storage, 1 TB egress</span></span>

- <span data-ttu-id="81f4e-187">10 VMs mit Hochleistungs-CPU: [Kostenschätzung][hpc-est-low]</span><span class="sxs-lookup"><span data-stu-id="81f4e-187">10 High Performance CPU VMs: [Cost Estimate][hpc-est-low]</span></span>
  
  <span data-ttu-id="81f4e-188">10 x H16m (16 Kerne, 225 GB RAM, Storage Premium 512 GB), 2 TB Blobspeicher, 1 TB ausgehender Datenverkehr</span><span class="sxs-lookup"><span data-stu-id="81f4e-188">10 x H16m (16 cores, 225GB RAM, Premium Storage 512GB), 2 TB Blob Storage, 1 TB egress</span></span>

### <a name="low-priority-vm-pricing"></a><span data-ttu-id="81f4e-189">Preise für VMs mit niedriger Priorität</span><span class="sxs-lookup"><span data-stu-id="81f4e-189">Low Priority VM Pricing</span></span>

<span data-ttu-id="81f4e-190">Azure Batch unterstützt auch die Verwendung von VMs mit niedriger Priorität\* in den Knotenpools, und dies kann unter Umständen zu erheblichen Kosteneinsparungen führen.</span><span class="sxs-lookup"><span data-stu-id="81f4e-190">Azure Batch also supports the use of Low Priority VMs\* in the node pools, which can potentially provide a substantial cost saving.</span></span> <span data-ttu-id="81f4e-191">Einen Preisvergleich zwischen Standard-VMs und VMs mit niedriger Priorität sowie weitere Informationen zu VMs mit niedriger Priorität finden Sie unter [Batch – Preise][batch-pricing].</span><span class="sxs-lookup"><span data-stu-id="81f4e-191">For a price comparison between standard VMs and Low Priority VMs, and to find out more about Low Priority VMs, please see [Batch Pricing][batch-pricing].</span></span>

<span data-ttu-id="81f4e-192">\* Beachten Sie, dass nur bestimmte Anwendungen und Workloads für die Ausführung auf VMs mit niedriger Priorität geeignet sind.</span><span class="sxs-lookup"><span data-stu-id="81f4e-192">\* Please note that only certain applications and workloads will be suitable to run on Low Priority VMs.</span></span>

## <a name="related-resources"></a><span data-ttu-id="81f4e-193">Zugehörige Ressourcen</span><span class="sxs-lookup"><span data-stu-id="81f4e-193">Related Resources</span></span>

<span data-ttu-id="81f4e-194">[Azure Batch – Übersicht][batch-overview]</span><span class="sxs-lookup"><span data-stu-id="81f4e-194">[Azure Batch Overview][batch-overview]</span></span>

<span data-ttu-id="81f4e-195">[Azure Batch-Dokumentation][batch-doc]</span><span class="sxs-lookup"><span data-stu-id="81f4e-195">[Azure Batch Documentation][batch-doc]</span></span>

<span data-ttu-id="81f4e-196">[Verwenden von Containern in Azure Batch][batch-containers]</span><span class="sxs-lookup"><span data-stu-id="81f4e-196">[Using containers on Azure Batch][batch-containers]</span></span>

<!-- links -->
[architecture]: ./media/native-hpc-ref-arch.png
[resource-groups]: /azure/azure-resource-manager/resource-group-overview
[security]: /azure/security/
[resiliency]: /azure/architecture/resiliency/
[scalability]: /azure/architecture/checklist/scalability
[vmss]: /azure/virtual-machine-scale-sets/overview
[vnet]: /azure/virtual-network/virtual-networks-overview
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
[batch-pricing]: https://azure.microsoft.com/en-gb/pricing/details/batch/
[batch-doc]: /azure/batch/
[batch-overview]: https://azure.microsoft.com/services/batch/
[batch-containers]: https://github.com/Azure/batch-shipyard
[azure-arm-templates]: /azure/azure-resource-manager/resource-group-overview#template-deployment
[batch-plugins]: /azure/batch/batch-rendering-service#options-for-submitting-a-render-job