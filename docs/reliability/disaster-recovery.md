---
title: Wiederherstellung von Azure-Anwendungen bei Fehlern oder Notfällen
description: Übersicht der Ansätze zur Notfallwiederherstellung in Azure
author: MikeWasson
ms.date: 04/10/2019
ms.topic: article
ms.service: architecture-center
ms.subservice: cloud-design-principles
ms.openlocfilehash: f00341b06b8092c798d6260317226190cbace66b
ms.sourcegitcommit: 579c39ff4b776704ead17a006bf24cd4cdc65edd
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/17/2019
ms.locfileid: "59642863"
---
# <a name="failure-and-disaster-recovery-for-azure-applications"></a>Wiederherstellung von Azure-Anwendungen bei Fehlern oder Notfällen

*Notfallwiederherstellung* ist der Vorgang des Wiederherstellens der Funktionalität von Anwendungen im Anschluss an einen katastrophalen Verlust.

Ihre Duldung eingeschränkter Funktionalität während eines Notfalls stellt eine Geschäftsentscheidung dar, die sich von Anwendung zu Anwendung unterscheidet. Es kann für einige Anwendungen akzeptabel sein, wenn sie für einen Zeitraum nicht verfügbar oder nur teilweise mit eingeschränkter Funktionalität oder verzögerter Verarbeitung verfügbar sind. Bei anderen Anwendungen ist jegliche Einschränkung der Funktionalität völlig inakzeptabel.

## <a name="disaster-recovery-plan"></a>Plan zur Notfallwiederherstellung

Beginnen Sie, indem Sie einen Wiederherstellungsplan erstellen. Der Plan gilt als abgeschlossen, nachdem er vollständig getestet wurde. Beziehen Sie die Personen, Prozesse und Anwendungen ein, die zum Wiederherstellen der Funktionalität im Rahmen des SLAs (Service-Level Agreement) erforderlich sind, das Sie für Ihre Kunden definiert haben.

Betrachten Sie beim Erstellen und Testen Ihres Plans für die Notfallwiederherstellung die folgenden Aspekte:

- Schließen Sie in Ihren Plan den Vorgang der Kontaktaufnahme mit dem Support und der Eskalation von Problemen mit ein. Diese Informationen helfen Ihnen, längere Ausfallzeiten bei der ersten Ausarbeitung des Wiederherstellungsprozesses zu vermeiden.
- Werten Sie die geschäftlichen Auswirkungen von Anwendungsausfällen aus.
- Wählen Sie für unternehmenskritische Anwendungen eine regionsübergreifende Wiederherstellungsarchitektur.
- Identifizieren Sie einen bestimmten Besitzer des Plans für die Notfallwiederherstellung, einschließlich Automation und Testing.
- Dokumentieren Sie den Prozess, insbesondere alle manuellen Schritte.
- Automatisieren Sie den Prozess so weit wie möglich.
- Richten Sie eine Sicherungsstrategie für alle Verweis- und Transaktionsdaten ein, und testen Sie die Wiederherstellung dieser Sicherungen regelmäßig.
- Richten Sie Warnungen für den Stapel der Azure-Dienste ein, die von Ihrer Anwendung genutzt werden.
- Schulen Sie das Betriebspersonal in der Ausführung des Plans.
- Führen Sie regelmäßig Notfallsimulationen durch, um den Plan zu überprüfen und zu verbessern.

Wenn Sie virtuelle Computer (VMs) mit [Azure Site Recovery](/azure/site-recovery/) replizieren, erstellen Sie einen vollständig automatisierten Wiederherstellungsplan, um ein Failover der gesamten Anwendung ausführen zu können.

## <a name="manual-responses"></a>Manuelle Reaktionen

Zwar stellt Automation das Ideal dar, für einige Strategien zur Notfallwiederherstellung sind jedoch manuelle Reaktionen erforderlich.

### <a name="alerts"></a>Alerts

