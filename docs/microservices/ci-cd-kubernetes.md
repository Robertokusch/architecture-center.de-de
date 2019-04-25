---
title: Erstellen einer CI/CD-Pipeline für Microservices in Kubernetes
description: Beschreibt eine CI/CD-Beispielpipeline für das Bereitstellen von Microservices in Azure Kubernetes Service (AKS).
author: MikeWasson
ms.date: 04/11/2019
ms.topic: guide
ms.service: architecture-center
ms.subservice: reference-architecture
ms.custom: microservices
ms.openlocfilehash: e7da8a1b4111fb7856b919b40d033833a4e475e0
ms.sourcegitcommit: d58e6b2b891c9c99e951c59f15fce71addcb96b1
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/12/2019
ms.locfileid: "59533137"
---
<!-- markdownlint-disable MD040 -->

# <a name="building-a-cicd-pipeline-for-microservices-on-kubernetes"></a>Erstellen einer CI/CD-Pipeline für Microservices in Kubernetes

Das Erstellen eines zuverlässigen CI/CD-Prozesses für eine Microservicearchitektur kann schwierig sein. Einzelne Teams müssen in der Lage sein, Dienste schnell und zuverlässig zu veröffentlichen, ohne andere Teams zu stören oder die Stabilität der gesamten Anwendung zu gefährden.

Dieser Artikel beschreibt eine CI/CD-Beispielpipeline für das Bereitstellen von Microservices in Azure Kubernetes Service (AKS). Jedes Team und jedes Projekt ist anders, daher sollten Sie diesen Artikel nicht als feststehende Sammlung unverrückbarer Regeln verstehen. Er soll vielmehr einen Ausgangspunkt für Ihren Entwurf Ihres eigenen CI/CD-Prozesses bilden.

