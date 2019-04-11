---
title: Einführung in Microservices-Architekturen
description: Eine Einführung in Microservices-Architekturen.
author: MikeWasson
ms.date: 02/26/2019
ms.topic: guide
ms.service: architecture-center
ms.subservice: reference-architecture
ms.custom: microservices
ms.openlocfilehash: fbb79ea2f9a4822fb221b99ce5e94ce78067fd1f
ms.sourcegitcommit: c053e6edb429299a0ad9b327888d596c48859d4a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/20/2019
ms.locfileid: "58241501"
---
# <a name="introduction-to-microservices-architectures"></a>Einführung in Microservices-Architekturen

Eine Microservicearchitektur besteht aus einer Sammlung kleiner, autonomer Dienste. Jeder Dienst ist eigenständig und sollte eine einzige Geschäftsfunktion implementieren. Im Anschluss finden Sie einige definierende Merkmale von Microservices:

- In einer Microservicearchitektur sind die Dienste klein, unabhängig und lose gekoppelt.
- Ein Microservice ist so kompakt, dass er von einem kleinen Entwicklerteam geschrieben und verwaltet werden kann.
- Die Dienste können unabhängig voneinander bereitgestellt werden. Ein Team kann einen vorhandenen Dienst aktualisieren, ohne die gesamte Anwendung neu erstellen und erneut bereitstellen zu müssen.
- Die Dienste sind dafür zuständig, ihre eigenen Daten und ihren externen Zustand beizubehalten. Dies ist ein Unterschied zum herkömmlichen Modell, in dem eine separate Datenschicht die Datenpersistenz verwaltet.
- Dienste kommunizieren über klar definierte APIs miteinander. Die internen Implementierungsdetails jedes Diensts werden für andere Dienste verborgen.
- Dienste müssen nicht den gleichen Technologiestapel, die gleichen Bibliotheken oder die gleichen Frameworks verwenden.

![Logische Microservices-Architektur](../guide/architecture-styles/images/microservices-logical.svg)

<!-- markdownlint-disable MD026 -->

## <a name="why-build-microservices"></a>Was spricht für die Erstellung von Microservices?

Microservices bieten eine ganze Reihe von Vorteilen:

- **Flexibilität:** Die unabhängige Bereitstellung von Microservices vereinfacht die Verwaltung von Fehlerkorrekturen und Featureveröffentlichungen. Sie können einen Dienst aktualisieren, ohne die gesamte Anwendung erneut bereitstellen zu müssen, und im Problemfall einen Rollback für ein Update ausführen. In vielen herkömmlichen Anwendung kann die Entdeckung eines Fehlers in einem Teil der Anwendung den gesamte Releaseprozess blockieren. Dies kann dazu führen, dass sich die Einführung neuer Features verzögert, bis eine Fehlerkorrektur integriert, getestet und veröffentlicht wurde.  

- **Kompakter Code, kleine Teams:** Ein Microservice muss so kompakt sein, dass er von einem einzelnen Featureteam erstellt, getestet und bereitgestellt werden kann. Eine kompakte Codebasis ist leichter nachvollziehbar. In einer umfangreichen monolithischen Anwendung werden Codeabhängigkeiten im Laufe der Zeit häufig unübersichtlich, sodass zum Hinzufügen eines neuen Features Code an zahlreichen Stellen bearbeitet werden muss. Da sich Microservices keinen Code und keine Datenspeicher teilen, enthält eine Microservices-Architektur weniger Abhängigkeiten und vereinfacht dadurch das Hinzufügen neuer Features. Kleinere Teams sind außerdem agiler. Eine Regel besagt, dass ein Team zu groß ist, wenn zwei Pizzen nicht ausreichen, um das gesamte Team zu versorgen. Das ist natürlich keine exakte Metrik und hängt vom Appetit der Teammitglieder ab. Entscheidend ist jedoch, dass große Gruppen in der Regel weniger produktiv sind, da sich die Kommunikation verlangsamt, der Verwaltungsaufwand steigt und die Agilität abnimmt.  

- **Kombination verschiedener Technologien:** Teams können sich für die Technologie entscheiden, die am besten zu ihrem Dienst passt, und eine Kombination aus geeigneten Technologiestapeln verwenden. 

- **Resilienz:** Der Ausfall eines einzelnen Microservice hat nicht den Ausfall der gesamten Anwendung zur Folge – vorausgesetzt, die Upstream-Microservices sind für eine korrekte Behandlung von Ausfällen konzipiert (beispielsweise durch Implementierung des Trennschalter-Musters).

- **Skalierbarkeit**. In einer Microservices-Architektur kann jeder Microservice unabhängig skaliert werden. Dadurch lassen sich Subsysteme, die mehr Ressourcen benötigen, horizontal hochskalieren, ohne die gesamte Anwendung hochzuskalieren. Wenn Sie Dienste in Containern bereitstellen, können Sie Microservices zudem dichter auf einem einzelnen Host platzieren und Ressourcen effizienter nutzen.

