---
title: Filmempfehlungen in Azure
description: Automatisieren Sie Empfehlungen (beispielsweise für Filme oder Produkte) unter Verwendung von maschinellem Lernen und einer Azure DSVM-Instanz (Data Science Virtual Machine), um ein Modell in Azure zu trainieren.
author: njray
ms.date: 1/9/2019
ms.custom: azcat-ai
social_image_url: /azure/architecture/example-scenario/ai/media/architecture-movie-recommender.png
ms.openlocfilehash: 9e68f38cb61d7a3255b76a662c58907704914052
ms.sourcegitcommit: 3b15d65e7c35a19506e562c444343f8467b6a073
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/25/2019
ms.locfileid: "54908255"
---
# <a name="movie-recommendations-on-azure"></a><span data-ttu-id="3ccca-103">Filmempfehlungen in Azure</span><span class="sxs-lookup"><span data-stu-id="3ccca-103">Movie recommendations on Azure</span></span>

<span data-ttu-id="3ccca-104">In diesem Beispielszenario wird gezeigt, wie ein Unternehmen mithilfe von maschinellem Lernen Produktempfehlungen für Kunden automatisieren kann.</span><span class="sxs-lookup"><span data-stu-id="3ccca-104">This example scenario shows how a business can use machine learning to automate product recommendations for their customers.</span></span> <span data-ttu-id="3ccca-105">Eine Azure DSVM-Instanz (Data Science Virtual Machine) wird verwendet, um ein Modell in Azure zu trainieren, das Benutzern Filme auf der Grundlage von Filmbewertungen empfiehlt.</span><span class="sxs-lookup"><span data-stu-id="3ccca-105">An Azure Data Science Virtual Machine (DSVM) is used to train a model on Azure that recommends movies to users based on ratings that have been given to movies.</span></span>

<span data-ttu-id="3ccca-106">Empfehlungen sind in verschiedensten Branchen hilfreich – vom Einzelhandel über Nachrichten bis hin zu Medien.</span><span class="sxs-lookup"><span data-stu-id="3ccca-106">Recommendations can be useful in various industries from retail to news to media.</span></span> <span data-ttu-id="3ccca-107">Zu den möglichen Anwendungen zählen unter anderem Empfehlungen für Produkte in einem virtuellen Store, Empfehlungen für Nachrichten oder Beiträge sowie Musikempfehlungen.</span><span class="sxs-lookup"><span data-stu-id="3ccca-107">Potential applications include providing  product recommendations in a virtual store, providing news or post recommendations, or providing music recommendations.</span></span> <span data-ttu-id="3ccca-108">In der Vergangenheit mussten Unternehmen Assistenten einstellen und schulen, die personalisierte Empfehlungen für Kunden erstellen.</span><span class="sxs-lookup"><span data-stu-id="3ccca-108">Traditionally, businesses had to hire and train assistants to make personalized recommendations to customers.</span></span> <span data-ttu-id="3ccca-109">Heutzutage können wir dagegen mithilfe von Azure Modelle trainieren und die Präferenzen von Kunden untersuchen, um maßgeschneiderte Empfehlungen bereitzustellen.</span><span class="sxs-lookup"><span data-stu-id="3ccca-109">Today, we can  provide customized recommendations at scale by utilizing Azure to train models to understand customer preferences.</span></span>

## <a name="relevant-use-cases"></a><span data-ttu-id="3ccca-110">Relevante Anwendungsfälle</span><span class="sxs-lookup"><span data-stu-id="3ccca-110">Relevant use cases</span></span>

<span data-ttu-id="3ccca-111">Erwägen Sie dieses Szenario für folgende Anwendungsfälle:</span><span class="sxs-lookup"><span data-stu-id="3ccca-111">Consider this scenario for the following use cases:</span></span>

* <span data-ttu-id="3ccca-112">Filmempfehlungen auf einer Website</span><span class="sxs-lookup"><span data-stu-id="3ccca-112">Movie recommendations on a website.</span></span>
* <span data-ttu-id="3ccca-113">Produktempfehlungen für Verbraucher in einer mobilen App</span><span class="sxs-lookup"><span data-stu-id="3ccca-113">Consumer product recommendations in a mobile app.</span></span>
* <span data-ttu-id="3ccca-114">Nachrichtenempfehlungen in Streamingmedien</span><span class="sxs-lookup"><span data-stu-id="3ccca-114">News recommendations on streaming media.</span></span>

