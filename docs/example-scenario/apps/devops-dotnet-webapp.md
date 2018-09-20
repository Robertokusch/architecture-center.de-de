---
title: CI/CD-Pipeline mit VSTS
description: Beispiel für die Erstellung und Freigabe einer .NET-App für Azure Web Apps
author: christianreddington
ms.date: 07/11/18
ms.openlocfilehash: aea757087f4a505a8c52658abe1841c5455977cc
ms.sourcegitcommit: c49aeef818d7dfe271bc4128b230cfc676f05230
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/11/2018
ms.locfileid: "44389272"
---
# <a name="cicd-pipeline-with-vsts"></a>CI/CD-Pipeline mit VSTS

Bei DevOps geht es um die Integration von Entwicklung, Qualitätssicherung und IT-Vorgängen. Für DevOps wird nicht nur eine einheitliche Kultur benötigt, sondern es sind auch robuste Prozesse zum Bereitstellen der Software erforderlich.

In diesem Beispielszenario wird veranschaulicht, wie Entwicklungsteams Visual Studio Team Services verwenden können, um eine .NET-Webanwendung mit zwei Ebenen für Azure App Service bereitzustellen. Die Webanwendung ist von nachgeschalteten Azure-PaaS-Diensten (Platform-as-a-Service) abhängig. In diesem Dokument wird auch auf einige Aspekte eingegangen, die Sie berücksichtigen sollten, wenn Sie mit Azure-PaaS (Platform-as-a-Service) ein Szenario dieser Art entwerfen.

Durch die Wahl eines modernen Ansatzes für die Anwendungsentwicklung mit Continuous Integration (CI) und Continuous Deployment (CD) können Sie schnell einen Mehrwert für Ihre Benutzer schaffen, indem Sie einen robusten Build-, Test-, Bereitstellungs- und Überwachungsdienst zur Verfügung stellen. Indem Organisationen eine Plattform wie Visual Studio Team Services zusätzlich zu Azure-Diensten wie App Service nutzen, können sie sich auf die Entwicklung des gewünschten Szenarios konzentrieren und müssen sich nicht um die Verwaltung der zugrunde liegenden Infrastruktur kümmern.

## <a name="related-use-cases"></a>Verwandte Anwendungsfälle

Erwägen Sie die Verwendung von DevOps für die folgenden Anwendungsfälle:

* Beschleunigen der Lebenszyklen für die Anwendungsentwicklung und -bereitstellung
* Sicherstellen der Qualität und Konsistenz für den automatisierten Build- und Freigabeprozess

## <a name="architecture"></a>Architecture

![Architekturübersicht über die Azure-Komponenten eines DevOps-Szenarios mit Visual Studio Team Services und Azure App Service][architecture]

Bei diesem Szenario geht es um eine DevOps-Pipeline für eine .NET-Webanwendung mit Visual Studio Team Services (VSTS). Die Daten durchlaufen das Szenario wie folgt:

1. Der Quellcode der Anwendung wird geändert.
2. Der Anwendungscode und die Datei „Web.config“ von Web-Apps werden committet.
3. Continuous Integration löst den Anwendungsbuildvorgang und Komponententests aus.
4. Der Continuous Deployment-Trigger orchestriert die Bereitstellung von Anwendungsartefakten *mit umgebungsspezifischen parametrisierten Konfigurationswerten*.
5. Bereitstellung in Azure App Service.
6. Azure Application Insights sammelt und analysiert Integritäts-, Leistungs- und Nutzungsdaten.
7. Lesen Sie die Informationen zu Integrität, Leistung und Nutzung.

### <a name="components"></a>Komponenten

