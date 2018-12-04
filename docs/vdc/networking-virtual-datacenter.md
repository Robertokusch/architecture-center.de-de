---
title: 'Virtuelles Rechenzentrum in Microsoft Azure: Eine Netzwerkperspektive'
description: In diesem Artikel erfahren Sie, wie Sie Ihr virtuelles Rechenzentrum in Azure erstellen.
author: tracsman
manager: rossort
tags: azure-resource-manager
ms.service: virtual-network
ms.date: 09/24/2018
ms.author: jonor
ms.openlocfilehash: b10930ac6d6458872d8b626825d21bd0bed2b807
ms.sourcegitcommit: 16bc6a91b6b9565ca3bcc72d6eb27c2c4ae935e4
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/28/2018
ms.locfileid: "52550460"
---
# <a name="azure-virtual-datacenter-a-network-perspective"></a>Virtuelles Rechenzentrum in Microsoft Azure: Eine Netzwerkperspektive

## <a name="overview"></a>Übersicht
Durch das Migrieren lokaler Anwendungen zu Azure können Organisationen ohne größere Änderungen eine sichere und kosteneffiziente Infrastruktur bereitstellen. Dieser Ansatz wird als Migration per **Lift & Shift** bezeichnet. Um die durch Cloud Computing mögliche Agilität optimal zu nutzen, sollten Unternehmen ihre Architekturen weiterentwickeln und von Azure-Funktionen Gebrauch machen. Microsoft Azure bietet hyperskalierbare Dienste und Infrastrukturen, Zuverlässigkeit und Funktionen für Unternehmen sowie vielfältige Auswahlmöglichkeiten für Hybridkonnektivität. 

Kunden können über das Internet oder mit privater Netzwerkkonnektivität über Microsoft Azure ExpressRoute auf diese Clouddienste zugreifen. Mit der Microsoft Azure Platform können Kunden ihre Infrastruktur nahtlos auf die Cloud erweitern und Architekturen mit mehreren Ebenen erstellen. Zudem bieten Microsoft-Partner erweiterte Funktionen in Form von Sicherheitsdiensten und virtuellen Geräten an, die für die Verwendung in Azure optimiert sind.

Dieser Artikel bietet einen Überblick über die Muster und Entwürfe, mit denen Kunden die architektonischen Herausforderungen hinsichtlich der Skalierung, Leistung und Sicherheit meistern können, die bei einer Migration zur Cloud auf sie zukommen. Außerdem werden verschiedene IT-Organisationsrollen und ihre Verwendung für die Verwaltung und Governance des Systems erläutert. Hierbei befassen wir uns schwerpunktmäßig mit den Sicherheitsanforderungen und der Kostenoptimierung.

## <a name="what-is-a-virtual-datacenter"></a>Was ist ein virtuelles Rechenzentrum?
Cloudlösungen wurden ursprünglich dazu entworfen, einzelne, relativ isolierte Anwendungen im öffentlichen Spektrum zu hosten. Dieser Ansatz hat sich für einige Jahre bewährt. Im Laufe der Zeit wurden jedoch die Vorteile von Cloudlösungen deutlich, und es wurden mehrere umfangreiche Workloads in der Cloud gehostet. Die Erfüllung der Anforderungen hinsichtlich der Sicherheit, Zuverlässigkeit, Leistung und Kosten von Bereitstellungen in einer oder mehreren Regionen entwickelte sich dadurch zu einem entscheidenden Faktor im Lebenszyklus des Clouddiensts.

Das **rote Kästchen** im folgenden Diagramm einer Cloudbereitstellung zeigt ein Beispiel für eine Sicherheitslücke. Das **gelbe Kästchen** stellt Möglichkeiten zur Optimierung von virtuellen Netzwerkgeräten (Network Virtual Appliance, NVA) in verschiedenen Workloads dar.

[![0]][0]

Die Idee eines virtuellen Rechenzentrums (Virtual Data Center, VDC) entstand aus der Notwendigkeit, Rechenzentren zur Unterstützung von Unternehmensworkloads zu skalieren. Zudem soll ein virtuelles Rechenzentrum die Probleme lösen, die mit der Unterstützung umfangreicher Anwendungen in der Public Cloud einhergehen.

Ein virtuelles Rechenzentrum besteht nicht nur aus den Anwendungsworkloads in der Cloud. Es umfasst auch das Netzwerk, die Sicherheit, Verwaltung und Infrastruktur. Beispiele hierfür sind das Domain Name System (DNS) und Verzeichnisdienste. In der Regel stellt ein virtuelles Rechenzentrum eine private Verbindung zurück zu einem lokalen Netzwerk oder Rechenzentrum bereit. Je mehr Workloads zu Azure migriert werden, desto wichtiger ist es, die unterstützende Infrastruktur und die Objekte zu bedenken, in denen diese Workloads platziert werden. Durch eine sorgfältige Planung der Ressourcenstruktur können Sie verhindern, dass Hunderte von **Workloadinseln** entstehen, die getrennt voneinander mit jeweils eigenen Anforderungen in Bezug auf die Datenflüsse, Sicherheitsmodelle und Konformität verwaltet werden müssen.

Das virtuelle Rechenzentrum ist eine Sammlung von separaten, jedoch miteinander verwandten Entitäten mit gemeinsam genutzten unterstützenden Funktionen und Features sowie einer gemeinsamen Infrastruktur. Wenn Sie Ihre Workloads als ein integriertes virtuelles Rechenzentrum begreifen, können Sie durch die Zentralisierung von Komponenten und Datenflüssen aufgrund von Skaleneffekten und der optimierten Sicherheit Kosten sparen. Zudem können Sie den Betrieb, die Verwaltung und Konformitätsprüfungen vereinfachen.

> [!NOTE]  
> Virtuelle Rechenzentren sind jedoch **kein** diskretes Azure-Produkt. Ein virtuelles Rechenzentrum ist eine auf Ihre Anforderungen zugeschnittene Kombination verschiedener Features und Funktionen. Das virtuelle Rechenzentrum stellt ein bestimmtes Verständnis Ihrer Workloads und Azure-Nutzung dar, dessen Ziel es ist, Ihre Ressourcen und Möglichkeiten in der Cloud zu maximieren. In dieser Hinsicht kann das virtuelle Rechenzentrum daher als ein modularer Ansatz für die Erstellung von IT-Diensten in Azure unter Berücksichtigung der Organisationsrollen und Verantwortlichkeiten verstanden werden.

Sie können Unternehmen dabei helfen, Workloads und Anwendungen für die folgenden Szenarios in Azure auszuführen:

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

## <a name="who-should-implement-a-virtual-datacenter"></a>Wer sollte ein virtuelles Rechenzentrum implementieren?
Alle Azure-Kunden, die mehr als nur einige wenige Workloads zu Azure migrieren müssen, können von gemeinsamen Ressourcen profitieren. Je nach Größe können auch einzelne Anwendungen von den Mustern und Komponenten profitieren, die zum Erstellen des virtuellen Rechenzentrums verwendet werden.

Wenn Ihre Organisation über ein zentrales IT-, Netzwerk-, Sicherheits- oder für die Konformität zuständiges Team bzw. eine entsprechende Abteilung verfügt, kann das virtuelle Rechenzentrum dabei helfen, Richtlinien zu erzwingen und Aufgaben zu trennen. Mit einem virtuellen Rechenzentrum können Sie auch die Einheitlichkeit der zugrunde liegenden gemeinsamen Komponenten sicherstellen und gleichzeitig Anwendungsteams die für Ihre Anforderungen notwendige Freiheit und Kontrolle bieten.

Organisationen, die Azure DevOps nutzen möchten, können mit den Konzepten des virtuellen Rechenzentrums autorisierte Taschen von Azure-Ressourcen bereitstellen und sicherstellen, dass sie innerhalb dieser Gruppe die vollständige Kontrolle haben. Gruppen sind entweder Abonnements oder Ressourcengruppen in einem allgemeinen Abonnement. Die Netzwerk- und Sicherheitsbegrenzungen bleiben jedoch entsprechend den definierten Vorgaben einer zentralen Richtlinie in einem virtuellen Hub-Netzwerk und einer Ressourcengruppe konform.

## <a name="considerations-when-you-implement-a-virtual-datacenter"></a>Überlegungen zur Implementierung des virtuellen Rechenzentrums
Beim Entwerfen Ihres virtuellen Rechenzentrums sind mehrere zentrale Punkte zu berücksichtigen:

-   Identitäts- und Verzeichnisdienste
-   Sicherheitsinfrastruktur
-   Konnektivität mit der Cloud
-   Konnektivität innerhalb der Cloud

### <a name="identity-and-directory-services"></a>Identitäts- und Verzeichnisdienste
Identitäts- und Verzeichnisdienste sind ein wichtiger Aspekt jedes Rechenzentrums, ganz gleich, ob lokal oder in der Cloud gehostet. Identität bezieht sich auf alle Aspekte des Zugriffs und der Autorisierung für Dienste innerhalb des virtuellen Rechenzentrums. Um sicherzustellen, dass nur autorisierte Benutzer und Prozesse auf Ihr Azure-Konto und Ihre Azure-Ressourcen zugreifen, verwendet Azure mehrere Typen von Anmeldeinformationen für die Authentifizierung. Dazu zählen Kennwörter für den Zugriff auf das Azure-Konto, kryptografische Schlüssel, digitale Signaturen und Zertifikate. 

