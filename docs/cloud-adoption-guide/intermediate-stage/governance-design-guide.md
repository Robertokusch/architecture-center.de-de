---
title: 'Governance-Entwurfshandbuch: Neue Entwicklung in Azure für mehrere Teams'
description: Enthält eine Anleitung zum Konfigurieren von Azure-Governance-Kontrollen für mehrere Teams, mehrere Workloads und mehrere Umgebungen.
author: petertay
ms.openlocfilehash: 05f1f9bb24af4f4da55b15c1aca2c71bc0b65411
ms.sourcegitcommit: b3d74d8a89b2224fc796ce0e89cea447af43a0d4
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/11/2018
ms.locfileid: "35291159"
---
# <a name="governance-design-guide-new-development-in-azure-for-multiple-teams"></a>Governance-Entwurfshandbuch: Neue Entwicklung in Azure für mehrere Teams

Der Zweck dieser Anleitung besteht darin, dass Sie sich mit dem Prozess für das Entwerfen eines Modells zur Ressourcenkontrolle (Governance) in Azure vertraut machen können.  Wir sehen uns eine Reihe von hypothetischen Governance-Anforderungen an und gehen dann mehrere Beispielimplementierungen durch, die diese Anforderungen erfüllen. 

Hierfür bestehen die folgenden Anforderungen:
* Identitätsverwaltung für mehrere Teams mit verschiedenen Anforderungen an den Ressourcenzugriff in Azure. Im System für die Identitätsverwaltung wird die Identität der folgenden Benutzer gespeichert:
  1. Die Person in unserer Organisation, die für den Besitz von **Abonnements** verantwortlich ist.
  2. Die Person in unserer Organisation, die für die **Ressourcen der freigegebenen Infrastruktur** verantwortlich ist, über die für unser lokales Netzwerk eine Verbindung mit einem virtuellen Azure-Netzwerk hergestellt wird. 
  3. Zwei Personen unserer Organisation, die für die Verwaltung einer **Workload** verantwortlich sind. 
* Unterstützung für mehrere **Umgebungen**. Wie bereits beschrieben, ist eine Umgebung eine logische Gruppierung von Ressourcen mit ähnlichen Verwaltungs- und Sicherheitsanforderungen. Hier sind drei Umgebungen erforderlich:
  1. Eine **Umgebung mit freigegebener Infrastruktur** mit Ressourcen, die von Workloads in anderen Umgebungen freigegeben werden. Beispiel: Ein virtuelles Netzwerk mit einem Gatewaysubnetz, das die Konnektivität für die lokale Umgebung bereitstellt.
  2. Eine **Produktionsumgebung** mit den restriktivsten Sicherheitsrichtlinien. Sie kann interne oder externe Workloads umfassen.
  3. Eine **Entwicklungsumgebung** für Proof-of-Concept- und Testarbeiten. Diese Umgebung verfügt über Richtlinien für die Bereiche Sicherheit, Konformität und Kosten, die sich von den Richtlinien in der Produktionsumgebung unterscheiden.
* Ein **Berechtigungsmodell der geringsten Berechtigung**, bei dem Benutzer standardmäßig nicht über Berechtigungen verfügen. Das Modell muss Folgendes unterstützen:
  * Einen einzelnen vertrauenswürdigen Benutzer im Abonnementbereich mit Berechtigung zum Zuweisen von Ressourcenzugriffsrechten.
  * Für jeden Workloadbesitzer wird der Zugriff auf Ressourcen standardmäßig erst einmal verweigert. Ressourcenzugriffsrechte werden explizit jeweils vom alleinigen vertrauenswürdigen Benutzer des Abonnementbereichs gewährt.
  * Der Verwaltungszugriff für die Ressourcen der freigegebenen Infrastruktur ist auf den Besitzer der freigegebenen Infrastruktur beschränkt.  
  * Der Verwaltungszugriff für jede Workload ist jeweils auf den Workloadbesitzer beschränkt.
  * Ausschließliche Verwendung von [integrierten RBAC-Rollen][rbac-built-in-roles]. Keine benutzerdefinierten RBAC-Rollen.
* Nachverfolgung von Kosten nach Name des Workloadbesitzers, Umgebung oder beidem. 

## <a name="identity-management"></a>Identitätsverwaltung

Bevor wir die Identitätsverwaltung für unser Governance-Modell entwerfen können, ist es wichtig, die vier damit verbundenen Hauptbereiche zu verstehen:

* Verwaltung: Die Prozesse und Tools zum Erstellen, Bearbeiten und Löschen der Benutzeridentität.
* Authentifizierung: Die Überprüfung der Benutzeridentität durch das Verifizieren der Anmeldeinformationen, z.B. Benutzername und Kennwort.
* Autorisierung: Die Ermittlung, auf welche Ressourcen ein authentifizierter Benutzer zugreifen darf oder für welche Vorgänge die Berechtigung zur Durchführung vorliegt.
* Überwachung: Regelmäßige Überprüfung von Protokollen und anderen Informationen, um Sicherheitsprobleme in Bezug auf die Benutzeridentität zu ermitteln. Hierzu gehören das Überprüfen von verdächtigen Nutzungsmustern, das regelmäßige Überprüfen von Benutzerberechtigungen, um deren Richtigkeit sicherzustellen, und andere Funktionen.

