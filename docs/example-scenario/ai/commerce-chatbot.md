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
# <a name="conversational-chatbot-for-hotel-reservations-on-azure"></a><span data-ttu-id="98915-103">Interaktiver Chatbot für Hotelreservierungen in Azure</span><span class="sxs-lookup"><span data-stu-id="98915-103">Conversational chatbot for hotel reservations on Azure</span></span>

<span data-ttu-id="98915-104">Dieses Beispielszenario richtet sich an Unternehmen, die einen interaktiven Chatbot in Anwendungen integrieren möchten.</span><span class="sxs-lookup"><span data-stu-id="98915-104">This example scenario is applicable to businesses that need to integrate a conversational chatbot into applications.</span></span> <span data-ttu-id="98915-105">In diesem Szenario wird ein C#-Chatbot für eine Hotelkette verwendet, mit dem Kunden die Verfügbarkeit prüfen und Zimmer über eine webbasierte oder mobile Anwendung buchen können.</span><span class="sxs-lookup"><span data-stu-id="98915-105">In this scenario, a C# chatbot is used for a hotel chain that allows customers to check availability and book accommodation through a web or mobile application.</span></span>

<span data-ttu-id="98915-106">Potenzielle Benutzer erhalten so beispielsweise die Möglichkeit, die Hotelverfügbarkeit anzuzeigen und Zimmer zu buchen, sich auf der Speisekarte eines Restaurants über Speisen zum Mitnehmen zu informieren und Essen zu bestellen oder nach Fotos zu suchen und Abzüge zu bestellen.</span><span class="sxs-lookup"><span data-stu-id="98915-106">Potential uses include providing a way for customers to view hotel availability and book rooms, review a restaurant take-out menu and place a food order, or search for and order prints of photographs.</span></span> <span data-ttu-id="98915-107">In der Vergangenheit mussten Unternehmen für solche Kundenanfragen üblicherweise Kundenservicemitarbeiter einstellen und schulen, und Kunden mussten warten, bis ein Mitarbeiter verfügbar war, um ihnen weiterzuhelfen.</span><span class="sxs-lookup"><span data-stu-id="98915-107">Traditionally, businesses would need to hire and train customer service agents to respond to these customer requests, and customers would have to wait until a representative is available to provide assistance.</span></span>

<span data-ttu-id="98915-108">Dank Azure-Diensten wie Bot Service und Language Understanding oder der Sprach-API können Unternehmen automatisierte, skalierbare Bots verwenden, um Kunden weiterzuhelfen und Bestellungen/Reservierungen zu bearbeiten.</span><span class="sxs-lookup"><span data-stu-id="98915-108">By using Azure services such as the Bot Service and Language Understanding or Speech API services, companies can assist customers and process orders or reservations with automated, scalable bots.</span></span>

## <a name="relevant-use-cases"></a><span data-ttu-id="98915-109">Relevante Anwendungsfälle</span><span class="sxs-lookup"><span data-stu-id="98915-109">Relevant use cases</span></span>

<span data-ttu-id="98915-110">Erwägen Sie dieses Szenario für folgende Anwendungsfälle:</span><span class="sxs-lookup"><span data-stu-id="98915-110">Consider this scenario for the following use cases:</span></span>

* <span data-ttu-id="98915-111">Anzeigen einer Restaurantspeisekarte mit Speisen zum Mitnehmen und Bestellen von Essen</span><span class="sxs-lookup"><span data-stu-id="98915-111">View restaurant take-out menu and order food</span></span>
* <span data-ttu-id="98915-112">Überprüfen der Hotelverfügbarkeit und Reservieren eines Zimmers</span><span class="sxs-lookup"><span data-stu-id="98915-112">Check hotel availability and reserve a room</span></span>
* <span data-ttu-id="98915-113">Suchen nach verfügbaren Fotos und Bestellen von Abzügen</span><span class="sxs-lookup"><span data-stu-id="98915-113">Search available photos and order prints</span></span>

## <a name="architecture"></a><span data-ttu-id="98915-114">Architecture</span><span class="sxs-lookup"><span data-stu-id="98915-114">Architecture</span></span>

