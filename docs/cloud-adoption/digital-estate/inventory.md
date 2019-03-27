---
title: 'Framework für die Cloudeinführung (Cloud Adoption Framework, CAF): Erfassen von Bestandsdaten für digitale Ressourcen'
titleSuffix: Microsoft Cloud Adoption Framework for Azure
description: Informationen zum Erfassen von Bestandsdaten für ein digitales Umfeld.
author: BrianBlanchard
ms.date: 12/10/2018
ms.topic: guide
ms.service: architecture-center
ms.subservice: enterprise-cloud-adoption
ms.openlocfilehash: 0c68ff1e5ff51395698d37fb9b59c7a76c9479b7
ms.sourcegitcommit: c053e6edb429299a0ad9b327888d596c48859d4a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/20/2019
ms.locfileid: "58242551"
---
# <a name="gather-inventory-data-for-a-digital-estate"></a>Erfassen von Bestandsdaten für digitale Ressourcen

Die Erstellung eines Bestandsverzeichnisses ist der erste Schritt bei der [Planung für digitale Ressourcen](overview.md). Hierzu wird eine Liste mit IT-Ressourcen, die bestimmte Geschäftsfunktionen unterstützen, zur späteren Analyse und Rationalisierung erfasst. In diesem Artikel wird davon ausgegangen, dass für die Anforderungsplanung ein Bottom-up-Analyseansatz am besten geeignet ist. Weitere Informationen finden Sie unter [Enterprise Cloud Adoption: Approaches to digital estate planning](./approach.md) (Enterprise Cloud-Einführung: Ansätze für die Planung digitaler Ressourcen).

## <a name="take-inventory-of-a-digital-estate"></a>Erfassen von Bestandsdaten für ein digitales Umfeld

Der Bestand zur Unterstützung eines digitalen Umfelds ist abhängig von der gewünschten digitalen Transformation und dem entsprechenden Transformationsprozess.

- **Betriebliche Transformation**. Im Zuge einer betrieblichen Transformation empfiehlt es sich häufig, die Bestandsermittlung mithilfe von Überprüfungstools durchzuführen, die eine zentrale Liste aller virtuellen Computer und Server erstellen können. Einige Tools können auch Netzwerkzuordnungen und Abhängigkeiten erstellen, was beim Definieren des Workloadabgleichs hilfreich ist.

- **Inkrementelle Transformation**. Die Bestandsermittlung für eine inkrementelle Transformation beginnt beim Kunden. Dabei hat es sich bewährt, zunächst die gesamte Benutzererfahrung zu erfassen. Durch den Abgleich dieser Erfassung mit Anwendungen, APIs, Daten und anderen Ressourcen entsteht ein detaillierter Bestand für die Analyse.

- **Transformation zur Chancenverbesserung**. Bei einer Transformation zur Chancenverbesserung steht das Produkt oder der Dienst im Mittelpunkt. Eine entsprechende Bestandserfassung beinhaltet eine Erfassung der Möglichkeiten, die Chancen auf dem Markt zu verbessern, sowie der dazu erforderlichen Funktionen.

## <a name="accuracy-and-completeness-of-an-inventory"></a>Genauigkeit und Vollständigkeit eines Bestands

Eine Bestandsaufnahme ist selten im ersten Anlauf vollständig. Daher sollten verschiedene Mitglieder des Cloudstrategieteams bei der Überprüfung des Bestands mit Projektbeteiligten und Hauptbenutzern zusammenarbeiten. Gegebenenfalls können mithilfe zusätzlicher Tools wie etwa einer Netzwerk- und Abhängigkeitsanalyse Ressourcen ermittelt werden, an die Datenverkehr gesendet wird, die aber nicht im Bestand erfasst wurden.

## <a name="next-steps"></a>Nächste Schritte

Nach Abschluss und Überprüfung der Bestandsaufnahme kann der Bestand rationalisiert werden. Die Rationalisierung des Bestands ist der nächste Schritt bei der Planung für digitale Ressourcen.

> [!div class="nextstepaction"]
> [Rationalisieren der digitalen Ressourcen](rationalize.md)