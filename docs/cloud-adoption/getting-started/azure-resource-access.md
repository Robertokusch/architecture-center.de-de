---
title: 'CAF: Ressourcenzugriffsverwaltung in Azure'
description: 'Erklärung der Konstrukte für die Verwaltung des Ressourcenzugriffs in Azure: Azure Resource Manager, Abonnements, Ressourcengruppen und Ressourcen.'
author: petertaylor9999
ms.date: 2/11/2019
ms.openlocfilehash: f23540a03c82fbc46872645ac0fd82d574db353a
ms.sourcegitcommit: 273e690c0cfabbc3822089c7d8bc743ef41d2b6e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/08/2019
ms.locfileid: "55898118"
---
# <a name="resource-access-management-in-azure"></a><span data-ttu-id="68aa4-103">Ressourcenzugriffsverwaltung in Azure</span><span class="sxs-lookup"><span data-stu-id="68aa4-103">Resource access management in Azure</span></span>

<span data-ttu-id="68aa4-104">In [Was ist die Ressourcenkontrolle?](what-is-governance.md) haben Sie erfahren, dass mit „Kontrolle“ (Governance) das fortlaufende Verwalten, Überwachen und Überprüfen der Nutzung von Azure-Ressourcen gemeint ist, um die Ziele und Anforderungen Ihrer Organisation zu erfüllen.</span><span class="sxs-lookup"><span data-stu-id="68aa4-104">In [what is resource governance?](what-is-governance.md), you learned that governance refers to the ongoing process of managing, monitoring, and auditing the use of Azure resources to meet the goals and requirements of your organization.</span></span> <span data-ttu-id="68aa4-105">Bevor Sie fortfahren und sich darüber informieren, wie Sie ein Modell für die Ressourcenkontrolle entwerfen, sollten Sie sich unbedingt mit den Kontrollen vertraut machen, die in Azure für die Ressourcenzugriffsverwaltung verwendet werden.</span><span class="sxs-lookup"><span data-stu-id="68aa4-105">Before you move on to learn how to design a governance model, it's important to understand the resource access management controls in Azure.</span></span> <span data-ttu-id="68aa4-106">Die Konfiguration dieser Kontrollen für die Ressourcenzugriffsverwaltung ist die Grundlage Ihres Modells für die Ressourcenkontrolle.</span><span class="sxs-lookup"><span data-stu-id="68aa4-106">The configuration of these resource access management controls forms the basis of your governance model.</span></span>

<span data-ttu-id="68aa4-107">Zunächst sehen wir uns erst einmal genauer an, wie Ressourcen in Azure bereitgestellt werden.</span><span class="sxs-lookup"><span data-stu-id="68aa4-107">Let's begin by taking a closer look at how resources are deployed in Azure.</span></span>

<!-- markdownlint-disable MD026 -->

## <a name="what-is-an-azure-resource"></a><span data-ttu-id="68aa4-108">Was ist eine Azure-Ressource?</span><span class="sxs-lookup"><span data-stu-id="68aa4-108">What is an Azure resource?</span></span>

<span data-ttu-id="68aa4-109">In Azure bezieht sich der Begriff **Ressource** auf eine von Azure verwaltete Entität.</span><span class="sxs-lookup"><span data-stu-id="68aa4-109">In Azure, the term **resource** refers to an entity managed by Azure.</span></span> <span data-ttu-id="68aa4-110">Beispielsweise werden virtuelle Computer, virtuelle Netzwerke und Speicherkonten allesamt als Azure-Ressourcen bezeichnet.</span><span class="sxs-lookup"><span data-stu-id="68aa4-110">For example, virtual machines, virtual networks, and storage accounts are all referred to as Azure resources.</span></span>

<span data-ttu-id="68aa4-111">![](../_images/governance-1-9.png)
*Abbildung 1. Ressource*</span><span class="sxs-lookup"><span data-stu-id="68aa4-111">![](../_images/governance-1-9.png)
*Figure 1. A resource.*</span></span>

## <a name="what-is-an-azure-resource-group"></a><span data-ttu-id="68aa4-112">Was ist eine Azure-Ressourcengruppe?</span><span class="sxs-lookup"><span data-stu-id="68aa4-112">What is an Azure resource group?</span></span>

