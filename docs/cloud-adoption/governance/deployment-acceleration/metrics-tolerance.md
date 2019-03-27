---
title: 'CAF: Beschleunigung der Bereitstellung – Metriken, Indikatoren und Risikotoleranz'
titleSuffix: Microsoft Cloud Adoption Framework for Azure
ms.service: architecture-center
ms.subservice: enterprise-cloud-adoption
ms.custom: governance
ms.date: 02/11/2019
description: Beschleunigung der Bereitstellung – Metriken, Indikatoren und Risikotoleranz
author: alexbuckgit
ms.openlocfilehash: 87d6f9f67b98d5761aced560392c9d38fa682ee7
ms.sourcegitcommit: c053e6edb429299a0ad9b327888d596c48859d4a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/20/2019
ms.locfileid: "58241211"
---
# <a name="deployment-acceleration-metrics-indicators-and-risk-tolerance"></a>Beschleunigung der Bereitstellung – Metriken, Indikatoren und Risikotoleranz

Dieser Artikel soll Ihnen bei der Quantifizierung der Geschäftsrisikotoleranz im Zusammenhang mit der Beschleunigung der Bereitstellung helfen. Das Definieren von Metriken und Indikatoren ermöglicht das Erstellen eines Geschäftsszenarios, mit dem Sie verstärkt auf die Ausgereiftheit der Disziplin „Beschleunigung der Bereitstellung“ setzen können.

## <a name="metrics"></a>Metriken

Die Disziplin „Beschleunigung der Bereitstellung“ konzentriert sich auf die Bereitstellung, Aktualisierung und Wartung von den für einen ordnungsgemäßen Systembetrieb konfigurierten Cloudressourcen. Die folgenden Informationen sind bei der Einführung dieser Cloud Governance-Disziplin hilfreich:

- **Recovery Time Objective (RTO)**. Gibt den maximal zulässigen Zeitraum an, den eine Anwendung nach einem Vorfall nicht verfügbar sein darf.
- **Recovery Point Objective (RPO)**. Gibt den maximalen Zeitraum für den Datenverlust an, der bei einem Notfall akzeptabel ist. Wenn Sie Daten beispielsweise in einer einzelnen Datenbank ohne Replikation in anderen Datenbanken speichern und stündliche Sicherungen durchführen, können Daten für bis zu eine Stunde verloren gehen.
- **Mean Time To Recover (MTTR)**. Gibt die durchschnittliche Zeit an, die zum Wiederherstellen einer Komponente nach einem Ausfall erforderlich ist.
- **Mean Time Between Failures (MTBF)**. Gibt die mittlere Betriebsdauer für die Komponente an, die zwischen Ausfällen zu erwarten ist. Anhand dieser Metrik können Sie berechnen, wie häufig ein Dienst nicht verfügbar sein wird.
- **Vereinbarungen zum Servicelevel (SLAs)**. Diese können sowohl Microsoft-Verpflichtungen für Verfügbarkeit und Konnektivität von Azure-Diensten sowie Verpflichtungen des Unternehmens gegenüber externen und internen Kunden beinhalten.
- **Bereitstellungszeit**. Die Zeitspanne, die zum Bereitstellen von Updates auf einem vorhandenen System benötigt wird.
- **Nicht konforme Ressourcen**. Die Anzahl oder der Prozentsatz der Ressourcen, die nicht mit den definierten Richtlinien konform sind.

## <a name="risk-tolerance-indicators"></a>Risikotoleranzindikatoren

Risiken im Zusammenhang mit der Beschleunigung der Bereitstellung beziehen sich hauptsächlich auf die Anzahl und Komplexität der cloudbasierten Systeme, die für Ihr Unternehmen bereitgestellt werden. Mit dem Wachstum der Cloudumgebung erhöht sich auch die Anzahl der bereitgestellten Systeme und die Häufigkeit der Aktualisierung Ihrer Cloudressourcen. Durch die Abhängigkeiten zwischen den Ressourcen wird es immer wichtiger, eine ordnungsgemäße Konfiguration der Ressourcen sicherzustellen und resiliente Systeme zu entwerfen, falls eine oder mehrere Ressourcen unerwartet ausfallen.

<!-- "en-us" location is required for the URL below. -->

