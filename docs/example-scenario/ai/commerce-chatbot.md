---
title: Interaktiver Chatbot für Hotelreservierungen
titleSuffix: Azure Example Scenarios
description: Erstellen Sie einen interaktiven Chatbot für gewerbliche Anwendungen mit Azure Bot Service.
author: iainfoulds
ms.date: 07/05/2018
ms.topic: example-scenario
ms.service: architecture-center
ms.subservice: example-scenario
social_image_url: /azure/architecture/example-scenario/ai/media/architecture-commerce-chatbot.png
ms.openlocfilehash: c4859cb0e43603991e4f8e6a0311a28537f29f1a
ms.sourcegitcommit: 579c39ff4b776704ead17a006bf24cd4cdc65edd
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/17/2019
ms.locfileid: "59640259"
---
# <a name="conversational-chatbot-for-hotel-reservations-on-azure"></a><span data-ttu-id="43d14-103">Interaktiver Chatbot für Hotelreservierungen in Azure</span><span class="sxs-lookup"><span data-stu-id="43d14-103">Conversational chatbot for hotel reservations on Azure</span></span>

<span data-ttu-id="43d14-104">Dieses Beispielszenario richtet sich an Unternehmen, die einen interaktiven Chatbot in Anwendungen integrieren möchten.</span><span class="sxs-lookup"><span data-stu-id="43d14-104">This example scenario is applicable to businesses that need to integrate a conversational chatbot into applications.</span></span> <span data-ttu-id="43d14-105">In diesem Szenario wird ein C#-Chatbot für eine Hotelkette verwendet, mit dem Kunden die Verfügbarkeit prüfen und Zimmer über eine webbasierte oder mobile Anwendung buchen können.</span><span class="sxs-lookup"><span data-stu-id="43d14-105">In this scenario, a C# chatbot is used for a hotel chain that allows customers to check availability and book accommodation through a web or mobile application.</span></span>

<span data-ttu-id="43d14-106">Potenzielle Benutzer erhalten so beispielsweise die Möglichkeit, die Hotelverfügbarkeit anzuzeigen und Zimmer zu buchen, sich auf der Speisekarte eines Restaurants über Speisen zum Mitnehmen zu informieren und Essen zu bestellen oder nach Fotos zu suchen und Abzüge zu bestellen.</span><span class="sxs-lookup"><span data-stu-id="43d14-106">Potential uses include providing a way for customers to view hotel availability and book rooms, review a restaurant take-out menu and place a food order, or search for and order prints of photographs.</span></span> <span data-ttu-id="43d14-107">In der Vergangenheit mussten Unternehmen für solche Kundenanfragen üblicherweise Kundenservicemitarbeiter einstellen und schulen, und Kunden mussten warten, bis ein Mitarbeiter verfügbar war, um ihnen weiterzuhelfen.</span><span class="sxs-lookup"><span data-stu-id="43d14-107">Traditionally, businesses would need to hire and train customer service agents to respond to these customer requests, and customers would have to wait until a representative is available to provide assistance.</span></span>

<span data-ttu-id="43d14-108">Dank Azure-Diensten wie Bot Service und Language Understanding oder der Sprach-API können Unternehmen automatisierte, skalierbare Bots verwenden, um Kunden weiterzuhelfen und Bestellungen/Reservierungen zu bearbeiten.</span><span class="sxs-lookup"><span data-stu-id="43d14-108">By using Azure services such as the Bot Service and Language Understanding or Speech API services, companies can assist customers and process orders or reservations with automated, scalable bots.</span></span>

## <a name="relevant-use-cases"></a><span data-ttu-id="43d14-109">Relevante Anwendungsfälle</span><span class="sxs-lookup"><span data-stu-id="43d14-109">Relevant use cases</span></span>

<span data-ttu-id="43d14-110">Zu den weiteren relevanten Anwendungsfällen zählen:</span><span class="sxs-lookup"><span data-stu-id="43d14-110">Other relevant use cases include:</span></span>

