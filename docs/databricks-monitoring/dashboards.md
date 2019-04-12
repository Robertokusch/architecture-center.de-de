---
title: Visualisieren von Azure Databricks-Metriken mithilfe von Dashboards
description: 'Gewusst wie: Bereitstellen eines Grafana-Dashboards zum Überwachen der Leistung in Azure Databricks'
author: petertaylor9999
ms.date: 03/26/2019
ms.openlocfilehash: 36fcd93f6ca757e8e750d0fcbbdf0311c08560b0
ms.sourcegitcommit: 1a3cc91530d56731029ea091db1f15d41ac056af
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/03/2019
ms.locfileid: "58887827"
---
# <a name="use-dashboards-to-visualize-azure-databricks-metrics"></a><span data-ttu-id="6e9b1-103">Visualisieren von Azure Databricks-Metriken mithilfe von Dashboards</span><span class="sxs-lookup"><span data-stu-id="6e9b1-103">Use dashboards to visualize Azure Databricks metrics</span></span>

<span data-ttu-id="6e9b1-104">In diesem Artikel wird das Einrichten eines Grafana-Dashboards zum Überwachen von Azure Databricks-Aufträgen auf Leistungsprobleme veranschaulicht.</span><span class="sxs-lookup"><span data-stu-id="6e9b1-104">This article shows how to set up a Grafana dashboard to monitor Azure Databricks jobs for performance issues.</span></span>

