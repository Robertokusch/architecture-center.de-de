---
title: Kriterien für die Auswahl einer Azure-Compute-Option
description: Vergleichen von Azure-Computediensten unter verschiedenen Aspekten
author: MikeWasson
ms.date: 06/13/2018
ms.openlocfilehash: b7a5b08e1d9a9eba33003b3d478a61388a496272
ms.sourcegitcommit: c4106b58ad08f490e170e461009a4693578294ea
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/10/2018
ms.locfileid: "43016083"
---
# <a name="criteria-for-choosing-an-azure-compute-service"></a>Kriterien für die Auswahl einer Azure-Compute-Option

Der Begriff *Compute* bezieht sich auf das Hostingmodell für die Computeressourcen, auf denen Ihre Anwendungen ausgeführt werden. Die folgenden Tabellen vergleichen Azure-Computedienste unter verschiedenen Aspekten miteinander. Ziehen Sie diese Tabellen zu Rate, wenn Sie eine Compute-Option für Ihre Anwendung auswählen.

## <a name="hosting-model"></a>Hostingmodell

| Kriterien | Virtual Machines | App Service | Service Fabric | Azure-Funktionen | Azure Kubernetes Service | Container Instances | Azure Batch |
|----------|-----------------|-------------|----------------|-----------------|-------------------------|----------------|-------------|
| Anwendungskomposition | Agnostisch | Anwendungen, Container | Dienste, ausführbare Gastdateien, Container | Functions | Container | Container | Geplante Aufträge  |
| Dichte | Agnostisch | Mehrere Apps pro Instanz über App Service-Pläne | Mehrere Dienste pro VM | Serverlos <a href="#note1"><sup>1</sup></a> | Mehrere Container pro Knoten |Keine dedizierten Instanzen | Mehrere Apps pro VM |
| Mindestanzahl von Knoten | 1<a href="#note2"><sup>2</sup></a>  | 1 | 5<a href="#note3"><sup>3</sup></a> | Serverlos <a href="#note1"><sup>1</sup></a> | 3 <a href="#note3"><sup>3</sup></a> | Keine dedizierten Knoten | 1<a href="#note4"><sup>4</sup></a> |
| Zustandsverwaltung | Zustandslos oder zustandsbehaftet | Zustandslos | Zustandslos oder zustandsbehaftet | Zustandslos | Zustandslos oder zustandsbehaftet | Zustandslos | Zustandslos |
| Webhosting | Agnostisch | Integriert | Agnostisch | Nicht zutreffend | Agnostisch | Agnostisch | Nein  |
| Bereitstellung in dediziertem VNET möglich? | Unterstützt | Unterstützt<a href="#note5"><sup>5</sup></a> | Unterstützt | Unterstützt<a href="#note5"><sup>5</sup></a> | [Unterstützt](/azure/aks/networking-overview) | Nicht unterstützt | Unterstützt |
| Hybridkonnektivität | Unterstützt | Unterstützt<a href="#note6"><sup>6</sup></a>  | Unterstützt | Unterstützt<a href="#node7"><sup>7</sup></a> | Unterstützt | Nicht unterstützt | Unterstützt |

Notizen

1. <span id="note1">Bei Verwendung eines Verbrauchsplans. Bei Verwendung eines App Service-Plans werden Funktionen auf den VMs ausgeführt, die Ihrem App Service-Plan zugeordnet sind. Weitere Informationen finden Sie unter [Vergleich von Hostingplänen für Azure Functions][function-plans].</span>
2. <span id="note2">Höhere SLA mit mindestens zwei Instanzen.</span>
3. <span id="note3">Empfohlen für Produktionsumgebungen.</span>
4. <span id="note4">Kann nach Abschluss eines Auftrags zentral auf 0 herunterskaliert werden.</span>
5. <span id="note5">Erfordert App Service-Umgebung (App Service Environment, ASE).</span>
6. <span id="note6">Verwenden Sie [Azure App Service-Hybridverbindungen][app-service-hybrid].</span>
7. <span id="note7">Erfordert einen App Service-Plan.</span>

## <a name="devops"></a>DevOps

| Kriterien | Virtual Machines | App Service | Service Fabric | Azure-Funktionen | Azure Kubernetes Service | Container Instances | Azure Batch |
|----------|-----------------|-------------|----------------|-----------------|-------------------------|----------------|-------------|
| Lokales Debugging | Agnostisch | IIS Express, weitere<a href="#note1b"><sup>1</sup></a> | Lokaler Knotencluster | Visual Studio oder Azure Functions-Befehlszeilenschnittstelle | Minikube, andere | Lokale Containerruntime | Nicht unterstützt |
| Programmiermodell | Agnostisch | Web- und API-Anwendungen, WebJobs für Hintergrundtasks | Ausführbare Gastdatei, Dienstmodell, Akteurmodell, Container | Funktionen mit Auslösern | Agnostisch | Agnostisch | Befehlszeilenanwendung |
| Anwendungsupdate | Keine integrierte Unterstützung | Bereitstellungsslots | Rollierendes Upgrade (pro Dienst) | Bereitstellungsslots | Paralleles Update | Nicht zutreffend |

Notizen

1. <span id="note1b">Optionen umfassen IIS Express für ASP.NET oder node.js (iisnode); PHP-Webserver; Azure Toolkit für IntelliJ, Azure Toolkit für Eclipse. App Service unterstützt auch das Remotedebuggen einer bereitgestellten Web-App.</span>
2. <span id="note2b">Informationen dazu finden Sie unter [Ressourcenanbieter und -typen][resource-manager-supported-services].</span> 


