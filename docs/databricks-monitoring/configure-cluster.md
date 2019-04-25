---
title: Konfigurieren von Azure Databricks zum Senden von Metriken an Azure Monitor
description: Eine Scala-Bibliothek, um die Überwachung von Metriken und Protokollierungsdaten in Azure Log Analytics zu ermöglichen
author: petertaylor9999
ms.date: 03/26/2019
ms.openlocfilehash: f2fc1fd19da661b74ddf032dd1d5153ce575345c
ms.sourcegitcommit: 579c39ff4b776704ead17a006bf24cd4cdc65edd
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/17/2019
ms.locfileid: "59639885"
---
<!-- markdownlint-disable MD040 -->

# <a name="configure-azure-databricks-to-send-metrics-to-azure-monitor"></a><span data-ttu-id="2ee9b-103">Konfigurieren von Azure Databricks zum Senden von Metriken an Azure Monitor</span><span class="sxs-lookup"><span data-stu-id="2ee9b-103">Configure Azure Databricks to send metrics to Azure Monitor</span></span>

<span data-ttu-id="2ee9b-104">In diesem Artikel wird gezeigt, wie Sie einen Azure Databricks-Cluster so konfigurieren, dass Metriken an einen [Log Analytics-Arbeitsbereich ](/azure/azure-monitor/platform/manage-access) gesendet werden.</span><span class="sxs-lookup"><span data-stu-id="2ee9b-104">This article shows how to configure an Azure Databricks cluster to send metrics to a [Log Analytics workspace](/azure/azure-monitor/platform/manage-access).</span></span> <span data-ttu-id="2ee9b-105">Dazu wird die [Azure Databricks-Überwachungsbibliothek](https://github.com/mspnp/spark-monitoring) verwendet, die auf GitHub verfügbar ist.</span><span class="sxs-lookup"><span data-stu-id="2ee9b-105">It uses the [Azure Databricks Monitoring Library](https://github.com/mspnp/spark-monitoring), which is available on GitHub.</span></span> <span data-ttu-id="2ee9b-106">Sie sollten mit Java, Scala und Maven vertraut sein.</span><span class="sxs-lookup"><span data-stu-id="2ee9b-106">Understanding of Java, Scala, and Maven are recommended as prerequisites.</span></span>

## <a name="about-the-azure-databricks-monitoring-library"></a><span data-ttu-id="2ee9b-107">Informationen zur Azure Databricks-Überwachungsbibliothek</span><span class="sxs-lookup"><span data-stu-id="2ee9b-107">About the Azure Databricks Monitoring Library</span></span>

<span data-ttu-id="2ee9b-108">Die [GitHub-Repository](https://github.com/mspnp/spark-monitoring) für die Azure Databricks-Überwachungsbibliothek hat die folgende Verzeichnisstruktur:</span><span class="sxs-lookup"><span data-stu-id="2ee9b-108">The [GitHub repo](https://github.com/mspnp/spark-monitoring) for the Azure Databricks Monitoring Library has following directory structure:</span></span>

```
/src  
    /spark-jobs  
    /spark-listeners-loganalytics  
    /spark-listeners  
    /pom.xml  
```

<span data-ttu-id="2ee9b-109">Das Verzeichnis **spark-jobs** ist eine Spark-Beispielanwendung, die zeigt, wie ein Zähler für Spark-Anwendungsmetriken implementiert wird.</span><span class="sxs-lookup"><span data-stu-id="2ee9b-109">The **spark-jobs** directory is a sample Spark application demonstrating how to implement a Spark application metric counter.</span></span>

<span data-ttu-id="2ee9b-110">Das Verzeichnis **spark-listeners** enthält Funktionen, die es Azure Databricks ermöglichen, Apache Spark-Ereignisse auf Dienstebene an einen Azure Log Analytics-Arbeitsbereich zu senden.</span><span class="sxs-lookup"><span data-stu-id="2ee9b-110">The **spark-listeners** directory includes functionality that enables Azure Databricks to send Apache Spark events at the service level to an Azure Log Analytics workspace.</span></span> <span data-ttu-id="2ee9b-111">Azure Databricks ist ein auf Apache Spark basierender Dienst, der eine Reihe strukturierter APIs für die Batchverarbeitung von Daten mit Datasets, DataFrames und SQL enthält.</span><span class="sxs-lookup"><span data-stu-id="2ee9b-111">Azure Databricks is a service based on Apache Spark, which includes a set of structured APIs for batch processing data using Datasets, DataFrames, and SQL.</span></span> <span data-ttu-id="2ee9b-112">In Apache Spark 2.0 wurde Unterstützung für [Structured Streaming](https://spark.apache.org/docs/latest/structured-streaming-programming-guide.html) hinzugefügt, eine Datenstromverarbeitungs-API, die auf Batchverarbeitungs-APIs von Spark basiert.</span><span class="sxs-lookup"><span data-stu-id="2ee9b-112">With Apache Spark 2.0, support was added for [Structured Streaming](https://spark.apache.org/docs/latest/structured-streaming-programming-guide.html), a data stream processing API built on Spark's batch processing APIs.</span></span>

<span data-ttu-id="2ee9b-113">Das Verzeichnis **spark-listeners-loganalytics** enthält eine Senke für Spark-Listener, eine Senke für DropWizard und einen Client für einen Azure Log Analytics-Arbeitsbereich.</span><span class="sxs-lookup"><span data-stu-id="2ee9b-113">The **spark-listeners-loganalytics** directory includes a sink for Spark listeners, a sink for DropWizard, and a client for an Azure Log Analytics Workspace.</span></span> <span data-ttu-id="2ee9b-114">Dieses Verzeichnis enthält auch einen log4j-Appender für Ihre Apache Spark-Anwendungsprotokolle.</span><span class="sxs-lookup"><span data-stu-id="2ee9b-114">This directory also includes a log4j Appender for your Apache Spark application logs.</span></span>

<span data-ttu-id="2ee9b-115">Die Verzeichnisse **spark-listeners-loganalytics** und **spark-listeners** enthalten den Code zum Erstellen der beiden JAR-Dateien, die im Databricks-Cluster bereitgestellt werden.</span><span class="sxs-lookup"><span data-stu-id="2ee9b-115">The **spark-listeners-loganalytics** and **spark-listeners** directories contain the code for building the two JAR files that are deployed to the Databricks cluster.</span></span> <span data-ttu-id="2ee9b-116">Das Verzeichnis **spark-listeners** enthält das Verzeichnis **scripts** mit einem Skript für die Initialisierung von Clusterknoten, um die JAR-Dateien aus einem Stagingverzeichnis im Dateisystem von Azure Databricks auf Ausführungsknoten zu kopieren.</span><span class="sxs-lookup"><span data-stu-id="2ee9b-116">The **spark-listeners** directory includes a **scripts** directory that contains a cluster node initialization script to copy the JAR files from a staging directory in the Azure Databricks file system to execution nodes.</span></span>

<span data-ttu-id="2ee9b-117">Die Datei **pom.xml** ist die wichtigste Maven-Builddatei für das gesamte Projekt.</span><span class="sxs-lookup"><span data-stu-id="2ee9b-117">The **pom.xml** file is the main Maven build file for the entire project.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="2ee9b-118">Voraussetzungen</span><span class="sxs-lookup"><span data-stu-id="2ee9b-118">Prerequisites</span></span>

<span data-ttu-id="2ee9b-119">Stellen Sie als Erstes die folgenden Azure-Ressourcen bereit:</span><span class="sxs-lookup"><span data-stu-id="2ee9b-119">To get started, deploy the following Azure resources:</span></span>

- <span data-ttu-id="2ee9b-120">Einen Azure Databricks-Arbeitsbereich.</span><span class="sxs-lookup"><span data-stu-id="2ee9b-120">An Azure Databricks workspace.</span></span> <span data-ttu-id="2ee9b-121">Weitere Informationen finden Sie unter [Schnellstart: Ausführen eines Spark-Auftrags in Azure Databricks über das Azure-Portal](/azure/azure-databricks/quickstart-create-databricks-workspace-portal).</span><span class="sxs-lookup"><span data-stu-id="2ee9b-121">See [Quickstart: Run a Spark job on Azure Databricks using the Azure portal](/azure/azure-databricks/quickstart-create-databricks-workspace-portal).</span></span>
- <span data-ttu-id="2ee9b-122">Einen Log Analytics-Arbeitsbereich</span><span class="sxs-lookup"><span data-stu-id="2ee9b-122">A Log Analytics workspace.</span></span> <span data-ttu-id="2ee9b-123">Siehe [Erstellen eines Log Analytics-Arbeitsbereichs im Azure-Portal](/azure/azure-monitor/learn/quick-create-workspace).</span><span class="sxs-lookup"><span data-stu-id="2ee9b-123">See [Create a Log Analytics workspace in the Azure portal](/azure/azure-monitor/learn/quick-create-workspace).</span></span>

<span data-ttu-id="2ee9b-124">Installieren Sie als Nächstes die [Azure Databricks CLI](https://docs.databricks.com/user-guide/dev-tools/databricks-cli.html#install-the-cli).</span><span class="sxs-lookup"><span data-stu-id="2ee9b-124">Next, install the [Azure Databricks CLI](https://docs.databricks.com/user-guide/dev-tools/databricks-cli.html#install-the-cli).</span></span> <span data-ttu-id="2ee9b-125">Ein persönliches Azure Databricks-Zugriffstoken ist für die Nutzung der CLI erforderlich.</span><span class="sxs-lookup"><span data-stu-id="2ee9b-125">An Azure Databricks personal access token is required to use the CLI.</span></span> <span data-ttu-id="2ee9b-126">Anweisungen hierzu finden Sie unter [Generieren eines Tokens](https://docs.azuredatabricks.net/api/latest/authentication.html#token-management).</span><span class="sxs-lookup"><span data-stu-id="2ee9b-126">For instructions, see [Generate a token](https://docs.azuredatabricks.net/api/latest/authentication.html#token-management).</span></span>

<span data-ttu-id="2ee9b-127">Rufen Sie Ihre Log Analytics-Arbeitsbereichs-ID und den Arbeitsbereichsschlüssel ab.</span><span class="sxs-lookup"><span data-stu-id="2ee9b-127">Find your Log Analytics workspace ID and key.</span></span> <span data-ttu-id="2ee9b-128">Anweisungen finden Sie unter [Abrufen von Arbeitsbereichs-ID und -Schlüssel](/azure/azure-monitor/platform/agent-windows#obtain-workspace-id-and-key).</span><span class="sxs-lookup"><span data-stu-id="2ee9b-128">For instructions, see [Obtain workspace ID and key](/azure/azure-monitor/platform/agent-windows#obtain-workspace-id-and-key).</span></span> <span data-ttu-id="2ee9b-129">Sie benötigen diese Werte, wenn Sie den Databricks-Cluster konfigurieren.</span><span class="sxs-lookup"><span data-stu-id="2ee9b-129">You will need these values when you configure the Databricks cluster.</span></span>

<span data-ttu-id="2ee9b-130">Um die Überwachungsbibliothek zu erstellen, benötigen Sie eine Java-IDE mit den folgenden Ressourcen:</span><span class="sxs-lookup"><span data-stu-id="2ee9b-130">To build the monitoring library, you will need a Java IDE with the following resources:</span></span>

- [<span data-ttu-id="2ee9b-131">Java Development Kit (JDK), Version 1.8</span><span class="sxs-lookup"><span data-stu-id="2ee9b-131">Java Development Kit (JDK) version 1.8</span></span>](http://www.oracle.com/technetwork/java/javase/downloads/index.html)
- [<span data-ttu-id="2ee9b-132">Scala Language SDK 2.11</span><span class="sxs-lookup"><span data-stu-id="2ee9b-132">Scala language SDK 2.11</span></span>](https://www.scala-lang.org/download/)
- [<span data-ttu-id="2ee9b-133">Apache Maven 3.5.4</span><span class="sxs-lookup"><span data-stu-id="2ee9b-133">Apache Maven 3.5.4</span></span>](http://maven.apache.org/download.cgi)

## <a name="build-the-azure-databricks-monitoring-library"></a><span data-ttu-id="2ee9b-134">Erstellen der Azure Databricks-Überwachungsbibliothek</span><span class="sxs-lookup"><span data-stu-id="2ee9b-134">Build the Azure Databricks Monitoring Library</span></span>

<span data-ttu-id="2ee9b-135">Um die Azure Databricks-Überwachungsbibliothek zu erstellen, führen Sie die folgenden Schritte aus:</span><span class="sxs-lookup"><span data-stu-id="2ee9b-135">To build the Azure Databricks Monitoring Library, perform the following steps:</span></span>

1. <span data-ttu-id="2ee9b-136">Klonen oder forken Sie das [GitHub-Repository](https://github.com/mspnp/spark-monitoring), oder laden Sie es herunter.</span><span class="sxs-lookup"><span data-stu-id="2ee9b-136">Clone, fork, or download the [GitHub repository](https://github.com/mspnp/spark-monitoring).</span></span>

1. <span data-ttu-id="2ee9b-137">Importieren Sie die Objektmodelldatei des Maven-Projekts _pom.xml_, die sich in Ihrem Projekt im Ordner **/src** befindet.</span><span class="sxs-lookup"><span data-stu-id="2ee9b-137">Import the Maven project object model file, _pom.xml_, located in the **/src** folder into your project.</span></span> <span data-ttu-id="2ee9b-138">Daraufhin werden drei Projekte importiert:</span><span class="sxs-lookup"><span data-stu-id="2ee9b-138">This will import three projects:</span></span>

    - <span data-ttu-id="2ee9b-139">spark-jobs</span><span class="sxs-lookup"><span data-stu-id="2ee9b-139">spark-jobs</span></span>
    - <span data-ttu-id="2ee9b-140">spark-listeners</span><span class="sxs-lookup"><span data-stu-id="2ee9b-140">spark-listeners</span></span>
    - <span data-ttu-id="2ee9b-141">spark-listeners-loganalytics</span><span class="sxs-lookup"><span data-stu-id="2ee9b-141">spark-listeners-loganalytics</span></span>

1. <span data-ttu-id="2ee9b-142">Führen Sie die Buildphase für das Maven-**Paket** in Ihrer Java-IDE aus, um die JAR-Dateien für jedes dieser Projekte zu erstellen:</span><span class="sxs-lookup"><span data-stu-id="2ee9b-142">Execute the Maven **package** build phase in your Java IDE to build the JAR files for each of these projects:</span></span>

    |<span data-ttu-id="2ee9b-143">Project</span><span class="sxs-lookup"><span data-stu-id="2ee9b-143">Project</span></span>| <span data-ttu-id="2ee9b-144">JAR-Datei</span><span class="sxs-lookup"><span data-stu-id="2ee9b-144">JAR file</span></span>|
    |-------|---------|
    |<span data-ttu-id="2ee9b-145">spark-jobs</span><span class="sxs-lookup"><span data-stu-id="2ee9b-145">spark-jobs</span></span>|<span data-ttu-id="2ee9b-146">spark-jobs-1.0-SNAPSHOT.jar</span><span class="sxs-lookup"><span data-stu-id="2ee9b-146">spark-jobs-1.0-SNAPSHOT.jar</span></span>|
    |<span data-ttu-id="2ee9b-147">spark-listeners</span><span class="sxs-lookup"><span data-stu-id="2ee9b-147">spark-listeners</span></span>|<span data-ttu-id="2ee9b-148">spark-listeners-1.0-SNAPSHOT.jar</span><span class="sxs-lookup"><span data-stu-id="2ee9b-148">spark-listeners-1.0-SNAPSHOT.jar</span></span>|
    |<span data-ttu-id="2ee9b-149">spark-listeners-loganalytics</span><span class="sxs-lookup"><span data-stu-id="2ee9b-149">spark-listeners-loganalytics</span></span>|<span data-ttu-id="2ee9b-150">spark-listeners-loganalytics-1.0-SNAPSHOT.jar</span><span class="sxs-lookup"><span data-stu-id="2ee9b-150">spark-listeners-loganalytics-1.0-SNAPSHOT.jar</span></span>|

1. <span data-ttu-id="2ee9b-151">Verwenden Sie die Azure Databricks CLI zum Erstellen eines Verzeichnisses mit dem Namen **dbfs:/databricks/monitoring-staging**:</span><span class="sxs-lookup"><span data-stu-id="2ee9b-151">Use the Azure Databricks CLI to create a directory named **dbfs:/databricks/monitoring-staging**:</span></span>  

    ```bash
    dbfs mkdirs dbfs:/databricks/monitoring-staging
    ```

1. <span data-ttu-id="2ee9b-152">Verwenden Sie die Azure Databricks CLI zum Kopieren von **/src/spark-listeners/scripts/listeners.sh** in das zuvor erstellte Verzeichnis:</span><span class="sxs-lookup"><span data-stu-id="2ee9b-152">Use the Azure Databricks CLI to copy **/src/spark-listeners/scripts/listeners.sh** to the directory previously:</span></span>

    ```bash
    dbfs cp ./src/spark-listeners/scripts/listeners.sh dbfs:/databricks/monitoring-staging/listeners.sh
    ```

1. <span data-ttu-id="2ee9b-153">Verwenden Sie die Azure Databricks CLI zum Kopieren von **/src/spark-listeners/scripts/metrics.properties** in das zuvor erstellte Verzeichnis:</span><span class="sxs-lookup"><span data-stu-id="2ee9b-153">Use the Azure Databricks CLI to copy **/src/spark-listeners/scripts/metrics.properties** to the directory created previously:</span></span>

    ```bash
    dbfs cp <local path to metrics.properties> dbfs:/databricks/monitoring-staging/metrics.properties
    ```

1. <span data-ttu-id="2ee9b-154">Verwenden Sie die Azure Databricks CLI zum Kopieren von **spark-listeners-1.0-SNAPSHOT.jar** und **spark-listeners-loganalytics-1.0-SNAPSHOT.jar** in das zuvor erstellte Verzeichnis:</span><span class="sxs-lookup"><span data-stu-id="2ee9b-154">Use the Azure Databricks CLI to copy **spark-listeners-1.0-SNAPSHOT.jar** and **spark-listeners-loganalytics-1.0-SNAPSHOT.jar** to the directory created previously:</span></span>

    ```bash
    dbfs cp ./src/spark-listeners/target/spark-listeners-1.0-SNAPSHOT.jar dbfs:/databricks/monitoring-staging/spark-listeners-1.0-SNAPSHOT.jar
    dbfs cp ./src/spark-listeners-loganalytics/target/spark-listeners-loganalytics-1.0-SNAPSHOT.jar dbfs:/databricks/monitoring-staging/spark-listeners-loganalytics-1.0-SNAPSHOT.jar
    ```

## <a name="create-and-configure-an-azure-databricks-cluster"></a><span data-ttu-id="2ee9b-155">Erstellen und Konfigurieren eines Azure Databricks-Clusters</span><span class="sxs-lookup"><span data-stu-id="2ee9b-155">Create and configure an Azure Databricks cluster</span></span>

<span data-ttu-id="2ee9b-156">Führen Sie zum Erstellen und Konfigurieren eines Azure Databricks-Clusters die folgenden Schritte aus:</span><span class="sxs-lookup"><span data-stu-id="2ee9b-156">To create and configure an Azure Databricks cluster, follow these steps:</span></span>

1. <span data-ttu-id="2ee9b-157">Navigieren Sie im Azure-Portal zu Ihrem Azure Databricks-Arbeitsbereich.</span><span class="sxs-lookup"><span data-stu-id="2ee9b-157">Navigate to your Azure Databricks workspace in the Azure portal.</span></span>
1. <span data-ttu-id="2ee9b-158">Klicken Sie auf der Startseite auf **Neuer Cluster**.</span><span class="sxs-lookup"><span data-stu-id="2ee9b-158">On the home page, click **New Cluster**.</span></span>
1. <span data-ttu-id="2ee9b-159">Geben Sie im Textfeld **Clustername** einen Namen für den Cluster ein.</span><span class="sxs-lookup"><span data-stu-id="2ee9b-159">Enter a name for your cluster in the **Cluster Name** text box.</span></span>
1. <span data-ttu-id="2ee9b-160">Wählen Sie **4.3** oder höher in der Dropdownliste **Databricks Runtime-Version** aus.</span><span class="sxs-lookup"><span data-stu-id="2ee9b-160">In the **Databricks Runtime Version** dropdown, select **4.3** or greater.</span></span>
1. <span data-ttu-id="2ee9b-161">Klicken Sie unter **Erweiterte Optionen**auf die Registerkarte **Spark**. Geben Sie die folgenden Name/Wert-Paare in der Textfeld **Spark Config** (Spark-Konfiguration)ein:</span><span class="sxs-lookup"><span data-stu-id="2ee9b-161">Under **Advanced Options**, click on the **Spark** tab. Enter the following name-value pairs in the **Spark Config** text box:</span></span>

    ```
    spark.extraListeners com.databricks.backend.daemon.driver.DBCEventLoggingListener,org.apache.spark.listeners.UnifiedSparkListener
    spark.unifiedListener.sink org.apache.spark.listeners.sink.loganalytics.LogAnalyticsListenerSink
    spark.unifiedListener.logBlockUpdates false
    ```

1. <span data-ttu-id="2ee9b-162">Geben Sie weiterhin auf der Registerkarte **Spark** die folgenden Werte in das Textfeld **Environment Variables** (Umgebungsvariablen) ein:</span><span class="sxs-lookup"><span data-stu-id="2ee9b-162">While still under the **Spark** tab, enter the following values in the **Environment Variables** text box:</span></span>

    ```
    LOG_ANALYTICS_WORKSPACE_ID=[your Azure Log Analytics workspace ID]
    LOG_ANALYTICS_WORKSPACE_KEY=[your Azure Log Analytics workspace key]
    ```

    ![Screenshot der Databricks-Benutzeroberfläche](./_images/create-cluster1.png)

1. <span data-ttu-id="2ee9b-164">Klicken Sie weiterhin im Abschnitt **Erweiterte Optionen** auf die Registerkarte **Init Scripts** (Initialisierungsskripts).</span><span class="sxs-lookup"><span data-stu-id="2ee9b-164">While still under the **Advanced Options** section, click on the **Init Scripts** tab.</span></span>
1. <span data-ttu-id="2ee9b-165">Wählen Sie in der Dropdownliste **Destination** (Ziel) den Eintrag **DBFS** aus.</span><span class="sxs-lookup"><span data-stu-id="2ee9b-165">In the **Destination** dropdown, select **DBFS**.</span></span>
1. <span data-ttu-id="2ee9b-166">Geben Sie unter **Init Script Path** (Pfad des Initialisierungsskript) `dbfs:/databricks/monitoring-staging/listeners.sh` ein.</span><span class="sxs-lookup"><span data-stu-id="2ee9b-166">Under **Init Script Path**, enter `dbfs:/databricks/monitoring-staging/listeners.sh`.</span></span>
1. <span data-ttu-id="2ee9b-167">Klicken Sie auf **Hinzufügen**.</span><span class="sxs-lookup"><span data-stu-id="2ee9b-167">Click **Add**.</span></span>

    ![Screenshot der Databricks-Benutzeroberfläche](./_images/create-cluster2.png)

1. <span data-ttu-id="2ee9b-169">Wählen Sie **Create Cluster** (Cluster erstellen), um den Cluster zu erstellen.</span><span class="sxs-lookup"><span data-stu-id="2ee9b-169">Click **Create Cluster** to create the cluster.</span></span>

## <a name="view-metrics"></a><span data-ttu-id="2ee9b-170">Anzeigen von Metriken</span><span class="sxs-lookup"><span data-stu-id="2ee9b-170">View metrics</span></span>

<span data-ttu-id="2ee9b-171">Nachdem Sie diese Schritte abgeschlossen haben, streamt Ihr Databricks-Cluster einige Metrikdaten zum Cluster selbst an Azure Monitor.</span><span class="sxs-lookup"><span data-stu-id="2ee9b-171">After you complete these steps, your Databricks cluster streams some metric data about the cluster itself to Azure Monitor.</span></span> <span data-ttu-id="2ee9b-172">Diese Protokolldaten stehen in Ihrem Azure Log Analytics-Arbeitsbereich unter dem Schema „Aktiv | Benutzerdefinierte Protokolle | SparkMetric_C“ zur Verfügung.</span><span class="sxs-lookup"><span data-stu-id="2ee9b-172">This log data is available in your Azure Log Analytics workspace under the "Active | Custom Logs | SparkMetric_CL" schema.</span></span> <span data-ttu-id="2ee9b-173">Weitere Informationen zu den Arten von Metriken, die protokolliert werden, finden Sie in der Dokumentation zu Dropwizard unter [Metrics Core](https://metrics.dropwizard.io/4.0.0/manual/core.html) (Hauptmetriken).</span><span class="sxs-lookup"><span data-stu-id="2ee9b-173">For more information about the types of metrics that are logged, see [Metrics Core](https://metrics.dropwizard.io/4.0.0/manual/core.html) in the Dropwizard documentation.</span></span> <span data-ttu-id="2ee9b-174">Den Metriktyp, wie z.B. Messgerät, Zähler oder das Histogramm, wird in das Feld „Typ“ geschrieben.</span><span class="sxs-lookup"><span data-stu-id="2ee9b-174">The metric type, such as gauge, counter, or histogram, is written to the Type field.</span></span>

<span data-ttu-id="2ee9b-175">Darüber hinaus streamt die Bibliothek Ereignisse auf Apache Spark-Ebene und Metriken für Spark Structured Streaming aus Ihren Aufträgen an Azure Monitor.</span><span class="sxs-lookup"><span data-stu-id="2ee9b-175">In addition, the library streams Apache Spark level events and Spark Structured Streaming metrics from your jobs to Azure Monitor.</span></span> <span data-ttu-id="2ee9b-176">Für diese Ereignisse und Metriken müssen Sie keine Änderungen an Ihrem Anwendungscode vornehmen.</span><span class="sxs-lookup"><span data-stu-id="2ee9b-176">You don't need to make any changes to your application code for these events and metrics.</span></span> <span data-ttu-id="2ee9b-177">Spark Structured StreamingProtokolldaten stehen in unter dem Schema „Aktiv | Benutzerdefinierte Protokolle | SparkListenerEvent_CL“ zur Verfügung.</span><span class="sxs-lookup"><span data-stu-id="2ee9b-177">Spark Structured Streaming log data is available under the "Active | Custom Logs | SparkListenerEvent_CL" schema.</span></span>

![Screenshot eines Log Analytics-Arbeitsbereichs](./_images/workspace.png)

<span data-ttu-id="2ee9b-179">Weitere Informationen zum Anzeigen von Protokolle finden Sie unter [Anzeigen und Analysieren von Protokolldaten in Azure Monitor](/azure/azure-monitor/log-query/portals).</span><span class="sxs-lookup"><span data-stu-id="2ee9b-179">For more information about viewing logs, see [Viewing and analyzing log data in Azure Monitor](/azure/azure-monitor/log-query/portals).</span></span>

## <a name="next-steps"></a><span data-ttu-id="2ee9b-180">Nächste Schritte</span><span class="sxs-lookup"><span data-stu-id="2ee9b-180">Next steps</span></span>

<span data-ttu-id="2ee9b-181">Dieser Artikel beschreibt, wie Sie Ihren Cluster so konfigurieren, dass er Metriken an Azure Monitor sendet.</span><span class="sxs-lookup"><span data-stu-id="2ee9b-181">This article described how to configure your cluster to send metrics to Azure Monitor.</span></span> <span data-ttu-id="2ee9b-182">Der nächste Artikel zeigt, wie Sie die Azure Databricks-Überwachungsbibliothek verwenden können, um Metriken und Protokolle von Anwendungen zu senden.</span><span class="sxs-lookup"><span data-stu-id="2ee9b-182">The next article shows how to use the Azure Databricks Monitoring Library to send application metrics and logs.</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="2ee9b-183">Senden von Anwendungsprotokollen an Azure Monitor</span><span class="sxs-lookup"><span data-stu-id="2ee9b-183">Send application logs to Azure Monitor</span></span>](./application-logs.md)
