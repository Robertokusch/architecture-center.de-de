---
title: Ausführen von CFD-Simulationen
titleSuffix: Azure Example Scenarios
description: Führen Sie Simulationen numerischer Strömungsmechaniken (Computational Fluid Dynamics, CFD) in Azure aus.
author: mikewarr
ms.date: 09/20/2018
ms.topic: example-scenario
ms.service: architecture-center
ms.subservice: example-scenario
ms.custom: fasttrack
social_image_url: /azure/architecture/example-scenario/infrastructure/media/architecture-hpc-cfd.png
ms.openlocfilehash: 6972b701a608351104c23459bc2c38029a5c8d3d
ms.sourcegitcommit: 3b15d65e7c35a19506e562c444343f8467b6a073
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/25/2019
ms.locfileid: "54908421"
---
# <a name="running-computational-fluid-dynamics-cfd-simulations-on-azure"></a><span data-ttu-id="9112f-103">Ausführen von Simulationen numerischer Strömungsmechaniken (Computational Fluid Dynamics, CFD) in Azure</span><span class="sxs-lookup"><span data-stu-id="9112f-103">Running computational fluid dynamics (CFD) simulations on Azure</span></span>

<span data-ttu-id="9112f-104">Simulationen numerischer Strömungsmechaniken (Computational Fluid Dynamics, CFD) erfordern erhebliche Computezeit und spezielle Hardware.</span><span class="sxs-lookup"><span data-stu-id="9112f-104">Computational Fluid Dynamics (CFD) simulations require significant compute time along with specialized hardware.</span></span> <span data-ttu-id="9112f-105">Mit zunehmender Clusterverwendung nehmen auch der Zeitaufwand für Simulationen und die Nutzung von Rastern insgesamt zu, was wiederum zu Problemen mit der begrenzten Kapazität und langen Warteschlangenzeiten führt.</span><span class="sxs-lookup"><span data-stu-id="9112f-105">As cluster usage increases, simulation times and overall grid use grow, leading to issues with spare capacity and long queue times.</span></span> <span data-ttu-id="9112f-106">Das Hinzufügen von physischer Hardware kann kostspielig und nicht für die schwankende Nutzung mit variierenden Spitzen- und Tiefstwerten geeignet sein, die bei Unternehmen üblich ist.</span><span class="sxs-lookup"><span data-stu-id="9112f-106">Adding physical hardware can be expensive, and may not align to the usage peaks and valleys that a business goes through.</span></span> <span data-ttu-id="9112f-107">Mit Azure können viele dieser Herausforderungen ohne jeglichen Investitionsaufwand gemeistert werden.</span><span class="sxs-lookup"><span data-stu-id="9112f-107">By taking advantage of Azure, many of these challenges can be overcome with no capital expenditure.</span></span>

<span data-ttu-id="9112f-108">Azure bietet die Hardware, die Sie zum Ausführen Ihrer CFD-Aufträge auf GPU- und CPU-VMs benötigen.</span><span class="sxs-lookup"><span data-stu-id="9112f-108">Azure provides the hardware you need to run your CFD jobs on both GPU and CPU virtual machines.</span></span> <span data-ttu-id="9112f-109">VM-Größen, die den Remotezugriff auf den direkten Speicher (Remote Direct Memory Access, RDMA) unterstützen, verfügen über FDR InfiniBand-basierte Netzwerke, die eine MPI-Kommunikation (Message Passing Interface) mit geringer Wartezeit ermöglichen.</span><span class="sxs-lookup"><span data-stu-id="9112f-109">RDMA (Remote Direct Memory Access) enabled VM sizes have FDR InfiniBand-based networking which allows for low latency MPI (Message Passing Interface) communication.</span></span> <span data-ttu-id="9112f-110">In Kombination mit Avere vFXT, einem gruppierten Dateisystem für Unternehmen, können Kunden einen maximalen Durchsatz für Lesevorgänge in Azure sicherstellen.</span><span class="sxs-lookup"><span data-stu-id="9112f-110">Combined with the Avere vFXT, which provides an enterprise-scale clustered file system, customers can ensure maximum throughput for read operations in Azure.</span></span>

