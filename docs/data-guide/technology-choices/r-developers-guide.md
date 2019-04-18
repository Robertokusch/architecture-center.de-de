---
title: R-Entwicklerhandbuch für Azure – R-Programmierung | Microsoft-Dokumentation
description: Dieser Artikel enthält eine Übersicht über die verschiedenen Möglichkeiten, die Datenanalysten offen stehen, um ihre vorhandenen Fähigkeiten mit der R-Programmiersprache in Azure zu nutzen. Azure bietet viele Dienste, mit denen R-Entwickler ihre Data Science-Workloads auf die Cloud erweitern können.
services: machine-learning
author: AnalyticJeremy
ms.service: machine-learning
ms.workload: data-services
ms.devlang: R
ms.topic: article
ms.date: 03/19/2019
ms.author: jepeach
ms.openlocfilehash: 7b263d0f49b3b41c9a25b5842de908d5e507780b
ms.sourcegitcommit: 579c39ff4b776704ead17a006bf24cd4cdc65edd
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/17/2019
ms.locfileid: "59640480"
---
# <a name="r-developers-guide-to-azure"></a>R-Entwicklerleitfaden für Azure

<img src="../images/logo_r.svg" alt="R logo" align="right" width="200" />

Viele Datenanalysten müssen ständig wachsende Datenmengen verarbeiten und suchen nach Möglichkeiten, die Leistung von Cloud Computing für ihre Analysen nutzen.  Dieser Artikel enthält eine Übersicht über die verschiedenen Möglichkeiten, die Datenanalysten offen stehen, um ihre vorhandenen Fähigkeiten mit der [Programmiersprache R](https://www.r-project.org) in Azure zu nutzen.

Microsoft hat die Programmiersprache R als erstklassiges Tool für Data Scientists vollständig einbezogen.  Durch die Bereitstellung vieler verschiedener Optionen für R-Entwickler zur Ausführung ihres Codes in Azure ermöglicht das Unternehmen den Data Scientists, ihre Data Science-Workloads bei umfangreichen Projekten auf die Cloud auszuweiten.

Sehen wir uns die verschiedenen Optionen und die jeweils passendsten Szenarien an.

## <a name="azure-services-with-r-language-support"></a>Azure-Dienste mit Unterstützung der Sprache R

Dieser Artikel behandelt die folgenden Azure-Dienste, die die R-Sprache unterstützen:

|Dienst                                                          |BESCHREIBUNG                                                                       |
|-----------------------------------------------------------------|----------------------------------------------------------------------------------|
|[Virtueller Computer für Data Science](#data-science-virtual-machine)    |Eine benutzerdefinierte VM, die als Data Science-Arbeitsstation oder als benutzerdefiniertes Computeziel verwendet wird|
|[ML Services in HDInsight](#ml-services-on-hdinsight)            |Clusterbasiertes System für die Ausführung von R-Analysen für große Datasets auf zahlreichen Knoten   |
|[Azure Databricks](#azure-databricks)                            |Spark-Umgebung zur Zusammenarbeit, die R und andere Sprachen unterstützt               |
|[Azure Machine Learning Studio](#azure-machine-learning-studio)  |Ausführung benutzerdefinierter R-Skripts in Azure Machine Learning-Experimenten                      |
|[Azure Batch](#azure-batch)                                      |Angebot einer Vielzahl von Optionen zur wirtschaftlichen Ausführung von R-Code auf vielen Knoten in einem Cluster|
|[Azure Notebooks](#azure-notebooks)                              |Eine kostenfreie cloudbasierte Version von Jupyter-Notebooks                  |
|[Azure SQL-Datenbank](#azure-sql-database)                        |Ausführung von R-Skripts innerhalb der SQL Server-Datenbank-Engine                            |

## <a name="data-science-virtual-machine"></a>Virtueller Computer für Data Science

[Data Science Virtual Machine](https://docs.microsoft.com/azure/machine-learning/data-science-virtual-machine/overview) (DSVM) ist ein benutzerdefiniertes VM-Image auf der Microsoft Azure-Cloudplattform, das speziell für Data Science erstellt wurde. Dieses Image umfasst viele gängige Data Science-Tools:

* [Microsoft R Open](https://mran.microsoft.com/open/)
* [Microsoft Machine Learning Server](https://docs.microsoft.com/machine-learning-server/what-is-machine-learning-server)
* [RStudio Desktop](https://www.rstudio.com/products/rstudio/#Desktop)
* [RStudio Server](https://www.rstudio.com/products/rstudio/#Server)

DSVM kann entweder mit Windows oder Linux als Betriebssystem bereitgestellt werden.  Sie können DSVM auf zwei unterschiedliche Arten verwenden: als interaktive Arbeitsstation oder als Computeplattform für einen benutzerdefinierten Cluster.

### <a name="as-a-workstation"></a>Als Arbeitsstation

Wenn Sie schnell und einfach mit R in der Cloud beginnen möchten, ist dies die beste Lösung.  Die Umgebung ist jedem vertraut, der mit R auf einer lokalen Arbeitsstation gearbeitet hat.  Statt lokale Ressourcen zu verwenden, wird die R-Umgebung jedoch auf einem virtuellen Computer in der Cloud ausgeführt.  Wenn Ihre Daten bereits in Azure gespeichert sind, hat dies den zusätzlichen Vorteil, dass Ihre R-Skripts näher an den Daten ausgeführt werden. Statt die Daten über das Internet zu übertragen, kann der Zugriff über das interne Azure-Netzwerk erfolgen, welches viel schneller Zugriffszeiten bietet.

DSVM kann besonders für kleine Teams von R-Entwicklern nützlich sein.  Anstatt in leistungsfähige Arbeitsstationen für jeden einzelnen Entwickler zu investieren, sodass Teammitglieder sich über die zu verwendenden Versionen der verschiedenen Softwarepakete abstimmen müssen, kann jeder Entwickler ganz nach Bedarf eine Instanz von DSVM einrichten.

### <a name="as-a-compute-platform"></a>Als Computeplattform

Zusätzlich zur Verwendung als Arbeitsstation wird DSVM auch als elastisch skalierbare Computeplattform für R-Projekte verwendet werden.  Mit dem [`AzureDSVM`](https://github.com/Azure/AzureDSVM)-R-Paket können Sie das Erstellen und Löschen von DSVM-Instanzen programmgesteuert verwalten.  Sie können die Instanzen in einem Cluster zusammenfassen und eine verteilte Analyse zur Durchführung in der Cloud bereitstellen.  Dieser gesamte Prozess kann durch R-Code gesteuert werden, der auf Ihrer lokalen Arbeitsstation ausgeführt wird.

Weitere Informationen zu DSVM finden Sie unter [Einführung in Azure Data Science Virtual Machine für Linux und Windows](https://docs.microsoft.com/azure/machine-learning/data-science-virtual-machine/overview).

## <a name="ml-services-on-hdinsight"></a>ML Services in HDInsight

[Microsoft ML Services](https://docs.microsoft.com/azure/hdinsight/r-server/r-server-overview) ermöglicht Data Scientists, Statistikern und R-Programmierern bei Bedarf den Zugriff auf skalierbare, verteilte Analysemethoden in HDInsight.  Diese Lösung bietet die neuesten Funktionen für die R-basierte Analyse praktisch beliebig großer Datasets, die entweder in Azure-Blobspeicher oder Data Lake Storage geladen wurden.

Dies ist eine Lösung auf Unternehmensniveau, die Ihnen das Skalieren Ihres R-Codes auf einen Cluster ermöglicht.  Durch die Nutzung von Funktionen im [`RevoScaleR`](https://docs.microsoft.com/machine-learning-server/r-reference/revoscaler/revoscaler)
-Paket von Microsoft können Ihre R-Skripts für HDInsight Funktionen zur Datenverarbeitung auf zahlreichen Knoten in einem Cluster parallel ausführen.  Dadurch kann R Daten in einem viel größeren Maßstab verarbeiten, als dies mit Singlethread-R auf einer Arbeitsstation möglich ist.

Diese Skalierungsmöglichkeit macht ML Services in HDInsight zu einer hervorragenden Option für R-Entwickler mit riesigen Datasets.  Es bietet eine flexible und skalierbare Plattform für die Ausführung Ihrer R-Skripts in der Cloud.

Eine exemplarische Vorgehensweise zum Erstellen eines ML Services-Clusters finden Sie unter [Erste Schritte mit ML Services in Azure HDInsight](https://docs.microsoft.com/azure/hdinsight/r-server/r-server-get-started).

## <a name="azure-databricks"></a>Azure Databricks

[Azure Databricks](https://azure.microsoft.com/services/databricks/) ist eine Apache Spark-basierte Analyseplattform, die für die Microsoft Azure-Clouddienstplattform optimiert ist.  Databricks wurde zusammen mit den Gründern von Apache Spark entworfen und ist in Azure integriert, um Folgendes zu ermöglichen: Einrichtung mit nur einem Klick, optimierte Workflows und einen interaktiven Arbeitsbereich für die Zusammenarbeit von Datenspezialisten, Data Engineers und Business Analysts.

Die Zusammenarbeit in Databricks wird durch das Notebook-System der Plattform ermöglicht.  Benutzer können Notebooks zusammen mit anderen Benutzern der Systeme erstellen, freigeben und bearbeiten.  Mit diesen Notebooks können Benutzer Code schreiben, der für Spark-Cluster ausgeführt wird, die in der Databricks-Umgebung verwaltet werden.  Diese Notebooks bieten vollständige Unterstützung für R und erteilen Benutzern den Zugriff auf Spark über die Pakete `SparkR` und `sparklyr`.

Da Databricks auf Spark basiert und einen starken Fokus auf die Zusammenarbeit legt, wird die Plattform häufig von Teams aus Data Scientists verwendet, die zusammen an komplexen Analysen umfangreicher Datasets arbeiten.  Da die Notebooks in Databricks zusätzlich zu R auch andere Sprachen unterstützen, eignen sich diese Lösung besonders für Teams, in denen die Analysten verschiedene Sprachen für ihre primäre Arbeit verwenden.

Im Artikel [Was ist Azure Databricks?](https://docs.microsoft.com/azure/azure-databricks/what-is-azure-databricks) finden Sie weitere Details zur Plattform sowie Unterstützung bei den ersten Schritten.

## <a name="azure-machine-learning-studio"></a>Azure Machine Learning Studio

[Azure Machine Learning Studio](https://azure.microsoft.com/services/machine-learning-studio/) ist ein Drag & Drop-Tool für die Zusammenarbeit, mit dem Sie Lösungen für Vorhersageanalysen in der Cloud erstellen, testen und bereitstellen können.  Es ermöglicht unerfahrenen Data Scientists das Erstellen und Bereitstellen von Machine Learning-Modellen, ohne viel Code schreiben zu müssen.

Azure Machine Learning Studio unterstützt sowohl R als auch Python.  Sie können R mit Azure Machine Learning Studio auf zwei Arten verwenden.

### <a name="custom-r-scripts-in-your-experiments"></a>Benutzerdefinierte R-Skripts in Ihren Experimenten

Erstens können Sie die ML Studio-Funktionen für Datenbearbeitung und maschinelles Lernen erweitern, indem Sie benutzerdefinierte R-Skripts schreiben.
Auch wenn ML Studio eine breite Vielzahl von Modulen für die Vorbereitung und Analyse von Daten umfasst, kann es nicht die Funktionen einer ausgereiften Sprache wie R bieten. Aus diesem Grund wurde der Dienst so konzipiert, dass Sie Ihre eigenen benutzerdefinierten R-Skripts einbringen können, sobald die bereitgestellten Module Ihren Anforderungen nicht entsprechen.

Um diese Funktion zu nutzen, ziehen Sie ein Modul „R-Skript ausführen“ per Drag & Drop in Ihr Experiment.  Verwenden Sie anschließend den Code-Editor im Bereich „Eigenschaften“, um ein neues R-Skript zu schreiben oder ein vorhandenes Skript einzufügen.  Innerhalb des Skripts können Sie auf externe R-Pakete verweisen.  Sie können das Skript verwenden, um Daten zu ändern oder komplexe ML-Modelle zu trainieren, die nicht Teil der Standardbibliothek für Azure Machine Learning Studio-Modelle sind.

Eine umfassende Einführung zur Verwendung von R in ML Studio-Experimenten finden Sie unter [Erste Schritte mit R in Azure Machine Learning Studio](https://docs.microsoft.com/azure/machine-learning/studio/r-quickstart).

### <a name="create-manage-and-deploy-experiments-from-your-local-r-environment"></a>Erstellen, Verwalten und Bereitstellen von Experimenten über Ihre lokale R-Umgebung

Die andere Möglichkeit, R mit Azure Machine Learning Studio zu verwenden, ist die Verwendung des [`AzureML`](https://cran.r-project.org/web/packages/AzureML/vignettes/getting_started.html)-Pakets zum Überwachen und Steuern des Experimentprozesses mit der R-Programmierumgebung.  Dieses Paket wird von Microsoft verwaltet und ermöglicht das Hoch- und Herunterladen von Datasets in und aus Azure Machine Learning Studio, das Abfragen von Experimenten, das Veröffentlichen von R-Funktionen als Webdienste sowie das Ausführen von R-Daten über vorhandene Webdienste und das Abrufen der jeweiligen Ausgabe.

Durch dieses Paket wird die Verwendung von Azure Machine Learning Studio als skalierbare Plattform für Ihren R-Code erheblich vereinfacht.  Statt in der Benutzeroberfläche zu klicken und Elemente zu ziehen, können Sie den gesamten Bereitstellungsprozess mit den Ihnen bereits vertrauten Tools automatisieren.

## <a name="azure-batch"></a>Azure Batch

Für umfangreiche R-Aufträge können Sie [Azure Batch](https://azure.microsoft.com/services/batch/) verwenden.  Dieser Dienst bietet Auftragsplanung und Computeverwaltung im Maßstab der Cloud, sodass Sie Ihre R-Workload auf Dutzende, Hunderte oder Tausende virtueller Computer skalieren können.  Da es sich um eine generalisierte Computeplattform handelt, gibt es einige wenige Optionen für die Ausführung von R-Aufträgen in Azure Batch.

Eine Option besteht in der Verwendung des Microsoft-Pakets [`doAzureParallel`](https://github.com/Azure/doAzureParallel).  Dieses R-Paket ist ein paralleles Back-End für das Paket `foreach`.  Es ermöglicht die parallele Ausführung jeder Iteration der `foreach`-Schleife auf einem Knoten innerhalb des Azure Batch-Clusters.  Eine Einführung in das Paket finden Sie in dem Blogbeitrag [doAzureParallel: Take advantage of Azure’s flexible compute directly from your R session (doAzureParallel: Nutzen Sie die flexiblen Computeressourcen von Azure direkt von Ihrer R-Sitzung aus)](https://azure.microsoft.com/blog/doazureparallel/).

Eine weitere Option für die Ausführung eines R-Skripts in Azure Batch besteht darin, Ihren Code mit „RScript.exe“ als Batch-Apps im Azure-Portal zu bündeln.  Eine ausführliche exemplarische Vorgehensweise finden Sie unter [R Workloads on Azure Batch](https://azure.microsoft.com/blog/r-workloads-on-azure-batch/) (R-Workloads in Azure Batch).

Als dritte Möglichkeit können Sie das [Azure Distributed Data Engineering Toolkit](https://github.com/Azure/aztk) (AZTK) verwenden. Dieses ermöglicht die Bereitstellung bedarfsgesteuerter Spark-Cluster mithilfe von Docker-Containern in Azure Batch.  Dadurch bietet sich eine preisgünstige Möglichkeit zum Ausführen von Spark-Aufträgen in Azure.  Indem Sie [SparklyR mit AZTK](https://github.com/Azure/aztk/wiki/SparklyR-on-Azure-with-AZTK) verwenden, können Ihre R-Skripts in der Cloud einfach und kostengünstig horizontal hochskaliert werden.

## <a name="azure-notebooks"></a>Azure-Notebooks

[Azure Notebooks](https://notebooks.azure.com) ist eine kostengünstige, unkomplizierte Methode für R-Entwickler, die lieber mit Notebooks arbeiten, um ihren Code in Azure zu integrieren.  Dies ist ein kostenloser Dienst für jeden, der Code mit [Jupyter](https://jupyter.org/) im Browser entwickeln und ausführen möchte. Dabei handelt es sich um ein Open Source-Projekt, das das Kombinieren von Markdowntext, ausführbarem Code und Grafiken in einer einzigen Canvas ermöglicht.

Der kostenlose Diensttarif von Azure Notebooks ist eine ideale Option für kleine Projekte, da er den Prozess jedes Notebooks auf 4GB Arbeitsspeicher und 1GB für Datasets beschränkt. Wenn Sie über diese Einschränkungen hinausgehende Rechen- und Datenleistung benötigen, können Sie Notebooks in einer Data Science Virtual Machine-Instanz ausführen. Weitere Informationen finden Sie unter [Verwalten und Konfigurieren von Projekten – Compute-Tarif](/azure/notebooks/configure-manage-azure-notebooks-projects#compute-tier).

## <a name="azure-sql-database"></a>Azure SQL-Datenbank

[Azure SQL-Datenbank](https://azure.microsoft.com/services/sql-database/) ist ein intelligenter und vollständig verwalteter Microsoft-Dienst für relationale Clouddatenbanken.  Damit können Sie das volle Potenzial von SQL Server nutzen, ohne die Infrastruktur einrichten zu müssen.  Darin enthalten ist [Machine Learning Services (R, Python) in SQL Server 2017Machine Learning-Dienste (R, Python), in SQLServer 2017](https://docs.microsoft.com/sql/advanced-analytics/what-is-sql-server-machine-learning?view=sql-server-2017), einer der Neuzugänge bei SQL.

Dieses Feature bietet eine eingebettete Predictive Analytics- und Data Science-Engine, die R-Code in einer SQL Server-Datenbank als gespeicherte Prozeduren, als T-SQL-Skripts mit R-Anweisungen oder als R-Code mit T-SQL ausführen kann.  Statt Daten aus der Datenbank zu extrahieren und in die R-Umgebung zu laden, laden Sie Ihren R-Code direkt in die Datenbank und lassen ihn neben den Daten ausführen.

Machine Learning Services ist bereits seit 2016 Bestandteil der lokalen SQL Server-Version, wurde aber in Azure SQL-Datenbank erst vor kurzem eingeführt.  Zurzeit liegt es in der [eingeschränkten Vorschauversion](https://docs.microsoft.com/sql/advanced-analytics/what-s-new-in-sql-server-machine-learning-services?view=sql-server-2017#azure-sql-database-roadmap) vor, wird jedoch weiterentwickelt.

### <a name="next-steps"></a>Nächste Schritte

* [Ausführen Ihres R-Code in Azure mit mrsdeploy](https://blog.revolutionanalytics.com/2017/03/running-your-r-code-azure.html)
* [Machine Learning Server in der Cloud](https://docs.microsoft.com/machine-learning-server/install/machine-learning-server-in-the-cloud)
* [Zusätzliche Ressourcen für Machine Learning Server und Microsoft R](https://docs.microsoft.com/machine-learning-server/resources-more)
* [R in Azure](https://github.com/yueguoguo/r-on-azure): eine Übersicht über die Pakete, Tools und Fallstudien zur Verwendung von R mit Azure

---

<sub>Für das R-Logo gilt &copy; 2016 The R Foundation. Es wird gemäß den Bedingungen der [Creative Commons Attribution-ShareAlike 4.0 International-Lizenz](https://creativecommons.org/licenses/by-sa/4.0/) verwendet.</sub>
