---
title: Senden von Azure Databricks-Anwendungsprotokollen an Azure Monitor
description: Informationen zum Senden benutzerdefinierter Protokolle und Metriken aus Azure Databricks an Azure Monitor
author: petertaylor9999
ms.date: 03/26/2019
ms.openlocfilehash: ea67122d7871663e8aaf42b7af0043492f63b6b1
ms.sourcegitcommit: 579c39ff4b776704ead17a006bf24cd4cdc65edd
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/17/2019
ms.locfileid: "59639183"
---
# <a name="send-azure-databricks-application-logs-to-azure-monitor"></a>Senden von Azure Databricks-Anwendungsprotokollen an Azure Monitor

In diesem Artikel wird gezeigt, wie Sie Anwendungsprotokolle und Metriken von Azure Databricks an einen [Log Analytics-Arbeitsbereich](/azure/azure-monitor/platform/manage-access) senden können. Dazu wird die [Azure Databricks-Überwachungsbibliothek](https://github.com/mspnp/spark-monitoring) verwendet, die auf GitHub verfügbar ist.

## <a name="prerequisites"></a>Voraussetzungen

Konfigurieren Sie Ihren Azure Databricks Cluster für die Verwendung der Überwachungsbibliothek, wie unter [Konfigurieren von Azure Databricks zum Senden von Metriken an Azure Monitor][config-cluster] beschrieben.

> [!NOTE]
> Die Überwachungsbibliothek streamt Ereignisse auf Apache Spark-Ebene und Metriken für Spark Structured Streaming aus Ihren Aufträgen an Azure Monitor. Für diese Ereignisse und Metriken müssen Sie keine Änderungen an Ihrem Anwendungscode vornehmen.

## <a name="send-application-metrics-using-dropwizard"></a>Senden von Anwendungsmetriken mithilfe von Dropwizard

Spark verwendet ein konfigurierbares System für Metriken, das auf der Dropwizard-Bibliothek für Metriken basiert. Weitere Informationen finden Sie unter [Metriken](https://spark.apache.org/docs/latest/monitoring.html#metrics) in der Spark-Dokumentation.

Um Anwendungsmetriken aus dem Anwendungscode von Azure Databricks an Azure Monitor zu senden, führen Sie diese Schritte aus:

1. Erstellen Sie die JAR-Datei **spark-listeners-loganalytics-1.0-SNAPSHOT.jar** entsprechend den Anweisungen unter [Erstellen der Azure Databricks-Überwachungsbibliothek][build-lib].

1. Erstellen Sie in Ihrem Anwendungscode Dropwizard-[Messgeräte oder -Zähler](https://metrics.dropwizard.io/4.0.0/manual/core.html). Sie können die `UserMetricsSystem`-Klasse verwenden, die in der Überwachungsbibliothek definiert ist. Im folgenden Beispiel wird ein Zähler namens `counter1` erstellt.

    ```Scala
    import org.apache.spark.metrics.UserMetricsSystems
    import org.apache.spark.sql.SparkSession

    object StreamingQueryListenerSampleJob  {

      private final val METRICS_NAMESPACE = "samplejob"
      private final val COUNTER_NAME = "counter1"

      def main(args: Array[String]): Unit = {

        val spark = SparkSession
          .builder
          .getOrCreate

        val driverMetricsSystem = UserMetricsSystems
            .getMetricSystem(METRICS_NAMESPACE, builder => {
              builder.registerCounter(COUNTER_NAME)
            })

        driverMetricsSystem.counter(COUNTER_NAME).inc(5)
      }
    }
    ```

    Die Überwachungsbibliothek enthält eine [Beispielanwendung][sample-app], die die Verwendung der `UserMetricsSystem`-Klasse demonstriert.

## <a name="send-application-logs-using-log4j"></a>Senden von Anwendungsprotokollen mit Log4j

Um Ihre Azure Databricks Anwendungsprotokolle mit dem [Log4j-Appender](https://logging.apache.org/log4j/2.x/manual/appenders.html) in der Bibliothek an Azure Log Analytics zu senden, führen Sie diese Schritte aus:

1. Erstellen Sie die JAR-Datei **spark-listeners-loganalytics-1.0-SNAPSHOT.jar** entsprechend den Anweisungen unter [Erstellen der Azure Databricks-Überwachungsbibliothek][build-lib].

1. Erstellen Sie für Ihre Anwendung die [Konfigurationsdatei](https://logging.apache.org/log4j/2.x/manual/configuration.html) **log4j.properties**. Fügen Sie die folgenden Konfigurationseigenschaften hinzu. Ersetzen Sie an den angegebenen Stellen den Namen Ihres Anwendungspakets und die Protokollebene:

    ```YAML
    log4j.appender.A1=com.microsoft.pnp.logging.loganalytics.LogAnalyticsAppender
    log4j.appender.A1.layout=com.microsoft.pnp.logging.JSONLayout
    log4j.appender.A1.layout.LocationInfo=false
    log4j.additivity.<your application package name>=false
    log4j.logger.<your application package name>=<log level>, A1
    ```

    [Hier][log4j.properties] finden Sie eine Beispielkonfigurationsdatei.

1. Fügen Sie in Ihren Anwendungscode das Projekt **spark-listeners-loganalytics** ein, und importieren Sie `com.microsoft.pnp.logging.Log4jconfiguration` in Ihren Anwendungscode.

    ```Scala
    import com.microsoft.pnp.logging.Log4jConfiguration
    ```

1. Konfigurieren Sie Log4j mithilfe der Datei **log4j.properties**, die Sie in Schritt 3 erstellt haben:

    ```Scala
    getClass.getResourceAsStream("<path to file in your JAR file>/log4j.properties")) {
          stream => {
            Log4jConfiguration.configure(stream)
          }
    }
    ```

1. Fügen Sie Apache Spark-Protokollnachrichten nach Bedarf auf der entsprechenden Ebene im Code hinzu. Verwenden Sie beispielsweise die `logDebug`-Methode, um eine auf Debuggen bezogene Protokollnachricht zu senden. Weitere Informationen finden Sie unter [Protokollierung][spark-logging] in der Spark-Dokumentation.

    ```Scala
    logTrace("Trace message")
    logDebug("Debug message")
    logInfo("Info message")
    logWarning("Warning message")
    logError("Error message")
    ```

## <a name="run-the-sample-application"></a>Ausführen der Beispielanwendung

Die Überwachungsbibliothek enthält eine [Beispielanwendung][sample-app], die veranschaulicht, wie sowohl Anwendungsmetriken als auch Anwendungsprotokolle an Azure Monitor gesendet werden. So führen Sie das Beispiel aus:

1. Erstellen Sie in der Überwachungsbibliothek das Projekt **spark-jobs** entsprechend den Anweisungen unter [Erstellen der Azure Databricks-Überwachungsbibliothek][build-lib].

1. Wechseln Sie zu Ihrem Databricks-Arbeitsbereich, und erstellen Sie gemäß den Anweisungen [hier](https://docs.azuredatabricks.net/user-guide/jobs.html#create-a-job) einen neuen Auftrag.

1. Wählen Sie auf der Detailseite des Auftrags **Set JAR** (JAR-Datei festlegen) aus.

1. Laden Sie die JAR-Datei aus `/src/spark-jobs/target/spark-jobs-1.0-SNAPSHOT.jar` hoch.

1. Geben Sie `com.microsoft.pnp.samplejob.StreamingQueryListenerSampleJob` für **Main-Klasse** ein.

1. Wählen Sie einen Cluster aus, der bereits für die Verwendung der Überwachungsbibliothek konfiguriert ist. Siehe [Konfigurieren von Azure Databricks zum Senden von Metriken an Azure Monitor][config-cluster].

Nachdem der Auftrag ausgeführt wurde, können Sie die Anwendungsprotokolle und Metriken in Ihrem Log Analytics-Arbeitsbereich anzeigen.

Anwendungsprotokolle werden unter „SparkLoggingEvent_CL“ angezeigt:

```Kusto
SparkLoggingEvent_CL | where logger_name_s contains "com.microsoft.pnp"
```

Anwendungsmetriken werden unter „SparkMetric_CL“ angezeigt:

```Kusto
SparkMetric_CL | where name_s contains "rowcounter" | limit 50
```

## <a name="next-steps"></a>Nächste Schritte

Stellen Sie das Dashboard für die Leistungsüberwachung bereit, das diese Codebibliothek begleitet, um Leistungsprobleme in Ihren Azure Databricks-Workloads in der Produktion zu beheben.

> [!div class="nextstepaction"]
> [Visualisieren von Azure Databricks-Metriken mithilfe von Dashboards](./dashboards.md)

<!-- links -->

[build-lib]: ./configure-cluster.md##build-the-azure-databricks-monitoring-library
[config-cluster]: ./configure-cluster.md
[log4j.properties]: https://github.com/mspnp/spark-monitoring/blob/master/src/spark-jobs/src/main/resources/com/microsoft/pnp/samplejob/log4j.properties
[sample-app]: https://github.com/mspnp/spark-monitoring/tree/master/src/spark-jobs
[spark-logging]: https://spark.apache.org/docs/2.3.0/api/java/org/apache/spark/internal/Logging.html