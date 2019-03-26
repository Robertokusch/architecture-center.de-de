---
title: Azure IoT-Referenzarchitektur
description: Empfohlene Architektur für IoT-Anwendungen in Azure mit PaaS-Komponenten (Platform-as-a-Service)
titleSuffix: Azure Reference Architectures
author: MikeWasson
ms.date: 01/09/2019
---

# <a name="azure-iot-reference-architecture"></a>Azure IoT-Referenzarchitektur

Diese Referenzarchitektur zeigt eine empfohlene Architektur für IoT-Anwendungen in Azure mit PaaS-Komponenten (Platform-as-a-Service).

![Diagramm der Architektur](./_images/iot.png)

IoT-Anwendungen (Internet of Things = Internet der Dinge) können als **Dinge** (Geräte) beschrieben werden, die Daten senden, aus denen **Erkenntnisse** generiert werden. Diese Erkenntnisse generieren **Aktionen** zum Verbessern eines Unternehmens oder Prozesses. Ein Beispiel ist ein Modul (das „Ding“), das Temperaturdaten sendet. Diese Daten werden ausgewertet, um zu ermitteln, ob das Modul wie erwartet funktioniert (die „Erkenntnis“). Die Erkenntnis wird verwendet, um den Wartungszeitplan für das Modul (die „Aktion“) proaktiv zu priorisieren.

Diese Referenzarchitektur verwendet Azure-PaaS-Komponenten (Platform-as-a-Service). Folgende weitere Optionen sind für die Erstellung von IoT-Lösungen in Azure verfügbar:

- [Azure IoT Central](/azure/iot-central/). IoT Central ist eine vollständig verwaltete SaaS-Lösung (Software-as-a-Service). Sie abstrahiert die technischen Auswahlmöglichkeiten und erlaubt es Ihnen, sich ausschließlich auf Ihre Lösung zu konzentrieren. Diese Einfachheit bringt jedoch den Nachteil mit sich, dass die Lösung weniger anpassbar ist als eine PaaS-basierte Lösung.
- Verwendung von OSS-Komponenten (Open Source Software) wie dem SMACK-Stapel (Spark, Mesos, Akka, Cassandra, Kafka), die auf Azure-VMs bereitgestellt werden. Dieser Ansatz bietet umfangreiche Steuerungsmöglichkeiten, ist jedoch komplexer.

Allgemein betrachtet gibt es zwei Möglichkeiten zum Verarbeiten von Telemetriedaten: warmer Pfad (Hot Path) und kalter Pfad (Cold Path). Der Unterschied besteht in den Anforderungen hinsichtlich der Wartezeit und des Datenzugriffs.

- Der **warme Pfad** analysiert Daten nahezu in Echtzeit, sobald sie eintreffen. Im warmen Pfad müssen Telemetriedaten mit sehr geringer Wartezeit verarbeitet werden. Der warme Pfad wird in der Regel mithilfe eines Moduls zur Streamverarbeitung implementiert. Die Ausgabe kann eine Warnung auslösen oder in ein strukturiertes Format geschrieben werden, das mit Analysetools abgefragt werden kann.
- Der **kalte Pfad** führt eine Batchverarbeitung in längeren Intervallen (stündlich oder täglich) aus. In der Regel verarbeitet der kalte Pfad große Datenmengen, die Ergebnisse müssen jedoch nicht so zeitnah sein wie beim warmen Pfad. Im kalten Pfad werden Telemetrierohdaten erfasst und dann in einen Batchprozess gespeist.

## <a name="architecture"></a>Architecture

Diese Architektur umfasst die folgenden Komponenten. Einige Anwendungen erfordern möglicherweise nicht alle der hier aufgeführten Komponenten.

**IoT-Geräte**. Geräte können sicher in der Cloud registriert werden und zum Senden und Empfangen von Daten eine Verbindung mit der Cloud herstellen. Einige Geräte sind u. U. **Edgegeräte**, die Daten auf dem Gerät selbst oder in einem Bereichsgateway verarbeiten. Wir empfehlen für die Edgeverarbeitung [Azure IoT Edge](/azure/iot-edge/).

