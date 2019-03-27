---
title: 'Mainframemigration: Migration von Mainframeanwendungen'
description: Migrieren von Anwendungen aus Mainframe-Umgebungen zu Azure, einer bewährten, hoch verfügbaren und skalierbaren Infrastruktur für Systeme, die derzeit auf Mainframes ausgeführt werden
author: njray
ms.date: 12/26/2018
ms.openlocfilehash: 2a22eb038da693671ce309c76afcfc41946034f3
ms.sourcegitcommit: c053e6edb429299a0ad9b327888d596c48859d4a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/20/2019
ms.locfileid: "58246391"
---
# <a name="mainframe-application-migration"></a>Migration von Mainframeanwendungen

Bei der Migration von Anwendungen aus Mainframe-Umgebungen zu Azure verfolgen die meisten Teams einen pragmatischen Ansatz: Nach Möglichkeit werden die Anwendungen weiterverwendet. Anschließend wird eine Bereitstellung in Phasen gestartet, bei der Anwendungen neu geschrieben oder ersetzt werden.

Die Anwendungsmigration umfasst üblicherweise eine oder mehrere der folgenden Strategien:

- Zuweisen eines neuen Hosts: Sie können vorhandenen Code, Programme und Anwendungen aus dem Mainframe verlagern. Anschließend wird der Code neu kompiliert, sodass er in einem Mainframe-Emulator ausgeführt werden kann, der in einer Cloudinstanz gehostet wird. Bei diesem Ansatz werden normalerweise als Erstes die Anwendungen in einen cloudbasierten Emulator verschoben, und dann wird die Datenbank zu einer cloudbasierten Datenbank migriert. Neben den Daten- und Dateikonvertierungen ist etwas Engineering und Refactoring erforderlich.

    Alternativ dazu können Sie die Zuweisung eines neuen Hosts über einen herkömmlichen Hostinganbieter durchführen. Einer der Hauptvorteile der Cloud besteht im Outsourcing der Infrastrukturverwaltung. Sie können einen Rechenzentrumsanbieter finden, der Ihre Mainframeworkloads für Sie hostet. Durch dieses Modell können Sie Zeit gewinnen, sind nicht so abhängig von einem Anbieter und sparen zwischenzeitlich Kosten ein.

- Außerbetriebnahme: Alle Anwendungen, die nicht mehr benötigt werden, sollten vor der Migration außer Betrieb genommen werden.

- Neuerstellung: Einige Organisationen entscheiden sich dazu, Programme mithilfe moderner Techniken vollständig umzuschreiben. Angesichts der zusätzlichen Kosten und Komplexität dieses Ansatzes ist dies nicht so üblich wie eine Migration per Lift & Shift. Nach dieser Art der Migration ist es häufig sinnvoll, Module und Code mithilfe von Codetransformations-Engines zu ersetzen.

- Ersetzen Sie: Bei diesem Ansatz wird die Mainframefunktionalität durch entsprechende Features in der Cloud ersetzt. Software-as-a-Service (SaaS) ist eine Möglichkeit, bei der eine speziell für Unternehmensbelange erstellte Lösung verwendet wird, darunter Finanzen, Personalwesen, Fertigung oder Unternehmensressourcenplanung. Darüber hinaus stehen jetzt viele branchenspezifische Apps zur Verfügung, um Probleme zu lösen, die zuvor durch benutzerdefinierte Mainframelösungen gelöst wurden.

Sie sollten als Erstes die Workloads planen, die Sie zuerst migrieren möchten, und dann die Anforderungen für das Verschieben der zugehörigen Anwendungen, älteren Codebases und Datenbanken ermitteln.

## <a name="mainframe-emulation-in-azure"></a>Mainframe-Emulation in Azure

Azure-Clouddienste können herkömmliche Mainframe-Umgebungen emulieren, sodass Sie vorhandenen Mainframecode und zugehörige Anwendungen weiterverwenden können. Zu den allgemeinen Serverkomponenten, die Sie emulieren können, gehören OLTP- (Onlinetransaktionsverarbeitung), Batch- und Datenerfassungssysteme.