- <span data-ttu-id="43d14-111">Anzeigen einer Restaurantspeisekarte mit Speisen zum Mitnehmen und Bestellen von Essen</span><span class="sxs-lookup"><span data-stu-id="43d14-111">Viewing a restaurant take-out menu and ordering food</span></span>
- <span data-ttu-id="43d14-112">Überprüfen der Hotelverfügbarkeit und Reservieren eines Zimmers</span><span class="sxs-lookup"><span data-stu-id="43d14-112">Checking hotel availability and reserving a room</span></span>
- <span data-ttu-id="43d14-113">Suchen nach verfügbaren Fotos und Bestellen von Abzügen</span><span class="sxs-lookup"><span data-stu-id="43d14-113">Searching available photos and ordering prints</span></span>

## <a name="architecture"></a><span data-ttu-id="43d14-114">Architecture</span><span class="sxs-lookup"><span data-stu-id="43d14-114">Architecture</span></span>

![Übersicht über die Architektur der Azure-Komponenten für einen interaktiven Chatbot][architecture]

<span data-ttu-id="43d14-116">Dieses Szenario umfasst einen interaktiven Bot, der als Concierge für ein Hotel fungiert.</span><span class="sxs-lookup"><span data-stu-id="43d14-116">This scenario covers a conversational bot that functions as a concierge for a hotel.</span></span> <span data-ttu-id="43d14-117">Die Daten durchlaufen das Szenario wie folgt:</span><span class="sxs-lookup"><span data-stu-id="43d14-117">The data flows through the scenario as follows:</span></span>

1. <span data-ttu-id="43d14-118">Der Kunde greift über eine mobile oder webbasierte App auf den Chatbot zu.</span><span class="sxs-lookup"><span data-stu-id="43d14-118">The customer accesses the chatbot with a mobile or web app.</span></span>
2. <span data-ttu-id="43d14-119">Der Benutzer wird mithilfe von Azure Active Directory B2C (Business-to-Consumer) authentifiziert.</span><span class="sxs-lookup"><span data-stu-id="43d14-119">Using Azure Active Directory B2C (Business 2 Customer), the user is authenticated.</span></span>
3. <span data-ttu-id="43d14-120">Der Benutzer interagiert mit dem Botdienst und fordert Informationen zur Hotelverfügbarkeit an.</span><span class="sxs-lookup"><span data-stu-id="43d14-120">Interacting with the Bot Service, the user requests information about hotel availability.</span></span>
4. <span data-ttu-id="43d14-121">Cognitive Services verarbeitet die Anfrage in natürlicher Sprache, um die Kommunikation mit dem Kunden zu verstehen.</span><span class="sxs-lookup"><span data-stu-id="43d14-121">Cognitive Services processes the natural language request to understand the customer communication.</span></span>
5. <span data-ttu-id="43d14-122">Wenn der Benutzer mit den Ergebnissen zufrieden ist, fügt der Bot die Reservierung des Kunden einer SQL-Datenbank hinzu bzw. aktualisiert sie.</span><span class="sxs-lookup"><span data-stu-id="43d14-122">After the user is happy with the results, the bot adds or updates the customer’s reservation in a SQL Database.</span></span>
6. <span data-ttu-id="43d14-123">Application Insights erfasst während des gesamten Prozesses Telemetriedaten der Runtime, um das DevOps-Team bei der Verbesserung der Botleistung und -nutzung zu unterstützen.</span><span class="sxs-lookup"><span data-stu-id="43d14-123">Application Insights gathers runtime telemetry throughout the process to help the DevOps team with bot performance and usage.</span></span>

### <a name="components"></a><span data-ttu-id="43d14-124">Komponenten</span><span class="sxs-lookup"><span data-stu-id="43d14-124">Components</span></span>

