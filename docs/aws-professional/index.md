---
title: Azure für AWS-Spezialisten
titleSuffix: Azure Architecture Center
description: Sie lernen die Grundlagen der Konten, Plattform und Dienste von Microsoft Azure kennen. Zudem lernen Sie die wichtigsten Gemeinsamkeiten und Unterschiede zwischen den Plattformen AWS und Azure kennen. Sie wenden Ihre Erfahrungen zu AWS in Azure an.
keywords: AWS experts, Azure comparison, AWS comparison, difference between azure and aws, azure and aws
author: lbrader
ms.date: 09/19/2018
ms.topic: article
ms.service: architecture-center
ms.subservice: cloud-fundamentals
ms.custom: seodec18
ms.openlocfilehash: 6e44953cccf39cc40be775f4043bcf8b1b890dae
ms.sourcegitcommit: 579c39ff4b776704ead17a006bf24cd4cdc65edd
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/17/2019
ms.locfileid: "59641160"
---
# <a name="azure-for-aws-professionals"></a>Azure für AWS-Spezialisten

Dieser Artikel ist für AWS-Experten (Amazon Web Services) bestimmt und soll ihnen die Grundlagen zu Konten, Plattform und Diensten von Microsoft Azure vermitteln. Zudem werden die wichtigsten Gemeinsamkeiten und Unterschiede zwischen der AWS- und Azure-Plattform erläutert.

Sie lernen Folgendes:

- Organisation der Konten und Ressourcen in Azure
- Struktur der verfügbaren Lösungen in Azure
- Unterschiede zwischen den wichtigsten Azure- und AWS-Diensten

Die Funktionalität von Azure und AWS hat sich im Laufe der Zeit unabhängig voneinander weiterentwickelt, sodass bedeutende Unterschiede zwischen diesen hinsichtlich Implementierung und Entwurf bestehen.

## <a name="overview"></a>Übersicht

Wie AWS basiert auch Microsoft Azure auf einer zentralen Gruppe von Compute-, Speicher-, Datenbank- und Netzwerkdiensten. In der Vielzahl der Fälle herrscht bei beiden Plattformen eine grundsätzliche Äquivalenz zwischen den angebotenen Produkten und Diensten. Sowohl mit AWS als auch Azure können hochverfügbare Lösungen basierend auf Windows- oder Linux-Hosts erstellt werden. Wenn Sie also mit der Entwicklung mit Linux- und OSS-Technologien vertraut sind, so sind beide Plattformen geeignet.

Die Funktionalität beider Plattformen weist zwar Ähnlichkeiten auf, allerdings sind die Ressourcen, die diese Funktionalität bereitstellen, häufig unterschiedlich organisiert. Zwischen den Diensten, die für die Erstellung einer Lösung erforderlich sind, herrscht nicht immer eine 100%ige Übereinstimmung. Zudem kann es vorkommen, dass ein bestimmter Dienst nur auf einer Plattform angeboten wird, jedoch nicht auf der anderen. Weitere Informationen finden Sie in den [Tabellen zum Vergleich zwischen Azure- und AWS-Diensten](./services.md).

## <a name="accounts-and-subscriptions"></a>Konten und Abonnements

