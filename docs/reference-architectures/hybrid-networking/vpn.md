---
title: Verbinden eines lokalen Netzwerks mit Azure über ein VPN
titleSuffix: Azure Reference Architectures
description: Hier wird erläutert, wie Sie eine sichere Netzwerkarchitektur zwischen Standorten implementieren, die ein virtuelles Azure-Netzwerk und ein lokales Netzwerk umfasst, die über ein VPN verbunden sind.
author: RohitSharma-pnp
ms.date: 10/22/2018
ms.openlocfilehash: a1bb2e250cb261e1a56abfb58b099fd078c068e5
ms.sourcegitcommit: 88a68c7e9b6b772172b7faa4b9fd9c061a9f7e9d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/08/2018
ms.locfileid: "53120440"
---
# <a name="connect-an-on-premises-network-to-azure-using-a-vpn-gateway"></a><span data-ttu-id="5cb34-103">Verbinden eines lokalen Netzwerks mit Azure über ein VPN-Gateway</span><span class="sxs-lookup"><span data-stu-id="5cb34-103">Connect an on-premises network to Azure using a VPN gateway</span></span>

<span data-ttu-id="5cb34-104">Diese Referenzarchitektur zeigt, wie Sie ein lokales Netzwerk auf Azure ausdehnen, indem Sie ein VPN (virtuelles privates Netzwerk) zwischen Standorten verwenden.</span><span class="sxs-lookup"><span data-stu-id="5cb34-104">This reference architecture shows how to extend an on-premises network to Azure, using a site-to-site virtual private network (VPN).</span></span> <span data-ttu-id="5cb34-105">Der Datenverkehr zwischen dem lokalen Netzwerk und dem virtuellen Azure-Netzwerk (VNet) wird durch einen IPSec-VPN-Tunnel übertragen.</span><span class="sxs-lookup"><span data-stu-id="5cb34-105">Traffic flows between the on-premises network and an Azure Virtual Network (VNet) through an IPSec VPN tunnel.</span></span> <span data-ttu-id="5cb34-106">[**Stellen Sie diese Lösung bereit**](#deploy-the-solution).</span><span class="sxs-lookup"><span data-stu-id="5cb34-106">[**Deploy this solution**](#deploy-the-solution).</span></span>

![Hybrides Netzwerk, das die lokale Infrastruktur und die Azure-Infrastruktur umfasst](./images/vpn.png)

<span data-ttu-id="5cb34-108">*Laden Sie eine [Visio-Datei][visio-download] mit dieser Architektur herunter.*</span><span class="sxs-lookup"><span data-stu-id="5cb34-108">*Download a [Visio file][visio-download] of this architecture.*</span></span>

## <a name="architecture"></a><span data-ttu-id="5cb34-109">Architecture</span><span class="sxs-lookup"><span data-stu-id="5cb34-109">Architecture</span></span>

<span data-ttu-id="5cb34-110">Die Architektur umfasst die folgenden Komponenten.</span><span class="sxs-lookup"><span data-stu-id="5cb34-110">The architecture consists of the following components.</span></span>

- <span data-ttu-id="5cb34-111">**Lokales Netzwerk**.</span><span class="sxs-lookup"><span data-stu-id="5cb34-111">**On-premises network**.</span></span> <span data-ttu-id="5cb34-112">Ein in einer Organisation betriebenes privates lokales Netzwerk.</span><span class="sxs-lookup"><span data-stu-id="5cb34-112">A private local-area network running within an organization.</span></span>

- <span data-ttu-id="5cb34-113">**VPN-Gerät**.</span><span class="sxs-lookup"><span data-stu-id="5cb34-113">**VPN appliance**.</span></span> <span data-ttu-id="5cb34-114">Ein Gerät oder ein Dienst, das bzw. der externe Konnektivität mit dem lokalen Netzwerk bereitstellt.</span><span class="sxs-lookup"><span data-stu-id="5cb34-114">A device or service that provides external connectivity to the on-premises network.</span></span> <span data-ttu-id="5cb34-115">Bei dem VPN-Gerät kann es sich um ein Hardwaregerät oder eine Softwarelösung, z.B. den Routing- und RAS-Dienst unter Windows Server 2012, handeln.</span><span class="sxs-lookup"><span data-stu-id="5cb34-115">The VPN appliance may be a hardware device, or it can be a software solution such as the Routing and Remote Access Service (RRAS) in Windows Server 2012.</span></span> <span data-ttu-id="5cb34-116">Eine Liste unterstützter VPN-Geräte und Informationen zu ihrer Konfiguration für die Verbindung mit einem Azure-VPN-Gateway finden Sie in den Anweisungen für das ausgewählte Gerät unter [Informationen zu VPN-Geräten für VPN-Gatewayverbindungen zwischen Standorten][vpn-appliance].</span><span class="sxs-lookup"><span data-stu-id="5cb34-116">For a list of supported VPN appliances and information on configuring them to connect to an Azure VPN gateway, see the instructions for the selected device in the article [About VPN devices for Site-to-Site VPN Gateway connections][vpn-appliance].</span></span>

- <span data-ttu-id="5cb34-117">**Virtuelles Netzwerk (VNET)**.</span><span class="sxs-lookup"><span data-stu-id="5cb34-117">**Virtual network (VNet)**.</span></span> <span data-ttu-id="5cb34-118">Die Cloudanwendung und die Komponenten des Azure-VPN-Gateways befinden sich im selben [VNet][azure-virtual-network].</span><span class="sxs-lookup"><span data-stu-id="5cb34-118">The cloud application and the components for the Azure VPN gateway reside in the same [VNet][azure-virtual-network].</span></span>

- <span data-ttu-id="5cb34-119">**Azure-VPN-Gateway**.</span><span class="sxs-lookup"><span data-stu-id="5cb34-119">**Azure VPN gateway**.</span></span> <span data-ttu-id="5cb34-120">Der Dienst [VPN Gateway][azure-vpn-gateway] ermöglicht dem VNet, über ein VPN-Gerät eine Verbindung mit dem lokalen Netzwerk herzustellen.</span><span class="sxs-lookup"><span data-stu-id="5cb34-120">The [VPN gateway][azure-vpn-gateway] service enables you to connect the VNet to the on-premises network through a VPN appliance.</span></span> <span data-ttu-id="5cb34-121">Weitere Informationen finden Sie unter [Verbinden eines lokalen Netzwerks mit einem Microsoft Azure Virtual Network][connect-to-an-Azure-vnet].</span><span class="sxs-lookup"><span data-stu-id="5cb34-121">For more information, see [Connect an on-premises network to a Microsoft Azure virtual network][connect-to-an-Azure-vnet].</span></span> <span data-ttu-id="5cb34-122">Das VPN-Gateway enthält die folgenden Elemente:</span><span class="sxs-lookup"><span data-stu-id="5cb34-122">The VPN gateway includes the following elements:</span></span>

  - <span data-ttu-id="5cb34-123">**Gateway des virtuellen Netzwerks**.</span><span class="sxs-lookup"><span data-stu-id="5cb34-123">**Virtual network gateway**.</span></span> <span data-ttu-id="5cb34-124">Eine Ressource, die dem VNet ein virtuelles VPN-Gerät bereitstellt.</span><span class="sxs-lookup"><span data-stu-id="5cb34-124">A resource that provides a virtual VPN appliance for the VNet.</span></span> <span data-ttu-id="5cb34-125">Dieses ist für die Weiterleitung des Datenverkehrs vom lokalen Netzwerk zum VNet zuständig.</span><span class="sxs-lookup"><span data-stu-id="5cb34-125">It is responsible for routing traffic from the on-premises network to the VNet.</span></span>
  - <span data-ttu-id="5cb34-126">**Gateway des lokalen Netzwerks**.</span><span class="sxs-lookup"><span data-stu-id="5cb34-126">**Local network gateway**.</span></span> <span data-ttu-id="5cb34-127">Eine Abstraktion des lokalen VPN-Geräts.</span><span class="sxs-lookup"><span data-stu-id="5cb34-127">An abstraction of the on-premises VPN appliance.</span></span> <span data-ttu-id="5cb34-128">Netzwerkdatenverkehr aus der Cloudanwendung zum lokalen Netzwerk wird durch dieses Gateway geleitet.</span><span class="sxs-lookup"><span data-stu-id="5cb34-128">Network traffic from the cloud application to the on-premises network is routed through this gateway.</span></span>
  - <span data-ttu-id="5cb34-129">**Verbindung**.</span><span class="sxs-lookup"><span data-stu-id="5cb34-129">**Connection**.</span></span> <span data-ttu-id="5cb34-130">Die Verbindung verfügt über Eigenschaften, die den Verbindungstyp (IPSec) und den Schlüssel angeben, der für das lokale VPN-Gerät freigegeben wird, um Datenverkehr zu verschlüsseln.</span><span class="sxs-lookup"><span data-stu-id="5cb34-130">The connection has properties that specify the connection type (IPSec) and the key shared with the on-premises VPN appliance to encrypt traffic.</span></span>
  - <span data-ttu-id="5cb34-131">**Gatewaysubnetz**.</span><span class="sxs-lookup"><span data-stu-id="5cb34-131">**Gateway subnet**.</span></span> <span data-ttu-id="5cb34-132">Das virtuelle Netzwerkgateway befindet sich in einem eigenen Subnetz, das verschiedenen Anforderungen unterliegt, die im Abschnitt „Empfehlungen“ weiter unten beschrieben werden.</span><span class="sxs-lookup"><span data-stu-id="5cb34-132">The virtual network gateway is held in its own subnet, which is subject to various requirements, described in the Recommendations section below.</span></span>

- <span data-ttu-id="5cb34-133">**Cloudanwendung**.</span><span class="sxs-lookup"><span data-stu-id="5cb34-133">**Cloud application**.</span></span> <span data-ttu-id="5cb34-134">Die in Azure gehostete Anwendung.</span><span class="sxs-lookup"><span data-stu-id="5cb34-134">The application hosted in Azure.</span></span> <span data-ttu-id="5cb34-135">Sie kann mehrere Schichten umfassen, wobei mehrere Subnetze über Azure Load Balancer verbunden sind.</span><span class="sxs-lookup"><span data-stu-id="5cb34-135">It might include multiple tiers, with multiple subnets connected through Azure load balancers.</span></span> <span data-ttu-id="5cb34-136">Weitere Informationen zur Anwendungsinfrastruktur finden Sie unter [Ausführen von Windows-VM-Workloads][windows-vm-ra] und [Ausführen von Linux-VM-Workloads][linux-vm-ra].</span><span class="sxs-lookup"><span data-stu-id="5cb34-136">For more information about the application infrastructure, see [Running Windows VM workloads][windows-vm-ra] and [Running Linux VM workloads][linux-vm-ra].</span></span>

- <span data-ttu-id="5cb34-137">**Interner Load Balancer**.</span><span class="sxs-lookup"><span data-stu-id="5cb34-137">**Internal load balancer**.</span></span> <span data-ttu-id="5cb34-138">Netzwerkverkehr aus dem VPN-Gateway wird über einen internen Load Balancer an die Cloudanwendung weitergeleitet.</span><span class="sxs-lookup"><span data-stu-id="5cb34-138">Network traffic from the VPN gateway is routed to the cloud application through an internal load balancer.</span></span> <span data-ttu-id="5cb34-139">Der Load Balancer befindet sich im Front-End-Subnetz der Anwendung.</span><span class="sxs-lookup"><span data-stu-id="5cb34-139">The load balancer is located in the front-end subnet of the application.</span></span>

## <a name="recommendations"></a><span data-ttu-id="5cb34-140">Empfehlungen</span><span class="sxs-lookup"><span data-stu-id="5cb34-140">Recommendations</span></span>

<span data-ttu-id="5cb34-141">Die folgenden Empfehlungen gelten für die meisten Szenarios.</span><span class="sxs-lookup"><span data-stu-id="5cb34-141">The following recommendations apply for most scenarios.</span></span> <span data-ttu-id="5cb34-142">Sofern Sie keine besonderen Anforderungen haben, die Vorrang haben, sollten Sie diese Empfehlungen befolgen.</span><span class="sxs-lookup"><span data-stu-id="5cb34-142">Follow these recommendations unless you have a specific requirement that overrides them.</span></span>

### <a name="vnet-and-gateway-subnet"></a><span data-ttu-id="5cb34-143">VNet und Gatewaysubnetz</span><span class="sxs-lookup"><span data-stu-id="5cb34-143">VNet and gateway subnet</span></span>

<span data-ttu-id="5cb34-144">Erstellen Sie ein Azure VNet mit einem Adressraum, der für alle erforderlichen Ressourcen ausreichend groß ist.</span><span class="sxs-lookup"><span data-stu-id="5cb34-144">Create an Azure VNet with an address space large enough for all of your required resources.</span></span> <span data-ttu-id="5cb34-145">Stellen Sie sicher, dass der VNet-Adressraum genügend Platz für Wachstum bietet, falls in Zukunft weitere VMs benötigt werden.</span><span class="sxs-lookup"><span data-stu-id="5cb34-145">Ensure that the VNet address space has sufficient room for growth if additional VMs are likely to be needed in the future.</span></span> <span data-ttu-id="5cb34-146">Der Adressraum des VNet darf sich nicht mit dem des lokalen Netzwerks überschneiden.</span><span class="sxs-lookup"><span data-stu-id="5cb34-146">The address space of the VNet must not overlap with the on-premises network.</span></span> <span data-ttu-id="5cb34-147">Im obigen Diagramm wird beispielsweise der Adressraum 10.20.0.0/16 für das VNet verwendet.</span><span class="sxs-lookup"><span data-stu-id="5cb34-147">For example, the diagram above uses the address space 10.20.0.0/16 for the VNet.</span></span>

<span data-ttu-id="5cb34-148">Erstellen Sie ein Subnetz mit dem Namen *GatewaySubnet* mit dem Adressbereich „/27“.</span><span class="sxs-lookup"><span data-stu-id="5cb34-148">Create a subnet named *GatewaySubnet*, with an address range of /27.</span></span> <span data-ttu-id="5cb34-149">Dieses Subnetz ist für das Gateway für virtuelle Netzwerke erforderlich.</span><span class="sxs-lookup"><span data-stu-id="5cb34-149">This subnet is required by the virtual network gateway.</span></span> <span data-ttu-id="5cb34-150">Durch Zuweisung von 32 Adressen zu diesem Subnetz wird eine künftige Überschreitung von Beschränkungen der Gatewaygröße verhindert.</span><span class="sxs-lookup"><span data-stu-id="5cb34-150">Allocating 32 addresses to this subnet will help to prevent reaching gateway size limitations in the future.</span></span> <span data-ttu-id="5cb34-151">Vermeiden Sie außerdem, dieses Subnetz in der Mitte des Adressraums zu platzieren.</span><span class="sxs-lookup"><span data-stu-id="5cb34-151">Also, avoid placing this subnet in the middle of the address space.</span></span> <span data-ttu-id="5cb34-152">Eine empfehlenswerte Vorgehensweise besteht darin, den Adressraum für das Gatewaysubnetz am oberen Ende des VNet-Adressraums festzulegen.</span><span class="sxs-lookup"><span data-stu-id="5cb34-152">A good practice is to set the address space for the gateway subnet at the upper end of the VNet address space.</span></span> <span data-ttu-id="5cb34-153">In dem im Diagramm gezeigten Beispiel wird „10.20.255.224/27“ verwendet.</span><span class="sxs-lookup"><span data-stu-id="5cb34-153">The example shown in the diagram uses 10.20.255.224/27.</span></span>  <span data-ttu-id="5cb34-154">Es folgt eine schnelle Methode zur [CIDR]-Berechnung:</span><span class="sxs-lookup"><span data-stu-id="5cb34-154">Here is a quick procedure to calculate the [CIDR]:</span></span>

1. <span data-ttu-id="5cb34-155">Legen Sie die variablen Bits im Adressraum des VNet bis zu den Bits, die vom Gatewaysubnetz verwendet werden, auf 1 und dann die restlichen Bits auf 0 fest.</span><span class="sxs-lookup"><span data-stu-id="5cb34-155">Set the variable bits in the address space of the VNet to 1, up to the bits being used by the gateway subnet, then set the remaining bits to 0.</span></span>
2. <span data-ttu-id="5cb34-156">Konvertieren Sie die resultierenden Bits in Dezimalzahlen, und drücken Sie sie als Adressraum aus, wobei die Präfixlänge auf die Größe des Subnetzes des Gateways festgelegt wird.</span><span class="sxs-lookup"><span data-stu-id="5cb34-156">Convert the resulting bits to decimal and express it as an address space with the prefix length set to the size of the gateway subnet.</span></span>

<span data-ttu-id="5cb34-157">Beispielsweise ändert sich der IP-Adressbereich „10.20.0.0.0/16“ eines VNet nach Anwenden von Schritt 1 oben in „10.20.0b11111111.0b11100000“.</span><span class="sxs-lookup"><span data-stu-id="5cb34-157">For example, for a VNet with an IP address range of 10.20.0.0/16, applying step #1 above becomes 10.20.0b11111111.0b11100000.</span></span>  <span data-ttu-id="5cb34-158">Nach der Konvertierung in Dezimalzahlen und deren Ausdruck als Adressraum ergibt sich „10.20.255.255.224/27“.</span><span class="sxs-lookup"><span data-stu-id="5cb34-158">Converting that to decimal and expressing it as an address space yields 10.20.255.224/27.</span></span>

> [!WARNING]
> <span data-ttu-id="5cb34-159">Stellen Sie im Gatewaysubnetz keine VMs bereit.</span><span class="sxs-lookup"><span data-stu-id="5cb34-159">Do not deploy any VMs to the gateway subnet.</span></span> <span data-ttu-id="5cb34-160">Weisen Sie außerdem diesem Subnetz keine Netzwerksicherheitsgruppe (NSG) zu, da das Gateway sonst nicht mehr funktioniert.</span><span class="sxs-lookup"><span data-stu-id="5cb34-160">Also, do not assign an NSG to this subnet, as it will cause the gateway to stop functioning.</span></span>
>

### <a name="virtual-network-gateway"></a><span data-ttu-id="5cb34-161">Gateway des virtuellen Netzwerks</span><span class="sxs-lookup"><span data-stu-id="5cb34-161">Virtual network gateway</span></span>

<span data-ttu-id="5cb34-162">Ordnen Sie dem Gateway für virtuelle Netzwerke eine öffentliche IP-Adresse zu.</span><span class="sxs-lookup"><span data-stu-id="5cb34-162">Allocate a public IP address for the virtual network gateway.</span></span>

<span data-ttu-id="5cb34-163">Erstellen Sie das Gateway für virtuelle Netzwerke im Gatewaysubnetz, und weisen Sie ihm die neu zugeordnete öffentliche IP-Adresse zu.</span><span class="sxs-lookup"><span data-stu-id="5cb34-163">Create the virtual network gateway in the gateway subnet and assign it the newly allocated public IP address.</span></span> <span data-ttu-id="5cb34-164">Verwenden Sie den Gatewaytyp, der Ihren Anforderungen am ehesten entspricht und der von Ihrem VPN-Gerät unterstützt wird:</span><span class="sxs-lookup"><span data-stu-id="5cb34-164">Use the gateway type that most closely matches your requirements and that is enabled by your VPN appliance:</span></span>

- <span data-ttu-id="5cb34-165">Erstellen Sie ein [richtlinienbasiertes Gateway ][policy-based-routing], wenn Sie die Weiterleitung von Anforderungen anhand von Richtlinienkriterien wie Adresspräfixen genau steuern müssen.</span><span class="sxs-lookup"><span data-stu-id="5cb34-165">Create a [policy-based gateway][policy-based-routing] if you need to closely control how requests are routed based on policy criteria such as address prefixes.</span></span> <span data-ttu-id="5cb34-166">Richtlinienbasierte Gateways arbeiten mit statischem Routing und funktionieren nur mit Verbindungen zwischen Standorten.</span><span class="sxs-lookup"><span data-stu-id="5cb34-166">Policy-based gateways use static routing, and only work with site-to-site connections.</span></span>

- <span data-ttu-id="5cb34-167">Erstellen Sie ein [routenbasiertes Gateway][route-based-routing], wenn Sie eine Verbindung mit dem lokalen Netzwerk über RRAS herstellen, Verbindungen mit mehreren Standorten oder regionsübergreifende Verbinden unterstützen oder VNet-zu-VNet-Verbindungen implementieren (einschließlich Routen, die mehrere VNets durchqueren).</span><span class="sxs-lookup"><span data-stu-id="5cb34-167">Create a [route-based gateway][route-based-routing] if you connect to the on-premises network using RRAS, support multi-site or cross-region connections, or implement VNet-to-VNet connections (including routes that traverse multiple VNets).</span></span> <span data-ttu-id="5cb34-168">Routenbasierte Gateways verwenden dynamisches Routing, um den Datenverkehr zwischen Netzwerken zu lenken.</span><span class="sxs-lookup"><span data-stu-id="5cb34-168">Route-based gateways use dynamic routing to direct traffic between networks.</span></span> <span data-ttu-id="5cb34-169">Sie können Ausfälle im Netzwerkpfad besser verkraften als statische Routen, weil sie alternative Routen ausprobieren können.</span><span class="sxs-lookup"><span data-stu-id="5cb34-169">They can tolerate failures in the network path better than static routes because they can try alternative routes.</span></span> <span data-ttu-id="5cb34-170">Routenbasierte Gateways können auch den Verwaltungsaufwand reduzieren, da Routen nicht manuell aktualisiert werden müssen, wenn sich Netzwerkadressen ändern.</span><span class="sxs-lookup"><span data-stu-id="5cb34-170">Route-based gateways can also reduce the management overhead because routes might not need to be updated manually when network addresses change.</span></span>

<span data-ttu-id="5cb34-171">Eine Liste der unterstützten VPN-Geräte finden Sie unter [Informationen zu VPN-Geräten für VPN Gateway-Verbindungen zwischen Standorten][vpn-appliances].</span><span class="sxs-lookup"><span data-stu-id="5cb34-171">For a list of supported VPN appliances, see [About VPN devices for Site-to-Site VPN Gateway connections][vpn-appliances].</span></span>

> [!NOTE]
> <span data-ttu-id="5cb34-172">Wenn Sie nach Erstellung des Gateways die Gatewaytypen ändern möchten, müssen Sie das Gateway löschen und neu erstellen.</span><span class="sxs-lookup"><span data-stu-id="5cb34-172">After the gateway has been created, you cannot change between gateway types without deleting and re-creating the gateway.</span></span>
>

<span data-ttu-id="5cb34-173">Wählen Sie die Azure VPN Gateway SKU, die Ihren Durchsatzanforderungen am ehesten entspricht.</span><span class="sxs-lookup"><span data-stu-id="5cb34-173">Select the Azure VPN gateway SKU that most closely matches your throughput requirements.</span></span> <span data-ttu-id="5cb34-174">Weitere Informationen finden Sie unter [Gateway-SKUs][azure-gateway-skus].</span><span class="sxs-lookup"><span data-stu-id="5cb34-174">For more informayion, see [Gateway SKUs][azure-gateway-skus]</span></span>

> [!NOTE]
> <span data-ttu-id="5cb34-175">Die SKU „Basic“ ist nicht mit Azure ExpressRoute kompatibel.</span><span class="sxs-lookup"><span data-stu-id="5cb34-175">The Basic SKU is not compatible with Azure ExpressRoute.</span></span> <span data-ttu-id="5cb34-176">Sie können [die SKU ändern][changing-SKUs], nachdem das Gateway erstellt wurde.</span><span class="sxs-lookup"><span data-stu-id="5cb34-176">You can [change the SKU][changing-SKUs] after the gateway has been created.</span></span>
>

<span data-ttu-id="5cb34-177">Gebühren fallen basierend auf der Bereitstellungs- und Verfügbarkeitsdauer des Gateways an.</span><span class="sxs-lookup"><span data-stu-id="5cb34-177">You are charged based on the amount of time that the gateway is provisioned and available.</span></span> <span data-ttu-id="5cb34-178">Siehe [VPN Gateway – Preise][azure-gateway-charges].</span><span class="sxs-lookup"><span data-stu-id="5cb34-178">See [VPN Gateway Pricing][azure-gateway-charges].</span></span>

<span data-ttu-id="5cb34-179">Erstellen Sie Routingregeln für das Gatewaysubnetz, die eingehenden Anwendungsdatenverkehr vom Gateway zum internen Load Balancer leiten, anstatt Anforderungen direkt an die Anwendungs-VMs weiterleiten zu lassen.</span><span class="sxs-lookup"><span data-stu-id="5cb34-179">Create routing rules for the gateway subnet that direct incoming application traffic from the gateway to the internal load balancer, rather than allowing requests to pass directly to the application VMs.</span></span>

### <a name="on-premises-network-connection"></a><span data-ttu-id="5cb34-180">Lokale Netzwerkverbindung</span><span class="sxs-lookup"><span data-stu-id="5cb34-180">On-premises network connection</span></span>

<span data-ttu-id="5cb34-181">Erstellen Sie ein Gateway für das lokale Netzwerk.</span><span class="sxs-lookup"><span data-stu-id="5cb34-181">Create a local network gateway.</span></span> <span data-ttu-id="5cb34-182">Geben Sie die öffentliche IP-Adresse des lokalen VPN-Geräts und den Adressraum des lokalen Netzwerks an.</span><span class="sxs-lookup"><span data-stu-id="5cb34-182">Specify the public IP address of the on-premises VPN appliance, and the address space of the on-premises network.</span></span> <span data-ttu-id="5cb34-183">Beachten Sie, dass das lokale VPN-Gerät über eine öffentliche IP-Adresse verfügen muss, auf die das lokale Netzwerkgateway in Azure VPN Gateway zugreifen kann.</span><span class="sxs-lookup"><span data-stu-id="5cb34-183">Note that the on-premises VPN appliance must have a public IP address that can be accessed by the local network gateway in Azure VPN Gateway.</span></span> <span data-ttu-id="5cb34-184">Das VPN-Gerät darf sich nicht hinter einem Gerät für Netzwerkadressenübersetzung (Network Address Translation, NAT) befinden.</span><span class="sxs-lookup"><span data-stu-id="5cb34-184">The VPN device cannot be located behind a network address translation (NAT) device.</span></span>

<span data-ttu-id="5cb34-185">Richten Sie für das virtuelle und das lokale Netzwerkgateway eine Verbindung zwischen Standorten ein.</span><span class="sxs-lookup"><span data-stu-id="5cb34-185">Create a site-to-site connection for the virtual network gateway and the local network gateway.</span></span> <span data-ttu-id="5cb34-186">Wählen Sie den Typ der Verbindung zwischen Standorten (IPSec) aus, und geben Sie den gemeinsam verwendeten Schlüssel an.</span><span class="sxs-lookup"><span data-stu-id="5cb34-186">Select the site-to-site (IPSec) connection type, and specify the shared key.</span></span> <span data-ttu-id="5cb34-187">Die Verschlüsselung zwischen Standorten mit dem Azure-VPN-Gateway basiert auf dem IPSec-Protokoll, wobei zur Authentifizierung vorinstallierte Schlüssel verwendet werden.</span><span class="sxs-lookup"><span data-stu-id="5cb34-187">Site-to-site encryption with the Azure VPN gateway is based on the IPSec protocol, using preshared keys for authentication.</span></span> <span data-ttu-id="5cb34-188">Sie können den Schlüssel angeben, wenn Sie das Azure-VPN-Gateway erstellen.</span><span class="sxs-lookup"><span data-stu-id="5cb34-188">You specify the key when you create the Azure VPN gateway.</span></span> <span data-ttu-id="5cb34-189">Sie müssen das VPN-Gerät, das lokal ausgeführt wird, mit demselben Schlüssel konfigurieren.</span><span class="sxs-lookup"><span data-stu-id="5cb34-189">You must configure the VPN appliance running on-premises with the same key.</span></span> <span data-ttu-id="5cb34-190">Andere Authentifizierungsmechanismen werden derzeit nicht unterstützt.</span><span class="sxs-lookup"><span data-stu-id="5cb34-190">Other authentication mechanisms are not currently supported.</span></span>

<span data-ttu-id="5cb34-191">Stellen Sie sicher, dass die lokale Routinginfrastruktur so konfiguriert ist, dass Anforderungen für Adressen im Azure VNet an das VPN-Gerät weitergeleitet werden.</span><span class="sxs-lookup"><span data-stu-id="5cb34-191">Ensure that the on-premises routing infrastructure is configured to forward requests intended for addresses in the Azure VNet to the VPN device.</span></span>

<span data-ttu-id="5cb34-192">Öffnen Sie im lokalen Netzwerk alle Ports, die von der Cloudanwendung benötigt werden.</span><span class="sxs-lookup"><span data-stu-id="5cb34-192">Open any ports required by the cloud application in the on-premises network.</span></span>

<span data-ttu-id="5cb34-193">Testen Sie die Verbindung, um zu überprüfen, ob:</span><span class="sxs-lookup"><span data-stu-id="5cb34-193">Test the connection to verify that:</span></span>

- <span data-ttu-id="5cb34-194">Das lokale VPN-Gerät den Datenverkehr über das Azure-VPN-Gateway ordnungsgemäß an die Cloudanwendung weiterleitet.</span><span class="sxs-lookup"><span data-stu-id="5cb34-194">The on-premises VPN appliance correctly routes traffic to the cloud application through the Azure VPN gateway.</span></span>
- <span data-ttu-id="5cb34-195">Das VNet den Datenverkehr ordnungsgemäß zum lokalen Netzwerk zurückleitet.</span><span class="sxs-lookup"><span data-stu-id="5cb34-195">The VNet correctly routes traffic back to the on-premises network.</span></span>
- <span data-ttu-id="5cb34-196">Unzulässiger Datenverkehr in beide Richtungen ordnungsgemäß blockiert wird.</span><span class="sxs-lookup"><span data-stu-id="5cb34-196">Prohibited traffic in both directions is blocked correctly.</span></span>

## <a name="scalability-considerations"></a><span data-ttu-id="5cb34-197">Überlegungen zur Skalierbarkeit</span><span class="sxs-lookup"><span data-stu-id="5cb34-197">Scalability considerations</span></span>

<span data-ttu-id="5cb34-198">Sie können eine begrenzte vertikale Skalierbarkeit erreichen, indem Sie von den VPN Gateway SKUs „Basic“ oder „Standard“ zur VPN SKU „High Performance“ wechseln.</span><span class="sxs-lookup"><span data-stu-id="5cb34-198">You can achieve limited vertical scalability by moving from the Basic or Standard VPN Gateway SKUs to the High Performance VPN SKU.</span></span>

<span data-ttu-id="5cb34-199">Für VNets, die ein hohes Volumen an VPN-Datenverkehr erwarten, sollten Sie das Verteilen der verschiedenen Workloads in separate kleinere VNets und die Konfiguration eines VPN-Gateways für jedes einzelne davon in Betracht ziehen.</span><span class="sxs-lookup"><span data-stu-id="5cb34-199">For VNets that expect a large volume of VPN traffic, consider distributing the different workloads into separate smaller VNets and configuring a VPN gateway for each of them.</span></span>

<span data-ttu-id="5cb34-200">Das VNet kann entweder horizontal oder vertikal partitioniert werden.</span><span class="sxs-lookup"><span data-stu-id="5cb34-200">You can partition the VNet either horizontally or vertically.</span></span> <span data-ttu-id="5cb34-201">Verschieben Sie zum horizontalen Partitionieren einige VM-Instanzen von jeder Ebene in Subnetze des neuen VNet.</span><span class="sxs-lookup"><span data-stu-id="5cb34-201">To partition horizontally, move some VM instances from each tier into subnets of the new VNet.</span></span> <span data-ttu-id="5cb34-202">Das Ergebnis ist, dass jedes VNet dieselbe Struktur und Funktionalität aufweist.</span><span class="sxs-lookup"><span data-stu-id="5cb34-202">The result is that each VNet has the same structure and functionality.</span></span> <span data-ttu-id="5cb34-203">Um eine vertikale Partitionierung vorzunehmen, müssen Sie jede Ebene neu gestalten, um die Funktionalität in verschiedene logische Bereiche zu unterteilen (z.B. Auftragsbearbeitung, Fakturierung, Kundenbetreuung usw.).</span><span class="sxs-lookup"><span data-stu-id="5cb34-203">To partition vertically, redesign each tier to divide the functionality into different logical areas (such as handling orders, invoicing, customer account management, and so on).</span></span> <span data-ttu-id="5cb34-204">Jeder Funktionsbereich kann dann in einem eigenen VNet platziert werden.</span><span class="sxs-lookup"><span data-stu-id="5cb34-204">Each functional area can then be placed in its own VNet.</span></span>

<span data-ttu-id="5cb34-205">Die Replikation eines lokalen Active Directory-Domänencontrollers im VNet und die Implementierung von DNS im VNet kann dazu beitragen, einen Teil des sicherheitsrelevanten und administrativen Datenverkehrs zu reduzieren, der von den lokalen Systemen in die Cloud fließt.</span><span class="sxs-lookup"><span data-stu-id="5cb34-205">Replicating an on-premises Active Directory domain controller in the VNet, and implementing DNS in the VNet, can help to reduce some of the security-related and administrative traffic flowing from on-premises to the cloud.</span></span> <span data-ttu-id="5cb34-206">Weitere Informationen finden Sie unter [Erweitern von Active Directory Domain Services (AD DS) auf Azure][adds-extend-domain].</span><span class="sxs-lookup"><span data-stu-id="5cb34-206">For more information, see [Extending Active Directory Domain Services (AD DS) to Azure][adds-extend-domain].</span></span>

## <a name="availability-considerations"></a><span data-ttu-id="5cb34-207">Überlegungen zur Verfügbarkeit</span><span class="sxs-lookup"><span data-stu-id="5cb34-207">Availability considerations</span></span>

<span data-ttu-id="5cb34-208">Wenn Sie sicherstellen müssen, dass das lokale Netzwerk für das Azure-VPN-Gateway verfügbar bleibt, implementieren Sie für das lokale VPN-Gateway einen Failovercluster.</span><span class="sxs-lookup"><span data-stu-id="5cb34-208">If you need to ensure that the on-premises network remains available to the Azure VPN gateway, implement a failover cluster for the on-premises VPN gateway.</span></span>

<span data-ttu-id="5cb34-209">Wenn Ihre Organisation über mehrere lokale Standorte verfügt, richten Sie [Verbindungen dieser Standorte][vpn-gateway-multi-site] mit einem oder mehreren Azure VNets ein.</span><span class="sxs-lookup"><span data-stu-id="5cb34-209">If your organization has multiple on-premises sites, create [multi-site connections][vpn-gateway-multi-site] to one or more Azure VNets.</span></span> <span data-ttu-id="5cb34-210">Dieser Ansatz erfordert dynamisches (routenbasiertes) Routing. Stellen Sie daher sicher, dass das lokale VPN-Gateway dies unterstützt.</span><span class="sxs-lookup"><span data-stu-id="5cb34-210">This approach requires dynamic (route-based) routing, so make sure that the on-premises VPN gateway supports this feature.</span></span>

<span data-ttu-id="5cb34-211">Weitere Informationen zu Vereinbarungen zum Servicelevel finden Sie unter [SLA für VPN Gateway][sla-for-vpn-gateway].</span><span class="sxs-lookup"><span data-stu-id="5cb34-211">For details about service level agreements, see [SLA for VPN Gateway][sla-for-vpn-gateway].</span></span>

## <a name="manageability-considerations"></a><span data-ttu-id="5cb34-212">Überlegungen zur Verwaltbarkeit</span><span class="sxs-lookup"><span data-stu-id="5cb34-212">Manageability considerations</span></span>

<span data-ttu-id="5cb34-213">Überwachen Sie von lokalen VPN-Geräten stammende Diagnoseinformationen.</span><span class="sxs-lookup"><span data-stu-id="5cb34-213">Monitor diagnostic information from on-premises VPN appliances.</span></span> <span data-ttu-id="5cb34-214">Dieser Vorgang hängt von den Funktionen ab, die das VPN-Gerät bietet.</span><span class="sxs-lookup"><span data-stu-id="5cb34-214">This process depends on the features provided by the VPN appliance.</span></span> <span data-ttu-id="5cb34-215">Wenn Sie z.B. den Routing- und RAS-Dienst unter Windows Server 2012 verwenden, wählen Sie die [RRAS-Protokollierung][rras-logging].</span><span class="sxs-lookup"><span data-stu-id="5cb34-215">For example, if you are using the Routing and Remote Access Service on Windows Server 2012, [RRAS logging][rras-logging].</span></span>

<span data-ttu-id="5cb34-216">Verwenden Sie die [Azure VPN Gateway-Diagnose][gateway-diagnostic-logs] zum Erfassen von Informationen zu Verbindungsproblemen.</span><span class="sxs-lookup"><span data-stu-id="5cb34-216">Use [Azure VPN gateway diagnostics][gateway-diagnostic-logs] to capture information about connectivity issues.</span></span> <span data-ttu-id="5cb34-217">Diese Protokolle können verwendet werden, um Informationen wie Quelle und Ziele von Verbindungsanforderungen nachzuverfolgen, welches Protokoll verwendet wurde und wie die Verbindung aufgebaut wurde (oder warum der Versuch fehlgeschlagen ist).</span><span class="sxs-lookup"><span data-stu-id="5cb34-217">These logs can be used to track information such as the source and destinations of connection requests, which protocol was used, and how the connection was established (or why the attempt failed).</span></span>

<span data-ttu-id="5cb34-218">Überwachen Sie die Betriebsprotokolle des Azure-VPN-Gateways mithilfe der Überwachungsprotokolle, die im Azure-Portal verfügbar sind.</span><span class="sxs-lookup"><span data-stu-id="5cb34-218">Monitor the operational logs of the Azure VPN gateway using the audit logs available in the Azure portal.</span></span> <span data-ttu-id="5cb34-219">Für das lokale Netzwerkgateway, das Azure-Netzwerkgateway und die Verbindung sind separate Protokolle verfügbar.</span><span class="sxs-lookup"><span data-stu-id="5cb34-219">Separate logs are available for the local network gateway, the Azure network gateway, and the connection.</span></span> <span data-ttu-id="5cb34-220">Diese Informationen können verwendet werden, um jegliche Änderungen am Gateway nachzuverfolgen, und sind ggf. nützlich, wenn ein zuvor funktionierendes Gateway aus irgendeinem Grund nicht mehr funktioniert.</span><span class="sxs-lookup"><span data-stu-id="5cb34-220">This information can be used to track any changes made to the gateway, and can be useful if a previously functioning gateway stops working for some reason.</span></span>

![Überwachungsprotokolle im Azure-Portal](../_images/guidance-hybrid-network-vpn/audit-logs.png)

<span data-ttu-id="5cb34-222">Überwachen Sie Verbindungen, und verfolgen Sie Verbindungsfehlerereignisse nach.</span><span class="sxs-lookup"><span data-stu-id="5cb34-222">Monitor connectivity, and track connectivity failure events.</span></span> <span data-ttu-id="5cb34-223">Sie können ein Überwachungspaket wie [Nagios][nagios] verwenden, um diese Informationen zu erfassen und zu melden.</span><span class="sxs-lookup"><span data-stu-id="5cb34-223">You can use a monitoring package such as [Nagios][nagios] to capture and report this information.</span></span>

## <a name="security-considerations"></a><span data-ttu-id="5cb34-224">Sicherheitshinweise</span><span class="sxs-lookup"><span data-stu-id="5cb34-224">Security considerations</span></span>

<span data-ttu-id="5cb34-225">Generieren Sie für jedes VPN-Gateway einen anderen gemeinsam verwendeten Schlüssel.</span><span class="sxs-lookup"><span data-stu-id="5cb34-225">Generate a different shared key for each VPN gateway.</span></span> <span data-ttu-id="5cb34-226">Wählen Sie einen sicheren gemeinsam verwendeten Schlüssel, um Brute-Force-Angriffe abzuwehren.</span><span class="sxs-lookup"><span data-stu-id="5cb34-226">Use a strong shared key to help resist brute-force attacks.</span></span>

> [!NOTE]
> <span data-ttu-id="5cb34-227">Derzeit können Sie Azure Key Vault nicht für das Vorinstallieren von Schlüsseln für das Azure-VPN-Gateway nutzen.</span><span class="sxs-lookup"><span data-stu-id="5cb34-227">Currently, you cannot use Azure Key Vault to preshare keys for the Azure VPN gateway.</span></span>
>

<span data-ttu-id="5cb34-228">Stellen Sie sicher, dass das lokale VPN-Gerät eine Verschlüsselungsmethode verwendet, die [mit dem Azure-VPN-Gateway kompatibel][vpn-appliance-ipsec] ist.</span><span class="sxs-lookup"><span data-stu-id="5cb34-228">Ensure that the on-premises VPN appliance uses an encryption method that is [compatible with the Azure VPN gateway][vpn-appliance-ipsec].</span></span> <span data-ttu-id="5cb34-229">Für richtlinienbasiertes Routing unterstützt das Azure-VPN-Gateway die Verschlüsselungsalgorithmen AES256, AES128 und 3DES.</span><span class="sxs-lookup"><span data-stu-id="5cb34-229">For policy-based routing, the Azure VPN gateway supports the AES256, AES128, and 3DES encryption algorithms.</span></span> <span data-ttu-id="5cb34-230">Routenbasierte Gateways unterstützen AES256 und 3DES.</span><span class="sxs-lookup"><span data-stu-id="5cb34-230">Route-based gateways support AES256 and 3DES.</span></span>

<span data-ttu-id="5cb34-231">Wenn sich Ihr lokales VPN-Gerät in einem Umkreisnetzwerk (DMZ) befindet, das eine Firewall zwischen dem Umkreisnetzwerk und dem Internet aufweist, müssen Sie möglicherweise [zusätzliche Firewallregeln][additional-firewall-rules] konfigurieren, um die VPN-Verbindung zwischen Standorten zuzulassen.</span><span class="sxs-lookup"><span data-stu-id="5cb34-231">If your on-premises VPN appliance is on a perimeter network (DMZ) that has a firewall between the perimeter network and the Internet, you might have to configure [additional firewall rules][additional-firewall-rules] to allow the site-to-site VPN connection.</span></span>

<span data-ttu-id="5cb34-232">Wenn die Anwendung im VNet Daten an das Internet sendet, erwägen Sie [die Implementierung der Tunnelerzwingung][forced-tunneling], um den gesamten internetgebundenen Datenverkehr durch das lokale Netzwerk zu leiten.</span><span class="sxs-lookup"><span data-stu-id="5cb34-232">If the application in the VNet sends data to the Internet, consider [implementing forced tunneling][forced-tunneling] to route all Internet-bound traffic through the on-premises network.</span></span> <span data-ttu-id="5cb34-233">Auf diese Weise können Sie aus der lokalen Infrastruktur ausgehende Anforderungen der Anwendung überprüfen.</span><span class="sxs-lookup"><span data-stu-id="5cb34-233">This approach enables you to audit outgoing requests made by the application from the on-premises infrastructure.</span></span>

> [!NOTE]
> <span data-ttu-id="5cb34-234">Die Tunnelerzwingung kann die Konnektivität mit Azure-Diensten (z.B. Azure Storage) und dem Windows-Lizenz-Manager beeinträchtigen.</span><span class="sxs-lookup"><span data-stu-id="5cb34-234">Forced tunneling can impact connectivity to Azure services (the Storage Service, for example) and the Windows license manager.</span></span>
>

## <a name="troubleshooting"></a><span data-ttu-id="5cb34-235">Problembehandlung</span><span class="sxs-lookup"><span data-stu-id="5cb34-235">Troubleshooting</span></span>

<span data-ttu-id="5cb34-236">Allgemeine Informationen zur Behandlung gängiger Fehler im Zusammenhang mit einem VPN finden Sie im Blogbeitrag [Troubleshooting common VPN related errors][troubleshooting-vpn-errors] (Problembehandlung bei gängigen VPN-bezogenen Fehlern).</span><span class="sxs-lookup"><span data-stu-id="5cb34-236">For general information on troubleshooting common VPN-related errors, see [Troubleshooting common VPN related errors][troubleshooting-vpn-errors].</span></span>

<span data-ttu-id="5cb34-237">Die folgenden Empfehlungen sind nützlich, um festzustellen, ob Ihr lokales VPN-Gerät ordnungsgemäß funktioniert.</span><span class="sxs-lookup"><span data-stu-id="5cb34-237">The following recommendations are useful for determining if your on-premises VPN appliance is functioning correctly.</span></span>

- <span data-ttu-id="5cb34-238">**Überprüfen Sie die vom VPN-Gerät generierten Protokolldateien auf Fehler oder Ausfälle.**</span><span class="sxs-lookup"><span data-stu-id="5cb34-238">**Check any log files generated by the VPN appliance for errors or failures.**</span></span>

    <span data-ttu-id="5cb34-239">Dies hilft Ihnen festzustellen, ob das VPN-Gerät ordnungsgemäß funktioniert.</span><span class="sxs-lookup"><span data-stu-id="5cb34-239">This will help you determine if the VPN appliance is functioning correctly.</span></span> <span data-ttu-id="5cb34-240">Der Speicherort dieser Informationen variiert abhängig von Ihrer Anwendung.</span><span class="sxs-lookup"><span data-stu-id="5cb34-240">The location of this information will vary according to your appliance.</span></span> <span data-ttu-id="5cb34-241">Wenn Sie beispielsweise RRAS unter Windows Server 2012 verwenden, können Sie den folgenden PowerShell-Befehl verwenden, um Fehlerereignisinformationen für den RRAS-Dienst anzuzeigen:</span><span class="sxs-lookup"><span data-stu-id="5cb34-241">For example, if you are using RRAS on Windows Server 2012, you can use the following PowerShell command to display error event information for the RRAS service:</span></span>

    ```PowerShell
    Get-EventLog -LogName System -EntryType Error -Source RemoteAccess | Format-List -Property *
    ```

    <span data-ttu-id="5cb34-242">Die *Message*-Eigenschaft jedes Eintrags enthält eine Beschreibung des Fehlers.</span><span class="sxs-lookup"><span data-stu-id="5cb34-242">The *Message* property of each entry provides a description of the error.</span></span> <span data-ttu-id="5cb34-243">Hier einige typische Beispiele:</span><span class="sxs-lookup"><span data-stu-id="5cb34-243">Some common examples are:</span></span>

        - Inability to connect, possibly due to an incorrect IP address specified for the Azure VPN gateway in the RRAS VPN network interface configuration.

        ```console
        EventID            : 20111
        MachineName        : on-prem-vm
        Data               : {41, 3, 0, 0}
        Index              : 14231
        Category           : (0)
        CategoryNumber     : 0
        EntryType          : Error
        Message            : RoutingDomainID- {00000000-0000-0000-0000-000000000000}: A demand dial connection to the remote
                             interface AzureGateway on port VPN2-4 was successfully initiated but failed to complete
                             successfully because of the  following error: The network connection between your computer and
                             the VPN server could not be established because the remote server is not responding. This could
                             be because one of the network devices (for example, firewalls, NAT, routers, and so on) between your computer
                             and the remote server is not configured to allow VPN connections. Please contact your
                             Administrator or your service provider to determine which device may be causing the problem.
        Source             : RemoteAccess
        ReplacementStrings : {{00000000-0000-0000-0000-000000000000}, AzureGateway, VPN2-4, The network connection between
                             your computer and the VPN server could not be established because the remote server is not
                             responding. This could be because one of the network devices (for example, firewalls, NAT, routers, and so on)
                             between your computer and the remote server is not configured to allow VPN connections. Please
                             contact your Administrator or your service provider to determine which device may be causing the
                             problem.}
        InstanceId         : 20111
        TimeGenerated      : 3/18/2016 1:26:02 PM
        TimeWritten        : 3/18/2016 1:26:02 PM
        UserName           :
        Site               :
        Container          :
        ```

        - The wrong shared key being specified in the RRAS VPN network interface configuration.

        ```console
        EventID            : 20111
        MachineName        : on-prem-vm
        Data               : {233, 53, 0, 0}
        Index              : 14245
        Category           : (0)
        CategoryNumber     : 0
        EntryType          : Error
        Message            : RoutingDomainID- {00000000-0000-0000-0000-000000000000}: A demand dial connection to the remote
                             interface AzureGateway on port VPN2-4 was successfully initiated but failed to complete
                             successfully because of the  following error: Internet key exchange (IKE) authentication credentials are unacceptable.

        Source             : RemoteAccess
        ReplacementStrings : {{00000000-0000-0000-0000-000000000000}, AzureGateway, VPN2-4, IKE authentication credentials are
                             unacceptable.
                             }
        InstanceId         : 20111
        TimeGenerated      : 3/18/2016 1:34:22 PM
        TimeWritten        : 3/18/2016 1:34:22 PM
        UserName           :
        Site               :
        Container          :
        ```

    <span data-ttu-id="5cb34-244">Sie können auch Ereignisprotokollinformationen zu Verbindungsversuchen über den RRAS-Dienst mithilfe des folgenden PowerShell-Befehls abrufen:</span><span class="sxs-lookup"><span data-stu-id="5cb34-244">You can also obtain event log information about attempts to connect through the RRAS service using the following PowerShell command:</span></span>

    ```powershell
    Get-EventLog -LogName Application -Source RasClient | Format-List -Property *
    ```

    <span data-ttu-id="5cb34-245">Bei einem Verbindungsfehler enthält dieses Protokoll Fehler, die wie folgt aussehen:</span><span class="sxs-lookup"><span data-stu-id="5cb34-245">In the event of a failure to connect, this log will contain errors that look similar to the following:</span></span>

    ```console
    EventID            : 20227
    MachineName        : on-prem-vm
    Data               : {}
    Index              : 4203
    Category           : (0)
    CategoryNumber     : 0
    EntryType          : Error
    Message            : CoId={B4000371-A67F-452F-AA4C-3125AA9CFC78}: The user SYSTEM dialed a connection named
                         AzureGateway that has failed. The error code returned on failure is 809.
    Source             : RasClient
    ReplacementStrings : {{B4000371-A67F-452F-AA4C-3125AA9CFC78}, SYSTEM, AzureGateway, 809}
    InstanceId         : 20227
    TimeGenerated      : 3/18/2016 1:29:21 PM
    TimeWritten        : 3/18/2016 1:29:21 PM
    UserName           :
    Site               :
    Container          :
    ```

- <span data-ttu-id="5cb34-246">**Überprüfen Sie die Konnektivität und Weiterleitung im gesamten VPN-Gateway.**</span><span class="sxs-lookup"><span data-stu-id="5cb34-246">**Verify connectivity and routing across the VPN gateway.**</span></span>

    <span data-ttu-id="5cb34-247">Das VPN-Gerät leitet möglicherweise den Datenverkehr nicht ordnungsgemäß durch das Azure-VPN-Gateway weiter.</span><span class="sxs-lookup"><span data-stu-id="5cb34-247">The VPN appliance may not be correctly routing traffic through the Azure VPN Gateway.</span></span> <span data-ttu-id="5cb34-248">Verwenden Sie ein Tool wie[PsPing][psping], um die Konnektivität und Weiterleitung im gesamten VPN-Gateway zu überprüfen.</span><span class="sxs-lookup"><span data-stu-id="5cb34-248">Use a tool such as [PsPing][psping] to verify connectivity and routing across the VPN gateway.</span></span> <span data-ttu-id="5cb34-249">Um beispielsweise die Konnektivität eines lokalen Computers mit einem Webserver im VNet zu testen, führen Sie den folgenden Befehl aus (ersetzen Sie `<<web-server-address>>` durch die Adresse des Webservers):</span><span class="sxs-lookup"><span data-stu-id="5cb34-249">For example, to test connectivity from an on-premises machine to a web server located on the VNet, run the following command (replacing `<<web-server-address>>` with the address of the web server):</span></span>

    ```console
    PsPing -t <<web-server-address>>:80
    ```

    <span data-ttu-id="5cb34-250">Wenn der lokale Rechner Datenverkehr an den Webserver weiterleiten kann, sollten Sie eine Ausgabe ähnlich der folgenden sehen:</span><span class="sxs-lookup"><span data-stu-id="5cb34-250">If the on-premises machine can route traffic to the web server, you should see output similar to the following:</span></span>

    ```console
    D:\PSTools>psping -t 10.20.0.5:80

    PsPing v2.01 - PsPing - ping, latency, bandwidth measurement utility
    Copyright (C) 2012-2014 Mark Russinovich
    Sysinternals - www.sysinternals.com

    TCP connect to 10.20.0.5:80:
    Infinite iterations (warmup 1) connecting test:
    Connecting to 10.20.0.5:80 (warmup): 6.21ms
    Connecting to 10.20.0.5:80: 3.79ms
    Connecting to 10.20.0.5:80: 3.44ms
    Connecting to 10.20.0.5:80: 4.81ms

      Sent = 3, Received = 3, Lost = 0 (0% loss),
      Minimum = 3.44ms, Maximum = 4.81ms, Average = 4.01ms
    ```

    <span data-ttu-id="5cb34-251">Wenn der lokale Computer nicht mit dem angegebenen Ziel kommunizieren kann, werden Meldungen wie diese angezeigt:</span><span class="sxs-lookup"><span data-stu-id="5cb34-251">If the on-premises machine cannot communicate with the specified destination, you will see messages like this:</span></span>

    ```console
    D:\PSTools>psping -t 10.20.1.6:80

    PsPing v2.01 - PsPing - ping, latency, bandwidth measurement utility
    Copyright (C) 2012-2014 Mark Russinovich
    Sysinternals - www.sysinternals.com

    TCP connect to 10.20.1.6:80:
    Infinite iterations (warmup 1) connecting test:
    Connecting to 10.20.1.6:80 (warmup): This operation returned because the timeout period expired.
    Connecting to 10.20.1.6:80: This operation returned because the timeout period expired.
    Connecting to 10.20.1.6:80: This operation returned because the timeout period expired.
    Connecting to 10.20.1.6:80: This operation returned because the timeout period expired.
    Connecting to 10.20.1.6:80:
      Sent = 3, Received = 0, Lost = 3 (100% loss),
      Minimum = 0.00ms, Maximum = 0.00ms, Average = 0.00ms
    ```

- <span data-ttu-id="5cb34-252">**Stellen Sie sicher, dass die lokale Firewall VPN-Datenverkehr passieren lässt und die richtigen Ports geöffnet sind.**</span><span class="sxs-lookup"><span data-stu-id="5cb34-252">**Verify that the on-premises firewall allows VPN traffic to pass and that the correct ports are opened.**</span></span>

- <span data-ttu-id="5cb34-253">**Prüfen Sie, ob das lokale VPN-Gerät eine Verschlüsselungsmethode verwendet, die [mit dem Azure-VPN-Gateway kompatibel][vpn-appliance] ist.**</span><span class="sxs-lookup"><span data-stu-id="5cb34-253">**Verify that the on-premises VPN appliance uses an encryption method that is [compatible with the Azure VPN gateway][vpn-appliance].**</span></span> <span data-ttu-id="5cb34-254">Für richtlinienbasiertes Routing unterstützt das Azure-VPN-Gateway die Verschlüsselungsalgorithmen AES256, AES128 und 3DES.</span><span class="sxs-lookup"><span data-stu-id="5cb34-254">For policy-based routing, the Azure VPN gateway supports the AES256, AES128, and 3DES encryption algorithms.</span></span> <span data-ttu-id="5cb34-255">Routenbasierte Gateways unterstützen AES256 und 3DES.</span><span class="sxs-lookup"><span data-stu-id="5cb34-255">Route-based gateways support AES256 and 3DES.</span></span>

<span data-ttu-id="5cb34-256">Die folgenden Empfehlungen sind nützlich, um festzustellen, ob ein Problem mit dem Azure-VPN-Gateway vorliegt:</span><span class="sxs-lookup"><span data-stu-id="5cb34-256">The following recommendations are useful for determining if there is a problem with the Azure VPN gateway:</span></span>

- <span data-ttu-id="5cb34-257">**Untersuchen Sie [Diagnoseprotokolle für das Azure-VPN-Gateway][gateway-diagnostic-logs] auf potenzielle Probleme.**</span><span class="sxs-lookup"><span data-stu-id="5cb34-257">**Examine [Azure VPN gateway diagnostic logs][gateway-diagnostic-logs] for potential issues.**</span></span>

- <span data-ttu-id="5cb34-258">**Stellen Sie sicher, dass das Azure-VPN-Gateway und das lokale VPN-Gerät mit demselben gemeinsam genutzten Authentifizierungsschlüssel konfiguriert sind.**</span><span class="sxs-lookup"><span data-stu-id="5cb34-258">**Verify that the Azure VPN gateway and on-premises VPN appliance are configured with the same shared authentication key.**</span></span>

    <span data-ttu-id="5cb34-259">Sie können den vom Azure-VPN-Gateway gespeicherten gemeinsamen Schlüssel mithilfe des folgenden Azure CLI-Befehls einsehen:</span><span class="sxs-lookup"><span data-stu-id="5cb34-259">You can view the shared key stored by the Azure VPN gateway using the following Azure CLI command:</span></span>

    ```azurecli
    azure network vpn-connection shared-key show <<resource-group>> <<vpn-connection-name>>
    ```

    <span data-ttu-id="5cb34-260">Verwenden Sie den Befehl entsprechend Ihrem lokalen VPN-Gerät, um den gemeinsam genutzten Schlüssel anzuzeigen, der für dieses Gerät konfiguriert wurde.</span><span class="sxs-lookup"><span data-stu-id="5cb34-260">Use the command appropriate for your on-premises VPN appliance to show the shared key configured for that appliance.</span></span>

    <span data-ttu-id="5cb34-261">Vergewissern Sie sich, dass das Subnetz *GatewaySubnet*, in dem sich das Azure-VPN-Gateway befindet, keiner Netzwerksicherheitsgruppe zugeordnet ist.</span><span class="sxs-lookup"><span data-stu-id="5cb34-261">Verify that the *GatewaySubnet* subnet holding the Azure VPN gateway is not associated with an NSG.</span></span>

    <span data-ttu-id="5cb34-262">Sie können die Details des Subnetzes mit dem folgenden Azure CLI-Befehl einsehen:</span><span class="sxs-lookup"><span data-stu-id="5cb34-262">You can view the subnet details using the following Azure CLI command:</span></span>

    ```azurecli
    azure network vnet subnet show -g <<resource-group>> -e <<vnet-name>> -n GatewaySubnet
    ```

    <span data-ttu-id="5cb34-263">Stellen Sie sicher, dass es kein Datenfeld mit dem Namen *Network Security Group id* gibt. Das folgende Beispiel zeigt die Ergebnisse für eine Instanz von *GatewaySubnet*, der eine Netzwerksicherheitsgruppe (*VPN-Gateway-Group*) zugewiesen ist.</span><span class="sxs-lookup"><span data-stu-id="5cb34-263">Ensure there is no data field named *Network Security Group id*. The following example shows the results for an instance of the *GatewaySubnet* that has an assigned NSG (*VPN-Gateway-Group*).</span></span> <span data-ttu-id="5cb34-264">Wenn für diese Netzwerksicherheitsgruppe Regeln definiert sind, kann dies das ordnungsgemäße Funktionieren des Gateways verhindern.</span><span class="sxs-lookup"><span data-stu-id="5cb34-264">This can prevent the gateway from working correctly if there are any rules defined for this NSG.</span></span>

    ```console
    C:\>azure network vnet subnet show -g profx-prod-rg -e profx-vnet -n GatewaySubnet
        info:    Executing command network vnet subnet show
        + Looking up virtual network "profx-vnet"
        + Looking up the subnet "GatewaySubnet"
        data:    Id                              : /subscriptions/########-####-####-####-############/resourceGroups/profx-prod-rg/providers/Microsoft.Network/virtualNetworks/profx-vnet/subnets/GatewaySubnet
        data:    Name                            : GatewaySubnet
        data:    Provisioning state              : Succeeded
        data:    Address prefix                  : 10.20.3.0/27
        data:    Network Security Group id       : /subscriptions/########-####-####-####-############/resourceGroups/profx-prod-rg/providers/Microsoft.Network/networkSecurityGroups/VPN-Gateway-Group
        info:    network vnet subnet show command OK
    ```

- <span data-ttu-id="5cb34-265">**Vergewissern Sie sich, dass die virtuellen Computer im Azure VNet so konfiguriert sind, dass sie Datenverkehr von außerhalb des VNet zulassen.**</span><span class="sxs-lookup"><span data-stu-id="5cb34-265">**Verify that the virtual machines in the Azure VNet are configured to permit traffic coming in from outside the VNet.**</span></span>

    <span data-ttu-id="5cb34-266">Überprüfen Sie jegliche NSG-Regeln, die mit Subnetzen verknüpft sind, die diese virtuellen Computer enthalten.</span><span class="sxs-lookup"><span data-stu-id="5cb34-266">Check any NSG rules associated with subnets containing these virtual machines.</span></span> <span data-ttu-id="5cb34-267">Sie können alle NSG-Regeln mit dem folgenden Azure CLI-Befehl einsehen:</span><span class="sxs-lookup"><span data-stu-id="5cb34-267">You can view all NSG rules using the following Azure CLI command:</span></span>

    ```azurecli
    azure network nsg show -g <<resource-group>> -n <<nsg-name>>
    ```

- <span data-ttu-id="5cb34-268">**Stellen Sie sicher, dass das Azure-VPN-Gateway verbunden ist.**</span><span class="sxs-lookup"><span data-stu-id="5cb34-268">**Verify that the Azure VPN gateway is connected.**</span></span>

    <span data-ttu-id="5cb34-269">Mit dem folgenden Azure PowerShell-Befehl können Sie den aktuellen Status der Azure-VPN-Verbindung überprüfen.</span><span class="sxs-lookup"><span data-stu-id="5cb34-269">You can use the following Azure PowerShell command to check the current status of the Azure VPN connection.</span></span> <span data-ttu-id="5cb34-270">Der Parameter `<<connection-name>>` ist der Name der Azure-VPN-Verbindung, die das virtuelle Netzwerkgateway mit dem lokalen Gateway verknüpft.</span><span class="sxs-lookup"><span data-stu-id="5cb34-270">The `<<connection-name>>` parameter is the name of the Azure VPN connection that links the virtual network gateway and the local gateway.</span></span>

    ```powershell
    Get-AzureRmVirtualNetworkGatewayConnection -Name <<connection-name>> - ResourceGroupName <<resource-group>>
    ```

    <span data-ttu-id="5cb34-271">In den folgenden Codeausschnitte ist die Ausgabe hervorgehoben, die erzeugt wird, wenn das Gateway verbunden (das erste Beispiel) und getrennt (das zweite Beispiel) ist:</span><span class="sxs-lookup"><span data-stu-id="5cb34-271">The following snippets highlight the output generated if the gateway is connected (the first example), and disconnected (the second example):</span></span>

    ```powershell
    PS C:\> Get-AzureRmVirtualNetworkGatewayConnection -Name profx-gateway-connection -ResourceGroupName profx-prod-rg

    AuthorizationKey           :
    VirtualNetworkGateway1     : Microsoft.Azure.Commands.Network.Models.PSVirtualNetworkGateway
    VirtualNetworkGateway2     :
    LocalNetworkGateway2       : Microsoft.Azure.Commands.Network.Models.PSLocalNetworkGateway
    Peer                       :
    ConnectionType             : IPsec
    RoutingWeight              : 0
    SharedKey                  : ####################################
    ConnectionStatus           : Connected
    EgressBytesTransferred     : 55254803
    IngressBytesTransferred    : 32227221
    ProvisioningState          : Succeeded
    ...
    ```

    ```powershell
    PS C:\> Get-AzureRmVirtualNetworkGatewayConnection -Name profx-gateway-connection2 -ResourceGroupName profx-prod-rg

    AuthorizationKey           :
    VirtualNetworkGateway1     : Microsoft.Azure.Commands.Network.Models.PSVirtualNetworkGateway
    VirtualNetworkGateway2     :
    LocalNetworkGateway2       : Microsoft.Azure.Commands.Network.Models.PSLocalNetworkGateway
    Peer                       :
    ConnectionType             : IPsec
    RoutingWeight              : 0
    SharedKey                  : ####################################
    ConnectionStatus           : NotConnected
    EgressBytesTransferred     : 0
    IngressBytesTransferred    : 0
    ProvisioningState          : Succeeded
    ...
    ```

<span data-ttu-id="5cb34-272">Die folgenden Empfehlungen sind nützlich, um festzustellen, ob es Probleme mit der Host-VM-Konfiguration, der Auslastung von Netzwerkbandbreite oder der Anwendungsleistung gibt:</span><span class="sxs-lookup"><span data-stu-id="5cb34-272">The following recommendations are useful for determining if there is an issue with Host VM configuration, network bandwidth utilization, or application performance:</span></span>

- <span data-ttu-id="5cb34-273">**Überprüfen Sie, ob die Firewall im Gastbetriebssystem, die auf den Azure-VMs im Subnetz ausgeführt wird, ordnungsgemäß konfiguriert ist, um zulässigen Datenverkehr aus den lokalen IP-Bereichen zuzulassen.**</span><span class="sxs-lookup"><span data-stu-id="5cb34-273">**Verify that the firewall in the guest operating system running on the Azure VMs in the subnet is configured correctly to allow permitted traffic from the on-premises IP ranges.**</span></span>

- <span data-ttu-id="5cb34-274">**Stellen Sie sicher, dass das Datenverkehrsvolumen nicht nahe an der Grenze der Bandbreite liegt, die dem Azure-VPN-Gateway zur Verfügung steht.**</span><span class="sxs-lookup"><span data-stu-id="5cb34-274">**Verify that the volume of traffic is not close to the limit of the bandwidth available to the Azure VPN gateway.**</span></span>

    <span data-ttu-id="5cb34-275">Wie Sie dies überprüfen können, hängt vom VPN-Gerät ab, das lokal ausgeführt wird.</span><span class="sxs-lookup"><span data-stu-id="5cb34-275">How to verify this depends on the VPN appliance running on-premises.</span></span> <span data-ttu-id="5cb34-276">Wenn Sie z.B. RRAS unter Windows Server 2012 verwenden, können Sie mit Systemmonitor die Menge der Daten verfolgen, die über die VPN-Verbindung empfangen und übertragen werden.</span><span class="sxs-lookup"><span data-stu-id="5cb34-276">For example, if you are using RRAS on Windows Server 2012, you can use Performance Monitor to track the volume of data being received and transmitted over the VPN connection.</span></span> <span data-ttu-id="5cb34-277">Wählen Sie unter Verwendung des Objekts *RAS insgesamt* die Leistungsindikatoren *Empfangene Bytes/Sek.* und *Übertragene Bytes/Sek.*:</span><span class="sxs-lookup"><span data-stu-id="5cb34-277">Using the *RAS Total* object, select the *Bytes Received/Sec* and *Bytes Transmitted/Sec* counters:</span></span>

    ![Leistungsindikatoren zum Überwachen von VPN-Netzwerkdatenverkehr](../_images/guidance-hybrid-network-vpn/RRAS-perf-counters.png)

    <span data-ttu-id="5cb34-279">Vergleichen Sie die Ergebnisse mit der Bandbreite, die dem VPN-Gateway zur Verfügung steht (von 100 MBit/s für die Basic-SKU bis 1,25 GBit/s für die VpnGw3-SKU):</span><span class="sxs-lookup"><span data-stu-id="5cb34-279">You should compare the results with the bandwidth available to the VPN gateway (from 100 Mbps for the Basic SKU to 1.25 Gbps for VpnGw3 SKU):</span></span>

    ![Beispieldiagramm für die Leistung eines VPN-Netzwerks](../_images/guidance-hybrid-network-vpn/RRAS-perf-graph.png)

- <span data-ttu-id="5cb34-281">**Vergewissern Sie sich, dass Sie die richtige Anzahl und Größe von VMs für Ihre Anwendungslast bereitgestellt haben.**</span><span class="sxs-lookup"><span data-stu-id="5cb34-281">**Verify that you have deployed the right number and size of VMs for your application load.**</span></span>

    <span data-ttu-id="5cb34-282">Stellen Sie fest, ob virtuelle Computer im Azure VNet langsam arbeiten.</span><span class="sxs-lookup"><span data-stu-id="5cb34-282">Determine if any of the virtual machines in the Azure VNet are running slowly.</span></span> <span data-ttu-id="5cb34-283">Falls ja, können sie überlastet sein, es können zu wenige sein, um die Last zu bewältigen, oder die Load Balancer sind möglicherweise nicht richtig konfiguriert.</span><span class="sxs-lookup"><span data-stu-id="5cb34-283">If so, they may be overloaded, there may be too few to handle the load, or the load-balancers may not be configured correctly.</span></span> <span data-ttu-id="5cb34-284">Um dies zu ermitteln, [müssen Sie Diagnoseinformationen aufzeichnen und analysieren][azure-vm-diagnostics].</span><span class="sxs-lookup"><span data-stu-id="5cb34-284">To determine this, [capture and analyze diagnostic information][azure-vm-diagnostics].</span></span> <span data-ttu-id="5cb34-285">Sie können die Ergebnisse im Azure-Portal einsehen. Es stehen aber auch viele Tools von Drittanbietern zur Verfügung, die detaillierte Einblicke in die Leistungsdaten ermöglichen.</span><span class="sxs-lookup"><span data-stu-id="5cb34-285">You can examine the results using the Azure portal, but many third-party tools are also available that can provide detailed insights into the performance data.</span></span>

- <span data-ttu-id="5cb34-286">**Vergewissern Sie sich, dass die Anwendung Cloudressourcen effizient nutzt.**</span><span class="sxs-lookup"><span data-stu-id="5cb34-286">**Verify that the application is making efficient use of cloud resources.**</span></span>

    <span data-ttu-id="5cb34-287">Instrumentieren Sie Anwendungscode, der auf jeder VM ausgeführt wird, um festzustellen, ob Anwendungen Ressourcen optimal nutzen.</span><span class="sxs-lookup"><span data-stu-id="5cb34-287">Instrument application code running on each VM to determine whether applications are making the best use of resources.</span></span> <span data-ttu-id="5cb34-288">Dafür können Sie Tools wie [Application Insights][application-insights] verwenden.</span><span class="sxs-lookup"><span data-stu-id="5cb34-288">You can use tools such as [Application Insights][application-insights].</span></span>

## <a name="deploy-the-solution"></a><span data-ttu-id="5cb34-289">Bereitstellen der Lösung</span><span class="sxs-lookup"><span data-stu-id="5cb34-289">Deploy the solution</span></span>

<span data-ttu-id="5cb34-290">**Voraussetzungen**</span><span class="sxs-lookup"><span data-stu-id="5cb34-290">**Prerequisites**.</span></span> <span data-ttu-id="5cb34-291">Sie müssen über eine lokale Infrastruktur verfügen, die bereits mit einer geeigneten Netzwerkappliance konfiguriert ist.</span><span class="sxs-lookup"><span data-stu-id="5cb34-291">You must have an existing on-premises infrastructure already configured with a suitable network appliance.</span></span>

<span data-ttu-id="5cb34-292">Führen Sie die folgenden Schritte aus, um die Lösung bereitzustellen.</span><span class="sxs-lookup"><span data-stu-id="5cb34-292">To deploy the solution, perform the following steps.</span></span>

<!-- markdownlint-disable MD033 -->

1. <span data-ttu-id="5cb34-293">Klicken Sie auf diese Schaltfläche:</span><span class="sxs-lookup"><span data-stu-id="5cb34-293">Click the button below:</span></span><br><a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2Fmspnp%2Freference-architectures%2Fmaster%2Fhybrid-networking%2Fvpn%2Fazuredeploy.json" target="_blank"><img src="https://azuredeploy.net/deploybutton.png"/></a>
2. <span data-ttu-id="5cb34-294">Warten Sie, bis der Link im Azure-Portal geöffnet wird, und gehen Sie dann folgendermaßen vor:</span><span class="sxs-lookup"><span data-stu-id="5cb34-294">Wait for the link to open in the Azure portal, then follow these steps:</span></span>
   - <span data-ttu-id="5cb34-295">Der Name der **Ressourcengruppe** ist bereits in der Parameterdatei definiert. Wählen Sie also **Neu erstellen**, und geben Sie im Textfeld `ra-hybrid-vpn-rg` ein.</span><span class="sxs-lookup"><span data-stu-id="5cb34-295">The **Resource group** name is already defined in the parameter file, so select **Create New** and enter `ra-hybrid-vpn-rg` in the text box.</span></span>
   - <span data-ttu-id="5cb34-296">Wählen Sie im Dropdownfeld **Standort** die Region aus.</span><span class="sxs-lookup"><span data-stu-id="5cb34-296">Select the region from the **Location** drop down box.</span></span>
   - <span data-ttu-id="5cb34-297">Lassen Sie die Textfelder für den **Vorlagenstamm-URI** bzw. **Parameterstamm-URI** unverändert.</span><span class="sxs-lookup"><span data-stu-id="5cb34-297">Do not edit the **Template Root Uri** or the **Parameter Root Uri** text boxes.</span></span>
   - <span data-ttu-id="5cb34-298">Überprüfen Sie die allgemeinen Geschäftsbedingungen, und aktivieren Sie dann das Kontrollkästchen **Ich stimme den oben genannten Geschäftsbedingungen zu**.</span><span class="sxs-lookup"><span data-stu-id="5cb34-298">Review the terms and conditions, then click the **I agree to the terms and conditions stated above** checkbox.</span></span>
   - <span data-ttu-id="5cb34-299">Klicken Sie auf die Schaltfläche **Kaufen**.</span><span class="sxs-lookup"><span data-stu-id="5cb34-299">Click the **Purchase** button.</span></span>
3. <span data-ttu-id="5cb34-300">Warten Sie, bis die Bereitstellung abgeschlossen ist.</span><span class="sxs-lookup"><span data-stu-id="5cb34-300">Wait for the deployment to complete.</span></span>

<!-- markdownlint-enable MD033 -->

<!-- links -->

[adds-extend-domain]: ../identity/adds-extend-domain.md
[expressroute]: ../hybrid-networking/expressroute.md
[windows-vm-ra]: ../virtual-machines-windows/index.md
[linux-vm-ra]: ../virtual-machines-linux/index.md

[naming conventions]: /azure/guidance/guidance-naming-conventions

[resource-manager-overview]: /azure/azure-resource-manager/resource-group-overview
[arm-templates]: /azure/resource-group-authoring-templates
[azure-cli]: /azure/virtual-machines-command-line-tools
[azure-portal]: /azure/azure-portal/resource-group-portal
[azure-powershell]: /azure/powershell-azure-resource-manager
[azure-virtual-network]: /azure/virtual-network/virtual-networks-overview
[vpn-appliance]: /azure/vpn-gateway/vpn-gateway-about-vpn-devices
[azure-vpn-gateway]: https://azure.microsoft.com/services/vpn-gateway/
[azure-gateway-charges]: https://azure.microsoft.com/pricing/details/vpn-gateway/
[azure-gateway-skus]: /azure/vpn-gateway/vpn-gateway-about-vpngateways#gwsku
[connect-to-an-Azure-vnet]: https://technet.microsoft.com/library/dn786406.aspx
[vpn-gateway-multi-site]: /azure/vpn-gateway/vpn-gateway-multi-site
[policy-based-routing]: https://en.wikipedia.org/wiki/Policy-based_routing
[route-based-routing]: https://en.wikipedia.org/wiki/Static_routing
[network-security-group]: /azure/virtual-network/virtual-networks-nsg
[sla-for-vpn-gateway]: https://azure.microsoft.com/support/legal/sla/vpn-gateway/v1_2/
[additional-firewall-rules]: https://technet.microsoft.com/library/dn786406.aspx#firewall
[nagios]: https://www.nagios.org/
[azure-vpn-gateway-diagnostics]: https://blogs.technet.microsoft.com/keithmayer/2014/12/18/diagnose-azure-virtual-network-vpn-connectivity-issues-with-powershell/
[ping]: https://technet.microsoft.com/library/ff961503.aspx
[tracert]: https://technet.microsoft.com/library/ff961507.aspx
[psping]: https://technet.microsoft.com/sysinternals/jj729731.aspx
[nmap]: https://nmap.org
[changing-SKUs]: https://azure.microsoft.com/blog/azure-virtual-network-gateway-improvements/
[gateway-diagnostic-logs]: https://blogs.technet.microsoft.com/keithmayer/2016/10/12/step-by-step-capturing-azure-resource-manager-arm-vnet-gateway-diagnostic-logs/
[troubleshooting-vpn-errors]: https://blogs.technet.microsoft.com/rrasblog/2009/08/12/troubleshooting-common-vpn-related-errors/
[rras-logging]: https://www.petri.com/enable-diagnostic-logging-in-windows-server-2012-r2-routing-and-remote-access
[create-on-prem-network]: https://technet.microsoft.com/library/dn786406.aspx#routing
[azure-vm-diagnostics]: https://azure.microsoft.com/blog/windows-azure-virtual-machine-monitoring-with-wad-extension/
[application-insights]: /azure/application-insights/app-insights-overview-usage
[forced-tunneling]: https://azure.microsoft.com/documentation/articles/vpn-gateway-about-forced-tunneling/
[vpn-appliances]: /azure/vpn-gateway/vpn-gateway-about-vpn-devices
[visio-download]: https://archcenter.blob.core.windows.net/cdn/hybrid-network-architectures.vsdx
[vpn-appliance-ipsec]: /azure/vpn-gateway/vpn-gateway-about-vpn-devices#ipsec-parameters
[virtualNetworkGateway-parameters]: https://github.com/mspnp/hybrid-networking/vpn/parameters/virtualNetworkGateway.parameters.json
[azure-cli]: https://azure.microsoft.com/documentation/articles/xplat-cli-install/
[CIDR]: https://en.wikipedia.org/wiki/Classless_Inter-Domain_Routing
