---
title: Verbinden eines lokalen Netzwerks mit Azure
titleSuffix: Azure Reference Architectures
description: Dieser Artikel vergleicht Referenzarchitekturen zum Herstellen einer Verbindung zwischen einem lokalen Netzwerk und Azure.
author: telmosampaio
ms.date: 07/02/2018
ms.topic: reference-architecture
ms.service: architecture-center
ms.subservice: reference-architecture
ms.custom: networking
ms.openlocfilehash: 6172866b08197b0ca1cd3aabb3c14c01b4f06f9c
ms.sourcegitcommit: 1b50810208354577b00e89e5c031b774b02736e2
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/23/2019
ms.locfileid: "54486848"
---
# <a name="choose-a-solution-for-connecting-an-on-premises-network-to-azure"></a><span data-ttu-id="1d4f7-103">Auswählen einer Lösung zum Herstellen einer Verbindung zwischen einem lokalen Netzwerk und Azure</span><span class="sxs-lookup"><span data-stu-id="1d4f7-103">Choose a solution for connecting an on-premises network to Azure</span></span>

<span data-ttu-id="1d4f7-104">Dieser Artikel vergleicht Optionen zum Herstellen einer Verbindung zwischen einem lokalen Netzwerk und einem virtuellen Azure-Netzwerk (VNET).</span><span class="sxs-lookup"><span data-stu-id="1d4f7-104">This article compares options for connecting an on-premises network to an Azure Virtual Network (VNet).</span></span> <span data-ttu-id="1d4f7-105">Für jede Option ist eine detailliertere Referenzarchitektur verfügbar.</span><span class="sxs-lookup"><span data-stu-id="1d4f7-105">For each option, a more detailed reference architecture is available.</span></span>

## <a name="vpn-connection"></a><span data-ttu-id="1d4f7-106">VPN-Verbindung</span><span class="sxs-lookup"><span data-stu-id="1d4f7-106">VPN connection</span></span>

<span data-ttu-id="1d4f7-107">Ein [VPN Gateway](/azure/vpn-gateway/vpn-gateway-about-vpngateways) ist eine Art von Gateway für virtuelle Netzwerke, mit dem verschlüsselter Datenverkehr zwischen einem virtuellen Azure-Netzwerk und einem lokalen Standort gesendet wird.</span><span class="sxs-lookup"><span data-stu-id="1d4f7-107">A [VPN gateway](/azure/vpn-gateway/vpn-gateway-about-vpngateways) is a type of virtual network gateway that sends encrypted traffic between an Azure virtual network and an on-premises location.</span></span> <span data-ttu-id="1d4f7-108">Der verschlüsselte Datenverkehr wird über das öffentliche Internet übertragen.</span><span class="sxs-lookup"><span data-stu-id="1d4f7-108">The encrypted traffic goes over the public Internet.</span></span>

<span data-ttu-id="1d4f7-109">Diese Architektur eignet sich für hybride Anwendungen, bei denen wahrscheinlich nur wenig Datenverkehr zwischen der lokalen Hardware und der Cloud stattfindet. Sie ist auch dann geeignet, wenn Sie eine geringfügig erhöhte Latenz in Kauf nehmen können, um von der Flexibilität und der Verarbeitungsleistung der Cloud zu profitieren.</span><span class="sxs-lookup"><span data-stu-id="1d4f7-109">This architecture is suitable for hybrid applications where the traffic between on-premises hardware and the cloud is likely to be light, or you are willing to trade slightly extended latency for the flexibility and processing power of the cloud.</span></span>

### <a name="benefits"></a><span data-ttu-id="1d4f7-110">Vorteile</span><span class="sxs-lookup"><span data-stu-id="1d4f7-110">Benefits</span></span>

- <span data-ttu-id="1d4f7-111">Diese Architektur lässt sich leicht konfigurieren.</span><span class="sxs-lookup"><span data-stu-id="1d4f7-111">Simple to configure.</span></span>

### <a name="challenges"></a><span data-ttu-id="1d4f7-112">Herausforderungen</span><span class="sxs-lookup"><span data-stu-id="1d4f7-112">Challenges</span></span>