Azure-Dienste können abhängig von der Größe und den Anforderungen Ihres Unternehmens mit verschiedenen Preisoptionen erworben werden. Weitere Informationen finden Sie auf der Seite [Azure-Preise](https://azure.microsoft.com/pricing/).

[Azure-Abonnements](/azure/virtual-machines/linux/infrastructure-example) stellen eine Gruppierung von Ressourcen mit einem zugewiesenen Besitzer dar, der für die Verwaltung der Abrechnung und Berechtigungen zuständig ist. Im Gegensatz zu AWS, bei dem alle unter dem jeweiligen AWS-Konto erstellten Ressourcen mit diesem Konto verknüpft sind, sind Abonnements nicht von den jeweiligen Besitzerkonten abhängig und können bei Bedarf neuen Besitzern zugewiesen werden.

<!-- markdownlint-disable MD033 -->

![Vergleich der Struktur und des Besitzes von AWS-Konten und Azure-Abonnements](./images/azure-aws-account-compare.png "Vergleich der Struktur und des Besitzes von AWS-Konten und Azure-Abonnements")
<br/>*Vergleich der Struktur und des Besitzes von AWS-Konten und Azure-Abonnements*
<br/><br/>

<!-- markdownlint-enable MD033 -->

Abonnements sind drei Arten von Administratorkonten zugewiesen:

- **Kontoadministrator** Der Abonnementbesitzer und das Konto, für das die im Abonnement verwendeten Ressourcen in Rechnung gestellt werden. Eine Änderung des Kontoadministrators ist nur durch Übertragung des Abonnementbesitzes möglich.

- **Dienstadministrator** Dieses Konto ist mit Rechten zum Erstellen und Verwalten von Ressourcen im Abonnement ausgestattet, jedoch nicht zum Verwalten der Abrechnung. Standardmäßig sind der Konto- und Dienstadministrator demselben Konto zugeordnet. Der Kontoadministrator kann dem Dienstadministratorkonto einen separaten Benutzer für die Verwaltung technischer und betrieblicher Aufgaben eines Abonnements zuweisen. Pro Abonnement gibt es nur einen Dienstadministrator.

- **Co-Administrator** Einem Abonnement können mehrere Co-Administratorkonten zugewiesen sein. Co-Administratoren besitzen die vollständige Kontrolle über Abonnementsressourcen und Benutzer, können jedoch nicht den Dienstadministrator ändern.

Unterhalb der Abonnementebene können bestimmten Ressourcen auch Benutzerrollen und individuelle Berechtigungen zugewiesen werden, ähnlich wie bei der Vergabe von Berechtigungen an IAM-Benutzer und -Gruppen in AWS. In Azure sind alle Benutzerkonten entweder mit einem Microsoft-Konto oder einem Geschäfts-, Schul- oder Uni-Konto (einem mit Azure Active Directory verwalteten Konto) verknüpft.

Wie bei AWS-Konten gelten für Abonnements standardmäßige Dienstkontingente und -einschränkungen. Eine vollständige Liste zu diesen Einschränkungen finden Sie unter [Einschränkungen für Azure-Abonnements und Dienste, Kontingente und Einschränkungen](/azure/azure-subscription-service-limits).
Diese Einschränkungen können bis zum Maximalwert erhöht werden, wenn Sie eine entsprechende [Supportanfrage im Verwaltungsportal stellen](https://blogs.msdn.microsoft.com/girishp/2015/09/20/increasing-core-quota-limits-in-azure/).

### <a name="see-also"></a>Weitere Informationen

- [Hinzufügen oder Ändern von Azure-Administratorrollen](/azure/billing/billing-add-change-azure-subscription-administrator)

- [Herunterladen von Azure-Rechnungen und täglichen Nutzungsdaten](/azure/billing/billing-download-azure-invoice-daily-usage-date)

## <a name="resource-management"></a>Ressourcenverwaltung

Der Begriff „Ressource“ wird in Azure und AWS deckungsgleich verwendet und bezeichnet Serverinstanzen, Speicherobjekte, Netzwerkgeräte oder sonstige Entitäten, die Sie innerhalb der Plattform erstellen oder konfigurieren können.

Azure-Ressourcen werden mit einem dieser beiden Modelle bereitgestellt und verwaltet: [Azure Resource Manager](/azure/azure-resource-manager/resource-group-overview) oder dem älteren [klassischen Azure-Bereitstellungsmodell](/azure/azure-resource-manager/resource-manager-deployment-model). Neue Ressourcen werden mithilfe des Resource Manager-Modells erstellt.

### <a name="resource-groups"></a>Ressourcengruppen

Sowohl Azure als auch AWS verfügen über Entitäten zur Organisation von Ressourcen, die als „Ressourcengruppen“ bezeichnet werden (z.B. VMs, Speicher, virtuelle Netzwerkgeräte). [Azure-Ressourcengruppen](/azure/virtual-machines/windows/infrastructure-example) lassen sich jedoch nicht direkt mit AWS-Ressourcengruppen vergleichen.

Während eine Ressource bei AWS mehreren Ressourcengruppen zugeteilt werden kann, ist eine Azure-Ressource immer nur mit einer Ressourcengruppe verknüpft. Eine Ressource, die in einer Ressourcengruppe erstellt wird, kann in eine andere Gruppe verschoben werden, sich jedoch nur in jeweils einer Ressourcengruppe befinden. Ressourcengruppen zählen zu den grundlegenden Gruppierungsmethoden in Azure Resource Manager.

Ressourcen können auch mithilfe von [Tags](/azure/azure-resource-manager/resource-group-using-tags) organisiert werden. Bei Tags handelt es sich um Schlüssel-Wert-Paare, mit denen Sie Ressourcen unabhängig von ihrer Zugehörigkeit zu einer Ressourcengruppe in Ihrem Abonnement gruppieren können.

### <a name="management-interfaces"></a>Verwaltungsoberflächen

Azure bietet verschiedene Möglichkeiten zur Verwaltung Ihrer Ressourcen:

- [Weboberfläche](/azure/azure-resource-manager/resource-group-portal):
    Wie das AWS-Dashboard bietet auch das Azure-Portal eine vollständige webbasierte Verwaltungsoberfläche für Azure-Ressourcen.

- [REST-API](/rest/api/):
    Die Azure Resource Manager-REST-API ermöglicht einen programmgesteuerten Zugriff auf die meisten im Azure-Portal verfügbaren Funktionen.

- [Befehlszeile](/azure/azure-resource-manager/cli-azure-resource-manager):
    Das Tool Azure CLI 2.0 verfügt über eine Befehlszeilenschnittstelle, mit der Azure-Ressourcen erstellt und verwaltet werden können. Die Azure CLI ist für [Windows, Linux und Mac OS](https://aka.ms/azurecli2) verfügbar.

- [PowerShell](/azure/azure-resource-manager/powershell-azure-resource-manager):
    Mit den Azure-Modulen für PowerShell können Sie anhand eines Skripts automatisierte Verwaltungsaufgaben ausführen. PowerShell ist für [Windows, Linux und Mac OS](https://github.com/PowerShell/PowerShell) verfügbar.

- [Vorlagen](/azure/azure-resource-manager/resource-group-authoring-templates):
    Azure Resource Manager-Vorlagen bieten ähnliche auf JSON-Vorlagen basierende Ressourcenverwaltungsfunktionen wie der AWS CloudFormation-Dienst.

Bei beiden Schnittstellen spielen Ressourcengruppen eine zentrale Rolle für die Art und Weise, wie Azure-Ressourcen erstellt, bereitgestellt oder geändert werden. Dies lässt sich mit der Rolle eines „Stapels“ vergleichen, die dieser bei der Gruppierung von AWS-Ressourcen bei CloudFormation-Bereitstellungen spielt.

Die Syntax und Struktur dieser Schnittstellen unterscheiden sich von ihren Pendants in AWS, bieten jedoch eine vergleichbare Funktionalität. Darüber hinaus sind viele Verwaltungstools von Drittanbietern, die in AWS verwendet werden (z.B. [Terraform von Hashicorp](https://www.terraform.io/docs/providers/azurerm/) und [Netflix Spinnaker](https://www.spinnaker.io/)), auch in Azure verfügbar.

<!-- markdownlint-disable MD024 -->

### <a name="see-also"></a>Weitere Informationen

- [Richtlinien für Azure-Ressourcengruppen](/azure/azure-resource-manager/resource-group-overview#resource-groups)

## <a name="regions-and-zones-high-availability"></a>Regionen und Zonen (Hochverfügbarkeit)

Fehler und Ausfälle können mit unterschiedlichen Auswirkungen verbunden sein. Einige Hardwarefehler, z.B. ein ausgefallener Datenträger, wirken sich ggf. nur auf einen einzelnen Hostcomputer aus. Ein fehlerhafter Netzwerkswitch kann sich auf ein gesamtes Serverrack auswirken. Weniger häufig treten Fehler auf, die zu Störungen für ein gesamtes Rechenzentrum führen, z.B. zu einem Stromausfall im Rechenzentrum. In selten Fällen kann es vorkommen, dass eine gesamte Region nicht mehr verfügbar ist.

Eines der wichtigsten Verfahren, mit dem für eine Anwendung die Resilienz sichergestellt werden kann, ist die Redundanz. Sie müssen diese Redundanz aber beim Entwerfen der Anwendung einplanen. Außerdem richtet sich der jeweils benötigte Redundanzgrad nach Ihren Geschäftsanforderungen – nicht für jede Anwendung ist eine regionsübergreifende Redundanz als Schutz vor einem regionalen Ausfall erforderlich. In der Regel muss ein Kompromiss zwischen höherer Redundanz und Zuverlässigkeit und einer höheren Kostensumme und Komplexität gefunden werden.

Bei AWS ist eine Region in mindestens zwei Verfügbarkeitszonen unterteilt. Eine Verfügbarkeitszone entspricht einem physisch isolierten Rechenzentrum in einer geografischen Region. Azure verfügt über eine Reihe von Features (u.a. **Verfügbarkeitsgruppen**, **Verfügbarkeitszonen** und **Regionspaare**), mit denen für eine Anwendung für alle Fehlerebenen Redundanz erzielt werden kann.

![Redundanz](../resiliency/images/redundancy.svg)

Die einzelnen Optionen sind in der folgenden Tabelle zusammengefasst:

| &nbsp; | Verfügbarkeitsgruppe | Verfügbarkeitszone | Regionspaar |
|--------|------------------|-------------------|---------------|
| Fehlerumfang | Rack | Datacenter | Region |
| Routinganforderung | Load Balancer | Zonenübergreifender Lastenausgleich | Traffic Manager |
| Netzwerklatenz | Sehr niedrig | Niedrig | Mittel bis hoch |
| Virtuelle Netzwerke  | VNet | VNet | Regionsübergreifendes VNet-Peering |

### <a name="availability-sets"></a>Verfügbarkeitsgruppen

Stellen Sie als Schutz vor lokalen Hardwarefehlern, z.B. ein Ausfall eines Datenträgers oder Netzwerkswitchs, in einer Verfügbarkeitsgruppe zwei oder mehr VMs bereit. Eine Verfügbarkeitsgruppe besteht aus mindestens zwei *Fehlerdomänen*, die eine Stromquelle und einen Netzwerkswitch gemeinsam nutzen. VMs in einer Verfügbarkeitsgruppe sind auf die Fehlerdomänen verteilt. Wenn ein Hardwarefehler eine Fehlerdomäne betrifft, kann der Netzwerkdatenverkehr so weiterhin an die VMs in den anderen Fehlerdomänen weitergeleitet werden. Weitere Informationen zu Verfügbarkeitsgruppen finden Sie unter [Verwalten der Verfügbarkeit virtueller Windows-Computer in Azure](/azure/virtual-machines/windows/manage-availability).

Wenn VM-Instanzen zu Verfügbarkeitsgruppen hinzugefügt werden, wird ihnen auch eine [Updatedomäne](https://azure.microsoft.com/documentation/articles/virtual-machines-linux-manage-availability/) zugewiesen. Eine Updatedomäne ist eine Gruppe von VMs, die gleichzeitig für geplante Wartungsereignisse festgelegt sind. Durch die Verteilung von VMs auf mehrere Updatedomänen wird sichergestellt, dass geplante Update- und Patchingereignisse jeweils nur eine Teilmenge dieser VMs betreffen.

Verfügbarkeitsgruppen sollten nach der Rolle der Instanz in Ihrer Anwendung organisiert werden, um sicherzustellen, dass eine Instanz in jeder Rolle betriebsbereit ist. Erstellen Sie beispielsweise in einer Webanwendung mit drei Ebenen separate Verfügbarkeitsgruppen für die Front-End-, die Anwendungs- und die Datenebene.

![Azure-Verfügbarkeitsgruppen für die einzelnen Anwendungsrollen](./images/three-tier-example.png "Verfügbarkeitsgruppen für die einzelnen Anwendungsrollen")

### <a name="availability-zones"></a>Verfügbarkeitszonen

Eine [Verfügbarkeitszone](/azure/availability-zones/az-overview) ist eine physisch getrennte Zone in einer Azure-Region. Jede Verfügbarkeitszone verfügt über eine eigene Stromquelle, ein Netzwerk und eine Kühlung. Die Bereitstellung von VMs über Verfügbarkeitszonen hinweg dient dem Schutz einer Anwendung vor Ausfällen, die ein gesamtes Rechenzentrum betreffen.

### <a name="paired-regions"></a>Regionspaare

Um eine Anwendung vor einem regionalen Ausfall zu schützen, können Sie sie in mehreren Regionen bereitstellen, indem Sie [Azure Traffic Manager][traffic-manager] zum Verteilen von Internetdatenverkehr auf die verschiedenen Regionen verwenden. Jede Azure-Region ist mit einer anderen Region gekoppelt. Zusammen bilden sie ein [Regionspaar][paired-regions]. Mit Ausnahme von „Brasilien, Süden“ befinden sich die Regionen der Regionspaare immer innerhalb des gleichen geografischen Gebiets, um steuerliche und rechtliche Anforderungen an den Speicherort von Daten zu erfüllen.

Im Gegensatz zu Verfügbarkeitszonen, bei denen es sich um physisch getrennte Rechenzentren handelt, die sich jedoch in relativ nahe gelegenen geografischen Gebieten befinden können, liegen Regionspaare in der Regel mindestens 480 km voneinander entfernt. Dadurch wird sichergestellt, dass sich größere Katastrophen nur auf eine der Regionen eines Paars auswirken. Bei benachbarten Paaren können Datenbank- und Speicherdienstdaten synchronisiert werden, und sie sind so konfiguriert, dass Plattformupdates jeweils nur in einer Region eines Paares ausgeführt werden.

[Georedundanter Azure-Speicher](/azure/storage/common/storage-redundancy-grs) wird automatisch im entsprechenden Regionspaar gesichert. Bei allen anderen Ressourcen heißt die Erstellung einer vollständig redundanten Lösung mit Regionspaaren, dass in beiden Regionen eine vollständige Kopie Ihrer Lösung erstellt wird.

### <a name="see-also"></a>Weitere Informationen

- [Regionen und Verfügbarkeit für virtuelle Computer in Azure](/azure/virtual-machines/linux/regions-and-availability)

- [Hochverfügbarkeit für Azure-Anwendungen](../resiliency/high-availability-azure-applications.md)

- [Failure and disaster recovery for Azure applications](../reliability/disaster-recovery.md) (Wiederherstellung bei einem Fehler oder Notfall für Azure-Anwendungen)

- [Geplante Wartung für virtuelle Linux-Computer in Azure](/azure/virtual-machines/linux/maintenance-and-updates)

## <a name="services"></a>Dienste

Informationen zur Zuordnung von Diensten zwischen verschiedenen Plattformen finden Sie unter [Vergleich von AWS mit Azure-Diensten](./services.md).

Es sind nicht alle Azure-Produkte und -Dienste in allen Regionen verfügbar. Weitere Informationen finden Sie auf der Seite [Produkte nach Region](https://azure.microsoft.com/global-infrastructure/services/). Auf der Seite [Vereinbarungen zum Servicelevel (SLAs)](https://azure.microsoft.com/support/legal/sla/) finden Sie die Richtlinien zu Garantien für Betriebszeiten und Downtimegutschriften für die einzelnen Azure-Produkte oder -Dienste.

Die folgenden Abschnitte enthalten einen kurzen Überblick über die Unterschiede häufig verwendeter Funktionen und Dienste zwischen der AWS- und Azure-Plattform.

### <a name="compute-services"></a>Compute Services

#### <a name="ec2-instances-and-azure-virtual-machines"></a>EC2-Instanzen und virtuelle Azure-Computer

Obwohl die Größen von AWS-Instanztypen und virtuellen Azure-Computern eine ähnliche Aufteilung aufweisen, bestehen Unterschiede in den RAM-, CPU- und Speicherkapazitäten.

- [Amazon EC2-Instanztypen](https://aws.amazon.com/ec2/instance-types/)

- [Größen für virtuelle Computer in Azure (Windows)](/azure/virtual-machines/windows/sizes)

- [Größen für virtuelle Computer in Azure (Linux)](/azure/virtual-machines/linux/sizes)

Ähnlich wie bei der sekundengenauen Abrechnung in AWS werden On-Demand-VMs von Azure sekundengenau abgerechnet.

#### <a name="ebs-and-azure-storage-for-vm-disks"></a>EBS und Azure Storage für VM-Datenträger

Durch [Datenträger](/azure/virtual-machines/linux/about-disks-and-vhds) in Blob Storage wird eine dauerhafte Datenspeicherung für Azure-VMs sichergestellt. Dies ist vergleichbar mit der Art und Weise, wie Speicherdatenträgervolumes von EC2-Instanzen in Elastic Block Store (EBS) gespeichert werden. Der [temporäre Azure-Speicher](https://blogs.msdn.microsoft.com/mast/2013/12/06/understanding-the-temporary-drive-on-windows-azure-virtual-machines/) stellt außerdem denselben temporären Lese-/Schreibspeicher mit niedriger Latenz für VMs bereit wie EC2-Instanzspeicher (auch als kurzlebige Speicher bezeichnet).

Mit [Azure Premium Storage](/azure/virtual-machines/windows/premium-storage) werden Datenträger-E/As für höhere Leistung unterstützt. Vergleichen lässt sich dies mit den Speicheroptionen mit bereitgestellten IOPS von AWS.

#### <a name="lambda-azure-functions-azure-web-jobs-and-azure-logic-apps"></a>Lambda, Azure Functions, Azure WebJobs und Azure Logic Apps

[Azure Functions](https://azure.microsoft.com/services/functions/) ist das primäre Pendant zu AWS Lambda hinsichtlich der Bereitstellung von serverlosem On-Demand-Code. Die Lambda-Funktionalität weist jedoch auch Überschneidungen mit anderen Azure-Diensten auf:

- [WebJobs](/azure/app-service/web-sites-create-web-jobs) ermöglicht die Erstellung von geplanten oder fortlaufend ausgeführten Hintergrundaufgaben.

- [Logic Apps](https://azure.microsoft.com/services/logic-apps/) stellt Verwaltungsdienste für die Kommunikation, Integration und für Unternehmensregeln bereit.

#### <a name="autoscaling-azure-vm-scaling-and-azure-app-service-autoscale"></a>Automatische Skalierung mit Azure-VM-Skalierungsgruppen und Azure App Service

Die automatische Skalierung in Azure wird von zwei Diensten durchgeführt:

- [VM-Skalierungsgruppen](/azure/virtual-machine-scale-sets/overview) ermöglichen die Bereitstellung und Verwaltung einer identischen VM-Gruppe. Die Anzahl der Instanzen kann je nach Leistungsanforderungen automatisch skaliert werden.

- [Automatische Skalierung mit App Service](/azure/app-service/web-sites-scale) bietet die Möglichkeit zur automatischen Skalierung von Lösungen mit Azure App Service.

#### <a name="container-service"></a>Container Service

[Azure Kubernetes Service ](/azure/aks/intro-kubernetes) unterstützt Docker-Container, die über Kubernetes verwaltet werden.

#### <a name="other-compute-services"></a>Sonstige Computedienste

Azure bietet mehrere Computedienste, für die es keine direkte Übereinstimmung in AWS gibt:

- [Azure Batch](/azure/batch/batch-technical-overview) ermöglicht die Verwaltung von rechenintensiven Aufgaben für eine skalierbare Sammlung von virtuellen Computern.

- [Service Fabric](/azure/service-fabric/service-fabric-overview) ist eine Plattform für die Entwicklung und das Hosting skalierbarer [Microservicelösungen](/azure/service-fabric/service-fabric-overview-microservices).

#### <a name="see-also"></a>Weitere Informationen

- [Erstellen eines virtuellen Linux-Computers in Azure mithilfe des Portals](/azure/virtual-machines/linux/quick-create-portal)

- [Azure-Referenzarchitektur: Ausführen einer Linux-VM in Azure](/azure/architecture/reference-architectures/n-tier/linux-vm)

- [Erste Schritte mit Node.js-Web-Apps in Azure App Service](/azure/app-service/app-service-web-get-started-nodejs)

- [Azure-Referenzarchitektur: Einfache Webanwendung](/azure/architecture/reference-architectures/app-service-web-app/basic-web-app)

- [Erstellen Sie Ihre erste Funktion in Azure Functions](/azure/azure-functions/functions-create-first-azure-function)

### <a name="storage"></a>Storage

#### <a name="s3ebsefs-and-azure-storage"></a>S3/EBS/EFS und Azure Storage

In der AWS-Plattform ist der Cloudspeicher primär in drei Dienste unterteilt:

- **Simple Storage Service (S3)** Grundlegender Objektspeicher, der Daten über eine per Internet zugängliche API verfügbar macht.

- **Elastic Block Storage (EBS)** Speicher auf Blockebene für den Zugriff durch einen einzelnen virtuellen Computer.

- **Elastic File System (EFS)**. Dateispeicher, der für den Einsatz als freigegebener Speicher für Tausende von EC2-Instanzen vorgesehen ist.

In Azure Storage können mit an Abonnements gebundene [Speicherkonten](/azure/storage/common/storage-quickstart-create-account) die folgenden Speicherdienste erstellt und verwaltet werden:

- [Blob Storage](/azure/storage/common/storage-quickstart-create-account) speichert alle Arten von Text- oder Binärdaten, z.B. ein Dokument, eine Mediendatei oder ein Installer einer Anwendung. Sie können Blob Storage für den privaten Zugriff einrichten oder Inhalte öffentlich im Internet zugänglich machen. Blob Storage dient demselben Zweck wie AWS S3 und EBS.
- [Table Storage](/azure/cosmos-db/table-storage-how-to-use-nodejs) speichert strukturierte Datensätze. Table Storage ist ein Datenspeicher für NoSQL-Schlüsselattribute, der eine schnelle Entwicklung und einen schnellen Zugriff auf große Datenmengen ermöglicht. Dies lässt sich mit dem SimpleDB- and DynamoDB-Dienst von AWS vergleichen.

- [Queue Storage](/azure/storage/queues/storage-nodejs-how-to-use-queues) bietet Messaging für die Workflowverarbeitung und die Kommunikation zwischen Komponenten von Clouddiensten.

- [File Storage](/azure/storage/files/storage-java-how-to-use-file-storage) bietet einen gemeinsam genutzten Speicher für Legacyanwendungen mit dem SMB-Standardprotokoll (Server Message Block). File Storage wird ähnlich wie EFS in der AWS-Plattform verwendet.

#### <a name="glacier-and-azure-storage"></a>Glacier und Azure Storage

[Azure Archive Blob Storage](/azure/storage/blobs/storage-blob-storage-tiers#archive-access-tier) ist vergleichbar mit dem Glacier-Speicherdienst von AWS. Es ist für selten verwendete Daten konzipiert, die für mindestens 180 Tage gespeichert werden und bei denen eine Abrufwartezeit von mehreren Stunden akzeptabel ist.

Für Daten, die zwar selten verwendet werden, beim Zugriff aber umgehend verfügbar sein müssen, stellt [Azure Cool Blob Storage](/azure/storage/blobs/storage-blob-storage-tiers#cool-access-tier) im Vergleich zum Standard-Blobspeicher eine kostengünstigere Speicheroption dar. Diese Speicherebene ist vergleichbar mit AWS S3 (Speicherdienst für selten verwendete Daten).

#### <a name="see-also"></a>Weitere Informationen

- [Checkliste zu Leistung und Skalierbarkeit von Microsoft Azure Storage](/azure/storage/common/storage-performance-checklist)

- [Azure Storage-Sicherheitsleitfaden](/azure/storage/common/storage-security-guide)

- [Bewährte Methoden für die Verwendung von Content Delivery Networks (CDNs)](/azure/architecture/best-practices/cdn)

### <a name="networking"></a>Netzwerk

#### <a name="elastic-load-balancing-azure-load-balancer-and-azure-application-gateway"></a>Elastischer Lastenausgleich, Azure Load Balancer und Azure Application Gateway

Die Pendants zu den beiden Diensten für den elastischen Lastenausgleich in Azure zählen Folgende:

- [Load Balancer](https://azure.microsoft.com/documentation/articles/load-balancer-overview/): Bietet dieselbe Funktionalität wie AWS Classic Load Balancer, mit dem Sie den Datenverkehr für mehrere VMs auf Netzwerkebene verteilen können. Zudem werden Failoverfunktionen bereitgestellt.

- [Application Gateway](https://azure.microsoft.com/documentation/articles/application-gateway-introduction/): Bietet regelbasiertes Routing auf Anwendungsebene, vergleichbar mit AWS Application Load Balancer.

#### <a name="route-53-azure-dns-and-azure-traffic-manager"></a>Route 53, Azure DNS und Azure Traffic Manager

In AWS bietet Route 53 Funktionen zur Verwaltung von DNS-Namen sowie Datenverkehrrouting und Failoverdienste auf DNS-Ebene. In Azure wird dies über zwei Dienste abgewickelt:

- [Azure DNS](https://azure.microsoft.com/documentation/services/dns/) bietet Domänen- und DNS-Verwaltung.

- [Traffic Manager][traffic-manager] bietet Datenverkehrsrouting, Lastenausgleich und Failoverfunktionen auf DNS-Ebene.

#### <a name="direct-connect-and-azure-expressroute"></a>Direct Connect und Azure ExpressRoute

Azure bietet über seinen [ExpressRoute](https://azure.microsoft.com/documentation/services/expressroute/)-Dienst ähnliche dedizierte Site-to-Site-Verbindungen an. Mit ExpressRoute kann über eine dedizierte VPN-Verbindung eine direkte Verbindung zwischen Ihrem lokalen Netzwerk und Azure-Ressourcen hergestellt werden. Azure bietet auch konventionellere [Site-to-Site-VPN-Verbindungen](https://azure.microsoft.com/documentation/articles/vpn-gateway-howto-site-to-site-resource-manager-portal/) zu geringeren Kosten.

#### <a name="see-also"></a>Weitere Informationen

- [Erstellen eines virtuellen Netzwerks im Azure-Portal](https://azure.microsoft.com/documentation/articles/virtual-networks-create-vnet-arm-pportal/)

- [Planen und Entwerfen von Azure Virtual Networks](https://azure.microsoft.com/documentation/articles/virtual-network-vnet-plan-design-arm/)

- [Bewährte Methoden für die Azure-Netzwerksicherheit](https://azure.microsoft.com/documentation/articles/azure-security-network-security-best-practices/)

### <a name="database-services"></a>Datenbankdienste

#### <a name="rds-and-azure-relational-database-services"></a>RDS und Azure-Dienste für relationale Datenbanken

In Azure stehen verschiedene Dienste für relationale Datenbanken zur Verfügung, die dem Dienst für relationale Datenbanken (Relational Database Service, RDS) von AWS entsprechen.

- [SQL-Datenbank](/azure/sql-database/sql-database-technical-overview)
- [Azure Database for MySQL](/azure/mysql/overview)
- [Azure-Datenbank für PostgreSQL](/azure/postgresql/overview)

Andere Datenbank-Engines wie [SQL Server](https://azure.microsoft.com/services/virtual-machines/sql-server/), [Oracle](https://azure.microsoft.com/campaigns/oracle/) und [MySQL](https://azure.microsoft.com/documentation/articles/virtual-machines-windows-classic-mysql-2008r2/) können über Azure-VM-Instanzen bereitgestellt werden.

Die Kosten für AWS RDS werden durch die Menge der Hardwareressourcen bestimmt, die Ihre Instanz verbraucht, z.B. CPU-, RAM- und Speicherkapazitäten sowie Netzwerkbandbreite. Bei den Azure-Datenbankdiensten hängen die Kosten von der Datenbankgröße, der Anzahl der gleichzeitigen Verbindungen und den Durchsatzstufen ab.

#### <a name="see-also"></a>Weitere Informationen

- [Tutorials zu Azure-SQL-Datenbanken](https://azure.microsoft.com/documentation/articles/sql-database-explore-tutorials/)

- [Konfigurieren der Georeplikation für Azure SQL-Datenbank mit dem Azure-Portal](https://azure.microsoft.com/documentation/articles/sql-database-geo-replication-portal/)

- [Einführung in Cosmos DB: JSON-NoSQL-Datenbanken](/azure/cosmos-db/sql-api-introduction)

- [Verwenden des Azure-Tabellenspeichers mit Node.js](https://azure.microsoft.com/documentation/articles/storage-nodejs-how-to-use-table-storage/)

### <a name="security-and-identity"></a>Sicherheit und Identität

#### <a name="directory-service-and-azure-active-directory"></a>Verzeichnisdienst und Azure Active Directory

Azure unterteilt Verzeichnisdienste in folgende Angebote:

- [Azure Active Directory](https://azure.microsoft.com/documentation/articles/active-directory-whatis/): Cloudbasierter Verzeichnis- und Identitätsverwaltungsdienst

- [Azure Active Directory B2B](https://azure.microsoft.com/documentation/articles/active-directory-b2b-collaboration-overview/): Ermöglicht Zugriff auf Unternehmensanwendungen mit von Partnern verwalteten Identitäten

- [Azure Active Directory B2C](https://azure.microsoft.com/documentation/articles/active-directory-b2c-overview/): Dienst, der Unterstützung für einmaliges Anmelden und Benutzerverwaltung für kundenorientierte Anwendungen bietet

- [Azure Active Directory Domain Services](https://azure.microsoft.com/documentation/articles/active-directory-ds-overview/): Gehosteter Domänencontrollerdienst, der mit Active Directory kompatible Domänenbeitritts- und Benutzerverwaltungsfunktionen ermöglicht

#### <a name="web-application-firewall"></a>Web Application Firewall

Neben der [Application Gateway-WAF (Web Application Firewall)](/azure/application-gateway/waf-overview) können Sie auch Web Application Firewalls von Drittanbietern wie [Barracuda Networks](https://azure.microsoft.com/marketplace/partners/barracudanetworks/waf/) verwenden.

#### <a name="see-also"></a>Weitere Informationen

- [Erste Schritte mit Microsoft Azure-Sicherheit](/azure/security)

- [Azure-Identitätsverwaltung und Sicherheit der Zugriffssteuerung – Bewährte Methoden](/azure/security/azure-security-identity-management-best-practices)

### <a name="application-and-messaging-services"></a>Anwendungs- und Messagingdienste

#### <a name="simple-email-service"></a>Simple Email Service

AWS stellt mit Simple Email Service (SES) Funktionen zum Versand von Benachrichtigungs-, Transaktions- oder Marketing-E-Mails bereit. In Azure bieten Lösungen von Drittanbietern wie [SendGrid](https://sendgrid.com/partners/azure/) E-Mail-Dienste an.

#### <a name="simple-queueing-service"></a>Simple Queueing Service

AWS Simple Queueing Service (SQS) stellt ein Messagingsystem bereit, das innerhalb der AWS-Plattform eine Verbindung mit Anwendungen, Diensten und Geräten herstellt. Azure bietet zwei Dienste mit ähnlicher Funktionalität an:

- [Queue Storage](/azure/storage/queues/storage-nodejs-how-to-use-queues): Ein Cloudmessagingdienst, der die Kommunikation zwischen Anwendungskomponenten innerhalb der Azure-Plattform ermöglicht

- [Service Bus](https://azure.microsoft.com/services/service-bus/): Ein zuverlässigeres Messagingsystem, um für Anwendungen, Diensten und Geräte eine Verbindung herzustellen. Mit dem entsprechenden [Service Bus Relay](/azure/service-bus-relay/relay-what-is-it) kann Service Bus auch eine Verbindung mit remote gehosteten Anwendungen und Diensten herstellen.

#### <a name="device-farm"></a>Device Farm

AWS Device Farm bietet geräteübergreifende Testdienste. In Azure bietet [Xamarin Test Cloud](https://www.xamarin.com/test-cloud) ähnliche geräteübergreifende Front-End-Tests für Mobilgeräte.

Zusätzlich zu den Front-End-Tests bietet [Azure DevTest Labs](https://azure.microsoft.com/services/devtest-lab/) Back-End-Testressourcen für Linux- und Windows-Umgebungen.

#### <a name="see-also"></a>Weitere Informationen

- [Gewusst wie: Verwenden von Queue Storage mit Node.js](/azure/storage/queues/storage-nodejs-how-to-use-queues)

- [Verwenden von Service Bus-Warteschlangen](/azure/service-bus-messaging/service-bus-nodejs-how-to-use-queues)

### <a name="analytics-and-big-data"></a>Analysen und Big Data

Die [Cortana Intelligence Suite](https://azure.microsoft.com/suites/cortana-intelligence-suite/) ist ein Paket aus Azure-Produkten und -Diensten, das für die Erfassung, Organisation, Analyse und Visualisierung großer Datenmengen bestimmt ist. Die Cortana Suite besteht aus folgenden Diensten:

- [HDInsight](https://azure.microsoft.com/documentation/services/hdinsight/): Verwaltete Apache-Verteilung, die Hadoop, Spark, Storm oder HBase umfasst

- [Data Factory](https://azure.microsoft.com/documentation/services/data-factory/): Bietet Datenorchestrierungs- und Datenpipelinefunktionen

- [SQL Data Warehouse](https://azure.microsoft.com/documentation/services/sql-data-warehouse/): Umfangreicher relationaler Datenspeicher

- [Data Lake Store](https://azure.microsoft.com/documentation/services/data-lake-store/): Großspeicher, der für Big Data-Analyseworkloads optimiert ist

- [Machine Learning](https://azure.microsoft.com/documentation/services/machine-learning/): Wird verwendet, um Predictive Analytics zu erstellen und auf Daten anzuwenden

- [Stream Analytics](https://azure.microsoft.com/documentation/services/stream-analytics/): Datenanalyse in Echtzeit

- [Data Lake Analytics](https://azure.microsoft.com/documentation/articles/data-lake-analytics-overview/): Umfassender Analysedienst, der für die Verwendung mit Data Lake Store optimiert ist

- [Power BI](https://powerbi.microsoft.com/): Wird zur Bereitstellung von Datenvisualisierung verwendet

#### <a name="see-also"></a>Weitere Informationen

- [Cortana Intelligence Gallery](https://gallery.cortanaintelligence.com/)

- [Überblick über Big Data-Lösungen von Microsoft](https://msdn.microsoft.com/library/dn749804.aspx)

- [Blog zu Azure Data Lake und Azure HDInsight](https://blogs.msdn.microsoft.com/azuredatalake/)

### <a name="internet-of-things"></a>Internet der Dinge

#### <a name="see-also"></a>Weitere Informationen

- [Erste Schritte mit Azure IoT Hub](https://azure.microsoft.com/documentation/articles/iot-hub-csharp-csharp-getstarted/)

- [Vergleich von IoT Hub und Event Hubs](https://azure.microsoft.com/documentation/articles/iot-hub-compare-event-hubs/)

### <a name="mobile-services"></a>Mobile Services

#### <a name="notifications"></a>Benachrichtigungen

Notification Hubs unterstützt nicht das Senden von SMS oder E-Mail-Nachrichten. Daher sind für diese Zustellarten Dienste von Drittanbietern erforderlich.

#### <a name="see-also"></a>Weitere Informationen

- [Erstellen einer Android-App](https://azure.microsoft.com/documentation/articles/app-service-mobile-android-get-started/)

- [Authentifizierung und Autorisierung in Azure Mobile Apps](https://azure.microsoft.com/documentation/articles/app-service-mobile-auth/)

- [Senden von Pushbenachrichtigungen mit Azure Notification Hubs](https://azure.microsoft.com/documentation/articles/notification-hubs-android-push-notification-google-fcm-get-started/)

### <a name="management-and-monitoring"></a>Verwaltung und Überwachung

#### <a name="see-also"></a>Weitere Informationen

- [Anleitung zur Überwachung und Diagnose](https://azure.microsoft.com/documentation/articles/best-practices-monitoring/)

- [Bewährte Methoden für das Erstellen von Azure Resource Manager-Vorlagen](https://azure.microsoft.com/documentation/articles/resource-manager-template-best-practices/)

- [Azure Resource Manager-Schnellstartvorlagen](https://azure.microsoft.com/documentation/templates/)

## <a name="next-steps"></a>Nächste Schritte

- [Erste Schritte mit Azure](https://azure.microsoft.com/get-started/)

- [Azure-Lösungsarchitekturen](https://azure.microsoft.com/solutions/architecture/)

- [Azure-Referenzarchitekturen](https://azure.microsoft.com/documentation/articles/guidance-architecture/)

<!-- links -->

[paired-regions]: https://azure.microsoft.com/documentation/articles/best-practices-availability-paired-regions/
[traffic-manager]: /azure/traffic-manager/

<!-- markdownlint-enable MD024 -->
