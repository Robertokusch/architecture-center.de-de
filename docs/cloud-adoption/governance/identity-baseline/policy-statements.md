---
title: 'CAF: Beispiele für Richtlinienanweisungen der Identitätsbaseline'
titleSuffix: Microsoft Cloud Adoption Framework for Azure
ms.service: architecture-center
ms.subservice: enterprise-cloud-adoption
ms.custom: governance
ms.date: 02/11/2019
description: Beispiele für Richtlinienanweisungen der Identitätsbaseline
author: BrianBlanchard
ms.openlocfilehash: 5a106dc73cfc1fc97084853507372610b3f1c69c
ms.sourcegitcommit: c053e6edb429299a0ad9b327888d596c48859d4a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/20/2019
ms.locfileid: "58246481"
---
# <a name="identity-baseline-sample-policy-statements"></a>Beispiele für Richtlinienanweisungen der Identitätsbaseline

Einzelne Cloudrichtlinienanweisungen sind Anleitungen für den Umgang mit bestimmten Risiken, die während Ihres Risikobewertungsprozesses identifiziert wurden. Diese Anweisungen sollten eine präzise Zusammenfassung der Risiken sowie der Pläne für den Umgang mit diesen bereitstellen. Jede Anweisungsdefinition sollte diese Informationen enthalten:

- Technisches Risiko: eine Zusammenfassung des Risikos, das diese Richtlinie behandelt.
- Richtlinienanweisung: eine klare, zusammenfassende Erläuterung der Anforderungen der Richtlinie.
- Entwurfsoptionen: direkt umsetzbare Empfehlungen, Spezifikationen oder andere Anleitungen, die IT-Teams und Entwickler bei der Implementierung der Richtlinie verwenden können.

Die folgenden Beispielrichtlinienanweisungen behandeln eine Reihe von allgemeinen identitätsbezogenen Geschäftsrisiken und sollen Ihnen als Beispiele dienen, auf die Sie sich beim Entwerfen von Richtlinienanweisungen beziehen können, um die Anforderungen Ihrer eigenen Organisation zu erfüllen. Diese Beispiele sind nicht restriktiv gemeint, und es gibt immer potenziell mehrere Richtlinienoptionen für den Umgang mit einem spezifischen Risiko. Arbeiten Sie eng mit den Geschäfts- und IT-Teams zusammen, um die besten Richtlinienlösungen für Ihre spezielle Risikogruppe zu identifizieren.

## <a name="lack-of-access-controls"></a>Fehlende Zugriffssteuerung

**Technisches Risiko:** Unzureichende oder Ad-hoc-Zugriffssteuerungseinstellungen können das Risiko eines nicht autorisierten Zugriffs auf sensible oder unternehmenskritische Ressourcen bergen.

**Richtlinienanweisung:** Alle in der Cloud bereitgestellten Ressourcen sollten mit Identitäten und Rollen kontrolliert werden, die von aktuellen Governancerichtlinien genehmigt werden.

**Potenzielle Entwurfsoptionen:** Der [bedingte Azure Active Directory-Zugriff](/azure/active-directory/conditional-access/overview) ist der Standardmechanismus für die Zugriffssteuerung in Azure.

## <a name="overprovisioned-access"></a>Überdimensionierter Zugriff

**Technisches Risiko:** Wenn Benutzer und Gruppen die Kontrolle über Ressourcen außerhalb ihres Zuständigkeitsbereichs haben, kann dies zu nicht autorisierten Änderungen mit daraus resultierenden Ausfällen oder Sicherheitsrisiken führen.

**Richtlinienanweisung:** Die folgenden Richtlinien werden implementiert:

- Das Zugriffsmodell der geringsten Rechte wird auf alle Ressourcen angewandt, die für unternehmenskritische Anwendungen oder geschützte Daten verwendet werden.
- Erhöhte Berechtigungen müssen eine Ausnahme bleiben, und solche Ausnahmen müssen vom Cloudgovernanceteam erfasst werden. Ausnahmen werden regelmäßig überwacht.

