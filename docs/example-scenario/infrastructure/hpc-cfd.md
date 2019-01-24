---
title: Ausführen von CFD-Simulationen
titleSuffix: Azure Example Scenarios
description: Führen Sie Simulationen numerischer Strömungsmechaniken (Computational Fluid Dynamics, CFD) in Azure aus.
author: mikewarr
ms.date: 09/20/2018
ms.topic: example-scenario
ms.service: architecture-center
ms.subservice: example-scenario
ms.custom: fasttrack
ms.openlocfilehash: 0bd0590ccc2975481e23ac2154c4f0998e1f0877
ms.sourcegitcommit: 1b50810208354577b00e89e5c031b774b02736e2
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/23/2019
ms.locfileid: "54488476"
---
# <a name="running-computational-fluid-dynamics-cfd-simulations-on-azure"></a>Ausführen von Simulationen numerischer Strömungsmechaniken (Computational Fluid Dynamics, CFD) in Azure

Simulationen numerischer Strömungsmechaniken (Computational Fluid Dynamics, CFD) erfordern erhebliche Computezeit und spezielle Hardware. Mit zunehmender Clusterverwendung nehmen auch der Zeitaufwand für Simulationen und die Nutzung von Rastern insgesamt zu, was wiederum zu Problemen mit der begrenzten Kapazität und langen Warteschlangenzeiten führt. Das Hinzufügen von physischer Hardware kann kostspielig und nicht für die schwankende Nutzung mit variierenden Spitzen- und Tiefstwerten geeignet sein, die bei Unternehmen üblich ist. Mit Azure können viele dieser Herausforderungen ohne jeglichen Investitionsaufwand gemeistert werden.

Azure bietet die Hardware, die Sie zum Ausführen Ihrer CFD-Aufträge auf GPU- und CPU-VMs benötigen. VM-Größen, die den Remotezugriff auf den direkten Speicher (Remote Direct Memory Access, RDMA) unterstützen, verfügen über FDR InfiniBand-basierte Netzwerke, die eine MPI-Kommunikation (Message Passing Interface) mit geringer Wartezeit ermöglichen. In Kombination mit Avere vFXT, einem gruppierten Dateisystem für Unternehmen, können Kunden einen maximalen Durchsatz für Lesevorgänge in Azure sicherstellen.

Um die Erstellung, Verwaltung und Optimierung von HPC-Clustern (High Performance Computing) zu vereinfachen, kann Azure CycleCloud zum Bereitstellen von Clustern und Orchestrieren von Daten in Hybrid- und Cloudszenarien verwendet werden. CycleCloud überwacht ausstehende Aufträge und startet automatisch bedarfsbasierte Computeressourcen mit nutzungsabhängiger Bezahlung, die mit dem Workloadplaner Ihrer Wahl verbunden sind.

## <a name="relevant-use-cases"></a>Relevante Anwendungsfälle

Weitere relevante Branchen für CFD-Anwendungen sind:

- Luft- und Raumfahrt
- Automobilbau
- Heizungs-, Lüftungs- und Klimaanlagen
- Öl- und Gasanlagen
- Biowissenschaften

## <a name="architecture"></a>Architecture

![Architekturdiagramm][architecture]

Das obige Diagramm zeigt eine allgemeine Übersicht über einen typischen Hybridentwurf mit Auftragsüberwachung der On-Demand-Knoten in Azure:

1. Stellen Sie eine Verbindung mit dem Azure CycleCloud-Server her, um den Cluster zu konfigurieren.
2. Konfigurieren und erstellen Sie den Hauptknoten des Clusters mit RDMA-fähigen Computern für MPI.
3. Fügen Sie den lokalen Hauptknoten hinzu, und konfigurieren Sie ihn.
4. Wenn nicht genügend Ressourcen verfügbar sind, skaliert Azure CycleCloud Computeressourcen in Azure zentral hoch (oder herunter). Es kann ein Grenzwert vordefiniert werden, um eine Überlastung zu verhindern.
5. Aufgaben werden den Ausführungsknoten zugeordnet.
6. Daten werden in Azure vom lokalen NFS-Server zwischengespeichert.
7. Daten werden aus Avere vFXT für den Azure-Cache eingelesen.
8. Die Auftrags- und Aufgabeninformationen werden mittels Relay an den Azure CycleCloud-Server weitergeleitet.

### <a name="components"></a>Komponenten