Die hier beschriebene Beispielpipeline wurde für eine Microservice-Referenzimplementierung erstellt, die sich Drohnenlieferungsanwendung nennt und auf [GitHub][ri] verfügbar ist. Das Anwendungsszenario ist [hier](./design/index.md#reference-implementation) beschrieben.

Die Ziele der Pipeline können wie folgt zusammengefasst werden:

- Teams können ihre Dienste unabhängig voneinander entwickeln und bereitstellen.
- Codeänderungen, die den CI-Prozess durchlaufen, werden automatisch in einer produktionsähnlichen Umgebung bereitgestellt.
- In jeder Phase der Pipeline werden Quality Gates durchgesetzt.
- Eine neue Version eines Diensts kann neben der Vorgängerversion bereitgestellt werden.

Hintergrundinformationen dazu finden Sie unter [CI/CD für Microservicearchitekturen](./ci-cd.md).

## <a name="assumptions"></a>Annahmen

Für die Zwecke dieses Beispiels machen wir einige Annahmen über das Entwicklungsteam und die Codebasis:

- Für das Coderepository wird der Monorepo-Ansatz genutzt, und die Ordner sind nach Microservice organisiert.
- Die Branchstrategie des Teams basiert auf der [Trunk-basierten Entwicklung](https://trunkbaseddevelopment.com/).
- Das Team verwendet [Releasebranches](/azure/devops/repos/git/git-branching-guidance?view=azure-devops#manage-releases) zur Verwaltung von Releases. Für jeden Microservice werden separate Releases erstellt.
- Der CI/CD-Prozess verwendet [Azure Pipelines](/azure/devops/pipelines/?view=azure-devops) zum Erstellen, Testen und Bereitstellen der Microservices in AKS.
- Die Containerimages für die einzelnen Microservices sind in der [Azure-Containerregistrierung](/azure/container-registry/) gespeichert.
- Das Team verwendet Helm-Charts, um die Microservices zu verpacken.

Diese Annahmen bestimmen viele der spezifischen Details der CI/CD-Pipeline. Der hier beschriebene Grundansatz lässt sich jedoch für andere Prozesse, Tools und Dienste anpassen, wie etwa Jenkins oder Docker Hub.

## <a name="validation-builds"></a>Validierungsbuilds

Nehmen wir an, ein Entwickler arbeitet an einem Microservice mit dem Namen „Delivery Service“ (Lieferdienst). Beim Entwickeln eines neuen Features checkt der Entwickler Code in einen Featurebranch ein. Für Featurebranches wird die Benennungskonvention `feature/*` verwendet.

![CI/CD-Workflow](./images/aks-cicd-1.png)

Die Builddefinitionsdatei enthält einen Trigger, der nach dem Branchnamen und dem Quellpfad filtert:

```yaml
trigger:
  batch: true
  branches:
    include:
    - master
    - feature/*
    - topic/*

    exclude:
    - feature/experimental/*
    - topic/experimental/*

  paths:
     include:
     - /src/shipping/delivery/
```

&#11162; Sehen Sie dazu die [Quelldatei](https://github.com/mspnp/microservices-reference-implementation/blob/master/src/shipping/delivery/azure-pipelines-validation.yml).

Mit diesem Ansatz kann jedes Team über eine eigene Buildpipeline verfügen. Nur Code, der im Ordner `/src/shipping/delivery` eingecheckt wird, löst einen Build des Lieferdiensts aus. Das Pushen von Commits auf einen Branch, der mit dem Filter übereinstimmt, löst einen CI-Build aus. An diesem Punkt des Workflows führt der CI-Build eine Codeüberprüfung mit minimalem Umfang durch:

1. Erstellen des Codes.
1. Ausführen von Komponententests.

Das Ziel besteht darin, die Buildzeiten kurz zu halten, damit der Entwickler schnell Feedback erhält. Wenn das Feature für die Zusammenführung mit dem Master bereit ist, öffnet der Entwickler einen PR-Vorgang. Hierdurch wird ein weiterer CI-Build ausgelöst, bei dem einige zusätzliche Überprüfungen durchgeführt werden:

1. Erstellen des Codes.
1. Ausführen von Komponententests.
1. Erstellen des Runtime-Containerimages.
1. Durchführen von Überprüfungen auf Sicherheitsrisiken für das Image.

![CI/CD-Workflow](./images/aks-cicd-2.png)

> [!NOTE]
> In Azure DevOps-Repos können Sie [Richtlinien](/azure/devops/repos/git/branch-policies) zum Schützen von Branches definieren. Für die Richtlinie sind beispielsweise ggf. ein erfolgreicher CI-Build sowie eine Genehmigung eines Prüfers erforderlich, damit die Zusammenführung mit dem Master erfolgen kann.

## <a name="full-cicd-build"></a>Vollständiger CI/CD-Build

An einem bestimmten Punkt ist das Team zum Bereitstellen einer neuen Version des Lieferdiensts bereit. Der Release Manager erstellt einen Branch vom Master und verwendet dazu das folgende Benennungsmuster: `release/<microservice name>/<semver>`. Beispiel: `release/delivery/v1.0.2`.

![CI/CD-Workflow](./images/aks-cicd-3.png)

Die Erstellung dieses Branches löst einen vollständigen CI-Build aus, für den alle der vorstehenden Schritte ausgeführt werden, plus:

1. Pushübertragung des Containerimages in die Azure-Containerregistrierung Das Image wird mit der Versionsnummer aus dem Branchnamen versehen.
2. Ausführen von `helm package` zum Verpacken des Helm-Charts für den Dienst Das Chart ist außerdem mit einer Versionsnummer gekennzeichnet.
3. Übertragen des Helm-Pakets an die Containerregistrierung per Push.

Wenn dieser Buildvorgang erfolgreich ist, wird mithilfe einer [Releasepipeline](/azure/devops/pipelines/release/what-is-release-management) von Azure Pipelines ein Bereitstellungsprozess (CD) ausgelöst. Diese Pipeline weist folgende Stufen auf:

1. Bereitstellen des Helm-Charts in einer QA-Umgebung.
1. Eine genehmigende Person meldet sich ab, bevor das Paket für die Produktion bereitgestellt wird. Weitere Informationen finden Sie unter [Release deployment control using approvals](/azure/devops/pipelines/release/approvals/approvals) (Steuerung von Releasebereitstellungen durch Genehmigungen).
1. Versehen Sie das Docker-Image für den Produktionsnamespace in Azure Container Registry mit einem neuen Tag. Wenn das derzeitige Tag beispielsweise `myrepo.azurecr.io/delivery:v1.0.2` lautet, lautet das Tag für die Produktion `myrepo.azurecr.io/prod/delivery:v1.0.2`.
1. Stellen Sie das Helm-Chart in der Produktionsumgebung bereit.

Auch beim Monorepo-Ansatz können diese Aufgaben einzelnen Microservices zugeordnet werden, damit die Teams Bereitstellungen schneller durchführen können. Der Prozess weist einige manuelle Schritte auf: Genehmigen von PRs, Erstellen von Releasebranches und Genehmigen von Bereitstellungen im Produktionscluster. Der Richtlinie nach sind dies manuelle Schritte – aber sie können auch automatisiert werden, falls die Organisation dies vorzieht.

## <a name="isolation-of-environments"></a>Isolation von Umgebungen

Sie verfügen über mehrere Umgebungen, in denen Sie Dienste bereitstellen, z.B. Entwicklung, Feuerprobe, Integrationstests, Auslastungstests und schließlich Produktion. Für diese Umgebungen ist ein gewisses Maß an Isolation erforderlich. In Kubernetes haben Sie die Wahl zwischen physischer Isolation und logischer Isolation. Physische Isolation bedeutet, dass die Bereitstellung in separaten Clustern erfolgt. Bei der logischen Isolation werden wie oben beschrieben Namespaces und Richtlinien verwendet.

Unsere Empfehlung lautet, einen dedizierten Produktionscluster mit einem zusätzlichen separaten Cluster für Ihre Entwicklungs- und Testumgebungen zu erstellen. Verwenden Sie die logische Isolation, um Umgebungen im Entwicklungs-/Testcluster zu trennen. Im Entwicklungs-/Testcluster bereitgestellte Dienste sollten niemals Zugriff auf Datenspeicher haben, in denen sich Geschäftsdaten befinden.

## <a name="build-process"></a>Buildprozess

Verpacken Sie nach Möglichkeit Ihren Buildprozess in einem Docker-Container. Auf diese Weise können Sie Ihre Codeartefakte mithilfe von Docker erstellen, ohne die Buildumgebung auf jedem Buildcomputer konfigurieren zu müssen. Ein Container-Buildprozess erleichtert das horizontale Hochskalieren der CI-Pipeline durch Hinzufügen neuer Build-Agents. Außerdem kann jeder Entwickler im Team den Code durch einfaches Ausführen des Buildcontainers erstellen.

Durch die Verwendung mehrstufiger Builds in Docker können Sie die Buildumgebung und das Laufzeitimage in einem einzelnen Dockerfile definieren. Dies ist beispielsweise ein Dockerfile, das eine ASP.NET Core-Anwendung erstellt:

```
FROM microsoft/dotnet:2.2-runtime AS base
WORKDIR /app

FROM microsoft/dotnet:2.2-sdk AS build
WORKDIR /src/Fabrikam.Workflow.Service

COPY Fabrikam.Workflow.Service/Fabrikam.Workflow.Service.csproj .
RUN dotnet restore Fabrikam.Workflow.Service.csproj

COPY Fabrikam.Workflow.Service/. .
RUN dotnet build Fabrikam.Workflow.Service.csproj -c Release -o /app

FROM build AS publish
RUN dotnet publish Fabrikam.Workflow.Service.csproj -c Release -o /app

FROM base AS final
WORKDIR /app
COPY --from=publish /app .
ENTRYPOINT ["dotnet", "Fabrikam.Workflow.Service.dll"]
```

&#11162; Sehen Sie dazu die [Quelldatei](https://github.com/mspnp/microservices-reference-implementation/blob/master/src/shipping/workflow/Dockerfile).

Dieses Dockerfile definiert mehrere Buildphasen. Beachten Sie, dass die Phase mit dem Namen `base` die ASP.NET Core-Runtime verwendet, während die Phase mit dem Namen `build` das vollständige ASP.NET Core SDK verwendet. Die `build` Phase wird verwendet, um das ASP.NET Core-Projekt zu erstellen. Der endgültige Laufzeitcontainer wird jedoch aus `base` erstellt, das nur die Runtime enthält und erheblich kleiner als das vollständige SDK-Image ist.

### <a name="building-a-test-runner"></a>Erstellen eines Test Runners

Eine weitere bewährte Methode ist das Ausführen von Komponententests im Container. Dies ist beispielsweise ein Teil einer Docker-Datei, die einen Test Runner erstellt:

```
FROM build AS testrunner
WORKDIR /src/tests

COPY Fabrikam.Workflow.Service.Tests/*.csproj .
RUN dotnet restore Fabrikam.Workflow.Service.Tests.csproj

COPY Fabrikam.Workflow.Service.Tests/. .
ENTRYPOINT ["dotnet", "test", "--logger:trx"]
```

Ein Entwickler kann diese Docker-Datei zur lokalen Ausführung der Tests verwenden:

```bash
docker build . -t delivery-test:1 --target=testrunner
docker run -p 8080:8080 delivery-test:1
```

Die CI-Pipeline sollte die Tests ebenfalls im Rahmen des Buildüberprüfungsschritts ausführen.

Beachten Sie, dass in dieser Datei der Docker-Befehl `ENTRYPOINT` verwendet wird, um die Tests auszuführen, nicht der Docker-Befehl `RUN`.

- Wenn Sie den Befehl `RUN` verwenden, werden die Tests jedes Mal ausgeführt, wenn Sie das Image erstellen. Durch Verwendung von `ENTRYPOINT` sind die Tests optional aktivierbar. Sie werden nur ausgeführt, wenn Sie die Phase `testrunner` explizit ansteuern.
- Ein Fehler im Test führt nicht zu einem Fehler beim Docker-Befehl `build`. Auf diese Weise können Sie Buildfehler des Containers von Testfehlern unterscheiden.
- Testergebnisse können auf einem eingebundenen Volume gespeichert werden.

### <a name="container-best-practices"></a>Bewährte Methoden für Container

Hier sind einige weitere bewährte Methoden, die beim Arbeiten mit Containern berücksichtigt werden sollten:

- Definieren Sie organisationsweite Konventionen für Containertags und für die Versionsverwaltung sowie Namenskonventionen für im Cluster bereitgestellte Ressourcen (Pods, Dienste und Ähnliches). Dies kann die Diagnose von Bereitstellungsproblemen vereinfachen.

- Im Rahmen des Entwicklungs- und Testzyklus erstellt der CI/CD-Prozess zahlreiche Containerimages. Nicht alle dieser Images kommen für die Veröffentlichung in Frage, und nur einige der so genannten Release Candidates werden in die Produktionsumgebung heraufgestuft. Entwickeln Sie eine klare Versionsverwaltungsstrategie, damit Sie wissen, welche Images derzeit in der Produktionsumgebung bereitgestellt sind, und bei Bedarf einen Rollback auf eine vorherige Version ausführen können.

- Stellen Sie immer spezifische Containerversionstags bereit, nicht einfach `latest`.

- Verwenden Sie [Namespaces](/azure/container-registry/container-registry-best-practices#repository-namespaces) in der Azure-Containerregistrierung, um Images, die bereits für die Produktion genehmigt wurden, von den noch in der Testphase befindlichen Images zu isolieren. Verschieben Sie ein Image nur dann in den Produktionsnamespace, wenn Sie bereit sind, es für die Produktion bereitzustellen. Wenn Sie diese Vorgehensweise mit der semantischen Versionsverwaltung von Containerimages kombinieren, verringert sich die Wahrscheinlichkeit, dass Sie versehentlich eine Version bereitstellen, die nicht für die Veröffentlichung freigegeben wurde.

- Befolgen Sie das Prinzip der geringsten Rechte, indem Sie Container als Benutzer ohne besondere Rechte ausführen. In Kubernetes können Sie eine Pod-Sicherheitsrichtlinie erstellen, die die Ausführung von Containern als *root* verhindert. Weitere Informationen finden Sie unter [Prevent Pods From Running With Root Privileges](https://docs.bitnami.com/kubernetes/how-to/secure-kubernetes-cluster-psp/) (Verhindern der Ausführung von Pods mit Stammberechtigungen).

## <a name="helm-charts"></a>Helm-Diagramme

Erwägen Sie die Verwendung von Helm zum Erstellen und Bereitstellen von Diensten. Dies sind einige der Features von Helm, die für CI/CD nützlich sind:

- Häufig wird ein einzelner Microservice durch mehrere Kubernetes-Objekte definiert. Helm ermöglicht das Verpacken dieser Objekte in einem einzelnen Helm-Chart.
- Ein Chart kann mit einem einzelnen Helm-Befehl statt einer Reihe von kubectl-Befehlen bereitgestellt werden.
- Charts enthalten eine explizite Versionsangabe. Verwenden Sie Helm, um eine Version zu veröffentlichen, Releases anzuzeigen und das Rollback zu einer früheren Version auszuführen. Nachverfolgen von Updates und Revisionen per semantischer Versionierung und möglicher Rollback auf eine vorherige Version
- Helm-Charts verwenden Vorlagen zur Vermeidung von doppelten Informationen, z.B. Bezeichnungen und Selektoren, über viele Dateien hinweg.
- Helm kann Abhängigkeiten zwischen Charts verwalten.
- Charts können in einem Helm-Repository gespeichert werden, beispielsweise in der Azure-Containerregistrierung, und in die Buildpipeline integriert werden.

Weitere Informationen zur Verwendung von Container Registry als Helm-Repository finden Sie unter [Verwenden Sie Azure Container Registry als Helm-Repository für Ihre Anwendungsdiagramme](/azure/container-registry/container-registry-helm-repos).

Bei einem einzelnen Microservice können mehrere Kubernetes-Konfigurationsdateien im Spiel sein. Das Aktualisieren eines Diensts kann bedeuten, dass alle diese Dateien bearbeitet werden müssen, um Selektoren, Bezeichnungen und Imagetags zu aktualisieren. Helm behandelt diese als einzelnes Paket, das Chart genannt wird und es Ihnen ermöglicht, die YAML-Dateien mithilfe von Variablen komfortabel zu aktualisieren. Helm verwendet eine Vorlagensprache (die auf Go-Vorlagen beruht), um Ihnen das Schreiben von parametrisierten YAML-Konfigurationsdateien zu ermöglichen.

Dies ist beispielsweise ein Teil einer YAML-Datei, die eine Bereitstellung definiert:

```yaml
apiVersion: apps/v1beta2
kind: Deployment
metadata:
  name: {{ include "package.fullname" . | replace "." "" }}
  labels:
    app.kubernetes.io/name: {{ include "package.name" . }}
    app.kubernetes.io/instance: {{ .Release.Name }}
  annotations:
    kubernetes.io/change-cause: {{ .Values.reason }}

...

  spec:
      containers:
      - name: &package-container_name fabrikam-package
        image: {{ .Values.dockerregistry }}/{{ .Values.image.repository }}:{{ .Values.image.tag }}
        imagePullPolicy: {{ .Values.image.pullPolicy }}
        env:
        - name: LOG_LEVEL
          value: {{ .Values.log.level }}
```

&#11162; Sehen Sie dazu die [Quelldatei](https://github.com/mspnp/microservices-reference-implementation/blob/master/charts/package/templates/package-deploy.yaml).

Sie können sehen, dass für den Bereitstellungsnamen, die Bezeichnungen und die Containerspezifikation sämtlich Vorlagenparameter verwendet werden, die zum Zeitpunkt der Bereitstellung zur Verfügung gestellt werden. Beispielsweise an der Befehlszeile:

```bash
helm install $HELM_CHARTS/package/ \
     --set image.tag=0.1.0 \
     --set image.repository=package \
     --set dockerregistry=$ACR_SERVER \
     --namespace backend \
     --name package-v0.1.0
```

Zwar könnte Ihre CI/CD-Pipeline ein Chart direkt in Kubernetes installieren, wir empfehlen jedoch, ein Chartarchiv (TGZ-Datei) zu erstellen und das Chart per Push in ein Helm-Repository zu übertragen, wie etwa die Azure-Containerregistrierung. Weitere Informationen finden Sie unter [Package Docker-based apps in Helm charts in Azure Pipelines](/azure/devops/pipelines/languages/helm?view=azure-devops) (Verpacken von Docker-basierten Apps in Helm-Charts in Azure Pipelines).

Erwägen Sie, Helm in seinem eigenen Namespace bereitzustellen und RBAC (rollenbasierte Zugriffssteuerung) zu verwenden, um die Namespaces für die Bereitstellung einzuschränken. Weitere Informationen finden Sie in der Helm-Dokumentation zur [rollenbasierten Zugriffssteuerung](https://helm.sh/docs/using_helm/#helm-and-role-based-access-control).

### <a name="revisions"></a>Revisionen

Helm-Charts weisen immer eine Versionsnummer auf, die [semantische Versionierung](https://semver.org/) befolgen muss. Ein Chart kann außerdem eine `appVersion` aufweisen. Dieses Feld ist optional und muss nicht auf die Chartversion bezogen sein. Für manche Teams kann es sinnvoll sein, die Version von Anwendungen separat von Chartaktualisierungen zu verwalten. Es ist jedoch einfacher, nur eine Versionsnummer zu verwenden, so dass eine 1:1-Beziehung zwischen der Chartversion und der Anwendungsversion besteht. Auf diese Weise können Sie ein Chart pro Release speichern und das gewünschte Release komfortabel bereitstellen:

```bash
helm install <package-chart-name> --version <desiredVersion>
```

Eine weitere bewährte Methode besteht im Angeben einer Anmerkung für den Änderungsgrund in der Bereitstellungsvorlage:

```yaml
apiVersion: apps/v1beta2
kind: Deployment
metadata:
  name: {{ include "delivery.fullname" . | replace "." "" }}
  labels:
     ...
  annotations:
    kubernetes.io/change-cause: {{ .Values.reason }}
```

Auf diese Weise können Sie mithilfe des Befehls `kubectl rollout history` das Feld mit dem Änderungsgrund für jede Revision anzeigen. Im vorherigen Beispiel wird der Änderungsgrund als Helm-Chartparameter bereitgestellt.

```bash
$ kubectl rollout history deployments/delivery-v010 -n backend
deployment.extensions/delivery-v010
REVISION  CHANGE-CAUSE
1         Initial deployment
```

Sie können aber auch den Befehl `helm list` verwenden, um den Revisionsverlauf anzuzeigen:

```bash
~$ helm list
NAME            REVISION    UPDATED                     STATUS        CHART            APP VERSION     NAMESPACE
delivery-v0.1.0 1           Sun Apr  7 00:25:30 2019    DEPLOYED      delivery-v0.1.0  v0.1.0          backend
```

> [!TIP]
> Verwenden Sie beim Initialisieren von Helm das `--history-max`-Flag. Diese Einstellung schränkt die Anzahl der Revisionen ein, die Tiller in seinem Verlauf speichert. Tiller speichert den Revisionsverlauf in Konfigurationskarten. Wenn Sie häufig Aktualisierungen veröffentlichen, können die Konfigurationskarten sehr groß werden, sofern Sie die Verlaufsgröße nicht beschränken.

## <a name="azure-devops-pipeline"></a>Azure DevOps-Pipeline

In Azure Pipelines wird zwischen *Buildpipelines* und *Releasepipelines* unterschieden. Die Buildpipeline führt den CI-Prozess aus und erstellt Buildartefakte. Für eine Microservicearchitektur in Kubernetes sind diese Artefakte die Containerimages und Helm-Charts, die jeden Microservice definieren. Die Releasepipeline führt den CD-Prozess aus, der einen Microservice in einem Cluster bereitstellt.

Aufbauend auf dem weiter oben in diesem Artikel beschriebenen CI-Flow, kann eine Buildpipeline aus den folgenden Aufgaben bestehen:

1. Erstellen des Test Runner-Containers.

    ```yaml
    - task: Docker@1
      inputs:
        azureSubscriptionEndpoint: $(AzureSubscription)
        azureContainerRegistry: $(AzureContainerRegistry)
        arguments: '--pull --target testrunner'
        dockerFile: $(System.DefaultWorkingDirectory)/$(dockerFileName)
        imageName: '$(imageName)-test'
    ```

1. Ausführen der Tests durch Aufrufen der Docker-Ausführung für den Test Runner-Container.

    ```yaml
    - task: Docker@1
      inputs:
        azureSubscriptionEndpoint: $(AzureSubscription)
        azureContainerRegistry: $(AzureContainerRegistry)
        command: 'run'
        containerName: testrunner
        volumes: '$(System.DefaultWorkingDirectory)/TestResults:/app/tests/TestResults'
        imageName: '$(imageName)-test'
        runInBackground: false
    ```

1. Veröffentlichen der Testergebnisse. Weitere Informationen finden Sie unter [Integrate build and test tasks](/azure/devops/pipelines/languages/docker?view=azure-devops&tabs=yaml#integrate-build-and-test-tasks) (Integration von Build- und Testaufgaben).

    ```yaml
    - task: PublishTestResults@2
      inputs:
        testResultsFormat: 'VSTest'
        testResultsFiles: 'TestResults/*.trx'
        searchFolder: '$(System.DefaultWorkingDirectory)'
        publishRunAttachments: true
    ```

1. Erstellen Sie den Runtimecontainer.

    ```yaml
    - task: Docker@1
      inputs:
        azureSubscriptionEndpoint: $(AzureSubscription)
        azureContainerRegistry: $(AzureContainerRegistry)
        dockerFile: $(System.DefaultWorkingDirectory)/$(dockerFileName)
        includeLatestTag: false
        imageName: '$(imageName)'
    ```

1. Übertragen Sie per Push aus der Azure-Containerregistrierung (oder einer anderen Containerregistrierung) in den Container.

    ```yaml
    - task: Docker@1
      inputs:
        azureSubscriptionEndpoint: $(AzureSubscription)
        azureContainerRegistry: $(AzureContainerRegistry)
        command: 'Push an image'
        imageName: '$(imageName)'
        includeSourceTags: false
    ```

1. Verpacken Sie das Helm-Chart.

    ```yaml
    - task: HelmDeploy@0
      inputs:
        command: package
        chartPath: $(chartPath)
        chartVersion: $(Build.SourceBranchName)
        arguments: '--app-version $(Build.SourceBranchName)'
    ```

1. Übertragen Sie das Helm-Paket in die Azure-Containerregistrierung (oder ein anderes Helm-Repository).

    ```yaml
    task: AzureCLI@1
      inputs:
        azureSubscription: $(AzureSubscription)
        scriptLocation: inlineScript
        inlineScript: |
        az acr helm push $(System.ArtifactsDirectory)/$(repositoryName)-$(Build.SourceBranchName).tgz --name $(AzureContainerRegistry);
    ```

&#11162; Sehen Sie dazu die [Quelldatei](https://github.com/mspnp/microservices-reference-implementation/blob/master/src/shipping/delivery/azure-pipelines-ci.yml).

Die Ausgabe der CI-Pipeline ist ein produktionsbereites Containerimage und ein aktualisiertes Helm-Chart für den Microservice. An diesem Punkt kann die Releasepipeline übernehmen. Sie führt die folgenden Schritte aus:

- Bereitstellen in den DEV-/QA-/Stagingumgebungen.
- Warten auf eine genehmigende Person, die die Bereitstellung genehmigt oder ablehnt.
- Zuordnen eines neuen Tags zum Containerimage für das Release
- Übertragen Sie das Releasetag per Push in die Containerregistrierung.
- Aktualisieren Sie das Helm-Chart im Produktionscluster.

Weitere Informationen zum Erstellen einer Releasepipeline finden Sie unter [Release pipelines, draft releases, and release options](/azure/devops/pipelines/release/?view=azure-devops) (Releasepipelines, Entwurfsreleases und Releaseoptionen).

Im folgenden Diagramm ist der in diesem Artikel beschriebene End-to-End-CI/CD--Prozess dargestellt:

![CI/CD-Pipeline](./images/aks-cicd-flow.png)

## <a name="next-steps"></a>Nächste Schritte

Dieser Artikel basiert auf einer Referenzimplementierung, die Sie auf [GitHub][ri] finden können.

[ri]: https://github.com/mspnp/microservices-reference-implementation