**Cloudgateway**. Ein Cloudgateway stellt einen Cloudhub bereit, über den Geräte eine sichere Verbindung mit der Cloud herstellen und Daten senden können. Es bietet zudem Geräteverwaltungsfunktionen, einschließlich der Steuerung von Geräten mit Befehlen. Für das Cloudgateway empfehlen wir [IoT Hub](/azure/iot-hub/). IoT Hub ist ein gehosteter Clouddienst, der Ereignisse von Geräten erfasst und als Nachrichtenbroker zwischen Geräten und Back-End-Diensten fungiert. IoT Hub bietet sichere Konnektivität, Ereigniserfassung, bidirektionale Kommunikation und Geräteverwaltung.

**Gerätebereitstellung**. Zum Registrieren und Verbinden einer großen Anzahl von Geräten empfehlen wir [IoT Hub Device Provisioning Service](/azure/iot-dps/) (DPS). Mit DPS können Sie Geräte bedarfsorientiert bestimmten Azure IoT Hub-Endpunkten zuweisen und registrieren.

**Streamverarbeitung:** Die Streamverarbeitung analysiert große Streams von Datensätzen und wertet die Regeln für diese Streams aus. Für die Streamverarbeitung empfehlen wir [Azure Stream Analytics](/azure/stream-analytics/). Stream Analytics kann komplexe Analysen in großem Umfang mit Zeitfensterfunktionen, Streamaggregationen und externen Datenquellenverknüpfungen ausführen. Eine weitere Option ist Apache Spark in [Azure Databricks](/azure/azure-databricks/).

Machine Learning ermöglicht die Ausführung von Vorhersagealgorithmen für historische Telemetriedaten und somit Szenarien wie Predictive Maintenance. Für Machine Learning empfehlen wir [Azure Machine Learning Service](/azure/machine-learning/service/).

Der **Speicher für warme Daten** enthält Daten, die direkt vom Gerät für die Berichterstellung und Visualisierung verfügbar sein müssen. Für den Speicher für warme Daten empfehlen wir [Cosmos DB](/azure/cosmos-db/introduction). Cosmos DB ist eine global verteilte Datenbank, die mehrere Modelle unterstützt.

Der **Speicher für kalte Daten** enthält Daten, die über einen längeren Zeitraum gespeichert und zur Batchverarbeitung verwendet werden. Für den Speicher für kalte Daten empfehlen wir [Azure Blob Storage](/azure/storage/blobs/storage-blobs-introduction). Daten können in Blob Storage kostengünstig für unbegrenzte Zeit archiviert werden und sind dort für die Batchverarbeitung leicht zugänglich.

Durch die **Datentransformation** wird der Telemetriedatenstrom bearbeitet oder aggregiert. Beispiele hierfür sind die Protokolltransformation, z. B. das Konvertieren von Binärdaten in JSON, oder das Kombinieren von Datenpunkten. Wenn die Daten transformiert werden müssen, bevor sie IoT Hub erreichen, empfehlen wir die Verwendung eines [Protokollgateways](/azure/iot-hub/iot-hub-protocol-gateway) (nicht dargestellt). Andernfalls können die Daten transformiert werden, nachdem sie IoT Hub erreicht haben. In diesem Fall empfehlen wir die Verwendung von [Azure Functions](/azure/azure-functions/). Functions bietet standardmäßige Integration in IoT Hub, Cosmos DB und Blob Storage.

Durch die **Integration von Geschäftsprozessen** werden basierend auf den Erkenntnissen aus den Gerätedaten Aktionen ausgeführt. Bei diesen Aktionen kann es sich um das Speichern von Informationsmeldungen, Auslösen von Alarmen, Senden von E-Mails oder SMS oder die Integration in CRM handeln. Für die Integration von Geschäftsprozessen empfehlen wir [Azure Logic Apps](/azure/logic-apps/logic-apps-overview).

Die **Benutzerverwaltung** beschränkt die Benutzer oder Gruppen, die Aktionen auf Geräten ausführen können (z. B. Aktualisierung der Firmware). Außerdem definiert sie Funktionen für Benutzer in Anwendungen. Für die Authentifizierung und Autorisierung von Benutzern empfehlen wir [Azure Active Directory](/azure/active-directory/).

## <a name="scalability-considerations"></a>Überlegungen zur Skalierbarkeit

Eine IoT-Anwendung sollte in Form von separaten Diensten erstellt werden, die unabhängig voneinander skaliert werden können. Beachten Sie die folgenden Hinweise zur Skalierbarkeit:

**IoTHub**. Beachten Sie für IoT Hub die folgenden Skalierungsfaktoren:

- Maximales [tägliches Kontingent](/azure/iot-hub/iot-hub-devguide-quotas-throttling) von Nachrichten, die an IoT Hub gesendet werden
- Kontingent von verbundenen Geräten in einer IoT Hub-Instanz
- Erfassungsdurchsatz, d. h. wie schnell Nachrichten von IoT Hub erfasst werden können
- Verarbeitungsdurchsatz, d. h. wie schnell die eingehenden Nachrichten verarbeitet werden

Jede IoT Hub-Instanz wird mit einer bestimmten Anzahl von Einheiten zu einem spezifischen Tarif bereitgestellt. Der Tarif und die Anzahl von Einheiten bestimmen das maximale tägliche Kontingent von Nachrichten, die Geräte an den Hub senden können. Weitere Informationen finden Sie in der Referenz zu IoT Hub-Kontingenten und -Drosselung. Sie können einen Hub ohne Unterbrechung bestehender Vorgänge zentral hochskalieren.

**Stream Analytics**. Stream Analytics-Aufträge lassen sich am besten skalieren, wenn sie an allen Punkten in der Stream Analytics-Pipeline parallel sind – von der Eingabe der Abfrage bis zur Ausgabe. Bei einem vollständig parallelen Auftrag kann Stream Analytics die Arbeit auf mehrere Computeknoten aufteilen. Andernfalls muss Stream Analytics die Streamdaten an einer Stelle kombinieren. Weitere Informationen finden Sie unter [Nutzen der Parallelisierung von Abfragen in Azure Stream Analytics](/azure/stream-analytics/stream-analytics-parallelization).

IoT Hub partitioniert Gerätenachrichten automatisch auf Grundlage der Geräte-ID. Alle Nachrichten von einem bestimmten Gerät gelangen immer auf dieselbe Partition, eine einzelne Partition enthält jedoch Nachrichten von mehreren Geräten. Aus diesem Grund ist die Partitions-ID die Einheit der Parallelisierung.

**Funktionen**. Beim Lesen vom Event Hubs-Endpunkt gilt eine maximal Anzahl von Funktionsinstanzen pro Event Hub-Partition. Die maximale Verarbeitungsrate hängt davon ab, wie schnell eine Funktionsinstanz die Ereignisse aus einer einzelnen Partition verarbeiten kann. Die Funktion sollte Nachrichten in Batches verarbeiten.

