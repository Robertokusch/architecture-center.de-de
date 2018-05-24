---
title: Bewährte Methoden für Unternehmen, die auf Azure umstellen| Microsoft Docs
description: Beschreibt ein Gerüst, mit dem Unternehmen eine sichere und verwaltbare Umgebung sicherstellen können.
services: azure-resource-manager
documentationcenter: na
author: rdendtler
manager: timlt
editor: tysonn
ms.assetid: 8692f37e-4d33-4100-b472-a8da37ce628f
ms.service: azure-resource-manager
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 03/31/2017
ms.author: rodend;karlku;tomfitz
ms.openlocfilehash: 8bc2ed253b951ea04eb25b76f67dcc2092dc94ad
ms.sourcegitcommit: 4ec010846b9b5545c843a32e08293f906e512302
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/17/2018
---
# <a name="azure-enterprise-scaffold---prescriptive-subscription-governance"></a>Azure-Unternehmensgerüst – präskriptive Abonnementgovernance
Immer mehr Unternehmen führen für Mobilität und Flexibilität eine öffentliche Cloud ein. Sie nutzen die Stärken der Cloud, um Umsatz zu generieren oder Ressourcen für das Unternehmen zu optimieren. Microsoft Azure bietet eine Vielzahl von Diensten, die Unternehmen wie Bausteine zusammenstellen können, um ein großes Spektrum von Workloads und Anwendungen zu berücksichtigen. 

Allerdings ist der Anfang oft schwierig. Nachdem der Entschluss gefasst wurde, Azure zu verwenden, ergeben sich häufig einige Fragen:

* Wie erfülle ich die rechtlichen Vorgaben hinsichtlich der Datenhoheit in bestimmten Ländern?
* Wie stelle ich sicher, dass kritische Systeme nicht versehentlich geändert werden?
* Wie erfahre ich, was die einzelnen Ressourcen unterstützen, damit ich sie genau zuordnen und abrechnen kann?

Die Vorstellung eines leeren Abonnements ohne Schutzmaßnahmen wirkt abschreckend und kann Ihre Umstellung auf Azure behindern.

Dieser Artikel bietet einen Ausgangspunkt für technische-Experten, die die Notwendigkeit von Governance berücksichtigen und auf die Notwendigkeit von Agilität abstimmen müssen. Er führt das Konzept eines Unternehmensgerüsts ein, das Organisationen beim Implementieren und Verwalten von Azure-Abonnements hilft. 

## <a name="need-for-governance"></a>Notwendigkeit von Governance
Beim Wechsel zu Azure müssen Sie das Thema Governance früh angehen, um die erfolgreiche Verwendung der Cloud innerhalb des Unternehmens sicherzustellen. Leider bedeuten die Zeit und der Aufwand beim Erstellen eines umfassenden Systems für Governance, dass sich einige Unternehmensgruppen direkt an Lieferanten wenden, ohne die Unternehmens-IT einzubeziehen. Durch diesen Ansatz können im Unternehmen Sicherheitsrisiken bestehen bleiben, wenn die Ressourcen nicht richtig verwaltet werden. Die Eigenschaften der öffentlichen Cloud – Agilität, Flexibilität und nutzungsbasierte Preise – sind wichtig für Unternehmensgruppen, die schnell die Anforderungen von Kunden (intern und extern) erfüllen müssen. Aber die Unternehmens-IT muss sicherstellen, dass Daten und Systeme effektiv geschützt sind.

In der Praxis wird ein Gerüst verwendet, um die Basis der Struktur zu erstellen. Das Gerüst gibt die allgemeine Gliederung vor und bietet Ankerpunkte für die Bereitstellung permanenterer Systeme. Ein Unternehmensgerüst entspricht einer Reihe von flexiblen Steuerelementen und Azure-Funktionen, die der Umgebung eine Struktur geben, sowie Ankern für die in der öffentlichen Cloud erstellten Dienste. Es bietet den Entwicklern (IT und Unternehmensgruppen) eine Grundlage zum Erstellen und Einbinden von neuen Diensten.