[Microsoft Azure Multi-Factor Authentication (MFA)][MFA] ist eine zusätzliche Sicherheitsebene für den Zugriff auf Azure-Dienste. MFA bietet eine sichere Authentifizierung mit einer Auswahl von einfachen Überprüfungsoptionen. Kunden können die von ihnen bevorzugte Methode auswählen. Zu den verfügbaren Optionen zählen Telefonanrufe, SMS und Benachrichtigungen in einer mobilen App. 

Alle großen Unternehmen müssen einen Identitätsverwaltungsprozess definieren, der die Verwaltung der einzelnen Identitäten sowie deren Authentifizierung, Autorisierung, Rollen und Berechtigungen innerhalb oder zwischen virtuellen Rechenzentren beschreibt. Durch diesen Prozess sollen die Sicherheit und Produktivität gesteigert und gleichzeitig die Kosten, die Downtime und sich wiederholende manuelle Aufgaben reduziert werden.

In Unternehmen und Organisationen kann eine anspruchsvolle Kombination von Diensten für verschiedene Branchen erforderlich sein. Zudem haben Mitarbeiter oft unterschiedliche Rollen inne, wenn sie an verschiedenen Projekten arbeiten. Das virtuelle Rechenzentrum erfordert eine gute Zusammenarbeit zwischen verschiedenen Teams mit bestimmten Rollendefinitionen, um Systeme verantwortungsbewusst mit geeigneter Governance aufzusetzen. Die Matrix der Zuständigkeiten, Zugriffsrechte und Berechtigungen kann komplex sein. Die Identitätsverwaltung in virtuellen Rechenzentren wird über [Azure Active Directory (Azure AD)][AAD] und die rollenbasierte Zugriffssteuerung implementiert.

Ein Verzeichnisdienst ist eine freigegebene Informationsstruktur für die Suche, Verwaltung und Organisation von täglich verwendeten Elementen und Netzwerkressourcen. Diese Ressourcen können Volumes, Ordner, Dateien, Drucker, Benutzer, Gruppen, Geräte und andere Objekte enthalten. Jede Ressource im Netzwerk wird vom Verzeichnisserver als Objekt angesehen. Informationen zu einer Ressource werden als eine Auflistung von dieser Ressource oder diesem Objekt zugeordneten Attributen gespeichert.

Alle Microsoft-Onlinedienste für Unternehmen nutzen für Anmelde- und andere Identitätsanforderungen Azure Active Directory (Azure AD). Azure Active Directory ist eine umfassende, hochverfügbare Cloudlösung für die Identitäts- und Zugriffsverwaltung, die grundlegende Verzeichnisdienste, erweiterte Identitätsgovernance und Zugriffsverwaltung für Anwendungen kombiniert. Azure AD kann in eine lokale Active Directory-Instanz integriert werden, um einmaliges Anmelden für alle cloudbasierten und lokal gehosteten Anwendungen zu ermöglichen. Die Benutzerattribute der lokalen Active Directory-Instanz können automatisch mit Azure AD synchronisiert werden.

In einer Implementierung des virtuellen Rechenzentrums muss nicht ein einzelner globaler Administrator alle Berechtigungen zuweisen. Stattdessen können jeder spezifischen Abteilung oder Gruppe von Benutzern oder Diensten im Verzeichnisdienst die erforderlichen Berechtigungen zum Verwalten ihrer eigenen Ressourcen innerhalb des virtuellen Rechenzentrums zugewiesen werden. Das Strukturieren von Berechtigungen muss gut durchdacht sein. Zu viele Berechtigungen können die Leistungseffizienz beeinträchtigen. Zu wenige oder ungenau definierte Berechtigungen können die Sicherheitsrisiken erhöhen. Die rollenbasierte Zugriffssteuerung in Azure ermöglicht eine differenzierte Zugriffsverwaltung für die Ressourcen eines virtuellen Rechenzentrums und trägt damit zur Lösung dieses Problems bei.

#### <a name="security-infrastructure"></a>Sicherheitsinfrastruktur
Die Sicherheitsinfrastruktur dient zur Trennung des Datenverkehrs im jeweiligen virtuellen Netzwerksegment des virtuellen Rechenzentrums und zur Steuerung der ein- und ausgehenden Datenflüsse im gesamten virtuellen Rechenzentrum. Die Azure-Architektur beruht auf mehreren Mandanten, sodass nicht autorisierter oder unbeabsichtigter Datenverkehr zwischen Bereitstellungen verhindert wird. Sie nutzt die Isolation virtueller Netzwerke, Zugriffssteuerungslisten (Access Control List, ACL), Lastenausgleichsmodule, IP-Filter und Richtlinien für den Datenverkehrsfluss. Die Netzwerkadressenübersetzung (Network Address Translation, NAT) trennt den internen vom externen Netzwerkdatenverkehr.

Das Azure-Fabric ordnet Infrastrukturressourcen Mandantenworkloads zu und verwaltet die ein- und ausgehende Kommunikation von virtuellen Computern (Virtual Machines, VMs). Der Azure-Hypervisor erzwingt eine Speicher- und Prozesstrennung zwischen virtuellen Computern und routet den Netzwerkdatenverkehr sicher zu den Gastbetriebssystem-Mandanten.

#### <a name="connectivity-to-the-cloud"></a>Verbindung mit der Cloud
Eine Implementierung des virtuellen Rechenzentrums erfordert Konnektivität mit externen Netzwerken, um Dienste für Kunden, Partner und interne Benutzer bereitzustellen. In der Regel ist nicht nur Konnektivität mit dem Internet, sondern auch mit lokalen Netzwerken und Rechenzentren erforderlich.

Kunden steuern mithilfe von [Azure Firewall][AzFW] oder anderen virtuellen Netzwerkgeräten, angepassten Routingrichtlinien mit [benutzerdefinierten Routen][UDR] und der Netzwerkfilterung mit [Netzwerksicherheitsgruppen][NSG], welche Dienste Zugriff auf das öffentliche Internet haben und über das öffentliche Internet zugänglich sind. Wir empfehlen, alle Ressourcen mit Internetzugriff zusätzlich durch [Azure DDoS Protection Standard][DDOS] zu schützen.

Unternehmen müssen ihre Implementierungen des virtuellen Rechenzentrums oft mit lokalen Rechenzentren oder anderen Ressourcen verbinden. Die Konnektivität zwischen Azure und lokalen Netzwerken ist daher ein entscheidender Aspekt beim Entwerfen einer effektiven Architektur. Unternehmen haben zwei Möglichkeiten, eine Verbindung zwischen Implementierungen des virtuellen Rechenzentrums und lokalen Netzwerken in Azure zu erstellen: Übertragung über das Internet oder private direkte Verbindungen.

Ein [Azure-Site-to-Site-VPN][VPN] verbindet das lokale Netzwerk eines Unternehmens über das öffentliche Internet mit ihrer Implementierung des virtuellen Rechenzentrums. Datenverkehr über das VPN wird mit IPSec und IKE-Tunneln verschlüsselt. Azure-Site-to-Site-Verbindungen sind flexibel und können schnell erstellt werden. Außerdem erfordert dieses VPN keine weiteren Bereitstellungen, da alle Verbindungen über das Internet hergestellt werden.

Der Netzwerkdienst [Azure Virtual WAN][vWAN] bietet für eine große Anzahl von VPN-Verbindungen optimierte und automatisierte Konnektivität zwischen Branches über Azure. Mit Virtual WAN können Sie Branchgeräte für die Kommunikation mit Azure verbinden und konfigurieren. Diese Verbindung kann entweder manuell oder mit Geräten eines bevorzugten Anbieters über einen Virtual WAN-Partner hergestellt werden. Die Nutzung von Geräten eines bevorzugten Anbieters ist besonders benutzerfreundlich, da sie die Konnektivitäts- und Konfigurationsverwaltung vereinfacht. Das integrierte Azure WAN-Dashboard liefert Ihnen umgehend Erkenntnisse zur Problembehandlung, die Ihnen viel Zeit sparen können. Zudem können Sie im Dashboard auf einfache Weise umfassende Informationen zur Site-to-Site-Konnektivität anzeigen.

[ExpressRoute][ExR], ein Azure-Konnektivitätsdienst, ermöglicht private Verbindungen zwischen dem lokalen Netzwerk des Unternehmens und seiner Implementierung des virtuellen Rechenzentrums. ExpressRoute bietet bei gleichbleibenden Wartezeiten mehr Sicherheit und Zuverlässigkeit sowie höhere Geschwindigkeiten (bis zu 10 GBit/s). ExpressRoute ist sehr nützlich für Implementierungen des virtuellen Rechenzentrums, da ExpressRoute-Kunden von den Konformitätsregeln privater Verbindungen profitieren. Falls Sie einen höheren Bandbreitenbedarf haben, können Sie mit [ExpressRoute Direct][ExRD] eine direkte 100-GBit/s-Verbindung mit Microsoft-Routern herstellen.

