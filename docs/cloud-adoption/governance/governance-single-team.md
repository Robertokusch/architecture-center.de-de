---
title: 'Enterprise Cloud-Einführung: Governance-Entwurf für eine einfache Workload'
description: Leitfaden zur Konfiguration von Kontrollen in Bezug auf Azure-Governance, um für Benutzer die Bereitstellung einer einfachen Workload zu ermöglichen.
author: petertaylor9999
ms.date: 09/10/2018
ms.topic: guide
ms.service: architecture-center
ms.subservice: enterprise-cloud-adoption
ms.openlocfilehash: f87584b4182e5eca143f6429220c822b4be2ed70
ms.sourcegitcommit: 1b50810208354577b00e89e5c031b774b02736e2
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/23/2019
ms.locfileid: "54481727"
---
# <a name="enterprise-cloud-adoption-governance-design-for-a-simple-workload"></a>Enterprise Cloud-Einführung: Governance-Entwurf für eine einfache Workload

Anhand dieser Anleitung können Sie sich mit dem Prozess für das Entwerfen eines Modells zur Ressourcenkontrolle (Governance) in Azure vertraut machen, um ein einzelnes Team und eine einzelne Workload zu unterstützen.  Wir sehen uns eine Reihe von hypothetischen Governance-Anforderungen an und gehen dann mehrere Beispielimplementierungen durch, die diese Anforderungen erfüllen. 

In der grundlegenden Einführungsphase besteht das Ziel darin, eine einfache Workload in Azure bereitzustellen. Dies führt zu den folgenden Anforderungen:
* Identitätsverwaltung für einen einzelnen **Workloadbesitzer**, der für das Bereitstellen und Warten der einfachen Workload verantwortlich ist. Der Workloadbesitzer benötigt die Berechtigung zum Erstellen, Lesen, Aktualisieren und Löschen von Ressourcen sowie die Berechtigung zum Delegieren dieser Rechte an andere Benutzer im Identitätsverwaltungssystem.
* Verwaltung aller Ressourcen für die einfache Workload zusammen in einer Verwaltungseinheit.

## <a name="licensing-azure"></a>Lizenzieren von Azure

Bevor wir damit beginnen, das Governance-Modell zu entwerfen, ist es wichtig, dass wir uns mit der Azure-Lizenzierung vertraut machen. Der Grund ist, dass die Administratorkonten, die Ihrer Azure-Lizenz zugeordnet sind, über die höchste Zugriffsebene auf Ihre gesamten Azure-Ressourcen verfügen. Diese Administratorkonten bilden die Grundlage Ihres Governance-Modells.  