![Übersicht über die Architektur der Azure-Komponenten für einen interaktiven Chatbot][architecture]

<span data-ttu-id="98915-116">Dieses Szenario umfasst einen interaktiven Bot, der als Concierge für ein Hotel fungiert.</span><span class="sxs-lookup"><span data-stu-id="98915-116">This scenario covers a conversational bot that functions as a concierge for a hotel.</span></span> <span data-ttu-id="98915-117">Die Daten durchlaufen das Szenario wie folgt:</span><span class="sxs-lookup"><span data-stu-id="98915-117">The data flows through the scenario as follows:</span></span>

1. <span data-ttu-id="98915-118">Der Kunde greift über eine mobile oder webbasierte App auf den Chatbot zu.</span><span class="sxs-lookup"><span data-stu-id="98915-118">The customer accesses the chatbot with a mobile or web app.</span></span>
2. <span data-ttu-id="98915-119">Der Benutzer wird mithilfe von Azure Active Directory B2C (Business-to-Consumer) authentifiziert.</span><span class="sxs-lookup"><span data-stu-id="98915-119">Using Azure Active Directory B2C (Business 2 Customer), the user is authenticated.</span></span>
3. <span data-ttu-id="98915-120">Der Benutzer interagiert mit dem Botdienst und fordert Informationen zur Hotelverfügbarkeit an.</span><span class="sxs-lookup"><span data-stu-id="98915-120">Interacting with the Bot Service, the user requests information about hotel availability.</span></span>
4. <span data-ttu-id="98915-121">Cognitive Services verarbeitet die Anfrage in natürlicher Sprache, um die Kommunikation mit dem Kunden zu verstehen.</span><span class="sxs-lookup"><span data-stu-id="98915-121">Cognitive Services processes the natural language request to understand the customer communication.</span></span>
5. <span data-ttu-id="98915-122">Wenn der Benutzer mit den Ergebnissen zufrieden ist, fügt der Bot die Reservierung des Kunden einer SQL-Datenbank hinzu bzw. aktualisiert sie.</span><span class="sxs-lookup"><span data-stu-id="98915-122">After the user is happy with the results, the bot adds or updates the customer’s reservation in a SQL Database.</span></span>
6. <span data-ttu-id="98915-123">Application Insights erfasst während des gesamten Prozesses Telemetriedaten der Runtime, um das DevOps-Team bei der Verbesserung der Botleistung und -nutzung zu unterstützen.</span><span class="sxs-lookup"><span data-stu-id="98915-123">Application Insights gathers runtime telemetry throughout the process to help the DevOps team with bot performance and usage.</span></span>

### <a name="components"></a><span data-ttu-id="98915-124">Komponenten</span><span class="sxs-lookup"><span data-stu-id="98915-124">Components</span></span>

