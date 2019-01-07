---
title: 'Virtuelles Azure-Rechenzentrum: Eine Netzwerkperspektive'
description: In diesem Artikel erfahren Sie, wie Sie Ihr virtuelles Rechenzentrum in Azure erstellen.
author: tracsman
manager: rossort
tags: azure-resource-manager
ms.service: virtual-network
ms.date: 11/28/2018
ms.author: jonor
ms.openlocfilehash: 1d8a9e860ab1a66104dc4133eb5f22ffb4706b84
ms.sourcegitcommit: 5a3fa0bf35376bbe4a6dd668f2d7d44f9cf9c806
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/14/2018
ms.locfileid: "53411683"
---
# <a name="azure-virtual-datacenter-a-network-perspective"></a>Virtuelles Azure-Rechenzentrum: Eine Netzwerkperspektive

## <a name="overview"></a>Übersicht

Durch das Migrieren von lokalen Anwendungen zu Azure kommen Organisationen in den Genuss einer geschützten und kosteneffizienten Infrastruktur, obwohl die Anwendungen mit minimalen Änderungen migriert werden. Um die mit Cloud Computing mögliche Agilität und die Azure-Funktionen bestmöglich auszunutzen, müssen Unternehmen ihre Architekturen aber weiterentwickeln. 

Microsoft Azure stellt Dienste mit Hyperskalierung und Infrastruktur mit Funktionen und Zuverlässigkeit auf Unternehmensniveau bereit. Mit diesen Diensten und der Infrastruktur haben Sie viele Möglichkeiten in Bezug auf die Hybridkonnektivität, sodass Kunden die Wahl haben, ob sie über das öffentliche Internet oder über eine private Azure ExpressRoute-Verbindung darauf zugreifen möchten. Zudem bieten Microsoft-Partner erweiterte Funktionen in Form von Sicherheitsdiensten und virtuellen Geräten an, die für die Verwendung in Azure optimiert sind.

Kunden können über das Internet oder mit privater Netzwerkkonnektivität über Microsoft Azure ExpressRoute auf diese Clouddienste zugreifen. Mit der Microsoft Azure Platform können Kunden ihre Infrastruktur nahtlos auf die Cloud erweitern und Architekturen mit mehreren Ebenen erstellen. Zudem bieten Microsoft-Partner erweiterte Funktionen in Form von Sicherheitsdiensten und virtuellen Geräten an, die für die Verwendung in Azure optimiert sind.

## <a name="what-is-the-virtual-datacenter"></a>Was ist das virtuelle Rechenzentrum?

Bei ihrer Einführung war die Cloud im Wesentlichen eine Plattform zum Hosten von öffentlichen Anwendungen. Unternehmen begannen, den Nutzen der Cloud zu verstehen und interne Branchenanwendungen in die Cloud zu verlagern. Diese Arten von Anwendungen waren mit zusätzlicher Sicherheit, Zuverlässigkeit und Leistung sowie mit Kostenaspekten verbunden, die eine weitere Flexibilisierung in Bezug auf die Bereitstellung von Clouddiensten nötig machten. Dies ebnete nicht nur den Weg für neue Infrastruktur und Netzwerkdienste, mit denen für diese Flexibilität gesorgt wurde, sondern auch für neue Features in den Bereichen Skalierung, Notfallwiederherstellung und mehr.

## <a name="what-is-a-virtual-datacenter"></a>Was ist ein virtuelles Rechenzentrum?
Cloudlösungen wurden ursprünglich dazu entworfen, einzelne, relativ isolierte Anwendungen im öffentlichen Spektrum zu hosten. Dieser Ansatz hat sich für einige Jahre bewährt. Im Laufe der Zeit wurden jedoch die Vorteile von Cloudlösungen deutlich, und es wurden mehrere umfangreiche Workloads in der Cloud gehostet. Die Erfüllung der Anforderungen hinsichtlich der Sicherheit, Zuverlässigkeit, Leistung und Kosten von Bereitstellungen in einer oder mehreren Regionen entwickelte sich dadurch zu einem entscheidenden Faktor im Lebenszyklus des Clouddiensts.

Das **rote Kästchen** im folgenden Diagramm einer Cloudbereitstellung zeigt ein Beispiel für eine Sicherheitslücke. Das **gelbe Kästchen** stellt Möglichkeiten zur Optimierung von virtuellen Netzwerkgeräten (Network Virtual Appliance, NVA) in verschiedenen Workloads dar.

[![0]][0]

Das Konzept eines virtuellen Rechenzentrums (Virtual Data Center, VDC) entstand aus der Notwendigkeit, Rechenzentren zur Unterstützung von Unternehmensworkloads zu skalieren und gleichzeitig die Probleme zu bewältigen, die mit der Unterstützung umfangreicher Anwendungen in der Public Cloud einhergingen.

Die Implementierung eines virtuellen Rechenzentrums besteht nicht nur aus den Anwendungsworkloads in der Cloud, sondern umfasst auch das Netzwerk, die Sicherheit, Verwaltung und Infrastruktur (z.B. DNS und Verzeichnisdienste). Je mehr Workloads eines Unternehmens zu Azure migriert werden, desto wichtiger ist es, die unterstützende Infrastruktur und die Objekte zu bedenken, in denen diese Workloads platziert werden. Überlegungen im Vorfeld zur Strukturierung von Ressourcen können Hunderte von „Workload-Inseln“ vermeiden, die mit getrennt unabhängigen Anforderungen in Bezug auf Datenflüssen, Sicherheitsmodellen und Compliance verwaltet werden müssen.

Das Konzept eines virtuellen Rechenzentrums umfasst eine Reihe von Empfehlungen und bewährten Methoden für die Implementierung einer Sammlung mit separaten (aber verwandten) Entitäten, die über gemeinsame unterstützende Funktionen, Features und Infrastruktur verfügen. Wenn Sie Ihre Workloads aus Sicht des virtuellen Rechenzentrums begreifen, können Sie aufgrund von Skaleneffekten, optimierter Sicherheit durch die Zentralisierung von Komponenten und Datenflüssen sowie die Vereinfachung von Vorgängen, der Verwaltung und Konformitätsprüfungen Kosten sparen.

> [!NOTE]
> Virtuelle Rechenzentren sind aber **kein** diskretes Azure-Produkt, sondern eine bestimmte Kombination verschiedener Features und Funktionen, die Ihre Anforderungen erfüllen. Das virtuelle Rechenzentrum stellt ein bestimmtes Verständnis Ihrer Workloads und Azure-Nutzung dar, dessen Ziel es ist, Ihre Ressourcen und Möglichkeiten in der Cloud zu maximieren. Es handelt sich um einen modularen Ansatz zum Erstellen von IT-Diensten in Azure, bei dem die organisatorischen Rollen und Zuständigkeiten des Unternehmens respektiert werden.

Eine Implementierung eines virtuellen Rechenzentrums kann Unternehmen dabei helfen, Workloads und Anwendungen für die folgenden Szenarien in Azure auszuführen:

-   Hosten mehrerer verwandter Workloads
-   Migrieren von Workloads aus einer lokalen Umgebung zu Azure
-   Implementieren von gemeinsam genutzten oder zentralisierten Sicherheits- und Zugriffsanforderungen für verschiedene Workloads
-   Kombinieren von Azure DevOps und der zentralen IT für große Unternehmen

Um sämtliche Vorteile eines virtuellen Rechenzentrums nutzen zu können, ist eine zentrale Topologie (Hub und Spokes) mit verschiedenen Azure-Features notwendig: 

- [Microsoft Azure Virtual Network][VNet] 
- [Netzwerksicherheitsgruppen (NSGs)][NSG]
- [Peering in virtuellen Netzwerken][VNetPeering] 
- [Benutzerdefinierte Routen][UDR] (User-Defined Routes, UDRs)
- Azure-Identitätsdienste mit [rollenbasierter Zugriffssteuerung][RBAC] (Role-Based Access Control, RBAC) 
- Optional [Azure Firewall][AzFW], [Azure DNS][DNS], [Azure Front Door Service (AFD)][AFD] und [Azure Virtual WAN][vWAN]

Der Schlüssel zur Nutzung der Vorteile eines virtuellen Rechenzentrums ist eine zentrale Hub-and-Spoke-Netzwerktopologie mit einer Mischung aus Azure-Diensten und -Features:

* [Azure Virtual Network][VNet],
* [Netzwerksicherheitsgruppen][NSG],
* [Peering virtueller Netzwerke][VNetPeering],
* [Benutzerdefinierte Routen (User-Defined Routes, UDR)][UDR] und
* Azure-Identität mit [rollenbasierter Zugriffssteuerung (Role-Based Access Control, RBAC)][RBAC] sowie optional [Azure Firewall][AzFW], [Azure DNS][DNS], [Azure Front Door][AFD] und [Azure Virtual WAN][vWAN].

## <a name="who-should-implement-a-virtual-datacenter"></a>Wer sollte ein virtuelles Rechenzentrum implementieren?

Alle Azure-Kunden, die sich für die Einführung der Cloud entschieden haben, können von der effizienten Konfiguration eines Ressourcensatzes profitieren, der von allen Anwendungen gemeinsam genutzt wird. Je nach Größe können auch einzelne Anwendungen Vorteile aus den Mustern und den Komponenten ziehen, die zum Erstellen der Implementierung eines virtuellen Rechenzentrums verwendet werden.

Falls Ihre Organisation über eine zentrale IT-, Netzwerk-, Sicherheits- und/oder für die Konformität zuständige Complianceabteilung verfügt, kann die Implementierung eines virtuellen Rechenzentrums dabei helfen, Richtlinien zu erzwingen, Aufgaben zu trennen und die Einheitlichkeit der zugrunde liegenden gemeinsamen Komponenten sicherzustellen. Gleichzeitig haben Anwendungsteams so viel Freiheit und Kontrolle, wie es Ihren Anforderungen entspricht.

Organisationen, die auf DevOps setzen, können mit VDC-Konzepten auch autorisierte Segmente mit Azure-Ressourcen bereitstellen und sicherstellen, dass sie innerhalb dieser Gruppe (ein Abonnement oder eine Ressourcengruppe in einem allgemeinen Abonnement) die vollständige Kontrolle haben. Die Netzwerk- und Sicherheitsbegrenzungen bleiben hierbei aber konform, so wie dies durch eine zentrale Richtlinie in einem Hub-VNET und einer Ressourcengruppe definiert ist.

## <a name="considerations-for-implementing-a-virtual-datacenter"></a>Überlegungen zur Implementierung eines virtuellen Rechenzentrums

