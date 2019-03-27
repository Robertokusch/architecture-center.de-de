---
title: Datenstromverarbeitung mit Azure Databricks
titleSuffix: Azure Reference Architectures
description: Erstellen einer End-to-End-Pipeline zur Datenstromverarbeitung in Azure mit Azure Databricks
author: petertaylor9999
ms.date: 11/30/2018
ms.topic: reference-architecture
ms.service: architecture-center
ms.subservice: reference-architecture
ms.custom: seodec18
ms.openlocfilehash: 748b191aeee931d580dd27b1ad54c4f4bd63e369
ms.sourcegitcommit: c053e6edb429299a0ad9b327888d596c48859d4a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/20/2019
ms.locfileid: "58243841"
---
# <a name="create-a-stream-processing-pipeline-with-azure-databricks"></a><span data-ttu-id="cbc98-103">Erstellen einer Pipeline zur Datenstromverarbeitung mit Azure Databricks</span><span class="sxs-lookup"><span data-stu-id="cbc98-103">Create a stream processing pipeline with Azure Databricks</span></span>

<span data-ttu-id="cbc98-104">Diese Referenzarchitektur zeigt eine End-to-End-Pipeline zur [Datenstromverarbeitung](/azure/architecture/data-guide/big-data/real-time-processing).</span><span class="sxs-lookup"><span data-stu-id="cbc98-104">This reference architecture shows an end-to-end [stream processing](/azure/architecture/data-guide/big-data/real-time-processing) pipeline.</span></span> <span data-ttu-id="cbc98-105">Dieser Pipelinetyp hat vier Phasen: Erfassung, Verarbeitung, Speicherung und Analyse/Berichterstellung.</span><span class="sxs-lookup"><span data-stu-id="cbc98-105">This type of pipeline has four stages: ingest, process, store, and analysis and reporting.</span></span> <span data-ttu-id="cbc98-106">In dieser Referenzarchitektur erfasst die Pipeline Daten aus zwei Quellen, verknüpft verwandte Datensätze aus den einzelnen Datenströmen, reichert das Ergebnis an und berechnet einen Durchschnitt in Echtzeit.</span><span class="sxs-lookup"><span data-stu-id="cbc98-106">For this reference architecture, the pipeline ingests data from two sources, performs a join on related records from each stream, enriches the result, and calculates an average in real time.</span></span> <span data-ttu-id="cbc98-107">Die Ergebnisse werden zur weiteren Analyse gespeichert.</span><span class="sxs-lookup"><span data-stu-id="cbc98-107">The results are stored for further analysis.</span></span> <span data-ttu-id="cbc98-108">[**Stellen Sie diese Lösung bereit**](#deploy-the-solution).</span><span class="sxs-lookup"><span data-stu-id="cbc98-108">[**Deploy this solution**](#deploy-the-solution).</span></span>

![Referenzarchitektur für Datenstromverarbeitung mit Azure Databricks](./images/stream-processing-databricks.png)

<span data-ttu-id="cbc98-110">**Szenario:** Ein Taxiunternehmen erfasst Daten zu jeder Taxifahrt.</span><span class="sxs-lookup"><span data-stu-id="cbc98-110">**Scenario**: A taxi company collects data about each taxi trip.</span></span> <span data-ttu-id="cbc98-111">In diesem Szenario gehen wir davon aus, dass zwei separate Geräte Daten senden.</span><span class="sxs-lookup"><span data-stu-id="cbc98-111">For this scenario, we assume there are two separate devices sending data.</span></span> <span data-ttu-id="cbc98-112">Das Taxi verfügt ein Messgerät, das Informationen zu jeder Fahrt sendet – die Dauer, die Strecke sowie die Abhol- und Zielorte.</span><span class="sxs-lookup"><span data-stu-id="cbc98-112">The taxi has a meter that sends information about each ride &mdash; the duration, distance, and pickup and dropoff locations.</span></span> <span data-ttu-id="cbc98-113">Ein separates Gerät akzeptiert Zahlungen von Kunden und sendet Daten zu den Fahrpreisen.</span><span class="sxs-lookup"><span data-stu-id="cbc98-113">A separate device accepts payments from customers and sends data about fares.</span></span> <span data-ttu-id="cbc98-114">Das Taxiunternehmen möchte Fahrgasttrends ermitteln und dazu für jedes Stadtviertel in Echtzeit das durchschnittliche Trinkgeld pro gefahrener Meile berechnen.</span><span class="sxs-lookup"><span data-stu-id="cbc98-114">To spot ridership trends, the taxi company wants to calculate the average tip per mile driven, in real time, for each neighborhood.</span></span>

## <a name="architecture"></a><span data-ttu-id="cbc98-115">Architecture</span><span class="sxs-lookup"><span data-stu-id="cbc98-115">Architecture</span></span>

<span data-ttu-id="cbc98-116">Die Architektur umfasst die folgenden Komponenten.</span><span class="sxs-lookup"><span data-stu-id="cbc98-116">The architecture consists of the following components.</span></span>

<span data-ttu-id="cbc98-117">**Datenquellen:**</span><span class="sxs-lookup"><span data-stu-id="cbc98-117">**Data sources**.</span></span> <span data-ttu-id="cbc98-118">In dieser Architektur gibt es zwei Datenquellen, die Datenströme in Echtzeit generieren.</span><span class="sxs-lookup"><span data-stu-id="cbc98-118">In this architecture, there are two data sources that generate data streams in real time.</span></span> <span data-ttu-id="cbc98-119">Der erste Datenstrom enthält Informationen zur Fahrt und der zweite Informationen zum Fahrpreis.</span><span class="sxs-lookup"><span data-stu-id="cbc98-119">The first stream contains ride information, and the second contains fare information.</span></span> <span data-ttu-id="cbc98-120">Die Referenzarchitektur umfasst einen simulierten Datengenerator, der aus einem Satz von statischen Dateien liest und die Daten an Event Hubs pusht.</span><span class="sxs-lookup"><span data-stu-id="cbc98-120">The reference architecture includes a simulated data generator that reads from a set of static files and pushes the data to Event Hubs.</span></span> <span data-ttu-id="cbc98-121">Bei einer echten Anwendung stammen die Daten von Geräten, die in den Taxis installiert sind.</span><span class="sxs-lookup"><span data-stu-id="cbc98-121">The data sources in a real application would be devices installed in the taxi cabs.</span></span>

<span data-ttu-id="cbc98-122">**Azure Event Hubs**:</span><span class="sxs-lookup"><span data-stu-id="cbc98-122">**Azure Event Hubs**.</span></span> <span data-ttu-id="cbc98-123">[Event Hubs](/azure/event-hubs/) ist ein Ereigniserfassungsdienst.</span><span class="sxs-lookup"><span data-stu-id="cbc98-123">[Event Hubs](/azure/event-hubs/) is an event ingestion service.</span></span> <span data-ttu-id="cbc98-124">Die hier gezeigte Architektur verwendet zwei Event Hub-Instanzen, eine für jede Datenquelle.</span><span class="sxs-lookup"><span data-stu-id="cbc98-124">This architecture uses two event hub instances, one for each data source.</span></span> <span data-ttu-id="cbc98-125">Jede Datenquelle sendet einen Datenstrom an den zugehörigen Event Hub.</span><span class="sxs-lookup"><span data-stu-id="cbc98-125">Each data source sends a stream of data to the associated event hub.</span></span>

<span data-ttu-id="cbc98-126">**Azure Databricks**:</span><span class="sxs-lookup"><span data-stu-id="cbc98-126">**Azure Databricks**.</span></span> <span data-ttu-id="cbc98-127">[Databricks](/azure/azure-databricks/) ist eine Apache Spark-basierte Analyseplattform, die für die Microsoft Azure-Clouddienstplattform optimiert ist.</span><span class="sxs-lookup"><span data-stu-id="cbc98-127">[Databricks](/azure/azure-databricks/) is an Apache Spark-based analytics platform optimized for the Microsoft Azure cloud services platform.</span></span> <span data-ttu-id="cbc98-128">Sie wird verwendet, um die Taxifahrtdaten und die Fahrpreisdaten zu korrelieren und die korrelierten Daten mit den gespeicherten Stadtvierteldaten aus dem Databricks-Dateisystem anzureichern.</span><span class="sxs-lookup"><span data-stu-id="cbc98-128">Databricks is used to correlate of the taxi ride and fare data, and also to enrich the correlated data with neighborhood data stored in the Databricks file system.</span></span>

<span data-ttu-id="cbc98-129">**Cosmos DB**:</span><span class="sxs-lookup"><span data-stu-id="cbc98-129">**Cosmos DB**.</span></span> <span data-ttu-id="cbc98-130">Der Azure Databricks-Auftrag gibt eine Reihe von Datensätzen aus, die unter Verwendung der Cassandra-API in [Cosmos DB](/azure/cosmos-db/) geschrieben werden.</span><span class="sxs-lookup"><span data-stu-id="cbc98-130">The output from Azure Databricks job is a series of records, which are written to [Cosmos DB](/azure/cosmos-db/) using the Cassandra API.</span></span> <span data-ttu-id="cbc98-131">Die Cassandra-API wird verwendet, da sie Modelle mit Zeitreihendaten unterstützt.</span><span class="sxs-lookup"><span data-stu-id="cbc98-131">The Cassandra API is used because it supports time series data modeling.</span></span>

<span data-ttu-id="cbc98-132">**Azure Log Analytics**:</span><span class="sxs-lookup"><span data-stu-id="cbc98-132">**Azure Log Analytics**.</span></span> <span data-ttu-id="cbc98-133">Die von [Azure Monitor](/azure/monitoring-and-diagnostics/) erfassten Anwendungsprotokolldaten werden in einem [Log Analytics-Arbeitsbereich](/azure/log-analytics) gespeichert.</span><span class="sxs-lookup"><span data-stu-id="cbc98-133">Application log data collected by [Azure Monitor](/azure/monitoring-and-diagnostics/) is stored in a [Log Analytics workspace](/azure/log-analytics).</span></span> <span data-ttu-id="cbc98-134">Mithilfe von Log Analytics-Abfragen können Metriken analysiert und visualisiert sowie Protokollmeldungen auf Probleme innerhalb der Anwendung untersucht werden.</span><span class="sxs-lookup"><span data-stu-id="cbc98-134">Log Analytics queries can be used to analyze and visualize metrics and inspect log messages to identify issues within the application.</span></span>

## <a name="data-ingestion"></a><span data-ttu-id="cbc98-135">Datenerfassung</span><span class="sxs-lookup"><span data-stu-id="cbc98-135">Data ingestion</span></span>

<!-- markdownlint-disable MD033 -->

<span data-ttu-id="cbc98-136">Um eine Datenquelle zu simulieren, verwendet die Referenzarchitektur das Dataset [New York City Taxi Data](https://uofi.app.box.com/v/NYCtaxidata/folder/2332218797)<sup>[[1]](#note1)</sup>.</span><span class="sxs-lookup"><span data-stu-id="cbc98-136">To simulate a data source, this reference architecture uses the [New York City Taxi Data](https://uofi.app.box.com/v/NYCtaxidata/folder/2332218797) dataset<sup>[[1]](#note1)</sup>.</span></span> <span data-ttu-id="cbc98-137">Dieses Dataset enthält Daten zu Taxifahrten in New York City für einen Zeitraum von vier Jahren (2010&ndash;2013).</span><span class="sxs-lookup"><span data-stu-id="cbc98-137">This dataset contains data about taxi trips in New York City over a four-year period (2010 &ndash; 2013).</span></span> <span data-ttu-id="cbc98-138">Es umfasst zwei Datensatztypen: Fahrtdaten und Fahrpreisdaten.</span><span class="sxs-lookup"><span data-stu-id="cbc98-138">It contains two types of record: Ride data and fare data.</span></span> <span data-ttu-id="cbc98-139">Die Fahrtdaten enthalten die Fahrtdauer, die Fahrtstrecke sowie die Abhol- und Zielorte.</span><span class="sxs-lookup"><span data-stu-id="cbc98-139">Ride data includes trip duration, trip distance, and pickup and dropoff location.</span></span> <span data-ttu-id="cbc98-140">Die Fahrpreisdaten enthalten die Beträge von Fahrpreis, Steuern und Trinkgeld.</span><span class="sxs-lookup"><span data-stu-id="cbc98-140">Fare data includes fare, tax, and tip amounts.</span></span> <span data-ttu-id="cbc98-141">Gemeinsame Felder in beiden Datensatztypen sind die Taxinummer (Medallion), die Taxilizenz (Hack license) und die Anbieter-ID (Vendor ID).</span><span class="sxs-lookup"><span data-stu-id="cbc98-141">Common fields in both record types include medallion number, hack license, and vendor ID.</span></span> <span data-ttu-id="cbc98-142">Anhand dieser drei Felder werden ein Taxi und ein Fahrer eindeutig identifiziert.</span><span class="sxs-lookup"><span data-stu-id="cbc98-142">Together these three fields uniquely identify a taxi plus a driver.</span></span> <span data-ttu-id="cbc98-143">Die Daten werden im CSV-Format gespeichert.</span><span class="sxs-lookup"><span data-stu-id="cbc98-143">The data is stored in CSV format.</span></span>

> <span data-ttu-id="cbc98-144">[1] <span id="note1">Donovan, Brian; Work, Dan (2016): New York City Taxi Trip Data (2010 – 2013).</span><span class="sxs-lookup"><span data-stu-id="cbc98-144">[1] <span id="note1">Donovan, Brian; Work, Dan (2016): New York City Taxi Trip Data (2010-2013).</span></span> <span data-ttu-id="cbc98-145">Universität Illinois in Urbana-Champaign.</span><span class="sxs-lookup"><span data-stu-id="cbc98-145">University of Illinois at Urbana-Champaign.</span></span> <https://doi.org/10.13012/J8PN93H8>

<!-- markdownlint-enable MD033 -->

<span data-ttu-id="cbc98-146">Der Datengenerator ist eine .NET Core-Anwendung, die Datensätze liest und an Azure Event Hubs sendet.</span><span class="sxs-lookup"><span data-stu-id="cbc98-146">The data generator is a .NET Core application that reads the records and sends them to Azure Event Hubs.</span></span> <span data-ttu-id="cbc98-147">Der Generator sendet Fahrtdaten im JSON-Format und Fahrpreisdaten im CSV-Format.</span><span class="sxs-lookup"><span data-stu-id="cbc98-147">The generator sends ride data in JSON format and fare data in CSV format.</span></span>

<span data-ttu-id="cbc98-148">Event Hubs verwendet [Partitionen](/azure/event-hubs/event-hubs-features#partitions) zum Segmentieren der Daten.</span><span class="sxs-lookup"><span data-stu-id="cbc98-148">Event Hubs uses [partitions](/azure/event-hubs/event-hubs-features#partitions) to segment the data.</span></span> <span data-ttu-id="cbc98-149">Partitionen ermöglichen es einem Consumer, die einzelnen Partitionen gleichzeitig zu lesen.</span><span class="sxs-lookup"><span data-stu-id="cbc98-149">Partitions allow a consumer to read each partition in parallel.</span></span> <span data-ttu-id="cbc98-150">Wenn Sie Daten an Event Hubs senden, können Sie den Partitionsschlüssel explizit angeben.</span><span class="sxs-lookup"><span data-stu-id="cbc98-150">When you send data to Event Hubs, you can specify the partition key explicitly.</span></span> <span data-ttu-id="cbc98-151">Andernfalls werden Datensätze nach einem Roundrobinverfahren Partitionen zugewiesen.</span><span class="sxs-lookup"><span data-stu-id="cbc98-151">Otherwise, records are assigned to partitions in round-robin fashion.</span></span>

<span data-ttu-id="cbc98-152">In diesem Szenario sollen Fahrtdaten und Fahrpreisdaten für ein bestimmtes Taxi die gleiche Partitions-ID erhalten.</span><span class="sxs-lookup"><span data-stu-id="cbc98-152">In this scenario, ride data and fare data should end up with the same partition ID for a given taxi cab.</span></span> <span data-ttu-id="cbc98-153">Dies ermöglicht Databricks eine gewisse Parallelität beim Korrelieren der beiden Datenströme.</span><span class="sxs-lookup"><span data-stu-id="cbc98-153">This enables Databricks to apply a degree of parallelism when it correlates the two streams.</span></span> <span data-ttu-id="cbc98-154">Ein Datensatz in der Partition *n* der Fahrtdaten entspricht einem Datensatz in der Partition *n* der Fahrpreisdaten.</span><span class="sxs-lookup"><span data-stu-id="cbc98-154">A record in partition *n* of the ride data will match a record in partition *n* of the fare data.</span></span>

![Diagramm der Datenstromverarbeitung mit Azure Databricks und Event Hubs](./images/stream-processing-databricks-eh.png)

<span data-ttu-id="cbc98-156">Im Datengenerator enthält das gemeinsame Datenmodell für beide Datensatztypen eine `PartitionKey`-Eigenschaft, bei der es sich um die Verkettung von `Medallion`, `HackLicense` und `VendorId` handelt.</span><span class="sxs-lookup"><span data-stu-id="cbc98-156">In the data generator, the common data model for both record types has a `PartitionKey` property that is the concatenation of `Medallion`, `HackLicense`, and `VendorId`.</span></span>

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

<span data-ttu-id="cbc98-157">Diese Eigenschaft wird verwendet, um beim Senden der Daten an Event Hubs einen expliziten Partitionsschlüssel bereitzustellen:</span><span class="sxs-lookup"><span data-stu-id="cbc98-157">This property is used to provide an explicit partition key when sending to Event Hubs:</span></span>

```csharp
using (var client = pool.GetObject())
{
    return client.Value.SendAsync(new EventData(Encoding.UTF8.GetBytes(
        t.GetData(dataFormat))), t.PartitionKey);
}
```

### <a name="event-hubs"></a><span data-ttu-id="cbc98-158">Event Hubs</span><span class="sxs-lookup"><span data-stu-id="cbc98-158">Event Hubs</span></span>

<span data-ttu-id="cbc98-159">Die Durchsatzkapazität von Event Hubs wird in [Durchsatzeinheiten](/azure/event-hubs/event-hubs-features#throughput-units) gemessen.</span><span class="sxs-lookup"><span data-stu-id="cbc98-159">The throughput capacity of Event Hubs is measured in [throughput units](/azure/event-hubs/event-hubs-features#throughput-units).</span></span> <span data-ttu-id="cbc98-160">Sie können einen Event Hub automatisch skalieren, indem Sie die Funktion für [automatische Vergrößerung](/azure/event-hubs/event-hubs-auto-inflate) aktivieren. Dadurch werden die Durchsatzeinheiten basierend auf dem Datenverkehr automatisch bis zu einem konfigurierten Höchstwert hochskaliert.</span><span class="sxs-lookup"><span data-stu-id="cbc98-160">You can autoscale an event hub by enabling [auto-inflate](/azure/event-hubs/event-hubs-auto-inflate), which automatically scales the throughput units based on traffic, up to a configured maximum.</span></span>

## <a name="stream-processing"></a><span data-ttu-id="cbc98-161">Datenstromverarbeitung</span><span class="sxs-lookup"><span data-stu-id="cbc98-161">Stream processing</span></span>

<span data-ttu-id="cbc98-162">Die Datenverarbeitung in Azure Databricks erfolgt durch einen Auftrag.</span><span class="sxs-lookup"><span data-stu-id="cbc98-162">In Azure Databricks, data processing is performed by a job.</span></span> <span data-ttu-id="cbc98-163">Der Auftrag wird einem Cluster zugewiesen und darin ausgeführt.</span><span class="sxs-lookup"><span data-stu-id="cbc98-163">The job is assigned to and runs on a cluster.</span></span> <span data-ttu-id="cbc98-164">Bei dem Auftrag kann es sich um benutzerdefinierten Java-Code oder um ein Spark-[Notebook](https://docs.databricks.com/user-guide/notebooks/index.html) handeln.</span><span class="sxs-lookup"><span data-stu-id="cbc98-164">The job can either be custom code written in Java, or a Spark [notebook](https://docs.databricks.com/user-guide/notebooks/index.html).</span></span>

<span data-ttu-id="cbc98-165">In dieser Referenzarchitektur ist der Auftrag ein Java-Archiv mit Klassen (geschrieben in Java und Scala).</span><span class="sxs-lookup"><span data-stu-id="cbc98-165">In this reference architecture, the job is a Java archive with classes written in both Java and Scala.</span></span> <span data-ttu-id="cbc98-166">Bei Angabe des Java-Archivs für einen Databricks-Auftrag wird die Klasse für die Ausführung durch den Databricks-Cluster angegeben.</span><span class="sxs-lookup"><span data-stu-id="cbc98-166">When specifying the Java archive for a Databricks job, the class is specified for execution by the Databricks cluster.</span></span> <span data-ttu-id="cbc98-167">Hier enthält die **main**-Methode der Klasse **com.microsoft.pnp.TaxiCabReader** die Datenverarbeitungslogik.</span><span class="sxs-lookup"><span data-stu-id="cbc98-167">Here, the **main** method of the **com.microsoft.pnp.TaxiCabReader** class contains the data processing logic.</span></span>

### <a name="reading-the-stream-from-the-two-event-hub-instances"></a><span data-ttu-id="cbc98-168">Lesen des Datenstroms aus den beiden Event Hub-Instanzen</span><span class="sxs-lookup"><span data-stu-id="cbc98-168">Reading the stream from the two event hub instances</span></span>

<span data-ttu-id="cbc98-169">Die Datenverarbeitungslogik verwendet [strukturiertes Spark-Streaming](https://spark.apache.org/docs/2.1.2/structured-streaming-programming-guide.html), um Daten aus den beiden Azure Event Hub-Instanzen zu lesen:</span><span class="sxs-lookup"><span data-stu-id="cbc98-169">The data processing logic uses [Spark structured streaming](https://spark.apache.org/docs/2.1.2/structured-streaming-programming-guide.html) to read from the two Azure event hub instances:</span></span>

```scala
val rideEventHubOptions = EventHubsConf(rideEventHubConnectionString)
      .setConsumerGroup(conf.taxiRideConsumerGroup())
      .setStartingPosition(EventPosition.fromStartOfStream)
    val rideEvents = spark.readStream
      .format("eventhubs")
      .options(rideEventHubOptions.toMap)
      .load

    val fareEventHubOptions = EventHubsConf(fareEventHubConnectionString)
      .setConsumerGroup(conf.taxiFareConsumerGroup())
      .setStartingPosition(EventPosition.fromStartOfStream)
    val fareEvents = spark.readStream
      .format("eventhubs")
      .options(fareEventHubOptions.toMap)
      .load
```

### <a name="enriching-the-data-with-the-neighborhood-information"></a><span data-ttu-id="cbc98-170">Anreichern der Daten mit Stadtviertelinformationen</span><span class="sxs-lookup"><span data-stu-id="cbc98-170">Enriching the data with the neighborhood information</span></span>

<span data-ttu-id="cbc98-171">Die Fahrtdaten enthalten die Koordinaten (Längen- und Breitengrad) des Start- und Zielorts.</span><span class="sxs-lookup"><span data-stu-id="cbc98-171">The ride data includes the latitude and longitude coordinates of the pick up and drop off locations.</span></span> <span data-ttu-id="cbc98-172">Diese Koordinaten sind zwar hilfreich, eignen sich aber nicht besonders für die Analyse.</span><span class="sxs-lookup"><span data-stu-id="cbc98-172">While these coordinates are useful, they are not easily consumed for analysis.</span></span> <span data-ttu-id="cbc98-173">Daher werden die Daten mit Stadtvierteldaten aus einer [Shape-Datei](https://en.wikipedia.org/wiki/Shapefile) angereichert.</span><span class="sxs-lookup"><span data-stu-id="cbc98-173">Therefore, this data is enriched with neighborhood data that is read from a [shapefile](https://en.wikipedia.org/wiki/Shapefile).</span></span>

<span data-ttu-id="cbc98-174">Das Shape-Dateiformat ist binär und lässt sich nicht so einfach analysieren. Die Bibliothek [GeoTools](http://geotools.org/) bietet jedoch Tools für Geodaten, die das Shape-Dateiformat verwenden.</span><span class="sxs-lookup"><span data-stu-id="cbc98-174">The shapefile format is binary and not easily parsed, but the [GeoTools](http://geotools.org/) library provides tools for geospatial data that use the shapefile format.</span></span> <span data-ttu-id="cbc98-175">Diese Bibliothek wird in der Klasse **com.microsoft.pnp.GeoFinder** verwendet, um auf der Grundlage der Koordinaten des Start- und Zielorts den Namen des Stadtviertels zu bestimmen.</span><span class="sxs-lookup"><span data-stu-id="cbc98-175">This library is used in the **com.microsoft.pnp.GeoFinder** class to determine the neighborhood name based on the pick up and drop off coordinates.</span></span>

```scala
val neighborhoodFinder = (lon: Double, lat: Double) => {
      NeighborhoodFinder.getNeighborhood(lon, lat).get()
    }
```

### <a name="joining-the-ride-and-fare-data"></a><span data-ttu-id="cbc98-176">Verknüpfen von Fahrt- und Fahrpreisdaten</span><span class="sxs-lookup"><span data-stu-id="cbc98-176">Joining the ride and fare data</span></span>

<span data-ttu-id="cbc98-177">Zunächst werden die Fahrt- und Fahrpreisdaten transformiert:</span><span class="sxs-lookup"><span data-stu-id="cbc98-177">First the ride and fare data is transformed:</span></span>

```scala
    val rides = transformedRides
      .filter(r => {
        if (r.isNullAt(r.fieldIndex("errorMessage"))) {
          true
        }
        else {
          malformedRides.add(1)
          false
        }
      })
      .select(
        $"ride.*",
        to_neighborhood($"ride.pickupLon", $"ride.pickupLat")
          .as("pickupNeighborhood"),
        to_neighborhood($"ride.dropoffLon", $"ride.dropoffLat")
          .as("dropoffNeighborhood")
      )
      .withWatermark("pickupTime", conf.taxiRideWatermarkInterval())

    val fares = transformedFares
      .filter(r => {
        if (r.isNullAt(r.fieldIndex("errorMessage"))) {
          true
        }
        else {
          malformedFares.add(1)
          false
        }
      })
      .select(
        $"fare.*",
        $"pickupTime"
      )
      .withWatermark("pickupTime", conf.taxiFareWatermarkInterval())
```

<span data-ttu-id="cbc98-178">Anschließend werden die Fahrtdaten mit den Fahrpreisdaten verknüpft:</span><span class="sxs-lookup"><span data-stu-id="cbc98-178">And then the ride data is joined with the fare data:</span></span>

```scala
val mergedTaxiTrip = rides.join(fares, Seq("medallion", "hackLicense", "vendorId", "pickupTime"))
```

### <a name="processing-the-data-and-inserting-into-cosmos-db"></a><span data-ttu-id="cbc98-179">Verarbeiten der Daten und Einfügen in Cosmos DB</span><span class="sxs-lookup"><span data-stu-id="cbc98-179">Processing the data and inserting into Cosmos DB</span></span>

<span data-ttu-id="cbc98-180">Der durchschnittliche Fahrpreis für jedes Stadtviertel wird für ein bestimmtes Zeitintervall berechnet:</span><span class="sxs-lookup"><span data-stu-id="cbc98-180">The average fare amount for each neighborhood is calculated for a given time interval:</span></span>

```scala
val maxAvgFarePerNeighborhood = mergedTaxiTrip.selectExpr("medallion", "hackLicense", "vendorId", "pickupTime", "rateCode", "storeAndForwardFlag", "dropoffTime", "passengerCount", "tripTimeInSeconds", "tripDistanceInMiles", "pickupLon", "pickupLat", "dropoffLon", "dropoffLat", "paymentType", "fareAmount", "surcharge", "mtaTax", "tipAmount", "tollsAmount", "totalAmount", "pickupNeighborhood", "dropoffNeighborhood")
      .groupBy(window($"pickupTime", conf.windowInterval()), $"pickupNeighborhood")
      .agg(
        count("*").as("rideCount"),
        sum($"fareAmount").as("totalFareAmount"),
        sum($"tipAmount").as("totalTipAmount")
      )
      .select($"window.start", $"window.end", $"pickupNeighborhood", $"rideCount", $"totalFareAmount", $"totalTipAmount")
```

<span data-ttu-id="cbc98-181">Das Ergebnis wird schließlich in Cosmos DB eingefügt:</span><span class="sxs-lookup"><span data-stu-id="cbc98-181">Which is then inserted into Cosmos DB:</span></span>

```scala
maxAvgFarePerNeighborhood
      .writeStream
      .queryName("maxAvgFarePerNeighborhood_cassandra_insert")
      .outputMode(OutputMode.Append())
      .foreach(new CassandraSinkForeach(connector))
      .start()
      .awaitTermination()
```

## <a name="security-considerations"></a><span data-ttu-id="cbc98-182">Sicherheitshinweise</span><span class="sxs-lookup"><span data-stu-id="cbc98-182">Security considerations</span></span>

<span data-ttu-id="cbc98-183">Der Zugriff auf den Arbeitsbereich der Azure-Datenbank wird über die [Administratorkonsole](https://docs.databricks.com/administration-guide/admin-settings/index.html) gesteuert.</span><span class="sxs-lookup"><span data-stu-id="cbc98-183">Access to the Azure Database workspace is controlled using the [administrator console](https://docs.databricks.com/administration-guide/admin-settings/index.html).</span></span> <span data-ttu-id="cbc98-184">Über die Administratorkonsole können Benutzer hinzugefügt, Benutzerberechtigungen verwaltet und einmaliges Anmelden eingerichtet werden.</span><span class="sxs-lookup"><span data-stu-id="cbc98-184">The administrator console includes functionality to add users, manage user permissions, and set up single sign-on.</span></span> <span data-ttu-id="cbc98-185">Die Zugriffssteuerung für Arbeitsbereiche, Cluster, Aufträge und Tabellen kann ebenfalls über die Administratorkonsole festgelegt werden.</span><span class="sxs-lookup"><span data-stu-id="cbc98-185">Access control for workspaces, clusters, jobs, and tables can also be set through the administrator console.</span></span>

### <a name="managing-secrets"></a><span data-ttu-id="cbc98-186">Verwaltung geheimer Schlüssel</span><span class="sxs-lookup"><span data-stu-id="cbc98-186">Managing secrets</span></span>

<span data-ttu-id="cbc98-187">Azure Databricks enthält einen [Geheimnisspeicher](https://docs.azuredatabricks.net/user-guide/secrets/index.html) zum Speichern von Geheimnissen (beispielsweise Verbindungszeichenfolgen, Zugriffsschlüssel, Benutzernamen und Kennwörter).</span><span class="sxs-lookup"><span data-stu-id="cbc98-187">Azure Databricks includes a [secret store](https://docs.azuredatabricks.net/user-guide/secrets/index.html) that is used to store secrets, including connection strings, access keys, user names, and passwords.</span></span> <span data-ttu-id="cbc98-188">Geheimnisse werden im Azure Databricks-Geheimnisspeicher nach **Bereichen** partitioniert:</span><span class="sxs-lookup"><span data-stu-id="cbc98-188">Secrets within the Azure Databricks secret store are partitioned by **scopes**:</span></span>

```bash
databricks secrets create-scope --scope "azure-databricks-job"
```

<span data-ttu-id="cbc98-189">Geheimnisse werden auf der Bereichsebene hinzugefügt:</span><span class="sxs-lookup"><span data-stu-id="cbc98-189">Secrets are added at the scope level:</span></span>

```bash
databricks secrets put --scope "azure-databricks-job" --key "taxi-ride"
```

> [!NOTE]
> <span data-ttu-id="cbc98-190">Anstelle des nativen Azure Databricks-Bereichs kann auch ein Azure Key Vault-basierter Bereich verwendet werden.</span><span class="sxs-lookup"><span data-stu-id="cbc98-190">An Azure Key Vault-backed scope can be used instead of the native Azure Databricks scope.</span></span> <span data-ttu-id="cbc98-191">Weitere Informationen finden Sie unter [Azure Key Vault-backed scopes](https://docs.azuredatabricks.net/user-guide/secrets/secret-scopes.html#azure-key-vault-backed-scopes) (Azure Key Vault-basierte Bereiche).</span><span class="sxs-lookup"><span data-stu-id="cbc98-191">To learn more, see [Azure Key Vault-backed scopes](https://docs.azuredatabricks.net/user-guide/secrets/secret-scopes.html#azure-key-vault-backed-scopes).</span></span>

<span data-ttu-id="cbc98-192">Im Code erfolgt der Zugriff auf Geheimnisse über die [secrets-Hilfsprogramme](https://docs.databricks.com/user-guide/dev-tools/dbutils.html#secrets-utilities) von Azure Databricks.</span><span class="sxs-lookup"><span data-stu-id="cbc98-192">In code, secrets are accessed via the Azure Databricks [secrets utilities](https://docs.databricks.com/user-guide/dev-tools/dbutils.html#secrets-utilities).</span></span>

## <a name="monitoring-considerations"></a><span data-ttu-id="cbc98-193">Aspekte der Überwachung</span><span class="sxs-lookup"><span data-stu-id="cbc98-193">Monitoring considerations</span></span>

<span data-ttu-id="cbc98-194">Azure Databricks basiert auf Apache Spark. Beide Lösungen verwenden [log4j](https://logging.apache.org/log4j/2.x/) als Standardbibliothek für die Protokollierung.</span><span class="sxs-lookup"><span data-stu-id="cbc98-194">Azure Databricks is based on Apache Spark, and both use [log4j](https://logging.apache.org/log4j/2.x/) as the standard library for logging.</span></span> <span data-ttu-id="cbc98-195">Zusätzlich zur von Apache Spark bereitgestellten Standardprotokollierung sendet diese Referenzarchitektur auch Protokolle und Metriken an [Azure Log Analytics](/azure/log-analytics/).</span><span class="sxs-lookup"><span data-stu-id="cbc98-195">In addition to the default logging provided by Apache Spark, this reference architecture sends logs and metrics to [Azure Log Analytics](/azure/log-analytics/).</span></span>

<span data-ttu-id="cbc98-196">Die Klasse **com.microsoft.pnp.TaxiCabReader** konfiguriert das Apache Spark-Protokollierungssystem so, dass dessen Protokolle an Azure Log Analytics gesendet werden. Hierbei werden die Werte aus der Datei **log4j.properties** verwendet.</span><span class="sxs-lookup"><span data-stu-id="cbc98-196">The **com.microsoft.pnp.TaxiCabReader** class configures the Apache Spark logging system to send its logs to Azure Log Analytics using the values in the **log4j.properties** file.</span></span> <span data-ttu-id="cbc98-197">Die Meldungen der Apache Spark-Protokollierung liegen als Zeichenfolgen vor. Für Azure Log Analytics werden jedoch Protokollmeldungen im JSON-Format benötigt.</span><span class="sxs-lookup"><span data-stu-id="cbc98-197">While the Apache Spark logger messages are strings, Azure Log Analytics requires log messages to be formatted as JSON.</span></span> <span data-ttu-id="cbc98-198">Die Klasse **com.microsoft.pnp.log4j.LogAnalyticsAppender** transformiert die Meldungen in das JSON-Format:</span><span class="sxs-lookup"><span data-stu-id="cbc98-198">The **com.microsoft.pnp.log4j.LogAnalyticsAppender** class transforms these messages to JSON:</span></span>

```scala

    @Override
    protected void append(LoggingEvent loggingEvent) {
        if (this.layout == null) {
            this.setLayout(new JSONLayout());
        }

        String json = this.getLayout().format(loggingEvent);
        try {
            this.client.send(json, this.logType);
        } catch(IOException ioe) {
            LogLog.warn("Error sending LoggingEvent to Log Analytics", ioe);
        }
    }

```

<span data-ttu-id="cbc98-199">Bei der Verarbeitung von Fahrt- und Fahrpreismeldungen durch die Klasse **com.microsoft.pnp.TaxiCabReader** kann es zu Problemen mit der Formatierung und somit zu ungültigen Meldungen kommen.</span><span class="sxs-lookup"><span data-stu-id="cbc98-199">As the **com.microsoft.pnp.TaxiCabReader** class processes ride and fare messages, it's possible that either one may be malformed and therefore not valid.</span></span> <span data-ttu-id="cbc98-200">In einer Produktionsumgebung müssen diese falsch formatierten Meldungen unbedingt analysiert werden, um ein Problem mit den Datenquellen zu erkennen und schnell zu beheben, damit keine Daten verloren gehen.</span><span class="sxs-lookup"><span data-stu-id="cbc98-200">In a production environment, it's important to analyze these malformed messages to identify a problem with the data sources so it can be fixed quickly to prevent data loss.</span></span> <span data-ttu-id="cbc98-201">Die Klasse **com.microsoft.pnp.TaxiCabReader** registriert einen Apache Spark-Akkumulator zur Nachverfolgung der Anzahl falsch formatierter Fahrpreis- und Fahrtdatensätze:</span><span class="sxs-lookup"><span data-stu-id="cbc98-201">The **com.microsoft.pnp.TaxiCabReader** class registers an Apache Spark Accumulator that keeps track of the number of malformed fare and ride records:</span></span>

```scala
    @transient val appMetrics = new AppMetrics(spark.sparkContext)
    appMetrics.registerGauge("metrics.malformedrides", AppAccumulators.getRideInstance(spark.sparkContext))
    appMetrics.registerGauge("metrics.malformedfares", AppAccumulators.getFareInstance(spark.sparkContext))
    SparkEnv.get.metricsSystem.registerSource(appMetrics)
```

<span data-ttu-id="cbc98-202">Apache Spark verwendet die Dropwizard-Bibliothek, um Metriken zu senden. Einige der nativen Dropwizard-Metrikfelder sind jedoch nicht mit Azure Log Analytics kompatibel.</span><span class="sxs-lookup"><span data-stu-id="cbc98-202">Apache Spark uses the Dropwizard library to send metrics, and some of the native Dropwizard metrics fields are incompatible with Azure Log Analytics.</span></span> <span data-ttu-id="cbc98-203">Daher enthält diese Referenzarchitektur eine benutzerdefinierte Dropwizard-Senke und einen entsprechenden Reporter.</span><span class="sxs-lookup"><span data-stu-id="cbc98-203">Therefore, this reference architecture includes a custom Dropwizard sink and reporter.</span></span> <span data-ttu-id="cbc98-204">Die Metriken werden so formatiert, wie es von Azure Log Analytics erwartet wird.</span><span class="sxs-lookup"><span data-stu-id="cbc98-204">It formats the metrics in the format expected by Azure Log Analytics.</span></span> <span data-ttu-id="cbc98-205">Wenn Apache Spark Metriken meldet, werden auch die benutzerdefinierten Metriken für die falsch formatierten Fahrt- und Fahrpreisdaten gesendet.</span><span class="sxs-lookup"><span data-stu-id="cbc98-205">When Apache Spark reports metrics, the custom metrics for the malformed ride and fare data are also sent.</span></span>

<span data-ttu-id="cbc98-206">Als letzte Metrik wird im Azure Log Analytics-Arbeitsbereich der kumulative Fortschritt des strukturierten Spark-Streamingauftrags protokolliert.</span><span class="sxs-lookup"><span data-stu-id="cbc98-206">The last metric to be logged to the Azure Log Analytics workspace is the cumulative progress of the Spark Structured Streaming job progress.</span></span> <span data-ttu-id="cbc98-207">Hierzu wird in der Klasse **com.microsoft.pnp.StreamingMetricsListener** ein benutzerdefinierter StreamingQuery-Listener implementiert.</span><span class="sxs-lookup"><span data-stu-id="cbc98-207">This is done using a custom StreamingQuery listener implemented in the **com.microsoft.pnp.StreamingMetricsListener** class.</span></span> <span data-ttu-id="cbc98-208">Diese Klasse wird beim Ausführen des Auftrags bei der Apache Spark-Sitzung registriert:</span><span class="sxs-lookup"><span data-stu-id="cbc98-208">This class is registered to the Apache Spark Session when the job runs:</span></span>

```scala
spark.streams.addListener(new StreamingMetricsListener())
```

<span data-ttu-id="cbc98-209">Die Methoden in „StreamingMetricsListener“ werden von der Apache Spark-Runtime aufgerufen, wenn ein strukturiertes Streamingereignis auftritt. Dabei werden Protokollmeldungen und Metriken an den Azure Log Analytics-Arbeitsbereich gesendet.</span><span class="sxs-lookup"><span data-stu-id="cbc98-209">The methods in the StreamingMetricsListener are called by the Apache Spark runtime whenever a structured steaming event occurs, sending log messages and metrics to the Azure Log Analytics workspace.</span></span> <span data-ttu-id="cbc98-210">Zur Überwachung der Anwendung können Sie in Ihrem Arbeitsbereich die folgenden Abfragen verwenden:</span><span class="sxs-lookup"><span data-stu-id="cbc98-210">You can use the following queries in your workspace to monitor the application:</span></span>

### <a name="latency-and-throughput-for-streaming-queries"></a><span data-ttu-id="cbc98-211">Wartezeit und Durchsatz für Streamingabfragen</span><span class="sxs-lookup"><span data-stu-id="cbc98-211">Latency and throughput for streaming queries</span></span>

```shell
taxijob_CL
| where TimeGenerated > startofday(datetime(<date>)) and TimeGenerated < endofday(datetime(<date>))
| project  mdc_inputRowsPerSecond_d, mdc_durationms_triggerExecution_d
| render timechart
```

### <a name="exceptions-logged-during-stream-query-execution"></a><span data-ttu-id="cbc98-212">Ausnahmen, die während der Ausführung von Datenstromabfragen protokolliert wurden</span><span class="sxs-lookup"><span data-stu-id="cbc98-212">Exceptions logged during stream query execution</span></span>

```shell
taxijob_CL
| where TimeGenerated > startofday(datetime(<date>)) and TimeGenerated < endofday(datetime(<date>))
| where Level contains "Error"
```

### <a name="accumulation-of-malformed-fare-and-ride-data"></a><span data-ttu-id="cbc98-213">Kumulation falsch formatierter Fahrpreis- und Fahrtdaten</span><span class="sxs-lookup"><span data-stu-id="cbc98-213">Accumulation of malformed fare and ride data</span></span>

```shell
SparkMetric_CL
| where TimeGenerated > startofday(datetime(<date>)) and TimeGenerated < endofday(datetime(<date>))
| render timechart
| where name_s contains "metrics.malformedrides"

SparkMetric_CL
| where TimeGenerated > startofday(datetime(<date>)) and TimeGenerated < endofday(datetime(<date>))
| render timechart
| where name_s contains "metrics.malformedfares"
```

### <a name="job-execution-to-trace-resiliency"></a><span data-ttu-id="cbc98-214">Auftragsausführung zur Nachverfolgung der Resilienz</span><span class="sxs-lookup"><span data-stu-id="cbc98-214">Job execution to trace resiliency</span></span>

```shell
SparkMetric_CL
| where TimeGenerated > startofday(datetime(<date>)) and TimeGenerated < endofday(datetime(<date>))
| render timechart
| where name_s contains "driver.DAGScheduler.job.allJobs"
```

## <a name="deploy-the-solution"></a><span data-ttu-id="cbc98-215">Bereitstellen der Lösung</span><span class="sxs-lookup"><span data-stu-id="cbc98-215">Deploy the solution</span></span>

<span data-ttu-id="cbc98-216">Führen Sie zum Bereitstellen und Ausführen der Referenzimplementierung die Schritte aus der [GitHub-Infodatei](https://github.com/mspnp/azure-databricks-streaming-analytics) aus.</span><span class="sxs-lookup"><span data-stu-id="cbc98-216">To the deploy and run the reference implementation, follow the steps in the [GitHub readme](https://github.com/mspnp/azure-databricks-streaming-analytics).</span></span>