- [Azure CycleCloud][cyclecloud] ist ein Tool zum Erstellen, Verwalten, Ausführen und Optimieren von HPC- und Big Compute-Clustern in Azure.
- [Avere vFXT in Azure][avere] wird verwendet, um ein für die Cloud konzipiertes Dateisystem für Unternehmen bereitzustellen.
- [Microsoft Azure Virtual Machines][vms] (VMs) wird verwendet, um einen statischen Satz von Computeinstanzen zu erstellen.
- [Microsoft Azure Virtual Machine Scale Sets][vmss] (VM-Skalierungsgruppen) stellt eine Gruppe von identischen VMs bereit, die von Azure CycleCloud zentral hoch- und herunterskaliert werden können.
- [Azure Storage-Konten](/azure/storage/common/storage-introduction) werden für die Synchronisierung und Datenaufbewahrung verwendet.
- [Virtuelle Netzwerke](/azure/virtual-network/virtual-networks-overview) ermöglichen vielen Azure-Ressourcentypen (beispielsweise Azure-VMs) die sichere Kommunikation miteinander sowie mit dem Internet und lokalen Netzwerken.

### <a name="alternatives"></a>Alternativen

Kunden können Azure CycleCloud auch verwenden, um ein Raster vollständig in Azure zu erstellen. Bei diesem Setup wird der Azure CycleCloud-Server in Ihrem Azure-Abonnement ausgeführt.

Für einen modernen Anwendungsansatz, bei dem die Verwaltung eines Workloadplaners nicht erforderlich ist, kann [Microsoft Azure Batch][batch] hilfreich sein. Sie können mithilfe von Azure Batch umfangreiche auf Parallelverarbeitung ausgelegte HPC-Anwendungen effizient in der Cloud ausführen. Azure Batch ermöglicht es Ihnen, die Azure-Computeressourcen für die parallele oder skalierte Ausführung Ihrer Anwendungen ohne manuelle Konfiguration oder Verwaltung der Infrastruktur zu definieren. Azure Batch plant rechenintensive Aufgaben und fügt basierend auf Ihren Anforderungen Computeressourcen dynamisch hinzu oder entfernt sie.

### <a name="scalability-and-security"></a>Skalierbarkeit und Sicherheit

Die Ausführungsknoten in Azure CycleCloud können manuell oder mittels automatischer Skalierung skaliert werden. Weitere Informationen finden Sie unter [CycleCloud Autoscaling][cycle-scale] (Automatische Skalierung in CycleCloud).

Allgemeine Informationen zum Entwerfen sicherer Lösungen finden Sie in der [Dokumentation zur Azure-Sicherheit][security].

## <a name="deploy-the-scenario"></a>Bereitstellen des Szenarios

### <a name="prerequisites"></a>Voraussetzungen

Führen Sie vor dem Bereitstellen der Resource Manager-Vorlage die folgenden Schritte aus:

1. Erstellen Sie einen [Dienstprinzipal][cycle-svcprin] zum Abrufen von „appId“ (App-ID), „displayName“ (Anzeigename), „name“ (Name), „password“ (Kennwort) and „tenant“ (Mandant).
2. Generieren Sie ein [SSH-Schlüsselpaar][cycle-ssh] für die sichere Anmeldung beim CycleCloud-Server.

    <!-- markdownlint-disable MD033 -->

    <a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FCycleCloudCommunity%2Fcyclecloud_arm%2Fmaster%2Fazuredeploy.json" target="_blank">
        <img src="https://azuredeploy.net/deploybutton.png"/>
    </a>

    <!-- markdownlint-enable MD033 -->

3. [Melden Sie sich beim CycleCloud-Server an][cycle-login], um einen neuen Cluster zu konfigurieren und zu erstellen.
4. [Erstellen Sie einen Cluster][cycle-create].

Der Avere-Cache ist eine optionale Lösung, die den Durchsatz der Lesevorgänge für die Anwendungsauftragsdaten deutlich erhöhen kann. Avere vFXT für Azure löst das Problem, das mit der Ausführung dieser HPC-Unternehmensanwendungen in der Cloud und der gleichzeitigen Nutzung von lokal oder in Azure Blob Storage gespeicherten Daten einhergeht.

Für Organisationen, die eine Hybridinfrastruktur mit lokalem Speicher und Cloud Computing planen, können HPC-Anwendungen anhand von Daten, die auf NAS-Geräten (Network Attached Storage) gespeichert sind, „Bursts“ in Azure ausführen und die erforderlichen CPUs nach Bedarf einrichten. Das Dataset wird nie vollständig in die Cloud verschoben. Die angeforderten Bytes werden während der Verarbeitung mithilfe eines Avere-Clusters vorübergehend zwischengespeichert.

