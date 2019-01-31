---
title: Intelligente Produktsuchmaschine für E-Commerce
titleSuffix: Azure Example Scenarios
description: Stellen Sie eine erstklassige Sucherfahrung in einer E-Commerce-Anwendung bereit.
author: jelledruyts
ms.date: 09/14/2018
ms.topic: example-scenario
ms.service: architecture-center
ms.subservice: example-scenario
ms.custom: fasttrack
social_image_url: /azure/architecture/example-scenario/apps/media/architecture-ecommerce-search.png
ms.openlocfilehash: 12829c0b765831efdd08c8116a8fd847bd4e6abd
ms.sourcegitcommit: 3b15d65e7c35a19506e562c444343f8467b6a073
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/25/2019
ms.locfileid: "54908173"
---
# <a name="intelligent-product-search-engine-for-e-commerce"></a>Intelligente Produktsuchmaschine für E-Commerce

In diesem Beispielszenario wird veranschaulicht, wie durch die Verwendung eines dedizierten Suchdiensts die Relevanz der Suchergebnisse für Ihre E-Commerce-Kunden erheblich verbessert werden kann.

Die Suche ist der wichtigste Mechanismus, mit dem Kunden Produkte finden und diese dann erwerben. Daher ist es von entscheidender Bedeutung, dass Suchergebnisse in Bezug auf die _Absicht_ der Suchabfrage relevant sind und die End-to-End-Suchumgebung den Umgebungen von großen Suchanbietern entspricht (nahezu sofortige Anzeige von Ergebnissen und Funktionen wie linguistische Analyse, Abgleich des geografischen Standorts, Filterung, Faceting, automatische Vervollständigung, Treffermarkierung usw.).

Stellen Sie sich eine typische E-Commerce-Webanwendung vor, bei der die Produktdaten in einer relationalen Datenbank wie SQL Server oder Azure SQL-Datenbank gespeichert sind. Suchabfragen werden häufig innerhalb der Datenbank verarbeitet, indem Features wie `LIKE`-Abfragen oder die [Volltextsuche][docs-sql-fts] genutzt werden. Wenn Sie stattdessen [Azure Search][docs-search] verwenden, befreien Sie Ihre Betriebsdatenbank von der Verarbeitung der Abfragen und können auf einfache Weise die Vorteile der schwierig zu implementierenden Features nutzen, mit denen Ihre Kunden die bestmögliche Suchumgebung erhalten. Da Azure Search eine PaaS-Komponente (Platform-as-a-Service) ist, müssen Sie sich auch nicht um die Verwaltung der Infrastruktur kümmern oder zu einem Suchexperten werden.

## <a name="relevant-use-cases"></a>Relevante Anwendungsfälle

Zu den weiteren relevanten Anwendungsfällen zählen:

- Suchen nach Immobilienangeboten/-maklern in der Nähe des physischen Standorts des Benutzers
- Suchen nach Artikeln auf einer Nachrichtenwebsite oder nach Sportergebnissen, wobei _aktuelle_ Informationen bevorzugt werden
- Durchsuchen von großen Repositorys nach _dokumentorientierten_ Organisationen, z.B. Entscheidungsträgern und Notaren.

Letztendlich können _alle_ Anwendungen, die über eine Art von Suchfunktionalität verfügen, von einem dedizierten Suchdienst profitieren.

## <a name="architecture"></a>Architecture

![Architekturübersicht über die Azure-Komponenten, die an einer intelligenten Suchmaschine für Produkte im E-Commerce-Bereich beteiligt sind][architecture]

Mit diesem Szenario wird eine E-Commerce-Lösung abgedeckt, bei der Kunden einen Produktkatalog durchsuchen können.

