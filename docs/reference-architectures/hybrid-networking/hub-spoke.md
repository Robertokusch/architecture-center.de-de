---
title: Implementieren einer Hub-Spoke-Netzwerktopologie in Azure
description: Vorgehensweise zum Implementieren einer Hub-Spoke-Netzwerktopologie in Azure
author: telmosampaio
ms.date: 04/09/2018
pnp.series.title: Implement a hub-spoke network topology in Azure
pnp.series.prev: expressroute
ms.openlocfilehash: 9105748f434e5d655b09b1fe0775417f33a912b0
ms.sourcegitcommit: f7fa67e3bdbc57d368edb67bac0e1fdec63695d2
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/05/2018
ms.locfileid: "37843591"
---
# <a name="implement-a-hub-spoke-network-topology-in-azure"></a><span data-ttu-id="be1e0-103">Implementieren einer Hub-Spoke-Netzwerktopologie in Azure</span><span class="sxs-lookup"><span data-stu-id="be1e0-103">Implement a hub-spoke network topology in Azure</span></span>

<span data-ttu-id="be1e0-104">Diese Referenzarchitektur zeigt, wie eine Hub-Spoke-Topologie in Azure implementiert wird.</span><span class="sxs-lookup"><span data-stu-id="be1e0-104">This reference architecture shows how to implement a hub-spoke topology in Azure.</span></span> <span data-ttu-id="be1e0-105">Bei einem *Hub* handelt es sich um ein virtuelles Netzwerk (VNET) in Azure, das als zentraler Konnektivitätspunkt für Ihr lokales Netzwerk fungiert.</span><span class="sxs-lookup"><span data-stu-id="be1e0-105">The *hub* is a virtual network (VNet) in Azure that acts as a central point of connectivity to your on-premises network.</span></span> <span data-ttu-id="be1e0-106">*Spokes* sind VNETs, die eine Peeringverbindung mit dem Hub herstellen und zur Isolierung von Workloads verwendet werden können.</span><span class="sxs-lookup"><span data-stu-id="be1e0-106">The *spokes* are VNets that peer with the hub, and can be used to isolate workloads.</span></span> <span data-ttu-id="be1e0-107">Der Datenverkehr wird über eine ExpressRoute- oder VPN-Gatewayverbindung zwischen dem lokalen Rechenzentrum und dem Hub weitergeleitet.</span><span class="sxs-lookup"><span data-stu-id="be1e0-107">Traffic flows between the on-premises datacenter and the hub through an ExpressRoute or VPN gateway connection.</span></span>  <span data-ttu-id="be1e0-108">[**Stellen Sie diese Lösung bereit**](#deploy-the-solution).</span><span class="sxs-lookup"><span data-stu-id="be1e0-108">[**Deploy this solution**](#deploy-the-solution).</span></span>

<span data-ttu-id="be1e0-109">![[0]][0]</span><span class="sxs-lookup"><span data-stu-id="be1e0-109">![[0]][0]</span></span>

<span data-ttu-id="be1e0-110">*Laden Sie eine [Visio-Datei][visio-download] mit dieser Architektur herunter.*</span><span class="sxs-lookup"><span data-stu-id="be1e0-110">*Download a [Visio file][visio-download] of this architecture*</span></span>


<span data-ttu-id="be1e0-111">Diese Topologie bietet unter anderem folgende Vorteile:</span><span class="sxs-lookup"><span data-stu-id="be1e0-111">The benefits of this toplogy include:</span></span>

* <span data-ttu-id="be1e0-112">**Kosteneinsparungen** durch die Zentralisierung von Diensten, die von mehreren Workloads (z.B. Network Virtual Appliances, kurz NVAs, und DNS-Servern) an einem einzigen Standort gemeinsam genutzt werden können</span><span class="sxs-lookup"><span data-stu-id="be1e0-112">**Cost savings** by centralizing services that can be shared by multiple workloads, such as network virtual appliances (NVAs) and DNS servers, in a single location.</span></span>
* <span data-ttu-id="be1e0-113">**Umgehung von Abonnementbeschränkungen** durch die Herstellung von Peeringverbindungen zwischen VNETs verschiedener Abonnements und dem zentralen Hub</span><span class="sxs-lookup"><span data-stu-id="be1e0-113">**Overcome subscriptions limits** by peering VNets from different subscriptions to the central hub.</span></span>
* <span data-ttu-id="be1e0-114">**Trennung der Belange** zwischen zentralen IT-Vorgängen (SecOps, InfraOps) und Workloads (DevOps)</span><span class="sxs-lookup"><span data-stu-id="be1e0-114">**Separation of concerns** between central IT (SecOps, InfraOps) and workloads (DevOps).</span></span>

<span data-ttu-id="be1e0-115">Typische Einsatzmöglichkeiten für diese Architektur sind Folgende:</span><span class="sxs-lookup"><span data-stu-id="be1e0-115">Typical uses for this architecture include:</span></span>

* <span data-ttu-id="be1e0-116">Workloads, die in verschiedenen Umgebungen wie Entwicklungs-, Test- und Produktionsumgebungen eingesetzt werden und gemeinsame Dienste wie DNS, IDS, NTP oder AD DS erfordern</span><span class="sxs-lookup"><span data-stu-id="be1e0-116">Workloads deployed in different environments, such as development, testing, and production, that require shared services such as DNS, IDS, NTP, or AD DS.</span></span> <span data-ttu-id="be1e0-117">Gemeinsame Dienste werden im Hub-VNET platziert, während die einzelnen Umgebungen in einem Spoke bereitgestellt werden, um die Isolation beizubehalten.</span><span class="sxs-lookup"><span data-stu-id="be1e0-117">Shared services are placed in the hub VNet, while each environment is deployed to a spoke to maintain isolation.</span></span>
* <span data-ttu-id="be1e0-118">Workloads, bei denen keine Konnektivität untereinander bestehen muss, die jedoch Zugriff auf gemeinsame Dienste erfordern</span><span class="sxs-lookup"><span data-stu-id="be1e0-118">Workloads that do not require connectivity to each other, but require access to shared services.</span></span>
* <span data-ttu-id="be1e0-119">Unternehmen, die auf eine zentrale Steuerung von Sicherheitsmechanismen (z.B. eine Firewall im Hub als DMZ) und eine getrennte Verwaltung von Workloads in den einzelnen Spokes angewiesen sind</span><span class="sxs-lookup"><span data-stu-id="be1e0-119">Enterprises that require central control over security aspects, such as a firewall in the hub as a DMZ, and segregated management for the workloads in each spoke.</span></span>

## <a name="architecture"></a><span data-ttu-id="be1e0-120">Architecture</span><span class="sxs-lookup"><span data-stu-id="be1e0-120">Architecture</span></span>

<span data-ttu-id="be1e0-121">Die Architektur umfasst die folgenden Komponenten.</span><span class="sxs-lookup"><span data-stu-id="be1e0-121">The architecture consists of the following components.</span></span>

* <span data-ttu-id="be1e0-122">**Lokales Netzwerk**.</span><span class="sxs-lookup"><span data-stu-id="be1e0-122">**On-premises network**.</span></span> <span data-ttu-id="be1e0-123">Ein in einer Organisation betriebenes privates lokales Netzwerk.</span><span class="sxs-lookup"><span data-stu-id="be1e0-123">A private local-area network running within an organization.</span></span>

* <span data-ttu-id="be1e0-124">**VPN-Gerät**:</span><span class="sxs-lookup"><span data-stu-id="be1e0-124">**VPN device**.</span></span> <span data-ttu-id="be1e0-125">Ein Gerät oder ein Dienst, der externe Konnektivität mit dem lokalen Netzwerk bereitstellt.</span><span class="sxs-lookup"><span data-stu-id="be1e0-125">A device or service that provides external connectivity to the on-premises network.</span></span> <span data-ttu-id="be1e0-126">Bei dem VPN-Gerät kann es sich um ein Hardwaregerät oder eine Softwarelösung wie den Routing- und RAS-Dienst (RRAS) unter Windows Server 2012 handeln.</span><span class="sxs-lookup"><span data-stu-id="be1e0-126">The VPN device may be a hardware device, or a software solution such as the Routing and Remote Access Service (RRAS) in Windows Server 2012.</span></span> <span data-ttu-id="be1e0-127">Eine Liste der unterstützten VPN-Appliances und Informationen zur Konfiguration ausgewählter VPN-Geräte für die Verbindung mit Azure finden Sie unter [Informationen zu VPN-Geräten und IPsec-/IKE-Parametern für VPN-Gatewayverbindungen zwischen Standorten][vpn-appliance].</span><span class="sxs-lookup"><span data-stu-id="be1e0-127">For a list of supported VPN appliances and information on configuring selected VPN appliances for connecting to Azure, see [About VPN devices for Site-to-Site VPN Gateway connections][vpn-appliance].</span></span>

* <span data-ttu-id="be1e0-128">**VPN-Gateway für ein virtuelles Netzwerk oder ExpressRoute-Gateway**:</span><span class="sxs-lookup"><span data-stu-id="be1e0-128">**VPN virtual network gateway or ExpressRoute gateway**.</span></span> <span data-ttu-id="be1e0-129">Über das Gateway für virtuelle Netzwerke kann das VNET zur Konnektivität mit Ihrem lokalen Netzwerk eine Verbindung mit dem VPN-Gerät oder eine ExpressRoute-Verbindung herstellen.</span><span class="sxs-lookup"><span data-stu-id="be1e0-129">The virtual network gateway enables the VNet to connect to the VPN device, or ExpressRoute circuit, used for connectivity with your on-premises network.</span></span> <span data-ttu-id="be1e0-130">Weitere Informationen finden Sie unter [Verbinden eines lokalen Netzwerks mit einem virtuellen Microsoft Azure-Netzwerk][connect-to-an-Azure-vnet].</span><span class="sxs-lookup"><span data-stu-id="be1e0-130">For more information, see [Connect an on-premises network to a Microsoft Azure virtual network][connect-to-an-Azure-vnet].</span></span>

> [!NOTE]
> <span data-ttu-id="be1e0-131">In den Bereitstellungsskripts für diese Referenzarchitektur werden ein VPN-Gateway für die Konnektivität und ein VNET in Azure verwendet, um Ihr lokales Netzwerk zu simulieren.</span><span class="sxs-lookup"><span data-stu-id="be1e0-131">The deployment scripts for this reference architecture use a VPN gateway for connectivity, and a VNet in Azure to simulate your on-premises network.</span></span>

* <span data-ttu-id="be1e0-132">**Hub-VNET**:</span><span class="sxs-lookup"><span data-stu-id="be1e0-132">**Hub VNet**.</span></span> <span data-ttu-id="be1e0-133">Das Azure-VNET, das als Hub in der Hub-Spoke-Topologie verwendet wird.</span><span class="sxs-lookup"><span data-stu-id="be1e0-133">Azure VNet used as the hub in the hub-spoke topology.</span></span> <span data-ttu-id="be1e0-134">Der Hub ist der zentrale Konnektivitätspunkt für Ihr lokales Netzwerk, mit dem Sie Dienste hosten, die von den verschiedenen in den Spoke-VNETs gehosteten Workloads genutzt werden können.</span><span class="sxs-lookup"><span data-stu-id="be1e0-134">The hub is the central point of connectivity to your on-premises network, and a place to host services that can be consumed by the different workloads hosted in the spoke VNets.</span></span>

* <span data-ttu-id="be1e0-135">**Gatewaysubnetz**:</span><span class="sxs-lookup"><span data-stu-id="be1e0-135">**Gateway subnet**.</span></span> <span data-ttu-id="be1e0-136">Die Gateways für virtuelle Netzwerke befinden sich in demselben Subnetz.</span><span class="sxs-lookup"><span data-stu-id="be1e0-136">The virtual network gateways are held in the same subnet.</span></span>

* <span data-ttu-id="be1e0-137">**Spoke-VNETs**:</span><span class="sxs-lookup"><span data-stu-id="be1e0-137">**Spoke VNets**.</span></span> <span data-ttu-id="be1e0-138">Ein oder mehrere Azure-VNETs, die als Spokes in der Hub-Spoke-Topologie verwendet werden.</span><span class="sxs-lookup"><span data-stu-id="be1e0-138">One or more Azure VNets that are used as spokes in the hub-spoke topology.</span></span> <span data-ttu-id="be1e0-139">Spokes können verwendet werden, um Workloads in ihren eigenen VNETs, die getrennt von anderen Spokes verwaltet werden, zu isolieren.</span><span class="sxs-lookup"><span data-stu-id="be1e0-139">Spokes can be used to isolate workloads in their own VNets, managed separately from other spokes.</span></span> <span data-ttu-id="be1e0-140">Jede Workload kann mehrere Schichten umfassen, wobei mehrere Subnetze über Azure Load Balancer verbunden sind.</span><span class="sxs-lookup"><span data-stu-id="be1e0-140">Each workload might include multiple tiers, with multiple subnets connected through Azure load balancers.</span></span> <span data-ttu-id="be1e0-141">Weitere Informationen zur Anwendungsinfrastruktur finden Sie unter [Ausführen von Windows-VM-Workloads][windows-vm-ra] und [Ausführen von Linux-VM-Workloads][linux-vm-ra].</span><span class="sxs-lookup"><span data-stu-id="be1e0-141">For more information about the application infrastructure, see [Running Windows VM workloads][windows-vm-ra] and [Running Linux VM workloads][linux-vm-ra].</span></span>

* <span data-ttu-id="be1e0-142">**VNET-Peering**:</span><span class="sxs-lookup"><span data-stu-id="be1e0-142">**VNet peering**.</span></span> <span data-ttu-id="be1e0-143">Über eine [Peeringverbindung][vnet-peering] können zwei VNETs in derselben Azure-Region miteinander verbunden werden.</span><span class="sxs-lookup"><span data-stu-id="be1e0-143">Two VNets in the same Azure region can be connected using a [peering connection][vnet-peering].</span></span> <span data-ttu-id="be1e0-144">Peeringverbindungen sind nicht-transitive Verbindungen zwischen VNETs mit niedrigen Latenzen.</span><span class="sxs-lookup"><span data-stu-id="be1e0-144">Peering connections are non-transitive, low latency connections between VNets.</span></span> <span data-ttu-id="be1e0-145">Sobald eine Peeringverbindung hergestellt wurde, tauschen die VNETs ohne Einsatz eines Routers Datenverkehr über den Azure-Backbone aus.</span><span class="sxs-lookup"><span data-stu-id="be1e0-145">Once peered, the VNets exchange traffic by using the Azure backbone, without the need for a router.</span></span> <span data-ttu-id="be1e0-146">In einer Hub-Spoke-Netzwerktopologie wird durch VNET-Peering eine Verbindung zwischen dem Hub und den einzelnen Spokes hergestellt.</span><span class="sxs-lookup"><span data-stu-id="be1e0-146">In a hub-spoke network topology, you use VNet peering to connect the hub to each spoke.</span></span>

> [!NOTE]
> <span data-ttu-id="be1e0-147">In diesem Artikel werden ausschließlich [Resource Manager](/azure/azure-resource-manager/resource-group-overview)-Bereitstellungen behandelt, Sie können jedoch auch eine Verbindung zwischen einem klassischen VNET und einem Resource Manager-VNET im selben Abonnement herstellen.</span><span class="sxs-lookup"><span data-stu-id="be1e0-147">This article only covers [Resource Manager](/azure/azure-resource-manager/resource-group-overview) deployments, but you can also connect a classic VNet to a Resource Manager VNet in the same subscription.</span></span> <span data-ttu-id="be1e0-148">Auf diese Weise können Ihre Spokes klassische Bereitstellungen hosten und trotzdem von gemeinsamen Diensten im Hub profitieren.</span><span class="sxs-lookup"><span data-stu-id="be1e0-148">That way, your spokes can host classic deployments and still benefit from services shared in the hub.</span></span>

## <a name="recommendations"></a><span data-ttu-id="be1e0-149">Empfehlungen</span><span class="sxs-lookup"><span data-stu-id="be1e0-149">Recommendations</span></span>

<span data-ttu-id="be1e0-150">Die folgenden Empfehlungen gelten für die meisten Szenarios.</span><span class="sxs-lookup"><span data-stu-id="be1e0-150">The following recommendations apply for most scenarios.</span></span> <span data-ttu-id="be1e0-151">Sofern Sie keine besonderen Anforderungen haben, die Vorrang haben, sollten Sie diese Empfehlungen befolgen.</span><span class="sxs-lookup"><span data-stu-id="be1e0-151">Follow these recommendations unless you have a specific requirement that overrides them.</span></span>

### <a name="resource-groups"></a><span data-ttu-id="be1e0-152">Ressourcengruppen</span><span class="sxs-lookup"><span data-stu-id="be1e0-152">Resource groups</span></span>

<span data-ttu-id="be1e0-153">Das Hub-VNET und die einzelnen Spoke-VNETs können in verschiedenen Ressourcengruppen und sogar in verschiedenen Abonnements implementiert werden, sofern sie demselben Azure Active Directory-Mandanten (Azure AD) in der gleichen Azure-Region angehören.</span><span class="sxs-lookup"><span data-stu-id="be1e0-153">The hub VNet, and each spoke VNet, can be implemented in different resource groups, and even different subscriptions, as long as they belong to the same Azure Active Directory (Azure AD) tenant in the same Azure region.</span></span> <span data-ttu-id="be1e0-154">Dies ermöglicht nicht nur eine dezentralisierte Verwaltung der einzelnen Workloads, sondern auch die Verwaltung gemeinsamer Dienste im Hub-VNET.</span><span class="sxs-lookup"><span data-stu-id="be1e0-154">This allows for a decentralized management of each workload, while sharing services maintained in the hub VNet.</span></span>

### <a name="vnet-and-gatewaysubnet"></a><span data-ttu-id="be1e0-155">VNET und GatewaySubnet</span><span class="sxs-lookup"><span data-stu-id="be1e0-155">VNet and GatewaySubnet</span></span>

<span data-ttu-id="be1e0-156">Erstellen Sie ein Subnetz mit dem Namen *GatewaySubnet* mit dem Adressbereich /27.</span><span class="sxs-lookup"><span data-stu-id="be1e0-156">Create a subnet named *GatewaySubnet*, with an address range of /27.</span></span> <span data-ttu-id="be1e0-157">Dieses Subnetz ist für das Gateway für virtuelle Netzwerke erforderlich.</span><span class="sxs-lookup"><span data-stu-id="be1e0-157">This subnet is required by the virtual network gateway.</span></span> <span data-ttu-id="be1e0-158">Durch Zuweisung von 32 Adressen zu diesem Subnetz wird eine künftige Überschreitung von Beschränkungen der Gatewaygröße verhindert.</span><span class="sxs-lookup"><span data-stu-id="be1e0-158">Allocating 32 addresses to this subnet will help to prevent reaching gateway size limitations in the future.</span></span>

<span data-ttu-id="be1e0-159">Weitere Informationen zum Einrichten des Gateways finden Sie abhängig von Ihrem Verbindungstyp in den folgenden Referenzarchitekturen:</span><span class="sxs-lookup"><span data-stu-id="be1e0-159">For more information about setting up the gateway, see the following reference architectures, depending on your connection type:</span></span>

- <span data-ttu-id="be1e0-160">[Hybrides Netzwerk mit ExpressRoute][guidance-expressroute]</span><span class="sxs-lookup"><span data-stu-id="be1e0-160">[Hybrid network using ExpressRoute][guidance-expressroute]</span></span>
- <span data-ttu-id="be1e0-161">[Hybrides Netzwerk mit einem VPN-Gateway][guidance-vpn]</span><span class="sxs-lookup"><span data-stu-id="be1e0-161">[Hybrid network using a VPN gateway][guidance-vpn]</span></span>

<span data-ttu-id="be1e0-162">Um eine höhere Verfügbarkeit zu erzielen, können Sie ExpressRoute mit einem VPN für Failoverzwecke kombinieren.</span><span class="sxs-lookup"><span data-stu-id="be1e0-162">For higher availability, you can use ExpressRoute plus a VPN for failover.</span></span> <span data-ttu-id="be1e0-163">Weitere Informationen finden Sie unter [Verbinden eines lokalen Netzwerks mit Azure unter Verwendung von ExpressRoute mit VPN-Failover][hybrid-ha].</span><span class="sxs-lookup"><span data-stu-id="be1e0-163">See [Connect an on-premises network to Azure using ExpressRoute with VPN failover][hybrid-ha].</span></span>

<span data-ttu-id="be1e0-164">Eine Hub-Spoke-Topologie kann auch ohne Gateway verwendet werden, wenn keine Konnektivität mit Ihrem lokalen Netzwerk erforderlich ist.</span><span class="sxs-lookup"><span data-stu-id="be1e0-164">A hub-spoke topology can also be used without a gateway, if you don't need connectivity with your on-premises network.</span></span> 

### <a name="vnet-peering"></a><span data-ttu-id="be1e0-165">VNet-Peering</span><span class="sxs-lookup"><span data-stu-id="be1e0-165">VNet peering</span></span>

<span data-ttu-id="be1e0-166">Beim VNET-Peering wird eine nicht-transitive Beziehung zwischen zwei VNETs hergestellt.</span><span class="sxs-lookup"><span data-stu-id="be1e0-166">VNet peering is a non-transitive relationship between two VNets.</span></span> <span data-ttu-id="be1e0-167">Wenn Sie eine Verbindung zwischen Spokes herstellen möchten, sollten Sie eventuell eine separate Peeringverbindung zwischen diesen Spokes herstellen.</span><span class="sxs-lookup"><span data-stu-id="be1e0-167">If you require spokes to connect to each other, consider adding a separate peering connection between those spokes.</span></span>

<span data-ttu-id="be1e0-168">Wenn jedoch zwischen mehreren Spokes eine Verbindung hergestellt werden muss, werden die möglichen Peeringverbindungen sehr schnell zur Neige gehen, da [die Anzahl der VNET-Peerings pro VNET begrenzt ist][vnet-peering-limit].</span><span class="sxs-lookup"><span data-stu-id="be1e0-168">However, if you have several spokes that need to connect with each other, you will run out of possible peering connections very quickly due to the [limitation on number of VNets peerings per VNet][vnet-peering-limit].</span></span> <span data-ttu-id="be1e0-169">In diesem Szenario sollten Sie die Verwendung von benutzerdefinierten Routen (User Defined Routes, UDRs) in Erwägung ziehen, um zu erzwingen, dass der für einen Spoke vorgesehene Datenverkehr an eine NVA, die als Router im Hub-VNET fungiert, gesendet wird.</span><span class="sxs-lookup"><span data-stu-id="be1e0-169">In this scenario, consider using user defined routes (UDRs) to force traffic destined to a spoke to be sent to an NVA acting as a router at the hub VNet.</span></span> <span data-ttu-id="be1e0-170">Hierdurch können die Spokes miteinander verbunden werden.</span><span class="sxs-lookup"><span data-stu-id="be1e0-170">This will allow the spokes to connect to each other.</span></span>

<span data-ttu-id="be1e0-171">Sie können Spokes auch für die Kommunikation mit Remotenetzwerken über das Gateway für das Hub-VNET konfigurieren.</span><span class="sxs-lookup"><span data-stu-id="be1e0-171">You can also configure spokes to use the hub VNet gateway to communicate with remote networks.</span></span> <span data-ttu-id="be1e0-172">Damit der Gatewaydatenverkehr zwischen den Spokes und dem Hub weitergeleitet und eine Verbindung mit Remotenetzwerken hergestellt werden kann, müssen Sie folgende Schritte durchführen:</span><span class="sxs-lookup"><span data-stu-id="be1e0-172">To allow gateway traffic to flow from spoke to hub, and connect to remote networks, you must:</span></span>

  - <span data-ttu-id="be1e0-173">Konfigurieren Sie die VNET-Peeringverbindung im Hub dahingehend, dass **Gatewaytransit zugelassen** wird.</span><span class="sxs-lookup"><span data-stu-id="be1e0-173">Configure the VNet peering connection in the hub to **allow gateway transit**.</span></span>
  - <span data-ttu-id="be1e0-174">Konfigurieren Sie die VNET-Peeringverbindung in den einzelnen Spokes dahingehend, dass **Remotegateways verwendet werden**.</span><span class="sxs-lookup"><span data-stu-id="be1e0-174">Configure the VNet peering connection in each spoke to **use remote gateways**.</span></span>
  - <span data-ttu-id="be1e0-175">Konfigurieren Sie alle VNET-Peeringverbindungen dahingehend, dass **weitergeleiteter Datenverkehr zugelassen** wird.</span><span class="sxs-lookup"><span data-stu-id="be1e0-175">Configure all VNet peering connections to **allow forwarded traffic**.</span></span>

## <a name="considerations"></a><span data-ttu-id="be1e0-176">Überlegungen</span><span class="sxs-lookup"><span data-stu-id="be1e0-176">Considerations</span></span>

### <a name="spoke-connectivity"></a><span data-ttu-id="be1e0-177">Konnektivität zwischen Spokes</span><span class="sxs-lookup"><span data-stu-id="be1e0-177">Spoke connectivity</span></span>

<span data-ttu-id="be1e0-178">Wenn Konnektivität zwischen Spokes hergestellt werden muss, sollten Sie eine NVA für das Routing im Hub implementieren und UDRs im Spoke zur Weiterleitung des Datenverkehrs an den Hub verwenden.</span><span class="sxs-lookup"><span data-stu-id="be1e0-178">If you require connectivity between spokes, consider implementing an NVA for routing in the hub, and using UDRs in the spoke to forward traffic to the hub.</span></span>

<span data-ttu-id="be1e0-179">![[2]][2]</span><span class="sxs-lookup"><span data-stu-id="be1e0-179">![[2]][2]</span></span>

<span data-ttu-id="be1e0-180">In diesem Szenario müssen Sie die Peeringverbindungen dahingehend konfigurieren, dass **weitergeleiteter Verkehr zugelassen** wird.</span><span class="sxs-lookup"><span data-stu-id="be1e0-180">In this scenario, you must configure the peering connections to **allow forwarded traffic**.</span></span>

### <a name="overcoming-vnet-peering-limits"></a><span data-ttu-id="be1e0-181">Umgehung von VNET-Peeringbeschränkungen</span><span class="sxs-lookup"><span data-stu-id="be1e0-181">Overcoming VNet peering limits</span></span>

<span data-ttu-id="be1e0-182">Stellen Sie sicher, dass Sie die [Begrenzung der Anzahl von VNET-Peerings pro VNET][vnet-peering-limit] in Azure einhalten.</span><span class="sxs-lookup"><span data-stu-id="be1e0-182">Make sure you consider the [limitation on number of VNets peerings per VNet][vnet-peering-limit] in Azure.</span></span> <span data-ttu-id="be1e0-183">Falls Sie mehr Spokes als die maximal zulässige Anzahl benötigen, sollten Sie eventuell eine Hub-Spoke-Hub-Spoke-Topologie erstellen, bei der die erste Ebene von Spokes auch als Hub fungiert.</span><span class="sxs-lookup"><span data-stu-id="be1e0-183">If you decide you need more spokes than the limit will allow, consider creating a hub-spoke-hub-spoke topology, where the first level of spokes also act as hubs.</span></span> <span data-ttu-id="be1e0-184">Im folgenden Diagramm wird diese Vorgehensweise veranschaulicht.</span><span class="sxs-lookup"><span data-stu-id="be1e0-184">The following diagram shows this approach.</span></span>

<span data-ttu-id="be1e0-185">![[3]][3]</span><span class="sxs-lookup"><span data-stu-id="be1e0-185">![[3]][3]</span></span>

<span data-ttu-id="be1e0-186">Berücksichtigen Sie auch die Dienste, die im Hub gemeinsam genutzt werden, um sicherzustellen, dass sich der Hub für eine größere Anzahl von Spokes skalieren lässt.</span><span class="sxs-lookup"><span data-stu-id="be1e0-186">Also consider what services are shared in the hub, to ensure the hub scales for a larger number of spokes.</span></span> <span data-ttu-id="be1e0-187">Wenn Ihr Hub beispielsweise Firewalldienste bereitstellt, sollten Sie beim Hinzufügen mehrerer Spokes die Bandbreitenbeschränkungen Ihrer Firewalllösung berücksichtigen.</span><span class="sxs-lookup"><span data-stu-id="be1e0-187">For instance, if your hub provides firewall services, consider the bandwidth limits of your firewall solution when adding multiple spokes.</span></span> <span data-ttu-id="be1e0-188">Es wird empfohlen, einige dieser gemeinsamen Dienste auf eine zweite Hubebene zu verlagern.</span><span class="sxs-lookup"><span data-stu-id="be1e0-188">You might want to move some of these shared services to a second level of hubs.</span></span>

## <a name="deploy-the-solution"></a><span data-ttu-id="be1e0-189">Bereitstellen der Lösung</span><span class="sxs-lookup"><span data-stu-id="be1e0-189">Deploy the solution</span></span>

<span data-ttu-id="be1e0-190">Eine Bereitstellung für diese Architektur ist auf [GitHub][ref-arch-repo] verfügbar.</span><span class="sxs-lookup"><span data-stu-id="be1e0-190">A deployment for this architecture is available on [GitHub][ref-arch-repo].</span></span> <span data-ttu-id="be1e0-191">Bei dieser werden zum Testen der Konnektivität VMs in jedem VNET verwendet.</span><span class="sxs-lookup"><span data-stu-id="be1e0-191">It uses VMs in each VNet to test connectivity.</span></span> <span data-ttu-id="be1e0-192">Es gibt keine tatsächlichen Dienste, die im Subnetz für **gemeinsame Dienste** im **Hub-VNET** gehostet werden.</span><span class="sxs-lookup"><span data-stu-id="be1e0-192">There are no actual services hosted in the **shared-services** subnet in the **hub VNet**.</span></span>

<span data-ttu-id="be1e0-193">Die Bereitstellung erstellt die folgenden Ressourcengruppen in Ihrem Abonnement:</span><span class="sxs-lookup"><span data-stu-id="be1e0-193">The deployment creates the following resource groups in your subscription:</span></span>

- <span data-ttu-id="be1e0-194">hub-nva-rg</span><span class="sxs-lookup"><span data-stu-id="be1e0-194">hub-nva-rg</span></span>
- <span data-ttu-id="be1e0-195">hub-vnet-rg</span><span class="sxs-lookup"><span data-stu-id="be1e0-195">hub-vnet-rg</span></span>
- <span data-ttu-id="be1e0-196">onprem-jb-rg</span><span class="sxs-lookup"><span data-stu-id="be1e0-196">onprem-jb-rg</span></span>
- <span data-ttu-id="be1e0-197">onprem-vnet-rg</span><span class="sxs-lookup"><span data-stu-id="be1e0-197">onprem-vnet-rg</span></span>
- <span data-ttu-id="be1e0-198">spoke1-vnet-rg</span><span class="sxs-lookup"><span data-stu-id="be1e0-198">spoke1-vnet-rg</span></span>
- <span data-ttu-id="be1e0-199">spoke2-vent-rg</span><span class="sxs-lookup"><span data-stu-id="be1e0-199">spoke2-vent-rg</span></span>

<span data-ttu-id="be1e0-200">Die Vorlagenparameterdateien beziehen sich auf diese Namen, d.h. wenn Sie sie ändern, müssen Sie die Parameterdateien entsprechend aktualisieren.</span><span class="sxs-lookup"><span data-stu-id="be1e0-200">The template parameter files refer to these names, so if you change them, update the parameter files to match.</span></span>

### <a name="prerequisites"></a><span data-ttu-id="be1e0-201">Voraussetzungen</span><span class="sxs-lookup"><span data-stu-id="be1e0-201">Prerequisites</span></span>

[!INCLUDE [ref-arch-prerequisites.md](../../../includes/ref-arch-prerequisites.md)]

### <a name="deploy-the-simulated-on-premises-datacenter"></a><span data-ttu-id="be1e0-202">Bereitstellen des simulierten lokalen Rechenzentrums</span><span class="sxs-lookup"><span data-stu-id="be1e0-202">Deploy the simulated on-premises datacenter</span></span>

<span data-ttu-id="be1e0-203">Führen Sie die folgenden Schritte aus, um das simulierte lokale Rechenzentrum als Azure-VNET bereitzustellen:</span><span class="sxs-lookup"><span data-stu-id="be1e0-203">To deploy the simulated on-premises datacenter as an Azure VNet, follow these steps:</span></span>

1. <span data-ttu-id="be1e0-204">Navigieren Sie zum `hybrid-networking/hub-spoke`-Ordner des Repositorys für Referenzarchitekturen.</span><span class="sxs-lookup"><span data-stu-id="be1e0-204">Navigate to the `hybrid-networking/hub-spoke` folder of the reference architectures repository.</span></span>

2. <span data-ttu-id="be1e0-205">Öffnen Sie die Datei `onprem.json` .</span><span class="sxs-lookup"><span data-stu-id="be1e0-205">Open the `onprem.json` file.</span></span> <span data-ttu-id="be1e0-206">Ersetzen Sie die Werte für `adminUsername` und `adminPassword`.</span><span class="sxs-lookup"><span data-stu-id="be1e0-206">Replace the values for `adminUsername` and `adminPassword`.</span></span>

    ```bash
    "adminUsername": "<user name>",
    "adminPassword": "<password>",
    ```

3. <span data-ttu-id="be1e0-207">(Optional) Legen Sie für eine Linux-Bereitstellung `osType` auf `Linux` fest.</span><span class="sxs-lookup"><span data-stu-id="be1e0-207">(Optional) For a Linux deployment, set `osType` to `Linux`.</span></span>

4. <span data-ttu-id="be1e0-208">Führen Sie den folgenden Befehl aus:</span><span class="sxs-lookup"><span data-stu-id="be1e0-208">Run the following command:</span></span>

    ```bash
    azbb -s <subscription_id> -g onprem-vnet-rg -l <location> -p onprem.json --deploy
    ```

5. <span data-ttu-id="be1e0-209">Warten Sie, bis die Bereitstellung abgeschlossen ist.</span><span class="sxs-lookup"><span data-stu-id="be1e0-209">Wait for the deployment to finish.</span></span> <span data-ttu-id="be1e0-210">Bei dieser Bereitstellung werden ein virtuelles Netzwerk, ein virtueller Computer und ein VPN-Gateway erstellt.</span><span class="sxs-lookup"><span data-stu-id="be1e0-210">This deployment creates a virtual network, a virtual machine, and a VPN gateway.</span></span> <span data-ttu-id="be1e0-211">Das Erstellen des VPN-Gateways kann etwa 40 Minuten dauern.</span><span class="sxs-lookup"><span data-stu-id="be1e0-211">It can take about 40 minutes to create the VPN gateway.</span></span>

### <a name="deploy-the-hub-vnet"></a><span data-ttu-id="be1e0-212">Bereitstellen des Hub-VNet</span><span class="sxs-lookup"><span data-stu-id="be1e0-212">Deploy the hub VNet</span></span>

<span data-ttu-id="be1e0-213">Führen Sie die folgenden Schritte aus, um das Hub-VNet bereitzustellen.</span><span class="sxs-lookup"><span data-stu-id="be1e0-213">To deploy the hub VNet, perform the following steps.</span></span>

1. <span data-ttu-id="be1e0-214">Öffnen Sie die Datei `hub-vnet.json` .</span><span class="sxs-lookup"><span data-stu-id="be1e0-214">Open the `hub-vnet.json` file.</span></span> <span data-ttu-id="be1e0-215">Ersetzen Sie die Werte für `adminUsername` und `adminPassword`.</span><span class="sxs-lookup"><span data-stu-id="be1e0-215">Replace the values for `adminUsername` and `adminPassword`.</span></span>

    ```bash
    "adminUsername": "<user name>",
    "adminPassword": "<password>",
    ```

2. <span data-ttu-id="be1e0-216">(Optional) Legen Sie für eine Linux-Bereitstellung `osType` auf `Linux` fest.</span><span class="sxs-lookup"><span data-stu-id="be1e0-216">(Optional) For a Linux deployment, set `osType` to `Linux`.</span></span>

3. <span data-ttu-id="be1e0-217">Suchen Sie beide Instanzen von `sharedKey`, und geben Sie einen gemeinsam genutzten Schlüssel für die VPN-Verbindung ein.</span><span class="sxs-lookup"><span data-stu-id="be1e0-217">Find both instances of `sharedKey` and enter a shared key for the VPN connection.</span></span> <span data-ttu-id="be1e0-218">Die Werte müssen übereinstimmen.</span><span class="sxs-lookup"><span data-stu-id="be1e0-218">The values must match.</span></span>

    ```bash
    "sharedKey": "",
    ```

4. <span data-ttu-id="be1e0-219">Führen Sie den folgenden Befehl aus:</span><span class="sxs-lookup"><span data-stu-id="be1e0-219">Run the following command:</span></span>

    ```bash
    azbb -s <subscription_id> -g hub-vnet-rg -l <location> -p hub-vnet.json --deploy
    ```

5. <span data-ttu-id="be1e0-220">Warten Sie, bis die Bereitstellung abgeschlossen ist.</span><span class="sxs-lookup"><span data-stu-id="be1e0-220">Wait for the deployment to finish.</span></span> <span data-ttu-id="be1e0-221">Bei dieser Bereitstellung werden ein virtuelles Netzwerk, ein virtueller Computer, ein VPN-Gateway und eine Verbindung mit dem Gateway erstellt.</span><span class="sxs-lookup"><span data-stu-id="be1e0-221">This deployment creates a virtual network, a virtual machine, a VPN gateway, and a connection to the gateway.</span></span>  <span data-ttu-id="be1e0-222">Das Erstellen des VPN-Gateways kann etwa 40 Minuten dauern.</span><span class="sxs-lookup"><span data-stu-id="be1e0-222">It can take about 40 minutes to create the VPN gateway.</span></span>

### <a name="test-connectivity-with-the-hub"></a><span data-ttu-id="be1e0-223">Testen der Konnektivität mit dem Hub</span><span class="sxs-lookup"><span data-stu-id="be1e0-223">Test connectivity with the hub</span></span>

<span data-ttu-id="be1e0-224">Testen Sie die Konnektivität zwischen der simulierten lokalen Umgebung und dem Hub-VNET.</span><span class="sxs-lookup"><span data-stu-id="be1e0-224">Test conectivity from the simulated on-premises environment to the hub VNet.</span></span>

<span data-ttu-id="be1e0-225">**Windows-Bereitstellung**</span><span class="sxs-lookup"><span data-stu-id="be1e0-225">**Windows deployment**</span></span>

1. <span data-ttu-id="be1e0-226">Suchen Sie im Azure-Portal die VM namens `jb-vm1` in der `onprem-jb-rg`-Ressourcengruppe.</span><span class="sxs-lookup"><span data-stu-id="be1e0-226">Use the Azure portal to find the VM named `jb-vm1` in the `onprem-jb-rg` resource group.</span></span>

2. <span data-ttu-id="be1e0-227">Klicke Sie auf `Connect`, um eine Remotedesktopsitzung mit der VM zu öffnen.</span><span class="sxs-lookup"><span data-stu-id="be1e0-227">Click `Connect` to open a remote desktop session to the VM.</span></span> <span data-ttu-id="be1e0-228">Verwenden Sie das Kennwort, das Sie in der `onprem.json`-Parameterdatei angegeben haben.</span><span class="sxs-lookup"><span data-stu-id="be1e0-228">Use the password that you specified in the `onprem.json` parameter file.</span></span>

3. <span data-ttu-id="be1e0-229">Öffnen Sie auf der VM eine PowerShell-Konsole, und verwenden Sie das `Test-NetConnection`-Cmdlet, um sicherzustellen, dass Sie eine Verbindung mit der Jumpbox-VM im Hub-VNet herstellen können.</span><span class="sxs-lookup"><span data-stu-id="be1e0-229">Open a PowerShell console in the VM, and use the `Test-NetConnection` cmdlet to verify that you can connect to the jumpbox VM in the hub VNet.</span></span>

   ```powershell
   Test-NetConnection 10.0.0.68 -CommonTCPPort RDP
   ```
<span data-ttu-id="be1e0-230">Die Ausgabe sollte in etwa wie folgt aussehen:</span><span class="sxs-lookup"><span data-stu-id="be1e0-230">The output should look similar to the following:</span></span>

```powershell
ComputerName     : 10.0.0.68
RemoteAddress    : 10.0.0.68
RemotePort       : 3389
InterfaceAlias   : Ethernet 2
SourceAddress    : 192.168.1.000
TcpTestSucceeded : True
```

> [!NOTE]
> <span data-ttu-id="be1e0-231">Standardmäßig lassen Windows Server-VMs in Azure keine ICMP-Antworten zu.</span><span class="sxs-lookup"><span data-stu-id="be1e0-231">By default, Windows Server VMs do not allow ICMP responses in Azure.</span></span> <span data-ttu-id="be1e0-232">Wenn Sie `ping` zum Testen der Konnektivität nutzen möchten, müssen Sie für jede VM ICMP-Datenverkehr in der erweiterten Windows-Firewall aktivieren.</span><span class="sxs-lookup"><span data-stu-id="be1e0-232">If you want to use `ping` to test connectivity, you need to enable ICMP traffic in the Windows Advanced Firewall for each VM.</span></span>

<span data-ttu-id="be1e0-233">**Linux-Bereitstellung**</span><span class="sxs-lookup"><span data-stu-id="be1e0-233">**Linux deployment**</span></span>

1. <span data-ttu-id="be1e0-234">Suchen Sie im Azure-Portal die VM namens `jb-vm1` in der `onprem-jb-rg`-Ressourcengruppe.</span><span class="sxs-lookup"><span data-stu-id="be1e0-234">Use the Azure portal to find the VM named `jb-vm1` in the `onprem-jb-rg` resource group.</span></span>

2. <span data-ttu-id="be1e0-235">Klicken Sie auf `Connect`, und kopieren Sie den im Portal angezeigten `ssh`-Befehl.</span><span class="sxs-lookup"><span data-stu-id="be1e0-235">Click `Connect` and copy the `ssh` command shown in the portal.</span></span> 

3. <span data-ttu-id="be1e0-236">Führen Sie in einer Linux-Eingabeaufforderung `ssh` zum Herstellen der Verbindung mit der simulierten lokalen Umgebung aus.</span><span class="sxs-lookup"><span data-stu-id="be1e0-236">From a Linux prompt, run `ssh` to connect to the simulated on-premises environment.</span></span> <span data-ttu-id="be1e0-237">Verwenden Sie das Kennwort, das Sie in der `onprem.json`-Parameterdatei angegeben haben.</span><span class="sxs-lookup"><span data-stu-id="be1e0-237">Use the password that you specified in the `onprem.json` parameter file.</span></span>

4. <span data-ttu-id="be1e0-238">Verwenden Sie den Befehl `ping`, um die Konnektivität mit der Jumpbox-VM im Hub-VNet zu testen:</span><span class="sxs-lookup"><span data-stu-id="be1e0-238">Use the `ping` command to test connectivity to the jumpbox VM in the hub VNet:</span></span>

   ```bash
   ping 10.0.0.68
   ```

### <a name="deploy-the-spoke-vnets"></a><span data-ttu-id="be1e0-239">Bereitstellen der Spoke-VNets</span><span class="sxs-lookup"><span data-stu-id="be1e0-239">Deploy the spoke VNets</span></span>

<span data-ttu-id="be1e0-240">Führen Sie die folgenden Schritte aus, um die Spoke-VNETs bereitzustellen.</span><span class="sxs-lookup"><span data-stu-id="be1e0-240">To deploy the spoke VNets, perform the following steps.</span></span>

1. <span data-ttu-id="be1e0-241">Öffnen Sie die Datei `spoke1.json` .</span><span class="sxs-lookup"><span data-stu-id="be1e0-241">Open the `spoke1.json` file.</span></span> <span data-ttu-id="be1e0-242">Ersetzen Sie die Werte für `adminUsername` und `adminPassword`.</span><span class="sxs-lookup"><span data-stu-id="be1e0-242">Replace the values for `adminUsername` and `adminPassword`.</span></span>

    ```bash
    "adminUsername": "<user name>",
    "adminPassword": "<password>",
    ```

2. <span data-ttu-id="be1e0-243">(Optional) Legen Sie für eine Linux-Bereitstellung `osType` auf `Linux` fest.</span><span class="sxs-lookup"><span data-stu-id="be1e0-243">(Optional) For a Linux deployment, set `osType` to `Linux`.</span></span>

3. <span data-ttu-id="be1e0-244">Führen Sie den folgenden Befehl aus:</span><span class="sxs-lookup"><span data-stu-id="be1e0-244">Run the following command:</span></span>

   ```bash
   azbb -s <subscription_id> -g spoke1-vnet-rg -l <location> -p spoke1.json --deploy
   ```
  
4. <span data-ttu-id="be1e0-245">Wiederholen Sie die Schritte 1 bis 2 für die `spoke2.json`-Datei.</span><span class="sxs-lookup"><span data-stu-id="be1e0-245">Repeat steps 1-2 for the `spoke2.json` file.</span></span>

5. <span data-ttu-id="be1e0-246">Führen Sie den folgenden Befehl aus:</span><span class="sxs-lookup"><span data-stu-id="be1e0-246">Run the following command:</span></span>

   ```bash
   azbb -s <subscription_id> -g spoke2-vnet-rg -l <location> -p spoke2.json --deploy
   ```

6. <span data-ttu-id="be1e0-247">Führen Sie den folgenden Befehl aus:</span><span class="sxs-lookup"><span data-stu-id="be1e0-247">Run the following command:</span></span>

   ```bash
   azbb -s <subscription_id> -g hub-vnet-rg -l <location> -p hub-vnet-peering.json --deploy
   ```

### <a name="test-connectivity"></a><span data-ttu-id="be1e0-248">Testen der Konnektivität</span><span class="sxs-lookup"><span data-stu-id="be1e0-248">Test connectivity</span></span>

<span data-ttu-id="be1e0-249">Testen Sie die Konnektivität zwischen der simulierten lokalen Umgebung und den Spoke-VNets.</span><span class="sxs-lookup"><span data-stu-id="be1e0-249">Test conectivity from the simulated on-premises environment to the spoke VNets.</span></span>

<span data-ttu-id="be1e0-250">**Windows-Bereitstellung**</span><span class="sxs-lookup"><span data-stu-id="be1e0-250">**Windows deployment**</span></span>

1. <span data-ttu-id="be1e0-251">Suchen Sie im Azure-Portal die VM namens `jb-vm1` in der `onprem-jb-rg`-Ressourcengruppe.</span><span class="sxs-lookup"><span data-stu-id="be1e0-251">Use the Azure portal to find the VM named `jb-vm1` in the `onprem-jb-rg` resource group.</span></span>

2. <span data-ttu-id="be1e0-252">Klicke Sie auf `Connect`, um eine Remotedesktopsitzung mit der VM zu öffnen.</span><span class="sxs-lookup"><span data-stu-id="be1e0-252">Click `Connect` to open a remote desktop session to the VM.</span></span> <span data-ttu-id="be1e0-253">Verwenden Sie das Kennwort, das Sie in der `onprem.json`-Parameterdatei angegeben haben.</span><span class="sxs-lookup"><span data-stu-id="be1e0-253">Use the password that you specified in the `onprem.json` parameter file.</span></span>

3. <span data-ttu-id="be1e0-254">Öffnen Sie auf dem virtuellen Computer eine PowerShell-Konsole, und stellen Sie mithilfe des `Test-NetConnection`-Cmdlets sicher, dass Sie eine Verbindung mit dem virtuellen Jumpbox-Computer in den Spoke-VNETs herstellen können.</span><span class="sxs-lookup"><span data-stu-id="be1e0-254">Open a PowerShell console in the VM, and use the `Test-NetConnection` cmdlet to verify that you can connect to the jumpbox VMs in the spoke VNets.</span></span>

   ```powershell
   Test-NetConnection 10.1.0.68 -CommonTCPPort RDP
   Test-NetConnection 10.2.0.68 -CommonTCPPort RDP
   ```

<span data-ttu-id="be1e0-255">**Linux-Bereitstellung**</span><span class="sxs-lookup"><span data-stu-id="be1e0-255">**Linux deployment**</span></span>

<span data-ttu-id="be1e0-256">Führen Sie die folgenden Schritte aus, um die Konnektivität der simulierten lokalen Umgebung mit den Spoke-VNETs mit Linux-VMs zu testen:</span><span class="sxs-lookup"><span data-stu-id="be1e0-256">To test conectivity from the simulated on-premises environment to the spoke VNets using Linux VMs, perform the following steps:</span></span>

1. <span data-ttu-id="be1e0-257">Suchen Sie im Azure-Portal die VM namens `jb-vm1` in der `onprem-jb-rg`-Ressourcengruppe.</span><span class="sxs-lookup"><span data-stu-id="be1e0-257">Use the Azure portal to find the VM named `jb-vm1` in the `onprem-jb-rg` resource group.</span></span>

2. <span data-ttu-id="be1e0-258">Klicken Sie auf `Connect`, und kopieren Sie den im Portal angezeigten `ssh`-Befehl.</span><span class="sxs-lookup"><span data-stu-id="be1e0-258">Click `Connect` and copy the `ssh` command shown in the portal.</span></span> 

3. <span data-ttu-id="be1e0-259">Führen Sie in einer Linux-Eingabeaufforderung `ssh` zum Herstellen der Verbindung mit der simulierten lokalen Umgebung aus.</span><span class="sxs-lookup"><span data-stu-id="be1e0-259">From a Linux prompt, run `ssh` to connect to the simulated on-premises environment.</span></span> <span data-ttu-id="be1e0-260">Verwenden Sie das Kennwort, das Sie in der `onprem.json`-Parameterdatei angegeben haben.</span><span class="sxs-lookup"><span data-stu-id="be1e0-260">Use the password that you specified in the `onprem.json` parameter file.</span></span>

5. <span data-ttu-id="be1e0-261">Verwenden Sie den Befehl `ping`, um die Konnektivität mit den Jumpbox-VMs in den einzelnen Spokes zu testen:</span><span class="sxs-lookup"><span data-stu-id="be1e0-261">Use the `ping` command to test connectivity to the jumpbox VMs in each spoke:</span></span>

   ```bash
   ping 10.1.0.68
   ping 10.2.0.68
   ```

### <a name="add-connectivity-between-spokes"></a><span data-ttu-id="be1e0-262">Herstellen von Konnektivität zwischen Spokes</span><span class="sxs-lookup"><span data-stu-id="be1e0-262">Add connectivity between spokes</span></span>

<span data-ttu-id="be1e0-263">Dieser Schritt ist optional.</span><span class="sxs-lookup"><span data-stu-id="be1e0-263">This step is optional.</span></span> <span data-ttu-id="be1e0-264">Wenn Sie zulassen möchten, dass Spokes Verbindungen miteinander herstellen können, müssen Sie ein virtuelles Netzwerkgerät als Router im Hub-VNet verwenden. Außerdem müssen Sie erzwingen, dass Datenverkehr von den Spokes über den Router verläuft, wenn versucht wird, eine Verbindung mit anderen Spokes herzustellen.</span><span class="sxs-lookup"><span data-stu-id="be1e0-264">If you want to allow spokes to connect to each other, you must use a newtwork virtual appliance (NVA) as a router in the hub VNet, and force traffic from spokes to the router when trying to connect to another spoke.</span></span> <span data-ttu-id="be1e0-265">Führen Sie die folgenden Schritte aus, um ein einfaches Beispiel-Netzwerkgerät als einzelne VM und benutzerdefinierte Routen bereitzustellen, um die Verbindungsherstellung zwischen den zwei Spoke-VNETs zu ermöglichen:</span><span class="sxs-lookup"><span data-stu-id="be1e0-265">To deploy a basic sample NVA as a single VM, along with user-defined routes (UDRs) to allow the two spoke VNets to connect, perform the following steps:</span></span>

1. <span data-ttu-id="be1e0-266">Öffnen Sie die Datei `hub-nva.json` .</span><span class="sxs-lookup"><span data-stu-id="be1e0-266">Open the `hub-nva.json` file.</span></span> <span data-ttu-id="be1e0-267">Ersetzen Sie die Werte für `adminUsername` und `adminPassword`.</span><span class="sxs-lookup"><span data-stu-id="be1e0-267">Replace the values for `adminUsername` and `adminPassword`.</span></span>

    ```bash
    "adminUsername": "<user name>",
    "adminPassword": "<password>",
    ```

2. <span data-ttu-id="be1e0-268">Führen Sie den folgenden Befehl aus:</span><span class="sxs-lookup"><span data-stu-id="be1e0-268">Run the following command:</span></span>

   ```bash
   azbb -s <subscription_id> -g hub-nva-rg -l <location> -p hub-nva.json --deploy
   ```

<!-- links -->

[azure-cli-2]: /azure/install-azure-cli
[azbb]: https://github.com/mspnp/template-building-blocks/wiki/Install-Azure-Building-Blocks
[azure-vpn-gateway]: /azure/vpn-gateway/vpn-gateway-about-vpngateways
[best-practices-security]: /azure/best-practices-network-securit
[connect-to-an-Azure-vnet]: https://technet.microsoft.com/library/dn786406.aspx
[guidance-expressroute]: ./expressroute.md
[guidance-vpn]: ./vpn.md
[linux-vm-ra]: ../virtual-machines-linux/index.md
[hybrid-ha]: ./expressroute-vpn-failover.md
[naming conventions]: /azure/guidance/guidance-naming-conventions
[resource-manager-overview]: /azure/azure-resource-manager/resource-group-overview
[vnet-peering]: /azure/virtual-network/virtual-network-peering-overview
[vnet-peering-limit]: /azure/azure-subscription-service-limits#networking-limits
[vpn-appliance]: /azure/vpn-gateway/vpn-gateway-about-vpn-devices
[windows-vm-ra]: ../virtual-machines-windows/index.md

[visio-download]: https://archcenter.blob.core.windows.net/cdn/hybrid-network-hub-spoke.vsdx
[ref-arch-repo]: https://github.com/mspnp/reference-architectures
[0]: ./images/hub-spoke.png "Hub-Spoke-Topologie in Azure"
[1]: ./images/hub-spoke-gateway-routing.svg "Hub-Spoke-Topologie in Azure mit transitivem Routing"
[2]: ./images/hub-spoke-no-gateway-routing.svg "Hub-Spoke-Topologie in Azure mit transitivem Routing unter Verwendung eines NVA"
[3]: ./images/hub-spokehub-spoke.svg "Hub-Spoke-Hub-Spoke-Topologie in Azure"
[ARM-Templates]: https://azure.microsoft.com/documentation/articles/resource-group-authoring-templates/
