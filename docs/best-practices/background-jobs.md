---
title: Anleitungen für Hintergrundaufträge
titleSuffix: Best practices for cloud applications
description: Anleitungen für Hintergrundaufgaben, die unabhängig von der Benutzeroberfläche ausgeführt werden.
author: dragon119
ms.date: 11/05/2018
ms.topic: best-practice
ms.service: architecture-center
ms.subservice: cloud-fundamentals
ms.custom: seodec18
ms.openlocfilehash: 4b96c19dd8613a941a7408e1b99945d5fa0f5364
ms.sourcegitcommit: c053e6edb429299a0ad9b327888d596c48859d4a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/20/2019
ms.locfileid: "58248965"
---
# <a name="background-jobs"></a>Hintergrundaufträge

Viele Arten von Anwendungen erfordern Hintergrundaufgaben, die unabhängig von der Benutzeroberfläche (UI) ausgeführt werden. Beispiele hierfür sind Batchaufträge, rechenintensive Aufgaben und langwierige Prozesse wie Workflows. Hintergrundaufträge können ohne Benutzerinteraktion ausgeführt werden. Die Anwendung kann den Auftrag starten und dann mit der Verarbeitung interaktiver Benutzeranforderungen fortfahren. Damit kann die Arbeitsauslastung der Anwendungsbenutzeroberfläche minimiert werden, was zu einer höheren Verfügbarkeit und zu kürzeren interaktiven Antwortzeiten beitragen kann.

Wenn eine Anwendung z. B. Miniaturansichten von Bildern, die von den Benutzern hochgeladen wurden, generieren muss, kann dies als Hintergrundauftrag ausgeführt werden. Die fertiggestellte Miniaturansicht kann dann im Speicher abgelegt werden, ohne dass der Benutzer auf den Abschluss des Prozesses warten muss. Auf die gleiche Weise kann ein Benutzer, der eine Bestellung aufgibt, einen Hintergrund-Workflow zur Verarbeitung der Bestellung initiieren, während die Benutzeroberfläche zulässt, dass der Benutzer die Web-App weiterhin durchsucht. Wenn der Hintergrundauftrag abgeschlossen ist, können die gespeicherten Bestelldaten aktualisiert werden, und eine E-Mail kann zur Bestätigung der Bestellung an den Benutzer gesendet werden.

Bei der Überlegung, ob eine Aufgabe als Hintergrundauftrag implementiert werden soll, ist das Hauptkriterium, ob die Aufgabe ohne Benutzerinteraktion ausgeführt werden kann und ohne dass die Benutzeroberfläche auf den Abschluss des Auftrags warten muss. Aufgaben, bei denen der Benutzer oder die Benutzeroberfläche warten muss, bis sie abgeschlossen sind, eignen sich unter Umständen nicht als Hintergrundaufträge.

## <a name="types-of-background-jobs"></a>Typen von Hintergrundaufträgen

Hintergrundaufträge besitzen in der Regel mindestens einen der folgenden Auftragstypen:

- CPU-intensive Aufträge, z. B. mathematische Berechnungen oder Strukturmodellanalysen.
- E/A-intensive Aufträge, z. B. die Ausführung einer Reihe von Speichertransaktionen oder die Indizierung von Dateien.
- Batchaufträge, z. B. nächtliche Datenaktualisierungen oder eine geplante Verarbeitung.
- Lang andauernde Workflows, z. B. Auftragserfüllung oder Bereitstellung von Diensten und Systemen.
- Verarbeitung vertraulicher Daten, wobei die Aufgabe zur Verarbeitung an einen sichereren Ort übergeben wird. Beispiel: Sie möchten vertrauliche Daten nicht in einer Web-App verarbeiten. Stattdessen könnten Sie ein Muster wie [Gatekeeper](../patterns/gatekeeper.md) verwenden, um die Daten an einen isolierten Hintergrundprozess zu übertragen, der auf geschützten Speicher zugreifen kann.

## <a name="triggers"></a>Trigger

Hintergrundaufträge können auf verschiedene Weise initiiert werden. Sie gehören zu einer der folgenden Kategorien:

- [**Ereignisgesteuerte Trigger**](#event-driven-triggers). Die Aufgabe wird in Reaktion auf ein Ereignis, in der Regel eine Benutzeraktion oder ein Schritt in einem Workflow, gestartet.
- [**Zeitplangesteuerte Trigger**](#schedule-driven-triggers). Die Aufgabe wird nach einem Zeitplan basierend auf einem Timer aufgerufen. Es kann sich dabei um einen periodischen Zeitplan oder einen in der Zukunft geplanten einmaligen Aufruf handeln.

### <a name="event-driven-triggers"></a>Ereignisgesteuerte Trigger

Bei einem ereignisgesteuerten Aufruf wird die Hintergrundaufgabe über einen Trigger gestartet. Beispiele für die Verwendung ereignisgesteuerter Trigger:

- Die Benutzeroberfläche oder ein anderer Auftrag fügt eine Nachricht in eine Warteschlange ein. Die Nachricht enthält Daten über eine Aktion, die ausgeführt wurde, wie z. B. die Bestellung des Benutzers. Die Hintergrundaufgabe überwacht diese Warteschlange und erkennt den Eingang einer neuen Nachricht. Sie liest die Nachricht und verwendet die darin enthaltenen Daten als Eingabe für den Hintergrundauftrag.
- Die Benutzeroberfläche oder ein anderer Auftrag speichert oder aktualisiert einen Wert im Speicher. Die Hintergrundaufgabe überwacht den Speicher und erkennt Änderungen. Sie liest die Daten und verwendet sie als Eingabe für den Hintergrundauftrag.
- Die Benutzeroberfläche oder ein anderer Auftrag sendet eine Anforderung an einen Endpunkt, z. B. einen HTTPS-URI oder eine API, die als Webdienst verfügbar gemacht wurde. Anschließend werden die Daten, die zum Abschließen der Hintergrundaufgabe erforderlich sind, als Teil der Anforderung übergeben. Der Endpunkt oder Webdienst ruft die Hintergrundaufgabe auf, die die Daten als Eingabe verwendet.

Typische Beispiele für Aufgaben, die sich für einen ereignisgesteuerte Aufruf eignen, sind unter anderem Bildverarbeitung, Workflows, Senden von Informationen an Remotedienste, Senden von E-Mails, Bereitstellen neuer Benutzer in mehrinstanzenfähigen Anwendungen.

### <a name="schedule-driven-triggers"></a>Zeitplangesteuerte Trigger

Bei einem zeitplangesteuerten Aufruf wird die Hintergrundaufgabe über einen Timer gestartet. Beispiele für die Verwendung zeitplangesteuerter Trigger:

- Ein Timer, der in der Anwendung oder als Teil des Betriebssystems der Anwendung lokal ausgeführt wird, ruft in regelmäßigen Abständen eine Hintergrundaufgabe auf.
- Ein Timer, der in einer anderen Anwendung ausgeführt wird, oder ein Timerdienst wie Azure Scheduler sendet in regelmäßigen Abständen eine Anforderung an eine API oder einen Webdienst. Die API oder der Webdienst ruft die Hintergrundaufgabe auf.
- Ein separater Prozess oder eine Anwendung startet einen Timer, der bewirkt, dass die Hintergrundaufgabe erst nach einer angegebenen Zeitverzögerung oder zu einem bestimmten Zeitpunkt aufgerufen wird.

Typische Beispiele für Aufgaben, die sich für einen zeitplangesteuerten Aufruf eignen, sind unter anderem Batchverarbeitungsroutinen (z. B. das Aktualisieren von Listen relevanter Produkte für Benutzer basierend auf dem aktuellen Verhalten der Benutzer), routinemäßige Datenverarbeitungsaufgaben (wie das Aktualisieren von Indizes oder das Generieren kumulierter Ergebnisse), Datenanalysen für tägliche Berichte, Bereinigung der Datenaufbewahrung und Datenkonsistenzprüfungen.

Bei Verwendung einer zeitplangesteuerten Aufgabe, die als einzelne Instanz ausgeführt werden muss, müssen Sie Folgendes beachten:

- Wenn die Compute-Instanz, welche die Aufgabenplanung ausführt (z. B. virtuelle Computer mit Windows-Aufgabenplanung), skaliert wird, werden mehrere Instanzen der Aufgabenplanung ausgeführt. Diese können mehrere Instanzen der Aufgabe starten.
- Falls die Ausführungszeit der Aufgaben länger als der Zeitraum zwischen geplanten Ereignissen ist, kann die Aufgabenplanung eine weitere Instanz der Aufgabe starten, während die vorherige Aufgabe noch ausgeführt wird.

## <a name="returning-results"></a>Rückgabe von Ergebnissen

Hintergrundaufträge werden asynchron in einem separaten Prozess oder sogar an einem anderen Ort als die Benutzeroberfläche oder der Prozess ausgeführt, die bzw. der die Hintergrundaufgabe aufgerufen hat. Im Idealfall sind Hintergrundaufgaben Vorgänge, deren Ausführungsstatus keine Auswirkungen auf die Benutzeroberfläche oder den aufrufenden Prozess hat. Dies bedeutet, dass der aufrufende Prozess nicht auf den Abschluss der Aufgaben wartet. Daher kann er nicht automatisch erkennen, ob die Aufgabe beendet ist.

Wenn eine Hintergrundaufgabe mit der aufrufenden Aufgabe kommunizieren soll, um ihre Verarbeitung oder ihren Abschluss anzuzeigen, müssen Sie einen entsprechenden Mechanismus implementieren. Hier einige Beispiele:

- Schreiben Sie einen Statusindikatorwert in den Speicher, der für die Benutzeroberfläche oder die aufrufende Aufgabe zugänglich ist, um diesen Wert bei Bedarf überwachen oder überprüfen zu können. Andere Daten, die die Hintergrundaufgabe an den Aufrufer zurückgeben muss, können in den gleichen Speicher eingefügt werden.
- Richten Sie eine Antwortwarteschlange ein, die von der Benutzeroberfläche oder dem Aufrufer überwacht werden kann. Die Hintergrundaufgabe kann Nachrichten an die Warteschlange senden, die den Status und den Abschluss angeben. Daten, die die Hintergrundaufgabe an den Aufrufer zurückgeben muss, können in die Nachrichten eingefügt werden. Wenn Sie Azure Service Bus verwenden, können Sie die Eigenschaften **ReplyTo** und **CorrelationId** zur Implementierung dieser Funktion verwenden.
- Verfügbar machen Sie eine API oder einen Endpunkt der Hintergrundaufgabe verfügbar, auf die bzw. den die Benutzeroberfläche oder der Aufrufer zugreifen kann, um Statusinformationen abzurufen. Daten, die die Hintergrundaufgabe an den Aufrufer zurückgeben muss, können in die Antwort eingefügt werden.
- Lassen Sie die Hintergrundaufgabe über eine API die Benutzeroberfläche oder den Aufrufer zurückrufen, um an vordefinierten Punkten oder beim Abschluss den Status anzugeben. Dies kann durch lokal ausgelöste Ereignisse oder über einen Veröffentlichen-und-Abonnieren-Mechanismus geschehen. Die Daten, die die Hintergrundaufgabe an den Aufrufer zurückgeben muss, können in der Anforderungs- oder Ereignisnutzlast enthalten sein.

## <a name="hosting-environment"></a>Hosting-Umgebung

Hintergrundaufgaben können mithilfe einer Reihe von verschiedenen Azure Platform-Diensten gehostet werden:

- [**Azure-Web-Apps und WebJobs**](#azure-web-apps-and-webjobs). Mit WebJobs können benutzerdefinierte Aufträge basierend auf einer Reihe verschiedener Typen von Skripts oder ausführbaren Programmen im Kontext einer Web-App ausgeführt werden.
- [**Azure Virtual Machines**](#azure-virtual-machines). Wenn Sie einen Windows-Dienst haben oder die Windows-Aufgabenplanung verwenden möchten, ist es üblich, die Hintergrundaufgaben auf einem dedizierten virtuellen Computer zu hosten.
- [**Azure Batch**](#azure-batch). Batch ist ein Plattformdienst, mit dem rechenintensive Arbeitsschritte auf einer verwalteten Sammlung virtueller Computer ausgeführt werden. Sie ermöglicht das automatische Skalieren von Computeressourcen.
- [**Azure Kubernetes Service**](#azure-kubernetes-service) (AKS). Azure Kubernetes Service stellt eine verwaltete Hostingumgebung für Kubernetes in Azure bereit.

In den folgenden Abschnitten werden diese einzelnen Optionen und zu berücksichtigende Aspekte ausführlich beschrieben, um Ihnen die Auswahl der geeigneten Option zu erleichtern.

### <a name="azure-web-apps-and-webjobs"></a>Azure-Web-Apps und WebJobs

Mit Azure WebJobs können benutzerdefinierte Aufträge als Hintergrundaufgaben in einer Azure-Web-App ausgeführt werden. WebJobs werden im Kontext der Web-App als fortlaufender Prozess ausgeführt. WebJobs können auch in Reaktion auf ein Triggerereignis von Azure Scheduler oder in Reaktion auf externe Faktoren ausgeführt werden, wie z. B. Änderungen an Speicherblobs und Nachrichtenwarteschlangen. Aufträge können bei Bedarf gestartet und angehalten und ordnungsgemäß heruntergefahren werden. Wenn in einem fortlaufend ausgeführten WebJob ein Fehler auftritt, wird er automatisch neu gestartet. Wiederholungs- und Fehleraktionen sind konfigurierbar.

Wenn Sie einen WebJob konfigurieren:

- Wenn der Auftrag auf einen ereignisgesteuerten Trigger reagieren soll, sollten Sie ihn mit der Option **Dauerhaft ausführen**konfigurieren. Das Skript oder Programm wird im Ordner "site/wwwroot/app_data/jobs/continuous" gespeichert.
- Wenn der Auftrag auf einen zeitplangesteuerten Trigger reagieren soll, sollten Sie ihn mit der Option **Gemäß einem Zeitplan ausführen**konfigurieren. Das Skript oder Programm wird im Ordner "site/wwwroot/app_data/jobs/triggered" gespeichert.
- Wenn Sie zum Konfigurieren eines Auftrags die Option **Bedarfsgesteuert ausführen** wählen, wird beim Start des Auftrags der gleichen Code wie bei der Option **Gemäß einem Zeitplan ausführen** ausgeführt.

Azure WebJobs werden in der Sandbox der Web-App ausgeführt. Sie können also auf Umgebungsvariablen zugreifen und Informationen wie Verbindungszeichenfolgen mit der Web-App gemeinsam nutzen. Der Auftrag hat Zugriff auf den eindeutigen Bezeichner des Computers, auf dem der Auftrag ausgeführt wird. Die Verbindungszeichenfolge mit dem Namen **AzureWebJobsStorage** ermöglicht den Zugriff auf Azure-Speicherwarteschlangen, -Blobs und -Tabellen für Anwendungsdaten sowie auf Service Bus für Messaging und Kommunikation. Die Verbindungszeichenfolge mit dem Namen **AzureWebJobsDashboard** ermöglicht den Zugriff auf die Protokolldateien zu den Auftragsaktionen.

Azure WebJobs weisen folgende Merkmale auf:

- **Sicherheit:** WebJobs werden durch die Bereitstellungsanmeldeinformationen der Web-App geschützt.
- **Unterstützte Dateitypen:** Sie können WebJobs als Befehlsskripts (CMD), Batchdateien (BAT), PowerShell-Skripts (PS1), Bash-Shellskripts (SH), PHP-Skripts (PHP), Python-Skripts (PY), JavaScript-Code (JS) und ausführbare Programme (EXE, JAR usw.) definieren.
- **Bereitstellung:** Sie können Skripts und ausführbare Dateien über das [Azure-Portal](/azure/app-service-web/web-sites-create-web-jobs), mithilfe von [Visual Studio](/azure/app-service-web/websites-dotnet-deploy-webjobs) oder mithilfe des [Azure WebJobs SDK](/azure/app-service/webjobs-sdk-get-started) bereitstellen oder Skripts und Dateien direkt an folgende Speicherorte kopieren:
  - Für die durch Trigger initiierte Ausführung: site/wwwroot/app_data/jobs/triggered/{Auftragsname}
  - Für die fortwährende Ausführung: site/wwwroot/app_data/jobs/continuous/{Auftragsname}
- **Protokollierung:** Console.Out wird als INFO behandelt (markiert). Console.Error wird als ERROR behandelt. Sie können mit dem Azure-Portal auf Informationen zur Überwachung und Diagnose zugreifen. Sie können Protokolldateien direkt von der Website herunterladen. Sie werden an den folgenden Speicherorten gespeichert:
  - Triggerbasierte Ausführung: Vfs/data/jobs/triggered/jobName
  - Fortlaufende Ausführung: Vfs/data/jobs/continuous/jobName
- **Konfiguration:** Sie können WebJobs über das Portal, per REST-API oder mithilfe von PowerShell konfigurieren. Sie können Konfigurationsinformationen für einen Auftrag in einer Konfigurationsdatei namens „settings.job“, die sich im gleichen Stammverzeichnis wie das Auftragsskript befinden muss, bereitstellen. Beispiel: 
  - { "stopping_wait_time": 60 }
  - { "is_singleton": true }

#### <a name="considerations"></a>Überlegungen

- Standardmäßig werden WebJobs mit der Web-App skaliert. Allerdings können Sie Aufträge für die Ausführung in einer einzelnen Instanz konfigurieren, indem Sie die Konfigurationseigenschaft **is_singleton** auf **true** festlegen. Einzelinstanz-WebJobs sind für Aufgaben sinnvoll, die nicht skaliert oder gleichzeitig in mehreren Instanzen ausgeführt werden sollen, z. B. eine erneute Indizierung, Datenanalyse oder ähnliche Aufgaben.
- Damit die Leistung der Web-App durch die Aufträge möglichst minimal beeinträchtigt wird, sollten Sie in Betracht ziehen, eine leere Azure-Web-App-Instanz in einem neuen App Service-Plan zum Hosten von WebJobs zu erstellen, die langwierig oder ressourcenintensiv sind.

### <a name="azure-virtual-machines"></a>Azure Virtual Machines

Hintergrundaufgaben können so implementiert werden, dass sie nicht in Azure-Web-Apps bereitgestellt werden können oder sich dazu nicht eignen. Typische Beispiele sind Windows-Dienste sowie Hilfsprogramme und ausführbare Programme von Drittanbietern. Ein weiteres Beispiel sind Programme, die für eine Ausführungsumgebung geschrieben wurden, die sich von der Umgebung unterscheidet, in der die Anwendung gehostet wird. Beispielsweise kann dies ein UNIX- oder Linux-Programm sein, das von einer Windows- oder .NET-Anwendung ausgeführt werden soll. Sie können eines von verschiedenen Betriebssystemen für einen virtuellen Azure-Computer auswählen und den Dienst oder die ausführbare Datei dann auf diesem virtuellen Computer ausführen.

Informationen dazu, wann sich die Verwendung von Virtual Machines anbietet, finden Sie unter [Azure App Service, Cloud Services und Virtual Machines im Vergleich](/azure/app-service-web/choose-web-site-cloud-service-vm/). Die Virtual Machines-Optionen werden unter [Größen für Windows-VMs in Azure](/azure/virtual-machines/windows/sizes) erläutert. Weitere Informationen zu den verfügbaren Betriebssystemen und vorgefertigten Images für virtuelle Computer finden Sie im [Marketplace für virtuelle Azure-Computer](https://azure.microsoft.com/gallery/virtual-machines/).

Zum Initiieren der Hintergrundaufgabe in einem separaten virtuellen Computer stehen verschiedene Optionen zur Auswahl:

- Sie können die Aufgabe bei Bedarf direkt über Ihre Anwendung ausführen, indem Sie eine Anforderung an einen Endpunkt senden, den die Aufgabe verfügbar macht. Dadurch werden alle für die Aufgabe erforderlichen Daten übergeben. Der Endpunkt ruft die Aufgabe auf.
- Sie können die Aufgabe unter Verwendung eines im ausgewählten Betriebssystem verfügbaren Planungsmoduls oder Timers so konfigurieren, dass sie nach einem Zeitplan ausgeführt wird. Unter Windows können Sie beispielsweise die Windows-Aufgabenplanung zum Ausführen von Skripts und Aufgaben verwenden. Wenn Sie SQL Server auf dem virtuellen Computer installiert haben, können Sie den SQL Server-Agent zum Ausführen von Skripts und Aufgaben verwenden.
- Mit Azure Scheduler können Sie die Aufgabe initiieren, indem Sie eine Nachricht in eine Warteschlange einfügen, die von der Aufgabe überwacht wird, oder indem Sie eine Anforderung an eine API senden, die von der Aufgabe verfügbar gemacht wird.

Weitere Informationen zum Initiieren von Hintergrundaufgaben finden Sie weiter oben im Abschnitt [Trigger](#triggers) .

<!-- markdownlint-disable MD024 -->

#### <a name="considerations"></a>Überlegungen

<!-- markdownlint-enable MD024 -->

Berücksichtigen Sie die folgenden Punkte, wenn Sie entscheiden, ob Hintergrundaufgaben auf einem virtuellen Azure-Computer bereitgestellt werden sollen:

- Das Hosten von Hintergrundaufgaben in einem separaten virtuellen Azure-Computer ermöglicht Flexibilität und eine präzise Steuerung der Initiierung, Ausführung, Planung und Ressourcenzuweisung. Allerdings werden die Laufzeitkosten höher, wenn ein virtueller Computer eigens zur Ausführung von Hintergrundaufgaben bereitgestellt werden muss.
- Es ist keine Funktion zum Überwachen der Aufgaben im Azure-Portal verfügbar, und es gibt auch keine Funktion zum automatischen Neustarten fehlgeschlagener Aufgaben. Sie können allerdings den grundlegenden Status des virtuellen Computers mithilfe der [Azure Resource Manager-Cmdlets](https://msdn.microsoft.com/library/mt125356.aspx) überwachen und verwalten. Es gibt jedoch keine Möglichkeit, die Prozesse und Threads in Serverknoten zu steuern. In der Regel erfordert die Verwendung eines virtuellen Computers zusätzlichen Aufwand zur Implementierung eines Mechanismus, um Daten von der Instrumentierung der Aufgabe und vom Betriebssystem des virtuellen Computers zu sammeln. Eine möglicherweise geeignete Lösung besteht darin, das [System Center Management Pack für Azure](https://www.microsoft.com/download/details.aspx?id=50013) zu verwenden.
- Sie sollten die Erstellung von Überwachungsprüfmodulen, die über HTTP-Endpunkte verfügbar gemacht werden, in Betracht ziehen. Der Code für diese Tests könnte Integritätsprüfungen durchführen, operative Informationen und Statistiken sammeln oder Fehlerinformationen sortieren und diese Daten an die Verwaltungsanwendung zurückgeben. Weitere Informationen finden Sie unter [Überwachungsmuster für den Integritätsendpunkt](../patterns/health-endpoint-monitoring.md).

Weitere Informationen finden Sie unter

- [Virtuelle Computer](https://azure.microsoft.com/services/virtual-machines/)
- [Virtuelle Computer in Azure – häufig gestellte Fragen](/azure/virtual-machines/virtual-machines-linux-classic-faq?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json)

### <a name="azure-batch"></a>Azure Batch

Erwägen Sie die Nutzung von [Azure Batch](/azure/batch/), wenn Sie umfangreiche, parallele HPC-Workloads (High-Performance Computing) auf Dutzenden, Hunderten oder Tausenden von VMs ausführen müssen.

Der Batch-Dienst stellt die VMs bereit, weist den VMs Aufgaben zu, führt die Aufgaben aus und überwacht den Fortschritt. Batch kann die VMs als Reaktion auf die Workload automatisch horizontal hochskalieren. Batch ermöglicht auch eine Auftragsplanung. Azure Batch unterstützt sowohl Linux- als auch Windows-VMs.

<!-- markdownlint-disable MD024 -->

#### <a name="considerations"></a>Überlegungen

<!-- markdownlint-enable MD024 -->

Batch funktioniert gut für intrinsisch parallele Workloads. Es können auch parallele Berechnungen mit einem Reduzierungsschritt am Ende durchgeführt werden, oder es können [MPI-Anwendungen (Message Passing Interface)](/azure/batch/batch-mpi) für parallele Aufgaben ausgeführt werden, für die das Übergeben von Nachrichten zwischen Knoten erforderlich ist.

Ein Azure Batch-Auftrag führt einen Pool mit Knoten (VMs) aus. Ein Ansatz besteht darin, einen Pool nur bei Bedarf zuzuordnen und dann zu löschen, nachdem der Auftrag abgeschlossen wurde. Hierdurch wird die Auslastung maximiert, da die Knoten nicht ungenutzt bleiben, aber für den Auftrag muss gewartet werden, bis die Knoten zugeordnet wurden. Alternativ hierzu können Sie schon vorher einen Pool erstellen. Bei diesem Ansatz wird die Zeit verringert, die zum Starten eines Auftrags benötigt wird, aber er kann auch dazu führen, dass Knoten nicht genutzt werden. Weitere Informationen finden Sie unter [Gültigkeitsdauer für Pools und Computeknoten](/azure/batch/batch-api-basics#pool-and-compute-node-lifetime).

Weitere Informationen finden Sie unter

- [Was ist Azure Batch?](/azure/batch/batch-technical-overview)
- [Entwickeln von parallelen Computelösungen in größerem Umfang mit Batch](/azure/batch/batch-api-basics)
- [HPC-, Batch- und Big Compute-Lösungen, mit Azure-VMs](/azure/batch/batch-hpc-solutions)

### <a name="azure-kubernetes-service"></a>Azure Kubernetes Service

Azure Kubernetes Service (AKS) verwaltet Ihre gehostete Kubernetes-Umgebung und ermöglicht so die schnelle und einfache Bereitstellung und Verwaltung von Containeranwendungen.

Container können zum Ausführen von Hintergrundaufträgen nützlich sein. Es ergeben sich beispielsweise folgende Vorteile:

- Container unterstützen eine hohe Hostingdichte. Sie können eine Hintergrundaufgabe in einem Container isolieren und mehrere Container in jeder VM anordnen.
- Der Containerorchestrator führt den internen Lastenausgleich, die Konfiguration des internen Netzwerks und andere Konfigurationsaufgaben durch.
- Container können je nach Bedarf gestartet und beendet werden.
- Mit der Azure Container Registry können Sie Ihre Container innerhalb von Azure-Grenzen registrieren. Hieraus ergeben sich Vorteile in den Bereichen Sicherheit, Datenschutz und Entfernungsverringerung.

<!-- markdownlint-disable MD024 -->

#### <a name="considerations"></a>Überlegungen

<!-- markdownlint-enable MD024 -->

- Sie müssen mit der Nutzung eines Containerorchestrators vertraut sein. Je nach den Fähigkeiten Ihres DevOps-Teams kann dies ein Problem sein.

Weitere Informationen finden Sie unter

- [Übersicht über Container in Azure](https://azure.microsoft.com/overview/containers/)

- [Einführung in private Docker-Containerregistrierungen](/azure/container-registry/container-registry-intro)

## <a name="partitioning"></a>Partitionierung

Wenn Sie Hintergrundaufgaben in eine vorhandene Compute-Instanz aufnehmen möchten, müssen Sie berücksichtigen, wie sich dies auf die Qualitätsattribute der Compute-Instanz und auf die Hintergrundaufgabe selbst auswirken wird. Anhand dieser Faktoren können Sie entscheiden, ob die Aufgaben in der vorhandenen Compute-Instanz oder in einer separaten Compute-Instanz ausgeführt werden sollen:

- **Verfügbarkeit:** Für Hintergrundaufgaben ist unter Umständen nicht die gleiche Verfügbarkeit erforderlich wie für andere Anwendungskomponenten (insbesondere für die Benutzeroberfläche und für Komponenten, die direkt an der Interaktion mit dem Benutzer beteiligt sind). Hintergrundaufgaben können toleranter gegenüber Latenz, wiederholten Verbindungsfehlern und anderen Faktoren sein, die sich auf die Verfügbarkeit auswirken, da die Vorgänge in eine Warteschlange eingefügt werden können. Allerdings muss genügend Kapazität vorhanden sein, um die Sicherung von Anforderungen zu verhindern, wodurch Warteschlangen blockiert werden könnten und die Anwendung als Ganzes beeinträchtigt werden könnte.

- **Skalierbarkeit:** Für Hintergrundaufgaben gelten häufig andere Skalierbarkeitsanforderungen als für die Benutzeroberfläche und die interaktiven Komponenten der Anwendung. Eine Skalierung der Benutzeroberfläche kann erforderlich sein, um Nachfragespitzen zu bedienen, während ausstehende Hintergrundaufgaben in Zeiten mit weniger starker Auslastung von einer geringeren Anzahl von Compute-Instanzen abgeschlossen werden können.

- **Resilienz:** Der Ausfall einer Compute-Instanz, die lediglich Hintergrundaufgaben hostet, hat unter Umständen keine schwerwiegenden Auswirkungen auf die gesamte Anwendung, wenn die Anforderungen für diese Aufgaben in eine Warteschlange eingereiht oder zurückgestellt werden können, bis die Aufgabe wieder verfügbar ist. Wenn die Compute-Instanz und/oder die Aufgaben in einem angemessenen Zeitraum neu gestartet werden können, sind Benutzer der Anwendung möglicherweise nicht davon betroffen.

- **Sicherheit:** Für Hintergrundaufgaben gelten unter Umständen andere Sicherheitsanforderungen oder Einschränkungen als für die Benutzeroberfläche oder andere Komponenten der Anwendung. Wenn Sie eine separate Serverinstanz verwenden, können Sie für diese Aufgaben eine andere Sicherheitsumgebung angeben. Sie können auch Muster wie Gatekeeper verwenden, um die Serverinstanzen für Hintergrundaufgaben von der Benutzeroberfläche zu isolieren und Sicherheit und Trennung zu maximieren.

- **Leistung:** Sie können die Art der Compute-Instanz für Hintergrundaufgaben so wählen, dass sie die spezifischen Leistungsanforderungen der Aufgaben erfüllt. Dies kann bedeuten, dass eine kostengünstigere Compute-Option ausreicht, wenn für die Aufgaben nicht dieselben Verarbeitungsfunktionen wie für die Benutzeroberfläche erforderlich sind, oder dass eine größere Instanz verwendet werden muss, wenn sie zusätzliche Kapazität und Ressourcen erfordern.

- **Verwaltbarkeit:** Hintergrundaufgaben haben unter Umständen einen anderen Entwicklungs- und Bereitstellungsrhythmus als der Hauptanwendungscode oder die Benutzeroberfläche. Durch ihre Bereitstellung in einer separaten Serverinstanz können Aktualisierungen und die Versionsverwaltung vereinfacht werden.

- **Kosten:** Durch das Hinzufügen von Compute-Instanzen zur Ausführung von Hintergrundaufgaben steigen die Hostingkosten. Sie sollten sorgfältig zwischen zusätzlicher Kapazität und zusätzlichen Kosten abwägen.

Weitere Informationen finden Sie unter [Muster für die Auswahl einer übergeordneten Instanz](../patterns/leader-election.md) sowie unter [Muster für konkurrierende Consumer](../patterns/competing-consumers.md).

## <a name="conflicts"></a>Konflikte

Wenn mehrere Instanzen eines Hintergrundauftrags vorhanden sind, ist es möglich, dass sie um den Zugriff auf Ressourcen und Dienste wie Datenbanken und Speicher konkurrieren. Dieser gleichzeitige Zugriff kann zu Ressourcenkonflikte führen, die Konflikte bei der Verfügbarkeit der Dienste und der Integrität der Daten im Speicher verursachen können. Sie können Ressourcenkonflikte lösen, indem Sie pessimistische Sperren verwenden. Dadurch wird verhindert, dass konkurrierende Instanzen einer Aufgabe gleichzeitig auf einen Dienst zugreifen oder Daten beschädigen.

Ein anderer Konfliktlösungsansatz ist die Definition der Hintergrundaufgaben als Singleton, sodass immer nur eine Instanz ausgeführt wird. Allerdings gehen dadurch die Vorteile hinsichtlich der Zuverlässigkeit und Leistung verloren, die eine Konfiguration mit mehreren Instanzen bieten kann. Dies gilt insbesondere dann, wenn die Benutzeroberfläche genügend Arbeit zur Auslastung mehrerer Hintergrundaufgaben liefert.

Es muss unbedingt sichergestellt werden, dass die Hintergrundaufgabe automatisch neu gestartet werden kann und dass ausreichend Kapazität zur Verarbeitung von Nachfragespitzen verfügbar ist. Sie können dies erreichen, indem Sie einer Compute-Instanz ausreichend Ressourcen zuordnen, einen Warteschlangenmechanismus implementieren, der Anforderungen für die spätere Ausführung in Zeiten geringerer Nachfrage speichern kann, oder eine Kombination dieser Techniken verwenden.

## <a name="coordination"></a>Koordination

Die Hintergrundaufgaben können komplex sein und die Ausführung von mehreren Einzelaufgaben erfordern, um ein Ergebnis zu erhalten oder alle Anforderungen zu erfüllen. In diesen Szenarien wird die Aufgabe häufig in kleinere Einzelschritte oder Unteraufgaben aufgeteilt, die von mehreren Consumern ausgeführt werden können. Aus mehreren Schritten bestehende Aufträge können effizienter und flexibler sein, da einzelne Schritte möglicherweise in mehreren Aufträgen wiederverwendbar sind. Zudem können einzelne Schritte einfach hinzugefügt oder entfernt werden, oder die Reihenfolge der Schritte kann geändert werden.

Die Koordination mehrerer Aufgaben und Schritte kann schwierig sein, aber es gibt drei allgemeine Muster, an denen Sie sich bei der Implementierung einer Lösung orientieren können:

- **Zerlegen einer Aufgabe in mehrere wiederverwendbare Schritte**. Es kann von einer Anwendung verlangt werden, dass sie verschiedene Aufgaben unterschiedlicher Komplexität mit den von ihr verarbeiteten Informationen ausführt. Ein einfacher, aber unflexibler Ansatz zur Implementierung einer solchen Anwendung könnte darin bestehen, diese Verarbeitung in einem monolithischen Modul durchzuführen. Dieser Ansatz würde jedoch die Möglichkeiten zur Umgestaltung des Codes, zu seiner Optimierung und Wiederverwendung einschränken, wenn an anderer Stelle in der Anwendung Teile des gleichen Verarbeitungsmechanismus erforderlich sind. Weitere Informationen finden Sie unter [Muster „Pipes und Filter“](../patterns/pipes-and-filters.md).

- **Verwalten der Ausführung der Schritte einer Aufgabe**. Eine Anwendung kann Aufgaben ausführen, die eine Reihe von Schritten umfassen (von denen einige möglicherweise Remotedienste aufrufen oder auf Remoteressourcen zugreifen). Die einzelnen Schritte können voneinander unabhängig sein, aber sie werden von der Anwendungslogik orchestriert, die die Aufgabe implementiert. Weitere Informationen finden Sie unter [Muster „Scheduler-Agent-Supervisor“](../patterns/scheduler-agent-supervisor.md).

- **Verwalten der Wiederherstellung von fehlgeschlagenen Aufgabenschritten**. Eine Anwendung muss möglicherweise die Vorgänge rückgängig machen, die für eine Reihe von Schritten ausgeführt wurden (die schlussendlich zusammen einen konsistenten Vorgang bilden), wenn mindestens einer dieser Schritte fehlschlägt. Weitere Informationen finden Sie unter [Muster „Kompensierende Transaktion“](../patterns/compensating-transaction.md).

## <a name="resiliency-considerations"></a>Überlegungen zur Resilienz

Hintergrundaufgaben müssen stabil sein, um der Anwendung zuverlässige Dienste zur Verfügung stellen zu können. Berücksichtigen Sie beim Planen und Entwerfen von Hintergrundaufgaben die folgenden Punkte:

- Hintergrundaufgaben müssen in der Lage sein, Neustarts ordnungsgemäß zu verarbeiten, ohne Daten zu beschädigen oder Inkonsistenzen in die Anwendung einzuführen. Bei langwierigen oder aus mehreren Schritten bestehenden Aufgaben sollten Sie die Verwendung von *Prüfpunkten* in Betracht ziehen, um den Status von Aufträgen im beständigen Speicher oder als Nachrichten in einer Warteschlange zu speichern, wenn dies zweckmäßig ist. Beispielsweise können Sie Statusinformationen in einer Nachricht in einer Warteschlange speichern und diese Statusinformationen inkrementell mit dem Aufgabenstatus aktualisieren, sodass die Aufgabe ab dem letzten bekannten fehlerfreien Prüfpunkt verarbeitet werden kann, statt sie ganz von vorn neu starten zu müssen. Bei Verwendung von Azure Service Bus-Warteschlangen können Sie dieses Szenario mithilfe von Nachrichtensitzungen ermöglichen. Sitzungen ermöglichen Ihnen, den Anwendungsverarbeitungsstatus mithilfe der Methoden [SetState](/dotnet/api/microsoft.servicebus.messaging.messagesession.setstate?view=azureservicebus-4.0.0) und [GetState](/dotnet/api/microsoft.servicebus.messaging.messagesession.getstate?view=azureservicebus-4.0.0) zu speichern und abzurufen. Weitere Informationen zum Entwerfen von zuverlässigen mehrstufigen Prozessen und Workflows finden Sie unter [Muster „Scheduler-Agent-Supervisor“](../patterns/scheduler-agent-supervisor.md).

- Wenn Sie Warteschlangen zur Kommunikation mit Hintergrundaufgaben verwenden, können die Warteschlangen in Zeiten, in denen die Auslastung der Anwendung höher als üblich ist, als Puffer zum Speichern von Anforderungen, die an die Aufgaben gesendet werden, fungieren. Dadurch können die Aufgaben in Zeiträumen mit einer geringeren Auslastung mit der Benutzeroberfläche gleich ziehen. Es bedeutet auch, dass die Benutzeroberfläche nicht durch Neustarts blockiert wird. Weitere Informationen finden Sie unter [Warteschlangenbasiertes Lastenausgleichsmuster](../patterns/queue-based-load-leveling.md). Wenn einige Aufgaben wichtiger als andere sind, ziehen Sie in Betracht, durch eine Implementierung des Musters [Prioritätswarteschlange](../patterns/priority-queue.md) sicherzustellen, dass diese Aufgaben vor den weniger wichtigen Aufgaben ausgeführt werden.

- Hintergrundaufgaben, die durch Nachrichten ausgelöst werden oder Nachrichten verarbeiten, müssen so entworfen werden, dass sie Inkonsistenzen behandeln können. Dies sind beispielsweise Nachrichten, die nicht in der richtigen Reihenfolge eintreffen, Nachrichten, die wiederholt einen Fehler verursachen (so genannte *nicht verarbeitbare Nachrichten*), und Nachrichten, die mehrmals übermittelt werden. Beachten Sie Folgendes:

  - Nachrichten, die in einer bestimmten Reihenfolge verarbeitet werden müssen, z. B. Nachrichten, die Daten basierend auf dem vorhandenen Datenwert (z. B. durch Hinzufügen eines Werts zu einem vorhandenen Wert) ändern, treffen möglicherweise nicht in der ursprünglichen Reihenfolge ein, in der sie gesendet wurden. Stattdessen können sie auch von anderen Instanzen einer Hintergrundaufgabe in einer anderen Reihenfolge je nach Auslastung der verschiedenen Instanzen behandelt werden. Nachrichten, die in einer bestimmten Reihenfolge verarbeitet werden müssen, sollten eine Sequenznummer, einen Schlüssel oder einen anderen Indikator enthalten, damit in Hintergrundaufgaben sichergestellt werden kann, dass sie in der richtigen Reihenfolge verarbeitet werden. Bei Verwendung von Azure Service Bus können Sie mit Nachrichtensitzungen die Reihenfolge der Bereitstellung gewährleisten. Es ist in der Regel jedoch effizienter, den Prozess möglichst so zu entwerfen, dass die Reihenfolge der Nachrichten nicht von Bedeutung ist.

  - In der Regel sieht eine Hintergrundaufgabe Nachrichten in der Warteschlange ein. Dadurch sind sie vorübergehend vor anderen Nachrichtenconsumern verborgen. Dann löscht sie die Nachrichten, nachdem sie erfolgreich verarbeitet wurden. Wenn eine Hintergrundaufgabe beim Verarbeiten einer Nachricht fehlschlägt, wird die Nachricht in der Warteschlange erneut angezeigt, sobald das Timeout für das Einsehen erreicht ist. Sie wird von einer anderen Instanz der Aufgabe oder während des nächsten Verarbeitungszyklus dieser Instanz verarbeitet. Wenn die Nachricht beständig einen Fehler im Consumer verursacht, blockiert sie die Aufgabe, die Warteschlange und schließlich die Anwendung selbst, wenn die Warteschlange voll ist. Daher ist es wichtig, dass nicht verarbeitbare Nachrichten erkannt und aus der Warteschlange entfernt werden. Bei Verwendung von Azure Service Bus können Nachrichten, die einen Fehler verursachen, automatisch oder manuell an eine zugeordnete Warteschlange für unzustellbare Nachrichten verschoben werden.

  - Warteschlangen verfügen garantiert über Übermittlungsmechanismen für eine *mindestens einmalige* Zustellung, aber sie können die gleiche Nachricht mehrmals übermitteln. Wenn eine Hintergrundaufgabe ausfällt, nachdem eine Nachricht verarbeitet, aber bevor diese aus der Warteschlange gelöscht wurde, wird die Nachricht wieder zur Verarbeitung verfügbar. Hintergrundaufgaben sollte idempotent sein, d. h. dass die mehrmalige Verarbeitung derselben Nachricht keinen Fehler oder keine Inkonsistenzen in den Daten der Anwendung verursacht. Einige Vorgänge sind naturgemäß idempotent, z. B. das Festlegen einer gespeicherten Einstellung auf einen bestimmten neuen Wert. Jedoch führen andere Vorgänge zu Inkonsistenzen, z. B. das Hinzufügen eines Werts zu einem vorhandenen gespeicherten Wert, ohne zu überprüfen, ob der gespeicherte Wert dem Wert entspricht, der in der Nachricht ursprünglich gesendet wurde. Azure Service Bus-Warteschlangen können so konfiguriert werden, dass doppelte Nachrichten automatisch entfernt werden.

  - Einige Messagingsysteme, wie z. B. Azure-Speicherwarteschlangen und Azure Service Bus-Warteschlangen unterstützen eine Eigenschaft, die angibt, wie oft eine Nachricht aus der Warteschlange gelesen wurde. Dies kann bei der Verarbeitung wiederholter und nicht verarbeitbarer Nachrichten hilfreich sein. Weitere Informationen finden Sie unter [Einführung in asynchrone Nachrichten](https://msdn.microsoft.com/library/dn589781.aspx) und [Idempotenzmuster](https://blog.jonathanoliver.com/idempotency-patterns/).

## <a name="scaling-and-performance-considerations"></a>Überlegungen zur Skalierung und Leistung

Hintergrundaufgaben müssen ausreichend Leistung bieten, damit sichergestellt ist, dass sie nicht die Anwendung blockieren oder durch eine verzögerte Verarbeitung bei einem stark ausgelasteten System zu Inkonsistenzen führen. In der Regel wird die Leistung durch die Skalierung der Serverinstanzen, die die Hintergrundaufgaben hosten, verbessert. Berücksichtigen Sie beim Planen und Entwerfen von Hintergrundaufgaben die folgenden Punkte in Bezug auf Skalierbarkeit und Leistung:

- Azure unterstützt die automatische Skalierung (horizontales Hoch- und Herunterskalieren) basierend auf der aktuellen Nachfrage und Last oder nach einem vordefinierten Zeitplan für Web-Apps und Bereitstellungen, die auf virtuellen Computern gehostet werden. Nutzen Sie diese Funktion, um sicherzustellen, dass die Anwendung als Ganzes über ausreichend Leistungsfähigkeit verfügt, und gleichzeitig die Laufzeitkosten zu minimieren.

- Wenn Hintergrundaufgaben eine andere Leistungsfähigkeit als andere Teile einer Anwendung haben (z.B. die Benutzeroberfläche oder Komponenten wie die Datenzugriffsschicht), kann durch das gemeinsame Hosten der Hintergrundaufgaben in einem separaten Computedienst erreicht werden, dass die Benutzeroberfläche und die Hintergrundaufgaben unabhängig voneinander skaliert werden, um die Last zu verwalten. Wenn sich die Leistungsfähigkeit verschiedener Hintergrundaufgaben deutlich voneinander unterscheidet, sollten Sie in Erwägung ziehen, diese aufzuteilen und die verschiedenen Typen getrennt voneinander zu skalieren. Beachten Sie jedoch, dass dies die Laufzeitkosten erhöhen kann.

- Eine einfache Skalierung der Computeressourcen ist u.U. nicht ausreichend, um Leistungsverluste unter Last zu verhindern. Sie müssen möglicherweise auch Speicherwarteschlangen und andere Ressourcen skalieren, um zu vermeiden, dass ein einzelner Punkt in der gesamten Verarbeitungskette zum Engpass wird. Berücksichtigen Sie auch andere Einschränkungen, z. B. den maximalen Durchsatz des Speichers sowie anderer Dienste, von denen die Anwendung und die Hintergrundaufgaben abhängen.

- Hintergrundaufgaben müssen für die Skalierung konzipiert werden. Beispielsweise müssen sie dynamisch die Anzahl der verwendeten Speicherwarteschlangen erkennen können, um Nachrichten überwachen oder an die entsprechende Warteschlange senden zu können.

- In der Standardeinstellung werden WebJobs zusammen mit ihrer zugehörigen Azure-Web-Apps-Instanz skaliert. Wenn ein WebJob jedoch nur als einzelne Instanz ausgeführt werden soll, können Sie eine Datei namens „Settings.job“ erstellen, in der die JSON-Daten **{"Is_singleton": true}** enthalten sind. Dadurch wird Azure gezwungen, nur eine Instanz des WebJob auszuführen, auch wenn mehrere Instanzen der zugeordneten Web-App vorhanden sind. Dies kann eine nützliche Technik für geplante Aufträge sein, die nur als einzelne Instanz ausgeführt werden müssen.

## <a name="related-patterns"></a>Verwandte Muster

- [Leitfaden zur Serverpartitionierung](https://msdn.microsoft.com/library/dn589773.aspx)
