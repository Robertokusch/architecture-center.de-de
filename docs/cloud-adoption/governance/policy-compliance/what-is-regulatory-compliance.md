---
title: 'CAF: Einführung in die Einhaltung gesetzlicher Bestimmungen'
titleSuffix: Microsoft Cloud Adoption Framework for Azure
ms.service: architecture-center
ms.subservice: enterprise-cloud-adoption
ms.custom: governance
ms.date: 02/11/2019
description: Einführung in die Einhaltung gesetzlicher Bestimmungen
author: BrianBlanchard
ms.openlocfilehash: 3703367dd03b63e5ecf86408ab29dafc7a6e494d
ms.sourcegitcommit: c053e6edb429299a0ad9b327888d596c48859d4a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/20/2019
ms.locfileid: "58242421"
---
# <a name="introduction-to-regulatory-compliance"></a>Einführung in die Einhaltung gesetzlicher Bestimmungen

Dies ist ein einführender Artikel zur Einhaltung gesetzlicher Bestimmungen, der nicht für die Implementierung einer Compliancestrategie vorgesehen ist. Er dient lediglich zur allgemeinen Information. Ausführlichere Informationen zu [Azure-Complianceangeboten](https://aka.ms/allcompliance) sind im [Microsoft Trust Center] verfügbar. Alle herunterladbaren Dokumente sind darüber hinaus für Azure-Kunden mit einem Geheimhaltungsvertrag im [Microsoft Service Trust Portal](https://servicetrust.microsoft.com/) verfügbar.

Die Einhaltung gesetzlicher Bestimmungen bezieht sich auf die Disziplin und den Prozess des Sicherstellens, dass ein Unternehmen die von Regierungsbehörden in der eigenen Region oder Branche erlassenen Gesetze befolgt. Im Rahmen der Einhaltung gesetzlicher Bestimmungen in der IT überwachen Personen oder Prozesse Unternehmenssysteme, um Verletzungen der durch geltende Gesetze und Bestimmungen festgelegten Richtlinien und Verfahren zu erkennen und zu verhindern. Dies betrifft wiederum einen sehr großen Bereich von Überwachungs- und Erzwingungsprozessen. Je nach Branche und Region können diese Prozesse sehr langwierig und komplex werden.

Für multinationale Unternehmen (insbesondere in umfassend regulierten Branchen wie dem Gesundheits-und Finanzsektor) kann Compliance sehr anspruchsvoll sein. Es gibt viele Standards und Bestimmungen, die außerdem natürlich häufig geändert werden. Dadurch ist es schwierig für Unternehmen, mit ständig weiterentwickelten internationalen Gesetzen zur elektronischen Datenverarbeitung mitzuhalten.

Wie bei Sicherheitskontrollen sollten Organisationen sich über die Unterteilung der Zuständigkeiten bezüglich der Einhaltung gesetzlicher Bestimmungen in der Cloud im Klaren sein. Cloudanbieter bemühen sich sicherzustellen, dass ihre Plattformen und Dienste die gesetzlichen Bestimmungen einhalten. Organisationen müssen aber ebenfalls sicherstellen, dass ihre und die von Drittanbietern bereitgestellten Anwendungen konform sind. Ähnlich ist für Anwendungen in regulierten Branchen, die Clouddienste nutzen, eventuell eine Zertifizierung des Cloudanbieters erforderlich.

Im Folgenden finden Sie Beschreibungen von behördlichen Vorschriften in verschiedenen Branchen und Regionen:

## <a name="hipaa"></a>HIPAA

Eine Anwendung im US-Gesundheitswesen, die geschützte gesundheitliche Informationen (Protected Health Information, PHI) verarbeitet, unterliegt sowohl den Regeln zur Privatsphäre als auch denen zur Sicherheit im Health Information Portability and Accountability Act (HIPAA). Als Minimum ist für HIPAA wahrscheinlich erforderlich, dass ein Unternehmen im Gesundheitswesen schriftliche Zusicherungen vom Cloudanbieter erhält, dass alle empfangenen oder erstellten Patientendaten sicher verwahrt werden.

## <a name="pci"></a>PCI

Der Payment Card Industry Data Security Standard (PCI DSS) ist ein proprietärer Informationssicherheitsstandard für Organisationen, die mit Markenkreditkarten großer Kartenschemas wie Visa, Mastercard, American Express, Discover und JCB umgehen. Der PCI-Standard wird von den Kartenmarken beauftragt und vom Payment Card Industry Security Standards Council verwaltet. Der Standard wurde erstellt, um die Kontrolle über die Daten von Karteninhabern zu verbessern und Kreditkartenbetrug zu verringern. Eine Überprüfung der Konformität wird jährlich durchgeführt – für Organisationen, die große Transaktionsvolumen verarbeiten, durch einen externen qualifizierten Sicherheitsbewerter oder einen firmenspezifischen internen Sicherheitsbewerter, der einen Konformitätsbericht erstellt, oder für Unternehmen durch einen Selbstbewertungsfragebogen.

## <a name="pii"></a>PII

Personenbezogene Informationen (Personally Identifiable Information, PII) sind alle Datenpunkte, mit denen ein Kunde, Mitarbeiter, Partner oder eine beliebige natürliche oder juristische Person identifiziert werden kann. Viele neue Gesetze, besonders zu Datenschutz und PII, erfordern, dass die Unternehmen gesetzliche Bestimmungen eigenständig einhalten und Berichte zur Einhaltung und zu eventuell aufgetretenen Verletzungen erstellen.

## <a name="gdpr"></a>GDPR

Eine der wichtigsten Entwicklungen in diesem Bereich ist das kürzliche Inkrafttreten der allgemeinen Datenschutzgrundverordnung (DSGVO) der Europäischen Kommission, die speziell entwickelt wurde, um den Schutz von Daten für Einzelpersonen innerhalb der Europäischen Union zu stärken. Die DSGVO erfordert, dass Daten über Einzelpersonen, (wie Namen, Privatadressen, Fotos, E-Mail-Adressen, Bankdaten, Beiträge in sozialen Netzwerken, medizinische Informationen oder IP-Adressen eines Computers) auf Servern innerhalb der EU verwaltet und nicht an einen Ort außerhalb der EU übertragen werden. Sie erfordert auch, dass Unternehmen Personen über Datenschutzverletzungen benachrichtigen, und fordert, dass Unternehmen einen Datenschutzbeauftragten haben müssen. Andere Länder haben ähnliche Arten von Vorschriften oder entwickeln diese derzeit.

## <a name="compliant-foundation-in-azure"></a>Konformitätsbasis in Azure

Um Kunden bei der Erfüllung ihrer eigenen Konformitätsverpflichtungen in regulierten Branchen und auf Märkten weltweit zu unterstützen, verfügt Azure über das größte Complianceportfolio der Branche – in der Breite (Gesamtanzahl von Angeboten), aber auch der Tiefe (Anzahl von kundenorientierten Diensten im Rahmen des Bewertungsbereichs). Azure-Complianceangebote sind in vier Segmente unterteilt: globale Relevanz, US Government, branchenspezifisch und regions-/landesspezifisch.

Azure-Complianceangebote basieren auf unterschiedlichen Arten von Zusicherungen, z.B. formalen Zertifizierungen, Nachweisen, Validierungen, Autorisierungen und Bewertungen, die von unabhängigen externen Prüfungsgesellschaften erstellt wurden, sowie Vertragsänderungen, Selbstbewertungen und Kundenleitfäden, die von Microsoft erstellt wurden. Jede Angebotsbeschreibung in diesem Dokument enthält eine aktuelle Umfangserklärung, die angibt, welche Azure-Kundendienste für die Bewertung in Frage kommen, sowie Links zu herunterladbaren Ressourcen, um Kunden bei ihren eigenen Konformitätsverpflichtungen zu unterstützen.

Ausführlichere Informationen zu [Azure-Complianceangeboten](/trustcenter/compliance/complianceofferings) sind im [Microsoft Trust Center] verfügbar. Alle herunterladbaren Dokumente sind darüber hinaus für Azure-Kunden mit einem Geheimhaltungsvertrag im [Service Trust Portal](https://servicetrust.microsoft.com) in den folgenden Abschnitten verfügbar:

* Prüfberichte: Enthält Abschnitte zu FedRAMP, GRC-Bewertung, ISO, PCI DSS und SOC
* Datenschutzressourcen: Enthält Konformitätsrichtlinien, häufig gestellte Fragen und Whitepaper sowie Abschnitte zu Penetrationstests und Sicherheitsbewertungen
