---
title: 'Framework für die Cloudeinführung (Cloud Adoption Framework, CAF): Kleine bis mittlere Unternehmen: Entwicklung der Sicherheitsbaseline'
titleSuffix: Microsoft Cloud Adoption Framework for Azure
ms.service: architecture-center
ms.subservice: enterprise-cloud-adoption
ms.custom: governance
ms.date: 02/11/2019
description: 'Erläuterung für kleine bis mittlere Unternehmen: Entwicklung der Sicherheitsbaseline'
author: BrianBlanchard
ms.openlocfilehash: 05bb2f7023c999cdf7154b8189a104ba06950720
ms.sourcegitcommit: c053e6edb429299a0ad9b327888d596c48859d4a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/20/2019
ms.locfileid: "58238814"
---
# <a name="caf-small-to-medium-enterprise-security-baseline-evolution"></a>Framework für die Cloudeinführung (Cloud Adoption Framework, CAF): Kleine bis mittlere Unternehmen: Entwicklung der Sicherheitsbaseline

In diesem Artikel wird die Geschichte weiterentwickelt. Es werden Sicherheitskontrollen hinzugefügt, die das Verschieben geschützter Daten in die Cloud unterstützen.

## <a name="evolution-of-the-narrative"></a>Weiterentwicklung der Geschichte

IT- und Unternehmensführung waren mit den Ergebnissen der frühen Experimente der IT-, App-Entwicklungs- und BI-Teams zufrieden. Um aus diesen Experimenten konkrete Geschäftswerte zu gewinnen, müssen diese Teams in die Lage versetzt werden, geschützte Daten in Lösungen zu integrieren. Dies löst Änderungen in der Unternehmensrichtlinie aus, erfordert aber auch eine Weiterentwicklung der Cloud Governance-Implementierungen, bevor geschützte Daten in die Cloud verschoben werden können.

### <a name="evolution-of-the-cloud-governance-team"></a>Entwicklung des Cloud Governance-Teams

Angesichts der Auswirkungen der sich verändernden Darstellung und der bisher geleisteten Unterstützung wird das Cloud Governance-Team nun anders beurteilt. Die beiden Systemadministratoren, die das Team gegründet haben, gelten heute als erfahrene Cloudarchitekten. Während sich dieser Geschichte entwickelt, wird sich die Wahrnehmung von ihnen verschieben: aus einer Cloudverwalter- zu einer Cloudwächterrolle.

Obwohl der Unterschied subtil ist, ist dies eine wichtige Unterscheidung beim Erstellen einer governanceorientierten IT-Kultur. Ein Cloudverwalter bereinigt die Unordnung, die innovative Cloudarchitekten hinterlassen haben. Die beiden Rollen besitzen eine natürliche Reibung und gegensätzliche Ziele. Auf der anderen Seite trägt ein Cloudwächter zur Sicherheit der Cloud bei, sodass andere Cloudarchitekten schneller und mit weniger Unordnung arbeiten können. Darüber hinaus ist ein Cloudwächter an der Erstellung von Vorlagen beteiligt, die die Bereitstellung und Einführung beschleunigen. Er wird daher zu einem Innovationsbeschleuniger sowie zu einem Verteidiger der fünf Disziplinen der Cloud-Governance.

### <a name="evolution-of-the-current-state"></a>Weiterentwicklung des aktuellen Status

Zu Beginn dieser Geschichte arbeiteten die Anwendungsentwicklungsteams noch mit Entwicklungs-/Testkapazität, und das BI-Team befand sich noch in der Experimentierphase. Die IT-Abteilung betrieb zwei gehostete Infrastrukturumgebungen namens „Prod“ und „DR“.

Seit diesem Zeitpunkt haben sich einige Dinge geändert, die sich auf die Governance auswirken:

- Das Anwendungsentwicklungsteam hat eine CI/CD-Pipeline implementiert, um eine cloudnative Anwendung mit einer verbesserten Benutzeroberfläche bereitzustellen. Diese Anwendung interagiert noch nicht mit geschützten Daten, daher ist sie nicht für die Produktion bereit.
- Das Business Intelligence-Team der IT-Abteilung pflegt aktiv Daten in der Cloud aus Logistik, Inventar und von Drittanbietern. Diese Daten werden verwendet, um neue Vorhersagen zu treffen, die Geschäftsprozesse beeinflussen könnten. Diese Vorhersagen und Erkenntnisse sind jedoch erst dann umsetzbar, wenn Kunden- und Finanzdaten in die Datenplattform integriert werden können.
- Das IT-Team setzt zurzeit die Pläne des CIO und des CFO um, um das DR-Datencenter außer Betrieb zu nehmen. Mehr als 1.000 der 2.000 Ressourcen im DR-Datencenter wurden außer Betrieb genommen oder migriert.
- Die lose definierten Richtlinien für PII und Finanzdaten wurden modernisiert. Allerdings sind die neuen Unternehmensrichtlinien abhängig von der Implementierung der entsprechenden Sicherheits- und Governancerichtlinien. Die Teams befinden sich immer noch im Stillstand.

