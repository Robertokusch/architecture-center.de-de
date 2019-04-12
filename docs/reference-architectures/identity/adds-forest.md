---
title: Erstellen einer AD DS-Ressourcengesamtstruktur in Azure
titleSuffix: Azure Reference Architectures
description: >-
  Informationen zum Erstellen einer vertrauenswürdigen Active Directory-Domäne in Azure.

  Anleitungen, VPN-Gateway, ExpressRoute, Lastenausgleich, virtuelles Netzwerk, Active Directory
author: telmosampaio
ms.date: 05/02/2018
ms.topic: reference-architecture
ms.service: architecture-center
ms.subservice: reference-architecture
ms.custom: seodec18, identity
ms.openlocfilehash: 5fe966f657782b41ec1926d0fd4bb83eb7a3c0fb
ms.sourcegitcommit: 548374a0133f3caed3934fda6a380c76e6eaecea
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/25/2019
ms.locfileid: "58420038"
---
# <a name="create-an-active-directory-domain-services-ad-ds-resource-forest-in-azure"></a><span data-ttu-id="80976-104">Erstellen einer Active Directory Domain Services (AD DS)-Ressourcengesamtstruktur in Azure</span><span class="sxs-lookup"><span data-stu-id="80976-104">Create an Active Directory Domain Services (AD DS) resource forest in Azure</span></span>