## <a name="architecture"></a><span data-ttu-id="3ccca-115">Architecture</span><span class="sxs-lookup"><span data-stu-id="3ccca-115">Architecture</span></span>

![Architektur eines Machine Learning-Modells zum Trainieren von Filmempfehlungen][architecture]

<span data-ttu-id="3ccca-117">Dieses Szenario behandelt das Trainieren und Auswerten des Machine Learning-Modells mit dem [ALS-Algorithmus][als] (Alternating Least Squares) von Spark auf der Grundlage eines Datasets mit Filmbewertungen.</span><span class="sxs-lookup"><span data-stu-id="3ccca-117">This scenario covers the training and evaluating of the machine learning model using the Spark [alternating least squares][als] (ALS) algorithm on a dataset of movie ratings.</span></span> <span data-ttu-id="3ccca-118">Das Szenario umfasst folgende Schritte:</span><span class="sxs-lookup"><span data-stu-id="3ccca-118">The steps for this scenario are as following:</span></span>

1. <span data-ttu-id="3ccca-119">Das Front-End (Website oder App-Dienst) sammelt historische Daten zu den Filminteraktionen von Benutzern – dargestellt in einer Tabelle mit Benutzer, Element und numerischen Bewertungstupeln.</span><span class="sxs-lookup"><span data-stu-id="3ccca-119">The front-end website or app service collects historical data of user-movie interactions, which are represented in a table of user, item, and numerical rating tuples.</span></span>

2. <span data-ttu-id="3ccca-120">Die gesammelten historischen Daten werden in Blob Storage gespeichert.</span><span class="sxs-lookup"><span data-stu-id="3ccca-120">The collected historical data is stored in a blob storage.</span></span>

3. <span data-ttu-id="3ccca-121">Eine DSVM-Instanz wird häufig verwendet, um ein Spark-ALS-Empfehlungsmodell für Experimente oder zur Produktentwicklung zu nutzen.</span><span class="sxs-lookup"><span data-stu-id="3ccca-121">A DSVM is often used to experiment with or productize a Spark ALS recommender model.</span></span> <span data-ttu-id="3ccca-122">Das ALS Modell wird mithilfe eines Trainingsdatasets trainiert, das auf der Grundlage des Gesamtdataset unter Anwendung einer geeigneten Datenaufteilungsstrategie erstellt wird.</span><span class="sxs-lookup"><span data-stu-id="3ccca-122">The ALS model is trained using a training dataset, which is produced from the overall dataset by applying the appropriate data splitting strategy.</span></span> <span data-ttu-id="3ccca-123">Das Dataset kann je nach geschäftlichen Anforderungen beispielsweise nach dem Zufallsprinzip, chronologisch oder geschichtet aufgeteilt werden.</span><span class="sxs-lookup"><span data-stu-id="3ccca-123">For example, the dataset can be split into sets randomly, chronologically, or stratified, depending on the business requirement.</span></span> <span data-ttu-id="3ccca-124">Ein Empfehlungssystem wird ähnlich wie bei anderen Machine Learning-Aufgaben anhand von Auswertungsmetriken (precision\@*k*, recall\@*k*, [MAP][map], [nDCG\@k][ndcg] und Ähnliches) überprüft.</span><span class="sxs-lookup"><span data-stu-id="3ccca-124">Similar to other machine learning tasks, a recommender is validated by using evaluation metrics (for example, precision\@*k*, recall\@*k*, [MAP][map], [nDCG\@k][ndcg]).</span></span>

4. <span data-ttu-id="3ccca-125">Zur Koordinierung der Experimente (wie etwa Hyperparameter-Sweeping und Modellverwaltung) wird Azure Machine Learning Service verwendet.</span><span class="sxs-lookup"><span data-stu-id="3ccca-125">Azure Machine Learning service is used for coordinating the experimentation, such as hyperparameter sweeping and model management.</span></span>

