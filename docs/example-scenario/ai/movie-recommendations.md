---
title: Filmempfehlungen in Azure
description: Automatisieren Sie Empfehlungen (beispielsweise für Filme oder Produkte) unter Verwendung von maschinellem Lernen und einer Azure DSVM-Instanz (Data Science Virtual Machine), um ein Modell in Azure zu trainieren.
author: njray
ms.date: 01/09/2019
ms.custom: azcat-ai, AI
social_image_url: /azure/architecture/example-scenario/ai/media/architecture-movie-recommender.png
ms.topic: example-scenario
ms.service: architecture-center
ms.subservice: example-scenario
ms.openlocfilehash: be7959c1201e3a2aaf92c192d73a0ca47a990934
ms.sourcegitcommit: c053e6edb429299a0ad9b327888d596c48859d4a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/20/2019
ms.locfileid: "58249645"
---
# <a name="movie-recommendations-on-azure"></a>Filmempfehlungen in Azure

In diesem Beispielszenario wird gezeigt, wie ein Unternehmen mithilfe von maschinellem Lernen Produktempfehlungen für Kunden automatisieren kann. Eine Azure DSVM-Instanz (Data Science Virtual Machine) wird verwendet, um ein Modell in Azure zu trainieren, das Benutzern Filme auf der Grundlage von Filmbewertungen empfiehlt.

Empfehlungen sind in verschiedensten Branchen hilfreich – vom Einzelhandel über Nachrichten bis hin zu Medien. Zu den möglichen Anwendungen zählen unter anderem Empfehlungen für Produkte in einem virtuellen Store, Empfehlungen für Nachrichten oder Beiträge sowie Musikempfehlungen. In der Vergangenheit mussten Unternehmen Assistenten einstellen und schulen, die personalisierte Empfehlungen für Kunden erstellen. Heutzutage können wir dagegen mithilfe von Azure Modelle trainieren und die Präferenzen von Kunden untersuchen, um maßgeschneiderte Empfehlungen bereitzustellen.

## <a name="relevant-use-cases"></a>Relevante Anwendungsfälle

Erwägen Sie dieses Szenario für folgende Anwendungsfälle:

* Filmempfehlungen auf einer Website
* Produktempfehlungen für Verbraucher in einer mobilen App
* Nachrichtenempfehlungen in Streamingmedien

## <a name="architecture"></a>Architecture

![Architektur eines Machine Learning-Modells zum Trainieren von Filmempfehlungen][architecture]

Dieses Szenario behandelt das Trainieren und Auswerten des Machine Learning-Modells mit dem [ALS-Algorithmus][als] (Alternating Least Squares) von Spark auf der Grundlage eines Datasets mit Filmbewertungen. Das Szenario umfasst folgende Schritte:

1. Das Front-End (Website oder App-Dienst) sammelt historische Daten zu den Filminteraktionen von Benutzern – dargestellt in einer Tabelle mit Benutzer, Element und numerischen Bewertungstupeln.

2. Die gesammelten historischen Daten werden in Blob Storage gespeichert.

3. Eine DSVM-Instanz wird häufig verwendet, um ein Spark-ALS-Empfehlungsmodell für Experimente oder zur Produktentwicklung zu nutzen. Das ALS Modell wird mithilfe eines Trainingsdatasets trainiert, das auf der Grundlage des Gesamtdataset unter Anwendung einer geeigneten Datenaufteilungsstrategie erstellt wird. Das Dataset kann je nach geschäftlichen Anforderungen beispielsweise nach dem Zufallsprinzip, chronologisch oder geschichtet aufgeteilt werden. Ein Empfehlungssystem wird ähnlich wie bei anderen Machine Learning-Aufgaben anhand von Auswertungsmetriken (precision\@*k*, recall\@*k*, [MAP][map], [nDCG\@k][ndcg] und Ähnliches) überprüft.

4. Zur Koordinierung der Experimente (wie etwa Hyperparameter-Sweeping und Modellverwaltung) wird Azure Machine Learning Service verwendet.

5. Ein trainiertes Modell wird in Azure Cosmos DB gespeichert und kann später zur Empfehlung der besten *k* Filme für einen bestimmten Benutzer verwendet werden.

6. Das Modell wird dann mithilfe von Azure Container Instances oder Azure Kubernetes Service in einem Web- oder App-Dienst bereitgestellt.

Eine ausführliche Anleitung für die Erstellung und Skalierung eines Empfehlungsdiensts finden Sie unter [Erstellen einer Echtzeitempfehlungs-API in Azure][ref-arch].

### <a name="components"></a>Komponenten

* [Data Science Virtual Machine][dsvm] (DSVM) ist ein virtueller Computer mit Deep Learning-Frameworks und Tools für Machine Learning und Data Science. Die DSVM-Instanz verfügt über eine eigenständige Spark-Umgebung, die zum Ausführen von ALS verwendet werden kann.

