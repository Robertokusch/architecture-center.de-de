---
title: CI/CD-Pipeline mit Azure DevOps
description: Erstellen und Veröffentlichen Sie mithilfe von Azure DevOps eine .NET-App in Azure-Web-Apps.
author: christianreddington
ms.date: 07/11/18
ms.openlocfilehash: 97f16b2d3d9c15bc6f5db6fad4c9d8097243ad3d
ms.sourcegitcommit: 0a31fad9b68d54e2858314ca5fe6cba6c6b95ae4
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/13/2018
ms.locfileid: "51610786"
---
# <a name="cicd-pipeline-with-azure-devops"></a>CI/CD-Pipeline mit Azure DevOps

Bei DevOps geht es um die Integration von Entwicklung, Qualitätssicherung und IT-Vorgängen. Für DevOps wird nicht nur eine einheitliche Kultur benötigt, sondern es sind auch robuste Prozesse zum Bereitstellen der Software erforderlich.

In diesem Beispielszenario wird veranschaulicht, wie Entwicklungsteams Azure DevOps verwenden können, um eine .NET-Webanwendung mit zwei Ebenen in Azure App Service bereitzustellen. Die Webanwendung ist von nachgeschalteten Azure-PaaS-Diensten (Platform-as-a-Service) abhängig. In diesem Dokument wird auch auf einige Aspekte eingegangen, die Sie berücksichtigen sollten, wenn Sie mit Azure-PaaS ein Szenario dieser Art entwerfen.

Durch die Wahl eines modernen Ansatzes für die Anwendungsentwicklung mit Continuous Integration und Continuous Deployment (CI/CD) können Sie schnell einen Mehrwert für Ihre Benutzer schaffen, indem Sie einen robusten Build-, Test-, Bereitstellungs- und Überwachungsdienst zur Verfügung stellen. Die Verwendung einer Plattform wie Azure DevOps in Verbindung mit Azure-Diensten wie App Service ermöglicht es Organisationen, sich auf die Entwicklung des gewünschten Szenarios zu konzentrieren, anstatt sich um die Verwaltung der unterstützenden Infrastruktur kümmern zu müssen.

## <a name="relevant-use-cases"></a>Relevante Anwendungsfälle

Erwägen Sie die Verwendung von DevOps für die folgenden Anwendungsfälle:

* Beschleunigen der Anwendungsentwicklung und Entwicklungslebenszyklen
* Sicherstellen der Qualität und Konsistenz für den automatisierten Build- und Freigabeprozess

## <a name="architecture"></a>Architecture

![Übersicht über die Architektur der Azure-Komponenten in einem DevOps-Szenario mit Azure DevOps und Azure App Service][architecture]

Dieses Szenario umfasst eine CI/CD-Pipeline für eine .NET-Webanwendung mit Azure DevOps. Die Daten durchlaufen das Szenario wie folgt:

1. Der Quellcode der Anwendung wird geändert.
2. Der Anwendungscode und die Datei „Web.config“ von Web-Apps werden committet.
3. Continuous Integration löst den Anwendungsbuildvorgang und Komponententests aus.
4. Der Continuous Deployment-Trigger orchestriert die Bereitstellung von Anwendungsartefakten *mit umgebungsspezifischen parametrisierten Konfigurationswerten*.
5. Bereitstellung in Azure App Service.
6. Azure Application Insights sammelt und analysiert Integritäts-, Leistungs- und Nutzungsdaten.
7. Lesen Sie die Informationen zu Integrität, Leistung und Nutzung.

### <a name="components"></a>Komponenten

* [Azure DevOps][vsts] ist ein Dienst für die umfassende und nahtlose Verwaltung Ihres Entwicklungslebenszyklus – von der Planung und dem Projektmanagement über die Codeverwaltung bis hin zum Build- und Releasevorgang.
* [Azure-Web-Apps][web-apps] ist ein PaaS-Dienst zum Hosten von Webanwendungen, REST-APIs und mobilen Back-Ends. In diesem Artikel geht es zwar um .NET, aber es werden auch verschiedene andere Optionen für Entwicklungsplattformen unterstützt.
* [Application Insights][application-insights] ist ein erweiterbarer, für Webentwickler konzipierter Erstanbieterdienst zur Verwaltung der Anwendungsleistung (Application Performance Management, APM) auf mehreren Plattformen.

