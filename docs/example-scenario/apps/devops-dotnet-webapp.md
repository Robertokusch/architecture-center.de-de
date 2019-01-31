---
title: Entwerfen einer CI/CD-Pipeline mithilfe von Azure DevOps
titleSuffix: Azure Example Scenarios
description: Erstellen und Veröffentlichen Sie mithilfe von Azure DevOps eine .NET-App in Azure-Web-Apps.
author: christianreddington
ms.date: 12/06/2018
ms.topic: example-scenario
ms.service: architecture-center
ms.subservice: example-scenario
ms.custom: fasttrack, seodec18
social_image_url: /azure/architecture/example-scenario/apps/media/architecture-devops-dotnet-webapp.svg
ms.openlocfilehash: 0d6ac13fb357657a2ec5e6284abadb46d6926907
ms.sourcegitcommit: 3b15d65e7c35a19506e562c444343f8467b6a073
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/25/2019
ms.locfileid: "54908493"
---
# <a name="design-a-cicd-pipeline-using-azure-devops"></a><span data-ttu-id="d2a87-103">Entwerfen einer CI/CD-Pipeline mithilfe von Azure DevOps</span><span class="sxs-lookup"><span data-stu-id="d2a87-103">Design a CI/CD pipeline using Azure DevOps</span></span>

<span data-ttu-id="d2a87-104">Dieses Szenario fungiert als Architektur- und Entwurfsleitfaden für die Erstellung einer CI/CD-Pipeline (Continuous Integration/Continuous Deployment).</span><span class="sxs-lookup"><span data-stu-id="d2a87-104">This scenario provides architecture and design guidance for building a continuous integration (CI) and continuous deployment (CD) pipeline.</span></span> <span data-ttu-id="d2a87-105">In diesem Beispiel stellt die CI/CD-Pipeline eine .NET-Webanwendung mit zwei Ebenen in Azure App Service bereit.</span><span class="sxs-lookup"><span data-stu-id="d2a87-105">In this example, the CI/CD pipeline deploys a two-tier .NET web application to the Azure App Service.</span></span>

<span data-ttu-id="d2a87-106">Die Migration zu modernen CI/CD-Prozessen bietet zahlreiche Vorteile für Build-, Bereitstellungs-, Test- und Überwachungsvorgänge für Anwendungen.</span><span class="sxs-lookup"><span data-stu-id="d2a87-106">Migrating to modern CI/CD processes provides many benefits for application builds, deployments, testing, and monitoring.</span></span> <span data-ttu-id="d2a87-107">Durch die Verwendung von Azure DevOps mit anderen Diensten wie etwa App Service müssen sich Organisationen nicht um die Verwaltung der unterstützenden Infrastruktur kümmern und können sich stattdessen ganz auf die App-Entwicklung konzentrieren.</span><span class="sxs-lookup"><span data-stu-id="d2a87-107">By utilizing Azure DevOps along with other services such as App Service, organizations can focus on the development of their apps rather than the management of the supporting infrastructure.</span></span>

## <a name="relevant-use-cases"></a><span data-ttu-id="d2a87-108">Relevante Anwendungsfälle</span><span class="sxs-lookup"><span data-stu-id="d2a87-108">Relevant use cases</span></span>

<span data-ttu-id="d2a87-109">Azure DevOps und CI/CD-Prozesse eignen sich für Folgendes:</span><span class="sxs-lookup"><span data-stu-id="d2a87-109">Consider Azure DevOps and CI/CD processes for:</span></span>

- <span data-ttu-id="d2a87-110">Beschleunigen der Anwendungsentwicklung und Entwicklungslebenszyklen</span><span class="sxs-lookup"><span data-stu-id="d2a87-110">Accelerating application development and development life cycles</span></span>
- <span data-ttu-id="d2a87-111">Sicherstellen der Qualität und Konsistenz für den automatisierten Build- und Freigabeprozess</span><span class="sxs-lookup"><span data-stu-id="d2a87-111">Building quality and consistency into an automated build and release process</span></span>
- <span data-ttu-id="d2a87-112">Steigern von Anwendungsstabilität und -betriebszeit</span><span class="sxs-lookup"><span data-stu-id="d2a87-112">Increasing application stability and uptime</span></span>