Für die Bereitstellung von ExpressRoute-Verbindungen ist in der Regel ein ExpressRoute-Dienstanbieter erforderlich. Kunden, die schnell die Arbeit aufnehmen müssen, verwenden anfangs üblicherweise Site-to-Site-VPN-Verbindungen, um die Konnektivität zwischen dem virtuellen Rechenzentrum und den lokalen Ressourcen herzustellen. Sobald die physische Verbindung mit dem Dienstanbieter eingerichtet wurde, migrieren sie dann zu einer ExpressRoute-Verbindung.

#### <a name="connectivity-within-the-cloud"></a>Konnektivität in der Cloud
[Virtuelle Netzwerke][VNet] und das [Peering in virtuellen Netzwerken][VNetPeering] sind die grundlegenden Netzwerkkonnektivitätsdienste innerhalb des virtuellen Rechenzentrums. Ein virtuelles Netzwerk gewährleistet eine natürliche Isolationsgrenze zwischen den Ressourcen in einem virtuellen Rechenzentrum. Darüber hinaus ermöglicht das Peering in virtuellen Netzwerken die Kommunikation zwischen verschiedenen virtuellen Netzwerken in derselben oder sogar in mehreren Azure-Regionen. Bei der Steuerung des Datenverkehrs in und zwischen virtuellen Netzwerken müssen Sicherheitsregeln erfüllt werden, die in Zugriffssteuerungslisten festgelegt werden. Lesen Sie hierzu die Artikel zu [Netzwerksicherheitsgruppen][NSG], [virtuellen Netzwerkgeräten][NVA] und [benutzerdefinierten Routingtabellen für benutzerdefinierte Routen][UDR].

## <a name="virtual-datacenter-overview"></a>Übersicht über das virtuelle Rechenzentrum

### <a name="topology"></a>Topologie
Das **Hub-Spoke**-Modell ist ein Konzept zur Erweiterung eines virtuellen Rechenzentrums innerhalb einer einzelnen Azure-Region.

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
In Azure wird jede Komponente, unabhängig vom Typ, in einem Azure-Abonnement bereitgestellt. Die Isolation von Azure-Komponenten in verschiedenen Azure-Abonnements kann die Anforderungen verschiedener Branchen erfüllen. Ein Beispiel hierfür ist das Einrichten unterschiedlicher Zugriffs- und Autorisierungsebenen.

Eine einzelne Implementierung des virtuellen Rechenzentrums kann auf eine große Anzahl von Spokes zentral hochskaliert werden. Dabei gelten jedoch wie bei jedem IT-System gewisse Plattformbeschränkungen. Die Hub-Bereitstellung ist an ein bestimmtes Azure-Abonnement gebunden. Beispielsweise kann eine maximale Anzahl von Peerings in virtuellen Netzwerken gelten. Weitere Informationen finden Sie unter [Grenzwerte für Azure-Abonnements und Dienste, Kontingente und Einschränkungen][Limits]. In Fällen, in denen diese Beschränkungen ein Problem darstellen, kann die Architektur weiter zentral hochskaliert werden. Dazu wird das Modell von einem einzelnen Hub und Spokes auf ein Cluster von Hub und Spokes erweitert. Mehrere Hubs in einer oder mehreren Azure-Regionen können mithilfe von Peering in virtuellen Netzwerken, ExpressRoute, Virtual WAN oder einem Site-to-Site-VPN miteinander verbunden werden.

[![2]][2]

Die Einführung von mehreren Hubs erhöht die Kosten und den Verwaltungsaufwand für das System. Dieser Aufwand lässt sich nur durch die Skalierbarkeit (z. B. Systemeinschränkungen oder Redundanz) und die regionale Replikation (z. B. Leistung für Endbenutzer oder Notfallwiederherstellung) rechtfertigen. In Szenarios, die mehrere Hubs erfordern, sollten alle Hubs die gleichen Dienste anbieten, um die Vorgänge nicht zu erschweren.

#### <a name="interconnection-between-spokes"></a>Verbindung zwischen Spokes
Innerhalb eines einzelnen Spokes ist es möglich, komplexe Workloads mit mehreren Ebenen zu implementieren. Sie können Konfigurationen mit mehreren Ebenen implementieren, indem Sie für jede Ebene im gleichen virtuellen Netzwerk ein Subnetz verwenden. Filtern Sie die Flüsse dabei mithilfe von Netzwerksicherheitsgruppen.

Ein Architekt möchte vielleicht eine Workload mit mehreren Ebenen in mehreren virtuellen Netzwerken bereitstellen. Mittels Peering in virtuellen Netzwerken können Spokes Verbindungen mit anderen Spokes im gleichen Hub oder in anderen Hubs herstellen. Ein typisches Beispiel für dieses Szenario ist die Platzierung von Anwendungsverarbeitungsservern in einem Spoke oder virtuellen Netzwerk, während die Datenbank in einem anderen Spoke oder virtuellen Netzwerk bereitgestellt wird. In diesem Fall ist es einfach, die Spokes mittels Peering in virtuellen Netzwerken zu verbinden und den Hub dabei zu umgehen. Die Architektur und Sicherheit sollten sorgfältig überprüft werden, um sicherzustellen, dass bei der Umgehung des Hubs keine wichtigen Sicherheits- oder Überwachungspunkte umgangen werden, die nur im Hub vorhanden sind.

[![3]][3]

Spokes können auch mit einem Spoke verbunden werden, der als Hub fungiert. Bei diesem Ansatz wird eine Hierarchie mit zwei Ebenen erstellt: Die Spokes auf höherer Ebene (Ebene 0) werden zum Hub der Spokes auf niedrigerer Ebene (Ebene 1) in der Hierarchie. Die Spokes einer Implementierung des virtuellen Rechenzentrums müssen den Datenverkehr an den zentralen Hub weiterleiten, um sich entweder mit dem lokalen Netzwerk oder dem Internet zu verbinden. Eine Architektur mit zwei Hub-Ebenen führt komplexes Routing ein, das die Vorteile einer einfachen Hub-Spoke-Beziehung aufhebt.

Obwohl Azure komplexe Topologien zulässt, ist eines der wesentlichen Prinzipien virtueller Rechenzentren die Wiederholbarkeit und Einfachheit. Um den Verwaltungsaufwand zu minimieren, empfehlen wir den einfachen Hub-Spoke-Entwurf als Referenzarchitektur für virtuelle Rechenzentren.

### <a name="components"></a>Komponenten
Eine Implementierung des virtuellen Rechenzentrums besteht aus vier grundlegenden Komponententypen: der **Infrastruktur**, den **Umkreisnetzwerken**, den **Workloads** und der **Überwachung**.

Jeder Komponententyp besteht aus verschiedenen Azure-Funktionen und -Ressourcen. Die Implementierung des virtuellen Rechenzentrums eines Unternehmens besteht aus Instanzen verschiedener Komponententypen und mehreren Varianten des gleichen Komponententyps. Angenommen, Sie haben viele verschiedene, logisch getrennte Workloadinstanzen, die unterschiedliche Anwendungen darstellen. Aus diesen verschiedenen Komponententypen und Instanzen erstellen Sie letztendlich das virtuelle Rechenzentrum.

[![4]][4]

Die vorangehende allgemeine Architektur einer Implementierung des virtuellen Rechenzentrums zeigt verschiedene Komponententypen, die in unterschiedlichen Zonen der Hub-Spoke-Topologie verwendet werden. Die Abbildung zeigt die Infrastrukturkomponenten in verschiedenen Teilen der Architektur.

Es empfiehlt sich, für ein lokales Rechenzentrum oder eine Implementierung des virtuellen Rechenzentrums gruppenbasierte Zugriffsrechte und Berechtigungen zu verwenden. Die Verwendung von Gruppen anstelle einzelner Benutzer erleichtert die teamübergreifende Verwaltung einheitlicher Zugriffsrichtlinien und minimiert Konfigurationsfehler. Weisen Sie Benutzer den entsprechenden Gruppen zu, und entfernen Sie sie aus Gruppen, um die Berechtigungen bestimmter Benutzer auf dem neuesten Stand zu halten.

Der Name jeder Rollengruppe sollte ein eindeutiges Präfix aufweisen. Anhand dieses Präfixes kann einfach ermittelt werden, welcher Gruppe eine Workload zugeordnet ist. Einer Workload, die einen Authentifizierungsdienst hostet, könnten beispielsweise Gruppen namens **AuthServiceNetOps**, **AuthServiceSecOps**, **AuthServiceDevOps** und **AuthServiceInfraOps** zugewiesen sein. Für zentrale Rollen oder Rollen, die in keinem Zusammenhang mit einem bestimmten Dienst stehen, könnte das Präfix **Corp** verwendet werden. Ein Beispiel hierfür ist **CorpNetOps**.

