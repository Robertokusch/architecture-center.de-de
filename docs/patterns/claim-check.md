---
title: Muster für die Anspruchsprüfung
titleSuffix: Cloud Design Patterns
description: Teilen Sie eine große Nachricht in eine Anspruchsprüfung und eine Nutzlast auf, um die Überlastung eines Nachrichtenbusses zu vermeiden.
keywords: Entwurfsmuster
author: yorek
ms.date: 03/05/2019
ms.topic: design-pattern
ms.service: architecture-center
ms.subservice: cloud-fundamentals
ms.openlocfilehash: 88457983df4ea3fd4ed8d0c447de6302422339f8
ms.sourcegitcommit: c053e6edb429299a0ad9b327888d596c48859d4a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/20/2019
ms.locfileid: "58248865"
---
# <a name="claim-check-pattern"></a>Muster für die Anspruchsprüfung

Teilen Sie eine große Nachricht in eine Anspruchsprüfung und eine Nutzlast auf. Senden Sie die Anspruchsprüfung an die Messagingplattform, und speichern Sie die Nutzlast in einem externen Dienst. Dieses Muster ermöglicht die Verarbeitung großer Nachrichten, während der Nachrichtenbus und der Client vor Überlastung oder Verlangsamung geschützt werden. Dieses Muster trägt auch zur Kostensenkung bei, da Speicher in der Regel günstiger ist als die von der Messagingplattform verwendeten Ressourceneinheiten.

Dieses Muster ist auch als verweisbasiertes Messaging bekannt und wurde erstmals im Buch *Enterprise Integration Patterns* von Gregor Hohpe und Roland Woolf [beschrieben][enterprise-integration-patterns].

## <a name="context-and-problem"></a>Kontext und Problem

Eine auf Messaging basierende Architektur muss zu gegebener Zeit in der Lage sein, große Nachrichten zu senden, zu empfangen und zu bearbeiten. Solche Nachrichten können alles Mögliche enthalten, wie z.B. Bilder (wie MRT-Aufnahmen), Audiodateien (wie Call-Center-Anrufe), Textdokumente oder jede Art binärer Daten beliebiger Größe.

Das direkte Senden solch großer Nachrichten an den Nachrichtenbus wird nicht empfohlen, da für deren Nutzung mehr Ressourcen und Bandbreite benötigt werden. Große Nachrichten können auch die gesamte Lösung verlangsamen, da Messagingplattformen in der Regel auf die Verarbeitung großer Mengen kleiner Nachrichten ausgelegt sind. Außerdem gelten bei den meisten Messagingplattformen Beschränkungen für die Nachrichtengröße, die Sie möglicherweise für große Nachrichten umgehen müssen.

## <a name="solution"></a>Lösung

Speichern Sie die gesamte Nachrichtennutzlast in einem externen Dienst, z.B. einer Datenbank. Rufen Sie den Verweis auf die gespeicherte Nutzlast ab, und senden Sie bloß diesen Verweis zum Nachrichtenbus. Der Verweis verhält sich wie eine Anspruchsprüfung bei Abholung eines Gepäckstücks, daher der Name des Musters. Clients, die an der Verarbeitung dieser spezifischen Nachricht interessiert sind, können den erhaltenen Verweis verwenden, um bei Bedarf die Nutzlast abzurufen.

![Diagramm des Musters für die Anspruchsprüfung](./_images/claim-check.jpg)

## <a name="issues-and-considerations"></a>Probleme und Überlegungen

Beachten Sie die folgenden Punkte bei der Entscheidung, wie dieses Muster implementiert werden soll:

- Erwägen Sie das Löschen der Nachrichtendaten nach der Verwendung, wenn Sie die Nachrichten nicht archivieren müssen. Obwohl Blobspeicher relativ kostengünstig ist, verursacht er auf lange Sicht Kosten, vor allem bei vielen Daten. Das Löschen der Nachricht kann synchron durch die Anwendung, die die Nachricht empfängt und verarbeitet, oder asynchron in einem gesonderten dedizierten Prozess erfolgen. Beim asynchronen Ansatz werden alte Daten entfernt, ohne dass sich dies auf den Durchsatz und die Verarbeitungsleistung der empfangenden Anwendung auswirkt.

- Das Speichern und Abrufen der Nachricht verursacht zusätzlichen Aufwand und Wartezeiten. Möglicherweise möchten Sie in der sendenden Anwendung Logik implementieren, um dieses Muster nur dann zu nutzen, wenn die Nachrichtengröße den Datengrenzwert des Nachrichtenbusses überschreitet. Bei kleineren Nachrichten wird das Muster nicht berücksichtigt. Dieser Ansatz führt zu einem bedingungsabhängigen Muster der Anspruchsprüfung.