Für Azure gilt in Bezug auf die Identität nur ein Dienst als vertrauenswürdig: Azure Active Directory (AAD). Wir fügen Azure AD Benutzer hinzu und verwenden diese für alle oben aufgeführten Funktionen. Bevor wir uns die Vorgehensweise zur Konfiguration von Azure AD ansehen, sollten Sie mit den privilegierten Konten vertraut sein, die zum Verwalten des Zugriffs auf diese Dienste verwendet werden.

Wenn sich Ihre Organisation für ein Azure-Konto registriert hat, wurden mindestens ein Azure-**Kontobesitzer** und ein Azure AD-**Mandant** erstellt, falls für Ihre Organisation zur Nutzung von anderen Microsoft-Diensten, z.B. Office 365, nicht bereits ein Azure AD-Mandant zugeordnet war. Ein **globaler Administrator** mit vollständigen Berechtigungen auf dem Azure AD-Mandanten wurde während der Erstellung zugeordnet. 

Sowohl die Benutzeridentität des Azure-Kontobesitzers als auch des globalen Azure AD-Administrators wird in einem hochgradig sicheren Identitätssystem gespeichert, das von Microsoft verwaltet wird. Der Azure-Kontobesitzer ist dazu berechtigt, Abonnements zu erstellen, zu aktualisieren und zu löschen. Der globale Azure AD-Administrator ist zur Durchführung von vielen Aktionen in Azure AD berechtigt, aber in diesem Entwurfshandbuch konzentrieren wir uns auf das Erstellen und Löschen der Benutzeridentität. 

> [!NOTE]
> Unter Umständen verfügt Ihre Organisation bereits über einen Azure AD-Mandanten, falls Ihrem Konto eine vorhandene Office 365- oder Intune-Lizenz zugeordnet ist.

Der Azure-Kontobesitzer verfügt über die Berechtigung zum Erstellen, Aktualisieren und Löschen von Abonnements: 

![Azure-Konto mit Azure-Konto-Manager und globalem Azure AD-Administrator](../_images/governance-3-0.png)
*Abbildung 1: Azure-Konto mit einem Azure-Konto-Manager und globalem Azure AD-Administrator*

Der globale Azure AD-**Administrator** verfügt über die Berechtigung zum Erstellen von Benutzerkonten:  

![Azure-Konto mit Azure-Konto-Manager und globalem Azure AD-Administrator](../_images/governance-3-0a.png)
*Abbildung 2: Der globale Azure AD-Administrator erstellt die erforderlichen Benutzerkonten im Mandanten.*

Die ersten beiden Konten, **App1-Workloadbesitzer** und **App2-Workloadbesitzer**, sind jeweils einer Person in unserer Organisation zugeordnet, die für die Verwaltung einer Workload verantwortlich ist. Das Konto für den **Netzwerkbetrieb** befindet sich im Besitz der Person, die für die Ressourcen der freigegebenen Infrastruktur verantwortlich ist. Das Konto für den **Abonnementbesitzer** ist der Person zugeordnet, die für den Besitz von Abonnements verantwortlich ist.

## <a name="resource-access-permissions-model-of-least-privilege"></a>Ressourcenzugriff-Berechtigungsmodell der geringsten Berechtigung

Nachdem wir nun unser Identitätssystem und die Benutzerkonten erstellt haben, muss die Entscheidung getroffen werden, wie die Rollen der rollenbasierten Zugriffssteuerung (Role-Based Access Control, RBAC) auf die einzelnen Konten angewendet werden, um ein Berechtigungsmodell der geringsten Berechtigung zu unterstützen.

Die andere Anforderung besteht darin, dass die Ressourcen, die den einzelnen Workloads zugeordnet sind, voneinander isoliert werden, sodass kein Workloadbesitzer über Verwaltungszugriff auf andere Workloads verfügt, die sich nicht in seinem Besitz befinden. Es besteht die Anforderung, dass dieses Modell nur mit [integrierten RBAC-Rollen für Azure][rbac-built-in-roles] implementiert werden darf.

Jede RBAC-Rolle wird in Azure in einem von drei Bereichen angewendet: **Abonnement**, **Ressourcengruppe** und dann für eine einzelne **Ressource**. Rollen werden an untergeordnete Bereiche vererbt. Wenn ein Benutzer beispielsweise auf Abonnementebene der [integrierten Rolle „Besitzer“][rbac-built-in-owner] zugewiesen ist, wird diese Rolle dem Benutzer auch auf Ebene der Ressourcengruppe und der einzelnen Ressource zugewiesen, sofern dies nicht außer Kraft gesetzt wird.

Zum Erstellen eines Modells für den Zugriff mit der geringsten Berechtigung müssen wir entscheiden, welche Aktionen ein bestimmter Typ von Benutzer für diese drei Bereiche jeweils durchführen darf. Es besteht beispielsweise die Anforderung, dass ein Workloadbesitzer nur über die Berechtigung zum Verwalten des Zugriffs auf die Ressourcen verfügen darf, die seiner Workload zugeordnet sind, und nicht für andere Ressourcen. Wenn wir die [integrierte Rolle „Besitzer“][rbac-built-in-owner] im Abonnementbereich zuweisen, verfügt jeder Workloadbesitzer über Verwaltungszugriff auf alle Workloads.

Wir sehen uns zwei Beispiele für Berechtigungsmodelle an, um dieses Konzept etwas besser zu verstehen. Im ersten Beispiel wird in unserem Modell in Bezug auf die Erstellung von Ressourcengruppen nur der Dienstadministrator als vertrauenswürdig angesehen. Im zweiten Beispiel wird in unserem Modell jedem Workloadbesitzer im Abonnementbereich die integrierte Rolle „Besitzer“ zugewiesen. 

