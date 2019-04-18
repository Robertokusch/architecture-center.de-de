---
title: Überwachung von Webanwendungen in Azure
description: Überwachen Sie eine in Azure App Service gehostete Webanwendung.
author: adamboeglin
ms.date: 12/12/2018
ms.topic: reference-architecture
ms.service: architecture-center
ms.subservice: reference-architecture
ms.custom: azcat
ms.openlocfilehash: 34c21f4b5356dc0acbd5c2c85124300a6ed13c99
ms.sourcegitcommit: 579c39ff4b776704ead17a006bf24cd4cdc65edd
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/17/2019
ms.locfileid: "59640497"
---
# <a name="web-application-monitoring-on-azure"></a>Überwachung von Webanwendungen in Azure

Azure-PaaS-Angebote (Platform-as-a-Service) verwalten Computeressourcen für Sie und verändern die Art und Weise, wie Sie Bereitstellungen überwachen. Azure umfasst mehrere Überwachungsdienste, die jeweils eine bestimmte Rolle haben. Zusammen bieten diese Dienste eine umfassende Lösung für das Erfassen, Übertragen und Analysieren von Telemetriedaten von Ihrer Anwendung und den von ihnen genutzten Azure-Ressourcen.

In diesem Szenario geht es um die Überwachungsdienste, die Sie nutzen können, und es wird ein Datenflussmodell für die Verwendung mit mehreren Datenquellen beschrieben. In Bezug auf die Überwachung funktionieren viele Tools und Dienste für Azure-Bereitstellungen. In diesem Szenario wählen wir gezielt problemlos verfügbare Dienste aus, da sie auf einfache Weise genutzt werden können. Weitere Überwachungsoptionen werden weiter unten in diesem Artikel beschrieben.

## <a name="relevant-use-cases"></a>Relevante Anwendungsfälle

Zu den weiteren relevanten Anwendungsfällen zählen:

- Instrumentieren einer Webanwendung für die Überwachung von Telemetriedaten
- Sammeln von Front-End- und Back-End-Telemetriedaten für eine unter Azure bereitgestellte Anwendung
- Überwachen von Metriken und Kontingenten, die Diensten unter Azure zugeordnet sind

## <a name="architecture"></a>Architecture

![„Architekturdiagramm“](./images/architecture-diagram-app-monitoring.svg)

In diesem Szenario wird eine verwaltete Azure-Umgebung verwendet, um eine Anwendung und eine Datenebene zu hosten. Die Daten durchlaufen das Szenario wie folgt:

1. Ein Benutzer interagiert mit der Anwendung.
2. Der Browser und App Service geben Telemetriedaten aus.
3. Application Insights sammelt und analysiert Integritäts-, Leistungs- und Nutzungsdaten von Anwendungen.
4. Entwickler und Administratoren können die Informationen zur Integrität, Leistung und Nutzung überprüfen.
5. Azure SQL-Datenbank gibt Telemetriedaten aus.
6. Azure Monitor sammelt und analysiert Infrastrukturmetriken und -kontingente.
7. Log Analytics erfasst und analysiert Protokolle und Metriken.
8. Entwickler und Administratoren können die Informationen zur Integrität, Leistung und Nutzung überprüfen.

### <a name="components"></a>Komponenten

