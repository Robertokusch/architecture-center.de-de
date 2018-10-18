---
title: Interaktiver Chatbot für Hotelreservierungen in Azure
description: Erstellen Sie einen interaktiven Chatbot für gewerbliche Anwendungen mit Azure Bot Service.
author: iainfoulds
ms.date: 07/05/2018
ms.openlocfilehash: 826aa36da5f30a871abd90fd8ab2b202ffdf93f0
ms.sourcegitcommit: b2a4eb132857afa70201e28d662f18458865a48e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/05/2018
ms.locfileid: "48819657"
---
# <a name="conversational-chatbot-for-hotel-reservations-on-azure"></a>Interaktiver Chatbot für Hotelreservierungen in Azure

Dieses Beispielszenario richtet sich an Unternehmen, die einen interaktiven Chatbot in Anwendungen integrieren möchten. In diesem Szenario wird ein C#-Chatbot für eine Hotelkette verwendet, mit dem Kunden die Verfügbarkeit prüfen und Zimmer über eine webbasierte oder mobile Anwendung buchen können.

Potenzielle Benutzer erhalten so beispielsweise die Möglichkeit, die Hotelverfügbarkeit anzuzeigen und Zimmer zu buchen, sich auf der Speisekarte eines Restaurants über Speisen zum Mitnehmen zu informieren und Essen zu bestellen oder nach Fotos zu suchen und Abzüge zu bestellen. In der Vergangenheit mussten Unternehmen für solche Kundenanfragen üblicherweise Kundenservicemitarbeiter einstellen und schulen, und Kunden mussten warten, bis ein Mitarbeiter verfügbar war, um ihnen weiterzuhelfen.

Dank Azure-Diensten wie Bot Service und Language Understanding oder der Sprach-API können Unternehmen automatisierte, skalierbare Bots verwenden, um Kunden weiterzuhelfen und Bestellungen/Reservierungen zu bearbeiten.

## <a name="relevant-use-cases"></a>Relevante Anwendungsfälle

Erwägen Sie dieses Szenario für folgende Anwendungsfälle:

* Anzeigen einer Restaurantspeisekarte mit Speisen zum Mitnehmen und Bestellen von Essen
* Überprüfen der Hotelverfügbarkeit und Reservieren eines Zimmers
* Suchen nach verfügbaren Fotos und Bestellen von Abzügen

## <a name="architecture"></a>Architecture

![Übersicht über die Architektur der Azure-Komponenten für einen interaktiven Chatbot][architecture]

Dieses Szenario umfasst einen interaktiven Bot, der als Concierge für ein Hotel fungiert. Die Daten durchlaufen das Szenario wie folgt:

1. Der Kunde greift über eine mobile oder webbasierte App auf den Chatbot zu.
2. Der Benutzer wird mithilfe von Azure Active Directory B2C (Business-to-Consumer) authentifiziert.
3. Der Benutzer interagiert mit dem Botdienst und fordert Informationen zur Hotelverfügbarkeit an.
4. Cognitive Services verarbeitet die Anfrage in natürlicher Sprache, um die Kommunikation mit dem Kunden zu verstehen.
5. Wenn der Benutzer mit den Ergebnissen zufrieden ist, fügt der Bot die Reservierung des Kunden einer SQL-Datenbank hinzu bzw. aktualisiert sie.
6. Application Insights erfasst während des gesamten Prozesses Telemetriedaten der Runtime, um das DevOps-Team bei der Verbesserung der Botleistung und -nutzung zu unterstützen.

### <a name="components"></a>Komponenten

