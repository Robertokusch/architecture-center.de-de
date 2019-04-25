# <a name="choosing-a-compute-option-for-microservices"></a>Auswählen einer Computeoption für Microservices

Der Begriff *Compute* bezieht sich auf das Hostingmodell für die Computeressourcen, auf denen Ihre Anwendung ausgeführt wird. Bei einer Microservices-Architektur werden bevorzugt die beiden folgenden Ansätze verwendet:

- Ein Dienstorchestrator, der Dienste auf dedizierten Knoten (virtuellen Computern) verwaltet
- Eine serverlose Architektur mit Funktionen als Dienst (Functions-as-a-Service, FaaS)

Dies sind zwar nicht die einzigen Optionen, sie haben sich aber bei der Erstellung von Microservices bewährt. Eine Anwendung kann beide Ansätze umfassen.

## <a name="service-orchestrators"></a>Dienstorchestratoren

Ein Orchestrator bewältigt Aufgaben im Zusammenhang mit der Bereitstellung und Verwaltung einer Gruppe von Diensten. Diese Aufgaben umfassen das Platzieren von Diensten auf Knoten, das Überwachen der Integrität von Diensten, das Neustarten fehlerhafter Dienste, das Durchführen eines Lastenausgleichs für den Netzwerkdatenverkehr der Dienstinstanzen, das Ermitteln von Diensten, das Skalieren der Anzahl von Dienstinstanzen sowie das Anwenden von Konfigurationsaktualisierungen. Beliebte Orchestratoren sind Kubernetes, Service Fabric, DC/OS und Docker Swarm.

Berücksichtigen Sie die folgenden Optionen auf der Azure-Plattform:

- [Azure Kubernetes Service](/azure/aks/) (AKS) ist ein Managed Kubernetes-Dienst. AKS stellt Kubernetes bereit und macht die Kubernetes-API-Endpunkte verfügbar, hostet und verwaltet aber die Kubernetes-Steuerungsebene und führt somit automatisch Upgrade-, Patch- und andere Verwaltungsaufgaben durch. AKS können Sie sich als „Kubernetes-APIs als Dienst“ vorstellen.

- [Service Fabric](/azure/service-fabric/) ist eine Plattform für verteilte Systeme, mit der Microservices gepackt, bereitgestellt und verwaltet werden können. Microservices können in Service Fabric als Container, als binäre ausführbare Dateien oder als [Reliable Services](/azure/service-fabric/service-fabric-reliable-services-introduction) bereitgestellt werden. Mit dem Reliable Services-Programmiermodell können Dienste direkt über Service Fabric-APIs das System abfragen, die Integrität melden, Benachrichtigungen zu Konfigurations- und Codeänderungen empfangen sowie andere Dienste ermitteln. Ein zentraler Unterschied von Service Fabric ist der starke Fokus auf die Erstellung zustandsbehafteter Dienste mit [Reliable Collections](/azure/service-fabric/service-fabric-reliable-services-reliable-collections).

