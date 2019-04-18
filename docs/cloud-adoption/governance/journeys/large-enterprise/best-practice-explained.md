---
title: 'Framework für die Cloudeinführung (Cloud Adoption Framework, CAF): Große Unternehmen: Beschreibung der bewährten Methode'
titleSuffix: Microsoft Cloud Adoption Framework for Azure
ms.service: architecture-center
ms.subservice: enterprise-cloud-adoption
ms.custom: governance
ms.date: 02/11/2019
description: 'Große Unternehmen: Beschreibung der bewährten Methode'
author: BrianBlanchard
ms.openlocfilehash: 2d52797f1c3541fab1c97d97d0438210d2e66f79
ms.sourcegitcommit: 579c39ff4b776704ead17a006bf24cd4cdc65edd
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/17/2019
ms.locfileid: "59068989"
---
# <a name="large-enterprise-best-practice-explained"></a>Große Unternehmen: Beschreibung der bewährten Methode

Der Governanceprozess beginnt mit einer Sammlung von anfänglichen [Unternehmensrichtlinien](./initial-corporate-policy.md). Diese Richtlinien werden verwendet, um ein Miminum Viable-Product (MVP, minimal brauchbares Produkt) für die Governance einzurichten, das [bewährte Methoden](./overview.md) berücksichtigt.

In diesem Artikel werden die allgemeinen Strategien behandelt, die zum Erstellen eines Governance-MVP erforderlich sind. Der Kern des Governance-MVP ist das Verfahren zur [Beschleunigung der Bereitstellung](../../deployment-acceleration/overview.md). Die in dieser Phase angewandten Tools und Muster ermöglichen die inkrementellen Entwicklungen, die für die zukünftige Erweiterung von Governance erforderlich sind.

## <a name="governance-mvp-cloud-adoption-foundation"></a>Governance-MVP (Cloud Adoption Foundation)

Die schnelle Einführung von Governance und Unternehmensrichtlinien ist dank einiger einfacher Prinzipien und cloudbasierter Governancetools möglich. Dies sind die ersten der drei Governance-Verfahren, die in jedem Governanceprozess zur Anwendung kommen. Jedes dieser Verfahren wird in diesem Artikel behandelt.

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

Das **gemischte** Muster wurde für Azure-Abonnements ausgewählt.

- Wenn neue Anforderungen für Azure-Ressourcen auftreten, sollte für jede größere Geschäftseinheit in jeder geografischen Betriebsregion eine „Abteilung“ eingerichtet werden. Innerhalb jeder der Abteilungen sollten „Abonnements“ für jeden Anwendungsarchetyp erstellt werden.
- Ein Anwendungsarchetyp ist eine Möglichkeit, Anwendungen mit ähnlichen Anwendungen zu gruppieren. Häufige Beispiele sind: Anwendungen mit geschützten Daten, verwaltete Anwendungen (wie HIPAA oder FedRAMP), risikoarme Anwendungen, Anwendungen mit lokalen Abhängigkeiten, SAP oder andere Mainframes in Azure oder Anwendungen, die lokales SAP oder lokale Mainframes erweitern. Jedes Unternehmen hat individuelle Anforderungen, die auf Datenklassifikationen und den Arten von Anwendungen basieren, die das Geschäft unterstützen. Die Abhängigkeitszuordnung der digitalen Infrastruktur kann bei der Definition der Anwendungsarchitekturen in einem Unternehmen helfen.
- Im Rahmen des Abonnementdesigns sollte auf der Grundlage der beiden vorangegangenen Aufzählungspunkte eine gemeinsame Namenskonvention vereinbart werden.

### <a name="resource-consistency"></a>Ressourcenkonsistenz

**Hierarchische Konsistenz** wurde als Muster der Ressourcenkonsistenz gewählt.

