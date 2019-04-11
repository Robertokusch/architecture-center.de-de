---
title: Szenarien und Beispiele für die Abonnementgovernance
description: Enthält Beispiele für das Implementieren der Azure-Abonnementgovernance für allgemeine Szenarien.
author: rdendtler
ms.date: 01/03/2017
ms.author: rodend
ms.topic: guide
ms.service: architecture-center
ms.subservice: enterprise-cloud-adoption
ms.openlocfilehash: 5ecdf1782fc274205e2750b9dc36fd2de3010e1f
ms.sourcegitcommit: c053e6edb429299a0ad9b327888d596c48859d4a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/20/2019
ms.locfileid: "58244181"
---
# <a name="examples-of-implementing-azure-enterprise-scaffold"></a>Beispiele für das Implementieren eines Azure-Unternehmensgerüsts

Dieser Artikel enthält Beispiele dafür, wie ein Unternehmen die Empfehlungen für ein [Azure-Unternehmensgerüst](azure-scaffold.md) implementieren kann. Ein fiktives Unternehmen mit dem Namen Contoso wird verwendet, um die bewährten Methoden für allgemeine Szenarien zu veranschaulichen.

## <a name="background"></a>Hintergrund

Contoso ist ein globales Unternehmen, das Supply Chain-Lösungen für Kunden bereitstellt. Sie bieten alles, vom Software-as-a-Service-Modell bis hin zu einem lokal bereitgestellten Paketmodell. Sie entwickeln Software auf der ganzen Welt, und betreiben große Entwicklungseinrichtungen in Indien, den USA und Kanada.

Der ISV-Bereich des Unternehmens ist in mehrere unabhängige Unternehmenseinheiten unterteilt, die Produkte in einem erheblichen Umfang verwalten. Jede Unternehmenseinheit verfügt über eigene Entwickler, Produktmanager und Architekten.

Die Unternehmenseinheit Enterprise Technology Services (ETS) bietet zentrale IT-Funktionen und verwaltet mehrere Datencenter, in denen Unternehmenseinheiten ihre Anwendungen hosten. Neben der Verwaltung der Datencenter bietet und verwaltet die ETS-Organisation zentralisierte Dienste für die Zusammenarbeit (beispielsweise E-Mails und Websites) sowie Netzwerk-/Telefoniedienste. Sie verwalten zudem kundenorientierte Workloads für kleinere Unternehmenseinheiten, die kein Betriebspersonal haben.

In diesem Artikel werden die folgenden Personas verwendet:

* Dave ist der Azure-Administrator von ETS.
* Alice ist bei Contoso die Leiterin der Entwicklung in der Supply Chain-Unternehmenseinheit.

Contoso muss eine branchenspezifische App und eine kundenorientierte App erstellen. Das Unternehmen hat entschieden, die Apps in Azure auszuführen. Dave liest den Artikel zur [präskriptiven Abonnementgovernance](azure-scaffold.md) und kann jetzt die Empfehlungen implementieren.

## <a name="scenario-1-line-of-business-application"></a>Szenario 1: Branchenanwendung

Contoso baut ein Quellcodeverwaltungssystem (BitBucket) auf, das von Entwicklern auf der ganzen Welt verwendet werden soll. Die Anwendung nutzt Infrastructure-as-a-Service (IaaS) zum Hosten und besteht aus Webservern und einem Datenbankserver. Entwickler greifen auf Server in ihren Entwicklungsumgebungen zu, aber sie benötigen keinen Zugriff auf die Server in Azure. Contoso ETS möchte dem Anwendungsbesitzer und dem Team das Verwalten der Anwendung ermöglichen. Die Anwendung ist nur über das Unternehmensnetzwerk von Contoso verfügbar. Dave muss das Abonnement für diese Anwendung einrichten. Das Abonnement soll in der Zukunft auch andere Software für Entwickler hosten.

### <a name="naming-standards-and-resource-groups"></a>Benennungsstandards und Ressourcengruppen

