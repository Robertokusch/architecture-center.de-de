---
title: 'CAF: Kleine bis mittlere Unternehmen – zusätzliche technische Details in Bezug auf das Governance-MVP'
titleSuffix: Microsoft Cloud Adoption Framework for Azure
ms.service: architecture-center
ms.subservice: enterprise-cloud-adoption
ms.custom: governance
ms.date: 02/11/2019
description: 'Erläuterung: Kleine bis mittlere Unternehmen – zusätzliche technische Details in Bezug auf das Governance-MVP'
author: BrianBlanchard
ms.openlocfilehash: e726213459c8bee63e3cc77ab54868fe7196b3ac
ms.sourcegitcommit: 273e690c0cfabbc3822089c7d8bc743ef41d2b6e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/08/2019
ms.locfileid: "55901560"
---
# <a name="small-to-medium-enterprise-best-practice-explained"></a>Kleine bis mittlere Unternehmen: Beschreibung der bewährten Methode

Der Governanceprozess beginnt mit einer Sammlung von anfänglichen [Unternehmensrichtlinien](./initial-corporate-policy.md). Diese Richtlinien werden verwendet, um ein Governance-MVP einzurichten, das [bewährte Methoden](./overview.md) berücksichtigt.

In diesem Artikel werden die allgemeinen Strategien behandelt, die zum Erstellen eines Governance-MVP erforderlich sind. Der Kern des Governance-MVP ist das Verfahren zur [Beschleunigung der Bereitstellung](../../deployment-acceleration/overview.md). Die in dieser Phase angewandten Tools und Muster ermöglichen die inkrementellen Entwicklungen, die für die zukünftige Erweiterung von Governance erforderlich sind.

## <a name="governance-mvp-cloud-adoption-foundation"></a>Governance-MVP (Cloud Adoption Foundation)

Die schnelle Einführung von Governance und Unternehmensrichtlinien ist dank einiger einfacher Prinzipien und cloudbasierter Governancetools möglich. Dies sind die ersten der drei Cloud Governance-Verfahren, die in jedem Governanceprozess zur Anwendung kommen. Jedes dieser Verfahren wird in diesem Artikel behandelt.

Als Ausgangspunkt werden in diesem Artikel die High-Level-Strategien hinter Identitätsbaseline, Sicherheitsbaseline und Beschleunigung der Bereitstellung behandelt, die für die Erstellung eines Governance-MVP erforderlich sind, das als Grundlage für jede Umsetzung dient.

![Beispiel für ein inkrementelles Governance-MVP](../../../_images/governance/governance-mvp.png)

## <a name="implementation-process"></a>Implementierungsprozess

Die Implementierung des Governance-MVP weist Abhängigkeiten von Identität, Sicherheit und Netzwerk auf. Sobald die Abhängigkeiten aufgelöst wurden, entscheidet das Cloud Governance-Team über einige Aspekte von Governance. Die Entscheidungen des Cloud Governance-Teams und der unterstützenden Teams werden durch ein einziges Paket von Durchsetzungsmaßnahmen umgesetzt.

![Beispiel für ein inkrementelles Governance-MVP](../../../_images/governance/governance-mvp-implementation-flow.png)

Diese Implementierung kann auch mithilfe einer einfachen Prüfliste beschrieben werden:

1. Einholen von Entscheidungen über Kernabhängigkeiten: Identität, Netzwerk und Verschlüsselung.
2. Bestimmen des Musters, das bei der Durchsetzung von Unternehmensrichtlinien verwendet werden soll.
3. Bestimmen der geeigneten Governancemuster für die Disziplinen Ressourcenkonsistenz, Ressourcentagging, Protokollierung und Berichterstellung.
4. Implementieren der Governancetools, die auf das gewählte Muster der Richtliniendurchsetzung abgestimmt sind, um die abhängigen Entscheidungen und Governanceentscheidungen anzuwenden.

[!INCLUDE [implementation-process](../../../../../includes/cloud-adoption/governance/implementation-process.md)]

## <a name="application-of-governance-defined-patterns"></a>Anwenden von durch Governance definierten Mustern

Das Cloud Governance-Team ist für die folgenden Entscheidungen und Implementierungen verantwortlich. Viele Entscheidungen erfordern Informationen von anderen Teams, aber das Cloud Governance Team wird wahrscheinlich sowohl für die endgültige Entscheidung als auch für die Implementierung verantwortlich sein. In den folgenden Abschnitten werden die für diesen Anwendungsfall getroffenen Entscheidungen und Details zu jeder Entscheidung beschrieben.

### <a name="subscription-model"></a>Abonnementmodell

Das Muster **Anwendungskategorie** wurde für Azure-Abonnements ausgewählt.

