---
title: 'CAF: Beispiele für Finanzergebnisse'
titleSuffix: Microsoft Cloud Adoption Framework for Azure
ms.service: architecture-center
ms.subservice: enterprise-cloud-adoption
ms.custom: governance
description: Beispiele für Finanzergebnisse
author: BrianBlanchard
ms.date: 10/10/2018
ms.topic: guide
ms.openlocfilehash: 5b28e22fca294bd1311d29ee5f70ca45e70fae3e
ms.sourcegitcommit: 579c39ff4b776704ead17a006bf24cd4cdc65edd
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/17/2019
ms.locfileid: "59640225"
---
# <a name="examples-of-fiscal-outcomes"></a>Beispiele für Finanzergebnisse

Auf der obersten Ebene stehen drei grundlegende Konzepte im Mittelpunkt von Gesprächen über Finanzergebnisse:

* Umsatz: Fließt durch den Verkauf von Waren oder Dienstleistungen mehr Geld in das Unternehmen?
* Kosten: Muss weniger Geld für die Erzeugung bzw. Erstellung, das Marketing, den Vertrieb oder die Bereitstellung von Waren oder Dienstleistungen ausgegeben werden.
* Gewinn: Obwohl es selten vorkommt, können einige Transformationen sowohl den Umsatz steigern als auch die Kosten senken. Dies ist ein Gewinnergebnis.

Im weiteren Verlauf dieses Artikels werden die oben genannten Finanzergebnisse im Kontext einer Cloudtransformation erläutert.

> [!NOTE]
> Die folgenden Beispiele sind hypothetisch und sollten nicht als Garantie für die mit der Einführung einer Cloudstrategie erzielbaren Erträge betrachtet werden.

## <a name="revenue-outcomes"></a>Umsatzergebnisse

### <a name="new-revenue-streams"></a>Neue Umsatzquellen

Die Cloud bietet die Möglichkeit, neue Produkte für Kunden anzubieten oder vorhandene Produkte auf neue Weise bereitzustellen. Viele Geschäftsleute sehen neue Umsatzquellen als eine innovative, unternehmerische und spannende Chance. Neue Umsatzquellen sind jedoch auch fehleranfällig und werden in vielen Unternehmen als hohes Risiko betrachtet. Werden sie von der IT vorgeschlagen, ist die Wahrscheinlichkeit hoch, dass sie abgelehnt oder zurückgestellt werden. Um diesen Ergebnissen Glaubwürdigkeit zu verleihen, sollten Sie sich mit einem Geschäftsführer zusammentun, der sich bereits als Innovator positioniert hat. Durch die Überprüfung der Umsatzquelle in einer frühen Phase des Prozesses lassen sich Hindernisse von Seiten des Unternehmens vermeiden.

* Beispiel: Ein Unternehmen verkauft seit mehr als 100 Jahren Bücher. Ein Mitarbeiter des Unternehmens erkennt, dass die Inhalte auf elektronischem Weg bereitgestellt werden können. Er entwickelt ein Gerät, das den direkten Download der gleichen Bücher ermöglicht und in der Buchhandlung verkauft werden kann. Dadurch werden für Bücher neue Verkäufe mit einem Umsatz von X US-Dollar erzielt.

### <a name="revenue-increases"></a>Umsatzsteigerungen

Dank globaler Skalierbarkeit und digitaler Reichweite bietet die Cloud Unternehmen die Möglichkeit, den mit bestehenden Umsatzquellen erzielten Umsatz zu steigern. Oftmals ist ein solches Ergebnis auf eine Abstimmung mit der Vertriebs- oder Marketingleitung zurückzuführen.