Dave erstellt ein Abonnement, um Entwicklertools zu unterstützen, die in allen Unternehmenseinheiten genutzt werden. Dave muss aussagekräftige Namen für das Abonnement und die Ressourcengruppen (für die Anwendung und die Netzwerke) erstellen. Er erstellt das Abonnement und die Ressourcengruppen wie folgt:

| Item | NAME | BESCHREIBUNG |
| --- | --- | --- |
| Abonnement |Contoso ETS DeveloperTools Production |Unterstützt allgemeine Entwicklertools |
| Ressourcengruppe |bitbucket-prod-rg |Enthält den Anwendungswebserver und den Datenbankserver |
| Ressourcengruppe |corenetworks-prod-rg |Enthält die virtuellen Netzwerke und die Site-to-Site-Gatewayverbindung |

### <a name="role-based-access-control"></a>Rollenbasierte Zugriffssteuerung

Nach dem Erstellen seines Abonnement möchte Dave sicherstellen, dass die entsprechenden Teams und Anwendungsbesitzer ihre Ressourcen zugreifen können. Dave erkennt, dass jedes Team andere Anforderungen hat. Er nutzt die Gruppen, die aus der lokalen Active Directory-Instanz (AD) von Contoso mit Azure Active Directory synchronisiert wurden, und bietet den Teams die passende Zugriffsebene.

Dave weist die folgenden Rollen für das Abonnement zu:

| Rolle | Zugewiesen zu | BESCHREIBUNG |
| --- | --- | --- |
| [Besitzer](/azure/role-based-access-control/built-in-roles#owner) |Verwaltete-ID aus AD von Contoso |Diese ID wird mit Just-In-Time-Zugriff (JIT) über das Identitätsverwaltungstool von Contoso gesteuert und stellt sicher, dass der Zugriff des Abonnementbesitzers vollständig überwacht wird. |
| [Benutzer mit Leseberechtigung für Sicherheitsfunktionen](/azure/role-based-access-control/built-in-roles#security-reader) |Abteilung für Sicherheits- und Risikomanagement |Diese Rolle ermöglicht es Benutzern, das Azure Security Center und den Status der Ressourcen anzuzeigen. |
| [Mitwirkender von virtuellem Netzwerk](/azure/role-based-access-control/built-in-roles#network-contributor) |Netzwerkteam |Diese Rolle ermöglicht es dem Netzwerkteam von Contoso, das Site-to-Site-VPN und die virtuellen Netzwerke zu verwalten. |
| *Benutzerdefinierte Rolle* |Anwendungsbesitzer |Dave erstellt eine Rolle, die die Berechtigung zum Ändern von Ressourcen innerhalb der Ressourcengruppe gewährt. Weitere Informationen finden Sie unter [Benutzerdefinierte Rollen in Azure RBAC](/azure/role-based-access-control/custom-roles). |

### <a name="policies"></a>Richtlinien

Dave hat die folgenden Anforderungen für die Verwaltung von Ressourcen im Abonnement:

* Da die Entwicklungstools Entwickler auf der ganzen Welt unterstützen, möchte er die Benutzer nicht daran hindern, Ressourcen in einer beliebigen Region zu erstellen. Er muss jedoch wissen, wo Ressourcen erstellt werden.
* Er macht sich Gedanken über die Kosten. Aus diesem Grund möchte er verhindern, dass Anwendungsbesitzer unnötig teure virtuelle Computer erstellen.
* Da diese Anwendung Entwicklern in vielen Unternehmenseinheiten dient, möchte er jede Ressource mit der Unternehmenseinheit und dem Anwendungsbesitzer kennzeichnen. Mithilfe dieser Tags kann ETS die Ressourcen den richtigen Teams in Rechnung stellen.

Er erstellt die folgenden [Azure-Richtlinien](/azure/azure-policy/azure-policy-introduction):

| Feld | Wirkung | BESCHREIBUNG |
| --- | --- | --- |
| location |audit |Überwachen der Erstellung von Ressourcen in einer beliebigen Region |
| type |deny |Verweigern der Erstellung von virtuellen Computern der G-Serie |
| tags |deny |Erfordern des Tags für Anwendungsbesitzer |
| tags |deny |Erfordern Tags für Kostenstellen |
| tags |append |Anfügen des Tagnamens **BusinessUnit** und des Tagwerts **ETS** an alle Ressourcen |

### <a name="resource-tags"></a>Ressourcentags

Dave weiß, dass in der Rechnung spezifische Informationen enthalten sein müssen, um die Kostenstelle für die BitBucket-Implementierung zu identifizieren. Darüber hinaus möchte Dave alle Ressourcen kennen, die ETS besitzt.

Er fügt die folgenden [Tags](/azure/azure-resource-manager/resource-group-using-tags) den Ressourcengruppen und Ressourcen hinzu.

| Tagname | Tagwert |
| --- | --- |
| ApplicationOwner |Der Name der Person, die diese Anwendung verwaltet |
| CostCenter |Die Kostenstelle der Gruppe, die für die Azure-Nutzung zahlt |
| BusinessUnit |**ETS** (die dem Abonnement zugeordneten Unternehmenseinheit) |

### <a name="core-network"></a>Kernnetzwerk

Das Contoso ETS-Team für Informationssicherheit und Risikomanagement überprüft den von Dave vorgeschlagenen Plan, die Anwendung nach Azure zu verschieben. Sie möchten sicherstellen, dass die Anwendung nicht über das Internet verfügbar gemacht wird.  Dave verfügt auch über Entwickler-Apps, die in der Zukunft nach Azure verschoben werden. Für diese Apps sind öffentliche Schnittstellen erforderlich.  Um diese Anforderungen zu erfüllen, stellt er interne und externe virtuelle Netzwerke und eine Netzwerksicherheitsgruppe zum Einschränken des Zugriffs bereit.

Er erstellt die folgenden Ressourcen:

| Ressourcentyp | NAME | BESCHREIBUNG |
| --- | --- | --- |
| Virtual Network |internal-vnet |Wird mit der Anwendung BitBucket verwendet und ist über ExpressRoute mit dem Contoso-Unternehmensnetzwerk verbunden.  Ein Subnetz (`bitbucket`) stellt einen bestimmten IP-Adressbereich für die Anwendung bereit. |
| Virtual Network |external-vnet |Dies ist für zukünftige Anwendungen verfügbar, die öffentliche Endpunkte erfordern. |
| Netzwerksicherheitsgruppen (NSG) |bitbucket-nsg |Stellt sicher, dass die Angriffsfläche dieser Workload minimiert wird, indem Verbindungen für das Subnetz mit der Anwendung (`bitbucket`) nur über Port 443 zugelassen werden. |

### <a name="resource-locks"></a>Ressourcensperren

Dave erkennt, dass die Konnektivität zwischen dem Unternehmensnetzwerk von Contoso und dem internen virtuellen Netzwerk vor fehlerhaften Skripts oder versehentlichem Löschen geschützt werden muss.

Er erstellt die folgende [Ressourcensperre](/azure/azure-resource-manager/resource-group-lock-resources):

| Sperrtyp | Ressource | BESCHREIBUNG |
| --- | --- | --- |
| **CanNotDelete** |internal-vnet |Verhindert, dass Benutzer das virtuelle Netzwerk oder Subnetze löschen; verhindert jedoch nicht, dass neue Subnetze hinzugefügt werden. |

### <a name="azure-automation"></a>Azure-Automatisierung

Dave möchte für diese Anwendung nichts automatisieren. Obwohl er ein Azure Automation-Konto erstellt hat, verwendet er es zunächst nicht.

### <a name="azure-security-center"></a>Azure Security Center

Die IT-Dienstverwaltung von Contoso muss Bedrohungen schnell identifizieren und behandeln. Sie möchten wissen, welche Probleme vorliegen können.

Um diese Anforderungen zu erfüllen, aktiviert Dave das [Azure Security Center](/azure/security-center/security-center-intro) und bietet Zugriff auf die Rolle „Benutzer mit Leseberechtigung für Sicherheitsfunktionen“.

## <a name="scenario-2-customer-facing-app"></a>Szenario 2: Kundenorientierte App

Die Unternehmensführung in der Supply Chain-Unternehmenseinheit hat verschiedene Möglichkeiten identifiziert, um mithilfe einer Kundenkarte die Kundenbindung für Contoso zu verbessern. Das Team von Alice muss diese Anwendung erstellen und beschließt, dass mit Azure die Anforderungen ihres Unternehmens besser erfüllt werden können. Alice arbeitet mit Dave aus der ETS-Abteilung zusammen, um zwei Abonnements zum Entwickeln und Betreiben dieser Anwendung zu konfigurieren.

### <a name="azure-subscriptions"></a>Azure-Abonnements

Dave meldet sich beim Azure Enterprise Portal an und stellt fest, dass die Supply Chain-Abteilung bereits vorhanden ist.  Da das Projekt jedoch das erste Entwicklungsprojekt für das Supply Chain-Team in Azure ist, erkennt Dave, dass ein neues Konto für das Entwicklungsteam von Alice eingerichtet werden muss.  Er erstellt das Konto „R&D“ für das Team und gewährt Alice Zugriff. Alice meldet sich über das Azure-Portal an und erstellt zwei Abonnements: eines für die Entwicklungsserver und eines für die Produktionsserver.  Sie befolgt beim Erstellen der folgenden Abonnements die zuvor festgelegten Benennungsstandards:

| Abonnementnutzung | NAME |
| --- | --- |
| Entwicklung |Contoso SupplyChain ResearchDevelopment LoyaltyCard Development |
| Bereitstellung |SupplyChain Operations LoyaltyCard Production |

### <a name="policies"></a>Richtlinien

Dave und Alice besprechen die Anwendung und legen fest, dass diese Anwendung nur Kunden in der Region Nordamerika dienen soll.  Alice und ihr Team möchten die Azure Application Service-Umgebung und Azure SQL verwenden, um die Anwendung zu erstellen. Möglicherweise müssen sie während der Entwicklung virtuelle Computer erstellen.  Alice und ihr Team möchten sicherstellen, dass ihre Entwickler über die erforderlichen Ressourcen verfügen, um Probleme ohne Unterstützung durch ETS zu untersuchen.

Für das **Entwicklungsabonnement** erstellen sie die folgende Richtlinie:

| Feld | Wirkung | BESCHREIBUNG |
| --- | --- | --- |
| location |audit |Überwachen der Erstellung von Ressourcen in einer beliebigen Region |

Sie begrenzen den Typ von SKU nicht, die ein Benutzer bei der Entwicklung erstellen kann, und sie erfordern keine Tags für Ressourcen oder Ressourcengruppen.

Für das **Produktionsabonnement** erstellen sie die folgenden Richtlinien:

| Feld | Wirkung | BESCHREIBUNG |
| --- | --- | --- |
| location |deny |Verweigern der Erstellung von Ressourcen außerhalb der US-amerikanischen Datencentern |
| tags |deny |Erfordern des Tags für Anwendungsbesitzer |
| tags |deny |Anfordern eines Abteilungstags |
| tags |append |Anfügen von Tags an jede Ressourcengruppe in der Produktionsumgebung |

Sie beschränken den Typ von SKU nicht, den ein Benutzer in der Produktion erstellen kann.

### <a name="resource-tags"></a>Ressourcentags

Dave erkennt, dass er spezifische Informationen benötigt, um die richtigen Unternehmensgruppen für Abrechnung und Besitz zu identifizieren. Er definiert Ressourcentags für Ressourcengruppen und Ressourcen.

| Tagname | Tagwert |
| --- | --- |
| ApplicationOwner |Der Name der Person, die diese Anwendung verwaltet |
| Department |Die Kostenstelle der Gruppe, die für die Azure-Nutzung zahlt |
| EnvironmentType |**Production** (Auch wenn das Abonnement das Wort **Production** im Namen enthält, ermöglicht dieser Tag eine leichte Identifikation der Ressourcen im Portal oder auf der Rechnung.) |

### <a name="core-networks"></a>Kernnetzwerke

Das Contoso ETS-Team für Informationssicherheit und Risikomanagement überprüft den von Dave vorgeschlagenen Plan, die Anwendung nach Azure zu verschieben. Sie möchten sicherstellen, dass die Anwendung für die Kundenkarte angemessen isoliert und in einem DMZ-Netzwerk geschützt ist.  Um diese Anforderung zu erfüllen, erstellen Dave und Alice ein externes virtuelles Netzwerk und eine Netzwerksicherheitsgruppe. Damit wird die Anwendung für die Kundenkarte vom Contoso-Unternehmensnetzwerk isoliert.

Für das **Entwicklungsabonnement** erstellen sie Folgendes:

| Ressourcentyp | NAME | BESCHREIBUNG |
| --- | --- | --- |
| Virtual Network |internal-vnet |Ist für die Entwicklungsumgebung der Contoso-Kundenkarte vorgesehen und über ExpressRoute mit dem Contoso-Unternehmensnetzwerk verbunden. |

Für das **Produktionsabonnement** erstellen sie Folgendes:

| Ressourcentyp | NAME | BESCHREIBUNG |
| --- | --- | --- |
| Virtual Network |external-vnet |Hostet die Anwendung für die Kundenkarte und ist nicht direkt mit der ExpressRoute-Instanz von Contoso verbunden. Code wird über das Quellcodesystem direkt an die PaaS-Dienste übertragen |
| Netzwerksicherheitsgruppen (NSG) |loyaltycard-nsg |Stellt sicher, dass die Angriffsfläche dieser Workload minimiert wird, indem eingehende Kommunikation nur über TCP 443 zugelassen wird.  Contoso untersucht auch die Nutzung einer Web Application Firewall für zusätzlichen Schutz |

### <a name="resource-locks"></a>Ressourcensperren

Dave und Alice beschließen, Ressourcensperren für einige der wichtigsten Ressourcen in der Umgebung hinzuzufügen, um ein versehentliches Löschen bei einem fehlerhaften Codepush zu verhindern.

Die folgende Sperre wird erstellt:

| Sperrtyp | Ressource | BESCHREIBUNG |
| --- | --- | --- |
| **CanNotDelete** |external-vnet |Verhindert, dass das virtuelle Netzwerk oder die Subnetze gelöscht werden. Die Sperre verhindert nicht das Hinzufügen neuer Subnetze |

### <a name="azure-automation"></a>Azure-Automatisierung

Alice und ihr Entwicklungsteam verfügen über umfangreiche Runbooks zum Verwalten der Umgebung für diese Anwendung. Die Runbooks ermöglichen das Hinzufügen/Löschen von Knoten für die Anwendung und andere DevOps-Aufgaben.

Um diese Runbooks zu verwenden, aktivieren sie [Automation](/azure/automation/automation-intro).

### <a name="azure-security-center"></a>Azure Security Center

Die IT-Dienstverwaltung von Contoso muss Bedrohungen schnell identifizieren und behandeln. Sie möchten wissen, welche Probleme vorliegen können.

Um diese Anforderungen zu erfüllen, aktiviert Dave das Azure Security Center. Er stellt sicher, dass das Azure Security Center die Ressourcen überwacht, und gewährt den DevOps- und Sicherheitsteams Zugriff.

## <a name="next-steps"></a>Nächste Schritte

* Informationen zum Erstellen von Resource Manager-Vorlagen finden Sie unter [Bewährte Methoden für das Erstellen von Azure Resource Manager-Vorlagen](/azure/azure-resource-manager/resource-manager-template-best-practices).
