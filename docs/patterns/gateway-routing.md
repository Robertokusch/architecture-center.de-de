---
title: Muster „Gatewayrouting“
titleSuffix: Cloud Design Patterns
description: Leiten Sie Anforderungen an mehrere Dienste mit einem einzelnen Endpunkt weiter.
keywords: Entwurfsmuster
author: dragon119
ms.date: 06/23/2017
ms.topic: design-pattern
ms.service: architecture-center
ms.subservice: cloud-fundamentals
ms.custom: seodec18
ms.openlocfilehash: e5c93c98a562e790d547d08fdf312c973cfceed8
ms.sourcegitcommit: 1b50810208354577b00e89e5c031b774b02736e2
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/23/2019
ms.locfileid: "54487227"
---
# <a name="gateway-routing-pattern"></a>Muster „Gatewayrouting“

Leiten Sie Anforderungen an mehrere Dienste mit einem einzelnen Endpunkt weiter. Dieses Muster ist hilfreich, wenn Sie mehrere Dienste auf einem einzigen Endpunkt verfügbar machen möchten und die Weiterleitung an den geeigneten Dienst basierend auf der Anforderung erfolgen soll.

## <a name="context-and-problem"></a>Kontext und Problem

Wenn ein Client mehrere Dienste nutzen muss, kann es schwierig sein, einen separaten Endpunkt für jeden Dienst einzurichten und dafür zu sorgen, dass der Client jeden Endpunkt verwaltet. Eine E-Commerce-Anwendung stellt beispielsweise Dienste für Folgendes bereit: Suche, Reviews, Warenkorb, Auftragsabschluss und Auftragsverlauf. Jeder Dienst verwendet eine andere API, mit der der Client interagieren muss, und der Client muss jeden Endpunkt kennen, um eine Verbindung mit den Diensten herstellen zu können. Wenn sich eine API ändert, muss auch der Client aktualisiert werden. Wenn Sie einen Dienst in zwei oder mehr separate Dienste umgestalten, muss der Code sowohl im Dienst als auch im Client geändert werden.

## <a name="solution"></a>Lösung

Platzieren Sie ein Gateway vor einer Reihe von Anwendungen, Diensten oder Bereitstellungen. Implementieren Sie ein Layer-7-Routing für die Anwendung, um Anforderungen an die geeigneten Instanzen weiterzuleiten.

Bei diesem Muster muss die Clientanwendung nur einen einzigen Endpunkt kennen und mit diesem kommunizieren. Wenn ein Dienst konsolidiert oder aufgeteilt wird, muss der Client nicht unbedingt aktualisiert werden. Der Client kann weiterhin Anforderungen an das Gateway senden, es ändert sich nur die Weiterleitung.

Mit einem Gateway können Sie auch die Back-End-Dienste von den Clients abstrahieren, sodass Clientaufrufe einfach bleiben, aber Änderungen in den Back-End-Diensten hinter dem Gateway möglich sind. Clientaufrufe können an jeden Dienst weitergeleitet werden, der das erwartete Clientverhalten verarbeiten muss. So können Sie Dienste hinter dem Gateway hinzufügen, teilen und neu anordnen, ohne den Client ändern zu müssen.

![Diagramm des Musters „Gatewayrouting“](./_images/gateway-routing.png)

Dieses Muster kann auch bei der Bereitstellung helfen, da Sie bestimmen können, wie Updates für die Benutzer bereitgestellt werden. Wenn eine neue Version Ihres Diensts bereitgestellt wird, kann diese Bereitstellung parallel zur vorhandenen Version erfolgen. Durch Routing können Sie steuern, welche Version des Diensts den Clients präsentiert wird. So können Sie flexibel verschiedene Releasestrategien für Updates nutzen – als inkrementelle, parallele oder vollständige Rollouts. Sollten nach der Bereitstellung eines neuen Diensts Probleme auftreten, können Sie einfach eine Konfigurationsänderung auf dem Gateway vornehmen, ohne dass Clients beeinträchtigt werden.

## <a name="issues-and-considerations"></a>Probleme und Überlegungen

- Der Gatewaydienst kann ein Single Point of Failure darstellen. Stellen Sie sicher, dass es auf Ihre Verfügbarkeitsanforderungen ausgelegt ist. Berücksichtigen Sie bei der Implementierung die Aspekte Resilienz und Fehlertoleranz.
- Der Gatewaydienst kann einen Engpass darstellen. Stellen Sie sicher, dass das Gateway leistungsfähig genug ist, um die Last zu verarbeiten, und dass es sich problemlos gemäß Ihren Wachstumserwartungen skalieren lässt.
- Führen Sie einen Auslastungstest für das Gateway durch, um sicherzustellen, dass keine kaskadierenden Ausfälle bei den Diensten auftreten.
- Das Gatewayrouting erfolgt auf Ebene 7. Es kann auf IP, Port, Header oder URL basieren.

## <a name="when-to-use-this-pattern"></a>Verwendung dieses Musters

Verwenden Sie dieses Muster in folgenden Fällen:

- Ein Client muss mehrere Dienste nutzen, auf die hinter einem Gateway zugegriffen werden kann.
- Sie möchten Clientanwendungen durch Nutzung eines einzigen Endpunkts vereinfachen.
- Sie müssen Anforderungen von extern adressierbaren Endpunkten an interne virtuelle Endpunkte weiterleiten – beispielsweise müssen Sie Ports auf einem virtuellen Computer verfügbar machen, um virtuelle IP-Adressen zu gruppieren.

Dieses Muster eignet sich wahrscheinlich nicht, wenn Sie über eine einfache Anwendung verfügen, die nur einen oder zwei Dienste nutzt.

## <a name="example"></a>Beispiel

Der folgende Code ist eine einfache Beispielkonfigurationsdatei für einen Server, der Anforderungen für Anwendungen, die sich in verschiedenen virtuellen Verzeichnissen befinden, an verschiedene Computer im Back-End weiterleitet. Als Router wird nginx verwendet.

```console
server {
    listen 80;
    server_name domain.com;

    location /app1 {
        proxy_pass http://10.0.3.10:80;
    }

    location /app2 {
        proxy_pass http://10.0.3.20:80;
    }

    location /app3 {
        proxy_pass http://10.0.3.30:80;
    }
}
```

## <a name="related-guidance"></a>Verwandte Leitfäden

- [Muster „Back-Ends für Front-Ends“](./backends-for-frontends.md)
- [Gatewayaggregationsmuster](./gateway-aggregation.md)
- [Muster „Gatewayabladung“](./gateway-offloading.md)
