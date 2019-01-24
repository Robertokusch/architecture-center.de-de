---
title: Auswählen einer Technologie für die Datenstromverarbeitung
description: ''
author: zoinerTejada
ms.date: 02/12/2018
ms.topic: guide
ms.service: architecture-center
ms.subservice: cloud-fundamentals
ms.openlocfilehash: 342e44d960682c72901a7482caaf328514eb73d8
ms.sourcegitcommit: 1b50810208354577b00e89e5c031b774b02736e2
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/23/2019
ms.locfileid: "54486114"
---
# <a name="choosing-a-stream-processing-technology-in-azure"></a>Auswählen einer Technologie für die Datenstromverarbeitung in Azure

In diesem Artikel werden die Technologieoptionen für die Echtzeit-Datenstromverarbeitung in Azure verglichen.

Die Echtzeit-Datenstromverarbeitung liest Nachrichten aus warteschlangen- oder dateibasiertem Speicher, verarbeitet die Nachrichten und leitet das Ergebnis an eine andere Nachrichtenwarteschlange, einen anderen Dateispeicher oder eine andere Datenbank weiter. Die Verarbeitung beinhaltet unter Umständen das Abfragen, Filtern und Aggregieren von Nachrichten. Datenstromverarbeitungs-Engines müssen in der Lage sein, unbegrenzte Datenströme zu nutzen und mit minimaler Wartezeit Ergebnisse zu erzeugen. Weitere Informationen finden Sie unter [Real time processing](../big-data/real-time-processing.md) (Verarbeitung in Echtzeit).

<!-- markdownlint-disable MD026 -->

## <a name="what-are-your-options-when-choosing-a-technology-for-real-time-processing"></a>Welche Technologien für die Echtzeitverarbeitung stehen zur Verfügung?

<!-- markdownlint-enable MD026 -->

In Azure erfüllen alle folgenden Datenspeicher die grundlegenden Anforderungen für die Echtzeitverarbeitung:

- [Azure Stream Analytics](/azure/stream-analytics/)
- [HDInsight mit Spark Streaming](/azure/hdinsight/spark/apache-spark-streaming-overview)
- [Apache Spark in Azure Databricks](/azure/azure-databricks/)
- [HDInsight mit Storm](/azure/hdinsight/storm/apache-storm-overview)
- [Azure-Funktionen](/azure/azure-functions/functions-overview)
- [WebJobs in Azure App Service](/azure/app-service/web-sites-create-web-jobs)

## <a name="key-selection-criteria"></a>Wichtige Auswahlkriterien

Beginnen Sie bei Szenarien für die Echtzeitverarbeitung mit der Auswahl des geeigneten Diensts für Ihre Anforderungen, indem Sie die folgenden Fragen beantworten:

- Bevorzugen Sie beim Erstellen der Logik für die Datenstromverarbeitung einen deklarativen oder einen imperativen Ansatz?

- Benötigen Sie integrierten Unterstützung für temporale Verarbeitung oder Windowing?

- Werden Daten in anderen Formaten als Avro, JSON oder CSV empfangen? Falls ja, ziehen Sie Optionen in Erwägung, die mithilfe von benutzerdefiniertem Code alle Formate unterstützen.

- Müssen Sie die Verarbeitung auf über 1 GB/s skalieren? Falls ja, ziehen Sie die Optionen in Erwägung, die mit der Clustergröße skaliert werden.

## <a name="capability-matrix"></a>Funktionsmatrix

In den folgenden Tabellen sind die Hauptunterschiede in Bezug auf die Funktionen zusammengefasst.

### <a name="general-capabilities"></a>Allgemeine Funktionen

