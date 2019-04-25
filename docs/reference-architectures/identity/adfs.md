---
title: Erweitern einer lokalen AD FS-Instanz auf Azure
titleSuffix: Azure Reference Architectures
description: Es wird beschrieben, wie Sie eine sichere hybride Netzwerkarchitektur mit Active Directory-Verbunddienst-Autorisierung in Azure implementieren.
author: telmosampaio
ms.date: 12/18.2018
ms.topic: reference-architecture
ms.service: architecture-center
ms.subservice: reference-architecture
ms.custom: seodec18, identity
ms.openlocfilehash: 873b6a86da14e00d0a537f910d10922444cc1ded
ms.sourcegitcommit: 579c39ff4b776704ead17a006bf24cd4cdc65edd
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/17/2019
ms.locfileid: "59640735"
---
# <a name="extend-active-directory-federation-services-ad-fs-to-azure"></a><span data-ttu-id="5410b-103">Erweitern von Active Directory Federation Services (AD FS) auf Azure</span><span class="sxs-lookup"><span data-stu-id="5410b-103">Extend Active Directory Federation Services (AD FS) to Azure</span></span>

<span data-ttu-id="5410b-104">Mit dieser Referenzarchitektur wird ein sicheres hybrides Netzwerk implementiert, das Ihr lokales Netzwerk auf Azure erweitert, und [Active Directory-Verbunddienste (AD FS)][active-directory-federation-services] verwendet, um Verbundauthentifizierung und Autorisierung für Komponenten auszuführen, die in Azure ausgeführt werden.</span><span class="sxs-lookup"><span data-stu-id="5410b-104">This reference architecture implements a secure hybrid network that extends your on-premises network to Azure and uses [Active Directory Federation Services (AD FS)][active-directory-federation-services] to perform federated authentication and authorization for components running in Azure.</span></span> <span data-ttu-id="5410b-105">[**Stellen Sie diese Lösung bereit**](#deploy-the-solution).</span><span class="sxs-lookup"><span data-stu-id="5410b-105">[**Deploy this solution**](#deploy-the-solution).</span></span>

![Schützen einer Hybrid-Netzwerkarchitektur mit Active Directory](./images/adfs.png)

<span data-ttu-id="5410b-107">*Laden Sie eine [Visio-Datei][visio-download] mit dieser Architektur herunter.*</span><span class="sxs-lookup"><span data-stu-id="5410b-107">*Download a [Visio file][visio-download] of this architecture.*</span></span>

<span data-ttu-id="5410b-108">AD FS kann lokal gehostet werden, aber wenn Ihre Anwendung eine hybride Anwendung ist, in der einige Teile in Azure implementiert sind, kann es effizienter sein, AD FS in der Cloud zu replizieren.</span><span class="sxs-lookup"><span data-stu-id="5410b-108">AD FS can be hosted on-premises, but if your application is a hybrid in which some parts are implemented in Azure, it may be more efficient to replicate AD FS in the cloud.</span></span>

<span data-ttu-id="5410b-109">Im Diagramm sind folgende Szenarios dargestellt:</span><span class="sxs-lookup"><span data-stu-id="5410b-109">The diagram shows the following scenarios:</span></span>

- <span data-ttu-id="5410b-110">Anwendungscode von einer Partnerorganisation greift auf eine Webanwendung zu, die in Ihrem Azure VNet gehostet wird.</span><span class="sxs-lookup"><span data-stu-id="5410b-110">Application code from a partner organization accesses a web application hosted inside your Azure VNet.</span></span>
- <span data-ttu-id="5410b-111">Ein externer registrierter Benutzer mit Anmeldeinformationen, die in Active Directory Domain Services (DS) gespeichert sind, greift auf eine Webanwendung zu, die in Ihrem Azure VNet gehostet wird.</span><span class="sxs-lookup"><span data-stu-id="5410b-111">An external, registered user with credentials stored inside Active Directory Domain Services (DS) accesses a web application hosted inside your Azure VNet.</span></span>
- <span data-ttu-id="5410b-112">Ein Benutzer, der über ein autorisiertes Gerät eine Verbindung mit Ihrem virtuellen Netzwerk (VNet) hergestellt hat, führt eine Webanwendung aus, die in Ihrem Azure VNet gehostet wird.</span><span class="sxs-lookup"><span data-stu-id="5410b-112">A user connected to your VNet using an authorized device executes a web application hosted inside your Azure VNet.</span></span>

<span data-ttu-id="5410b-113">Typische Einsatzmöglichkeiten für diese Architektur sind:</span><span class="sxs-lookup"><span data-stu-id="5410b-113">Typical uses for this architecture include:</span></span>

- <span data-ttu-id="5410b-114">Hybridanwendungen, in denen Workloads teilweise lokal und teilweise in Azure ausgeführt werden</span><span class="sxs-lookup"><span data-stu-id="5410b-114">Hybrid applications where workloads run partly on-premises and partly in Azure.</span></span>
- <span data-ttu-id="5410b-115">Lösungen, in denen Verbundautorisierung verwendet wird, um Webanwendungen für Partnerorganisationen verfügbar zu machen</span><span class="sxs-lookup"><span data-stu-id="5410b-115">Solutions that use federated authorization to expose web applications to partner organizations.</span></span>
- <span data-ttu-id="5410b-116">Systeme, die Zugriff über Webbrowser ermöglichen, die außerhalb der Firewall der Organisation ausgeführt werden</span><span class="sxs-lookup"><span data-stu-id="5410b-116">Systems that support access from web browsers running outside of the organizational firewall.</span></span>
- <span data-ttu-id="5410b-117">Systeme, die Benutzern Zugriff auf Webanwendungen ermöglichen, indem sie eine Verbindung von einem autorisierten externen Gerät herstellen, etwa ein Remotecomputer, ein Notebook oder ein anderes mobiles Gerät</span><span class="sxs-lookup"><span data-stu-id="5410b-117">Systems that enable users to access to web applications by connecting from authorized external devices such as remote computers, notebooks, and other mobile devices.</span></span>

<span data-ttu-id="5410b-118">In dieser Referenzarchitektur liegt der Schwerpunkt auf *passivem Verbund*, in dem die Verbundserver entscheiden, wie und wann ein Benutzer authentifiziert werden muss.</span><span class="sxs-lookup"><span data-stu-id="5410b-118">This reference architecture focuses on *passive federation*, in which the federation servers decide how and when to authenticate a user.</span></span> <span data-ttu-id="5410b-119">Der Benutzer gibt Anmeldeinformationen an, wenn die Anwendung gestartet wird.</span><span class="sxs-lookup"><span data-stu-id="5410b-119">The user provides sign in information when the application is started.</span></span> <span data-ttu-id="5410b-120">Dieser Mechanismus wird am häufigsten von Webbrowsern verwendet und beinhaltet ein Protokoll, das den Browser zu einer Website umleitet, auf der sich der Benutzer authentifiziert.</span><span class="sxs-lookup"><span data-stu-id="5410b-120">This mechanism is most commonly used by web browsers and involves a protocol that redirects the browser to a site where the user authenticates.</span></span> <span data-ttu-id="5410b-121">AD FS unterstützt auch *aktiven Verbund*, bei dem eine Anwendung die jeweiligen Anmeldeinformationen bereitstellt, ohne dass weiterer Benutzereingriff erforderlich ist. Dieses Szenario ist aber nicht Bestandteil dieser Architektur.</span><span class="sxs-lookup"><span data-stu-id="5410b-121">AD FS also supports *active federation*, where an application takes on responsibility for supplying credentials without further user interaction, but that scenario is outside the scope of this architecture.</span></span>

<span data-ttu-id="5410b-122">Weitere Überlegungen finden Sie unter [Auswählen einer Lösung für die Integration einer lokalen Active Directory-Instanz in Azure][considerations].</span><span class="sxs-lookup"><span data-stu-id="5410b-122">For additional considerations, see [Choose a solution for integrating on-premises Active Directory with Azure][considerations].</span></span>

## <a name="architecture"></a><span data-ttu-id="5410b-123">Architecture</span><span class="sxs-lookup"><span data-stu-id="5410b-123">Architecture</span></span>

<span data-ttu-id="5410b-124">Diese Architektur erweitert die Implementierung, die unter [Erweitern von Active Directory Domain Services (AD DS) auf Azure][extending-ad-to-azure] beschrieben ist.</span><span class="sxs-lookup"><span data-stu-id="5410b-124">This architecture extends the implementation described in [Extending AD DS to Azure][extending-ad-to-azure].</span></span> <span data-ttu-id="5410b-125">Sie enthält die folgenden Komponenten.</span><span class="sxs-lookup"><span data-stu-id="5410b-125">It contains the followign components.</span></span>

- <span data-ttu-id="5410b-126">**AD DS-Subnetz**.</span><span class="sxs-lookup"><span data-stu-id="5410b-126">**AD DS subnet**.</span></span> <span data-ttu-id="5410b-127">Der AD DS-Server befinden sich in ihrem eigenen Subnetz mit Regeln für Netzwerksicherheitsgruppen (NSG), die als Firewall fungieren.</span><span class="sxs-lookup"><span data-stu-id="5410b-127">The AD DS servers are contained in their own subnet with network security group (NSG) rules acting as a firewall.</span></span>

- <span data-ttu-id="5410b-128">**AD DS-Server**.</span><span class="sxs-lookup"><span data-stu-id="5410b-128">**AD DS servers**.</span></span> <span data-ttu-id="5410b-129">Domänencontroller, die als virtuelle Computer in Azure ausgeführt werden.</span><span class="sxs-lookup"><span data-stu-id="5410b-129">Domain controllers running as VMs in Azure.</span></span> <span data-ttu-id="5410b-130">Diese Server bieten Authentifizierung von lokalen Identitäten in der Domäne.</span><span class="sxs-lookup"><span data-stu-id="5410b-130">These servers provide authentication of local identities within the domain.</span></span>

- <span data-ttu-id="5410b-131">**AD FS-Subnetz**.</span><span class="sxs-lookup"><span data-stu-id="5410b-131">**AD FS subnet**.</span></span> <span data-ttu-id="5410b-132">Die AD FS-Server befinden sich in ihrem eigenen Subnetz mit NSG-Regeln, die als Firewall fungieren.</span><span class="sxs-lookup"><span data-stu-id="5410b-132">The AD FS servers are located within their own subnet with NSG rules acting as a firewall.</span></span>

- <span data-ttu-id="5410b-133">**AD FS-Server**.</span><span class="sxs-lookup"><span data-stu-id="5410b-133">**AD FS servers**.</span></span> <span data-ttu-id="5410b-134">Die AD FS-Server bieten Verbundautorisierung und -authentifizierung.</span><span class="sxs-lookup"><span data-stu-id="5410b-134">The AD FS servers provide federated authorization and authentication.</span></span> <span data-ttu-id="5410b-135">In dieser Architektur führen sie die folgenden Aufgaben aus:</span><span class="sxs-lookup"><span data-stu-id="5410b-135">In this architecture, they perform the following tasks:</span></span>

  - <span data-ttu-id="5410b-136">Empfangen von Sicherheitstoken, die Ansprüche enthalten, die ein Partnerverbundserver im Auftrag eines Partnerbenutzers stellt.</span><span class="sxs-lookup"><span data-stu-id="5410b-136">Receiving security tokens containing claims made by a partner federation server on behalf of a partner user.</span></span> <span data-ttu-id="5410b-137">AD FS überprüft, ob die Token gültig sind, bevor die Ansprüche an die in Azure ausgeführte Webanwendung übergeben werden, um die Anforderungen zu autorisieren.</span><span class="sxs-lookup"><span data-stu-id="5410b-137">AD FS verifies that the tokens are valid before passing the claims to the web application running in Azure to authorize requests.</span></span>

    <span data-ttu-id="5410b-138">Die in Azure ausgeführte Anwendung ist die *vertrauende Seite*.</span><span class="sxs-lookup"><span data-stu-id="5410b-138">The application running in Azure is the *relying party*.</span></span> <span data-ttu-id="5410b-139">Die Partnerverbundserver muss Ansprüche ausgeben, die von der Webanwendung verstanden werden.</span><span class="sxs-lookup"><span data-stu-id="5410b-139">The partner federation server must issue claims that are understood by the web application.</span></span> <span data-ttu-id="5410b-140">Die Partnerverbundserver werden als *Kontopartner* bezeichnet, weil sie Anforderungen im Auftrag von authentifizierten Konten in der Partnerorganisation übergeben.</span><span class="sxs-lookup"><span data-stu-id="5410b-140">The partner federation servers are referred to as *account partners*, because they submit access requests on behalf of authenticated accounts in the partner organization.</span></span> <span data-ttu-id="5410b-141">Die AD FS-Server werden als *Ressourcenpartner* bezeichnet, weil sie Zugriff auf Ressourcen (die Webanwendung) ermöglichen.</span><span class="sxs-lookup"><span data-stu-id="5410b-141">The AD FS servers are called *resource partners* because they provide access to resources (the web application).</span></span>

  - <span data-ttu-id="5410b-142">Authentifizieren und Autorisieren von eingehenden Anforderungen von externen Benutzern, die einen Webbrowser oder ein Gerät ausführen, der oder das Zugriff auf Webanwendungen benötigt, durch Verwenden von AD DS und dem [Active Directory-Geräteregistrierungsdienst][ADDRS].</span><span class="sxs-lookup"><span data-stu-id="5410b-142">Authenticating and authorizing incoming requests from external users running a web browser or device that needs access to web applications, by using AD DS and the [Active Directory Device Registration Service][ADDRS].</span></span>

  <span data-ttu-id="5410b-143">Die AD FS-Server sind als eine Farm konfiguriert, auf die über einen Azure-Lastenausgleich zugegriffen wird.</span><span class="sxs-lookup"><span data-stu-id="5410b-143">The AD FS servers are configured as a farm accessed through an Azure load balancer.</span></span> <span data-ttu-id="5410b-144">Diese Implementierung verbessert die Verfügbarkeit und die Skalierbarkeit.</span><span class="sxs-lookup"><span data-stu-id="5410b-144">This implementation improves availability and scalability.</span></span> <span data-ttu-id="5410b-145">Die AD FS-Server werden nicht direkt im Internet verfügbar gemacht.</span><span class="sxs-lookup"><span data-stu-id="5410b-145">The AD FS servers are not exposed directly to the Internet.</span></span> <span data-ttu-id="5410b-146">Der gesamte Internet-Datenverkehr wird über AD FS-Webanwendungs-Proxyserver und eine DMZ (auch als Umkreisnetzwerk bezeichnet) gefiltert.</span><span class="sxs-lookup"><span data-stu-id="5410b-146">All Internet traffic is filtered through AD FS web application proxy servers and a DMZ (also referred to as a perimeter network).</span></span>

  <span data-ttu-id="5410b-147">Weitere Informationen zur Funktionsweise von AD FS finden Sie unter [Active Directory-Verbunddienste: Übersicht][active-directory-federation-services-overview].</span><span class="sxs-lookup"><span data-stu-id="5410b-147">For more information about how AD FS works, see [Active Directory Federation Services Overview][active-directory-federation-services-overview].</span></span> <span data-ttu-id="5410b-148">Darüber hinaus enthält der Artikel [AD FS-Bereitstellung in Azure][adfs-intro] eine ausführliche schrittweise Einführung in die Implementierung.</span><span class="sxs-lookup"><span data-stu-id="5410b-148">Also, the article [AD FS deployment in Azure][adfs-intro] contains a detailed step-by-step introduction to implementation.</span></span>

- <span data-ttu-id="5410b-149">**AD FS-Proxysubnetz**.</span><span class="sxs-lookup"><span data-stu-id="5410b-149">**AD FS proxy subnet**.</span></span> <span data-ttu-id="5410b-150">Die AD FS-Proxyserver können in ihrem eigenen Subnetz enthalten sein, in dem NSG-Regeln Schutz bieten.</span><span class="sxs-lookup"><span data-stu-id="5410b-150">The AD FS proxy servers can be contained within their own subnet, with NSG rules providing protection.</span></span> <span data-ttu-id="5410b-151">Die Server in diesem Subnetz werden dem Internet über eine Reihe von virtuellen Netzwerkgeräten verfügbar gemacht, die eine Firewall zwischen Ihrem virtuellen Azure-Netzwerk und dem Internet bereitstellen.</span><span class="sxs-lookup"><span data-stu-id="5410b-151">The servers in this subnet are exposed to the Internet through a set of network virtual appliances that provide a firewall between your Azure virtual network and the Internet.</span></span>

- <span data-ttu-id="5410b-152">**AD FS-Webanwendungs-Proxyserver (WAP-Server)**.</span><span class="sxs-lookup"><span data-stu-id="5410b-152">**AD FS web application proxy (WAP) servers**.</span></span> <span data-ttu-id="5410b-153">Diese virtuellen Computer fungieren als AD FS-Server für eingehende Anforderungen von Partnerorganisationen und externen Geräten.</span><span class="sxs-lookup"><span data-stu-id="5410b-153">These VMs act as AD FS servers for incoming requests from partner organizations and external devices.</span></span> <span data-ttu-id="5410b-154">Die WAP-Server fungieren als Filter, der die AD FS-Server gegen direkten Zugriff aus dem Internet abschirmt.</span><span class="sxs-lookup"><span data-stu-id="5410b-154">The WAP servers act as a filter, shielding the AD FS servers from direct access from the Internet.</span></span> <span data-ttu-id="5410b-155">So wie bei den AD FS-Servern bietet Ihnen Bereitstellen der WAP-Server in einer Farm mit Lastenausgleich größere Verfügbarkeit und Skalierbarkeit, als dies bei Bereitstellung einer Sammlung von eigenständigen Servern der Fall ist.</span><span class="sxs-lookup"><span data-stu-id="5410b-155">As with the AD FS servers, deploying the WAP servers in a farm with load balancing gives you greater availability and scalability than deploying a collection of stand-alone servers.</span></span>

  > [!NOTE]
  > <span data-ttu-id="5410b-156">Ausführliche Informationen zu, Installieren von WAP-Servern finden Sie unter [Installieren und Konfigurieren des Webanwendungsproxy-Servers][install_and_configure_the_web_application_proxy_server].</span><span class="sxs-lookup"><span data-stu-id="5410b-156">For detailed information about installing WAP servers, see [Install and Configure the Web Application Proxy Server][install_and_configure_the_web_application_proxy_server]</span></span>
  >

- <span data-ttu-id="5410b-157">**Partnerorganisation**.</span><span class="sxs-lookup"><span data-stu-id="5410b-157">**Partner organization**.</span></span> <span data-ttu-id="5410b-158">Eine Partnerorganisation, die eine Webanwendung ausführt, die Zugriff auf eine Webanwendung anfordert, die in Azure ausgeführt wird.</span><span class="sxs-lookup"><span data-stu-id="5410b-158">A partner organization running a web application that requests access to a web application running in Azure.</span></span> <span data-ttu-id="5410b-159">Der Verbundserver in der Partnerorganisation authentifiziert Anforderungen lokal und sendet Sicherheitstoken, die Ansprüche enthalten, an AD FS, der in Azure ausgeführt wird.</span><span class="sxs-lookup"><span data-stu-id="5410b-159">The federation server at the partner organization authenticates requests locally, and submits security tokens containing claims to AD FS running in Azure.</span></span> <span data-ttu-id="5410b-160">AD FS in Azure überprüft die Sicherheitstoken und kann, wenn die Token gültig sind, die Ansprüche an die in Azure ausgeführte Webanwendung übergeben, um sie zu autorisieren.</span><span class="sxs-lookup"><span data-stu-id="5410b-160">AD FS in Azure validates the security tokens, and if valid can pass the claims to the web application running in Azure to authorize them.</span></span>

  > [!NOTE]
  > <span data-ttu-id="5410b-161">Sie können auch mit einem Azure-Gateway einen VPN-Tunnel konfigurieren, um für vertrauenswürdige Partner direkten Zugriff auf AD FS zu ermöglichen.</span><span class="sxs-lookup"><span data-stu-id="5410b-161">You can also configure a VPN tunnel using Azure gateway to provide direct access to AD FS for trusted partners.</span></span> <span data-ttu-id="5410b-162">Anforderungen, die von diesen Partnern eingehen, durchlaufen nicht den WAP-Server.</span><span class="sxs-lookup"><span data-stu-id="5410b-162">Requests received from these partners do not pass through the WAP servers.</span></span>
  >

## <a name="recommendations"></a><span data-ttu-id="5410b-163">Empfehlungen</span><span class="sxs-lookup"><span data-stu-id="5410b-163">Recommendations</span></span>

<span data-ttu-id="5410b-164">Die folgenden Empfehlungen gelten für die meisten Szenarios.</span><span class="sxs-lookup"><span data-stu-id="5410b-164">The following recommendations apply for most scenarios.</span></span> <span data-ttu-id="5410b-165">Sofern Sie keine besonderen Anforderungen haben, die Vorrang haben, sollten Sie diese Empfehlungen befolgen.</span><span class="sxs-lookup"><span data-stu-id="5410b-165">Follow these recommendations unless you have a specific requirement that overrides them.</span></span>

### <a name="networking-recommendations"></a><span data-ttu-id="5410b-166">Netzwerkempfehlungen</span><span class="sxs-lookup"><span data-stu-id="5410b-166">Networking recommendations</span></span>

<span data-ttu-id="5410b-167">Konfigurieren Sie die Netzwerkschnittstelle für jeden der virtuellen Computer, die AD FS und WAP-Server hosten, mit statischen privaten IP-Adressen.</span><span class="sxs-lookup"><span data-stu-id="5410b-167">Configure the network interface for each of the VMs hosting AD FS and WAP servers with static private IP addresses.</span></span>

<span data-ttu-id="5410b-168">Geben Sie den virtuellen Computern mit AD FS keine öffentlichen IP-Adressen.</span><span class="sxs-lookup"><span data-stu-id="5410b-168">Do not give the AD FS VMs public IP addresses.</span></span> <span data-ttu-id="5410b-169">Weitere Informationen hierzu finden Sie im Abschnitt [Sicherheitshinweise](#security-considerations).</span><span class="sxs-lookup"><span data-stu-id="5410b-169">For more information, see the [Security considerations](#security-considerations) section.</span></span>

<span data-ttu-id="5410b-170">Legen Sie die IP-Adresse des bevorzugten und die des sekundären DNS-Servers (Domain Name System) für die Netzwerkschnittstellen für jeden virtuellen AD FS- und WAP-Computer so fest, dass sie auf die virtuellen Active Directory Domain Services-Computer verweisen.</span><span class="sxs-lookup"><span data-stu-id="5410b-170">Set the IP address of the preferred and secondary domain name service (DNS) servers for the network interfaces for each AD FS and WAP VM to reference the Active Directory DS VMs.</span></span> <span data-ttu-id="5410b-171">DNS sollte auf den virtuellen Active Directory Domain Services-Computern ausgeführt werden.</span><span class="sxs-lookup"><span data-stu-id="5410b-171">The Active Directory DS VMs should be running DNS.</span></span> <span data-ttu-id="5410b-172">Dieser Schritt ist notwendig, damit jeder virtuelle Computer der Domäne beitreten kann.</span><span class="sxs-lookup"><span data-stu-id="5410b-172">This step is necessary to enable each VM to join the domain.</span></span>

### <a name="ad-fs-installation"></a><span data-ttu-id="5410b-173">AD FS-Installation</span><span class="sxs-lookup"><span data-stu-id="5410b-173">AD FS installation</span></span>

<span data-ttu-id="5410b-174">Der Artikel [Bereitstellen einer Verbundserverfarm][Deploying_a_federation_server_farm] enthält ausführliche Anweisungen zum Installieren und Konfigurieren von AD FS.</span><span class="sxs-lookup"><span data-stu-id="5410b-174">The article [Deploying a Federation Server Farm][Deploying_a_federation_server_farm] provides detailed instructions for installing and configuring AD FS.</span></span> <span data-ttu-id="5410b-175">Führen Sie die folgenden Aufgaben aus, bevor Sie den ersten AD FS-Server in der Farm konfigurieren:</span><span class="sxs-lookup"><span data-stu-id="5410b-175">Perform the following tasks before configuring the first AD FS server in the farm:</span></span>

1. <span data-ttu-id="5410b-176">Rufen Sie ein öffentlich vertrauenswürdiges Zertifikat ab, mit dem eine Serverauthentifizierung vorgenommen wird.</span><span class="sxs-lookup"><span data-stu-id="5410b-176">Obtain a publicly trusted certificate for performing server authentication.</span></span> <span data-ttu-id="5410b-177">Der *Antragstellername* muss den Namen enthalten, den Clients dazu verwenden, auf den Verbunddienst zuzugreifen.</span><span class="sxs-lookup"><span data-stu-id="5410b-177">The *subject name* must contain the name clients use to access the federation service.</span></span> <span data-ttu-id="5410b-178">Dies kann der DNS-Name sein, der für den Lastenausgleich registriert ist, etwa *adfs.contoso.com* (aus Sicherheitsgründen sollten Sie keine Platzhalterzeichen, z.B. \**. contoso.com*, verwenden).</span><span class="sxs-lookup"><span data-stu-id="5410b-178">This can be the DNS name registered for the load balancer, for example, *adfs.contoso.com* (avoid using wildcard names such as \**.contoso.com*, for security reasons).</span></span> <span data-ttu-id="5410b-179">Verwenden Sie auf allen virtuellen Computern, die als AD FS-Server fungieren, dasselbe Zertifikat.</span><span class="sxs-lookup"><span data-stu-id="5410b-179">Use the same certificate on all AD FS server VMs.</span></span> <span data-ttu-id="5410b-180">Sie können ein Zertifikat von einer vertrauenswürdigen Zertifizierungsstelle erwerben, wird in Ihrer Organisation aber Active Directory-Zertifikatdienste verwendet, können Sie Ihr eigenes Zertifikat erstellen.</span><span class="sxs-lookup"><span data-stu-id="5410b-180">You can purchase a certificate from a trusted certification authority, but if your organization uses Active Directory Certificate Services you can create your own.</span></span>

    <span data-ttu-id="5410b-181">*Alternativer Antragstellername* wird vom Geräteregistrierungsdienst (Device Registration Service, DRS) verwendet, um Zugriff von externen Geräten zu ermöglichen.</span><span class="sxs-lookup"><span data-stu-id="5410b-181">The *subject alternative name* is used by the device registration service (DRS) to enable access from external devices.</span></span> <span data-ttu-id="5410b-182">Dieser Name muss in der Form *enterpriseregistration.contoso.com* vorliegen.</span><span class="sxs-lookup"><span data-stu-id="5410b-182">This should be of the form *enterpriseregistration.contoso.com*.</span></span>

    <span data-ttu-id="5410b-183">Weitere Informationen hierzu finden Sie unter [Abrufen und Konfigurieren eines SSL-Zertifikats (Secure Sockets Layer) für AD FS][adfs_certificates].</span><span class="sxs-lookup"><span data-stu-id="5410b-183">For more information, see [Obtain and Configure a Secure Sockets Layer (SSL) Certificate for AD FS][adfs_certificates].</span></span>

2. <span data-ttu-id="5410b-184">Generieren Sie auf dem Domänencontroller einen neuen Stammschlüssel für den Schlüsselverteilungsdienst.</span><span class="sxs-lookup"><span data-stu-id="5410b-184">On the domain controller, generate a new root key for the Key Distribution Service.</span></span> <span data-ttu-id="5410b-185">Legen Sie die effektive Zeit auf die aktuelle Zeit abzüglich 10 Stunden fest (diese Konfiguration verringert die Verzögerung, die auftreten kann, wenn Schlüssel in der Domäne verteilt und synchronisiert werden).</span><span class="sxs-lookup"><span data-stu-id="5410b-185">Set the effective time to the current time minus 10 hours (this configuration reduces the delay that can occur in distributing and synchronizing keys across the domain).</span></span> <span data-ttu-id="5410b-186">Dieser Schritt ist erforderlich, damit das Gruppendienstkonto erstellt werden kann, mit dem der AD FS-Dienst ausgeführt wird.</span><span class="sxs-lookup"><span data-stu-id="5410b-186">This step is necessary to support creating the group service account that is used to run the AD FS service.</span></span> <span data-ttu-id="5410b-187">Im folgenden PowerShell-Befehl ist beispielhaft gezeigt, wie dies ausgeführt wird:</span><span class="sxs-lookup"><span data-stu-id="5410b-187">The following PowerShell command shows an example of how to do this:</span></span>

    ```powershell
    Add-KdsRootKey -EffectiveTime (Get-Date).AddHours(-10)
    ```

3. <span data-ttu-id="5410b-188">Fügen Sie jeden virtuellen Computer, der als AD FS-Server fungiert, zur Domäne hinzu.</span><span class="sxs-lookup"><span data-stu-id="5410b-188">Add each AD FS server VM to the domain.</span></span>

> [!NOTE]
> <span data-ttu-id="5410b-189">Damit AD FS installiert werden kann, müssen die virtuellen AD FS-Computer auf den Domänencontroller zugreifen können, auf dem die FSMO-Rolle (Flexible Single Master Operation) für den PDC-Emulator (Primary Domain Controller, primärer Domänencontroller) ausgeführt wird, und dieser Domänencontroller muss ausgeführt werden.</span><span class="sxs-lookup"><span data-stu-id="5410b-189">To install AD FS, the domain controller running the primary domain controller (PDC) emulator flexible single master operation (FSMO) role for the domain must be running and accessible from the AD FS VMs.</span></span> <span data-ttu-id="5410b-190"><<RBC: Is there a way to make this less repetitive?>></span><span class="sxs-lookup"><span data-stu-id="5410b-190"><<RBC: Is there a way to make this less repetitive?>></span></span>
>

### <a name="ad-fs-trust"></a><span data-ttu-id="5410b-191">AD FS-Vertrauensstellung</span><span class="sxs-lookup"><span data-stu-id="5410b-191">AD FS trust</span></span>

<span data-ttu-id="5410b-192">Richten Sie eine Verbundvertrauensstellung zwischen Ihrer AD FS-Installation und den Verbundservern jeder Partnerorganisation ein.</span><span class="sxs-lookup"><span data-stu-id="5410b-192">Establish federation trust between your AD FS installation, and the federation servers of any partner organizations.</span></span> <span data-ttu-id="5410b-193">Konfigurieren Sie jegliche erforderliche Ansprüchefilterung und -zuordnung.</span><span class="sxs-lookup"><span data-stu-id="5410b-193">Configure any claims filtering and mapping required.</span></span>

- <span data-ttu-id="5410b-194">DevOps-Mitarbeiter in jeder Partnerorganisation müssen eine Vertrauensstellung der vertrauenden Seite für die Webanwendungen hinzufügen, auf die über Ihre AD FS-Server zugegriffen werden kann.</span><span class="sxs-lookup"><span data-stu-id="5410b-194">DevOps staff at each partner organization must add a relying party trust for the web applications accessible through your AD FS servers.</span></span>
- <span data-ttu-id="5410b-195">DevOps-Mitarbeiter in Ihrer Organisation müssen eine Anspruchsanbieter-Vertrauensstellung konfigurieren, damit Ihre AD FS-Server den Ansprüchen vertrauen können, die von Partnerorganisationen bereitgestellt werden.</span><span class="sxs-lookup"><span data-stu-id="5410b-195">DevOps staff in your organization must configure claims-provider trust to enable your AD FS servers to trust the claims that partner organizations provide.</span></span>
- <span data-ttu-id="5410b-196">DevOps-Mitarbeiter in Ihrer Organisation müssen außerdem AD FS so konfigurieren, dass Ansprüche an die Webanwendungen Ihrer Organisation übergeben werden.</span><span class="sxs-lookup"><span data-stu-id="5410b-196">DevOps staff in your organization must also configure AD FS to pass claims on to your organization's web applications.</span></span>

<span data-ttu-id="5410b-197">Weitere Informationen hierzu finden Sie unter [Establishing Federation Trust][establishing-federation-trust].</span><span class="sxs-lookup"><span data-stu-id="5410b-197">For more information, see [Establishing Federation Trust][establishing-federation-trust].</span></span>

<span data-ttu-id="5410b-198">Veröffentlichen Sie die Webanwendungen Ihrer Organisation, und machen Sie diese für externe Partner verfügbar, indem Sie Präauthentifizierung durch die WAP-Server verwenden.</span><span class="sxs-lookup"><span data-stu-id="5410b-198">Publish your organization's web applications and make them available to external partners by using preauthentication through the WAP servers.</span></span> <span data-ttu-id="5410b-199">Weitere Informationen hierzu finden Sie unter [Veröffentlichen von Anwendungen mit AD FS-Vorauthentifizierung][publish_applications_using_AD_FS_preauthentication].</span><span class="sxs-lookup"><span data-stu-id="5410b-199">For more information, see [Publish Applications using AD FS Preauthentication][publish_applications_using_AD_FS_preauthentication]</span></span>

<span data-ttu-id="5410b-200">AD FS unterstützt Tokentransformation und -ergänzung.</span><span class="sxs-lookup"><span data-stu-id="5410b-200">AD FS supports token transformation and augmentation.</span></span> <span data-ttu-id="5410b-201">Azure Active Directory bietet diese Funktionalität nicht.</span><span class="sxs-lookup"><span data-stu-id="5410b-201">Azure Active Directory does not provide this feature.</span></span> <span data-ttu-id="5410b-202">Wenn Sie die Vertrauensstellungen einrichten, können Sie mit AD FS:</span><span class="sxs-lookup"><span data-stu-id="5410b-202">With AD FS, when you set up the trust relationships, you can:</span></span>

- <span data-ttu-id="5410b-203">Anspruchstransformationen für Autorisierungsregeln konfigurieren.</span><span class="sxs-lookup"><span data-stu-id="5410b-203">Configure claim transformations for authorization rules.</span></span> <span data-ttu-id="5410b-204">Beispielsweise können Sie Gruppensicherheit aus einer Darstellung, die von einer Nicht-Microsoft-Partnerorganisation verwendet wird, zu einer anderen Darstellung zuordnen, die von Active Directory DS in Ihrer Organisation autorisiert werden kann.</span><span class="sxs-lookup"><span data-stu-id="5410b-204">For example, you can map group security from a representation used by a non-Microsoft partner organization to something that Active Directory DS can authorize in your organization.</span></span>
- <span data-ttu-id="5410b-205">Ansprüche aus einem Format in ein anderes transformieren.</span><span class="sxs-lookup"><span data-stu-id="5410b-205">Transform claims from one format to another.</span></span> <span data-ttu-id="5410b-206">Beispielsweise können Sie von SAML 2.0 zu SAML 1.1 zuordnen, wenn Ihre Anwendung nur SAML 1.1-Ansprüche unterstützt.</span><span class="sxs-lookup"><span data-stu-id="5410b-206">For example, you can map from SAML 2.0 to SAML 1.1 if your application only supports SAML 1.1 claims.</span></span>

### <a name="ad-fs-monitoring"></a><span data-ttu-id="5410b-207">AD FS-Überwachung</span><span class="sxs-lookup"><span data-stu-id="5410b-207">AD FS monitoring</span></span>

<span data-ttu-id="5410b-208">Das [System Center-Management Pack für Active Directory Federation Services 2012 R2 ][oms-adfs-pack] bietet sowohl proaktive als auch reaktive Überwachung Ihrer AD FS-Bereitstellung für den Verbundserver.</span><span class="sxs-lookup"><span data-stu-id="5410b-208">The [Microsoft System Center Management Pack for Active Directory Federation Services 2012 R2][oms-adfs-pack] provides both proactive and reactive monitoring of your AD FS deployment for the federation server.</span></span> <span data-ttu-id="5410b-209">Dieses Management Pack überwacht Folgendes:</span><span class="sxs-lookup"><span data-stu-id="5410b-209">This management pack monitors:</span></span>

- <span data-ttu-id="5410b-210">Ereignisse, die der AD FS-Dienst in seinen Ereignisprotokollen speichert</span><span class="sxs-lookup"><span data-stu-id="5410b-210">Events that the AD FS service records in its event logs.</span></span>
- <span data-ttu-id="5410b-211">Die Leistungsdaten, die von den AD FS-Leistungsindikatoren gesammelt werden</span><span class="sxs-lookup"><span data-stu-id="5410b-211">The performance data that the AD FS performance counters collect.</span></span>
- <span data-ttu-id="5410b-212">Die Gesamtintegrität des AD FS-Systems und der Webanwendungen (vertrauende Seiten); stellt außerdem Benachrichtigung für schwerwiegende Probleme und Warnungen bereit</span><span class="sxs-lookup"><span data-stu-id="5410b-212">The overall health of the AD FS system and web applications (relying parties), and provides alerts for critical issues and warnings.</span></span>

## <a name="scalability-considerations"></a><span data-ttu-id="5410b-213">Überlegungen zur Skalierbarkeit</span><span class="sxs-lookup"><span data-stu-id="5410b-213">Scalability considerations</span></span>

<span data-ttu-id="5410b-214">Die folgenden Aspekte, die aus dem Artikel [Planen der AD FS-Bereitstellung][plan-your-adfs-deployment] zusammengefasst sind, bieten einen Ausgangspunkt zum Anpassen der Größe von AD FS-Farmen:</span><span class="sxs-lookup"><span data-stu-id="5410b-214">The following considerations, summarized from the article [Plan your AD FS deployment][plan-your-adfs-deployment], give a starting point for sizing AD FS farms:</span></span>

- <span data-ttu-id="5410b-215">Wenn Sie weniger als 1000 Benutzer haben, sollten Sie keine dedizierten Server erstellen, sondern stattdessen AD FS auf jedem der Active Directory DS-Server in der Cloud installieren.</span><span class="sxs-lookup"><span data-stu-id="5410b-215">If you have fewer than 1000 users, do not create dedicated servers, but instead install AD FS on each of the Active Directory DS servers in the cloud.</span></span> <span data-ttu-id="5410b-216">Verwenden Sie dazu mindestens zwei Active Directory DS-Server, damit Verfügbarkeit sichergestellt ist.</span><span class="sxs-lookup"><span data-stu-id="5410b-216">Make sure that you have at least two Active Directory DS servers to maintain availability.</span></span> <span data-ttu-id="5410b-217">Erstellen Sie einen einzelnen WAP-Server.</span><span class="sxs-lookup"><span data-stu-id="5410b-217">Create a single WAP server.</span></span>
- <span data-ttu-id="5410b-218">Wenn Sie zwischen 1000 und 15000 Benutzer haben, erstellen Sie zwei dedizierte AD FS-Server und zwei dedizierte WAP-Server.</span><span class="sxs-lookup"><span data-stu-id="5410b-218">If you have between 1000 and 15000 users, create two dedicated AD FS servers and two dedicated WAP servers.</span></span>
- <span data-ttu-id="5410b-219">Wenn Sie zwischen 15000 und 60000 Benutzer haben, erstellen Sie drei bis fünf dedizierte AD FS-Server und mindestens zwei dedizierte WAP-Server.</span><span class="sxs-lookup"><span data-stu-id="5410b-219">If you have between 15000 and 60000 users, create between three and five dedicated AD FS servers and at least two dedicated WAP servers.</span></span>

<span data-ttu-id="5410b-220">Für diese Angaben wird angenommen, dass Sie virtuelle Computer in einer Größe mit doppeltem Vierkern (Standard_D4_v2 oder besser) in Azure verwenden.</span><span class="sxs-lookup"><span data-stu-id="5410b-220">These considerations assume that you are using dual quad-core VM (Standard D4_v2, or better) sizes in Azure.</span></span>

<span data-ttu-id="5410b-221">Wenn Sie die interne Windows-Datenbank verwenden, um die AD FS-Konfigurationsdaten zu speichern, sind Sie auf acht AD FS-Server in der Farm beschränkt.</span><span class="sxs-lookup"><span data-stu-id="5410b-221">If you are using the Windows Internal Database to store AD FS configuration data, you are limited to eight AD FS servers in the farm.</span></span> <span data-ttu-id="5410b-222">Wenn sich davon ausgehen, dass Sie zukünftig mehr benötigen, sollten Sie SQL Server verwenden.</span><span class="sxs-lookup"><span data-stu-id="5410b-222">If you anticipate that you will need more in the future, use SQL Server.</span></span> <span data-ttu-id="5410b-223">Weitere Informationen hierzu finden Sie unter [Die Rolle der AD FS-Konfigurationsdatenbank][adfs-configuration-database].</span><span class="sxs-lookup"><span data-stu-id="5410b-223">For more information, see [The Role of the AD FS Configuration Database][adfs-configuration-database].</span></span>

## <a name="availability-considerations"></a><span data-ttu-id="5410b-224">Überlegungen zur Verfügbarkeit</span><span class="sxs-lookup"><span data-stu-id="5410b-224">Availability considerations</span></span>

<span data-ttu-id="5410b-225">Erstellen Sie eine AD FS-Farm mit mindestens zwei Servern, um die Verfügbarkeit des Diensts zu erhöhen.</span><span class="sxs-lookup"><span data-stu-id="5410b-225">Create an AD FS farm with at least two servers to increase availability of the service.</span></span> <span data-ttu-id="5410b-226">Verwenden Sie verschiedene Speicherkonten für jeden virtuellen AD FS-Computer in der Farm.</span><span class="sxs-lookup"><span data-stu-id="5410b-226">Use different storage accounts for each AD FS VM in the farm.</span></span> <span data-ttu-id="5410b-227">Mit dieser Vorgehensweise lässt sich sicherstellen, dass ein Fehler in einem Speicherkonto nicht dazu führt, dass die gesamte Farm unzugänglich wird.</span><span class="sxs-lookup"><span data-stu-id="5410b-227">This approach helps to ensure that a failure in a single storage account does not make the entire farm inaccessible.</span></span>

<span data-ttu-id="5410b-228">Erstellen Sie getrennte Azure-Verfügbarkeitsgruppen für die virtuellen AD FS- und WAP-Computer.</span><span class="sxs-lookup"><span data-stu-id="5410b-228">Create separate Azure availability sets for the AD FS and WAP VMs.</span></span> <span data-ttu-id="5410b-229">Stellen Sie sicher, dass in jeder Gruppe mindestens zwei virtuelle Computer vorhanden sind.</span><span class="sxs-lookup"><span data-stu-id="5410b-229">Ensure that there are at least two VMs in each set.</span></span> <span data-ttu-id="5410b-230">Jede Verfügbarkeitsgruppe muss mindestens zwei Upgdatedomänen und zwei Fehlerdomänen haben.</span><span class="sxs-lookup"><span data-stu-id="5410b-230">Each availability set must have at least two update domains and two fault domains.</span></span>

<span data-ttu-id="5410b-231">Konfigurieren Sie die Lastenausgleiche für die virtuellen AD FS- und WAP-Computer wie folgt:</span><span class="sxs-lookup"><span data-stu-id="5410b-231">Configure the load balancers for the AD FS VMs and WAP VMs as follows:</span></span>

- <span data-ttu-id="5410b-232">Verwenden Sie einen Azure-Lastenausgleich, um externen Zugriff auf die virtuellen WAP-Computer zu bieten, und einen internen Lastenausgleich, um die Last auf die AD FS-Server in der Farm zu verteilen.</span><span class="sxs-lookup"><span data-stu-id="5410b-232">Use an Azure load balancer to provide external access to the WAP VMs, and an internal load balancer to distribute the load across the AD FS servers in the farm.</span></span>
- <span data-ttu-id="5410b-233">Übergeben Sie an die AD FS/WAP-Server nur Datenverkehr, der am Port 443 (HTTPS) ankommt.</span><span class="sxs-lookup"><span data-stu-id="5410b-233">Only pass traffic appearing on port 443 (HTTPS) to the AD FS/WAP servers.</span></span>
- <span data-ttu-id="5410b-234">Geben Sie dem Lastenausgleich eine statische IP-Adresse.</span><span class="sxs-lookup"><span data-stu-id="5410b-234">Give the load balancer a static IP address.</span></span>
- <span data-ttu-id="5410b-235">Erstellen Sie einen Integritätstest, indem Sie HTTP für `/adfs/probe` verwenden.</span><span class="sxs-lookup"><span data-stu-id="5410b-235">Create a health probe using HTTP against `/adfs/probe`.</span></span> <span data-ttu-id="5410b-236">Weitere Informationen finden Sie unter [Hardware Load Balancer Health Checks and Web Application Proxy/AD FS 2012 R2](https://blogs.technet.microsoft.com/applicationproxyblog/2014/10/17/hardware-load-balancer-health-checks-and-web-application-proxy-ad-fs-2012-r2/) (Hardware-Load Balancer-Integritätsprüfungen und Webanwendungsproxy/AD FS 2012 R2).</span><span class="sxs-lookup"><span data-stu-id="5410b-236">For more information, see [Hardware Load Balancer Health Checks and Web Application Proxy / AD FS 2012 R2](https://blogs.technet.microsoft.com/applicationproxyblog/2014/10/17/hardware-load-balancer-health-checks-and-web-application-proxy-ad-fs-2012-r2/).</span></span>

  > [!NOTE]
  > <span data-ttu-id="5410b-237">AD FS-Server verwenden das SNI-Protokoll (Server Name Indication), daher schlägt ein Versuch, den Test über einen HTTPS-Endpunkt aus dem Lastenausgleich auszuführen, fehl.</span><span class="sxs-lookup"><span data-stu-id="5410b-237">AD FS servers use the Server Name Indication (SNI) protocol, so attempting to probe using an HTTPS endpoint from the load balancer fails.</span></span>
  >

- <span data-ttu-id="5410b-238">Fügen Sie einen DNS-*A*-Datensatz zu der Domäne für den AD FS-Lastenausgleich hinzu.</span><span class="sxs-lookup"><span data-stu-id="5410b-238">Add a DNS *A* record to the domain for the AD FS load balancer.</span></span> <span data-ttu-id="5410b-239">Geben Sie die IP-Adresse des Lastenausgleichs an, und geben sie ihm einen Namen in der Domäne (z.B. „adfs.contoso.com“).</span><span class="sxs-lookup"><span data-stu-id="5410b-239">Specify the IP address of the load balancer, and give it a name in the domain (such as adfs.contoso.com).</span></span> <span data-ttu-id="5410b-240">Dies ist der Name, den Clients und WAP-Server verwenden, um auf die AD FS-Serverfarm zuzugreifen.</span><span class="sxs-lookup"><span data-stu-id="5410b-240">This is the name clients and the WAP servers use to access the AD FS server farm.</span></span>

<span data-ttu-id="5410b-241">Sie können entweder SQL Server oder die interne Windows-Datenbank verwenden, um die AD FS-Konfigurationsinformationen zu speichern.</span><span class="sxs-lookup"><span data-stu-id="5410b-241">You can use either SQL Server or the Windows Internal Database to hold AD FS configuration information.</span></span> <span data-ttu-id="5410b-242">Die interne Windows-Datenbank bietet grundlegende Redundanz.</span><span class="sxs-lookup"><span data-stu-id="5410b-242">The Windows Internal Database provides basic redundancy.</span></span> <span data-ttu-id="5410b-243">Änderungen werden direkt nur in eine der AD FS-Datenbanken im AD FS-Cluster geschrieben, während die anderen Server Pullreplikation verwenden, um ihre Datenbanken auf dem neuesten Stand zu halten.</span><span class="sxs-lookup"><span data-stu-id="5410b-243">Changes are written directly to only one of the AD FS databases in the AD FS cluster, while the other servers use pull replication to keep their databases up to date.</span></span> <span data-ttu-id="5410b-244">Wird SQL Server verwendet, können vollständige Datenbankredundanz und hohe Verfügbarkeit bereitgestellt werden, indem Failoverclustering oder Spiegelung verwendet wird.</span><span class="sxs-lookup"><span data-stu-id="5410b-244">Using SQL Server can provide full database redundancy and high availability using failover clustering or mirroring.</span></span>

## <a name="manageability-considerations"></a><span data-ttu-id="5410b-245">Überlegungen zur Verwaltbarkeit</span><span class="sxs-lookup"><span data-stu-id="5410b-245">Manageability considerations</span></span>

<span data-ttu-id="5410b-246">DevOps-Mitarbeiter sollten darauf vorbereitet sein, die folgenden Aufgaben auszuführen:</span><span class="sxs-lookup"><span data-stu-id="5410b-246">DevOps staff should be prepared to perform the following tasks:</span></span>

- <span data-ttu-id="5410b-247">Verwalten der Verbundserver, einschließlich Verwalten der AD FS-Farm, Verwalten der Vertrauensrichtlinien auf den Verbundservern und Verwalten der Zertifikate, die von den Verbunddiensten verwendet werden</span><span class="sxs-lookup"><span data-stu-id="5410b-247">Managing the federation servers, including managing the AD FS farm, managing trust policy on the federation servers, and managing the certificates used by the federation services.</span></span>
- <span data-ttu-id="5410b-248">Verwalten der WAP-Server, einschließlich Verwalten der WAP-Farm und -Zertifikate</span><span class="sxs-lookup"><span data-stu-id="5410b-248">Managing the WAP servers including managing the WAP farm and certificates.</span></span>
- <span data-ttu-id="5410b-249">Verwalten von Webanwendungen, einschließlich Konfigurieren der vertrauenden Seiten, der Authentifizierungsmethoden und der Anspruchszuordnungen</span><span class="sxs-lookup"><span data-stu-id="5410b-249">Managing web applications including configuring relying parties, authentication methods, and claims mappings.</span></span>
- <span data-ttu-id="5410b-250">Sichern der AD FS-Komponenten</span><span class="sxs-lookup"><span data-stu-id="5410b-250">Backing up AD FS components.</span></span>

## <a name="security-considerations"></a><span data-ttu-id="5410b-251">Sicherheitshinweise</span><span class="sxs-lookup"><span data-stu-id="5410b-251">Security considerations</span></span>

<span data-ttu-id="5410b-252">Da für AD FS HTTPS verwendet wird, müssen Sie darauf achten, dass die NSG-Regeln für das Subnetz, in dem die virtuellen Computer auf Webebene enthalten sind, HTTPS-Anforderungen zulassen.</span><span class="sxs-lookup"><span data-stu-id="5410b-252">AD FS uses HTTPS, so make sure that the NSG rules for the subnet containing the web tier VMs permit HTTPS requests.</span></span> <span data-ttu-id="5410b-253">Diese Anforderungen können aus dem lokalen Netzwerk, aus den Subnetzen, die die Webebene, die Geschäftsebene, die Datenebene, die private DMZ oder die öffentliche DMZ enthalten, oder aus dem Subnetz stammen, das die AD FS-Server enthält.</span><span class="sxs-lookup"><span data-stu-id="5410b-253">These requests can originate from the on-premises network, the subnets containing the web tier, business tier, data tier, private DMZ, public DMZ, and the subnet containing the AD FS servers.</span></span>

<span data-ttu-id="5410b-254">Verhindern Sie, dass AD FS-Server direkt für das Internet verfügbar gemacht werden.</span><span class="sxs-lookup"><span data-stu-id="5410b-254">Prevent direct exposure of the AD FS servers to the Internet.</span></span> <span data-ttu-id="5410b-255">AD FS-Server sind zur Domäne gehörende Computer, die die vollständige Autorisierung haben, Sicherheitstoken zu gewähren.</span><span class="sxs-lookup"><span data-stu-id="5410b-255">AD FS servers are domain-joined computers that have full authorization to grant security tokens.</span></span> <span data-ttu-id="5410b-256">Wenn ein Server kompromittiert ist, kann ein böswilliger Benutzer Token für Vollzugriff auf alle Webanwendungen und alle Verbundserver ausgeben, die durch AD FS geschützt sind.</span><span class="sxs-lookup"><span data-stu-id="5410b-256">If a server is compromised, a malicious user can issue full access tokens to all web applications and to all federation servers that are protected by AD FS.</span></span> <span data-ttu-id="5410b-257">Muss Ihr System Anforderungen von externen Benutzern verarbeiten, die nicht über vertrauenswürdige Partnerwebsites zugreifen, sollten Sie diese Anforderungen mit WAP-Servern verarbeiten.</span><span class="sxs-lookup"><span data-stu-id="5410b-257">If your system must handle requests from external users not connecting from trusted partner sites, use WAP servers to handle these requests.</span></span> <span data-ttu-id="5410b-258">Weitere Informationen hierzu finden Sie unter [Platzieren eines Verbundserverproxys][where-to-place-an-fs-proxy].</span><span class="sxs-lookup"><span data-stu-id="5410b-258">For more information, see [Where to Place a Federation Server Proxy][where-to-place-an-fs-proxy].</span></span>

<span data-ttu-id="5410b-259">Platzieren Sie AD FS-Server und WAP-Server in getrennten Subnetzen mit jeweils eigener Firewall.</span><span class="sxs-lookup"><span data-stu-id="5410b-259">Place AD FS servers and WAP servers in separate subnets with their own firewalls.</span></span> <span data-ttu-id="5410b-260">Sie können NSG-Regeln verwenden, um Firewallregeln zu definieren.</span><span class="sxs-lookup"><span data-stu-id="5410b-260">You can use NSG rules to define firewall rules.</span></span> <span data-ttu-id="5410b-261">Alle Firewalls müssen Datenverkehr über den Port 443 (HTTPS) zulassen.</span><span class="sxs-lookup"><span data-stu-id="5410b-261">All firewalls should allow traffic on port 443 (HTTPS).</span></span>

<span data-ttu-id="5410b-262">Beschränken Sie direkten Anmeldezugriff auf die AD FS- und WAP-Server.</span><span class="sxs-lookup"><span data-stu-id="5410b-262">Restrict direct sign in access to the AD FS and WAP servers.</span></span> <span data-ttu-id="5410b-263">Nur DevOps-Mitarbeiter sollten Verbindungen herstellen können.</span><span class="sxs-lookup"><span data-stu-id="5410b-263">Only DevOps staff should be able to connect.</span></span> <span data-ttu-id="5410b-264">Binden Sie die WAP-Server nicht in die Domäne ein.</span><span class="sxs-lookup"><span data-stu-id="5410b-264">Do not join the WAP servers to the domain.</span></span>

<span data-ttu-id="5410b-265">Sie sollten erwägen, eine Reihe von virtuellen Netzwerkgeräten zu verwenden, die zu Überwachungszwecken ausführliche Informationen zu dem Datenverkehr protokollieren, der in Ihrem virtuellen Netzwerk empfangen wird.</span><span class="sxs-lookup"><span data-stu-id="5410b-265">Consider using a set of network virtual appliances that logs detailed information on traffic traversing the edge of your virtual network for auditing purposes.</span></span>

## <a name="deploy-the-solution"></a><span data-ttu-id="5410b-266">Bereitstellen der Lösung</span><span class="sxs-lookup"><span data-stu-id="5410b-266">Deploy the solution</span></span>

<span data-ttu-id="5410b-267">Eine Bereitstellung für diese Architektur ist auf [GitHub][github] verfügbar.</span><span class="sxs-lookup"><span data-stu-id="5410b-267">A deployment for this architecture is available on [GitHub][github].</span></span> <span data-ttu-id="5410b-268">Beachten Sie, dass die gesamte Bereitstellung bis zu zwei Stunden dauern kann. Dieser Vorgang umfasst die Erstellung des VPN-Gateways und die Ausführung der Skripts, mit denen Active Directory und AD FS konfiguriert werden.</span><span class="sxs-lookup"><span data-stu-id="5410b-268">Note that the entire deployment can take up to two hours, which includes creating the VPN gateway and running the scripts that configure Active Directory and AD FS.</span></span>

### <a name="prerequisites"></a><span data-ttu-id="5410b-269">Voraussetzungen</span><span class="sxs-lookup"><span data-stu-id="5410b-269">Prerequisites</span></span>

1. <span data-ttu-id="5410b-270">Führen Sie das Klonen, Forken oder Herunterladen der ZIP-Datei für das [GitHub-Repository](https://github.com/mspnp/identity-reference-architectures) durch.</span><span class="sxs-lookup"><span data-stu-id="5410b-270">Clone, fork, or download the zip file for the [GitHub repository](https://github.com/mspnp/identity-reference-architectures).</span></span>

1. <span data-ttu-id="5410b-271">Installieren Sie [Azure CLI 2.0](/cli/azure/install-azure-cli?view=azure-cli-latest).</span><span class="sxs-lookup"><span data-stu-id="5410b-271">Install [Azure CLI 2.0](/cli/azure/install-azure-cli?view=azure-cli-latest).</span></span>

1. <span data-ttu-id="5410b-272">Installieren Sie das npm-Paket mit den [Azure Bausteinen](https://github.com/mspnp/template-building-blocks/wiki/Install-Azure-Building-Blocks).</span><span class="sxs-lookup"><span data-stu-id="5410b-272">Install the [Azure building blocks](https://github.com/mspnp/template-building-blocks/wiki/Install-Azure-Building-Blocks) npm package.</span></span>

   ```bash
   npm install -g @mspnp/azure-building-blocks
   ```

1. <span data-ttu-id="5410b-273">Melden Sie sich über eine Eingabeaufforderung, eine Bash-Eingabeaufforderung oder die PowerShell-Eingabeaufforderung folgendermaßen bei Ihrem Azure-Konto an:</span><span class="sxs-lookup"><span data-stu-id="5410b-273">From a command prompt, bash prompt, or PowerShell prompt, sign into your Azure account as follows:</span></span>

   ```bash
   az login
   ```

### <a name="deploy-the-simulated-on-premises-datacenter"></a><span data-ttu-id="5410b-274">Bereitstellen des simulierten lokalen Rechenzentrums</span><span class="sxs-lookup"><span data-stu-id="5410b-274">Deploy the simulated on-premises datacenter</span></span>

1. <span data-ttu-id="5410b-275">Navigieren Sie zum Ordner `adfs` des GitHub-Repositorys.</span><span class="sxs-lookup"><span data-stu-id="5410b-275">Navigate to the `adfs` folder of the GitHub repository.</span></span>

1. <span data-ttu-id="5410b-276">Öffnen Sie die Datei `onprem.json` .</span><span class="sxs-lookup"><span data-stu-id="5410b-276">Open the `onprem.json` file.</span></span> <span data-ttu-id="5410b-277">Suchen Sie nach Instanzen von `adminPassword`, `Password` und `SafeModeAdminPassword`, und aktualisieren Sie die Kennwörter.</span><span class="sxs-lookup"><span data-stu-id="5410b-277">Search for instances of `adminPassword`, `Password`, and `SafeModeAdminPassword` and update the passwords.</span></span>

1. <span data-ttu-id="5410b-278">Führen Sie den folgenden Befehl aus, und warten Sie, bis die Bereitstellung abgeschlossen ist:</span><span class="sxs-lookup"><span data-stu-id="5410b-278">Run the following command and wait for the deployment to finish:</span></span>

    ```bash
    azbb -s <subscription_id> -g <resource group> -l <location> -p onprem.json --deploy
    ```

### <a name="deploy-the-azure-infrastructure"></a><span data-ttu-id="5410b-279">Bereitstellen der Azure-Infrastruktur</span><span class="sxs-lookup"><span data-stu-id="5410b-279">Deploy the Azure infrastructure</span></span>

1. <span data-ttu-id="5410b-280">Öffnen Sie die Datei `azure.json` .</span><span class="sxs-lookup"><span data-stu-id="5410b-280">Open the `azure.json` file.</span></span>  <span data-ttu-id="5410b-281">Suchen Sie nach Instanzen von `adminPassword` und `Password`, und fügen Sie Werte für die Kennwörter hinzu.</span><span class="sxs-lookup"><span data-stu-id="5410b-281">Search for instances of `adminPassword` and `Password` and add values for the passwords.</span></span>

1. <span data-ttu-id="5410b-282">Führen Sie den folgenden Befehl aus, und warten Sie, bis die Bereitstellung abgeschlossen ist:</span><span class="sxs-lookup"><span data-stu-id="5410b-282">Run the following command and wait for the deployment to finish:</span></span>

    ```bash
    azbb -s <subscription_id> -g <resource group> -l <location> -p azure.json --deploy
    ```

### <a name="set-up-the-ad-fs-farm"></a><span data-ttu-id="5410b-283">Einrichten der AD FS-Farm</span><span class="sxs-lookup"><span data-stu-id="5410b-283">Set up the AD FS farm</span></span>

1. <span data-ttu-id="5410b-284">Öffnen Sie die Datei `adfs-farm-first.json` .</span><span class="sxs-lookup"><span data-stu-id="5410b-284">Open the `adfs-farm-first.json` file.</span></span>  <span data-ttu-id="5410b-285">Suchen Sie nach `AdminPassword`, und ersetzen Sie das Standardkennwort.</span><span class="sxs-lookup"><span data-stu-id="5410b-285">Search for `AdminPassword` and replace the default password.</span></span>

1. <span data-ttu-id="5410b-286">Führen Sie den folgenden Befehl aus:</span><span class="sxs-lookup"><span data-stu-id="5410b-286">Run the following command:</span></span>

    ```bash
    azbb -s <subscription_id> -g <resource group> -l <location> -p adfs-farm-first.json --deploy
    ```

1. <span data-ttu-id="5410b-287">Öffnen Sie die Datei `adfs-farm-rest.json` .</span><span class="sxs-lookup"><span data-stu-id="5410b-287">Open the `adfs-farm-rest.json` file.</span></span>  <span data-ttu-id="5410b-288">Suchen Sie nach `AdminPassword`, und ersetzen Sie das Standardkennwort.</span><span class="sxs-lookup"><span data-stu-id="5410b-288">Search for `AdminPassword` and replace the default password.</span></span>

1. <span data-ttu-id="5410b-289">Führen Sie den folgenden Befehl aus, und warten Sie, bis die Bereitstellung abgeschlossen ist:</span><span class="sxs-lookup"><span data-stu-id="5410b-289">Run the following command and wait for the deployment to finish:</span></span>

    ```bash
    azbb -s <subscription_id> -g <resource group> -l <location> -p adfs-farm-rest.json --deploy
    ```

### <a name="configure-ad-fs-part-1"></a><span data-ttu-id="5410b-290">Konfigurieren von AD FS (Teil 1)</span><span class="sxs-lookup"><span data-stu-id="5410b-290">Configure AD FS (part 1)</span></span>

1. <span data-ttu-id="5410b-291">Öffnen Sie eine Remotedesktopsitzung mit dem virtuellen Computer mit dem Namen `ra-adfs-jb-vm1`. Dies ist die Jumpbox-VM.</span><span class="sxs-lookup"><span data-stu-id="5410b-291">Open a remote desktop session to the VM named `ra-adfs-jb-vm1`, which is the jumpbox VM.</span></span> <span data-ttu-id="5410b-292">Der Benutzername lautet `testuser`.</span><span class="sxs-lookup"><span data-stu-id="5410b-292">The user name is `testuser`.</span></span>

1. <span data-ttu-id="5410b-293">Öffnen Sie über die Jumpbox eine Remotedesktopsitzung mit dem virtuellen Computer `ra-adfs-proxy-vm1`.</span><span class="sxs-lookup"><span data-stu-id="5410b-293">From the jumpbox, open a remote desktop session to the VM named `ra-adfs-proxy-vm1`.</span></span> <span data-ttu-id="5410b-294">Die private IP-Adresse lautet 10.0.6.4.</span><span class="sxs-lookup"><span data-stu-id="5410b-294">The private IP address is 10.0.6.4.</span></span>

1. <span data-ttu-id="5410b-295">Führen Sie für diese Remotedesktopsitzung die [PowerShell ISE](/powershell/scripting/components/ise/windows-powershell-integrated-scripting-environment--ise-) aus.</span><span class="sxs-lookup"><span data-stu-id="5410b-295">From this remote desktop session, run the [PowerShell ISE](/powershell/scripting/components/ise/windows-powershell-integrated-scripting-environment--ise-).</span></span>

1. <span data-ttu-id="5410b-296">Navigieren Sie in PowerShell zum folgenden Verzeichnis:</span><span class="sxs-lookup"><span data-stu-id="5410b-296">In PowerShell, navigate to the following directory:</span></span>

    ```powershell
    C:\Packages\Plugins\Microsoft.Powershell.DSC\2.77.0.0\DSCWork\adfs-v2.0
    ```

1. <span data-ttu-id="5410b-297">Fügen Sie den folgenden Code in einen Skriptbereich ein, und führen Sie ihn aus:</span><span class="sxs-lookup"><span data-stu-id="5410b-297">Paste the following code into a script pane and run it:</span></span>

    ```powershell
    . .\adfs-webproxy.ps1
    $cd = @{
        AllNodes = @(
            @{
                NodeName = 'localhost'
                PSDscAllowPlainTextPassword = $true
                PSDscAllowDomainUser = $true
            }
        )
    }

    $c1 = Get-Credential -UserName testuser -Message "Enter password"
    InstallWebProxyApp -DomainName contoso.com -FederationName adfs.contoso.com -WebApplicationProxyName "Contoso App" -AdminCreds $c1 -ConfigurationData $cd
    Start-DscConfiguration .\InstallWebProxyApp
    ```

    <span data-ttu-id="5410b-298">Geben Sie an der `Get-Credential`-Eingabeaufforderung das Kennwort ein, das Sie in der Bereitstellungsparameterdatei angegeben haben.</span><span class="sxs-lookup"><span data-stu-id="5410b-298">At the `Get-Credential` prompt, enter the password that you specified in the deployment parameter file.</span></span>

1. <span data-ttu-id="5410b-299">Führen Sie den folgenden Befehl aus, um den Status der [DSC](/powershell/dsc/overview/overview)-Konfiguration zu überwachen:</span><span class="sxs-lookup"><span data-stu-id="5410b-299">Run the following command to monitor the progress of the [DSC](/powershell/dsc/overview/overview) configuration:</span></span>

    ```powershell
    Get-DscConfigurationStatus
    ```

    <span data-ttu-id="5410b-300">Es kann mehrere Minuten dauern, um Konsistenz zu erzielen.</span><span class="sxs-lookup"><span data-stu-id="5410b-300">It can take several minutes to reach consistency.</span></span> <span data-ttu-id="5410b-301">Während dieses Zeitraums werden unter Umständen Fehler für den Befehl angezeigt.</span><span class="sxs-lookup"><span data-stu-id="5410b-301">During this time, you may see errors from the command.</span></span> <span data-ttu-id="5410b-302">Wenn die Konfiguration erfolgreich ist, sollte die Ausgabe etwa wie folgt aussehen:</span><span class="sxs-lookup"><span data-stu-id="5410b-302">When the configuration succeeds, the output should look similar to the following:</span></span>

    ```powershell
    PS C:\Packages\Plugins\Microsoft.Powershell.DSC\2.77.0.0\DSCWork\adfs-v2.0> Get-DscConfigurationStatus

    Status     StartDate                 Type            Mode  RebootRequested      NumberOfResources
    ------     ---------                 ----            ----  ---------------      -----------------
    Success    12/17/2018 8:21:09 PM     Consistency     PUSH  True                 4
    ```

### <a name="configure-ad-fs-part-2"></a><span data-ttu-id="5410b-303">Konfigurieren von AD FS (Teil 2)</span><span class="sxs-lookup"><span data-stu-id="5410b-303">Configure AD FS (part 2)</span></span>

1. <span data-ttu-id="5410b-304">Öffnen Sie über die Jumpbox eine Remotedesktopsitzung mit dem virtuellen Computer `ra-adfs-proxy-vm2`.</span><span class="sxs-lookup"><span data-stu-id="5410b-304">From the jumpbox, open a remote desktop session to the VM named `ra-adfs-proxy-vm2`.</span></span> <span data-ttu-id="5410b-305">Die private IP-Adresse lautet 10.0.6.5.</span><span class="sxs-lookup"><span data-stu-id="5410b-305">The private IP address is 10.0.6.5.</span></span>

1. <span data-ttu-id="5410b-306">Führen Sie für diese Remotedesktopsitzung die [PowerShell ISE](/powershell/scripting/components/ise/windows-powershell-integrated-scripting-environment--ise-) aus.</span><span class="sxs-lookup"><span data-stu-id="5410b-306">From this remote desktop session, run the [PowerShell ISE](/powershell/scripting/components/ise/windows-powershell-integrated-scripting-environment--ise-).</span></span>

1. <span data-ttu-id="5410b-307">Navigieren Sie zum folgenden Verzeichnis:</span><span class="sxs-lookup"><span data-stu-id="5410b-307">Navigate to the following directory:</span></span>

    ```powershell
    C:\Packages\Plugins\Microsoft.Powershell.DSC\2.77.0.0\DSCWork\adfs-v2.0
    ```

1. <span data-ttu-id="5410b-308">Fügen Sie Folgendes in einen Skriptbereich ein, und führen Sie das Skript aus:</span><span class="sxs-lookup"><span data-stu-id="5410b-308">Past the following in a script pane and run the script:</span></span>

    ```powershell
    . .\adfs-webproxy-rest.ps1
    $cd = @{
        AllNodes = @(
            @{
                NodeName = 'localhost'
                PSDscAllowPlainTextPassword = $true
                PSDscAllowDomainUser = $true
            }
        )
    }

    $c1 = Get-Credential -UserName testuser -Message "Enter password"
    InstallWebProxy -DomainName contoso.com -FederationName adfs.contoso.com -WebApplicationProxyName "Contoso App" -AdminCreds $c1 -ConfigurationData $cd
    Start-DscConfiguration .\InstallWebProxy
    ```

    <span data-ttu-id="5410b-309">Geben Sie an der `Get-Credential`-Eingabeaufforderung das Kennwort ein, das Sie in der Bereitstellungsparameterdatei angegeben haben.</span><span class="sxs-lookup"><span data-stu-id="5410b-309">At the `Get-Credential` prompt, enter the password that you specified in the deployment parameter file.</span></span>

1. <span data-ttu-id="5410b-310">Führen Sie den folgenden Befehl aus, um den Status der DSC-Konfiguration zu überwachen:</span><span class="sxs-lookup"><span data-stu-id="5410b-310">Run the following command to monitor the progress of the DSC configuration:</span></span>

    ```powershell
    Get-DscConfigurationStatus
    ```

    <span data-ttu-id="5410b-311">Es kann mehrere Minuten dauern, um Konsistenz zu erzielen.</span><span class="sxs-lookup"><span data-stu-id="5410b-311">It can take several minutes to reach consistency.</span></span> <span data-ttu-id="5410b-312">Während dieses Zeitraums werden unter Umständen Fehler für den Befehl angezeigt.</span><span class="sxs-lookup"><span data-stu-id="5410b-312">During this time, you may see errors from the command.</span></span> <span data-ttu-id="5410b-313">Wenn die Konfiguration erfolgreich ist, sollte die Ausgabe etwa wie folgt aussehen:</span><span class="sxs-lookup"><span data-stu-id="5410b-313">When the configuration succeeds, the output should look similar to the following:</span></span>

    ```powershell
    PS C:\Packages\Plugins\Microsoft.Powershell.DSC\2.77.0.0\DSCWork\adfs-v2.0> Get-DscConfigurationStatus

    Status     StartDate                 Type            Mode  RebootRequested      NumberOfResources
    ------     ---------                 ----            ----  ---------------      -----------------
    Success    12/17/2018 8:21:09 PM     Consistency     PUSH  True                 4
    ```

    <span data-ttu-id="5410b-314">Es kann vorkommen, dass ein DSC-Fehler auftritt.</span><span class="sxs-lookup"><span data-stu-id="5410b-314">Sometimes this DSC fails.</span></span> <span data-ttu-id="5410b-315">Wenn bei der Statusprüfung `Status=Failure` und `Type=Consistency` angezeigt werden, können Sie versuchen, Schritt 4 erneut auszuführen.</span><span class="sxs-lookup"><span data-stu-id="5410b-315">If the status check shows `Status=Failure` and `Type=Consistency`, try re-running step 4.</span></span>

### <a name="sign-into-ad-fs"></a><span data-ttu-id="5410b-316">Anmelden an AD FS</span><span class="sxs-lookup"><span data-stu-id="5410b-316">Sign into AD FS</span></span>

1. <span data-ttu-id="5410b-317">Öffnen Sie über die Jumpbox eine Remotedesktopsitzung mit dem virtuellen Computer `ra-adfs-adfs-vm1`.</span><span class="sxs-lookup"><span data-stu-id="5410b-317">From the jumpbox, open a remote desktop session to the VM named `ra-adfs-adfs-vm1`.</span></span> <span data-ttu-id="5410b-318">Die private IP-Adresse lautet 10.0.5.4.</span><span class="sxs-lookup"><span data-stu-id="5410b-318">The private IP address is 10.0.5.4.</span></span>

1. <span data-ttu-id="5410b-319">Führen Sie die Schritte unter [Enable the Idp-Initiated Sign on page](/windows-server/identity/ad-fs/troubleshooting/ad-fs-tshoot-initiatedsignon#enable-the-idp-intiated-sign-on-page) (Aktivieren der vom Identitätsanbieter initiierten Anmeldeseite) aus.</span><span class="sxs-lookup"><span data-stu-id="5410b-319">Follow the steps in [Enable the Idp-Intiated Sign on page](/windows-server/identity/ad-fs/troubleshooting/ad-fs-tshoot-initiatedsignon#enable-the-idp-intiated-sign-on-page) to enable the sign-on page.</span></span>

1. <span data-ttu-id="5410b-320">Navigieren Sie von der Jumpbox zu `https://adfs.contoso.com/adfs/ls/idpinitiatedsignon.htm`.</span><span class="sxs-lookup"><span data-stu-id="5410b-320">From the jump box, browse to `https://adfs.contoso.com/adfs/ls/idpinitiatedsignon.htm`.</span></span> <span data-ttu-id="5410b-321">Unter Umständen wird eine Zertifikatwarnung angezeigt, die Sie für diesen Test ignorieren können.</span><span class="sxs-lookup"><span data-stu-id="5410b-321">You may receive a certificate warning that you can ignore for this test.</span></span>

1. <span data-ttu-id="5410b-322">Vergewissern Sie sich, dass die Anmeldeseite von Contoso Corporation angezeigt wird.</span><span class="sxs-lookup"><span data-stu-id="5410b-322">Verify that the Contoso Corporation sign-in page appears.</span></span> <span data-ttu-id="5410b-323">Melden Sie sich als **contoso\testuser** an.</span><span class="sxs-lookup"><span data-stu-id="5410b-323">Sign in as **contoso\testuser**.</span></span>

<!-- links -->
[extending-ad-to-azure]: adds-extend-domain.md

[vm-recommendations]: ../virtual-machines-windows/single-vm.md
[implementing-a-secure-hybrid-network-architecture]: ../dmz/secure-vnet-hybrid.md
[implementing-a-secure-hybrid-network-architecture-with-internet-access]: ../dmz/secure-vnet-dmz.md
[hybrid-azure-on-prem-vpn]: ../hybrid-networking/vpn.md

[azure-cli]: /azure/azure-resource-manager/xplat-cli-azure-resource-manager
[DRS]: https://technet.microsoft.com/library/dn280945.aspx
[where-to-place-an-fs-proxy]: https://technet.microsoft.com/library/dd807048.aspx
[ADDRS]: https://technet.microsoft.com/library/dn486831.aspx
[plan-your-adfs-deployment]: https://msdn.microsoft.com/library/azure/dn151324.aspx
[ad_network_recommendations]: #network_configuration_recommendations_for_AD_DS_VMs
[adfs_certificates]: https://technet.microsoft.com/library/dn781428(v=ws.11).aspx
[create_service_account_for_adfs_farm]: https://technet.microsoft.com/library/dd807078.aspx
[adfs-configuration-database]: https://technet.microsoft.com/library/ee913581(v=ws.11).aspx
[active-directory-federation-services]: /windows-server/identity/active-directory-federation-services
[security-considerations]: #security-considerations
[recommendations]: #recommendations
[active-directory-federation-services-overview]: https://technet.microsoft.com/library/hh831502(v=ws.11).aspx
[establishing-federation-trust]: https://blogs.msdn.microsoft.com/alextch/2011/06/27/establishing-federation-trust/
[Deploying_a_federation_server_farm]:  /windows-server/identity/ad-fs/deployment/deploying-a-federation-server-farm
[install_and_configure_the_web_application_proxy_server]: https://technet.microsoft.com/library/dn383662.aspx
[publish_applications_using_AD_FS_preauthentication]: https://technet.microsoft.com/library/dn383640.aspx
[managing-adfs-components]: https://technet.microsoft.com/library/cc759026.aspx
[oms-adfs-pack]: https://www.microsoft.com/download/details.aspx?id=41184
[azure-powershell-download]: /powershell/azure/overview
[aad]: /azure/active-directory/
[aadb2c]: /azure/active-directory-b2c/
[adfs-intro]: /azure/active-directory/hybrid/whatis-hybrid-identity
[github]: https://github.com/mspnp/identity-reference-architectures/tree/master/adfs
[adfs_certificates]: https://technet.microsoft.com/library/dn781428(v=ws.11).aspx
[considerations]: ./considerations.md
[visio-download]: https://archcenter.blob.core.windows.net/cdn/identity-architectures.vsdx
