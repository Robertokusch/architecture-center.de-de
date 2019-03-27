---
title: Implementieren einer Hub-Spoke-Netzwerktopologie in Azure
titleSuffix: Azure Reference Architectures
description: Implementieren einer Hub-Spoke-Netzwerktopologie in Azure
author: telmosampaio
ms.date: 10/08/2018
ms.topic: reference-architecture
ms.service: architecture-center
ms.subservice: reference-architecture
ms.custom: seodec18, networking
ms.openlocfilehash: 4235e5d1bb3b202cff9f7c703f079651982aac59
ms.sourcegitcommit: c053e6edb429299a0ad9b327888d596c48859d4a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/20/2019
ms.locfileid: "58246111"
---
# <a name="implement-a-hub-spoke-network-topology-in-azure"></a><span data-ttu-id="a14f5-103">Implementieren einer Hub-Spoke-Netzwerktopologie in Azure</span><span class="sxs-lookup"><span data-stu-id="a14f5-103">Implement a hub-spoke network topology in Azure</span></span>

<span data-ttu-id="a14f5-104">Diese Referenzarchitektur zeigt, wie eine Hub-Spoke-Topologie in Azure implementiert wird.</span><span class="sxs-lookup"><span data-stu-id="a14f5-104">This reference architecture shows how to implement a hub-spoke topology in Azure.</span></span> <span data-ttu-id="a14f5-105">Bei einem *Hub* handelt es sich um ein virtuelles Netzwerk (VNET) in Azure, das als zentraler Konnektivitätspunkt für Ihr lokales Netzwerk fungiert.</span><span class="sxs-lookup"><span data-stu-id="a14f5-105">The *hub* is a virtual network (VNet) in Azure that acts as a central point of connectivity to your on-premises network.</span></span> <span data-ttu-id="a14f5-106">*Spokes* sind VNETs, die eine Peeringverbindung mit dem Hub herstellen und zur Isolierung von Workloads verwendet werden können.</span><span class="sxs-lookup"><span data-stu-id="a14f5-106">The *spokes* are VNets that peer with the hub, and can be used to isolate workloads.</span></span> <span data-ttu-id="a14f5-107">Der Datenverkehr wird über eine ExpressRoute- oder VPN-Gatewayverbindung zwischen dem lokalen Rechenzentrum und dem Hub weitergeleitet.</span><span class="sxs-lookup"><span data-stu-id="a14f5-107">Traffic flows between the on-premises datacenter and the hub through an ExpressRoute or VPN gateway connection.</span></span> <span data-ttu-id="a14f5-108">[**Stellen Sie diese Lösung bereit**](#deploy-the-solution).</span><span class="sxs-lookup"><span data-stu-id="a14f5-108">[**Deploy this solution**](#deploy-the-solution).</span></span>

<span data-ttu-id="a14f5-109">![[0]][0]</span><span class="sxs-lookup"><span data-stu-id="a14f5-109">![[0]][0]</span></span>

<span data-ttu-id="a14f5-110">*Laden Sie eine [Visio-Datei][visio-download] mit dieser Architektur herunter.*</span><span class="sxs-lookup"><span data-stu-id="a14f5-110">*Download a [Visio file][visio-download] of this architecture*</span></span>

<span data-ttu-id="a14f5-111">Diese Topologie bietet unter anderem folgende Vorteile:</span><span class="sxs-lookup"><span data-stu-id="a14f5-111">The benefits of this toplogy include:</span></span>

- <span data-ttu-id="a14f5-112">**Kosteneinsparungen** durch die Zentralisierung von Diensten, die von mehreren Workloads (z.B. Network Virtual Appliances, kurz NVAs, und DNS-Servern) an einem einzigen Standort gemeinsam genutzt werden können</span><span class="sxs-lookup"><span data-stu-id="a14f5-112">**Cost savings** by centralizing services that can be shared by multiple workloads, such as network virtual appliances (NVAs) and DNS servers, in a single location.</span></span>
- <span data-ttu-id="a14f5-113">**Umgehung von Abonnementbeschränkungen** durch die Herstellung von Peeringverbindungen zwischen VNETs verschiedener Abonnements und dem zentralen Hub</span><span class="sxs-lookup"><span data-stu-id="a14f5-113">**Overcome subscriptions limits** by peering VNets from different subscriptions to the central hub.</span></span>
- <span data-ttu-id="a14f5-114">**Trennung der Belange** zwischen zentralen IT-Vorgängen (SecOps, InfraOps) und Workloads (DevOps)</span><span class="sxs-lookup"><span data-stu-id="a14f5-114">**Separation of concerns** between central IT (SecOps, InfraOps) and workloads (DevOps).</span></span>

<span data-ttu-id="a14f5-115">Typische Einsatzmöglichkeiten für diese Architektur sind Folgende:</span><span class="sxs-lookup"><span data-stu-id="a14f5-115">Typical uses for this architecture include:</span></span>

- <span data-ttu-id="a14f5-116">Workloads, die in verschiedenen Umgebungen wie Entwicklungs-, Test- und Produktionsumgebungen eingesetzt werden und gemeinsame Dienste wie DNS, IDS, NTP oder AD DS erfordern</span><span class="sxs-lookup"><span data-stu-id="a14f5-116">Workloads deployed in different environments, such as development, testing, and production, that require shared services such as DNS, IDS, NTP, or AD DS.</span></span> <span data-ttu-id="a14f5-117">Gemeinsame Dienste werden im Hub-VNET platziert, während die einzelnen Umgebungen in einem Spoke bereitgestellt werden, um die Isolation beizubehalten.</span><span class="sxs-lookup"><span data-stu-id="a14f5-117">Shared services are placed in the hub VNet, while each environment is deployed to a spoke to maintain isolation.</span></span>
- <span data-ttu-id="a14f5-118">Workloads, bei denen keine Konnektivität untereinander bestehen muss, die jedoch Zugriff auf gemeinsame Dienste erfordern</span><span class="sxs-lookup"><span data-stu-id="a14f5-118">Workloads that do not require connectivity to each other, but require access to shared services.</span></span>
- <span data-ttu-id="a14f5-119">Unternehmen, die auf eine zentrale Steuerung von Sicherheitsmechanismen (z.B. eine Firewall im Hub als DMZ) und eine getrennte Verwaltung von Workloads in den einzelnen Spokes angewiesen sind</span><span class="sxs-lookup"><span data-stu-id="a14f5-119">Enterprises that require central control over security aspects, such as a firewall in the hub as a DMZ, and segregated management for the workloads in each spoke.</span></span>

## <a name="architecture"></a><span data-ttu-id="a14f5-120">Architecture</span><span class="sxs-lookup"><span data-stu-id="a14f5-120">Architecture</span></span>

<span data-ttu-id="a14f5-121">Die Architektur umfasst die folgenden Komponenten.</span><span class="sxs-lookup"><span data-stu-id="a14f5-121">The architecture consists of the following components.</span></span>

- <span data-ttu-id="a14f5-122">**Lokales Netzwerk**.</span><span class="sxs-lookup"><span data-stu-id="a14f5-122">**On-premises network**.</span></span> <span data-ttu-id="a14f5-123">Ein in einer Organisation betriebenes privates lokales Netzwerk.</span><span class="sxs-lookup"><span data-stu-id="a14f5-123">A private local-area network running within an organization.</span></span>

- <span data-ttu-id="a14f5-124">**VPN-Gerät**:</span><span class="sxs-lookup"><span data-stu-id="a14f5-124">**VPN device**.</span></span> <span data-ttu-id="a14f5-125">Ein Gerät oder ein Dienst, der externe Konnektivität mit dem lokalen Netzwerk bereitstellt.</span><span class="sxs-lookup"><span data-stu-id="a14f5-125">A device or service that provides external connectivity to the on-premises network.</span></span> <span data-ttu-id="a14f5-126">Bei dem VPN-Gerät kann es sich um ein Hardwaregerät oder eine Softwarelösung wie den Routing- und RAS-Dienst (RRAS) unter Windows Server 2012 handeln.</span><span class="sxs-lookup"><span data-stu-id="a14f5-126">The VPN device may be a hardware device, or a software solution such as the Routing and Remote Access Service (RRAS) in Windows Server 2012.</span></span> <span data-ttu-id="a14f5-127">Eine Liste der unterstützten VPN-Appliances und Informationen zur Konfiguration ausgewählter VPN-Geräte für die Verbindung mit Azure finden Sie unter [Informationen zu VPN-Geräten und IPsec-/IKE-Parametern für VPN-Gatewayverbindungen zwischen Standorten][vpn-appliance].</span><span class="sxs-lookup"><span data-stu-id="a14f5-127">For a list of supported VPN appliances and information on configuring selected VPN appliances for connecting to Azure, see [About VPN devices for Site-to-Site VPN Gateway connections][vpn-appliance].</span></span>

- <span data-ttu-id="a14f5-128">**VPN-Gateway für ein virtuelles Netzwerk oder ExpressRoute-Gateway**:</span><span class="sxs-lookup"><span data-stu-id="a14f5-128">**VPN virtual network gateway or ExpressRoute gateway**.</span></span> <span data-ttu-id="a14f5-129">Über das Gateway für virtuelle Netzwerke kann das VNET zur Konnektivität mit Ihrem lokalen Netzwerk eine Verbindung mit dem VPN-Gerät oder eine ExpressRoute-Verbindung herstellen.</span><span class="sxs-lookup"><span data-stu-id="a14f5-129">The virtual network gateway enables the VNet to connect to the VPN device, or ExpressRoute circuit, used for connectivity with your on-premises network.</span></span> <span data-ttu-id="a14f5-130">Weitere Informationen finden Sie unter [Verbinden eines lokalen Netzwerks mit einem virtuellen Microsoft Azure-Netzwerk][connect-to-an-Azure-vnet].</span><span class="sxs-lookup"><span data-stu-id="a14f5-130">For more information, see [Connect an on-premises network to a Microsoft Azure virtual network][connect-to-an-Azure-vnet].</span></span>

> [!NOTE]
> <span data-ttu-id="a14f5-131">In den Bereitstellungsskripts für diese Referenzarchitektur werden ein VPN-Gateway für die Konnektivität und ein VNET in Azure verwendet, um Ihr lokales Netzwerk zu simulieren.</span><span class="sxs-lookup"><span data-stu-id="a14f5-131">The deployment scripts for this reference architecture use a VPN gateway for connectivity, and a VNet in Azure to simulate your on-premises network.</span></span>

- <span data-ttu-id="a14f5-132">**Hub-VNET**:</span><span class="sxs-lookup"><span data-stu-id="a14f5-132">**Hub VNet**.</span></span> <span data-ttu-id="a14f5-133">Das Azure-VNET, das als Hub in der Hub-Spoke-Topologie verwendet wird.</span><span class="sxs-lookup"><span data-stu-id="a14f5-133">Azure VNet used as the hub in the hub-spoke topology.</span></span> <span data-ttu-id="a14f5-134">Der Hub ist der zentrale Konnektivitätspunkt für Ihr lokales Netzwerk, mit dem Sie Dienste hosten, die von den verschiedenen in den Spoke-VNETs gehosteten Workloads genutzt werden können.</span><span class="sxs-lookup"><span data-stu-id="a14f5-134">The hub is the central point of connectivity to your on-premises network, and a place to host services that can be consumed by the different workloads hosted in the spoke VNets.</span></span>

- <span data-ttu-id="a14f5-135">**Gatewaysubnetz**:</span><span class="sxs-lookup"><span data-stu-id="a14f5-135">**Gateway subnet**.</span></span> <span data-ttu-id="a14f5-136">Die Gateways für virtuelle Netzwerke befinden sich in demselben Subnetz.</span><span class="sxs-lookup"><span data-stu-id="a14f5-136">The virtual network gateways are held in the same subnet.</span></span>

- <span data-ttu-id="a14f5-137">**Spoke-VNETs**:</span><span class="sxs-lookup"><span data-stu-id="a14f5-137">**Spoke VNets**.</span></span> <span data-ttu-id="a14f5-138">Ein oder mehrere Azure-VNETs, die als Spokes in der Hub-Spoke-Topologie verwendet werden.</span><span class="sxs-lookup"><span data-stu-id="a14f5-138">One or more Azure VNets that are used as spokes in the hub-spoke topology.</span></span> <span data-ttu-id="a14f5-139">Spokes können verwendet werden, um Workloads in ihren eigenen VNETs, die getrennt von anderen Spokes verwaltet werden, zu isolieren.</span><span class="sxs-lookup"><span data-stu-id="a14f5-139">Spokes can be used to isolate workloads in their own VNets, managed separately from other spokes.</span></span> <span data-ttu-id="a14f5-140">Jede Workload kann mehrere Schichten umfassen, wobei mehrere Subnetze über Azure Load Balancer verbunden sind.</span><span class="sxs-lookup"><span data-stu-id="a14f5-140">Each workload might include multiple tiers, with multiple subnets connected through Azure load balancers.</span></span> <span data-ttu-id="a14f5-141">Weitere Informationen zur Anwendungsinfrastruktur finden Sie unter [Ausführen von Windows-VM-Workloads][windows-vm-ra] und [Ausführen von Linux-VM-Workloads][linux-vm-ra].</span><span class="sxs-lookup"><span data-stu-id="a14f5-141">For more information about the application infrastructure, see [Running Windows VM workloads][windows-vm-ra] and [Running Linux VM workloads][linux-vm-ra].</span></span>

- <span data-ttu-id="a14f5-142">**VNET-Peering**:</span><span class="sxs-lookup"><span data-stu-id="a14f5-142">**VNet peering**.</span></span> <span data-ttu-id="a14f5-143">Zwei VNETs können über eine [Peeringverbindung][vnet-peering] miteinander verbunden werden.</span><span class="sxs-lookup"><span data-stu-id="a14f5-143">Two VNets can be connected using a [peering connection][vnet-peering].</span></span> <span data-ttu-id="a14f5-144">Peeringverbindungen sind nicht-transitive Verbindungen zwischen VNETs mit niedrigen Latenzen.</span><span class="sxs-lookup"><span data-stu-id="a14f5-144">Peering connections are non-transitive, low latency connections between VNets.</span></span> <span data-ttu-id="a14f5-145">Sobald eine Peeringverbindung hergestellt wurde, tauschen die VNETs ohne Einsatz eines Routers Datenverkehr über den Azure-Backbone aus.</span><span class="sxs-lookup"><span data-stu-id="a14f5-145">Once peered, the VNets exchange traffic by using the Azure backbone, without the need for a router.</span></span> <span data-ttu-id="a14f5-146">In einer Hub-Spoke-Netzwerktopologie wird durch VNET-Peering eine Verbindung zwischen dem Hub und den einzelnen Spokes hergestellt.</span><span class="sxs-lookup"><span data-stu-id="a14f5-146">In a hub-spoke network topology, you use VNet peering to connect the hub to each spoke.</span></span> <span data-ttu-id="a14f5-147">Sie können virtuelle Netzwerke in derselben Region oder in verschiedenen Regionen per Peering verknüpfen.</span><span class="sxs-lookup"><span data-stu-id="a14f5-147">You can peer virtual networks in the same region, or different regions.</span></span> <span data-ttu-id="a14f5-148">Weitere Informationen finden Sie unter [Anforderungen und Einschränkungen][vnet-peering-requirements].</span><span class="sxs-lookup"><span data-stu-id="a14f5-148">For more information, see [Requirements and constraints][vnet-peering-requirements].</span></span>

> [!NOTE]
> <span data-ttu-id="a14f5-149">In diesem Artikel werden ausschließlich [Resource Manager](/azure/azure-resource-manager/resource-group-overview)-Bereitstellungen behandelt, Sie können jedoch auch eine Verbindung zwischen einem klassischen VNET und einem Resource Manager-VNET im selben Abonnement herstellen.</span><span class="sxs-lookup"><span data-stu-id="a14f5-149">This article only covers [Resource Manager](/azure/azure-resource-manager/resource-group-overview) deployments, but you can also connect a classic VNet to a Resource Manager VNet in the same subscription.</span></span> <span data-ttu-id="a14f5-150">Auf diese Weise können Ihre Spokes klassische Bereitstellungen hosten und trotzdem von gemeinsamen Diensten im Hub profitieren.</span><span class="sxs-lookup"><span data-stu-id="a14f5-150">That way, your spokes can host classic deployments and still benefit from services shared in the hub.</span></span>

## <a name="recommendations"></a><span data-ttu-id="a14f5-151">Empfehlungen</span><span class="sxs-lookup"><span data-stu-id="a14f5-151">Recommendations</span></span>

<span data-ttu-id="a14f5-152">Die folgenden Empfehlungen gelten für die meisten Szenarios.</span><span class="sxs-lookup"><span data-stu-id="a14f5-152">The following recommendations apply for most scenarios.</span></span> <span data-ttu-id="a14f5-153">Sofern Sie keine besonderen Anforderungen haben, die Vorrang haben, sollten Sie diese Empfehlungen befolgen.</span><span class="sxs-lookup"><span data-stu-id="a14f5-153">Follow these recommendations unless you have a specific requirement that overrides them.</span></span>

### <a name="resource-groups"></a><span data-ttu-id="a14f5-154">Ressourcengruppen</span><span class="sxs-lookup"><span data-stu-id="a14f5-154">Resource groups</span></span>

<span data-ttu-id="a14f5-155">Das Hub-VNET und die einzelnen Spoke-VNETS können in verschiedenen Ressourcengruppen und sogar in verschiedenen Abonnements implementiert werden.</span><span class="sxs-lookup"><span data-stu-id="a14f5-155">The hub VNet, and each spoke VNet, can be implemented in different resource groups, and even different subscriptions.</span></span> <span data-ttu-id="a14f5-156">Wenn Sie eine Peerverbindung zwischen virtuellen Netzwerken in verschiedenen Abonnements herstellen, können beide Abonnements demselben oder einem anderen Azure Active Directory-Mandanten zugeordnet sein.</span><span class="sxs-lookup"><span data-stu-id="a14f5-156">When you peer virtual networks in different subscriptions, both subscriptions can be associated to the same or different Azure Active Directory tenant.</span></span> <span data-ttu-id="a14f5-157">Dies ermöglicht nicht nur eine dezentralisierte Verwaltung der einzelnen Workloads, sondern auch die Verwaltung gemeinsamer Dienste im Hub-VNET.</span><span class="sxs-lookup"><span data-stu-id="a14f5-157">This allows for a decentralized management of each workload, while sharing services maintained in the hub VNet.</span></span>

### <a name="vnet-and-gatewaysubnet"></a><span data-ttu-id="a14f5-158">VNET und GatewaySubnet</span><span class="sxs-lookup"><span data-stu-id="a14f5-158">VNet and GatewaySubnet</span></span>

<span data-ttu-id="a14f5-159">Erstellen Sie ein Subnetz mit dem Namen *GatewaySubnet* mit dem Adressbereich /27.</span><span class="sxs-lookup"><span data-stu-id="a14f5-159">Create a subnet named *GatewaySubnet*, with an address range of /27.</span></span> <span data-ttu-id="a14f5-160">Dieses Subnetz ist für das Gateway für virtuelle Netzwerke erforderlich.</span><span class="sxs-lookup"><span data-stu-id="a14f5-160">This subnet is required by the virtual network gateway.</span></span> <span data-ttu-id="a14f5-161">Durch Zuweisung von 32 Adressen zu diesem Subnetz wird eine künftige Überschreitung von Beschränkungen der Gatewaygröße verhindert.</span><span class="sxs-lookup"><span data-stu-id="a14f5-161">Allocating 32 addresses to this subnet will help to prevent reaching gateway size limitations in the future.</span></span>

<span data-ttu-id="a14f5-162">Weitere Informationen zum Einrichten des Gateways finden Sie abhängig von Ihrem Verbindungstyp in den folgenden Referenzarchitekturen:</span><span class="sxs-lookup"><span data-stu-id="a14f5-162">For more information about setting up the gateway, see the following reference architectures, depending on your connection type:</span></span>

- <span data-ttu-id="a14f5-163">[Hybrides Netzwerk mit ExpressRoute][guidance-expressroute]</span><span class="sxs-lookup"><span data-stu-id="a14f5-163">[Hybrid network using ExpressRoute][guidance-expressroute]</span></span>
- <span data-ttu-id="a14f5-164">[Hybrides Netzwerk mit einem VPN-Gateway][guidance-vpn]</span><span class="sxs-lookup"><span data-stu-id="a14f5-164">[Hybrid network using a VPN gateway][guidance-vpn]</span></span>

<span data-ttu-id="a14f5-165">Um eine höhere Verfügbarkeit zu erzielen, können Sie ExpressRoute mit einem VPN für Failoverzwecke kombinieren.</span><span class="sxs-lookup"><span data-stu-id="a14f5-165">For higher availability, you can use ExpressRoute plus a VPN for failover.</span></span> <span data-ttu-id="a14f5-166">Weitere Informationen finden Sie unter [Verbinden eines lokalen Netzwerks mit Azure unter Verwendung von ExpressRoute mit VPN-Failover][hybrid-ha].</span><span class="sxs-lookup"><span data-stu-id="a14f5-166">See [Connect an on-premises network to Azure using ExpressRoute with VPN failover][hybrid-ha].</span></span>

<span data-ttu-id="a14f5-167">Eine Hub-Spoke-Topologie kann auch ohne Gateway verwendet werden, wenn keine Konnektivität mit Ihrem lokalen Netzwerk erforderlich ist.</span><span class="sxs-lookup"><span data-stu-id="a14f5-167">A hub-spoke topology can also be used without a gateway, if you don't need connectivity with your on-premises network.</span></span>

### <a name="vnet-peering"></a><span data-ttu-id="a14f5-168">VNet-Peering</span><span class="sxs-lookup"><span data-stu-id="a14f5-168">VNet peering</span></span>

<span data-ttu-id="a14f5-169">Beim VNET-Peering wird eine nicht-transitive Beziehung zwischen zwei VNETs hergestellt.</span><span class="sxs-lookup"><span data-stu-id="a14f5-169">VNet peering is a non-transitive relationship between two VNets.</span></span> <span data-ttu-id="a14f5-170">Wenn Sie eine Verbindung zwischen Spokes herstellen möchten, sollten Sie eventuell eine separate Peeringverbindung zwischen diesen Spokes herstellen.</span><span class="sxs-lookup"><span data-stu-id="a14f5-170">If you require spokes to connect to each other, consider adding a separate peering connection between those spokes.</span></span>

<span data-ttu-id="a14f5-171">Wenn jedoch zwischen mehreren Spokes eine Verbindung hergestellt werden muss, werden die möglichen Peeringverbindungen sehr schnell zur Neige gehen, da [die Anzahl der VNET-Peerings pro VNET begrenzt ist][vnet-peering-limit].</span><span class="sxs-lookup"><span data-stu-id="a14f5-171">However, if you have several spokes that need to connect with each other, you will run out of possible peering connections very quickly due to the [limitation on number of VNets peerings per VNet][vnet-peering-limit].</span></span> <span data-ttu-id="a14f5-172">In diesem Szenario sollten Sie die Verwendung von benutzerdefinierten Routen (User Defined Routes, UDRs) in Erwägung ziehen, um zu erzwingen, dass der für einen Spoke vorgesehene Datenverkehr an eine NVA, die als Router im Hub-VNET fungiert, gesendet wird.</span><span class="sxs-lookup"><span data-stu-id="a14f5-172">In this scenario, consider using user defined routes (UDRs) to force traffic destined to a spoke to be sent to an NVA acting as a router at the hub VNet.</span></span> <span data-ttu-id="a14f5-173">Hierdurch können die Spokes miteinander verbunden werden.</span><span class="sxs-lookup"><span data-stu-id="a14f5-173">This will allow the spokes to connect to each other.</span></span>

<span data-ttu-id="a14f5-174">Sie können Spokes auch für die Kommunikation mit Remotenetzwerken über das Gateway für das Hub-VNET konfigurieren.</span><span class="sxs-lookup"><span data-stu-id="a14f5-174">You can also configure spokes to use the hub VNet gateway to communicate with remote networks.</span></span> <span data-ttu-id="a14f5-175">Damit der Gatewaydatenverkehr zwischen den Spokes und dem Hub weitergeleitet und eine Verbindung mit Remotenetzwerken hergestellt werden kann, müssen Sie folgende Schritte durchführen:</span><span class="sxs-lookup"><span data-stu-id="a14f5-175">To allow gateway traffic to flow from spoke to hub, and connect to remote networks, you must:</span></span>

- <span data-ttu-id="a14f5-176">Konfigurieren Sie die VNET-Peeringverbindung im Hub dahingehend, dass **Gatewaytransit zugelassen** wird.</span><span class="sxs-lookup"><span data-stu-id="a14f5-176">Configure the VNet peering connection in the hub to **allow gateway transit**.</span></span>
- <span data-ttu-id="a14f5-177">Konfigurieren Sie die VNET-Peeringverbindung in den einzelnen Spokes dahingehend, dass **Remotegateways verwendet werden**.</span><span class="sxs-lookup"><span data-stu-id="a14f5-177">Configure the VNet peering connection in each spoke to **use remote gateways**.</span></span>
- <span data-ttu-id="a14f5-178">Konfigurieren Sie alle VNET-Peeringverbindungen dahingehend, dass **weitergeleiteter Datenverkehr zugelassen** wird.</span><span class="sxs-lookup"><span data-stu-id="a14f5-178">Configure all VNet peering connections to **allow forwarded traffic**.</span></span>

## <a name="considerations"></a><span data-ttu-id="a14f5-179">Überlegungen</span><span class="sxs-lookup"><span data-stu-id="a14f5-179">Considerations</span></span>

### <a name="spoke-connectivity"></a><span data-ttu-id="a14f5-180">Konnektivität zwischen Spokes</span><span class="sxs-lookup"><span data-stu-id="a14f5-180">Spoke connectivity</span></span>

<span data-ttu-id="a14f5-181">Wenn Konnektivität zwischen Spokes hergestellt werden muss, sollten Sie eine NVA für das Routing im Hub implementieren und UDRs im Spoke zur Weiterleitung des Datenverkehrs an den Hub verwenden.</span><span class="sxs-lookup"><span data-stu-id="a14f5-181">If you require connectivity between spokes, consider implementing an NVA for routing in the hub, and using UDRs in the spoke to forward traffic to the hub.</span></span>

<span data-ttu-id="a14f5-182">![[2]][2]</span><span class="sxs-lookup"><span data-stu-id="a14f5-182">![[2]][2]</span></span>

<span data-ttu-id="a14f5-183">In diesem Szenario müssen Sie die Peeringverbindungen dahingehend konfigurieren, dass **weitergeleiteter Verkehr zugelassen** wird.</span><span class="sxs-lookup"><span data-stu-id="a14f5-183">In this scenario, you must configure the peering connections to **allow forwarded traffic**.</span></span>

### <a name="overcoming-vnet-peering-limits"></a><span data-ttu-id="a14f5-184">Umgehung von VNET-Peeringbeschränkungen</span><span class="sxs-lookup"><span data-stu-id="a14f5-184">Overcoming VNet peering limits</span></span>

<span data-ttu-id="a14f5-185">Stellen Sie sicher, dass Sie die [Begrenzung der Anzahl von VNET-Peerings pro VNET][vnet-peering-limit] in Azure einhalten.</span><span class="sxs-lookup"><span data-stu-id="a14f5-185">Make sure you consider the [limitation on number of VNets peerings per VNet][vnet-peering-limit] in Azure.</span></span> <span data-ttu-id="a14f5-186">Falls Sie mehr Spokes als die maximal zulässige Anzahl benötigen, sollten Sie eventuell eine Hub-Spoke-Hub-Spoke-Topologie erstellen, bei der die erste Ebene von Spokes auch als Hub fungiert.</span><span class="sxs-lookup"><span data-stu-id="a14f5-186">If you decide you need more spokes than the limit will allow, consider creating a hub-spoke-hub-spoke topology, where the first level of spokes also act as hubs.</span></span> <span data-ttu-id="a14f5-187">Im folgenden Diagramm wird diese Vorgehensweise veranschaulicht.</span><span class="sxs-lookup"><span data-stu-id="a14f5-187">The following diagram shows this approach.</span></span>

<span data-ttu-id="a14f5-188">![[3]][3]</span><span class="sxs-lookup"><span data-stu-id="a14f5-188">![[3]][3]</span></span>

<span data-ttu-id="a14f5-189">Berücksichtigen Sie auch die Dienste, die im Hub gemeinsam genutzt werden, um sicherzustellen, dass sich der Hub für eine größere Anzahl von Spokes skalieren lässt.</span><span class="sxs-lookup"><span data-stu-id="a14f5-189">Also consider what services are shared in the hub, to ensure the hub scales for a larger number of spokes.</span></span> <span data-ttu-id="a14f5-190">Wenn Ihr Hub beispielsweise Firewalldienste bereitstellt, sollten Sie beim Hinzufügen mehrerer Spokes die Bandbreitenbeschränkungen Ihrer Firewalllösung berücksichtigen.</span><span class="sxs-lookup"><span data-stu-id="a14f5-190">For instance, if your hub provides firewall services, consider the bandwidth limits of your firewall solution when adding multiple spokes.</span></span> <span data-ttu-id="a14f5-191">Es wird empfohlen, einige dieser gemeinsamen Dienste auf eine zweite Hubebene zu verlagern.</span><span class="sxs-lookup"><span data-stu-id="a14f5-191">You might want to move some of these shared services to a second level of hubs.</span></span>

## <a name="deploy-the-solution"></a><span data-ttu-id="a14f5-192">Bereitstellen der Lösung</span><span class="sxs-lookup"><span data-stu-id="a14f5-192">Deploy the solution</span></span>

<span data-ttu-id="a14f5-193">Eine Bereitstellung für diese Architektur ist auf [GitHub][ref-arch-repo] verfügbar.</span><span class="sxs-lookup"><span data-stu-id="a14f5-193">A deployment for this architecture is available on [GitHub][ref-arch-repo].</span></span> <span data-ttu-id="a14f5-194">Bei dieser werden zum Testen der Konnektivität VMs in jedem VNET verwendet.</span><span class="sxs-lookup"><span data-stu-id="a14f5-194">It uses VMs in each VNet to test connectivity.</span></span> <span data-ttu-id="a14f5-195">Es gibt keine tatsächlichen Dienste, die im Subnetz für **gemeinsame Dienste** im **Hub-VNET** gehostet werden.</span><span class="sxs-lookup"><span data-stu-id="a14f5-195">There are no actual services hosted in the **shared-services** subnet in the **hub VNet**.</span></span>

<span data-ttu-id="a14f5-196">Die Bereitstellung erstellt die folgenden Ressourcengruppen in Ihrem Abonnement:</span><span class="sxs-lookup"><span data-stu-id="a14f5-196">The deployment creates the following resource groups in your subscription:</span></span>

- <span data-ttu-id="a14f5-197">hub-nva-rg</span><span class="sxs-lookup"><span data-stu-id="a14f5-197">hub-nva-rg</span></span>
- <span data-ttu-id="a14f5-198">hub-vnet-rg</span><span class="sxs-lookup"><span data-stu-id="a14f5-198">hub-vnet-rg</span></span>
- <span data-ttu-id="a14f5-199">onprem-jb-rg</span><span class="sxs-lookup"><span data-stu-id="a14f5-199">onprem-jb-rg</span></span>
- <span data-ttu-id="a14f5-200">onprem-vnet-rg</span><span class="sxs-lookup"><span data-stu-id="a14f5-200">onprem-vnet-rg</span></span>
- <span data-ttu-id="a14f5-201">spoke1-vnet-rg</span><span class="sxs-lookup"><span data-stu-id="a14f5-201">spoke1-vnet-rg</span></span>
- <span data-ttu-id="a14f5-202">spoke2-vent-rg</span><span class="sxs-lookup"><span data-stu-id="a14f5-202">spoke2-vent-rg</span></span>

<span data-ttu-id="a14f5-203">Die Vorlagenparameterdateien beziehen sich auf diese Namen, d.h. wenn Sie sie ändern, müssen Sie die Parameterdateien entsprechend aktualisieren.</span><span class="sxs-lookup"><span data-stu-id="a14f5-203">The template parameter files refer to these names, so if you change them, update the parameter files to match.</span></span>

### <a name="prerequisites"></a><span data-ttu-id="a14f5-204">Voraussetzungen</span><span class="sxs-lookup"><span data-stu-id="a14f5-204">Prerequisites</span></span>

[!INCLUDE [ref-arch-prerequisites.md](../../../includes/ref-arch-prerequisites.md)]

### <a name="deploy-the-simulated-on-premises-datacenter"></a><span data-ttu-id="a14f5-205">Bereitstellen des simulierten lokalen Rechenzentrums</span><span class="sxs-lookup"><span data-stu-id="a14f5-205">Deploy the simulated on-premises datacenter</span></span>

<span data-ttu-id="a14f5-206">Führen Sie die folgenden Schritte aus, um das simulierte lokale Rechenzentrum als Azure-VNET bereitzustellen:</span><span class="sxs-lookup"><span data-stu-id="a14f5-206">To deploy the simulated on-premises datacenter as an Azure VNet, follow these steps:</span></span>

1. <span data-ttu-id="a14f5-207">Navigieren Sie zum `hybrid-networking/hub-spoke`-Ordner des Repositorys für Referenzarchitekturen.</span><span class="sxs-lookup"><span data-stu-id="a14f5-207">Navigate to the `hybrid-networking/hub-spoke` folder of the reference architectures repository.</span></span>

2. <span data-ttu-id="a14f5-208">Öffnen Sie die Datei `onprem.json` .</span><span class="sxs-lookup"><span data-stu-id="a14f5-208">Open the `onprem.json` file.</span></span> <span data-ttu-id="a14f5-209">Ersetzen Sie die Werte für `adminUsername` und `adminPassword`.</span><span class="sxs-lookup"><span data-stu-id="a14f5-209">Replace the values for `adminUsername` and `adminPassword`.</span></span>

    ```json
    "adminUsername": "<user name>",
    "adminPassword": "<password>",
    ```

3. <span data-ttu-id="a14f5-210">(Optional) Legen Sie für eine Linux-Bereitstellung `osType` auf `Linux` fest.</span><span class="sxs-lookup"><span data-stu-id="a14f5-210">(Optional) For a Linux deployment, set `osType` to `Linux`.</span></span>

4. <span data-ttu-id="a14f5-211">Führen Sie den folgenden Befehl aus:</span><span class="sxs-lookup"><span data-stu-id="a14f5-211">Run the following command:</span></span>

    ```bash
    azbb -s <subscription_id> -g onprem-vnet-rg -l <location> -p onprem.json --deploy
    ```

5. <span data-ttu-id="a14f5-212">Warten Sie, bis die Bereitstellung abgeschlossen ist.</span><span class="sxs-lookup"><span data-stu-id="a14f5-212">Wait for the deployment to finish.</span></span> <span data-ttu-id="a14f5-213">Bei dieser Bereitstellung werden ein virtuelles Netzwerk, ein virtueller Computer und ein VPN-Gateway erstellt.</span><span class="sxs-lookup"><span data-stu-id="a14f5-213">This deployment creates a virtual network, a virtual machine, and a VPN gateway.</span></span> <span data-ttu-id="a14f5-214">Das Erstellen des VPN-Gateways kann etwa 40 Minuten dauern.</span><span class="sxs-lookup"><span data-stu-id="a14f5-214">It can take about 40 minutes to create the VPN gateway.</span></span>

### <a name="deploy-the-hub-vnet"></a><span data-ttu-id="a14f5-215">Bereitstellen des Hub-VNet</span><span class="sxs-lookup"><span data-stu-id="a14f5-215">Deploy the hub VNet</span></span>

<span data-ttu-id="a14f5-216">Führen Sie die folgenden Schritte aus, um das Hub-VNet bereitzustellen.</span><span class="sxs-lookup"><span data-stu-id="a14f5-216">To deploy the hub VNet, perform the following steps.</span></span>

1. <span data-ttu-id="a14f5-217">Öffnen Sie die Datei `hub-vnet.json` .</span><span class="sxs-lookup"><span data-stu-id="a14f5-217">Open the `hub-vnet.json` file.</span></span> <span data-ttu-id="a14f5-218">Ersetzen Sie die Werte für `adminUsername` und `adminPassword`.</span><span class="sxs-lookup"><span data-stu-id="a14f5-218">Replace the values for `adminUsername` and `adminPassword`.</span></span>

    ```json
    "adminUsername": "<user name>",
    "adminPassword": "<password>",
    ```

2. <span data-ttu-id="a14f5-219">(Optional) Legen Sie für eine Linux-Bereitstellung `osType` auf `Linux` fest.</span><span class="sxs-lookup"><span data-stu-id="a14f5-219">(Optional) For a Linux deployment, set `osType` to `Linux`.</span></span>

3. <span data-ttu-id="a14f5-220">Suchen Sie beide Instanzen von `sharedKey`, und geben Sie einen gemeinsam genutzten Schlüssel für die VPN-Verbindung ein.</span><span class="sxs-lookup"><span data-stu-id="a14f5-220">Find both instances of `sharedKey` and enter a shared key for the VPN connection.</span></span> <span data-ttu-id="a14f5-221">Die Werte müssen übereinstimmen.</span><span class="sxs-lookup"><span data-stu-id="a14f5-221">The values must match.</span></span>

    ```json
    "sharedKey": "",
    ```

4. <span data-ttu-id="a14f5-222">Führen Sie den folgenden Befehl aus:</span><span class="sxs-lookup"><span data-stu-id="a14f5-222">Run the following command:</span></span>

    ```bash
    azbb -s <subscription_id> -g hub-vnet-rg -l <location> -p hub-vnet.json --deploy
    ```

5. <span data-ttu-id="a14f5-223">Warten Sie, bis die Bereitstellung abgeschlossen ist.</span><span class="sxs-lookup"><span data-stu-id="a14f5-223">Wait for the deployment to finish.</span></span> <span data-ttu-id="a14f5-224">Bei dieser Bereitstellung werden ein virtuelles Netzwerk, ein virtueller Computer, ein VPN-Gateway und eine Verbindung mit dem Gateway erstellt.</span><span class="sxs-lookup"><span data-stu-id="a14f5-224">This deployment creates a virtual network, a virtual machine, a VPN gateway, and a connection to the gateway.</span></span>  <span data-ttu-id="a14f5-225">Das Erstellen des VPN-Gateways kann etwa 40 Minuten dauern.</span><span class="sxs-lookup"><span data-stu-id="a14f5-225">It can take about 40 minutes to create the VPN gateway.</span></span>

### <a name="test-connectivity-to-the-hub-vnet-mdash-windows-deployment"></a><span data-ttu-id="a14f5-226">Testen der Konnektivität mit dem Hub-VNET – Windows-Bereitstellung</span><span class="sxs-lookup"><span data-stu-id="a14f5-226">Test connectivity to the hub VNet &mdash; Windows deployment</span></span>

<span data-ttu-id="a14f5-227">Führen Sie die folgenden Schritte aus, um die Konnektivität der simulierten lokalen Umgebung mit dem Hub-VNET mit virtuellen Windows-Computern zu testen:</span><span class="sxs-lookup"><span data-stu-id="a14f5-227">To test conectivity from the simulated on-premises environment to the hub VNet using Windows VMs, follow these steps:</span></span>

1. <span data-ttu-id="a14f5-228">Suchen Sie im Azure-Portal die VM namens `jb-vm1` in der `onprem-jb-rg`-Ressourcengruppe.</span><span class="sxs-lookup"><span data-stu-id="a14f5-228">Use the Azure portal to find the VM named `jb-vm1` in the `onprem-jb-rg` resource group.</span></span>

2. <span data-ttu-id="a14f5-229">Klicke Sie auf `Connect`, um eine Remotedesktopsitzung mit der VM zu öffnen.</span><span class="sxs-lookup"><span data-stu-id="a14f5-229">Click `Connect` to open a remote desktop session to the VM.</span></span> <span data-ttu-id="a14f5-230">Verwenden Sie das Kennwort, das Sie in der `onprem.json`-Parameterdatei angegeben haben.</span><span class="sxs-lookup"><span data-stu-id="a14f5-230">Use the password that you specified in the `onprem.json` parameter file.</span></span>

3. <span data-ttu-id="a14f5-231">Öffnen Sie auf der VM eine PowerShell-Konsole, und verwenden Sie das `Test-NetConnection`-Cmdlet, um sicherzustellen, dass Sie eine Verbindung mit der Jumpbox-VM im Hub-VNet herstellen können.</span><span class="sxs-lookup"><span data-stu-id="a14f5-231">Open a PowerShell console in the VM, and use the `Test-NetConnection` cmdlet to verify that you can connect to the jumpbox VM in the hub VNet.</span></span>

   ```powershell
   Test-NetConnection 10.0.0.68 -CommonTCPPort RDP
   ```

<span data-ttu-id="a14f5-232">Die Ausgabe sollte in etwa wie folgt aussehen:</span><span class="sxs-lookup"><span data-stu-id="a14f5-232">The output should look similar to the following:</span></span>

```powershell
ComputerName     : 10.0.0.68
RemoteAddress    : 10.0.0.68
RemotePort       : 3389
InterfaceAlias   : Ethernet 2
SourceAddress    : 192.168.1.000
TcpTestSucceeded : True
```

> [!NOTE]
> <span data-ttu-id="a14f5-233">Standardmäßig lassen Windows Server-VMs in Azure keine ICMP-Antworten zu.</span><span class="sxs-lookup"><span data-stu-id="a14f5-233">By default, Windows Server VMs do not allow ICMP responses in Azure.</span></span> <span data-ttu-id="a14f5-234">Wenn Sie `ping` zum Testen der Konnektivität nutzen möchten, müssen Sie für jede VM ICMP-Datenverkehr in der erweiterten Windows-Firewall aktivieren.</span><span class="sxs-lookup"><span data-stu-id="a14f5-234">If you want to use `ping` to test connectivity, you need to enable ICMP traffic in the Windows Advanced Firewall for each VM.</span></span>

### <a name="test-connectivity-to-the-hub-vnet-mdash-linux-deployment"></a><span data-ttu-id="a14f5-235">Testen der Konnektivität mit dem Hub-VNET – Linux-Bereitstellung</span><span class="sxs-lookup"><span data-stu-id="a14f5-235">Test connectivity to the hub VNet &mdash; Linux deployment</span></span>

<span data-ttu-id="a14f5-236">Führen Sie die folgenden Schritte aus, um die Konnektivität der simulierten lokalen Umgebung mit dem Hub-VNET mit virtuellen Linux-Computern zu testen:</span><span class="sxs-lookup"><span data-stu-id="a14f5-236">To test conectivity from the simulated on-premises environment to the hub VNet using Linux VMs, follow these steps:</span></span>

1. <span data-ttu-id="a14f5-237">Suchen Sie im Azure-Portal die VM namens `jb-vm1` in der `onprem-jb-rg`-Ressourcengruppe.</span><span class="sxs-lookup"><span data-stu-id="a14f5-237">Use the Azure portal to find the VM named `jb-vm1` in the `onprem-jb-rg` resource group.</span></span>

2. <span data-ttu-id="a14f5-238">Klicken Sie auf `Connect`, und kopieren Sie den im Portal angezeigten `ssh`-Befehl.</span><span class="sxs-lookup"><span data-stu-id="a14f5-238">Click `Connect` and copy the `ssh` command shown in the portal.</span></span>

3. <span data-ttu-id="a14f5-239">Führen Sie in einer Linux-Eingabeaufforderung `ssh` zum Herstellen der Verbindung mit der simulierten lokalen Umgebung aus.</span><span class="sxs-lookup"><span data-stu-id="a14f5-239">From a Linux prompt, run `ssh` to connect to the simulated on-premises environment.</span></span> <span data-ttu-id="a14f5-240">Verwenden Sie das Kennwort, das Sie in der `onprem.json`-Parameterdatei angegeben haben.</span><span class="sxs-lookup"><span data-stu-id="a14f5-240">Use the password that you specified in the `onprem.json` parameter file.</span></span>

4. <span data-ttu-id="a14f5-241">Verwenden Sie den Befehl `ping`, um die Konnektivität mit der Jumpbox-VM im Hub-VNet zu testen:</span><span class="sxs-lookup"><span data-stu-id="a14f5-241">Use the `ping` command to test connectivity to the jumpbox VM in the hub VNet:</span></span>

   ```shell
   ping 10.0.0.68
   ```

### <a name="deploy-the-spoke-vnets"></a><span data-ttu-id="a14f5-242">Bereitstellen der Spoke-VNets</span><span class="sxs-lookup"><span data-stu-id="a14f5-242">Deploy the spoke VNets</span></span>

<span data-ttu-id="a14f5-243">Führen Sie die folgenden Schritte aus, um die Spoke-VNETs bereitzustellen.</span><span class="sxs-lookup"><span data-stu-id="a14f5-243">To deploy the spoke VNets, perform the following steps.</span></span>

1. <span data-ttu-id="a14f5-244">Öffnen Sie die Datei `spoke1.json` .</span><span class="sxs-lookup"><span data-stu-id="a14f5-244">Open the `spoke1.json` file.</span></span> <span data-ttu-id="a14f5-245">Ersetzen Sie die Werte für `adminUsername` und `adminPassword`.</span><span class="sxs-lookup"><span data-stu-id="a14f5-245">Replace the values for `adminUsername` and `adminPassword`.</span></span>

    ```json
    "adminUsername": "<user name>",
    "adminPassword": "<password>",
    ```

2. <span data-ttu-id="a14f5-246">(Optional) Legen Sie für eine Linux-Bereitstellung `osType` auf `Linux` fest.</span><span class="sxs-lookup"><span data-stu-id="a14f5-246">(Optional) For a Linux deployment, set `osType` to `Linux`.</span></span>

3. <span data-ttu-id="a14f5-247">Führen Sie den folgenden Befehl aus:</span><span class="sxs-lookup"><span data-stu-id="a14f5-247">Run the following command:</span></span>

   ```bash
   azbb -s <subscription_id> -g spoke1-vnet-rg -l <location> -p spoke1.json --deploy
   ```

4. <span data-ttu-id="a14f5-248">Wiederholen Sie die Schritte 1 bis 2 für die `spoke2.json`-Datei.</span><span class="sxs-lookup"><span data-stu-id="a14f5-248">Repeat steps 1-2 for the `spoke2.json` file.</span></span>

5. <span data-ttu-id="a14f5-249">Führen Sie den folgenden Befehl aus:</span><span class="sxs-lookup"><span data-stu-id="a14f5-249">Run the following command:</span></span>

   ```bash
   azbb -s <subscription_id> -g spoke2-vnet-rg -l <location> -p spoke2.json --deploy
   ```

6. <span data-ttu-id="a14f5-250">Führen Sie den folgenden Befehl aus:</span><span class="sxs-lookup"><span data-stu-id="a14f5-250">Run the following command:</span></span>

   ```bash
   azbb -s <subscription_id> -g hub-vnet-rg -l <location> -p hub-vnet-peering.json --deploy
   ```

### <a name="test-connectivity-to-the-spoke-vnets-mdash-windows-deployment"></a><span data-ttu-id="a14f5-251">Testen der Konnektivität mit den Spoke-VNETs – Windows-Bereitstellung</span><span class="sxs-lookup"><span data-stu-id="a14f5-251">Test connectivity to the spoke VNets &mdash; Windows deployment</span></span>

<span data-ttu-id="a14f5-252">Führen Sie die folgenden Schritte aus, um die Konnektivität der simulierten lokalen Umgebung mit den Spoke-VNETs mit virtuellen Windows-Computern zu testen:</span><span class="sxs-lookup"><span data-stu-id="a14f5-252">To test conectivity from the simulated on-premises environment to the spoke VNets using Windows VMs, perform the following steps:</span></span>

1. <span data-ttu-id="a14f5-253">Suchen Sie im Azure-Portal die VM namens `jb-vm1` in der `onprem-jb-rg`-Ressourcengruppe.</span><span class="sxs-lookup"><span data-stu-id="a14f5-253">Use the Azure portal to find the VM named `jb-vm1` in the `onprem-jb-rg` resource group.</span></span>

2. <span data-ttu-id="a14f5-254">Klicke Sie auf `Connect`, um eine Remotedesktopsitzung mit der VM zu öffnen.</span><span class="sxs-lookup"><span data-stu-id="a14f5-254">Click `Connect` to open a remote desktop session to the VM.</span></span> <span data-ttu-id="a14f5-255">Verwenden Sie das Kennwort, das Sie in der `onprem.json`-Parameterdatei angegeben haben.</span><span class="sxs-lookup"><span data-stu-id="a14f5-255">Use the password that you specified in the `onprem.json` parameter file.</span></span>

3. <span data-ttu-id="a14f5-256">Öffnen Sie auf dem virtuellen Computer eine PowerShell-Konsole, und stellen Sie mithilfe des `Test-NetConnection`-Cmdlets sicher, dass Sie eine Verbindung mit dem virtuellen Jumpbox-Computer in den Spoke-VNETs herstellen können.</span><span class="sxs-lookup"><span data-stu-id="a14f5-256">Open a PowerShell console in the VM, and use the `Test-NetConnection` cmdlet to verify that you can connect to the jumpbox VMs in the spoke VNets.</span></span>

   ```powershell
   Test-NetConnection 10.1.0.68 -CommonTCPPort RDP
   Test-NetConnection 10.2.0.68 -CommonTCPPort RDP
   ```

### <a name="test-connectivity-to-the-spoke-vnets-mdash-linux-deployment"></a><span data-ttu-id="a14f5-257">Testen der Konnektivität mit den Spoke-VNETs – Linux-Bereitstellung</span><span class="sxs-lookup"><span data-stu-id="a14f5-257">Test connectivity to the spoke VNets &mdash; Linux deployment</span></span>

<span data-ttu-id="a14f5-258">Führen Sie die folgenden Schritte aus, um die Konnektivität der simulierten lokalen Umgebung mit den Spoke-VNETs mit Linux-VMs zu testen:</span><span class="sxs-lookup"><span data-stu-id="a14f5-258">To test conectivity from the simulated on-premises environment to the spoke VNets using Linux VMs, perform the following steps:</span></span>

1. <span data-ttu-id="a14f5-259">Suchen Sie im Azure-Portal die VM namens `jb-vm1` in der `onprem-jb-rg`-Ressourcengruppe.</span><span class="sxs-lookup"><span data-stu-id="a14f5-259">Use the Azure portal to find the VM named `jb-vm1` in the `onprem-jb-rg` resource group.</span></span>

2. <span data-ttu-id="a14f5-260">Klicken Sie auf `Connect`, und kopieren Sie den im Portal angezeigten `ssh`-Befehl.</span><span class="sxs-lookup"><span data-stu-id="a14f5-260">Click `Connect` and copy the `ssh` command shown in the portal.</span></span>

3. <span data-ttu-id="a14f5-261">Führen Sie in einer Linux-Eingabeaufforderung `ssh` zum Herstellen der Verbindung mit der simulierten lokalen Umgebung aus.</span><span class="sxs-lookup"><span data-stu-id="a14f5-261">From a Linux prompt, run `ssh` to connect to the simulated on-premises environment.</span></span> <span data-ttu-id="a14f5-262">Verwenden Sie das Kennwort, das Sie in der `onprem.json`-Parameterdatei angegeben haben.</span><span class="sxs-lookup"><span data-stu-id="a14f5-262">Use the password that you specified in the `onprem.json` parameter file.</span></span>

4. <span data-ttu-id="a14f5-263">Verwenden Sie den Befehl `ping`, um die Konnektivität mit den Jumpbox-VMs in den einzelnen Spokes zu testen:</span><span class="sxs-lookup"><span data-stu-id="a14f5-263">Use the `ping` command to test connectivity to the jumpbox VMs in each spoke:</span></span>

   ```bash
   ping 10.1.0.68
   ping 10.2.0.68
   ```

### <a name="add-connectivity-between-spokes"></a><span data-ttu-id="a14f5-264">Herstellen von Konnektivität zwischen Spokes</span><span class="sxs-lookup"><span data-stu-id="a14f5-264">Add connectivity between spokes</span></span>

<span data-ttu-id="a14f5-265">Dieser Schritt ist optional.</span><span class="sxs-lookup"><span data-stu-id="a14f5-265">This step is optional.</span></span> <span data-ttu-id="a14f5-266">Wenn Sie zulassen möchten, dass Spokes Verbindungen miteinander herstellen können, müssen Sie ein virtuelles Netzwerkgerät als Router im Hub-VNET verwenden. Außerdem müssen Sie erzwingen, dass Datenverkehr von den Spokes über den Router verläuft, wenn versucht wird, eine Verbindung mit anderen Spokes herzustellen.</span><span class="sxs-lookup"><span data-stu-id="a14f5-266">If you want to allow spokes to connect to each other, you must use a network virtual appliance (NVA) as a router in the hub VNet, and force traffic from spokes to the router when trying to connect to another spoke.</span></span> <span data-ttu-id="a14f5-267">Führen Sie die folgenden Schritte aus, um ein einfaches Beispiel-Netzwerkgerät als einzelne VM und benutzerdefinierte Routen bereitzustellen, um die Verbindungsherstellung zwischen den zwei Spoke-VNETs zu ermöglichen:</span><span class="sxs-lookup"><span data-stu-id="a14f5-267">To deploy a basic sample NVA as a single VM, along with user-defined routes (UDRs) to allow the two spoke VNets to connect, perform the following steps:</span></span>

1. <span data-ttu-id="a14f5-268">Öffnen Sie die Datei `hub-nva.json` .</span><span class="sxs-lookup"><span data-stu-id="a14f5-268">Open the `hub-nva.json` file.</span></span> <span data-ttu-id="a14f5-269">Ersetzen Sie die Werte für `adminUsername` und `adminPassword`.</span><span class="sxs-lookup"><span data-stu-id="a14f5-269">Replace the values for `adminUsername` and `adminPassword`.</span></span>

    ```json
    "adminUsername": "<user name>",
    "adminPassword": "<password>",
    ```

2. <span data-ttu-id="a14f5-270">Führen Sie den folgenden Befehl aus:</span><span class="sxs-lookup"><span data-stu-id="a14f5-270">Run the following command:</span></span>

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
[vnet-peering-requirements]: /azure/virtual-network/virtual-network-manage-peering#requirements-and-constraints
[vpn-appliance]: /azure/vpn-gateway/vpn-gateway-about-vpn-devices
[windows-vm-ra]: ../virtual-machines-windows/index.md
[visio-download]: https://archcenter.blob.core.windows.net/cdn/hybrid-network-hub-spoke.vsdx
[ref-arch-repo]: https://github.com/mspnp/reference-architectures

[0]: ./images/hub-spoke.png "Hub-Spoke-Topologie in Azure"
[1]: ./images/hub-spoke-gateway-routing.svg "Hub-Spoke-Topologie in Azure mit transitivem Routing"
[2]: ./images/hub-spoke-no-gateway-routing.svg "Hub-Spoke-Topologie in Azure mit transitivem Routing unter Verwendung eines NVA"
[3]: ./images/hub-spokehub-spoke.svg "Hub-Spoke-Hub-Spoke-Topologie in Azure"