* Beispiel: Ein Unternehmen, das Widgets verkauft, könnte mehr Widgets verkaufen, wenn die Vertriebsmitarbeiter sicheren Zugriff auf den digitalen Katalog und die Lagerbestände des Unternehmens hätten. Leider sind diese Daten nur im ERP-System des Unternehmens verfügbar, auf das nur über ein mit dem Netzwerk verbundenes Gerät zugegriffen werden kann. Durch das Erstellen einer mit dem ERP-System verbundenen Dienstfassade und das Verfügbarmachen der Katalogliste und nicht sensiblen Bestandsmengen in einer Anwendung in der Cloud könnten die Vertriebsmitarbeiter vor Ort beim Kunden auf die von ihnen benötigten Daten zugreifen. Die Erweiterung von Active Directory mit Azure AD und die Integration des rollenbasierten Zugriffs in die Anwendung würde es dem Unternehmen ermöglichen, den Schutz der Daten sicherzustellen. Dieses einfache Projekt könnte den Umsatz für eine vorhandene Produktlinie um X % steigern.

### <a name="profit-increases"></a>Gewinnsteigerungen

Nur selten können mit einem einzigen Projekt gleichzeitig der Umsatz gesteigert und die Kosten gesenkt werden. Wenn dies der Fall ist, sollte die Darstellung eines oder mehrerer Umsatzergebnisse jedoch mit mindestens einem Kostenergebnis abgestimmt werden, um das gewünschte Ergebnis zu kommunizieren.

## <a name="cost-outcomes"></a>Kostenergebnisse

### <a name="cost-reduction"></a>Kostensenkung

Cloud Computing kann die Investitionskosten (CapEx, Capital Expenses) im Zusammenhang mit dem Erwerb von Hardware und Software, der Einrichtung von Datencentern, dem Betrieb von lokalen Datencentern usw. senken. Die Kosten für Serverracks, Stromversorgung und Kühlung rund um die Uhr sowie IT-Experten für die Verwaltung der Infrastruktur summieren sich schnell. Durch die Stilllegung eines Datencenters kann die Kapitalbindung reduziert werden. Diese Vorgehensweise wird im Allgemeinen als „Ausstieg aus dem Datencenterbetrieb“ bezeichnet. Die Kostensenkung wird in der Regel an den Mitteln im aktuellen Budget gemessen, das je nach Finanzmanagement des CFO (Chief Financial Officer) ein bis fünf Jahre umfassen kann.

* Beispiel 1: Das Datencenter eines Unternehmens verschlingt einen großen Teil des jährlichen IT-Budgets. Die IT entscheidet sich, eine betriebliche Transformation durchzuführen, und migriert die Ressourcen in diesem Datencenter zu IaaS-Lösungen (Infrastructure-as-a-Service), wodurch die Kosten im Laufe der nächsten drei Jahren gesenkt werden.
* Beispiel 2: Eine Holdinggesellschaft hat vor Kurzem ein neues Unternehmen erworben. In den Übernahmebedingungen ist festgelegt, dass die neue Entität innerhalb von sechs Monaten aus den aktuellen Datencentern entfernt werden muss. Andernfalls muss die Holdinggesellschaft ein Bußgeld in Höhe von 1 Millionen US-Dollar pro Monat zahlen. Die Migration der digitalen Ressourcen zur Cloud im Rahmen einer betrieblichen Transformation könnte eine schnelle Außerbetriebnahme der alten Ressourcen ermöglichen.
* Beispiel 3: Ein Unternehmen, das Einkommensteuerberatung für Verbraucher anbietet, verzeichnet 70 % seines Jahresumsatzes in den ersten drei Monaten des Jahrs. Während des restlichen Jahrs bleiben die erheblichen IT-Investitionen des Unternehmens relativ ungenutzt. Eine betriebliche Transformation würde es der IT-Abteilung ermöglichen, die erforderliche Compute-/Hostingkapazität für diese drei Monate bereitzustellen. Während der verbleibenden neun Monate könnten die IaaS-Kosten durch Verringern des Computebedarfs erheblich reduziert werden.

### <a name="coverdell"></a>Coverdell