* [Azure Active Directory][aad-docs] ist der mehrinstanzenfähige cloudbasierte Verzeichnis- und Identitätsverwaltungsdienst von Microsoft. Azure AD unterstützt einen B2C-Connector und somit die Identifizierung von Einzelpersonen anhand externer IDs (beispielsweise Google, Facebook oder Microsoft-Konto).
* [App Service][appservice-docs] ermöglicht das Erstellen und Hosten von Webanwendungen in der Programmiersprache Ihrer Wahl, ohne eine Infrastruktur verwalten zu müssen.
* [Bot Service][botservice-docs] bietet Tools zum Erstellen, Testen, Bereitstellen und Verwalten intelligenter Bots.
* [Cognitive Services][cognitive-docs] ermöglicht die Verwendung intelligenter Algorithmen zum Sehen, Hören, Sprechen, Verstehen und Interpretieren der Wünsche von Benutzern bei Verwendung natürlicher Kommunikationsmethoden.
* [SQL-Datenbank][sqldatabase-docs] ist ein vollständig verwalteter, relationaler und mit der SQL Server-Engine kompatibler Clouddatenbankdienst.
* [Application Insights][appinsights-docs] ist ein erweiterbarer APM-Dienst (Application Performance Management, Anwendungsleistungsverwaltung), mit dem Sie die Leistung von Anwendungen wie Ihrem Chatbot überwachen können.

### <a name="alternatives"></a>Alternativen

* Mit der [Sprach-API von Microsoft][speech-api] können Sie die Art der Botinteraktion für Kunden verändern.
* Mit [QnA Maker][qna-maker] können Sie Ihren Bot schnell mit Informationen aus teilweise strukturierten Inhalten (etwa FAQs) versorgen.
* Mit dem Dienst [Textübersetzung][translator] können Sie Ihren Bot ganz einfach mehrsprachig machen.

## <a name="considerations"></a>Überlegungen

### <a name="availability"></a>Verfügbarkeit

In diesem Szenario werden Kundenreservierungen in Azure SQL-Datenbank gespeichert. SQL-Datenbank umfasst zonenredundante Datenbanken, Failovergruppen und Georeplikation. Weitere Informationen finden Sie unter [Verfügbarkeitsfunktionen][sqlavailability-docs].

Weitere Verfügbarkeitsthemen finden Sie im Azure Architecture Center in der [Checkliste für die Verfügbarkeit][availability].

### <a name="scalability"></a>Skalierbarkeit

Für dieses Szenario wird Azure App Service verwendet. Mit App Service können Sie automatisch die Anzahl von Instanzen für die Botausführung skalieren. Dadurch können Sie bei Ihrer Webanwendung und Ihrem Chatbot mit der Kundennachfrage Schritt halten. Weitere Informationen zur automatischen Skalierung finden Sie im Azure Architecture Center unter [Automatische Skalierung][autoscaling].

Weitere Skalierbarkeitsthemen finden Sie im Azure Architecture Center in der [Checkliste für die Skalierbarkeit][scalability].

### <a name="security"></a>Sicherheit

In diesem Szenario wird Azure Active Directory B2C (Business-to-Consumer) für die Benutzerauthentifizierung verwendet. Mit AAD B2C speichert Ihr Chatbot keine vertraulichen Kundenkonto- oder Anmeldeinformationen. Weitere Informationen finden Sie unter [Azure Active Directory B2C – Übersicht][aadb2c-docs].

In Azure SQL-Datenbank gespeicherte Informationen werden im Ruhezustand mit Transparent Data Encryption (TDE) verschlüsselt. SQL-Datenbank bietet auch Always Encrypted, wodurch Daten bei der Abfrage und bei der Verarbeitung verschlüsselt werden. Weitere Informationen zur Sicherheit von SQL-Datenbank finden Sie unter [Erweiterte Sicherheit und Konformität][sqlsecurity-docs].

Allgemeine Informationen zur Entwicklung sicherer Lösungen finden Sie in der [Dokumentation zur Azure-Sicherheit][security].

### <a name="resiliency"></a>Resilienz

In diesem Szenario werden Kundenreservierungen in Azure SQL-Datenbank gespeichert. SQL-Datenbank umfasst zonenredundante Datenbanken, Failovergruppen, Georeplikation und automatische Sicherungen. Diese Features sorgen dafür, dass Ihre Anwendung im Falle eines Wartungsereignisses oder Ausfalls weiter ausgeführt werden kann. Weitere Informationen finden Sie unter [Verfügbarkeitsfunktionen][sqlavailability-docs].