- <span data-ttu-id="43d14-125">[Azure Active Directory][aad-docs] ist der mehrinstanzenfähige cloudbasierte Verzeichnis- und Identitätsverwaltungsdienst von Microsoft.</span><span class="sxs-lookup"><span data-stu-id="43d14-125">[Azure Active Directory][aad-docs] is Microsoft’s multi-tenant cloud-based directory and identity management service.</span></span> <span data-ttu-id="43d14-126">Azure AD unterstützt einen B2C-Connector und somit die Identifizierung von Einzelpersonen anhand externer IDs (beispielsweise Google, Facebook oder Microsoft-Konto).</span><span class="sxs-lookup"><span data-stu-id="43d14-126">Azure AD supports a B2C connector allowing you to identify individuals using external IDs such as Google, Facebook, or a Microsoft Account.</span></span>
- <span data-ttu-id="43d14-127">[App Service][appservice-docs] ermöglicht das Erstellen und Hosten von Webanwendungen in der Programmiersprache Ihrer Wahl, ohne eine Infrastruktur verwalten zu müssen.</span><span class="sxs-lookup"><span data-stu-id="43d14-127">[App Service][appservice-docs] enables you to build and host web applications in the programming language of your choice without managing infrastructure.</span></span>
- <span data-ttu-id="43d14-128">[Bot Service][botservice-docs] bietet Tools zum Erstellen, Testen, Bereitstellen und Verwalten intelligenter Bots.</span><span class="sxs-lookup"><span data-stu-id="43d14-128">[Bot Service][botservice-docs] provides tools to build, test, deploy, and manage intelligent bots.</span></span>
- <span data-ttu-id="43d14-129">[Cognitive Services][cognitive-docs] ermöglicht die Verwendung intelligenter Algorithmen zum Sehen, Hören, Sprechen, Verstehen und Interpretieren der Wünsche von Benutzern bei Verwendung natürlicher Kommunikationsmethoden.</span><span class="sxs-lookup"><span data-stu-id="43d14-129">[Cognitive Services][cognitive-docs] lets you use intelligent algorithms to see, hear, speak, understand, and interpret your user needs through natural methods of communication.</span></span>
- <span data-ttu-id="43d14-130">[SQL-Datenbank][sqldatabase-docs] ist ein vollständig verwalteter, relationaler und mit der SQL Server-Engine kompatibler Clouddatenbankdienst.</span><span class="sxs-lookup"><span data-stu-id="43d14-130">[SQL Database][sqldatabase-docs] is a fully managed relational cloud database service that provides SQL Server engine compatibility.</span></span>
- <span data-ttu-id="43d14-131">[Application Insights][appinsights-docs] ist ein erweiterbarer APM-Dienst (Application Performance Management, Anwendungsleistungsverwaltung), mit dem Sie die Leistung von Anwendungen wie Ihrem Chatbot überwachen können.</span><span class="sxs-lookup"><span data-stu-id="43d14-131">[Application Insights][appinsights-docs] is an extensible Application Performance Management (APM) service that lets you monitor the performance of applications, such as your chatbot.</span></span>

### <a name="alternatives"></a><span data-ttu-id="43d14-132">Alternativen</span><span class="sxs-lookup"><span data-stu-id="43d14-132">Alternatives</span></span>

- <span data-ttu-id="43d14-133">Mit der [Sprach-API von Microsoft][speech-api] können Sie die Art der Botinteraktion für Kunden verändern.</span><span class="sxs-lookup"><span data-stu-id="43d14-133">[Microsoft Speech API][speech-api] can be used to change how customers interface with your bot.</span></span>
- <span data-ttu-id="43d14-134">Mit [QnA Maker][qna-maker] können Sie Ihren Bot schnell mit Informationen aus teilweise strukturierten Inhalten (etwa FAQs) versorgen.</span><span class="sxs-lookup"><span data-stu-id="43d14-134">[QnA Maker][qna-maker] can be used as to quickly add knowledge to your bot from semi-structured content like an FAQ.</span></span>
- <span data-ttu-id="43d14-135">Mit dem Dienst [Textübersetzung][translator] können Sie Ihren Bot ganz einfach mehrsprachig machen.</span><span class="sxs-lookup"><span data-stu-id="43d14-135">[Translator Text][translator] is a service that you might consider to easily add multi-lingual support to your bot.</span></span>

## <a name="considerations"></a><span data-ttu-id="43d14-136">Überlegungen</span><span class="sxs-lookup"><span data-stu-id="43d14-136">Considerations</span></span>

### <a name="availability"></a><span data-ttu-id="43d14-137">Verfügbarkeit</span><span class="sxs-lookup"><span data-stu-id="43d14-137">Availability</span></span>