## <a name="architecture"></a><span data-ttu-id="d2a87-113">Architecture</span><span class="sxs-lookup"><span data-stu-id="d2a87-113">Architecture</span></span>

![Architekturdiagramm mit den Azure-Komponenten in einem DevOps-Szenario mit Azure DevOps und Azure App Service][architecture]

<span data-ttu-id="d2a87-115">Die Daten durchlaufen das Szenario wie folgt:</span><span class="sxs-lookup"><span data-stu-id="d2a87-115">The data flows through the scenario as follows:</span></span>

1. <span data-ttu-id="d2a87-116">Ein Entwickler ändert den Quellcode der Anwendung.</span><span class="sxs-lookup"><span data-stu-id="d2a87-116">A developer changes application source code.</span></span>
2. <span data-ttu-id="d2a87-117">Der Anwendungscode wird im Quellcoderepository in Azure Repos committet (einschließlich der Datei „web.config“).</span><span class="sxs-lookup"><span data-stu-id="d2a87-117">Application code including the web.config file is committed to the source code repository in Azure Repos.</span></span>
3. <span data-ttu-id="d2a87-118">Continuous Integration löst den Anwendungsbuildvorgang und Komponententests mit Azure Test Plans aus.</span><span class="sxs-lookup"><span data-stu-id="d2a87-118">Continuous integration triggers application build and unit tests using Azure Test Plans.</span></span>
4. <span data-ttu-id="d2a87-119">Continuous Deployment in Azure Pipelines löst eine automatisierte Bereitstellung von Anwendungsartefakten *mit umgebungsspezifischen Konfigurationswerten* aus.</span><span class="sxs-lookup"><span data-stu-id="d2a87-119">Continuous deployment within Azure Pipelines triggers an automated deployment of application artifacts *with environment-specific configuration values*.</span></span>
5. <span data-ttu-id="d2a87-120">Die Artefakte werden in Azure App Service bereitgestellt.</span><span class="sxs-lookup"><span data-stu-id="d2a87-120">The artifacts are deployed to Azure App Service.</span></span>
6. <span data-ttu-id="d2a87-121">Azure Application Insights sammelt und analysiert Integritäts-, Leistungs- und Nutzungsdaten.</span><span class="sxs-lookup"><span data-stu-id="d2a87-121">Azure Application Insights collects and analyzes health, performance, and usage data.</span></span>
7. <span data-ttu-id="d2a87-122">Entwickler überwachen und verwalten Integritäts-, Leistungs- und Nutzungsinformationen.</span><span class="sxs-lookup"><span data-stu-id="d2a87-122">Developers monitor and mange health, performance, and usage information.</span></span>
8. <span data-ttu-id="d2a87-123">Neue Features und Fehlerbehebungen werden unter Verwendung von Azure Boards auf der Grundlage von Backloginformationen priorisiert.</span><span class="sxs-lookup"><span data-stu-id="d2a87-123">Backlog information is used to prioritize new features and bug fixes using Azure Boards.</span></span>

### <a name="components"></a><span data-ttu-id="d2a87-124">Komponenten</span><span class="sxs-lookup"><span data-stu-id="d2a87-124">Components</span></span>

- <span data-ttu-id="d2a87-125">[Azure DevOps][vsts] ist ein Dienst für die umfassende und nahtlose Verwaltung Ihres Entwicklungslebenszyklus – von der Planung und dem Projektmanagement über die Codeverwaltung bis hin zum Build- und Releasevorgang.</span><span class="sxs-lookup"><span data-stu-id="d2a87-125">[Azure DevOps][vsts] is a service for managing your development life cycle end-to-end &mdash; from planning and project management, to code management, and continuing to build and release.</span></span>

