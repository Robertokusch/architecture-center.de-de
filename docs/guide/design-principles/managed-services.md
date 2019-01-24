---
title: Verwenden verwalteter Dienste
titleSuffix: Azure Application Architecture Guide
description: Verwenden Sie nach Möglichkeit PaaS (Platform-as-a-Service) anstelle von IaaS (Infrastructure-as-a-Service).
author: MikeWasson
ms.date: 08/30/2018
ms.topic: guide
ms.service: architecture-center
ms.subservice: reference-architecture
ms.custom: seojan19
ms.openlocfilehash: f08914e1eacc4f02fdb16093fe590f46249e3da3
ms.sourcegitcommit: 1b50810208354577b00e89e5c031b774b02736e2
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/23/2019
ms.locfileid: "54485807"
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
