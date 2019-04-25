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
# <a name="send-azure-databricks-application-logs-to-azure-monitor"></a><span data-ttu-id="c464c-103">Senden von Azure Databricks-Anwendungsprotokollen an Azure Monitor</span><span class="sxs-lookup"><span data-stu-id="c464c-103">Send Azure Databricks application logs to Azure Monitor</span></span>

<span data-ttu-id="c464c-104">In diesem Artikel wird gezeigt, wie Sie Anwendungsprotokolle und Metriken von Azure Databricks an einen [Log Analytics-Arbeitsbereich](/azure/azure-monitor/platform/manage-access) senden können.</span><span class="sxs-lookup"><span data-stu-id="c464c-104">This article shows how to send application logs and metrics from Azure Databricks to a [Log Analytics workspace](/azure/azure-monitor/platform/manage-access).</span></span> <span data-ttu-id="c464c-105">Dazu wird die [Azure Databricks-Überwachungsbibliothek](https://github.com/mspnp/spark-monitoring) verwendet, die auf GitHub verfügbar ist.</span><span class="sxs-lookup"><span data-stu-id="c464c-105">It uses the [Azure Databricks Monitoring Library](https://github.com/mspnp/spark-monitoring), which is available on GitHub.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="c464c-106">Voraussetzungen</span><span class="sxs-lookup"><span data-stu-id="c464c-106">Prerequisites</span></span>

<span data-ttu-id="c464c-107">Konfigurieren Sie Ihren Azure Databricks Cluster für die Verwendung der Überwachungsbibliothek, wie unter [Konfigurieren von Azure Databricks zum Senden von Metriken an Azure Monitor][config-cluster] beschrieben.</span><span class="sxs-lookup"><span data-stu-id="c464c-107">Configure your Azure Databricks cluster to use the monitoring library, as described in [Configure Azure Databricks to send metrics to Azure Monitor][config-cluster].</span></span>

> [!NOTE]
> <span data-ttu-id="c464c-108">Die Überwachungsbibliothek streamt Ereignisse auf Apache Spark-Ebene und Metriken für Spark Structured Streaming aus Ihren Aufträgen an Azure Monitor.</span><span class="sxs-lookup"><span data-stu-id="c464c-108">The monitoring library streams Apache Spark level events and Spark Structured Streaming metrics from your jobs to Azure Monitor.</span></span> <span data-ttu-id="c464c-109">Für diese Ereignisse und Metriken müssen Sie keine Änderungen an Ihrem Anwendungscode vornehmen.</span><span class="sxs-lookup"><span data-stu-id="c464c-109">You don't need to make any changes to your application code for these events and metrics.</span></span>

## <a name="send-application-metrics-using-dropwizard"></a><span data-ttu-id="c464c-110">Senden von Anwendungsmetriken mithilfe von Dropwizard</span><span class="sxs-lookup"><span data-stu-id="c464c-110">Send application metrics using Dropwizard</span></span>

<span data-ttu-id="c464c-111">Spark verwendet ein konfigurierbares System für Metriken, das auf der Dropwizard-Bibliothek für Metriken basiert.</span><span class="sxs-lookup"><span data-stu-id="c464c-111">Spark uses a configurable metrics system based on the Dropwizard Metrics Library.</span></span> <span data-ttu-id="c464c-112">Weitere Informationen finden Sie unter [Metriken](https://spark.apache.org/docs/latest/monitoring.html#metrics) in der Spark-Dokumentation.</span><span class="sxs-lookup"><span data-stu-id="c464c-112">For more information, see [Metrics](https://spark.apache.org/docs/latest/monitoring.html#metrics) in the Spark documentation.</span></span>

<span data-ttu-id="c464c-113">Um Anwendungsmetriken aus dem Anwendungscode von Azure Databricks an Azure Monitor zu senden, führen Sie diese Schritte aus:</span><span class="sxs-lookup"><span data-stu-id="c464c-113">To send application metrics from Azure Databricks application code to Azure Monitor, follow these steps:</span></span>

1. <span data-ttu-id="c464c-114">Erstellen Sie die JAR-Datei **spark-listeners-loganalytics-1.0-SNAPSHOT.jar** entsprechend den Anweisungen unter [Erstellen der Azure Databricks-Überwachungsbibliothek][build-lib].</span><span class="sxs-lookup"><span data-stu-id="c464c-114">Build the **spark-listeners-loganalytics-1.0-SNAPSHOT.jar** JAR file as described in [Build the Azure Databricks Monitoring Library][build-lib].</span></span>

1. <span data-ttu-id="c464c-115">Erstellen Sie in Ihrem Anwendungscode Dropwizard-[Messgeräte oder -Zähler](https://metrics.dropwizard.io/4.0.0/manual/core.html).</span><span class="sxs-lookup"><span data-stu-id="c464c-115">Create Dropwizard [gauges or counters](https://metrics.dropwizard.io/4.0.0/manual/core.html) in your application code.</span></span> <span data-ttu-id="c464c-116">Sie können die `UserMetricsSystem`-Klasse verwenden, die in der Überwachungsbibliothek definiert ist.</span><span class="sxs-lookup"><span data-stu-id="c464c-116">You can use the `UserMetricsSystem` class defined in the monitoring library.</span></span> <span data-ttu-id="c464c-117">Im folgenden Beispiel wird ein Zähler namens `counter1` erstellt.</span><span class="sxs-lookup"><span data-stu-id="c464c-117">The following example creates a counter named `counter1`.</span></span>

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

    <span data-ttu-id="c464c-118">Die Überwachungsbibliothek enthält eine [Beispielanwendung][sample-app], die die Verwendung der `UserMetricsSystem`-Klasse demonstriert.</span><span class="sxs-lookup"><span data-stu-id="c464c-118">The monitoring library includes a [sample application][sample-app] that demonstrates how to use the `UserMetricsSystem` class.</span></span>

## <a name="send-application-logs-using-log4j"></a><span data-ttu-id="c464c-119">Senden von Anwendungsprotokollen mit Log4j</span><span class="sxs-lookup"><span data-stu-id="c464c-119">Send application logs using Log4j</span></span>

<span data-ttu-id="c464c-120">Um Ihre Azure Databricks Anwendungsprotokolle mit dem [Log4j-Appender](https://logging.apache.org/log4j/2.x/manual/appenders.html) in der Bibliothek an Azure Log Analytics zu senden, führen Sie diese Schritte aus:</span><span class="sxs-lookup"><span data-stu-id="c464c-120">To send your Azure Databricks application logs to Azure Log Analytics using the [Log4j appender](https://logging.apache.org/log4j/2.x/manual/appenders.html) in the library, follow these steps:</span></span>

1. <span data-ttu-id="c464c-121">Erstellen Sie die JAR-Datei **spark-listeners-loganalytics-1.0-SNAPSHOT.jar** entsprechend den Anweisungen unter [Erstellen der Azure Databricks-Überwachungsbibliothek][build-lib].</span><span class="sxs-lookup"><span data-stu-id="c464c-121">Build the **spark-listeners-loganalytics-1.0-SNAPSHOT.jar** JAR file as described in [Build the Azure Databricks Monitoring Library][build-lib].</span></span>

1. <span data-ttu-id="c464c-122">Erstellen Sie für Ihre Anwendung die [Konfigurationsdatei](https://logging.apache.org/log4j/2.x/manual/configuration.html) **log4j.properties**.</span><span class="sxs-lookup"><span data-stu-id="c464c-122">Create a **log4j.properties** [configuration file](https://logging.apache.org/log4j/2.x/manual/configuration.html) for your application.</span></span> <span data-ttu-id="c464c-123">Fügen Sie die folgenden Konfigurationseigenschaften hinzu.</span><span class="sxs-lookup"><span data-stu-id="c464c-123">Include the following configuration properties.</span></span> <span data-ttu-id="c464c-124">Ersetzen Sie an den angegebenen Stellen den Namen Ihres Anwendungspakets und die Protokollebene:</span><span class="sxs-lookup"><span data-stu-id="c464c-124">Substitute your application package name and log level where indicated:</span></span>

    ```YAML
    log4j.appender.A1=com.microsoft.pnp.logging.loganalytics.LogAnalyticsAppender
    log4j.appender.A1.layout=com.microsoft.pnp.logging.JSONLayout
    log4j.appender.A1.layout.LocationInfo=false
    log4j.additivity.<your application package name>=false
    log4j.logger.<your application package name>=<log level>, A1
    ```

    <span data-ttu-id="c464c-125">[Hier][log4j.properties] finden Sie eine Beispielkonfigurationsdatei.</span><span class="sxs-lookup"><span data-stu-id="c464c-125">You can find a sample configuration file [here][log4j.properties].</span></span>

1. <span data-ttu-id="c464c-126">Fügen Sie in Ihren Anwendungscode das Projekt **spark-listeners-loganalytics** ein, und importieren Sie `com.microsoft.pnp.logging.Log4jconfiguration` in Ihren Anwendungscode.</span><span class="sxs-lookup"><span data-stu-id="c464c-126">In your application code, include the **spark-listeners-loganalytics** project, and import `com.microsoft.pnp.logging.Log4jconfiguration` to your application code.</span></span>

    ```Scala
    import com.microsoft.pnp.logging.Log4jConfiguration
    ```

1. <span data-ttu-id="c464c-127">Konfigurieren Sie Log4j mithilfe der Datei **log4j.properties**, die Sie in Schritt 3 erstellt haben:</span><span class="sxs-lookup"><span data-stu-id="c464c-127">Configure Log4j using the **log4j.properties** file you created in step 3:</span></span>

    ```Scala
    getClass.getResourceAsStream("<path to file in your JAR file>/log4j.properties")) {
          stream => {
            Log4jConfiguration.configure(stream)
          }
    }
    ```

1. <span data-ttu-id="c464c-128">Fügen Sie Apache Spark-Protokollnachrichten nach Bedarf auf der entsprechenden Ebene im Code hinzu.</span><span class="sxs-lookup"><span data-stu-id="c464c-128">Add Apache Spark log messages at the appropriate level in your code as required.</span></span> <span data-ttu-id="c464c-129">Verwenden Sie beispielsweise die `logDebug`-Methode, um eine auf Debuggen bezogene Protokollnachricht zu senden.</span><span class="sxs-lookup"><span data-stu-id="c464c-129">For example, use the `logDebug` method to send a debug log message.</span></span> <span data-ttu-id="c464c-130">Weitere Informationen finden Sie unter [Protokollierung][spark-logging] in der Spark-Dokumentation.</span><span class="sxs-lookup"><span data-stu-id="c464c-130">For more information, see [Logging][spark-logging] in the Spark documentation.</span></span>

    ```Scala
    logTrace("Trace message")
    logDebug("Debug message")
    logInfo("Info message")
    logWarning("Warning message")
    logError("Error message")
    ```

## <a name="run-the-sample-application"></a><span data-ttu-id="c464c-131">Ausführen der Beispielanwendung</span><span class="sxs-lookup"><span data-stu-id="c464c-131">Run the sample application</span></span>

<span data-ttu-id="c464c-132">Die Überwachungsbibliothek enthält eine [Beispielanwendung][sample-app], die veranschaulicht, wie sowohl Anwendungsmetriken als auch Anwendungsprotokolle an Azure Monitor gesendet werden.</span><span class="sxs-lookup"><span data-stu-id="c464c-132">The monitoring library includes a [sample application][sample-app] that demonstrates how to send both application metrics and application logs to Azure Monitor.</span></span> <span data-ttu-id="c464c-133">So führen Sie das Beispiel aus:</span><span class="sxs-lookup"><span data-stu-id="c464c-133">To run the sample:</span></span>

1. <span data-ttu-id="c464c-134">Erstellen Sie in der Überwachungsbibliothek das Projekt **spark-jobs** entsprechend den Anweisungen unter [Erstellen der Azure Databricks-Überwachungsbibliothek][build-lib].</span><span class="sxs-lookup"><span data-stu-id="c464c-134">Build the **spark-jobs** project in the monitoring library, as described in [Build the Azure Databricks Monitoring Library][build-lib].</span></span>

1. <span data-ttu-id="c464c-135">Wechseln Sie zu Ihrem Databricks-Arbeitsbereich, und erstellen Sie gemäß den Anweisungen [hier](https://docs.azuredatabricks.net/user-guide/jobs.html#create-a-job) einen neuen Auftrag.</span><span class="sxs-lookup"><span data-stu-id="c464c-135">Navigate to your Databricks workspace and create a new job, as described [here](https://docs.azuredatabricks.net/user-guide/jobs.html#create-a-job).</span></span>

1. <span data-ttu-id="c464c-136">Wählen Sie auf der Detailseite des Auftrags **Set JAR** (JAR-Datei festlegen) aus.</span><span class="sxs-lookup"><span data-stu-id="c464c-136">In the job detail page, select **Set JAR**.</span></span>

1. <span data-ttu-id="c464c-137">Laden Sie die JAR-Datei aus `/src/spark-jobs/target/spark-jobs-1.0-SNAPSHOT.jar` hoch.</span><span class="sxs-lookup"><span data-stu-id="c464c-137">Upload the JAR file from `/src/spark-jobs/target/spark-jobs-1.0-SNAPSHOT.jar`.</span></span>

1. <span data-ttu-id="c464c-138">Geben Sie `com.microsoft.pnp.samplejob.StreamingQueryListenerSampleJob` für **Main-Klasse** ein.</span><span class="sxs-lookup"><span data-stu-id="c464c-138">For **Main class**, enter `com.microsoft.pnp.samplejob.StreamingQueryListenerSampleJob`.</span></span>

1. <span data-ttu-id="c464c-139">Wählen Sie einen Cluster aus, der bereits für die Verwendung der Überwachungsbibliothek konfiguriert ist.</span><span class="sxs-lookup"><span data-stu-id="c464c-139">Select a cluster that is already configured to use the monitoring library.</span></span> <span data-ttu-id="c464c-140">Siehe [Konfigurieren von Azure Databricks zum Senden von Metriken an Azure Monitor][config-cluster].</span><span class="sxs-lookup"><span data-stu-id="c464c-140">See [Configure Azure Databricks to send metrics to Azure Monitor][config-cluster].</span></span>

<span data-ttu-id="c464c-141">Nachdem der Auftrag ausgeführt wurde, können Sie die Anwendungsprotokolle und Metriken in Ihrem Log Analytics-Arbeitsbereich anzeigen.</span><span class="sxs-lookup"><span data-stu-id="c464c-141">When the job runs, you can view the application logs and metrics in your Log Analytics workspace.</span></span>

<span data-ttu-id="c464c-142">Anwendungsprotokolle werden unter „SparkLoggingEvent_CL“ angezeigt:</span><span class="sxs-lookup"><span data-stu-id="c464c-142">Application logs appear under SparkLoggingEvent_CL:</span></span>

```Kusto
SparkLoggingEvent_CL | where logger_name_s contains "com.microsoft.pnp"
```

<span data-ttu-id="c464c-143">Anwendungsmetriken werden unter „SparkMetric_CL“ angezeigt:</span><span class="sxs-lookup"><span data-stu-id="c464c-143">Application metrics appear under SparkMetric_CL:</span></span>

```Kusto
SparkMetric_CL | where name_s contains "rowcounter" | limit 50
```

## <a name="next-steps"></a><span data-ttu-id="c464c-144">Nächste Schritte</span><span class="sxs-lookup"><span data-stu-id="c464c-144">Next steps</span></span>

<span data-ttu-id="c464c-145">Stellen Sie das Dashboard für die Leistungsüberwachung bereit, das diese Codebibliothek begleitet, um Leistungsprobleme in Ihren Azure Databricks-Workloads in der Produktion zu beheben.</span><span class="sxs-lookup"><span data-stu-id="c464c-145">Deploy the performance monitoring dashboard that accompanies this code library to troubleshoot performance issues in your production Azure Databricks workloads.</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="c464c-146">Visualisieren von Azure Databricks-Metriken mithilfe von Dashboards</span><span class="sxs-lookup"><span data-stu-id="c464c-146">Use dashboards to visualize Azure Databricks metrics</span></span>](./dashboards.md)

<!-- links -->

[build-lib]: ./configure-cluster.md##build-the-azure-databricks-monitoring-library
[config-cluster]: ./configure-cluster.md
[log4j.properties]: https://github.com/mspnp/spark-monitoring/blob/master/src/spark-jobs/src/main/resources/com/microsoft/pnp/samplejob/log4j.properties
[sample-app]: https://github.com/mspnp/spark-monitoring/tree/master/src/spark-jobs
[spark-logging]: https://spark.apache.org/docs/2.3.0/api/java/org/apache/spark/internal/Logging.html