Das Gerüst basiert auf Methoden, die wir bei der Zusammenarbeit mit vielen Kunden verschiedener Größen gesammelt haben. Zu diesen Kunden gehören kleine Unternehmen, die Lösungen in der Cloud entwickeln, aber auch Fortune 500-Unternehmen und unabhängige Softwarehersteller, die Lösungen in die Cloud migrieren und dort entwickeln. Das Unternehmensgerüst wurde speziell so flexibel entwickelt, dass es sowohl herkömmliche IT-Workloads als auch agile Workloads unterstützt, wie z.B. Entwickler, die Software-as-a-Service-Anwendungen (SaaS) auf der Grundlage von Azure-Funktionen erstellen.

Das Unternehmensgerüst ist als Basis für jedes neue Abonnement in Azure vorgesehen. Administratoren können damit sicherstellen, dass Workloads in einer Organisation die minimalen Anforderungen an Governance erfüllen, ohne zu verhindern, dass Unternehmensgruppen und Entwickler schnell ihre eigenen Ziele erreichen.

> [!IMPORTANT]
> Governance ist entscheidend für den Erfolg von Azure. In diesem Artikel wird die technische Implementierung eines Unternehmensgerüsts behandelt, auf den umfassenden Prozess und die Beziehungen zwischen den Komponenten wird jedoch nur am Rande eingegangen. Die Richtliniengovernance verläuft von oben nach unten und richtet sich danach, was das Unternehmen erreichen möchte. Natürlich sind beim Erstellen eines Governancemodells für Azure Vertreter aus der IT beteiligt, wichtiger jedoch ist eine nachhaltige Unterstützung durch Führungskräfte von Unternehmensgruppen sowie aus dem Bereich Sicherheits- und Risikomanagement. Bei einem Unternehmensgerüst geht es letztlich um die Minderung von Unternehmensrisiken, um Mission und Ziele der Organisation zu fördern.
> 
> 

Die folgende Abbildung beschreibt die Komponenten des Gerüsts. Das Fundament ist ein gut durchdachter Plan für Abteilungen, Konten und Abonnements. Die Säulen bestehen aus Resource Manager-Richtlinien und soliden Benennungsstandards. Das restliche Gerüst bilden zentrale Azure-Funktionen und -Features, die eine sichere und verwaltbare Umgebung ermöglichen.

![Gerüstkomponenten](./_images/components.png)

> [!NOTE]
> Seit der Einführung im Jahr 2008 ist Azure schnell gewachsen. Aufgrund dieses Wachstums mussten die Microsoft-Entwicklerteams ihre Herangehensweise beim Verwalten und Bereitstellen von Diensten überdenken. Das Azure Resource Manager-Modell wurde 2014 eingeführt und ersetzt das klassische Bereitstellungsmodell. Mit Resource Manager können Organisationen Azure-Ressourcen leichter bereitstellen, organisieren und steuern. Resource Manager umfasst eine Parallelisierung beim Erstellen von Ressourcen, sodass komplexe, voneinander abhängige Lösungen schneller bereitgestellt werden können. Dazu gehört auch eine präzise Zugriffssteuerung und die Möglichkeit, Ressourcen mit Metadaten zu markieren. Microsoft empfiehlt, alle Ressourcen mit dem Resource Manager-Modell zu erstellen. Das Unternehmensgerüst wurde explizit für das Resource Manager-Modell konzipiert.
> 
> 

## <a name="define-your-hierarchy"></a>Definieren der Hierarchie
Die Grundlage für das Gerüst ist die Unternehmensregistrierung bei Azure (und im Enterprise Portal). Die Unternehmensregistrierung definiert die Form und die Nutzung der Azure-Dienste in einem Unternehmen und die zentrale Governancestruktur. Im Rahmen des Enterprise Agreement können Kunden die Umgebung in Abteilungen, Konten und Abonnements weiter unterteilen. Ein Azure-Abonnement ist die grundlegende Einheit, in der alle Ressourcen enthalten sind. Es definiert außerdem einige Beschränkungen in Azure, wie z.B. die Anzahl der Kerne, Ressourcen usw.

![Hierarchie](./_images/agreement.png)