- <span data-ttu-id="d2a87-126">[Azure-Web-Apps][web-apps] ist ein PaaS-Dienst zum Hosten von Webanwendungen, REST-APIs und mobilen Back-Ends.</span><span class="sxs-lookup"><span data-stu-id="d2a87-126">[Azure Web Apps][web-apps] is a PaaS service for hosting web applications, REST APIs, and mobile back ends.</span></span> <span data-ttu-id="d2a87-127">In diesem Artikel geht es zwar um .NET, aber es werden auch verschiedene andere Optionen für Entwicklungsplattformen unterstützt.</span><span class="sxs-lookup"><span data-stu-id="d2a87-127">While this article focuses on .NET, there are several additional development platform options supported.</span></span>

- <span data-ttu-id="d2a87-128">[Application Insights][application-insights] ist ein erweiterbarer, für Webentwickler konzipierter Erstanbieterdienst zur Verwaltung der Anwendungsleistung (Application Performance Management, APM) auf mehreren Plattformen.</span><span class="sxs-lookup"><span data-stu-id="d2a87-128">[Application Insights][application-insights] is a first-party, extensible Application Performance Management (APM) service for web developers on multiple platforms.</span></span>

### <a name="alternatives"></a><span data-ttu-id="d2a87-129">Alternativen</span><span class="sxs-lookup"><span data-stu-id="d2a87-129">Alternatives</span></span>

<span data-ttu-id="d2a87-130">In diesem Artikel steht zwar Azure DevOps im Mittelpunkt, Sie können aber auch [Azure DevOps Server][azure-devops-server] (ehemals Team Foundation Server) als lokale Alternative nutzen.</span><span class="sxs-lookup"><span data-stu-id="d2a87-130">While this article focuses on Azure DevOps, [Azure DevOps Server][azure-devops-server] (previously known as Team Foundation Server) could be used as an on-premises substitute.</span></span> <span data-ttu-id="d2a87-131">Darüber hinaus steht eine Reihe von Technologien zur Verfügung, mit denen Sie eine Open-Source-Entwicklungspipeline mit [Jenkins][jenkins-on-azure] erstellen können.</span><span class="sxs-lookup"><span data-stu-id="d2a87-131">Alternatively, you could also use a set of technologies for an open-source development pipeline using [Jenkins][jenkins-on-azure].</span></span>

<span data-ttu-id="d2a87-132">Aus Sicht von „Infrastructure-as-Code“ wurden [Resource Manager-Vorlagen][arm-templates] im Rahmen des Azure DevOps-Projekts verwendet. Sie können aber auch andere Verwaltungstechnologien wie etwa [Terraform][terraform] oder [Chef][chef] verwenden.</span><span class="sxs-lookup"><span data-stu-id="d2a87-132">From an infrastructure-as-code perspective, [Resource Manager templates][arm-templates] were used as part of the Azure DevOps project, but you could consider other management technologies such as [Terraform][terraform] or [Chef][chef].</span></span> <span data-ttu-id="d2a87-133">Wenn Sie eine IaaS-basierte Bereitstellung (Infrastructure-as-a-Service) vorziehen und eine Konfigurationsverwaltung benötigen, können Sie [Azure Automation State Configuration][desired-state-configuration], [Ansible][ansible] oder [Chef][chef] verwenden.</span><span class="sxs-lookup"><span data-stu-id="d2a87-133">If you prefer an infrastructure-as-a-service (IaaS)-based deployment and require configuration management, you could consider either [Azure Automation State Configuration][desired-state-configuration], [Ansible][ansible], or [Chef][chef].</span></span>

<span data-ttu-id="d2a87-134">Erwägen Sie ggf. die folgenden Alternativen zum Hosten in Azure-Web-Apps :</span><span class="sxs-lookup"><span data-stu-id="d2a87-134">You could consider these alternatives to hosting in Azure Web Apps:</span></span>

