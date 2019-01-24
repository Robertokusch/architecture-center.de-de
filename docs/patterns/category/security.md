---
title: Sicherheitsmuster
titleSuffix: Cloud Design Patterns
description: Die Sicherheit ist die Fähigkeit eines Systems, böswillige oder unbeabsichtigte Aktionen, die nicht dem Verwendungszweck des Systems entsprechen, sowie die Offenlegung oder den Verlust von Informationen zu verhindern. Cloudanwendungen werden im Internet außerhalb vertrauenswürdiger lokaler Grenzen verfügbar gemacht, sind oft öffentlich zugänglich und können von nicht vertrauenswürdigen Benutzern verwendet werden. Durch den Entwurf und die Bereitstellung von Anwendungen muss sichergestellt werden, dass sie vor böswilligen Angriffen geschützt sind, der Zugriff auf genehmigte Benutzer beschränkt ist und vertrauliche Daten sicher sind.
keywords: Entwurfsmuster
author: dragon119
ms.date: 06/23/2017
ms.topic: design-pattern
ms.service: architecture-center
ms.subservice: cloud-fundamentals
ms.custom: seodec18
ms.openlocfilehash: c18255dccdbd8659705884a4141f5ecd099d9ece
ms.sourcegitcommit: 1b50810208354577b00e89e5c031b774b02736e2
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/23/2019
ms.locfileid: "54482084"
---
# <a name="security-patterns"></a><span data-ttu-id="81828-106">Sicherheitsmuster</span><span class="sxs-lookup"><span data-stu-id="81828-106">Security patterns</span></span>

[!INCLUDE [header](../../_includes/header.md)]

<span data-ttu-id="81828-107">Die Sicherheit ist die Fähigkeit eines Systems, böswillige oder unbeabsichtigte Aktionen, die nicht dem Verwendungszweck des Systems entsprechen, sowie die Offenlegung oder den Verlust von Informationen zu verhindern.</span><span class="sxs-lookup"><span data-stu-id="81828-107">Security is the capability of a system to prevent malicious or accidental actions outside of the designed usage, and to prevent disclosure or loss of information.</span></span> <span data-ttu-id="81828-108">Cloudanwendungen werden im Internet außerhalb vertrauenswürdiger lokaler Grenzen verfügbar gemacht, sind oft öffentlich zugänglich und können von nicht vertrauenswürdigen Benutzern verwendet werden.</span><span class="sxs-lookup"><span data-stu-id="81828-108">Cloud applications are exposed on the Internet outside trusted on-premises boundaries, are often open to the public, and may serve untrusted users.</span></span> <span data-ttu-id="81828-109">Durch den Entwurf und die Bereitstellung von Anwendungen muss sichergestellt werden, dass sie vor böswilligen Angriffen geschützt sind, der Zugriff auf genehmigte Benutzer beschränkt ist und vertrauliche Daten sicher sind.</span><span class="sxs-lookup"><span data-stu-id="81828-109">Applications must be designed and deployed in a way that protects them from malicious attacks, restricts access to only approved users, and protects sensitive data.</span></span>

|                    <span data-ttu-id="81828-110">Muster</span><span class="sxs-lookup"><span data-stu-id="81828-110">Pattern</span></span>                     |                                                                                                         <span data-ttu-id="81828-111">Zusammenfassung</span><span class="sxs-lookup"><span data-stu-id="81828-111">Summary</span></span>                                                                                                         |
|------------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| [<span data-ttu-id="81828-112">Verbundidentität</span><span class="sxs-lookup"><span data-stu-id="81828-112">Federated Identity</span></span>](../federated-identity.md) |                                                                                <span data-ttu-id="81828-113">Authentifizierung an einen externen Identitätsanbieter delegieren</span><span class="sxs-lookup"><span data-stu-id="81828-113">Delegate authentication to an external identity provider.</span></span>                                                                                |
|         [<span data-ttu-id="81828-114">Gatekeeper</span><span class="sxs-lookup"><span data-stu-id="81828-114">Gatekeeper</span></span>](../gatekeeper.md)         | <span data-ttu-id="81828-115">Schützen Sie Anwendungen und Dienste durch Verwendung einer dedizierten Hostinstanz, die als Broker zwischen Clients und der Anwendung oder dem Dienst fungiert, Anforderungen überprüft und bereinigt sowie Anforderungen und Daten zwischen ihnen weiterleitet.</span><span class="sxs-lookup"><span data-stu-id="81828-115">Protect applications and services by using a dedicated host instance that acts as a broker between clients and the application or service, validates and sanitizes requests, and passes requests and data between them.</span></span> |
|          [<span data-ttu-id="81828-116">Valet-Schlüssel</span><span class="sxs-lookup"><span data-stu-id="81828-116">Valet Key</span></span>](../valet-key.md)          |                                                        <span data-ttu-id="81828-117">Ein Token oder einen Schlüssel verwenden, das bzw. der Clients eingeschränkten direkten Zugriff auf eine bestimmte Ressource oder einen bestimmten Dienst bietet</span><span class="sxs-lookup"><span data-stu-id="81828-117">Use a token or key that provides clients with restricted direct access to a specific resource or service.</span></span>                                                        |