<span data-ttu-id="43d14-138">In diesem Szenario werden Kundenreservierungen in Azure SQL-Datenbank gespeichert.</span><span class="sxs-lookup"><span data-stu-id="43d14-138">This scenario uses Azure SQL Database for storing customer reservations.</span></span> <span data-ttu-id="43d14-139">SQL-Datenbank umfasst zonenredundante Datenbanken, Failovergruppen und Georeplikation.</span><span class="sxs-lookup"><span data-stu-id="43d14-139">SQL Database includes zone redundant databases, failover groups, and geo-replication.</span></span> <span data-ttu-id="43d14-140">Weitere Informationen finden Sie unter [Verfügbarkeitsfunktionen][sqlavailability-docs].</span><span class="sxs-lookup"><span data-stu-id="43d14-140">For more information, see [Azure SQL Database availability capabilities][sqlavailability-docs].</span></span>

### <a name="scalability"></a><span data-ttu-id="43d14-141">Skalierbarkeit</span><span class="sxs-lookup"><span data-stu-id="43d14-141">Scalability</span></span>

<span data-ttu-id="43d14-142">Für dieses Szenario wird Azure App Service verwendet.</span><span class="sxs-lookup"><span data-stu-id="43d14-142">This scenario uses Azure App Service.</span></span> <span data-ttu-id="43d14-143">Mit App Service können Sie automatisch die Anzahl von Instanzen für die Botausführung skalieren.</span><span class="sxs-lookup"><span data-stu-id="43d14-143">With App Service, you can automatically scale the number of instances that run your bot.</span></span> <span data-ttu-id="43d14-144">Dadurch können Sie bei Ihrer Webanwendung und Ihrem Chatbot mit der Kundennachfrage Schritt halten.</span><span class="sxs-lookup"><span data-stu-id="43d14-144">This functionality lets you keep up with customer demand for your web application and chatbot.</span></span> <span data-ttu-id="43d14-145">Weitere Informationen zur automatischen Skalierung finden Sie im Azure Architecture Center unter [Automatische Skalierung][autoscaling].</span><span class="sxs-lookup"><span data-stu-id="43d14-145">For more information on autoscale, see [Autoscaling best practices][autoscaling] in the Azure Architecture Center.</span></span>

<span data-ttu-id="43d14-146">Weitere Skalierbarkeitsthemen finden Sie im Azure Architecture Center in der [Checkliste für die Skalierbarkeit][scalability].</span><span class="sxs-lookup"><span data-stu-id="43d14-146">For other scalability topics, see the [scalability checklist][scalability] in the Azure Architecture Center.</span></span>

### <a name="security"></a><span data-ttu-id="43d14-147">Sicherheit</span><span class="sxs-lookup"><span data-stu-id="43d14-147">Security</span></span>

<span data-ttu-id="43d14-148">In diesem Szenario wird Azure Active Directory B2C (Business-to-Consumer) für die Benutzerauthentifizierung verwendet.</span><span class="sxs-lookup"><span data-stu-id="43d14-148">This scenario uses Azure Active Directory B2C (Business 2 Consumer) to authenticate users.</span></span> <span data-ttu-id="43d14-149">Mit AAD B2C speichert Ihr Chatbot keine vertraulichen Kundenkonto- oder Anmeldeinformationen.</span><span class="sxs-lookup"><span data-stu-id="43d14-149">With AAD B2C, your chatbot doesn't store any sensitive customer account information or credentials.</span></span> <span data-ttu-id="43d14-150">Weitere Informationen finden Sie unter [Azure Active Directory B2C – Übersicht][aadb2c-docs].</span><span class="sxs-lookup"><span data-stu-id="43d14-150">For more information, see [Azure Active Directory B2C overview][aadb2c-docs].</span></span>

<span data-ttu-id="43d14-151">In Azure SQL-Datenbank gespeicherte Informationen werden im Ruhezustand mit Transparent Data Encryption (TDE) verschlüsselt.</span><span class="sxs-lookup"><span data-stu-id="43d14-151">Information stored in Azure SQL Database is encrypted at rest with transparent data encryption (TDE).</span></span> <span data-ttu-id="43d14-152">SQL-Datenbank bietet auch Always Encrypted, wodurch Daten bei der Abfrage und bei der Verarbeitung verschlüsselt werden.</span><span class="sxs-lookup"><span data-stu-id="43d14-152">SQL Database also offers Always Encrypted which encrypts data during querying and processing.</span></span> <span data-ttu-id="43d14-153">Weitere Informationen zur Sicherheit von SQL-Datenbank finden Sie unter [Erweiterte Sicherheit und Konformität][sqlsecurity-docs].</span><span class="sxs-lookup"><span data-stu-id="43d14-153">For more information on SQL Database security, see [Azure SQL Database security and compliance][sqlsecurity-docs].</span></span>

