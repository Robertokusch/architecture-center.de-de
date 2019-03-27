---
title: Erstellen eines für Unternehmen konzipierten interaktiven Bots
description: Erfahren Sie, wie Sie mit Azure Bot Framework einen für Unternehmen geeigneten interaktiven Bot (Chatbot) erstellen.
author: roalexan
ms.date: 01/24/2019
ms.topic: reference-architecture
ms.service: architecture-center
ms.subservice: reference-architecture
ms.custom: azcat-ai
ms.openlocfilehash: 0f5de0eca6fbd35cca1a0e8443f363df09ffc6aa
ms.sourcegitcommit: c053e6edb429299a0ad9b327888d596c48859d4a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/20/2019
ms.locfileid: "58248695"
---
# <a name="enterprise-grade-conversational-bot"></a>Für Unternehmen konzipierter interaktiver Bot

Diese Referenzarchitektur beschreibt, wie Sie mit [Azure Bot Framework][bot-framework] einen für Unternehmen geeigneten interaktiven Bot (Chatbot) erstellen. Jeder Bot ist anders, es gibt jedoch einige allgemeine Muster, Workflows und Technologien, die Ihnen bekannt sein sollten. Insbesondere bei einem Bot für Unternehmensworkloads müssen neben den Kernfunktionen viele Überlegungen zum Entwurf berücksichtigt werden. In diesem Artikel werden die wichtigsten Entwurfsaspekte erläutert und die erforderlichen Tools zum Erstellen eines stabilen, sicheren Bots, der aktives Lernen unterstützt, vorgestellt.

[![Diagramm der Architektur][0]][0]

Bei den Hilfsprogrammbeispielen, die in dieser Architektur als bewährte Methoden verwendet werden, handelt es sich um reine Open-Source-Beispiele, die auf [GitHub][git-repo-base] verfügbar sind. 

## <a name="architecture"></a>Architecture

Die hier gezeigte Architektur verwendet die folgenden Azure-Dienste. Ihr Bot kann beliebig viele dieser Dienste nutzen, und zudem können Sie zusätzliche Dienste einbinden.

### <a name="bot-logic-and-user-experience"></a>Botlogik und Benutzererfahrung

- **[Bot Framework-Dienst][bot-framework-service]** (Bot Framework Service, BFS). Dieser Dienst verbindet Ihren Bot mit einer Kommunikations-App wie Cortana, Facebook Messenger oder Slack. Er ermöglicht die Kommunikation zwischen Ihrem Bot und dem Benutzer.
- **[Azure App Service][app-service]**. Die Anwendungslogik des Bots wird in Azure App Service gehostet.

### <a name="bot-cognition-and-intelligence"></a>Kognitive und intelligente Funktionen des Bots

- **[Language Understanding Intelligent Service][luis]** (LUIS). LUIS ist Teil von [Azure Cognitive Services][cognitive-services]. Der Dienst identifiziert Absichten des Benutzers sowie Entitäten und ermöglicht Ihrem Bot so das Verständnis natürliche Sprache.
- **[Azure Search][search]**. Search ist ein verwalteter Dienst, der einen schnell durchsuchbaren Dokumentindex bereitstellt.
- **[QnA Maker][qna-maker]**. QnA Maker ist ein cloudbasierter API-Dienst, mit dem eine Frage-und-Antwort-Ebene im Konversationsstil für Ihre Daten erstellt wird. In der Regel werden teilweise strukturierte Inhalte wie häufig gestellte Fragen (Frequently Asked Questions, FAQs) in den Dienst geladen. Verwenden Sie QnA Maker, um eine Wissensdatenbank zum Beantworten von Fragen in natürlicher Sprache zu erstellen.
- **[Web-App][webapp]**. Wenn Ihr Bot KI-Lösungen (künstliche Intelligenz) erfordert, die nicht von einem vorhandenen Dienst bereitgestellt werden, können Sie Ihre eigene benutzerdefinierte KI implementieren und als Web-App hosten. Dadurch erhalten Sie einen Webendpunkt, der von Ihrem Bot aufgerufen werden kann.

