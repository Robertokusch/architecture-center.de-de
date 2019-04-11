---
title: Microservices-Architektur in Azure Service Fabric
description: Microservices-Architektur in Azure Service Fabric
author: PageWriter-MSFT
ms.date: 3/07/2019
ms.topic: reference-architecture
ms.service: architecture-center
ms.subservice: reference-architecture
ms.custom: microservices
ms.openlocfilehash: 228e3242ce9f1ca45bc15c5b34f770bfc092dd43
ms.sourcegitcommit: c053e6edb429299a0ad9b327888d596c48859d4a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/20/2019
ms.locfileid: "58244341"
---
# <a name="microservices-architecture-on-azure-service-fabric"></a>Microservices-Architektur in Azure Service Fabric

Diese Referenzarchitektur zeigt eine in Azure Service Fabric bereitgestellte Microservices-Architektur. Zu sehen ist eine einfache Clusterkonfiguration, die als Ausgangspunkt für die meisten Bereitstellungen dienen kann.

![Referenzarchitektur für Service Fabric](./_images/ra-sf-arch.png)

> [!NOTE]
> Der Fokus dieses Artikels liegt auf dem Programmiermodell [Reliable Services](/azure/service-fabric/service-fabric-reliable-services-introduction) für Service Fabric. Die Nutzung von Service Fabric zur Bereitstellung und Verwaltung von [Containern](/azure/service-fabric/service-fabric-containers-overview) ist nicht Gegenstand dieses Artikels.

## <a name="architecture"></a>Architecture

Die Architektur umfasst die folgenden Komponenten. Informationen zu anderen Begriffen finden in der [Übersicht über die Service Fabric-Terminologie](/azure/service-fabric/service-fabric-technical-overview).

**Service Fabric-Cluster**. In einem Netzwerk verbundene virtuelle Computer (VMs), auf denen Ihre Microservices bereitgestellt und verwaltet werden.

**VM-Skalierungsgruppen**. Mit VM-Skalierungsgruppen können Sie einer Gruppe identischer virtueller Computer mit Lastenausgleich und automatischer Skalierung erstellen und verwalten. Sie bieten außerdem Fehler- und Upgradedomänen.

**Knoten**. Die Knoten sind die VMs, die dem Service Fabric-Cluster angehören.