## <a name="when-to-use-this-pattern"></a>Verwendung dieses Musters

Dieses Muster sollte verwendet werden, wenn eine Nachricht nicht an den unterstützten Nachrichtengrenzwert der gewählten Nachrichtenbustechnologie angepasst werden kann. Event Hubs weist beispielsweise derzeit (im Tarif „Basic“) einen Grenzwert von 256 KB auf, während Event Grid nur 64-KB-Nachrichten unterstützt.

Das Muster kann auch verwendet werden, wenn auf die Nutzlast nur von Diensten zugegriffen werden soll, die zur Ansicht berechtigt sind. Durch das Auslagern der Nutzlast an eine externe Ressource können strengere Authentifizierungs- und Autorisierungsregeln eingeführt werden, um sicherzustellen, dass Sicherheit gewährleistet ist, wenn sensible Daten in der Nutzlast gespeichert sind.

## <a name="examples"></a>Beispiele

In Azure kann dieses Muster auf verschiedene Weisen und mit unterschiedlichen Technologien implementiert werden, wobei es jedoch zwei Hauptkategorien gibt. In beiden Fällen liegt es in der Verantwortung des Empfängers, die Anspruchsprüfung zu lesen und damit die Nutzlast abzurufen.

- **Automatische Generierung der Anspruchsprüfung**. Bei diesem Ansatz wird mit [Azure Event Grid](/azure/event-grid/) automatisch die Anspruchsprüfung generiert und per Push an den Nachrichtenbus übertragen.

- **Manuelle Generierung der Anspruchsprüfung**. Bei diesem Ansatz ist der Absender für die Verwaltung der Nutzlast zuständig. Der Absender speichert die Nutzlast mithilfe des entsprechenden Diensts, erhält oder generiert die Anspruchsprüfung und sendet diese an den Nachrichtenbus.

Event Grid ist ein Dienst für das Ereignisrouting und versucht, Ereignisse innerhalb einer konfigurierbaren Zeitspanne von bis zu 24 Stunden zu übermitteln. Danach werden die Ereignisse entweder verworfen oder als unzustellbar markiert. Wenn Sie die Ereignisnutzlasten archivieren oder den Ereignisstrom wiedergeben müssen, können Sie ein Event Grid-Abonnement für die Dienste Event Hubs oder Queue Storage hinzufügen, in denen Nachrichten über einen längeren Zeitraum aufbewahrt werden können und die Archivierung von Nachrichten unterstützt wird. Informationen zur Optimierung der Zustellung und deren Wiederholung von Event Grid-Nachrichten und der Konfiguration für unzustellbare Nachrichten finden Sie unter [Richtlinien für unzustellbare Nachrichten und Wiederholung](/azure/event-grid/manage-event-delivery).

### <a name="automatic-claim-check-generation-with-blob-storage-and-event-grid"></a>Automatische Generierung der Anspruchsprüfung mit Blob Storage und Event Grid

Bei diesem Ansatz legt der Absender die Nachrichtennutzlast in einen dafür vorgesehenen Azure Blob Storage-Container ab. Event Grid generiert automatisch ein Tag bzw. einen Verweis und sendet ihn an einen unterstützten Nachrichtenbus, wie z.B. Azure Storage Queues. Der Empfänger kann die Warteschlange abfragen, die Nachricht abrufen und dann die gespeicherten Verweisdaten verwenden, um die Nutzlast direkt aus Blob Storage herunterzuladen.

Dieselbe Event Grid-Nachricht kann direkt von [Azure Functions](/azure/azure-functions/) genutzt werden, ohne dass ein Nachrichtenbus durchlaufen werden muss. Dieser Ansatz nutzt voll aus, das Azure Event Grid und Azure Functions serverlos sind.

Beispielcode für diesen Ansatz finden Sie [hier][example-1].

### <a name="event-grid-with-event-hubs"></a>Event Grid mit Event Hubs

Ähnlich wie im vorherigen Beispiel erzeugt Event Grid automatisch eine Nachricht, wenn eine Nutzlast in einen Azure Blob-Container geschrieben wird. Doch in diesem Beispiel wird der Nachrichtenbus mithilfe von Event Hubs implementiert. Ein Client kann sich so registrieren, dass er den Stream der Nachrichten empfängt, während sie in den Event Hub geschrieben werden. Der Event Hub kann auch so konfiguriert werden, dass Nachrichten archiviert und als Avro-Datei bereitgestellt werden, die mit Tools wie Apache Spark, Apache Drill oder einer der verfügbaren Avro-Bibliotheken abgefragt werden kann.

Beispielcode für diesen Ansatz finden Sie [hier][example-2].

