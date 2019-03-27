---
title: 'Framework für die Cloudeinführung (CAF): Metriken, Indikatoren und Risikotoleranz in der Ressourcenkonsistenz'
titleSuffix: Microsoft Cloud Adoption Framework for Azure
ms.service: architecture-center
ms.subservice: enterprise-cloud-adoption
ms.custom: governance
ms.date: 02/11/2019
description: Metriken, Indikatoren und Risikotoleranz in der Ressourcenkonsistenz
author: BrianBlanchard
ms.openlocfilehash: 3a2561a6d1d81a6395bb256e921a7a26898b45a0
ms.sourcegitcommit: c053e6edb429299a0ad9b327888d596c48859d4a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/20/2019
ms.locfileid: "58241991"
---
# <a name="resource-consistency-metrics-indicators-and-risk-tolerance"></a>Metriken, Indikatoren und Risikotoleranz in der Ressourcenkonsistenz

Dieser Artikel soll Ihnen bei der Quantifizierung der Geschäftsrisikotoleranz im Zusammenhang mit der Ressourcenkonsistenz helfen. Das Definieren von Metriken und Indikatoren ermöglicht das Erstellen eines Geschäftsszenarios, mit dem Sie in die Weiterentwicklung der Disziplin „Ressourcenkonsistenz“ investieren können.

## <a name="metrics"></a>Metriken

Die Disziplin „Ressourcenkonsistenz“ konzentriert sich auf den Umgang mit Risiken im Zusammenhang mit der Betriebsverwaltung Ihrer Cloudbereitstellungen. Im Rahmen der Risikoanalyse sollten Sie Daten zu Ihren IT-Abläufen sammeln, um zu ermitteln, welches Risiko vorliegt und wie wichtig Investitionen in die Governance der Ressourcenkonsistenz für Ihre geplanten Cloudbereitstellungen ist.

Jede Organisation verfügt über die verschiedenen Betriebsszenarien, aber die folgenden Elemente stellen nützliche Beispiele für die Metriken dar, die Sie sammeln sollten, wenn Sie die Risikotoleranz innerhalb der Disziplin „Ressourcenkonsistenz“ beurteilen:

- **Cloudressourcen**. Die Gesamtanzahl der in der Cloud bereitgestellten Ressourcen.
- **Unmarkierte Ressourcen**. Die Anzahl der Ressourcen ohne erforderliche Buchhaltung, Auswirkungen auf das Unternehmen oder Organisationstags.
- **Nicht ausgelastete Ressourcen**. Die Anzahl der Ressourcen, bei denen die Arbeitsspeicher-, CPU- oder Netzwerkfunktionen kontinuierlich nicht ausgelastet sind.
- **Ressourcenschwund**. Die Anzahl der Ressourcen, bei denen die Arbeitsspeicher-, CPU- oder Netzwerkfunktionen durch Lasten erschöpft sind.
- **Ressourcenalter**. Die Zeit, seit die Ressource zuletzt bereitgestellt oder geändert wurde.
- **Dienstverfügbarkeit**. Der Prozentsatz der tatsächlichen Verfügbarkeit der in der Cloud gehosteten Workloads im Vergleich zur erwarteten Verfügbarkeit.
- **Virtuelle Computer in kritischem Zustand**. Die Anzahl der bereitgestellten virtuellen Computer, auf denen mindestens ein kritischer Fehler entdeckt wurde, der behandelt werden muss, um die normale Funktionsweise wiederherzustellen.
- **Warnungen nach Schweregrad**. Die Gesamtanzahl von Warnungen für eine bereitgestellte Ressource, aufgeschlüsselt nach Schweregrad.
- **Fehlerhafte Subnetzlinks**. Die Anzahl der Ressourcen mit Netzwerkkonnektivitätsproblemen.
- **Fehlerhafte Dienstendpunkte**. Die Anzahl von Problemen mit externen Netzwerkendpunkten.
- **Cloudanbieter-Dienstintegritätsvorfälle**. Die Anzahl von Unterbrechungen oder Leistungsvorfällen, die vom Cloudanbieter verursacht wurden.
- **Sicherungsintegrität**. Die Anzahl von Sicherungen, die aktiv synchronisiert werden.
- **Wiederherstellungsintegrität**. Die Anzahl von Wiederherstellungsvorgängen, die erfolgreich ausgeführt wurden.

## <a name="risk-tolerance-indicators"></a>Risikotoleranzindikatoren

Cloudplattformen bieten eine Reihe von Basisfunktionen, die es Bereitstellungsteams ermöglichen, kleine Bereitstellungen effektiv zu verwalten, ohne umfangreiche zusätzliche Planung oder Prozesse. Infolgedessen stellen kleine Dev/Test- oder experimentelle erste Workloads, die eine relativ geringe Anzahl cloudbasierter Ressourcen umfassen, ein geringes Risiko dar und werden wahrscheinlich keine hohen Anforderungen in Bezug auf eine formale Richtlinie für die Ressourcenkonsistenz stellen.

Mit zunehmender Größe Ihrer Cloudumgebung wird jedoch auch die Komplexität der Verwaltung Ihrer Ressourcen deutlich schwieriger werden. Mit steigender Anzahl von Ressourcen in der Cloud erhält die Fähigkeit zur Identifizierung des Besitzes von Ressourcen und die nützliche Kontrolle von Ressourcen eine kritische Bedeutung für die Minimierung von Risiken. Wenn zunehmend unternehmenskritische Workloads in der Cloud bereitgestellt werden, wird die Verfügbarkeitsdauer des Diensts kritischer, und die Toleranz für potenzielle Kostenüberschreitungen durch Dienstunterbrechungen nimmt schnell ab.

