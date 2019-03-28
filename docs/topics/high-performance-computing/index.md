---
title: High Performance Computing (HPC) in Azure
description: Eine Anleitung zum Erstellen und Ausführen von HPC-Workloads in Azure
author: adamboeglin
ms.date: 2/4/2019
---
<!-- markdownlint-disable MD033 -->
<!-- markdownlint-disable MD026 -->

# <a name="high-performance-computing-hpc-on-azure"></a><span data-ttu-id="589aa-103">High Performance Computing (HPC) in Azure</span><span class="sxs-lookup"><span data-stu-id="589aa-103">High Performance Computing (HPC) on Azure</span></span>

## <a name="introduction-to-hpc"></a><span data-ttu-id="589aa-104">Einführung in HPC</span><span class="sxs-lookup"><span data-stu-id="589aa-104">Introduction to HPC</span></span>

<!-- markdownlint-disable MD034 -->

> [!VIDEO https://www.youtube.com/embed/rKURT32faJk]

<!-- markdownlint-enable MD034 -->

<span data-ttu-id="589aa-105">High Performance Computing (HPC) wird auch „Big Compute“ genannt und nutzt zahlreiche CPU- oder GPU-basierte Computer, um komplexe mathematische Aufgaben zu lösen.</span><span class="sxs-lookup"><span data-stu-id="589aa-105">High Performance Computing (HPC), also called "Big Compute", uses a large number of CPU or GPU-based computers to solve complex mathematical tasks.</span></span>

<span data-ttu-id="589aa-106">HPC wird in vielen Branchen zur Lösung besonders anspruchsvoller Aufgabenstellungen eingesetzt.</span><span class="sxs-lookup"><span data-stu-id="589aa-106">Many industries use HPC to solve some of their most difficult problems.</span></span>  <span data-ttu-id="589aa-107">Beispiele für Workloads wären etwa:</span><span class="sxs-lookup"><span data-stu-id="589aa-107">These include workloads such as:</span></span>

- <span data-ttu-id="589aa-108">Genomics</span><span class="sxs-lookup"><span data-stu-id="589aa-108">Genomics</span></span>
- <span data-ttu-id="589aa-109">Simulationen für Öl- und Gasunternehmen</span><span class="sxs-lookup"><span data-stu-id="589aa-109">Oil and gas simulations</span></span>
- <span data-ttu-id="589aa-110">Finanzen</span><span class="sxs-lookup"><span data-stu-id="589aa-110">Finance</span></span>
- <span data-ttu-id="589aa-111">Halbleiterdesign</span><span class="sxs-lookup"><span data-stu-id="589aa-111">Semiconductor design</span></span>
- <span data-ttu-id="589aa-112">Entwicklung</span><span class="sxs-lookup"><span data-stu-id="589aa-112">Engineering</span></span>
- <span data-ttu-id="589aa-113">Wettermodelle</span><span class="sxs-lookup"><span data-stu-id="589aa-113">Weather modeling</span></span>

### <a name="how-is-hpc-different-on-the-cloud"></a><span data-ttu-id="589aa-114">Was zeichnet HPC in der Cloud aus?</span><span class="sxs-lookup"><span data-stu-id="589aa-114">How is HPC different on the cloud?</span></span>

<span data-ttu-id="589aa-115">Einer der Hauptunterschiede zwischen einem lokalen HPC-System und einem HPC-System in der Cloud besteht darin, dass Ressourcen dynamisch nach Bedarf hinzugefügt und entfernt werden können.</span><span class="sxs-lookup"><span data-stu-id="589aa-115">One of the primary differences between an on-premise HPC system and one in the cloud is the ability for resources to dynamically be added and removed as they're needed.</span></span>  <span data-ttu-id="589aa-116">Dank der dynamischen Skalierung ist die Computekapazität kein Engpass mehr, und Kunden können ihre Infrastruktur optimal auf die Anforderungen ihrer jeweiligen Aufgaben abstimmen.</span><span class="sxs-lookup"><span data-stu-id="589aa-116">Dynamic scaling removes compute capacity as a bottleneck and instead allow customers to right size their infrastructure for the requirements of their jobs.</span></span>

<span data-ttu-id="589aa-117">Weitere Informationen zur dynamischen Skalierung finden Sie in den folgenden Artikeln:</span><span class="sxs-lookup"><span data-stu-id="589aa-117">The following articles provide more detail about this dynamic scaling capability.</span></span>

- [<span data-ttu-id="589aa-118">Big Compute-Architekturstil</span><span class="sxs-lookup"><span data-stu-id="589aa-118">Big Compute Architecture Style</span></span>](/azure/architecture/guide/architecture-styles/big-compute?context=/azure/architecture/topics/high-performance-computing/context/hpc-context)
- [<span data-ttu-id="589aa-119">Automatische Skalierung</span><span class="sxs-lookup"><span data-stu-id="589aa-119">Autoscaling best practices</span></span>](/azure/architecture/best-practices/auto-scaling?context=/azure/architecture/topics/high-performance-computing/context/hpc-context)

## <a name="implementation-checklist"></a><span data-ttu-id="589aa-120">Checkliste für die Implementierung</span><span class="sxs-lookup"><span data-stu-id="589aa-120">Implementation checklist</span></span>

<span data-ttu-id="589aa-121">Beschäftigen Sie sich zunächst mit folgenden Aspekten, bevor Sie Ihre eigene HPC-Lösung in Azure implementieren:</span><span class="sxs-lookup"><span data-stu-id="589aa-121">As you're looking to implement your own HPC solution on Azure, ensure you're reviewed the following topics:</span></span>

<!-- markdownlint-disable MD032 -->

> [!div class="checklist"]
> - <span data-ttu-id="589aa-122">Wählen Sie eine geeignete [Architektur](#infrastructure) für Ihre Anforderungen.</span><span class="sxs-lookup"><span data-stu-id="589aa-122">Choose the appropriate [architecture](#infrastructure) based on your requirements</span></span>
> - <span data-ttu-id="589aa-123">Informieren Sie sich darüber, welche [Computeoptionen](#compute) für Ihre Workload geeignet sind.</span><span class="sxs-lookup"><span data-stu-id="589aa-123">Know which [compute](#compute) options is right for your workload</span></span>
> - <span data-ttu-id="589aa-124">Ermitteln Sie die passende [Speicherlösung](#storage) für Ihre Anforderungen.</span><span class="sxs-lookup"><span data-stu-id="589aa-124">Identify the right [storage](#storage) solution that meets your needs</span></span>
> - <span data-ttu-id="589aa-125">Entscheiden Sie, wie Sie alle Ihre Ressourcen [verwalten](#management) möchten.</span><span class="sxs-lookup"><span data-stu-id="589aa-125">Decide how you're going to [manage](#management) all your resources</span></span>
> - <span data-ttu-id="589aa-126">Optimieren Sie Ihre [Anwendung](#hpc-applications) für die Cloud.</span><span class="sxs-lookup"><span data-stu-id="589aa-126">Optimize your [application](#hpc-applications) for the cloud</span></span>
> - <span data-ttu-id="589aa-127">[Schützen](#security) Sie Ihre Infrastruktur.</span><span class="sxs-lookup"><span data-stu-id="589aa-127">[Secure](#security) your Infrastructure</span></span>

<!-- markdownlint-enable MD032 -->

## <a name="infrastructure"></a><span data-ttu-id="589aa-128">Infrastruktur</span><span class="sxs-lookup"><span data-stu-id="589aa-128">Infrastructure</span></span>

<span data-ttu-id="589aa-129">Ein HPC-System basiert auf einer Reihe von Infrastrukturkomponenten.</span><span class="sxs-lookup"><span data-stu-id="589aa-129">There are a number of infrastructure components necessary to build an HPC system.</span></span>  <span data-ttu-id="589aa-130">Compute, Speicher und Netzwerk werden immer benötigt – ganz gleich, für welche HPC-Workloadverwaltung Sie sich entscheiden.</span><span class="sxs-lookup"><span data-stu-id="589aa-130">Compute, Storage, and Networking provide the underlying components, no matter how you choose to manage your HPC workloads.</span></span>

### <a name="example-hpc-architectures"></a><span data-ttu-id="589aa-131">HPC-Beispielarchitekturen</span><span class="sxs-lookup"><span data-stu-id="589aa-131">Example HPC architectures</span></span>

<span data-ttu-id="589aa-132">Für eine HPC-Architektur in Azure gibt es zahlreiche Gestaltungs- und Implementierungsmöglichkeiten.</span><span class="sxs-lookup"><span data-stu-id="589aa-132">There are a number of different ways to design and implement your HPC architecture on Azure.</span></span>  <span data-ttu-id="589aa-133">HPC-Anwendungen können auf Tausende von Computekernen skaliert werden, lokale Cluster erweitern oder als vollständig cloudbasierte Lösung ausgeführt werden.</span><span class="sxs-lookup"><span data-stu-id="589aa-133">HPC applications can scale to thousands of compute cores, extend on-premises clusters, or run as a 100% cloud native solution.</span></span>

<span data-ttu-id="589aa-134">In den folgenden Szenarien werden einige gängige Ansätze für HPC-Lösungen beschrieben.</span><span class="sxs-lookup"><span data-stu-id="589aa-134">The following scenarios outline a few of the common ways HPC solutions are built.</span></span>

<ul class="columns is-multiline has-margin-left-none has-margin-bottom-none has-padding-top-medium">
    <li class="column is-one-third has-padding-top-small-mobile has-padding-bottom-small">
        <a class="is-undecorated is-full-height is-block"
            href="/azure/architecture/example-scenario/apps/hpc-saas?context=/azure/architecture/topics/high-performance-computing/context/hpc-context">
            <article class="card has-outline-hover is-relative is-fullheight">
                    <figure class="image has-margin-right-none has-margin-left-none has-margin-top-none has-margin-bottom-none">
                        <img role="presentation" alt="" src="../../example-scenario/apps/media/architecture-hpc-saas.png">
                    </figure>
                <div class="card-content has-text-overflow-ellipsis">
                    <div class="has-padding-bottom-none">
                        <h3 class="is-size-4 has-margin-top-none has-margin-bottom-none has-text-primary"><span data-ttu-id="589aa-135">CAE-Dienste (Computer-Aided Engineering; computergestützte Entwicklung) in Azure</span><span class="sxs-lookup"><span data-stu-id="589aa-135">Computer-aided engineering services on Azure</span></span></h3>
                    </div>
                    <div class="is-size-7 has-margin-top-small has-line-height-reset">
                        <p><span data-ttu-id="589aa-136">Stellen Sie eine SaaS-Plattform (Software-as-a-Service) für computergestützte Entwicklung (Computer-Aided Engineering) in Azure bereit.</span><span class="sxs-lookup"><span data-stu-id="589aa-136">Provide a software-as-a-service (SaaS) platform for computer-aided engineering (CAE) on Azure.</span></span></p>
                    </div>
                </div>
            </article>
        </a>
    </li>
    <li class="column is-one-third has-padding-top-small-mobile has-padding-bottom-small">
        <a class="is-undecorated is-full-height is-block"
            href="/azure/architecture/example-scenario/infrastructure/hpc-cfd?context=/azure/architecture/topics/high-performance-computing/context/hpc-context">
            <article class="card has-outline-hover is-relative is-fullheight">
                    <figure class="image has-margin-right-none has-margin-left-none has-margin-top-none has-margin-bottom-none">
                        <img role="presentation" alt="" src="../../example-scenario/infrastructure/media/architecture-hpc-cfd.png">
                    </figure>
                <div class="card-content has-text-overflow-ellipsis">
                    <div class="has-padding-bottom-none">
                        <h3 class="is-size-4 has-margin-top-none has-margin-bottom-none has-text-primary"><span data-ttu-id="589aa-137">CFD-Simulationen (Computational Fluid Dynamics; numerische Strömungsmechanik) in Azure</span><span class="sxs-lookup"><span data-stu-id="589aa-137">Computational fluid dynamics (CFD) simulations on Azure</span></span></h3>
                    </div>
                    <div class="is-size-7 has-margin-top-small has-line-height-reset">
                        <p><span data-ttu-id="589aa-138">Führen Sie Simulationen numerischer Strömungsmechaniken (Computational Fluid Dynamics, CFD) in Azure aus.</span><span class="sxs-lookup"><span data-stu-id="589aa-138">Execute computational fluid dynamics (CFD) simulations on Azure.</span></span></p>
                    </div>
                </div>
            </article>
        </a>
    </li>
    <li class="column is-one-third has-padding-top-small-mobile has-padding-bottom-small">
        <a class="is-undecorated is-full-height is-block"
            href="/azure/architecture/example-scenario/infrastructure/video-rendering?context=/azure/architecture/topics/high-performance-computing/context/hpc-context">
            <article class="card has-outline-hover is-relative is-fullheight">
                    <figure class="image has-margin-right-none has-margin-left-none has-margin-top-none has-margin-bottom-none">
                        <img role="presentation" alt="" src="../../example-scenario/infrastructure/media/architecture-video-rendering.png">
                    </figure>
                <div class="card-content has-text-overflow-ellipsis">
                    <div class="has-padding-bottom-none">
                        <h3 class="is-size-4 has-margin-top-none has-margin-bottom-none has-text-primary"><span data-ttu-id="589aa-139">3D-Videorendering in Azure</span><span class="sxs-lookup"><span data-stu-id="589aa-139">3D video rendering on Azure</span></span></h3>
                    </div>
                    <div class="is-size-7 has-margin-top-small has-line-height-reset">
                        <p><span data-ttu-id="589aa-140">Ausführen nativer HPC-Workloads in Azure mithilfe des Azure Batch-Diensts</span><span class="sxs-lookup"><span data-stu-id="589aa-140">Run native HPC workloads in Azure using the Azure Batch service</span></span></p>
                    </div>
                </div>
            </article>
        </a>
    </li>
</ul>

### <a name="compute"></a><span data-ttu-id="589aa-141">Compute</span><span class="sxs-lookup"><span data-stu-id="589aa-141">Compute</span></span>

<span data-ttu-id="589aa-142">Azure bietet eine Reihe von Größen, die sowohl für CPU- als auch für GPU-intensive Workloads optimiert sind.</span><span class="sxs-lookup"><span data-stu-id="589aa-142">Azure offers a range of sizes that are optimized for both CPU & GPU intensive workloads.</span></span>

#### <a name="cpu-based-virtual-machines"></a><span data-ttu-id="589aa-143">CPU-basierte virtuelle Computer</span><span class="sxs-lookup"><span data-stu-id="589aa-143">CPU-based virtual machines</span></span>

- [<span data-ttu-id="589aa-144">Virtuelle Linux-Computer</span><span class="sxs-lookup"><span data-stu-id="589aa-144">Linux VMs</span></span>](/azure/virtual-machines/linux/sizes-hpc?context=/azure/architecture/topics/high-performance-computing/context/hpc-context)
- <span data-ttu-id="589aa-145">[Virtuelle Windows-Computer](/azure/virtual-machines/windows/sizes-hpc?context=/azure/architecture/topics/high-performance-computing/context/hpc-context)</span><span class="sxs-lookup"><span data-stu-id="589aa-145">[Windows VM's](/azure/virtual-machines/windows/sizes-hpc?context=/azure/architecture/topics/high-performance-computing/context/hpc-context) VMs</span></span>
  
#### <a name="gpu-enabled-virtual-machines"></a><span data-ttu-id="589aa-146">GPU-fähige virtuelle Computer</span><span class="sxs-lookup"><span data-stu-id="589aa-146">GPU-enabled virtual machines</span></span>

<span data-ttu-id="589aa-147">Virtuelle Computer der N-Serie verfügen über NVIDIA-GPUs für rechen- oder grafikintensive Anwendungen wie KI-Lernen und Visualisierung.</span><span class="sxs-lookup"><span data-stu-id="589aa-147">N-series VMs feature NVIDIA GPUs designed for compute-intensive or graphics-intensive applications including artificial intelligence (AI) learning and visualization.</span></span>

- [<span data-ttu-id="589aa-148">Virtuelle Linux-Computer</span><span class="sxs-lookup"><span data-stu-id="589aa-148">Linux VMs</span></span>](/azure/virtual-machines/linux/sizes-gpu?context=/azure/architecture/topics/high-performance-computing/context/hpc-context)
- [<span data-ttu-id="589aa-149">Virtuelle Windows-Computer</span><span class="sxs-lookup"><span data-stu-id="589aa-149">Windows VMs</span></span>](/azure/virtual-machines/windows/sizes-gpu?context=/azure/architecture/topics/high-performance-computing/context/hpc-context)

### <a name="storage"></a><span data-ttu-id="589aa-150">Storage</span><span class="sxs-lookup"><span data-stu-id="589aa-150">Storage</span></span>

<span data-ttu-id="589aa-151">Herkömmliche Clouddateisysteme sind den Anforderungen, die umfangreiche Batch- und HPC-Workloads in puncto Datenspeicherung und -zugriff haben, nicht gewachsen.</span><span class="sxs-lookup"><span data-stu-id="589aa-151">Large-scale Batch and HPC workloads have demands for data storage and access that exceed the capabilities of traditional cloud file systems.</span></span>  <span data-ttu-id="589aa-152">Es gibt eine Reihe von Lösungen, um den Geschwindigkeits- und Kapazitätsbedarf von HPC-Anwendungen in Azure zu verwalten.</span><span class="sxs-lookup"><span data-stu-id="589aa-152">There are a number of solutions to manage both the speed and capacity needs of HPC applications on Azure</span></span>

- <span data-ttu-id="589aa-153">[Avere vFXT](https://azure.microsoft.com/services/storage/avere-vfxt/) zur schnelleren und zugänglicheren Datenspeicherung für High Performance Computing im Edgebereich</span><span class="sxs-lookup"><span data-stu-id="589aa-153">[Avere vFXT](https://azure.microsoft.com/services/storage/avere-vfxt/) for faster, more accessible data storage for high-performance computing at the edge</span></span>
- [<span data-ttu-id="589aa-154">BeeGFS</span><span class="sxs-lookup"><span data-stu-id="589aa-154">BeeGFS</span></span>](https://azure.microsoft.com/resources/implement-glusterfs-on-azure/)
- [<span data-ttu-id="589aa-155">Speicheroptimierte virtuelle Computer</span><span class="sxs-lookup"><span data-stu-id="589aa-155">Storage Optimized Virtual Machines</span></span>](/azure/virtual-machines/windows/sizes-storage?context=/azure/architecture/topics/high-performance-computing/context/hpc-context)
- [<span data-ttu-id="589aa-156">Blob, Table und Queue Storage</span><span class="sxs-lookup"><span data-stu-id="589aa-156">Blob, table, and queue storage</span></span>](/azure/storage/storage-introduction?context=/azure/architecture/topics/high-performance-computing/context/hpc-context)
- [<span data-ttu-id="589aa-157">Azure-SMB-Dateispeicher</span><span class="sxs-lookup"><span data-stu-id="589aa-157">Azure SMB File storage</span></span>](/azure/storage/files/storage-files-introduction?context=/azure/architecture/topics/high-performance-computing/context/hpc-context)
- [<span data-ttu-id="589aa-158">Intel Cloud Edition Lustre</span><span class="sxs-lookup"><span data-stu-id="589aa-158">Intel Cloud Edition Lustre</span></span>](https://azuremarketplace.microsoft.com/marketplace/apps/intel.intel-cloud-edition-gs)

<span data-ttu-id="589aa-159">Weitere Informationen sowie einen Vergleich von Lustre, GlusterFS und BeeGFS in Azure finden Sie im E-Book [Parallel Files Systems on Azure](https://blogs.msdn.microsoft.com/azurecat/2018/06/11/azurecat-ebook-parallel-virtual-file-systems-on-microsoft-azure/) (Parallele Dateisysteme in Azure).</span><span class="sxs-lookup"><span data-stu-id="589aa-159">For more information comparing Lustre, GlusterFS, and BeeGFS on Azure, review the [Parallel Files Systems on Azure eBook](https://blogs.msdn.microsoft.com/azurecat/2018/06/11/azurecat-ebook-parallel-virtual-file-systems-on-microsoft-azure/)</span></span>

### <a name="networking"></a><span data-ttu-id="589aa-160">Netzwerk</span><span class="sxs-lookup"><span data-stu-id="589aa-160">Networking</span></span>

<span data-ttu-id="589aa-161">Virtuelle Computer vom Typ „H16r“, „H16mr“, „A8“ und „A9“ können beispielsweise eine Verbindung mit einem Back-End-RDMA-Netzwerk mit hohem Durchsatz herstellen.</span><span class="sxs-lookup"><span data-stu-id="589aa-161">H16r, H16mr, A8, and A9 VMs can connect to a high throughput back-end RDMA network.</span></span> <span data-ttu-id="589aa-162">Dieses Netzwerk kann die Leistung eng gekoppelter paralleler Anwendungen unter Microsoft MPI oder Intel MPI verbessern.</span><span class="sxs-lookup"><span data-stu-id="589aa-162">This network can improve the performance of tightly coupled parallel applications running under Microsoft MPI or Intel MPI.</span></span>

- [<span data-ttu-id="589aa-163">RDMA-fähige Instanzen</span><span class="sxs-lookup"><span data-stu-id="589aa-163">RDMA Capable Instances</span></span>](/azure/virtual-machines/windows/sizes-hpc?context=/azure/architecture/topics/high-performance-computing/context/hpc-context#rdma-capable-instances)
- [<span data-ttu-id="589aa-164">Virtual Network</span><span class="sxs-lookup"><span data-stu-id="589aa-164">Virtual Network</span></span>](/azure/virtual-network/virtual-networks-overview?context=/azure/architecture/topics/high-performance-computing/context/hpc-context)
- [<span data-ttu-id="589aa-165">ExpressRoute</span><span class="sxs-lookup"><span data-stu-id="589aa-165">ExpressRoute</span></span>](/azure/expressroute/expressroute-introduction?context=/azure/architecture/topics/high-performance-computing/context/hpc-context)

## <a name="management"></a><span data-ttu-id="589aa-166">Verwaltung</span><span class="sxs-lookup"><span data-stu-id="589aa-166">Management</span></span>

### <a name="do-it-yourself"></a><span data-ttu-id="589aa-167">Eigenständig</span><span class="sxs-lookup"><span data-stu-id="589aa-167">Do-it-yourself</span></span>

<span data-ttu-id="589aa-168">Ein von Grund auf neu erstelltes HPC-System in Azure bietet zwar ein hohes Maß an Flexibilität, ist aber häufig sehr wartungsintensiv.</span><span class="sxs-lookup"><span data-stu-id="589aa-168">Building an HPC system from scratch on Azure offers a significant amount of flexibility, but is often very maintenance intensive.</span></span>  

1. <span data-ttu-id="589aa-169">Richten Sie auf virtuellen Azure-Computern oder in [VM-Skalierungsgruppen](/azure/virtual-machine-scale-sets/overview?context=/azure/architecture/topics/high-performance-computing/context/hpc-context) Ihre eigene Clusterumgebung ein.</span><span class="sxs-lookup"><span data-stu-id="589aa-169">Set up your own cluster environment in Azure virtual machines or [virtual machine scale sets](/azure/virtual-machine-scale-sets/overview?context=/azure/architecture/topics/high-performance-computing/context/hpc-context).</span></span>
2. <span data-ttu-id="589aa-170">Verwenden Sie Azure Resource Manager-Vorlagen, um führende [Workload-Manager](#workload-managers), Infrastruktur und [Anwendungen](#hpc-applications) bereitzustellen.</span><span class="sxs-lookup"><span data-stu-id="589aa-170">Use Azure Resource Manager templates to deploy leading [workload managers](#workload-managers), infrastructure, and [applications](#hpc-applications).</span></span>
3. <span data-ttu-id="589aa-171">Wählen Sie [HPC- und GPU-VM-Größen](#compute) aus, die spezielle Hardware und Netzwerkverbindungen für MPI- oder GPU-Workloads enthalten.</span><span class="sxs-lookup"><span data-stu-id="589aa-171">Choose HPC and GPU [VM sizes](#compute) that include specialized hardware and network connections for MPI or GPU workloads.</span></span>
4. <span data-ttu-id="589aa-172">Fügen Sie [Hochleistungsspeicher](#storage) für Workloads mit hoher E/A-Intensität hinzu.</span><span class="sxs-lookup"><span data-stu-id="589aa-172">Add [high performance storage](#storage) for I/O-intensive workloads.</span></span>

### <a name="hybrid-and-cloud-bursting"></a><span data-ttu-id="589aa-173">Hybridlösung und Cloudbursting</span><span class="sxs-lookup"><span data-stu-id="589aa-173">Hybrid and cloud Bursting</span></span>

<span data-ttu-id="589aa-174">Falls Sie bereits über ein lokales HPC-System verfügen, das Sie mit Azure verknüpfen möchten, steht Ihnen eine Reihe von Ressourcen zur Verfügung, die Sie bei den ersten Schritten unterstützen.</span><span class="sxs-lookup"><span data-stu-id="589aa-174">If you have an existing on-premise HPC system that you'd like to connect to Azure, there are a number of resources to help get you started.</span></span>

<span data-ttu-id="589aa-175">Lesen Sie zunächst den Artikel [Auswählen einer Lösung zum Herstellen einer Verbindung zwischen einem lokalen Netzwerk und Azure](/azure/architecture/reference-architectures/hybrid-networking/?context=/azure/architecture/topics/high-performance-computing/context/hpc-context) in der Dokumentation.</span><span class="sxs-lookup"><span data-stu-id="589aa-175">First, review the [Options for connecting an on-premises network to Azure](/azure/architecture/reference-architectures/hybrid-networking/?context=/azure/architecture/topics/high-performance-computing/context/hpc-context) article in the documentation.</span></span>  <span data-ttu-id="589aa-176">Anschließend können Sie sich mit den folgenden Konnektivitätsoptionen beschäftigen:</span><span class="sxs-lookup"><span data-stu-id="589aa-176">From there, you may want information on these connectivity options:</span></span>

<ul class="columns is-multiline has-margin-left-none has-margin-bottom-none has-padding-top-medium">
    <li class="column is-one-third has-padding-top-small-mobile has-padding-bottom-small">
        <a class="is-undecorated is-full-height is-block"
            href="/azure/architecture/reference-architectures/hybrid-networking/vpn?context=/azure/architecture/topics/high-performance-computing/context/hpc-context">
            <article class="card has-outline-hover is-relative is-fullheight">
                    <figure class="image has-margin-right-none has-margin-left-none has-margin-top-none has-margin-bottom-none">
                        <img role="presentation" alt="" src="/azure/architecture/reference-architectures/hybrid-networking/images/vpn.png">
                    </figure>
                <div class="card-content has-text-overflow-ellipsis">
                    <div class="has-padding-bottom-none">
                        <h3 class="is-size-4 has-margin-top-none has-margin-bottom-none has-text-primary"><span data-ttu-id="589aa-177">Verbinden eines lokalen Netzwerks mit Azure über ein VPN-Gateway</span><span class="sxs-lookup"><span data-stu-id="589aa-177">Connect an on-premises network to Azure using a VPN gateway</span></span></h3>
                    </div>
                    <div class="is-size-7 has-margin-top-small has-line-height-reset">
                        <p><span data-ttu-id="589aa-178">Diese Referenzarchitektur zeigt, wie Sie ein lokales Netzwerk auf Azure ausdehnen, indem Sie ein VPN (virtuelles privates Netzwerk) zwischen Standorten verwenden.</span><span class="sxs-lookup"><span data-stu-id="589aa-178">This reference architecture shows how to extend an on-premises network to Azure, using a site-to-site virtual private network (VPN).</span></span></p>
                    </div>
                </div>
            </article>
        </a>
    </li>
    <li class="column is-one-third has-padding-top-small-mobile has-padding-bottom-small">
        <a class="is-undecorated is-full-height is-block"
            href="/azure/architecture/reference-architectures/hybrid-networking/expressroute?context=/azure/architecture/topics/high-performance-computing/context/hpc-context">
            <article class="card has-outline-hover is-relative is-fullheight">
                    <figure class="image has-margin-right-none has-margin-left-none has-margin-top-none has-margin-bottom-none">
                        <img role="presentation" alt="" src="/azure/architecture/reference-architectures/hybrid-networking/images/expressroute.png">
                    </figure>
                <div class="card-content has-text-overflow-ellipsis">
                    <div class="has-padding-bottom-none">
                        <h3 class="is-size-4 has-margin-top-none has-margin-bottom-none has-text-primary"><span data-ttu-id="589aa-179">Verbinden eines lokalen Netzwerks mit Azure über ExpressRoute</span><span class="sxs-lookup"><span data-stu-id="589aa-179">Connect an on-premises network to Azure using ExpressRoute</span></span></h3>
                    </div>
                    <div class="is-size-7 has-margin-top-small has-line-height-reset">
                        <p><span data-ttu-id="589aa-180">ExpressRoute-Verbindungen nutzen eine dedizierte private Verbindung über einen Drittanbieter für die Konnektivität.</span><span class="sxs-lookup"><span data-stu-id="589aa-180">ExpressRoute connections use a private, dedicated connection through a third-party connectivity provider.</span></span> <span data-ttu-id="589aa-181">Die private Verbindung erweitert Ihr lokales Netzwerk auf Azure.</span><span class="sxs-lookup"><span data-stu-id="589aa-181">The private connection extends your on-premises network into Azure.</span></span></p>
                    </div>
                </div>
            </article>
        </a>
    </li>
    <li class="column is-one-third has-padding-top-small-mobile has-padding-bottom-small">
        <a class="is-undecorated is-full-height is-block"
            href="/azure/architecture/reference-architectures/hybrid-networking/expressroute-vpn-failover?context=/azure/architecture/topics/high-performance-computing/context/hpc-context">
            <article class="card has-outline-hover is-relative is-fullheight">
                    <figure class="image has-margin-right-none has-margin-left-none has-margin-top-none has-margin-bottom-none">
                        <img role="presentation" alt="" src="/azure/architecture/reference-architectures/hybrid-networking/images/expressroute-vpn-failover.png">
                    </figure>
                <div class="card-content has-text-overflow-ellipsis">
                    <div class="has-padding-bottom-none">
                        <h3 class="is-size-4 has-margin-top-none has-margin-bottom-none has-text-primary"><span data-ttu-id="589aa-182">Verbinden eines lokalen Netzwerks mit Azure unter Verwendung von ExpressRoute mit VPN-Failover</span><span class="sxs-lookup"><span data-stu-id="589aa-182">Connect an on-premises network to Azure using ExpressRoute with VPN failover</span></span></h3>
                    </div>
                    <div class="is-size-7 has-margin-top-small has-line-height-reset">
                        <p><span data-ttu-id="589aa-183">Implementieren Sie eine hochverfügbare und sichere Site-to-Site-Netzwerkarchitektur, die ein virtuelles Azure-Netzwerk und ein lokales Netzwerk mit ExpressRoute-Verbindung und VPN-Gatewayfailover umfasst.</span><span class="sxs-lookup"><span data-stu-id="589aa-183">Implement a highly available and secure site-to-site network architecture that spans an Azure virtual network and an on-premises network connected using ExpressRoute with VPN gateway failover.</span></span></p>
                    </div>
                </div>
            </article>
        </a>
    </li>
</ul>

<span data-ttu-id="589aa-184">Nachdem Sie eine zuverlässige Netzwerkverbindung hergestellt haben, können Sie mit den Burstfunktionen Ihres bereits vorhandenen [Workload-Managers](#workload-managers) mit der bedarfsabhängigen Nutzung von Cloud Computing-Ressourcen beginnen.</span><span class="sxs-lookup"><span data-stu-id="589aa-184">Once network connectivity is securely established, you can start using cloud compute resources on-demand with the bursting capabilities of your existing [workload manager](#workload-managers).</span></span>

### <a name="marketplace-solutions"></a><span data-ttu-id="589aa-185">Marketplace-Lösungen</span><span class="sxs-lookup"><span data-stu-id="589aa-185">Marketplace solutions</span></span>

<span data-ttu-id="589aa-186">Im [Azure Marketplace](https://azuremarketplace.microsoft.com/marketplace/) wird eine Reihe von Workload-Managern angeboten.</span><span class="sxs-lookup"><span data-stu-id="589aa-186">There are a number of workload managers offered in the [Azure Marketplace](https://azuremarketplace.microsoft.com/marketplace/).</span></span>

- [<span data-ttu-id="589aa-187">CentOS-basiertes HPC von RogueWave</span><span class="sxs-lookup"><span data-stu-id="589aa-187">RogueWave CentOS-based HPC</span></span>](https://azuremarketplace.microsoft.com/marketplace/apps/RogueWave.CentOSbased73HPC?tab=Overview)
- [<span data-ttu-id="589aa-188">SUSE Linux Enterprise Server für HPC</span><span class="sxs-lookup"><span data-stu-id="589aa-188">SUSE Linux Enterprise Server for HPC</span></span>](https://azure.microsoft.com/marketplace/partners/suse/suselinuxenterpriseserver12optimizedforhighperformancecompute/)
- [<span data-ttu-id="589aa-189">TIBCO Grid Server Engine</span><span class="sxs-lookup"><span data-stu-id="589aa-189">TIBCO Grid Server Engine</span></span>](https://azuremarketplace.microsoft.com/marketplace/apps/tibco-software.gridserverlinuxengine?tab=Overview)
- [<span data-ttu-id="589aa-190">Virtueller Computer für Data Science für Linux und Windows</span><span class="sxs-lookup"><span data-stu-id="589aa-190">Azure Data Science VM for Windows and Linux</span></span>](/azure/machine-learning/data-science-virtual-machine/overview?context=/azure/architecture/topics/high-performance-computing/context/hpc-context)
- [<span data-ttu-id="589aa-191">D3View</span><span class="sxs-lookup"><span data-stu-id="589aa-191">D3View</span></span>](https://azuremarketplace.microsoft.com/marketplace/apps/xfinityinc.d3view-v5?tab=Overview)
- [<span data-ttu-id="589aa-192">UberCloud</span><span class="sxs-lookup"><span data-stu-id="589aa-192">UberCloud</span></span>](https://azure.microsoft.com/search/marketplace/?q=ubercloud)

### <a name="azure-batch"></a><span data-ttu-id="589aa-193">Azure Batch</span><span class="sxs-lookup"><span data-stu-id="589aa-193">Azure Batch</span></span>

<span data-ttu-id="589aa-194">Bei [Azure Batch](/azure/batch/batch-technical-overview?context=/azure/architecture/topics/high-performance-computing/context/hpc-context) handelt es sich um eine Plattform zur Ausführung umfangreicher paralleler und leistungsstarker Anwendungen (High Performance Computing, HPC) in der Cloud.</span><span class="sxs-lookup"><span data-stu-id="589aa-194">[Azure Batch](/azure/batch/batch-technical-overview?context=/azure/architecture/topics/high-performance-computing/context/hpc-context) is a platform service for running large-scale parallel and high-performance computing (HPC) applications efficiently in the cloud.</span></span> <span data-ttu-id="589aa-195">Azure Batch plant die Ausführung rechenintensiver Aufgaben mit einem verwalteten Pool virtueller Computer und kann Computeressourcen automatisch skalieren, um den Anforderungen Ihrer Aufträge gerecht zu werden.</span><span class="sxs-lookup"><span data-stu-id="589aa-195">Azure Batch schedules compute-intensive work to run on a managed pool of virtual machines, and can automatically scale compute resources to meet the needs of your jobs.</span></span>

<span data-ttu-id="589aa-196">SaaS-Anbieter oder Entwickler können die Batch-SDKs und -Tools verwenden, um HPC-Anwendungen oder Containerworkloads in Azure zu integrieren, Daten in Azure bereitzustellen und Pipelines für die Auftragsausführung zu erstellen.</span><span class="sxs-lookup"><span data-stu-id="589aa-196">SaaS providers or developers can use the Batch SDKs and tools to integrate HPC applications or container workloads with Azure, stage data to Azure, and build job execution pipelines.</span></span>

### <a name="azure-cyclecloud"></a><span data-ttu-id="589aa-197">Azure CycleCloud</span><span class="sxs-lookup"><span data-stu-id="589aa-197">Azure CycleCloud</span></span>

<span data-ttu-id="589aa-198">[Azure CycleCloud](/azure/cyclecloud/?context=/azure/architecture/topics/high-performance-computing/context/hpc-context) ist die einfachste Möglichkeit, HPC-Workloads mit einem beliebigen Planer (etwa Slurm, Grid Engine, HPC Pack, HTCondor, LSF, PBS Pro oder Symphony) in Azure zu verwalten.</span><span class="sxs-lookup"><span data-stu-id="589aa-198">[Azure CycleCloud](/azure/cyclecloud/?context=/azure/architecture/topics/high-performance-computing/context/hpc-context) Provides the simplest way to manage HPC workloads using any scheduler (like Slurm, Grid Engine, HPC Pack, HTCondor, LSF, PBS Pro, or Symphony), on Azure</span></span>

<span data-ttu-id="589aa-199">CycleCloud ermöglicht Folgendes:</span><span class="sxs-lookup"><span data-stu-id="589aa-199">CycleCloud allows you to:</span></span>

- <span data-ttu-id="589aa-200">Stellen Sie vollständige Cluster und andere Ressourcen bereit (einschließlich Planer, Compute-VMs, Speicher, Netzwerk und Cache).</span><span class="sxs-lookup"><span data-stu-id="589aa-200">Deploy full clusters and other resources, including scheduler, compute VMs, storage, networking, and cache</span></span>
- <span data-ttu-id="589aa-201">Orchestrieren Sie Workflows für Aufträge, Daten und die Cloud.</span><span class="sxs-lookup"><span data-stu-id="589aa-201">Orchestrate job, data, and cloud workflows</span></span>
- <span data-ttu-id="589aa-202">Geben Sie Administratoren die vollständige Kontrolle darüber, wer wann und zu welchem Preis Aufträge ausführen darf.</span><span class="sxs-lookup"><span data-stu-id="589aa-202">Give admins full control over which users can run jobs, as well as where and at what cost</span></span>
- <span data-ttu-id="589aa-203">Nutzen Sie erweiterte Richtlinien- und Governancefeatures (einschließlich Kostenkontrolle, Active Directory-Integration, Überwachung und Berichterstellung), um Cluster anzupassen und zu optimieren.</span><span class="sxs-lookup"><span data-stu-id="589aa-203">Customize and optimize clusters through advanced policy and governance features, including cost controls, Active Directory integration, monitoring, and reporting</span></span>
- <span data-ttu-id="589aa-204">Verwenden Sie Ihren aktuellen Aufgabenplaner und Ihre vertrauten Anwendungen, ohne diese anpassen zu müssen.</span><span class="sxs-lookup"><span data-stu-id="589aa-204">Use your current job scheduler and applications without modification</span></span>
- <span data-ttu-id="589aa-205">Profitieren Sie von der integrierten automatischen Skalierung sowie von bewährten Referenzarchitekturen für unterschiedlichste HPC-Workloads und Branchen.</span><span class="sxs-lookup"><span data-stu-id="589aa-205">Take advantage of built-in autoscaling and battle-tested reference architectures for a wide range of HPC workloads and industries</span></span>

### <a name="workload-managers"></a><span data-ttu-id="589aa-206">Workload-Manager</span><span class="sxs-lookup"><span data-stu-id="589aa-206">Workload managers</span></span>

<span data-ttu-id="589aa-207">Unten sind Beispiele für Cluster- und Workload-Manager angegeben, die in der Azure-Infrastruktur ausgeführt werden können.</span><span class="sxs-lookup"><span data-stu-id="589aa-207">The following are examples of cluster and workload managers that can run in Azure infrastructure.</span></span> <span data-ttu-id="589aa-208">Erstellen Sie eigenständige Cluster auf Azure-VMs, oder führen Sie aus einem lokalen Cluster einen Burst-Vorgang auf Azure-VMs durch.</span><span class="sxs-lookup"><span data-stu-id="589aa-208">Create stand-alone clusters in Azure VMs or burst to Azure VMs from an on-premises cluster.</span></span>

- [<span data-ttu-id="589aa-209">Alces Flight Compute</span><span class="sxs-lookup"><span data-stu-id="589aa-209">Alces Flight Compute</span></span>](https://azuremarketplace.microsoft.com/marketplace/apps/alces-flight-limited.alces-flight-compute-solo?tab=Overview)
- [<span data-ttu-id="589aa-210">TIBCO DataSynapse GridServer</span><span class="sxs-lookup"><span data-stu-id="589aa-210">TIBCO DataSynapse GridServer</span></span>](https://azure.microsoft.com/blog/tibco-datasynapse-comes-to-the-azure-marketplace/)
- [<span data-ttu-id="589aa-211">Bright Cluster Manager</span><span class="sxs-lookup"><span data-stu-id="589aa-211">Bright Cluster Manager</span></span>](http://www.brightcomputing.com/technology-partners/microsoft)
- [<span data-ttu-id="589aa-212">IBM Spectrum Symphony und Symphony LSF</span><span class="sxs-lookup"><span data-stu-id="589aa-212">IBM Spectrum Symphony and Symphony LSF</span></span>](https://azure.microsoft.com/blog/ibm-and-microsoft-azure-support-spectrum-symphony-and-spectrum-lsf/)
- [<span data-ttu-id="589aa-213">PBS Pro</span><span class="sxs-lookup"><span data-stu-id="589aa-213">PBS Pro</span></span>](http://pbspro.org)
- [<span data-ttu-id="589aa-214">Altair</span><span class="sxs-lookup"><span data-stu-id="589aa-214">Altair</span></span>](http://www.altair.com/)
- [<span data-ttu-id="589aa-215">Rescale</span><span class="sxs-lookup"><span data-stu-id="589aa-215">Rescale</span></span>](https://www.rescale.com/azure/)
- [<span data-ttu-id="589aa-216">Microsoft HPC Pack</span><span class="sxs-lookup"><span data-stu-id="589aa-216">Microsoft HPC Pack</span></span>](https://technet.microsoft.com/library/mt744885.aspx)
  - [<span data-ttu-id="589aa-217">HPC Pack für Windows</span><span class="sxs-lookup"><span data-stu-id="589aa-217">HPC Pack for Windows</span></span>](/azure/virtual-machines/windows/hpcpack-cluster-options?context=/azure/architecture/topics/high-performance-computing/context/hpc-context)
  - [<span data-ttu-id="589aa-218">HPC Pack für Linux</span><span class="sxs-lookup"><span data-stu-id="589aa-218">HPC Pack for Linux</span></span>](/azure/virtual-machines/linux/hpcpack-cluster-options?context=/azure/architecture/topics/high-performance-computing/context/hpc-context)

#### <a name="containers"></a><span data-ttu-id="589aa-219">Container</span><span class="sxs-lookup"><span data-stu-id="589aa-219">Containers</span></span>

<span data-ttu-id="589aa-220">Einige HPC-Workloads können auch mithilfe von Containern verwaltet werden.</span><span class="sxs-lookup"><span data-stu-id="589aa-220">Containers can also be used to manage some HPC workloads.</span></span>  <span data-ttu-id="589aa-221">Mit Diensten wie Azure Kubernetes Service (AKS) können Sie ganz einfach einen verwalteten Kubernetes-Cluster in Azure bereitstellen.</span><span class="sxs-lookup"><span data-stu-id="589aa-221">Services like the Azure Kubernetes Service (AKS) makes it simple to deploy a managed Kubernetes cluster in Azure.</span></span>

- [<span data-ttu-id="589aa-222">Azure Kubernetes Service (AKS)</span><span class="sxs-lookup"><span data-stu-id="589aa-222">Azure Kubernetes Service (AKS)</span></span>](/azure/aks/intro-kubernetes?context=/azure/architecture/topics/high-performance-computing/context/hpc-context)
- [<span data-ttu-id="589aa-223">Containerregistrierung</span><span class="sxs-lookup"><span data-stu-id="589aa-223">Container Registry</span></span>](/azure/container-registry/container-registry-intro?context=/azure/architecture/topics/high-performance-computing/context/hpc-context)

## <a name="cost-management"></a><span data-ttu-id="589aa-224">Kostenverwaltung</span><span class="sxs-lookup"><span data-stu-id="589aa-224">Cost management</span></span>

<span data-ttu-id="589aa-225">Für die Verwaltung Ihrer HPC-Kosten in Azure gibt es mehrere Möglichkeiten.</span><span class="sxs-lookup"><span data-stu-id="589aa-225">Managing your HPC cost on Azure can be done through a few different ways.</span></span>  <span data-ttu-id="589aa-226">Ermitteln Sie anhand der [Azure-Kaufoptionen](https://azure.microsoft.com/pricing/purchase-options/) die Methode, die am besten für Ihre Organisation geeignet ist.</span><span class="sxs-lookup"><span data-stu-id="589aa-226">Ensure you've reviewed the [Azure purchasing options](https://azure.microsoft.com/pricing/purchase-options/) to find the method that works best for your organization.</span></span>

<span data-ttu-id="589aa-227">Mithilfe von [VMs mit niedriger Priorität](/azure/virtual-machine-scale-sets/virtual-machine-scale-sets-use-low-priority?context=/azure/architecture/topics/high-performance-computing/context/hpc-context) können Sie unsere ungenutzte Kapazität mit signifikanten Kosteneinsparungen nutzen.</span><span class="sxs-lookup"><span data-stu-id="589aa-227">[Low priority VMs](/azure/virtual-machine-scale-sets/virtual-machine-scale-sets-use-low-priority?context=/azure/architecture/topics/high-performance-computing/context/hpc-context) allow you to take advantage of our unutilized capacity at a significant cost savings.</span></span>

## <a name="security"></a><span data-ttu-id="589aa-228">Sicherheit</span><span class="sxs-lookup"><span data-stu-id="589aa-228">Security</span></span>

<span data-ttu-id="589aa-229">Eine Übersicht über bewährte Sicherheitsmethoden in Azure finden Sie in der [Dokumentation zur Azure-Sicherheit](/azure/security/azure-security?context=/azure/architecture/topics/high-performance-computing/context/hpc-context).</span><span class="sxs-lookup"><span data-stu-id="589aa-229">For an overview of security best practices on Azure, review the [Azure Security Documentation](/azure/security/azure-security?context=/azure/architecture/topics/high-performance-computing/context/hpc-context).</span></span>  

<span data-ttu-id="589aa-230">Neben den im [Cloudbursting-Abschnitt](#hybrid-and-cloud-bursting) verfügbaren Netzwerkkonfigurationen können Sie bei Bedarf auch eine Hub-Spoke-Konfiguration implementieren, um Ihre Computeressourcen zu isolieren:</span><span class="sxs-lookup"><span data-stu-id="589aa-230">In addition to the network configurations available in the [Cloud Bursting](#hybrid-and-cloud-bursting) section, you may want to implement a hub/spoke configuration to isolate your compute resources:</span></span>

<ul class="columns is-multiline has-margin-left-none has-margin-bottom-none has-padding-top-medium">
    <li class="column is-one-third has-padding-top-small-mobile has-padding-bottom-small">
        <a class="is-undecorated is-full-height is-block"
            href="/azure/architecture/reference-architectures/hybrid-networking/hub-spoke?context=/azure/architecture/topics/high-performance-computing/context/hpc-context">
            <article class="card has-outline-hover is-relative is-fullheight">
                    <figure class="image has-margin-right-none has-margin-left-none has-margin-top-none has-margin-bottom-none">
                        <img role="presentation" alt="" src="/azure/architecture/reference-architectures/hybrid-networking/images/hub-spoke.png">
                    </figure>
                <div class="card-content has-text-overflow-ellipsis">
                    <div class="has-padding-bottom-none">
                        <h3 class="is-size-4 has-margin-top-none has-margin-bottom-none has-text-primary"><span data-ttu-id="589aa-231">Implementieren einer Hub-Spoke-Netzwerktopologie in Azure</span><span class="sxs-lookup"><span data-stu-id="589aa-231">Implement a hub-spoke network topology in Azure</span></span></h3>
                    </div>
                    <div class="is-size-7 has-margin-top-small has-line-height-reset">
                        <p><span data-ttu-id="589aa-232">Bei einem Hub handelt es sich um ein virtuelles Netzwerk (VNET) in Azure, das als zentraler Konnektivitätspunkt für Ihr lokales Netzwerk fungiert.</span><span class="sxs-lookup"><span data-stu-id="589aa-232">The hub is a virtual network (VNet) in Azure that acts as a central point of connectivity to your on-premises network.</span></span> <span data-ttu-id="589aa-233">Bei Speichen handelt es sich um VNETs, die eine Peerverbindung mit dem Hub herstellen und zur Isolierung von Workloads verwendet werden können.</span><span class="sxs-lookup"><span data-stu-id="589aa-233">The spokes are VNets that peer with the hub, and can be used to isolate workloads.</span></span></p>
                    </div>
                </div>
            </article>
        </a>
    </li>
    <li class="column is-one-third has-padding-top-small-mobile has-padding-bottom-small">
        <a class="is-undecorated is-full-height is-block"
            href="/azure/architecture/reference-architectures/hybrid-networking/shared-services?context=/azure/architecture/topics/high-performance-computing/context/hpc-context">
            <article class="card has-outline-hover is-relative is-fullheight">
                    <figure class="image has-margin-right-none has-margin-left-none has-margin-top-none has-margin-bottom-none">
                        <img role="presentation" alt="" src="/azure/architecture/reference-architectures/hybrid-networking/images/shared-services.png">
                    </figure>
                <div class="card-content has-text-overflow-ellipsis">
                    <div class="has-padding-bottom-none">
                        <h3 class="is-size-4 has-margin-top-none has-margin-bottom-none has-text-primary"><span data-ttu-id="589aa-234">Implementieren einer Hub-Spoke-Netzwerktopologie mit gemeinsamen Diensten in Azure</span><span class="sxs-lookup"><span data-stu-id="589aa-234">Implement a hub-spoke network topology with shared services in Azure</span></span></h3>
                    </div>
                    <div class="is-size-7 has-margin-top-small has-line-height-reset">
                        <p><span data-ttu-id="589aa-235">Diese Referenzarchitektur basiert auf der Hub-Spoke-Referenzarchitektur, um gemeinsame Dienste in den Hub einzubinden, die von allen Spokes genutzt werden können.</span><span class="sxs-lookup"><span data-stu-id="589aa-235">This reference architecture builds on the hub-spoke reference architecture to include shared services in the hub that can be consumed by all spokes.</span></span></p>
                    </div>
                </div>
            </article>
        </a>
    </li>
</ul>

## <a name="hpc-applications"></a><span data-ttu-id="589aa-236">HPC-Anwendungen</span><span class="sxs-lookup"><span data-stu-id="589aa-236">HPC applications</span></span>

<span data-ttu-id="589aa-237">Führen Sie benutzerdefinierte oder kommerzielle HPC-Anwendungen in Azure aus.</span><span class="sxs-lookup"><span data-stu-id="589aa-237">Run custom or commercial HPC applications in Azure.</span></span> <span data-ttu-id="589aa-238">Für mehrere Beispiele in diesem Abschnitt wurden Benchmarks erstellt, um eine effiziente Skalierung mit zusätzlichen virtuellen Computern oder Computekernen zu ermöglichen.</span><span class="sxs-lookup"><span data-stu-id="589aa-238">Several examples in this section are benchmarked to scale efficiently with additional VMs or compute cores.</span></span> <span data-ttu-id="589aa-239">Besuchen Sie den [Azure Marketplace](https://azuremarketplace.microsoft.com/marketplace), um auf Lösungen zuzugreifen, die sofort bereitgestellt werden können.</span><span class="sxs-lookup"><span data-stu-id="589aa-239">Visit the [Azure Marketplace](https://azuremarketplace.microsoft.com/marketplace) for ready-to-deploy solutions.</span></span>

> [!NOTE]
> <span data-ttu-id="589aa-240">Informieren Sie sich bei Verwendung kommerzieller Anwendungen beim jeweiligen Hersteller über lizenzbedingte oder anderweitige Einschränkungen für die Ausführung in der Cloud.</span><span class="sxs-lookup"><span data-stu-id="589aa-240">Check with the vendor of any commercial application for licensing or other restrictions for running in the cloud.</span></span> <span data-ttu-id="589aa-241">Nicht alle Hersteller bieten ein nutzungsbasiertes Lizenzmodell an.</span><span class="sxs-lookup"><span data-stu-id="589aa-241">Not all vendors offer pay-as-you-go licensing.</span></span> <span data-ttu-id="589aa-242">Unter Umständen benötigen Sie für Ihre Lösung einen Lizenzserver in der Cloud oder eine Verbindung mit einem lokalen Lizenzserver.</span><span class="sxs-lookup"><span data-stu-id="589aa-242">You might need a licensing server in the cloud for your solution, or connect to an on-premises license server.</span></span>

### <a name="engineering-applications"></a><span data-ttu-id="589aa-243">Technische Anwendungen</span><span class="sxs-lookup"><span data-stu-id="589aa-243">Engineering applications</span></span>

- [<span data-ttu-id="589aa-244">Altair RADIOSS</span><span class="sxs-lookup"><span data-stu-id="589aa-244">Altair RADIOSS</span></span>](https://azure.microsoft.com/blog/availability-of-altair-radioss-rdma-on-microsoft-azure/)
- [<span data-ttu-id="589aa-245">ANSYS CFD</span><span class="sxs-lookup"><span data-stu-id="589aa-245">ANSYS CFD</span></span>](https://azure.microsoft.com/blog/ansys-cfd-and-microsoft-azure-perform-the-best-hpc-scalability-in-the-cloud/)
- [<span data-ttu-id="589aa-246">MATLAB Distributed Computing Server</span><span class="sxs-lookup"><span data-stu-id="589aa-246">MATLAB Distributed Computing Server</span></span>](/azure/virtual-machines/windows/matlab-mdcs-cluster?context=/azure/architecture/topics/high-performance-computing/context/hpc-context)
- [<span data-ttu-id="589aa-247">StarCCM+</span><span class="sxs-lookup"><span data-stu-id="589aa-247">StarCCM+</span></span>](https://blogs.msdn.microsoft.com/azurecat/2017/07/07/run-star-ccm-in-an-azure-hpc-cluster/)
- [<span data-ttu-id="589aa-248">OpenFOAM</span><span class="sxs-lookup"><span data-stu-id="589aa-248">OpenFOAM</span></span>](https://simulation.azure.com/casestudies/Team-182-ABB-UC-Final.pdf)

### <a name="graphics-and-rendering"></a><span data-ttu-id="589aa-249">Grafik und Rendering</span><span class="sxs-lookup"><span data-stu-id="589aa-249">Graphics and rendering</span></span>

- <span data-ttu-id="589aa-250">[Autodesk Maya, 3ds Max und Arnold](/azure/batch/batch-rendering-service?context=/azure/architecture/topics/high-performance-computing/context/hpc-context) unter Azure Batch</span><span class="sxs-lookup"><span data-stu-id="589aa-250">[Autodesk Maya, 3ds Max, and Arnold](/azure/batch/batch-rendering-service?context=/azure/architecture/topics/high-performance-computing/context/hpc-context) on Azure Batch</span></span>

### <a name="ai-and-deep-learning"></a><span data-ttu-id="589aa-251">KI und Deep Learning</span><span class="sxs-lookup"><span data-stu-id="589aa-251">AI and deep learning</span></span>

- [<span data-ttu-id="589aa-252">Microsoft Cognitive Toolkit</span><span class="sxs-lookup"><span data-stu-id="589aa-252">Microsoft Cognitive Toolkit</span></span>](/cognitive-toolkit/cntk-on-azure)
- [<span data-ttu-id="589aa-253">Virtueller Deep Learning-Computer</span><span class="sxs-lookup"><span data-stu-id="589aa-253">Deep Learning VM</span></span>](https://azuremarketplace.microsoft.com/marketplace/apps/microsoft-ads.dsvm-deep-learning)
- [<span data-ttu-id="589aa-254">Batch Shipyard-Rezepte für Deep Learning</span><span class="sxs-lookup"><span data-stu-id="589aa-254">Batch Shipyard recipes for deep learning</span></span>](https://github.com/Azure/batch-shipyard/tree/master/recipes#deeplearning)

### <a name="mpi-providers"></a><span data-ttu-id="589aa-255">MPI-Anbieter</span><span class="sxs-lookup"><span data-stu-id="589aa-255">MPI Providers</span></span>

- [<span data-ttu-id="589aa-256">Microsoft MPI</span><span class="sxs-lookup"><span data-stu-id="589aa-256">Microsoft MPI</span></span>](/message-passing-interface/microsoft-mpi)

## <a name="remote-visualization"></a><span data-ttu-id="589aa-257">Remotevisualisierung</span><span class="sxs-lookup"><span data-stu-id="589aa-257">Remote visualization</span></span>

<ul class="columns is-multiline has-margin-left-none has-margin-bottom-none has-padding-top-medium">
    <li class="column is-one-third has-padding-top-small-mobile has-padding-bottom-small">
        <a class="is-undecorated is-full-height is-block"
            href="/azure/architecture/example-scenario/infrastructure/linux-vdi-citrix?context=/azure/architecture/topics/high-performance-computing/context/hpc-context">
            <article class="card has-outline-hover is-relative is-fullheight">
                    <figure class="image has-margin-right-none has-margin-left-none has-margin-top-none has-margin-bottom-none">
                        <img role="presentation" alt="" src="../../example-scenario/infrastructure/media/azure-citrix-sample-diagram.png">
                    </figure>
                <div class="card-content has-text-overflow-ellipsis">
                    <div class="has-padding-bottom-none">
                        <h3 class="is-size-4 has-margin-top-none has-margin-bottom-none has-text-primary"><span data-ttu-id="589aa-258">Virtuelle Linux-Desktops mit Citrix</span><span class="sxs-lookup"><span data-stu-id="589aa-258">Linux virtual desktops with Citrix</span></span></h3>
                    </div>
                    <div class="is-size-7 has-margin-top-small has-line-height-reset">
                        <p><span data-ttu-id="589aa-259">Erstellen Sie mithilfe von Citrix eine VDI-Umgebung für Linux-Desktops in Azure.</span><span class="sxs-lookup"><span data-stu-id="589aa-259">Build a VDI environment for Linux Desktops using Citrix on Azure.</span></span></p>
                    </div>
                </div>
            </article>
        </a>
    </li>
</ul>

## <a name="performance-benchmarks"></a><span data-ttu-id="589aa-260">Leistungsbenchmarks</span><span class="sxs-lookup"><span data-stu-id="589aa-260">Performance Benchmarks</span></span>

- [<span data-ttu-id="589aa-261">Compute-Benchmarkergebnisse</span><span class="sxs-lookup"><span data-stu-id="589aa-261">Compute benchmarks</span></span>](/azure/virtual-machines/windows/compute-benchmark-scores?context=/azure/architecture/topics/high-performance-computing/context/hpc-context)

## <a name="customer-stories"></a><span data-ttu-id="589aa-262">Kundenstimmen</span><span class="sxs-lookup"><span data-stu-id="589aa-262">Customer stories</span></span>

<span data-ttu-id="589aa-263">Azure wird bereits von zahlreichen Kunden mit großem Erfolg für HPC-Workloads genutzt.</span><span class="sxs-lookup"><span data-stu-id="589aa-263">There are a number of customers who have seen great success by using Azure for their HPC workloads.</span></span>  <span data-ttu-id="589aa-264">Im Anschluss finden Sie einige Kundenreferenzen:</span><span class="sxs-lookup"><span data-stu-id="589aa-264">You can find a few of these customer case studies below:</span></span>

- [<span data-ttu-id="589aa-265">ANEO</span><span class="sxs-lookup"><span data-stu-id="589aa-265">ANEO</span></span>](https://customers.microsoft.com/story/it-provider-finds-highly-scalable-cloud-based-hpc-redu)
- [<span data-ttu-id="589aa-266">AXA Global P&amp;C</span><span class="sxs-lookup"><span data-stu-id="589aa-266">AXA Global P&C</span></span>](https://customers.microsoft.com/story/axa-global-p-and-c)
- [<span data-ttu-id="589aa-267">Axioma</span><span class="sxs-lookup"><span data-stu-id="589aa-267">Axioma</span></span>](https://customers.microsoft.com/story/axioma-delivers-fintechs-first-born-in-the-cloud-multi-asset-class-enterprise-risk-solution)
- [<span data-ttu-id="589aa-268">d3View</span><span class="sxs-lookup"><span data-stu-id="589aa-268">d3View</span></span>](https://customers.microsoft.com/story/big-data-solution-provider-adopts-new-cloud-gains-thou)
- [<span data-ttu-id="589aa-269">EFS</span><span class="sxs-lookup"><span data-stu-id="589aa-269">EFS</span></span>](https://customers.microsoft.com/story/efs-professionalservices-azure)
- [<span data-ttu-id="589aa-270">Hymans Robertson</span><span class="sxs-lookup"><span data-stu-id="589aa-270">Hymans Robertson</span></span>](https://customers.microsoft.com/story/hymans-robertson)
- [<span data-ttu-id="589aa-271">MetLife</span><span class="sxs-lookup"><span data-stu-id="589aa-271">MetLife</span></span>](https://enterprise.microsoft.com/customer-story/industries/insurance/metlife/)
- [<span data-ttu-id="589aa-272">Microsoft Research</span><span class="sxs-lookup"><span data-stu-id="589aa-272">Microsoft Research</span></span>](https://customers.microsoft.com/doclink/fast-lmm-and-windows-azure-put-genetics-research-on-fa)
- [<span data-ttu-id="589aa-273">Milliman</span><span class="sxs-lookup"><span data-stu-id="589aa-273">Milliman</span></span>](https://customers.microsoft.com/story/actuarial-firm-works-to-transform-insurance-industry-w)
- [<span data-ttu-id="589aa-274">Mitsubishi UFJ Securities International</span><span class="sxs-lookup"><span data-stu-id="589aa-274">Mitsubishi UFJ Securities International</span></span>](https://customers.microsoft.com/story/powering-risk-compute-grids-in-the-cloud)
- [<span data-ttu-id="589aa-275">NeuroInitiative</span><span class="sxs-lookup"><span data-stu-id="589aa-275">NeuroInitiative</span></span>](https://customers.microsoft.com/story/neuroinitiative-health-provider-azure)
- [<span data-ttu-id="589aa-276">Schlumberger</span><span class="sxs-lookup"><span data-stu-id="589aa-276">Schlumberger</span></span>](https://azure.microsoft.com/blog/big-compute-for-large-engineering-simulations)
- [<span data-ttu-id="589aa-277">Towers Watson</span><span class="sxs-lookup"><span data-stu-id="589aa-277">Towers Watson</span></span>](https://customers.microsoft.com/story/insurance-tech-provider-delivers-disruptive-solutions)

## <a name="other-important-information"></a><span data-ttu-id="589aa-278">Weitere wichtige Informationen</span><span class="sxs-lookup"><span data-stu-id="589aa-278">Other important information</span></span>

- <span data-ttu-id="589aa-279">Achten Sie vor dem Ausführen umfangreicher Workloads darauf, dass Ihr [vCPU-Kontingent](/azure/virtual-machines/linux/quotas?context=/azure/architecture/topics/high-performance-computing/context/hpc-context) erhöht wurde.</span><span class="sxs-lookup"><span data-stu-id="589aa-279">Ensure your [vCPU quota](/azure/virtual-machines/linux/quotas?context=/azure/architecture/topics/high-performance-computing/context/hpc-context) has been increased before attempting to run large-scale workloads.</span></span>

## <a name="next-steps"></a><span data-ttu-id="589aa-280">Nächste Schritte</span><span class="sxs-lookup"><span data-stu-id="589aa-280">Next steps</span></span>

<span data-ttu-id="589aa-281">Die neuesten Ankündigungen finden Sie hier:</span><span class="sxs-lookup"><span data-stu-id="589aa-281">For the latest announcements, see:</span></span>

- [<span data-ttu-id="589aa-282">Blog des HPC- und Batch-Teams von Microsoft</span><span class="sxs-lookup"><span data-stu-id="589aa-282">Microsoft HPC and Batch team blog</span></span>](http://blogs.technet.com/b/windowshpc/)
- <span data-ttu-id="589aa-283">[Azure-Blog](https://azure.microsoft.com/blog/tag/hpc/)</span><span class="sxs-lookup"><span data-stu-id="589aa-283">Visit the [Azure blog](https://azure.microsoft.com/blog/tag/hpc/).</span></span>

### <a name="microsoft-batch-examples"></a><span data-ttu-id="589aa-284">Microsoft Batch-Beispiele</span><span class="sxs-lookup"><span data-stu-id="589aa-284">Microsoft Batch Examples</span></span>

<span data-ttu-id="589aa-285">Die folgenden Tutorials enthalten ausführliche Informationen zum Ausführen von Anwendungen in Microsoft-Batch:</span><span class="sxs-lookup"><span data-stu-id="589aa-285">These tutorials will provide you with details on running applications on Microsoft Batch</span></span>

- [<span data-ttu-id="589aa-286">Erste Schritte beim Entwickeln mit Batch</span><span class="sxs-lookup"><span data-stu-id="589aa-286">Get started developing with Batch</span></span>](/azure/batch/quick-run-dotnet?context=/azure/architecture/topics/high-performance-computing/context/hpc-context)
- [<span data-ttu-id="589aa-287">Verwenden von Codebeispielen für Azure Batch</span><span class="sxs-lookup"><span data-stu-id="589aa-287">Use Azure Batch code samples</span></span>](https://github.com/Azure/azure-batch-samples)
- [<span data-ttu-id="589aa-288">Verwenden von VMs mit niedriger Priorität mit Batch</span><span class="sxs-lookup"><span data-stu-id="589aa-288">Use low-priority VMs with Batch</span></span>](/azure/batch/batch-low-pri-vms?context=/azure/architecture/topics/high-performance-computing/context/hpc-context)
- [<span data-ttu-id="589aa-289">Ausführen von HPC-Workloads in Containern mit Batch Shipyard</span><span class="sxs-lookup"><span data-stu-id="589aa-289">Run containerized HPC workloads with Batch Shipyard</span></span>](https://github.com/Azure/batch-shipyard)
- [<span data-ttu-id="589aa-290">Ausführen gleichzeitiger R-Workloads in Batch</span><span class="sxs-lookup"><span data-stu-id="589aa-290">Run parallel R workloads on Batch</span></span>](https://github.com/Azure/doAzureParallel)
- [<span data-ttu-id="589aa-291">Ausführen bedarfsorientierter Spark-Aufträge in Batch</span><span class="sxs-lookup"><span data-stu-id="589aa-291">Run on-demand Spark jobs on Batch</span></span>](https://github.com/Azure/aztk)
- [<span data-ttu-id="589aa-292">Verwenden rechenintensiver virtueller Computer in Batch-Pools</span><span class="sxs-lookup"><span data-stu-id="589aa-292">Use compute-intensive VMs in Batch pools</span></span>](/azure/batch/batch-pool-compute-intensive-sizes?context=/azure/architecture/topics/high-performance-computing/context/hpc-context)