<span data-ttu-id="9112f-111">Um die Erstellung, Verwaltung und Optimierung von HPC-Clustern (High Performance Computing) zu vereinfachen, kann Azure CycleCloud zum Bereitstellen von Clustern und Orchestrieren von Daten in Hybrid- und Cloudszenarien verwendet werden.</span><span class="sxs-lookup"><span data-stu-id="9112f-111">To simplify the creation, management, and optimization of HPC clusters, Azure CycleCloud can be used to provision clusters and orchestrate data in both hybrid and cloud scenarios.</span></span> <span data-ttu-id="9112f-112">CycleCloud überwacht ausstehende Aufträge und startet automatisch bedarfsbasierte Computeressourcen mit nutzungsabhängiger Bezahlung, die mit dem Workloadplaner Ihrer Wahl verbunden sind.</span><span class="sxs-lookup"><span data-stu-id="9112f-112">By monitoring the pending jobs, CycleCloud will automatically launch on-demand compute, where you only pay for what you use, connected to the workload scheduler of your choice.</span></span>

## <a name="relevant-use-cases"></a><span data-ttu-id="9112f-113">Relevante Anwendungsfälle</span><span class="sxs-lookup"><span data-stu-id="9112f-113">Relevant use cases</span></span>

<span data-ttu-id="9112f-114">Weitere relevante Branchen für CFD-Anwendungen sind:</span><span class="sxs-lookup"><span data-stu-id="9112f-114">Other relevant industries for CFD applications include:</span></span>

- <span data-ttu-id="9112f-115">Luft- und Raumfahrt</span><span class="sxs-lookup"><span data-stu-id="9112f-115">Aeronautics</span></span>
- <span data-ttu-id="9112f-116">Automobilbau</span><span class="sxs-lookup"><span data-stu-id="9112f-116">Automotive</span></span>
- <span data-ttu-id="9112f-117">Heizungs-, Lüftungs- und Klimaanlagen</span><span class="sxs-lookup"><span data-stu-id="9112f-117">Building HVAC</span></span>
- <span data-ttu-id="9112f-118">Öl- und Gasanlagen</span><span class="sxs-lookup"><span data-stu-id="9112f-118">Oil and gas</span></span>
- <span data-ttu-id="9112f-119">Biowissenschaften</span><span class="sxs-lookup"><span data-stu-id="9112f-119">Life sciences</span></span>

## <a name="architecture"></a><span data-ttu-id="9112f-120">Architecture</span><span class="sxs-lookup"><span data-stu-id="9112f-120">Architecture</span></span>

![Architekturdiagramm][architecture]

<span data-ttu-id="9112f-122">Das obige Diagramm zeigt eine allgemeine Übersicht über einen typischen Hybridentwurf mit Auftragsüberwachung der On-Demand-Knoten in Azure:</span><span class="sxs-lookup"><span data-stu-id="9112f-122">This diagram shows a high-level overview of a typical hybrid design providing job monitoring of the on-demand nodes in Azure:</span></span>

