---
title: Trainieren von Python-Modellen vom Typ „scikit-learn“ in Azure
description: Diese Referenzarchitektur veranschaulicht bewährte Methoden für die Optimierung der Hyperparameter (Trainingsparameter) eines Python-Modells vom Typ „scikit-learn“.
author: njray
ms.date: 03/07/19
ms.custom: azcat-ai
ms.openlocfilehash: 23689ff7423b681c6cf5aeb713920a98f658044f
ms.sourcegitcommit: c053e6edb429299a0ad9b327888d596c48859d4a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/20/2019
ms.locfileid: "58242241"
---
# <a name="training-of-python-scikit-learn-models-on-azure"></a>Trainieren von Python-Modellen vom Typ „scikit-learn“ in Azure

Diese Referenzarchitektur veranschaulicht bewährte Methoden für die Optimierung der Hyperparameter (Trainingsparameter) eines Python-Modells vom Typ [scikit-learn][scikit]. Eine Referenzimplementierung für diese Architektur ist auf [GitHub][github] verfügbar.

![Architekturdiagramm][0]

**Szenario:** Hier wird der Abgleich von häufig gestellten Fragen (FAQs) behandelt. In diesem Szenario wird eine Teilmenge der Stack Overflow-Fragedaten verwendet, die die ursprünglichen Fragen (als JavaScript markiert), die zugehörigen doppelten Fragen und Antworten umfasst. Das Szenario dient zum Optimieren einer scikit-learn-Pipeline für die Vorhersage der Wahrscheinlichkeit, dass eine doppelte Frage einer der ursprünglichen Fragen entspricht.

Die Verarbeitung dieses Szenarios umfasst folgende Schritte:

1. Das Python-Trainingsskript wird an [Azure Machine Learning Service][aml] übermittelt.

1. Das Skript wird in Docker-Containern ausgeführt, die auf den einzelnen Knoten erstellt werden.

1. Dieses Skript ruft Trainings- und Testdaten aus [Azure Storage][storage] ab.

1. Das Skript lernt anhand der Trainingsdaten unter Verwendung der entsprechenden Kombination aus Trainingsparametern ein Modell.

1. Die Leistung des Modells wird anhand der Testdaten ermittelt und in Azure Storage geschrieben.

1. Das Modell mit der besten Leistung wird bei Azure Machine Learning Service registriert.

Überlegungen im Zusammenhang mit dem Trainieren von Deep  Learning-Modellen mit GPUs finden Sie [hier][training-deep-learning].

## <a name="architecture"></a>Architecture

Diese Architektur besteht aus mehreren Azure-Clouddiensten, die Ressourcen nach Bedarf skalieren. Im Anschluss werden die Dienste und ihre Rollen in dieser Lösung beschrieben.

[Microsoft Data Science Virtual Machine][dsvm] (DSVM) ist ein VM-Image, das mit Tools für die Datenanalyse und für maschinelles Lernen konfiguriert ist. Es sind sowohl Windows Server- als auch Linux-Versionen verfügbar. In dieser Bereitstellung werden die Linux-Editionen von DSVM unter Ubuntu 16.04 LTS verwendet.

[Azure Machine Learning Service][aml] dient zum Trainieren, Bereitstellen, Automatisieren und Verwalten von Machine Learning-Modellen in der Cloud. Der Dienst wird in dieser Architektur verwendet, um die Zuordnung und Verwendung der weiter unten beschriebenen Azure-Ressourcen zu verwalten.

Die Ressource [Azure Machine Learning Compute][aml-compute] wird verwendet, um Machine Learning- und KI-Modelle in Azure bedarfsabhängig zu trainieren und zu testen. Das [Computeziel][target] ist in diesem Szenario ein Cluster mit Knoten, die nach Bedarf auf der Grundlage einer automatischen [Skalierungsoption][scaling] zugeordnet werden. Jeder Knoten ist ein virtueller Computer, der einen Trainingsauftrag für bestimmte [Hyperparameter][hyperparameter] ausführt.

[Azure Container Registry][acr] speichert Images für alle Arten von Docker-Containerbereitstellungen. Diese Container werden auf jedem Knoten erstellt und zum Ausführen des Python-Trainingsskripts verwendet. Das im Machine Learning Compute-Cluster verwendete Image wird von Machine Learning Service in den Notebooks für die lokale Ausführung und die Hyperparameteroptimierung erstellt und anschließend mithilfe von Push an Container Registry übertragen.