## <a name="scalability"></a>Skalierbarkeit

| Kriterien | Virtual Machines | App Service | Service Fabric | Azure-Funktionen | Azure Kubernetes Service | Container Instances | Azure Batch |
|----------|-----------------|-------------|----------------|-----------------|-------------------------|----------------|-------------|
| Automatische Skalierung | VM-Skalierungsgruppen | Integrierter Dienst | VM Scale Sets | Integrierter Dienst | Nicht unterstützt | Nicht unterstützt | N/V |
| Load Balancer | Azure Load Balancer | Integriert | Azure Load Balancer | Integriert | Integriert |  Keine integrierte Unterstützung | Azure Load Balancer |
| Skalierungslimit<a href="#note1c"><sup>1</sup></a> | Plattformimage: 1.000 Knoten pro VMSS, benutzerdefiniertes Image: 100 Knoten pro VMSS | 20 Instanzen, 100 mit App Service-Umgebung | 100 Knoten pro VMSS | 200 Instanzen pro Funktionen-App | 100 Knoten pro Cluster (Standardgrenzwert) |20 Containergruppen pro Abonnement (Standardgrenzwert) | 20 Kerne (Standardgrenzwert) |

Notizen

1. <span id="note1c">Weitere Informationen finden Sie unter [Einschränkungen für Azure-Abonnements und Dienste, Kontingente und Einschränkungen](/azure/azure-subscription-service-limits).</span>

## <a name="availability"></a>Verfügbarkeit

| Kriterien | Virtual Machines | App Service | Service Fabric | Azure-Funktionen | Azure Kubernetes Service | Container Instances | Azure Batch |
|----------|-----------------|-------------|----------------|-----------------|-------------------------|----------------|-------------|
| SLA | [SLA für virtuelle Computer][sla-vm] | [SLA für App Service][sla-app-service] | [SLA für Service Fabric][sla-sf] | [SLA für Functions][sla-functions] | [SLA für AKS][sla-acs] | [SLA für Container Instances](https://azure.microsoft.com/support/legal/sla/container-instances/) | [SLA für Azure Batch][sla-batch] |
| Failover über mehrere Regionen | Traffic Manager | Traffic Manager | Traffic Manager, regionsübergreifender Cluster | Nicht unterstützt  | Traffic Manager | Nicht unterstützt | Nicht unterstützt |

## <a name="other"></a>Andere

| Kriterien | Virtual Machines | App Service | Service Fabric | Azure-Funktionen | Azure Kubernetes Service | Container Instances | Azure Batch |
|----------|-----------------|-------------|----------------|-----------------|-------------------------|----------------|-------------|
| SSL | In VM konfiguriert | Unterstützt | Unterstützt  | Unterstützt | [Eingangscontroller](/azure/aks/ingress) | Verwenden eines [Sidecar](../../patterns/sidecar.md)-Containers | Unterstützt |
| Kosten | [Windows][cost-windows-vm], [Linux][cost-linux-vm] | [App Service – Preise][cost-app-service] | [Service Fabric – Preise][cost-service-fabric] | [Azure Functions – Preise][cost-functions] | [AKS – Preise][cost-acs] | [Container Instances – Preise](https://azure.microsoft.com/pricing/details/container-instances/) | [Azure Batch – Preise][cost-batch]
| Geeignete Architekturstile | [N-schichtig][n-tier], [Big Compute][big-compute] (HPC) | [Web-Warteschlange-Worker][w-q-w], [n-schichtig][n-tier] | [Microservices][microservices], [ereignisgesteuerte Architektur][event-driven] | [Microservices][microservices], [ereignisgesteuerte Architektur][event-driven] | [Microservices][microservices], [ereignisgesteuerte Architektur][event-driven] | [Microservices][microservices], Automatisierung von Aufgaben, Batchaufträge  | [Big Compute][big-compute] (HPC) |

[cost-linux-vm]: https://azure.microsoft.com/pricing/details/virtual-machines/linux/
[cost-windows-vm]: https://azure.microsoft.com/pricing/details/virtual-machines/windows/
[cost-app-service]: https://azure.microsoft.com/pricing/details/app-service/
[cost-service-fabric]: https://azure.microsoft.com/pricing/details/service-fabric/
[cost-functions]: https://azure.microsoft.com/pricing/details/functions/
[cost-acs]: https://azure.microsoft.com/pricing/details/kubernetes-service/
[cost-batch]: https://azure.microsoft.com/pricing/details/batch/

[function-plans]: /azure/azure-functions/functions-scale
[sla-acs]: https://azure.microsoft.com/support/legal/sla/kubernetes-service
[sla-app-service]: https://azure.microsoft.com/support/legal/sla/app-service/
[sla-batch]: https://azure.microsoft.com/support/legal/sla/batch/
[sla-functions]: https://azure.microsoft.com/support/legal/sla/functions/
[sla-sf]: https://azure.microsoft.com/support/legal/sla/service-fabric/
[sla-vm]: https://azure.microsoft.com/support/legal/sla/virtual-machines/

[resource-manager-supported-services]: /azure/azure-resource-manager/resource-manager-supported-services
[scale-acs]: /azure/container-service/kubernetes/container-service-scale#scaling-considerations

[n-tier]: ../architecture-styles/n-tier.md
[w-q-w]: ../architecture-styles/web-queue-worker.md
[microservices]: ../architecture-styles/microservices.md
[event-driven]: ../architecture-styles/event-driven.md
[big-date]: ../architecture-styles/big-data.md
[big-compute]: ../architecture-styles/big-compute.md

[app-service-hybrid]: /azure/app-service/app-service-hybrid-connections
