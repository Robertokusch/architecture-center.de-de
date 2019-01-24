---
title: Auswählen einer Batchverarbeitungstechnologie
description: ''
author: zoinerTejada
ms.date: 11/03/2018
ms.topic: guide
ms.service: architecture-center
ms.subservice: cloud-fundamentals
ms.openlocfilehash: 53f8b233b0e0c1ff83a72a04b2707caa528d6f6b
ms.sourcegitcommit: 1b50810208354577b00e89e5c031b774b02736e2
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/23/2019
ms.locfileid: "54486453"
---
# <a name="choosing-a-batch-processing-technology-in-azure"></a>Auswählen einer Batchverarbeitungstechnologie in Azure

Big Data-Lösungen nutzen häufig Batchaufträge mit langer Ausführungszeit, um Daten für die Analyse zu filtern, zu aggregieren oder anderweitig vorzubereiten. In der Regel umfassen diese Aufträge das Lesen von Quelldateien aus skalierbarem Speicher (etwa HDFS, Azure Data Lake Store und Azure Storage), das Verarbeiten dieser Dateien und das Schreiben der Ausgabe in neue Dateien in skalierbarem Speicher.

Die wichtigste Anforderung bei diesen Batchverarbeitungs-Engines ist die Möglichkeit zur Skalierung von Berechnungen, um große Datenvolumen zu verarbeiten. Im Gegensatz zur Echtzeitverarbeitung werden bei der Batchverarbeitung Wartezeiten (die Zeit zwischen der Datenerfassung und der Berechnung eines Ergebnisses) erwartet, die in Minuten oder Stunden angegeben werden.

## <a name="technology-choices-for-batch-processing"></a>Technologieoptionen für die Batchverarbeitung

### <a name="azure-sql-data-warehouse"></a>Azure SQL Data Warehouse

[SQL Data Warehouse](/azure/sql-data-warehouse/) ist ein verteiltes System für die Analyse großer Datenmengen. Es unterstützt massive Parallelverarbeitung (Massive Parallel Processing, MPP), die die Ausführung von Hochleistungsanalysen ermöglicht. Sie sollten SQL Data Warehouse verwenden, wenn Sie große Datenmengen (mehr als 1 TB) haben und eine Analyseworkload ausführen, die von der Parallelität profitiert.

### <a name="azure-data-lake-analytics"></a>Azure Data Lake Analytics

[Data Lake Analytics](/azure/data-lake-analytics/data-lake-analytics-overview) ist ein bedarfsgesteuerter Dienst für Analyseaufträge. Er ist für die verteilte Verarbeitung von sehr großen, in Azure Data Lake Store gespeicherten Datasets optimiert.

