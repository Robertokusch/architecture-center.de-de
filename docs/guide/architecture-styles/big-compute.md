---
title: Big Compute-Architekturstil
titleSuffix: Azure Application Architecture Guide
description: In diesem Artikel werden die Vorteile, Herausforderungen und bewährten Methoden für Big Compute-Architekturen in Azure beschrieben.
author: MikeWasson
ms.date: 08/30/2018
ms.custom: seojan19
ms.openlocfilehash: 7dbd8e25a0db79e6dde4c1c7e787eaa040ffdb3b
ms.sourcegitcommit: 1f4cdb08fe73b1956e164ad692f792f9f635b409
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/08/2019
ms.locfileid: "54114061"
---
# <a name="big-compute-architecture-style"></a><span data-ttu-id="d188f-103">Big Compute-Architekturstil</span><span class="sxs-lookup"><span data-stu-id="d188f-103">Big compute architecture style</span></span>

<span data-ttu-id="d188f-104">Mit dem Begriff *Big Compute* werden umfangreiche Workloads beschrieben, für die eine große Anzahl von Kernen erforderlich ist (häufig Hunderte oder Tausende).</span><span class="sxs-lookup"><span data-stu-id="d188f-104">The term *big compute* describes large-scale workloads that require a large number of cores, often numbering in the hundreds or thousands.</span></span> <span data-ttu-id="d188f-105">Zu den Szenarien gehören unter anderem Bildrendering, Fluiddynamik, Modellierung von finanziellen Risiken, Erdölsuche, Arzneimittelentwicklung und Belastungsanalysen im Maschinenbau.</span><span class="sxs-lookup"><span data-stu-id="d188f-105">Scenarios include image rendering, fluid dynamics, financial risk modeling, oil exploration, drug design, and engineering stress analysis, among others.</span></span>

![Logisches Diagramm für die Big Compute-Architektur](./images/big-compute-logical.png)

<span data-ttu-id="d188f-107">Hier sind einige typische Merkmale von Big Compute-Anwendungen aufgeführt:</span><span class="sxs-lookup"><span data-stu-id="d188f-107">Here are some typical characteristics of big compute applications:</span></span>

- <span data-ttu-id="d188f-108">Die Arbeit kann in einzelne Aufgaben unterteilt werden, die dann gleichzeitig auf vielen Kernen ausgeführt werden können.</span><span class="sxs-lookup"><span data-stu-id="d188f-108">The work can be split into discrete tasks, which can be run across many cores simultaneously.</span></span>
- <span data-ttu-id="d188f-109">Jede Aufgabe ist endlich.</span><span class="sxs-lookup"><span data-stu-id="d188f-109">Each task is finite.</span></span> <span data-ttu-id="d188f-110">Es ist eine Eingabe erforderlich, anschließend erfolgt die Verarbeitung, und zuletzt wird die Ausgabe erstellt.</span><span class="sxs-lookup"><span data-stu-id="d188f-110">It takes some input, does some processing, and produces output.</span></span> <span data-ttu-id="d188f-111">Die gesamte Anwendung wird über einen endlichen Zeitraum ausgeführt (Minuten bis Tage).</span><span class="sxs-lookup"><span data-stu-id="d188f-111">The entire application runs for a finite amount of time (minutes to days).</span></span> <span data-ttu-id="d188f-112">Ein häufiges Muster ist die Bereitstellung einer großen Anzahl von Kernen in einem Burstvorgang und das anschließende Herunterfahren auf null, nachdem die Anwendung beendet wurde.</span><span class="sxs-lookup"><span data-stu-id="d188f-112">A common pattern is to provision a large number of cores in a burst, and then spin down to zero once the application completes.</span></span>
- <span data-ttu-id="d188f-113">Die Anwendung muss nicht rund um die Uhr aktiv sein.</span><span class="sxs-lookup"><span data-stu-id="d188f-113">The application does not need to stay up 24/7.</span></span> <span data-ttu-id="d188f-114">Das System muss aber Knotenfehler oder Anwendungsabstürze verarbeiten können.</span><span class="sxs-lookup"><span data-stu-id="d188f-114">However, the system must handle node failures or application crashes.</span></span>
- <span data-ttu-id="d188f-115">Bei einigen Anwendungen sind Aufgaben unabhängig voneinander und können parallel ausgeführt werden.</span><span class="sxs-lookup"><span data-stu-id="d188f-115">For some applications, tasks are independent and can run in parallel.</span></span> <span data-ttu-id="d188f-116">In anderen Fällen sind Aufgaben eng verknüpft und müssen ggf. interagieren oder Zwischenergebnisse austauschen.</span><span class="sxs-lookup"><span data-stu-id="d188f-116">In other cases, tasks are tightly coupled, meaning they must interact or exchange intermediate results.</span></span> <span data-ttu-id="d188f-117">Hierfür kann die Nutzung von Hochgeschwindigkeits-Netzwerktechnologie ratsam sein, z.B. InfiniBand und Remotezugriff auf den direkten Speicher (Remote Direct Memory Access, RDMA).</span><span class="sxs-lookup"><span data-stu-id="d188f-117">In that case, consider using high-speed networking technologies such as InfiniBand and remote direct memory access (RDMA).</span></span>
- <span data-ttu-id="d188f-118">Je nach Workload kann auch der Einsatz von rechenintensiven VM-Größen (H16r, H16mr und A9) ratsam sein.</span><span class="sxs-lookup"><span data-stu-id="d188f-118">Depending on your workload, you might use compute-intensive VM sizes (H16r, H16mr, and A9).</span></span>

