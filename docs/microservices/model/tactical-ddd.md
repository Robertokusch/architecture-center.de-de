# <a name="using-tactical-ddd-to-design-microservices"></a>Entwerfen von Microservices im Rahmen der taktischen DDD-Phase

Während der strategischen DDD-Phase entwerfen Sie die Geschäftsdomäne und definieren Kontextgrenzen für Ihre Domänenmodelle. In der taktischen DDD-Phase definieren Sie Ihre Domänenmodelle mit größerer Genauigkeit. Die taktischen Muster werden nur in einem Kontextgrenzenbereich angewendet. Bei einer Microservices-Architektur sind wir vor allem an den Entitäts- und Aggregatmustern interessiert. Die Anwendung dieser Muster hilft uns dabei, natürliche Grenzen für die Dienste in unserer Anwendung zu identifizieren. (Weitere Informationen finden Sie im [nächsten Artikel](./microservice-boundaries.md) dieser Reihe.) Es gilt das allgemeine Prinzip, dass ein Microservice nicht kleiner als ein Aggregat und nicht größer als eine Kontextgrenze sein sollte. Zuerst überprüfen wir die taktischen Muster. Anschließend wenden wir sie auf die Kontextgrenze „Lieferung“ in der Anwendung für die Drohnenlieferung an.

## <a name="overview-of-the-tactical-patterns"></a>Übersicht über die taktischen Muster

Dieser Abschnitt enthält eine kurze Zusammenfassung der taktischen DDD-Muster. Unter Umständen können Sie diesen Abschnitt also überspringen, falls Sie mit DDD bereits vertraut sind. Die Muster werden im Buch von Eric Evans (Kapitel 5 und 6) und in *Implementing Domain-Driven Design* von Vaughn Vernon ausführlicher beschrieben.

![Diagramm taktischer Muster für DDD (Domain-Driven Design)](../images/ddd-patterns.png)

**Entitäten**: Eine Entität ist ein Objekt mit einer eindeutigen Identität, die bestehen bleibt. In einer Anwendung für Bankgeschäfte sind Kunden und Konten beispielsweise Entitäten.

- Eine Entität verfügt im System über einen eindeutigen Bezeichner, mit dem die Entität nachgeschlagen bzw. abgerufen werden kann. Dies bedeutet nicht, dass der Bezeichner immer direkt für Benutzer verfügbar gemacht wird. Es kann sich um eine GUID oder einen Primärschlüssel in einer Datenbank handeln.
- Eine Identität kann übergreifend für mehrere Kontextgrenzenbereiche gelten und auch über die Lebensdauer der Anwendung hinaus beibehalten werden. Beispielsweise sind Bankkontonummern oder von Behörden ausgestellte IDs nicht an die Lebensdauer einer bestimmten Anwendung gebunden.
- Die Attribute einer Entität können sich im Laufe der Zeit ändern. Beispielsweise können sich Name oder Adresse einer Person ändern, während sich die Person nicht ändert.
- Eine Entität kann Verweise auf andere Entitäten enthalten.

**Wertobjekte**: Ein Wertobjekt hat keine Identität. Es wird allein durch die Werte seiner Attribute definiert. Außerdem sind Wertobjekte unveränderlich. Für die Aktualisierung eines Wertobjekts erstellen Sie immer eine neue Instanz, um die alte zu ersetzen. Wertobjekte können über Methoden verfügen, in denen die Domänenlogik gekapselt ist, aber diese Methoden sollten nicht mit Nebenwirkungen in Bezug auf den Status des Objekts verbunden sein. Typische Beispiele für Wertobjekte sind Farben, Datum und Uhrzeit und Währungswerte.

**Aggregate**: Ein Aggregat definiert eine Konsistenzgrenze für eine oder mehrere Entitäten. Eine bestimmte Entität in einem Aggregat ist die Stammentität. Die Suche wird anhand des Bezeichners der Stammentität durchgeführt. Alle anderen Entitäten im Aggregat sind untergeordnete Elemente der Stammentität, und es wird darauf verwiesen, indem den Zeigern der Stammentität gefolgt wird.