Alle Unternehmen sind unterschiedlich, und in der Hierarchie der vorherigen Abbildung wird eine enorme Flexibilität im Hinblick auf den Aufbau von Azure im Unternehmen berücksichtigt. Vor der Implementierung der in diesem Dokument enthaltenen Richtlinien sollten Sie Ihre Hierarchie modellieren und die Auswirkungen auf die Abrechnung, den Zugriff auf Ressourcen und die Komplexität verstehen.

Die drei allgemeinen Muster für Azure-Registrierungen sind:

* Das auf **Funktionen** basierende Muster
  
    ![functional](./_images/functional.png)
* Das auf **Unternehmenseinheiten** basierende Muster 
  
    ![business](./_images/business.png)
* Das auf **geografischen Regionen** basierende Muster
  
    ![geographic](./_images/geographic.png)

Sie wenden das Gerüst auf Abonnementebene an, um die Anforderungen an Governance in Ihrem Unternehmen auf das Abonnement auszudehnen.

## <a name="naming-standards"></a>Benennungsstandards
Die erste Säule des Gerüsts sind Benennungsstandards. Mit sorgfältig konzipierten Benennungsstandards können Sie Ressourcen im Portal, auf einer Rechnung und innerhalb von Skripts identifizieren. Wahrscheinlich verwenden Sie bereits Benennungsstandards für die lokale Infrastruktur. Wenn Sie Azure zu Ihrer Umgebung hinzufügen, sollten Sie diese Benennungsstandards für Azure-Ressourcen übernehmen. Benennungsstandards ermöglichen eine effizientere Verwaltung der Umgebung auf allen Ebenen.

> [!TIP]
> Informationen zu Benennungskonventionen:
> * Lesen Sie den [Leitfaden mit Mustern und Verfahren](../best-practices/naming-conventions.md), und übernehmen Sie ihn wo möglich. Dieser Leitfaden hilft Ihnen bei der Entscheidung für einen sinnvollen Benennungsstandard.
> * Verwenden Sie die Höckerschreibweise für Namen von Ressourcen (z.B. meineRessourcengruppe und VnetNetzwerkname). Hinweis: Es gibt bestimmte Ressourcen, z.B. Speicherkonten, bei denen nur Kleinbuchstaben (und keine anderen Sonderzeichen) verwendet werden dürfen.
> * Sie könnten Azure Resource Manager-Richtlinien (im nächsten Abschnitt beschrieben) nutzen, um die Benennungsstandards zu erzwingen.
> 
> Die vorstehenden Tipps sollen Ihnen das Implementieren einer einheitlichen Benennungskonvention erleichtern.

## <a name="policies-and-auditing"></a>Richtlinien und Überwachung
Die zweite Säule des Gerüsts umfasst das Erstellen von [Azure-Richtlinien](/azure/azure-policy/azure-policy-introduction) und das [Überwachen des Aktivitätsprotokolls](/azure/azure-resource-manager/resource-group-audit). Resource Manager-Richtlinien bieten Ihnen die Möglichkeit, mit Risiken in Azure umzugehen. Sie können Richtlinien definieren, die die Datenhoheit sicherstellen, indem Sie bestimmte Aktionen einschränken, erzwingen oder überwachen. 

* Richtlinien sind standardmäßig ein **Zulassungssystem**. Aktionen werden durch das Definieren und Zuweisen von Richtlinien zu Ressourcen gesteuert, die Aktionen für Ressourcen verweigern oder überwachen.
* Richtlinien werden von Richtliniendefinitionen in einer Richtliniendefinitionssprache (If-Then-Bedingungen) beschrieben.
* Sie erstellen Richtlinien mit Dateien, die mit JSON (JavaScript Object Notation) formatiert sind. Nach dem Definieren einer Richtlinie weisen Sie ihr einen bestimmten Bereich zu: Abonnement, Ressourcengruppe oder Ressource.

Richtlinien weisen mehrere Aktionen auf, die einen differenzierten Ansatz für Ihre Szenarien ermöglichen. Die Aktionen sind:

* **Verweigern**: Blockiert die Ressourcenanforderung
* **Überwachen**: Lässt die Anforderung zu, fügt jedoch eine Zeile zum Aktivitätsprotokoll hinzu (die für Warnungen oder zum Auslösen von Runbooks verwendet werden kann)
* **Anfügen**: Fügt angegebene Informationen zur Ressource hinzu. Beispiel: Wenn eine Ressource kein Tag „Kostenstelle“ aufweist, fügen Sie dieses Tag mit einem Standardwert hinzu.

### <a name="common-uses-of-resource-manager-policies"></a>Allgemeine Verwendungsmöglichkeiten von Resource Manager-Richtlinien
Azure Resource Manager-Richtlinien sind ein leistungsstarkes Tool im Azure-Toolkit. Sie können damit unerwartete Kosten vermeiden, durch Tags eine Kostenstelle für Ressourcen identifizieren und sicherstellen, dass die Anforderungen an die Compliance erfüllt werden. Wenn Richtlinien mit integrierten Überwachungsfeatures kombiniert werden, können Sie komplexe und flexible Lösungen herstellen. Richtlinien ermöglichen Unternehmen die Steuerung von Workloads der „herkömmlichen IT“ und von „agilen“ Workloads, z.B. das Entwickeln von Kundenanwendungen. Die am häufigsten verwendeten Muster für Richtlinien sind:

* **Geografische Compliance/Datenhoheit**: Azure bietet Regionen auf der ganzen Welt. Unternehmen möchten häufig steuern, wo Ressourcen erstellt werden (um Datenhoheit sicherzustellen oder nur um sicherzustellen, dass Ressourcen in der Nähe der Endbenutzer der Ressourcen erstellt werden).
* **Kostenverwaltung**: Ein Azure-Abonnement kann Ressourcen vieler Typen und Größen enthalten. Unternehmen möchten häufig sicherstellen, dass für Standardabonnements keine unnötig umfangreichen Ressourcen verwendet werden, die pro Monat hohe Kosten verursachen.
* **Standardgovernance durch erforderliche Tags**: Das Anfordern von Tags ist eine der Funktionen, die am häufigsten verwendet und gewünscht wird. Mithilfe von Azure Resource Manager-Richtlinien können Unternehmen sicherstellen, dass eine Ressource entsprechend mit Tags versehen wird. Die am häufigsten verwendeten Tags sind: Abteilung, Ressourcenbesitzer und Umgebungstyp (z.B. Produktion, Test, Entwicklung)

**Beispiele**

Abonnement der „herkömmlichen IT“ für Branchenanwendungen

* Erzwingen der Tags für Abteilung und Besitzer für alle Ressourcen
* Beschränken der Erstellung von Ressourcen auf die Region Nordamerika
* Beschränken der Möglichkeit, virtuelle Computer der G-Serie und HDInsight-Cluster zu erstellen

„Agile“ Umgebung für eine Unternehmenseinheit zum Erstellen von Cloudanwendungen

* Zulassen der Erstellung von Ressourcen NUR in einer bestimmten Region, um die Anforderungen an die Datenhoheit zu erfüllen.
* Erzwingen des Umgebungstags für alle Ressourcen. Wenn eine Ressource ohne Tag erstellt wird, fügen Sie das Tag **Umgebung: Unbekannt** an die Ressource an.
* Überwachen, wenn Ressourcen außerhalb von Nordamerika erstellt werden, dies jedoch nicht verhindern.
* Überwachen, wenn Ressourcen erstellt werden, die hohe Kosten verursachen.

> [!TIP]
> Die häufigste Verwendung von Resource Manager-Richtlinien in Organisationen besteht darin zu steuern, *wo* Ressourcen erstellt werden können und *welche* Arten von Ressourcen erstellt werden können. Viele Unternehmen steuern mit Richtlinien nicht nur, *wo* und *welche* Ressourcen erstellt werden, sondern stellen auf diese Weise auch sicher, dass Ressourcen die entsprechenden Metadaten aufweisen, um die Nutzung abzurechnen. Sie sollten Richtlinien auf Abonnementebene für Folgendes anzuwenden:
> 
> * Geografische Compliance/Datenhoheit
> * Kostenverwaltung
> * Erforderliche Tags (festgelegt durch die Anforderungen des Unternehmens, z.B. für den Rechnungsempfänger oder den Anwendungsbesitzer)
> 
> Sie können weitere Richtlinien auf niedrigeren Ebenen des Bereichs anwenden.
> 
> 