* [Ressourcengruppen][resource-groups] sind ein logischer Container für Azure-Ressourcen und stellen außerdem eine Zugriffssteuerungsgrenze für die Verwaltungsebene dar. Sie können sich eine Ressourcengruppe wie eine „Bereitstellungseinheit“ vorstellen.
* [Visual Studio Team Services (VSTS)][vsts] ist ein Dienst, mit dem Sie Ihren Entwicklungslebenszyklus umfassend verwalten können: von der Planung und dem Projektmanagement über die Codeverwaltung bis hin zu Build und Release.
* [Azure Web Apps][web-apps] ist ein PaaS-Dienst (Platform-as-a-Service) zum Hosten von Webanwendungen, REST-APIs und mobilen Back-Ends. In diesem Artikel geht es zwar um .NET, aber es werden auch verschiedene andere Optionen für Entwicklungsplattformen unterstützt.
* [Application Insights][application-insights] ist ein erweiterbarer, für Webentwickler konzipierter Erstanbieterdienst zur Verwaltung der Anwendungsleistung (Application Performance Management, APM) auf mehreren Plattformen.

### <a name="alternative-devops-tooling-options"></a>Alternative DevOps-Tooloptionen

In diesem Artikel geht es um Visual Studio Team Services, aber Sie können auch [Team Foundation Server][team-foundation-server] als lokales Ersatztool nutzen. Alternativ hierzu ist ggf. auch eine Technologiesammlung verfügbar, die für eine Open-Source-Entwicklungspipeline mit Nutzung von [Jenkins][jenkins-on-azure] verwendet wird.

Aus Sicht von „Infrastruktur als Code“ werden [Azure Resource Manager-Vorlagen][arm-templates] im Rahmen des Azure DevOps-Projekts genutzt, Sie können aber auch [Terraform][terraform] oder [Chef][chef] verwenden, falls Sie in diese Lösungen investiert haben. Wenn Sie eine IaaS-basierte Bereitstellung (Infrastructure-as-a-Service) vorziehen und eine Konfigurationsverwaltung benötigen, können Sie die Nutzung von [Azure Automation State Configuration][desired-state-configuration], [Ansible][ansible] oder [Chef][chef] erwägen.

### <a name="alternatives-to-web-app-hosting"></a>Alternativen zum Web-App-Hosting

Alternativen zum Hosten in Azure-Web-Apps:

* [VM][compare-vm-hosting]: Für Workloads, für die ein hohes Maß an Kontrolle erforderlich ist oder die von Betriebssystemkomponenten bzw. -diensten abhängig sind, über die Web-Apps nicht verfügen (etwa Windows GAC oder COM).
* [Containerhosting][azure-containers]: Wird verwendet, wenn Betriebssystemabhängigkeiten bestehen und die Hostingportabilität bzw. Hostingdichte zu den Anforderungen gehören.
* [Service Fabric][service-fabric]: Eine gute Option, wenn es bei der Workloadarchitektur um verteilte Komponenten geht, die von der Bereitstellung profitieren und in einem stark kontrollierten Cluster ausgeführt werden. Service Fabric kann auch zum Hosten von Containern verwendet werden.
* [Serverlose Azure-Funktionen][azure-functions]: Eine gute Option, wenn die Workloadarchitektur auf fünf abgestimmten verteilten Komponenten mit minimalen Abhängigkeiten basiert, wobei einzelne Komponenten nur bedarfsabhängig ausgeführt werden müssen (nicht ständig) und die Orchestrierung von Komponenten nicht erforderlich ist.

### <a name="devops"></a>DevOps

Bei **[Continuous Integration (CI)][continuous-integration]** sollte das Ziel darin bestehen, einen stabilen Build zu erstellen, wobei ein einzelner Entwickler oder ein Team fortlaufend kleine, häufige Änderungen für die gemeinsame Codebasis committet.
Gehen Sie für Ihre Continuous Integration-Pipeline wie folgt vor:

* Checken Sie häufig kleinere Codemengen ein (das Zusammenfassen von größeren oder komplexeren Änderungen sollte vermieden werden, da die Zusammenführung schwieriger sein kann).
* Führen Sie für die Komponenten Ihrer Anwendung Komponententests mit ausreichender Code Coverage durch (einschließlich problematischer Pfade).
* Stellen Sie sicher, dass der Buildvorgang für die freigegebene Masterbranch (bzw. den Trunk) ausgeführt wird. Diese Branch sollte stabil und „bereit für die Bereitstellung“ sein. Unvollständige oder in Bearbeitung befindliche Änderungen sollten in einer separaten Branch mit häufigen Zusammenführungen vom Typ „Vorwärtsintegration“ isoliert werden, um später das Auftreten von Konflikten zu vermeiden.

