---
title: 'CAF: Große Unternehmen: Multi-Cloud-Entwicklung'
titleSuffix: Microsoft Cloud Adoption Framework for Azure
ms.service: architecture-center
ms.subservice: enterprise-cloud-adoption
ms.custom: governance
ms.date: 02/11/2019
description: 'Große Unternehmen: Multi-Cloud-Entwicklung'
author: BrianBlanchard
ms.openlocfilehash: 5ef29aa523c04ff93b2d4f983482f94654a4a039
ms.sourcegitcommit: 273e690c0cfabbc3822089c7d8bc743ef41d2b6e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/08/2019
ms.locfileid: "55901810"
---
# <a name="large-enterprise-multi-cloud-evolution"></a>Große Unternehmen: Multi-Cloud-Entwicklung

## <a name="evolution-of-the-narrative"></a>Weiterentwicklung der Lösung

Microsoft ist sich bewusst, dass Kunden für bestimmte Zwecke mehrere Clouds nutzen. Das fiktive Unternehmen in diesem Beitrag bildet da keine Ausnahme. Parallel zur Einführung von Azure hat der geschäftliche Erfolg zur Übernahme eines kleinen, aber ergänzenden Unternehmens geführt. Das Unternehmen führt sämtliche IT-Vorgänge bei einem anderen Cloudanbieter aus.

In diesem Artikel wird beschrieben, wie sich die Dinge ändern, wenn die neue Organisation integriert wird. Im Rahmen der Lösung wird davon ausgegangen, dass dieses Unternehmen alle Entwicklungsschritte zur Governance abgeschlossen hat, die in dieser Customer Journey beschrieben wurden.

### <a name="evolution-of-the-current-state"></a>Weiterentwicklung des aktuellen Status

In der vorherigen Phase dieser Lösung hatte das Unternehmen damit begonnen, Kostenkontrollen und Kostenüberwachung zu implementieren, da Cloudausgaben Teil der regulären Betriebskosten des Unternehmens werden.

Seit diesem Zeitpunkt haben sich einige Dinge geändert, die sich auf die Governance auswirken:

- Die Identität wird von einer lokalen Instanz von Active Directory kontrolliert. Hybrididentität wird mithilfe von Replikation in Azure Active Directory vereinfacht.
- Der IT- oder Cloudbetrieb wird hauptsächlich von Azure Monitor und verwandten Automatisierungen verwaltet.
- Notfallwiederherstellung und Geschäftskontinuität werden von Azure Vault-Instanzen kontrolliert.
- Für die Überwachung von Sicherheitsverletzungen und Angriffen wird Azure Security Center genutzt.
- Azure Security Center und Azure Monitor werden zusammen für die Überwachung der Governance in der Cloud verwendet.
- Azure Blueprints, Azure Policy und Verwaltungsgruppen werden verwendet, um die Richtlinienkonformität zu automatisieren.

### <a name="evolution-of-the-future-state"></a>Weiterentwicklung des zukünftigen Status

Das Ziel ist die Integration des übernommenen Unternehmens in den bestehenden Betrieb, wo immer dies möglich ist.

## <a name="evolution-of-tangible-risks"></a>Entwicklung materieller Risiken

**Kosten der Geschäftsübernahme**: Die Übernahme des neuen Unternehmens soll sich in ungefähr fünf Jahren amortisieren. Aufgrund der langsamen Rendite will der Vorstand die Anschaffungskosten so weit wie möglich kontrollieren. Es besteht die Gefahr von Konflikten zwischen Kostenkontrolle und technischer Integration.

Dieses Unternehmensrisiko lässt sich auf einige technische Risiken erweitern

- Es besteht die Gefahr, dass die Migration in die Cloud weitere Übernahmekosten verursacht.
- Es besteht außerdem die Gefahr, dass die neue Umgebung nicht ordnungsgemäß verwaltet ist oder es zu Richtlinienverstößen kommt.

## <a name="evolution-of-the-policy-statements"></a>Weiterentwicklung der Richtlinienanweisungen

Die folgenden Änderungen an der Richtlinie verringern die neuen Risiken und vereinfachen die Implementierung.

