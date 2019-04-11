---
title: 'Framework für die Cloudeinführung (Cloud Adoption Framework, CAF): Große Unternehmen: Entwicklung der Sicherheitsbaseline '
titleSuffix: Microsoft Cloud Adoption Framework for Azure
ms.service: architecture-center
ms.subservice: enterprise-cloud-adoption
ms.custom: governance
ms.date: 02/11/2019
description: 'Große Unternehmen: Entwicklung der Sicherheitsbaseline'
author: BrianBlanchard
ms.openlocfilehash: e9e8b7dc1eaeb3a8555326a51b2548ad668e0171
ms.sourcegitcommit: c053e6edb429299a0ad9b327888d596c48859d4a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/20/2019
ms.locfileid: "58241611"
---
# <a name="large-enterprise-security-baseline-evolution"></a>Große Unternehmen: Entwicklung der Sicherheitsbaseline

In diesem Artikel wird die Geschichte weiterentwickelt. Es werden Sicherheitskontrollen hinzugefügt, die das Verschieben geschützter Daten in die Cloud unterstützen.

## <a name="evolution-of-the-narrative"></a>Weiterentwicklung der Geschichte

Der CIO hat mehrere Monate lang mit Kollegen und der Rechtsabteilung des Unternehmens zusammengearbeitet. Ein Unternehmensberater mit Fachkenntnissen im Bereich der Cybersicherheit wurde engagiert, um die vorhandenen Teams für IT-Sicherheit und IT-Governance beim Entwurf einer neuen Richtlinie für geschützte Daten zu unterstützen. Die Gruppe konnte die Unterstützung des Vorstands dafür gewinnen, die vorhandene Richtlinie zu ersetzen, sodass personenbezogene Informationen und Finanzdaten von genehmigten Cloudanbietern gehostet werden dürfen. Dafür mussten eine Reihe von Sicherheitsanforderungen und ein Governanceprozess installiert werden, um die Einhaltung dieser Richtlinien zu verifizieren und zu dokumentieren.

In den letzten zwölf Monaten haben die Cloudeinführungsteams die meisten der 5.000 Ressourcen aus den beiden Rechenzentren außer Betrieb gesetzt. Die 350 inkompatiblen Ressourcen wurden in ein anderes Rechenzentrum verschoben. Es verbleiben nur die 1.250 virtuellen Computer, die geschützte Daten enthalten.

### <a name="evolution-of-the-cloud-governance-team"></a>Entwicklung des Cloudgovernanceteams

Das Cloudgovernanceteam entwickelt sich im Laufe der Geschichte weiter. Die beiden Gründungsmitglieder des Teams gehören jetzt zu den angesehensten Cloudarchitekten im Unternehmen. Die Sammlung von Konfigurationsskripts wächst, während neue Teams innovative neue Bereitstellungen in Angriff nehmen. Auch das Cloudgovernanceteam ist gewachsen. Bis vor kurzem haben Mitglieder des IT-Betriebsteams sich den Aktivitäten des Cloudgovernanceteams angeschlossen, um sich auf Cloudvorgänge vorzubereiten. Die Cloudarchitekten, die diese Community gefördert haben, werden sowohl als Cloudwächter als auch als Cloudbeschleuniger betrachtet.

Der Unterschied ist zwar subtil, aber beim Erstellen einer governancebezogenen IT-Kultur von entscheidender Bedeutung. Ein Cloudverwaltungsberechtigter räumt das Chaos auf, das von innovativen Cloudarchitekten hinterlassen wird, und zwischen den beiden Rollen gibt es natürliche Reibung und gegensätzliche Ziele. Ein Cloudwächter hält die Cloud sicher, damit andere Cloudarchitekten mit weniger Chaos schneller vorankommen. Ein Cloudbeschleuniger erfüllt beide Funktionen, ist jedoch auch an der Erstellung von Vorlagen zur Beschleunigung von Bereitstellung und Einführung beteiligt. Damit ist er sowohl Innovationsbeschleuniger als auch Verteidiger der fünf Disziplinen der Cloud-Governance.

