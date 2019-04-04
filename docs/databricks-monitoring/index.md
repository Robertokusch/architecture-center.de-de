---
title: Überwachen von Azure Databricks mit Azure Monitor
description: Scala-Bibliothek, um die Überwachung von Azure Databricks in Azure Log Analytics zu ermöglichen
author: petertaylor9999
ms.date: 03/26/2019
ms.openlocfilehash: 4544d3abc3264ec459a80ac1a61a912e6d30d6b2
ms.sourcegitcommit: 1a3cc91530d56731029ea091db1f15d41ac056af
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/03/2019
ms.locfileid: "58887691"
---
# <a name="monitoring-azure-databricks"></a>Überwachen von Azure Databricks

[Azure Databricks](/azure/azure-databricks/) ist ein schneller, leistungsstarker Analysedienst, der auf [Apache Spark](https://spark.apache.org/) basiert und eine zügige Entwicklung und Bereitstellung von Analyse- und KI-Lösungen für Big Data ermöglicht. Zahlreiche Benutzer profitieren von der Unkompliziertheit einfacher Notebooks in ihren Azure Databricks-Lösungen. Für Benutzer, die stabilere Computingoptionen benötigen, unterstützt Azure Databricks die verteilte Ausführung von benutzerdefiniertem Anwendungscode.

Überwachung ist ein wichtiger Bestandteil jeder Lösung auf Produktionsebene. Azure Databricks bietet stabile Funktionen für die Überwachung von benutzerdefinierten Metriken, Streamingabfrageereignissen und Anwendungsprotokollmeldungen. Azure Databricks kann diese Überwachungsdaten an verschiedene Protokollierungsdienste senden.

In den folgenden Artikeln wird gezeigt, wie Sie Überwachungsdaten aus Azure Databricks an [Azure Monitor](/azure/azure-monitor/overview), die Plattform für Überwachungsdaten für Azure, senden. Sie sollten diese Artikel in der angegebenen Reihenfolge lesen.

1. [Konfigurieren von Azure Databricks zum Senden von Metriken an Azure Monitor](./configure-cluster.md)
1. [Senden von Azure Databricks-Anwendungsprotokollen an Azure Monitor](./application-logs.md)
1. [Visualisieren von Azure Databricks-Metriken mithilfe von Dashboards](./dashboards.md)

Die zu diesen Artikeln gehörende Codebibliothek erweitert die grundlegende Überwachungsfunktion von Azure Databricks, damit Spark-Metriken, -Ereignisse und -Protokollierungsinformationen an Azure Monitor gesendet werden können.

Diese Artikel und die zugehörige Codebibliothek sind für Entwickler von Apache Spark- und Azure Databricks-Lösungen gedacht. Der Code muss in JAR-Dateien (Java Archive) integriert und anschließend in einem Azure Databricks-Cluster bereitgestellt werden. Der Code ist eine Kombination aus [Scala](https://www.scala-lang.org/) und Java und enthält entsprechende [Maven](https://maven.apache.org)-POM-Dateien (Projektobjektmodell) zum Erstellen der JAR-Ausgabedateien. Es wird empfohlen, sich mit Java, Scala und Maven vertraut zu machen.

## <a name="next-steps"></a>Nächste Schritte

Erstellen Sie zunächst die Codebibliothek, und stellen Sie sie in Ihrem Azure Databricks-Cluster bereit.

> [!div class="nextstepaction"]
> [Konfigurieren von Azure Databricks zum Senden von Metriken an Azure Monitor](./configure-cluster.md)