Beim Entwerfen der Implementierung eines virtuellen Rechenzentrums sind mehrere zentrale Punkte zu berücksichtigen:

### <a name="identity-and-directory-services"></a>Identitäts- und Verzeichnisdienste
Identitäts- und Verzeichnisdienste sind ein wichtiger Aspekt jedes Rechenzentrums, ganz gleich, ob lokal oder in der Cloud gehostet. Identität bezieht sich auf alle Aspekte des Zugriffs und der Autorisierung für Dienste innerhalb des virtuellen Rechenzentrums. Um sicherzustellen, dass nur autorisierte Benutzer und Prozesse auf Ihr Azure-Konto und Ihre Azure-Ressourcen zugreifen, verwendet Azure mehrere Typen von Anmeldeinformationen für die Authentifizierung. Dazu zählen Kennwörter für den Zugriff auf das Azure-Konto, kryptografische Schlüssel, digitale Signaturen und Zertifikate. 

### <a name="identity-and-directory-service"></a>Identitäts- und Verzeichnisdienst

Identitäts- und Verzeichnisdienste sind ein wichtiger Aspekt jedes Rechenzentrums, ganz gleich, ob lokal oder in der Cloud gehostet. Identität bezieht sich auf alle Aspekte des Zugriffs und der Autorisierung für Dienste innerhalb der Implementierung eines virtuellen Rechenzentrums. Um sicherzustellen, dass nur autorisierte Benutzer und Prozesse auf Ihr Azure-Konto und die entsprechenden Ressourcen zugreifen, verwendet Azure mehrere Typen von Anmeldeinformationen für die Authentifizierung. Dazu gehören Kennwörter (um auf das Azure-Konto zuzugreifen), kryptografische Schlüssel, digitale Signaturen und Zertifikate. [Microsoft Azure Multi-Factor Authentication (MFA)][MFA] ist eine zusätzliche Sicherheitsebene für den Zugriff auf Azure-Dienste. Azure MFA stellt anhand eine Reihe von einfachen Überprüfungsoptionen – Telefonanruf, SMS oder Benachrichtigung in einer mobilen App – eine sichere Authentifizierung bereit und ermöglicht es Kunden, die von ihnen bevorzugte Methode zu wählen.

Alle großen Unternehmen müssen einen Identitätsverwaltungsprozess definieren, der die Verwaltung der einzelnen Identitäten, deren Authentifizierung, Autorisierung, Rollen und Berechtigungen in der Implementierung des virtuellen Rechenzentrums beschreibt (einzeln oder übergreifend). Mit diesem Prozess sollte Sicherheit und Produktivität gesteigert und gleichzeitig Kosten, Ausfallzeiten und sich wiederholende manuelle Aufgaben verringert werden.

Unternehmen/Organisationen können eine anspruchsvolle Kombination von Diensten für verschiedene Branchen (Line-of-Business, LOB) erfordern, und Mitarbeiter haben häufig unterschiedliche Rollen zugleich, wenn sie an verschiedenen Projekten arbeiten. Das virtuelle Rechenzentrum erfordert eine gute Zusammenarbeit zwischen verschiedenen Teams mit bestimmten Rollendefinitionen, um Systeme verantwortungsbewusst mit geeigneter Governance aufzusetzen. Die Matrix der Zuständigkeiten, Zugriffsrechte und Berechtigungen kann komplex sein. Die Identitätsverwaltung im virtuellen Rechenzentrum wird über [*Azure Active Directory* (Azure AD)][AAD] und die rollenbasierte Zugriffssteuerung implementiert.

Ein Verzeichnisdienst ist eine freigegebene Informationsstruktur für die Suche, Verwaltung und Organisation von täglich verwendeten Elementen und Netzwerkressourcen. Diese Ressourcen können Volumes, Ordner, Dateien, Drucker, Benutzer, Gruppen, Geräte und andere Objekte enthalten. Jede Ressource im Netzwerk wird vom Verzeichnisserver als Objekt angesehen. Informationen zu einer Ressource werden als eine Auflistung von dieser Ressource oder diesem Objekt zugeordneten Attributen gespeichert.

Alle Microsoft-Onlinedienste für Unternehmen nutzen für Anmelde- und andere Identitätsanforderungen Azure Active Directory (Azure AD). Azure Active Directory ist eine umfassende, hochverfügbare Cloudlösung für die Identitäts- und Zugriffsverwaltung, die grundlegende Verzeichnisdienste, erweiterte Identitätsgovernance und Zugriffsverwaltung für Anwendungen kombiniert. Azure AD kann in eine lokale Active Directory-Instanz integriert werden, um einmaliges Anmelden für alle cloudbasierten und lokal gehosteten Anwendungen zu ermöglichen. Die Benutzerattribute der lokalen Active Directory-Instanz können automatisch mit Azure AD synchronisiert werden.

In einer Implementierung des virtuellen Rechenzentrums muss nicht ein einzelner globaler Administrator alle Berechtigungen zuweisen. Stattdessen können jeder spezifischen Abteilung, Gruppe von Benutzern oder Diensten im Verzeichnisdienst die erforderlichen Berechtigungen zum Verwalten ihrer eigenen Ressourcen innerhalb der Implementierung eines virtuellen Rechenzentrums zugewiesen werden. Das Strukturieren von Berechtigungen muss gut durchdacht sein. Zu viele Berechtigungen können die Leistungseffizienz beeinträchtigen, und zu wenige oder ungenau definierte Berechtigungen können Sicherheitsrisiken erhöhen. Die rollenbasierte Zugriffssteuerung in Azure begegnet diesem Problem, indem sie eine differenzierte Zugriffsverwaltung für die Ressourcen der Implementierung eines virtuellen Rechenzentrums ermöglicht.

#### <a name="security-infrastructure"></a>Sicherheitsinfrastruktur

Die Sicherheitsinfrastruktur bezieht sich auf die Trennung des Datenverkehrs im spezifischen VNET-Segment der Implementierung des virtuellen Netzwerks. Mit dieser Infrastruktur wird angegeben, wie der Ein- und Ausgang bei der Implementierung eines virtuellen Rechenzentrums gesteuert wird. Azure basiert auf einer mehrinstanzenfähigen Architektur. So wird nicht autorisierter oder unbeabsichtigter Datenverkehr zwischen Bereitstellungen mithilfe der Isolation virtueller Netzwerke (VNETs), Zugriffssteuerungslisten (ACLs), Load Balancern, IP-Filtern und Richtlinien zum Datenverkehrsfluss verhindert. Die Netzwerkadressenübersetzung (Network Address Translation, NAT) trennt den internen vom externen Netzwerkdatenverkehr.

Das Azure-Fabric ordnet Infrastrukturressourcen Mandantenworkloads zu und verwaltet die ein- und ausgehende Kommunikation von virtuellen Computern (Virtual Machines, VMs). Der Azure-Hypervisor erzwingt eine Speicher- und Prozesstrennung zwischen virtuellen Computern und routet den Netzwerkdatenverkehr sicher zu den Gastbetriebssystem-Mandanten.

#### <a name="connectivity-to-the-cloud"></a>Verbindung mit der Cloud

Eine Implementierung des virtuellen Rechenzentrums erfordert Konnektivität mit externen Netzwerken, um Dienste für Kunden, Partner bzw. interne Benutzer bereitzustellen. Es ist nicht nur Konnektivität mit dem Internet, sondern auch mit lokalen Netzwerken und Rechenzentren erforderlich.

Kunden steuern mithilfe von [Azure Firewall][AzFW] oder anderen virtuellen Netzwerkgeräten, angepassten Routingrichtlinien mit [benutzerdefinierten Routen][UDR] und der Netzwerkfilterung mit [Netzwerksicherheitsgruppen][NSG], welche Dienste Zugriff auf das öffentliche Internet haben und über das öffentliche Internet zugänglich sind. Wir empfehlen, alle Ressourcen mit Internetzugriff zusätzlich durch [Azure DDoS Protection Standard][DDOS] zu schützen.

Unternehmen müssen ihre Implementierungen des virtuellen Rechenzentrums unter Umständen mit lokalen Rechenzentren oder anderen Ressourcen verbinden. Diese Konnektivität zwischen Azure und lokalen Netzwerken ist ein entscheidender Aspekt beim Entwerfen einer effektiven Architektur. Unternehmen haben zwei Möglichkeiten, diese Verknüpfung einzurichten: Übertragung über das Internet und/oder über private Direktverbindungen.

Eine [**Azure-Site-to-Site-VPN-Verbindung**][VPN] ist ein Dienst für eine Verbindung über das Internet zwischen lokalen Netzwerken und einer Implementierung des virtuellen Rechenzentrums, die über sichere, verschlüsselte Verbindungen (IPsec-/IKE-Tunnel) hergestellt wird. Azure-Standort-zu-Standort-Verbindungen sind flexibel und können schnell erstellt werden. Außerdem sind keine weiteren Bereitstellungen erforderlich, da alle Verbindungen über das Internet hergestellt werden.

Der Netzwerkdienst [**Azure Virtual WAN**][vWAN] bietet für eine große Anzahl von VPN-Verbindungen optimierte und automatisierte Konnektivität zwischen Branches über Azure. Per Virtual WAN können Sie Branchgeräte für die Kommunikation mit Azure verbinden und konfigurieren. Die Verbindungsherstellung und Konfiguration kann entweder manuell oder mit Geräten eines bevorzugten Anbieters über einen Virtual WAN-Partner durchgeführt werden. Die Nutzung von Geräten eines bevorzugten Anbieters ermöglicht eine einfache Verwendung, die Vereinfachung der Verbindungsherstellung und die Konfigurationsverwaltung. Über das integrierte Azure WAN-Dashboard können Sie anhand der Problembehandlung sofort Erkenntnisse gewinnen, um Zeit zu sparen, und auf einfache Weise umfangreiche Site-to-Site-Verbindungen anzeigen.

[**ExpressRoute**][ExR] ist ein Azure-Konnektivitätsdienst, mit dem private Verbindungen zwischen einer VDC-Implementierung und lokalen Netzwerken möglich sind. ExpressRoute-Verbindungen erfolgen nicht über das öffentliche Internet und bieten eine höhere Sicherheit, größere Zuverlässigkeit und schnellere Geschwindigkeit (bis zu 10 Gbps) bei konstanter Latenz. ExpressRoute ist sehr nützlich für Implementierungen des virtuellen Rechenzentrums, da ExpressRoute-Kunden von den Konformitätsregeln privater Verbindungen profitieren können. Mit [ExpressRoute Direct][ExRD] können Kunden mit höherem Bandbreitenbedarf eine direkte 100-GBit/s-Verbindung mit Microsoft-Routern herstellen.