Coverdell modernisiert seine Infrastruktur und erzielt mit Azure rekordverdächtige Kosteneinsparungen. Die Entscheidung von Coverdell, in Azure zu investieren und sein Netzwerk von Websites, Anwendungen, Daten und Infrastruktur in dieser Umgebung zu vereinen, führte zu höheren Kosteneinsparungen als das Unternehmen jemals erwartet hätte. Durch die Migration zu einer reinen Azure-Umgebung konnten 54.000 US-Dollar an monatlichen Kosten für Co-Location-Dienste eingespart werden. Allein durch die neue, vereinte Infrastruktur des Unternehmens erwartet Coverdell in den nächsten zwei bis drei Jahren Einsparungen von etwa einer Million US-Dollar.
„Der Zugang zum Azure-Technologiestapel ebnet den Weg für skalierbare, einfach zu implementierende und hoch verfügbare Lösungen, die kostengünstig sind. Dadurch können unsere Architekten die von ihnen bereitgestellten Lösungen deutlich kreativer gestalten.“
Ryan Sorensen, Director of Application Development and Enterprise Architecture, Coverdell

### <a name="cost-avoidance"></a>Kostenvermeidung

Durch die Stilllegung von Datencentern lassen sich auch zukünftige Aktualisierungszyklen und somit Kosten vermeiden. Der Kauf neuer Hardware und Software, um veraltete lokale Systeme zu ersetzen, wird als Aktualisierungszyklus bezeichnet. In Azure werden die Hardware und das Betriebssystem ohne zusätzliche Kosten für den Kunden regelmäßig gewartet, gepatcht und aktualisiert. Dies versetzt einen CFO in die Lage, geplante zukünftige Ausgaben aus langfristigen finanziellen Prognosen zu streichen. Die Kostenvermeidung wird in Euro gemessen. Sie unterscheidet sich von der Kostensenkung, bei der es in der Regel um ein zukünftiges Budget geht, das noch nicht vollständig genehmigt wurde.

* Beispiel: Für das Datencenter eines Unternehmens steht in sechs Monaten eine Verlängerung des Mietvertrags an. Das Datencenter ist seit acht Jahren in Betrieb. Vor vier Jahren hat das Unternehmen mehrere Millionen US-Dollar in die Aktualisierung und Virtualisierung aller Server investiert. Im nächsten Jahr sollen die Hardware und Software erneut aktualisiert werden. Durch die Migration der Ressourcen in diesem Datencenter im Rahmen einer betrieblichen Transformation würden Kosten vermieden, da die geplante Aktualisierung im prognostizierten Budget des nächsten Jahres wegfallen würde. Zudem könnte sie zu einer Kostensenkung führen, da die Mietkosten verringert oder entfallen würden.

### <a name="capex-versus-opex"></a>Investitionskosten und Betriebskosten im Vergleich

Bevor wir näher auf die Kostenergebnisse eingehen, ist das Verständnis der zwei primären Kostenoptionen wichtig: Investitionskosten (CapEx, Capital Expenses) und Betriebskosten (Operating Expenses, OpEx).

Die folgenden Begriffsdefinitionen sollen die Unterschiede zwischen Investitions- und Betriebskosten verdeutlichen und die Gespräche erleichtern, die Sie im Unternehmen über Ihre Transformation führen.