### <a name="evolution-of-the-future-state"></a>Weiterentwicklung des zukünftigen Status

Frühe Experimente der Teams für Anwendungsentwicklung und Business Intelligence haben potenzielle Verbesserungen für die Kundenumgebung und datengestützte Entscheidungen aufgezeigt. Beide Teams möchten die Einführung der Cloud in den nächsten 18 Monaten erweitern, indem Sie diese Lösungen in der Produktionsumgebung bereitstellen.

In den verbleibenden sechs Monaten wird das Cloud Governance-Team Sicherheits- und Governanceanforderungen umsetzen, damit die Cloudeinführungsteams die geschützten Daten in diesen Datencentern migrieren können.

Die Änderungen des aktuellen und zukünftigen Status bergen neue Risiken, die neue Richtlinienanweisungen erfordern.

## <a name="evolution-of-tangible-risks"></a>Weiterentwicklung materieller Risiken

**Datenpanne**: Bei Einführung einer neuen Datenplattform gibt es eine inhärente Zunahme der Haftung im Zusammenhang mit potenziellen Datenpannen. Das technische Personal zur Einführung von Cloudtechnologien ist zunehmend dafür verantwortlich, dass Lösungen implementiert werden, die dieses Risiko senken können. Eine robuste Sicherheits- und Governancestrategie muss implementiert werden, um sicherzustellen, dass dieses technische Personal dieser Verantwortung nachkommt.

Dieses Unternehmensrisiko lässt sich auf einige technische Risiken erweitern:

- Unternehmenskritische Anwendungen oder geschützten Daten werden möglicherweise unbeabsichtigt bereitgestellt.
- Geschützte Daten könnten aufgrund falscher Verschlüsselungsentscheidungen im Speicher verfügbar gemacht werden.
- Nicht autorisierte Benutzer können möglicherweise auf geschützte Daten zugreifen.
- Ein Eindringen von außerhalb kann den Zugriff auf geschützte Daten ermöglichen.
- Ein Eindringen von außerhalb oder Denial-of-Service-Angriffe können zu einer Unterbrechung des Geschäftsbetriebs führen.
- Organisatorische Änderungen oder Personalwechsel können den unbefugten Zugriff auf geschützte Daten ermöglichen.
- Neue Exploits können neue Angriffs- oder Zugriffsmöglichkeiten schaffen.
- Inkonsistente Bereitstellungsprozesse können Sicherheitslücken herbeiführen, die zu Datenverlusten oder Unterbrechungen führen können.
- Konfigurationsabweichungen oder fehlende Patches können unbeabsichtigte Sicherheitslücken zur Folge haben, die zu Datenverlusten oder Unterbrechungen führen können.

## <a name="evolution-of-the-policy-statements"></a>Weiterentwicklung der Richtlinienanweisungen

Die folgenden Änderungen an der Richtlinie verringern die neuen Risiken und vereinfachen die Implementierung. Die Liste wirkt lang, aber die Einführung dieser Richtlinien ist möglicherweise einfacher, als es den Anschein hat.

