---
title: 'Framework für die Cloudeinführung (Cloud Adoption Framework, CAF): Was ist die Datenklassifizierung?'
titleSuffix: Microsoft Cloud Adoption Framework for Azure
ms.service: architecture-center
ms.subservice: enterprise-cloud-adoption
ms.custom: governance
ms.date: 02/11/2019
description: Was ist die Datenklassifizierung?
author: BrianBlanchard
ms.openlocfilehash: bb40745b02e4ad6d7faa7054bf62443964f6784b
ms.sourcegitcommit: c053e6edb429299a0ad9b327888d596c48859d4a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/20/2019
ms.locfileid: "58245141"
---
<!-- markdownlint-disable MD026 -->

# <a name="what-is-data-classification"></a>Was ist die Datenklassifizierung?

Dieser Artikel ist eine Einführung in das allgemeine Thema „Datenklassifizierung“. Die Datenklassifizierung ist ein sehr häufiger Ausgangspunkt für die gesamte Governance.

## <a name="business-risks-and-governance"></a>Geschäftsrisiken und Governance

In den meisten Organisationen können die Hauptgründe für eine Investition in Governance auf drei Geschäftsrisiken reduziert werden:

* Haftung im Zusammenhang mit Datenpannen
* Geschäftsunterbrechung infolge von Ausfällen
* Nicht geplante bzw. unerwartete Ausgaben

Es gibt viele Varianten dieser drei Geschäftsrisiken. Die hier aufgeführten Risiken sind jedoch die gängigsten.

## <a name="understand-then-mitigate"></a>Verstehen, dann mindern

Bevor ein Risiko gemindert werden kann, muss es verstanden werden. Im Fall der Haftung bei Datenpannen beginnt dieses Verstehen mit der Datenklassifizierung. Datenklassifizierung ist der Prozess, in dem jedem Objekt in einer digitalen Ressource ein Metadatenmerkmal zugeordnet wird, das den mit diesem Objekt verknüpften Datentyp identifiziert.

Microsoft empfiehlt, dass es zu einem Objekt, das als möglicher Kandidat für Migration in oder Bereitstellung für die Cloud identifiziert wurde, dokumentierte Metadaten zum Aufzeichnen der Datenklassifizierung, geschäftlichen Bedeutung und Verantwortung für Abrechnung geben sollte. Diese drei Aspekte der Klassifizierung können zum Verstehen und Mindern von Risiken beitragen.

## <a name="microsofts-data-classification"></a>Datenklassifizierung bei Microsoft

In der nachstehenden Liste sind die von Microsoft verwendeten Klassifizierungen aufgeführt. Je nach Ihrer Branche oder vorhandenen Sicherheitsanforderungen gibt es in Ihrer Organisation vielleicht bereits Standards für Datenklassifizierungen. Wenn es keinen Standard gibt, können Sie gerne diese Beispielklassifizierung verwenden, um Ihre digitalen Ressourcen und Ihr Risikoprofil besser zu verstehen.  

* **Nicht geschäftlich:** Daten aus Ihrem Privatleben, die nicht zu Microsoft gehören
* **Öffentlich:** Geschäftsdaten, die frei verfügbar sind und für den öffentlichen Gebrauch genehmigt werden
* **Allgemein:** Geschäftsdaten, die nicht für eine öffentliche Zielgruppe bestimmt sind
* **Vertraulich:** Geschäftsdaten, die Microsoft Schaden zufügen könnten, wenn sie übermäßig freigegeben werden
* **Streng vertraulich:** Geschäftsdaten, die Microsoft großen Schaden zufügen könnten, wenn sie übermäßig freigegeben werden

## <a name="tagging-data-classification-in-azure"></a>Kennzeichnen der Datenklassifizierung in Azure

Jeder Cloud-Dienstanbieter sollte einen Mechanismus zum Aufzeichnen von Metadaten zu einem Objekt anbieten. Metadaten sind sehr wichtig für die Verwaltung von Objekten in der Cloud. Im Fall von Azure sind Ressourcentags die empfohlene Vorgehensweise für Metadatenspeicher. Zusätzliche Informationen zu Tags für Ressourcen in Azure finden Sie im Artikel zu [Verwenden von Tags zum Organisieren Ihrer Azure-Ressourcen](/azure/azure-resource-manager/resource-group-using-tags).

## <a name="next-steps"></a>Nächste Schritte

Wenden Sie Datenklassifizierungen während einer der handlungsrelevanten Governance Journeys an.

> [!div class="nextstepaction"]
> [Nützliche Governance-Vorgehensweisen](../journeys/overview.md)
