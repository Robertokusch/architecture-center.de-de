---
title: 'CAF: Definieren der Anweisungen von Unternehmensrichtlinien'
description: Wie richten Sie eine Richtlinie ein, um Risiken wiederzugeben und zu verringern?
author: BrianBlanchard
ms.date: 01/02/2019
ms.openlocfilehash: 94645d55cb30aea55dceeb96a2cb9a4af6098b5a
ms.sourcegitcommit: c053e6edb429299a0ad9b327888d596c48859d4a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/20/2019
ms.locfileid: "58241221"
---
<!---
I understand risk and tolerance, now what do I do?
Define the policy... [aspirational statement to move towards 2/1] If you need help defining policies, each discipline includes references to common business risks and policies to mitigate the risks...
--->

# <a name="defining-corporate-policy-for-cloud-governance"></a>Definieren von Unternehmensrichtlinien für Cloudgovernance

Nachdem Sie die bekannten Risiken und damit verbundene Risikotoleranzen für den Weg Ihrer Organisation in die Cloud analysiert haben, besteht Ihr nächster Schritt darin, eine Richtlinie einzurichten, die diese Risiken explizit behandelt und die Schritte, um sie nach Möglichkeit zu verringern, definiert.

<!-- markdownlint-disable MD026 -->

## <a name="how-can-corporate-it-policy-become-cloud-ready"></a>Wie kann für die IT-Unternehmensrichtlinie die Bereitschaft für die Cloud erzielt werden?

Bei traditioneller Governance und inkrementeller Governance wird die Arbeitsdefinition von Governance in Unternehmensrichtlinien erstellt. Die meisten IT-Governanceaktionen sind darauf ausgelegt, Technologien zum Überwachen, Erzwingen, Ausführen und Automatisieren dieser Unternehmensrichtlinien zu implementieren. Cloudgovernance basiert auf ähnlichen Konzepten.

![Unternehmensgovernance und Governancedisziplinen](../../_images/operational-transformation-govern.png)

*Abbildung 1: Unternehmensgovernance und Governancedisziplinen*

In der Abbildung werden die Interaktionen zwischen Geschäftsrisiken, Richtlinien und Compliance veranschaulicht, und es wird gezeigt, wie durch Überwachung und Erzwingung eine Governancestrategie erstellt wird. Darüber hinaus sehen Sie die fünf Disziplinen der Cloudgovernance zum Umsetzen Ihrer Strategie.

Cloudgovernance ist das Produkt von fortlaufenden Bemühungen zur Einführung, da eine nachhaltige Transformation nicht über Nacht geschieht. Der Versuch, mit einer schnellen, aggressiven Methode vollständige Cloudgovernance herzustellen, bevor zentrale Änderungen der Unternehmensrichtlinien durchdacht und umgesetzt wurden, führt nur selten zu den gewünschten Ergebnissen. Stattdessen empfiehlt sich ein inkrementeller Ansatz.

Ein Framework für die Einführung der Cloud hebt sich durch seinen Kaufzyklus sowie durch die Ermöglichung einer authentischen Transformation ab. Da keine umfangreichen Kapitalinvestitionen für Anschaffungen erforderlich sind, können Techniker bereits früher mit dem Test- und Einführungsprozess beginnen. In den meisten Unternehmenskulturen kann die Tatsache, dass Kapitalausgaben kein Hindernis für die Einführung mehr sind, zu kürzeren Feedbackschleifen, einem organischen Wachstum und inkrementeller Ausführung führen.

Die Einführung der Cloud erfordert ein Umdenken bei der Governance. In vielen Organisationen sorgt die Transformation der Unternehmensrichtlinien für verbesserte Governance und eine umfassendere Einhaltung durch inkrementelle Richtlinienänderungen sowie automatisierte Erzwingung dieser Änderungen, gestützt durch die neu definierten Funktionen, die Sie mit Ihrem Clouddienstanbieter konfigurieren.

<!-- markdownlint-enable MD026 -->

## <a name="review-existing-policies"></a>Überprüfen vorhandener Richtlinien

Während Ihre Cloudbereitstellung ausgereifter wird und ein wachsender Anteil Ihrer IT-Umgebung in die Cloud verschoben wird, ändern sich auch Ihre Risiken und die damit verbundenen Richtlinienanforderungen. Governance ist ein fortlaufender Prozess, und Richtlinien sollten regelmäßig gemeinsam mit den IT-Mitarbeitern und Projektbeteiligten überprüft werden, um sicherzustellen, dass in der Cloud gehostete Ressourcen die allgemeinen Unternehmensziele und -anforderungen weiterhin einhalten. Ihr Verständnis neuer Risiken und akzeptabler Risiken kann zu einer [Überprüfung der vorhandenen Richtlinien](what-is-a-cloud-policy-review.md) führen, bei der ermittelt werden soll, welcher Grad an Governance für Ihre Organisation geeignet ist.