* <span data-ttu-id="98915-125">[Azure Active Directory][aad-docs] ist der mehrinstanzenfähige cloudbasierte Verzeichnis- und Identitätsverwaltungsdienst von Microsoft.</span><span class="sxs-lookup"><span data-stu-id="98915-125">[Azure Active Directory][aad-docs] is Microsoft’s multi-tenant cloud-based directory and identity management service.</span></span> <span data-ttu-id="98915-126">Azure AD unterstützt einen B2C-Connector und somit die Identifizierung von Einzelpersonen anhand externer IDs (beispielsweise Google, Facebook oder Microsoft-Konto).</span><span class="sxs-lookup"><span data-stu-id="98915-126">Azure AD supports a B2C connector allowing you to identify individuals using external IDs such as Google, Facebook, or a Microsoft Account.</span></span>
* <span data-ttu-id="98915-127">[App Service][appservice-docs] ermöglicht das Erstellen und Hosten von Webanwendungen in der Programmiersprache Ihrer Wahl, ohne eine Infrastruktur verwalten zu müssen.</span><span class="sxs-lookup"><span data-stu-id="98915-127">[App Service][appservice-docs] enables you to build and host web applications in the programming language of your choice without managing infrastructure.</span></span>
* <span data-ttu-id="98915-128">[Bot Service][botservice-docs] bietet Tools zum Erstellen, Testen, Bereitstellen und Verwalten intelligenter Bots.</span><span class="sxs-lookup"><span data-stu-id="98915-128">[Bot Service][botservice-docs] provides tools to build, test, deploy, and manage intelligent bots.</span></span>
* <span data-ttu-id="98915-129">[Cognitive Services][cognitive-docs] ermöglicht die Verwendung intelligenter Algorithmen zum Sehen, Hören, Sprechen, Verstehen und Interpretieren der Wünsche von Benutzern bei Verwendung natürlicher Kommunikationsmethoden.</span><span class="sxs-lookup"><span data-stu-id="98915-129">[Cognitive Services][cognitive-docs] lets you use intelligent algorithms to see, hear, speak, understand, and interpret your user needs through natural methods of communication.</span></span>
* <span data-ttu-id="98915-130">[SQL-Datenbank][sqldatabase-docs] ist ein vollständig verwalteter, relationaler und mit der SQL Server-Engine kompatibler Clouddatenbankdienst.</span><span class="sxs-lookup"><span data-stu-id="98915-130">[SQL Database][sqldatabase-docs] is a fully managed relational cloud database service that provides SQL Server engine compatibility.</span></span>
* <span data-ttu-id="98915-131">[Application Insights][appinsights-docs] ist ein erweiterbarer APM-Dienst (Application Performance Management, Anwendungsleistungsverwaltung), mit dem Sie die Leistung von Anwendungen wie Ihrem Chatbot überwachen können.</span><span class="sxs-lookup"><span data-stu-id="98915-131">[Application Insights][appinsights-docs] is an extensible Application Performance Management (APM) service that lets you monitor the performance of applications, such as your chatbot.</span></span>

### <a name="alternatives"></a><span data-ttu-id="98915-132">Alternativen</span><span class="sxs-lookup"><span data-stu-id="98915-132">Alternatives</span></span>

* <span data-ttu-id="98915-133">Mit der [Sprach-API von Microsoft][speech-api] können Sie die Art der Botinteraktion für Kunden verändern.</span><span class="sxs-lookup"><span data-stu-id="98915-133">[Microsoft Speech API][speech-api] can be used to change how customers interface with your bot.</span></span>
* <span data-ttu-id="98915-134">Mit [QnA Maker][qna-maker] können Sie Ihren Bot schnell mit Informationen aus teilweise strukturierten Inhalten (etwa FAQs) versorgen.</span><span class="sxs-lookup"><span data-stu-id="98915-134">[QnA Maker][qna-maker] can be used as to quickly add knowledge to your bot from semi-structured content like an FAQ.</span></span>
* <span data-ttu-id="98915-135">Mit dem Dienst [Textübersetzung][translator] können Sie Ihren Bot ganz einfach mehrsprachig machen.</span><span class="sxs-lookup"><span data-stu-id="98915-135">[Translator Text][translator] is a service that you might consider to easily add multi-lingual support to your bot.</span></span>

## <a name="considerations"></a><span data-ttu-id="98915-136">Überlegungen</span><span class="sxs-lookup"><span data-stu-id="98915-136">Considerations</span></span>

### <a name="availability"></a><span data-ttu-id="98915-137">Verfügbarkeit</span><span class="sxs-lookup"><span data-stu-id="98915-137">Availability</span></span>

<span data-ttu-id="98915-138">In diesem Szenario werden Kundenreservierungen in Azure SQL-Datenbank gespeichert.</span><span class="sxs-lookup"><span data-stu-id="98915-138">This scenario uses Azure SQL Database for storing customer reservations.</span></span> <span data-ttu-id="98915-139">SQL-Datenbank umfasst zonenredundante Datenbanken, Failovergruppen und Georeplikation.</span><span class="sxs-lookup"><span data-stu-id="98915-139">SQL Database includes zone redundant databases, failover groups, and geo-replication.</span></span> <span data-ttu-id="98915-140">Weitere Informationen finden Sie unter [Verfügbarkeitsfunktionen][sqlavailability-docs].</span><span class="sxs-lookup"><span data-stu-id="98915-140">For more information, see [Azure SQL Database availability capabilities][sqlavailability-docs].</span></span>