1. Kunden navigieren von vielen unterschiedlichen Geräten aus zur **E-Commerce-Webanwendung**.
2. Der Produktkatalog wird in einer **Azure SQL-Datenbank**-Instanz für die Transaktionsverarbeitung verwaltet.
3. Azure Search nutzt eine **Indexerstellung für die Suche**, um den Suchindex per integrierter Nachverfolgung von Änderungen automatisch auf dem aktuellen Stand zu halten.
4. Suchabfragen von Kunden werden auf den **Azure Search**-Dienst ausgelagert, der die Abfrage verarbeitet und die relevantesten Ergebnisse zurückgibt.
5. Als Alternative zu einer webbasierten Suchumgebung können Kunden auch einen **Konversationsbot** in sozialen Netzwerken oder direkt über digitale Assistenten nutzen, um nach Produkten zu suchen und die Suchabfrage und die Ergebnisse inkrementell zu verfeinern.
6. Optional kann das Feature **Cognitive Search** verwendet werden, um künstliche Intelligenz zu nutzen und so eine noch intelligentere Verarbeitung zu erzielen.

### <a name="components"></a>Komponenten

- [App Services – Web-Apps][docs-webapps] hostet Webanwendungen und ermöglicht die automatische Skalierung und Bereitstellung von Hochverfügbarkeit, ohne eine Infrastruktur verwalten zu müssen.
- [SQL-Datenbank][docs-sql-database] ist ein relationaler verwalteter Datenbankdienst in Microsoft Azure für allgemeine Zwecke, der Strukturen wie relationale Daten, JSON, räumliche Daten und XML unterstützt.
- [Azure Search][docs-search] ist eine Search-as-a-Service-Cloudlösung, die umfangreiche Suchfunktionen für private, heterogene Inhalte in Web- und Unternehmensanwendungen sowie in mobilen Anwendungen ermöglicht.
- [Bot Service][docs-botservice] bietet Tools zum Erstellen, Testen, Bereitstellen und Verwalten intelligenter Bots.
- [Cognitive Services][docs-cognitive] ermöglicht die Verwendung intelligenter Algorithmen zum Sehen, Hören, Sprechen, Verstehen und Interpretieren der Wünsche von Benutzern bei Verwendung natürlicher Kommunikationsmethoden.

### <a name="alternatives"></a>Alternativen

- Sie können Funktionen für die **datenbankinterne Suche** verwenden, z.B. die SQL Server-Volltextsuche. In diesem Fall verarbeitet Ihr Transaktionsspeicher aber auch Abfragen (sodass eine höhere Verarbeitungsleistung benötigt wird), und die Suchfunktionen in der Datenbank sind stärker eingeschränkt.
- Sie können die Open-Source-Anwendung [Apache Lucene][apache-lucene] (worauf Azure Search basiert) für Azure Virtual Machines hosten, aber dann sind Sie wieder bei der IaaS-Verwaltung (Infrastructure-as-a-Service) angelangt und profitieren nicht von den vielen Features, die Azure Search zusätzlich zu Lucene bereitstellt.
- Sie können auch erwägen, [Elastic Search][elastic-marketplace] über den Azure Marketplace bereitzustellen. Dies ist eine Alternative und ein geeignetes Suchprodukt von einem Drittanbieter, aber auch in diesem Fall führen Sie eine IaaS-Workload aus.

Andere Optionen für die Datenschicht:

- [Cosmos DB](/azure/cosmos-db/introduction): Eine global verteilte Datenbank von Microsoft mit mehreren Modellen. Cosmos DB stellt eine Plattform für die Ausführung von anderen Datenmodellen bereit, z.B. Mongo DB, Cassandra, Graphdaten oder einfache Tabellenspeicherung. Azure Search unterstützt auch das direkte Indizieren von Daten über Cosmos DB.

## <a name="considerations"></a>Überlegungen

### <a name="scalability"></a>Skalierbarkeit

Der [Tarif][search-tier] des Azure Search-Diensts bestimmt nicht die verfügbaren Features, aber er wird hauptsächlich für die [Kapazitätsplanung][search-capacity] verwendet. Er definiert den maximalen Speicher, den Sie erhalten, und die Anzahl von Partitionen und Replikaten, die Sie bereitstellen können. Mit **Partitionen** können Sie mehr Dokumente indizieren und höhere Schreibdurchsätze erzielen, während **Replikate** mehr Abfragen pro Sekunde und Hochverfügbarkeit ermöglichen.

