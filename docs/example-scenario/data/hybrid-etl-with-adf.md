---
title: ETL-Hybridvorgänge mit vorhandenen lokalen SSIS-Bereitstellungen und Azure Data Factory
titleSuffix: Azure Example Scenarios
description: ETL-Hybridvorgänge mit vorhandenen lokalen SSIS-Bereitstellungen (SQL Server Integration Services) und Azure Data Factory.
author: alhieng
ms.date: 09/20/2018
ms.topic: example-scenario
ms.service: architecture-center
ms.subservice: example-scenario
ms.custom: tsp-team
social_image_url: /azure/architecture/example-scenario/data/media/architecture-diagram-hybrid-etl-with-adf.png
ms.openlocfilehash: 354b8ee14f82631842902da3de852f777b1954cc
ms.sourcegitcommit: c053e6edb429299a0ad9b327888d596c48859d4a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/20/2019
ms.locfileid: "58248495"
---
# <a name="hybrid-etl-with-existing-on-premises-ssis-and-azure-data-factory"></a>ETL-Hybridvorgänge mit vorhandenen lokalen SSIS-Bereitstellungen und Azure Data Factory

Organisationen, die ihre SQL Server-Datenbanken in die Cloud migrieren, können von erheblichen Kosteneinsparungen und Leistungszuwächsen sowie von einer höheren Flexibilität und einer besseren Skalierbarkeit profitieren. Die Überarbeitung bereits vorhandener ETL-Prozesse (Extrahieren, Transformieren und Laden), die mit SQL Server Integration Services (SSIS) erstellt wurden, kann sich jedoch als Migrationshindernis erweisen. In anderen Fällen erfordert der Prozess zum Laden von Daten eine komplexe Logik bzw. bestimmte Datentoolkomponenten, die von Azure Data Factory v2 noch nicht unterstützt werden. Häufig werden SSIS-Funktionen wie Transformationen für Fuzzysuche und Fuzzygruppierung, CDC (Change Data Capture), SCD (Slowly Changing Dimensions) und DQS (Data Quality Services) verwendet.

Für die Lift & Shift-Migration einer vorhandenen SQL-Datenbank empfiehlt sich unter Umständen die Verwendung eines ETL-Hybridansatzes. Ein Hybridansatz verwendet zwar Data Factory als primäre Orchestrierungsengine, nutzt aber weiterhin vorhandene SSIS-Pakete für die Bereinigung von Daten und die Verwendung lokaler Ressourcen. Bei diesem Ansatz wird die Data Factory-basierte SQL Server-IR (Integration Runtime) verwendet, um eine Lift & Shift-Migration vorhandener Datenbanken in die Cloud zu ermöglichen, und es werden bereits vorhandener Code und vorhandene SSIS-Pakete genutzt.

Dieses Beispielszenario ist für Organisationen relevant, die Datenbanken in die Cloud verlagern und die Verwendung von Data Factory als primäre cloudbasierte ETL-Engine in Betracht ziehen, während sie vorhandene SSIS-Pakete in ihren neuen Clouddatenworkflow integrieren. Viele Organisationen haben stark in die Entwicklung von SSIS-ETL-Paketen für bestimmte Datenaufgaben investiert. Die Vorstellung, diese Pakete umschreiben zu müssen, kann durchaus abschreckend sein. Darüber hinaus sind viele vorhandene Codepakete von lokalen Ressourcen abhängig, was eine Migration in die Cloud verhindert.

Mit Data Factory können Kunden ihre bereits vorhandenen ETL-Pakete nutzen und weitere Investitionen in die lokale ETL-Entwicklung zurückfahren. In diesem Beispiel werden mögliche Anwendungsfälle für die Nutzung vorhandener SSIS-Pakete im Rahmen eines neuen Clouddatenworkflows mit Azure Data Factory v2 erläutert.

## <a name="potential-use-cases"></a>Mögliche Anwendungsfälle