Bei **[Continuous Delivery (CD)][continuous-delivery]** sollte das Ziel darin bestehen, nicht nur einen stabilen Buildvorgang zu erzielen, sondern eine stabile Bereitstellung. Aus diesem Grund ist es etwas schwieriger, CD umzusetzen. Es sind eine umgebungsspezifische Konfiguration und ein Mechanismus zum korrekten Festlegen dieser Werte erforderlich.

Außerdem muss eine ausreichende Abdeckung für Integrationstests vorhanden sein, um sicherzustellen, dass die unterschiedlichen Komponenten konfiguriert sind und für den gesamten Prozess richtig funktionieren.

Hierfür kann es auch erforderlich sein, umgebungsspezifische Daten zurückzusetzen und Datenbankschema-Versionen zu verwalten.

Continuous Delivery kann auch auf Umgebungen für Auslastungstests und Benutzerakzeptanztests ausgedehnt werden.

Continuous Delivery profitiert von der ständigen Überwachung, die idealerweise übergreifend für alle Umgebungen erfolgt.
Die Konsistenz und Zuverlässigkeit von umgebungsübergreifenden Bereitstellungen und Integrationstests wird vereinfacht, indem für die Erstellung und Konfiguration oder die Hostinginfrastruktur die Skripterstellung gewählt wird (deutlich einfacher für cloudbasierte Workloads; siehe „Azure-Infrastruktur als Code“). Dies wird als [„Infrastruktur als Code“][infra-as-code] bezeichnet.

* Beginnen Sie so früh wie möglich im Projektlebenszyklus mit dem Continuous Delivery-Prozess. Je später Sie starten, desto schwieriger wird es.
* Integrations- und Komponententests sollten die gleiche Priorität wie die Projektfunktionen haben.
* Verwenden Sie umgebungsunabhängige Bereitstellungspakete, und verwalten Sie die umgebungsspezifische Konfiguration über den Releaseprozess.
* Schützen Sie sensible Konfigurationen in den Tools für die Freigabeverwaltung, oder nutzen Sie ein Hardwaresicherheitsmodul (HSM) oder [Key Vault][azure-key-vault] während des Freigabeprozesses. Speichern Sie sensible Konfigurationsdaten nicht innerhalb der Quellcodeverwaltung.

**Continuous Learning**: Die effektivste Überwachung einer CD-Umgebung ist mit APM-Tools (Application Performance Monitoring, Anwendungsleistungsüberwachung) möglich, z.B. [Application Insights][application-insights] von Microsoft. Eine ausreichende Überwachungstiefe für eine Anwendungsworkload ist wichtig, um Fehler und die Leistung bei hoher Auslastung zu verstehen. [App Insights kann in VSTS integriert werden, um die ständige Überwachung der CD-Pipeline zu ermöglichen][app-insights-cd-monitoring]. Hiermit kann der automatische Übergang auf die nächste Ebene ohne Eingriff durch den Menschen oder ein Rollback bei Erkennung einer Warnung durchgeführt werden.

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

Sehen Sie sich die [typischen Entwurfsmuster für die Resilienz][design-patterns-resiliency] an, und erwägen Sie die Implementierung, falls dies möglich ist.

Im Azure Architecture Center sind verschiedene [empfohlene Vorgehensweisen für App Service][resiliency-app-service] aufgeführt.

Allgemeine Informationen zur Entwicklung robuster Lösungen finden Sie unter [Entwerfen robuster Anwendungen für Azure][resiliency].

## <a name="deploy-the-scenario"></a>Bereitstellen des Szenarios

### <a name="prerequisites"></a>Voraussetzungen

* Sie benötigen ein bestehendes Azure-Konto. Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto][azure-free-account] erstellen, bevor Sie beginnen.
* Sie müssen über ein vorhandenes VSTS-Konto (Visual Studio Team Services) verfügen. Ausführliche Informationen hierzu finden Sie unter [Erstellen eines VSTS-Kontos (Visual Studio Team Services)][vsts-account-create].

