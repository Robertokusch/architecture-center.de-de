---
title: Echtzeitbewertung von R-Machine Learning-Modellen
description: Implementieren Sie mithilfe von Machine Learning Server unter Azure Kubernetes Service (AKS) einen Echtzeit-Prognosedienst in R.
author: njray
ms.date: 12/12/2018
ms.topic: reference-architecture
ms.service: architecture-center
ms.subservice: reference-architecture
ms.custom: azcat-ai
ms.openlocfilehash: 5f3cc62c81c9ef9e5c3c27b1d66badd3e481c228
ms.sourcegitcommit: 579c39ff4b776704ead17a006bf24cd4cdc65edd
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/18/2019
ms.locfileid: "59740424"
---
# <a name="real-time-scoring-of-r-machine-learning-models-on-azure"></a>Echtzeitbewertung von R-Machine Learning-Modellen in Azure

Diese Referenzarchitektur zeigt, wie Sie mithilfe von Microsoft Machine Learning Server unter Azure Kubernetes Service (AKS) einen (synchronen) Echtzeit-Prognosedienst in R implementieren. Diese generische Architektur eignet sich für jedes beliebige R-basierte Prognosemodell, das Sie als Echtzeitdienst bereitstellen möchten. **[Stellen Sie diese Lösung bereit.][github]**

## <a name="architecture"></a>Architecture

![Echtzeitbewertung von R-Machine Learning-Modellen in Azure][0]

In dieser Referenzarchitektur wird ein containerbasierter Ansatz verwendet. Es werden ein Docker-Image, das R enthält, sowie die verschiedenen Artefakte erstellt, die zur Bewertung neuer Daten erforderlich sind. Dazu gehören das Modellobjekt selbst und ein Bewertungsskript. Dieses Image wird an eine in Azure gehostete Docker-Registrierung gepusht und anschließend in einem Kubernetes-Cluster (ebenfalls in Azure) bereitgestellt.

Die Architektur dieses Workflows umfasst folgende Komponenten:

- **[Azure Container Registry:][acr]** Dient zum Speichern der Images für diesen Workflow. Mit Container Registry erstellte Registrierungen können über die standardmäßige [Docker-Registrierungs-API (Version 2)][docker] und den entsprechenden Client verwaltet werden.

- **[Azure Kubernetes Service:][aks]** Dient zum Hosten der Bereitstellung und des Diensts. Mit AKS erstellte Cluster können über die standardmäßige [Kubernetes-API][k-api] und den entsprechenden Client (kubectl) verwaltet werden.

- **[Microsoft Machine Learning Server:][mmls]** Dient zum Definieren der REST-API für den Dienst und enthält die [Modelloperationalisierung][operationalization]. Dieser dienstorientierte Webserverprozess lauscht auf Anforderungen, die dann an andere Hintergrundprozesse übergeben werden, die wiederum den eigentlichen R-Code ausführen, um die Ergebnisse zu generieren. Bei dieser Konfiguration werden alle hier genannten Prozesse auf einem einzelnen Knoten in einem Container ausgeführt. Ausführliche Informationen zur Verwendung dieses Diensts außerhalb einer Entwicklungs- oder Testumgebung erhalten Sie von Ihrem Ansprechpartner bei Microsoft.

## <a name="performance-considerations"></a>Überlegungen zur Leistung

Machine Learning-Workloads sind häufig mit einem hohen Rechenaufwand verbunden. Das gilt sowohl beim Trainieren als auch beim Bewerten neuer Daten. Es wird daher davon abgeraten, mehrere Bewertungsprozesse pro Kern auszuführen. Mit Machine Learning Server können Sie die Anzahl von R-Prozessen definieren, die in den einzelnen Containern ausgeführt werden können. Der Wert ist standardmäßig auf fünf Prozesse festgelegt. Wenn Sie ein relativ einfaches Modell (etwa eine lineare Regression mit nur einigen wenigen Variablen) oder eine einfache Entscheidungsstruktur erstellen, können Sie die Prozessanzahl erhöhen. Überwachen Sie die CPU-Auslastung der Clusterknoten, um einen geeigneten Grenzwert für die Anzahl von Containern zu ermitteln.