Die Integrität Ihrer Anwendung wird bei diesem Szenario mithilfe von Application Insights überwacht. Mit Application Insights können Sie Warnungen generieren und auf Leistungsprobleme reagieren, die die Benutzerfreundlichkeit und Verfügbarkeit des Chatbots beeinträchtigen. Weitere Informationen finden Sie unter [Was ist Application Insights?][appinsights-docs].

Allgemeine Informationen zur Entwicklung robuster Lösungen finden Sie unter [Entwerfen robuster Anwendungen für Azure][resiliency].

## <a name="deploy-the-scenario"></a>Bereitstellen des Szenarios

Dieses Szenario ist in drei Komponenten unterteilt, sodass Sie sich auf die Bereiche konzentrieren können, die für Sie besonders interessant sind:

* [Infrastrukturkomponenten:](#deploy-infrastructure-components) Verwenden Sie eine Azure Resource Manager-Vorlage, um die Hauptkomponenten der Infrastruktur (App Service, Web-App, Application Insights, Speicherkonto, SQL Server und Datenbank) bereitzustellen.
* [Web-App-Chatbot:](#deploy-web-app-chatbot) Verwenden Sie die Azure-Befehlszeilenschnittstelle, um einen Bot mit Bot Service und LUIS-App (Language Understanding and Intelligent Services) bereitzustellen.
* [C#-Chatbot-Beispielanwendung:](#deploy-chatbot-c-application-code) Verwenden Sie Visual Studio, um sich mit dem Code der C#-Beispielanwendung für Hotelreservierungen vertraut zu machen und ihn für einen Bot in Azure bereitzustellen.

**Voraussetzungen:** Sie benötigen ein bestehendes Azure-Konto. Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) erstellen, bevor Sie beginnen.

### <a name="deploy-infrastructure-components"></a>Bereitstellen der Infrastrukturkomponenten

Gehen Sie wie folgt vor, um die Infrastrukturkomponenten mit einer Azure Resource Manager-Vorlage bereitzustellen.

1. Klicken Sie auf die Schaltfläche **Deploy to Azure** (In Azure bereitstellen):<br><a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2Fmspnp%2Fsolution-architectures%2Fmaster%2Fapps%2Fcommerce-chatbot.json" target="_blank"><img src="https://azuredeploy.net/deploybutton.png"/></a>
2. Warten Sie, bis die Vorlagenbereitstellung im Azure-Portal geöffnet wurde, und führen Sie anschließend folgende Schritte aus:
   * Erstellen Sie über **Neu erstellen** eine neue Ressourcengruppe, und geben Sie einen Namen (beispielsweise *myCommerceChatBotInfrastructure*) in das Textfeld ein.
   * Wählen Sie im Dropdownfeld **Standort** eine Region aus.
   * Geben Sie einen Benutzernamen und ein sicheres Kennwort für das SQL Server-Administratorkonto an.
   * Lesen Sie die allgemeinen Geschäftsbedingungen, und aktivieren Sie dann das Kontrollkästchen **Ich stimme den oben genannten Geschäftsbedingungen zu**.
   * Klicken Sie auf die Schaltfläche **Kaufen**.

Es dauert einige Minuten, bis die Bereitstellung abgeschlossen ist.

### <a name="deploy-web-app-chatbot"></a>Bereitstellen des Web-App-Chatbots

Erstellen Sie den Chatbot unter Verwendung der Azure-Befehlszeilenschnittstelle. Im folgenden Beispiel wird die CLI-Erweiterung für Bot Service installiert, eine Ressourcengruppe erstellt und anschließend ein Bot bereitgestellt, der Application Insights verwendet. Authentifizieren Sie bei entsprechender Aufforderung Ihr Microsoft-Konto, und lassen Sie die Registrierung des Bots bei Bot Service und LUIS-App (Language Understanding and Intelligent Services) zu.

```azurecli-interactive
# Install the Azure CLI extension for the Bot Service
az extension add --name botservice --yes

# Create a resource group
az group create --name myCommerceChatbot --location eastus

# Create a Web App Chatbot that uses Application Insights
az bot create \
    --resource-group myCommerceChatbot \
    --name commerceChatbot \
    --location eastus \
    --kind webapp \
    --sku S1 \
    --insights eastus
```

### <a name="deploy-chatbot-c-application-code"></a>Bereitstellen des Codes der C#-Chatbot-Anwendung

Auf GitHub ist eine C#-Beispielanwendung verfügbar: 

* [C#-Beispiel für einen gewerblichen Bot](https://github.com/Microsoft/AzureBotServices-scenarios/tree/master/CSharp/Commerce/src)

Die Beispielanwendung umfasst die Komponenten für die Azure Active Directory-Authentifizierung sowie die Integration der LUIS-Komponenten (Language Understanding and Intelligent Services) von Cognitive Services. Die Anwendung benötigt Visual Studio, um das Szenario erstellen und bereitstellen zu können. Weitere Informationen zum Konfigurieren von AAD B2C und der LUIS-App finden Sie in der Dokumentation des GitHub-Repositorys.

## <a name="pricing"></a>Preise

Zur Ermittlung der Betriebskosten für dieses Szenario sind alle Dienste im Kostenrechner vorkonfiguriert. Wenn Sie wissen möchten, welche Kosten für Ihren spezifischen Anwendungsfall entstehen, passen Sie die entsprechenden Variablen an Ihren voraussichtlichen Datenverkehr an.

Auf der Grundlage der Anzahl von Nachrichten, die voraussichtlich von Ihrem Chatbot verarbeitet werden, haben wir drei exemplarische Kostenprofile erstellt:

* [Klein][small-pricing]: Dieses Preisbeispiel entspricht der Verarbeitung von < 10.000 Nachrichten pro Monat.
* [Mittel][medium-pricing]: Dieses Preisbeispiel entspricht der Verarbeitung von < 500.000 Nachrichten pro Monat.
* [Groß][large-pricing]: Dieses Preisbeispiel entspricht der Verarbeitung von < 10 Millionen Nachrichten pro Monat.

## <a name="related-resources"></a>Zugehörige Ressourcen

Eine Reihe geführter Tutorials zur Nutzung von Azure Bot Service finden Sie unter dem [Tutorialknoten][botservice-docs] der Dokumentation.

<!-- links -->
[aadb2c-docs]: /azure/active-directory-b2c/active-directory-b2c-overview
[aad-docs]: /azure/active-directory/
[appinsights-docs]: /azure/application-insights/app-insights-overview
[appservice-docs]: /azure/app-service/
[architecture]: ./media/architecture-commerce-chatbot.png
[autoscaling]: ../../best-practices/auto-scaling.md
[availability]: ../../checklist/availability.md
[botservice-docs]: /azure/bot-service/
[cognitive-docs]: /azure/cognitive-services/
[resiliency]: ../../resiliency/index.md
[resource-groups]: /azure/azure-resource-manager/resource-group-overview
[security]: /azure/security/
[scalability]: ../../checklist/scalability.md
[sqlavailability-docs]: /azure/sql-database/sql-database-technical-overview#availability-capabilities
[sqldatabase-docs]: /azure/sql-database/
[sqlsecurity-docs]: /azure/sql-database/sql-database-technical-overview#advanced-security-and-compliance
[qna-maker]: /azure/cognitive-services/QnAMaker/Overview/overview
[speech-api]: /azure/cognitive-services/speech/home
[translator]: /azure/cognitive-services/translator/translator-info-overview

[small-pricing]: https://azure.com/e/dce05b6184904c50b38e1a8654f726b6
[medium-pricing]: https://azure.com/e/304d17106afc480dbc414f9726078a03
[large-pricing]: https://azure.com/e/8319dd5e5e3d4f118f9029e32a80e887