### <a name="evolution-of-the-current-state"></a>Weiterentwicklung des aktuellen Status

In der vorherigen Phase dieser Geschichte hatte das Unternehmen damit begonnen, zwei Rechenzentren außer Betrieb zu nehmen. Zu diesem laufenden Prozess gehört die Migration einiger Anwendungen mit älteren Authentifizierungsanforderungen, zu der eine Weiterentwicklung der Identitätsbasis erforderlich war, wie im [vorherigen Artikel](identity-baseline-evolution.md) beschrieben.

Seit diesem Zeitpunkt haben sich einige Dinge geändert, die sich auf die Governance auswirken:

- Tausende von IT- und Unternehmensressourcen werden in der Cloud bereitgestellt.
- Das Team für die Anwendungsentwicklung hat eine CI/CD-Pipeline (Continuous Integration/Continuous Deployment) implementiert, um eine native Cloudanwendung mit einer verbesserten Benutzeroberfläche bereitzustellen. Diese Anwendung interagiert noch nicht mit geschützten Daten, daher ist sie nicht für die Produktion bereit.
- Das Business Intelligence-Team innerhalb der IT-Abteilung stellt aktiv Daten aus Logistik, Inventur und Drittanbieterdaten in der Cloud zusammen. Diese Daten werden für neue Vorhersagen verwendet, die zum Gestalten von Geschäftsprozessen dienen können. Allerdings sind diese Vorhersagen und Erkenntnisse erst umsetzbar, wenn Kunden- und Finanzdaten in die Datenplattform integriert werden können.
- Das IT-Team setzt zurzeit die Pläne des CIO und des CFO um, zwei Rechenzentren außer Betrieb zu nehmen. Fast 3.500 Ressourcen in den beiden Rechenzentren wurden außer Betrieb genommen oder migriert.
- Die Richtlinien im Hinblick auf personenbezogene Informationen und Finanzdaten wurden modernisiert. Allerdings sind die neuen Unternehmensrichtlinien abhängig von der Implementierung der entsprechenden Sicherheits- und Governancerichtlinien. Teams können noch nicht weiterarbeiten.

### <a name="evolution-of-the-future-state"></a>Weiterentwicklung des zukünftigen Status

- Frühe Experimente der Teams für Anwendungsentwicklung und Business Intelligence haben potenzielle Verbesserungen für die Kundenumgebung und datengestützte Entscheidungen aufgezeigt. Beide Teams möchten die Einführung der Cloud in den nächsten 18 Monaten erweitern, indem Sie diese Lösungen in der Produktionsumgebung bereitstellen.
- Die IT-Abteilung hat eine geschäftliche Rechtfertigung für die Migration von fünf weiteren Rechenzentren zu Azure entwickelt. Dadurch werden die IT-Kosten weiter gesenkt, und eine höhere geschäftliche Flexibilität wird ermöglicht. Auch wenn diese Rechenzentren kleiner sind, sollen sich die Kosteneinsparungen durch die Außerbetriebnahme dieser Rechenzentren insgesamt verdoppeln.
- Von den Budgets für Kapital- und Betriebsaufwand wurde die Implementierung der erforderlichen Richtlinien, Tools und Prozesse für Sicherheit und Governance genehmigt. Die erwarteten Kosteneinsparungen durch die Außerbetriebnahme von Rechenzentren reichen zur Finanzierung dieser neuen Initiative deutlich aus. Die IT-Abteilung und die Unternehmensführung sind zuversichtlich, dass diese Investition die Rendite in anderen Bereichen beschleunigen wird. Das Cloudgovernance-Basisteam ist mittlerweile ein anerkanntes Team mit engagierten Führungskräften und Mitarbeitern.
- Die Teams für Cloudeinführung, Cloudgovernance, IT-Sicherheit und IT-Governance implementieren zusammen Sicherheits- und Governanceanforderungen, um den Cloudeinführungsteams die Migration geschützter Daten in die Cloud zu ermöglichen.

## <a name="evolution-of-tangible-risks"></a>Weiterentwicklung materieller Risiken

