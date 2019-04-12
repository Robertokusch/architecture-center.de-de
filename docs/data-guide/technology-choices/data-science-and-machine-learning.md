---
title: Auswählen einer Machine Learning-Technologie
description: Vergleichen Sie die Optionen für die Erstellung, Bereitstellung und Verwaltung Ihrer Machine Learning-Modelle. Treffen Sie die Wahl unter den verschiedenen für Ihre Lösung verfügbaren Microsoft-Produkten.
author: MikeWasson
ms.date: 03/06/2019
ms.topic: guide
ms.service: architecture-center
ms.subservice: cloud-fundamentals
ms.openlocfilehash: 1020e938a04c6a82e6cc831e6620ec17c9a10cc7
ms.sourcegitcommit: 9854bd27fb5cf92041bbfb743d43045cd3552a69
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/27/2019
ms.locfileid: "58503229"
---
# <a name="what-are-the-machine-learning-products-at-microsoft"></a>Welche Machine Learning-Produkte bietet Microsoft?

Machine Learning ist ein Data Science-Verfahren, mit dem Computer aus vorhandenen Daten lernen können, um zukünftiges Verhalten, Ergebnisse und Trends vorherzusagen. Mithilfe von maschinellem Lernen lernen Computer ohne vorherige explizite Programmierung.

Machine Learning-Lösungen sind iterativ aufgebaut und umfassen verschiedene Phasen:

- Aufbereiten der Daten
- Experimentieren mit und Trainieren von Modellen
- Bereitstellen trainierter Modelle
- Verwalten bereitgestellter Modelle

Microsoft bietet eine Reihe von Produkten zum Aufbereiten, Erstellen, Bereitstellen und Verwalten Ihrer Modelle für maschinelles Lernen. Vergleichen Sie diese Produkte, und wählen Sie aus, was Sie benötigen, um Ihre Lösungen für maschinelles Lernen so effektiv wie möglich zu entwickeln.

## <a name="cloud-based-options"></a>Cloudbasierte Optionen

In der Azure-Cloud stehen die folgenden Optionen für maschinelles Lernen zur Verfügung.