### <a name="alternative-devops-tooling-options"></a>Alternative DevOps-Tooloptionen

In diesem Artikel geht es um Azure DevOps, Sie können aber auch [Team Foundation Server][team-foundation-server] als lokales Ersatztool nutzen. Alternativ ist eine Reihe von Technologien verfügbar, mit denen Sie eine Open Source-Entwicklungspipeline mit [Jenkins][jenkins-on-azure] erstellen können.

Aus der Sicht von „Infrastructure-as-Code“ werden [Azure Resource Manager-Vorlagen][arm-templates] im Rahmen des Azure DevOps-Projekts genutzt, Sie können aber auch [Terraform][terraform] oder [Chef][chef] verwenden. Wenn Sie eine IaaS-basierte Bereitstellung (Infrastructure-as-a-Service) vorziehen und eine Konfigurationsverwaltung benötigen, können Sie [Azure Automation State Configuration][desired-state-configuration], [Ansible][ansible] oder [Chef][chef] verwenden.

### <a name="alternatives-to-azure-web-apps"></a>Alternativen zu Azure-Web-Apps

Erwägen Sie ggf. die folgenden Alternativen zum Hosten in Azure-Web-Apps :

* [Microsoft Azure Virtual Machines][compare-vm-hosting]: Für Workloads, die ein hohes Maß an Kontrolle erfordern oder von Betriebssystemkomponenten und -diensten abhängig sind, die mit Web-Apps nicht genutzt werden können (etwa der globale Assemblycache von Windows oder COM).
* [Microsoft Azure Service Fabric][service-fabric]: Eine gute Option, wenn die Workloadarchitektur hauptsächlich verteilte Komponenten umfasst, die von der Bereitstellung und Ausführung in einem stark kontrollierten Cluster profitieren. Service Fabric kann auch zum Hosten von Containern verwendet werden.
* [Azure Functions][azure-functions]: Ein effektiver serverloser Ansatz, wenn die Workloadarchitektur auf abgestimmten verteilten Komponenten mit minimalen Abhängigkeiten basiert, einzelne Komponenten nur bedarfsabhängig ausgeführt werden müssen (nicht ständig) und die Orchestrierung von Komponenten nicht erforderlich ist.

Diese [Entscheidungsstruktur](/azure/architecture/guide/technology-choices/compute-decision-tree) kann Ihnen bei der Auswahl des richtigen Migrationspfads behilflich sein.

### <a name="devops"></a>DevOps

**[Continuous Integration (CI)][continuous-integration]** sorgt für einen stabilen Buildvorgang, bei dem mehrere Entwickler regelmäßig kleine, häufige Änderungen für die gemeinsame Codebasis committen. Gehen Sie für Ihre Continuous Integration-Pipeline wie folgt vor:
* Committen Sie kleinere Codeänderungen regelmäßig. Vermeiden Sie die Batchverarbeitung von größeren oder komplexeren Änderungen, die sich möglicherweise schwer zusammenführen lassen.
* Führen Sie für die Komponenten Ihrer Anwendung Komponententests mit ausreichender Code Coverage durch (einschließlich problematischer Pfade).
* Stellen Sie sicher, dass der Buildvorgang für den freigegebenen Masterbranch (bzw. Trunk) ausgeführt wird. Diese Branch sollte stabil und „bereit für die Bereitstellung“ sein. Unvollständige oder in Bearbeitung befindliche Änderungen sollten in einem separaten Branch mit häufigen Zusammenführungen vom Typ „Vorwärtsintegration“ isoliert werden, um spätere Konflikten zu vermeiden.

