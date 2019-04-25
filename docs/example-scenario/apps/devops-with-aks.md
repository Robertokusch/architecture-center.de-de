---
title: CI/CD-Pipeline für containerbasierte Workloads
titleSuffix: Azure Example Scenarios
description: Erstellen Sie eine DevOps-Pipeline für eine Node.js-Web-App mit Jenkins, Azure Container Registry, Azure Kubernetes Service, Cosmos DB und Grafana.
author: iainfoulds
ms.date: 07/05/2018
ms.topic: example-scenario
ms.service: architecture-center
ms.subservice: example-scenario
social_image_url: /azure/architecture/example-scenario/apps/media/architecture-devops-with-aks.png
ms.openlocfilehash: 9be4f828c96c4ac321acf9d9719d0ef465fb35cf
ms.sourcegitcommit: 579c39ff4b776704ead17a006bf24cd4cdc65edd
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/17/2019
ms.locfileid: "59641177"
---
# <a name="cicd-pipeline-for-container-based-workloads"></a>CI/CD-Pipeline für containerbasierte Workloads

Dieses Beispielszenario richtet sich an Unternehmen, die ihre Anwendungsentwicklung durch die Verwendung von Containern und DevOps-Workflows modernisieren möchten. In diesem Szenario wird eine Node.js-Web-App erstellt und von Jenkins in einer Azure Container Registry-Instanz und in Azure Kubernetes Service bereitgestellt. Für eine global verteilte Datenbankschicht wird Azure Cosmos DB verwendet. Zur Überwachung der Anwendungsleistung sowie zur Behandlung entsprechender Probleme wird Azure Monitor mit einer Grafana-Instanz und einem entsprechenden Dashboard verknüpft.

Beispielszenarien für diese Anwendung wären etwa die Bereitstellung einer automatisierten Entwicklungsumgebung, die Überprüfung neuer Codecommits oder das Pushen neuer Bereitstellungen an Staging- oder Produktionsumgebungen. In der Vergangenheit mussten Unternehmen Anwendungen und Updates in der Regel manuell erstellen und kompilieren sowie eine umfangreiche monolithische Codebasis pflegen. Mit einem modernen Konzept für die Anwendungsentwicklung, das auf Continuous Integration (CI) und Continuous Deployment (CD) setzt, können Sie Dienste schneller erstellen, testen und bereitstellen. Durch diesen modernen Ansatz können Sie Anwendungen und Updates schneller für Ihre Kunden bereitstellen und flexibler auf veränderte geschäftliche Anforderungen reagieren.

Azure-Dienste wie Azure Kubernetes Service, Container Registry und Cosmos DB bieten Unternehmen die neuesten Techniken und Tools für die Anwendungsentwicklung und vereinfachen die Implementierung von Hochverfügbarkeit.

## <a name="relevant-use-cases"></a>Relevante Anwendungsfälle

Zu den weiteren relevanten Anwendungsfällen zählen:

- Modernisieren der Anwendungsentwicklung mit einem Microservice- und Container-basierten Ansatz
- Beschleunigen von Anwendungsentwicklung und Bereitstellungslebenszyklen
- Automatisieren von Bereitstellungen in Test- oder Akzeptanzumgebungen zu Überprüfungszwecken

## <a name="architecture"></a>Architecture

![Übersicht über die Architektur der Azure-Komponenten für ein DevOps-Szenario mit Jenkins, Azure Container Registry und Azure Kubernetes Service][architecture]

Dieses Szenario umfasst eine DevOps-Pipeline für eine Node.js-Webanwendung und ein Datenbank-Back-End. Die Daten durchlaufen das Szenario wie folgt:

1. Ein Entwickler ändert den Quellcode der Node.js-Webanwendung.
2. Die Codeänderung wird in einem Quellcodeverwaltungsrepository wie GitHub committet.
3. Zum Starten des CI-Prozesses (Continuous Integration) löst ein GitHub-Webhook einen Jenkins-Projektbuildvorgang aus.
4. Der Jenkins-Buildauftrag verwendet einen dynamischen Build-Agent in Azure Kubernetes Service, um einen Containerbuildprozess auszuführen.
5. Auf der Grundlage des Codes in der Quellcodeverwaltung wird ein Containerimage erstellt und anschließend an Azure Container Registry gepusht.
6. Über Continuous Deployment (CD) stellt Jenkins dieses aktualisierte Containerimage im Kubernetes-Cluster bereit.
7. Die Node.js-Webanwendung verwendet Cosmos DB als Back-End. Sowohl Cosmos DB als auch Azure Kubernetes Service übermitteln Metriken an Azure Monitor.
8. Eine Grafana-Instanz bietet visuelle Dashboards zur Anwendungsleistung, die auf den Daten von Azure Monitor basieren.

