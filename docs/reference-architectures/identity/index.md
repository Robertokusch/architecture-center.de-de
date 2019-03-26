---
title: Integrieren einer lokalen Active Directory-Instanz in Azure
titleSuffix: Azure Reference Architectures
description: Vergleich der Referenzarchitekturen für die Integration einer lokalen Active Directory-Instanz in Azure
ms.date: 07/02/2018
ms.topic: reference-architecture
ms.service: architecture-center
ms.subservice: reference-architecture
ms.custom: 'seodec18, identity'
---

# <a name="choose-a-solution-for-integrating-on-premises-active-directory-with-azure"></a><span data-ttu-id="e888a-103">Auswählen einer Lösung für die Integration einer lokalen Active Directory-Instanz in Azure</span><span class="sxs-lookup"><span data-stu-id="e888a-103">Choose a solution for integrating on-premises Active Directory with Azure</span></span>

<span data-ttu-id="e888a-104">In diesem Artikel werden Optionen für die Integration Ihrer lokalen Active Directory-Umgebung (AD) in ein Azure-Netzwerk verglichen.</span><span class="sxs-lookup"><span data-stu-id="e888a-104">This article compares options for integrating your on-premises Active Directory (AD) environment with an Azure network.</span></span> <span data-ttu-id="e888a-105">Für jede Option ist eine detailliertere Referenzarchitektur verfügbar.</span><span class="sxs-lookup"><span data-stu-id="e888a-105">For each option, a more detailed reference architecture is available.</span></span>

<span data-ttu-id="e888a-106">Viele Organisationen nutzen Active Directory Domain Services (AD DS), um mit Benutzern, Computern, Anwendungen oder anderen Ressourcen verknüpfte Identitäten zu authentifizieren, die in einer Sicherheitsbegrenzung enthalten sind.</span><span class="sxs-lookup"><span data-stu-id="e888a-106">Many organizations use Active Directory Domain Services (AD DS) to authenticate identities associated with users, computers, applications, or other resources that are included in a security boundary.</span></span> <span data-ttu-id="e888a-107">Verzeichnis- und Identitätsdienste werden in der Regel lokal gehostet, aber wenn Ihre Anwendung zum Teil lokal und zum Teil in Azure gehostet wird, können beim Senden von Authentifizierungsanforderungen von Azure zum lokalen System Latenzen auftreten.</span><span class="sxs-lookup"><span data-stu-id="e888a-107">Directory and identity services are typically hosted on-premises, but if your application is hosted partly on-premises and partly in Azure, there may be latency sending authentication requests from Azure back to on-premises.</span></span> <span data-ttu-id="e888a-108">Durch die Implementierung von Verzeichnis- und Identitätsdiensten in Azure können diese Latenzen verringert werden.</span><span class="sxs-lookup"><span data-stu-id="e888a-108">Implementing directory and identity services in Azure can reduce this latency.</span></span>

<span data-ttu-id="e888a-109">Azure stellt zwei Lösungen für die Implementierung von Verzeichnis- und Identitätsdiensten in Azure bereit:</span><span class="sxs-lookup"><span data-stu-id="e888a-109">Azure provides two solutions for implementing directory and identity services in Azure:</span></span>

- <span data-ttu-id="e888a-110">Verwenden Sie [Azure AD][azure-active-directory], um eine Active Directory-Domäne in der Cloud zu erstellen und mit Ihrer lokalen Active Directory-Domäne zu verbinden.</span><span class="sxs-lookup"><span data-stu-id="e888a-110">Use [Azure AD][azure-active-directory] to create an Active Directory domain in the cloud and connect it to your on-premises Active Directory domain.</span></span> <span data-ttu-id="e888a-111">Mit [Azure AD Connect][azure-ad-connect] können Sie Ihre lokalen Verzeichnisse in Azure AD integrieren.</span><span class="sxs-lookup"><span data-stu-id="e888a-111">[Azure AD Connect][azure-ad-connect] integrates your on-premises directories with Azure AD.</span></span>

