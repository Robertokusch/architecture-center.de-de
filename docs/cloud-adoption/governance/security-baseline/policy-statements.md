---
title: 'CAF: Beispiele für Richtlinienanweisungen der Sicherheitsbaseline'
titleSuffix: Microsoft Cloud Adoption Framework for Azure
ms.service: architecture-center
ms.subservice: enterprise-cloud-adoption
ms.custom: governance
ms.date: 02/11/2019
description: Beispiele für Richtlinienanweisungen der Sicherheitsbaseline
author: BrianBlanchard
ms.openlocfilehash: 00a1ac274b3ab095ac1b915852f6c950ed19eeed
ms.sourcegitcommit: c053e6edb429299a0ad9b327888d596c48859d4a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/20/2019
ms.locfileid: "58247291"
---
# <a name="security-baseline-sample-policy-statements"></a>Beispiele für Richtlinienanweisungen der Sicherheitsbaseline

Einzelne Cloudrichtlinienanweisungen sind Anleitungen für den Umgang mit bestimmten Risiken, die während Ihres Risikobewertungsprozesses identifiziert wurden. Diese Anweisungen sollten eine präzise Zusammenfassung der Risiken sowie der Pläne für den Umgang mit diesen bereitstellen. Jede Anweisungsdefinition sollte diese Informationen enthalten:

- Technisches Risiko: eine Zusammenfassung des Risikos, das diese Richtlinie behandelt.
- Richtlinienanweisung: Eine klare, zusammenfassende Erläuterung der Anforderungen der Richtlinie.
- Technische Optionen: Direkt umsetzbare Empfehlungen, Spezifikationen oder anderen Anleitungen, die IT-Teams und Entwickler bei der Implementierung der Richtlinie verwenden können.

Die folgenden Beispielrichtlinienanweisungen behandeln eine Reihe von allgemeinen sicherheitsbezogenen Geschäftsrisiken und sollen Ihnen als Beispiele dienen, auf die Sie sich beim Entwerfen von eigentlichen Richtlinienanweisungen beziehen können, um die Anforderungen Ihrer eigenen Organisation zu erfüllen. Diese Beispiele sind nicht restriktiv gemeint, und es gibt immer potenziell mehrere Richtlinienoptionen für den Umgang mit einem einzelnen identifizierten Risiko. Arbeiten Sie eng mit den Geschäfts-, Sicherheits- und IT-Teams zusammen, um die besten Richtlinienlösungen für Ihre besonderen Sicherheitsrisiken zu identifizieren.  

## <a name="asset-classification"></a>Ressourcenklassifizierung

**Technisches Risiko**: Ressourcen, die nicht richtig als unternehmenskritisch oder im Zusammenhang mit sensiblen Daten stehend identifiziert werden, erhalten möglicherweise keinen ausreichenden Schutz, was zu potenziellen Datenverlusten oder Geschäftsunterbrechungen führt.

**Richtlinienanweisung**: Alle bereitgestellten Ressourcen müssen nach Wichtigkeit und Datenklassifizierung kategorisiert werden. Vor der Bereitstellung in der Cloud müssen die Klassifizierungen durch das Cloudgovernanceteam und die Besitzer der Anwendung überprüft werden.

**Potenzielle Entwurfsoption**: Legen Sie [Standards für Ressourcentags](../../decision-guides/resource-tagging/overview.md) fest, und stellen Sie sicher, dass IT-Mitarbeiter sie konsistent mithilfe von [Azure-Ressourcentags](/azure/azure-resource-manager/resource-group-using-tags) auf alle bereitgestellten Ressourcen anwenden.

## <a name="data-encryption"></a>Datenverschlüsselung

**Technisches Risiko**: Es besteht das Risiko, dass geschützte Daten während der Speicherung ungeschützt sind.

**Richtlinienanweisung**: Alle geschützten Daten müssen im Ruhezustand verschlüsselt sein.

**Potenzielle Entwurfsoption**: Im Artikel [Übersicht über die Azure-Verschlüsselung](/azure/security/security-azure-encryption-overview) finden Sie Informationen darüber, wie die Verschlüsselung für ruhende Daten auf der Azure-Plattform ausgeführt wird.  

## <a name="network-isolation"></a>Netzwerkisolation

**Technisches Risiko**: Konnektivität zwischen Netzwerken und Subnetzen innerhalb von Netzwerken weist potenzielle Sicherheitslücken auf, die zu Datenverlusten oder einer Unterbrechung der unternehmenskritischen Dienste führen können.

**Richtlinienanweisung**: Netzwerksubnetze mit geschützten Daten müssen von allen anderen Subnetzen isoliert werden. Der Netzwerkdatenverkehr zwischen Subnetzen mit geschützten Daten muss regelmäßig überwacht werden.

**Potenzielle Entwurfsoption**: In Azure wird die Isolation von Netzwerk und Subnetz über [Azure Virtual Networks](/azure/virtual-network/virtual-networks-overview) verwaltet.

## <a name="secure-external-access"></a>Sicherer externer Zugriff

**Technisches Risiko**: Den Zugriff auf Workloads über das öffentliche Internet zuzulassen bringt das Risiko des unbefugten Eindringens mit sich, was nicht autorisierte Offenlegung von Daten oder Geschäftsunterbrechung zur Folge hat.