In den frühen Phasen der Cloudeinführung arbeiten Sie mit Ihrem IT-Betriebsteam und den Projektbeteiligten des Unternehmens zusammen, um [Geschäftsrisiken](business-risks.md) im Zusammenhang mit der Ressourcenkonsistenz zu identifizieren, und bestimmen anschließend eine akzeptable Baseline für die Risikotoleranz. Dieser Abschnitt des CAF enthält einige Beispiele. Die Risiken und Baselines in Ihrem Unternehmen bzw. bei Ihren Bereitstellungen weichen im Detail möglicherweise davon ab.

Legen Sie nach der Einigung auf eine Baseline minimale Benchmarks fest, die eine unzulässige Zunahme der identifizierten Risiken darstellen. Diese Benchmarks fungieren als Auslöser und geben an, wann Sie Maßnahmen zum Beheben dieser Risiken ergreifen müssen. Im Folgenden wird anhand einiger Beispiele dargestellt, wie Betriebsmetriken (z.B. die oben erwähnten) eine höhere Investition in die Disziplin „Ressourcenkonsistenz“ rechtfertigen können.

- **Trigger „Markieren (Tagging) und Benennen“**. Ein Unternehmen mit mehr als X Ressourcen, denen erforderliche Markierungsinformationen fehlen die Benennungsstandards nicht einhalten, sollten Investitionen in die Disziplin „Ressourcenkonsistenz“ in Erwägung ziehen, um die Optimierung dieser Standards zu unterstützen und ihre konsistente Anwendung auf in der Cloud bereitgestellte Ressourcen sicherzustellen.
- **Trigger „Überdimensionierte Ressourcen“**. Wenn in einem Unternehmen mehr als X % der Ressourcen regelmäßig nur sehr geringe Mengen ihrer verfügbaren Arbeitsspeicher-, CPU- oder Netzwerkfunktionen nutzen, wird vorgeschlagen, in die Disziplin „Ressourcenkonsistenz“ zu investieren, um die Optimierung der Ressourcennutzung für diese Elemente zu optimieren.
- **Trigger „Unterdimensionierte Ressourcen“**. Wenn in einem Unternehmen mehr als X % der Ressourcen regelmäßig den größten Teil ihrer verfügbaren Arbeitsspeicher-, CPU- oder Netzwerkfunktionen erschöpfen, wird vorgeschlagen, in die Disziplin „Ressourcenkonsistenz“ zu investieren, um sicherzustellen, dass diese Objekte die notwendigen Ressourcen erhalten, die zur Verhinderung von Dienstunterbrechungen erforderlich sind.
- **Trigger „Ressourcenalter“**. Ein Unternehmen mit mehr als X Ressourcen, die in mehr als X Monaten nicht aktualisiert wurden, könnte von Investitionen in die Disziplin „Ressourcenkonsistenz“ profitieren, die darauf abzielt sicherzustellen, dass Ressourcen gepatcht und fehlerfrei sind, während gleichzeitig veraltete oder anderweitig nicht verwendete Ressourcen außer Betrieb genommen werden.  
- **Trigger „Dienstverfügbarkeit“**. Ein Unternehmen, in dem es bei unternehmenskritischen Diensten zu einer Verfügbarkeit von weniger als X % kommt, sollte in die Disziplin „Ressourcenkonsistenz“ investieren, um die Dienstzuverlässigkeit zu verbessern.
- **Trigger „VM-Integrität“**. Ein Unternehmen, in dem bei mehr als X % der VMs kritische Integritätsprobleme auftreten, sollte in die Disziplin „Ressourcenkonsistenz“ investieren, um Probleme zu identifizieren und die VM-Stabilität zu verbessern.
- **Trigger „Netzwerkintegrität“**. Ein Unternehmen, in dem bei mehr als X % der Netzwerksubnetze oder -endpunkte Konnektivitätsprobleme auftreten, sollte in die Disziplin „Ressourcenkonsistenz“ investieren, um Netzwerkprobleme zu identifizieren und zu beheben.
- **Trigger „Sicherungsabdeckung“**. Ein Unternehmen mit X % unternehmenskritischer Ressourcen ohne vorhandene aktuelle Sicherungen würde von erhöhten Investitionen in die Disziplin „Ressourcenkonsistenz“ profitieren, um eine konsistente Sicherungsstrategie sicherzustellen.
- **Trigger „Sicherungsintegrität“**. Ein Unternehmen, in dem mehr als X % der Wiederherstellungsvorgänge fehlschlagen, sollte in die Disziplin „Ressourcenkonsistenz“ investieren, um Probleme mit der Sicherung zu identifizieren und sicherzustellen, dass wichtige Ressourcen geschützt sind.

Die genauen Metriken und Auslöser, die Sie zum Bemessen der Risikotoleranz verwenden, und die Höhe der Investitionen in die Disziplin „Ressourcenkonsistenz“ sind für Ihre Organisation spezifisch. Die oben genannten Beispiele sollten jedoch eine hilfreiche Diskussionsgrundlage für Ihr Cloud Governance-Team darstellen.  

## <a name="next-steps"></a>Nächste Schritte

Dokumentieren Sie mit der Vorlage [Cloudverwaltung](./template.md) die Metriken und Toleranzindikatoren, die sich an dem aktuellen Cloudeinführungsplan orientieren.

Erstellen Sie anhand der Risiken und Toleranzen einen Prozess, mit dem Sie die Einhaltung der Richtlinie für die Ressourcenkonsistenz steuern und kommunizieren.

> [!div class="nextstepaction"]
> [Festlegen von Prozessen für Richtlinienkonformität](compliance-processes.md)