- <span data-ttu-id="e888a-112">Erweitern Sie Ihre vorhandene lokale Active Directory-Infrastruktur auf Azure, indem Sie eine VM in Azure bereitstellen, die AD DS als Domänencontroller ausführt.</span><span class="sxs-lookup"><span data-stu-id="e888a-112">Extend your existing on-premises Active Directory infrastructure to Azure, by deploying a VM in Azure that runs AD DS as a domain controller.</span></span> <span data-ttu-id="e888a-113">Diese Architektur wird häufiger verwendet, wenn das lokale Netzwerk und das virtuelle Azure-Netzwerk (VNET) über eine VPN- oder ExpressRoute-Verbindung miteinander verbunden sind.</span><span class="sxs-lookup"><span data-stu-id="e888a-113">This architecture is more common when the on-premises network and the Azure virtual network (VNet) are connected by a VPN or ExpressRoute connection.</span></span> <span data-ttu-id="e888a-114">Es sind mehrere Varianten dieser Architektur möglich:</span><span class="sxs-lookup"><span data-stu-id="e888a-114">Several variations of this architecture are possible:</span></span>

  - <span data-ttu-id="e888a-115">Erstellen einer Domäne in Azure, die dann zu Ihrer lokalen AD-Gesamtstruktur hinzugefügt wird</span><span class="sxs-lookup"><span data-stu-id="e888a-115">Create a domain in Azure and join it to your on-premises AD forest.</span></span>
  - <span data-ttu-id="e888a-116">Erstellen einer separaten Gesamtstruktur in Azure, die für Domänen in Ihrer lokalen Gesamtstruktur vertrauenswürdig sind</span><span class="sxs-lookup"><span data-stu-id="e888a-116">Create a separate forest in Azure that is trusted by domains in your on-premises forest.</span></span>
  - <span data-ttu-id="e888a-117">Replizieren einer AD FS-Bereitstellung (Active Directory-Verbunddienste) nach Azure</span><span class="sxs-lookup"><span data-stu-id="e888a-117">Replicate an Active Directory Federation Services (AD FS) deployment to Azure.</span></span>

<span data-ttu-id="e888a-118">In den folgenden Abschnitten werden diese Optionen ausführlicher beschrieben.</span><span class="sxs-lookup"><span data-stu-id="e888a-118">The next sections describe each of these options in more detail.</span></span>

## <a name="integrate-your-on-premises-domains-with-azure-ad"></a><span data-ttu-id="e888a-119">Integrieren von lokalen Domänen in Azure AD</span><span class="sxs-lookup"><span data-stu-id="e888a-119">Integrate your on-premises domains with Azure AD</span></span>

<span data-ttu-id="e888a-120">Verwenden Sie Azure Active Directory (Azure AD), um eine Domäne in Azure zu erstellen und mit einer lokalen AD-Domäne zu verknüpfen.</span><span class="sxs-lookup"><span data-stu-id="e888a-120">Use Azure Active Directory (Azure AD) to create a domain in Azure and link it to an on-premises AD domain.</span></span>

<span data-ttu-id="e888a-121">Das Azure AD-Verzeichnis stellt keine Erweiterung eines lokalen Verzeichnisses dar.</span><span class="sxs-lookup"><span data-stu-id="e888a-121">The Azure AD directory is not an extension of an on-premises directory.</span></span> <span data-ttu-id="e888a-122">Es handelt sich vielmehr um eine Kopie, die die gleichen Objekte und Identitäten enthält.</span><span class="sxs-lookup"><span data-stu-id="e888a-122">Rather, it's a copy that contains the same objects and identities.</span></span> <span data-ttu-id="e888a-123">Die lokal an diesen Elementen vorgenommenen Änderungen werden in Azure AD kopiert, wohingegen Änderungen in Azure AD nicht wieder zur lokalen Domäne repliziert werden.</span><span class="sxs-lookup"><span data-stu-id="e888a-123">Changes made to these items on-premises are copied to Azure AD, but changes made in Azure AD are not replicated back to the on-premises domain.</span></span>

