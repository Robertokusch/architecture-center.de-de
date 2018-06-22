---
title: Mittlere Einführungsphase für Azure
description: Hier wird der Wissensstand beschrieben, den ein Unternehmen in der mittleren Einführungsphase für Azure benötigt.
author: petertay
ms.openlocfilehash: 227d9558647ed8076b2832d95e192f2f0c43b9db
ms.sourcegitcommit: 26b04f138a860979aea5d253ba7fecffc654841e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/19/2018
ms.locfileid: "36206360"
---
# <a name="azure-cloud-adoption-guide-intermediate-overview"></a>Handbuch zur Einführung von Azure Cloud: Übersicht über die mittlere Phase

In der grundlegenden Einführungsphase wurden die Grundkonzepte der Ressourcenkontrolle für Azure vorgestellt. Die grundlegende Phase diente dazu, Sie mit den ersten Schritten auf dem Weg zur Einführung von Azure vertraut zu machen, und Sie haben erfahren, wie Sie eine einfache Workload mit einem einzelnen kleinen Team bereitstellen. Die meisten großen Organisationen verfügen allerdings über eine Vielzahl von Teams, die gleichzeitig an vielen verschiedenen Workloads arbeiten. Es dürfte daher wenig überraschend sein, dass ein einfaches Governance-Modell für die Verwaltung komplexerer Organisations- und Entwicklungsszenarien nicht ausreicht.

Der Schwerpunkt der mittleren Azure-Einführungsphase liegt auf der Gestaltung Ihres Governance-Modells für mehrere Teams, die an mehreren neue Azure-Entwicklungsworkloads arbeiten.  

Die Zielgruppe für diese Phase des Handbuchs umfasst folgende Personas in Ihrer Organisation:
- *Finanzen:* Zuständig für die finanzielle Verpflichtung gegenüber Azure und verantwortlich für die Entwicklung von Richtlinien und Verfahren zur Überwachung der Kosten für die Ressourcennutzung (einschließlich Abrechnung und verbrauchsbasierte Kostenzuteilung).
- *Zentrale IT:* Zuständig für die Steuerung der Cloudressourcen Ihrer Organisation (einschließlich Ressourcenverwaltung und -zugriff) sowie für Workloadintegrität und -überwachung.
- *Besitzer der gemeinsamen Infrastruktur:* Technische Rollen, die für die Netzwerkkonnektivität zwischen lokaler Umgebung und Cloud zuständig sind.
- *Sicherheit:* Zuständig für die Implementierung der Sicherheitsrichtlinie, die erforderlich ist, um die Sicherheitsgrenze auf Azure auszuweiten. Kann auch für Sicherheitsinfrastruktur in Azure zum Speichern von Geheimnissen zuständig sein.
- *Workloadbesitzer:* Zuständig für die Veröffentlichung einer Workload in Azure. Je nach Struktur der Entwicklungsteams Ihrer Organisation kann es sich hierbei etwa um einen Entwicklungsleiter, einen leitenden Programm-Manager oder um einen leitenden Buildmanager handeln. Der Veröffentlichungsvorgang beinhaltet ggf. auch die Bereitstellung von Ressourcen in Azure.
  - *Workloadmitwirkender:* Zuständig für die Mitwirkung an der Veröffentlichung einer Workload in Azure. Für die Leistungsüberwachung und -optimierung ist unter Umständen Lesezugriff auf Azure-Ressourcen erforderlich. Berechtigungen zum Erstellen, Aktualisieren oder Löschen von Ressourcen sind nicht erforderlich.

## <a name="section-1-azure-concepts-for-multiple-workloads-and-multiple-teams"></a>Abschnitt 1: Azure-Konzepte für mehrere Workloads und Teams

In der grundlegenden Einführungsphase wurden einige Azure-Grundlagen vermittelt, und Sie haben erfahren, wie Ressourcen erstellt, gelesen, aktualisiert und gelöscht werden. Darüber hinaus wurde das Identitätskonzept erläutert, und Sie haben gelernt, dass Azure bei der Authentifizierung und Autorisierung von Benutzern, die Zugriff auf diese Ressourcen benötigen, nur Azure Active Directory (AD) vertraut.

