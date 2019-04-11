---
title: 'CAF: Was ist eine Überprüfung von Cloudrichtlinien?'
titleSuffix: Microsoft Cloud Adoption Framework for Azure
ms.service: architecture-center
ms.subservice: enterprise-cloud-adoption
ms.custom: governance
ms.date: 02/11/2019
description: Was ist eine Überprüfung von Cloudrichtlinien?
author: BrianBlanchard
ms.openlocfilehash: 2e50c6bac0118db1b6a223244cf8efc43a4ab438
ms.sourcegitcommit: c053e6edb429299a0ad9b327888d596c48859d4a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/20/2019
ms.locfileid: "58242081"
---
<!-- markdownlint-disable MD026 -->

# <a name="what-is-a-cloud-policy-review"></a>Was ist eine Überprüfung von Cloudrichtlinien?

Eine **Überprüfung von Cloudrichtlinien** ist der erste Schritt zur [Governance-Einsatzreife](../overview.md) in der Cloud. Das Ziel dieses Prozesses ist eine Modernisierung der vorhandenen IT-Unternehmensrichtlinien. Nach seinem Abschluss bieten die aktualisierten Richtlinien eine gleichwertige Ebene der Risikominderung für cloudbasierte Ressourcen. In diesem Artikel wird der Prozess zur Überprüfung von Cloudrichtlinien und deren Bedeutung erläutert.

## <a name="why-perform-a-cloud-policy-review"></a>Warum sollte eine Überprüfung von Cloudrichtlinien durchgeführt werden?

In den meisten Unternehmen wird die IT über die Ausführung von Prozessen verwaltet, die sich an die geltenden Richtlinien angleichen. In kleinen Unternehmen gibt es möglicherweise nur einzelne Richtlinien, und Prozesse sind ungenau definiert. Wenn aus Unternehmen Großunternehmen werden, werden Richtlinien und Prozesse tendenziell klarer dokumentiert und konsequenter ausgeführt.

Während Unternehmen für ausgereiftere IT-Unternehmensrichtlinien sorgen, gibt es bei Abhängigkeiten von den letzten technischen Entscheidungen die Tendenz, in leitende Richtlinien einzudringen. So kommt es beispielsweise häufig vor, dass Notfallwiederherstellungsprozesse Richtlinien enthalten, die Offsite-Bandsicherungen erfordern. Diese Bedingung setzt eine Abhängigkeit von einem einzigen Technologietyp (Bandsicherungen) voraus, der vielleicht nicht mehr die relevanteste Lösung ist.

Cloudtransformationen schaffen einen natürlichen Wendepunkt, um die Entscheidungen zu Legacyrichtlinien aus der Vergangenheit zu überdenken. Technische Funktionen und Standardprozesse ändern sich in der Cloud erheblich, genauso wie die Vererbungsrisiken. Im vorherigen Beispiel ist die Bandsicherungsrichtlinie auf das Risiko eines Single Point of Failure zurückzuführen, der sich durch die Speicherung der Daten an einem einzelnen Ort ergab und das Unternehmen dazu veranlasste, das Risikoprofil durch die Minderung dieses Risikos zu verbessern. In einer Cloudbereitstellung gibt es mehrere Optionen, die dieselbe Risikominderung mit viel geringeren RTOs (Recovery Time Objectives) bieten. Beispiel:

- Eine cloudnative Lösung könnte die geografische Replikation der SQL Azure-Datenbank aktivieren.
- Eine Hybridlösung kann beispielsweise Azure Site Recovery nutzen, um eine IaaS-Workload in mehreren Datencentern zu replizieren.

Bei Ausführung einer Cloudtransformation steuern Richtlinien oft viele der Tools, Dienste und Prozesse, die für die Cloudeinführungsteams zur Verfügung stehen. Wenn diese Richtlinien auf älteren Technologien basieren, können sie die Bemühungen des Teams behindern, für Änderungen zu sorgen. Im schlimmsten Fall werden wichtige Richtlinien vom Migrationsteam vollständig ignoriert, um Problemumgehungen zu ermöglichen. Keines davon ist ein akzeptables Ergebnis.

## <a name="the-cloud-policy-review-process"></a>Der Prozess der Überprüfung von Cloudrichtlinien

