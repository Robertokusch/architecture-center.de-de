---
title: 'Framework für die Cloudeinführung (Cloud Adoption Framework, CAF): Große Unternehmen: Entwicklung der Identitätsbaseline'
titleSuffix: Microsoft Cloud Adoption Framework for Azure
ms.service: architecture-center
ms.subservice: enterprise-cloud-adoption
ms.custom: governance
ms.date: 02/11/2019
description: 'Große Unternehmen: Entwicklung der Identitätsbaseline'
author: BrianBlanchard
ms.openlocfilehash: 7eff9d8e8fd0fd40afa1d9815941fcce8d447579
ms.sourcegitcommit: c053e6edb429299a0ad9b327888d596c48859d4a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/20/2019
ms.locfileid: "58242581"
---
# <a name="large-enterprise-identity-baseline-evolution"></a>Große Unternehmen: Entwicklung der Identitätsbaseline

Dieser Artikel entwickelt die Lösung weiter, indem dem Governance-MVP Steuerungsmechanismen für die Identitätsbaseline hinzugefügt werden.

## <a name="evolution-of-the-narrative"></a>Weiterentwicklung der Lösung

Die geschäftliche Begründung für die Cloudmigration der zwei Rechenzentren wurde vom CFO genehmigt. Im Rahmen der technischen Machbarkeitsstudie wurden mehrere Hindernisse entdeckt:

- Geschützte Daten und geschäftskritische Anwendungen machen 25 % der Workload in den beiden Rechenzentren aus. Beide lassen sich erst nach einer Modernisierung der aktuellen Governance-Richtlinien für personenbezogene Informationen und geschäftskritische Anwendungen lösen.
- 7 % der Ressourcen in diesen Rechenzentren sind nicht cloudkompatibel. Sie werden vor der Ende des Rechenzentrumsvertrags in ein alternatives Rechenzentrum ausgelagert.
- Bei 15 % der Ressourcen im Rechenzentrum (750 virtuelle Computer) besteht eine Abhängigkeit von Legacyauthentifizierung oder mehrstufiger Authentifizierung von Drittanbietern.
- Die VPN-Verbindung, die bestehende Rechenzentren und Azure verbindet, bietet hinsichtlich Datenübertragungsgeschwindigkeiten oder Latenz keine ausreichende Kapazität, um die Menge der Ressourcen innerhalb der zweijährigen Frist bis zur Stilllegung des Rechenzentrums zu migrieren.

Die zwei ersten Hindernisse werden parallel abgebaut. Dieser Artikel behandelt die Beseitigung des dritten und vierten Hindernisses.

### <a name="evolution-of-the-cloud-governance-team"></a>Entwicklung des Cloud Governance-Teams

Das Cloud Governance-Team wächst. Da zusätzliche Unterstützung beim Identitätsmanagement erforderlich ist, nimmt ein Systemadministrator aus dem Identitätsbaselineteam nun an einer wöchentlichen Besprechung teil, um die vorhandenen Teammitglieder über Änderungen auf dem Laufenden zu halten.

### <a name="evolution-of-the-current-state"></a>Weiterentwicklung des aktuellen Status

Das IT-Team hat die Genehmigung, mit den Plänen von CIO und CFO zur Stilllegung von zwei Rechenzentren fortzufahren. Allerdings äußert die IT Bedenken, dass 750 (15 %) der Ressourcen in diesen Rechenzentren an einen anderen Ort außerhalb der Cloud ausgelagert werden müssen.

### <a name="evolution-of-the-future-state"></a>Weiterentwicklung des zukünftigen Status

Die neuen Pläne für den zukünftigen Status erfordern eine robustere Lösung für die Identitätsbaseline, um die 750 virtuellen Computer mit Legacyanforderungen an die Authentifizierung zu migrieren. Über diese zwei Rechenzentren hinaus wird von einem ähnlichen Prozentsatz der Ressourcen in anderen Rechenzentren ausgegangen, die von dieser Herausforderung betroffen sind.
Der zukünftige Status erfordert jetzt ebenfalls eine Verbindung vom Cloudanbieter zur MPLS-/Mietleitungslösung des Unternehmens.

Die Änderungen des aktuellen und zukünftigen Status bergen neue Risiken, die neue Richtlinienanweisungen erfordern.

## <a name="evolution-of-tangible-risks"></a>Entwicklung materieller Risiken

**Unterbrechung des Geschäftsbetriebs während der Migration**. Die Migration in die Cloud schafft ein kontrolliertes, zeitlich begrenztes Risiko, das verwaltet werden kann. Das Verlagern alternder Hardware in einen anderen Teil der Welt bringt ein viel höheres Risiko mit sich. Eine Entschärfungsstrategie ist erforderlich, um Unterbrechungen des Geschäftsbetriebs zu vermeiden.

**Bestehende Identitätsabhängigkeiten**. Abhängigkeiten von bestehenden Authentifizierungs- und Identitätsdiensten können die Migration einiger Workloads in die Cloud verzögern oder verhindern. Erfolgt die Rückgabe der zwei Rechenzentren nicht rechtzeitig, fallen Millionen Dollar an Rechenzentren-Leasinggebühren an.