<span data-ttu-id="e888a-124">Sie können Azure AD auch ohne ein lokales Verzeichnis verwenden.</span><span class="sxs-lookup"><span data-stu-id="e888a-124">You can also use Azure AD without using an on-premises directory.</span></span> <span data-ttu-id="e888a-125">In diesem Fall enthält Azure AD keine Daten, die von einem lokalen Verzeichnis repliziert wurden, sondern verhält sich als primäre Quelle für alle Identitätsinformationen.</span><span class="sxs-lookup"><span data-stu-id="e888a-125">In this case, Azure AD acts as the primary source of all identity information, rather than containing data replicated from an on-premises directory.</span></span>

<span data-ttu-id="e888a-126">**Vorteile**</span><span class="sxs-lookup"><span data-stu-id="e888a-126">**Benefits**</span></span>

- <span data-ttu-id="e888a-127">Sie müssen keine AD-Infrastruktur in der Cloud verwalten.</span><span class="sxs-lookup"><span data-stu-id="e888a-127">You don't need to maintain an AD infrastructure in the cloud.</span></span> <span data-ttu-id="e888a-128">Azure AD wird vollständig von Microsoft verwaltet und gewartet.</span><span class="sxs-lookup"><span data-stu-id="e888a-128">Azure AD is entirely managed and maintained by Microsoft.</span></span>
- <span data-ttu-id="e888a-129">Azure AD stellt die gleichen Identitätsinformationen bereit, die lokal verfügbar sind.</span><span class="sxs-lookup"><span data-stu-id="e888a-129">Azure AD provides the same identity information that is available on-premises.</span></span>
- <span data-ttu-id="e888a-130">Die Authentifizierung kann in Azure durchgeführt werden, wodurch externe Anwendungen und Benutzer nicht so häufig eine Verbindung mit der lokalen Domäne herstellen müssen.</span><span class="sxs-lookup"><span data-stu-id="e888a-130">Authentication can happen in Azure, reducing the need for external applications and users to contact the on-premises domain.</span></span>

<span data-ttu-id="e888a-131">**Herausforderungen**</span><span class="sxs-lookup"><span data-stu-id="e888a-131">**Challenges**</span></span>

- <span data-ttu-id="e888a-132">Identitätsdienste sind auf Benutzer und Gruppen beschränkt.</span><span class="sxs-lookup"><span data-stu-id="e888a-132">Identity services are limited to users and groups.</span></span> <span data-ttu-id="e888a-133">Es gibt keine Möglichkeit zur Authentifizierung von Dienst- und Computerkonten.</span><span class="sxs-lookup"><span data-stu-id="e888a-133">There is no ability to authenticate service and computer accounts.</span></span>
- <span data-ttu-id="e888a-134">Sie müssen die Konnektivität mit Ihrer lokalen Domäne konfigurieren, damit Azure AD Directory weiterhin synchronisiert wird.</span><span class="sxs-lookup"><span data-stu-id="e888a-134">You must configure connectivity with your on-premises domain to keep the Azure AD directory synchronized.</span></span>
- <span data-ttu-id="e888a-135">Anwendungen müssen möglicherweise neu geschrieben werden, um eine Authentifizierung über Azure AD zu ermöglichen.</span><span class="sxs-lookup"><span data-stu-id="e888a-135">Applications may need to be rewritten to enable authentication through Azure AD.</span></span>

<span data-ttu-id="e888a-136">**Referenzarchitektur**</span><span class="sxs-lookup"><span data-stu-id="e888a-136">**Reference architecture**</span></span>

- <span data-ttu-id="e888a-137">[Integrieren von lokalen Active Directory-Domänen in Azure Active Directory][aad]</span><span class="sxs-lookup"><span data-stu-id="e888a-137">[Integrate on-premises Active Directory domains with Azure Active Directory][aad]</span></span>

## <a name="ad-ds-in-azure-joined-to-an-on-premises-forest"></a><span data-ttu-id="e888a-138">Verknüpfung von AD DS in Azure mit einer lokalen Gesamtstruktur</span><span class="sxs-lookup"><span data-stu-id="e888a-138">AD DS in Azure joined to an on-premises forest</span></span>