**Datenpanne**: Bei Einführung einer neuen Datenplattform gibt es eine inhärente Zunahme der Haftung im Zusammenhang mit Datenpannen. Das technische Personal zur Einführung von Cloudtechnologien ist zunehmend dafür verantwortlich, dass Lösungen implementiert werden, die dieses Risiko senken können. Eine robuste Sicherheits- und Governancestrategie muss implementiert werden, um sicherzustellen, dass dieses technische Personal dieser Verantwortung nachkommt.

Dieses Unternehmensrisiko lässt sich auf einige technische Risiken erweitern:

- Unternehmenskritische Apps oder geschützten Daten werden möglicherweise unbeabsichtigt bereitgestellt.
- Geschützte Daten könnten aufgrund falscher Verschlüsselungsentscheidungen im Speicher verfügbar gemacht werden.
- Nicht autorisierte Benutzer können möglicherweise auf geschützte Daten zugreifen.
- Ein Eindringen von außerhalb kann den Zugriff auf geschützte Daten ermöglichen.
- Ein Eindringen von außerhalb oder Denial-of-Service-Angriffe können zu einer Unterbrechung des Geschäftsbetriebs führen.
- Organisatorische Änderungen oder Personalwechsel können den unbefugten Zugriff auf geschützte Daten ermöglichen.
- Neue Exploits schaffen Möglichkeiten für Eindringlinge oder für unbefugten Zugriff.
- Inkonsistente Bereitstellungsprozesse können Sicherheitslücken herbeiführen, die zu Datenverlusten oder Unterbrechungen führen können.
- Konfigurationsabweichungen oder fehlende Patches können unbeabsichtigte Sicherheitslücken zur Folge haben, die zu Datenverlusten oder Unterbrechungen führen können.
- Verschiedenartige Edge-Geräte können die Kosten für den Netzwerkbetrieb erhöhen.
- Verschiedenartige Gerätekonfigurationen können zu Fehlern in der Konfiguration und zur Gefährdung der Sicherheit führen.
- Das Team für Cybersicherheit besteht darauf, dass durch das Generieren von Verschlüsselungsschlüsseln auf der Plattform eines einzigen Cloudanbieters das Risiko einer Anbieterabhängigkeit besteht. Auch wenn diese Sorge unbegründet ist, wurde dies vom Team vorerst akzeptiert.

## <a name="evolution-of-the-policy-statements"></a>Weiterentwicklung der Richtlinienanweisungen

Die folgenden Änderungen an der Richtlinie verringern die neuen Risiken und vereinfachen die Implementierung. Die Liste wirkt lang, aber die Einführung dieser Richtlinien ist möglicherweise einfacher, als es den Anschein hat.