[Azure Blob Storage][blob] empfängt die vom Python-Trainingsskript verwendeten Trainings- und Testdatasets von Machine Learning Service. Storage wird als virtuelles Laufwerk auf jedem Knoten eines Machine Learning Compute-Clusters eingebunden. 

## <a name="performance-considerations"></a>Überlegungen zur Leistung

Die einzelnen Gruppen von [Hyperparametern][hyperparameter] werden jeweils auf einem Knoten des Machine Learning-[Computeziels][target] ausgeführt. In dieser Architektur ist jeder Knoten ein virtueller Computer vom Typ „Standard D4 v2“ mit vier Kernen. Diese Architektur verwendet eine [LightGBM][lightgbm]-Klassifizierung für Machine Learning (ein Gradient Boosting-Framework). Diese Software kann parallel auf allen vier Kernen ausgeführt werden und somit jede Ausführung um das bis zu Vierfache beschleunigen. Verglichen mit der Ausführung auf einem Machine Learning-Computeziel mit virtuellen Computern vom Typ „Standard D1 v2“, die jeweils nur über einen einzelnen Kern verfügen, lässt sich dadurch die Gesamtdauer der Hyperparameteroptimierung auf bis zu ein Viertel der Zeit verkürzen.

Die maximale Anzahl von Machine Learning Compute-Knoten wirkt sich auf die Gesamtlaufzeit aus. Die empfohlene Mindestanzahl für die Knoten ist Null. Bei Verwendung dieser Einstellung beinhaltet die Zeit, die bis zum Start eines Auftrags vergeht, einige Minuten für die automatische Skalierung auf mindestens einen Knoten im Cluster. Wenn die Hyperparameteroptimierung allerdings nicht lange dauert, erhöht sich durch zentrales Hochskalieren des Auftrags der Mehraufwand. So kann es beispielsweise sein, dass ein Auftrag in unter fünf Minuten ausgeführt wird, das zentrale Hochskalieren auf einen Knoten jedoch weitere fünf Minuten dauert. In diesem Fall ist es schneller, das Minimum auf einen einzelnen Knoten festzulegen. Dadurch fallen allerdings höhere Kosten an.

## <a name="monitoring-and-logging-considerations"></a>Überlegungen zur Überwachung und Protokollierung

Übermitteln Sie eine [HyperDrive][hyperparameter]-Laufzeitkonfiguration, um ein Ausführungsobjekt zur Überwachung des Ausführungsfortschritts zurückzugeben.

### <a name="rundetails-jupyter-widget"></a>Jupyter-Widget „RunDetails“

Verwenden Sie das Ausführungsobjekt mit dem [Jupyter-Widget][jupyter] „RunDetails“, um beim Hinzufügen zur Warteschlange sowie beim Ausführen der entsprechenden untergeordneten Aufträge bequem den Fortschritt zu überwachen. Das Widget zeigt auch die in Echtzeit protokollierten Werte der primären Statistik an.

### <a name="azure-portal"></a>Azure-Portal

Geben Sie ein Ausführungsobjekt aus, um einen Link zur Seite der Ausführung im Azure-Portal anzuzeigen:

![Ausführungsobjekt][1]

Auf dieser Seite können Sie den Status der Ausführung und der dazugehörigen untergeordneten Ausführungen überwachen. Jede untergeordnete Ausführung verfügt über ein Treiberprotokoll mit der Ausgabe des ausgeführten Trainingsskripts.

## <a name="cost-considerations"></a>Kostenbetrachtung

Die Kosten für die Ausführung einer Hyperparameteroptimierung hängen direkt davon ab, welche VM-Größe für Machine Learning Compute gewählt wurde, ob Knoten mit niedriger Priorität verwendet werden und wie viele Knoten im Cluster maximal zulässig sind.

