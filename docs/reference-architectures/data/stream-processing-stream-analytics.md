---
title: Datenstromverarbeitung mit Azure Stream Analytics
description: Erstellen einer End-to-End-Pipeline zur Datenstromverarbeitung in Azure
author: MikeWasson
ms.date: 08/09/2018
ms.openlocfilehash: 82887bdd45f811ac733ead18c1f256098e575253
ms.sourcegitcommit: c4106b58ad08f490e170e461009a4693578294ea
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/10/2018
ms.locfileid: "40025511"
---
# <a name="stream-processing-with-azure-stream-analytics"></a><span data-ttu-id="9587d-103">Datenstromverarbeitung mit Azure Stream Analytics</span><span class="sxs-lookup"><span data-stu-id="9587d-103">Stream processing with Azure Stream Analytics</span></span>

<span data-ttu-id="9587d-104">Diese Referenzarchitektur zeigt eine End-to-End-Pipeline zur Datenstromverarbeitung.</span><span class="sxs-lookup"><span data-stu-id="9587d-104">This reference architecture shows an end-to-end stream processing pipeline.</span></span> <span data-ttu-id="9587d-105">Die Pipeline erfasst Daten aus zwei Quellen, korreliert Datensätze in den beiden Datenströmen und berechnet einen gleitenden Durchschnitt über ein Zeitfenster.</span><span class="sxs-lookup"><span data-stu-id="9587d-105">The pipeline ingests data from two sources, correlates records in the two streams, and calculates a rolling average across a time window.</span></span> <span data-ttu-id="9587d-106">Die Ergebnisse werden zur weiteren Analyse gespeichert.</span><span class="sxs-lookup"><span data-stu-id="9587d-106">The results are stored for further analysis.</span></span> [<span data-ttu-id="9587d-107">**So stellen Sie diese Lösung bereit**.</span><span class="sxs-lookup"><span data-stu-id="9587d-107">**Deploy this solution**.</span></span>](#deploy-the-solution)

![](./images/stream-processing-asa/stream-processing-asa.png)

<span data-ttu-id="9587d-108">**Szenario**: Ein Taxiunternehmen erfasst Daten zu jeder Taxifahrt.</span><span class="sxs-lookup"><span data-stu-id="9587d-108">**Scenario**: A taxi company collects data about each taxi trip.</span></span> <span data-ttu-id="9587d-109">In diesem Szenario gehen wir davon aus, dass zwei separate Geräte Daten senden.</span><span class="sxs-lookup"><span data-stu-id="9587d-109">For this scenario, we assume there are two separate devices sending data.</span></span> <span data-ttu-id="9587d-110">Das Taxi verfügt ein Messgerät, das Informationen zu jeder Fahrt sendet – die Dauer, die Strecke sowie die Abhol- und Zielorte.</span><span class="sxs-lookup"><span data-stu-id="9587d-110">The taxi has a meter that sends information about each ride &mdash; the duration, distance, and pickup and dropoff locations.</span></span> <span data-ttu-id="9587d-111">Ein separates Gerät akzeptiert Zahlungen von Kunden und sendet Daten zu den Fahrpreisen.</span><span class="sxs-lookup"><span data-stu-id="9587d-111">A separate device accepts payments from customers and sends data about fares.</span></span> <span data-ttu-id="9587d-112">Das Taxiunternehmen möchte das durchschnittliche Trinkgeld pro gefahrener Meile in Echtzeit berechnen, um Trends zu erkennen.</span><span class="sxs-lookup"><span data-stu-id="9587d-112">The taxi company wants to calculate the average tip per mile driven, in real time, in order to spot trends.</span></span>

## <a name="architecture"></a><span data-ttu-id="9587d-113">Architecture</span><span class="sxs-lookup"><span data-stu-id="9587d-113">Architecture</span></span>

<span data-ttu-id="9587d-114">Die Architektur umfasst die folgenden Komponenten.</span><span class="sxs-lookup"><span data-stu-id="9587d-114">The architecture consists of the following components.</span></span>

<span data-ttu-id="9587d-115">**Datenquellen:**</span><span class="sxs-lookup"><span data-stu-id="9587d-115">**Data sources**.</span></span> <span data-ttu-id="9587d-116">In dieser Architektur gibt es zwei Datenquellen, die Datenströme in Echtzeit generieren.</span><span class="sxs-lookup"><span data-stu-id="9587d-116">In this architecture, there are two data sources that generate data streams in real time.</span></span> <span data-ttu-id="9587d-117">Der erste Datenstrom enthält Informationen zur Fahrt und der zweite Informationen zum Fahrpreis.</span><span class="sxs-lookup"><span data-stu-id="9587d-117">The first stream contains ride information, and the second contains fare information.</span></span> <span data-ttu-id="9587d-118">Die Referenzarchitektur umfasst einen simulierten Datengenerator, der aus einem Satz von statischen Dateien liest und die Daten an Event Hubs pusht.</span><span class="sxs-lookup"><span data-stu-id="9587d-118">The reference architecture includes a simulated data generator that reads from a set of static files and pushes the data to Event Hubs.</span></span> <span data-ttu-id="9587d-119">In einer echten Anwendung würde es sich bei den Datenquellen um Geräte handeln, die in den Taxis installiert sind.</span><span class="sxs-lookup"><span data-stu-id="9587d-119">In a real application, the data sources would be devices installed in the taxi cabs.</span></span>

<span data-ttu-id="9587d-120">**Azure Event Hubs**:</span><span class="sxs-lookup"><span data-stu-id="9587d-120">**Azure Event Hubs**.</span></span> <span data-ttu-id="9587d-121">[Event Hubs](/azure/event-hubs/) ist ein Ereigniserfassungsdienst.</span><span class="sxs-lookup"><span data-stu-id="9587d-121">[Event Hubs](/azure/event-hubs/) is an event ingestion service.</span></span> <span data-ttu-id="9587d-122">Die hier gezeigte Architektur verwendet zwei Event Hub-Instanzen, eine für jede Datenquelle.</span><span class="sxs-lookup"><span data-stu-id="9587d-122">This architecture uses two event hub instances, one for each data source.</span></span> <span data-ttu-id="9587d-123">Jede Datenquelle sendet einen Datenstrom an den zugehörigen Event Hub.</span><span class="sxs-lookup"><span data-stu-id="9587d-123">Each data source sends a stream of data to the associated event hub.</span></span>

<span data-ttu-id="9587d-124">**Azure Stream Analytics**:</span><span class="sxs-lookup"><span data-stu-id="9587d-124">**Azure Stream Analytics**.</span></span> <span data-ttu-id="9587d-125">[Stream Analytics](/azure/stream-analytics/) ist ein Ereignisverarbeitungsmodul.</span><span class="sxs-lookup"><span data-stu-id="9587d-125">[Stream Analytics](/azure/stream-analytics/) is an event-processing engine.</span></span> <span data-ttu-id="9587d-126">Ein Stream Analytics-Auftrag liest die Datenströme aus den beiden Event Hubs und führt die Datenstromverarbeitung durch.</span><span class="sxs-lookup"><span data-stu-id="9587d-126">A Stream Analytics job reads the data streams from the two event hubs and performs stream processing.</span></span>

<span data-ttu-id="9587d-127">**Cosmos DB**:</span><span class="sxs-lookup"><span data-stu-id="9587d-127">**Cosmos DB**.</span></span> <span data-ttu-id="9587d-128">Die Ausgabe des Stream Analytics-Auftrags ist eine Reihe von Datensätzen, die als JSON-Dokumente in eine Cosmos DB-Dokumentdatenbank geschrieben werden.</span><span class="sxs-lookup"><span data-stu-id="9587d-128">The output from the Stream Analytics job is a series of records, which are written as JSON documents to a Cosmos DB document database.</span></span>

<span data-ttu-id="9587d-129">**Microsoft Power BI**:</span><span class="sxs-lookup"><span data-stu-id="9587d-129">**Microsoft Power BI**.</span></span> <span data-ttu-id="9587d-130">Power BI ist eine Suite aus Business Analytics-Tools zum Analysieren von Daten für Einblicke in Geschäftsvorgänge.</span><span class="sxs-lookup"><span data-stu-id="9587d-130">Power BI is a suite of business analytics tools to analyze data for business insights.</span></span> <span data-ttu-id="9587d-131">In dieser Architektur lädt Power BI die Daten aus Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="9587d-131">In this architecture, it loads the data from Cosmos DB.</span></span> <span data-ttu-id="9587d-132">Dadurch können Benutzer den vollständigen Satz von historischen Daten, die gesammelt wurden, analysieren.</span><span class="sxs-lookup"><span data-stu-id="9587d-132">This allows users to analyze the complete set of historical data that's been collected.</span></span> <span data-ttu-id="9587d-133">Sie könnten die Ergebnisse auch direkt von Stream Analytics an Power BI streamen, um die Daten in Echtzeit anzuzeigen.</span><span class="sxs-lookup"><span data-stu-id="9587d-133">You could also stream the results directly from Stream Analytics to Power BI for a real-time view of the data.</span></span> <span data-ttu-id="9587d-134">Weitere Informationen finden Sie unter [Echtzeitstreaming in Power BI](/power-bi/service-real-time-streaming).</span><span class="sxs-lookup"><span data-stu-id="9587d-134">For more information, see [Real-time streaming in Power BI](/power-bi/service-real-time-streaming).</span></span>

<span data-ttu-id="9587d-135">**Azure Monitor**:</span><span class="sxs-lookup"><span data-stu-id="9587d-135">**Azure Monitor**.</span></span> <span data-ttu-id="9587d-136">[Azure Monitor](/azure/monitoring-and-diagnostics/) erfasst Leistungsmetriken zu den in der Lösung bereitgestellten Azure-Diensten.</span><span class="sxs-lookup"><span data-stu-id="9587d-136">[Azure Monitor](/azure/monitoring-and-diagnostics/) collects performance metrics about the Azure services deployed in the solution.</span></span> <span data-ttu-id="9587d-137">Durch die Visualisierung dieser Metriken in einem Dashboard können Sie Erkenntnisse über die Integrität der Lösung gewinnen.</span><span class="sxs-lookup"><span data-stu-id="9587d-137">By visualizing these in a dashboard, you can get insights into the health of the solution.</span></span> 

## <a name="data-ingestion"></a><span data-ttu-id="9587d-138">Datenerfassung</span><span class="sxs-lookup"><span data-stu-id="9587d-138">Data ingestion</span></span>

<span data-ttu-id="9587d-139">Um eine Datenquelle zu simulieren, verwendet die Referenzarchitektur das Dataset [New York City Taxi Data](https://uofi.app.box.com/v/NYCtaxidata/folder/2332218797)<sup>[[1]](#note1)</sup>.</span><span class="sxs-lookup"><span data-stu-id="9587d-139">To simulate a data source, this reference architecture uses the [New York City Taxi Data](https://uofi.app.box.com/v/NYCtaxidata/folder/2332218797) dataset<sup>[[1]](#note1)</sup>.</span></span> <span data-ttu-id="9587d-140">Dieses Dataset enthält Daten zu Taxifahrten in New York City für einen Zeitraum von vier Jahren (2010 &ndash; 2013).</span><span class="sxs-lookup"><span data-stu-id="9587d-140">This dataset contains data about taxi trips in New York City over a 4-year period (2010 &ndash; 2013).</span></span> <span data-ttu-id="9587d-141">Es umfasst zwei Datensatztypen: Fahrtdaten und Fahrpreisdaten.</span><span class="sxs-lookup"><span data-stu-id="9587d-141">It contains two types of record: Ride data and fare data.</span></span> <span data-ttu-id="9587d-142">Die Fahrtdaten enthalten die Fahrtdauer, die Fahrtstrecke sowie die Abhol- und Zielorte.</span><span class="sxs-lookup"><span data-stu-id="9587d-142">Ride data includes trip duration, trip distance, and pickup and dropoff location.</span></span> <span data-ttu-id="9587d-143">Die Fahrpreisdaten enthalten die Beträge von Fahrpreis, Steuern und Trinkgeld.</span><span class="sxs-lookup"><span data-stu-id="9587d-143">Fare data includes fare, tax, and tip amounts.</span></span> <span data-ttu-id="9587d-144">Gemeinsame Felder in beiden Datensatztypen sind die Taxinummer (Medallion), die Taxilizenz (Hack license) und die Anbieter-ID (Vendor ID).</span><span class="sxs-lookup"><span data-stu-id="9587d-144">Common fields in both record types include medallion number, hack license, and vendor ID.</span></span> <span data-ttu-id="9587d-145">Anhand dieser drei Felder werden ein Taxi und ein Fahrer eindeutig identifiziert.</span><span class="sxs-lookup"><span data-stu-id="9587d-145">Together these three fields uniquely identify a taxi plus a driver.</span></span> <span data-ttu-id="9587d-146">Die Daten werden im CSV-Format gespeichert.</span><span class="sxs-lookup"><span data-stu-id="9587d-146">The data is stored in CSV format.</span></span> 

<span data-ttu-id="9587d-147">Der Datengenerator ist eine .NET Core-Anwendung, die Datensätze liest und an Azure Event Hubs sendet.</span><span class="sxs-lookup"><span data-stu-id="9587d-147">The data generator is a .NET Core application that reads the records and sends them to Azure Event Hubs.</span></span> <span data-ttu-id="9587d-148">Der Generator sendet Fahrtdaten im JSON-Format und Fahrpreisdaten im CSV-Format.</span><span class="sxs-lookup"><span data-stu-id="9587d-148">The generator sends ride data in JSON format and fare data in CSV format.</span></span> 

<span data-ttu-id="9587d-149">Event Hubs verwendet [Partitionen](/azure/event-hubs/event-hubs-features#partitions) zum Segmentieren der Daten.</span><span class="sxs-lookup"><span data-stu-id="9587d-149">Event Hubs uses [partitions](/azure/event-hubs/event-hubs-features#partitions) to segment the data.</span></span> <span data-ttu-id="9587d-150">Partitionen ermöglichen es einem Consumer, die einzelnen Partitionen gleichzeitig zu lesen.</span><span class="sxs-lookup"><span data-stu-id="9587d-150">Partitions allow a consumer to read each partition in parallel.</span></span> <span data-ttu-id="9587d-151">Wenn Sie Daten an Event Hubs senden, können Sie den Partitionsschlüssel explizit angeben.</span><span class="sxs-lookup"><span data-stu-id="9587d-151">When you send data to Event Hubs, you can specify the partition key explicitly.</span></span> <span data-ttu-id="9587d-152">Andernfalls werden Datensätze nach einem Roundrobinverfahren Partitionen zugewiesen.</span><span class="sxs-lookup"><span data-stu-id="9587d-152">Otherwise, records are assigned to partitions in round-robin fashion.</span></span> 

<span data-ttu-id="9587d-153">In diesem speziellen Szenario sollen Fahrtdaten und Fahrpreisdaten für ein bestimmtes Taxi die gleiche Partitions-ID erhalten.</span><span class="sxs-lookup"><span data-stu-id="9587d-153">In this particular scenario, ride data and fare data should end up with the same partition ID for a given taxi cab.</span></span> <span data-ttu-id="9587d-154">Dadurch kann Stream Analytics beim Korrelieren der beiden Datenströme einen Grad an Parallelität anwenden.</span><span class="sxs-lookup"><span data-stu-id="9587d-154">This enables Stream Analytics to apply a degree of parallelism when it correlates the two streams.</span></span> <span data-ttu-id="9587d-155">Ein Datensatz in der Partition *n* der Fahrtdaten entspricht einem Datensatz in der Partition *n* der Fahrpreisdaten.</span><span class="sxs-lookup"><span data-stu-id="9587d-155">A record in partition *n* of the ride data will match a record in partition *n* of the fare data.</span></span>

![](./images/stream-processing-asa/stream-processing-eh.png)

<span data-ttu-id="9587d-156">Im Datengenerator enthält das gemeinsame Datenmodell für beide Datensatztypen eine `PartitionKey`-Eigenschaft, bei der es sich um die Verkettung von `Medallion`, `HackLicense` und `VendorId` handelt.</span><span class="sxs-lookup"><span data-stu-id="9587d-156">In the data generator, the common data model for both record types has a `PartitionKey` property which is the concatenation of `Medallion`, `HackLicense`, and `VendorId`.</span></span>

```csharp
public abstract class TaxiData
{
    public TaxiData()
    {
    }

    [JsonProperty]
    public long Medallion { get; set; }

    [JsonProperty]
    public long HackLicense { get; set; }

    [JsonProperty]
    public string VendorId { get; set; }

    [JsonProperty]
    public DateTimeOffset PickupTime { get; set; }

    [JsonIgnore]
    public string PartitionKey
    {
        get => $"{Medallion}_{HackLicense}_{VendorId}";
    }
```

<span data-ttu-id="9587d-157">Diese Eigenschaft wird verwendet, um beim Senden der Daten an Event Hubs einen expliziten Partitionsschlüssel bereitzustellen:</span><span class="sxs-lookup"><span data-stu-id="9587d-157">This property is used to provide an explicit partition key when sending to Event Hubs:</span></span>

```csharp
using (var client = pool.GetObject())
{
    return client.Value.SendAsync(new EventData(Encoding.UTF8.GetBytes(
        t.GetData(dataFormat))), t.PartitionKey);
}
```

## <a name="stream-processing"></a><span data-ttu-id="9587d-158">Datenstromverarbeitung</span><span class="sxs-lookup"><span data-stu-id="9587d-158">Stream processing</span></span>

<span data-ttu-id="9587d-159">Der Auftrag zur Datenstromverarbeitung wird anhand einer SQL-Abfrage mit mehreren separaten Schritten definiert.</span><span class="sxs-lookup"><span data-stu-id="9587d-159">The stream processing job is defined using a SQL query with several distinct steps.</span></span> <span data-ttu-id="9587d-160">In den ersten beiden Schritten werden lediglich Datensätze aus den zwei Eingabedatenströmen ausgewählt.</span><span class="sxs-lookup"><span data-stu-id="9587d-160">The first two steps simply select records from the two input streams.</span></span>

```sql
WITH
Step1 AS (
    SELECT PartitionId,
           TRY_CAST(Medallion AS nvarchar(max)) AS Medallion,
           TRY_CAST(HackLicense AS nvarchar(max)) AS HackLicense,
           VendorId,
           TRY_CAST(PickupTime AS datetime) AS PickupTime,
           TripDistanceInMiles
    FROM [TaxiRide] PARTITION BY PartitionId
),
Step2 AS (
    SELECT PartitionId,
           medallion AS Medallion,
           hack_license AS HackLicense,
           vendor_id AS VendorId,
           TRY_CAST(pickup_datetime AS datetime) AS PickupTime,
           tip_amount AS TipAmount
    FROM [TaxiFare] PARTITION BY PartitionId
),
```

<span data-ttu-id="9587d-161">Im nächsten Schritt werden die beiden Eingabedatenströme verknüpft, um übereinstimmende Datensätze aus jedem Datenstrom auszuwählen.</span><span class="sxs-lookup"><span data-stu-id="9587d-161">The next step joins the two input streams to select matching records from each stream.</span></span>

```sql
Step3 AS (
  SELECT
         tr.Medallion,
         tr.HackLicense,
         tr.VendorId,
         tr.PickupTime,
         tr.TripDistanceInMiles,
         tf.TipAmount
    FROM [Step1] tr
    PARTITION BY PartitionId
    JOIN [Step2] tf PARTITION BY PartitionId
      ON tr.Medallion = tf.Medallion
     AND tr.HackLicense = tf.HackLicense
     AND tr.VendorId = tf.VendorId
     AND tr.PickupTime = tf.PickupTime
     AND tr.PartitionId = tf.PartitionId
     AND DATEDIFF(minute, tr, tf) BETWEEN 0 AND 15
)
```

<span data-ttu-id="9587d-162">Diese Abfrage verknüpft Datensätze für eine Gruppe von Feldern, die übereinstimmende Datensätze eindeutig identifizieren („Medallion“, „HackLicense“, „VendorId“ und „PickupTime“).</span><span class="sxs-lookup"><span data-stu-id="9587d-162">This query joins records on a set of fields that uniquely identify matching records (Medallion, HackLicense, VendorId, and PickupTime).</span></span> <span data-ttu-id="9587d-163">Die `JOIN`-Anweisung enthält auch die Partitions-ID.</span><span class="sxs-lookup"><span data-stu-id="9587d-163">The `JOIN` statement also includes the partition ID.</span></span> <span data-ttu-id="9587d-164">Wie bereits erwähnt, wird hierbei die Tatsache genutzt, dass übereinstimmende Datensätze in diesem Szenario immer die gleiche Partitions-ID aufweisen.</span><span class="sxs-lookup"><span data-stu-id="9587d-164">As mentioned, this takes advantage of the fact that matching records always have the same partition ID in this scenario.</span></span>

<span data-ttu-id="9587d-165">In Stream Analytics sind Verknüpfungen *temporal*, d. h. Datensätze werden innerhalb eines bestimmten Zeitfensters verknüpft.</span><span class="sxs-lookup"><span data-stu-id="9587d-165">In Stream Analytics, joins are *temporal*, meaning records are joined within a particular window of time.</span></span> <span data-ttu-id="9587d-166">Andernfalls müsste der Auftrag möglicherweise unbegrenzt auf eine Übereinstimmung warten.</span><span class="sxs-lookup"><span data-stu-id="9587d-166">Otherwise, the job might need to wait indefinitely for a match.</span></span> <span data-ttu-id="9587d-167">Die Funktion [DATEDIFF](https://msdn.microsoft.com/azure/stream-analytics/reference/join-azure-stream-analytics) gibt an, wie weit zwei übereinstimmende Datensätze zeitlich auseinander liegen können, um als Übereinstimmung ermittelt zu werden.</span><span class="sxs-lookup"><span data-stu-id="9587d-167">The [DATEDIFF](https://msdn.microsoft.com/azure/stream-analytics/reference/join-azure-stream-analytics) function specifies how far two matching records can be separated in time for a match.</span></span> 

<span data-ttu-id="9587d-168">Im letzten Schritt des Auftrags wird das durchschnittliche Trinkgeld pro Meile gruppiert anhand eines springenden Fensters von fünf Minuten berechnet.</span><span class="sxs-lookup"><span data-stu-id="9587d-168">The last step in the job computes the average tip per mile, grouped by a hopping window of 5 minutes.</span></span>

```sql
SELECT System.Timestamp AS WindowTime,
       SUM(tr.TipAmount) / SUM(tr.TripDistanceInMiles) AS AverageTipPerMile
  INTO [TaxiDrain]
  FROM [Step3] tr
  GROUP BY HoppingWindow(Duration(minute, 5), Hop(minute, 1))
```

<span data-ttu-id="9587d-169">Stream Analytics bietet mehrere [Windowing-Funktionen](/azure/stream-analytics/stream-analytics-window-functions).</span><span class="sxs-lookup"><span data-stu-id="9587d-169">Stream Analytics provides several [windowing functions](/azure/stream-analytics/stream-analytics-window-functions).</span></span> <span data-ttu-id="9587d-170">Ein springendes Fenster bewegt sich um einen festen Zeitraum vorwärts (in diesem Fall eine Minute pro Sprung).</span><span class="sxs-lookup"><span data-stu-id="9587d-170">A hopping window moves forward in time by a fixed period, in this case 1 minute per hop.</span></span> <span data-ttu-id="9587d-171">Auf diese Weise wird ein gleitender Durchschnitt für die letzten fünf Minuten berechnet.</span><span class="sxs-lookup"><span data-stu-id="9587d-171">The result is to calculate a moving average over the past 5 minutes.</span></span>

<span data-ttu-id="9587d-172">In der hier gezeigten Architektur werden nur die Ergebnisse des Stream Analytics-Auftrags in Cosmos DB gespeichert.</span><span class="sxs-lookup"><span data-stu-id="9587d-172">In the architecture shown here, only the results of the Stream Analytics job are saved to Cosmos DB.</span></span> <span data-ttu-id="9587d-173">Für ein Big Data-Szenario sollten Sie auch die Verwendung von [Event Hubs Capture](/azure/event-hubs/event-hubs-capture-overview) zum Speichern der Rohereignisdaten in Azure Blob Storage in Erwägung ziehen.</span><span class="sxs-lookup"><span data-stu-id="9587d-173">For a big data scenario, consider also using [Event Hubs Capture](/azure/event-hubs/event-hubs-capture-overview) to save the raw event data into Azure Blob storage.</span></span> <span data-ttu-id="9587d-174">Das Speichern der Rohdaten bietet Ihnen die Möglichkeit, zu einem späteren Zeitpunkt Batchabfragen für Ihre historischen Daten auszuführen, um neue Erkenntnisse aus den Daten zu gewinnen.</span><span class="sxs-lookup"><span data-stu-id="9587d-174">Keeping the raw data will allow you to run batch queries over your historical data at later time, in order to derive new insights from the data.</span></span>

## <a name="scalability-considerations"></a><span data-ttu-id="9587d-175">Überlegungen zur Skalierbarkeit</span><span class="sxs-lookup"><span data-stu-id="9587d-175">Scalability considerations</span></span>

### <a name="event-hubs"></a><span data-ttu-id="9587d-176">Event Hubs</span><span class="sxs-lookup"><span data-stu-id="9587d-176">Event Hubs</span></span>

<span data-ttu-id="9587d-177">Die Durchsatzkapazität von Event Hubs wird in [Durchsatzeinheiten](/azure/event-hubs/event-hubs-features#throughput-units) gemessen.</span><span class="sxs-lookup"><span data-stu-id="9587d-177">The throughput capacity of Event Hubs is measured in [throughput units](/azure/event-hubs/event-hubs-features#throughput-units).</span></span> <span data-ttu-id="9587d-178">Sie können einen Event Hub automatisch skalieren, indem Sie die Funktion für [automatische Vergrößerung](/azure/event-hubs/event-hubs-auto-inflate) aktivieren. Dadurch werden die Durchsatzeinheiten basierend auf dem Datenverkehr automatisch bis zu einem konfigurierten Höchstwert hochskaliert.</span><span class="sxs-lookup"><span data-stu-id="9587d-178">You can autoscale an event hub by enabling [auto-inflate](/azure/event-hubs/event-hubs-auto-inflate), which automatically scales the throughput units based on traffic, up to a configured maximum.</span></span> 

### <a name="stream-analytics"></a><span data-ttu-id="9587d-179">Stream Analytics</span><span class="sxs-lookup"><span data-stu-id="9587d-179">Stream Analytics</span></span>

<span data-ttu-id="9587d-180">Für Stream Analytics werden die Computingressourcen, die einem Auftrag zugeordnet sind, in Streamingeinheiten gemessen.</span><span class="sxs-lookup"><span data-stu-id="9587d-180">For Stream Analytics, the computing resources allocated to a job are measured in Streaming Units.</span></span> <span data-ttu-id="9587d-181">Stream Analytics-Aufträge lassen sich am besten skalieren, wenn der Auftrag parallelisiert werden kann.</span><span class="sxs-lookup"><span data-stu-id="9587d-181">Stream Analytics jobs scale best if the job can be parallelized.</span></span> <span data-ttu-id="9587d-182">Auf diese Weise kann Stream Analytics den Auftrag auf mehrere Computeknoten verteilen.</span><span class="sxs-lookup"><span data-stu-id="9587d-182">That way, Stream Analytics can distribute the job across multiple compute nodes.</span></span>

<span data-ttu-id="9587d-183">Verwenden Sie für die Event Hubs-Eingabe das Schlüsselwort `PARTITION BY`, um den Stream Analytics-Auftrag zu partitionieren.</span><span class="sxs-lookup"><span data-stu-id="9587d-183">For Event Hubs input, use the `PARTITION BY` keyword to partition the Stream Analytics job.</span></span> <span data-ttu-id="9587d-184">Die Daten werden basierend auf den Event Hubs-Partitionen in Teilmengen aufgeteilt.</span><span class="sxs-lookup"><span data-stu-id="9587d-184">The data will be divided into subsets based on the Event Hubs partitions.</span></span> 

<span data-ttu-id="9587d-185">Windowing-Funktionen und temporale Verknüpfungen erfordern zusätzliche Streamingeinheiten (SU).</span><span class="sxs-lookup"><span data-stu-id="9587d-185">Windowing functions and temporal joins require additional SU.</span></span> <span data-ttu-id="9587d-186">Verwenden Sie nach Möglichkeit `PARTITION BY`, sodass jede Partition separat verarbeitet wird.</span><span class="sxs-lookup"><span data-stu-id="9587d-186">When possible, use `PARTITION BY` so that each partition is processed separately.</span></span> <span data-ttu-id="9587d-187">Weitere Informationen finden Sie unter [Überblick über Streamingeinheiten und Informationen zu Anpassungen](/azure/stream-analytics/stream-analytics-streaming-unit-consumption#windowed-aggregates).</span><span class="sxs-lookup"><span data-stu-id="9587d-187">For more information, see [Understand and adjust Streaming Units](/azure/stream-analytics/stream-analytics-streaming-unit-consumption#windowed-aggregates).</span></span>

<span data-ttu-id="9587d-188">Falls es nicht möglich ist, den gesamten Stream Analytics-Auftrag zu parallelisieren, versuchen Sie, den Auftrag beginnend mit einem oder mehreren parallelen Schritten in mehrere Schritte zu unterteilen.</span><span class="sxs-lookup"><span data-stu-id="9587d-188">If it's not possible to parallelize the entire Stream Analytics job, try to break the job into multiple steps, starting with one or more parallel steps.</span></span> <span data-ttu-id="9587d-189">So können die ersten Schritte parallel ausgeführt werden.</span><span class="sxs-lookup"><span data-stu-id="9587d-189">That way, the first steps can run in parallel.</span></span> <span data-ttu-id="9587d-190">Für die hier gezeigte Referenzarchitektur gilt beispielsweise Folgendes:</span><span class="sxs-lookup"><span data-stu-id="9587d-190">For example, in this reference architecture:</span></span>

- <span data-ttu-id="9587d-191">Die Schritte 1 und 2 sind einfache `SELECT`-Anweisungen, die Datensätze in einer einzelnen Partition auswählen.</span><span class="sxs-lookup"><span data-stu-id="9587d-191">Steps 1 and 2 are simple `SELECT` statements that select records within a single partition.</span></span> 
- <span data-ttu-id="9587d-192">Schritt 3 führt eine partitionierte Verknüpfung für zwei Eingabedatenströme durch.</span><span class="sxs-lookup"><span data-stu-id="9587d-192">Step 3 performs a partitioned join across two input streams.</span></span> <span data-ttu-id="9587d-193">Dieser Schritt macht sich die Tatsache zu Nutze, dass übereinstimmende Datensätze den gleichen Partitionsschlüssel aufweisen und daher immer in jedem Eingabedatenstrom die gleiche Partitions-ID haben.</span><span class="sxs-lookup"><span data-stu-id="9587d-193">This step takes advantage of the fact that matching records share the same partition key, and so are guaranteed to have the same partition ID in each input stream.</span></span>
- <span data-ttu-id="9587d-194">Schritt 4 aggregiert Daten über alle Partitionen hinweg.</span><span class="sxs-lookup"><span data-stu-id="9587d-194">Step 4 aggregates across all of the partitions.</span></span> <span data-ttu-id="9587d-195">Dieser Schritt kann nicht parallelisiert werden.</span><span class="sxs-lookup"><span data-stu-id="9587d-195">This step cannot be parallelized.</span></span>

<span data-ttu-id="9587d-196">Verwenden Sie das Stream Analytics-[Auftragsdiagramm](/azure/stream-analytics/stream-analytics-job-diagram-with-metrics), um festzustellen, wie viele Partitionen den einzelnen Schritten im Auftrag zugewiesen sind.</span><span class="sxs-lookup"><span data-stu-id="9587d-196">Use the Stream Analytics [job diagram](/azure/stream-analytics/stream-analytics-job-diagram-with-metrics) to see how many partitions are assigned to each step in the job.</span></span> <span data-ttu-id="9587d-197">Das folgende Diagramm zeigt das Auftragsdiagramm für diese Referenzarchitektur:</span><span class="sxs-lookup"><span data-stu-id="9587d-197">The following diagram shows the job diagram for this reference architecture:</span></span>

![](./images/stream-processing-asa/job-diagram.png)

### <a name="cosmos-db"></a><span data-ttu-id="9587d-198">Cosmos DB</span><span class="sxs-lookup"><span data-stu-id="9587d-198">Cosmos DB</span></span>

<span data-ttu-id="9587d-199">Die Durchsatzkapazität für Cosmos DB wird in [Anforderungseinheiten](/azure/cosmos-db/request-units) (RU) gemessen.</span><span class="sxs-lookup"><span data-stu-id="9587d-199">Throughput capacity for Cosmos DB is measured in [Request Units](/azure/cosmos-db/request-units) (RU).</span></span> <span data-ttu-id="9587d-200">Um einen Cosmos DB-Container auf eine Kapazität über 10.000 RU zu skalieren, müssen Sie beim Erstellen des Containers einen [Partitionsschlüssel](/azure/cosmos-db/partition-data) angeben und den Partitionsschlüssel in jedem Dokument hinzufügen.</span><span class="sxs-lookup"><span data-stu-id="9587d-200">In order to scale a Cosmos DB container past 10,000 RU, you must specify a [partition key](/azure/cosmos-db/partition-data) when you create the container, and include the partition key in every document.</span></span> 

<span data-ttu-id="9587d-201">In dieser Referenzarchitektur werden neue Dokumente nur einmal pro Minute (Intervall des springenden Fensters) erstellt, sodass die Durchsatzanforderungen relativ gering sind.</span><span class="sxs-lookup"><span data-stu-id="9587d-201">In this reference architecture, new documents are created only once per minute (the hopping window interval), so the throughput requirements are quite low.</span></span> <span data-ttu-id="9587d-202">In diesem Szenario ist es daher nicht erforderlich, einen Partitionsschlüssel zuzuweisen.</span><span class="sxs-lookup"><span data-stu-id="9587d-202">For that reason, there's no need to assign a partition key in this scenario.</span></span>

## <a name="monitoring-considerations"></a><span data-ttu-id="9587d-203">Aspekte der Überwachung</span><span class="sxs-lookup"><span data-stu-id="9587d-203">Monitoring considerations</span></span>

<span data-ttu-id="9587d-204">Bei jeder Lösung zur Verarbeitung von Datenströmen ist es wichtig, die Leistung und Integrität des Systems zu überwachen.</span><span class="sxs-lookup"><span data-stu-id="9587d-204">With any stream processing solution, it's important to monitor the performance and health of the system.</span></span> <span data-ttu-id="9587d-205">[Azure Monitor](/azure/monitoring-and-diagnostics/) erfasst Metriken und Diagnoseprotokolle für die in der Architektur verwendeten Azure-Dienste.</span><span class="sxs-lookup"><span data-stu-id="9587d-205">[Azure Monitor](/azure/monitoring-and-diagnostics/) collects metrics and diagnostics logs for the Azure services used in the architecture.</span></span> <span data-ttu-id="9587d-206">Azure Monitor ist in die Azure-Plattform integriert und erfordert keinen zusätzlichen Code in Ihrer Anwendung.</span><span class="sxs-lookup"><span data-stu-id="9587d-206">Azure Monitor is built into the Azure platform and does not require any additional code in your application.</span></span>

<span data-ttu-id="9587d-207">Jedes der folgenden Warnsignale weist darauf hin, dass Sie die jeweilige Azure-Ressource horizontal hochskalieren sollten:</span><span class="sxs-lookup"><span data-stu-id="9587d-207">Any of the following warning signals indicate that you should scale out the relevant Azure resource:</span></span>

- <span data-ttu-id="9587d-208">Event Hubs drosselt Anforderungen oder nähert sich dem täglichen Nachrichtenkontingent.</span><span class="sxs-lookup"><span data-stu-id="9587d-208">Event Hubs throttles requests or is close to the daily message quota.</span></span>
- <span data-ttu-id="9587d-209">Der Stream Analytics-Auftrag nutzt regelmäßig mehr als 80 % der zugeordneten Streamingeinheiten (SU).</span><span class="sxs-lookup"><span data-stu-id="9587d-209">The Stream Analytics job consistently uses more than 80% of allocated Streaming Units (SU).</span></span>
- <span data-ttu-id="9587d-210">Cosmos DB beginnt, Anforderungen zu drosseln.</span><span class="sxs-lookup"><span data-stu-id="9587d-210">Cosmos DB begins to throttle requests.</span></span>

<span data-ttu-id="9587d-211">Die Referenzarchitektur enthält ein im Azure-Portal bereitgestelltes benutzerdefiniertes Dashboard.</span><span class="sxs-lookup"><span data-stu-id="9587d-211">The reference architecture includes a custom dashboard, which is deployed to the Azure portal.</span></span> <span data-ttu-id="9587d-212">Nachdem Sie die Architektur bereitgestellt haben, können Sie das Dashboard anzeigen, indem Sie das [Azure-Portal](https://portal.azure.com) öffnen und `TaxiRidesDashboard` in der Liste der Dashboards auswählen.</span><span class="sxs-lookup"><span data-stu-id="9587d-212">After you deploy the architecture, you can view the dashboard by opening the [Azure Portal](https://portal.azure.com) and selecting `TaxiRidesDashboard` from list of dashboards.</span></span> <span data-ttu-id="9587d-213">Weitere Informationen zum Erstellen und Bereitstellen von benutzerdefinierten Dashboards im Azure-Portal finden Sie unter [Programmgesteuertes Erstellen von Azure-Dashboards](/azure/azure-portal/azure-portal-dashboards-create-programmatically).</span><span class="sxs-lookup"><span data-stu-id="9587d-213">For more information about creating and deploying custom dashboards in the Azure portal, see [Programmatically create Azure Dashboards](/azure/azure-portal/azure-portal-dashboards-create-programmatically).</span></span>

<span data-ttu-id="9587d-214">Die folgende Abbildung zeigt das Dashboard, nachdem der Stream Analytics-Auftrag etwa eine Stunde lang ausgeführt wurde.</span><span class="sxs-lookup"><span data-stu-id="9587d-214">The following image shows the dashboard after the Stream Analytics job ran for about an hour.</span></span>

![](./images/stream-processing-asa/asa-dashboard.png)

<span data-ttu-id="9587d-215">Im unteren linken Bereich können Sie sehen, dass der SU-Verbrauch für den Stream Analytics-Auftrag während der ersten 15 Minuten ansteigt und dann gleichbleibt.</span><span class="sxs-lookup"><span data-stu-id="9587d-215">The panel on the lower left shows that the SU consumption for the Stream Analytics job climbs during the first 15 minutes and then levels off.</span></span> <span data-ttu-id="9587d-216">Dies ist ein typisches Muster, da der Auftrag einen stabilen Zustand erreicht.</span><span class="sxs-lookup"><span data-stu-id="9587d-216">This is a typical pattern as the job reaches a steady state.</span></span> 

<span data-ttu-id="9587d-217">Beachten Sie, dass Event Hubs Anforderungen drosselt. Dies können Sie im oberen rechten Bereich sehen.</span><span class="sxs-lookup"><span data-stu-id="9587d-217">Notice that Event Hubs is throttling requests, shown in the upper right panel.</span></span> <span data-ttu-id="9587d-218">Das gelegentliche Drosseln einer Anforderung stellt kein Problem dar, da das Event Hubs-Client-SDK den Vorgang automatisch wiederholt, wenn es einen Drosselungsfehler empfängt.</span><span class="sxs-lookup"><span data-stu-id="9587d-218">An occasional throttled request is not a problem, because the Event Hubs client SDK automatically retries when it receives a throttling error.</span></span> <span data-ttu-id="9587d-219">Wenn jedoch regelmäßig Drosselungsfehler auftreten, bedeutet dies, dass der Event Hub mehr Durchsatzeinheiten benötigt.</span><span class="sxs-lookup"><span data-stu-id="9587d-219">However, if you see consistent throttling errors, it means the event hub needs more throughput units.</span></span> <span data-ttu-id="9587d-220">Das folgende Diagramm zeigt einen Testlauf mit der Event Hubs-Funktion zur automatischen Vergrößerung, die je nach Bedarf die Durchsatzeinheiten automatisch horizontal hochskaliert.</span><span class="sxs-lookup"><span data-stu-id="9587d-220">The following graph shows a test run using the Event Hubs auto-inflate feature, which automatically scales out the throughput units as needed.</span></span> 

![](./images/stream-processing-asa/stream-processing-eh-autoscale.png)

<span data-ttu-id="9587d-221">Die automatische Vergrößerung wurde ungefähr bei 06:35 aktiviert.</span><span class="sxs-lookup"><span data-stu-id="9587d-221">Auto-inflate was enabled at about the 06:35 mark.</span></span> <span data-ttu-id="9587d-222">Wie Sie sehen, hat die Anzahl gedrosselter Anforderungen abgenommen, da Event Hubs automatisch auf drei Durchsatzeinheiten hochskaliert wurde.</span><span class="sxs-lookup"><span data-stu-id="9587d-222">You can see the p drop in throttled requests, as Event Hubs automatically scaled up to 3 throughput units.</span></span>

<span data-ttu-id="9587d-223">Als Nebenwirkung erhöhte sich dadurch interessanterweise die SU-Nutzung im Stream Analytics-Auftrag.</span><span class="sxs-lookup"><span data-stu-id="9587d-223">Interestingly, this had the side effect of increasing the SU utilization in the Stream Analytics job.</span></span> <span data-ttu-id="9587d-224">Durch die Drosselung hat Event Hubs die Erfassungsrate für den Stream Analytics-Auftrag künstlich reduziert.</span><span class="sxs-lookup"><span data-stu-id="9587d-224">By throttling, Event Hubs was artificially reducing the ingestion rate for the Stream Analytics job.</span></span> <span data-ttu-id="9587d-225">Tatsächlich kommt es häufig vor, dass durch die Behebung eines Leistungsengpasses ein anderer aufgedeckt wird.</span><span class="sxs-lookup"><span data-stu-id="9587d-225">It's actually common that resolving one performance bottleneck reveals another.</span></span> <span data-ttu-id="9587d-226">In diesem Fall konnte das Problem durch die Zuordnung zusätzlicher Streamingeinheiten für den Stream Analytics-Auftrag behoben werden.</span><span class="sxs-lookup"><span data-stu-id="9587d-226">In this case, allocating additional SU for the Stream Analytics job resolved the issue.</span></span>

## <a name="deploy-the-solution"></a><span data-ttu-id="9587d-227">Bereitstellen der Lösung</span><span class="sxs-lookup"><span data-stu-id="9587d-227">Deploy the solution</span></span>

<span data-ttu-id="9587d-228">Eine Bereitstellung für diese Referenzarchitektur ist auf [GitHub](https://github.com/mspnp/reference-architectures/tree/master/data) verfügbar.</span><span class="sxs-lookup"><span data-stu-id="9587d-228">A deployment for this reference architecture is available on [GitHub](https://github.com/mspnp/reference-architectures/tree/master/data).</span></span> 

### <a name="prerequisites"></a><span data-ttu-id="9587d-229">Voraussetzungen</span><span class="sxs-lookup"><span data-stu-id="9587d-229">Prerequisites</span></span>

1. <span data-ttu-id="9587d-230">Klonen oder Forken Sie das GitHub-Repository [Referenzarchitekturen](https://github.com/mspnp/reference-architectures), oder laden Sie die entsprechende ZIP-Datei herunter.</span><span class="sxs-lookup"><span data-stu-id="9587d-230">Clone, fork, or download the zip file for the [reference architectures](https://github.com/mspnp/reference-architectures) GitHub repository.</span></span>

2. <span data-ttu-id="9587d-231">Installieren Sie [Docker](https://www.docker.com/), um den Datengenerator auszuführen.</span><span class="sxs-lookup"><span data-stu-id="9587d-231">Install [Docker](https://www.docker.com/) to run the data generator.</span></span>

3. <span data-ttu-id="9587d-232">Installieren Sie [Azure CLI 2.0](/cli/azure/install-azure-cli?view=azure-cli-latest).</span><span class="sxs-lookup"><span data-stu-id="9587d-232">Install [Azure CLI 2.0](/cli/azure/install-azure-cli?view=azure-cli-latest).</span></span>

4. <span data-ttu-id="9587d-233">Melden Sie sich über eine Eingabeaufforderung, eine Bash-Eingabeaufforderung oder die PowerShell-Eingabeaufforderung folgendermaßen bei Ihrem Azure-Konto an:</span><span class="sxs-lookup"><span data-stu-id="9587d-233">From a command prompt, bash prompt, or PowerShell prompt, sign into your Azure account as follows:</span></span>

    ```
    az login
    ```

### <a name="download-the-source-data-files"></a><span data-ttu-id="9587d-234">Herunterladen der Quelldatendateien</span><span class="sxs-lookup"><span data-stu-id="9587d-234">Download the source data files</span></span>

1. <span data-ttu-id="9587d-235">Erstellen Sie ein Verzeichnis mit dem Namen `DataFile` im Verzeichnis `data/streaming_asa` im GitHub-Repository.</span><span class="sxs-lookup"><span data-stu-id="9587d-235">Create a directory named `DataFile` under the `data/streaming_asa` directory in the GitHub repo.</span></span>

2. <span data-ttu-id="9587d-236">Öffnen Sie einen Webbrowser, und navigieren Sie zu https://uofi.app.box.com/v/NYCtaxidata/folder/2332219935.</span><span class="sxs-lookup"><span data-stu-id="9587d-236">Open a web browser and navigate to https://uofi.app.box.com/v/NYCtaxidata/folder/2332219935.</span></span>

3. <span data-ttu-id="9587d-237">Klicken Sie auf dieser Seite auf die Schaltfläche zum **Herunterladen**, um eine ZIP-Datei mit den Taxidaten für dieses Jahr herunterzuladen.</span><span class="sxs-lookup"><span data-stu-id="9587d-237">Click the **Download** button on this page to download a zip file of all the taxi data for that year.</span></span>

4. <span data-ttu-id="9587d-238">Extrahieren Sie die ZIP-Datei in das Verzeichnis `DataFile`.</span><span class="sxs-lookup"><span data-stu-id="9587d-238">Extract the zip file to the `DataFile` directory.</span></span>

    > [!NOTE]
    > <span data-ttu-id="9587d-239">Diese ZIP-Datei enthält weitere ZIP-Dateien.</span><span class="sxs-lookup"><span data-stu-id="9587d-239">This zip file contains other zip files.</span></span> <span data-ttu-id="9587d-240">Extrahieren Sie die untergeordneten ZIP-Dateien nicht.</span><span class="sxs-lookup"><span data-stu-id="9587d-240">Don't extract the child zip files.</span></span>

<span data-ttu-id="9587d-241">Die Verzeichnisstruktur sollte wie folgt aussehen:</span><span class="sxs-lookup"><span data-stu-id="9587d-241">The directory structure should look like the following:</span></span>

```
/data
    /streaming_asa
        /DataFile
            /FOIL2013
                trip_data_1.zip
                trip_data_2.zip
                trip_data_3.zip
                ...
```

### <a name="deploy-the-azure-resources"></a><span data-ttu-id="9587d-242">Bereitstellen der Azure-Ressourcen</span><span class="sxs-lookup"><span data-stu-id="9587d-242">Deploy the Azure resources</span></span>

1. <span data-ttu-id="9587d-243">Führen Sie an einer Shell- oder Windows-Eingabeaufforderung den folgenden Befehl aus, und folgen Sie den Anweisungen in der Anmeldeaufforderung:</span><span class="sxs-lookup"><span data-stu-id="9587d-243">From a shell or Windows Command Prompt, run the following command and follow the sign-in prompt:</span></span>

    ```bash
    az login
    ```

2. <span data-ttu-id="9587d-244">Navigieren Sie zum Ordner `data/streaming_asa` im GitHub-Repository.</span><span class="sxs-lookup"><span data-stu-id="9587d-244">Navigate to the folder `data/streaming_asa` in the GitHub repository</span></span>

    ```bash
    cd data/streaming_asa
    ```

2. <span data-ttu-id="9587d-245">Führen Sie die folgenden Befehle aus, um die Azure-Ressourcen bereitzustellen:</span><span class="sxs-lookup"><span data-stu-id="9587d-245">Run the following commands to deploy the Azure resources:</span></span>

    ```bash
    export resourceGroup='[Resource group name]'
    export resourceLocation='[Location]'
    export cosmosDatabaseAccount='[Cosmos DB account name]'
    export cosmosDatabase='[Cosmod DB database name]'
    export cosmosDataBaseCollection='[Cosmos DB collection name]'
    export eventHubNamespace='[Event Hubs namespace name]'

    # Create a resource group
    az group create --name $resourceGroup --location $resourceLocation

    # Deploy resources
    az group deployment create --resource-group $resourceGroup \
      --template-file ./azure/deployresources.json --parameters \
      eventHubNamespace=$eventHubNamespace \
      outputCosmosDatabaseAccount=$cosmosDatabaseAccount \
      outputCosmosDatabase=$cosmosDatabase \
      outputCosmosDatabaseCollection=$cosmosDataBaseCollection

    # Create a database 
    az cosmosdb database create --name $cosmosDatabaseAccount \
        --db-name $cosmosDatabase --resource-group $resourceGroup

    # Create a collection
    az cosmosdb collection create --collection-name $cosmosDataBaseCollection \
        --name $cosmosDatabaseAccount --db-name $cosmosDatabase \
        --resource-group $resourceGroup
    ```

3. <span data-ttu-id="9587d-246">Navigieren Sie im Azure-Portal zu der Ressourcengruppe, die erstellt wurde.</span><span class="sxs-lookup"><span data-stu-id="9587d-246">In the Azure portal, navigate to the resource group that was created.</span></span>

4. <span data-ttu-id="9587d-247">Öffnen Sie das Blatt für den Stream Analytics-Auftrag.</span><span class="sxs-lookup"><span data-stu-id="9587d-247">Open the blade for the Stream Analytics job.</span></span>

5. <span data-ttu-id="9587d-248">Klicken Sie auf **Starten**, um den Auftrag zu starten.</span><span class="sxs-lookup"><span data-stu-id="9587d-248">Click **Start** to start the job.</span></span> <span data-ttu-id="9587d-249">Wählen Sie **Jetzt** als Startzeit der Ausgabe aus.</span><span class="sxs-lookup"><span data-stu-id="9587d-249">Select **Now** as the output start time.</span></span> <span data-ttu-id="9587d-250">Warten Sie, bis der Auftrag gestartet wird.</span><span class="sxs-lookup"><span data-stu-id="9587d-250">Wait for the job to start.</span></span>

### <a name="run-the-data-generator"></a><span data-ttu-id="9587d-251">Ausführen des Datengenerators</span><span class="sxs-lookup"><span data-stu-id="9587d-251">Run the data generator</span></span>

1. <span data-ttu-id="9587d-252">Rufen Sie die Event Hub-Verbindungszeichenfolgen ab.</span><span class="sxs-lookup"><span data-stu-id="9587d-252">Get the Event Hub connection strings.</span></span> <span data-ttu-id="9587d-253">Sie können die Verbindungszeichenfolgen über das Azure-Portal oder durch Ausführen der folgenden CLI-Befehle abrufen:</span><span class="sxs-lookup"><span data-stu-id="9587d-253">You can get these from the Azure portal, or by running the following CLI commands:</span></span>

    ```bash
    # RIDE_EVENT_HUB
    az eventhubs eventhub authorization-rule keys list \
        --eventhub-name taxi-ride \
        --name taxi-ride-asa-access-policy \
        --namespace-name $eventHubNamespace \
        --resource-group $resourceGroup \
        --query primaryConnectionString

    # FARE_EVENT_HUB
    az eventhubs eventhub authorization-rule keys list \
        --eventhub-name taxi-fare \
        --name taxi-fare-asa-access-policy \
        --namespace-name $eventHubNamespace \
        --resource-group $resourceGroup \
        --query primaryConnectionString
    ```

2. <span data-ttu-id="9587d-254">Navigieren Sie zum Verzeichnis `data/streaming_asa/onprem` im GitHub-Repository.</span><span class="sxs-lookup"><span data-stu-id="9587d-254">Navigate to the directory `data/streaming_asa/onprem` in the GitHub repository</span></span>

3. <span data-ttu-id="9587d-255">Aktualisieren Sie die Werte in der Datei `main.env` wie folgt:</span><span class="sxs-lookup"><span data-stu-id="9587d-255">Update the values in the file `main.env` as follows:</span></span>

    ```
    RIDE_EVENT_HUB=[Connection string for taxi-ride event hub]
    FARE_EVENT_HUB=[Connection string for taxi-fare event hub]
    RIDE_DATA_FILE_PATH=/DataFile/FOIL2013
    MINUTES_TO_LEAD=0
    PUSH_RIDE_DATA_FIRST=false
    ```

4. <span data-ttu-id="9587d-256">Führen Sie den folgenden Befehl aus, um das Docker-Image zu erstellen.</span><span class="sxs-lookup"><span data-stu-id="9587d-256">Run the following command to build the Docker image.</span></span>

    ```bash
    docker build --no-cache -t dataloader .
    ```

5. <span data-ttu-id="9587d-257">Navigieren Sie zurück zum übergeordneten Verzeichnis `data/stream_asa`.</span><span class="sxs-lookup"><span data-stu-id="9587d-257">Navigate back to the parent directory, `data/stream_asa`.</span></span>

    ```bash
    cd ..
    ```

6. <span data-ttu-id="9587d-258">Führen Sie den folgenden Befehl aus, um das Docker-Image auszuführen.</span><span class="sxs-lookup"><span data-stu-id="9587d-258">Run the following command to run the Docker image.</span></span>

    ```bash
    docker run -v `pwd`/DataFile:/DataFile --env-file=onprem/main.env dataloader:latest
    ```

<span data-ttu-id="9587d-259">Die Ausgabe sollte wie folgt aussehen:</span><span class="sxs-lookup"><span data-stu-id="9587d-259">The output should look like the following:</span></span>

```
Created 10000 records for TaxiFare
Created 10000 records for TaxiRide
Created 20000 records for TaxiFare
Created 20000 records for TaxiRide
Created 30000 records for TaxiFare
...
```

<span data-ttu-id="9587d-260">Lassen Sie das Programm mindestens fünf Minuten lang laufen (dies ist das in der Stream Analytics-Abfrage definierte Zeitfenster).</span><span class="sxs-lookup"><span data-stu-id="9587d-260">Let the program run for at least 5 minutes, which is the window defined in the Stream Analytics query.</span></span> <span data-ttu-id="9587d-261">Um zu überprüfen, ob der Stream Analytics-Auftrag korrekt ausgeführt wird, öffnen Sie das Azure-Portal, und navigieren Sie zur Cosmos DB-Datenbank.</span><span class="sxs-lookup"><span data-stu-id="9587d-261">To verify the Stream Analytics job is running correctly, open the Azure portal and navigate to the Cosmos DB database.</span></span> <span data-ttu-id="9587d-262">Öffnen Sie das Blatt **Daten-Explorer**, und zeigen Sie die Dokumente an.</span><span class="sxs-lookup"><span data-stu-id="9587d-262">Open the **Data Explorer** blade and view the documents.</span></span> 

<span data-ttu-id="9587d-263">[1] <span id="note1">Donovan, Brian; Work, Dan (2016): New York City Taxi Trip Data (2010 – 2013).</span><span class="sxs-lookup"><span data-stu-id="9587d-263">[1] <span id="note1">Donovan, Brian; Work, Dan (2016): New York City Taxi Trip Data (2010-2013).</span></span> <span data-ttu-id="9587d-264">Universität Illinois in Urbana-Champaign.</span><span class="sxs-lookup"><span data-stu-id="9587d-264">University of Illinois at Urbana-Champaign.</span></span> <span data-ttu-id="9587d-265">https://doi.org/10.13012/J8PN93H8</span><span class="sxs-lookup"><span data-stu-id="9587d-265">https://doi.org/10.13012/J8PN93H8</span></span>