* [Azure Blob Storage][blob] wird zum Speichern des Datasets für Filmempfehlungen verwendet.

* [Azure Machine Learning Service][mls] wird verwendet, um die Erstellung, Verwaltung und Bereitstellung von Machine Learning-Modellen zu beschleunigen.

* [Azure Cosmos DB][cosmosdb] ermöglicht die Verwendung eines global verteilten Multimodell-Datenbankspeichers.

* [Azure Container Instances][aci] wird verwendet, um die trainierten Modelle für Web- oder App-Dienste bereitzustellen (optional unter Verwendung von [Azure Kubernetes Service][aks]).

### <a name="alternatives"></a>Alternativen

[Azure Databricks][databricks] ist ein verwalteter Spark-Cluster, in dem Modelle trainiert und ausgewertet werden. Eine verwaltete Spark-Umgebung ist in wenigen Minuten eingerichtet und [automatisch skalierbar][autoscale], was zur Reduzierung des Ressourcenbedarfs und der Kosten im Zusammenhang mit der manuellen Clusterskalierung beiträgt. Zur Ressourcenschonung können Sie auch festlegen, dass inaktive [Cluster][clusters] automatisch beendet werden sollen.

## <a name="considerations"></a>Überlegungen

### <a name="availability"></a>Verfügbarkeit

Machine Learning-basierte Apps sind in zwei Ressourcenkomponenten unterteilt: Trainingsressourcen und Bereitstellungsressourcen. Für Trainingsressourcen ist in der Regel keine Hochverfügbarkeit erforderlich, da diese Ressourcen nicht direkt von Liveanforderungen aus der Produktionsumgebung betroffen sind. Bereitstellungsrelevante Ressourcen müssen hochverfügbar sein, um Kundenanforderungen bedienen zu können.

Zu Trainingszwecken ist die DSVM-Instanz in [mehreren Regionen][regions] auf der ganzen Welt verfügbar und erfüllt die Anforderungen der [Vereinbarung zum Servicelevel][sla] (Service Level Agreement, SLA) für virtuelle Computer. Für die Bereitstellung bietet Azure Kubernetes Service eine [hochverfügbare][ha] Infrastruktur. Agent-Knoten erfüllen ebenfalls die Anforderungen der [SLA][sla-aks] für virtuelle Computer.

### <a name="scalability"></a>Skalierbarkeit

Bei großen Daten kann die DSVM-Instanz skaliert werden, um die Trainingsdauer zu verkürzen. Sie können einen virtuellen Computer zentral hoch- oder herunterskalieren, indem Sie die [VM-Größe][vm-size] ändern. Wählen Sie die Größe des Arbeitsspeichers so, dass Ihr Dataset hineinpasst, und verwenden Sie eine höhere vCPU-Anzahl, um das Training zu beschleunigen.

### <a name="security"></a>Sicherheit

In diesem Szenario können Benutzer für den [Zugriff auf die DSVM-Instanz][dsvm-id], die Ihren Code, Ihre Modelle und Ihre Daten (im Arbeitsspeicher) enthält, mithilfe von Azure Active Directory authentifiziert werden. Daten werden vor dem Laden in eine DSVM-Instanz in Azure Storage gespeichert, wo sie automatisch unter Verwendung der [Speicherdienstverschlüsselung][storage-security] verschlüsselt werden. Berechtigungen können über die Azure Active Directory-Authentifizierung oder mithilfe der rollenbasierten Zugriffssteuerung verwaltet werden.

## <a name="deploy-this-scenario"></a>Bereitstellen dieses Szenarios

**Voraussetzungen**: Sie benötigen ein bestehendes Azure-Konto. Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto][free] erstellen, bevor Sie beginnen.

Der gesamte Code für dieses Szenario steht im [Microsoft-Repository „Recommenders“][github] zur Verfügung.

Gehen Sie wie folgt vor, um das [ALS-Schnellstartnotebook][notebook] auszuführen:

1. [Erstellen Sie eine DSVM-Instanz][dsvm-ubuntu] über das Azure-Portal.

2. Klonen Sie das Repository im Ordner „Notebooks“:

    ```shell
    cd notebooks
    git clone https://github.com/Microsoft/Recommenders
    ```

3. Installieren Sie die Conda-Abhängigkeiten gemäß der Anleitung in der Datei [SETUP.md][setup].

4. Öffnen Sie in einem Browser Ihren virtuellen jupyterlab-Computer, und navigieren Sie zu `notebooks/00_quick_start/als_pyspark_movielens.ipynb`.

5. Führen Sie das Notebook aus.

## <a name="related-resources"></a>Zugehörige Ressourcen

Eine ausführliche Anleitung für die Erstellung und Skalierung eines Empfehlungsdiensts finden Sie unter [Erstellen einer Echtzeitempfehlungs-API in Azure][ref-arch]. Tutorials und Beispiele für Empfehlungssysteme finden Sie im [Microsoft-Repository „Recommenders“][github].

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
