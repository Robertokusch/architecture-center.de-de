---
title: 'Framework für die Cloudeinführung (Cloud Adoption Framework, CAF): Softwaredefinierte Netzwerke – Cloudnativ'
titleSuffix: Microsoft Cloud Adoption Framework for Azure
ms.service: architecture-center
ms.subservice: enterprise-cloud-adoption
ms.custom: governance
ms.date: 02/11/2019
description: Erläuterung cloudnativer virtueller Netzwerkdienste
author: rotycenh
ms.openlocfilehash: c6200491bc9ba35a9f00e0003e51716b58628980
ms.sourcegitcommit: 273e690c0cfabbc3822089c7d8bc743ef41d2b6e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/08/2019
ms.locfileid: "55900705"
---
# <a name="software-defined-networks-cloud-native"></a>Softwaredefinierte Netzwerke: Cloudnativ

Ein cloudnatives virtuelles Netzwerk ist erforderlich, wenn IaaS-Ressourcen wie virtuelle Computer auf einer Cloudplattform bereitgestellt werden. Der Zugriff auf virtuelle Netzwerke aus mit dem Web vergleichbaren externen Quellen muss explizit bereitgestellt werden. Diese Arten virtueller Netzwerke unterstützen die Erstellung von Subnetzen, Routingregeln sowie von virtuellen Geräten für Firewallfunktionen und Datenverkehrsverwaltung.

Ein cloudnatives virtuelles Netzwerk weist keine Abhängigkeiten von den lokalen oder anderen Nicht-Cloudressourcen Ihrer Organisation auf, um die in der Cloud gehosteten Workloads zu unterstützen. Alle benötigten Ressourcen werden entweder im virtuellen Netzwerk selbst oder über verwaltete PaaS-Angebote bereitgestellt.

## <a name="cloud-native-assumptions"></a>Annahmen für cloudnative virtuelle Netzwerke

Für die Bereitstellung eines cloudnativen virtuellen Netzwerks wird Folgendes angenommen:

- Die Workloads, die Sie im virtuellen Netzwerk bereitstellen, weisen keine Abhängigkeiten von Anwendungen oder Diensten auf, auf die nur innerhalb Ihres lokalen Netzwerks zugegriffen werden kann. Sofern sie keine Endpunkte bereitstellen, auf die über das öffentliche Internet zugegriffen wird, sind Anwendungen und Dienste, die intern lokal gehostet werden, nicht von Ressourcen nutzbar, die auf einer Cloudplattform gehostet werden.
- Die Identitätsverwaltung und Zugriffssteuerung Ihres Workloads hängt von den Identitätsdiensten der Cloudplattform oder IaaS-Servern ab, die in Ihrer Cloudumgebung gehostet werden. Sie müssen sich nicht direkt mit Identitätsdiensten verbinden, die lokal oder an anderen externen Standorten gehostet werden.
- Ihre Identitätsdienste müssen eimaliges Anmelden (SSO) bei lokalen Verzeichnissen nicht unterstützen.

Cloudnative virtuelle Netzwerke weisen keine externen Abhängigkeiten auf. Dadurch sind sie einfach bereitzustellen und zu konfigurieren, und deshalb ist diese Architektur oft die beste Wahl für Experimente oder andere kleinere, in sich geschlossene oder schnell iterierende Bereitstellungen.

Zusätzliche Aspekte, die Ihr für den Umstieg auf die Cloud zuständiges Team berücksichtigen sollte, wenn es um den Einsatz einer cloudnativen virtuellen Netzwerkarchitektur geht, sind u.a.:

- Bestehende Workloads, die für den Betrieb in einem lokalen Rechenzentrum konzipiert sind, müssen möglicherweise umfassend angepasst werden, um in den Genuss cloudbasierter Funktionen, wie z.B. von Speicher- oder Authentifizierungsdiensten, zu kommen.
- Cloudnative Netzwerke werden ausschließlich mithilfe der von der Cloudplattform bereitgestellten Verwaltungstools verwaltet, weshalb es im Laufe der Zeit zu Abweichungen von Ihren bestehenden IT-Standards in Bezug auf Verwaltung und Richtlinien kommen kann.

## <a name="learn-more"></a>Weitere Informationen

Nachstehend finden Sie weitere Informationen zu cloudnativen virtuellen Netzwerken auf der Azure-Plattform.

- [Azure Virtual Network: Anleitungen](/azure/virtual-network/virtual-network-vnet-plan-design-arm). Neu erstellte Azure Virtual Networks sind standardmäßig cloudnativ. Befolgen Sie diese Anleitungen bei der Planung des Entwurfs und der Bereitstellung Ihrer virtuellen Netzwerke.
- [Grenzwerte für Abonnements: Netzwerk](/azure/azure-subscription-service-limits?toc=%2fazure%2fvirtual-network%2ftoc.json#networking-limits). Alle zu einem einzelnen virtuellen Netzwerk gehörenden Ressourcen können nur innerhalb eines Einzelabonnements vorhanden sein und unterliegen Abonnementgrenzen.