<span data-ttu-id="80976-105">Diese Referenzarchitektur zeigt, wie eine separate Active Directory-Domäne in Azure erstellt wird, der Domänen in der lokalen Active Directory-Gesamtstruktur vertrauen.</span><span class="sxs-lookup"><span data-stu-id="80976-105">This reference architecture shows how to create a separate Active Directory domain in Azure that is trusted by domains in your on-premises AD forest.</span></span> <span data-ttu-id="80976-106">[**Stellen Sie diese Lösung bereit**](#deploy-the-solution).</span><span class="sxs-lookup"><span data-stu-id="80976-106">[**Deploy this solution**](#deploy-the-solution).</span></span>

![Schützen einer Hybrid-Netzwerkarchitektur mit separaten Active Directory-Domänen](./images/adds-forest.png)

<span data-ttu-id="80976-108">*Laden Sie eine [Visio-Datei][visio-download] mit dieser Architektur herunter.*</span><span class="sxs-lookup"><span data-stu-id="80976-108">*Download a [Visio file][visio-download] of this architecture.*</span></span>

<span data-ttu-id="80976-109">In Active Directory Domain Services (AD DS) werden Identitätsinformationen in einer hierarchischen Struktur gespeichert.</span><span class="sxs-lookup"><span data-stu-id="80976-109">Active Directory Domain Services (AD DS) stores identity information in a hierarchical structure.</span></span> <span data-ttu-id="80976-110">Der oberste Knoten in der hierarchischen Struktur wird als Gesamtstruktur bezeichnet.</span><span class="sxs-lookup"><span data-stu-id="80976-110">The top node in the hierarchical structure is known as a forest.</span></span> <span data-ttu-id="80976-111">Eine Gesamtstruktur enthält Domänen, und Domänen enthalten andere Objekttypen.</span><span class="sxs-lookup"><span data-stu-id="80976-111">A forest contains domains, and domains contain other types of objects.</span></span> <span data-ttu-id="80976-112">In dieser Referenzarchitektur wird eine AD DS-Gesamtstruktur in Azure mit einer unidirektionalen ausgehenden Vertrauensstellung mit einer lokalen Domäne erstellt.</span><span class="sxs-lookup"><span data-stu-id="80976-112">This reference architecture creates an AD DS forest in Azure with a one-way outgoing trust relationship with an on-premises domain.</span></span> <span data-ttu-id="80976-113">Die Gesamtstruktur in Azure enthält eine Domäne, die lokal nicht vorhanden ist.</span><span class="sxs-lookup"><span data-stu-id="80976-113">The forest in Azure contains a domain that does not exist on-premises.</span></span> <span data-ttu-id="80976-114">Aufgrund der Vertrauensstellung werden Anmeldungen bei lokalen Domänen als vertrauenswürdig für den Zugriff auf Ressourcen in der separaten Azure-Domäne angesehen.</span><span class="sxs-lookup"><span data-stu-id="80976-114">Because of the trust relationship, logons made against on-premises domains can be trusted for access to resources in the separate Azure domain.</span></span>

<span data-ttu-id="80976-115">Zu den typischen Verwendungsmöglichkeiten dieser Architektur zählen die Verwaltung einer Sicherheitstrennung für in der Cloud gespeicherte Objekte und Identitäten sowie die Migration einzelner Domänen aus einem lokalen System in die Cloud.</span><span class="sxs-lookup"><span data-stu-id="80976-115">Typical uses for this architecture include maintaining security separation for objects and identities held in the cloud, and migrating individual domains from on-premises to the cloud.</span></span>

<span data-ttu-id="80976-116">Weitere Überlegungen finden Sie unter [Auswählen einer Lösung für die Integration einer lokalen Active Directory-Instanz in Azure][considerations].</span><span class="sxs-lookup"><span data-stu-id="80976-116">For additional considerations, see [Choose a solution for integrating on-premises Active Directory with Azure][considerations].</span></span>

## <a name="architecture"></a><span data-ttu-id="80976-117">Architecture</span><span class="sxs-lookup"><span data-stu-id="80976-117">Architecture</span></span>

<span data-ttu-id="80976-118">Diese Architektur besteht aus den folgenden Komponenten.</span><span class="sxs-lookup"><span data-stu-id="80976-118">The architecture has the following components.</span></span>

- <span data-ttu-id="80976-119">**Lokales Netzwerk**.</span><span class="sxs-lookup"><span data-stu-id="80976-119">**On-premises network**.</span></span> <span data-ttu-id="80976-120">Das lokale Netzwerk enthält eine eigene Active Directory-Gesamtstruktur und eigene Domänen.</span><span class="sxs-lookup"><span data-stu-id="80976-120">The on-premises network contains its own Active Directory forest and domains.</span></span>
- <span data-ttu-id="80976-121">**Active Directory-Server**.</span><span class="sxs-lookup"><span data-stu-id="80976-121">**Active Directory servers**.</span></span> <span data-ttu-id="80976-122">Hierbei handelt es sich um Domänencontroller, die Domänendienste implementieren und als virtuelle Computer in der Cloud ausgeführt werden.</span><span class="sxs-lookup"><span data-stu-id="80976-122">These are domain controllers implementing domain services running as VMs in the cloud.</span></span> <span data-ttu-id="80976-123">Diese Server hosten eine Gesamtstruktur mit einer oder mehrere Domänen, die von den lokal gehosteten getrennt sind.</span><span class="sxs-lookup"><span data-stu-id="80976-123">These servers host a forest containing one or more domains, separate from those located on-premises.</span></span>
- <span data-ttu-id="80976-124">**Unidirektionale Vertrauensstellung**.</span><span class="sxs-lookup"><span data-stu-id="80976-124">**One-way trust relationship**.</span></span> <span data-ttu-id="80976-125">Das Beispiel im Diagramm zeigt eine unidirektionale Vertrauensstellung von der Domäne in Azure zur lokalen Domäne.</span><span class="sxs-lookup"><span data-stu-id="80976-125">The example in the diagram shows a one-way trust from the domain in Azure to the on-premises domain.</span></span> <span data-ttu-id="80976-126">Diese Beziehung ermöglicht lokalen Benutzern den Zugriff auf Ressourcen in der Domäne in Azure, jedoch nicht umgekehrt.</span><span class="sxs-lookup"><span data-stu-id="80976-126">This relationship enables on-premises users to access resources in the domain in Azure, but not the other way around.</span></span> <span data-ttu-id="80976-127">Es kann eine bidirektionale Vertrauensstellung erstellt werden, wenn Cloudbenutzer auch Zugriff auf lokale Ressourcen benötigen.</span><span class="sxs-lookup"><span data-stu-id="80976-127">It is possible to create a two-way trust if cloud users also require access to on-premises resources.</span></span>
- <span data-ttu-id="80976-128">**Active Directory-Subnetz**.</span><span class="sxs-lookup"><span data-stu-id="80976-128">**Active Directory subnet**.</span></span> <span data-ttu-id="80976-129">Die AD DS-Server werden in einem separaten Subnetz gehostet.</span><span class="sxs-lookup"><span data-stu-id="80976-129">The AD DS servers are hosted in a separate subnet.</span></span> <span data-ttu-id="80976-130">NSG-Regeln (Netzwerksicherheitsgruppe) schützen die AD DS-Server und stellen eine Firewall für Datenverkehr von unerwarteten Quellen dar.</span><span class="sxs-lookup"><span data-stu-id="80976-130">Network security group (NSG) rules protect the AD DS servers and provide a firewall against traffic from unexpected sources.</span></span>
- <span data-ttu-id="80976-131">**Azure-Gateway**.</span><span class="sxs-lookup"><span data-stu-id="80976-131">**Azure gateway**.</span></span> <span data-ttu-id="80976-132">Das Azure-Gateway stellt eine Verbindung zwischen dem lokalen Netzwerk und dem Azure-VNet her.</span><span class="sxs-lookup"><span data-stu-id="80976-132">The Azure gateway provides a connection between the on-premises network and the Azure VNet.</span></span> <span data-ttu-id="80976-133">Dabei kann es sich um eine [VPN-Verbindung][azure-vpn-gateway] oder um [Azure ExpressRoute][azure-expressroute] handeln.</span><span class="sxs-lookup"><span data-stu-id="80976-133">This can be a [VPN connection][azure-vpn-gateway] or [Azure ExpressRoute][azure-expressroute].</span></span> <span data-ttu-id="80976-134">Weitere Informationen finden Sie unter [Implementieren einer sicheren Hybrid-Netzwerkarchitektur in Azure][implementing-a-secure-hybrid-network-architecture].</span><span class="sxs-lookup"><span data-stu-id="80976-134">For more information, see [Implementing a secure hybrid network architecture in Azure][implementing-a-secure-hybrid-network-architecture].</span></span>

## <a name="recommendations"></a><span data-ttu-id="80976-135">Empfehlungen</span><span class="sxs-lookup"><span data-stu-id="80976-135">Recommendations</span></span>

<span data-ttu-id="80976-136">Spezifische Empfehlungen zur Implementierung von Active Directory in Azure finden unter [Erweitern von Active Directory Domain Services (AD DS) auf Azure][adds-extend-domain].</span><span class="sxs-lookup"><span data-stu-id="80976-136">For specific recommendations on implementing Active Directory in Azure, see [Extending Active Directory Domain Services (AD DS) to Azure][adds-extend-domain].</span></span>

### <a name="trust"></a><span data-ttu-id="80976-137">Vertrauen</span><span class="sxs-lookup"><span data-stu-id="80976-137">Trust</span></span>

<span data-ttu-id="80976-138">Die lokalen Domänen sind in einer anderen Gesamtstruktur als die Domänen in der Cloud enthalten.</span><span class="sxs-lookup"><span data-stu-id="80976-138">The on-premises domains are contained within a different forest from the domains in the cloud.</span></span> <span data-ttu-id="80976-139">Um die Authentifizierung von lokalen Benutzern in der Cloud zu ermöglichen, müssen die Domänen in Azure der Anmeldedomäne in der lokalen Gesamtstruktur vertrauen.</span><span class="sxs-lookup"><span data-stu-id="80976-139">To enable authentication of on-premises users in the cloud, the domains in Azure must trust the logon domain in the on-premises forest.</span></span> <span data-ttu-id="80976-140">Ebenso kann es bei Bereitstellung einer Anmeldedomäne für externe Benutzer in der Cloud erforderlich sein, dass die lokale Gesamtstruktur der Clouddomäne vertraut.</span><span class="sxs-lookup"><span data-stu-id="80976-140">Similarly, if the cloud provides a logon domain for external users, it may be necessary for the on-premises forest to trust the cloud domain.</span></span>

<span data-ttu-id="80976-141">Sie können Vertrauensstellungen auf Gesamtstrukturebene durch [Erstellen von Gesamtstrukturvertrauensstellungen][creating-forest-trusts] oder auf Domänenebene durch [Erstellen externer Vertrauensstellungen][creating-external-trusts] einrichten.</span><span class="sxs-lookup"><span data-stu-id="80976-141">You can establish trusts at the forest level by [creating forest trusts][creating-forest-trusts], or at the domain level by [creating external trusts][creating-external-trusts].</span></span> <span data-ttu-id="80976-142">Eine Vertrauensstellung auf Gesamtstrukturebene erstellt eine Beziehung zwischen allen Domänen in zwei Gesamtstrukturen.</span><span class="sxs-lookup"><span data-stu-id="80976-142">A forest level trust creates a relationship between all domains in two forests.</span></span> <span data-ttu-id="80976-143">Eine Vertrauensstellung auf Ebene externer Domänen erstellt nur eine Beziehung zwischen zwei angegebenen Domänen.</span><span class="sxs-lookup"><span data-stu-id="80976-143">An external domain level trust only creates a relationship between two specified domains.</span></span> <span data-ttu-id="80976-144">Sie sollten Vertrauensstellungen auf Ebene externer Domänen nur zwischen Domänen in verschiedenen Gesamtstrukturen erstellen.</span><span class="sxs-lookup"><span data-stu-id="80976-144">You should only create external domain level trusts between domains in different forests.</span></span>

<span data-ttu-id="80976-145">Vertrauensstellungen können unidirektional oder bidirektional sein:</span><span class="sxs-lookup"><span data-stu-id="80976-145">Trusts can be unidirectional (one-way) or bidirectional (two-way):</span></span>

- <span data-ttu-id="80976-146">Eine unidirektionale Vertrauensstellung ermöglicht Benutzern in einer Domäne oder Gesamtstruktur (als *eingehende* Domäne oder Gesamtstruktur bezeichnet) Zugriff auf die Ressourcen in einer anderen Domäne oder Gesamtstruktur (als *ausgehende* Domäne oder Gesamtstruktur bezeichnet).</span><span class="sxs-lookup"><span data-stu-id="80976-146">A one-way trust enables users in one domain or forest (known as the *incoming* domain or forest) to access the resources held in another (the *outgoing* domain or forest).</span></span>
- <span data-ttu-id="80976-147">Eine bidirektionale Vertrauensstellung ermöglicht Benutzern in beiden Domänen oder Gesamtstrukturen den Zugriff auf Ressourcen in der jeweils anderen Domäne oder Gesamtstruktur.</span><span class="sxs-lookup"><span data-stu-id="80976-147">A two-way trust enables users in either domain or forest to access resources held in the other.</span></span>

<span data-ttu-id="80976-148">Die folgende Tabelle enthält eine Übersicht über Vertrauensstellungskonfigurationen für einige einfache Szenarien:</span><span class="sxs-lookup"><span data-stu-id="80976-148">The following table summarizes trust configurations for some simple scenarios:</span></span>

| <span data-ttu-id="80976-149">Szenario</span><span class="sxs-lookup"><span data-stu-id="80976-149">Scenario</span></span> | <span data-ttu-id="80976-150">Lokale Vertrauensstellung</span><span class="sxs-lookup"><span data-stu-id="80976-150">On-premises trust</span></span> | <span data-ttu-id="80976-151">Cloudvertrauensstellung</span><span class="sxs-lookup"><span data-stu-id="80976-151">Cloud trust</span></span> |
| --- | --- | --- |
| <span data-ttu-id="80976-152">Lokale Benutzer benötigen Zugriff auf Ressourcen in der Cloud, aber nicht umgekehrt</span><span class="sxs-lookup"><span data-stu-id="80976-152">On-premises users require access to resources in the cloud, but not vice versa</span></span> |<span data-ttu-id="80976-153">Unidirektional, eingehend</span><span class="sxs-lookup"><span data-stu-id="80976-153">One-way, incoming</span></span> |<span data-ttu-id="80976-154">Unidirektional, ausgehend</span><span class="sxs-lookup"><span data-stu-id="80976-154">One-way, outgoing</span></span> |
| <span data-ttu-id="80976-155">Benutzer in der Cloud benötigen Zugriff auf lokale Ressourcen, aber nicht umgekehrt</span><span class="sxs-lookup"><span data-stu-id="80976-155">Users in the cloud require access to resources located on-premises, but not vice versa</span></span> |<span data-ttu-id="80976-156">Unidirektional, ausgehend</span><span class="sxs-lookup"><span data-stu-id="80976-156">One-way, outgoing</span></span> |<span data-ttu-id="80976-157">Unidirektional, eingehend</span><span class="sxs-lookup"><span data-stu-id="80976-157">One-way, incoming</span></span> |
| <span data-ttu-id="80976-158">Sowohl Benutzer in der Cloud als auch lokale Benutzer benötigen Zugriff auf Ressourcen in der Cloud und auf lokale Ressourcen</span><span class="sxs-lookup"><span data-stu-id="80976-158">Users in the cloud and on-premises both requires access to resources held in the cloud and on-premises</span></span> |<span data-ttu-id="80976-159">Bidirektional, eingehend und ausgehend</span><span class="sxs-lookup"><span data-stu-id="80976-159">Two-way, incoming and outgoing</span></span> |<span data-ttu-id="80976-160">Bidirektional, eingehend und ausgehend</span><span class="sxs-lookup"><span data-stu-id="80976-160">Two-way, incoming and outgoing</span></span> |

## <a name="scalability-considerations"></a><span data-ttu-id="80976-161">Überlegungen zur Skalierbarkeit</span><span class="sxs-lookup"><span data-stu-id="80976-161">Scalability considerations</span></span>

<span data-ttu-id="80976-162">Active Directory ist für Domänencontroller, die Teil derselben Domäne sind, automatisch skalierbar.</span><span class="sxs-lookup"><span data-stu-id="80976-162">Active Directory is automatically scalable for domain controllers that are part of the same domain.</span></span> <span data-ttu-id="80976-163">Anforderungen werden auf alle Controller innerhalb einer Domäne verteilt.</span><span class="sxs-lookup"><span data-stu-id="80976-163">Requests are distributed across all controllers within a domain.</span></span> <span data-ttu-id="80976-164">Sie können einen weiteren Domänencontroller hinzufügen, und er wird automatisch mit der Domäne synchronisiert.</span><span class="sxs-lookup"><span data-stu-id="80976-164">You can add another domain controller, and it synchronizes automatically with the domain.</span></span> <span data-ttu-id="80976-165">Konfigurieren Sie keinen separaten Lastenausgleich, um Datenverkehr an Controller innerhalb der Domäne weiterzuleiten.</span><span class="sxs-lookup"><span data-stu-id="80976-165">Do not configure a separate load balancer to direct traffic to controllers within the domain.</span></span> <span data-ttu-id="80976-166">Stellen Sie sicher, dass alle Domänencontroller über genügend Arbeitsspeicher und Speicherressourcen für den Umgang mit der Domänendatenbank verfügen.</span><span class="sxs-lookup"><span data-stu-id="80976-166">Ensure that all domain controllers have sufficient memory and storage resources to handle the domain database.</span></span> <span data-ttu-id="80976-167">Erstellen Sie alle Domänencontroller als virtuelle Computer derselben Größe.</span><span class="sxs-lookup"><span data-stu-id="80976-167">Make all domain controller VMs the same size.</span></span>

## <a name="availability-considerations"></a><span data-ttu-id="80976-168">Überlegungen zur Verfügbarkeit</span><span class="sxs-lookup"><span data-stu-id="80976-168">Availability considerations</span></span>

<span data-ttu-id="80976-169">Stellen Sie mindestens zwei Domänencontroller für jede Domäne bereit.</span><span class="sxs-lookup"><span data-stu-id="80976-169">Provision at least two domain controllers for each domain.</span></span> <span data-ttu-id="80976-170">Dies ermöglicht eine automatische Replikation zwischen Servern.</span><span class="sxs-lookup"><span data-stu-id="80976-170">This enables automatic replication between servers.</span></span> <span data-ttu-id="80976-171">Erstellen Sie eine Verfügbarkeitsgruppe für die virtuellen Computer, die als Active Directory-Server für die Handhabung der einzelnen Domänen dienen.</span><span class="sxs-lookup"><span data-stu-id="80976-171">Create an availability set for the VMs acting as Active Directory servers handling each domain.</span></span> <span data-ttu-id="80976-172">Es müssen mindestens zwei Server in dieser Verfügbarkeitsgruppe enthalten sein.</span><span class="sxs-lookup"><span data-stu-id="80976-172">Put at least two servers in this availability set.</span></span>

<span data-ttu-id="80976-173">Überlegen Sie auch, ob Sie einen oder mehrere Server in jeder Domäne als [Standby-Betriebsmaster][standby-operations-masters] für den Fall festlegen, dass die Verbindung mit einem Server mit FSMO-Rolle (Flexible Single Master Operation) fehlschlägt.</span><span class="sxs-lookup"><span data-stu-id="80976-173">Also, consider designating one or more servers in each domain as [standby operations masters][standby-operations-masters] in case connectivity to a server acting as a flexible single master operation (FSMO) role fails.</span></span>

## <a name="manageability-considerations"></a><span data-ttu-id="80976-174">Überlegungen zur Verwaltbarkeit</span><span class="sxs-lookup"><span data-stu-id="80976-174">Manageability considerations</span></span>

<span data-ttu-id="80976-175">Informationen zu Überlegungen in Bezug auf Verwaltung und Überwachung finden Sie unter [Erweitern von Active Directory auf Azure][adds-extend-domain].</span><span class="sxs-lookup"><span data-stu-id="80976-175">For information about management and monitoring considerations, see [Extending Active Directory to Azure][adds-extend-domain].</span></span>

<span data-ttu-id="80976-176">Weitere Informationen finden Sie unter [Überwachen von Active Directory][monitoring_ad].</span><span class="sxs-lookup"><span data-stu-id="80976-176">For additional information, see [Monitoring Active Directory][monitoring_ad].</span></span> <span data-ttu-id="80976-177">Sie können Tools wie z.B. [Microsoft Systems Center][microsoft_systems_center] auf einem Überwachungsserver im Verwaltungssubnetz installieren, um diese Aufgaben auszuführen.</span><span class="sxs-lookup"><span data-stu-id="80976-177">You can install tools such as [Microsoft Systems Center][microsoft_systems_center] on a monitoring server in the management subnet to help perform these tasks.</span></span>

## <a name="security-considerations"></a><span data-ttu-id="80976-178">Sicherheitshinweise</span><span class="sxs-lookup"><span data-stu-id="80976-178">Security considerations</span></span>

<span data-ttu-id="80976-179">Vertrauensstellungen auf Gesamtstrukturebene sind transitiv.</span><span class="sxs-lookup"><span data-stu-id="80976-179">Forest level trusts are transitive.</span></span> <span data-ttu-id="80976-180">Wenn Sie eine Vertrauensstellung auf Gesamtstrukturebene zwischen einer lokalen Gesamtstruktur und einer Gesamtstruktur in der Cloud erstellen, wird diese Vertrauensstellung auf weitere neue Domänen ausgeweitet, die in beiden Gesamtstrukturen erstellt werden.</span><span class="sxs-lookup"><span data-stu-id="80976-180">If you establish a forest level trust between an on-premises forest and a forest in the cloud, this trust is extended to other new domains created in either forest.</span></span> <span data-ttu-id="80976-181">Wenn Sie Domänen verwenden, um eine Trennung aus Sicherheitsgründen bereitzustellen, sollten Sie Vertrauensstellungen nur auf Domänenebene erstellen.</span><span class="sxs-lookup"><span data-stu-id="80976-181">If you use domains to provide separation for security purposes, consider creating trusts at the domain level only.</span></span> <span data-ttu-id="80976-182">Vertrauensstellungen auf Domäneneben sind nicht transitiv.</span><span class="sxs-lookup"><span data-stu-id="80976-182">Domain level trusts are non-transitive.</span></span>

<span data-ttu-id="80976-183">Spezifische Überlegungen zur Sicherheit für Active Directory finden Sie im Abschnitt „Sicherheitshinweise“ unter [Erweitern von Active Directory auf Azure][adds-extend-domain].</span><span class="sxs-lookup"><span data-stu-id="80976-183">For Active Directory-specific security considerations, see the security considerations section in [Extending Active Directory to Azure][adds-extend-domain].</span></span>

## <a name="deploy-the-solution"></a><span data-ttu-id="80976-184">Bereitstellen der Lösung</span><span class="sxs-lookup"><span data-stu-id="80976-184">Deploy the solution</span></span>

<span data-ttu-id="80976-185">Eine Bereitstellung für diese Architektur ist auf [GitHub][github] verfügbar.</span><span class="sxs-lookup"><span data-stu-id="80976-185">A deployment for this architecture is available on [GitHub][github].</span></span> <span data-ttu-id="80976-186">Beachten Sie, dass die gesamte Bereitstellung bis zu zwei Stunden dauern kann, was das Erstellen des VPN-Gateways und die Ausführung der Skripts beinhaltet, die AD DS konfigurieren.</span><span class="sxs-lookup"><span data-stu-id="80976-186">Note that the entire deployment can take up to two hours, which includes creating the VPN gateway and running the scripts that configure AD DS.</span></span>

### <a name="prerequisites"></a><span data-ttu-id="80976-187">Voraussetzungen</span><span class="sxs-lookup"><span data-stu-id="80976-187">Prerequisites</span></span>

1. <span data-ttu-id="80976-188">Führen Sie das Klonen, Forken oder Herunterladen der ZIP-Datei für das [GitHub-Repository](https://github.com/mspnp/identity-reference-architectures) durch.</span><span class="sxs-lookup"><span data-stu-id="80976-188">Clone, fork, or download the zip file for the [GitHub repository](https://github.com/mspnp/identity-reference-architectures).</span></span>

2. <span data-ttu-id="80976-189">Installieren Sie [Azure CLI 2.0](/cli/azure/install-azure-cli?view=azure-cli-latest).</span><span class="sxs-lookup"><span data-stu-id="80976-189">Install [Azure CLI 2.0](/cli/azure/install-azure-cli?view=azure-cli-latest).</span></span>

3. <span data-ttu-id="80976-190">Installieren Sie das npm-Paket mit den [Azure Bausteinen](https://github.com/mspnp/template-building-blocks/wiki/Install-Azure-Building-Blocks).</span><span class="sxs-lookup"><span data-stu-id="80976-190">Install the [Azure building blocks](https://github.com/mspnp/template-building-blocks/wiki/Install-Azure-Building-Blocks) npm package.</span></span>

   ```bash
   npm install -g @mspnp/azure-building-blocks
   ```

4. <span data-ttu-id="80976-191">Melden Sie sich über eine Eingabeaufforderung, eine Bash-Eingabeaufforderung oder die PowerShell-Eingabeaufforderung folgendermaßen bei Ihrem Azure-Konto an:</span><span class="sxs-lookup"><span data-stu-id="80976-191">From a command prompt, bash prompt, or PowerShell prompt, sign into your Azure account as follows:</span></span>

   ```bash
   az login
   ```

### <a name="deploy-the-simulated-on-premises-datacenter"></a><span data-ttu-id="80976-192">Bereitstellen des simulierten lokalen Rechenzentrums</span><span class="sxs-lookup"><span data-stu-id="80976-192">Deploy the simulated on-premises datacenter</span></span>

1. <span data-ttu-id="80976-193">Navigieren Sie zum Ordner `identity/adds-forest` des GitHub-Repositorys.</span><span class="sxs-lookup"><span data-stu-id="80976-193">Navigate to the `identity/adds-forest` folder of the GitHub repository.</span></span>

2. <span data-ttu-id="80976-194">Öffnen Sie die Datei `onprem.json` .</span><span class="sxs-lookup"><span data-stu-id="80976-194">Open the `onprem.json` file.</span></span> <span data-ttu-id="80976-195">Suchen Sie nach Instanzen von `adminPassword` und `Password`, und fügen Sie Werte für die Kennwörter hinzu.</span><span class="sxs-lookup"><span data-stu-id="80976-195">Search for instances of `adminPassword` and `Password` and add values for the passwords.</span></span>

3. <span data-ttu-id="80976-196">Führen Sie den folgenden Befehl aus, und warten Sie, bis die Bereitstellung abgeschlossen ist:</span><span class="sxs-lookup"><span data-stu-id="80976-196">Run the following command and wait for the deployment to finish:</span></span>

    ```bash
    azbb -s <subscription_id> -g <resource group> -l <location> -p onprem.json --deploy
    ```

### <a name="deploy-the-azure-vnet"></a><span data-ttu-id="80976-197">Bereitstellen von Azure VNet</span><span class="sxs-lookup"><span data-stu-id="80976-197">Deploy the Azure VNet</span></span>

1. <span data-ttu-id="80976-198">Öffnen Sie die Datei `azure.json` .</span><span class="sxs-lookup"><span data-stu-id="80976-198">Open the `azure.json` file.</span></span> <span data-ttu-id="80976-199">Suchen Sie nach Instanzen von `adminPassword` und `Password`, und fügen Sie Werte für die Kennwörter hinzu.</span><span class="sxs-lookup"><span data-stu-id="80976-199">Search for instances of `adminPassword` and `Password` and add values for the passwords.</span></span>

2. <span data-ttu-id="80976-200">Suchen Sie in der gleichen Datei nach Instanzen von `sharedKey`, und geben Sie gemeinsam verwendete Schlüssel für die VPN-Verbindung ein.</span><span class="sxs-lookup"><span data-stu-id="80976-200">In the same file, search for instances of `sharedKey` and enter shared keys for the VPN connection.</span></span>

    ```json
    "sharedKey": "",
    ```

3. <span data-ttu-id="80976-201">Führen Sie den folgenden Befehl aus, und warten Sie, bis die Bereitstellung abgeschlossen ist.</span><span class="sxs-lookup"><span data-stu-id="80976-201">Run the following command and wait for the deployment to finish.</span></span>

    ```bash
    azbb -s <subscription_id> -g <resource group> -l <location> -p azure.json --deploy
    ```

   <span data-ttu-id="80976-202">Führen Sie die Bereitstellung in der gleichen Ressourcengruppe durch, in der das lokale VNet bereitgestellt ist.</span><span class="sxs-lookup"><span data-stu-id="80976-202">Deploy to the same resource group as the on-premises VNet.</span></span>

### <a name="test-the-ad-trust-relation"></a><span data-ttu-id="80976-203">Testen der AD-Vertrauensbeziehung</span><span class="sxs-lookup"><span data-stu-id="80976-203">Test the AD trust relation</span></span>

1. <span data-ttu-id="80976-204">Navigieren Sie im Azure-Portal zu der Ressourcengruppe, die Sie erstellt haben.</span><span class="sxs-lookup"><span data-stu-id="80976-204">Use the Azure portal, navigate to the resource group that you created.</span></span>

2. <span data-ttu-id="80976-205">Suchen Sie im Azure-Portal die VM `ra-adt-mgmt-vm1`.</span><span class="sxs-lookup"><span data-stu-id="80976-205">Use the Azure portal to find the VM named `ra-adt-mgmt-vm1`.</span></span>

3. <span data-ttu-id="80976-206">Klicke Sie auf `Connect`, um eine Remotedesktopsitzung mit der VM zu öffnen.</span><span class="sxs-lookup"><span data-stu-id="80976-206">Click `Connect` to open a remote desktop session to the VM.</span></span> <span data-ttu-id="80976-207">Der Benutzername ist `contoso\testuser`, und das Kennwort entspricht dem, das Sie in der `onprem.json`-Parameterdatei angegeben haben.</span><span class="sxs-lookup"><span data-stu-id="80976-207">The username is `contoso\testuser`, and the password is the one that you specified in the `onprem.json` parameter file.</span></span>

4. <span data-ttu-id="80976-208">Öffnen Sie von der Remotedesktopsitzung aus eine andere Remotedesktopsitzung mit 192.168.0.4 (IP-Adresse des virtuellen Computers namens `ra-adtrust-onpremise-ad-vm1`).</span><span class="sxs-lookup"><span data-stu-id="80976-208">From inside your remote desktop session, open another remote desktop session to 192.168.0.4, which is the IP address of the VM named `ra-adtrust-onpremise-ad-vm1`.</span></span> <span data-ttu-id="80976-209">Der Benutzername ist `contoso\testuser`, und das Kennwort entspricht dem, das Sie in der `azure.json`-Parameterdatei angegeben haben.</span><span class="sxs-lookup"><span data-stu-id="80976-209">The username is `contoso\testuser`, and the password is the one that you specified in the `azure.json` parameter file.</span></span>

5. <span data-ttu-id="80976-210">Wechseln Sie von der Remotedesktopsitzung für `ra-adtrust-onpremise-ad-vm1` zum **Server-Manager**, und klicken Sie auf **Extras** > **Active Directory-Domänen und -Vertrauensstellungen**.</span><span class="sxs-lookup"><span data-stu-id="80976-210">From inside the remote desktop session for `ra-adtrust-onpremise-ad-vm1`, go to **Server Manager** and click **Tools** > **Active Directory Domains and Trusts**.</span></span>

6. <span data-ttu-id="80976-211">Klicken Sie im linken Bereich mit der rechten Maustaste auf „contoso.com“, und klicken Sie auf **Eigenschaften**.</span><span class="sxs-lookup"><span data-stu-id="80976-211">In the left pane, right-click on the contoso.com and select **Properties**.</span></span>

7. <span data-ttu-id="80976-212">Klicken Sie auf die Registerkarte **Vertrauensstellungen**. Als eingehende Vertrauensstellung sollte „treyresearch.net“ angezeigt werden.</span><span class="sxs-lookup"><span data-stu-id="80976-212">Click the **Trusts** tab. You should see treyresearch.net listed as an incoming trust.</span></span>

![Screenshot des Vertrauensstellungsdialogfelds für die Active Directory-Gesamtstruktur](./images/ad-forest-trust.png)

## <a name="next-steps"></a><span data-ttu-id="80976-214">Nächste Schritte</span><span class="sxs-lookup"><span data-stu-id="80976-214">Next steps</span></span>

- <span data-ttu-id="80976-215">Informieren Sie sich über bewährte Methoden für das [Erweitern Ihrer lokalen AD DS-Domäne auf Azure][adds-extend-domain].</span><span class="sxs-lookup"><span data-stu-id="80976-215">Learn the best practices for [extending your on-premises AD DS domain to Azure][adds-extend-domain]</span></span>
- <span data-ttu-id="80976-216">Informieren Sie sich über bewährte Methoden für das [Erstellen einer AD FS-Infrastruktur][adfs] in Azure.</span><span class="sxs-lookup"><span data-stu-id="80976-216">Learn the best practices for [creating an AD FS infrastructure][adfs] in Azure.</span></span>

<!-- links -->
[adds-extend-domain]: adds-extend-domain.md
[adfs]: adfs.md
[azure-cli-2]: /azure/install-azure-cli
[azbb]: https://github.com/mspnp/template-building-blocks/wiki/Install-Azure-Building-Blocks

[implementing-a-secure-hybrid-network-architecture]: ../dmz/secure-vnet-hybrid.md
[implementing-a-secure-hybrid-network-architecture-with-internet-access]: ../dmz/secure-vnet-dmz.md

[running-VMs-for-an-N-tier-architecture-on-Azure]: ../virtual-machines-windows/n-tier.md

[ad-azure-guidelines]: https://msdn.microsoft.com/library/azure/jj156090.aspx
[azure-expressroute]: /azure/expressroute/expressroute-introduction
[azure-vpn-gateway]: /azure/vpn-gateway/vpn-gateway-about-vpngateways
[considerations]: ./considerations.md
[creating-external-trusts]: https://technet.microsoft.com/library/cc816837(v=ws.10).aspx
[creating-forest-trusts]: https://technet.microsoft.com/library/cc816810(v=ws.10).aspx
[github]: https://github.com/mspnp/identity-reference-architectures/tree/master/adds-forest
[incoming-trust]: https://raw.githubusercontent.com/mspnp/identity-reference-architectures/master/adds-forest/extensions/incoming-trust.ps1
[microsoft_systems_center]: https://microsoft.com/cloud-platform/system-center
[monitoring_ad]: https://msdn.microsoft.com/library/bb727046.aspx
[resource-manager-overview]: /azure/azure-resource-manager/resource-group-overview
[solution-script]: https://raw.githubusercontent.com/mspnp/identity-reference-architectures/master/adds-forest/Deploy-ReferenceArchitecture.ps1
[standby-operations-masters]: https://technet.microsoft.com/library/cc794737(v=ws.10).aspx
[outgoing-trust]: https://raw.githubusercontent.com/mspnp/identity-reference-architectures/master/adds-forest/extensions/outgoing-trust.ps1
[verify-a-trust]: https://technet.microsoft.com/library/cc753821.aspx
[visio-download]: https://archcenter.blob.core.windows.net/cdn/identity-architectures.vsdx