Die laufenden Kosten während der Nichtverwendung des Clusters hängen davon ab, wie viele Knoten der Cluster mindestens benötigt. Durch die automatische Clusterskalierung fügt das System automatisch eine der Auftragsanzahl entsprechende Anzahl von Knoten hinzu, bis der zulässige Maximalwert erreicht ist. Wenn die Knoten nicht mehr benötigt werden, werden sie wieder entfernt, bis nur noch die angeforderte Mindestanzahl vorhanden ist. Falls der Cluster auf null Knoten herunterskaliert werden kann, fallen für den Cluster keinerlei Kosten an, wenn er nicht verwendet wird.

## <a name="security-considerations"></a>Sicherheitshinweise

### <a name="restrict-access-to-azure-blob-storage"></a>Beschränken des Zugriffs auf Azure Blob Storage

In dieser Architektur werden [Speicherkontoschlüssel][storage-security] für den Zugriff auf Blob Storage verwendet. Für noch mehr Kontrolle und Schutz sollten Sie stattdessen Shared Access Signatur (SAS) verwenden. Dadurch wird der Zugriff auf die gespeicherten Objekte eingeschränkt, ohne dass die Kontoschlüssel hartcodiert oder als Klartext gespeichert werden müssen. Mit SAS können Sie außerdem sicherstellen, dass das Speicherkonto über eine ordnungsgemäße Governance verfügt und der Zugriff nur ausgewählten Personen gewährt wird.

Stellen Sie in Szenarien mit sensibleren Daten sicher, dass alle Ihre Speicherschlüssel geschützt sind, weil diese Schlüssel den Vollzugriff auf alle Ein- und Ausgabedaten der Workload ermöglichen.

### <a name="encrypt-data-at-rest-and-in-motion"></a>Verschlüsseln von ruhenden und übertragenen Daten

In Szenarien mit sensiblen Daten müssen ruhende Daten (Daten im Speicher) verschlüsselt werden. Bei Datenübertragungen müssen die Daten mithilfe von SSL geschützt werden. Weitere Informationen finden Sie im [Azure Storage-Sicherheitsleitfaden][storage-security].

### <a name="secure-data-in-a-virtual-network"></a>Schützen von Daten in einem virtuellen Netzwerk

Bei Produktionsbereitstellungen empfiehlt es sich ggf., den Cluster in einem Subnetz eines von Ihnen angegebenen virtuellen Netzwerks bereitzustellen. Dadurch können die Computeknoten im Cluster sicher mit anderen virtuellen Computern oder mit einem lokalen Netzwerk kommunizieren. Sie können auch [Dienstendpunkte][endpoints] mit Blob Storage verwenden, um den Zugriff über ein virtuelles Netzwerk zu ermöglichen, oder ein NFS mit einem einzelnen Knoten innerhalb des virtuellen Netzwerks mit Azure Machine Learning Service.

## <a name="deployment"></a>Bereitstellung

Führen Sie die Schritte im [GitHub-Repository][github] aus, um die Referenzimplementierung für diese Architektur bereitzustellen.

[0]: ./_images//training-python-models.png
[1]: ./_images/run-object.png
[acr]: /azure/container-registry/container-registry-intro
[ai]: /azure/application-insights/app-insights-overview
[aml]: /azure/machine-learning/service/overview-what-is-azure-ml
[aml-compute]: /azure/machine-learning/service/how-to-set-up-training-targets
[amls]: /azure/machine-learning/service/overview-what-is-azure-ml
[blob]: /azure/storage/blobs/storage-blobs-introduction
[dsvm]: /azure/machine-learning/data-science-virtual-machine/overview
[endpoints]: /azure/storage/common/storage-network-security?toc=%2fazure%2fvirtual-network%2ftoc.json#grant-access-from-a-virtual-network
[github]: https://github.com/Microsoft/MLHyperparameterTuning
[hyperparameter]: /azure/machine-learning/service/how-to-tune-hyperparameters
[jupyter]: http://jupyter.org/widgets
[lightgbm]: https://github.com/Microsoft/LightGBM
[scaling]: /azure/virtual-machine-scale-sets/overview
[scikit]: https://pypi.org/project/scikit-learn/
[storage]: /azure/storage/common/storage-introduction
[storage-security]: /azure/storage/common/storage-security-guide
[target]: /azure/machine-learning/service/how-to-auto-train-remote
[training-deep-learning]: /azure/architecture/reference-architectures/ai/training-deep-learning