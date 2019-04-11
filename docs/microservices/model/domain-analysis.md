---
title: Domänenanalyse für Microservices
description: Domänenanalyse für Microservices.
author: MikeWasson
ms.date: 02/25/2019
ms.topic: guide
ms.service: architecture-center
ms.subservice: reference-architecture
ms.custom: microservices
ms.openlocfilehash: 7f3ce8bb7d1e03d59670cc4685589ec76738d95c
ms.sourcegitcommit: c053e6edb429299a0ad9b327888d596c48859d4a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/20/2019
ms.locfileid: "58243971"
---
# <a name="using-domain-analysis-to-model-microservices"></a>Verwenden der Domänenanalyse zur Modellierung von Microservices

Eine der größten Herausforderungen in Bezug auf Microservices besteht darin, die Grenzen der einzelnen Dienste zu definieren. Eine allgemeine Regel besagt, dass ein Dienst nur einem Zweck dienen soll. Die Umsetzung dieser Regel in die Praxis bedarf jedoch sorgfältiger Überlegung. Es gibt keinen mechanischen Prozess, mit dem das „richtige“ Design erzielt werden kann. Es ist erforderlich, dass Sie sich eingehend mit Ihrer Geschäftsdomäne und den dazugehörigen Anforderungen und Zielen beschäftigen. Andernfalls erhalten Sie unter Umständen ein planloses Design mit einigen unerwünschten Merkmalen, z.B. versteckten Abhängigkeiten zwischen Diensten, zu enger Kopplung oder schlecht entworfenen Schnittstellen. In diesem Artikel wird ein domänenbasierter Ansatz für den Entwurf von Microservices vorgestellt.

Als Beispiel dient in diesem Artikel ein Drohnenlieferdienst. Weitere Informationen zum Szenario sowie die entsprechende Referenzimplementierung finden Sie [hier](../design/index.md).

## <a name="introduction"></a>Einführung

Microservices sollten basierend auf den geschäftlichen Funktionen entworfen werden, nicht auf horizontalen Ebenen wie dem Datenzugriff oder Messaging. Außerdem sollten sie eine lose Kopplung und eine hohe funktionsbezogene Kohäsion aufweisen. Microservices sind *lose gekoppelt*, wenn Sie einen Dienst ändern können, ohne dass gleichzeitig andere Dienste aktualisiert werden müssen. Ein Microservice ist *kohäsiv*, wenn er einem einzelnen, klar definierten Zweck dient, z.B. der Verwaltung von Benutzerkonten oder der Nachverfolgung des Lieferverlaufs. Ein Dienst sollte das Wissen des Geschäftsbereichs umfassen und dieses Wissen von den Clients abstrahieren. Ein Client sollte beispielsweise dazu in der Lage sein, eine Drohne zu planen, ohne die Details des Planungsalgorithmus oder der Drohnenflottenverwaltung zu kennen.

Bei dem am Geschäftsbereich ausgerichteten Entwurf (Domain-Driven Design, DDD) wird ein Framework bereitgestellt, mit dem Sie die meisten Voraussetzungen auf dem Weg zu einem guten Entwurf der Microservices bereits erfüllen können. DDD verfügt über zwei separate Phasen: eine strategische und eine taktische Phase. In der strategischen DDD-Phase definieren Sie die übergeordnete Struktur des Systems. In dieser Phase können Sie sicherstellen, dass Ihre Architektur klar auf die geschäftlichen Funktionen ausgerichtet ist. In der taktischen DDD-Phase wird eine Gruppe von Entwurfsmustern bereitgestellt, die Sie zum Erstellen des Domänenmodells verwenden können. Zu diesen Mustern gehören Entitäten, Aggregate und Domänendienste. Diese taktischen Muster sind hilfreich beim Entwerfen von Microservices, die sowohl lose gekoppelt als auch kohäsiv sind.

![Diagramm eines DDD-Prozesses (Domain-Driven Design; am Geschäftsbereich ausgerichteter Entwurf)](../images/ddd-process.png)

In diesem und im nächsten Artikel werden die folgenden Schritte beschrieben und auf die Anwendung für die Drohnenlieferung (Drone Delivery) angewendet:

1. Zunächst analysieren wir die Geschäftsdomäne, um die funktionsbezogenen Anforderungen der Anwendung zu verstehen. Das Ergebnis dieses Schritts ist eine informelle Beschreibung des Geschäftsbereichs, die zu einer formelleren Gruppe von Domänenmodellen verfeinert werden kann.

2. Als Nächstes definieren wir die *Kontextgrenzen* der Domäne. Jeder Kontextgrenzenbereich enthält ein Domänenmodell, das eine bestimmte Unterdomäne der übergeordneten Anwendung darstellt.

3. Wenden Sie innerhalb einer Kontextgrenze taktische DDD-Muster an, um Entitäten, Aggregate und Domänendienste zu definieren.

4. Verwenden Sie die Ergebnisse des vorherigen Schritts, um die Microservices Ihrer Anwendung zu identifizieren.

In diesem Artikel werden die ersten drei Schritte behandelt, in denen es hauptsächlich um DDD geht. Im nächsten Artikel beschäftigen wir uns dann mit den Microservices. Hierbei sollte aber unbedingt beachtet werden, dass DDD ein iterativer, fortlaufender Prozess ist. Dienstgrenzen sind nicht in Stein gemeißelt. Wenn sich eine Anwendung weiterentwickelt, treffen Sie unter Umständen die Entscheidung, dass ein Dienst in mehrere kleinere Dienste unterteilt werden soll.

> [!NOTE]
> In diesem Artikel wird keine vollständige und umfassende Domänenanalyse gezeigt. Wir haben das Beispiel absichtlich kurz gehalten, um die wichtigsten Punkte besser verdeutlichen zu können. Weitere Hintergrundinformationen zu DDD finden Sie im Buch *Domain-Driven Design* von Eric Evans, in dem dieser Begriff zuerst verwendet wurde. Eine andere gute Quelle ist *Implementing Domain-Driven Design* von Vaughn Vernon.

## <a name="scenario-drone-delivery"></a>Szenario: Drohnenlieferung

Fabrikam, Inc. führt einen Drohnenlieferdienst ein. Das Unternehmen verfügt über eine Drohnenflotte. Unternehmen registrieren sich bei dem Dienst, und Benutzer können eine Drohne anfordern, die auszuliefernde Waren abholt. Wenn ein Kunde einen Abholtermin festlegt, weist ein Back-End-System eine Drohne zu und teilt dem Benutzer die voraussichtliche Lieferzeit mit. Während der Durchführung der Lieferung kann der Kunde die Position der Drohne nachverfolgen, und die voraussichtliche Ankunftszeit wird kontinuierlich aktualisiert.

Dieses Szenario beinhaltet eine recht komplizierte Domäne. Zu den Anliegen des Unternehmens zählen unter anderem die Zeitplanung für die Drohnen, die Nachverfolgung von Paketen, die Verwaltung von Benutzerkonten sowie die Speicherung und Analyse von Verlaufsdaten. Darüber hinaus ist Fabrikam an einer schnellen Marktreife interessiert und möchte dann zeitnah neue Features und Funktionen hinzufügen. Die Anwendung muss cloudfähig sein und über ein hohes Servicelevelziel (Service Level Objective, SLO) verfügen. Fabrikam erwartet außerdem erhebliche Unterschiede bei den Datenspeicher- und Abfrageanforderungen der verschiedenen Systemkomponenten. Aufgrund dieser Überlegungen hat sich Fabrikam bei seiner Drohnenlieferungsanwendung für eine Microservices-Architektur entschieden.

## <a name="analyze-the-domain"></a>Analysieren der Domäne

Bei der Nutzung eines DDD-Ansatzes können Sie Microservices so entwerfen, dass jeder Dienst auf natürliche Weise zu einer funktionsbezogenen Geschäftsanforderung passt. Auf diese Weise können Sie es vermeiden, dass Ihr Entwurf durch organisatorische Grenzen oder die ausgewählte Technologie bestimmt wird.

Bevor Sie Code schreiben, müssen Sie sich einen allgemeinen Überblick über das zu erstellende System verschaffen. Beim DDD-Ansatz wird zuerst die Geschäftsdomäne modelliert und ein *Domänenmodell* erstellt. Das Domänenmodell ist ein abstraktes Modell der Geschäftsdomäne. Hiermit wird das Domänenwissen zusammengefasst und organisiert und eine gemeinsame Sprache für Entwickler und Domänenexperten gefunden.

