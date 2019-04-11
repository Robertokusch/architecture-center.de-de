---
title: Übersicht über Azure-Computeoptionen
titleSuffix: Azure Application Architecture Guide
description: Eine Übersicht über Azure-Computeoptionen.
author: MikeWasson
ms.date: 06/13/2018
ms.topic: guide
ms.service: architecture-center
ms.subservice: reference-architecture
ms.custom: seojan19
ms.openlocfilehash: 63754ce84f001226b6cddf1f30a152bcfce3e7ac
ms.sourcegitcommit: c053e6edb429299a0ad9b327888d596c48859d4a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/20/2019
ms.locfileid: "58245581"
---
# <a name="overview-of-azure-compute-options"></a>Übersicht über Azure-Computeoptionen

Der Begriff *Compute* bezieht sich auf das Hostingmodell für die Computeressourcen, auf denen Ihre Anwendung ausgeführt wird.

## <a name="overview"></a>Übersicht

An einem Ende des Spektrums befindet sich IaaS (**Infrastructure-as-a-Service**). Bei IaaS stellen Sie die benötigten VMs zusammen mit den zugeordneten Netzwerk- und Speicherkomponenten bereit. Anschließend stellen Sie die Software und Anwendungen bereit, die für diese VMs jeweils vorgesehen sind. Dieses Modell weist die größte Ähnlichkeit mit einer herkömmlichen lokalen Umgebung auf – mit der Ausnahme, dass die Infrastruktur von Microsoft verwaltet wird. Sie verwalten weiterhin die einzelnen VMs.

**PaaS (Platform-as-a-Service)** bietet eine verwaltete Hostingumgebung, in der Sie Ihre Anwendung ohne Verwaltung von virtuellen Computern oder Netzwerkressourcen bereitstellen können. Anstatt beispielsweise einzelne VMs zu erstellen, geben Sie eine Instanzanzahl an, und der Dienst übernimmt das Bereitstellen, Konfigurieren und Verwalten der erforderlichen Ressourcen. Azure App Service ist ein Beispiel für einen PaaS-Dienst.

Es wird ein Spektrum von IaaS bis zu PaaS in Reinform abgedeckt. Für Azure-VMs kann beispielsweise mithilfe von VM Scale Sets die automatische Skalierung durchgeführt werden. Diese Funktion für die automatische Skalierung entspricht nicht genau PaaS, aber es handelt sich um die Art von Verwaltungsfeature, das Teil eines PaaS-Diensts sein kann.

Bei FaaS (**Functions-as-a-Service**) geht dies noch weiter, da es auch nicht mehr erforderlich ist, sich um die Hostingumgebung zu kümmern. Anstatt Computeinstanzen zu erstellen und Code auf diesen Instanzen bereitzustellen, stellen Sie einfach Ihren Code bereit, der vom Dienst dann automatisch ausgeführt wird. Es ist nicht erforderlich, dass Sie die Computeressourcen verwalten. Diese Dienste verwenden eine serverlose Architektur und können nahtlos und zentral auf die Ebene hoch- oder herunterskaliert werden, die zum Verarbeiten des Datenverkehrs erforderlich ist. Bei Azure Functions handelt es sich um einen Dienst vom Typ „FaaS“.

IaaS bietet das höchste Maß an Steuerung, Flexibilität und Portabilität. FaaS bietet Einfachheit, elastische Skalierung und potenzielle Kosteneinsparungen, da Sie nur für die Zeiten zahlen, in denen Ihr Code ausgeführt wird. PaaS liegt zwischen IaaS und FaaS. Im Allgemeinen gilt Folgendes: Je mehr Flexibilität ein Dienst ermöglicht, desto größer ist Ihre Verantwortung für die Konfiguration und Verwaltung der Ressourcen. Mit FaaS-Diensten werden automatisch fast alle Aspekte der Anwendungsausführung verwaltet, während es bei IaaS-Lösungen erforderlich ist, dass Sie die von Ihnen erstellten VMs und Netzwerkkomponenten bereitstellen, konfigurieren und verwalten.

## <a name="azure-compute-options"></a>Compute-Optionen in Azure

Hier sind die wichtigsten Compute-Optionen aufgeführt, die in Azure derzeit verfügbar sind:

- [Virtual Machines](/azure/virtual-machines/) ist ein IaaS-Dienst, mit dem Sie VMs in einem virtuellen Netzwerk (VNet) bereitstellen und verwalten können.
- [App Service](/azure/app-service/app-service-value-prop-what-is) ist ein verwaltetes PaaS-Angebot zum Hosten von Web-Apps, mobilen App-Back-Ends, RESTful-APIs oder automatisierten Geschäftsprozessen.
- [Service Fabric](/azure/service-fabric/service-fabric-overview) ist eine Plattform für verteilte Systeme, die in vielen Umgebungen ausgeführt werden kann, z.B. Azure oder lokal. Service Fabric ist ein Orchestrator von Microservices in einem Cluster mit Computern.
- [Azure Kubernetes Service](/azure/aks/) verwaltet einen gehosteten Kubernetes-Dienst für die Ausführung von Containeranwendungen.
- Mit [Azure Container Instances](/azure/container-instances/container-instances-overview) lassen sich Container in Azure besonders schnell und einfach ausführen, ohne dass Sie dazu virtuelle Computer bereitstellen oder einen übergeordneten Dienst einführen müssen.
- [Azure Functions](/azure/azure-functions/functions-overview) ist ein verwalteter FaaS-Dienst.
- [Azure Batch](/azure/batch/batch-technical-overview) ist ein verwalteter Dienst zum Ausführen von umfassenden parallelen HPC-Anwendungen (High-Performance Computing).
- Bei [Cloud Services](/azure/cloud-services/cloud-services-choose-me) handelt es sich um einen verwalteten Dienst zum Ausführen von Cloudanwendungen. Hierfür wird ein PaaS-Hostingmodell verwendet.

Berücksichtigen Sie bei der Auswahl einer Compute-Option die folgenden Faktoren:

- Hostingmodell: Wie wird der Dienst gehostet? Welche Anforderungen und Einschränkungen gelten für diese Hostingumgebung?
- DevOps: Ist eine integrierte Unterstützung für Anwendungsupgrades vorhanden? Welches Bereitstellungsmodell wird verwendet?
- Skalierbarkeit: Wie wird für den Dienst das Hinzufügen oder Entfernen von Instanzen durchgeführt? Kann die automatische Skalierung basierend auf der Last und anderen Metriken erfolgen?
- Verfügbarkeit: Was umfasst die Vereinbarung zum Servicelevel (SLA) des Diensts?
- Kosten: Zusätzlich zu den Kosten des eigentlichen Diensts sollten Sie die Betriebskosten für die Verwaltung einer Lösung berücksichtigen, die für den Dienst erstellt wird. Für IaaS-Lösungen können die Betriebskosten unter Umständen höher sein.
- Welche allgemeinen Beschränkungen gelten für die einzelnen Dienste?
- Welche Art von Anwendungsarchitekturen sind für diesen Dienst geeignet?

## <a name="next-steps"></a>Nächste Schritte

Wählen Sie einen Computedienst für Ihre Anwendung mithilfe der [Entscheidungsstruktur für Azure-Computedienste](./compute-decision-tree.md) aus.

Einen ausführlicheren Vergleich von Compute-Optionen in Azure finden Sie unter [Kriterien für die Auswahl einer Azure-Compute-Option](./compute-comparison.md).
