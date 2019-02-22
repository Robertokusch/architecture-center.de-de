---
title: 'Große Unternehmen: Entwicklung der Ressourcenkonsistenz'
titleSuffix: Microsoft Cloud Adoption Framework for Azure
ms.service: architecture-center
ms.subservice: enterprise-cloud-adoption
ms.custom: governance
ms.date: 02/11/2019
description: 'Große Unternehmen: Entwicklung der Ressourcenkonsistenz'
author: BrianBlanchard
ms.openlocfilehash: 9fc6ca015310c02338463d3c8aa53ddfc74699df
ms.sourcegitcommit: 273e690c0cfabbc3822089c7d8bc743ef41d2b6e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/08/2019
ms.locfileid: "55901633"
---
# <a name="large-enterprise-resource-consistency-evolution"></a>Große Unternehmen: Entwicklung der Ressourcenkonsistenz

Dieser Artikel entwickelt die Lösung weiter, indem Steuerelemente für Ressourcenkonsistenz zum Governance-MVP hinzugefügt werden, um unternehmenskritische Anwendungen zu unterstützen.

## <a name="evolution-of-the-narrative"></a>Weiterentwicklung der Lösung

Die Teams zur Cloudeinführung haben alle Anforderungen für die Verschiebung geschützter Daten erfüllt. Mit diesen Anwendungen kommen SLA-Verpflichtungen und der Unterstützungsbedarf durch IT Operations auf das Unternehmen zu. Direkt hinter dem Team, das die zwei Rechenzentren migriert, stehen mehrere App-Entwicklungs- und Business Intelligence-Teams bereit, um mit dem Start neuer Lösungen in der Produktion zu beginnen. Für IT Operations ist der Gedanke des Cloud-Betriebs neu, und es wird eine Möglichkeit zur schnellen Integration bestehender Betriebsprozesse benötigt.

### <a name="evolution-of-current-state"></a>Weiterentwicklung des aktuellen Status

- IT verschiebt aktiv Produktionsworkloads mit geschützten Daten nach Azure. Eine Reihe von Workloads mit niedriger Priorität bedient den Produktionsdatenverkehr. Weitere können umgestellt werden, sobald IT Operations offiziell seine Bereitschaft erklärt, Support für die Workloads zu leisten.
- Die Anwendungsentwicklungsteams sind für Produktionsdatenverkehr bereit.
- Das Business Intelligence-Team steht bereit, Prognosen und Erkenntnisse in die Systeme zu integrieren, die den Betrieb für die drei Geschäftseinheiten tragen.

### <a name="evolution-of-the-future-state"></a>Weiterentwicklung des zukünftigen Status

- Für IT Operations ist der Gedanke des Cloud-Betriebs neu, und es wird eine Möglichkeit zur schnellen Integration bestehender Betriebsprozesse benötigt.

Die Änderungen des aktuellen und zukünftigen Status bergen neue Risiken, die neue Richtlinienanweisungen erfordern.

## <a name="evolution-of-tangible-risks"></a>Entwicklung materieller Risiken

**Unterbrechung des Geschäftsbetriebs**: Jede neue Plattform bringt ein inhärentes Risiko für Unterbrechungen in unternehmenskritischen Geschäftsabläufen mit sich. Das IT Operations-Team und die Teams, die in verschiedenen Cloud-Einsatztypen agieren, sind relativ unerfahren in Cloud Operations. Dadurch steigt die Gefahr von Unterbrechungen, die entsprechend gemildert und kontrolliert werden muss.

Dieses Geschäftsrisiko lässt sich auf eine Reihe von technischen Risiken ausweiten:

- Falsch ausgerichtete Betriebsprozesse können zu Ausfällen führen, die nicht erkannt oder schnell wiederhergestellt werden können.
- Ein Eindringen von außerhalb oder Denial-of-Service-Angriffe können zu einer Unterbrechung des Geschäftsbetriebs führen.
- Unternehmenswichtige Ressourcen werden möglicherweise nicht korrekt identifiziert und daher nicht ordnungsgemäß betrieben.
- Nicht identifizierte oder falsch bezeichnete Ressourcen werden möglicherweise durch die vorhandenen Prozesse des Betriebsmanagements nicht unterstützt.
- Die Konfiguration der bereitgestellten Ressourcen erfüllt möglicherweise nicht die Leistungserwartungen.
- Die Protokollierung wird eventuell nicht ordnungsgemäß aufgezeichnet und ist nicht korrekt zentralisiert, um die Behebung der Leistungsprobleme zu ermöglichen.
- Bei Wiederherstellungsrichtlinien treten möglicherweise Fehler auf, oder ihre Ausführung braucht länger als erwartet.
- Inkonsistente Bereitstellungsprozesse können Sicherheitslücken herbeiführen, die zu Datenverlusten oder Unterbrechungen führen können.
- Konfigurationsabweichungen oder fehlende Patches können unbeabsichtigte Sicherheitslücken zur Folge haben, die zu Datenverlusten oder Unterbrechungen führen können.
- Die Konfiguration setzt möglicherweise die Anforderungen der definierten SLAs oder der festgeschriebenen Wiederherstellungsanforderungen nicht durch.
- Die bereitgestellten Betriebssysteme oder Anwendungen genügen eventuell nicht den Härtungsanforderungen an Betriebssysteme und Anwendungen.
- Es besteht die Gefahr von Inkonsistenzen aufgrund der verschiedenen Teams, die in der Cloud arbeiten.

