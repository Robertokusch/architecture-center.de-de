---
title: Konfigurieren von Azure Databricks zum Senden von Metriken an Azure Monitor
description: Eine Scala-Bibliothek, um die Überwachung von Metriken und Protokollierungsdaten in Azure Log Analytics zu ermöglichen
author: petertaylor9999
ms.date: 03/26/2019
ms.openlocfilehash: af6b6433f87964ac60c179ecf498e54129344126
ms.sourcegitcommit: 9854bd27fb5cf92041bbfb743d43045cd3552a69
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/27/2019
ms.locfileid: "58503406"
---
<!-- markdownlint-disable MD040 -->

# <a name="configure-azure-databricks-to-send-metrics-to-azure-monitor"></a>Konfigurieren von Azure Databricks zum Senden von Metriken an Azure Monitor

In diesem Artikel wird gezeigt, wie Sie einen Azure Databricks-Cluster so konfigurieren, dass Metriken an einen [Log Analytics-Arbeitsbereich ](/azure/azure-monitor/platform/manage-access) gesendet werden. Dazu wird die [Azure Databricks-Überwachungsbibliothek](https://github.com/mspnp/spark-monitoring) verwendet, die auf GitHub verfügbar ist. Es wird empfohlen, sich mit Java, Scala und Maven vertraut zu machen.

## <a name="about-the-azure-databricks-monitoring-library"></a>Informationen zur Azure Databricks-Überwachungsbibliothek

Die [GitHub-Repository](https://github.com/mspnp/spark-monitoring) für die Azure Databricks-Überwachungsbibliothek hat die folgende Verzeichnisstruktur:

```
/src  
    /spark-jobs  
    /spark-listeners-loganalytics  
    /spark-listeners  
    /pom.xml  
```

Das Verzeichnis **spark-jobs** ist eine Spark-Beispielanwendung, die zeigt, wie ein Zähler für Spark-Anwendungsmetriken implementiert wird.

Das Verzeichnis **spark-listeners** enthält Funktionen, die es Azure Databricks ermöglichen, Apache Spark-Ereignisse auf Dienstebene an einen Azure Log Analytics-Arbeitsbereich zu senden. Azure Databricks ist ein auf Apache Spark basierender Dienst, der eine Reihe strukturierter APIs für die Batchverarbeitung von Daten mit Datasets, DataFrames und SQL enthält. In Apache Spark 2.0 wurde Unterstützung für [Structured Streaming](https://spark.apache.org/docs/latest/structured-streaming-programming-guide.html) hinzugefügt, eine Datenstromverarbeitungs-API, die auf Batchverarbeitungs-APIs von Spark basiert.

Das Verzeichnis **spark-listeners-loganalytics** enthält eine Senke für Spark-Listener, eine Senke für DropWizard und einen Client für einen Azure Log Analytics-Arbeitsbereich. Dieses Verzeichnis enthält auch einen log4j-Appender für Ihre Apache Spark-Anwendungsprotokolle.

Die Verzeichnisse **spark-listeners-loganalytics** und **spark-listeners** enthalten den Code zum Erstellen der beiden JAR-Dateien, die im Databricks-Cluster bereitgestellt werden. Das Verzeichnis **spark-listeners** enthält das Verzeichnis **scripts** mit einem Skript für die Initialisierung von Clusterknoten, um die JAR-Dateien aus einem Stagingverzeichnis im Dateisystem von Azure Databricks auf Ausführungsknoten zu kopieren.

Die Datei **pom.xml** ist die wichtigste Maven-Builddatei für das gesamte Projekt.

## <a name="prerequisites"></a>Voraussetzungen

Stellen Sie als Erstes die folgenden Azure-Ressourcen bereit:

- Einen Azure Databricks-Arbeitsbereich. Weitere Informationen finden Sie unter [Schnellstart: Ausführen eines Spark-Auftrags in Azure Databricks über das Azure-Portal](/azure/azure-databricks/quickstart-create-databricks-workspace-portal).
- Einen Log Analytics-Arbeitsbereich Siehe [Erstellen eines Log Analytics-Arbeitsbereichs im Azure-Portal](/azure/azure-monitor/learn/quick-create-workspace).

Installieren Sie als Nächstes die [Azure Databricks CLI](https://docs.databricks.com/user-guide/dev-tools/databricks-cli.html#install-the-cli). Ein persönliches Azure Databricks-Zugriffstoken ist für die Nutzung der CLI erforderlich. Anweisungen hierzu finden Sie unter [Generieren eines Tokens](https://docs.azuredatabricks.net/api/latest/authentication.html#token-management).

Rufen Sie Ihre Log Analytics-Arbeitsbereichs-ID und den Arbeitsbereichsschlüssel ab. Anweisungen finden Sie unter [Abrufen von Arbeitsbereichs-ID und -Schlüssel](/azure/azure-monitor/platform/agent-windows#obtain-workspace-id-and-key). Sie benötigen diese Werte, wenn Sie den Databricks-Cluster konfigurieren.

Um die Überwachungsbibliothek zu erstellen, benötigen Sie eine Java-IDE mit den folgenden Ressourcen:

- [Java Development Kit (JDK), Version 1.8](http://www.oracle.com/technetwork/java/javase/downloads/index.html)
- [Scala Language SDK 2.11](https://www.scala-lang.org/download/)
- [Apache Maven 3.5.4](http://maven.apache.org/download.cgi)

## <a name="build-the-azure-databricks-monitoring-library"></a>Erstellen der Azure Databricks-Überwachungsbibliothek

Um die Azure Databricks-Überwachungsbibliothek zu erstellen, führen Sie die folgenden Schritte aus:

1. Klonen oder forken Sie das [GitHub-Repository](https://github.com/mspnp/spark-monitoring), oder laden Sie es herunter.

1. Importieren Sie die Objektmodelldatei des Maven-Projekts _pom.xml_, die sich in Ihrem Projekt im Ordner **/src** befindet. Daraufhin werden drei Projekte importiert:

    - spark-jobs
    - spark-listeners
    - spark-listeners-loganalytics

1. Führen Sie die Buildphase für das Maven-**Paket** in Ihrer Java-IDE aus, um die JAR-Dateien für jedes dieser Projekte zu erstellen:

    |Project| JAR-Datei|
    |-------|---------|
    |spark-jobs|spark-jobs-1.0-SNAPSHOT.jar|
    |spark-listeners|spark-listeners-1.0-SNAPSHOT.jar|
    |spark-listeners-loganalytics|spark-listeners-loganalytics-1.0-SNAPSHOT.jar|

1. Verwenden Sie die Azure Databricks CLI zum Erstellen eines Verzeichnisses mit dem Namen **dbfs:/databricks/monitoring-staging**:  

    ```bash
    dbfs mkdirs dbfs:/databricks/monitoring-staging
    ```

1. Verwenden Sie die Azure Databricks CLI zum Kopieren von **/src/spark-listeners/scripts/listeners.sh** in das zuvor erstellte Verzeichnis:

    ```bash
    dbfs cp ./src/spark-listeners/scripts/listeners.sh dbfs:/databricks/monitoring-staging/listeners.sh
    ```

1. Verwenden Sie die Azure Databricks CLI zum Kopieren von **/src/spark-listeners/scripts/metrics.properties** in das zuvor erstellte Verzeichnis:

    ```bash
    dbfs cp <local path to metrics.properties> dbfs:/databricks/monitoring-staging/metrics.properties
    ```

1. Verwenden Sie die Azure Databricks CLI zum Kopieren von **spark-listeners-1.0-SNAPSHOT.jar** und **spark-listeners-loganalytics-1.0-SNAPSHOT.jar** in das zuvor erstellte Verzeichnis:

    ```bash
    dbfs cp ./src/spark-listeners/target/spark-listeners-1.0-SNAPSHOT.jar dbfs:/databricks/monitoring-staging/spark-listeners-1.0-SNAPSHOT.jar
    dbfs cp ./src/spark-listeners-loganalytics/target/spark-listeners-loganalytics-1.0-SNAPSHOT.jar dbfs:/databricks/monitoring-staging/spark-listeners-loganalytics-1.0-SNAPSHOT.jar
    ```

## <a name="create-and-configure-an-azure-databricks-cluster"></a>Erstellen und Konfigurieren eines Azure Databricks-Clusters

Führen Sie zum Erstellen und Konfigurieren eines Azure Databricks-Clusters die folgenden Schritte aus:

1. Navigieren Sie im Azure-Portal zu Ihrem Azure Databricks-Arbeitsbereich.
1. Klicken Sie auf der Startseite auf **Neuer Cluster**.
1. Geben Sie im Textfeld **Clustername** einen Namen für den Cluster ein.
1. Wählen Sie **4.3** oder höher in der Dropdownliste **Databricks Runtime-Version** aus.
1. Klicken Sie unter **Erweiterte Optionen**auf die Registerkarte **Spark**. Geben Sie die folgenden Name/Wert-Paare in der Textfeld **Spark Config** (Spark-Konfiguration)ein:

    ```
    spark.extraListeners com.databricks.backend.daemon.driver.DBCEventLoggingListener,org.apache.spark.listeners.UnifiedSparkListener
    spark.unifiedListener.sink org.apache.spark.listeners.sink.loganalytics.LogAnalyticsListenerSink
    spark.unifiedListener.logBlockUpdates false
    ```

1. Geben Sie weiterhin auf der Registerkarte **Spark** die folgenden Werte in das Textfeld **Environment Variables** (Umgebungsvariablen) ein:

    ```
    LOG_ANALYTICS_WORKSPACE_ID=[your Azure Log Analytics workspace ID]
    LOG_ANALYTICS_WORKSPACE_KEY=[your Azure Log Analytics workspace key]
    ```

    ![Screenshot der Databricks-Benutzeroberfläche](./_images/create-cluster1.png)

1. Klicken Sie weiterhin im Abschnitt **Erweiterte Optionen** auf die Registerkarte **Init Scripts** (Initialisierungsskripts).
1. Wählen Sie in der Dropdownliste **Destination** (Ziel) den Eintrag **DBFS** aus.
1. Geben Sie unter **Init Script Path** (Pfad des Initialisierungsskript) `dbfs:/databricks/monitoring-staging/listeners.sh` ein.
1. Klicken Sie auf **Hinzufügen**.

    ![Screenshot der Databricks-Benutzeroberfläche](./_images/create-cluster2.png)

1. Wählen Sie **Create Cluster** (Cluster erstellen), um den Cluster zu erstellen.

## <a name="view-metrics"></a>Anzeigen von Metriken

Nachdem Sie diese Schritte abgeschlossen haben, streamt Ihr Databricks-Cluster einige Metrikdaten zum Cluster selbst an Azure Monitor. Diese Protokolldaten stehen in Ihrem Azure Log Analytics-Arbeitsbereich unter dem Schema „Aktiv | Benutzerdefinierte Protokolle | SparkMetric_C“ zur Verfügung. Weitere Informationen zu den Arten von Metriken, die protokolliert werden, finden Sie in der Dokumentation zu Dropwizard unter [Metrics Core](https://metrics.dropwizard.io/4.0.0/manual/core.html) (Hauptmetriken). Den Metriktyp, wie z.B. Messgerät, Zähler oder das Histogramm, wird in das Feld „Typ“ geschrieben.

Darüber hinaus streamt die Bibliothek Ereignisse auf Apache Spark-Ebene und Metriken für Spark Structured Streaming aus Ihren Aufträgen an Azure Monitor. Für diese Ereignisse und Metriken müssen Sie keine Änderungen an Ihrem Anwendungscode vornehmen. Spark Structured StreamingProtokolldaten stehen in unter dem Schema „Aktiv | Benutzerdefinierte Protokolle | SparkListenerEvent_CL“ zur Verfügung.

![Screenshot eines Log Analytics-Arbeitsbereichs](./_images/workspace.png)

Weitere Informationen zum Anzeigen von Protokolle finden Sie unter [Anzeigen und Analysieren von Protokolldaten in Azure Monitor](/azure/azure-monitor/log-query/portals).

## <a name="next-steps"></a>Nächste Schritte

Dieser Artikel beschreibt, wie Sie Ihren Cluster so konfigurieren, dass er Metriken an Azure Monitor sendet. Der nächste Artikel zeigt, wie Sie die Azure Databricks-Überwachungsbibliothek verwenden können, um Metriken und Protokolle von Anwendungen zu senden.

> [!div class="nextstepaction"]
> [Senden von Anwendungsprotokollen an Azure Monitor](./application-logs.md)