Überwachen Sie Ihre Anwendung auf Warnsignale, die proaktive Eingriffe nötig machen. Wenn beispielsweise Ihre Anwendung von Azure SQL-Datenbank oder Azure Cosmos DB ständig gedrosselt wird, müssen Sie ggf. Ihre Datenbankkapazität erhöhen oder Ihre Abfragen optimieren. Ihre Telemetrie sollte auch dann eine Warnung auslösen, wenn die Anwendung Drosselungsfehler transparent behandelt, damit Sie Maßnahmen ergreifen können.

Es empfiehlt sich, für Dienstgrenzwerte und Kontingentschwellenwerte Warnungen in den Azure-Ressourcenmetriken und Diagnoseprotokollen zu konfigurieren. Richten Sie nach Möglichkeit Warnungen für Metriken ein, die eine geringere Latenz als Diagnoseprotokolle aufweisen.

Mithilfe von  [Resource Health](/azure/service-health/resource-health-checks-resource-types) stellt Azure eine Reihe von integrierten Überprüfungen des Integritätsstatus zur Verfügung, die Sie bei der Diagnose von Drosselungsproblemen bei Azure-Diensten unterstützen können.

### <a name="failover"></a>Failover

Konfigurieren Sie für jede Azure-Anwendung und ihre Azure-Dienste eine Strategie zur Notfallwiederherstellung. Akzeptable Bereitstellungsstrategien zur Unterstützung der Notfallwiederherstellung können sich je nach den für alle Komponenten der einzelnen Anwendungen erforderlichen SLAs unterscheiden.  

Azure bietet in vielen Azure-Diensten verschiedene Funktionen für ein manuelles Failover, z.B.  [Georeplikation mithilfe von Redis Cache](/azure/azure-cache-for-redis/cache-how-to-geo-replication), oder ein automatisches Failover, etwa in Form von  [SQL-Autofailovergruppen](/azure/sql-database/sql-database-auto-failover-group). Beispiel: 

- Für Anwendungen, die hauptsächlich virtuelle Computer verwenden, können Sie Azure Site Recovery für die Web- und Logikschichten verwenden. Weitere Informationen finden Sie unter [Architektur der Notfallwiederherstellung von Azure zu Azure](/azure/site-recovery/azure-to-azure-architecture). Verwenden Sie für SQL Server auf VMs [SQL Server Always On-Verfügbarkeitsgruppen](/azure/virtual-machines/windows/sql/virtual-machines-windows-portal-sql-availability-group-dr).
- Für eine Anwendung, die App Service und Azure SQL-Datenbank verwendet, können Sie einen App Service-Plan eines kleineren Tarifs verwenden, der in der sekundären Region konfiguriert ist und beim Eintreten eines Failovers automatisch skaliert. Verwenden Sie Failovergruppen für die Datenbankschicht.

In beiden Szenarien ermöglicht ein  [Azure Traffic Manager](/azure/traffic-manager/traffic-manager-overview) -Profil das automatische regionsübergreifende Failover. [Load Balancer](/azure/load-balancer/load-balancer-overview) oder [Anwendungsgateways](/azure/application-gateway/overview) sollten in der sekundären Region eingerichtet werden, um beim Eintreten des Failovers schnellere Verfügbarkeit zu unterstützen.

### <a name="operational-readiness-testing"></a>Testen der Betriebsbereitschaft

Führen Sie einen Test auf Betriebsbereitschaft für das Failover in die sekundäre Region und für das Failback in die primäre Region durch. Viele Azure-Dienste unterstützen manuelle Failover oder Testfailover für Übungen zur Notfallwiederherstellung. Alternativ können Sie einen Ausfall simulieren, indem Sie Azure-Dienste herunterfahren oder entfernen.

## <a name="application-failure"></a>Anwendungsausfall

Anwendungsausfälle sind entweder behebbar oder nicht behebbar. Ein behebbarer Fehler lässt sich entschärfen, nicht behebbare Fehler führen jedoch zum Absturz der Anwendung.

