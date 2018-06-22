---
title: Entscheidungsstruktur für Azure-Computedienste
description: Ein Flussdiagramm zur Auswahl eines Computediensts
author: MikeWasson
ms.date: 06/13/2018
ms.openlocfilehash: 60bb84d4bf210888d3d43498db043b6e452f6a80
ms.sourcegitcommit: 26b04f138a860979aea5d253ba7fecffc654841e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/19/2018
ms.locfileid: "36206726"
---
# <a name="decision-tree-for-azure-compute-services"></a>Entscheidungsstruktur für Azure-Computedienste

Azure bietet eine Vielzahl von Möglichkeiten zum Hosten Ihres Anwendungscodes. Der Begriff *Compute* bezieht sich auf das Hostingmodell für die Computeressourcen, auf denen Ihre Anwendung ausgeführt wird. Das folgende Flussdiagramm unterstützt Sie bei der Auswahl eines Computediensts für Ihre Anwendung. Das Flussdiagramm führt Sie durch eine Reihe wichtiger Entscheidungskriterien, um eine Empfehlung zu erzielen. 

**Betrachten Sie dieses Flussdiagramm als Ausgangspunkt.** Da jede Anwendung besondere Anforderungen aufweist, betrachten Sie die Empfehlung als Ausgangspunkt. Führen Sie dann eine ausführlichere Auswertung durch, bei der Sie z.B. folgende Aspekte betrachten:
 
- Funktionsumfang
- [Diensteinschränkungen](/azure/azure-subscription-service-limits)
- [Kosten](https://azure.microsoft.com/pricing/)
- [SLA](https://azure.microsoft.com/support/legal/sla/)
- [Regionale Verfügbarkeit](https://azure.microsoft.com/global-infrastructure/services/)
- Entwicklerökosystem und Teamkenntnisse
- [Kriterien für die Auswahl einer Azure-Compute-Option](./compute-comparison.md)

Wenn Ihre Anwendung aus mehreren Workloads besteht, bewerten Sie jede Workload getrennt. Eine vollständige Lösung kann zwei oder mehr Computedienste umfassen.

## <a name="flowchart"></a>Flussdiagramm

![](../images/compute-decision-tree.svg)

## <a name="definitions"></a>Definitionen

- **Greenfield** beschreibt ein völlig neues Softwareprojekt, das von Grund auf neu erstellt wird. Es enthält keinen Legacycode. 

- **Brownfield** beschreibt ein Softwareprojekt, das auf einer vorhandenen Anwendung basiert. Es enthält unter Umständen Legacycode oder -frameworks.

- **Lift und Shift** ist eine Strategie zum Migrieren einer Workload zur Cloud ohne Überarbeitung der Anwendung oder Änderung des Codes. Dies wird auch als *erneutes Hosten* bezeichnet. Weitere Informationen finden Sie im [Azure-Migrationscenter](https://azure.microsoft.com/migration/).

- **Cloudoptimierung** ist eine Strategie für die Migration zur Cloud durch Umgestaltung einer Anwendung, um cloudeigene Features und Funktionen zu nutzen.

## <a name="next-steps"></a>Nächste Schritte

Weitere Aspekte, die berücksichtigt werden sollten, finden Sie unter [Kriterien für die Auswahl einer Azure-Compute-Option](./compute-comparison.md).