Des Weiteren haben Sie eine Einführung in das Konfigurieren der Governance-Tools von Azure erhalten, um die Azure-Ressourcennutzung Ihrer Organisation zu verwalten. In der grundlegenden Phase haben Sie erfahren, wie Sie den Zugriff eines einzelnen Teams auf die Ressourcen steuern, die zum Bereitstellen einer einfachen Workload erforderlich sind. Tatsächlich befinden sich in Ihrer Organisation jedoch mehrere Teams, die gleichzeitig an verschiedenen Workloads arbeiten. 

Bevor wir beginnen, müssen wir kurz auf die Bedeutung des Begriffs **Workload** eingehen. Er bezeichnet in der Regel eine beliebige Funktionseinheit – etwa eine Anwendung oder einen Dienst. Eine Workload kann als Kombination aus den Codeartefakten, die auf einem Server bereitgestellt werden, und jeglichen anderen erforderlichen Diensten (etwa eine Datenbank) betrachtet werden. Diese Definition ist zwar für lokale Anwendungen oder Dienste hilfreich, muss für die Cloud allerdings noch etwas erweitert werden. 

In der Cloud umfasst eine Workload nicht nur sämtliche Artefakte, sondern auch die Cloudressourcen. Cloudressourcen werden von uns aufgrund eines Konzepts namens **Infrastructure-as-Code** in die Definition einbezogen. Wie Sie in „Erläuterungen: Wie funktioniert Azure?“ gelernt haben, werden Ressourcen in Azure durch einen Orchestratordienst bereitgestellt. Der Orchestratordienst macht diese Funktion über eine Web-API verfügbar. Diese Web-API kann über verschiedene Tools aufgerufen werden. Hierzu zählen etwa PowerShell, die Azure-Befehlszeilenschnittstelle (command line interface, CLI) und das Azure-Portal. Folglich können wir unsere Ressourcen in einer maschinenlesbaren Datei angeben, die zusammen mit den Codeartefakten für unsere Anwendung gespeichert werden kann.

Dadurch können wir eine Workload als Kombination aus Codeartefakten und den erforderlichen Cloudressourcen angeben und somit unsere Workloads **isolieren**. Die Isolierung von Workloads kann auf der Art der Ressourcenstrukturierung, auf der Netzwerktopologie oder auf anderen Attributen basieren. Die Workloadisolierung dient dazu, die spezifischen Ressourcen einer Workload einem Team zuzuordnen, damit das Team alle Aspekte dieser Ressourcen unabhängig verwalten kann. Dies ermöglicht die gemeinsame Nutzung von Ressourcenverwaltungsdiensten in Azure durch mehrere Teams und verhindert gleichzeitig die versehentliche Löschung oder Änderung fremder Ressourcen.

