---
title: Beschleunigen der digitalen bildbasierten Modellierung in Azure
titleSuffix: Azure Example Scenarios
description: Beschleunigen der digitalen bildbasierten Modellierung in Azure mit Avere und Agisoft PhotoScan
author: adamboeglin
ms.date: 1/11/2019
ms.topic: example-scenario
ms.service: architecture-center
ms.subservice: example-scenario
ms.custom: cat-team, Linux, HPC
social_image_url: /azure/architecture/example-scenario/infrastructure/media/architecture-image-modeling.png
ms.openlocfilehash: 87b43347fb5f4baec0081a67c8b003dccd2fdf0d
ms.sourcegitcommit: 14226018a058e199523106199be9c07f6a3f8592
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/31/2019
ms.locfileid: "55483010"
---
# <a name="accelerate-digital-image-based-modeling-on-azure"></a>Beschleunigen der digitalen bildbasierten Modellierung in Azure

Dieses Beispielszenario kann als Architektur- und Entwurfsleitfaden für jede Organisation dienen, die die bildbasierte Modellierung in Azure IaaS (Infrastructure-as-a-Service) durchführen möchte. Das Szenario ist für die Ausführung von Photogrammetriesoftware in Azure Virtual Machines (VMs) mit Hochleistungsspeicher zum Beschleunigen der Verarbeitung konzipiert. Die Umgebung kann ohne Kompromisse bei der Leistung nach Bedarf zentral hoch- und herunterskaliert werden und unterstützt Terabytes von Speicher.

## <a name="relevant-use-cases"></a>Relevante Anwendungsfälle

Zu den relevanten Anwendungsfällen zählen:

- Modellierung und Messung von Gebäuden und Bauwerken sowie Unfallorten zur forensischen Untersuchung
- Erstellung von visuellen Effekten für Computerspiele und Filme
- Verwendung digitaler Bilder zum indirekten Generieren von Messungen von Objekten verschiedener Größen (z. B. bei der Stadtplanung und anderen Anwendungen)

## <a name="architecture"></a>Architecture

In diesem Beispiel wird die Verwendung der Photogrammetriesoftware Agisoft PhotoScan mit Avere vFXT-Speicher beschrieben. Die Software PhotoScan wurde aufgrund ihrer Beliebtheit im Bereich der GIS-Anwendungen (geografische Informationssysteme), der Dokumentation von Kulturgütern, Entwicklung von Spielen und Erzeugung visueller Effekte ausgewählt. Sie eignet sich sowohl für die Nahbereichsphotogrammetrie als auch die Luftphotogrammetrie.

