---
title: Ein Dienst für computergestützte Entwicklung
titleSuffix: Azure Example Scenarios
description: Stellen Sie eine SaaS-Plattform (Software-as-a-Service) für computergestützte Entwicklung (Computer-Aided Engineering) in Azure bereit.
author: alexbuckgit
ms.date: 08/22/2018
ms.openlocfilehash: 0e8ce29639e4e189acef633585191b178c6c8721
ms.sourcegitcommit: bb7fcffbb41e2c26a26f8781df32825eb60df70c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/20/2018
ms.locfileid: "53643865"
---
# <a name="a-computer-aided-engineering-service-on-azure"></a>Ein CAE-Dienst in Azure

Dieses Beispielszenario veranschaulicht die Bereitstellung einer SaaS-Plattform (Software-as-a-Service), die auf den HPC-Funktionen (High Performance Computing) von Azure aufbaut. Das Szenario basiert auf einer Softwarelösung für Ingenieure. Die Architektur ist jedoch auch für andere Branchen relevant, die HPC-Ressourcen benötigen – etwa zum Rendern von Bildern, zum Erstellen komplexer Modelle oder zum Berechnen finanzieller Risiken.

In diesem Beispiel wird ein Anbieter von Ingenieurssoftware verwendet, der CAE-Anwendungen (Computer-Aided Engineering) für Ingenieurbüros und Fertigungsunternehmen bereitstellt. CAE-Lösungen ermöglichen Innovationen, beschleunigen die Entwicklung und tragen während der gesamten Lebensdauer eines Produktentwurfs zur Senkung der Kosten bei. Lösungen dieser Art haben einen erheblichen Computeressourcenbedarf und verarbeiten häufig große Datenmengen. Aufgrund der hohen Kosten lokaler HPC-Appliances oder High-End-Arbeitsstationen sind diese Technologien für kleine Ingenieurbüros sowie für Unternehmer und Studenten oft nicht erschwinglich.

Das Unternehmen möchte den Markt für seine Anwendungen erweitern und dazu eine SaaS-Plattform aufbauen, die auf cloudbasierten HPC-Technologien basiert. Die Kunden sollen für Computeressourcen nach Bedarf bezahlen können und Zugriff auf hohe Rechenleistung erhalten, die für sie andernfalls zu teuer wäre.

Das Unternehmen hat folgende Ziele:

- Nutzen von HPC-Funktionen in Azure zur Beschleunigung von Produktentwicklung und Tests
- Verwenden der neuesten Hardwareinnovationen zur Ausführung komplexer Simulationen bei gleichzeitiger Minimierung der Kosten für einfachere Simulationen
- Ermöglichen von naturgetreuen Visualisierungen und naturgetreuem Rendering in einem Webbrowser ohne High-End-Arbeitsstation für Ingenieure

## <a name="relevant-use-cases"></a>Relevante Anwendungsfälle

Zu den weiteren relevanten Anwendungsfällen zählen:

- Genomforschung
- Wettersimulation
- Computergestützte Chemieanwendungen

## <a name="architecture"></a>Architecture

![Architektur für eine SaaS-Lösung mit HPC-Funktionen][architecture]