In beiden Beispielen wird ein Dienstadministrator des Abonnements der [integrierte Rolle „Besitzer“][rbac-built-in-owner] im Abonnementbereich zugewiesen. Wir erinnern uns, dass mit der [integrierte Rolle „Besitzer“][rbac-built-in-owner] alle Berechtigungen gewährt werden, einschließlich des Zugriffs auf Ressourcen.
![Dienstadministrator des Abonnements mit Rolle „Besitzer“](../_images/governance-2-1.png)
*Abbildung 3: Ein Abonnement mit einem Dienstadministrator, dem die integrierte Rolle „Besitzer“ zugewiesen ist.* 

1. In unserem ersten Beispiel verfügt **Workloadbesitzer A** über keine Berechtigungen für den Abonnementbereich. Er besitzt standardmäßig keine Verwaltungsrechte für den Ressourcenzugriff. Dieser Benutzer möchte die Ressourcen für seine Workload bereitstellen und verwalten. Er muss sich an den **Dienstadministrator** wenden, um die Erstellung einer Ressourcengruppe anzufordern.
![Workloadbesitzer fordert Erstellung von Ressourcengruppe A an](../_images/governance-2-2.png)  

2. Der **Dienstadministrator** prüft die Anforderung und erstellt die **Ressourcengruppe A**. **Workloadbesitzer A** verfügt an diesem Punkt immer noch nicht über Berechtigungen zur Durchführung von Aktionen.
![Dienstadministrator erstellt Ressourcengruppe A](../_images/governance-2-3.png)

