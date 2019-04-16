---
title: 'CAF: Wie funktioniert Azure?'
titleSuffix: Microsoft Cloud Adoption Framework for Azure
ms.service: architecture-center
ms.subservice: enterprise-cloud-adoption
description: Enthält eine Beschreibung der internen Funktionsweise von Azure.
author: petertaylor9999
ms.date: 02/11/2019
ms.openlocfilehash: 215e4e4954a2f3e722e3ac865fea19508f6edadd
ms.sourcegitcommit: 0a8a60d782facc294f7f78ec0e9033e3ee16bf4a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/08/2019
ms.locfileid: "59068819"
---
<!-- markdownlint-disable MD026 -->

# <a name="how-does-azure-work"></a>Wie funktioniert Azure?

Azure ist die öffentliche Cloudplattform von Microsoft. Azure umfasst eine große Sammlung von Diensten, z.B. Platform-as-a-Service (PaaS), Infrastructure-as-a-Service (IaaS) und Database-as-a-Service (DBaaS). Aber was ist Azure genau, und wie funktioniert es?

<!-- markdownlint-disable MD034 -->

> [!VIDEO https://www.microsoft.com/en-us/videoplayer/embed/RE2ixGo]

<!-- markdownlint-enable MD034 -->

Wie andere Cloudplattformen auch, basiert Azure auf einer Technologie, die als **Virtualisierung** bezeichnet wird. Ein Großteil der Computerhardware kann per Software emuliert werden, da es sich bei Computerhardware in den meisten Fällen lediglich um einen Satz mit Anweisungen handelt, die permanent oder semipermanent in Silizium codiert sind. Indem eine Emulationsebene verwendet wird, mit der Softwareanweisungen Hardwareanweisungen zugeordnet werden, kann virtualisierte Hardware so per Software ausgeführt werden, als ob es sich wirklich um Hardware handeln würde.

Im Wesentlichen umfasst die Cloud eine Gruppe von physischen Servern in einem oder mehreren Rechenzentren, auf denen virtualisierte Hardware im Namen der Kunden ausgeführt wird. Wie wird es also für die Cloud erreicht, dass Millionen von Instanzen virtualisierter Hardware für Millionen von Kunden gleichzeitig erstellt, gestartet, beendet und gelöscht werden können?

Wir sehen uns die Architektur der Hardware im Rechenzentrum an.  Jedes Rechenzentrum verfügt über eine Sammlung von Servern, die in Serverracks angeordnet sind. Jedes Serverrack enthält viele Server**blades** sowie einen Netzwerkswitch für die Netzwerkkonnektivität und eine Stromversorgungseinheit für die Stromversorgung. Racks werden auch in größeren Einheiten gruppiert, die als **Cluster** bezeichnet werden.

Innerhalb jedes Racks oder Clusters haben die meisten Server die Aufgabe, diese virtualisierten Hardwareinstanzen im Namen des Benutzers auszuführen. Auf einigen Servern wird jedoch Software für die Cloudverwaltung ausgeführt, die als Fabric Controller bezeichnet wird. Der **Fabric Controller** ist eine verteilte Anwendung mit vielen Aufgaben. Er dient zum Zuordnen von Diensten, Überwachen der Integrität des Servers und der darauf ausgeführten Dienste und Wiederherstellen der Serverintegrität nach einem Ausfall.

Jede Instanz des Fabric Controllers ist mit einem anderen Satz von Servern verbunden, auf denen Software für die Cloudorchestrierung ausgeführt wird, die normalerweise als **Front-End** bezeichnet wird. Auf dem Front-End werden die Webdienste, RESTful-APIs und internen Azure-Datenbanken gehostet, die für alle Funktionen der Cloud verwendet werden.

Beispielsweise hostet das Front-End die Dienste, mit denen Kundenanforderungen zur Zuteilung von Azure-Ressourcen verarbeitet werden. Hierzu zählen beispielsweise [virtuelle Netzwerke](/azure/virtual-network/virtual-networks-overview), [virtuelle Computer](/azure/virtual-machines) (VMs) und Dienste wie [Cosmos DB](/azure/cosmos-db/introduction). Zuerst überprüft das Front-End den Benutzer und stellt sicher, dass der Benutzer zur Zuordnung der angeforderten Ressourcen berechtigt ist. Falls ja, zieht das Front-End eine Datenbank heran, um ein Serverrack mit ausreichender Kapazität zu ermitteln. Anschließend weist es den Fabric Controller im Rack an, die Ressource zuzuteilen.

Azure ist also im Wesentlichen eine riesige Sammlung von Servern und Netzwerkhardware-Komponenten, auf denen ein komplexer Satz verteilter Anwendungen ausgeführt wird, um Konfiguration und Betrieb der virtualisierten Hardware und Software auf diesen Servern zu orchestrieren. Diese Orchestrierung macht Azure so leistungsstark. Benutzer müssen sich nicht mehr um die Wartung und Upgrades von Hardware kümmern, da diese Aufgaben von Azure im Hintergrund durchgeführt werden.

## <a name="next-steps"></a>Nächste Schritte

Nachdem Sie sich mit den internen Zusammenhängen von Azure vertraut gemacht haben, können Sie sich über Cloudressourcenkontrolle informieren.

> [!div class="nextstepaction"]
> [Informationen zur Ressourcenkontrolle](what-is-governance.md)

<!-- Links -->

[docs-add-users-to-aad]: /azure/active-directory/add-users-azure-active-directory?toc=/azure/architecture/cloud-adoption-guide/toc.json
