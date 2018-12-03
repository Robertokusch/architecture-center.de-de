---
title: Webanwendung mit mehreren Ebenen, die für Hochverfügbarkeit und Notfallwiederherstellung in Azure konzipiert ist
description: Erstellen einer Webanwendung mit mehreren Ebenen für Hochverfügbarkeit und Notfallwiederherstellung in Azure mithilfe von virtuellen Azure-Computern, Verfügbarkeitsgruppen, Verfügbarkeitszonen, Azure Site Recovery und Azure Traffic Manager
author: sujayt
ms.date: 11/16/2018
ms.openlocfilehash: 28593c680746dc5ac8f7f25641faa57569dcc53f
ms.sourcegitcommit: 16bc6a91b6b9565ca3bcc72d6eb27c2c4ae935e4
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/28/2018
ms.locfileid: "52579463"
---
# <a name="multitier-web-application-built-for-high-availability-and-disaster-recovery-on-azure"></a>Webanwendung mit mehreren Ebenen, die für Hochverfügbarkeit und Notfallwiederherstellung in Azure konzipiert ist

Dieses Beispielszenario ist für alle Branchen relevant, in denen robuste Anwendungen mit mehreren Ebenen bereitgestellt werden müssen, die für Hochverfügbarkeit und Notfallwiederherstellung konzipiert sind. Die Anwendung in diesem Szenario umfasst drei Ebenen.

- Webebene: Die oberste Ebene, die auch die Benutzeroberfläche enthält. Diese Ebene analysiert Benutzerinteraktionen und übergibt die Aktionen zur Verarbeitung an die nächste Ebene.
- Geschäftsebene: Verarbeitet die Benutzerinteraktionen und trifft logische Entscheidungen hinsichtlich der nächsten Schritte. Diese Ebene verbindet die Webebene mit der Datenebene.
- Datenebene: Speichert die Anwendungsdaten. Hierzu wird In der Regel eine Datenbank, ein Objektspeicher oder ein Dateispeicher verwendet.

Zu den gängigen Anwendungsszenarien zählen unter anderem unternehmenskritische Anwendungen unter Windows oder Linux. Hierbei kann es sich um Standardanwendungen wie SAP und SharePoint oder um eine spezielle Branchenanwendung handeln.

## <a name="relevant-use-cases"></a>Relevante Anwendungsfälle

Zu den weiteren relevanten Anwendungsfällen zählen:

* Bereitstellen von Anwendungen mit hoher Resilienz wie SAP und SharePoint
* Entwerfen eines BCDR-Plans (Business Continuity & Disaster Recovery) für Branchenanwendungen
* Konfigurieren der Notfallwiederherstellung und Ausführen entsprechender Übungen zu Compliancezwecken

## <a name="architecture"></a>Architektur