1. <span data-ttu-id="9112f-123">Stellen Sie eine Verbindung mit dem Azure CycleCloud-Server her, um den Cluster zu konfigurieren.</span><span class="sxs-lookup"><span data-stu-id="9112f-123">Connect to the Azure CycleCloud server to configure the cluster.</span></span>
2. <span data-ttu-id="9112f-124">Konfigurieren und erstellen Sie den Hauptknoten des Clusters mit RDMA-fähigen Computern für MPI.</span><span class="sxs-lookup"><span data-stu-id="9112f-124">Configure and create the cluster head node, using RDMA enabled machines for MPI.</span></span>
3. <span data-ttu-id="9112f-125">Fügen Sie den lokalen Hauptknoten hinzu, und konfigurieren Sie ihn.</span><span class="sxs-lookup"><span data-stu-id="9112f-125">Add and configure the on-premises head node.</span></span>
4. <span data-ttu-id="9112f-126">Wenn nicht genügend Ressourcen verfügbar sind, skaliert Azure CycleCloud Computeressourcen in Azure zentral hoch (oder herunter).</span><span class="sxs-lookup"><span data-stu-id="9112f-126">If there are insufficient resources, Azure CycleCloud will scale up (or down) compute resources in Azure.</span></span> <span data-ttu-id="9112f-127">Es kann ein Grenzwert vordefiniert werden, um eine Überlastung zu verhindern.</span><span class="sxs-lookup"><span data-stu-id="9112f-127">A predetermined limit can be defined to prevent over allocation.</span></span>
5. <span data-ttu-id="9112f-128">Aufgaben werden den Ausführungsknoten zugeordnet.</span><span class="sxs-lookup"><span data-stu-id="9112f-128">Tasks allocated to the execute nodes.</span></span>
6. <span data-ttu-id="9112f-129">Daten werden in Azure vom lokalen NFS-Server zwischengespeichert.</span><span class="sxs-lookup"><span data-stu-id="9112f-129">Data cached in Azure from on-premises NFS server.</span></span>
7. <span data-ttu-id="9112f-130">Daten werden aus Avere vFXT für den Azure-Cache eingelesen.</span><span class="sxs-lookup"><span data-stu-id="9112f-130">Data read in from the Avere vFXT for Azure cache.</span></span>
8. <span data-ttu-id="9112f-131">Die Auftrags- und Aufgabeninformationen werden mittels Relay an den Azure CycleCloud-Server weitergeleitet.</span><span class="sxs-lookup"><span data-stu-id="9112f-131">Job and task information relayed to the Azure CycleCloud server.</span></span>

### <a name="components"></a><span data-ttu-id="9112f-132">Komponenten</span><span class="sxs-lookup"><span data-stu-id="9112f-132">Components</span></span>

- <span data-ttu-id="9112f-133">[Azure CycleCloud][cyclecloud] ist ein Tool zum Erstellen, Verwalten, Ausführen und Optimieren von HPC- und Big Compute-Clustern in Azure.</span><span class="sxs-lookup"><span data-stu-id="9112f-133">[Azure CycleCloud][cyclecloud] a tool for creating, managing, operating, and optimizing HPC and Big Compute clusters in Azure.</span></span>
- <span data-ttu-id="9112f-134">[Avere vFXT in Azure][avere] wird verwendet, um ein für die Cloud konzipiertes Dateisystem für Unternehmen bereitzustellen.</span><span class="sxs-lookup"><span data-stu-id="9112f-134">[Avere vFXT on Azure][avere] is used to provide an enterprise-scale clustered file system built for the cloud.</span></span>
- <span data-ttu-id="9112f-135">[Microsoft Azure Virtual Machines][vms] (VMs) wird verwendet, um einen statischen Satz von Computeinstanzen zu erstellen.</span><span class="sxs-lookup"><span data-stu-id="9112f-135">[Azure Virtual Machines (VMs)][vms] are used to create a static set of compute instances.</span></span>
- <span data-ttu-id="9112f-136">[Microsoft Azure Virtual Machine Scale Sets][vmss] (VM-Skalierungsgruppen) stellt eine Gruppe von identischen VMs bereit, die von Azure CycleCloud zentral hoch- und herunterskaliert werden können.</span><span class="sxs-lookup"><span data-stu-id="9112f-136">[Virtual Machine Scale Sets (virtual machine scale set)][vmss] provide a group of identical VMs capable of being scaled up or down by Azure CycleCloud.</span></span>
- <span data-ttu-id="9112f-137">[Azure Storage-Konten](/azure/storage/common/storage-introduction) werden für die Synchronisierung und Datenaufbewahrung verwendet.</span><span class="sxs-lookup"><span data-stu-id="9112f-137">[Azure Storage accounts](/azure/storage/common/storage-introduction) are used for synchronization and data retention.</span></span>
- <span data-ttu-id="9112f-138">[Virtuelle Netzwerke](/azure/virtual-network/virtual-networks-overview) ermöglichen vielen Azure-Ressourcentypen (beispielsweise Azure-VMs) die sichere Kommunikation miteinander sowie mit dem Internet und lokalen Netzwerken.</span><span class="sxs-lookup"><span data-stu-id="9112f-138">[Virtual Networks](/azure/virtual-network/virtual-networks-overview) enable many types of Azure resources, such as Azure Virtual Machines (VMs), to securely communicate with each other, the internet, and on-premises networks.</span></span>

