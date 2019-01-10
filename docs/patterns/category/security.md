---
title: Sicherheitsmuster
titleSuffix: Cloud Design Patterns
description: Die Sicherheit ist die Fähigkeit eines Systems, böswillige oder unbeabsichtigte Aktionen, die nicht dem Verwendungszweck des Systems entsprechen, sowie die Offenlegung oder den Verlust von Informationen zu verhindern. Cloudanwendungen werden im Internet außerhalb vertrauenswürdiger lokaler Grenzen verfügbar gemacht, sind oft öffentlich zugänglich und können von nicht vertrauenswürdigen Benutzern verwendet werden. Durch den Entwurf und die Bereitstellung von Anwendungen muss sichergestellt werden, dass sie vor böswilligen Angriffen geschützt sind, der Zugriff auf genehmigte Benutzer beschränkt ist und vertrauliche Daten sicher sind.
keywords: Entwurfsmuster
author: dragon119
ms.date: 06/23/2017
ms.custom: seodec18
ms.openlocfilehash: 259d4bd5178ca8ab85df0cd03e5268c5a7845a3a
ms.sourcegitcommit: 680c9cef945dff6fee5e66b38e24f07804510fa9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/04/2019
ms.locfileid: "54009439"
---
# <a name="security-patterns"></a>Sicherheitsmuster

[!INCLUDE [header](../../_includes/header.md)]

Die Sicherheit ist die Fähigkeit eines Systems, böswillige oder unbeabsichtigte Aktionen, die nicht dem Verwendungszweck des Systems entsprechen, sowie die Offenlegung oder den Verlust von Informationen zu verhindern. Cloudanwendungen werden im Internet außerhalb vertrauenswürdiger lokaler Grenzen verfügbar gemacht, sind oft öffentlich zugänglich und können von nicht vertrauenswürdigen Benutzern verwendet werden. Durch den Entwurf und die Bereitstellung von Anwendungen muss sichergestellt werden, dass sie vor böswilligen Angriffen geschützt sind, der Zugriff auf genehmigte Benutzer beschränkt ist und vertrauliche Daten sicher sind.

|                    Muster                     |                                                                                                         Zusammenfassung                                                                                                         |
|------------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| [Verbundidentität](../federated-identity.md) |                                                                                Authentifizierung an einen externen Identitätsanbieter delegieren                                                                                |
|         [Gatekeeper](../gatekeeper.md)         | Schützen Sie Anwendungen und Dienste durch Verwendung einer dedizierten Hostinstanz, die als Broker zwischen Clients und der Anwendung oder dem Dienst fungiert, Anforderungen überprüft und bereinigt sowie Anforderungen und Daten zwischen ihnen weiterleitet. |
|          [Valet-Schlüssel](../valet-key.md)          |                                                        Ein Token oder einen Schlüssel verwenden, das bzw. der Clients eingeschränkten direkten Zugriff auf eine bestimmte Ressource oder einen bestimmten Dienst bietet                                                        |
