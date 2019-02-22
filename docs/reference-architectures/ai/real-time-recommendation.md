---
title: Erstellen einer Echtzeitempfehlungs-API in Azure
description: Verwenden Sie das maschinelle Lernen zum Automatisieren von Empfehlungen mit Azure Databricks und Azure Data Science Virtual Machines (DSVM), um in Azure ein Modell zu trainieren.
author: njray
ms.date: 12/12/2018
ms.topic: reference-architecture
ms.service: architecture-center
ms.subservice: reference-architecture
ms.custom: azcat-ai
ms.openlocfilehash: c4bfd6e92fc9c770a03a63355fc922d19ef27b7b
ms.sourcegitcommit: f4ed242dff8b204cfd8ebebb7778f356a19f5923
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/13/2019
ms.locfileid: "56224163"
---
# <a name="build-a-real-time-recommendation-api-on-azure"></a><span data-ttu-id="733e4-103">Erstellen einer Echtzeitempfehlungs-API in Azure</span><span class="sxs-lookup"><span data-stu-id="733e4-103">Build a real-time recommendation API on Azure</span></span>

<span data-ttu-id="733e4-104">Diese Referenzarchitektur veranschaulicht, wie Sie ein Empfehlungsmodell mit Azure Databricks trainieren und als API bereitstellen, indem Sie Azure Cosmos DB, Azure Machine Learning und Azure Kubernetes Service (AKS) verwenden.</span><span class="sxs-lookup"><span data-stu-id="733e4-104">This reference architecture shows how to train a recommendation model using Azure Databricks and deploy it as an API by using Azure Cosmos DB, Azure Machine Learning, and Azure Kubernetes Service (AKS).</span></span> <span data-ttu-id="733e4-105">Diese Architektur kann für die meisten Empfehlungsmodulszenarien generalisiert werden, z.B. Empfehlungen für Produkte, Filme und Nachrichten.</span><span class="sxs-lookup"><span data-stu-id="733e4-105">This architecture can be generalized for most recommendation engine scenarios, including recommendations for products, movies, and news.</span></span>