Darüber hinaus ermöglicht diese Isolierung ein weiteres Konzept namens [DevOps](https://azure.microsoft.com/solutions/devops/). DevOps umfasst die Softwareentwicklungsverfahren, die neben der Softwareentwicklung und den IT-Vorgängen von weiter oben auch eine möglichst weitreichende Automatisierung beinhaltet. Zu den Prinzipien von DevOps zählen unter anderem Continuous Integration und Continuous Delivery (CI/CD). Continuous Integration bezieht sich auf die automatisierten Buildprozesse, die ausgeführt werden, wann immer ein Entwickler eine Codeänderung committet. Continuous Delivery bezieht sich auf die automatisierten Prozesse, die diesen Code in verschiedenen **Umgebungen** bereitstellen (beispielsweise zu Testzwecken in einer **Entwicklungsumgebung** oder zur endgültigen Bereitstellung in einer **Produktionsumgebung**).

## <a name="section-2-governance-design-for-multiple-teams-and-multiple-workloads"></a>Abschnitt 2: Governance-Gestaltung für mehrere Teams und Workloads

In der [grundlegenden Phase](/azure/architecture/cloud-adoption-guide/adoption-intro/overview) des Handbuchs zur Einführung von Azure Cloud wurde das Konzept der Cloud-Governance vorgestellt. Sie haben erfahren, wie Sie ein einfaches Governance-Modell für ein einzelnes Team entwerfen, das an einer einzelnen Workload arbeitet. 

In der mittleren Phase werden die Grundkonzepte durch das [Governance-Entwurfshandbuch](governance-design-guide.md) erweitert und mehrere Teams, Workloads und Umgebungen hinzugefügt. Nachdem Sie sich mit den Beispielen aus dem Dokument vertraut gemacht haben, können Sie die Entwurfsprinzipien auf die Gestaltung und Implementierung des Governance-Modells Ihrer Organisation übertragen.

## <a name="section-3-implementing-a-resource-management-model"></a>Abschnitt 3: Implementieren eines Ressourcenverwaltungsmodells

Das Cloud-Governance-Modell Ihrer Organisation bildet die Schnittmenge zwischen den Azure-Tools zur Verwaltung des Ressourcenzugriffs, Ihren Mitarbeitern und den definierten Zugriffsverwaltungsregeln. Im Governance-Entwurfshandbuch wurden verschiedene Modelle zur Steuerung des Zugriffs auf Azure-Ressourcen erläutert. Hier gehen wir nun auf die Schritte ein, die erforderlich sind, um das Ressourcenverwaltungsmodell mit einem einzelnen Abonnement für die im Entwurfshandbuch beschriebenen Umgebungen (**gemeinsame Infrastruktur**, **Produktion** und **Entwicklung**) zu implementieren. In unserem Beispiel ist für alle drei Umgebungen der gleiche **Abonnementbesitzer** zuständig. Die einzelnen Workloads werden jeweils in einer **Ressourcengruppe** mit einem **Workloadbesitzer** isoliert, der mit der Rolle **Mitwirkender** hinzugefügt wird.

> [!NOTE]
> Unter [Grundlegendes zum Zugriff auf Ressourcen in Azure][understand-resource-access-in-azure] erfahren Sie mehr über die Beziehung zwischen Azure-Konten und Abonnements. 

Folgen Sie diesen Schritten:

1. Erstellen Sie ein [Azure-Konto](/azure/active-directory/sign-up-organization), falls Ihre Organisation noch keins besitzt. Die Person, die sich für das Azure-Konto registriert, wird zum Azure-Kontoadministrator, und die Geschäftsleitung Ihrer Organisation muss eine Person bestimmen, die diese Rolle übernimmt. Die Aufgaben dieser Person umfassen Folgendes:
    * Erstellen von Abonnements
    * Erstellen und Verwalten von [Azure Active Directory (AD)](/azure/active-directory/active-directory-whatis)-Mandanten zur Speicherung der Benutzeridentität für diese Abonnements    
2. Die Geschäftsleitung Ihrer Organisation bestimmt die zuständigen Personen für Folgendes:
    * Verwaltung von Benutzeridentitäten. Bei der Erstellung des Azure-Kontos Ihrer Organisation wird standardmäßig auch ein [Azure AD-Mandant](/azure/active-directory/develop/active-directory-howto-tenant) erstellt, und der Kontoadministrator wird standardmäßig als [globaler Azure AD-Administrator](/azure/active-directory/active-directory-assign-admin-roles-azure-portal#details-about-the-global-administrator-role) hinzugefügt. Ihrer Organisation kann einen anderen Benutzer mit der Verwaltung von Benutzeridentitäten betrauen, indem sie [diesem Benutzer die Rolle des globalen Azure AD-Administrators zuweist](/azure/active-directory/active-directory-users-assign-role-azure-portal). 
    * Abonnements. Diese Benutzer haben folgende Aufgaben:
        * Verwalten der Kosten im Zusammenhang mit der Ressourcennutzung in diesem Abonnement
        * Implementieren und Verwalten des Modells der geringsten Berechtigungen für den Ressourcenzugriff
        * Überwachen von Diensteinschränkungen
    * Gemeinsame Infrastrukturdienste (sofern sich Ihre Organisation für die Verwendung dieses Modells entscheidet). Dieser Benutzer ist für Folgendes zuständig:
        * Konnektivität zwischen der lokalen Umgebung und dem Azure-Netzwerk 
        * Netzwerkkonnektivität innerhalb von Azure durch Peering virtueller Netzwerke
    * Workloadbesitzer 
3. Der globale Azure AD-Administrator [erstellt die neuen Benutzerkonten](/azure/active-directory/add-users-azure-active-directory) für folgende Benutzer:
    * Die Person, die als **Abonnementbesitzer** für die einzelnen Abonnements fungiert, die den einzelnen Umgebungen zugeordnet sind. Hinweis: Dies ist nur erforderlich, wenn der **Dienstadministrator** des Abonnements nicht für die Verwaltung des Ressourcenzugriffs für die einzelnen Abonnements/Umgebungen zuständig ist.
    * Die Person, die als **Netzwerkbetriebsbenutzer** fungiert
    * Die Personen, die als **Workloadbesitzer** fungieren
4. Der Azure-Kontoadministrator erstellt über das [Azure-Kontoportal](https://account.azure.com) die drei folgenden Abonnements:
    * Ein Abonnement für die **gemeinsame Infrastruktur**
    * Ein Abonnement für die **Produktionsumgebung** 
    * Ein Abonnement für die **Entwicklungsumgebung** 
5. Der Azure-Kontoadministrator [fügt den Abonnements jeweils den Abonnementdienstbesitzer hinzu](/azure/billing/billing-add-change-azure-subscription-administrator#add-an-rbac-owner-admin-for-a-subscription-in-azure-portal).
6. Erstellen Sie einen Genehmigungsprozess, damit **Workloadbesitzer** die Erstellung von Ressourcengruppen anfordern können. Der Genehmigungsprozess kann auf verschiedene Arten (beispielsweise per E-Mail) implementiert werden. Alternativ können Sie ein Prozessverwaltungstool wie [Sharepoint-Workflows](https://support.office.com/article/introduction-to-sharepoint-workflow-07982276-54e8-4e17-8699-5056eff4d9e3) verwenden. Der Genehmigungsprozess kann wie folgt aussehen:  
    * Der **Workloadbesitzer** erstellt eine Stückliste mit Azure-Ressourcen, die in der **Entwicklungsumgebung**, in der **Produktionsumgebung** oder in beiden benötigt werden, und übermittelt sie an den **Abonnementbesitzer**.
    * Der **Abonnementbesitzer** prüft die Stückliste und die angeforderten Ressourcen, um sicherzustellen, dass sie für die geplante Verwendung angemessen sind (etwa durch Überprüfung, ob die angeforderten [VM-Größen](/azure/virtual-machines/windows/sizes) korrekt sind).
    * Wird die Anforderung nicht genehmigt, erhält der **Workloadbesitzer** eine entsprechende Benachrichtigung. Wenn die Anforderung genehmigt wird, führt der **Abonnementbesitzer** die [Erstellung der angeforderten Ressourcengruppe](/azure/azure-resource-manager/resource-group-portal#manage-resource-groups) unter Einhaltung der [Benennungskonventionen](/azure/architecture/best-practices/naming-conventions) Ihrer Organisation durch, [fügt den **Workloadbesitzer** hinzu](/azure/role-based-access-control/role-assignments-portal#add-access) (mit der [Rolle **Mitwirkender**](/azure/role-based-access-control/built-in-roles#contributor)) und informiert den **Workloadbesitzer** über die Erstellung der Ressourcengruppe.
7. Erstellen Sie einen Genehmigungsprozess, damit Workloadbesitzer beim Besitzer der gemeinsamen Infrastruktur eine Verbindung mit Peering virtueller Netzwerke anfordern können. Dieser Genehmigungsprozess kann genau wie im vorherigen Schritt per E-Mail oder unter Verwendung eines Prozessverwaltungstools implementiert werden.

Nach der Implementierung Ihres Governance-Modells können Sie Ihre gemeinsamen Infrastrukturdienste bereitstellen.

## <a name="section-4-deploy-shared-infrastructure-services"></a>Abschnitt 4: Bereitstellen gemeinsamer Infrastrukturdienste

Ihrer Organisation kann zwischen verschiedenen [Referenzarchitekturen für Hybridnetzwerke](/azure/architecture/reference-architectures/hybrid-networking/) wählen, um ihr lokales Netzwerk mit Azure zu verbinden. Jede dieser Referenzarchitekturen beinhaltet eine Bereitstellung, die eine Abonnement-ID erfordert. Geben Sie im Rahmen der Bereitstellung die Abonnement-ID für das Abonnement an, das Ihrer **gemeinsamen Infrastruktur** zugeordnet ist. Darüber hinaus müssen Sie die Vorlagendateien bearbeiten, um die Ressourcengruppe anzugeben, die von Ihrem für **Netzwerkvorgänge** zuständigen Benutzer verwaltet wird. Alternativ können Sie die Standardressourcengruppen in der Bereitstellung verwenden und ihnen den für **Netzwerkvorgänge** zuständigen Benutzer mit der Rolle **Mitwirkender** hinzufügen.

<!-- links -->
[understand-resource-access-in-azure]: /azure/role-based-access-control/rbac-and-directory-admin-roles