> [!TIP]
> Wenn Ihre Organisation Complianceanforderungen von Drittanbietern erfüllen muss, entsteht eines der größten Geschäftsrisiken möglicherweise dadurch, dass [gesetzliche Bestimmungen eingehalten werden müssen](what-is-regulatory-compliance.md). Dieses Risiko kann häufig nicht minimiert werden, sodass eine strikte Einhaltung erforderlich ist. Sie sollten Ihre Anforderungen durch Drittanbietercompliance unbedingt verstehen, bevor Sie mit einer Richtlinienüberprüfung beginnen.

## <a name="create-cloud-policy-statements"></a>Erstellen von Cloudrichtlinienanweisungen

Cloudbasierte IT-Richtlinien legen die Anforderungen, Standards und Ziele fest, die Ihre IT-Mitarbeiter und automatisierten Systeme unterstützen müssen. Richtlinienentscheidungen sind einer der wichtigsten Faktoren in Ihrem [Cloudarchitekturentwurf](align-governance-journeys.md) und der Implementierung Ihrer [Richtlinieneinhaltungsprozesse](processes.md).

Einzelne Cloudrichtlinienanweisungen sind Anleitungen für den Umgang mit bestimmten Risiken, die während Ihres Risikobewertungsprozesses identifiziert wurden. Diese Richtlinien können in die Dokumentation Ihrer Unternehmensrichtlinien integriert werden, jedoch bieten Cloudrichtlinienanweisungen, die im CAF-Leitfaden erläutert werden, meist eine präzisere Zusammenfassung der Risiken und der Pläne für den Umgang mit ihnen. Jede Definition sollte diese Informationen enthalten:

- **Geschäftsrisiko:** Eine Zusammenfassung des Risikos, das diese Richtlinie behandelt.
- **Richtlinienanweisung:** Eine knappe Erläuterung der Richtlinienanforderungen und -ziele.
- **Entwurfs- oder technischer Leitfaden:** Direkt umsetzbare Empfehlungen, Spezifikationen oder andere Anleitungen zur Unterstützung und Erzwingung dieser Richtlinie, auf die IT-Teams und Entwickler beim Entwerfen und Erstellen ihrer Cloudbereitstellungen zurückgreifen können.

Wenn Sie Unterstützung bei den ersten Schritten der Definition von Richtlinien benötigen, lesen Sie die [Governancedisziplinen](../governance-disciplines.md), die in der Übersicht des Governanceabschnitts eingeführt werden. Die Artikel für die einzelnen Disziplinen enthalten Beispiele verbreiteter Geschäftsrisiken, die bei der Umstellung in die Cloud auftreten können, und Beispielrichtlinien zum Vermindern dieser Risiken (Beispiel: die [Beispielrichtliniendefinitionen](../cost-management/policy-statements.md) für die Disziplin der Kostenverwaltung).

## <a name="incremental-governance-and-integrating-with-existing-policy"></a>Inkrementelle Governance und Integration in vorhandene Richtlinien

Geplante Ergänzungen Ihrer Cloudumgebung sollten immer auf die Einhaltung vorhandener Richtlinien überprüft werden, und die Richtlinien sollten aktualisiert werden, sodass sie bisher nicht behandelte Probleme berücksichtigen. Sie sollten auch regelmäßig [Ihre Cloudrichtlinie überprüfen](what-is-a-cloud-policy-review.md), um sicherzustellen, dass diese auf dem neuesten Stand und im Einklang mit neuen Unternehmensrichtlinien ist.

Die Notwendigkeit, Cloudrichtlinien in Ihre bereits bestehenden IT-Richtlinien zu integrieren, hängt hauptsächlich von der Ausgereiftheit Ihrer Cloudgovernanceprozesse und der Größe Ihrer Cloudumgebung ab. Eine umfassendere Erläuterung zur Richtlinienintegration während der Cloudtransformation finden Sie im Artikel zu [inkrementeller Governance und dem Richtlinien-MVP](overview.md).

## <a name="next-steps"></a>Nächste Schritte

Entwerfen Sie nach dem Definieren Ihrer Richtlinien ein Architekturentwurfshandbuch, um für IT-Mitarbeiter und Entwickler umsetzbare Anleitungen bereitzustellen.

> [!div class="nextstepaction"]
> [Ausarbeiten eines Architekturentwurfshandbuchs](align-governance-journeys.md)
