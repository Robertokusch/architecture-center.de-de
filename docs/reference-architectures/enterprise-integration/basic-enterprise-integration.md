---
title: Unternehmensintegration mithilfe von Azure Integration Services
description: Dieser Architekturverweis zeigt, wie Sie ein einfaches Unternehmensintegrationsmuster mithilfe von Azure Logic Apps und Azure API Management implementieren.
services: logic-apps
author: mattfarm
ms.author: mattfarm
ms.reviewer: jonfan, estfan, LADocs
ms.topic: article
ms.date: 12/03/2018
ms.openlocfilehash: c8aa3f8b736fabd1a6701778f22a7eec9bf46ee7
ms.sourcegitcommit: e7e0e0282fa93f0063da3b57128ade395a9c1ef9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/05/2018
ms.locfileid: "52919101"
---
# <a name="basic-enterprise-integration-on-azure"></a>Einfache Unternehmensintegration in Azure

In dieser Referenzarchitektur wird [Azure Integration Services][integration-services] zum Orchestrieren von Aufrufen an Back-End-Unternehmenssysteme verwendet. Die Back-End-Systeme können SaaS-Systeme (Software-as-a-Service), Azure-Dienste und vorhandene Webdienste in Ihrem Unternehmen enthalten.

Azure Integration Services ist eine Sammlung von Diensten für die Integration von Anwendungen und Daten. Diese Architektur verwendet zwei dieser Dienste: [Logic Apps][logic-apps] zum Orchestrieren von Workflows und [API Management][apim] zum Erstellen von API-Katalogen. Diese Architektur ist für einfache Integrationsszenarien konzipiert, in denen der Workflow durch synchrone Back-End-Dienstaufrufe ausgelöst wird. Eine komplexere Architektur mit [Warteschlangen und Ereignissen](./queues-events.md) baut auf dieser einfachen Architektur auf. 

![Architekturdiagramm – einfache Unternehmensintegration](./_images/simple-enterprise-integration.png)

## <a name="architecture"></a>Architecture

Die Architektur besteht aus den folgenden Komponenten:

- **Back-End-Systeme**. Auf der rechten Seite des Diagramms befinden sich die verschiedenen Back-End-Systeme, die das Unternehmen bereitgestellt hat oder nutzt. Dazu gehören beispielsweise SaaS-Systeme, andere Azure-Dienste oder Webdienste, die Rest- oder SOAP-Endpunkte verfügbar machen.

- **Azur Logic Apps**. [Logic Apps][logic-apps] ist eine serverlose Plattform zum Erstellen von Unternehmensworkflows, die Anwendungen, Daten und Dienste integrieren. In dieser Architektur werden die Logik-Apps durch HTTP-Anforderungen ausgelöst. Für eine komplexere Orchestrierung können Sie Workflows auch schachteln. Logic Apps verwendet [Connectors][logic-apps-connectors] für die Integration in häufig verwendete Dienste. Logic Apps bietet hunderte von Connectors, und Sie können benutzerdefinierte Connectors erstellen.

- **Azure API Management:** [API Management][apim] ist ein verwalteter Dienst für das Veröffentlichen von HTTP-API-Katalogen, um Wiederverwendung und Erkennbarkeit zu fördern. API Management besteht aus zwei zugehörigen Komponenten:

    - **API-Gateway**. Das API-Gateway akzeptiert HTTP-Aufrufe und leitet sie an das Back-End weiter. 

    - **Entwicklerportal**. Jede Instanz von Azure API Management bietet Zugriff auf ein [Entwicklerportal][apim-dev-portal]. Über dieses Portal haben Ihre Entwickler Zugriff auf Dokumentation und Codebeispiele für das Aufrufen der APIs. Im Entwicklerportal können Sie außerdem APIs testen.

    In dieser Architektur werden Verbund-APIs durch das [Importieren von Logik-Apps][apim-logic-app] als APIs erstellt. Sie können auch vorhandene Webdienste importieren, indem Sie [OpenAPI][apim-openapi]-Spezifikationen (Swagger) oder [SOAP-APIs][apim-soap] aus WSDL-Spezifikationen importieren. 

    Das API-Gateways unterstützt die Entkopplung von Front-End-Clients vom Back-End. Dazu kann es beispielsweise URLs umschreiben oder Anforderungen transformieren, bevor sie das Back-End erreichen. Es behandelt außerdem viele übergreifende Aspekte wie Authentifizierung, Unterstützung von Cross-Origin Resource Sharing (CORS) und Zwischenspeicherung von Antworten.

