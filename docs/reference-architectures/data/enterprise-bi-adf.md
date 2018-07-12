---
title: Automatisierte Enterprise BI-Instanz mit SQL Data Warehouse und Azure Data Factory
description: Automatisieren eines ELT-Workflows in Azure mithilfe von Azure Data Factory
author: MikeWasson
ms.date: 07/01/2018
ms.openlocfilehash: ffd75ba8c57a9afbc6abad61f21f738c644c9bc8
ms.sourcegitcommit: 58d93e7ac9a6d44d5668a187a6827d7cd4f5a34d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/02/2018
ms.locfileid: "37142280"
---
# <a name="automated-enterprise-bi-with-sql-data-warehouse-and-azure-data-factory"></a>Automatisierte Enterprise BI-Instanz mit SQL Data Warehouse und Azure Data Factory

Diese Referenzarchitektur zeigt, wie inkrementelles Laden in einer [ELT](../../data-guide/relational-data/etl.md#extract-load-and-transform-elt)-Pipeline (Extrahieren, Laden, Transformieren). Sie verwendet Azure Data Factory, um die ELT-Pipeline zu automatisieren. Die Pipeline verschiebt die neuesten OLTP-Daten inkrementell aus einer lokalen SQL Server-Datenbank in SQL Data Warehouse. Transaktionsdaten werden in ein tabellarisches Modell für die Analyse transformiert. [**So stellen Sie diese Lösung bereit**.](#deploy-the-solution)

![](./images/enterprise-bi-sqldw-adf.png)

Diese Architektur baut auf der in [Enterprise BI mit SQL Data Warehouse](./enterprise-bi-sqldw.md) gezeigten auf, weist jedoch einige zusätzliche Funktionen auf, die für Data Warehousing-Szenarien für Unternehmen wichtig sind.

-   Automatisierung der Pipeline mithilfe von Data Factory.
-   Inkrementelles Laden.
-   Integration mehrerer Datenquellen.
-   Laden von binären Daten wie räumliche Daten und Bilder.

## <a name="architecture"></a>Architecture

Die Architektur umfasst die folgenden Komponenten.

### <a name="data-sources"></a>Datenquellen

**Lokaler SQL Server**. Die Quelldaten befinden sich in einer lokalen SQL Server-Datenbank. Um die lokale Umgebung zu simulieren, stellen die Bereitstellungsskripts für diese Architektur eine VM in Azure bereit, auf der SQL Server installiert ist. Die [OLTP-Beispieldatenbank von Wide World Importers][wwi] wird als Quelldatenbank verwendet.

**Externe Daten**. Ein gängiges Szenario für Data Warehouses ist die Integration mehrerer Datenquellen. Diese Referenzarchitektur lädt ein externes Dataset, das die Stadtbevölkerung nach Jahr enthält, und integriert es in die Daten aus der OLTP-Datenbank. Sie können diese Daten für Erkenntnisse nutzen, z.B.: „Entspricht die Umsatzsteigerung in jeder Region dem Bevölkerungswachstum, oder ist sie unverhältnismäßig höher?“

### <a name="ingestion-and-data-storage"></a>Erfassung und Datenspeicherung

**Blobspeicher**. Blobspeicher wird als Stagingbereich für die Quelldaten vor dem Laden in SQL Data Warehouse verwendet.

**Azure SQL Data Warehouse**. [SQL Data Warehouse](/azure/sql-data-warehouse/) ist ein verteiltes System für die Analyse großer Datenmengen. Es unterstützt massive Parallelverarbeitung (Massive Parallel Processing, MPP), die die Ausführung von Hochleistungsanalysen ermöglicht. 

**Azure Data Factory** [Data Factory][adf] ist ein verwalteter Dienst, der Datenverschiebung und Datentransformation orchestriert und automatisiert. In dieser Architektur koordiniert er die verschiedenen Phasen des ELT-Prozesses.

### <a name="analysis-and-reporting"></a>Analysen und Berichte

**Azure Analysis Services**: [Analysis Services](/azure/analysis-services/) ist ein vollständig verwalteter Dienst, der Datenmodellierungsfunktionen ermöglicht. Das semantische Modell wird in Analysis Services geladen.

**Power BI**: Power BI ist eine Suite aus Business Analytics-Tools zum Analysieren von Daten für Einblicke in Geschäftsvorgänge. In dieser Architektur dient sie zum Abfragen des in Analysis Services gespeicherten semantischen Modells.

### <a name="authentication"></a>Authentifizierung

**Azure Active Directory** (Azure AD) authentifiziert Benutzer, die über Power BI eine Verbindung mit dem Analysis Services-Server herstellen.

Data Factory auch Azure AD für die Authentifizierung bei SQL Data Warehouse nutzen, indem ein Dienstprinzipal oder eine verwaltete Dienstidentität (MSI) verwendet wird. Der Einfachheit halber verwendet die Beispielbereitstellung die SQL Server-Authentifizierung.

## <a name="data-pipeline"></a>Datenpipeline

In [Azure Data Factory][adf] ist eine Pipeline eine logische Gruppierung von Aktivitäten zum Koordinieren einer Aufgabe &mdash; in diesem Fall das Laden und Transformieren von Daten in SQL Data Warehouse. 

Diese Referenzarchitektur definiert eine Masterpipeline, die eine Sequenz von untergeordneten Pipelines ausführt. Jede untergeordnete Pipeline lädt Daten in eine oder mehrere Data Warehouse-Tabellen.

![](./images/adf-pipeline.png)

## <a name="incremental-loading"></a>Inkrementelles Laden

Beim Ausführen eines automatisierten ETL-oder ELT-Prozesses ist es am effizientesten, nur die Daten zu laden, die sich seit der vorhergehenden Ausführung geändert haben. Dies wird als *inkrementeller Ladevorgang* bezeichnet, im Gegensatz zu einem vollständige Ladevorgang, bei dem alle Daten geladen werden. Zum Ausführen eines inkrementellen Ladevorgangs benötigen Sie eine Möglichkeit zum Identifizieren, welche Daten sich geändert haben. Der gängigste Ansatz ist die Verwendung eines Werts zum *Erkennen von Änderungen mit oberem Grenzwert*, d.h. der aktuelle Wert einer Spalte in der Quelltabelle – entweder einer datetime-Spalte oder einer eindeutigen Integer-Spalte – wird nachverfolgt. 

Ab SQL Server 2016 können Sie [temporale Tabellen](/sql/relational-databases/tables/temporal-tables) verwenden. Hierbei handelt es sich um Tabellen mit Systemversionsverwaltung, die einen vollständigen Verlauf aller Datenänderungen beibehalten. Die Datenbank-Engine zeichnet den Verlauf aller Änderungen automatisch in einer separaten Verlaufstabelle auf. Sie können die Verlaufsdaten abfragen, indem Sie eine FOR SYSTEM_TIME-Klausel an eine Abfrage anhängen. Intern fragt die Datenbank-Engine die Verlaufstabelle ab, dies ist für die Anwendung jedoch transparent. 

> [!NOTE]
> Für frühere Versionen von SQL Server können Sie [Change Data Capture](/sql/relational-databases/track-changes/about-change-data-capture-sql-server) (CDC) verwenden. Dieser Ansatz ist weniger geeignet als temporale Tabellen, da Sie eine separate Änderungstabelle abfragen müssen, und Änderungen werden anhand einer Protokollfolgenummer anstatt eines Zeitstempels nachverfolgt. 

Temporale Tabellen sind hilfreich für Dimensionsdaten, die sich im Laufe der Zeit ändern können. Faktentabellen stellen in der Regel eine unveränderliche Transaktion wie einen Verkauf dar, und in diesem Fall ist das Beibehalten des Systemversionsverlaufs nicht sinnvoll. Stattdessen weisen Transaktionen normalerweise eine Spalte auf, die das Transaktionsdatum darstellt, das als Wasserzeichenwert verwendet werden kann. Beispielsweise enthalten in der OLTP-Datenbank von Wide World Importers die Tabellen „Sales.Invoices“ und „Sales.InvoiceLines“ das Feld `LastEditedWhen`, dessen Standardwert `sysdatetime()` ist. 

Hier ist der allgemeine Ablauf für die ELT-Pipeline:

1. Verfolgen Sie für jede Tabelle in der Quelldatenbank den Trennzeitpunkt der Ausführung des letzten ELT-Auftrags nach. Speichern Sie diese Informationen im Data Warehouse. (Bei der Ersteinrichtung sind alle Zeiten auf „1.1.1900“ festgelegt.)

2. Während des Datenexportschritts wird der Trennzeitpunkt als Parameter an einen Satz von gespeicherten Prozeduren in der Quelldatenbank übergeben. Diese gespeicherten Prozeduren fragen alle Datensätze ab, die nach dem Trennzeitpunkt geändert oder erstellt wurden. Für die Sales-Faktentabelle wird die Spalte `LastEditedWhen` verwendet. Für die Dimensionsdaten werden temporale Tabellen mit Systemversionsverwaltung verwendet.

3. Wenn die Datenmigration abgeschlossen ist, aktualisieren Sie die Tabelle, in der die Trennzeitpunkte gespeichert werden.

Es ist auch hilfreich, eine *Herkunft* für jede ELT-Ausführung aufzuzeichnen. Für einen bestimmten Datensatz ordnet die Herkunft diesen Datensatz der ELT-Ausführung zu, bei der die Daten erzeugt wurden. Bei jeder ETL-Ausführung wird für jede Tabelle ein neuer Herkunftsdatensatz erstellt, der die Start- und Endladezeiten anzeigt. Die Herkunftsschlüssel für die einzelnen Datensätze werden in den Dimensions- und Faktentabellen gespeichert.

![](./images/city-dimension-table.png)

Aktualisieren Sie das Analysis Services-Tabellenmodell, nachdem ein neuer Datenbatch in das Warehouse geladen wurde. Siehe [Asynchrones Aktualisieren mit der REST-API](/azure/analysis-services/analysis-services-async-refresh).

## <a name="data-cleansing"></a>Datenbereinigung

Die Datenbereinigung sollte Teil des ELT-Prozesses sein. In dieser Referenzarchitektur ist die Tabelle mit der Stadtbevölkerung eine Quelle ungültiger Daten, in der einige Städte keine Bevölkerung aufweisen, da möglicherweise keine Daten verfügbar waren. Während der Verarbeitung entfernt die ELT-Pipeline diese Städte aus der Tabelle mit der Stadtbevölkerung. Führen Sie die Datenbereinigung in Stagingtabellen statt in externen Tabellen durch.

Hier ist die gespeicherte Prozedur, die die Städte ohne Bevölkerung der Tabelle mit der Stadtbevölkerung entfernt. (Sie finden die Quelldatei [hier](https://github.com/mspnp/reference-architectures/blob/master/data/enterprise_bi_sqldw_advanced/azure/sqldw_scripts/citypopulation/%5BIntegration%5D.%5BMigrateExternalCityPopulationData%5D.sql).) 

```sql
DELETE FROM [Integration].[CityPopulation_Staging]
WHERE RowNumber in (SELECT DISTINCT RowNumber
FROM [Integration].[CityPopulation_Staging]
WHERE POPULATION = 0
GROUP BY RowNumber
HAVING COUNT(RowNumber) = 4)
```

## <a name="external-data-sources"></a>Externe Datenquellen

Data Warehouses fassen häufig Daten aus mehreren Quellen zusammen. Diese Referenzarchitektur lädt eine externe Datenquelle, die demografische Daten enthält. Dieses Dataset in Azure Blob Storage als Teil des Beispiels [WorldWideImportersDW](https://github.com/Microsoft/sql-server-samples/tree/master/samples/databases/wide-world-importers/sample-scripts/polybase) verfügbar.

Azure Data Factory kann mithilfe des [Blob Storage-Connectors](/azure/data-factory/connector-azure-blob-storage) direkt aus Blob Storage kopieren. Der Connector erfordert jedoch eine Verbindungszeichenfolge oder eine Shared Access Signature, damit er nicht zum Kopieren eines Blobs mit öffentlichem Lesezugriff verwendet werden kann. Um dieses Problem zu umgehen, können Sie PolyBase zum Erstellen einer externen Tabelle über Blob Storage verwenden und die externen Tabellen dann in SQL Data Warehouse kopieren. 

## <a name="handling-large-binary-data"></a>Verarbeiten von umfangreichen Binärdaten 

In der Quelldatenbank weist die Tabelle mit Städten die Spalte „Ort“ auf, die den räumlichen Datentyp [Geografie](/sql/t-sql/spatial-geography/spatial-types-geography) enthält. SQL Data Warehouse unterstützt den Typ **Geografie** nicht systemintern, daher wird dieses Feld während des Ladens in einen **varbinary**-Typ konvertiert. (Siehe [Verwenden von Problemumgehungen für nicht unterstützte Datentypen](/azure/sql-data-warehouse/sql-data-warehouse-tables-data-types#unsupported-data-types).)

PolyBase unterstützt jedoch eine maximale Spaltengröße von `varbinary(8000)`, was bedeutet, dass einige Daten möglicherweise abgeschnitten werden. Dieses Problem lässt sich umgehen, indem die Daten während des Exports wie folgt in Blöcke aufgeteilt und dann wieder zusammengefügt werden:

1. Erstellen Sie eine temporäre Stagingtabelle für die Standort-Spalte.

2. Teilen Sie die Standortdaten für jede Stadt in Blöcke von 8000 Bytes auf. Dies ergibt 1 &ndash; N Zeilen für jede Stadt.

3. Um die Blöcke wieder zusammenzusetzen, verwenden Sie den T-SQL-[PIVOT](/sql/t-sql/queries/from-using-pivot-and-unpivot)-Operator, um Zeilen in Spalten umwandeln und dann die Spaltenwerte für jede Stadt zu verketten.

Die Herausforderung besteht darin, dass jede Stadt je nach Größe der geografischen Daten in eine unterschiedliche Anzahl von Zeilen aufgeteilt wird. Damit der PIVOT-Operator funktioniert, muss jede Stadt die gleiche Anzahl von Zeilen aufweisen. Dazu wendet die T-SQL-Abfrage (die Sie [hier][MergeLocation] anzeigen können) einige Tricks aus, um die Zeilen mit leeren Werten aufzufüllen, sodass jede Stadt nach dem Pivot-Vorgang die gleiche Anzahl von Spalten aufweist. Die resultierende Abfrage erweist sich als wesentlich schneller als Durchlaufen der einzelnen Zeilen.

Der gleiche Ansatz wird für Bilddaten verwendet.

## <a name="slowly-changing-dimensions"></a>Langsam veränderliche Dimensionen

Dimensionsdaten sind relativ statisch, können sich jedoch ändern. Beispielsweise kann ein Produkt einer anderen Produktkategorie zugewiesen werden. Es gibt verschiedene Ansätze zur Handhabung von langsam veränderlichen Dimensionen. Ein gängiges Verfahren mit der Bezeichnung [Typ 2](https://wikipedia.org/wiki/Slowly_changing_dimension#Type_2:_add_new_row) ist das Hinzufügen eines neuen Datensatzes bei jeder Dimensionsänderung. 

Zur Umsetzung des Typ 2-Ansatzes erfordern Dimensionstabellen zusätzliche Spalten, die den effektiven Datumsbereich für einen bestimmten Datensatz angeben. Außerdem werden Primärschlüssel aus der Quelldatenbank dupliziert, daher muss die Dimensionstabelle über einen künstlichen Primärschlüssel verfügen.

Die folgende Abbildung zeigt die Tabelle „Dimension.City“. Die Spalte `WWI City ID` ist der Primärschlüssel aus der Quelldatenbank. Die Spalte `City Key` ist ein künstlicher Schlüssel, der während der Verarbeitung der ETL-Pipeline generiert wurde. Beachten Sie auch, dass in der Tabelle die Spalten `Valid From` und `Valid To` vorhanden sind, die den Bereich definieren, in dem die einzelnen Zeilen gültig waren. Das `Valid To`-Element aktueller Werte entspricht „9999-12-31“.

![](./images/city-dimension-table.png)

Der Vorteil dieses Ansatzes ist, dass historische Daten beibehalten werden, die für die Analyse nützlich sein können. Allerdings sind dabei auch mehrere Zeilen für dieselbe Entität vorhanden. Hier sehen Sie beispielsweise Datensätze, die `WWI City ID` = 28561 entsprechen:

![](./images/city-dimension-table-2.png)

Ordnen Sie jeden Sales-Fakt einer einzelnen Zeile in der Tabelle „Dimension.City“ zu, die dem Rechnungsdatum entspricht. Erstellen Sie als Teil des ETL-Prozesses eine weitere Spalte, die 

Die folgende T-SQL-Abfrage erstellt eine temporäre Tabelle, die jede Rechnung dem richtigen City-Schlüssel aus der Tabelle „Dimension.City“ zuordnet.

```sql
CREATE TABLE CityHolder
WITH (HEAP , DISTRIBUTION = HASH([WWI Invoice ID]))
AS
SELECT DISTINCT s1.[WWI Invoice ID] AS [WWI Invoice ID],
                c.[City Key] AS [City Key]
    FROM [Integration].[Sale_Staging] s1
    CROSS APPLY (
                SELECT TOP 1 [City Key]
                    FROM [Dimension].[City]
                WHERE [WWI City ID] = s1.[WWI City ID]
                    AND s1.[Last Modified When] > [Valid From]
                    AND s1.[Last Modified When] <= [Valid To]
                ORDER BY [Valid From], [City Key] DESC
                ) c

```

Diese Tabelle wird verwendet, um eine Spalte in der Sales-Faktentabelle auszufüllen:

```sql
UPDATE [Integration].[Sale_Staging]
SET [Integration].[Sale_Staging].[WWI Customer ID] =  CustomerHolder.[WWI Customer ID]
```

Diese Spalte ermöglicht, dass eine Power BI-Abfrage den richtigen City-Datensatz für eine bestimmte Verkaufsrechnung findet.

## <a name="security-considerations"></a>Sicherheitshinweise

Für höhere Sicherheit können Sie [Virtual Network-Dienstendpunkte](/azure/virtual-network/virtual-network-service-endpoints-overview) zum Schützen von Azure-Dienstressourcen verwenden, indem diese ausschließlich auf Ihr virtuelles Netzwerk beschränkt sind. Der öffentliche Internetzugriff auf diese Ressourcen wird dadurch vollständig entfernt, sodass nur Datenverkehr aus Ihrem virtuellen Netzwerk zulässig ist.

Mit diesem Ansatz erstellen Sie ein VNET in Azure und dann private Dienstendpunkte für Azure-Dienste. Diese Dienste sind dann auf Datenverkehr aus diesem virtuellen Netzwerk beschränkt. Sie können auch über ein Gateway aus Ihrem lokalen Netzwerk darauf zugreifen.

Bedenken Sie dabei folgende Einschränkungen:

- Zum Zeitpunkt der Erstellung dieser Referenzarchitektur werden VNET-Dienstendpunkte für Azure Storage und Azure SQL Data Warehouse, aber nicht für Azure Analysis Services unterstützt. Überprüfen Sie den aktuellen Status [hier](https://azure.microsoft.com/updates/?product=virtual-network). 

- Wenn Dienstendpunkte für Azure Storage aktiviert sind, kann PolyBase keine Daten aus Storage in SQL Data Warehouse kopieren. Dieses Problem kann entschärft werden. Weitere Informationen finden Sie unter [Auswirkungen der Verwendung von VNET-Dienstendpunkten mit Azure Storage](/azure/sql-database/sql-database-vnet-service-endpoint-rule-overview?toc=%2fazure%2fvirtual-network%2ftoc.json#impact-of-using-vnet-service-endpoints-with-azure-storage). 

- Zum Verschieben von Daten aus einem lokalen Speicher in Azure Storage müssen Sie öffentliche IP-Adressen in Ihrem lokalen Speicher oder ExpressRoute auf die Whitelist setzen. Details finden Sie unter [Schützen von Azure-Diensten in virtuellen Netzwerken](/azure/virtual-network/virtual-network-service-endpoints-overview#securing-azure-services-to-virtual-networks).

- Stellen Sie im virtuellen Netzwerk einen virtuellen Windows-Computer bereit, der den SQL Data Warehouse-Dienstendpunkt enthält, damit Analysis Services Daten aus SQL Data Warehouse lesen kann. Installieren Sie auf diesem virtuellen Computer ein [lokales Azure-Datengateway](/azure/analysis-services/analysis-services-gateway). Verbinden Sie dann Ihren Azure Analysis-Dienst, mit dem Datengateway.

## <a name="deploy-the-solution"></a>Bereitstellen der Lösung

Eine Bereitstellung für diese Referenzarchitektur ist auf [GitHub][ref-arch-repo-folder] verfügbar. Folgendes wird bereitgestellt:

  * Eine Windows-VM, um einen lokalen Datenbankserver zu simulieren. Sie enthält SQL Server 2017 und zugehörige Tools zusammen mit Power BI Desktop.
  * Ein Azure Storage-Konto, das Blobspeicher zum Speichern von Daten bereitstellt, die aus SQL Server-Datenbank exportiert wurden.
  * Eine Instanz von Azure SQL Data Warehouse.
  * Eine Azure Analysis Services-Instanz.
  * Azure Data Factory und die Data Factory-Pipeline für den ELT-Auftrag.

### <a name="prerequisites"></a>Voraussetzungen

[!INCLUDE [ref-arch-prerequisites.md](../../../includes/ref-arch-prerequisites.md)]

### <a name="variables"></a>Variables

Die folgenden Schritte enthalten einige benutzerdefinierte Variablen. Sie müssen diese durch von Ihnen definierte Werte ersetzen.

- `<data_factory_name>`(Fixierte Verbindung) festgelegt ist(Fixierte Verbindung) festgelegt ist. Data Factory-Name.
- `<analysis_server_name>`(Fixierte Verbindung) festgelegt ist(Fixierte Verbindung) festgelegt ist. Name des Analysis Services-Servers.
- `<active_directory_upn>`(Fixierte Verbindung) festgelegt ist(Fixierte Verbindung) festgelegt ist. Ihr Azure Active Directory-Benutzerprinzipalname (User Principal Name, UPN). Beispiel: `user@contoso.com`.
- `<data_warehouse_server_name>`(Fixierte Verbindung) festgelegt ist(Fixierte Verbindung) festgelegt ist. Name des SQL Data Warehouse-Servers.
- `<data_warehouse_password>`(Fixierte Verbindung) festgelegt ist(Fixierte Verbindung) festgelegt ist. SQL Data Warehouse-Administratorkennwort.
- `<resource_group_name>`(Fixierte Verbindung) festgelegt ist(Fixierte Verbindung) festgelegt ist. Der Name der Ressourcengruppe.
- `<region>`(Fixierte Verbindung) festgelegt ist(Fixierte Verbindung) festgelegt ist. Die Azure-Region, in der die Ressourcen bereitgestellt werden.
- `<storage_account_name>`(Fixierte Verbindung) festgelegt ist(Fixierte Verbindung) festgelegt ist. Name des Speicherkontos. Die [Benennungsregeln](../../best-practices/naming-conventions.md#naming-rules-and-restrictions) für Storage-Konten müssen eingehalten werden.
- `<sql-db-password>`(Fixierte Verbindung) festgelegt ist(Fixierte Verbindung) festgelegt ist. Kennwort für die SQL Server-Anmeldung.

### <a name="deploy-azure-data-factory"></a>Bereitstellen von Azure Data Factory

1. Navigieren Sie zum Ordner `data\enterprise_bi_sqldw_advanced\azure\templates` des [GitHub-Repositorys][ref-arch-repo].

2. Führen Sie den folgenden Azure CLI-Befehl aus, um eine Ressourcengruppe zu erstellen:  

    ```bash
    az group create --name <resource_group_name> --location <region>  
    ```

    Geben Sie eine Region an, die SQL Data Warehouse, Azure Analysis Services und Data Factory v2 unterstützt. Informationen finden Sie unter [Azure-Produkte nach Region](https://azure.microsoft.com/global-infrastructure/services/).

3. Führen Sie den folgenden Befehl aus

    ```
    az group deployment create --resource-group <resource_group_name> \
        --template-file adf-create-deploy.json \
        --parameters factoryName=<data_factory_name> location=<location>
    ```

Rufen Sie als Nächstes im Azure-Portal wie folgt den Authentifizierungsschlüssel für die [Integration Runtime ](/azure/data-factory/concepts-integration-runtime) von Azure Data Factory ab:

1. Navigieren Sie im [Azure-Portal](https://portal.azure.com/) zur Data Factory-Instanz.

2. Klicken Sie auf dem Data Factory-Blatt auf **Erstellen und überwachen**. Dadurch wird das Azure Data Factory-Portal in einem neuen Browserfenster geöffnet.

    ![](./images/adf-blade.png)

3. Wählen Sie im Azure Data Factory-Portal das Bleistiftsymbol („Autor“) aus. 

4. Klicken Sie auf **Verbindungen**, und wählen Sie dann **Integration Runtimes**.

5. Klicken Sie unter **sourceIntegrationRuntime** Bleistiftsymbol („Bearbeiten“).

    > [!NOTE]
    > Im Portal wird der Status als „Nicht verfügbar“ angezeigt. Dies ist zu erwarten, bis Sie den lokalen Server bereitstellen.

6. Suchen Sie **Key1**, und kopieren Sie den Wert des Authentifizierungsschlüssels.

Sie benötigen den Authentifizierungsschlüssel für den nächsten Schritt.

### <a name="deploy-the-simulated-on-premises-server"></a>Bereitstellen des simulierten lokalen Servers

In diesem Schritt wird ein virtueller Computer als simulierter lokaler Server bereitgestellt, auf dem SQL Server 2017 und die zugehörigen Tools vorhanden sind. Außerdem wird die [OLTP-Datenbank von Wide World Importers ][wwi] in SQL Server geladen.

1. Navigieren Sie zum Ordner `data\enterprise_bi_sqldw_advanced\onprem\templates` des Repositorys.

2. Suchen Sie in der Datei `onprem.parameters.json` nach `adminPassword`. Dies ist das Kennwort für die Anmeldung beim virtuellen SQL Server-Computer. Ersetzen Sie den Wert durch ein anderes Kennwort.

3. Suchen Sie in derselben Datei nach `SqlUserCredentials`. Diese Eigenschaft gibt die SQL Server-Kontoanmeldeinformationen an. Ersetzen Sie das Kennwort durch einen anderen Wert.

4. Fügen Sie in der gleichen Datei, Integration Runtime-Authentifizierungsschlüssel in den `IntegrationRuntimeGatewayKey`-Parameter ein, wie unten dargestellt:

    ```json
    "protectedSettings": {
        "configurationArguments": {
            "SqlUserCredentials": {
                "userName": ".\\adminUser",
                "password": "<sql-db-password>"
            },
            "IntegrationRuntimeGatewayKey": "<authentication key>"
        }
    ```

5. Führen Sie den folgenden Befehl aus:

    ```bash
    azbb -s <subscription_id> -g <resource_group_name> -l <region> -p onprem.parameters.json --deploy
    ```

Die Ausführung dieses Schritts kann 20 bis 30 Minuten dauern. Er umfasst die Ausführung eines [DSC](/powershell/dsc/overview)-Skripts zum Installieren der Tools und zum Wiederherstellen der Datenbank. 

### <a name="deploy-azure-resources"></a>Bereitstellen von Azure-Ressourcen

In diesem Schritt werden SQL Data Warehouse, Azure Analysis Services und Data Factory bereitgestellt.

1. Navigieren Sie zum Ordner `data\enterprise_bi_sqldw_advanced\azure\templates` des [GitHub-Repositorys][ref-arch-repo].

2. Führen Sie den folgenden Azure-CLI-Befehl aus: Ersetzen Sie die Parameterwerte in spitzen Klammern.

    ```bash
    az group deployment create --resource-group <resource_group_name> \
     --template-file azure-resources-deploy.json \
     --parameters "dwServerName"="<data_warehouse_server_name>" \
     "dwAdminLogin"="adminuser" "dwAdminPassword"="<data_warehouse_password>" \ 
     "storageAccountName"="<storage_account_name>" \
     "analysisServerName"="<analysis_server_name>" \
     "analysisServerAdmin"="<user@contoso.com>"
    ```

    - Beim `storageAccountName`-Parameter müssen Sie die [Benennungsregeln](../../best-practices/naming-conventions.md#naming-rules-and-restrictions) für Speicherkonten befolgen. 
    - Verwenden Sie für den `analysisServerAdmin`-Parameter Ihren Azure Active Directory-Benutzerprinzipalnamen (User Principal Name, UPN).

3. Führen Sie den folgenden Azure CLI-Befehl zum Abrufen des Zugriffsschlüssels für das Speicherkonto aus. Diesen Schlüssel verwenden Sie im nächsten Schritt.

    ```bash
    az storage account keys list -n <storage_account_name> -g <resource_group_name> --query [0].value
    ```

4. Führen Sie den folgenden Azure-CLI-Befehl aus: Ersetzen Sie die Parameterwerte in spitzen Klammern. 

    ```bash
    az group deployment create --resource-group <resource_group_name> \
    --template-file adf-pipeline-deploy.json \
    --parameters "factoryName"="<data_factory_name>" \
    "sinkDWConnectionString"="Server=tcp:<data_warehouse_server_name>.database.windows.net,1433;Initial Catalog=wwi;Persist Security Info=False;User ID=adminuser;Password=<data_warehouse_password>;MultipleActiveResultSets=False;Encrypt=True;TrustServerCertificate=False;Connection Timeout=30;" \
    "blobConnectionString"="DefaultEndpointsProtocol=https;AccountName=<storage_account_name>;AccountKey=<storage_account_key>;EndpointSuffix=core.windows.net" \
    "sourceDBConnectionString"="Server=sql1;Database=WideWorldImporters;User Id=adminuser;Password=<sql-db-password>;Trusted_Connection=True;"
    ```

    Die Verbindungszeichenfolgen weisen Teilzeichenfolgen in spitzen Klammern auf, die ersetzt werden müssen. Verwenden Sie für `<storage_account_key>` den Schlüssel, den Sie im vorherigen Schritt abgerufen haben. Verwenden Sie für `<sql-db-password>` das SQL Server-Kontokennwort, das Sie zuvor in der Datei `onprem.parameters.json` angegeben haben.

### <a name="run-the-data-warehouse-scripts"></a>Ausführen der Data Warehouse-Skripts

1. Suchen Sie im [Azure-Portal](https://portal.azure.com/) den lokalen virtuellen Computer mit dem Namen `sql-vm1`. Der Benutzername und das Kennwort für den virtuellen Computer sind in der Datei `onprem.parameters.json` angegeben.

2. Klicken Sie auf **Verbinden**, und stellen Sie über Remotedesktop eine Verbindung zum virtuellen Computer her.

3. Öffnen Sie über die Remotedesktopsitzung eine Eingabeaufforderung, und navigieren Sie zum folgenden Ordner auf dem virtuellen Computer:

    ```
    cd C:\SampleDataFiles\reference-architectures\data\enterprise_bi_sqldw_advanced\azure\sqldw_scripts
    ```

4. Führen Sie den folgenden Befehl aus:

    ```
    deploy_database.cmd -S <data_warehouse_server_name>.database.windows.net -d wwi -U adminuser -P <data_warehouse_password> -N -I
    ```

    Für `<data_warehouse_server_name>` und `<data_warehouse_password>` verwenden Sie den Data Warehouse-Namen und das Kennwort von vorhin.

Zum Überprüfen dieses Schritts können Sie mithilfe von SQL Server Management Studio (SSMS) eine Verbindung mit der SQL Data Warehouse-Datenbank herstellen. Die Datenbank-Tabellenschemas sollten angezeigt werden.

### <a name="run-the-data-factory-pipeline"></a>Ausführen der Data Factory-Pipeline

1. Öffnen Sie in der gleichen Remotedesktopsitzung ein PowerShell-Fenster.

2. Führen Sie den folgenden PowerShell-Befehl aus: Wählen Sie **Ja**, wenn Sie dazu aufgefordert werden.

    ```powershell
    Install-Module -Name AzureRM -AllowClobber
    ```

3. Führen Sie den folgenden PowerShell-Befehl aus: Geben Sie Ihre Azure-Anmeldeinformationen ein.

    ```powershell
    Connect-AzureRmAccount 
    ```

4. Führen Sie die folgenden PowerShell-Befehle aus: Ersetzen Sie die Werte in spitzen Klammern.

    ```powershell
    Set-AzureRmContext -SubscriptionId <subscription id>

    Invoke-AzureRmDataFactoryV2Pipeline -DataFactory <data-factory-name> -PipelineName "MasterPipeline" -ResourceGroupName <resource_group_name>

5. In the Azure Portal, navigate to the Data Factory instance that was created earlier.

6. In the Data Factory blade, click **Author & Monitor**. This opens the Azure Data Factory portal in another browser window.

    ![](./images/adf-blade.png)

7. In the Azure Data Factory portal, click the **Monitor** icon. 

8. Verify that the pipeline completes successfully. It can take a few minutes.

    ![](./images/adf-pipeline-progress.png)


## Build the Analysis Services model

In this step, you will create a tabular model that imports data from the data warehouse. Then you will deploy the model to Azure Analysis Services.

**Create a new tabular project**

1. From your Remote Desktop session, launch SQL Server Data Tools 2015.

2. Select **File** > **New** > **Project**.

3. In the **New Project** dialog, under **Templates**, select  **Business Intelligence** > **Analysis Services** > **Analysis Services Tabular Project**. 

4. Name the project and click **OK**.

5. In the **Tabular model designer** dialog, select **Integrated workspace**  and set **Compatibility level** to `SQL Server 2017 / Azure Analysis Services (1400)`. 

6. Click **OK**.


**Import data**

1. In the **Tabular Model Explorer** window, right-click the project and select **Import from Data Source**.

2. Select **Azure SQL Data Warehouse** and click **Connect**.

3. For **Server**, enter the fully qualified name of your Azure SQL Data Warehouse server. You can get this value from the Azure Portal. For **Database**, enter `wwi`. Click **OK**.

4. In the next dialog, choose **Database** authentication and enter your Azure SQL Data Warehouse user name and password, and click **OK**.

5. In the **Navigator** dialog, select the checkboxes for the **Fact.\*** and **Dimension.\*** tables.

    ![](./images/analysis-services-import-2.png)

6. Click **Load**. When processing is complete, click **Close**. You should now see a tabular view of the data.

**Create measures**

1. In the model designer, select the **Fact Sale** table.

2. Click a cell in the the measure grid. By default, the measure grid is displayed below the table. 

    ![](./images/tabular-model-measures.png)

3. In the formula bar, enter the following and press ENTER:

    ```
    Total Sales:=SUM('Fact Sale'[Gesamtsumme einschließlich Steuer])
    ```

4. Repeat these steps to create the following measures:

    ```
    Number of Years:=(MAX('Fact CityPopulation'[Jahreszahl])-MIN('Fact CityPopulation'[Jahreszahl]))+1
    
    Beginning Population:=CALCULATE(SUM('Fact CityPopulation'[Bevölkerung]),FILTER('Fact CityPopulation','Fact CityPopulation'[Jahreszahl]=MIN('Fact CityPopulation'[Jahreszahl])))
    
    Ending Population:=CALCULATE(SUM('Fact CityPopulation'[Bevölkerung]),FILTER('Fact CityPopulation','Fact CityPopulation'[Jahreszahl]=MAX('Fact CityPopulation'[Jahreszahl])))
    
    CAGR:=IFERROR((([Endbevölkerung]/[Anfangsbevölkerung])^(1/[Anzahl der Jahre]))-1,0)
    ```

    ![](./images/analysis-services-measures.png)

For more information about creating measures in SQL Server Data Tools, see [Measures](/sql/analysis-services/tabular-models/measures-ssas-tabular).

**Create relationships**

1. In the **Tabular Model Explorer** window, right-click the project and select **Model View** > **Diagram View**.

2. Drag the **[Fact Sale].[City Key]** field to the **[Dimension City].[City Key]** field to create a relationship.  

3. Drag the **[Face CityPopulation].[City Key]** field to the **[Dimension City].[City Key]** field.  

    ![](./images/analysis-services-relations-2.png)

**Deploy the model**

1. From the **File** menu, choose **Save All**.

2. In **Solution Explorer**, right-click the project and select **Properties**. 

3. Under **Server**, enter the URL of your Azure Analysis Services instance. You can get this value from the Azure Portal. In the portal, select the Analysis Services resource, click the Overview pane, and look for the **Server Name** property. It will be similar to `asazure://westus.asazure.windows.net/contoso`. Click **OK**.

    ![](./images/analysis-services-properties.png)

4. In **Solution Explorer**, right-click the project and select **Deploy**. Sign into Azure if prompted. When processing is complete, click **Close**.

5. In the Azure portal, view the details for your Azure Analysis Services instance. Verify that your model appears in the list of models.

    ![](./images/analysis-services-models.png)

## Analyze the data in Power BI Desktop

In this step, you will use Power BI to create a report from the data in Analysis Services.

1. From your Remote Desktop session, launch Power BI Desktop.

2. In the Welcome Scren, click **Get Data**.

3. Select **Azure** > **Azure Analysis Services database**. Click **Connect**

    ![](./images/power-bi-get-data.png)

4. Enter the URL of your Analysis Services instance, then click **OK**. Sign into Azure if prompted.

5. In the **Navigator** dialog, expand the tabular project, select the model, and click **OK**.

2. In the **Visualizations** pane, select the **Table** icon. In the Report view, resize the visualization to make it larger.

6. In the **Fields** pane, expand **Dimension City**.

7. From **Dimension City**, drag **City** and **State Province** to the **Values** well.

9. In the **Fields** pane, expand **Fact Sale**.

10. From **Fact Sale**, drag **CAGR**, **Ending Population**,  and **Total Sales** to the **Value** well.

11. Under **Visual Level Filters**, select **Ending Population**. Set the filter to "is greater than 100000" and click **Apply filter**.

12. Under **Visual Level Filters**, select **Total Sales**. Set the filter to "is 0" and click **Apply filter**.

![](./images/power-bi-report-2.png)

The table now shows cities with population greater than 100,000 and zero sales. CAGR  stands for Compounded Annual Growth Rate and measures the rate of population growth per city. You could use this value to find cities with high growth rates, for example. However, note that the values for CAGR in the model aren't accurate, because they are derived from sample data.

To learn more about Power BI Desktop, see [Getting started with Power BI Desktop](/power-bi/desktop-getting-started).


[adf]: //azure/data-factory
[azure-cli-2]: //azure/install-azure-cli
[azbb-repo]: https://github.com/mspnp/template-building-blocks
[azbb-wiki]: https://github.com/mspnp/template-building-blocks/wiki/Install-Azure-Building-Blocks
[MergeLocation]: https://github.com/mspnp/reference-architectures/blob/master/data/enterprise_bi_sqldw_advanced/azure/sqldw_scripts/city/%5BIntegration%5D.%5BMergeLocation%5D.sql
[ref-arch-repo]: https://github.com/mspnp/reference-architectures
[ref-arch-repo-folder]: https://github.com/mspnp/reference-architectures/tree/master/data/enterprise_bi_sqldw_advanced
[wwi]: //sql/sample/world-wide-importers/wide-world-importers-oltp-database