## <a name="evolution-of-the-policy-statements"></a>Weiterentwicklung der Richtlinienanweisungen

Die folgenden Änderungen an der Richtlinie verringern die neuen Risiken und vereinfachen die Implementierung. Die Liste wirkt lang, aber die Einführung dieser Richtlinien ist möglicherweise einfacher, als es den Anschein hat.

1. Alle bereitgestellten Ressourcen müssen nach Wichtigkeit und Datenklassifizierung kategorisiert werden. Vor der Bereitstellung in der Cloud müssen die Klassifizierungen durch das Cloud Governance-Team und die Besitzer der Anwendung überprüft werden.
2. Subnetze, die unternehmenskritische Anwendungen enthalten, müssen durch eine Firewalllösung geschützt werden, die Eindringversuche erkennen und auf Angriffe reagieren kann.
3. Die Anforderungen an die Netzwerkkonfiguration, die vom Sicherheitsbaselineteam definiert wurden, müssen mit Governancetools überwacht und durchgesetzt werden.
4. Mit den Governancetools muss überprüft werden, ob alle Ressourcen, die in einem Zusammenhang mit unternehmenskritischen Anwendungen oder geschützten Daten stehen, in die Überwachung auf Ressourcenschwund und -optimierung einbezogen sind.
5. Ferner muss mit den Governancetools überprüft werden, ob für alle unternehmenskritischen Anwendungen oder geschützten Daten Protokolldaten mit dem passenden Protokolliergrad erfasst werden.
6. Der Governanceprozess muss überprüfen, ob für unternehmenskritische Anwendungen und geschützte Daten die Sicherung, Wiederherstellung und Einhaltung von SLAs ordnungsgemäß implementiert sind.
7. Mit den Governancetools muss die Bereitstellung virtueller Computer ausschließlich auf genehmigte Images eingeschränkt werden.
8. Die Governancetools müssen erzwingen, dass für alle bereitgestellten Ressourcen, die unternehmenskritische Anwendungen unterstützen, automatische Updates **verhindert** werden. Verstöße müssen von Betriebsmanagementteams überprüft und in Übereinstimmung mit den Betriebsrichtlinien beseitigt werden. Ressourcen, die nicht automatisch aktualisiert werden, müssen in Prozesse einbezogen werden, die IT Operations unterstehen.
9. Mit Governancetools muss die Kennzeichnung (Tagging) im Hinblick auf Kosten, Kritikalität, SLA, Anwendung und Datenklassifizierung überprüft werden. Alle Werte müssen sich an vordefinierten Werten ausrichten, die vom Cloud Governance-Team verwaltet werden.
10. Governanceprozesse müssen Überwachungen zum Bereitstellungszeitpunkt und nachfolgend in regelmäßigen Zyklen umfassen, um für alle Ressourcen Konsistenz zu gewährleisten.
11. Trends und Exploits, die mögliche Auswirkungen auf Cloudbereitstellungen haben, müssen vom Sicherheitsteam regelmäßig überprüft werden, damit Updates für in der Cloud verwendete Sicherheitsbaselinetools bereitgestellt werden.
12. Vor der Veröffentlichung in einer Produktionsumgebung müssen alle unternehmenskritischen Anwendungen und geschützten Daten der designierten Betriebsüberwachungslösung hinzugefügt werden. Ressourcen, die von den gewählten IT Operations-Tools nicht erkannt werden können, können nicht für die Produktion freigegeben werden. Alle Änderungen, die erforderlich sind, um die Ressourcen erkennbar zu machen, müssen an den relevanten Bereitstellungsprozessen vorgenommen werden, um sicherzustellen, dass die Ressourcen in kommenden Bereitstellungen ermitelbar sind.
13. Bei der Ermittlung muss die Ressourcendimensionierung von den Betriebsmanagementteams überprüft werden, um sicherzustellen, dass die Ressourcen den Leistungsanforderungen genügen.
14. Bereitstellungstools müssen vom Cloud Governance-Team genehmigt werden, um eine kontinuierliche Governance für bereitgestellte Ressourcen sicherzustellen.
15. Bereitstellungsskripts müssen in einem zentralen Repository aufbewahrt werden, das für das Cloud Governance-Team zur regelmäßigen Überprüfung und Überwachung zugänglich ist.
16. Mithilfe von Governance-Prüfprozessen muss bestätigt werden, dass die bereitgestellten Ressourcen im Hinblick auf SLA- und Wiederherstellungsanforderungen ordnungsgemäß konfiguriert sind.