### <a name="components"></a>Komponenten

- [Jenkins][jenkins] ist ein Open-Source-Automatisierungsserver, der sich problemlos in Azure-Dienste integrieren lässt, um Continuous Integration (CI) und Continuous Deployment (CD) zu ermöglichen. In diesem Szenario orchestriert Jenkins die Erstellung neuer Containerimages auf der Grundlage von Commits in der Quellcodeverwaltung, pusht die Images an die Azure Container Registry-Instanz und aktualisiert anschließend Anwendungsinstanzen in Azure Kubernetes Service.
- [Virtuelle Linux-Computer in Azure][docs-virtual-machines] fungieren als IaaS-Plattform zum Ausführen der Jenkins- und Grafana-Instanzen.
- [Azure Container Registry][docs-acr] speichert und verwaltet Containerimages, die vom Azure Kubernetes Service-Cluster verwendet werden. Images werden sicher gespeichert und können von der Azure-Plattform in anderen Regionen repliziert werden, um die Bereitstellung zu beschleunigen.
- [Azure Kubernetes Service][docs-aks] ist eine verwaltete Kubernetes-Plattform, mit der Sie Containeranwendungen ganz ohne Kenntnisse auf dem Gebiet der Containerorchestrierung bereitstellen und verwalten können. Azure führt als gehosteter Kubernetes-Dienst wichtige Aufgaben für Sie aus, z.B. Systemüberwachung und Wartung.
- [Azure Cosmos DB][docs-cosmos-db] ist eine global verteilte Datenbank, bei der Sie zwischen verschiedenen Datenbank- und Konsistenzmodellen wählen können, um Ihre individuellen Anforderungen zu erfüllen. Mit Cosmos DB können Ihre Daten global repliziert werden, und es müssen keine Komponenten für die Clusterverwaltung oder Replikation bereitgestellt und konfiguriert werden.
- [Azure Monitor][docs-azure-monitor] dient zum Nachverfolgen der Leistung, Gewährleisten der Sicherheit und Identifizieren von Trends. Metriken von Monitor können von anderen Ressourcen und Tools (etwa von Grafana) verwendet werden.
- [Grafana][grafana] ist eine Open-Source-Lösung für Abfragen, Visualisierungen und Warnungen sowie zum Nachvollziehen von Metriken. Dank eines Datenquellen-Plug-Ins für Azure Monitor kann Grafana visuelle Dashboards zur Überwachung der Leistung Ihrer Anwendungen erstellen, die in Azure Kubernetes Service ausgeführt werden und Cosmos DB verwenden.

### <a name="alternatives"></a>Alternativen

- [Azure Pipelines][azure-pipelines] unterstützen Sie bei der Implementierung einer Pipeline für Continuous Integration (CI), Tests und Continuous Deployment (CD) für jede beliebige App.
- [Kubernetes][kubernetes] kann direkt auf virtuellen Azure-Computern ausgeführt werden und bietet so bei Bedarf ein höheres Maß an Kontrolle als bei der Ausführung über einen verwalteten Dienst.
- [Service Fabric][service-fabric] ist eine weitere Alternative für einen Containerorchestrator, die AKS ersetzen kann.

## <a name="considerations"></a>Überlegungen

### <a name="availability"></a>Verfügbarkeit

Bei diesem Szenario wird Azure Monitor mit Grafana für visuelle Dashboards kombiniert, um die Leistung Ihrer Anwendung zu überwachen und Probleme zu melden. Mithilfe dieser Tools können Sie die Leistung überwachen und entsprechende Probleme behandeln, für die ggf. Codeaktualisierungen erforderlich sind, die dann über die CI/CD-Pipeline bereitgestellt werden können.

Der Azure Kubernetes Service-Cluster verfügt über einen Lastenausgleich, der Anwendungsdatenverkehr an Container (Pods) verteilt, die Ihre Anwendung ausführen. Dieses Konzept der Ausführung von Containeranwendungen in Kubernetes stellt eine hochverfügbare Infrastruktur für Ihre Kunden bereit.