### <a name="audit---what-happened"></a>Überwachen – was ist passiert?
Um anzuzeigen, wie Ihre Umgebung funktioniert, müssen Sie die Benutzeraktivitäten überwachen. Die meisten Ressourcentypen in Azure erstellen Diagnoseprotokolle, die Sie über ein Protokolltool oder in Azure Log Analytics analysieren können. Sie können Aktivitätsprotokolle aus mehreren Abonnements erfassen, um Abteilungs- oder Unternehmensansichten bereitzustellen. Überwachungsdatensätze sind ein wichtiges Tool für die Diagnose und ein entscheidender Mechanismus zum Auslösen von Ereignissen in der Azure-Umgebung.

Mit Aktivitätsprotokollen aus Resource Manager-Bereitstellungen können Sie bestimmen, welche **Vorgänge** ausgeführt wurden und wer sie ausgeführt hat. Aktivitätsprotokolle können mit Tools wie Log Analytics gesammelt und aggregiert werden.

## <a name="resource-tags"></a>Ressourcentags
Wenn Benutzer in Ihrer Organisation Ressourcen zum Abonnement hinzufügen, wird es zunehmend wichtig, die Ressourcen der entsprechenden Abteilung, dem Kunden und der Umgebung zuzuordnen. Sie können über [Tags](/azure/azure-resource-manager/resource-group-using-tags) Metadaten an Ressourcen anfügen. Verwenden Sie Tags, um Informationen zur Ressource oder zum Besitzer bereitzustellen. Mit Tags können Sie Ressourcen nicht nur auf verschiedene Arten aggregieren und gruppieren, Sie können diese Daten auch für die verbrauchsbasierte Kostenzuteilung verwenden. Sie können Ressourcen mit bis zu 15 Schlüssel-Wert-Paaren markieren. 

Ressourcentags sind flexibel und sollten den meisten Ressourcen angefügt werden. Beispiele für gängige Ressourcentags sind:

* BillTo
* Abteilung (oder Unternehmenseinheit)
* Umgebung (Produktion, Staging, Entwicklung)
* Ebene (Webebene, Anwendungsebene)
* Anwendungsbesitzer
* ProjectName

![tags](./_images/resource-group-tagging.png)

Weitere Beispiele für Tags finden Sie unter [Empfohlene Benennungskonventionen für Azure-Ressourcen](../best-practices/naming-conventions.md).

> [!TIP]
> Sie könnten eine Richtlinie erstellen, die Tags für Folgendes erfordert:
> 
> * Ressourcengruppen
> * Speicher
> * Virtual Machines
> * App Service-Umgebungen/Webserver
> 
> Durch diese Tagstrategie wird für alle Ihre Abonnements festgelegt, welche Metadaten für die Bereiche Unternehmen, Finanzen, Sicherheit, Risikomanagement und Verwaltung der vollständigen Umgebung benötigt werden. 

## <a name="resource-group"></a>Ressourcengruppe
Mit Resource Manager können Sie Ressourcen in aussagekräftige Gruppen für Verwaltung, Abrechnung oder natürliche Affinität aufnehmen. Wie bereits erwähnt, weist Azure zwei Bereitstellungsmodelle auf. Im älteren klassischen Modell war die grundlegende Verwaltungseinheit das Abonnement. Es war schwierig, Ressourcen innerhalb eines Abonnements aufzuschlüsseln. Dies hat zur Erstellung einer großen Anzahl von Abonnements geführt. Mit dem Resource Manager-Modell wurden auch Ressourcengruppen eingeführt. Ressourcengruppen sind Container für Ressourcen, die einen gemeinsamen Lebenszyklus aufweisen oder gleiche Attribute wie „alle SQL Server“ oder „Anwendung A“ verwenden.