- Einige Ausfälle können durch automatische Fehlerbehandlung und das Einleiten alternativer Aktionen transparent angegangen werden. Beispielsweise behandelt Traffic Manager Ausfälle automatisch, die von der zugrunde liegenden Hardware oder Betriebssystemsoftware auf dem virtuellen Hostcomputer verursacht werden.
- Bei einigen Fehlern kann die Anwendung die Verarbeitung von Benutzeranforderungen mit eingeschränkter Funktionalität fortsetzen.
- Ernsthaftere Dienstunterbrechungen können aber dazu führen, dass die Anwendung nicht verfügbar ist.

Ein gut ausgelegtes System trennt die Zuständigkeiten auf Dienstebene – zur Entwurfszeit und zur Laufzeit. Durch diese Trennung wird verhindert, dass eine Unterbrechung bei einem abhängigen Dienst die gesamte Anwendung zum Absturz bringt. Betrachten Sie beispielsweise eine Web-Commerce-Anwendung mit den folgenden Modulen:

![LINK ZU ART HINZUFÜGEN](./_images/disaster-recovery.png)

Wenn die Datenbank zum Hosten von Bestellungen ausfällt, kann der Dienst für die Auftragsverarbeitung Verkaufstransaktionen nicht mehr verarbeiten. Je nach Architektur kann es unmöglich sein, die Dienstkomponenten „Übermittlung der Bestellung“ und „Auftragsverarbeitung“ weiter zu nutzen. Wenn Produktdaten jedoch an einem anderen Speicherort gespeichert sind, ist der Produktkatalog noch verfügbar, auch wenn andere Teile der Anwendung möglicherweise nicht mehr verfügbar sind.

Es liegt an Ihnen, festzulegen, wie die Anwendung Benutzer über temporäre Probleme informiert, und entsprechende Benachrichtigungen in das System zu integrieren. Im vorstehenden Beispiel kann die Anwendung das Anzeigen von Produkten und das Hinzufügen zu einem Einkaufswagen ermöglichen. Wenn der Benutzer allerdings versucht, einen Kauf zu tätigen, sollte ihn die Anwendung benachrichtigen, dass die Bestellfunktionalität vorübergehend nicht verfügbar ist. Dies ist für den Kunden zwar nicht ideal, doch verhindert dieser Ansatz eine anwendungsweite Dienstunterbrechung.

## <a name="data-corruption-and-restoration"></a>Beschädigung und Wiederherstellung von Daten

Wenn ein Datenspeicher ausfällt, kann es zu Inkonsistenzen bei den Daten kommen, sobald der Speicher wieder verfügbar ist. Dies gilt besonders, wenn die Daten repliziert wurden. Ein Verständnis von RTO (Recovery Time Objective) und RPO (Recovery Point Objective) replizierter Datenspeicher können Ihnen helfen, die Menge der verlorenen Daten vorherzusagen.

Um zu verstehen, ob das regionsübergreifende Failover manuell oder von Microsoft eingeleitet wird, lesen Sie die Azure-Dienst-SLAs. Bei Diensten, die keine SLAs für regionsübergreifendes Failover aufweisen, entscheidet normalerweise Microsoft, wann das Failover erfolgt, und üblicherweise wird die Wiederherstellung der Daten in der primären Region priorisiert. Wenn die Daten in der primären Region als nicht wiederherstellbar angesehen werden, führt Microsoft das Failover zur sekundären Region aus.

### <a name="restoring-data-from-backups"></a>Wiederherstellen von Daten aus Sicherungen

Sicherungen schützen Sie vor dem Verlust einer Komponente der Anwendung aufgrund von versehentlichem Löschen oder Beschädigung der Daten. Sie bewahren eine funktionsfähige Version der Komponente von einem früheren Zeitpunkt auf, die Sie zu deren Wiederherstellung verwenden können.