- [Azure App Service](/azure/app-service/) ist ein PaaS-Dienst zum Erstellen und Hosten von Apps auf verwalteten virtuellen Computern. Die zugrunde liegenden Infrastrukturen, in denen Ihre Apps ausgeführt werden, werden für Sie verwaltet. App Service ermöglicht das Überwachen von Kontingenten zur Ressourcenverwendung und App-Metriken, Protokollieren von Diagnoseinformationen und Anzeigen von Warnungen basierend auf Metriken. Noch besser ist, dass Sie Application Insights zum Erstellen von [Verfügbarkeitstests][availability-tests] verwenden können, um Ihre Anwendung von unterschiedlichen Regionen aus zu testen.
- [Application Insights][application-insights] ist ein erweiterbarer, für Entwickler konzipierter Dienst zur Verwaltung der Anwendungsleistung (Application Performance Management, APM), der mehrere Plattformen unterstützt. Er dient zum Überwachen der Anwendung, Erkennen von Anwendungsanomalien, z.B. schlechte Leistung und Fehler, und Senden von Telemetriedaten an das Azure-Portal. Application Insights kann auch für die Protokollierung, die verteilte Ablaufverfolgung und benutzerdefinierte Anwendungsmetriken verwendet werden.
- [Azure Monitor][azure-monitor] stellt grundlegende [Metriken und Protokolle][metrics] der Infrastruktur für die meisten Dienste in Azure bereit. Sie können auf unterschiedliche Weise mit den Metriken interagieren, z.B. die Diagrammdarstellung im Azure-Portal, den Zugriff über die REST-API oder die Abfrage über PowerShell oder die CLI. Azure Monitor bietet seine Daten auch direkt für [Log Analytics und andere Dienste] an, wo Sie diese abfragen und mit Daten aus anderen lokalen Quellen oder Cloudquellen kombinieren können.
- [Log Analytics][log-analytics] dient als Hilfe beim Korrelieren der von Application Insights gesammelten Nutzungs- und Leistungsdaten mit Konfigurations- und Leistungsdaten der Azure-Ressourcen, die die App unterstützen. In diesem Szenario wird der [Azure Log Analytics-Agent][Azure Log Analytics agent] verwendet, um SQL Server-Überwachungsprotokolle per Pushvorgang an Log Analytics zu übertragen. Sie können im Azure-Portal auf dem Blatt „Log Analytics“ Abfragen schreiben und Daten anzeigen.

## <a name="considerations"></a>Überlegungen

Eine empfohlene Methode besteht darin, Ihrem Code während der Entwicklung mit den [Application Insights SDKs][Application Insights SDKs] Application Insights hinzuzufügen und die Anpassung für jede Anwendung einzeln vorzunehmen. Diese Open Source-SDKs sind für die meisten Anwendungsframeworks verfügbar. Binden Sie die Nutzung der SDKs sowohl für Test- als auch für Produktionsbereitstellungen in Ihren Entwicklungsprozess ein, um die von Ihnen gesammelten Daten zu erweitern und zu steuern. Die wichtigste Voraussetzung besteht darin, dass für die App eine direkte oder indirekte „Sichtverbindung“ mit dem Applications Insights-Erfassungsendpunkt besteht, der mit einer über das Internet zugänglichen Adresse gehostet wird. Sie können dann Telemetriedaten hinzufügen oder eine vorhandene Telemetriedatensammlung erweitern.

Die Überwachung der Laufzeit ist eine weitere einfache Einstiegsmöglichkeit. Die gesammelten Telemetriedaten müssen mit Konfigurationsdateien gesteuert werden. Beispielsweise können Sie Laufzeitmethoden einfügen, mit denen Tools, z.B. [Application Insights-Statusmonitor][Application Insights Status Monitor], die SDKs im richtigen Ordner bereitstellen und die richtigen Konfigurationen hinzufügen können, um mit der Überwachung zu beginnen.

Wie Application Insights auch, verfügt Log Analytics über Tools zum [Analysieren von Daten über mehrere Quellen hinweg][analyzing data across sources], Erstellen von komplexen Abfragen und [Senden von proaktiven Warnungen][sending proactive alerts] basierend auf den jeweils angegebenen Bedingungen. Darüber hinaus können Sie Telemetriedaten im [Azure-Portal][the Azure portal] anzeigen. Log Analytics erhöht den Nutzen von vorhandenen Überwachungsdiensten wie [Azure Monitor][azure-monitor] und kann auch zum Überwachen von lokalen Umgebungen genutzt werden.

