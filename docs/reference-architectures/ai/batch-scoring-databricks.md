---
title: Batchbewertung von Spark-Modellen in Azure Databricks
description: Erstellen Sie eine skalierbare Lösung für die Batchbewertung eines Apache Spark-Klassifizierungsmodells nach einem Zeitplan mithilfe von Azure Databricks.
author: njray
ms.date: 02/07/2019
ms.topic: reference-architecture
ms.service: architecture-center
ms.subservice: reference-architecture
ms.custom: azcat-ai
ms.openlocfilehash: cba8d272ddbdbf2c2da94f68b288e9fb79be7de2
ms.sourcegitcommit: 1a3cc91530d56731029ea091db1f15d41ac056af
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/03/2019
ms.locfileid: "58887810"
---
# <a name="batch-scoring-of-spark-machine-learning-models-on-azure-databricks"></a>Batchbewertung von Spark-Modellen für Machine Learning in Azure Databricks

Diese Referenzarchitektur veranschaulicht, wie mit Azure Databricks, einer für Azure optimierten Apache Spark-basierten Analyseplattform, eine skalierbare Lösung für die Batchbewertung eines Apache Spark-Klassifizierungsmodells nach einem Zeitplan erstellt wird. Die Lösung kann als Vorlage verwendet werden, die für andere Szenarien generalisiert werden kann.

Eine Referenzimplementierung für diese Architektur ist auf [GitHub][github] verfügbar.

![Batchbewertung von Spark-Modellen in Azure Databricks](./_images/batch-scoring-spark.png)

**Szenario:** Ein Unternehmen in einer ressourcenlastigen Branche möchte die mit unerwarteten mechanischen Fehlern verbundenen Kosten und Ausfallzeiten minimieren. Mithilfe der von den Maschinen gesammelten IoT-Daten kann das Unternehmen ein Predictive Maintenance-Modell (Modell für die vorbeugende Wartung) erstellen. Dieses Modell ermöglicht es dem Unternehmen, Komponenten proaktiv zu warten und zu reparieren, bevor sie ausfallen. Durch Maximieren der Verwendung der mechanischen Komponenten können Kosten kontrolliert und Ausfallzeiten reduziert werden.

Ein Predictive Maintenance-Modell sammelt Daten von den Maschinen und speichert historische Beispiele von Komponentenausfällen. Das Modell kann dann verwendet werden, um den aktuellen Zustand der Komponenten zu überwachen und vorherzusagen, ob eine bestimmte Komponente in naher Zukunft ausfallen wird. Allgemeine Anwendungsfälle und Modellierungsansätze finden Sie im [Azure AI-Leitfaden für Predictive Maintenance-Lösungen][ai-guide].

Diese Referenzarchitektur ist für Workloads konzipiert, die durch das Vorhandensein neuer Daten der Komponentenmaschinen ausgelöst werden. Die Verarbeitung umfasst die folgenden Schritte:

1. Erfassen Sie die Daten aus dem externen Datenspeicher in einem Azure Databricks-Datenspeicher.

2. Trainieren Sie ein Machine Learning-Modell, indem Sie die Daten in ein Trainingsdataset transformieren, und erstellen Sie dann ein Spark MLlib-Modell. MLlib besteht aus den am häufigsten verwendeten Machine Learning-Algorithmen und -Dienstprogrammen, die für die Nutzung von Spark-Datenskalierbarkeitsfunktionen optimiert sind.

3. Wenden Sie das trainierte Modell an, um Komponentenausfälle vorherzusagen (zu klassifizieren), indem Sie die Daten in ein Bewertungsdataset transformieren. Bewerten Sie die Daten mit dem Spark MLLib-Modell.

4. Speichern Sie die Ergebnisse im Databricks-Datenspeicher zur Nachbearbeitung.

Auf  [GitHub][github] werden Notebooks für die Ausführung dieser Aufgaben bereitgestellt.

## <a name="architecture"></a>Architecture

Die Architektur definiert einen vollständig in [Azure Databricks][databricks] enthaltenen Datenfluss basierend auf einem Satz sequenziell ausgeführter [Notebooks][notebooks]. Die Architektur umfasst die folgenden Komponenten:

**[Datendateien][github]**: Bei der Referenzimplementierung wird ein simuliertes Dataset verwendet, das in fünf statischen Datendateien enthalten ist.

**[Erfassung][notebooks]**: Das Datenerfassungsnotebook lädt die Eingabedatendateien in eine Sammlung von Databricks-Datasets herunter. In einem realen Szenario würden Daten von IoT-Geräten in einen für Databricks zugänglichen Speicher wie Azure SQL Server oder Azure Blob Storage streamen. Databricks unterstützt mehrere [Datenquellen][data-sources].