Für die Bereitstellung von ExpressRoute-Verbindungen ist in der Regel ein ExpressRoute-Dienstanbieter erforderlich. Kunden, die schnell die Arbeit aufnehmen müssen, verwenden anfangs üblicherweise Site-to-Site-VPN-Verbindungen für die Konnektivität zwischen der Implementierung eines virtuellen Rechenzentrums und den lokalen Ressourcen. Anschließend führen sie die Migration zu einer ExpressRoute-Verbindung durch, wenn der Dienstanbieter die Einrichtung der physischen Verbindung abgeschlossen hat.

#### <a name="connectivity-within-the-cloud"></a>*Konnektivität in der Cloud*

[VNETs][VNet] und [VNET-Peering][VNetPeering] sind die grundlegenden Netzwerkkonnektivitätsdienste innerhalb der Implementierung eines virtuellen Rechenzentrums. Ein VNET gewährleistet eine natürliche Isolationslinie zwischen den Ressourcen in einem virtuellen Rechenzentrum, und VNET-Peering ermöglicht die Kommunikation zwischen verschiedenen VNETs innerhalb derselben oder sogar verschiedener Azure-Regionen. Bei der Steuerung des Datenverkehrs innerhalb eines VNETs und VNET übergreifend müssen einige Sicherheitsregeln erfüllt werden, die in Zugriffssteuerungslisten ([Netzwerksicherheitsgruppen][NSG]), [virtuelle Netzwerkgerät][NVA] und benutzerdefinierten Routingtabellen ([UDR][UDR]) angegeben sind.

## <a name="virtual-datacenter-overview"></a>Übersicht über das virtuelle Rechenzentrum

### <a name="topology"></a>Topologie

_Hub-and-Spoke_ ist ein Modell zum Entwerfen der Netzwerktopologie für die Implementierung eines virtuellen Rechenzentrums. 

[![1]][1]

Ein Hub ist die zentrale Netzwerkzone, die den ein- oder ausgehenden Datenverkehr zwischen den verschiedenen Zonen steuert und überprüft: im Internet, lokal und in den Spokes. Die Hub-Spoke-Topologie stellt für die IT-Abteilung eine effektive Möglichkeit zum Erzwingen von Sicherheitsrichtlinien an einem zentralen Ort dar. Außerdem verringert sie das Risiko von Fehlkonfigurationen und Offenlegungen.

Der Hub enthält die allgemeinen Dienstkomponenten, die von den Spokes genutzt werden. Im Folgenden finden Sie einige Beispiele für allgemeine zentrale Dienste:

-   Die Windows Active Directory-Infrastruktur, die für die Benutzerauthentifizierung von Drittanbietern, die über nicht vertrauenswürdige Netzwerke zugreifen, erforderlich ist, bevor sie Zugriff auf die Workloads im Spoke erhalten. Dies beinhaltet die zugehörigen Active Directory-Verbunddienste (Active Directory Federation Services, AD FS).
-   Ein DNS-Dienst, mit dem die Namen der Workloads in den Spokes aufgelöst werden, um lokal und über das Internet auf Ressourcen zuzugreifen, wenn [Azure DNS][DNS] nicht verwendet wird.
-   Eine Public Key-Infrastruktur (PKI), mit der das einmalige Anmelden in Workloads implementiert wird.
-   Flusssteuerung des TCP- und UDP-Datenverkehrs zwischen den Spoke-Netzwerkzonen und dem Internet.
-   Flusssteuerung zwischen den Spokes und dem lokalen Netzwerk.
-   Flusssteuerung zwischen zwei Spokes (sofern erforderlich).

Das virtuelle Rechenzentrum reduziert die Gesamtkosten mithilfe der gemeinsamen Hubinfrastruktur, die mehrere Spokes umfasst.

Die Rolle eines jeden Spokes kann über verschiedene Arten von Workloads gehostet werden. Die Spokes können auch einen modularen Ansatz für wiederholbare Bereitstellungen der gleichen Workloads bieten. Beispiele hierfür sind Entwicklung und Tests, Benutzerakzeptanztests (User Acceptance Testing, UAT), Präproduktion und Produktion. Mit den Spokes können auch verschiedene Gruppen in Ihrer Organisation getrennt und aktiviert werden. Ein Beispiel hierfür sind Azure DevOps-Gruppen. Innerhalb eines Spokes ist es möglich, eine einfache Workload oder komplexe Workloads mit mehreren Ebenen mit einer Datenverkehrssteuerung zwischen den Ebenen bereitzustellen.

#### <a name="subscription-limits-and-multiple-hubs"></a>Abonnementlimit und mehrere Hubs

In Azure wird jede Komponente, unabhängig vom Typ, in einem Azure-Abonnement bereitgestellt. Die Isolation von Azure-Komponenten in verschiedenen Azure-Abonnements kann die Anforderungen verschiedener Branchen erfüllen, z.B. das Einrichten unterschiedlicher Zugriffs- und Autorisierungsebenen.

Ein einzelne Implementierung eines virtuellen Rechenzentrums kann zentral auf eine große Anzahl von Spokes hochskaliert werden, wobei aber wie bei jedem IT-System gewisse Plattformbeschränkungen gelten. Die Hub-Bereitstellung ist an ein bestimmtes Azure-Abonnement gebunden, das Einschränkungen hat, z.B. eine maximale Anzahl von VNET-Peerings. Weitere Informationen finden Sie unter [Einschränkungen für Azure-Abonnements und Dienste, Kontingente und Einschränkungen][Limits]. In Fällen, in denen diese Beschränkungen ein Problem darstellen, kann die Architektur weiter zentral hochskaliert werden, indem das Modell von einem einzelnen Hub und Spoke auf ein Cluster von Hub und Spokes erweitert wird. Mehrere Hubs in einer oder mehreren Azure-Regionen können mithilfe von VNET-Peering, ExpressRoute, Virtual WAN oder einem Site-to-Site-VPN miteinander verbunden werden.

[![2]][2]

Die Einführung von mehreren Hubs erhöht die Kosten und den Verwaltungsaufwand für das System. Dieser Aufwand lässt sich nur durch die Skalierbarkeit (z. B. Systemeinschränkungen oder Redundanz) und die regionale Replikation (z. B. Leistung für Endbenutzer oder Notfallwiederherstellung) rechtfertigen. In Szenarios, die mehrere Hubs erfordern, sollten alle Hubs die gleichen Dienste anbieten, um die Vorgänge nicht zu erschweren.

#### <a name="interconnection-between-spokes"></a>Verbindung zwischen Spokes

Innerhalb eines einzelnen Spokes ist es möglich, komplexe Workloads mit mehreren Ebenen zu implementieren. Konfigurationen mit mehreren Ebenen können mit Subnetzen (eines für jede Ebene) im gleichen VNET implementiert werden. Dabei werden die Flüsse mit Netzwerksicherheitsgruppen gefiltert.

Ein Architekt möchte vielleicht eine Workload mit mehreren Ebenen in mehreren virtuellen Netzwerken bereitstellen. Mittels Peering in virtuellen Netzwerken können Spokes Verbindungen mit anderen Spokes im gleichen Hub oder in anderen Hubs herstellen. Ein typisches Beispiel für dieses Szenario ist die Platzierung von Anwendungsverarbeitungsservern in einem Spoke oder virtuellen Netzwerk, während die Datenbank in einem anderen Spoke oder virtuellen Netzwerk bereitgestellt wird. In diesem Fall ist es einfach, die Spokes mittels Peering in virtuellen Netzwerken zu verbinden und den Hub dabei zu umgehen. Die Architektur und Sicherheit sollten sorgfältig überprüft werden, um sicherzustellen, dass bei der Umgehung des Hubs keine wichtigen Sicherheits- oder Überwachungspunkte umgangen werden, die nur im Hub vorhanden sind.

[![3]][3]

Spokes können auch mit einem Spoke verbunden werden, der als Hub fungiert. Bei diesem Ansatz wird eine Hierarchie mit zwei Ebenen erstellt: Der Spoke auf der höheren Ebene (Ebene 0) wird der Hub des unteren Spokes (Stufe 1) in der Hierarchie. Die Spokes der Implementierung eines virtuellen Rechenzentrums sind zum Weiterleiten des Datenverkehrs an den zentralen Hub erforderlich, damit der Datenverkehr an sein Ziel im lokalen Netzwerk oder im öffentlichen Internet geleitet werden kann. Eine Architektur mit zwei Hub-Ebenen führt komplexes Routing ein, das die Vorteile einer einfachen Hub-Spoke-Beziehung aufhebt.

Obwohl Azure komplexe Topologien zulässt, ist eines der wesentlichen Prinzipien virtueller Rechenzentren die Wiederholbarkeit und Einfachheit. Um den Verwaltungsaufwand zu minimieren, empfehlen wir den einfachen Hub-Spoke-Entwurf als Referenzarchitektur für virtuelle Rechenzentren.

### <a name="components"></a>Komponenten

Das virtuelle Rechenzentrum besteht aus vier grundlegenden Komponententypen: **Infrastruktur**, **Umkreisnetzwerke**, **Workloads** und **Überwachung**.

Jeder Komponententyp besteht aus verschiedenen Azure-Funktionen und -Ressourcen. Ihre Implementierung des virtuellen Rechenzentrums besteht aus Instanzen verschiedener Komponententypen und mehreren Varianten des gleichen Komponententyps. Angenommen, Sie haben viele verschiedene, logisch getrennte Workload-Instanzen, die verschiedene Anwendungen darstellen. Aus diesen verschiedenen Komponententypen und Instanzen erstellen Sie letztendlich das virtuelle Rechenzentrum.

[![4]][4]

Die obige allgemeine konzeptionelle Architektur des virtuellen Rechenzentrums zeigt verschiedene Komponententypen, die in unterschiedlichen Zonen der Hub-and-Spoke-Topologie verwendet werden. Die Abbildung zeigt die Infrastrukturkomponenten in verschiedenen Teilen der Architektur.

Es ist im Allgemeinen eine bewährte Methode, gruppenbasierte Zugriffsrechte und Berechtigungen zu verwenden. Die Nutzung von Gruppen anstelle von einzelnen Benutzern vereinfacht die Wartung von Zugriffsrichtlinien, indem eine einheitliche teamübergreifende Verwaltung ermöglicht wird.  Darüber hinaus kommt es zu einer geringeren Anzahl von Konfigurationsfehlern. Das Zuweisen und Entfernen von Benutzern zu und aus den entsprechenden Gruppen erleichtert die Aktualisierung der Berechtigungen von bestimmten Benutzern.

