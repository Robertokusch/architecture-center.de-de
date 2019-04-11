---
title: 'CAF: Schaffen von Hybrid Cloud-Konsistenz'
description: Festlegen der Vorgehensweise zum Schaffen von Hybrid Cloud-Konsistenz
author: BrianBlanchard
ms.date: 12/27/2018
ms.openlocfilehash: 22637a0496dc1e776d00570c3ef9844cc185a35d
ms.sourcegitcommit: c053e6edb429299a0ad9b327888d596c48859d4a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/20/2019
ms.locfileid: "58243091"
---
# <a name="create-hybrid-cloud-consistency"></a>Schaffen von Hybrid Cloud-Konsistenz

Dieser Artikel führt Sie durch allgemeine Ansätze zum Schaffen von Hybrid Cloud-Konsistenz.

Hybridbereitstellungsmodelle können während der Migration das Risiko minimieren und zu einem problemlosen Übergang der Infrastruktur beitragen. Cloudplattformen bieten das höchste verfügbare Maß an Flexibilität bei Geschäftsprozessen. Viele Organisationen zögern beim Umstieg auf die Cloud und bevorzugen stattdessen, die vollständige Kontrolle über die meisten vertraulichen Daten zu behalten. Leider bieten lokale Server nicht die gleichen Innovationen wie die Cloud. Mit einer Hybrid Cloud-Lösung sichern Sie sich das Beste aus beiden Welten: Die Geschwindigkeit von Cloudinnovationen UND den Komfort einer lokalen Verwaltung.

## <a name="integrate-hybrid-cloud-consistency"></a>Integrieren von Hybrid Cloud-Konsistenz

Durch die Verwendung einer Hybrid Cloud-Lösung können Unternehmen Rechenressourcen skalieren. Außerdem ist damit weniger Kapitalaufwand erforderlich, um kurzfristige Spitzen im Bedarf zu kompensieren. Wenn veränderte Geschäftsbedingungen die Freigabe lokaler Ressourcen für vertraulichere Daten oder Anwendungen erfordern, ist es einfacher, schneller und günstiger, die Bereitstellung von Cloudressourcen aufzuheben. Sie bezahlen nur für die Ressourcen, die Ihre Organisation vorübergehend nutzt, und müssen keine zusätzlichen Ressourcen erwerben und verwalten. Dies reduziert die Menge der Geräte, die über einen längeren Zeitraum ungenutzt bleiben könnten. Hybrid Cloud-Computing ist eine Plattform, die das Beste der verschiedenen Typen in sich vereint: Sie verbindet die Vorzüge des Cloud Computings (Flexibilität, Skalierbarkeit und Kosteneffizienz) mit dem geringstmöglichen Offenlegungsrisiko für die Daten.

![Schaffen von Hybrid Cloud-Konsistenz für Identitäten, Verwaltung, Sicherheit, Daten, Entwicklung und DevOps](../../_images/hybrid-consistency.png)
*Abbildung 1. Schaffen von Hybrid Cloud-Konsistenz für Identitäten, Verwaltung, Sicherheit, Daten, Entwicklung und DevOps*

Eine echte Hybrid Cloud-Lösung muss vier Komponenten bereitstellen, von denen jede entscheidende Vorteile bietet:

- Einheitliche Identitäten für lokale und cloudbasierte Anwendungen: Dies verbessert die Produktivität der Benutzer, da diese das einmalige Anmelden (SSO) für alle ihre Anwendungen nutzen können. Zusätzlich wird Konsistenz gewährleistet, da Anwendungen und Benutzer die Grenzen von Netzwerk und Cloud überschreiten können.
- Integrierte Verwaltung und Sicherheit in der Hybrid Cloud: Nutzen Sie einen zentralen Ort zum Überwachen, Verwalten und Schützen der Umgebung bei mehr Transparenz und Kontrolle.
- Eine einheitliche Datenplattform für das Rechenzentrum und die Cloud: Dies schafft Datenportabilität und sorgt für einen nahtlosen Zugriff auf lokale und cloudbasierte Datendienste und damit für umfassendere Einblicke in alle Datenquellen.
- Konsistenz bei Entwicklung und DevOps in Cloud- und lokalen Rechenzentren: Dies ermöglicht es Ihnen, Anwendungen bei Bedarf zwischen den beiden Umgebungen zu verschieben, um so die Produktivität von Entwicklern zu verbessern, da für beide Orte jetzt die gleiche Entwicklungsumgebung verwendet wird.
  
Beispiele für diese Komponenten aus Sicht von Azure:

- Azure Active Directory (Azure AD) arbeitet mit dem lokalen Azure AD zusammen, um allen Benutzern eine einheitliche Identität bereitzustellen. SSO für lokale und cloudbasierte Anwendungen macht es Benutzern einfach, sicher auf die Anwendungen und Ressourcen zuzugreifen, die sie benötigen. Administratoren können Sicherheit und Governance steuern, damit Benutzer Zugriff auf alle benötigten Elemente erhalten, und behalten gleichzeitig die Flexibilität, diese Berechtigungen ohne Auswirkungen auf die Benutzererfahrung anpassen zu können.
- Azure bietet integrierte Verwaltungs- und Sicherheitsdienste für die Cloud- und die lokale Infrastruktur. Dazu gehört auch ein integrierter Satz von Tools zum Überwachen, Konfigurieren und Schützen von Hybrid Clouds. Dieser umfassende Ansatz bei der Verwaltung hilft insbesondere bei realen Problemen, denen Organisationen gegenüber stehen, die eine Hybrid Cloud-Lösung in Betracht ziehen.
- Azure Hybrid Cloud stellt Tools bereit, die einen sicheren, nahtlosen und effizienten Zugriff auf sämtliche Daten gewährleisten. Azure-Datendienste sorgen in Kombination mit Microsoft SQL Server für eine konsistente Datenplattform. Über ein konsistentes Hybrid Cloud-Modell können Benutzer mit betrieblichen und Analysedaten arbeiten, wobei sowohl lokal als auch in der Cloud dieselben Dienste für Datawarehousing, Datenanalyse und Datenvisualisierung bereitstehen.
- Microsoft Azure-Clouddienste und ein lokaler Microsoft Azure Stack ermöglichen einheitliche Vorgehensweisen bei Entwicklung und DevOps. Konsistenz in der Cloud und lokal vor Ort bedeutet, dass Ihr DevOps-Team Anwendungen erstellen kann, die in beiden Umgebungen ausgeführt und problemlos am richtigen Standort bereitgestellt werden können. Sie können Vorlagen auch in der Hybridlösung wiederverwenden und so DevOps-Prozesse noch weiter vereinfachen.

## <a name="azure-stack-in-a-hybrid-cloud-environment"></a>Azure Stack in einer Hybrid Cloud-Umgebung

Microsoft Azure Stack ist eine Hybrid Cloud-Lösung, mit der Organisationen mit Azure konsistente Dienste in ihren Rechenzentren ausführen können. Die Lösung bietet einfach zu nutzende Funktionen für Entwicklung, Verwaltung und Sicherheit, die mit Azure-Diensten in der Public Cloud konsistent sind. Azure Stack stellt eine Erweiterung von Azure dar, die Ihnen das Ausführen von Azure-Diensten aus Ihren lokalen Umgebungen ermöglicht. Sie können dann entscheiden, ob und wann Sie den Schritt in die Azure-Cloud wagen.

Mit Azure Stack können Sie IaaS und PaaS bereitstellen und betreiben und dafür die gleichen Tools und Umgebungen nutzen wie in der öffentlichen Azure-Cloud. Für die Verwaltung von Azure Stack – über das Webportal oder mithilfe von PowerShell – steht IT-Administratoren und Endbenutzern mit Azure eine einheitliche Oberfläche zur Verfügung.

Azure und Azure Stack machen ganz neue hybride Anwendungsfälle für Anwendungen für Kunden und für interne Branchenanwendungen möglich, einschließlich:

- **Edgelösungen und nicht vernetzte Lösungen:** Kunden können den Anforderungen im Hinblick auf Wartezeit und Konnektivität gerecht werden, indem sie Daten lokal in Azure Stack verarbeiten und dann zur weiteren Analyse in Azure aggregieren. Dazu verwenden sie in beiden Systemen eine gemeinsame Anwendungslogik. Für dieses Edgeszenario interessieren sich viele Kunden aus unterschiedlichen Kontexten, einschließlich Fertigungshallen, Kreuzfahrtschiffen und Bergbauminen.
- **Cloudanwendungen, die verschiedene Bestimmungen erfüllen:** Kunden können ihre Anwendungen in Azure entwickeln und dort bereitstellen. Dank vollständiger Flexibilität können sie dieselben Anwendungen mithilfe von Azure Stack in ihrer lokalen Umgebung bereitstellen, um gesetzliche oder richtlinienbasierte Anforderungen zu erfüllen – und dabei müssen sie keine einzige Codezeile ändern. Anwendungsbeispiele hierfür sind globale Überwachung, Finanzberichte, Devisenhandel, Onlinespiele und Spesenabrechnungen. Manchmal möchten Kunden je nach geschäftlichen oder technischen Anforderungen verschiedene Instanzen derselben Anwendung in Azure oder Azure Stack bereitstellen. Während Azure die meisten Anforderungen erfüllt, ergänzt Azure Stack das Bereitstellungskonzept, falls erforderlich.
- **Lokales Cloudanwendungsmodell:** Kunden können Azure-Webdienste, Container und serverlose und Microservicearchitekturen nutzen, um vorhandene Anwendungen zu aktualisieren oder zu erweitern oder um neue Anwendungen zu erstellen. Nutzen Sie konsistente DevOps-Prozesse in Azure (in der Cloud) sowie in Azure Stack (lokal). Die Modernisierung von Anwendungen wird immer interessanter – und dies schließt auch unternehmenskritische Anwendungen ein.