| | Azure Stream Analytics | HDInsight mit Spark Streaming | Apache Spark in Azure Databricks | HDInsight mit Storm | Azure-Funktionen | WebJobs in Azure App Service |
| --- | --- | --- | --- | --- | --- | --- |
| Programmierbarkeit | Stream Analytics-Abfragesprache, JavaScript | Scala, Python, Java | Scala, Python, Java, R | Java, C# | C#, F#, Node.js | C#, Node.js, PHP, Java, Python |
| Programmierparadigma | Deklarativ | Mischung aus deklarativ und imperativ | Mischung aus deklarativ und imperativ | Imperativ | Imperativ | Imperativ |
| Preismodell | [Streamingeinheiten](https://azure.microsoft.com/pricing/details/stream-analytics/) | Pro Clusterstunde | [Databricks-Einheiten](https://azure.microsoft.com/pricing/details/databricks/) | Pro Clusterstunde | Nach Funktionsausführung und Ressourcenverbrauch | Nach App Service-Plan-Stunde |  

### <a name="integration-capabilities"></a>Integrationsfunktionen

| | Azure Stream Analytics | HDInsight mit Spark Streaming | Apache Spark in Azure Databricks | HDInsight mit Storm | Azure-Funktionen | WebJobs in Azure App Service |
| --- | --- | --- | --- | --- | --- | --- |
| Eingaben | Azure Event Hubs, Azure IoT Hub, Azure Blob-Speicher  | Event Hubs, IoT Hub, Kafka, HDFS, Speicherblobs, Azure Data Lake Store  | Event Hubs, IoT Hub, Kafka, HDFS, Speicherblobs, Azure Data Lake Store  | Event Hubs, IoT Hub, Speicherblobs, Azure Data Lake Store  | [Unterstützte Bindungen](/azure/azure-functions/functions-triggers-bindings#supported-bindings) | Service Bus, Speicherwarteschlangen, Speicherblobs, Event Hubs, WebHooks, Cosmos DB, Dateien |
| Senken |  Azure Data Lake Store, Azure SQL-Datenbank, Storage-Blobs, Event Hubs, Power BI, Table Storage, Service Bus-Warteschlangen, Service Bus-Themen, Cosmos DB, Azure Functions  | HDFS, Kafka, Speicherblobs, Azure Data Lake Store, Cosmos DB | HDFS, Kafka, Speicherblobs, Azure Data Lake Store, Cosmos DB | Event Hubs, Service Bus, Kafka | [Unterstützte Bindungen](/azure/azure-functions/functions-triggers-bindings#supported-bindings) | Service Bus, Speicherwarteschlangen, Speicherblobs, Event Hubs, WebHooks, Cosmos DB, Dateien |

### <a name="processing-capabilities"></a>Verarbeitungsfunktionen

| | Azure Stream Analytics | HDInsight mit Spark Streaming | Apache Spark in Azure Databricks | HDInsight mit Storm | Azure-Funktionen | WebJobs in Azure App Service |
| --- | --- | --- | --- | --- | --- | --- |
| Unterstützung für integrierte temporal Verarbeitung/Windowing | JA | Ja | Ja | JA | Nein  | Nein  |
| Datenformate für die Eingabe | Avro, JSON oder CSV, UTF-8-codiert | Jedes Format mit benutzerdefiniertem Code | Jedes Format mit benutzerdefiniertem Code | Jedes Format mit benutzerdefiniertem Code | Jedes Format mit benutzerdefiniertem Code | Jedes Format mit benutzerdefiniertem Code |
| Skalierbarkeit | [Abfragepartitionen](/azure/stream-analytics/stream-analytics-parallelization) | Durch die Clustergröße begrenzt | Durch Konfiguration der Databricks-Clusterskalierung begrenzt | Durch die Clustergröße begrenzt | Parallele Verarbeitung von bis zu 200 Funktions-App-Instanzen | Durch die Kapazität des App Service-Plans begrenzt |
| Unterstützung für verspäteten Eingang und Behandlung von Ereignissen mit falscher Reihenfolge | JA | Ja | Ja | JA | Nein  | Nein  |

Weitere Informationen:

- [Choosing a real-time message ingestion technology](./real-time-ingestion.md) (Auswählen einer Technologie für die Echtzeiterfassung von Nachrichten)
- [Verarbeitung in Echtzeit](../big-data/real-time-processing.md)