### <a name="scalability"></a>Skalierbarkeit

Azure Kubernetes Service ermöglicht die Skalierung der Anzahl von Clusterknoten, um den Anforderungen Ihrer Anwendungen gerecht zu werden. Mit zunehmender Größe Ihrer Anwendung können Sie die Anzahl von Kubernetes-Knoten für die Ausführung Ihres Diensts horizontal hochskalieren.

Anwendungsdaten werden in Azure Cosmos DB gespeichert. Dabei handelt es sich um eine global verteilte und global skalierbare Datenbank mit mehreren Modellen. Cosmos DB abstrahiert die Notwendigkeit der Infrastrukturskalierung, wie dies bei herkömmlichen Datenbankkomponenten der Fall ist. Sie haben die Möglichkeit, Ihre Cosmos DB-Instanz global zu replizieren, um den Anforderungen Ihrer Kunden gerecht zu werden.

Weitere Skalierbarkeitsthemen finden Sie im Azure Architecture Center in der [Checkliste für die Skalierbarkeit][scalability].

### <a name="security"></a>Sicherheit

Zur Minimierung der Angriffsfläche wird die Jenkins-VM-Instanz in diesem Szenario nicht über HTTP verfügbar gemacht. Für Verwaltungsaufgaben, die eine Interaktion mit Jenkins erfordern, wird von Ihrem lokalen Computer aus über einen SSH-Tunnel eine sichere Remoteverbindung hergestellt. Für die VM-Instanzen von Jenkins und Grafana ist ausschließlich die Authentifizierung mit öffentlichem SSH-Schlüssel zulässig. Kennwortbasierte Anmeldungen sind deaktiviert. Weitere Informationen finden Sie unter [Ausführen eines Jenkins-Servers in Azure](./jenkins.md).

Zur Trennung von Anmeldeinformationen und Berechtigungen verwendet dieses Szenario einen dedizierten Azure Active Directory-Dienstprinzipal. Die Anmeldeinformationen für diesen Dienstprinzipal werden als Objekt mit sicheren Anmeldeinformationen in Jenkins gespeichert, damit sie nicht direkt in Skripts oder in der Buildpipeline verfügbar/sichtbar sind.

Allgemeine Informationen zur Entwicklung sicherer Lösungen finden Sie in der [Dokumentation zur Azure-Sicherheit][security].

### <a name="resiliency"></a>Resilienz

In diesem Szenario wird Azure Kubernetes Service für Ihre Anwendung genutzt. Kubernetes verfügt über integrierte Resilienzkomponenten, die die Container (Pods) überwachen und im Falle eines Problems neu starten. In Kombination mit der Ausführung mehrerer Kubernetes-Knoten kann Ihre Anwendung die Nichtverfügbarkeit eines Pods oder Knotens tolerieren.

Allgemeine Informationen zur Entwicklung robuster Lösungen finden Sie unter [Entwerfen zuverlässiger Azure-Anwendungen](../../reliability/index.md).

## <a name="deploy-the-scenario"></a>Bereitstellen des Szenarios

### <a name="prerequisites"></a>Voraussetzungen

- Sie benötigen ein bestehendes Azure-Konto. Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) erstellen, bevor Sie beginnen.

- Sie benötigen ein öffentliches SSH-Schlüsselpaar. Informationen zum Erstellen eines öffentlichen Schlüsselpaars finden Sie unter [Schnelle Schritte: Erstellen und Verwenden eines SSH-Schlüsselpaars (öffentlich und privat) für virtuelle Linux-Computer in Azure][sshkeydocs].

- Für die Authentifizierung des Diensts und der Ressourcen benötigen Sie einen Azure Active Directory-Dienstprinzipal. Bei Bedarf können Sie mit [az ad sp create-for-rbac][createsp] einen Dienstprinzipal erstellen.

    ```azurecli-interactive
    az ad sp create-for-rbac --name myDevOpsScenario
    ```

    Notieren Sie sich die Werte für *appId* und *password* aus der Befehlsausgabe. Diese Werte müssen beim Bereitstellen des Szenarios in der Vorlage angegeben werden.

### <a name="walk-through"></a>Exemplarische Vorgehensweise

Gehen Sie wie folgt vor, um dieses Szenario mit einer Azure Resource Manager-Vorlage bereitzustellen.

<!-- markdownlint-disable MD033 -->