Der Zweck eines Aggregats ist die Modellierung von Transaktionsinvarianten. Reale Dinge weisen komplexe Beziehungsgeflechte auf. Kunden geben Bestellungen auf, Bestellungen enthalten Produkte, Produkte haben Lieferanten usw. Wenn von der Anwendung mehrere zusammengehörige Objekte geändert werden, wie kann dann die Konsistenz gewährleistet werden? Wie können Invarianten nachverfolgt und erzwungen werden?  

In herkömmlichen Anwendungen wurden häufig Datenbanktransaktionen eingesetzt, um Konsistenz zu erzwingen. In einer verteilten Anwendung ist dies aber oftmals nicht möglich. Eine einzelne Geschäftstransaktion kann sich unter Umständen über mehrere Datenspeicher erstrecken, eine lange Ausführungsdauer aufweisen oder Drittanbieterdienste umfassen. Letztendlich liegt es an der Anwendung und nicht an der Datenschicht, die für die Domäne erforderlichen Invarianten zu erzwingen. Für die Durchführung dieser Modellierung sind Aggregate bestimmt.

> [!NOTE]
> Ein Aggregat kann ggf. aus einer einzelnen Entität ohne untergeordnete Entitäten bestehen. Durch die Transaktionsgrenze wird dies zu einem Aggregat.

**Domänen- und Anwendungsdienste**: In der DDD-Terminologie ist ein Dienst ein Objekt, mit dem Logik implementiert wird, ohne dass ein Zustand vorgehalten wird. Evans unterscheidet zwischen *Domänendiensten*, in denen die Domänenlogik gekapselt ist, und *Anwendungsdiensten*, mit denen die technische Funktionalität bereitgestellt wird, z.B. die Benutzerauthentifizierung oder das Senden einer SMS-Nachricht. Domänendienste werden häufig zum Modellieren von Verhalten verwendet, das mehrere Entitäten umfasst.

> [!NOTE]
> Der Begriff *Dienst* ist im Bereich der Softwareentwicklung mehrfach besetzt. Die Definition bezieht sich in diesem Fall nicht direkt auf Microservices.

**Domänenereignisse**: Domänenereignisse können verwendet werden, um andere Teile des Systems zu benachrichtigen, wenn etwas passiert. Wie der Name bereits vermuten lässt, sollten Domänenereignisse eine bestimmte Bedeutung innerhalb der Domäne haben. Der Vorgang „Datensatz wurde in eine Tabelle eingefügt“ ist beispielsweise kein Domänenereignis. Der Vorgang „Lieferung wurde storniert“ ist ein Domänenereignis. Domänenereignisse sind besonders in einer Microservices-Architektur relevant. Da Microservices verteilt vorliegen und keine Datenspeicher gemeinsam nutzen, stellen Domänenereignisse eine Möglichkeit dar, wie sich Microservices untereinander koordinieren können. Im [Artikel zur Kommunikation zwischen Diensten](../design/interservice-communication.md) wird das asynchrone Messaging ausführlicher beschrieben.

Es gibt noch einige andere DDD-Muster, die hier nicht aufgeführt sind, z.B. Factorys, Repositorys und Module. Dies können nützliche Muster für die Implementierung eines Microservice sein, aber sie sind weniger relevant, wenn es um das Entwerfen von Grenzen zwischen Microservices geht.

## <a name="drone-delivery-applying-the-patterns"></a>Drohnenlieferung: Anwenden der Muster

Wir beginnen mit den Szenarien, die vom Kontextgrenzenbereich „Lieferung“ verarbeitet werden müssen.

