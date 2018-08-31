---
title: Entwurfsprinzipien für Azure-Anwendungen
description: Entwurfsprinzipien für Azure-Anwendungen
author: MikeWasson
ms.date: 08/30/2018
ms.openlocfilehash: 5dd5d02019723ce57ba377d99b3965d0d7ed4079
ms.sourcegitcommit: ae8a1de6f4af7a89a66a8339879843d945201f85
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/31/2018
ms.locfileid: "43326073"
---
# <a name="ten-design-principles-for-azure-applications"></a>Zehn Entwurfsprinzipien für Azure-Anwendungen

Befolgen Sie die folgenden Entwurfsprinzipien, um die Skalierbarkeit, Resilienz und Verwaltbarkeit Ihrer Anwendung zu optimieren. 

**[Entwurf mit Blick auf Selbstreparatur](self-healing.md)**: In einem verteilten System kann es zu Ausfällen kommen. Entwerfen Sie Ihre Anwendung so, dass sie bei Ausfällen eine Selbstreparatur durchführt.

**[Herstellen von Redundanz für alle Anwendungskomponenten](redundancy.md)**: Schaffen Sie Redundanz in Ihrer Anwendung, um Ausfälle einzelner Komponenten zu verhindern.
 
**[Minimieren der Koordinierung](minimize-coordination.md)**: Minimieren Sie die Koordinierung zwischen Anwendungsdiensten, um Skalierbarkeit zu erzielen.
 
**[Ausrichtung des Entwurfs auf horizontale Skalierung](scale-out.md)**: Entwerfen Sie Ihre Anwendung so, dass sie durch Hinzufügen oder Entfernen neuer Instanzen je nach Bedarf horizontal skaliert werden kann.

**[Umgehung von Beschränkungen durch Partitionierung](partition.md)**: Setzen Sie Partitionierung ein, um Datenbank-, Netzwerk- und Computebeschränkungen zu umgehen.

**[Entwurf mit Blick auf den Betrieb](design-for-operations.md)**: Setzen Sie es sich beim Entwurf Ihre Anwendung als Ziel, dem Betriebsteam die benötigten Verwaltungstools zur Verfügung zu stellen.

**[Verwendung verwalteter Dienste](managed-services.md)**: Verwenden Sie nach Möglichkeit Platform as a Service (PaaS) statt Infrastructure-as-a-Service (IaaS).

**[Verwendung des idealen Datenspeichers](use-the-best-data-store.md)**: Wählen Sie die Speichertechnologie, die am besten für Ihre Daten und den vorgesehenen Einsatzzweck geeignet ist. 
 
**[Entwurf mit Blick auf die Entwicklung](design-for-evolution.md)**: Erfolgreiche Anwendungen ändern sich im Laufe der Zeit. Ein evolutionärer Entwurf ist für einen kontinuierlichen Innovationsstrom von entscheidender Bedeutung.

**[Ausrichtung des Entwurfs auf die Unternehmensanforderungen](build-for-business.md)**: Jede Entwurfsentscheidung muss durch eine geschäftliche Anforderung gerechtfertigt sein.