> [!NOTE]
> Wenn Ihre Organisation über ein vorhandenes [Microsoft Enterprise Agreement](https://www.microsoft.com/en-us/licensing/licensing-programs/enterprise.aspx) verfügt, in dem Azure nicht enthalten ist, kann Azure hinzugefügt werden, indem Sie vorab eine Zahlungsverpflichtung eingehen. Weitere Informationen finden Sie unter [Lizenzierung von Azure für das Unternehmen](https://azure.microsoft.com/pricing/enterprise-agreement/). 

Als Azure dem Enterprise Agreement Ihrer Organisation hinzugefügt wurde, wurde Ihre Organisation aufgefordert, ein **Azure-Konto** zu erstellen. Bei der Erstellung des Kontos wurden ein **Azure-Kontobesitzer** und ein Azure Active Directory-Mandant (Azure AD) mit einem Konto vom Typ **Globaler Administrator** erstellt. Ein Azure AD-Mandant ist ein logisches Konstrukt, das eine sichere, dedizierte Instanz von Azure AD darstellt.

![Azure-Konto mit Azure-Konto-Manager und globalem Azure AD-Administrator](../_images/governance-3-0.png)
*Abbildung 1: Azure-Konto mit einem Azure-Konto-Manager und globalem Azure AD-Administrator*

## <a name="identity-management"></a>Identitätsverwaltung

Da für Azure nur [Azure AD](/azure/active-directory) vertrauenswürdig ist, was die Authentifizierung von Benutzern und die Autorisierung des Benutzerzugriffs auf Ressourcen betrifft, ist Azure AD unser Identitätsverwaltungssystem. Der globale Azure AD-Administrator verfügt über Berechtigungen der höchsten Ebene und kann identitätsbezogene Aktionen durchführen, z.B. das Erstellen von Benutzern und das Zuweisen von Berechtigungen. 

Wir benötigen eine Identitätsverwaltung für einen einzelnen **Workloadbesitzer**, der für das Bereitstellen und Warten der einfachen Workload verantwortlich ist. Der Workloadbesitzer benötigt die Berechtigung zum Erstellen, Lesen, Aktualisieren und Löschen von Ressourcen sowie die Berechtigung zum Delegieren dieser Rechte an andere Benutzer im Identitätsverwaltungssystem.

Unser globaler Azure AD-Administrator erstellt das **Workloadbesitzer**-Konto für den **Workloadbesitzer**:

![Globaler Azure AD-Administrator erstellt das Workloadbesitzer-Konto](../_images/governance-1-2.png)
*Abbildung 2: Globaler Azure AD-Administrator erstellt das Workloadbesitzer-Konto.*

Wir können die Zugriffsberechtigung für Ressourcen erst zuweisen, nachdem dieser Benutzer einem **Abonnement** hinzugefügt wurde. Dies führen wir in den nächsten beiden Abschnitten durch. 

## <a name="resource-management-scope"></a>Ressourcenverwaltungsbereich

Wenn die Anzahl der von Ihrer Organisation bereitgestellten Ressourcen zunimmt, erhöht sich auch die Komplexität in Bezug auf die Steuerung dieser Ressourcen. Azure implementiert eine logische Containerhierarchie, damit Ihre Organisation die Ressourcen in Gruppen mit unterschiedlicher Granularität verwalten kann. Dies wird auch als **Bereich** (Scope) bezeichnet. 

Die oberste Ebene des Ressourcenverwaltungsbereichs ist die Ebene **Abonnement**. Ein Abonnement wird vom Azure-**Kontobesitzer** erstellt, der die Zahlungsverpflichtung einrichtet und für die Bezahlung aller Azure-Ressourcen verantwortlich ist, die dem Abonnement zugeordnet sind:

![Azure-Kontobesitzer erstellt ein Abonnement](../_images/governance-1-3.png)
*Abbildung 3: Der Azure-Kontobesitzer erstellt ein Abonnement.*

Bei der Erstellung des Abonnements ordnet der Azure-**Kontobesitzer** dem Abonnement einen Azure AD-Mandanten zu, und dieser Azure AD-Mandant wird verwendet, um Benutzer zu authentifizieren und zu autorisieren:

![Azure-Kontobesitzer ordnet den Azure AD-Mandanten dem Abonnement zu](../_images/governance-1-4.png)
*Abbildung 4: Der Azure-Kontobesitzer ordnet den Azure AD-Mandanten dem Abonnement zu.*

Unter Umständen haben Sie bemerkt, dass dem Abonnement derzeit kein Benutzer zugeordnet ist. Dies bedeutet, dass keine Person über die Berechtigung zum Verwalten von Ressourcen verfügt. In Wirklichkeit ist der **Kontobesitzer** der Besitzer des Abonnements und verfügt über die Berechtigung zur Durchführung aller Aktionen für eine Ressource des Abonnements. Aber in der Praxis ist der **Kontobesitzer** in Ihrer Organisation wahrscheinlich eher ein Mitarbeiter der Finanzabteilung und nicht dafür verantwortlich, Ressourcen zu erstellen, zu lesen, zu aktualisieren und zu löschen. Diese Aufgaben werden vom **Workloadbesitzer** übernommen. Daher müssen wir den **Workloadbesitzer** dem Abonnement hinzufügen und Berechtigungen zuweisen.

Da der **Kontobesitzer** derzeit der einzige Benutzer mit der Berechtigung zum Hinzufügen des **Workloadbesitzers** zum Abonnement ist, übernimmt er die Aufgabe, den **Workloadbesitzer** dem Abonnement hinzuzufügen:

![Azure-Kontobesitzer fügt den **Workloadbesitzer** dem Abonnement hinzu](../_images/governance-1-5.png)
*Abbildung 5: Azure-Kontobesitzer fügt den **Workloadbesitzer** dem Abonnement hinzu.*

Der Azure-**Kontobesitzer** erteilt dem **Workloadbesitzer** Berechtigungen, indem eine RBAC-Rolle ([Role-Based Access Control, rollenbasierte Zugriffssteuerung](/azure/role-based-access-control/)) zugewiesen wird. Mit der RBAC-Rolle wird ein Satz mit Berechtigungen angegeben, über die der **Workloadbesitzer** für einen oder mehrere Ressourcentypen verfügt.

Beachten Sie, dass der **Kontobesitzer** in diesem Beispiel die [integrierte Rolle **Besitzer**](/azure/role-based-access-control/built-in-roles#owner) zugewiesen hat: 

![Dem **Workloadbesitzer** wurde die integrierte Rolle „Besitzer“ zugewiesen](../_images/governance-1-6.png)
*Abbildung 6: Dem Workloadbesitzer wurde die integrierte Rolle „Besitzer“ zugewiesen.*

Mit der integrierten Rolle **Besitzer** werden für den **Workloadbesitzer** alle Berechtigungen für den Abonnementbereich gewährt. 

> [!IMPORTANT]
> Der Azure-**Kontobesitzer** ist für die Zahlungsverpflichtung verantwortlich, die dem Abonnement zugeordnet ist, aber der **Workloadbesitzer** verfügt über die gleichen Berechtigungen. Der **Kontobesitzer** muss darauf vertrauen, dass der **Workloadbesitzer** Ressourcen bereitstellt, die sich innerhalb des Abonnementbudgets bewegen.

Die nächste Ebene des Verwaltungsbereichs ist die Ebene **Ressourcengruppe**. Eine Ressourcengruppe ist ein logischer Container für Ressourcen. Vorgänge, die auf der Ressourcengruppenebene angewendet werden, gelten für alle Ressourcen einer Gruppe. Es ist auch wichtig zu beachten, dass die Berechtigungen für die einzelnen Benutzer von der nächsthöheren Ebene geerbt werden, sofern sie in diesem Bereich nicht explizit geändert werden. 

Zur Verdeutlichung sehen wir uns an, was passiert, wenn der **Workloadbesitzer** eine Ressourcengruppe erstellt:

![**Workloadbesitzer** erstellt eine Ressourcengruppe](../_images/governance-1-7.png)
*Abbildung 7: Der Workloadbesitzer erstellt eine Ressourcengruppe und erbt die integrierte Rolle „Besitzer“ für den Ressourcengruppenbereich.*

Mit der integrierten Rolle **Besitzer** werden für den **Workloadbesitzer** wiederum alle Berechtigungen für den Ressourcengruppenbereich gewährt. Wie bereits beschrieben, wird diese Rolle von der Abonnementebene geerbt. Wenn diesem Benutzer für diesen Bereich eine andere Rolle zugewiesen wird, gilt dies nur für den Bereich.

Die niedrigste Ebene des Verwaltungsbereichs ist die Ebene **Ressource**. Vorgänge, die auf Ressourcenebene durchgeführt werden, gelten nur für die Ressource selbst. Auch hier werden die Berechtigungen der Ressourcenebene vom Ressourcengruppenbereich geerbt. Wir können uns beispielsweise ansehen, was passiert, wenn der **Workloadbesitzer** in der Ressourcengruppe ein [virtuelles Netzwerk](/azure/virtual-network/virtual-networks-overview) bereitstellt:

![**Workloadbesitzer** erstellt eine Ressource](../_images/governance-1-8.png)
*Abbildung 8: Der Workloadbesitzer erstellt eine Ressource und erbt die integrierte Rolle „Besitzer“ für den Ressourcenbereich.*

Der **Workloadbesitzer** erbt die Rolle „Besitzer“ für den Ressourcenbereich. Dies bedeutet, dass der Workloadbesitzer über alle Berechtigungen für das virtuelle Netzwerk verfügt.

## <a name="implementing-the-basic-resource-access-management-model"></a>Implementieren des grundlegenden Modells für die Ressourcenzugriffsverwaltung

Kommen wir zur Implementierung des zuvor entworfenen Governance-Modells. 

Ihre Organisation benötigt ein Azure-Konto, um beginnen zu können. Wenn Ihre Organisation über ein vorhandenes [Microsoft Enterprise Agreement](https://www.microsoft.com/licensing/licensing-programs/enterprise.aspx) verfügt, in dem Azure nicht enthalten ist, kann Azure hinzugefügt werden, indem Sie vorab eine Zahlungsverpflichtung eingehen. Weitere Informationen finden Sie unter [Lizenzierung von Azure für das Unternehmen](https://azure.microsoft.com/pricing/enterprise-agreement/). 

Beim Erstellen Ihres Azure-Kontos geben Sie eine Person in Ihrer Organisation als Azure-**Kontobesitzer** an. Anschließend wird standardmäßig ein Azure AD-Mandant (Azure Active Directory) erstellt. Ihr Azure-**Kontobesitzer** muss die [Erstellung des Benutzerkontos](/azure/active-directory/add-users-azure-active-directory) für die Person in Ihrer Organisation durchführen, die als **Workloadbesitzer** fungiert. 

Als Nächstes muss Ihr Azure-**Kontobesitzer** ein [Abonnement erstellen](https://docs.microsoft.com/partner-center/create-a-new-subscription) und diesem [den Azure AD-Mandanten zuordnen](/azure/active-directory/fundamentals/active-directory-how-subscriptions-associated-directory).

Nachdem das Abonnement erstellt und Ihr Azure AD-Mandant zugeordnet wurde, können Sie [den **Workloadbesitzer** dem Abonnement mit der integrierten Rolle **Besitzer** hinzufügen](/azure/billing/billing-add-change-azure-subscription-administrator#add-an-rbac-owner-for-a-subscription-in-azure-portal).

## <a name="next-steps"></a>Nächste Schritte
> [!div class="nextstepaction"]
> [Bereitstellen einer grundlegenden Workload in Azure](../infrastructure/basic-workload.md)
> [!div class="nextstepaction"]
> [Informationen zum Ressourcenzugriff für mehrere Teams](governance-multiple-teams.md)