- Ein Kunde kann eine Drohne anfordern, um Waren von einem Unternehmen abzuholen, das beim Dienst für die Drohnenlieferung registriert ist.
- Der Absender generiert eine Kennzeichnung per Tag (Strichcode oder RFID) für das Paket.
- Eine Drohne holt ein Paket ab und liefert es vom Ausgangsort an den Zielort.
- Wenn ein Kunde eine Lieferung plant, wird vom System eine geschätzte Ankunftszeit angegeben, die auf den Routeninformationen, Wetterbedingungen und Verlaufsdaten basiert.
- Wenn eine Drohne in der Luft ist, kann ein Benutzer den aktuellen Standort und die zuletzt ermittelte geschätzte Ankunftszeit nachverfolgen.
- Der Kunde kann eine Lieferung stornieren, bis eine Drohne das Paket abgeholt hat.
- Der Kunde wird benachrichtigt, wenn die Lieferung abgeschlossen ist.
- Der Absender kann vom Kunden eine Bestätigung der Lieferung in Form einer Signatur oder eines Fingerabdrucks anfordern.
- Benutzer können den Verlauf einer abgeschlossenen Lieferung anzeigen.

Anhand dieser Szenarien hat das Entwicklungsteam die folgenden **Entitäten** identifiziert.

- Lieferung
- Paket
- Drohne
- Konto
- Bestätigung
- Benachrichtigung
- Tag

Die ersten vier Entitäten (Lieferung, Paket, Drohne und Konto) sind allesamt **Aggregate**, die für die Grenzen der Transaktionskonsistenz stehen. Bestätigungen und Benachrichtigungen sind untergeordnete Elemente von Lieferungen, und Tags sind untergeordnete Elemente von Paketen.

Zu den **Wertobjekten** dieses Entwurfs gehören Standort, geschätzte Ankunftszeit, Paketgewicht und Paketgröße (Location, ETA, PackageWeight und PackageSize).

Zur besseren Veranschaulichung ist hier ein UML-Diagramm des Aggregats für die Lieferung (Delivery) angegeben. Beachten Sie, dass es Verweise auf andere Aggregate enthält, z.B. Konto (Account), Paket (Package) und Drohne (Drone).

![UML-Diagramm des Aggregats für die Lieferung (Delivery)](../images/delivery-entity.png)

Es sind zwei Domänenereignisse vorhanden:

- Während eine Drohne in der Luft ist, sendet die Drone-Entität DroneStatus-Ereignisse, mit denen der Standort und Status (Flug, Gelandet) der Drohne beschrieben werden.

- Die Delivery-Entität sendet jeweils DeliveryTracking-Ereignisse, wenn sich die Phase einer Lieferung ändert. Beispiele hierfür sind DeliveryCreated, DeliveryRescheduled, DeliveryHeadedToDropoff und DeliveryCompleted.

Beachten Sie, dass diese Ereignisse Dinge beschreiben, die innerhalb des Domänenmodells eine Bedeutung haben. Sie beschreiben einen Aspekt der Domäne und sind nicht an ein Konstrukt einer bestimmten Programmiersprache gebunden.

Das Entwicklungsteam hat noch einen weiteren Funktionalitätsbereich identifiziert, der nicht ohne Weiteres einer der bisher beschriebenen Entitäten zugeordnet werden kann. Ein Teil des Systems muss alle Schritte koordinieren, die an der Planung oder Aktualisierung einer Lieferung beteiligt sind. Aus diesem Grund hat das Entwicklungsteam dem Entwurf zwei **Domänendienste** hinzugefügt: einen *Scheduler* zum Koordinieren der Schritte und einen *Supervisor*, mit dem der Status der einzelnen Schritte überwacht und erkannt werden soll, ob für Schritte ein Fehler oder eine Zeitüberschreitung aufgetreten ist. Dies ist eine Variante des [Musters „Scheduler-Agent-Supervisor“](../../patterns/scheduler-agent-supervisor.md).

![Diagramm des überarbeiteten Domänenmodells](../images/drone-ddd.png)

## <a name="next-steps"></a>Nächste Schritte

Im nächsten Schritt werden die Grenzen für die einzelnen Microservices definiert.

> [!div class="nextstepaction"]
> [Identifizieren von Microservice-Grenzen](./microservice-boundaries.md)