## <a name="when-to-use-this-architecture"></a><span data-ttu-id="d188f-119">Einsatzmöglichkeiten für diese Architektur</span><span class="sxs-lookup"><span data-stu-id="d188f-119">When to use this architecture</span></span>

- <span data-ttu-id="d188f-120">Rechenintensive Vorgänge, z.B. Simulation und Zahlenverarbeitung.</span><span class="sxs-lookup"><span data-stu-id="d188f-120">Computationally intensive operations such as simulation and number crunching.</span></span>
- <span data-ttu-id="d188f-121">Simulationen, die rechenintensiv sind und auf CPUs mehrerer Computer (10 bis mehrere Tausend) aufgeteilt werden müssen.</span><span class="sxs-lookup"><span data-stu-id="d188f-121">Simulations that are computationally intensive and must be split across CPUs in multiple computers (10-1000s).</span></span>
- <span data-ttu-id="d188f-122">Simulationen, für die zu viel Arbeitsspeicher für einen Computer erforderlich ist und die auf mehrere Computer aufgeteilt werden müssen.</span><span class="sxs-lookup"><span data-stu-id="d188f-122">Simulations that require too much memory for one computer, and must be split across multiple computers.</span></span>
- <span data-ttu-id="d188f-123">Rechenvorgänge mit langer Ausführungsdauer, die auf einem einzelnen Computer zu lange dauern würden.</span><span class="sxs-lookup"><span data-stu-id="d188f-123">Long-running computations that would take too long to complete on a single computer.</span></span>
- <span data-ttu-id="d188f-124">Kleinere Rechenvorgänge, die mehrere hundert oder tausend Mal ausgeführt werden müssen, z.B. Monte Carlo-Simulationen.</span><span class="sxs-lookup"><span data-stu-id="d188f-124">Smaller computations that must be run 100s or 1000s of times, such as Monte Carlo simulations.</span></span>

## <a name="benefits"></a><span data-ttu-id="d188f-125">Vorteile</span><span class="sxs-lookup"><span data-stu-id="d188f-125">Benefits</span></span>

- <span data-ttu-id="d188f-126">Hohe Leistung mit „[hochgradig paralleler][embarrassingly-parallel]“ Verarbeitung.</span><span class="sxs-lookup"><span data-stu-id="d188f-126">High performance with "[embarrassingly parallel][embarrassingly-parallel]" processing.</span></span>
- <span data-ttu-id="d188f-127">Es können Hunderte oder Tausende von Computerprozessorkernen genutzt werden, um komplexe Probleme schneller zu lösen.</span><span class="sxs-lookup"><span data-stu-id="d188f-127">Can harness hundreds or thousands of computer cores to solve large problems faster.</span></span>
- <span data-ttu-id="d188f-128">Zugriff auf spezialisierte Hochleistungs-Hardware mit dedizierten InfiniBand-Hochgeschwindigkeitsnetzwerken.</span><span class="sxs-lookup"><span data-stu-id="d188f-128">Access to specialized high-performance hardware, with dedicated high-speed InfiniBand networks.</span></span>
- <span data-ttu-id="d188f-129">Sie können VMs je nach Bedarf für Arbeitsschritte bereitstellen und dann wieder aussondern.</span><span class="sxs-lookup"><span data-stu-id="d188f-129">You can provision VMs as needed to do work, and then tear them down.</span></span>

