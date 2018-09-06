---
title: Verwenden verwalteter Dienste
description: Verwenden Sie nach Möglichkeit PaaS (Platform-as-a-Service) anstelle von IaaS (Infrastructure-as-a-Service).
author: MikeWasson
ms.date: 08/30/2018
ms.openlocfilehash: f6777a19e126a8a7f64be05dfad9bc503d27b1c3
ms.sourcegitcommit: ae8a1de6f4af7a89a66a8339879843d945201f85
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/31/2018
ms.locfileid: "43325773"
---
# <a name="use-managed-services"></a><span data-ttu-id="c6c02-103">Verwenden verwalteter Dienste</span><span class="sxs-lookup"><span data-stu-id="c6c02-103">Use managed services</span></span>

## <a name="when-possible-use-platform-as-a-service-paas-rather-than-infrastructure-as-a-service-iaas"></a><span data-ttu-id="c6c02-104">Verwenden Sie nach Möglichkeit PaaS (Platform-as-a-Service) anstelle von IaaS (Infrastructure-as-a-Service)</span><span class="sxs-lookup"><span data-stu-id="c6c02-104">When possible, use platform as a service (PaaS) rather than infrastructure as a service (IaaS)</span></span>

<span data-ttu-id="c6c02-105">IaaS können Sie sich wie eine Kiste mit Einzelteilen vorstellen.</span><span class="sxs-lookup"><span data-stu-id="c6c02-105">IaaS is like having a box of parts.</span></span> <span data-ttu-id="c6c02-106">Sie können beliebige Dinge bauen, aber Sie müssen alles selbst zusammensetzen.</span><span class="sxs-lookup"><span data-stu-id="c6c02-106">You can build anything, but you have to assemble it yourself.</span></span> <span data-ttu-id="c6c02-107">Verwaltete Dienste können einfacher konfiguriert und verwaltet werden.</span><span class="sxs-lookup"><span data-stu-id="c6c02-107">Managed services are easier to configure and administer.</span></span> <span data-ttu-id="c6c02-108">Sie müssen keine VMs bereitstellen, VNets einrichten oder Patches und Updates verwalten, und auch der restliche Mehraufwand für die Ausführung von Software auf einer VM fällt weg.</span><span class="sxs-lookup"><span data-stu-id="c6c02-108">You don't need to provision VMs, set up VNets, manage patches and updates, and all of the other overhead associated with running software on a VM.</span></span>

<span data-ttu-id="c6c02-109">Angenommen, Ihre Anwendung benötigt eine Nachrichtenwarteschlange.</span><span class="sxs-lookup"><span data-stu-id="c6c02-109">For example, suppose your application needs a message queue.</span></span> <span data-ttu-id="c6c02-110">Sie können Ihren eigenen Messagingdienst auf einer VM einrichten, indem Sie beispielsweise RabbitMQ verwenden.</span><span class="sxs-lookup"><span data-stu-id="c6c02-110">You could set up your own messaging service on a VM, using something like RabbitMQ.</span></span> <span data-ttu-id="c6c02-111">Für Azure Service Bus wird aber bereits ein zuverlässiger Messagingdienst bereitgestellt, der leichter einzurichten ist.</span><span class="sxs-lookup"><span data-stu-id="c6c02-111">But Azure Service Bus already provides reliable messaging as service, and it's simpler to set up.</span></span> <span data-ttu-id="c6c02-112">Erstellen Sie einfach einen Service Bus-Namespace (z.B. im Rahmen des Bereitstellungsskripts), und rufen Sie anschließend Service Bus auf, indem Sie das Client-SDK verwenden.</span><span class="sxs-lookup"><span data-stu-id="c6c02-112">Just create a Service Bus namespace (which can be done as part of a deployment script) and then call Service Bus using the client SDK.</span></span> 

<span data-ttu-id="c6c02-113">Es kann natürlich sein, dass Ihre Anwendung über bestimmte Anforderungen verfügt, für die ein IaaS-Ansatz besser geeignet ist.</span><span class="sxs-lookup"><span data-stu-id="c6c02-113">Of course, your application may have specific requirements that make an IaaS approach more suitable.</span></span> <span data-ttu-id="c6c02-114">Auch wenn Ihre Anwendung auf IaaS basiert, sollten Sie nach Stellen suchen, für die die Einbindung von verwalteten Diensten hilfreich ist.</span><span class="sxs-lookup"><span data-stu-id="c6c02-114">However, even if your application is based on IaaS, look for places where it may be natural to incorporate managed services.</span></span> <span data-ttu-id="c6c02-115">Hierzu gehören die Bereiche Cache, Warteschlangen und Datenspeicher.</span><span class="sxs-lookup"><span data-stu-id="c6c02-115">These include cache, queues, and data storage.</span></span>

| <span data-ttu-id="c6c02-116">Anstelle der Ausführung von:</span><span class="sxs-lookup"><span data-stu-id="c6c02-116">Instead of running...</span></span> | <span data-ttu-id="c6c02-117">Mögliche Verwendung von:</span><span class="sxs-lookup"><span data-stu-id="c6c02-117">Consider using...</span></span> |
|-----------------------|-------------|
| <span data-ttu-id="c6c02-118">Active Directory</span><span class="sxs-lookup"><span data-stu-id="c6c02-118">Active Directory</span></span> | <span data-ttu-id="c6c02-119">Azure Active Directory Domain Services</span><span class="sxs-lookup"><span data-stu-id="c6c02-119">Azure Active Directory Domain Services</span></span> |
| <span data-ttu-id="c6c02-120">Elasticsearch</span><span class="sxs-lookup"><span data-stu-id="c6c02-120">Elasticsearch</span></span> | <span data-ttu-id="c6c02-121">Azure Search</span><span class="sxs-lookup"><span data-stu-id="c6c02-121">Azure Search</span></span> |
| <span data-ttu-id="c6c02-122">Hadoop</span><span class="sxs-lookup"><span data-stu-id="c6c02-122">Hadoop</span></span> | <span data-ttu-id="c6c02-123">HDInsight</span><span class="sxs-lookup"><span data-stu-id="c6c02-123">HDInsight</span></span> |
| <span data-ttu-id="c6c02-124">IIS</span><span class="sxs-lookup"><span data-stu-id="c6c02-124">IIS</span></span> | <span data-ttu-id="c6c02-125">App Service</span><span class="sxs-lookup"><span data-stu-id="c6c02-125">App Service</span></span> |
| <span data-ttu-id="c6c02-126">MongoDB</span><span class="sxs-lookup"><span data-stu-id="c6c02-126">MongoDB</span></span> | <span data-ttu-id="c6c02-127">Cosmos DB</span><span class="sxs-lookup"><span data-stu-id="c6c02-127">Cosmos DB</span></span> |
| <span data-ttu-id="c6c02-128">Redis</span><span class="sxs-lookup"><span data-stu-id="c6c02-128">Redis</span></span> | <span data-ttu-id="c6c02-129">Azure Redis Cache</span><span class="sxs-lookup"><span data-stu-id="c6c02-129">Azure Redis Cache</span></span> |
| <span data-ttu-id="c6c02-130">SQL Server</span><span class="sxs-lookup"><span data-stu-id="c6c02-130">SQL Server</span></span> | <span data-ttu-id="c6c02-131">Azure SQL-Datenbank</span><span class="sxs-lookup"><span data-stu-id="c6c02-131">Azure SQL Database</span></span> |