5. <span data-ttu-id="3ccca-126">Ein trainiertes Modell wird in Azure Cosmos DB gespeichert und kann später zur Empfehlung der besten *k* Filme für einen bestimmten Benutzer verwendet werden.</span><span class="sxs-lookup"><span data-stu-id="3ccca-126">A trained model is preserved on Azure Cosmos DB, which can then be applied for recommending the top *k* movies for a given user.</span></span>

6. <span data-ttu-id="3ccca-127">Das Modell wird dann mithilfe von Azure Container Instances oder Azure Kubernetes Service in einem Web- oder App-Dienst bereitgestellt.</span><span class="sxs-lookup"><span data-stu-id="3ccca-127">The model is then deployed onto a web or app service by using Azure Container Instances or Azure Kubernetes Service.</span></span>

<span data-ttu-id="3ccca-128">Eine ausführliche Anleitung für die Erstellung und Skalierung eines Empfehlungsdiensts finden Sie unter [Erstellen einer Echtzeitempfehlungs-API in Azure][ref-arch].</span><span class="sxs-lookup"><span data-stu-id="3ccca-128">For an in-depth guide to building and scaling a recommender service, see [Build a real-time recommendation API on Azure][ref-arch].</span></span>

### <a name="components"></a><span data-ttu-id="3ccca-129">Komponenten</span><span class="sxs-lookup"><span data-stu-id="3ccca-129">Components</span></span>

* <span data-ttu-id="3ccca-130">[Data Science Virtual Machine][dsvm] (DSVM) ist ein virtueller Computer mit Deep Learning-Frameworks und Tools für Machine Learning und Data Science.</span><span class="sxs-lookup"><span data-stu-id="3ccca-130">[Data Science Virtual Machine][dsvm] (DSVM) is an Azure virtual machine with deep learning frameworks and tools for machine learning and data science.</span></span> <span data-ttu-id="3ccca-131">Die DSVM-Instanz verfügt über eine eigenständige Spark-Umgebung, die zum Ausführen von ALS verwendet werden kann.</span><span class="sxs-lookup"><span data-stu-id="3ccca-131">The DSVM has a standalone Spark environment that can be used to run ALS.</span></span>

* <span data-ttu-id="3ccca-132">[Azure Blob Storage][blob] wird zum Speichern des Datasets für Filmempfehlungen verwendet.</span><span class="sxs-lookup"><span data-stu-id="3ccca-132">[Azure Blob storage][blob] stores the dataset for movie recommendations.</span></span>

* <span data-ttu-id="3ccca-133">[Azure Machine Learning Service][mls] wird verwendet, um die Erstellung, Verwaltung und Bereitstellung von Machine Learning-Modellen zu beschleunigen.</span><span class="sxs-lookup"><span data-stu-id="3ccca-133">[Azure Machine Learning service][mls] is used to accelerate the building, managing, and deploying of machine learning models.</span></span>

* <span data-ttu-id="3ccca-134">[Azure Cosmos DB][cosmosdb] ermöglicht die Verwendung eines global verteilten Multimodell-Datenbankspeichers.</span><span class="sxs-lookup"><span data-stu-id="3ccca-134">[Azure Cosmos DB][cosmosdb] enables globally distributed and multi-model database storage.</span></span>

* <span data-ttu-id="3ccca-135">[Azure Container Instances][aci] wird verwendet, um die trainierten Modelle für Web- oder App-Dienste bereitzustellen (optional unter Verwendung von [Azure Kubernetes Service][aks]).</span><span class="sxs-lookup"><span data-stu-id="3ccca-135">[Azure Container Instances][aci] is used to deploy the trained models to web or app services, optionally using [Azure Kubernetes Service][aks].</span></span>

### <a name="alternatives"></a><span data-ttu-id="3ccca-136">Alternativen</span><span class="sxs-lookup"><span data-stu-id="3ccca-136">Alternatives</span></span>

