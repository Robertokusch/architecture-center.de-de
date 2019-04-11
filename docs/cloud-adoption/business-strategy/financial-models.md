---
title: 'CAF: Erstellen eines Finanzmodells für die Cloudtransformation'
titleSuffix: Microsoft Cloud Adoption Framework for Azure
ms.service: architecture-center
ms.subservice: enterprise-cloud-adoption
ms.custom: governance
description: Erfahren Sie, wie Sie ein Finanzmodell für die Cloudtransformation erstellen.
author: BrianBlanchard
ms.date: 12/10/2018
ms.topic: guide
ms.openlocfilehash: 1f3ed8a84b84ba577ad5e5db8b1becd318dc04a3
ms.sourcegitcommit: 0a8a60d782facc294f7f78ec0e9033e3ee16bf4a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/08/2019
ms.locfileid: "59068870"
---
# <a name="create-a-financial-model-for-cloud-transformation"></a>Erstellen eines Finanzmodells für die Cloudtransformation

Die Erstellung eines Finanzmodells, mit dem der gesamte Geschäftswert einer Cloudtransformation präzise dargestellt wird, kann kompliziert sein. Finanzmodelle und geschäftliche Begründungen unterscheiden sich meist von Organisation zu Organisation. In diesem Artikel sind einige Formeln enthalten, und es wird auf einige Punkte hingewiesen, die beim Erstellen eines Finanzmodells häufig vergessen werden.

## <a name="return-on-investment-roi"></a>Rendite (ROI)

Die Rendite (Return on Investment, ROI) ist für die Führungs- bzw. Vorstandsebene häufig ein wichtiges Kriterium. Der ROI-Wert wird verwendet, um unterschiedliche Möglichkeiten in Bezug auf die Investition von begrenztem Kapital zu vergleichen. Die ROI-Formel ist recht einfach. Die Details, die zum Ermitteln der einzelnen Eingabewerte für die Formel benötigt werden, sind dagegen nicht ganz so einfach zu erlangen. Die Rendite ist im Wesentlichen der Ertrag, der sich aus einer Erstinvestition ergibt. Der Wert wird normalerweise als Prozentsatz dargestellt:

![Return on Investment (ROI) = (Investitionsertrag - Investitionskosten) / Investitionskosten](../_images/formula-roi.png)

<!-- markdownlint-disable MD036 -->
<!--*ROI = (Gain from Investment &minus; Initial Investment) / Initial Investment*-->
<!-- markdownlint-enable MD036 -->

In den nächsten Abschnitten werden die Daten beschrieben, die zum Berechnen der Erstinvestition und des Investitionsertrags (Einnahmen) benötigt werden.

## <a name="calculating-initial-investment"></a>Berechnen der Erstinvestition

Die Erstinvestition umfasst die Investitionskosten (Capital Expenditure, CapEx) und Betriebskosten (Operating Expenditure, OpEx), die für die Durchführung einer Transformation erforderlich sind. Die Klassifizierung der Kosten kann je nach Buchhaltungsmodell und Präferenz des CFO variieren. In der Regel enthält diese Kategorie aber Folgendes: Zu transformierende Dienstleistungen, Softwarelizenzen, die ausschließlich während der Transformation genutzt werden, Kosten der Clouddienste während der Transformation und ggf. die Kosten für die Mitarbeitergehälter während der Transformation.

Addieren Sie diese Kosten, um eine Schätzung der Erstinvestition zu erhalten.

## <a name="calculating-the-gain-from-investment"></a>Berechnen des Investitionsertrags

Zum Berechnen des Investitionsertrags wird häufig noch eine zweite Formel benötigt, die stark von den Geschäftsergebnissen und den damit verbundenen technischen Änderungen abhängt. Die Einnahmen lassen sich nicht so einfach wie eine Kostenreduzierung berechnen.

Für die Berechnung der Einnahmen sind zwei Variablen erforderlich:

![Investitionsertrag = Umsatzdeltawerte + Kostendeltawerte](../_images/formula-gain-from-investment.png)

<!-- markdownlint-disable MD036 -->
<!--*Gain from Investment = Revenue Deltas + Cost Deltas*-->
<!-- markdownlint-enable MD036 -->

Die einzelnen Elemente sind unten beschrieben.

