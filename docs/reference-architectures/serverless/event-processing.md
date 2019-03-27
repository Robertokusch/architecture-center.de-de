---
title: Serverlose Ereignisverarbeitung mit Azure Functions
titleSuffix: Azure Reference Architectures
description: Referenzarchitektur für die serverlose Ereigniserfassung und -verarbeitung mithilfe von Azure Functions.
author: MikeWasson
ms.date: 10/16/2018
ms.topic: reference-architecture
ms.service: architecture-center
ms.subservice: reference-architecture
ms.custom: seodec18, serverless
ms.openlocfilehash: 9d2535c3e350000783265dc58c83d00a38d45448
ms.sourcegitcommit: c053e6edb429299a0ad9b327888d596c48859d4a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/20/2019
ms.locfileid: "58244311"
---
# <a name="serverless-event-processing-using-azure-functions"></a>Serverlose Ereignisverarbeitung mit Azure Functions

Diese Referenzarchitektur zeigt eine [serverlose](https://azure.microsoft.com/solutions/serverless/), ereignisgesteuerte Architektur, die einen Datenstrom erfasst, die Daten verarbeitet und die Ergebnisse in eine Back-End-Datenbank schreibt. Eine Referenzimplementierung für diese Architektur ist auf [GitHub][github] verfügbar.

![Referenzarchitektur für die serverlose Ereignisverarbeitung mithilfe von Azure Functions](./_images/serverless-event-processing.png)

## <a name="architecture"></a>Architecture

Der Datenstrom wird mit **Event Hubs** erfasst. [Event Hubs][eh] ist für Datenstromszenarien mit hohem Durchsatz konzipiert.

> [!NOTE]
> Für IoT-Szenarien empfehlen wir die Verwendung von IoT Hub. IoT Hub verfügt über einen integrierten Endpunkt, der mit der Azure Event Hubs-API kompatibel ist, sodass Sie beide Dienste in dieser Architektur ohne größere Änderungen bei der Back-End-Verarbeitung nutzen können. Weitere Informationen finden Sie unter [Verbinden von IoT-Geräten mit Azure: IoT Hub und Event Hubs][iot].

**Funktions-App**. [Azure Functions][functions] ist eine serverlose Computeoption. Es wird ein ereignisgesteuertes Modell verwendet, bei dem ein Codeabschnitt (eine „Funktion“) durch einen Trigger aufgerufen wird. Wenn in dieser Architektur Ergebnisse auf einer Event Hubs-Instanz eingehen, lösen sie eine Funktion aus, mit der die Ereignisse verarbeitet und die Ergebnisse in den Speicher geschrieben werden.

Funktions-Apps sind für die Verarbeitung einzelner Datensätze von Event Hubs geeignet. Für komplexere Szenarien zur Verarbeitung von Datenströmen können Sie erwägen, Apache Spark mit Azure Databricks oder Azure Stream Analytics zu verwenden.

**Cosmos DB**: [Cosmos DB][cosmosdb] ist ein Datenbankdienst mit mehreren Modellen. Für dieses Szenario speichert die Funktion für die Ereignisverarbeitung JSON-Datensätze mit der Cosmos DB-[SQL-API][cosmosdb-sql].

**Warteschlangenspeicher**. [Warteschlangenspeicher][queue] wird für unzustellbare Nachrichten verwendet. Wenn bei der Verarbeitung eines Ereignisses ein Fehler auftritt, speichert die Funktion die Ereignisdaten in einer Warteschlange für unzustellbare Nachrichten zur späteren Verarbeitung. Weitere Informationen finden Sie unter [Überlegungen zur Resilienz](#resiliency-considerations).

**Azure Monitor**: [Monitor][monitor] erfasst Leistungsmetriken zu den in der Lösung bereitgestellten Azure-Diensten. Durch die Visualisierung dieser Metriken in einem Dashboard können Sie einen Einblick in die Integrität der Lösung gewinnen.

**Azure Pipelines**. [Pipelines][pipelines] ist ein CI-/CD-Dienst (Continuous Integration/Continuous Delivery), mit dem die Anwendung erstellt, getestet und bereitgestellt wird.

## <a name="scalability-considerations"></a>Überlegungen zur Skalierbarkeit

### <a name="event-hubs"></a>Event Hubs

Die Durchsatzkapazität von Event Hubs wird in [Durchsatzeinheiten][eh-throughput] gemessen. Sie können einen Event Hub automatisch skalieren, indem Sie die Funktion für [automatische Vergrößerung][eh-autoscale] aktivieren. Dadurch werden die Durchsatzeinheiten basierend auf dem Datenverkehr automatisch bis zu einem konfigurierten Höchstwert hochskaliert.

Der [Event Hub-Trigger][eh-trigger] in der Funktions-App wird gemäß der Anzahl von Partitionen im Event Hub skaliert. Jeder Partition wird jeweils eine Funktionsinstanz zugewiesen. Zur Erhöhung des Durchsatzes empfangen Sie die Ereignisse nicht einzeln nacheinander, sondern als Batch.

### <a name="cosmos-db"></a>Cosmos DB

Die Durchsatzkapazität für Cosmos DB wird in [Anforderungseinheiten][ru] (RUs) gemessen. Um einen Cosmos DB-Container auf eine Kapazität von mehr als 10.000 RU zu skalieren, müssen Sie beim Erstellen des Containers einen [Partitionsschlüssel][partition-key] angeben und diesen in jedes von Ihnen erstellte Dokument einfügen.

Hier sind einige Merkmale eines gut geeigneten Partitionsschlüssels aufgeführt:

- Der Schlüsselwertbereich ist ausreichend groß.
- Es erfolgt eine gleichmäßige Verteilung von Lese- und Schreibvorgängen pro Schlüsselwert, um eine Überlastung von Schlüsseln zu vermeiden.
- Die maximale Datenmenge, die für einen Schlüsselwert gespeichert wird, übersteigt niemals die maximale Größe der physischen Partition (10 GB).
- Der Partitionsschlüssel für ein Dokument ändert sich nicht. Sie können den Partitionsschlüssel für ein vorhandenes Dokument nicht aktualisieren.

Im Szenario für diese Referenzarchitektur speichert die Funktion genau ein Dokument pro Gerät, das Daten sendet. Die Funktion aktualisiert die Dokumente laufend mit dem aktuellen Gerätestatus, indem ein Upsert-Vorgang verwendet wird. Die Geräte-ID ist ein guter Partitionsschlüssel für dieses Szenario, da Schreibvorgänge gleichmäßig auf die Schlüssel verteilt werden. Außerdem ist die Größe jeder Partition streng begrenzt, weil ein einzelnes Dokument für jeden Schlüsselwert vorhanden ist. Weitere Informationen zu Partitionsschlüsseln finden Sie unter [Partitionieren und Skalieren in Azure Cosmos DB][cosmosdb-scale].

## <a name="resiliency-considerations"></a>Überlegungen zur Resilienz

Beim Verwenden des Event Hubs-Triggers mit Functions ist es ratsam, Ausnahmen in Ihrer Verarbeitungsschleife abzufangen. Wenn ein Ausnahmefehler auftritt, führt die Functions-Runtime keinen Wiederholungsversuch für die Nachrichten durch. Wenn eine Nachricht nicht verarbeitet werden kann, sollte sie in eine Warteschlange für unzustellbare Nachrichten eingereiht werden. Verwenden Sie einen Out-of-band-Prozess, um die Nachrichten zu untersuchen und die Korrekturmaßnahmen zu bestimmen.

Mit dem folgenden Code wird veranschaulicht, wie die Erfassungsfunktion Ausnahmen abfängt und nicht verarbeitete Nachrichten in eine Warteschlange für unzustellbare Nachrichten einreiht.

```csharp
[FunctionName("RawTelemetryFunction")]
[StorageAccount("DeadLetterStorage")]
public static async Task RunAsync(
    [EventHubTrigger("%EventHubName%", Connection = "EventHubConnection", ConsumerGroup ="%EventHubConsumerGroup%")]EventData[] messages,
    [Queue("deadletterqueue")] IAsyncCollector<DeadLetterMessage> deadLetterMessages,
    ILogger logger)
{
    foreach (var message in messages)
    {
        DeviceState deviceState = null;

        try
        {
            deviceState = telemetryProcessor.Deserialize(message.Body.Array, logger);
        }
        catch (Exception ex)
        {
            logger.LogError(ex, "Error deserializing message", message.SystemProperties.PartitionKey, message.SystemProperties.SequenceNumber);
            await deadLetterMessages.AddAsync(new DeadLetterMessage { Issue = ex.Message, EventData = message });
        }

        try
        {
            await stateChangeProcessor.UpdateState(deviceState, logger);
        }
        catch (Exception ex)
        {
            logger.LogError(ex, "Error updating status document", deviceState);
            await deadLetterMessages.AddAsync(new DeadLetterMessage { Issue = ex.Message, EventData = message, DeviceState = deviceState });
        }
    }
}
```

Beachten Sie, dass für die Funktion die [Queue Storage-Ausgabebindung][queue-binding] genutzt wird, um Elemente in die Warteschlange einzureihen.

Mit dem obigen Code werden Ausnahmen auch in Application Insights protokolliert. Sie können den Partitionsschlüssel und die Sequenznummer auch verwenden, um unzustellbare Nachrichten mit den Ausnahmen in den Protokollen zu korrelieren.

Nachrichten in der Warteschlange für unzustellbare Nachrichten sollten über genügend Informationen verfügen, damit Sie den Kontext des Fehlers verstehen können. In diesem Beispiel enthält die Klasse `DeadLetterMessage` die Ausnahmenachricht, die ursprünglichen Ereignisdaten und die deserialisierte Ereignisnachricht (falls verfügbar).

```csharp
public class DeadLetterMessage
{
    public string Issue { get; set; }
    public EventData EventData { get; set; }
    public DeviceState DeviceState { get; set; }
}
```

Nutzen Sie [Azure Monitor][monitor], um den Event Hub zu überwachen. Wenn eine Eingabe vorhanden ist, aber keine Ausgabe, bedeutet dies, dass Nachrichten nicht verarbeitet werden. Navigieren Sie in diesem Fall zu [Log Analytics][log-analytics], und suchen Sie nach Ausnahmen oder anderen Fehlern.

## <a name="disaster-recovery-considerations"></a>Überlegungen zur Notfallwiederherstellung

Die hier dargestellte Bereitstellung befindet sich in nur einer Azure-Region. Nutzen Sie die Features für die geografische Verteilung in den verschiedenen Diensten, um einen stabileren Ansatz für die Notfallwiederherstellung zu erzielen:

- **Event Hubs**. Erstellen Sie zwei Event Hubs-Namespaces: einen primären (aktiven) Namespace und einen sekundären (passiven) Namespace. Nachrichten werden automatisch an den aktiven Namespace weitergeleitet, sofern Sie kein Failover zum sekundären Namespace ausführen. Weitere Informationen finden Sie unter [Georedundante Notfallwiederherstellung in Azure Event Hubs][eh-dr].

- **Funktions-App**. Stellen Sie eine zweite Funktions-App bereit, die darauf wartet, vom sekundären Event Hubs-Namespace zu lesen. Mit dieser Funktion werden Daten in ein sekundäres Speicherkonto für die Warteschlange für unzustellbare Nachrichten geschrieben.

- **Cosmos DB**: Cosmos DB unterstützt [mehrere Masterregionen][cosmosdb-geo], sodass Schreibvorgänge in alle Regionen möglich sind, die Sie Ihrem Cosmos DB-Konto hinzufügen. Wenn Sie die Funktion für mehrere Master (Multimaster) nicht aktivieren, können Sie trotzdem ein Failover in die primäre Schreibregion ausführen. Die Cosmos DB-Client-SDKs und die Azure-Funktionsbindungen verarbeiten das Failover automatisch, sodass Sie keine Einstellungen für die Anwendungskonfiguration aktualisieren müssen.

- **Azure Storage**. Verwenden Sie für die Warteschlange für unzustellbare Nachrichten Speicher vom Typ [RA-GRS][ra-grs] (georedundanter Speicher mit Lesezugriff). Hierdurch wird ein schreibgeschütztes Replikat in einer anderen Region erstellt. Wenn die primäre Region ausfällt, können Sie die derzeit in der Warteschlange befindlichen Elemente lesen. Stellen Sie darüber hinaus ein weiteres Speicherkonto in der sekundären Region bereit, in das die Funktion nach einem Failover schreiben kann.

## <a name="deploy-the-solution"></a>Bereitstellen der Lösung

Zeigen Sie zum Bereitstellen dieser Referenzarchitektur die [GitHub-Infodatei][readme] an.

<!-- links -->

[cosmosdb]: /azure/cosmos-db/introduction
[cosmosdb-geo]: /azure/cosmos-db/distribute-data-globally
[cosmosdb-scale]: /azure/cosmos-db/partition-data
[cosmosdb-sql]: /azure/cosmos-db/sql-api-introduction
[eh]: /azure/event-hubs/
[eh-autoscale]: /azure/event-hubs/event-hubs-auto-inflate
[eh-dr]: /azure/event-hubs/event-hubs-geo-dr
[eh-throughput]: /azure/event-hubs/event-hubs-features#throughput-units
[eh-trigger]: /azure/azure-functions/functions-bindings-event-hubs
[functions]: /azure/azure-functions/functions-overview
[iot]: /azure/iot-hub/iot-hub-compare-event-hubs
[log-analytics]: /azure/log-analytics/log-analytics-queries
[monitor]: /azure/azure-monitor/overview
[partition-key]: /azure/cosmos-db/partition-data
[pipelines]: /azure/devops/pipelines/index
[queue]: /azure/storage/queues/storage-queues-introduction
[queue-binding]: /azure/azure-functions/functions-bindings-storage-queue#output
[ra-grs]: /azure/storage/common/storage-redundancy-grs
[ru]: /azure/cosmos-db/request-units

[github]: https://github.com/mspnp/serverless-reference-implementation
[readme]: https://github.com/mspnp/serverless-reference-implementation/blob/master/README.md