- **Azure DNS:** [Azure DNS][dns] ist ein Hostingdienst für DNS-Domänen. Azure DNS bietet eine Namensauflösung mithilfe der Microsoft Azure-Infrastruktur. Durch das Hosten Ihrer Domänen in Azure können Sie Ihre DNS-Einträge mithilfe der gleichen Anmeldeinformationen, APIs, Tools und Abrechnung wie für die anderen Azure-Dienste verwalten. Erstellen Sie zur Verwendung eines benutzerdefinierten Domänennamens (etwa contoso.com) DNS-Einträge, die den benutzerdefinierten Domänennamen der IP-Adresse zuordnen. Weitere Informationen finden Sie unter [Konfigurieren eines benutzerdefinierten Domänennamens in API Management][apim-domain].

- **Azure Active Directory (Azure AD):** Verwenden Sie [Azure AD][aad] zum Authentifizieren von Clients, die das Gateway-API-aufrufen. Azure AD unterstützt das OpenID Connect-Protokoll (OIDC). Clients rufen ein Zugriffstokens von Azure AD ab, und das API-Gateway [überprüft das Token][apim-jwt], um die Anforderung zu autorisieren. Bei Verwendung des Standard- oder Premium-Tarifs von API Management kann Azure AD auch Zugriff auf das Entwicklerportal sichern.

## <a name="recommendations"></a>Empfehlungen

Ihre individuellen Anforderungen können von der hier gezeigten generischen Architektur abweichen. Verwenden Sie die Empfehlungen in diesem Abschnitt als Ausgangspunkt.

### <a name="api-management"></a>API Management

Verwenden Sie die Tarife „Basic“, „Standard“ oder „Premium“ für API Management. Diese Tarife bieten eine Vereinbarung zum Servicelevel (SLA) für die Produktionsumgebung und unterstützen eine horizontale Skalierung innerhalb der Azure-Region. Die Durchsatzkapazität für API Management wird in *Einheiten* gemessen. Die horizontale Skalierung ist für jeden Tarif beschränkt. Der Tarif „Premium“ unterstützt zudem die horizontale Skalierung über mehrere Azure-Regionen hinweg. Wählen Sie Ihren Tarif basierend auf Ihrem Funktionsumfang und dem erforderlichen Durchsatz. Weitere Informationen finden Sie unter [API Management-Preise][apim-pricing] und [Kapazität einer Azure API Management-Instanz][apim-capacity].

Jede Azure API Management-Instanz besitzt einen Standarddomänennamen. Dabei handelt es sich um eine Unterdomäne von `azure-api.net` (Beispiel: `contoso.azure-api.net`). Erwägen Sie die Konfiguration einer [benutzerdefinierte Domäne][apim-domain] für Ihre Organisation.

### <a name="logic-apps"></a>Logic Apps 

Logic Apps eignet sich am besten für Szenarien, die keine Antworten mit geringer Wartezeit erfordern (etwa asynchrone API-Aufrufe oder API-Aufrufe mittlerer Dauer). Ist eine geringe Wartezeit erforderlich (beispielsweise bei einem Aufruf, der eine Benutzeroberfläche blockiert), muss eine andere Technologie verwendet werden. Verwenden Sie beispielsweise Azure Functions oder eine in Azure App Service bereitgestellte Web-API. Verwenden Sie API Management, um die API für Ihre API-Consumer verfügbar zu machen.

### <a name="region"></a>Region

Stellen Sie API Management und Logic Apps in der gleichen Region bereit, um die Netzwerklatenz zu minimieren. Wählen Sie grundsätzlich die Ihren Benutzern (oder Ihren Back-End-Diensten) am nächsten gelegene Region aus.