Ein GPU-fähiger Cluster kann einige Arten von Workloads beschleunigen. Das gilt insbesondere für Deep Learning-Modelle. Von GPUs profitieren nur Workloads, die intensiven Gebrauch von Matrixalgebra machen. So entstehen etwa bei strukturbasierten Modellen (einschließlich Random Forests und Boosting-Modelle) in der Regel keine Vorteile durch GPUs.

Bestimmte Modelltypen (unter anderem Random Forests) sind auf CPUs hochgradig parallelisierbar. In diesen Fällen können Sie die Bewertung einer einzelnen Anforderung beschleunigen, indem Sie die Workload auf mehrere Kerne verteilen. Dies geht jedoch zulasten Ihrer Verarbeitungskapazität für mehrere Bewertungsanforderungen bei einer festen Clustergröße.

Open-Source-basierte R-Modelle speichern in der Regel alle ihre Daten im Arbeitsspeicher. Stellen Sie daher sicher, dass Sie über genügend Arbeitsspeicher für die Prozesse verfügen, die Sie parallel ausführen möchten. Wenn Sie Ihre Modelle mithilfe von Machine Learning Server anpassen, verwenden Sie die Bibliotheken, die Daten auf dem Datenträger verarbeiten können, anstatt alle Daten in den Arbeitsspeicher zu lesen. Dadurch lässt sich der Arbeitsspeicherbedarf ggf. erheblich senken. Überwachen Sie unabhängig von der Verwendung von Machine Learning Server oder Open-Source-R Ihre Knoten, um sicherzustellen, dass Ihren Bewertungsprozessen nicht der Arbeitsspeicher ausgeht.

## <a name="security-considerations"></a>Sicherheitshinweise

### <a name="network-encryption"></a>Netzwerkverschlüsselung

