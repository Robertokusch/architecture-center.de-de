---
title: SAP für Dev/Test-Workloads
description: SAP-Szenario für eine Dev/Test-Umgebung
author: AndrewDibbins
ms.date: 7/11/18
ms.openlocfilehash: 675a5cb4b1ee4001ca50d24c145ce1a177f90da4
ms.sourcegitcommit: 71cbef121c40ef36e2d6e3a088cb85c4260599b9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/14/2018
ms.locfileid: "39060961"
---
# <a name="sap-for-devtest-workloads"></a>SAP für Dev/Test-Workloads

Dieses Beispiel enthält eine Anleitung für die Durchführung einer Dev/Test-Implementierung von SAP NetWeaver in einer Windows- oder Linux-Umgebung in Azure. Als Datenbank wird AnyDB verwendet. Dies ist der SAP-Begriff für alle unterstützten Datenbank-Managementsysteme, bei denen es sich nicht um SAP HANA handelt. Da diese Architektur nicht für Produktionsumgebungen ausgelegt ist, sondern nur für andere Umgebungen, wird sie mit nur einem virtuellen Computer (VM) bereitgestellt. Die Größe kann geändert werden, um die jeweiligen Anforderungen Ihrer Organisation zu erfüllen.

Sehen Sie sich für Anwendungsfälle für die Produktion die hier angegebenen SAP-Referenzarchitekturen an:

* [SAP NetWeaver für AnyDB][sap-netweaver]
* [SAP S/4Hana][sap-hana]
* [SAP in Azure (große Instanzen)][sap-large]

## <a name="related-use-cases"></a>Verwandte Anwendungsfälle

Erwägen Sie dieses Szenario für folgende Anwendungsfälle:

* Nicht kritische SAP-Workloads, die nicht für die Produktion bestimmt sind (Sandbox, Entwicklung, Test, Qualitätssicherung)
* Nicht kritische SAP Business One-Workloads

## <a name="architecture"></a>Architektur

![Diagramm](media/sap-2tier/SAP-Infra-2Tier_finalversion.png)

Mit diesem Szenario wird die Bereitstellung einer einzelnen SAP-Systemdatenbank und eines SAP-Anwendungsservers auf einem einzelnen virtuellen Computer behandelt. Die Daten durchlaufen das Szenario wie folgt:

1. Kunden auf der Präsentationsebene verwenden ihre SAP-GUI oder andere Benutzeroberflächen (Internet Explorer, Excel oder eine andere Webanwendung) lokal, um auf das Azure-basierte SAP-System zuzugreifen.
2. Die Konnektivität wird über die eingerichtete ExpressRoute-Verbindung bereitgestellt. Die Express Route-Verbindung endet in Azure auf dem ExpressRoute-Gateway. Netzwerkdatenverkehr wird über das ExpressRoute-Gateway an das Gatewaysubnetz und von dort an das Spoke-Subnetz auf der Anwendungsebene geleitet (siehe [Hub-Spoke][hub-spoke]-Muster). Anschließend fließt der Datenverkehr über ein Netzwerksicherheitsgateway an den virtuellen Computer der SAP-Anwendung.
3. Die Server für die Identitätsverwaltung stellen Authentifizierungsdienste bereit.
4. Über die Jumpbox sind lokale Verwaltungsfunktionen verfügbar.

### <a name="components"></a>Komponenten

