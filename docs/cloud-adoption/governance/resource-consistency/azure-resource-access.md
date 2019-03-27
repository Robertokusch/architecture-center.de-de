---
title: 'Framework für die Cloudeinführung (Cloud Adoption Framework, CAF): Ressourcenzugriffsverwaltung in Azure'
titleSuffix: Microsoft Cloud Adoption Framework for Azure
ms.service: architecture-center
ms.subservice: enterprise-cloud-adoption
ms.custom: governance
ms.date: 02/11/2019
description: 'Erläuterung der Konstrukte für die Verwaltung des Ressourcenzugriffs in Azure: Azure Resource Manager, Abonnements, Ressourcengruppen und Ressourcen'
author: petertaylor9999
ms.openlocfilehash: b98cdc94d6d3a37c1e65da1d4de35d5d9520d6eb
ms.sourcegitcommit: c053e6edb429299a0ad9b327888d596c48859d4a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/20/2019
ms.locfileid: "58243201"
---
# <a name="resource-access-management-in-azure"></a><span data-ttu-id="87d33-103">Ressourcenzugriffsverwaltung in Azure</span><span class="sxs-lookup"><span data-stu-id="87d33-103">Resource access management in Azure</span></span>

<span data-ttu-id="87d33-104">[Cloudgovernance](../overview.md) umreißt die fünf Disziplinen der Cloudgovernance, darunter die Verwaltung von Ressourcen.</span><span class="sxs-lookup"><span data-stu-id="87d33-104">[Cloud Governance](../overview.md) outlines the five disciplines of Cloud Governance, which includes Resource Management.</span></span>  <span data-ttu-id="87d33-105">In den Ausführungen zur [Ressourcenzugriffsgovernance](overview.md) wird erläutert, wie sich die Ressourcenzugriffsverwaltung in die Disziplin der Ressourcenverwaltung eingliedert.</span><span class="sxs-lookup"><span data-stu-id="87d33-105">[What is resource access governance](overview.md) furthers explains how resource access management fits into the resource management discipline.</span></span> <span data-ttu-id="87d33-106">Bevor Sie fortfahren und sich darüber informieren, wie Sie ein Modell für die Ressourcenkontrolle entwerfen, sollten Sie sich unbedingt mit den Kontrollen vertraut machen, die in Azure für die Ressourcenzugriffsverwaltung verwendet werden.</span><span class="sxs-lookup"><span data-stu-id="87d33-106">Before you move on to learn how to design a governance model, it's important to understand the resource access management controls in Azure.</span></span> <span data-ttu-id="87d33-107">Die Konfiguration dieser Kontrollen für die Ressourcenzugriffsverwaltung ist die Grundlage Ihres Modells für die Ressourcenkontrolle.</span><span class="sxs-lookup"><span data-stu-id="87d33-107">The configuration of these resource access management controls forms the basis of your governance model.</span></span>

<span data-ttu-id="87d33-108">Sehen Sie sich erst einmal genauer an, wie Ressourcen in Azure bereitgestellt werden.</span><span class="sxs-lookup"><span data-stu-id="87d33-108">Begin by taking a closer look at how resources are deployed in Azure.</span></span>

<!-- markdownlint-disable MD026 -->

## <a name="what-is-an-azure-resource"></a><span data-ttu-id="87d33-109">Was ist eine Azure-Ressource?</span><span class="sxs-lookup"><span data-stu-id="87d33-109">What is an Azure resource?</span></span>

<span data-ttu-id="87d33-110">In Azure bezieht sich der Begriff **Ressource** auf eine von Azure verwaltete Entität.</span><span class="sxs-lookup"><span data-stu-id="87d33-110">In Azure, the term **resource** refers to an entity managed by Azure.</span></span> <span data-ttu-id="87d33-111">Beispielsweise werden virtuelle Computer, virtuelle Netzwerke und Speicherkonten allesamt als Azure-Ressourcen bezeichnet.</span><span class="sxs-lookup"><span data-stu-id="87d33-111">For example, virtual machines, virtual networks, and storage accounts are all referred to as Azure resources.</span></span>

<span data-ttu-id="87d33-112">![Diagramm einer Ressource](../../_images/governance-1-9.png)
*Abbildung 1. Ressource*</span><span class="sxs-lookup"><span data-stu-id="87d33-112">![Diagram of a resource](../../_images/governance-1-9.png)
*Figure 1. A resource.*</span></span>

