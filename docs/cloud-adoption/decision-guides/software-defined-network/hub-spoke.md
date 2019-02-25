---
title: 'Framework für die Cloudeinführung (Cloud Adoption Framework, CAF): Softwaredefinierte Netzwerke – Cloudnativ'
titleSuffix: Microsoft Cloud Adoption Framework for Azure
ms.service: architecture-center
ms.subservice: enterprise-cloud-adoption
ms.custom: governance
ms.date: 02/11/2019
description: Erläuterung cloudnativer virtueller Netzwerkdienste
author: rotycenh
ms.openlocfilehash: e0ad6803f2ddc982ea0c42c59fdf2486e1710433
ms.sourcegitcommit: 273e690c0cfabbc3822089c7d8bc743ef41d2b6e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/08/2019
ms.locfileid: "55901553"
---
# <a name="software-defined-networks-hub-and-spoke"></a>Softwaredefinierte Netzwerke: Hub-and-Spoke-Modell

Im Hub-and-Spoke-Netzwerkmodell wird Ihre auf Azure basierende Cloud-Netzwerkinfrastruktur in mehreren verbundenen virtuellen Netzwerken organisiert. Mithilfe dieses Modells können Sie gängige Kommunikations- oder Sicherheitsanforderungen effizienter bewältigen und mögliche Einschränkungen des Abonnements berücksichtigen.

Im Hub-and-Spoke-Modell ist der *Hub* ein virtuelles Netzwerk, das als Zentrale für die Verwaltung externer Konnektivitäts- und Hostingdienste dient, die von mehreren Workloads genutzt werden. Die *Spokes* sind virtuelle Netzwerke, die Workloads hosten und sich über [Peering virtueller Netzwerke](/virtual-network/virtual-network-peering-overview) mit dem zentralen Hub verbinden.

Der gesamte Datenverkehr, der in die oder aus den Spoke-Netzwerken der Workload geleitet wird, wird über das Hub-Netzwerk geleitet, wo er weitergeleitet, inspiziert oder anderweitig von zentral verwalteten IT-Regeln oder -Prozessen verwaltet werden kann.

Dieses Modell zielt darauf ab, die folgenden Probleme zu lösen:

- Kosteneinsparungen und Effizienz der Verwaltung. Zentralisierung von Diensten, die von mehreren Workloads, z.B. Network Virtual Appliances (NVAs) und DNS-Servern an einem zentralen Standort gemeinsam genutzt werden können. Auf diese Weise kann die IT-Abteilung redundante Ressourcen und den Verwaltungsaufwand für mehrere Workloads minimieren.
- Umgehen von Abonnementgrenzen. Große cloudbasierte Workloads können den Einsatz von mehr Ressourcen erfordern, als innerhalb eines einzelnen Azure-Abonnements zulässig ist (siehe [Abonnementgrenzen](/azure/azure-subscription-service-limits)). Durch ein Peering virtueller Netzwerke für eine Workload aus verschiedenen Abonnements zu einem zentralen Hub können diese Grenzwerte umgangen werden.
- Trennung von Zuständigkeiten. Die Fähigkeit, einzelne Workloads zwischen zentralen IT-Teams und für Workloads zuständige Teams bereitzustellen.

Das folgende Diagramm zeigt eine exemplarische Hub-and-Spoke-Architektur mit zentral verwalteter Hybridkonnektivität.

![Hub-and-Spoke-Netzwerkarchitektur](../../../reference-architectures/hybrid-networking/images/hub-spoke.png)

Die Hub-and-Spoke-Architektur wird häufig zusammen mit der hybriden Netzwerkarchitektur verwendet und bietet eine zentral verwaltete Verbindung mit Ihrer lokalen Umgebung, die von mehreren Workloads gemeinsam genutzt wird. In diesem Szenario durchläuft der gesamte Datenverkehr zwischen den Workloads und den lokalen Standorten den Hub, wo er verwaltet und geschützt werden kann.

## <a name="hub-and-spoke-assumptions"></a>Annahmen für das Hub-and-Spoke-Modell

Für die Implementieren einer virtuellen Netzwerkarchitektur gemäß Hub-and-Spoke-Modell wird Folgendes angenommen:

- Ihre Cloudbereitstellungen umfassen Workloads, die in separaten Arbeitsumgebungen wie Entwicklung, Test und Produktion gehostet werden, und die alle auf einer Reihe von gemeinsamen Diensten wie DNS oder Verzeichnisdiensten basieren.
- Ihre Workloads müssen nicht miteinander kommunizieren, sondern haben gemeinsame Anforderungen an die externe Kommunikation und gemeinsam genutzte Dienste.
- Ihre Workloads erfordern mehr Ressourcen, als innerhalb eines einzelnen Azure-Abonnements verfügbar sind.
- Sie müssen für Workloads zuständigen Teams delegierte Verwaltungsrechte für ihre eigenen Ressourcen zur Verfügung stellen und gleichzeitig die zentrale Sicherheitskontrolle über die externe Konnektivität beibehalten.

## <a name="global-hub-and-spoke"></a>Globales Hub-and-Spoke-Modell

Hub-and-Spoke-Architekturen werden häufig mit virtuellen Netzwerken implementiert, die in derselben Azure-Region bereitgestellt werden, um die Wartezeiten zwischen Netzwerken zu minimieren. Große Unternehmen mit globaler Reichweite müssen jedoch möglicherweise Workloads in mehreren Regionen bereitstellen, um Verfügbarkeit, Notfallwiederherstellung oder die Erfüllung gesetzlicher Anforderungen zu gewährleisten. Durch den Einsatz eines [globalen Peerings virtueller Netzwerke](/azure/virtual-network/virtual-network-peering-overview) in Azure kann das Hub-and-Spoke-Modell die zentralisierte Verwaltung und die gemeinsamen Dienste regionsübergreifend erweitern, um die über die ganze Welt verteilten Workloads zu unterstützen.

## <a name="learn-more"></a>Weitere Informationen

Beispiele der Implementierung von Hub-and-Spoke-Netzwerken in Azure finden Sie in den folgenden Beispielen auf der Website zu Azure Referenzarchitekturen:

- [Implementieren einer Hub-Spoke-Netzwerktopologie in Azure](../../../reference-architectures/hybrid-networking/hub-spoke.md)
- [Implementieren einer Hub-Spoke-Netzwerktopologie mit gemeinsamen Diensten in Azure](../../../reference-architectures/hybrid-networking/shared-services.md)
