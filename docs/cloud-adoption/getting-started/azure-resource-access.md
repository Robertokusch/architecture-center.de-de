---
title: 'Enterprise Cloud-Einführung: Ressourcenzugriffsverwaltung in Azure'
description: 'Erklärung der Konstrukte für die Verwaltung des Ressourcenzugriffs in Azure: Azure Resource Manager, Abonnements, Ressourcengruppen und Ressourcen'
author: petertaylor9999
ms.openlocfilehash: cd26b73e0327fa15b6ae29492b45331a19b9d6c2
ms.sourcegitcommit: ae8a1de6f4af7a89a66a8339879843d945201f85
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/31/2018
ms.locfileid: "43327488"
---
# <a name="enterprise-cloud-adoption-resource-access-management-in-azure"></a>Enterprise Cloud-Einführung: Ressourcenzugriffsverwaltung in Azure

In [Was ist die Ressourcenkontrolle?](what-is-governance.md) haben Sie erfahren, dass mit „Kontrolle“ (Governance) das fortlaufende Verwalten, Überwachen und Überprüfen der Nutzung von Azure-Ressourcen gemeint ist, um die Ziele und Anforderungen Ihrer Organisation zu erfüllen. Bevor Sie fortfahren und sich darüber informieren, wie Sie ein Modell für die Ressourcenkontrolle entwerfen, sollten Sie sich unbedingt mit den Kontrollen vertraut machen, die in Azure für die Ressourcenzugriffsverwaltung verwendet werden. Die Konfiguration dieser Kontrollen für die Ressourcenzugriffsverwaltung ist die Grundlage Ihres Modells für die Ressourcenkontrolle.

Zunächst sehen wir uns erst einmal genauer an, wie Ressourcen in Azure bereitgestellt werden. 

## <a name="what-is-an-azure-resource"></a>Was ist eine Azure-Ressource?

In Azure bezieht sich der Begriff **Ressource** auf eine von Azure verwaltete Entität. Beispielsweise werden virtuelle Computer, virtuelle Netzwerke und Speicherkonten allesamt als Azure-Ressourcen bezeichnet.

![](../_images/governance-1-9.png)   
*Abbildung 1: Ressource*

## <a name="what-is-an-azure-resource-group"></a>Was ist eine Azure-Ressourcengruppe?

