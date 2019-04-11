---
title: 'Framework für die Cloudeinführung (Cloud Adoption Framework, CAF): Kleine bis mittlere Unternehmen: Weiterentwicklung des Kostenmanagements'
titleSuffix: Microsoft Cloud Adoption Framework for Azure
ms.service: architecture-center
ms.subservice: enterprise-cloud-adoption
ms.custom: governance
ms.date: 02/11/2019
description: 'Kleine bis mittlere Unternehmen: Weiterentwicklung des Kostenmanagements'
author: BrianBlanchard
ms.openlocfilehash: cded37b69d538016501d39e7c09e7095a74a96be
ms.sourcegitcommit: c053e6edb429299a0ad9b327888d596c48859d4a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/20/2019
ms.locfileid: "58245181"
---
# <a name="small-to-medium-enterprise-cost-management-evolution"></a>Kleines bis mittleres Unternehmen: Cost Management-Entwicklung

Dieser Artikel führt die Geschichte weiter, indem dem Governance-MVP Kostenkontrolle hinzugefügt wird.

## <a name="evolution-of-the-narrative"></a>Weiterentwicklung der Lösung

Die Einführung hat sich über den im Governance-MVP definierten Kostentoleranzindikator hinaus entwickelt. Das ist gut so, denn dies korrespondiert mit Migrationen aus dem Datencenter „DR“. Die Steigerung in den Ausgaben rechtfertigt jetzt den Aufwand an Zeit seitens des Cloud Governance-Teams.

### <a name="evolution-of-the-current-state"></a>Weiterentwicklung des aktuellen Status

In der vorherigen Phase dieser Lösung hatte die IT 100 % des DR-Datencenters außer Betrieb genommen. Die Anwendungsentwicklungs- und BI-Teams sind für Produktionsdatenverkehr bereit.

Seit diesem Zeitpunkt haben sich einige Dinge geändert, die sich auf die Governance auswirken:

- Das Migrationsteam hat mit der Migration von VMs aus dem Produktionsdatencenter begonnen.
- Die Anwendungsentwicklungsteams übertragen aktiv Produktionsanwendungen über CI/CD-Pipelines in die Cloud. Diese Anwendungen können als Reaktion auf Benutzeranforderungen skaliert werden.
- Das Business Intelligence-Team der IT-Abteilung hat eine Reihe von Predictive Analytics-Tools in der Cloud bereitgestellt. Die Menge der Daten, die in der Cloud aggregiert werden, wächst beständig.
- All dieses Wachstum unterstützt überzeugende Geschäftsergebnisse. Allerdings haben die Kosten begonnen, sich zu vervielfachen. Die prognostizierten Budgets wachsen schneller als erwartet. Der CFO benötigt verbesserte Ansätze für das Kostenmanagement.

### <a name="evolution-of-the-future-state"></a>Weiterentwicklung des zukünftigen Status

Die Cloudlösung soll um Kostenüberwachung und -berichterstellung erweitert werden. Die IT dient nach wie vor als Kostenverrechnungsstelle. Das bedeutet, dass die Vergütung für Clouddienste weiterhin aus der IT-Beschaffung stammt. Die Berichterstellung sollte jedoch die direkten Betriebskosten an die Funktionen binden, die die Cloudkosten verbrauchen. Dieses Modell wird als „Showback“-Modell für Cloudbuchhaltung bezeichnet.

Die Änderungen des aktuellen und zukünftigen Status bergen neue Risiken, die neue Richtlinienanweisungen erfordern.

## <a name="evolution-of-tangible-risks"></a>Entwicklung materieller Risiken

**Budgetkontrolle**: Es besteht die grundsätzliche Gefahr, dass Self-Service-Fähigkeiten zu überhöhten und unerwarteten Kosten auf der neuen Plattform führen. Es müssen Governanceprozesse zur Kostenüberwachung und Reduzierung der laufenden Kostenrisiken wirksam sein, um eine kontinuierliche Ausrichtung am Planbudget zu gewährleisten.

Dieses Unternehmensrisiko lässt sich auf einige technische Risiken erweitern:

- Die Ist-Kosten können den Plan übersteigen.
- Geschäftsbedingungen ändern sich. Wenn dieser Fall eintritt, werden einzelne Geschäftsfunktion mehr Clouddienste nutzen müssen als erwartet, was zu Anomalien bei den Ausgaben führt. Es besteht die Gefahr, dass diese zusätzlichen Ausgaben als Überschüsse betrachtet werden, im Gegensatz zu einer notwendigen Anpassung des Plans.
- Systeme könnten überdimensioniert sein, was zu Ausgabenüberschüssen führen kann.