**Potenzielle Entwurfsoptionen:** Machen Sie sich mit den [bewährten Methoden für die Azure-Identitätsverwaltung](/azure/security/azure-security-identity-management-best-practices) vertraut, um eine rollenbasierte Zugriffssteuerung (RBAC) zu implementieren, über die der Zugriff basierend auf den Sicherheitsprinzipien [Need-to-Know](https://wikipedia.org/wiki/Need_to_know) und [geringste Rechte](https://wikipedia.org/wiki/Principle_of_least_privilege) beschränkt wird.

## <a name="lack-of-shared-management-accounts-between-on-premises-and-the-cloud"></a>Fehlende freigegebene Verwaltungskonten zwischen dem lokalen System und der Cloud

**Technisches Risiko:** Mitarbeiter in der IT-Verwaltung oder Administration mit Konten in Ihrer lokalen Active Directory-Instanz haben möglicherweise keinen ausreichenden Zugriff auf Cloudressourcen und können so unter Umständen Betriebs- oder Sicherheitsprobleme nicht effizient beheben.

**Richtlinienanweisung:** Alle Gruppen in der lokalen Active Directory-Infrastruktur mit erhöhten Rechten sollten einer genehmigten RBAC-Rolle zugeordnet werden.

**Potenzielle Entwurfsoptionen:** Implementieren Sie eine Hybrididentitätslösung zwischen der cloudbasierten Azure Active Directory-Instanz und der lokalen Azure Active Directory-Instanz, und fügen Sie die entsprechenden lokalen Gruppen den für ihre Aufgaben erforderlichen RBAC-Rollen zu.

## <a name="weak-authentication-mechanisms"></a>Schwache Authentifizierungsmechanismen

**Technisches Risiko:** Identitätsverwaltungssysteme mit unzureichend sicheren Methoden der Benutzerauthentifizierung, z.B. einfache Kombinationen von Benutzer und Kennwort, können zu kompromittierten oder gehackten Kennwörtern führen und ein erhebliches Risiko eines nicht autorisierten Zugriffs auf sichere Cloudsysteme bergen.

**Richtlinienanweisung:** Für alle Konten muss die Anmeldung bei gesicherten Ressourcen über eine Methode der mehrstufigen Authentifizierung (MFA) erfolgen.

**Potenzielle Entwurfsoptionen:** Implementieren Sie für Azure Active Directory [Azure Multi-Factor Authentication](/azure/active-directory/authentication/concept-mfa-howitworks) als Teil des Prozesses zur Benutzerautorisierung.

## <a name="isolated-identity-providers"></a>Isolierte Identitätsanbieter

**Technisches Risiko:** Inkompatible Identitätsanbieter können dazu führen, dass Ressourcen oder Dienste nicht für Kunden oder andere Geschäftspartner freigegeben werden können.

**Richtlinienanweisung:** Die Bereitstellung von Anwendungen, für die eine Kundenauthentifizierung erforderlich ist, erfordert einen genehmigten Identitätsanbieter, der mit dem primären Identitätsanbieter für interne Benutzer kompatibel ist.

**Potenzielle Entwurfsoptionen:** Implementieren Sie einen [Verbund mit Azure Active Directory](/azure/active-directory/hybrid/whatis-fed) zwischen Ihren internen Identitätsanbietern und den Identitätsanbietern von Kunden.

## <a name="identity-reviews"></a>Identitätsüberprüfungen

**Technisches Risiko:** Im Laufe der Zeit können Geschäftsveränderungen, das Hinzufügen neuer Cloudbereitstellungen oder andere Sicherheitsaspekte die Risiken von autorisiertem Zugriff auf gesicherte Ressourcen erhöhen.

**Richtlinienanweisung:** Cloudgovernanceprozesse müssen vierteljährliche Überprüfungen durch Identitätsverwaltungsteams umfassen, um böswillige Akteure oder Nutzungsmuster zu identifizieren, die durch die Cloudressourcenkonfiguration verhindert werden sollten.

**Potenzielle Entwurfsoptionen:** Beraumen Sie ein vierteljährlich Meeting zur Sicherheitsüberprüfung an, an dem Governanceteammitglieder und IT-Mitarbeiter teilnehmen, die für die Verwaltung von Identitätsdiensten verantwortlich sind. Überprüfen Sie vorhandene Sicherheitsdaten und Metriken, um Lücken in der aktuellen Richtlinie und den Tools zur Identitätsverwaltung zu ermitteln, und aktualisieren Sie die Richtlinie, um alle neuen Risiken zu verringern.

## <a name="next-steps"></a>Nächste Schritte

Verwenden Sie die in diesem Artikel erwähnten Beispiele als Ausgangspunkt für die Entwicklung von Richtlinien, die bestimmte Geschäftsrisiken behandeln, die Ihren Plänen für die Einführung der Cloud entsprechen.

Um mit der Entwicklung Ihrer eigenen, benutzerdefinierten Richtlinienanweisungen mit Bezug auf die Identitätsbaseline zu beginnen, laden Sie die [Vorlage für die Identitätsbaseline](template.md) herunter.

Um die Einführung dieser Disziplin zu beschleunigen, wählen Sie die [nützliche Governance-Vorgehensweise](../journeys/overview.md) aus, die am besten zu Ihrer Umgebung passt. Ändern Sie dann den Entwurf, um Ihre speziellen Entscheidungen für Unternehmensrichtlinien zu integrieren.

> [!div class="nextstepaction"]
> [Nützliche Governancevorgehensweisen](../journeys/overview.md)