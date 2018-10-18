---
title: Data Warehousing und Analysen für Vertrieb und Marketing
description: Fassen Sie Daten aus mehreren Quellen zusammen, um die Datenanalyse zu optimieren.
author: alexbuckgit
ms.date: 09/15/2018
ms.openlocfilehash: f9ca9785b65f18098a91aedc1f3157f49456a6e1
ms.sourcegitcommit: b2a4eb132857afa70201e28d662f18458865a48e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/05/2018
ms.locfileid: "48818648"
---
# <a name="data-warehousing-and-analytics-for-sales-and-marketing"></a>Data Warehousing und Analysen für Vertrieb und Marketing

In diesem Beispielszenario wird eine Datenpipeline veranschaulicht, die große Datenmengen aus mehreren Quellen in eine einheitliche Analyseplattform in Azure integriert. Dieses spezielle Szenario basiert zwar auf einer Lösung für Vertrieb und Marketing, die Entwurfsmuster sind jedoch für viele Branchen relevant, in denen erweiterte Analysen von umfangreichen Datasets benötigt werden. Hierzu zählen beispielsweise E-Commerce, Einzelhandel und Gesundheitswesen.

Das Unternehmen in diesem Beispiel ist im Bereich Vertrieb und Marketing tätig und entwickelt Anreizprogramme. Diese Programme dienen zur Belohnung von Kunden, Lieferanten, Verkäufern und Mitarbeitern. Die Programme sind auf Daten angewiesen, und das Unternehmen möchte mit Azure die per Datenanalyse gewonnenen Erkenntnisse verbessern.

Das Unternehmen benötigt einen modernen Ansatz für die Datenanalyse, um Entscheidungen zur richtigen Zeit und auf der Grundlage der richtigen Daten treffen zu können. Das Unternehmen hat folgende Ziele:
* Kombinieren verschiedene Arten von Datenquellen in einer Cloudplattform
* Transformieren von Quelldaten in eine allgemeine Taxonomie und Struktur, um die Daten konsistent zu machen und einfach vergleichen zu können
* Laden von Daten unter Verwendung eines hochgradig parallelisierten Ansatzes, der Tausende von Anreizprogrammen unterstützt, aber ohne die hohen Kosten für die Bereitstellung und Pflege einer lokalen Infrastruktur
* Deutliches Beschleunigen der Datenerfassung und -transformation, um sich auf die Analyse der Daten konzentrieren zu können

## <a name="relevant-use-cases"></a>Relevante Anwendungsfälle

Sie können diesen Ansatz auch für Folgendes verwenden:

* Einrichten eines Data Warehouse als alleingültige Quelle für Ihre Daten
* Integrieren relationaler Datenquellen in andere unstrukturierte Datasets
* Verwenden von Semantikmodellen und leistungsstarken Visualisierungstools zur Vereinfachung der Datenanalyse

## <a name="architecture"></a>Architektur

![Architektur für ein Szenario mit Data Warehouse und Datenanalyse in Azure][architecture]

Die Daten durchlaufen die Lösung wie folgt:

1. Aktualisierungen der einzelnen Datenquellen werden in regelmäßigen Abständen in einen Stagingbereich in Azure Blob Storage exportiert.
2. Data Factory lädt die Daten inkrementell aus Blob Storage in Stagingtabellen in SQL Data Warehouse. Dabei werden die Daten bereinigt und transformiert. PolyBase kann den Prozess für umfangreiche Datasets parallelisieren.
3. Nachdem ein neuer Datenbatch in das Warehouse geladen wurde, wird ein zuvor erstelltes Analysis Services-Tabellenmodell aktualisiert. Dieses Semantikmodell vereinfacht die Analyse von Geschäftsdaten und -beziehungen.
4. Business Analysts verwenden Microsoft Power BI, um Warehouse-Daten unter Verwendung des Analysis Services-Semantikmodells zu analysieren.

### <a name="components"></a>Komponenten

Das Unternehmen verfügt über Datenquellen auf vielen verschiedenen Plattformen:
* SQL Server (lokal)
* Oracle (lokal)
* Azure SQL-Datenbank
* Azure Table Storage
* Cosmos DB