Der Name jeder Rollengruppe sollte ein eindeutiges Präfix aufweisen. Anhand dieses Präfixes kann einfach ermittelt werden, welcher Gruppe eine Workload zugeordnet ist. Einer Workload, die einen Authentifizierungsdienst hostet, könnten beispielsweise Gruppen namens **AuthServiceNetOps**, **AuthServiceSecOps**, **AuthServiceDevOps** und **AuthServiceInfraOps** zugewiesen sein. Für zentrale Rollen oder Rollen, die in keinem Zusammenhang mit einem bestimmten Dienst stehen, könnte das Präfix **Corp** verwendet werden. Ein Beispiel hierfür ist **CorpNetOps**.

Viele Organisationen verwenden in etwa die folgenden Gruppen, um Rollen bereitzustellen:

-   Die zentrale IT-Gruppe **Corp** verfügt über die Besitzrechte zum Steuern von Infrastrukturkomponenten. Beispiele hierfür sind Netzwerke und die Sicherheit. Der Gruppe muss über die Rolle „Mitwirkender“ für das Abonnement, die Kontrolle über den Hub und die Rechte eines Mitwirkenden des virtuellen Netzwerks in den Spokes verfügen. Große Unternehmen teilen diese Verwaltungsaufgaben häufig zwischen mehreren Teams auf, beispielsweise zwischen einer Gruppe für Netzwerkvorgänge **CorpNetOps**, die sich ausschließlich um den Netzwerkbetrieb kümmert, und einer Gruppe für Sicherheitsvorgänge **CorpSecOps**, die für die Firewall- und Sicherheitsrichtlinien verantwortlich ist. In diesem speziellen Fall müssen zwei unterschiedliche Gruppen für die Zuweisung dieser benutzerdefinierten Rollen erstellt werden.
-   Die für die Entwicklung und Tests zuständige Gruppe **AppDevOps** ist für die Bereitstellung von App- oder Dienstworkloads verantwortlich. Diese Gruppe übernimmt die Rolle des VM-Mitwirkenden für IaaS-Bereitstellungen oder eine oder mehrere Rollen von PaaS-Mitwirkenden. Informationen dazu finden Sie unter [Integrierte Rollen für die rollenbasierte Zugriffssteuerung in Azure][Roles]. Optional benötigt das Entwicklungs- und Testteam möglicherweise Einblick in Sicherheitsrichtlinien, Netzwerksicherheitsgruppen und Routingrichtlinien sowie benutzerdefinierte Routen im Hub oder einem bestimmte Spoke. Zusätzlich zur Rolle des Mitwirkenden für Workloads benötigt diese Gruppe auch die Rolle des Netzwerklesers.
-   Die Betriebs- und Wartungsgruppe **CorpInfraOps** oder **AppInfraOps** ist für die Verwaltung von Workloads in der Produktion zuständig. Diese Gruppe muss ein Mitwirkender des Abonnements von Workloads in jedem Produktionsabonnement sein. Manche Organisationen sollten zudem prüfen, ob sie ein zusätzliches Team für den Eskalationssupport mit der Rolle des Mitwirkenden des Abonnements in der Produktion und im zentralen Hub-Abonnement benötigen. Diese zusätzliche Gruppe behebt potenzielle Konfigurationsprobleme in der Produktionsumgebung.

Das virtuelle Rechenzentrum ist so ausgelegt, dass es für Gruppen, die für zentrale IT-Gruppen zur Verwaltung des Hubs erstellt wurden, entsprechende Gruppen auf Workloadebene gibt. Zusätzlich zur Verwaltung der Hub-Ressourcen kann nur die zentrale IT-Gruppe den externen Zugriff und die Berechtigungen der obersten Ebene für das Abonnement steuern. Über Workloadgruppen können Ressourcen und Berechtigungen des zugehörigen VNET ebenfalls unabhängig von der zentralen IT gesteuert werden.

Das virtuelle Rechenzentrum ist partitioniert, damit mehrere Projekte in unterschiedlichen Branchenanwendungen sicher gehostet werden können. Alle Projekte erfordern unterschiedliche isolierte Umgebungen (Dev, UAT, Produktion). Einzelne Azure-Abonnements für jede dieser Umgebungen stellen natürliche Isolationsstufen bereit.

[![5]][5]

Das obige Diagramm veranschaulicht die Beziehung zwischen den Projekten, Benutzern und Gruppen einer Organisation und den Umgebungen, in denen die Azure-Komponenten bereitgestellt werden.

In der Regel ist eine Umgebung (oder Ebene) in der IT ein System, in dem mehrere Anwendungen bereitgestellt und ausgeführt werden. Große Unternehmen verwenden eine Entwicklungsumgebung (in der Änderungen ursprünglich vorgenommen und getestet werden) und eine Produktionsumgebung (die die Endbenutzer verwenden). Diese Umgebungen werden häufig durch mehrere Stagingumgebungen getrennt, um die Bereitstellung (Rollout), Tests und den Rollback in Phasen zu ermöglichen, falls Probleme auftreten. Bereitstellungsarchitekturen unterscheiden sich erheblich. In der Regel wird aber dem Grundmuster (Entwicklung (DEV) = Beginn, Produktion (PROD) = Ende) gefolgt.

Eine übliche Architektur für diese Arten von Umgebungen mit mehreren Ebenen besteht aus einer Azure DevOps-Umgebung für die Entwicklung und Tests, einer UAT-Umgebung für das Staging und einer Produktionsumgebung. Organisationen können einzelne oder mehrere Azure AD-Mandanten nutzen, um Zugriff und Rechte auf diese Umgebungen zu definieren. Das obige Diagramm veranschaulicht die Verwendung von zwei verschiedenen Azure AD-Mandanten: ein Mandant für Azure DevOps und Benutzerakzeptanztests und ein weiterer Mandant ausschließlich für die Produktion.

Wenn mehrere Azure AD-Mandanten vorhanden sind, müssen die Umgebungen getrennt werden. Für den Zugriff auf einen anderen Azure AD-Mandanten muss sich dieselbe Benutzergruppe (z. B. die zentrale IT) mit einem anderen URI authentifizieren, um die Rollen oder Berechtigungen der Azure DevOps-Umgebung oder der Produktionsumgebung eines Projekts zu ändern. Die Verwendung unterschiedlicher Benutzerauthentifizierungen für den Zugriff auf verschiedene Umgebungen reduziert das Risiko möglicher Ausfälle und anderer Probleme durch menschliches Versagen.

#### <a name="component-type-infrastructure"></a>Komponententyp: Infrastruktur

In diesem Komponententyp befindet sich der Großteil der unterstützenden Infrastruktur. Dort verbringen auch die zentrale IT-Sicherheit und/oder die Complianceteams die meiste Zeit.

[![6]][6]

Infrastrukturkomponenten stellen eine Verbindung für die unterschiedlichen Komponenten der Implementierung eines virtuellen Rechenzentrums bereit und sind sowohl auf dem Hub als auch auf den Spokes vorhanden. Die Verantwortung für die Verwaltung und Wartung der Infrastrukturkomponenten wird in der Regel der zentralen IT und/oder dem Sicherheitsteam zugewiesen.

Eine der wichtigsten Aufgaben des IT-Infrastrukturteams ist die Gewährleistung der Konsistenz von IP-Adressenschemata im gesamten Unternehmen. Der private IP-Adressraum, der einer Implementierung eines virtuellen Rechenzentrums zugewiesen ist, muss konsistent sein und darf sich NICHT mit privaten IP-Adressen in Ihren lokalen Netzwerken überlappen.

Obwohl die Netzwerkadressenübersetzung (Network Address Translation, NAT) in den lokalen Edgeroutern oder in Azure-Umgebungen IP-Adressenkonflikte vermeiden kann, bedeutet sie Komplikationen für Ihre Infrastrukturkomponenten. Die Vereinfachung der Verwaltung ist eines der wichtigsten Ziele des virtuellen Rechenzentrums. Aus diesem Grund ist es nicht empfehlenswert, die Netzwerkadressenübersetzung zur Behandlung von IP-Problemen zu verwenden.

Infrastrukturkomponenten bieten die folgenden Funktionen:

-   [**Identitäts-und Verzeichnisdienste**][AAD]. Der Zugriff auf jeden Ressourcentyp in Azure wird durch eine in einem Verzeichnisdienst gespeicherte Identität gesteuert. Der Verzeichnisdienst speichert nicht nur die Liste der Benutzer, sondern auch die Zugriffsrechte für Ressourcen in einem bestimmten Azure-Abonnement. Diese Dienste können rein cloudbasiert sein, oder sie können mit einer lokalen Identität in Active Directory synchronisiert werden.
-   [**Virtuelle Netzwerke**][VPN]. Virtuelle Netzwerke sind eine der Hauptkomponenten des virtuellen Rechenzentrums und ermöglichen es Ihnen, eine Isolationsgrenze für den Datenverkehr auf der Azure-Plattform zu erstellen. Ein virtuelles Netzwerk besteht aus einem einzelnen oder mehreren virtuellen Netzwerksegmenten, die jeweils ein bestimmtes Präfix für IP-Netzwerke (ein Subnetz) haben. Das virtuelle Netzwerk definiert einen internen Umkreisbereich, in dem virtuelle IaaS-Computer und PaaS-Dienste private Kommunikation herstellen können. Virtuelle Computer (und PaaS-Dienste) in einem virtuellen Netzwerk können nicht direkt mit virtuellen Computern (und PaaS-Diensten) in anderen virtuellen Netzwerken kommunizieren, selbst wenn beide virtuelle Netzwerke vom gleichen Kunden im selben Abonnement erstellt wurden. Isolation ist eine wichtige Eigenschaft, mit der sichergestellt wird, dass VMs und die Kommunikation von Kunden innerhalb eines virtuellen Netzwerks privat bleiben.
-   [**UDR**][UDR]. Datenverkehr in einem virtuellen Netzwerk wird standardmäßig basierend auf der Systemroutingtabelle weitergeleitet. Eine benutzerdefinierte Route ist eine benutzerdefinierte Routingtabelle, die von Netzwerkadministratoren einem oder mehreren Subnetzen zum Überschreiben des Verhaltens der Systemroutingtabelle und zum Definieren eines Kommunikationspfads innerhalb eines virtuellen Netzwerks zugeordnet werden können. UDRs gewährleisten, dass ausgehender Datenverkehr aus dem Spoke durch einen bestimmten benutzerdefinierten virtuellen Computer und/oder virtuelle Netzwerkgeräte und Load Balancer im Hub und in den Spokes geleitet wird.
-   [**NSG**][NSG]. Eine Netzwerksicherheitsgruppe (NSG) ist eine Liste von Sicherheitsregeln, die als Datenverkehrfilter für IP-Quellen, IP-Ziele, Protokolle, IP-Quellports und IP-Zielports fungieren. Die NSG kann auf ein Subnetz, auf eine virtuelle Netzwerkkarte (NIC), die mit einer Azure-VM verknüpft ist, oder beide angewendet werden. Die Netzwerksicherheitsgruppen sind entscheidend für die Implementierung einer korrekten Flusssteuerung im Hub und den Spokes. Das Maß an Sicherheit, das die NSGs bieten, hängt davon ab, welche Ports Sie öffnen und zu welchem Zweck. Kunden sollten zusätzliche Filter für jede VM mit hostbasierten Firewalls anwenden, z.B. IPtables oder die Windows-Firewall.
-   [**DNS**][DNS]. Die Namensauflösung von Ressourcen in den VNETs einer VDC-Implementierung wird über das Domain Name System (DNS) bereitgestellt. Azure bietet DNS-Dienste für [öffentliche][DNS] und [private][PrivateDNS] Namensauflösung. Private Zonen bieten Namensauflösung in einem virtuellen Netzwerk sowie zwischen virtuellen Netzwerken. Sie können private Zonen einrichten, die sich nicht nur über virtuelle Netzwerke in derselben Region erstrecken, sondern auch zwischen Regionen und Abonnements. Für die öffentliche Auflösung bietet Azure DNS einen Hostingdienst für DNS-Domänen, der die Namensauflösung mithilfe von Microsoft Azure-Infrastruktur durchführt. Durch das Hosten Ihrer Domänen in Azure können Sie Ihre DNS-Einträge mithilfe der gleichen Anmeldeinformationen, APIs, Tools und Abrechnung wie für die anderen Azure-Dienste verwalten.
-   [**Abonnement**][SubMgmt] und [**Ressourcengruppenverwaltung**][RGMgmt]. Ein Abonnement definiert eine natürliche Grenze, um mehrere Ressourcengruppen in Azure zu erstellen. Ressourcen in einem Abonnement werden in logischen Containern namens Ressourcengruppen zusammen assembliert. Die Ressourcengruppe stellt eine logische Gruppe dar, in der die Ressourcen einer VDC-Implementierung organisiert werden.
-   [**RBAC**][RBAC]. Über RBAC ist es möglich, die Organisationsrolle zusammen mit der Zugriffsberechtigung für bestimmte Azure-Ressourcen zuzuordnen und gleichzeitig Benutzer auf eine bestimmte Teilmenge von Aktionen zu beschränken. Mit RBAC können Sie Zugriff gewähren, indem Sie Benutzern, Gruppen und Anwendungen innerhalb des relevanten Bereichs die entsprechende Rolle zuweisen. Der Bereich einer Rollenzuweisung kann ein Azure-Abonnement, eine Ressourcengruppe oder eine einzelne Ressource sein. RBAC ermöglicht die Vererbung von Berechtigungen. Mit einer Rolle, die einem übergeordneten Bereich zugewiesen ist, wird außerdem der Zugriff auf die darin enthaltenen untergeordneten Elemente gewährt. Mit RBAC können Sie Aufgaben verteilen und Benutzern nur den Zugriff gewähren, den sie zur Ausführung ihrer Aufgaben benötigen. Gestatten Sie z.B. mit RBAC einem Mitarbeiter die Verwaltung virtueller Computer in einem Abonnement, während ein anderer im gleichen Abonnement SQL-Datenbanken verwalten kann.
-   [**VNET-Peering**][VNetPeering]. Die grundlegende Funktion für das Erstellen der Infrastruktur des virtuellen Rechenzentrums ist das VNET-Peering. Hierbei werden zwei virtuelle Netzwerke (VNETs) in der gleichen Region über das Netzwerk des Azure-Rechenzentrums oder regionsübergreifend über den weltweiten Azure-Backbone verbunden.

#### <a name="component-type-perimeter-networks"></a>Komponententyp: Umkreisnetzwerke

Mit den Komponenten eines [Umkreisnetzwerks][DMZ] können Sie Netzwerkkonnektivität zwischen Ihren lokalen oder physischen Rechenzentrumsnetzwerken und darüber hinaus Konnektivität mit dem Internet bereitstellen. Dafür bringen Ihre Netzwerk- und Sicherheitsteams wahrscheinlich den Großteil ihrer Zeit auf.

Eingehende Pakete sollten durch die Sicherheitsgeräte im Hub geleitet werden, bevor sie die Back-End-Server in den Spokes erreichen. Beispiele für diese Sicherheitsgeräte sind die Firewall, IDS und IPS. Von den Workloads an das Internet gesendete Pakete sollten ebenfalls über die Sicherheitsgeräte im Umkreisnetzwerk geleitet werden, bevor sie das Netzwerk verlassen. Dieser Fluss dient der Richtlinienerzwingung, Überprüfung und Überwachung.

Umkreisnetzwerk-Komponenten stellen folgende Funktionen bereit:

-   [Virtuelle Netzwerke][VNet], [benutzerdefinierte Routen][UDR] und [Netzwerksicherheitsgruppen][NSG]
-   [Virtuelle Netzwerkgeräte][NVA]
-   [Azure Load Balancer][ALB]
-   [Azure Application Gateway][AppGW] und [Web Application Firewall (WAF)][WAF]
-   [Öffentliche IP-Adressen][PIP]
-   [Azure Front Door Service][AFD]
-   [Azure Firewall][AzFW]

In der Regel sind die zentralen IT- und Sicherheitsteams für die Anforderungsdefinition und die Vorgänge in den Umkreisnetzwerken verantwortlich.

[![7]][7]

Das obige Diagramm veranschaulicht die Erzwingung von zwei Umkreisnetzwerken mit Zugriff auf das Internet und ein lokales Netzwerk, die sich beide in den DMZ- und vWAN-Hubs befinden. Im DMZ-Hub kann das Umkreisnetzwerk zum Internet zur Unterstützung einer großen Anzahl von Branchenanwendungen mit mehreren Farmen von Web Application Firewalls (WAFs) und/oder Azure-Firewalls zentral hochskaliert werden. Im vWAN-Hub wird je nach Bedarf per VPN oder ExpressRoute eine hochgradig skalierbare Konnektivität zwischen Branches sowie zwischen Branches und Azure bereitgestellt.

[**Virtuelle Netzwerke**][VNet]. Der Hub basiert normalerweise auf einem virtuellen Netzwerk mit mehreren Subnetzen, um verschiedene Arten von Diensten zu hosten, die den Internetdatenverkehr über virtuelle Netzwerkgeräte, WAFs und Azure Application Gateway-Instanzen filtern und überprüfen.

[**UDR**][UDR]: Mit UDRs können Kunden Firewalls, IDS/IPS und andere virtuelle Geräte bereitstellen und den Netzwerkdatenverkehr über diese Sicherheitsgeräte routen, um Sicherheitsrichtlinien zu erzwingen und die Überprüfung und Überwachung durchzuführen. UDRs können sowohl auf dem Hub als auch auf den Spokes erstellt werden, um sicherzustellen, dass Datenverkehr durch die spezifischen benutzerdefinierten VMs, virtuellen Netzwerkgeräte und Lastenausgleichsmodule geleitet wird, die von einer Implementierung eines virtuellen Rechenzentrums verwendet werden. Um zu gewährleisten, dass der von virtuellen Computern generierte Datenverkehr im Spoke zu den richtigen virtuellen Geräten geleitet wird, muss ein UDR in den Subnetzen des Spokes festgelegt werden, indem die Front-End-IP-Adresse des internen Load Balancers als nächster Hop festgelegt wird. Der interne Load Balancer verteilt den internen Datenverkehr auf die virtuellen Geräte (Back-End-Pool des Load Balancers).

[**Azure Firewall**][AzFW] ist ein verwalteter, cloudbasierter Netzwerksicherheitsdienst, der Ihre Azure Virtual Network-Ressourcen schützt. Es ist eine vollständig zustandsbehaftete Firewall-as-a-Service mit integrierter Hochverfügbarkeit und uneingeschränkter Cloudskalierbarkeit. Sie können Richtlinien zur Anwendungs- und Netzwerkkonnektivität übergreifend für Abonnements und virtuelle Netzwerke zentral erstellen, erzwingen und protokollieren. Azure Firewall verwendet eine statische öffentliche IP-Adresse für Ihre virtuellen Netzwerkressourcen. Dadurch können externe Firewalls Datenverkehr aus Ihrem virtuellen Netzwerk identifizieren. Der Dienst ist für Protokollierung und Analyse vollständig in Azure Monitor integriert.

[**Virtuelle Netzwerkgeräte**][NVA]. Im Hub wird das Umkreisnetzwerk mit Zugriff auf das Internet normalerweise über eine Azure Firewall-Instanz oder eine Farm von Firewalls oder Web Application Firewall (WAF) verwaltet.

In verschiedenen Branchen werden häufig viele Webanwendungen verwendet. Diese Anwendungen weisen häufig unterschiedliche Sicherheitsrisiken und Exploits auf. Eine Web Application Firewall ist ein spezielles Produkt, das Angriffe auf Webanwendungen (HTTP/HTTPS) genauer erkennt als eine generische Firewall. Verglichen mit herkömmlichen Firewalls besitzen WAFs einen Satz von bestimmten Features, um den internen Webserver vor Bedrohungen zu schützen.

Azure Firewall oder eine NVA-Firewall wird jeweils über eine gemeinsame Verwaltungsebene mit einem Satz von Sicherheitsregeln ausgeführt, die die in den Spokes gehosteten Workloads schützen und den Zugriff auf lokale Netzwerke steuern. Azure Firewall verfügt über integrierte Skalierbarkeit, während NVA-Firewalls hinter einem Lastenausgleich manuell skaliert werden können. Im Allgemeinen verfügt eine Firewallfarm im Vergleich zu einer Web Application Firewall über weniger spezialisierte Software. Sie hat jedoch einen umfassenderen Anwendungsbereich, sodass sie alle Arten von ein- und ausgehendem Datenverkehr filtern und überprüfen kann. Virtuelle Netzwerkgeräte für den NVA-Ansatz sind auf dem Azure Marketplace verfügbar und können von dort bereitgestellt werden.