1. Alle bereitgestellten Ressourcen müssen nach Wichtigkeit und Datenklassifizierung kategorisiert werden. Vor der Bereitstellung in der Cloud müssen die Klassifizierungen durch das Cloudgovernanceteam und die Anwendung überprüft werden.
2. Anwendungen, die geschützte Daten speichern oder darauf zugreifen, sind anders zu verwalten als Anwendungen, die nicht mit geschützten Daten arbeiten. Zumindest müssen sie segmentiert werden, um einen unbeabsichtigten Zugriff auf geschützte Daten zu vermeiden.
3. Alle geschützten Daten müssen im Ruhezustand verschlüsselt sein.
4. Erhöhte Berechtigungen in einem Segment mit geschützten Daten müssen eine Ausnahme bleiben. Solche Ausnahmen werden vom Cloud Governance-Team erfasst und regelmäßig überwacht.
5. Netzwerksubnetze mit geschützten Daten müssen von allen anderen Subnetzen isoliert werden. Der Netzwerkdatenverkehr zwischen Subnetzen mit geschützten Daten wird regelmäßig überwacht.
6. Kein Subnetz mit geschützten Daten ist direkt über das öffentliche Internet oder rechenzentrumsübergreifend zugänglich. Der Zugriff auf diese Subnetze muss über zwischengeschaltete Subnetzwerke geroutet werden. Der gesamte Zugriff auf diese Subnetze muss über eine Firewalllösung erfolgen, die Funktionen zur Paketüberprüfung und Sperrfunktionen durchführen kann.
7. Die Anforderungen an die Netzwerkkonfiguration, die vom Sicherheitsmanagementteam definiert wurden, müssen mit Governancetools überwacht und durchgesetzt werden.
8. Governancetools müssen die VM-Bereitstellung auf genehmigte Images begrenzen.
9. Nach Möglichkeit muss die Knotenkonfigurationsverwaltung Richtlinienanforderungen auf die Konfiguration aller Gastbetriebssysteme anwenden. Die Knotenkonfigurationsverwaltung muss die vorhandene Investition in das Gruppenrichtlinienobjekt (Group Policy Object, GPO) für die Ressourcenkonfiguration berücksichtigen.
10. Governancetools überwachen die Aktivierung automatischer Updates für alle bereitgestellten Assets. Nach Möglichkeit werden automatische Updates durchgesetzt. Wenn sie nicht durch Tools erzwungen werden, müssen Verstöße auf Knotenebene von Betriebsmanagementteams überprüft und in Übereinstimmung mit den Betriebsrichtlinien beseitigt werden. Ressourcen, die nicht automatisch aktualisiert werden, müssen in Prozesse des IT-Betriebs einbezogen werden.
11. Für die Erstellung neuer Abonnements oder Verwaltungsgruppen für unternehmenskritische Anwendungen oder geschützte Daten ist eine Überprüfung durch das Cloudgovernanceteam erforderlich, um eine angemessene Blaupausenzuweisung sicherzustellen.
12. Das Zugriffsmodell der geringsten Rechte wird auf alle Abonnements angewendet, die unternehmenskritische Anwendungen oder geschützte Daten enthalten.
13. Der Cloudanbieter muss Verschlüsselungsschlüssel integrieren können, die von der vorhandenen lokalen Lösung verwaltet werden.
14. Der Cloudanbieter muss die vorhandene Lösung für Edge-Geräte und alle erforderlichen Konfigurationen zum Schutz öffentlich zugänglicher Netzwerkgrenzen unterstützen können.
15. Der Cloudanbieter muss eine freigegebene Verbindung mit dem globalen WAN unterstützen können, bei der die Datenübertragung über die vorhandene Edge-Gerätelösung geroutet wird.
16. Trends und Exploits, die mögliche Auswirkungen auf Cloudbereitstellungen haben, müssen vom Sicherheitsteam regelmäßig überprüft werden, damit Updates für in der Cloud verwendete Sicherheitsbaselinetools bereitgestellt werden.
17. Bereitstellungstools müssen vom Cloudgovernanceteam genehmigt werden, um eine kontinuierliche Governance für bereitgestellte Ressourcen sicherzustellen.
18. Bereitstellungsskripts müssen in einem zentralen Repository aufbewahrt werden, das für das Cloud Governance-Team zur regelmäßigen Überprüfung und Überwachung zugänglich ist.
19. Governanceprozesse müssen Überwachungen zum Bereitstellungszeitpunkt und in regelmäßigen Zyklen umfassen, um für alle Ressourcen Konsistenz zu gewährleisten.
20. Die Bereitstellung von Anwendungen, für die eine Kundenauthentifizierung erforderlich ist, erfordert einen genehmigten Identitätsanbieter, der mit dem primären Identitätsanbieter für interne Benutzer kompatibel ist.
21. Cloudgovernanceprozesse müssen vierteljährliche Überprüfungen durch Identitätsbasisteams umfassen, um böswillige Akteure oder Nutzungsmuster zu identifizieren, die durch die Cloudressourcenkonfiguration verhindert werden sollten.

## <a name="evolution-of-the-best-practices"></a>Weiterentwicklung bewährter Methoden

In diesem Abschnitt des Artikels wird der Governance-MVP-Entwurf so weiterentwickelt, dass er neue Azure-Richtlinien und eine Implementierung von Azure Cost Management umfasst. Zusammen erfüllen diese beiden Entwurfsänderungen die neuen Richtlinienanweisungen des Unternehmens.