Die Ressourcengruppe weist ebenfalls eine Region auf. Diese Region gibt an, wo die Metadaten der Bereitstellung gespeichert werden und die Bereitstellungsvorlage ausgeführt wird. Platzieren Sie die Ressourcengruppe und die Ressourcen in der gleichen Region, um die Verfügbarkeit während der Bereitstellung zu verbessern.

## <a name="scalability-considerations"></a>Überlegungen zur Skalierbarkeit

Fügen Sie gegebenenfalls [Cachingrichtlinien][apim-caching] hinzu, um die Skalierbarkeit von API Management zu erhöhen. Mit Caching können Sie zudem die Last für ihre Back-End-Dienste verringern.

Sie können die Tarife „Basic“, „Standard“ und „Premium“ von Azure API Management können in einer Azure-Region horizontal hochskalieren, um mehr Kapazität zu bieten. Zum Analysieren der Nutzung für Ihren Dienst wählen Sie im Menü **Metriken** die Option **Kapazitätsmetrik** aus und nehmen dann eine entsprechende zentrale Hoch- oder Herunterskalierung vor. Es kann zwischen 15 und 45 Minuten dauern, bis der Upgrade- bzw. Skalierungsprozess abgeschlossen ist.

Empfehlungen zur Skalierung eines API Management-Diensts:

- Berücksichtigen Sie Datenverkehrsmuster bei der Skalierung. Kunden mit veränderlichen Datenverkehrsmustern benötigen mehr Kapazität.

- Eine konstante Kapazität von mehr als 66% kann darauf hinweisen, dass eine Hochskalierung notwendig ist.

- Eine konstante Kapazität von unter 20% kann darauf hinweisen, dass eine Herunterskalierung notwendig ist.

- Führen Sie immer einen Auslastungstest mit einer repräsentativen Last für Ihren API Management-Dienst durch, bevor Sie die Last in der Produktion aktivieren.

Mit dem Premium-Tarif können Sie eine API Management-Instanz über mehrere Azure-Regionen hinweg skalieren. Dadurch ist API Management für eine höhere SLA berechtigt, und Sie können Dienste in mehreren Regionen in der Nähe von Benutzern bereitstellen.


Das serverlose Modell von Logic Apps bedeutet, dass Administratoren die Skalierbarkeit der Dienste nicht planen müssen. Der Dienst wird automatisch entsprechend den Anforderungen skaliert.

## <a name="availability-considerations"></a>Überlegungen zur Verfügbarkeit

Lesen Sie die SLA für jeden Dienst:

- [SLA für API Management][apim-sla]
- [SLA für Logic Apps][logic-apps-sla]

Wenn Sie API Management in mindestens zwei Regionen mit Premium-Tarif bereitstellen, besteht Anspruch auf eine höhere SLA. Siehe [API Management-Preise][apim-pricing].

### <a name="backups"></a>Backups

[Sichern][apim-backup] Sie Ihre API Management-Konfiguration regelmäßig. Speichern Sie Ihre Sicherungsdateien an einem Ort oder in einer Azure-Region, an dem bzw. in der sich der Dienst nicht befindet. Wählen Sie basierend auf Ihrer [RTO][rto] eine Strategie für die Notfallwiederherstellung:

* Bei einer Notfallwiederherstellung stellen Sie eine neue API Management-Instanz bereit, stellen die Sicherung auf dieser neuen Instanz wieder her und leiten die DNS-Einträge um.

* Behalten Sie eine passive Instanz des API Management-Diensts in einer anderen Azure-Region bei. Stellen Sie in dieser Instanz regelmäßig Sicherungen wieder her, um sie mit dem aktiven Dienst synchron zu halten. Bei einer Notfallwiederherstellung des Diensts müssen Sie lediglich die DNS-Einträge umleiten. Dieser Ansatz verursacht zwar zusätzliche Kosten, da Sie für die passive Instanz bezahlen, die Zeit bis zur Wiederherstellung wird jedoch reduziert. 