* **Kapital** sind die finanziellen Mittel oder Aktiva eines Unternehmens, die für einen bestimmten Zweck zur Verfügung stehen, z. B. die Erhöhung der Serverkapazität oder die Erstellung einer Anwendung.
* **Investitionskosten** sind Ausgaben, die langfristig betrachtet einen Nutzen generieren. Eine solche Ausgabe ist normalerweise nicht wiederkehrend und führt zum Erwerb von Vermögenswerten. Die Kosten für die Erstellung einer Anwendung können als Investitionskosten eingestuft werden.
* **Betriebskosten** sind Ausgaben, bei denen es sich um laufende Kosten der Geschäftstätigkeit eines Unternehmens handelt. Die Nutzung von Clouddiensten in einem nutzungsbasierten Bezahlungsmodell kann als Betriebsausgaben eingestuft werden.
* Ein Vermögenswert ist eine wirtschaftliche Ressource, die zum Generieren von Mehrwert dient oder genutzt wird. Server, Data Lakes, Anwendungen usw. können als Vermögenswerte eingestuft werden.
* **Abschreibung** definiert, wie der Wert eines Vermögenswerts im Laufe der Zeit abnimmt. Von größerer Bedeutung für das Gespräch über Investitions- und Betriebskosten ist die Zuordnung eines Vermögenswerts im Laufe der Zeiträume, in denen er genutzt wird. Wenn Sie beispielsweise in diesem Jahr eine Anwendung erstellen, deren erwartete durchschnittliche Lebensdauer jedoch fünf Jahre beträgt (wie bei den meisten kommerziellen Anwendungen), werden die Kosten für das Entwicklungsteam und die erforderlichen Tools zum Entwickeln und Bereitstellen der Codebasis gleichmäßig über fünf Jahre abgeschrieben.
* **Bewertung** ist der Prozess, durch den der Wert eines Unternehmens geschätzt wird. In den meisten Branchen beruht die Bewertung auf der Fähigkeit des Unternehmens, unter Berücksichtigung der erforderlichen Betriebskosten für die Erzeugung der entsprechenden Waren Umsatz und Gewinn zu generieren. In manchen Branchen, beispielsweise im Einzelhandel, oder bei Transaktionstypen wie Private Equity können Vermögenswerte und die Abschreibung eine wichtige Rolle bei der Bewertung des Unternehmens spielen.

Fast immer ist davon auszugehen, dass verschiedene Führungskräfte (einschließlich des CIO) darüber debattieren, wie das Kapital optimal genutzt werden sollte, um das Unternehmen in der gewünschten Richtung voranzubringen. Dem CIO die Möglichkeit zu geben, nicht die von Konkurrenz geprägten Gespräche über Investitionskosten führen zu müssen, sondern konkrete Aussagen bezüglich der Betriebskostenkontrolle treffen zu können, kann allein schon ein gutes Ergebnis sein. In vielen Branchen suchen CFOs aktiv nach Möglichkeiten zur besseren Zuordnung der finanziellen Verantwortlichkeit zu den Kosten der verkauften Waren.

Bevor eine Transformation mit einer solchen Umrechnung der Investitionskosten in Betriebskosten in Verbindung gebracht wird, empfiehlt es sich jedoch, in Treffen mit den CFO- oder CIO-Teams herauszufinden, ob das Unternehmen Investitions- oder Betriebskostenstrukturen bevorzugt. In einigen Organisationen ist die Senkung der Investitionskosten zugunsten der Betriebskosten tatsächlich ein äußerst wünschenswertes Ergebnis. Wie oben erwähnt, ist dies mitunter bei Einzelhandels-, Holding- und Private Equity-Unternehmen der Fall, die viel Wert auf herkömmliche Anlagenrechnungsmodelle, aber wenig Wert auf intellektuelles Eigentum legen. Auch bei Unternehmen, die in der Vergangenheit negative Erfahrungen mit der Auslagerung von IT-Mitarbeitern oder anderen Funktionen gemacht haben, ist dies zu beobachten.

Für Fälle, in denen die Betriebskosten im Mittelpunkt stehen, kann das folgende Beispiel ein tragfähiges Geschäftsergebnis sein:

* Beispiel: Das Datencenter eines Unternehmens wird derzeit jährlich mit X US-Dollar für die nächsten drei Jahre abgeschrieben. In den nächsten Jahren wird für eine Aktualisierung der Hardware ein zusätzlicher Kostenaufwand von Y US-Dollar erwartet. All diese Investitionskosten können mit einer gleichmäßig verteilten Rate von Z US-Dollar/Monat umgerechnet werden, wodurch eine bessere Verwaltung und Kontrolle der Betriebskosten für die Technologie möglich ist.