### <a name="data-ingestion"></a>Datenerfassung

Der Bot basiert auf Rohdaten, die erfasst und vorbereitet werden müssen. Folgende Optionen stehen Ihnen zum Orchestrieren dieses Prozesses zur Auswahl:

- **[Azure Data Factory][data-factory]**. Data Factory orchestriert und automatisiert die Datenverschiebung und -transformation.
- **[Logic Apps][logic-apps]**. Logic Apps ist eine serverlose Plattform zum Erstellen von Workflows, die Anwendungen, Daten und Dienste integrieren. Logic Apps bietet Datenconnectors für viele Anwendungen, einschließlich Office 365.
- **[Azure Functions][functions]**. Mit Azure Functions können Sie benutzerdefinierten serverlosen Code schreiben, der von einem [Trigger][functions-triggers]&mdash; aufgerufen wird (z. B. jedes Mal, wenn ein Dokument in Blob Storage oder Cosmos DB hinzugefügt wird).

### <a name="logging-and-monitoring"></a>Protokollierung und Überwachung

- **[Application Insights][app-insights]**. Mit Application Insights können Sie Anwendungsmetriken zur Überwachung und Diagnose sowie für analytische Zwecke protokollieren.
- **[Azure Blob Storage][blob]**. Blobspeicher ist für die Speicherung großer Mengen unstrukturierter Daten optimiert.
- **[Cosmos DB][cosmosdb]**. Cosmos DB eignet sich gut zum Speichern von teilweise strukturierten Daten wie Konversationen.
- **[Power BI][power-bi]**. Verwenden Sie Power BI, um Überwachungsdashboards für Ihren Bot zu erstellen.

### <a name="security-and-governance"></a>Sicherheit und Governance

- **[Azure Active Directory][aad]** (Azure AD). Benutzer werden über einen Identitätsanbieter wie Azure AD authentifiziert. Der Bot Service ist für den Authentifizierungsflow und die Verwaltung der OAuth-Token zuständig. Siehe [Hinzufügen von Authentifizierung zu Ihrem Bot über Azure Bot Service][bot-authentication].
- **[Azure Key Vault][key-vault]**. Speichern Sie Anmeldeinformationen und andere Geheimnisse mithilfe von Key Vault.

### <a name="quality-assurance-and-enhancements"></a>Qualitätssicherung und Verbesserungen

- **[Azure DevOps][devops]** bietet viele Dienste für die Verwaltung von Apps, darunter Quellcodeverwaltung, Erstellung, Tests, Bereitstellung und Projektnachverfolgung.
- **[VS Code][vscode]** ist ein reduzierter Code-Editor für die App-Entwicklung. Sie können eine beliebige andere integrierte Entwicklungsumgebung (Integrated Development Environment, IDE) mit ähnlichen Features verwenden.

## <a name="design-considerations"></a>Überlegungen zum Entwurf

Allgemein betrachtet lässt sich ein interaktiver Bot in die Botfunktionalität (das „Gehirn“) und eine Gruppe von umgebenden Anforderungen (den „Körper“) unterteilen. Die Botfunktionalität umfasst die domänenfähigen Komponenten, einschließlich der Botlogik und ML-Funktionen (Machine Learning). Andere Komponenten sind domänenagnostisch und dienen für nicht funktionsbezogenen Anforderungen, z. B. CI/CD, Qualitätssicherung und Sicherheit.

![Logisches Diagramm der Botfunktionalität](./_images/conversational-bot-logical.png)

Bevor wir uns mit den Details dieser Architektur befassen, werfen wir einen Blick auf den Datenfluss durch die einzelnen Unterkomponenten des Entwurfs. Der Datenfluss umfasst vom Benutzer initiierte und vom System initiierte Datenflüsse.

### <a name="user-message-flow"></a>Benutzernachrichtenfluss