Bei **[Continuous Delivery (CD)][continuous-delivery]** geht es nicht nur um einen stabilen Buildvorgang, sondern auch um eine stabile Bereitstellung. Die Umsetzung von CD ist daher etwas schwieriger, weil eine umgebungsspezifische Konfiguration und ein Mechanismus zum korrekten Festlegen dieser Werte erforderlich sind. Beachten Sie im Hinblick auf Continuous Delivery zudem Folgendes:
* Es muss eine ausreichende Abdeckung für Integrationstests vorhanden sein, um zu überprüfen, ob die unterschiedlichen Komponenten konfiguriert sind und durchgängig richtig funktionieren.
* Continuous Delivery kann auch das Einrichten und Zurücksetzen umgebungsspezifischer Daten sowie die Verwaltung von Versionen des Datenbankschemas erfordern.
* Continuous Delivery sollte auch auf Umgebungen für Auslastungstests und Benutzerakzeptanztests ausgedehnt werden.
* Continuous Delivery profitiert von einer kontinuierlichen Überwachung, die idealerweise übergreifend für alle Umgebungen erfolgt.
* Die Konsistenz und Zuverlässigkeit von umgebungsübergreifenden Bereitstellungen und Integrationstests kann leichter gewährleistet werden, wenn Skripts für die Erstellung und Konfiguration der Hostinginfrastruktur erstellt werden. Dies ist für cloudbasierte Workloads erheblich einfacher. Weitere Informationen finden Sie unter [Infrastructure-as-Code][infra-as-code].
* Beginnen Sie so früh wie möglich im Projektlebenszyklus mit dem Continuous Delivery-Prozess. Je später Sie damit beginnen, desto schwieriger wird es, ihn zu integrieren.
* Integrations- und Komponententests sollten die gleiche Priorität haben wie Anwendungsfunktionen.
* Verwenden Sie umgebungsunabhängige Bereitstellungspakete, und verwalten Sie die umgebungsspezifische Konfiguration über den Releaseprozess.
* Schützen Sie sensible Konfigurationen mit den Tools für die Releaseverwaltung, oder nutzen Sie während des Releaseprozesses ein Hardwaresicherheitsmodul (HSM) oder [Azure Key Vault][azure-key-vault]. Speichern Sie sensible Konfigurationsdaten nicht innerhalb der Quellcodeverwaltung.

**Kontinuierlicher Informationsfluss**. Eine CD-Umgebung lässt sich am effektivsten mit Tools zur Überwachung der Anwendungsleistung wie [Application Insights][application-insights] überwachen. Eine ausreichende Überwachungstiefe für eine Anwendungsworkload ist wichtig, um Fehler und die Leistung bei Auslastung zu verstehen. Application Insights kann in VSTS integriert werden, um eine [kontinuierliche Überwachung der CD-Pipeline zu ermöglichen][app-insights-cd-monitoring]. Hiermit kann der automatische Übergang auf die nächste Ebene ohne Eingriff durch den Menschen oder ein Rollback bei Erkennung einer Warnung durchgeführt werden.

## <a name="considerations"></a>Überlegungen

### <a name="availability"></a>Verfügbarkeit

Erwägen Sie beim Erstellen Ihrer Cloudanwendung die Nutzung der [typischen Entwurfsmuster für die Verfügbarkeit][design-patterns-availability].

Sehen Sie sich die Aspekte zur Verfügbarkeit in der entsprechenden [Referenzarchitektur für App Service-Webanwendungen][app-service-reference-architecture] an.

Weitere Verfügbarkeitsthemen finden Sie im Azure Architecture Center in der [Checkliste für die Verfügbarkeit][availability].

### <a name="scalability"></a>Skalierbarkeit

Beachten Sie beim Erstellen einer Cloudanwendung die [typischen Entwurfsmuster für die Skalierbarkeit][design-patterns-scalability].

Sehen Sie sich die Aspekte zur Skalierbarkeit in der entsprechenden [Referenzarchitektur für App Service-Webanwendungen][app-service-reference-architecture] an.

Weitere Skalierbarkeitsthemen finden Sie im Azure Architecture Center in der [Checkliste für die Skalierbarkeit][scalability].

