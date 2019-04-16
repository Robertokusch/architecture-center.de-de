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
ms.openlocfilehash: f999b2ffdf234161f668887d5b2327ecf50f0e55
ms.sourcegitcommit: bb75a25bd589a761c79e39f2ccdec4acc7d71d60
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/11/2019
ms.locfileid: "59480132"
---
# <a name="design-a-cicd-pipeline-using-azure-devops"></a>Entwerfen einer CI/CD-Pipeline mithilfe von Azure DevOps

Dieses Szenario fungiert als Architektur- und Entwurfsleitfaden für die Erstellung einer CI/CD-Pipeline (Continuous Integration/Continuous Deployment). In diesem Beispiel stellt die CI/CD-Pipeline eine .NET-Webanwendung mit zwei Ebenen in Azure App Service bereit.

Die Migration zu modernen CI/CD-Prozessen bietet zahlreiche Vorteile für Build-, Bereitstellungs-, Test- und Überwachungsvorgänge für Anwendungen. Durch die Verwendung von Azure DevOps mit anderen Diensten wie etwa App Service müssen sich Organisationen nicht um die Verwaltung der unterstützenden Infrastruktur kümmern und können sich stattdessen ganz auf die App-Entwicklung konzentrieren.

## <a name="relevant-use-cases"></a>Relevante Anwendungsfälle

Azure DevOps und CI/CD-Prozesse eignen sich für Folgendes:

- Beschleunigen der Anwendungsentwicklung und Entwicklungslebenszyklen
- Sicherstellen der Qualität und Konsistenz für den automatisierten Build- und Freigabeprozess
- Steigern von Anwendungsstabilität und -betriebszeit

## <a name="architecture"></a>Architecture

![Architekturdiagramm mit den Azure-Komponenten in einem DevOps-Szenario mit Azure DevOps und Azure App Service][architecture]

Die Daten durchlaufen das Szenario wie folgt:

1. Ein Entwickler ändert den Quellcode der Anwendung.
2. Der Anwendungscode wird im Quellcoderepository in Azure Repos committet (einschließlich der Datei „web.config“).
3. Continuous Integration löst den Anwendungsbuildvorgang und Komponententests mit Azure Test Plans aus.
4. Continuous Deployment in Azure Pipelines löst eine automatisierte Bereitstellung von Anwendungsartefakten *mit umgebungsspezifischen Konfigurationswerten* aus.
5. Die Artefakte werden in Azure App Service bereitgestellt.
6. Azure Application Insights sammelt und analysiert Integritäts-, Leistungs- und Nutzungsdaten.
7. Entwickler überwachen und verwalten Integritäts-, Leistungs- und Nutzungsinformationen.
8. Neue Features und Fehlerbehebungen werden unter Verwendung von Azure Boards auf der Grundlage von Backloginformationen priorisiert.

### <a name="components"></a>Komponenten

- [Azure DevOps][vsts] ist ein Dienst für die umfassende und nahtlose Verwaltung Ihres Entwicklungslebenszyklus – von der Planung und dem Projektmanagement über die Codeverwaltung bis hin zum Build- und Releasevorgang.

- [Azure-Web-Apps][web-apps] ist ein PaaS-Dienst zum Hosten von Webanwendungen, REST-APIs und mobilen Back-Ends. In diesem Artikel geht es zwar um .NET, aber es werden auch verschiedene andere Optionen für Entwicklungsplattformen unterstützt.

- [Application Insights][application-insights] ist ein erweiterbarer, für Webentwickler konzipierter Erstanbieterdienst zur Verwaltung der Anwendungsleistung (Application Performance Management, APM) auf mehreren Plattformen.

### <a name="alternatives"></a>Alternativen

In diesem Artikel steht zwar Azure DevOps im Mittelpunkt, Sie können aber auch [Azure DevOps Server][azure-devops-server] (ehemals Team Foundation Server) als lokale Alternative nutzen. Darüber hinaus steht eine Reihe von Technologien zur Verfügung, mit denen Sie eine Open-Source-Entwicklungspipeline mit [Jenkins][jenkins-on-azure] erstellen können.

Aus Sicht von „Infrastructure-as-Code“ wurden [Resource Manager-Vorlagen][arm-templates] im Rahmen des Azure DevOps-Projekts verwendet. Sie können aber auch andere Verwaltungstechnologien wie etwa [Terraform][terraform] oder [Chef][chef] verwenden. Wenn Sie eine IaaS-basierte Bereitstellung (Infrastructure-as-a-Service) vorziehen und eine Konfigurationsverwaltung benötigen, können Sie [Azure Automation State Configuration][desired-state-configuration], [Ansible][ansible] oder [Chef][chef] verwenden.

Erwägen Sie ggf. die folgenden Alternativen zum Hosten in Azure-Web-Apps :

- [Microsoft Azure Virtual Machines:][compare-vm-hosting] Für Workloads, die ein hohes Maß an Kontrolle erfordern oder von Betriebssystemkomponenten und -diensten abhängig sind, die mit Web-Apps nicht genutzt werden können (etwa der globale Assemblycache von Windows oder COM).

- [Microsoft Azure Service Fabric:][service-fabric] Eine gute Option, wenn die Workloadarchitektur hauptsächlich verteilte Komponenten umfasst, die von der Bereitstellung und Ausführung in einem stark kontrollierten Cluster profitieren. Service Fabric kann auch zum Hosten von Containern verwendet werden.