| Cloudoptionen | Funktionsbeschreibung | Gebotene Möglichkeiten |
|-|-|-|
| [Azure Machine Learning-Dienst](#azure-machine-learning-service) | Verwalteter Clouddienst für maschinelles Lernen  | Trainieren, Bereitstellen und Verwalten von Modellen in Azure mithilfe von Python und CLI |
| [Azure Machine Learning Studio](#azure-machine-learning-studio) | Visuelle Drag&ndash;&&ndash;Drop-Oberfläche für maschinelles Lernen | Erstellen von, Experimentieren mit und Bereitstellen von Modellen mithilfe von vorkonfigurierten Algorithmen |

Wenn Sie vorgefertigte KI- und Machine Learning-Modelle verwenden möchten, können Sie mit [Azure Cognitive Services](#azure-cognitive-services) Ihren Anwendungen ganz einfach intelligente Features hinzufügen.

## <a name="on-premises-options"></a>Lokale Optionen

In der lokalen Umgebung stehen für maschinelles Lernen die folgenden Optionen zur Verfügung. Lokale Server können auch auf einem virtuellen Computer in der Cloud ausgeführt werden.

| Lokale&nbsp;Optionen | Funktionsbeschreibung | Gebotene Möglichkeiten |
|-|-|-|
| [SQL Server Machine Learning Services](#sql-server-machine-learning-services) | In SQL eingebettete Analyse-Engine | Erstellen und Bereitstellen von Modellen innerhalb von SQL Server |
| [Microsoft Machine Learning Server](#microsoft-machine-learning-server) | Eigenständiger Enterprise-Server für prädiktive Analyse | Erstellen und Bereitstellen von Modellen für vorverarbeitete Daten |

## <a name="development-platforms-and-tools"></a>Entwicklungsplattformen und-tools

Für maschinelles Lernen stehen die folgenden Entwicklungsplattformen und -tools zur Verfügung.

| Plattformen und Tools | Funktionsbeschreibung | Gebotene Möglichkeiten |
|-|-|-|
| [Azure Data Science Virtual Machine](#azure-data-science-virtual-machine) | Virtueller Computer mit vorinstallierten Data Science-Tools | Entwickeln von Lösungen für maschinelles Lernen in einer vorkonfigurierten Umgebung |
| [Azure Databricks](#azure-databricks) | Spark-basierte Analyseplattform | Erstellen und Bereitstellen von Modellen und Datenworkflows |
| [ML.NET](#mlnet) | Plattformübergreifendes Machine Learning SDK auf Open-Source-Basis | Entwickeln von Lösungen für maschinelles Lernen für .NET-Anwendungen |
| [Windows ML](#windows-ml) | Machine Learning-Plattform unter Windows 10 | Auswerten von trainierten Modellen auf einem Windows 10-Gerät |

## <a name="azure-machine-learning-service"></a>Azure Machine Learning-Dienst

[Azure Machine Learning Service](/azure/machine-learning/service/overview-what-is-azure-ml.md) ist ein vollständig verwalteter Clouddienst, der zum Trainieren, Bereitstellen und Verwalten von Machine Learning-Modellen nach Maß verwendet wird. Er unterstützt ohne Einschränkungen alle Open Source-Technologien. Sie können dadurch Zehntausende von verschiedenen Open Source-Paketen für Python wie TensorFlow, PyTorch und scikit-learn verwenden. Außerdem stehen umfassende Tools zur Verfügung, wie etwa [Azure-Notebooks](https://notebooks.azure.com/), [Jupyter-Notebooks](http://jupyter.org) oder die Erweiterung [Azure Machine Learning für Visual Studio Code](https://aka.ms/vscodetoolsforai), die das Untersuchen und Transformieren von Daten und das anschließende Trainieren und Bereitstellen von Modellen einfach machen. Der Azure Machine Learning Service beinhaltet Features zur Automatisierung der Modellgenerierung und zum einfachen, effizienten und präzisen Optimieren.

Verwenden Sie den Azure Machine Learning Service, um Machine Learning-Modelle mithilfe von Python und CLI in der Cloud zu trainieren, bereitzustellen und zu verwalten.

Probieren Sie die [kostenlose oder kostenpflichtige Version von Azure Machine Learning Service](http://aka.ms/AMLFree) aus.

|||
|-|-|
|**Typ**                   |Cloudbasierte Lösung für maschinelles Lernen|
|**Unterstützte Sprachen**    |Python|
|**Phasen beim maschinellen Lernen**|Vorbereitung der Daten<br>Modelltraining<br>Bereitstellung<br>Verwaltung|
|**Hauptvorteile**           |Zentrale Verwaltung von Skripts und Ausführungsverlauf zur Vereinfachung des Vergleichs von Modellversionen<br/><br/>Komfortable Bereitstellung und Verwaltung von Modellen in der Cloud oder auf Edgegeräten|
|**Überlegungen**         |Erfordert eine gewisse Erfahrung mit dem Modellverwaltungsmodell.|

## <a name="azure-machine-learning-studio"></a>Azure Machine Learning Studio

Mit [Azure Machine Learning Studio](/azure/machine-learning/studio/) steht Ihnen ein interaktiver visueller Arbeitsbereich zur Verfügung, den Sie zum schnellen und komfortablen Erstellen, Testen und Bereitstellen vorkonfigurierter Algorithmen für maschinelles Lernen verwenden können. Machine Learning Studio veröffentlicht Modelle als Webdienste, die von benutzerdefinierten Apps oder BI-Tools wie Excel problemlos genutzt werden können.
Es ist keine Programmierung erforderlich – Sie konstruieren ihr Modell für maschinelles Lernen durch Verbinden von Datasets und Analysemodulen auf einer interaktiven Zeichnungsfläche und stellen es anschließend mit ein paar Klicks bereit.

Verwenden Sie Machine Learning Studio, wenn Sie Modelle entwickeln und bereitstellen möchten, ohne Code schreiben zu müssen.

Probieren Sie [Azure Machine Learning Studio](https://studio.azureml.net/?selectAccess=true&o=2&target=_blank) aus (kostenpflichtige und kostenlose Versionen verfügbar).

|||
|-|-|
|**Typ**                   |Cloudbasierte Drag & Drop-Lösung für maschinelles Lernen|
|**Unterstützte Sprachen**    |Python, R|
|**Phasen beim maschinellen Lernen**|Vorbereitung der Daten<br>Modelltraining<br>Bereitstellung<br>Verwaltung|
|**Hauptvorteile**           |Interaktive visuelle Oberfläche für Machine Learning-Modelle mit minimalem Programmieraufwand<br/><br/>Integrierte Jupyter Notebooks für Datenuntersuchungen<br/><br/>Direkte Bereitstellung trainierter Modelle als Azure-Webdienste|
|**Überlegungen**         |Begrenzte Skalierbarkeit: Die maximale Größe eines Trainingsdatasets beträgt 10 GB.<br/><br/>Nur online verfügbar: Es steht keine Offlineentwicklungsumgebung zur Verfügung.|

## <a name="azure-cognitive-services"></a>Azure Cognitive Services

Bei [Azure Cognitive Services](/azure/cognitive-services/welcome) handelt es sich um eine Gruppe von APIs, mit denen Sie Apps erstellen können, für die natürliche Kommunikationsmethoden genutzt werden. Mit diesen APIs können Ihre Apps mit nur wenigen Codezeilen sehen, hören, sprechen, verstehen und Benutzeranforderungen interpretieren. Fügen Sie Ihren Apps auf einfache Weise intelligente Features hinzu, z.B.:

- Emotions- und Stimmungserkennung
- Maschinelles Sehen und Spracherkennung
- Sprachverständnis (LUIS)
- Wissen und Suche

Verwenden Sie Cognitive Services, um Apps übergreifend für Geräte und Plattformen zu entwickeln. Die APIs werden ständig verbessert und lassen sich einfach einrichten.

|||
|-|-|
|**Typ**                   |APIs für die Entwicklung intelligenter Anwendungen|
|**Unterstützte Sprachen**    |je nach Dienst viele Optionen|
|**Phasen beim maschinellen Lernen**|Bereitstellung|
|**Hauptvorteile**           |Integrieren von Machine Learning-Funktionen in Anwendungen mithilfe vorab trainierter Modelle.<br/><br/>Verschiedene Modelle für natürliche Kommunikationsmethoden mithilfe der Dienste für maschinelles Sehen und Spracherkennung.|
|**Überlegungen**         |Modelle wurden vorab trainiert und sind nicht anpassbar.|

## <a name="sql-server-machine-learning-services"></a>SQL Server Machine Learning Services

[SQL Server Microsoft Machine Learning Service](https://docs.microsoft.com/sql/advanced-analytics/r/r-services) fügt SQL Server-Datenbanken statistische Analyse, Datenvisualisierung und prädiktive Analyse für relationale Daten in R und Python hinzu. R- und Python-Bibliotheken von Microsoft enthalten fortgeschrittene Modellierungs- und Machine Learning-Algorithmen, die parallel und nach Maß in SQL Server ausgeführt werden können.

Verwenden Sie SQL Server Machine Learning Services, wenn Sie integrierte KI und prädiktive Analyse von relationalen Daten in SQL Server benötigen.

|||
|-|-|
|**Typ**                   |Lokale Predictive Analytics für relationale Daten|
|**Unterstützte Sprachen**    |Python, R|
|**Phasen beim maschinellen Lernen**|Vorbereitung der Daten<br>Modelltraining<br>Bereitstellung|
|**Hauptvorteile**           |Einfache Einbeziehung in datenschichtinterne Logik durch Kapselung von Prognoselogik in einer Datenbankfunktion|
|**Überlegungen**         |Setzt eine SQL Server-Datenbank als Datenschicht für Ihre Anwendung voraus.|

## <a name="microsoft-machine-learning-server"></a>Microsoft Machine Learning Server

[Microsoft Machine Learning Server](https://docs.microsoft.com/machine-learning-server/what-is-machine-learning-server) ist ein Unternehmensserver zum Hosten und Verwalten von parallelen und verteilten Workloads für R- und Python-Prozesse. Microsoft Machine Learning Server kann unter Linux, Windows, Hadoop und Apache Spark sowie in [HDInsight](https://azure.microsoft.com/services/hdinsight/r-server/) ausgeführt werden. Es stellt ein Ausführungsmodul für Lösungen bereit, die mit [RevoScaleR](https://docs.microsoft.com/machine-learning-server/r-reference/revoscaler/revoscaler), [revoscalepy](https://docs.microsoft.com/machine-learning-server/python-reference/revoscalepy/revoscalepy-package) und [MicrosoftML-Paketen](https://docs.microsoft.com/r-server/r/concept-what-is-the-microsoftml-package) erstellt wurden, und erweitert Open-Source-R und -Python um Unterstützung für Hochleistungsanalyse, statistische Analyse, maschinelles Lernen und Datasets von erheblicher Größe. Diese Funktionalität wird anhand von eigenen Paketen bereitgestellt, die mit dem Server installiert werden. Für die Entwicklung können Sie IDEs wie beispielsweise [R Tools für Visual Studio](https://www.visualstudio.com/vs/rtvs/) und [Python Tools für Visual Studio](https://www.visualstudio.com/vs/python/) verwenden.

Verwenden Sie Microsoft Machine Learning Server, wenn Sie mit R und Python erstellte Modelle auf einem Server erstellen und betriebsbereit machen müssen, oder verteilen Sie R- und Python-Training in großem Maßstab auf einem Hadoop- oder Spark-Cluster.

|||
|-|-|
|**Typ**                   |Eigenständiger Enterprise-Server für Predictive Analytics|
|**Unterstützte Sprachen**    |Python, R|
|**Phasen beim maschinellen Lernen**|Modelltraining<br>Bereitstellung|
|**Hauptvorteile**           |Hohe Skalierbarkeit|
|**Überlegungen**         |Machine Learning Server muss in Ihrem Unternehmen bereitgestellt und verwaltet werden.|

## <a name="azure-data-science-virtual-machine"></a>Azure Data Science Virtual Machine

[Azure Data Science Virtual Machine](https://docs.microsoft.com/azure/machine-learning/data-science-virtual-machine/overview) ist eine benutzerdefinierte VM-Umgebung in der Microsoft Azure-Cloud, die speziell für Data Science konfiguriert wurde. Es hat viele beliebte Data Science und andere Tools vorinstalliert und vorkonfiguriert, damit Sie sofort intelligente Anwendungen für die erweiterte Analyse erstellen können.

Die Data Science Virtual Machine wird als Ziel für den Azure Machine Learning Service unterstützt.
Sie ist in Versionen für Windows und Linux Ubuntu verfügbar. (Azure Machine Learning Service wird unter Linux CentOS nicht unterstützt.)
Spezifische Versionsinformationen und eine Liste mit dem Lieferumfang finden Sie unter [Introduction to the Azure Data Science Virtual Machine](/azure/machine-learning/data-science-virtual-machine/overview.md) (Einführung in die Azure Data Science Virtual Machine).

Verwenden Sie die Data Science-VM, wenn Sie Ihre Aufträge auf einem einzelnen Knoten ausführen oder hosten müssen. Eine anderer Grund für die Nutzung kann das zentrale Hochskalieren eines einzelnen Computers per Remotezugriff sein.

|||
|-|-|
|**Typ**                   |Angepasste VM-Umgebung für Data Science|
|**Hauptvorteile**           |Geringerer Installations-, Verwaltungs- und Problembehandlungsaufwand für Data Science-Tools und -Frameworks.<br/><br/>Verfügbarkeit der neuesten Versionen aller gängigen Tools und Frameworks<br/><br/>VM-Optionen mit hochgradig skalierbaren Images und GPU-Funktionen für intensive Datenmodellierung|
|**Überlegungen**         |Der virtuelle Computer steht offline nicht zur Verfügung.<br/><br/>Bei der Ausführung eines virtuellen Computers fallen Azure-Gebühren an. Achten Sie daher darauf, dass er nur bei Bedarf ausgeführt wird.|

## <a name="azure-databricks"></a>Azure Databricks

[Azure Databricks](/azure/azure-databricks/what-is-azure-databricks) ist eine Apache Spark-basierte Analyseplattform, die für die Microsoft Azure-Clouddienstplattform optimiert ist. Databricks ist in Azure integriert, um Folgendes zu ermöglichen: Einrichtung mit nur einem Klick, optimierte Workflows und einen interaktiven Arbeitsbereich für die Zusammenarbeit von Datenspezialisten, Data Engineers und Business Analysts.
Verwenden Sie Python-, R-, Scala- und SQL-Code in webbasierten Notebooks zum Abfragen, Visualisieren und Modellieren von Daten.

Verwenden Sie Databricks, wenn Sie gemeinsam an der Erstellung von Lösungen für maschinelles Lernen auf Apache Spark arbeiten möchten.

|||
|-|-|
|**Typ**                   |Apache Spark-basierte Analyseplattform|
|**Unterstützte Sprachen**    |Python, R, Scala, SQL|
|**Phasen beim maschinellen Lernen**|Datenabfrage<br>Modelltraining|

## <a name="mlnet"></a>ML.NET

[ML.NET](https://docs.microsoft.com/dotnet/machine-learning/) ist ein kostenloses, plattformübergreifendes Open-Source-Framework für maschinelles Lernen, das es Ihnen ermöglicht, benutzerdefinierte Lösungen für maschinelles Lernen zu erstellen und sie in Ihre .NET-Anwendungen zu integrieren.

Verwenden Sie ML.NET, wenn Sie Lösungen für maschinelles Lernen in Ihre .NET-Anwendungen integrieren möchten.

|||
|-|-|
|**Typ**                   |Open-Source-Framework für die Entwicklung benutzerdefinierter Machine Learning-Anwendungen|
|**Unterstützte Sprachen**    |.NET|

## <a name="windows-ml"></a>Windows ML

Mit der [Windows ML](https://docs.microsoft.com/windows/uwp/machine-learning/)-Engine für Rückschlüsse können Sie trainierte Modelle für maschinelles Lernen in Ihren Anwendungen verwenden, wobei trainierte Modelle lokal auf Windows 10-Geräten ausgewertet werden.

Verwenden Sie Windows ML, wenn Sie trainierte Modelle für maschinelles Lernen innerhalb Ihrer Windows-Anwendungen einsetzen möchten.

|||
|-|-|
|**Typ**                   |Rückschluss-Engine für trainierte Modelle auf Windows-Geräten|
|**Unterstützte Sprachen**    |C#/C++, JavaScript|

## <a name="next-steps"></a>Nächste Schritte

- Informationen zu allen Entwicklungsprodukten für Künstliche Intelligenz (KI), die von Microsoft erhältlich sind, finden Sie unter [Microsoft-KI-Plattform](https://www.microsoft.com/ai)
- Schulungen zum Entwickeln von KI-Lösungen finden Sie unter [Microsoft AI School](https://aischool.microsoft.com/learning-paths)