Verwenden Sie eine Gruppe mit Azure-Firewalls (oder virtuellen Netzwerkgeräten) für Datenverkehr aus dem Internet und eine andere für Datenverkehr mit lokalem Ursprung. Die Verwendung einer einzigen Gruppe von Firewalls für den gesamten Datenverkehr stellt ein Sicherheitsrisiko dar, da sie keinen wirksamen Sicherheitsbereich zwischen den beiden Arten von Netzwerkdatenverkehr bietet. Separate Firewallebenen vereinfachen die Überprüfung von Sicherheitsregeln und ermöglichen eine eindeutige Zuordnung von Regeln zu eingehenden Netzwerkanforderungen.

Wir empfehlen, eine Gruppe von Azure Firewall-Instanzen oder virtuellen Netzwerkgeräten für Datenverkehr aus dem Internet zu verwenden. Verwenden Sie für Datenverkehr mit lokalem Ursprung eine andere Gruppe. Die Verwendung einer einzigen Gruppe von Firewalls für den gesamten Datenverkehr stellt ein Sicherheitsrisiko dar, da sie keinen wirksamen Sicherheitsbereich zwischen den beiden Arten von Netzwerkdatenverkehr bietet. Separate Firewallebenen vereinfachen die Überprüfung von Sicherheitsregeln und ermöglichen eine eindeutige Zuordnung von Regeln zu eingehenden Netzwerkanforderungen.

[**Azure Load Balancer**][ALB] stellt einen Dienst mit hoher Verfügbarkeit der Ebene 4 (TCP, UDP) bereit, mit dem eingehender Datenverkehr zwischen Dienstinstanzen verteilt werden kann, die in einem Satz mit Lastenausgleich definiert sind. Der Datenverkehr, der von den Front-End-Endpunkten (öffentliche IP-Endpunkte oder private IP-Endpunkte) an den Load Balancer gesendet wird, kann mit oder ohne Adressübersetzung auf einen Satz von Back-End-IP-Adresspools weiterverteilt werden (z.B. virtuelle Netzwerkgeräte oder VMs).

Der Azure Load Balancer kann die Integrität der verschiedenen Serverinstanzen überprüfen, und wenn bei einem Test die Antwort ausbleibt, beendet der Load Balancer das Senden von Datenverkehr an die fehlerhafte Instanz. Im virtuellen Rechenzentrum wird ein externer Load Balancer für den Hub und die Spokes bereitgestellt. Auf dem Hub wird der Load Balancer zum effizienten Weiterleiten von Datenverkehr an Dienste auf den Spokes verwendet. Auf den Spokes dienen Load Balancers zum Verwalten des Datenverkehrs von Anwendungen.

Der Microsoft-Dienst [**Azure Front Door Service**][AFD] (AFDS) stellt eine hoch verfügbare und hochgradig skalierbare Plattform für die Beschleunigung von Webanwendungen mit globalem HTTP-Lastenausgleich, Anwendungsschutz und Content Delivery Network bereit. AFDS wird an mehr als 100 Standorten im Edgebereich des globalen Netzwerks von Microsoft ausgeführt und ermöglicht Ihnen das Erstellen, Ausführen und horizontale Hochskalieren Ihrer dynamischen Webanwendung und statischen Inhalte. AFDS stellt für Ihre Anwendung erstklassige Endbenutzerleistung, einheitliche Automatisierung der Regions-/Stempelwartung, BCDR-Automatisierung, einheitliche Client-/Benutzerinformationen, Zwischenspeicherung und Erkenntnisse zu Ihren Diensten bereit. Die Plattform bietet Leistung, Zuverlässigkeit, Support-SLAs, Compliancezertifizierungen sowie überwachbare Sicherheitsverfahren, die von Azure entwickelt, betrieben und nativ unterstützt werden.

[**Microsoft Azure Application Gateway**][AppGW]: Microsoft Azure Application Gateway ist ein dediziertes virtuelles Gerät mit einem ADC (Application Delivery Controller) als Dienst und bietet verschiedene Lastenausgleichsfunktionen der Ebene 7 für Ihre Anwendung. Sie können damit die Produktivität von Webfarmen steigern, indem sie die CPU-intensive SSL-Beendigung an das Anwendungsgateway auslagern. Darüber hinaus werden noch weitere Routingfunktionen der Ebene 7 bereitgestellt. Hierzu zählen etwa die Roundrobin-Verteilung des eingehenden Datenverkehrs, cookiebasierte Sitzungsaffinität, Routing auf URL-Pfadbasis und die Möglichkeit zum Hosten mehrerer Websites hinter einer einzelnen Application Gateway-Instanz. Eine Web Application Firewall (WAF) wird auch als Teil des WAF SKU des Anwendungsgateways bereitgestellt. Dieser SKU bietet Schutz für Webanwendungen vor allgemeinen Onlinesicherheitsrisiken und Exploits. Application Gateway kann als Gateway mit Internetanbindung, rein internes Gateway oder als Kombination dieser beiden Optionen konfiguriert werden. 

[**Application Gateway**][AppGW] ist ein dediziertes virtuelles Gerät, das einen ADC (Application Delivery Controller) als Dienst bereitstellt und für Ihre Anwendung verschiedene Layer-7-Lastenausgleichsfunktionen bietet. Sie können die Produktivität von Webfarmen steigern, indem Sie die CPU-intensive SSL-Beendigung an die Application Gateway-Instanz abladen. Außerdem werden beispielsweise die folgenden Layer-7-Routingfunktionen bereitgestellt: 
* Roundrobin-Verteilung des eingehenden Datenverkehrs. 
* Cookiebasierte Sitzungsaffinität: 
* Routing auf URL-Pfadbasis. 
* Die Möglichkeit, mehrere Websites hinter einer einzelnen Application Gateway-Instanz zu hosten. Eine Web Application Firewall (WAF) wird ebenfalls als Teil der WAF-SKU von Application Gateway bereitgestellt. Dieser SKU bietet Schutz für Webanwendungen vor allgemeinen Onlinesicherheitsrisiken und Exploits. Application Gateway kann als Gateway mit Internetzugriff, rein internes Gateway oder als Kombination dieser beiden Optionen konfiguriert werden. 

[**Öffentliche IP-Adressen**][PIP]. Einige Azure-Features ermöglichen es Ihnen, einer öffentlichen IP-Adresse Dienstendpunkte zuzuordnen, sodass über das Internet auf die Ressource zugegriffen werden kann. Der Endpunkt verwendet die Netzwerkadressenübersetzung, um Datenverkehr zur internen Adresse und zum Port im virtuellen Azure-Netzwerk zu leiten. Dieser Pfad ist der primäre Weg, auf dem externer Datenverkehr in das virtuelle Netzwerk gelangt. Sie können die öffentlichen IP-Adressen konfigurieren, um festzulegen, welcher Datenverkehr zugelassen wird bzw. wie und wo eine Übersetzung in das virtuelle Netzwerk stattfindet.

[**Azure DDoS Protection Standard**][DDOS] stellt über den [Diensttarif „Basic“][DDOS] zusätzliche Funktionen zur Bedrohungsabwehr bereit, die speziell für Azure Virtual Network-Ressourcen optimiert sind. DDoS Protection Standard kann leicht aktiviert werden und erfordert keine Änderung der Anwendung. Schutzrichtlinien werden über dedizierte Datenverkehrsüberwachung und Machine Learning-Algorithmen optimiert. Richtlinien werden auf öffentliche IP-Adressen angewendet, die in virtuellen Netzwerken bereitgestellten Ressourcen zugeordnet sind. Beispiele hierfür sind Instanzen von Azure Load Balancer, Azure Application Gateway und Azure Service Fabric. Über Azure Monitor-Ansichten steht Echtzeit-Telemetrie während eines Angriffs und für den Verlauf zur Verfügung. Mit der Web Application Firewall für Azure Application Gateway können Sie Schutz auf der Anwendungsebene hinzufügen. Der Schutz wird für öffentliche Azure-IPv4-Adressen bereitgestellt.

#### <a name="component-type-monitoring"></a>Komponententyp: Überwachung

Überwachungskomponenten bieten Sichtbarkeit und Warnungen aus allen anderen Komponententypen. Alle Teams sollten die Komponenten und Dienste, auf die sie Zugriff haben, überwachen können. Wenn Sie über einen zentralen Helpdesk oder Betriebsteams verfügen, benötigen diese integrierten Zugriff auf die von diesen Komponenten bereitgestellten Daten.

Azure bietet verschiedene Protokollierungs- und Überwachungsdienste zum Nachverfolgen des Verhaltens von in Azure gehosteten Ressourcen. Die Governance und Steuerung von Workloads in Azure basiert nicht nur auf dem Sammeln von Protokolldaten, sondern auch auf der Möglichkeit, Aktionen auf der Grundlage von bestimmten gemeldeten Ereignissen auszulösen.

[**Azure Monitor**][Monitor]. Azure umfasst mehrere Dienste, die eine bestimmte Rolle oder Aufgabe im Überwachungsbereich ausführen. Zusammen bieten diese Dienste eine umfassende Lösung für das Sammeln, Übertragen und Analysieren von Telemetriedaten aus Ihrer Anwendung und den zugrunde liegenden Azure-Ressourcen, die sie unterstützen. Sie können auch kritische lokale Ressourcen überwachen und so eine hybride Überwachungsumgebung bilden. Voraussetzung der Entwicklung einer vollständigen Überwachungsstrategie für Ihre Anwendung ist die Kenntnis der verfügbaren Tools und Daten.

Es gibt zwei Hauptarten von Protokollen in Azure:

-   Das [Azure-Aktivitätsprotokoll][ActLog] (früher als **Betriebsprotokolle** bezeichnet) bietet Erkenntnisse zu den Vorgängen, die für Ressourcen im Azure-Abonnement ausgeführt wurden. Diese Protokolle melden die Ereignisse der Steuerungsebene für Ihre Abonnements. Jede Azure-Ressource erzeugt Überwachungsprotokolle.

-   [Azure Monitor-Diagnoseprotokolle][DiagLog] sind von einer Ressource generierte Protokolle mit umfangreichen, in kurzen Abständen erfassten Betriebsdaten der Ressource. Der Inhalt dieser Protokolle variiert je nach Ressourcentyp.

[![9]][9]

Es ist wichtig, die Protokolle von Netzwerksicherheitsgruppen und insbesondere die folgenden Informationen nachzuverfolgen:

-   [Ereignisprotokolle][NSGLog] enthalten Informationen zu den NSG-Regeln, die auf Grundlage der MAC-Adresse auf VMs und Instanzrollen angewendet werden.
-   [Leistungsindikatorprotokolle][NSGLog] verfolgen, wie oft jede NSG-Regel ausgeführt wurde, um Datenverkehr zuzulassen oder zu verweigern.