### <a name="walk-through"></a>Exemplarische Vorgehensweise

In diesem Szenario verwenden Sie das Azure DevOps-Projekt, um Ihre CI/CD-Pipeline zu erstellen.

Über das DevOps-Projekt werden für Sie ein App Service-Plan, eine App Service-Instanz und eine App Insights-Ressource bereitgestellt, und das Visual Studio Team Services-Projekt wird für Sie konfiguriert.

Nachdem Sie das DevOps-Projekt bereitgestellt haben und der Buildvorgang abgeschlossen ist, können Sie sich die entsprechenden Codeänderungen, Arbeitselemente und Testergebnisse ansehen. Sie werden bemerken, dass keine Testergebnisse angezeigt werden, da der Code keine auszuführenden Tests enthält.

Sehen Sie sich die Releasedefinition an. Beachten Sie, dass eine Releasepipeline eingerichtet wurde, um unsere Anwendung für Dev freizugeben. Ein **Continuous Deployment-Trigger** wird über das **Drop**-Buildartefakt mit automatischen Freigaben für die Dev-Umgebungen festgelegt. Im Rahmen eines Continuous Deployment-Prozesses gelten Freigaben ggf. über mehrere Umgebungen hinweg. Eine Freigabe kann sowohl die Infrastruktur (mit Verfahren wie „Infrastruktur als Code“) als auch die Bereitstellung der erforderlichen Anwendungspakete sowie von Aufgaben nach der Konfiguration umfassen.

**Zusätzliche Überlegungen**

* Erwägen Sie die Nutzung einer [Tokenisierungsaufgabe][vsts-tokenization], von denen auf dem VSTS-Marketplace mehrere verfügbar sind.
* Erwägen Sie die Nutzung der VSTS-Aufgabe [Bereitstellen: Azure Key Vault][download-keyvault-secrets], um Geheimnisse aus einer Azure Key Vault-Instanz in Ihre Freigabe herunterzuladen. Sie können diese Geheimnisse dann als Variablen im Rahmen Ihrer Releasedefinition verwenden. Sie sollten nicht unter der Quellcodeverwaltung gespeichert werden.
* Erwägen Sie die Verwendung von [Releasevariablen][vsts-release-variables] in Ihren Releasedefinitionen als Unterstützung von Konfigurationsänderungen Ihrer Umgebungen. Releasevariablen können für eine gesamte Freigabe oder eine bestimmte Umgebung gelten. Stellen Sie bei der Verwendung von Variablen für Geheimnisinformationen sicher, dass Sie das Schlosssymbol auswählen.
* Erwägen Sie die Verwendung von [Bereitstellungsgates][vsts-deployment-gates] in Ihrer Freigabepipeline. Dies ermöglicht die Nutzung von Überwachungsdaten mit externen Systemen (etwa Incident Management oder zusätzliche individuelle Systeme), um zu ermitteln, ob ein Release höher gestuft werden sollte.
* Wenn ein manueller Eingriff in einer Releasepipeline erforderlich ist, sollten Sie die Funktionalität für [Genehmigungen][vsts-approvals] verwenden.
* Es ist ratsam, [Application Insights][application-insights] und weitere Überwachungstools so früh wie möglich für Ihre Freigabepipeline einzusetzen. Die meisten Organisationen beginnen erst in der Produktionsumgebung mit der Überwachung, aber Sie können potenzielle Fehler schon zu einem früheren Zeitpunkt des Prozesses identifizieren, um Auswirkungen auf Ihre Benutzer während der Produktion zu verhindern.

## <a name="pricing"></a>Preise

Die Kosten für Visual Studio Team Services richten sich nach der Anzahl von Benutzern in Ihrer Organisation, die Zugriff benötigen, sowie nach Faktoren wie der Anzahl von erforderlichen gleichzeitigen Buildvorgängen/Freigaben und der Anzahl von Testbenutzern. Ausführlichere Informationen hierzu finden Sie auf der [Seite mit den VSTS-Preisen][vsts-pricing-page].

