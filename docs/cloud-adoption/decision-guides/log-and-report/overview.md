---
title: 'Framework für die Cloudeinführung (Cloud Adoption Framework, CAF): Leitfaden zur Entscheidungsfindung für Protokollierung und Berichterstellung'
titleSuffix: Microsoft Cloud Adoption Framework for Azure
ms.service: architecture-center
ms.subservice: enterprise-cloud-adoption
ms.custom: governance
ms.date: 02/11/2019
description: Erfahren Sie mehr über die wichtigen Bereiche der Protokollierung, Berichterstellung und Überwachung bei Azure-Migrationen.
author: rotycenh
ms.openlocfilehash: 36552488872622ec59e2fcf4816da4184c3d4fbf
ms.sourcegitcommit: 273e690c0cfabbc3822089c7d8bc743ef41d2b6e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/08/2019
ms.locfileid: "55901717"
---
# <a name="logging-and-reporting-decision-guide"></a>Leitfaden zur Entscheidungsfindung für Protokollierung und Berichterstellung

Alle Organisationen benötigen Mechanismen, um IT-Teams über Leistungs-, Verfügbarkeits- und Sicherheitsprobleme zu informieren, bevor diese zu ernsthaften Problemen werden. Eine erfolgreiche Überwachungsstrategie ermöglicht Ihnen, die Leistung der Komponenten zu verstehen, aus denen sich Ihre Workloads und Netzwerkinfrastruktur zusammensetzen. Im Rahmen einer Migration in die öffentliche Cloud ist die Integration der Protokollierung und Berichterstellung in alle Ihre bestehenden Überwachungssysteme von entscheidender Bedeutung. Dabei werden wichtige Ereignisse und Metriken an die entsprechenden IT-Mitarbeiter weitergegeben, um sicherzustellen, dass Ihr Unternehmen seine Ziele hinsichtlich Betriebsbereitschaft, Sicherheit und Konformität mit Vorschriften erfüllt.

![Abbildung der Protokollierungs-, Berichterstellungs- und Überwachungsoptionen mit zunehmender Komplexität entsprechend den nachstehenden weiterführenden Links](../../_images/discovery-guides/discovery-guide-logs-and-reporting.png)