**Trainingspipeline**: Dieses Notebook führt das Featureentwicklungsnotebook aus, um ein Analysedataset aus den erfassten Daten zu erstellen. Dann führt es ein Modellerstellungsnotebook aus, das das Machine Learning-Modell mithilfe der skalierbaren Machine Learning-Bibliothek [Apache Spark MLlib][mllib] trainiert.

**Bewertungspipeline**: Dieses Notebook führt das Featureentwicklungsnotebook aus, um ein Bewertungsdataset aus den erfassten Daten zu erstellen, und dann führt es das Bewertungsnotebook aus. Das Bewertungsnotebook verwendet das trainierte [Spark MLlib][mllib-spark]-Modell, um Vorhersagen für die Beobachtungen im Bewertungsdataset zu generieren. Die Vorhersagen werden im Ergebnisspeicher gespeichert. Dabei handelt es sich um ein neues Dataset für den Databricks-Datenspeicher.

**Scheduler**: Ein geplanter Databricks-[Auftrag][job] verarbeitet die Batchbewertung mit dem Spark-Modell. Der Auftrag führt das Bewertungspipelinenotebook aus, wobei variable Argumente durch Notebookparameter übergeben werden, um die Details für das Erstellen des Bewertungsdatasets und den Speicherort für das Ergebnisdataset anzugeben.

Das Szenario ist als Pipelineflow konzipiert. Jedes Notebook ist für die Ausführung in einer Batcheinstellung für jeden der folgenden Vorgänge optimiert: Erfassung, Featureentwicklung, Modellerstellung und Modellbewertung. Zu diesem Zweck ist das Featureentwicklungsnotebook so konzipiert, dass es ein allgemeines Dataset für alle Trainings-, Kalibrierungs-, Test- oder Bewertungsvorgänge generiert. In diesem Szenario verwenden wir für diese Vorgänge eine temporale Aufteilungsstrategie, sodass die Notebookparameter zum Festlegen der Datumsbereichsfilterung verwendet werden.

Weil in dem Szenario eine Batchpipeline erstellt wird, stellen wir eine Reihe optionaler Überprüfungsnotebooks bereit, um die Ausgabe der Pipelinenotebooks zu untersuchen. Sie finden diese im GitHub-Repository:

- `1a_raw-data_exploring`
- `2a_feature_exploration`
- `2b_model_testing`
- `3b_model_scoring_evaluation`

## <a name="recommendations"></a>Empfehlungen

Databricks ist so eingerichtet, dass Sie Ihre trainierten Modelle laden und bereitstellen können, um Vorhersagen mit neuen Daten zu treffen. Wir haben Databricks für dieses Szenario verwendet, weil diese Plattform die folgenden zusätzlichen Vorteile bietet:

- Unterstützung des einmaligen Anmeldens mit Azure Active Directory-Anmeldeinformationen
- Auftragsplaner zum Ausführen von Aufträgen für Produktionspipelines
- Vollständig interaktives Notebook mit Zusammenarbeitsfunktionen, Dashboards und REST-APIs
- Unbegrenzte Cluster, die auf eine beliebige Größe skaliert werden können
- Erweiterte Sicherheit, rollenbasierte Zugriffssteuerungen und Überwachungsprotokolle

Verwenden Sie für die Interaktion mit dem Azure Databricks-Dienst die Benutzeroberfläche des Databricks-[Arbeitsbereichs][workspace] in einem Webbrowser oder die Databricks-[Befehlszeilenschnittstelle][cli] (Command-Line Interface, CLI). Auf die Databricks-CLI können Sie über jede Plattform zugreifen, die Python 2.7.9 bis 3.6 unterstützt.

Bei der Referenzimplementierung werden [Notebooks][notebooks] verwendet, um Aufgaben nacheinander auszuführen. Jedes Notebook speichert Zwischendatenartefakte (Trainings-, Test-, Bewertungs- oder Ergebnisdatasets) im gleichen Datenspeicher wie die Eingabedaten. Ziel ist es, Ihnen die Verwendung nach Bedarf in Ihrem bestimmten Anwendungsfall zu erleichtern. In der Praxis würden Sie Ihre Datenquelle mit Ihrer Azure Databricks-Instanz verbinden, damit die Notebooks direkt in Ihren Speicher lesen und schreiben können.

Sie können die Auftragsausführung je nach Bedarf über die Databricks-Benutzeroberfläche, den Datenspeicher oder die Databricks-[CLI][cli] überwachen. Überwachen Sie den Cluster mithilfe des [Ereignisprotokolls][log] und anderer [Metriken][metrics], die Databricks bereitstellt.

## <a name="performance-considerations"></a>Überlegungen zur Leistung