* [Visual Studio Team Services (VSTS)][vsts-pricing-calculator] ist ein Dienst, mit dem Sie Ihren Entwicklungslebenszyklus verwalten können und für den Kosten pro Benutzer und Monat berechnet werden. Es können weitere Gebühren anfallen, die sich nach den erforderlichen gleichzeitigen Pipelines und den zusätzlichen Testbenutzern bzw. Basic-Lizenzen für Benutzer richten.

## <a name="related-resources"></a>Zugehörige Ressourcen

* [Was ist DevOps?][devops-whatis]
* [DevOps bei Microsoft – Arbeiten mit Visual Studio Team Services][devops-microsoft]
* [Schritt-für-Schritt-Tutorials: DevOps mit Visual Studio Team Services][devops-with-vsts]
* [Erstellen einer CI/CD-Pipeline für .NET mit dem Azure DevOps-Projekt][devops-project-create]

<!-- links -->
[ansible]: /azure/ansible/
[application-insights]: /azure/application-insights/app-insights-overview
[app-service-reference-architecture]: /azure/architecture/reference-architectures/app-service-web-app/
[azure-free-account]: https://azure.microsoft.com/free/?WT.mc_id=A261C142F
[arm-templates]: /azure/azure-resource-manager/resource-group-overview#template-deployment
[architecture]: ./media/devops-dotnet-webapp/architecture-devops-dotnet-webapp.png
[availability]: /azure/architecture/checklist/availability
[chef]: /azure/chef/
[design-patterns-availability]: /azure/architecture/patterns/category/availability
[design-patterns-resiliency]: /azure/architecture/patterns/category/resiliency
[design-patterns-scalability]: /azure/architecture/patterns/category/performance-scalability
[design-patterns-security]: /azure/architecture/patterns/category/security
[desired-state-configuration]: /azure/automation/automation-dsc-overview
[devops-microsoft]: /azure/devops/devops-at-microsoft/
[devops-with-vsts]: https://almvm.azurewebsites.net/labs/vsts/
[application-insights]: https://azure.microsoft.com/en-gb/services/application-insights/
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
[vsts-account-create]: /vsts/organizations/accounts/create-account-msa-or-work-student?view=vsts
[vsts-approvals]: /vsts/pipelines/release/approvals/approvals?view=vsts
[devops-project]: https://portal.azure.com/?feature.customportal=false#create/Microsoft.AzureProject
[vsts-deployment-gates]: /vsts/pipelines/release/approvals/gates?view=vsts
[vsts-pricing-calculator]: https://azure.com/e/498aa024454445a8a352e75724f900b1
[vsts-pricing-page]: https://azure.microsoft.com/en-us/pricing/details/visual-studio-team-services/
[vsts-release-variables]: /vsts/pipelines/release/variables?view=vsts&tabs=batch
[vsts-tokenization]: https://marketplace.visualstudio.com/search?term=token&target=VSTS&category=All%20categories&sortBy=Relevance
[azure-key-vault]: /azure/key-vault/key-vault-overview
[infra-as-code]: https://blogs.msdn.microsoft.com/mvpawardprogram/2018/02/13/infrastructure-as-code/
[team-foundation-server]: https://visualstudio.microsoft.com/tfs/
[infra-as-code]: https://blogs.msdn.microsoft.com/mvpawardprogram/2018/02/13/infrastructure-as-code/
[service-fabric]:/azure/service-fabric/
[azure-functions]:/azure/azure-functions/
[azure-containers]:https://azure.microsoft.com/en-us/overview/containers/
[compare-vm-hosting]:/azure/app-service/choose-web-site-cloud-service-vm
[app-insights-cd-monitoring]:/azure/application-insights/app-insights-vsts-continuous-monitoring
[azure-region-pair-bcdr]:/azure/best-practices-availability-paired-regions
[devops-project-create]: /vsts/pipelines/apps/cd/azure/azure-devops-project-aspnetcore?view=vsts
[security]: /azure/security/