- <span data-ttu-id="d2a87-135">[Microsoft Azure Virtual Machines:][compare-vm-hosting] Für Workloads, die ein hohes Maß an Kontrolle erfordern oder von Betriebssystemkomponenten und -diensten abhängig sind, die mit Web-Apps nicht genutzt werden können (etwa der globale Assemblycache von Windows oder COM).</span><span class="sxs-lookup"><span data-stu-id="d2a87-135">[Azure Virtual Machines][compare-vm-hosting] handles workloads that require a high degree of control, or depend on OS components and services that are not possible with Web Apps (for example, the Windows GAC, or COM).</span></span>

- <span data-ttu-id="d2a87-136">[Microsoft Azure Service Fabric:][service-fabric] Eine gute Option, wenn die Workloadarchitektur hauptsächlich verteilte Komponenten umfasst, die von der Bereitstellung und Ausführung in einem stark kontrollierten Cluster profitieren.</span><span class="sxs-lookup"><span data-stu-id="d2a87-136">[Service Fabric][service-fabric] is a good option if the workload architecture is focused around distributed components that benefit from being deployed and run across a cluster with a high degree of control.</span></span> <span data-ttu-id="d2a87-137">Service Fabric kann auch zum Hosten von Containern verwendet werden.</span><span class="sxs-lookup"><span data-stu-id="d2a87-137">Service Fabric can also be used to host containers.</span></span>

- <span data-ttu-id="d2a87-138">[Azure Functions:][azure-functions] Ein effektiver serverloser Ansatz, wenn die Workloadarchitektur auf abgestimmten verteilten Komponenten mit minimalen Abhängigkeiten basiert, einzelne Komponenten nur bedarfsabhängig (also nicht ständig) ausgeführt werden müssen und keine Orchestrierung von Komponenten erforderlich ist.</span><span class="sxs-lookup"><span data-stu-id="d2a87-138">[Azure Functions][azure-functions] provides an effective serverless approach if the workload architecture is centered around fine grained distributed components, requiring minimal dependencies, where individual components are only required to run on demand (not continuously) and orchestration of components is not required.</span></span>

<span data-ttu-id="d2a87-139">Diese [Entscheidungsstruktur für Azure-Computedienste](/azure/architecture/guide/technology-choices/compute-decision-tree) kann bei der Wahl des richtigen Migrationspfads hilfreich sein.</span><span class="sxs-lookup"><span data-stu-id="d2a87-139">This [decision tree for Azure compute services](/azure/architecture/guide/technology-choices/compute-decision-tree) may help when choosing the right path to take for a migration.</span></span>

## <a name="management-and-security-considerations"></a><span data-ttu-id="d2a87-140">Verwaltungs- und Sicherheitsaspekte</span><span class="sxs-lookup"><span data-stu-id="d2a87-140">Management and Security Considerations</span></span>

- <span data-ttu-id="d2a87-141">Erwägen Sie die Nutzung einer der [Tokenisierungsaufgaben][vsts-tokenization], die auf dem VSTS-Marketplace verfügbar sind.</span><span class="sxs-lookup"><span data-stu-id="d2a87-141">Consider leveraging one of the [tokenization tasks][vsts-tokenization] available in the VSTS marketplace.</span></span>

- <span data-ttu-id="d2a87-142">[Azure Key Vault][download-keyvault-secrets]-Aufgaben können Geheimnisse aus einer Azure Key Vault-Instanz in Ihr Release herunterladen.</span><span class="sxs-lookup"><span data-stu-id="d2a87-142">[Azure Key Vault][download-keyvault-secrets] tasks can download secrets from an Azure Key Vault into your release.</span></span> <span data-ttu-id="d2a87-143">Diese Geheimnisse können dann als Variablen in Ihrer Releasedefinition verwendet werden, um die Speicherung in der Quellcodeverwaltung zu vermeiden.</span><span class="sxs-lookup"><span data-stu-id="d2a87-143">You can then use those secrets as variables in your release definition, which avoids storing them in source control.</span></span>

