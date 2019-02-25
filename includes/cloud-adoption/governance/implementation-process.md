<!-- TEMPLATE FILE - DO NOT ADD METADATA -->

## <a name="dependent-decisions"></a>Abhängige Entscheidungen

Die folgenden Entscheidungen stammen von Teams außerhalb des Cloudgovernanceteams. Die Implementierung wird von denselben Teams durchgeführt. Das Cloudgovernanceteam ist jedoch verantwortlich für die Implementierung einer Lösung, die sicherstellt, dass diese Implementierungen konsistent angewendet werden.

### <a name="identity-baseline"></a>Identitätsbaseline

Die Identitätsbaseline ist der grundlegende Ausgangspunkt für die gesamte Governance. Bevor Sie versuchen, Governance anzuwenden, muss eine Identität eingerichtet werden. Die eingerichtete Identitätsstrategie wird anschließend durch die Governancelösungen erzwungen.
In diesem Governancevorgang implementiert das Identitätsverwaltungsteam das Muster für die **Verzeichnissynchronisierung**:

- RBAC wird von Azure Active Directory (Azure AD) über die Verzeichnissynchronisierung oder dieselbe Anmeldung bereitgestellt, die während der Migration des Unternehmens zu Office 365 implementiert wurde. Einen Implementierungsleitfaden finden Sie unter [Referenzarchitektur für die Azure AD-Integration](/azure/architecture/reference-architectures/identity/azure-ad).
- Der Azure AD-Mandanten steuert außerdem die Authentifizierung und den Zugriff für Ressourcen, die in Azure bereitgestellt werden.

Im Governance-MVP erzwingt das Governanceteam die Anwendung des replizierten Mandanten über Tools für die Abonnementgovernance, die [weiter unten in diesem Artikel](#subscription-model) erläutert werden. In der Weiterentwicklung kann das Governanceteam auch umfassende Tools in Azure AD durchsetzen, um diese Funktion zu erweitern.

### <a name="security-baseline-networking"></a>Sicherheitsbaseline: Netzwerk

Ein softwaredefiniertes Netzwerk ist ein wichtiger Anfangsaspekt der Sicherheitsbaseline. Die Einrichtung des Governance-MVP hängt von frühen Entscheidungen aus dem Team zur Sicherheitsverwaltung ab, das die sichere Konfiguration von Netzwerken definiert.

Da keine Anforderungen bestehen, geht die IT-Sicherheit auf Nummer sicher und hat ein **Cloud-DMZ**-Muster angefordert. Das bedeutet, dass die Governance der Azure-Bereitstellungen selbst sehr gering sein wird.

- Azure-Abonnements können über VPN eine Verbindung mit einem vorhandenen Rechenzentrum herstellen, müssen aber alle vorhandenen lokalen Richtlinien der IT-Governance in Bezug auf die Verbindung einer demilitarisierte Zone mit geschützten Ressourcen befolgen. Eine Implementierungsanleitung in Bezug auf die VPN-Konnektivität finden Sie unter [VPN-Referenzarchitektur](/azure/architecture/reference-architectures/hybrid-networking/vpn).
- Entscheidungen in Bezug auf das Subnetz, die Firewall und das Routing werden derzeit auf die einzelnen Anwendungs-/Workloadleads verschoben.
- Vor der Freigabe geschützter Daten oder unternehmenskritischer Workloads sind zusätzliche Analysen erforderlich.

Bei diesem Muster können Cloudnetzwerke eine Verbindung mit lokalen Ressourcen nur über ein vorab zugeordnetes VPN herstellen, das mit Azure kompatibel ist. Der Datenverkehr über diese Verbindung wird wie jeder andere Datenverkehr aus einer demilitarisierten Zone behandelt. Für das lokale Edgegerät sind möglicherweise weitere Überlegungen erforderlich, um den Datenverkehr von Azure sicher zu verarbeiten.

Das Cloudgovernanceteam hat Mitglieder der Netzwerk- und IT-Sicherheitsteams proaktiv zu regelmäßigen Besprechungen eingeladen, um den Netzwerkanforderungen und Risiken einen Schritt voraus zu bleiben.

### <a name="security-baseline-encryption"></a>Sicherheitsbaseline: Verschlüsselung

Die Verschlüsselung ist eine weitere grundlegende Entscheidung innerhalb der Disziplin „Sicherheitsbaseline“. Da das Unternehmen derzeit noch keine geschützten Daten in der Cloud speichert, hat das Sicherheitsteam sich für ein weniger aggressives Muster für die Verschlüsselung entschieden.
An dieser Stelle wird ein **cloudnatives** Muster für die Verschlüsselung vorgeschlagen, aber von keinem Entwicklungsteam verlangt.

- Im Hinblick auf den Einsatz der Verschlüsselung wurden keine Governanceanforderungen festgelegt, weil die aktuelle Unternehmensrichtlinie keine unternehmenswichtigen oder geschützten Daten in der Cloud zulässt.
- Vor der Freigabe geschützter Daten oder unternehmenskritischer Workloads sind zusätzliche Analysen erforderlich.

## <a name="policy-enforcement"></a>Durchsetzung von Richtlinien

Die erste Entscheidung in Bezug auf die Beschleunigung der Bereitstellung ist das Muster für die Erzwingung. In dieser Geschichte hat sich das Governanceteam dafür entschieden, das Muster **Automatisierte Erzwingung** zu implementieren.

- Azure Security Center wird den Teams für Sicherheit und Identität zur Überwachung von Sicherheitsrisiken zur Verfügung gestellt. Außerdem verwenden beide Teams wahrscheinlich Security Center, um neue Risiken zu identifizieren und die Unternehmensrichtlinien weiterzuentwickeln.
- RBAC ist in allen Abonnements erforderlich, um die Durchsetzung der Authentifizierung zu erzwingen.
- Azure Policy wird in den einzelnen Verwaltungsgruppen veröffentlicht und auf alle Abonnements angewendet. Die Ebene der erzwungenen Richtlinien wird in diesem ersten Governance-MVP jedoch sehr begrenzt sein.
- Auch wenn Azure-Verwaltungsgruppen verwendet werden, wird eine relativ einfache Hierarchie erwartet.
- Azure Blueprints dient zum Bereitstellen und Aktualisieren von Abonnements, indem RBAC-Anforderungen, Resource Manager-Vorlagen und Azure Policy verwaltungsgruppenübergreifend angewendet werden.

## <a name="applying-the-dependent-patterns"></a>Anwenden der abhängigen Muster

Die folgenden Entscheidungen stellen die Muster dar, die durch die oben dargestellte Strategie zur Richtliniendurchsetzung erzwungen werden sollen:

**Identitätsbaseline**. RBAC-Anforderungen werden von Azure Blueprints auf Abonnementebene festgelegt, um sicherzustellen, dass für alle Abonnements eine konsistente Identität konfiguriert wird.

**Sicherheitsbaseline: Netzwerk**. Das Cloudgovernanceteam verwaltet eine Resource Manager-Vorlage für die Einrichtung eines VPN-Gateways zwischen Azure und dem lokalen VPN-Gerät. Wenn ein Anwendungsteam eine VPN-Verbindung benötigt, wendet das Cloudgovernanceteam die Resource Manager-Vorlage für das Gateway über Azure Blueprints an.

**Sicherheitsbaseline: Verschlüsselung**. An diesem Punkt des Vorgangs müssen in diesem Bereich keine Richtlinien erzwungen werden. In der späteren Weiterentwicklung wird dieser Vorgang noch einmal aufgerufen.