**Authentifizierung**. Benutzer authentifizieren sich zunächst anhand des von ihrem Kommunikationskanal bereitgestellten Mechanismus beim Bot. Bot Framework unterstützt viele Kommunikationskanäle, einschließlich Cortana, Microsoft Teams, Facebook Messenger, Kik und Slack. Eine Liste der Kanäle finden Sie unter [Verbinden eines Bots mit Kanälen](/azure/bot-service/bot-service-manage-channels). Wenn Sie einen Bot mit Azure Bot Service erstellen, wird der [Webchatkanal][webchat] automatisch konfiguriert. Über diesen Kanal können Benutzer direkt auf einer Webseite mit Ihrem Bot interagieren. Sie können den Bot mithilfe des [Direct Line](/azure/bot-service/bot-service-channel-connect-directline)-Kanals auch mit einer benutzerdefinierten App verbinden. Die Identität des Benutzers wird verwendet, um die rollenbasierte Zugriffssteuerung und personalisierte Inhalte bereitzustellen.

**Benutzernachricht**. Nach der Authentifizierung sendet der Benutzer eine Nachricht an den Bot. Der Bot liest die Nachricht und leitet sie an einen Dienst weiter, der natürliche Sprache versteht (beispielsweise [LUIS](/azure/cognitive-services/luis/)). In diesem Schritt werden die **Absichten** (was der Benutzer tun möchte) und **Entitäten** (die Dinge, für die sich der Benutzer interessiert) ermittelt. Der Bot erstellt dann eine Abfrage und übergibt sie an einen Dienst, der Informationen bereitstellt, beispielsweise [Azure Search][search] für den Dokumentabruf, [QnA Maker](https://www.qnamaker.ai/) für häufig gestellte Fragen oder eine benutzerdefinierte Wissensdatenbank. Anhand dieser Ergebnisse erstellt der Bot eine Antwort. Um das bestmögliche Ergebnis für eine bestimmte Abfrage bereitzustellen, führt der Bot möglicherweise mehrere Aufrufe zwischen diesen Remotediensten aus.

**Antwort**. Der Bot hat die beste Antwort bestimmt und sendet sie nun an den Benutzer. Wenn die Zuverlässigkeitsbewertung der besten Antwort niedrig ist, ist die Antwort u. U. eine Frage zur Klärung von Mehrdeutigkeiten oder eine Bestätigung, dass der Bot nicht angemessen antworten konnte.

**Protokollierung**: Wenn eine Benutzeranforderung empfangen oder eine Antwort gesendet wird, sollten alle Konversationsaktionen zusammen mit Leistungsmetriken und allgemeinen Fehlern von externen Diensten in einem Protokollspeicher protokolliert werden. Diese Protokolle sind später hilfreich beim Diagnostizieren von Problemen und Verbessern des Systems.

**Feedback**. Eine weitere bewährte Methode ist das Erfassen von Feedback und Zufriedenheitsbewertungen der Benutzer. Als Folgeaktion sollte der Bot den Benutzer nach dem Senden der endgültigen Antwort auffordern, seine Zufriedenheit mit der Antwort zu bewerten. Feedback kann Ihnen dabei helfen, das Kaltstartproblem beim Verstehen natürlicher Sprache zu beheben und die Genauigkeit von Antworten kontinuierlich zu verbessern.

### <a name="system-data-flow"></a>Systemdatenfluss

**ETL (Extrahieren, Transformieren und Laden)**. Die Informationen und das Wissen, auf denen der Bot beruht, werden von einem ETL-Prozess im Back-End aus den Rohdaten extrahiert. Diese Daten können strukturiert (SQL-Datenbank), teilweise strukturiert (CRM-System, häufig gestellte Fragen) oder unstrukturiert (Word-Dokumente, PDF-Dateien, Webprotokolle) sein. Ein ETL-Subsystem extrahiert die Daten nach einem festen Zeitplan. Der Inhalt wird transformiert und angereichert und anschließend in einen Zwischendatenspeicher wie Cosmos DB oder Azure Blob Storage geladen.

Daten im Zwischenspeicher werden dann für den Dokumentabruf in Azure Search indiziert, in QnA Maker geladen, um Frage- und Antwort-Paare zu erstellen, oder in eine benutzerdefinierte Web-App zur Verarbeitung von unstrukturiertem Text geladen. Die Daten werden auch verwendet, um ein LUIS-Modell zum Extrahieren von Absichten und Entitäten zu trainieren.

**Qualitätssicherung**. Die Konversationsprotokolle werden zum Diagnostizieren und Beheben von Fehlern verwendet, liefern Erkenntnisse zur Nutzung des Bots und dienen zur Nachverfolgung der Gesamtleistung. Feedbackdaten können für das erneute Trainieren der KI-Modelle zum Verbessern der Leistung des Bots hilfreich sein.

## <a name="building-a-bot"></a>Erstellen eines Bots

Bevor Sie auch nur eine einzige Zeile Code schreiben, sollten Sie unbedingt eine Funktionsspezifikation verfassen, damit das Entwicklungsteam eine klare Vorstellung davon hat, welche Funktionen vom Bot erwartet werden. Die Spezifikation sollte eine angemessen umfassende Liste von Benutzereingaben und erwarteten Botantworten in verschiedenen Wissensgebieten enthalten. Dieses dynamische Dokument ist ein wertvoller Leitfaden für das Entwickeln und Testen Ihres Bots.

### <a name="ingest-data"></a>Erfassen von Daten

Identifizieren Sie anschließend die Datenquellen, die dem Bot die intelligente Interaktion mit Benutzern ermöglichen. Wie bereits erwähnt, können diese Datenquellen strukturierte, teilweise strukturierte oder unstrukturierte Datasets enthalten. Es empfiehlt sich, zu Beginn eine einmalige Kopie der Daten in einem zentralen Speicher (z. B. Cosmos DB oder Azure Storage) zu erstellen. Wenn die Erstellung Ihres Bots voranschreitet, sollten Sie eine automatisierte Datenerfassungspipeline erstellen, damit diese Daten stets aktuell sind. Zu den verfügbaren Optionen für eine automatisierte Erfassungspipeline zählen Data Factory, Functions und Logic Apps. Je nach Datenspeichern und Schemas können Sie eine Kombination dieser Ansätze nutzen.

Bei den ersten Schritten ist es sinnvoll, Azure-Ressourcen manuell im Azure-Portal zu erstellen. Später sollten Sie erwägen, die Bereitstellung dieser Ressourcen zu automatisieren.

### <a name="core-bot-logic-and-ux"></a>Kernlogik und Benutzererfahrung (User Experience, UX) des Bots

Sobald Sie eine Spezifikation erstellt haben und über einige Daten verfügen, ist es an der Zeit, mit der Realisierung Ihres Bots zu beginnen. Konzentrieren wir uns auf die Kernlogik des Bots. Hierbei handelt es sich um den Code, der die Konversation mit dem Benutzer verwaltet, einschließlich der Routinglogik, der Mehrdeutigkeitsvermeidungslogik und Protokollierung. Machen Sie sich zunächst mit [Bot Framework][bot-framework] und Folgendem vertraut:

- Grundlegende Konzepte und Terminologie des Frameworks, insbesondere [Konversationen], [Turns] und [Aktivitäten]
- Dem [Bot-Connectordienst](/azure/bot-service/rest-api/bot-framework-rest-connector-quickstart), der die Netzwerkverbindung zwischen dem Bot und Ihren Kanälen verwaltet
- Der Beibehaltung des [Konversationszustands](/azure/bot-service/bot-builder-concept-state) im Arbeitsspeicher oder besser noch in einem Speicher wie Azure Blob Storage oder Azure Cosmos DB
- [Middleware](/azure/bot-service/bot-builder-basics#middleware) und deren Verwendung zum Verbinden Ihres Bots mit externen Diensten wie Cognitive Services

Zum Bereitstellen einer umfassenden [Benutzererfahrung](/azure/bot-service/bot-service-design-user-experience) stehen zahlreiche Optionen zur Verfügung.

- Sie können [Karten](/azure/bot-service/bot-service-design-user-experience#cards) verwenden, um Schaltflächen, Bilder, Karusselle und Menüs einzubinden.
- Ein Bot kann die Spracherkennung unterstützen.
- Sie können Ihren Bot sogar in eine App oder Website einbetten und die Funktionen der hostenden App nutzen.

Zum Einstieg können Sie Ihren Bot online mit dem [Azure Bot Service](/azure/bot-service/bot-service-quickstart) anhand der verfügbaren C#- und Node.js-Vorlagen erstellen. Wenn Ihr Bot komplexer wird, müssen Sie ihn jedoch lokal erstellen und anschließend im Web bereitstellen. Wählen Sie eine IDE wie Visual Studio oder Visual Studio Code und eine Programmiersprache aus. SDKs sind für folgende Programmiersprachen verfügbar:

- [C#](https://github.com/microsoft/botbuilder-dotnet)
- [JavaScript](https://github.com/microsoft/botbuilder-js)
- [Java](https://github.com/microsoft/botbuilder-java) (Vorschauversion)
- [Python](https://github.com/microsoft/botbuilder-python) (Vorschauversion)

Als Startpunkt können Sie den Quellcode für den mit dem Azure Bot Service erstellten Bot herunterladen. Es ist auch [Beispielcode](https://github.com/Microsoft/BotBuilder-Samples/blob/master/README.md) für verschiedene Bottypen verfügbar, von einfachen Echobots bis hin zu komplexeren, in verschiedene KI-Dienste integrierten Bots.

### <a name="add-smarts-to-your-bot"></a>Hinzufügen von intelligenten Funktionen zum Bot

Bei einem einfachen Bot mit einer klar definierten Liste von Befehlen können Sie möglicherweise einen regelbasierten Ansatz verwenden, um die Benutzereingabe über RegEx zu analysieren. Dieser Ansatz hat den Vorteil, dass er deterministisch und verständlich ist. Wenn Ihr Bot jedoch die Absichten und Entitäten in einer Nachricht in natürlicherer Sprache verstehen muss, können Sie auf KI-Dienste zurückgreifen.

- LUIS wurde eigens dafür entwickelt, Absichten des Benutzers und Entitäten zu verstehen. LUIS wird mit einer mittelgroßen Sammlung von relevanten [Benutzereingaben](/azure/cognitive-services/luis/luis-concept-utterance) und gewünschten Antworten trainiert und gibt die Absichten und Entitäten für eine bestimmte Benutzernachricht zurück.

- Azure Search kann zusammen mit LUIS eingesetzt werden. Mithilfe von Search erstellen Sie durchsuchbare Indizes für alle relevanten Daten. Der Bot fragt diese Indizes für die von LUIS extrahierten Entitäten ab. Azure Search unterstützt auch [Synonyme][synonyms] und bietet dadurch die Möglichkeit, die Auswahl an korrekten Wortzuordnungen zu erweitern.

- QnA Maker ist ein weiterer Dienst, der Antworten für bestimmte Fragen zurückgibt. Der Dienst wird in der Regel mit teilweise strukturierten Daten wie häufig gestellten Fragen trainiert.

Ihr Bot kann auch andere KI-Dienste nutzen, um die Benutzererfahrung zu verbessern. Die [Cognitive Services-Suite vorgefertigter KI](https://azure.microsoft.com/en-us/services/cognitive-services/?v=18.44a)-Dienste (beinhaltet LUIS und QnA Maker) umfasst Dienste für Bildanalyse, Spracherkennung, Suche und Standorterkennung. Sie können schnell Funktionen wie Sprachübersetzung, Rechtschreibprüfung, Stimmungsanalyse, optische Zeichenerkennung (Optical Character Recognition, OCR), Standorterkennung und Inhaltsmoderation hinzufügen. Diese Dienste können als Middlewaremodule in Ihrem Bot eingerichtet werden, um auf natürlichere und intelligentere Weise mit dem Benutzer zu interagieren.

Eine weitere Option ist die Integration eines eigenen benutzerdefinierten KI-Diensts. Dieser Ansatz ist komplexer, bietet Ihnen jedoch vollständige Flexibilität in Bezug auf den Machine Learning-Algorithmus, das Training und das Modell. Sie können beispielsweise Ihre eigene Themenmodellierung implementieren und einen Algorithmus wie [LDA][lda] verwenden, um nach ähnlichen oder relevanten Dokumenten zu suchen. Ein guter Ansatz ist es, Ihre benutzerdefinierte KI-Lösung als Webdienstendpunkt verfügbar zu machen und den Endpunkt über die Kernlogik des Bots aufzurufen. Der Webdienst kann in App Service oder in einem VM-Cluster gehostet werden. [Azure Machine Learning][aml] bietet eine Reihe von Diensten und Bibliotheken, die Sie beim [Trainieren](https://github.com/Azure/MachineLearningNotebooks/tree/master/how-to-use-azureml/training) und [Bereitstellen](https://github.com/Azure/MachineLearningNotebooks/tree/master/how-to-use-azureml/deployment) Ihrer Modelle unterstützen.

## <a name="quality-assurance-and-enhancement"></a>Qualitätssicherung und Verbesserung

**Protokollierung**: Protokollieren Sie Benutzerkonversationen mit dem Bot sowie die zugrunde liegenden Leistungsmetriken und etwaige Fehler. Diese Protokolle liefern wertvolle Informationen für das Debuggen von Problemen, Verstehen von Benutzerinteraktionen und Verbessern des Systems. Für die unterschiedlichen Protokolltypen können verschiedene Datenspeicher zweckmäßig sein. Erwägen Sie beispielsweise die Verwendung von Application Insights für Webprotokolle, Cosmos DB für Konversationen und Azure Storage für große Nutzlasten. Weitere Informationen finden Sie unter [Direktes Schreiben in Azure Storage][transcript-storage].

**Feedback**. Zu wissen, wie zufrieden Benutzer mit ihren Botinteraktionen sind, ist ebenfalls wichtig. Wenn Sie einen Datensatz mit Benutzerfeedback haben, können Sie Ihre Aktivitäten mithilfe dieser Daten auf die Verbesserung bestimmter Interaktionen ausrichten und die KI-Modelle zum Verbessern der Leistung erneut trainieren. Verwenden Sie das Feedback zum erneuten Trainieren der Modelle (z. B. LUIS) in Ihrem System.

**Testen**. Das Testen eines Bots umfasst Komponententests, Integrationstests, Regressionstests und Funktionstests. Wir empfehlen, zum Testen echte HTTP-Antworten von externen Diensten wie Azure Search oder QnA Maker aufzuzeichnen. Diese Antworten können dann bei Komponententests wiedergegeben werden, ohne tatsächliche Netzwerkaufrufe an externe Dienste durchzuführen.

>[!NOTE]
> Sehen Sie sich die [Botbuilder-Hilfsprogramme für JavaScript][git-repo-base] an, die Ihnen bei der Entwicklung in diesen Bereichen als Einstiegshilfe dienen. Das Repository enthält Hilfsprogramm-Beispielcode für Bots, die mit [Microsoft Bot Framework v4][bot-framework] erstellt werden und Node.js ausführen. Es enthält die folgenden Pakete:
>
> - [Cosmos DB-Protokollspeicher][cosmosdb-logger]: Zeigt, wie Botprotokolle in Cosmos DB gespeichert und abgefragt werden.
> - [Application Insights-Protokollspeicher][appinsights-logger]: Zeigt, wie Botprotokolle in Application Insights gespeichert und abgefragt werden.
> - [Feedback Collection Middleware][feedback-util] (Middleware zur Feedbackerfassung): Beispielmiddleware mit einem Mechanismus zum Anfordern von Feedback von Botbenutzern.
> - [Http Test Recorder][testing util] (HTTP-Testaufzeichnung): Zeichnet HTTP-Datenverkehr von Diensten auf, die außerhalb des Bots verwendet werden. Das Paket umfasst vordefinierte Unterstützung für LUIS, Azure Search und QnAMaker, es sind jedoch Erweiterungen zur Unterstützung beliebiger Dienste verfügbar. Dies dient Ihnen als Hilfe beim Automatisieren von Bot-Tests.
>
> Diese Pakete werden als Hilfsprogramm-Beispielcode ohne Garantie hinsichtlich Support oder Updates bereitgestellt.

## <a name="availability-considerations"></a>Überlegungen zur Verfügbarkeit

Wenn Sie neue Features oder Fehlerbehebungen für Ihren Bot einführen, empfiehlt sich die Verwendung mehrerer Bereitstellungsumgebungen (z. B. Staging und Produktion). Mithilfe der [Bereitstellungsslots][slots] von [Azure DevOps][devops] ist dies ohne Downtime möglich. Sie können Ihre neuesten Aktualisierungen in der Stagingumgebung testen, bevor Sie sie in der Produktionsumgebung bereitstellen. In Bezug auf die Verarbeitung der Last ist App Service für manuelles oder automatisches zentrales Hochskalieren konzipiert. Da Ihr Bot in der globalen Rechenzentrumsinfrastruktur von Microsoft gehostet wird, garantiert die App Service-SLA Hochverfügbarkeit.

## <a name="security-considerations"></a>Sicherheitshinweise

Wie jede andere Anwendung kann der Bot sensible Daten verarbeiten. Daher müssen die Benutzer, die sich anmelden und den Bot nutzen können, eingeschränkt werden. Schränken Sie auch die Daten, auf die zugegriffen werden kann, basierend auf der Identität oder Rolle des Benutzers ein. Verwenden Sie Azure AD für die Identitäts- und Zugriffsteuerung und Key Vault zum Verwalten von Schlüsseln und Geheimnissen.

## <a name="manageability-considerations"></a>Überlegungen zur Verwaltbarkeit

### <a name="monitoring-and-reporting"></a>Überwachung und Berichterstellung

Wenn Ihr Bot in der Produktionsumgebung ausgeführt wird, benötigen Sie ein DevOps-Team, das für eine gleichbleibend hohe Leistung und einen unterbrechungsfreien Betrieb des Bots sorgt. Überwachen Sie das System lückenlos, um sicherzustellen, dass der Bot mit optimaler Leistung arbeitet. Verwenden Sie die an Application Insights oder Cosmos DB gesendeten Protokolle, um mithilfe von Application Insights selbst oder mit Power BI Überwachungsdashboards oder ein benutzerdefiniertes Web-App-Dashboard zu erstellen. Senden Sie Benachrichtigungen an das DevOps-Team, wenn schwerwiegende Fehler auftreten oder die Leistung unter einen akzeptablen Schwellenwert fällt.

### <a name="automated-resource-deployment"></a>Automatisierte Bereitstellung von Ressourcen

Der Bot selbst ist nur ein Teil eines größeren Systems, das den Bot mit den neuesten Daten versorgt und seinen ordnungsgemäßen Betrieb sicherstellt. All diese anderen Azure-Ressourcen &mdash; Datenorchestrierungsdienste wie Data Factory, Speicherdienste wie Cosmos DB usw. &mdash; müssen bereitgestellt werden. Azure Resource Manager bietet eine einheitliche Verwaltungsebene, auf die Sie über das Azure-Portal, PowerShell oder die Azure-Befehlszeilenschnittstelle (Azure CLI) zugreifen können. Aus Gründen der Geschwindigkeit und Konsistenz empfiehlt es sich, die Bereitstellung mit einem dieser Ansätze zu automatisieren.

### <a name="continuous-bot-deployment"></a>Kontinuierliche Botbereitstellung

Sie können die Botlogik direkt über die IDE oder über eine Befehlszeile wie die Azure CLI bereitstellen. Bei fortschreitender Entwicklung Ihres Bots empfiehlt es sich jedoch, einen kontinuierlichen Bereitstellungsprozess mit einer CI/CD-Lösung wie Azure DevOps zu verwenden. Informationen dazu finden Sie im Artikel zum [Einrichten von Continuous Deployment](/azure/bot-service/bot-service-build-continuous-deployment). Auf diese Weise können Sie das Testen neuer Features und Fehlerbehebungen in Ihrem Bot in einer produktionsnahen Umgebung vereinfachen. Es empfiehlt sich auch, mit mehreren Bereitstellungsumgebungen zu arbeiten (in der Regel mindestens eine Stagingumgebung und eine Produktionsumgebung). Azure DevOps unterstützt diesen Ansatz.

<!-- links -->

[0]: ./_images/conversational-bot.png
[aad]: /azure/active-directory/
[Aktivitäten]: /azure/bot-service/rest-api/bot-framework-rest-connector-activities
[aml]: /azure/machine-learning/service/
[app-insights]: /azure/azure-monitor/app/app-insights-overview
[app-service]: /azure/app-service/
[blob]: /azure/storage/blobs/storage-blobs-introduction
[bot-authentication]: /azure/bot-service/bot-builder-authentication
[bot-framework]: https://dev.botframework.com/
[bot-framework-service]: /azure/bot-service/bot-builder-basics
[cognitive-services]: /azure/cognitive-services/welcome
[Konversationen]: /azure/bot-service/bot-service-design-conversation-flow
[cosmosdb]: /azure/cosmos-db/
[data-factory]: /azure/data-factory/
[data-factory-ref-arch]: ../data/enterprise-bi-adf.md
[devops]: https://azure.microsoft.com/solutions/devops/
[functions]: /azure/azure-functions/
[functions-triggers]: /azure/azure-functions/functions-triggers-bindings
[git-repo-appinsights-logger]: https://github.com/Microsoft/botbuilder-utils-js/tree/master/packages/botbuilder-transcript-app-insights
[git-repo-base]: https://github.com/Microsoft/botbuilder-utils-js
[git-repo-cosmosdb-logger]: https://github.com/Microsoft/botbuilder-utils-js/tree/master/packages/botbuilder-transcript-cosmosdb
[git-repo-feedback-util]: https://github.com/Microsoft/botbuilder-utils-js/tree/master/packages/botbuilder-feedback
[git-repo-testing-util]: https://github.com/Microsoft/botbuilder-utils-js/tree/master/packages/botbuilder-http-test-recorder
[testing-util]: https://github.com/Microsoft/botbuilder-utils-js/tree/master/packages/botbuilder-http-test-recorder
[key-vault]: /azure/key-vault/
[lda]: https://wikipedia.org/wiki/Latent_Dirichlet_allocation/
[logic-apps]: /azure/logic-apps/logic-apps-overview
[luis]: /azure/cognitive-services/luis/
[power-bi]: /power-bi/
[qna-maker]: /azure/cognitive-services/QnAMaker/
[search]: /azure/search/
[slots]: /azure/app-service/deploy-staging-slots/
[synonyms]: /azure/search/search-synonyms
[transcript-storage]: /azure/bot-service/bot-builder-howto-v4-storage
[Turns]: /azure/bot-service/bot-builder-basics#defining-a-turn
[vscode]: https://azure.microsoft.com/products/visual-studio-code/
[webapp]: /azure/app-service/overview
[webchat]: /azure/bot-service/bot-service-channel-connect-webchat?view=azure-bot-service-4.0/

[cosmosdb-logger]: https://github.com/Microsoft/botbuilder-utils-js/tree/master/packages/botbuilder-transcript-cosmosdb
[appinsights-logger]: https://github.com/Microsoft/botbuilder-utils-js/tree/master/packages/botbuilder-transcript-app-insights
[feedback-util]: https://github.com/Microsoft/botbuilder-utils-js/tree/master/packages/botbuilder-feedback
[testing util]: https://github.com/Microsoft/botbuilder-utils-js/tree/master/packages/botbuilder-http-test-recorder