Beginnen Sie, indem Sie alle Geschäftsfunktionen und ihre Verbindungen zuordnen. Dies ist normalerweise ein Vorgang, der von Domänenexperten, Softwarearchitekten und anderen Projektbeteiligten gemeinsam durchgeführt wird. Es muss kein bestimmter Formalismus eingehalten werden.  Skizzieren Sie ein Diagramm, oder verwenden Sie ein Whiteboard.

Beim Erstellen des Diagramms können Sie damit beginnen, einzelne Unterdomänen zu identifizieren. Welche Funktionen sind eng verwandt? Welche Funktionen sind für das Geschäft besonders wichtig, und welche Funktionen erfüllen Hilfszwecke? Was ist das Abhängigkeitsdiagramm? Während dieser ersten Phase geht es nicht um Technologien oder Implementierungsdetails. Sie sollten sich aber darüber bewusst sein, an welchen Stellen die Anwendung in externe Systeme integriert werden muss, z.B. Systeme für CRM, Zahlungsverarbeitung oder Abrechnung.

## <a name="example-drone-delivery-application"></a>Beispiel: Drohnenlieferungsanwendung

Nach der ersten Domänenanalyse hat das Fabrikam-Team eine grobe Skizze erstellt, in der die Domäne der Drohnenlieferung dargestellt ist.

![Diagramm der Drohnenlieferdomäne](../images/ddd1.svg)

- **Lieferung** ist in der Mitte des Diagramms angeordnet, weil dies der Kern des Geschäfts ist. Alle anderen Elemente des Diagramms dienen der Ermöglichung dieser Funktionalität.
- Die **Drohnenverwaltung** ist auch ein wichtiger Teil des Geschäfts. Funktionen, die eng mit der Drohnenverwaltung verbunden sind, sind die **Drohnenreparatur** und der Einsatz von **Predictive Analysis**, um vorherzusagen, wann für Drohnen Wartungsarbeiten durchgeführt werden müssen.
- Mit der **ETA-Analyse** werden Schätzungen für den Zeitpunkt der Abholung und Lieferung bereitgestellt.
- Mit dem **Drittanbietertransport** wird die Anwendung in die Lage versetzt, alternative Transportmethoden zu planen, falls ein Paket nicht vollständig per Drohne ausgeliefert werden kann.
- Die **Drohnenvermietung** ist eine mögliche Erweiterung des Kerngeschäfts. Unter Umständen verfügt das Unternehmen zu bestimmten Zeiten über überschüssige Drohnenkapazität, sodass Drohnen vermietet werden können, die andernfalls ungenutzt bleiben würden. Dieses Feature ist nicht Teil der ersten Releaseversion.
- Die **Videoüberwachung** ist ein weiterer Bereich, in den das Unternehmen später expandieren kann.
- **Benutzerkonten**, **Rechnungsstellung** und **Callcenter** sind Unterdomänen zur Unterstützung des Kerngeschäfts.

Beachten Sie, dass wir an diesem Punkt des Prozesses noch keinerlei Entscheidungen zur Implementierung oder zur Technologie getroffen haben. Einige Untersysteme können externe Softwaresysteme oder Drittanbieterdienste betreffen. Trotzdem muss die Anwendung mit diesen Systemen und Diensten interagieren, und es ist wichtig, sie in das Domänenmodell einzubinden.

> [!NOTE]
> Wenn eine Anwendung von einem externen System abhängig ist, besteht das Risiko, dass das Datenschema oder die API des externen Systems in Ihre Anwendung hineinreicht und letztendlich den Entwurf der Architektur kompromittiert. Dies gilt besonders für ältere Systeme, bei denen ggf. keine modernen bewährten Methoden befolgt und verworrene Datenschemas oder veraltete APIs verwendet werden. In diesem Fall ist es wichtig, eine klar definierte Grenze zwischen diesen externen Systemen und der Anwendung einzurichten. Erwägen Sie zu diesem Zweck die Verwendung des [Einschnürungsmusters](../../patterns/strangler.md) oder des [Musters „Antibeschädigungsebene“](../../patterns/anti-corruption-layer.md).

## <a name="define-bounded-contexts"></a>Definieren von Kontextgrenzen