- Sprachen: [U-SQL](/azure/data-lake-analytics/data-lake-analytics-u-sql-get-started) (einschließlich Python-, R-, und C#-Erweiterungen).
- Lässt sich in Azure Data Lake Store, Azure Storage-Blobs, Azure SQL-Datenbank und SQL Data Warehouse integrieren.
- Das Preismodell ist auftragsbasiert.

### <a name="hdinsight"></a>HDInsight

HDInsight ist ein verwalteter Hadoop-Dienst. Verwenden Sie ihn zum Bereitstellen und Verwalten von Hadoop-Clustern in Azure. Für die Batchverarbeitung können Sie [Spark](/azure/hdinsight/spark/apache-spark-overview), [Hive](/azure/hdinsight/hadoop/hdinsight-use-hive), [Hive LLAP](/azure/hdinsight/interactive-query/apache-interactive-query-get-started) und [MapReduce](/azure/hdinsight/hadoop/hdinsight-use-mapreduce) verwenden.

- Sprachen: R, Python, Java, Scala, SQL
- Kerberos-Authentifizierung mit Active Directory, Apache Ranger-basierte Zugriffssteuerung
- Bietet Ihnen volle Kontrolle über den Hadoop-Cluster

### <a name="azure-databricks"></a>Azure Databricks

[Azure Databricks](/azure/azure-databricks/) ist eine Apache Spark-basierte Analyseplattform. Sie können es sich als „Spark-as-a-Service“ vorstellen. Es ist die einfachste Möglichkeit zum Verwenden von Spark auf der Azure-Plattform.

- Sprachen: R, Python, Java, Scala, Spark SQL
- Schnellere Startzeiten für Cluster, automatische Beendigung, automatische Skalierung.
- Verwaltet den Spark-Cluster für Sie.
- Integrierte Integration in Azure Blob Storage, Azure Data Lake Storage (ADLS), Azure SQL Data Warehouse (SQL DW) und andere Dienste. Siehe [Datenquellen](https://docs.azuredatabricks.net/spark/latest/data-sources/index.html).
- Benutzerauthentifizierung mit Azure Active Directory
- Webbasierte [Notebooks](https://docs.azuredatabricks.net/user-guide/notebooks/index.html) für Zusammenarbeit und das Durchsuchen von Daten
- Unterstützt [GPU-fähige Cluster](https://docs.azuredatabricks.net/user-guide/clusters/gpu.html)

### <a name="azure-distributed-data-engineering-toolkit"></a>Azure Distributed Data Engineering Toolkit

Das [Distributed Data Engineering Toolkit](https://github.com/azure/aztk) (AZTK) ist ein Tool für die Bereitstellung von bedarfsgesteuerten Spark-Instanzen in Docker-Clustern in Azure.

AZTK ist kein Azure-Dienst. Es handelt sich vielmehr um ein clientseitiges Tool mit einer CLI und einer Python-SDK-Schnittstelle, das auf Azure Batch basiert. Diese Option bietet Ihnen beim Bereitstellen eines Spark-Clusters die größtmögliche Kontrolle über die Infrastruktur.

- Verwenden Sie Ihr eigenes Docker-Image.
- Verwenden Sie virtuelle Computer mit niedriger Priorität für einen Rabatt von 80%.
- Cluster im gemischten Modus, die virtuelle Computer mit niedriger Priorität sowie dedizierte virtuelle Computer verwenden.
- Integrierte Unterstützung für Azure Blob Storage und Azure Data Lake-Verbindung.

## <a name="key-selection-criteria"></a>Wichtige Auswahlkriterien

Beantworten Sie die folgenden Fragen, um die Auswahl einzuschränken:

- Möchten Sie einen verwalteten Dienst verwenden, anstatt Ihre eigenen Server zu verwalten?

- Möchten Sie Batchverarbeitungslogik deklarativ oder imperativ erstellen?

- Führen Sie Batchaufträge schubweise aus? Falls ja, ziehen Sie Optionen in Erwägung, bei denen Sie den Cluster automatisch beenden können bzw. bei denen die Kosten pro Batchauftrag abgerechnet werden.

- Müssen Sie bei der Batchverarbeitung auch relationale Datenspeicher abfragen, etwa zum Nachschlagen von Referenzdaten? Falls ja, ziehen Sie Optionen in Erwägung, die das Abfragen von externen relationalen Speichern ermöglichen.

## <a name="capability-matrix"></a>Funktionsmatrix

In den folgenden Tabellen sind die Hauptunterschiede in Bezug auf die Funktionen zusammengefasst.

### <a name="general-capabilities"></a>Allgemeine Funktionen

<!-- markdownlint-disable MD033 -->

| | Azure Data Lake Analytics | Azure SQL Data Warehouse | HDInsight | Azure Databricks |
| --- | --- | --- | --- | --- | --- |
| Verwalteter Dienst | JA | JA | Ja<sup>1</sup> | JA |
| Relationaler Datenspeicher | JA | JA | Nein  | Nein  |
| Preismodell | Pro Batchauftrag | Nach Clusterstunde | Nach Clusterstunde | Databricks-Einheit<sup>2</sup> + Clusterstunde |

[1] Mit manueller Konfiguration und Skalierung

[2] Eine Databricks-Einheit (Databricks Unit, DBU) ist eine Einheit für die Verarbeitungskapazität pro Stunde.

### <a name="capabilities"></a>Funktionen

| | Azure Data Lake Analytics | SQL Data Warehouse | HDInsight mit Spark | HDInsight mit Hive | HDInsight mit Hive LLAP | Azure Databricks |
| --- | --- | --- | --- | --- | --- | --- |
| Automatische Skalierung | Nein  | Nein  | Nein  | Nein  | Nein  | JA |
| Granularität bei der horizontalen Skalierung  | Pro Auftrag | Pro Cluster | Pro Cluster | Pro Cluster | Pro Cluster | Pro Cluster |
| Speicherinternes Zwischenspeichern | Nein  | Ja | JA | Nein | Ja | JA |
| Abfragen über externe relationale Speicher | JA | Nein | Ja | Nein  | Nein  | JA |
| Authentifizierung  | Azure AD | SQL/Azure AD | Nein  | Azure AD<sup>1</sup> | Azure AD<sup>1</sup> | Azure AD |
| Überwachung  | JA | JA | Nein  | Ja<sup>1</sup> | Ja<sup>1</sup> | JA |
| Sicherheit auf Zeilenebene | Nein  | Nein  | Nein  | Ja<sup>1</sup> | Ja<sup>1</sup> | Nein  |
| Unterstützung von Firewalls | JA | Ja | JA | Ja<sup>2</sup> | Ja<sup>2</sup> | Nein  |
| Dynamische Datenmaskierung | Nein  | Nein  | Nein  | Ja<sup>1</sup> | Ja<sup>1</sup> | Nein  |

<!-- markdownlint-enable MD033 -->

[1] Erfordert die Verwendung eines [in die Domäne eingebundenen HDInsight-Clusters](/azure/hdinsight/domain-joined/apache-domain-joined-introduction).

[2] Unterstützt bei [Verwendung in einem virtuellen Azure-Netzwerk](/azure/hdinsight/hdinsight-extend-hadoop-virtual-network)