## <a name="challenges"></a><span data-ttu-id="d188f-130">Herausforderungen</span><span class="sxs-lookup"><span data-stu-id="d188f-130">Challenges</span></span>

- <span data-ttu-id="d188f-131">Die Verwaltung der VM-Infrastruktur.</span><span class="sxs-lookup"><span data-stu-id="d188f-131">Managing the VM infrastructure.</span></span>
- <span data-ttu-id="d188f-132">Die Verwaltung des Umfangs der Zahlenverarbeitung.</span><span class="sxs-lookup"><span data-stu-id="d188f-132">Managing the volume of number crunching</span></span>
- <span data-ttu-id="d188f-133">Die rechtzeitige Bereitstellung von Tausenden von Kernen.</span><span class="sxs-lookup"><span data-stu-id="d188f-133">Provisioning thousands of cores in a timely manner.</span></span>
- <span data-ttu-id="d188f-134">Für eng verknüpfte Aufgaben kann das Hinzufügen von mehr Kernen auch negative Auswirkungen haben.</span><span class="sxs-lookup"><span data-stu-id="d188f-134">For tightly coupled tasks, adding more cores can have diminishing returns.</span></span> <span data-ttu-id="d188f-135">Unter Umständen müssen Sie etwas experimentieren, um die optimale Anzahl von Kernen zu ermitteln.</span><span class="sxs-lookup"><span data-stu-id="d188f-135">You may need to experiment to find the optimum number of cores.</span></span>

## <a name="big-compute-using-azure-batch"></a><span data-ttu-id="d188f-136">Big Compute per Azure Batch</span><span class="sxs-lookup"><span data-stu-id="d188f-136">Big compute using Azure Batch</span></span>

<span data-ttu-id="d188f-137">[Azure Batch][batch] ist ein verwalteter Dienst zum Ausführen von umfassenden HPC-Anwendungen (High-Performance Computing).</span><span class="sxs-lookup"><span data-stu-id="d188f-137">[Azure Batch][batch] is a managed service for running large-scale high-performance computing (HPC) applications.</span></span>

<span data-ttu-id="d188f-138">Mit Azure Batch konfigurieren Sie einen VM-Pool und laden die Anwendungen und Datendateien hoch.</span><span class="sxs-lookup"><span data-stu-id="d188f-138">Using Azure Batch, you configure a VM pool, and upload the applications and data files.</span></span> <span data-ttu-id="d188f-139">Anschließend stellt der Batch-Dienst die VMs bereit, weist den VMs Aufgaben zu, führt die Aufgaben aus und überwacht den Fortschritt.</span><span class="sxs-lookup"><span data-stu-id="d188f-139">Then the Batch service provisions the VMs, assign tasks to the VMs, runs the tasks, and monitors the progress.</span></span> <span data-ttu-id="d188f-140">Batch kann die VMs als Reaktion auf die Workload automatisch horizontal hochskalieren.</span><span class="sxs-lookup"><span data-stu-id="d188f-140">Batch can automatically scale out the VMs in response to the workload.</span></span> <span data-ttu-id="d188f-141">Batch ermöglicht auch eine Auftragsplanung.</span><span class="sxs-lookup"><span data-stu-id="d188f-141">Batch also provides job scheduling.</span></span>

![Diagramm von Big Compute mit Azure Batch](./images/big-compute-batch.png)

## <a name="big-compute-running-on-virtual-machines"></a><span data-ttu-id="d188f-143">Big Compute auf virtuellen Computern</span><span class="sxs-lookup"><span data-stu-id="d188f-143">Big compute running on Virtual Machines</span></span>