Die neuen bewährten Methoden lassen sich in zwei Kategorien unterteilen: Unternehmens-IT (Hub) und Cloudeinführung (Spoke).

**Einrichten eines Hub-Spoke-Abonnements für die Unternehmens-IT zum Zentralisieren der Sicherheitsbaseline**: In dieser bewährten Methode wird die vorhandene Governancekapazität von einer [Hub-Spoke-Topologie mit Shared Services][shared-services] umschlossen, mit einigen wichtigen Ergänzungen des Cloudgovernanceteams.

1. Azure DevOps-Repository. Erstellen Sie in Azure DevOps ein Repository zur Speicherung und Versionsverwaltung für alle relevanten Azure Resource Manager-Vorlagen und Skriptkonfigurationen.
2. Hub-Spoke-Vorlage.
    1. Die Anleitung in der [Hub-Spoke-Referenzarchitektur mit Shared Services][shared-services] kann zum Generieren von Resource Manager-Vorlagen für die Ressourcen verwendet werden, die in einem Unternehmens-IT-Hub erforderlich sind.
    2. Mit diesen Vorlagen kann diese Struktur im Rahmen einer zentralen Governancestrategie wiederholbar gemacht werden.
    3. Zusätzlich zur aktuellen Referenzarchitektur wird empfohlen, eine NSG-Vorlage (Netzwerksicherheitsgruppe) zu erstellen, die alle Anforderungen zu Portsperren oder Whitelists für das VNET erfasst, das die Firewall hosten soll. Diese NSG unterscheidet sich von früheren NSGs, weil sie als erste NSG öffentlichen Datenverkehr in einem VNET zulässt.
3. Erstellen von Azure-Richtlinien. Erstellen Sie eine Richtlinie namens `Hub NSG Enforcement`, um die Konfiguration der NSG durchzusetzen, die einem in diesem Abonnement erstellten VNET zugewiesen ist. Wenden Sie die integrierten Richtlinien für die Gastkonfiguration wie folgt an:
    1. Überwachen Sie die Verwendung sicherer Kommunikationsprotokolle auf Windows-Webservern.
    2. Überwachen Sie die korrekte Festlegung der Kennwortsicherheitseinstellungen auf Linux- und Windows-Computern.
4. Unternehmens-IT-Blaupause.
    1. Erstellen Sie eine Azure-Blaupause namens `corporate-it-subscription`.
    2. Fügen Sie die Hub-Spoke-Vorlagen und die Hub-NSG-Richtlinie hinzu.
5. Erweitern der anfänglichen Verwaltungsgruppenhierarchie.
    1. Für jede Verwaltungsgruppe, die Unterstützung für geschützte Daten angefordert hat, bietet die Blaupause `corporate-it-subscription-blueprint` eine beschleunigte Hublösung.
    2. Da Verwaltungsgruppen in diesem fiktiven Beispiel neben einer Geschäftseinheitshierarchie auch eine regionale Hierarchie umfassen, wird diese Blaupause in jeder Region bereitgestellt.
    3. Erstellen Sie für jede Region in der Verwaltungsgruppenhierarchie ein Abonnement namens `Corporate IT Subscription`.
    4. Wenden Sie die Blaupause `corporate-it-subscription-blueprint` auf die einzelnen regionalen Instanzen an.
    5. Dadurch wird für jede Geschäftseinheit in jeder Region ein Hub eingerichtet. Hinweis: Weitere Kosteneinsparungen können durch die Freigabe von Hubs für Geschäftseinheiten in den einzelnen Regionen erzielt werden.