- Ein Anwendungsarchetyp ist eine Möglichkeit, Anwendungen mit ähnlichen Anforderungen zu gruppieren. Häufige Beispiele sind: Anwendungen mit geschützten Daten, verwaltete Anwendungen (wie HIPAA oder FedRAMP), risikoarme Anwendungen, Anwendungen mit lokalen Abhängigkeiten, SAP oder andere Mainframes in Azure oder Anwendungen, die lokales SAP oder lokale Mainframes erweitern. Diese Archetypen sind pro Unternehmen einzigartig und basieren auf Datenklassifikationen und den Typen von Anwendungen, die das Unternehmen ausmachen. Die Abhängigkeitszuordnung der digitalen Infrastruktur kann bei der Definition der Anwendungsarchitekturen in einem Unternehmen helfen.
- Abteilungen werden angesichts des aktuellen Schwerpunkts wahrscheinlich nicht erforderlich sein. Es wird erwartet, dass die Bereitstellungen innerhalb einer einzigen Abrechnungseinheit eingeschränkt sind. Im Stadium der Einführung gibt es möglicherweise nicht einmal eine Unternehmensvereinbarung zur Zentralisierung der Abrechnung. Es ist wahrscheinlich, dass dieser Grad der Einführung von einem einzigen Azure-Abonnement mit nutzungsbasierter Bezahlung verwaltet wird.
- Unabhängig von der Verwendung des EA Portals oder dem Vorhandensein einer Unternehmensvereinbarung sollte trotzdem ein Abonnementmodell definiert und vereinbart werden, um den Verwaltungsmehraufwand über die reine Abrechnung hinaus zu minimieren.
- Im Muster **Anwendungskategorie** werden Abonnements für jeden Anwendungsarchetyp erstellt. Jedes Abonnement gehört zu einem Konto pro Umgebung (Entwicklung, Test und Produktion).
- Im Rahmen des Abonnementdesigns sollte auf der Grundlage der beiden vorangegangenen Punkte eine gemeinsame Namenskonvention vereinbart werden.

### <a name="resource-consistency"></a>Ressourcenkonsistenz

Das Muster **Bereitstellungskonsistenz** wurde als Ressourcenkonsistenz ausgewählt.

- Ressourcengruppen werden für jede Anwendung erstellt. Verwaltungsgruppen werden für jeden Anwendungsarchetyp erstellt. Azure Policy sollte auf alle Abonnements aus der zugehörigen Verwaltungsgruppe angewendet werden.
- Im Rahmen des Bereitstellungsprozesses sollten Azure Resource Consistency-Vorlagen für die Ressourcengruppe in der Quellcodeverwaltung gespeichert werden.
- Jede Ressourcengruppe ist einer bestimmten Workload oder Anwendung zugeordnet.
- Azure-Verwaltungsgruppen ermöglichen das Aktualisieren der Governanceentwürfe, wenn sich die Unternehmensrichtlinie weiterentwickelt.
- Eine umfassende Implementierung von Azure Policy könnte die zeitlichen Verpflichtungen des Teams überschreiten und zu diesem Zeitpunkt möglicherweise keinen großen Mehrwert bieten. Allerdings sollte eine einfache Standardrichtlinie erstellt und auf jede Verwaltungsgruppe angewendet werden, um die geringe Anzahl der aktuellen Richtlinienerklärungen zur Cloud-Governance durchzusetzen. In dieser Richtlinie wird die Implementierung spezifischer Governanceanforderungen definiert. Diese Implementierungen können dann auf alle bereitgestellten Ressourcen angewendet werden.

### <a name="resource-tagging"></a>Tags für Ressourcen

Das Muster **Klassifizierung** zur Kennzeichnung wurde als ein Modell für Ressourcentagging ausgewählt.

- Bereitgestellte Ressourcen sollten mit den folgenden Werten gekennzeichnet werden: Datenklassifizierung, Wichtigkeit, SLA und Umgebung.
- Diese vier Werte sind für Governance-, Vorgangs- und Sicherheitentscheidungen wichtig.
- Wenn diese Governancelösung für eine Geschäftseinheit oder ein Team innerhalb eines größeren Unternehmens implementiert wird, sollte das Tagging auch Metadaten für die Abrechnungseinheit enthalten.

### <a name="logging-and-reporting"></a>Protokollierung und Berichterstellung

An dieser Stelle wird ein **cloudnatives** Muster für Protokollierung und Berichterstellung vorgeschlagen, aber von keinem Entwicklungsteam verlangt.

- Es wurden keine Governanceanforderungen in Bezug auf die für die Protokollierung oder Berichterstellung zu erfassenden Daten festgelegt.
- Vor der Freigabe geschützter Daten oder unternehmenskritischer Workloads sind zusätzliche Analysen erforderlich.

## <a name="evolution-of-governance-processes"></a>Weiterentwicklung von Governanceprozessen

Im Zuge der Weiterentwicklung von Governance können oder sollten einige Richtlinienerklärungen nicht durch automatisierte Tools kontrolliert werden. Andere Richtlinien werden im Laufe der Zeit zu einem erhöhten Aufwand für das IT-Sicherheitsteam und das lokale Identity Management-Team führen. Um neu auftretende Risiken zu minimieren, wird das Cloud Governance-Team die folgenden Prozesse überwachen.

