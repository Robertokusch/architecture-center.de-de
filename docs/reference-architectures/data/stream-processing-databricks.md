---
title: Datenstromverarbeitung mit Azure Databricks
titleSuffix: Azure Reference Architectures
description: Erstellen einer End-to-End-Pipeline zur Datenstromverarbeitung in Azure mit Azure Databricks
author: petertaylor9999
ms.date: 11/30/2018
ms.custom: seodec18
ms.openlocfilehash: 822a3c448dcc2bdd4ae77ef2a2b7a9ffad633440
ms.sourcegitcommit: 88a68c7e9b6b772172b7faa4b9fd9c061a9f7e9d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/08/2018
ms.locfileid: "53120301"
---
# <a name="create-a-stream-processing-pipeline-with-azure-databricks"></a>Erstellen einer Pipeline zur Datenstromverarbeitung mit Azure Databricks

Diese Referenzarchitektur zeigt eine End-to-End-Pipeline zur [Datenstromverarbeitung](/azure/architecture/data-guide/big-data/real-time-processing). Dieser Pipelinetyp hat vier Phasen: Erfassung, Verarbeitung, Speicherung und Analyse/Berichterstellung. In dieser Referenzarchitektur erfasst die Pipeline Daten aus zwei Quellen, verknüpft verwandte Datensätze aus den einzelnen Datenströmen, reichert das Ergebnis an und berechnet einen Durchschnitt in Echtzeit. Die Ergebnisse werden zur weiteren Analyse gespeichert. [**Stellen Sie diese Lösung bereit**](#deploy-the-solution).

![Referenzarchitektur für Datenstromverarbeitung mit Azure Databricks](./images/stream-processing-databricks.png)

**Szenario:** Ein Taxiunternehmen erfasst Daten zu jeder Taxifahrt. In diesem Szenario gehen wir davon aus, dass zwei separate Geräte Daten senden. Das Taxi verfügt ein Messgerät, das Informationen zu jeder Fahrt sendet – die Dauer, die Strecke sowie die Abhol- und Zielorte. Ein separates Gerät akzeptiert Zahlungen von Kunden und sendet Daten zu den Fahrpreisen. Das Taxiunternehmen möchte Fahrgasttrends ermitteln und dazu für jedes Stadtviertel in Echtzeit das durchschnittliche Trinkgeld pro gefahrener Meile berechnen.

## <a name="architecture"></a>Architecture

Die Architektur umfasst die folgenden Komponenten.

**Datenquellen:** In dieser Architektur gibt es zwei Datenquellen, die Datenströme in Echtzeit generieren. Der erste Datenstrom enthält Informationen zur Fahrt und der zweite Informationen zum Fahrpreis. Die Referenzarchitektur umfasst einen simulierten Datengenerator, der aus einem Satz von statischen Dateien liest und die Daten an Event Hubs pusht. Bei einer echten Anwendung stammen die Daten von Geräten, die in den Taxis installiert sind.

**Azure Event Hubs**: [Event Hubs](/azure/event-hubs/) ist ein Ereigniserfassungsdienst. Die hier gezeigte Architektur verwendet zwei Event Hub-Instanzen, eine für jede Datenquelle. Jede Datenquelle sendet einen Datenstrom an den zugehörigen Event Hub.

**Azure Databricks**: [Databricks](/azure/azure-databricks/) ist eine Apache Spark-basierte Analyseplattform, die für die Microsoft Azure-Clouddienstplattform optimiert ist. Sie wird verwendet, um die Taxifahrtdaten und die Fahrpreisdaten zu korrelieren und die korrelierten Daten mit den gespeicherten Stadtvierteldaten aus dem Databricks-Dateisystem anzureichern.

**Cosmos DB**: Der Azure Databricks-Auftrag gibt eine Reihe von Datensätzen aus, die unter Verwendung der Cassandra-API in [Cosmos DB](/azure/cosmos-db/) geschrieben werden. Die Cassandra-API wird verwendet, da sie Modelle mit Zeitreihendaten unterstützt.

**Azure Log Analytics**: Die von [Azure Monitor](/azure/monitoring-and-diagnostics/) erfassten Anwendungsprotokolldaten werden in einem [Log Analytics-Arbeitsbereich](/azure/log-analytics) gespeichert. Mithilfe von Log Analytics-Abfragen können Metriken analysiert und visualisiert sowie Protokollmeldungen auf Probleme innerhalb der Anwendung untersucht werden.

## <a name="data-ingestion"></a>Datenerfassung

Um eine Datenquelle zu simulieren, verwendet die Referenzarchitektur das Dataset [New York City Taxi Data](https://uofi.app.box.com/v/NYCtaxidata/folder/2332218797)<sup>[[1]](#note1)</sup>. Dieses Dataset enthält Daten zu Taxifahrten in New York City für einen Zeitraum von vier Jahren (2010&ndash;2013). Es umfasst zwei Datensatztypen: Fahrtdaten und Fahrpreisdaten. Die Fahrtdaten enthalten die Fahrtdauer, die Fahrtstrecke sowie die Abhol- und Zielorte. Die Fahrpreisdaten enthalten die Beträge von Fahrpreis, Steuern und Trinkgeld. Gemeinsame Felder in beiden Datensatztypen sind die Taxinummer (Medallion), die Taxilizenz (Hack license) und die Anbieter-ID (Vendor ID). Anhand dieser drei Felder werden ein Taxi und ein Fahrer eindeutig identifiziert. Die Daten werden im CSV-Format gespeichert.

Der Datengenerator ist eine .NET Core-Anwendung, die Datensätze liest und an Azure Event Hubs sendet. Der Generator sendet Fahrtdaten im JSON-Format und Fahrpreisdaten im CSV-Format.

Event Hubs verwendet [Partitionen](/azure/event-hubs/event-hubs-features#partitions) zum Segmentieren der Daten. Partitionen ermöglichen es einem Consumer, die einzelnen Partitionen gleichzeitig zu lesen. Wenn Sie Daten an Event Hubs senden, können Sie den Partitionsschlüssel explizit angeben. Andernfalls werden Datensätze nach einem Roundrobinverfahren Partitionen zugewiesen.

In diesem Szenario sollen Fahrtdaten und Fahrpreisdaten für ein bestimmtes Taxi die gleiche Partitions-ID erhalten. Dies ermöglicht Databricks eine gewisse Parallelität beim Korrelieren der beiden Datenströme. Ein Datensatz in der Partition *n* der Fahrtdaten entspricht einem Datensatz in der Partition *n* der Fahrpreisdaten.

![Diagramm der Datenstromverarbeitung mit Azure Databricks und Event Hubs](./images/stream-processing-databricks-eh.png)

Im Datengenerator enthält das gemeinsame Datenmodell für beide Datensatztypen eine `PartitionKey`-Eigenschaft, bei der es sich um die Verkettung von `Medallion`, `HackLicense` und `VendorId` handelt.

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

Diese Eigenschaft wird verwendet, um beim Senden der Daten an Event Hubs einen expliziten Partitionsschlüssel bereitzustellen:

```csharp
using (var client = pool.GetObject())
{
    return client.Value.SendAsync(new EventData(Encoding.UTF8.GetBytes(
        t.GetData(dataFormat))), t.PartitionKey);
}
```

### <a name="event-hubs"></a>Event Hubs

Die Durchsatzkapazität von Event Hubs wird in [Durchsatzeinheiten](/azure/event-hubs/event-hubs-features#throughput-units) gemessen. Sie können einen Event Hub automatisch skalieren, indem Sie die Funktion für [automatische Vergrößerung](/azure/event-hubs/event-hubs-auto-inflate) aktivieren. Dadurch werden die Durchsatzeinheiten basierend auf dem Datenverkehr automatisch bis zu einem konfigurierten Höchstwert hochskaliert.

## <a name="stream-processing"></a>Datenstromverarbeitung

Die Datenverarbeitung in Azure Databricks erfolgt durch einen Auftrag. Der Auftrag wird einem Cluster zugewiesen und darin ausgeführt. Bei dem Auftrag kann es sich um benutzerdefinierten Java-Code oder um ein Spark-[Notebook](https://docs.databricks.com/user-guide/notebooks/index.html) handeln.

In dieser Referenzarchitektur ist der Auftrag ein Java-Archiv mit Klassen (geschrieben in Java und Scala). Bei Angabe des Java-Archivs für einen Databricks-Auftrag wird die Klasse für die Ausführung durch den Databricks-Cluster angegeben. Hier enthält die **main**-Methode der Klasse **com.microsoft.pnp.TaxiCabReader** die Datenverarbeitungslogik.

### <a name="reading-the-stream-from-the-two-event-hub-instances"></a>Lesen des Datenstroms aus den beiden Event Hub-Instanzen

Die Datenverarbeitungslogik verwendet [strukturiertes Spark-Streaming](https://spark.apache.org/docs/2.1.2/structured-streaming-programming-guide.html), um Daten aus den beiden Azure Event Hub-Instanzen zu lesen:

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

### <a name="enriching-the-data-with-the-neighborhood-information"></a>Anreichern der Daten mit Stadtviertelinformationen

Die Fahrtdaten enthalten die Koordinaten (Längen- und Breitengrad) des Start- und Zielorts. Diese Koordinaten sind zwar hilfreich, eignen sich aber nicht besonders für die Analyse. Daher werden die Daten mit Stadtvierteldaten aus einer [Shape-Datei](https://en.wikipedia.org/wiki/Shapefile) angereichert.

Das Shape-Dateiformat ist binär und lässt sich nicht so einfach analysieren. Die Bibliothek [GeoTools](http://geotools.org/) bietet jedoch Tools für Geodaten, die das Shape-Dateiformat verwenden. Diese Bibliothek wird in der Klasse **com.microsoft.pnp.GeoFinder** verwendet, um auf der Grundlage der Koordinaten des Start- und Zielorts den Namen des Stadtviertels zu bestimmen.

```scala
val neighborhoodFinder = (lon: Double, lat: Double) => {
      NeighborhoodFinder.getNeighborhood(lon, lat).get()
    }
```

### <a name="joining-the-ride-and-fare-data"></a>Verknüpfen von Fahrt- und Fahrpreisdaten

Zunächst werden die Fahrt- und Fahrpreisdaten transformiert:

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

Anschließend werden die Fahrtdaten mit den Fahrpreisdaten verknüpft:

```scala
val mergedTaxiTrip = rides.join(fares, Seq("medallion", "hackLicense", "vendorId", "pickupTime"))
```

### <a name="processing-the-data-and-inserting-into-cosmos-db"></a>Verarbeiten der Daten und Einfügen in Cosmos DB

Der durchschnittliche Fahrpreis für jedes Stadtviertel wird für ein bestimmtes Zeitintervall berechnet:

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

Das Ergebnis wird schließlich in Cosmos DB eingefügt:

```scala
maxAvgFarePerNeighborhood
      .writeStream
      .queryName("maxAvgFarePerNeighborhood_cassandra_insert")
      .outputMode(OutputMode.Append())
      .foreach(new CassandraSinkForeach(connector))
      .start()
      .awaitTermination()
```

## <a name="security-considerations"></a>Sicherheitshinweise

Der Zugriff auf den Arbeitsbereich der Azure-Datenbank wird über die [Administratorkonsole](https://docs.databricks.com/administration-guide/admin-settings/index.html) gesteuert. Über die Administratorkonsole können Benutzer hinzugefügt, Benutzerberechtigungen verwaltet und einmaliges Anmelden eingerichtet werden. Die Zugriffssteuerung für Arbeitsbereiche, Cluster, Aufträge und Tabellen kann ebenfalls über die Administratorkonsole festgelegt werden.

### <a name="managing-secrets"></a>Verwaltung geheimer Schlüssel

Azure Databricks enthält einen [Geheimnisspeicher](https://docs.azuredatabricks.net/user-guide/secrets/index.html) zum Speichern von Geheimnissen (beispielsweise Verbindungszeichenfolgen, Zugriffsschlüssel, Benutzernamen und Kennwörter). Geheimnisse werden im Azure Databricks-Geheimnisspeicher nach **Bereichen** partitioniert:

```bash
databricks secrets create-scope --scope "azure-databricks-job"
```

Geheimnisse werden auf der Bereichsebene hinzugefügt:

```bash
databricks secrets put --scope "azure-databricks-job" --key "taxi-ride"
```

> [!NOTE]
> Anstelle des nativen Azure Databricks-Bereichs kann auch ein Azure Key Vault-basierter Bereich verwendet werden. Weitere Informationen finden Sie unter [Azure Key Vault-backed scopes](https://docs.azuredatabricks.net/user-guide/secrets/secret-scopes.html#azure-key-vault-backed-scopes) (Azure Key Vault-basierte Bereiche).

Im Code erfolgt der Zugriff auf Geheimnisse über die [secrets-Hilfsprogramme](https://docs.databricks.com/user-guide/dev-tools/dbutils.html#secrets-utilities) von Azure Databricks.

## <a name="monitoring-considerations"></a>Aspekte der Überwachung

Azure Databricks basiert auf Apache Spark. Beide Lösungen verwenden [log4j](https://logging.apache.org/log4j/2.x/) als Standardbibliothek für die Protokollierung. Zusätzlich zur von Apache Spark bereitgestellten Standardprotokollierung sendet diese Referenzarchitektur auch Protokolle und Metriken an [Azure Log Analytics](/azure/log-analytics/).

Die Klasse **com.microsoft.pnp.TaxiCabReader** konfiguriert das Apache Spark-Protokollierungssystem so, dass dessen Protokolle an Azure Log Analytics gesendet werden. Hierbei werden die Werte aus der Datei **log4j.properties** verwendet. Die Meldungen der Apache Spark-Protokollierung liegen als Zeichenfolgen vor. Für Azure Log Analytics werden jedoch Protokollmeldungen im JSON-Format benötigt. Die Klasse **com.microsoft.pnp.log4j.LogAnalyticsAppender** transformiert die Meldungen in das JSON-Format:

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

Bei der Verarbeitung von Fahrt- und Fahrpreismeldungen durch die Klasse **com.microsoft.pnp.TaxiCabReader** kann es zu Problemen mit der Formatierung und somit zu ungültigen Meldungen kommen. In einer Produktionsumgebung müssen diese falsch formatierten Meldungen unbedingt analysiert werden, um ein Problem mit den Datenquellen zu erkennen und schnell zu beheben, damit keine Daten verloren gehen. Die Klasse **com.microsoft.pnp.TaxiCabReader** registriert einen Apache Spark-Akkumulator zur Nachverfolgung der Anzahl falsch formatierter Fahrpreis- und Fahrtdatensätze:

```scala
    @transient val appMetrics = new AppMetrics(spark.sparkContext)
    appMetrics.registerGauge("metrics.malformedrides", AppAccumulators.getRideInstance(spark.sparkContext))
    appMetrics.registerGauge("metrics.malformedfares", AppAccumulators.getFareInstance(spark.sparkContext))
    SparkEnv.get.metricsSystem.registerSource(appMetrics)
```

Apache Spark verwendet die Dropwizard-Bibliothek, um Metriken zu senden. Einige der nativen Dropwizard-Metrikfelder sind jedoch nicht mit Azure Log Analytics kompatibel. Daher enthält diese Referenzarchitektur eine benutzerdefinierte Dropwizard-Senke und einen entsprechenden Reporter. Die Metriken werden so formatiert, wie es von Azure Log Analytics erwartet wird. Wenn Apache Spark Metriken meldet, werden auch die benutzerdefinierten Metriken für die falsch formatierten Fahrt- und Fahrpreisdaten gesendet.

Als letzte Metrik wird im Azure Log Analytics-Arbeitsbereich der kumulative Fortschritt des strukturierten Spark-Streamingauftrags protokolliert. Hierzu wird in der Klasse **com.microsoft.pnp.StreamingMetricsListener** ein benutzerdefinierter StreamingQuery-Listener implementiert. Diese Klasse wird beim Ausführen des Auftrags bei der Apache Spark-Sitzung registriert:

```scala
spark.streams.addListener(new StreamingMetricsListener())
```

Die Methoden in „StreamingMetricsListener“ werden von der Apache Spark-Runtime aufgerufen, wenn ein strukturiertes Streamingereignis auftritt. Dabei werden Protokollmeldungen und Metriken an den Azure Log Analytics-Arbeitsbereich gesendet. Zur Überwachung der Anwendung können Sie in Ihrem Arbeitsbereich die folgenden Abfragen verwenden:

### <a name="latency-and-throughput-for-streaming-queries"></a>Wartezeit und Durchsatz für Streamingabfragen

```shell
taxijob_CL
| where TimeGenerated > startofday(datetime(<date>)) and TimeGenerated < endofday(datetime(<date>))
| project  mdc_inputRowsPerSecond_d, mdc_durationms_triggerExecution_d
| render timechart
```

### <a name="exceptions-logged-during-stream-query-execution"></a>Ausnahmen, die während der Ausführung von Datenstromabfragen protokolliert wurden

```shell
taxijob_CL
| where TimeGenerated > startofday(datetime(<date>)) and TimeGenerated < endofday(datetime(<date>))
| where Level contains "Error"
```

### <a name="accumulation-of-malformed-fare-and-ride-data"></a>Kumulation falsch formatierter Fahrpreis- und Fahrtdaten

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

### <a name="job-execution-to-trace-resiliency"></a>Auftragsausführung zur Nachverfolgung der Resilienz

```shell
SparkMetric_CL
| where TimeGenerated > startofday(datetime(<date>)) and TimeGenerated < endofday(datetime(<date>))
| render timechart
| where name_s contains "driver.DAGScheduler.job.allJobs"
```

## <a name="deploy-the-solution"></a>Bereitstellen der Lösung

Eine Bereitstellung für diese Referenzarchitektur ist auf [GitHub](https://github.com/mspnp/azure-databricks-streaming-analytics) verfügbar.

### <a name="prerequisites"></a>Voraussetzungen

1. Klonen oder forken Sie das GitHub-Repository [stream processing with Azure Databricks](https://github.com/mspnp/azure-databricks-streaming-analytics) (Datenstromverarbeitung mit Azure Databricks), oder laden Sie es herunter.

2. Installieren Sie [Docker](https://www.docker.com/), um den Datengenerator auszuführen.

3. Installieren Sie [Azure CLI 2.0](/cli/azure/install-azure-cli?view=azure-cli-latest).

4. Installieren Sie die [Databricks-Befehlszeilenschnittstelle](https://docs.databricks.com/user-guide/dev-tools/databricks-cli.html).

5. Melden Sie sich über eine Eingabeaufforderung, eine Bash-Eingabeaufforderung oder die PowerShell-Eingabeaufforderung folgendermaßen bei Ihrem Azure-Konto an:
    ```shell
    az login
    ```
6. Installieren Sie eine Java-IDE mit folgenden Ressourcen:
    - JDK 1.8
    - Scala SDK 2.11
    - Maven 3.5.4

### <a name="download-the-new-york-city-taxi-and-neighborhood-data-files"></a>Herunterladen der Datendateien für New Yorker Taxis und Stadtviertel

1. Erstellen Sie in Ihrem lokalen Dateisystem am Stamm des geklonten Github-Repositorys ein Verzeichnis namens `DataFile`.

2. Öffnen Sie einen Webbrowser, und navigieren Sie zu https://uofi.app.box.com/v/NYCtaxidata/folder/2332219935.

3. Klicken Sie auf dieser Seite auf die Schaltfläche zum **Herunterladen**, um eine ZIP-Datei mit den Taxidaten für dieses Jahr herunterzuladen.

4. Extrahieren Sie die ZIP-Datei in das Verzeichnis `DataFile`.

    > [!NOTE]
    > Diese ZIP-Datei enthält weitere ZIP-Dateien. Extrahieren Sie die untergeordneten ZIP-Dateien nicht.

    Die Verzeichnisstruktur muss wie folgt aussehen:

    ```shell
    /DataFile
        /FOIL2013
            trip_data_1.zip
            trip_data_2.zip
            trip_data_3.zip
            ...
    ```

5. Öffnen Sie einen Webbrowser, und navigieren Sie zu https://www.zillow.com/howto/api/neighborhood-boundaries.htm.

6. Klicken Sie auf **New York Neighborhood Boundaries** (Grenzen der Stadtviertel von New York), um die Datei herunterzuladen.

7. Kopieren Sie die Datei **ZillowNeighborhoods-NY.zip** aus dem Downloadverzeichnis**Ihres** Browsers in das `DataFile` Verzeichnis.

### <a name="deploy-the-azure-resources"></a>Bereitstellen der Azure-Ressourcen

1. Führen Sie an einer Shell- oder Windows-Eingabeaufforderung den folgenden Befehl aus, und folgen Sie den Anweisungen in der Anmeldeaufforderung:

    ```bash
    az login
    ```

2. Navigieren Sie im GitHub-Repository zum Ordner `azure`:

    ```bash
    cd azure
    ```

3. Führen Sie die folgenden Befehle aus, um die Azure-Ressourcen bereitzustellen:

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

4. Die Ausgabe der Bereitstellung wird nach Abschluss des Vorgangs in der Konsole angezeigt. Suchen Sie in der Ausgabe nach folgendem JSON-Code:

```json
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

Bei diesen Werten handelt es sich um die Geheimnisse, die in späteren Abschnitten zu Databricks-Geheimnissen hinzugefügt werden. Bewahren Sie sie bis dahin sicher auf.

### <a name="add-a-cassandra-table-to-the-cosmos-db-account"></a>Hinzufügen einer Cassandra-Tabelle zum Cosmos DB-Konto

1. Navigieren Sie im Azure-Portal zu der Ressourcengruppe, die weiter oben im Abschnitt **Bereitstellen der Azure-Ressourcen** erstellt wurde. Klicken Sie auf **Azure Cosmos DB-Konto**. Erstellen Sie eine Tabelle mit der Cassandra-API.

2. Klicken Sie auf dem Blatt **Übersicht** auf **Tabelle hinzufügen**.

3. Daraufhin wird das Blatt **Tabelle hinzufügen** geöffnet. Geben Sie dort `newyorktaxi` in das Textfeld **Keyspace name** (Keyspace-Name) ein.

4. Geben Sie im Abschnitt **enter CQL command to create the table** (CQL-Befehl eingeben, um Tabelle zu erstellen) die Zeichenfolge `neighborhoodstats` in das Textfeld neben `newyorktaxi` ein.

5. Geben Sie im Textfeld darunter Folgendes ein:
    ```shell
    (neighborhood text, window_end timestamp, number_of_rides bigint,total_fare_amount double, primary key(neighborhood, window_end))
    ```
6. Geben Sie im Textfeld **Durchsatz (1.000–1.000.000 RU/s)** den Wert `4000` ein.

7. Klicken Sie auf **OK**.

### <a name="add-the-databricks-secrets-using-the-databricks-cli"></a>Hinzufügen der Databricks-Geheimnisse mithilfe der Databricks-Befehlszeilenschnittstelle

Geben Sie zunächst die Geheimnisse für Event Hub ein:

1. Erstellen Sie mithilfe der **Azure Databricks-Befehlszeilenschnittstelle**, die Sie im zweiten Vorbereitungsschritt installiert haben, den Azure Databricks-Geheimnisbereich:
    ```shell
    databricks secrets create-scope --scope "azure-databricks-job"
    ```
2. Fügen Sie das Geheimnis für den Taxifahrt-Event Hub hinzu:
    ```shell
    databricks secrets put --scope "azure-databricks-job" --key "taxi-ride"
    ```
    Durch diesen Befehl wird der vi-Editor geöffnet. Geben Sie den **taxi-ride-eh**-Wert aus dem **eventHubs**-Ausgabeabschnitt aus Schritt 4 des Abschnitts *Bereitstellen der Azure-Ressourcen* ein. Speichern Sie Ihre Angaben, und beenden Sie vi.

3. Fügen Sie das Geheimnis für den Fahrpreis-Event Hub hinzu:
    ```shell
    databricks secrets put --scope "azure-databricks-job" --key "taxi-fare"
    ```
    Durch diesen Befehl wird der vi-Editor geöffnet. Geben Sie den **taxi-fare-eh**-Wert aus dem **eventHubs**-Ausgabeabschnitt aus Schritt 4 des Abschnitts *Bereitstellen der Azure-Ressourcen* ein. Speichern Sie Ihre Angaben, und beenden Sie vi.

Geben Sie als Nächstes die Geheimnisse für Cosmos DB ein:

1. Navigieren Sie im Azure-Portal zu der Ressourcengruppe, die Sie in Schritt 3 des Abschnitts **Bereitstellen der Azure-Ressourcen** angegeben haben. Klicken Sie auf das Azure Cosmos DB-Konto.

2. Fügen Sie mithilfe der **Azure Databricks-Befehlszeilenschnittstelle** das Geheimnis für den Cosmos DB-Benutzernamen hinzu:
    ```shell
    databricks secrets put --scope azure-databricks-job --key "cassandra-username"
    ```
Durch diesen Befehl wird der vi-Editor geöffnet. Geben Sie den **username**-Wert aus dem **CosmosDb**-Ausgabeabschnitt aus Schritt 4 des Abschnitts *Bereitstellen der Azure-Ressourcen* ein. Speichern Sie Ihre Angaben, und beenden Sie vi.

3. Fügen Sie als Nächstes das Geheimnis für das Cosmos DB-Kennwort hinzu:
    ```shell
    databricks secrets put --scope azure-databricks-job --key "cassandra-password"
    ```

Durch diesen Befehl wird der vi-Editor geöffnet. Geben Sie den **secret**-Wert aus dem **CosmosDb**-Ausgabeabschnitt aus Schritt 4 des Abschnitts *Bereitstellen der Azure-Ressourcen* ein. Speichern Sie Ihre Angaben, und beenden Sie vi.

> [!NOTE]
> Wenn Sie einen [Azure Key Vault-basierten Geheimnisbereich](https://docs.azuredatabricks.net/user-guide/secrets/secret-scopes.html#azure-key-vault-backed-scopes) verwenden, muss der Bereich **azure-databricks-job** heißen, und die Geheimnisse müssen exakt die oben angegebenen Namen haben.

### <a name="add-the-zillow-neighborhoods-data-file-to-the-databricks-file-system"></a>Hinzufügen der Datendatei „Zillow Neighborhoods“ zum Databricks-Dateisystem

1. Erstellen Sie ein Verzeichnis im Databricks-Dateisystem:
    ```bash
    dbfs mkdirs dbfs:/azure-databricks-jobs
    ```

2. Navigieren Sie zum Verzeichnis `DataFile`, und geben Sie Folgendes ein:
    ```bash
    dbfs cp ZillowNeighborhoods-NY.zip dbfs:/azure-databricks-jobs
    ```

### <a name="add-the-azure-log-analytics-workspace-id-and-primary-key-to-configuration-files"></a>Hinzufügen der Log Analytics-Arbeitsbereich-ID und des Primärschlüssels zu Konfigurationsdateien

In diesem Abschnitt benötigen Sie die Log Analytics-Arbeitsbereich-ID und den Primärschlüssel. Bei der Arbeitsbereich-ID handelt es sich um den **workspaceId**-Wert aus dem **logAnalytics**-Ausgabeabschnitt aus Schritt 4 des Abschnitts *Bereitstellen der Azure-Ressourcen*. Der Primärschlüssel ist der **secret**-Wert aus dem Ausgabeabschnitt.

1. Öffnen Sie `\azure\AzureDataBricksJob\src\main\resources\com\microsoft\pnp\azuredatabricksjob\log4j.properties`, um die log4j-Protokollierung zu konfigurieren. Bearbeiten Sie die beiden folgenden Werte:
    ```shell
    log4j.appender.A1.workspaceId=<Log Analytics workspace ID>
    log4j.appender.A1.secret=<Log Analytics primary key>
    ```

2. Öffnen Sie `\azure\azure-databricks-monitoring\scripts\metrics.properties`, um die benutzerdefinierte Protokollierung zu konfigurieren. Bearbeiten Sie die beiden folgenden Werte:
    ```shell
    *.sink.loganalytics.workspaceId=<Log Analytics workspace ID>
    *.sink.loganalytics.secret=<Log Analytics primary key>
    ```

### <a name="build-the-jar-files-for-the-databricks-job-and-databricks-monitoring"></a>Erstellen der JAR-Dateien für den Databricks-Auftrag und die Databricks-Überwachung

1. Importieren Sie mithilfe der Java-IDE die Maven-Projektdatei **pom.xml** aus dem Stammverzeichnis.

2. Erstellen Sie einen bereinigten Build. Dieser Build gibt die Dateien **azure-databricks-job-1.0-SNAPSHOT.jar** und **azure-databricks-monitoring-0.9.jar** aus.

### <a name="configure-custom-logging-for-the-databricks-job"></a>Konfigurieren der benutzerdefinierten Protokollierung für den Databricks-Auftrag

1. Kopieren Sie die Datei **azure-databricks-monitoring-0.9.jar** in das Databricks-Dateisystem, indem Sie den folgenden Befehl in die **Databricks-Befehlszeilenschnittstelle** eingeben:
    ```shell
    databricks fs cp --overwrite azure-databricks-monitoring-0.9.jar dbfs:/azure-databricks-job/azure-databricks-monitoring-0.9.jar
    ```

2. Kopieren Sie die Eigenschaften der benutzerdefinierten Protokollierung aus `\azure\azure-databricks-monitoring\scripts\metrics.properties` in das Databricks-Dateisystem. Verwenden Sie dazu den folgenden Befehl:
    ```shell
    databricks fs cp --overwrite metrics.properties dbfs:/azure-databricks-job/metrics.properties
    ```

3. Wählen Sie einen Namen für Ihren Databricks-Cluster. Der Name muss weiter unten im Databricks-Dateisystempfad für Ihren Cluster eingegeben werden. Kopieren Sie das Initialisierungsskript aus `\azure\azure-databricks-monitoring\scripts\spark.metrics` in das Databricks-Dateisystem. Verwenden Sie dazu den folgenden Befehl:
    ```shell
    databricks fs cp --overwrite spark-metrics.sh dbfs:/databricks/init/<cluster-name>/spark-metrics.sh
    ```

### <a name="create-a-databricks-cluster"></a>Erstellen eines Databricks-Clusters

1. Klicken Sie im Databricks-Arbeitsbereich auf „Cluster“ und anschließend auf „Cluster erstellen“. Geben Sie den Clusternamen ein, den Sie weiter oben in Schritt 3 des Abschnitts **Konfigurieren der benutzerdefinierten Protokollierung für den Databricks-Auftrag** erstellt haben.

2. Wählen Sie einen**Standardclustermodus** aus.

3. Legen Sie die **Databricks-Runtimeversion** auf **4.3** (beinhaltet Apache Spark 2.3.1 und Scala 2.11) fest.

4. Legen Sie die **Python-Version** auf **2** fest.

5. Legen Sie den **Treibertyp** auf **Same as worker** (Identisch mit Worker) fest.

6. Legen Sie den **Workertyp** auf **Standard_DS3_v2** fest.

7. Legen Sie **Min Workers** (Min. Worker) auf **2** fest.

8. Deaktivieren Sie das Kontrollkästchen **Enable autoscaling** (Automatische Skalierung aktivieren).

9. Klicken Sie unterhalb des Dialogfelds **Auto Termination** (Automatische Beendigung) auf **Init Scripts** (Initialisierungsskripts).

10. Geben Sie **dbfs:/databricks/init/<Clustername>/spark-metrics.sh** ein, und ersetzen Sie dabei „<Clustername>“ durch den in Schritt 1 erstellten Clusternamen.

11. Klicken Sie auf die Schaltfläche **Hinzufügen** .

12. Klicken Sie auf die Schaltfläche **Cluster erstellen**.

### <a name="create-a-databricks-job"></a>Erstellen eines Databricks-Auftrags

1. Klicken Sie im Databricks-Arbeitsbereich auf „Aufträge“ > „Auftrag erstellen“.

2. Geben Sie einen Auftragsnamen ein.

3. Klicken Sie auf „set jar“ (JAR-Datei festlegen). Daraufhin wird das Dialogfeld „Upload JAR to Run“ (Auszuführende JAR-Datei hochladen) angezeigt.

4. Ziehen Sie die Datei **azure-databricks-job-1.0-SNAPSHOT.jar**, die Sie im Abschnitt **Erstellen der JAR-Dateien für den Databricks-Auftrag und die Databricks-Überwachung** erstellt haben, in das Feld **Drop JAR here to upload** (Hochzuladende JAR-Datei hier ablegen).

5. Geben Sie **com.microsoft.pnp.TaxiCabReader** in das Feld **Main Class** (Hauptklasse) ein.

6. Geben Sie in den Argumentfeldern Folgendes ein:
    ```shell
    -n jar:file:/dbfs/azure-databricks-jobs/ZillowNeighborhoods-NY.zip!/ZillowNeighborhoods-NY.shp --taxi-ride-consumer-group taxi-ride-eh-cg --taxi-fare-consumer-group taxi-fare-eh-cg --window-interval "1 minute" --cassandra-host <Cosmos DB Cassandra host name from above>
    ```

7. Installieren Sie die abhängigen Bibliotheken:

    1. Klicken Sie auf der Databricks-Benutzeroberfläche auf die Schaltfläche **Startseite**.

    2. Klicken Sie im Dropdownmenü **Benutzer** auf den Namen Ihres Benutzerkontos, um die Einstellungen für Ihren Kontoarbeitsbereich zu öffnen.

    3. Klicken Sie neben Ihrem Kontonamen auf den Dropdownpfeil, klicken Sie auf **Erstellen**, und klicken Sie anschließend auf **Bibliothek**, um das Dialogfeld **Neue Bibliothek** zu öffnen.

    4. Wählen Sie im Dropdown-Steuerelement **Quelle** die Option **Maven Coordinate** (Maven-Koordinate) aus.

    5. Geben Sie unter der Überschrift **Install Maven Artifacts** (Maven-Artefakte installieren) die Zeichenfolge `com.microsoft.azure:azure-eventhubs-spark_2.11:2.3.5` in das Textfeld **Koordinate** ein.

    6. Klicken Sie auf **Bibliothek erstellen**, um das Fenster **Artefakte** zu öffnen.

    7. Aktivieren Sie unter **Status on running clusters** (Status für ausgeführte Cluster) das Kontrollkästchen **Attach automatically to all clusters** (Automatisch an alle Cluster anfügen).

    8. Wiederholen Sie die Schritte 1 bis 7 für die Maven-Koordinate `com.microsoft.azure.cosmosdb:azure-cosmos-cassandra-spark-helper:1.0.0`.

    9. Wiederholen Sie die Schritte 1 bis 6 für die Maven-Koordinate `org.geotools:gt-shapefile:19.2`.

    10. Klicken Sie auf **Erweiterte Optionen**.

    11. Geben Sie `http://download.osgeo.org/webdav/geotools/` in das Textfeld **Repository** ein.

    12. Klicken Sie auf **Bibliothek erstellen**, um das Fenster **Artefakte** zu öffnen. 

    13. Aktivieren Sie unter **Status on running clusters** (Status für ausgeführte Cluster) das Kontrollkästchen **Attach automatically to all clusters** (Automatisch an alle Cluster anfügen).

8. Fügen Sie die abhängigen Bibliotheken, die Sie in Schritt 7 hinzugefügt haben, dem Auftrag hinzu, den Sie in Schritt 6 erstellt haben:

    1. Klicken Sie im Azure Databricks-Arbeitsbereich auf **Aufträge**.

    2. Klicken Sie auf den Auftragsnamen, den Sie in Schritt 2 des Abschnitts **Erstellen eines Databricks-Auftrags** erstellt haben.

    3. Klicken Sie neben dem Abschnitt **Dependent Libraries** (Abhängige Bibliotheken) auf **Hinzufügen**, um das Dialogfeld **Add Dependent Library** (Abhängige Bibliothek hinzufügen) zu öffnen.

    4. Wählen Sie unter **Library From** (Bibliothek aus) die Option **Arbeitsbereich** aus.

    5. Klicken Sie auf **Benutzer**, auf Ihren Benutzernamen und anschließend auf `azure-eventhubs-spark_2.11:2.3.5`.

    6. Klicken Sie auf **OK**.

    7. Wiederholen Sie die Schritte 1 bis 6 für `spark-cassandra-connector_2.11:2.3.1` und `gt-shapefile:19.2`.

9. Klicken Sie neben **Cluster:** auf **Bearbeiten**. Daraufhin wird das Dialogfeld **Cluster konfigurieren** geöffnet. Wählen Sie im Dropdownmenü **Clustertyp** die Option **Existing Cluster** (Vorhandener Cluster) aus. Wählen Sie im Dropdownmenü **Select Cluster** (Cluster auswählen) den Cluster aus, den Sie im Abschnitt **Erstellen eines Databricks-Clusters** erstellt haben. Klicken Sie auf **Bestätigen**.

10. Klicken Sie auf **Jetzt ausführen**.

### <a name="run-the-data-generator"></a>Ausführen des Datengenerators

1. Navigieren Sie im GitHub-Repository zum Verzeichnis `onprem`.

2. Aktualisieren Sie die Werte in der Datei **main.env** wie folgt:

    ```shell
    RIDE_EVENT_HUB=[Connection string for the taxi-ride event hub]
    FARE_EVENT_HUB=[Connection string for the taxi-fare event hub]
    RIDE_DATA_FILE_PATH=/DataFile/FOIL2013
    MINUTES_TO_LEAD=0
    PUSH_RIDE_DATA_FIRST=false
    ```
    Bei der Verbindungszeichenfolge für den Event Hub „taxi-ride“ handelt es sich um den **taxi-ride-eh**-Wert aus dem **eventHubs**-Ausgabeabschnitt aus Schritt 4 des Abschnitts *Bereitstellen der Azure-Ressourcen*. Bei der Verbindungszeichenfolge für den Event Hub „taxi-fare“ handelt es sich um den **taxi-fare-eh**-Wert aus dem **eventHubs**-Ausgabeabschnitt aus Schritt 4 des Abschnitts *Bereitstellen der Azure-Ressourcen*.

3. Führen Sie den folgenden Befehl aus, um das Docker-Image zu erstellen.

    ```bash
    docker build --no-cache -t dataloader .
    ```

4. Kehren Sie zum übergeordneten Verzeichnis zurück.

    ```bash
    cd ..
    ```

5. Führen Sie den folgenden Befehl aus, um das Docker-Image auszuführen.

    ```bash
    docker run -v `pwd`/DataFile:/DataFile --env-file=onprem/main.env dataloader:latest
    ```

Die Ausgabe sollte wie folgt aussehen:

```
Created 10000 records for TaxiFare
Created 10000 records for TaxiRide
Created 20000 records for TaxiFare
Created 20000 records for TaxiRide
Created 30000 records for TaxiFare
...
```

Um zu überprüfen, ob der Databricks-Auftrag korrekt ausgeführt wird, öffnen Sie das Azure-Portal, und navigieren Sie zur Cosmos DB-Datenbank. Öffnen Sie das Blatt **Daten-Explorer**, und untersuchen Sie die Daten in der Tabelle **taxi records**. 

[1] <span id="note1">Donovan, Brian; Work, Dan (2016): New York City Taxi Trip Data (2010 – 2013). Universität Illinois in Urbana-Champaign. https://doi.org/10.13012/J8PN93H8