Folgen Sie zum Einrichten und Konfigurieren einer Avere vFXT-Installation den Anweisungen unter [Avere Setup and Configuration guide][avere] (Leitfaden für die Avere-Einrichtung und -Konfiguration).

## <a name="pricing"></a>Preise

Die Kosten für die Ausführung einer HPC-Implementierung mit einem CycleCloud-Server sind von verschiedenen Faktoren abhängig. Beispielsweise wird CycleCloud basierend auf der genutzten Computezeit berechnet, wobei der Masterserver und der CycleCloud-Server in der Regel ständig zugeordnet sind und ausgeführt werden. Die Kosten für die Ausführungsknoten hängen davon ab, wie lange diese aktiv sind und ausgeführt werden und welche Größe verwendet wird. Die normalen Azure-Gebühren für Speicher und Netzwerk gelten ebenfalls.

Dieses Szenario zeigt, wie CFD-Anwendungen in Azure ausgeführt werden können- Die Computer erfordern daher RDMA-Funktionen, die nur für bestimmte VM-Größen verfügbar sind. Im Folgenden sind Beispiele für die Kosten aufgeführt, die in einem Monat für eine Skalierungsgruppe mit einem ausgehenden Datenverkehr von 1 TB, die kontinuierlich für acht Stunden pro Tag zugeordnet ist, anfallen können. Enthalten sind auch die Preise für den Azure CycleCloud-Server und die Installation von Avere vFXT für Azure:

- Region: Nordeuropa
- Azure CycleCloud-Server: 1x Standard D3 (vier CPUs, 14 GB Arbeitsspeicher, HDD Standard-Datenträger mit 32 GB)
- Azure CycleCloud-Masterserver: 1x Standard D12 v2 (vier CPUs, 28 GB Arbeitsspeicher, HDD Standard-Datenträger mit 32 GB)
- Azure CycleCloud-Knotenarray: 10x Standard H16r (16 CPUs, 112 GB Arbeitsspeicher)
- Avere vFXT für Azure-Cluster: 3x D16s v3 (200 GB Betriebssystem, SSD Premium-Datenträger mit 1 TB)
- Ausgehende Daten: 1 TB

Die Preisschätzung für die oben genannte Hardware finden Sie [hier][pricing].

## <a name="next-steps"></a>Nächste Schritte

Nachdem Sie das Beispiel bereitgestellt haben, können Sie sich ausführlicher über [Azure CycleCloud][cyclecloud] informieren.

## <a name="related-resources"></a>Zugehörige Ressourcen

- [RDMA-fähige Instanzen][rdma]
- [Anpassen einer VM für RDMA-fähige Instanzen][rdma-custom]

<!-- links -->
[architecture]: ./media/architecture-hpc-cfd.png
[calculator]: https://azure.com/e/
[availability]: /azure/architecture/checklist/availability
[resource-groups]: /azure/azure-resource-manager/resource-group-overview
[resiliency]: /azure/architecture/resiliency/
[security]: /azure/security/
[scalability]: /azure/architecture/checklist/scalability
[vmss]: /azure/virtual-machine-scale-sets/overview
[cyclecloud]: /azure/cyclecloud/
[rdma]: /azure/virtual-machines/windows/sizes-hpc#rdma-capable-instances
[gpu]: /azure/virtual-machines/windows/sizes-gpu
[hpcsizes]: /azure/virtual-machines/windows/sizes-hpc
[vms]: /azure/virtual-machines/
[low-pri]: /azure/virtual-machine-scale-sets/virtual-machine-scale-sets-use-low-priority
[batch]: /azure/batch/
[avere]: https://github.com/Azure/Avere/blob/master/README.md
[cycle-prereq]: /azure/cyclecloud/quickstart-install-cyclecloud#prerequisites
[cycle-svcprin]: /azure/cyclecloud/quickstart-install-cyclecloud#service-principal
[cycle-ssh]: /azure/cyclecloud/quickstart-install-cyclecloud#ssh-keypair
[cycle-login]: /azure/cyclecloud/quickstart-install-cyclecloud#log-into-the-cyclecloud-application-server
[cycle-create]: /azure/cyclecloud/quickstart-create-and-run-cluster
[rdma]: /azure/virtual-machines/windows/sizes-hpc#rdma-capable-instances
[rdma-custom]: /azure/virtual-machines/linux/classic/rdma-cluster#customize-the-vm
[pricing]: https://azure.com/e/53030a04a2ab47a289156e2377a4247a
[cycle-scale]: /azure/cyclecloud/autoscale