In der Vergangenheit war SSIS für viele SQL Server-Datenexperten das bevorzugte ETL-Tool für Datentransformationen und -ladevorgänge. Gelegentlich wurden bestimmte SSIS-Features oder Plug-In-Komponenten von Drittanbietern verwendet, um die Entwicklung voranzutreiben. Kommt ein Austausch oder eine Neuentwicklung dieser Pakete nicht in Frage, können Kunden ihre Datenbanken nicht in die Cloud migrieren. Kunden suchen daher nach Möglichkeiten, wie sie ihre vorhandenen Datenbanken möglichst effizient in die Cloud migrieren und ihre vorhandenen SSIS-Pakete weiter verwenden können.

Im Anschluss finden Sie einige potenzielle lokale Anwendungsfälle:

- Laden von Netzwerkrouterprotokollen in eine Datenbank zu Analysezwecken
- Vorbereiten von Beschäftigungsdaten der Personalabteilung für Analyseberichte
- Laden von Produkt- und Vertriebsdaten in ein Data Warehouse, um Umsatzprognosen zu erstellen
- Automatisieren von Ladevorgänge für Speicher operativer Daten oder Data Warehouses für das Finanz- und Rechnungswesen.

## <a name="architecture"></a>Architecture

![Übersicht über die Architektur eines ETL-Hybridprozesses mit Azure Data Factory][architecture-diagram]

1. Daten werden aus dem Blobspeicher in Data Factory gelesen.
2. Die Data Factory-Pipeline ruft eine gespeicherte Prozedur auf, um einen lokal gehosteten SSIS-Auftrag über die Integrated Runtime auszuführen.
3. Die Datenbereinigungsaufträge werden ausgeführt, um die Daten für die Downstreamnutzung vorzubereiten.
4. Nach erfolgreicher Datenbereinigung werden die bereinigten Daten mithilfe einer Kopieraufgabe in Azure geladen.
5. Danach werden die bereinigten Daten in Tabellen in SQL Data Warehouse geladen.

### <a name="components"></a>Komponenten

- [Blobspeicher][docs-blob-storage] wird als Dateispeicher sowie als Datenquelle für Data Factory verwendet.
- [SQL Server Integration Services][docs-ssis] enthält die lokalen ETL-Pakete für die Ausführung aufgabenspezifischer Workloads.
- [Azure Data Factory][docs-data-factory] ist die Cloudorchestrierungsengine, die Daten aus mehreren Quellen miteinander kombiniert, orchestriert und in ein Data Warehouse lädt.
- [SQL Data Warehouse][docs-sql-data-warehouse] zentralisiert Daten in der Cloud, sodass über standardmäßige ANSI-SQL-Abfragen problemlos auf sie zugegriffen werden kann.

### <a name="alternatives"></a>Alternativen

Data Factory kann auch Datenbereinigungsprozeduren aufrufen, die mit anderen Technologien (etwa mit einem Databricks-Notebook, einem Python-Skript oder einer auf einem virtuellen Computer ausgeführten SSIS-Instanz) implementiert werden. Eine mögliche Alternative zum Hybridansatz ist das [Installieren kostenpflichtiger oder lizenzierter benutzerdefinierter Komponenten für Azure-SSIS Integration Runtime](/azure/data-factory/how-to-develop-azure-ssis-ir-licensed-components).

## <a name="considerations"></a>Überlegungen