### <a name="oltp-systems"></a>OLTP-Systeme

Auf vielen Mainframes werden OLTP-Systeme verwendet, die Tausende oder Millionen von Updates für eine riesige Anzahl von Benutzern verarbeiten. Diese Anwendungen verwenden häufig die Transaktionsverarbeitung sowie Software zur Verarbeitung von Bildschirmformularen, beispielsweise CICS (Customer Information Control System), IMS (Information Management System) und TIP (Terminal Interface Processor).

Beim Verlagern von OLTP-Anwendungen in Azure stehen Emulatoren für Monitore der Mainframetransaktionsverarbeitung (TP) zur Ausführung als Infrastructure-as-a-Service (IaaS) mit virtuellen Computern (VMs) in Azure zur Verfügung. Die Bildschirmverarbeitungs- und Formularfunktionalität kann auch Webservern implementiert werden. Für den Datenzugriff und für Transaktionen kann dieser Ansatz mit Datenbank-APIs wie ADO (ActiveX Data Objects), ODBC (Open Database Connectivity) und JDBC (Java Database Connectivity) kombiniert werden.

### <a name="time-constrained-batch-updates"></a>Zeitlich beschränkte Batchupdates

Viele Mainframesysteme führen monatliche oder jährliche Updates von Millionen von Kontodatensätzen durch, wie sie z.B. in Banken, Versicherungen und Behörden verwendet werden. Mainframes verarbeiten diese Arten von Workloads durch die Bereitstellung von Datenverarbeitungssystemen mit hohem Durchsatz. Mainframebatchaufträge sind in der Regel serieller Natur, und ihre Leistung hängt von den Eingabe-/Ausgabevorgängen pro Sekunde (IOPS) ab, die vom Mainframebackbone ermöglicht werden.

Die Leistung cloudbasierter Umgebungen beruht auf parallelen Computeressourcen und Hochgeschwindigkeitsnetzwerken. Wenn Sie die Batchleistung optimieren möchten, bietet Azure verschiedene Compute-, Speicher- und Netzwerkoptionen.

### <a name="data-ingestion-systems"></a>Datenerfassungssysteme

Mainframes erfassen große Datenbatches aus Einzelhandel, Finanzdienstleistungen, Fertigung und anderen Verarbeitungslösungen. Mit Azure können Sie einfache Befehlszeilen-Hilfsprogramme wie z.B. [AzCopy](/azure/storage/common/storage-use-azcopy) zum Kopieren von Daten in und aus dem Speicherort verwenden. Sie können auch den [Azure Data Factory](/azure/data-factory/introduction)-Dienst verwenden, um Daten aus unterschiedlichen Datenspeichern zu erfassen und datengesteuerte Workflows zu erstellen und zu planen.

Neben Emulationsumgebungen stellt Azure auch PaaS-Lösungen (Platform-as-a-Service) und Analysedienste bereit, mit denen vorhandene Mainframe-Umgebungen erweitert werden können.

## <a name="migrate-oltp-workloads-to-azure"></a>Migrieren von Workloads zu Azure

Die Migration per Lift & Shift ist die codelose Option zur schnellen Migration vorhandener Anwendungen zu Azure. Jede Anwendung wird im vorliegenden Zustand migriert, wodurch Sie die Vorteile der Cloud, jedoch ohne die Risiken und Kosten von Codeänderungen erhalten. Durch die Verwendung eines Emulators für Monitore der Mainframetransaktionsverarbeitung (TP) in Azure wird dieser Ansatz unterstützt.