<span data-ttu-id="98915-141">Weitere Verfügbarkeitsthemen finden Sie im Azure Architecture Center in der [Checkliste für die Verfügbarkeit][availability].</span><span class="sxs-lookup"><span data-stu-id="98915-141">For other availability topics, see the [availability checklist][availability] in the Azure Architecture Center.</span></span>

### <a name="scalability"></a><span data-ttu-id="98915-142">Skalierbarkeit</span><span class="sxs-lookup"><span data-stu-id="98915-142">Scalability</span></span>

<span data-ttu-id="98915-143">Für dieses Szenario wird Azure App Service verwendet.</span><span class="sxs-lookup"><span data-stu-id="98915-143">This scenario uses Azure App Service.</span></span> <span data-ttu-id="98915-144">Mit App Service können Sie automatisch die Anzahl von Instanzen für die Botausführung skalieren.</span><span class="sxs-lookup"><span data-stu-id="98915-144">With App Service, you can automatically scale the number of instances that run your bot.</span></span> <span data-ttu-id="98915-145">Dadurch können Sie bei Ihrer Webanwendung und Ihrem Chatbot mit der Kundennachfrage Schritt halten.</span><span class="sxs-lookup"><span data-stu-id="98915-145">This functionality lets you keep up with customer demand for your web application and chatbot.</span></span> <span data-ttu-id="98915-146">Weitere Informationen zur automatischen Skalierung finden Sie im Azure Architecture Center unter [Automatische Skalierung][autoscaling].</span><span class="sxs-lookup"><span data-stu-id="98915-146">For more information on autoscale, see [Autoscaling best practices][autoscaling] in the Azure Architecture Center.</span></span>

<span data-ttu-id="98915-147">Weitere Skalierbarkeitsthemen finden Sie im Azure Architecture Center in der [Checkliste für die Skalierbarkeit][scalability].</span><span class="sxs-lookup"><span data-stu-id="98915-147">For other scalability topics, see the [scalability checklist][scalability] in the Azure Architecture Center.</span></span>

### <a name="security"></a><span data-ttu-id="98915-148">Sicherheit</span><span class="sxs-lookup"><span data-stu-id="98915-148">Security</span></span>

<span data-ttu-id="98915-149">In diesem Szenario wird Azure Active Directory B2C (Business-to-Consumer) für die Benutzerauthentifizierung verwendet.</span><span class="sxs-lookup"><span data-stu-id="98915-149">This scenario uses Azure Active Directory B2C (Business 2 Consumer) to authenticate users.</span></span> <span data-ttu-id="98915-150">Mit AAD B2C speichert Ihr Chatbot keine vertraulichen Kundenkonto- oder Anmeldeinformationen.</span><span class="sxs-lookup"><span data-stu-id="98915-150">With AAD B2C, your chatbot doesn't store any sensitive customer account information or credentials.</span></span> <span data-ttu-id="98915-151">Weitere Informationen finden Sie unter [Azure Active Directory B2C – Übersicht][aadb2c-docs].</span><span class="sxs-lookup"><span data-stu-id="98915-151">For more information, see [Azure Active Directory B2C overview][aadb2c-docs].</span></span>

<span data-ttu-id="98915-152">In Azure SQL-Datenbank gespeicherte Informationen werden im Ruhezustand mit Transparent Data Encryption (TDE) verschlüsselt.</span><span class="sxs-lookup"><span data-stu-id="98915-152">Information stored in Azure SQL Database is encrypted at rest with transparent data encryption (TDE).</span></span> <span data-ttu-id="98915-153">SQL-Datenbank bietet auch Always Encrypted, wodurch Daten bei der Abfrage und bei der Verarbeitung verschlüsselt werden.</span><span class="sxs-lookup"><span data-stu-id="98915-153">SQL Database also offers Always Encrypted which encrypts data during querying and processing.</span></span> <span data-ttu-id="98915-154">Weitere Informationen zur Sicherheit von SQL-Datenbank finden Sie unter [Erweiterte Sicherheit und Konformität][sqlsecurity-docs].</span><span class="sxs-lookup"><span data-stu-id="98915-154">For more information on SQL Database security, see [Azure SQL Database security and compliance][sqlsecurity-docs].</span></span>