Denken Sie zu einem frühen Zeitpunkt der Entwicklung Ihrer Cloudeinführungsstrategie über eine Organisationskultur mit DevOps- oder [DevSecOps](https://www.microsoft.com/en-us/securityengineering/devsecops)-Vorgehensweisen nach. Herkömmliche IT-Organisationen in Unternehmen bilden oftmals siloartige Teams für Betrieb, Sicherheit und Entwicklung, die häufig nicht gut zusammenarbeiten, gelegentlich sogar kontraproduktiv oder den anderen feindlich gesinnt sind. Wenn Sie diese Herausforderungen zu einem frühen Zeitpunkt erkennen und wichtige Mitglieder der einzelnen Teams einbinden, können Sie für Flexibilität, aber auch für Sicherheit und eine gute Steuerung bei der Cloudeinführung sorgen.

Identifizieren Sie zusammen mit Ihrem DevSecOps-Team und der Unternehmensführung die [Geschäftsrisiken](business-risks.md) im Zusammenhang mit der Konfiguration, und bestimmen Sie anschließend eine akzeptable Baseline für die entsprechende Risikotoleranz. Dieser Abschnitt des CAF-Leitfadens enthält einige Beispiele. Die Risiken und Baselines in Ihrem Unternehmen bzw. bei Ihren Bereitstellungen weichen im Detail wahrscheinlich davon ab.

Legen Sie nach der Einigung auf eine Baseline minimale Benchmarks fest, die eine unzulässige Zunahme der identifizierten Risiken darstellen. Diese Benchmarks fungieren als Auslöser und geben an, wann Sie Maßnahmen zum Beheben dieser Risiken ergreifen müssen. Im Folgenden wird anhand einiger Beispiele dargestellt, wie konfigurationsbezogene Metriken (z.B. die oben erwähnten) eine höhere Investition in die Disziplin „Beschleunigung der Bereitstellung“ rechtfertigen können.

- **Trigger für die Vereinbarung zum Servicelevel (SLA)**. Ein Unternehmen, das seine mit externen Kunden oder internen Partnern getroffenen Vereinbarungen zum Servicelevel (SLAs) nicht einhalten kann, sollte in die Disziplin „Beschleunigung der Bereitstellung“ investieren, um die Systemausfallzeiten zu reduzieren.
- **Trigger für die Wiederherstellungszeit**. Ein Unternehmen, das die erforderlichen Schwellenwerte für die Wiederherstellungszeit nach einem Systemausfall überschreitet, sollte in die Verbesserung der Disziplin „Beschleunigung der Bereitstellung“ und in das Systemdesign investieren, um Fehler zu reduzieren bzw. ganz zu beseitigen oder die Auswirkungen beim Ausfall einzelner Komponenten abzufedern.
- **Trigger bei Konfigurationsabweichungen**. Ein Unternehmen, bei dem unerwartete Änderungen an der Konfiguration wichtiger Systemkomponenten oder Fehler bei Bereitstellung oder Systemupdates auftreten, sollte in die Disziplin „Beschleunigung der Bereitstellung“ investieren, um die Hauptursachen und Lösungsschritte zu identifizieren.  
- **Trigger bei Nichtkonformität**. Überschreitet die Anzahl der nicht konformen Ressourcen (entweder als Zahlenwert oder als Prozentsatz der Gesamtressourcen) einen definierten Schwellenwert, sollte ein Unternehmen in Verbesserungen der Disziplin „Beschleunigung der Bereitstellung“ investieren, um sicherzustellen, dass die Konfiguration der einzelnen Ressourcen während des gesamten Lebenszyklus der Ressource konform bleibt.
- **Trigger für den Projektzeitplan**. Wenn die Zeit zum Bereitstellen von Unternehmensressourcen und -anwendungen häufig einen bestimmten Schwellenwert überschreitet, sollte ein Unternehmen in die Prozesse im Zusammenhang mit der Beschleunigung der Bereitstellung investieren und aus Konsistenz- und Verhersagbarkeitsgründen automatische Bereitstellungen einführen (oder optimieren). Bereitstellungszeiten, die in Tagen oder sogar Wochen gemessen werden, weisen in der Regel auf eine suboptimale Strategie für die Beschleunigung der Bereitstellung hin.

## <a name="next-steps"></a>Nächste Schritte

Dokumentieren Sie mit der Vorlage [Cloudverwaltung](./template.md) die Metriken und Toleranzindikatoren, die sich an dem aktuellen Cloudeinführungsplan orientieren.

Erstellen Sie anhand der Risiken und Toleranzen einen Prozess, mit dem Sie die Einhaltung der Richtlinie für die Beschleunigung der Bereitstellung steuern und kommunizieren.

> [!div class="nextstepaction"]
> [Festlegen von Prozessen zur Einhaltung von Richtlinien](compliance-processes.md)
