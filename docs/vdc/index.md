---
title: Virtuelles Azure-Rechenzentrum
description: Ressourcen für das virtuelle Rechenzentrum in Microsoft Azure
keywords: Azure
layout: LandingPage
ms.topic: landing-page
ms.service: architecture-center
ms.subservice: enterprise-cloud-adoption
ms.openlocfilehash: f49640f4a994bf1cdca8029260fc95a5da323ff2
ms.sourcegitcommit: 579c39ff4b776704ead17a006bf24cd4cdc65edd
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/17/2019
ms.locfileid: "59640616"
---
# <a name="azure-virtual-datacenter-and-the-enterprise-control-plane"></a>Virtuelles Azure-Rechenzentrum und die Steuerungsebene für Unternehmen

Mit Azure Virtual Datacenter können Sie die Funktionen der Azure-Cloudplattform optimal nutzen und gleichzeitig Ihren bereits vorhandenen Sicherheits- und Netzwerkrichtlinien gerecht werden. Bei der Bereitstellung von Unternehmensworkloads in der Cloud müssen IT-Organisationen und Unternehmenseinheiten die richtige Balance zwischen Governance und Entwickleragilität finden. Mit den Modellen des virtuellen Azure-Rechenzentrums können Sie diese Balance erreichen, wobei die Governance im Vordergrund steht.

## <a name="resources"></a>Ressourcen

<table>
<tr>
    <td style="width: 64px; vertical-align: middle;"><a href="https://aka.ms/VDC/Concepts"><img src="../_images/virtual-datacenter.svg" alt="Virtual Datacenter eBook" /></a></td>
    <td>
        <h3><a href="https://aka.ms/VDC/Concepts">Virtuelles Azure-Rechenzentrum: Konzepte</a></h3>
        <p>In diesem E-Book erfahren Sie, wie Sie Unternehmensworkloads unter Berücksichtigung Ihrer bereits vorhandenen Sicherheits- und Netzwerkrichtlinien für die Azure-Cloudplattform bereitstellen.</p>
    </td>
</tr>
<tr>
    <td style="width: 64px; vertical-align: middle;"><a href="/azure/networking/networking-virtual-datacenter"><img src="./images/vdc-network.png" alt="Network Perspective" /></a></td>
    <td>
        <h3><a href="networking-virtual-datacenter.md">Virtuelles Azure-Rechenzentrum: Eine Netzwerkperspektive</a></h3>
        <p>Dieser Onlineartikel bietet einen Überblick über Netzwerkmuster und Designs, mit denen Kunden die architektonischen Herausforderungen hinsichtlich der Skalierung, Leistung und Sicherheit meistern können, die bei einer Migration in die Cloud auf sie zukommen.</p>
    </td>
</tr>
<tr>
    <td style="width: 64px; vertical-align: middle;"><a href="https://aka.ms/VDC/Lift"><img src="./images/vdc-lift-and-shift.png" alt="Lift and Shift Guide" /></a></td>
    <td>
        <h3><a href="https://aka.ms/VDC/Lift">Virtuelles Azure-Rechenzentrum: Lift and Shift-Leitfaden </a></h3>
        <p>In diesem Whitepaper erfahren Sie, wie IT-Mitarbeiter und Entscheidungsträger in Unternehmen die Lift and Shift-Migration von Anwendungen und Servern zu Azure vorbereiten und planen sowie zusätzliche Entwicklungskosten beim Optimieren von Cloudhostingoptionen minimieren können.</p>
    </td>
</tr>
<tr>
    <td style="width: 64px; vertical-align: middle;"><a href="https://aka.ms/VDC/Deck"><img src="./images/vdc-deck.png" alt="Presentation Deck" /></a></td>
    <td>
        <h3><a href="https://aka.ms/VDC/Deck">Virtuelles Azure-Rechenzentrum: Präsentation </a></h3>
        <p>Diese Präsentation behandelt den Leitfaden und die Tools für das virtuelle Azure-Rechenzentrum (Virtual Datacenter, VDC). Sie erläutert, was Kunden antreibt, geht auf VDC-Ziele, Azure-Regionen, die Elemente einer VDC-Automatisierung sowie auf industrialisierte und vertrauenswürdige Azure VDCs ein und endet mit einem Aktionsplan für CIOs. Außerdem werden Support- und Schulungsinformationen bereitgestellt.</p>
    </td>
</tr>
</table>

## <a name="what-is-the-azure-virtual-datacenter"></a>Was ist das virtuelle Azure-Rechenzentrum?

Um Workloads in der Cloud bereitstellen zu können, müssen Sie der Cloud das gleiche Vertrauen entgegenbringen wie Ihren eigenen Datencentern. Das erste Modell des Leitfadens für das virtuelle Azure-Rechenzentrum fungiert hierbei gewissermaßen als Brücke und verwendet einen geschlossenen Ansatz für virtuelle Infrastrukturen. Dieser Ansatz ist nicht für jeden geeignet. Er wurde speziell konzipiert, um IT-Gruppen von Unternehmen bei der Erweiterung ihrer lokalen Infrastruktur in die öffentliche Azure-Cloud zu unterstützen. Wir nennen diesen Ansatz das „vertrauenswürdige Datencenter-Erweiterungsmodell“. Im Laufe der Zeit werden weitere Modelle angeboten – auch solche mit sicherem Internetzugriff direkt über ein virtuelles Datencenter.

<img src="./images/vdc-components.svg" alt="Virtual Datacenter components" style="max-width:700px;"/>

Das virtuelle Azure-Rechenzentrum basiert auf vier Komponenten: Identität, Verschlüsselung, softwaredefiniertes Netzwerk und Compliance (einschließlich Protokollen und Berichten).

Im Modell des virtuellen Azure-Rechenzentrums können Sie Isolationsrichtlinien anwenden, die Cloud ähnlich wie ein vertrautes physisches Datencenter gestalten und das nötige Maß an Sicherheit und Vertrauen erreichen. Dies wird durch vier Komponenten ermöglicht, die jedem IT-Team eines Unternehmens bekannt sein dürften: softwaredefiniertes Netzwerk, Verschlüsselung, Identitätsverwaltung und die der Azure-Plattform zugrunde liegenden Compliancestandards und -zertifizierungen. Diese vier Komponenten machen ein virtuelles Datencenter zu einer vertrauenswürdigen Erweiterung Ihrer bestehenden Infrastruktur.

Weitere Informationen erhalten Sie im E-Book <a href="https://aka.ms/VDC/eBook">Azure Virtual Datacenter: Concepts</a> (Virtuelles Azure-Rechenzentrum: Konzepte).
