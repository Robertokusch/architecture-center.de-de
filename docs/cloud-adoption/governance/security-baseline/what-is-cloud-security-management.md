---
title: 'CAF: Was ist die Cloudsicherheitsbaseline'
titleSuffix: Microsoft Cloud Adoption Framework for Azure
ms.service: architecture-center
ms.subservice: enterprise-cloud-adoption
ms.custom: governance
ms.date: 10/10/2018
description: Was ist die Cloudsicherheitsbaseline?
author: BrianBlanchard
ms.openlocfilehash: cb6e3372fd76486e5c4467ef822163eac0499573
ms.sourcegitcommit: 273e690c0cfabbc3822089c7d8bc743ef41d2b6e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/08/2019
ms.locfileid: "55901693"
---
<!-- markdownlint-disable MD026 -->

# <a name="what-is-the-cloud-security-baseline"></a>Was ist die Cloudsicherheitsbaseline?

Dieser Artikel ist eine allgemeine Einführung in das Thema Cloudsicherheitsbaseline, die auf den [fünf Disziplinen von Cloud Governance](../governance-disciplines.md) zum Aufbau eines Governance-Frameworks basiert. Ausführlichere Informationen zur Cloudsicherheit finden Sie in der [vertrauenswürdigen Cloud von Azure](https://azure.microsoft.com/overview/trusted-cloud/). Methoden zur Verbesserung des Sicherheitsstatus Ihrer Organisation finden Sie im [Katalog der Cloudsicherheitsdienste](https://www.microsoft.com/security/information-protection).

> [!NOTE]
> Dieser Artikel ist nicht dazu gedacht, ausreichenden Kontext für den Leser zum Implementieren einer Sicherheitsstrategie zu bieten. Er dient lediglich zur allgemeinen Information.

## <a name="cloud-security"></a>Cloudsicherheit

Bei der Cloudsicherheit handelt es sich um eine Erweiterung der herkömmlichen Methoden der Informationssicherheit. Herkömmliche IT-Sicherheit umfasst Richtlinien und Kontrollen für Computersicherheit, Netzwerksicherheit, Datenschutz, Informationsnutzung usw. Dieselben Richtlinien und Kontrollen sind auch in der Cloud erforderlich. Bei jeder Cloudtransformation ist es unerlässlich, dass der CISO aktiv beteiligt ist und mit der Cloudlandschaft vertraut ist. Nur so lässt sich sicherstellen, dass IT-Legacyrichtlinien den Steuerungsebenen in der Cloud korrekt zugeordnet werden.

Als Mindestanforderung sollten bei jeder Cloudsicherheitsstrategie die folgenden Punkte berücksichtigt werden:

* Klassifizieren von Daten: Richtige Datenklassifizierung, um Datenquellen zu erkennen, die privat, geschützt oder streng vertraulich sind. Dies hilft beim Risikomanagement während der Planung.
* Planen für ein Hybridcloudszenario: Durch Kenntnisse darüber, wie ältere, lokale Netzwerke eine Verbindung mit der Cloud herstellen, kann der CISO Risiken erkennen und reduzieren.
* Planen für Angriffe und Fehler: In den ersten Monaten einer Transformation werden Fehler gemacht, während das Team lernt. Beginnen Sie mit geringem Risiko und stark eingeschränkten Bereitstellungen, um ein sicheres Lernen zu ermöglichen.
* Priorisieren von Datenschutz: Während jeder Transformation sollte für das gesamte Team der Schutz von Kunden- und Mitarbeiterdaten oberste Priorität haben. Ihre Daten sind in der Cloud sicher, doch sollte das Team beim Umgang mit sensiblen Daten besonders aufmerksam und vorsichtig sein.

## <a name="protecting-data-and-privacy"></a>Schutz von Daten und Privatsphäre

Für Organisationen weltweit &ndash; ob Regierungsbehörden, gemeinnützige Organisationen oder Unternehmen &ndash; ist Cloud Computing zu einem wichtigen Bestandteil ihrer laufenden IT-Strategie geworden. Clouddienste ermöglichen Organisationen jeder Größe den Zugriff auf praktisch unbegrenzten Datenspeicher und befreien sie von der Notwendigkeit, eigene Netzwerke und Computersysteme zu kaufen, zu verwalten und zu aktualisieren. Microsoft und andere Cloudanbieter bieten die IT-Infrastruktur, Plattform und SaaS (Software-as-a-Service) und ermöglichen den Kunden das schnelle Hoch- und Herunterskalieren nach Bedarf, sodass nur für die genutzte Computeleistung und den genutzten Speicher bezahlt werden muss.

Da Unternehmen weiterhin die Vorteile von Clouddiensten wie größere Auswahl und Flexibilität nutzen und gleichzeitig die Effizienz steigern und IT-Kosten senken möchten, muss überlegt werden, wie sich die Einführung von Clouddiensten auf Datenschutz, Sicherheit und Konformitätsstatus auswirkt. Microsoft arbeitet daran, seine Cloudangebote nicht nur skalierbar, zuverlässig und verwaltbar zu machen, sondern auch sicherzustellen, dass die Daten der Kunden geschützt und auf transparente Weise genutzt werden.

Sicherheit ist ein wesentlicher Bestandteil starker Datenschutzmaßnahmen in allen Online-Computingumgebungen. Aber Sicherheit allein reicht nicht aus. Die Bereitschaft von Kunden und Unternehmen, ein bestimmtes Cloud Computing-Produkt zu verwenden, hängt auch davon ab, ob sie darauf vertrauen können, dass ihre Informationen geschützt werden und ihre Daten nur entsprechend den Erwartungen der Kunden verwendet werden. Erfahren Sie mehr über den Ansatz von Microsoft zum [Schutz von Daten und Privatsphäre in der Cloud](https://go.microsoft.com/fwlink/?LinkId=808242&clcid=0x409).

## <a name="risk-mitigation"></a>Risikominderung

Die beiden größten Risiken in jedem Rechenzentrum können in zwei Gruppen zusammengefasst werden: alternde Systeme und menschliche Fehler. Der Schutz vor diesen beiden Risiken stellt eine Mindestanforderung beim Definieren einer IT-Sicherheitsstrategie dar. Dasselbe gilt in der Cloud. Es folgen einige Beispiele für Kontrollen, die eingesetzt werden können, um Risiken zu verringern und Ihre Cloudsicherheitsstrategie zu stärken.

* Legacysysteme: Viele Komponenten in lokalen Rechenzentrumslösungen bestehen aus Software, Hardware und Prozessen, die den aktuellen Sicherheitsrisiken nicht mehr entsprechen. Diese Systeme sollten bei einer Cloudtransformation nach Möglichkeit korrigiert, ersetzt oder außer Betrieb gestellt werden. Natürlich ist das nicht immer machbar. Wenn Legacysysteme bei einer Hybridlösung in der Produktionsumgebung verbleiben, ist es wichtig, dass diese Systeme während des Entwurfs des virtuellen Rechenzentrums inventarisiert und verstanden werden. Dadurch ist das Entwurfsteam in der Lage, den Zugriff auf diese Systeme aus der Cloud zu unterbinden oder zu kontrollieren.
* IT-Sicherheitsprozesse und -kontrollen: Als Mindestvoraussetzung sollten Cloudentwurfsteams hinsichtlich bestehender IT-Sicherheitsprozesse und -kontrollen auf den neuesten Stand gebracht werden, damit diese in die Cloud übertragen werden. Im Idealfall wird ein Mitglied des IT-Sicherheitsteams in Cybersicherheit geschult und als Mitglied des Entwurfs- und Implementierungsteams eingesetzt.
* Überwachung und Auditing: Beim Entwurf von Governanceprozessen und -Tools ist darauf zu achten, dass die Lösungen für Überwachung und Auditing auch Sicherheitsrisiken oder -verstöße umfassen.

> [!NOTE]
> Offenlegung technischer Schulden: Dieses Thema weist keine umsetzbaren nächsten Schritte auf. Das Thema wird im Laufe der Zeit durch zusätzliche Artikel erweitert. Ausführlichere Informationen zur Cloudsicherheit finden Sie in der [vertrauenswürdigen Cloud von Azure](https://azure.microsoft.com/overview/trusted-cloud/). Methoden zur Verbesserung des Sicherheitsstatus Ihrer Organisation finden Sie im [Katalog der Cloudsicherheitsdienste](https://www.microsoft.com/security/information-protection).