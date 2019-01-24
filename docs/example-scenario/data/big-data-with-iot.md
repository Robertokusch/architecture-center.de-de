---
title: IoT und Datenanalyse in der Bauindustrie
titleSuffix: Azure Example Scenarios
description: Verwenden Sie IoT-Geräte und Datenanalysen für eine umfassende Verwaltung und den Betrieb bei Bauprojekten.
author: alexbuckgit
ms.date: 08/29/2018
ms.topic: example-scenario
ms.service: architecture-center
ms.subservice: example-scenario
ms.custom: IoT, data-analytics
ms.openlocfilehash: dba67dfa7eb480a892229a9bc57d5c5f7ee21017
ms.sourcegitcommit: 1b50810208354577b00e89e5c031b774b02736e2
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/23/2019
ms.locfileid: "54488153"
---
# <a name="iot-and-data-analytics-in-the-construction-industry"></a>IoT und Datenanalyse in der Bauindustrie

Dieses Beispielszenario ist für Entwickler von Lösungen relevant, die Daten zahlreicher IoT-Geräte in eine umfassende Datenanalysearchitektur integrieren, um Entscheidungsprozesse zu verbessern und zu automatisieren. Zu den möglichen Anwendungsbereichen zählen unter anderem die Baubranche, Bergbau und die Fertigungsbranche sowie andere Branchen, in denen große Datenmengen aus zahlreichen IoT-basierten Dateneingaben anfallen.

In diesem Szenario stellt ein Baugerätehersteller Fahrzeuge, Messgeräte und Drohnen her, die mit IoT- und GPS-Technologien ausgestattet sind und Telemetriedaten generieren. Das Unternehmen möchte seine Datenarchitektur modernisieren, um die Betriebsumgebung und den Zustand der Geräte besser überwachen zu können. Eine Möglichkeit wäre, die alte Lösung des Unternehmens durch eine lokale Infrastruktur zu ersetzen. Dies wäre jedoch sehr aufwendig, und die neue Lösung wäre nicht ausreichend skalierbar, um das erwartete Datenvolumen zu bewältigen.

Das Unternehmen möchte eine cloudbasierte intelligente Lösung für die Baubranche aufbauen. Diese Lösung soll umfassende Daten für eine Baustelle sammeln sowie den Betrieb und die Wartung der verschiedenen Baustellenkomponenten automatisieren. Das Unternehmen hat folgende Ziele:

- Integrieren und Analysieren sämtlicher Baugeräte und Daten, um Standzeiten zu minimieren und Diebstählen vorzubeugen
- Ermöglichen der Remotesteuerung und Automatisierung von Baugeräten, um die Auswirkungen des Arbeitskräftemangels zu kompensieren und langfristig die Anzahl der benötigten Arbeiter zu senken sowie schlechter ausgebildeten Arbeitern eine Chance zu geben
- Senken der Betriebskosten und Arbeitsanforderungen für die zugrunde liegende Infrastruktur bei gleichzeitiger Erhöhung der Produktivität und Sicherheit
- Problemloses Skalieren der Infrastruktur zur Bewältigung eines höheren Telemetriedatenaufkommens
- Einhalten aller relevanten gesetzlichen Vorgaben durch Bereitstellung von Ressourcen innerhalb des Landes, ohne die Verfügbarkeit des Systems zu beeinträchtigen
- Verwenden von Open-Source-Software zur Maximierung der Investition in die aktuellen Kenntnisse der Arbeiter

Durch die Verwendung verwalteter Azure-Dienste wie IoT Hub und HDInsight kann der Kunde schnell eine umfassende Lösung mit geringeren Betriebskosten aufbauen und bereitstellen. Sollten Sie noch weitere Datenanalyseanforderungen haben, sehen Sie sich die Liste mit den verfügbaren [vollständig verwalteten Datenanalysediensten in Azure][product-category] an.

## <a name="relevant-use-cases"></a>Relevante Anwendungsfälle

Zu den weiteren relevanten Anwendungsfällen zählen:

- Bauwesen, Bergbau oder Herstellung von Baugeräten
- Sammlung umfangreicher Gerätedaten zur Speicherung und Analyse
- Erfassung und Analyse umfangreicher Datasets

## <a name="architecture"></a>Architecture

![Architektur für IoT und Datenanalyse in der Bauindustrie][architecture]

Die Daten durchlaufen die Lösung wie folgt:

1. Baugeräte sammeln Sensordaten und senden die resultierenden Baudaten in regelmäßigen Abständen an Webdienste mit Lastenausgleich, die in einem Cluster virtueller Azure-Computer gehostet werden.
2. Die benutzerdefinierten Webdienste erfassen die resultierenden Baudaten und speichern Sie in einem Apache Cassandra-Cluster, der ebenfalls auf virtuellen Azure-Computern basiert.
3. Ein weiteres Dataset wird von IoT-Sensoren verschiedener Baugeräte gesammelt und an IoT Hub gesendet.
4. Die gesammelten Rohdaten werden von IoT Hub direkt an Azure Blob Storage gesendet und können sofort angezeigt und analysiert werden.
5. Über IoT Hub gesammelte Daten werden nahezu in Echtzeit durch einen Azure Stream Analytics-Auftrag verarbeitet und in einer Azure SQL-Datenbank gespeichert.
6. Über die Webanwendung „Smart Construction Cloud“ können Analysten und Endbenutzer Sensordaten und Bilder anzeigen und analysieren.
7. Batchaufträge werden bei Bedarf von Benutzern der Webanwendung initiiert. Der Batchauftrag wird in Apache Spark in HDInsight ausgeführt und analysiert neue Daten, die im Cassandra-Cluster gespeichert wurden.

### <a name="components"></a>Komponenten