Ressourcengruppen können nicht ineinander enthalten sein, und Ressourcen können nur einer einzigen Ressourcengruppe angehören. Sie können bestimmte Aktionen auf alle Ressourcen in einer Ressourcengruppe anwenden. Durch das Löschen einer Ressourcengruppe werden beispielsweise alle Ressourcen innerhalb der Ressourcengruppe entfernt. In der Regel nehmen Sie eine vollständige Anwendung oder ein verknüpftes System in die gleiche Ressourcengruppe auf. Beispielsweise würde eine Anwendung mit drei Ebenen namens „Contoso-Webanwendung“ den Webserver, den Anwendungsserver und den SQL-Server in der gleichen Ressourcengruppe enthalten.

> [!TIP]
> Wie Sie die Ressourcengruppen organisieren, kann je nach Workload der „herkömmlichen IT“ und der „agilen IT“ variieren:
> 
> * Workloads der „herkömmlichen IT“ werden meist nach Elementen innerhalb des gleichen Lebenszyklus gruppiert (z.B. eine Anwendung). Eine Gruppierung nach Anwendung ermöglicht die Verwaltung einzelner Anwendungen.
> * Workloads der „agilen IT“ konzentrieren sich in der Regel auf Cloudanwendungen, die externen Kunden zugänglich sind. Die Ressourcengruppen sollten die Ebenen der Bereitstellung (z.B. Webebene, App-Ebene) und der Verwaltung widerspiegeln.
> 
> Das Verständnis Ihrer Workloads erleichtert es Ihnen, eine Strategie für Ressourcengruppen zu entwickeln.

## <a name="role-based-access-control"></a>Rollenbasierte Zugriffssteuerung
Sie fragen sich wahrscheinlich, wer Zugriff auf Ressourcen haben sollte und wie dieser Zugriff gesteuert werden kann. Das Zulassen oder Untersagen von Zugriff auf das Azure-Portal und das Steuern des Zugriffs auf Ressourcen im Portal ist entscheidend. 

Als Azure ursprünglich veröffentlicht wurde, waren Zugriffssteuerungen für ein Abonnement einfach: Administrator oder Co-Administrator. Durch den Zugriff auf ein Abonnement im klassischen Modell war Zugriff auf alle Ressourcen im Portal möglich. Dieses Fehlen einer präziseren Steuerung hat zur Zunahme von Abonnements geführt, um eine angemessene Zugriffssteuerung für eine Azure-Registrierung bereitzustellen.

Diese Zunahme von Abonnements ist nicht mehr erforderlich. Mit der rollenbasierten Zugriffssteuerung können Sie Benutzer zu Standardrollen (z.B. zu den allgemeinen Rollentypen „Leser“ und „Autor“) zuweisen. Sie können auch benutzerdefinierte Rollen definieren.

> [!TIP]
> So implementieren Sie die rollenbasierte Zugriffssteuerung:
> * Verbinden Sie den Identitätsspeicher Ihres Unternehmens (meist Active Directory) über das AD Connect-Tool mit Azure Active Directory.
> * Kontrollieren Sie den Administrator/Co-Administrator eines Abonnements mithilfe einer verwalteten Identität. Weisen Sie den Administrator/Co-Administrator **nicht** einem neuen Abonnementbesitzer zu. Verwenden Sie stattdessen die RBAC-Rollen, um die Rechte eines **Besitzers** für eine Gruppe oder eine Einzelperson bereitzustellen.
> * Fügen Sie Azure-Benutzer einer Gruppe (z.B. Besitzer von Anwendung X) in Active Directory hinzu. Verwenden Sie die synchronisierte Gruppe, um Mitgliedern der Gruppe die erforderlichen Rechte zum Verwalten der Ressourcengruppe, die die Anwendung enthält, zu gewähren.
> * Befolgen Sie das Prinzip, die **geringsten Rechte** zu gewähren, die zur Erledigung der erwarteten Arbeit erforderlich sind. Beispiel: 
>   * Bereitstellungsgruppe: Eine Gruppe, die nur Ressourcen bereitstellen kann.
>   * Verwaltung virtueller Computer: Eine Gruppe, die virtuelle Computer (für Vorgänge) neu starten kann.
> 
> Diese Tipps sollen Sie beim Verwalten des Benutzerzugriffs für Ihr gesamtes Abonnement unterstützen.