Sowohl für Application Insights als auch für Log Analytics wird die [Azure Log Analytics-Abfragesprache][Azure Log Analytics Query Language] verwendet. Sie können auch [ressourcenübergreifende Abfragen](https://azure.microsoft.com/blog/query-across-resources) verwenden, um die Telemetriedaten, die von Application Insights und Log Analytics gesammelt werden, in einer einzelnen Abfrage zu analysieren.

Azure Monitor, Application Insights und Log Analytics senden [Warnungen](/azure/monitoring-and-diagnostics/monitoring-overview-alerts). Beispielsweise sendet Azure Monitor Warnungen für Metriken auf Plattformebene, z.B. zur CPU-Auslastung, während Application Insights Warnungen für Metriken auf Anwendungsebene sendet, z.B. zur Serverreaktionszeit. Azure Monitor sendet Warnungen für neue Ereignisse im Azure-Aktivitätsprotokoll, während Log Analytics Warnungen zu Metriken oder Ereignisdaten für die Dienste ausgeben kann, für die die Nutzung konfiguriert ist. [Einheitliche Warnungen in Azure Monitor](/azure/monitoring-and-diagnostics/monitoring-overview-unified-alerts) ist eine neue einheitliche Benutzeroberfläche für Warnungen in Azure, für die eine andere Taxonomie verwendet wird.

### <a name="alternatives"></a>Alternativen

In diesem Artikel werden problemlos verfügbare Überwachungsoptionen mit beliebten Features beschrieben, aber Sie haben eine große Auswahl, z.B. die Option zur Erstellung Ihrer eigenen Protokollierungsmechanismen. Eine empfohlene Methode ist das Hinzufügen von Überwachungsdiensten, wenn Sie in einer Lösung Ebenen erstellen. Hier sind einige mögliche Erweiterungen und Alternativen aufgeführt:

- Konsolidieren von Azure Monitor- und Application Insights-Metriken in Grafana per [Azure Monitor Data Source For Grafana][Azure Monitor Data Source For Grafana] (Azure Monitor-Datenquelle für Grafana)
- [Data Dog][data-dog] enthält einen Connector für Azure Monitor
- Automatisieren von Überwachungsfunktionen mit [Azure Automation][Azure Automation]
- Hinzufügen der Kommunikation mit [ITSM-Lösungen][ITSM solutions]
- Erweitern von Log Analytics mit einer [Verwaltungslösung][management solution]

### <a name="scalability-and-availability"></a>Skalierbarkeit und Verfügbarkeit

In diesem Szenario liegt der Schwerpunkt vor allem auf PaaS-Lösungen für die Überwachung, da Sie damit bequem für Verfügbarkeit und Skalierbarkeit sorgen können und dafür Vereinbarungen zum Servicelevel (SLAs) vorhanden sind. App Services verfügt beispielsweise über eine garantierte [SLA][SLA] für die Verfügbarkeit.

Für Application Insights gelten [Grenzwerte][app-insights-limits] für die Anzahl von Anforderungen, die pro Sekunde verarbeitet werden können. Wenn der Grenzwert für die Anforderungen überschritten wird, kommt es ggf. zu einer Drosselung der Nachrichten. Um eine Drosselung zu verhindern, können Sie eine [Filterung][message-filtering] oder [Sampling][message-sampling] zum Reduzieren der Datenrate implementieren.

Für die Aspekte der Hochverfügbarkeit für die von Ihnen ausgeführte App ist aber der Entwickler verantwortlich. Informationen zur Skalierung finden Sie beispielsweise im Abschnitt [Überlegungen zur Skalierbarkeit](./basic-web-app.md#scalability-considerations) in der Referenzarchitektur für einfache Webanwendungen. Nachdem eine App bereitgestellt wurde, können Sie Tests zum [Überwachen der Verfügbarkeit][monitor its availability] einrichten, indem Sie Application Insights verwenden.

### <a name="security"></a>Sicherheit

Die Anforderungen in Bezug auf vertrauliche Informationen und Konformität wirken sich auf die Sammlung, Aufbewahrung und Speicherung von Daten aus. Erfahren Sie mehr dazu, wie [Application Insights][application-insights] und [Log Analytics][log-analytics] Telemetriedaten verarbeiten.

Unter Umständen gelten auch die folgenden Sicherheitsaspekte:

- Entwickeln Sie einen Plan zur Verarbeitung von persönlichen Informationen, wenn es für Entwickler zulässig ist, eigene Daten zu sammeln oder vorhandene Telemetriedaten zu erweitern.
- Berücksichtigen Sie die Datenaufbewahrung. Für Application Insights werden Telemetriedaten beispielsweise 90 Tage lang aufbewahrt. Archivieren Sie Daten, auf die der Zugriff längere Zeit möglich sein soll, mit Microsoft Power BI, dem fortlaufenden Export oder der REST-API. Hierfür gelten die Speichergebühren.
- Beschränken Sie den Zugriff auf Azure-Ressourcen, um den Datenzugriff zu steuern und festzulegen, wer Telemetriedaten über eine bestimmte Anwendung anzeigen kann. Informationen zum Sperren des Zugriffs auf Telemetriedaten der Überwachung finden Sie unter [Ressourcen, Rollen und Zugriffssteuerung in Application Insights][Resources, roles, and access control in Application Insights].
- Überlegen Sie, ob der Lese-/Schreibzugriff im Anwendungscode gesteuert werden sollte. So wird verhindert, dass Benutzer Versions- oder Tagmarkierungen hinzufügen, mit denen die Datenerfassung für die Anwendung eingeschränkt wird. Bei Application Insights können einzelne Datenelemente nach dem Senden an eine Ressource nicht mehr gesteuert werden. Wenn ein Benutzer also Zugriff auf Daten hat, hat er auch Zugriff auf alle Daten einer einzelnen Ressource.
- Fügen Sie Mechanismen für [Governance](/azure/security/governance-in-azure) hinzu, um bei Bedarf Richtlinien- oder Kostenkontrollen für Azure-Ressourcen zu erzwingen. Nutzen Sie beispielsweise Log Analytics für die sicherheitsbezogene Überwachung, z. B. Richtlinien und die rollenbasierte Zugriffssteuerung, oder verwenden Sie [Azure Policy](/azure/azure-policy/azure-policy-introduction), um Richtliniendefinitionen zu erstellen, zuzuweisen und zu verwalten.
- Zum Überwachen potenzieller Sicherheitsprobleme und Erlangen eines zentralen Überblicks über den Sicherheitszustand Ihrer Azure-Ressourcen können Sie auch [Azure Security Center](/azure/security-center/security-center-intro) nutzen.

## <a name="pricing"></a>Preise

Die Gebühren für die Überwachung können sich schnell zu einem höheren Betrag anwachsen. Daher sollten Sie sich die Preise im Voraus ansehen, sich im Klaren darüber sein, was Sie überwachen, und die entsprechenden Gebühren für jeden Dienst prüfen. Azure Monitor stellt [einfache Metriken][basic metrics] kostenlos bereit, während sich die Überwachungskosten für [Application Insights][application-insights-pricing] und [Log Analytics][log-analytics] nach der Menge der erfassten Daten und der Anzahl von durchgeführten Tests richten.

Verwenden Sie als Einstiegshilfe den [Preisrechner][pricing], um die Kosten zu schätzen. Wenn Sie wissen möchten, welche Kosten für Ihren spezifischen Anwendungsfall entstehen, können Sie die unterschiedlichen Optionen ändern und an Ihre erwartete Bereitstellung anpassen.

Telemetriedaten von Application Insights werden beim Debuggen und nach dem Veröffentlichen Ihrer App an das Azure-Portal gesendet. Zu Testzwecken und zur Vermeidung von Gebühren wird eine begrenzte Menge von Telemetriedaten instrumentiert. Sie können den Grenzwert für die Telemetriedaten erhöhen, um weitere Indikatoren hinzuzufügen. Informationen zu einer präziseren Steuerung finden Sie unter [Erstellen von Stichproben in Application Insights][Sampling in Application Insights].

Nach der Bereitstellung können Sie sich einen [Live Metrics Stream][Live Metrics Stream] mit Leistungsindikatoren ansehen. Diese Daten werden nicht gespeichert (Sie zeigen Echtzeitmetriken an), die Telemetriedaten können jedoch erfasst und später analysiert werden. Für Live Stream-Daten fallen keine Gebühren an.

Die Abrechnung von Log Analytics erfolgt pro GB an Daten, die im Dienst erfasst werden. Die ersten 5 GB an Daten, die jeden Monat mit dem Azure Log Analytics-Dienst erfasst werden, sind kostenlos. Die Daten werden für die ersten 31 Tage ohne Gebühren in Ihrem Log Analytics-Arbeitsbereich aufbewahrt.

## <a name="next-steps"></a>Nächste Schritte

Nutzen Sie diese Ressourcen als Hilfe bei den ersten Schritten mit Ihrer eigenen Überwachungslösung:

[Referenzarchitektur für einfache Webanwendungen][Basic web application reference architecture]

[Starten der Überwachung Ihrer ASP.NET-Webanwendung][Start monitoring your ASP.NET Web Application]

[Sammeln von Daten über virtuelle Azure-Computer][Collect data about Azure Virtual Machines]

## <a name="related-resources"></a>Zugehörige Ressourcen

[Überwachen von Azure-Anwendungen und -Ressourcen][Monitoring Azure applications and resources]

[Suchen und Diagnostizieren von Laufzeitausnahmen mit Azure Application Insights][Find and diagnose run-time exceptions with Azure Application Insights]

<!-- links -->
[architecture]: ./images/architecture-diagram-app-monitoring.svg
[availability-tests]: /azure/application-insights/app-insights-monitor-web-app-availability
[application-insights]: /azure/application-insights/app-insights-overview
[azure-monitor]: /azure/monitoring-and-diagnostics/monitoring-overview-azure-monitor
[metrics]: /azure/monitoring-and-diagnostics/monitoring-supported-metrics
[Log Analytics und andere Dienste]: /azure/log-analytics/log-analytics-azure-storage
[log-analytics]: /azure/log-analytics/log-analytics-overview
[Azure Log Analytics agent]: https://blogs.msdn.microsoft.com/sqlsecurity/2017/12/28/azure-log-analytics-oms-agent-now-collects-sql-server-audit-logs/
[application-insights-pricing]: https://azure.microsoft.com/pricing/details/application-insights/
[Application Insights SDKs]: /azure/application-insights/app-insights-asp-net
[Application Insights Status Monitor]: https://azure.microsoft.com/updates/application-insights-status-monitor-and-sdk-updated/
[analyzing data across sources]: /azure/log-analytics/log-analytics-dashboards
[sending proactive alerts]: /azure/log-analytics/log-analytics-alerts
[the Azure portal]: /azure/log-analytics/log-analytics-tutorial-dashboards
[Azure Log Analytics Query Language]: https://docs.loganalytics.io/docs/Learn
[cross-resource queries]: https://azure.microsoft.com/blog/query-across-resources/
[alerts]: /azure/monitoring-and-diagnostics/monitoring-overview-alerts
[Alerts (Preview)]: /azure/monitoring-and-diagnostics/monitoring-overview-unified-alerts
[Azure Monitor Data Source For Grafana]: https://grafana.com/plugins/grafana-azure-monitor-datasource
[Azure Automation]: /azure/automation/automation-intro
[ITSM solutions]: https://azure.microsoft.com/blog/itsm-connector-for-azure-is-now-generally-available/
[management solution]: /azure/monitoring/monitoring-solutions
[SLA]: https://azure.microsoft.com/support/legal/sla/app-service/v1_4/
[monitor its availability]: /azure/application-insights/app-insights-monitor-web-app-availability
[Resources, roles, and access control in Application Insights]: /azure/application-insights/app-insights-resources-roles-access-control
[basic metrics]: /azure/monitoring-and-diagnostics/monitoring-supported-metrics
[pricing]: https://azure.microsoft.com/pricing/calculator/#log-analyticsc126d8c1-ec9c-4e5b-9b51-4db95d06a9b1
[Sampling in Application Insights]: /azure/application-insights/app-insights-sampling
[Live Metrics Stream]: /azure/application-insights/app-insights-live-stream
[Basic web application reference architecture]: /azure/architecture/reference-architectures/app-service-web-app/basic-web-app#scalability-considerations
[Start monitoring your ASP.NET Web Application]: /azure/application-insights/quick-monitor-portal
[Collect data about Azure Virtual Machines]: /azure/log-analytics/log-analytics-quick-collect-azurevm
[Monitoring Azure applications and resources]: /azure/monitoring-and-diagnostics/monitoring-overview
[Find and diagnose run-time exceptions with Azure Application Insights]: /azure/application-insights/app-insights-tutorial-runtime-exceptions
[data-dog]: https://www.datadoghq.com/blog/azure-monitoring-enhancements/
[app-insights-limits]: /azure/azure-subscription-service-limits#application-insights-limits
[message-filtering]: /azure/application-insights/app-insights-api-filtering-sampling
[message-sampling]: /azure/application-insights/app-insights-sampling
