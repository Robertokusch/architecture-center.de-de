---
title: 'Framework für die Cloudeinführung (Cloud Adoption Framework, CAF): Softwaredefinierte Netzwerke – Reine Paas-Lösung'
titleSuffix: Microsoft Cloud Adoption Framework for Azure
ms.service: architecture-center
ms.subservice: enterprise-cloud-adoption
ms.custom: governance
ms.date: 02/11/2019
description: Erläuterung des reinen PaaS-Modells für cloudbasierte Netzwerkfunktionen
author: rotycenh
ms.openlocfilehash: 2f3f82d781ddb6544721e82e7b7d795222a2f8ff
ms.sourcegitcommit: c053e6edb429299a0ad9b327888d596c48859d4a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/20/2019
ms.locfileid: "58241201"
---
# <a name="software-defined-networks-paas-only"></a><span data-ttu-id="22240-103">Softwaredefinierte Netzwerke: Reine PaaS-Lösung</span><span class="sxs-lookup"><span data-stu-id="22240-103">Software Defined Networks: PaaS-only</span></span>

<span data-ttu-id="22240-104">Wenn Sie eine PaaS-Ressource (Platform as a Service) implementieren, wird im Bereitstellungsprozess automatisch ein angenommenes zugrunde liegendes Netzwerk mit einer begrenzten Anzahl von Steuerungsmöglichkeiten über dieses Netzwerk erstellt, einschließlich Lastenausgleich, Portblockierung und Verbindungen mit anderen PaaS-Diensten.</span><span class="sxs-lookup"><span data-stu-id="22240-104">When you implement a platform as a service (PaaS) resource, the deployment process automatically creates an assumed underlying network with a limited number of controls over that network, including load balancing, port blocking, and connections to other PaaS services.</span></span>

<span data-ttu-id="22240-105">In Azure können mehrere PaaS-Ressourcentypen in einem virtuellen Netzwerk [bereitgestellt](/azure/virtual-network/virtual-network-for-azure-services) oder damit [verbunden](/azure/virtual-network/virtual-network-service-endpoints-overview) werden, sodass diese Ressourcen in Ihre bestehende virtuelle Netzwerkinfrastruktur integriert werden können.</span><span class="sxs-lookup"><span data-stu-id="22240-105">In Azure, several PaaS resource types can be [deployed into](/azure/virtual-network/virtual-network-for-azure-services) or [connected to](/azure/virtual-network/virtual-network-service-endpoints-overview) a virtual network, allowing these resources to integrate with your existing virtual networking infrastructure.</span></span> <span data-ttu-id="22240-106">In vielen Fällen reicht jedoch eine reine PaaS-Netzwerkarchitektur, die sich ausschließlich auf die standardmäßigen Netzwerkfunktionen stützt, die von PaaS-Ressourcen nativ bereitgestellt werden, aus, um die Anforderungen an die Workloads zu erfüllen.</span><span class="sxs-lookup"><span data-stu-id="22240-106">However, in many cases a PaaS only networking architecture, relying only on these default networking capabilities natively provided by PaaS resources, is sufficient to meet workload requirements.</span></span>

<span data-ttu-id="22240-107">Wenn Sie eine reine PaaS-Netzwerkarchitektur in Betracht ziehen, sollten Sie sicherstellen, dass Ihre Annahmen mit Ihren Anforderungen übereinstimmen.</span><span class="sxs-lookup"><span data-stu-id="22240-107">If you are considering a PaaS only networking architecture, be sure you validate that the required assumptions align with your requirements.</span></span>

## <a name="paas-only-assumptions"></a><span data-ttu-id="22240-108">Annahmen für eine reine Paas-Lösung</span><span class="sxs-lookup"><span data-stu-id="22240-108">PaaS-only assumptions</span></span>

<span data-ttu-id="22240-109">Für die Bereitstellung einer reinen PaaS-Netzwerkarchitektur wird Folgendes angenommen:</span><span class="sxs-lookup"><span data-stu-id="22240-109">Deploying a PaaS-only networking architecture assumes the following:</span></span>

- <span data-ttu-id="22240-110">Die bereitzustellende Anwendung ist eine eigenständige Anwendung ODER ist nur von anderen PaaS-Ressourcen abhängig.</span><span class="sxs-lookup"><span data-stu-id="22240-110">The application being deployed is a standalone application OR is dependent on only other PaaS resources.</span></span>
- <span data-ttu-id="22240-111">Ihre IT-Betriebsteams können ihre Tools, Trainings und Prozesse aktualisieren, um die Verwaltung, Konfiguration und Bereitstellung eigenständiger PaaS-Anwendungen zu unterstützen.</span><span class="sxs-lookup"><span data-stu-id="22240-111">Your IT operations teams can update their tools, training, and processes to support management, configuration, and deployment of standalone PaaS applications.</span></span>
- <span data-ttu-id="22240-112">Die PaaS-Anwendung ist nicht Teil einer umfassenderen Cloudmigration, die IaaS-Ressourcen einbezieht.</span><span class="sxs-lookup"><span data-stu-id="22240-112">The PaaS application is not part of a broader cloud migration effort that will include IaaS resources.</span></span>

<span data-ttu-id="22240-113">Diese Annahmen sind Mindestkriterien, die auf die Bereitstellung eines reinen PaaS-Netzwerks ausgelegt sind.</span><span class="sxs-lookup"><span data-stu-id="22240-113">These assumptions are minimum qualifiers aligned to deploying a PaaS-only network.</span></span> <span data-ttu-id="22240-114">Wenngleich dieser Ansatz die Anforderungen einer einzelnen Anwendungsbereitstellung erfüllen kann, sollte Ihr für die Cloudeinführung zuständiges Team diese längerfristigen Fragen prüfen:</span><span class="sxs-lookup"><span data-stu-id="22240-114">While this approach may align with the requirements of a single application deployment, your Cloud Adoption Team should examine these long-term questions:</span></span>

- <span data-ttu-id="22240-115">Wird diese Bereitstellung in Bezug auf Umfang oder Skalierung erweitert, um den Zugriff auf andere Nicht-PaaS-Ressourcen zu ermöglichen?</span><span class="sxs-lookup"><span data-stu-id="22240-115">Will this deployment expand in scope or scale to require access to other non-PaaS resources?</span></span>
- <span data-ttu-id="22240-116">Sind weitere PaaS-Bereitstellungen über die aktuelle Lösung hinaus geplant?</span><span class="sxs-lookup"><span data-stu-id="22240-116">Are other PaaS deployments planned beyond the current solution?</span></span>
- <span data-ttu-id="22240-117">Hat die Organisation Pläne für weitere künftige Cloudmigrationen?</span><span class="sxs-lookup"><span data-stu-id="22240-117">Does the organization have plans for other future cloud migrations?</span></span>

<span data-ttu-id="22240-118">Die Antworten auf diese Fragen werden ein Team nicht davon abhalten, sich für eine reine PaaS-Lösung zu entscheiden, sollten aber vor einer endgültigen Entscheidung berücksichtigt werden.</span><span class="sxs-lookup"><span data-stu-id="22240-118">The answers to these questions would not preclude a team from choosing a PaaS only option but should be considered before making a final decision.</span></span>