- [IoT Hub](/azure/iot-hub/about-iot-hub) fungiert als zentraler Nachrichtenhub für die sichere bidirektionale Kommunikation mit gerätespezifischer Identität zwischen Cloudplattform, Baugeräten und anderen Baustellenkomponenten. IoT Hub kann schnell Daten für jedes Gerät sammeln, die dann in der Datenanalysepipeline erfasst werden.
- [Azure Stream Analytics](/azure/stream-analytics/stream-analytics-introduction) ist eine Ereignisverarbeitungsengine, die große Datenmengen analysieren kann, die von Geräten und anderen Quellen gestreamt werden. Zur Identifizierung von Mustern und Beziehungen wird zudem das Extrahieren von Informationen aus Datenströmen unterstützt. In diesem Szenario erfasst und analysiert Stream Analytics Daten von IoT-Geräten und speichert die Ergebnisse in Azure SQL-Datenbank.
- [Azure SQL-Datenbank](/azure/sql-database/sql-database-technical-overview) enthält die Ergebnisse der analysierten Daten von IoT-Geräten und Messgeräten. Diese können von Analysten und Benutzern über eine Azure-basierte Webanwendung angezeigt werden.
- [Blob Storage](/azure/storage/blobs/storage-blobs-introduction) speichert Bilddaten, die von den IoT Hub-Geräten erfasst wurden. Die Bilddaten können über die Webanwendung angezeigt werden.
- [Traffic Manager](/azure/traffic-manager/traffic-manager-overview) steuert die Verteilung von Benutzerdatenverkehr für Dienstendpunkte in unterschiedlichen Azure-Regionen.
- [Load Balancer](/azure/load-balancer/load-balancer-overview) verteilt Datenübermittlungen von Baugeräten auf die VM-basierten Webdienste, um eine hohe Verfügbarkeit zu gewährleisten.
- [Virtuelle Azure-Computer](/azure/virtual-machines) fungieren als Host für die Webdienste, die die resultierenden Baudaten empfangen und in der Apache Cassandra-Datenbank erfassen.
- [Apache Cassandra](https://cassandra.apache.org) ist eine verteilte NoSQL-Datenbank zum Speichern von Baudaten, die später durch Apache Spark verarbeitet werden.
- [Web-Apps](/azure/app-service/app-service-web-overview) hostet die Endbenutzer-Webanwendung, mit der Quelldaten und Bilder abgefragt und angezeigt werden können. Über die Anwendung können Benutzer auch Batchaufträge in Apache Spark initiieren.
- [Apache Spark in HDInsight](/azure/hdinsight/spark/apache-spark-overview) unterstützt In-Memory-Verarbeitung, um die Leistung von Anwendungen bei der Analyse großer Datenmengen zu steigern. In diesem Szenario wird Spark verwendet, um komplexe Algorithmen für die in Apache Cassandra gespeicherten Daten auszuführen.

### <a name="alternatives"></a>Alternativen

- [Cosmos DB](/azure/cosmos-db/introduction) ist eine alternative NoSQL-Datenbanktechnologie. Cosmos DB bietet [Multimasterunterstützung auf globaler Ebene](/azure/cosmos-db/multi-region-writers) mit [mehreren klar definierten Konsistenzebenen](/azure/cosmos-db/consistency-levels), um verschiedenste Kundenanforderungen zu erfüllen. Darüber hinaus unterstützt Cosmos DB auch die [Cassandra-API](/azure/cosmos-db/cassandra-introduction).
- [Azure Databricks](/azure/azure-databricks/what-is-azure-databricks) ist eine Apache Spark-basierte Analyseplattform, die für Azure optimiert ist. Dank Azure-Integration ermöglicht sie die Einrichtung mit nur einem Klick und bietet optimierte Workflows sowie einen interaktiven Arbeitsbereich für die Zusammenarbeit.
- [Data Lake Storage](/azure/storage/data-lake-storage) ist eine Alternative zu Blob Storage. Bei diesem Szenario war Data Lake Storage in der Zielregion nicht verfügbar.
- [Web-Apps](/azure/app-service) kann auch zum Hosten der Webdienste für die Erfassung der resultierenden Baudaten verwendet werden.
- Für die Echtzeiterfassung von Nachrichten, Datenspeicherung, Datenstromverarbeitung und Speicherung von Analysedaten sowie für Analysen und Berichte stehen zahlreiche Technologieoptionen zur Verfügung. Eine Übersicht über diese Optionen, die zugehörigen Funktionen und wichtige Auswahlkriterien finden Sie unter [Big Data-Architekturen: Echtzeitverarbeitung](/azure/architecture/data-guide/technology-choices/real-time-ingestion) im [Azure-Datenarchitekturleitfaden](/azure/architecture/data-guide).

## <a name="considerations"></a>Überlegungen

Die breite Verfügbarkeit von Azure-Regionen ist ein wichtiger Faktor für dieses Szenario. Die Verfügbarkeit mehrerer Regionen in einem einzelnen Land kann bei der Notfallwiederherstellung genutzt werden und ermöglicht gleichzeitig die Einhaltung vertraglicher Verpflichtungen sowie der Anforderungen von Strafverfolgungsbehörden. Die Hochgeschwindigkeitskommunikation zwischen Azure-Regionen ist ebenfalls ein wichtiger Faktor in diesem Szenario.

Dank der Unterstützung von Open Source-Technologien durch Azure konnte der Kunde auf bereits vorhandene Kenntnisse seines Personals zurückgreifen. Im Vergleich zu einer lokalen Lösung kann der Kunde außerdem schneller neue Technologien einführen und von geringeren Kosten und einer geringeren Arbeitsauslastung profitieren.

## <a name="pricing"></a>Preise

Folgende Aspekte haben erheblichen Einfluss auf die Kosten für diese Lösung:

- Die Kosten für virtuelle Azure-Computer steigen linear, wenn weitere Instanzen bereitgestellt werden. Für virtuelle Computer mit aufgehobener Bereitstellung fallen nur Speicherkosten und keine Computekosten an. Diese Computer mit aufgehobener Zuordnung können bei entsprechend hohem Bedarf neu zugeordnet werden.
- Die Kosten für [IoT Hub](https://azure.microsoft.com/pricing/details/iot-hub) hängen von der Anzahl bereitgestellter IoT-Einheiten sowie vom gewählten Diensttarif ab, der wiederum die Anzahl der pro Tag und Einheit zulässigen Nachrichten bestimmt.
- Die Kosten für [Stream Analytics](https://azure.microsoft.com/pricing/details/stream-analytics) hängen von der Anzahl von Streamingeinheiten ab, die für die Verarbeitung der Daten im Dienst erforderlich sind.

## <a name="related-resources"></a>Zugehörige Ressourcen

In der [Kundengeschichte von Komatsu][customer-story] können Sie sich eine Implementierung einer ähnlichen Architektur ansehen.

Einen Leitfaden für Big Data-Architekturen finden Sie im [Azure-Datenarchitekturleitfaden](/azure/architecture/data-guide).

<!-- links -->

[product-category]: https://azure.microsoft.com/product-categories/analytics/
[customer-site]: https://home.komatsu/en/
[customer-story]: https://customers.microsoft.com/story/komatsu-manufacturing-azure-iot-hub-japan
[architecture]: ./media/architecture-big-data-with-iot.png