<span data-ttu-id="6e9b1-105">[Azure Databricks](/azure/azure-databricks/) ist ein schneller, leistungsstarker, für Zusammenarbeit ausgelegter Analysedienst, der auf [Apache Spark](https://spark.apache.org/) basiert und eine zügige Entwicklung und Bereitstellung von Analyse- und KI-Lösungen für Big Data ermöglicht.</span><span class="sxs-lookup"><span data-stu-id="6e9b1-105">[Azure Databricks](/azure/azure-databricks/) is a fast, powerful, and collaborative [Apache Spark](https://spark.apache.org/)–based analytics service that makes it easy to rapidly develop and deploy big data analytics and artificial intelligence (AI) solutions.</span></span> <span data-ttu-id="6e9b1-106">Die Überwachung ist eine wichtige Komponente des Einsatzes von Azure Databricks-Workloads in der Produktion.</span><span class="sxs-lookup"><span data-stu-id="6e9b1-106">Monitoring is a critical component of operating Azure Databricks workloads in production.</span></span> <span data-ttu-id="6e9b1-107">Der erste Schritt besteht im Sammeln von Metriken in einem Arbeitsbereich für die Analyse.</span><span class="sxs-lookup"><span data-stu-id="6e9b1-107">The first step is to gather metrics into a workspace for analysis.</span></span> <span data-ttu-id="6e9b1-108">In Azure ist [Azure Monitor](/azure/azure-monitor/) die beste Lösung zur Verwaltung von Protokolldaten.</span><span class="sxs-lookup"><span data-stu-id="6e9b1-108">In Azure, the best solution for managing log data is [Azure Monitor](/azure/azure-monitor/).</span></span> <span data-ttu-id="6e9b1-109">Azure Databricks unterstützt nicht nativ das Senden von Daten an Azure Monitor, aber eine [Bibliothek für diese Funktionalität](https://github.com/mspnp/spark-monitoring) steht in [Github](https://github.com) zur Verfügung.</span><span class="sxs-lookup"><span data-stu-id="6e9b1-109">Azure Databricks does not natively support sending log data to Azure monitor, but a [library for this functionality](https://github.com/mspnp/spark-monitoring) is available in [Github](https://github.com).</span></span>

<span data-ttu-id="6e9b1-110">Diese Bibliothek aktiviert sowohl die Protokollierung von Azure Databricks-Dienstmetriken als auch die Ereignismetriken für die Strukturiertes-Streaming-Abfrage in Apache Spark.</span><span class="sxs-lookup"><span data-stu-id="6e9b1-110">This library enables logging of Azure Databricks service metrics as well as Apache Spark structure streaming query event metrics.</span></span> <span data-ttu-id="6e9b1-111">Nachdem Sie diese Bibliothek erfolgreich für einen Azure Databricks-Cluster bereitgestellt haben, können Sie weiter eine Reihe von [Grafana](https://granfana.com)-Dashboards bereitstellen, die Sie als Teil Ihrer Produktionsumgebung bereitstellen können.</span><span class="sxs-lookup"><span data-stu-id="6e9b1-111">Once you've successfully deployed this library to an Azure Databricks cluster, you can further deploy a set of [Grafana](https://granfana.com) dashboards that you can deploy as part of your production environment.</span></span>

![Screenshot des Dashboards](./_images/dashboard-screenshot.png)

## <a name="prequisites"></a><span data-ttu-id="6e9b1-113">Voraussetzungen</span><span class="sxs-lookup"><span data-stu-id="6e9b1-113">Prequisites</span></span>

<span data-ttu-id="6e9b1-114">Klonen Sie das [Github-Repository](https://github.com/mspnp/spark-monitoring), und [befolgen Sie die Bereitstellungsanweisungen](./configure-cluster.md) zum Erstellen und Konfigurieren der Azure Monitor-Protokollierung für die Azure Databricks-Bibliothek, um Protokolle an den Azure Log Analytics-Arbeitsbereich zu senden.</span><span class="sxs-lookup"><span data-stu-id="6e9b1-114">Clone the [Github repository](https://github.com/mspnp/spark-monitoring) and [follow the deployment instructions](./configure-cluster.md) to build and configure the Azure Monitor logging for Azure Databricks library to send logs to your Azure Log Analytics workspace.</span></span>

## <a name="deploy-the-azure-log-analytics-workspace"></a><span data-ttu-id="6e9b1-115">Bereitstellen des Azure Log Analytics-Arbeitsbereichs</span><span class="sxs-lookup"><span data-stu-id="6e9b1-115">Deploy the Azure Log Analytics workspace</span></span>

<span data-ttu-id="6e9b1-116">Gehen Sie zum Bereitstellen des Azure Log Analytics-Arbeitsbereichs folgendermaßen vor:</span><span class="sxs-lookup"><span data-stu-id="6e9b1-116">To deploy the Azure Log Analytics workspace, follow these steps:</span></span>

1. <span data-ttu-id="6e9b1-117">Navigieren Sie zum Verzeichnis `/perftools/deployment/loganalytics`.</span><span class="sxs-lookup"><span data-stu-id="6e9b1-117">Navigate to the `/perftools/deployment/loganalytics` directory.</span></span>
1. <span data-ttu-id="6e9b1-118">Stellen Sie die Azure Resource Manager-Vorlage **logAnalyticsDeploy.json** bereit.</span><span class="sxs-lookup"><span data-stu-id="6e9b1-118">Deploy the **logAnalyticsDeploy.json** Azure Resource Manager template.</span></span> <span data-ttu-id="6e9b1-119">Weitere Informationen zur Bereitstellung von Resource Manager-Vorlagen finden Sie unter [Bereitstellen von Ressourcen mit Azure Resource Manager-Vorlagen und Azure CLI][rm-cli].</span><span class="sxs-lookup"><span data-stu-id="6e9b1-119">For more information about deploying Resource Manager templates, see [Deploy resources with Resource Manager templates and Azure CLI][rm-cli].</span></span> <span data-ttu-id="6e9b1-120">Die Vorlage hat die folgenden Parameter:</span><span class="sxs-lookup"><span data-stu-id="6e9b1-120">The template has the following parameters:</span></span>

    * <span data-ttu-id="6e9b1-121">**location**: Die Region, in der Log Analytics-Arbeitsbereich und Dashboards bereitgestellt werden.</span><span class="sxs-lookup"><span data-stu-id="6e9b1-121">**location**: The region where the Log Analytics workspace and dashboards are deployed.</span></span>
    * <span data-ttu-id="6e9b1-122">**serviceTier**: Der Arbeitsbereichstarif.</span><span class="sxs-lookup"><span data-stu-id="6e9b1-122">**serviceTier**: Rhe workspace pricing tier.</span></span> <span data-ttu-id="6e9b1-123">[Hier][sku] finden Sie eine Liste gültiger Werte.</span><span class="sxs-lookup"><span data-stu-id="6e9b1-123">See [here][sku] for a list of valid values.</span></span>
    * <span data-ttu-id="6e9b1-124">**dataRetention** (optional): Die Dauer der Speicherung von Protokolldaten im Log Analytics-Arbeitsbereich in Tagen.</span><span class="sxs-lookup"><span data-stu-id="6e9b1-124">**dataRetention** (optional): The number of days log data is retained in the Log Analytics workspace.</span></span> <span data-ttu-id="6e9b1-125">Der Standardwert ist 30 Tage.</span><span class="sxs-lookup"><span data-stu-id="6e9b1-125">The default value is 30 days.</span></span> <span data-ttu-id="6e9b1-126">Im Tarif `Free` müssen die Daten 7 Tage beibehalten werden.</span><span class="sxs-lookup"><span data-stu-id="6e9b1-126">If the pricing tier is `Free`, the data retention must be 7 days.</span></span>
    * <span data-ttu-id="6e9b1-127">**workspaceName** (optional): Ein Name für den Arbeitsbereich.</span><span class="sxs-lookup"><span data-stu-id="6e9b1-127">**workspaceName** (optional): A name for the workspace.</span></span> <span data-ttu-id="6e9b1-128">Ohne Angabe generiert die Vorlage einen Namen.</span><span class="sxs-lookup"><span data-stu-id="6e9b1-128">If not specified, the template generates a name.</span></span>

    ```bash
    az group deployment create --resource-group <resource-group-name> --template-file logAnalyticsDeploy.json --parameters location='East US' serviceTier='Standalone'
    ```

<span data-ttu-id="6e9b1-129">Diese Vorlage erstellt den Arbeitsbereich und auch einen Satz von vordefinierten Abfragen, die vom Dashboard verwendet werden.</span><span class="sxs-lookup"><span data-stu-id="6e9b1-129">This template creates the workspace and also creates a set of predefined queries that are used by by dashboard.</span></span>

## <a name="deploy-grafana-in-a-virtual-machine"></a><span data-ttu-id="6e9b1-130">Bereitstellen von Grafana auf einem virtuellen Computer</span><span class="sxs-lookup"><span data-stu-id="6e9b1-130">Deploy Grafana in a virtual machine</span></span>

<span data-ttu-id="6e9b1-131">Das Open-Source-Projekt Grafana können Sie bereitstellen, um die in Ihrem Azure Log Analytics-Arbeitsbereich gespeicherten Zeitreihenmetriken mithilfe des Grafana-Plug-Ins für Azure Monitor zu visualisieren.</span><span class="sxs-lookup"><span data-stu-id="6e9b1-131">Grafana is an open source project you can deploy to visualize the time series metrics stored in your Azure Log Analytics workspace using the Grafana plugin for Azure Monitor.</span></span> <span data-ttu-id="6e9b1-132">Grafana wird auf einem virtuellen Computer (VM) ausgeführt und erfordert ein Speicherkonto, ein virtuelles Netzwerk und andere Ressourcen.</span><span class="sxs-lookup"><span data-stu-id="6e9b1-132">Grafana executes on a virtual machine (VM) and requires a storage account, virtual network, and other resources.</span></span> <span data-ttu-id="6e9b1-133">Gehen Sie zum Bereitstellen eines virtuellen Computers mit dem Bitnami-zertifizierten Grafana-Image und dazugehörigen Ressourcen folgendermaßen vor:</span><span class="sxs-lookup"><span data-stu-id="6e9b1-133">To deploy a virtual machine with the bitnami certified Grafana image and associated resources, follow these steps:</span></span>

1. <span data-ttu-id="6e9b1-134">Verwenden Sie die Azure-Befehlszeilenschnittstelle, um die Azure Marketplace-Image-Bedingungen für Grafana zu akzeptieren.</span><span class="sxs-lookup"><span data-stu-id="6e9b1-134">Use the Azure CLI to accept the Azure Marketplace image terms for Grafana.</span></span>

    ```bash
    az vm image accept-terms --publisher bitnami --offer grafana --plan default
    ```

1. <span data-ttu-id="6e9b1-135">Navigieren Sie in Ihrer lokalen Kopie des GitHub-Repositorys zum `/spark-monitoring/perftools/deployment/grafana`-Verzeichnis.</span><span class="sxs-lookup"><span data-stu-id="6e9b1-135">Navigate to the `/spark-monitoring/perftools/deployment/grafana` directory in your local copy of the GitHub repo.</span></span>
1. <span data-ttu-id="6e9b1-136">Stellen Sie die Resource Manager-Vorlage **grafanaDeploy.json** wie folgt bereit:</span><span class="sxs-lookup"><span data-stu-id="6e9b1-136">Deploy the **grafanaDeploy.json** Resource Manager template as follows:</span></span>

    ```bash
    export DATA_SOURCE="https://raw.githubusercontent.com/mspnp/spark-monitoring/master/perftools/deployment/grafana/AzureDataSource.sh"
    az group deployment create \
        --resource-group <resource-group-name> \
        --template-file grafanaDeploy.json \
        --parameters adminPass='<vm password>' dataSource=$DATA_SOURCE
    ```

<span data-ttu-id="6e9b1-137">Sobald die Bereitstellung abgeschlossen ist, wird das Bitnami-Image von Grafana auf dem virtuellen Computer installiert.</span><span class="sxs-lookup"><span data-stu-id="6e9b1-137">Once the deployment is complete, the bitnami image of Grafana is installed on the virtual machine.</span></span>

## <a name="update-the-grafana-password"></a><span data-ttu-id="6e9b1-138">Aktualisieren des Grafana-Kennworts</span><span class="sxs-lookup"><span data-stu-id="6e9b1-138">Update the Grafana password</span></span>

<span data-ttu-id="6e9b1-139">Im Rahmen des Installationsvorgangs gibt das Grafana-Installationsskript ein temporäres Kennwort für den **Administrator**-Benutzer aus.</span><span class="sxs-lookup"><span data-stu-id="6e9b1-139">As part of the setup process, the Grafana installation script outputs a temporary password for the **admin** user.</span></span> <span data-ttu-id="6e9b1-140">Sie benötigen dieses temporäre Kennwort für die Anmeldung.</span><span class="sxs-lookup"><span data-stu-id="6e9b1-140">You need this temporary password to sign in.</span></span> <span data-ttu-id="6e9b1-141">Um das temporäre Kennwort zu erhalten, gehen Sie folgendermaßen vor:</span><span class="sxs-lookup"><span data-stu-id="6e9b1-141">To obtain the temporary password, follow these steps:</span></span>  

1. <span data-ttu-id="6e9b1-142">Melden Sie sich beim Azure-Portal an.</span><span class="sxs-lookup"><span data-stu-id="6e9b1-142">Log in to the Azure portal.</span></span>  
1. <span data-ttu-id="6e9b1-143">Wählen Sie die Ressourcengruppe aus, in der die Ressourcen bereitgestellt wurden.</span><span class="sxs-lookup"><span data-stu-id="6e9b1-143">Select the resource group where the resources were deployed.</span></span>
1. <span data-ttu-id="6e9b1-144">Wählen Sie den virtuellen Computer aus, auf dem Grafana installiert wurde.</span><span class="sxs-lookup"><span data-stu-id="6e9b1-144">Select the VM where Grafana was installed.</span></span> <span data-ttu-id="6e9b1-145">Wenn Sie den Standardparameternamen in der Bereitstellungsvorlage verwendet haben, wird dem VM-Namen **sparkmonitoring-vm-grafana** vorangestellt.</span><span class="sxs-lookup"><span data-stu-id="6e9b1-145">If you used the default parameter name in the deployment template, the VM name is prefaced with **sparkmonitoring-vm-grafana**.</span></span>
1. <span data-ttu-id="6e9b1-146">Klicken Sie im Abschnitt **Support + Problembehandlung** auf **Startdiagnose**, um die Startdiagnoseseite zu öffnen.</span><span class="sxs-lookup"><span data-stu-id="6e9b1-146">In the **Support + troubleshooting** section, click **Boot diagnostics** to open the boot diagnostics page.</span></span>
1. <span data-ttu-id="6e9b1-147">Klicken Sie auf der Startdiagnoseseite auf **Serielles Protokoll**.</span><span class="sxs-lookup"><span data-stu-id="6e9b1-147">Click **Serial log** on the boot diagnostics page.</span></span>
1. <span data-ttu-id="6e9b1-148">Suchen Sie nach folgender Zeichenfolge: „Setting Bitnami application password to“.</span><span class="sxs-lookup"><span data-stu-id="6e9b1-148">Search for the following string: "Setting Bitnami application password to".</span></span>
1. <span data-ttu-id="6e9b1-149">Kopieren Sie das Kennwort an einen sicheren Speicherort.</span><span class="sxs-lookup"><span data-stu-id="6e9b1-149">Copy the password to a safe location.</span></span>

<span data-ttu-id="6e9b1-150">Ändern Sie als Nächstes mit folgenden Schritten das Grafana-Administratorkennwort:</span><span class="sxs-lookup"><span data-stu-id="6e9b1-150">Next, change the Grafana administrator password by following these steps:</span></span>

1. <span data-ttu-id="6e9b1-151">Wählen Sie im Azure-Portal den virtuellen Computer aus, und klicken Sie auf **Übersicht**.</span><span class="sxs-lookup"><span data-stu-id="6e9b1-151">In the Azure portal, select the VM and click **Overview**.</span></span>
1. <span data-ttu-id="6e9b1-152">Kopieren Sie die öffentliche IP-Adresse.</span><span class="sxs-lookup"><span data-stu-id="6e9b1-152">Copy the public IP address.</span></span>
1. <span data-ttu-id="6e9b1-153">Öffnen Sie einen Browser, und navigieren Sie zur folgenden URL: `http://<IP addresss>:3000`.</span><span class="sxs-lookup"><span data-stu-id="6e9b1-153">Open a web browser and navigate to the following URL: `http://<IP addresss>:3000`.</span></span>
1. <span data-ttu-id="6e9b1-154">Geben Sie im Grafana-Anmeldebildschirm **admin** für den Benutzernamen ein, und verwenden Sie das Grafana-Kennwort aus den vorherigen Schritten.</span><span class="sxs-lookup"><span data-stu-id="6e9b1-154">At the Grafana log in screen, enter **admin** for the user name, and use the Grafana password from the previous steps.</span></span>
1. <span data-ttu-id="6e9b1-155">Wählen Sie nach der Anmeldung **Konfiguration** aus (das Zahnradsymbol).</span><span class="sxs-lookup"><span data-stu-id="6e9b1-155">Once logged in, select **Configuration** (the gear icon).</span></span>
1. <span data-ttu-id="6e9b1-156">Wählen Sie **Serveradministrator** aus.</span><span class="sxs-lookup"><span data-stu-id="6e9b1-156">Select **Server Admin**.</span></span>
1. <span data-ttu-id="6e9b1-157">Wählen Sie auf der Registerkarte **Benutzer** die **admin**-Anmeldung aus.</span><span class="sxs-lookup"><span data-stu-id="6e9b1-157">On the **Users** tab, select the **admin** login.</span></span>
1. <span data-ttu-id="6e9b1-158">Aktualisieren Sie das Kennwort.</span><span class="sxs-lookup"><span data-stu-id="6e9b1-158">Update the password.</span></span>

## <a name="create-an-azure-monitor-data-source"></a><span data-ttu-id="6e9b1-159">Erstellen einer Azure Monitor-Datenquelle</span><span class="sxs-lookup"><span data-stu-id="6e9b1-159">Create an Azure Monitor data source</span></span>

1. <span data-ttu-id="6e9b1-160">Erstellen Sie einen Dienstprinzipal, mit dem Grafana Ihren Zugriff auf den Log Analytics-Arbeitsbereich verwalten kann.</span><span class="sxs-lookup"><span data-stu-id="6e9b1-160">Create a service principal that allows Grafana to manage access to your Log Analytics workspace.</span></span> <span data-ttu-id="6e9b1-161">Weitere Informationen finden Sie unter [Erstellen eines Azure-Dienstprinzipals mit Azure CLI](/cli/azure/create-an-azure-service-principal-azure-cli?view=azure-cli-latest).</span><span class="sxs-lookup"><span data-stu-id="6e9b1-161">For more information, see [Create an Azure service principal with Azure CLI](/cli/azure/create-an-azure-service-principal-azure-cli?view=azure-cli-latest)</span></span>

    ```bash
    az ad sp create-for-rbac --name http://<service principal name> --role "Log Analytics Reader"
    ```

1. <span data-ttu-id="6e9b1-162">Beachten Sie die Werte für „appId“, „password“ und „tenant“ in der Ausgabe dieses Befehls:</span><span class="sxs-lookup"><span data-stu-id="6e9b1-162">Note the values for appId, password, and tenant in the output from this command:</span></span>

    ```json
    {
        "appId": "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx",
        "displayName": "azure-cli-2019-03-27-00-33-39",
        "name": "http://<service principal name>",
        "password": "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx",
        "tenant": "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
    }
    ```

1. <span data-ttu-id="6e9b1-163">Melden Sie sich wie weiter oben beschrieben bei Grafana an.</span><span class="sxs-lookup"><span data-stu-id="6e9b1-163">Log into Grafana as described earlier.</span></span> <span data-ttu-id="6e9b1-164">Wählen Sie **Konfiguration** (das Zahnradsymbol) und dann **Datenquellen** aus.</span><span class="sxs-lookup"><span data-stu-id="6e9b1-164">Select **Configuration** (the gear icon) and then **Data Sources**.</span></span>
1. <span data-ttu-id="6e9b1-165">Klicken Sie auf der Registerkarte **Datenquellen** auf **Datenquelle hinzufügen**.</span><span class="sxs-lookup"><span data-stu-id="6e9b1-165">In the **Data Sources** tab, click **Add data source**.</span></span>
1. <span data-ttu-id="6e9b1-166">Wählen Sie **Azure Monitor** als Datenquelle aus.</span><span class="sxs-lookup"><span data-stu-id="6e9b1-166">Select **Azure Monitor** as the data source type.</span></span>
1. <span data-ttu-id="6e9b1-167">Geben Sie im Abschnitt **Einstellungen** im Textfeld **Name** einen Namen für die Datenquelle ein.</span><span class="sxs-lookup"><span data-stu-id="6e9b1-167">In the **Settings** section, enter a name for the data source in the **Name** textbox.</span></span>
1. <span data-ttu-id="6e9b1-168">Geben Sie im Abschnitt **Azure Monitor-API-Details** die folgenden Informationen ein:</span><span class="sxs-lookup"><span data-stu-id="6e9b1-168">In the **Azure Monitor API Details** section, enter the following information:</span></span>

    * <span data-ttu-id="6e9b1-169">Subscription Id: Die ID Ihres Azure-Abonnements.</span><span class="sxs-lookup"><span data-stu-id="6e9b1-169">Subscription Id: Your Azure subscription ID.</span></span>
    * <span data-ttu-id="6e9b1-170">Tenant Id: Die frühere Mandanten-ID.</span><span class="sxs-lookup"><span data-stu-id="6e9b1-170">Tenant Id: The tenant ID from earlier.</span></span>
    * <span data-ttu-id="6e9b1-171">Client Id: Der frühere Wert von „appId“.</span><span class="sxs-lookup"><span data-stu-id="6e9b1-171">Client Id: The value of "appId" from earlier.</span></span>
    * <span data-ttu-id="6e9b1-172">Geheimer Clientschlüssel: Der frühere Wert von „password“.</span><span class="sxs-lookup"><span data-stu-id="6e9b1-172">Client Secret: The value of "password" from earlier.</span></span>

1. <span data-ttu-id="6e9b1-173">Aktivieren Sie im Abschnitt **Azure Log Analytics-API-Details** das Kontrollkästchen **Dieselben Details wie die Azure Monitor-API**.</span><span class="sxs-lookup"><span data-stu-id="6e9b1-173">In the **Azure Log Analytics API Details** section, check the **Same Details as Azure Monitor API** checkbox.</span></span>
1. <span data-ttu-id="6e9b1-174">Klicken Sie auf **Speichern und testen**.</span><span class="sxs-lookup"><span data-stu-id="6e9b1-174">Click **Save & Test**.</span></span> <span data-ttu-id="6e9b1-175">Wenn die Log Analytics-Datenquelle ordnungsgemäß konfiguriert ist, wird eine Erfolgsmeldung angezeigt.</span><span class="sxs-lookup"><span data-stu-id="6e9b1-175">If the Log Analytics data source is correctly configured, a success message is displayed.</span></span>

## <a name="create-the-dashboard"></a><span data-ttu-id="6e9b1-176">Erstellen des Dashboards</span><span class="sxs-lookup"><span data-stu-id="6e9b1-176">Create the dashboard</span></span>

<span data-ttu-id="6e9b1-177">Erstellen Sie mit folgenden Schritten die Dashboards in Grafana:</span><span class="sxs-lookup"><span data-stu-id="6e9b1-177">Create the dashboards in Grafana by following these steps:</span></span>

1. <span data-ttu-id="6e9b1-178">Navigieren Sie in Ihrer lokalen Kopie des GitHub-Repositorys zum `/perftools/dashboards/grafana`-Verzeichnis.</span><span class="sxs-lookup"><span data-stu-id="6e9b1-178">Navigate to the `/perftools/dashboards/grafana` directory in your local copy of the GitHub repo.</span></span>
1. <span data-ttu-id="6e9b1-179">Führen Sie das folgende Skript aus:</span><span class="sxs-lookup"><span data-stu-id="6e9b1-179">Run the following script:</span></span>

    ```bash
    export WORKSPACE=<your Azure Log Analytics workspace ID>
    export LOGTYPE=SparkListenerEvent_CL

    sh DashGen.sh
    ```

    <span data-ttu-id="6e9b1-180">Die Ausgabe des Skripts ist eine Datei namens **SparkMonitoringDash.json**.</span><span class="sxs-lookup"><span data-stu-id="6e9b1-180">The output from the script is a file named **SparkMonitoringDash.json**.</span></span>

1. <span data-ttu-id="6e9b1-181">Kehren Sie zum Grafana-Dashboard zurück, und wählen Sie **Erstellen** aus (das Plussymbol).</span><span class="sxs-lookup"><span data-stu-id="6e9b1-181">Return to the Grafana dashboard and select **Create** (the plus icon).</span></span>
1. <span data-ttu-id="6e9b1-182">Wählen Sie **Importieren** aus.</span><span class="sxs-lookup"><span data-stu-id="6e9b1-182">Select **Import**.</span></span>
1. <span data-ttu-id="6e9b1-183">Klicken Sie auf **JSON-Datei hochladen**.</span><span class="sxs-lookup"><span data-stu-id="6e9b1-183">Click **Upload .json File**.</span></span>
1. <span data-ttu-id="6e9b1-184">Wählen Sie die in Schritt 2 erstellte **SparkMonitoringDash.json**-Datei aus.</span><span class="sxs-lookup"><span data-stu-id="6e9b1-184">Select the **SparkMonitoringDash.json** file created in step 2.</span></span>
1. <span data-ttu-id="6e9b1-185">Wählen Sie im Abschnitt **Optionen** unter **ALA** die zuvor erstellte Azure Monitor-Datenquelle aus.</span><span class="sxs-lookup"><span data-stu-id="6e9b1-185">In the **Options** section, under **ALA**, select the Azure Monitor data source created earlier.</span></span>
1. <span data-ttu-id="6e9b1-186">Klicken Sie auf **Importieren**.</span><span class="sxs-lookup"><span data-stu-id="6e9b1-186">Click **Import**.</span></span>

## <a name="visualizations-in-the-dashboards"></a><span data-ttu-id="6e9b1-187">Visualisierungen in den Dashboards</span><span class="sxs-lookup"><span data-stu-id="6e9b1-187">Visualizations in the dashboards</span></span>

<span data-ttu-id="6e9b1-188">Sowohl Azure Log Analytics als auch Grafana-Dashboards enthalten eine Reihe von Zeitreihenvisualisierungen.</span><span class="sxs-lookup"><span data-stu-id="6e9b1-188">Both the Azure Log Analytics and Grafana dashboards include a set of time-series visualizations.</span></span> <span data-ttu-id="6e9b1-189">Jedes Diagramm ist ein Zeitreihenplot von Metrikdaten, die sich auf einen Apache Spark-[Auftrag](https://spark.apache.org/docs/latest/job-scheduling.html), die Phasen des Auftrags und die Aufgaben, aus denen jede Phase besteht, beziehen.</span><span class="sxs-lookup"><span data-stu-id="6e9b1-189">Each graph is time-series plot of metric data related to an Apache Spark [job](https://spark.apache.org/docs/latest/job-scheduling.html), stages of the job, and tasks that make up each stage.</span></span>

<span data-ttu-id="6e9b1-190">Die Visualisierungen lauten wie folgt:</span><span class="sxs-lookup"><span data-stu-id="6e9b1-190">The visualizations are as follows:</span></span>

### <a name="job-latency"></a><span data-ttu-id="6e9b1-191">Auftragslatenz</span><span class="sxs-lookup"><span data-stu-id="6e9b1-191">Job latency</span></span>

<span data-ttu-id="6e9b1-192">Diese Visualisierung zeigt die Ausführungslatenz für einen Auftrag an, eine grobe Sicht der Gesamtleistung eines Auftrags.</span><span class="sxs-lookup"><span data-stu-id="6e9b1-192">This visualization shows execution latency for a job, which is a coarse view on the overall peformance of a job.</span></span> <span data-ttu-id="6e9b1-193">Die Dauer der Auftragsausführung wird vom Anfang bis zum Abschluss angezeigt.</span><span class="sxs-lookup"><span data-stu-id="6e9b1-193">Displays the job execution duration from start to completion.</span></span> <span data-ttu-id="6e9b1-194">Beachten Sie, dass die Startzeit des Auftrags nicht mit der Auftragsübermittlungszeit identisch ist.</span><span class="sxs-lookup"><span data-stu-id="6e9b1-194">Note that the job start time is not the same as the job submission time.</span></span> <span data-ttu-id="6e9b1-195">Die Latenz wird in Quantilen (10%, 30%, 50%, 90%) der Auftragsausführung dargestellt, indiziert durch Cluster-ID und Anwendungs-ID.</span><span class="sxs-lookup"><span data-stu-id="6e9b1-195">Latency is represented as percentiles (10%, 30%, 50%, 90%) of job execution indexed by cluster ID and application ID.</span></span>

### <a name="stage-latency"></a><span data-ttu-id="6e9b1-196">Phasenlatenz</span><span class="sxs-lookup"><span data-stu-id="6e9b1-196">Stage latency</span></span>

<span data-ttu-id="6e9b1-197">Die Visualisierung zeigt die Latenz der einzelnen Phasen pro Cluster, Anwendung und für die einzelne Phase an.</span><span class="sxs-lookup"><span data-stu-id="6e9b1-197">The visualization shows the latency of each stage per cluster, per application, and per individual stage.</span></span> <span data-ttu-id="6e9b1-198">Diese Visualisierung ist nützlich zum Identifizieren einer bestimmten Phase, die langsam ausgeführt wird.</span><span class="sxs-lookup"><span data-stu-id="6e9b1-198">This visualization is useful for identifying a particular stage that is running slowly.</span></span>

### <a name="task-latency"></a><span data-ttu-id="6e9b1-199">Aufgabenlatenz</span><span class="sxs-lookup"><span data-stu-id="6e9b1-199">Task latency</span></span>

<span data-ttu-id="6e9b1-200">Diese Visualisierung zeigt die Ausführungslatenz von Aufgaben an.</span><span class="sxs-lookup"><span data-stu-id="6e9b1-200">This visualization shows task execution latency.</span></span> <span data-ttu-id="6e9b1-201">Die Latenz wird als ein Quantil der Taskausführung pro Cluster, Phasenname und Anwendung dargestellt.</span><span class="sxs-lookup"><span data-stu-id="6e9b1-201">Latency is represented as a percentile of task execution per cluster, stage name, and application.</span></span>

### <a name="sum-task-execution-per-host"></a><span data-ttu-id="6e9b1-202">Summe der Aufgabenausführung pro Host</span><span class="sxs-lookup"><span data-stu-id="6e9b1-202">Sum Task Execution per host</span></span>

<span data-ttu-id="6e9b1-203">Diese Visualisierung zeigt die Summe der Aufgabenausführungslatenz pro Host in einem Cluster an.</span><span class="sxs-lookup"><span data-stu-id="6e9b1-203">This visualization shows the sum of task execution latency per host running on a cluster.</span></span> <span data-ttu-id="6e9b1-204">Das Anzeigen der Aufgabenausführungslatenz pro Host identifiziert Hosts mit einer viel höheren gesamten Aufgabenlatenz als andere Hosts.</span><span class="sxs-lookup"><span data-stu-id="6e9b1-204">Viewing task execution latency per host identifies hosts that have much higher overall task latency than other hosts.</span></span> <span data-ttu-id="6e9b1-205">Dies kann bedeuten, dass Aufgaben ineffizient oder ungleichmäßig auf Hosts verteilt wurden.</span><span class="sxs-lookup"><span data-stu-id="6e9b1-205">This may mean that tasks have been inefficiently or unevenly distributed to hosts.</span></span>

### <a name="task-metrics"></a><span data-ttu-id="6e9b1-206">Aufgabenmetriken</span><span class="sxs-lookup"><span data-stu-id="6e9b1-206">Task metrics</span></span>

<span data-ttu-id="6e9b1-207">Diese Visualisierung zeigt einen Satz von Ausführungsmetriken für die Ausführung einer bestimmten Aufgabe an.</span><span class="sxs-lookup"><span data-stu-id="6e9b1-207">This visualization shows a set of the execution metrics for a given task's execution.</span></span> <span data-ttu-id="6e9b1-208">Diese Metriken beinhalten die Größe und Dauer eines Datenshuffles, die Dauer der Serialisierungs- und Deserialisierungsvorgänge und Sonstiges.</span><span class="sxs-lookup"><span data-stu-id="6e9b1-208">These metrics include the size and duration of a data shuffle, duration of serialization and deserialization operations, and others.</span></span> <span data-ttu-id="6e9b1-209">Wenn Sie die Log Analytics-Abfrage für den Bereich anzeigen, wird der vollständige Satz der Metriken angezeigt.</span><span class="sxs-lookup"><span data-stu-id="6e9b1-209">For the full set of metrics, view the Log Analytics query for the panel.</span></span> <span data-ttu-id="6e9b1-210">Diese Visualisierung ist nützlich, um die Vorgänge zu verstehen, die eine Aufgabe ausmachen, und den Ressourcenverbrauch der einzelnen Vorgänge zu identifizieren.</span><span class="sxs-lookup"><span data-stu-id="6e9b1-210">This visualization is useful for understanding the operations that make up a task and identifying resource consumption of each operation.</span></span> <span data-ttu-id="6e9b1-211">Spitzen im Diagramm stellen kostspielige Vorgänge dar, die untersucht werden sollten.</span><span class="sxs-lookup"><span data-stu-id="6e9b1-211">Spikes in the graph represent costly operations that should be investigated.</span></span>

### <a name="cluster-throughput"></a><span data-ttu-id="6e9b1-212">Clusterdurchsatz</span><span class="sxs-lookup"><span data-stu-id="6e9b1-212">Cluster throughput</span></span>

<span data-ttu-id="6e9b1-213">Diese Visualisierung ist eine allgemeine Ansicht von nach Cluster und Anwendung indizierten Arbeitselementen zur Darstellung des pro Cluster und Anwendung verrichteten Arbeitspensums.</span><span class="sxs-lookup"><span data-stu-id="6e9b1-213">This visualization is a high level view of work items indexed by cluster and application to represent the amount of work done per cluster and application.</span></span> <span data-ttu-id="6e9b1-214">Sie zeigt die Anzahl der pro Cluster, Anwendung und Phase abgeschlossenen Aufträge, Aufgaben und Phasen in Schritten von einer Minute an.</span><span class="sxs-lookup"><span data-stu-id="6e9b1-214">It shows the number of jobs, tasks, and stages completed per cluster, application, and stage in one minute increments.</span></span> 

### <a name="streaming-throughputlatency"></a><span data-ttu-id="6e9b1-215">Streamingdurchsatz/Latenz</span><span class="sxs-lookup"><span data-stu-id="6e9b1-215">Streaming Throughput/Latency</span></span>

<span data-ttu-id="6e9b1-216">Diese Visualisierung bezieht sich auf die Metriken in Zusammenhang mit einer Strukturiertes-Streaming-Abfrage.</span><span class="sxs-lookup"><span data-stu-id="6e9b1-216">This visualzation is related to the metrics associated with a structured streaming query.</span></span> <span data-ttu-id="6e9b1-217">Das Diagramm zeigt die Anzahl der Eingabezeilen pro Sekunde und die Anzahl der pro Sekunde verarbeiteten Zeilen.</span><span class="sxs-lookup"><span data-stu-id="6e9b1-217">The graphs shows the number of input rows per second and the number of rows processed per second.</span></span> <span data-ttu-id="6e9b1-218">Die Streamingmetriken werden auch pro Anwendung dargestellt.</span><span class="sxs-lookup"><span data-stu-id="6e9b1-218">The streaming metrics are also represented per application.</span></span> <span data-ttu-id="6e9b1-219">Diese Metriken werden gesendet, wenn das OnQueryProgress-Ereignis bei der Verarbeitung der Strukturiertes-Streaming-Abfrage generiert wird, und die Visualisierung stellt die Streaminglatenz als Zeitspanne in Millisekunden dar, die die Ausführung eines Abfragebatches benötigt.</span><span class="sxs-lookup"><span data-stu-id="6e9b1-219">These metrics are sent when the OnQueryProgress event is generated as the structured streaming query is processed and the visualization represents streaming latency as the amount of time, in milliseconds, taken to execute a query batch.</span></span>

### <a name="resource-consumption-per-executor"></a><span data-ttu-id="6e9b1-220">Ressourcenverbrauch pro Executor</span><span class="sxs-lookup"><span data-stu-id="6e9b1-220">Resource consumption per executor</span></span>

<span data-ttu-id="6e9b1-221">Als Nächstes wird in einer Reihe von Visualisierungen für das Dashboard der bestimmte Ressourcentyp und seine Nutzung pro Executor auf den einzelnen Clustern angezeigt.</span><span class="sxs-lookup"><span data-stu-id="6e9b1-221">Next is a set of visualizations for the dashboard show the particular type of resource and how it is consumed per executor on each cluster.</span></span> <span data-ttu-id="6e9b1-222">Diese Visualisierungen erleichtern das Identifizieren von Ausreißern im Ressourcenverbrauch pro Executor.</span><span class="sxs-lookup"><span data-stu-id="6e9b1-222">These visualizations help identify outliers in resource consumption per executor.</span></span> <span data-ttu-id="6e9b1-223">Wenn z.B. die Zuordnung der Arbeit für einen bestimmten Executor verzerrt ist, wird der Ressourcenverbrauch im Verhältnis zu anderen im Cluster ausgeführten Executorn erhöht.</span><span class="sxs-lookup"><span data-stu-id="6e9b1-223">For example, if the work allocation for a particular executor is skewed, resource consumption will be elevated in relation to other executors running on the cluster.</span></span> <span data-ttu-id="6e9b1-224">Dies kann anhand der Spitzen im Ressourcenverbrauch für einen Executor identifiziert werden.</span><span class="sxs-lookup"><span data-stu-id="6e9b1-224">This can be identified by spikes in the resource consumption for an executor.</span></span>

### <a name="executor-compute-time-metrics"></a><span data-ttu-id="6e9b1-225">Executor-Computezeitmetriken</span><span class="sxs-lookup"><span data-stu-id="6e9b1-225">Executor compute time metrics</span></span>

<span data-ttu-id="6e9b1-226">Als Nächstes wird in einer Reihe von Visualisierungen für das Dashboard das Verhältnis von Executorserialisierungszeit, Deserialisierungszeit, CPU-Zeit und Java-VM-Zeit zur allgemeinen Executorcomputezeit angezeigt.</span><span class="sxs-lookup"><span data-stu-id="6e9b1-226">Next is a set of visualizations for the dashboard that show the ratio of executor serialize time, deserialize time, CPU time, and Java virtual machine time to overall executor compute time.</span></span> <span data-ttu-id="6e9b1-227">Dies veranschaulicht visuell, wie viel jede dieser vier Metriken zur allgemeinen Executorverarbeitung beiträgt.</span><span class="sxs-lookup"><span data-stu-id="6e9b1-227">This demonstrates visually how much each of these four metrics are contributing to overall executor processing.</span></span>

### <a name="shuffle-metrics"></a><span data-ttu-id="6e9b1-228">Shufflemetriken</span><span class="sxs-lookup"><span data-stu-id="6e9b1-228">Shuffle metrics</span></span>

<span data-ttu-id="6e9b1-229">Der letzte Satz von Visualisierungen zeigt die Datenshufflemetriken in Verbindung mit einer alle Executor übergreifenden Strukturiertes-Streaming-Abfrage an.</span><span class="sxs-lookup"><span data-stu-id="6e9b1-229">The final set of visualizations show the data shuffle metrics associated with a structured streaming query across all executors.</span></span> <span data-ttu-id="6e9b1-230">Dazu gehören gelesene Shufflebytes, geschriebene Shufflebytes, Shufflearbeitsspeicher und Datenträgernutzung in Abfragen, in denen das Dateisystem verwendet wird.</span><span class="sxs-lookup"><span data-stu-id="6e9b1-230">These include shuffle bytes read, shuffle bytes written, shuffle memory, and disk usage in queries where the file system is used.</span></span>

## <a name="next-steps"></a><span data-ttu-id="6e9b1-231">Nächste Schritte</span><span class="sxs-lookup"><span data-stu-id="6e9b1-231">Next steps</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="6e9b1-232">Behandeln von Leistungsengpässen</span><span class="sxs-lookup"><span data-stu-id="6e9b1-232">Troubleshoot performance bottlenecks</span></span>](./performance-troubleshooting.md)

<!-- links -->

[rm-cli]: /azure/azure-resource-manager/resource-group-template-deploy-cli
[sku]: /azure/templates/Microsoft.OperationalInsights/2015-11-01-preview/workspaces#sku-object