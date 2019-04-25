---
title: Bereitstellen von Azure-Anwendungen mit hoher Resilienz und Verfügbarkeit
description: Eine Übersicht der Verfahren zum Erstellen zuverlässiger Bereitstellungen in Azure
author: MikeWasson
ms.date: 04/10/2019
ms.topic: article
ms.service: architecture-center
ms.subservice: cloud-design-principles
ms.openlocfilehash: d8df3fe1c3353c60462959a625ba13ae47b96cf8
ms.sourcegitcommit: 579c39ff4b776704ead17a006bf24cd4cdc65edd
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/17/2019
ms.locfileid: "59642862"
---
# <a name="deploying-azure-applications-for-resiliency-and-availability"></a>Bereitstellen von Azure-Anwendungen mit hoher Resilienz und Verfügbarkeit

Beim Bereitstellen und Aktualisieren von Azure-Ressourcen, Anwendungscode und Konfigurationseinstellungen hilft Ihnen ein wiederholbarer und vorhersagbarer Prozess, Fehler und Ausfallzeiten zu vermeiden. Es empfiehlt sich, automatisierte Prozesse für die Bereitstellung einzusetzen, die bei Bedarf ausgeführt und beim Auftreten von Fehlern erneut ausgeführt werden können. Wenn Ihre Bereitstellungsprozesse erst einmal problemlos laufen, kann Prozessdokumentation dafür sorgen, dass dieser Zustand erhalten bleibt.

## <a name="automate-processes"></a>Automatisieren von Prozessen

Um Ressourcen bei Bedarf zu aktivieren, Lösungen schnell bereitzustellen, menschliche Fehler zu minimieren und konsistente und wiederholbare Ergebnisse zu erzeugen, müssen Sie Bereitstellungen und Aktualisierungen automatisieren.

### <a name="automate-as-many-processes-as-possible"></a>Automatisieren möglichst vieler Prozesse

Die zuverlässigsten Bereitstellungsprozesse sind automatisiert und *idempotent* – also wiederholbar, um stets gleiche Ergebnisse zu erzeugen.