**Schnellere Einführung**: Das Cloud Governance-Team hat Bereitstellungsskripts in mehreren Teams überprüft. Es verwaltet eine Reihe von Skripts, die als Bereitstellungsvorlagen dienen. Diese Vorlagen werden von den Cloudeinführungs- und DevOps-Teams verwendet, um Bereitstellungen schneller zu definieren. Jedes dieser Skripts enthält die erforderlichen Anforderungen, um eine Reihe von Governancerichtlinien durchzusetzen, und zwar ohne zusätzlichen Aufwand für Cloudeinführungs-Engineers. Als Kurator dieser Skripts kann das Cloud Governance-Team Richtlinienänderungen schneller umsetzen. Als Ergebnis der Skriptkuratierung wird das Cloud Governance-Team als Quelle der Beschleunigung der Einführung angesehen. Dies schafft Konsistenz zwischen den Bereitstellungen, ohne die Einhaltung streng zu erzwingen.

**Schulung der Engineers**: Das Cloud Governance-Team bietet alle zwei Monate Schulungen an und hat zwei Videos für Engineers erstellt. Diese Materialien helfen den Engineers, die Governancekultur und die Vorgehensweise bei der Bereitstellung schnell zu erlernen. Das Team fügt Schulungsressourcen hinzu, die den Unterschied zwischen Produktions- und Nicht-Produktionsbereitstellungen zeigen, sodass die Engineers verstehen, wie sich die neuen Richtlinien auf die Einführung auswirken. Dies schafft Konsistenz zwischen den Bereitstellungen, ohne die Einhaltung streng zu erzwingen.

**Bereitstellungsplanung**: Vor der Bereitstellung von Ressourcen, die geschützte Daten enthalten, überprüft das Cloud Governance-Team die Bereitstellungsskripts, um die Ausrichtung von Governance zu überprüfen. Vorhandene Teams mit bereits genehmigten Bereitstellungen werden mithilfe von programmgesteuerten Tools überprüft.

**Monatliche Überprüfung und Berichterstellung**: Jeden Monat führt das Cloud Governance-Team eine Überprüfung aller Cloudbereitstellungen durch, um die kontinuierliche Einhaltung der Richtlinien zu überprüfen. Wenn Abweichungen festgestellt werden, werden sie dokumentiert und an die Cloudeinführungsteams weitergegeben. Wenn die Durchsetzung keine Betriebsunterbrechung oder Datenlecks zur Folge hat, werden die Richtlinien automatisch durchgesetzt. Am Ende der Überprüfung erstellt das Cloud Governance-Team einen Bericht für das Cloud Strategy-Team und jedes Cloudeinführungsteam, um die allgemeine Einhaltung der Richtlinien zu kommunizieren. Der Bericht wird außerdem für Überwachung und rechtliche Zwecke gespeichert.

**Vierteljährliche Richtlinienüberprüfung**: Jedes Quartal überprüfen das Cloud Governance-Team und das Cloud Strategy-Team die Überwachungsergebnisse und schlagen Änderungen der Unternehmensrichtlinien vor. Viele dieser Vorschläge sind das Ergebnis kontinuierlicher Verbesserungen und der Beobachtung von Nutzungsmustern. Genehmigte Richtlinienänderungen werden während der nachfolgenden Überwachungszyklen in die Governancetools integriert.

## <a name="alternative-patterns"></a>Alternative Muster

Wenn eines der in dieser Governancelösung ausgewählten Muster nicht mit den Anforderungen Ihres Unternehmens übereinstimmt, gibt es Alternativen zu jedem Muster:

- [Verschlüsselungsmuster](../../../decision-guides/encryption/overview.md)
- [Identitätsmuster](../../../decision-guides/identity/overview.md)
- [Protokollierungs- und Berichterstellungsmuster](../../../decision-guides/log-and-report/overview.md)
- [Richtliniendurchsetzungsmuster](../../../decision-guides/policy-enforcement/overview.md)
- [Ressourcenkonsistenzmuster](../../../decision-guides/resource-consistency/overview.md)
- [Ressourcentaggingmuster](../../../decision-guides/resource-tagging/overview.md)
- [Softwaredefinierte Netzwerkmuster](../../../decision-guides/software-defined-network/overview.md)
- [Abonnemententwurfsmuster](../../../decision-guides/subscriptions/overview.md)

## <a name="next-steps"></a>Nächste Schritte

Sobald dieser Leitfaden implementiert wurde, kann jedes Cloudeinführungsteam seine Arbeit mit einer soliden Governancebasis fortsetzen. Das Cloud Governance-Team arbeitet parallel daran, die Unternehmensrichtlinien und Governanceverfahren kontinuierlich zu aktualisieren.

Die beiden Teams verwenden die Toleranzindikatoren, um die nächste Entwicklung zu identifizieren, die erforderlich ist, um die Einführung der Cloud weiterhin zu unterstützen. Für das fiktive Unternehmen in dieser Lösung ist der nächste Schritt die Entwicklung der Sicherheitsbaseline zur Unterstützung des Verschiebens geschützter Daten in die Cloud.

> [!div class="nextstepaction"]
> [Entwicklung der Sicherheitsbaseline](./security-baseline-evolution.md)