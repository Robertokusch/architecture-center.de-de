---
title: 'CAF: Passen Sie die Entwurfshandbücher an die Richtlinie an.'
description: Wie passen Sie Entwurfshandbücher an die Richtlinie an?
author: BrianBlanchard
ms.date: 01/04/2019
ms.openlocfilehash: 77a35597585e5f58967ea79d3c7b0fa17b6ab80e
ms.sourcegitcommit: 273e690c0cfabbc3822089c7d8bc743ef41d2b6e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/08/2019
ms.locfileid: "55900736"
---
<!---
I've established policies. How to help developers adopt these policies?
Draft an architecture design guide.

[Aspirational statement] If you're using Azure, you can use one of ours as a starting point. The choose one of the following 6 as a starting point and mold it to fit your policies.
--->

<!-- markdownlint-disable MD026 -->

# <a name="how-do-you-align-design-guides-with-policy"></a>Wie passen Sie Entwurfshandbücher an die Richtlinie an?

Nachdem Sie [Cloudrichtlinien definiert](define-policy.md) haben, die auf Ihren [identifizierten Risiken](understanding-business-risk.md) basieren, müssen Sie handlungsrelevante Leitfäden generieren, die an diese Richtlinien für Ihre IT-Mitarbeiter und Entwickler angepasst sind, damit sie sich darauf beziehen können. Das Entwerfen eines Entwurfshandbuch für Cloud-Governance ermöglicht es Ihnen, bestimmte strukturelle, technologische und prozessuale Optionen anzugeben, die auf den Richtlinienanweisungen basieren, die Sie für jede der fünf Governance-Disziplinen generiert haben.

Ein Entwurfshandbuch zur Cloud-Governance sollte die Architekturoptionen und Entwurfsmuster für jede der zentralen Infrastrukturkomponenten von Cloudbereitstellungen einrichten, die Ihre Richtlinienanforderungen am besten erfüllen. Neben diesen sollten Sie eine allgemeine Erläuterung der Technologie, Tools und Prozesse liefern, die jede dieser Entwurfsentscheidungen unterstützen werden.

Obwohl Ihre Risikoanalyse und Richtlinienanweisungen möglicherweise zu einem gewissen Grad unabhängig von der Cloudplattform sein können, sollte Ihr Entwurfshandbuch plattformspezifische Implementierungsdetails für Ihre IT und Entwickler enthalten. Konzentrieren Sie sich auf die Architektur, Tools und Features Ihrer gewählten Plattform, wenn Sie Ihre Entwurfsentscheidung treffen und einen Leitfaden bereitstellen.

Während Cloud-Entwurfshandbücher einige der technischen Details berücksichtigen sollten, die mit den einzelnen Infrastrukturkomponenten verbunden sind, sollen sie keine umfangreichen technischen Dokumente oder Spezifikationen sein. Stellen Sie sicher, dass Ihre Handbücher alle Ihre Richtlinienanweisungen behandeln, und geben Sie die Entwurfsentscheidungen in einem Format an, das für Ihre Mitarbeiter einfach zu verstehen ist und für Bezugnahmen verwendet werden kann.

<!-- markdownlint-enable MD033 -->

## <a name="using-the-actionable-governance-journeys"></a>Verwenden der handlungsrelevanten Governance Journeys

Wenn Sie planen, die Azure-Plattform für Ihre Cloudeinführung zu nutzen, bietet das CAF [Governance Journeys](../journeys/overview.md), um den inkrementellen Ansatz des CAF Governance-Modells zu veranschaulichen. Diese beschreibenden Journeys decken einen Bereich von allgemeinen Einführungsszenarien ab, darunter die Geschäftsrisiken, Toleranzanforderungen und Richtlinienanweisungen, die in die Erstellung eines Minimum Viable Product (MVP) für die Governance einbezogen wurden. Diese Journeys stellen eine Synthese aus realen Benutzererfahrungen beim Prozess der Cloudeinführung in Azure dar.

Während es bei jeder Cloudeinführung eindeutige Ziele, Prioritäten und Herausforderungen gibt, sollten diese Beispiele eine gute Vorlage für die Umwandlung Ihrer Richtlinie in einen Leitfaden liefern. Wählen Sie das Ihrer Situation am besten entsprechende Szenario als Ausgangspunkt aus, und passen Sie es an Ihre spezifischen Richtlinienanforderungen an.

## <a name="next-steps"></a>Nächste Schritte

Nach Abschluss des Entwurfsleitfadens richten Sie Prozesse zur Einhaltung von Richtlinien ein, um die Richtlinienkonformität sicherzustellen.

> [!div class="nextstepaction"]
> [Prozesse zur Einhaltung von Richtlinien](processes.md)