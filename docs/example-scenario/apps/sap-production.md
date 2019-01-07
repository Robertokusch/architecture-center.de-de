---
title: Ausführen von SAP-Produktionsworkloads mit einer Oracle Database-Instanz
titleSuffix: Azure Example Scenarios
description: Führen Sie eine SAP-Produktionsbereitstellung in Azure mit einer Oracle-Datenbank aus.
author: DharmeshBhagat
ms.date: 9/12/2018
ms.custom: fasttrack
ms.openlocfilehash: 2f398e98e383053f40fa8debcf5636c609339baf
ms.sourcegitcommit: bb7fcffbb41e2c26a26f8781df32825eb60df70c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/20/2018
ms.locfileid: "53643729"
---
# <a name="running-sap-production-workloads-using-an-oracle-database-on-azure"></a>Ausführen von SAP-Produktionsworkloads mit einer Oracle Database-Instanz in Azure

SAP-Systeme werden verwendet, um unternehmenskritische Geschäftsanwendungen auszuführen. Jeder Ausfall unterbricht wichtige Prozesse und kann die Ausgaben erhöhen oder zu Umsatzverlusten führen. Um dies zu verhindern, ist eine hoch verfügbare und im Fall von Fehlern resiliente SAP-Infrastruktur erforderlich.

Die Erstellung einer hoch verfügbaren SAP-Umgebung erfordert, dass Single Points of Failure in Ihrer Systemarchitektur und Ihren Prozessen beseitigt werden. Single Points of Failure können durch Standortausfälle, Fehler in Systemkomponenten oder sogar durch menschliche Fehler verursacht werden.

Dieses Beispielszenario veranschaulicht eine SAP-Bereitstellung auf Windows- oder Linux-VMs in Azure mit einer Oracle-Hochverfügbarkeitsdatenbank. Für Ihre SAP-Bereitstellung können Sie je nach Ihren Anforderungen VMs unterschiedlicher Größe verwenden.

## <a name="relevant-use-cases"></a>Relevante Anwendungsfälle

Zu den weiteren relevanten Anwendungsfällen zählen:

- In SAP ausgeführte unternehmenskritische Workloads
- Nicht kritische SAP-Workloads
- Testumgebungen für SAP, die eine Hochverfügbarkeitsumgebung simulieren

## <a name="architecture"></a>Architecture

![Übersicht über die Architektur einer SAP-Produktionsumgebung in Azure][architecture]

Dieses Beispiel umfasst eine Hochverfügbarkeitskonfiguration für eine Oracle-Datenbank, SAP Central Services und mehrere SAP-Anwendungsserver, die auf verschiedenen VMs ausgeführt werden. Das Azure-Netzwerk beruht aus Sicherheitsgründen auf einer [Hub-Spoke-Topologie](/azure/architecture/reference-architectures/hybrid-networking/hub-spoke). Die Daten durchlaufen die Lösung wie folgt:

1. Benutzer greifen über die SAP-Benutzeroberfläche, einen Webbrowser oder andere Clienttools wie Microsoft Excel auf das SAP-System zu. Eine ExpressRoute-Verbindung ermöglicht den Zugriff vom lokalen Unternehmensnetzwerk auf Ressourcen, die in Azure ausgeführt werden.
2. Die ExpressRoute-Verbindung endet in Azure auf dem ExpressRoute-Gateway des virtuellen Netzwerks (VNET). Netzwerkdatenverkehr wird über das im Hub-VNET erstellte ExpressRoute-Gateway zu einem Gatewaysubnetz geleitet.
3. Das Hub-VNET ist mittels Peering mit einem Spoke-VNET verbunden. Das Subnetz der Logikschicht hostet die VMs, auf denen SAP ausgeführt wird, in einer Verfügbarkeitsgruppe.
4. Die Server für die Identitätsverwaltung stellen Authentifizierungsdienste für die Lösung bereit.
5. Die Jumpbox wird von Systemadministratoren verwendet, um die in Azure bereitgestellten Ressourcen sicher zu verwalten.

### <a name="components"></a>Komponenten