<span data-ttu-id="e888a-139">Stellen Sie AD DS-Server (AD Domain Services) für Azure bereit.</span><span class="sxs-lookup"><span data-stu-id="e888a-139">Deploy AD Domain Services (AD DS) servers to Azure.</span></span> <span data-ttu-id="e888a-140">Erstellen Sie eine Domäne in Azure, und fügen Sie sie zu Ihrer lokalen AD-Gesamtstruktur hinzu.</span><span class="sxs-lookup"><span data-stu-id="e888a-140">Create a domain in Azure and join it to your on-premises AD forest.</span></span>

<span data-ttu-id="e888a-141">Ziehen Sie diese Option in Erwägung, wenn Sie AD DS-Features verwenden, die zurzeit nicht von Azure AD implementiert sind.</span><span class="sxs-lookup"><span data-stu-id="e888a-141">Consider this option if you need to use AD DS features that are not currently implemented by Azure AD.</span></span>

<span data-ttu-id="e888a-142">**Vorteile**</span><span class="sxs-lookup"><span data-stu-id="e888a-142">**Benefits**</span></span>

- <span data-ttu-id="e888a-143">AD DS bietet Zugriff auf die gleichen Identitätsinformationen, die lokal verfügbar sind.</span><span class="sxs-lookup"><span data-stu-id="e888a-143">Provides access to the same identity information that is available on-premises.</span></span>
- <span data-ttu-id="e888a-144">Sie können Benutzer-, Dienst- und Computerkonten lokal und in Azure authentifizieren.</span><span class="sxs-lookup"><span data-stu-id="e888a-144">You can authenticate user, service, and computer accounts on-premises and in Azure.</span></span>
- <span data-ttu-id="e888a-145">Sie müssen keine separate AD-Gesamtstruktur verwalten.</span><span class="sxs-lookup"><span data-stu-id="e888a-145">You don't need to manage a separate AD forest.</span></span> <span data-ttu-id="e888a-146">Die Domäne in Azure kann der lokalen Gesamtstruktur angehören.</span><span class="sxs-lookup"><span data-stu-id="e888a-146">The domain in Azure can belong to the on-premises forest.</span></span>
- <span data-ttu-id="e888a-147">Sie können die von lokalen Gruppenrichtlinienobjekten definierte Gruppenrichtlinie auf die Domäne in Azure anwenden.</span><span class="sxs-lookup"><span data-stu-id="e888a-147">You can apply group policy defined by on-premises Group Policy Objects to the domain in Azure.</span></span>

<span data-ttu-id="e888a-148">**Herausforderungen**</span><span class="sxs-lookup"><span data-stu-id="e888a-148">**Challenges**</span></span>

- <span data-ttu-id="e888a-149">Sie müssen Ihre eigenen AD DS-Server und -Domänen in der Cloud bereitstellen und verwalten.</span><span class="sxs-lookup"><span data-stu-id="e888a-149">You must deploy and manage your own AD DS servers and domain in the cloud.</span></span>
- <span data-ttu-id="e888a-150">Es gibt möglicherweise gewisse Latenzen bei der Synchronisierung zwischen den Domänenservern in der Cloud und den lokal ausgeführten Servern.</span><span class="sxs-lookup"><span data-stu-id="e888a-150">There may be some synchronization latency between the domain servers in the cloud and the servers running on-premises.</span></span>

<span data-ttu-id="e888a-151">**Referenzarchitektur**</span><span class="sxs-lookup"><span data-stu-id="e888a-151">**Reference architecture**</span></span>

- <span data-ttu-id="e888a-152">[Erweitern von Active Directory Domain Services (AD DS) auf Azure][ad-ds]</span><span class="sxs-lookup"><span data-stu-id="e888a-152">[Extend Active Directory Domain Services (AD DS) to Azure][ad-ds]</span></span>

## <a name="ad-ds-in-azure-with-a-separate-forest"></a><span data-ttu-id="e888a-153">AD DS in Azure mit einer separaten Gesamtstruktur</span><span class="sxs-lookup"><span data-stu-id="e888a-153">AD DS in Azure with a separate forest</span></span>