Strategien zur Notfallwiederherstellung stellen keinen Ersatz für Sicherungen dar, aber regelmäßige Sicherungen von Anwendungsdaten unterstützen einige Szenarien zur Notfallwiederherstellung. Ihre Entscheidungen zur Speicherung von Sicherungen sollten auf Ihrer Strategie zur Notfallwiederherstellung aufbauen.

Die Ausführungshäufigkeit des Sicherungsprozesses bestimmt Ihr RPO. Wenn Sie beispielsweise einmal pro Stunde Datensicherung ausführen und ein Notfall zwei Minuten vor der Sicherung auftritt, verlieren Sie die Daten der letzten 58 Minuten. Ihr Plan zur Notfallwiederherstellung sollte Maßnahmen zum Umgang mit verlorenen Daten beinhalten.

Es ist üblich, dass Daten in einem Datenspeicher auf Daten in einem anderen Datenspeicher verweisen. Stellen Sie sich beispielsweise eine SQL-Datenbank vor, die über eine Spalte mit einem Link zu einem Blob in Azure Storage verfügt. Wenn Sicherungen nicht gleichzeitig erfolgen, enthält die Datenbank möglicherweise einen Zeiger auf ein Blob, das vor dem Ausfall nicht gesichert wurde. Die Anwendung oder der Notfallwiederherstellungsplan muss Prozesse implementieren, damit diese Inkonsistenz nach einer Wiederherstellung behandelt wird.

> [!NOTE]
> In einigen Szenarien, beispielsweise bei VMs, die mithilfe von [Azure Backup](/azure/backup/backup-azure-vms-first-look-arm) gesichert wurden, kann die Wiederherstellung nur aus einer Sicherung in der gleichen Region erfolgen. Andere Azure-Dienste, wie etwa [Azure Cache for Redis](/azure/azure-cache-for-redis/cache-how-to-geo-replication), bieten georeplizierte Sicherungen, die Sie zum regionsübergreifenden Wiederherstellen von Diensten verwenden können.

### <a name="azure-storage-and-azure-sql-database"></a>Azure Storage und Azure SQL-Datenbank

Azure speichert Daten aus Azure Storage und SQL-Datenbank automatisch dreimal innerhalb verschiedener Fehlerdomänen in der gleichen Region. Wenn Georeplikation verwendet wird, werden die Daten drei weitere Male in einer anderen Region gespeichert. Wenn die Daten jedoch in der primären Kopie beschädigt oder gelöscht werden (etwa aufgrund eines Benutzerfehlers), werden diese Änderungen in die anderen Kopien repliziert.

Ihnen stehen zwei Optionen zum Verwalten potenzieller Datenbeschädigung oder -löschung zur Verfügung:

- **Erstellen einer benutzerdefinierten Sicherungsstrategie.** Sie können Ihre Daten in Azure oder lokal speichern, abhängig von Ihren Geschäftsanforderungen und Ihren Governancevorschriften.
- **Die Option zur Zeitpunktwiederherstellung verwenden**, um eine SQL-Datenbank wiederherzustellen.

#### <a name="azure-storage-recovery"></a>Azure Storage-Wiederherstellung

Sie können einen benutzerdefinierten Sicherungsprozess für Azure Storage entwickeln oder eins der vielen Sicherungstools von Drittanbietern verwenden.

Azure Storage bietet Datenresilienz mithilfe automatischer Replikate, das verhindert jedoch keine Beschädigung von Daten durch Anwendungscode oder Benutzer. Um Datengenauigkeit nach Anwendungs- oder Benutzerfehlern zu gewährleisten, sind weitergehende Methoden erforderlich – beispielsweise das Kopieren der Daten an einen sekundären Speicherort mit einem Überwachungsprotokoll. Sie haben mehrere Möglichkeiten:

- [**Blockblobs.**](/rest/api/storageservices/understanding-block-blobs--append-blobs--and-page-blobs) Erstellen Sie eine Zeitpunkt-Momentaufnahme von jedem Blockblob. Bei jeder Momentaufnahme wird Ihnen nur die erforderliche Speicherkapazität berechnet, die Sie benötigen, um die Veränderungen innerhalb des Blobs seit der vorhergehenden Momentaufnahme zu speichern. Die Momentaufnahmen hängen von dem ursprünglichen Blob ab, daher empfehlen wir das Kopieren in einen weiteren Blob oder sogar ein weiteres Speicherkonto. Dieser Ansatz stellt den Schutz der Sicherungsdaten vor versehentlichem Löschen sicher. Verwenden Sie [AzCopy](/azure/storage/common/storage-use-azcopy) oder [Azure PowerShell](/azure/storage/common/storage-powershell-guide-full), um die Blobs in ein anderes Speicherkonto zu kopieren.

    Weitere Informationen finden Sie unter [Erstellen einer Momentaufnahme eines Blobs](/rest/api/storageservices/creating-a-snapshot-of-a-blob).

- [**Azure Files.**](/azure/storage/files/storage-files-introduction) Verwenden Sie [Freigabemomentaufnahmen](/azure/storage/files/storage-snapshots-files), AzCopy oder PowerShell, um die Dateien in ein anderes Speicherkonto zu kopieren.
- [**Azure-Tabellenspeicher.**](/azure/storage/tables/table-storage-overview) Verwenden Sie AzCopy, um die Tabellendaten in ein anderes Speicherkonto in einer anderen Region zu exportieren.

#### <a name="sql-database-recovery"></a>SQL-Datenbank-Wiederherstellung

Um Ihr Unternehmen vor Datenverlusten zu schützen, führt SQL-Datenbank automatisch eine Kombination aus wöchentlichen vollständigen Datenbanksicherungen, stündlichen differenziellen Datenbanksicherungen sowie Transaktionsprotokollsicherungen im Abstand von 5–10 Minuten durch. Verwenden Sie für die SQL-Datenbanktarife Basic, Standard und Premium Zeitpunktwiederherstellung, um eine Datenbank zu einem früheren Zeitpunkt wiederherzustellen. Weitere Informationen finden Sie in den folgenden Artikeln:

- [Wiederherstellen einer Azure SQL-Datenbank mit automatisierten Datenbanksicherungen](/azure/sql-database/sql-database-recovery-using-backups)
- [Übersicht über die Geschäftskontinuität mit Azure SQL-Datenbank](/azure/sql-database/sql-database-business-continuity)

Eine weitere Option besteht in der Verwendung der aktiven Georeplikation für SQL-Datenbank, die automatisch Änderungen an Datenbanken in sekundäre Datenbanken in der gleichen oder einer anderen Azure-Region repliziert. Weitere Informationen finden Sie unter [Erstellen und Verwenden der aktiven Georeplikation](/azure/sql-database/sql-database-active-geo-replication).

Sie können auch einen stärker manuellen Ansatz für die Sicherung und Wiederherstellung verwenden:

- Verwenden Sie den Befehl **DATABASE COPY**, um eine Kopie der Datenbank mit Transaktionskonsistenz zu erstellen.
- Verwenden Sie den Import-/Export-Dienst, der den Export von Datenbanken in BACPAC-Dateien (komprimierte Dateien, die Ihr Datenbankschema und die dazugehörigen Daten enthalten) unterstützt, die in Azure Blob Storage gespeichert werden. Zum Schutz vor einer regionsweiten Dienstunterbrechung kopieren Sie die BACPAC-Dateien in eine alternative Region.

### <a name="sql-server-on-vms"></a>SQL Server auf virtuellen Computern

Für SQL Server auf virtuellen Computern bestehen zwei Optionen: herkömmliche Sicherungen und Protokollversand.

