---
title: 'Mainframemigration: Übersicht'
description: Migrieren von Anwendungen aus Mainframe-Umgebungen zu Azure, einer bewährten, hoch verfügbaren und skalierbaren Infrastruktur für Systeme, die derzeit auf Mainframes ausgeführt werden.
author: njray
ms.date: 12/27/2018
ms.openlocfilehash: 41fc799f15500276ada1667121e5f1fce3413a3a
ms.sourcegitcommit: 273e690c0cfabbc3822089c7d8bc743ef41d2b6e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/08/2019
ms.locfileid: "55900790"
---
# <a name="mainframe-migration-overview"></a>Übersicht zur Mainframemigration

Viele Unternehmen und Organisationen profitieren davon, einige oder alle ihrer Mainframeworkloads, -anwendungen und -datenbanken in die Cloud zu verschieben. Azure bietet mainframeähnliche Funktionen im Cloudmaßstab ohne viele der mit Mainframes assoziierten Nachteile.

Der Begriff „Mainframe“ bezieht sich im Allgemeinen auf ein Großrechnersystem, der Großteil der derzeit eingesetzten Mainframes sind jedoch IBM System Z-Server oder IBM-Plug-kompatible Systeme unter MVS, DOS, VSE, OS/390 oder z/OS. Mainframesysteme werden weiterhin in zahlreichen Branchen verwendet, um wichtige Informationssysteme auszuführen, und sie haben einen Platz in hochspezifischen Szenarien, z.B. in transaktionsintensiven großen IT-Umgebungen mit hohem Datenvolumen.

Mit der Migration in die Cloud können Unternehmen ihre Infrastruktur modernisieren. Mit Clouddiensten können Sie Mainframeanwendungen, und den Wert, den sie bieten, als Workload verfügbar machen, wenn Ihre Organisation sie benötigt. Viele Workloads können mit nur geringfügigen Änderungen am Code, z.B. Aktualisieren der Namen der Datenbanken, in Azure übertragen werden. Sie können komplexere Workloads in Phasen migrieren.

Die meisten Fortune 500-Unternehmen setzen Azure bereits für ihre kritischen Workloads ein. Die erheblichen Grundkostenanreize von Azure motivieren viele Migrationsprojekte. Unternehmen verschieben in der Regel die Entwicklungs- und Testworkloads zuerst in Azure, gefolgt von DevOps, E-Mail und Notfallwiederherstellung als Dienst.

## <a name="intended-audience"></a>Zielpublikum

Wenn Sie eine Migration oder das Hinzufügen von Clouddiensten als Option für Ihre IT-Umgebung in Betracht ziehen, ist dieses Handbuch für Sie richtig.

Diese Anleitung hilft IT-Organisationen, die Konversation über die Migration zu beginnen. Da Sie mit Azure und cloudbasierten Infrastrukturen vielleicht besser vertraut sind als mit Mainframes, beginnt dieses Handbuch mit einem Überblick zur Funktionsweise von Mainframes und fährt mit verschiedenen Strategien fort, zu bestimmen, was und wie migriert werden sollte.

## <a name="mainframe-architecture"></a>Mainframearchitektur

In den späten 1950er-Jahren wurden Mainframes als zentral hochskalierte Server zur Ausführung umfangreicher Onlinetransaktionen und zur Batchverarbeitung konzipiert. Aus diesem Grund verfügen Mainframes über Software für Onlinetransaktionsformulare (manchmal als grüne Bildschirme bezeichnet) und Hochleistungs-E/A-Systeme für die Verarbeitung von Batchausführungen.

Mainframes haben den Ruf hoher Zuverlässigkeit und Verfügbarkeit und sind bekannt für ihre Fähigkeit zum Ausführen großer Onlinetransaktionen und Batchaufträge. Eine Transaktion ergibt sich aus einem durch eine einzelne Anforderung initiierten Teil der Verarbeitung, in der Regel von einem Benutzer an einem Terminal. Transaktionen können auch aus vielen anderen Quellen einschließlich Webseiten, Remotearbeitsstationen und Anwendungen von anderen Informationssystemen stammen. Eine Transaktion kann auch automatisch zu einem vordefinierten Zeitpunkt ausgelöst werden, wie die folgende Abbildung zeigt.

![Komponenten in einer typischen IBM-Mainframearchitektur](../../_images/mainframe-migration/zOS-architectural-layers.png)

Eine typische IBM-Mainframearchitektur umfasst diese gemeinsamen Komponenten:

- **Front-End-Systeme:** Benutzer können Transaktionen von Terminals, Webseiten oder Remotearbeitsstationen aus initiieren. Mainframeanwendungen verfügen häufig über benutzerdefinierte Benutzeroberflächen, die nach der Migration zu Azure beibehalten werden können. Terminalemulatoren werden immer noch zum Zugriff auf Mainframeanwendungen verwendet und auch als Terminals mit grünen Bildschirmen bezeichnet.