Für Logik-Apps empfehlen wir einen codebasierten Konfigurationsansatz für die Sicherung und Wiederherstellung. Da Logik-Apps serverlos sind, können Sie sie aus Azure Resource Manager-Vorlagen schnell wiederherstellen. Speichern Sie die Vorlagen in der Quellcodeverwaltung, und integrieren Sie die Vorlagen in Ihren CI/CD-Prozess (Continuous Integration/Continuous Deployment). Stellen Sie die Vorlage bei einer Notfallwiederherstellung in einer neuen Region bereit.

Wenn Sie eine Logik-App in einer anderen Region bereitstellen, aktualisieren Sie die Konfiguration in API Management. Sie können die **Backend**-Eigenschaft der API mithilfe eines einfachen PowerShell-Skripts aktualisieren.

## <a name="manageability-considerations"></a>Überlegungen zur Verwaltbarkeit

Erstellen Sie separate Ressourcengruppen für Produktions-, Entwicklungs- und Testumgebungen. Separate Ressourcengruppen erleichtern das Verwalten von Bereitstellungen, das Löschen von Testbereitstellungen und das Zuweisen von Zugriffsrechten.

Berücksichtigen Sie beim Zuweisen von Ressourcen zu Ressourcengruppen diese Faktoren:

* **Lebenszyklus**. Platzieren Sie Ressourcen mit gleichem Lebenszyklus im Allgemeinen in der gleichen Ressourcengruppe.

* **Zugriff**. Sie können die [rollenbasierte Zugriffssteuerung][rbac] (RBAC) verwenden, um Zugriffsrichtlinien auf die Ressourcen in einer Gruppe anzuwenden.

* **Abrechnung**. Sie können die anfallenden Kosten für die Ressourcengruppe anzeigen.

* **Tarif für API Management**. Verwenden Sie für Entwicklungs- und Testumgebungen den Developer-Tarif. Zur Kostenminimierung während der Präproduktion stellen Sie ein Replikat Ihrer Produktionsumgebung bereit, führen die Tests aus und fahren das Replikat dann herunter.

### <a name="deployment"></a>Bereitstellung

Verwenden Sie [Azure Resource Manager-Vorlagen][arm] für die Bereitstellung der Ressourcen. Mithilfe von Vorlagen können Bereitstellungen über PowerShell oder die Azure CLI einfacher automatisiert werden.

Integrieren Sie Azure API Management und die einzelnen Logik-Apps in eigene separate Resource Manager-Vorlagen. Durch die Verwendung unterschiedlicher Vorlagen können Sie die Ressourcen in Quellcodeverwaltungs-Systemen speichern. Sie können die Vorlagen gemeinsam oder einzeln im Rahmen eines CI/CD-Prozesses bereitstellen.

### <a name="versions"></a>Versionen

Bei jeder Konfigurationsänderung an einer Logik-App und jeder Bereitstellung eines Updates über eine Resource Manager-Vorlage bewahrt Azure eine Kopie dieser Version auf und speichert alle Versionen, die einen Ausführungsverlauf aufweisen. Sie können diese Versionen verwenden, um Änderungen im Verlauf zu verfolgen, oder eine Version als aktuelle Konfiguration der Logik-App höherzustufen. Beispielsweise können Sie für eine Logik-App ein Rollback auf eine vorherige Version ausführen.

API Management unterstützt zwei unterschiedliche, sich jedoch ergänzende Versionierungskonzepte:

* *Versionen* bieten API-Consumern die Möglichkeit, eine API-Version basierend auf ihren Anforderungen zu wählen, z.B. v1, v2, Beta oder Produktion.

* *Revisionen* ermöglichen API-Administratoren das Vornehmen geringfügiger Änderungen in einer API und das Bereitstellen dieser Änderungen zusammen mit einem Änderungsprotokoll, um API-Benutzer über die Änderungen zu informieren.

