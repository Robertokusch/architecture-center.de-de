---
title: 'CAF: Große Unternehmen: mehrere Governance-Ebenen in großen Unternehmen'
titleSuffix: Microsoft Cloud Adoption Framework for Azure
ms.service: architecture-center
ms.subservice: enterprise-cloud-adoption
ms.custom: governance
ms.date: 02/11/2019
description: 'Große Unternehmen: mehrere Governance-Ebenen in großen Unternehmen'
author: BrianBlanchard
ms.openlocfilehash: 42f4159ca0701c6a798f239359509a3e3b246c38
ms.sourcegitcommit: 273e690c0cfabbc3822089c7d8bc743ef41d2b6e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/08/2019
ms.locfileid: "55901861"
---
# <a name="multiple-layers-of-governance-in-large-enterprises"></a>Große Unternehmen: mehrere Governance-Ebenen in großen Unternehmen

Wenn große Unternehmen mehrere Governance-Ebenen benötigen, gibt es ein höheres Maß an Komplexität, das in den Governance MVP und spätere Weiterentwicklungen der Governance einfließen muss.

Dies sind einige allgemeine Beispiele für solche Komplexität:

- Verteilte Governance-Funktionen.
- Eine Unternehmens-IT, die IT-Organisationen von Geschäftseinheiten unterstützt.
- Eine Unternehmens-IT, die geografisch verteilte IT-Organisationen unterstützt.

Dieser Artikel untersucht einige Möglichkeiten, in dieser Komplexität die Orientierung zu bewahren.

## <a name="large-enterprise-governance-is-a-team-sport"></a>Governance in großen Unternehmen ist ein Mannschaftssport

Große, etablierte Unternehmen verfügen oft über Teams oder Mitarbeiter, die sich auf die auf diesem Weg erwähnten Fachrichtungen spezialisiert haben. Der beschriebene Weg veranschaulicht einen Ansatz, um Governance zu einem Mannschaftssport zu machen.

In vielen großen Unternehmen können sich die Fachrichtungen der Cloud Governance als Einführungshindernisse erweisen. Die Entwicklung von Cloudexpertise in den Bereichen Identität, Sicherheit, Betrieb, Bereitstellung und Konfiguration in einem gesamten Unternehmen erfordert Zeit. Eine ganzheitliche Implementierung von IT-Governance-Richtlinien und IT-Sicherheit kann Innovationen um Monate oder sogar Jahre verzögern. Das Gleichgewicht zwischen dem Innovationsbedarf des Unternehmens und dem Governance-Bedarf zum Schutz der vorhandenen Ressourcen ist schwierig.

Die inhärenten Fähigkeiten der Cloud können Innovationshemmnisse beseitigen, aber die Risiken erhöhen. Auf unserem Weg durch das Feld der Governance haben wir gezeigt, wie unser Beispielunternehmen Leitplanken errichtet hat, um die Risiken zu minimieren. Anstatt sich mit jeder der Disziplinen zu befassen, die zum Schutz der Umgebung benötigt werden, verfolgt das Cloud Governance-Team einen risikobasierten Ansatz, um zu steuern, was sich einsetzen lässt, während die anderen Teams die erforderliche Cloudreife aufbauen. Besonders wichtig ist dabei, dass Governance die Lösungen der einzelnen Teams ganzheitlich übernimmt, sobald sie die Cloudreife erreicht haben. Mit zunehmender Reife der Teams und ihrem wachsendem Anteil an der Gesamtlösung kann das Cloud Governance-Team Stage Gates öffnen, wodurch Innovation und Akzeptanz zusätzlichen Schub erhalten.

Dieses Modell veranschaulicht die wachsende Partnerschaft zwischen dem Cloud Governance-Team und bestehenden Unternehmensteams (Sicherheit, IT Governance, Netzwerk, Identität und andere). Der Weg beginnt mit dem Governance-MVP und verbreitert sich dank der Weiterentwicklung der Governance zu einem ganzheitlichen Endzustand.

## <a name="requirements-to-supporting-such-a-team-sport"></a>Anforderungen zur Unterstützung von Governance als Mannschaftssport

Die erste Anforderung an ein mehrschichtiges Governance-Modell ist das Verständnis der Governance-Hierarchie. Die Antworten auf die folgenden Fragen helfen Ihnen, die allgemeine Governance-Hierarchie zu verstehen:

- Wie ist die Cloudabrechnung (Abrechnung von Clouddiensten) auf die Geschäftseinheiten verteilt?
- Wie sind die Governance-Zuständigkeiten zwischen der Unternehmens-IT und den einzelnen Geschäftseinheiten verteilt?
- Welche Arten von Umgebungen verwalten die einzelnen IT-Einheiten?

## <a name="central-governance-of-a-distributed-governance-hierarchy"></a>Zentrale Governance einer verteilten Governance-Hierarchie

Mithilfe von Tools wie Verwaltungsgruppen kann die Unternehmens-IT eine Hierarchiestruktur erstellen, die der Governance-Hierarchie entspricht. Tools wie Azure Blueprints können Ressourcen auf verschiedene Ebenen dieser Hierarchie anwenden. Azure Blueprints können versioniert werden, und verschiedene Versionen können auf Verwaltungsgruppen, Abonnements oder Ressourcengruppen angewendet werden. Die einzelnen Konzepte sind im Governance-MVP ausführlicher erläutert.

Der wichtige Aspekt jedes dieser Tools ist seine Fähigkeit, mehrere Blaupausen auf eine Hierarchie anzuwenden. Dadurch kann Governance zu einem mehrstufigen Prozess werden. Hier folgt ein Beispiel dieser hierarchischen Anwendung von Governance:

- Unternehmens-IT: Die Unternehmens-IT erstellt eine Reihe von Standards und Richtlinien, die die gesamte Cloudeinführung betreffen. Dies wird in einer "Baseline"-Blaupause festgehalten. Die Unternehmens-IT besitzt dann die Hierarchie der Verwaltungsgruppen, was sicherstellt, dass eine Version der Baseline auf alle Abonnements in der Hierarchie angewendet wird.
- Regionale oder Geschäftseinheits-IT: Verschiedene IT-Teams können eine zusätzliche Governance-Ebene anwenden, indem sie ihre eigene Blaupause erstellen. Mithilfe dieser Blaupausen werden zusätzliche Richtlinien und Standards erstellt. Nach der Entwicklung kann die Unternehmens-IT diese Blaupausen auf die entsprechenden Knoten innerhalb der Verwaltungsgruppenhierarchie anwenden.
- Cloudeinführungsteams: Detaillierte Entscheidungen und Implementierungen von Anwendungen oder Workloads können von den Cloudeinführungsteams im Rahmen von Governance-Anforderungen getroffen bzw. vorgenommen werden. Gegebenenfalls kann das Team auch zusätzliche Azure Resource Consistency Templates anfordern, um die Einführungsbemühungen zu beschleunigen.

Die Einzelheiten der Umsetzung der Governance auf jeder Ebene erfordern eine Koordination zwischen den einzelnen Teams. Die auf diesem Weg skizzierten Governance-MVP und Governance-Entwicklungen können zur Abstimmung dieser Koordination beitragen.
