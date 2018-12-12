---
title: Datenstromverarbeitung mit Azure Databricks
description: Erstellen einer End-to-End-Pipeline zur Datenstromverarbeitung in Azure mit Azure Databricks
author: petertaylor9999
ms.date: 11/30/2018
ms.openlocfilehash: 0640e900c212d2b75cc9cdd5bec3a4f7c050490d
ms.sourcegitcommit: e7e0e0282fa93f0063da3b57128ade395a9c1ef9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/05/2018
ms.locfileid: "52902832"
---
# <a name="stream-processing-with-azure-databricks"></a><span data-ttu-id="b5e70-103">Datenstromverarbeitung mit Azure Databricks</span><span class="sxs-lookup"><span data-stu-id="b5e70-103">Stream processing with Azure Databricks</span></span>

<span data-ttu-id="b5e70-104">Diese Referenzarchitektur zeigt eine End-to-End-Pipeline zur [Datenstromverarbeitung](/azure/architecture/data-guide/big-data/real-time-processing).</span><span class="sxs-lookup"><span data-stu-id="b5e70-104">This reference architecture shows an end-to-end [stream processing](/azure/architecture/data-guide/big-data/real-time-processing) pipeline.</span></span> <span data-ttu-id="b5e70-105">Dieser Pipelinetyp hat vier Phasen: Erfassung, Verarbeitung, Speicherung und Analyse/Berichterstellung.</span><span class="sxs-lookup"><span data-stu-id="b5e70-105">This type of pipeline has four stages: ingest, process, store, and analysis and reporting.</span></span> <span data-ttu-id="b5e70-106">In dieser Referenzarchitektur erfasst die Pipeline Daten aus zwei Quellen, verknüpft verwandte Datensätze aus den einzelnen Datenströmen, reichert das Ergebnis an und berechnet einen Durchschnitt in Echtzeit.</span><span class="sxs-lookup"><span data-stu-id="b5e70-106">For this reference architecture, the pipeline ingests data from two sources, performs a join on related records from each stream, enriches the result, and calculates an average in real time.</span></span> <span data-ttu-id="b5e70-107">Die Ergebnisse werden zur weiteren Analyse gespeichert.</span><span class="sxs-lookup"><span data-stu-id="b5e70-107">The results are stored for further analysis.</span></span> [<span data-ttu-id="b5e70-108">**So stellen Sie diese Lösung bereit**.</span><span class="sxs-lookup"><span data-stu-id="b5e70-108">**Deploy this solution**.</span></span>](#deploy-the-solution)

![](./images/stream-processing-databricks.png)

<span data-ttu-id="b5e70-109">**Szenario**: Ein Taxiunternehmen erfasst Daten zu jeder Taxifahrt.</span><span class="sxs-lookup"><span data-stu-id="b5e70-109">**Scenario**: A taxi company collects data about each taxi trip.</span></span> <span data-ttu-id="b5e70-110">In diesem Szenario gehen wir davon aus, dass zwei separate Geräte Daten senden.</span><span class="sxs-lookup"><span data-stu-id="b5e70-110">For this scenario, we assume there are two separate devices sending data.</span></span> <span data-ttu-id="b5e70-111">Das Taxi verfügt ein Messgerät, das Informationen zu jeder Fahrt sendet – die Dauer, die Strecke sowie die Abhol- und Zielorte.</span><span class="sxs-lookup"><span data-stu-id="b5e70-111">The taxi has a meter that sends information about each ride &mdash; the duration, distance, and pickup and dropoff locations.</span></span> <span data-ttu-id="b5e70-112">Ein separates Gerät akzeptiert Zahlungen von Kunden und sendet Daten zu den Fahrpreisen.</span><span class="sxs-lookup"><span data-stu-id="b5e70-112">A separate device accepts payments from customers and sends data about fares.</span></span> <span data-ttu-id="b5e70-113">Das Taxiunternehmen möchte Fahrgasttrends ermitteln und dazu für jedes Stadtviertel in Echtzeit das durchschnittliche Trinkgeld pro gefahrener Meile berechnen.</span><span class="sxs-lookup"><span data-stu-id="b5e70-113">To spot ridership trends, the taxi company wants to calculate the average tip per mile driven, in real time, for each neighborhood.</span></span>

## <a name="architecture"></a><span data-ttu-id="b5e70-114">Architecture</span><span class="sxs-lookup"><span data-stu-id="b5e70-114">Architecture</span></span>

<span data-ttu-id="b5e70-115">Die Architektur umfasst die folgenden Komponenten.</span><span class="sxs-lookup"><span data-stu-id="b5e70-115">The architecture consists of the following components.</span></span>

<span data-ttu-id="b5e70-116">**Datenquellen:**</span><span class="sxs-lookup"><span data-stu-id="b5e70-116">**Data sources**.</span></span> <span data-ttu-id="b5e70-117">In dieser Architektur gibt es zwei Datenquellen, die Datenströme in Echtzeit generieren.</span><span class="sxs-lookup"><span data-stu-id="b5e70-117">In this architecture, there are two data sources that generate data streams in real time.</span></span> <span data-ttu-id="b5e70-118">Der erste Datenstrom enthält Informationen zur Fahrt und der zweite Informationen zum Fahrpreis.</span><span class="sxs-lookup"><span data-stu-id="b5e70-118">The first stream contains ride information, and the second contains fare information.</span></span> <span data-ttu-id="b5e70-119">Die Referenzarchitektur umfasst einen simulierten Datengenerator, der aus einem Satz von statischen Dateien liest und die Daten an Event Hubs pusht.</span><span class="sxs-lookup"><span data-stu-id="b5e70-119">The reference architecture includes a simulated data generator that reads from a set of static files and pushes the data to Event Hubs.</span></span> <span data-ttu-id="b5e70-120">Bei einer echten Anwendung stammen die Daten von Geräten, die in den Taxis installiert sind.</span><span class="sxs-lookup"><span data-stu-id="b5e70-120">The data sources in a real application would be devices installed in the taxi cabs.</span></span>

<span data-ttu-id="b5e70-121">**Azure Event Hubs**:</span><span class="sxs-lookup"><span data-stu-id="b5e70-121">**Azure Event Hubs**.</span></span> <span data-ttu-id="b5e70-122">[Event Hubs](/azure/event-hubs/) ist ein Ereigniserfassungsdienst.</span><span class="sxs-lookup"><span data-stu-id="b5e70-122">[Event Hubs](/azure/event-hubs/) is an event ingestion service.</span></span> <span data-ttu-id="b5e70-123">Die hier gezeigte Architektur verwendet zwei Event Hub-Instanzen, eine für jede Datenquelle.</span><span class="sxs-lookup"><span data-stu-id="b5e70-123">This architecture uses two event hub instances, one for each data source.</span></span> <span data-ttu-id="b5e70-124">Jede Datenquelle sendet einen Datenstrom an den zugehörigen Event Hub.</span><span class="sxs-lookup"><span data-stu-id="b5e70-124">Each data source sends a stream of data to the associated event hub.</span></span>

<span data-ttu-id="b5e70-125">**Azure Databricks**:</span><span class="sxs-lookup"><span data-stu-id="b5e70-125">**Azure Databricks**.</span></span> <span data-ttu-id="b5e70-126">[Databricks](/azure/azure-databricks/) ist eine Apache Spark-basierte Analyseplattform, die für die Microsoft Azure-Clouddienstplattform optimiert ist.</span><span class="sxs-lookup"><span data-stu-id="b5e70-126">[Databricks](/azure/azure-databricks/) is an Apache Spark-based analytics platform optimized for the Microsoft Azure cloud services platform.</span></span> <span data-ttu-id="b5e70-127">Sie wird verwendet, um die Taxifahrtdaten und die Fahrpreisdaten zu korrelieren und die korrelierten Daten mit den gespeicherten Stadtvierteldaten aus dem Databricks-Dateisystem anzureichern.</span><span class="sxs-lookup"><span data-stu-id="b5e70-127">Databricks is used to correlate of the taxi ride and fare data, and also to enrich the correlated data with neighborhood data stored in the Databricks file system.</span></span>

<span data-ttu-id="b5e70-128">**Cosmos DB**:</span><span class="sxs-lookup"><span data-stu-id="b5e70-128">**Cosmos DB**.</span></span> <span data-ttu-id="b5e70-129">Der Azure Databricks-Auftrag gibt eine Reihe von Datensätzen aus, die unter Verwendung der Cassandra-API in [Cosmos DB](/azure/cosmos-db/) geschrieben werden.</span><span class="sxs-lookup"><span data-stu-id="b5e70-129">The output from Azure Databricks job is a series of records, which are written to [Cosmos DB](/azure/cosmos-db/) using the Cassandra API.</span></span> <span data-ttu-id="b5e70-130">Die Cassandra-API wird verwendet, da sie Modelle mit Zeitreihendaten unterstützt.</span><span class="sxs-lookup"><span data-stu-id="b5e70-130">The Cassandra API is used because it supports time series data modeling.</span></span>

<span data-ttu-id="b5e70-131">**Azure Log Analytics**:</span><span class="sxs-lookup"><span data-stu-id="b5e70-131">**Azure Log Analytics**.</span></span> <span data-ttu-id="b5e70-132">Die von [Azure Monitor](/azure/monitoring-and-diagnostics/) erfassten Anwendungsprotokolldaten werden in einem [Log Analytics-Arbeitsbereich](/azure/log-analytics) gespeichert.</span><span class="sxs-lookup"><span data-stu-id="b5e70-132">Application log data collected by [Azure Monitor](/azure/monitoring-and-diagnostics/) is stored in a [Log Analytics workspace](/azure/log-analytics).</span></span> <span data-ttu-id="b5e70-133">Mithilfe von Log Analytics-Abfragen können Metriken analysiert und visualisiert sowie Protokollmeldungen auf Probleme innerhalb der Anwendung untersucht werden.</span><span class="sxs-lookup"><span data-stu-id="b5e70-133">Log Analytics queries can be used to analyze and visualize metrics and inspect log messages to identify issues within the application.</span></span>

## <a name="data-ingestion"></a><span data-ttu-id="b5e70-134">Datenerfassung</span><span class="sxs-lookup"><span data-stu-id="b5e70-134">Data ingestion</span></span>

<span data-ttu-id="b5e70-135">Um eine Datenquelle zu simulieren, verwendet die Referenzarchitektur das Dataset [New York City Taxi Data](https://uofi.app.box.com/v/NYCtaxidata/folder/2332218797)<sup>[[1]](#note1)</sup>.</span><span class="sxs-lookup"><span data-stu-id="b5e70-135">To simulate a data source, this reference architecture uses the [New York City Taxi Data](https://uofi.app.box.com/v/NYCtaxidata/folder/2332218797) dataset<sup>[[1]](#note1)</sup>.</span></span> <span data-ttu-id="b5e70-136">Dieses Dataset enthält Daten zu Taxifahrten in New York City für einen Zeitraum von vier Jahren (2010&ndash;2013).</span><span class="sxs-lookup"><span data-stu-id="b5e70-136">This dataset contains data about taxi trips in New York City over a four-year period (2010 &ndash; 2013).</span></span> <span data-ttu-id="b5e70-137">Es umfasst zwei Datensatztypen: Fahrtdaten und Fahrpreisdaten.</span><span class="sxs-lookup"><span data-stu-id="b5e70-137">It contains two types of record: Ride data and fare data.</span></span> <span data-ttu-id="b5e70-138">Die Fahrtdaten enthalten die Fahrtdauer, die Fahrtstrecke sowie die Abhol- und Zielorte.</span><span class="sxs-lookup"><span data-stu-id="b5e70-138">Ride data includes trip duration, trip distance, and pickup and dropoff location.</span></span> <span data-ttu-id="b5e70-139">Die Fahrpreisdaten enthalten die Beträge von Fahrpreis, Steuern und Trinkgeld.</span><span class="sxs-lookup"><span data-stu-id="b5e70-139">Fare data includes fare, tax, and tip amounts.</span></span> <span data-ttu-id="b5e70-140">Gemeinsame Felder in beiden Datensatztypen sind die Taxinummer (Medallion), die Taxilizenz (Hack license) und die Anbieter-ID (Vendor ID).</span><span class="sxs-lookup"><span data-stu-id="b5e70-140">Common fields in both record types include medallion number, hack license, and vendor ID.</span></span> <span data-ttu-id="b5e70-141">Anhand dieser drei Felder werden ein Taxi und ein Fahrer eindeutig identifiziert.</span><span class="sxs-lookup"><span data-stu-id="b5e70-141">Together these three fields uniquely identify a taxi plus a driver.</span></span> <span data-ttu-id="b5e70-142">Die Daten werden im CSV-Format gespeichert.</span><span class="sxs-lookup"><span data-stu-id="b5e70-142">The data is stored in CSV format.</span></span> 

<span data-ttu-id="b5e70-143">Der Datengenerator ist eine .NET Core-Anwendung, die Datensätze liest und an Azure Event Hubs sendet.</span><span class="sxs-lookup"><span data-stu-id="b5e70-143">The data generator is a .NET Core application that reads the records and sends them to Azure Event Hubs.</span></span> <span data-ttu-id="b5e70-144">Der Generator sendet Fahrtdaten im JSON-Format und Fahrpreisdaten im CSV-Format.</span><span class="sxs-lookup"><span data-stu-id="b5e70-144">The generator sends ride data in JSON format and fare data in CSV format.</span></span> 

<span data-ttu-id="b5e70-145">Event Hubs verwendet [Partitionen](/azure/event-hubs/event-hubs-features#partitions) zum Segmentieren der Daten.</span><span class="sxs-lookup"><span data-stu-id="b5e70-145">Event Hubs uses [partitions](/azure/event-hubs/event-hubs-features#partitions) to segment the data.</span></span> <span data-ttu-id="b5e70-146">Partitionen ermöglichen es einem Consumer, die einzelnen Partitionen gleichzeitig zu lesen.</span><span class="sxs-lookup"><span data-stu-id="b5e70-146">Partitions allow a consumer to read each partition in parallel.</span></span> <span data-ttu-id="b5e70-147">Wenn Sie Daten an Event Hubs senden, können Sie den Partitionsschlüssel explizit angeben.</span><span class="sxs-lookup"><span data-stu-id="b5e70-147">When you send data to Event Hubs, you can specify the partition key explicitly.</span></span> <span data-ttu-id="b5e70-148">Andernfalls werden Datensätze nach einem Roundrobinverfahren Partitionen zugewiesen.</span><span class="sxs-lookup"><span data-stu-id="b5e70-148">Otherwise, records are assigned to partitions in round-robin fashion.</span></span> 

<span data-ttu-id="b5e70-149">In diesem Szenario sollen Fahrtdaten und Fahrpreisdaten für ein bestimmtes Taxi die gleiche Partitions-ID erhalten.</span><span class="sxs-lookup"><span data-stu-id="b5e70-149">In this scenario, ride data and fare data should end up with the same partition ID for a given taxi cab.</span></span> <span data-ttu-id="b5e70-150">Dies ermöglicht Databricks eine gewisse Parallelität beim Korrelieren der beiden Datenströme.</span><span class="sxs-lookup"><span data-stu-id="b5e70-150">This enables Databricks to apply a degree of parallelism when it correlates the two streams.</span></span> <span data-ttu-id="b5e70-151">Ein Datensatz in der Partition *n* der Fahrtdaten entspricht einem Datensatz in der Partition *n* der Fahrpreisdaten.</span><span class="sxs-lookup"><span data-stu-id="b5e70-151">A record in partition *n* of the ride data will match a record in partition *n* of the fare data.</span></span>

![](./images/stream-processing-databricks-eh.png)

<span data-ttu-id="b5e70-152">Im Datengenerator enthält das gemeinsame Datenmodell für beide Datensatztypen eine `PartitionKey`-Eigenschaft, bei der es sich um die Verkettung von `Medallion`, `HackLicense` und `VendorId` handelt.</span><span class="sxs-lookup"><span data-stu-id="b5e70-152">In the data generator, the common data model for both record types has a `PartitionKey` property that is the concatenation of `Medallion`, `HackLicense`, and `VendorId`.</span></span>

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

<span data-ttu-id="b5e70-153">Diese Eigenschaft wird verwendet, um beim Senden der Daten an Event Hubs einen expliziten Partitionsschlüssel bereitzustellen:</span><span class="sxs-lookup"><span data-stu-id="b5e70-153">This property is used to provide an explicit partition key when sending to Event Hubs:</span></span>

```csharp
using (var client = pool.GetObject())
{
    return client.Value.SendAsync(new EventData(Encoding.UTF8.GetBytes(
        t.GetData(dataFormat))), t.PartitionKey);
}
```

### <a name="event-hubs"></a><span data-ttu-id="b5e70-154">Event Hubs</span><span class="sxs-lookup"><span data-stu-id="b5e70-154">Event Hubs</span></span>

<span data-ttu-id="b5e70-155">Die Durchsatzkapazität von Event Hubs wird in [Durchsatzeinheiten](/azure/event-hubs/event-hubs-features#throughput-units) gemessen.</span><span class="sxs-lookup"><span data-stu-id="b5e70-155">The throughput capacity of Event Hubs is measured in [throughput units](/azure/event-hubs/event-hubs-features#throughput-units).</span></span> <span data-ttu-id="b5e70-156">Sie können einen Event Hub automatisch skalieren, indem Sie die Funktion für [automatische Vergrößerung](/azure/event-hubs/event-hubs-auto-inflate) aktivieren. Dadurch werden die Durchsatzeinheiten basierend auf dem Datenverkehr automatisch bis zu einem konfigurierten Höchstwert hochskaliert.</span><span class="sxs-lookup"><span data-stu-id="b5e70-156">You can autoscale an event hub by enabling [auto-inflate](/azure/event-hubs/event-hubs-auto-inflate), which automatically scales the throughput units based on traffic, up to a configured maximum.</span></span> 

## <a name="stream-processing"></a><span data-ttu-id="b5e70-157">Datenstromverarbeitung</span><span class="sxs-lookup"><span data-stu-id="b5e70-157">Stream processing</span></span>

<span data-ttu-id="b5e70-158">Die Datenverarbeitung in Azure Databricks erfolgt durch einen Auftrag.</span><span class="sxs-lookup"><span data-stu-id="b5e70-158">In Azure Databricks, data processing is performed by a job.</span></span> <span data-ttu-id="b5e70-159">Der Auftrag wird einem Cluster zugewiesen und darin ausgeführt.</span><span class="sxs-lookup"><span data-stu-id="b5e70-159">The job is assigned to and runs on a cluster.</span></span> <span data-ttu-id="b5e70-160">Bei dem Auftrag kann es sich um benutzerdefinierten Java-Code oder um ein Spark-[Notebook](https://docs.databricks.com/user-guide/notebooks/index.html) handeln.</span><span class="sxs-lookup"><span data-stu-id="b5e70-160">The job can either be custom code written in Java, or a Spark [notebook](https://docs.databricks.com/user-guide/notebooks/index.html).</span></span>

<span data-ttu-id="b5e70-161">In dieser Referenzarchitektur ist der Auftrag ein Java-Archiv mit Klassen (geschrieben in Java und Scala).</span><span class="sxs-lookup"><span data-stu-id="b5e70-161">In this reference architecture, the job is a Java archive with classes written in both Java and Scala.</span></span> <span data-ttu-id="b5e70-162">Bei Angabe des Java-Archivs für einen Databricks-Auftrag wird die Klasse für die Ausführung durch den Databricks-Cluster angegeben.</span><span class="sxs-lookup"><span data-stu-id="b5e70-162">When specifying the Java archive for a Databricks job, the class is specified for execution by the Databricks cluster.</span></span> <span data-ttu-id="b5e70-163">Hier enthält die **main**-Methode der Klasse **com.microsoft.pnp.TaxiCabReader** die Datenverarbeitungslogik.</span><span class="sxs-lookup"><span data-stu-id="b5e70-163">Here, the **main** method of the **com.microsoft.pnp.TaxiCabReader** class contains the data processing logic.</span></span> 

### <a name="reading-the-stream-from-the-two-event-hub-instances"></a><span data-ttu-id="b5e70-164">Lesen des Datenstroms aus den beiden Event Hub-Instanzen</span><span class="sxs-lookup"><span data-stu-id="b5e70-164">Reading the stream from the two event hub instances</span></span>

<span data-ttu-id="b5e70-165">Die Datenverarbeitungslogik verwendet [strukturiertes Spark-Streaming](https://spark.apache.org/docs/2.1.2/structured-streaming-programming-guide.html), um Daten aus den beiden Azure Event Hub-Instanzen zu lesen:</span><span class="sxs-lookup"><span data-stu-id="b5e70-165">The data processing logic uses [Spark structured streaming](https://spark.apache.org/docs/2.1.2/structured-streaming-programming-guide.html) to read from the two Azure event hub instances:</span></span>

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

### <a name="enriching-the-data-with-the-neighborhood-information"></a><span data-ttu-id="b5e70-166">Anreichern der Daten mit Stadtviertelinformationen</span><span class="sxs-lookup"><span data-stu-id="b5e70-166">Enriching the data with the neighborhood information</span></span>

<span data-ttu-id="b5e70-167">Die Fahrtdaten enthalten die Koordinaten (Längen- und Breitengrad) des Start- und Zielorts.</span><span class="sxs-lookup"><span data-stu-id="b5e70-167">The ride data includes the latitude and longitude coordinates of the pick up and drop off locations.</span></span> <span data-ttu-id="b5e70-168">Diese Koordinaten sind zwar hilfreich, eignen sich aber nicht besonders für die Analyse.</span><span class="sxs-lookup"><span data-stu-id="b5e70-168">While these coordinates are useful, they are not easily consumed for analysis.</span></span> <span data-ttu-id="b5e70-169">Daher werden die Daten mit Stadtvierteldaten aus einer [Shape-Datei](https://en.wikipedia.org/wiki/Shapefile) angereichert.</span><span class="sxs-lookup"><span data-stu-id="b5e70-169">Therefore, this data is enriched with neighborhood data that is read from a [shapefile](https://en.wikipedia.org/wiki/Shapefile).</span></span> 

<span data-ttu-id="b5e70-170">Das Shape-Dateiformat ist binär und lässt sich nicht so einfach analysieren. Die Bibliothek [GeoTools](http://geotools.org/) bietet jedoch Tools für Geodaten, die das Shape-Dateiformat verwenden.</span><span class="sxs-lookup"><span data-stu-id="b5e70-170">The shapefile format is binary and not easily parsed, but the [GeoTools](http://geotools.org/) library provides tools for geospatial data that use the shapefile format.</span></span> <span data-ttu-id="b5e70-171">Diese Bibliothek wird in der Klasse **com.microsoft.pnp.GeoFinder** verwendet, um auf der Grundlage der Koordinaten des Start- und Zielorts den Namen des Stadtviertels zu bestimmen.</span><span class="sxs-lookup"><span data-stu-id="b5e70-171">This library is used in the **com.microsoft.pnp.GeoFinder** class to determine the neighborhood name based on the pick up and drop off coordinates.</span></span> 

```scala
val neighborhoodFinder = (lon: Double, lat: Double) => {
      NeighborhoodFinder.getNeighborhood(lon, lat).get()
    }
```

### <a name="joining-the-ride-and-fare-data"></a><span data-ttu-id="b5e70-172">Verknüpfen von Fahrt- und Fahrpreisdaten</span><span class="sxs-lookup"><span data-stu-id="b5e70-172">Joining the ride and fare data</span></span>

<span data-ttu-id="b5e70-173">Zunächst werden die Fahrt- und Fahrpreisdaten transformiert:</span><span class="sxs-lookup"><span data-stu-id="b5e70-173">First the ride and fare data is transformed:</span></span>

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

<span data-ttu-id="b5e70-174">Anschließend werden die Fahrtdaten mit den Fahrpreisdaten verknüpft:</span><span class="sxs-lookup"><span data-stu-id="b5e70-174">And then the ride data is joined with the fare data:</span></span>

```scala
val mergedTaxiTrip = rides.join(fares, Seq("medallion", "hackLicense", "vendorId", "pickupTime"))
```

### <a name="processing-the-data-and-inserting-into-cosmos-db"></a><span data-ttu-id="b5e70-175">Verarbeiten der Daten und Einfügen in Cosmos DB</span><span class="sxs-lookup"><span data-stu-id="b5e70-175">Processing the data and inserting into Cosmos DB</span></span>

<span data-ttu-id="b5e70-176">Der durchschnittliche Fahrpreis für jedes Stadtviertel wird für ein bestimmtes Zeitintervall berechnet:</span><span class="sxs-lookup"><span data-stu-id="b5e70-176">The average fare amount for each neighborhood is calculated for a given time interval:</span></span>

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

<span data-ttu-id="b5e70-177">Das Ergebnis wird schließlich in Cosmos DB eingefügt:</span><span class="sxs-lookup"><span data-stu-id="b5e70-177">Which is then inserted into Cosmos DB:</span></span>

```scala
maxAvgFarePerNeighborhood
      .writeStream
      .queryName("maxAvgFarePerNeighborhood_cassandra_insert")
      .outputMode(OutputMode.Append())
      .foreach(new CassandraSinkForeach(connector))
      .start()
      .awaitTermination()
```

## <a name="security-considerations"></a><span data-ttu-id="b5e70-178">Sicherheitshinweise</span><span class="sxs-lookup"><span data-stu-id="b5e70-178">Security considerations</span></span>

<span data-ttu-id="b5e70-179">Der Zugriff auf den Arbeitsbereich der Azure-Datenbank wird über die [Administratorkonsole](https://docs.databricks.com/administration-guide/admin-settings/index.html) gesteuert.</span><span class="sxs-lookup"><span data-stu-id="b5e70-179">Access to the Azure Database workspace is controlled using the [administrator console](https://docs.databricks.com/administration-guide/admin-settings/index.html).</span></span> <span data-ttu-id="b5e70-180">Über die Administratorkonsole können Benutzer hinzugefügt, Benutzerberechtigungen verwaltet und einmaliges Anmelden eingerichtet werden.</span><span class="sxs-lookup"><span data-stu-id="b5e70-180">The administrator console includes functionality to add users, manage user permissions, and set up single sign-on.</span></span> <span data-ttu-id="b5e70-181">Die Zugriffssteuerung für Arbeitsbereiche, Cluster, Aufträge und Tabellen kann ebenfalls über die Administratorkonsole festgelegt werden.</span><span class="sxs-lookup"><span data-stu-id="b5e70-181">Access control for workspaces, clusters, jobs, and tables can also be set through the administrator console.</span></span>

### <a name="managing-secrets"></a><span data-ttu-id="b5e70-182">Verwaltung geheimer Schlüssel</span><span class="sxs-lookup"><span data-stu-id="b5e70-182">Managing secrets</span></span>

<span data-ttu-id="b5e70-183">Azure Databricks enthält einen [Geheimnisspeicher](https://docs.azuredatabricks.net/user-guide/secrets/index.html) zum Speichern von Geheimnissen (beispielsweise Verbindungszeichenfolgen, Zugriffsschlüssel, Benutzernamen und Kennwörter).</span><span class="sxs-lookup"><span data-stu-id="b5e70-183">Azure Databricks includes a [secret store](https://docs.azuredatabricks.net/user-guide/secrets/index.html) that is used to store secrets, including connection strings, access keys, user names, and passwords.</span></span> <span data-ttu-id="b5e70-184">Geheimnisse werden im Azure Databricks-Geheimnisspeicher nach **Bereichen** partitioniert:</span><span class="sxs-lookup"><span data-stu-id="b5e70-184">Secrets within the Azure Databricks secret store are partitioned by **scopes**:</span></span>

```bash
databricks secrets create-scope --scope "azure-databricks-job"
```

<span data-ttu-id="b5e70-185">Geheimnisse werden auf der Bereichsebene hinzugefügt:</span><span class="sxs-lookup"><span data-stu-id="b5e70-185">Secrets are added at the scope level:</span></span>

```bash
databricks secrets put --scope "azure-databricks-job" --key "taxi-ride"
```

> [!NOTE]
> <span data-ttu-id="b5e70-186">Anstelle des nativen Azure Databricks-Bereichs kann auch ein Azure Key Vault-basierter Bereich verwendet werden.</span><span class="sxs-lookup"><span data-stu-id="b5e70-186">An Azure Key Vault-backed scope can be used instead of the native Azure Databricks scope.</span></span> <span data-ttu-id="b5e70-187">Weitere Informationen finden Sie unter [Azure Key Vault-backed scopes](https://docs.azuredatabricks.net/user-guide/secrets/secret-scopes.html#azure-key-vault-backed-scopes) (Azure Key Vault-basierte Bereiche).</span><span class="sxs-lookup"><span data-stu-id="b5e70-187">To learn more, see [Azure Key Vault-backed scopes](https://docs.azuredatabricks.net/user-guide/secrets/secret-scopes.html#azure-key-vault-backed-scopes).</span></span>

<span data-ttu-id="b5e70-188">Im Code erfolgt der Zugriff auf Geheimnisse über die [secrets-Hilfsprogramme](https://docs.databricks.com/user-guide/dev-tools/dbutils.html#secrets-utilities) von Azure Databricks.</span><span class="sxs-lookup"><span data-stu-id="b5e70-188">In code, secrets are accessed via the Azure Databricks [secrets utilities](https://docs.databricks.com/user-guide/dev-tools/dbutils.html#secrets-utilities).</span></span>


## <a name="monitoring-considerations"></a><span data-ttu-id="b5e70-189">Aspekte der Überwachung</span><span class="sxs-lookup"><span data-stu-id="b5e70-189">Monitoring considerations</span></span>

<span data-ttu-id="b5e70-190">Azure Databricks basiert auf Apache Spark. Beide Lösungen verwenden [log4j](https://logging.apache.org/log4j/2.x/) als Standardbibliothek für die Protokollierung.</span><span class="sxs-lookup"><span data-stu-id="b5e70-190">Azure Databricks is based on Apache Spark, and both use [log4j](https://logging.apache.org/log4j/2.x/) as the standard library for logging.</span></span> <span data-ttu-id="b5e70-191">Zusätzlich zur von Apache Spark bereitgestellten Standardprotokollierung sendet diese Referenzarchitektur auch Protokolle und Metriken an [Azure Log Analytics](/azure/log-analytics/).</span><span class="sxs-lookup"><span data-stu-id="b5e70-191">In addition to the default logging provided by Apache Spark, this reference architecture sends logs and metrics to [Azure Log Analytics](/azure/log-analytics/).</span></span>

<span data-ttu-id="b5e70-192">Die Klasse **com.microsoft.pnp.TaxiCabReader** konfiguriert das Apache Spark-Protokollierungssystem so, dass dessen Protokolle an Azure Log Analytics gesendet werden. Hierbei werden die Werte aus der Datei **log4j.properties** verwendet.</span><span class="sxs-lookup"><span data-stu-id="b5e70-192">The **com.microsoft.pnp.TaxiCabReader** class configures the Apache Spark logging system to send its logs to Azure Log Analytics using the values in the **log4j.properties** file.</span></span> <span data-ttu-id="b5e70-193">Die Meldungen der Apache Spark-Protokollierung liegen als Zeichenfolgen vor. Für Azure Log Analytics werden jedoch Protokollmeldungen im JSON-Format benötigt.</span><span class="sxs-lookup"><span data-stu-id="b5e70-193">While the Apache Spark logger messages are strings, Azure Log Analytics requires log messages to be formatted as JSON.</span></span> <span data-ttu-id="b5e70-194">Die Klasse **com.microsoft.pnp.log4j.LogAnalyticsAppender** transformiert die Meldungen in das JSON-Format:</span><span class="sxs-lookup"><span data-stu-id="b5e70-194">The **com.microsoft.pnp.log4j.LogAnalyticsAppender** class transforms these messages to JSON:</span></span>

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

<span data-ttu-id="b5e70-195">Bei der Verarbeitung von Fahrt- und Fahrpreismeldungen durch die Klasse **com.microsoft.pnp.TaxiCabReader** kann es zu Problemen mit der Formatierung und somit zu ungültigen Meldungen kommen.</span><span class="sxs-lookup"><span data-stu-id="b5e70-195">As the **com.microsoft.pnp.TaxiCabReader** class processes ride and fare messages, it's possible that either one may be malformed and therefore not valid.</span></span> <span data-ttu-id="b5e70-196">In einer Produktionsumgebung müssen diese falsch formatierten Meldungen unbedingt analysiert werden, um ein Problem mit den Datenquellen zu erkennen und schnell zu beheben, damit keine Daten verloren gehen.</span><span class="sxs-lookup"><span data-stu-id="b5e70-196">In a production environment, it's important to analyze these malformed messages to identify a problem with the data sources so it can be fixed quickly to prevent data loss.</span></span> <span data-ttu-id="b5e70-197">Die Klasse **com.microsoft.pnp.TaxiCabReader** registriert einen Apache Spark-Akkumulator zur Nachverfolgung der Anzahl falsch formatierter Fahrpreis- und Fahrtdatensätze:</span><span class="sxs-lookup"><span data-stu-id="b5e70-197">The **com.microsoft.pnp.TaxiCabReader** class registers an Apache Spark Accumulator that keeps track of the number of malformed fare and ride records:</span></span>

```scala
    @transient val appMetrics = new AppMetrics(spark.sparkContext)
    appMetrics.registerGauge("metrics.malformedrides", AppAccumulators.getRideInstance(spark.sparkContext))
    appMetrics.registerGauge("metrics.malformedfares", AppAccumulators.getFareInstance(spark.sparkContext))
    SparkEnv.get.metricsSystem.registerSource(appMetrics)
```

<span data-ttu-id="b5e70-198">Apache Spark verwendet die Dropwizard-Bibliothek, um Metriken zu senden. Einige der nativen Dropwizard-Metrikfelder sind jedoch nicht mit Azure Log Analytics kompatibel.</span><span class="sxs-lookup"><span data-stu-id="b5e70-198">Apache Spark uses the Dropwizard library to send metrics, and some of the native Dropwizard metrics fields are incompatible with Azure Log Analytics.</span></span> <span data-ttu-id="b5e70-199">Daher enthält diese Referenzarchitektur eine benutzerdefinierte Dropwizard-Senke und einen entsprechenden Reporter.</span><span class="sxs-lookup"><span data-stu-id="b5e70-199">Therefore, this reference architecture includes a custom Dropwizard sink and reporter.</span></span> <span data-ttu-id="b5e70-200">Die Metriken werden so formatiert, wie es von Azure Log Analytics erwartet wird.</span><span class="sxs-lookup"><span data-stu-id="b5e70-200">It formats the metrics in the format expected by Azure Log Analytics.</span></span> <span data-ttu-id="b5e70-201">Wenn Apache Spark Metriken meldet, werden auch die benutzerdefinierten Metriken für die falsch formatierten Fahrt- und Fahrpreisdaten gesendet.</span><span class="sxs-lookup"><span data-stu-id="b5e70-201">When Apache Spark reports metrics, the custom metrics for the malformed ride and fare data are also sent.</span></span>

<span data-ttu-id="b5e70-202">Als letzte Metrik wird im Azure Log Analytics-Arbeitsbereich der kumulative Fortschritt des strukturierten Spark-Streamingauftrags protokolliert.</span><span class="sxs-lookup"><span data-stu-id="b5e70-202">The last metric to be logged to the Azure Log Analytics workspace is the cumulative progress of the Spark Structured Streaming job progress.</span></span> <span data-ttu-id="b5e70-203">Hierzu wird in der Klasse **com.microsoft.pnp.StreamingMetricsListener** ein benutzerdefinierter StreamingQuery-Listener implementiert.</span><span class="sxs-lookup"><span data-stu-id="b5e70-203">This is done using a custom StreamingQuery listener implemented in the **com.microsoft.pnp.StreamingMetricsListener** class.</span></span> <span data-ttu-id="b5e70-204">Diese Klasse wird beim Ausführen des Auftrags bei der Apache Spark-Sitzung registriert:</span><span class="sxs-lookup"><span data-stu-id="b5e70-204">This class is registered to the Apache Spark Session when the job runs:</span></span>

```scala
spark.streams.addListener(new StreamingMetricsListener())
```

<span data-ttu-id="b5e70-205">Die Methoden in „StreamingMetricsListener“ werden von der Apache Spark-Runtime aufgerufen, wenn ein strukturiertes Streamingereignis auftritt. Dabei werden Protokollmeldungen und Metriken an den Azure Log Analytics-Arbeitsbereich gesendet.</span><span class="sxs-lookup"><span data-stu-id="b5e70-205">The methods in the StreamingMetricsListener are called by the Apache Spark runtime whenever a structured steaming event occurs, sending log messages and metrics to the Azure Log Analytics workspace.</span></span> <span data-ttu-id="b5e70-206">Zur Überwachung der Anwendung können Sie in Ihrem Arbeitsbereich die folgenden Abfragen verwenden:</span><span class="sxs-lookup"><span data-stu-id="b5e70-206">You can use the following queries in your workspace to monitor the application:</span></span>

### <a name="latency-and-throughput-for-streaming-queries"></a><span data-ttu-id="b5e70-207">Wartezeit und Durchsatz für Streamingabfragen</span><span class="sxs-lookup"><span data-stu-id="b5e70-207">Latency and throughput for streaming queries</span></span> 

```shell
taxijob_CL
| where TimeGenerated > startofday(datetime(<date>)) and TimeGenerated < endofday(datetime(<date>))
| project  mdc_inputRowsPerSecond_d, mdc_durationms_triggerExecution_d  
| render timechart
``` 
### <a name="exceptions-logged-during-stream-query-execution"></a><span data-ttu-id="b5e70-208">Ausnahmen, die während der Ausführung von Datenstromabfragen protokolliert wurden</span><span class="sxs-lookup"><span data-stu-id="b5e70-208">Exceptions logged during stream query execution</span></span>

```shell
taxijob_CL
| where TimeGenerated > startofday(datetime(<date>)) and TimeGenerated < endofday(datetime(<date>))
| where Level contains "Error" 
```

### <a name="accumulation-of-malformed-fare-and-ride-data"></a><span data-ttu-id="b5e70-209">Kumulation falsch formatierter Fahrpreis- und Fahrtdaten</span><span class="sxs-lookup"><span data-stu-id="b5e70-209">Accumulation of malformed fare and ride data</span></span>

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

### <a name="job-execution-to-trace-resiliency"></a><span data-ttu-id="b5e70-210">Auftragsausführung zur Nachverfolgung der Resilienz</span><span class="sxs-lookup"><span data-stu-id="b5e70-210">Job execution to trace resiliency</span></span>

```shell
SparkMetric_CL 
| where TimeGenerated > startofday(datetime(<date>)) and TimeGenerated < endofday(datetime(<date>))
| render timechart 
| where name_s contains "driver.DAGScheduler.job.allJobs" 
```

## <a name="deploy-the-solution"></a><span data-ttu-id="b5e70-211">Bereitstellen der Lösung</span><span class="sxs-lookup"><span data-stu-id="b5e70-211">Deploy the solution</span></span>

<span data-ttu-id="b5e70-212">Eine Bereitstellung für diese Referenzarchitektur ist auf [GitHub](https://github.com/mspnp/azure-databricks-streaming-analytics) verfügbar.</span><span class="sxs-lookup"><span data-stu-id="b5e70-212">A deployment for this reference architecture is available on [GitHub](https://github.com/mspnp/azure-databricks-streaming-analytics).</span></span> 

### <a name="prerequisites"></a><span data-ttu-id="b5e70-213">Voraussetzungen</span><span class="sxs-lookup"><span data-stu-id="b5e70-213">Prerequisites</span></span>

1. <span data-ttu-id="b5e70-214">Klonen oder forken Sie das GitHub-Repository [stream processing with Azure Databricks](https://github.com/mspnp/azure-databricks-streaming-analytics) (Datenstromverarbeitung mit Azure Databricks), oder laden Sie es herunter.</span><span class="sxs-lookup"><span data-stu-id="b5e70-214">Clone, fork, or download the [stream processing with Azure Databricks](https://github.com/mspnp/azure-databricks-streaming-analytics) GitHub repository.</span></span>

2. <span data-ttu-id="b5e70-215">Installieren Sie [Docker](https://www.docker.com/), um den Datengenerator auszuführen.</span><span class="sxs-lookup"><span data-stu-id="b5e70-215">Install [Docker](https://www.docker.com/) to run the data generator.</span></span>

3. <span data-ttu-id="b5e70-216">Installieren Sie [Azure CLI 2.0](/cli/azure/install-azure-cli?view=azure-cli-latest).</span><span class="sxs-lookup"><span data-stu-id="b5e70-216">Install [Azure CLI 2.0](/cli/azure/install-azure-cli?view=azure-cli-latest).</span></span>

4. <span data-ttu-id="b5e70-217">Installieren Sie die [Databricks-Befehlszeilenschnittstelle](https://docs.databricks.com/user-guide/dev-tools/databricks-cli.html).</span><span class="sxs-lookup"><span data-stu-id="b5e70-217">Install [Databricks CLI](https://docs.databricks.com/user-guide/dev-tools/databricks-cli.html).</span></span>

5. <span data-ttu-id="b5e70-218">Melden Sie sich über eine Eingabeaufforderung, eine Bash-Eingabeaufforderung oder die PowerShell-Eingabeaufforderung folgendermaßen bei Ihrem Azure-Konto an:</span><span class="sxs-lookup"><span data-stu-id="b5e70-218">From a command prompt, bash prompt, or PowerShell prompt, sign into your Azure account as follows:</span></span>
    ```shell
    az login
    ```
6. <span data-ttu-id="b5e70-219">Installieren Sie eine Java-IDE mit folgenden Ressourcen:</span><span class="sxs-lookup"><span data-stu-id="b5e70-219">Install a Java IDE, with the following resources:</span></span>
    - <span data-ttu-id="b5e70-220">JDK 1.8</span><span class="sxs-lookup"><span data-stu-id="b5e70-220">JDK 1.8</span></span>
    - <span data-ttu-id="b5e70-221">Scala SDK 2.11</span><span class="sxs-lookup"><span data-stu-id="b5e70-221">Scala SDK 2.11</span></span>
    - <span data-ttu-id="b5e70-222">Maven 3.5.4</span><span class="sxs-lookup"><span data-stu-id="b5e70-222">Maven 3.5.4</span></span>

### <a name="download-the-new-york-city-taxi-and-neighborhood-data-files"></a><span data-ttu-id="b5e70-223">Herunterladen der Datendateien für New Yorker Taxis und Stadtviertel</span><span class="sxs-lookup"><span data-stu-id="b5e70-223">Download the New York City taxi and neighborhood data files</span></span>

1. <span data-ttu-id="b5e70-224">Erstellen Sie in Ihrem lokalen Dateisystem am Stamm des geklonten Github-Repositorys ein Verzeichnis namens `DataFile`.</span><span class="sxs-lookup"><span data-stu-id="b5e70-224">Create a directory named `DataFile` in the root of the cloned Github repository in your local file system.</span></span>

2. <span data-ttu-id="b5e70-225">Öffnen Sie einen Webbrowser, und navigieren Sie zu https://uofi.app.box.com/v/NYCtaxidata/folder/2332219935.</span><span class="sxs-lookup"><span data-stu-id="b5e70-225">Open a web browser and navigate to https://uofi.app.box.com/v/NYCtaxidata/folder/2332219935.</span></span>

3. <span data-ttu-id="b5e70-226">Klicken Sie auf dieser Seite auf die Schaltfläche zum **Herunterladen**, um eine ZIP-Datei mit den Taxidaten für dieses Jahr herunterzuladen.</span><span class="sxs-lookup"><span data-stu-id="b5e70-226">Click the **Download** button on this page to download a zip file of all the taxi data for that year.</span></span>

4. <span data-ttu-id="b5e70-227">Extrahieren Sie die ZIP-Datei in das Verzeichnis `DataFile`.</span><span class="sxs-lookup"><span data-stu-id="b5e70-227">Extract the zip file to the `DataFile` directory.</span></span>

    > [!NOTE]
    > <span data-ttu-id="b5e70-228">Diese ZIP-Datei enthält weitere ZIP-Dateien.</span><span class="sxs-lookup"><span data-stu-id="b5e70-228">This zip file contains other zip files.</span></span> <span data-ttu-id="b5e70-229">Extrahieren Sie die untergeordneten ZIP-Dateien nicht.</span><span class="sxs-lookup"><span data-stu-id="b5e70-229">Don't extract the child zip files.</span></span>

    <span data-ttu-id="b5e70-230">Die Verzeichnisstruktur muss wie folgt aussehen:</span><span class="sxs-lookup"><span data-stu-id="b5e70-230">The directory structure must look like the following:</span></span>

    ```shell
    /DataFile
        /FOIL2013
            trip_data_1.zip
            trip_data_2.zip
            trip_data_3.zip
            ...
    ```

5. <span data-ttu-id="b5e70-231">Öffnen Sie einen Webbrowser, und navigieren Sie zu https://www.zillow.com/howto/api/neighborhood-boundaries.htm.</span><span class="sxs-lookup"><span data-stu-id="b5e70-231">Open a web browser and navigate to https://www.zillow.com/howto/api/neighborhood-boundaries.htm.</span></span> 

6. <span data-ttu-id="b5e70-232">Klicken Sie auf **New York Neighborhood Boundaries** (Grenzen der Stadtviertel von New York), um die Datei herunterzuladen.</span><span class="sxs-lookup"><span data-stu-id="b5e70-232">Click on **New York Neighborhood Boundaries** to download the file.</span></span>

7. <span data-ttu-id="b5e70-233">Kopieren Sie die Datei **ZillowNeighborhoods-NY.zip** aus dem Downloadverzeichnis**Ihres** Browsers in das `DataFile` Verzeichnis.</span><span class="sxs-lookup"><span data-stu-id="b5e70-233">Copy the **ZillowNeighborhoods-NY.zip** file from your browser's **downloads** directory to the `DataFile` directory.</span></span>

### <a name="deploy-the-azure-resources"></a><span data-ttu-id="b5e70-234">Bereitstellen der Azure-Ressourcen</span><span class="sxs-lookup"><span data-stu-id="b5e70-234">Deploy the Azure resources</span></span>

1. <span data-ttu-id="b5e70-235">Führen Sie an einer Shell- oder Windows-Eingabeaufforderung den folgenden Befehl aus, und folgen Sie den Anweisungen in der Anmeldeaufforderung:</span><span class="sxs-lookup"><span data-stu-id="b5e70-235">From a shell or Windows Command Prompt, run the following command and follow the sign-in prompt:</span></span>

    ```bash
    az login
    ```

2. <span data-ttu-id="b5e70-236">Navigieren Sie im GitHub-Repository zum Ordner `azure`:</span><span class="sxs-lookup"><span data-stu-id="b5e70-236">Navigate to the folder named `azure` in the GitHub repository:</span></span>

    ```bash
    cd azure
    ```

3. <span data-ttu-id="b5e70-237">Führen Sie die folgenden Befehle aus, um die Azure-Ressourcen bereitzustellen:</span><span class="sxs-lookup"><span data-stu-id="b5e70-237">Run the following commands to deploy the Azure resources:</span></span>

    ```bash
    export resourceGroup='[Resource group name]'
    export resourceLocation='[Region]'
    export eventHubNamespace='[Event Hubs namespace name]'
    export databricksWorkspaceName='[Azure Databricks workspace name]'
    export cosmosDatabaseAccount='[Cosmos DB database name]'
    export logAnalyticsWorkspaceName='[Log Analytics workspace name]'
    export logAnalyticsWorkspaceRegion='[Log Analytics region]'

    # Create a resource group
    az group create --name $resourceGroup --location $resourceLocation

    # Deploy resources
    az group deployment create --resource-group $resourceGroup \
        --template-file deployresources.json --parameters \
        eventHubNamespace=$eventHubNamespace \
        databricksWorkspaceName=$databricksWorkspaceName \
        cosmosDatabaseAccount=$cosmosDatabaseAccount \
        logAnalyticsWorkspaceName=$logAnalyticsWorkspaceName \
        logAnalyticsWorkspaceRegion=$logAnalyticsWorkspaceRegion
    ```

4. <span data-ttu-id="b5e70-238">Die Ausgabe der Bereitstellung wird nach Abschluss des Vorgangs in der Konsole angezeigt.</span><span class="sxs-lookup"><span data-stu-id="b5e70-238">The output of the deployment is written to the console once complete.</span></span> <span data-ttu-id="b5e70-239">Suchen Sie in der Ausgabe nach folgendem JSON-Code:</span><span class="sxs-lookup"><span data-stu-id="b5e70-239">Search the output for the following JSON:</span></span>

```JSON
"outputs": {
        "cosmosDb": {
          "type": "Object",
          "value": {
            "hostName": <value>,
            "secret": <value>,
            "username": <value>
          }
        },
        "eventHubs": {
          "type": "Object",
          "value": {
            "taxi-fare-eh": <value>,
            "taxi-ride-eh": <value>
          }
        },
        "logAnalytics": {
          "type": "Object",
          "value": {
            "secret": <value>,
            "workspaceId": <value>
          }
        }
},
```
<span data-ttu-id="b5e70-240">Bei diesen Werten handelt es sich um die Geheimnisse, die in späteren Abschnitten zu Databricks-Geheimnissen hinzugefügt werden.</span><span class="sxs-lookup"><span data-stu-id="b5e70-240">These values are the secrets that will be added to Databricks secrets in upcoming sections.</span></span> <span data-ttu-id="b5e70-241">Bewahren Sie sie bis dahin sicher auf.</span><span class="sxs-lookup"><span data-stu-id="b5e70-241">Keep them secure until you add them in those sections.</span></span>

### <a name="add-a-cassandra-table-to-the-cosmos-db-account"></a><span data-ttu-id="b5e70-242">Hinzufügen einer Cassandra-Tabelle zum Cosmos DB-Konto</span><span class="sxs-lookup"><span data-stu-id="b5e70-242">Add a Cassandra table to the Cosmos DB Account</span></span>

1. <span data-ttu-id="b5e70-243">Navigieren Sie im Azure-Portal zu der Ressourcengruppe, die weiter oben im Abschnitt **Bereitstellen der Azure-Ressourcen** erstellt wurde.</span><span class="sxs-lookup"><span data-stu-id="b5e70-243">In the Azure portal, navigate to the resource group created in the **deploy the Azure resources** section above.</span></span> <span data-ttu-id="b5e70-244">Klicken Sie auf **Azure Cosmos DB-Konto**.</span><span class="sxs-lookup"><span data-stu-id="b5e70-244">Click on **Azure Cosmos DB Account**.</span></span> <span data-ttu-id="b5e70-245">Erstellen Sie eine Tabelle mit der Cassandra-API.</span><span class="sxs-lookup"><span data-stu-id="b5e70-245">Create a table with the Cassandra API.</span></span>

2. <span data-ttu-id="b5e70-246">Klicken Sie auf dem Blatt **Übersicht** auf **Tabelle hinzufügen**.</span><span class="sxs-lookup"><span data-stu-id="b5e70-246">In the **overview** blade, click **add table**.</span></span>

3. <span data-ttu-id="b5e70-247">Daraufhin wird das Blatt **Tabelle hinzufügen** geöffnet. Geben Sie dort `newyorktaxi` in das Textfeld **Keyspace name** (Keyspace-Name) ein.</span><span class="sxs-lookup"><span data-stu-id="b5e70-247">When the **add table** blade opens, enter `newyorktaxi` in the **Keyspace name** text box.</span></span> 

4. <span data-ttu-id="b5e70-248">Geben Sie im Abschnitt **enter CQL command to create the table** (CQL-Befehl eingeben, um Tabelle zu erstellen) die Zeichenfolge `neighborhoodstats` in das Textfeld neben `newyorktaxi` ein.</span><span class="sxs-lookup"><span data-stu-id="b5e70-248">In the **enter CQL command to create the table** section, enter `neighborhoodstats` in the text box beside `newyorktaxi`.</span></span>

5. <span data-ttu-id="b5e70-249">Geben Sie im Textfeld darunter Folgendes ein:</span><span class="sxs-lookup"><span data-stu-id="b5e70-249">In the text box below, enter the following::</span></span>
```shell
(neighborhood text, window_end timestamp, number_of_rides bigint,total_fare_amount double, primary key(neighborhood, window_end))
```
6. <span data-ttu-id="b5e70-250">Geben Sie im Textfeld **Durchsatz (1.000–1.000.000 RU/s)** den Wert `4000` ein.</span><span class="sxs-lookup"><span data-stu-id="b5e70-250">In the **Throughput (1,000 - 1,000,000 RU/s)** text box enter the value `4000`.</span></span>

7. <span data-ttu-id="b5e70-251">Klicken Sie auf **OK**.</span><span class="sxs-lookup"><span data-stu-id="b5e70-251">Click **OK**.</span></span>

### <a name="add-the-databricks-secrets-using-the-databricks-cli"></a><span data-ttu-id="b5e70-252">Hinzufügen der Databricks-Geheimnisse mithilfe der Databricks-Befehlszeilenschnittstelle</span><span class="sxs-lookup"><span data-stu-id="b5e70-252">Add the Databricks secrets using the Databricks CLI</span></span>

<span data-ttu-id="b5e70-253">Geben Sie zunächst die Geheimnisse für Event Hub ein:</span><span class="sxs-lookup"><span data-stu-id="b5e70-253">First, enter the secrets for EventHub:</span></span>

1. <span data-ttu-id="b5e70-254">Erstellen Sie mithilfe der **Azure Databricks-Befehlszeilenschnittstelle**, die Sie im zweiten Vorbereitungsschritt installiert haben, den Azure Databricks-Geheimnisbereich:</span><span class="sxs-lookup"><span data-stu-id="b5e70-254">Using the **Azure Databricks CLI** installed in step 2 of the prerequisites, create the Azure Databricks secret scope:</span></span>
    ```shell
    databricks secrets create-scope --scope "azure-databricks-job"
    ```
2. <span data-ttu-id="b5e70-255">Fügen Sie das Geheimnis für den Taxifahrt-Event Hub hinzu:</span><span class="sxs-lookup"><span data-stu-id="b5e70-255">Add the secret for the taxi ride EventHub:</span></span>
    ```shell
    databricks secrets put --scope "azure-databricks-job" --key "taxi-ride"
    ```
    <span data-ttu-id="b5e70-256">Durch diesen Befehl wird der vi-Editor geöffnet.</span><span class="sxs-lookup"><span data-stu-id="b5e70-256">Once executed, this command opens the vi editor.</span></span> <span data-ttu-id="b5e70-257">Geben Sie den **taxi-ride-eh**-Wert aus dem **eventHubs**-Ausgabeabschnitt aus Schritt 4 des Abschnitts *Bereitstellen der Azure-Ressourcen* ein.</span><span class="sxs-lookup"><span data-stu-id="b5e70-257">Enter the **taxi-ride-eh** value from the **eventHubs** output section in step 4 of the *deploy the Azure resources* section.</span></span> <span data-ttu-id="b5e70-258">Speichern Sie Ihre Angaben, und beenden Sie vi.</span><span class="sxs-lookup"><span data-stu-id="b5e70-258">Save and exit vi.</span></span>

3. <span data-ttu-id="b5e70-259">Fügen Sie das Geheimnis für den Fahrpreis-Event Hub hinzu:</span><span class="sxs-lookup"><span data-stu-id="b5e70-259">Add the secret for the taxi fare EventHub:</span></span>
    ```shell
    databricks secrets put --scope "azure-databricks-job" --key "taxi-fare"
    ```
    <span data-ttu-id="b5e70-260">Durch diesen Befehl wird der vi-Editor geöffnet.</span><span class="sxs-lookup"><span data-stu-id="b5e70-260">Once executed, this command opens the vi editor.</span></span> <span data-ttu-id="b5e70-261">Geben Sie den **taxi-fare-eh**-Wert aus dem **eventHubs**-Ausgabeabschnitt aus Schritt 4 des Abschnitts *Bereitstellen der Azure-Ressourcen* ein.</span><span class="sxs-lookup"><span data-stu-id="b5e70-261">Enter the **taxi-fare-eh** value from the **eventHubs** output section in step 4 of the *deploy the Azure resources* section.</span></span> <span data-ttu-id="b5e70-262">Speichern Sie Ihre Angaben, und beenden Sie vi.</span><span class="sxs-lookup"><span data-stu-id="b5e70-262">Save and exit vi.</span></span>

<span data-ttu-id="b5e70-263">Geben Sie als Nächstes die Geheimnisse für Cosmos DB ein:</span><span class="sxs-lookup"><span data-stu-id="b5e70-263">Next, enter the secrets for Cosmos DB:</span></span>

1. <span data-ttu-id="b5e70-264">Navigieren Sie im Azure-Portal zu der Ressourcengruppe, die Sie in Schritt 3 des Abschnitts **Bereitstellen der Azure-Ressourcen** angegeben haben.</span><span class="sxs-lookup"><span data-stu-id="b5e70-264">Open the Azure portal, and navigate to the resource group specified in step 3 of the **deploy the Azure resources** section.</span></span> <span data-ttu-id="b5e70-265">Klicken Sie auf das Azure Cosmos DB-Konto.</span><span class="sxs-lookup"><span data-stu-id="b5e70-265">Click on the Azure Cosmos DB Account.</span></span>

2. <span data-ttu-id="b5e70-266">Fügen Sie mithilfe der **Azure Databricks-Befehlszeilenschnittstelle** das Geheimnis für den Cosmos DB-Benutzernamen hinzu:</span><span class="sxs-lookup"><span data-stu-id="b5e70-266">Using the **Azure Databricks CLI**, add the secret for the Cosmos DB user name:</span></span>
    ```shell
    databricks secrets put --scope azure-databricks-job --key "cassandra-username"
    ```
<span data-ttu-id="b5e70-267">Durch diesen Befehl wird der vi-Editor geöffnet.</span><span class="sxs-lookup"><span data-stu-id="b5e70-267">Once executed, this command opens the vi editor.</span></span> <span data-ttu-id="b5e70-268">Geben Sie den **username**-Wert aus dem **CosmosDb**-Ausgabeabschnitt aus Schritt 4 des Abschnitts *Bereitstellen der Azure-Ressourcen* ein.</span><span class="sxs-lookup"><span data-stu-id="b5e70-268">Enter the **username** value from the **CosmosDb** output section in step 4 of the *deploy the Azure resources* section.</span></span> <span data-ttu-id="b5e70-269">Speichern Sie Ihre Angaben, und beenden Sie vi.</span><span class="sxs-lookup"><span data-stu-id="b5e70-269">Save and exit vi.</span></span>

3. <span data-ttu-id="b5e70-270">Fügen Sie als Nächstes das Geheimnis für das Cosmos DB-Kennwort hinzu:</span><span class="sxs-lookup"><span data-stu-id="b5e70-270">Next, add the secret for the Cosmos DB password:</span></span>
    ```shell
    databricks secrets put --scope azure-databricks-job --key "cassandra-password"
    ```

<span data-ttu-id="b5e70-271">Durch diesen Befehl wird der vi-Editor geöffnet.</span><span class="sxs-lookup"><span data-stu-id="b5e70-271">Once executed, this command opens the vi editor.</span></span> <span data-ttu-id="b5e70-272">Geben Sie den **secret**-Wert aus dem **CosmosDb**-Ausgabeabschnitt aus Schritt 4 des Abschnitts *Bereitstellen der Azure-Ressourcen* ein.</span><span class="sxs-lookup"><span data-stu-id="b5e70-272">Enter the **secret** value from the **CosmosDb** output section in step 4 of the *deploy the Azure resources* section.</span></span> <span data-ttu-id="b5e70-273">Speichern Sie Ihre Angaben, und beenden Sie vi.</span><span class="sxs-lookup"><span data-stu-id="b5e70-273">Save and exit vi.</span></span>

> [!NOTE]
> <span data-ttu-id="b5e70-274">Wenn Sie einen [Azure Key Vault-basierten Geheimnisbereich](https://docs.azuredatabricks.net/user-guide/secrets/secret-scopes.html#azure-key-vault-backed-scopes) verwenden, muss der Bereich **azure-databricks-job** heißen, und die Geheimnisse müssen exakt die oben angegebenen Namen haben.</span><span class="sxs-lookup"><span data-stu-id="b5e70-274">If using an [Azure Key Vault-backed secret scope](https://docs.azuredatabricks.net/user-guide/secrets/secret-scopes.html#azure-key-vault-backed-scopes), the scope must be named **azure-databricks-job** and the secrets must have the exact same names as those above.</span></span>

### <a name="add-the-zillow-neighborhoods-data-file-to-the-databricks-file-system"></a><span data-ttu-id="b5e70-275">Hinzufügen der Datendatei „Zillow Neighborhoods“ zum Databricks-Dateisystem</span><span class="sxs-lookup"><span data-stu-id="b5e70-275">Add the Zillow Neighborhoods data file to the Databricks file system</span></span>

1. <span data-ttu-id="b5e70-276">Erstellen Sie ein Verzeichnis im Databricks-Dateisystem:</span><span class="sxs-lookup"><span data-stu-id="b5e70-276">Create a directory in the Databricks file system:</span></span>
    ```bash
    dbfs mkdirs dbfs:/azure-databricks-jobs
    ```

2. <span data-ttu-id="b5e70-277">Navigieren Sie zum Verzeichnis `DataFile`, und geben Sie Folgendes ein:</span><span class="sxs-lookup"><span data-stu-id="b5e70-277">Navigate to the `DataFile` directory and enter the following:</span></span>
    ```bash
    dbfs cp ZillowNeighborhoods-NY.zip dbfs:/azure-databricks-jobs
    ```

### <a name="add-the-azure-log-analytics-workspace-id-and-primary-key-to-configuration-files"></a><span data-ttu-id="b5e70-278">Hinzufügen der Log Analytics-Arbeitsbereich-ID und des Primärschlüssels zu Konfigurationsdateien</span><span class="sxs-lookup"><span data-stu-id="b5e70-278">Add the Azure Log Analytics workspace ID and primary key to configuration files</span></span>

<span data-ttu-id="b5e70-279">In diesem Abschnitt benötigen Sie die Log Analytics-Arbeitsbereich-ID und den Primärschlüssel.</span><span class="sxs-lookup"><span data-stu-id="b5e70-279">For this section, you require the Log Analytics workspace ID and primary key.</span></span> <span data-ttu-id="b5e70-280">Bei der Arbeitsbereich-ID handelt es sich um den **workspaceId**-Wert aus dem **logAnalytics**-Ausgabeabschnitt aus Schritt 4 des Abschnitts *Bereitstellen der Azure-Ressourcen*.</span><span class="sxs-lookup"><span data-stu-id="b5e70-280">The workspace ID is the **workspaceId** value from the **logAnalytics** output section in step 4 of the *deploy the Azure resources* section.</span></span> <span data-ttu-id="b5e70-281">Der Primärschlüssel ist der **secret**-Wert aus dem Ausgabeabschnitt.</span><span class="sxs-lookup"><span data-stu-id="b5e70-281">The primary key is the **secret** from the output section.</span></span> 

1. <span data-ttu-id="b5e70-282">Öffnen Sie `\azure\AzureDataBricksJob\src\main\resources\com\microsoft\pnp\azuredatabricksjob\log4j.properties`, um die log4j-Protokollierung zu konfigurieren.</span><span class="sxs-lookup"><span data-stu-id="b5e70-282">To configure log4j logging, open `\azure\AzureDataBricksJob\src\main\resources\com\microsoft\pnp\azuredatabricksjob\log4j.properties`.</span></span> <span data-ttu-id="b5e70-283">Bearbeiten Sie die beiden folgenden Werte:</span><span class="sxs-lookup"><span data-stu-id="b5e70-283">Edit the following two values:</span></span>
    ```shell
    log4j.appender.A1.workspaceId=<Log Analytics workspace ID>
    log4j.appender.A1.secret=<Log Analytics primary key>
    ```

2. <span data-ttu-id="b5e70-284">Öffnen Sie `\azure\azure-databricks-monitoring\scripts\metrics.properties`, um die benutzerdefinierte Protokollierung zu konfigurieren.</span><span class="sxs-lookup"><span data-stu-id="b5e70-284">To configure custom logging, open `\azure\azure-databricks-monitoring\scripts\metrics.properties`.</span></span> <span data-ttu-id="b5e70-285">Bearbeiten Sie die beiden folgenden Werte:</span><span class="sxs-lookup"><span data-stu-id="b5e70-285">Edit the following two values:</span></span>
    ```shell
    *.sink.loganalytics.workspaceId=<Log Analytics workspace ID>
    *.sink.loganalytics.secret=<Log Analytics primary key>
    ```

### <a name="build-the-jar-files-for-the-databricks-job-and-databricks-monitoring"></a><span data-ttu-id="b5e70-286">Erstellen der JAR-Dateien für den Databricks-Auftrag und die Databricks-Überwachung</span><span class="sxs-lookup"><span data-stu-id="b5e70-286">Build the .jar files for the Databricks job and Databricks monitoring</span></span>

1. <span data-ttu-id="b5e70-287">Importieren Sie mithilfe der Java-IDE die Maven-Projektdatei **pom.xml** aus dem Stammverzeichnis.</span><span class="sxs-lookup"><span data-stu-id="b5e70-287">Use your Java IDE to import the Maven project file named **pom.xml** located in the root directory.</span></span> 

2. <span data-ttu-id="b5e70-288">Erstellen Sie einen bereinigten Build.</span><span class="sxs-lookup"><span data-stu-id="b5e70-288">Perform a clean build.</span></span> <span data-ttu-id="b5e70-289">Dieser Build gibt die Dateien **azure-databricks-job-1.0-SNAPSHOT.jar** und **azure-databricks-monitoring-0.9.jar** aus.</span><span class="sxs-lookup"><span data-stu-id="b5e70-289">The output of this build is files named **azure-databricks-job-1.0-SNAPSHOT.jar** and **azure-databricks-monitoring-0.9.jar**.</span></span> 

### <a name="configure-custom-logging-for-the-databricks-job"></a><span data-ttu-id="b5e70-290">Konfigurieren der benutzerdefinierten Protokollierung für den Databricks-Auftrag</span><span class="sxs-lookup"><span data-stu-id="b5e70-290">Configure custom logging for the Databricks job</span></span>

1. <span data-ttu-id="b5e70-291">Kopieren Sie die Datei **azure-databricks-monitoring-0.9.jar** in das Databricks-Dateisystem, indem Sie den folgenden Befehl in die **Databricks-Befehlszeilenschnittstelle** eingeben:</span><span class="sxs-lookup"><span data-stu-id="b5e70-291">Copy the **azure-databricks-monitoring-0.9.jar** file to the Databricks file system by entering the following command in the **Databricks CLI**:</span></span>
    ```shell
    databricks fs cp --overwrite azure-databricks-monitoring-0.9.jar dbfs:/azure-databricks-job/azure-databricks-monitoring-0.9.jar
    ```

2. <span data-ttu-id="b5e70-292">Kopieren Sie die Eigenschaften der benutzerdefinierten Protokollierung aus `\azure\azure-databricks-monitoring\scripts\metrics.properties` in das Databricks-Dateisystem. Verwenden Sie dazu den folgenden Befehl:</span><span class="sxs-lookup"><span data-stu-id="b5e70-292">Copy the custom logging properties from `\azure\azure-databricks-monitoring\scripts\metrics.properties` to the Databricks file system by entering the following command:</span></span>
    ```shell
    databricks fs cp --overwrite metrics.properties dbfs:/azure-databricks-job/metrics.properties
    ```

3. <span data-ttu-id="b5e70-293">Wählen Sie einen Namen für Ihren Databricks-Cluster.</span><span class="sxs-lookup"><span data-stu-id="b5e70-293">While you haven't yet decided on a name for your Databricks cluster, select one now.</span></span> <span data-ttu-id="b5e70-294">Der Name muss weiter unten im Databricks-Dateisystempfad für Ihren Cluster eingegeben werden.</span><span class="sxs-lookup"><span data-stu-id="b5e70-294">You'll enter the name below in the Databricks file system path for your cluster.</span></span> <span data-ttu-id="b5e70-295">Kopieren Sie das Initialisierungsskript aus `\azure\azure-databricks-monitoring\scripts\spark.metrics` in das Databricks-Dateisystem. Verwenden Sie dazu den folgenden Befehl:</span><span class="sxs-lookup"><span data-stu-id="b5e70-295">Copy the initialization script from `\azure\azure-databricks-monitoring\scripts\spark.metrics` to the Databricks file system by entering the following command:</span></span>
    ```
    databricks fs cp --overwrite spark-metrics.sh dbfs:/databricks/init/<cluster-name>/spark-metrics.sh
    ```

### <a name="create-a-databricks-cluster"></a><span data-ttu-id="b5e70-296">Erstellen eines Databricks-Clusters</span><span class="sxs-lookup"><span data-stu-id="b5e70-296">Create a Databricks cluster</span></span>

1. <span data-ttu-id="b5e70-297">Klicken Sie im Databricks-Arbeitsbereich auf „Cluster“ und anschließend auf „Cluster erstellen“.</span><span class="sxs-lookup"><span data-stu-id="b5e70-297">In the Databricks workspace, click "Clusters", then click "create cluster".</span></span> <span data-ttu-id="b5e70-298">Geben Sie den Clusternamen ein, den Sie weiter oben in Schritt 3 des Abschnitts **Konfigurieren der benutzerdefinierten Protokollierung für den Databricks-Auftrag** erstellt haben.</span><span class="sxs-lookup"><span data-stu-id="b5e70-298">Enter the cluster name you created in step 3 of the **configure custom logging for the Databricks job** section above.</span></span> 

2. <span data-ttu-id="b5e70-299">Wählen Sie einen**Standardclustermodus** aus.</span><span class="sxs-lookup"><span data-stu-id="b5e70-299">Select a **standard** cluster mode.</span></span>

3. <span data-ttu-id="b5e70-300">Legen Sie die **Databricks-Runtimeversion** auf **4.3** (beinhaltet Apache Spark 2.3.1 und Scala 2.11) fest.</span><span class="sxs-lookup"><span data-stu-id="b5e70-300">Set **Databricks runtime version** to **4.3 (includes Apache Spark 2.3.1, Scala 2.11)**</span></span>

4. <span data-ttu-id="b5e70-301">Legen Sie die **Python-Version** auf **2** fest.</span><span class="sxs-lookup"><span data-stu-id="b5e70-301">Set **Python version** to **2**.</span></span>

5. <span data-ttu-id="b5e70-302">Legen Sie den **Treibertyp** auf **Same as worker** (Identisch mit Worker) fest.</span><span class="sxs-lookup"><span data-stu-id="b5e70-302">Set **Driver Type** to **Same as worker**</span></span>

6. <span data-ttu-id="b5e70-303">Legen Sie den **Workertyp** auf **Standard_DS3_v2** fest.</span><span class="sxs-lookup"><span data-stu-id="b5e70-303">Set **Worker Type** to **Standard_DS3_v2**.</span></span>

7. <span data-ttu-id="b5e70-304">Legen Sie **Min Workers** (Min. Worker) auf **2** fest.</span><span class="sxs-lookup"><span data-stu-id="b5e70-304">Set **Min Workers** to **2**.</span></span>

8. <span data-ttu-id="b5e70-305">Deaktivieren Sie das Kontrollkästchen **Enable autoscaling** (Automatische Skalierung aktivieren).</span><span class="sxs-lookup"><span data-stu-id="b5e70-305">Deselect **Enable autoscaling**.</span></span> 

9. <span data-ttu-id="b5e70-306">Klicken Sie unterhalb des Dialogfelds **Auto Termination** (Automatische Beendigung) auf **Init Scripts** (Initialisierungsskripts).</span><span class="sxs-lookup"><span data-stu-id="b5e70-306">Below the **Auto Termination** dialog box, click on **Init Scripts**.</span></span> 

10. <span data-ttu-id="b5e70-307">Geben Sie **dbfs:/databricks/init/<Clustername>/spark-metrics.sh** ein, und ersetzen Sie dabei „<Clustername>“ durch den in Schritt 1 erstellten Clusternamen.</span><span class="sxs-lookup"><span data-stu-id="b5e70-307">Enter **dbfs:/databricks/init/<cluster-name>/spark-metrics.sh**, substituting the cluster name created in step 1 for <cluster-name>.</span></span>

11. <span data-ttu-id="b5e70-308">Klicken Sie auf die Schaltfläche **Hinzufügen** .</span><span class="sxs-lookup"><span data-stu-id="b5e70-308">Click the **Add** button.</span></span>

12. <span data-ttu-id="b5e70-309">Klicken Sie auf die Schaltfläche **Cluster erstellen**.</span><span class="sxs-lookup"><span data-stu-id="b5e70-309">Click the **Create Cluster** button.</span></span>

### <a name="create-a-databricks-job"></a><span data-ttu-id="b5e70-310">Erstellen eines Databricks-Auftrags</span><span class="sxs-lookup"><span data-stu-id="b5e70-310">Create a Databricks job</span></span>

1. <span data-ttu-id="b5e70-311">Klicken Sie im Databricks-Arbeitsbereich auf „Aufträge“ > „Auftrag erstellen“.</span><span class="sxs-lookup"><span data-stu-id="b5e70-311">In the Databricks workspace, click "Jobs", "create job".</span></span>

2. <span data-ttu-id="b5e70-312">Geben Sie einen Auftragsnamen ein.</span><span class="sxs-lookup"><span data-stu-id="b5e70-312">Enter a job name.</span></span>

3. <span data-ttu-id="b5e70-313">Klicken Sie auf „set jar“ (JAR-Datei festlegen). Daraufhin wird das Dialogfeld „Upload JAR to Run“ (Auszuführende JAR-Datei hochladen) angezeigt.</span><span class="sxs-lookup"><span data-stu-id="b5e70-313">Click "set jar", this opens the "Upload JAR to Run" dialog box.</span></span>

4. <span data-ttu-id="b5e70-314">Ziehen Sie die Datei **azure-databricks-job-1.0-SNAPSHOT.jar**, die Sie im Abschnitt **Erstellen der JAR-Dateien für den Databricks-Auftrag und die Databricks-Überwachung** erstellt haben, in das Feld **Drop JAR here to upload** (Hochzuladende JAR-Datei hier ablegen).</span><span class="sxs-lookup"><span data-stu-id="b5e70-314">Drag the **azure-databricks-job-1.0-SNAPSHOT.jar** file you created in the **build the .jar for the Databricks job** section to the **Drop JAR here to upload** box.</span></span>

5. <span data-ttu-id="b5e70-315">Geben Sie **com.microsoft.pnp.TaxiCabReader** in das Feld **Main Class** (Hauptklasse) ein.</span><span class="sxs-lookup"><span data-stu-id="b5e70-315">Enter **com.microsoft.pnp.TaxiCabReader** in the **Main Class** field.</span></span>

6. <span data-ttu-id="b5e70-316">Geben Sie in den Argumentfeldern Folgendes ein:</span><span class="sxs-lookup"><span data-stu-id="b5e70-316">In the arguments field, enter the following:</span></span>
    ```shell
    -n jar:file:/dbfs/azure-databricks-jobs/ZillowNeighborhoods-NY.zip!/ZillowNeighborhoods-NY.shp --taxi-ride-consumer-group taxi-ride-eh-cg --taxi-fare-consumer-group taxi-fare-eh-cg --window-interval "1 minute" --cassandra-host <Cosmos DB Cassandra host name from above> 
    ``` 

7. <span data-ttu-id="b5e70-317">Installieren Sie die abhängigen Bibliotheken:</span><span class="sxs-lookup"><span data-stu-id="b5e70-317">Install the dependent libraries by following these steps:</span></span>
    
    1. <span data-ttu-id="b5e70-318">Klicken Sie auf der Databricks-Benutzeroberfläche auf die Schaltfläche **Startseite**.</span><span class="sxs-lookup"><span data-stu-id="b5e70-318">In the Databricks user interface, click on the **home** button.</span></span>
    
    2. <span data-ttu-id="b5e70-319">Klicken Sie im Dropdownmenü **Benutzer** auf den Namen Ihres Benutzerkontos, um die Einstellungen für Ihren Kontoarbeitsbereich zu öffnen.</span><span class="sxs-lookup"><span data-stu-id="b5e70-319">In the **Users** drop-down, click on your user account name to open your account workspace settings.</span></span>
    
    3. <span data-ttu-id="b5e70-320">Klicken Sie neben Ihrem Kontonamen auf den Dropdownpfeil, klicken Sie auf **Erstellen**, und klicken Sie anschließend auf **Bibliothek**, um das Dialogfeld **Neue Bibliothek** zu öffnen.</span><span class="sxs-lookup"><span data-stu-id="b5e70-320">Click on the drop-down arrow beside your account name, click on **create**, and click on **Library** to open the **New Library** dialog.</span></span>
    
    4. <span data-ttu-id="b5e70-321">Wählen Sie im Dropdown-Steuerelement **Quelle** die Option **Maven Coordinate** (Maven-Koordinate) aus.</span><span class="sxs-lookup"><span data-stu-id="b5e70-321">In the **Source** drop-down control, select **Maven Coordinate**.</span></span>
    
    5. <span data-ttu-id="b5e70-322">Geben Sie unter der Überschrift **Install Maven Artifacts** (Maven-Artefakte installieren) die Zeichenfolge `com.microsoft.azure:azure-eventhubs-spark_2.11:2.3.5` in das Textfeld **Koordinate** ein.</span><span class="sxs-lookup"><span data-stu-id="b5e70-322">Under the **Install Maven Artifacts** heading, enter `com.microsoft.azure:azure-eventhubs-spark_2.11:2.3.5` in the **Coordinate** text box.</span></span> 
    
    6. <span data-ttu-id="b5e70-323">Klicken Sie auf **Bibliothek erstellen**, um das Fenster **Artefakte** zu öffnen.</span><span class="sxs-lookup"><span data-stu-id="b5e70-323">Click on **Create Library** to open the **Artifacts** window.</span></span>
    
    7. <span data-ttu-id="b5e70-324">Aktivieren Sie unter **Status on running clusters** (Status für ausgeführte Cluster) das Kontrollkästchen **Attach automatically to all clusters** (Automatisch an alle Cluster anfügen).</span><span class="sxs-lookup"><span data-stu-id="b5e70-324">Under **Status on running clusters** check the **Attach automatically to all clusters** checkbox.</span></span>
    
    8. <span data-ttu-id="b5e70-325">Wiederholen Sie die Schritte 1 bis 7 für die Maven-Koordinate `com.microsoft.azure.cosmosdb:azure-cosmos-cassandra-spark-helper:1.0.0`.</span><span class="sxs-lookup"><span data-stu-id="b5e70-325">Repeat steps 1 - 7 for the `com.microsoft.azure.cosmosdb:azure-cosmos-cassandra-spark-helper:1.0.0` Maven coordinate.</span></span>
    
    9. <span data-ttu-id="b5e70-326">Wiederholen Sie die Schritte 1 bis 6 für die Maven-Koordinate `org.geotools:gt-shapefile:19.2`.</span><span class="sxs-lookup"><span data-stu-id="b5e70-326">Repeat steps 1 - 6 for the `org.geotools:gt-shapefile:19.2` Maven coordinate.</span></span>
    
    10. <span data-ttu-id="b5e70-327">Klicken Sie auf **Erweiterte Optionen**.</span><span class="sxs-lookup"><span data-stu-id="b5e70-327">Click on **Advanced Options**.</span></span>
    
    11. <span data-ttu-id="b5e70-328">Geben Sie `http://download.osgeo.org/webdav/geotools/` in das Textfeld **Repository** ein.</span><span class="sxs-lookup"><span data-stu-id="b5e70-328">Enter `http://download.osgeo.org/webdav/geotools/` in the **Repository** text box.</span></span> 
    
    12. <span data-ttu-id="b5e70-329">Klicken Sie auf **Bibliothek erstellen**, um das Fenster **Artefakte** zu öffnen.</span><span class="sxs-lookup"><span data-stu-id="b5e70-329">Click **Create Library** to open the **Artifacts** window.</span></span> 
    
    13. <span data-ttu-id="b5e70-330">Aktivieren Sie unter **Status on running clusters** (Status für ausgeführte Cluster) das Kontrollkästchen **Attach automatically to all clusters** (Automatisch an alle Cluster anfügen).</span><span class="sxs-lookup"><span data-stu-id="b5e70-330">Under **Status on running clusters** check the **Attach automatically to all clusters** checkbox.</span></span>

8. <span data-ttu-id="b5e70-331">Fügen Sie die abhängigen Bibliotheken, die Sie in Schritt 7 hinzugefügt haben, dem Auftrag hinzu, den Sie in Schritt 6 erstellt haben:</span><span class="sxs-lookup"><span data-stu-id="b5e70-331">Add the dependent libraries added in step 7 to the job created at the end of step 6:</span></span>
    1. <span data-ttu-id="b5e70-332">Klicken Sie im Azure Databricks-Arbeitsbereich auf **Aufträge**.</span><span class="sxs-lookup"><span data-stu-id="b5e70-332">In the Azure Databricks workspace, click on **Jobs**.</span></span>

    2. <span data-ttu-id="b5e70-333">Klicken Sie auf den Auftragsnamen, den Sie in Schritt 2 des Abschnitts **Erstellen eines Databricks-Auftrags** erstellt haben.</span><span class="sxs-lookup"><span data-stu-id="b5e70-333">Click on the job name created in step 2 of the **create a Databricks job** section.</span></span> 
    
    3. <span data-ttu-id="b5e70-334">Klicken Sie neben dem Abschnitt **Dependent Libraries** (Abhängige Bibliotheken) auf **Hinzufügen**, um das Dialogfeld **Add Dependent Library** (Abhängige Bibliothek hinzufügen) zu öffnen.</span><span class="sxs-lookup"><span data-stu-id="b5e70-334">Beside the **Dependent Libraries** section, click on **Add** to open the **Add Dependent Library** dialog.</span></span> 
    
    4. <span data-ttu-id="b5e70-335">Wählen Sie unter **Library From** (Bibliothek aus) die Option **Arbeitsbereich** aus.</span><span class="sxs-lookup"><span data-stu-id="b5e70-335">Under **Library From** select **Workspace**.</span></span>
    
    5. <span data-ttu-id="b5e70-336">Klicken Sie auf **Benutzer**, auf Ihren Benutzernamen und anschließend auf `azure-eventhubs-spark_2.11:2.3.5`.</span><span class="sxs-lookup"><span data-stu-id="b5e70-336">Click on **users**, then your username, then click on `azure-eventhubs-spark_2.11:2.3.5`.</span></span> 
    
    6. <span data-ttu-id="b5e70-337">Klicken Sie auf **OK**.</span><span class="sxs-lookup"><span data-stu-id="b5e70-337">Click **OK**.</span></span>
    
    7. <span data-ttu-id="b5e70-338">Wiederholen Sie die Schritte 1 bis 6 für `spark-cassandra-connector_2.11:2.3.1` und `gt-shapefile:19.2`.</span><span class="sxs-lookup"><span data-stu-id="b5e70-338">Repeat steps 1 - 6 for `spark-cassandra-connector_2.11:2.3.1` and `gt-shapefile:19.2`.</span></span>

9. <span data-ttu-id="b5e70-339">Klicken Sie neben **Cluster:** auf **Bearbeiten**.</span><span class="sxs-lookup"><span data-stu-id="b5e70-339">Beside **Cluster:**, click on **Edit**.</span></span> <span data-ttu-id="b5e70-340">Daraufhin wird das Dialogfeld **Cluster konfigurieren** geöffnet.</span><span class="sxs-lookup"><span data-stu-id="b5e70-340">This opens the **Configure Cluster** dialog.</span></span> <span data-ttu-id="b5e70-341">Wählen Sie im Dropdownmenü **Clustertyp** die Option **Existing Cluster** (Vorhandener Cluster) aus.</span><span class="sxs-lookup"><span data-stu-id="b5e70-341">In the **Cluster Type** drop-down, select **Existing Cluster**.</span></span> <span data-ttu-id="b5e70-342">Wählen Sie im Dropdownmenü **Select Cluster** (Cluster auswählen) den Cluster aus, den Sie im Abschnitt **Erstellen eines Databricks-Clusters** erstellt haben.</span><span class="sxs-lookup"><span data-stu-id="b5e70-342">In the **Select Cluster** drop-down, select the cluster created the **create a Databricks cluster** section.</span></span> <span data-ttu-id="b5e70-343">Klicken Sie auf **Bestätigen**.</span><span class="sxs-lookup"><span data-stu-id="b5e70-343">Click **confirm**.</span></span>

10. <span data-ttu-id="b5e70-344">Klicken Sie auf **Jetzt ausführen**.</span><span class="sxs-lookup"><span data-stu-id="b5e70-344">Click **run now**.</span></span>

### <a name="run-the-data-generator"></a><span data-ttu-id="b5e70-345">Ausführen des Datengenerators</span><span class="sxs-lookup"><span data-stu-id="b5e70-345">Run the data generator</span></span>

1. <span data-ttu-id="b5e70-346">Navigieren Sie im GitHub-Repository zum Verzeichnis `onprem`.</span><span class="sxs-lookup"><span data-stu-id="b5e70-346">Navigate to the directory named `onprem` in the GitHub repository.</span></span>

2. <span data-ttu-id="b5e70-347">Aktualisieren Sie die Werte in der Datei **main.env** wie folgt:</span><span class="sxs-lookup"><span data-stu-id="b5e70-347">Update the values in the file **main.env** as follows:</span></span>

    ```shell
    RIDE_EVENT_HUB=[Connection string for the taxi-ride event hub]
    FARE_EVENT_HUB=[Connection string for the taxi-fare event hub]
    RIDE_DATA_FILE_PATH=/DataFile/FOIL2013
    MINUTES_TO_LEAD=0
    PUSH_RIDE_DATA_FIRST=false
    ```
    <span data-ttu-id="b5e70-348">Bei der Verbindungszeichenfolge für den Event Hub „taxi-ride“ handelt es sich um den **taxi-ride-eh**-Wert aus dem **eventHubs**-Ausgabeabschnitt aus Schritt 4 des Abschnitts *Bereitstellen der Azure-Ressourcen*.</span><span class="sxs-lookup"><span data-stu-id="b5e70-348">The connection string for the taxi-ride event hub is the **taxi-ride-eh** value from the **eventHubs** output section in step 4 of the *deploy the Azure resources* section.</span></span> <span data-ttu-id="b5e70-349">Bei der Verbindungszeichenfolge für den Event Hub „taxi-fare“ handelt es sich um den **taxi-fare-eh**-Wert aus dem **eventHubs**-Ausgabeabschnitt aus Schritt 4 des Abschnitts *Bereitstellen der Azure-Ressourcen*.</span><span class="sxs-lookup"><span data-stu-id="b5e70-349">The connection string for the taxi-fare event hub the **taxi-fare-eh** value from the **eventHubs** output section in step 4 of the *deploy the Azure resources* section.</span></span>

3. <span data-ttu-id="b5e70-350">Führen Sie den folgenden Befehl aus, um das Docker-Image zu erstellen.</span><span class="sxs-lookup"><span data-stu-id="b5e70-350">Run the following command to build the Docker image.</span></span>

    ```bash
    docker build --no-cache -t dataloader .
    ```

4. <span data-ttu-id="b5e70-351">Kehren Sie zum übergeordneten Verzeichnis zurück.</span><span class="sxs-lookup"><span data-stu-id="b5e70-351">Navigate back to the parent directory.</span></span>

    ```bash
    cd ..
    ```

5. <span data-ttu-id="b5e70-352">Führen Sie den folgenden Befehl aus, um das Docker-Image auszuführen.</span><span class="sxs-lookup"><span data-stu-id="b5e70-352">Run the following command to run the Docker image.</span></span>

    ```bash
    docker run -v `pwd`/DataFile:/DataFile --env-file=onprem/main.env dataloader:latest
    ```

<span data-ttu-id="b5e70-353">Die Ausgabe sollte wie folgt aussehen:</span><span class="sxs-lookup"><span data-stu-id="b5e70-353">The output should look like the following:</span></span>

```
Created 10000 records for TaxiFare
Created 10000 records for TaxiRide
Created 20000 records for TaxiFare
Created 20000 records for TaxiRide
Created 30000 records for TaxiFare
...
```

<span data-ttu-id="b5e70-354">Um zu überprüfen, ob der Databricks-Auftrag korrekt ausgeführt wird, öffnen Sie das Azure-Portal, und navigieren Sie zur Cosmos DB-Datenbank.</span><span class="sxs-lookup"><span data-stu-id="b5e70-354">To verify the Databricks job is running correctly, open the Azure portal and navigate to the Cosmos DB database.</span></span> <span data-ttu-id="b5e70-355">Öffnen Sie das Blatt **Daten-Explorer**, und untersuchen Sie die Daten in der Tabelle **taxi records**.</span><span class="sxs-lookup"><span data-stu-id="b5e70-355">Open the **Data Explorer** blade and examine the data in the **taxi records** table.</span></span> 

<span data-ttu-id="b5e70-356">[1] <span id="note1">Donovan, Brian; Work, Dan (2016): New York City Taxi Trip Data (2010 – 2013).</span><span class="sxs-lookup"><span data-stu-id="b5e70-356">[1] <span id="note1">Donovan, Brian; Work, Dan (2016): New York City Taxi Trip Data (2010-2013).</span></span> <span data-ttu-id="b5e70-357">Universität Illinois in Urbana-Champaign.</span><span class="sxs-lookup"><span data-stu-id="b5e70-357">University of Illinois at Urbana-Champaign.</span></span> <span data-ttu-id="b5e70-358">https://doi.org/10.13012/J8PN93H8</span><span class="sxs-lookup"><span data-stu-id="b5e70-358">https://doi.org/10.13012/J8PN93H8</span></span>