- Für jede Anwendung sollten Ressourcengruppen erstellt werden. Für jeden Anwendungsarchetyp sollten Verwaltungsgruppen erstellt werden. Azure Policy sollte auf alle Abonnements in der zugehörigen Verwaltungsgruppe angewendet werden.
- Im Rahmen des Bereitstellungsprozesses sollten Ressourcenkonsistenzvorlagen für alle Ressourcen in der Quellcodeverwaltung gespeichert werden.
- Jede Ressourcengruppe sollte an einer bestimmten Workload oder Anwendung ausgerichtet werden.
- Die definierte Hierarchie der Azure-Verwaltungsgruppen sollte die Abrechnungszuständigkeit und den Anwendungsbesitz mithilfe von verschachtelten Gruppen darstellen.
- Eine umfassende Implementierung von Azure Policy könnte die zeitlichen Verpflichtungen des Teams überschreiten und bietet an diesem Punkt möglicherweise keinen großen Mehrwert. Allerdings sollte eine einfache Standardrichtlinie erstellt und auf jede Ressourcengruppe angewendet werden, um die wenigen ersten Richtlinienerklärungen zur Cloud-Governance durchzusetzen. Diese dient dazu, die Implementierung spezifischer Governanceanforderungen zu definieren. Diese Implementierungen können dann auf alle bereitgestellten Ressourcen angewendet werden.

### <a name="resource-tagging"></a>Ressourcenkennzeichnung (Tagging)

Für die Ressourcenkennzeichnung wurde das Muster **Buchhaltung** gewählt.

- Bereitgestellte Ressourcen sollten mit Werten für Folgendes gekennzeichnet werden: Abrechnung/Kostenstelle, Geografie, Datenklassifizierung, Kritikalität, SLA, Umgebung, Anwendungsarchetyp, Anwendung und Anwendungsbesitzer.
- Diese Werte beeinflussen in Kombination mit der Azure-Verwaltungsgruppe und dem einer bereitgestellten Ressource zugeordneten Abonnement die Entscheidungen in den Bereichen Governance, Betrieb und Sicherheit.

### <a name="logging-and-reporting"></a>Protokollierung und Berichterstellung

An dieser Stelle wird für Protokollierung und Berichterstellung ein **Hybrid**-Muster vorgeschlagen, es ist aber für keins der Entwicklungsteams erforderlich.

- Es sind aktuell keine Governanceanforderungen in Bezug auf die für die Protokollierung oder Berichterstellung zu erfassenden spezifischen Datenpunkte festgelegt. Dies ist für diese fiktive Erzählung spezifisch und sollte als Antimuster betrachtet werden. Protokollierungsstandards sollten so früh wie möglich festgelegt und durchgesetzt werden.
- Vor der Freigabe geschützter Daten oder unternehmenskritischer Workloads sind zusätzliche Analysen erforderlich.
- Vor der Unterstützung geschützter Daten oder unternehmenskritischer Workloads muss der bestehenden lokalen Betriebsüberwachungslösung Zugang zu dem für die Protokollierung verwendeten Workspace gewährt werden. Anwendungen müssen die mit der Nutzung des betreffenden Mandanten einhergehenden Sicherheits- und Protokollierungsanforderungen erfüllen, wenn die Anwendung mit einem definierten SLA unterstützt werden soll.

## <a name="evolution-of-governance-processes"></a>Weiterentwicklung von Governanceprozessen

Einige der Richtlinienanweisungen können oder sollten nicht durch automatisierte Tools gesteuert werden. Andere Richtlinien erfordern regelmäßigen Aufwand seitens der IT-Sicherheits- und Identitätsbaselineteams. Das Cloud Governance-Team muss die folgenden Prozesse beaufsichtigen, um die letzten acht Richtlinienanweisungen zu implementieren:

**Änderungen der Unternehmensrichtlinie**: Das Cloud Governance-Team nimmt Änderungen am Entwurf des Governance-MVP vor, um die neuen Richtlinien zu übernehmen. Der Wert des Governance-MVP besteht darin, dass es die automatische Durchsetzung der neuen Richtlinien ermöglicht.

**Schnellere Einführung**: Das Cloud Governance-Team hat Bereitstellungsskripts in mehreren Teams überprüft. Es verwaltet eine Reihe von Skripts, die als Bereitstellungsvorlagen dienen. Diese Vorlagen können von den Cloudeinführungs- und DevOps-Teams verwendet werden, um Bereitstellungen schneller zu definieren. Jedes Skript enthält die Anforderungen für die Durchsetzung von Governance-Richtlinien, und zusätzliche Anstrengungen von Technikern für die Cloudeinführung sind nicht erforderlich. Als Kurator dieser Skripts kann das Cloud Governance-Team Richtlinienänderungen schneller umsetzen. Darüber hinaus werden sie als Beschleuniger der Einführung angesehen. Dadurch sind konsistente Bereitstellungen sichergestellt, ohne dass die Einhaltung streng durchgesetzt werden müsste.