In diesem Szenario wird eine Anwendung mit mehreren Ebenen gezeigt, die ASP.NET und Microsoft SQL Server verwendet. In [Azure-Regionen, die Verfügbarkeitszonen unterstützen](/azure/availability-zones/az-overview#regions-that-support-availability-zones), können Sie Ihre virtuellen Computer (Virtual Machines, VMs) in einer Quellregion über Verfügbarkeitszonen hinweg bereitstellen und die virtuellen Computer in der für die Notfallwiederherstellung verwendeten Zielregion replizieren. In Azure-Regionen, die keine Verfügbarkeitszonen unterstützen, können Sie Ihre virtuellen Computer innerhalb einer Verfügbarkeitsgruppe bereitstellen und die virtuellen Computer in der Zielregion replizieren.

![Architekturübersicht für eine Webanwendung mit hoher Resilienz und mehreren Ebenen][architecture]

- Verteilen Sie in Regionen, die Zonen unterstützen, die virtuellen Computer der einzelnen Ebenen auf zwei Verfügbarkeitszonen. Stellen Sie in anderen Regionen die virtuellen Computer der einzelnen Ebenen in einer einzelnen Verfügbarkeitsgruppe bereit.
- Die Datenbankebene kann so konfiguriert werden, dass Always On-Verfügbarkeitsgruppen konfiguriert werden. Mit dieser SQL Server-Konfiguration wird eine primäre Datenbank in einem Cluster mit bis zu acht sekundären Datenbanken konfiguriert. Im Falle eines Problems mit der primären Datenbank führt der Cluster ein Failover auf eine der sekundären Datenbanken aus, damit die Anwendung weiterhin verfügbar ist. Weitere Informationen finden Sie unter [Übersicht über Always On-Verfügbarkeitsgruppen für SQL Server][docs-sql-always-on].
- In Notfallwiederherstellungsszenarien können Sie eine asynchrone native SQL AlwaysOn-Replikation in der für die Notfallwiederherstellung verwendeten Zielregion konfigurieren. Sie können auch eine Azure Site Recovery-Replikation in der Zielregion konfigurieren, sofern die Datenänderungsrate innerhalb der unterstützten Grenzwerte von Azure Site Recovery liegt.
- Benutzer greifen über den Traffic Manager-Endpunkt auf die Front-End-ASP.NET-Webebene zu.
- Der Traffic Manager leitet Datenverkehr an den primären öffentlichen IP-Endpunkt in der primären Quellregion weiter.
- Die öffentliche IP-Adresse leitet den Aufruf über einen öffentlichen Load Balancer an eine der VM-Instanzen der Webebene weiter. Alle VM-Instanzen der Webebene befinden sich im gleichen Subnetz.
- Von dem virtuellen Computer auf der Webebene wird jeder Aufruf über einen internen Load Balancer zur Verarbeitung an eine der VM-Instanzen auf der Geschäftsebene weitergeleitet. Alle virtuellen Computer auf der Geschäftsebene befinden sich in einem separaten Subnetz.
- Der Vorgang wird auf der Geschäftsebene verarbeitet, und die ASP.NET-Anwendung stellt über einen internen Azure Load Balancer eine Verbindung mit dem Microsoft SQL Server-Cluster auf einer Back-End-Ebene her. Diese SQL Server-Back-End-Instanzen befinden sich in einem separaten Subnetz.
- Der sekundäre Endpunkt des Traffic Managers wird als öffentliche IP-Adresse in der für die Notfallwiederherstellung verwendeten Zielregion konfiguriert.
- Im Falle einer Störung der primären Region rufen Sie ein Azure Site Recovery-Failover auf, und die Anwendung wird in der Zielregion aktiv.
- Der Traffic Manager-Endpunkt leitet den Clientdatenverkehr automatisch an die öffentliche IP-Adresse in der Zielregion um.

### <a name="components"></a>Komponenten

* [Verfügbarkeitsgruppen][docs-availability-sets] sorgen dafür, dass die von Ihnen in Azure bereitgestellten virtuellen Computer auf mehrere isolierte Hardwareknoten in einem Cluster verteilt werden. Dadurch wirken sich Hardware- oder Softwarefehler in Azure nur auf einen Teil Ihrer virtuellen Computer aus, und Ihre Lösung bleibt insgesamt verfügbar und betriebsbereit.
* [Verfügbarkeitszonen][docs-availability-zones] schützen Ihre Anwendungen und Daten vor Datencenterausfällen. Bei Verfügbarkeitszonen handelt es sich um separate physische Standorte in einer Azure-Region. Jede Zone besteht aus mindestens einem Datencenter mit eigener Stromversorgung, Kühlung und Netzwerk. 
* Mit [Azure Site Recovery (ASR)][docs-azure-site-recovery] können Sie virtuelle Computer in einer anderen Azure-Region replizieren, um Ihre BCDR-Anforderungen (Business Continuity & Disaster Recovery) zu erfüllen. Sie können regelmäßige Notfallwiederherstellungsübungen ausführen, um die Einhaltung von Complianceanforderungen zu gewährleisten. Der virtuelle Computer wird mit den angegebenen Einstellungen in der ausgewählten Region repliziert, sodass Sie Ihre Anwendungen bei Ausfällen in der Quellregion wiederherstellen können.
* [Azure Traffic Manager][docs-traffic-manager] ist ein DNS-basierter Lastenausgleich, der Datenverkehr optimal auf Dienste in den globalen Azure-Regionen verteilt und gleichzeitig für Hochverfügbarkeit und kurze Reaktionszeiten sorgt.
* [Azure Load Balancer][docs-load-balancer] verteilt eingehenden Datenverkehr auf der Grundlage von definierten Regeln und Integritätstests. Ein Load Balancer sorgt für kurze Wartezeiten und hohen Durchsatz und kann auf Millionen von Flows für alle TCP- und UDP-Anwendungen skalieren. In diesem Szenario wird ein öffentlicher Load Balancer verwendet, um eingehenden Clientdatenverkehr auf der Webebene zu verteilen. Außerdem wird in diesem Szenario ein interner Lastenausgleich verwendet, um Datenverkehr von der Geschäftsebene auf den Back-End-SQL Server-Cluster zu verteilen.

### <a name="alternatives"></a>Alternativen

* Windows kann durch verschiedene andere Betriebssysteme ersetzt werden, da die Infrastruktur nicht vom Betriebssystem abhängig ist.
* [SQL Server für Linux][docs-sql-server-linux] kann den Back-End-Datenspeicher ersetzen.
* Die Datenbank kann durch eine beliebige Standard-Datenbankanwendung ersetzt werden.

## <a name="other-considerations"></a>Weitere Überlegungen

### <a name="scalability"></a>Skalierbarkeit

Sie können auf jeder Ebene virtuelle Computer hinzufügen oder entfernen, um Ihre Skalierungsanforderungen zu erfüllen. Da in diesem Szenario Load Balancer verwendet werden, können Sie einer Ebene weitere virtuelle Computer hinzufügen, ohne dass dies Auswirkungen auf die Anwendungsbetriebszeit hat.

Weitere Skalierbarkeitsthemen finden Sie im Azure Architecture Center in der [Checkliste für die Skalierbarkeit][scalability].

### <a name="security"></a>Sicherheit

Der gesamte Datenverkehr des virtuellen Netzwerks an die Front-End-Anwendungsebene wird durch Netzwerksicherheitsgruppen geschützt. Anhand von Regeln wird der Datenverkehrsfluss eingeschränkt, sodass nur die VM-Instanzen der Front-End-Anwendungsebene auf die Back-End-Datenbankebene zugreifen können. Auf der Geschäfts- oder Datenbankebene ist kein ausgehender Internetdatenverkehr zulässig. Es werden keine Ports für die direkte Remoteverwaltung geöffnet, um die Angriffsfläche zu reduzieren. Weitere Informationen finden Sie unter [Azure-Netzwerksicherheitsgruppen][docs-nsg].

Allgemeine Informationen zur Entwicklung sicherer Szenarien finden Sie in der [Dokumentation zur Azure-Sicherheit][security].

## <a name="pricing"></a>Preise

Wenn Sie die Notfallwiederherstellung für virtuelle Azure-Computer mit Azure Site Recovery konfigurieren, fallen regelmäßig folgende Gebühren an:

- Azure Site Recovery-Lizenzierungskosten pro virtuellem Computer.
- Kosten für ausgehenden Netzwerkdatenverkehr, um Datenänderungen auf den Datenträgern virtueller Quellcomputer in einer anderen Azure-Region zu replizieren. Azure Site Recovery verwendet eine integrierte Komprimierung, um den Datenübertragungsbedarf um etwa 50 Prozent zu verringern.
- Speicherkosten am Wiederherstellungsstandort. Diese entsprechen in der Regel dem Quellregionsspeicher plus dem Speicher, der ggf. zusätzlich benötigt wird, um die Wiederherstellungspunkte als Momentaufnahmen für die Wiederherstellung zu verwalten.

Wir haben einen [Beispielkostenrechner][calculator] für die Konfiguration der Notfallwiederherstellung für eine Anwendung mit drei Ebenen und sechs virtuellen Computern bereitgestellt. In dem Kostenrechner sind bereits alle Dienste vorkonfiguriert. Wenn Sie wissen möchten, welche Kosten für Ihren spezifischen Anwendungsfall entstehen, können Sie die entsprechenden Variablen anpassen, um eine Kostenschätzung zu erhalten.

<!-- links -->
[architecture]: ./media/arhitecture-disaster-recovery-multi-tier-app.png
[autoscaling]: /azure/architecture/best-practices/auto-scaling
[availability]: ../../checklist/availability.md
[resiliency]: /azure/architecture/resiliency/
[security]: /azure/security/
[scalability]: /azure/architecture/checklist/scalability
[docs-availability-zones]: /azure/availability-zones/az-overview
[docs-load-balancer]: /azure/load-balancer/load-balancer-overview
[docs-nsg]: /azure/virtual-network/security-overview
[docs-vmss]: /azure/virtual-machine-scale-sets/overview
[docs-sql-always-on]: /sql/database-engine/availability-groups/windows/overview-of-always-on-availability-groups-sql-server
[docs-vmss-autoscale]: /azure/virtual-machine-scale-sets/virtual-machine-scale-sets-autoscale-overview
[docs-vnet]: /azure/virtual-network/virtual-networks-overview
[docs-sql-server-linux]: /sql/linux/sql-server-linux-overview?view=sql-server-linux-2017
[docs-traffic-manager]: /azure/traffic-manager/
[docs-azure-site-recovery]: /azure/site-recovery/azure-to-azure-quickstart/
[docs-availability-sets]: /azure/virtual-machines/windows/manage-availability/
[calculator]: https://azure.com/e/6835332265044d6d931d68c917979e6d/