## <a name="evolution-of-the-policy-statements"></a>Weiterentwicklung der Richtlinienanweisungen

Die folgenden Änderungen an der Richtlinie verringern die neuen Risiken und vereinfachen die Implementierung.

1. Alle Cloudkosten sollten wöchentlich vom Governance-Team anhand eines Plans überwacht werden. Berichte zu Abweichungen zwischen den Cloudkosten und dem Plan sind monatlich mit der IT-Leitung und der Finanzabteilung zu teilen. Alle Cloudkosten und Planaktualisierungen sollten monatlich zusammen mit der IT-Leitung und der Finanzabteilung überprüft werden.
2. Alle Kosten müssen zum Zweck der Rechnungslegung einer Geschäftsfunktion zugeordnet sein.
3. Cloudressourcen sollten fortlaufend auf Optimierungsmöglichkeiten überprüft werden.
4. Die Cloud Governance-Tools müssen die Dimensionierungsoptionen für Ressourcen auf eine genehmigte Liste mit Konfigurationen einschränken. Die Tools müssen sicherstellen, dass alle Ressourcen ermittelbar sind und von der Lösung zur Kostenüberwachung verfolgt werden.
5. Während der Bereitstellungsplanung sollten alle erforderlichen Cloudressourcen, die dem Hosten von Produktionsworkloads dienen, dokumentiert werden. Diese Dokumentation kann bei der Verfeinerung der Budgets und bei der Vorbereitung zusätzlicher Automatisierungen helfen, um den Einsatz teurerer Optionen zu vermeiden. Dabei sollten verschiedene vom Cloudanbieter angebotene Diskontierungstools, z.B. reservierte Instanzen oder Lizenzkostenreduzierungen, in Betracht gezogen werden.
6. Alle Besitzer von Anwendungen sind verpflichtet, an Schulungen zu Praktiken zur Optimierung von Workloads teilzunehmen, um die Cloudkosten besser zu kontrollieren.

## <a name="evolution-of-the-best-practices"></a>Weiterentwicklung bewährter Methoden

In diesem Abschnitt des Artikels wird der Governance-MVP-Entwurf so weiterentwickelt, dass er neue Azure-Richtlinien und eine Implementierung von Azure Cost Management umfasst. Zusammen erfüllen diese beiden Entwurfsänderungen die neuen Richtlinienanweisungen des Unternehmens.

1. Implementieren von Azure Cost Management
    1. Legen Sie den richtigen Zugriffsumfang fest, um ihn an das Abonnementmuster und die Disziplin „Ressourcenkonsistenz“ anzupassen. Unter der Annahme der Angleichung an das in früheren Artikeln definierte Governance-MVP erfordert dies Zugriff des Umfangs **Registrierungskonto** für das Cloud Governance-Team, das die Berichtserstellung auf hoher Ebene durchführt. Weitere Teams außerhalb von Governance benötigen möglicherweise Zugriff des Umfangs **Ressourcengruppe**.
    2. Richten Sie ein Budget in Azure Cost Management ein.
    3. Prüfen und reagieren Sie auf anfängliche Empfehlungen. Nutzen Sie einen regelmäßiger Prozess zur Unterstützung der Berichterstellung.
    4. Konfigurieren Sie Azure Cost Management-Berichterstellung mit anfänglicher und regelmäßiger Ausführung.
2. Aktualisieren von Azure Policy
    1. Überwachen Sie die Tagging-, Verwaltungsgruppen-, Abonnement- und Ressourcengruppenwerte, um Abweichungen zu identifizieren.
    2. Legen Sie Optionen für die SKU-Größe fest, um die Bereitstellung auf die in der Dokumentation zur Bereitstellungsplanung aufgeführten SKUs zu beschränken.

## <a name="conclusion"></a>Zusammenfassung

Durch das Hinzufügen dieser Prozesse und Änderungen zum Governance-MVP lassen sich viele Risiken im Zusammenhang mit der Kostengovernance verringern. Gemeinsam schaffen sie die Transparenz, Zuverlässigkeit und Optimierung, die zur Kostenkontrolle erforderlich sind.

## <a name="next-steps"></a>Nächste Schritte

Mit der Weiterentwicklung der Cloudeinführung und der damit verbundenen Steigerung des Geschäftswerts entwickeln sich auch die Risiken und Anforderungen an Cloud Governance. Für das fiktive Unternehmen auf diesem Weg ist der nächste Schritt die Nutzung dieser Governanceinvestition zur Verwaltung mehrerer Clouds.

> [!div class="nextstepaction"]
> [Multi-Cloud-Entwicklung](./multi-cloud-evolution.md)