<span data-ttu-id="68aa4-113">In Azure muss jede Ressource zu einer [Ressourcengruppe](/azure/azure-resource-manager/resource-group-overview#resource-groups) gehören.</span><span class="sxs-lookup"><span data-stu-id="68aa4-113">Each resource in Azure must belong to a [resource group](/azure/azure-resource-manager/resource-group-overview#resource-groups).</span></span> <span data-ttu-id="68aa4-114">Eine Ressourcengruppe ist einfach ein logisches Konstrukt, mit dem mehrere Ressourcen gruppiert werden, damit sie zusammen als Entität verwaltet werden können.</span><span class="sxs-lookup"><span data-stu-id="68aa4-114">A resource group is simply a logical construct that groups multiple resources together so they can be managed as a single entity.</span></span> <span data-ttu-id="68aa4-115">Beispielsweise können Ressourcen, die über einen ähnlichen Lebenszyklus verfügen, z.B. die Ressourcen für eine [n-schichtige Anwendung](/azure/architecture/guide/architecture-styles/n-tier), als Gruppe erstellt oder gelöscht werden.</span><span class="sxs-lookup"><span data-stu-id="68aa4-115">For example, resources that share a similar lifecycle, such as the resources for an [n-tier application](/azure/architecture/guide/architecture-styles/n-tier) may be created or deleted as a group.</span></span>

<span data-ttu-id="68aa4-116">![](../_images/governance-1-10.png)
*Abbildung 2. Eine Ressourcengruppe enthält eine Ressource.*</span><span class="sxs-lookup"><span data-stu-id="68aa4-116">![](../_images/governance-1-10.png)
*Figure 2. A resource group contains a resource.*</span></span>

<span data-ttu-id="68aa4-117">Ressourcengruppen und die darin enthaltenen Ressourcen sind einem Azure-**Abonnement** zugeordnet.</span><span class="sxs-lookup"><span data-stu-id="68aa4-117">Resource groups and the resources they contain are associated with an Azure **subscription**.</span></span>

## <a name="what-is-an-azure-subscription"></a><span data-ttu-id="68aa4-118">Was ist ein Azure-Abonnement?</span><span class="sxs-lookup"><span data-stu-id="68aa4-118">What is an Azure subscription?</span></span>

<span data-ttu-id="68aa4-119">Ein Azure-Abonnement ähnelt einer Ressourcengruppe darin, dass es sich um ein logisches Konstrukt handelt, unter dem Ressourcengruppen und die zugehörigen Ressourcen gruppiert werden.</span><span class="sxs-lookup"><span data-stu-id="68aa4-119">An Azure subscription is similar to a resource group in that it's a logical construct that groups together resource groups and their resources.</span></span> <span data-ttu-id="68aa4-120">Darüber hinaus ist ein Azure-Abonnement aber auch noch den Kontrollen zugeordnet, die von Azure Resource Manager genutzt werden.</span><span class="sxs-lookup"><span data-stu-id="68aa4-120">However, an Azure subscription is also associated with the controls used by Azure Resource Manager.</span></span> <span data-ttu-id="68aa4-121">Was bedeutet dies?</span><span class="sxs-lookup"><span data-stu-id="68aa4-121">What does this mean?</span></span> <span data-ttu-id="68aa4-122">Wir sehen uns Resource Manager genauer an, um uns über die Beziehung dieses Diensts zu einem Azure-Abonnement zu informieren.</span><span class="sxs-lookup"><span data-stu-id="68aa4-122">Let's take a closer look at Resource Manager to learn about the relationship between it and an Azure subscription.</span></span>

<span data-ttu-id="68aa4-123">![](../_images/governance-1-11.png)
*Abbildung 3. Ein Azure-Abonnement.*</span><span class="sxs-lookup"><span data-stu-id="68aa4-123">![](../_images/governance-1-11.png)
*Figure 3. An Azure subscription.*</span></span>

## <a name="what-is-azure-resource-manager"></a><span data-ttu-id="68aa4-124">Was ist Azure Resource Manager?</span><span class="sxs-lookup"><span data-stu-id="68aa4-124">What is Azure Resource Manager?</span></span>

<span data-ttu-id="68aa4-125">Unter [Wie funktioniert Azure?](what-is-azure.md) haben Sie erfahren, dass Azure über ein „Front-End“ mit vielen Diensten verfügt, über die alle Funktionen von Azure orchestriert werden.</span><span class="sxs-lookup"><span data-stu-id="68aa4-125">In [how does Azure work?](what-is-azure.md) you learned that Azure includes a "front end" with many services that orchestrate all the functions of Azure.</span></span> <span data-ttu-id="68aa4-126">Einer dieser Dienste ist [Resource Manager](/azure/azure-resource-manager/). Er hostet die RESTful-API, die von Clients zum Verwalten von Ressourcen verwendet wird.</span><span class="sxs-lookup"><span data-stu-id="68aa4-126">One of these services is [Resource Manager](/azure/azure-resource-manager/), and this service hosts the RESTful API used by clients to manage resources.</span></span>

<span data-ttu-id="68aa4-127">![](../_images/governance-1-12.png)
*Abbildung 4. Azure Resource Manager.*</span><span class="sxs-lookup"><span data-stu-id="68aa4-127">![](../_images/governance-1-12.png)
*Figure 4. Azure resource manager.*</span></span>

<span data-ttu-id="68aa4-128">In der folgenden Abbildung sind drei Clients dargestellt: [PowerShell](/powershell/azure/overview), das [Azure-Portal](https://portal.azure.com) und die [Azure-Befehlszeilenschnittstelle (Azure CLI)](/cli/azure):</span><span class="sxs-lookup"><span data-stu-id="68aa4-128">The following figure shows three clients: [PowerShell](/powershell/azure/overview), the [Azure portal](https://portal.azure.com), and the [Azure command line interface (CLI)](/cli/azure):</span></span>

<span data-ttu-id="68aa4-129">![](../_images/governance-1-13.png)
*Abbildung 5. Azure-Clients stellen eine Verbindung mit der RESTful-API von Resource Manager her.*</span><span class="sxs-lookup"><span data-stu-id="68aa4-129">![](../_images/governance-1-13.png)
*Figure 5. Azure clients connect to the Resource Manager RESTful API.*</span></span>

<span data-ttu-id="68aa4-130">Diese Clients stellen zwar über die RESTful-API eine Verbindung mit Resource Manager her, aber Resource Manager umfasst keine Funktionen zum direkten Verwalten von Ressourcen.</span><span class="sxs-lookup"><span data-stu-id="68aa4-130">While these clients connect to Resource Manager using the RESTful API, Resource Manager does not include functionality to manage resources directly.</span></span> <span data-ttu-id="68aa4-131">Stattdessen verfügen die meisten Ressourcentypen in Azure über ihren eigenen [**Ressourcenanbieter**](/azure/azure-resource-manager/resource-group-overview#terminology).</span><span class="sxs-lookup"><span data-stu-id="68aa4-131">Rather, most resource types in Azure have their own [**resource provider**](/azure/azure-resource-manager/resource-group-overview#terminology).</span></span>

<span data-ttu-id="68aa4-132">\*
*Abbildung 6: Azure-Ressourcenanbieter*</span><span class="sxs-lookup"><span data-stu-id="68aa4-132">![](../_images/governance-1-14.png)
*Figure 6. Azure resource providers.*</span></span>

<span data-ttu-id="68aa4-133">Wenn ein Client eine Anforderung zur Verwaltung einer bestimmten Ressource sendet, stellt Resource Manager eine Verbindung mit dem Ressourcenanbieter für diesen Ressourcentyp her, damit die Anforderung abgeschlossen werden kann.</span><span class="sxs-lookup"><span data-stu-id="68aa4-133">When a client makes a request to manage a specific resource, Resource Manager connects to the resource provider for that resource type to complete the request.</span></span> <span data-ttu-id="68aa4-134">Wenn ein Client beispielsweise eine Anforderung zur Verwaltung einer VM-Ressource sendet, stellt Resource Manager eine Verbindung mit dem Ressourcenanbieter **Microsoft.Compute** her.</span><span class="sxs-lookup"><span data-stu-id="68aa4-134">For example, if a client makes a request to manage a virtual machine resource, Resource Manager connects to the **Microsoft.Compute** resource provider.</span></span>

<span data-ttu-id="68aa4-135">![](../_images/governance-1-15.png)
*Abbildung 7: Resource Manager stellt eine Verbindung mit dem Ressourcenanbieter **Microsoft.Compute** her, um die in der Clientanforderung angegebene Ressource zu verwalten.*</span><span class="sxs-lookup"><span data-stu-id="68aa4-135">![](../_images/governance-1-15.png)
*Figure 7. Resource Manager connects to the **Microsoft.Compute** resource provider to manage the resource specified in the client request.*</span></span>

<span data-ttu-id="68aa4-136">Resource Manager setzt voraus, dass der Client einen Bezeichner für das Abonnement sowie für die Ressourcengruppe angibt, damit die VM-Ressource verwaltet werden kann.</span><span class="sxs-lookup"><span data-stu-id="68aa4-136">Resource Manager requires the client to specify an identifier for both the subscription and the resource group in order to manage the virtual machine resource.</span></span>

<span data-ttu-id="68aa4-137">Nachdem Sie sich mit der Funktionsweise von Resource Manager vertraut gemacht haben, kehren wir zu der Beschreibung zurück, wie ein Azure-Abonnement den von Azure Resource Manager verwendeten Kontrollen zugeordnet ist.</span><span class="sxs-lookup"><span data-stu-id="68aa4-137">Now that you have an understanding of how Resource Manager works, let's return to our discussion of how an Azure subscription is associated with the controls used by Azure resource manager.</span></span> <span data-ttu-id="68aa4-138">Bevor eine Anforderung zur Durchführung der Ressourcenverwaltung von Resource Manager ausgeführt werden kann, werden einige Kontrollmechanismen überprüft.</span><span class="sxs-lookup"><span data-stu-id="68aa4-138">Before any resource management request can be executed by Resource Manager, a set of controls are checked.</span></span>

<span data-ttu-id="68aa4-139">Die erste Kontrolle besteht darin, dass eine Anforderung von einem geprüften Benutzer durchgeführt werden muss und Resource Manager über eine vertrauenswürdige Beziehung mit [Azure Active Directory (Azure AD)](/azure/active-directory/) verfügt, um die Funktionen für die Benutzeridentität bereitstellen zu können.</span><span class="sxs-lookup"><span data-stu-id="68aa4-139">The first control is that a request must be made by a validated user, and Resource Manager has a trusted relationship with [Azure Active Directory](/azure/active-directory/) (Azure AD) to provide user identity functionality.</span></span>

<span data-ttu-id="68aa4-140">![](../_images/governance-1-16.png)
*Abbildung 8. Azure Active Directory.*</span><span class="sxs-lookup"><span data-stu-id="68aa4-140">![](../_images/governance-1-16.png)
*Figure 8. Azure Active Directory.*</span></span>

<span data-ttu-id="68aa4-141">In Azure AD werden Benutzer in **Mandanten** segmentiert.</span><span class="sxs-lookup"><span data-stu-id="68aa4-141">In Azure AD, users are segmented into **tenants**.</span></span> <span data-ttu-id="68aa4-142">Ein Mandant ist ein logisches Konstrukt, das eine sichere, dedizierte Instanz von Azure AD darstellt, die in der Regel einer Organisation zugeordnet ist.</span><span class="sxs-lookup"><span data-stu-id="68aa4-142">A tenant is a logical construct that represents a secure, dedicated instance of Azure AD typically associated with an organization.</span></span> <span data-ttu-id="68aa4-143">Jedes Abonnement ist einem Azure AD-Mandanten zugeordnet.</span><span class="sxs-lookup"><span data-stu-id="68aa4-143">Each subscription is associated with an Azure AD tenant.</span></span>

<span data-ttu-id="68aa4-144">![](../_images/governance-1-17.png)
*Abbildung 9. Ein Azure AD-Mandant, der einem Abonnement zugeordnet ist.*</span><span class="sxs-lookup"><span data-stu-id="68aa4-144">![](../_images/governance-1-17.png)
*Figure 9. An Azure AD tenant associated with a subscription.*</span></span>

<span data-ttu-id="68aa4-145">Für jede Clientanforderung zur Verwaltung einer Ressource unter einem bestimmten Abonnement ist es erforderlich, dass der Benutzer im zugeordneten Azure AD-Mandanten über ein Konto verfügt.</span><span class="sxs-lookup"><span data-stu-id="68aa4-145">Each client request to manage a resource in a particular subscription requires that the user has an account in the associated Azure AD tenant.</span></span>

<span data-ttu-id="68aa4-146">Die nächste Kontrolle ist eine Überprüfung, ob der Benutzer über ausreichende Berechtigungen zum Senden der Anforderungen verfügt.</span><span class="sxs-lookup"><span data-stu-id="68aa4-146">The next control is a check that the user has sufficient permission to make the request.</span></span> <span data-ttu-id="68aa4-147">Berechtigungen werden Benutzern über die [rollenbasierte Zugriffssteuerung (RBAC)](/azure/role-based-access-control/) zugewiesen.</span><span class="sxs-lookup"><span data-stu-id="68aa4-147">Permissions are assigned to users using [role-based access control (RBAC)](/azure/role-based-access-control/).</span></span>

<span data-ttu-id="68aa4-148">![](../_images/governance-1-18.png)
*Abbildung 10. Jedem Benutzer im Mandanten wird mindestens eine RBAC-Rolle zugewiesen.*</span><span class="sxs-lookup"><span data-stu-id="68aa4-148">![](../_images/governance-1-18.png)
*Figure 10. Each user in the tenant is assigned one or more RBAC roles.*</span></span>

<span data-ttu-id="68aa4-149">Mit einer RBAC-Rolle wird ein Satz mit Berechtigungen angegeben, die einem Benutzer für eine bestimmte Ressource zur Verfügung stehen.</span><span class="sxs-lookup"><span data-stu-id="68aa4-149">An RBAC role specifies a set of permissions a user may take on a specific resource.</span></span> <span data-ttu-id="68aa4-150">Wenn die Rolle dem Benutzer zugewiesen wird, werden diese Berechtigungen angewendet.</span><span class="sxs-lookup"><span data-stu-id="68aa4-150">When the role is assigned to the user, those permissions are applied.</span></span> <span data-ttu-id="68aa4-151">Mit der [integrierten Rolle**Besitzer**](/azure/role-based-access-control/built-in-roles#owner) kann ein Benutzer für eine Ressource beispielsweise alle Aktionen durchführen.</span><span class="sxs-lookup"><span data-stu-id="68aa4-151">For example, the [built-in **owner** role](/azure/role-based-access-control/built-in-roles#owner) allows a user to perform any action on a resource.</span></span>

<span data-ttu-id="68aa4-152">Die nächste Kontrolle umfasst eine Überprüfung, ob die Anforderung gemäß den Einstellungen, die für die [Azure-Ressourcenrichtlinie](/azure/governance/policy/) angegeben wurden, zulässig ist.</span><span class="sxs-lookup"><span data-stu-id="68aa4-152">The next control is a check that the request is allowed under the settings specified for [Azure resource policy](/azure/governance/policy/).</span></span> <span data-ttu-id="68aa4-153">Mit Azure-Ressourcenrichtlinien werden die Vorgänge angegeben, die für eine bestimmte Ressource zulässig sind.</span><span class="sxs-lookup"><span data-stu-id="68aa4-153">Azure resource policies specify the operations allowed for a specific resource.</span></span> <span data-ttu-id="68aa4-154">Mithilfe einer Azure-Ressourcenrichtlinie kann beispielsweise angegeben werden, dass Benutzer nur einen bestimmten Typ eines virtuellen Computers bereitstellen dürfen.</span><span class="sxs-lookup"><span data-stu-id="68aa4-154">For example, an Azure resource policy can specify that users are only allowed to deploy a specific type of virtual machine.</span></span>

<span data-ttu-id="68aa4-155">![](../_images/governance-1-19.png)
*Abbildung 11. Azure-Ressourcenrichtlinie*</span><span class="sxs-lookup"><span data-stu-id="68aa4-155">![](../_images/governance-1-19.png)
*Figure 11. Azure resource policy.*</span></span>

<span data-ttu-id="68aa4-156">Mit der nächsten Kontrolle wird sichergestellt, dass für die Anforderung kein [Grenzwert für das Azure-Abonnement](/azure/azure-subscription-service-limits) überschritten wird.</span><span class="sxs-lookup"><span data-stu-id="68aa4-156">The next control is a check that the request does not exceed an [Azure subscription limit](/azure/azure-subscription-service-limits).</span></span> <span data-ttu-id="68aa4-157">Beispielsweise gilt für alle Abonnements ein Grenzwert von 980 Ressourcengruppen pro Abonnement.</span><span class="sxs-lookup"><span data-stu-id="68aa4-157">For example, each subscription has a limit of 980 resource groups per subscription.</span></span> <span data-ttu-id="68aa4-158">Wenn eine Anforderung zur Bereitstellung einer weiteren Ressourcengruppe eingeht, wird dies verweigert, falls der Grenzwert bereits erreicht ist.</span><span class="sxs-lookup"><span data-stu-id="68aa4-158">If a request is received to deploy an additional resource group once the limit has been reached, it is denied.</span></span>

<span data-ttu-id="68aa4-159">![](../_images/governance-1-20.png)
*Abbildung 12. Azure-Ressourcengrenzwerte*</span><span class="sxs-lookup"><span data-stu-id="68aa4-159">![](../_images/governance-1-20.png)
*Figure 12. Azure resource limits.*</span></span>

<span data-ttu-id="68aa4-160">Die letzte Kontrolle umfasst eine Überprüfung, ob sich die Anforderung innerhalb der Zahlungsverpflichtung des Abonnements bewegt.</span><span class="sxs-lookup"><span data-stu-id="68aa4-160">The final control is a check that the request is within the financial commitment associated with the subscription.</span></span> <span data-ttu-id="68aa4-161">Wenn es bei der Anforderung beispielsweise um die Bereitstellung eines virtuellen Computers geht, überprüft Resource Manager, ob das Abonnement über ausreichende Zahlungsinformationen verfügt.</span><span class="sxs-lookup"><span data-stu-id="68aa4-161">For example, if the request is to deploy a virtual machine, Resource Manager verifies that the subscription has sufficient payment information.</span></span>

<span data-ttu-id="68aa4-162">![](../_images/governance-1-21.png)
*Abbildung 13. Einem Abonnement ist eine Zahlungsverpflichtung zugeordnet.*</span><span class="sxs-lookup"><span data-stu-id="68aa4-162">![](../_images/governance-1-21.png)
*Figure 13. A financial commitment is associated with a subscription.*</span></span>

## <a name="summary"></a><span data-ttu-id="68aa4-163">Zusammenfassung</span><span class="sxs-lookup"><span data-stu-id="68aa4-163">Summary</span></span>

<span data-ttu-id="68aa4-164">In diesem Artikel wurde beschrieben, wie der Zugriff auf Ressourcen in Azure mit Azure Resource Manager verwaltet wird.</span><span class="sxs-lookup"><span data-stu-id="68aa4-164">In this article, you learned about how resource access is managed in Azure using Azure Resource Manager.</span></span>

## <a name="next-steps"></a><span data-ttu-id="68aa4-165">Nächste Schritte</span><span class="sxs-lookup"><span data-stu-id="68aa4-165">Next steps</span></span>

<span data-ttu-id="68aa4-166">Nachdem Sie nun wissen, wie der Ressourcenzugriff in Azure verwaltet wird, können Sie sich darüber informieren, wie Sie ein Governancemodell entwerfen.</span><span class="sxs-lookup"><span data-stu-id="68aa4-166">Now that you understand how resource access is managed in Azure, move on to learn how to design a governance model.</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="68aa4-167">Cloud Governance</span><span class="sxs-lookup"><span data-stu-id="68aa4-167">Cloud governance</span></span>](../governance/overview.md)
