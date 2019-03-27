---
title: 'Framework für die Cloudeinführung (Cloud Adoption Framework, CAF): Softwaredefinierte Netzwerke – Cloud-DMZ'
titleSuffix: Microsoft Cloud Adoption Framework for Azure
ms.service: architecture-center
ms.subservice: enterprise-cloud-adoption
ms.custom: governance
ms.date: 02/11/2019
description: Diese Netzwerkarchitektur ermöglicht einen eingeschränkten Zugriff zwischen Ihren lokalen und cloudbasierten Netzwerken.
author: rotycenh
ms.openlocfilehash: a192541dcfb0f3d713f4139a2ab0541d0c7202db
ms.sourcegitcommit: c053e6edb429299a0ad9b327888d596c48859d4a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/20/2019
ms.locfileid: "58242811"
---
# <a name="software-defined-networks-cloud-dmz"></a>Softwaredefinierte Netzwerke: Cloud-DMZ

Die Netzwerkarchitektur von Cloud-DMZ ermöglicht einen eingeschränkten Zugriff zwischen Ihren lokalen und cloudbasierten Netzwerken, indem ein virtuelles privates Netzwerk (VPN) zur Verbindung der Netzwerke verwendet wird. Eine DMZ wird in der Cloud bereitgestellt, um den Zugriff auf das lokale Netzwerk aus cloudbasierten Ressourcen zu schützen.

![Sichere Hybrid-Netzwerkarchitektur](../../../reference-architectures/dmz/images/dmz-private.png)

Diese Architektur ist so ausgelegt, dass Szenarien unterstützt werden, in denen Ihr Unternehmen mit der Integration von cloudbasierten Workloads in lokale Workloads beginnen möchte, aber möglicherweise noch keine vollständig ausgereiften Sicherheitsrichtlinien für die Cloud hat oder keine sichere dedizierte WAN-Verbindung zwischen den beiden Umgebungen hergestellt wurde. Daher sollten Cloudnetzwerke wie eine DMZ behandelt werden, um zu gewährleisten, dass lokale Dienste sicher sind.

Die DMZ stellt virtuelle Netzwerkappliances (NVAs) bereit, die Sicherheitsfunktionen wie Firewalls und Paketüberprüfung implementieren. Datenverkehr zwischen lokalen und cloudbasierten Anwendungen oder Diensten muss die DMZ passieren, wo er überprüft werden kann. VPN-Verbindungen und die Regeln, die festlegen, welcher Datenverkehr durch das DMZ-Netzwerk geleitet werden darf, werden von IT-Sicherheitsteams streng kontrolliert.

## <a name="cloud-dmz-assumptions"></a>Annahmen für Cloud-DMZ

Für die Bereitstellung von Cloud-DMZ wird Folgendes angenommen:

- Ihre Sicherheitsteams haben die lokalen und cloudbasierten Sicherheitsanforderungen und -richtlinien noch nicht vollständig aufeinander abgestimmt.
- Ihre cloudbasierten Workloads erfordern einen eingeschränkten Zugriff auf Dienste, die in Ihren lokalen oder Drittanbieternetzwerken gehostet werden, oder Ihre Benutzer oder Anwendungen in Ihren lokalen Umgebungen benötigen eingeschränkten Zugriff auf in der Cloud gehostete Ressourcen.
- Die Implementierung eines VPN zwischen Ihren lokalen Netzwerken und dem Cloudanbieter wird nicht durch unternehmensinterne Richtlinien, regulatorische Anforderungen oder technische Kompatibilitätsprobleme erschwert.
- Ihre Workloads erfordern entweder nicht mehrere Abonnements, um die Ressourcenbeschränkungen für Abonnements zu umgehen, oder sie umfassen mehrere Abonnements, erfordern aber keine zentrale Verwaltung der Konnektivität oder gemeinsamen Dienste, die von auf mehrere Abonnements verteilten Ressourcen genutzt werden.

Ihr für den Umstieg auf die Cloud zuständiges Team muss die folgenden Aspekte berücksichtigen, wenn die Implementierung einer virtuellen Netzwerkarchitektur mit Cloud-DMZ erwogen wird:

- Durch die Verbindung von lokalen Netzwerken mit Cloudnetzwerken steigt die Komplexität Ihrer Sicherheitsanforderungen. Auch wenn die Verbindung zwischen Cloudnetzwerken und der lokalen Umgebung geschützt ist, müssen Sie dennoch dafür sorgen, dass Cloudressourcen geschützt sind.
- Die Cloud-DMZ-Architektur wird häufig als Ausgangspunkt verwendet, während die Konnektivität weiter abgesichert und die Sicherheitsrichtlinien zwischen lokalen und Cloudnetzwerken abgestimmt werden, was eine breitere Einführung einer vollständig hybriden Netzwerkarchitektur ermöglicht.

## <a name="learn-more"></a>Weitere Informationen

Nachstehend finden Sie weitere Informationen zur Cloud-DMZ-Implementierung auf der Azure-Plattform.

- [Implementieren einer DMZ zwischen Azure und Ihrem lokalen Rechenzentrum](../../../reference-architectures/dmz/secure-vnet-hybrid.md). In diesem Artikel wird das Implementieren einer sicheren hybriden Netzwerkarchitektur in Azure erörtert.