6. Integrieren von Gruppenrichtlinienobjekten (GPO) über DSC (Desired State Configuration):
    1. Konvertieren Sie ein GPO in DSC. Das Projekt zur [Microsoft-Basislinienverwaltung](https://github.com/Microsoft/BaselineManagement) auf Github kann diese Aufgabe beschleunigen. * Stellen Sie sicher, dass DSC im Repository parallel zu den Resource Manager-Vorlagen gespeichert wird.
    2. Stellen Sie Azure Automation DSC für alle Instanzen des Unternehmens-IT-Abonnements bereit. Mit Azure Automation kann DSC auf virtuelle Computer angewendet werden, die in unterstützten Abonnements innerhalb der Verwaltungsgruppe bereitgestellt werden.
    3. In der aktuellen Roadmap ist die Aktivierung benutzerdefinierter Gastkonfigurationsrichtlinien geplant. Wenn dieses Feature veröffentlicht wird, ist die Verwendung von Azure Automation in dieser bewährten Methode nicht mehr erforderlich.

**Anwenden zusätzlicher Governance auf ein Cloudeinführungsabonnement (Spoke)**: Aufbauend auf dem `Corporate IT Subscription` können geringfügige Änderungen am Governance-MVP, das auf die einzelnen Abonnements zur Unterstützung von Anwendungsarchetypen angewendet wird, für eine schnelle Weiterentwicklung sorgen.

In früheren Weiterentwicklungen der bewährte Methode wurden NSGs definiert, die öffentlichen Datenverkehr blockiert und internen Datenverkehr per Whitelist zugelassen haben. Darüber hinaus wurden durch die Azure-Blaupause vorübergehend DMZ- und Active Directory-Funktionen erstellt. Im Rahmen dieser Weiterentwicklung werden wir diese Ressourcen ein wenig anpassen und so eine neue Version der Azure-Blaupause erstellen.

1. Vorlage zum Netzwerkpeering. Mit dieser Vorlage wird das VNET in den einzelnen Abonnements mit dem Hub-VNET im Unternehmens-IT-Abonnement gekoppelt.
    1. In der Anleitung aus dem vorherigen Abschnitt, [Hub-Spoke-Referenzarchitektur mit Shared Services][shared-services], wurde eine Resource Manager-Vorlage zum Aktivieren des VNET-Peerings generiert.
    2. Diese Vorlage kann als Anleitung zum Ändern der DMZ-Vorlage aus der vorherigen Governanceentwicklung verwendet werden.
    3. Im Wesentlichen fügen wir jetzt VNET-Peering dem DMZ-VNET hinzu, das zuvor mit dem lokalen Edge-Gerät über VPN verbunden war.
    4. *** Es empfiehlt sich außerdem, das VPN aus dieser Vorlage zu entfernen und sicherzustellen, dass kein Datenverkehr direkt an das lokale Rechenzentrum weitergeleitet wird, ohne das Unternehmens-IT-Abonnement und die Firewalllösung zu passieren.
    5. Es ist noch eine zusätzliche [Netzwerkkonfiguration](/azure/automation/automation-dsc-overview#network-planning) erforderlich, damit Azure Automation DSC auf gehostete virtuelle Computer anwendet.
2. Ändern Sie die NSG. Blockieren Sie den gesamten öffentlichen UND direkten lokalen Datenverkehr in der NSG. Der einzige eingehende Datenverkehr sollte über den VNET-Peer im Unternehmens-IT-Abonnement eintreffen.
    1. In der vorherigen Weiterentwicklung wurde eine NSG erstellt, die dem gesamten öffentlichen Datenverkehr blockiert und den gesamten internen Datenverkehr über eine Whitelist zulässt. Jetzt möchten wir diese NSG etwas verlagern.
    2. In der neuen NSG-Konfiguration wird der gesamte öffentliche Datenverkehr sowie der gesamte Datenverkehr aus dem lokalen Rechenzentrum blockiert.
    3. Bei diesem VNET eingehender Datenverkehr darf nur von dem VNET auf der anderen Seite des VNET-Peers stammen.
3. Implementierung von Azure Security Center.
    1. Konfigurieren Sie Azure Security Center für jede Verwaltungsgruppe, die Klassifizierungen geschützter Daten enthält.
    2. Legen Sie automatische Bereitstellung standardmäßig auf „Aktiviert“ fest, um Patchingkonformität zu gewährleisten.
    3. Richten Sie Sicherheitskonfigurationen von Betriebssystemen ein. Das IT-Sicherheitsteam definiert die Konfiguration.
    4. Unterstützen Sie das IT-Sicherheitsteam bei der anfänglichen Verwendung von Azure Security Center. Übertragen Sie die Verwendung von Security Center an das IT-Sicherheitsteam, aber behalten Sie den Zugriff, um die Governance stetig zu verbessern.
    5. Erstellen Sie eine Resource Manager-Vorlage, die die erforderlichen Änderungen für die Azure Security Center-Konfiguration in einem Abonnement widerspiegelt.
4. Aktualisieren Sie Azure Policy für alle Abonnements.
    1. Überprüfen und erzwingen Sie die Wichtigkeits- und Datenklassifizierung für alle Verwaltungsgruppen und Abonnements, um Abonnements mit Klassifizierungen geschützter Daten zu identifizieren.
    2. Überwachen und erzwingen Sie die ausschließliche Verwendung genehmigter Betriebssystemimages.
    3. Überwachen und erzwingen Sie Gastkonfigurationen basierend auf den Sicherheitsanforderungen für die einzelnen Knoten.
5. Aktualisieren Sie Azure Policy für alle Abonnements, die Klassifizierungen geschützter Daten enthalten.
    1. Überwachen und erzwingen Sie die ausschließliche Verwendung von Standardrollen.
    2. Überwachen und erzwingen Sie die Anwendung der Verschlüsselung für alle Speicherkonten und Dateien, die sich auf den einzelnen Knoten im Ruhezustand befinden.
    3. Überwachen und erzwingen Sie die Anwendung der neuen Version der DMZ-Netzwerksicherheitsgruppe.
    4. Überwachen und erzwingen Sie die Verwendung eines genehmigten Netzwerksubnetzes und VNET pro Netzwerkschnittstelle.
    5. Überwachen und erzwingen Sie die Einschränkung benutzerdefinierter Routingtabellen.
6. Azure-Blaupause:
    1. Erstellen Sie eine Azure-Blaupause namens `protected-data`.
    2. Fügen Sie der Blaupause die VNET-Peer-, NSG- und Azure Security Center-Vorlagen hinzu.
    3. Stellen Sie sicher, dass die Vorlage für Active Directory aus der vorherigen Weiterentwicklung NICHT in der Blaupause enthalten ist. Alle Abhängigkeiten von Active Directory werden vom Unternehmens-IT-Abonnement bereitgestellt.
    4. Beenden Sie alle vorhandenen Active Directory-VMs, die in der vorherigen Weiterentwicklung bereitgestellt wurden.
    5. Fügen Sie die neuen Richtlinien für Abonnements von geschützten Daten hinzu.
    6. Veröffentlichen Sie die Blaupause für alle Verwaltungsgruppen, die zum Hosten geschützter Daten bestimmt sind.
    7. Wenden Sie die neue Blaupause zusammen mit vorhandenen Blaupausen auf die betroffenen Abonnements an.

## <a name="conclusion"></a>Zusammenfassung

Durch das Hinzufügen dieser Prozesse und Änderungen zum Governance-MVP lassen sich viele Risiken im Zusammenhang mit der Sicherheitsgovernance verringern. Zusammen stellen Sie die Tools zur Überwachung von Netzwerk, Identität und Sicherheit bereit, die zum Schutz von Daten erforderlich sind.

## <a name="next-steps"></a>Nächste Schritte

Im fortschreitenden Verlauf der Cloudeinführung steigen neben dem Geschäftswert auch die Risiken und der Bedarf an Cloudgovernance. Für das fiktive Unternehmen in diesem Beispiel besteht der nächste Schritt darin, unternehmenskritische Workloads zu unterstützen. An diesem Punkt werden Steuerelemente zur Ressourcenkonsistenz benötigt.

> [!div class="nextstepaction"]
> [Entwicklung der Ressourcenkonsistenz](./resource-consistency-evolution.md)

<!-- links -->

[shared-services]: ../../../../reference-architectures/hybrid-networking/shared-services.md