Alle Protokolle können zur Überwachung und statischen Analyse oder zu Sicherungszwecken in Azure-Speicherkonten gespeichert werden. Wenn Sie die Protokolle in einem Azure-Speicherkonto speichern, können Kunden verschiedene Typen von Frameworks zum Abrufen, Vorbereiten, Analysieren und Visualisieren dieser Daten verwenden, um den Status und die Integrität von Cloudressourcen zu melden. 

Große Unternehmen sollten bereits über ein Standardframework für die Überwachung lokaler Systeme verfügen. Sie können dieses Framework erweitern, um von Cloudbereitstellungen generierte Protokolle einzubinden. Mithilfe von [Azure Log Analytics] [https://docs.microsoft.com/en-us/azure/log-analytics/log-analytics-queries] können Organisationen die gesamte Protokollierung in der Cloud ausführen. Log Analytics wird als cloudbasierter Dienst implementiert und ist daher mit minimalen Investitionen in Infrastrukturdienste schnell betriebsbereit. Log Analytics kann auch in System Center-Komponenten wie System Center Operations Manager integriert werden, um Ihre bestehenden Investitionen in die Cloud zu erweitern. 

Log Analytics ist ein Dienst in Azure, mit dem von Betriebssystemen, Anwendungen und Komponenten der Cloud-Infrastruktur generierte Protokoll- und Leistungsdaten erfasst, korreliert, durchsucht und bearbeitet werden können. Diese Komponente liefert Kunden in Echtzeit Erkenntnisse zum Betrieb, da sie mithilfe der integrierten Suche und mit benutzerdefinierten Dashboards die Analyse aller Datensätze in sämtlichen Workloads Ihrer Implementierung des virtuellen Rechenzentrums ermöglicht.

[Azure Network Watcher][NetWatch] bietet Tools für die Überwachung, Diagnose und Anzeige von Metriken sowie die Aktivierung oder Deaktivierung von Protokollen für Ressourcen in einem virtuellen Azure-Netzwerk. Es ist ein breit gefächerter Dienst, der unter anderem die folgenden Funktionen bietet:
-    Überwachen der Kommunikation zwischen einer VM und einem Endpunkt
-    Anzeigen von Ressourcen in einem virtuellen Netzwerk mit den dazugehörigen Beziehungen
-    Diagnostizieren von Problemen bei der Filterung des ein- und ausgehenden Netzwerkdatenverkehrs einer VM
-    Diagnostizieren von Problemen beim Netzwerkrouting über eine VM
-    Diagnostizieren ausgehender Verbindungen einer VM
-    Erfassen von ein- und ausgehenden Paketen einer VM
-    Diagnostizieren von Problemen mit einem Gateway eines virtuellen Azure-Netzwerks und Verbindungen
-    Bestimmen von relativen Wartezeiten zwischen Azure-Regionen und Internetdienstanbietern
-    Anzeigen der Sicherheitsregeln für eine Netzwerkschnittstelle
-    Anzeigen von Netzwerkmetriken
-    Analysieren des ein- und ausgehenden Datenverkehrs einer Netzwerksicherheitsgruppe
-    Anzeigen von Diagnoseprotokollen für Netzwerkressourcen

Die in der Operations Management Suite (OMS) enthaltene Lösung [Netzwerkleistungsmonitor][NPM] kann durchgängig detaillierte Netzwerkinformationen liefern. Unter anderem bietet sie eine zentrale Ansicht Ihrer Azure-Netzwerke und lokalen Netzwerke. Die Lösung enthält spezifische Monitore für ExpressRoute und öffentliche Dienste.

#### <a name="component-type-workloads"></a>Komponententyp: Workloads

Workload-Komponenten befinden sich dort, wo auch Ihre tatsächlichen Anwendungen und Dienste sind. Dort verbringen auch Ihre Anwendungsentwicklungsteams die meiste Zeit.

Die Möglichkeiten für Workloads sind endlos. Im Folgenden finden Sie einige der möglichen Workload-Typen:

**Interne Branchenanwendungen**: Branchenanwendungen sind Computeranwendungen, die für den laufenden Unternehmensbetrieb entscheidend sind. Branchenanwendungen haben einige gemeinsame Merkmale:

-   Sie sind von Natur aus **interaktiv**. Daten werden eingegeben, und Ergebnisse oder Berichte werden zurückgegeben.
-   Sie sind **datengesteuert**. Branchenanwendungen sind datenintensiv und greifen häufig auf Datenbanken oder einen anderen Speicher zu.
-   Sie sind **integriert**. Branchenanwendungen ermöglichen die Integration in andere Systeme innerhalb oder außerhalb der Organisation.

**Websites für Kunden (im Internet oder intern)**: Die meisten Anwendungen, die mit dem Internet interagieren, sind Websites. Azure bietet die Möglichkeit, eine Website auf einer IaaS-VM oder von einer [Azure-Web-Apps][WebApps]-Website (PaaS) auszuführen. Azure-Web-Apps unterstützen die Integration in VNETs, die die Bereitstellung der Web-Apps in einer Spoke-Netzwerkzone ermöglichen. Für interne Websites muss kein öffentlicher Internetendpunkt verfügbar gemacht werden, weil auf die Ressourcen aus dem privaten VNET über Adressen zugegriffen werden kann, die nicht über das Internet geroutet werden können.

**Big Data + Analyse**: Wenn Daten zentral auf ein großes Volumen hochskaliert werden müssen, kann es passieren, dass Datenbanken nicht korrekt zentral hochskaliert werden. Die Hadoop-Technologie bietet ein System zum parallelen Ausführen von verteilten Abfragen auf einer großen Anzahl von Knoten. Kunden haben die Möglichkeit, Datenworkloads auf IaaS-VMs oder PaaS auszuführen ([HDInsight][HDI]). HDInsight unterstützt das Bereitstellen in einem lokalen VNET: Die Bereitstellung kann in einem Cluster in einem Spoke des virtuellen Rechenzentrums erfolgen.

**Ereignisse und Messaging**: [Azure Event Hubs][EventHubs] ist ein hyperskalierbarer Dienst für die Erfassung von Telemetriedaten, der Millionen von Ereignissen sammelt, transformiert und speichert. Diese verteilte Streamingplattform bietet niedrige Latenz und konfigurierbare Aufbewahrungszeiten, wodurch Sie riesige Mengen an Telemetriedaten in Azure einspeisen und die Daten verschiedener Anwendungen lesen können. In Event Hubs kann ein einziger Datenstrom in Echtzeit und batchbasierte Pipelines unterstützen.

Über [Azure Service Bus][ServiceBus] können Sie einen zuverlässigen Cloudmessagingdienst zwischen Anwendungen und Diensten implementieren. Der Dienst bietet asynchrones Brokermessaging zwischen Client und Server, strukturiertes FIFO-Messaging (First In, First Out) sowie Funktionen zum Veröffentlichen und Abonnieren.

[![10]][10]

### <a name="making-the-vdc-highly-available-multiple-vdcs"></a>Hochverfügbarkeit für das virtuelle Rechenzentrum: mehrere VDCs

Bisher ging es in diesem Artikel in erster Linie um den Entwurf eines einzelnen virtuellen Rechenzentrums sowie die grundlegenden Komponenten und die Architektur, die zur Resilienz beitragen. Azure-Funktionen wie der Azure Load Balancer, NVAs, Verfügbarkeitsgruppen, Skalierungsgruppen und andere Mechanismen bilden ein System, mit dem Sie solide Servicelevels (SLA) in Ihre Produktionsdienste integrieren können.

Da ein einzelnes virtuelles Rechenzentrum normalerweise in einer einzelnen Region implementiert wird, kann es anfällig für größere Ausfälle sein, die die gesamte Region betreffen. Kunden, die hohe Servicelevels benötigen, müssen die Dienste schützen, indem sie dasselbe Projekt in zwei (oder mehr) Implementierungen des virtuellen Rechenzentrums in unterschiedlichen Regionen bereitstellen.

Zusätzlich zu den Überlegungen zu SLAs gibt es einige allgemeine Szenarien, in denen die Bereitstellung mehrerer Implementierungen des virtuellen Rechenzentrums sinnvoll ist:

-   Regionale oder globale Präsenz
-   Notfallwiederherstellung
-   Mechanismus zum Umleiten von Datenverkehr zwischen Rechenzentren

#### <a name="regionalglobal-presence"></a>Regionale/globale Präsenz

Azure-Rechenzentren befinden sich in zahlreichen Regionen weltweit. Wenn Sie mehrere Azure-Rechenzentren auswählen, müssen Kunden zwei zusammenhängende Faktoren berücksichtigen: die geografische Entfernung und die Latenz. Die höchste Benutzerfreundlichkeit erzielen Sie, indem Sie die geografische Entfernung zwischen den einzelnen Implementierungen virtueller Rechenzentren und die Entfernung zwischen einzelnen Implementierungen virtueller Rechenzentren und den Endbenutzern auswerten.

Die Region, in der die Implementierungen des virtuellen Rechenzentrums gehostet werden, muss auch die gesetzlichen Anforderungen des Rechtsraums erfüllen, in dem Ihre Organisation tätig ist.

#### <a name="disaster-recovery"></a>Notfallwiederherstellung

Der Entwurf eines Plans für die Notfallwiederherstellung richtet sich nach den Arten von Workloads und der Möglichkeit, den Zustand dieser Workloads zwischen unterschiedlichen Implementierungen des virtuellen Rechenzentrums zu synchronisieren. Die meisten Kunden wünschen sich im Idealfall einen schnellen Failovermechanismus. Hierfür ist ggf. eine Synchronisierung der Anwendungsdaten zwischen den Bereitstellungen erforderlich, für die mehrere Implementierungen von Rechenzentren ausgeführt werden. Beim Entwerfen von Plänen für die Notfallwiederherstellung sollte aber unbedingt berücksichtigt werden, dass die meisten Anwendungen empfindlich auf die Latenz reagieren, die durch diese Datensynchronisierung verursacht werden kann.

Für die Synchronisierung und Heartbeatüberwachung von Anwendungen in verschiedenen Implementierungen des virtuellen Rechenzentrums ist eine Kommunikation über das Netzwerk erforderlich. Zwei Implementierungen des virtuellen Rechenzentrums in unterschiedlichen Regionen können auf folgende Weise verbunden werden:

-   VNET-Peering: Mittels VNET-Peering können Hubs regionsübergreifend verbunden werden.
-   Privates ExpressRoute-Peering, wenn die Hubs jeder Implementierung des virtuelles Netzwerks mit derselben ExpressRoute-Leitung verbunden sind.
-   Mehrere ExpressRoute-Verbindungen, die über Ihren firmeneigenen Backbone und Ihre Implementierungen der virtuellen Rechenzentren mit den ExpressRoute-Leitungen verbunden sind.
-   Site-to-Site-VPN-Verbindungen zwischen der Hubzone Ihrer Implementierungen virtueller Rechenzentren in jeder Azure-Region.

Normalerweise stellen VNET-Peering- oder ExpressRoute-Verbindungen aufgrund ihrer höheren Bandbreite und konsistenten Latenzebenen bei der Übertragung über den Microsoft-Backbone die bevorzugte Art von Netzwerkkonnektivität dar.

Unsere Empfehlung lautet: Kunden sollten Netzwerkqualifizierungstests durchführen, um die Latenz und Bandbreite dieser Verbindungen zu überprüfen, und dann anhand des Ergebnisses entscheiden, ob eine synchrone oder asynchrone Datenreplikation geeignet ist. Es ist auch wichtig, diese Ergebnisse hinsichtlich des optimalen RTO-Werts (Recovery Time Objective) zu bewerten.

#### <a name="disaster-recovery-diverting-traffic-from-one-region-to-another"></a>Notfallwiederherstellung: Umleiten von Datenverkehr aus einer Region in eine andere

[Azure Traffic Manager][TM] überprüft regelmäßig die Dienstintegrität von öffentlichen Endpunkten in unterschiedlichen Implementierungen virtueller Rechenzentren. Wenn für diese Endpunkte ein Fehler auftritt, werden die Daten über das Domain Name System (DNS) automatisch an das sekundäre virtuelle Netzwerk umgeleitet. 

Da das DNS genutzt wird, kann Traffic Manager nur mit öffentlichen Azure-Endpunkten verwendet werden.  Der Dienst wird normalerweise verwendet, um Datenverkehr zu steuern oder an Azure-VMs und Web-Apps auf der fehlerfreien Instanz der Implementierung eines virtuellen Netzwerks umzuleiten. Traffic Manager ist auch dann resilient, wenn eine gesamte Azure-Region ausfällt, und kann die Verteilung von Benutzerdatenverkehr für Dienstendpunkte in unterschiedlichen virtuellen Netzwerken anhand von mehreren Kriterien steuern. Beispiele hierfür sind der Ausfall eines Diensts für eine spezifische Implementierung eines virtuellen Netzwerks oder die Auswahl der VDC-Implementierung mit der geringsten Netzwerklatenz.

### <a name="summary"></a>Zusammenfassung

Das virtuelle Rechenzentrum (VDC) ist ein Ansatz für die Migration von Rechenzentren zur Erstellung einer skalierbaren Architektur in Azure, mit der die Nutzung von Cloudressourcen maximiert und die Kosten reduziert werden und die Steuerung des Systems vereinfacht wird. VDC basiert auf einer Hub-and-Spoke-Netzwerktopologie, in der allgemein freigegebene Dienste auf dem Hub bereitgestellt und bestimmte Anwendungen/Workloads in den Spokes zugelassen werden. Darüber hinaus wird bei VDC auch die Struktur von Unternehmensrollen berücksichtigt, indem unterschiedliche Abteilungen, z.B. zentrale IT-Abteilung, DevOps, Betrieb und Wartung, zusammenarbeiten, während sie ihre jeweiligen Rollen ausfüllen. Das virtuelle Rechenzentrum erfüllt die Anforderungen einer „Lift & Shift“-Migration, bietet aber auch zahlreiche Vorteile für native Cloudbereitstellungen.

## <a name="references"></a>Referenzen

Die folgenden Features wurden in diesem Dokument erläutert. Nutzen Sie die Links, um mehr zu erfahren.

| | | |
|-|-|-|
|Netzwerkfunktionen|Lastenausgleich|Konnektivität|
|[Virtuelle Azure-Netzwerke][VNet]</br>[Netzwerksicherheitsgruppen][NSG]</br>[Protokollanalysen für Netzwerksicherheitsgruppen (NSGs)][NSGLog]</br>[Benutzerdefiniertes Routing][UDR]</br>[Virtuelle Netzwerkgeräte][NVA]</br>[Öffentliche IP-Adressen][PIP]</br>[Azure DDoS][DDOS]</br>[Azure Firewall][AzFW]</br>[Azure DNS][DNS]|[Azure Front Door Service][AFD]</br>[Azure Load Balancer (L3) ][ALB]</br>[Application Gateway (L7) ][AppGW]</br>[Web Application Firewall][WAF]</br>[Azure Traffic Manager][TM]</br></br></br></br></br> |[VNET-Peering][VNetPeering]</br>[Virtuelle private Netzwerke][VPN]</br>[Virtual WAN][vWAN]</br>[ExpressRoute][ExR]</br>[ExpressRoute Direct][ExRD]</br></br></br></br></br>
|Identity</br>|Überwachung</br>|Bewährte Methoden</br>|
|[Azure Active Directory][AAD]</br>[Multi-Factor Authentication][MFA]</br>[Rollenbasierte Zugriffssteuerung][RBAC]</br>[Azure AD-Standardrollen][Roles]</br></br></br> |[Network Watcher][NetWatch]</br>[Azure Monitor][Monitor]</br>[Aktivitätsprotokolle][ActLog]</br>[Diagnoseprotokolle][DiagLog]</br>[Microsoft Operations Management Suite][OMS]</br>[Netzwerkleistungsmonitor][NPM]|[Bewährte Methoden für Umkreisnetzwerke][DMZ]</br>[Abonnementverwaltung][SubMgmt]</br>[Verwaltung von Ressourcengruppen][RGMgmt]</br>[Einschränkungen für Azure-Abonnements][Limits] </br></br></br>|
|Weitere Azure-Dienste|
|[Azure-Web-Apps][WebApps]</br>[HDInsights (Hadoop)][HDI]</br>[Event Hubs][EventHubs]</br>[Service Bus][ServiceBus]|

## <a name="next-steps"></a>Nächste Schritte

 - Lernen Sie das [VNET-Peering][VNetPeering] kennen, die technologische Grundlage für Hub-Spoke-Entwürfe von virtuellen Rechenzentren.
 - Implementieren Sie [Azure AD][AAD], um die Funktionen der [rollenbasierten Zugriffssteuerung][RBAC] zu erkunden.
 - Entwickeln Sie Modelle für die Abonnement- und Ressourcenverwaltung sowie die rollenbasierte Zugriffssteuerung, die der Struktur, den Anforderungen und den Richtlinien Ihrer Organisation entsprechen. Die wichtigste Aktivität ist die Planung. Planen Sie Neuorganisationen, Fusionen, neue Produktlinien usw. so weit wie möglich. <!--Image References-->
[0]: ./images/networking-redundant-equipment.png "Beispiele für Komponentenüberlappung" 
[1]: ./images/networking-vdc-high-level.png "Allgemeines Beispiel für ein virtuelles Hub-Spoke-Rechenzentrum"
[2]: ./images/networking-hub-spokes-cluster.png "Cluster mit Hub und Spokes"
[3]: ./images/networking-spoke-to-spoke.png "Spoke-zu-Spoke"
[4]: ./images/networking-vdc-block-level-diagram.png "Diagramm des virtuellen Rechenzentrums auf der Blockebene"
[5]: ./images/networking-users-groups-subsciptions.png "Benutzer, Gruppen, Abonnements und Projekte"
[6]: ./images/networking-infrastructure-high-level.png "Diagramm zur allgemeinen Infrastruktur"
[7]: ./images/networking-highlevel-perimeter-networks.png "Diagramm zur allgemeinen Infrastruktur"
[8]: ./images/networking-vnet-peering-perimeter-neworks.png "VNET-Peering und Umkreisnetzwerke"
[9]: ./images/networking-high-level-diagram-monitoring.png "Übersichtsdiagramm zur Überwachung"
[10]: ./images/networking-high-level-workloads.png "Übersichtsdiagramm zu Workloads"

<!--Link References-->
[Limits]: /azure/azure-subscription-service-limits
[Roles]: /azure/role-based-access-control/built-in-roles
[VNet]: /azure/virtual-network/virtual-networks-overview
[NSG]: /azure/virtual-network/virtual-networks-nsg
[DNS]: /azure/dns/dns-overview
[PrivateDNS]: /azure/dns/private-dns-overview
[VNetPeering]: /azure/virtual-network/virtual-network-peering-overview 
[UDR]: /azure/virtual-network/virtual-networks-udr-overview 
[RBAC]: /azure/role-based-access-control/overview
[MFA]: /azure/multi-factor-authentication/multi-factor-authentication
[AAD]: /azure/active-directory/active-directory-whatis
[VPN]: /azure/vpn-gateway/vpn-gateway-about-vpngateways 
[ExR]: /azure/expressroute/expressroute-introduction
[ExRD]: https://docs.microsoft.com/en-us/azure/expressroute/expressroute-erdirect-about
[vWAN]: /azure/virtual-wan/virtual-wan-about
[NVA]: /azure/architecture/reference-architectures/dmz/nva-ha
[AzFW]: /azure/firewall/overview
[SubMgmt]: /azure/architecture/cloud-adoption/appendix/azure-scaffold 
[RGMgmt]: /azure/azure-resource-manager/resource-group-overview
[DMZ]: /azure/best-practices-network-security
[ALB]: /azure/load-balancer/load-balancer-overview
[DDOS]: /azure/virtual-network/ddos-protection-overview
[PIP]: /azure/virtual-network/resource-groups-networking#public-ip-address
[AFD]: https://docs.microsoft.com/en-us/azure/frontdoor/front-door-overview
[AppGW]: /azure/application-gateway/application-gateway-introduction
[WAF]: /azure/application-gateway/application-gateway-web-application-firewall-overview
[Monitor]: /azure/monitoring-and-diagnostics/
[ActLog]: /azure/monitoring-and-diagnostics/monitoring-overview-activity-logs 
[DiagLog]: /azure/monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs
[NSGLog]: /azure/virtual-network/virtual-network-nsg-manage-log
[OMS]: /azure/operations-management-suite/operations-management-suite-overview
[NPM]: /azure/log-analytics/log-analytics-network-performance-monitor
[NetWatch]: /azure/network-watcher/network-watcher-monitoring-overview
[WebApps]: /azure/app-service/
[HDI]: /azure/hdinsight/hdinsight-hadoop-introduction
[EventHubs]: /azure/event-hubs/event-hubs-what-is-event-hubs 
[ServiceBus]: /azure/service-bus-messaging/service-bus-messaging-overview
[TM]: /azure/traffic-manager/traffic-manager-overview