1. Die Überwachung sämtlicher Ressourcen in einer sekundären Cloud muss über die vorhandenen Tools zur Geschäftsverwaltung und Sicherheitsüberwachung erfolgen.
2. Alle Organisationseinheiten müssen mit dem bestehenden Identitätsanbieter integriert werden.
3. Der primäre Identitätsanbieter sollte die Authentifizierung für Ressourcen in der sekundären Cloud steuern.

## <a name="evolution-of-the-best-practices"></a>Weiterentwicklung bewährter Methoden

In diesem Abschnitt des Artikels wird der Governance-MVP-Entwurf so weiterentwickelt, dass er neue Azure-Richtlinien und eine Implementierung von Azure Cost Management umfasst. Zusammen erfüllen diese beiden Entwurfsänderungen die neuen Richtlinienanweisungen des Unternehmens.

1. Verbinden der Netzwerke: vom Netzwerk- und IT-Sicherheitsteam mit Unterstützung durch Governance durchgeführt
    1. Das Hinzufügen einer Verbindung vom MPLS-/Mietleitungsanbieter zur neuen Cloud bewirkt die Integration der Netzwerke. Über das Hinzufügen von Routingtabellen und Firewallkonfigurationen werden der Zugriff und der Datenverkehr zwischen den Umgebungen gesteuert.
2. Konsolidieren der Identitätsanbieter. Abhängig von den Workloads, die in der sekundären Cloud gehostet werden, gibt es mehrere Optionen für das Zusammenführen der Identitätsanbieter. Im Folgenden finden Sie einige Beispiele:
    1. Für Anwendungen mit einer Authentifizierung per OAuth 2 können Benutzer von Active Directory in der sekundären Cloud einfach zum vorhandenen Azure AD-Mandanten repliziert werden.
    2. Im anderen Extremfall würde ein Partnerverbund zwischen den zwei lokalen Identitätsanbietern die Replikation der Benutzer aus den neuen Active Directory-Domänen nach Azure ermöglichen.
3. Hinzufügen von Ressourcen zu Azure Site Recovery
    1. Azure Site Recovery wurde von Anfang an als Hybrid Cloud-/Multi-Cloud-Tool entwickelt.
    2. Virtuelle Computer in der sekundären Cloud können möglicherweise über die gleichen Azure Site Recovery-Prozesse geschützt werden, die auch für lokale Ressourcen verwendet werden.
4. Hinzufügen von Ressourcen zu Azure Cost Management
    1. Azure Cost Management wurde von Anfang an als Multi-Cloud-Tool entwickelt.
    2. Virtuelle Computer in der sekundären Cloud sind bei einigen Cloudanbietern eventuell mit Azure Cost Management kompatibel. Es können zusätzliche Kosten anfallen.
5. Hinzufügen von Ressourcen zu Azure Monitor
    1. Azure Monitor wurde von Anfang an als ein Hybrid Cloud-Tool konzipiert.
    2. Virtuelle Computer in der sekundären Cloud sind möglicherweise mit Azure Monitor-Agents kompatibel, sodass sie für die Überwachung der Abläufe in Azure Monitor eingebunden werden können.
6. Tools für die Durchsetzung von Governance
    1. Die Durchsetzung von Governance ist cloudspezifisch.
    2. Die Unternehmensrichtlinien, die in der Governance Journey eingerichtet wurden, sind hingegen nicht cloudspezifisch. Obwohl die Implementierung von Cloud zu Cloud variieren kann, können die Richtlinienanweisungen auf den sekundären Anbieter angewandt werden.

Mit zunehmender Einführung der Multi-Cloud-Umgebung schreitet auch die oben beschriebene Entwicklung voran.

## <a name="next-steps"></a>Nächste Schritte

In vielen großen Unternehmen können sich die Fachrichtungen der Cloud Governance als Einführungshindernisse erweisen. Der nächste Artikel enthält einige abschließende Gedanken darüber, wie Governance zu einem Mannschaftssport gemacht werden kann, um den langfristigen Erfolg in der Cloud zu sichern.

> [!div class="nextstepaction"]
> [Mehrere Governance-Ebenen](./multiple-layers-of-governance.md)
