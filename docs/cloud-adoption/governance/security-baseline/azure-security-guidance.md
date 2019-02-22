---
title: 'CAF: Azure-Sicherheitsleitfaden'
titleSuffix: Microsoft Cloud Adoption Framework for Azure
ms.service: architecture-center
ms.subservice: enterprise-cloud-adoption
ms.custom: governance
ms.date: 02/11/2019
description: Welchen Sicherheitsleitfaden bietet Microsoft?
author: BrianBlanchard
ms.openlocfilehash: 845b02422e2df63425e29acdfd9b43b744934c9f
ms.sourcegitcommit: 273e690c0cfabbc3822089c7d8bc743ef41d2b6e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/08/2019
ms.locfileid: "55900748"
---
<!-- markdownlint-disable MD026 -->

# <a name="what-security-guidance-does-microsoft-provide"></a>Welchen Sicherheitsleitfaden bietet Microsoft?

## <a name="security-guidance-and-tools"></a>Sicherheitsleitfaden und -tools

Microsoft hat die [Service Trust Platform](https://servicetrust.microsoft.com) und den Compliance-Manager zur Unterstützung in folgenden Bereichen eingeführt:

- Bewältigung der Anforderungen der Konformitätsverwaltung
- Erfüllung der Verantwortlichkeiten zur Einhaltung gesetzlicher Anforderungen
- Durchführen von Self-Service-Überprüfungen und Self-Service-Risikobewertungen der Nutzung des Enterprise Cloud-Diensts

Diese Tools wurden entwickelt, um Organisationen bei der Erfüllung komplexer Konformitätsverpflichtungen und der Verbesserung der Funktionen zum Datenschutz bei der Auswahl und Nutzung von Microsoft Cloud-Diensten zu unterstützen.

Die **Service Trust Platform (STP)** enthält ausführliche Informationen und Tools, die Ihnen helfen, Ihre Anforderungen in Bezug auf die Nutzung von Microsoft Cloud-Diensten (einschließlich Azure, Office 365, Dynamics 365 und Windows) zu erfüllen. STP ist eine zentrale Anlaufstelle für Informationen zu Sicherheit, gesetzlichen Bestimmungen, Konformität und Datenschutz in Bezug auf Microsoft Cloud. Hier veröffentlichen wir die Informationen und Ressourcen, die für die Durchführung von Self-Service-Risikobewertungen von Clouddiensten und Tools erforderlich sind. STP wurde entwickelt, damit Aktivitäten zur Einhaltung gesetzlicher Bestimmungen in Azure nachverfolgt werden können, beispielsweise:

- **Compliance-Manager:** Compliance-Manager ist ein workflowbasiertes Tool zur Risikobewertung in der Microsoft Service Trust Platform, über das Sie die Aktivitäten Ihrer Organisation zur Einhaltung gesetzlicher Bestimmungen in Bezug auf Microsoft Cloud-Dienste, z.B. Office 365, Dynamics 365 und Azure, nachverfolgen, zuweisen und überprüfen können. Weitere Informationen finden Sie im nächsten Abschnitt.
- **Vertrauensstellungsdokumente:** Derzeit stehen drei Kategorien von Leitfäden zur Verfügung: umfangreiche Ressourcen zur Bewertung von Microsoft Cloud; Informationen zu Microsoft-Vorgängen in Bezug auf Sicherheit, Konformität und Datenschutz sowie Hinweise zur Verbesserung der Funktionen zum Datenschutz. Das umfasst:
- **Prüfberichte:** Mit Prüfberichten bleiben Sie im Hinblick auf die aktuellen Informationen zu Datenschutz, Sicherheit und Konformität für Microsoft Cloud-Dienste immer auf dem Laufenden. Dazu gehören ISO, SOC, FedRAMP und andere Prüfberichte, Bridge Letters und Material in Bezug auf unabhängige Überprüfungen von Microsoft Cloud-Diensten durch Drittanbieter, z.B. Azure, Office 365 oder Dynamics 365.
- **Leitfäden zum Datenschutz:** Leitfäden zum Datenschutz enthalten Informationen dazu, wie Ihre Daten in Microsoft Cloud-Diensten geschützt werden und wie Sie die Datensicherheit und Datenkonformität in der Cloud für Ihre Organisation verwalten können. Dazu gehören ausführliche Whitepapers mit Angaben dazu, wie Microsoft Clouddienste entwirft und betreibt, sowie häufig gestellte Fragen, Sicherheitsjahresbewertungen, Ergebnisse von Penetrationtests und Anweisungen zur Durchführung der Risikobewertung und Verbesserung der Funktionen zum Datenschutz.
- **Azure-Blaupause für Sicherheit und Compliance:** Blaupausen umfassen Ressourcen zum Erstellen und Starten von in der Cloud bereitgestellten Anwendungen, mit denen Sie strikte Bestimmungen und Standards einhalten können. Da Azure über mehr Zertifizierungen verfügt als jeder andere Cloudanbieter, können Sie Ihre unternehmenskritischen Workloads vertrauensvoll in Azure bereitstellen, mit Blaupausen, die Folgendes enthalten:

- Branchenspezifische Übersicht und Anleitung
- Kundenzuständigkeitsmatrix
- Referenzarchitekturen mit Gefahrenmodellen
- Regulierungsimplementierungsmatrizen
- Automatisierung zur Bereitstellung von Referenzarchitekturen
- Ressourcen zum Datenschutz: Ihnen werden Dokumente zu Datenschutz-Folgenabschätzungen, Anträgen betroffener Personen und Meldungen von Verletzungen des Schutzes personenbezogener Daten zur Verfügung gestellt, die Sie in ein eigenes Verantwortlichkeitsprogramm zur Unterstützung der Datenschutz-Grundverordnung (DSGVO) integrieren können.

- **Einstieg in die DSGVO:** In den Produkten und Diensten von Microsoft können Organisationen die Anforderungen der DSGVO erfüllen und personenbezogene Daten erfassen und verarbeiten. STP ist so konzipiert, dass Sie Informationen zu den Funktionen in Microsoft-Diensten erhalten, über die Sie spezifische Anforderungen der DSGVO erfüllen können. Die Dokumentation enthält grundlegende Informationen zu Ihrer Verantwortlichkeit in Bezug auf die DSGVO sowie zu technischen und organisatorischen Maßnahmen. Ihnen werden Dokumente zu Datenschutz-Folgenabschätzungen, Anträgen betroffener Personen und Meldungen von Verletzungen des Schutzes personenbezogener Daten zur Verfügung gestellt, die Sie in ein eigenes Verantwortlichkeitsprogramm zur Unterstützung der DSGVO integrieren können.
  - **Anträge betroffener Personen:** Gemäß der DSGVO haben natürliche Personen (oder betroffene Personen) bestimmte Rechte im Hinblick auf die Verarbeitung ihrer personenbezogenen Daten. Dazu gehören das Recht der betroffenen Personen auf Berichtigung unrichtiger personenbezogener Daten, auf Löschung personenbezogener Daten oder Einschränkung ihrer Verarbeitung sowie das Recht, die personenbezogenen Daten zu erhalten und an einen anderen Verantwortlichen zu übermitteln.
  - **Verletzung des Schutzes personenbezogener Daten:** Die DSGVO schreibt Verantwortlichen und Auftragsverarbeitern die Meldung von Verletzungen des Schutzes personenbezogener Daten vor. In STP erhalten Sie Informationen darüber, wie Microsoft versucht, Verletzungen von vornherein zu verhindern, wie Microsoft eine Verletzung erkennt und wie Microsoft im Fall einer Verletzung reagiert und Sie als Verantwortlichen benachrichtigt.
  - **Datenschutz-Folgenabschätzung:** Microsoft unterstützt Verantwortliche bei der Durchführung von Datenschutz-Folgenabschätzungen gemäß der DSGVO. Die DSGVO enthält eine nicht vollständige Aufstellung von Fällen, in denen eine Datenschutz-Folgenabschätzung erforderlich ist, z.B. automatisierte Verarbeitung zum Zweck der Profilerstellung o.Ä., umfangreiche Verarbeitung besonderer Kategorien von personenbezogenen Daten und systematische umfangreiche Überwachung öffentlich zugänglicher Bereiche.
  - **Weitere Ressourcen:** Zusätzlich zu den in den Abschnitten oben genannten Tools und Leitfäden umfasst STP auch weitere Ressourcen, z.B. zur regionalen Compliance, zusätzliche Ressourcen für das Security and Compliance Center sowie häufig gestellte Fragen zur Service Trust Platform, dem Compliance-Manager und zum Datenschutz und der Datenschutz-Grundverordnung.
- **Regionale Compliance:** STP umfasst verschiedene Compliancedokumente und Leitfäden für Microsoft-Onlinedienste zur Einhaltung von Compliancevorgaben für verschiedene Regionen und Länder, z.B. Tschechische Republik, Polen und Rumänien.

## <a name="unique-intelligent-insights"></a>Spezifische Intelligent Insights

Mit zunehmendem Maße des Volumens und der Komplexität von Sicherheitssignalen dauert es viel zu lange, festzustellen, ob es sich bei den Signalen um glaubwürdige Bedrohungen handelt. Microsoft bietet eine beispiellose Bandbreite der Security Intelligence auf Cloudebene, sodass Sie Bedrohungen schnell erkennen und beheben können.

## <a name="azure-threat-intelligence"></a>Azure Threat Intelligence

Mithilfe der Option „Informationen zu Bedrohungen“ in Security Center können IT-Administratoren Sicherheitsrisiken für die Umgebung identifizieren. So können sie beispielsweise ermitteln, ob ein bestimmter Computer Teil eines Botnets ist. Computer können zu Knoten in einem Botnet werden, wenn Angreifer illegal Schadsoftware installieren, die diesen Computer heimlich mit einem Steuerknoten verbindet. Die Informationen zu Bedrohungen ermöglichen auch die Erkennung potenzieller Bedrohungen aus Kommunikationskanälen wie dem Darknet.

Für den Aufbau dieser Informationen zu Bedrohungen werden in Security Center Daten aus mehreren Quellen von Microsoft herangezogen. Security Center nutzt diese Daten zur Erkennung potenzieller Bedrohungen für Ihre Umgebung. Der Bereich „Informationen zu Bedrohungen“ umfasst drei wichtige Optionen:

- Arten der erkannten Bedrohungen
- Ursprung der Bedrohung
- Threat Intelligence-Karte

## <a name="machine-learning-in-azure-security-center"></a>Maschinelles Lernen in Azure Security Center

Azure Security Center analysiert eine Fülle von Daten aus verschiedensten Microsoft- und Partnerlösungen, um Ihnen zu helfen, eine höhere Sicherheit zu erreichen. Um diese Daten nutzen zu können, verwendet das Unternehmen Data Science und maschinelles Lernen zum Schutz vor Bedrohungen, zur Erkennung und schließlich zur Untersuchung.

Ganz allgemein können mit Azure Machine Learning zwei Ergebnisse erzielt werden:

## <a name="next-generation-detection"></a>Erkennung der nächsten Generation

Angreifer sind zunehmend automatisiert und technisch versiert. Auch sie nutzen die Möglichkeiten von Data Science. Sie entwickeln Reverse-Engineering-Schutzmaßnahmen und erstellen Systeme, die Verhaltensmutationen unterstützen. Sie tarnen ihre Aktivitäten als Störungen und lernen schnell aus Fehlern. Mit maschinellem Lernen können wir auf diese Entwicklungen reagieren.

## <a name="simplified-security-baseline"></a>Vereinfachte Sicherheitsbaseline

Effektive Sicherheitsentscheidungen sind nicht leicht zu treffen. Dies erfordert Erfahrung und Fachwissen im Hinblick auf Sicherheitsbelange. Während einige große Organisationen Spezialisten beschäftigen, ist das bei vielen Unternehmen nicht der Fall. Mit Azure Machine Learning können Kunden vom Wissen anderer Organisationen profitieren, wenn sie Sicherheitsentscheidungen treffen.

## <a name="behavioral-analytics"></a>Verhaltensanalyse

Die Verhaltensanalyse ist ein Verfahren, bei dem Daten mit einer Sammlung bekannter Muster analysiert und verglichen werden. Bei diesen Mustern handelt es sich aber nicht nur um einfache Signaturen. Sie werden anhand von komplexen Algorithmen für maschinelles Lernen bestimmt, die auf große Datasets angewandt werden. Außerdem werden sie anhand einer sorgfältigen Analyse von schädlichem Verhalten durch erfahrene Analysten bestimmt. Azure Security Center kann die Verhaltensanalyse verwenden, um kompromittierte Ressourcen basierend auf der Analyse der Protokolle von virtuellen Computern, virtuellen Netzwerkgeräten, Fabric-Protokolle, Absturzabbilder und anderen Quellen zu identifizieren.