In Azure muss jede Ressource zu einer [Ressourcengruppe](/azure/azure-resource-manager/resource-group-overview#resource-groups) gehören. Eine Ressourcengruppe ist einfach ein logisches Konstrukt, mit dem mehrere Ressourcen gruppiert werden, damit sie zusammen als Entität verwaltet werden können. Beispielsweise können Ressourcen, die über einen ähnlichen Lebenszyklus verfügen, z.B. die Ressourcen für eine [n-schichtige Anwendung](/azure/architecture/guide/architecture-styles/n-tier), als Gruppe erstellt oder gelöscht werden. 

![](../_images/governance-1-10.png)   
*Abbildung 2: Eine Ressourcengruppe enthält eine Ressource.* 

Ressourcengruppen und die darin enthaltenen Ressourcen sind einem Azure-**Abonnement** zugeordnet. 

## <a name="what-is-an-azure-subscription"></a>Was ist ein Azure-Abonnement?

Ein Azure-Abonnement ähnelt einer Ressourcengruppe darin, dass es sich um ein logisches Konstrukt handelt, unter dem Ressourcengruppen und die zugehörigen Ressourcen gruppiert werden. Darüber hinaus ist ein Azure-Abonnement aber auch noch den Kontrollen zugeordnet, die von Azure Resource Manager genutzt werden. Was bedeutet dies? Wir sehen uns Azure Resource Manager genauer an, um uns über die Beziehung dieses Diensts zu einem Azure-Abonnement zu informieren.

![](../_images/governance-1-11.png)   
*Abbildung 3: Azure-Abonnement*

## <a name="what-is-azure-resource-manager"></a>Was ist Azure Resource Manager?

Unter [Wie funktioniert Azure?](what-is-azure.md) haben Sie erfahren, dass Azure über ein „Front-End“ mit vielen Diensten verfügt, über die alle Funktionen von Azure orchestriert werden. Einer dieser Dienste ist der [Azure Resource Manager](/azure/azure-resource-manager/). Er hostet die RESTful-API, die von Clients zum Verwalten von Ressourcen verwendet wird. 

![](../_images/governance-1-12.png)   
*Abbildung 4: Azure Resource Manager*

In der folgenden Abbildung sind drei Clients dargestellt: [PowerShell](/powershell/azure/overview), [das Azure-Portal](https://portal.azure.com) und die [Azure-Befehlszeilenschnittstelle (Azure CLI)](/cli/azure):

![](../_images/governance-1-13.png)   
*Abbildung 5: Azure-Clients stellen eine Verbindung mit der RESTful-API von Azure Resource Manager her.*

Diese Clients stellen zwar über die RESTful-API eine Verbindung mit Azure Resource Manager her, aber Azure Resource Manager umfasst keine Funktionen zum direkten Verwalten von Ressourcen. Stattdessen verfügen die meisten Ressourcentypen in Azure über ihren eigenen [**Ressourcenanbieter**](/azure/azure-resource-manager/resource-group-overview#terminology). 

![](../_images/governance-1-14.png)   
*Abbildung 6: Azure-Ressourcenanbieter*

Wenn ein Client eine Anforderung zur Verwaltung einer bestimmten Ressource sendet, stellt Azure Resource Manager eine Verbindung mit dem Ressourcenanbieter für diesen Ressourcentyp her, damit die Anforderung abgeschlossen werden kann. Wenn ein Client beispielsweise eine Anforderung zur Verwaltung einer VM-Ressource sendet, stellt Azure Resource Manager eine Verbindung mit dem Ressourcenanbieter **Microsoft.compute** her. 

![](../_images/governance-1-15.png)   
*Abbildung 7: Azure Resource Manager stellt eine Verbindung mit dem Ressourcenanbieter **Microsoft.compute** her, um die in der Clientanforderung angegebene Ressource zu verwalten.*

Azure Resource Manager setzt voraus, dass der Client einen Bezeichner für das Abonnement sowie für die Ressourcengruppe angibt, damit die VM-Ressource verwaltet werden kann. 

Nachdem Sie sich mit der Funktionsweise von Azure Resource Manager vertraut gemacht haben, kehren wir zu der Beschreibung zurück, wie ein Azure-Abonnement den von Azure Resource Manager verwendeten Kontrollen zugeordnet ist. Bevor eine Anforderung zur Durchführung der Ressourcenverwaltung von Azure Resource Manager ausgeführt werden kann, werden einige Kontrollmechanismen überprüft. 

Die erste Kontrolle besteht darin, dass eine Anforderung von einem geprüften Benutzer durchgeführt werden muss und Azure Resource Manager über eine vertrauenswürdige Beziehung mit [Azure Active Directory (Azure AD)](/azure/active-directory/) verfügt, um die Funktionen für die Benutzeridentität bereitstellen zu können.

![](../_images/governance-1-16.png)   
*Abbildung 8: Azure Active Directory*

In Azure AD werden Benutzer in **Mandanten** segmentiert. Ein Mandant ist ein logisches Konstrukt, das eine sichere, dedizierte Instanz von Azure AD darstellt, die in der Regel einer Organisation zugeordnet ist. Jedes Abonnement ist einem Azure AD-Mandanten zugeordnet.

![](../_images/governance-1-17.png)   
*Abbildung 9: Ein Azure AD-Mandant, der einem Abonnement zugeordnet ist.*

Für jede Clientanforderung zur Verwaltung einer Ressource unter einem bestimmten Abonnement ist es erforderlich, dass der Benutzer im zugeordneten Azure AD-Mandanten über ein Konto verfügt. 

Die nächste Kontrolle ist eine Überprüfung, ob der Benutzer über ausreichende Berechtigungen zum Senden der Anforderungen verfügt. Berechtigungen werden Benutzern über die [rollenbasierte Zugriffssteuerung (RBAC)](/azure/role-based-access-control/) zugewiesen.

![](../_images/governance-1-18.png)   
*Abbildung 10: Jedem Benutzer im Mandanten wird mindestens eine RBAC-Rolle zugewiesen.*

Mit einer RBAC-Rolle wird ein Satz mit Berechtigungen angegeben, die einem Benutzer für eine bestimmte Ressource zur Verfügung stehen. Wenn die Rolle dem Benutzer zugewiesen wird, werden diese Berechtigungen angewendet. Mit der [integrierten Rolle**Besitzer**](/azure/role-based-access-control/built-in-roles#owner) kann ein Benutzer für eine Ressource beispielsweise alle Aktionen durchführen.

Die nächste Kontrolle umfasst eine Überprüfung, ob die Anforderung gemäß den Einstellungen, die für die [Azure-Ressourcenrichtlinie](/azure/azure-policy/) angegeben wurden, zulässig ist. Mit Azure-Ressourcenrichtlinien werden die Vorgänge angegeben, die für eine bestimmte Ressource zulässig sind. Mithilfe einer Azure-Ressourcenrichtlinie kann beispielsweise angegeben werden, dass Benutzer nur einen bestimmten Typ eines virtuellen Computers bereitstellen dürfen.

![](../_images/governance-1-19.png)   
*Abbildung 11: Azure-Ressourcenrichtlinie*

Mit der nächsten Kontrolle wird sichergestellt, dass für die Anforderung kein [Grenzwert für das Azure-Abonnement](/azure/azure-subscription-service-limits) überschritten wird. Beispielsweise gilt für alle Abonnements ein Grenzwert von 980 Ressourcengruppen pro Abonnement. Wenn eine Anforderung zur Bereitstellung einer weiteren Ressourcengruppe eingeht, wird dies verweigert, falls der Grenzwert bereits erreicht ist.

![](../_images/governance-1-20.png)   
*Abbildung 12: Azure-Ressourcengrenzwerte* 

Die letzte Kontrolle umfasst eine Überprüfung, ob sich die Anforderung innerhalb der Zahlungsverpflichtung des Abonnements bewegt. Wenn es bei der Anforderung beispielsweise um die Bereitstellung eines virtuellen Computers geht, überprüft Azure Resource Manager, ob das Abonnement über ausreichende Zahlungsinformationen verfügt.

![](../_images/governance-1-21.png)   
*Abbildung 13: Einem Abonnement ist eine Zahlungsverpflichtung zugeordnet.*

## <a name="summary"></a>Zusammenfassung

In diesem Artikel wurde beschrieben, wie der Zugriff auf Ressourcen in Azure mit Azure Resource Manager verwaltet wird.

## <a name="next-steps"></a>Nächste Schritte

Nachdem Sie nun wissen, wie der Ressourcenzugriff in Azure verwaltet wird, können Sie sich darüber informieren, wie Sie ein Governance-Modell [für ein einzelnes Team](../governance/governance-single-team.md) oder [für mehrere Teams](../governance/governance-multiple-teams.md) entwerfen, die diese Dienste verwenden.

> [!div class="nextstepaction"]
> [Governance-Übersicht](../governance/overview.md)
