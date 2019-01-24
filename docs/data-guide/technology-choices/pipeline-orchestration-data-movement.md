---
title: Auswählen einer Technologie für die Datenpipelineorchestrierung
description: ''
author: zoinerTejada
ms.date: 02/12/2018
ms.topic: guide
ms.service: architecture-center
ms.subservice: cloud-fundamentals
ms.openlocfilehash: 76a101b76497ae2b2aacff973175bb0fe4703d9e
ms.sourcegitcommit: 1b50810208354577b00e89e5c031b774b02736e2
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/23/2019
ms.locfileid: "54482441"
---
# <a name="choosing-a-data-pipeline-orchestration-technology-in-azure"></a>Auswählen einer Technologie für die Datenpipelineorchestrierung in Azure

Die meisten Big Data-Lösungen setzen sich aus wiederholten Datenverarbeitungsvorgängen zusammen, die in Workflows gekapselt sind. Ein Pipelineorchestrator ist ein Tool, mit dem diese Workflows automatisiert werden können. Ein Orchestrator kann Aufträge planen, Workflows ausführen und Abhängigkeiten zwischen Tasks koordinieren.

## <a name="what-are-your-options-for-data-pipeline-orchestration"></a>Welche Datenpipelineorchestrierungs-Optionen stehen zur Verfügung?

In Azure erfüllen die folgenden Dienste und Tools die grundlegenden Anforderungen für Pipelineorchestrierung, Ablaufsteuerung und Datenverschiebung:

- [Azure Data Factory](/azure/data-factory/)
- [Oozie in HDInsight](/azure/hdinsight/hdinsight-use-oozie-linux-mac)
- [SQL Server Integration Services (SSIS)](/sql/integration-services/sql-server-integration-services)

Diese Dienste und Tools können unabhängig voneinander oder zusammen zum Erstellen einer Hybridlösung verwendet werden. Beispielsweise kann die Integration Runtime (IR) in Azure Data Factory V2 SSIS-Pakete nativ in einer verwalteten Azure-Computeumgebung ausführen. Einige Funktionen dieser Dienste überschneiden sich zwar, es gibt jedoch auch wesentliche Unterschiede.

## <a name="key-selection-criteria"></a>Wichtige Auswahlkriterien

Beantworten Sie die folgenden Fragen, um die Auswahl einzuschränken:

- Benötigen Sie Big Data-Funktionen zum Verschieben und Transformieren von Daten? In der Regel geht es dabei um Datenvolumen von mehreren Gigabytes oder Terabytes. Falls Sie solche Funktionen benötigen, können Sie sich auf die Optionen beschränken, die sich am besten für Big Data eignen.

- Benötigen Sie einen verwalteten Dienst, der bedarfsorientiert ausgeführt werden kann? Falls ja, wählen Sie einen der cloudbasierten Dienste aus, die nicht durch die lokale Verarbeitungsleistung beschränkt sind.

- Befinden sich einige Ihrer Datenquellen lokal? Falls ja, suchen Sie Optionen, die mit cloudbasierten und lokalen Datenquellen oder -zielen verwendet werden können.

- Werden Ihre Quelldaten im Blobspeicher oder in einem HDFS-Dateisystem gespeichert? Wenn dies der Fall ist, wählen Sie eine Option, die Hive-Abfragen unterstützt.

## <a name="capability-matrix"></a>Funktionsmatrix

In den folgenden Tabellen sind die Hauptunterschiede in Bezug auf die Funktionen zusammengefasst.

### <a name="general-capabilities"></a>Allgemeine Funktionen

| | Azure Data Factory | SQL Server Integration Services (SSIS) | Oozie in HDInsight
| --- | --- | --- | --- |
| Verwaltet | JA | Nein | JA |
| Cloudbasiert | JA | Nein (lokal) | JA |
| Voraussetzung | Azure-Abonnement | SQL Server  | Azure-Abonnement, HDInsight-Cluster |
| Verwaltungstools | Azure-Portal, PowerShell, CLI, .NET SDK | SSMS, PowerShell | Bash-Shell, Oozie-REST-API, Oozie-Webbenutzeroberfläche |
| Preise | Nutzungsbasierte Bezahlung | Lizenzierung/Bezahlung für Funktionen | Keine Zusatzgebühren (nur Gebühren für die Ausführung des HDInsight-Clusters) |

### <a name="pipeline-capabilities"></a>Pipelinefunktionen

| | Azure Data Factory | SQL Server Integration Services (SSIS) | Oozie in HDInsight
| --- | --- | --- | --- |
| Kopieren von Daten | JA | Ja | JA |
| Benutzerdefinierte Transformationen | JA | JA | Ja (MapReduce-, Pig- und Hive-Aufträge) |
| Azure Machine Learning-Bewertung | JA | Ja (mit Skripts) | Nein  |
| HDInsight (bedarfsgesteuert) | JA | Nein  | Nein  |
| Azure Batch | JA | Nein  | Nein  |
| Pig, Hive, MapReduce | JA | Nein | JA |
| Spark | JA | Nein  | Nein  |
| Ausführen des SSIS-Pakets | JA | JA | Nein  |
| Ablaufsteuerung | JA | Ja | JA |
| Zugriff auf lokale Daten | JA | JA | Nein  |

### <a name="scalability-capabilities"></a>Skalierbarkeitsfunktionen

| | Azure Data Factory | SQL Server Integration Services (SSIS) | Oozie in HDInsight
| --- | --- | --- | --- |
| Zentrales Hochskalieren | JA | Nein  | Nein  |
| Horizontales Skalieren | JA | Nein  | Ja (durch Hinzufügen von Workerknoten zum Cluster) |
| Für Big Data optimiert | JA | Nein | JA |