Die in diesem Artikel vorgestellten Konzepte gelten für alle HPC-Workloads (High Performance Computing) auf der Grundlage von Planer- und Workerknoten, die als Infrastruktur verwaltet werden.  Die Speicherlösung Avere vFXT wurde aufgrund ihrer herausragenden Leistung in Benchmarktests für diese Workload ausgewählt.  Da das Szenario den Speicher von der Verarbeitung entkoppelt, können jedoch auch andere Speicherlösungen verwendet werden (siehe [Alternativen](#alternatives) weiter unten in diesem Dokument).

Zum Steuern des Zugriffs auf Azure-Ressourcen und Bereitstellen der internen Namensauflösung durch das Domain Name System (DNS) enthält diese Architektur auch Active Directory-Domänencontroller. Jumpboxes bieten Administratoren Zugriff auf die Windows- und Linux-VMs, auf denen die Lösung ausgeführt wird.

![Architekturdiagramm](./media/architecture-image-modeling.png)

1. Der Benutzer übermittelt eine Reihe von Bildern an PhotoScan.
2. Der PhotoScan-Planer wird auf einer Windows-VM ausgeführt, die als Hauptknoten fungiert und die Verarbeitung der Bilder des Benutzers steuert.
3. PhotoScan sucht nach allgemeinen Punkten in den Fotos und erstellt die Geometrie (Gittermodell) mithilfe der PhotoScan-Verarbeitungsknoten, die auf VMs mit Grafikprozessoren (Graphics Processing Unit, GPU) ausgeführt werden.
4. Avere vFXT stellt eine Hochleistungsspeicherlösung in Azure bereit, die auf Network File System Version 3 (NFS V3) basiert und aus mindestens vier VMs besteht.
5. PhotoScan rendert das Modell.

### <a name="components"></a>Komponenten

- [Agisoft PhotoScan](http://www.agisoft.com/): Der PhotoScan-Planer wird auf einer Windows Server 2016-VM ausgeführt, und die Verarbeitungsknoten nutzen fünf VMs mit GPUs unter CentOS Linux 7.5.
- [Avere vFXT](/azure/avere-vfxt/avere-vfxt-overview) ist eine Lösung zur Dateizwischenspeicherung, die Objektspeicher und herkömmlichen Network Attached Storage (NAS) verwendet, um die Speicherung großer Datasets zu optimieren.  Sie hat folgenden Inhalt:
  - Avere-Controller. Diese VM führt das Skript aus, durch das der Avere vFXT-Cluster installiert wird, und führt Ubuntu 18.04 LTS aus. Die VM kann später zum Hinzufügen oder Entfernen von Clusterknoten und auch zum Zerstören des Clusters verwendet werden.
  - vFXT-Cluster. Es werden mindestens drei VMs verwendet, eine für jeden der auf Avere OS 5.0.2.1 basierenden Avere vFXT-Knoten. Diese VMs bilden den vFXT-Cluster, der an Azure Blob Storage angefügt wird.
- [Microsoft Active Directory-Domänencontroller](/windows/desktop/ad/active-directory-domain-services) ermöglichen den Hostzugriff auf Domänenressourcen und stellen die DNS-Namensauflösung bereit. Avere vFXT fügt eine Reihe von A-Einträgen hinzu. Beispielsweise verweist jeder A-Eintrag in einem vFXT-Cluster auf die IP-Adresse der einzelnen Avere vFXT-Knoten. In dieser Konfiguration greifen alle VMs mit dem Roundrobin-Muster auf vFXT-Exporte zu.
- [Andere VMs](/azure/virtual-machines/) dienen als Jumpboxes, über die der Administrator auf die Planer- und Verarbeitungsknoten zugreift. Die Windows-Jumpbox ist erforderlich, damit der Administrator über das Remotedesktopprotokoll auf den Hauptknoten zugreifen kann. Die zweite Jumpbox ist optional und führt zur Verwaltung der Workerknoten Linux aus.
- [Netzwerksicherheitsgruppen](/azure/virtual-network/manage-network-security-group) (NSGs) beschränken den Zugriff auf die öffentliche IP-Adresse (PIP) und lassen die Ports 3389 und 22 für den Zugriff auf die VMs zu, die an das Subnetz der Jumpbox angefügt sind.
- Per [Peering in virtuellen Netzwerken](/azure/virtual-network/virtual-network-peering-overview) wird ein virtuelles PhotoScan-Netzwerk mit einem virtuellen Avere-Netzwerk verbunden.
- [Azure Blob Storage](/azure/storage/blobs/storage-blobs-introduction) arbeitet mit Avere vFXT als Kernspeichereinheit, um die committeten Daten zu speichern, die verarbeitet werden. Avere vFXT identifiziert die in Azure Blob Storage gespeicherten aktiven Daten und verschiebt sie per Tiering auf SSD-Datenträger (Solid State Drive), die während der Ausführung eines PhotoScan-Auftrags zum Zwischenspeichern in den Computeknoten verwendet werden. Wenn Änderungen vorgenommen werden, werden die Daten asynchron zurück an die Kernspeichereinheit committet.
- [Azure Key Vault](/azure/key-vault/key-vault-overview) wird zum Speichern der Administratorkennwörter und des PhotoScan-Aktivierungscodes verwendet.

### <a name="alternatives"></a>Alternativen

- Wenn Sie Azure-Dienste für die Verwaltung eines HPC-Clusters nutzen möchten, können Sie Tools wie Azure CycleCloud oder Azure Batch verwenden, anstatt die Ressourcen über Vorlagen oder Skripts zu verwalten.
- Stellen Sie anstelle Avere vFXT das parallele virtuelle BeeGFS-Dateisystem als Back-End-Speicher in Azure bereit. Verwenden Sie die [BeeGFS-Vorlage](https://github.com/paulomarquesc/beegfs-template), um diese End-to-End-Lösung in Azure bereitzustellen.
- Stellen Sie die Speicherlösung Ihrer Wahl bereit, z. B. GlusterFS, Lustre oder direkte Windows-Speicherplätze. Bearbeiten Sie dazu die [PhotoScan-Vorlage](https://github.com/paulomarquesc/photoscan-template) zur Verwendung der gewünschten Speicherlösung.
- Stellen Sie die Workerknoten nicht mit Linux (Standardoption), sondern mit dem Windows-Betriebssystem bereit. Wenn Sie sich für Windows-Knoten entscheiden, werden die Optionen für die Speicherintegration nicht von den Bereitstellungsvorlagen ausgeführt. Sie müssen die Umgebung manuell in eine vorhandene Speicherlösung integrieren oder die PhotoScan-Vorlage anpassen, um eine solche Automatisierung zu implementieren (Informationen dazu finden Sie im [Repository](https://github.com/paulomarquesc/photoscan-template/blob/master/docs/AverePostDeploymentSteps.md)).

## <a name="considerations"></a>Überlegungen

Dieses Szenario ist speziell für die Bereitstellung von Hochleistungsspeicher für eine HPC-Workload unter Windows oder Linux konzipiert. Im Allgemeinen sollte die Speicherkonfiguration der HPC-Workload den jeweiligen bewährten Methoden entsprechen, die für lokale Bereitstellungen verwendet werden.

Die Überlegungen zur Bereitstellung hängen von den verwendeten Anwendungen und Diensten ab. Folgendes ist jedoch zu beachten:

- Verwenden Sie beim Erstellen von Hochleistungsanwendungen Azure Storage Premium, und [optimieren Sie die Anwendungsschicht](/azure/virtual-machines/windows/premium-storage-performance). Optimieren Sie den Speicher für den häufigen Zugriff mit der [heißen Zugriffsebene](/azure/storage/blobs/storage-blob-storage-tiers) von Azure Blob Storage.
- Verwenden Sie eine [Replikationsoption](/azure/storage/common/storage-redundancy) für den Speicher, die Ihre Anforderungen in Bezug auf die Verfügbarkeit und Leistung erfüllt. In diesem Beispiel wird Avere vFXT standardmäßig mit lokal redundantem Speicher (Locally Redundant Storage, LRS) für Hochverfügbarkeit konfiguriert. Zum Bereitstellen eines Lastenausgleichs greifen in dieser Konfiguration alle VMs mit dem Roundrobin-Muster auf vFXT-Exporte zu.
- Verwenden Sie Samba-Server zum Unterstützen der Windows-Knoten, wenn der Back-End-Speicher sowohl von Windows- als auch Linux-Clients genutzt wird. Eine auf BeeGFS beruhende [Version](https://github.com/paulomarquesc/beegfs-template) dieses Beispielszenarios verwendet Samba, um den Planerknoten der unter Windows ausgeführten HPC-Workload (PhotoScan) zu unterstützen. Ein Lastenausgleich wird als intelligenter Ersatz für den DNS-Roundrobin bereitgestellt.
- Führen Sie HPC-Anwendungen mit dem VM-Typ aus, der sich am besten für Ihre [Windows](/azure/virtual-machines/windows/sizes-hpc)- oder [Linux](/azure/virtual-machines/linux/sizes?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)-Workload eignet.
- Isolieren Sie die HPC-Workload von den Speicherressourcen, indem Sie sie jeweils in einem eigenen virtuellen Netzwerk bereitstellen, und verbinden Sie die beiden Komponenten dann per [Peering](/azure/virtual-network/virtual-network-peering-overview) in virtuellen Netzwerken. Durch das Peering wird eine Verbindung mit geringer Wartezeit und hoher Bandbreite zwischen Ressourcen in unterschiedlichen virtuellen Netzwerken erstellt und Datenverkehr durch die Microsoft-Backbone-Infrastruktur nur über private IP-Adressen weitergeleitet.

### <a name="security"></a>Sicherheit

In diesem Beispiel geht es in erster Linie um die Bereitstellung einer Hochleistungsspeicherlösung für eine HPC-Workload. Die vorgestellte Lösung ist daher keine Sicherheitslösung. Ziehen Sie bei allen Änderungen Ihr Sicherheitsteam hinzu.

Die Beispielinfrastruktur ermöglicht es, alle Windows-VMs in eine Domäne einzubinden, und verwendet Active Directory für die zentrale Authentifizierung, um die Sicherheit zu verbessern. Zudem bietet sie benutzerdefinierte DNS-Dienste für alle VMs. Um die Umgebung zu schützen, beruht diese Vorlage auf [Netzwerksicherheitsgruppen (NSGs)](/azure/virtual-network/security-overview). NSGs bieten grundlegende Datenverkehrsfilter und Sicherheitsregeln.

Die Sicherheit kann in diesem Szenario mit den folgenden Optionen zusätzlich erhöht werden:

- Verwenden von virtuellen Netzwerkgeräten wie Fortinet, Checkpoint und Juniper
- Anwenden der [rollenbasierten Zugriffssteuerung](/azure/role-based-access-control/overview) auf die Ressourcengruppen
- Aktivieren des VM-[JIT](/azure/security-center/security-center-just-in-time)-Zugriffs, wenn über das Internet auf Jumpboxes zugegriffen wird
- Verwenden von [Azure Key Vault](/azure/key-vault/quick-create-portal) zum Speichern der Kennwörter von Administratorkonten

## <a name="pricing"></a>Preise

Die Kosten für die Umsetzung dieses Szenarios hängen von mehreren Faktoren ab und können erheblich variieren.  Die Anzahl und Größe der VMs, der erforderliche Speicherplatz und die zum Ausführen eines Auftrags benötigte Zeit bestimmen die Kosten.

Das folgende Beispielkostenprofil im [Azure-Preisrechner](https://azure.com/e/42362ddfd2e245a28a8e78bc609c80f3) basiert auf einer typischen Konfiguration für Avere vFXT und PhotoScan:

- Eine A1\_v2-Ubuntu-VM zum Ausführen des Avere-Controllers
- Drei D16s\_v3-Avere OS-VMs (eine für jeden der Avere vFXT-Knoten, aus denen der vFXT-Cluster besteht)
- Fünf NC24\_v2-Linux-VMs zum Bereitstellen der erforderlichen GPUs für die PhotoScan-Verarbeitungsknoten
- Eine D8s\_v3-CentOS-VM für den PhotoScan-Planerknoten
- Eine DS2\_v2-CentOS-VM zur Verwendung als Administrator-Jumpbox
- Zwei DS2\_v2-VMs für die Active Directory-Domänencontroller
- Verwaltete Premium-Datenträger
- Universeller v2 (GPv2)-Blobspeicher mit lokal redundantem Speicher und heißer Zugriffsebene (das Zugriffsebenenattribut ist nur bei GPv2-Speicherkonten verfügbar)
- Virtuelles Netzwerk mit Unterstützung für die 10 TB-Datenübertragung

Ausführliche Informationen zu dieser Architektur finden Sie im [E-Book](https://azure.microsoft.com/en-us/resources/deploy-agisoft-photoscan-on-azure-with-azere-vfxt-for-azure-or-beegfs/). Wenn Sie wissen möchten, welche Kosten für Ihren spezifischen Anwendungsfall entstehen, wählen Sie im Preisrechner die unterschiedlichen VM-Größen für Ihre erwartete Bereitstellung aus.

## <a name="deployment"></a>Bereitstellung

Ausführliche Anweisungen zum Bereitstellen dieser Architektur (einschließlich aller Voraussetzungen für die Verwendung von Avere FXT oder BeeGFS) finden Sie in dem zum Download verfügbaren E-Book: [Deploy Agisoft PhotoScan on Azure With Avere vFXT for Azure or BeeGFS](https://azure.microsoft.com/en-us/resources/deploy-agisoft-photoscan-on-azure-with-azere-vfxt-for-azure-or-beegfs/) (Bereitstellen von Agisoft PhotoScan in Azure mit Avere vFXT for Azure oder BeeGFS)

## <a name="related-resources"></a>Zugehörige Ressourcen

In den folgenden Ressourcen finden Sie weitere Informationen zu den in diesem Szenario verwendeten Komponenten und alternative Ansätze für das Batchcomputing in Azure.

- Übersicht über [Avere vFXT for Azure](/azure/avere-vfxt/avere-vfxt-overview)
- Homepage von [Agisoft PhotoScan](https://www.agisoft.com/)
- [Checkliste zu Leistung und Skalierbarkeit von Microsoft Azure Storage](/azure/storage/common/storage-performance-checklist)
- [Parallel Virtual File Systems on Microsoft Azure: Performance tests of Lustre, GlusterFS, and BeeGFS](https://azure.microsoft.com/mediahandler/files/resourcefiles/parallel-virtual-file-systems-on-microsoft-azure/Parallel_Virtual_File_Systems_on_Microsoft_Azure.pdf) (Parallele virtuelle Dateisysteme in Microsoft Azure: Leistungstests von Lustre, GlusterFS und BeeGFS (PDF))
- Beispielszenario für einen [CAE-Dienst in Azure](/azure/architecture/example-scenario/apps/hpc-saas)
- Homepage von [HPC in Azure](https://azure.microsoft.com/en-us/solutions/high-performance-computing/)
- Übersicht über [Big Compute: HPC &amp; Microsoft Batch](https://azure.microsoft.com/en-us/solutions/big-compute/)