- <span data-ttu-id="d2a87-144">Verwenden Sie in Ihren Releasedefinitionen [Releasevariablen][vsts-release-variables], um Konfigurationsänderungen Ihrer Umgebungen zu steuern.</span><span class="sxs-lookup"><span data-stu-id="d2a87-144">Use [release variables][vsts-release-variables] in your release definitions to drive configuration changes of your environments.</span></span> <span data-ttu-id="d2a87-145">Releasevariablen können für eine gesamte Freigabe oder eine bestimmte Umgebung gelten.</span><span class="sxs-lookup"><span data-stu-id="d2a87-145">Release variables can be scoped to an entire release or a given environment.</span></span> <span data-ttu-id="d2a87-146">Denken Sie daran, auf das Schlosssymbol zu klicken, wenn Sie Variablen für Geheimnisinformationen verwenden.</span><span class="sxs-lookup"><span data-stu-id="d2a87-146">When using variables for secret information, ensure that you select the padlock icon.</span></span>

- <span data-ttu-id="d2a87-147">In Ihrer Releasepipeline sollten [Bereitstellungsgates][vsts-deployment-gates] verwendet werden.</span><span class="sxs-lookup"><span data-stu-id="d2a87-147">[Deployment gates][vsts-deployment-gates] should be used in your release pipeline.</span></span> <span data-ttu-id="d2a87-148">Auf diese Weise können Sie Überwachungsdaten in Verbindung mit externen Systemen (etwa Incident Management oder zusätzliche individuelle Systeme) nutzen, um festzustellen, ob ein Release höher gestuft werden sollte.</span><span class="sxs-lookup"><span data-stu-id="d2a87-148">This lets you leverage monitoring data in association with external systems (for example, incident management or additional bespoke systems) to determine whether a release should be promoted.</span></span>

- <span data-ttu-id="d2a87-149">Sollte für eine Releasepipeline ein manueller Eingriff erforderlich sein, verwenden Sie die [Genehmigungsfunktion][vsts-approvals].</span><span class="sxs-lookup"><span data-stu-id="d2a87-149">Where manual intervention in a release pipeline is required, use the [approvals][vsts-approvals] functionality.</span></span>

- <span data-ttu-id="d2a87-150">Es ist ratsam, [Application Insights][application-insights] und weitere Überwachungstools so früh wie möglich in der Releasepipeline einzusetzen.</span><span class="sxs-lookup"><span data-stu-id="d2a87-150">Consider using [Application Insights][application-insights] and additional monitoring tools as early as possible in your release pipeline.</span></span> <span data-ttu-id="d2a87-151">Viele Organisationen beginnen erst in der Produktionsumgebung mit der Überwachung.</span><span class="sxs-lookup"><span data-stu-id="d2a87-151">Many organizations only begin monitoring in their production environment.</span></span> <span data-ttu-id="d2a87-152">Durch die Überwachung Ihrer anderen Umgebungen lassen sich Fehler früher im Entwicklungsprozess erkennen und Probleme in der Produktionsumgebung vermeiden.</span><span class="sxs-lookup"><span data-stu-id="d2a87-152">By monitoring your other environments, you can identify bugs earlier in the development process and avoid issues in your production environment.</span></span>

## <a name="deploy-the-scenario"></a><span data-ttu-id="d2a87-153">Bereitstellen des Szenarios</span><span class="sxs-lookup"><span data-stu-id="d2a87-153">Deploy the scenario</span></span>

### <a name="prerequisites"></a><span data-ttu-id="d2a87-154">Voraussetzungen</span><span class="sxs-lookup"><span data-stu-id="d2a87-154">Prerequisites</span></span>