Die Integrated Runtime (IR) unterstützt zwei Modelle: selbstgehostet und von Azure gehostet. Sie müssen sich zunächst für eine dieser beiden Optionen entscheiden. Die selbstgehostete Variante ist kostengünstiger, aber auch mit einem höheren Wartungs- und Verwaltungsaufwand verbunden. Weitere Informationen finden Sie unter [Selbstgehostete Integrationslaufzeit](/azure/data-factory/concepts-integration-runtime#self-hosted-integration-runtime). Eine Entscheidungshilfe für die Wahl der passenden IR finden Sie bei Bedarf unter [Ermitteln der richtigen Integrationslaufzeit](/azure/data-factory/concepts-integration-runtime#determining-which-ir-to-use).

Bei Verwendung der von Azure gehosteten Variante müssen Sie entscheiden, wie viel Leistung für die Verarbeitung Ihrer Daten erforderlich ist. Bei der von Azure gehosteten Konfiguration können Sie im Rahmen der Konfigurationsschritte die VM-Größe auswählen. Weitere Informationen zum Auswählen von VM-Größen finden Sie unter [Überlegungen zur Leistung](/azure/cloud-services/cloud-services-sizes-specs#performance-considerations).

Die Entscheidung wird deutlich einfacher, wenn Sie bereits über SSIS-Pakete mit lokalen Abhängigkeiten verfügen (beispielsweise Datenquellen oder Dateien, auf die nicht von Azure aus zugegriffen werden kann). In diesem Fall kommt nur die selbstgehostete IR in Frage. Dieser Ansatz ist am flexibelsten und ermöglicht es, die Cloud als Orchestrierungsengine zu nutzen, ohne vorhandene Pakete umschreiben zu müssen.

Letztendlich geht es darum, die verarbeiteten Daten zur weiteren Optimierung in die Cloud zu laden oder mit anderen in der Cloud gespeicherten Daten zu kombinieren. Achten Sie im Rahmen des Entwurfsprozesses auf die Anzahl von Aktivitäten, die in den Data Factory-Pipelines verwendet werden. Weitere Informationen finden Sie unter [Pipelines und Aktivitäten in Azure Data Factory](/azure/data-factory/concepts-pipelines-activities).

## <a name="pricing"></a>Preise

Data Factory ist eine kostengünstige Möglichkeit, um die Datenverschiebung in die Cloud zu orchestrieren. Die Kosten basieren auf verschiedenen Faktoren:

- Anzahl von Pipelineausführungen
- Anzahl verwendeter Entitäten/Aktivitäten innerhalb der Pipeline
- Anzahl von Überwachungsvorgängen
- Anzahl von Integrationsdurchläufen (in Azure gehostete IR oder selbstgehostete IR)

Für Data Factory wird eine nutzungsbasierte Abrechnung verwendet. Kosten fallen daher nur während Pipelineausführungen und während der Überwachung an. Die Ausführung einer einfachen Pipeline kostet gerade einmal 50 Cent, die Überwachung lediglich 25 Cent. Mit dem [Azure-Kostenrechner](https://azure.microsoft.com/pricing/calculator/) können Sie eine genauere Schätzung auf der Grundlage Ihrer spezifischen Workload erstellen.

Wenn Sie eine ETL-Hybridworkload ausführen, müssen Sie auch die Kosten für den virtuellen Computer berücksichtigen, der als Host für Ihre SSIS-Pakete fungiert. Diese Kosten richten sich nach der Größe des virtuellen Computers und reichen von D1v2 (ein Kern, 3,5 GB RAM, Datenträger mit 50 GB) bis E64V3 (64 Kerne, 432 GB RAM, Datenträger mit 1.600 GB). Weitere Informationen zur Wahl der geeigneten VM-Größe finden Sie unter [Überlegungen zur Leistung](/azure/cloud-services/cloud-services-sizes-specs#performance-considerations).

## <a name="next-steps"></a>Nächste Schritte

- Informieren Sie sich ausführlicher über [Azure Data Factory](https://azure.microsoft.com/services/data-factory/).
- Absolvieren Sie das [Schritt-für-Schritt-Tutorial](/azure/data-factory/#step-by-step-tutorials), um sich mit Azure Data Factory vertraut zu machen.
- [Stellen Sie Azure-SSIS Integration Runtime in Azure Data Factory bereit.](/azure/data-factory/tutorial-deploy-ssis-packages-azure)

<!-- links -->
[architecture-diagram]: ./media/architecture-diagram-hybrid-etl-with-adf.png
[small-pricing]: https://azure.com/e/
[medium-pricing]: https://azure.com/e/
[large-pricing]: https://azure.com/e/
[availability]: /azure/architecture/checklist/availability
[resource-groups]: /azure/azure-resource-manager/resource-group-overview
[resiliency]: /azure/architecture/resiliency/
[security]: /azure/security/
[scalability]: /azure/architecture/checklist/scalability
[docs-blob-storage]: /azure/storage/blobs/
[docs-data-factory]: /azure/data-factory/introduction
[docs-resource-groups]: /azure/azure-resource-manager/resource-group-overview
[docs-ssis]: /sql/integration-services/sql-server-integration-services
[docs-sql-data-warehouse]: /azure/sql-data-warehouse/sql-data-warehouse-overview-what-is