- In diesem Szenario werden [virtuelle Netzwerke](/azure/virtual-network/virtual-networks-overview) verwendet, um eine virtuelle Hub-Spoke-Topologie in Azure zu erstellen.
- [VMs](/azure/virtual-machines/windows/overview) stellen die Computeressourcen für die einzelnen Ebenen der Lösung bereit. Jeder VM-Cluster ist als [Verfügbarkeitsgruppe](/azure/virtual-machines/windows/regions-and-availability#availability-sets) konfiguriert.
- [ExpressRoute](/azure/expressroute/expressroute-introduction) dehnt das lokale Netzwerk über eine private Verbindung, die von einem Konnektivitätsanbieter bereitgestellt wird, auf die Microsoft Cloud aus.
- [Netzwerksicherheitsgruppen (NSGs)](/azure/virtual-network/security-overview) beschränken den Netzwerkzugriff auf die Ressourcen in einem virtuellen Netzwerk. Eine Netzwerksicherheitsgruppe enthält eine Liste mit Sicherheitsregeln, die Netzwerkdatenverkehr basierend auf IP-Adresse, Port und Protokoll (für die Quelle bzw. das Ziel) zulassen oder ablehnen.
- [Ressourcengruppen](/azure/azure-resource-manager/resource-group-overview#resource-groups) fungieren als logische Container für Azure-Ressourcen.

### <a name="alternatives"></a>Alternativen

SAP bietet flexible Optionen für verschiedene Kombinationen von Betriebssystem, Datenbank-Managementsystem (DBMS) und VM-Typen in einer Azure-Umgebung. Weitere Informationen finden Sie im [SAP-Hinweis 1928533](https://launchpad.support.sap.com/#/notes/1928533): „SAP Applications on Azure: Supported Products and Azure VM types“ (SAP-Anwendungen in Azure: Unterstützte Produkte und Azure-VM-Typen).

## <a name="considerations"></a>Überlegungen

Für die Erstellung von hoch verfügbaren SAP-Umgebungen in Azure wurden empfohlene Vorgehensweisen definiert. Weitere Informationen finden Sie unter [Architektur und Szenarien für die Hochverfügbarkeit von SAP NetWeaver](/azure/virtual-machines/workloads/sap/sap-high-availability-architecture-scenarios) Informationen hierzu finden Sie auch unter [Hochverfügbarkeit von SAP-Anwendungen auf Azure-VMs](/azure/virtual-machines/workloads/sap/high-availability-guide).

Für Oracle-Datenbanken sind ebenfalls empfohlene Vorgehensweisen für Azure verfügbar. Weitere Informationen finden Sie unter [Entwerfen und Implementieren einer Oracle-Datenbank in Azure](/azure/virtual-machines/workloads/oracle/oracle-design).

Oracle Data Guard wird verwendet, um Single Points of Failure für unternehmenskritische Oracle-Datenbanken zu beseitigen. Weitere Informationen finden Sie unter [Implementieren von Oracle Data Guard auf einer Linux-VM in Azure](/azure/virtual-machines/workloads/oracle/configure-oracle-dataguard).

Microsoft Azure bietet Infrastrukturdienste, die zum Bereitstellen von SAP-Produkten mit einer Oracle-Datenbank verwendet werden können. Weitere Informationen finden Sie unter [Bereitstellen eines Oracle-DBMS in Azure für eine SAP-Workload](/azure/virtual-machines/workloads/sap/dbms_guide_oracle).

## <a name="pricing"></a>Preise

Um Ihnen die Ermittlung der Betriebskosten für dieses Szenario zu erleichtern, sind alle Dienste in den unten stehenden Kostenrechnerbeispielen vorkonfiguriert. Wenn Sie wissen möchten, welche Kosten für Ihren spezifischen Anwendungsfall entstehen, passen Sie die entsprechenden Variablen an Ihren voraussichtlichen Datenverkehr an.

Auf der Grundlage des zu erwartenden Datenverkehrsaufkommens haben wir vier exemplarische Kostenprofile erstellt:

|Größe|SAPs|Datenbank-VM-Typ|Datenbankspeicher|(A)SCS-VM|(A)SCS-Speicher|App-VM-Typ|App-Speicher|Azure-Preisrechner|
|----|----|-------|-------|-----|---|---|--------|---------------|
|Klein|30000|DS13_v2|4 x P20, 1 x P20|DS11_v2|1 x P10|DS13_v2|1 x P10|[Klein](https://azure.com/e/45880ba0bfdf47d497851a7cf2650c7c)|
|Mittel|70000|DS14_v2|6 x P20, 1 x P20|DS11_v2|1 x P10|4 x DS13_v2|1 x P10|[Mittel](https://azure.com/e/9a523f79591347ca9a48c3aaa1406f8a)|
Groß|180.000|E32s_v3|5 x P30, 1 x P20|DS11_v2|1 x P10|6 x DS14_v2|1 x P10|[Groß](https://azure.com/e/f70fccf571e948c4b37d4fecc07cbf42)|
Sehr groß|250.000|M64s|6 x P30, 1 x P30|DS11_v2|1 x P10|10 x DS14_v2|1 x P10|[Sehr groß](https://azure.com/e/58c636922cf94faf9650f583ff35e97b)|

> [!NOTE]
> Die Preise sind als Orientierungshilfe gedacht und geben nur die Kosten für VMs und Speicher an. Gebühren für Netzwerk, Sicherungsspeicher und ein-/ausgehende Daten sind nicht enthalten.

- [Klein:](https://azure.com/e/45880ba0bfdf47d497851a7cf2650c7c) Ein kleines System umfasst Folgendes: Den VM-Typ „DS13_v2“ für den Datenbankserver mit acht vCPUs, 56 GB RAM und 112 GB temporärem Speicher sowie zusätzlich fünf Storage Premium-Datenträger mit 512 GB. Einen SAP Central Instance-Server mit dem VM-Typ „DS11_v2“ mit zwei vCPUs, 14 GB RAM und 28 GB temporärem Speicher. Eine einzelne VM vom Typ „DS13_v2“ für den SAP-Anwendungsserver mit acht vCPUs, 56 GB RAM und 400 GB temporärem Speicher sowie zusätzlich einem Storage Premium-Datenträger mit 128 GB.

- [Mittel:](https://azure.com/e/9a523f79591347ca9a48c3aaa1406f8a) Ein mittleres System umfasst Folgendes: Den VM-Typ „DS14_v2“ für den Datenbankserver mit 16 vCPUs, 112 GB RAM und 800 GB temporärem Speicher sowie zusätzlich sieben Storage Premium-Datenträger mit 512 GB. Einen SAP Central Instance-Server mit dem VM-Typ „DS11_v2“ mit zwei vCPUs, 14 GB RAM und 28 GB temporärem Speicher. Vier VMs vom Typ „DS13_v2“ für den SAP-Anwendungsserver mit acht vCPUs, 56 GB RAM und 400 GB temporärem Speicher sowie zusätzlich einem Storage Premium-Datenträger mit 128 GB.

- [Groß:](https://azure.com/e/f70fccf571e948c4b37d4fecc07cbf42) Ein großes System umfasst Folgendes: Den VM-Typ „E32s_v3“ für den Datenbankserver mit 32 vCPUs, 256 GB RAM und 800 GB temporärem Speicher sowie zusätzlich vier Storage Premium-Datenträger (drei mit 512 GB, einer mit 128 GB). Einen SAP Central Instance-Server mit dem VM-Typ „DS11_v2“ mit zwei vCPUs, 14 GB RAM und 28 GB temporärem Speicher. Sechs VMs vom Typ „DS14_v2“ für die SAP-Anwendungsserver mit 16 vCPUs, 112 GB RAM und 224 GB temporärem Speicher sowie zusätzlich sechs Storage Premium-Datenträger mit 128 GB.

- [Sehr groß:](https://azure.com/e/58c636922cf94faf9650f583ff35e97b) Ein sehr großes System umfasst Folgendes: Den VM-Typ „M64s“ für den Datenbankserver mit 64 vCPUs, 1024 GB RAM und 2000 GB temporärem Speicher sowie zusätzlich sieben Storage Premium-Datenträger mit 1024 GB. Einen SAP Central Instance-Server mit dem VM-Typ „DS11_v2“ mit zwei vCPUs, 14 GB RAM und 28 GB temporärem Speicher. Zehn VMs vom Typ „DS14_v2“ für die SAP-Anwendungsserver mit 16 vCPUs, 112 GB RAM und 224 GB temporärem Speicher sowie zusätzlich zehn Storage Premium-Datenträger mit 128 GB.

## <a name="deployment"></a>Bereitstellung

Klicken Sie auf den folgenden Link, um die zugrunde liegende Infrastruktur für dieses Szenario bereitzustellen.

<!-- markdownlint-disable MD033 -->

<a
href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2Fmspnp%2Fsolution-architectures%2Fmaster%2Fapps%2Fsap-3tier-distributed-ora%2Fazuredeploy.json" target="_blank">
    <img src="https://azuredeploy.net/deploybutton.png"/>
</a>

<!-- markdownlint-enable MD033 -->

> [!NOTE]
> SAP und Oracle werden während dieser Bereitstellung nicht installiert. Sie müssen diese Komponenten separat bereitstellen.

## <a name="related-resources"></a>Zugehörige Ressourcen

Weitere Informationen zum Ausführen von SAP-Produktionsworkloads in Azure finden Sie in den folgenden Referenzarchitekturen:

- [SAP NetWeaver für AnyDB](/azure/architecture/reference-architectures/sap/sap-netweaver)
- [SAP S/4HANA](/azure/architecture/reference-architectures/sap/sap-s4hana)
- [Große SAP HANA-Instanzen](/azure/architecture/reference-architectures/sap/hana-large-instances)

<!-- links -->
[architecture]: media/architecture-sap-production.png