1. Alle bereitgestellten Ressourcen müssen nach Wichtigkeit und Datenklassifizierung kategorisiert werden. Vor der Bereitstellung in der Cloud müssen die Klassifizierungen durch das Cloud Governance-Team und die Besitzer der Anwendung überprüft werden.
2. Anwendungen, die geschützte Daten speichern oder darauf zugreifen, sind anders zu verwalten als Anwendungen, die nicht mit geschützten Daten arbeiten. Zumindest müssen sie segmentiert werden, um einen unbeabsichtigten Zugriff auf geschützte Daten zu vermeiden.
3. Alle geschützten Daten müssen im Ruhezustand verschlüsselt sein.
4. Erhöhte Berechtigungen in einem Segment mit geschützten Daten müssen eine Ausnahme bleiben. Solche Ausnahmen werden vom Cloud Governance-Team erfasst und regelmäßig überwacht.
5. Netzwerksubnetze mit geschützten Daten müssen von allen anderen Subnetzen isoliert werden. Der Netzwerkdatenverkehr zwischen Subnetzen mit geschützten Daten wird regelmäßig überwacht.
6. Kein Subnetz mit geschützten Daten ist direkt über das öffentliche Internet oder datencenterübergreifend zugänglich. Der Zugriff auf diese Subnetze muss über zwischengeschaltete Subnetze geroutet werden. Der gesamte Zugriff auf diese Subnetze muss über eine Firewalllösung erfolgen, die Funktionen zur Paketüberprüfung und Sperrfunktionen durchführen kann.
7. Die Anforderungen an die Netzwerkkonfiguration, die vom Sicherheitsverwaltungsteam definiert wurden, müssen mit Governancetools überwacht und durchgesetzt werden.
8. Governancetools müssen die VM-Bereitstellung auf genehmigte Images begrenzen.
9. Nach Möglichkeit muss die Knotenkonfigurationsverwaltung Richtlinienanforderungen auf die Konfiguration aller Gastbetriebssysteme anwenden.
10. Governancetools müssen durchsetzen, dass automatische Updates für alle bereitgestellten Ressourcen aktiviert sind. Verstöße auf Knotenebene müssen von Betriebsmanagementteams überprüft und in Übereinstimmung mit den Betriebsrichtlinien beseitigt werden. Ressourcen, die nicht automatisch aktualisiert werden, müssen in Prozesse des IT-Betriebs einbezogen werden.
11. Für die Erstellung neuer Abonnements oder Verwaltungsgruppen für unternehmenskritische Anwendungen oder geschützte Daten ist eine Überprüfung durch das Cloud Governance-Team erforderlich, um sicherzustellen, dass die richtige Blaupause zugewiesen wird.
12. Das Zugriffsmodell der geringsten Rechte wird auf alle Verwaltungsgruppen oder Abonnements angewendet, die unternehmenskritische Apps oder geschützte Daten enthalten.
13. Trends und Exploits, die mögliche Auswirkungen auf Cloudbereitstellungen haben, müssen vom Sicherheitsteam regelmäßig überprüft werden, damit Updates für in der Cloud verwendete Sicherheitsverwaltungstools bereitgestellt werden.
14. Bereitstellungstools müssen vom Cloud Governance-Team genehmigt werden, um eine kontinuierliche Governance für bereitgestellte Ressourcen sicherzustellen.
15. Bereitstellungsskripts müssen in einem zentralen Repository aufbewahrt werden, das für das Cloud Governance-Team zur regelmäßigen Überprüfung und Überwachung zugänglich ist.
16. Governanceprozesse müssen Überwachungen zum Bereitstellungszeitpunkt und in regelmäßigen Zyklen umfassen, um für alle Ressourcen Konsistenz zu gewährleisten.
17. Die Bereitstellung von Anwendungen, für die eine Kundenauthentifizierung erforderlich ist, erfordert einen genehmigten Identitätsanbieter, der mit dem primären Identitätsanbieter für interne Benutzer kompatibel ist.
18. Cloud Governance-Prozesse müssen vierteljährliche Überprüfungen mit den Identitätsverwaltungsteams umfassen. Diese Überprüfungen können helfen, böswillige Akteure oder Nutzungsmuster zu identifizieren, die durch die Konfiguration von Cloudressourcen verhindert werden sollten.

## <a name="evolution-of-the-best-practices"></a>Weiterentwicklung bewährter Methoden

Der Governance-MVP-Entwurf wird so weiterentwickelt, dass er neue Azure-Richtlinien und eine Azure Cost Management-Implementierung umfasst. Zusammen erfüllen diese beiden Entwurfsänderungen die neuen Richtlinienanweisungen des Unternehmens.