Überprüfungen von Cloudrichtlinien gleichen vorhandene Richtlinien für IT-Governance und IT-Sicherheit an die [fünf Disziplinen von Cloud Governance](../overview.md) an: [Cost Management](../cost-management/overview.md), [Sicherheitsbaseline](../security-baseline/overview.md), [Identitätsbaseline](../identity-baseline/overview.md), [Ressourcenkonsistenz](../resource-consistency/overview.md) und [Beschleunigung der Bereitstellung](../deployment-acceleration/overview.md).

Bei jeder dieser Disziplinen besteht der Überprüfungsprozess aus folgenden Schritten:

1. Überprüfung vorhandener lokaler Richtlinien im Hinblick auf die jeweilige Disziplin, wobei nach zwei wichtigen Datenpunkten gesucht wird: Legacyabhängigkeiten und identifizierte Geschäftsrisiken.
2. Auswertung jedes Geschäftsrisikos, indem eine einfache Frage gestellt wird: „Ist das Geschäftsrisiko in einem Cloudmodell weiterhin vorhanden?“
3. Wenn das zutrifft, schreiben Sie die Richtlinie um, indem Sie die erforderliche Risikominderung (nicht die technische Lösung) dokumentieren.
4. Überprüfen Sie die aktualisierte Richtlinie zusammen mit den Cloudeinführungsteams, um mögliche Lösungen für die erforderliche Risikominderung zu verstehen.

## <a name="example-of-a-policy-review-for-a-legacy-policy"></a>Beispiel für eine Richtlinienüberprüfung bei einer Legacyrichtlinie

Als Beispiel für den erforderlichen Prozess wird die Bandsicherungsrichtlinie aus dem vorgehenden Abschnitt verwendet:

- Eine Unternehmensrichtlinie erfordert Offsite-Bandsicherungen für alle Produktionssysteme. In dieser Richtlinie gibt es zwei relevante Datenpunkte:
  - Legacyabhängigkeit bei einer Bandsicherungslösung
  - Ein angenommenes Geschäftsrisiko im Zusammenhang mit der Speicherung von Sicherungen an demselben physischen Ort wie die Produktionsgeräte.
- Ist das Risiko weiterhin vorhanden? Ja. Sogar in der Cloud ist die Abhängigkeit von einem einzigen Standort mit einem gewissen Risiko verbunden. Zwar ist es weniger wahrscheinlich, dass sich dieses Risiko auf das Geschäft auswirkt, wie es bei der lokalen Lösung der Fall war, doch das Risiko besteht weiterhin.
- Schreiben Sie die Richtlinie um. Bei einem Notfall, der ein gesamtes Datencenter betrifft, muss es innerhalb von 24 Stunden nach dem Ausfall eine Möglichkeit zur Wiederherstellung von Produktionssystemen in einem anderen Datencenter und an einem anderen geografischen Standort geben.
- Überprüfen Sie dies mit den Cloudeinführungsteams. Je nach implementierter Lösung gibt es mehrere Möglichkeiten zur Einhaltung dieser Richtlinie zur Ressourcenkonsistenz.

## <a name="tools-to-help-create-modern-policies"></a>Tools als Hilfe zum Erstellen moderner Richtlinien

Um die Erstellung moderner Richtlinien zu beschleunigen, steht eine Reihe von Beispielrichtlinien in jeder der fünf Disziplinen von Cloud Governance zur Verfügung. Jede dieser Beispielrichtlinien beginnt mit einer dieser drei Entwurfsannahmen:

- **Cloudnativ**: Die bereitgestellte Lösung ist cloudnativ und kann von den in Azure enthaltenen Standardlösungen, mit minimaler Konfiguration, profitieren.
- **Enterprise**: Die bereitgestellte Lösung ist komplex und erfordert ein Hybrid Cloud-Bereitstellungsmodell. Dazu sind komplexere Implementierungen bestimmter Governance-Disziplinen erforderlich.
- **Kompatibel mit dem Cloud-Entwurfsprinzip (CDP)**: Die bereitgestellte Lösung muss die im CDP definierten Architekturachsen einhalten, wofür ein viel höheres Maß an Governance erforderlich ist.  

Für jede Disziplin muss eine Beispielrichtlinie auf jeder dieser Ebenen erstellt werden. Jedes Beispiel soll innerhalb der Unternehmensumgebung Gedanken und Unterhaltungen auslösen. Beachten Sie bitte: Bei diesen Beispielen wird nicht beabsichtigt, dass sie als Alternative zu einer ordnungsgemäß erstellten IT-Unternehmensrichtlinie verwendet werden.