<span data-ttu-id="98915-155">Allgemeine Informationen zur Entwicklung sicherer Lösungen finden Sie in der [Dokumentation zur Azure-Sicherheit][security].</span><span class="sxs-lookup"><span data-stu-id="98915-155">For general guidance on designing secure solutions, see the [Azure Security Documentation][security].</span></span>

### <a name="resiliency"></a><span data-ttu-id="98915-156">Resilienz</span><span class="sxs-lookup"><span data-stu-id="98915-156">Resiliency</span></span>

<span data-ttu-id="98915-157">In diesem Szenario werden Kundenreservierungen in Azure SQL-Datenbank gespeichert.</span><span class="sxs-lookup"><span data-stu-id="98915-157">This scenario uses Azure SQL Database for storing customer reservations.</span></span> <span data-ttu-id="98915-158">SQL-Datenbank umfasst zonenredundante Datenbanken, Failovergruppen, Georeplikation und automatische Sicherungen.</span><span class="sxs-lookup"><span data-stu-id="98915-158">SQL Database includes zone redundant databases, failover groups, geo-replication, and automatic backups.</span></span> <span data-ttu-id="98915-159">Diese Features sorgen dafür, dass Ihre Anwendung im Falle eines Wartungsereignisses oder Ausfalls weiter ausgeführt werden kann.</span><span class="sxs-lookup"><span data-stu-id="98915-159">These features allow your application to continue running if there is a maintenance event or outage.</span></span> <span data-ttu-id="98915-160">Weitere Informationen finden Sie unter [Verfügbarkeitsfunktionen][sqlavailability-docs].</span><span class="sxs-lookup"><span data-stu-id="98915-160">For more information, see [Azure SQL Database availability capabilities][sqlavailability-docs].</span></span>

<span data-ttu-id="98915-161">Die Integrität Ihrer Anwendung wird bei diesem Szenario mithilfe von Application Insights überwacht.</span><span class="sxs-lookup"><span data-stu-id="98915-161">To monitor the health of your application, this scenario uses Application Insights.</span></span> <span data-ttu-id="98915-162">Mit Application Insights können Sie Warnungen generieren und auf Leistungsprobleme reagieren, die die Benutzerfreundlichkeit und Verfügbarkeit des Chatbots beeinträchtigen.</span><span class="sxs-lookup"><span data-stu-id="98915-162">With Application Insights, you can generate alerts and respond to performance issues that would impact the customer experience and availability of the chatbot.</span></span> <span data-ttu-id="98915-163">Weitere Informationen finden Sie unter [Was ist Application Insights?][appinsights-docs].</span><span class="sxs-lookup"><span data-stu-id="98915-163">For more information, see [What is Application Insights?][appinsights-docs]</span></span>

<span data-ttu-id="98915-164">Allgemeine Informationen zur Entwicklung robuster Lösungen finden Sie unter [Entwerfen robuster Anwendungen für Azure][resiliency].</span><span class="sxs-lookup"><span data-stu-id="98915-164">For general guidance on designing resilient solutions, see [Designing resilient applications for Azure][resiliency].</span></span>

## <a name="deploy-the-scenario"></a><span data-ttu-id="98915-165">Bereitstellen des Szenarios</span><span class="sxs-lookup"><span data-stu-id="98915-165">Deploy the scenario</span></span>

<span data-ttu-id="98915-166">Dieses Szenario ist in drei Komponenten unterteilt, sodass Sie sich auf die Bereiche konzentrieren können, die für Sie besonders interessant sind:</span><span class="sxs-lookup"><span data-stu-id="98915-166">This scenario is divided into three components for you to explore areas that you are most focused on:</span></span>