## <a name="revenue-delta"></a>Umsatzdeltawert

Der Umsatzdeltawert sollte in Zusammenarbeit mit dem Unternehmen prognostiziert werden. Nachdem sich die Projektbeteiligten des Unternehmens auf die Auswirkungen auf den Umsatz geeinigt haben, kann diese Information zur Verbesserung der Einnahmenposition verwendet werden.

## <a name="cost-deltas"></a>Kostendeltawerte

Kostendeltawerte umfassen die Erhöhung bzw. Verringerung, die sich aus der Transformation ergibt. Es gibt einige unabhängige Variablen, die sich auf die Kostendeltawerte auswirken können. Die Einnahmen basieren größtenteils auf harten Kostenfaktoren wie Investitionskostenreduzierung, Kostenvermeidung, Betriebskostenreduzierung und Abschreibungskostenreduzierung. Die folgenden Abschnitte enthalten Beispiele für die zu berücksichtigenden Kostendeltawerte.

### <a name="depreciation-reductions-or-acceleration"></a>Reduzierung oder Beschleunigung von Abschreibungen

Informationen zu Abschreibungen erhalten Sie bei Ihrem CFO bzw. von Ihrer Finanzabteilung. Hier sind zum Thema Abschreibungen nur einige allgemeine Informationen angegeben.

Wenn beim Erwerb einer Ressource Kapital investiert wird, kann diese Investition für finanzielle oder steuerliche Zwecke genutzt werden, um sich für die zu erwartende Lebensdauer der Ressource Vorteile zu verschaffen. Einige Unternehmen betrachten Abschreibungen als Steuervorteil. Andere sehen sie als zwingende, ständige Ausgaben an, die anderen wiederkehrenden Ausgaben im IT-Jahresbudget ähneln.

Wenden Sie sich an Ihre Finanzabteilung, um zu ermitteln, ob ein Verzicht auf Abschreibungen möglich ist und sich dadurch eine positive Auswirkung auf die Kostendeltawerte ergibt.

### <a name="physical-asset-recovery"></a>Kostendeckung für physische Ressourcen

In einigen Fällen können ausgesonderte Ressourcen verkauft werden, um Umsatz zu generieren. Dieser Umsatz wird der Einfachheit halber oft der Kostenreduzierung zugeordnet. Es handelt sich aber eigentlich um eine Steigerung des Umsatzes, die unter Umständen entsprechend besteuert wird. Wenden Sie sich an Ihre Finanzabteilung, um Informationen zu erhalten, ob diese Option geeignet ist und wie der daraus erzielte Umsatz angegeben werden muss.

### <a name="operational-cost-reductions"></a>Betriebskostenreduzierung

Wiederkehrende Ausgaben, die für den Betrieb des Unternehmens anfallen, werden häufig als Betriebskosten (OpEx) bezeichnet. Die Kategorie der Betriebskosten ist sehr umfangreich. In den meisten Buchhaltungsmodellen umfassen sie Softwarelizenzierung, Ausgaben für Hosting, Stromkosten, Mietkosten für Gebäude, Ausgaben für Kühlung, benötigtes Zeitarbeitspersonal, Mietkosten für Ausrüstung, Ersatzteile, Wartungsverträge, Reparaturdienstleistungen, Dienstleistungen für Geschäftskontinuität/Notfallwiederherstellung und einige andere Ausgaben, für die keine Investitionsanträge erforderlich sind.

Diese Kategorie stellt bei der betrieblichen Transformation einen der größten Einnahmenbereiche dar. Zeit, die in die Optimierung der Liste mit diesen Punkten investiert wird, ist nur selten verschwendete Zeit. Stellen Sie dem CIO und der Finanzabteilung entsprechende Fragen, um sicherzustellen, dass alle Betriebskosten berücksichtigt werden.

### <a name="cost-avoidance"></a>Kostenvermeidung

Wenn Betriebskosten zu erwarten, aber noch nicht Teil eines genehmigten Budgets sind, können sie unter Umständen noch nicht einer Kostenreduzierungskategorie zugeordnet werden. Falls beispielsweise VMWare- und Microsoft-Lizenzen im nächsten Jahr neu ausgehandelt und bezahlt werden müssen, handelt es sich noch nicht um vollständig relevante Kosten. Reduzierungen dieser zu erwartenden Kosten werden in Bezug auf die Berechnung von Kostendeltawerten dann wie Betriebskosten behandelt. Auf nicht formaler Ebene sollten sie aber als „Kostenvermeidung“ bezeichnet werden, bis die Aushandlung erfolgt ist und das Budget genehmigt wurde.