Viele Organisationen verwenden in etwa die folgenden Gruppen, um Rollen bereitzustellen:

-   Die zentrale IT-Gruppe **Corp** verfügt über die Besitzrechte zum Steuern von Infrastrukturkomponenten. Beispiele hierfür sind Netzwerke und die Sicherheit. Der Gruppe muss über die Rolle „Mitwirkender“ für das Abonnement, die Kontrolle über den Hub und die Rechte eines Mitwirkenden des virtuellen Netzwerks in den Spokes verfügen. Große Unternehmen teilen diese Verwaltungsaufgaben häufig zwischen mehreren Teams auf, beispielsweise zwischen einer Gruppe für Netzwerkvorgänge **CorpNetOps**, die sich ausschließlich um den Netzwerkbetrieb kümmert, und einer Gruppe für Sicherheitsvorgänge **CorpSecOps**, die für die Firewall- und Sicherheitsrichtlinien verantwortlich ist. In diesem speziellen Fall müssen zwei unterschiedliche Gruppen für die Zuweisung dieser benutzerdefinierten Rollen erstellt werden.
-   Die für die Entwicklung und Tests zuständige Gruppe **AppDevOps** ist für die Bereitstellung von App- oder Dienstworkloads verantwortlich. Diese Gruppe übernimmt die Rolle des VM-Mitwirkenden für IaaS-Bereitstellungen oder eine oder mehrere Rollen von PaaS-Mitwirkenden. Informationen dazu finden Sie unter [Integrierte Rollen für die rollenbasierte Zugriffssteuerung in Azure][Roles]. Optional benötigt das Entwicklungs- und Testteam möglicherweise Einblick in Sicherheitsrichtlinien, Netzwerksicherheitsgruppen und Routingrichtlinien sowie benutzerdefinierte Routen im Hub oder einem bestimmte Spoke. Zusätzlich zur Rolle des Mitwirkenden für Workloads benötigt diese Gruppe auch die Rolle des Netzwerklesers.
-   Die Betriebs- und Wartungsgruppe **CorpInfraOps** oder **AppInfraOps** ist für die Verwaltung von Workloads in der Produktion zuständig. Diese Gruppe muss ein Mitwirkender des Abonnements von Workloads in jedem Produktionsabonnement sein. Manche Organisationen sollten zudem prüfen, ob sie ein zusätzliches Team für den Eskalationssupport mit der Rolle des Mitwirkenden des Abonnements in der Produktion und im zentralen Hub-Abonnement benötigen. Diese zusätzliche Gruppe behebt potenzielle Konfigurationsprobleme in der Produktionsumgebung.

Das virtuelle Rechenzentrum ist so strukturiert, dass Gruppen, die für die mit der Verwaltung des Hubs betrauten zentralen IT-Gruppen erstellt werden, über entsprechende Gruppen auf der Workloadebene verfügen. Zusätzlich zur Verwaltung der Hub-Ressourcen könnten nur die zentralen IT-Gruppen den externen Zugriff und die Berechtigungen auf oberster Ebene für das Abonnement steuern. Allerdings wären Workloadgruppen in der Lage, Ressourcen und Berechtigungen ihres eigenen virtuellen Netzwerks unabhängig von der zentralen IT zu steuern.

Partitionieren Sie das virtuelle Rechenzentrum, um mehrere Projekte sicher in unterschiedlichen Branchenanwendungen zu hosten. Alle Projekte erfordern unterschiedliche isolierte Umgebungen, beispielsweise Entwicklung, Benutzerakzeptanztests und Produktion. Einzelne Azure-Abonnements für jede dieser Umgebungen stellen natürliche Isolationsstufen bereit.

[![5]][5]

Das obige Diagramm veranschaulicht die Beziehung zwischen den Projekten, Benutzern und Gruppen einer Organisation und den Umgebungen, in denen die Azure-Komponenten bereitgestellt werden.

In der IT ist eine Umgebung oder Ebene üblicherweise ein System, in dem mehrere Anwendungen bereitgestellt und ausgeführt werden. Große Unternehmen verwenden eine Entwicklungsumgebung, in der Änderungen vorgenommen und getestet werden, und eine Produktionsumgebung für Endbenutzer. Diese Umgebungen werden häufig durch mehrere Stagingumgebungen getrennt, um eine stufenweise Bereitstellung (Rollout), Tests und bei Problemen ein Rollback zu ermöglichen. Bereitstellungsarchitekturen unterscheiden sich erheblich, folgen in der Regel aber dem Grundmuster, das mit der Entwicklung (**DEV**) beginnt und mit der Produktion (**PROD**) endet.

Eine übliche Architektur für diese Arten von Umgebungen mit mehreren Ebenen besteht aus einer Azure DevOps-Umgebung für die Entwicklung und Tests, einer UAT-Umgebung für das Staging und einer Produktionsumgebung. Organisationen können einzelne oder mehrere Azure AD-Mandanten nutzen, um Zugriff und Rechte auf diese Umgebungen zu definieren. Das obige Diagramm veranschaulicht die Verwendung von zwei verschiedenen Azure AD-Mandanten: ein Mandant für Azure DevOps und Benutzerakzeptanztests und ein weiterer Mandant ausschließlich für die Produktion.

Wenn mehrere Azure AD-Mandanten vorhanden sind, müssen die Umgebungen getrennt werden. Für den Zugriff auf einen anderen Azure AD-Mandanten muss sich dieselbe Benutzergruppe (z. B. die zentrale IT) mit einem anderen URI authentifizieren, um die Rollen oder Berechtigungen der Azure DevOps-Umgebung oder der Produktionsumgebung eines Projekts zu ändern. Die Verwendung unterschiedlicher Benutzerauthentifizierungen für den Zugriff auf verschiedene Umgebungen reduziert das Risiko möglicher Ausfälle und anderer Probleme durch menschliches Versagen.

#### <a name="component-type-infrastructure"></a>Komponententyp: Infrastruktur
In diesem Komponententyp befindet sich der Großteil der unterstützenden Infrastruktur. Diese Komponente beansprucht auch die meiste Zeit Ihrer Teams für die zentrale IT, Sicherheit und Konformität.

[![6]][6]

Infrastrukturkomponenten stellen eine Verbindung zwischen den verschiedenen Komponenten des virtuellen Rechenzentrums bereit. Sie sind sowohl im Hub als auch in den Spokes vorhanden. Die Verantwortung für die Verwaltung und Wartung der Infrastrukturkomponenten wird in der Regel dem zentralen IT-Team oder dem Sicherheitsteam übertragen.

Eine der wichtigsten Aufgaben des IT-Infrastrukturteams ist die Gewährleistung der Konsistenz von IP-Adressenschemata im gesamten Unternehmen. Der private IP-Adressraum, der dem virtuellen Rechenzentrum zugewiesen ist, muss konsistent sein. Er darf sich nicht mit privaten IP-Adressen überlappen, die in Ihren lokalen Netzwerken zugewiesen sind.

Durch die Netzwerkadressenübersetzung (Network Adress Translation, NAT) auf den lokalen Edgeroutern oder in Azure-Umgebungen können IP-Adressenkonflikte vermieden werden. Ihre Verwendung bringt jedoch Komplikationen für die Infrastrukturkomponenten mit sich. Die Vereinfachung der Verwaltung ist eines der wichtigsten Ziele von virtuellen Rechenzentren. Aus diesem Grund ist es nicht empfehlenswert, die Netzwerkadressenübersetzung zur Behandlung von IP-Problemen zu verwenden.

Infrastrukturkomponenten bieten die folgenden Funktionen:

-   [**Identitäts-und Verzeichnisdienste**][AAD]. Der Zugriff auf jeden Ressourcentyp in Azure wird durch eine in einem Verzeichnisdienst gespeicherte Identität gesteuert. Der Verzeichnisdienst speichert die Liste der Benutzer und die Zugriffsrechte für Ressourcen in einem bestimmten Azure-Abonnement. Diese Dienste können rein cloudbasiert sein. Alternativ können sie mit einer in Azure Active Directory gespeicherten lokalen Identität synchronisiert werden.
-   [**Virtuelles Netzwerk**][VPN]. Virtuelle Netzwerke sind eine der wichtigsten Komponenten des virtuellen Rechenzentrums. Mithilfe von virtuellen Netzwerken können Sie eine Isolationsbegrenzung für Datenverkehr innerhalb der Azure Platform schaffen. Ein virtuelles Netzwerk besteht aus einem oder mehreren virtuellen Netzwerksegmenten. Jedes Segment verfügt über ein bestimmtes IP-Netzwerkpräfix, d. h. ein Subnetz. Das virtuelle Netzwerk definiert einen internen Umkreisbereich, in dem IaaS-VMs und PaaS-Dienste eine private Kommunikation herstellen können. VMs und PaaS-Dienste in einem virtuellen Netzwerk können nicht direkt mit VMs und PaaS-Diensten in einem anderen virtuellen Netzwerk kommunizieren. Diese Isolierung erfolgt auch dann, wenn der gleiche Kunde die beiden virtuellen Netzwerke im gleichen Abonnement erstellt. Isolation ist eine wichtige Eigenschaft, mit der sichergestellt wird, dass VMs und die Kommunikation von Kunden innerhalb eines virtuellen Netzwerks privat bleiben.
-   [**UDR**][UDR]. Datenverkehr in einem virtuellen Netzwerk wird standardmäßig basierend auf der Systemroutingtabelle weitergeleitet. Eine benutzerdefinierte Route ist eine benutzerdefinierte Routingtabelle, die Netzwerkadministratoren einem oder mehreren Subnetzen zuordnen können. Durch diese Zuordnung wird das Verhalten der Systemroutingtabelle überschrieben und ein Kommunikationspfad innerhalb eines virtuellen Netzwerks definiert. Benutzerdefinierte Routen gewährleisten, dass ausgehender Datenverkehr vom Spoke durch bestimmte benutzerdefinierte VMs oder virtuelle Netzwerkgeräte und Lastenausgleichsmodule im Hub und in den Spokes geleitet wird.
-   [**NSG**][NSG]. Eine Netzwerksicherheitsgruppe (NSG) ist eine Liste von Sicherheitsregeln, die als Datenverkehrsfilter für IP-Quellen, IP-Ziele, Protokolle, IP-Quellports und IP-Zielports fungiert. Die Netzwerksicherheitsgruppe kann für ein Subnetz und/oder eine virtuelle Netzwerkschnittstellenkarte (Network Interface Card, NIC), die mit einer Azure-VM verknüpft ist, gelten. Netzwerksicherheitsgruppen sind entscheidend für die Implementierung einer korrekten Flusssteuerung im Hub und in den Spokes. Das Maß an Sicherheit, das die Netzwerksicherheitsgruppen bieten, hängt davon ab, welche Ports Sie öffnen und zu welchem Zweck. Kunden sollten zusätzliche Filter für jede VM mit hostbasierten Firewalls anwenden, z. B. IPtables oder die Windows-Firewall.
-   [**DNS**][DNS]. Die Namensauflösung von Ressourcen in den virtuellen Netzwerken des virtuellen Rechenzentrums wird über das Domain Name System (DNS) bereitgestellt. Azure bietet DNS-Dienste für [öffentliche][DNS] und [private][PrivateDNS] Namensauflösung. Private Zonen bieten Namensauflösung in einem virtuellen Netzwerk sowie zwischen virtuellen Netzwerken. Sie können private Zonen, die sich über virtuelle Netzwerke in derselben Region erstrecken, und auch private Zonen zwischen Regionen und Abonnements einrichten. Für die öffentliche Auflösung bietet Azure DNS einen Hostingdienst für DNS-Domänen. Er ermöglicht eine Namensauflösung mithilfe der Microsoft Azure-Infrastruktur. Indem Sie Ihre Domänen in Azure hosten, können Sie Ihre DNS-Einträge unter Verwendung der gleichen Anmeldeinformationen, APIs, Tools und Abrechnungsabläufe wie bei Ihren anderen Azure-Diensten verwalten.
-   [**Abonnement**][SubMgmt]- und [**Ressourcengruppenverwaltung**][RGMgmt]. Ein Abonnement definiert eine natürliche Grenze, um mehrere Ressourcengruppen in Azure zu erstellen. Ressourcen in einem Abonnement werden in logischen Containern zusammengefasst, die als **Ressourcengruppen** bezeichnet werden. Die Ressourcengruppe stellt eine logische Gruppe dar, in der die Ressourcen des virtuellen Rechenzentrums organisiert werden.
-   [**RBAC**][RBAC]. Über die rollenbasierte Zugriffssteuerung können Organisationsrollen zusammen mit Zugriffsrechten für bestimmte Azure-Ressourcen zugeordnet werden, sodass Sie Benutzer auf eine bestimmte Teilmenge von Aktionen beschränken können. Mit RBAC können Sie Zugriff gewähren, indem Sie Benutzern, Gruppen und Anwendungen innerhalb des relevanten Bereichs die entsprechende Rolle zuweisen. Der Bereich einer Rollenzuweisung kann ein Azure-Abonnement, eine Ressourcengruppe oder eine einzelne Ressource sein. RBAC ermöglicht die Vererbung von Berechtigungen. Mit einer Rolle, die einem übergeordneten Bereich zugewiesen ist, wird außerdem der Zugriff auf die darin enthaltenen untergeordneten Elemente gewährt. Mit der rollenbasierten Zugriffssteuerung können Sie Aufgaben trennen und Benutzern nur den Zugriff gewähren, den sie zur Ausführung ihrer Aufgaben benötigen. Beispielsweise können Sie mit der rollenbasierten Zugriffssteuerung einem Mitarbeiter Zugriff zum Verwalten von VMs in einem Abonnement und einem anderen Mitarbeiter Zugriff zum Verwalten von SQL-Datenbanken im gleichen Abonnement gewähren.
-   [**Peering in virtuellen Netzwerken**][VNetPeering]. Die grundlegende Funktion für das Erstellen der Infrastruktur des virtuellen Rechenzentrums ist das Peering in virtuellen Netzwerken. Dabei werden zwei virtuelle Netzwerke in der gleichen Region über das Netzwerk des Azure-Rechenzentrums oder mithilfe des weltweiten Azure-Backbones regionsübergreifend verbunden.

#### <a name="component-type-perimeter-networks"></a>Komponententyp: Umkreisnetzwerke
Mit [Umkreisnetzwerkkomponenten][DMZ] bzw. einem DMZ-Netzwerk können Sie Netzwerkkonnektivität mit Ihren lokalen oder physischen Rechenzentrumsnetzwerken und darüber hinaus Konnektivität mit dem Internet bereitstellen. Dafür bringen Ihre Netzwerk- und Sicherheitsteams wahrscheinlich den Großteil ihrer Zeit auf.

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

Das obige Diagramm veranschaulicht die Erzwingung von zwei Umkreisnetzwerken mit Zugriff auf das Internet und ein lokales Netzwerk. Beide befinden sich in den DMZ- und vWAN-Hubs. Im DMZ-Hub kann das Umkreisnetzwerk zum Internet zur Unterstützung einer großen Anzahl von Branchenanwendungen mit mehreren Farmen von Web Application Firewalls (WAFs) oder Azure Firewall-Instanzen zentral hochskaliert werden. Im vWAN-Hub wird je nach Bedarf über ein VPN oder ExpressRoute eine hochgradig skalierbare Konnektivität zwischen Branches sowie zwischen Branches und Azure bereitgestellt.

[**Virtuelle Netzwerke**][VNet]. Der Hub basiert normalerweise auf einem virtuellen Netzwerk mit mehreren Subnetzen, um verschiedene Arten von Diensten zu hosten, die den Internetdatenverkehr über virtuelle Netzwerkgeräte, WAFs und Azure Application Gateway-Instanzen filtern und überprüfen.

[**Benutzerdefinierte Routen**][UDR]. Mit benutzerdefinierten Routen können Kunden Firewalls, IDS- oder IPS-Geräte und andere virtuelle Geräte bereitstellen. Sie können den Netzwerkdatenverkehr zum Erzwingen von Sicherheitsrichtlinien, zur Überwachung und zur Überprüfung über diese Sicherheitsgeräte routen. Benutzerdefinierte Routen können sowohl im Hub als auch in den Spokes erstellt werden. Sie stellen sicher, dass Datenverkehr durch die spezifischen benutzerdefinierten VMs, virtuellen Netzwerkgeräte und Lastenausgleichsmodule geleitet wird, die im virtuellen Rechenzentrum verwendet werden. Um zu gewährleisten, dass der von VMs im Spoke generierte Datenverkehr zu den richtigen virtuellen Geräten geleitet wird, muss eine benutzerdefinierte Route in den Subnetzen des Spokes festgelegt werden. Dazu legen Sie die Front-End-IP-Adresse des internen Lastenausgleichs als nächsten Hop fest. Der interne Lastenausgleich verteilt den internen Datenverkehr auf die virtuellen Geräte bzw. den Back-End-Pool des Lastenausgleichs.

[**Azure Firewall**][AzFW] ist ein verwalteter, cloudbasierter Netzwerksicherheitsdienst, der Ihre Azure Virtual Network-Ressourcen schützt. Es ist eine vollständig zustandsbehaftete Firewall-as-a-Service mit integrierter Hochverfügbarkeit und uneingeschränkter Cloudskalierbarkeit. Sie können Richtlinien zur Anwendungs- und Netzwerkkonnektivität übergreifend für Abonnements und virtuelle Netzwerke zentral erstellen, erzwingen und protokollieren. Azure Firewall verwendet eine statische öffentliche IP-Adresse für Ihre virtuellen Netzwerkressourcen. Dadurch können externe Firewalls Datenverkehr aus Ihrem virtuellen Netzwerk identifizieren. Der Dienst ist für Protokollierung und Analyse vollständig in Azure Monitor integriert.

