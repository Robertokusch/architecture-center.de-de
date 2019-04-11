---
title: 'Framework für die Cloudeinführung (Cloud Adoption Framework, CAF): Kleine bis mittlere Unternehmen: Entwicklung der Ressourcenkonsistenz'
titleSuffix: Microsoft Cloud Adoption Framework for Azure
ms.service: architecture-center
ms.subservice: enterprise-cloud-adoption
ms.custom: governance
ms.date: 02/11/2019
description: 'Kleine bis mittlere Unternehmen: Entwicklung der Ressourcenkonsistenz'
author: BrianBlanchard
ms.openlocfilehash: caacd62da9da526ec01ab935896598065f64d9d1
ms.sourcegitcommit: c053e6edb429299a0ad9b327888d596c48859d4a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/20/2019
ms.locfileid: "58246501"
---
# <a name="small-to-medium-enterprise-resource-consistency-evolution"></a>Kleines bis mittleres Unternehmen: Entwicklung der Ressourcenkonsistenz

Dieser Artikel erzählt die Geschichte fort, indem Steuerelemente für Ressourcenkonsistenz hinzugefügt werden, um unternehmenskritische Apps zu unterstützen.

## <a name="evolution-of-the-narrative"></a>Weiterentwicklung der Lösung

Neue Kundenerfahrungen, neue Vorhersagetools und migrierte Infrastruktur entwickeln sich weiter. Das Unternehmen kann jetzt damit beginnen, diese Ressourcen mit einer Produktionskapazität zu verwenden.

### <a name="evolution-of-the-current-state"></a>Weiterentwicklung des aktuellen Status

In der vorherigen Phase dieser Lösung waren die Anwendungsentwicklungs- und BI-Teams nahezu bereit, Kunden- und Finanzdaten in die Produktionsworkloads zu integrieren. Das IT-Team war dabei, das DR-Datencenter außer Betrieb zu nehmen.

Seit diesem Zeitpunkt haben sich einige Dinge geändert, die sich auf die Governance auswirken:

- Die IT-Abteilung hat 100 % des DR-Datencenters vorzeitig außer Betrieb genommen. Dabei wurde eine Reihe von Ressourcen im Produktionsdatencenter als Cloudmigrationskandidaten identifiziert.
- Die Anwendungsentwicklungsteams sind nun für Produktionsdatenverkehr bereit.
- Das BI-Team ist bereit, Vorhersagen und Erkenntnisse in die Betriebssysteme im Produktionsdatencenter zurückzuliefern.

### <a name="evolution-of-the-future-state"></a>Weiterentwicklung des zukünftigen Status

Bevor Azure-Bereitstellungen in Produktionsgeschäftsprozessen eingesetzt werden können, muss der Cloudbetrieb ausgereift sein. In Verbindung damit ist eine zusätzliche Weiterentwicklung von Governance erforderlich, um sicherzustellen, dass Ressourcen ordnungsgemäß betrieben werden können.

Die Änderungen des aktuellen und zukünftigen Status bergen neue Risiken, die neue Richtlinienanweisungen erfordern.

## <a name="evolution-of-tangible-risks"></a>Entwicklung materieller Risiken

**Unterbrechung des Geschäftsbetriebs**: Jede neue Plattform bringt ein inhärentes Risiko für Unterbrechungen in unternehmenskritischen Geschäftsabläufen mit sich. Das IT Operations-Team und die Teams, die in verschiedenen Cloud-Einsatztypen agieren, sind relativ unerfahren in Cloud Operations. Dadurch steigt die Gefahr von Unterbrechungen, die entsprechend gemildert und kontrolliert werden muss.

Dieses Unternehmensrisiko lässt sich auf einige technische Risiken erweitern:

- Ein Eindringen von außerhalb oder Denial-of-Service-Angriffe können zu einer Unterbrechung des Geschäftsbetriebs führen.
- Unternehmenswichtige Ressourcen werden möglicherweise nicht richtig identifiziert und daher nicht ordnungsgemäß betrieben.
- Nicht identifizierte oder falsch bezeichnete Ressourcen werden möglicherweise durch die vorhandenen Prozesse des Betriebsmanagements nicht unterstützt.
- Die Konfiguration der bereitgestellten Ressourcen erfüllt möglicherweise nicht die Leistungserwartungen.
- Die Protokollierung wird möglicherweise nicht ordnungsgemäß aufgezeichnet und ist nicht richtig zentralisiert, um die Behebung der Leistungsprobleme zu ermöglichen.
- Bei Wiederherstellungsrichtlinien treten möglicherweise Fehler auf, oder ihre Ausführung dauert länger als erwartet.
- Inkonsistente Bereitstellungsprozesse können Sicherheitslücken verursachen, die zu Datenverlusten oder Unterbrechungen führen können.
- Konfigurationsabweichungen oder fehlende Patches können unbeabsichtigte Sicherheitslücken zur Folge haben, die zu Datenverlusten oder Unterbrechungen führen können.
- Die Konfiguration kann möglicherweise die Anforderungen der definierten SLAs oder der festgeschriebenen Wiederherstellungsanforderungen nicht durchsetzen.
- Die bereitgestellten Betriebssysteme oder Anwendungen genügen möglicherweise nicht den Härtungsanforderungen.
- Aufgrund der zahlreichen Teams, die in der Cloud arbeiten, besteht die Gefahr von Inkonsistenzen.