**Richtlinienanweisung**: Kein Subnetz mit geschützten Daten ist direkt über das öffentliche Internet oder rechenzentrumsübergreifend zugänglich. Der Zugriff auf diese Subnetze muss über zwischengeschaltete Subnetze geroutet werden. Der gesamte Zugriff auf diese Subnetze muss über eine Firewalllösung erfolgen, die Funktionen zur Paketüberprüfung und Sperrfunktionen durchführen kann.

**Potenzielle Entwurfsoption**: Sichern Sie in Azure öffentliche Endpunkte durch Bereitstellen einer [DMZ zwischen dem öffentlichen Internet und Ihrem cloudbasierten Netzwerk](/azure/architecture/reference-architectures/dmz/secure-vnet-dmz).

## <a name="ddos-protection"></a>DDoS-Schutz

**Technisches Risiko**: Verteilte DoS-Angriffe (Distributed denial of service, DDoS) können zu einer Unterbrechung des Geschäftsbetriebs führen.

**Richtlinienanweisung**: Stellen Sie automatisierte DDoS-Entschärfungsmechanismen für alle öffentlich zugänglichen Netzwerkendpunkte bereit.

**Potenzielle Entwurfsoption**: Minimieren Sie durch DDoS-Angriffe verursachte Unterbrechungen mit [Azure DDoS Protection](/azure/virtual-network/ddos-protection-overview).

## <a name="secure-on-premises-connectivity"></a>Absichern der lokalen Konnektivität

**Technisches Risiko**: Unverschlüsselter Datenverkehr zwischen Ihrem Cloudnetzwerk und dem lokalen über das öffentliche Internet ist anfällig dafür, abgefangen zu werden – mit dem Risiko der Offenlegung von Daten.

**Richtlinienanweisung**: Alle Verbindungen zwischen lokalen und Cloudnetzwerken müssen entweder über eine sichere, verschlüsselte VPN-Verbindung oder eine dedizierte private WAN-Verbindung erfolgen.

**Potenzielle Entwurfsoption**: Verwenden Sie in Azure ExpressRoute oder Azure VPN, um private Verbindungen zwischen Ihren lokalen und Cloudnetzwerken herzustellen.

## <a name="network-monitoring-and-enforcement"></a>Netzwerküberwachung und Durchsetzung

**Technisches Risiko**: Änderungen an der Netzwerkkonfiguration können zu neuen Sicherheits- und Datenoffenlegungs-Risiken führen.

**Richtlinienanweisung**: Die Anforderungen an die Netzwerkkonfiguration, die vom Sicherheitsbaselineteam definiert wurden, müssen mit Governancetools überwacht und durchgesetzt werden.

**Potenzielle Entwurfsoption**: In Azure kann die Netzwerkaktivität mit [Azure Network Watcher](/azure/network-watcher/network-watcher-monitoring-overview) überwacht werden, und [Azure Security Center](/azure/security-center/security-center-network-recommendations) kann Sicherheitsrisiken erkennen. Mit Azure Policy können Sie die Netzwerkressourcen- und Ressourcenkonfigurationsrichtlinie gemäß den vom Sicherheitsteam definierten Limits einschränken.

## <a name="security-review"></a>Sicherheitsüberprüfung

**Technisches Risiko**: Im Laufe der Zeit treten neue Sicherheitsrisiken und Angriffstypen auf und erhöhen das Risiko der Offenlegung oder Unterbrechung Ihrer Cloudressourcen.

**Richtlinienanweisung**: Trends und potenzielle Exploits, die mögliche Auswirkungen auf Cloudbereitstellungen haben, müssen vom Sicherheitsteam regelmäßig überprüft werden, damit Updates für in der Cloud verwendete Sicherheitsbaselinetools bereitgestellt werden.

**Potenzielle Entwurfsoption**: Legen Sie ein regelmäßiges Meeting zur Sicherheitsüberprüfung fest, an dem relevante IT- und Governanceteammitglieder teilnehmen. Überprüfen Sie vorhandene Sicherheitsdaten und Metriken, um Lücken in den aktuellen Richtlinien- und Sicherheitsbaselinetools zu ermitteln, und aktualisieren Sie die Richtlinie, um alle neuen Risiken zu verringern.

## <a name="next-steps"></a>Nächste Schritte

Verwenden Sie die in diesem Artikel erwähnten Beispiele als Ausgangspunkt für die Entwicklung von Richtlinien, die bestimmte Sicherheitsrisiken behandeln, die Ihren Plänen für die Einführung der Cloud entsprechen.

Um mit der Entwicklung Ihrer eigenen, benutzerdefinierten Richtlinienanweisungen mit Bezug auf die Sicherheitsbaseline zu beginnen, laden Sie die [Sicherheitsbaselinevorlage](template.md) herunter.

Um die Einführung dieser Disziplin zu beschleunigen, wählen Sie die [nützliche Governance-Vorgehensweise](../journeys/overview.md) aus, die am besten zu Ihrer Umgebung passt. Ändern Sie dann den Entwurf, um Ihre speziellen Entscheidungen für Unternehmensrichtlinien zu integrieren.

> [!div class="nextstepaction"]
> [Nützliche Governancevorgehensweisen](../journeys/overview.md)