### <a name="alternatives"></a><span data-ttu-id="9112f-139">Alternativen</span><span class="sxs-lookup"><span data-stu-id="9112f-139">Alternatives</span></span>

<span data-ttu-id="9112f-140">Kunden können Azure CycleCloud auch verwenden, um ein Raster vollständig in Azure zu erstellen.</span><span class="sxs-lookup"><span data-stu-id="9112f-140">Customers can also use Azure CycleCloud to create a grid entirely in Azure.</span></span> <span data-ttu-id="9112f-141">Bei diesem Setup wird der Azure CycleCloud-Server in Ihrem Azure-Abonnement ausgeführt.</span><span class="sxs-lookup"><span data-stu-id="9112f-141">In this setup, the Azure CycleCloud server is run within your Azure subscription.</span></span>

<span data-ttu-id="9112f-142">Für einen modernen Anwendungsansatz, bei dem die Verwaltung eines Workloadplaners nicht erforderlich ist, kann [Microsoft Azure Batch][batch] hilfreich sein.</span><span class="sxs-lookup"><span data-stu-id="9112f-142">For a modern application approach where management of a workload scheduler is not needed, [Azure Batch][batch] can help.</span></span> <span data-ttu-id="9112f-143">Sie können mithilfe von Azure Batch umfangreiche auf Parallelverarbeitung ausgelegte HPC-Anwendungen effizient in der Cloud ausführen.</span><span class="sxs-lookup"><span data-stu-id="9112f-143">Azure Batch can run large-scale parallel and high-performance computing (HPC) applications efficiently in the cloud.</span></span> <span data-ttu-id="9112f-144">Azure Batch ermöglicht es Ihnen, die Azure-Computeressourcen für die parallele oder skalierte Ausführung Ihrer Anwendungen ohne manuelle Konfiguration oder Verwaltung der Infrastruktur zu definieren.</span><span class="sxs-lookup"><span data-stu-id="9112f-144">Azure Batch allows you to define the Azure compute resources to execute your applications in parallel or at scale without manually configuring or managing infrastructure.</span></span> <span data-ttu-id="9112f-145">Azure Batch plant rechenintensive Aufgaben und fügt basierend auf Ihren Anforderungen Computeressourcen dynamisch hinzu oder entfernt sie.</span><span class="sxs-lookup"><span data-stu-id="9112f-145">Azure Batch schedules compute-intensive tasks and dynamically adds and removes compute resources based on your requirements.</span></span>

### <a name="scalability-and-security"></a><span data-ttu-id="9112f-146">Skalierbarkeit und Sicherheit</span><span class="sxs-lookup"><span data-stu-id="9112f-146">Scalability, and Security</span></span>

<span data-ttu-id="9112f-147">Die Ausführungsknoten in Azure CycleCloud können manuell oder mittels automatischer Skalierung skaliert werden.</span><span class="sxs-lookup"><span data-stu-id="9112f-147">Scaling the execute nodes on Azure CycleCloud can be accomplished either manually or using autoscaling.</span></span> <span data-ttu-id="9112f-148">Weitere Informationen finden Sie unter [CycleCloud Autoscaling][cycle-scale] (Automatische Skalierung in CycleCloud).</span><span class="sxs-lookup"><span data-stu-id="9112f-148">For more information, see [CycleCloud Autoscaling][cycle-scale].</span></span>

<span data-ttu-id="9112f-149">Allgemeine Informationen zum Entwerfen sicherer Lösungen finden Sie in der [Dokumentation zur Azure-Sicherheit][security].</span><span class="sxs-lookup"><span data-stu-id="9112f-149">For general guidance on designing secure solutions, see the [Azure security documentation][security].</span></span>

## <a name="deploy-the-scenario"></a><span data-ttu-id="9112f-150">Bereitstellen des Szenarios</span><span class="sxs-lookup"><span data-stu-id="9112f-150">Deploy the scenario</span></span>

### <a name="prerequisites"></a><span data-ttu-id="9112f-151">Voraussetzungen</span><span class="sxs-lookup"><span data-stu-id="9112f-151">Prerequisites</span></span>

<span data-ttu-id="9112f-152">Führen Sie vor dem Bereitstellen der Resource Manager-Vorlage die folgenden Schritte aus:</span><span class="sxs-lookup"><span data-stu-id="9112f-152">Follow these steps before deploying the Resource Manager template:</span></span>