<span data-ttu-id="3ccca-137">[Azure Databricks][databricks] ist ein verwalteter Spark-Cluster, in dem Modelle trainiert und ausgewertet werden.</span><span class="sxs-lookup"><span data-stu-id="3ccca-137">[Azure Databricks][databricks] is a managed Spark cluster where model training and evaluating is performed.</span></span> <span data-ttu-id="3ccca-138">Eine verwaltete Spark-Umgebung ist in wenigen Minuten eingerichtet und [automatisch skalierbar][autoscale], was zur Reduzierung des Ressourcenbedarfs und der Kosten im Zusammenhang mit der manuellen Clusterskalierung beiträgt.</span><span class="sxs-lookup"><span data-stu-id="3ccca-138">You can set up a managed Spark environment in minutes, and [autoscale][autoscale] up and down to help reduce the resources and costs associated with scaling clusters manually.</span></span> <span data-ttu-id="3ccca-139">Zur Ressourcenschonung können Sie auch festlegen, dass inaktive [Cluster][clusters] automatisch beendet werden sollen.</span><span class="sxs-lookup"><span data-stu-id="3ccca-139">Another resource-saving option is to configure inactive [clusters][clusters] to terminate automatically.</span></span>

## <a name="considerations"></a><span data-ttu-id="3ccca-140">Überlegungen</span><span class="sxs-lookup"><span data-stu-id="3ccca-140">Considerations</span></span>

### <a name="availability"></a><span data-ttu-id="3ccca-141">Verfügbarkeit</span><span class="sxs-lookup"><span data-stu-id="3ccca-141">Availability</span></span>

<span data-ttu-id="3ccca-142">Machine Learning-basierte Apps sind in zwei Ressourcenkomponenten unterteilt: Trainingsressourcen und Bereitstellungsressourcen.</span><span class="sxs-lookup"><span data-stu-id="3ccca-142">Machine-learning-built apps are split into two resource components: resources for training, and resources for serving.</span></span> <span data-ttu-id="3ccca-143">Für Trainingsressourcen ist in der Regel keine Hochverfügbarkeit erforderlich, da diese Ressourcen nicht direkt von Liveanforderungen aus der Produktionsumgebung betroffen sind.</span><span class="sxs-lookup"><span data-stu-id="3ccca-143">Resources required for training generally do not need  high availability, as live production requests do not directly hit these resources.</span></span> <span data-ttu-id="3ccca-144">Bereitstellungsrelevante Ressourcen müssen hochverfügbar sein, um Kundenanforderungen bedienen zu können.</span><span class="sxs-lookup"><span data-stu-id="3ccca-144">Resources required for serving need to have high availability to serve customer requests.</span></span>

<span data-ttu-id="3ccca-145">Zu Trainingszwecken ist die DSVM-Instanz in [mehreren Regionen][regions] auf der ganzen Welt verfügbar und erfüllt die Anforderungen der [Vereinbarung zum Servicelevel][sla] (Service Level Agreement, SLA) für virtuelle Computer.</span><span class="sxs-lookup"><span data-stu-id="3ccca-145">For training, the DSVM is available in [multiple regions][regions] around the globe and meets the [service level agreement][sla] (SLA) for virtual machines.</span></span> <span data-ttu-id="3ccca-146">Für die Bereitstellung bietet Azure Kubernetes Service eine [hochverfügbare][ha] Infrastruktur.</span><span class="sxs-lookup"><span data-stu-id="3ccca-146">For serving, Azure Kubernetes Service provides a [highly available][ha] infrastructure.</span></span> <span data-ttu-id="3ccca-147">Agent-Knoten erfüllen ebenfalls die Anforderungen der [SLA][sla-aks] für virtuelle Computer.</span><span class="sxs-lookup"><span data-stu-id="3ccca-147">Agent nodes also follow the [SLA][sla-aks] for virtual machines.</span></span>

### <a name="scalability"></a><span data-ttu-id="3ccca-148">Skalierbarkeit</span><span class="sxs-lookup"><span data-stu-id="3ccca-148">Scalability</span></span>

<span data-ttu-id="3ccca-149">Bei großen Daten kann die DSVM-Instanz skaliert werden, um die Trainingsdauer zu verkürzen.</span><span class="sxs-lookup"><span data-stu-id="3ccca-149">If you have a large data size, you can scale your DSVM to shorten training time.</span></span> <span data-ttu-id="3ccca-150">Sie können einen virtuellen Computer zentral hoch- oder herunterskalieren, indem Sie die [VM-Größe][vm-size] ändern.</span><span class="sxs-lookup"><span data-stu-id="3ccca-150">You can scale a VM up or down by changing the [VM size][vm-size].</span></span> <span data-ttu-id="3ccca-151">Wählen Sie die Größe des Arbeitsspeichers so, dass Ihr Dataset hineinpasst, und verwenden Sie eine höhere vCPU-Anzahl, um das Training zu beschleunigen.</span><span class="sxs-lookup"><span data-stu-id="3ccca-151">Choose a memory size large enough to fit your dataset in-memory and a higher vCPU count in order to decrease the amount of time that training takes.</span></span>