## <a name="evolution-of-the-policy-statements"></a>Weiterentwicklung der Richtlinienanweisungen

Die folgenden Änderungen an der Richtlinie verringern die neuen Risiken und vereinfachen die Implementierung. Die Liste wirkt lang, aber die Einführung dieser Richtlinien ist möglicherweise einfacher, als es den Anschein hat.

1. Alle bereitgestellten Ressourcen müssen nach Wichtigkeit und Datenklassifizierung kategorisiert werden. Vor der Bereitstellung in der Cloud müssen die Klassifizierungen durch das Cloud Governance-Team und die Besitzer der Anwendung überprüft werden.
2. Subnetze, die unternehmenskritische Anwendungen enthalten, müssen durch eine Firewalllösung geschützt werden, die Eindringversuche erkennen und auf Angriffe reagieren kann.
3. Die Anforderungen an die Netzwerkkonfiguration, die vom Sicherheitsverwaltungsteam definiert wurden, müssen mit Governancetools überwacht und durchgesetzt werden.
4. Mit den Governancetools muss überprüft werden, ob alle Ressourcen, die in einem Zusammenhang mit unternehmenskritischen Apps oder geschützten Daten stehen, in die Überwachung auf Ressourcenschwund und -optimierung einbezogen sind.
5. Ferner muss mit den Governancetools überprüft werden, ob für alle unternehmenskritischen Anwendungen oder geschützten Daten Protokolldaten mit dem passenden Protokolliergrad erfasst werden.
6. Der Governanceprozess muss überprüfen, ob für unternehmenskritische Anwendungen und geschützte Daten die Sicherung, Wiederherstellung und Einhaltung von SLAs ordnungsgemäß implementiert sind.
7. Mit den Governancetools müssen die Bereitstellungen virtueller Computer ausschließlich auf genehmigte Images eingeschränkt werden.
8. Die Governancetools müssen erzwingen, dass für alle bereitgestellten Ressourcen, die unternehmenskritische Anwendungen unterstützen, automatische Updates verhindert werden. Verstöße müssen von Betriebsmanagementteams überprüft und in Übereinstimmung mit den Betriebsrichtlinien beseitigt werden. Ressourcen, die nicht automatisch aktualisiert werden, müssen in Prozesse des IT-Betriebs einbezogen werden.
9. Mit Governancetools muss die Kennzeichnung (Tagging) im Hinblick auf Kosten, Kritikalität, SLA, Anwendung und Datenklassifizierung überprüft werden. Alle Werte müssen sich an vordefinierten Werten ausrichten, die vom Governanceteam verwaltet werden.
10. Governanceprozesse müssen Überwachungen zum Bereitstellungszeitpunkt und in regelmäßigen Zyklen umfassen, um für alle Ressourcen Konsistenz zu gewährleisten.
11. Trends und Exploits, die mögliche Auswirkungen auf Cloudbereitstellungen haben, müssen vom Sicherheitsteam regelmäßig überprüft werden, damit Updates für in der Cloud verwendete Sicherheitsverwaltungstools bereitgestellt werden.
12. Vor der Veröffentlichung in einer Produktionsumgebung müssen alle unternehmenskritischen Apps und geschützten Daten der designierten Betriebsüberwachungslösung hinzugefügt werden. Ressourcen, die von den gewählten IT Operations-Tools nicht erkannt werden können, können nicht für die Produktion freigegeben werden. Alle Änderungen, die erforderlich sind, um die Ressourcen ermittelbar zu machen, müssen an den relevanten Bereitstellungsprozessen vorgenommen werden, um sicherzustellen, dass die Ressourcen in kommenden Bereitstellungen erkennbar sind.
13. Bei der Ermittlung dimensionieren die operativen Managementteams die Ressourcen, um sicherzustellen, dass die Ressourcen den Leistungsanforderungen entsprechen.
14. Bereitstellungstools müssen vom Cloud Governance-Team genehmigt werden, um eine kontinuierliche Governance für bereitgestellte Ressourcen sicherzustellen.
15. Bereitstellungsskripts müssen in einem zentralen Repository aufbewahrt werden, das für das Cloud Governance-Team zur regelmäßigen Überprüfung und Überwachung zugänglich ist.
16. Mithilfe von Governanceprüfprozessen muss bestätigt werden, dass die bereitgestellten Ressourcen im Hinblick auf SLA- und Wiederherstellungsanforderungen ordnungsgemäß konfiguriert sind.