- **Datenisolation:** Schemaaktualisierungen sind wesentlich einfacher, da nur ein einzelner Microservice betroffen ist. Bei einer monolithischen Anwendung können sich Schemaaktualisierungen als echte Herausforderung erweisen, da verschiedene Teile der Anwendung unter Umständen die gleichen Daten nutzen, was jegliche Anpassung des Schemas zu einer riskanten Angelegenheit macht.

## <a name="when-should-i-build-microservices"></a>Wann soll ich Microservices erstellen?

Eine Microservices-Architektur eignet sich für Folgendes:

- Große Anwendungen mit hoher Releaserate
- Komplexe Anwendungen, die ein hohes Maß an Skalierbarkeit aufweisen müssen
- Anwendungen mit umfangreichen Domänen oder einer Vielzahl von Unterdomänen
- Eine Organisation, die aus vielen kleinen Entwicklungsteams besteht

## <a name="challenges"></a>Herausforderungen

Die Vorteile von Microservices gibt es jedoch nicht umsonst. Im Anschluss finden Sie einige der Herausforderungen, die Sie vor der Erstellung einer Microservices-Architektur berücksichtigen sollten.

- **Komplexität**. Eine Microserviceanwendung verfügt über mehr bewegliche Teile als die entsprechende monolithische Anwendung. Jeder Dienst für sich genommen ist einfacher, aber das System als Ganzes ist komplexer.

- **Entwicklung und Tests**. Das Schreiben eines kleinen Diensts, der von anderen abhängigen Diensten abhängig ist, erfordert eine andere Herangehensweise als das Schreiben einer herkömmlichen monolithischen oder mehrschichtigen Anwendung. Vorhandene Tools sind nicht immer für die Arbeit mit Dienstabhängigkeiten geeignet. Eine Umgestaltung über Dienstgrenzen hinweg kann schwierig sein. Das Testen von Dienstabhängigkeiten kann ebenfalls eine Herausforderung darstellen, insbesondere dann, wenn sich die Anwendung schnell weiterentwickelt.

- **Unzureichende Governance**. Der dezentralisierte Ansatz für die Erstellung von Microservices bietet einige Vorteile, kann jedoch auch zu Problemen führen. Es kann passieren, dass so viele verschiedene Sprachen und Frameworks verwendet werden, dass die Verwaltung der Anwendung schwierig wird. Es empfiehlt sich möglicherweise, einige für das gesamte Projekt gültige Standards zu etablieren, ohne die Flexibilität der Teams zu sehr einzuschränken. Dies gilt insbesondere für übergreifende Funktionen wie die Protokollierung.

- **Netzwerkkonflikte und -latenz**. Die Verwendung vieler kleiner, genau abgestimmter Dienste kann zu einer vermehrten Kommunikation zwischen den Diensten führen. Wenn die Kette der Dienstabhängigkeiten zu lang wird (Dienst A ruft B auf, der wiederum C aufruft...), kann die zusätzliche Latenz zu einem Problem werden. Beim Entwurf der APIs sollten Sie größte Umsicht walten lassen. Vermeiden Sie überladene APIs, denken Sie über Serialisierungsformate nach, und suchen Sie nach Stellen, an denen Sie asynchrone Kommunikationsmuster verwenden können.

- **Datenintegrität**. Jeder Microservice ist für die eigene Datenpersistenz zuständig. Dadurch kann die Sicherstellung der Datenkonsistenz zu einer Herausforderung werden. Implementieren Sie die letztliche Konsistenz, wo immer möglich.

- **Verwaltung**. Für den erfolgreichen Einsatz von Microservices ist eine ausgereifte DevOps-Kultur erforderlich. Die korrelierte Protokollierung über Dienste hinweg kann eine Herausforderung sein. In der Regel muss die Protokollierung mehrere Dienstaufrufe für einen einzigen Benutzervorgang korrelieren.

- **Versionsverwaltung**. Dienstupdates dürfen nicht zu Unterbrechungen bei abhängigen Diensten führen. Dienste können jederzeit aktualisiert werden, daher könnten ohne sorgfältigen Entwurf Probleme mit der Abwärts- oder Aufwärtskompatibilität entstehen.

- **Kompetenz**. Microservices sind Systeme mit einem hohen Maß an Verteilung. Beurteilen Sie sehr genau, ob das Team über die notwendige Kompetenz und Erfahrung verfügt, erfolgreiche Arbeit zu leisten.

## <a name="next-steps"></a>Nächste Schritte

- Eine der größten Herausforderungen im Zusammenhang mit Microservices ist die Erstellung geeigneter Dienstgrenzen. In den Artikeln unter [Verwenden der Domänenanalyse zur Modellierung von Microservices](./model/domain-analysis.md) wird ein domänenbasierter Ansatz für dieses Problem vorgestellt.