### <a name="soft-cost-reductions"></a>Reduzierungen von weichen Kosten

In einigen Unternehmen können auch weiche Kosten enthalten sein, z.B. Reduzierung der betrieblichen Komplexität oder Reduzierung der Anzahl von Vollzeitmitarbeitern, die für den Betrieb eines Datencenters benötigt werden. Die Einbeziehung von weichen Kosten ist aber nicht immer ratsam. Mit der Einbeziehung von weichen Kosten wird die nicht dokumentierte Annahme getroffen, dass die Kostenreduzierung auch zu greifbaren Kosteneinsparungen führt. Bei Technologieprojekten kommt es aber nur selten zur Deckung von weichen Kosten.

### <a name="headcount-reductions"></a>Reduzierung der Mitarbeiterzahl

Zeiteinsparungen beim Personal werden häufig der Reduzierung von weichen Kosten zugerechnet. Wenn diese Zeiteinsparungen zu einer tatsächlichen Reduzierung des IT-Gehalts oder -Personals führen, kann dies separat als Reduzierung der Mitarbeiterzahl angerechnet werden.

Die lokal benötigten Fähigkeiten sind im Allgemeinen aber in ähnlichem (oder höherem) Maße auch in der Cloud erforderlich. Dies bedeutet, dass nach einer Cloudmigration in der Regel kein Personal abgebaut wird.

Eine Ausnahme dieser Regel ist ein Fall, in dem für den Betrieb erforderliche Dienste von einem Drittanbieter oder Anbieter von verwalteten Diensten (Managed Services Provider, MSP) bereitgestellt werden. Wenn IT-Systeme von einem Drittanbieter verwaltet werden, können die Betriebskosten durch eine cloudnative Lösung oder einen cloudnativen MSP ersetzt werden. Ein cloudnativer MSP arbeitet wahrscheinlich effizienter und unter Umständen auch kostengünstiger. Wenn dies der Fall ist, sind Betriebskostenreduzierungen den harten Kosten zuzuordnen.

### <a name="capital-expense-reductions-or-avoidance"></a>Investitionskostenreduzierung oder -vermeidung

Investitionskosten (CapEx) unterscheiden sich leicht von den Betriebskosten. In dieser Kategorie geht es normalerweise um Aktualisierungszyklen oder Datencentererweiterungen. Ein Beispiel für die Erweiterung eines Datencenters ist ein neuer Hochleistungscluster zum Hosten einer Big Data-Lösung oder eines Data Warehouse. Dies gehört im Allgemeinen zur Kategorie „Investitionskosten“. Üblicher sind die grundlegenden Aktualisierungszyklen. Einige Unternehmen verfügen über starre Hardwareaktualisierungszyklen, bei denen Ressourcen nach einem regelmäßigen Zyklus ausgesondert und ausgetauscht werden (normalerweise alle drei, fünf oder acht Jahre). Diese Zyklen stimmen oft mit Leasezyklen für Ressourcen oder der prognostizierten Lebensdauer von Ausrüstung überein. Wenn ein Aktualisierungszyklus ansteht, fallen in der IT-Abteilung Investitionskosten zur Beschaffung neuer Ausrüstung an.

Falls ein Aktualisierungszyklus genehmigt und mit einem Budget versehen wurde, kann eine Cloudtransformation dazu beitragen, diese Kosten zu beseitigen. Wenn ein Aktualisierungszyklus geplant wurde, aber die Genehmigung noch nicht erteilt wurde, kann die Cloudtransformation eine Vermeidung der Investitionskosten bewirken. Beide Szenarien fließen in den Kostendeltawert ein.

## <a name="next-steps"></a>Nächste Schritte

Sehen Sie sich einige Beispiele für Finanzergebnisse im Kontext einer Cloudtransformation an.

> [!div class="nextstepaction"]
> [Beispiele für Finanzergebnisse](./business-outcomes/fiscal-outcomes.md)