TP-Monitore sind von verschiedenen Herstellern erhältlich und werden auf virtuellen Computern ausgeführt – eine IaaS-Option (Infrastructure-as-a-Service) in Azure. Die folgenden Vorher-/Nachher-Diagramme zeigen die Migration einer von IBM DB2 unterstützten Onlineanwendung, einem Managementsystem für relationale Datenbanken (DBMS), auf einem IBM z/OS-Mainframe. DB2 für z/OS verwendet VSAM-Dateien (Virtual Storage Access Method) zum Speichern der Daten sowie die indizierte sequenzielle Zugriffsmethode (ISAM) für Flatfiles. Diese Architektur verwendet zudem CICS für die Transaktionsüberwachung.

![Migration einer Mainframe-Umgebung per Lift & Shift zu Azure mithilfe von Emulationssoftware](../../_images/mainframe-migration/mainframe-vs-azure.png)

In Azure werden Emulationsumgebungen verwendet, um den TP-Manager und die Batchaufträge auszuführen, die JCL nutzen. In der Datenschicht wird DB2 durch [Azure SQL-Datenbank](/azure/sql-database/sql-database-technical-overview) ersetzt, obwohl auch Microsoft SQL Server, DB2 LUW oder Oracle Database verwendet werden können. Ein Emulator unterstützt IMS, VSAM und SEQ. Die Systemverwaltungstools des Mainframes werden durch Azure-Dienste und Software von anderen Herstellern ersetzt, die auf virtuellen Computern ausgeführt werden.

Die Funktionalität für Bildschirmverarbeitung und Formulareinträge wird häufig über Webserver implementiert, die mit Datenbank-APIs wie ADO, ODBC und JDBC für den Datenzugriff und Transaktionen kombiniert werden können. Die genaue Zusammenstellung zu verwendender Azure-IaaS-Komponenten hängt vom bevorzugten Betriebssystem ab. Beispiel: 

- Windows-basierte virtuelle Computer: Internet Information Server (IIS) zusammen mit ASP.NET für Bildschirmverarbeitung und Geschäftslogik. Verwenden Sie ADO.NET für Datenzugriff und Transaktionen.

- Linux-basierte virtuelle Computer: Die Java-basierten Anwendungsserver, die zur Verfügung stehen, z.B. Apache Tomcat für die Bildschirmverarbeitung und Java-basierte Geschäftsfunktionen. Verwenden Sie JDBC für Datenzugriff und Transaktionen.

## <a name="migrate-batch-workloads-to-azure"></a>Migrieren von Batchworkloads zu Azure

Batchvorgänge in Azure unterscheiden sich von der typischen Batchumgebung auf Mainframes. Mainframebatchaufträge sind in der Regel serieller Natur, und ihre Leistung hängt von den IOPS ab, die vom Mainframebackbone ermöglicht werden. Die Leistung cloudbasierter Umgebungen beruht auf parallelen Computeressourcen und Hochgeschwindigkeitsnetzwerken.