- Andere Optionen, wie etwa Docker Enterprise Edition und Mesosphere DC/OS, können in einer IaaS-Umgebung auf Azure ausgeführt werden. Bereitstellungsvorlagen finden Sie im [Azure Marketplace](https://azuremarketplace.microsoft.com).

## <a name="containers"></a>Container

Container und Microservices werden gelegentlich in einen Topf geworfen. Das ist allerdings nicht angebracht: Für die Erstellung von Microservices werden keine Container benötigt. Container bieten jedoch einige Vorteile, die besonders für Microservices relevant sind:

- **Übertragbarkeit:** Ein Containerimage ist ein eigenständiges Paket, für dessen Ausführung weder Bibliotheken noch andere Abhängigkeiten installiert werden müssen. Dadurch lassen sie sich ganz einfach bereitstellen. Container können schnell gestartet und beendet werden, um bei Knotenausfällen oder zur Bewältigung einer höheren Last neue Instanzen zu aktivieren.

- **Dichte:** Im Vergleich zur Ausführung eines virtuellen Computers weisen Container eine geringere Dichte auf, da sie Betriebssystemressourcen gemeinsam nutzen. Dadurch können sich mehrere Container auf einem einzelnen Knoten befinden, was besonders hilfreich ist, wenn sich die Anwendung aus vielen kompakten Diensten zusammensetzt.

- **Ressourcenisolierung:** Sie können die verfügbare Arbeitsspeicher- und CPU-Kapazität für einen Container einschränken, um zu verhindern, dass ein außer Kontrolle geratener Prozess sämtliche Hostressourcen beansprucht. Weitere Informationen finden Sie unter [Bulkhead-Muster](../../patterns/bulkhead.md).

## <a name="serverless-functions-as-a-service"></a>Serverlos (Funktionen als Dienst)

Bei einer [serverlosen](https://azure.microsoft.com/solutions/serverless/) Architektur verwalten Sie nicht die virtuellen Computer oder die virtuelle Netzwerkinfrastruktur. Stattdessen stellen Sie Code bereit, der dann durch den Hostingdienst auf einem virtuellen Computer platziert und ausgeführt wird. Dieser Ansatz bevorzugt eher kompakte, präzise Funktionen, die mithilfe ereignisbasierter Trigger koordiniert werden. So kann beispielsweise eine Nachricht, die in einer Warteschlange platziert wird, eine Funktion auslösen, die die Nachricht aus der Warteschlange liest und sie verarbeitet.

[Azure Functions](/azure/azure-functions/) ist ein serverloser Computedienst, der verschiedene Funktionstrigger wie etwa HTTP-Anforderungen, Service Bus-Warteschlangen und Event Hubs-Ereignisse unterstützt. Eine vollständige Liste finden Sie unter [Konzepte für Azure Functions-Trigger und -Bindungen](/azure/azure-functions/functions-triggers-bindings). Erwägen Sie außerdem die Verwendung von [Azure Event Grid](/azure/event-grid/). Hierbei handelt es sich um einen verwalteten Ereignisroutingdienst in Azure.

<!-- markdownlint-disable MD026 -->

## <a name="orchestrator-or-serverless"></a>Orchestrator oder serverlos?

<!-- markdownlint-enable MD026 -->

Im Anschluss finden Sie einige Faktoren, die Sie bei der Wahl eines Orchestratoransatzes oder eines serverlosen Ansatzes berücksichtigen sollten.

**Verwaltbarkeit:** Eine serverlose Anwendung lässt sich einfach verwalten, da Ihnen die Plattform die Verwaltung sämtlicher Computeressourcen abnimmt. Ein Orchestrator abstrahiert zwar einige Aspekte der Clusterverwaltung und -konfiguration, kann die zugrunde liegenden virtuellen Computer jedoch nicht vollständig verbergen. Bei Verwendung eines Orchestrators müssen Sie sich mit Aspekten wie Lastenausgleich, CPU-/Arbeitsspeicherauslastung und Netzwerk auseinandersetzen.

**Flexibilität und Steuerung:** Ein Orchestrator bietet umfangreiche Steuerungsmöglichkeiten für die Konfiguration und Verwaltung Ihrer Dienste und des Clusters. Dafür müssen Sie jedoch eine höhere Komplexität in Kauf nehmen. Bei einer serverlosen Architektur geben Sie ein gewisses Maß an Kontrolle auf, da diese Details abstrahiert werden.

**Übertragbarkeit:** Alle hier aufgeführten Orchestratoren (Kubernetes, DC/OS, Docker Swarm und Service Fabric) können lokal oder in mehreren öffentlichen Clouds ausgeführt werden.

**Anwendungsintegration:** Die Erstellung einer komplexen Anwendung mit einer serverlosen Architektur ist unter Umständen nicht ganz einfach. Eine Option in Azure ist die Koordinierung einer Gruppe von Azure-Funktionen mit [Azure Logic Apps](/azure/logic-apps/). Ein Beispiel für diese Methode finden Sie unter [Erstellen einer Funktion, die in Azure Logic Apps integriert ist](/azure/azure-functions/functions-twitter-email).

**Kosten**: Bei Verwendung eines Orchestrators bezahlen Sie für die virtuellen Computer, die im Cluster ausgeführt werden. Bei einer serverlosen Anwendung bezahlen Sie nur für die tatsächlich genutzten Serverressourcen. In beiden Fällen müssen Sie die Kosten für zusätzliche Dienste wie Speicher, Datenbanken und Messaging berücksichtigen.

**Skalierbarkeit**. Azure Functions wird auf der Grundlage der Anzahl eingehender Ereignisse automatisch nach Bedarf skaliert. Bei Verwendung eines Orchestrators können Sie horizontal hochskalieren, indem Sie die Anzahl der im Cluster ausgeführten Dienstinstanzen erhöhen. Darüber hinaus können Sie dem Cluster zur Skalierung weitere virtuelle Computer hinzufügen.

In unserer Referenzimplementierung setzen wir in erster Linie auf Kubernetes. Für den Dienst „Delivery History“ haben wir allerdings Azure Functions verwendet. Aufgrund der ereignisgesteuerten Workload eignet sich Azure Functions gut für diesen speziellen Dienst. Da die Funktion über einen Event Hubs-Trigger aufgerufen wird, war für den Dienst nur sehr wenig Code erforderlich. Der Dienst „Delivery History“ gehört außerdem nicht zum Hauptworkflow, sodass die Ausführung außerhalb des Kubernetes-Clusters keine Auswirkungen auf die End-to-End-Wartezeit der von Benutzern initiierten Vorgänge hat.

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Kommunikation zwischen Diensten](./interservice-communication.md)