Sie können die Anzahl von Partitionen und Replikaten dynamisch ändern, aber es ist nicht möglich, den Tarif zu ändern. Aus diesem Grund sollten Sie sich sorgfältig überlegen, welcher Tarif für Ihre Zielworkload richtig ist. Wenn Sie den Tarif trotzdem ändern müssen, müssen Sie einen neuen Dienst parallel bereitstellen und Ihre Indizes dafür neu laden. An diesem Punkt können Sie für Ihre Anwendungen dann auf den neuen Dienst verweisen.

### <a name="availability"></a>Verfügbarkeit

Azure Search bietet eine [Vereinbarung zum Servicelevel mit einer Verfügbarkeit von 99,9%][search-sla] für _Lesevorgänge_ (also Abfragen), wenn Sie mindestens über zwei Replikate verfügen, und für _Updates_ (Aktualisierung der Suchindizes), wenn Sie mindestens über drei Replikate verfügen. Daher sollten Sie mindestens zwei Replikate bereitstellen, wenn Sie möchten, dass Ihre Kunden zuverlässig _suchen_ können. Stellen Sie drei Replikate bereit, wenn die tatsächlichen _Änderungen am Index_ ebenfalls als Hochverfügbarkeitsvorgänge angesehen werden sollten.

Falls grundlegende Änderungen am Index ohne Ausfallszeiten erforderlich sind (z.B. Änderung von Datentypen, Löschen oder Umbenennen von Feldern), muss der Index neu erstellt werden. Ähnlich wie beim Ändern des Tarifs bedeutet dies, dass ein neuer Index erstellt werden muss, dieser wieder mit den Daten gefüllt werden muss und Ihre Anwendungen dann aktualisiert werden müssen, damit sie auf den neuen Index verweisen.

### <a name="security"></a>Sicherheit

Azure Search erfüllt viele [Sicherheits- und Datenschutzstandards][search-security], sodass der Dienst in den meisten Branchen genutzt werden kann.

Zum Schützen des Zugriffs auf den Dienst werden für Azure Search zwei Arten von Schlüsseln verwendet: **Administratorschlüssel**, mit denen Sie für den Dienst _alle_ Aufgaben durchführen können, und **Abfrageschlüssel**, die nur für schreibgeschützte Vorgänge, z.B. Abfragen, verwendet werden können. Normalerweise wird der Index von der Anwendung, die die Suche durchführt, nicht aktualisiert. Sie sollte daher nur mit einem Abfrageschlüssel und nicht mit einem Administratorschlüssel konfiguriert werden (vor allem, wenn die Suche über ein Endbenutzergerät durchgeführt wird, z.B. mit einem in einem Webbrowser ausgeführten Skript).

### <a name="search-relevance"></a>Suchrelevanz

Wie erfolgreich Ihre E-Commerce-Anwendung ist, richtet sich größtenteils nach der Relevanz der Suchergebnisse für Ihre Kunden. Wenn Sie Ihren Suchdienst sorgfältig optimieren, um basierend auf der Recherche der Benutzer optimale Ergebnisse zu liefern, oder integrierte Features wie die [Analyse zum Suchdatenverkehr][search-analysis] verwenden, um die Suchmuster Ihrer Kunden zu verstehen, können Sie anhand dieser Daten Entscheidungen treffen.

Beispiele für Möglichkeiten zur Optimierung Ihres Suchdiensts sind:

- Verwendung von [Bewertungsprofilen][search-scoring], um die Relevanz von Suchergebnissen zu beeinflussen, z.B. basierend darauf, welches Feld die Übereinstimmung mit der Abfrage ergeben hat, wie aktuell die Daten sind, wie die geografische Entfernung zum Benutzer ist usw.
- Verwendung der [von Microsoft bereitgestellten Sprachanalysen][search-languages], für die ein Stapel zur Verarbeitung von natürlicher Sprache (Natural Language Processing, NLP) genutzt wird, um Abfragen besser interpretieren zu können
- Verwendung von [benutzerdefinierten Analysen][search-analyzers], um sicherzustellen, dass Ihre Produkte richtig gefunden werden (vor allem, wenn Sie nach nicht sprachbasierten Informationen suchen möchten, z.B. Marke und Modell eines Produkts)

## <a name="deploy-the-scenario"></a>Bereitstellen des Szenarios