**Schulung der Techniker**: Das Cloud Governance-Team bietet alle zwei Monate Schulungen an und hat zwei Videos für Techniker erstellt. Beide Ressourcen helfen Technikern, schnell mit der Governance-Kultur und der Durchführung von Bereitstellungen vertraut zu werden. Das Team fügt Schulungsressourcen hinzu, die den Unterschied zwischen Produktions- und Nicht-Produktionsbereitstellungen zeigen, was Technikern das Verständnis erleichtert, wie sich die neuen Richtlinien auf die Einführung auswirken. Dadurch sind konsistente Bereitstellungen sichergestellt, ohne dass die Einhaltung streng durchgesetzt werden müsste.

**Bereitstellungsplanung**: Vor der Bereitstellung von Ressourcen, die geschützte Daten enthalten, muss das Cloud Governance-Team anhand der Bereitstellungsskripts die Governanceausrichtung überprüfen. Vorhandene Teams mit bereits genehmigten Bereitstellungen werden mithilfe von programmgesteuerten Tools überprüft.

**Monatliche Überprüfung und Berichterstellung**: Jeden Monat führt das Cloud Governance-Team eine Überprüfung aller Cloudbereitstellungen durch, um die kontinuierliche Einhaltung der Richtlinien zu überprüfen. Wenn Abweichungen festgestellt werden, werden sie dokumentiert und an die Cloudeinführungsteams weitergegeben. Wenn die Durchsetzung keine Betriebsunterbrechung oder Datenlecks zur Folge hat, werden die Richtlinien automatisch durchgesetzt. Am Ende der Überprüfung erstellt das Cloud Governance-Team einen Bericht für das Cloud Strategy-Team und jedes Cloudeinführungsteam, um die allgemeine Einhaltung der Richtlinien zu kommunizieren. Der Bericht wird auch für prüfungsbezogene und rechtliche Zwecke gespeichert.

**Vierteljährliche Richtlinienüberprüfung**: Jedes Quartal überprüfen das Cloud Governance-Team und das Cloud Strategy-Team die Überwachungsergebnisse und schlagen Änderungen der Unternehmensrichtlinien vor. Viele dieser Vorschläge sind das Ergebnis kontinuierlicher Verbesserungen und der Beobachtung von Nutzungsmustern. Genehmigte Richtlinienänderungen werden während der nachfolgenden Überwachungszyklen in die Governancetools integriert.

## <a name="alternative-patterns"></a>Alternative Muster

Wenn eines der in dieser Governancelösung ausgewählten Muster nicht mit den Anforderungen Ihres Unternehmens übereinstimmt, gibt es Alternativen zu jedem Muster:

- [Verschlüsselungsmuster](../../../decision-guides/encryption/overview.md)
- [Identitätsmuster](../../../decision-guides/identity/overview.md)
- [Protokollierungs- und Berichterstellungsmuster](../../../decision-guides/log-and-report/overview.md)
- [Richtliniendurchsetzungsmuster](../../../decision-guides/policy-enforcement/overview.md)
- [Ressourcenkonsistenzmuster](../../../decision-guides/resource-consistency/overview.md)
- [Ressourcenkennzeichnungsmuster](../../../decision-guides/resource-tagging/overview.md)
- [Softwaredefinierte Netzwerkmuster](../../../decision-guides/software-defined-network/overview.md)
- [Abonnemententwurfsmuster](../../../decision-guides/subscriptions/overview.md)

## <a name="next-steps"></a>Nächste Schritte

Sobald dieser Leitfaden implementiert wurde, kann jedes Cloudeinführungsteam seine Arbeit mit einer soliden Governancebasis fortsetzen. Das Cloud Governance-Team arbeitet parallel daran, die Unternehmensrichtlinien und Governanceverfahren kontinuierlich zu aktualisieren.

Beide Teams verwenden die Toleranzindikatoren, um die nächste Entwicklung zu identifizieren, die erforderlich ist, um die Einführung der Cloud weiterhin zu unterstützen. Der nächste Schritt für das Unternehmen auf diesem Weg ist die Weiterentwicklung seiner Governance-Basislinie zur Unterstützung von Anwendungen mit Legacy- oder Drittanbieter-Multifaktor-Authentifizierung (MFA).

> [!div class="nextstepaction"]
> [Entwicklung der Identitätsbaseline](./identity-baseline-evolution.md)