- [Azure Functions:][azure-functions] Ein effektiver serverloser Ansatz, wenn die Workloadarchitektur auf abgestimmten verteilten Komponenten mit minimalen Abhängigkeiten basiert, einzelne Komponenten nur bedarfsabhängig (also nicht ständig) ausgeführt werden müssen und keine Orchestrierung von Komponenten erforderlich ist.

Diese [Entscheidungsstruktur für Azure-Computedienste](/azure/architecture/guide/technology-choices/compute-decision-tree) kann bei der Wahl des richtigen Migrationspfads hilfreich sein.

## <a name="management-and-security-considerations"></a>Verwaltungs- und Sicherheitsaspekte

- Erwägen Sie die Nutzung einer der [Tokenisierungsaufgaben][vsts-tokenization], die auf dem VSTS-Marketplace verfügbar sind.

- [Azure Key Vault][download-keyvault-secrets]-Aufgaben können Geheimnisse aus einer Azure Key Vault-Instanz in Ihr Release herunterladen. Diese Geheimnisse können dann als Variablen in Ihrer Releasedefinition verwendet werden, um die Speicherung in der Quellcodeverwaltung zu vermeiden.

- Verwenden Sie in Ihren Releasedefinitionen [Releasevariablen][vsts-release-variables], um Konfigurationsänderungen Ihrer Umgebungen zu steuern. Releasevariablen können für eine gesamte Freigabe oder eine bestimmte Umgebung gelten. Denken Sie daran, auf das Schlosssymbol zu klicken, wenn Sie Variablen für Geheimnisinformationen verwenden.

- In Ihrer Releasepipeline sollten [Bereitstellungsgates][vsts-deployment-gates] verwendet werden. Auf diese Weise können Sie Überwachungsdaten in Verbindung mit externen Systemen (etwa Incident Management oder zusätzliche individuelle Systeme) nutzen, um festzustellen, ob ein Release höher gestuft werden sollte.

- Sollte für eine Releasepipeline ein manueller Eingriff erforderlich sein, verwenden Sie die [Genehmigungsfunktion][vsts-approvals].

- Es ist ratsam, [Application Insights][application-insights] und weitere Überwachungstools so früh wie möglich in der Releasepipeline einzusetzen. Viele Organisationen beginnen erst in der Produktionsumgebung mit der Überwachung. Durch die Überwachung Ihrer anderen Umgebungen lassen sich Fehler früher im Entwicklungsprozess erkennen und Probleme in der Produktionsumgebung vermeiden.

## <a name="deploy-the-scenario"></a>Bereitstellen des Szenarios

### <a name="prerequisites"></a>Voraussetzungen

- Sie benötigen ein bestehendes Azure-Konto. Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) erstellen, bevor Sie beginnen.

- Zuerst müssen Sie sich für eine Azure DevOps-Organisation registrieren. Weitere Informationen finden Sie unter [Quickstart: Create an organization][vsts-account-create] (Schnellstart: Erstellen einer Organisation).

### <a name="walk-through"></a>Exemplarische Vorgehensweise

Über [Azure DevOps Projects](/azure/devops-project/azure-devops-project-github) werden Ihnen ein App Service-Plan, eine App Service-Instanz und eine App Insights-Ressource bereitgestellt. Außerdem wird eine Azure Pipelines-Pipeline für Sie konfiguriert.

Nachdem Sie mit Azure DevOps Projects eine Pipeline konfiguriert haben und der Buildvorgang abgeschlossen ist, können Sie sich die entsprechenden Codeänderungen, Arbeitselemente und Testergebnisse ansehen. Sie werden bemerken, dass keine Testergebnisse angezeigt werden, da der Code keine auszuführenden Tests enthält.

Die Pipeline erstellt eine Releasedefinition und einen Continuous Deployment-Trigger für die Bereitstellung der Anwendung in der Entwicklungsumgebung. Im Rahmen eines Continuous Deployment-Prozesses können Releases für mehrere Umgebungen gelten. Ein Release kann sowohl die Infrastruktur (mit Verfahren wie „Infrastructure-as-Code“) als auch die Bereitstellung der erforderlichen Anwendungspakete mit Aufgaben nach der Konfiguration umfassen.

## <a name="pricing"></a>Preise

Die Kosten für Azure DevOps richten sich nach der Anzahl von Benutzern in Ihrer Organisation, die Zugriff benötigen, und anderen Faktoren wie der Anzahl von erforderlichen gleichzeitigen Builds/Releases sowie der Anzahl von Testbenutzern. Weitere Informationen finden Sie in der [Azure DevOps-Preisübersicht][vsts-pricing-page].

Der [Preisrechner][vsts-pricing-calculator] liefert eine Kostenschätzung für den Betrieb von Azure DevOps mit 20 Benutzern.

Azure DevOps wird monatlich nach Benutzern abgerechnet. Es können weitere Gebühren anfallen, die sich nach den erforderlichen gleichzeitigen Pipelines und den zusätzlichen Testbenutzern bzw. Basic-Lizenzen für Benutzer richten.

## <a name="related-resources"></a>Zugehörige Ressourcen

Die folgenden Ressourcen enthalten weitere Informationen zu CI/CD und Azure DevOps:

- [Was ist DevOps?][devops-whatis]
- [DevOps at Microsoft – How we work with Azure DevOps][devops-microsoft] (DevOps bei Microsoft – Arbeiten mit Azure DevOps)
- [Schritt-für-Schritt-Tutorials: DevOps mit Azure DevOps][devops-with-vsts]
- [Checkliste für DevOps][devops-checklist]
- [Erstellen einer CI/CD-Pipeline für .NET mit Azure DevOps Projects][devops-project-create]

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