## <a name="evolution-of-the-best-practices"></a>Weiterentwicklung bewährter Methoden

In diesem Abschnitt des Artikels wird der Governance-MVP-Entwurf so weiterentwickelt, dass er neue Azure-Richtlinien und eine Implementierung von Azure Cost Management umfasst. Zusammen erfüllen diese beiden Entwurfsänderungen die neuen Richtlinienanweisungen des Unternehmens.

1. Das Cloud Operations-Team definiert operative Überwachungstools und automatisierte Korrekturtools. Das Cloud Governance-Team unterstützt diese Ermittlungsprozesse. In diesem Anwendungsfall hat das Cloud Operations-Team Azure Monitor als primäres Tool für die Überwachung unternehmenskritischer Anwendungen ausgewählt.
2. Erstellen Sie in Azure DevOps ein Repository zur Speicherung und Versionsverwaltung für alle relevanten Resource Manager-Vorlagen und Skriptkonfigurationen.
3. Azure -Tresorimplementierung:
    1. Definieren Sie den Azure-Tresor für Sicherungs- und Wiederherstellungsvorgänge, und stellen Sie ihn bereit.
    2. Erstellen Sie eine Resource Manager-Vorlage zum Erstellen eines Tresors in jedem Abonnement.
4. Aktualisieren Sie Azure Policy für alle Abonnements:
    1. Überprüfen und erzwingen Sie die Wichtigkeits- und Datenklassifizierung für alle Abonnements, um Abonnements mit unternehmenskritischen Ressourcen zu identifizieren.
    2. Überwachen und erzwingen Sie die ausschließliche Verwendung genehmigter Images.
5. Azure Monitor-Implementierung:
    1. Nachdem ein unternehmenskritisches Abonnement identifiziert wurde, erstellen Sie mit PowerShell einen Azure Monitor-Arbeitsbereich. Dies ist ein Prozess vor der Bereitstellung.
    2. Während der Bereitstellungstests stellt das Cloud Operations-Team die erforderlichen Agents bereit und testet die Ermittlung.
6. Aktualisieren Sie Azure Policy für alle Abonnements, die unternehmenskritische Anwendungen enthalten.
    1. Überwachen und erzwingen Sie die Anwendung einer NSG auf alle NICs und Subnetze. Netzwerke und IT-Sicherheit definieren die NSG.
    2. Überwachen und erzwingen Sie die Verwendung genehmigter Netzwerksubnetze und VNETs für jede Netzwerkschnittstelle.
    3. Überwachen und erzwingen Sie die Einschränkung benutzerdefinierter Routingtabellen.
    4. Überwachen und erzwingen Sie die Bereitstellung von Azure Monitor-Agents für alle virtuellen Computer.
    5. Überwachen und erzwingen Sie, dass der Azure-Tresor im Abonnement vorhanden ist.
7. Firewallkonfiguration:
    1. Identifizieren Sie eine Konfiguration von Azure Firewall, die die Sicherheitsanforderungen erfüllt. Identifizieren Sie alternativ eine Appliance eines Drittanbieters, die mit Azure kompatibel ist.
    2. Erstellen Sie eine Resource Manager-Vorlage, um die Firewall mit den erforderlichen Konfigurationen bereitzustellen.
8. Azure-Blaupause:
    1. Erstellen Sie eine neue Azure-Blaupause namens `protected-data`.
    2. Fügen Sie der Blaupause die Firewall und die Azure-Tresorvorlagen hinzu.
    3. Fügen Sie die neuen Richtlinien für Abonnements geschützter Daten hinzu.
    4. Veröffentlichen Sie die Blaupause für alle Verwaltungsgruppen, die zum Hosten unternehmenskritischer Anwendungen bestimmt sind.
    5. Wenden Sie die neue Blaupause zusammen mit vorhandenen Blaupausen auf die betroffenen Abonnements an.

## <a name="conclusion"></a>Zusammenfassung

Durch diese zusätzlichen Prozesse und Änderungen am Governance-MVP lassen sich viele Risiken im Zusammenhang mit der Ressourcengovernance verringern. In Kombination fügen sie die Steuerelemente für Wiederherstellung, Dimensionierung und Überwachung hinzu, die einen cloudfähigen Betrieb ermöglichen.

## <a name="next-steps"></a>Nächste Schritte

Mit der Weiterentwicklung der Cloudeinführung und der damit verbundenen Steigerung des Geschäftswerts entwickeln sich auch die Risiken und Anforderungen an Cloud Governance. Für das fiktive Unternehmen, das wir auf diesem Weg begleiten, wird der nächste Trigger ausgelöst, wenn die Größe der Bereitstellung 100 Ressourcen in der Cloud oder monatliche Ausgaben von 1.000 USD übersteigt. An diesem Punkt fügt das Cloud Governance-Team Steuerelemente zum Kostenmanagement hinzu.

> [!div class="nextstepaction"]
> [Weiterentwicklung des Kostenmanagements](./cost-management-evolution.md)