- Mit herkömmlichen Sicherungen ist die Wiederherstellung auf einen bestimmten Zeitpunkt möglich – allerdings ist der Wiederherstellungsprozess langsam. Um herkömmliche Sicherungen wiederherzustellen, müssen Sie mit einer vollständigen Erstsicherung beginnen und dann alle inkrementellen Sicherungen anwenden.
- Sie können eine Protokollversandsitzung so konfigurieren, dass die Wiederherstellung von Protokollsicherungen verzögert wird. In dem entstandenen Zeitfenster können Fehler im primären Replikat behoben werden.

### <a name="azure-database-for-mysql-and-azure-database-for-postgresql"></a>Azure Database for MySQL und Azure Database for PostgreSQL

In Azure Database for MySQL oder Azure Database for PostgreSQL erstellt der Datenbankdienst automatisch alle fünf Minuten eine Sicherung. Sie können diese automatisierten Sicherungen verwenden, um den Server und seine Datenbanken auf einem neuen Server auf einen früheren Zeitpunkt wiederherzustellen. Weitere Informationen finden Sie unter

- [Sichern und Wiederherstellen eines Servers in Azure Database for MySQL mit dem Azure-Portal](/azure/mysql/howto-restore-server-portal)
- [Sichern und Wiederherstellen eines Servers in Azure Database for PostgreSQL mit dem Azure-Portal](/azure/postgresql/howto-restore-server-portal)

### <a name="azure-cosmos-db"></a>Azure Cosmos DB

Cosmos DB erstellt in regelmäßigen Abständen automatisch eine Sicherung. Sicherungen werden separat in einem anderen Speicherdienst gespeichert und zum Schutz vor regionalen Ausfällen global repliziert. Falls Sie Ihre Datenbank oder -sammlung versehentlich löschen, können Sie ein Supportticket anfordern oder den Azure Support bitten, die Daten aus der letzten automatischen Sicherung wiederherzustellen. Weitere Informationen finden Sie unter [Automatische Onlinesicherung und -wiederherstellung bei Bedarf mit Azure Cosmos DB](/azure/cosmos-db/online-backup-and-restore).

### <a name="azure-virtual-machines"></a>Azure Virtual Machines

Verwenden Sie [Azure Backup](/azure/backup/), um virtuelle Azure-Computer vor Anwendungsfehlern oder versehentlichem Löschen zu schützen. Die erstellten Sicherungen sind über mehrere VM-Datenträger hinweg konsistent. Darüber hinaus kann der Azure-Sicherungstresor über Regionen hinweg repliziert werden, um bei Ausfall einer Region Wiederherstellung zu unterstützen.

## <a name="network-outage"></a>Netzwerkausfall

Wenn auf Teile des Azure-Netzwerks nicht zugegriffen werden kann, können Sie möglicherweise nicht auf Ihre Anwendung oder Ihre Daten zugreifen. In dieser Situation empfiehlt es sich, die Strategie zur Notfallwiederherstellung für die Ausführung der meisten Anwendungen mit eingeschränkter Funktionalität auszulegen.

Falls eingeschränkte Funktionalität nicht in Frage kommt, sind die verbleibenden Optionen Ausfallzeiten der Anwendung oder ein Failover in eine andere Region.

In einem Szenario mit eingeschränkter Funktionalität:

- Wenn Ihre Anwendung aufgrund eines Azure-Netzwerkausfalls nicht auf die Daten zugreifen kann, können Sie möglicherweise mithilfe zwischengespeicherter Daten bei eingeschränkter Anwendungsfunktionalität lokal arbeiten.
- Möglicherweise können Sie Daten an einem alternativen Speicherort speichern, bis die Verbindung wiederhergestellt wurde.

## <a name="dependent-service-failure"></a>Ausfall abhängiger Dienste

