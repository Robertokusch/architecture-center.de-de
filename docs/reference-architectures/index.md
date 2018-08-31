---
title: Azure-Referenzarchitekturen
description: Referenzarchitekturen, Blaupausen und reglementierende Implementierungsanweisungen für gängige Workloads in Azure
layout: LandingPage
ms.topic: landing-page
ms.date: 08/30/2018
ms.openlocfilehash: e9b3a65c48c759f9fc07da9f2c4195fc2db4c782
ms.sourcegitcommit: ae8a1de6f4af7a89a66a8339879843d945201f85
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/31/2018
ms.locfileid: "43325570"
---
# <a name="azure-reference-architectures"></a>Azure-Referenzarchitekturen

Unsere Referenzarchitekturen sind nach Szenarien angeordnet, wobei verwandte Architekturen gruppiert sind. Jede Architektur ist mit empfohlenen Methoden sowie Überlegungen in Bezug auf die Skalierbarkeit, Verfügbarkeit, Verwaltbarkeit und Sicherheit versehen. Bei einem Großteil wird außerdem eine bereitstellbare Lösung angegeben.

Springen zu: [Big Data](#big-data-solutions) | [Webanwendungen](#web-applications) | [n-schichtige Anwendungen](#n-tier-applications) | [Virtuelle Netzwerke](#virtual-networks) | [Active Directory](#extending-on-premises-active-directory-to-azure) | [VM-Workloads](#vm-workloads)

## <a name="big-data-solutions"></a>Big Data-Lösungen

<ul  class="panelContent cardsF">
<!-- SQL Data Warehouse -->
<li style="display: flex; flex-direction: column;">
    <a href="./data/enterprise-bi-sqldw.md" style="display: flex; flex-direction: column; flex: 1 0 auto;">
        <div class="cardSize" style="flex: 1 0 auto; display: flex;">
            <div class="cardPadding" style="display: flex;">
                <div class="card">
                    <div class="cardImageOuter">
                        <div class="cardImage">
                            <img src="./data/images/data-guide.svg" />
                        </div>
                    </div>
                    <div class="cardText">
                        <h3>Enterprise BI mit SQL Data Warehouse</h3>
                        <p>Führen Sie den ELT-Vorgang (extrahieren – laden – transformieren) für eine Pipeline aus, um Daten aus einer lokalen Datenbank in SQL Data Warehouse zu verschieben.</p>
                    </div>
                </div>
            </div>
        </div>
    </a>
</li>
<li style="display: flex; flex-direction: column;">
    <a href="./data/enterprise-bi-adf.md" style="display: flex; flex-direction: column; flex: 1 0 auto;">
        <div class="cardSize" style="flex: 1 0 auto; display: flex;">
            <div class="cardPadding" style="display: flex;">
                <div class="card">
                    <div class="cardImageOuter">
                        <div class="cardImage">
                            <img src="./data/images/data-guide.svg" />
                        </div>
                    </div>
                    <div class="cardText">
                        <h3>Automatisierte Enterprise BI-Instanz mit Azure Data Factory</h3>
                        <p>Automatisieren Sie eine ELT-Pipeline für das inkrementelle Laden aus einer lokalen Datenbank.</p>
                    </div>
                </div>
            </div>
        </div>
    </a>
</li>
<!-- Stream Analytics -->
<li style="display: flex; flex-direction: column;">
    <a href="./data/stream-processing-stream-analytics.md" style="display: flex; flex-direction: column; flex: 1 0 auto;">
        <div class="cardSize" style="flex: 1 0 auto; display: flex;">
            <div class="cardPadding" style="display: flex;">
                <div class="card">
                    <div class="cardImageOuter">
                        <div class="cardImage">
                            <img src="./_images/azure-analysis-service.svg" />
                        </div>
                    </div>
                    <div class="cardText">
                        <h3>Datenstromverarbeitung mit Azure Stream Analytics</h3>
                        <p>End-to-End-Pipeline zur Datenstromverarbeitung, die zum Berechnen des gleitenden Durchschnitts Datensätze aus zwei Datenströmen korreliert</p>
                    </div>
                </div>
            </div>
        </div>
    </a>
</li>
</ul>

## <a name="web-applications"></a>Webanwendungen

<ul  class="panelContent cardsF">
<li style="display: flex; flex-direction: column;">
    <a href="./app-service-web-app/basic-web-app.md" style="display: flex; flex-direction: column; flex: 1 0 auto;">
        <div class="cardSize" style="flex: 1 0 auto; display: flex;">
            <div class="cardPadding" style="display: flex;">
                <div class="card">
                    <div class="cardImageOuter">
                        <div class="cardImage">
                            <img src="./_images/app-service.svg" height="140px" />
                        </div>
                    </div>
                    <div class="cardText">
                        <h3>Einfache Webanwendung</h3>
                        <p>Webanwendung mit Azure App Service und Azure SQL-Datenbank.</p>
                    </div>
                </div>
            </div>
        </div>
    </a>
</li>
<li style="display: flex; flex-direction: column;">
    <a href="./app-service-web-app/scalable-web-app.md" style="display: flex; flex-direction: column; flex: 1 0 auto;">
        <div class="cardSize" style="flex: 1 0 auto; display: flex;">
            <div class="cardPadding" style="display: flex;">
                <div class="card">
                    <div class="cardImageOuter">
                        <div class="cardImage">
                            <img src="./_images/app-service.svg" height="140px" />
                        </div>
                    </div>
                    <div class="cardText">
                        <h3>Hochgradig skalierbare Webanwendung</h3>
                        <p>Bewährte Methoden zum Verbessern der Skalierbarkeit in einer Webanwendung.</p>
                    </div>
                </div>
            </div>
        </div>
    </a>
</li>
<li style="display: flex; flex-direction: column;">
    <a href="./app-service-web-app/multi-region.md" style="display: flex; flex-direction: column; flex: 1 0 auto;">
        <div class="cardSize" style="flex: 1 0 auto; display: flex;">
            <div class="cardPadding" style="display: flex;">
                <div class="card">
                    <div class="cardImageOuter">
                        <div class="cardImage">
                            <img src="./_images/app-service.svg" height="140px" />
                        </div>
                    </div>
                    <div class="cardText">
                        <h3>Hoch verfügbare Webanwendung</h3>
                        <p>Führen Sie eine App Service-Web-App in mehreren Regionen aus, um Hochverfügbarkeit zu erzielen.</p>
                    </div>
                </div>
            </div>
        </div>
    </a>
</li>
</ul>

## <a name="n-tier-applications"></a>n-schichtige Anwendungen

<ul  class="panelContent cardsF">
<li style="display: flex; flex-direction: column;">
    <a href="./n-tier/n-tier-sql-server.md" style="display: flex; flex-direction: column; flex: 1 0 auto;">
        <div class="cardSize" style="flex: 1 0 auto; display: flex;">
            <div class="cardPadding" style="display: flex;">
                <div class="card">
                    <div class="cardImageOuter">
                        <div class="cardImage">
                            <img src="./n-tier/images/Windows.svg" height="140px" />
                        </div>
                    </div>
                    <div class="cardText">
                        <h3>n-schichtige Anwendung mit SQL Server</h3>
                        <p>Virtuelle Computer, die für eine n-schichtige Anwendung mit SQL Server unter Windows konfiguriert wurden.</p>
                    </div>
                </div>
            </div>
        </div>
    </a>
</li>

<!-- Multi-region Windows -->
<li style="display: flex; flex-direction: column;">
    <a href="./n-tier/multi-region-sql-server.md" style="display: flex; flex-direction: column; flex: 1 0 auto;">
        <div class="cardSize" style="flex: 1 0 auto; display: flex;">
            <div class="cardPadding" style="display: flex;">
                <div class="card">
                    <div class="cardImageOuter">
                        <div class="cardImage">
                            <img src="./n-tier/images/Windows.svg" height="140px" />
                        </div>
                    </div>
                    <div class="cardText">
                        <h3>n-schichtige Anwendung für mehrere Regionen</h3>
                        <p>n-schichtige Anwendung in zwei Regionen für Hochverfügbarkeit und mit SQL Server Always On-Verfügbarkeitsgruppen.</p>
                    </div>
                </div>
            </div>
        </div>
    </a>
</li>

<!-- N-tier Linux -->
<li style="display: flex; flex-direction: column;">
    <a href="./n-tier/n-tier-cassandra.md" style="display: flex; flex-direction: column; flex: 1 0 auto;">
        <div class="cardSize" style="flex: 1 0 auto; display: flex;">
            <div class="cardPadding" style="display: flex;">
                <div class="card">
                    <div class="cardImageOuter">
                        <div class="cardImage">
                            <img src="./n-tier/images/LinuxPenguin.svg" height="140px" />
                        </div>
                    </div>
                    <div class="cardText">
                        <h3>n-schichtige Anwendung mit Cassandra</h3>
                        <p>Virtuelle Computer, die mit Apache Cassandra unter Linux für eine n-schichtige Anwendung konfiguriert wurden.</p>
                    </div>
                </div>
            </div>
        </div>
    </a>
</li>
</ul>

## <a name="virtual-networks"></a>Virtuelle Netzwerke

<ul  class="panelContent cardsF">
<li style="display: flex; flex-direction: column;">
    <a href="./hybrid-networking/vpn.md" style="display: flex; flex-direction: column; flex: 1 0 auto;">
        <div class="cardSize" style="flex: 1 0 auto; display: flex;">
            <div class="cardPadding" style="display: flex;">
                <div class="card">
                    <div class="cardImageOuter">
                        <div class="cardImage">
                            <img src="./_images/vpn.svg" height="140px" />
                        </div>
                    </div>
                    <div class="cardText">
                        <h3>Hybridnetzwerk mit einem virtuellen privaten Netzwerk (VPN)</h3>
                        <p>Verbinden Sie ein lokales Netzwerk mit einem virtuellen Azure-Netzwerk.</p>
                    </div>
                </div>
            </div>
        </div>
    </a>
</li>
<!-- ExpressRoute -->
<li style="display: flex; flex-direction: column;">
    <a href="./hybrid-networking/expressroute.md" style="display: flex; flex-direction: column; flex: 1 0 auto;">
        <div class="cardSize" style="flex: 1 0 auto; display: flex;">
            <div class="cardPadding" style="display: flex;">
                <div class="card">
                    <div class="cardImageOuter">
                        <div class="cardImage">
                            <img src="./_images/expressroute.svg" height="140px" />
                        </div>
                    </div>
                    <div class="cardText">
                        <h3>Hybridnetzwerk mit ExpressRoute</h3>
                        <p>Verwenden Sie eine dedizierte private Verbindung, um ein lokales Netzwerk auf Azure zu erweitern.</p>
                    </div>
                </div>
            </div>
        </div>
    </a>
</li>
<!-- ExpressRoute + VPN -->
<li style="display: flex; flex-direction: column;">
    <a href="./hybrid-networking/expressroute-vpn-failover.md" style="display: flex; flex-direction: column; flex: 1 0 auto;">
        <div class="cardSize" style="flex: 1 0 auto; display: flex;">
            <div class="cardPadding" style="display: flex;">
                <div class="card">
                    <div class="cardImageOuter">
                        <div class="cardImage">
                            <img src="./_images/expressroute.svg" height="140px" />
                        </div>
                    </div>
                    <div class="cardText">
                        <h3>Hybridnetzwerk mit ExpressRoute und VPN-Failover</h3>
                        <p>Verwenden Sie ExpressRoute mit einem VPN als Failoververbindung für hohe Verfügbarkeit.</p>
                    </div>
                </div>
            </div>
        </div>
    </a>
</li>
<!-- Hub spoke -->
<li style="display: flex; flex-direction: column;">
    <a href="./hybrid-networking/hub-spoke.md" style="display: flex; flex-direction: column; flex: 1 0 auto;">
        <div class="cardSize" style="flex: 1 0 auto; display: flex;">
            <div class="cardPadding" style="display: flex;">
                <div class="card">
                    <div class="cardImageOuter">
                        <div class="cardImage">
                            <img src="./_images/gateway.svg" height="140px" />
                        </div>
                    </div>
                    <div class="cardText">
                        <h3>Hub-Spoke-Netzwerktopologie</h3>
                        <p>Erstellen Sie einen zentralen Konnektivitätspunkt für Ihr lokales Netzwerk, und isolieren Sie gleichzeitig Workloads.</p>
                    </div>
                </div>
            </div>
        </div>
    </a>
</li>
<!-- Shared services -->
<li style="display: flex; flex-direction: column;">
    <a href="./hybrid-networking/hub-spoke.md" style="display: flex; flex-direction: column; flex: 1 0 auto;">
        <div class="cardSize" style="flex: 1 0 auto; display: flex;">
            <div class="cardPadding" style="display: flex;">
                <div class="card">
                    <div class="cardImageOuter">
                        <div class="cardImage">
                            <img src="./_images/gateway.svg" height="140px" />
                        </div>
                    </div>
                    <div class="cardText">
                        <h3>Hub-Spoke-Topologie mit gemeinsamen Diensten</h3>
                        <p>Erweitern Sie eine Hub-Spoke-Topologie durch Einbinden gemeinsamer Dienste wie Active Directory.</p>
                    </div>
                </div>
            </div>
        </div>
    </a>
</li>
<!-- Hybrid DMZ -->
<li style="display: flex; flex-direction: column;">
    <a href="./dmz/secure-vnet-hybrid.md" style="display: flex; flex-direction: column; flex: 1 0 auto;">
        <div class="cardSize" style="flex: 1 0 auto; display: flex;">
            <div class="cardPadding" style="display: flex;">
                <div class="card">
                    <div class="cardImageOuter">
                        <div class="cardImage">
                            <img src="./_images/vnet.svg" height="140px" />
                        </div>
                    </div>
                    <div class="cardText">
                        <h3>DMZ zwischen Azure und einem lokalen Netzwerk</h3>
                        <p>Verwenden Sie virtuelle Netzwerkgeräte, um ein sicheres Hybridnetzwerk zu erstellen.</p>
                    </div>
                </div>
            </div>
        </div>
    </a>
</li>
<!-- Internet DMZ -->
<li style="display: flex; flex-direction: column;">
    <a href="./dmz/secure-vnet-dmz.md" style="display: flex; flex-direction: column; flex: 1 0 auto;">
        <div class="cardSize" style="flex: 1 0 auto; display: flex;">
            <div class="cardPadding" style="display: flex;">
                <div class="card">
                    <div class="cardImageOuter">
                        <div class="cardImage">
                            <img src="./_images/vnet.svg" height="140px" />
                        </div>
                    </div>
                    <div class="cardText">
                        <h3>DMZ zwischen Azure und dem Internet</h3>
                        <p>Verwenden Sie virtuelle Netzwerkgeräte, um ein sicheres Netzwerk zu erstellen, das Datenverkehr aus dem Internet akzeptiert.</p>
                    </div>
                </div>
            </div>
        </div>
    </a>
</li>
</ul>

## <a name="extending-on-premises-active-directory-to-azure"></a>Erweitern der lokalen Active Directory-Instanz auf Azure

<ul class="panelContent cardsF">
<!-- Azure AD -->
<li style="display: flex; flex-direction: column;">
    <a href="./identity/azure-ad.md" style="display: flex; flex-direction: column; flex: 1 0 auto;">
        <div class="cardSize" style="flex: 1 0 auto; display: flex;">
            <div class="cardPadding" style="display: flex;">
                <div class="card">
                    <div class="cardImageOuter">
                        <div class="cardImage">
                            <img src="./_images/azure-active-directory.svg" height="140px" />
                        </div>
                    </div>
                    <div class="cardText">
                        <h3>Integrieren in Azure Active Directory</h3>
                        <p>Integrieren Sie lokale AD-Domänen in Azure Active Directory.</p>
                    </div>
                </div>
            </div>
        </div>
    </a>
</li>
<!-- AD DS -->
<li style="display: flex; flex-direction: column;">
    <a href="./identity/adds-extend-domain.md" style="display: flex; flex-direction: column; flex: 1 0 auto;">
        <div class="cardSize" style="flex: 1 0 auto; display: flex;">
            <div class="cardPadding" style="display: flex;">
                <div class="card">
                    <div class="cardImageOuter">
                        <div class="cardImage">
                            <img src="./_images/active-directory-vm.svg" height="140px" />
                        </div>
                    </div>
                    <div class="cardText">
                        <h3>Erweitern einer lokalen Active Directory-Domäne auf Azure</h3>
                        <p>Stellen Sie Active Directory Domain Services (AD DS) in Azure bereit, um Ihre lokale Domäne zu erweitern.</p>
                    </div>
                </div>
            </div>
        </div>
    </a>
</li>
<!-- AD DS Forest -->
<li style="display: flex; flex-direction: column;">
    <a href="./identity/adds-forest.md" style="display: flex; flex-direction: column; flex: 1 0 auto;">
        <div class="cardSize" style="flex: 1 0 auto; display: flex;">
            <div class="cardPadding" style="display: flex;">
                <div class="card">
                    <div class="cardImageOuter">
                        <div class="cardImage">
                            <img src="./_images/active-directory-vm.svg" height="140px" />
                        </div>
                    </div>
                    <div class="cardText">
                        <h3>AD DS-Gesamtstruktur in Azure erstellen</h3>
                        <p>Erstellen Sie in Azure eine separate AD-Domäne, die für Ihre lokale AD-Gesamtstruktur vertrauenswürdig ist.</p>
                    </div>
                </div>
            </div>
        </div>
    </a>
</li>
<!-- AD FS -->
<li style="display: flex; flex-direction: column;">
    <a href="./identity/adfs.md" style="display: flex; flex-direction: column; flex: 1 0 auto;">
        <div class="cardSize" style="flex: 1 0 auto; display: flex;">
            <div class="cardPadding" style="display: flex;">
                <div class="card">
                    <div class="cardImageOuter">
                        <div class="cardImage">
                            <img src="./_images/active-directory-vm.svg" height="140px" />
                        </div>
                    </div>
                    <div class="cardText">
                        <h3>Erweitern von Active Directory Federation Services (AD FS) auf Azure</h3>
                        <p>Verwenden Sie AD FS für die Verbundauthentifizierung und Autorisierung für in Azure ausgeführte Komponenten.</p>
                    </div>
                </div>
            </div>
        </div>
    </a>
</li>
</ul>

## <a name="vm-workloads"></a>VM-Workloads

<ul  class="panelContent cardsF">
<!-- Jenkins -->
<li style="display: flex; flex-direction: column;">
    <a href="./jenkins/index.md" style="display: flex; flex-direction: column; flex: 1 0 auto;">
        <div class="cardSize" style="flex: 1 0 auto; display: flex;">
            <div class="cardPadding" style="display: flex;">
                <div class="card">
                    <div class="cardImageOuter">
                        <div class="cardImage">
                            <img src="./jenkins/images/logo.svg" height="140px" />
                        </div>
                    </div>
                    <div class="cardText">
                        <h3>Jenkins-Buildserver</h3>
                        <p>Skalierbarer Jenkins-Servers in Azure auf Unternehmensniveau.</p>
                    </div>
                </div>
            </div>
        </div>
    </a>
</li>
<!-- SharePoint -->
<li style="display: flex; flex-direction: column;">
    <a href="./sharepoint/index.md" style="display: flex; flex-direction: column; flex: 1 0 auto;">
        <div class="cardSize" style="flex: 1 0 auto; display: flex;">
            <div class="cardPadding" style="display: flex;">
                <div class="card">
                    <div class="cardImageOuter">
                        <div class="cardImage">
                            <img src="./sharepoint/images/sharepoint.svg" height="140px" />
                        </div>
                    </div>
                    <div class="cardText">
                        <h3>SharePoint Server 2016-Farm</h3>
                        <p>SharePoint Server 2016-Hochverfügbarkeitsfarm in Azure mit SQL Server AlwaysOn-Verfügbarkeitsgruppen.</p>
                    </div>
                </div>
            </div>
        </div>
    </a>
</li>
<!-- SAP -->
<li style="display: flex; flex-direction: column;">
    <a href="./sap/sap-netweaver.md" style="display: flex; flex-direction: column; flex: 1 0 auto;">
        <div class="cardSize" style="flex: 1 0 auto; display: flex;">
            <div class="cardPadding" style="display: flex;">
                <div class="card">
                    <div class="cardImageOuter">
                        <div class="cardImage">
                            <img src="./sap/images/sap.svg" height="140px" />
                        </div>
                    </div>
                    <div class="cardText">
                        <h3>SAP NetWeaver</h3>
                        <p>SAP NetWeaver unter Windows in einer Hochverfügbarkeitsumgebung, die Wiederherstellung im Notfall unterstützt.</p>
                    </div>
                </div>
            </div>
        </div>
    </a>
</li>
<li style="display: flex; flex-direction: column;">
    <a href="./sap/sap-s4hana.md" style="display: flex; flex-direction: column; flex: 1 0 auto;">
        <div class="cardSize" style="flex: 1 0 auto; display: flex;">
            <div class="cardPadding" style="display: flex;">
                <div class="card">
                    <div class="cardImageOuter">
                        <div class="cardImage">
                            <img src="./sap/images/sap.svg" height="140px" />
                        </div>
                    </div>
                    <div class="cardText">
                        <h3>SAP S/4HANA</h3>
                        <p>SAP S/4HANA unter Linux in einer Hochverfügbarkeitsumgebung, die Wiederherstellung im Notfall unterstützt.</p>
                    </div>
                </div>
            </div>
        </div>
    </a>
</li>
<li style="display: flex; flex-direction: column;">
    <a href="./sap/hana-large-instances.md" style="display: flex; flex-direction: column; flex: 1 0 auto;">
        <div class="cardSize" style="flex: 1 0 auto; display: flex;">
            <div class="cardPadding" style="display: flex;">
                <div class="card">
                    <div class="cardImageOuter">
                        <div class="cardImage">
                            <img src="./sap/images/sap.svg" height="140px" />
                        </div>
                    </div>
                    <div class="cardText">
                        <h3>SAP HANA in Azure (große Instanzen)</h3>
                        <p>Große HANA-Instanzen werden auf physischen Servern in Azure-Regionen bereitgestellt.</p>
                    </div>
                </div>
            </div>
        </div>
    </a>
</li>
</ul>

