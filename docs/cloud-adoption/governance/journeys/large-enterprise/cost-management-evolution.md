---
title: 'CAF: Große Unternehmen: Weiterentwicklung des Kostenmanagements'
titleSuffix: Microsoft Cloud Adoption Framework for Azure
ms.service: architecture-center
ms.subservice: enterprise-cloud-adoption
ms.custom: governance
ms.date: 02/11/2019
description: 'Große Unternehmen: Weiterentwicklung des Kostenmanagements'
author: BrianBlanchard
ms.openlocfilehash: 6bf63e36f6fb19dd0e5f9a16a7f66eb6e0ed54ff
ms.sourcegitcommit: 273e690c0cfabbc3822089c7d8bc743ef41d2b6e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/08/2019
ms.locfileid: "55901685"
---
# <a name="large-enterprise-cost-management-evolution"></a>Große Unternehmen: Weiterentwicklung des Kostenmanagements

Dieser Artikel führt die Lösung weiter, indem der Minimum Viable Product-Governance Kostenkontrolle hinzugefügt wird.

## <a name="evolution-of-the-narrative"></a>Weiterentwicklung der Lösung

Die Einführung hat sich über den im Governance-MVP definierten Toleranzindikator hinaus entwickelt. Die Steigerung in den Ausgaben rechtfertigt jetzt den Aufwand an Zeit seitens des Cloud Governance-Teams für die Überwachung und Kontrolle von Ausgabenmustern.

Als treibende Kraft der Innovation wird IT nicht mehr in erster Linie als Kostenstelle betrachtet. Da die IT-Organisation Mehrwert generiert, stimmen der CIO und CFO überein, dass jetzt die Zeit gekommen ist, die Rolle der IT im Unternehmen weiterzuentwickeln. Unter anderem will der CFO einen Direktvergütungsansatz für die Cloud-Rechnungslegung der kanadischen Niederlassung einer der Geschäftseinheiten testen. Eins der beiden stillgelegten Rechenzentren hostete exklusiv Ressourcen für den kanadischen Betrieb der besagten Geschäftseinheit. In diesem Modell werden der kanadischen Niederlassung der Geschäftseinheit die Betriebsausgaben im Zusammenhang mit den gehosteten Ressourcen direkt in Rechnung gestellt. Dieses Modell ermöglicht es der IT, sich weniger auf die Verwaltung der Ausgaben anderer als vielmehr auf die Wertschöpfung zu konzentrieren. Bevor dieser Übergang erfolgen kann, müssen jedoch Kostenverwaltungstools implementiert worden sein.

### <a name="evolution-of-current-state"></a>Weiterentwicklung des aktuellen Status

In der vorherigen Phase dieser Erzählung hat das IT-Team Produktionsworkloads mit geschützten Daten aktiv nach Azure verschoben.

Seit dieser Zeit haben sich einige Dinge geändert, die sich auf die Governance auswirken:

- 5.000 Ressourcen wurden aus den beiden zur Stilllegung markierten Rechenzentren entfernt. Auftragswesen und IT-Sicherheit heben nun die Bereitstellung der verbleibenden physischen Ressourcen auf.
- Die Anwendungsentwicklungsteams haben CI/CD-Pipelines implementiert, um eine Reihe von nativen Cloudanwendungen bereitzustellen, mit erheblichen Auswirkungen auf das Kundenerlebnis.
- Das Business Intelligence-Team hat Aggregations-, Zusammenstellungs-, Erkenntnis- und Prognoseprozesse entwickelt, die konkrete Vorteile für den Geschäftsbetrieb bringen. Diese Prognosen ermöglichen jetzt kreative neue Produkte und Dienstleistungen.

### <a name="evolution-of-future-state"></a>Weiterentwicklung des zukünftigen Status

- Die Cloudlösung soll um Kostenüberwachung und -berichterstellung erweitert werden. Die Berichterstellung sollte die direkten Betriebskosten an die Funktionen binden, die die Cloudkosten verbrauchen. Zusätzliche Berichte sollten es der IT-Abteilung ermöglichen, die Ausgaben zu überwachen und technische Hinweise zum Kostenmanagement zu geben. Für die kanadische Zweigstelle wird direkt mit der Abteilung abgerechnet.

## <a name="evolution-of-tangible-risks"></a>Entwicklung materieller Risiken

**Budgetkontrolle**: Es besteht die Gefahr, dass Self-Service-Funktionen zu überhöhten und unerwarteten Kosten auf der neuen Plattform führen. Es müssen Governance-Prozesse zur Kostenüberwachung und Reduzierung der laufenden Kostenrisiken wirksam sein, um eine kontinuierliche Ausrichtung am Planbudget zu gewährleisten.

Dieses Unternehmensrisiko lässt sich auf einige technische Risiken erweitern:

- Es besteht die Gefahr, dass die Ist-Kosten den Plan übersteigen.
- Geschäftsbedingungen ändern sich. Wenn dieser Fall eintritt, werden einzelne Geschäftsfunktion mehr Clouddienste nutzen müssen als erwartet, was zu Anomalien bei den Ausgaben führt. Es besteht das Risiko, dass diese zusätzlichen Kosten als überschießend angesehen werden, statt als notwendige Anpassung des Plans. Bei Erfolg sollte das kanadische Experiment ein Beitrag zum Abmildern dieses Risikos sein.
- Es besteht die Gefahr überdimensionierter Systeme, die Mehrausgaben verursachen.