## <a name="azure-resource-locks"></a>Azure-Ressourcensperren
Wenn Ihre Organisation dem Abonnement zentrale Dienste hinzufügt, wird es zunehmend wichtig sicherzustellen, dass diese Dienste verfügbar sind, um eine Unterbrechung des Geschäftsbetriebs zu vermeiden. [Ressourcensperren](/azure/azure-resource-manager/resource-group-lock-resources) ermöglichen Ihnen, Vorgänge auf wertvollen Ressourcen einzuschränken, bei denen das Ändern oder Löschen einen erheblichen Einfluss auf Ihre Anwendungen oder die Cloudinfrastruktur hätte. Sie können Sperren auf ein Abonnement, eine Ressourcengruppe oder eine Ressource anwenden. Normalerweise wenden Sie Sperren auf grundlegende Ressourcen wie Speicherkonten, virtuelle Netzwerke und Gateways an. 

Ressourcensperren unterstützen aktuell zwei Werte: CanNotDelete und ReadOnly. CanNotDelete bedeutet, dass Benutzer (mit den entsprechenden Berechtigungen) eine Ressource lesen oder ändern können, sie können sie jedoch nicht löschen. ReadOnly bedeutet, dass autorisierte Benutzer eine Ressource nicht löschen oder ändern können.

Zum Erstellen oder Löschen von Verwaltungssperren benötigen Sie Zugriff auf `Microsoft.Authorization/*`- oder `Microsoft.Authorization/locks/*`-Aktionen.
Unter den integrierten Rollen verfügen nur „Besitzer“ und „Benutzerzugriffsadministrator“ über diese Aktionen.

> [!TIP]
> Optionen des Kernnetzwerks sollten durch Sperren geschützt sein. Das versehentliche Löschen eines Site-to-Site-VPN mit Gateway wäre katastrophal für ein Azure-Abonnement. Azure lässt das Löschen eines virtuellen Netzwerks, das verwendet wird, nicht zu, aber die Anwendung weiterer Einschränkungen ist eine hilfreiche Sicherheitsmaßnahme. 
> 
> * Virtuelles Netzwerk: CanNotDelete
> * Netzwerksicherheitsgruppe: CanNotDelete
> * Richtlinien: CanNotDelete
> 
> Auch Richtlinien sind für die Aufrechterhaltung der entsprechenden Kontrolle entscheidend. Sie sollten eine **CanNotDelete**-Sperre auf Richtlinien anwenden, die verwendet werden.

## <a name="core-networking-resources"></a>Zentrale Netzwerkressourcen
Der Zugriff auf Ressourcen kann intern (innerhalb des Unternehmensnetzwerks) oder extern (über das Internet) erfolgen. Für Benutzer in Ihrer Organisation ist es einfach, versehentlich Ressourcen an der falschen Stelle zu platzieren und so böswilligen Zugriff darauf zu ermöglichen. Wie bei lokalen Geräten müssen Unternehmen die entsprechende Kontrolle implementieren, um sicherzustellen, dass Azure-Benutzer die richtigen Entscheidungen treffen. Für die Abonnementgovernance wurden Hauptressourcen identifiziert, die eine grundlegende Kontrolle des Zugriffs bieten. Zu diesen Hauptressourcen gehören:

* **Virtuelle Netzwerke** sind Containerobjekte für Subnetze. Obwohl dies nicht zwingend notwendig ist, werden sie häufig beim Verbinden von Anwendungen mit internen Unternehmensressourcen verwendet.
* **Netzwerksicherheitsgruppen** ähneln einer Firewall und bieten Regeln dafür, wie eine Ressource über das Netzwerk „kommunizieren“ kann. Mit ihnen können Sie präzise steuern, wie/ob ein Subnetz (oder ein virtueller Computer) eine Verbindung mit dem Internet oder anderen Subnetzen im gleichen virtuellen Netzwerk herstellen kann.

![core networking](./_images/core-network.png)