### <a name="security"></a><span data-ttu-id="3ccca-152">Sicherheit</span><span class="sxs-lookup"><span data-stu-id="3ccca-152">Security</span></span>

<span data-ttu-id="3ccca-153">In diesem Szenario können Benutzer für den [Zugriff auf die DSVM-Instanz][dsvm-id], die Ihren Code, Ihre Modelle und Ihre Daten (im Arbeitsspeicher) enthält, mithilfe von Azure Active Directory authentifiziert werden.</span><span class="sxs-lookup"><span data-stu-id="3ccca-153">This scenario can use Azure Active Directory to authenticate users for [access to the DSVM][dsvm-id], which contains your code, models, and (in-memory) data.</span></span> <span data-ttu-id="3ccca-154">Daten werden vor dem Laden in eine DSVM-Instanz in Azure Storage gespeichert, wo sie automatisch unter Verwendung der [Speicherdienstverschlüsselung][storage-security] verschlüsselt werden.</span><span class="sxs-lookup"><span data-stu-id="3ccca-154">Data is stored in Azure Storage prior to being loaded on a DSVM, where it is automatically encrypted using [Storage Service Encryption][storage-security].</span></span> <span data-ttu-id="3ccca-155">Berechtigungen können über die Azure Active Directory-Authentifizierung oder mithilfe der rollenbasierten Zugriffssteuerung verwaltet werden.</span><span class="sxs-lookup"><span data-stu-id="3ccca-155">Permissions can be managed via Azure Active Directory authentication or role-based access control.</span></span>

## <a name="deploy-this-scenario"></a><span data-ttu-id="3ccca-156">Bereitstellen dieses Szenarios</span><span class="sxs-lookup"><span data-stu-id="3ccca-156">Deploy this scenario</span></span>

<span data-ttu-id="3ccca-157">**Voraussetzungen**: Sie benötigen ein bestehendes Azure-Konto.</span><span class="sxs-lookup"><span data-stu-id="3ccca-157">**Prerequisites**: You must have an existing Azure account.</span></span> <span data-ttu-id="3ccca-158">Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto][free] erstellen, bevor Sie beginnen.</span><span class="sxs-lookup"><span data-stu-id="3ccca-158">If you don't have an Azure subscription, create a [free account][free] before you begin.</span></span>

<span data-ttu-id="3ccca-159">Der gesamte Code für dieses Szenario steht im [Microsoft-Repository „Recommenders“][github] zur Verfügung.</span><span class="sxs-lookup"><span data-stu-id="3ccca-159">All the code for this scenario is available in the [Microsoft Recommenders repository][github].</span></span>

<span data-ttu-id="3ccca-160">Gehen Sie wie folgt vor, um das [ALS-Schnellstartnotebook][notebook] auszuführen:</span><span class="sxs-lookup"><span data-stu-id="3ccca-160">Follow these steps to run the [ALS quickstart notebook][notebook]:</span></span>

1. <span data-ttu-id="3ccca-161">[Erstellen Sie eine DSVM-Instanz][dsvm-ubuntu] über das Azure-Portal.</span><span class="sxs-lookup"><span data-stu-id="3ccca-161">[Create a DSVM][dsvm-ubuntu] from the Azure portal.</span></span>

2. <span data-ttu-id="3ccca-162">Klonen Sie das Repository im Ordner „Notebooks“:</span><span class="sxs-lookup"><span data-stu-id="3ccca-162">Clone the repo in the Notebooks folder:</span></span>

    ```shell
    cd notebooks
    git clone https://github.com/Microsoft/Recommenders
    ```

3. <span data-ttu-id="3ccca-163">Installieren Sie die Conda-Abhängigkeiten gemäß der Anleitung in der Datei [SETUP.md][setup].</span><span class="sxs-lookup"><span data-stu-id="3ccca-163">Install the conda dependencies following the steps described in the [SETUP.md][setup] file.</span></span>