Das Domänenmodell enthält Darstellungen von realen Dingen: Benutzer, Drohnen, Pakete usw. Dies bedeutet aber nicht, dass jeder Teil des Systems für dieselben Dinge die gleichen Darstellungen verwenden muss.

Beispielsweise müssen Subsysteme, die für die Drohnenreparatur und Predictive Analytics zuständig sind, viele physische Merkmale von Drohnen darstellen, z.B. den Wartungsverlauf, Kilometerleistung, Alter, Modellnummer, Leistungsmerkmale usw. Wenn es aber um die Planung einer Lieferung geht, kümmern wir uns nicht um diese Dinge. Das Subsystem für die Planung muss nur wissen, ob eine Drohne verfügbar ist und welcher geschätzte Zeitpunkt für die Abholung und die Lieferung gilt.

Wenn wir versuchen würden, ein gemeinsames Modell für beide Subsysteme zu erstellen, wäre dies ein unnötig komplexer Vorgang. Außerdem wäre es schwieriger, das Modell im Laufe der Zeit weiterzuentwickeln, da alle Änderungen die Zustimmung mehrerer Teams erhalten müssen, die an separaten Subsystemen arbeiten. Daher ist es häufig besser, separate Modelle zu entwerfen, bei denen die gleiche reale Entität (in diesem Fall eine Drohne) in zwei unterschiedlichen Kontexten dargestellt wird. Jedes Modell enthält nur die Features und Attribute, die im jeweiligen Kontext relevant sind.

An dieser Stelle wird auf das DDD-Konzept der *Kontextgrenzen* zurückgegriffen. Eine Kontextgrenze bezeichnet einfach den abgegrenzten Bereich innerhalb einer Domäne, in dem ein bestimmtes Domänenmodell gilt. Wenn wir uns das obige Diagramm ansehen, können wir die Funktionalität danach gruppieren, ob von den einzelnen Funktionen ein Domänenmodell gemeinsam genutzt wird.

![Diagramm zu Kontextgrenzen](../images/ddd2.svg)

Kontextgrenzen bedeuten nicht unbedingt, dass Kontexte voneinander isoliert sind. In diesem Diagramm zeigen die durchgehenden Verbindungslinien zwischen den Kontextgrenzen die Stellen an, an denen die Kontexte zweier Kontextgrenzen interagieren. Beispielsweise ist „Lieferung“ von „Benutzerkonten“ abhängig, um Informationen zu Kunden zu erhalten, und von „Drohnenverwaltung“, um die Drohnen der Flotte einplanen zu können.

In seinem Buch *Domain Driven Design* beschreibt Eric Evans mehrere Muster zur Wahrung der Integrität eines Domänenmodells, wenn es mit dem Kontext einer anderen Kontextgrenze interagiert. Eines der wichtigsten Prinzipien von Microservices ist die Kommunikation der Dienste über klar definierte APIs. Dieser Ansatz entspricht zwei Mustern, die von Eric Evans als „Open Host Service“ (Offener Hostdienst) und „Published Language“ (Veröffentlichte Sprache) bezeichnet werden. Bei „Open Host Service“ definiert ein Subsystem ein formelles Protokoll (API) für die Kommunikation mit anderen Subsystemen. Mit „Published Language“ wird dieser Ansatz erweitert, indem die API in einer Form veröffentlicht wird, die von anderen Teams zum Schreiben von Clients genutzt werden kann. Im Artikel [Designing APIs for microservices](../design/api-design.md) (Entwerfen von APIs für Microservices) wird die Verwendung der [OpenAPI-Spezifikation](https://www.openapis.org/specification/repo) (ehemals Swagger) beschrieben. Sie dient zum Definieren von sprachunabhängigen Schnittstellenbeschreibungen für REST-APIs, die im JSON- oder YAML-Format ausgedrückt werden.

Im restlichen Teil dieser Vorgehensweise konzentrieren wir uns auf die Kontextgrenze „Lieferung“.

## <a name="next-steps"></a>Nächste Schritte

Nach Abschluss einer Domänenanalyse folgt die taktische DDD-Phase, in der Sie Ihre Domänenmodelle genauer definieren.

> [!div class="nextstepaction"]
> [Taktische DDD-Phase](./tactical-ddd.md)