Azure Stack wird in zwei Bereitstellungsoptionen angeboten:

- **Integrierte Azure Stack-Systeme:** Integrierte Azure Stack-Systeme werden im Rahmen einer Partnerschaft zwischen Microsoft und Hardwarepartnern angeboten. Dadurch entsteht eine ausgewogene Lösung mit cloudbasierten Innovationen und komfortabler Verwaltung. Bei Azure Stack handelt es sich um ein integriertes System aus Hardware und Software. Somit bietet die Plattform genau das richtige Maß an Flexibilität und Kontrolle bei gleichzeitiger Bereitstellung von cloudbasierten Innovationen. Integrierte Azure Stack-Systeme haben eine Größe von 4 bis 12 Knoten, und der Support wird vom Hardwarepartner und von Microsoft gemeinsam bereitgestellt. Mit integrierten Azure Stack-Systemen ermöglichen Sie neue Szenarien für Ihre Produktionsworkloads.
- **Azure Stack Development Kit:** Microsoft Azure Stack Development Kit ist eine Einzelknotenbereitstellung von Azure Stack, mit der Sie Azure Stack auswerten und näher kennenlernen können. Sie können das Kit auch für Entwicklungsarbeiten mit APIs und Tools verwenden, die mit Azure konsistent sind. Azure Stack Development Kit ist nicht für die Verwendung als Produktionsumgebung konzipiert.

## <a name="azure-stack-one-cloud-ecosystem"></a>Azure Stack-Ökosystem mit einer Cloud

Sie können Azure Stack-Initiativen beschleunigen, indem Sie das vollständige Azure-Ökosystem nutzen:

- Azure stellt sicher, dass die meisten Anwendungen und Dienste, die für Azure zertifiziert sind, auch in Azure Stack funktionieren. Mehrere unabhängige Softwarehersteller (ISVs) – einschließlich Bitnami, Docker, Kemp Technologies, Pivotal Cloud Foundry, Red Hat Enterprise Linux und SUSE Linux – erweitern ihre Lösungen auf Azure Stack.
- Sie können Azure Stack wahlweise als einen vollständig verwalteten Dienst bereitstellen und betreiben. Mehrere Partner – z.B. Tieto, Yourhosting, Revera, Pulsant und NTT – werden in Kürze Angebote für verwaltete Dienste in Azure und Azure Stack anbieten. Diese Partner haben bereits über das Cloud Solution Provider-Programm (Cloudanbieter) verwaltete Dienste für Azure angeboten und erweitern ihre Angebote jetzt auf Hybridlösungen.
- Avanade stellt ein Beispiel für eine umfassende, vollständig verwaltete Hybrid Cloud-Lösung dar. Es handelt sich um ein All-in-One-Angebot, das Dienste für Cloudtransformation, Software, Infrastruktur, Einrichtung und Konfiguration ebenso enthält wie fortlaufend verwaltete Dienste, sodass Kunden Azure Stack heute schon genauso wie Azure nutzen können.
- Systemintegratoren (SI) können dazu beitragen, Modernisierungsinitiativen für Anwendungen zu beschleunigen, indem sie für die Kunden End-to-End-Lösungen mit Azure erstellen. Sie verfügen über umfassende Kenntnisse mit Azure – theoretisch wie praktisch – und kennen sich mit den Abläufen aus (z.B. DevOps). Jede Azure Stack-Cloud bietet einem SI die Möglichkeit, die Lösung zu entwerfen, die Systembereitstellung anzuleiten und zu beeinflussen, die enthaltenen Funktionen anzupassen und Betriebsaktivitäten umzusetzen. Dies schließt SIs wie Avanade, DXC, Dell EMC Services, InFront Consulting Group, HPE Pointnext und Pricewaterhouse Coopers (PwC) ein.