- <span data-ttu-id="d2a87-155">Sie benötigen ein bestehendes Azure-Konto.</span><span class="sxs-lookup"><span data-stu-id="d2a87-155">You must have an existing Azure account.</span></span> <span data-ttu-id="d2a87-156">Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) erstellen, bevor Sie beginnen.</span><span class="sxs-lookup"><span data-stu-id="d2a87-156">If you don't have an Azure subscription, create a [free account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) before you begin.</span></span>

- <span data-ttu-id="d2a87-157">Zuerst müssen Sie sich für eine Azure DevOps-Organisation registrieren.</span><span class="sxs-lookup"><span data-stu-id="d2a87-157">You must sign up for an Azure DevOps organization.</span></span> <span data-ttu-id="d2a87-158">Weitere Informationen finden Sie unter [Quickstart: Create an organization][vsts-account-create] (Schnellstart: Erstellen einer Organisation).</span><span class="sxs-lookup"><span data-stu-id="d2a87-158">For more information, see [Quickstart: Create your organization][vsts-account-create].</span></span>

### <a name="walk-through"></a><span data-ttu-id="d2a87-159">Exemplarische Vorgehensweise</span><span class="sxs-lookup"><span data-stu-id="d2a87-159">Walk-through</span></span>

<span data-ttu-id="d2a87-160">Über das [Azure DevOps-Projekt](/azure/devops-project/azure-devops-project-github) werden ein App Service-Plan, eine App Service-Instanz und eine App Insights-Ressource bereitgestellt, und das Azure DevOps-Projekt wird für Sie konfiguriert.</span><span class="sxs-lookup"><span data-stu-id="d2a87-160">The [Azure DevOps project](/azure/devops-project/azure-devops-project-github) will deploy an App Service Plan, App Service, and an App Insights resource for you, as well as configure the Azure DevOps project for you.</span></span>

<span data-ttu-id="d2a87-161">Nachdem Sie das Azure DevOps-Projekt bereitgestellt haben und der Buildvorgang abgeschlossen ist, können Sie sich die entsprechenden Codeänderungen, Arbeitselemente und Testergebnisse ansehen.</span><span class="sxs-lookup"><span data-stu-id="d2a87-161">Once you've deployed the Azure DevOps project and the build is completed, review the associated code changes, work items, and test results.</span></span> <span data-ttu-id="d2a87-162">Sie werden bemerken, dass keine Testergebnisse angezeigt werden, da der Code keine auszuführenden Tests enthält.</span><span class="sxs-lookup"><span data-stu-id="d2a87-162">You will notice that no test results are displayed, because the code does not contain any tests to run.</span></span>

<span data-ttu-id="d2a87-163">Das Projekt erstellt eine Releasepipeline und einen Continuous Deployment-Trigger für die Bereitstellung der Anwendung in der Entwicklungsumgebung.</span><span class="sxs-lookup"><span data-stu-id="d2a87-163">The project creates a release pipeline and continuous deployment trigger, deploying our application into the Dev environment.</span></span> <span data-ttu-id="d2a87-164">Im Rahmen eines Continuous Deployment-Prozesses können Releases für mehrere Umgebungen gelten.</span><span class="sxs-lookup"><span data-stu-id="d2a87-164">As part of a continuous deployment process, you may see releases that span multiple environments.</span></span> <span data-ttu-id="d2a87-165">Ein Release kann sowohl die Infrastruktur (mit Verfahren wie „Infrastructure-as-Code“) als auch die Bereitstellung der erforderlichen Anwendungspakete mit Aufgaben nach der Konfiguration umfassen.</span><span class="sxs-lookup"><span data-stu-id="d2a87-165">A release can span both infrastructure (using techniques such as infrastructure-as-code), and can also deploy the application packages required along with any post-configuration tasks.</span></span>

