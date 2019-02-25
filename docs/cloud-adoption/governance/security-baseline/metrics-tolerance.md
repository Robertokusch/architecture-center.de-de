---
title: 'CAF: Metriken, Indikatoren und Risikotoleranz für Sicherheitsbaseline'
titleSuffix: Microsoft Cloud Adoption Framework for Azure
ms.service: architecture-center
ms.subservice: enterprise-cloud-adoption
ms.custom: governance
ms.date: 02/11/2019
description: Metriken, Indikatoren und Risikotoleranz für Sicherheitsbaseline
author: BrianBlanchard
ms.openlocfilehash: 30deafca59b2e09c78432ad3b59d328fb27a1e2c
ms.sourcegitcommit: 273e690c0cfabbc3822089c7d8bc743ef41d2b6e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/08/2019
ms.locfileid: "55901821"
---
# <a name="security-baseline-metrics-indicators-and-risk-tolerance"></a>Metriken, Indikatoren und Risikotoleranz für Sicherheitsbaseline

Dieser Artikel soll Ihnen bei der Quantifizierung der Geschäftsrisikotoleranz im Zusammenhang mit der Sicherheitsbaseline helfen. Das Definieren von Metriken und Indikatoren ermöglicht das Erstellen eines Geschäftsszenarios, mit dem Sie verstärkt auf die Ausgereiftheit der Disziplin „Sicherheitsbaseline“ setzen können.

## <a name="metrics"></a>Metriken

Bei der Sicherheitsbaseline steht im Allgemeinen die Erkennung potenzieller Sicherheitsrisiken in Ihren Cloudbereitstellungen im Mittelpunkt. Im Rahmen der Risikoanalyse sollten Sie Daten zu Ihrer Sicherheitsumgebung sammeln, um zu bestimmen, wie hoch das Risiko ist und wie wichtig Investitionen in die Governance der Sicherheitsbaseline für Ihre geplanten Cloudbereitstellungen ist.

Jede Organisation weist unterschiedliche Sicherheitsumgebungen und -anforderungen sowie verschiedene potenzielle Quellen von Sicherheitsdaten auf. Es folgen Beispiele für nützliche Metriken, die Sie sammeln sollten, um die Risikotoleranz innerhalb der Disziplin „Sicherheitsbaseline“ zu bewerten:

- **Datenklassifizierung**. Anzahl der in der Cloud gespeicherten Daten und Dienste, die gemäß den Standards Ihrer Organisation für Datenschutz, Compliance oder Geschäftsauswirkungen nicht klassifiziert sind.
- **Anzahl der Speicher für sensible Daten**: Anzahl der Speicherendpunkte oder Datenbanken, die sensible Daten enthalten und geschützt werden sollten.
- **Anzahl der Speicher für unverschlüsselte Daten**: Anzahl der Speicher für sensible Daten, die nicht verschlüsselt sind.
- **Angriffsfläche**. Wie viele Datenquellen, Dienste und Anwendungen werden insgesamt in der Cloud gehostet? Welcher Prozentsatz dieser Datenquellen wird als sensibel eingestuft? Welcher Prozentsatz dieser Anwendungen und Dienste ist geschäftskritisch?
- **Abgedeckte Standards**. Anzahl der vom Sicherheitsteam definierten Sicherheitsstandards.
- **Abgedeckte Ressourcen**. Bereitgestellte Ressourcen, die durch Sicherheitsstandards abgedeckt sind.
- **Allgemeine Einhaltung von Standards**. Quote der Einhaltung von Sicherheitsstandards.
- **Angriffe nach Schweregrad**. Wie viele koordinierte Versuche, Ihre in der Cloud gehosteten Dienste zu stören, z.B. durch DDoS-Angriffe (Distributed Denial of Service), treten in Ihrer Infrastruktur auf? Wie sieht es mit Umfang und Schweregrad dieser Angriffe aus?
- **Schutz vor Schadsoftware**. Prozentsatz der bereitgestellten virtuellen Computer (VMs), auf denen alle erforderliche Antischad-, Firewall- oder andere Sicherheitssoftware installiert ist.
- **Patch-Latenzzeit**. Wie lange ist es her, dass Betriebssystem- und Softwarepatches auf VMs angewendet wurden?
- **Empfehlungen für Sicherheitsintegrität**. Anzahl der Empfehlungen von Sicherheitssoftware als Lösung für Integritätsstandards bereitgestellter Ressourcen, nach Schweregrad geordnet.

## <a name="risk-tolerance-indicators"></a>Risikotoleranzindikatoren

Cloudplattformen bieten eine Reihe von Basisfunktionen, die es kleinen Bereitstellungsteams ermöglichen, grundlegende Sicherheitseinstellungen ohne umfangreiche zusätzliche Planung zu konfigurieren. Infolgedessen stellen kleine Dev/Test- oder experimentelle erste Workloads, die keine sensiblen Daten enthalten, ein relativ geringes Risiko dar und werden wahrscheinlich keine hohen Anforderungen in Bezug auf eine formale Richtlinie für die Sicherheitsbaseline bedeuten. Sobald jedoch wichtige Daten oder geschäftskritische Funktionen in die Cloud verschoben werden, steigen die Sicherheitsrisiken, während die Toleranz für diese Risiken schnell abnimmt. Je mehr Ihrer Daten und Funktionen in der Cloud bereitgestellt werden, desto wahrscheinlicher ist es, dass höhere Investitionen in die Disziplin „Sicherheitsbaseline“ erforderlich sind.