### <a name="security"></a>Sicherheit

Erwägen Sie die Nutzung der [typischen Entwurfsmuster für die Sicherheit][design-patterns-security], wo dies möglich ist.

Sehen Sie sich die Aspekte zur Sicherheit in der entsprechenden [Referenzarchitektur für App Service-Webanwendungen][app-service-reference-architecture] an.

Allgemeine Informationen zur Entwicklung sicherer Lösungen finden Sie in der [Dokumentation zur Azure-Sicherheit][security].

### <a name="resiliency"></a>Resilienz

Erwägen Sie ggf. die Implementierung der [typischen Entwurfsmuster für die Resilienz][design-patterns-resiliency].

Im Azure Architecture Center sind verschiedene [empfohlene Vorgehensweisen für App Service][resiliency-app-service] aufgeführt.

Allgemeine Informationen zur Entwicklung robuster Lösungen finden Sie unter [Entwerfen robuster Anwendungen für Azure][resiliency].

## <a name="deploy-the-scenario"></a>Bereitstellen des Szenarios

### <a name="prerequisites"></a>Voraussetzungen

* Sie benötigen ein bestehendes Azure-Konto. Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto][azure-free-account] erstellen, bevor Sie beginnen.
* Zuerst müssen Sie sich für eine Azure DevOps-Organisation registrieren. Weitere Informationen finden Sie unter [Quickstart: Create your organization][vsts-account-create] (Schnellstart: Erstellen Ihrer Organisation).

### <a name="walk-through"></a>Exemplarische Vorgehensweise

In diesem Szenario verwenden Sie das Azure DevOps-Projekt, um Ihre CI/CD-Pipeline zu erstellen.

Über das Azure DevOps-Projekt werden für Sie ein App Service-Plan, eine App Service-Instanz und eine App Insights-Ressource bereitgestellt, und das Azure DevOps-Projekt wird für Sie konfiguriert.

Nachdem Sie das Azure DevOps-Projekt bereitgestellt haben und der Buildvorgang abgeschlossen ist, können Sie sich die entsprechenden Codeänderungen, Arbeitselemente und Testergebnisse ansehen. Sie werden bemerken, dass keine Testergebnisse angezeigt werden, da der Code keine auszuführenden Tests enthält.

Lesen Sie die Releasedefinitionen. Beachten Sie, dass eine Releasepipeline eingerichtet wurde, um die Anwendung für die Entwicklungsumgebung freizugeben. Ein **Continuous Deployment-Trigger** ist über das **Drop**-Buildartefakt mit automatischen Freigaben für die Entwicklungsumgebung festgelegt. Im Rahmen eines Continuous Deployment-Prozesses können Releases für mehrere Umgebungen gelten. Ein Release kann sowohl die Infrastruktur (mit Verfahren wie „Infrastructure-as-Code“) als auch die Bereitstellung der erforderlichen Anwendungspakete mit Aufgaben nach der Konfiguration umfassen.

## <a name="additional-considerations"></a>Zusätzliche Überlegungen

* Erwägen Sie die Nutzung einer der [Tokenisierungsaufgaben][vsts-tokenization], die auf dem VSTS-Marketplace verfügbar sind.
* Erwägen Sie die Verwendung der VSTS-Aufgabe [Bereitstellen: Azure Key Vault][download-keyvault-secrets], um Geheimnisse aus einer Azure Key Vault-Instanz in Ihr Release herunterzuladen. Sie können diese Geheimnisse dann als Variablen in Ihrer Releasedefinition verwenden, damit Sie sie nicht in der Quellcodeverwaltung speichern müssen.
* Erwägen Sie die Verwendung von [Releasevariablen][vsts-release-variables] in Ihren Releasedefinitionen als Unterstützung von Konfigurationsänderungen Ihrer Umgebungen. Releasevariablen können für eine gesamte Freigabe oder eine bestimmte Umgebung gelten. Denken Sie daran, auf das Schlosssymbol zu klicken, wenn Sie Variablen für Geheimnisinformationen verwenden.
* Erwägen Sie die Verwendung von [Bereitstellungsgates][vsts-deployment-gates] in Ihrer Freigabepipeline. Auf diese Weise können Sie Überwachungsdaten in Verbindung mit externen Systemen (etwa Incident Management oder zusätzliche individuelle Systeme) nutzen, um festzustellen, ob ein Release höher gestuft werden sollte.
* Wenn ein manueller Eingriff in einer Releasepipeline erforderlich ist, sollten Sie die Funktionalität für [Genehmigungen][vsts-approvals] verwenden.
* Es ist ratsam, [Application Insights][application-insights] und weitere Überwachungstools so früh wie möglich in der Releasepipeline einzusetzen. Viele Organisationen beginnen erst in der Produktionsumgebung mit der Überwachung. Durch die Überwachung Ihrer anderen Umgebungen können Sie Fehler früher im Entwicklungsprozess identifizieren und Probleme in der Produktionsumgebung vermeiden.