[**Virtuelle Netzwerkgeräte**][NVA]. Im Hub wird das Umkreisnetzwerk mit Zugriff auf das Internet normalerweise über eine Azure Firewall-Instanz oder eine Farm von Firewalls oder Web Application Firewall (WAF) verwaltet.

In verschiedenen Branchen werden häufig viele Webanwendungen verwendet. Diese Anwendungen weisen häufig unterschiedliche Sicherheitsrisiken und Exploits auf. Eine Web Application Firewall ist ein spezielles Produkt, das Angriffe auf Webanwendungen (HTTP/HTTPS) genauer erkennt als eine generische Firewall. Verglichen mit herkömmlichen Firewalls besitzen WAFs einen Satz von bestimmten Features, um den internen Webserver vor Bedrohungen zu schützen.

Eine einzelne Implementierung des virtuellen Rechenzentrums muss jedoch innerhalb einer einzelnen Region gehostet werden, da die Abonnementanforderungen eine Implementierung in mehreren Regionen nicht zulassen. Aufgrund dieser Anforderung ist eine einzelne Implementierung des virtuellen Rechenzentrums anfällig für regionale Ausfälle.

Eine Azure Firewall-Instanz oder NVA-Firewallfarm wird über eine gemeinsame Verwaltungsebene ausgeführt. Sicherheitsregeln schützen die in den Spokes gehosteten Workloads und steuern den Zugriff auf lokale Netzwerke. Azure Firewall verfügt über integrierte Skalierbarkeit, während NVA-Firewalls hinter einem Lastenausgleich manuell skaliert werden können. Im Allgemeinen verfügt eine Firewallfarm im Vergleich zu einer Web Application Firewall über weniger spezialisierte Software. Sie hat jedoch einen umfassenderen Anwendungsbereich, sodass sie alle Arten von ein- und ausgehendem Datenverkehr filtern und überprüfen kann. Virtuelle Netzwerkgeräte für den NVA-Ansatz sind auf dem Azure Marketplace verfügbar und können von dort bereitgestellt werden.

Wir empfehlen, eine Gruppe von Azure Firewall-Instanzen oder virtuellen Netzwerkgeräten für Datenverkehr aus dem Internet zu verwenden. Verwenden Sie für Datenverkehr mit lokalem Ursprung eine andere Gruppe. Die Verwendung einer einzigen Gruppe von Firewalls für den gesamten Datenverkehr stellt ein Sicherheitsrisiko dar, da sie keinen wirksamen Sicherheitsbereich zwischen den beiden Arten von Netzwerkdatenverkehr bietet. Separate Firewallebenen vereinfachen die Überprüfung von Sicherheitsregeln und ermöglichen eine eindeutige Zuordnung von Regeln zu eingehenden Netzwerkanforderungen.

Die meisten großen Unternehmen verwalten mehrere Domänen. [Azure DNS][DNS] kann zum Hosten der DNS-Einträge für eine bestimmte Domäne verwendet werden. Die virtuelle IP-Adresse (VIP) des externen Azure-Lastenausgleichs oder der Web Application Firewall kann beispielsweise im **A**-Eintrag eines Azure DNS-Datensatzes registriert werden. Für die Verwaltung der privaten Adressräume innerhalb von virtuellen Netzwerken ist auch [**privates DNS**][PrivateDNS] verfügbar.

[**Azure Load Balancer**][ALB] bietet einen Dienst mit hoher Verfügbarkeit der Ebene 4 (TCP, UDP). Der Dienst kann eingehenden Datenverkehr zwischen Dienstinstanzen verteilen, die in einer Gruppe mit Lastenausgleich definiert sind. Der Datenverkehr, der von Front-End-Endpunkten (öffentliche oder private IP) an den Lastenausgleich gesendet wird, kann mit oder ohne Adressübersetzung auf eine Gruppe von Back-End-IP-Adresspools weiterverteilt werden. Beispiele hierfür sind virtuelle Netzwerkgeräte oder VMs.

Azure Load Balancer kann auch die Integrität der verschiedenen Serverinstanzen testen. Wenn ein Test nicht reagiert, sendet der Lastenausgleich keinen weiteren Datenverkehr an die fehlerhafte Instanz. Im virtuellen Rechenzentrum verteilt ein externer Lastenausgleich im Hub beispielsweise den Datenverkehr an virtuelle Netzwerkgeräte. In den Spokes verteilt er beispielsweise den Datenverkehr zwischen verschiedenen VMs in einer Anwendung mit mehreren Ebenen.

Der Microsoft-Dienst [**Azure Front Door Service**][AFD] (AFD) stellt eine hoch verfügbare und hochgradig skalierbare Plattform für die Beschleunigung von Webanwendungen mit globalem HTTP-Lastenausgleich, Anwendungsschutz und Content Delivery Network bereit. Azure Front Door Service wird an mehr als 100 Standorten im Edgebereich des globalen Netzwerks von Microsoft ausgeführt. Mit Azure Front Door Service können Sie Ihre dynamischen Webanwendungen und statischen Inhalte erstellen, ausführen und erweitern. Azure Front Door Service bietet Ihrer Anwendung en folgenden Nutzen: 
* Erstklassige Endbenutzerleistung
* Einheitliche Automatisierung der Regions- oder Stempelwartung
* BCDR-Automatisierung (Business Continuity & Disaster Recovery)
* Einheitliche Client- oder Benutzerinformationen 
* Zwischenspeichern 
* Erkenntnisse zu Diensten Die Plattform bietet Leistung, Zuverlässigkeit, Support-SLAs, Konformitätszertifizierungen und überwachbare Sicherheitsverfahren, die von Azure entwickelt, betrieben und nativ unterstützt werden.

[**Application Gateway**][AppGW] ist ein dediziertes virtuelles Gerät, das einen ADC (Application Delivery Controller) als Dienst bereitstellt und für Ihre Anwendung verschiedene Layer-7-Lastenausgleichsfunktionen bietet. Sie können die Produktivität von Webfarmen steigern, indem Sie die CPU-intensive SSL-Beendigung an die Application Gateway-Instanz abladen. Außerdem werden beispielsweise die folgenden Layer-7-Routingfunktionen bereitgestellt: 
* Roundrobin-Verteilung des eingehenden Datenverkehrs. 
* Cookiebasierte Sitzungsaffinität: 
* Routing auf URL-Pfadbasis. 
* Die Möglichkeit, mehrere Websites hinter einer einzelnen Application Gateway-Instanz zu hosten. Eine Web Application Firewall (WAF) wird ebenfalls als Teil der WAF-SKU von Application Gateway bereitgestellt. Dieser SKU bietet Schutz für Webanwendungen vor allgemeinen Onlinesicherheitsrisiken und Exploits. Application Gateway kann als Gateway mit Internetzugriff, rein internes Gateway oder als Kombination dieser beiden Optionen konfiguriert werden. 

[**Öffentliche IP-Adressen**][PIP]. Einige Azure-Features ermöglichen es Ihnen, einer öffentlichen IP-Adresse Dienstendpunkte zuzuordnen, sodass über das Internet auf die Ressource zugegriffen werden kann. Der Endpunkt verwendet die Netzwerkadressenübersetzung, um Datenverkehr zur internen Adresse und zum Port im virtuellen Azure-Netzwerk zu leiten. Dieser Pfad ist der primäre Weg, auf dem externer Datenverkehr in das virtuelle Netzwerk gelangt. Sie können die öffentlichen IP-Adressen konfigurieren, um festzulegen, welcher Datenverkehr zugelassen wird bzw. wie und wo eine Übersetzung in das virtuelle Netzwerk stattfindet.

[**Azure DDoS Protection Standard**][DDOS] stellt über den [Diensttarif „Basic“][DDOS] zusätzliche Funktionen zur Bedrohungsabwehr bereit, die speziell für Azure Virtual Network-Ressourcen optimiert sind. DDoS Protection Standard kann leicht aktiviert werden und erfordert keine Änderung der Anwendung. Schutzrichtlinien werden über dedizierte Datenverkehrsüberwachung und Machine Learning-Algorithmen optimiert. Richtlinien werden auf öffentliche IP-Adressen angewendet, die in virtuellen Netzwerken bereitgestellten Ressourcen zugeordnet sind. Beispiele hierfür sind Instanzen von Azure Load Balancer, Azure Application Gateway und Azure Service Fabric. Über Azure Monitor-Ansichten steht Echtzeit-Telemetrie während eines Angriffs und für den Verlauf zur Verfügung. Mit der Web Application Firewall für Azure Application Gateway können Sie Schutz auf der Anwendungsebene hinzufügen. Der Schutz wird für öffentliche Azure-IPv4-Adressen bereitgestellt.

#### <a name="component-type-monitoring"></a>Komponententyp: Überwachung
Überwachungskomponenten bieten Sichtbarkeit und Warnungen aus allen anderen Komponententypen. Alle Teams sollten die Komponenten und Dienste, auf die sie Zugriff haben, überwachen können. Wenn Sie einen zentralen Helpdesk oder Betriebsteams haben, benötigen diese integrierten Zugriff auf die von diesen Komponenten bereitgestellten Daten.