* <span data-ttu-id="98915-167">[Infrastrukturkomponenten:](#deploy-infrastructure-components)</span><span class="sxs-lookup"><span data-stu-id="98915-167">[Infrastructure components](#deploy-infrastructure-components).</span></span> <span data-ttu-id="98915-168">Verwenden Sie eine Azure Resource Manager-Vorlage, um die Hauptkomponenten der Infrastruktur (App Service, Web-App, Application Insights, Speicherkonto, SQL Server und Datenbank) bereitzustellen.</span><span class="sxs-lookup"><span data-stu-id="98915-168">Use an Azure Resource Manger template to deploy the core infrastructure components of an App Service, Web App, Application Insights, Storage account, and SQL Server and database.</span></span>
* <span data-ttu-id="98915-169">[Web-App-Chatbot:](#deploy-web-app-chatbot)</span><span class="sxs-lookup"><span data-stu-id="98915-169">[Web App Chatbot](#deploy-web-app-chatbot).</span></span> <span data-ttu-id="98915-170">Verwenden Sie die Azure-Befehlszeilenschnittstelle, um einen Bot mit Bot Service und LUIS-App (Language Understanding and Intelligent Services) bereitzustellen.</span><span class="sxs-lookup"><span data-stu-id="98915-170">Use the Azure CLI to deploy a bot with the Bot Service and Language Understanding and Intelligent Services (LUIS) app.</span></span>
* <span data-ttu-id="98915-171">[C#-Chatbot-Beispielanwendung:](#deploy-chatbot-c-application-code)</span><span class="sxs-lookup"><span data-stu-id="98915-171">[Sample C# chatbot application](#deploy-chatbot-c-application-code).</span></span> <span data-ttu-id="98915-172">Verwenden Sie Visual Studio, um sich mit dem Code der C#-Beispielanwendung für Hotelreservierungen vertraut zu machen und ihn für einen Bot in Azure bereitzustellen.</span><span class="sxs-lookup"><span data-stu-id="98915-172">Use Visual Studio to review the sample hotel reservation C# application code and deploy to a bot in Azure.</span></span>

<span data-ttu-id="98915-173">**Voraussetzungen:**</span><span class="sxs-lookup"><span data-stu-id="98915-173">**Prerequisites.**</span></span> <span data-ttu-id="98915-174">Sie benötigen ein bestehendes Azure-Konto.</span><span class="sxs-lookup"><span data-stu-id="98915-174">You must have an existing Azure account.</span></span> <span data-ttu-id="98915-175">Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) erstellen, bevor Sie beginnen.</span><span class="sxs-lookup"><span data-stu-id="98915-175">If you don't have an Azure subscription, create a [free account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) before you begin.</span></span>

### <a name="deploy-infrastructure-components"></a><span data-ttu-id="98915-176">Bereitstellen der Infrastrukturkomponenten</span><span class="sxs-lookup"><span data-stu-id="98915-176">Deploy infrastructure components</span></span>

<span data-ttu-id="98915-177">Gehen Sie wie folgt vor, um die Infrastrukturkomponenten mit einer Azure Resource Manager-Vorlage bereitzustellen.</span><span class="sxs-lookup"><span data-stu-id="98915-177">To deploy the infrastructure components with an Azure Resource Manager template, perform the following steps.</span></span>

1. <span data-ttu-id="98915-178">Klicken Sie auf die Schaltfläche **Deploy to Azure** (In Azure bereitstellen):</span><span class="sxs-lookup"><span data-stu-id="98915-178">Click the **Deploy to Azure** button:</span></span><br><a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2Fmspnp%2Fsolution-architectures%2Fmaster%2Fapps%2Fcommerce-chatbot.json" target="_blank"><img src="https://azuredeploy.net/deploybutton.png"/></a>
2. <span data-ttu-id="98915-179">Warten Sie, bis die Vorlagenbereitstellung im Azure-Portal geöffnet wurde, und führen Sie anschließend folgende Schritte aus:</span><span class="sxs-lookup"><span data-stu-id="98915-179">Wait for the template deployment to open in the Azure portal, then complete the following steps:</span></span>
   * <span data-ttu-id="98915-180">Erstellen Sie über **Neu erstellen** eine neue Ressourcengruppe, und geben Sie einen Namen (beispielsweise *myCommerceChatBotInfrastructure*) in das Textfeld ein.</span><span class="sxs-lookup"><span data-stu-id="98915-180">Choose to **Create new** resource group, then provide a name such as *myCommerceChatBotInfrastructure* in the text box.</span></span>
   * <span data-ttu-id="98915-181">Wählen Sie im Dropdownfeld **Standort** eine Region aus.</span><span class="sxs-lookup"><span data-stu-id="98915-181">Select a region from the **Location** drop-down box.</span></span>
   * <span data-ttu-id="98915-182">Geben Sie einen Benutzernamen und ein sicheres Kennwort für das SQL Server-Administratorkonto an.</span><span class="sxs-lookup"><span data-stu-id="98915-182">Provide a username and secure password for the SQL Server administrator account.</span></span>
   * <span data-ttu-id="98915-183">Lesen Sie die allgemeinen Geschäftsbedingungen, und aktivieren Sie dann das Kontrollkästchen **Ich stimme den oben genannten Geschäftsbedingungen zu**.</span><span class="sxs-lookup"><span data-stu-id="98915-183">Review the terms and conditions, then check **I agree to the terms and conditions stated above**.</span></span>
   * <span data-ttu-id="98915-184">Klicken Sie auf die Schaltfläche **Kaufen**.</span><span class="sxs-lookup"><span data-stu-id="98915-184">Select the **Purchase** button.</span></span>

<span data-ttu-id="98915-185">Es dauert einige Minuten, bis die Bereitstellung abgeschlossen ist.</span><span class="sxs-lookup"><span data-stu-id="98915-185">It takes a few minutes for the deployment to complete.</span></span>

### <a name="deploy-web-app-chatbot"></a><span data-ttu-id="98915-186">Bereitstellen des Web-App-Chatbots</span><span class="sxs-lookup"><span data-stu-id="98915-186">Deploy Web App chatbot</span></span>

<span data-ttu-id="98915-187">Erstellen Sie den Chatbot unter Verwendung der Azure-Befehlszeilenschnittstelle.</span><span class="sxs-lookup"><span data-stu-id="98915-187">To create the chatbot, use the Azure CLI.</span></span> <span data-ttu-id="98915-188">Im folgenden Beispiel wird die CLI-Erweiterung für Bot Service installiert, eine Ressourcengruppe erstellt und anschließend ein Bot bereitgestellt, der Application Insights verwendet.</span><span class="sxs-lookup"><span data-stu-id="98915-188">The following example installs the CLI extension for Bot Service, creates a resource group, then deploys a bot that uses Application Insights.</span></span> <span data-ttu-id="98915-189">Authentifizieren Sie bei entsprechender Aufforderung Ihr Microsoft-Konto, und lassen Sie die Registrierung des Bots bei Bot Service und LUIS-App (Language Understanding and Intelligent Services) zu.</span><span class="sxs-lookup"><span data-stu-id="98915-189">When prompted, authenticate your Microsoft account and allow the bot to register itself with the Bot Service and Language Understanding and Intelligent Services (LUIS) app.</span></span>

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

### <a name="deploy-chatbot-c-application-code"></a><span data-ttu-id="98915-190">Bereitstellen des Codes der C#-Chatbot-Anwendung</span><span class="sxs-lookup"><span data-stu-id="98915-190">Deploy chatbot C# application code</span></span>

<span data-ttu-id="98915-191">Auf GitHub ist eine C#-Beispielanwendung verfügbar:</span><span class="sxs-lookup"><span data-stu-id="98915-191">A sample C# application is available on GitHub:</span></span> 

* [<span data-ttu-id="98915-192">C#-Beispiel für einen gewerblichen Bot</span><span class="sxs-lookup"><span data-stu-id="98915-192">Commerce Bot C# sample</span></span>](https://github.com/Microsoft/AzureBotServices-scenarios/tree/master/CSharp/Commerce/src)

<span data-ttu-id="98915-193">Die Beispielanwendung umfasst die Komponenten für die Azure Active Directory-Authentifizierung sowie die Integration der LUIS-Komponenten (Language Understanding and Intelligent Services) von Cognitive Services.</span><span class="sxs-lookup"><span data-stu-id="98915-193">The sample application includes the Azure Active Directory authentication components and integration with the Language Understanding and Intelligent Services (LUIS) component of Cognitive Services.</span></span> <span data-ttu-id="98915-194">Die Anwendung benötigt Visual Studio, um das Szenario erstellen und bereitstellen zu können.</span><span class="sxs-lookup"><span data-stu-id="98915-194">The application requires Visual Studio to build and deploy the scenario.</span></span> <span data-ttu-id="98915-195">Weitere Informationen zum Konfigurieren von AAD B2C und der LUIS-App finden Sie in der Dokumentation des GitHub-Repositorys.</span><span class="sxs-lookup"><span data-stu-id="98915-195">Additional information on configuring AAD B2C and the LUIS app can be found in the GitHub repo documentation.</span></span>

## <a name="pricing"></a><span data-ttu-id="98915-196">Preise</span><span class="sxs-lookup"><span data-stu-id="98915-196">Pricing</span></span>

<span data-ttu-id="98915-197">Zur Ermittlung der Betriebskosten für dieses Szenario sind alle Dienste im Kostenrechner vorkonfiguriert.</span><span class="sxs-lookup"><span data-stu-id="98915-197">To explore the cost of running this scenario, all of the services are pre-configured in the cost calculator.</span></span> <span data-ttu-id="98915-198">Wenn Sie wissen möchten, welche Kosten für Ihren spezifischen Anwendungsfall entstehen, passen Sie die entsprechenden Variablen an Ihren voraussichtlichen Datenverkehr an.</span><span class="sxs-lookup"><span data-stu-id="98915-198">To see how the pricing would change for your particular use case, change the appropriate variables to match your expected traffic.</span></span>

<span data-ttu-id="98915-199">Auf der Grundlage der Anzahl von Nachrichten, die voraussichtlich von Ihrem Chatbot verarbeitet werden, haben wir drei exemplarische Kostenprofile erstellt:</span><span class="sxs-lookup"><span data-stu-id="98915-199">We have provided three sample cost profiles based on the number of messages you expect your chatbot to process:</span></span>

* <span data-ttu-id="98915-200">[Klein][small-pricing]: Dieses Preisbeispiel entspricht der Verarbeitung von < 10.000 Nachrichten pro Monat.</span><span class="sxs-lookup"><span data-stu-id="98915-200">[Small][small-pricing]: this pricing example correlates to processing < 10,000 messages per month.</span></span>
* <span data-ttu-id="98915-201">[Mittel][medium-pricing]: Dieses Preisbeispiel entspricht der Verarbeitung von < 500.000 Nachrichten pro Monat.</span><span class="sxs-lookup"><span data-stu-id="98915-201">[Medium][medium-pricing]: this pricing example correlates to processing < 500,000 messages per month.</span></span>
* <span data-ttu-id="98915-202">[Groß][large-pricing]: Dieses Preisbeispiel entspricht der Verarbeitung von < 10 Millionen Nachrichten pro Monat.</span><span class="sxs-lookup"><span data-stu-id="98915-202">[Large][large-pricing]: this pricing example correlates to processing < 10 million messages per month.</span></span>

## <a name="related-resources"></a><span data-ttu-id="98915-203">Zugehörige Ressourcen</span><span class="sxs-lookup"><span data-stu-id="98915-203">Related resources</span></span>

<span data-ttu-id="98915-204">Eine Reihe geführter Tutorials zur Nutzung von Azure Bot Service finden Sie unter dem [Tutorialknoten][botservice-docs] der Dokumentation.</span><span class="sxs-lookup"><span data-stu-id="98915-204">For a set of guided tutorials on leveraging the Azure Bot Service, see the [tutorial node][botservice-docs] of the documentation.</span></span>

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