## <a name="pricing"></a>Preise

Die Kosten für Azure DevOps richten sich nach der Anzahl von Benutzern in Ihrer Organisation, die Zugriff benötigen, und anderen Faktoren wie der Anzahl von erforderlichen gleichzeitigen Builds/Releases sowie der Anzahl von Testbenutzern. Weitere Informationen finden Sie in der [Azure DevOps-Preisübersicht][vsts-pricing-page].

* [Azure DevOps][vsts-pricing-calculator] ist ein Dienst, der Ihnen die Verwaltung Ihres Entwicklungszyklus ermöglicht. Er wird pro Benutzer und Monat bezahlt. Es können weitere Gebühren anfallen, die sich nach den erforderlichen gleichzeitigen Pipelines und den zusätzlichen Testbenutzern bzw. Basic-Lizenzen für Benutzer richten.

## <a name="related-resources"></a>Zugehörige Ressourcen

* [Was ist DevOps?][devops-whatis]
* [DevOps at Microsoft – How we work with Azure DevOps][devops-microsoft] (DevOps bei Microsoft – Arbeiten mit Azure DevOps)
* [Schritt-für-Schritt-Tutorials: DevOps mit Azure DevOps][devops-with-vsts]
* [Checkliste für DevOps][devops-checklist]
* [Erstellen einer CI/CD-Pipeline für .NET mit dem Azure DevOps-Projekt][devops-project-create]

<!-- links -->
[ansible]: /azure/ansible/
[application-insights]: /azure/application-insights/app-insights-overview
[app-service-reference-architecture]: ../../reference-architectures/app-service-web-app/basic-web-app.md
[azure-free-account]: https://azure.microsoft.com/free/?WT.mc_id=A261C142F
[arm-templates]: /azure/azure-resource-manager/resource-group-overview#template-deployment
[architecture]: ./media/architecture-devops-dotnet-webapp.png
[availability]: /azure/architecture/checklist/availability
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
[resiliency]: /azure/architecture/checklist/resiliency
[scalability]: /azure/architecture/checklist/scalability
[vsts]: /vsts/?view=vsts#pivot=services
[continuous-integration]: /azure/devops/what-is-continuous-integration
[continuous-delivery]: /azure/devops/what-is-continuous-delivery
[web-apps]: /azure/app-service/app-service-web-overview
[terraform]: /azure/terraform/
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
[team-foundation-server]: https://visualstudio.microsoft.com/tfs/
[infra-as-code]: https://blogs.msdn.microsoft.com/mvpawardprogram/2018/02/13/infrastructure-as-code/
[service-fabric]: /azure/service-fabric/
[azure-functions]: /azure/azure-functions/
[azure-containers]: https://azure.microsoft.com/overview/containers/
[compare-vm-hosting]: /azure/app-service/choose-web-site-cloud-service-vm
[app-insights-cd-monitoring]: /azure/application-insights/app-insights-vsts-continuous-monitoring
[azure-region-pair-bcdr]: /azure/best-practices-availability-paired-regions
[devops-project-create]: /azure/devops-project/azure-devops-project-aspnet-core
[security]: /azure/security/