- Benutzer können unter Verwendung des [Apache Guacamole-Diensts](https://guacamole.apache.org/) über einen Browser mit einer HTML5-basierten RDP-Verbindung auf virtuelle Computer der NV-Serie zugreifen. Diese VM-Instanzen bieten leistungsstarke GPUs für Rendering und Zusammenarbeit. Benutzer können ihre Entwürfe ohne mobile High-End-Computergeräte oder -Laptops bearbeiten und die Ergebnisse anzeigen. Der Planer startet zusätzliche virtuelle Computer auf der Grundlage einer benutzerdefinierten Heuristik.
- Benutzer können in einer CAD-Desktopsitzung Workloads für die Ausführung in verfügbaren HPC-Clusterknoten übermitteln. Mit diesen Workloads werden Aufgaben wie etwa Spannungsanalysen oder Berechnungen zur Fluiddynamik ausgeführt, sodass keine dedizierten lokalen Computecluster mehr benötigt werden. Die Clusterknoten können mit einer automatischen Skalierung konfiguriert werden, die auf der Auslastung oder auf der Warteschlangenlänge basiert (abhängig vom Computeressourcenbedarf der aktiven Benutzer).
- Azure Kubernetes Service (AKS) wird verwendet, um die Webressourcen zu hosten, die für Endbenutzer zur Verfügung stehen.

### <a name="components"></a>Komponenten

- [Virtuelle Computer der H-Serie](/azure/virtual-machines/linux/sizes-hpc) werden verwendet, um rechenintensive Simulationen wie molekulare Modellierungen und Berechnungen zur Fluiddynamik auszuführen. Die Lösung nutzt auch Technologien wie RDMA-Verbindungen (Remote Direct Memory Access) und InfiniBand-Netzwerkfunktionen.
- [Virtuelle Computer der NV-Serie](/azure/virtual-machines/windows/sizes-gpu) bieten Ingenieuren Funktionen von High-End-Arbeitsstationen in einem Standard-Webbrowser. Diese virtuellen Computer verfügen über NVIDIA Tesla M60-GPUs, die erweitertes Rendering unterstützen und Workloads mit einfacher Genauigkeit ausführen können.
- [Universelle virtuelle Computer](/azure/virtual-machines/linux/sizes-general) mit CentOS werden für traditionellere Workloads verwendet (beispielsweise für Webanwendungen).
- [Application Gateway](/azure/application-gateway/overview) nimmt einen Lastausgleich für die Anforderungen vor, die bei den Webservern eingehen.
- [Azure Kubernetes Service (AKS)](/azure/aks/intro-kubernetes) wird verwendet, um für Simulationen, die ohne die High-End-Funktionen von HPC- oder GPU-VMs auskommen, günstigere skalierbare Workloads auszuführen.
- [Altair PBS Works Suite](https://www.pbsworks.com/PBSProduct.aspx?n=PBS-Works-Suite&c=Overview-and-Capabilities) orchestriert den HPC-Workflow und stellt sicher, dass genügend VM-Instanzen verfügbar sind, um die aktuelle Auslastung zu bewältigen. Darüber hinaus hebt diese Komponente bei geringerem Bedarf die Zuordnung virtueller Computer auf, um Kosten zu sparen.
- [Blob Storage](/azure/storage/blobs/storage-blobs-introduction) speichert Dateien, die die geplanten Aufträge unterstützen.

### <a name="alternatives"></a>Alternativen

- [Azure CycleCloud](/azure/cyclecloud/overview) vereinfacht das Erstellen, Verwalten, Betreiben und Optimieren von HPC-Clustern. Die Lösung bietet erweiterte Richtlinien- und Governancefeatures. CycleCloud unterstützt jeden beliebigen Auftragsplaner oder Softwarestapel.
- [HPC Pack](/azure/virtual-machines/windows/hpcpack-cluster-options) kann einen Azure-HPC-Cluster für Windows Server-basierte Workloads erstellen und verwalten. Für Linux-basierte Workloads ist HPC Pack dagegen nicht geeignet.
- [Azure Automation State Configuration](/azure/automation/automation-dsc-overview) bietet einen Infrastructure-as-Code-Ansatz zum Definieren der bereitzustellenden virtuellen Computer und Software. VMs können als Teil einer VM-Skalierungsgruppe mit Regeln für die automatische Skalierung für Computeknoten auf der Grundlage der Anzahl von Aufträgen bereitgestellt werden, die an die Auftragswarteschlange übermittelt werden. Wird ein neuer virtueller Computer benötigt, wird er unter Verwendung des neuesten gepatchten Images aus dem Azure-Imagekatalog bereitgestellt. Anschließend wird die benötigte Software installiert und über ein PowerShell DSC-Konfigurationsskript konfiguriert.
- [Azure-Funktionen](/azure/azure-functions/functions-overview)

## <a name="considerations"></a>Überlegungen

- Der Infrastructure-as-Code-Ansatz eignet sich zwar bestens für die Verwaltung von VM-Builddefinitionen, die skriptbasierte Bereitstellung eines neuen virtuellen Computers nimmt jedoch mitunter viel Zeit in Anspruch. In dieser Lösung wurde ein guter Kompromiss gefunden: Mithilfe des DSC-Skripts wird in regelmäßigen Abständen ein optimiertes Image („Golden Image“) erstellt. Auf der Grundlage dieses Skripts kann ein neuer virtueller Computer schneller erstellt werden als wenn bei Bedarf ein virtueller Computer vollständig per DSC erstellt werden muss. Azure DevOps Services und andere CI/CD-Tools können optimierte Images in regelmäßigen Abständen unter Verwendung von DSC-Skripts aktualisieren.
- Ein zentraler Aspekt ist die Erzielung eines ausgewogenen Verhältnisses zwischen den Gesamtkosten der Lösung und der schnellen Verfügbarkeit von Computeressourcen. Die Betriebskosten lassen sich senken, wenn Sie einen Pool mit VM-Instanzen der N-Serie bereitstellen und in einen Zustand mit aufgehobener Zuordnung versetzen. Wird ein zusätzlicher virtueller Computer benötigt, muss er zur erneuten Zuordnung auf einem anderen Host hochgefahren werden. Allerdings entfällt dabei die Zeit für die PCI-Bus-Erkennung, die das Betriebssystem benötigt, um Treiber für die GPU zu identifizieren und zu installieren, da ein virtueller Computer mit aufgehobener Zuordnung bei der erneuten Zuordnung nach dem Neustart den gleichen PCI-Bus für die GPU verwendet.
- In der ursprünglichen Architektur werden zum Ausführen von Simulationen ausschließlich virtuelle Azure-Computer verwendet. Um die Kosten für Workloads zu senken, die nicht alle Funktionen eines virtuellen Computers benötigen, wurden diese Workloads Containern hinzugefügt und in Azure Kubernetes Service (AKS) bereitgestellt.
- Die Mitarbeiter des Unternehmens hatten bereits Erfahrung mit Open-Source-Technologien. Sie können von dieser Erfahrung profitieren und auf Technologien wie Linux und Kubernetes aufbauen.

## <a name="pricing"></a>Preise

Um Ihnen die Ermittlung der Betriebskosten für dieses Szenario zu erleichtern, wurden viele der benötigten Dienste in einem [Kostenrechnerbeispiel][calculator] vorkonfiguriert. Die Kosten Ihrer Lösung hängen von Anzahl und Umfang der Dienste ab, die für Ihre Anforderungen erforderlich sind.

Folgende Aspekte haben erheblichen Einfluss auf die Kosten für diese Lösung:

- Die Kosten für virtuelle Azure-Computer steigen linear, wenn weitere Instanzen bereitgestellt werden. Für virtuelle Computer mit aufgehobener Bereitstellung fallen nur Speicherkosten und keine Computekosten an. Diese Computer mit aufgehobener Zuordnung können bei entsprechend hohem Bedarf neu zugeordnet werden.
- Azure Kubernetes Service-Kosten basieren auf dem VM-Typ, der zur Unterstützung der Workload gewählt wurde. Die Kosten steigen linear und sind abhängig von der Anzahl virtueller Computer im Cluster.

## <a name="next-steps"></a>Nächste Schritte

- Lesen Sie die [Kundengeschichte von Altair][source-document]. Dieses Beispielszenario basiert auf einer Version der Architektur dieses Unternehmens.
- Informieren Sie sich über weitere verfügbare [Big Compute-Lösungen](https://azure.microsoft.com/solutions/big-compute) in Azure.

<!-- links -->
[architecture]: ./media/architecture-hpc-saas.png
[source-document]: https://customers.microsoft.com/story/altair-manufacturing-azure
[calculator]: https://azure.com/e/3cb9ccdc893f41ffbcdb00c328178ccf
