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

# <a name="building-a-cicd-pipeline-for-microservices-on-kubernetes"></a><span data-ttu-id="ceb99-103">Erstellen einer CI/CD-Pipeline für Microservices in Kubernetes</span><span class="sxs-lookup"><span data-stu-id="ceb99-103">Building a CI/CD pipeline for microservices on Kubernetes</span></span>

<span data-ttu-id="ceb99-104">Das Erstellen eines zuverlässigen CI/CD-Prozesses für eine Microservicearchitektur kann schwierig sein.</span><span class="sxs-lookup"><span data-stu-id="ceb99-104">It can be challenging to create a reliable CI/CD process for a microservices architecture.</span></span> <span data-ttu-id="ceb99-105">Einzelne Teams müssen in der Lage sein, Dienste schnell und zuverlässig zu veröffentlichen, ohne andere Teams zu stören oder die Stabilität der gesamten Anwendung zu gefährden.</span><span class="sxs-lookup"><span data-stu-id="ceb99-105">Individual teams must be able to release services quickly and reliably, without disrupting other teams or destabilizing the application as a whole.</span></span>

<span data-ttu-id="ceb99-106">Dieser Artikel beschreibt eine CI/CD-Beispielpipeline für das Bereitstellen von Microservices in Azure Kubernetes Service (AKS).</span><span class="sxs-lookup"><span data-stu-id="ceb99-106">This article describes an example CI/CD pipeline for deploying microservices to Azure Kubernetes Service (AKS).</span></span> <span data-ttu-id="ceb99-107">Jedes Team und jedes Projekt ist anders, daher sollten Sie diesen Artikel nicht als feststehende Sammlung unverrückbarer Regeln verstehen.</span><span class="sxs-lookup"><span data-stu-id="ceb99-107">Every team and project is different, so don't take this article as a set of hard-and-fast rules.</span></span> <span data-ttu-id="ceb99-108">Er soll vielmehr einen Ausgangspunkt für Ihren Entwurf Ihres eigenen CI/CD-Prozesses bilden.</span><span class="sxs-lookup"><span data-stu-id="ceb99-108">Instead, it's meant to be a starting point for designing your own CI/CD process.</span></span>

