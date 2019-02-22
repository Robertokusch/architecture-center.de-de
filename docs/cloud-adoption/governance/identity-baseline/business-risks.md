---
title: 'CAF: Identitätsbaseline: Motivationen und Geschäftsrisiken'
titleSuffix: Microsoft Cloud Adoption Framework for Azure
ms.service: architecture-center
ms.subservice: enterprise-cloud-adoption
ms.custom: governance
ms.date: 02/11/2019
description: 'Identitätsbaseline: Motivationen und Geschäftsrisiken'
author: BrianBlanchard
ms.openlocfilehash: ef831d1b3b01251b27f1f7b18e551c49996a2468
ms.sourcegitcommit: 273e690c0cfabbc3822089c7d8bc743ef41d2b6e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/08/2019
ms.locfileid: "55900725"
---
# <a name="identity-baseline-motivations-and-business-risks"></a>Identitätsbaseline: Motivationen und Geschäftsrisiken

In diesem Artikel werden die Gründe beschrieben, warum Kunden typischerweise eine Disziplin „Identitätsbaseline“ in ihre Cloud Governance-Strategie integrieren. Darüber hinaus werden einige Beispiele für Geschäftsrisiken aufgeführt, die zu Richtlinienanweisungen führen.

<!-- markdownlint-disable MD026 -->

## <a name="is-identity-baseline-relevant"></a>Ist die Identitätsbaseline relevant?

Traditionelle lokale Verzeichnisse sind so konzipiert, dass Unternehmen Berechtigungen und Richtlinien für Benutzer, Gruppen und Rollen in ihren internen Netzwerken und Rechenzentren streng kontrollieren können. Dies ist in der Regel zur Unterstützung von Implementierungen einzelner Mandanten gedacht, wobei die Dienste nur innerhalb der lokalen Umgebung anwendbar sind.

Cloudidentitätsdienste sollen die Authentifizierungs- und Zugriffssteuerungsfunktionen eines Unternehmens auf das Internet erweitern. Sie unterstützen Mehrinstanzenfähigkeit und können zur Verwaltung von Benutzern und Zugriffsrichtlinien über Cloudanwendungen und -bereitstellungen hinweg verwendet werden. Öffentliche Cloudplattformen verfügen über eine bestimmte Form von cloudbasierten Identitätsdiensten, die Verwaltungs- und Bereitstellungsaufgaben unterstützen und in der Lage sind, [unterschiedliche Integrationsebenen](../../decision-guides/identity/overview.md) mit Ihren vorhandenen lokalen Identitätslösungen zu erreichen. Alle diese Features können dazu führen, dass die Cloudidentitätsrichtlinie komplizierter ist, als es Ihre traditionellen lokalen Lösungen erfordern.

Die Bedeutung der Disziplin „Identitätsbaseline“ für Ihre Cloudbereitstellung hängt von der Größe Ihres Teams ab und muss Ihre cloudbasierte Identitätslösung mit einem vorhandenen lokalen Identitätsdienst integrieren. Erste Testbereitstellungen erfordern möglicherweise nicht viel in Bezug auf die Benutzerorganisation oder -verwaltung, aber wenn Ihre Cloudumgebung wächst, müssen Sie wahrscheinlich eine kompliziertere organisatorische Integration und zentralisierte Verwaltung unterstützen.

## <a name="business-risk"></a>Geschäftsrisiken

Mit der Disziplin „Identitätsbaseline“ wird versucht, die grundlegenden Geschäftsrisiken im Zusammenhang mit Identitätsdiensten und Zugriffssteuerung anzugehen. Arbeiten Sie mit Ihrem Unternehmen zusammen, um diese Risiken zu identifizieren und auf Relevanz zu überwachen, um sie bei der Planung und Implementierung Ihrer Cloudbereitstellungen zu berücksichtigen.

Die Risiken werden je nach Organisation unterschiedlich sein, aber die folgenden allgemeinen identitätsbezogenen Risiken können als Ausgangspunkt für Diskussionen innerhalb Ihres Cloud Governance-Teams verwendet werden:

- **Nicht autorisierter Zugriff**. Wenn nicht autorisierte Benutzer auf sensible Daten und Ressourcen zugreifen können, kann dies zu Datenlecks oder Dienstunterbrechungen führen, den Sicherheitsumkreis Ihres Unternehmens verletzen und geschäftliche oder rechtliche Haftungsrisiken mit sich bringen.
- **Ineffizienz aufgrund mehrerer Identitätslösungen**. Organisationen mit mehreren Identitätsdienstmandanten benötigen unter Umständen mehrere Konten für Benutzer. Dies kann zu Ineffizienz für Benutzer, die sich mehrere Anmeldeinformationen merken müssen, und für die IT-Abteilung bei der Verwaltung von Konten über mehrere Systeme hinweg führen. Wenn die Benutzerzugriffszuweisungen nicht in allen Identitätslösungen aktualisiert werden, wenn Mitarbeiter oder Teams wechseln oder sich Unternehmensziele ändern, können Ihre Cloudressourcen für unbefugten Zugriff anfällig sein, oder Benutzer können nicht auf die erforderlichen Ressourcen zugreifen.
- **Unfähigkeit, Ressourcen mit externen Partnern zu teilen**. Schwierigkeiten beim Hinzufügen externer Geschäftspartner zu Ihren bestehenden Identitätslösungen können eine effiziente Ressourcenteilung und Geschäftskommunikation verhindern.
- **Lokale Identitätsabhängigkeiten**. Veralteten Authentifizierungsmechanismen oder Multi-Faktor-Authentifizierung von Drittanbietern sind möglicherweise nicht in der Cloud verfügbar, sodass entweder neue Tools für die Migration von Workloads erstellt oder zusätzliche Identitätsdienste in der Cloud bereitgestellt werden müssen. Beide Anforderungen könnten die Migration verzögern oder verhindern und die Kosten erhöhen.

## <a name="next-steps"></a>Nächste Schritte

Dokumentieren Sie mit der [Cloudverwaltungsvorlage](./template.md) Geschäftsrisiken, die durch den aktuellen Cloudeinführungsplan wahrscheinlich entstehen.

Sobald ein Verständnis für realistische Geschäftsrisiken hergestellt ist, besteht der nächste Schritt darin, die Risikotoleranz des Unternehmens zu dokumentieren sowie die Indikatoren und Schlüsselmetrik zur Überwachung dieser Toleranz zu erfassen.

> [!div class="nextstepaction"]
> [Grundlegendes zu Indikatoren, Metriken und Risikotoleranz](./metrics-tolerance.md)