1. Klicken Sie auf die Schaltfläche **Deploy to Azure** (In Azure bereitstellen):<br><a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2Fmspnp%2Fsolution-architectures%2Fmaster%2Fapps%2Fdevops-with-aks%2Fazuredeploy.json" target="_blank"><img src="https://azuredeploy.net/deploybutton.png"/></a>
2. Warten Sie, bis die Vorlagenbereitstellung im Azure-Portal geöffnet wurde, und führen Sie anschließend folgende Schritte aus:
   - Erstellen Sie über **Neu erstellen** eine neue Ressourcengruppe, und geben Sie einen Namen (beispielsweise *myAKSDevOpsScenario*) in das Textfeld ein.
   - Wählen Sie im Dropdownfeld **Standort** eine Region aus.
   - Geben Sie die App-ID und das Kennwort Ihres Dienstprinzipals aus dem Befehl `az ad sp create-for-rbac` ein.
   - Geben Sie einen Benutzernamen und ein sicheres Kennwort für die Jenkins-Instanz und die Grafana-Konsole an.
   - Geben Sie einen SSH-Schlüssel an, um Anmeldungen bei den virtuellen Linux-Computern zu schützen.
   - Lesen Sie die allgemeinen Geschäftsbedingungen, und aktivieren Sie dann das Kontrollkästchen **Ich stimme den oben genannten Geschäftsbedingungen zu**.
   - Klicken Sie auf die Schaltfläche **Kaufen**.

<!-- markdownlint-enable MD033 -->

Der Bereitstellungsvorgang kann zwischen 15 und 20 Minuten dauern.

## <a name="pricing"></a>Preise

Zur Ermittlung der Betriebskosten für dieses Szenario sind alle Dienste im Kostenrechner vorkonfiguriert. Wenn Sie wissen möchten, welche Kosten für Ihren spezifischen Anwendungsfall entstehen, passen Sie die entsprechenden Variablen an Ihren voraussichtlichen Datenverkehr an.

Auf der Grundlage der Anzahl der zu speichernden Containerimages und der Kubernetes-Knoten für die Ausführung Ihrer Anwendungen haben wir drei exemplarische Kostenprofile erstellt:

- [Klein:][small-pricing] Dieses Preisbeispiel entspricht 1.000 Containerbuildvorgängen pro Monat.
- [Mittel:][medium-pricing] Dieses Preisbeispiel entspricht 100.000 Containerbuildvorgängen pro Monat.
- [Groß:][large-pricing] Dieses Preisbeispiel entspricht 1.000.000 Containerbuildvorgängen pro Monat.

## <a name="related-resources"></a>Zugehörige Ressourcen

Dieses Szenario verwendet Azure Container Registry und Azure Kubernetes Service, um eine containerbasierte Anwendung zu speichern und auszuführen. Azure Container Instances kann ebenfalls zum Ausführen containerbasierter Anwendungen verwendet werden, ohne dafür Orchestrierungskomponenten bereitstellen zu müssen. Weitere Informationen finden Sie in der [Übersicht über Azure Container Instances][docs-aci].

<!-- links -->
[architecture]: ./media/architecture-devops-with-aks.png
[autoscaling]: ../../best-practices/auto-scaling.md
[docs-aci]: /azure/container-instances/container-instances-overview
[docs-acr]: /azure/container-registry/container-registry-intro
[docs-aks]: /azure/aks/intro-kubernetes
[docs-azure-monitor]: /azure/monitoring-and-diagnostics/monitoring-overview
[docs-cosmos-db]: /azure/cosmos-db/introduction
[docs-virtual-machines]: /azure/virtual-machines/linux/overview
[createsp]: /cli/azure/ad/sp#az-ad-sp-create
[grafana]: https://grafana.com/
[jenkins]: https://jenkins.io/
[resource-groups]: /azure/azure-resource-manager/resource-group-overview
[security]: /azure/security/
[scalability]: ../../checklist/scalability.md
[sshkeydocs]: /azure/virtual-machines/linux/mac-create-ssh-keys
[azure-pipelines]: /azure/devops/pipelines
[kubernetes]: https://kubernetes.io/
[service-fabric]: /azure/service-fabric/

[small-pricing]: https://azure.com/e/841f0a75b1ea4802ba1ac8f7918a71e7
[medium-pricing]: https://azure.com/e/eea0e6d79b4e45618a96d33383ec77ba
[large-pricing]: https://azure.com/e/3faab662c54c473da55a1e93a27e0e64