## <a name="evolution-of-the-policy-statements"></a>Weiterentwicklung der Richtlinienanweisungen

Die folgenden Änderungen an der Richtlinie verringern die neuen Risiken und vereinfachen die Implementierung.

1. Alle Cloudkosten sollten wöchentlich vom Cloud Governance-Team anhand eines Plans überwacht werden. Berichte zu Abweichungen zwischen den Cloudkosten und dem Plan sind monatlich mit der IT-Leitung und der Finanzabteilung zu teilen. Alle Cloudkosten und Planaktualisierungen sollten monatlich zusammen mit der IT-Leitung und der Finanzabteilung überprüft werden.
2. Alle Kosten müssen zum Zweck der Rechnungslegung einer Geschäftsfunktion zugeordnet sein
3. Cloudressourcen sollten fortlaufend auf Optimierungsmöglichkeiten überprüft werden
4. Die Cloud Governance-Tools müssen die Dimensionierungsoptionen für Ressourcen auf eine genehmigte Liste mit Konfigurationen einschränken. Die Tools müssen sicherstellen, dass alle Ressourcen ermittelbar sind und von der Lösung zur Kostenüberwachung verfolgt werden.
5. Während der Bereitstellungsplanung sollten alle erforderlichen Cloudressourcen, die dem Hosten von Produktionsworkloads dienen, dokumentiert werden. Diese Dokumentation kann bei der Verfeinerung der Budgets und bei der Vorbereitung zusätzlicher Automatisierungen helfen, um den Einsatz teurerer Optionen zu vermeiden. Während dieses Prozesses sollten verschiedene vom Cloudanbieter angebotene Rabattierungstools wie z.B. reservierte Instanzen oder Lizenzkostenreduzierungen in Betracht gezogen werden.
6. Alle Besitzer von Anwendungen sind verpflichtet, an Schulungen zu Praktiken zur Optimierung von Workloads teilzunehmen, um die Cloudkosten besser zu kontrollieren.

## <a name="evolution-of-the-best-practices"></a>Weiterentwicklung bewährter Methoden

In diesem Abschnitt des Artikels wird der Governance-MVP-Entwurf so weiterentwickelt, dass er neue Azure-Richtlinien und eine Implementierung von Azure Cost Management umfasst. Zusammen erfüllen diese beiden Entwurfsänderungen die neuen Richtlinienanweisungen des Unternehmens.

1. Änderungen am Azure Enterprise-Portal, um Rechnungen an den Abteilungsadministrator für die kanadische Bereitstellung zu stellen.
2. Implementieren von Azure Cost Management.
    1. Festlegen des richtigen Zugriffsumfangs, um ihn mit dem Abonnementmuster und dem Ressourcengruppierungsmuster abzustimmen. Unter der Annahme der Angleichung an das in früheren Artikeln definierte Governance-MVP sollte dies Zugriff des Umfangs **Registrierungskonto** für das Cloud Governance-Team erfordern, das die Berichterstellung auf hoher Ebene durchführt. Zusätzliche Teams außerhalb der Governance, wie etwa das kanadische Beschaffungsteam, benötigen Zugriff im **Ressourcengruppenumfang**.
    2. Einrichten eines Budgets in Azure Cost Management.
    3. Überprüfen Sie die anfänglichen Empfehlungen, und reagieren Sie entsprechend. Es empfiehlt sich, zur Unterstützung des Berichterstellungsprozesses einen sich wiederholenden Prozess einzurichten.
    4. Konfigurieren Sie Azure Cost Management-Berichterstellung mit anfänglicher und regelmäßiger Ausführung.
3. Aktualisieren von Azure Policy.
    1. Überwachen Sie die Tagging-, Verwaltungsgruppen-, Abonnement- und Ressourcengruppenwerte, um Abweichungen zu identifizieren.
    2. Legen Sie Optionen für die SKU-Größe fest, um die Bereitstellung auf die in der Dokumentation zur Bereitstellungsplanung aufgeführten SKUs zu beschränken.

## <a name="conclusion"></a>Zusammenfassung

Durch das Hinzufügen der oben genannten Prozesse und Änderungen zum Governance-MVP lassen sich viele Risiken im Zusammenhang mit der Kostengovernance verringern. Gemeinsam schaffen sie die Transparenz, Zuverlässigkeit und Optimierung, die zur Kostenkontrolle erforderlich sind.

## <a name="next-steps"></a>Nächste Schritte

Mit der Weiterentwicklung der Cloudeinführung und der damit verbundenen Steigerung des Geschäftswerts entwickeln sich auch die Risiken und Anforderungen an Cloud Governance. Für das fiktive Unternehmen auf diesem Weg ist der nächste Schritt die Nutzung dieser Governanceinvestition zur Verwaltung mehrerer Clouds.

> [!div class="nextstepaction"]
> [Multi-Cloud-Entwicklung](./multi-cloud-evolution.md)