- **Anwendungsschicht:** Mainframes enthalten in der Regel ein Kundeninformations-Steuersystem (Customer Information Control System, CICS), eine führende Transaktionsverwaltungssuite für IBM z/OS-Mainframes, die häufig mit dem IBM Information Management System (IMS), einem nachrichtenbasierten Transaktions-Manager, verwendet wird. Batchsysteme behandeln Datenupdates mit hohen Durchsätzen für große Mengen von Kontodatensätzen.

- **Code:** Zu den von Mainframes verwendeten Programmiersprachen zählen COBOL, Fortran, PL/I und Natural. Job Control Language (JCL) wird zur Arbeit mit z/OS verwendet.

- **Datenbankebene:** Ein allgemeines relationales Datenbank-Managementsystem (Database Management System, DBMS) für z/OS ist IBM DD2. Es verwaltet als *dbspaces* bezeichnete Datenstrukturen, die eine oder mehrere Tabellen enthalten und Speicherpools von physischen Datasets namens *dbextents* zugewiesen sind. Zwei wichtige Datenbankkomponenten sind das Verzeichnis, das Datenspeicherorte in den Speicherpools identifiziert, und das Protokoll, das eine Aufzeichnung der an der Datenbank ausgeführten Vorgänge enthält. Verschiedene Flatfiledatenformate werden unterstützt. DB2 für z/OS verwendet in der Regel VSAM-Datasets (Virtual Storage Access Method, Methode des virtuellen Speicherzugriffs) zum Speichern der Daten.

- **Verwaltungsschicht:** IBM-Mainframes enthalten Planungssoftware wie TWS-OPC, Tools für die Druck- und Ausgabeverwaltung wie CA-SAR und SPOOL sowie ein Quellcodeverwaltungssystem. Die sichere Zugriffssteuerung für z/OS wird von der Resource Access Control Facility (RACF) behandelt. Ein Datenbank-Manager ermöglicht den Zugriff auf Daten in der Datenbank und wird in einer eigenen Partition in einer z/OS-Umgebung ausgeführt.

- **LPAR:** Logische Partitionen oder LPARs werden verwendet, um Computeressourcen zu unterteilen. Ein Mainframe wird in mehrere LPARs partitioniert.

- **z/OS:** Ein 64-Bit-Betriebssystem, das am häufigsten für IBM-Mainframes verwendet wird.

IBM-Systeme verwenden eine Transaktionsüberwachung wie CICS, um alle Aspekte einer Geschäftstransaktion nachzuverfolgen und zu verwalten. CICS verwaltet die gemeinsame Nutzung von Ressourcen, die Integrität von Daten und die Priorisierung der Ausführung. CICS autorisiert Benutzer, weist Ressourcen zu und übergibt Datenbankanforderungen mittels der Anwendung an einen Datenbank-Manager, z.B. IBM DB2.

Für eine genauere Optimierung wird CICS häufig mit IMS/TM (früher IMS/Data Communications oder IMS/DC) verwendet. IMS wurde entwickelt, um die Datenredundanz zu verringern, indem nur eine einzige Kopie der Daten beibehalten wird. Es ergänzt CICS als Transaktionsüberwachung, indem der Status während des Prozesses beibehalten wird und Geschäftsfunktionen in einem Datenspeicher aufgezeichnet werden.

## <a name="mainframe-operations"></a>Mainframevorgänge

Folgende Mainframevorgänge sind typisch:

- **Online:** Zu den Workloads zählen Transaktionsverarbeitung, Datenbankverwaltung und Verbindungen. Sie werden häufig mithilfe von IBM DB2-, CICS- und z/OS-Connectors implementiert.

- **Batch:** Aufträge werden ohne Eingreifen des Benutzers ausgeführt, in der Regel nach einem regelmäßigen Zeitplan, z.B. morgens an jedem Werktag. Batchaufträge können mit einem JCL-Emulator wie Micro Focus Enterprise Server oder BMC Control-M-Software auf Systemen ausgeführt werden, die auf Windows oder Linux basieren.

- **Job Control Language (JCL):** Geben Sie die erforderlichen Ressourcen zum Verarbeiten von Batchaufträgen an. JCL überträgt diese Informationen über eine Reihe von Auftragssteuerungsanweisungen auf z/OS. Grundlegendes JCL enthält sechs Typen von Anweisungen: JOB, ASSGN, DLBL, EXTENT, LIBDEF und EXEC. Ein Auftrag kann mehrere EXEC-Anweisungen (Schritte) enthalten, und jeder Schritt kann mehrere LIBDEF-, ASSGN-, DLBL- und EXTENT-Anweisungen aufweisen.

- **Initial Program Load (IPL):**  Bezieht sich auf das Laden einer Kopie des Betriebssystems von einem Datenträger in den tatsächlichen Speicher eines Prozessors und ihre Ausführung. IPLs dienen zur Wiederherstellung nach Downtime. Ein IPL entspricht dem Starten des Betriebssystems auf Windows- oder Linux-VMs.

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Mythen und Fakten](myths-and-facts.md)
