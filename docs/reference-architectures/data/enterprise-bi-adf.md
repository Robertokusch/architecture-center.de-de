---
title: Automatisierte Enterprise BI-Instanz mit SQL Data Warehouse und Azure Data Factory
description: Automatisieren eines ELT-Workflows in Azure mithilfe von Azure Data Factory
author: MikeWasson
ms.date: 07/01/2018
ms.openlocfilehash: f004c02da93335e74b07b9720236832ad7f744db
ms.sourcegitcommit: 62945777e519d650159f0f963a2489b6bb6ce094
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2018
ms.locfileid: "48876901"
---
# <a name="automated-enterprise-bi-with-sql-data-warehouse-and-azure-data-factory"></a><span data-ttu-id="cc8f5-103">Automatisierte Enterprise BI-Instanz mit SQL Data Warehouse und Azure Data Factory</span><span class="sxs-lookup"><span data-stu-id="cc8f5-103">Automated enterprise BI with SQL Data Warehouse and Azure Data Factory</span></span>

<span data-ttu-id="cc8f5-104">Diese Referenzarchitektur zeigt, wie inkrementelles Laden in einer [ELT](../../data-guide/relational-data/etl.md#extract-load-and-transform-elt)-Pipeline (Extrahieren, Laden, Transformieren).</span><span class="sxs-lookup"><span data-stu-id="cc8f5-104">This reference architecture shows how to perform incremental loading in an [ELT](../../data-guide/relational-data/etl.md#extract-load-and-transform-elt) (extract-load-transform) pipeline.</span></span> <span data-ttu-id="cc8f5-105">Sie verwendet Azure Data Factory, um die ELT-Pipeline zu automatisieren.</span><span class="sxs-lookup"><span data-stu-id="cc8f5-105">It uses Azure Data Factory to automate the ELT pipeline.</span></span> <span data-ttu-id="cc8f5-106">Die Pipeline verschiebt die neuesten OLTP-Daten inkrementell aus einer lokalen SQL Server-Datenbank in SQL Data Warehouse.</span><span class="sxs-lookup"><span data-stu-id="cc8f5-106">The pipeline incrementally moves the latest OLTP data from an on-premises SQL Server database into SQL Data Warehouse.</span></span> <span data-ttu-id="cc8f5-107">Transaktionsdaten werden in ein tabellarisches Modell für die Analyse transformiert.</span><span class="sxs-lookup"><span data-stu-id="cc8f5-107">Transactional data is transformed into a tabular model for analysis.</span></span> [<span data-ttu-id="cc8f5-108">**So stellen Sie diese Lösung bereit**.</span><span class="sxs-lookup"><span data-stu-id="cc8f5-108">**Deploy this solution**.</span></span>](#deploy-the-solution)

![](./images/enterprise-bi-sqldw-adf.png)

<span data-ttu-id="cc8f5-109">Diese Architektur baut auf der in [Enterprise BI mit SQL Data Warehouse](./enterprise-bi-sqldw.md) gezeigten auf, weist jedoch einige zusätzliche Funktionen auf, die für Data Warehousing-Szenarien für Unternehmen wichtig sind.</span><span class="sxs-lookup"><span data-stu-id="cc8f5-109">This architecture builds on the one shown in [Enterprise BI with SQL Data Warehouse](./enterprise-bi-sqldw.md), but adds some features that are important for enterprise data warehousing scenarios.</span></span>

-   <span data-ttu-id="cc8f5-110">Automatisierung der Pipeline mithilfe von Data Factory.</span><span class="sxs-lookup"><span data-stu-id="cc8f5-110">Automation of the pipeline using Data Factory.</span></span>
-   <span data-ttu-id="cc8f5-111">Inkrementelles Laden.</span><span class="sxs-lookup"><span data-stu-id="cc8f5-111">Incremental loading.</span></span>
-   <span data-ttu-id="cc8f5-112">Integration mehrerer Datenquellen.</span><span class="sxs-lookup"><span data-stu-id="cc8f5-112">Integrating multiple data sources.</span></span>
-   <span data-ttu-id="cc8f5-113">Laden von binären Daten wie räumliche Daten und Bilder.</span><span class="sxs-lookup"><span data-stu-id="cc8f5-113">Loading binary data such as geospatial data and images.</span></span>

## <a name="architecture"></a><span data-ttu-id="cc8f5-114">Architecture</span><span class="sxs-lookup"><span data-stu-id="cc8f5-114">Architecture</span></span>

<span data-ttu-id="cc8f5-115">Die Architektur umfasst die folgenden Komponenten.</span><span class="sxs-lookup"><span data-stu-id="cc8f5-115">The architecture consists of the following components.</span></span>

### <a name="data-sources"></a><span data-ttu-id="cc8f5-116">Datenquellen</span><span class="sxs-lookup"><span data-stu-id="cc8f5-116">Data sources</span></span>

<span data-ttu-id="cc8f5-117">**Lokaler SQL Server**.</span><span class="sxs-lookup"><span data-stu-id="cc8f5-117">**On-premises SQL Server**.</span></span> <span data-ttu-id="cc8f5-118">Die Quelldaten befinden sich in einer lokalen SQL Server-Datenbank.</span><span class="sxs-lookup"><span data-stu-id="cc8f5-118">The source data is located in a SQL Server database on premises.</span></span> <span data-ttu-id="cc8f5-119">Um die lokale Umgebung zu simulieren, stellen die Bereitstellungsskripts für diese Architektur eine VM in Azure bereit, auf der SQL Server installiert ist.</span><span class="sxs-lookup"><span data-stu-id="cc8f5-119">To simulate the on-premises environment, the deployment scripts for this architecture provision a virtual machine in Azure with SQL Server installed.</span></span> <span data-ttu-id="cc8f5-120">Die [OLTP-Beispieldatenbank von Wide World Importers][wwi] wird als Quelldatenbank verwendet.</span><span class="sxs-lookup"><span data-stu-id="cc8f5-120">The [Wide World Importers OLTP sample database][wwi] is used as the source database.</span></span>

<span data-ttu-id="cc8f5-121">**Externe Daten**.</span><span class="sxs-lookup"><span data-stu-id="cc8f5-121">**External data**.</span></span> <span data-ttu-id="cc8f5-122">Ein gängiges Szenario für Data Warehouses ist die Integration mehrerer Datenquellen.</span><span class="sxs-lookup"><span data-stu-id="cc8f5-122">A common scenario for data warehouses is to integrate multiple data sources.</span></span> <span data-ttu-id="cc8f5-123">Diese Referenzarchitektur lädt ein externes Dataset, das die Stadtbevölkerung nach Jahr enthält, und integriert es in die Daten aus der OLTP-Datenbank.</span><span class="sxs-lookup"><span data-stu-id="cc8f5-123">This reference architecture loads an external data set that contains city populations by year, and integrates it with the data from the OLTP database.</span></span> <span data-ttu-id="cc8f5-124">Sie können diese Daten für Erkenntnisse nutzen, z.B.: „Entspricht die Umsatzsteigerung in jeder Region dem Bevölkerungswachstum, oder ist sie unverhältnismäßig höher?“</span><span class="sxs-lookup"><span data-stu-id="cc8f5-124">You can use this data for insights such as: "Does sales growth in each region match or exceed population growth?"</span></span>

### <a name="ingestion-and-data-storage"></a><span data-ttu-id="cc8f5-125">Erfassung und Datenspeicherung</span><span class="sxs-lookup"><span data-stu-id="cc8f5-125">Ingestion and data storage</span></span>

<span data-ttu-id="cc8f5-126">**Blobspeicher**.</span><span class="sxs-lookup"><span data-stu-id="cc8f5-126">**Blob Storage**.</span></span> <span data-ttu-id="cc8f5-127">Blobspeicher wird als Stagingbereich für die Quelldaten vor dem Laden in SQL Data Warehouse verwendet.</span><span class="sxs-lookup"><span data-stu-id="cc8f5-127">Blob storage is used as a staging area for the source data before loading it into SQL Data Warehouse.</span></span>

<span data-ttu-id="cc8f5-128">**Azure SQL Data Warehouse**.</span><span class="sxs-lookup"><span data-stu-id="cc8f5-128">**Azure SQL Data Warehouse**.</span></span> <span data-ttu-id="cc8f5-129">[SQL Data Warehouse](/azure/sql-data-warehouse/) ist ein verteiltes System für die Analyse großer Datenmengen.</span><span class="sxs-lookup"><span data-stu-id="cc8f5-129">[SQL Data Warehouse](/azure/sql-data-warehouse/) is a distributed system designed to perform analytics on large data.</span></span> <span data-ttu-id="cc8f5-130">Es unterstützt massive Parallelverarbeitung (Massive Parallel Processing, MPP), die die Ausführung von Hochleistungsanalysen ermöglicht.</span><span class="sxs-lookup"><span data-stu-id="cc8f5-130">It supports massive parallel processing (MPP), which makes it suitable for running high-performance analytics.</span></span> 

<span data-ttu-id="cc8f5-131">**Azure Data Factory**</span><span class="sxs-lookup"><span data-stu-id="cc8f5-131">**Azure Data Factory**.</span></span> <span data-ttu-id="cc8f5-132">[Data Factory][adf] ist ein verwalteter Dienst, der Datenverschiebung und Datentransformation orchestriert und automatisiert.</span><span class="sxs-lookup"><span data-stu-id="cc8f5-132">[Data Factory][adf] is a managed service that orchestrates and automates data movement and data transformation.</span></span> <span data-ttu-id="cc8f5-133">In dieser Architektur koordiniert er die verschiedenen Phasen des ELT-Prozesses.</span><span class="sxs-lookup"><span data-stu-id="cc8f5-133">In this architecture, it coordinates the various stages of the ELT process.</span></span>

### <a name="analysis-and-reporting"></a><span data-ttu-id="cc8f5-134">Analysen und Berichte</span><span class="sxs-lookup"><span data-stu-id="cc8f5-134">Analysis and reporting</span></span>

<span data-ttu-id="cc8f5-135">**Azure Analysis Services**:</span><span class="sxs-lookup"><span data-stu-id="cc8f5-135">**Azure Analysis Services**.</span></span> <span data-ttu-id="cc8f5-136">[Analysis Services](/azure/analysis-services/) ist ein vollständig verwalteter Dienst, der Datenmodellierungsfunktionen ermöglicht.</span><span class="sxs-lookup"><span data-stu-id="cc8f5-136">[Analysis Services](/azure/analysis-services/) is a fully managed service that provides data modeling capabilities.</span></span> <span data-ttu-id="cc8f5-137">Das semantische Modell wird in Analysis Services geladen.</span><span class="sxs-lookup"><span data-stu-id="cc8f5-137">The semantic model is loaded into Analysis Services.</span></span>

<span data-ttu-id="cc8f5-138">**Power BI**:</span><span class="sxs-lookup"><span data-stu-id="cc8f5-138">**Power BI**.</span></span> <span data-ttu-id="cc8f5-139">Power BI ist eine Suite aus Business Analytics-Tools zum Analysieren von Daten für Einblicke in Geschäftsvorgänge.</span><span class="sxs-lookup"><span data-stu-id="cc8f5-139">Power BI is a suite of business analytics tools to analyze data for business insights.</span></span> <span data-ttu-id="cc8f5-140">In dieser Architektur dient sie zum Abfragen des in Analysis Services gespeicherten semantischen Modells.</span><span class="sxs-lookup"><span data-stu-id="cc8f5-140">In this architecture, it queries the semantic model stored in Analysis Services.</span></span>

### <a name="authentication"></a><span data-ttu-id="cc8f5-141">Authentifizierung</span><span class="sxs-lookup"><span data-stu-id="cc8f5-141">Authentication</span></span>

<span data-ttu-id="cc8f5-142">**Azure Active Directory** (Azure AD) authentifiziert Benutzer, die über Power BI eine Verbindung mit dem Analysis Services-Server herstellen.</span><span class="sxs-lookup"><span data-stu-id="cc8f5-142">**Azure Active Directory** (Azure AD) authenticates users who connect to the Analysis Services server through Power BI.</span></span>

<span data-ttu-id="cc8f5-143">Data Factory auch Azure AD für die Authentifizierung bei SQL Data Warehouse nutzen, indem ein Dienstprinzipal oder eine verwaltete Dienstidentität (MSI) verwendet wird.</span><span class="sxs-lookup"><span data-stu-id="cc8f5-143">Data Factory can use also use Azure AD to authenticate to SQL Data Warehouse, by using a service principal or Managed Service Identity (MSI).</span></span> <span data-ttu-id="cc8f5-144">Der Einfachheit halber verwendet die Beispielbereitstellung die SQL Server-Authentifizierung.</span><span class="sxs-lookup"><span data-stu-id="cc8f5-144">For simplicity, the example deployment uses SQL Server authentication.</span></span>

## <a name="data-pipeline"></a><span data-ttu-id="cc8f5-145">Datenpipeline</span><span class="sxs-lookup"><span data-stu-id="cc8f5-145">Data pipeline</span></span>

<span data-ttu-id="cc8f5-146">In [Azure Data Factory][adf] ist eine Pipeline eine logische Gruppierung von Aktivitäten zum Koordinieren einer Aufgabe &mdash; in diesem Fall das Laden und Transformieren von Daten in SQL Data Warehouse.</span><span class="sxs-lookup"><span data-stu-id="cc8f5-146">In [Azure Data Factory][adf], a pipeline is a logical grouping of activities used to coordinate a task &mdash; in this case, loading and transforming data into SQL Data Warehouse.</span></span> 

<span data-ttu-id="cc8f5-147">Diese Referenzarchitektur definiert eine Masterpipeline, die eine Sequenz von untergeordneten Pipelines ausführt.</span><span class="sxs-lookup"><span data-stu-id="cc8f5-147">This reference architecture defines a master pipeline that runs a sequence of child pipelines.</span></span> <span data-ttu-id="cc8f5-148">Jede untergeordnete Pipeline lädt Daten in eine oder mehrere Data Warehouse-Tabellen.</span><span class="sxs-lookup"><span data-stu-id="cc8f5-148">Each child pipeline loads data into one or more data warehouse tables.</span></span>

![](./images/adf-pipeline.png)

## <a name="incremental-loading"></a><span data-ttu-id="cc8f5-149">Inkrementelles Laden</span><span class="sxs-lookup"><span data-stu-id="cc8f5-149">Incremental loading</span></span>

<span data-ttu-id="cc8f5-150">Beim Ausführen eines automatisierten ETL-oder ELT-Prozesses ist es am effizientesten, nur die Daten zu laden, die sich seit der vorhergehenden Ausführung geändert haben.</span><span class="sxs-lookup"><span data-stu-id="cc8f5-150">When you run an automated ETL or ELT process, it's most efficient to load only the data that changed since the previous run.</span></span> <span data-ttu-id="cc8f5-151">Dies wird als *inkrementeller Ladevorgang* bezeichnet, im Gegensatz zu einem vollständige Ladevorgang, bei dem alle Daten geladen werden.</span><span class="sxs-lookup"><span data-stu-id="cc8f5-151">This is called an *incremental load*, as opposed to a full load that loads all of the data.</span></span> <span data-ttu-id="cc8f5-152">Zum Ausführen eines inkrementellen Ladevorgangs benötigen Sie eine Möglichkeit zum Identifizieren, welche Daten sich geändert haben.</span><span class="sxs-lookup"><span data-stu-id="cc8f5-152">To perform an incremental load, you need a way to identify which data has changed.</span></span> <span data-ttu-id="cc8f5-153">Der gängigste Ansatz ist die Verwendung eines Werts zum *Erkennen von Änderungen mit oberem Grenzwert*, d.h. der aktuelle Wert einer Spalte in der Quelltabelle – entweder einer datetime-Spalte oder einer eindeutigen Integer-Spalte – wird nachverfolgt.</span><span class="sxs-lookup"><span data-stu-id="cc8f5-153">The most common approach is to use a *high water mark* value, which means tracking the latest value of some column in the source table, either a datetime column or a unique integer column.</span></span> 

<span data-ttu-id="cc8f5-154">Ab SQL Server 2016 können Sie [temporale Tabellen](/sql/relational-databases/tables/temporal-tables) verwenden.</span><span class="sxs-lookup"><span data-stu-id="cc8f5-154">Starting with SQL Server 2016, you can use [temporal tables](/sql/relational-databases/tables/temporal-tables).</span></span> <span data-ttu-id="cc8f5-155">Hierbei handelt es sich um Tabellen mit Systemversionsverwaltung, die einen vollständigen Verlauf aller Datenänderungen beibehalten.</span><span class="sxs-lookup"><span data-stu-id="cc8f5-155">These are system-versioned tables that keep a full history of data changes.</span></span> <span data-ttu-id="cc8f5-156">Die Datenbank-Engine zeichnet den Verlauf aller Änderungen automatisch in einer separaten Verlaufstabelle auf.</span><span class="sxs-lookup"><span data-stu-id="cc8f5-156">The database engine automatically records the history of every change in a separate history table.</span></span> <span data-ttu-id="cc8f5-157">Sie können die Verlaufsdaten abfragen, indem Sie eine FOR SYSTEM_TIME-Klausel an eine Abfrage anhängen.</span><span class="sxs-lookup"><span data-stu-id="cc8f5-157">You can query the historical data by adding a FOR SYSTEM_TIME clause to a query.</span></span> <span data-ttu-id="cc8f5-158">Intern fragt die Datenbank-Engine die Verlaufstabelle ab, dies ist für die Anwendung jedoch transparent.</span><span class="sxs-lookup"><span data-stu-id="cc8f5-158">Internally, the database engine queries the history table, but this is transparent to the application.</span></span> 

> [!NOTE]
> <span data-ttu-id="cc8f5-159">Für frühere Versionen von SQL Server können Sie [Change Data Capture](/sql/relational-databases/track-changes/about-change-data-capture-sql-server) (CDC) verwenden.</span><span class="sxs-lookup"><span data-stu-id="cc8f5-159">For earlier versions of SQL Server, you can use [Change Data Capture](/sql/relational-databases/track-changes/about-change-data-capture-sql-server) (CDC).</span></span> <span data-ttu-id="cc8f5-160">Dieser Ansatz ist weniger geeignet als temporale Tabellen, da Sie eine separate Änderungstabelle abfragen müssen, und Änderungen werden anhand einer Protokollfolgenummer anstatt eines Zeitstempels nachverfolgt.</span><span class="sxs-lookup"><span data-stu-id="cc8f5-160">This approach is less convenient than temporal tables, because you have to query a separate change table, and changes are tracked by a log sequence number, rather than a timestamp.</span></span> 

<span data-ttu-id="cc8f5-161">Temporale Tabellen sind hilfreich für Dimensionsdaten, die sich im Laufe der Zeit ändern können.</span><span class="sxs-lookup"><span data-stu-id="cc8f5-161">Temporal tables are useful for dimension data, which can change over time.</span></span> <span data-ttu-id="cc8f5-162">Faktentabellen stellen in der Regel eine unveränderliche Transaktion wie einen Verkauf dar, und in diesem Fall ist das Beibehalten des Systemversionsverlaufs nicht sinnvoll.</span><span class="sxs-lookup"><span data-stu-id="cc8f5-162">Fact tables usually represent an immutable transaction such as a sale, in which case keeping the system version history doesn't make sense.</span></span> <span data-ttu-id="cc8f5-163">Stattdessen weisen Transaktionen normalerweise eine Spalte auf, die das Transaktionsdatum darstellt, das als Wasserzeichenwert verwendet werden kann.</span><span class="sxs-lookup"><span data-stu-id="cc8f5-163">Instead, transactions usually have a column that represents the transaction date, which can be used as the watermark value.</span></span> <span data-ttu-id="cc8f5-164">Beispielsweise enthalten in der OLTP-Datenbank von Wide World Importers die Tabellen „Sales.Invoices“ und „Sales.InvoiceLines“ das Feld `LastEditedWhen`, dessen Standardwert `sysdatetime()` ist.</span><span class="sxs-lookup"><span data-stu-id="cc8f5-164">For example, in the Wide World Importers OLTP database, the Sales.Invoices and Sales.InvoiceLines tables have a `LastEditedWhen` field that defaults to `sysdatetime()`.</span></span> 

<span data-ttu-id="cc8f5-165">Hier ist der allgemeine Ablauf für die ELT-Pipeline:</span><span class="sxs-lookup"><span data-stu-id="cc8f5-165">Here is the general flow for the ELT pipeline:</span></span>

1. <span data-ttu-id="cc8f5-166">Verfolgen Sie für jede Tabelle in der Quelldatenbank den Trennzeitpunkt der Ausführung des letzten ELT-Auftrags nach.</span><span class="sxs-lookup"><span data-stu-id="cc8f5-166">For each table in the source database, track the cutoff time when the last ELT job ran.</span></span> <span data-ttu-id="cc8f5-167">Speichern Sie diese Informationen im Data Warehouse.</span><span class="sxs-lookup"><span data-stu-id="cc8f5-167">Store this information in the data warehouse.</span></span> <span data-ttu-id="cc8f5-168">(Bei der Ersteinrichtung sind alle Zeiten auf „1.1.1900“ festgelegt.)</span><span class="sxs-lookup"><span data-stu-id="cc8f5-168">(On initial setup, all times are set to '1-1-1900'.)</span></span>

2. <span data-ttu-id="cc8f5-169">Während des Datenexportschritts wird der Trennzeitpunkt als Parameter an einen Satz von gespeicherten Prozeduren in der Quelldatenbank übergeben.</span><span class="sxs-lookup"><span data-stu-id="cc8f5-169">During the data export step, the cutoff time is passed as a parameter to a set of stored procedures in the source database.</span></span> <span data-ttu-id="cc8f5-170">Diese gespeicherten Prozeduren fragen alle Datensätze ab, die nach dem Trennzeitpunkt geändert oder erstellt wurden.</span><span class="sxs-lookup"><span data-stu-id="cc8f5-170">These stored procedures query for any records that were changed or created after the cutoff time.</span></span> <span data-ttu-id="cc8f5-171">Für die Sales-Faktentabelle wird die Spalte `LastEditedWhen` verwendet.</span><span class="sxs-lookup"><span data-stu-id="cc8f5-171">For the Sales fact table, the `LastEditedWhen` column is used.</span></span> <span data-ttu-id="cc8f5-172">Für die Dimensionsdaten werden temporale Tabellen mit Systemversionsverwaltung verwendet.</span><span class="sxs-lookup"><span data-stu-id="cc8f5-172">For the dimension data, system-versioned temporal tables are used.</span></span>

3. <span data-ttu-id="cc8f5-173">Wenn die Datenmigration abgeschlossen ist, aktualisieren Sie die Tabelle, in der die Trennzeitpunkte gespeichert werden.</span><span class="sxs-lookup"><span data-stu-id="cc8f5-173">When the data migration is complete, update the table that stores the cutoff times.</span></span>

<span data-ttu-id="cc8f5-174">Es ist auch hilfreich, eine *Herkunft* für jede ELT-Ausführung aufzuzeichnen.</span><span class="sxs-lookup"><span data-stu-id="cc8f5-174">It's also useful to record a *lineage* for each ELT run.</span></span> <span data-ttu-id="cc8f5-175">Für einen bestimmten Datensatz ordnet die Herkunft diesen Datensatz der ELT-Ausführung zu, bei der die Daten erzeugt wurden.</span><span class="sxs-lookup"><span data-stu-id="cc8f5-175">For a given record, the lineage associates that record with the ELT run that produced the data.</span></span> <span data-ttu-id="cc8f5-176">Bei jeder ETL-Ausführung wird für jede Tabelle ein neuer Herkunftsdatensatz erstellt, der die Start- und Endladezeiten anzeigt.</span><span class="sxs-lookup"><span data-stu-id="cc8f5-176">For each ETL run, a new lineage record is created for every table, showing the starting and ending load times.</span></span> <span data-ttu-id="cc8f5-177">Die Herkunftsschlüssel für die einzelnen Datensätze werden in den Dimensions- und Faktentabellen gespeichert.</span><span class="sxs-lookup"><span data-stu-id="cc8f5-177">The lineage keys for each record are stored in the dimension and fact tables.</span></span>

![](./images/city-dimension-table.png)

<span data-ttu-id="cc8f5-178">Aktualisieren Sie das Analysis Services-Tabellenmodell, nachdem ein neuer Datenbatch in das Warehouse geladen wurde.</span><span class="sxs-lookup"><span data-stu-id="cc8f5-178">After a new batch of data is loaded into the warehouse, refresh the Analysis Services tabular model.</span></span> <span data-ttu-id="cc8f5-179">Siehe [Asynchrones Aktualisieren mit der REST-API](/azure/analysis-services/analysis-services-async-refresh).</span><span class="sxs-lookup"><span data-stu-id="cc8f5-179">See [Asynchronous refresh with the REST API](/azure/analysis-services/analysis-services-async-refresh).</span></span>

## <a name="data-cleansing"></a><span data-ttu-id="cc8f5-180">Datenbereinigung</span><span class="sxs-lookup"><span data-stu-id="cc8f5-180">Data cleansing</span></span>

<span data-ttu-id="cc8f5-181">Die Datenbereinigung sollte Teil des ELT-Prozesses sein.</span><span class="sxs-lookup"><span data-stu-id="cc8f5-181">Data cleansing should be part of the ELT process.</span></span> <span data-ttu-id="cc8f5-182">In dieser Referenzarchitektur ist die Tabelle mit der Stadtbevölkerung eine Quelle ungültiger Daten, in der einige Städte keine Bevölkerung aufweisen, da möglicherweise keine Daten verfügbar waren.</span><span class="sxs-lookup"><span data-stu-id="cc8f5-182">In this reference architecture, one source of bad data is the city population table, where some cities have zero population, perhaps because no data was available.</span></span> <span data-ttu-id="cc8f5-183">Während der Verarbeitung entfernt die ELT-Pipeline diese Städte aus der Tabelle mit der Stadtbevölkerung.</span><span class="sxs-lookup"><span data-stu-id="cc8f5-183">During processing, the ELT pipeline removes those cities from the city population table.</span></span> <span data-ttu-id="cc8f5-184">Führen Sie die Datenbereinigung in Stagingtabellen statt in externen Tabellen durch.</span><span class="sxs-lookup"><span data-stu-id="cc8f5-184">Perform data cleansing on staging tables, rather than external tables.</span></span>

<span data-ttu-id="cc8f5-185">Hier ist die gespeicherte Prozedur, die die Städte ohne Bevölkerung der Tabelle mit der Stadtbevölkerung entfernt.</span><span class="sxs-lookup"><span data-stu-id="cc8f5-185">Here is the stored procedure that removes the cities with zero population from the City Population table.</span></span> <span data-ttu-id="cc8f5-186">(Sie finden die Quelldatei [hier](https://github.com/mspnp/reference-architectures/blob/master/data/enterprise_bi_sqldw_advanced/azure/sqldw_scripts/citypopulation/%5BIntegration%5D.%5BMigrateExternalCityPopulationData%5D.sql).)</span><span class="sxs-lookup"><span data-stu-id="cc8f5-186">(You can find the source file [here](https://github.com/mspnp/reference-architectures/blob/master/data/enterprise_bi_sqldw_advanced/azure/sqldw_scripts/citypopulation/%5BIntegration%5D.%5BMigrateExternalCityPopulationData%5D.sql).)</span></span> 

```sql
DELETE FROM [Integration].[CityPopulation_Staging]
WHERE RowNumber in (SELECT DISTINCT RowNumber
FROM [Integration].[CityPopulation_Staging]
WHERE POPULATION = 0
GROUP BY RowNumber
HAVING COUNT(RowNumber) = 4)
```

## <a name="external-data-sources"></a><span data-ttu-id="cc8f5-187">Externe Datenquellen</span><span class="sxs-lookup"><span data-stu-id="cc8f5-187">External data sources</span></span>

<span data-ttu-id="cc8f5-188">Data Warehouses fassen häufig Daten aus mehreren Quellen zusammen.</span><span class="sxs-lookup"><span data-stu-id="cc8f5-188">Data warehouses often consolidate data from multiple sources.</span></span> <span data-ttu-id="cc8f5-189">Diese Referenzarchitektur lädt eine externe Datenquelle, die demografische Daten enthält.</span><span class="sxs-lookup"><span data-stu-id="cc8f5-189">This reference architecture loads an external data source that contains demographics data.</span></span> <span data-ttu-id="cc8f5-190">Dieses Dataset in Azure Blob Storage als Teil des Beispiels [WorldWideImportersDW](https://github.com/Microsoft/sql-server-samples/tree/master/samples/databases/wide-world-importers/sample-scripts/polybase) verfügbar.</span><span class="sxs-lookup"><span data-stu-id="cc8f5-190">This dataset is available in Azure blob storage as part of the [WorldWideImportersDW](https://github.com/Microsoft/sql-server-samples/tree/master/samples/databases/wide-world-importers/sample-scripts/polybase) sample.</span></span>

<span data-ttu-id="cc8f5-191">Azure Data Factory kann mithilfe des [Blob Storage-Connectors](/azure/data-factory/connector-azure-blob-storage) direkt aus Blob Storage kopieren.</span><span class="sxs-lookup"><span data-stu-id="cc8f5-191">Azure Data Factory can copy directly from blob storage, using the [blob storage connector](/azure/data-factory/connector-azure-blob-storage).</span></span> <span data-ttu-id="cc8f5-192">Der Connector erfordert jedoch eine Verbindungszeichenfolge oder eine Shared Access Signature, damit er nicht zum Kopieren eines Blobs mit öffentlichem Lesezugriff verwendet werden kann.</span><span class="sxs-lookup"><span data-stu-id="cc8f5-192">However, the connector requires a connection string or a shared access signature, so it can't be used to copy a blob with public read access.</span></span> <span data-ttu-id="cc8f5-193">Um dieses Problem zu umgehen, können Sie PolyBase zum Erstellen einer externen Tabelle über Blob Storage verwenden und die externen Tabellen dann in SQL Data Warehouse kopieren.</span><span class="sxs-lookup"><span data-stu-id="cc8f5-193">As a workaround, you can use PolyBase to create an external table over Blob storage and then copy the external tables into SQL Data Warehouse.</span></span> 

## <a name="handling-large-binary-data"></a><span data-ttu-id="cc8f5-194">Verarbeiten von umfangreichen Binärdaten</span><span class="sxs-lookup"><span data-stu-id="cc8f5-194">Handling large binary data</span></span> 

<span data-ttu-id="cc8f5-195">In der Quelldatenbank weist die Tabelle mit Städten die Spalte „Ort“ auf, die den räumlichen Datentyp [Geografie](/sql/t-sql/spatial-geography/spatial-types-geography) enthält.</span><span class="sxs-lookup"><span data-stu-id="cc8f5-195">In the source database, the Cities table has a Location column that holds a [geography](/sql/t-sql/spatial-geography/spatial-types-geography) spatial data type.</span></span> <span data-ttu-id="cc8f5-196">SQL Data Warehouse unterstützt den Typ **Geografie** nicht systemintern, daher wird dieses Feld während des Ladens in einen **varbinary**-Typ konvertiert.</span><span class="sxs-lookup"><span data-stu-id="cc8f5-196">SQL Data Warehouse doesn't support the **geography** type natively, so this field is converted to a **varbinary** type during loading.</span></span> <span data-ttu-id="cc8f5-197">(Siehe [Verwenden von Problemumgehungen für nicht unterstützte Datentypen](/azure/sql-data-warehouse/sql-data-warehouse-tables-data-types#unsupported-data-types).)</span><span class="sxs-lookup"><span data-stu-id="cc8f5-197">(See [Workarounds for unsupported data types](/azure/sql-data-warehouse/sql-data-warehouse-tables-data-types#unsupported-data-types).)</span></span>

<span data-ttu-id="cc8f5-198">PolyBase unterstützt jedoch eine maximale Spaltengröße von `varbinary(8000)`, was bedeutet, dass einige Daten möglicherweise abgeschnitten werden.</span><span class="sxs-lookup"><span data-stu-id="cc8f5-198">However, PolyBase supports a maximum column size of `varbinary(8000)`, which means some data could be truncated.</span></span> <span data-ttu-id="cc8f5-199">Dieses Problem lässt sich umgehen, indem die Daten während des Exports wie folgt in Blöcke aufgeteilt und dann wieder zusammengefügt werden:</span><span class="sxs-lookup"><span data-stu-id="cc8f5-199">A workaround for this problem is to break the data up into chunks during export, and then reassemble the chunks, as follows:</span></span>

1. <span data-ttu-id="cc8f5-200">Erstellen Sie eine temporäre Stagingtabelle für die Standort-Spalte.</span><span class="sxs-lookup"><span data-stu-id="cc8f5-200">Create a temporary staging table for the Location column.</span></span>

2. <span data-ttu-id="cc8f5-201">Teilen Sie die Standortdaten für jede Stadt in Blöcke von 8000 Bytes auf. Dies ergibt 1 &ndash; N Zeilen für jede Stadt.</span><span class="sxs-lookup"><span data-stu-id="cc8f5-201">For each city, split the location data into 8000-byte chunks, resulting in 1 &ndash; N rows for each city.</span></span>

3. <span data-ttu-id="cc8f5-202">Um die Blöcke wieder zusammenzusetzen, verwenden Sie den T-SQL-[PIVOT](/sql/t-sql/queries/from-using-pivot-and-unpivot)-Operator, um Zeilen in Spalten umwandeln und dann die Spaltenwerte für jede Stadt zu verketten.</span><span class="sxs-lookup"><span data-stu-id="cc8f5-202">To reassemble the chunks, use the T-SQL [PIVOT](/sql/t-sql/queries/from-using-pivot-and-unpivot) operator to convert rows into columns and then concatenate the column values for each city.</span></span>

<span data-ttu-id="cc8f5-203">Die Herausforderung besteht darin, dass jede Stadt je nach Größe der geografischen Daten in eine unterschiedliche Anzahl von Zeilen aufgeteilt wird.</span><span class="sxs-lookup"><span data-stu-id="cc8f5-203">The challenge is that each city will be split into a different number of rows, depending on the size of geography data.</span></span> <span data-ttu-id="cc8f5-204">Damit der PIVOT-Operator funktioniert, muss jede Stadt die gleiche Anzahl von Zeilen aufweisen.</span><span class="sxs-lookup"><span data-stu-id="cc8f5-204">For the PIVOT operator to work, every city must have the same number of rows.</span></span> <span data-ttu-id="cc8f5-205">Dazu wendet die T-SQL-Abfrage (die Sie [hier][MergeLocation] anzeigen können) einige Tricks aus, um die Zeilen mit leeren Werten aufzufüllen, sodass jede Stadt nach dem Pivot-Vorgang die gleiche Anzahl von Spalten aufweist.</span><span class="sxs-lookup"><span data-stu-id="cc8f5-205">To make this work, the T-SQL query (which you can view [here][MergeLocation]) does some tricks to pad out the rows with blank values, so that every city has the same number of columns after the pivot.</span></span> <span data-ttu-id="cc8f5-206">Die resultierende Abfrage erweist sich als wesentlich schneller als Durchlaufen der einzelnen Zeilen.</span><span class="sxs-lookup"><span data-stu-id="cc8f5-206">The resulting query turns out to be much faster than looping through the rows one at a time.</span></span>

<span data-ttu-id="cc8f5-207">Der gleiche Ansatz wird für Bilddaten verwendet.</span><span class="sxs-lookup"><span data-stu-id="cc8f5-207">The same approach is used for image data.</span></span>

## <a name="slowly-changing-dimensions"></a><span data-ttu-id="cc8f5-208">Langsam veränderliche Dimensionen</span><span class="sxs-lookup"><span data-stu-id="cc8f5-208">Slowly changing dimensions</span></span>

<span data-ttu-id="cc8f5-209">Dimensionsdaten sind relativ statisch, können sich jedoch ändern.</span><span class="sxs-lookup"><span data-stu-id="cc8f5-209">Dimension data is relatively static, but it can change.</span></span> <span data-ttu-id="cc8f5-210">Beispielsweise kann ein Produkt einer anderen Produktkategorie zugewiesen werden.</span><span class="sxs-lookup"><span data-stu-id="cc8f5-210">For example, a product might get reassigned to a different product category.</span></span> <span data-ttu-id="cc8f5-211">Es gibt verschiedene Ansätze zur Handhabung von langsam veränderlichen Dimensionen.</span><span class="sxs-lookup"><span data-stu-id="cc8f5-211">There are several approaches to handling slowly changing dimensions.</span></span> <span data-ttu-id="cc8f5-212">Ein gängiges Verfahren mit der Bezeichnung [Typ 2](https://wikipedia.org/wiki/Slowly_changing_dimension#Type_2:_add_new_row) ist das Hinzufügen eines neuen Datensatzes bei jeder Dimensionsänderung.</span><span class="sxs-lookup"><span data-stu-id="cc8f5-212">A common technique, called [Type 2](https://wikipedia.org/wiki/Slowly_changing_dimension#Type_2:_add_new_row), is to add a new record whenever a dimension changes.</span></span> 

<span data-ttu-id="cc8f5-213">Zur Umsetzung des Typ 2-Ansatzes erfordern Dimensionstabellen zusätzliche Spalten, die den effektiven Datumsbereich für einen bestimmten Datensatz angeben.</span><span class="sxs-lookup"><span data-stu-id="cc8f5-213">In order to implement the Type 2 approach, dimension tables need additional columns that specify the effective date range for a given record.</span></span> <span data-ttu-id="cc8f5-214">Außerdem werden Primärschlüssel aus der Quelldatenbank dupliziert, daher muss die Dimensionstabelle über einen künstlichen Primärschlüssel verfügen.</span><span class="sxs-lookup"><span data-stu-id="cc8f5-214">Also, primary keys from the source database will be duplicated, so the dimension table must have an artificial primary key.</span></span>

<span data-ttu-id="cc8f5-215">Die folgende Abbildung zeigt die Tabelle „Dimension.City“.</span><span class="sxs-lookup"><span data-stu-id="cc8f5-215">The following image shows the Dimension.City table.</span></span> <span data-ttu-id="cc8f5-216">Die Spalte `WWI City ID` ist der Primärschlüssel aus der Quelldatenbank.</span><span class="sxs-lookup"><span data-stu-id="cc8f5-216">The `WWI City ID` column is the primary key from the source database.</span></span> <span data-ttu-id="cc8f5-217">Die Spalte `City Key` ist ein künstlicher Schlüssel, der während der Verarbeitung der ETL-Pipeline generiert wurde.</span><span class="sxs-lookup"><span data-stu-id="cc8f5-217">The `City Key` column is an artificial key generated during the ETL pipeline.</span></span> <span data-ttu-id="cc8f5-218">Beachten Sie auch, dass in der Tabelle die Spalten `Valid From` und `Valid To` vorhanden sind, die den Bereich definieren, in dem die einzelnen Zeilen gültig waren.</span><span class="sxs-lookup"><span data-stu-id="cc8f5-218">Also notice that the table has `Valid From` and `Valid To` columns, which define the range when each row was valid.</span></span> <span data-ttu-id="cc8f5-219">Das `Valid To`-Element aktueller Werte entspricht „9999-12-31“.</span><span class="sxs-lookup"><span data-stu-id="cc8f5-219">Current values have a `Valid To` equal to '9999-12-31'.</span></span>

![](./images/city-dimension-table.png)

<span data-ttu-id="cc8f5-220">Der Vorteil dieses Ansatzes ist, dass historische Daten beibehalten werden, die für die Analyse nützlich sein können.</span><span class="sxs-lookup"><span data-stu-id="cc8f5-220">The advantage of this approach is that it preserves historical data, which can be valuable for analysis.</span></span> <span data-ttu-id="cc8f5-221">Allerdings sind dabei auch mehrere Zeilen für dieselbe Entität vorhanden.</span><span class="sxs-lookup"><span data-stu-id="cc8f5-221">However, it also means there will be multiple rows for the same entity.</span></span> <span data-ttu-id="cc8f5-222">Hier sehen Sie beispielsweise Datensätze, die `WWI City ID` = 28561 entsprechen:</span><span class="sxs-lookup"><span data-stu-id="cc8f5-222">For example, here are the records that match `WWI City ID` = 28561:</span></span>

![](./images/city-dimension-table-2.png)

<span data-ttu-id="cc8f5-223">Ordnen Sie jeden Sales-Fakt einer einzelnen Zeile in der Tabelle „Dimension.City“ zu, die dem Rechnungsdatum entspricht.</span><span class="sxs-lookup"><span data-stu-id="cc8f5-223">For each Sales fact, you want to associate that fact with a single row in City dimension table, corresponding to the invoice date.</span></span> <span data-ttu-id="cc8f5-224">Erstellen Sie als Teil des ETL-Prozesses eine weitere Spalte, die</span><span class="sxs-lookup"><span data-stu-id="cc8f5-224">As part of the ETL process, create an additional column that</span></span> 

<span data-ttu-id="cc8f5-225">Die folgende T-SQL-Abfrage erstellt eine temporäre Tabelle, die jede Rechnung dem richtigen City-Schlüssel aus der Tabelle „Dimension.City“ zuordnet.</span><span class="sxs-lookup"><span data-stu-id="cc8f5-225">The following T-SQL query creates a temporary table that associates each invoice with the correct City Key from the City dimension table.</span></span>

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

<span data-ttu-id="cc8f5-226">Diese Tabelle wird verwendet, um eine Spalte in der Sales-Faktentabelle auszufüllen:</span><span class="sxs-lookup"><span data-stu-id="cc8f5-226">This table is used to populate a column in the Sales fact table:</span></span>

```sql
UPDATE [Integration].[Sale_Staging]
SET [Integration].[Sale_Staging].[WWI Customer ID] =  CustomerHolder.[WWI Customer ID]
```

<span data-ttu-id="cc8f5-227">Diese Spalte ermöglicht, dass eine Power BI-Abfrage den richtigen City-Datensatz für eine bestimmte Verkaufsrechnung findet.</span><span class="sxs-lookup"><span data-stu-id="cc8f5-227">This column enables a Power BI query to find the correct City record for a given sales invoice.</span></span>

## <a name="security-considerations"></a><span data-ttu-id="cc8f5-228">Sicherheitshinweise</span><span class="sxs-lookup"><span data-stu-id="cc8f5-228">Security considerations</span></span>

<span data-ttu-id="cc8f5-229">Für höhere Sicherheit können Sie [Virtual Network-Dienstendpunkte](/azure/virtual-network/virtual-network-service-endpoints-overview) zum Schützen von Azure-Dienstressourcen verwenden, indem diese ausschließlich auf Ihr virtuelles Netzwerk beschränkt sind.</span><span class="sxs-lookup"><span data-stu-id="cc8f5-229">For additional security, you can use [Virtual Network service endpoints](/azure/virtual-network/virtual-network-service-endpoints-overview) to secure Azure service resources to only your virtual network.</span></span> <span data-ttu-id="cc8f5-230">Der öffentliche Internetzugriff auf diese Ressourcen wird dadurch vollständig entfernt, sodass nur Datenverkehr aus Ihrem virtuellen Netzwerk zulässig ist.</span><span class="sxs-lookup"><span data-stu-id="cc8f5-230">This fully removes public Internet access to those resources, allowing traffic only from your virtual network.</span></span>

<span data-ttu-id="cc8f5-231">Mit diesem Ansatz erstellen Sie ein VNET in Azure und dann private Dienstendpunkte für Azure-Dienste.</span><span class="sxs-lookup"><span data-stu-id="cc8f5-231">With this approach, you create a VNet in Azure and then create private service endpoints for Azure services.</span></span> <span data-ttu-id="cc8f5-232">Diese Dienste sind dann auf Datenverkehr aus diesem virtuellen Netzwerk beschränkt.</span><span class="sxs-lookup"><span data-stu-id="cc8f5-232">Those services are then restricted to traffic from that virtual network.</span></span> <span data-ttu-id="cc8f5-233">Sie können auch über ein Gateway aus Ihrem lokalen Netzwerk darauf zugreifen.</span><span class="sxs-lookup"><span data-stu-id="cc8f5-233">You can also reach them from your on-premises network through a gateway.</span></span>

<span data-ttu-id="cc8f5-234">Bedenken Sie dabei folgende Einschränkungen:</span><span class="sxs-lookup"><span data-stu-id="cc8f5-234">Be aware of the following limitations:</span></span>

- <span data-ttu-id="cc8f5-235">Zum Zeitpunkt der Erstellung dieser Referenzarchitektur werden VNET-Dienstendpunkte für Azure Storage und Azure SQL Data Warehouse, aber nicht für Azure Analysis Services unterstützt.</span><span class="sxs-lookup"><span data-stu-id="cc8f5-235">At the time this reference architecture was created, VNet service endpoints are supported for Azure Storage and Azure SQL Data Warehouse, but not for Azure Analysis Service.</span></span> <span data-ttu-id="cc8f5-236">Überprüfen Sie den aktuellen Status [hier](https://azure.microsoft.com/updates/?product=virtual-network).</span><span class="sxs-lookup"><span data-stu-id="cc8f5-236">Check the latest status [here](https://azure.microsoft.com/updates/?product=virtual-network).</span></span> 

- <span data-ttu-id="cc8f5-237">Wenn Dienstendpunkte für Azure Storage aktiviert sind, kann PolyBase keine Daten aus Storage in SQL Data Warehouse kopieren.</span><span class="sxs-lookup"><span data-stu-id="cc8f5-237">If service endpoints are enabled for Azure Storage, PolyBase cannot copy data from Storage into SQL Data Warehouse.</span></span> <span data-ttu-id="cc8f5-238">Dieses Problem kann entschärft werden.</span><span class="sxs-lookup"><span data-stu-id="cc8f5-238">There is a mitigation for this issue.</span></span> <span data-ttu-id="cc8f5-239">Weitere Informationen finden Sie unter [Auswirkungen der Verwendung von VNET-Dienstendpunkten mit Azure Storage](/azure/sql-database/sql-database-vnet-service-endpoint-rule-overview?toc=%2fazure%2fvirtual-network%2ftoc.json#impact-of-using-vnet-service-endpoints-with-azure-storage).</span><span class="sxs-lookup"><span data-stu-id="cc8f5-239">For more information, see [Impact of using VNet Service Endpoints with Azure storage](/azure/sql-database/sql-database-vnet-service-endpoint-rule-overview?toc=%2fazure%2fvirtual-network%2ftoc.json#impact-of-using-vnet-service-endpoints-with-azure-storage).</span></span> 

- <span data-ttu-id="cc8f5-240">Zum Verschieben von Daten aus einem lokalen Speicher in Azure Storage müssen Sie öffentliche IP-Adressen in Ihrem lokalen Speicher oder ExpressRoute auf die Whitelist setzen.</span><span class="sxs-lookup"><span data-stu-id="cc8f5-240">To move data from on-premises into Azure Storage, you will need to whitelist public IP addresses from your on-premises or ExpressRoute.</span></span> <span data-ttu-id="cc8f5-241">Details finden Sie unter [Schützen von Azure-Diensten in virtuellen Netzwerken](/azure/virtual-network/virtual-network-service-endpoints-overview#securing-azure-services-to-virtual-networks).</span><span class="sxs-lookup"><span data-stu-id="cc8f5-241">For details, see [Securing Azure services to virtual networks](/azure/virtual-network/virtual-network-service-endpoints-overview#securing-azure-services-to-virtual-networks).</span></span>

- <span data-ttu-id="cc8f5-242">Stellen Sie im virtuellen Netzwerk einen virtuellen Windows-Computer bereit, der den SQL Data Warehouse-Dienstendpunkt enthält, damit Analysis Services Daten aus SQL Data Warehouse lesen kann.</span><span class="sxs-lookup"><span data-stu-id="cc8f5-242">To enable Analysis Services to read data from SQL Data Warehouse, deploy a Windows VM to the virtual network that contains the SQL Data Warehouse service endpoint.</span></span> <span data-ttu-id="cc8f5-243">Installieren Sie auf diesem virtuellen Computer ein [lokales Azure-Datengateway](/azure/analysis-services/analysis-services-gateway).</span><span class="sxs-lookup"><span data-stu-id="cc8f5-243">Install [Azure On-premises Data Gateway](/azure/analysis-services/analysis-services-gateway) on this VM.</span></span> <span data-ttu-id="cc8f5-244">Verbinden Sie dann Ihren Azure Analysis-Dienst, mit dem Datengateway.</span><span class="sxs-lookup"><span data-stu-id="cc8f5-244">Then connect your Azure Analysis service to the data gateway.</span></span>

## <a name="deploy-the-solution"></a><span data-ttu-id="cc8f5-245">Bereitstellen der Lösung</span><span class="sxs-lookup"><span data-stu-id="cc8f5-245">Deploy the solution</span></span>

<span data-ttu-id="cc8f5-246">Eine Bereitstellung für diese Referenzarchitektur ist auf [GitHub][ref-arch-repo-folder] verfügbar.</span><span class="sxs-lookup"><span data-stu-id="cc8f5-246">A deployment for this reference architecture is available on [GitHub][ref-arch-repo-folder].</span></span> <span data-ttu-id="cc8f5-247">Folgendes wird bereitgestellt:</span><span class="sxs-lookup"><span data-stu-id="cc8f5-247">It deploys the following:</span></span>

  * <span data-ttu-id="cc8f5-248">Eine Windows-VM, um einen lokalen Datenbankserver zu simulieren.</span><span class="sxs-lookup"><span data-stu-id="cc8f5-248">A Windows VM to simulate an on-premises database server.</span></span> <span data-ttu-id="cc8f5-249">Sie enthält SQL Server 2017 und zugehörige Tools zusammen mit Power BI Desktop.</span><span class="sxs-lookup"><span data-stu-id="cc8f5-249">It includes SQL Server 2017 and related tools, along with Power BI Desktop.</span></span>
  * <span data-ttu-id="cc8f5-250">Ein Azure Storage-Konto, das Blobspeicher zum Speichern von Daten bereitstellt, die aus SQL Server-Datenbank exportiert wurden.</span><span class="sxs-lookup"><span data-stu-id="cc8f5-250">An Azure storage account that provides Blob storage to hold data exported from the SQL Server database.</span></span>
  * <span data-ttu-id="cc8f5-251">Eine Instanz von Azure SQL Data Warehouse.</span><span class="sxs-lookup"><span data-stu-id="cc8f5-251">An Azure SQL Data Warehouse instance.</span></span>
  * <span data-ttu-id="cc8f5-252">Eine Azure Analysis Services-Instanz.</span><span class="sxs-lookup"><span data-stu-id="cc8f5-252">An Azure Analysis Services instance.</span></span>
  * <span data-ttu-id="cc8f5-253">Azure Data Factory und die Data Factory-Pipeline für den ELT-Auftrag.</span><span class="sxs-lookup"><span data-stu-id="cc8f5-253">Azure Data Factory and the Data Factory pipeline for the ELT job.</span></span>

### <a name="prerequisites"></a><span data-ttu-id="cc8f5-254">Voraussetzungen</span><span class="sxs-lookup"><span data-stu-id="cc8f5-254">Prerequisites</span></span>

[!INCLUDE [ref-arch-prerequisites.md](../../../includes/ref-arch-prerequisites.md)]

### <a name="variables"></a><span data-ttu-id="cc8f5-255">Variables</span><span class="sxs-lookup"><span data-stu-id="cc8f5-255">Variables</span></span>

<span data-ttu-id="cc8f5-256">Die folgenden Schritte enthalten einige benutzerdefinierte Variablen.</span><span class="sxs-lookup"><span data-stu-id="cc8f5-256">The steps that follow include some user-defined variables.</span></span> <span data-ttu-id="cc8f5-257">Sie müssen diese durch von Ihnen definierte Werte ersetzen.</span><span class="sxs-lookup"><span data-stu-id="cc8f5-257">You will need to replace these with values that you define.</span></span>

- <span data-ttu-id="cc8f5-258">`<data_factory_name>`.</span><span class="sxs-lookup"><span data-stu-id="cc8f5-258">`<data_factory_name>`.</span></span> <span data-ttu-id="cc8f5-259">Data Factory-Name.</span><span class="sxs-lookup"><span data-stu-id="cc8f5-259">Data Factory name.</span></span>
- <span data-ttu-id="cc8f5-260">`<analysis_server_name>`.</span><span class="sxs-lookup"><span data-stu-id="cc8f5-260">`<analysis_server_name>`.</span></span> <span data-ttu-id="cc8f5-261">Name des Analysis Services-Servers.</span><span class="sxs-lookup"><span data-stu-id="cc8f5-261">Analysis Services server name.</span></span>
- <span data-ttu-id="cc8f5-262">`<active_directory_upn>`.</span><span class="sxs-lookup"><span data-stu-id="cc8f5-262">`<active_directory_upn>`.</span></span> <span data-ttu-id="cc8f5-263">Ihr Azure Active Directory-Benutzerprinzipalname (User Principal Name, UPN).</span><span class="sxs-lookup"><span data-stu-id="cc8f5-263">Your Azure Active Directory user principal name (UPN).</span></span> <span data-ttu-id="cc8f5-264">Beispiel: `user@contoso.com`.</span><span class="sxs-lookup"><span data-stu-id="cc8f5-264">For example, `user@contoso.com`.</span></span>
- <span data-ttu-id="cc8f5-265">`<data_warehouse_server_name>`.</span><span class="sxs-lookup"><span data-stu-id="cc8f5-265">`<data_warehouse_server_name>`.</span></span> <span data-ttu-id="cc8f5-266">Name des SQL Data Warehouse-Servers.</span><span class="sxs-lookup"><span data-stu-id="cc8f5-266">SQL Data Warehouse server name.</span></span>
- <span data-ttu-id="cc8f5-267">`<data_warehouse_password>`.</span><span class="sxs-lookup"><span data-stu-id="cc8f5-267">`<data_warehouse_password>`.</span></span> <span data-ttu-id="cc8f5-268">SQL Data Warehouse-Administratorkennwort.</span><span class="sxs-lookup"><span data-stu-id="cc8f5-268">SQL Data Warehouse administrator password.</span></span>
- <span data-ttu-id="cc8f5-269">`<resource_group_name>`.</span><span class="sxs-lookup"><span data-stu-id="cc8f5-269">`<resource_group_name>`.</span></span> <span data-ttu-id="cc8f5-270">Der Name der Ressourcengruppe.</span><span class="sxs-lookup"><span data-stu-id="cc8f5-270">The name of the resource group.</span></span>
- <span data-ttu-id="cc8f5-271">`<region>`.</span><span class="sxs-lookup"><span data-stu-id="cc8f5-271">`<region>`.</span></span> <span data-ttu-id="cc8f5-272">Die Azure-Region, in der die Ressourcen bereitgestellt werden.</span><span class="sxs-lookup"><span data-stu-id="cc8f5-272">The Azure region where the resources will be deployed.</span></span>
- <span data-ttu-id="cc8f5-273">`<storage_account_name>`.</span><span class="sxs-lookup"><span data-stu-id="cc8f5-273">`<storage_account_name>`.</span></span> <span data-ttu-id="cc8f5-274">Name des Speicherkontos.</span><span class="sxs-lookup"><span data-stu-id="cc8f5-274">Storage account name.</span></span> <span data-ttu-id="cc8f5-275">Die [Benennungsregeln](../../best-practices/naming-conventions.md#naming-rules-and-restrictions) für Storage-Konten müssen eingehalten werden.</span><span class="sxs-lookup"><span data-stu-id="cc8f5-275">Must follow the [naming rules](../../best-practices/naming-conventions.md#naming-rules-and-restrictions) for Storage accounts.</span></span>
- <span data-ttu-id="cc8f5-276">`<sql-db-password>`.</span><span class="sxs-lookup"><span data-stu-id="cc8f5-276">`<sql-db-password>`.</span></span> <span data-ttu-id="cc8f5-277">Kennwort für die SQL Server-Anmeldung.</span><span class="sxs-lookup"><span data-stu-id="cc8f5-277">SQL Server login password.</span></span>

### <a name="deploy-azure-data-factory"></a><span data-ttu-id="cc8f5-278">Bereitstellen von Azure Data Factory</span><span class="sxs-lookup"><span data-stu-id="cc8f5-278">Deploy Azure Data Factory</span></span>

1. <span data-ttu-id="cc8f5-279">Navigieren Sie zum Ordner `data\enterprise_bi_sqldw_advanced\azure\templates` des [GitHub-Repositorys][ref-arch-repo].</span><span class="sxs-lookup"><span data-stu-id="cc8f5-279">Navigate to the `data\enterprise_bi_sqldw_advanced\azure\templates` folder of the [GitHub repository][ref-arch-repo].</span></span>

2. <span data-ttu-id="cc8f5-280">Führen Sie den folgenden Azure CLI-Befehl aus, um eine Ressourcengruppe zu erstellen:</span><span class="sxs-lookup"><span data-stu-id="cc8f5-280">Run the following Azure CLI command to create a resource group.</span></span>  

    ```bash
    az group create --name <resource_group_name> --location <region>  
    ```

    <span data-ttu-id="cc8f5-281">Geben Sie eine Region an, die SQL Data Warehouse, Azure Analysis Services und Data Factory v2 unterstützt.</span><span class="sxs-lookup"><span data-stu-id="cc8f5-281">Specify a region that supports SQL Data Warehouse, Azure Analysis Services, and Data Factory v2.</span></span> <span data-ttu-id="cc8f5-282">Informationen finden Sie unter [Azure-Produkte nach Region](https://azure.microsoft.com/global-infrastructure/services/).</span><span class="sxs-lookup"><span data-stu-id="cc8f5-282">See [Azure Products by Region](https://azure.microsoft.com/global-infrastructure/services/)</span></span>

3. <span data-ttu-id="cc8f5-283">Führen Sie den folgenden Befehl aus</span><span class="sxs-lookup"><span data-stu-id="cc8f5-283">Run the following command</span></span>

    ```
    az group deployment create --resource-group <resource_group_name> \
        --template-file adf-create-deploy.json \
        --parameters factoryName=<data_factory_name> location=<location>
    ```

<span data-ttu-id="cc8f5-284">Rufen Sie als Nächstes im Azure-Portal wie folgt den Authentifizierungsschlüssel für die [Integration Runtime ](/azure/data-factory/concepts-integration-runtime) von Azure Data Factory ab:</span><span class="sxs-lookup"><span data-stu-id="cc8f5-284">Next, use the Azure Portal to get the authentication key for the Azure Data Factory [integration runtime](/azure/data-factory/concepts-integration-runtime), as follows:</span></span>

1. <span data-ttu-id="cc8f5-285">Navigieren Sie im [Azure-Portal](https://portal.azure.com/) zur Data Factory-Instanz.</span><span class="sxs-lookup"><span data-stu-id="cc8f5-285">In the [Azure Portal](https://portal.azure.com/), navigate to the Data Factory instance.</span></span>

2. <span data-ttu-id="cc8f5-286">Klicken Sie auf dem Data Factory-Blatt auf **Erstellen und überwachen**.</span><span class="sxs-lookup"><span data-stu-id="cc8f5-286">In the Data Factory blade, click **Author & Monitor**.</span></span> <span data-ttu-id="cc8f5-287">Dadurch wird das Azure Data Factory-Portal in einem neuen Browserfenster geöffnet.</span><span class="sxs-lookup"><span data-stu-id="cc8f5-287">This opens the Azure Data Factory portal in another browser window.</span></span>

    ![](./images/adf-blade.png)

3. <span data-ttu-id="cc8f5-288">Wählen Sie im Azure Data Factory-Portal das Bleistiftsymbol („Autor“) aus.</span><span class="sxs-lookup"><span data-stu-id="cc8f5-288">In the Azure Data Factory portal, select the pencil icon ("Author").</span></span> 

4. <span data-ttu-id="cc8f5-289">Klicken Sie auf **Verbindungen**, und wählen Sie dann **Integration Runtimes**.</span><span class="sxs-lookup"><span data-stu-id="cc8f5-289">Click **Connections**, and then select **Integration Runtimes**.</span></span>

5. <span data-ttu-id="cc8f5-290">Klicken Sie unter **sourceIntegrationRuntime** Bleistiftsymbol („Bearbeiten“).</span><span class="sxs-lookup"><span data-stu-id="cc8f5-290">Under **sourceIntegrationRuntime**, click the pencil icon ("Edit").</span></span>

    > [!NOTE]
    > <span data-ttu-id="cc8f5-291">Im Portal wird der Status als „Nicht verfügbar“ angezeigt.</span><span class="sxs-lookup"><span data-stu-id="cc8f5-291">The portal will show the status as "unavailable".</span></span> <span data-ttu-id="cc8f5-292">Dies ist zu erwarten, bis Sie den lokalen Server bereitstellen.</span><span class="sxs-lookup"><span data-stu-id="cc8f5-292">This is expected until you deploy the on-premises server.</span></span>

6. <span data-ttu-id="cc8f5-293">Suchen Sie **Key1**, und kopieren Sie den Wert des Authentifizierungsschlüssels.</span><span class="sxs-lookup"><span data-stu-id="cc8f5-293">Find **Key1** and copy the value of the authentication key.</span></span>

<span data-ttu-id="cc8f5-294">Sie benötigen den Authentifizierungsschlüssel für den nächsten Schritt.</span><span class="sxs-lookup"><span data-stu-id="cc8f5-294">You will need the authentication key for the next step.</span></span>

### <a name="deploy-the-simulated-on-premises-server"></a><span data-ttu-id="cc8f5-295">Bereitstellen des simulierten lokalen Servers</span><span class="sxs-lookup"><span data-stu-id="cc8f5-295">Deploy the simulated on-premises server</span></span>

<span data-ttu-id="cc8f5-296">In diesem Schritt wird ein virtueller Computer als simulierter lokaler Server bereitgestellt, auf dem SQL Server 2017 und die zugehörigen Tools vorhanden sind.</span><span class="sxs-lookup"><span data-stu-id="cc8f5-296">This step deploys a VM as a simulated on-premises server, which includes SQL Server 2017 and related tools.</span></span> <span data-ttu-id="cc8f5-297">Außerdem wird die [OLTP-Datenbank von Wide World Importers ][wwi] in SQL Server geladen.</span><span class="sxs-lookup"><span data-stu-id="cc8f5-297">It also loads the [Wide World Importers OLTP database][wwi] into SQL Server.</span></span>

1. <span data-ttu-id="cc8f5-298">Navigieren Sie zum Ordner `data\enterprise_bi_sqldw_advanced\onprem\templates` des Repositorys.</span><span class="sxs-lookup"><span data-stu-id="cc8f5-298">Navigate to the `data\enterprise_bi_sqldw_advanced\onprem\templates` folder of the repository.</span></span>

2. <span data-ttu-id="cc8f5-299">Suchen Sie in der Datei `onprem.parameters.json` nach `adminPassword`.</span><span class="sxs-lookup"><span data-stu-id="cc8f5-299">In the `onprem.parameters.json` file, search for `adminPassword`.</span></span> <span data-ttu-id="cc8f5-300">Dies ist das Kennwort für die Anmeldung beim virtuellen SQL Server-Computer.</span><span class="sxs-lookup"><span data-stu-id="cc8f5-300">This is the password to log into the SQL Server VM.</span></span> <span data-ttu-id="cc8f5-301">Ersetzen Sie den Wert durch ein anderes Kennwort.</span><span class="sxs-lookup"><span data-stu-id="cc8f5-301">Replace the value with another password.</span></span>

3. <span data-ttu-id="cc8f5-302">Suchen Sie in derselben Datei nach `SqlUserCredentials`.</span><span class="sxs-lookup"><span data-stu-id="cc8f5-302">In the same file, search for `SqlUserCredentials`.</span></span> <span data-ttu-id="cc8f5-303">Diese Eigenschaft gibt die SQL Server-Kontoanmeldeinformationen an.</span><span class="sxs-lookup"><span data-stu-id="cc8f5-303">This property specifies the SQL Server account credentials.</span></span> <span data-ttu-id="cc8f5-304">Ersetzen Sie das Kennwort durch einen anderen Wert.</span><span class="sxs-lookup"><span data-stu-id="cc8f5-304">Replace the password with a different value.</span></span>

4. <span data-ttu-id="cc8f5-305">Fügen Sie in der gleichen Datei, Integration Runtime-Authentifizierungsschlüssel in den `IntegrationRuntimeGatewayKey`-Parameter ein, wie unten dargestellt:</span><span class="sxs-lookup"><span data-stu-id="cc8f5-305">In the same file, paste the Integration Runtime authentication key into the `IntegrationRuntimeGatewayKey` parameter, as shown below:</span></span>

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

5. <span data-ttu-id="cc8f5-306">Führen Sie den folgenden Befehl aus:</span><span class="sxs-lookup"><span data-stu-id="cc8f5-306">Run the following command.</span></span>

    ```bash
    azbb -s <subscription_id> -g <resource_group_name> -l <region> -p onprem.parameters.json --deploy
    ```

<span data-ttu-id="cc8f5-307">Die Ausführung dieses Schritts kann 20 bis 30 Minuten dauern.</span><span class="sxs-lookup"><span data-stu-id="cc8f5-307">This step may take 20 to 30 minutes to complete.</span></span> <span data-ttu-id="cc8f5-308">Er umfasst die Ausführung eines [DSC](/powershell/dsc/overview)-Skripts zum Installieren der Tools und zum Wiederherstellen der Datenbank.</span><span class="sxs-lookup"><span data-stu-id="cc8f5-308">It includes running a [DSC](/powershell/dsc/overview) script to install the tools and restore the database.</span></span> 

### <a name="deploy-azure-resources"></a><span data-ttu-id="cc8f5-309">Bereitstellen von Azure-Ressourcen</span><span class="sxs-lookup"><span data-stu-id="cc8f5-309">Deploy Azure resources</span></span>

<span data-ttu-id="cc8f5-310">In diesem Schritt werden SQL Data Warehouse, Azure Analysis Services und Data Factory bereitgestellt.</span><span class="sxs-lookup"><span data-stu-id="cc8f5-310">This step provisions SQL Data Warehouse, Azure Analysis Services, and Data Factory.</span></span>

1. <span data-ttu-id="cc8f5-311">Navigieren Sie zum Ordner `data\enterprise_bi_sqldw_advanced\azure\templates` des [GitHub-Repositorys][ref-arch-repo].</span><span class="sxs-lookup"><span data-stu-id="cc8f5-311">Navigate to the `data\enterprise_bi_sqldw_advanced\azure\templates` folder of the [GitHub repository][ref-arch-repo].</span></span>

2. <span data-ttu-id="cc8f5-312">Führen Sie den folgenden Azure-CLI-Befehl aus:</span><span class="sxs-lookup"><span data-stu-id="cc8f5-312">Run the following Azure CLI command.</span></span> <span data-ttu-id="cc8f5-313">Ersetzen Sie die Parameterwerte in spitzen Klammern.</span><span class="sxs-lookup"><span data-stu-id="cc8f5-313">Replace the parameter values shown in angle brackets.</span></span>

    ```bash
    az group deployment create --resource-group <resource_group_name> \
     --template-file azure-resources-deploy.json \
     --parameters "dwServerName"="<data_warehouse_server_name>" \
     "dwAdminLogin"="adminuser" "dwAdminPassword"="<data_warehouse_password>" \ 
     "storageAccountName"="<storage_account_name>" \
     "analysisServerName"="<analysis_server_name>" \
     "analysisServerAdmin"="<user@contoso.com>"
    ```

    - <span data-ttu-id="cc8f5-314">Beim `storageAccountName`-Parameter müssen Sie die [Benennungsregeln](../../best-practices/naming-conventions.md#naming-rules-and-restrictions) für Speicherkonten befolgen.</span><span class="sxs-lookup"><span data-stu-id="cc8f5-314">The `storageAccountName` parameter must follow the [naming rules](../../best-practices/naming-conventions.md#naming-rules-and-restrictions) for Storage accounts.</span></span> 
    - <span data-ttu-id="cc8f5-315">Verwenden Sie für den `analysisServerAdmin`-Parameter Ihren Azure Active Directory-Benutzerprinzipalnamen (User Principal Name, UPN).</span><span class="sxs-lookup"><span data-stu-id="cc8f5-315">For the `analysisServerAdmin` parameter, use your Azure Active Directory user principal name (UPN).</span></span>

3. <span data-ttu-id="cc8f5-316">Führen Sie den folgenden Azure CLI-Befehl zum Abrufen des Zugriffsschlüssels für das Speicherkonto aus.</span><span class="sxs-lookup"><span data-stu-id="cc8f5-316">Run the following Azure CLI command to get the access key for the storage account.</span></span> <span data-ttu-id="cc8f5-317">Diesen Schlüssel verwenden Sie im nächsten Schritt.</span><span class="sxs-lookup"><span data-stu-id="cc8f5-317">You will use this key in the next step.</span></span>

    ```bash
    az storage account keys list -n <storage_account_name> -g <resource_group_name> --query [0].value
    ```

4. <span data-ttu-id="cc8f5-318">Führen Sie den folgenden Azure-CLI-Befehl aus:</span><span class="sxs-lookup"><span data-stu-id="cc8f5-318">Run the following Azure CLI command.</span></span> <span data-ttu-id="cc8f5-319">Ersetzen Sie die Parameterwerte in spitzen Klammern.</span><span class="sxs-lookup"><span data-stu-id="cc8f5-319">Replace the parameter values shown in angle brackets.</span></span> 

    ```bash
    az group deployment create --resource-group <resource_group_name> \
    --template-file adf-pipeline-deploy.json \
    --parameters "factoryName"="<data_factory_name>" \
    "sinkDWConnectionString"="Server=tcp:<data_warehouse_server_name>.database.windows.net,1433;Initial Catalog=wwi;Persist Security Info=False;User ID=adminuser;Password=<data_warehouse_password>;MultipleActiveResultSets=False;Encrypt=True;TrustServerCertificate=False;Connection Timeout=30;" \
    "blobConnectionString"="DefaultEndpointsProtocol=https;AccountName=<storage_account_name>;AccountKey=<storage_account_key>;EndpointSuffix=core.windows.net" \
    "sourceDBConnectionString"="Server=sql1;Database=WideWorldImporters;User Id=adminuser;Password=<sql-db-password>;Trusted_Connection=True;"
    ```

    <span data-ttu-id="cc8f5-320">Die Verbindungszeichenfolgen weisen Teilzeichenfolgen in spitzen Klammern auf, die ersetzt werden müssen.</span><span class="sxs-lookup"><span data-stu-id="cc8f5-320">The connection strings have substrings shown in angle brackets that must be replaced.</span></span> <span data-ttu-id="cc8f5-321">Verwenden Sie für `<storage_account_key>` den Schlüssel, den Sie im vorherigen Schritt abgerufen haben.</span><span class="sxs-lookup"><span data-stu-id="cc8f5-321">For `<storage_account_key>`, use the key that you got in the previous step.</span></span> <span data-ttu-id="cc8f5-322">Verwenden Sie für `<sql-db-password>` das SQL Server-Kontokennwort, das Sie zuvor in der Datei `onprem.parameters.json` angegeben haben.</span><span class="sxs-lookup"><span data-stu-id="cc8f5-322">For `<sql-db-password>`, use the SQL Server account password that you specified in the `onprem.parameters.json` file previously.</span></span>

### <a name="run-the-data-warehouse-scripts"></a><span data-ttu-id="cc8f5-323">Ausführen der Data Warehouse-Skripts</span><span class="sxs-lookup"><span data-stu-id="cc8f5-323">Run the data warehouse scripts</span></span>

1. <span data-ttu-id="cc8f5-324">Suchen Sie im [Azure-Portal](https://portal.azure.com/) den lokalen virtuellen Computer mit dem Namen `sql-vm1`.</span><span class="sxs-lookup"><span data-stu-id="cc8f5-324">In the [Azure Portal](https://portal.azure.com/), find the on-premises VM, which is named `sql-vm1`.</span></span> <span data-ttu-id="cc8f5-325">Der Benutzername und das Kennwort für den virtuellen Computer sind in der Datei `onprem.parameters.json` angegeben.</span><span class="sxs-lookup"><span data-stu-id="cc8f5-325">The user name and password for the VM are specified in the `onprem.parameters.json` file.</span></span>

2. <span data-ttu-id="cc8f5-326">Klicken Sie auf **Verbinden**, und stellen Sie über Remotedesktop eine Verbindung zum virtuellen Computer her.</span><span class="sxs-lookup"><span data-stu-id="cc8f5-326">Click **Connect** and use Remote Desktop to connect to the VM.</span></span>

3. <span data-ttu-id="cc8f5-327">Öffnen Sie über die Remotedesktopsitzung eine Eingabeaufforderung, und navigieren Sie zum folgenden Ordner auf dem virtuellen Computer:</span><span class="sxs-lookup"><span data-stu-id="cc8f5-327">From your Remote Desktop session, open a command prompt and navigate to the following folder on the VM:</span></span>

    ```
    cd C:\SampleDataFiles\reference-architectures\data\enterprise_bi_sqldw_advanced\azure\sqldw_scripts
    ```

4. <span data-ttu-id="cc8f5-328">Führen Sie den folgenden Befehl aus:</span><span class="sxs-lookup"><span data-stu-id="cc8f5-328">Run the following command:</span></span>

    ```
    deploy_database.cmd -S <data_warehouse_server_name>.database.windows.net -d wwi -U adminuser -P <data_warehouse_password> -N -I
    ```

    <span data-ttu-id="cc8f5-329">Für `<data_warehouse_server_name>` und `<data_warehouse_password>` verwenden Sie den Data Warehouse-Namen und das Kennwort von vorhin.</span><span class="sxs-lookup"><span data-stu-id="cc8f5-329">For `<data_warehouse_server_name>` and `<data_warehouse_password>`, use the data warehouse server name and password from earlier.</span></span>

<span data-ttu-id="cc8f5-330">Zum Überprüfen dieses Schritts können Sie mithilfe von SQL Server Management Studio (SSMS) eine Verbindung mit der SQL Data Warehouse-Datenbank herstellen.</span><span class="sxs-lookup"><span data-stu-id="cc8f5-330">To verify this step, you can use SQL Server Management Studio (SSMS) to connect to the SQL Data Warehouse database.</span></span> <span data-ttu-id="cc8f5-331">Die Datenbank-Tabellenschemas sollten angezeigt werden.</span><span class="sxs-lookup"><span data-stu-id="cc8f5-331">You should see the database table schemas.</span></span>

### <a name="run-the-data-factory-pipeline"></a><span data-ttu-id="cc8f5-332">Ausführen der Data Factory-Pipeline</span><span class="sxs-lookup"><span data-stu-id="cc8f5-332">Run the Data Factory pipeline</span></span>

1. <span data-ttu-id="cc8f5-333">Öffnen Sie in der gleichen Remotedesktopsitzung ein PowerShell-Fenster.</span><span class="sxs-lookup"><span data-stu-id="cc8f5-333">From the same Remote Desktop session, open a PowerShell window.</span></span>

2. <span data-ttu-id="cc8f5-334">Führen Sie den folgenden PowerShell-Befehl aus:</span><span class="sxs-lookup"><span data-stu-id="cc8f5-334">Run the following PowerShell command.</span></span> <span data-ttu-id="cc8f5-335">Wählen Sie **Ja**, wenn Sie dazu aufgefordert werden.</span><span class="sxs-lookup"><span data-stu-id="cc8f5-335">Choose **Yes** when prompted.</span></span>

    ```powershell
    Install-Module -Name AzureRM -AllowClobber
    ```

3. <span data-ttu-id="cc8f5-336">Führen Sie den folgenden PowerShell-Befehl aus:</span><span class="sxs-lookup"><span data-stu-id="cc8f5-336">Run the following PowerShell command.</span></span> <span data-ttu-id="cc8f5-337">Geben Sie Ihre Azure-Anmeldeinformationen ein.</span><span class="sxs-lookup"><span data-stu-id="cc8f5-337">Enter your Azure credentials when prompted.</span></span>

    ```powershell
    Connect-AzureRmAccount 
    ```

4. <span data-ttu-id="cc8f5-338">Führen Sie die folgenden PowerShell-Befehle aus:</span><span class="sxs-lookup"><span data-stu-id="cc8f5-338">Run the following PowerShell commands.</span></span> <span data-ttu-id="cc8f5-339">Ersetzen Sie die Werte in spitzen Klammern.</span><span class="sxs-lookup"><span data-stu-id="cc8f5-339">Replace the values in angle brackets.</span></span>

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
    <span data-ttu-id="cc8f5-340">Total Sales:=SUM('Fact Sale'[Gesamtsumme einschließlich Steuer])</span><span class="sxs-lookup"><span data-stu-id="cc8f5-340">Total Sales:=SUM('Fact Sale'[Total Including Tax])</span></span>
    ```

4. Repeat these steps to create the following measures:

    ```
    <span data-ttu-id="cc8f5-341">Number of Years:=(MAX('Fact CityPopulation'[Jahreszahl])-MIN('Fact CityPopulation'[Jahreszahl]))+1</span><span class="sxs-lookup"><span data-stu-id="cc8f5-341">Number of Years:=(MAX('Fact CityPopulation'[YearNumber])-MIN('Fact CityPopulation'[YearNumber]))+1</span></span>
    
    <span data-ttu-id="cc8f5-342">Beginning Population:=CALCULATE(SUM('Fact CityPopulation'[Bevölkerung]),FILTER('Fact CityPopulation','Fact CityPopulation'[Jahreszahl]=MIN('Fact CityPopulation'[Jahreszahl])))</span><span class="sxs-lookup"><span data-stu-id="cc8f5-342">Beginning Population:=CALCULATE(SUM('Fact CityPopulation'[Population]),FILTER('Fact CityPopulation','Fact CityPopulation'[YearNumber]=MIN('Fact CityPopulation'[YearNumber])))</span></span>
    
    <span data-ttu-id="cc8f5-343">Ending Population:=CALCULATE(SUM('Fact CityPopulation'[Bevölkerung]),FILTER('Fact CityPopulation','Fact CityPopulation'[Jahreszahl]=MAX('Fact CityPopulation'[Jahreszahl])))</span><span class="sxs-lookup"><span data-stu-id="cc8f5-343">Ending Population:=CALCULATE(SUM('Fact CityPopulation'[Population]),FILTER('Fact CityPopulation','Fact CityPopulation'[YearNumber]=MAX('Fact CityPopulation'[YearNumber])))</span></span>
    
    <span data-ttu-id="cc8f5-344">CAGR:=IFERROR((([Endbevölkerung]/[Anfangsbevölkerung])^(1/[Anzahl der Jahre]))-1,0)</span><span class="sxs-lookup"><span data-stu-id="cc8f5-344">CAGR:=IFERROR((([Ending Population]/[Beginning Population])^(1/[Number of Years]))-1,0)</span></span>
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