Sie können eine Revision in einer Entwicklungsumgebung erstellen und diese Änderung mithilfe von Resource Manager-Vorlagen in anderen Umgebungen bereitstellen. Weitere Informationen finden Sie unter [Veröffentlichen mehrerer Versionen Ihrer API][apim-versions].

Sie können Revisionen auch zum Testen einer API verwenden, bevor Sie die Änderungen übernehmen und für Benutzer zugänglich machen. Diese Methode wird jedoch für Auslastungstests oder Integrationstests nicht empfohlen. Verwenden Sie stattdessen separate Test- oder Präproduktionsumgebungen.

## <a name="diagnostics-and-monitoring"></a>Diagnose und Überwachung

Verwenden Sie [Azure Monitor][monitor] sowohl in API Management als auch Logic Apps für die betriebliche Überwachung. Azure Monitor liefert Informationen basierend auf den für die einzelnen Dienste konfigurierten Metriken und ist standardmäßig aktiviert. Weitere Informationen finden Sie unter

- [Überwachen von veröffentlichten APIs][apim-monitor]
- [Überwachen des Status, Einrichten der Diagnoseprotokollierung und Aktivieren von Warnungen für Azure Logic Apps][logic-apps-monitor]

Jeder Dienst verfügt außerdem über folgende Optionen:

* Zur eingehenderen Analyse und Dashboardanzeige senden Sie Logic Apps-Protokolle an [Azure Log Analytics][logic-apps-log-analytics].

* Zur Überwachung von DevOps konfigurieren Sie Azure Application Insights für API Management.

* API Management unterstützt die [Power BI-Lösungsvorlage für benutzerdefinierte API-Analysen][apim-pbi]. Sie können diese Lösungsvorlage zur Erstellung Ihrer eigenen Analyselösung verwenden. Für Geschäftskunden stellt Power BI Berichte zur Verfügung.

## <a name="security-considerations"></a>Sicherheitshinweise

Obwohl diese Liste nicht alle bewährten Sicherheitsmethoden vollständig beschreibt, finden Sie hier einige Sicherheitshinweise, die insbesondere für diese Architektur gelten:

* Der Azure API Management-Dienst weist eine feste öffentliche IP-Adresse auf. Beschränken Sie den Aufruf von Logic Apps-Endpunkten ausschließlich auf die IP-Adresse von API Management. Weitere Informationen finden Sie unter [Beschränken eingehender IP-Adressen][logic-apps-restrict-ip].

* Verwenden Sie rollenbasierte Zugriffssteuerung (RBAC), um sicherzustellen, dass Benutzer über entsprechende Zugriffsebenen verfügen.

* Sichern Sie öffentliche API-Endpunkte in API Management mit OAuth oder OpenID Connect. Um öffentliche API-Endpunkte zu sichern, konfigurieren Sie einen Identitätsanbieter und fügen eine JSON Web Token (JWT)-Validierungsrichtlinie hinzu. Weitere Informationen finden Sie unter [Schützen einer API über OAuth 2.0 mit Azure Active Directory und API Management][apim-oauth].

* Stellen Sie eine Verbindung zu Back-End-Diensten aus API Management her, indem Sie gemeinsame Zertifikate verwenden.

* Erzwingen Sie HTTPS für die API Management-APIs.

### <a name="storing-secrets"></a>Speichern von Geheimnissen

Checken Sie niemals Kennwörter, Zugriffsschlüssel oder Verbindungszeichenfolgen in die Quellcodeverwaltung ein. Wenn diese Werte benötigt werden, verwenden Sie die entsprechenden Verfahren, um sie bereitzustellen und zu sichern. 

Wenn eine Logik-App vertrauliche Werte erfordert, die Sie nicht in einem Connector erstellen können, speichern Sie diese Werte in Azure Key Vault und verweisen in einer Resource Manager-Vorlage darauf. Verwenden Sie Vorlageparameter für die Bereitstellung und Parameterdateien für jede Umgebung. Weitere Informationen finden Sie unter [Sichern von Parametern und Eingaben innerhalb eines Workflows][logic-apps-secure].