1. <span data-ttu-id="9112f-153">Erstellen Sie einen [Dienstprinzipal][cycle-svcprin] zum Abrufen von „appId“ (App-ID), „displayName“ (Anzeigename), „name“ (Name), „password“ (Kennwort) and „tenant“ (Mandant).</span><span class="sxs-lookup"><span data-stu-id="9112f-153">Create a [service principal][cycle-svcprin] for retrieving the appId, displayName, name, password, and tenant.</span></span>
2. <span data-ttu-id="9112f-154">Generieren Sie ein [SSH-Schlüsselpaar][cycle-ssh] für die sichere Anmeldung beim CycleCloud-Server.</span><span class="sxs-lookup"><span data-stu-id="9112f-154">Generate an [SSH key pair][cycle-ssh] to sign in securely to the CycleCloud server.</span></span>

    <!-- markdownlint-disable MD033 -->

    <a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FCycleCloudCommunity%2Fcyclecloud_arm%2Fmaster%2Fazuredeploy.json" target="_blank">
        <img src="https://azuredeploy.net/deploybutton.png"/>
    </a>

    <!-- markdownlint-enable MD033 -->

3. <span data-ttu-id="9112f-155">[Melden Sie sich beim CycleCloud-Server an][cycle-login], um einen neuen Cluster zu konfigurieren und zu erstellen.</span><span class="sxs-lookup"><span data-stu-id="9112f-155">[Log into the CycleCloud server][cycle-login] to configure and create a new cluster.</span></span>
4. <span data-ttu-id="9112f-156">[Erstellen Sie einen Cluster][cycle-create].</span><span class="sxs-lookup"><span data-stu-id="9112f-156">[Create a cluster][cycle-create].</span></span>

<span data-ttu-id="9112f-157">Der Avere-Cache ist eine optionale Lösung, die den Durchsatz der Lesevorgänge für die Anwendungsauftragsdaten deutlich erhöhen kann.</span><span class="sxs-lookup"><span data-stu-id="9112f-157">The Avere Cache is an optional solution that can drastically increase read throughput for the application job data.</span></span> <span data-ttu-id="9112f-158">Avere vFXT für Azure löst das Problem, das mit der Ausführung dieser HPC-Unternehmensanwendungen in der Cloud und der gleichzeitigen Nutzung von lokal oder in Azure Blob Storage gespeicherten Daten einhergeht.</span><span class="sxs-lookup"><span data-stu-id="9112f-158">Avere vFXT for Azure solves the problem of running these enterprise HPC applications in the cloud while leveraging data stored on-premises or in Azure Blob storage.</span></span>

<span data-ttu-id="9112f-159">Für Organisationen, die eine Hybridinfrastruktur mit lokalem Speicher und Cloud Computing planen, können HPC-Anwendungen anhand von Daten, die auf NAS-Geräten (Network Attached Storage) gespeichert sind, „Bursts“ in Azure ausführen und die erforderlichen CPUs nach Bedarf einrichten.</span><span class="sxs-lookup"><span data-stu-id="9112f-159">For organizations that are planning for a hybrid infrastructure with both on-premises storage and cloud computing, HPC applications can “burst” into Azure using data stored in NAS devices and spin up virtual CPUs as needed.</span></span> <span data-ttu-id="9112f-160">Das Dataset wird nie vollständig in die Cloud verschoben.</span><span class="sxs-lookup"><span data-stu-id="9112f-160">The data set is never moved completely into the cloud.</span></span> <span data-ttu-id="9112f-161">Die angeforderten Bytes werden während der Verarbeitung mithilfe eines Avere-Clusters vorübergehend zwischengespeichert.</span><span class="sxs-lookup"><span data-stu-id="9112f-161">The requested bytes are temporarily cached using an Avere cluster during processing.</span></span>

<span data-ttu-id="9112f-162">Folgen Sie zum Einrichten und Konfigurieren einer Avere vFXT-Installation den Anweisungen unter [Avere Setup and Configuration guide][avere] (Leitfaden für die Avere-Einrichtung und -Konfiguration).</span><span class="sxs-lookup"><span data-stu-id="9112f-162">To set up and configure an Avere vFXT installation, follow the [Avere Setup and Configuration guide][avere].</span></span>