<span data-ttu-id="733e4-106">Eine Referenzimplementierung für diese Architektur ist auf [GitHub](https://github.com/Microsoft/Recommenders/blob/master/notebooks/05_operationalize/als_movie_o16n.ipynb) verfügbar.</span><span class="sxs-lookup"><span data-stu-id="733e4-106">A reference implementation for this architecture is available on [GitHub](https://github.com/Microsoft/Recommenders/blob/master/notebooks/05_operationalize/als_movie_o16n.ipynb).</span></span>

![Architektur eines Machine Learning-Modells zum Trainieren von Filmempfehlungen](./_images/recommenders-architecture.png)

<span data-ttu-id="733e4-108">**Szenario:** Ein Medienunternehmen möchte für seine Benutzer Empfehlungen zu Filmen bzw. Videos bereitstellen.</span><span class="sxs-lookup"><span data-stu-id="733e4-108">**Scenario**: A media organization wants to provide movie or video recommendations to its users.</span></span> <span data-ttu-id="733e4-109">Durch die Bereitstellung von personalisierten Empfehlungen kann das Unternehmen mehrere Geschäftsziele erreichen, z.B. höhere Durchklickraten, bessere Kundenbindung auf der Website und höhere Kundenzufriedenheit.</span><span class="sxs-lookup"><span data-stu-id="733e4-109">By providing personalized recommendations, the organization meets several business goals, including increased click-through rates, increased engagement on site, and higher user satisfaction.</span></span>

<span data-ttu-id="733e4-110">Diese Referenzarchitektur ist für das Trainieren und Bereitstellen einer Echtzeitempfehlungs-API ausgelegt, mit der für einen Benutzer die Top 10 der Filmempfehlungen angegeben werden kann.</span><span class="sxs-lookup"><span data-stu-id="733e4-110">This reference architecture is for training and deploying a real-time recommender service API that can provide the top 10 movie recommendations for a given user.</span></span>

<span data-ttu-id="733e4-111">Der Datenfluss für dieses Empfehlungsmodell lautet wie folgt:</span><span class="sxs-lookup"><span data-stu-id="733e4-111">The data flow for this recommendation model is as follows:</span></span>

1. <span data-ttu-id="733e4-112">Das Benutzerverhalten wird nachverfolgt.</span><span class="sxs-lookup"><span data-stu-id="733e4-112">Track user behaviors.</span></span> <span data-ttu-id="733e4-113">Beispielsweise kann von einem Back-End-Dienst protokolliert werden, wenn ein Benutzer einen Film bewertet oder auf ein Produkt oder einen Nachrichtenartikel klickt.</span><span class="sxs-lookup"><span data-stu-id="733e4-113">For example, a backend service might log when a user rates a movie or clicks a product or news article.</span></span>

2. <span data-ttu-id="733e4-114">Laden Sie die Daten aus einer verfügbaren [Datenquelle][data-source] in Azure Databricks.</span><span class="sxs-lookup"><span data-stu-id="733e4-114">Load the data into Azure Databricks from an available [data source][data-source].</span></span>

3. <span data-ttu-id="733e4-115">Bereiten Sie die Daten auf, und unterteilen Sie sie in Trainings- und Testsätze, um das Modell zu trainieren.</span><span class="sxs-lookup"><span data-stu-id="733e4-115">Prepare the data and split it into training and testing sets to train the model.</span></span> <span data-ttu-id="733e4-116">(In [diesem Leitfaden][guide] werden Optionen zum Unterteilen von Daten beschrieben.)</span><span class="sxs-lookup"><span data-stu-id="733e4-116">([This guide][guide] describes options for splitting data.)</span></span>

4. <span data-ttu-id="733e4-117">Passen Sie das [Spark-Modell für das kombinierte Filtern][als] an die Daten an.</span><span class="sxs-lookup"><span data-stu-id="733e4-117">Fit the [Spark Collaborative Filtering][als] model to the data.</span></span>

5. <span data-ttu-id="733e4-118">Werten Sie die Qualität des Modells aus, indem Sie Metriken für die Bewertung und die Rangfolge verwenden.</span><span class="sxs-lookup"><span data-stu-id="733e4-118">Evaluate the quality of the model using rating and ranking metrics.</span></span> <span data-ttu-id="733e4-119">([Dieser Leitfaden][eval-guide] enthält ausführliche Informationen zu den Metriken, mit denen Sie Ihr Empfehlungssystem auswerten können.)</span><span class="sxs-lookup"><span data-stu-id="733e4-119">([This guide][eval-guide] provides details about the metrics you can evaluate your recommender on.)</span></span>

6. <span data-ttu-id="733e4-120">Berechnen Sie die Top 10 der Empfehlungen pro Benutzer voraus, und speichern Sie das Ergebnis als Cachedaten in Azure Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="733e4-120">Precompute the top 10 recommendations per user and store as a cache in Azure Cosmos DB.</span></span>

7. <span data-ttu-id="733e4-121">Stellen Sie einen API-Dienst für AKS bereit, indem Sie die Azure Machine Learning-APIs zum Packen der API in einen Container und für die zugehörige Bereitstellung verwenden.</span><span class="sxs-lookup"><span data-stu-id="733e4-121">Deploy an API service to AKS using the Azure Machine Learning APIs to containerize and deploy the API.</span></span>

8. <span data-ttu-id="733e4-122">Wenn der Back-End-Dienst eine Anforderung von einem Benutzer erhält, können Sie die in AKS gehostete Empfehlungs-API aufrufen, um die Top 10 der Empfehlungen abzurufen und für den Benutzer anzuzeigen.</span><span class="sxs-lookup"><span data-stu-id="733e4-122">When the backend service gets a request from a user, call the recommendations API hosted in AKS to get the top 10 recommendations and display them to the user.</span></span>

## <a name="architecture"></a><span data-ttu-id="733e4-123">Architecture</span><span class="sxs-lookup"><span data-stu-id="733e4-123">Architecture</span></span>

<span data-ttu-id="733e4-124">Diese Architektur umfasst die folgenden Komponenten:</span><span class="sxs-lookup"><span data-stu-id="733e4-124">This architecture consists of the following components:</span></span>

<span data-ttu-id="733e4-125">[Azure Databricks][databricks]:</span><span class="sxs-lookup"><span data-stu-id="733e4-125">[Azure Databricks][databricks].</span></span> <span data-ttu-id="733e4-126">Databricks ist eine Entwicklungsumgebung, die zum Aufbereiten von Eingabedaten und Trainieren des Empfehlungssystemmodells in einem Spark-Cluster genutzt wird.</span><span class="sxs-lookup"><span data-stu-id="733e4-126">Databricks is a development environment used to prepare input data and train the recommender model on a Spark cluster.</span></span> <span data-ttu-id="733e4-127">Mit Azure Databricks wird auch ein interaktiver Arbeitsbereich zum Ausführen von Notebooks und zum damit verbundenen gemeinsamen Durchführen der Datenverarbeitung oder von Machine Learning-Aufgaben bereitgestellt.</span><span class="sxs-lookup"><span data-stu-id="733e4-127">Azure Databricks also provides an interactive workspace to run and collaborate on notebooks for any data processing or machine learning tasks.</span></span>

<span data-ttu-id="733e4-128">[Azure Kubernetes Service][aks] (AKS):</span><span class="sxs-lookup"><span data-stu-id="733e4-128">[Azure Kubernetes Service][aks] (AKS).</span></span> <span data-ttu-id="733e4-129">AKS wird verwendet, um eine Machine Learning-Modelldienst-API in einem Kubernetes-Cluster bereitzustellen und zu operationalisieren.</span><span class="sxs-lookup"><span data-stu-id="733e4-129">AKS is used to deploy and operationalize a machine learning model service API on a Kubernetes cluster.</span></span> <span data-ttu-id="733e4-130">AKS hostet das Containermodell und sorgt für eine Skalierbarkeit, mit der Ihre Durchsatzanforderungen erfüllt werden, sowie für die Identitäts- und Zugriffsverwaltung und die Protokollierung und Systemüberwachung.</span><span class="sxs-lookup"><span data-stu-id="733e4-130">AKS hosts the containerized model, providing scalability that meets your throughput requirements, identity and access management, and logging and health monitoring.</span></span>

<span data-ttu-id="733e4-131">[Azure Cosmos DB][cosmosdb]:</span><span class="sxs-lookup"><span data-stu-id="733e4-131">[Azure Cosmos DB][cosmosdb].</span></span> <span data-ttu-id="733e4-132">Cosmos DB ist ein global verteilter Datenbankdienst zum Speichern der empfohlenen Top-10-Filme für jeden Benutzer.</span><span class="sxs-lookup"><span data-stu-id="733e4-132">Cosmos DB is a globally distributed database service used to store the top 10 recommended movies for each user.</span></span> <span data-ttu-id="733e4-133">Azure Cosmos DB ist für dieses Szenario gut geeignet, da das Lesen der empfohlenen Artikel für einen Benutzer nur mit einer geringen Latenz verbunden ist (10 ms, 99. Perzentil).</span><span class="sxs-lookup"><span data-stu-id="733e4-133">Azure Cosmos DB is well-suited for this scenario, because it provides low latency (10 ms at 99th percentile) to read the top recommended items for a given user.</span></span>

<span data-ttu-id="733e4-134">[Azure Machine Learning Service][mls]:</span><span class="sxs-lookup"><span data-stu-id="733e4-134">[Azure Machine Learning Service][mls].</span></span> <span data-ttu-id="733e4-135">Dieser Dienst wird verwendet, um Machine Learning-Modelle nachzuverfolgen und zu verwalten und diese dann zu verpacken und in einer skalierbaren AKS-Umgebung bereitzustellen.</span><span class="sxs-lookup"><span data-stu-id="733e4-135">This service is used to track and manage machine learning models, and then package and deploy these models to a scalable AKS environment.</span></span>

<span data-ttu-id="733e4-136">[Microsoft Recommenders][github]:</span><span class="sxs-lookup"><span data-stu-id="733e4-136">[Microsoft Recommenders][github].</span></span> <span data-ttu-id="733e4-137">Dieses Open-Source-Repository enthält Hilfsprogrammcode und Beispiele, um Benutzer beim Einstieg in das Erstellen, Evaluieren und Operationalisieren eines Empfehlungssystems zu unterstützen.</span><span class="sxs-lookup"><span data-stu-id="733e4-137">This open-source repository contains utility code and samples to help users get started in building, evaluating, and operationalizing a recommender system.</span></span>

## <a name="performance-considerations"></a><span data-ttu-id="733e4-138">Überlegungen zur Leistung</span><span class="sxs-lookup"><span data-stu-id="733e4-138">Performance considerations</span></span>

<span data-ttu-id="733e4-139">Die Leistung ist für Echtzeitempfehlungen ein wichtiger Aspekt, da sich Empfehlungen normalerweise auf dem kritischen Pfad des Vorgangs befinden, den ein Benutzer auf Ihrer Website durchführt.</span><span class="sxs-lookup"><span data-stu-id="733e4-139">Performance is a primary consideration for real-time recommendations, because recommendations usually fall in the critical path of the request a user makes on your site.</span></span>

<span data-ttu-id="733e4-140">Dank der Kombination aus AKS und Azure Cosmos DB stellt diese Architektur einen guten Ausgangspunkt zum Bereitstellen von Empfehlungen für eine mittlere Workload bei minimalem Mehraufwand dar.</span><span class="sxs-lookup"><span data-stu-id="733e4-140">The combination of AKS and Azure Cosmos DB enables this architecture to provide a good starting point to provide recommendations for a medium-sized workload with minimal overhead.</span></span> <span data-ttu-id="733e4-141">Bei einem Auslastungstest mit 200 gleichzeitigen Benutzern werden bei dieser Architektur Empfehlungen mit einer durchschnittlichen Latenz von ca. 60 ms bereitgestellt, und der Durchsatz erreicht 180 Anforderungen pro Sekunde.</span><span class="sxs-lookup"><span data-stu-id="733e4-141">Under a load test with 200 concurrent users, this architecture provides recommendations at a median latency of about 60 ms and performs at a throughput of 180 requests per second.</span></span> <span data-ttu-id="733e4-142">Der Auslastungstest wurde mit der Standardkonfiguration für die Bereitstellung durchgeführt (drei AKS-Cluster vom Typ „D3 v2“ mit 12 vCPUs, 42 GB Arbeitsspeicher und 11.000 [Anforderungseinheiten (Request Units, RUs) pro Sekunde][ru] für Azure Cosmos DB).</span><span class="sxs-lookup"><span data-stu-id="733e4-142">The load test was run against the default deployment configuration (a 3x D3 v2 AKS cluster with 12 vCPUs, 42 GB of memory, and 11,000 [Request Units (RUs) per second][ru] provisioned for Azure Cosmos DB).</span></span>

![Leistungsdiagramm](./_images/recommenders-performance.png)

![Durchsatzdiagramm](./_images/recommenders-throughput.png)

<span data-ttu-id="733e4-145">Der Einsatz von Azure Cosmos DB wird empfohlen, weil dieser Dienst sofort global verteilt werden kann und nützlich ist, um alle Datenbankanforderungen Ihrer App zu erfüllen.</span><span class="sxs-lookup"><span data-stu-id="733e4-145">Azure Cosmos DB is recommended for its turnkey global distribution and usefulness in meeting any database requirements your app has.</span></span> <span data-ttu-id="733e4-146">Falls Sie eine etwas [niedrigere Latenz][latency] benötigen, können Sie anstelle von Azure Cosmos DB auch die Nutzung von [Azure Redis Cache][redis] für die Bereitstellung der Suche erwägen.</span><span class="sxs-lookup"><span data-stu-id="733e4-146">For slightly [faster latency][latency], consider using [Azure Redis Cache][redis] instead of Azure Cosmos DB to serve lookups.</span></span> <span data-ttu-id="733e4-147">Mit Redis Cache kann die Leistung von Systemen verbessert werden, für die eine starke Abhängigkeit von Daten in Back-End-Speichern besteht.</span><span class="sxs-lookup"><span data-stu-id="733e4-147">Redis Cache can improve performance of systems that rely highly on data in back-end stores.</span></span>

## <a name="scalability-considerations"></a><span data-ttu-id="733e4-148">Überlegungen zur Skalierbarkeit</span><span class="sxs-lookup"><span data-stu-id="733e4-148">Scalability considerations</span></span>

<span data-ttu-id="733e4-149">Wenn Sie nicht die Nutzung von Spark planen oder über eine kleinere Workload verfügen, für die keine Verteilung erforderlich ist, können Sie erwägen, anstelle von Azure Databricks die [Data Science Virtual Machine][dsvm] (DSVM) zu verwenden.</span><span class="sxs-lookup"><span data-stu-id="733e4-149">If you don't plan to use Spark, or you have a smaller workload where you don't need distribution, consider using [Data Science Virtual Machine][dsvm] (DSVM) instead of Azure Databricks.</span></span> <span data-ttu-id="733e4-150">DSVM ist ein virtueller Computer mit Deep Learning-Frameworks und -Tools für Machine Learning und Data Science.</span><span class="sxs-lookup"><span data-stu-id="733e4-150">DSVM is an Azure virtual machine with deep learning frameworks and tools for machine learning and data science.</span></span> <span data-ttu-id="733e4-151">Wie bei Azure Databricks auch, können alle Modelle, die Sie auf einer DSVM erstellen, per Azure Machine Learning als Dienst in AKS operationalisiert werden.</span><span class="sxs-lookup"><span data-stu-id="733e4-151">As with Azure Databricks, any model you create in a DSVM can be operationalized as a service on AKS via Azure Machine Learning.</span></span>

<span data-ttu-id="733e4-152">Stellen Sie während des Trainings in Azure Databricks einen größeren Spark-Cluster mit fester Größe bereit, oder konfigurieren Sie die [automatische Skalierung][autoscaling].</span><span class="sxs-lookup"><span data-stu-id="733e4-152">During training, provision a larger fixed-size Spark cluster in Azure Databricks or configure [autoscaling][autoscaling].</span></span> <span data-ttu-id="733e4-153">Wenn die automatische Skalierung aktiviert ist, überwacht Databricks die Auslastung Ihres Clusters und führt bei Bedarf das zentrale Hoch- oder Herunterskalieren durch.</span><span class="sxs-lookup"><span data-stu-id="733e4-153">When autoscaling is enabled, Databricks monitors the load on your cluster and scales up and downs when required.</span></span> <span data-ttu-id="733e4-154">Stellen Sie einen größeren Cluster bereit bzw. skalieren Sie horizontal hoch, falls Sie über eine hohe Datengröße verfügen und die Verarbeitungsdauer in Bezug auf die Datenaufbereitungs- oder Modellierungsaufgaben verringern möchten.</span><span class="sxs-lookup"><span data-stu-id="733e4-154">Provision or scale out a larger cluster if you have a large data size and you want to reduce the amount of time it takes for data preparation or modeling tasks.</span></span>

<span data-ttu-id="733e4-155">Skalieren Sie den AKS-Cluster, um Ihre Anforderungen an die Leistung und den Durchsatz zu erfüllen.</span><span class="sxs-lookup"><span data-stu-id="733e4-155">Scale the AKS cluster to meet your performance and throughput requirements.</span></span> <span data-ttu-id="733e4-156">Achten Sie darauf, dass Sie die Anzahl von [Pods][scale] zentral hochskalieren, um den Cluster vollständig zu nutzen, und die [Knoten][nodes] des Clusters zu skalieren, um den Bedarf Ihres Diensts zu erfüllen.</span><span class="sxs-lookup"><span data-stu-id="733e4-156">Take care to scale up the number of [pods][scale] to fully utilize the cluster, and to scale the [nodes][nodes] of the cluster to meet the demand of your service.</span></span> <span data-ttu-id="733e4-157">Weitere Informationen dazu, wie Sie Ihren Cluster skalieren, damit die Leistungs- und Durchsatzanforderungen Ihres Empfehlungsdiensts erfüllt werden, finden Sie unter [Scaling Azure Container Service Clusters][blog] (Skalieren von Azure Container Service-Clustern).</span><span class="sxs-lookup"><span data-stu-id="733e4-157">For more information on how to scale your cluster to meet the performance and throughput requirements of your recommender service, see [Scaling Azure Container Service Clusters][blog].</span></span>

<span data-ttu-id="733e4-158">Schätzen Sie zum Verwalten der Azure Cosmos DB-Leistung die Anzahl von Lesevorgängen pro Sekunde, und stellen Sie die erforderliche Anzahl von [RUs pro Sekunde][ru] (Durchsatz) bereit.</span><span class="sxs-lookup"><span data-stu-id="733e4-158">To manage Azure Cosmos DB performance, estimate the number of reads required per second, and provision the number of [RUs per second][ru] (throughput) needed.</span></span> <span data-ttu-id="733e4-159">Befolgen Sie die bewährten Methoden für die [Partitionierung und horizontale Skalierung][partition-data].</span><span class="sxs-lookup"><span data-stu-id="733e4-159">Use best practices for [partitioning and horizontal scaling][partition-data].</span></span>

## <a name="cost-considerations"></a><span data-ttu-id="733e4-160">Kostenbetrachtung</span><span class="sxs-lookup"><span data-stu-id="733e4-160">Cost considerations</span></span>

<span data-ttu-id="733e4-161">Die Hauptkostenpunkte dieses Szenarios sind:</span><span class="sxs-lookup"><span data-stu-id="733e4-161">The main drivers of cost in this scenario are:</span></span>

- <span data-ttu-id="733e4-162">Die Azure Databricks-Clustergröße, die für das Training benötigt wird.</span><span class="sxs-lookup"><span data-stu-id="733e4-162">The Azure Databricks cluster size required for training.</span></span>
- <span data-ttu-id="733e4-163">Die AKS-Clustergröße, die für die Erfüllung Ihrer Leistungsanforderungen erforderlich ist.</span><span class="sxs-lookup"><span data-stu-id="733e4-163">The AKS cluster size required to meet your performance requirements.</span></span>
- <span data-ttu-id="733e4-164">Azure Cosmos DB-Anforderungseinheiten zur Erfüllung Ihrer Leistungsanforderungen.</span><span class="sxs-lookup"><span data-stu-id="733e4-164">Azure Cosmos DB RUs provisioned to meet your performance requirements.</span></span>

<span data-ttu-id="733e4-165">Behalten Sie die Azure Databricks-Kosten im Griff, indem Sie weniger häufig erneut trainieren und den Spark-Cluster deaktivieren, wenn er nicht benötigt wird.</span><span class="sxs-lookup"><span data-stu-id="733e4-165">Manage the Azure Databricks costs by retraining less frequently and turning off the Spark cluster when not in use.</span></span> <span data-ttu-id="733e4-166">Die Kosten für AKS und Azure Cosmos DB richten sich nach dem Durchsatz und der Leistung Ihrer Website und fallen je nach Datenverkehr auf Ihrer Website höher oder niedriger aus.</span><span class="sxs-lookup"><span data-stu-id="733e4-166">The AKS and Azure Cosmos DB costs are tied to the throughput and performance required by your site and will scale up and down depending on the volume of traffic to your site.</span></span>

## <a name="deploy-the-solution"></a><span data-ttu-id="733e4-167">Bereitstellen der Lösung</span><span class="sxs-lookup"><span data-stu-id="733e4-167">Deploy the solution</span></span>

<span data-ttu-id="733e4-168">Erstellen Sie für die Bereitstellung dieser Architektur zunächst eine Azure Databricks-Umgebung, um die Daten aufzubereiten und ein Empfehlungsmodell zu trainieren:</span><span class="sxs-lookup"><span data-stu-id="733e4-168">To deploy this architecture, first create an Azure Databricks environment to prepare data and train a recommender model:</span></span>

1. <span data-ttu-id="733e4-169">Erstellen Sie einen [Azure Databricks-Arbeitsbereich][workspace].</span><span class="sxs-lookup"><span data-stu-id="733e4-169">Create an [Azure Databricks workspace][workspace].</span></span>

2. <span data-ttu-id="733e4-170">Erstellen Sie in Azure Databricks einen neuen Cluster.</span><span class="sxs-lookup"><span data-stu-id="733e4-170">Create a new cluster in Azure Databricks.</span></span> <span data-ttu-id="733e4-171">Erforderliche Konfiguration:</span><span class="sxs-lookup"><span data-stu-id="733e4-171">The following configuration is required:</span></span>

    - <span data-ttu-id="733e4-172">Clustermodus: Standard</span><span class="sxs-lookup"><span data-stu-id="733e4-172">Cluster mode: Standard</span></span>
    - <span data-ttu-id="733e4-173">Databricks-Runtimeversion: 4.1 (enthält Apache Spark 2.3.0, Scala 2.11)</span><span class="sxs-lookup"><span data-stu-id="733e4-173">Databricks Runtime Version: 4.1 (includes Apache Spark 2.3.0, Scala 2.11)</span></span>
    - <span data-ttu-id="733e4-174">Python-Version: 3</span><span class="sxs-lookup"><span data-stu-id="733e4-174">Python Version: 3</span></span>
    - <span data-ttu-id="733e4-175">Treibertyp: Standard\_DS3\_v2</span><span class="sxs-lookup"><span data-stu-id="733e4-175">Driver Type: Standard\_DS3\_v2</span></span>
    - <span data-ttu-id="733e4-176">Worker-Typ: Standard\_DS3\_v2 (Minimum und Maximum je nach Bedarf)</span><span class="sxs-lookup"><span data-stu-id="733e4-176">Worker Type: Standard\_DS3\_v2 (min and max as required)</span></span>
    - <span data-ttu-id="733e4-177">Automatische Beendigung: (wie erforderlich)</span><span class="sxs-lookup"><span data-stu-id="733e4-177">Auto Termination: (as required)</span></span>
    - <span data-ttu-id="733e4-178">Spark-Konfiguration: (wie erforderlich)</span><span class="sxs-lookup"><span data-stu-id="733e4-178">Spark Config: (as required)</span></span>
    - <span data-ttu-id="733e4-179">Umgebungsvariablen: (wie erforderlich)</span><span class="sxs-lookup"><span data-stu-id="733e4-179">Environment Variables: (as required)</span></span>

3. <span data-ttu-id="733e4-180">Klonen Sie das Repository [Microsoft Recommenders][github] auf Ihrem lokalen Computer.</span><span class="sxs-lookup"><span data-stu-id="733e4-180">Clone the [Microsoft Recommenders][github] repository on your local computer.</span></span>

4. <span data-ttu-id="733e4-181">Zippen Sie den Inhalt des Ordners „Recommenders“:</span><span class="sxs-lookup"><span data-stu-id="733e4-181">Zip the content inside the Recommenders folder:</span></span>

    ```console
    cd Recommenders
    zip -r Recommenders.zip
    ```

5. <span data-ttu-id="733e4-182">Fügen Sie die Recommenders-Bibliothek wie folgt an Ihren Cluster an:</span><span class="sxs-lookup"><span data-stu-id="733e4-182">Attach the Recommenders library to your cluster as follows:</span></span>

    1. <span data-ttu-id="733e4-183">Verwenden Sie im nächsten Menü die Option zum Importieren einer Bibliothek („To import a library, such as a jar or egg, click here“ (Klicken Sie hier, um eine Bibliothek zu importieren, z.B. JAR oder EGG)), und wählen Sie **Click here** (Hier klicken).</span><span class="sxs-lookup"><span data-stu-id="733e4-183">In the next menu, use the option to import a library ("To import a library, such as a jar or egg, click here") and press **click here**.</span></span>

    2. <span data-ttu-id="733e4-184">Wählen Sie im ersten Dropdownmenü die Option **Upload Python egg or PyPI** (Python Egg oder PyPI hochladen).</span><span class="sxs-lookup"><span data-stu-id="733e4-184">At the first drop-down menu, select the **Upload Python egg or PyPI** option.</span></span>

    3. <span data-ttu-id="733e4-185">Wählen Sie **Drop library egg here to upload** (Bibliotheks-Egg zum Hochladen hier ablegen), und wählen Sie dann die eben erstellte Datei „Recommenders.zip“ aus.</span><span class="sxs-lookup"><span data-stu-id="733e4-185">Select **Drop library egg here to upload** and select the Recommenders.zip file you just created.</span></span>

    4. <span data-ttu-id="733e4-186">Wählen Sie **Create library** (Bibliothek erstellen), um die ZIP-Datei hochzuladen und in Ihrem Arbeitsbereich bereitzustellen.</span><span class="sxs-lookup"><span data-stu-id="733e4-186">Select **Create library** to upload the .zip file and make it available in your workspace.</span></span>

    5. <span data-ttu-id="733e4-187">Fügen Sie die Bibliothek im nächsten Menü an Ihren Cluster an.</span><span class="sxs-lookup"><span data-stu-id="733e4-187">In the next menu, attach the library to your cluster.</span></span>

6. <span data-ttu-id="733e4-188">Importieren Sie das [Beispiel „ALS Movie Operationalization“][als-example] in Ihren Arbeitsbereich.</span><span class="sxs-lookup"><span data-stu-id="733e4-188">In your workspace, import the [ALS Movie Operationalization example][als-example].</span></span>

7. <span data-ttu-id="733e4-189">Führen Sie das Notebook „ALS Movie Operationalization“ aus, um die für die Erstellung einer Empfehlungs-API erforderlichen Ressourcen zu erstellen. Über die API wird die Top 10 der Filmempfehlungen für einen Benutzer bereitgestellt.</span><span class="sxs-lookup"><span data-stu-id="733e4-189">Run the ALS Movie Operationalization notebook to create the resources required to create a recommendation API that provides the top-10 movie recommendations for a given user.</span></span>

## <a name="related-architectures"></a><span data-ttu-id="733e4-190">Verwandte Architekturen</span><span class="sxs-lookup"><span data-stu-id="733e4-190">Related architectures</span></span>

<span data-ttu-id="733e4-191">Wir haben auch eine Referenzarchitektur erstellt, die Spark und Azure Databricks verwendet, um geplante [Batchbewertungsprozesse][batch-scoring] auszuführen.</span><span class="sxs-lookup"><span data-stu-id="733e4-191">We have also built a reference architecture that uses Spark and Azure Databricks to execute scheduled [batch-scoring processes][batch-scoring].</span></span> <span data-ttu-id="733e4-192">Sehen Sie sich diese Referenzarchitektur an, um eine empfohlene Vorgehensweise zum routinemäßigen Generieren neuer Empfehlungen zu verstehen.</span><span class="sxs-lookup"><span data-stu-id="733e4-192">See that reference architecture to understand a recommended approach for generating new recommendations routinely.</span></span>

<!-- links -->
[aci]: /azure/container-instances/container-instances-overview
[aad]: /azure/active-directory-b2c/active-directory-b2c-overview
[aks]: /azure/aks/intro-kubernetes
[als]: https://spark.apache.org/docs/latest/ml-collaborative-filtering.html
[als-example]: https://github.com/Microsoft/Recommenders/blob/master/notebooks/04_operationalize/als_movie_o16n.ipynb
[autoscaling]: https://docs.azuredatabricks.net/user-guide/clusters/sizing.html
[autoscale]: https://docs.azuredatabricks.net/user-guide/clusters/sizing.html#autoscaling
[availability]: /azure/architecture/checklist/availability
[batch-scoring]: /azure/architecture/reference-architectures/ai/batch-scoring-databricks
[blob]: /azure/storage/blobs/storage-blobs-introduction
[blog]: https://blogs.technet.microsoft.com/machinelearning/2018/03/20/scaling-azure-container-service-cluster/
[clusters]: https://docs.azuredatabricks.net/user-guide/clusters/configure.html
[cosmosdb]: /azure/cosmos-db/introduction
[data-source]: https://docs.azuredatabricks.net/spark/latest/data-sources/index.html
[databricks]: /azure/azure-databricks/what-is-azure-databricks
[dsvm]: /azure/machine-learning/data-science-virtual-machine/overview
[dsvm-ubuntu]: /azure/machine-learning/data-science-virtual-machine/dsvm-ubuntu-intro
[eval-guide]: https://github.com/Microsoft/Recommenders/blob/master/notebooks/03_evaluate/evaluation.ipynb
[free]: https://azure.microsoft.com/free/?WT.mc_id=A261C142F
[github]: https://github.com/Microsoft/Recommenders
[guide]: https://github.com/Microsoft/Recommenders/blob/master/notebooks/01_prepare_data/data_split.ipynb
[latency]: https://github.com/jessebenson/azure-performance
[mls]: /azure/machine-learning/service/
[n-tier]: /azure/architecture/reference-architectures/n-tier/n-tier-cassandra
[ndcg]: https://en.wikipedia.org/wiki/Discounted_cumulative_gain
[nodes]: /azure/aks/scale-cluster
[notebook]: https://github.com/Microsoft/Recommenders/notebooks/00_quick_start/als_pyspark_movielens.ipynb
[partition-data]: /azure/cosmos-db/partition-data
[redis]: /azure/redis-cache/cache-overview
[regions]: https://azure.microsoft.com/global-infrastructure/services/?products=virtual-machines&regions=all
[resiliency]: /azure/architecture/resiliency/
[ru]: /azure/cosmos-db/request-units
[sec-docs]: /azure/security/
[setup]: https://github.com/Microsoft/Recommenders/blob/master/SETUP.md%60
[scale]: /azure/aks/tutorial-kubernetes-scale
[sla]: https://azure.microsoft.com/support/legal/sla/virtual-machines/v1_8/
[vm-size]: /azure/virtual-machines/virtual-machines-linux-change-vm-size
[workspace]: https://docs.azuredatabricks.net/getting-started/index.html