API Management verwaltet Geheimnisse über Objekte, die als *benannte Werte* oder *Eigenschaften* bezeichnet werden. In diesen Objekten werden Werte sicher gespeichert, auf die Sie über API Management-Richtlinien zugreifen können. Weitere Informationen finden Sie unter [Verwenden von benannten Werten in Azure API Management-Richtlinien][apim-properties].

## <a name="cost-considerations"></a>Kostenbetrachtung

Es entstehen Kosten für alle API Management-Instanzen, sobald sie ausgeführt werden. Wenn Sie zentral hochskaliert haben, das entsprechende Leistungsniveau jedoch nicht durchgehend benötigen, können Sie manuell zentral herunterskalieren oder die [automatische Skalierung][apim-autoscale] konfigurieren.

Logic Apps verwendet ein [serverloses](/azure/logic-apps/logic-apps-serverless-overview) Modell. Die Abrechnung erfolgt basierend auf Aktionen und Connectorausführung. Weitere Informationen hierzu finden Sie unter [Logic Apps – Preise](https://azure.microsoft.com/pricing/details/logic-apps/). Für Logic Apps gibt es derzeit keine Überlegungen zu den Tarifen.

## <a name="next-steps"></a>Nächste Schritte

Wenn Sie die Back-End-Systeme entkoppeln und eine höhere Zuverlässigkeit sowie eine bessere Skalierbarkeit erreichen möchten, verwenden Sie Nachrichtenwarteschlangen und Ereignisse. Dieses Muster wird in der nächsten Referenzarchitektur dieser Reihe behandelt: [Unternehmensintegration mithilfe von Nachrichtenwarteschlangen und Ereignissen](./queues-events.md).

<!-- links -->

[aad]: /azure/active-directory
[apim]: /azure/api-management
[apim-autoscale]: /azure/api-management/api-management-howto-autoscale
[apim-backup]: /azure/api-management/api-management-howto-disaster-recovery-backup-restore
[apim-caching]: /azure/api-management/api-management-howto-cache
[apim-capacity]: /azure/api-management/api-management-capacity
[apim-dev-portal]: /azure/api-management/api-management-key-concepts#a-namedeveloper-portal-a-developer-portal
[apim-domain]: /azure/api-management/configure-custom-domain
[apim-jwt]: /azure/api-management/policies/authorize-request-based-on-jwt-claims
[apim-logic-app]: /azure/api-management/import-logic-app-as-api
[apim-monitor]: /azure/api-management/api-management-howto-use-azure-monitor
[apim-oauth]: /azure/api-management/api-management-howto-protect-backend-with-aad
[apim-openapi]: /azure/api-management/import-api-from-oas
[apim-pbi]: http://aka.ms/apimpbi
[apim-pricing]: https://azure.microsoft.com/pricing/details/api-management/
[apim-properties]: /azure/api-management/api-management-howto-properties
[apim-sla]: https://azure.microsoft.com/support/legal/sla/api-management/
[apim-soap]: /azure/api-management/import-soap-api
[apim-versions]: /azure/api-management/api-management-get-started-publish-versions
[arm]: /azure/azure-resource-manager/resource-group-authoring-templates
[dns]: /azure/dns/
[integration-services]: https://azure.microsoft.com/product-categories/integration/
[logic-apps]: /azure/logic-apps/logic-apps-overview
[logic-apps-connectors]: /azure/connectors/apis-list
[logic-apps-log-analytics]: /azure/logic-apps/logic-apps-monitor-your-logic-apps-oms
[logic-apps-monitor]: /azure/logic-apps/logic-apps-monitor-your-logic-apps
[logic-apps-restrict-ip]: /azure/logic-apps/logic-apps-securing-a-logic-app#restrict-incoming-ip-addresses
[logic-apps-secure]: /azure/logic-apps/logic-apps-securing-a-logic-app#secure-parameters-and-inputs-within-a-workflow
[logic-apps-sla]: https://azure.microsoft.com/support/legal/sla/logic-apps
[monitor]: /azure/azure-monitor/overview
[rbac]: /azure/role-based-access-control/overview
[rto]: ../../resiliency/index.md#rto-and-rpo