Azure bietet verschiedene Protokollierungs- und Überwachungsdienste zum Nachverfolgen des Verhaltens von in Azure gehosteten Ressourcen. Die Governance und Steuerung von Workloads in Azure basiert nicht nur auf dem Sammeln von Protokolldaten, sondern auch auf der Möglichkeit, Aktionen auf der Grundlage von bestimmten gemeldeten Ereignissen auszulösen.

[**Azure Monitor**][Monitor]. Azure umfasst mehrere Dienste, die eine bestimmte Rolle oder Aufgabe im Überwachungsbereich ausführen. Zusammen bieten diese Dienste eine umfassende Lösung für das Sammeln, Übertragen und Analysieren von Telemetriedaten aus Ihrer Anwendung und den zugrunde liegenden Azure-Ressourcen, die sie unterstützen. Sie können auch kritische lokale Ressourcen überwachen und so eine hybride Überwachungsumgebung bilden. Voraussetzung der Entwicklung einer vollständigen Überwachungsstrategie für Ihre Anwendung ist die Kenntnis der verfügbaren Tools und Daten.

Es gibt zwei Hauptarten von Protokollen in Azure:

-   Das [Azure-Aktivitätsprotokoll][ActLog] (früher als **Betriebsprotokolle** bezeichnet) bietet Erkenntnisse zu den Vorgängen, die für Ressourcen im Azure-Abonnement ausgeführt wurden. Diese Protokolle melden die Ereignisse der Steuerungsebene für Ihre Abonnements. Jede Azure-Ressource erzeugt Überwachungsprotokolle.

-   [Azure Monitor-Diagnoseprotokolle][DiagLog] sind von einer Ressource generierte Protokolle mit umfangreichen, in kurzen Abständen erfassten Betriebsdaten der Ressource. Der Inhalt dieser Protokolle variiert je nach Ressourcentyp.

[![9]][9]

Im virtuellen Rechenzentrum ist es wichtig, die Protokolle von Netzwerksicherheitsgruppen und insbesondere die folgenden Informationen nachzuverfolgen:

-   [Ereignisprotokolle][NSGLog] enthalten Informationen zu den NSG-Regeln, die auf Grundlage der MAC-Adresse auf VMs und Instanzrollen angewendet werden.
-   [Leistungsindikatorprotokolle][NSGLog] verfolgen, wie oft jede NSG-Regel ausgeführt wurde, um Datenverkehr zuzulassen oder zu verweigern.

Alle Protokolle können zur Überwachung und statischen Analyse oder zu Sicherungszwecken in Azure-Speicherkonten gespeichert werden. Wenn Sie die Protokolle in einem Azure-Speicherkonto speichern, können Kunden verschiedene Typen von Frameworks zum Abrufen, Vorbereiten, Analysieren und Visualisieren dieser Daten verwenden, um den Status und die Integrität von Cloudressourcen zu melden. 