## <a name="evolution-of-the-best-practices"></a>Weiterentwicklung bewährter Methoden

In diesem Abschnitt des Artikels wird der Governance-MVP-Entwurf so weiterentwickelt, dass er neue Azure-Richtlinien und eine Implementierung von Azure Cost Management umfasst. Zusammen erfüllen diese beiden Entwurfsänderungen die neuen Richtlinienanweisungen des Unternehmens.

Nach den Erfahrungen mit diesem fiktiven Beispiel wird davon ausgegangen, dass die Weiterentwicklung der geschützten Daten bereits stattgefunden hat. Aufbauend auf diesen bewährten Verfahren, werden in der Folge Anforderungen an die Betriebsüberwachung hinzugefügt, mit denen ein Abonnement für geschäftskritische Anwendungen vorbereitet wird.

**Unternehmenseigenes IT-Abonnement**: Fügen Sie dem unternehmenseigenen IT-Abonnement, das als Hub fungiert, Folgendes hinzu.

1. Als externe Abhängigkeit muss das Cloud-Betriebsteam Tools für die Betriebsüberwachung, BCDR-Tools (Business Continuity/Disaster Recovery) und Tools zur automatischen Wartung definieren. Damit kann das Cloud Governance-Team die erforderlichen Ermittlungsvorgänge unterstützen.
    1. In diesem Anwendungsfall hat das Cloud Operations-Team Azure Monitor als primäres Tool für die Überwachung unternehmenskritischer Anwendungen gewählt.
    2. Das Team sich ferner für Azure Site Recovery als primäres BCDR-Tool entschieden.
2. Azure Site Recovery-Implementierung
    1. Definieren Sie den Azure-Tresor für Sicherungs- und Wiederherstellungsvorgänge, und stellen Sie ihn bereit
    2. Erstellen Sie eine Vorlage der Azure-Ressourcenverwaltung zur Erstellung eines Tresors in jedem Abonnement
3. Azure Monitor-Implementierung
    1. Nachdem ein unternehmenskritisches Abonnement ermittelt wurde, kann mit PowerShell ein Log Analytics-Arbeitsbereich erstellt werden. Dies ist ein Prozess vor der Bereitstellung.

**Individuelles Abonnement zur Cloudeinführung**: Mit den folgenden Maßnahmen stellen Sie sicher, dass jedes Abonnement von der Überwachungslösung ermittelbar und zur Aufnahme in BCDR-Praktiken bereit ist.

1. Azure-Richtlinie für unternehmenskritische Knoten
    1. Überwachen und erzwingen Sie die ausschließliche Verwendung von Standardrollen.
    2. Überwachen und erzwingen Sie die Anwendung von Verschlüsselung für alle Speicherkonten.
    3. Überwachen und erzwingen Sie die Verwendung eines genehmigten Netzwerksubnetzes und VNets pro Netzwerkschnittstelle.
    4. Überwachen und erzwingen Sie die Einschränkung benutzerdefinierter Routingtabellen.
    5. Überwachen und erzwingen Sie die Bereitstellung von Log Analytics-Agents für virtuelle Windows- und Linux-Computer.
2. Azure Blueprint
    1. Erstellen Sie eine Blaupause mit dem Namen `mission-critical-workloads-and-protected-data`. Diese Blaupause betrifft Ressourcen, in Ergänzung zur Blaupause der geschützten Daten.
    2. Fügen Sie der Blaupause die neuen Azure-Richtlinien hinzu.
    3. Wenden Sie die Blaupause auf jedes Abonnement an, von dem angenommen wird, dass es eine unternehmenskritische Anwendung hostet.

## <a name="conclusion"></a>Zusammenfassung

Durch das Hinzufügen dieser Prozesse und Änderungen zum Governance-MVP lassen sich viele Risiken im Zusammenhang mit der Ressourcengovernance verringern. In Kombination fügen Sie die Steuerelemente für Wiederherstellung, Dimensionierung und Überwachung hinzu, die zum Ermöglichen eines cloudfähigen Betriebs erforderlich sind.

## <a name="next-steps"></a>Nächste Schritte

Mit der Weiterentwicklung der Cloudeinführung und der damit verbundenen Steigerung des Geschäftswerts entwickeln sich auch die Risiken und Anforderungen an Cloud Governance. Für das fiktive Unternehmen, das wir auf diesem Weg begleiten, kommt der nächste Trigger, wenn die Größe der Bereitstellung 1.000 Ressourcen in der Cloud oder die monatlichen Ausgaben 10.000 $ übersteigen. An diesem Punkt fügt das Cloud Governance-Team Steuerelemente zum Kostenmanagement hinzu.

> [!div class="nextstepaction"]
> [Weiterentwicklung des Kostenmanagements](./cost-management-evolution.md)