Für jeden abhängigen Dienst sollten Sie die Auswirkungen einer Dienstunterbrechung und die Reaktion der Anwendung darauf kennen. Viele Dienste enthalten Funktionen, die Resilienz und Verfügbarkeit unterstützen, so dass Ihr Plan für die Notfallwiederherstellung sich durch die unabhängige Bewertung jedes einzelnen Diensts wahrscheinlich verbessert. Beispielsweise unterstützt Azure Event Hubs das [Failover](/azure/event-hubs/event-hubs-geo-dr#setup-and-failover-flow) auf den sekundären Namespace.

## <a name="region-wide-service-disruptions"></a>Regionsweite Dienstunterbrechungen

Viele Fehler lassen sich innerhalb derselben Azure-Region beherrschen. Im unwahrscheinlichen Fall einer regionsweiten Dienstunterbrechung sind die lokal redundanten Kopien Ihrer Daten aber nicht verfügbar. Wenn Sie die Georeplikation aktiviert haben, sind drei zusätzliche Kopien der Blobs und Tabellen in einer anderen Region vorhanden. Wenn Microsoft die Region als verloren deklariert, ordnet Azure alle DNS-Einträge der sekundären Region neu zu.

> [!NOTE]
> Dieser Prozess wird nur bei regionsweiten Dienstunterbrechungen ausgeführt und unterliegt nicht Ihrer Kontrolle. Erwägen Sie die Verwendung von [Azure Site Recovery](/azure/site-recovery/), um bessere RPO- und RTO-Werte zu erzielen. Beim Einsatz von Site Recovery entscheiden Sie, was ein akzeptabler Ausfall ist und wann das Failover auf die replizierten VMs erfolgen soll.

Ihre Reaktion auf eine regionsweite Dienstunterbrechung hängt von Ihrer Bereitstellung und Ihrem Plan für die Notfallwiederherstellung ab.

- Als Strategie zur Kostenkontrolle kann es für nicht kritische Anwendungen, für die keine garantierte Wiederherstellungszeit erforderlich ist, sinnvoll sein, eine erneute Bereitstellung in einer anderen Region vorzunehmen.
- Wechseln Sie für Anwendungen, die in einer anderen Region mit bereitgestellten Rollen gehostet sind, die aber Datenverkehr nicht regionsübergreifend verteilen (*Aktiv-/Passiv-Bereitstellung*), zum sekundären gehosteten Dienst in der alternativen Region.
- Routen Sie für Anwendungen, die eine sekundäre Bereitstellung in einer anderen Region im vollen Maßstab aufweisen (*Aktiv-/Aktiv-Bereitstellung*), den Datenverkehr in die betreffende Region um.

Weitere Informationen zur Wiederherstellung nach einer regionsweiten Dienstunterbrechung finden Sie unter [Wiederherstellung nach einer regionsweiten Dienstunterbrechung](../resiliency/recovery-loss-azure-region.md).

### <a name="vm-recovery"></a>VM-Wiederherstellung

Planen Sie für kritische Apps die Wiederherstellung von VMs im Fall einer regionsweiten Dienstunterbrechung.

- Verwenden Sie Azure Backup oder eine andere Sicherungsmethode zum Erstellen von anwendungskonsistenten, regionsübergreifenden Sicherungen. (Die Replikation des Sicherungstresors muss zum Zeitpunkt der Erstellung konfiguriert werden.)
- Verwenden Sie Site Recovery für die regionsübergreifende Replikation, um das Failover von Anwendungen und Failovertests mit nur einem Klick zu ermöglichen.
- Verwenden Sie Traffic Manager, um das Failover von Benutzerdatenverkehr in eine andere Region zu automatisieren.

Weitere Informationen finden Sie unter [Wiederherstellung nach einer regionsweiten Dienstunterbrechung, virtuelle Computer](../resiliency/recovery-loss-azure-region.md#virtual-machines).

### <a name="storage-recovery"></a>Speicherwiederherstellung

Um Ihren Speicher im Fall einer regionsweiten Dienstunterbrechung zu schützen:

- Verwenden Sie georedundanten Speicher.
- Wissen Sie, wo die Georeplikation Ihres Speichers erfolgt. Dies wirkt sich darauf aus, wo Sie weitere Instanzen Ihrer Daten bereitstellen, für die regionale Affinität mit Ihrem Speicher erforderlich ist.
- Überprüfen Sie die Daten nach dem Failover auf Konsistenz, und stellen Sie sie bei Bedarf aus einer Sicherung wieder her.

Weitere Informationen finden Sie unter [Entwerfen hochverfügbarer Anwendungen mithilfe von RA-GRS](/azure/storage/common/storage-designing-ha-apps-with-ragrs).

### <a name="sql-database-and-sql-server"></a>SQL-Datenbank und SQL SQL Server

Azure SQL-Datenbank stellt zwei Arten von Wiederherstellung bereit:

- Verwenden Sie Geowiederherstellung, um eine Datenbank aus einer Sicherungskopie in einer anderen Region wiederherzustellen. Weitere Informationen finden Sie unter [Wiederherstellen einer Azure SQL-Datenbank mit automatisierten Datenbanksicherungen](/azure/sql-database/sql-database-recovery-using-backups).
- Verwenden Sie aktive Georeplikation für das Failover zu einer sekundären Datenbank. Weitere Informationen finden Sie unter [Erstellen und Verwenden der aktiven Georeplikation](/azure/sql-database/sql-database-active-geo-replication).

Informationen zu SQL Server bei der Ausführung auf virtuellen Computern finden Sie unter [Hochverfügbarkeit und Notfallwiederherstellung für SQL Server auf virtuellen Azure-Computern](/azure/virtual-machines/windows/sql/virtual-machines-windows-sql-high-availability-dr/).

## <a name="service-specific-guidance"></a>Dienstspezifischer Leitfaden

In den folgenden Artikeln wird die Notfallwiederherstellung für spezifische Azure-Dienste beschrieben:

| Dienst                       | Artikel                                                                                                                                                                                       |
|-------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Azure Database for MySQL      | [Übersicht über die Geschäftskontinuität mit Azure Database for MySQL](/azure/mysql/concepts-business-continuity)                                                  |
| Azure Database for PostgreSQL | [Übersicht über die Geschäftskontinuität mit Azure Database for PostgreSQL](/azure/postgresql/concepts-business-continuity)                                        |
| Azure Cloud Services          | [Vorgehensweise bei einer Azure-Dienstunterbrechung mit Auswirkungen auf Azure Cloud Services](/azure/cloud-services/cloud-services-disaster-recovery-guidance) |
| Cosmos DB                     | [Hochverfügbarkeit mit Azure Cosmos DB](/azure/cosmos-db/high-availability)                                                                                |
| Azure Key Vault               | [Azure Key Vault: Verfügbarkeit und Redundanz](/azure/key-vault/key-vault-disaster-recovery-guidance)                                                        |
| Azure Storage                 | [Notfallwiederherstellung und Speicherkontofailover (Vorschau) in Azure Storage](/azure/storage/common/storage-disaster-recovery-guidance)                       |
| SQL-Datenbank                  | [Wiederherstellen einer Azure SQL-Datenbank oder eines Failovers in einer sekundären Region](/azure/sql-database/sql-database-disaster-recovery)                                       |
| Virtual Machines              | [Vorgehen bei einer Azure-Dienstunterbrechung mit Auswirkungen auf die Azure Cloud](/azure/cloud-services/cloud-services-disaster-recovery-guidance)               |
| Virtuelles Azure-Netzwerk         | [Virtuelles Netzwerk: Geschäftskontinuität](/azure/virtual-network/virtual-network-disaster-recovery-guidance)                                                  |

## <a name="next-steps"></a>Nächste Schritte

- [Wiederherstellung nach Datenbeschädigung oder unbeabsichtigtem Löschen](../resiliency/recovery-data-corruption.md)
- [Wiederherstellung nach einer regionsweiten Dienstunterbrechung](../resiliency/recovery-loss-azure-region.md)