<span data-ttu-id="e888a-154">Stellen Sie AD DS-Server (AD Domain Services) für Azure bereit, erstellen Sie jedoch eine separate Active Directory-[Gesamtstruktur][ad-forest-defn], die von der lokalen Gesamtstruktur getrennt ist.</span><span class="sxs-lookup"><span data-stu-id="e888a-154">Deploy AD Domain Services (AD DS) servers to Azure, but create a separate Active Directory [forest][ad-forest-defn] that is separate from the on-premises forest.</span></span> <span data-ttu-id="e888a-155">Domänen in Ihrer lokalen Gesamtstruktur sind für diese Gesamtstruktur vertrauenswürdig.</span><span class="sxs-lookup"><span data-stu-id="e888a-155">This forest is trusted by domains in your on-premises forest.</span></span>

<span data-ttu-id="e888a-156">Zu den typischen Einsatzmöglichkeiten dieser Architektur zählen die Verwaltung einer Sicherheitstrennung für Objekte und Identitäten, die in der Cloud gespeichert werden, sowie die Migration einzelner Domänen aus einem lokalen System in die Cloud.</span><span class="sxs-lookup"><span data-stu-id="e888a-156">Typical uses for this architecture include maintaining security separation for objects and identities held in the cloud, and migrating individual domains from on-premises to the cloud.</span></span>

<span data-ttu-id="e888a-157">**Vorteile**</span><span class="sxs-lookup"><span data-stu-id="e888a-157">**Benefits**</span></span>

- <span data-ttu-id="e888a-158">Sie können lokale Identitäten implementieren und von reinen Azure-Identitäten trennen.</span><span class="sxs-lookup"><span data-stu-id="e888a-158">You can implement on-premises identities and separate Azure-only identities.</span></span>
- <span data-ttu-id="e888a-159">Sie müssen keine Replikation von der lokalen AD-Gesamtstruktur nach Azure durchführen.</span><span class="sxs-lookup"><span data-stu-id="e888a-159">You don't need to replicate from the on-premises AD forest to Azure.</span></span>

<span data-ttu-id="e888a-160">**Herausforderungen**</span><span class="sxs-lookup"><span data-stu-id="e888a-160">**Challenges**</span></span>

- <span data-ttu-id="e888a-161">Eine Authentifizierung in Azure für lokale Identitäten erfordert zusätzliche Netzwerkhops zu lokalen AD-Servern.</span><span class="sxs-lookup"><span data-stu-id="e888a-161">Authentication within Azure for on-premises identities requires extra network hops to the on-premises AD servers.</span></span>
- <span data-ttu-id="e888a-162">Sie müssen Ihre eigenen AD DS-Server und die Gesamtstruktur in der Cloud bereitstellen und die entsprechenden Vertrauensstellungen zwischen Gesamtstrukturen einrichten.</span><span class="sxs-lookup"><span data-stu-id="e888a-162">You must deploy your own AD DS servers and forest in the cloud, and establish the appropriate trust relationships between forests.</span></span>

<span data-ttu-id="e888a-163">**Referenzarchitektur**</span><span class="sxs-lookup"><span data-stu-id="e888a-163">**Reference architecture**</span></span>

- <span data-ttu-id="e888a-164">[Erstellen einer Active Directory Domain Services (AD DS)-Ressourcengesamtstruktur in Azure][ad-ds-forest]</span><span class="sxs-lookup"><span data-stu-id="e888a-164">[Create an Active Directory Domain Services (AD DS) resource forest in Azure][ad-ds-forest]</span></span>

## <a name="extend-ad-fs-to-azure"></a><span data-ttu-id="e888a-165">Erweiterung von AD FS auf Azure</span><span class="sxs-lookup"><span data-stu-id="e888a-165">Extend AD FS to Azure</span></span>