- <span data-ttu-id="1d4f7-113">Es ist ein lokales VPN-Gerät erforderlich.</span><span class="sxs-lookup"><span data-stu-id="1d4f7-113">Requires an on-premises VPN device.</span></span>
- <span data-ttu-id="1d4f7-114">Microsoft garantiert zwar für jedes VPN Gateway eine Verfügbarkeit von 99,9 %, diese [SLA](https://azure.microsoft.com/support/legal/sla/vpn-gateway/) gilt aber nur für das VPN Gateway, nicht für Ihre Netzwerkverbindung zum Gateway.</span><span class="sxs-lookup"><span data-stu-id="1d4f7-114">Although Microsoft guarantees 99.9% availability for each VPN Gateway, this [SLA](https://azure.microsoft.com/support/legal/sla/vpn-gateway/) only covers the VPN gateway, and not your network connection to the gateway.</span></span>
- <span data-ttu-id="1d4f7-115">Eine VPN-Verbindung über Azure VPN Gateway unterstützt zurzeit eine Bandbreite von maximal 1,25 GBit/s.</span><span class="sxs-lookup"><span data-stu-id="1d4f7-115">A VPN connection over Azure VPN Gateway currently supports a maximum of 1.25 Gbps bandwidth.</span></span> <span data-ttu-id="1d4f7-116">Möglicherweise müssen Sie Ihr virtuelles Azure-Netzwerk über mehrere VPN-Verbindungen hinweg partitionieren, wenn Sie damit rechnen, diesen Durchsatz zu überschreiten.</span><span class="sxs-lookup"><span data-stu-id="1d4f7-116">You may need to partition your Azure virtual network across multiple VPN connections if you expect to exceed this throughput.</span></span>

### <a name="reference-architecture"></a><span data-ttu-id="1d4f7-117">Referenzarchitektur</span><span class="sxs-lookup"><span data-stu-id="1d4f7-117">Reference architecture</span></span>

- [<span data-ttu-id="1d4f7-118">Hybridnetzwerk mit VPN-Gateway</span><span class="sxs-lookup"><span data-stu-id="1d4f7-118">Hybrid network with VPN gateway</span></span>](./vpn.md)

<!-- markdownlint-disable MD024 -->

## <a name="azure-expressroute-connection"></a><span data-ttu-id="1d4f7-119">Azure ExpressRoute-Verbindung</span><span class="sxs-lookup"><span data-stu-id="1d4f7-119">Azure ExpressRoute connection</span></span>

<span data-ttu-id="1d4f7-120">[ExpressRoute](/azure/expressroute/)-Verbindungen nutzen eine dedizierte private Verbindung über einen Drittanbieter für die Konnektivität.</span><span class="sxs-lookup"><span data-stu-id="1d4f7-120">[ExpressRoute](/azure/expressroute/) connections use a private, dedicated connection through a third-party connectivity provider.</span></span> <span data-ttu-id="1d4f7-121">Die private Verbindung erweitert Ihr lokales Netzwerk auf Azure.</span><span class="sxs-lookup"><span data-stu-id="1d4f7-121">The private connection extends your on-premises network into Azure.</span></span>

<span data-ttu-id="1d4f7-122">Diese Architektur eignet sich für hybride Anwendungen, die umfangreiche geschäftskritische Workloads ausführen, für die ein hohes Maß an Skalierbarkeit erforderlich ist.</span><span class="sxs-lookup"><span data-stu-id="1d4f7-122">This architecture is suitable for hybrid applications running large-scale, mission-critical workloads that require a high degree of scalability.</span></span>

### <a name="benefits"></a><span data-ttu-id="1d4f7-123">Vorteile</span><span class="sxs-lookup"><span data-stu-id="1d4f7-123">Benefits</span></span>

- <span data-ttu-id="1d4f7-124">Es steht eine wesentlich höhere Bandbreite zur Verfügung – bis zu 10 GBit/s, je nach Konnektivitätsanbieter.</span><span class="sxs-lookup"><span data-stu-id="1d4f7-124">Much higher bandwidth available; up to 10 Gbps depending on the connectivity provider.</span></span>
- <span data-ttu-id="1d4f7-125">Die Architektur unterstützt eine dynamische Skalierung der Bandbreite, um in Zeiträumen mit geringerer Nachfrage die Kosten zu senken.</span><span class="sxs-lookup"><span data-stu-id="1d4f7-125">Supports dynamic scaling of bandwidth to help reduce costs during periods of lower demand.</span></span> <span data-ttu-id="1d4f7-126">Allerdings steht diese Option nicht bei allen Konnektivitätsanbietern zur Verfügung.</span><span class="sxs-lookup"><span data-stu-id="1d4f7-126">However, not all connectivity providers have this option.</span></span>
- <span data-ttu-id="1d4f7-127">Möglicherweise erhält Ihre Organisation direkten Zugriff auf landesweite Clouds – dies hängt vom Konnektivitätsanbieter ab.</span><span class="sxs-lookup"><span data-stu-id="1d4f7-127">May allow your organization direct access to national clouds, depending on the connectivity provider.</span></span>
- <span data-ttu-id="1d4f7-128">Es gilt für die gesamte Verbindung eine Verfügbarkeits-SLA von 99,9 %.</span><span class="sxs-lookup"><span data-stu-id="1d4f7-128">99.9% availability SLA across the entire connection.</span></span>

### <a name="challenges"></a><span data-ttu-id="1d4f7-129">Herausforderungen</span><span class="sxs-lookup"><span data-stu-id="1d4f7-129">Challenges</span></span>

- <span data-ttu-id="1d4f7-130">Die Einrichtung kann sehr komplex sein.</span><span class="sxs-lookup"><span data-stu-id="1d4f7-130">Can be complex to set up.</span></span> <span data-ttu-id="1d4f7-131">Zum Erstellen einer ExpressRoute-Verbindung müssen Sie mit einem Drittanbieter für die Konnektivität zusammenarbeiten.</span><span class="sxs-lookup"><span data-stu-id="1d4f7-131">Creating an ExpressRoute connection requires working with a third-party connectivity provider.</span></span> <span data-ttu-id="1d4f7-132">Der Anbieter ist für die Bereitstellung der Netzwerkverbindung verantwortlich.</span><span class="sxs-lookup"><span data-stu-id="1d4f7-132">The provider is responsible for provisioning the network connection.</span></span>
- <span data-ttu-id="1d4f7-133">Die Architektur erfordert lokale Router mit hoher Bandbreite.</span><span class="sxs-lookup"><span data-stu-id="1d4f7-133">Requires high-bandwidth routers on-premises.</span></span>

### <a name="reference-architecture"></a><span data-ttu-id="1d4f7-134">Referenzarchitektur</span><span class="sxs-lookup"><span data-stu-id="1d4f7-134">Reference architecture</span></span>

- [<span data-ttu-id="1d4f7-135">Hybridnetzwerk mit ExpressRoute</span><span class="sxs-lookup"><span data-stu-id="1d4f7-135">Hybrid network with ExpressRoute</span></span>](./expressroute.md)

## <a name="expressroute-with-vpn-failover"></a><span data-ttu-id="1d4f7-136">ExpressRoute mit VPN-Failover</span><span class="sxs-lookup"><span data-stu-id="1d4f7-136">ExpressRoute with VPN failover</span></span>

<span data-ttu-id="1d4f7-137">Diese Option kombiniert die beiden vorherigen: Unter normalen Bedingungen wird eine ExpressRoute-Verbindung verwendet, aber im Fall eines Konnektivitätsverlusts in der ExpressRoute-Leitung erfolgt ein Failover auf eine VPN-Verbindung.</span><span class="sxs-lookup"><span data-stu-id="1d4f7-137">This options combines the previous two, using ExpressRoute in normal conditions, but failing over to a VPN connection if there is a loss of connectivity in the ExpressRoute circuit.</span></span>

<span data-ttu-id="1d4f7-138">Diese Architektur eignet sich für hybride Anwendungen, die sowohl die höhere Bandbreite von ExpressRoute als auch Netzwerkkonnektivität mit hoher Verfügbarkeit benötigen.</span><span class="sxs-lookup"><span data-stu-id="1d4f7-138">This architecture is suitable for hybrid applications that need the higher bandwidth of ExpressRoute, and also require highly available network connectivity.</span></span>

### <a name="benefits"></a><span data-ttu-id="1d4f7-139">Vorteile</span><span class="sxs-lookup"><span data-stu-id="1d4f7-139">Benefits</span></span>

- <span data-ttu-id="1d4f7-140">Bei einem Ausfall der ExpressRoute-Leitung ist eine hohe Verfügbarkeit gewährleistet, auch wenn die Fallbackverbindung über ein Netzwerk mit geringerer Bandbreite erfolgt.</span><span class="sxs-lookup"><span data-stu-id="1d4f7-140">High availability if the ExpressRoute circuit fails, although the fallback connection is on a lower bandwidth network.</span></span>

### <a name="challenges"></a><span data-ttu-id="1d4f7-141">Herausforderungen</span><span class="sxs-lookup"><span data-stu-id="1d4f7-141">Challenges</span></span>

- <span data-ttu-id="1d4f7-142">Die Konfiguration ist komplex.</span><span class="sxs-lookup"><span data-stu-id="1d4f7-142">Complex to configure.</span></span> <span data-ttu-id="1d4f7-143">Sie müssen sowohl eine VPN-Verbindung als auch eine ExpressRoute-Leitung einrichten.</span><span class="sxs-lookup"><span data-stu-id="1d4f7-143">You need to set up both a VPN connection and an ExpressRoute circuit.</span></span>
- <span data-ttu-id="1d4f7-144">Es sind redundante Hardwarekomponenten (VPN-Geräte) und eine redundante Azure VPN-Gatewayverbindung erforderlich, wofür Gebühren anfallen.</span><span class="sxs-lookup"><span data-stu-id="1d4f7-144">Requires redundant hardware (VPN appliances), and a redundant Azure VPN Gateway connection for which you pay charges.</span></span>

### <a name="reference-architecture"></a><span data-ttu-id="1d4f7-145">Referenzarchitektur</span><span class="sxs-lookup"><span data-stu-id="1d4f7-145">Reference architecture</span></span>

- [<span data-ttu-id="1d4f7-146">Hybridnetzwerk mit ExpressRoute und VPN-Failover</span><span class="sxs-lookup"><span data-stu-id="1d4f7-146">Hybrid network with ExpressRoute and VPN failover</span></span>](./expressroute-vpn-failover.md)

<!-- markdownlint-disable MD024 -->

## <a name="hub-spoke-network-topology"></a><span data-ttu-id="1d4f7-147">Hub-Spoke-Netzwerktopologie</span><span class="sxs-lookup"><span data-stu-id="1d4f7-147">Hub-spoke network topology</span></span>

<span data-ttu-id="1d4f7-148">Eine Hub-Spoke-Netzwerktopologie ist eine Möglichkeit zur Isolierung von Workloads während Dienste wie Identität und Sicherheit gemeinsam verwendet werden.</span><span class="sxs-lookup"><span data-stu-id="1d4f7-148">A hub-spoke network topology is a way to isolate workloads while sharing services such as identity and security.</span></span> <span data-ttu-id="1d4f7-149">Bei einem Hub handelt es sich um ein virtuelles Netzwerk (VNET) in Azure, das als zentraler Konnektivitätspunkt für Ihr lokales Netzwerk fungiert.</span><span class="sxs-lookup"><span data-stu-id="1d4f7-149">The hub is a virtual network (VNet) in Azure that acts as a central point of connectivity to your on-premises network.</span></span> <span data-ttu-id="1d4f7-150">Bei den Spokes handelt es sich um VNETs, die eine Peeringverbindung mit dem Hub herstellen.</span><span class="sxs-lookup"><span data-stu-id="1d4f7-150">The spokes are VNets that peer with the hub.</span></span> <span data-ttu-id="1d4f7-151">Gemeinsame Dienste werden im Hub bereitgestellt, während einzelne Workloads als Spokes bereitgestellt werden.</span><span class="sxs-lookup"><span data-stu-id="1d4f7-151">Shared services are deployed in the hub, while individual workloads are deployed as spokes.</span></span>

### <a name="reference-architectures"></a><span data-ttu-id="1d4f7-152">Referenzarchitekturen</span><span class="sxs-lookup"><span data-stu-id="1d4f7-152">Reference architectures</span></span>

- [<span data-ttu-id="1d4f7-153">Hub-Spoke-Topologie</span><span class="sxs-lookup"><span data-stu-id="1d4f7-153">Hub-spoke topology</span></span>](./hub-spoke.md)
- [<span data-ttu-id="1d4f7-154">Hub-Spoke mit gemeinsamen Diensten</span><span class="sxs-lookup"><span data-stu-id="1d4f7-154">Hub-spoke with shared services</span></span>](./shared-services.md)