> [!TIP]
> Für Netzwerke:
> * Erstellen Sie virtuelle Netzwerke speziell für Workloads mit externem Zugriff und Workloads mit internem Zugriff. Dieser Ansatz reduziert die Wahrscheinlichkeit, dass versehentlich virtuelle Computer, die für interne Workloads vorgesehen sind, in einem Bereich mit externem Zugriff platziert werden.
> * Konfigurieren Sie Netzwerksicherheitsgruppen, um den Zugriff zu beschränken. Sie blockieren zumindest den Zugriff aus internen virtuellen Netzwerken auf das Internet und aus externen virtuellen Netzwerken auf das Unternehmensnetzwerk.
> 
> Diese Tipps sollen Sie beim Implementieren sicherer Netzwerkressourcen unterstützen.

### <a name="automation"></a>Automation
Das Verwalten von einzelnen Ressourcen ist zeitaufwändig und für bestimmte Vorgänge potenziell fehleranfällig. Azure bietet verschiedene Automatisierungsfunktionen, einschließlich Azure Automation, Logic Apps und Azure Functions. Mit [Azure Automation](/azure/automation/automation-intro) können Administratoren Runbooks erstellen und definieren, um häufige Aufgaben bei der Verwaltung von Ressourcen auszuführen. Sie erstellen Runbooks mithilfe eines PowerShell-Code-Editors oder eines grafischen Editors. Sie können komplexe mehrstufige Workflows erstellen. Azure Automation wird häufig verwendet, um allgemeine Aufgaben wie das Herunterfahren nicht verwendeter Ressourcen oder das Erstellen von Ressourcen als Reaktion auf einen bestimmten Trigger ohne Benutzereingriff auszuführen.

> [!TIP]
> Für Automation:
> * Erstellen Sie ein Azure Automation-Konto, und prüfen Sie die verfügbaren Runbooks (grafisch und Befehlszeile) im [Runbookkatalog](/azure/automation/automation-runbook-gallery).
> * Importieren Sie wichtige Runbooks, und passen Sie sie für Ihre eigenen Zwecke an.
> 
> Ein gängiges Szenario ist das Starten/Herunterfahren virtueller Computer nach einem Zeitplan. Es gibt Beispielrunbooks im Katalog, die dieses Szenario ausführen und veranschaulichen, wie Sie es erweitern können.
> 
> 

## <a name="azure-security-center"></a>Azure Security Center
Eines der größten Hindernisse bei der Einführung der Cloud waren vermutlich Bedenken wegen der Sicherheit. IT-Risikomanager und für die Sicherheit zuständige Abteilungen müssen sicherstellen, dass die Ressourcen in Azure sicher sind. 

Das [Azure Security Center](/azure/security-center/security-center-intro) bietet einen zentralen Überblick über den Sicherheitsstatus von Ressourcen in den Abonnements und gibt Empfehlungen, mit deren Hilfe die Gefährdung von Ressourcen verhindert wird. Es kann detailliertere Richtlinien ermöglichen (z.B. Richtlinien auf bestimmte Ressourcengruppen anwenden, mit denen das Unternehmen anpassen kann, wie es einem bestimmten Risiko ausgesetzt wird). Zudem ist das Azure Security Center eine offene Plattform, über die Microsoft-Partner und unabhängige Softwarehersteller Software erstellen können, die in das Azure Security Center eingebunden wird, um die Funktionen zu erweitern. 

> [!TIP]
> Das Azure Security Center ist standardmäßig in jedem Abonnement aktiviert. Allerdings müssen Sie die Datensammlung von virtuellen Computern aktivieren, damit das Azure Security Center den Agent installieren und mit dem Sammeln von Daten beginnen kann.
> 
> ![Datensammlung](./_images/data-collection.png)
> 
> 

## <a name="next-steps"></a>Nächste Schritte
* Nachdem Sie sich mit der Abonnementgovernance vertraut gemacht haben, ist es an der Zeit, diese Empfehlungen in der Praxis zu erleben. Informationen finden Sie unter [Beispiele für das Implementieren eines Azure-Unternehmensgerüsts](subscription-governance-examples.md).