In dieser Referenzarchitektur werden HTTPS für die Kommunikation mit dem Cluster und ein Bereitstellungszertifikat von [Let's Encrypt][encrypt] verwendet. Verwenden Sie in einer Produktionsumgebung ein eigenes Zertifikat von einer geeigneten Signaturstelle.

### <a name="authentication-and-authorization"></a>Authentifizierung und Autorisierung

Die [Modelloperationalisierung][operationalization] von Machine Learning Server erfordert eine Authentifizierung von Bewertungsanforderungen. In dieser Bereitstellung werden ein Benutzername und ein Kennwort verwendet. In einer Unternehmensumgebung können Sie die Authentifizierung über [Azure Active Directory][AAD] ermöglichen oder unter Verwendung von [Azure API Management][API] ein separates Front-End erstellen.

Damit die Modelloperationalisierung mit Machine Learning Server in Containern ordnungsgemäß funktioniert, muss ein JWT-Zertifikat (JSON Web Token) installiert werden. In dieser Bereitstellung wird ein von Microsoft bereitgestelltes Zertifikat verwendet. In einer Produktionsumgebung muss ein eigenes Zertifikat verwendet werden.

Für den Datenverkehr zwischen Container Registry und AKS empfiehlt sich ggf. die Aktivierung der [rollenbasierten Zugriffssteuerung][rbac] (role-based access control, RBAC), um Zugriffsrechte auf ein Mindestmaß zu beschränken.

### <a name="separate-storage"></a>Trennen des Speichers

Bei dieser Referenzarchitektur werden die Anwendung (R) und die Daten (Modellobjekt und Bewertungsskript) in einem einzelnen Image gebündelt. Manchmal kann es jedoch von Vorteil sein, diese Komponenten zu trennen. Sie können die Modelldaten und den Code in [Azure Storage][storage] (Blob- oder Dateispeicher) platzieren und bei der Containerinitialisierung abrufen. In diesem Fall muss das Speicherkonto so konfiguriert sein, dass nur authentifizierte Zugriffe zulässig sind und HTTPS verwendet werden muss.

## <a name="monitoring-and-logging-considerations"></a>Überlegungen zur Überwachung und Protokollierung

Verwenden Sie zur Überwachung des Gesamtstatus Ihres AKS-Clusters das [Kubernetes-Dashboard][dashboard]. Ausführlichere Informationen finden Sie im Azure-Portal auf dem Übersichtsblatt des Clusters. In den [GitHub][github]-Ressourcen erfahren Sie auch, wie Sie das Dashboard über R aufrufen.

Das Dashboard gibt zwar Aufschluss über die allgemeine Integrität Ihres Clusters, es ist aber auch wichtig, den Status einzelner Container nachzuverfolgen. Aktivieren Sie hierzu im Azure-Portal auf dem Übersichtsblatt des Clusters das Feature [Azure Monitor Insights][monitor], oder lesen Sie [Azure Monitor für Container (Vorschau) – Übersicht][monitor-containers].

## <a name="cost-considerations"></a>Kostenbetrachtung

Die Lizenzierung von Machine Learning Server ist kernbasiert. Dabei werden alle Kerne im Cluster berücksichtigt, auf denen Machine Learning Server ausgeführt wird. Wenden Sie sich als Unternehmenskunde mit Machine Learning Server oder Microsoft SQL Server an Ihren Ansprechpartner bei Microsoft, um Preisinformationen zu erhalten.

Eine Open-Source-Alternative zu Machine Learning Server ist [Plumber][plumber]. Dabei handelt es sich um ein R-Paket, das Ihren Code in eine REST-API verwandelt. Plumber bietet weniger Features als Machine Learning Server. So stehen standardmäßig beispielweise keine Features für die Anforderungsauthentifizierung zur Verfügung. Bei Verwendung von Plumber empfiehlt es sich, [Azure API Management][API] für die Authentifizierung zu aktivieren.

Für die Kosten sind neben der Lizenzierung insbesondere die Computeressourcen des Kubernetes-Clusters relevant. Der Cluster muss groß genug sein, um das zu Spitzenzeiten erwartete Anforderungsvolumen bewältigen zu können. Das führt jedoch dazu, dass sich Ressourcen außerhalb von Spitzenzeiten im Leerlauf befinden. Aktivieren Sie mithilfe des kubectl-Tools die [automatische horizontale Skalierung][autoscaler] für den Cluster, um die Auswirkungen von Ressourcen im Leerlauf zu verringern. Alternativ können Sie auch die [automatische Clusterskalierung][cluster-autoscaler] von AKS verwenden.

## <a name="deploy-the-solution"></a>Bereitstellen der Lösung

Die Referenzimplementierung dieser Architektur ist auf [GitHub][github] verfügbar. Gehen Sie wie dort beschrieben vor, um ein einfaches Prognosemodell als Dienst bereitzustellen.

<!-- links -->
[AAD]: /azure/active-directory/fundamentals/active-directory-whatis
[API]: /azure/api-management/api-management-key-concepts
[ACR]: /azure/container-registry/container-registry-intro
[AKS]: /azure/aks/intro-kubernetes
[autoscaler]: https://kubernetes.io/docs/tasks/run-application/horizontal-pod-autoscale/
[cluster-autoscaler]: /azure/aks/autoscaler
[monitor]: /azure/monitoring/monitoring-container-insights-overview
[dashboard]: /azure/aks/kubernetes-dashboard
[docker]: https://docs.docker.com/registry/spec/api/
[encrypt]: https://letsencrypt.org/
[gitHub]: https://github.com/Azure/RealtimeRDeployment
[K-API]: https://kubernetes.io/docs/reference/
[MMLS]: /machine-learning-server/what-is-machine-learning-server
[monitor-containers]: /azure/azure-monitor/insights/container-insights-overview
[operationalization]: /machine-learning-server/what-is-operationalization
[plumber]: https://www.rplumber.io
[RBAC]: /azure/role-based-access-control/overview
[storage]: /azure/storage/common/storage-introduction
[0]: ./_images/realtime-scoring-r.png