In den frühen Phasen der Cloudeinführung arbeiten Sie mit Ihrem IT-Sicherheitsteam und den Projektbeteiligten des Unternehmens zusammen, um [Geschäftsrisiken](business-risks.md) im Zusammenhang mit der Sicherheit zu identifizieren, und bestimmen anschließend eine akzeptable Baseline für die Sicherheitsrisikotoleranz. Dieser Abschnitt des CAF enthält einige Beispiele. Die Risiken und Baselines in Ihrem Unternehmen bzw. bei Ihren Bereitstellungen weichen im Detail möglicherweise davon ab.

Legen Sie nach der Einigung auf eine Baseline minimale Benchmarks fest, die eine unzulässige Zunahme der identifizierten Risiken darstellen. Diese Benchmarks fungieren als Auslöser und geben an, wann Sie Maßnahmen zum Beheben dieser Risiken ergreifen müssen. Im Folgenden wird anhand einiger Beispiele dargestellt, wie Sicherheitsmetriken (z.B. die oben erwähnten) eine höhere Investition in die Disziplin „Sicherheitsbaseline“ rechtfertigen können.

- **Auslöser für geschäftskritische Workloads**. Ein Unternehmen, das geschäftskritische Workloads in der Cloud bereitstellt, sollte in die Disziplin „Sicherheitsbaseline“ investieren, um mögliche Unterbrechungen des Diensts oder die Offenlegung sensibler Daten zu verhindern.
- **Auslöser für geschützte Daten**. Ein Unternehmen, das Daten in der Cloud hostet, die als vertraulich, privat oder anderweitig rechtlich bedenklich eingestuft werden können. Es ist eine Disziplin „Sicherheitsbaseline“ erforderlich, um sicherzustellen, dass diese Daten nicht verloren gehen, offengelegt oder gestohlen werden.
- **Auslöser für externe Angriffe**. Ein Unternehmen, bei dem X-mal im Monat schwere Angriffe auf die Netzwerkinfrastruktur stattfinden, kann von der Disziplin „Sicherheitsbaseline“ profitieren.  
- **Auslöser für Einhaltung von Standards**. Ein Unternehmen, bei dem mehr als X % seiner Ressourcen nicht den Sicherheitsstandards entsprechen, sollte in die Disziplin „Sicherheitsbaseline“ investieren, um sicherzustellen, dass Standards in der gesamten IT-Infrastruktur konsistent angewendet werden.
- **Auslöser für Größe der Cloudumgebung**. Ein Unternehmen, das mehr als X Anwendungen, Dienste oder Datenquellen hostet. Große Cloudbereitstellungen können von Investitionen in die Disziplin „Sicherheitsbaseline“ profitieren, um sicherzustellen, dass ihre gesamte Angriffsfläche angemessen vor unbefugtem Zugriff oder anderen externen Bedrohungen geschützt ist.
- **Auslöser für Konformität der Sicherheitssoftware**. Ein Unternehmen, in dem auf weniger als X % der bereitgestellten virtuellen Computer die gesamte erforderliche Sicherheitssoftware installiert ist. Mithilfe der Disziplin „Sicherheitsbaseline“ kann sichergestellt werden, dass Software konsistent auf allen Computern installiert ist.
- **Auslöser für Patching**. Ein Unternehmen, in dem auf bereitgestellten virtuellen Computern oder Diensten in den letzten X Tagen keine Betriebssystem- oder Softwarepatches angewendet wurden. Mithilfe der Disziplin „Sicherheitsbaseline“ kann sichergestellt werden, dass Patches innerhalb eines erforderlichen Zeitplans auf dem neuesten Stand gehalten werden.
- **Sicherheitsorientiert**. Einige Unternehmen haben strenge Anforderungen in Bezug auf Sicherheit und Vertraulichkeit von Daten, selbst für Test- und experimentelle Workloads. Diese Unternehmen müssen in die Disziplin „Sicherheitsbaseline“ investieren, bevor mit Bereitstellungen begonnen werden kann.

Die genauen Metriken und Auslöser, die Sie zum Bemessen der Risikotoleranz verwenden, und die Höhe der Investitionen in die Disziplin „Sicherheitsbaseline“ sind für Ihre Organisation spezifisch, doch sollten die oben genannten Beispiele eine hilfreiche Diskussionsgrundlage für Ihr Cloud Governance-Team darstellen.  

## <a name="next-steps"></a>Nächste Schritte

Dokumentieren Sie mit der Vorlage [Cloudverwaltung](./template.md) die Metriken und Toleranzindikatoren, die sich an dem aktuellen Cloudeinführungsplan orientieren.

Erstellen Sie anhand der Risiken und Toleranzen einen Prozess, mit dem Sie die Einhaltung der Richtlinie für die Sicherheitsbaseline steuern und kommunizieren.

> [!div class="nextstepaction"]
> [Festlegen von Prozessen für Richtlinienkonformität](compliance-processes.md)