## <a name="pricing"></a><span data-ttu-id="d2a87-166">Preise</span><span class="sxs-lookup"><span data-stu-id="d2a87-166">Pricing</span></span>

<span data-ttu-id="d2a87-167">Die Kosten für Azure DevOps richten sich nach der Anzahl von Benutzern in Ihrer Organisation, die Zugriff benötigen, und anderen Faktoren wie der Anzahl von erforderlichen gleichzeitigen Builds/Releases sowie der Anzahl von Testbenutzern.</span><span class="sxs-lookup"><span data-stu-id="d2a87-167">Azure DevOps costs depend on the number of users in your organization that require access, along with other factors like the number of concurrent build/releases required and number of test users.</span></span> <span data-ttu-id="d2a87-168">Weitere Informationen finden Sie in der [Azure DevOps-Preisübersicht][vsts-pricing-page].</span><span class="sxs-lookup"><span data-stu-id="d2a87-168">For more information, see [Azure DevOps pricing][vsts-pricing-page].</span></span>

<span data-ttu-id="d2a87-169">Der [Preisrechner][vsts-pricing-calculator] liefert eine Kostenschätzung für den Betrieb von Azure DevOps mit 20 Benutzern.</span><span class="sxs-lookup"><span data-stu-id="d2a87-169">This [pricing calculator][vsts-pricing-calculator] provides an estimate for running Azure DevOps with 20 users.</span></span>

<span data-ttu-id="d2a87-170">Azure DevOps wird monatlich nach Benutzern abgerechnet.</span><span class="sxs-lookup"><span data-stu-id="d2a87-170">Azure DevOps is billed on a per-user per-month basis.</span></span> <span data-ttu-id="d2a87-171">Es können weitere Gebühren anfallen, die sich nach den erforderlichen gleichzeitigen Pipelines und den zusätzlichen Testbenutzern bzw. Basic-Lizenzen für Benutzer richten.</span><span class="sxs-lookup"><span data-stu-id="d2a87-171">There may be additional charges dependent upon concurrent pipelines needed, in addition to any additional test users or user basic licenses.</span></span>

## <a name="related-resources"></a><span data-ttu-id="d2a87-172">Zugehörige Ressourcen</span><span class="sxs-lookup"><span data-stu-id="d2a87-172">Related resources</span></span>

<span data-ttu-id="d2a87-173">Die folgenden Ressourcen enthalten weitere Informationen zu CI/CD und Azure DevOps:</span><span class="sxs-lookup"><span data-stu-id="d2a87-173">Review the following resources to learn more about CI/CD and Azure DevOps:</span></span>

- <span data-ttu-id="d2a87-174">[Was ist DevOps?][devops-whatis]</span><span class="sxs-lookup"><span data-stu-id="d2a87-174">[What is DevOps?][devops-whatis]</span></span>
- <span data-ttu-id="d2a87-175">[DevOps at Microsoft – How we work with Azure DevOps][devops-microsoft] (DevOps bei Microsoft – Arbeiten mit Azure DevOps)</span><span class="sxs-lookup"><span data-stu-id="d2a87-175">[DevOps at Microsoft - How we work with Azure DevOps][devops-microsoft]</span></span>
- <span data-ttu-id="d2a87-176">[Schritt-für-Schritt-Tutorials: DevOps mit Azure DevOps][devops-with-vsts]</span><span class="sxs-lookup"><span data-stu-id="d2a87-176">[Step-by-step Tutorials: DevOps with Azure DevOps][devops-with-vsts]</span></span>
- <span data-ttu-id="d2a87-177">[Checkliste für DevOps][devops-checklist]</span><span class="sxs-lookup"><span data-stu-id="d2a87-177">[Devops Checklist][devops-checklist]</span></span>
- <span data-ttu-id="d2a87-178">[Erstellen einer CI/CD-Pipeline für .NET mit dem Azure DevOps-Projekt][devops-project-create]</span><span class="sxs-lookup"><span data-stu-id="d2a87-178">[Create a CI/CD pipeline for .NET with the Azure DevOps project][devops-project-create]</span></span>

