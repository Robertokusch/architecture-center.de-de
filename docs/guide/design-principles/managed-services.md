---
title: Verwenden verwalteter Dienste
description: Verwenden Sie nach Möglichkeit PaaS (Platform-as-a-Service) anstelle von IaaS (Infrastructure-as-a-Service).
author: MikeWasson
ms.openlocfilehash: 6d3cfb2e97b5a9b25bb1afd72059e981ef45c0d8
ms.sourcegitcommit: 26b04f138a860979aea5d253ba7fecffc654841e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/19/2018
ms.locfileid: "36206721"
---
# <a name="use-managed-services"></a>Verwenden verwalteter Dienste

## <a name="when-possible-use-platform-as-a-service-paas-rather-than-infrastructure-as-a-service-iaas"></a>Verwenden Sie nach Möglichkeit PaaS (Platform-as-a-Service) anstelle von IaaS (Infrastructure-as-a-Service)

IaaS können Sie sich wie eine Kiste mit Einzelteilen vorstellen. Sie können beliebige Dinge bauen, aber Sie müssen alles selbst zusammensetzen. Verwaltete Dienste können einfacher konfiguriert und verwaltet werden. Sie müssen keine VMs bereitstellen, VNets einrichten oder Patches und Updates verwalten, und auch der restliche Mehraufwand für die Ausführung von Software auf einer VM fällt weg.

Angenommen, Ihre Anwendung benötigt eine Nachrichtenwarteschlange. Sie können Ihren eigenen Messagingdienst auf einer VM einrichten, indem Sie beispielsweise RabbitMQ verwenden. Für Azure Service Bus wird aber bereits ein zuverlässiger Messagingdienst bereitgestellt, der leichter einzurichten ist. Erstellen Sie einfach einen Service Bus-Namespace (z.B. im Rahmen des Bereitstellungsskripts), und rufen Sie anschließend Service Bus auf, indem Sie das Client-SDK verwenden. 

Es kann natürlich sein, dass Ihre Anwendung über bestimmte Anforderungen verfügt, für die ein IaaS-Ansatz besser geeignet ist. Auch wenn Ihre Anwendung auf IaaS basiert, sollten Sie nach Stellen suchen, für die die Einbindung von verwalteten Diensten hilfreich ist. Hierzu gehören die Bereiche Cache, Warteschlangen und Datenspeicher.

| Anstelle der Ausführung von: | Mögliche Verwendung von: |
|-----------------------|-------------|
| Active Directory | Azure Active Directory Domain Services |
| Elasticsearch | Azure Search |
| Hadoop | HDInsight |
| IIS | App Service |
| MongoDB | Cosmos DB |
| Redis | Azure Redis Cache |
| SQL Server | Azure SQL-Datenbank |