Um eine umfassendere E-Commerce-Version dieses Szenarios bereitzustellen, können Sie [dieses Schritt-für-Schritt-Tutorial][end-to-end-walkthrough] durcharbeiten, in dem eine .NET-Beispielanwendung bereitgestellt wird, mit der ein einfaches Programm zum Kaufen von Tickets ausgeführt wird. Außerdem ist Azure Search enthalten, und es werden viele der beschriebenen Features genutzt. Darüber hinaus ist eine Resource Manager-Vorlage vorhanden, mit der die Bereitstellung der meisten Azure-Ressourcen automatisiert werden kann.

## <a name="pricing"></a>Preise

Zur Ermittlung der Betriebskosten für dieses Szenario sind alle oben erwähnten Dienste im Kostenrechner vorkonfiguriert. Wenn Sie wissen möchten, welche Kosten für Ihren spezifischen Anwendungsfall entstehen, können Sie die entsprechenden Variablen an Ihre voraussichtliche Nutzung anpassen.

Auf der Grundlage des zu erwartenden Datenverkehrsaufkommens haben wir drei exemplarische Kostenprofile erstellt:

- [Klein:][small-pricing] In diesem Profil verwenden wir eine einzelne `Standard S1`-Web-App zum Hosten der Website, den Free-Tarif des Azure Bot Service, eine einzelne `Basic`-Instanz des Azure Search-Diensts und eine SQL-Datenbank vom Typ `Standard S2`.
- [Mittel:][medium-pricing] Hier skalieren wir die Web-App vertikal auf zwei Instanzen des `Standard S3`-Tarifs hoch, upgraden den Search-Dienst auf den `Standard S1`-Tarif und verwenden eine SQL-Datenbank vom Typ `Standard S6`.
- [Groß:][large-pricing] Beim größten Profil verwenden wir vier Instanzen einer `Premium P2V2`-Web-App, upgraden den Azure Bot Service auf den `Standard S1`-Tarif (mit 1.000.000 Nachrichten in Premium-Kanälen) und verwenden zwei Einheiten des Azure Search-Diensts vom Typ `Standard S3` und eine SQL-Datenbank vom Typ `Premium P6`.

## <a name="related-resources"></a>Zugehörige Ressourcen

Weitere Informationen zu Azure Search finden Sie im [Dokumentationscenter][docs-search], und Sie können sich die [Beispiele][search-samples] oder eine voll funktionsfähige [Demowebsite][search-demo] in Aktion ansehen.

<!-- links -->
[architecture]: ./media/architecture-ecommerce-search.png
[docs-sql-fts]: /sql/relational-databases/search/query-with-full-text-search
[docs-search]: /azure/search/search-what-is-azure-search
[docs-sql-database]: /azure/sql-database/sql-database-technical-overview
[docs-webapps]: /azure/app-service/app-service-web-overview
[docs-botservice]: /azure/bot-service/
[docs-cognitive]: /azure/cognitive-services/
[apache-lucene]: https://lucene.apache.org/
[elastic-marketplace]: https://azuremarketplace.microsoft.com/marketplace/apps/elastic.elasticsearch
[end-to-end-walkthrough]: https://github.com/Azure/fta-customerfacingapps/tree/master/ecommerce/articles
[search-sla]: https://go.microsoft.com/fwlink/?LinkId=716855
[search-tier]: /azure/search/search-sku-tier
[search-capacity]: /azure/search/search-capacity-planning
[search-security]: /azure/search/search-security-overview
[search-analysis]: /azure/search/search-traffic-analytics
[search-languages]: /rest/api/searchservice/language-support
[search-analyzers]: /rest/api/searchservice/custom-analyzers-in-azure-search
[search-scoring]: /rest/api/searchservice/add-scoring-profiles-to-a-search-index
[search-samples]: https://azure.microsoft.com/resources/samples/?service=search&sort=0
[search-demo]: https://azjobsdemo.azurewebsites.net/
[small-pricing]: https://azure.com/e/db2672a55b6b4d768ef0060a8d9759bd
[medium-pricing]: https://azure.com/e/a5ad0706c9e74add811e83ef83766a1c
[large-pricing]: https://azure.com/e/57f95a898daa487795bd305599973ee6