- Zum Automatisieren der Bereitstellung von Azure-Ressourcen können Sie [Terraform](/azure/virtual-machines/windows/infrastructure-automation#terraform), [Ansible](/azure/virtual-machines/windows/infrastructure-automation#ansible), [Chef](/azure/virtual-machines/windows/infrastructure-automation#chef), [Puppet](/azure/virtual-machines/windows/infrastructure-automation#puppet), [Azure PowerShell](/powershell/azure/overview), die  [Azure-Befehlszeilenschnittstelle (CLI)](/cli/azure) oder  [Azure Resource Manager-Vorlagen](/azure/azure-resource-manager/resource-group-overview#template-deployment) verwenden.
- Um virtuelle Computer zu konfigurieren, können Sie [cloud-Init](/azure/virtual-machines/windows/infrastructure-automation#cloud-init) (für Linux-VMs) oder [Azure Automation State Configuration](/azure/automation/automation-dsc-overview) (DSC) verwenden.
- Zum Automatisieren der Anwendungsbereitstellung eignen sich [Azure DevOps Services](/azure/virtual-machines/windows/infrastructure-automation#azure-devops-services), [Jenkins](/azure/virtual-machines/windows/infrastructure-automation#jenkins) oder andere CI/CD-Lösungen.

Erstellen Sie als bewährte Methode ein Repository mit kategorisierten Automatisierungsskripts für den schnellen Zugriff, das mit Erklärungen der Parameter und Beispielen für die Verwendung der Skripts dokumentiert ist. Halten Sie diese Dokumentation stets auf dem Stand Ihrer Azure-Bereitstellungen, und weisen Sie eine Hauptperson zum Verwalten des Repositorys zu.

Auch die Aktivierung von Ressourcen bei Bedarf in der Notfallwiederherstellung kann mithilfe von Automatisierungsskripts erfolgen.

### <a name="use-code-to-provision-and-configure-infrastructure"></a>Verwenden von Code zum Bereitstellen und Konfigurieren von Infrastruktur

Für diese Praxis, die als *Infrastruktur als Code* bezeichnet wird, kann ein deklarativer oder ein imperativer Ansatz (oder eine Kombination von beiden) verwendet werden.

- [Resource Manager-Vorlagen](/azure/azure-resource-manager/resource-group-overview#template-deployment) sind ein Beispiel für einen deklarativen Ansatz.
- [PowerShell](/powershell/azure/overview)-Skripts sind ein Beispiel für einen imperativen Ansatz.

### <a name="practice-immutable-infrastructure"></a>Praktizieren von unveränderlicher Infrastruktur

Das heißt: Ändern Sie Infrastruktur nicht nach der Bereitstellung für die Produktion. Nach der Anwendung von Ad-Hoc-Änderungen wissen Sie möglicherweise nicht, was genau geändert wurde, sodass die Problembehandlung des Systems schwierig sein kann.

### <a name="automate-and-test-deployment-and-maintenance-tasks"></a>Automatisieren und Testen von Bereitstellungs- und Wartungsaufgaben

Verteilte Anwendungen bestehen aus mehreren Teilen, die zusammenarbeiten müssen. Die Bereitstellung sollte bewährte Mechanismen nutzen, beispielsweise Skripts, mit denen sich die Konfiguration aktualisieren und überprüfen und der Bereitstellungsprozess automatisieren lässt. Testen Sie alle Prozesse vollständig, um sicherzustellen, dass Fehler nicht zu zusätzlichen Ausfallzeiten führen.

### <a name="implement-deployment-security-measures"></a>Implementieren von Sicherheitsmaßnahmen für die Bereitstellung

Alle Bereitstellungstools müssen Sicherheitseinschränkungen beinhalten, um die bereitgestellte Anwendung zu schützen. Gehen Sie bei Definition und Durchsetzung von Bereitstellungsrichtlinien umsichtig vor, und minimieren Sie das Erfordernis menschlichen Eingriffs.

## <a name="deploy-to-multiple-regions-and-instances"></a>Bereitstellen in mehreren Regionen und Instanzen

Eine Anwendung, die von einer einzelnen Instanz eines Diensts abhängig ist, bildet einen Single Point of Failure. Um Resilienz und Skalierbarkeit zu verbessern, stellen Sie mehrere Instanzen bereit.

- Wählen Sie für [Azure App Service](/azure/app-service/app-service-value-prop-what-is/) einen [App Service-Plan](/azure/app-service/azure-web-sites-web-hosting-plans-in-depth-overview/) aus, der mehrere Instanzen bietet.
- Konfigurieren Sie für [Azure Cloud Services](/azure/cloud-services/cloud-services-choose-me) alle Rollen so, dass sie [mehrere Instanzen](/azure/cloud-services/cloud-services-choose-me/#scaling-and-management) verwenden.
- Stellen Sie für [Azure Virtual Machines](/azure/virtual-machines/virtual-machines-windows-about/?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) sicher, dass Ihre Architektur mehr als eine VM enthält und dass jede VM zu einer [Verfügbarkeitsgruppe](/azure/virtual-machines/virtual-machines-windows-manage-availability/) gehört.

### <a name="consider-deploying-across-multiple-regions"></a>Überlegungen zur Bereitstellung in mehreren Regionen

Es empfiehlt sich, alle bis auf besonders unkritische Anwendungen und Anwendungsdienste übergreifend in mehreren Regionen bereitzustellen. Wenn Ihre Anwendung in einer einzelnen Region bereitgestellt wird, ist die Anwendung nicht mehr verfügbar, falls doch einmal die gesamte Region ausfällt.

Wenn Sie sich für die Bereitstellung in einer einzelnen Region entscheiden, erwägen Sie, als Reaktion auf einen unerwarteten Ausfall eine erneute Bereitstellung in einer sekundären Region vorzubereiten.

### <a name="redeploy-to-a-secondary-region"></a>Erneute Bereitstellung in einer sekundären Region

Wenn Sie Anwendungen und Datenbanken in einer einzelnen, primären Region ohne Replikation ausführen, kann Ihre Wiederherstellungsstrategie darin bestehen, eine erneute Bereitstellung in einer anderen Region auszuführen. Diese Lösung ist erschwinglich, eignet sich aber vorwiegend für nicht kritische Anwendungen, bei denen längere Ausfallzeiten akzeptabel sind.

Wenn Sie diese Strategie wählen, automatisieren Sie den Bereitstellungsprozess so weit wie möglich, und schließen Sie Szenarien zur erneuten Bereitstellung in Ihre Tests für die Reaktion auf Notfälle ein.

Um Ihren Wiederbereitstellungsprozess zu automatisieren, erwägen Sie den Einsatz von [Azure Site Recovery](/azure/site-recovery/).

## <a name="use-staging-and-production-environments"></a>Verwendung von Staging- und Produktionsumgebungen

Durch sinnvollen Einsatz von Staging- und Produktionsumgebungen können Sie Updates in umfassend kontrollierter Weise per Push in die Produktionsumgebung übertragen und Unterbrechungen aufgrund unvorhergesehener Bereitstellungsprobleme minimieren.

- Die [*Blaugrün-Bereitstellung*](https://martinfowler.com/bliki/BlueGreenDeployment.html) beinhaltet das Bereitstellen eines Updates in einer von der Liveanwendung separaten Produktionsumgebung. Wechseln Sie für das Routing von Datenverkehr nach der Überprüfung der Bereitstellung dann zur aktualisierten Version. Dies kann beispielsweise durch Verwenden der [Stagingslots](/azure/app-service/web-sites-staged-publishing) erfolgen, die in Azure App Service für das Staging von Bereitstellungen vor dem Wechsel zur Produktion zur Verfügung stehen.
- [*Canary-Releases*](https://martinfowler.com/bliki/CanaryRelease.html) ähneln Blaugrün-Bereitstellungen. Statt den gesamten Datenverkehr in die aktualisierte Anwendung umzuleiten, routen Sie nur einen kleinen Teil des Datenverkehrs in die neue Bereitstellung. Falls ein Problem auftritt, kehren Sie wieder zur alten Bereitstellung zurück. Falls nicht, routen Sie nach und nach mehr Verkehr zur neuen Version. Wenn Sie Azure App Service verwenden, können Sie die Funktion Test-in-Produktion verwenden, um ein Canary-Release zu verwalten.

## <a name="create-a-rollback-plan-for-deployment-and-updates"></a>Erstellen eines Rollbackplans für Bereitstellungen und Updates

Falls bei einer Bereitstellung ein Fehler auftritt, ist Ihre Anwendung möglicherweise nicht verfügbar. Um Ausfallzeiten zu minimieren, entwerfen Sie einen Rollbackprozess, um zu einer als funktionierend bekannten Version zurückzukehren. Schließen Sie eine Strategie zum Zurücksetzen von Änderungen an Datenbanken und allen anderen Diensten ein, von denen Ihre App abhängt.

Wenn Sie Azure App Service verwenden, können Sie einen Slot für die letzte als funktionierend bekannte Website einrichten, den Sie für das Rollback von einer Web- oder API-App-Bereitstellung verwenden können.

## <a name="log-and-audit-deployments"></a>Protokollieren und Überwachen von Bereitstellungen

Implementieren Sie eine stabile Protokollierungsstrategie, um so viele versionsspezifische Informationen wie möglich zu erfassen. Wenn Sie mehrstufige Bereitstellungstechniken einsetzen, werden mehrere Versionen Ihrer Anwendung in der Produktionsumgebung ausgeführt. Wenn ein Problem auftritt, bestimmen Sie, von welcher Version es verursacht wird.

## <a name="document-release-processes"></a>Dokumentieren von Releaseprozessen

Ohne detaillierte Dokumentation des Releaseprozesses stellt ein Operator möglicherweise ein fehlerhaftes Update bereit oder konfiguriert die Einstellungen für Ihre Anwendung nicht ordnungsgemäß. Definieren und dokumentieren Sie den Freigabeprozess eindeutig, und stellen Sie sicher, dass er für das gesamte Betriebsteam verfügbar ist.

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Überwachen der Anwendungsintegrität für Zuverlässigkeit](./monitoring.md)