## <a name="what-is-an-azure-resource-group"></a><span data-ttu-id="87d33-113">Was ist eine Azure-Ressourcengruppe?</span><span class="sxs-lookup"><span data-stu-id="87d33-113">What is an Azure resource group?</span></span>

<span data-ttu-id="87d33-114">In Azure muss jede Ressource zu einer [Ressourcengruppe](/azure/azure-resource-manager/resource-group-overview#resource-groups) gehören.</span><span class="sxs-lookup"><span data-stu-id="87d33-114">Each resource in Azure must belong to a [resource group](/azure/azure-resource-manager/resource-group-overview#resource-groups).</span></span> <span data-ttu-id="87d33-115">Eine Ressourcengruppe ist einfach ein logisches Konstrukt, mit dem mehrere Ressourcen gruppiert werden, damit sie zusammen als Entität verwaltet werden können.</span><span class="sxs-lookup"><span data-stu-id="87d33-115">A resource group is simply a logical construct that groups multiple resources together so they can be managed as a single entity.</span></span> <span data-ttu-id="87d33-116">Beispielsweise können Ressourcen, die über einen ähnlichen Lebenszyklus verfügen, z.B. die Ressourcen für eine [n-schichtige Anwendung](/azure/architecture/guide/architecture-styles/n-tier), als Gruppe erstellt oder gelöscht werden.</span><span class="sxs-lookup"><span data-stu-id="87d33-116">For example, resources that share a similar lifecycle, such as the resources for an [n-tier application](/azure/architecture/guide/architecture-styles/n-tier) may be created or deleted as a group.</span></span>

<span data-ttu-id="87d33-117">![Diagramm einer Ressourcengruppe, die eine Ressource enthält](../../_images/governance-1-10.png)
*Abbildung 2. Eine Ressourcengruppe enthält eine Ressource.*</span><span class="sxs-lookup"><span data-stu-id="87d33-117">![Diagram of a resource group containing a resource](../../_images/governance-1-10.png)
*Figure 2. A resource group contains a resource.*</span></span>

<span data-ttu-id="87d33-118">Ressourcengruppen und die darin enthaltenen Ressourcen sind einem Azure-**Abonnement** zugeordnet.</span><span class="sxs-lookup"><span data-stu-id="87d33-118">Resource groups and the resources they contain are associated with an Azure **subscription**.</span></span>

## <a name="what-is-an-azure-subscription"></a><span data-ttu-id="87d33-119">Was ist ein Azure-Abonnement?</span><span class="sxs-lookup"><span data-stu-id="87d33-119">What is an Azure subscription?</span></span>

<span data-ttu-id="87d33-120">Ein Azure-Abonnement ähnelt einer Ressourcengruppe darin, dass es sich um ein logisches Konstrukt handelt, unter dem Ressourcengruppen und die zugehörigen Ressourcen gruppiert werden.</span><span class="sxs-lookup"><span data-stu-id="87d33-120">An Azure subscription is similar to a resource group in that it's a logical construct that groups together resource groups and their resources.</span></span> <span data-ttu-id="87d33-121">Darüber hinaus ist ein Azure-Abonnement aber auch noch den Kontrollen zugeordnet, die von Azure Resource Manager genutzt werden.</span><span class="sxs-lookup"><span data-stu-id="87d33-121">However, an Azure subscription is also associated with the controls used by Azure Resource Manager.</span></span> <span data-ttu-id="87d33-122">Was bedeutet dies?</span><span class="sxs-lookup"><span data-stu-id="87d33-122">What does this mean?</span></span> <span data-ttu-id="87d33-123">Sehen Sie sich Azure Resource Manager genauer an, um sich über die Beziehung dieses Diensts zu einem Azure-Abonnement zu informieren.</span><span class="sxs-lookup"><span data-stu-id="87d33-123">Take a closer look at Azure Resource Manager to learn about the relationship between it and an Azure subscription.</span></span>

<span data-ttu-id="87d33-124">![Diagramm eines Azure-Abonnements](../../_images/governance-1-11.png)
*Abbildung 3. Ein Azure-Abonnement.*</span><span class="sxs-lookup"><span data-stu-id="87d33-124">![Diagram of an Azure subscription](../../_images/governance-1-11.png)
*Figure 3. An Azure subscription.*</span></span>

## <a name="what-is-azure-resource-manager"></a><span data-ttu-id="87d33-125">Was ist Azure Resource Manager?</span><span class="sxs-lookup"><span data-stu-id="87d33-125">What is Azure Resource Manager?</span></span>

<span data-ttu-id="87d33-126">Unter [Wie funktioniert Azure?](../../getting-started/what-is-azure.md) haben Sie erfahren, dass Azure über ein „Front-End“ mit vielen Diensten verfügt, über die alle Funktionen von Azure orchestriert werden.</span><span class="sxs-lookup"><span data-stu-id="87d33-126">In [how does Azure work?](../../getting-started/what-is-azure.md) you learned that Azure includes a "front end" with many services that orchestrate all the functions of Azure.</span></span> <span data-ttu-id="87d33-127">Einer dieser Dienste ist der [Azure Resource Manager](/azure/azure-resource-manager/). Er hostet die RESTful-API, die von Clients zum Verwalten von Ressourcen verwendet wird.</span><span class="sxs-lookup"><span data-stu-id="87d33-127">One of these services is [Azure Resource Manager](/azure/azure-resource-manager/), and this service hosts the RESTful API used by clients to manage resources.</span></span>

<span data-ttu-id="87d33-128">![Diagramm des Azure Resource Managers](../../_images/governance-1-12.png)
*Abbildung 4. Azure Resource Manager.*</span><span class="sxs-lookup"><span data-stu-id="87d33-128">![Diagram of Azure Resource Manager](../../_images/governance-1-12.png)
*Figure 4. Azure Resource Manager.*</span></span>

<span data-ttu-id="87d33-129">In der folgenden Abbildung sind drei Clients dargestellt: [PowerShell](/powershell/azure/overview), das [Azure-Portal](https://portal.azure.com) und die [Azure-Befehlszeilenschnittstelle (CLI)](/cli/azure):</span><span class="sxs-lookup"><span data-stu-id="87d33-129">The following figure shows three clients: [PowerShell](/powershell/azure/overview), the [Azure portal](https://portal.azure.com), and the [Azure command-line interface (CLI)](/cli/azure):</span></span>

<span data-ttu-id="87d33-130">![Diagramm von Azure-Clients, die eine Verbindung mit der Azure Resource Manager-API herstellen](../../_images/governance-1-13.png)
*Abbildung 5. Azure-Clients stellen eine Verbindung mit der RESTful-API von Azure Resource Manager her.*</span><span class="sxs-lookup"><span data-stu-id="87d33-130">![Diagram of Azure clients connecting to the Azure Resource Manager API](../../_images/governance-1-13.png)
*Figure 5. Azure clients connect to the Azure Resource Manager RESTful API.*</span></span>

<span data-ttu-id="87d33-131">Diese Clients stellen zwar über die RESTful-API eine Verbindung mit Azure Resource Manager her, aber Azure Resource Manager umfasst keine Funktionen zum direkten Verwalten von Ressourcen.</span><span class="sxs-lookup"><span data-stu-id="87d33-131">While these clients connect to Azure Resource Manager using the RESTful API, Azure Resource Manager does not include functionality to manage resources directly.</span></span> <span data-ttu-id="87d33-132">Stattdessen verfügen die meisten Ressourcentypen in Azure über ihren eigenen [**Ressourcenanbieter**](/azure/azure-resource-manager/resource-group-overview#terminology).</span><span class="sxs-lookup"><span data-stu-id="87d33-132">Rather, most resource types in Azure have their own [**resource provider**](/azure/azure-resource-manager/resource-group-overview#terminology).</span></span>

<span data-ttu-id="87d33-133">![Azure-Ressourcenanbieter](../../_images/governance-1-14.png)
*Abbildung 6. Azure-Ressourcenanbieter*</span><span class="sxs-lookup"><span data-stu-id="87d33-133">![Azure resource providers](../../_images/governance-1-14.png)
*Figure 6. Azure resource providers.*</span></span>

<span data-ttu-id="87d33-134">Wenn ein Client eine Anforderung zur Verwaltung einer bestimmten Ressource sendet, stellt Azure Resource Manager eine Verbindung mit dem Ressourcenanbieter für diesen Ressourcentyp her, damit die Anforderung abgeschlossen werden kann.</span><span class="sxs-lookup"><span data-stu-id="87d33-134">When a client makes a request to manage a specific resource, Azure Resource Manager connects to the resource provider for that resource type to complete the request.</span></span> <span data-ttu-id="87d33-135">Wenn ein Client beispielsweise eine Anforderung zur Verwaltung einer VM-Ressource sendet, stellt Azure Resource Manager eine Verbindung mit dem Ressourcenanbieter **Microsoft.compute** her.</span><span class="sxs-lookup"><span data-stu-id="87d33-135">For example, if a client makes a request to manage a virtual machine resource, Azure Resource Manager connects to the **Microsoft.Compute** resource provider.</span></span>

<span data-ttu-id="87d33-136">![Azure Resource Manager stellt eine Verbindung mit dem Ressourcenanbieter „Microsoft.Compute“ her.](../../_images/governance-1-15.png)
*Abbildung 7. Azure Resource Manager stellt eine Verbindung mit dem Ressourcenanbieter **Microsoft.Compute** her, um die in der Clientanforderung angegebene Ressource zu verwalten.*</span><span class="sxs-lookup"><span data-stu-id="87d33-136">![Azure Resource Manager connecting to the Microsoft.Compute resource provider](../../_images/governance-1-15.png)
*Figure 7. Azure Resource Manager connects to the **Microsoft.Compute** resource provider to manage the resource specified in the client request.*</span></span>

<span data-ttu-id="87d33-137">Azure Resource Manager setzt voraus, dass der Client einen Bezeichner für das Abonnement sowie für die Ressourcengruppe angibt, damit die VM-Ressource verwaltet werden kann.</span><span class="sxs-lookup"><span data-stu-id="87d33-137">Azure Resource Manager requires the client to specify an identifier for both the subscription and the resource group in order to manage the virtual machine resource.</span></span>

<span data-ttu-id="87d33-138">Nachdem Sie sich mit der Funktionsweise von Azure Resource Manager vertraut gemacht haben, kehren Sie zu der Beschreibung zurück, wie ein Azure-Abonnement den von Azure Resource Manager verwendeten Kontrollen zugeordnet ist.</span><span class="sxs-lookup"><span data-stu-id="87d33-138">Now that you have an understanding of how Azure Resource Manager works, return to the discussion of how an Azure subscription is associated with the controls used by Azure Resource Manager.</span></span> <span data-ttu-id="87d33-139">Bevor eine Anforderung zur Durchführung der Ressourcenverwaltung von Azure Resource Manager ausgeführt werden kann, werden einige Kontrollmechanismen überprüft.</span><span class="sxs-lookup"><span data-stu-id="87d33-139">Before any resource management request can be executed by Azure Resource Manager, a set of controls are checked.</span></span>

<span data-ttu-id="87d33-140">Die erste Kontrolle besteht darin, dass eine Anforderung von einem geprüften Benutzer durchgeführt werden muss und Azure Resource Manager über eine vertrauenswürdige Beziehung mit [Azure Active Directory (Azure AD)](/azure/active-directory/) verfügt, um die Funktionen für die Benutzeridentität bereitstellen zu können.</span><span class="sxs-lookup"><span data-stu-id="87d33-140">The first control is that a request must be made by a validated user, and Azure Resource Manager has a trusted relationship with [Azure Active Directory (Azure AD)](/azure/active-directory/) to provide user identity functionality.</span></span>

<span data-ttu-id="87d33-141">![Azure Active Directory](../../_images/governance-1-16.png)
*Abbildung 8. Azure Active Directory.*</span><span class="sxs-lookup"><span data-stu-id="87d33-141">![Azure Active Directory](../../_images/governance-1-16.png)
*Figure 8. Azure Active Directory.*</span></span>

<span data-ttu-id="87d33-142">In Azure AD werden Benutzer in **Mandanten** segmentiert.</span><span class="sxs-lookup"><span data-stu-id="87d33-142">In Azure AD, users are segmented into **tenants**.</span></span> <span data-ttu-id="87d33-143">Ein Mandant ist ein logisches Konstrukt, das eine sichere, dedizierte Instanz von Azure AD darstellt, die in der Regel einer Organisation zugeordnet ist.</span><span class="sxs-lookup"><span data-stu-id="87d33-143">A tenant is a logical construct that represents a secure, dedicated instance of Azure AD typically associated with an organization.</span></span> <span data-ttu-id="87d33-144">Jedes Abonnement ist einem Azure AD-Mandanten zugeordnet.</span><span class="sxs-lookup"><span data-stu-id="87d33-144">Each subscription is associated with an Azure AD tenant.</span></span>

<span data-ttu-id="87d33-145">![Ein Azure AD-Mandant, der einem Abonnement zugeordnet ist](../../_images/governance-1-17.png)
*Abbildung 9. Ein Azure AD-Mandant, der einem Abonnement zugeordnet ist.*</span><span class="sxs-lookup"><span data-stu-id="87d33-145">![An Azure AD tenant associated with a subscription](../../_images/governance-1-17.png)
*Figure 9. An Azure AD tenant associated with a subscription.*</span></span>

<span data-ttu-id="87d33-146">Für jede Clientanforderung zur Verwaltung einer Ressource unter einem bestimmten Abonnement ist es erforderlich, dass der Benutzer im zugeordneten Azure AD-Mandanten über ein Konto verfügt.</span><span class="sxs-lookup"><span data-stu-id="87d33-146">Each client request to manage a resource in a particular subscription requires that the user has an account in the associated Azure AD tenant.</span></span>

<span data-ttu-id="87d33-147">Die nächste Kontrolle ist eine Überprüfung, ob der Benutzer über ausreichende Berechtigungen zum Senden der Anforderungen verfügt.</span><span class="sxs-lookup"><span data-stu-id="87d33-147">The next control is a check that the user has sufficient permission to make the request.</span></span> <span data-ttu-id="87d33-148">Berechtigungen werden Benutzern über die [rollenbasierte Zugriffssteuerung (RBAC)](/azure/role-based-access-control/) zugewiesen.</span><span class="sxs-lookup"><span data-stu-id="87d33-148">Permissions are assigned to users using [role-based access control (RBAC)](/azure/role-based-access-control/).</span></span>

<span data-ttu-id="87d33-149">![Benutzer, die RBAC-Rollen zugewiesen sind](../../_images/governance-1-18.png)
*Abbildung 10. Jedem Benutzer im Mandanten wird mindestens eine RBAC-Rolle zugewiesen.*</span><span class="sxs-lookup"><span data-stu-id="87d33-149">![Users assigned to RBAC roles](../../_images/governance-1-18.png)
*Figure 10. Each user in the tenant is assigned one or more RBAC roles.*</span></span>

<span data-ttu-id="87d33-150">Mit einer RBAC-Rolle wird ein Satz mit Berechtigungen angegeben, die einem Benutzer für eine bestimmte Ressource zur Verfügung stehen.</span><span class="sxs-lookup"><span data-stu-id="87d33-150">An RBAC role specifies a set of permissions a user may take on a specific resource.</span></span> <span data-ttu-id="87d33-151">Wenn die Rolle dem Benutzer zugewiesen wird, werden diese Berechtigungen angewendet.</span><span class="sxs-lookup"><span data-stu-id="87d33-151">When the role is assigned to the user, those permissions are applied.</span></span> <span data-ttu-id="87d33-152">Mit der [integrierten Rolle**Besitzer**](/azure/role-based-access-control/built-in-roles#owner) kann ein Benutzer für eine Ressource beispielsweise alle Aktionen durchführen.</span><span class="sxs-lookup"><span data-stu-id="87d33-152">For example, the [built-in **owner** role](/azure/role-based-access-control/built-in-roles#owner) allows a user to perform any action on a resource.</span></span>

<span data-ttu-id="87d33-153">Die nächste Kontrolle umfasst eine Überprüfung, ob die Anforderung gemäß den Einstellungen, die für die [Azure-Ressourcenrichtlinie](/azure/governance/policy/) angegeben wurden, zulässig ist.</span><span class="sxs-lookup"><span data-stu-id="87d33-153">The next control is a check that the request is allowed under the settings specified for [Azure resource policy](/azure/governance/policy/).</span></span> <span data-ttu-id="87d33-154">Mit Azure-Ressourcenrichtlinien werden die Vorgänge angegeben, die für eine bestimmte Ressource zulässig sind.</span><span class="sxs-lookup"><span data-stu-id="87d33-154">Azure resource policies specify the operations allowed for a specific resource.</span></span> <span data-ttu-id="87d33-155">Mithilfe einer Azure-Ressourcenrichtlinie kann beispielsweise angegeben werden, dass Benutzer nur einen bestimmten Typ eines virtuellen Computers bereitstellen dürfen.</span><span class="sxs-lookup"><span data-stu-id="87d33-155">For example, an Azure resource policy can specify that users are only allowed to deploy a specific type of virtual machine.</span></span>

<span data-ttu-id="87d33-156">![Azure-Ressourcenrichtlinie](../../_images/governance-1-19.png)
*Abbildung 11. Azure-Ressourcenrichtlinie*</span><span class="sxs-lookup"><span data-stu-id="87d33-156">![Azure resource policy](../../_images/governance-1-19.png)
*Figure 11. Azure resource policy.*</span></span>

<span data-ttu-id="87d33-157">Mit der nächsten Kontrolle wird sichergestellt, dass für die Anforderung kein [Grenzwert für das Azure-Abonnement](/azure/azure-subscription-service-limits) überschritten wird.</span><span class="sxs-lookup"><span data-stu-id="87d33-157">The next control is a check that the request does not exceed an [Azure subscription limit](/azure/azure-subscription-service-limits).</span></span> <span data-ttu-id="87d33-158">Beispielsweise gilt für alle Abonnements ein Grenzwert von 980 Ressourcengruppen pro Abonnement.</span><span class="sxs-lookup"><span data-stu-id="87d33-158">For example, each subscription has a limit of 980 resource groups per subscription.</span></span> <span data-ttu-id="87d33-159">Wenn eine Anforderung zur Bereitstellung einer weiteren Ressourcengruppe eingeht, wird dies verweigert, falls der Grenzwert bereits erreicht ist.</span><span class="sxs-lookup"><span data-stu-id="87d33-159">If a request is received to deploy an additional resource group once the limit has been reached, it is denied.</span></span>

<span data-ttu-id="87d33-160">![Azure-Ressourcenlimits](../../_images/governance-1-20.png)
*Abbildung 12. Azure-Ressourcengrenzwerte*</span><span class="sxs-lookup"><span data-stu-id="87d33-160">![Azure resource limits](../../_images/governance-1-20.png)
*Figure 12. Azure resource limits.*</span></span>

<span data-ttu-id="87d33-161">Die letzte Kontrolle umfasst eine Überprüfung, ob sich die Anforderung innerhalb der Zahlungsverpflichtung des Abonnements bewegt.</span><span class="sxs-lookup"><span data-stu-id="87d33-161">The final control is a check that the request is within the financial commitment associated with the subscription.</span></span> <span data-ttu-id="87d33-162">Wenn es bei der Anforderung beispielsweise um die Bereitstellung eines virtuellen Computers geht, überprüft Azure Resource Manager, ob das Abonnement über ausreichende Zahlungsinformationen verfügt.</span><span class="sxs-lookup"><span data-stu-id="87d33-162">For example, if the request is to deploy a virtual machine, Azure Resource Manager verifies that the subscription has sufficient payment information.</span></span>

<span data-ttu-id="87d33-163">![Einem Abonnement zugeordnete Zahlungsverpflichtung](../../_images/governance-1-21.png)
*Abbildung 13. Einem Abonnement ist eine Zahlungsverpflichtung zugeordnet.*</span><span class="sxs-lookup"><span data-stu-id="87d33-163">![A financial commitment associated with a subscription](../../_images/governance-1-21.png)
*Figure 13. A financial commitment is associated with a subscription.*</span></span>

## <a name="summary"></a><span data-ttu-id="87d33-164">Zusammenfassung</span><span class="sxs-lookup"><span data-stu-id="87d33-164">Summary</span></span>

<span data-ttu-id="87d33-165">In diesem Artikel wurde beschrieben, wie der Zugriff auf Ressourcen in Azure mit Azure Resource Manager verwaltet wird.</span><span class="sxs-lookup"><span data-stu-id="87d33-165">In this article, you learned about how resource access is managed in Azure using Azure Resource Manager.</span></span>

## <a name="next-steps"></a><span data-ttu-id="87d33-166">Nächste Schritte</span><span class="sxs-lookup"><span data-stu-id="87d33-166">Next steps</span></span>

<span data-ttu-id="87d33-167">Nachdem Sie nun wissen, wie der Ressourcenzugriff in Azure verwaltet wird, können Sie sich darüber informieren, wie Sie mit diesen Diensten ein Governancemodell [für eine einzelne Workload](governance-simple-workload.md) oder [für mehrere Teams](governance-multiple-teams.md) entwerfen.</span><span class="sxs-lookup"><span data-stu-id="87d33-167">Now that you understand how to manage resource access in Azure, move on to learn how to design a governance model [for a simple workload](governance-simple-workload.md) or for [multiple teams](governance-multiple-teams.md) using these services.</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="87d33-168">Governance-Übersicht</span><span class="sxs-lookup"><span data-stu-id="87d33-168">An overview of governance</span></span>](../overview.md)