4. <span data-ttu-id="3ccca-164">Öffnen Sie in einem Browser Ihren virtuellen jupyterlab-Computer, und navigieren Sie zu `notebooks/00_quick_start/als_pyspark_movielens.ipynb`.</span><span class="sxs-lookup"><span data-stu-id="3ccca-164">In a browser, go to your jupyterlab VM and navigate to `notebooks/00_quick_start/als_pyspark_movielens.ipynb`.</span></span>

5. <span data-ttu-id="3ccca-165">Führen Sie das Notebook aus.</span><span class="sxs-lookup"><span data-stu-id="3ccca-165">Execute the notebook.</span></span>

## <a name="related-resources"></a><span data-ttu-id="3ccca-166">Zugehörige Ressourcen</span><span class="sxs-lookup"><span data-stu-id="3ccca-166">Related resources</span></span>

<span data-ttu-id="3ccca-167">Eine ausführliche Anleitung für die Erstellung und Skalierung eines Empfehlungsdiensts finden Sie unter [Erstellen einer Echtzeitempfehlungs-API in Azure][ref-arch].</span><span class="sxs-lookup"><span data-stu-id="3ccca-167">For an in-depth guide to building and scaling a recommender service, see [Build a real-time recommendation API on Azure][ref-arch].</span></span> <span data-ttu-id="3ccca-168">Tutorials und Beispiele für Empfehlungssysteme finden Sie im [Microsoft-Repository „Recommenders“][github].</span><span class="sxs-lookup"><span data-stu-id="3ccca-168">For tutorials and examples of recommendation systems, see [Microsoft Recommenders repository][github].</span></span>

[architecture]: ./media/architecture-movie-recommender.png
[aci]: /azure/container-instances/container-instances-overview
[aad]: /azure/active-directory-b2c/active-directory-b2c-overview
[aks]: /azure/aks/intro-kubernetes
[als]: https://spark.apache.org/docs/latest/ml-collaborative-filtering.html
[autoscale]: https://docs.azuredatabricks.net/user-guide/clusters/sizing.html#autoscaling
[blob]: /azure/storage/blobs/storage-blobs-introduction
[clusters]: https://docs.azuredatabricks.net/user-guide/clusters/configure.html
[cosmosdb]: /azure/cosmos-db/introduction
[databricks]: /azure/azure-databricks/what-is-azure-databricks
[dsvm]: /azure/machine-learning/data-science-virtual-machine/overview
[dsvm-id]: /azure/machine-learning/data-science-virtual-machine/dsvm-common-identity
[dsvm-ubuntu]: /azure/machine-learning/data-science-virtual-machine/dsvm-ubuntu-intro
[free]: https://azure.microsoft.com/free/?WT.mc_id=A261C142F
[github]: https://github.com/Microsoft/Recommenders
[ha]: /azure/aks/container-service-quotas
[map]: https://en.wikipedia.org/wiki/Evaluation_measures_(information_retrieval)
[mls]: /azure/machine-learning/service/
[n-tier]: /azure/architecture/reference-architectures/n-tier/n-tier-cassandra
[ndcg]: https://en.wikipedia.org/wiki/Discounted_cumulative_gain
[notebook]: https://github.com/Microsoft/Recommenders/notebooks/00_quick_start/als_pyspark_movielens.ipynb
[ref-arch]: /azure/architecture/reference-architectures/ai/real-time-recommendation
[regions]: https://azure.microsoft.com/en-us/global-infrastructure/services/?products=virtual-machines&regions=all
[resiliency]: /azure/architecture/resiliency/
[sec-docs]: /azure/security/
[setup]: https://github.com/Microsoft/Recommenders/blob/master/SETUP.md%60
[sla]: https://azure.microsoft.com/en-us/support/legal/sla/virtual-machines/v1_8/
[sla-aks]: https://azure.microsoft.com/en-us/support/legal/sla/kubernetes-service/v1_0/
[storage-security]: /azure/storage/common/storage-service-encryption
[vm-size]: /azure/virtual-machines/virtual-machines-linux-change-vm-size