Um die Batchleistung mithilfe von Azure zu optimieren, ziehen Sie die [Compute](/azure/virtual-machines/windows/overview)-, [Speicher](/azure/storage/blobs/storage-blobs-introduction)-, [Netzwerk](https://azure.microsoft.com/blog/maximize-your-vm-s-performance-with-accelerated-networking-now-generally-available-for-both-windows-and-linux/)- und [Überwachungsoptionen](/azure/azure-monitor/overview) wie folgt in Betracht.

### <a name="compute"></a>Compute

Verwendung:

- VMs mit der höchsten Taktfrequenz. Mainframeanwendungen sind häufig Singlethreadanwendungen, und Mainframe-CPUs verfügen über eine sehr hohe Taktfrequenz.

- Virtuelle Computer mit großer Arbeitsspeicherkapazität, um die Zwischenspeicherung von Daten und Anwendungsarbeitsbereichen zu ermöglichen.

- VMs mit vCPUs höherer Dichte, um die Vorteile der Multithreadverarbeitung zu nutzen, wenn die Anwendung mehrere Threads unterstützt.

- Parallele Verarbeitung, da Azure für die Parallelverarbeitung einfach horizontal hochskaliert werden und mehr Computeleistung für eine Batchausführung bereitstellen kann.

### <a name="storage"></a>Storage

Verwendung:

- [Azure SSD Premium](/azure/virtual-machines/windows/premium-storage) oder [Azure SSD Ultra](/azure/virtual-machines/windows/disks-ultra-ssd) für maximal verfügbare IOPS.

- Striping mit mehreren Datenträgern für mehr IOPS pro Speichergröße.

- Partitionierung zur Verteilung der Speicher-E/A auf mehrere Azure-Speichergeräte.

### <a name="networking"></a>Netzwerk

- Verwenden Sie den [beschleunigten Azure-Netzwerkbetrieb](/azure/virtual-network/create-vm-accelerated-networking-powershell), um Wartezeiten zu verringern.

### <a name="monitoring"></a>Überwachung

- Verwenden Sie Überwachungstools. [Azure Monitor](/azure/azure-monitor/overview), [Azure Application Insights](/azure/application-insights/app-insights-overview) und auch die Azure-Protokolle ermöglichen Administratoren die Überwachung einer übermäßigen Auslastung von Batchausführungen und helfen bei der Beseitigung von Engpässen.

## <a name="migrate-development-environments"></a>Migrieren von Entwicklungsumgebungen

Die verteilten Architekturen der Cloud basieren auf einer anderen Reihe von Entwicklungstools, die den Vorteil moderner Methoden und Programmiersprachen bieten. Um diesen Übergang zu erleichtern, können Sie eine Entwicklungsumgebung mit anderen Tools verwenden, die zum Emulieren von IBM z/OS-Umgebungen entwickelt wurden. Die folgende Liste enthält Optionen von Microsoft und anderen Anbietern:

| Komponente        | Azure-Optionen                                                                                                                                  |
|------------------|---------------------------------------------------------------------------------------------------------------------------------------------------|
| z/OS             | Windows, Linux oder UNIX                                                                                                                      |
| CICS             | Von Micro Focus, Oracle, GT Software (Fujitsu), TmaxSoft, Raincode und NTT Data angebotene oder über Kubernetes neu generierte Azure-Dienste |
| IMS              | Von Micro Focus und Oracle angebotene Azure-Dienste                                                                                  |
| Assembler        | Azure-Dienste von Raincode und TmaxSoft bzw. COBOL, C oder Java oder Zuordnung zu Betriebssystemfunktionen               |
| JCL              | JCL, PowerShell oder andere Skripterstellungstools                                                                                                   |
| COBOL            | COBOL, C oder Java                                                                                                                            |
| Natural          | Natural, COBOL, C oder Java                                                                                                                  |
| FORTRAN und PL/I | FORTRAN, PL/I, COBOL, C oder Java                                                                                                           |
| REXX und PL/I    | REXX, PowerShell oder andere Skripterstellungstools                                                                                                  |

## <a name="migrate-databases-and-data"></a>Migrieren von Datenbanken und Daten

Die Anwendungsmigration umfasst in der Regel das Zuweisen eines neuen Hosts für die Datenschicht. Sie können Ihre SQL Server-, Open Source- und anderen relationalen Datenbanken mit [Azure Database Migration Service](/azure/dms/dms-overview) zu vollständig verwalteten Lösungen in Azure migrieren, z.B. zu [verwalteten Azure SQL-Datenbank-Instanzen](/azure/sql-database/sql-database-managed-instance), [Azure Database for PostgreSQL](/azure/postgresql/overview) und [Azure Database for MySQL](/azure/mysql/overview).

Beispielsweise können Sie die Migration durchführen, wenn in der Mainframedatenschicht Folgendes verwendet wird:

- IBM DB2 oder IMS-Datenbank: Verwenden Sie Azure SQL-Datenbank, SQL Server, DB2 LUW oder Oracle Database in Azure.

- VSAM- und andere Flatfiles: Verwenden Sie ISAM-Flatfiles (indizierte sequenzielle Zugriffsmethode) für Azure SQL, SQL Server, DB2 LUW oder Oracle.

- GDGs (Generation Date Groups): Migrieren Sie zu Dateien in Azure, die eine Namenskonvention und Dateinamenerweiterungen sowie eine ähnliche Funktionalität wie GDGs bereitstellen.

Die IBM-Datenschicht umfasst mehrere wichtige Komponenten, die ebenfalls migriert werden müssen. Wenn Sie beispielsweise eine Datenbank migrieren, migrieren Sie auch eine Sammlung von Daten in Pools. Diese enthalten jeweils dbextents, bei denen es sich um z/OS-VSAM-Datasets handelt. Ihre Migration muss das Verzeichnis einschließen, das die Datenspeicherorte in den Speicherpools identifiziert. Darüber hinaus muss Ihr Migrationsplan auch das Datenbankprotokoll berücksichtigen, welches einen Datensatz mit den für die Datenbank ausgeführten Vorgängen enthält. Eine Datenbank kann ein, zwei (dual oder alternativ) oder vier Protokolle (dual und alternativ) aufweisen.

Die Datenbankmigration umfasst außerdem folgende Komponenten:

- Datenbank-Manager: Bietet Zugriff auf Daten in der Datenbank. Der Datenbank-Manager wird in einer eigenen Partition in einer z/OS-Umgebung ausgeführt.

- Anwendungsanforderer: Nimmt Anforderungen von Anwendungen an, bevor sie an einen Anwendungsserver weitergeleitet werden.

- Onlineressourcenadapter: Enthält Komponenten des Anwendungsanforderers zur Verwendung in CICS-Transaktionen.

- Batchressourcenadapter: Implementiert Komponenten des Anwendungsanforderers für z/OS-Batchanwendungen.

- Interactive SQL (ISQL): Wird als CICS-Anwendung und Schnittstelle ausgeführt und ermöglicht den Benutzern die Eingabe von SQL-Anweisungen oder Operatorbefehlen.

- CICS-Anwendung: Wird unter CICS-Steuerung ausgeführt und verwendet verfügbare Ressourcen und Datenquellen in CICS.

- Batchanwendung: Führt Prozesslogik ohne interaktive Kommunikation mit Benutzern aus, um beispielsweise Massenaktualisierungen von Daten durchzuführen oder Berichte aus einer Datenbank zu generieren.

## <a name="optimize-scale-and-throughput-for-azure"></a>Optimieren von Skalierung und Durchsatz für Azure

Allgemein formuliert werden Mainframes zentral hochskaliert, während die Cloud horizontal hochskaliert wird. Um die Skalierung und den Durchsatz von Mainframeanwendungen unter Azure zu optimieren, müssen Sie wissen, wie Mainframes Anwendungen trennen und isolieren können. Ein z/OS-Mainframe verwendet ein Feature namens LPARs (logische Partitionen), um die Ressourcen für eine bestimmte Anwendung auf einer einzelnen Instanz zu isolieren und zu verwalten.

Ein Mainframe kann beispielsweise eine logische Partition (LPAR) für eine CICS-Region mit zugeordneten COBOL-Programmen und eine separate LPAR für DB2 verwenden. Zusätzliche LPARs werden häufig für die Entwicklungs-, Test- und Stagingumgebungen eingesetzt.

In Azure ist es eher üblich, zu diesen Zwecken separate virtuelle Computer zu verwenden. Azure-Architekturen stellen in der Regel VMs für die Logikschicht, einen separaten Satz von VMs für die Datenschicht, einen weiteren Satz für die Entwicklung usw. bereit. Jede Verarbeitungsebene kann über den am besten geeigneten VM-Typ und über die Features für die jeweilige Umgebung optimiert werden.

Darüber hinaus kann jede Ebene auch entsprechende Dienste für die Notfallwiederherstellung bereitstellen. Zum Beispiel ist für Produktions- und Datenbank-VMs möglicherweise eine Wiederherstellung auf heißer oder warmer Ebene erforderlich, während Entwicklungs- und Test-VMs eine Wiederherstellung auf kalter Ebene unterstützen.

Die folgende Abbildung zeigt eine mögliche Azure-Bereitstellung über einen primären und einen sekundären Standort. Am primären Standort werden die Produktions-, Präproduktions- und Test-VMs mit hoher Verfügbarkeit bereitgestellt. Der sekundäre Standort wird für die Sicherung und Notfallwiederherstellung verwendet.

![Eine mögliche Azure-Bereitstellung über einen primären und einen sekundären Standort](../../_images/mainframe-migration/migration-backup-DR.png)

## <a name="perform-a-staged-mainframe-to-azure"></a>Durchführen einer mehrstufigen Mainframemigration zu Azure

Das Verschieben von Lösungen aus einem Mainframe nach Azure kann eine *mehrstufige* Migration umfassen, bei der einige Anwendungen als Erste verschoben werden und andere vorübergehend oder dauerhaft auf dem Mainframe verbleiben. Für diesen Ansatz sind in der Regel Systeme erforderlich, die eine Zusammenarbeit von Anwendungen und Datenbanken zwischen Mainframe und Azure ermöglichen.

In einem häufigen Szenario wird eine Anwendung nach Azure verschoben, während die von der Anwendung verwendeten Daten auf dem Mainframe bleiben. Eine spezielle Software wird verwendet, um den Anwendungen in Azure den Zugriff auf Daten auf dem Mainframe zu ermöglichen. Glücklicherweise stellen zahlreiche Lösungen die Integration zwischen Azure und vorhandenen Mainframe-Umgebungen, die Unterstützung für Hybridszenarien und die Migration über einen längeren Zeitraum bereit. Microsoft-Partner, unabhängige Softwarehersteller und Systemintegratoren können Sie bei diesem Prozess unterstützen.

Eine Möglichkeit ist beispielsweise [Microsoft Host Integration Server](/host-integration-server) (HIS). Diese Lösung stellt die verteilte Architektur relationaler Datenbanken (DRDA) bereit, die erforderlich ist, damit Anwendungen in Azure auf Daten in DB2 auf dem Mainframe zugreifen können. Zu weiteren Optionen für die Integration von Mainframes in Azure zählen Lösungen von IBM, Attunity, Codit und anderen Anbietern sowie Open Source-Optionen.

## <a name="partner-solutions"></a>Partnerlösungen

Wenn Sie eine Mainframemigration in Betracht ziehen, steht Ihnen das Partnerökosystem zur Seite.

Azure bietet eine bewährte, hoch verfügbare und skalierbare Infrastruktur für Systeme, die derzeit auf Mainframes ausgeführt werden. Einige Workloads können relativ einfach migriert werden. Andere Workloads, die auf älterer Systemsoftware wie z.B. CICS und IMS beruhen, können über Partnerlösungen neuen Hosts zugewiesen und im Laufe der Zeit zu Azure migriert werden. Unabhängig von Ihrer Wahl stehen Microsoft und unsere Partner Ihnen zur Verfügung, um Sie bei der Optimierung für Azure und bei der Beibehaltung der Funktionalität von Mainframesystemsoftware zu unterstützen.

Ausführliche Anleitungen zur Auswahl einer Partnerlösung finden Sie in der [Platform Modernization Alliance](https://www.platformmodernization.org/pages/mainframe.aspx).

## <a name="learn-more"></a>Weitere Informationen

Weitere Informationen finden Sie in den folgenden Ressourcen:

- [Erste Schritte mit Azure](/azure)

- [Platform Modernization Alliance: Mainframemigration](https://www.platformmodernization.org/pages/mainframe.aspx)

- [Bereitstellen von IBM DB2 pureScale in Azure](https://azure.microsoft.com/resources/deploy-ibm-db2-purescale-on-azure)

- [Dokumentation zu Host Integration Server (HIS)](/host-integration-server)