Daten werden aus diesen unterschiedlichen Datenquellen unter Verwendung verschiedener Azure-Komponenten geladen:
* [Blob Storage](/azure/storage/blobs/storage-blobs-introduction) wird verwendet, um Quelldaten vor dem Laden in SQL Data Warehouse bereitzustellen.
* [Data Factory](/azure/data-factory) orchestriert die Transformation der bereitgestellten Daten in eine allgemeine Struktur in SQL Data Warehouse. Data Factory [verwendet PolyBase beim Laden von Daten in SQL Data Warehouse](/azure/data-factory/connector-azure-sql-data-warehouse#use-polybase-to-load-data-into-azure-sql-data-warehouse), um den Durchsatz zu maximieren. 
* [SQL Data Warehouse](/azure/sql-data-warehouse/sql-data-warehouse-overview-what-is) ist ein verteiltes System zum Speichern und Analysieren umfangreicher Datasets. Dank MPP (Massive Parallel Processing) eignet sich diese Komponente für Hochleistungsanalysen. In Kombination mit [PolyBase](/sql/relational-databases/polybase/polybase-guide) kann SQL Data Warehouse Daten mit hoher Geschwindigkeit aus Blob Storage laden.
* [Analysis Services](/azure/analysis-services) bietet ein Semantikmodell für Ihre Daten. Darüber hinaus kann die Komponente die Systemleistung beim Analysieren Ihrer Daten erhöhen. 
* [Power BI](/power-bi) ist eine Suite aus Business Analytics-Tools zum Analysieren von Daten und Teilen von Einblicken. Power BI kann ein in Analysis Services gespeichertes Semantikmodell oder direkt SQL Data Warehouse abfragen.
* [Azure Active Directory (Azure AD)](/azure/active-directory) authentifiziert Benutzer, die über Power BI eine Verbindung mit dem Analysis Services-Server herstellen. Azure AD kann auch von Data Factory für die Authentifizierung bei SQL Data Warehouse verwendet werden – entweder über einen Dienstprinzipal oder über eine [verwaltete Identität für Azure-Ressourcen](/azure/active-directory/managed-identities-azure-resources/overview).

### <a name="alternatives"></a>Alternativen

* Die Beispielpipeline enthält verschiedene Arten von Datenquellen. Diese Architektur eignet sich für ein breites Spektrum an relationalen und nicht relationalen Datenquellen.
* Data Factory orchestriert die Workflows für Ihre Datenpipeline. Wenn Sie Daten nur einmalig oder bei Bedarf laden möchten, können Sie beispielsweise das SQL Server-Tool zum Massenkopieren (bcp) oder AzCopy verwenden, um Daten in Blob Storage zu kopieren. Anschließend können Sie die Daten mithilfe von PolyBase direkt in SQL Data Warehouse laden.
* Wenn Sie über sehr große Datasets verfügen, empfiehlt sich unter Umständen die Verwendung von [Data Lake Storage](/azure/storage/data-lake-storage/introduction), da Ihnen hier unbegrenzter Speicher für Analysedaten zur Verfügung steht.
* Für die Verarbeitung von Big Data kann auch eine lokale Appliance vom Typ [SQL Server Parallel Data Warehouse](/sql/analytics-platform-system) verwendet werden. Eine verwaltete cloudbasierte Lösung wie SQL Data Warehouse verursacht jedoch meist deutlich geringere Betriebskosten. 
* SQL Data Warehouse ist nicht ideal für OLTP-Workloads oder Datasets mit einer Größe von weniger als 250 GB. In diesen Fällen empfiehlt sich die Verwendung von Azure SQL-Datenbank oder SQL Server.
* Vergleiche mit anderen Alternativen finden Sie hier:
    * [Auswählen einer Technologie für die Datenpipelineorchestrierung in Azure](/azure/architecture/data-guide/technology-choices/pipeline-orchestration-data-movement)
    * [Auswählen einer Batchverarbeitungstechnologie in Azure](/azure/architecture/data-guide/technology-choices/batch-processing)
    * [Auswählen eines Analysedatenspeichers in Azure](/azure/architecture/data-guide/technology-choices/analytical-data-stores)
    * [Auswählen einer Technologie für die Datenanalyse in Azure](/azure/architecture/data-guide/technology-choices/analysis-visualizations-reporting)

## <a name="considerations"></a>Überlegungen

Die Technologien in dieser Architektur wurden gewählt, da sie die Skalier- und Verfügbarkeitsanforderungen des Unternehmens erfüllen und das Unternehmen bei der Kostenkontrolle unterstützen.

* Die [MPP-Architektur (Massively Parallel Processing)](/azure/sql-data-warehouse/massively-parallel-processing-mpp-architecture) von SQL Data Warehouse zeichnet sich durch Skalierbarkeit und hohe Leistung aus.
* SQL Data Warehouse verfügt über [SLA-Garantien](https://azure.microsoft.com/support/legal/sla/sql-data-warehouse) und [empfohlene Vorgehensweisen zur Erreichung von Hochverfügbarkeit](/azure/sql-data-warehouse/sql-data-warehouse-best-practices).
* Bei geringer Analyseaktivität kann das Unternehmen [SQL Data Warehouse nach Bedarf skalieren](/azure/sql-data-warehouse/sql-data-warehouse-manage-compute-overview) und die Computeressourcen verringern oder sogar anhalten, um Kosten zu sparen.
* Azure Analysis Services kann [horizontal hochskaliert](/azure/analysis-services/analysis-services-scale-out) werden, um die Antwortzeiten bei einem hohen Aufkommen von Abfrageworkloads zu verkürzen. Darüber hinaus kann die Verarbeitung vom Abfragepool getrennt werden, sodass Clientabfragen nicht durch Verarbeitungsvorgänge verlangsamt werden. 
* Azure Analysis Services verfügt ebenfalls über [SLA-Garantien](https://azure.microsoft.com/support/legal/sla/analysis-services) und [empfohlene Vorgehensweisen zur Erreichung von Hochverfügbarkeit](/azure/analysis-services/analysis-services-bcdr).
* Das [Sicherheitsmodell von SQL Data Warehouse](/azure/sql-data-warehouse/sql-data-warehouse-overview-manage-security) bietet Verbindungssicherheit, [Authentifizierung und Autorisierung](/azure/sql-data-warehouse/sql-data-warehouse-authentication) mittels Azure AD- oder SQL Server-Authentifizierung sowie Verschlüsselung. [Azure Analysis Services](/azure/analysis-services/analysis-services-manage-users) verwendet Azure AD zur Identitätsverwaltung und Benutzerauthentifizierung. 

## <a name="pricing"></a>Preise

Sehen Sie sich über den Azure-Preisrechner ein [Preisbeispiel für ein Data Warehouse-Szenario][calculator] an. Passen Sie die Werte an, um zu ermitteln, wie sich Ihre Anforderungen auf die Kosten auswirken.

* Mit [SQL Data Warehouse](https://azure.microsoft.com/pricing/details/sql-data-warehouse/gen2) können Sie Ihre Compute- und Ihre Speicherebene unabhängig voneinander skalieren. Computeressourcen werden auf Stundenbasis abgerechnet und können nach Bedarf skaliert oder angehalten werden. Speicherressourcen werden nach Terabyte abgerechnet. Ihre Kosten steigen also, wenn Sie mehr Daten erfassen.
* Die Kosten für [Data Factory](https://azure.microsoft.com/pricing/details/data-factory) basieren auf der Anzahl von Lese-/Schreibvorgängen, Überwachungsvorgängen und Orchestrierungsaktivitäten, die in einer Workload ausgeführt werden. Die Kosten für Ihre Data Factory erhöhen sich mit jedem weiteren Datenstrom und der jeweils verarbeiteten Datenmenge.
* [Analysis Services](https://azure.microsoft.com/pricing/details/analysis-services) ist in den Tarifen „Developer“, „Basic“ und „Standard“ erhältlich. Die Preise der Instanzen basieren auf QPUs (Query Processing Units) und auf dem verfügbaren Arbeitsspeicher. Minimieren Sie die Anzahl ausgeführter Abfragen, den Umfang der durch die Abfragen verarbeiteten Daten sowie die Ausführungshäufigkeit dieser Abfragen, um die Kosten gering zu halten.
* [Power BI](https://powerbi.microsoft.com/pricing) bietet verschiedene Produktoptionen für unterschiedliche Anforderungen. [Power BI Embedded](https://azure.microsoft.com/pricing/details/power-bi-embedded) bietet eine Azure-basierte Option zum Einbetten von Power BI-Funktionen in Ihre Anwendungen. Eine Power BI Embedded-Instanz ist im obigen Preisbeispiel enthalten.

## <a name="next-steps"></a>Nächste Schritte

* Sehen Sie sich die [Azure-Referenzarchitektur für die automatisierte Enterprise BI-Instanz](/azure/architecture/reference-architectures/data/enterprise-bi-adf) an. Hier finden Sie auch eine Anleitung für die Bereitstellung einer Instanz dieser Architektur in Azure.
* Lesen Sie die [Kundengeschichte von Maritz Motivation Solutions][source-document]. Diese Geschichte beschreibt einen ähnlichen Ansatz für die Verwaltung von Kundendaten.
* Der [Azure-Datenarchitekturleitfaden](/azure/architecture/data-guide) enthält umfassende Architekturinformationen zu Datenpipelines, Data Warehouses, analytischer Onlineverarbeitung (Online Analytical Processing, OLAP) und Big Data.

<!-- links -->
[source-document]: https://customers.microsoft.com/story/maritz
[calculator]: https://azure.com/e/b798fb70c53e4dd19fdeacea4db78276
[architecture]: ./media/architecture-data-warehouse.png