<span data-ttu-id="d188f-144">Sie können [Microsoft HPC Pack][hpc-pack] verwenden, um einen Cluster mit VMs zu verwalten und HPC-Aufträge zu planen und zu überwachen.</span><span class="sxs-lookup"><span data-stu-id="d188f-144">You can use [Microsoft HPC Pack][hpc-pack] to administer a cluster of VMs, and schedule and monitor HPC jobs.</span></span> <span data-ttu-id="d188f-145">Bei diesem Ansatz müssen Sie die VMs und die Netzwerkinfrastruktur bereitstellen und verwalten.</span><span class="sxs-lookup"><span data-stu-id="d188f-145">With this approach, you must provision and manage the VMs and network infrastructure.</span></span> <span data-ttu-id="d188f-146">Erwägen Sie diesen Ansatz, wenn Sie über vorhandene HPC-Workloads verfügen und einige oder alle nach Azure verschieben möchten.</span><span class="sxs-lookup"><span data-stu-id="d188f-146">Consider this approach if you have existing HPC workloads and want to move some or all it to Azure.</span></span> <span data-ttu-id="d188f-147">Sie können den gesamten HPC-Cluster nach Azure verschieben oder Ihren HPC-Cluster weiter lokal nutzen und Azure für Burstkapazität-Zwecke verwenden.</span><span class="sxs-lookup"><span data-stu-id="d188f-147">You can move the entire HPC cluster to Azure, or keep your HPC cluster on-premises but use Azure for burst capacity.</span></span> <span data-ttu-id="d188f-148">Weitere Informationen finden Sie unter [HPC-, Batch- und Big Compute-Lösungen, mit Azure-VMs][batch-hpc-solutions].</span><span class="sxs-lookup"><span data-stu-id="d188f-148">For more information, see [Batch and HPC solutions for large-scale computing workloads][batch-hpc-solutions].</span></span>

### <a name="hpc-pack-deployed-to-azure"></a><span data-ttu-id="d188f-149">Bereitstellung von HPC Pack in Azure</span><span class="sxs-lookup"><span data-stu-id="d188f-149">HPC Pack deployed to Azure</span></span>

<span data-ttu-id="d188f-150">Bei diesem Szenario wird der HPC-Cluster vollständig in Azure erstellt.</span><span class="sxs-lookup"><span data-stu-id="d188f-150">In this scenario, the HPC cluster is created entirely within Azure.</span></span>

![Diagramm der Bereitstellung von HPC Pack in Azure](./images/big-compute-iaas.png)

<span data-ttu-id="d188f-152">Der Hauptknoten stellt Verwaltungs- und Auftragsplanungsdienste für den Cluster bereit.</span><span class="sxs-lookup"><span data-stu-id="d188f-152">The head node provides management and job scheduling services to the cluster.</span></span> <span data-ttu-id="d188f-153">Verwenden Sie für eng verknüpfte Aufgaben ein RDMA-Netzwerk, das zwischen VMs eine Kommunikation mit sehr hoher Bandbreite und geringer Wartezeit ermöglicht.</span><span class="sxs-lookup"><span data-stu-id="d188f-153">For tightly coupled tasks, use an RDMA network that provides very high bandwidth, low latency communication between VMs.</span></span> <span data-ttu-id="d188f-154">Weitere Informationen finden Sie unter [Bereitstellen eines HPC Pack 2016-Clusters in Azure][deploy-hpc-azure].</span><span class="sxs-lookup"><span data-stu-id="d188f-154">For more information see [Deploy an HPC Pack 2016 cluster in Azure][deploy-hpc-azure].</span></span>

### <a name="burst-an-hpc-cluster-to-azure"></a><span data-ttu-id="d188f-155">Burst eines HPC-Clusters in Azure</span><span class="sxs-lookup"><span data-stu-id="d188f-155">Burst an HPC cluster to Azure</span></span>

<span data-ttu-id="d188f-156">Bei diesem Szenario wird HPC Pack in einer Organisation lokal ausgeführt, und Azure-VMs werden für Burstkapazitäts-Zwecke genutzt.</span><span class="sxs-lookup"><span data-stu-id="d188f-156">In this scenario, an organization is running HPC Pack on-premises, and uses Azure VMs for burst capacity.</span></span> <span data-ttu-id="d188f-157">Der Hauptknoten des Clusters ist lokal angeordnet.</span><span class="sxs-lookup"><span data-stu-id="d188f-157">The cluster head node is on-premises.</span></span> <span data-ttu-id="d188f-158">Per ExpressRoute oder VPN Gateway wird für das lokale Netzwerk eine Verbindung mit dem Azure-VNet hergestellt.</span><span class="sxs-lookup"><span data-stu-id="d188f-158">ExpressRoute or VPN Gateway connects the on-premises network to the Azure VNet.</span></span>

![Diagramm eines Big Compute-Hybridclusters](./images/big-compute-hybrid.png)

<!-- links -->

[batch]: /azure/batch/
[batch-hpc-solutions]: /azure/batch/batch-hpc-solutions
[deploy-hpc-azure]: /azure/virtual-machines/windows/hpcpack-2016-cluster
[embarrassingly-parallel]: https://en.wikipedia.org/wiki/Embarrassingly_parallel
[hpc-pack]: https://technet.microsoft.com/library/cc514029