* [Ressourcengruppen](/azure/azure-resource-manager/resource-group-overview#resource-groups) sind logische Container für Azure-Ressourcen.
* [Virtuelle Netzwerke](/azure/virtual-network/virtual-networks-overview) sind die Grundlage für die Netzwerkkommunikation innerhalb von Azure.
* Mit [Azure Virtual Machines](/azure/virtual-machines/windows/overview) wird eine auf Windows- oder Linux-Servern basierende sichere, virtualisierte On-Demand-Infrastruktur mit umfangreicher Skalierung bereitgestellt.
* Mit [ExpressRoute](/azure/expressroute/expressroute-introduction) können Sie Ihre lokalen Netzwerke über eine private Verbindung, die von einem Konnektivitätsanbieter bereitgestellt wird, auf die Microsoft Cloud ausdehnen.
* Mit [Netzwerksicherheitsgruppen](/azure/virtual-network/security-overview) können Sie den Netzwerkdatenverkehr auf Ressourcen in einem virtuellen Netzwerk beschränken. Eine Netzwerksicherheitsgruppe enthält eine Liste mit Sicherheitsregeln, die ein- oder ausgehenden Netzwerkdatenverkehr basierend auf IP-Adresse, Port und Protokoll (für die Quelle bzw. das Ziel) zulassen oder ablehnen. 

## <a name="considerations"></a>Überlegungen

### <a name="availability"></a>Verfügbarkeit

 Microsoft bietet eine Vereinbarung zum Servicelevel (Service Level Agreement, SLA) für einzelne VM-Instanzen an. Weitere Informationen zur Vereinbarung zum Servicelevel von Microsoft Azure für Virtual Machines finden Sie unter [SLA für Virtual Machines](https://azure.microsoft.com/support/legal/sla/virtual-machines).

### <a name="scalability"></a>Skalierbarkeit

Allgemeine Informationen zur Entwicklung skalierbarer Lösungen finden Sie im Azure Architecture Center in der [Checkliste für die Skalierbarkeit][scalability].

### <a name="security"></a>Sicherheit

Allgemeine Informationen zur Entwicklung sicherer Lösungen finden Sie in der [Dokumentation zur Azure-Sicherheit][security].

### <a name="resiliency"></a>Resilienz

Allgemeine Informationen zur Entwicklung robuster Lösungen finden Sie unter [Entwerfen robuster Anwendungen für Azure][resiliency].

## <a name="pricing"></a>Preise

Hier können Sie die Betriebskosten für dieses Szenario ermitteln. Alle Dienste sind im Kostenrechner vorkonfiguriert.  Wenn Sie wissen möchten, welche Kosten für Ihren spezifischen Anwendungsfall entstehen, passen Sie die entsprechenden Variablen an Ihren voraussichtlichen Datenverkehr an.

Auf der Grundlage des zu erwartenden Datenverkehrsaufkommens haben wir vier exemplarische Kostenprofile erstellt:

|Größe|SAPs|VM-Typ|Speicher|Azure-Preisrechner|
|----|----|-------|-------|---------------|
|Klein|8.000|D8s_v3|2 x P20, 1 x P10|[Klein](https://azure.com/e/9d26b9612da9466bb7a800eab56e71d1)|
|Mittel|16000|D16s_v3|3 x P20, 1 x P10|[Mittel](https://azure.com/e/465bd07047d148baab032b2f461550cd)|
Groß|32000|E32s_v3|3 x P20, 1 x P10|[Groß](https://azure.com/e/ada2e849d68b41c3839cc976000c6931)|
Sehr groß|64000|M64s|4 x P20, 1 x P10|[Sehr groß](https://azure.com/e/975fb58a965c4fbbb54c5c9179c61cef)|

Hinweis: Die Preise sollen als Anhaltspunkte gelten und geben nur die Kosten für VMs und Speicher an (ohne Gebühren für Netzwerk, Sicherungsspeicher und Datenein- und -ausgang).

* [Klein](https://azure.com/e/9d26b9612da9466bb7a800eab56e71d1): Ein kleines System umfasst den VM-Typ D8s_v3 mit 8 vCPUs, 32 GB RAM und 200 GB temporärem Speicher sowie zusätzlich zwei Premium-Speicherdatenträgern mit 512 GB und einem mit 128 GB.
* [Mittel](https://azure.com/e/465bd07047d148baab032b2f461550cd): Ein mittelgroßes System umfasst den VM-Typ D16s_v3 mit 16 vCPUs, 64 GB RAM und 400 GB temporärem Speicher sowie zusätzlich drei Premium-Speicherdatenträgern mit 512 GB und einem mit 128 GB.
* [Groß](https://azure.com/e/ada2e849d68b41c3839cc976000c6931): Ein großes System umfasst den VM-Typ E32s_v3 mit 32 vCPUs, 256 GB RAM und 512 GB temporärem Speicher sowie zusätzlich drei Premium-Speicherdatenträgern mit 512 GB und einem mit 128 GB.
* [Sehr groß](https://azure.com/e/975fb58a965c4fbbb54c5c9179c61cef): Ein sehr großes System umfasst den VM-Typ M64s mit 64 vCPUs, 1.024 GB RAM und 2.000 GB temporärem Speicher sowie zusätzlich vier Premium-Speicherdatenträgern mit 512 GB und einem mit 128 GB.

## <a name="deployment"></a>Bereitstellung

Verwenden Sie die Schaltfläche „Bereitstellen“, um die zugrunde liegende Infrastruktur ähnlich wie im obigen Szenario bereitzustellen.

<a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2Fmspnp%2Fsolution-architectures%2Fmaster%2Fapps%2Fsap-2tier%2Fazuredeploy.json" target="_blank">
    <img src="http://azuredeploy.net/deploybutton.png"/>
</a>

\* SAP wird nicht installiert. Sie müssen dies durchführen, nachdem die Infrastruktur manuell erstellt wurde.

<!-- links -->
[reference architecture]:  /azure/architecture/reference-architectures/sap
[resiliency]: /azure/architecture/resiliency/
[security]: /azure/security/
[scalability]: /azure/architecture/checklist/scalability
[sap-netweaver]: /azure/architecture/reference-architectures/sap/sap-netweaver
[sap-hana]: /azure/architecture/reference-architectures/sap/sap-s4hana
[sap-large]: /azure/architecture/reference-architectures/sap/hana-large-instances
[hub-spoke]: /azure/architecture/reference-architectures/hybrid-networking/hub-spoke