<span data-ttu-id="ceb99-109">Die hier beschriebene Beispielpipeline wurde für eine Microservice-Referenzimplementierung erstellt, die sich Drohnenlieferungsanwendung nennt und auf [GitHub][ri] verfügbar ist.</span><span class="sxs-lookup"><span data-stu-id="ceb99-109">The example pipeline described here was created for a microservices reference implementation called the Drone Delivery application, which you can find on [GitHub][ri].</span></span> <span data-ttu-id="ceb99-110">Das Anwendungsszenario ist [hier](./design/index.md#reference-implementation) beschrieben.</span><span class="sxs-lookup"><span data-stu-id="ceb99-110">The application scenario is described [here](./design/index.md#reference-implementation).</span></span>

<span data-ttu-id="ceb99-111">Die Ziele der Pipeline können wie folgt zusammengefasst werden:</span><span class="sxs-lookup"><span data-stu-id="ceb99-111">The goals of the pipeline can be summarized as follows:</span></span>

- <span data-ttu-id="ceb99-112">Teams können ihre Dienste unabhängig voneinander entwickeln und bereitstellen.</span><span class="sxs-lookup"><span data-stu-id="ceb99-112">Teams can build and deploy their services independently.</span></span>
- <span data-ttu-id="ceb99-113">Codeänderungen, die den CI-Prozess durchlaufen, werden automatisch in einer produktionsähnlichen Umgebung bereitgestellt.</span><span class="sxs-lookup"><span data-stu-id="ceb99-113">Code changes that pass the CI process are automatically deployed to a production-like environment.</span></span>
- <span data-ttu-id="ceb99-114">In jeder Phase der Pipeline werden Quality Gates durchgesetzt.</span><span class="sxs-lookup"><span data-stu-id="ceb99-114">Quality gates are enforced at each stage of the pipeline.</span></span>
- <span data-ttu-id="ceb99-115">Eine neue Version eines Diensts kann neben der Vorgängerversion bereitgestellt werden.</span><span class="sxs-lookup"><span data-stu-id="ceb99-115">A new version of a service can be deployed side by side with the previous version.</span></span>

<span data-ttu-id="ceb99-116">Hintergrundinformationen dazu finden Sie unter [CI/CD für Microservicearchitekturen](./ci-cd.md).</span><span class="sxs-lookup"><span data-stu-id="ceb99-116">For more background, see [CI/CD for microservices architectures](./ci-cd.md).</span></span>

## <a name="assumptions"></a><span data-ttu-id="ceb99-117">Annahmen</span><span class="sxs-lookup"><span data-stu-id="ceb99-117">Assumptions</span></span>

<span data-ttu-id="ceb99-118">Für die Zwecke dieses Beispiels machen wir einige Annahmen über das Entwicklungsteam und die Codebasis:</span><span class="sxs-lookup"><span data-stu-id="ceb99-118">For purposes of this example, here are some assumptions about the development team and the code base:</span></span>

- <span data-ttu-id="ceb99-119">Für das Coderepository wird der Monorepo-Ansatz genutzt, und die Ordner sind nach Microservice organisiert.</span><span class="sxs-lookup"><span data-stu-id="ceb99-119">The code repository is a monorepo, with folders organized by microservice.</span></span>
- <span data-ttu-id="ceb99-120">Die Branchstrategie des Teams basiert auf der [Trunk-basierten Entwicklung](https://trunkbaseddevelopment.com/).</span><span class="sxs-lookup"><span data-stu-id="ceb99-120">The team's branching strategy is based on [trunk-based development](https://trunkbaseddevelopment.com/).</span></span>
- <span data-ttu-id="ceb99-121">Das Team verwendet [Releasebranches](/azure/devops/repos/git/git-branching-guidance?view=azure-devops#manage-releases) zur Verwaltung von Releases.</span><span class="sxs-lookup"><span data-stu-id="ceb99-121">The team uses [release branches](/azure/devops/repos/git/git-branching-guidance?view=azure-devops#manage-releases) to manage releases.</span></span> <span data-ttu-id="ceb99-122">Für jeden Microservice werden separate Releases erstellt.</span><span class="sxs-lookup"><span data-stu-id="ceb99-122">Separate releases are created for each microservice.</span></span>
- <span data-ttu-id="ceb99-123">Der CI/CD-Prozess verwendet [Azure Pipelines](/azure/devops/pipelines/?view=azure-devops) zum Erstellen, Testen und Bereitstellen der Microservices in AKS.</span><span class="sxs-lookup"><span data-stu-id="ceb99-123">The CI/CD process uses [Azure Pipelines](/azure/devops/pipelines/?view=azure-devops) to build, test, and deploy the microservices to AKS.</span></span>
- <span data-ttu-id="ceb99-124">Die Containerimages für die einzelnen Microservices sind in der [Azure-Containerregistrierung](/azure/container-registry/) gespeichert.</span><span class="sxs-lookup"><span data-stu-id="ceb99-124">The container images for each microservice are stored in [Azure Container Registry](/azure/container-registry/).</span></span>
- <span data-ttu-id="ceb99-125">Das Team verwendet Helm-Charts, um die Microservices zu verpacken.</span><span class="sxs-lookup"><span data-stu-id="ceb99-125">The team uses Helm charts to package each microservice.</span></span>

<span data-ttu-id="ceb99-126">Diese Annahmen bestimmen viele der spezifischen Details der CI/CD-Pipeline.</span><span class="sxs-lookup"><span data-stu-id="ceb99-126">These assumptions drive many of the specific details of the CI/CD pipeline.</span></span> <span data-ttu-id="ceb99-127">Der hier beschriebene Grundansatz lässt sich jedoch für andere Prozesse, Tools und Dienste anpassen, wie etwa Jenkins oder Docker Hub.</span><span class="sxs-lookup"><span data-stu-id="ceb99-127">However, the basic approach described here be adapted for other processes, tools, and services, such as Jenkins or Docker Hub.</span></span>

## <a name="validation-builds"></a><span data-ttu-id="ceb99-128">Validierungsbuilds</span><span class="sxs-lookup"><span data-stu-id="ceb99-128">Validation builds</span></span>

<span data-ttu-id="ceb99-129">Nehmen wir an, ein Entwickler arbeitet an einem Microservice mit dem Namen „Delivery Service“ (Lieferdienst).</span><span class="sxs-lookup"><span data-stu-id="ceb99-129">Suppose that a developer is working on a microservice called the Delivery Service.</span></span> <span data-ttu-id="ceb99-130">Beim Entwickeln eines neuen Features checkt der Entwickler Code in einen Featurebranch ein.</span><span class="sxs-lookup"><span data-stu-id="ceb99-130">While developing a new feature, the developer checks code into a feature branch.</span></span> <span data-ttu-id="ceb99-131">Für Featurebranches wird die Benennungskonvention `feature/*` verwendet.</span><span class="sxs-lookup"><span data-stu-id="ceb99-131">By convention, feature branches are named `feature/*`.</span></span>

![CI/CD-Workflow](./images/aks-cicd-1.png)

<span data-ttu-id="ceb99-133">Die Builddefinitionsdatei enthält einen Trigger, der nach dem Branchnamen und dem Quellpfad filtert:</span><span class="sxs-lookup"><span data-stu-id="ceb99-133">The build definition file includes a trigger that filters by the branch name and the source path:</span></span>

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

<span data-ttu-id="ceb99-134">&#11162; Sehen Sie dazu die [Quelldatei](https://github.com/mspnp/microservices-reference-implementation/blob/master/src/shipping/delivery/azure-pipelines-validation.yml).</span><span class="sxs-lookup"><span data-stu-id="ceb99-134">&#11162; See the [source file](https://github.com/mspnp/microservices-reference-implementation/blob/master/src/shipping/delivery/azure-pipelines-validation.yml).</span></span>

<span data-ttu-id="ceb99-135">Mit diesem Ansatz kann jedes Team über eine eigene Buildpipeline verfügen.</span><span class="sxs-lookup"><span data-stu-id="ceb99-135">Using this approach, each team can have its own build pipeline.</span></span> <span data-ttu-id="ceb99-136">Nur Code, der im Ordner `/src/shipping/delivery` eingecheckt wird, löst einen Build des Lieferdiensts aus.</span><span class="sxs-lookup"><span data-stu-id="ceb99-136">Only code that is checked into the `/src/shipping/delivery` folder triggers a build of the Delivery Service.</span></span> <span data-ttu-id="ceb99-137">Das Pushen von Commits auf einen Branch, der mit dem Filter übereinstimmt, löst einen CI-Build aus.</span><span class="sxs-lookup"><span data-stu-id="ceb99-137">Pushing commits to a branch that matches the filter triggers a CI build.</span></span> <span data-ttu-id="ceb99-138">An diesem Punkt des Workflows führt der CI-Build eine Codeüberprüfung mit minimalem Umfang durch:</span><span class="sxs-lookup"><span data-stu-id="ceb99-138">At this point in the workflow, the CI build runs some minimal code verification:</span></span>

1. <span data-ttu-id="ceb99-139">Erstellen des Codes.</span><span class="sxs-lookup"><span data-stu-id="ceb99-139">Build the code.</span></span>
1. <span data-ttu-id="ceb99-140">Ausführen von Komponententests.</span><span class="sxs-lookup"><span data-stu-id="ceb99-140">Run unit tests.</span></span>

<span data-ttu-id="ceb99-141">Das Ziel besteht darin, die Buildzeiten kurz zu halten, damit der Entwickler schnell Feedback erhält.</span><span class="sxs-lookup"><span data-stu-id="ceb99-141">The goal is to keep build times short, so the developer can get quick feedback.</span></span> <span data-ttu-id="ceb99-142">Wenn das Feature für die Zusammenführung mit dem Master bereit ist, öffnet der Entwickler einen PR-Vorgang.</span><span class="sxs-lookup"><span data-stu-id="ceb99-142">Once the feature is ready to merge into master, the developer opens a PR.</span></span> <span data-ttu-id="ceb99-143">Hierdurch wird ein weiterer CI-Build ausgelöst, bei dem einige zusätzliche Überprüfungen durchgeführt werden:</span><span class="sxs-lookup"><span data-stu-id="ceb99-143">This triggers another CI build that performs some additional checks:</span></span>

1. <span data-ttu-id="ceb99-144">Erstellen des Codes.</span><span class="sxs-lookup"><span data-stu-id="ceb99-144">Build the code.</span></span>
1. <span data-ttu-id="ceb99-145">Ausführen von Komponententests.</span><span class="sxs-lookup"><span data-stu-id="ceb99-145">Run unit tests.</span></span>
1. <span data-ttu-id="ceb99-146">Erstellen des Runtime-Containerimages.</span><span class="sxs-lookup"><span data-stu-id="ceb99-146">Build the runtime container image.</span></span>
1. <span data-ttu-id="ceb99-147">Durchführen von Überprüfungen auf Sicherheitsrisiken für das Image.</span><span class="sxs-lookup"><span data-stu-id="ceb99-147">Run vulnerability scans on the image.</span></span>

![CI/CD-Workflow](./images/aks-cicd-2.png)

> [!NOTE]
> <span data-ttu-id="ceb99-149">In Azure DevOps-Repos können Sie [Richtlinien](/azure/devops/repos/git/branch-policies) zum Schützen von Branches definieren.</span><span class="sxs-lookup"><span data-stu-id="ceb99-149">In Azure DevOps Repos, you can define [policies](/azure/devops/repos/git/branch-policies) to protect branches.</span></span> <span data-ttu-id="ceb99-150">Für die Richtlinie sind beispielsweise ggf. ein erfolgreicher CI-Build sowie eine Genehmigung eines Prüfers erforderlich, damit die Zusammenführung mit dem Master erfolgen kann.</span><span class="sxs-lookup"><span data-stu-id="ceb99-150">For example, the policy could require a successful CI build plus a sign-off from an approver in order to merge into master.</span></span>

## <a name="full-cicd-build"></a><span data-ttu-id="ceb99-151">Vollständiger CI/CD-Build</span><span class="sxs-lookup"><span data-stu-id="ceb99-151">Full CI/CD build</span></span>

<span data-ttu-id="ceb99-152">An einem bestimmten Punkt ist das Team zum Bereitstellen einer neuen Version des Lieferdiensts bereit.</span><span class="sxs-lookup"><span data-stu-id="ceb99-152">At some point, the team is ready to deploy a new version of the Delivery service.</span></span> <span data-ttu-id="ceb99-153">Der Release Manager erstellt einen Branch vom Master und verwendet dazu das folgende Benennungsmuster: `release/<microservice name>/<semver>`.</span><span class="sxs-lookup"><span data-stu-id="ceb99-153">The release manager creates a branch from master with this naming pattern: `release/<microservice name>/<semver>`.</span></span> <span data-ttu-id="ceb99-154">Beispiel: `release/delivery/v1.0.2`.</span><span class="sxs-lookup"><span data-stu-id="ceb99-154">For example, `release/delivery/v1.0.2`.</span></span>

![CI/CD-Workflow](./images/aks-cicd-3.png)

<span data-ttu-id="ceb99-156">Die Erstellung dieses Branches löst einen vollständigen CI-Build aus, für den alle der vorstehenden Schritte ausgeführt werden, plus:</span><span class="sxs-lookup"><span data-stu-id="ceb99-156">Creation of this branch triggers a full CI build that runs all of the previous steps plus:</span></span>

1. <span data-ttu-id="ceb99-157">Pushübertragung des Containerimages in die Azure-Containerregistrierung</span><span class="sxs-lookup"><span data-stu-id="ceb99-157">Push the container image to Azure Container Registry.</span></span> <span data-ttu-id="ceb99-158">Das Image wird mit der Versionsnummer aus dem Branchnamen versehen.</span><span class="sxs-lookup"><span data-stu-id="ceb99-158">The image is tagged with the version number taken from the branch name.</span></span>
2. <span data-ttu-id="ceb99-159">Ausführen von `helm package` zum Verpacken des Helm-Charts für den Dienst</span><span class="sxs-lookup"><span data-stu-id="ceb99-159">Run `helm package` to package the Helm chart for the service.</span></span> <span data-ttu-id="ceb99-160">Das Chart ist außerdem mit einer Versionsnummer gekennzeichnet.</span><span class="sxs-lookup"><span data-stu-id="ceb99-160">The chart is also tagged with a version number.</span></span>
3. <span data-ttu-id="ceb99-161">Übertragen des Helm-Pakets an die Containerregistrierung per Push.</span><span class="sxs-lookup"><span data-stu-id="ceb99-161">Push the Helm package to Container Registry.</span></span>

<span data-ttu-id="ceb99-162">Wenn dieser Buildvorgang erfolgreich ist, wird mithilfe einer [Releasepipeline](/azure/devops/pipelines/release/what-is-release-management) von Azure Pipelines ein Bereitstellungsprozess (CD) ausgelöst.</span><span class="sxs-lookup"><span data-stu-id="ceb99-162">Assuming this build succeeds, it triggers a deployment (CD) process using an Azure Pipelines [release pipeline](/azure/devops/pipelines/release/what-is-release-management).</span></span> <span data-ttu-id="ceb99-163">Diese Pipeline weist folgende Stufen auf:</span><span class="sxs-lookup"><span data-stu-id="ceb99-163">This pipeline has the following steps:</span></span>

1. <span data-ttu-id="ceb99-164">Bereitstellen des Helm-Charts in einer QA-Umgebung.</span><span class="sxs-lookup"><span data-stu-id="ceb99-164">Deploy the Helm chart to a QA environment.</span></span>
1. <span data-ttu-id="ceb99-165">Eine genehmigende Person meldet sich ab, bevor das Paket für die Produktion bereitgestellt wird.</span><span class="sxs-lookup"><span data-stu-id="ceb99-165">An approver signs off before the package moves to production.</span></span> <span data-ttu-id="ceb99-166">Weitere Informationen finden Sie unter [Release deployment control using approvals](/azure/devops/pipelines/release/approvals/approvals) (Steuerung von Releasebereitstellungen durch Genehmigungen).</span><span class="sxs-lookup"><span data-stu-id="ceb99-166">See [Release deployment control using approvals](/azure/devops/pipelines/release/approvals/approvals).</span></span>
1. <span data-ttu-id="ceb99-167">Versehen Sie das Docker-Image für den Produktionsnamespace in Azure Container Registry mit einem neuen Tag.</span><span class="sxs-lookup"><span data-stu-id="ceb99-167">Retag the Docker image for the production namespace in Azure Container Registry.</span></span> <span data-ttu-id="ceb99-168">Wenn das derzeitige Tag beispielsweise `myrepo.azurecr.io/delivery:v1.0.2` lautet, lautet das Tag für die Produktion `myrepo.azurecr.io/prod/delivery:v1.0.2`.</span><span class="sxs-lookup"><span data-stu-id="ceb99-168">For example, if the current tag is `myrepo.azurecr.io/delivery:v1.0.2`, the production tag is `myrepo.azurecr.io/prod/delivery:v1.0.2`.</span></span>
1. <span data-ttu-id="ceb99-169">Stellen Sie das Helm-Chart in der Produktionsumgebung bereit.</span><span class="sxs-lookup"><span data-stu-id="ceb99-169">Deploy the Helm chart to the production environment.</span></span>

<span data-ttu-id="ceb99-170">Auch beim Monorepo-Ansatz können diese Aufgaben einzelnen Microservices zugeordnet werden, damit die Teams Bereitstellungen schneller durchführen können.</span><span class="sxs-lookup"><span data-stu-id="ceb99-170">Even in a monorepo, these tasks can be scoped to individual microservices, so that teams can deploy with high velocity.</span></span> <span data-ttu-id="ceb99-171">Der Prozess weist einige manuelle Schritte auf: Genehmigen von PRs, Erstellen von Releasebranches und Genehmigen von Bereitstellungen im Produktionscluster.</span><span class="sxs-lookup"><span data-stu-id="ceb99-171">The process has some manual steps: Approving PRs, creating release branches, and approving deployments into the production cluster.</span></span> <span data-ttu-id="ceb99-172">Der Richtlinie nach sind dies manuelle Schritte – aber sie können auch automatisiert werden, falls die Organisation dies vorzieht.</span><span class="sxs-lookup"><span data-stu-id="ceb99-172">These steps are manual by policy &mdash; they could be automated if the organization prefers.</span></span>

## <a name="isolation-of-environments"></a><span data-ttu-id="ceb99-173">Isolation von Umgebungen</span><span class="sxs-lookup"><span data-stu-id="ceb99-173">Isolation of environments</span></span>

<span data-ttu-id="ceb99-174">Sie verfügen über mehrere Umgebungen, in denen Sie Dienste bereitstellen, z.B. Entwicklung, Feuerprobe, Integrationstests, Auslastungstests und schließlich Produktion.</span><span class="sxs-lookup"><span data-stu-id="ceb99-174">You will have multiple environments where you deploy services, including environments for development, smoke testing, integration testing, load testing, and finally production.</span></span> <span data-ttu-id="ceb99-175">Für diese Umgebungen ist ein gewisses Maß an Isolation erforderlich.</span><span class="sxs-lookup"><span data-stu-id="ceb99-175">These environments need some level of isolation.</span></span> <span data-ttu-id="ceb99-176">In Kubernetes haben Sie die Wahl zwischen physischer Isolation und logischer Isolation.</span><span class="sxs-lookup"><span data-stu-id="ceb99-176">In Kubernetes, you have a choice between physical isolation and logical isolation.</span></span> <span data-ttu-id="ceb99-177">Physische Isolation bedeutet, dass die Bereitstellung in separaten Clustern erfolgt.</span><span class="sxs-lookup"><span data-stu-id="ceb99-177">Physical isolation means deploying to separate clusters.</span></span> <span data-ttu-id="ceb99-178">Bei der logischen Isolation werden wie oben beschrieben Namespaces und Richtlinien verwendet.</span><span class="sxs-lookup"><span data-stu-id="ceb99-178">Logical isolation makes use of namespaces and policies, as described earlier.</span></span>

<span data-ttu-id="ceb99-179">Unsere Empfehlung lautet, einen dedizierten Produktionscluster mit einem zusätzlichen separaten Cluster für Ihre Entwicklungs- und Testumgebungen zu erstellen.</span><span class="sxs-lookup"><span data-stu-id="ceb99-179">Our recommendation is to create a dedicated production cluster along with a separate cluster for your dev/test environments.</span></span> <span data-ttu-id="ceb99-180">Verwenden Sie die logische Isolation, um Umgebungen im Entwicklungs-/Testcluster zu trennen.</span><span class="sxs-lookup"><span data-stu-id="ceb99-180">Use logical isolation to separate environments within the dev/test cluster.</span></span> <span data-ttu-id="ceb99-181">Im Entwicklungs-/Testcluster bereitgestellte Dienste sollten niemals Zugriff auf Datenspeicher haben, in denen sich Geschäftsdaten befinden.</span><span class="sxs-lookup"><span data-stu-id="ceb99-181">Services deployed to the dev/test cluster should never have access to data stores that hold business data.</span></span>

## <a name="build-process"></a><span data-ttu-id="ceb99-182">Buildprozess</span><span class="sxs-lookup"><span data-stu-id="ceb99-182">Build process</span></span>

<span data-ttu-id="ceb99-183">Verpacken Sie nach Möglichkeit Ihren Buildprozess in einem Docker-Container.</span><span class="sxs-lookup"><span data-stu-id="ceb99-183">When possible, package your build process into a Docker container.</span></span> <span data-ttu-id="ceb99-184">Auf diese Weise können Sie Ihre Codeartefakte mithilfe von Docker erstellen, ohne die Buildumgebung auf jedem Buildcomputer konfigurieren zu müssen.</span><span class="sxs-lookup"><span data-stu-id="ceb99-184">That allows you to build your code artifacts using Docker, without needing to configure the build environment on each build machine.</span></span> <span data-ttu-id="ceb99-185">Ein Container-Buildprozess erleichtert das horizontale Hochskalieren der CI-Pipeline durch Hinzufügen neuer Build-Agents.</span><span class="sxs-lookup"><span data-stu-id="ceb99-185">A containerized build process makes it easy to scale out the CI pipeline by adding new build agents.</span></span> <span data-ttu-id="ceb99-186">Außerdem kann jeder Entwickler im Team den Code durch einfaches Ausführen des Buildcontainers erstellen.</span><span class="sxs-lookup"><span data-stu-id="ceb99-186">Also, any developer on the team can build the code simply by running the build container.</span></span>

<span data-ttu-id="ceb99-187">Durch die Verwendung mehrstufiger Builds in Docker können Sie die Buildumgebung und das Laufzeitimage in einem einzelnen Dockerfile definieren.</span><span class="sxs-lookup"><span data-stu-id="ceb99-187">By using multi-stage builds in Docker, you can define the build environment and the runtime image in a single Dockerfile.</span></span> <span data-ttu-id="ceb99-188">Dies ist beispielsweise ein Dockerfile, das eine ASP.NET Core-Anwendung erstellt:</span><span class="sxs-lookup"><span data-stu-id="ceb99-188">For example, here's a Dockerfile that builds an ASP.NET Core application:</span></span>

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

<span data-ttu-id="ceb99-189">&#11162; Sehen Sie dazu die [Quelldatei](https://github.com/mspnp/microservices-reference-implementation/blob/master/src/shipping/workflow/Dockerfile).</span><span class="sxs-lookup"><span data-stu-id="ceb99-189">&#11162; See the [source file](https://github.com/mspnp/microservices-reference-implementation/blob/master/src/shipping/workflow/Dockerfile).</span></span>

<span data-ttu-id="ceb99-190">Dieses Dockerfile definiert mehrere Buildphasen.</span><span class="sxs-lookup"><span data-stu-id="ceb99-190">This Dockerfile defines several build stages.</span></span> <span data-ttu-id="ceb99-191">Beachten Sie, dass die Phase mit dem Namen `base` die ASP.NET Core-Runtime verwendet, während die Phase mit dem Namen `build` das vollständige ASP.NET Core SDK verwendet.</span><span class="sxs-lookup"><span data-stu-id="ceb99-191">Notice that the stage named `base` uses the ASP.NET Core runtime, while the stage named `build` uses the full ASP.NET Core SDK.</span></span> <span data-ttu-id="ceb99-192">Die `build` Phase wird verwendet, um das ASP.NET Core-Projekt zu erstellen.</span><span class="sxs-lookup"><span data-stu-id="ceb99-192">The `build` stage is used to build the ASP.NET Core project.</span></span> <span data-ttu-id="ceb99-193">Der endgültige Laufzeitcontainer wird jedoch aus `base` erstellt, das nur die Runtime enthält und erheblich kleiner als das vollständige SDK-Image ist.</span><span class="sxs-lookup"><span data-stu-id="ceb99-193">But the final runtime container is built from `base`, which contains just the runtime and is significantly smaller than the full SDK image.</span></span>

### <a name="building-a-test-runner"></a><span data-ttu-id="ceb99-194">Erstellen eines Test Runners</span><span class="sxs-lookup"><span data-stu-id="ceb99-194">Building a test runner</span></span>

<span data-ttu-id="ceb99-195">Eine weitere bewährte Methode ist das Ausführen von Komponententests im Container.</span><span class="sxs-lookup"><span data-stu-id="ceb99-195">Another good practice is to run unit tests in the container.</span></span> <span data-ttu-id="ceb99-196">Dies ist beispielsweise ein Teil einer Docker-Datei, die einen Test Runner erstellt:</span><span class="sxs-lookup"><span data-stu-id="ceb99-196">For example, here is part of a Docker file that builds a test runner:</span></span>

```
FROM build AS testrunner
WORKDIR /src/tests

COPY Fabrikam.Workflow.Service.Tests/*.csproj .
RUN dotnet restore Fabrikam.Workflow.Service.Tests.csproj

COPY Fabrikam.Workflow.Service.Tests/. .
ENTRYPOINT ["dotnet", "test", "--logger:trx"]
```

<span data-ttu-id="ceb99-197">Ein Entwickler kann diese Docker-Datei zur lokalen Ausführung der Tests verwenden:</span><span class="sxs-lookup"><span data-stu-id="ceb99-197">A developer can use this Docker file to run the tests locally:</span></span>

```bash
docker build . -t delivery-test:1 --target=testrunner
docker run -p 8080:8080 delivery-test:1
```

<span data-ttu-id="ceb99-198">Die CI-Pipeline sollte die Tests ebenfalls im Rahmen des Buildüberprüfungsschritts ausführen.</span><span class="sxs-lookup"><span data-stu-id="ceb99-198">The CI pipeline should also run the tests as part of the build verification step.</span></span>

<span data-ttu-id="ceb99-199">Beachten Sie, dass in dieser Datei der Docker-Befehl `ENTRYPOINT` verwendet wird, um die Tests auszuführen, nicht der Docker-Befehl `RUN`.</span><span class="sxs-lookup"><span data-stu-id="ceb99-199">Note that this file uses the Docker `ENTRYPOINT` command to run the tests, not the Docker `RUN` command.</span></span>

- <span data-ttu-id="ceb99-200">Wenn Sie den Befehl `RUN` verwenden, werden die Tests jedes Mal ausgeführt, wenn Sie das Image erstellen.</span><span class="sxs-lookup"><span data-stu-id="ceb99-200">If you use the `RUN` command, the tests run every time you build the image.</span></span> <span data-ttu-id="ceb99-201">Durch Verwendung von `ENTRYPOINT` sind die Tests optional aktivierbar.</span><span class="sxs-lookup"><span data-stu-id="ceb99-201">By using `ENTRYPOINT`, the tests are opt-in.</span></span> <span data-ttu-id="ceb99-202">Sie werden nur ausgeführt, wenn Sie die Phase `testrunner` explizit ansteuern.</span><span class="sxs-lookup"><span data-stu-id="ceb99-202">They run only when you explicitly target the `testrunner` stage.</span></span>
- <span data-ttu-id="ceb99-203">Ein Fehler im Test führt nicht zu einem Fehler beim Docker-Befehl `build`.</span><span class="sxs-lookup"><span data-stu-id="ceb99-203">A failing test doesn't cause the Docker `build` command to fail.</span></span> <span data-ttu-id="ceb99-204">Auf diese Weise können Sie Buildfehler des Containers von Testfehlern unterscheiden.</span><span class="sxs-lookup"><span data-stu-id="ceb99-204">That way you can distinguish container build failures from test failures.</span></span>
- <span data-ttu-id="ceb99-205">Testergebnisse können auf einem eingebundenen Volume gespeichert werden.</span><span class="sxs-lookup"><span data-stu-id="ceb99-205">Test results can be saved to a mounted volume.</span></span>

### <a name="container-best-practices"></a><span data-ttu-id="ceb99-206">Bewährte Methoden für Container</span><span class="sxs-lookup"><span data-stu-id="ceb99-206">Container best practices</span></span>

<span data-ttu-id="ceb99-207">Hier sind einige weitere bewährte Methoden, die beim Arbeiten mit Containern berücksichtigt werden sollten:</span><span class="sxs-lookup"><span data-stu-id="ceb99-207">Here are some other best practices to consider for containers:</span></span>

- <span data-ttu-id="ceb99-208">Definieren Sie organisationsweite Konventionen für Containertags und für die Versionsverwaltung sowie Namenskonventionen für im Cluster bereitgestellte Ressourcen (Pods, Dienste und Ähnliches).</span><span class="sxs-lookup"><span data-stu-id="ceb99-208">Define organization-wide conventions for container tags, versioning, and naming conventions for resources deployed to the cluster (pods, services, and so on).</span></span> <span data-ttu-id="ceb99-209">Dies kann die Diagnose von Bereitstellungsproblemen vereinfachen.</span><span class="sxs-lookup"><span data-stu-id="ceb99-209">That can make it easier to diagnose deployment issues.</span></span>

- <span data-ttu-id="ceb99-210">Im Rahmen des Entwicklungs- und Testzyklus erstellt der CI/CD-Prozess zahlreiche Containerimages.</span><span class="sxs-lookup"><span data-stu-id="ceb99-210">During the development and test cycle, the CI/CD process will build many container images.</span></span> <span data-ttu-id="ceb99-211">Nicht alle dieser Images kommen für die Veröffentlichung in Frage, und nur einige der so genannten Release Candidates werden in die Produktionsumgebung heraufgestuft.</span><span class="sxs-lookup"><span data-stu-id="ceb99-211">Only some of those images are candidates for release, and then only some of those release candidates will get promoted to production.</span></span> <span data-ttu-id="ceb99-212">Entwickeln Sie eine klare Versionsverwaltungsstrategie, damit Sie wissen, welche Images derzeit in der Produktionsumgebung bereitgestellt sind, und bei Bedarf einen Rollback auf eine vorherige Version ausführen können.</span><span class="sxs-lookup"><span data-stu-id="ceb99-212">Have a clear versioning strategy, so that you know which images are currently deployed to production, and can roll back to a previous version if necessary.</span></span>

- <span data-ttu-id="ceb99-213">Stellen Sie immer spezifische Containerversionstags bereit, nicht einfach `latest`.</span><span class="sxs-lookup"><span data-stu-id="ceb99-213">Always deploy specific container version tags, not `latest`.</span></span>

- <span data-ttu-id="ceb99-214">Verwenden Sie [Namespaces](/azure/container-registry/container-registry-best-practices#repository-namespaces) in der Azure-Containerregistrierung, um Images, die bereits für die Produktion genehmigt wurden, von den noch in der Testphase befindlichen Images zu isolieren.</span><span class="sxs-lookup"><span data-stu-id="ceb99-214">Use [namespaces](/azure/container-registry/container-registry-best-practices#repository-namespaces) in Azure Container Registry to isolate images that are approved for production from images that are still being tested.</span></span> <span data-ttu-id="ceb99-215">Verschieben Sie ein Image nur dann in den Produktionsnamespace, wenn Sie bereit sind, es für die Produktion bereitzustellen.</span><span class="sxs-lookup"><span data-stu-id="ceb99-215">Don't move an image into the production namespace until you're ready to deploy it into production.</span></span> <span data-ttu-id="ceb99-216">Wenn Sie diese Vorgehensweise mit der semantischen Versionsverwaltung von Containerimages kombinieren, verringert sich die Wahrscheinlichkeit, dass Sie versehentlich eine Version bereitstellen, die nicht für die Veröffentlichung freigegeben wurde.</span><span class="sxs-lookup"><span data-stu-id="ceb99-216">If you combine this practice with semantic versioning of container images, it can reduce the chance of accidentally deploying a version that wasn't approved for release.</span></span>

- <span data-ttu-id="ceb99-217">Befolgen Sie das Prinzip der geringsten Rechte, indem Sie Container als Benutzer ohne besondere Rechte ausführen.</span><span class="sxs-lookup"><span data-stu-id="ceb99-217">Follow the principle of least privilege by running containers as a nonprivileged user.</span></span> <span data-ttu-id="ceb99-218">In Kubernetes können Sie eine Pod-Sicherheitsrichtlinie erstellen, die die Ausführung von Containern als *root* verhindert.</span><span class="sxs-lookup"><span data-stu-id="ceb99-218">In Kubernetes, you can create a pod security policy that prevents containers from running as *root*.</span></span> <span data-ttu-id="ceb99-219">Weitere Informationen finden Sie unter [Prevent Pods From Running With Root Privileges](https://docs.bitnami.com/kubernetes/how-to/secure-kubernetes-cluster-psp/) (Verhindern der Ausführung von Pods mit Stammberechtigungen).</span><span class="sxs-lookup"><span data-stu-id="ceb99-219">See [Prevent Pods From Running With Root Privileges](https://docs.bitnami.com/kubernetes/how-to/secure-kubernetes-cluster-psp/).</span></span>

## <a name="helm-charts"></a><span data-ttu-id="ceb99-220">Helm-Diagramme</span><span class="sxs-lookup"><span data-stu-id="ceb99-220">Helm charts</span></span>

<span data-ttu-id="ceb99-221">Erwägen Sie die Verwendung von Helm zum Erstellen und Bereitstellen von Diensten.</span><span class="sxs-lookup"><span data-stu-id="ceb99-221">Consider using Helm to manage building and deploying services.</span></span> <span data-ttu-id="ceb99-222">Dies sind einige der Features von Helm, die für CI/CD nützlich sind:</span><span class="sxs-lookup"><span data-stu-id="ceb99-222">Here are some of the features of Helm that help with CI/CD:</span></span>

- <span data-ttu-id="ceb99-223">Häufig wird ein einzelner Microservice durch mehrere Kubernetes-Objekte definiert.</span><span class="sxs-lookup"><span data-stu-id="ceb99-223">Often a single microservice is defined by multiple Kubernetes objects.</span></span> <span data-ttu-id="ceb99-224">Helm ermöglicht das Verpacken dieser Objekte in einem einzelnen Helm-Chart.</span><span class="sxs-lookup"><span data-stu-id="ceb99-224">Helm allows these objects to be packaged into a single Helm chart.</span></span>
- <span data-ttu-id="ceb99-225">Ein Chart kann mit einem einzelnen Helm-Befehl statt einer Reihe von kubectl-Befehlen bereitgestellt werden.</span><span class="sxs-lookup"><span data-stu-id="ceb99-225">A chart can be deployed with a single Helm command, rather than a series of kubectl commands.</span></span>
- <span data-ttu-id="ceb99-226">Charts enthalten eine explizite Versionsangabe.</span><span class="sxs-lookup"><span data-stu-id="ceb99-226">Charts are explicitly versioned.</span></span> <span data-ttu-id="ceb99-227">Verwenden Sie Helm, um eine Version zu veröffentlichen, Releases anzuzeigen und das Rollback zu einer früheren Version auszuführen.</span><span class="sxs-lookup"><span data-stu-id="ceb99-227">Use Helm to release a version, view releases, and roll back to a previous version.</span></span> <span data-ttu-id="ceb99-228">Nachverfolgen von Updates und Revisionen per semantischer Versionierung und möglicher Rollback auf eine vorherige Version</span><span class="sxs-lookup"><span data-stu-id="ceb99-228">Tracking updates and revisions, using semantic versioning, along with the ability to roll back to a previous version.</span></span>
- <span data-ttu-id="ceb99-229">Helm-Charts verwenden Vorlagen zur Vermeidung von doppelten Informationen, z.B. Bezeichnungen und Selektoren, über viele Dateien hinweg.</span><span class="sxs-lookup"><span data-stu-id="ceb99-229">Helm charts use templates to avoid duplicating information, such as labels and selectors, across many files.</span></span>
- <span data-ttu-id="ceb99-230">Helm kann Abhängigkeiten zwischen Charts verwalten.</span><span class="sxs-lookup"><span data-stu-id="ceb99-230">Helm can manage dependencies between charts.</span></span>
- <span data-ttu-id="ceb99-231">Charts können in einem Helm-Repository gespeichert werden, beispielsweise in der Azure-Containerregistrierung, und in die Buildpipeline integriert werden.</span><span class="sxs-lookup"><span data-stu-id="ceb99-231">Charts can be stored in a Helm repository, such as Azure Container Registry, and integrated into the build pipeline.</span></span>

<span data-ttu-id="ceb99-232">Weitere Informationen zur Verwendung von Container Registry als Helm-Repository finden Sie unter [Verwenden Sie Azure Container Registry als Helm-Repository für Ihre Anwendungsdiagramme](/azure/container-registry/container-registry-helm-repos).</span><span class="sxs-lookup"><span data-stu-id="ceb99-232">For more information about using Container Registry as a Helm repository, see [Use Azure Container Registry as a Helm repository for your application charts](/azure/container-registry/container-registry-helm-repos).</span></span>

<span data-ttu-id="ceb99-233">Bei einem einzelnen Microservice können mehrere Kubernetes-Konfigurationsdateien im Spiel sein.</span><span class="sxs-lookup"><span data-stu-id="ceb99-233">A single microservice may involve multiple Kubernetes configuration files.</span></span> <span data-ttu-id="ceb99-234">Das Aktualisieren eines Diensts kann bedeuten, dass alle diese Dateien bearbeitet werden müssen, um Selektoren, Bezeichnungen und Imagetags zu aktualisieren.</span><span class="sxs-lookup"><span data-stu-id="ceb99-234">Updating a service can mean touching all of these files to update selectors, labels, and image tags.</span></span> <span data-ttu-id="ceb99-235">Helm behandelt diese als einzelnes Paket, das Chart genannt wird und es Ihnen ermöglicht, die YAML-Dateien mithilfe von Variablen komfortabel zu aktualisieren.</span><span class="sxs-lookup"><span data-stu-id="ceb99-235">Helm treats these as a single package called a chart and allows you to easily update the YAML files by using variables.</span></span> <span data-ttu-id="ceb99-236">Helm verwendet eine Vorlagensprache (die auf Go-Vorlagen beruht), um Ihnen das Schreiben von parametrisierten YAML-Konfigurationsdateien zu ermöglichen.</span><span class="sxs-lookup"><span data-stu-id="ceb99-236">Helm uses a template language (based on Go templates) to let you write parameterized YAML configuration files.</span></span>

<span data-ttu-id="ceb99-237">Dies ist beispielsweise ein Teil einer YAML-Datei, die eine Bereitstellung definiert:</span><span class="sxs-lookup"><span data-stu-id="ceb99-237">For example, here's part of a YAML file that defines a deployment:</span></span>

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

<span data-ttu-id="ceb99-238">&#11162; Sehen Sie dazu die [Quelldatei](https://github.com/mspnp/microservices-reference-implementation/blob/master/charts/package/templates/package-deploy.yaml).</span><span class="sxs-lookup"><span data-stu-id="ceb99-238">&#11162; See the [source file](https://github.com/mspnp/microservices-reference-implementation/blob/master/charts/package/templates/package-deploy.yaml).</span></span>

<span data-ttu-id="ceb99-239">Sie können sehen, dass für den Bereitstellungsnamen, die Bezeichnungen und die Containerspezifikation sämtlich Vorlagenparameter verwendet werden, die zum Zeitpunkt der Bereitstellung zur Verfügung gestellt werden.</span><span class="sxs-lookup"><span data-stu-id="ceb99-239">You can see that the deployment name, labels, and container spec all use template parameters, which are provided at deployment time.</span></span> <span data-ttu-id="ceb99-240">Beispielsweise an der Befehlszeile:</span><span class="sxs-lookup"><span data-stu-id="ceb99-240">For example, from the command line:</span></span>

```bash
helm install $HELM_CHARTS/package/ \
     --set image.tag=0.1.0 \
     --set image.repository=package \
     --set dockerregistry=$ACR_SERVER \
     --namespace backend \
     --name package-v0.1.0
```

<span data-ttu-id="ceb99-241">Zwar könnte Ihre CI/CD-Pipeline ein Chart direkt in Kubernetes installieren, wir empfehlen jedoch, ein Chartarchiv (TGZ-Datei) zu erstellen und das Chart per Push in ein Helm-Repository zu übertragen, wie etwa die Azure-Containerregistrierung.</span><span class="sxs-lookup"><span data-stu-id="ceb99-241">Although your CI/CD pipeline could install a chart directly to Kubernetes, we recommend creating a chart archive (.tgz file) and pushing the chart to a Helm repository such as Azure Container Registry.</span></span> <span data-ttu-id="ceb99-242">Weitere Informationen finden Sie unter [Package Docker-based apps in Helm charts in Azure Pipelines](/azure/devops/pipelines/languages/helm?view=azure-devops) (Verpacken von Docker-basierten Apps in Helm-Charts in Azure Pipelines).</span><span class="sxs-lookup"><span data-stu-id="ceb99-242">For more information, see [Package Docker-based apps in Helm charts in Azure Pipelines](/azure/devops/pipelines/languages/helm?view=azure-devops).</span></span>

<span data-ttu-id="ceb99-243">Erwägen Sie, Helm in seinem eigenen Namespace bereitzustellen und RBAC (rollenbasierte Zugriffssteuerung) zu verwenden, um die Namespaces für die Bereitstellung einzuschränken.</span><span class="sxs-lookup"><span data-stu-id="ceb99-243">Consider deploying Helm to its own namespace and using role-based access control (RBAC) to restrict which namespaces it can deploy to.</span></span> <span data-ttu-id="ceb99-244">Weitere Informationen finden Sie in der Helm-Dokumentation zur [rollenbasierten Zugriffssteuerung](https://helm.sh/docs/using_helm/#helm-and-role-based-access-control).</span><span class="sxs-lookup"><span data-stu-id="ceb99-244">For more information, see [Role-based Access Control](https://helm.sh/docs/using_helm/#helm-and-role-based-access-control) in the Helm documentation.</span></span>

### <a name="revisions"></a><span data-ttu-id="ceb99-245">Revisionen</span><span class="sxs-lookup"><span data-stu-id="ceb99-245">Revisions</span></span>

<span data-ttu-id="ceb99-246">Helm-Charts weisen immer eine Versionsnummer auf, die [semantische Versionierung](https://semver.org/) befolgen muss.</span><span class="sxs-lookup"><span data-stu-id="ceb99-246">Helm charts always have a version number, which must use [semantic versioning](https://semver.org/).</span></span> <span data-ttu-id="ceb99-247">Ein Chart kann außerdem eine `appVersion` aufweisen.</span><span class="sxs-lookup"><span data-stu-id="ceb99-247">A chart can also have an `appVersion`.</span></span> <span data-ttu-id="ceb99-248">Dieses Feld ist optional und muss nicht auf die Chartversion bezogen sein.</span><span class="sxs-lookup"><span data-stu-id="ceb99-248">This field is optional, and doesn't have to be related to the chart version.</span></span> <span data-ttu-id="ceb99-249">Für manche Teams kann es sinnvoll sein, die Version von Anwendungen separat von Chartaktualisierungen zu verwalten.</span><span class="sxs-lookup"><span data-stu-id="ceb99-249">Some teams might want to application versions separately from updates to the charts.</span></span> <span data-ttu-id="ceb99-250">Es ist jedoch einfacher, nur eine Versionsnummer zu verwenden, so dass eine 1:1-Beziehung zwischen der Chartversion und der Anwendungsversion besteht.</span><span class="sxs-lookup"><span data-stu-id="ceb99-250">But a simpler approach is to use one version number, so there's a 1:1 relation between chart version and application version.</span></span> <span data-ttu-id="ceb99-251">Auf diese Weise können Sie ein Chart pro Release speichern und das gewünschte Release komfortabel bereitstellen:</span><span class="sxs-lookup"><span data-stu-id="ceb99-251">That way, you can store one chart per release and easily deploy the desired release:</span></span>

```bash
helm install <package-chart-name> --version <desiredVersion>
```

<span data-ttu-id="ceb99-252">Eine weitere bewährte Methode besteht im Angeben einer Anmerkung für den Änderungsgrund in der Bereitstellungsvorlage:</span><span class="sxs-lookup"><span data-stu-id="ceb99-252">Another good practice is to provide a change-cause annotation in the deployment template:</span></span>

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

<span data-ttu-id="ceb99-253">Auf diese Weise können Sie mithilfe des Befehls `kubectl rollout history` das Feld mit dem Änderungsgrund für jede Revision anzeigen.</span><span class="sxs-lookup"><span data-stu-id="ceb99-253">This lets you view the change-cause field for each revision, using the `kubectl rollout history` command.</span></span> <span data-ttu-id="ceb99-254">Im vorherigen Beispiel wird der Änderungsgrund als Helm-Chartparameter bereitgestellt.</span><span class="sxs-lookup"><span data-stu-id="ceb99-254">In the previous example, the change-cause is provided as a Helm chart parameter.</span></span>

```bash
$ kubectl rollout history deployments/delivery-v010 -n backend
deployment.extensions/delivery-v010
REVISION  CHANGE-CAUSE
1         Initial deployment
```

<span data-ttu-id="ceb99-255">Sie können aber auch den Befehl `helm list` verwenden, um den Revisionsverlauf anzuzeigen:</span><span class="sxs-lookup"><span data-stu-id="ceb99-255">You can also use the `helm list` command to view the revision history:</span></span>

```bash
~$ helm list
NAME            REVISION    UPDATED                     STATUS        CHART            APP VERSION     NAMESPACE
delivery-v0.1.0 1           Sun Apr  7 00:25:30 2019    DEPLOYED      delivery-v0.1.0  v0.1.0          backend
```

> [!TIP]
> <span data-ttu-id="ceb99-256">Verwenden Sie beim Initialisieren von Helm das `--history-max`-Flag.</span><span class="sxs-lookup"><span data-stu-id="ceb99-256">Use the `--history-max` flag when initializing Helm.</span></span> <span data-ttu-id="ceb99-257">Diese Einstellung schränkt die Anzahl der Revisionen ein, die Tiller in seinem Verlauf speichert.</span><span class="sxs-lookup"><span data-stu-id="ceb99-257">This setting limits the number of revisions that Tiller saves in its history.</span></span> <span data-ttu-id="ceb99-258">Tiller speichert den Revisionsverlauf in Konfigurationskarten.</span><span class="sxs-lookup"><span data-stu-id="ceb99-258">Tiller stores revision history in configmaps.</span></span> <span data-ttu-id="ceb99-259">Wenn Sie häufig Aktualisierungen veröffentlichen, können die Konfigurationskarten sehr groß werden, sofern Sie die Verlaufsgröße nicht beschränken.</span><span class="sxs-lookup"><span data-stu-id="ceb99-259">If you're releasing updates frequently, the configmaps can grow very large unless you limit the history size.</span></span>

## <a name="azure-devops-pipeline"></a><span data-ttu-id="ceb99-260">Azure DevOps-Pipeline</span><span class="sxs-lookup"><span data-stu-id="ceb99-260">Azure DevOps Pipeline</span></span>

<span data-ttu-id="ceb99-261">In Azure Pipelines wird zwischen *Buildpipelines* und *Releasepipelines* unterschieden.</span><span class="sxs-lookup"><span data-stu-id="ceb99-261">In Azure Pipelines, pipelines are divided into *build pipelines* and *release pipelines*.</span></span> <span data-ttu-id="ceb99-262">Die Buildpipeline führt den CI-Prozess aus und erstellt Buildartefakte.</span><span class="sxs-lookup"><span data-stu-id="ceb99-262">The build pipeline runs the CI process and creates build artifacts.</span></span> <span data-ttu-id="ceb99-263">Für eine Microservicearchitektur in Kubernetes sind diese Artefakte die Containerimages und Helm-Charts, die jeden Microservice definieren.</span><span class="sxs-lookup"><span data-stu-id="ceb99-263">For a microservices architecture on Kubernetes, these artifacts are the container images and Helm charts that define each microservice.</span></span> <span data-ttu-id="ceb99-264">Die Releasepipeline führt den CD-Prozess aus, der einen Microservice in einem Cluster bereitstellt.</span><span class="sxs-lookup"><span data-stu-id="ceb99-264">The release pipeline runs that CD process that deploys a microservice into a cluster.</span></span>

<span data-ttu-id="ceb99-265">Aufbauend auf dem weiter oben in diesem Artikel beschriebenen CI-Flow, kann eine Buildpipeline aus den folgenden Aufgaben bestehen:</span><span class="sxs-lookup"><span data-stu-id="ceb99-265">Based on the CI flow described earlier in this article, a build pipeline might consist of the following tasks:</span></span>

1. <span data-ttu-id="ceb99-266">Erstellen des Test Runner-Containers.</span><span class="sxs-lookup"><span data-stu-id="ceb99-266">Build the test runner container.</span></span>

    ```yaml
    - task: Docker@1
      inputs:
        azureSubscriptionEndpoint: $(AzureSubscription)
        azureContainerRegistry: $(AzureContainerRegistry)
        arguments: '--pull --target testrunner'
        dockerFile: $(System.DefaultWorkingDirectory)/$(dockerFileName)
        imageName: '$(imageName)-test'
    ```

1. <span data-ttu-id="ceb99-267">Ausführen der Tests durch Aufrufen der Docker-Ausführung für den Test Runner-Container.</span><span class="sxs-lookup"><span data-stu-id="ceb99-267">Run the tests, by invoking docker run against the test runner container.</span></span>

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

1. <span data-ttu-id="ceb99-268">Veröffentlichen der Testergebnisse.</span><span class="sxs-lookup"><span data-stu-id="ceb99-268">Publish the test results.</span></span> <span data-ttu-id="ceb99-269">Weitere Informationen finden Sie unter [Integrate build and test tasks](/azure/devops/pipelines/languages/docker?view=azure-devops&tabs=yaml#integrate-build-and-test-tasks) (Integration von Build- und Testaufgaben).</span><span class="sxs-lookup"><span data-stu-id="ceb99-269">See [Integrate build and test tasks](/azure/devops/pipelines/languages/docker?view=azure-devops&tabs=yaml#integrate-build-and-test-tasks).</span></span>

    ```yaml
    - task: PublishTestResults@2
      inputs:
        testResultsFormat: 'VSTest'
        testResultsFiles: 'TestResults/*.trx'
        searchFolder: '$(System.DefaultWorkingDirectory)'
        publishRunAttachments: true
    ```

1. <span data-ttu-id="ceb99-270">Erstellen Sie den Runtimecontainer.</span><span class="sxs-lookup"><span data-stu-id="ceb99-270">Build the runtime container.</span></span>

    ```yaml
    - task: Docker@1
      inputs:
        azureSubscriptionEndpoint: $(AzureSubscription)
        azureContainerRegistry: $(AzureContainerRegistry)
        dockerFile: $(System.DefaultWorkingDirectory)/$(dockerFileName)
        includeLatestTag: false
        imageName: '$(imageName)'
    ```

1. <span data-ttu-id="ceb99-271">Übertragen Sie per Push aus der Azure-Containerregistrierung (oder einer anderen Containerregistrierung) in den Container.</span><span class="sxs-lookup"><span data-stu-id="ceb99-271">Push to the container to Azure Container Registry (or other container registry).</span></span>

    ```yaml
    - task: Docker@1
      inputs:
        azureSubscriptionEndpoint: $(AzureSubscription)
        azureContainerRegistry: $(AzureContainerRegistry)
        command: 'Push an image'
        imageName: '$(imageName)'
        includeSourceTags: false
    ```

1. <span data-ttu-id="ceb99-272">Verpacken Sie das Helm-Chart.</span><span class="sxs-lookup"><span data-stu-id="ceb99-272">Package the Helm chart.</span></span>

    ```yaml
    - task: HelmDeploy@0
      inputs:
        command: package
        chartPath: $(chartPath)
        chartVersion: $(Build.SourceBranchName)
        arguments: '--app-version $(Build.SourceBranchName)'
    ```

1. <span data-ttu-id="ceb99-273">Übertragen Sie das Helm-Paket in die Azure-Containerregistrierung (oder ein anderes Helm-Repository).</span><span class="sxs-lookup"><span data-stu-id="ceb99-273">Push the Helm package to Azure Container Registry (or other Helm repository).</span></span>

    ```yaml
    task: AzureCLI@1
      inputs:
        azureSubscription: $(AzureSubscription)
        scriptLocation: inlineScript
        inlineScript: |
        az acr helm push $(System.ArtifactsDirectory)/$(repositoryName)-$(Build.SourceBranchName).tgz --name $(AzureContainerRegistry);
    ```

<span data-ttu-id="ceb99-274">&#11162; Sehen Sie dazu die [Quelldatei](https://github.com/mspnp/microservices-reference-implementation/blob/master/src/shipping/delivery/azure-pipelines-ci.yml).</span><span class="sxs-lookup"><span data-stu-id="ceb99-274">&#11162; See the [source file](https://github.com/mspnp/microservices-reference-implementation/blob/master/src/shipping/delivery/azure-pipelines-ci.yml).</span></span>

<span data-ttu-id="ceb99-275">Die Ausgabe der CI-Pipeline ist ein produktionsbereites Containerimage und ein aktualisiertes Helm-Chart für den Microservice.</span><span class="sxs-lookup"><span data-stu-id="ceb99-275">The output from the CI pipeline is a production-ready container image and an updated Helm chart for the microservice.</span></span> <span data-ttu-id="ceb99-276">An diesem Punkt kann die Releasepipeline übernehmen.</span><span class="sxs-lookup"><span data-stu-id="ceb99-276">At this point, the release pipeline can take over.</span></span> <span data-ttu-id="ceb99-277">Sie führt die folgenden Schritte aus:</span><span class="sxs-lookup"><span data-stu-id="ceb99-277">It performs the following steps:</span></span>

- <span data-ttu-id="ceb99-278">Bereitstellen in den DEV-/QA-/Stagingumgebungen.</span><span class="sxs-lookup"><span data-stu-id="ceb99-278">Deploy to dev/QA/staging environments.</span></span>
- <span data-ttu-id="ceb99-279">Warten auf eine genehmigende Person, die die Bereitstellung genehmigt oder ablehnt.</span><span class="sxs-lookup"><span data-stu-id="ceb99-279">Wait for an approver to approve or reject the deployment.</span></span>
- <span data-ttu-id="ceb99-280">Zuordnen eines neuen Tags zum Containerimage für das Release</span><span class="sxs-lookup"><span data-stu-id="ceb99-280">Retag the container image for release</span></span>
- <span data-ttu-id="ceb99-281">Übertragen Sie das Releasetag per Push in die Containerregistrierung.</span><span class="sxs-lookup"><span data-stu-id="ceb99-281">Push the release tag to the container registry.</span></span>
- <span data-ttu-id="ceb99-282">Aktualisieren Sie das Helm-Chart im Produktionscluster.</span><span class="sxs-lookup"><span data-stu-id="ceb99-282">Upgrade the Helm chart in the production cluster.</span></span>

<span data-ttu-id="ceb99-283">Weitere Informationen zum Erstellen einer Releasepipeline finden Sie unter [Release pipelines, draft releases, and release options](/azure/devops/pipelines/release/?view=azure-devops) (Releasepipelines, Entwurfsreleases und Releaseoptionen).</span><span class="sxs-lookup"><span data-stu-id="ceb99-283">For more information about creating a release pipeline, see [Release pipelines, draft releases, and release options](/azure/devops/pipelines/release/?view=azure-devops).</span></span>

<span data-ttu-id="ceb99-284">Im folgenden Diagramm ist der in diesem Artikel beschriebene End-to-End-CI/CD--Prozess dargestellt:</span><span class="sxs-lookup"><span data-stu-id="ceb99-284">The following diagram shows the end-to-end CI/CD process described in this article:</span></span>

![CI/CD-Pipeline](./images/aks-cicd-flow.png)

## <a name="next-steps"></a><span data-ttu-id="ceb99-286">Nächste Schritte</span><span class="sxs-lookup"><span data-stu-id="ceb99-286">Next steps</span></span>

<span data-ttu-id="ceb99-287">Dieser Artikel basiert auf einer Referenzimplementierung, die Sie auf [GitHub][ri] finden können.</span><span class="sxs-lookup"><span data-stu-id="ceb99-287">This article was based on a reference implementation that you can find on [GitHub][ri].</span></span>

[ri]: https://github.com/mspnp/microservices-reference-implementation