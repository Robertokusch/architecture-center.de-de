---
title: Einschnürungsmuster
titleSuffix: Cloud Design Patterns
description: Migrieren Sie ein älteres System inkrementell, indem Sie bestimmte Teile der Funktionalität nach und nach durch neue Anwendungen und Dienste ersetzen.
keywords: Entwurfsmuster
author: dragon119
ms.date: 06/23/2017
ms.topic: design-pattern
ms.service: architecture-center
ms.subservice: cloud-fundamentals
ms.custom: seodec18
ms.openlocfilehash: f139d368c98256c0190753930983a47df81a5134
ms.sourcegitcommit: 1b50810208354577b00e89e5c031b774b02736e2
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/23/2019
ms.locfileid: "54480556"
---
# <a name="strangler-pattern"></a>Einschnürungsmuster

Migrieren Sie ein älteres System inkrementell, indem Sie bestimmte Teile der Funktionalität nach und nach durch neue Anwendungen und Dienste ersetzen. Durch das fortgesetzte Austauschen von Features des älteren Systems ersetzt das neue System schließlich alle Features des alten Systems, wodurch das alte System unbrauchbar wird und von Ihnen außer Betrieb genommen werden kann.

## <a name="context-and-problem"></a>Kontext und Problem

Mit zunehmendem Alter von Systemen überaltern auch die Entwicklungstools, die Hosting-Technologie und sogar Systemarchitekturen, auf denen sie basieren. Durch das Hinzufügen neuer Features und Funktionalität kann die Komplexität dieser Anwendungen erheblich zunehmen, sodass sie schwieriger zu verwalten oder neue Features schwerer hinzuzufügen sind.

Das vollständige Ersetzen eines komplexen Systems kann eine große Herausforderung darstellen. Häufig ist eine schrittweise Migration zu einem neuen System notwendig, während das alte System gleichzeitig zur Handhabung von Features verwendet wird, die noch nicht migriert wurden. Das Ausführen von zwei separaten Versionen einer Anwendung bedeutet jedoch, dass Clients wissen müssen, wo sich bestimmte Features befinden. Jedes Mal, wenn ein Feature oder Dienst migriert wird, müssen die Clients aktualisiert werden, um auf den neuen Speicherort zu verweisen.

## <a name="solution"></a>Lösung

Ersetzen Sie bestimmte Teile der Funktionalität nach und nach durch neue Anwendungen und Dienste. Erstellen Sie eine Fassade, die Anforderungen an das ältere Back-End-System abfängt. Die Fassade leitet diese Anforderungen entweder an die ältere Anwendung oder die neuen Dienste weiter. Vorhandene Features können allmählich zum neuen System migriert werden, und Kunden können weiterhin die gleiche Schnittstelle verwenden, ohne zu merken, dass eine Migration stattgefunden hat.

![Diagramm des Einschnürungsmusters](./_images/strangler.png)

Dieses Muster hilft, das Risiko der Migration zu minimieren und den Entwicklungsaufwand über einen Zeitraum zu verteilen. Da die Fassade die Benutzer sicher an die richtige Anwendung weiterleitet, können Sie dem neuen System in der jeweils gewünschten Geschwindigkeit Funktionalität hinzufügen und gleichzeitig sicherstellen, dass die ältere Anwendung weiterhin funktioniert. Wenn im Laufe der Zeit immer mehr Features zum neuen System migriert werden, ist das ältere System schließlich unbrauchbar und wird nicht mehr benötigt. Sobald dieser Vorgang abgeschlossen ist, kann das ältere System sicher außer Betrieb genommen werden.

## <a name="issues-and-considerations"></a>Probleme und Überlegungen

- Überlegen Sie, wie Dienste und Datenspeicher gehandhabt werden sollen, die möglicherweise sowohl vom neuen als auch vom älteren System verwendet werden. Stellen Sie sicher, dass beide parallel auf diese Ressourcen zugreifen können.
- Strukturieren Sie neue Anwendungen und Dienste auf solche Weise, dass sie leicht abgefangen und bei zukünftigen Einschnürungsmigrationen ersetzt werden können.
- Zu einem Zeitpunkt nach Abschluss der Migration ist die Fassade entweder nicht mehr vorhanden oder hat sich zu einem Adapter für ältere Clients entwickelt.
- Stellen Sie sicher, dass die Fassade mit der Migration Schritt hält.
- Stellen Sie sicher, dass die Fassade nicht zu einer einzelnen Fehlerquelle (Single Point of Failure) oder einem Leistungsengpass wird.

## <a name="when-to-use-this-pattern"></a>Verwendung dieses Musters

Verwenden Sie dieses Muster bei der schrittweisen Migration einer Back-End-Anwendung auf eine neue Architektur.

Dieses Muster ist in folgenden Fällen unter Umständen nicht geeignet:

- Wenn Anforderungen an das Back-End-System nicht abgefangen werden können.
- Für kleinere Systeme, bei denen die Komplexität eines Austausches für den Großhandel gering ist.

## <a name="related-guidance"></a>Verwandte Leitfäden

- Blogbeitrag von Martin Fowler zu [StranglerApplication](https://www.martinfowler.com/bliki/StranglerApplication.html)