Für einen Azure Databricks-Cluster ist die automatische Skalierung standardmäßig aktiviert, sodass Databricks während der Laufzeit Worker dynamisch neu zuordnen kann, um die Eigenschaften Ihres Auftrags zu berücksichtigen. Bestimmte Teile der Pipeline sind möglicherweise anspruchsvoller und rechenintensiver als andere. Databricks fügt während dieser Phasen Ihres Auftrags zusätzliche Worker hinzu (und entfernt diese, wenn sie nicht mehr benötigt werden). Durch die automatische Skalierung kann eine hohe [Clusterauslastung][cluster] einfacher erreicht werden, weil Sie den Cluster nicht entsprechend einer Workload bereitstellen müssen.

Darüber hinaus können mithilfe von [Azure Data Factory][adf] mit Azure Databricks komplexere geplante Pipelines entwickelt werden.

## <a name="storage-considerations"></a>Speicheraspekt

Bei dieser Referenzimplementierung werden die Daten der Einfachheit halber direkt im Databricks-Speicher gespeichert. In einer Produktionsumgebung können die Daten jedoch in einem Clouddatenspeicher wie [Azure Blob Storage][blob] gespeichert werden. [Databricks][databricks-connect] unterstützt auch Azure Data Lake Store, Azure SQL Data Warehouse, Azure Cosmos DB, Apache Kafka und Hadoop.

## <a name="cost-considerations"></a>Kostenbetrachtung

Azure Databricks ist ein Spark-Premiumangebot, das mit Kosten verbunden ist. Darüber hinaus gibt es [Tarife][pricing] für Databricks Standard und Databricks Premium.

In diesem Szenario ist der Tarif „Standard“ ausreichend. Wenn Ihre spezielle Anwendung jedoch eine automatische Skalierung von Clustern zur Bewältigung größerer Workloads oder für interaktive Databricks-Dashboards erfordert, könnten sich die Kosten durch den Tarif „Premium“ weiter erhöhen.

Die Lösungsnotebooks können auf jeder Spark-basierten Plattform mit minimalen Änderungen zum Entfernen der Databricks-spezifischen Pakete ausgeführt werden. Unter den folgenden Links finden Sie ähnliche Lösungen für verschiedene Azure-Plattformen:

- [Python für Azure Machine Learning Studio][python-aml]
- [SQL Server R Services][sql-r]
- [PySpark für Azure Data Science Virtual Machine][py-dvsm]

## <a name="deploy-the-solution"></a>Bereitstellen der Lösung

Führen Sie zum Bereitstellen dieser Referenzarchitektur die im [GitHub][github]-Repository beschriebenen Schritte aus, um eine skalierbare Lösung für die Batchbewertung von Spark-Modellen in Azure Databricks zu erstellen.

## <a name="related-architectures"></a>Verwandte Architekturen

Wir haben auch eine Referenzarchitektur erstellt, die Spark zum Erstellen von [Echtzeitempfehlungssystemen][recommendation] mit vorab berechneten Offlinebewertungen verwendet. Diese Empfehlungssysteme werden häufig in Szenarien verwendet, in denen eine Batchverarbeitung von Bewertungen erfolgt.

[adf]: https://azure.microsoft.com/blog/operationalize-azure-databricks-notebooks-using-data-factory/
[ai-guide]: /azure/machine-learning/team-data-science-process/cortana-analytics-playbook-predictive-maintenance
[blob]: https://docs.databricks.com/spark/latest/data-sources/azure/azure-storage.html
[cli]: https://docs.databricks.com/user-guide/dev-tools/databricks-cli.html
[cluster]: https://docs.azuredatabricks.net/user-guide/clusters/sizing.html
[databricks]: /azure/azure-databricks/
[databricks-connect]: /azure/azure-databricks/databricks-connect-to-data-sources
[data-sources]: https://docs.databricks.com/spark/latest/data-sources/index.html
[github]: https://github.com/Azure/BatchSparkScoringPredictiveMaintenance
[job]: https://docs.databricks.com/user-guide/jobs.html
[log]: https://docs.databricks.com/user-guide/clusters/event-log.html
[metrics]: https://docs.databricks.com/user-guide/clusters/metrics.html
[mllib]: https://docs.databricks.com/spark/latest/mllib/index.html
[mllib-spark]: https://docs.databricks.com/spark/latest/mllib/index.html#apache-spark-mllib
[notebooks]: https://docs.databricks.com/user-guide/notebooks/index.html
[pricing]: https://azure.microsoft.com/en-us/pricing/details/databricks/
[python-aml]: https://gallery.azure.ai/Notebook/Predictive-Maintenance-Modelling-Guide-Python-Notebook-1
[py-dvsm]: https://gallery.azure.ai/Tutorial/Predictive-Maintenance-using-PySpark
[recommendation]: /azure/architecture/reference-architectures/ai/real-time-recommendation
[sql-r]: https://gallery.azure.ai/Tutorial/Predictive-Maintenance-Modeling-Guide-using-SQL-R-Services-1
[workspace]: https://docs.databricks.com/user-guide/workspace.html