Große Unternehmen sollten bereits über ein Standardframework für die Überwachung lokaler Systeme verfügen. Sie können dieses Framework erweitern, um von Cloudbereitstellungen generierte Protokolle einzubinden. Mithilfe von [Azure Log Analytics] [https://docs.microsoft.com/en-us/azure/log-analytics/log-analytics-queries] können Organisationen die gesamte Protokollierung in der Cloud ausführen. Log Analytics wird als cloudbasierter Dienst implementiert und ist daher mit minimalen Investitionen in Infrastrukturdienste schnell betriebsbereit. Log Analytics kann auch in System Center-Komponenten wie System Center Operations Manager integriert werden, um Ihre bestehenden Investitionen in die Cloud zu erweitern. 

Log Analytics ist ein Dienst in Azure, mit dem von Betriebssystemen, Anwendungen und Komponenten der Cloud-Infrastruktur generierte Protokoll- und Leistungsdaten erfasst, korreliert, durchsucht und bearbeitet werden können. Der Dienst liefert Kunden in Echtzeit betriebliche Erkenntnisse, da er mithilfe der integrierten Suche und mit benutzerdefinierten Dashboards die Analyse aller Datensätze für sämtliche Workloads im virtuellen Rechenzentrum ermöglicht.

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
Workload-Komponenten befinden sich dort, wo auch Ihre tatsächlichen Anwendungen und Dienste sind. Für diese Komponenten wenden Ihre Anwendungsentwicklungsteams den Großteil ihrer Zeit auf.

Die Möglichkeiten für Workloads sind endlos. Im Folgenden finden Sie einige der möglichen Workload-Typen:

**Interne Branchenanwendungen** sind Computeranwendungen, die für den laufenden Betrieb eines Unternehmens entscheidend sind. Branchenanwendungen haben einige gemeinsame Merkmale:

-   Sie sind von Natur aus **interaktiv**. Daten werden eingegeben, und Ergebnisse oder Berichte werden zurückgegeben.
-   Sie sind **datengesteuert**. Branchenanwendungen sind datenintensiv und greifen häufig auf Datenbanken oder einen anderen Speicher zu.
-   Sie sind **integriert**. Branchenanwendungen ermöglichen die Integration in andere Systeme innerhalb oder außerhalb der Organisation.

**Websites für Kunden (im Internet oder intern)**. Die meisten Anwendungen, die mit dem Internet interagieren, sind Websites. Azure bietet die Möglichkeit, eine Website auf einer IaaS-VM oder über eine Website des [Web-Apps-Features von Microsoft Azure App Service][WebApps] (PaaS) auszuführen. Web-Apps unterstützen die Integration in virtuelle Netzwerke, die die Bereitstellung von Web-Apps im Spoke des virtuellen Rechenzentrums ermöglichen. Bei intern zugänglichen Websites mit Integration in virtuelle Netzwerke müssen Sie keinen Internetendpunkt für Ihre Anwendungen verfügbar machen. Stattdessen können Sie die Ressourcen über private, nicht über das Internet routingfähige Adressen Ihres privaten virtuellen Netzwerks verwenden.

**Big Data und Analysen**. Wenn Daten zentral auf ein großes Volumen hochskaliert werden müssen, kann es passieren, dass Datenbanken nicht korrekt zentral hochskaliert werden. Die Apache Hadoop-Technologie bietet ein System zum parallelen Ausführen von verteilten Abfragen auf einer großen Anzahl von Knoten. Kunden können Datenworkloads auf IaaS-VMs oder PaaS ausführen ([Azure HDInsight][HDI]). HDInsight unterstützt die Bereitstellung in einem standortbasierten virtuellen Netzwerk. Der Dienst kann in einem Cluster in einem Spoke des virtuellen Rechenzentrums bereitgestellt werden.

**Ereignisse und Messaging**. [Azure Event Hubs][EventHubs] ist ein hyperskalierbarer Dienst für die Erfassung von Telemetriedaten, der Millionen von Ereignissen sammelt, transformiert und speichert. Als verteilte Streamingplattform bietet der Dienst niedrige Wartezeiten und konfigurierbare Aufbewahrungszeiten. Sie können somit riesige Mengen an Telemetriedaten in Azure erfassen und diese Daten in verschiedenen Anwendungen lesen. In Event Hubs kann ein einziger Datenstrom in Echtzeit und batchbasierte Pipelines unterstützen.

Über [Azure Service Bus][ServiceBus] können Sie einen zuverlässigen Cloudmessagingdienst zwischen Anwendungen und Diensten implementieren. Der Dienst bietet asynchrones Brokermessaging zwischen Client und Server, strukturiertes FIFO-Messaging (First In, First Out) sowie Funktionen zum Veröffentlichen und Abonnieren.

[![10]][10]

### <a name="multiple-vdc-implementations"></a>Mehrere Implementierungen des virtuellen Rechenzentrums


Ein einzelnes virtuelles Rechenzentrum wird in einer einzigen Region gehostet. Es ist daher anfällig für einen größeren Ausfall, der die gesamte Region betreffen kann. Kunden, die hohe SLAs erzielen möchten, müssen ihre Dienste schützen, indem sie dasselbe Projekt in zwei (oder mehr) Implementierungen des virtuellen Rechenzentrums in unterschiedlichen Regionen bereitstellen.

Zusätzlich zu den Überlegungen zu SLAs gibt es einige allgemeine Szenarien, in denen die Bereitstellung mehrerer Implementierungen des virtuellen Rechenzentrums sinnvoll ist:

-   Regionale oder globale Präsenz
-   Notfallwiederherstellung
-   Mechanismus zum Umleiten von Datenverkehr zwischen Rechenzentren

#### <a name="regional-and-global-presence"></a>Regionale und globale Präsenz
Azure-Rechenzentren befinden sich in vielen Regionen auf der ganzen Welt. Kunden, die mehrere Azure-Rechenzentren auswählen, müssen zwei zusammenhängende Faktoren berücksichtigen: die geografische Entfernung und die Wartezeit. Um die bestmögliche Benutzererfahrung zu bieten, müssen Kunden die geografische Entfernung zwischen den Implementierungen des virtuellen Rechenzentrums sowie die Entfernung zwischen den Implementierungen des virtuellen Rechenzentrums und den Endbenutzern bedenken.

Die Azure-Region, in der die Implementierungen des virtuellen Rechenzentrums gehostet werden, muss auch die gesetzlichen Anforderungen des Rechtsraums erfüllen, in dem Ihre Organisation tätig ist.

#### <a name="disaster-recovery"></a>Notfallwiederherstellung
Die Implementierung des Notfallwiederherstellungsplans ist abhängig vom betroffenen Workloadtyp und der Möglichkeit, den Zustand der Workload zwischen verschiedenen Implementierungen des virtuellen Rechenzentrums zu synchronisieren. Die meisten Kunden möchten im Idealfall Anwendungsdaten zwischen Bereitstellungen synchronisieren, die in zwei verschiedenen Implementierungen des virtuellen Rechenzentrums ausgeführt werden, um einen schnellen Failovermechanismus zu implementieren. Die meisten Anwendungen reagieren empfindlich auf Latenz, was mögliche Timeouts und Verzögerungen bei der Datensynchronisierung verursachen kann.

Die Synchronisierung oder Heartbeatüberwachung von Anwendungen in verschiedenen Implementierungen des virtuellen Rechenzentrums erfordert eine Kommunikation zwischen den Implementierungen. Zwei Implementierungen des virtuellen Rechenzentrums in unterschiedlichen Regionen können wie folgt verbunden werden:

-   Mittels Peering in virtuellen Netzwerken können Hubs regionsübergreifend verbunden werden.
-   Mittels privatem Peering mit ExpressRoute, wenn die Hubs der virtuellen Rechenzentren mit der gleichen ExpressRoute-Verbindung verbunden sind.
-   Mittels mehrerer ExpressRoute-Verbindungen, die über Ihren firmeneigenen Backbone und Ihre Cloud von virtuellen Rechenzentren mit den ExpressRoute-Verbindungen verbunden sind.
-   Mittels Site-to-Site-VPN-Verbindungen zwischen den Hubs Ihrer virtuellen Rechenzentren in jeder Azure-Region.

Normalerweise werden das Peering in virtuellen Netzwerken oder ExpressRoute-Verbindungen aufgrund der höheren Bandbreite und der konsistenten Wartezeit bei der Übertragung über den Microsoft-Backbone bevorzugt.

Es gibt kein Patentrezept, um eine Anwendung zu überprüfen, die zwischen zwei oder mehr verschiedenen Implementierungen des virtuellen Rechenzentrums in unterschiedlichen Regionen verteilt ist. Kunden sollten Netzwerkqualifizierungstests ausführen, um die Wartezeit und Bandbreite der Verbindungen zu überprüfen. Anschließend sollten sie analysieren, ob die synchrone oder asynchrone Datenreplikation geeignet ist und welche RTO (Recovery Time Objective) für ihre Workloads optimal ist.

#### <a name="mechanism-to-divert-traffic-between-datacenters"></a>Mechanismus zum Umleiten von Datenverkehr zwischen Rechenzentren
Eine effektive Methode, um den in einem Rechenzentrum eingehenden Datenverkehr zu einem anderen Rechenzentrum umzuleiten, beruht auf dem Domain Name System (DNS). [Microsoft Azure Traffic Manager][TM] verwendet den DNS-Mechanismus, um Endbenutzerdatenverkehr an den am besten geeigneten öffentlichen Endpunkt in einem bestimmten virtuellen Rechenzentrum weiterzuleiten. Mithilfe von Tests überprüft Traffic Manager regelmäßig die Dienstintegrität von öffentlichen Endpunkten in verschiedenen Implementierungen des virtuellen Rechenzentrums. Bei einem Ausfall dieser Endpunkte wird der Datenverkehr automatisch zum sekundären virtuellen Rechenzentrum geleitet.

Traffic Manager funktioniert für öffentliche Azure-Endpunkte. Der Dienst kann beispielsweise den an Azure-VMs und Web-Apps im entsprechenden virtuellen Netzwerk gesendeten Datenverkehr steuern bzw. umleiten. Traffic Manager ist selbst bei einem Ausfall einer ganzen Azure-Region resilient. Der Dienst kann die Verteilung von Benutzerdatenverkehr für Dienstendpunkte in verschiedenen Implementierungen des virtuellen Rechenzentrums auf der Grundlage mehrerer Kriterien steuern. Beispiele sind der Ausfall eines Diensts in einem bestimmten virtuellen Rechenzentrum oder die Auswahl des virtuellen Rechenzentrums mit der geringsten Netzwerkwartezeit für den Client.

### <a name="conclusion"></a>Zusammenfassung
Das virtuelle Rechenzentrum ist eine Methode für die Migration von Rechenzentren zur Cloud, bei der eine Kombination von Features und Funktionen verwendet wird, um eine skalierbare Architektur in Azure zu erstellen. Es maximiert die Nutzung von Cloudressourcen, senkt die Kosten und vereinfacht die Systemgovernance. Das Konzept des virtuellen Rechenzentrums basiert auf einer Hub-Spoke-Topologie, in der allgemeine freigegebene Dienste im Hub bereitgestellt werden. Es ermöglicht die Ausführung bestimmter Anwendungen oder Workloads in den Spokes. Das virtuelle Rechenzentrum entspricht der Struktur von Unternehmensrollen: Verschiedene Abteilungen (beispielweise zentrale IT, Azure DevOps, Betrieb und Wartung) arbeiten zusammen und verfügen jeweils über eine bestimmte Liste von Rollen und Aufgaben. Das Konzept des virtuellen Rechenzentrums erfüllt die Anforderungen einer **Lift & Shift**-Migration. Darüber hinaus bietet es aber auch zahlreiche Vorteile für native Cloudbereitstellungen.

## <a name="references"></a>Referenzen
Erfahren Sie mehr über die folgenden Features, die in diesem Artikel erörtert wurden:

| | | |
|-|-|-|
|Netzwerkfunktionen|Lastenausgleich|Konnektivität|
|[Azure Virtual Network][VNet]</br>[Netzwerksicherheitsgruppen][NSG]</br>[Protokolle für Netzwerksicherheitsgruppen][NSGLog]</br>[Benutzerdefinierte Routen][UDR]</br>[Virtuelle Netzwerkgeräte][NVA]</br>[Öffentliche IP-Adressen][PIP]</br>[Azure DDoS Protection][DDOS]</br>[Azure Firewall][AzFW]</br>[Azure DNS][DNS]|[Azure Front Door Service][AFD]</br>[Azure Load Balancer (L3)][ALB]</br>[Application Gateway (L7)][AppGW]</br>[Web Application Firewall][WAF]</br>[Azure Traffic Manager][TM]</br></br></br></br></br> |[Peering in virtuellen Netzwerken][VNetPeering]</br>[Virtuelles privates Netzwerk][VPN]</br>[Virtual WAN][vWAN]</br>[ExpressRoute][ExR]</br>[ExpressRoute Direct][ExRD]</br></br></br></br></br>
|Identity</br>|Überwachung</br>|Bewährte Methoden</br>|
|[Azure Active Directory][AAD]</br>[Multi-Factor Authentication][MFA]</br>[Rollenbasierte Zugriffssteuerung][RBAC]</br>[Azure AD-Standardrollen][Roles]</br></br></br> |[Network Watcher][NetWatch]</br>[Azure Monitor][Monitor]</br>[Aktivitätsprotokoll][ActLog]</br>[Azure Monitor-Diagnoseprotokolle][DiagLog]</br>[Microsoft Operations Management Suite][OMS]</br>[Netzwerkleistungsmonitor][NPM]|[Bewährte Methoden für Umkreisnetzwerke][DMZ]</br>[Abonnementverwaltung][SubMgmt]</br>[Verwaltung von Ressourcengruppen][RGMgmt]</br>[Einschränkungen für Azure-Abonnements][Limits] </br></br></br>|
|Weitere Azure-Dienste|
|[Web-Apps][WebApps]</br>[HDInsight (Hadoop)][HDI]</br>[Event Hubs][EventHubs]</br>[Service Bus][ServiceBus]|

## <a name="next-steps"></a>Nächste Schritte
 - Informieren Sie sich über das [Peering in virtuellen Netzwerken][VNetPeering], die technologische Grundlage für Hub-Spoke-Entwürfe von virtuellen Rechenzentren.
 - Implementieren Sie [Azure AD][AAD], um die Funktionen der [rollenbasierten Zugriffssteuerung][RBAC] zu erkunden.
 - Entwickeln Sie Modelle für die Abonnement- und Ressourcenverwaltung sowie die rollenbasierte Zugriffssteuerung, die der Struktur, den Anforderungen und den Richtlinien Ihrer Organisation entsprechen. Die wichtigste Aktivität ist die Planung. Berücksichtigen Sie bei Ihrer Planung nach Möglichkeit Umstrukturierungen, Fusionen, neue Produktlinien und andere Möglichkeiten.

<!--Image References-->
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