<span data-ttu-id="e888a-166">Replizieren Sie eine AD FS-Bereitstellung (Active Directory-Verbunddienste) nach Azure, um eine Verbundauthentifizierung und -autorisierung für in Azure ausgeführte Komponenten auszuführen.</span><span class="sxs-lookup"><span data-stu-id="e888a-166">Replicate an Active Directory Federation Services (AD FS) deployment to Azure, to perform federated authentication and authorization for components running in Azure.</span></span>

<span data-ttu-id="e888a-167">Typische Einsatzmöglichkeiten für diese Architektur sind Folgende:</span><span class="sxs-lookup"><span data-stu-id="e888a-167">Typical uses for this architecture:</span></span>

- <span data-ttu-id="e888a-168">Authentifizieren und Autorisieren von Benutzern von Partnerorganisationen</span><span class="sxs-lookup"><span data-stu-id="e888a-168">Authenticate and authorize users from partner organizations.</span></span>
- <span data-ttu-id="e888a-169">Ermöglichung einer Benutzerauthentifizierung über Webbrowser, die außerhalb der Firewall der Organisation ausgeführt werden</span><span class="sxs-lookup"><span data-stu-id="e888a-169">Allow users to authenticate from web browsers running outside of the organizational firewall.</span></span>
- <span data-ttu-id="e888a-170">Ermöglichung eines Verbindungsaufbaus durch Benutzer über autorisierte externe Geräte wie Mobilgeräte</span><span class="sxs-lookup"><span data-stu-id="e888a-170">Allow users to connect from authorized external devices such as mobile devices.</span></span>

<span data-ttu-id="e888a-171">**Vorteile**</span><span class="sxs-lookup"><span data-stu-id="e888a-171">**Benefits**</span></span>

- <span data-ttu-id="e888a-172">Sie können Ansprüche unterstützende Anwendungen nutzen.</span><span class="sxs-lookup"><span data-stu-id="e888a-172">You can leverage claims-aware applications.</span></span>
- <span data-ttu-id="e888a-173">AD FS bietet die Möglichkeit, zur Authentifizierung externen Partnern zu vertrauen.</span><span class="sxs-lookup"><span data-stu-id="e888a-173">Provides the ability to trust external partners for authentication.</span></span>
- <span data-ttu-id="e888a-174">Es besteht Kompatibilität mit einem großen Spektrum an Authentifizierungsprotokollen.</span><span class="sxs-lookup"><span data-stu-id="e888a-174">Compatibility with large set of authentication protocols.</span></span>

<span data-ttu-id="e888a-175">**Herausforderungen**</span><span class="sxs-lookup"><span data-stu-id="e888a-175">**Challenges**</span></span>

- <span data-ttu-id="e888a-176">Sie müssen Ihre eigenen AD DS-, AD FS- und AD FS-Webanwendungsproxy-Server in Azure bereitstellen.</span><span class="sxs-lookup"><span data-stu-id="e888a-176">You must deploy your own AD DS, AD FS, and AD FS Web Application Proxy servers in Azure.</span></span>
- <span data-ttu-id="e888a-177">Die Konfiguration diese Architektur kann komplex sein.</span><span class="sxs-lookup"><span data-stu-id="e888a-177">This architecture can be complex to configure.</span></span>

<span data-ttu-id="e888a-178">**Referenzarchitektur**</span><span class="sxs-lookup"><span data-stu-id="e888a-178">**Reference architecture**</span></span>

- <span data-ttu-id="e888a-179">[Erweitern von Active Directory Federation Services (AD FS) auf Azure][adfs]</span><span class="sxs-lookup"><span data-stu-id="e888a-179">[Extend Active Directory Federation Services (AD FS) to Azure][adfs]</span></span>

<!-- links -->

[aad]: ./azure-ad.md
[ad-ds]: ./adds-extend-domain.md
[ad-ds-forest]: ./adds-forest.md
[ad-forest-defn]: /windows/desktop/AD/forests
[adfs]: ./adfs.md

[azure-active-directory]: /azure/active-directory-domain-services/active-directory-ds-overview
[azure-ad-connect]: /azure/active-directory/hybrid/whatis-hybrid-identity