## <a name="pricing"></a><span data-ttu-id="9112f-163">Preise</span><span class="sxs-lookup"><span data-stu-id="9112f-163">Pricing</span></span>

<span data-ttu-id="9112f-164">Die Kosten für die Ausführung einer HPC-Implementierung mit einem CycleCloud-Server sind von verschiedenen Faktoren abhängig.</span><span class="sxs-lookup"><span data-stu-id="9112f-164">The cost of running an HPC implementation using CycleCloud server will vary depending on a number of factors.</span></span> <span data-ttu-id="9112f-165">Beispielsweise wird CycleCloud basierend auf der genutzten Computezeit berechnet, wobei der Masterserver und der CycleCloud-Server in der Regel ständig zugeordnet sind und ausgeführt werden.</span><span class="sxs-lookup"><span data-stu-id="9112f-165">For example, CycleCloud is charged by the amount of compute time that is used, with the Master and CycleCloud server typically being constantly allocated and running.</span></span> <span data-ttu-id="9112f-166">Die Kosten für die Ausführungsknoten hängen davon ab, wie lange diese aktiv sind und ausgeführt werden und welche Größe verwendet wird.</span><span class="sxs-lookup"><span data-stu-id="9112f-166">The cost of running the Execute nodes will depend on how long these are up and running as well as what size is used.</span></span> <span data-ttu-id="9112f-167">Die normalen Azure-Gebühren für Speicher und Netzwerk gelten ebenfalls.</span><span class="sxs-lookup"><span data-stu-id="9112f-167">The normal Azure charges for storage and networking also apply.</span></span>

<span data-ttu-id="9112f-168">Dieses Szenario zeigt, wie CFD-Anwendungen in Azure ausgeführt werden können- Die Computer erfordern daher RDMA-Funktionen, die nur für bestimmte VM-Größen verfügbar sind.</span><span class="sxs-lookup"><span data-stu-id="9112f-168">This scenario shows how CFD applications can be run in Azure, so the machines will require RDMA functionality, which is only available on specific VM sizes.</span></span> <span data-ttu-id="9112f-169">Im Folgenden sind Beispiele für die Kosten aufgeführt, die in einem Monat für eine Skalierungsgruppe mit einem ausgehenden Datenverkehr von 1 TB, die kontinuierlich für acht Stunden pro Tag zugeordnet ist, anfallen können.</span><span class="sxs-lookup"><span data-stu-id="9112f-169">The following are examples of costs that could be incurred for a scale set that is allocated continuously for eight hours per day for one month, with data egress of 1 TB.</span></span> <span data-ttu-id="9112f-170">Enthalten sind auch die Preise für den Azure CycleCloud-Server und die Installation von Avere vFXT für Azure:</span><span class="sxs-lookup"><span data-stu-id="9112f-170">It also includes pricing for the Azure CycleCloud server and the Avere vFXT for Azure install:</span></span>

- <span data-ttu-id="9112f-171">Region: Nordeuropa</span><span class="sxs-lookup"><span data-stu-id="9112f-171">Region: North Europe</span></span>
- <span data-ttu-id="9112f-172">Azure CycleCloud-Server: 1x Standard D3 (vier CPUs, 14 GB Arbeitsspeicher, HDD Standard-Datenträger mit 32 GB)</span><span class="sxs-lookup"><span data-stu-id="9112f-172">Azure CycleCloud Server: 1 x Standard D3 (4 x CPUs, 14 GB Memory, Standard HDD 32 GB)</span></span>
- <span data-ttu-id="9112f-173">Azure CycleCloud-Masterserver: 1x Standard D12 v2 (vier CPUs, 28 GB Arbeitsspeicher, HDD Standard-Datenträger mit 32 GB)</span><span class="sxs-lookup"><span data-stu-id="9112f-173">Azure CycleCloud Master Server: 1 x Standard D12 v (4 x CPUs, 28 GB Memory, Standard HDD 32 GB)</span></span>
- <span data-ttu-id="9112f-174">Azure CycleCloud-Knotenarray: 10x Standard H16r (16 CPUs, 112 GB Arbeitsspeicher)</span><span class="sxs-lookup"><span data-stu-id="9112f-174">Azure CycleCloud Node Array: 10 x Standard H16r (16 x CPUs, 112 GB Memory)</span></span>
- <span data-ttu-id="9112f-175">Avere vFXT für Azure-Cluster: 3x D16s v3 (200 GB Betriebssystem, SSD Premium-Datenträger mit 1 TB)</span><span class="sxs-lookup"><span data-stu-id="9112f-175">Avere vFXT on Azure Cluster: 3 x D16s v3 (200 GB OS, Premium SSD 1-TB data disk)</span></span>
- <span data-ttu-id="9112f-176">Ausgehende Daten: 1 TB</span><span class="sxs-lookup"><span data-stu-id="9112f-176">Data Egress: 1 TB</span></span>