Dieses Unternehmensrisiko lässt sich auf einige technische Risiken erweitern:

- Möglicherweise steht in der Cloud keine Legacyauthentifizierung zur Verfügung, was die Bereitstellung einiger Anwendungen einschränkt.
- Eventuell ist die aktuelle MFA-Drittanbieterlösung in der Cloud nicht verfügbar, was die Bereitstellung einiger Anwendungen einschränkt.
- Sowohl die Umstellung auf andere Tools als auch die Auslagerung kann zu Ausfällen und höheren Kosten führen.
- Die Geschwindigkeit und Stabilität des VPN kann die Migration beeinträchtigen.
- In die Cloud eingehender Datenverkehr kann zu Sicherheitsproblemen in anderen Teilen des globalen Netzwerks führen.

## <a name="evolution-of-the-policy-statements"></a>Weiterentwicklung der Richtlinienanweisungen

Die folgenden Änderungen an der Richtlinie verringern die neuen Risiken und vereinfachen die Implementierung.

1. Der gewählte Cloudanbieter muss eine Möglichkeit zur Authentifizierung mithilfe von Legacymethoden bieten.
2. Der gewählte Cloudanbieter muss eine Möglichkeit zur Authentifizierung mit der aktuellen MFA-Drittanbieterlösung bieten.
3. Zwischen dem Cloudanbieter und dem Telekommunikationsanbieter des Unternehmens sollte eine private Hochgeschwindigkeitsverbindung eingerichtet werden, die den Cloudanbieter mit dem globalen Rechenzentrumsnetzwerk verbindet.
4. Solange keine ausreichenden Sicherheitsanforderungen implementiert sind, darf kein eingehender öffentlicher Verkehr auf Unternehmensressourcen zugreifen, die in der Cloud gehostet sind. Alle Ports von Quellen außerhalb des globalen WANs sind blockiert.

## <a name="evolution-of-the-best-practices"></a>Weiterentwicklung bewährter Methoden

Der Governance-MVP-Entwurf wird so weiterentwickelt, dass er neue Azure-Richtlinien und eine Implementierung von Active Directory auf einem virtuellen Computer umfasst. Zusammen erfüllen diese beiden Entwurfsänderungen die neuen Richtlinienanweisungen des Unternehmens.

Dies sind die neuen Best Practices:

1. DMZ-Blaupause: Die lokale Seite der DMZ sollte so konfiguriert werden, dass sie Kommunikation zwischen der folgenden Lösung und den lokalen Active Directory-Servern zulässt. Für diese Best Practices ist es erforderlich, dass eine DMZ Active Directory Domain Services über Netzwerkgrenzen hinweg aktiviert.
2. Azure Resource Manager-Vorlagen:
    1. Definieren Sie eine Netzwerksicherheitsgruppe, um externen Datenverkehr zu blockieren und internen Datenverkehr zuzulassen.
    1. Stellen Sie zwei virtuelle AD-Computer als Paar mit Lastenausgleich auf der Grundlage eines Golden Image bereit. Beim ersten Start führt dieses Image ein PowerShell-Skript aus, um den Domänenbeitritt und die Registrierung bei den Domänendiensten vorzunehmen. Weitere Informationen finden Sie unter [Erweitern von Active Directory Domain Services (AD DS) auf Azure](../../../../reference-architectures/identity/adds-extend-domain.md).
3. Azure Policy: Wenden Sie die NSG auf alle Ressourcen an.
4. Azure Blueprint
    1. Erstellen Sie eine Blaupause mit dem Namen `active-directory-virtual-machines`.
    1. Fügen Sie der Blaupause jede der AD-Vorlagen und -Richtlinien hinzu.
    1. Veröffentlichen Sie die Blaupause in jeder betroffenen Verwaltungsgruppe.
    1. Wenden Sie die Blaupause auf alle Abonnements an, für die Legacy- oder MFA-Drittanbieterauthentifizierung erforderlich ist.
    1. Die Instanz von AD, die in Azure ausgeführt wird, kann nun als Erweiterung der lokalen AD-Lösung verwendet werden, was ihr die Integration des vorhandenen MFA-Tools und das Anbieten von anspruchsbasierter Authentifizierung ermöglicht, beides mithilfe vorhandener Active Directory-Funktionen.

## <a name="conclusion"></a>Zusammenfassung

Die Ergänzung des Governance-MVPs um diese Änderungen hilft, viele der in diesem Artikel beschriebenen Risiken abzumildern, wodurch das Cloudeinführungsteam diese Hindernisse schnell hinter sich lassen kann.

## <a name="next-steps"></a>Nächste Schritte

Mit der Weiterentwicklung der Cloudeinführung und der damit verbundenen Steigerung des Geschäftswerts entwickeln sich auch die Risiken und Anforderungen an Cloud Governance. Die folgenden Artikel behandeln einige Weiterentwicklungen, die eintreten können. Für das fiktive Unternehmen in dieser Lösung besteht der nächste Trigger in der Einbeziehung geschützter Daten in den Plan für die Cloudeinführung. Diese Änderung erfordert zusätzliche Sicherheitsmaßnahmen.

> [!div class="nextstepaction"]
> [Entwicklung der Sicherheitsbaseline](./security-baseline-evolution.md)