3. Der **Dienstadministrator** fügt **Workloadbesitzer A** der **Ressourcengruppe A** hinzu und weist die [integrierte Rolle „Mitwirkender“](/azure/role-based-access-control/built-in-roles#contributor) zu. Mit der Rolle „Mitwirkender“ werden alle Berechtigungen für **Ressourcengruppe A** gewährt, mit Ausnahme der Berechtigung für die Verwaltung des Zugriffs.
![Dienstadministrator fügt Workloadbesitzer A der Ressourcengruppe A hinzu](../_images/governance-2-4.png)

4. Angenommen, für **Workloadbesitzer A** besteht die Anforderung, dass zwei Teammitglieder im Rahmen der Kapazitätsplanung für die Workload die Daten zur Überwachung der CPU und des Netzwerkdatenverkehrs anzeigen müssen. Da **Workloadbesitzer A** die Rolle „Mitwirkender“ zugewiesen wurde, verfügt er nicht über die Berechtigung zum Hinzufügen eines Benutzers zu **Ressourcengruppe A**. Er muss diese Anforderung an den **Dienstadministrator** senden.
![Workloadbesitzer fordert an, dass Mitwirkende der Workload der Ressourcengruppe hinzugefügt werden](../_images/governance-2-5.png)

5. Der **Dienstadministrator** überprüft die Anforderung und fügt **Ressourcengruppe A** die beiden **Mitwirkenden der Workload** hinzu. Da für keinen dieser beiden Benutzer eine Berechtigung zum Verwalten von Ressourcen erforderlich ist, wird ihnen die [integrierte Rolle „Leser“](/azure/role-based-access-control/built-in-roles#contributor) zugewiesen. 
![Dienstadministrator fügt Mitwirkende der Workload der Ressourcengruppe A hinzu](../_images/governance-2-6.png)

6. Als Nächstes benötigt **Workloadbesitzer B** außerdem eine Ressourcengruppe für die Ressourcen der Workload. Wie bei **Workloadbesitzer A** auch, verfügt **Workloadbesitzer B** anfänglich nicht über die Berechtigung zur Durchführung einer Aktion im Abonnementbereich, sodass er eine Anforderung an den **Dienstadministrator** senden muss. 
![Workloadbesitzer B fordert die Erstellung von Ressourcengruppe B an](../_images/governance-2-7.png)

7. Der **Dienstadministrator** prüft die Anforderung und erstellt **Ressourcengruppe B**. ![Dienstadministrator erstellt Ressourcengruppe B](../_images/governance-2-8.png)

8. Der **Dienstadministrator** fügt dann **Workloadbesitzer B** der **Ressourcengruppe B** hinzu und weist die [integrierte Rolle „Mitwirkender“](https://docs.microsoft.com/en-us/azure/role-based-access-control/built-in-roles#contributor) zu. 
![Dienstadministrator fügt Workloadbesitzer B der Ressourcengruppe B hinzu](../_images/governance-2-9.png)

An diesem Punkt ist jeder Workloadbesitzer in seiner eigenen Ressourcengruppe isoliert. Kein Workloadbesitzer oder dessen Teammitglieder verfügen über Verwaltungszugriff auf die Ressourcen in einer anderen Ressourcengruppe. 

![Abonnement mit Ressourcengruppen A und B](../_images/governance-2-10.png)
*Abbildung 4: Ein Abonnement mit zwei Workloadbesitzern, isoliert in ihrer eigenen Ressourcengruppe*

Dieses Modell ist ein Modell der geringsten Berechtigung: Jedem Benutzer wird jeweils die richtige Berechtigung für den richtigen Ressourcenverwaltungsbereich zugewiesen.

Bedenken Sie aber, dass jede Aufgabe in diesem Beispiel vom **Dienstadministrator** durchgeführt wurde. Dies ist zwar nur ein einfaches Beispiel, und es scheint kein Problem darzustellen, da es nur um zwei Workloadbesitzer geht. Es ist aber leicht vorstellbar, welche Probleme sich für eine große Organisation ergeben können. Beim **Dienstadministrator** können beispielsweise Engpässe auftreten, wenn sich eine große Zahl von Anforderungen staut und es zu Verzögerungen kommt.  

Wir sehen uns nun das zweite Beispiel an, bei dem die Anzahl von Aufgaben reduziert wird, die vom **Dienstadministrator** durchgeführt werden. 

1. Bei diesem Modell wird **Workloadbesitzer A** die [integrierte Rolle „Besitzer“][rbac-built-in-owner] für den Abonnementbereich zugewiesen, damit er seine eigene Ressourcengruppe erstellen kann: **Ressourcengruppe A**. ![Dienstadministrator fügt Workloadbesitzer A dem Abonnement hinzu](../_images/governance-2-11.png)

2. Beim Erstellen von **Ressourcengruppe A** wird **Workloadbesitzer A** standardmäßig hinzugefügt und erbt die [integrierte Rolle „Besitzer“][rbac-built-in-owner] vom Abonnementbereich.
![Workloadbesitzer A erstellt Ressourcengruppe A](../_images/governance-2-12.png)

3. Mit der [integrierten Rolle „Besitzer“][rbac-built-in-owner] wird **Workloadbesitzer A** die Berechtigung zum Verwalten des Zugriffs auf die Ressourcengruppe gewährt. **Workloadbesitzer A** fügt zwei **Mitwirkende für die Workload** hinzu und weist ihnen jeweils die [integrierte Rolle „Leser“][rbac-built-in-owner] zu. 
![Workloadbesitzer A fügt Mitwirkende der Workload hinzu](../_images/governance-2-13.png)

4. Nun fügt der **Dienstadministrator** den **Workloadbesitzer B** dem Abonnement mit der integrierten Rolle „Besitzer“ hinzu. 
![Dienstadministrator fügt Workloadbesitzer B dem Abonnement hinzu](../_images/governance-2-14.png)

5. **Workloadbesitzer B** erstellt die **Ressourcengruppe B** und wird standardmäßig hinzugefügt. **Workloadbesitzer B** erbt wieder die integrierte Rolle „Besitzer“ aus dem Abonnementbereich.
![Workloadbesitzer B erstellt Ressourcengruppe B](../_images/governance-2-15.png)

Beachten Sie bei diesem Modell Folgendes: Der **Dienstadministrator** hat weniger Aktionen als im ersten Beispiel durchgeführt, da der Verwaltungszugriff an die einzelnen Workloadbesitzer delegiert wurde.

![Abonnement mit Ressourcengruppen A und B](../_images/governance-2-16.png)
*Abbildung 5: Ein Abonnement mit einem Dienstadministrator und zwei Workloadbesitzern, denen jeweils die integrierte Rolle „Besitzer“ zugewiesen ist*

Da aber sowohl **Workloadbesitzer A** als auch **Workloadbesitzer B** im Abonnementbereich der integrierten Rolle „Besitzer“ zugewiesen ist, haben sie jeweils die integrierte Rolle „Besitzer“ für die Ressourcengruppe des anderen geerbt. Sie haben also nicht nur Vollzugriff auf die Ressourcen des anderen, sondern können auch den Verwaltungszugriff auf die Ressourcengruppen des jeweils anderen delegieren. Beispiel: **Workloadbesitzer B** verfügt über Rechte zum Hinzufügen von anderen Benutzern zu **Ressourcengruppe A** und kann diesen Benutzern beliebige Rollen zuweisen, einschließlich der integrierten Rolle „Besitzer“.

Wenn wir die Beispiele jeweils mit unseren Anforderungen vergleichen, sehen wir, dass bei beiden Beispielen ein einzelner vertrauenswürdiger Benutzer im Abonnementbereich mit der Berechtigung zum Gewähren von Ressourcenzugriffsrechten für die beiden Workloadbesitzer unterstützt wird. Standardmäßig hatten die beiden Workloadbesitzer keinen Zugriff auf die Ressourcenverwaltung und mussten den **Dienstadministrator** bitten, ihnen explizit Berechtigungen zuzuweisen. Aber nur im ersten Beispiel wird die Anforderung unterstützt, dass die den Workloads zugeordneten Ressourcen voneinander isoliert sind, damit kein Workloadbesitzer Zugriff auf die Ressourcen anderer Workloads hat.

## <a name="resource-management-model"></a>Ressourcenverwaltungsmodell

Nachdem wir nun ein Berechtigungsmodell der geringsten Berechtigung entworfen haben, werfen wir einen Blick auf einige praktische Anwendungen dieser Governance-Modelle. Gemäß unseren Anforderungen müssen wir die folgenden drei Umgebungen unterstützen:
1. **Freigegebene Infrastruktur:** Eine Gruppe von Ressourcen, die von allen Workloads genutzt wird. Beispiele für diese Ressourcen sind Netzwerkgateways, Firewalls und Sicherheitsdienste.  
2. **Entwicklung:** Mehrere Gruppen von Ressourcen, die für mehrere, nicht für die Produktion bereite Workloads stehen. Diese Ressourcen werden für Proof-of-Concept, Testing und andere Entwickleraktivitäten verwendet. Für diese Ressourcen kann ggf. ein weniger striktes Governance-Modell gelten, um die Flexibilität für Entwickler zu erhöhen.
3. **Produktion:** Mehrere Gruppen mit Ressourcen, die für mehrere Produktionsworkloads stehen. Diese Ressourcen werden verwendet, um die Anwendungselemente für den privaten und öffentlichen Bereich zu hosten. Diese Ressourcen verfügen normalerweise über die striktesten Governance- und Sicherheitsmodelle, um die Ressourcen, den Anwendungscode und die Daten vor unberechtigtem Zugriff zu schützen.

Für jede dieser drei Umgebungen besteht die Anforderung zum Nachverfolgen der Kostendaten nach **Workloadbesitzer**, **Umgebung** oder beidem. Wir möchten also die laufenden Kosten unserer **freigegebenen Infrastruktur**, die in den Umgebungen für die **Entwicklung** und **Produktion** anfallenden Kosten für Personen und dann die Gesamtkosten für die **Entwicklung** und die **Produktion** ermitteln. 

Sie haben bereits erfahren, dass diese Ressourcen in zwei Ebenen unterteilt sind: **Abonnement** und **Ressourcengruppe**. Als Erstes muss die Entscheidung getroffen werden, wie unsere Umgebungen nach **Abonnement** organisiert werden sollen. Es gibt nur zwei Möglichkeiten: ein einzelnes Abonnement oder mehrere Abonnements. 

Bevor wir uns mit Beispielen zu diesen einzelnen Modellen beschäftigen, sehen wir uns die Verwaltungsstruktur für Abonnements in Azure an. 

Sie kennen die Anforderungen und wissen, dass in unserer Organisation eine Person für Abonnements verantwortlich ist und dass dieser Benutzer über das Konto **Abonnementbesitzer** in unserem Azure AD-Mandanten verfügt. Dieses Konto verfügt aber nicht über die Berechtigung zum Erstellen von Abonnements. Nur der **Azure-Kontobesitzer** hat diese Berechtigung: ![](../_images/governance-3-0b.png)
*Abbildung 6: Ein Azure-Kontobesitzer erstellt ein Abonnement.*

Nachdem das Abonnement erstellt wurde, kann der **Azure-Kontobesitzer** das Konto **Abonnementbesitzer** dem Abonnement mit der Rolle **Besitzer** hinzufügen:

![](../_images/governance-3-0c.png)
*Abbildung 7: Der Azure-Kontobesitzer fügt das Benutzerkonto **Abonnementbesitzer** dem Abonnement mit der Rolle **Besitzer** hinzu.*

Der **Abonnementbesitzer** kann jetzt **Ressourcengruppen** erstellen und die Ressourcenzugriffsverwaltung delegieren.

Zuerst sehen wir uns ein Beispiel für ein Ressourcenverwaltungsmodell an, bei dem nur ein Abonnement verwendet wird. Als Erstes muss die Entscheidung getroffen werden, wie Ressourcengruppen nach den drei Umgebungen ausgerichtet werden sollen. Es gibt zwei Möglichkeiten:
1. Ausrichten jeder Umgebung auf eine bestimmte Ressourcengruppe. Alle Ressourcen der freigegebenen Infrastruktur werden für eine Ressourcengruppe der **freigegebenen Infrastruktur** bereitgestellt. Alle Ressourcen, die Entwicklungsworkloads zugeordnet sind, werden für eine Ressourcengruppe für die **Entwicklung** bereitgestellt. Alle Ressourcen, die Produktionsworkloads zugeordnet sind, werden für eine Ressourcengruppe für die **Produktion** bereitgestellt (Umgebung für die **Produktion**). 
2. Ausrichten von Workloads auf eine separate Ressourcengruppe, indem eine Benennungskonvention und Tags verwendet werden, um Ressourcengruppen den drei einzelnen Umgebungen zuzuordnen.  

Wir beginnen, indem wir die erste Option auswerten. Wir verwenden das Berechtigungsmodell, das im vorherigen Abschnitt beschrieben wurde, mit nur einem Dienstadministrator für das Abonnement, der Ressourcengruppen erstellt und ihnen Benutzer hinzufügt. Diese Benutzer verfügen entweder über die integrierte Rolle **Mitwirkender** oder **Leser**. 

1. Die erste bereitgestellte Ressourcengruppe steht für die Umgebung **Freigegebene Infrastruktur**. Der **Abonnementbesitzer** erstellt eine Ressourcengruppe für die Ressourcen der freigegebenen Infrastruktur mit dem Namen **netops-shared-rg**. 
![](../_images/governance-3-0d.png)
2. Der **Abonnementbesitzer** fügt das Konto des Benutzers für den **Netzwerkbetrieb** der Ressourcengruppe hinzu und weist die Rolle **Mitwirkender** zu. 
![](../_images/governance-3-0e.png)
3. Der Benutzer für den **Netzwerkbetrieb** erstellt ein [VPN-Gateway](/azure/vpn-gateway/vpn-gateway-about-vpngateways) und konfiguriert es so, dass eine Verbindung mit dem lokalen VPN-Gerät hergestellt werden kann. Der Benutzer für den **Netzwerkbetrieb** wendet auch ein [Tag](/azure/azure-resource-manager/resource-group-using-tags)-Paar auf die Ressourcen an: *environment:shared* und *managedBy:netOps*. Wenn der **Dienstadministrator für das Abonnement** einen Kostenbericht exportiert, werden die Kosten nach diesen Tags ausgerichtet. Dies ermöglicht es dem **Dienstadministrator für das Abonnement**, Kosten über das Tag *environment* und das Tag *managedBy* zu verteilen. Beachten Sie den Zähler für die **Ressourcengrenzwerte** oben rechts in der Abbildung. Jedes Azure-Abonnement verfügt über [Diensteinschränkungen](/azure/azure-subscription-service-limits), und damit Sie die Auswirkungen dieser Einschränkungen besser verstehen, behalten wir hier für jedes Abonnement den Grenzwert für virtuelle Netzwerke im Auge. Der Standardgrenzwert beträgt 50 virtuelle Netzwerke pro Abonnement, und nachdem das erste virtuelle Netzwerk bereitgestellt wurde, sind jetzt noch 49 verfügbar.
![](../_images/governance-3-1.png)
4. Zwei weitere Ressourcengruppen werden bereitgestellt. Die erste hat den Namen *prod-rg*. Diese Ressourcengruppe ist auf die Umgebung für die **Produktion** ausgerichtet. Die zweite Ressourcengruppe hat den Namen *dev-rg* und ist auf die Umgebung für die **Entwicklung** ausgerichtet. Alle Ressourcen, die Produktionsworkloads zugeordnet sind, werden in der Umgebung für die **Produktion** bereitgestellt, und alle Ressourcen, die Entwicklungsworkloads zugeordnet sind, werden in der Umgebung für die **Entwicklung** bereitgestellt. In diesem Beispiel stellen wir nur jeweils zwei Workloads für diese beiden Umgebungen bereit, sodass die Diensteinschränkungen für Azure-Abonnements hier nicht berührt werden. Es sollte aber berücksichtigt werden, dass für jede Ressourcengruppe ein Grenzwert von 800 Ressourcen pro Ressourcengruppe gilt. Wenn wir den Ressourcengruppen weitere Workloads hinzufügen, ist es also möglich, dass dieser Grenzwert erreicht wird. 
![](../_images/governance-3-2.png)
5. Der erste **Workloadbesitzer** sendet eine Anforderung an den **Dienstadministrator des Abonnements** und wird den einzelnen Ressourcengruppen der Umgebung für die **Entwicklung** und **Produktion** mit der Rolle **Mitwirkender** hinzugefügt. Wie bereits beschrieben, kann der Benutzer mit der Rolle **Mitwirkender** alle Vorgänge durchführen, mit Ausnahme der Zuweisung einer Rolle zu einem anderen Benutzer. Der erste **Workloadbesitzer** kann jetzt die Ressourcen erstellen, die seiner Workload zugeordnet sind.
![](../_images/governance-3-3.png)
6. Der erste **Workloadbesitzer** erstellt ein virtuelles Netzwerk in den beiden Ressourcengruppen mit jeweils einem VM-Paar. Der erste **Workloadbesitzer** wendet die Tags *environment* und *managedBy* auf alle Ressourcen an. Beachten Sie, dass laut Zähler für die Diensteinschränkung in Azure jetzt noch 47 weitere virtuelle Netzwerke zulässig sind.
![](../_images/governance-3-4.png)
7. Die einzelnen virtuellen Netzwerke verfügen nach der Erstellung nicht über eine Verbindung mit der lokalen Umgebung. Bei diesem Architekturtyp muss für jedes virtuelle Netzwerk ein Peering mit dem *hub-vnet* der Umgebung **Freigegebene Infrastruktur** eingerichtet werden. Beim VNet-Peering wird eine Verbindung zwischen zwei separaten virtuellen Netzwerken hergestellt, sodass dazwischen Netzwerkdatenverkehr fließen kann. Beachten Sie, dass das VNet-Peering nicht automatisch transitiv ist. Ein Peering muss jeweils in beiden virtuellen Netzwerken angegeben werden, für die die Verbindung hergestellt wird. Falls nur für eines der virtuellen Netzwerke ein Peering angegeben ist, ist die Verbindung unvollständig. Um diesen Effekt darzustellen, gibt der erste **Workloadbesitzer** ein Peering zwischen **prod-vnet** und **hub-vnet** an. Das erste Peering wird erstellt, aber es fließt kein Datenverkehr, da das entsprechende Peering von **hub-vnet** zu **prod-vnet** noch nicht angegeben wurde. Der erste **Workloadbesitzer** wendet sich an den Benutzer für den **Netzwerkbetrieb** und fordert das Gegenstück für die Peeringverbindung an.
![](../_images/governance-3-5.png)
8. Der Benutzer für den **Netzwerkbetrieb** überprüft die Anforderung, genehmigt sie und gibt dann das Peering in den Einstellungen für das **hub-vnet** an. Die Erstellung der Peeringverbindung ist jetzt abgeschlossen, und der Netzwerkdatenverkehr fließt zwischen den beiden virtuellen Netzwerken.
![](../_images/governance-3-6.png)
9. Nun sendet ein zweiter **Workloadbesitzer** eine Anforderung an den **Dienstadministrator des Abonnements** und wird den vorhandenen Ressourcengruppen der Umgebungen für die **Produktion** und **Entwicklung** mit der Rolle **Mitwirkender** hinzugefügt. Der zweite **Workloadbesitzer** verfügt in allen Ressourcengruppen jeweils über dieselben Berechtigungen für alle Ressourcen wie der erste **Workloadbesitzer**. 
![](../_images/governance-3-7.png)
10. Der zweite **Workloadbesitzer** erstellt ein Subnetz im virtuellen Netzwerk **prod-vnet** und fügt dann zwei virtuelle Computer hinzu. Der zweite **Workloadbesitzer** wendet die Tags *environment* und *managedBy* auf die einzelnen Ressourcen an.
![](../_images/governance-3-8.png) 

Mit diesem Beispiel für ein Ressourcenverwaltungsmodell können wir unsere Ressourcen in den drei erforderlichen Umgebungen verwalten. Unsere Ressourcen der freigegebenen Infrastruktur sind geschützt, da unter dem Abonnement nur ein Benutzer vorhanden ist, der über die Berechtigung zum Zugreifen auf diese Ressourcen verfügt. Jeder Workloadbesitzer kann die Ressourcen der freigegebenen Infrastruktur nutzen, ohne Berechtigungen für die eigentlichen freigegebenen Ressourcen haben zu müssen. Dieses Verwaltungsmodell verstößt gegen die Anforderung in Bezug auf die Isolation von Workloads: Jeder der beiden **Workloadbesitzer** kann auf die Ressourcen der Workload des anderen zugreifen. 

Für dieses Modell muss noch ein weiterer wichtiger Aspekt berücksichtigt werden, der ggf. nicht auf den ersten Blick erkennbar ist. In unserem Beispiel war es der **App1-Workloadbesitzer**, der die VNet-Peeringverbindung mit **hub-vnet** angefordert hat, um die Konnektivität für die lokale Umgebung bereitzustellen. Der Benutzer für den **Netzwerkbetrieb** hat diese Anforderung basierend auf den Ressourcen ausgewertet, die mit dieser Workload bereitgestellt wurden. Als der **Abonnementbesitzer** den **App2-Workloadbesitzer** mit der Rolle **Mitwirkender** hinzugefügt hat, hat dieser Benutzer über Verwaltungszugriffsrechte für alle Ressourcen in der Ressourcengruppe **prod-rg** verfügt. 

![](../_images/governance-3-10.png)

Dies bedeutet, dass der **App2-Workloadbesitzer** dazu berechtigt war, sein eigenes Subnetz mit virtuellen Computern im virtuellen Netzwerk **prod-vnet** bereitzustellen. Standardmäßig haben diese virtuellen Computer jetzt Zugriff auf das lokale Netzwerk. Der Benutzer für den **Netzwerkbetrieb** ist nicht über diese Computer informiert und hat deren Konnektivität für die lokale Umgebung nicht genehmigt.

Als Nächstes sehen wir uns ein einzelnes Abonnement mit mehreren Ressourcengruppen für unterschiedliche Umgebungen und Workloads an. Beachten Sie, dass die Ressourcen für die einzelnen Umgebungen im vorherigen Beispiel leicht identifizierbar waren, da sie sich in derselben Ressourcengruppe befunden haben. Nachdem diese Gruppierung nun nicht mehr vorhanden ist, müssen wir eine Namenskonvention für Ressourcengruppen nutzen, um diese Funktionalität bereitzustellen. 

1. Unsere Ressourcen für die **freigegebene Infrastruktur** verfügen bei diesem Modell weiterhin über eine separate Ressourcengruppe. Dies bleibt also unverändert. Für jede Workload sind zwei Ressourcengruppen erforderlich: jeweils eine für die Umgebung für die **Entwicklung** und die **Produktion**. Für die erste Workload erstellt der **Abonnementbesitzer** zwei Ressourcengruppen. Die erste hat den Namen **app1-prod-rg**, und die zweite hat den Namen **app1-dev-rg**. Wie bereits beschrieben, wird für die Ressourcen mit dieser Namenskonvention angegeben, dass sie der ersten Workload **app1** und entweder der Umgebung **dev** (Entwicklung) oder **prod** (Produktion) zugeordnet sind. Der *Abonnementbesitzer* fügt den **App1-Workloadbesitzer** der Ressourcengruppe mit der Rolle **Mitwirkender** hinzu.
![](../_images/governance-3-12.png)
2. Ähnlich wie im ersten Beispiel stellt der **App1-Workloadbesitzer** ein virtuelles Netzwerk mit dem Namen **app1-prod-vnet** in der Umgebung für die **Produktion** und ein weiteres mit dem Namen **app1-dev-vnet** in der Umgebung für die **Entwicklung** bereit. Der **App1-Workloadbesitzer** sendet eine Anforderung an den Benutzer für den **Netzwerkbetrieb**, um eine Peeringverbindung zu erstellen. Beachten Sie Folgendes: Der **App1-Workloadbesitzer** fügt die gleichen Tags wie im ersten Beispiel hinzu, und der Grenzwertzähler wurde auf 47 virtuelle Netzwerke verringert, die für das Abonnement verbleiben.
![](../_images/governance-3-13.png)
3. Der **Abonnementbesitzer** erstellt jetzt zwei Ressourcengruppen für den **App2-Workloadbesitzer**. Es werden die gleichen Konventionen wie für den **App1-Workloadbesitzer** befolgt, und die Ressourcengruppen erhalten die Namen **app2-prod-rg** und **app2-dev-rg**. Der **Abonnementbesitzer** fügt den **App2-Workloadbesitzer** den einzelnen Ressourcengruppen mit der Rolle **Mitwirkender** hinzu.
![](../_images/governance-3-14.png)
4. Der *App2-Workloadbesitzer* stellt virtuelle Netzwerke und virtuelle Computer für die Ressourcengruppen mit den gleichen Namenskonventionen bereit. Tags werden hinzugefügt, und der Grenzwertzähler wurde auf 45 virtuelle Netzwerke verringert, die für das *Abonnement* verbleiben.
![](../_images/governance-3-15.png)
5. Der *App2-Workloadbesitzer* sendet eine Anforderung an den Benutzer für den *Netzwerkbetrieb*, um für *app2-prod-vnet* das Peering mit *hub-vnet* herzustellen. Der Benutzer für den *Netzwerkbetrieb* erstellt die Peeringverbindung.
![](../_images/governance-3-16.png)

Das sich ergebende Verwaltungsmodell ähnelt dem Modell aus dem ersten Beispiel, aber es gibt auch einige wichtige Unterschiede:
* Jede der beiden Workloads ist nach Workload und Umgebung isoliert.
* Bei diesem Modell sind zwei virtuelle Netzwerke mehr als beim ersten Beispielmodell erforderlich. Wenn nur zwei Workloads verwendet werden, ist dies kein wichtiger Unterschied, aber der theoretische Grenzwert für die Anzahl von Workloads für dieses Modell ist 24. 
* Ressourcen werden nicht mehr für jede Umgebung in einer einzelnen Ressourcengruppe gruppiert. Zum Gruppieren von Ressourcen müssen Sie mit den Namenskonventionen vertraut sein, die für die einzelnen Umgebungen verwendet werden. 
* Jede VNet-Peeringverbindung wurde überprüft und vom Benutzer für den *Netzwerkbetrieb* genehmigt.

Wir sehen uns jetzt ein Modell für die Ressourcenverwaltung an, bei dem mehrere Abonnements verwendet werden. Bei diesem Modell richten wir die drei Umgebungen jeweils auf ein separates Abonnement aus: ein Abonnement vom Typ **Gemeinsame Dienste**, ein Abonnement für die **Produktion** und ein Abonnement für die **Entwicklung**. Für dieses Modell müssen ähnliche Aspekte wie für ein Modell mit nur einem Abonnement berücksichtigt werden, und wir müssen die Entscheidung treffen, wie Ressourcengruppen auf Workloads ausgerichtet werden. Wir haben bereits ermittelt, dass die Anforderung zur Isolation von Workloads durch die Erstellung einer Ressourcengruppe für jede Workload erfüllt wird. Daher verwenden wir dieses Modell auch in diesem Beispiel.

1. Bei diesem Modell gibt es drei *Abonnements*: *Freigegebene Infrastruktur*, *Produktion* und *Entwicklung*. Für diese drei Abonnements ist jeweils ein *Abonnementbesitzer* erforderlich, und in unserem einfachen Beispiel verwenden wir für alle drei das gleiche Benutzerkonto. Die Ressourcen für *Freigegebene Infrastruktur* werden auf ähnliche Weise wie in den ersten beiden Beispielen weiter oben verwaltet. Die erste Workload wird der Ressourcengruppe *app1-rg* in der Umgebung *Produktion* und der Ressourcengruppe gleichen Namens in der Umgebung *Entwicklung* zugeordnet. Der *App1-Workloadbesitzer* wird den Ressourcengruppen jeweils mit der Rolle *Mitwirkender* hinzugefügt. 
![](../_images/governance-3-17.png)
2. Wie in den obigen Beispielen auch, erstellt der *App1-Workloadbesitzer* die Ressourcen und fordert die Peeringverbindung mit dem virtuellen Netzwerk *Freigegebene Infrastruktur* an. Der *App1-Workloadbesitzer* fügt nur das Tag *managedBy* hinzu, da das Tag *environment* nicht mehr benötigt wird. Die Ressourcen für die einzelnen Umgebungen sind jetzt also unter demselben Abonnement (*subscription*) gruppiert, und das Tag *environment* ist überflüssig. Der Grenzwertzähler wird auf 49 verbleibende virtuelle Netzwerke verringert.
![](../_images/governance-3-18.png)
3. Abschließend wiederholt der *Abonnementbesitzer* den Prozess für die zweite Workload und fügt die Ressourcengruppen mit dem *App2-Workloadbesitzer* in der Rolle „Mitwirkender“ hinzu. Der Grenzwertzähler für die einzelnen Umgebungsabonnements wird auf 48 verbleibende virtuelle Netzwerke verringert. 

Für dieses Verwaltungsmodell gelten die Vorteile des zweiten oben angeführten Beispiels. Der entscheidende Unterschied besteht aber darin, dass Grenzwerte hier keine so große Rolle spielen, da sie auf zwei *Abonnements* verteilt sind. Der Nachteil ist, dass die über Tags nachverfolgten Kostendaten für alle drei *Abonnements* aggregiert werden müssen. 

Sie können die Wahl zwischen diesen beiden Beispielmodellen für die Ressourcenverwaltung also abhängig davon treffen, welche Anforderungen bei Ihnen Priorität haben. Wenn Sie der Meinung sind, dass Ihre Organisation die Dienstgrenzwerte für ein einzelnes Abonnement nicht erreicht, können Sie ein einzelnes Abonnement mit mehreren Ressourcengruppen verwenden. Falls für Ihre Organisation dagegen viele Workloads zu erwarten sind, sind mehrere Abonnements für die einzelnen Umgebungen unter Umständen besser geeignet.

<!-- ## Summary



## Next steps -->


<!-- links -->

[rbac-built-in-owner]: /azure/role-based-access-control/built-in-roles#owner
[rbac-built-in-roles]: /azure/role-based-access-control/built-in-roles