**Cosmos DB**: Um eine Cosmos DB-Sammlung horizontal zu skalieren, erstellen Sie die Sammlung mit einem Partitionsschlüssel, den Sie in jedem Dokument hinzufügen, das Sie schreiben. Weitere Informationen finden Sie unter [Bewährte Methoden bei der Auswahl eines Partitionsschlüssels](/azure/cosmos-db/partition-data#best-practices-when-choosing-a-partition-key).

- Wenn Sie ein einziges Dokument pro Gerät speichern und aktualisieren, eignet sich die Geräte-ID als Partitionsschlüssel. Schreibvorgänge werden gleichmäßig auf die Schlüssel verteilt. Die Größe der einzelnen Partitionen ist streng begrenzt, da ein einzelnes Dokument für jeden Schlüsselwert vorhanden ist.
- Wenn Sie für jede Gerätenachricht ein separates Dokument speichern, wäre das Limit von 10 GB pro Partition bei der Verwendung der Geräte-ID als Partitionsschlüssel schnell überschritten. In diesem Fall eignet sich die Nachrichten-ID besser als Partitionsschlüssel. Die Geräte-ID schließen Sie zum Indizieren und Ausführen von Abfragen in der Regel trotzdem in das Dokument ein.

## <a name="security-considerations"></a>Sicherheitshinweise

### <a name="trustworthy-and-secure-communication"></a>Vertrauenswürdige und sichere Kommunikation

Alle Informationen, die mit einem Gerät ausgetauscht werden, müssen vertrauenswürdig sein. Sofern ein Gerät die folgenden Kryptografiefunktionen nicht unterstützt, sollte es auf lokale Netzwerke beschränkt werden, und die gesamte Kommunikation zwischen den Netzwerken sollte über ein Bereichsgateway geleitet werden:

- Datenverschlüsselung mit einem nachweislich sicheren, öffentlich analysierten und umfassend implementierten Verschlüsselungsalgorithmus für den symmetrischen Schlüssel.
- Digitale Signatur mit einem nachweislich sicheren, öffentlich analysierten und umfassend implementierten Signaturalgorithmus für den symmetrischen Schlüssel.
- Unterstützung für TLS 1.2 für TCP oder andere streambasierte Kommunikationspfade oder DTLS 1.2 für datagrammbasierte Kommunikationspfade. Die Unterstützung der Verarbeitung von x.509-Zertifikaten ist optional und kann durch den bezüglich Compute und Übertragung effizienteren Modus mit vorinstallierten Schlüsseln für TLS ersetzt werden (dieser Modus kann mit Unterstützung für die AES- und SHA-2-Algorithmen implementiert werden).
- Aktualisierbarer Schlüsselspeicher und Schlüssel pro Gerät. Jedes Gerät muss über eindeutige Schlüsseldaten oder Token verfügen, die das Gerät gegenüber dem System identifizieren. Die Geräte müssen den Schlüssel sicher auf dem Gerät speichern (z. B. mit einem sicheren Schlüsselspeicher). Das Gerät muss die Schlüssel oder Token in regelmäßigen Abständen oder reaktiv in Notfallsituationen (z. B. bei einer Systemverletzung) aktualisieren können.
- Die Firmware und die Anwendungssoftware auf dem Gerät müssen Updates zur Reparatur erkannter Sicherheitsrisiken zulassen.

Viele Geräte sind jedoch zu eingeschränkt, um diese Anforderungen zu unterstützen. In diesem Fall sollte ein Bereichsgateway verwendet werden. Geräte stellen über ein lokales Netzwerk eine sichere Verbindung mit dem Bereichsgateway her, und das Gateway ermöglicht die sichere Kommunikation mit der Cloud.

### <a name="physical-tamper-proofing"></a>Überprüfung auf physische Manipulation

Zum Gewährleisten der Sicherheit, Integrität und Vertrauenswürdigkeit des gesamten Systems wird dringend empfohlen, Geräte mit integrierten Features zum Schutz vor physischen Manipulationsversuchen zu verwenden.

Beispiel: 

- Nutzen Sie Mikrocontroller/Mikroprozessoren oder zusätzliche Hardware, die eine sichere Speicherung und Verwendung von kryptografischen Schlüsseldaten bieten, z. B. die Integration des Trusted Platform Module (TPM).
- Im TPM sollten ein sicheres Startladeprogramm und sicheres Laden von Software verankert sein.
- Verwenden Sie Sensoren, um Eindringversuche und Versuche zur Manipulation der Geräteumgebung mit Warnungen sowie ggf. „digitaler Selbstzerstörung“ des Geräts zu erkennen.

Weitere Überlegungen zur Sicherheit finden Sie unter [Internet der Dinge (IoT) – Sicherheitsarchitektur](/azure/iot-fundamentals/iot-security-architecture).

### <a name="monitoring-and-logging"></a>Überwachung und Protokollierung

Protokollierungs- und Überwachungssysteme werden verwendet, um zu ermitteln, ob die Lösung funktioniert, und um Probleme zu beheben. Mithilfe von Überwachungs- und Protokollierungssystemen lassen sich folgende betriebliche Fragen beantworten:

- Liegt für Geräte oder Systeme eine Fehlerbedingung vor?
- Sind Geräte oder Systeme korrekt konfiguriert?
- Generieren Geräte oder Systeme genaue Daten?
- Erfüllen Systeme die Erwartungen des Unternehmens und der Endkunden?

Protokollierungs- und Überwachungstools bestehen in der Regel aus den folgenden vier Komponenten:

- Tools zur Visualisierung der Systemleistung und Zeitachsenvisualisierung für die Überwachung des Systems und die grundlegende Problembehandlung
- Gepufferte Datenerfassung zum Puffern von Protokolldaten
- Persistenter Speicher zum Speichern von Protokolldaten
- Such- und Abfragefunktionen zum Anzeigen von Protokolldaten für die detaillierte Problembehandlung

Überwachungssysteme liefern Erkenntnisse zur Integrität, Sicherheit, Stabilität und Leistung einer IoT-Lösung. Diese Systeme können auch eine detailliertere Ansicht bieten, Änderungen an der Komponentenkonfiguration aufzeichnen und extrahierte Protokollierungsdaten zum Erkennen potenzieller Sicherheitsrisiken bereitstellen, den Incident Management-Prozess optimieren und den Systembesitzer bei der Problembehandlung unterstützen. Umfassende Überwachungslösungen bieten die Möglichkeit, Informationen für bestimmte Subsysteme abzufragen oder für mehrere Subsysteme zu aggregieren.

Beim Entwickeln eines Überwachungssystems sollten in einem ersten Schritt der ordnungsgemäße Betrieb sowie die Konformitäts- und Überwachungsanforderungen definiert werden. Folgende Metriken können erfasst werden:

- Physische Geräte, Edgegeräte und Infrastrukturkomponenten, die Konfigurationsänderungen melden
- Anwendungen, die Konfigurationsänderungen, Sicherheitsüberwachungsprotokolle, Anforderungsraten, Antwortzeiten, Fehlerraten und Garbage Collection-Statistiken für verwaltete Sprachen melden
- Datenbanken, persistente Speicher und Caches, die die Abfrage- und Schreibleistung, Schemaänderungen, Sicherheitsüberwachungsprotokolle, Sperren oder Deadlocks, die Indexleistung sowie die CPU-, Speicher- und Datenträgerauslastung melden
- Verwaltete Dienste (IaaS, PaaS, SaaS und FaaS), die für die Integrität und Leistung abhängiger Systeme relevante Integritätsmetriken und Konfigurationsänderungen melden

Die Visualisierung von Überwachungsmetriken weist Bediener auf Instabilitäten des Systems hin und unterstützt sie bei der Reaktion auf Vorfälle.

### <a name="tracing-telemetry"></a>Ablaufverfolgung von Telemetriedaten

Mithilfe der Ablaufverfolgung von Telemetriedaten kann der Bediener nachverfolgen, über welche Pfade Telemetriedaten nach ihrer Erstellung im System übertragen werden. Die Ablaufverfolgung ist wichtig für das Debuggen und die Problembehandlung. Für IoT-Lösungen, die Azure IoT Hub und die [IoT Hub-Geräte-SDKs](/azure/iot-hub/iot-hub-devguide-sdks) verwenden, können Ablaufverfolgungsdatagramme aus den zwischen der Cloud und dem Gerät ausgetauschten Nachrichten erstellt und in den Telemetriedatenstrom eingeschlossen werden.

### <a name="logging"></a>Protokollierung

Protokollierungssysteme sind entscheidend für das Verständnis der von einer Lösung ausgeführten Aktionen oder Aktivitäten sowie der aufgetretenen Fehler und können die Behebung dieser Fehler erleichtern. Protokolle können analysiert werden, um Fehlerbedingungen zu verstehen und zu beheben, Leistungsmerkmale zu verbessern sowie die Konformität mit den geltenden Regeln und Bestimmungen sicherzustellen.

Die Nur-Text-Protokollierung verursacht zwar geringere Vorlaufkosten für die Entwicklung, ist für einen Computer aber schwieriger zu analysieren und zu lesen. Wir empfehlen die Verwendung einer strukturierten Protokollierung, da die gesammelten Informationen in diesem Fall sowohl für Computer analysierbar als auch für Menschen lesbar sind. Bei der strukturierten Protokollierung enthalten die Protokollinformationen auch den situativen Kontext und Metadaten. Eigenschaften haben bei der strukturierten Protokollierung Priorität und werden als Schlüssel-Wert-Paare oder mit einem festen Schema formatiert, um die Such- und Abfragefunktionen zu optimieren.

## <a name="next-steps"></a>Nächste Schritte

- Eine ausführlichere Erläuterung der empfohlenen Architektur und der Implementierungsoptionen finden Sie in der PDF-Datei [Microsoft Azure IoT Reference Architecture](http://aka.ms/iotrefarchitecture) (Microsoft Azure IoT-Referenzarchitektur).

- Eine ausführliche Dokumentation der verschiedenen Azure IoT-Dienste finden Sie unter [Azure IoT-Grundlagen](/azure/iot-fundamentals/).

- Eine IoT-Beispielimplementierung ist auf [GitHub](https://github.com/mspnp/iot-guidance) verfügbar.