1. Die Netzwerk- und IT-Sicherheitsteams definieren die Netzwerkanforderungen. Das Cloud Governance-Team unterstützt die Kommunikation.
2. Das Identitäts- und IT-Sicherheitsteam definiert die Identitätsanforderungen und nimmt alle erforderlichen Änderungen an der lokalen Active Directory-Implementierung vor. Das Cloud Governance-Team überprüft die Änderungen.
3. Erstellen Sie in Azure DevOps ein Repository zur Speicherung und Versionsverwaltung für alle relevanten Azure Resource Manager-Vorlagen und Skriptkonfigurationen.
4. Implementierung von Azure Security Center:
    1. Konfigurieren Sie Azure Security Center für jede Verwaltungsgruppe, die Klassifizierungen geschützter Daten enthält.
    2. Legen Sie automatische Bereitstellung standardmäßig auf „Aktiviert“ fest, um Patchingkompatibilität zu gewährleisten.
    3. Richten Sie Sicherheitskonfigurationen von Betriebssystemen ein. Das IT-Sicherheitsteam definiert die Konfiguration.
    4. Unterstützen Sie das IT-Sicherheitsteam bei der anfänglichen Verwendung von Security Center. Überführen Sie die Nutzung von Security Center in das IT-Sicherheitsteam, behalten Sie jedoch den Zugriff, um die Governance kontinuierlich zu verbessern.
    5. Erstellen Sie eine Resource Manager-Vorlage, die die erforderlichen Änderungen für die Security Center-Konfiguration in einem Abonnement widerspiegelt.
5. Aktualisieren Sie Azure-Richtlinien für alle Abonnements:
    1. Überprüfen und erzwingen Sie die Wichtigkeits- und Datenklassifizierung für alle Verwaltungsgruppen und Abonnements, um Abonnements mit Klassifizierungen geschützter Daten zu identifizieren.
    2. Überwachen und erzwingen Sie die ausschließliche Verwendung genehmigter Images.
6. Aktualisieren Sie Azure-Richtlinien für alle Abonnements, die Klassifizierungen geschützter Daten enthalten:
    1. Überwachen und erzwingen Sie die ausschließliche Verwendung von Azure RBAC-Standardrollen.
    2. Überwachen und erzwingen Sie die Verschlüsselung für alle Speicherkonten und Dateien im Ruhezustand auf einzelnen Knoten.
    3. Überwachen und erzwingen Sie die Anwendung einer NSG auf alle NICs und Subnetze. Die Netzwerk- und IT-Sicherheitsteams definieren die NSG.
    4. Überwachen und erzwingen Sie die Verwendung eines genehmigten Netzwerksubnetzes und VNETs pro Netzwerkschnittstelle.
    5. Überwachen und erzwingen Sie die Einschränkung benutzerdefinierter Routingtabellen.
    6. Wenden Sie die integrierten Richtlinien für die Gastkonfiguration wie folgt an:
        1. Überwachen Sie, dass Windows-Webserver sichere Kommunikationsprotokolle verwenden.
        2. Überwachen Sie die richtige Festlegung der Kennwortsicherheitseinstellungen auf Linux- und Windows-Computern.
7. Firewallkonfiguration:
    1. Identifizieren Sie eine Konfiguration von Azure Firewall, die die erforderlichen Sicherheitsanforderungen erfüllt. Identifizieren Sie alternativ eine kompatible Appliance eines Drittanbieters, die mit Azure kompatibel ist.
    2. Erstellen Sie eine Resource Manager-Vorlage, um die Firewall mit den erforderlichen Konfigurationen bereitzustellen.
8. Azure-Blaupause:
    1. Erstellen Sie eine neue Blaupause mit dem Namen `protected-data`.
    2. Fügen Sie der Blaupause die Firewall und die Azure Security Center-Vorlagen hinzu.
    3. Fügen Sie die neuen Richtlinien für Abonnements geschützter Daten hinzu.
    4. Veröffentlichen Sie die Blaupause für eine beliebige Verwaltungsgruppe, die aktuell plant, geschützte Daten zu hosten.
    5. Wenden Sie die neue Blaupause auf jedes betroffene Abonnement zusätzlich zu den vorhandenen Blaupausen an.

## <a name="conclusion"></a>Zusammenfassung

Das Hinzufügen der oben genannten Prozesse und Änderungen am Governance-MVP wird dazu beitragen, viele der mit der Sicherheitsgovernance verbundenen Risiken zu minimieren. Zusammen fügen diese Verfahren die Netzwerk-, Identitäts- und Sicherheitsüberwachungstools hinzu, die zum Schutz der Daten erforderlich sind.

## <a name="next-steps"></a>Nächste Schritte

Mit der Weiterentwicklung der Cloudeinführung und der damit verbundenen Steigerung des Geschäftswerts entwickeln sich auch die Risiken und Anforderungen an Cloud Governance. Für das fiktive Unternehmen in diesem Beispiel besteht der nächste Schritt darin, unternehmenskritische Workloads zu unterstützen. An diesem Punkt werden Steuerelemente zur Ressourcenkonsistenz benötigt.

> [!div class="nextstepaction"]
> [Entwicklung der Ressourcenkonsistenz](./resource-consistency-evolution.md)