### <a name="claim-check-generation-with-service-bus"></a>Generierung der Anspruchsprüfung mit Service Bus

Diese Lösung nutzt die Vorteile des speziellen Service Bus-Plug-Ins [ServiceBus.AttachmentPlugin](https://www.nuget.org/packages/ServiceBus.AttachmentPlugin/), das die Implementierung des Workflows zur Anspruchsprüfung vereinfacht. Das Plug-In wandelt jeden Nachrichtentext in eine Anlage um, die beim Senden der Nachricht in Azure Blob Storage gespeichert wird.

```csharp
using ServiceBus.AttachmentPlugin;
...

// Getting connection information
var serviceBusConnectionString = Environment.GetEnvironmentVariable("SERVICE_BUS_CONNECTION_STRING");
var queueName = Environment.GetEnvironmentVariable("QUEUE_NAME");
var storageConnectionString = Environment.GetEnvironmentVariable("STORAGE_CONNECTION_STRING");

// Creating config for sending message
var config = new AzureStorageAttachmentConfiguration(storageConnectionString);

// Creating and registering the sender using Service Bus Connection String and Queue Name
var sender = new MessageSender(serviceBusConnectionString, queueName);
sender.RegisterAzureStorageAttachmentPlugin(config);

// Create payload
var payload = new { data = "random data string for testing" };
var serialized = JsonConvert.SerializeObject(payload);
var payloadAsBytes = Encoding.UTF8.GetBytes(serialized);
var message = new Message(payloadAsBytes);

// Send the message
await sender.SendAsync(message);
```

Die Service Bus-Nachricht fungiert als Benachrichtigungswarteschlange, die ein Client abonnieren kann. Wenn der Consumer die Nachricht empfängt, ermöglicht das Plug-In das direkte Lesen der Nachrichtendaten aus Blob Storage. Dann können Sie wählen, wie die Nachricht weiter verarbeitet werden soll. Ein Vorteil dieses Ansatzes ist, dass er den Workflow der Anspruchsprüfung vom Absender und Empfänger abstrahiert.

Beispielcode für diesen Ansatz finden Sie [hier][example-3].

### <a name="manual-claim-check-generation-with-kafka"></a>Manuelle Generierung der Anspruchsprüfung mit Kafka

In diesem Beispiel schreibt ein Kafka-Client die Nutzlast in Azure Blob Storage. Anschließend sendet er eine Benachrichtigung mithilfe [Kafka-fähiger Event Hubs](/azure/event-hubs/event-hubs-quickstart-kafka-enabled-event-hubs). Der Consumer empfängt die Nachricht und kann in Blob Storage auf die Nutzlast zugreifen. Dieses Beispiel zeigt, wie ein anderes Messagingprotokoll verwendet werden kann, um das Muster der Anspruchsprüfung in Azure zu implementieren. Angenommen, Sie müssen bestehende Kafka-Clients unterstützen.

Beispielcode für diesen Ansatz finden Sie [hier][example-4].

## <a name="related-patterns-and-guidance"></a>Zugehörige Muster und Anleitungen

- Die oben beschriebenen Beispiele stehen auf [GitHub][sample-code] zur Verfügung.
- Die Website von Enterprise Integration Patterns bietet eine [Beschreibung][enterprise-integration-patterns] dieses Musters.
- Ein weiteres Beispiel finden Sie im Blogbeitrag [Dealing with large Service Bus messages using claim check pattern](https://www.serverless360.com/blog/deal-with-large-service-bus-messages-using-claim-check-pattern) (Verarbeiten großer Service Bus-Nachrichten mithilfe des Musters für die Anspruchsprüfung).
- Ein alternatives Muster zur Verarbeitung großer Nachrichten ist [Aufteilen][splitter] und [Aggregieren][aggregator].

<!-- links -->
[aggregator]: https://www.enterpriseintegrationpatterns.com/patterns/messaging/Aggregator.html
[enterprise-integration-patterns]: https://www.enterpriseintegrationpatterns.com/patterns/messaging/StoreInLibrary.html
[example-1]: https://github.com/mspnp/cloud-design-patterns/tree/master/claim-check/code-samples/sample-1
[example-2]: https://github.com/mspnp/cloud-design-patterns/tree/master/claim-check/code-samples/sample-2
[example-3]: https://github.com/mspnp/cloud-design-patterns/tree/master/claim-check/code-samples/sample-3
[example-4]: https://github.com/mspnp/cloud-design-patterns/tree/master/claim-check/code-samples/sample-4
[sample-code]: https://github.com/mspnp/cloud-design-patterns/tree/master/claim-check
[splitter]: https://www.enterpriseintegrationpatterns.com/patterns/messaging/Sequencer.html