<span data-ttu-id="9112f-177">Die Preisschätzung für die oben genannte Hardware finden Sie [hier][pricing].</span><span class="sxs-lookup"><span data-stu-id="9112f-177">Review this [price estimate][pricing] for the hardware listed above.</span></span>

## <a name="next-steps"></a><span data-ttu-id="9112f-178">Nächste Schritte</span><span class="sxs-lookup"><span data-stu-id="9112f-178">Next Steps</span></span>

<span data-ttu-id="9112f-179">Nachdem Sie das Beispiel bereitgestellt haben, können Sie sich ausführlicher über [Azure CycleCloud][cyclecloud] informieren.</span><span class="sxs-lookup"><span data-stu-id="9112f-179">Once you've deployed the sample, learn more about [Azure CycleCloud][cyclecloud].</span></span>

## <a name="related-resources"></a><span data-ttu-id="9112f-180">Zugehörige Ressourcen</span><span class="sxs-lookup"><span data-stu-id="9112f-180">Related resources</span></span>

- <span data-ttu-id="9112f-181">[RDMA-fähige Instanzen][rdma]</span><span class="sxs-lookup"><span data-stu-id="9112f-181">[RDMA Capable Machine Instances][rdma]</span></span>
- <span data-ttu-id="9112f-182">[Anpassen einer VM für RDMA-fähige Instanzen][rdma-custom]</span><span class="sxs-lookup"><span data-stu-id="9112f-182">[Customizing an RDMA Instance VM][rdma-custom]</span></span>

<!-- links -->
[architecture]: ./media/architecture-hpc-cfd.png
[calculator]: https://azure.com/e/
[availability]: /azure/architecture/checklist/availability
[resource-groups]: /azure/azure-resource-manager/resource-group-overview
[resiliency]: /azure/architecture/resiliency/
[security]: /azure/security/
[scalability]: /azure/architecture/checklist/scalability
[vmss]: /azure/virtual-machine-scale-sets/overview
[cyclecloud]: /azure/cyclecloud/
[rdma]: /azure/virtual-machines/windows/sizes-hpc#rdma-capable-instances
[gpu]: /azure/virtual-machines/windows/sizes-gpu
[hpcsizes]: /azure/virtual-machines/windows/sizes-hpc
[vms]: /azure/virtual-machines/
[low-pri]: /azure/virtual-machine-scale-sets/virtual-machine-scale-sets-use-low-priority
[batch]: /azure/batch/
[avere]: https://github.com/Azure/Avere/blob/master/README.md
[cycle-prereq]: /azure/cyclecloud/quickstart-install-cyclecloud#prerequisites
[cycle-svcprin]: /azure/cyclecloud/quickstart-install-cyclecloud#service-principal
[cycle-ssh]: /azure/cyclecloud/quickstart-install-cyclecloud#ssh-keypair
[cycle-login]: /azure/cyclecloud/quickstart-install-cyclecloud#log-into-the-cyclecloud-application-server
[cycle-create]: /azure/cyclecloud/quickstart-create-and-run-cluster
[rdma]: /azure/virtual-machines/windows/sizes-hpc#rdma-capable-instances
[rdma-custom]: /azure/virtual-machines/linux/classic/rdma-cluster#customize-the-vm
[pricing]: https://azure.com/e/53030a04a2ab47a289156e2377a4247a
[cycle-scale]: /azure/cyclecloud/autoscale