<span data-ttu-id="43d14-154">Allgemeine Informationen zur Entwicklung sicherer Lösungen finden Sie in der [Dokumentation zur Azure-Sicherheit][security].</span><span class="sxs-lookup"><span data-stu-id="43d14-154">For general guidance on designing secure solutions, see the [Azure Security Documentation][security].</span></span>

### <a name="resiliency"></a><span data-ttu-id="43d14-155">Resilienz</span><span class="sxs-lookup"><span data-stu-id="43d14-155">Resiliency</span></span>

<span data-ttu-id="43d14-156">In diesem Szenario werden Kundenreservierungen in Azure SQL-Datenbank gespeichert.</span><span class="sxs-lookup"><span data-stu-id="43d14-156">This scenario uses Azure SQL Database for storing customer reservations.</span></span> <span data-ttu-id="43d14-157">SQL-Datenbank umfasst zonenredundante Datenbanken, Failovergruppen, Georeplikation und automatische Sicherungen.</span><span class="sxs-lookup"><span data-stu-id="43d14-157">SQL Database includes zone redundant databases, failover groups, geo-replication, and automatic backups.</span></span> <span data-ttu-id="43d14-158">Diese Features sorgen dafür, dass Ihre Anwendung im Falle eines Wartungsereignisses oder Ausfalls weiter ausgeführt werden kann.</span><span class="sxs-lookup"><span data-stu-id="43d14-158">These features allow your application to continue running if there is a maintenance event or outage.</span></span> <span data-ttu-id="43d14-159">Weitere Informationen finden Sie unter [Verfügbarkeitsfunktionen][sqlavailability-docs].</span><span class="sxs-lookup"><span data-stu-id="43d14-159">For more information, see [Azure SQL Database availability capabilities][sqlavailability-docs].</span></span>

<span data-ttu-id="43d14-160">Die Integrität Ihrer Anwendung wird bei diesem Szenario mithilfe von Application Insights überwacht.</span><span class="sxs-lookup"><span data-stu-id="43d14-160">To monitor the health of your application, this scenario uses Application Insights.</span></span> <span data-ttu-id="43d14-161">Mit Application Insights können Sie Warnungen generieren und auf Leistungsprobleme reagieren, die die Benutzerfreundlichkeit und Verfügbarkeit des Chatbots beeinträchtigen.</span><span class="sxs-lookup"><span data-stu-id="43d14-161">With Application Insights, you can generate alerts and respond to performance issues that would impact the customer experience and availability of the chatbot.</span></span> <span data-ttu-id="43d14-162">Weitere Informationen finden Sie unter [Was ist Application Insights?][appinsights-docs].</span><span class="sxs-lookup"><span data-stu-id="43d14-162">For more information, see [What is Application Insights?][appinsights-docs]</span></span>

<span data-ttu-id="43d14-163">Andere auf Resilienz bezogene Themen finden Sie unter [Entwerfen zuverlässiger Azure-Anwendungen](../../reliability/index.md).</span><span class="sxs-lookup"><span data-stu-id="43d14-163">For other resiliency topics, see [Designing reliable Azure applications](../../reliability/index.md).</span></span>

## <a name="deploy-the-scenario"></a><span data-ttu-id="43d14-164">Bereitstellen des Szenarios</span><span class="sxs-lookup"><span data-stu-id="43d14-164">Deploy the scenario</span></span>

<span data-ttu-id="43d14-165">Dieses Szenario ist in drei Komponenten unterteilt, sodass Sie sich auf die Bereiche konzentrieren können, die für Sie besonders interessant sind:</span><span class="sxs-lookup"><span data-stu-id="43d14-165">This scenario is divided into three components for you to explore areas that you are most focused on:</span></span>