**Knotentypen**. Ein Knotentyp stellt eine VM-Skalierungsgruppe dar, die eine Sammlung von Knoten bereitstellt. Ein Service Fabric-Cluster hat mindestens einen Knotentyp. In einem Cluster mit mehreren Knotentypen müssen Sie den [primären Knotentyp](/azure/service-fabric/service-fabric-cluster-capacity#primary-node-type) festlegen. Auf dem primären Knotentyp in Ihrem Cluster werden die [Service Fabric-Systemdienste](/azure/service-fabric/service-fabric-technical-overview#system-services) ausgeführt. Diese Dienste stellen die Plattformfunktionen von Service Fabric zur Verfügung. Der primäre Knotentyp fungiert auch [Seedknoten](/azure/service-fabric/service-fabric-disaster-recovery#random-failures-leading-to-cluster-failures) für den Cluster. Dabei handelt es sich um die Knoten, die die Verfügbarkeit des zugrunde liegenden Clusters aufrechterhalten. Konfigurieren Sie [zusätzliche Knotentypen](/azure/service-fabric/service-fabric-cluster-capacity#non-primary-node-type) zum Ausführen Ihrer Dienste.

**Dienste**. Ein Dienst übernimmt eine eigenständige Funktion und kann unabhängig von anderen Diensten gestartet und ausgeführt werden. Instanzen von Diensten werden auf Knoten im Cluster bereitgestellt. Es gibt in Service Fabric zwei Arten von Dienst:

- **Zustandsloser Dienst**: Ein zustandsloser Dienst behält seinen Zustand innerhalb des Diensts nicht bei. Wenn Zustandspersistenz erforderlich ist, wird der Zustand in einen externen Speicher, wie z.B. Azure Cosmos DB, geschrieben und aus diesem abgerufen.
- **Zustandsbehafteter Dienst**: Der [Dienstzustand](/azure/service-fabric/service-fabric-concepts-state) wird innerhalb des Diensts selbst gespeichert. Bei den meisten zustandsbehafteten Diensten wird dies über die Service Fabric-Funktion [Zuverlässige Sammlungen](/azure/service-fabric/service-fabric-reliable-services-reliable-collections) implementiert.

**Service Fabric Explorer**. Bei [Service Fabric Explorer][sfx] handelt es sich um ein Open-Source-Tool zum Untersuchen und Verwalten von Service Fabric-Clustern.

**Azure Pipelines**. [Pipelines](/azure/devops/pipelines/?view=azure-devops) ist Teil von [Azure DevOps Services](/azure/devops/index?view=azure-devops) und führt automatisierte Build-, Test- und Bereitstellungsvorgänge durch. Sie können auch CI/CD-Lösungen von Drittanbietern, z.B. Jenkins, verwenden.

**Azure Monitor**: [Azure Monitor](/azure/azure-monitor/) erfasst und speichert Metriken und Protokolle. Beispiele hierfür sind Plattformmetriken für die Azure-Dienste in der Lösung und Anwendungstelemetrie. Nutzen Sie diese Daten zum Überwachen der Anwendung, Einrichten von Warnungen und Dashboards und Durchführen von Analysen der Grundursache von Fehlern. Azure Monitor wird in Service Fabric integriert, um Metriken von Controllern, Knoten und Containern sowie aus Container- und Masterknotenprotokollen zu erfassen.

**Azure Key Vault**. Verwenden Sie [Key Vault](/azure/key-vault/), um alle Anwendungsgeheimnisse zu speichern, die von den Microservices verwendet werden, wie z.B. Verbindungszeichenfolgen.

**Azure API Management:** In dieser Architektur fungiert [API Management](/azure/api-management/api-management-key-concepts) als API-Gateway, das Anforderungen von Clients entgegennimmt und an Ihre Dienste weiterleitet.

## <a name="design-considerations"></a>Überlegungen zum Entwurf

Diese Referenzarchitektur konzentriert sich auf [Architekturen von Microservices](../../guide/architecture-styles/microservices.md). Ein Microservice ist eine kleine unabhängige Codeeinheit mit Versionsangabe. Microservices können über Dienstermittlungsmethoden ermittelt werden und über APIs mit anderen Diensten kommunizieren. Jeder Dienst ist eigenständig und sollte eine einzige Geschäftsfunktion implementieren. Weitere Informationen darüber, wie Sie Ihre Anwendungsdomäne in Microservices aufteilen können, finden Sie unter [Verwenden der Domänenanalyse zur Modellierung von Microservices](../../microservices/model/domain-analysis.md).

Service Fabric bietet eine Infrastruktur für das effiziente Erstellen, Bereitstellen und Aktualisieren von Microservices. Der Dienst bietet auch Optionen für die automatische Skalierung, Zustandsverwaltung und -überwachung sowie für den Neustart von Diensten bei einem Ausfall.

Service Fabric arbeitet mit einem Anwendungsmodell, bei dem eine Anwendung eine Sammlung von Microservices ist. Die Anwendung wird in einer [Anwendungsmanifestdatei](/azure/service-fabric/service-fabric-application-and-service-manifests) beschrieben, die die verschiedenen Arten von Diensten, die in dieser Anwendung enthalten sind, und Verweise auf die unabhängigen Dienstpakete definiert. Das Anwendungspaket enthält in der Regel auch Parameter, die als Überschreibungen für bestimmte Einstellungen dienen, die von den Diensten verwendet werden. Jedes Dienstpaket enthält eine Manifestdatei, die die physischen Dateien und Ordner beschreibt, die für die Ausführung dieses Diensts erforderlich sind, einschließlich Binärdateien, Konfigurationsdateien und schreibgeschützte Daten für den jeweiligen Dienst. Dienste und Anwendungen können unabhängig voneinander mit Versionsangaben versehen und aktualisiert werden.

Optional kann das Anwendungsmanifest Dienste beschreiben, die automatisch bereitgestellt werden, wenn eine Instanz der Anwendung erstellt wird. Diese werden als Standarddienste bezeichnet. In diesem Fall beschreibt das Anwendungsmanifest auch, wie diese Dienste erstellt werden sollen, einschließlich des Namens des Diensts, der Anzahl der Instanzen, der Sicherheits-/Isolationsrichtlinie und der Platzierungsbeschränkungen.

> [!NOTE]
> Vermeiden Sie die Verwendung von Standarddiensten, wenn Sie die Lebensdauer Ihrer Dienste steuern möchten. Standarddienste werden beim Erstellen der Anwendung erstellt und werden so lange ausgeführt, wie die Anwendung ausgeführt wird.

Grundlegende Informationen zu Service Fabric finden Sie unter [Sie möchten sich über Service Fabric informieren?](/azure/service-fabric/service-fabric-content-roadmap).

### <a name="choose-an-application-to-service-packaging-model"></a>Wählen eines Anwendung-zu-Dienst-Paketerstellungsmodells

Ein Grundsatz von Microservices ist, dass jeder Dienst unabhängig bereitgestellt werden kann. Wenn Sie in Service Fabric alle Ihre Dienste in einem einzigen Anwendungspaket gruppieren und ein Dienst nicht aktualisiert werden kann, erfolgt ein Rollback des gesamten Updates der Anwendung, wodurch verhindert wird, dass andere Dienste aktualisiert werden.

Aus diesem Grund empfehlen wir in einer Microservices-Architektur die Verwendung mehrerer Anwendungspakete. Fassen Sie einen oder mehrere eng zusammengehörige Diensttypen zu einem einzigen Anwendungstyp zusammen. Wenn Ihr Team für eine Reihe von Diensten verantwortlich ist, die die gleiche Lebensdauer haben und gleichzeitig aktualisiert werden müssen, den gleichen Lebenszyklus haben oder Ressourcen wie Abhängigkeiten oder Konfigurationen gemeinsam nutzen, platzieren Sie diese Diensttypen im gleichen Anwendungstyp.

### <a name="service-fabric-programming-models"></a>Service Fabric-Programmiermodelle

Wenn Sie einen Microservice einer Service Fabric-Anwendung hinzufügen, entscheiden Sie, ob er über einen Zustand oder Daten verfügt, die hochverfügbar und zuverlässig zur Verfügung gestellt werden müssen. Falls ja, können Daten extern gespeichert werden oder sind die Daten als Teil des Diensts enthalten? Wählen Sie einen zustandslosen Dienst, wenn Sie keine Daten speichern müssen oder Daten in einem externen Speicher speichern möchten. Wenn Sie den Zustand oder die Daten als Teil des Diensts pflegen möchten (z.B. wenn sich diese Daten im Arbeitsspeicher in der Nähe des Codes befinden müssen) oder eine Abhängigkeit von einem externen Speicher nicht unterstützen können, sollten Sie einen zustandsbehafteten Dienst wählen.

Wenn Sie bereits Code haben, den Sie in Service Fabric ausführen möchten, können Sie ihn als ausführbare Gastdatei ausführen, bei der es sich um eine beliebige ausführbare Datei handelt, die als Dienst ausgeführt wird. Alternativ können Sie die ausführbare Datei in einem Container packen, der alle für die Bereitstellung erforderlichen Abhängigkeiten enthält. Service Fabric modelliert sowohl Container als auch ausführbare Gastdateien als zustandslose Dienste. Eine Anleitung zur Wahl eines Modells finden Sie unter [Übersicht über die Service Fabric-Programmiermodelle](/azure/service-fabric/service-fabric-choose-framework).

Bei ausführbaren Gastdateien sind Sie für die Wartung der Umgebung verantwortlich, in der sie ausgeführt werden. Angenommen, eine ausführbare Gastdatei verlangt Python. Wenn die ausführbare Datei nicht eigenständig ist, müssen Sie sicherstellen, dass die erforderliche Version von Python bereits in der Umgebung installiert ist. Service Fabric verwaltet nicht die Umgebung. Azure bietet mehrere Mechanismen zum Einrichten der Umgebung, einschließlich benutzerdefinierter VM-Images und -Erweiterungen.

Weitere Informationen finden Sie unter

- [Packen einer Anwendung](/azure/service-fabric/service-fabric-package-apps)
- [Packen und Bereitstellen einer vorhandenen ausführbaren Datei für Service Fabric](/azure/service-fabric/service-fabric-deploy-existing-app)

### <a name="api-gateway"></a>API-Gateway

Ein [API-Gateway](../..//microservices/design/gateway.md) (eingehend) befindet sich zwischen externen Clients und den Microservices. Es fungiert als Reverseproxy und leitet Anforderungen von Clients an Microservices weiter. Darüber hinaus kann es verschiedene übergreifende Aufgaben wie Authentifizierung, SSL-Beendigung und Ratenbegrenzung übernehmen.

Azure API Management wird in den meisten Szenarien empfohlen, aber [Træfik](https://docs.traefik.io/) ist eine beliebte Open-Source-Alternative. Beide Technologieoptionen sind in Service Fabric integriert.

- API Management macht eine öffentliche IP-Adresse verfügbar und leitet Datenverkehr an Ihre Dienste weiter. Die Lösung wird in einem dedizierten Subnetz im gleichen virtuellen Netzwerk wie der Service Fabric-Cluster ausgeführt.  Sie kann auf Dienste in einem Knotentyp zugreifen, der über einen Lastenausgleich mit einer privaten IP-Adresse verfügbar gemacht wird. Diese Option ist nur in den Tarifen „Premium“ und „Developer“ von API Management verfügbar. Wählen Sie für Produktionsworkloads den Premium-Tarif. Weitere Informationen zu den Preisen finden Sie unter [API Management-Preise](https://azure.microsoft.com/pricing/details/api-management/). Weitere Informationen finden Sie unter [Service Fabric mit Azure API Management: Übersicht](/azure/service-fabric/service-fabric-api-management-overview).
- Træfik unterstützt Features wie Routing, Ablaufverfolgung, Protokolle und Metriken. Træfik wird als zustandsloser Dienst im Service Fabric-Cluster ausgeführt. Die Dienstversionsverwaltung kann über Routing unterstützt werden. Informationen darüber, wie Sie Træfik für eingehende Dienstdaten und als Reverseproxy innerhalb des Clusters einrichten, finden Sie unter [Azure Service Fabric-Anbieter](https://docs.traefik.io/configuration/backends/servicefabric/). Weitere Informationen zur Verwendung von Træfik mit Service Fabric finden Sie im Blogbeitrag [Intelligent routing on Service Fabric with Træfik](https://blogs.msdn.microsoft.com/azureservicefabric/2018/04/05/intelligent-routing-on-service-fabric-with-traefik/) (Intelligentes Routing in Service Fabric mit Træfik).

Weitere API Management-Optionen sind [Azure Application Gateway](/azure/application-gateway/) und [Azure Front Door](/azure/frontdoor/). Diese Dienste können in Verbindung mit API Management verwendet werden, um Aufgaben wie Routing, SSL-Terminierung und Firewall abzudecken.

### <a name="interservice-communication"></a>Kommunikation zwischen Diensten

Um die Dienst-zu-Dienst-Kommunikation zu erleichtern, sollten Sie HTTP als Kommunikationsprotokoll in Erwägung ziehen. Als Grundlage für die meisten Szenarien wird der [Reverseproxydienst](/azure/service-fabric/service-fabric-reverseproxy) für die Dienstermittlung empfohlen.

- Kommunikationsprotokoll. In einer Microservices-Architektur müssen Dienste mit minimaler Kopplung zur Laufzeit miteinander kommunizieren. Um eine sprachunabhängige Kommunikation zu ermöglichen, ist HTTP ein Branchenstandard mit einer breiten Palette von Tools und HTTP-Servern, die in verschiedenen Sprachen verfügbar sind und allesamt von Service Fabric unterstützt werden. Daher wird für die meisten Workloads HTTP anstelle des in Service Fabric integrierten Dienstremotings empfohlen.
- Dienstermittlung: Um mit anderen Diensten innerhalb eines Clusters zu kommunizieren, muss ein Clientdienst den aktuellen Standort des Zieldiensts auflösen. In Service Fabric können Dienste zwischen Knoten verschoben werden, wodurch sich die Dienstendpunkte dynamisch ändern. Um Verbindungen mit veralteten Endpunkten zu vermeiden, kann der Naming Service von Service Fabric verwendet werden, um aktualisierte Endpunktinformationen abzurufen. Service Fabric bietet jedoch auch einen integrierten [Reverseproxydienst](/azure/service-fabric/service-fabric-reverseproxy), der den Naming Service abstrahiert. Diese Option ist einfacher zu verwenden und führt zu einfacherem Code.

Es folgen weitere Optionen für die Kommunikation zwischen Diensten:

- [Træfik](https://docs.traefik.io/) für erweitertes Routing.
- [DNS](/azure/dns/) für Kompatibilitätsszenarien, bei denen ein Dienst erwartet, dass DNS verwendet wird.
- [ServicePartitionClient&lt;TCommunicationClient&gt;](/dotnet/api/microsoft.servicefabric.services.communication.client.servicepartitionclient-1?view=azure-dotnet)-Klasse. Die Klasse speichert Dienstendpunkte zwischen und kann eine bessere Leistung ermöglichen, da Aufrufe direkt zwischen Diensten ohne Vermittler oder benutzerdefinierte Protokolle erfolgen.

## <a name="scalability-considerations"></a>Überlegungen zur Skalierbarkeit

Service Fabric unterstützt die Skalierung dieser Clusterentitäten:

- Skalieren der Anzahl von Knoten für Knotentyp
- Skalieren von Diensten

Dieser Abschnitt konzentriert sich auf die automatische Skalierung. In bestimmten Situationen können Sie die Skalierung manuell vornehmen. Dies gilt beispielsweise in einer Situation, in der ein manueller Eingriff erforderlich ist, um die Anzahl der Instanzen festzulegen.  

### <a name="initial-cluster-configuration-for-scalability"></a>Anfängliche Clusterkonfiguration für Skalierbarkeit

Wenn Sie einen Service Fabric-Cluster erstellen, stellen Sie die Knotentypen entsprechend Ihren Sicherheits- und Skalierungsanforderungen bereit. Jeder Knotentyp wird einer VM-Skalierungsgruppe zugeordnet und kann unabhängig skaliert werden.

- Erstellen Sie einen Knotentyp für jede Gruppe von Diensten, die unterschiedliche Skalierungs- oder Ressourcenanforderungen haben. Stellen Sie zunächst einen Knotentyp (der zum [primären Knotentyp](/azure/service-fabric/service-fabric-cluster-capacity#primary-node-type) wird) für die Systemdienste von Service Fabric bereit. Erstellen Sie anschließend gesonderte Knotentypen, um Ihre öffentlichen oder Front-End-Dienste und andere Knotentypen auszuführen, die für Ihre Back-End- bzw. private oder isolierte Dienste erforderlich sind. Geben Sie [Platzierungseinschränkungen](/azure/service-fabric/service-fabric-cluster-resource-manager-advanced-placement-rules-placement-policies) an, damit die Dienste nur für die vorgesehenen Knotentypen bereitgestellt werden.
- Geben Sie für jeden Knotentyp eine Dauerhaftigkeitsstufe an. Die Dauerhaftigkeitsstufe beschreibt die Möglichkeit von Service Fabric, Updates für VM-Skalierungsgruppen und Wartungsvorgänge zu beeinflussen. Wählen Sie für Produktionsworkloads mindestens die Dauerhaftigkeitsstufe „Silber“. Weitere Informationen zu jeder Stufe finden Sie unter [Die Dauerhaftigkeitsmerkmale des Clusters](/azure/service-fabric/service-fabric-cluster-capacity#the-durability-characteristics-of-the-cluster).
- Bei Verwenden der Dauerhaftigkeitsstufe „Bronze“ erfordern bestimmte Vorgänge manuelle Schritte.  Für Knotentypen mit der Dauerhaftigkeitsstufe „Bronze“ sind beim horizontalen Herunterskalieren zusätzliche Schritte erforderlich. Weitere Informationen zu Skalierungsvorgängen finden Sie in [dieser Anleitung](/azure/service-fabric/service-fabric-cluster-scale-up-down).

### <a name="scaling-nodes"></a>Skalieren von Knoten

Service Fabric bietet für die horizontale Hoch- und Herunterskalierung eine automatische Skalierung. Jeder Knotentyp kann unabhängig für die automatische Skalierung konfiguriert werden.

Jeder Knotentyp kann maximal 100 Knoten enthalten. Beginnen Sie mit einer kleinere Gruppe von Knoten, und fügen Sie abhängig von der Last weitere Knoten hinzu. Wenn Sie mehr als 100 Knoten in einem Knotentyp benötigen, müssen Sie weitere Knotentypen hinzufügen. Einzelheiten finden Sie unter [Überlegungen zur Kapazitätsplanung für Service Fabric-Cluster](/azure/service-fabric/service-fabric-cluster-capacity). Eine VM-Skalierungsgruppe wird nicht sofort skaliert, was Sie beim Einrichten von Regeln für die Autoskalierung berücksichtigen sollten.

Konfigurieren Sie für die automatische horizontale Herunterskalierung den Knotentyp mit der Dauerhaftigkeitsstufe „Silber“ oder „Gold“. Dadurch wird sichergestellt, dass die Skalierung verzögert wird, bis Service Fabric die Verlagerung von Diensten abgeschlossen hat, und dass die VM-Skalierungsgruppen Service Fabric darüber informieren, dass die VMs entfernt und nicht nur vorübergehend heruntergefahren werden.

Weitere Informationen zum Skalieren auf Knoten-/Clusterebene finden Sie unter [Skalieren von Azure Service Fabric-Clustern](/azure/service-fabric/service-fabric-cluster-scaling).

### <a name="scaling-services"></a>Skalieren von Diensten

Zustandslose und zustandsbehaftete Dienste verfolgen bei der Skalierung unterschiedliche Ansätze.

#### <a name="autoscaling-for-stateless-services"></a>Automatische Skalierung für zustandslose Dienste

- Verwenden Sie den Trigger bei durchschnittlicher Partitionslast. Dieser Trigger bestimmt, wann der Dienst horizontal herunter- oder hochskaliert wird, und zwar basierend auf einem in der Skalierungsrichtlinie angegebenen Schwellenwert für die Last. Sie können auch festlegen, wie oft der Trigger überprüft wird. Siehe [Trigger für die durchschnittliche Partitionslast mit instanzbasierter Skalierung](/azure/service-fabric/service-fabric-cluster-resource-manager-autoscaling#average-partition-load-trigger-with-instance-based-scaling). Dadurch ist ein zentrales Hochskalieren auf die Anzahl verfügbarer Knoten möglich.
- Legen Sie im Dienstmanifest **InstanceCount** auf -1 fest, was Service Fabric anweist, auf jedem Knoten eine Instanz des Diensts auszuführen. Dieser Ansatz ermöglicht dem Dienst eine dynamische Skalierung entsprechend der Skalierung des Clusters. Wenn sich die Anzahl der Knoten im Cluster ändert, erstellt und löscht Service Fabric automatisch entsprechende Dienstinstanzen.

> [!NOTE]
> In einigen Fällen möchten Sie Ihren Dienst möglicherweise manuell skalieren.  Wenn Sie beispielsweise einen Dienst haben, der Daten aus Event Hubs liest, möchten Sie möglicherweise, dass eine dedizierte Instanz Daten aus jeder Event Hub-Partition liest, um einen gleichzeitigen Zugriff auf die Partition zu vermeiden.  

#### <a name="scaling-for-stateful-services"></a>Skalierung für zustandsbehaftete Dienste

Bei einem zustandsbehafteten Dienst wird die Skalierung von der Anzahl der Partitionen, der Größe jeder Partition und der Anzahl der Partitionen/Replikate bestimmt, die auf einer bestimmten VM ausgeführt werden.

- Wenn Sie partitionierte Dienste erstellen, empfehlen wir eine hohe Anzahl kleiner Partitionen. Wenn weitere Knoten hinzugefügt werden, verteilt Service Fabric die Workloads standardmäßig auf die neuen VMs. Wenn es beispielsweise 5 Knoten und 10 Partitionen gibt, platziert Service Fabric standardmäßig auf jedem Knoten zwei primäre Replikate. Wenn Sie die Knoten horizontal skalieren, können Sie eine höhere Leistung erzielen, da die Verarbeitungsaufgaben gleichmäßig auf mehr Ressourcen verteilt werden. Weitere Informationen zu Szenarien, die diese Strategie befolgen, finden Sie unter [Skalierung in Service Fabric](/azure/service-fabric/service-fabric-concepts-scalability).
- Das Hinzufügen oder Entfernen von Partitionen wird nicht sonderlich gut unterstützt. Eine weitere häufig verwendete Option zur Skalierung ist das dynamische Erstellen oder Löschen von Diensten oder ganzen Anwendungsinstanzen. Ein Beispiel dieser Vorgehensweise finden Sie unter [Skalieren durch das Erstellen oder Entfernen von neuen benannten Diensten](/azure/service-fabric/service-fabric-concepts-scalability#scaling-by-creating-or-removing-new-named-services).

Weitere Informationen finden Sie unter

- [Zentrales Hoch- oder Herunterskalieren eines Service Fabric-Clusters mithilfe von Regeln für die automatische Skalierung oder eines manuellen Verfahrens](/azure/service-fabric/service-fabric-cluster-scale-up-down)
- [Programmgesteuertes Skalieren eines Service Fabric-Clusters](/azure/service-fabric/service-fabric-cluster-programmatic-scaling)
- [Skalieren eines Service Fabric-Clusters durch Hinzufügen einer VM-Skalierungsgruppe](/azure/service-fabric/virtual-machine-scale-set-scale-node-type-scale-out)

### <a name="using-metrics-to-balance-load"></a>Verwenden von Metriken für den Lastenausgleich

Je nachdem, wie Sie die Partition entwerfen, kann es Knoten mit Replikaten geben, die mehr Datenverkehr erhalten als andere. Um dies zu vermeiden, partitionieren Sie den Dienstzustand so, dass er auf alle Partitionen verteilt wird. Verwenden Sie das Bereichspartitionierungsschema mit einem geeigneten Hashalgorithmus. Siehe [Erste Schritte mit der Partitionierung](/azure/service-fabric/service-fabric-concepts-partitioning#get-started-with-partitioning).

Service Fabric verwendet Metriken, um zu wissen, wie Dienste innerhalb eines Clusters platziert und aufeinander abgestimmt werden sollen. Sie können beim Erstellen dieses Diensts für jede Metrik, die einem Dienst zugeordnet ist, eine Standardlast angeben. Service Fabric berücksichtigt diese Last anschließend bei der Platzierung des Diensts oder immer dann, wenn der Dienst verschoben werden muss (z.B. bei Upgrades), um zu versuchen, die Knoten im Cluster in Einklang zu bringen.

Die anfänglich angegebene Standardlast für einen Dienst ändert sich während dessen Lebensdauer nicht. Um sich ändernde Metriken für einen bestimmten Dienst zu erfassen, wird empfohlen, dass Sie Ihren Dienst überwachen und dann die Last dynamisch melden. Dies ermöglicht Service Fabric, die Zuordnung basierend auf der gemeldeten Last zu einem gegebenen Zeitpunkt anzupassen. Verwenden Sie die [IServicePartition.ReportLoad](/dotnet/api/system.fabric.iservicepartition.reportload?view=azure-dotnet)-Methode, um benutzerdefinierte Metriken zu melden. Weitere Informationen finden Sie unter [Dynamische Last](/azure/service-fabric/service-fabric-cluster-resource-manager-metrics#dynamic-load).

## <a name="availability-considerations"></a>Überlegungen zur Verfügbarkeit

Platzieren Sie Ihre Dienste auf einem anderen Knotentyp als dem primären Knotentyp. Die Service Fabric-Systemdienste werden stets auf dem primären Knotentyp platziert. Wenn Ihre Dienste auf dem primären Knotentyp bereitgestellt werden, können sie mit Systemdiensten um Ressourcen konkurrieren und die Systemdienste stören. Wenn von einem Knotentyp erwartet wird, dass er zustandsbehaftete Dienste hostet, stellen Sie sicher, dass es mindestens fünf Knoteninstanzen gibt, und Sie wählen die Dauerhaftigkeitsstufe „Silber“ oder „Gold“.

Erwägen Sie das Einschränken der Ressourcen Ihrer Dienste. Siehe [Ressourcenkontrollmechanismus](/azure/service-fabric/service-fabric-resource-governance#resource-governance-mechanism).

- Mischen Sie auf demselben Knotentyp keine Dienste mit Ressourcenkontrolle mit Diensten ohne Ressourcenkontrolle. Die Dienste ohne Kontrolle nutzen ggf. zu viele Ressourcen, wodurch die Dienste mit Ressourcenkontrolle beeinträchtigt werden. Geben Sie [Platzierungseinschränkungen](/azure/service-fabric/service-fabric-cluster-resource-manager-advanced-placement-rules-placement-policies) an, um sicherzustellen, dass diese Arten von Diensten nicht in der gleichen Gruppe von Knoten ausgeführt werden. Siehe [Festlegen der Ressourcenkontrolle](/azure/service-fabric/service-fabric-resource-governance#specify-resource-governance). (Dies ist eine Beispiel des [Bulkhead-Musters](../../patterns/bulkhead.md).)
- Geben Sie die CPU-Kerne und Arbeitsspeichergröße an, die für eine Dienstinstanz reserviert werden. Informationen zur Verwendung und Einschränkungen von Richtlinien zur Ressourcenkontrolle finden Sie unter [Ressourcenkontrolle](/azure/service-fabric/service-fabric-resource-governance).

Stellen Sie sicher, dass die Anzahl der Zielinstanzen oder Replikate jedes Diensts größer als 1 ist, um einen Single Point of Failure (SPOF) zu vermeiden. Die größtmögliche Anzahl von Dienstinstanzen oder Replikaten ist gleich der Anzahl der Knoten, auf die der Dienst beschränkt ist.

Stellen Sie sicher, dass alle zustandsbehafteten Dienste mindestens über zwei aktive sekundäre Replikate verfügen. Für Produktionsworkloads werden fünf Replikate empfohlen.

Weitere Informationen finden Sie unter [Verfügbarkeit der Service Fabric-Dienste](/azure/service-fabric/service-fabric-availability-services).

## <a name="security-considerations"></a>Sicherheitshinweise

Hier sind einige wichtige Punkte zur Absicherung Ihrer Anwendung in Service Fabric:

### <a name="virtual-network"></a>Virtuelles Netzwerk

Erwägen Sie, Subnetzgrenzen für jede VM-Skalierungsgruppe zu definieren, um den Kommunikationsfluss zu steuern. Jeder Knotentyp hat seine eigene VM-Skalierungsgruppe, die in einem Subnetz innerhalb des virtuellen Netzwerks des Service Fabric-Clusters festgelegt ist. Netzwerksicherheitsgruppen (NSGs) können Subnetzen zum Zulassen oder Verweigern von Netzwerkdatenverkehr hinzugefügt werden. Beispielsweise können Sie bei Front-End- und Back-End-Knotentypen dem Back-End-Subnetz eine NSG hinzufügen, um eingehenden Datenverkehr nur für das Front-End-Subnetz zu akzeptieren.

Verwenden Sie beim Aufrufen externer Azure-Dienste im Cluster die [Dienstendpunkte des virtuellen Netzwerks](/azure/virtual-network/virtual-network-service-endpoints-overview), sofern der Azure-Dienst dies unterstützt. Durch die Verwendung eines Dienstendpunkts wird der Dienst nur im virtuellen Netzwerk des Clusters abgesichert. Wenn Sie beispielsweise Cosmos DB zum Speichern von Daten verwenden, konfigurieren Sie das Cosmos DB-Konto mit einem Dienstendpunkt, um nur den Zugriff aus einem bestimmten Subnetz zuzulassen. Siehe [Zugreifen auf Azure Cosmos DB-Ressourcen über virtuelle Netzwerke](/azure/cosmos-db/vnet-service-endpoint).

### <a name="endpoints-and-interservice-communication"></a>Endpunkte und Kommunikation zwischen Diensten

Erstellen Sie keinen ungeschützten Service Fabric-Cluster. Falls der Cluster Verwaltungsendpunkte im öffentlichen Internet verfügbar macht, können anonyme Benutzer eine Verbindung mit ihm herstellen. Nicht geschützte Cluster werden für Produktionsworkloads nicht unterstützt. Siehe: [Szenarien für die Clustersicherheit in Service Fabric](/azure/service-fabric/service-fabric-cluster-security).

So schützen Sie die Kommunikation zwischen Diensten

- Erwägen Sie in Ihren ASP.NET Core- oder Java-Webdiensten das Aktivieren von HTTPS-Endpunkten.
- Richten Sie eine sichere Verbindung zwischen dem Reverseproxy und Diensten ein.  Einzelheiten finden Sie unter [Herstellen einer Verbindung mit einem sicheren Dienst](/azure/service-fabric/service-fabric-reverseproxy-configure-secure-communication).

Wenn Sie ein [API-Gateway](../../microservices/design/gateway.md) verwenden, können Sie die [Authentifizierung an das Gateway auslagern](../../patterns/gateway-offloading.md). Stellen Sie sicher, dass die einzelnen Dienste nicht direkt (ohne das API-Gateway) erreicht werden können, es sei denn, es ist zusätzliche Sicherheit für die Authentifizierung von Nachrichten vorhanden, ob sie vom Gateway stammen.

Machen Sie den Service Fabric-Reverseproxy nicht öffentlich verfügbar. Dadurch sind alle Dienste, die HTTP-Endpunkte verfügbar machen, von außerhalb des Clusters adressierbar, wodurch Sicherheitsrisiken entstehen und möglicherweise zusätzliche Informationen außerhalb des Clusters unnötig preisgegeben werden. Wenn Sie auf einen Dienst öffentlich zugreifen möchten, verwenden Sie ein API-Gateway. Einige Optionen werden im Abschnitt [API-Gateway](#api-gateway) genannt.

Remotedesktop ist nützlich für die Diagnose und Problembehandlung. Sie sollten diese Anwendung jedoch unbedingt schließen, damit keine Sicherheitslücke entsteht.

### <a name="secrets-and-certificates"></a>Geheime Schlüssel und Zertifikate

Um aus einem Service Fabric-Dienst auf Key Vault-Geheimnisse zuzugreifen, aktivieren Sie [verwaltete Identität](/azure/active-directory/managed-identities-azure-resources/services-support-msi#azure-virtual-machine-scale-sets) für die VM-Skalierungsgruppe, die den Dienst hostet. Beispielcode: Verwenden Sie Key Vault aus App Service mit einer verwalteten Dienstidentität.

Verwenden Sie keine Clientzertifikate für den Zugriff auf Service Fabric Explorer. Nutzen Sie stattdessen Azure Active Directory (Azure AD). Siehe auch [Azure-Dienste, die die Azure AD-Authentifizierung unterstützen](/azure/active-directory/managed-identities-azure-resources/services-support-msi%23azure-services-that-support-azure-ad-authentication).

Verwenden Sie keine selbstsignierten Zertifikate in der Produktion.

Weitere Informationen zum Absichern von Service Fabric finden Sie unter:

- [Übersicht über die Azure Service Fabric-Sicherheit](/azure/security/azure-service-fabric-security-overview)
- [Bewährte Methoden für die Azure Service Fabric-Sicherheit](/azure/security/azure-service-fabric-security-best-practices)
- [Checkliste für die Azure Service Fabric-Sicherheit](/azure/security/azure-service-fabric-security-checklist)

## <a name="monitoring-considerations"></a>Aspekte der Überwachung

Bevor Sie sich mit den Überwachungsoptionen befassen, empfehlen wir Ihnen, diesen Artikel über die [Diagnose gängiger Szenarien mit Service Fabric](/azure/service-fabric/service-fabric-diagnostics-common-scenarios) zu lesen. Sie können sich Überwachungsdaten in diesen Kategorien vorstellen:

- [Anwendungsmetriken und -protokolle](#application-metrics-and-logs)
- [Service Fabric-Integritäts- und Ereignisdaten](#service-fabric-health-and-event-data)
- [Infrastrukturmetriken und -protokolle](#infrastructure-metrics-and-logs)
- [Metriken und Protokolle für abhängige Dienste](#dependent-service-metrics)

Es gibt zwei Hauptoptionen für die Analyse von Daten:

- Application Insights
- Log Analytics

Mit Azure Monitor können Sie Dashboards für die Überwachung einrichten und Warnmeldungen an Bediener senden. Es gibt auch einige Überwachungstools von Drittanbietern, die in Service Fabric integriert werden können, z.B. Dynatrace. Einzelheiten finden Sie unter [Partnerlösungen für die Überwachung von Azure Service Fabric](/azure/service-fabric/service-fabric-diagnostics-partners).

### <a name="application-metrics-and-logs"></a>Anwendungsmetriken und -protokolle

Die Anwendungstelemetrie liefert Daten über Ihren Dienst, die Ihnen helfen können, die Integrität Ihres Diensts zu überwachen und Probleme auszumachen. So fügen Sie Ihrem Dienst Ablaufverfolgungen und Ereignisse hinzu

- [Microsoft.Extensions.Logging](/azure/service-fabric/service-fabric-how-to-diagnostics-log#microsoftextensionslogging), wenn Sie Ihren Dienst mit ASP.NET Core entwickeln. Verwenden Sie für andere Frameworks eine Protokollierungsbibliothek Ihrer Wahl wie z.B. Serilog.
- Sie können Ihre eigene Instrumentierung hinzufügen, indem Sie die [TelemetryClient](/dotnet/api/microsoft.applicationinsights.telemetryclient?view=azure-dotnet)-Klasse im SDK verwenden und die Daten in Application Insights anzeigen. Siehe [Hinzufügen einer benutzerdefinierten Instrumentierung zu Ihrer Anwendung](/azure/service-fabric/service-fabric-tutorial-monitoring-aspnet#add-custom-instrumentation-to-your-application).
- Protokollieren von ETW-Ereignissen mit [EventSource](/azure/service-fabric/service-fabric-diagnostics-event-generation-app#eventsource). Diese Option ist standardmäßig in einer Visual Studio Service Fabric-Lösung verfügbar.

Verwenden Sie zum Anzeigen der Ablaufverfolgungs- und Ereignisprotokolle [Application Insights](/azure/service-fabric/service-fabric-diagnostics-event-analysis-appinsights) als eine der Senken für die strukturierte Protokollierung, damit Ihr Dienst Ablaufverfolgungen und Ereignisse visualisieren kann. Application Insights bietet zahlreiche integrierte Telemetriedaten: Anforderungen, Ablaufverfolgungen, Ereignisse, Ausnahmen, Metriken, Abhängigkeiten. Weitere Informationen zum Instrumentieren Ihres Diensts für Application Insights finden Sie in diesen Artikeln:

- [Tutorial: Überwachen und Diagnostizieren einer ASP.NET Core-Anwendung in Service Fabric mithilfe von Application Insights](/azure/service-fabric/service-fabric-tutorial-monitoring-aspnet)
- [Application Insights für ASP.NET Core](/azure/application-insights/app-insights-asp-net-core)
- [Application Insights .NET SDK](/azure/application-insights/app-insights-api-custom-events-metrics)
- [Application Insights SDK für Service Fabric](https://github.com/Microsoft/ApplicationInsights-ServiceFabric)

ASP.NET Core-Dienste verwenden für die Anwendungsprotokollierung die [ILogger-Schnittstelle](/aspnet/core/fundamentals/logging/?view=aspnetcore-2.2). Um diese Anwendungsprotokolle in Azure Monitor verfügbar zu machen, übertragen Sie die `ILogger`-Ereignisse an Application Insights. Weitere Informationen finden Sie unter [ILogger in einer ASP.NET Core-Anwendung](https://github.com/Microsoft/ApplicationInsights-dotnet-logging/blob/develop/src/ILogger/Readme.md#aspnet-core-application). Application Insights kann Korrelationseigenschaften zu ILogger-Ereignissen hinzufügen, was für die Visualisierung der verteilten Ablaufverfolgung nützlich ist.

Weitere Informationen finden Sie unter

- [Anwendungsprotokollierung](/azure/service-fabric/service-fabric-diagnostics-event-generation-app)
- [Hinzufügen von Protokollierung zur Service Fabric-Anwendung](/azure/azure-monitor/app/correlation)

### <a name="service-fabric-health-and-event-data"></a>Service Fabric-Integritäts- und Ereignisdaten

Service Fabric-Telemetrie enthält Integritätsmetriken und Ereignisse im Zusammenhang mit Betrieb und Leistung des Service Fabric-Clusters und seiner Entitäten: Knoten, Anwendungen, Dienste, Partitionen und Replikate.

- [EventStore](/azure/service-fabric/service-fabric-diagnostics-eventstore). Ein zustandsbehafteter Systemdienst, der Ereignisse im Zusammenhang mit dem Cluster und seinen Entitäten erfasst. Service Fabric verwendet EventStore, um [Service Fabric-Ereignisse](/azure/service-fabric/service-fabric-diagnostics-event-generation-operational) zu schreiben und Informationen über Ihren Cluster bereitzustellen, und kann für Statusaktualisierungen, Problembehandlung und Überwachung verwendet werden. Das Programm kann auch Ereignisse aus verschiedenen Entitäten zu einem bestimmten Zeitpunkt korrelieren, um Probleme im Cluster zu erkennen. Der Dienst macht diese Ereignisse über eine REST-API verfügbar. Informationen zum Abfragen der EventStore-APIs finden Sie unter [Abfragen von EventStore-APIs nach Clusterereignissen](/azure/service-fabric/service-fabric-diagnostics-eventstore-query). Sie können die Ereignisse aus EventStore in Log Analytics anzeigen, indem Sie Ihren Cluster mit der WAD-Erweiterung konfigurieren.
- [HealthStore](/azure/service-fabric/service-fabric-health-introduction). Bietet eine Momentaufnahme der aktuellen Integrität des Clusters. Ein zustandsbehafteter Dienst, der alle von Entitäten gemeldeten Integritätsdaten in einer Hierarchie aggregiert. Die Daten werden im [Service Fabric Explorer][sfx] visualisiert. HealthStore überwacht außerdem Anwendungsupgrades. Sie können Integritätsabfragen in PowerShell, einer .NET-Anwendung oder REST-APIs ausführen. Siehe [Einführung in Service Fabric-Integritätsüberwachung](/azure/service-fabric/service-fabric-health-introduction).
- Erwägen Sie das Implementieren interner benutzerdefinierter Watchdog-Dienste. Diese Dienste können in regelmäßigen Abständen benutzerdefinierte Integritätsdaten wie fehlerhafte Status ausgeführter Dienste melden. Weitere Informationen finden Sie unter [Benutzerdefinierte Integritätsberichte](/azure/service-fabric/service-fabric-report-health). Die Integritätsberichte können mithilfe von Service Fabric Explorer gelesen werden.  

### <a name="infrastructure-metrics-and-logs"></a>Infrastrukturmetriken und -protokolle

Infrastrukturmetriken fördern das Verständnis der Ressourcenzuteilung im Cluster. Es folgen die wichtigsten Optionen zum Sammeln dieser Informationen:

- Microsoft Azure-Diagnose für Windows (WAD). Dient zum Sammeln von Protokollen und Metriken auf Knotenebene unter Windows. Sie können WAD verwenden, indem Sie die VM-Erweiterung „IaaSDiagnostics“ für jede VM-Skalierungsgruppe konfigurieren, die einem Knotentyp zugeordnet ist. Anschließend können Sie Diagnoseereignisse sammeln, wie z.B. Windows-Ereignisprotokolle, Leistungsindikatoren, System- und Betriebsereignisse für ETW/Manifeste sowie benutzerdefinierte Protokolle.
- Log Analytics-Agent. Konfigurieren Sie die VM-Erweiterung „MicrosoftMonitoringAgent“, um Windows-Ereignisprotokolle, Leistungsindikatoren und benutzerdefinierte Protokolle an Log Analytics zu senden.

Es gibt einige Überschneidungen bei der Art der Metriken, die von den vorhergehenden Mechanismen gesammelt wurden, wie beispielsweise Leistungsindikatoren. Wenn eine Überlappung vorliegt, wird der Log Analytics-Agent empfohlen. Da es keinen Azure-Speicher für den Log Analytics-Agent gibt, kommt es zu einer geringen Latenz. Außerdem lassen sich die Leistungsindikatoren in „IaaSDiagnostics“ nicht einfach in Log Analytics einlesen.

![Infrastrukturüberwachung in Service Fabric](./_images/ra-sf-monitoring.png)

Informationen zu VM-Erweiterungen finden Sie unter [Erweiterungen und Features für virtuelle Azure-Computer](/azure/virtual-machines/extensions/overview).

Um die Daten anzuzeigen, konfigurieren Sie Log Analytics so, dass die über WAD erfassten Daten angezeigt werden. Informationen zum Konfigurieren von Log Analytics zum Lesen von Ereignissen aus einem Speicherkonto finden Sie unter [Einrichten von Log Analytics für einen Cluster](/azure/service-fabric/service-fabric-diagnostics-oms-setup).

Sie können auch Leistungsprotokolle und Telemetriedaten im Zusammenhang mit einem Service Fabric-Cluster, Workloads, Netzwerkdatenverkehr, anstehenden Updates und mehr anzeigen. Siehe [Leistungsüberwachung mit Log Analytics](/azure/service-fabric/service-fabric-diagnostics-oms-agent).

Die [Dienstzuordnungslösung in Log Analytics](/azure/azure-monitor/insights/service-map) bietet Informationen zur Topologie des Clusters (d.h. den Prozessen, die auf den einzelnen Knoten ausgeführt werden). Übertragen Sie die Daten im Speicherkonto an [Application Insights](/azure/monitoring-and-diagnostics/azure-diagnostics-configure-application-insights). Es gibt möglicherweise eine Verzögerung beim Abrufen von Daten in Application Insights. Wenn Sie die Daten in Echtzeit sehen möchten, sollten Sie [Event Hub](/azure/monitoring-and-diagnostics/azure-diagnostics-streaming-event-hubs) mit Senken und Kanälen konfigurieren. Weitere Informationen finden Sie unter [Ereignisaggregation und -sammlung mit der Windows Azure-Diagnose](/azure/service-fabric/service-fabric-diagnostics-event-aggregation-wad).

### <a name="dependent-service-metrics"></a>Metriken abhängiger Dienste.

- Die [Anwendungsübersicht zu Application Insights](/azure/azure-monitor/app/app-map) zeigt die Topologie der Anwendung unter Verwendung von HTTP-Abhängigkeitsaufrufen zwischen Diensten mit dem installierten Application Insights SDK.
- Die [Dienstzuordnungslösung in Log Analytics](/azure/azure-monitor/insights/service-map) bietet Informationen zum ein- und ausgehenden Datenverkehr externer Dienste. Darüber hinaus lässt sich die Dienstzuordnung in andere Lösungen wie Updates oder Sicherheit integrieren.
- Benutzerdefinierte Watchdogs können zum Melden von Fehlerbedingungen in externen Diensten verwendet werden. Der Dienst kann beispielsweise einen Bericht zu Integritätsfehlern melden, wenn er nicht auf einen externen Dienst oder Datenspeicher (Azure Cosmos DB) zugreifen kann.  

### <a name="distributed-tracing"></a>Verteilte Ablaufverfolgung

In der Microservices-Architektur nehmen oft mehrere Dienste an der Erledigung einer Aufgabe teil. Die Telemetriedaten von jedem dieser Dienste wird mithilfe von Kontextfeldern (Vorgangs-ID, Anforderungs-ID usw.) in einer verteilten Ablaufverfolgung korreliert. Mithilfe der [Anwendungszuordnung](/azure/azure-monitor/app/app-map) in Application Insights können Sie die Ansicht des verteilten logischen Betriebs erstellen und den gesamten Dienstgraphen Ihrer Anwendung visualisieren. Sie können auch die Transaktionsdiagnose in Application Insight verwenden, um serverseitige Telemetrie zu korrelieren. Weitere Informationen finden Sie unter [Einheitliche komponentenübergreifende Transaktionsdiagnose](/azure/application-insights/app-insights-transaction-diagnostics).

Die [Application Insights-Anwendungsübersicht](/azure/azure-monitor/app/app-map) zeigt die Topologie der Anwendung unter Verwendung von HTTP-Abhängigkeitsaufrufen zwischen Diensten mit dem installierten Application Insights SDK. Es ist auch wichtig, Aufgaben zu korrelieren, die asynchron mithilfe einer Warteschlange verteilt werden. Weitere Informationen zum Senden korrelierter Telemetriedaten in eine Warteschlange finden Sie unter [Warteschlangeninstrumentierung](/azure/azure-monitor/app/custom-operations-tracking#queue-instrumentation).

Weitere Informationen finden Sie unter

- [Durchführen einer mehrere Ressourcen übergreifenden Abfrage](/azure/azure-monitor/log-query/cross-workspace-query#performing-a-query-across-multiple-resources)
- [Telemetriekorrelation in Application Insights](/azure/azure-monitor/app/correlation)

#### <a name="alerts-and-dashboards"></a>Warnungen und Dashboards

Application Insights und Log Analytics unterstützen mit Kusto eine [umfassende Abfragesprache](/azure/log-analytics/query-language/get-started-queries), mit der Sie Protokolldaten abrufen und analysieren können. Verwenden Sie die Abfragen, um Datasets zu erstellen und in Diagnosedashboards zu visualisieren.

Verwenden Sie Azure Monitor-Warnungen, um Systemadministratoren zu benachrichtigen, wenn spezifische Bedingungen in bestimmten Ressourcen auftreten. Die Benachrichtigung kann eine E-Mail, eine Azure-Funktion, ein Aufruf eines Webhooks und dergleichen sein. Weitere Informationen finden Sie unter [Warnungen in Azure Monitor](/azure/azure-monitor/platform/alerts-overview).

[Warnungsregeln für die Protokollsuche](/azure/azure-monitor/platform/alerts-unified-log) ermöglichen Ihnen das Definieren und regelmäßige Anwenden einer Kusto-Abfrage auf einen Log Analytics-Arbeitsbereich. Eine Warnung wird erstellt, wenn das Ergebnis der Abfrage mit einer bestimmten Bedingung übereinstimmt.

## <a name="next-steps"></a>Nächste Schritte

- [Verwenden der Domänenanalyse zur Modellierung von Microservices](../../microservices/model/domain-analysis.md)
- [Entwerfen einer Microservices-Architektur](../../microservices/design/index.md)

[sfx]: /azure/service-fabric/service-fabric-visualizing-your-cluster