Wechseln Sie zu: [Planen der Überwachungsinfrastruktur](#planning-your-monitoring-infrastructure) | [Cloudnativ](#cloud-native) | [Lokale Erweiterung](#on-premises-extension) | [Gatewayaggregation ](#gateway-aggregation) | [Hybridüberwachung (lokal)](#hybrid-monitoring-on-premises) | [Hybridüberwachung (cloudbasiert)](#hybrid-monitoring-cloud-based) | [Mehrere Clouds ](#multi-cloud) | [Weitere Informationen](#learn-more)

Der Kernpunkt bei der Bestimmung einer Cloudstrategie basiert in erster Linie auf bereits getätigten Investitionen Ihres Unternehmens in betriebliche Prozesse und zu einem gewissen Grad auf den Anforderungen, die Sie an die Unterstützung einer Strategie mit mehreren Clouds haben.

Es gibt mehrere Möglichkeiten, Aktivitäten in der Cloud zu protokollieren und zu dokumentieren. „Cloudnativ“ und „Zentrale Protokollierung“ sind zwei übliche SaaS-Optionen (Software-as-a-Service), die dem Abonnementmodell und der Anzahl der Abonnements unterliegen.

## <a name="planning-your-monitoring-infrastructure"></a>Planen der Überwachungsinfrastruktur

Bei der Planung Ihrer Bereitstellung müssen Sie berücksichtigen, wo Protokolldaten gespeichert werden und wie Sie cloudbasierte Berichts- und Überwachungsdienste in Ihre bestehenden Prozesse und Tools integrieren.

| Frage | Cloudnativ | Lokale Erweiterung | Hybridüberwachung | Gatewayaggregation |
|-----|-----|-----|-----|-----|
| Haben Sie eine vorhandene lokale Überwachungsinfrastruktur? | Nein  | Ja | Ja |  Nein  |
| Bestehen Anforderungen, die die Speicherung von Protokolldaten an externen Speicherorten verhindern? | Nein  | Ja | Nein  | Nein  |
| Muss die Cloudüberwachung in lokale Systeme integriert werden? | Nein  | Nein  | Ja | Nein  |
Müssen Sie Telemetriedaten verarbeiten oder filtern, bevor Sie sie an Ihre Überwachungssysteme senden? | Nein  | Nein  | Nein  | Ja |

### <a name="cloud-native"></a>Cloudbasiert

Wenn es in Ihrer Organisation derzeit an bewährten Protokollierungs- und Berichtssystemen fehlt oder Ihre geplante Cloudbereitstellung nicht in bestehende lokale oder andere externe Überwachungssysteme integriert werden muss, ist eine cloudnative SaaS-Lösung die einfachste Wahl.

Bei diesem Szenario werden die Protokolldaten erfasst und in derselben Cloudumgebung wie Ihr Workload gespeichert, während die Protokollierungs- und Berichterstellungstools, die Informationen verarbeiten und IT-Mitarbeitern zugänglich machen, als Teil der Cloudplattform angeboten werden.

Cloudnative Protokollierungslösungen können ad hoc abonnement- oder workloadbezogen für kleinere oder experimentelle Bereitstellungen implementiert werden und sind zentral organisiert, um Protokolldaten im gesamten Cloudumfeld zu überwachen.

**Annahmen für cloudnative Lösungen**. Für die Verwendung eines cloudnativen Protokollierungs- und Berichterstellungssystems wird Folgendes angenommen:

- Sie müssen die Protokolldaten Ihrer Cloudworkloads nicht in bestehende lokale Systeme integrieren.
- Sie verwenden Ihre cloudbasierten Berichterstellungssysteme nicht zur Überwachung lokaler Systeme.

### <a name="on-premises-extension"></a>Lokale Erweiterung

In Szenarien, in denen Sie Cloudtelemetriedaten in lokale Systeme integrieren müssen, die keine hybride Protokollierung und Berichterstellung unterstützen oder die Migration von Anwendungen und Diensten mit einem Minimum an Neuentwicklung unterstützen, müssen Sie Überwachungs-Agents auf VMs bereitstellen, die Protokolldaten direkt an Ihre lokalen Systeme senden, anstatt sie in der Cloudumgebung zu speichern.

Um diesen Ansatz zu unterstützen, müssen Ihre Cloudressourcen in der Lage sein, über eine Kombination aus [Hybridnetzwerken](../software-defined-network/hybrid.md) und [in der Cloud gehosteten Domänendiensten](../identity/overview.md#cloud-hosted-domain-services) direkt mit Ihren lokalen Systemen zu kommunizieren. Bei dieser Konfiguration fungiert das virtuelle Cloudnetzwerk als Netzwerkerweiterung der lokalen Umgebung. Daher können in der Cloud gehostete Workloads direkt mit Ihrem lokalen Protokollierungs- und Berichterstellungssystem kommunizieren.

Dieser Ansatz macht sich Ihre bereits getätigten Investitionen in Überwachungstools zunutze, wobei nur geringfügige Änderungen an allen in der Cloud bereitgestellten Anwendungen oder Diensten vorgenommen werden. Dies ist während einer Migration per Lift & Shift oft der schnellste Ansatz zur Unterstützung der Überwachung. Allerdings werden keine Protokolldaten erfasst, die von cloudbasierten PaaS- und SaaS-Ressourcen generiert werden, und es werden alle VM-bezogenen Protokolle ausgeklammert, die von der Cloudplattform selbst erstellt werden, wie z.B. der VM-Status. Daher sollte dieses Muster nur eine temporäre Lösung sein, bis eine umfassendere hybride Überwachungslösung implementiert ist.

Annahmen für eine rein lokale Lösung:

- Sie müssen Protokolldaten nur in Ihrer lokalen Umgebung vorhalten, entweder zur Unterstützung technischer Anforderungen oder aufgrund von gesetzlichen oder richtlinienspezifischen Anforderungen.
- Ihre lokalen Systeme unterstützen keine hybriden Protokollierungs- und Berichterstellungs- bzw. Gatewayaggregationslösungen.
- Ihre cloudbasierten Anwendungen können Telemetriedaten direkt an Ihre lokalen Protokollierungssysteme übermitteln oder Überwachungs-Agents, die Daten an lokale Anwendungen senden, können auf Workload-VMs bereitgestellt werden.
- Ihre Workloads sind nicht von PaaS- oder PaaS-Diensten abhängig, die eine cloudbasierte Protokollierung und Berichterstellung erfordern.

### <a name="gateway-aggregation"></a>Gatewayaggregation

Für Szenarien, in denen die Menge der cloudbasierten Telemetriedaten sehr groß ist oder Protokolldaten für bestehende lokale Überwachungssysteme geändert werden müssen, bevor sie verarbeitet werden können, kann ein Dienst zur [Gatewayaggregation](../../../patterns/gateway-aggregation.md) von Protokolldaten erforderlich sein.

Ein Gatewaydienst wird bei Ihrem Cloudanbieter bereitgestellt. Anschließend werden relevante Anwendungen und Dienste so konfiguriert, dass Telemetriedaten anstatt an ein Standardprotokollierungssystem an das Gateway übertragen werden. Das Gateway kann anschließend die Daten verarbeiten, indem es sie aggregiert, kombiniert oder anderweitig formatiert, bevor es sie dann zur Erfassung und Analyse an Ihren Überwachungsdienst übermittelt.

Außerdem kann ein Gateway verwendet werden, um Telemetriedaten zu aggregieren und vorzuverarbeiten, die für cloudnative oder Hybridsysteme bestimmt sind.

Annahmen für die Gatewayaggregation:

- Sie erwarten von Ihren cloudbasierten Anwendungen oder Diensten eine sehr große Menge an Telemetriedaten.
- Sie müssen Telemetriedaten formatieren oder anderweitig optimieren, bevor sie an Ihre Überwachungssysteme übertragen werden.
- Ihre Überwachungssysteme verfügen über APIs oder andere Mechanismen, um Protokolldaten nach der Verarbeitung durch das Gateway zu erfassen.

### <a name="hybrid-monitoring-on-premises"></a>Hybridüberwachung (lokal)

Eine Hybridüberwachungslösung kombiniert Protokolldaten sowohl aus Ihren lokalen als auch aus Cloudressourcen, um einen ganzheitlichen Überblick über den Betriebsstatus Ihres IT-Umfelds zu erhalten.

Wenn Sie bereits eine Investition in lokale Überwachungssysteme getätigt haben, deren Austausch schwierig oder kostspielig wäre, müssen Sie möglicherweise die Telemetriedaten aus Ihren Cloudworkloads in bereits vorhandene lokale Überwachungslösungen integrieren. In einem hybriden lokalen Überwachungssystem wird für die lokalen Telemetriedaten weiterhin das bestehende lokale Überwachungssystem verwendet. Cloudbasierte Telemetriedaten werden entweder direkt an das Cloudüberwachungssystem gesendet, oder die Daten werden zusammen mit Ihren Workloads in der Cloud gespeichert und dann in regelmäßigen Abständen zusammengestellt und im lokalen System erfasst.

**Annahmen für eine lokale Hybridüberwachungslösung**. Für die Verwendung eines lokalen Protokollierungs- und Berichterstellungssystems für die Hybridüberwachung wird Folgendes angenommen:

- Sie müssen vorhandene lokale Berichtssysteme verwenden, um Cloudworkloads zu überwachen.
- Sie müssen den Besitz an Protokolldaten lokal behalten.
- Ihre lokalen Verwaltungssysteme verfügen über APIs oder andere Mechanismen, um Protokolldaten aus cloudbasierten Systemen zu erfassen.

> [!TIP]
> Als Teil der iterativen Natur der Migration in die Cloud ist die Umstellung von einer ausgeprägten cloudnativen und lokalen Überwachung auf einen teilhybriden Ansatz wahrscheinlich. Achten Sie darauf, dass Änderungen an Ihrer Überwachungsarchitektur mit Ihren gesamten IT- und Betriebsprozessen in Einklang sind.

### <a name="hybrid-monitoring-cloud-based"></a>Hybridüberwachung (cloudbasiert)

Wenn Sie keinen zwingenden Bedarf an der Aufrechterhaltung eines lokalen Überwachungssystems haben oder lokale Überwachungssysteme durch eine SaaS-Lösung ersetzen möchten, können Sie lokale Protokolldaten auch in ein zentrales cloudbasiertes Überwachungssystem integrieren.

In diesem Szenario verwenden Cloudworkloads ihren standardmäßigen Cloudprotokolliermechanismus. Lokale Anwendungen und Dienste senden entweder ein Telemetrieverzeichnis an das cloudbasierte Protokollierungssystem oder aggregieren diese Daten zur Erfassung im Cloudsystem in regelmäßigen Abständen. Das cloudbasierte Überwachungssystem dient anschließend als primäres Überwachungs- und Berichterstellungssystem für Ihr gesamtes IT-Umfeld.

Annahmen für eine cloudbasierte Hybridüberwachungslösung: Für die Verwendung eines cloudbasierten Protokollierungs- und Berichterstellungssystems für die Hybridüberwachung wird Folgendes angenommen:

- Sie sind nicht auf bestehende lokale Überwachungssysteme angewiesen.
- Ihre Workloads unterliegen keinen gesetzlichen oder richtlinienspezifischen Anforderungen zur lokalen Speicherung von Protokolldaten.
- Ihre cloudbasierten Überwachungssysteme verfügen über APIs oder andere Mechanismen, um Protokolldaten aus lokalen Anwendungen und Diensten zu erfassen.

### <a name="multi-cloud"></a>Mehrere Clouds

Die Integration von Protokollierungs- und Berichterstellungsfunktionen auf einer Plattform mit mehreren Clouds kann kompliziert sein. Die für die jeweiligen Plattformen angebotenen Dienste sind oft nicht direkt vergleichbar, und auch die Protokollierungs- und Telemetriefunktionen dieser Dienste unterscheiden sich.
Die Unterstützung der Protokollierung mehrerer Clouds erfordert oft den Einsatz von Gatewaydiensten, um Protokolldaten in ein gemeinsames Format zu konvertieren, bevor Daten an eine hybride Protokollierungslösung übermittelt werden.

## <a name="learn-more"></a>Weitere Informationen

[Azure Monitor](/azure/azure-monitor/overview) ist der Azure-Standarddienst für Berichterstellung und Überwachung. Die Lösung umfasst Folgendes:

- Eine einheitliche Plattform für das Sammeln von App-Telemetriedaten, Host-Telemetriedaten (wie z.B. VMs), Containermetriken, Azure-Plattformmetriken und Ereignisprotokollen.
- Visualisierung, Abfragen, Warnungen und Analysetools. Der Dienst kann Einblicke in virtuelle Computer, Gastbetriebssysteme, virtuelle Netzwerke und workloadbezogene Anwendungsereignisse liefern.
- [REST-APIs](/azure/monitoring-and-diagnostics/monitoring-rest-api-walkthrough) für die Integration in externe Dienste und die Automatisierung von Überwachungs- und Benachrichtigungsdiensten.
- [Integration](/azure/monitoring-and-diagnostics/monitoring-partners) in zahlreiche beliebte Dienste von Drittanbietern.