- <span data-ttu-id="43d14-166">[Infrastrukturkomponenten:](#walk-through)</span><span class="sxs-lookup"><span data-stu-id="43d14-166">[Infrastructure components](#walk-through).</span></span> <span data-ttu-id="43d14-167">Verwenden Sie eine Azure Resource Manager-Vorlage, um die Hauptkomponenten der Infrastruktur (App Service, Web-App, Application Insights, Speicherkonto, SQL Server und Datenbank) bereitzustellen.</span><span class="sxs-lookup"><span data-stu-id="43d14-167">Use an Azure Resource Manger template to deploy the core infrastructure components of an App Service, Web App, Application Insights, Storage account, and SQL Server and database.</span></span>
- <span data-ttu-id="43d14-168">[Web-App-Chatbot:](#deploy-web-app-chatbot)</span><span class="sxs-lookup"><span data-stu-id="43d14-168">[Web App Chatbot](#deploy-web-app-chatbot).</span></span> <span data-ttu-id="43d14-169">Verwenden Sie die Azure-Befehlszeilenschnittstelle, um einen Bot mit Bot Service und LUIS-App (Language Understanding and Intelligent Services) bereitzustellen.</span><span class="sxs-lookup"><span data-stu-id="43d14-169">Use the Azure CLI to deploy a bot with the Bot Service and Language Understanding and Intelligent Services (LUIS) app.</span></span>
- <span data-ttu-id="43d14-170">[C#-Chatbot-Beispielanwendung:](#deploy-chatbot-c-application-code)</span><span class="sxs-lookup"><span data-stu-id="43d14-170">[Sample C# chatbot application](#deploy-chatbot-c-application-code).</span></span> <span data-ttu-id="43d14-171">Verwenden Sie Visual Studio, um sich mit dem Code der C#-Beispielanwendung für Hotelreservierungen vertraut zu machen und ihn für einen Bot in Azure bereitzustellen.</span><span class="sxs-lookup"><span data-stu-id="43d14-171">Use Visual Studio to review the sample hotel reservation C# application code and deploy to a bot in Azure.</span></span>

### <a name="prerequisites"></a><span data-ttu-id="43d14-172">Voraussetzungen</span><span class="sxs-lookup"><span data-stu-id="43d14-172">Prerequisites</span></span>

<span data-ttu-id="43d14-173">Sie benötigen ein bestehendes Azure-Konto.</span><span class="sxs-lookup"><span data-stu-id="43d14-173">You must have an existing Azure account.</span></span> <span data-ttu-id="43d14-174">Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) erstellen, bevor Sie beginnen.</span><span class="sxs-lookup"><span data-stu-id="43d14-174">If you don't have an Azure subscription, create a [free account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) before you begin.</span></span>

### <a name="walk-through"></a><span data-ttu-id="43d14-175">Exemplarische Vorgehensweise</span><span class="sxs-lookup"><span data-stu-id="43d14-175">Walk-through</span></span>

<span data-ttu-id="43d14-176">Gehen Sie wie folgt vor, um die Infrastrukturkomponenten mit einer Resource Manager-Vorlage bereitzustellen.</span><span class="sxs-lookup"><span data-stu-id="43d14-176">To deploy the infrastructure components with a Resource Manager template, perform the following steps.</span></span>

<!-- markdownlint-disable MD033 -->

1. <span data-ttu-id="43d14-177">Klicken Sie auf die Schaltfläche **Deploy to Azure** (In Azure bereitstellen):</span><span class="sxs-lookup"><span data-stu-id="43d14-177">Click the **Deploy to Azure** button:</span></span><br><a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2Fmspnp%2Fsolution-architectures%2Fmaster%2Fapps%2Fcommerce-chatbot.json" target="_blank"><img src="https://azuredeploy.net/deploybutton.png"/></a>
2. <span data-ttu-id="43d14-178">Warten Sie, bis die Vorlagenbereitstellung im Azure-Portal geöffnet wurde, und führen Sie anschließend folgende Schritte aus:</span><span class="sxs-lookup"><span data-stu-id="43d14-178">Wait for the template deployment to open in the Azure portal, then complete the following steps:</span></span>
   - <span data-ttu-id="43d14-179">Erstellen Sie über **Neu erstellen** eine neue Ressourcengruppe, und geben Sie einen Namen (beispielsweise *myCommerceChatBotInfrastructure*) in das Textfeld ein.</span><span class="sxs-lookup"><span data-stu-id="43d14-179">Choose to **Create new** resource group, then provide a name such as *myCommerceChatBotInfrastructure* in the text box.</span></span>
   - <span data-ttu-id="43d14-180">Wählen Sie im Dropdownfeld **Standort** eine Region aus.</span><span class="sxs-lookup"><span data-stu-id="43d14-180">Select a region from the **Location** drop-down box.</span></span>
   - <span data-ttu-id="43d14-181">Geben Sie einen Benutzernamen und ein sicheres Kennwort für das SQL Server-Administratorkonto an.</span><span class="sxs-lookup"><span data-stu-id="43d14-181">Provide a username and secure password for the SQL Server administrator account.</span></span>
   - <span data-ttu-id="43d14-182">Lesen Sie die allgemeinen Geschäftsbedingungen, und aktivieren Sie dann das Kontrollkästchen **Ich stimme den oben genannten Geschäftsbedingungen zu**.</span><span class="sxs-lookup"><span data-stu-id="43d14-182">Review the terms and conditions, then check **I agree to the terms and conditions stated above**.</span></span>
   - <span data-ttu-id="43d14-183">Klicken Sie auf die Schaltfläche **Kaufen**.</span><span class="sxs-lookup"><span data-stu-id="43d14-183">Select the **Purchase** button.</span></span>

<!-- markdownlint-enable MD033 -->

<span data-ttu-id="43d14-184">Es dauert einige Minuten, bis die Bereitstellung abgeschlossen ist.</span><span class="sxs-lookup"><span data-stu-id="43d14-184">It takes a few minutes for the deployment to complete.</span></span>

### <a name="deploy-web-app-chatbot"></a><span data-ttu-id="43d14-185">Bereitstellen des Web-App-Chatbots</span><span class="sxs-lookup"><span data-stu-id="43d14-185">Deploy Web App chatbot</span></span>

<span data-ttu-id="43d14-186">Erstellen Sie den Chatbot unter Verwendung der Azure-Befehlszeilenschnittstelle.</span><span class="sxs-lookup"><span data-stu-id="43d14-186">To create the chatbot, use the Azure CLI.</span></span> <span data-ttu-id="43d14-187">Im folgenden Beispiel wird die CLI-Erweiterung für Bot Service installiert, eine Ressourcengruppe erstellt und anschließend ein Bot bereitgestellt, der Application Insights verwendet.</span><span class="sxs-lookup"><span data-stu-id="43d14-187">The following example installs the CLI extension for Bot Service, creates a resource group, then deploys a bot that uses Application Insights.</span></span> <span data-ttu-id="43d14-188">Authentifizieren Sie bei entsprechender Aufforderung Ihr Microsoft-Konto, und lassen Sie die Registrierung des Bots bei Bot Service und LUIS-App (Language Understanding and Intelligent Services) zu.</span><span class="sxs-lookup"><span data-stu-id="43d14-188">When prompted, authenticate your Microsoft account and allow the bot to register itself with the Bot Service and Language Understanding and Intelligent Services (LUIS) app.</span></span>

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

### <a name="deploy-chatbot-c-application-code"></a><span data-ttu-id="43d14-189">Bereitstellen des Codes der C#-Chatbot-Anwendung</span><span class="sxs-lookup"><span data-stu-id="43d14-189">Deploy chatbot C# application code</span></span>

<span data-ttu-id="43d14-190">Auf GitHub ist eine C#-Beispielanwendung verfügbar:</span><span class="sxs-lookup"><span data-stu-id="43d14-190">A sample C# application is available on GitHub:</span></span>

- [<span data-ttu-id="43d14-191">C#-Beispiel für einen gewerblichen Bot</span><span class="sxs-lookup"><span data-stu-id="43d14-191">Commerce Bot C# sample</span></span>](https://github.com/Microsoft/AzureBotServices-scenarios/tree/master/CSharp/Commerce/src)

<span data-ttu-id="43d14-192">Die Beispielanwendung umfasst die Komponenten für die Azure Active Directory-Authentifizierung sowie die Integration der LUIS-Komponenten (Language Understanding and Intelligent Services) von Cognitive Services.</span><span class="sxs-lookup"><span data-stu-id="43d14-192">The sample application includes the Azure Active Directory authentication components and integration with the Language Understanding and Intelligent Services (LUIS) component of Cognitive Services.</span></span> <span data-ttu-id="43d14-193">Die Anwendung benötigt Visual Studio, um das Szenario erstellen und bereitstellen zu können.</span><span class="sxs-lookup"><span data-stu-id="43d14-193">The application requires Visual Studio to build and deploy the scenario.</span></span> <span data-ttu-id="43d14-194">Weitere Informationen zum Konfigurieren von AAD B2C und der LUIS-App finden Sie in der Dokumentation des GitHub-Repositorys.</span><span class="sxs-lookup"><span data-stu-id="43d14-194">Additional information on configuring AAD B2C and the LUIS app can be found in the GitHub repo documentation.</span></span>

## <a name="pricing"></a><span data-ttu-id="43d14-195">Preise</span><span class="sxs-lookup"><span data-stu-id="43d14-195">Pricing</span></span>

<span data-ttu-id="43d14-196">Zur Ermittlung der Betriebskosten für dieses Szenario sind alle Dienste im Kostenrechner vorkonfiguriert.</span><span class="sxs-lookup"><span data-stu-id="43d14-196">To explore the cost of running this scenario, all of the services are pre-configured in the cost calculator.</span></span> <span data-ttu-id="43d14-197">Wenn Sie wissen möchten, welche Kosten für Ihren spezifischen Anwendungsfall entstehen, passen Sie die entsprechenden Variablen an Ihren voraussichtlichen Datenverkehr an.</span><span class="sxs-lookup"><span data-stu-id="43d14-197">To see how the pricing would change for your particular use case, change the appropriate variables to match your expected traffic.</span></span>

<span data-ttu-id="43d14-198">Auf der Grundlage der Anzahl von Nachrichten, die voraussichtlich von Ihrem Chatbot verarbeitet werden, haben wir drei exemplarische Kostenprofile erstellt:</span><span class="sxs-lookup"><span data-stu-id="43d14-198">We have provided three sample cost profiles based on the number of messages you expect your chatbot to process:</span></span>

- <span data-ttu-id="43d14-199">[Klein][small-pricing]: Dieses Preisbeispiel entspricht der Verarbeitung von < 10.000 Nachrichten pro Monat.</span><span class="sxs-lookup"><span data-stu-id="43d14-199">[Small][small-pricing]: this pricing example correlates to processing < 10,000 messages per month.</span></span>
- <span data-ttu-id="43d14-200">[Mittel][medium-pricing]: Dieses Preisbeispiel entspricht der Verarbeitung von < 500.000 Nachrichten pro Monat.</span><span class="sxs-lookup"><span data-stu-id="43d14-200">[Medium][medium-pricing]: this pricing example correlates to processing < 500,000 messages per month.</span></span>
- <span data-ttu-id="43d14-201">[Groß][large-pricing]: Dieses Preisbeispiel entspricht der Verarbeitung von < 10 Millionen Nachrichten pro Monat.</span><span class="sxs-lookup"><span data-stu-id="43d14-201">[Large][large-pricing]: this pricing example correlates to processing < 10 million messages per month.</span></span>

## <a name="related-resources"></a><span data-ttu-id="43d14-202">Zugehörige Ressourcen</span><span class="sxs-lookup"><span data-stu-id="43d14-202">Related resources</span></span>

<span data-ttu-id="43d14-203">Eine Reihe geführter Tutorials für Azure Bot Service finden Sie im [Abschnitt „Tutorials“][botservice-docs] der Dokumentation.</span><span class="sxs-lookup"><span data-stu-id="43d14-203">For a set of guided tutorials for the Azure Bot Service, see the [tutorial section][botservice-docs] of the documentation.</span></span>

<!-- links -->

[aadb2c-docs]: /azure/active-directory-b2c/active-directory-b2c-overview
[aad-docs]: /azure/active-directory/
[appinsights-docs]: /azure/application-insights/app-insights-overview
[appservice-docs]: /azure/app-service/
[architecture]: ./media/architecture-commerce-chatbot.png
[autoscaling]: ../../best-practices/auto-scaling.md
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