<!-- links -->

[ansible]: /azure/ansible/
[application-insights]: /azure/application-insights/app-insights-overview
[app-service-reference-architecture]: ../../reference-architectures/app-service-web-app/basic-web-app.md
[arm-templates]: /azure/azure-resource-manager/resource-group-overview#template-deployment
[architecture]: ./media/architecture-devops-dotnet-webapp.svg
[chef]: /azure/chef/
[design-patterns-availability]: /azure/architecture/patterns/category/availability
[design-patterns-resiliency]: /azure/architecture/patterns/category/resiliency
[design-patterns-scalability]: /azure/architecture/patterns/category/performance-scalability
[design-patterns-security]: /azure/architecture/patterns/category/security
[desired-state-configuration]: /azure/automation/automation-dsc-overview
[devops-microsoft]: /azure/devops/devops-at-microsoft/
[devops-with-vsts]: https://almvm.azurewebsites.net/labs/vsts/
[devops-checklist]: /azure/architecture/checklist/dev-ops
[application-insights]: https://azure.microsoft.com/services/application-insights/
[cloud-based-load-testing]: https://visualstudio.microsoft.com/team-services/cloud-load-testing/
[cloud-based-load-testing-on-premises]: /vsts/test/load-test/clt-with-private-machines?view=vsts
[jenkins-on-azure]: /azure/jenkins/
[devops-whatis]: /azure/devops/what-is-devops
[download-keyvault-secrets]: /vsts/pipelines/tasks/deploy/azure-key-vault?view=vsts
[resource-groups]: /azure/azure-resource-manager/resource-group-overview
[resiliency-app-service]: /azure/architecture/checklist/resiliency-per-service#app-service
[vsts]: /vsts/?view=vsts#pivot=services
[continuous-integration]: /azure/devops/what-is-continuous-integration
[continuous-delivery]: /azure/devops/what-is-continuous-delivery
[web-apps]: /azure/app-service/app-service-web-overview
[vsts-account-create]: /azure/devops/organizations/accounts/create-organization-msa-or-work-student?view=vsts
[vsts-approvals]: /vsts/pipelines/release/approvals/approvals?view=vsts
[devops-project]: https://portal.azure.com/?feature.customportal=false#create/Microsoft.AzureProject
[vsts-deployment-gates]: /vsts/pipelines/release/approvals/gates?view=vsts
[vsts-pricing-calculator]: https://azure.com/e/498aa024454445a8a352e75724f900b1
[vsts-pricing-page]: https://azure.microsoft.com/pricing/details/visual-studio-team-services/
[vsts-release-variables]: /vsts/pipelines/release/variables?view=vsts&tabs=batch
[vsts-tokenization]: https://marketplace.visualstudio.com/search?term=token&target=VSTS&category=All%20categories&sortBy=Relevance
[azure-key-vault]: /azure/key-vault/key-vault-overview
[infra-as-code]: https://blogs.msdn.microsoft.com/mvpawardprogram/2018/02/13/infrastructure-as-code/
[azure-devops-server]: https://visualstudio.microsoft.com/tfs/
[infra-as-code]: https://blogs.msdn.microsoft.com/mvpawardprogram/2018/02/13/infrastructure-as-code/
[service-fabric]: /azure/service-fabric/
[azure-functions]: /azure/azure-functions/
[azure-containers]: https://azure.microsoft.com/overview/containers/
[compare-vm-hosting]: /azure/app-service/choose-web-site-cloud-service-vm
[app-insights-cd-monitoring]: /azure/application-insights/app-insights-vsts-continuous-monitoring
[azure-region-pair-bcdr]: /azure/best-practices-availability-paired-regions
[devops-project-create]: /azure/devops-project/azure-devops-project-aspnet-core
[terraform]: /azure/terraform/
