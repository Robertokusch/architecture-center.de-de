---
title: Implementieren einer DMZ zwischen Azure und dem Internet
titleSuffix: Azure Reference Architectures
description: Erfahren Sie, wie Sie eine sichere Hybrid-Netzwerkarchitektur mit Internetzugriff in Azure implementieren.
author: telmosampaio
ms.date: 10/22/2018
ms.topic: reference-architecture
ms.service: architecture-center
ms.subservice: reference-architecture
ms.custom: seodec18, networking
ms.openlocfilehash: 80125626d0c79888445bc7828577846bcce9fc67
ms.sourcegitcommit: 1b50810208354577b00e89e5c031b774b02736e2
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/23/2019
ms.locfileid: "54488221"
---
# <a name="implement-a-dmz-between-azure-and-the-internet"></a><span data-ttu-id="f184c-103">Implementieren einer DMZ zwischen Azure und dem Internet</span><span class="sxs-lookup"><span data-stu-id="f184c-103">Implement a DMZ between Azure and the Internet</span></span>

<span data-ttu-id="f184c-104">Diese Referenzarchitektur zeigt ein sicheres Hybridnetzwerk, das ein lokales Netzwerk in Azure erweitert und auch Internetdatenverkehr zulässt.</span><span class="sxs-lookup"><span data-stu-id="f184c-104">This reference architecture shows a secure hybrid network that extends an on-premises network to Azure and also accepts Internet traffic.</span></span> <span data-ttu-id="f184c-105">[**Stellen Sie diese Lösung bereit**](#deploy-the-solution).</span><span class="sxs-lookup"><span data-stu-id="f184c-105">[**Deploy this solution**](#deploy-the-solution).</span></span>

> [!NOTE]
> <span data-ttu-id="f184c-106">Dieses Szenario kann auch mithilfe von [Azure Firewall](/azure/firewall/), einem cloudbasierten Netzwerksicherheitsdienst, erreicht werden.</span><span class="sxs-lookup"><span data-stu-id="f184c-106">This scenario can also be accomplished using [Azure Firewall](/azure/firewall/), a cloud-based network security service.</span></span>

![Sichere Hybrid-Netzwerkarchitektur](./images/dmz-public.png)

<span data-ttu-id="f184c-108">*Laden Sie eine [Visio-Datei][visio-download] mit dieser Architektur herunter.*</span><span class="sxs-lookup"><span data-stu-id="f184c-108">*Download a [Visio file][visio-download] of this architecture.*</span></span>

<span data-ttu-id="f184c-109">Diese Referenzarchitektur erweitert die Architektur, die in [Implementieren einer DMZ zwischen Azure und Ihrem lokalen Rechenzentrum][implementing-a-secure-hybrid-network-architecture] beschrieben wird.</span><span class="sxs-lookup"><span data-stu-id="f184c-109">This reference architecture extends the architecture described in [Implementing a DMZ between Azure and your on-premises datacenter][implementing-a-secure-hybrid-network-architecture].</span></span> <span data-ttu-id="f184c-110">Dabei wird der privaten DMZ, die den Datenverkehr aus dem lokalen Netzwerk verwaltet, eine öffentliche DMZ für den Internetdatenverkehr hinzugefügt.</span><span class="sxs-lookup"><span data-stu-id="f184c-110">It adds a public DMZ that handles Internet traffic, in addition to the private DMZ that handles traffic from the on-premises network.</span></span>

<span data-ttu-id="f184c-111">Typische Einsatzmöglichkeiten für diese Architektur sind:</span><span class="sxs-lookup"><span data-stu-id="f184c-111">Typical uses for this architecture include:</span></span>

- <span data-ttu-id="f184c-112">Hybridanwendungen, in denen Workloads teilweise lokal und teilweise in Azure ausgeführt werden.</span><span class="sxs-lookup"><span data-stu-id="f184c-112">Hybrid applications where workloads run partly on-premises and partly in Azure.</span></span>
- <span data-ttu-id="f184c-113">Azure-Infrastruktur, die eingehenden Datenverkehr von lokalen Quellen und dem Internet weiterleitet</span><span class="sxs-lookup"><span data-stu-id="f184c-113">Azure infrastructure that routes incoming traffic from on-premises and the Internet.</span></span>

## <a name="architecture"></a><span data-ttu-id="f184c-114">Architecture</span><span class="sxs-lookup"><span data-stu-id="f184c-114">Architecture</span></span>

<span data-ttu-id="f184c-115">Die Architektur umfasst die folgenden Komponenten.</span><span class="sxs-lookup"><span data-stu-id="f184c-115">The architecture consists of the following components.</span></span>

- <span data-ttu-id="f184c-116">**Öffentliche IP-Adresse:**</span><span class="sxs-lookup"><span data-stu-id="f184c-116">**Public IP address (PIP)**.</span></span> <span data-ttu-id="f184c-117">Die IP-Adresse des öffentlichen Endpunkts.</span><span class="sxs-lookup"><span data-stu-id="f184c-117">The IP address of the public endpoint.</span></span> <span data-ttu-id="f184c-118">Externe Benutzer, die mit dem Internet verbunden sind, können über diese Adresse auf das System zugreifen.</span><span class="sxs-lookup"><span data-stu-id="f184c-118">External users connected to the Internet can access the system through this address.</span></span>
- <span data-ttu-id="f184c-119">**Virtuelles Netzwerkgerät:**</span><span class="sxs-lookup"><span data-stu-id="f184c-119">**Network virtual appliance (NVA)**.</span></span> <span data-ttu-id="f184c-120">Diese Architektur umfasst einen separaten Pool von virtuellen Netzwerkgeräten (Network Virtual Appliance, NVA) für Datenverkehr aus dem Internet.</span><span class="sxs-lookup"><span data-stu-id="f184c-120">This architecture includes a separate pool of NVAs for traffic originating on the Internet.</span></span>
- <span data-ttu-id="f184c-121">**Azure Load Balancer:**</span><span class="sxs-lookup"><span data-stu-id="f184c-121">**Azure load balancer**.</span></span> <span data-ttu-id="f184c-122">Alle aus dem Internet eingehende Anforderungen werden über den Lastenausgleich auf die NVAs in der öffentlichen DMZ verteilt.</span><span class="sxs-lookup"><span data-stu-id="f184c-122">All incoming requests from the Internet pass through the load balancer and are distributed to the NVAs in the public DMZ.</span></span>
- <span data-ttu-id="f184c-123">**Öffentliche DMZ – eingehendes Subnetz:**</span><span class="sxs-lookup"><span data-stu-id="f184c-123">**Public DMZ inbound subnet**.</span></span> <span data-ttu-id="f184c-124">Dieses Subnetz akzeptiert Anforderungen vom Azure Load Balancer.</span><span class="sxs-lookup"><span data-stu-id="f184c-124">This subnet accepts requests from the Azure load balancer.</span></span> <span data-ttu-id="f184c-125">Eingehende Anforderungen werden an eine der NVAs in der öffentlichen DMZ weitergeleitet.</span><span class="sxs-lookup"><span data-stu-id="f184c-125">Incoming requests are passed to one of the NVAs in the public DMZ.</span></span>
- <span data-ttu-id="f184c-126">**Öffentliche DMZ – ausgehendes Subnetz:**</span><span class="sxs-lookup"><span data-stu-id="f184c-126">**Public DMZ outbound subnet**.</span></span> <span data-ttu-id="f184c-127">Anforderungen, die von der NVA genehmigt wurden, werden über dieses Subnetz an den internen Lastenausgleich für die Webebene weitergeleitet.</span><span class="sxs-lookup"><span data-stu-id="f184c-127">Requests that are approved by the NVA pass through this subnet to the internal load balancer for the web tier.</span></span>

## <a name="recommendations"></a><span data-ttu-id="f184c-128">Empfehlungen</span><span class="sxs-lookup"><span data-stu-id="f184c-128">Recommendations</span></span>

<span data-ttu-id="f184c-129">Die folgenden Empfehlungen gelten für die meisten Szenarios.</span><span class="sxs-lookup"><span data-stu-id="f184c-129">The following recommendations apply for most scenarios.</span></span> <span data-ttu-id="f184c-130">Sofern Sie keine besonderen Anforderungen haben, die Vorrang haben, sollten Sie diese Empfehlungen befolgen.</span><span class="sxs-lookup"><span data-stu-id="f184c-130">Follow these recommendations unless you have a specific requirement that overrides them.</span></span>

### <a name="nva-recommendations"></a><span data-ttu-id="f184c-131">Empfehlungen für virtuelle Netzwerkgeräte</span><span class="sxs-lookup"><span data-stu-id="f184c-131">NVA recommendations</span></span>

<span data-ttu-id="f184c-132">Verwenden Sie einen Satz von NVAs für Datenverkehr aus dem Internet und einen anderen für Datenverkehr mit lokalem Ursprung.</span><span class="sxs-lookup"><span data-stu-id="f184c-132">Use one set of NVAs for traffic originating on the Internet, and another for traffic originating on-premises.</span></span> <span data-ttu-id="f184c-133">Wenn Sie für beides nur einen Satz von NVAs verwenden, stellt dies ein Sicherheitsrisiko dar, da kein wirksamer Sicherheitsbereich zwischen den beiden Arten von Netzwerkdatenverkehr aufgebaut wird.</span><span class="sxs-lookup"><span data-stu-id="f184c-133">Using only one set of NVAs for both is a security risk, because it provides no security perimeter between the two sets of network traffic.</span></span> <span data-ttu-id="f184c-134">Separate NVAs erleichtern das Überprüfen von Sicherheitsregeln und verdeutlichen, welche Regeln der eingehenden Netzwerkanforderung entsprechen.</span><span class="sxs-lookup"><span data-stu-id="f184c-134">Using separate NVAs reduces the complexity of checking security rules, and makes it clear which rules correspond to each incoming network request.</span></span> <span data-ttu-id="f184c-135">Einen Satz von NVAs implementiert nur Regeln für den Internetdatenverkehr, während ein anderer Satz von NVAs nur Regeln für den lokalen Datenverkehr implementiert.</span><span class="sxs-lookup"><span data-stu-id="f184c-135">One set of NVAs implements rules for Internet traffic only, while another set of NVAs implement rules for on-premises traffic only.</span></span>

<span data-ttu-id="f184c-136">Schließen Sie eine Ebene-7-NVA ein, um Anwendungsverbindungen auf NVA-Ebene zu beenden und Kompatibilität zwischen den Back-End-Ebenen sicherzustellen.</span><span class="sxs-lookup"><span data-stu-id="f184c-136">Include a layer-7 NVA to terminate application connections at the NVA level and maintain compatibility with the backend tiers.</span></span> <span data-ttu-id="f184c-137">Dies garantiert symmetrischen Konnektivität, bei der Antwortdatenverkehr von den Back-End-Ebenen über die NVA zurückgegeben wird.</span><span class="sxs-lookup"><span data-stu-id="f184c-137">This guarantees symmetric connectivity where response traffic from the backend tiers returns through the NVA.</span></span>

### <a name="public-load-balancer-recommendations"></a><span data-ttu-id="f184c-138">Empfehlungen für den öffentlichen Lastenausgleich</span><span class="sxs-lookup"><span data-stu-id="f184c-138">Public load balancer recommendations</span></span>

<span data-ttu-id="f184c-139">Für mehr Skalierbarkeit und Verfügbarkeit stellen Sie die NVAs der öffentlichen DMZ in einer [Verfügbarkeitsgruppe][availability-set] bereit und verwenden einen [Lastenausgleich mit Internetzugriff][load-balancer], um Internetanforderungen auf die NVAs in der Verfügbarkeitsgruppe zu verteilen.</span><span class="sxs-lookup"><span data-stu-id="f184c-139">For scalability and availability, deploy the public DMZ NVAs in an [availability set][availability-set] and use an [Internet facing load balancer][load-balancer] to distribute Internet requests across the NVAs in the availability set.</span></span>

<span data-ttu-id="f184c-140">Konfigurieren Sie den Lastenausgleich so, dass dieser nur Anforderungen über die Ports für den Internetdatenverkehr akzeptiert.</span><span class="sxs-lookup"><span data-stu-id="f184c-140">Configure the load balancer to accept requests only on the ports necessary for Internet traffic.</span></span> <span data-ttu-id="f184c-141">Beschränken Sie z.B. eingehende HTTP-Anforderungen auf Port 80 und eingehende HTTPS-Anforderungen auf Port 443.</span><span class="sxs-lookup"><span data-stu-id="f184c-141">For example, restrict inbound HTTP requests to port 80 and inbound HTTPS requests to port 443.</span></span>

## <a name="scalability-considerations"></a><span data-ttu-id="f184c-142">Überlegungen zur Skalierbarkeit</span><span class="sxs-lookup"><span data-stu-id="f184c-142">Scalability considerations</span></span>

<span data-ttu-id="f184c-143">Auch wenn Ihre Architektur anfänglich eine einzelne NVA in der öffentlichen DMZ erfordert, wird empfohlen, von Anfang an einen Lastenausgleich vor der öffentlichen DMZ zu platzieren.</span><span class="sxs-lookup"><span data-stu-id="f184c-143">Even if your architecture initially requires a single NVA in the public DMZ, we recommend putting a load balancer in front of the public DMZ from the beginning.</span></span> <span data-ttu-id="f184c-144">Dies vereinfacht die Skalierung auf mehrere NVAs, wenn dies in der Zukunft erforderlich wird.</span><span class="sxs-lookup"><span data-stu-id="f184c-144">That will make it easier to scale to multiple NVAs in the future, if needed.</span></span>

## <a name="availability-considerations"></a><span data-ttu-id="f184c-145">Überlegungen zur Verfügbarkeit</span><span class="sxs-lookup"><span data-stu-id="f184c-145">Availability considerations</span></span>

<span data-ttu-id="f184c-146">Für den Lastenausgleich mit Internetzugriff muss jede NVA im eingehenden Subnetz der öffentlichen DMZ einen [Integritätstest][lb-probe] implementieren.</span><span class="sxs-lookup"><span data-stu-id="f184c-146">The Internet facing load balancer requires each NVA in the public DMZ inbound subnet to implement a [health probe][lb-probe].</span></span> <span data-ttu-id="f184c-147">Wenn ein Integritätstest an diesem Endpunkt nicht antwortet, gilt er als nicht verfügbar. Der Lastenausgleich leitet Anforderungen dann an andere NVAs in derselben Verfügbarkeitsgruppe um.</span><span class="sxs-lookup"><span data-stu-id="f184c-147">A health probe that fails to respond on this endpoint is considered to be unavailable, and the load balancer will direct requests to other NVAs in the same availability set.</span></span> <span data-ttu-id="f184c-148">Bedenken Sie, dass ein Fehler bei der Anwendung auftritt, wenn keine der NVAs antwortet. Daher ist es wichtig, eine Überwachung zu konfigurieren, mit der die DevOps gewarnt werden, wenn die Anzahl der fehlerfreien NVAs einen festgelegten Schwellenwert unterschreitet.</span><span class="sxs-lookup"><span data-stu-id="f184c-148">Note that if all NVAs fail to respond, your application will fail, so it's important to have monitoring configured to alert DevOps when the number of healthy NVA instances falls below a defined threshold.</span></span>

## <a name="manageability-considerations"></a><span data-ttu-id="f184c-149">Überlegungen zur Verwaltbarkeit</span><span class="sxs-lookup"><span data-stu-id="f184c-149">Manageability considerations</span></span>

<span data-ttu-id="f184c-150">Sämtliche Vorgänge zur Überwachung und Verwaltung der NVAs in der öffentlichen DMZ sollten über die Jumpbox im Verwaltungssubnetz ausgeführt werden.</span><span class="sxs-lookup"><span data-stu-id="f184c-150">All monitoring and management for the NVAs in the public DMZ should be performed by the jumpbox in the management subnet.</span></span> <span data-ttu-id="f184c-151">Wie bereits unter [Implementieren einer DMZ zwischen Azure und Ihrem lokalen Rechenzentrum][implementing-a-secure-hybrid-network-architecture] erläutert, definieren Sie eine einzelne Netzwerkroute aus dem lokalen Netzwerk über das Gateway bis zur Jumpbox, um den Zugriff einzuschränken.</span><span class="sxs-lookup"><span data-stu-id="f184c-151">As discussed in [Implementing a DMZ between Azure and your on-premises datacenter][implementing-a-secure-hybrid-network-architecture], define a single network route from the on-premises network through the gateway to the jumpbox, in order to restrict access.</span></span>

<span data-ttu-id="f184c-152">Wenn keine Gatewaykonnektivität zwischen Ihrem lokalen Netzwerk und Azure besteht, können Sie weiterhin auf die Jumpbox zugreifen, indem Sie eine öffentliche IP-Adresse bereitstellen, diese der Jumpbox hinzufügen und sich dann über das Internet anmelden.</span><span class="sxs-lookup"><span data-stu-id="f184c-152">If gateway connectivity from your on-premises network to Azure is down, you can still reach the jumpbox by deploying a public IP address, adding it to the jumpbox, and logging in from the Internet.</span></span>

## <a name="security-considerations"></a><span data-ttu-id="f184c-153">Sicherheitshinweise</span><span class="sxs-lookup"><span data-stu-id="f184c-153">Security considerations</span></span>

<span data-ttu-id="f184c-154">Diese Referenzarchitektur implementiert mehrere Sicherheitsebenen:</span><span class="sxs-lookup"><span data-stu-id="f184c-154">This reference architecture implements multiple levels of security:</span></span>

- <span data-ttu-id="f184c-155">Der Lastenausgleich mit Internetzugriff leitet Anforderungen an die NVAs im eingehenden Subnetz der öffentlichen DMZ weiter – und zwar nur an Ports, die für die Anwendung erforderlich sind.</span><span class="sxs-lookup"><span data-stu-id="f184c-155">The Internet facing load balancer directs requests to the NVAs in the inbound public DMZ subnet, and only on the ports necessary for the application.</span></span>
- <span data-ttu-id="f184c-156">Die NSG-Regeln für die ein- und ausgehenden Subnetze der öffentlichen DMZ verhindern, dass die NVAs gefährdet werden, indem sie Anforderungen, auf die die NSG-Regeln nicht zutreffen, blockieren.</span><span class="sxs-lookup"><span data-stu-id="f184c-156">The NSG rules for the inbound and outbound public DMZ subnets prevent the NVAs from being compromised, by blocking requests that fall outside of the NSG rules.</span></span>
- <span data-ttu-id="f184c-157">Die NAT-Routingkonfiguration für die NVAs leitet eingehende Anforderungen an Port 80 und Port 443 an den Lastenausgleich für die Webebene weiter, ignoriert jedoch Anforderungen an allen anderen Ports.</span><span class="sxs-lookup"><span data-stu-id="f184c-157">The NAT routing configuration for the NVAs directs incoming requests on port 80 and port 443 to the web tier load balancer, but ignores requests on all other ports.</span></span>

<span data-ttu-id="f184c-158">Sie sollten alle eingehenden Anforderungen an sämtlichen Ports protokollieren.</span><span class="sxs-lookup"><span data-stu-id="f184c-158">You should log all incoming requests on all ports.</span></span> <span data-ttu-id="f184c-159">Überprüfen Sie die Protokolle regelmäßig, und achten Sie dabei auf Anforderungen, die außerhalb der erwarteten Parameter liegen, da sie auf Eindringversuche hindeuten.</span><span class="sxs-lookup"><span data-stu-id="f184c-159">Regularly audit the logs, paying attention to requests that fall outside of expected parameters, as these may indicate intrusion attempts.</span></span>

## <a name="deploy-the-solution"></a><span data-ttu-id="f184c-160">Bereitstellen der Lösung</span><span class="sxs-lookup"><span data-stu-id="f184c-160">Deploy the solution</span></span>

<span data-ttu-id="f184c-161">Eine Bereitstellung für eine Referenzarchitektur, die diese Empfehlungen implementiert, steht auf [GitHub][github-folder] zur Verfügung.</span><span class="sxs-lookup"><span data-stu-id="f184c-161">A deployment for a reference architecture that implements these recommendations is available on [GitHub][github-folder].</span></span>

### <a name="prerequisites"></a><span data-ttu-id="f184c-162">Voraussetzungen</span><span class="sxs-lookup"><span data-stu-id="f184c-162">Prerequisites</span></span>

[!INCLUDE [ref-arch-prerequisites.md](../../../includes/ref-arch-prerequisites.md)]

### <a name="deploy-resources"></a><span data-ttu-id="f184c-163">Bereitstellen von Ressourcen</span><span class="sxs-lookup"><span data-stu-id="f184c-163">Deploy resources</span></span>

1. <span data-ttu-id="f184c-164">Navigieren Sie zum Ordner `/dmz/secure-vnet-dmz` des GitHub-Repositorys für Referenzarchitekturen.</span><span class="sxs-lookup"><span data-stu-id="f184c-164">Navigate to the `/dmz/secure-vnet-dmz` folder of the reference architectures GitHub repository.</span></span>

2. <span data-ttu-id="f184c-165">Führen Sie den folgenden Befehl aus:</span><span class="sxs-lookup"><span data-stu-id="f184c-165">Run the following command:</span></span>

    ```bash
    azbb -s <subscription_id> -g <resource_group_name> -l <region> -p onprem.json --deploy
    ```

3. <span data-ttu-id="f184c-166">Führen Sie den folgenden Befehl aus:</span><span class="sxs-lookup"><span data-stu-id="f184c-166">Run the following command:</span></span>

    ```bash
    azbb -s <subscription_id> -g <resource_group_name> -l <region> -p secure-vnet-hybrid.json --deploy
    ```

### <a name="connect-the-on-premises-and-azure-gateways"></a><span data-ttu-id="f184c-167">Verbinden der lokalen und der Azure-Gateways</span><span class="sxs-lookup"><span data-stu-id="f184c-167">Connect the on-premises and Azure gateways</span></span>

<span data-ttu-id="f184c-168">In diesem Schritt verbinden Sie die zwei Gateways des lokalen Netzwerks.</span><span class="sxs-lookup"><span data-stu-id="f184c-168">In this step, you will connect the two local network gateways.</span></span>

1. <span data-ttu-id="f184c-169">Navigieren Sie im Azure-Portal zu der Ressourcengruppe, die Sie erstellt haben.</span><span class="sxs-lookup"><span data-stu-id="f184c-169">In the Azure Portal, navigate to the resource group that you created.</span></span>

2. <span data-ttu-id="f184c-170">Suchen Sie nach der Ressource mit dem Namen `ra-vpn-vgw-pip`, und kopieren Sie die auf dem Blatt **Übersicht** angezeigte IP-Adresse.</span><span class="sxs-lookup"><span data-stu-id="f184c-170">Find the resource named `ra-vpn-vgw-pip` and copy the IP address shown in the **Overview** blade.</span></span>

3. <span data-ttu-id="f184c-171">Suchen Sie nach der Ressource mit dem Namen `onprem-vpn-lgw`.</span><span class="sxs-lookup"><span data-stu-id="f184c-171">Find the resource named `onprem-vpn-lgw`.</span></span>

4. <span data-ttu-id="f184c-172">Klicken Sie auf das Blatt **Konfiguration**.</span><span class="sxs-lookup"><span data-stu-id="f184c-172">Click the **Configuration** blade.</span></span> <span data-ttu-id="f184c-173">Fügen Sie unter **IP-Adresse** die IP-Adresse aus Schritt 2 ein.</span><span class="sxs-lookup"><span data-stu-id="f184c-173">Under **IP address**, paste in the IP address from step 2.</span></span>

    ![Screenshot des Felds für die IP-Adresse](./images/local-net-gw.png)

5. <span data-ttu-id="f184c-175">Klicken Sie auf **Speichern**, und warten Sie, bis der Vorgang abgeschlossen ist.</span><span class="sxs-lookup"><span data-stu-id="f184c-175">Click **Save** and wait for the operation to complete.</span></span> <span data-ttu-id="f184c-176">Dies dauert etwa fünf Minuten.</span><span class="sxs-lookup"><span data-stu-id="f184c-176">It can take about 5 minutes.</span></span>

6. <span data-ttu-id="f184c-177">Suchen Sie nach der Ressource mit dem Namen `onprem-vpn-gateway1-pip`.</span><span class="sxs-lookup"><span data-stu-id="f184c-177">Find the resource named `onprem-vpn-gateway1-pip`.</span></span> <span data-ttu-id="f184c-178">Kopieren Sie die auf dem Blatt **Übersicht** angezeigte IP-Adresse.</span><span class="sxs-lookup"><span data-stu-id="f184c-178">Copy the IP address shown in the **Overview** blade.</span></span>

7. <span data-ttu-id="f184c-179">Suchen Sie nach der Ressource mit dem Namen `ra-vpn-lgw`.</span><span class="sxs-lookup"><span data-stu-id="f184c-179">Find the resource named `ra-vpn-lgw`.</span></span>

8. <span data-ttu-id="f184c-180">Klicken Sie auf das Blatt **Konfiguration**.</span><span class="sxs-lookup"><span data-stu-id="f184c-180">Click the **Configuration** blade.</span></span> <span data-ttu-id="f184c-181">Fügen Sie unter **IP-Adresse** die IP-Adresse aus Schritt 6 ein.</span><span class="sxs-lookup"><span data-stu-id="f184c-181">Under **IP address**, paste in the IP address from step 6.</span></span>

9. <span data-ttu-id="f184c-182">Klicken Sie auf **Speichern**, und warten Sie, bis der Vorgang abgeschlossen ist.</span><span class="sxs-lookup"><span data-stu-id="f184c-182">Click **Save** and wait for the operation to complete.</span></span>

10. <span data-ttu-id="f184c-183">Zum Überprüfen der Verbindung rufen Sie das Blatt **Verbindungen** für jedes Gateway auf.</span><span class="sxs-lookup"><span data-stu-id="f184c-183">To verify the connection, go to the **Connections** blade for each gateway.</span></span> <span data-ttu-id="f184c-184">Der Status sollte **Verbunden** lauten.</span><span class="sxs-lookup"><span data-stu-id="f184c-184">The status should be **Connected**.</span></span>

### <a name="verify-that-network-traffic-reaches-the-web-tier"></a><span data-ttu-id="f184c-185">Sicherstellen, dass Netzwerkdatenverkehr die Webebene erreicht</span><span class="sxs-lookup"><span data-stu-id="f184c-185">Verify that network traffic reaches the web tier</span></span>

1. <span data-ttu-id="f184c-186">Navigieren Sie im Azure-Portal zu der Ressourcengruppe, die Sie erstellt haben.</span><span class="sxs-lookup"><span data-stu-id="f184c-186">In the Azure Portal, navigate to the resource group that you created.</span></span>

2. <span data-ttu-id="f184c-187">Suchen Sie nach der Ressource mit dem Namen `pub-dmz-lb`, d.h. dem Lastenausgleich vor der öffentlichen DMZ.</span><span class="sxs-lookup"><span data-stu-id="f184c-187">Find the resource named `pub-dmz-lb`, which is the load balancer in front of the public DMZ.</span></span>

3. <span data-ttu-id="f184c-188">Kopieren Sie die öffentliche IP-Adresse aus dem Blatt **Übersicht**, und öffnen Sie diese Adresse in einem Webbrowser.</span><span class="sxs-lookup"><span data-stu-id="f184c-188">Copy the public IP addess from the **Overview** blade and open this address in a web browser.</span></span> <span data-ttu-id="f184c-189">Daraufhin sollte die standardmäßige Apache2-Serverhomepage angezeigt werden.</span><span class="sxs-lookup"><span data-stu-id="f184c-189">You should see the default Apache2 server home page.</span></span>

4. <span data-ttu-id="f184c-190">Suchen Sie nach der Ressource mit dem Namen `int-dmz-lb`, d.h. dem Lastenausgleich vor der privaten DMZ.</span><span class="sxs-lookup"><span data-stu-id="f184c-190">Find the resource named `int-dmz-lb`, which is the load balancer in front of the private DMZ.</span></span> <span data-ttu-id="f184c-191">Kopieren Sie die private IP-Adresse aus dem Blatt **Übersicht**.</span><span class="sxs-lookup"><span data-stu-id="f184c-191">Copy the private IP address from the **Overview** blade.</span></span>

5. <span data-ttu-id="f184c-192">Suchen Sie den virtuellen Computer mit dem Namen `jb-vm1`.</span><span class="sxs-lookup"><span data-stu-id="f184c-192">Find the VM named `jb-vm1`.</span></span> <span data-ttu-id="f184c-193">Klicken Sie auf **Verbinden**, und stellen Sie über Remotedesktop eine Verbindung zum virtuellen Computer her.</span><span class="sxs-lookup"><span data-stu-id="f184c-193">Click **Connect** and use Remote Desktop to connect to the VM.</span></span> <span data-ttu-id="f184c-194">Der Benutzername und das Kennwort sind in der Datei „onprem.json“ angegeben.</span><span class="sxs-lookup"><span data-stu-id="f184c-194">The user name and password are specified in the onprem.json file.</span></span>

6. <span data-ttu-id="f184c-195">Öffnen Sie in der Remotedesktopsitzung einen Webbrowser, und navigieren Sie zur IP-Adresse aus Schritt 4.</span><span class="sxs-lookup"><span data-stu-id="f184c-195">From the Remote Desktop Session, open a web browser and navigate to the IP address from step 4.</span></span> <span data-ttu-id="f184c-196">Daraufhin sollte die standardmäßige Apache2-Serverhomepage angezeigt werden.</span><span class="sxs-lookup"><span data-stu-id="f184c-196">You should see the default Apache2 server home page.</span></span>

[availability-set]: /azure/virtual-machines/virtual-machines-windows-manage-availability
[github-folder]: https://github.com/mspnp/reference-architectures/tree/master/dmz/secure-vnet-dmz

[implementing-a-secure-hybrid-network-architecture]: ./secure-vnet-hybrid.md
[iptables]: https://help.ubuntu.com/community/IptablesHowTo
[lb-probe]: /azure/load-balancer/load-balancer-custom-probe-overview
[load-balancer]: /azure/load-balancer/load-balancer-Internet-overview
[network-security-group]: /azure/virtual-network/virtual-networks-nsg

[visio-download]: https://archcenter.blob.core.windows.net/cdn/dmz-reference-architectures.vsdx
