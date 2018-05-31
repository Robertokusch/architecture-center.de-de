---
title: Anleitung zu dienstspezifischen Wiederholungsmechanismen
description: Spezifische Dienstanleitung für die Festlegung des Wiederholungsmechanismus.
author: dragon119
ms.date: 07/13/2016
pnp.series.title: Best Practices
ms.openlocfilehash: 65206c5f39a74d228c7eaa0fea0c5b1b0710b22f
ms.sourcegitcommit: bb348bd3a8a4e27ef61e8eee74b54b07b65dbf98
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/21/2018
ms.locfileid: "34423016"
---
# <a name="retry-guidance-for-specific-services"></a><span data-ttu-id="bb080-103">Wiederholungsanleitung für bestimmte Dienste</span><span class="sxs-lookup"><span data-stu-id="bb080-103">Retry guidance for specific services</span></span>

<span data-ttu-id="bb080-104">Die meisten Azure-Dienste und Client-SDKs enthalten einen Wiederholungsmechanismus.</span><span class="sxs-lookup"><span data-stu-id="bb080-104">Most Azure services and client SDKs include a retry mechanism.</span></span> <span data-ttu-id="bb080-105">Diese unterscheiden sich jedoch, da jeder Dienst unterschiedliche Merkmale und Anforderungen hat und somit jeder Wiederholungsmechanismus für einen bestimmten Dienst optimiert ist.</span><span class="sxs-lookup"><span data-stu-id="bb080-105">However, these differ because each service has different characteristics and requirements, and so each retry mechanism is tuned to a specific service.</span></span> <span data-ttu-id="bb080-106">Dieses Handbuch fasst die Wiederholungsmechanismusfunktionen für die Mehrzahl der Azure-Dienste zusammen und enthält Informationen, mit denen Sie die Wiederholungsmechanismus für diesen Dienst verwenden, anpassen oder erweitern können.</span><span class="sxs-lookup"><span data-stu-id="bb080-106">This guide summarizes the retry mechanism features for the majority of Azure services, and includes information to help you use, adapt, or extend the retry mechanism for that service.</span></span>

<span data-ttu-id="bb080-107">Allgemeine Anweisungen zum Behandeln von vorübergehenden Fehlern und für Wiederholungsverbindungen und Operationen für Dienste und Ressourcen finden Sie unter [Wiederholungsanleitung](./transient-faults.md).</span><span class="sxs-lookup"><span data-stu-id="bb080-107">For general guidance on handling transient faults, and retrying connections and operations against services and resources, see [Retry guidance](./transient-faults.md).</span></span>

<span data-ttu-id="bb080-108">In der folgende Tabelle werden die Wiederholungsfunktionen für die in dieser Anleitung beschriebenen Azure-Dienste zusammengefasst.</span><span class="sxs-lookup"><span data-stu-id="bb080-108">The following table summarizes the retry features for the Azure services described in this guidance.</span></span>

| <span data-ttu-id="bb080-109">**Service**</span><span class="sxs-lookup"><span data-stu-id="bb080-109">**Service**</span></span> | <span data-ttu-id="bb080-110">**Wiederholungsfunktionen**</span><span class="sxs-lookup"><span data-stu-id="bb080-110">**Retry capabilities**</span></span> | <span data-ttu-id="bb080-111">**Richtlinienkonfiguration**</span><span class="sxs-lookup"><span data-stu-id="bb080-111">**Policy configuration**</span></span> | <span data-ttu-id="bb080-112">**Umfang**</span><span class="sxs-lookup"><span data-stu-id="bb080-112">**Scope**</span></span> | <span data-ttu-id="bb080-113">**Telemetriefunktionen**</span><span class="sxs-lookup"><span data-stu-id="bb080-113">**Telemetry features**</span></span> |
| --- | --- | --- | --- | --- |
| <span data-ttu-id="bb080-114">**[Azure Active Directory](#azure-active-directory)**</span><span class="sxs-lookup"><span data-stu-id="bb080-114">**[Azure Active Directory](#azure-active-directory)**</span></span> |<span data-ttu-id="bb080-115">Nativ in ADAL-Bibliothek</span><span class="sxs-lookup"><span data-stu-id="bb080-115">Native in ADAL library</span></span> |<span data-ttu-id="bb080-116">Eingebettet in ADAL-Bibliothek</span><span class="sxs-lookup"><span data-stu-id="bb080-116">Embeded into ADAL library</span></span> |<span data-ttu-id="bb080-117">Intern</span><span class="sxs-lookup"><span data-stu-id="bb080-117">Internal</span></span> |<span data-ttu-id="bb080-118">Keine</span><span class="sxs-lookup"><span data-stu-id="bb080-118">None</span></span> |
| <span data-ttu-id="bb080-119">**[Cosmos DB](#cosmos-db)**</span><span class="sxs-lookup"><span data-stu-id="bb080-119">**[Cosmos DB](#cosmos-db)**</span></span> |<span data-ttu-id="bb080-120">Systemeigen im Dienst</span><span class="sxs-lookup"><span data-stu-id="bb080-120">Native in service</span></span> |<span data-ttu-id="bb080-121">Nicht konfigurierbar</span><span class="sxs-lookup"><span data-stu-id="bb080-121">Non-configurable</span></span> |<span data-ttu-id="bb080-122">Global</span><span class="sxs-lookup"><span data-stu-id="bb080-122">Global</span></span> |<span data-ttu-id="bb080-123">TraceSource</span><span class="sxs-lookup"><span data-stu-id="bb080-123">TraceSource</span></span> |
| <span data-ttu-id="bb080-124">**[Event Hubs](#azure-event-hubs)**</span><span class="sxs-lookup"><span data-stu-id="bb080-124">**[Event Hubs](#azure-event-hubs)**</span></span> |<span data-ttu-id="bb080-125">Systemeigen in Client</span><span class="sxs-lookup"><span data-stu-id="bb080-125">Native in client</span></span> |<span data-ttu-id="bb080-126">Programmgesteuert</span><span class="sxs-lookup"><span data-stu-id="bb080-126">Programmatic</span></span> |<span data-ttu-id="bb080-127">Client</span><span class="sxs-lookup"><span data-stu-id="bb080-127">Client</span></span> |<span data-ttu-id="bb080-128">Keine</span><span class="sxs-lookup"><span data-stu-id="bb080-128">None</span></span> |
| <span data-ttu-id="bb080-129">**[Redis Cache](#azure-redis-cache)**</span><span class="sxs-lookup"><span data-stu-id="bb080-129">**[Redis Cache](#azure-redis-cache)**</span></span> |<span data-ttu-id="bb080-130">Systemeigen in Client</span><span class="sxs-lookup"><span data-stu-id="bb080-130">Native in client</span></span> |<span data-ttu-id="bb080-131">Programmgesteuert</span><span class="sxs-lookup"><span data-stu-id="bb080-131">Programmatic</span></span> |<span data-ttu-id="bb080-132">Client</span><span class="sxs-lookup"><span data-stu-id="bb080-132">Client</span></span> |<span data-ttu-id="bb080-133">TextWriter</span><span class="sxs-lookup"><span data-stu-id="bb080-133">TextWriter</span></span> |
| <span data-ttu-id="bb080-134">**[Search](#azure-search)**</span><span class="sxs-lookup"><span data-stu-id="bb080-134">**[Search](#azure-search)**</span></span> |<span data-ttu-id="bb080-135">Systemeigen in Client</span><span class="sxs-lookup"><span data-stu-id="bb080-135">Native in client</span></span> |<span data-ttu-id="bb080-136">Programmgesteuert</span><span class="sxs-lookup"><span data-stu-id="bb080-136">Programmatic</span></span> |<span data-ttu-id="bb080-137">Client</span><span class="sxs-lookup"><span data-stu-id="bb080-137">Client</span></span> |<span data-ttu-id="bb080-138">ETW oder benutzerdefiniert</span><span class="sxs-lookup"><span data-stu-id="bb080-138">ETW or Custom</span></span> |
| <span data-ttu-id="bb080-139">**[Service Bus](#service-bus)**</span><span class="sxs-lookup"><span data-stu-id="bb080-139">**[Service Bus](#service-bus)**</span></span> |<span data-ttu-id="bb080-140">Systemeigen in Client</span><span class="sxs-lookup"><span data-stu-id="bb080-140">Native in client</span></span> |<span data-ttu-id="bb080-141">Programmgesteuert</span><span class="sxs-lookup"><span data-stu-id="bb080-141">Programmatic</span></span> |<span data-ttu-id="bb080-142">Namespace-Manager, Messaging Factory und Client</span><span class="sxs-lookup"><span data-stu-id="bb080-142">Namespace Manager, Messaging Factory, and Client</span></span> |<span data-ttu-id="bb080-143">ETW</span><span class="sxs-lookup"><span data-stu-id="bb080-143">ETW</span></span> |
| <span data-ttu-id="bb080-144">**[Service Fabric](#service-fabric)**</span><span class="sxs-lookup"><span data-stu-id="bb080-144">**[Service Fabric](#service-fabric)**</span></span> |<span data-ttu-id="bb080-145">Systemeigen in Client</span><span class="sxs-lookup"><span data-stu-id="bb080-145">Native in client</span></span> |<span data-ttu-id="bb080-146">Programmgesteuert</span><span class="sxs-lookup"><span data-stu-id="bb080-146">Programmatic</span></span> |<span data-ttu-id="bb080-147">Client</span><span class="sxs-lookup"><span data-stu-id="bb080-147">Client</span></span> |<span data-ttu-id="bb080-148">Keine</span><span class="sxs-lookup"><span data-stu-id="bb080-148">None</span></span> | 
| <span data-ttu-id="bb080-149">**[SQL-Datenbank mit ADO.NET](#sql-database-using-adonet)**</span><span class="sxs-lookup"><span data-stu-id="bb080-149">**[SQL Database with ADO.NET](#sql-database-using-adonet)**</span></span> |[<span data-ttu-id="bb080-150">Polly</span><span class="sxs-lookup"><span data-stu-id="bb080-150">Polly</span></span>](#transient-fault-handling-with-polly) |<span data-ttu-id="bb080-151">Deklarativ und programmatisch</span><span class="sxs-lookup"><span data-stu-id="bb080-151">Declarative and programmatic</span></span> |<span data-ttu-id="bb080-152">Einzelne Anweisungen oder Codeblöcke</span><span class="sxs-lookup"><span data-stu-id="bb080-152">Single statements or blocks of code</span></span> |<span data-ttu-id="bb080-153">Benutzerdefiniert</span><span class="sxs-lookup"><span data-stu-id="bb080-153">Custom</span></span> |
| <span data-ttu-id="bb080-154">**[SQL-Datenbank mit Entity Framework](#sql-database-using-entity-framework-6)**</span><span class="sxs-lookup"><span data-stu-id="bb080-154">**[SQL Database with Entity Framework](#sql-database-using-entity-framework-6)**</span></span> |<span data-ttu-id="bb080-155">Systemeigen in Client</span><span class="sxs-lookup"><span data-stu-id="bb080-155">Native in client</span></span> |<span data-ttu-id="bb080-156">Programmgesteuert</span><span class="sxs-lookup"><span data-stu-id="bb080-156">Programmatic</span></span> |<span data-ttu-id="bb080-157">Global pro AppDomain</span><span class="sxs-lookup"><span data-stu-id="bb080-157">Global per AppDomain</span></span> |<span data-ttu-id="bb080-158">Keine</span><span class="sxs-lookup"><span data-stu-id="bb080-158">None</span></span> |
| <span data-ttu-id="bb080-159">**[SQL-Datenbank mit Entity Framework Core](#sql-database-using-entity-framework-core)**</span><span class="sxs-lookup"><span data-stu-id="bb080-159">**[SQL Database with Entity Framework Core](#sql-database-using-entity-framework-core)**</span></span> |<span data-ttu-id="bb080-160">Systemeigen in Client</span><span class="sxs-lookup"><span data-stu-id="bb080-160">Native in client</span></span> |<span data-ttu-id="bb080-161">Programmgesteuert</span><span class="sxs-lookup"><span data-stu-id="bb080-161">Programmatic</span></span> |<span data-ttu-id="bb080-162">Global pro AppDomain</span><span class="sxs-lookup"><span data-stu-id="bb080-162">Global per AppDomain</span></span> |<span data-ttu-id="bb080-163">Keine</span><span class="sxs-lookup"><span data-stu-id="bb080-163">None</span></span> |
| <span data-ttu-id="bb080-164">**[Storage](#azure-storage)**</span><span class="sxs-lookup"><span data-stu-id="bb080-164">**[Storage](#azure-storage)**</span></span> |<span data-ttu-id="bb080-165">Systemeigen in Client</span><span class="sxs-lookup"><span data-stu-id="bb080-165">Native in client</span></span> |<span data-ttu-id="bb080-166">Programmgesteuert</span><span class="sxs-lookup"><span data-stu-id="bb080-166">Programmatic</span></span> |<span data-ttu-id="bb080-167">Clientvorgänge und einzelne Vorgänge</span><span class="sxs-lookup"><span data-stu-id="bb080-167">Client and individual operations</span></span> |<span data-ttu-id="bb080-168">TraceSource</span><span class="sxs-lookup"><span data-stu-id="bb080-168">TraceSource</span></span> |

> [!NOTE]
> <span data-ttu-id="bb080-169">Für die meisten der integrierten Azure-Mechanismen gibt es derzeit keine Möglichkeit, unterschiedliche Wiederholungsrichtlinien für verschiedene Arten von Fehlern oder Ausnahmen anzuwenden.</span><span class="sxs-lookup"><span data-stu-id="bb080-169">For most of the Azure built-in retry mechanisms, there is currently no way apply a different retry policy for different types of error or exception.</span></span> <span data-ttu-id="bb080-170">Konfigurieren Sie eine Richtlinie mit optimalem Leistungs- und Verfügbarkeitsdurchschnitt.</span><span class="sxs-lookup"><span data-stu-id="bb080-170">You should configure a policy that provides the optimum average performance and availability.</span></span> <span data-ttu-id="bb080-171">Eine Möglichkeit zur Optimierung der Richtlinie ist die Analyse von Protokolldateien, um den Typ der vorübergehenden Fehler zu bestimmen, die auftreten.</span><span class="sxs-lookup"><span data-stu-id="bb080-171">One way to fine-tune the policy is to analyze log files to determine the type of transient faults that are occurring.</span></span> 

## <a name="azure-active-directory"></a><span data-ttu-id="bb080-172">Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="bb080-172">Azure Active Directory</span></span>
<span data-ttu-id="bb080-173">Azure Active Directory (Azure AD) ist eine umfassende Cloud-Lösung für die Identitäts- und Zugriffsverwaltung, die zentrale Verzeichnisdienste, eine erweiterte Identitätsgovernance, Sicherheit und Zugriffsverwaltung von Anwendungen kombiniert.</span><span class="sxs-lookup"><span data-stu-id="bb080-173">Azure Active Directory (Azure AD) is a comprehensive identity and access management cloud solution that combines core directory services, advanced identity governance, security, and application access management.</span></span> <span data-ttu-id="bb080-174">Azure AD bietet Entwicklern auf der Grundlage von zentralen Richtlinien und Regeln auch eine Plattform für die Identitätsverwaltung zur Bereitstellung einer Zugriffssteuerung für deren Anwendungen.</span><span class="sxs-lookup"><span data-stu-id="bb080-174">Azure AD also offers developers an identity management platform to deliver access control to their applications, based on centralized policy and rules.</span></span>

### <a name="retry-mechanism"></a><span data-ttu-id="bb080-175">Wiederholungsmechanismus</span><span class="sxs-lookup"><span data-stu-id="bb080-175">Retry mechanism</span></span>
<span data-ttu-id="bb080-176">In der Active Directory-Authentifizierungsbibliothek (ADAL) ist ein integrierter Wiederholungsmechanismus für Azure Active Directory enthalten.</span><span class="sxs-lookup"><span data-stu-id="bb080-176">There is a built-in retry mechanism for Azure Active Directory in the Active Directory Authentication Library (ADAL).</span></span> <span data-ttu-id="bb080-177">Zur Vermeidung von unerwarteten Sperrungen empfehlen wir, für Drittanbieterbibliotheken und -anwendungscode **keine** Wiederholungsversuche zum Herstellen von fehlgeschlagenen Verbindungen durchzuführen, sondern die Verarbeitung von Wiederholungsversuchen ADAL zu überlassen.</span><span class="sxs-lookup"><span data-stu-id="bb080-177">To avoid unexpected lockouts, we recommend that third party libraries and application code do **not** retry failed connections, but allow ADAL to handle retries.</span></span> 

### <a name="retry-usage-guidance"></a><span data-ttu-id="bb080-178">Gebrauchsanleitung Wiederholungen</span><span class="sxs-lookup"><span data-stu-id="bb080-178">Retry usage guidance</span></span>
<span data-ttu-id="bb080-179">Berücksichtigen Sie Bei Verwendung von Azure Active Directory die folgenden Richtlinien:</span><span class="sxs-lookup"><span data-stu-id="bb080-179">Consider the following guidelines when using Azure Active Directory:</span></span>

* <span data-ttu-id="bb080-180">Verwenden Sie nach Möglichkeit die ADAL-Bibliothek und die integrierte Unterstützung für Wiederholungsversuche.</span><span class="sxs-lookup"><span data-stu-id="bb080-180">When possible, use the ADAL library and the built-in support for retries.</span></span>
* <span data-ttu-id="bb080-181">Wenn Sie die REST-API für Azure Active Directory verwenden, wiederholen Sie den Vorgang, wenn der Ergebniscode 429 (Zu viele Anforderungen) lautet oder das Ergebnis ein Fehler im Bereich 5xx ist.</span><span class="sxs-lookup"><span data-stu-id="bb080-181">If you are using the REST API for Azure Active Directory, retry the operation if the result code is 429 (Too Many Requests) or an error in the 5xx range.</span></span> <span data-ttu-id="bb080-182">Bei anderen Fehlern den Vorgang nicht wiederholen.</span><span class="sxs-lookup"><span data-stu-id="bb080-182">Do not retry for any other errors.</span></span>
* <span data-ttu-id="bb080-183">Eine exponentielle Backoff-Richtlinie wird für die Verwendung in Batch-Szenarien mit Azure Active Directory empfohlen.</span><span class="sxs-lookup"><span data-stu-id="bb080-183">An exponential back-off policy is recommended for use in batch scenarios with Azure Active Directory.</span></span>

<span data-ttu-id="bb080-184">Erwägen Sie, mit den folgenden Einstellungen für Wiederholungsvorgänge zu beginnen.</span><span class="sxs-lookup"><span data-stu-id="bb080-184">Consider starting with the following settings for retrying operations.</span></span> <span data-ttu-id="bb080-185">Hierbei handelt es sich um allgemeine Einstellungen und Sie sollten die Vorgänge überwachen und die Werte entsprechend Ihrem Szenario optimieren.</span><span class="sxs-lookup"><span data-stu-id="bb080-185">These are general purpose settings, and you should monitor the operations and fine tune the values to suit your own scenario.</span></span>

| <span data-ttu-id="bb080-186">**Context**</span><span class="sxs-lookup"><span data-stu-id="bb080-186">**Context**</span></span> | <span data-ttu-id="bb080-187">**Beispiel-Ziel E2E<br />Maximale Wartezeit**</span><span class="sxs-lookup"><span data-stu-id="bb080-187">**Sample target E2E<br />max latency**</span></span> | <span data-ttu-id="bb080-188">**Wiederholungsstrategie**</span><span class="sxs-lookup"><span data-stu-id="bb080-188">**Retry strategy**</span></span> | <span data-ttu-id="bb080-189">**Einstellungen**</span><span class="sxs-lookup"><span data-stu-id="bb080-189">**Settings**</span></span> | <span data-ttu-id="bb080-190">**Werte**</span><span class="sxs-lookup"><span data-stu-id="bb080-190">**Values**</span></span> | <span data-ttu-id="bb080-191">**So funktioniert's**</span><span class="sxs-lookup"><span data-stu-id="bb080-191">**How it works**</span></span> |
| --- | --- | --- | --- | --- | --- |
| <span data-ttu-id="bb080-192">Interaktiv, Benutzeroberfläche</span><span class="sxs-lookup"><span data-stu-id="bb080-192">Interactive, UI,</span></span><br /><span data-ttu-id="bb080-193">oder Vordergrund</span><span class="sxs-lookup"><span data-stu-id="bb080-193">or foreground</span></span> |<span data-ttu-id="bb080-194">2 Sek</span><span class="sxs-lookup"><span data-stu-id="bb080-194">2 sec</span></span> |<span data-ttu-id="bb080-195">FixedInterval</span><span class="sxs-lookup"><span data-stu-id="bb080-195">FixedInterval</span></span> |<span data-ttu-id="bb080-196">Anzahl der Wiederholungen</span><span class="sxs-lookup"><span data-stu-id="bb080-196">Retry count</span></span><br /><span data-ttu-id="bb080-197">Wiederholungsintervall</span><span class="sxs-lookup"><span data-stu-id="bb080-197">Retry interval</span></span><br /><span data-ttu-id="bb080-198">Erster schneller Wiederholungsversuch</span><span class="sxs-lookup"><span data-stu-id="bb080-198">First fast retry</span></span> |<span data-ttu-id="bb080-199">3</span><span class="sxs-lookup"><span data-stu-id="bb080-199">3</span></span><br /><span data-ttu-id="bb080-200">500 ms</span><span class="sxs-lookup"><span data-stu-id="bb080-200">500 ms</span></span><br /><span data-ttu-id="bb080-201">true</span><span class="sxs-lookup"><span data-stu-id="bb080-201">true</span></span> |<span data-ttu-id="bb080-202">Versuch 1 – Verzögerung 0 Sek.</span><span class="sxs-lookup"><span data-stu-id="bb080-202">Attempt 1 - delay 0 sec</span></span><br /><span data-ttu-id="bb080-203">Versuch 2 – Verzögerung 500 ms</span><span class="sxs-lookup"><span data-stu-id="bb080-203">Attempt 2 - delay 500 ms</span></span><br /><span data-ttu-id="bb080-204">Versuch 3 – Verzögerung 500 ms</span><span class="sxs-lookup"><span data-stu-id="bb080-204">Attempt 3 - delay 500 ms</span></span> |
| <span data-ttu-id="bb080-205">Hintergrund oder </span><span class="sxs-lookup"><span data-stu-id="bb080-205">Background or</span></span><br /><span data-ttu-id="bb080-206">Batch</span><span class="sxs-lookup"><span data-stu-id="bb080-206">batch</span></span> |<span data-ttu-id="bb080-207">60 Sekunden</span><span class="sxs-lookup"><span data-stu-id="bb080-207">60 sec</span></span> |<span data-ttu-id="bb080-208">ExponentialBackoff</span><span class="sxs-lookup"><span data-stu-id="bb080-208">ExponentialBackoff</span></span> |<span data-ttu-id="bb080-209">Anzahl der Wiederholungen</span><span class="sxs-lookup"><span data-stu-id="bb080-209">Retry count</span></span><br /><span data-ttu-id="bb080-210">Min. Backoff</span><span class="sxs-lookup"><span data-stu-id="bb080-210">Min back-off</span></span><br /><span data-ttu-id="bb080-211">Max. Backoff</span><span class="sxs-lookup"><span data-stu-id="bb080-211">Max back-off</span></span><br /><span data-ttu-id="bb080-212">Delta-Backoff</span><span class="sxs-lookup"><span data-stu-id="bb080-212">Delta back-off</span></span><br /><span data-ttu-id="bb080-213">Erster schneller Wiederholungsversuch</span><span class="sxs-lookup"><span data-stu-id="bb080-213">First fast retry</span></span> |<span data-ttu-id="bb080-214">5</span><span class="sxs-lookup"><span data-stu-id="bb080-214">5</span></span><br /><span data-ttu-id="bb080-215">0 Sek.</span><span class="sxs-lookup"><span data-stu-id="bb080-215">0 sec</span></span><br /><span data-ttu-id="bb080-216">60 Sekunden</span><span class="sxs-lookup"><span data-stu-id="bb080-216">60 sec</span></span><br /><span data-ttu-id="bb080-217">2 Sek</span><span class="sxs-lookup"><span data-stu-id="bb080-217">2 sec</span></span><br /><span data-ttu-id="bb080-218">false</span><span class="sxs-lookup"><span data-stu-id="bb080-218">false</span></span> |<span data-ttu-id="bb080-219">Versuch 1 – Verzögerung 0 Sek.</span><span class="sxs-lookup"><span data-stu-id="bb080-219">Attempt 1 - delay 0 sec</span></span><br /><span data-ttu-id="bb080-220">Versuch 2 – Verzögerung ca. 2 Sek.</span><span class="sxs-lookup"><span data-stu-id="bb080-220">Attempt 2 - delay ~2 sec</span></span><br /><span data-ttu-id="bb080-221">Versuch 3 – Verzögerung ca. 6 Sek.</span><span class="sxs-lookup"><span data-stu-id="bb080-221">Attempt 3 - delay ~6 sec</span></span><br /><span data-ttu-id="bb080-222">Versuch 4 – Verzögerung ca. 14 Sek.</span><span class="sxs-lookup"><span data-stu-id="bb080-222">Attempt 4 - delay ~14 sec</span></span><br /><span data-ttu-id="bb080-223">Versuch 5 – Verzögerung ca. 30 Sek.</span><span class="sxs-lookup"><span data-stu-id="bb080-223">Attempt 5 - delay ~30 sec</span></span> |

### <a name="more-information"></a><span data-ttu-id="bb080-224">Weitere Informationen</span><span class="sxs-lookup"><span data-stu-id="bb080-224">More information</span></span>
* <span data-ttu-id="bb080-225">[Azure Active Directory-Authentifizierungsbibliotheken][adal]</span><span class="sxs-lookup"><span data-stu-id="bb080-225">[Azure Active Directory Authentication Libraries][adal]</span></span>

## <a name="cosmos-db"></a><span data-ttu-id="bb080-226">Cosmos DB</span><span class="sxs-lookup"><span data-stu-id="bb080-226">Cosmos DB</span></span>

<span data-ttu-id="bb080-227">Cosmos DB ist eine vollständig verwaltete Datenbank mit mehreren Modellen, die schemalose JSON-Daten unterstützt.</span><span class="sxs-lookup"><span data-stu-id="bb080-227">Cosmos DB is a fully-managed multi-model database that supports schema-less JSON data.</span></span> <span data-ttu-id="bb080-228">Sie bietet konfigurierbare und zuverlässige Leistung, systemeigene JavaScript-Transaktionsverarbeitung und ist für die Cloud mit elastischer Skalierung konzipiert.</span><span class="sxs-lookup"><span data-stu-id="bb080-228">It offers configurable and reliable performance, native JavaScript transactional processing, and is built for the cloud with elastic scale.</span></span>

### <a name="retry-mechanism"></a><span data-ttu-id="bb080-229">Wiederholungsmechanismus</span><span class="sxs-lookup"><span data-stu-id="bb080-229">Retry mechanism</span></span>
<span data-ttu-id="bb080-230">Die `DocumentClient`-Klasse führt bei Fehlversuchen automatisch Wiederholungsversuche durch.</span><span class="sxs-lookup"><span data-stu-id="bb080-230">The `DocumentClient` class automatically retries failed attempts.</span></span> <span data-ttu-id="bb080-231">Konfigurieren Sie zum Festlegen der Anzahl von Wiederholungsversuchen und der maximalen Wartezeit [ConnectionPolicy.RetryOptions].</span><span class="sxs-lookup"><span data-stu-id="bb080-231">To set the number of retries and the maximum wait time, configure [ConnectionPolicy.RetryOptions].</span></span> <span data-ttu-id="bb080-232">Ausnahmen, die vom Client ausgelöst werden, unterliegen entweder nicht der Wiederholungsrichtlinie oder sind keine vorübergehenden Fehler.</span><span class="sxs-lookup"><span data-stu-id="bb080-232">Exceptions that the client raises are either beyond the retry policy or are not transient errors.</span></span>

<span data-ttu-id="bb080-233">Wenn Cosmos DB den Client einschränkt, wird ein HTTP 429-Fehler zurückgegeben.</span><span class="sxs-lookup"><span data-stu-id="bb080-233">If Cosmos DB throttles the client, it returns an HTTP 429 error.</span></span> <span data-ttu-id="bb080-234">Überprüfen Sie den Statuscode unter `DocumentClientException`.</span><span class="sxs-lookup"><span data-stu-id="bb080-234">Check the status code in the `DocumentClientException`.</span></span>

### <a name="policy-configuration"></a><span data-ttu-id="bb080-235">Richtlinienkonfiguration</span><span class="sxs-lookup"><span data-stu-id="bb080-235">Policy configuration</span></span>
<span data-ttu-id="bb080-236">Die folgende Tabelle enthält die Standardeinstellungen für die `RetryOptions`-Klasse.</span><span class="sxs-lookup"><span data-stu-id="bb080-236">The following table shows the default settings for the `RetryOptions` class.</span></span>

| <span data-ttu-id="bb080-237">Einstellung</span><span class="sxs-lookup"><span data-stu-id="bb080-237">Setting</span></span> | <span data-ttu-id="bb080-238">Standardwert</span><span class="sxs-lookup"><span data-stu-id="bb080-238">Default value</span></span> | <span data-ttu-id="bb080-239">BESCHREIBUNG</span><span class="sxs-lookup"><span data-stu-id="bb080-239">Description</span></span> |
| --- | --- | --- |
| <span data-ttu-id="bb080-240">MaxRetryAttemptsOnThrottledRequests</span><span class="sxs-lookup"><span data-stu-id="bb080-240">MaxRetryAttemptsOnThrottledRequests</span></span> |<span data-ttu-id="bb080-241">9</span><span class="sxs-lookup"><span data-stu-id="bb080-241">9</span></span> |<span data-ttu-id="bb080-242">Die maximale Anzahl von Wiederholungsversuchen bei einem Fehlschlagen der Anforderung, weil Cosmos DB die Rateneinschränkung auf den Client angewendet hat.</span><span class="sxs-lookup"><span data-stu-id="bb080-242">The maximum number of retries if the request fails because Cosmos DB applied rate limiting on the client.</span></span> |
| <span data-ttu-id="bb080-243">MaxRetryWaitTimeInSeconds</span><span class="sxs-lookup"><span data-stu-id="bb080-243">MaxRetryWaitTimeInSeconds</span></span> |<span data-ttu-id="bb080-244">30</span><span class="sxs-lookup"><span data-stu-id="bb080-244">30</span></span> |<span data-ttu-id="bb080-245">Die maximale Wiederholungszeit in Sekunden.</span><span class="sxs-lookup"><span data-stu-id="bb080-245">The maximum retry time in seconds.</span></span> |

### <a name="example"></a><span data-ttu-id="bb080-246">Beispiel</span><span class="sxs-lookup"><span data-stu-id="bb080-246">Example</span></span>
```csharp
DocumentClient client = new DocumentClient(new Uri(endpoint), authKey); ;
var options = client.ConnectionPolicy.RetryOptions;
options.MaxRetryAttemptsOnThrottledRequests = 5;
options.MaxRetryWaitTimeInSeconds = 15;
```

### <a name="telemetry"></a><span data-ttu-id="bb080-247">Telemetrie</span><span class="sxs-lookup"><span data-stu-id="bb080-247">Telemetry</span></span>
<span data-ttu-id="bb080-248">Wiederholungsversuche werden als unstrukturierte Ablaufverfolgungsmeldungen über eine .NET **TraceSource**protokolliert.</span><span class="sxs-lookup"><span data-stu-id="bb080-248">Retry attempts are logged as unstructured trace messages through a .NET **TraceSource**.</span></span> <span data-ttu-id="bb080-249">Sie müssen einen **TraceListener** konfigurieren, um die Ereignisse zu erfassen und diese in ein geeignetes Zielprotokoll zu schreiben.</span><span class="sxs-lookup"><span data-stu-id="bb080-249">You must configure a **TraceListener** to capture the events and write them to a suitable destination log.</span></span>

<span data-ttu-id="bb080-250">Wenn Sie Ihrer Datei „App.config“ beispielsweise Folgendes hinzugefügt haben, werden die Ablaufverfolgungen in einer Textdatei an demselben Speicherort wie die ausführbare Datei generiert:</span><span class="sxs-lookup"><span data-stu-id="bb080-250">For example, if you add the following to your App.config file, traces will be generated in a text file in the same location as the executable:</span></span>

```xml
<configuration>
  <system.diagnostics>
    <switches>
      <add name="SourceSwitch" value="Verbose"/>
    </switches>
    <sources>
      <source name="DocDBTrace" switchName="SourceSwitch" switchType="System.Diagnostics.SourceSwitch" >
        <listeners>
          <add name="MyTextListener" type="System.Diagnostics.TextWriterTraceListener" traceOutputOptions="DateTime,ProcessId,ThreadId" initializeData="CosmosDBTrace.txt"></add>
        </listeners>
      </source>
    </sources>
  </system.diagnostics>
</configuration>
```

## <a name="event-hubs"></a><span data-ttu-id="bb080-251">Event Hubs</span><span class="sxs-lookup"><span data-stu-id="bb080-251">Event Hubs</span></span>

<span data-ttu-id="bb080-252">Azure Event Hubs ist ein hyperskalierter Dienst für die Erfassung von Telemetriedaten, der Millionen von Ereignissen sammelt, transformiert und speichert.</span><span class="sxs-lookup"><span data-stu-id="bb080-252">Azure Event Hubs is a hyper-scale telemetry ingestion service that collects, transforms, and stores millions of events.</span></span>

### <a name="retry-mechanism"></a><span data-ttu-id="bb080-253">Wiederholungsmechanismus</span><span class="sxs-lookup"><span data-stu-id="bb080-253">Retry mechanism</span></span>
<span data-ttu-id="bb080-254">Das Wiederholungsverhalten in der Azure Event Hubs-Clientbibliothek wird von der `RetryPolicy`-Eigenschaft in der `EventHubClient`-Klasse gesteuert.</span><span class="sxs-lookup"><span data-stu-id="bb080-254">Retry behavior in the Azure Event Hubs Client Library is controlled by the `RetryPolicy` property on the `EventHubClient` class.</span></span> <span data-ttu-id="bb080-255">Die Standardrichtlinie führt Wiederholungsversuche mit exponentiell ansteigender Wartezeit durch, wenn Azure Event Hub eine vorübergehende Ausnahme vom Typ `EventHubsException` oder `OperationCanceledException` zurückgibt.</span><span class="sxs-lookup"><span data-stu-id="bb080-255">The default policy retries with exponential backoff when Azure Event Hub returns a transient `EventHubsException` or an `OperationCanceledException`.</span></span>

### <a name="example"></a><span data-ttu-id="bb080-256">Beispiel</span><span class="sxs-lookup"><span data-stu-id="bb080-256">Example</span></span>
```csharp
EventHubClient client = EventHubClient.CreateFromConnectionString("[event_hub_connection_string]");
client.RetryPolicy = RetryPolicy.Default;
```

### <a name="more-information"></a><span data-ttu-id="bb080-257">Weitere Informationen</span><span class="sxs-lookup"><span data-stu-id="bb080-257">More information</span></span>
<span data-ttu-id="bb080-258">[.NET Standard client library for Azure Event Hubs](https://github.com/Azure/azure-event-hubs-dotnet) (.NET Standard-Clientbibliothek für Azure Event Hubs)</span><span class="sxs-lookup"><span data-stu-id="bb080-258">[ .NET Standard client library for Azure Event Hubs](https://github.com/Azure/azure-event-hubs-dotnet)</span></span>

## <a name="azure-redis-cache"></a><span data-ttu-id="bb080-259">Azure Redis Cache</span><span class="sxs-lookup"><span data-stu-id="bb080-259">Azure Redis Cache</span></span>
<span data-ttu-id="bb080-260">Azure Redis Cache ist ein schneller Datenzugriff und ein Cache Service mit niedriger Latenz, der auf dem beliebten Open Source-Redis Cache basiert.</span><span class="sxs-lookup"><span data-stu-id="bb080-260">Azure Redis Cache is a fast data access and low latency cache service based on the popular open source Redis Cache.</span></span> <span data-ttu-id="bb080-261">Er ist sicher, von Microsoft verwaltet und ist von jeder Anwendung in Azure zugänglich.</span><span class="sxs-lookup"><span data-stu-id="bb080-261">It is secure, managed by Microsoft, and is accessible from any application in Azure.</span></span>

<span data-ttu-id="bb080-262">Die Anleitung in diesem Abschnitt beruht auf der Verwendung des StackExchange.Redis-Clients für den Zugriff auf den Cache.</span><span class="sxs-lookup"><span data-stu-id="bb080-262">The guidance in this section is based on using the StackExchange.Redis client to access the cache.</span></span> <span data-ttu-id="bb080-263">Eine Liste der anderer geeigneter Clients finden Sie auf der [Redis-Website](http://redis.io/clients), diese haben möglicherweise unterschiedliche Wiederholungsmechanismen.</span><span class="sxs-lookup"><span data-stu-id="bb080-263">A list of other suitable clients can be found on the [Redis website](http://redis.io/clients), and these may have different retry mechanisms.</span></span>

<span data-ttu-id="bb080-264">Beachten Sie, dass der StackExchange.Redis Client Multiplex über eine einzige Verbindung verwendet.</span><span class="sxs-lookup"><span data-stu-id="bb080-264">Note that the StackExchange.Redis client uses multiplexing through a single connection.</span></span> <span data-ttu-id="bb080-265">Die empfohlene Verwendung ist das Erstellen einer Instanz des Clients beim Starten der Anwendung und die Verwendung dieser Instanz für alle Vorgänge im Cache.</span><span class="sxs-lookup"><span data-stu-id="bb080-265">The recommended usage is to create an instance of the client at application startup and use this instance for all operations against the cache.</span></span> <span data-ttu-id="bb080-266">Aus diesem Grund wird die Verbindung mit dem Cache nur einmal hergestellt und die Anleitungen in diesem Abschnitt beziehen sich auf die Wiederholungsrichtlinie für die anfängliche Verbindung - und nicht für jeden Vorgang, der auf den Cache zugreift.</span><span class="sxs-lookup"><span data-stu-id="bb080-266">For this reason, the connection to the cache is made only once, and so all of the guidance in this section is related to the retry policy for this initial connection—and not for each operation that accesses the cache.</span></span>

### <a name="retry-mechanism"></a><span data-ttu-id="bb080-267">Wiederholungsmechanismus</span><span class="sxs-lookup"><span data-stu-id="bb080-267">Retry mechanism</span></span>
<span data-ttu-id="bb080-268">Der StackExchange.Redis-Client verwendet eine Verbindungs-Manager-Klasse, die über eine Reihe von Optionen konfiguriert ist, z.B.:</span><span class="sxs-lookup"><span data-stu-id="bb080-268">The StackExchange.Redis client uses a connection manager class that is configured through a set of options, incuding:</span></span>

- <span data-ttu-id="bb080-269">**ConnectRetry**:</span><span class="sxs-lookup"><span data-stu-id="bb080-269">**ConnectRetry**.</span></span> <span data-ttu-id="bb080-270">Die Anzahl, wie häufig erneut versucht wird, eine fehlgeschlagene Verbindung mit dem Cache herzustellen.</span><span class="sxs-lookup"><span data-stu-id="bb080-270">The number of times a failed connection to the cache will be retried.</span></span>
- <span data-ttu-id="bb080-271">**ReconnectRetryPolicy**:</span><span class="sxs-lookup"><span data-stu-id="bb080-271">**ReconnectRetryPolicy**.</span></span> <span data-ttu-id="bb080-272">Die Wiederholungsstrategie, die verwendet werden soll.</span><span class="sxs-lookup"><span data-stu-id="bb080-272">The retry strategy to use.</span></span>
- <span data-ttu-id="bb080-273">**ConnectTimeout**:</span><span class="sxs-lookup"><span data-stu-id="bb080-273">**ConnectTimeout**.</span></span> <span data-ttu-id="bb080-274">Die maximale Wartezeit in Millisekunden.</span><span class="sxs-lookup"><span data-stu-id="bb080-274">The maximum waiting time in milliseconds.</span></span>

### <a name="policy-configuration"></a><span data-ttu-id="bb080-275">Richtlinienkonfiguration</span><span class="sxs-lookup"><span data-stu-id="bb080-275">Policy configuration</span></span>
<span data-ttu-id="bb080-276">Wiederholungsrichtlinien werden programmgesteuert durch Festlegen der Optionen für den Client konfiguriert, bevor eine Verbindung mit dem Cache hergestellt wird.</span><span class="sxs-lookup"><span data-stu-id="bb080-276">Retry policies are configured programmatically by setting the options for the client before connecting to the cache.</span></span> <span data-ttu-id="bb080-277">Dies kann durch das Erstellen einer Instanz der **ConfigurationOptions**-Klasse, das Auffüllen ihrer Eigenschaften und die Übergabe an die **Connect**-Methode erreicht werden.</span><span class="sxs-lookup"><span data-stu-id="bb080-277">This can be done by creating an instance of the **ConfigurationOptions** class, populating its properties, and passing it to the **Connect** method.</span></span>

<span data-ttu-id="bb080-278">Die integrierten Klassen unterstützen die lineare (konstante) Verzögerung und exponentiell ansteigende Wartezeiten mit zufälligen Wiederholungsintervallen.</span><span class="sxs-lookup"><span data-stu-id="bb080-278">The built-in classes support linear (constant) delay and exponential backoff with randomized retry intervals.</span></span> <span data-ttu-id="bb080-279">Sie können auch eine benutzerdefinierte Wiederholungsrichtlinie erstellen, indem Sie die **IReconnectRetryPolicy**-Schnittstelle implementieren.</span><span class="sxs-lookup"><span data-stu-id="bb080-279">You can also create a custom retry policy by implementing the **IReconnectRetryPolicy** interface.</span></span>

<span data-ttu-id="bb080-280">Im folgenden Beispiel wird eine Wiederholungsstrategie mit exponentiell ansteigender Wartezeit konfiguriert.</span><span class="sxs-lookup"><span data-stu-id="bb080-280">The following example configures a retry strategy using exponential backoff.</span></span>

```csharp
var deltaBackOffInMilliseconds = TimeSpan.FromSeconds(5).Milliseconds;
var maxDeltaBackOffInMilliseconds = TimeSpan.FromSeconds(20).Milliseconds;
var options = new ConfigurationOptions
{
    EndPoints = {"localhost"},
    ConnectRetry = 3,
    ReconnectRetryPolicy = new ExponentialRetry(deltaBackOffInMilliseconds, maxDeltaBackOffInMilliseconds),
    ConnectTimeout = 2000
};
ConnectionMultiplexer redis = ConnectionMultiplexer.Connect(options, writer);
```

<span data-ttu-id="bb080-281">Alternativ können Sie die Optionen als Zeichenfolge angeben und diese an die **Connect** Methode übergeben.</span><span class="sxs-lookup"><span data-stu-id="bb080-281">Alternatively, you can specify the options as a string, and pass this to the **Connect** method.</span></span> <span data-ttu-id="bb080-282">Beachten Sie, dass die **ReconnectRetryPolicy**-Eigenschaft auf diese Weise nicht festgelegt werden kann, sondern nur per Code.</span><span class="sxs-lookup"><span data-stu-id="bb080-282">Note that the **ReconnectRetryPolicy** property cannot be set this way, only through code.</span></span>

```csharp
var options = "localhost,connectRetry=3,connectTimeout=2000";
ConnectionMultiplexer redis = ConnectionMultiplexer.Connect(options, writer);
```

<span data-ttu-id="bb080-283">Es ist auch möglich, Optionen direkt beim Herstellen der Verbindung mit dem Cache anzugeben.</span><span class="sxs-lookup"><span data-stu-id="bb080-283">You can also specify options directly when you connect to the cache.</span></span>

```csharp
var conn = ConnectionMultiplexer.Connect("redis0:6380,redis1:6380,connectRetry=3");
```

<span data-ttu-id="bb080-284">Weitere Informationen finden Sie unter [Configuration](https://stackexchange.github.io/StackExchange.Redis/Configuration) (Konfiguration) in der StackExchange.Redis-Dokumentation.</span><span class="sxs-lookup"><span data-stu-id="bb080-284">For more information, see [Stack Exchange Redis Configuration](https://stackexchange.github.io/StackExchange.Redis/Configuration) in the StackExchange.Redis documentation.</span></span>

<span data-ttu-id="bb080-285">Die folgende Tabelle zeigt die Standardeinstellungen für die integrierte Wiederholungsrichtlinie.</span><span class="sxs-lookup"><span data-stu-id="bb080-285">The following table shows the default settings for the built-in retry policy.</span></span>

| <span data-ttu-id="bb080-286">**Context**</span><span class="sxs-lookup"><span data-stu-id="bb080-286">**Context**</span></span> | <span data-ttu-id="bb080-287">**Einstellung**</span><span class="sxs-lookup"><span data-stu-id="bb080-287">**Setting**</span></span> | <span data-ttu-id="bb080-288">**Standardwert**</span><span class="sxs-lookup"><span data-stu-id="bb080-288">**Default value**</span></span><br /><span data-ttu-id="bb080-289">(v 1.2.2)</span><span class="sxs-lookup"><span data-stu-id="bb080-289">(v 1.2.2)</span></span> | <span data-ttu-id="bb080-290">**Bedeutung**</span><span class="sxs-lookup"><span data-stu-id="bb080-290">**Meaning**</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="bb080-291">ConfigurationOptions</span><span class="sxs-lookup"><span data-stu-id="bb080-291">ConfigurationOptions</span></span> |<span data-ttu-id="bb080-292">ConnectRetry</span><span class="sxs-lookup"><span data-stu-id="bb080-292">ConnectRetry</span></span><br /><br /><span data-ttu-id="bb080-293">ConnectTimeout</span><span class="sxs-lookup"><span data-stu-id="bb080-293">ConnectTimeout</span></span><br /><br /><span data-ttu-id="bb080-294">SyncTimeout</span><span class="sxs-lookup"><span data-stu-id="bb080-294">SyncTimeout</span></span><br /><br /><span data-ttu-id="bb080-295">ReconnectRetryPolicy</span><span class="sxs-lookup"><span data-stu-id="bb080-295">ReconnectRetryPolicy</span></span> |<span data-ttu-id="bb080-296">3</span><span class="sxs-lookup"><span data-stu-id="bb080-296">3</span></span><br /><br /><span data-ttu-id="bb080-297">Maximal 5000 ms plus SyncTimeout</span><span class="sxs-lookup"><span data-stu-id="bb080-297">Maximum 5000 ms plus SyncTimeout</span></span><br /><span data-ttu-id="bb080-298">1000</span><span class="sxs-lookup"><span data-stu-id="bb080-298">1000</span></span><br /><br /><span data-ttu-id="bb080-299">LinearRetry 5000 ms</span><span class="sxs-lookup"><span data-stu-id="bb080-299">LinearRetry 5000 ms</span></span> |<span data-ttu-id="bb080-300">Die Anzahl der Wiederholungen von Verbindungsversuchen während des Erstverbindungsvorgangs.</span><span class="sxs-lookup"><span data-stu-id="bb080-300">The number of times to repeat connect attempts during the initial connection operation.</span></span><br /><span data-ttu-id="bb080-301">Zeitlimit (ms) für Verbindungen.</span><span class="sxs-lookup"><span data-stu-id="bb080-301">Timeout (ms) for connect operations.</span></span> <span data-ttu-id="bb080-302">Keine Verzögerung zwischen den Wiederholungsversuchen.</span><span class="sxs-lookup"><span data-stu-id="bb080-302">Not a delay between retry attempts.</span></span><br /><span data-ttu-id="bb080-303">Zeit (ms), um synchrone Vorgänge zu ermöglichen.</span><span class="sxs-lookup"><span data-stu-id="bb080-303">Time (ms) to allow for synchronous operations.</span></span><br /><br /><span data-ttu-id="bb080-304">Wiederholung jeweils nach 5000 ms.</span><span class="sxs-lookup"><span data-stu-id="bb080-304">Retry every 5000 ms.</span></span>|

> [!NOTE]
> <span data-ttu-id="bb080-305">Für synchrone Vorgänge kann `SyncTimeout` einen Beitrag zur End-to-End-Wartezeit leisten, aber wenn der Wert zu niedrig festgelegt wird, kann es zu übermäßigen Zeitüberschreitungen kommen.</span><span class="sxs-lookup"><span data-stu-id="bb080-305">For synchronous operations, `SyncTimeout` can add to the end-to-end latency, but setting the value too low can cause excessive timeouts.</span></span> <span data-ttu-id="bb080-306">Weitere Informationen finden Sie unter [Problembehandlung für Azure Redis Cache][redis-cache-troubleshoot].</span><span class="sxs-lookup"><span data-stu-id="bb080-306">See [How to troubleshoot Azure Redis Cache][redis-cache-troubleshoot].</span></span> <span data-ttu-id="bb080-307">Im Allgemeinen ist es ratsam, die Verwendung von synchronen Vorgängen zu vermeiden und stattdessen asynchrone Vorgänge zu verwenden.</span><span class="sxs-lookup"><span data-stu-id="bb080-307">In general, avoid using synchronous operations, and use asynchronous operations instead.</span></span> <span data-ttu-id="bb080-308">Weitere Informationen finden Sie unter [Pipelines und Multiplexers](http://github.com/StackExchange/StackExchange.Redis/blob/master/Docs/PipelinesMultiplexers.md).</span><span class="sxs-lookup"><span data-stu-id="bb080-308">For more information see [Pipelines and Multiplexers](http://github.com/StackExchange/StackExchange.Redis/blob/master/Docs/PipelinesMultiplexers.md).</span></span>
>
>

### <a name="retry-usage-guidance"></a><span data-ttu-id="bb080-309">Gebrauchsanleitung Wiederholungen</span><span class="sxs-lookup"><span data-stu-id="bb080-309">Retry usage guidance</span></span>
<span data-ttu-id="bb080-310">Berücksichtigen Sie bei Verwendung von Azure Redis Cache die folgenden Richtlinien :</span><span class="sxs-lookup"><span data-stu-id="bb080-310">Consider the following guidelines when using Azure Redis Cache:</span></span>

* <span data-ttu-id="bb080-311">Der StackExchange Redis-Client verwaltet seine eigene Wiederholungen, aber nur beim Herstellen einer Verbindung mit dem Cache beim ersten Starten der Anwendung.</span><span class="sxs-lookup"><span data-stu-id="bb080-311">The StackExchange Redis client manages its own retries, but only when establishing a connection to the cache when the application first starts.</span></span> <span data-ttu-id="bb080-312">Sie können das Verbindungstimeout, die Anzahl von Wiederholungsversuchen und die Dauer zwischen den Wiederholungen zur Herstellung dieser Verbindung konfigurieren, aber die Wiederholungsrichtlinie gilt nicht für Vorgänge für den Cache.</span><span class="sxs-lookup"><span data-stu-id="bb080-312">You can configure the connection timeout, the number of retry attempts, and the time between retries to establish this connection, but the retry policy does not apply to operations against the cache.</span></span>
* <span data-ttu-id="bb080-313">Anstatt eine große Anzahl von Wiederholungsversuchen zu verwenden, sollten Sie auf die ursprüngliche Datenquelle zurückgreifen.</span><span class="sxs-lookup"><span data-stu-id="bb080-313">Instead of using a large number of retry attempts, consider falling back by accessing the original data source instead.</span></span>

### <a name="telemetry"></a><span data-ttu-id="bb080-314">Telemetrie</span><span class="sxs-lookup"><span data-stu-id="bb080-314">Telemetry</span></span>
<span data-ttu-id="bb080-315">Sie können Informationen zu Verbindungen (aber nicht für andere Vorgänge) mit einem **TextWriter**sammeln.</span><span class="sxs-lookup"><span data-stu-id="bb080-315">You can collect information about connections (but not other operations) using a **TextWriter**.</span></span>

```csharp
var writer = new StringWriter();
ConnectionMultiplexer redis = ConnectionMultiplexer.Connect(options, writer);
```

<span data-ttu-id="bb080-316">Ein Beispiel für die dadurch generierte Ausgabe ist unten dargestellt.</span><span class="sxs-lookup"><span data-stu-id="bb080-316">An example of the output this generates is shown below.</span></span>

```text
localhost:6379,connectTimeout=2000,connectRetry=3
1 unique nodes specified
Requesting tie-break from localhost:6379 > __Booksleeve_TieBreak...
Allowing endpoints 00:00:02 to respond...
localhost:6379 faulted: SocketFailure on PING
localhost:6379 failed to nominate (Faulted)
> UnableToResolvePhysicalConnection on GET
No masters detected
localhost:6379: Standalone v2.0.0, master; keep-alive: 00:01:00; int: Connecting; sub: Connecting; not in use: DidNotRespond
localhost:6379: int ops=0, qu=0, qs=0, qc=1, wr=0, sync=1, socks=2; sub ops=0, qu=0, qs=0, qc=0, wr=0, socks=2
Circular op-count snapshot; int: 0 (0.00 ops/s; spans 10s); sub: 0 (0.00 ops/s; spans 10s)
Sync timeouts: 0; fire and forget: 0; last heartbeat: -1s ago
resetting failing connections to retry...
retrying; attempts left: 2...
...
```

### <a name="examples"></a><span data-ttu-id="bb080-317">Beispiele</span><span class="sxs-lookup"><span data-stu-id="bb080-317">Examples</span></span>
<span data-ttu-id="bb080-318">Im folgenden Codebeispiel wird eine konstante (lineare) Verzögerung zwischen Wiederholungsversuchen konfiguriert, wenn der StackExchange.Redis-Client initialisiert wird.</span><span class="sxs-lookup"><span data-stu-id="bb080-318">The following code example configures a constant (linear) delay between retries when initializing the StackExchange.Redis client.</span></span> <span data-ttu-id="bb080-319">In diesem Beispiel wird veranschaulicht, wie die Konfiguration mit einer **ConfigurationOptions**-Instanz festgelegt wird.</span><span class="sxs-lookup"><span data-stu-id="bb080-319">This example shows how to set the configuration using a **ConfigurationOptions** instance.</span></span>

```csharp
using System;
using System.Collections.Generic;
using System.IO;
using System.Linq;
using System.Text;
using System.Threading.Tasks;
using StackExchange.Redis;

namespace RetryCodeSamples
{
    class CacheRedisCodeSamples
    {
        public async static Task Samples()
        {
            var writer = new StringWriter();
            {
                try
                {
                    var retryTimeInMilliseconds = TimeSpan.FromSeconds(4).Milliseconds; // delay between retries

                    // Using object-based configuration.
                    var options = new ConfigurationOptions
                                        {
                                            EndPoints = { "localhost" },
                                            ConnectRetry = 3,
                                            ReconnectRetryPolicy = new LinearRetry(retryTimeInMilliseconds)
                                        };
                    ConnectionMultiplexer redis = ConnectionMultiplexer.Connect(options, writer);

                    // Store a reference to the multiplexer for use in the application.
                }
                catch
                {
                    Console.WriteLine(writer.ToString());
                    throw;
                }
            }
        }
    }
}
```

<span data-ttu-id="bb080-320">Im nächsten Beispiel wird die Konfiguration durch Angeben der Optionen als Zeichenfolge festgelegt.</span><span class="sxs-lookup"><span data-stu-id="bb080-320">The next example sets the configuration by specifying the options as a string.</span></span> <span data-ttu-id="bb080-321">Das Verbindungstimeout ist der maximale Zeitraum für das Warten auf die Verbindungsherstellung mit dem Cache und nicht die Verzögerung zwischen Wiederholungsversuchen.</span><span class="sxs-lookup"><span data-stu-id="bb080-321">The connection timeout is the maximum period of time to wait for a connection to the cache, not the delay between retry attempts.</span></span> <span data-ttu-id="bb080-322">Beachten Sie, dass die **ReconnectRetryPolicy**-Eigenschaft nur per Code festgelegt werden kann.</span><span class="sxs-lookup"><span data-stu-id="bb080-322">Note that the **ReconnectRetryPolicy** property can only be set by code.</span></span>

```csharp
using System.Collections.Generic;
using System.IO;
using System.Linq;
using System.Text;
using System.Threading.Tasks;
using StackExchange.Redis;

namespace RetryCodeSamples
{
    class CacheRedisCodeSamples
    {
        public async static Task Samples()
        {
            var writer = new StringWriter();
            {
                try
                {
                    // Using string-based configuration.
                    var options = "localhost,connectRetry=3,connectTimeout=2000";
                    ConnectionMultiplexer redis = ConnectionMultiplexer.Connect(options, writer);

                    // Store a reference to the multiplexer for use in the application.
                }
                catch
                {
                    Console.WriteLine(writer.ToString());
                    throw;
                }
            }
        }
    }
}
```

<span data-ttu-id="bb080-323">Weitere Beispiele finden Sie unter [Konfiguration](http://github.com/StackExchange/StackExchange.Redis/blob/master/Docs/Configuration.md#configuration) auf der Projektwebsite.</span><span class="sxs-lookup"><span data-stu-id="bb080-323">For more examples, see [Configuration](http://github.com/StackExchange/StackExchange.Redis/blob/master/Docs/Configuration.md#configuration) on the project website.</span></span>

### <a name="more-information"></a><span data-ttu-id="bb080-324">Weitere Informationen</span><span class="sxs-lookup"><span data-stu-id="bb080-324">More information</span></span>
* [<span data-ttu-id="bb080-325">Redis-Website</span><span class="sxs-lookup"><span data-stu-id="bb080-325">Redis website</span></span>](http://redis.io/)

## <a name="azure-search"></a><span data-ttu-id="bb080-326">Azure Search</span><span class="sxs-lookup"><span data-stu-id="bb080-326">Azure Search</span></span>
<span data-ttu-id="bb080-327">Azure Search kann dafür verwendet werden, nützliche und anspruchsvolle Suchfunktionen zu einer Website oder Anwendung hinzuzufügen, Suchergebnisse schnell und einfach zu optimieren und umfassende und fein abgestimmte Rangmodelle zu erstellen.</span><span class="sxs-lookup"><span data-stu-id="bb080-327">Azure Search can be used to add powerful and sophisticated search capabilities to a website or application, quickly and easily tune search results, and construct rich and fine-tuned ranking models.</span></span>

### <a name="retry-mechanism"></a><span data-ttu-id="bb080-328">Wiederholungsmechanismus</span><span class="sxs-lookup"><span data-stu-id="bb080-328">Retry mechanism</span></span>
<span data-ttu-id="bb080-329">Das Wiederholungsverhalten im Azure Search SDK wird mit der `SetRetryPolicy`-Methode in den Klassen [SearchServiceClient] und [SearchIndexClient] gesteuert.</span><span class="sxs-lookup"><span data-stu-id="bb080-329">Retry behavior in the Azure Search SDK is controlled by the `SetRetryPolicy` method on the [SearchServiceClient] and [SearchIndexClient] classes.</span></span> <span data-ttu-id="bb080-330">Bei der Standardrichtlinie werden Wiederholungsversuche mit exponentiellem Backoff (exponentiell ansteigende Wartezeiten) durchgeführt, wenn Azure Search eine 5xx- oder 408-Antwort (Anforderungstimeout) zurückgibt.</span><span class="sxs-lookup"><span data-stu-id="bb080-330">The default policy retries with exponential backoff when Azure Search returns a 5xx or 408 (Request Timeout) response.</span></span>

### <a name="telemetry"></a><span data-ttu-id="bb080-331">Telemetrie</span><span class="sxs-lookup"><span data-stu-id="bb080-331">Telemetry</span></span>
<span data-ttu-id="bb080-332">Nachverfolgung mit ETW oder per Registrierung eines benutzerdefinierten Ablaufverfolgungsanbieters.</span><span class="sxs-lookup"><span data-stu-id="bb080-332">Trace with ETW or by registering a custom trace provider.</span></span> <span data-ttu-id="bb080-333">Weitere Informationen finden Sie in der [AutoRest-Dokumentation][autorest].</span><span class="sxs-lookup"><span data-stu-id="bb080-333">For more information, see the [AutoRest documentation][autorest].</span></span>

## <a name="service-bus"></a><span data-ttu-id="bb080-334">Service Bus</span><span class="sxs-lookup"><span data-stu-id="bb080-334">Service Bus</span></span>
<span data-ttu-id="bb080-335">Service Bus ist ein Cloud  Messaging-Plattform, die lose gekoppelten Nachrichtenaustausch mit verbesserter Skalierbarkeit und Stabilität für die Komponenten einer Anwendung bietet, egal ob in der Cloud oder lokal gehostet.</span><span class="sxs-lookup"><span data-stu-id="bb080-335">Service Bus is a cloud messaging platform that provides loosely coupled message exchange with improved scale and resiliency for components of an application, whether hosted in the cloud or on-premises.</span></span>

### <a name="retry-mechanism"></a><span data-ttu-id="bb080-336">Wiederholungsmechanismus</span><span class="sxs-lookup"><span data-stu-id="bb080-336">Retry mechanism</span></span>
<span data-ttu-id="bb080-337">Service Bus implementiert Wiederholungen mithilfe von Implementierungen der [RetryPolicy](http://msdn.microsoft.com/library/microsoft.servicebus.retrypolicy.aspx) -Basisklasse.</span><span class="sxs-lookup"><span data-stu-id="bb080-337">Service Bus implements retries using implementations of the [RetryPolicy](http://msdn.microsoft.com/library/microsoft.servicebus.retrypolicy.aspx) base class.</span></span> <span data-ttu-id="bb080-338">Alle Service Bus-Clients machen eine **RetryPolicy**-Eigenschaft verfügbar, die auf eine der Implementierungen der **RetryPolicy**-Basisklasse festgelegt werden kann.</span><span class="sxs-lookup"><span data-stu-id="bb080-338">All of the Service Bus clients expose a **RetryPolicy** property that can be set to one of the implementations of the **RetryPolicy** base class.</span></span> <span data-ttu-id="bb080-339">Die integrierten Implementierungen sind:</span><span class="sxs-lookup"><span data-stu-id="bb080-339">The built-in implementations are:</span></span>

* <span data-ttu-id="bb080-340">Die [RetryExponential Class](http://msdn.microsoft.com/library/microsoft.servicebus.retryexponential.aspx).</span><span class="sxs-lookup"><span data-stu-id="bb080-340">The [RetryExponential Class](http://msdn.microsoft.com/library/microsoft.servicebus.retryexponential.aspx).</span></span> <span data-ttu-id="bb080-341">Dies macht Eigenschaften verfügbar, die das Backoff-Intervall, die Anzahl der Wiederholungsversuche und die **TerminationTimeBuffer** -Eigenschaft steuern, die verwendet wird, um die Gesamtzeit für die Ausführung des Vorgangs zu beschränken.</span><span class="sxs-lookup"><span data-stu-id="bb080-341">This exposes properties that control the back-off interval, the retry count, and the **TerminationTimeBuffer** property that is used to limit the total time for the operation to complete.</span></span>
* <span data-ttu-id="bb080-342">Die [NoRetry-Klasse](http://msdn.microsoft.com/library/microsoft.servicebus.noretry.aspx).</span><span class="sxs-lookup"><span data-stu-id="bb080-342">The [NoRetry Class](http://msdn.microsoft.com/library/microsoft.servicebus.noretry.aspx).</span></span> <span data-ttu-id="bb080-343">Dies wird verwendet, wenn Wiederholungen auf der Ebene des Service Bus-API Levels  nicht erforderlich sind, z. B. wenn Wiederholungen von einem anderen Prozess als Teil eines Batches oder als Vorgang mit mehreren Schritten verwaltet werden.</span><span class="sxs-lookup"><span data-stu-id="bb080-343">This is used when retries at the Service Bus API level are not required, such as when retries are managed by another process as part of a batch or multiple step operation.</span></span>

<span data-ttu-id="bb080-344">Service Bus-Aktionen können einen Reihe von Ausnahmen zurückgeben, wie im [Anhang: Messagingausnahmen](http://msdn.microsoft.com/library/hh418082.aspx)aufgeführt.</span><span class="sxs-lookup"><span data-stu-id="bb080-344">Service Bus actions can return a range of exceptions, as listed in [Appendix: Messaging Exceptions](http://msdn.microsoft.com/library/hh418082.aspx).</span></span> <span data-ttu-id="bb080-345">Die Liste enthält Informationen darüber, ob diese für die Wiederholung des Vorgangs geeignet ist.</span><span class="sxs-lookup"><span data-stu-id="bb080-345">The list provides information about which if these indicate that retrying the operation is appropriate.</span></span> <span data-ttu-id="bb080-346">Angenommen, eine [ServerBusyException](http://msdn.microsoft.com/library/microsoft.servicebus.messaging.serverbusyexception.aspx) gibt an, dass der Client für eine bestimmte Zeitspanne warten sollte, bevor der Vorgang wiederholt wird.</span><span class="sxs-lookup"><span data-stu-id="bb080-346">For example, a [ServerBusyException](http://msdn.microsoft.com/library/microsoft.servicebus.messaging.serverbusyexception.aspx) indicates that the client should wait for a period of time, then retry the operation.</span></span> <span data-ttu-id="bb080-347">Das Auftreten einer **ServerBusyException** bewirkt auch, dass Service Bus in einen anderen Modus wechselt, in dem den berechneten Wiederholungsverzögerungen zusätzliche 10 Sekunden hinzugefügt werden.</span><span class="sxs-lookup"><span data-stu-id="bb080-347">The occurrence of a **ServerBusyException** also causes Service Bus to switch to a different mode, in which an extra 10-second delay is added to the computed retry delays.</span></span> <span data-ttu-id="bb080-348">Dieser Modus wird nach kurzer Zeit zurückgesetzt.</span><span class="sxs-lookup"><span data-stu-id="bb080-348">This mode is reset after a short period.</span></span>

<span data-ttu-id="bb080-349">Die vom Service Bus zurückgegebenen Ausnahmen machen die **IsTransient**-Eigenschaft verfügbar, die angibt, ob der Client den Vorgang wiederholen soll.</span><span class="sxs-lookup"><span data-stu-id="bb080-349">The exceptions returned from Service Bus expose the **IsTransient** property that indicates if the client should retry the operation.</span></span> <span data-ttu-id="bb080-350">Die integrierte **RetryExponential**-Richtlinie stützt sich auf die **IsTransient**-Eigenschaft in der **MessagingException**-Klasse, die die Basisklasse für alle Service Bus-Ausnahmen ist.</span><span class="sxs-lookup"><span data-stu-id="bb080-350">The built-in **RetryExponential** policy relies on the **IsTransient** property in the **MessagingException** class, which is the base class for all Service Bus exceptions.</span></span> <span data-ttu-id="bb080-351">Bei der Erstellung benutzerdefinierter Implementierungen der **RetryPolicy**-Basisklasse können Sie eine Kombination aus Ausnahmetyp und **IsTransient**-Eigenschaft verwenden, um eine genauere Kontrolle über Wiederholungsaktionen zu ermöglichen.</span><span class="sxs-lookup"><span data-stu-id="bb080-351">If you create custom implementations of the **RetryPolicy** base class you could use a combination of the exception type and the **IsTransient** property to provide more fine-grained control over retry actions.</span></span> <span data-ttu-id="bb080-352">Sie können z.B. eine **QuotaExceededException** erkennen und Maßnahmen ergreifen, um die Warteschlange zu leeren, bevor erneut versucht wird, darüber eine Nachricht zu senden.</span><span class="sxs-lookup"><span data-stu-id="bb080-352">For example, you could detect a **QuotaExceededException** and take action to drain the queue before retrying sending a message to it.</span></span>

### <a name="policy-configuration"></a><span data-ttu-id="bb080-353">Richtlinienkonfiguration</span><span class="sxs-lookup"><span data-stu-id="bb080-353">Policy configuration</span></span>
<span data-ttu-id="bb080-354">Wiederholungsrichtlinien werden programmgesteuert festgelegt und können als Standardrichtlinie für einen **NamespaceManager** und für eine **MessagingFactory** oder einzeln für jeden Messaging Client festgelegt werden.</span><span class="sxs-lookup"><span data-stu-id="bb080-354">Retry policies are set programmatically, and can be set as a default policy for a **NamespaceManager** and for a **MessagingFactory**, or individually for each messaging client.</span></span> <span data-ttu-id="bb080-355">Um die Standard-Wiederholungsrichtlinie für die Messaging-Sitzung festzulegen, legen Sie die **RetryPolicy** von **NamespaceManager** fest.</span><span class="sxs-lookup"><span data-stu-id="bb080-355">To set the default retry policy for a messaging session you set the **RetryPolicy** of the **NamespaceManager**.</span></span>

```csharp
namespaceManager.Settings.RetryPolicy = new RetryExponential(minBackoff: TimeSpan.FromSeconds(0.1),
                                                                maxBackoff: TimeSpan.FromSeconds(30),
                                                                maxRetryCount: 3);
```

<span data-ttu-id="bb080-356">Legen Sie zum Festlegen der Standard-Wiederholungsrichtlinie für alle Clients einer Messaging-Factory die **RetryPolicy** von **MessagingFactory** fest.</span><span class="sxs-lookup"><span data-stu-id="bb080-356">To set the default retry policy for all clients created from a messaging factory, you set the **RetryPolicy** of the **MessagingFactory**.</span></span>

```csharp
messagingFactory.RetryPolicy = new RetryExponential(minBackoff: TimeSpan.FromSeconds(0.1),
                                                    maxBackoff: TimeSpan.FromSeconds(30),
                                                    maxRetryCount: 3);
```

<span data-ttu-id="bb080-357">Um die Standard-Wiederholungsrichtlinie für einen Messaging Client festzulegen oder die Standardrichtlinie zu überschreiben, legen Sie seine **RetryPolicy** -Eigenschaft mit einer Instanz der erforderlichen Richtlinienklasse fest:</span><span class="sxs-lookup"><span data-stu-id="bb080-357">To set the retry policy for a messaging client, or to override its default policy, you set its **RetryPolicy** property using an instance of the required policy class:</span></span>

```csharp
client.RetryPolicy = new RetryExponential(minBackoff: TimeSpan.FromSeconds(0.1),
                                            maxBackoff: TimeSpan.FromSeconds(30),
                                            maxRetryCount: 3);
```

<span data-ttu-id="bb080-358">Die Wiederholungsrichtlinie kann auf der individuellen Vorgangsebene festgelegt werden.</span><span class="sxs-lookup"><span data-stu-id="bb080-358">The retry policy cannot be set at the individual operation level.</span></span> <span data-ttu-id="bb080-359">Sie gilt für alle Vorgänge des Messagin-Client.</span><span class="sxs-lookup"><span data-stu-id="bb080-359">It applies to all operations for the messaging client.</span></span>
<span data-ttu-id="bb080-360">Die folgende Tabelle zeigt die Standardeinstellungen für die integrierte Wiederholungsrichtlinie.</span><span class="sxs-lookup"><span data-stu-id="bb080-360">The following table shows the default settings for the built-in retry policy.</span></span>

| <span data-ttu-id="bb080-361">Einstellung</span><span class="sxs-lookup"><span data-stu-id="bb080-361">Setting</span></span> | <span data-ttu-id="bb080-362">Standardwert</span><span class="sxs-lookup"><span data-stu-id="bb080-362">Default value</span></span> | <span data-ttu-id="bb080-363">Bedeutung</span><span class="sxs-lookup"><span data-stu-id="bb080-363">Meaning</span></span> |
|---------|---------------|---------|
| <span data-ttu-id="bb080-364">Richtlinie</span><span class="sxs-lookup"><span data-stu-id="bb080-364">Policy</span></span> | <span data-ttu-id="bb080-365">Exponentiell</span><span class="sxs-lookup"><span data-stu-id="bb080-365">Exponential</span></span> | <span data-ttu-id="bb080-366">Exponentielles Backoff.</span><span class="sxs-lookup"><span data-stu-id="bb080-366">Exponential back-off.</span></span> |
| <span data-ttu-id="bb080-367">MinimalBackoff</span><span class="sxs-lookup"><span data-stu-id="bb080-367">MinimalBackoff</span></span> | <span data-ttu-id="bb080-368">0</span><span class="sxs-lookup"><span data-stu-id="bb080-368">0</span></span> | <span data-ttu-id="bb080-369">Das minimale Backoff-Intervall.</span><span class="sxs-lookup"><span data-stu-id="bb080-369">Minimum back-off interval.</span></span> <span data-ttu-id="bb080-370">Dieser Wert wird zum Wiederholungsintervall addiert, das auf der Grundlage von „deltaBackoff“ berechnet wurde.</span><span class="sxs-lookup"><span data-stu-id="bb080-370">This is added to the retry interval computed from deltaBackoff.</span></span> |
| <span data-ttu-id="bb080-371">MaximumBackoff</span><span class="sxs-lookup"><span data-stu-id="bb080-371">MaximumBackoff</span></span> | <span data-ttu-id="bb080-372">30 Sekunden</span><span class="sxs-lookup"><span data-stu-id="bb080-372">30 seconds</span></span> | <span data-ttu-id="bb080-373">Das maximale Backoff-Intervall.</span><span class="sxs-lookup"><span data-stu-id="bb080-373">Maximum back-off interval.</span></span> <span data-ttu-id="bb080-374">„MaximumBackoff“ wird verwendet, wenn das berechnete Wiederholungsintervall größer als „MaxBackoff“ ist.</span><span class="sxs-lookup"><span data-stu-id="bb080-374">MaximumBackoff is used if the computed retry interval is greater than MaxBackoff.</span></span> |
| <span data-ttu-id="bb080-375">DeltaBackoff</span><span class="sxs-lookup"><span data-stu-id="bb080-375">DeltaBackoff</span></span> | <span data-ttu-id="bb080-376">3 Sekunden</span><span class="sxs-lookup"><span data-stu-id="bb080-376">3 seconds</span></span> | <span data-ttu-id="bb080-377">Backoff-Intervall zwischen den Wiederholungen.</span><span class="sxs-lookup"><span data-stu-id="bb080-377">Back-off interval between retries.</span></span> <span data-ttu-id="bb080-378">Ein Vielfaches dieser Zeitspanne wird für nachfolgende Wiederholungen verwendet.</span><span class="sxs-lookup"><span data-stu-id="bb080-378">Multiples of this timespan will be used for subsequent retry attempts.</span></span> |
| <span data-ttu-id="bb080-379">TimeBuffer</span><span class="sxs-lookup"><span data-stu-id="bb080-379">TimeBuffer</span></span> | <span data-ttu-id="bb080-380">5 Sekunden</span><span class="sxs-lookup"><span data-stu-id="bb080-380">5 seconds</span></span> | <span data-ttu-id="bb080-381">Der Beendigungszeitpuffer für die Wiederholung.</span><span class="sxs-lookup"><span data-stu-id="bb080-381">The termination time buffer associated with the retry.</span></span> <span data-ttu-id="bb080-382">Wiederholungsversuche werden abgebrochen, wenn die verbleibende Zeit kleiner als „TimeBuffer“ ist.</span><span class="sxs-lookup"><span data-stu-id="bb080-382">Retry attempts will be abandoned if the remaining time is less than TimeBuffer.</span></span> |
| <span data-ttu-id="bb080-383">MaxRetryCount</span><span class="sxs-lookup"><span data-stu-id="bb080-383">MaxRetryCount</span></span> | <span data-ttu-id="bb080-384">10</span><span class="sxs-lookup"><span data-stu-id="bb080-384">10</span></span> | <span data-ttu-id="bb080-385">Die maximale Anzahl von Warnungen.</span><span class="sxs-lookup"><span data-stu-id="bb080-385">The maximum number of retries.</span></span> |
| <span data-ttu-id="bb080-386">ServerBusyBaseSleepTime</span><span class="sxs-lookup"><span data-stu-id="bb080-386">ServerBusyBaseSleepTime</span></span> | <span data-ttu-id="bb080-387">10 Sekunden</span><span class="sxs-lookup"><span data-stu-id="bb080-387">10 seconds</span></span> | <span data-ttu-id="bb080-388">Wenn es sich bei der zuletzt aufgetretenen Ausnahme um **ServerBusyException** handelt, wird dieser Wert zum berechneten Wiederholungsintervall addiert.</span><span class="sxs-lookup"><span data-stu-id="bb080-388">If the last exception encountered was **ServerBusyException**, this value will be added to the computed retry interval.</span></span> <span data-ttu-id="bb080-389">Dieser Wert kann nicht geändert werden.</span><span class="sxs-lookup"><span data-stu-id="bb080-389">This value cannot be changed.</span></span> |

### <a name="retry-usage-guidance"></a><span data-ttu-id="bb080-390">Gebrauchsanleitung Wiederholungen</span><span class="sxs-lookup"><span data-stu-id="bb080-390">Retry usage guidance</span></span>
<span data-ttu-id="bb080-391">Berücksichtigen Sie bei Verwendung von Service Bus die folgenden Richtlinien:</span><span class="sxs-lookup"><span data-stu-id="bb080-391">Consider the following guidelines when using Service Bus:</span></span>

* <span data-ttu-id="bb080-392">Implementieren Sie bei Verwendung der integrierten **RetryExponential** -Implementierung keinen Fallbackvorgang, da die Richtlinie auf Server Busy-Ausnahmen reagiert und automatisch in einen entsprechenden Wiederholungsmodus wechselt.</span><span class="sxs-lookup"><span data-stu-id="bb080-392">When using the built-in **RetryExponential** implementation, do not implement a fallback operation as the policy reacts to Server Busy exceptions and automatically switches to an appropriate retry mode.</span></span>
* <span data-ttu-id="bb080-393">Service Bus unterstützt eine Funktion namens Paired Namespace, mit der automatisches Failover zu einer Backup-Warteschlange in einem separaten Namespace implementiert wird, falls die Warteschlange im primären Namespace fehlschlägt.</span><span class="sxs-lookup"><span data-stu-id="bb080-393">Service Bus supports a feature called Paired Namespaces, which implements automatic failover to a backup queue in a separate namespace if the queue in the primary namespace fails.</span></span> <span data-ttu-id="bb080-394">Nachrichten aus der sekundären Warteschlange können an die primäre Warteschlange gesendet werden, wenn diese wiederhergestellt wird.</span><span class="sxs-lookup"><span data-stu-id="bb080-394">Messages from the secondary queue can be sent back to the primary queue when it recovers.</span></span> <span data-ttu-id="bb080-395">Mit dieser Funktion können vorübergehender Fehler behandelt werden.</span><span class="sxs-lookup"><span data-stu-id="bb080-395">This feature helps to address transient failures.</span></span> <span data-ttu-id="bb080-396">Weitere Informationen finden Sie unter [Asynchrone Nachrichtenmuster und Hochverfügbarkeit](http://msdn.microsoft.com/library/azure/dn292562.aspx).</span><span class="sxs-lookup"><span data-stu-id="bb080-396">For more information, see [Asynchronous Messaging Patterns and High Availability](http://msdn.microsoft.com/library/azure/dn292562.aspx).</span></span>

<span data-ttu-id="bb080-397">Erwägen Sie, mit den folgenden Einstellungen für Wiederholungsvorgänge zu beginnen.</span><span class="sxs-lookup"><span data-stu-id="bb080-397">Consider starting with following settings for retrying operations.</span></span> <span data-ttu-id="bb080-398">Hierbei handelt es sich um allgemeine Einstellungen und Sie sollten die Vorgänge überwachen und die Werte entsprechend Ihrem Szenario optimieren.</span><span class="sxs-lookup"><span data-stu-id="bb080-398">These are general purpose settings, and you should monitor the operations and fine tune the values to suit your own scenario.</span></span>

| <span data-ttu-id="bb080-399">Kontext</span><span class="sxs-lookup"><span data-stu-id="bb080-399">Context</span></span> | <span data-ttu-id="bb080-400">Beispiel für maximale Wartezeit</span><span class="sxs-lookup"><span data-stu-id="bb080-400">Example maximum latency</span></span> | <span data-ttu-id="bb080-401">Wiederholungsrichtlinie</span><span class="sxs-lookup"><span data-stu-id="bb080-401">Retry policy</span></span> | <span data-ttu-id="bb080-402">Einstellungen</span><span class="sxs-lookup"><span data-stu-id="bb080-402">Settings</span></span> | <span data-ttu-id="bb080-403">So funktioniert's</span><span class="sxs-lookup"><span data-stu-id="bb080-403">How it works</span></span> |
|---------|---------|---------|---------|---------|
| <span data-ttu-id="bb080-404">Interaktiv, Benutzeroberfläche oder Vordergrund</span><span class="sxs-lookup"><span data-stu-id="bb080-404">Interactive, UI, or foreground</span></span> | <span data-ttu-id="bb080-405">2 Sekunden\*</span><span class="sxs-lookup"><span data-stu-id="bb080-405">2 seconds\*</span></span>  | <span data-ttu-id="bb080-406">Exponentiell</span><span class="sxs-lookup"><span data-stu-id="bb080-406">Exponential</span></span> | <span data-ttu-id="bb080-407">MinimumBackoff = 0</span><span class="sxs-lookup"><span data-stu-id="bb080-407">MinimumBackoff = 0</span></span> <br/> <span data-ttu-id="bb080-408">MaximumBackoff = 30 s</span><span class="sxs-lookup"><span data-stu-id="bb080-408">MaximumBackoff = 30 sec.</span></span> <br/> <span data-ttu-id="bb080-409">DeltaBackoff = 300 ms</span><span class="sxs-lookup"><span data-stu-id="bb080-409">DeltaBackoff = 300 msec.</span></span> <br/> <span data-ttu-id="bb080-410">TimeBuffer = 300 ms</span><span class="sxs-lookup"><span data-stu-id="bb080-410">TimeBuffer = 300 msec.</span></span> <br/> <span data-ttu-id="bb080-411">MaxRetryCount = 2</span><span class="sxs-lookup"><span data-stu-id="bb080-411">MaxRetryCount = 2</span></span> | <span data-ttu-id="bb080-412">1. Versuch: Verzögerung von 0 s</span><span class="sxs-lookup"><span data-stu-id="bb080-412">Attempt 1: Delay 0 sec.</span></span> <br/> <span data-ttu-id="bb080-413">2. Versuch: Verzögerung von ~300 ms</span><span class="sxs-lookup"><span data-stu-id="bb080-413">Attempt 2: Delay ~300 msec.</span></span> <br/> <span data-ttu-id="bb080-414">3. Versuch: Verzögerung von ~900 ms</span><span class="sxs-lookup"><span data-stu-id="bb080-414">Attempt 3: Delay ~900 msec.</span></span> |
| <span data-ttu-id="bb080-415">Hintergrund oder Batch</span><span class="sxs-lookup"><span data-stu-id="bb080-415">Background or batch</span></span> | <span data-ttu-id="bb080-416">30 Sekunden</span><span class="sxs-lookup"><span data-stu-id="bb080-416">30 seconds</span></span> | <span data-ttu-id="bb080-417">Exponentiell</span><span class="sxs-lookup"><span data-stu-id="bb080-417">Exponential</span></span> | <span data-ttu-id="bb080-418">MinimumBackoff = 1</span><span class="sxs-lookup"><span data-stu-id="bb080-418">MinimumBackoff = 1</span></span> <br/> <span data-ttu-id="bb080-419">MaximumBackoff = 30 s</span><span class="sxs-lookup"><span data-stu-id="bb080-419">MaximumBackoff = 30 sec.</span></span> <br/> <span data-ttu-id="bb080-420">DeltaBackoff = 1,75 s</span><span class="sxs-lookup"><span data-stu-id="bb080-420">DeltaBackoff = 1.75 sec.</span></span> <br/> <span data-ttu-id="bb080-421">TimeBuffer = 5 s</span><span class="sxs-lookup"><span data-stu-id="bb080-421">TimeBuffer = 5 sec.</span></span> <br/> <span data-ttu-id="bb080-422">MaxRetryCount = 3</span><span class="sxs-lookup"><span data-stu-id="bb080-422">MaxRetryCount = 3</span></span> | <span data-ttu-id="bb080-423">1. Versuch: Verzögerung von ~1 s</span><span class="sxs-lookup"><span data-stu-id="bb080-423">Attempt 1: Delay ~1 sec.</span></span> <br/> <span data-ttu-id="bb080-424">2. Versuch: Verzögerung von ~3 s</span><span class="sxs-lookup"><span data-stu-id="bb080-424">Attempt 2: Delay ~3 sec.</span></span> <br/> <span data-ttu-id="bb080-425">3. Versuch: Verzögerung von ~6 ms</span><span class="sxs-lookup"><span data-stu-id="bb080-425">Attempt 3: Delay ~6 msec.</span></span> <br/> <span data-ttu-id="bb080-426">4. Versuch: Verzögerung von ~13 ms</span><span class="sxs-lookup"><span data-stu-id="bb080-426">Attempt 4: Delay ~13 msec.</span></span> |

<span data-ttu-id="bb080-427">\*Ohne Berücksichtigung der Verzögerung, die hinzukommt, wenn eine Antwort vom Typ „Server ausgelastet“ empfangen wird.</span><span class="sxs-lookup"><span data-stu-id="bb080-427">\* Not including additional delay that is added if a Server Busy response is received.</span></span>

### <a name="telemetry"></a><span data-ttu-id="bb080-428">Telemetrie</span><span class="sxs-lookup"><span data-stu-id="bb080-428">Telemetry</span></span>
<span data-ttu-id="bb080-429">Service Bus protokollieren Wiederholungen als ETW-Ereignisse mit einer **EventSource**.</span><span class="sxs-lookup"><span data-stu-id="bb080-429">Service Bus logs retries as ETW events using an **EventSource**.</span></span> <span data-ttu-id="bb080-430">Sie müssen einen **EventListener** an die Ereignisquelle anfügen, um die Ereignisse zu erfassen und in Performance-Viewer anzuzeigen oder diese in ein geeignetes Ziel-Protokoll schreiben.</span><span class="sxs-lookup"><span data-stu-id="bb080-430">You must attach an **EventListener** to the event source to capture the events and view them in Performance Viewer, or write them to a suitable destination log.</span></span> <span data-ttu-id="bb080-431">Dazu können Sie den [Anwendungsblock Semantic Logging](http://msdn.microsoft.com/library/dn775006.aspx) verwenden.</span><span class="sxs-lookup"><span data-stu-id="bb080-431">You could use the [Semantic Logging Application Block](http://msdn.microsoft.com/library/dn775006.aspx) to do this.</span></span> <span data-ttu-id="bb080-432">Die Wiederholungsereignisse sind im folgenden Format:</span><span class="sxs-lookup"><span data-stu-id="bb080-432">The retry events are of the following form:</span></span>

```text
Microsoft-ServiceBus-Client/RetryPolicyIteration
ThreadID="14,500"
FormattedMessage="[TrackingId:] RetryExponential: Operation Get:https://retry-tests.servicebus.windows.net/TestQueue/?api-version=2014-05 at iteration 0 is retrying after 00:00:00.1000000 sleep because of Microsoft.ServiceBus.Messaging.MessagingCommunicationException: The remote name could not be resolved: 'retry-tests.servicebus.windows.net'.TrackingId:6a26f99c-dc6d-422e-8565-f89fdd0d4fe3, TimeStamp:9/5/2014 10:00:13 PM."
trackingId=""
policyType="RetryExponential"
operation="Get:https://retry-tests.servicebus.windows.net/TestQueue/?api-version=2014-05"
iteration="0"
iterationSleep="00:00:00.1000000"
lastExceptionType="Microsoft.ServiceBus.Messaging.MessagingCommunicationException"
exceptionMessage="The remote name could not be resolved: 'retry-tests.servicebus.windows.net'.TrackingId:6a26f99c-dc6d-422e-8565-f89fdd0d4fe3,TimeStamp:9/5/2014 10:00:13 PM"
```

### <a name="examples"></a><span data-ttu-id="bb080-433">Beispiele</span><span class="sxs-lookup"><span data-stu-id="bb080-433">Examples</span></span>
<span data-ttu-id="bb080-434">Das folgende Codebeispiel zeigt, wie Sie die Wiederholungsrichtlinie festlegen für:</span><span class="sxs-lookup"><span data-stu-id="bb080-434">The following code example shows how to set the retry policy for:</span></span>

* <span data-ttu-id="bb080-435">Einen Namespace-Manager.</span><span class="sxs-lookup"><span data-stu-id="bb080-435">A namespace manager.</span></span> <span data-ttu-id="bb080-436">Die Richtlinie gilt für alle Vorgänge dieses Managers und kann nicht durch einzelne Vorgänge überschrieben werden.</span><span class="sxs-lookup"><span data-stu-id="bb080-436">The policy applies to all operations on that manager, and cannot be overridden for individual operations.</span></span>
* <span data-ttu-id="bb080-437">Eine Messaging-Factory.</span><span class="sxs-lookup"><span data-stu-id="bb080-437">A messaging factory.</span></span> <span data-ttu-id="bb080-438">Die Richtlinie gilt für alle Clients, die von dieser Factory erstellt wurden, und kann bei der Erstellung einzelner Clients nicht überschrieben werden.</span><span class="sxs-lookup"><span data-stu-id="bb080-438">The policy applies to all clients created from that factory, and cannot be overridden when creating individual clients.</span></span>
* <span data-ttu-id="bb080-439">Ein einzelner Messaging Client.</span><span class="sxs-lookup"><span data-stu-id="bb080-439">An individual messaging client.</span></span> <span data-ttu-id="bb080-440">Nachdem ein Client erstellt wurde, können Sie die Wiederholungsrichtlinie für diesen festlegen.</span><span class="sxs-lookup"><span data-stu-id="bb080-440">After a client has been created, you can set the retry policy for that client.</span></span> <span data-ttu-id="bb080-441">Die Richtlinie gilt für alle Vorgänge auf dem Client.</span><span class="sxs-lookup"><span data-stu-id="bb080-441">The policy applies to all operations on that client.</span></span>

```csharp
using System;
using System.Threading.Tasks;
using Microsoft.ServiceBus;
using Microsoft.ServiceBus.Messaging;

namespace RetryCodeSamples
{
    class ServiceBusCodeSamples
    {
        private const string connectionString =
            @"Endpoint=sb://[my-namespace].servicebus.windows.net/;
                SharedAccessKeyName=RootManageSharedAccessKey;
                SharedAccessKey=C99..........Mk=";

        public async static Task Samples()
        {
            const string QueueName = "TestQueue";

            ServiceBusEnvironment.SystemConnectivity.Mode = ConnectivityMode.Http;

            var namespaceManager = NamespaceManager.CreateFromConnectionString(connectionString);

            // The namespace manager will have a default exponential policy with 10 retry attempts
            // and a 3 second delay delta.
            // Retry delays will be approximately 0 sec, 3 sec, 9 sec, 25 sec and the fixed 30 sec,
            // with an extra 10 sec added when receiving a ServiceBusyException.

            {
                // Set different values for the retry policy, used for all operations on the namespace manager.
                namespaceManager.Settings.RetryPolicy =
                    new RetryExponential(
                        minBackoff: TimeSpan.FromSeconds(0),
                        maxBackoff: TimeSpan.FromSeconds(30),
                        maxRetryCount: 3);

                // Policies cannot be specified on a per-operation basis.
                if (!await namespaceManager.QueueExistsAsync(QueueName))
                {
                    await namespaceManager.CreateQueueAsync(QueueName);
                }
            }

            var messagingFactory = MessagingFactory.Create(
                namespaceManager.Address, namespaceManager.Settings.TokenProvider);
            // The messaging factory will have a default exponential policy with 10 retry attempts
            // and a 3 second delay delta.
            // Retry delays will be approximately 0 sec, 3 sec, 9 sec, 25 sec and the fixed 30 sec,
            // with an extra 10 sec added when receiving a ServiceBusyException.

            {
                // Set different values for the retry policy, used for clients created from it.
                messagingFactory.RetryPolicy =
                    new RetryExponential(
                        minBackoff: TimeSpan.FromSeconds(1),
                        maxBackoff: TimeSpan.FromSeconds(30),
                        maxRetryCount: 3);


                // Policies cannot be specified on a per-operation basis.
                var session = await messagingFactory.AcceptMessageSessionAsync();
            }

            {
                var client = messagingFactory.CreateQueueClient(QueueName);
                // The client inherits the policy from the factory that created it.


                // Set different values for the retry policy on the client.
                client.RetryPolicy =
                    new RetryExponential(
                        minBackoff: TimeSpan.FromSeconds(0.1),
                        maxBackoff: TimeSpan.FromSeconds(30),
                        maxRetryCount: 3);

                // Policies cannot be specified on a per-operation basis.
                var session = await client.AcceptMessageSessionAsync();
            }
        }
    }
}
```

### <a name="more-information"></a><span data-ttu-id="bb080-442">Weitere Informationen</span><span class="sxs-lookup"><span data-stu-id="bb080-442">More information</span></span>
* [<span data-ttu-id="bb080-443">Asynchrone Nachrichtenmuster und Hochverfügbarkeit</span><span class="sxs-lookup"><span data-stu-id="bb080-443">Asynchronous Messaging Patterns and High Availability</span></span>](http://msdn.microsoft.com/library/azure/dn292562.aspx)

## <a name="service-fabric"></a><span data-ttu-id="bb080-444">Service Fabric</span><span class="sxs-lookup"><span data-stu-id="bb080-444">Service Fabric</span></span>

<span data-ttu-id="bb080-445">Die Verteilung von Reliable Services in einem Service Fabric-Cluster dient als Schutz vor den meisten potenziellen vorübergehenden Fehlern, die in diesem Artikel beschrieben wurden.</span><span class="sxs-lookup"><span data-stu-id="bb080-445">Distributing reliable services in a Service Fabric cluster guards against most of the potential transient faults discussed in this article.</span></span> <span data-ttu-id="bb080-446">Einige vorübergehende Fehler sind aber weiterhin möglich.</span><span class="sxs-lookup"><span data-stu-id="bb080-446">Some transient faults are still possible, however.</span></span> <span data-ttu-id="bb080-447">Für den Naming Service kann beispielsweise gerade eine Routingänderung durchgeführt werden, wenn er eine Anforderung erhält, sodass eine Ausnahme ausgelöst wird.</span><span class="sxs-lookup"><span data-stu-id="bb080-447">For example, the naming service might be in the middle of a routing change when it gets a request, causing it to throw an exception.</span></span> <span data-ttu-id="bb080-448">Wenn dieselbe Anforderung 100 Millisekunden später eintrifft, ist der Vorgang wahrscheinlich erfolgreich.</span><span class="sxs-lookup"><span data-stu-id="bb080-448">If the same request comes 100 milliseconds later, it will probably succeed.</span></span>

<span data-ttu-id="bb080-449">Intern verwaltet Service Fabric diese Art von vorübergehenden Fehlern.</span><span class="sxs-lookup"><span data-stu-id="bb080-449">Internally, Service Fabric manages this kind of transient fault.</span></span> <span data-ttu-id="bb080-450">Sie können einige Einstellungen konfigurieren, indem Sie beim Einrichten Ihrer Dienste die `OperationRetrySettings`-Klasse verwenden.</span><span class="sxs-lookup"><span data-stu-id="bb080-450">You can configure some settings by using the `OperationRetrySettings` class while setting up your services.</span></span>  <span data-ttu-id="bb080-451">Der folgende Code enthält hierzu ein Beispiel.</span><span class="sxs-lookup"><span data-stu-id="bb080-451">The following code shows an example.</span></span> <span data-ttu-id="bb080-452">In den meisten Fällen sollte dies nicht erforderlich sein, sodass die Standardeinstellungen verwendet werden können.</span><span class="sxs-lookup"><span data-stu-id="bb080-452">In most cases, this should not be necessary, and the default settings will be fine.</span></span>

```csharp
FabricTransportRemotingSettings transportSettings = new FabricTransportRemotingSettings
{
    OperationTimeout = TimeSpan.FromSeconds(30)
};

var retrySettings = new OperationRetrySettings(TimeSpan.FromSeconds(15), TimeSpan.FromSeconds(1), 5);

var clientFactory = new FabricTransportServiceRemotingClientFactory(transportSettings);

var serviceProxyFactory = new ServiceProxyFactory((c) => clientFactory, retrySettings);

var client = serviceProxyFactory.CreateServiceProxy<ISomeService>(
    new Uri("fabric:/SomeApp/SomeStatefulReliableService"),
    new ServicePartitionKey(0));
```

### <a name="more-information"></a><span data-ttu-id="bb080-453">Weitere Informationen</span><span class="sxs-lookup"><span data-stu-id="bb080-453">More information</span></span>

* <span data-ttu-id="bb080-454">[Remote Exception Handling](https://github.com/Microsoft/azure-docs/blob/master/articles/service-fabric/service-fabric-reliable-services-communication-remoting.md#remoting-exception-handling) (Remotebehandlung von Ausnahmen)</span><span class="sxs-lookup"><span data-stu-id="bb080-454">[Remote Exception Handling](https://github.com/Microsoft/azure-docs/blob/master/articles/service-fabric/service-fabric-reliable-services-communication-remoting.md#remoting-exception-handling)</span></span>

## <a name="sql-database-using-adonet"></a><span data-ttu-id="bb080-455">SQL-Datenbank mit ADO.NET</span><span class="sxs-lookup"><span data-stu-id="bb080-455">SQL Database using ADO.NET</span></span>
<span data-ttu-id="bb080-456">SQL-Datenbank ist eine gehostete SQL-Datenbank, die in unterschiedlichen Größen und als Standard (freigegeben) und Premium (nicht freigegebenen)-Dienst verfügbar ist.</span><span class="sxs-lookup"><span data-stu-id="bb080-456">SQL Database is a hosted SQL database available in a range of sizes and as both a standard (shared) and premium (non-shared) service.</span></span>

### <a name="retry-mechanism"></a><span data-ttu-id="bb080-457">Wiederholungsmechanismus</span><span class="sxs-lookup"><span data-stu-id="bb080-457">Retry mechanism</span></span>
<span data-ttu-id="bb080-458">SQL-Datenbank hat keine integrierte Unterstützung für Wiederholungen, wenn auf diese mit ADO.NET zugegriffen wird.</span><span class="sxs-lookup"><span data-stu-id="bb080-458">SQL Database has no built-in support for retries when accessed using ADO.NET.</span></span> <span data-ttu-id="bb080-459">Allerdings können die Rückgabecodes von Anforderungen verwendet werden, um zu bestimmen, warum eine Anforderung fehlgeschlagen ist.</span><span class="sxs-lookup"><span data-stu-id="bb080-459">However, the return codes from requests can be used to determine why a request failed.</span></span> <span data-ttu-id="bb080-460">Weitere Informationen zur SQL-Datenbank-Drosselung finden Sie unter [Ressourceneinschränkungen für Azure SQL-Datenbank](/azure/sql-database/sql-database-resource-limits).</span><span class="sxs-lookup"><span data-stu-id="bb080-460">For more information about SQL Database throttling, see [Azure SQL Database resource limits](/azure/sql-database/sql-database-resource-limits).</span></span> <span data-ttu-id="bb080-461">Eine Liste mit relevanten Fehlercodes finden Sie unter [SQL-Fehlercodes für SQL-Datenbank-Clientanwendungen](/azure/sql-database/sql-database-develop-error-messages).</span><span class="sxs-lookup"><span data-stu-id="bb080-461">For a list of relevant error codes, see [SQL error codes for SQL Database client applications](/azure/sql-database/sql-database-develop-error-messages).</span></span>

<span data-ttu-id="bb080-462">Sie können die Polly-Bibliothek verwenden, um Wiederholungen für SQL-Datenbank zu implementieren.</span><span class="sxs-lookup"><span data-stu-id="bb080-462">You can use the Polly library to implement retries for SQL Database.</span></span> <span data-ttu-id="bb080-463">Weitere Informationen finden Sie unter [Behandeln von vorübergehenden Fehlern mit Polly](#transient-fault-handling-with-polly).</span><span class="sxs-lookup"><span data-stu-id="bb080-463">See [Transient fault handling with Polly](#transient-fault-handling-with-polly).</span></span>

### <a name="retry-usage-guidance"></a><span data-ttu-id="bb080-464">Gebrauchsanleitung Wiederholungen</span><span class="sxs-lookup"><span data-stu-id="bb080-464">Retry usage guidance</span></span>
<span data-ttu-id="bb080-465">Beachten Sie den Zugriff auf SQL-Datenbank mit ADO.NET die folgenden Richtlinien:</span><span class="sxs-lookup"><span data-stu-id="bb080-465">Consider the following guidelines when accessing SQL Database using ADO.NET:</span></span>

* <span data-ttu-id="bb080-466">Wählen Sie die entsprechende Dienstoption (freigegeben oder Premium).</span><span class="sxs-lookup"><span data-stu-id="bb080-466">Choose the appropriate service option (shared or premium).</span></span> <span data-ttu-id="bb080-467">Bei einer freigegebenen Instanz kann es möglicherweise zu längeren Verbindungsverzögerungen als üblich und Drosselung durch die Verwendung des gemeinsam genutzten Servers durch andere Mandanten kommen.</span><span class="sxs-lookup"><span data-stu-id="bb080-467">A shared instance may suffer longer than usual connection delays and throttling due to the usage by other tenants of the shared server.</span></span> <span data-ttu-id="bb080-468">Wenn Vorgänge mit vorhersagbarerer Leistung und zuverlässig geringe Latenz erforderlich sind, sollten Sie die Premium-Option wählen.</span><span class="sxs-lookup"><span data-stu-id="bb080-468">If more predictable performance and reliable low latency operations are required, consider choosing the premium option.</span></span>
* <span data-ttu-id="bb080-469">Stellen Sie sicher, dass Sie Wiederholungen auf der entsprechenden Ebene oder im entsprechenden Bereich durchführen, um nicht-idempotente Operationen zu vermeiden, die Dateninkonsistenzen verursachen.</span><span class="sxs-lookup"><span data-stu-id="bb080-469">Ensure that you perform retries at the appropriate level or scope to avoid non-idempotent operations causing inconsistency in the data.</span></span> <span data-ttu-id="bb080-470">Im Idealfall sollten alle Vorgänge idempotent sein, sodass sie ohne Inkonsistenzen wiederholt werden können.</span><span class="sxs-lookup"><span data-stu-id="bb080-470">Ideally, all operations should be idempotent so that they can be repeated without causing inconsistency.</span></span> <span data-ttu-id="bb080-471">Wenn dies nicht der Fall ist, sollte die Wiederholung auf einer Ebene oder in einem Bereich durchgeführt werden, die ermöglichen, dass alle zugehörigen Änderungen rückgängig gemacht werden können, wenn ein Vorgang fehlschlägt; zum Beispiel im Transaktionsbereich.</span><span class="sxs-lookup"><span data-stu-id="bb080-471">Where this is not the case, the retry should be performed at a level or scope that allows all related changes to be undone if one operation fails; for example, from within a transactional scope.</span></span> <span data-ttu-id="bb080-472">Weitere Informationen finden Sie unter [Clouddienst-Grundlagen Datenzugriffsebene - Handhabung vorübergehender Fehler](http://social.technet.microsoft.com/wiki/contents/articles/18665.cloud-service-fundamentals-data-access-layer-transient-fault-handling.aspx#Idempotent_Guarantee).</span><span class="sxs-lookup"><span data-stu-id="bb080-472">For more information, see [Cloud Service Fundamentals Data Access Layer – Transient Fault Handling](http://social.technet.microsoft.com/wiki/contents/articles/18665.cloud-service-fundamentals-data-access-layer-transient-fault-handling.aspx#Idempotent_Guarantee).</span></span>
* <span data-ttu-id="bb080-473">Eine Strategie mit festgelegtem Intervall wird für die Verwendung mit Azure SQL-Datenbank nicht empfohlen, mit Ausnahme von interaktiven Szenarien, wo nur wenige Wiederholungen in sehr kurzen Intervallen vorkommen.</span><span class="sxs-lookup"><span data-stu-id="bb080-473">A fixed interval strategy is not recommended for use with Azure SQL Database except for interactive scenarios where there are only a few retries at very short intervals.</span></span> <span data-ttu-id="bb080-474">Ziehen Sie stattdessen eine exponentielle Backoff-Strategie für die meisten Szenarien in Betracht.</span><span class="sxs-lookup"><span data-stu-id="bb080-474">Instead, consider using an exponential back-off strategy for the majority of scenarios.</span></span>
* <span data-ttu-id="bb080-475">Wählen Sie einen geeigneten Wert für die Verbindungs- und Befehls-Timeouts bei der Definition von Verbindungen.</span><span class="sxs-lookup"><span data-stu-id="bb080-475">Choose a suitable value for the connection and command timeouts when defining connections.</span></span> <span data-ttu-id="bb080-476">Ein zu kurzes Timeout kann zu vorzeitigen Fehlern bei Verbindungen führen, wenn die Datenbank ausgelastet ist.</span><span class="sxs-lookup"><span data-stu-id="bb080-476">Too short a timeout may result in premature failures of connections when the database is busy.</span></span> <span data-ttu-id="bb080-477">Ein zu langes Timeout kann verhindern, dass die Wiederholungslogik ordnungsgemäß funktioniert, da sie vor der Erkennung einer fehlgeschlagenen Verbindung zu lange wartet.</span><span class="sxs-lookup"><span data-stu-id="bb080-477">Too long a timeout may prevent the retry logic working correctly by waiting too long before detecting a failed connection.</span></span> <span data-ttu-id="bb080-478">Der Wert für das Timeout ist eine Komponente der End-to-End-Latenz. Sie wird effektiv der Wiederholungsverzögerung hinzugefügt, die in der Wiederholungsrichtlinie für jeden Wiederholungsversuch festgelegt ist.</span><span class="sxs-lookup"><span data-stu-id="bb080-478">The value of the timeout is a component of the end-to-end latency; it is effectively added to the retry delay specified in the retry policy for every retry attempt.</span></span>
* <span data-ttu-id="bb080-479">Schließen Sie die Verbindung nach einer bestimmten Anzahl von Wiederholungen, selbst wenn Sie eine exponentielle Backoff-Wiederholungslogik verwenden, und wiederholen Sie den Vorgang auf einer neuen Verbindung.</span><span class="sxs-lookup"><span data-stu-id="bb080-479">Close the connection after a certain number of retries, even when using an exponential back off retry logic, and retry the operation on a new connection.</span></span> <span data-ttu-id="bb080-480">Das mehrmalige Wiederholen des gleichen Vorgangs für dieselbe Verbindung kann ein Faktor sein, der zu Verbindungsproblemen beiträgt.</span><span class="sxs-lookup"><span data-stu-id="bb080-480">Retrying the same operation multiple times on the same connection can be a factor that contributes to connection problems.</span></span> <span data-ttu-id="bb080-481">Beispiele für dieses Verfahrens finden Sie unter [Clouddienst-Grundlagen Datenzugriffsebene - Handhabung vorübergehender Fehler](http://social.technet.microsoft.com/wiki/contents/articles/18665.cloud-service-fundamentals-data-access-layer-transient-fault-handling.aspx).</span><span class="sxs-lookup"><span data-stu-id="bb080-481">For an example of this technique, see [Cloud Service Fundamentals Data Access Layer – Transient Fault Handling](http://social.technet.microsoft.com/wiki/contents/articles/18665.cloud-service-fundamentals-data-access-layer-transient-fault-handling.aspx).</span></span>
* <span data-ttu-id="bb080-482">Wenn Verbindungspooling verwendet wird (Standardeinstellung), besteht die Möglichkeit, dass auch nach dem Schließen und erneuten Öffnen einer Verbindung dieselbe Verbindung aus dem Pool ausgewählt wird.</span><span class="sxs-lookup"><span data-stu-id="bb080-482">When connection pooling is in use (the default) there is a chance that the same connection will be chosen from the pool, even after closing and reopening a connection.</span></span> <span data-ttu-id="bb080-483">Wenn dies der Fall ist, ist ein Verfahren zu Behebung das Aufrufen der **ClearPool**-Methode der **SqlConnection**-Klasse, um die Verbindung als nicht wiederverwendbar zu markieren.</span><span class="sxs-lookup"><span data-stu-id="bb080-483">If this is the case, a technique to resolve it is to call the **ClearPool** method of the **SqlConnection** class to mark the connection as not reusable.</span></span> <span data-ttu-id="bb080-484">Allerdings sollten Sie dies erst nach mehreren fehlgeschlagenen Verbindungsversuchen tun und nur, wenn die spezifische Klasse des vorübergehende Fehler wie z. B. SQL-Timeouts (Fehlercode: -2) im Zusammenhang mit fehlerhaften Verbindungen auftritt.</span><span class="sxs-lookup"><span data-stu-id="bb080-484">However, you should do this only after several connection attempts have failed, and only when encountering the specific class of transient failures such as SQL timeouts (error code -2) related to faulty connections.</span></span>
* <span data-ttu-id="bb080-485">Wenn der Datenzugriffscode als **TransactionScope** -Instanzen initiierte Transaktionen verwendet, sollte die Wiederholungslogik erneut eine Verbindung herstellen und einen neuen Transaktionsbereich initiieren.</span><span class="sxs-lookup"><span data-stu-id="bb080-485">If the data access code uses transactions initiated as **TransactionScope** instances, the retry logic should reopen the connection and initiate a new transaction scope.</span></span> <span data-ttu-id="bb080-486">Aus diesem Grund sollte der wiederholbare Codeblock den gesamten Bereich der Transaktion umfassen.</span><span class="sxs-lookup"><span data-stu-id="bb080-486">For this reason, the retryable code block should encompass the entire scope of the transaction.</span></span>

<span data-ttu-id="bb080-487">Erwägen Sie, mit den folgenden Einstellungen für Wiederholungsvorgänge zu beginnen.</span><span class="sxs-lookup"><span data-stu-id="bb080-487">Consider starting with following settings for retrying operations.</span></span> <span data-ttu-id="bb080-488">Hierbei handelt es sich um allgemeine Einstellungen und Sie sollten die Vorgänge überwachen und die Werte entsprechend Ihrem Szenario optimieren.</span><span class="sxs-lookup"><span data-stu-id="bb080-488">These are general purpose settings, and you should monitor the operations and fine tune the values to suit your own scenario.</span></span>

| <span data-ttu-id="bb080-489">**Context**</span><span class="sxs-lookup"><span data-stu-id="bb080-489">**Context**</span></span> | <span data-ttu-id="bb080-490">**Beispiel-Ziel E2E<br />Maximale Wartezeit**</span><span class="sxs-lookup"><span data-stu-id="bb080-490">**Sample target E2E<br />max latency**</span></span> | <span data-ttu-id="bb080-491">**Wiederholungsstrategie**</span><span class="sxs-lookup"><span data-stu-id="bb080-491">**Retry strategy**</span></span> | <span data-ttu-id="bb080-492">**Einstellungen**</span><span class="sxs-lookup"><span data-stu-id="bb080-492">**Settings**</span></span> | <span data-ttu-id="bb080-493">**Werte**</span><span class="sxs-lookup"><span data-stu-id="bb080-493">**Values**</span></span> | <span data-ttu-id="bb080-494">**So funktioniert's**</span><span class="sxs-lookup"><span data-stu-id="bb080-494">**How it works**</span></span> |
| --- | --- | --- | --- | --- | --- |
| <span data-ttu-id="bb080-495">Interaktiv, Benutzeroberfläche</span><span class="sxs-lookup"><span data-stu-id="bb080-495">Interactive, UI,</span></span><br /><span data-ttu-id="bb080-496">oder Vordergrund</span><span class="sxs-lookup"><span data-stu-id="bb080-496">or foreground</span></span> |<span data-ttu-id="bb080-497">2 Sek</span><span class="sxs-lookup"><span data-stu-id="bb080-497">2 sec</span></span> |<span data-ttu-id="bb080-498">FixedInterval</span><span class="sxs-lookup"><span data-stu-id="bb080-498">FixedInterval</span></span> |<span data-ttu-id="bb080-499">Anzahl der Wiederholungen</span><span class="sxs-lookup"><span data-stu-id="bb080-499">Retry count</span></span><br /><span data-ttu-id="bb080-500">Wiederholungsintervall</span><span class="sxs-lookup"><span data-stu-id="bb080-500">Retry interval</span></span><br /><span data-ttu-id="bb080-501">Erster schneller Wiederholungsversuch</span><span class="sxs-lookup"><span data-stu-id="bb080-501">First fast retry</span></span> |<span data-ttu-id="bb080-502">3</span><span class="sxs-lookup"><span data-stu-id="bb080-502">3</span></span><br /><span data-ttu-id="bb080-503">500 ms</span><span class="sxs-lookup"><span data-stu-id="bb080-503">500 ms</span></span><br /><span data-ttu-id="bb080-504">true</span><span class="sxs-lookup"><span data-stu-id="bb080-504">true</span></span> |<span data-ttu-id="bb080-505">Versuch 1 – Verzögerung 0 Sek.</span><span class="sxs-lookup"><span data-stu-id="bb080-505">Attempt 1 - delay 0 sec</span></span><br /><span data-ttu-id="bb080-506">Versuch 2 – Verzögerung 500 ms</span><span class="sxs-lookup"><span data-stu-id="bb080-506">Attempt 2 - delay 500 ms</span></span><br /><span data-ttu-id="bb080-507">Versuch 3 – Verzögerung 500 ms</span><span class="sxs-lookup"><span data-stu-id="bb080-507">Attempt 3 - delay 500 ms</span></span> |
| <span data-ttu-id="bb080-508">Hintergrund</span><span class="sxs-lookup"><span data-stu-id="bb080-508">Background</span></span><br /><span data-ttu-id="bb080-509">oder Batch</span><span class="sxs-lookup"><span data-stu-id="bb080-509">or batch</span></span> |<span data-ttu-id="bb080-510">30 Sek</span><span class="sxs-lookup"><span data-stu-id="bb080-510">30 sec</span></span> |<span data-ttu-id="bb080-511">ExponentialBackoff</span><span class="sxs-lookup"><span data-stu-id="bb080-511">ExponentialBackoff</span></span> |<span data-ttu-id="bb080-512">Anzahl der Wiederholungen</span><span class="sxs-lookup"><span data-stu-id="bb080-512">Retry count</span></span><br /><span data-ttu-id="bb080-513">Min. Backoff</span><span class="sxs-lookup"><span data-stu-id="bb080-513">Min back-off</span></span><br /><span data-ttu-id="bb080-514">Max. Backoff</span><span class="sxs-lookup"><span data-stu-id="bb080-514">Max back-off</span></span><br /><span data-ttu-id="bb080-515">Delta-Backoff</span><span class="sxs-lookup"><span data-stu-id="bb080-515">Delta back-off</span></span><br /><span data-ttu-id="bb080-516">Erster schneller Wiederholungsversuch</span><span class="sxs-lookup"><span data-stu-id="bb080-516">First fast retry</span></span> |<span data-ttu-id="bb080-517">5</span><span class="sxs-lookup"><span data-stu-id="bb080-517">5</span></span><br /><span data-ttu-id="bb080-518">0 Sek.</span><span class="sxs-lookup"><span data-stu-id="bb080-518">0 sec</span></span><br /><span data-ttu-id="bb080-519">60 Sekunden</span><span class="sxs-lookup"><span data-stu-id="bb080-519">60 sec</span></span><br /><span data-ttu-id="bb080-520">2 Sek</span><span class="sxs-lookup"><span data-stu-id="bb080-520">2 sec</span></span><br /><span data-ttu-id="bb080-521">false</span><span class="sxs-lookup"><span data-stu-id="bb080-521">false</span></span> |<span data-ttu-id="bb080-522">Versuch 1 – Verzögerung 0 Sek.</span><span class="sxs-lookup"><span data-stu-id="bb080-522">Attempt 1 - delay 0 sec</span></span><br /><span data-ttu-id="bb080-523">Versuch 2 – Verzögerung ca. 2 Sek.</span><span class="sxs-lookup"><span data-stu-id="bb080-523">Attempt 2 - delay ~2 sec</span></span><br /><span data-ttu-id="bb080-524">Versuch 3 – Verzögerung ca. 6 Sek.</span><span class="sxs-lookup"><span data-stu-id="bb080-524">Attempt 3 - delay ~6 sec</span></span><br /><span data-ttu-id="bb080-525">Versuch 4 – Verzögerung ca. 14 Sek.</span><span class="sxs-lookup"><span data-stu-id="bb080-525">Attempt 4 - delay ~14 sec</span></span><br /><span data-ttu-id="bb080-526">Versuch 5 – Verzögerung ca. 30 Sek.</span><span class="sxs-lookup"><span data-stu-id="bb080-526">Attempt 5 - delay ~30 sec</span></span> |

> [!NOTE]
> <span data-ttu-id="bb080-527">Die End-to-End-Latenzziele setzen das Standardtimeout für Verbindungen mit dem Dienst voraus.</span><span class="sxs-lookup"><span data-stu-id="bb080-527">The end-to-end latency targets assume the default timeout for connections to the service.</span></span> <span data-ttu-id="bb080-528">Wenn Sie längere Verbindungstimeouts angeben, wird die End-to-End-Latenz durch diese zusätzliche Zeit für jeden Wiederholungsversuch erweitert.</span><span class="sxs-lookup"><span data-stu-id="bb080-528">If you specify longer connection timeouts, the end-to-end latency will be extended by this additional time for every retry attempt.</span></span>
>
>

### <a name="examples"></a><span data-ttu-id="bb080-529">Beispiele</span><span class="sxs-lookup"><span data-stu-id="bb080-529">Examples</span></span>
<span data-ttu-id="bb080-530">In diesem Abschnitt wird veranschaulicht, wie Sie Polly zum Zugreifen auf Azure SQL-Datenbank verwenden können, indem Sie eine Reihe von Wiederholungsrichtlinien verwenden, die in der `Policy`-Klasse konfiguriert sind.</span><span class="sxs-lookup"><span data-stu-id="bb080-530">This section shows how you can use Polly to access Azure SQL Database using a set of retry policies configured in the `Policy` class.</span></span>

<span data-ttu-id="bb080-531">Der folgende Code enthält eine Erweiterungsmethode für die `SqlCommand`-Klasse, mit der `ExecuteAsync` mit exponentiell ansteigender Wartezeit aufgerufen wird.</span><span class="sxs-lookup"><span data-stu-id="bb080-531">The following code shows an extension method on the `SqlCommand` class that calls `ExecuteAsync` with exponential backoff.</span></span>

```csharp
public async static Task<SqlDataReader> ExecuteReaderWithRetryAsync(this SqlCommand command)
{
    GuardConnectionIsNotNull(command);

    var policy = Policy.Handle<Exception>().WaitAndRetryAsync(
        retryCount: 3, // Retry 3 times
        sleepDurationProvider: attempt => TimeSpan.FromMilliseconds(200 * Math.Pow(2, attempt - 1)), // Exponential backoff based on an initial 200ms delay.
        onRetry: (exception, attempt) => 
        {
            // Capture some info for logging/telemetry.  
            logger.LogWarn($"ExecuteReaderWithRetryAsync: Retry {attempt} due to {exception}.");
        });

    // Retry the following call according to the policy.
    await policy.ExecuteAsync<SqlDataReader>(async token =>
    {
        // This code is executed within the Policy 

        if (conn.State != System.Data.ConnectionState.Open) await conn.OpenAsync(token);
        return await command.ExecuteReaderAsync(System.Data.CommandBehavior.Default, token);

    }, cancellationToken);
}
```

<span data-ttu-id="bb080-532">Diese asynchrone Erweiterungsmethode kann wie unten beschrieben verwendet werden.</span><span class="sxs-lookup"><span data-stu-id="bb080-532">This asynchronous extension method can be used as follows.</span></span>

```csharp
var sqlCommand = sqlConnection.CreateCommand();
sqlCommand.CommandText = "[some query]";

using (var reader = await sqlCommand.ExecuteReaderWithRetryAsync())
{
    // Do something with the values
}
```

### <a name="more-information"></a><span data-ttu-id="bb080-533">Weitere Informationen</span><span class="sxs-lookup"><span data-stu-id="bb080-533">More information</span></span>
* [<span data-ttu-id="bb080-534">Clouddienst-Grundlagen Datenzugriffsebene - Handhabung vorübergehender Fehler.</span><span class="sxs-lookup"><span data-stu-id="bb080-534">Cloud Service Fundamentals Data Access Layer – Transient Fault Handling</span></span>](http://social.technet.microsoft.com/wiki/contents/articles/18665.cloud-service-fundamentals-data-access-layer-transient-fault-handling.aspx)

<span data-ttu-id="bb080-535">Eine allgemeine Anleitung dazu, wie Sie für SQL-Datenbank den größtmöglichen Nutzen erzielen, finden Sie unter [Anleitung zu Leistung und Flexibilität von Azure SQL-Datenbanken](http://social.technet.microsoft.com/wiki/contents/articles/3507.windows-azure-sql-database-performance-and-elasticity-guide.aspx).</span><span class="sxs-lookup"><span data-stu-id="bb080-535">For general guidance on getting the most from SQL Database, see [Azure SQL Database Performance and Elasticity Guide](http://social.technet.microsoft.com/wiki/contents/articles/3507.windows-azure-sql-database-performance-and-elasticity-guide.aspx).</span></span>

## <a name="sql-database-using-entity-framework-6"></a><span data-ttu-id="bb080-536">SQL-Datenbank mit Entity Framework 6</span><span class="sxs-lookup"><span data-stu-id="bb080-536">SQL Database using Entity Framework 6</span></span>
<span data-ttu-id="bb080-537">SQL-Datenbank ist eine gehostete SQL-Datenbank, die in unterschiedlichen Größen und als Standard (freigegeben) und Premium (nicht freigegebenen)-Dienst verfügbar ist.</span><span class="sxs-lookup"><span data-stu-id="bb080-537">SQL Database is a hosted SQL database available in a range of sizes and as both a standard (shared) and premium (non-shared) service.</span></span> <span data-ttu-id="bb080-538">Entity Framework ist eine objektrelationale Zuordnung, die .NET Entwicklern die  Arbeit mit relationalen Daten mithilfe von domänenspezifischen Objekten ermöglicht.</span><span class="sxs-lookup"><span data-stu-id="bb080-538">Entity Framework is an object-relational mapper that enables .NET developers to work with relational data using domain-specific objects.</span></span> <span data-ttu-id="bb080-539">Es entfällt die Notwendigkeit für den Großteil des Datenzugriffs-Codes, den Entwickler normalerweise schreiben müssen.</span><span class="sxs-lookup"><span data-stu-id="bb080-539">It eliminates the need for most of the data-access code that developers usually need to write.</span></span>

### <a name="retry-mechanism"></a><span data-ttu-id="bb080-540">Wiederholungsmechanismus</span><span class="sxs-lookup"><span data-stu-id="bb080-540">Retry mechanism</span></span>
<span data-ttu-id="bb080-541">Beim Zugriff auf SQL-Datenbank mit Entity Framework 6.0 und höher über einen Mechanismus namens [Verbindungsstabilität / Wiederholungslogik](http://msdn.microsoft.com/data/dn456835.aspx)wird Wiederholungsunterstützung geboten.</span><span class="sxs-lookup"><span data-stu-id="bb080-541">Retry support is provided when accessing SQL Database using Entity Framework 6.0 and higher through a mechanism called [Connection Resiliency / Retry Logic](http://msdn.microsoft.com/data/dn456835.aspx).</span></span> <span data-ttu-id="bb080-542">Die wichtigsten Features des Wiederholungsmechanismus sind:</span><span class="sxs-lookup"><span data-stu-id="bb080-542">The main features of the retry mechanism are:</span></span>

* <span data-ttu-id="bb080-543">Die primäre Abstraktion ist die **IDbExecutionStrategy** -Schnittstelle.</span><span class="sxs-lookup"><span data-stu-id="bb080-543">The primary abstraction is the **IDbExecutionStrategy** interface.</span></span> <span data-ttu-id="bb080-544">Diese Benutzeroberfläche:</span><span class="sxs-lookup"><span data-stu-id="bb080-544">This interface:</span></span>
  * <span data-ttu-id="bb080-545">Definiert die synchrone und asynchrone Methoden zum **Ausführen**\*.</span><span class="sxs-lookup"><span data-stu-id="bb080-545">Defines synchronous and asynchronous **Execute**\* methods.</span></span>
  * <span data-ttu-id="bb080-546">Definiert Klassen, die direkt verwendet oder in einem Datenbankkontext als eine Standardstrategie konfiguriert werden können, einem Anbieternamen oder einem Anbieternamen und Servernamen zugeordnet werden können.</span><span class="sxs-lookup"><span data-stu-id="bb080-546">Defines classes that can be used directly or can be configured on a database context as a default strategy, mapped to provider name, or mapped to a provider name and server name.</span></span> <span data-ttu-id="bb080-547">Wenn sie für einen Kontext konfiguriert ist, treten Wiederholungen auf der Ebene der einzelnen Datenbankvorgängen auf, von denen es möglicherweise mehrere für einen angegebenen Kontext gibt.</span><span class="sxs-lookup"><span data-stu-id="bb080-547">When configured on a context, retries occur at the level of individual database operations, of which there might be several for a given context operation.</span></span>
  * <span data-ttu-id="bb080-548">Definiert, wann und wie versucht wird, einen fehlgeschlagene Verbindung erneut herzustellen.</span><span class="sxs-lookup"><span data-stu-id="bb080-548">Defines when to retry a failed connection, and how.</span></span>
* <span data-ttu-id="bb080-549">Sie umfasst mehrere integrierte Implementierungen für die **IDbExecutionStrategy** -Schnittstelle:</span><span class="sxs-lookup"><span data-stu-id="bb080-549">It includes several built-in implementations of the **IDbExecutionStrategy** interface:</span></span>
  * <span data-ttu-id="bb080-550">Standard - keine Wiederholung.</span><span class="sxs-lookup"><span data-stu-id="bb080-550">Default - no retrying.</span></span>
  * <span data-ttu-id="bb080-551">Standard für SQL-Datenbank (automatisch) - kein erneuter Versuch, es werden jedoch Ausnahmen überprüft und in der SQL-Datenbank-Strategie mit der Empfehlung zur Verwendung eingebunden.</span><span class="sxs-lookup"><span data-stu-id="bb080-551">Default for SQL Database (automatic) - no retrying, but inspects exceptions and wraps them with suggestion to use the SQL Database strategy.</span></span>
  * <span data-ttu-id="bb080-552">Standardeinstellung für SQL-Datenbank - exponentiell (von der Basisklasse geerbt) plus SQL-Datenbank Erkennungslogik.</span><span class="sxs-lookup"><span data-stu-id="bb080-552">Default for SQL Database - exponential (inherited from base class) plus SQL Database detection logic.</span></span>
* <span data-ttu-id="bb080-553">Sie implementiert eine exponentielle Backoff-Strategie, die einen Zufallsgenerator enthält.</span><span class="sxs-lookup"><span data-stu-id="bb080-553">It implements an exponential back-off strategy that includes randomization.</span></span>
* <span data-ttu-id="bb080-554">Die integrierten Wiederholungsklassen sind zustandsbehaftet und nicht threadsicher.</span><span class="sxs-lookup"><span data-stu-id="bb080-554">The built-in retry classes are stateful and are not thread safe.</span></span> <span data-ttu-id="bb080-555">Sie können jedoch wiederverwendet werden, nachdem der aktuelle Vorgang abgeschlossen ist.</span><span class="sxs-lookup"><span data-stu-id="bb080-555">However, they can be reused after the current operation is completed.</span></span>
* <span data-ttu-id="bb080-556">Wenn die angegebene Wiederholungsanzahl überschritten wird, werden die Ergebnisse in einer neue Ausnahme umschlossen.</span><span class="sxs-lookup"><span data-stu-id="bb080-556">If the specified retry count is exceeded, the results are wrapped in a new exception.</span></span> <span data-ttu-id="bb080-557">Sie bringt die aktuelle Ausnahme nicht zum Vorschein.</span><span class="sxs-lookup"><span data-stu-id="bb080-557">It does not bubble up the current exception.</span></span>

### <a name="policy-configuration"></a><span data-ttu-id="bb080-558">Richtlinienkonfiguration</span><span class="sxs-lookup"><span data-stu-id="bb080-558">Policy configuration</span></span>
<span data-ttu-id="bb080-559">Beim Zugriff auf SQL-Datenbank mit Entity Framework 6.0 und höher wird Wiederholungsunterstützung  geboten.</span><span class="sxs-lookup"><span data-stu-id="bb080-559">Retry support is provided when accessing SQL Database using Entity Framework 6.0 and higher.</span></span> <span data-ttu-id="bb080-560">Wiederholungsrichtlinien werden programmgesteuert konfiguriert.</span><span class="sxs-lookup"><span data-stu-id="bb080-560">Retry policies are configured programmatically.</span></span> <span data-ttu-id="bb080-561">Die Konfiguration kann nicht pro Vorgang geändert werden.</span><span class="sxs-lookup"><span data-stu-id="bb080-561">The configuration cannot be changed on a per-operation basis.</span></span>

<span data-ttu-id="bb080-562">Wenn Sie eine Strategie im Kontext als Standard konfigurieren, geben Sie eine Funktion an, die bei Bedarf eine neue Strategie erstellt.</span><span class="sxs-lookup"><span data-stu-id="bb080-562">When configuring a strategy on the context as the default, you specify a function that creates a new strategy on demand.</span></span> <span data-ttu-id="bb080-563">Der folgende Code zeigt, wie Sie eine Wiederholungskonfigurationsklasse erstellen können, die die **DbConfiguration** -Basisklasse erweitert.</span><span class="sxs-lookup"><span data-stu-id="bb080-563">The following code shows how you can create a retry configuration class that extends the **DbConfiguration** base class.</span></span>

```csharp
public class BloggingContextConfiguration : DbConfiguration
{
  public BlogConfiguration()
  {
    // Set up the execution strategy for SQL Database (exponential) with 5 retries and 4 sec delay
    this.SetExecutionStrategy(
         "System.Data.SqlClient", () => new SqlAzureExecutionStrategy(5, TimeSpan.FromSeconds(4)));
  }
}
```

<span data-ttu-id="bb080-564">Sie können diese dann mit der **SetConfiguration**-Methode der **DbConfiguration**-Instanz als Standardwiederholungsstrategie für alle Vorgänge festlegen, wenn die Anwendung gestartet wird.</span><span class="sxs-lookup"><span data-stu-id="bb080-564">You can then specify this as the default retry strategy for all operations using the **SetConfiguration** method of the **DbConfiguration** instance when the application starts.</span></span> <span data-ttu-id="bb080-565">In der Standardeinstellung erkennt EF die Konfigurationsklasse automatisch und verwenden diese.</span><span class="sxs-lookup"><span data-stu-id="bb080-565">By default, EF will automatically discover and use the configuration class.</span></span>

```csharp
DbConfiguration.SetConfiguration(new BloggingContextConfiguration());
```

<span data-ttu-id="bb080-566">Sie können die Wiederholungskonfigurationsklasse für einen Kontext angeben, indem Sie die Kontextklasse mit einem **DbConfigurationType** -Attribut kommentieren.</span><span class="sxs-lookup"><span data-stu-id="bb080-566">You can specify the retry configuration class for a context by annotating the context class with a **DbConfigurationType** attribute.</span></span> <span data-ttu-id="bb080-567">Wenn Sie jedoch über nur eine Konfigurationsklasse verfügen, wird EF diese verwenden, ohne dass der Kontext kommentieren werden muss.</span><span class="sxs-lookup"><span data-stu-id="bb080-567">However, if you have only one configuration class, EF will use it without the need to annotate the context.</span></span>

```csharp
[DbConfigurationType(typeof(BloggingContextConfiguration))]
public class BloggingContext : DbContext
```

<span data-ttu-id="bb080-568">Wenn Sie unterschiedliche Wiederholungsstrategien für bestimmte Vorgänge verwenden oder Wiederholungen für bestimmte Vorgänge deaktivieren müssen, können Sie eine Konfigurationsklasse erstellen, mit der Sie Strategien anhalten oder  austauschen können, indem ein Flag in **CallContext**setzen.</span><span class="sxs-lookup"><span data-stu-id="bb080-568">If you need to use different retry strategies for specific operations, or disable retries for specific operations, you can create a configuration class that allows you to suspend or swap strategies by setting a flag in the **CallContext**.</span></span> <span data-ttu-id="bb080-569">Die Konfigurationsklasse kann dieses Flag verwenden, um Strategien zu wechseln oder die Strategie zu deaktivieren, die Sie bereitstellen und eine Standardstrategie verwenden.</span><span class="sxs-lookup"><span data-stu-id="bb080-569">The configuration class can use this flag to switch strategies, or disable the strategy you provide and use a default strategy.</span></span> <span data-ttu-id="bb080-570">Weitere Informationen finden Sie unter [Ausführungsstrategie anhalten](http://msdn.microsoft.com/dn307226#transactions_workarounds) auf der Seite Ausführungsstrategien wiederholen (EF6 oder höher).</span><span class="sxs-lookup"><span data-stu-id="bb080-570">For more information, see [Suspend Execution Strategy](http://msdn.microsoft.com/dn307226#transactions_workarounds) in the page Limitations with Retrying Execution Strategies (EF6 onwards).</span></span>

<span data-ttu-id="bb080-571">Eine andere Technik für die Verwendung von bestimmten Wiederholungsstrategien für einzelne Vorgänge ist das Erstellen einer Instanz der erforderlichen Strategieklasse und das Liefern der gewünschten Einstellungen mithilfe von Parametern.</span><span class="sxs-lookup"><span data-stu-id="bb080-571">Another technique for using specific retry strategies for individual operations is to create an instance of the required strategy class and supply the desired settings through parameters.</span></span> <span data-ttu-id="bb080-572">Rufen Sie dann die **ExecuteAsync** -Methode auf.</span><span class="sxs-lookup"><span data-stu-id="bb080-572">You then invoke its **ExecuteAsync** method.</span></span>

```csharp
var executionStrategy = new SqlAzureExecutionStrategy(5, TimeSpan.FromSeconds(4));
var blogs = await executionStrategy.ExecuteAsync(
    async () =>
    {
        using (var db = new BloggingContext("Blogs"))
        {
            // Acquire some values asynchronously and return them
        }
    },
    new CancellationToken()
);
```

<span data-ttu-id="bb080-573">Die einfachste Möglichkeit zum Verwenden einer **DbConfiguration**-Klasse ist es, diese in der gleichen Assembly wie die **DbContext**-Klasse zu platzieren.</span><span class="sxs-lookup"><span data-stu-id="bb080-573">The simplest way to use a **DbConfiguration** class is to locate it in the same assembly as the **DbContext** class.</span></span> <span data-ttu-id="bb080-574">Dies ist jedoch nicht geeignet, wenn der gleiche Kontext in verschiedenen Szenarios benötigt wird, wie z. B. bei anderen interaktiven Strategien und Hintergrundwiederholungsstrategien.</span><span class="sxs-lookup"><span data-stu-id="bb080-574">However, this is not appropriate when the same context is required in different scenarios, such as different interactive and background retry strategies.</span></span> <span data-ttu-id="bb080-575">Wenn die unterschiedlichen Kontexte in separaten AppDomains ausgeführt werden, können Sie die integrierte Unterstützung für das Angeben von Konfigurationsklassen in der Konfigurationsdatei verwenden oder explizit mithilfe von Code festlegen.</span><span class="sxs-lookup"><span data-stu-id="bb080-575">If the different contexts execute in separate AppDomains, you can use the built-in support for specifying configuration classes in the configuration file or set it explicitly using code.</span></span> <span data-ttu-id="bb080-576">Wenn die unterschiedlichen Kontexte in derselben AppDomain ausgeführt werden müssen, ist eine benutzerdefinierte Lösung erforderlich.</span><span class="sxs-lookup"><span data-stu-id="bb080-576">If the different contexts must execute in the same AppDomain, a custom solution will be required.</span></span>

<span data-ttu-id="bb080-577">Weitere Informationen finden Sie unter [Codebasierte Konfiguration (EF6 oder höher)](http://msdn.microsoft.com/data/jj680699.aspx).</span><span class="sxs-lookup"><span data-stu-id="bb080-577">For more information, see [Code-Based Configuration (EF6 onwards)](http://msdn.microsoft.com/data/jj680699.aspx).</span></span>

<span data-ttu-id="bb080-578">Die folgende Tabelle zeigt die Standardeinstellungen für die integrierte Wiederholungsrichtlinie bei Verwendung von EF6.</span><span class="sxs-lookup"><span data-stu-id="bb080-578">The following table shows the default settings for the built-in retry policy when using EF6.</span></span>

| <span data-ttu-id="bb080-579">Einstellung</span><span class="sxs-lookup"><span data-stu-id="bb080-579">Setting</span></span> | <span data-ttu-id="bb080-580">Standardwert</span><span class="sxs-lookup"><span data-stu-id="bb080-580">Default value</span></span> | <span data-ttu-id="bb080-581">Bedeutung</span><span class="sxs-lookup"><span data-stu-id="bb080-581">Meaning</span></span> |
|---------|---------------|---------|
| <span data-ttu-id="bb080-582">Richtlinie</span><span class="sxs-lookup"><span data-stu-id="bb080-582">Policy</span></span> | <span data-ttu-id="bb080-583">Exponentiell</span><span class="sxs-lookup"><span data-stu-id="bb080-583">Exponential</span></span> | <span data-ttu-id="bb080-584">Exponentielles Backoff.</span><span class="sxs-lookup"><span data-stu-id="bb080-584">Exponential back-off.</span></span> |
| <span data-ttu-id="bb080-585">MaxRetryCount</span><span class="sxs-lookup"><span data-stu-id="bb080-585">MaxRetryCount</span></span> | <span data-ttu-id="bb080-586">5</span><span class="sxs-lookup"><span data-stu-id="bb080-586">5</span></span> | <span data-ttu-id="bb080-587">Die maximale Anzahl von Warnungen.</span><span class="sxs-lookup"><span data-stu-id="bb080-587">The maximum number of retries.</span></span> |
| <span data-ttu-id="bb080-588">MaxDelay</span><span class="sxs-lookup"><span data-stu-id="bb080-588">MaxDelay</span></span> | <span data-ttu-id="bb080-589">30 Sekunden</span><span class="sxs-lookup"><span data-stu-id="bb080-589">30 seconds</span></span> | <span data-ttu-id="bb080-590">Die maximale Verzögerung zwischen Wiederholungsversuchen.</span><span class="sxs-lookup"><span data-stu-id="bb080-590">The maximum delay between retries.</span></span> <span data-ttu-id="bb080-591">Dieser Wert hat keine Auswirkung auf die Berechnung der Verzögerungsreihen.</span><span class="sxs-lookup"><span data-stu-id="bb080-591">This value does not affect how the series of delays are computed.</span></span> <span data-ttu-id="bb080-592">Er definiert lediglich eine Obergrenze.</span><span class="sxs-lookup"><span data-stu-id="bb080-592">It only defines an upper bound.</span></span> |
| <span data-ttu-id="bb080-593">DefaultCoefficient</span><span class="sxs-lookup"><span data-stu-id="bb080-593">DefaultCoefficient</span></span> | <span data-ttu-id="bb080-594">1 Sekunde</span><span class="sxs-lookup"><span data-stu-id="bb080-594">1 second</span></span> | <span data-ttu-id="bb080-595">Der Koeffizient für die Berechnung des exponentiellen Backoffs.</span><span class="sxs-lookup"><span data-stu-id="bb080-595">The coefficient for the exponential back-off computation.</span></span> <span data-ttu-id="bb080-596">Dieser Wert kann nicht geändert werden.</span><span class="sxs-lookup"><span data-stu-id="bb080-596">This value cannot be changed.</span></span> |
| <span data-ttu-id="bb080-597">DefaultRandomFactor</span><span class="sxs-lookup"><span data-stu-id="bb080-597">DefaultRandomFactor</span></span> | <span data-ttu-id="bb080-598">1.1</span><span class="sxs-lookup"><span data-stu-id="bb080-598">1.1</span></span> | <span data-ttu-id="bb080-599">Der Multiplikator zum Hinzufügen einer beliebigen Verzögerung für die einzelnen Einträge.</span><span class="sxs-lookup"><span data-stu-id="bb080-599">The multiplier used to add a random delay for each entry.</span></span> <span data-ttu-id="bb080-600">Dieser Wert kann nicht geändert werden.</span><span class="sxs-lookup"><span data-stu-id="bb080-600">This value cannot be changed.</span></span> |
| <span data-ttu-id="bb080-601">DefaultExponentialBase</span><span class="sxs-lookup"><span data-stu-id="bb080-601">DefaultExponentialBase</span></span> | <span data-ttu-id="bb080-602">2</span><span class="sxs-lookup"><span data-stu-id="bb080-602">2</span></span> | <span data-ttu-id="bb080-603">Der Multiplikator für die Berechnung der nächsten Verzögerung.</span><span class="sxs-lookup"><span data-stu-id="bb080-603">The multiplier used to calculate the next delay.</span></span> <span data-ttu-id="bb080-604">Dieser Wert kann nicht geändert werden.</span><span class="sxs-lookup"><span data-stu-id="bb080-604">This value cannot be changed.</span></span> |

### <a name="retry-usage-guidance"></a><span data-ttu-id="bb080-605">Gebrauchsanleitung Wiederholungen</span><span class="sxs-lookup"><span data-stu-id="bb080-605">Retry usage guidance</span></span>
<span data-ttu-id="bb080-606">Beachten Sie den Zugriff auf SQL-Datenbank mit EF6 die folgenden Richtlinien:</span><span class="sxs-lookup"><span data-stu-id="bb080-606">Consider the following guidelines when accessing SQL Database using EF6:</span></span>

* <span data-ttu-id="bb080-607">Wählen Sie die entsprechende Dienstoption (freigegeben oder Premium).</span><span class="sxs-lookup"><span data-stu-id="bb080-607">Choose the appropriate service option (shared or premium).</span></span> <span data-ttu-id="bb080-608">Bei einer freigegebenen Instanz kann es möglicherweise zu längeren Verbindungsverzögerungen als üblich und Drosselung durch die Verwendung des gemeinsam genutzten Servers durch andere Mandanten kommen.</span><span class="sxs-lookup"><span data-stu-id="bb080-608">A shared instance may suffer longer than usual connection delays and throttling due to the usage by other tenants of the shared server.</span></span> <span data-ttu-id="bb080-609">Wenn Vorgänge mit vorhersagbarer Leistung und zuverlässig geringe Latenz erforderlich sind, sollten Sie die Premium-Option wählen.</span><span class="sxs-lookup"><span data-stu-id="bb080-609">If predictable performance and reliable low latency operations are required, consider choosing the premium option.</span></span>
* <span data-ttu-id="bb080-610">Eine Strategie mit festgelegtem Intervall wird für die Verwendung mit Azure SQL-Datenbank nicht empfohlen.</span><span class="sxs-lookup"><span data-stu-id="bb080-610">A fixed interval strategy is not recommended for use with Azure SQL Database.</span></span> <span data-ttu-id="bb080-611">Verwenden Sie stattdessen eine exponentielle Backoff-Strategie, da der Dienst möglicherweise überlastet ist und längere Verzögerungen mehr Zeit für die Wiederherstellung einräumen.</span><span class="sxs-lookup"><span data-stu-id="bb080-611">Instead, use an exponential back-off strategy because the service may be overloaded, and longer delays allow more time for it to recover.</span></span>
* <span data-ttu-id="bb080-612">Wählen Sie einen geeigneten Wert für die Verbindungs- und Befehls-Timeouts bei der Definition von Verbindungen.</span><span class="sxs-lookup"><span data-stu-id="bb080-612">Choose a suitable value for the connection and command timeouts when defining connections.</span></span> <span data-ttu-id="bb080-613">Stützen Sie das Timeout auf Ihren Geschäftslogikentwurf und auf Tests.</span><span class="sxs-lookup"><span data-stu-id="bb080-613">Base the timeout on both your business logic design and through testing.</span></span> <span data-ttu-id="bb080-614">Sie müssen diesen Wert möglicherweise mit der Zeit ändern, wenn sich die Datenmengen oder Geschäftsprozesse ändern.</span><span class="sxs-lookup"><span data-stu-id="bb080-614">You may need to modify this value over time as the volumes of data or the business processes change.</span></span> <span data-ttu-id="bb080-615">Ein zu kurzes Timeout kann zu vorzeitigen Fehlern bei Verbindungen führen, wenn die Datenbank ausgelastet ist.</span><span class="sxs-lookup"><span data-stu-id="bb080-615">Too short a timeout may result in premature failures of connections when the database is busy.</span></span> <span data-ttu-id="bb080-616">Ein zu langes Timeout kann verhindern, dass die Wiederholungslogik ordnungsgemäß funktioniert, da sie vor der Erkennung einer fehlgeschlagenen Verbindung zu lange wartet.</span><span class="sxs-lookup"><span data-stu-id="bb080-616">Too long a timeout may prevent the retry logic working correctly by waiting too long before detecting a failed connection.</span></span> <span data-ttu-id="bb080-617">Der Wert für das Timeout ist eine Komponente der End-to-End-Latenz, auch es nicht leicht ist zu ermitteln, wie viele Befehle beim Speichern des Kontexts ausgeführt werden.</span><span class="sxs-lookup"><span data-stu-id="bb080-617">The value of the timeout is a component of the end-to-end latency, although you cannot easily determine how many commands will execute when saving the context.</span></span> <span data-ttu-id="bb080-618">Sie können das Standardtimeout ändern, indem Sie die Einstellung der **CommandTimeout**-Eigenschaft der **DbContext**-Instanz festlegen.</span><span class="sxs-lookup"><span data-stu-id="bb080-618">You can change the default timeout by setting the **CommandTimeout** property of the **DbContext** instance.</span></span>
* <span data-ttu-id="bb080-619">Entity Framework unterstützt Wiederholungskonfigurationen, die in Konfigurationsdateien definiert sind.</span><span class="sxs-lookup"><span data-stu-id="bb080-619">Entity Framework supports retry configurations defined in configuration files.</span></span> <span data-ttu-id="bb080-620">Für maximale Flexibilität in Azure sollten Sie allerdings die Konfiguration in der Anwendung  programmgesteuert erstellen.</span><span class="sxs-lookup"><span data-stu-id="bb080-620">However, for maximum flexibility on Azure you should consider creating the configuration programmatically within the application.</span></span> <span data-ttu-id="bb080-621">Die einzelnen Parameter für die Wiederholungsrichtlinien, wie z. B. die Anzahl der Wiederholungen und die Wiederholungsintervalle können in der Dienstkonfigurationsdatei gespeichert und zur Laufzeit verwendet werden, um die entsprechenden Richtlinien zu erstellen.</span><span class="sxs-lookup"><span data-stu-id="bb080-621">The specific parameters for the retry policies, such as the number of retries and the retry intervals, can be stored in the service configuration file and used at runtime to create the appropriate policies.</span></span> <span data-ttu-id="bb080-622">Dadurch können die Einstellungen darin geändert werden, ohne dass die Anwendung neu gestartet werden muss.</span><span class="sxs-lookup"><span data-stu-id="bb080-622">This allows the settings to be changed without requiring the application to be restarted.</span></span>

<span data-ttu-id="bb080-623">Erwägen Sie, mit den folgenden Einstellungen für Wiederholungsvorgänge zu beginnen.</span><span class="sxs-lookup"><span data-stu-id="bb080-623">Consider starting with the following settings for retrying operations.</span></span> <span data-ttu-id="bb080-624">Sie können die Verzögerung zwischen den Wiederholungen nicht angeben (sie ist als eine exponentielle Sequenz festgelegt und generiert).</span><span class="sxs-lookup"><span data-stu-id="bb080-624">You cannot specify the delay between retry attempts (it is fixed and generated as an exponential sequence).</span></span> <span data-ttu-id="bb080-625">Sofern Sie keine benutzerdefinierte Wiederholungsstrategie erstellen, können Sie wie hier gezeigt nur die maximalen Werte angeben.</span><span class="sxs-lookup"><span data-stu-id="bb080-625">You can specify only the maximum values, as shown here; unless you create a custom retry strategy.</span></span> <span data-ttu-id="bb080-626">Hierbei handelt es sich um allgemeine Einstellungen und Sie sollten die Vorgänge überwachen und die Werte entsprechend Ihrem Szenario optimieren.</span><span class="sxs-lookup"><span data-stu-id="bb080-626">These are general purpose settings, and you should monitor the operations and fine tune the values to suit your own scenario.</span></span>

| <span data-ttu-id="bb080-627">**Context**</span><span class="sxs-lookup"><span data-stu-id="bb080-627">**Context**</span></span> | <span data-ttu-id="bb080-628">**Beispiel-Ziel E2E<br />Maximale Wartezeit**</span><span class="sxs-lookup"><span data-stu-id="bb080-628">**Sample target E2E<br />max latency**</span></span> | <span data-ttu-id="bb080-629">**Wiederholungsrichtlinie**</span><span class="sxs-lookup"><span data-stu-id="bb080-629">**Retry policy**</span></span> | <span data-ttu-id="bb080-630">**Einstellungen**</span><span class="sxs-lookup"><span data-stu-id="bb080-630">**Settings**</span></span> | <span data-ttu-id="bb080-631">**Werte**</span><span class="sxs-lookup"><span data-stu-id="bb080-631">**Values**</span></span> | <span data-ttu-id="bb080-632">**So funktioniert's**</span><span class="sxs-lookup"><span data-stu-id="bb080-632">**How it works**</span></span> |
| --- | --- | --- | --- | --- | --- |
| <span data-ttu-id="bb080-633">Interaktiv, Benutzeroberfläche</span><span class="sxs-lookup"><span data-stu-id="bb080-633">Interactive, UI,</span></span><br /><span data-ttu-id="bb080-634">oder Vordergrund</span><span class="sxs-lookup"><span data-stu-id="bb080-634">or foreground</span></span> |<span data-ttu-id="bb080-635">2 Sekunden</span><span class="sxs-lookup"><span data-stu-id="bb080-635">2 seconds</span></span> |<span data-ttu-id="bb080-636">Exponentiell</span><span class="sxs-lookup"><span data-stu-id="bb080-636">Exponential</span></span> |<span data-ttu-id="bb080-637">MaxRetryCount</span><span class="sxs-lookup"><span data-stu-id="bb080-637">MaxRetryCount</span></span><br /><span data-ttu-id="bb080-638">MaxDelay</span><span class="sxs-lookup"><span data-stu-id="bb080-638">MaxDelay</span></span> |<span data-ttu-id="bb080-639">3</span><span class="sxs-lookup"><span data-stu-id="bb080-639">3</span></span><br /><span data-ttu-id="bb080-640">750 ms</span><span class="sxs-lookup"><span data-stu-id="bb080-640">750 ms</span></span> |<span data-ttu-id="bb080-641">Versuch 1 – Verzögerung 0 Sek.</span><span class="sxs-lookup"><span data-stu-id="bb080-641">Attempt 1 - delay 0 sec</span></span><br /><span data-ttu-id="bb080-642">Versuch 2 – Verzögerung 750 ms</span><span class="sxs-lookup"><span data-stu-id="bb080-642">Attempt 2 - delay 750 ms</span></span><br /><span data-ttu-id="bb080-643">Versuch 3 - Verzögerung 750 ms</span><span class="sxs-lookup"><span data-stu-id="bb080-643">Attempt 3 – delay 750 ms</span></span> |
| <span data-ttu-id="bb080-644">Hintergrund</span><span class="sxs-lookup"><span data-stu-id="bb080-644">Background</span></span><br /> <span data-ttu-id="bb080-645">oder Batch</span><span class="sxs-lookup"><span data-stu-id="bb080-645">or batch</span></span> |<span data-ttu-id="bb080-646">30 Sekunden</span><span class="sxs-lookup"><span data-stu-id="bb080-646">30 seconds</span></span> |<span data-ttu-id="bb080-647">Exponentiell</span><span class="sxs-lookup"><span data-stu-id="bb080-647">Exponential</span></span> |<span data-ttu-id="bb080-648">MaxRetryCount</span><span class="sxs-lookup"><span data-stu-id="bb080-648">MaxRetryCount</span></span><br /><span data-ttu-id="bb080-649">MaxDelay</span><span class="sxs-lookup"><span data-stu-id="bb080-649">MaxDelay</span></span> |<span data-ttu-id="bb080-650">5</span><span class="sxs-lookup"><span data-stu-id="bb080-650">5</span></span><br /><span data-ttu-id="bb080-651">12 Sekunden</span><span class="sxs-lookup"><span data-stu-id="bb080-651">12 seconds</span></span> |<span data-ttu-id="bb080-652">Versuch 1 – Verzögerung 0 Sek.</span><span class="sxs-lookup"><span data-stu-id="bb080-652">Attempt 1 - delay 0 sec</span></span><br /><span data-ttu-id="bb080-653">Versuch 2 – Verzögerung ca. 1 Sek.</span><span class="sxs-lookup"><span data-stu-id="bb080-653">Attempt 2 - delay ~1 sec</span></span><br /><span data-ttu-id="bb080-654">Versuch 3 – Verzögerung ca. 3 Sek.</span><span class="sxs-lookup"><span data-stu-id="bb080-654">Attempt 3 - delay ~3 sec</span></span><br /><span data-ttu-id="bb080-655">Versuch 4 – Verzögerung ca. 7 Sek.</span><span class="sxs-lookup"><span data-stu-id="bb080-655">Attempt 4 - delay ~7 sec</span></span><br /><span data-ttu-id="bb080-656">Versuch 5 – Verzögerung ca. 12 Sek.</span><span class="sxs-lookup"><span data-stu-id="bb080-656">Attempt 5 - delay 12 sec</span></span> |

> [!NOTE]
> <span data-ttu-id="bb080-657">Die End-to-End-Latenzziele setzen das Standardtimeout für Verbindungen mit dem Dienst voraus.</span><span class="sxs-lookup"><span data-stu-id="bb080-657">The end-to-end latency targets assume the default timeout for connections to the service.</span></span> <span data-ttu-id="bb080-658">Wenn Sie längere Verbindungstimeouts angeben, wird die End-to-End-Latenz durch diese zusätzliche Zeit für jeden Wiederholungsversuch erweitert.</span><span class="sxs-lookup"><span data-stu-id="bb080-658">If you specify longer connection timeouts, the end-to-end latency will be extended by this additional time for every retry attempt.</span></span>
>
>

### <a name="examples"></a><span data-ttu-id="bb080-659">Beispiele</span><span class="sxs-lookup"><span data-stu-id="bb080-659">Examples</span></span>
<span data-ttu-id="bb080-660">Im folgenden Codebeispiel wird eine einfachen Datenzugriffslösung definiert, die Entity Framework verwendet.</span><span class="sxs-lookup"><span data-stu-id="bb080-660">The following code example defines a simple data access solution that uses Entity Framework.</span></span> <span data-ttu-id="bb080-661">Eine bestimmte Wiederholungsstrategie wird durch die Definition einer Instanz einer Klasse mit dem Namen **BlogConfiguration** festgelegt, die **DbConfiguration** erweitert.</span><span class="sxs-lookup"><span data-stu-id="bb080-661">It sets a specific retry strategy by defining an instance of a class named **BlogConfiguration** that extends **DbConfiguration**.</span></span>

```csharp
using System;
using System.Collections.Generic;
using System.Data.Entity;
using System.Data.Entity.SqlServer;
using System.Threading.Tasks;

namespace RetryCodeSamples
{
    public class BlogConfiguration : DbConfiguration
    {
        public BlogConfiguration()
        {
            // Set up the execution strategy for SQL Database (exponential) with 5 retries and 12 sec delay.
            // These values could be loaded from configuration rather than being hard-coded.
            this.SetExecutionStrategy(
                    "System.Data.SqlClient", () => new SqlAzureExecutionStrategy(5, TimeSpan.FromSeconds(12)));
        }
    }

    // Specify the configuration type if more than one has been defined.
    // [DbConfigurationType(typeof(BlogConfiguration))]
    public class BloggingContext : DbContext
    {
        // Definition of content goes here.
    }

    class EF6CodeSamples
    {
        public async static Task Samples()
        {
            // Execution strategy configured by DbConfiguration subclass, discovered automatically or
            // or explicitly indicated through configuration or with an attribute. Default is no retries.
            using (var db = new BloggingContext("Blogs"))
            {
                // Add, edit, delete blog items here, then:
                await db.SaveChangesAsync();
            }
        }
    }
}
```

<span data-ttu-id="bb080-662">Weitere Beispiele zur Verwendung des Entity Framework Wiederholungsmechanismus finden Sie unter [Verbindungsstabilität / Wiederholungslogik](http://msdn.microsoft.com/data/dn456835.aspx).</span><span class="sxs-lookup"><span data-stu-id="bb080-662">More examples of using the Entity Framework retry mechanism can be found in [Connection Resiliency / Retry Logic](http://msdn.microsoft.com/data/dn456835.aspx).</span></span>

### <a name="more-information"></a><span data-ttu-id="bb080-663">Weitere Informationen</span><span class="sxs-lookup"><span data-stu-id="bb080-663">More information</span></span>
* [<span data-ttu-id="bb080-664">Anleitung zu Leistung und Flexibilität von Azure SQL-Datenbanken</span><span class="sxs-lookup"><span data-stu-id="bb080-664">Azure SQL Database Performance and Elasticity Guide</span></span>](http://social.technet.microsoft.com/wiki/contents/articles/3507.windows-azure-sql-database-performance-and-elasticity-guide.aspx)

## <a name="sql-database-using-entity-framework-core"></a><span data-ttu-id="bb080-665">SQL-Datenbank mit Entity Framework Core</span><span class="sxs-lookup"><span data-stu-id="bb080-665">SQL Database using Entity Framework Core</span></span>
<span data-ttu-id="bb080-666">[Entity Framework Core](/ef/core/) ist eine objektrelationale Zuordnung, die .NET Core-Entwicklern die Arbeit mit Daten mithilfe von domänenspezifischen Objekten ermöglicht.</span><span class="sxs-lookup"><span data-stu-id="bb080-666">[Entity Framework Core](/ef/core/) is an object-relational mapper that enables .NET Core developers to work with data using domain-specific objects.</span></span> <span data-ttu-id="bb080-667">Es entfällt die Notwendigkeit für den Großteil des Datenzugriffs-Codes, den Entwickler normalerweise schreiben müssen.</span><span class="sxs-lookup"><span data-stu-id="bb080-667">It eliminates the need for most of the data-access code that developers usually need to write.</span></span> <span data-ttu-id="bb080-668">Diese Version von Entity Framework wurde von Grund auf neu geschrieben und erbt nicht automatisch alle Features von EF6.x.</span><span class="sxs-lookup"><span data-stu-id="bb080-668">This version of Entity Framework was written from the ground up, and doesn't automatically inherit all the features from EF6.x.</span></span>

### <a name="retry-mechanism"></a><span data-ttu-id="bb080-669">Wiederholungsmechanismus</span><span class="sxs-lookup"><span data-stu-id="bb080-669">Retry mechanism</span></span>
<span data-ttu-id="bb080-670">Beim Zugriff auf SQL-Datenbank mit Entity Framework Core wird über einen Mechanismus mit dem Namen [Verbindungsstabilität](/ef/core/miscellaneous/connection-resiliency) die Wiederholungsunterstützung ermöglicht.</span><span class="sxs-lookup"><span data-stu-id="bb080-670">Retry support is provided when accessing SQL Database using Entity Framework Core through a mechanism called [Connection Resiliency](/ef/core/miscellaneous/connection-resiliency).</span></span> <span data-ttu-id="bb080-671">Verbindungsstabilität wurde mit EF Core 1.1.0 eingeführt.</span><span class="sxs-lookup"><span data-stu-id="bb080-671">Connection resiliency was introduced in EF Core 1.1.0.</span></span>

<span data-ttu-id="bb080-672">Die primäre Abstraktion ist die `IExecutionStrategy`-Schnittstelle.</span><span class="sxs-lookup"><span data-stu-id="bb080-672">The primary abstraction is the `IExecutionStrategy` interface.</span></span> <span data-ttu-id="bb080-673">Die Ausführungsstrategie für SQL Server, einschließlich SQL Azure, ist über die Ausnahmetypen informiert, die wiederholt werden können, und verfügt über sinnvolle Standardwerte für maximale Wiederholungsversuche, Verzögerung zwischen Wiederholungen usw.</span><span class="sxs-lookup"><span data-stu-id="bb080-673">The execution strategy for SQL Server, including SQL Azure, is aware of the exception types that can be retried and has sensible defaults for maximum retries, delay between retries, and so on.</span></span>

### <a name="examples"></a><span data-ttu-id="bb080-674">Beispiele</span><span class="sxs-lookup"><span data-stu-id="bb080-674">Examples</span></span>

<span data-ttu-id="bb080-675">Der folgende Code ermöglicht automatische Wiederholungen beim Konfigurieren des DbContext-Objekts, das eine Sitzung mit der Datenbank repräsentiert.</span><span class="sxs-lookup"><span data-stu-id="bb080-675">The following code enables automatic retries when configuring the DbContext object, which represents a session with the database.</span></span> 

```csharp
protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
{
    optionsBuilder
        .UseSqlServer(
            @"Server=(localdb)\mssqllocaldb;Database=EFMiscellanous.ConnectionResiliency;Trusted_Connection=True;",
            options => options.EnableRetryOnFailure());
}
```

<span data-ttu-id="bb080-676">Der folgende Code veranschaulicht, wie Sie eine Transaktion mit automatischen Wiederholungsversuchen ausführen, indem Sie eine Ausführungsstrategie verwenden.</span><span class="sxs-lookup"><span data-stu-id="bb080-676">The following code shows how to execute a transaction with automatic retries, by using an execution strategy.</span></span> <span data-ttu-id="bb080-677">Die Transaktion wird in einem Delegaten definiert.</span><span class="sxs-lookup"><span data-stu-id="bb080-677">The transaction is defined in a delegate.</span></span> <span data-ttu-id="bb080-678">Wenn ein vorübergehender Fehler auftritt, wird der Delegat von der Ausführungsstrategie erneut aufgerufen.</span><span class="sxs-lookup"><span data-stu-id="bb080-678">If a transient failure occurs, the execution strategy will invoke the delegate again.</span></span>

```csharp
using (var db = new BloggingContext())
{
    var strategy = db.Database.CreateExecutionStrategy();

    strategy.Execute(() =>
    {
        using (var transaction = db.Database.BeginTransaction())
        {
            db.Blogs.Add(new Blog { Url = "http://blogs.msdn.com/dotnet" });
            db.SaveChanges();

            db.Blogs.Add(new Blog { Url = "http://blogs.msdn.com/visualstudio" });
            db.SaveChanges();

            transaction.Commit();
        }
    });
}
```

## <a name="azure-storage"></a><span data-ttu-id="bb080-679">Azure Storage</span><span class="sxs-lookup"><span data-stu-id="bb080-679">Azure Storage</span></span>
<span data-ttu-id="bb080-680">Azure-Speicherdienste umfassen Tabellen- und Blob-Speicher, Dateien und Speicher-Warteschlangen.</span><span class="sxs-lookup"><span data-stu-id="bb080-680">Azure storage services include table and blob storage, files, and storage queues.</span></span>

### <a name="retry-mechanism"></a><span data-ttu-id="bb080-681">Wiederholungsmechanismus</span><span class="sxs-lookup"><span data-stu-id="bb080-681">Retry mechanism</span></span>
<span data-ttu-id="bb080-682">Wiederholungen treten auf individueller REST-Vorgangsebene auf und sind ein wesentlicher Bestandteil der Client-API-Implementierung.</span><span class="sxs-lookup"><span data-stu-id="bb080-682">Retries occur at the individual REST operation level and are an integral part of the client API implementation.</span></span> <span data-ttu-id="bb080-683">Das Client-Speicher SDK verwendet Klassen, die die [IExtendedRetryPolicy Schnittstelle](http://msdn.microsoft.com/library/microsoft.windowsazure.storage.retrypolicies.iextendedretrypolicy.aspx)implementieren.</span><span class="sxs-lookup"><span data-stu-id="bb080-683">The client storage SDK uses classes that implement the [IExtendedRetryPolicy Interface](http://msdn.microsoft.com/library/microsoft.windowsazure.storage.retrypolicies.iextendedretrypolicy.aspx).</span></span>

<span data-ttu-id="bb080-684">Es gibt verschiedene Implementierungen der Schnittstelle.</span><span class="sxs-lookup"><span data-stu-id="bb080-684">There are different implementations of the interface.</span></span> <span data-ttu-id="bb080-685">Speicherclients können Richtlinien auswählen, die speziell für den Zugriff auf Tabellen, Blobs und Warteschlangen entworfen wurden.</span><span class="sxs-lookup"><span data-stu-id="bb080-685">Storage clients can choose from policies specifically designed for accessing tables, blobs, and queues.</span></span> <span data-ttu-id="bb080-686">Jede Implementierung verwendet eine andere Wiederholungsstrategie, die im Wesentlichen das Wiederholungsintervall und andere Details definiert.</span><span class="sxs-lookup"><span data-stu-id="bb080-686">Each implementation uses a different retry strategy that essentially defines the retry interval and other details.</span></span>

<span data-ttu-id="bb080-687">Die integrierten Klassen bieten Unterstützung für linear (konstante Verzögerung) und exponentiell mit Zufallsgenerator-Wiederholungsintervallen.</span><span class="sxs-lookup"><span data-stu-id="bb080-687">The built-in classes provide support for linear (constant delay) and exponential with randomization retry intervals.</span></span> <span data-ttu-id="bb080-688">Es gibt auch eine Richtlinie für keine Wiederholung, wenn ein anderer Prozess Wiederholungen auf höherer Ebene behandelt.</span><span class="sxs-lookup"><span data-stu-id="bb080-688">There is also a no retry policy for use when another process is handling retries at a higher level.</span></span> <span data-ttu-id="bb080-689">Allerdings können Sie Ihre eigenen Wiederholungsklassen implementieren, wenn Sie spezielle Anforderungen haben, die von den integrierten Klassen nicht bereitgestellt werden.</span><span class="sxs-lookup"><span data-stu-id="bb080-689">However, you can implement your own retry classes if you have specific requirements not provided by the built-in classes.</span></span>

<span data-ttu-id="bb080-690">Abwechselnde Wiederholungen wechseln zwischen primären und sekundären Speicherdienststandorten, wenn Sie Lesezugriff auf georedundante Speicher (RA-GRS) verwenden und das Ergebnis der Anforderung ist ein wiederholbarer Fehler.</span><span class="sxs-lookup"><span data-stu-id="bb080-690">Alternate retries switch between primary and secondary storage service location if you are using read access geo-redundant storage (RA-GRS) and the result of the request is a retryable error.</span></span> <span data-ttu-id="bb080-691">Weitere Informationen finden Sie unter [Redundanzoptionen für Azure Storage](http://msdn.microsoft.com/library/azure/dn727290.aspx) .</span><span class="sxs-lookup"><span data-stu-id="bb080-691">See [Azure Storage Redundancy Options](http://msdn.microsoft.com/library/azure/dn727290.aspx) for more information.</span></span>

### <a name="policy-configuration"></a><span data-ttu-id="bb080-692">Richtlinienkonfiguration</span><span class="sxs-lookup"><span data-stu-id="bb080-692">Policy configuration</span></span>
<span data-ttu-id="bb080-693">Wiederholungsrichtlinien werden programmgesteuert konfiguriert.</span><span class="sxs-lookup"><span data-stu-id="bb080-693">Retry policies are configured programmatically.</span></span> <span data-ttu-id="bb080-694">Eine typische Vorgehensweise ist das Erstellen und Ausfüllen von **TableRequestOptions**-, **BlobRequestOptions**-, **FileRequestOptions**- oder **QueueRequestOptions**-Instanzen.</span><span class="sxs-lookup"><span data-stu-id="bb080-694">A typical procedure is to create and populate a **TableRequestOptions**, **BlobRequestOptions**, **FileRequestOptions**, or **QueueRequestOptions** instance.</span></span>

```csharp
TableRequestOptions interactiveRequestOption = new TableRequestOptions()
{
  RetryPolicy = new LinearRetry(TimeSpan.FromMilliseconds(500), 3),
  // For Read-access geo-redundant storage, use PrimaryThenSecondary.
  // Otherwise set this to PrimaryOnly.
  LocationMode = LocationMode.PrimaryThenSecondary,
  // Maximum execution time based on the business use case. 
  MaximumExecutionTime = TimeSpan.FromSeconds(2)
};
```

<span data-ttu-id="bb080-695">Die Anforderungsoptioneninstanz kann dann auf dem Client festgelegt werden und alle Operationen mit dem Client verwenden die angegebene Anforderungsoptionen.</span><span class="sxs-lookup"><span data-stu-id="bb080-695">The request options instance can then be set on the client, and all operations with the client will use the specified request options.</span></span>

```csharp
client.DefaultRequestOptions = interactiveRequestOption;
var stats = await client.GetServiceStatsAsync();
```

<span data-ttu-id="bb080-696">Sie können die Client-Anforderungsoptionen überschreiben, indem Sie eine ausgefüllte Instanz der Anforderungsoptionsklasse als Parameter an Vorgangsmethoden übergeben.</span><span class="sxs-lookup"><span data-stu-id="bb080-696">You can override the client request options by passing a populated instance of the request options class as a parameter to operation methods.</span></span>

```csharp
var stats = await client.GetServiceStatsAsync(interactiveRequestOption, operationContext: null);
```

<span data-ttu-id="bb080-697">Sie verwenden eine **OperationContext** -Instanz, um den auszuführenden Code anzugeben, wenn eine Wiederholung auftritt und ein Vorgang abgeschlossen wurde.</span><span class="sxs-lookup"><span data-stu-id="bb080-697">You use an **OperationContext** instance to specify the code to execute when a retry occurs and when an operation has completed.</span></span> <span data-ttu-id="bb080-698">Dieser Code kann Informationen über den Vorgang zur Verwendung in Protokollen und Telemetrie erfassen.</span><span class="sxs-lookup"><span data-stu-id="bb080-698">This code can collect information about the operation for use in logs and telemetry.</span></span>

```csharp
// Set up notifications for an operation
var context = new OperationContext();
context.ClientRequestID = "some request id";
context.Retrying += (sender, args) =>
{
    /* Collect retry information */
};
context.RequestCompleted += (sender, args) =>
{
    /* Collect operation completion information */
};
var stats = await client.GetServiceStatsAsync(null, context);
```

<span data-ttu-id="bb080-699">Zusätzlich zur Angabe, ob ein Fehler für die Wiederholung geeignet ist, geben die erweiterten Wiederholungsrichtlinien ein **RetryContext** -Objekt zurück, das folgendes angibt: die Anzahl der Wiederholungen, die Ergebnisse der letzten Anforderung, ob die nächste Wiederholung im primären oder sekundären Standort ausgeführt werden (siehe Tabelle unten).</span><span class="sxs-lookup"><span data-stu-id="bb080-699">In addition to indicating whether a failure is suitable for retry, the extended retry policies return a **RetryContext** object that indicates the number of retries, the results of the last request, whether the next retry will happen in the primary or secondary location (see table below for details).</span></span> <span data-ttu-id="bb080-700">Die Eigenschaften des **RetryContext** Objekts können verwendet werden, um zu entscheiden, ob und wann eine Wiederholung versucht wird.</span><span class="sxs-lookup"><span data-stu-id="bb080-700">The properties of the **RetryContext** object can be used to decide if and when to attempt a retry.</span></span> <span data-ttu-id="bb080-701">Weitere Informationen finden Sie unter [IExtendedRetryPolicy.Evaluate-Methode](http://msdn.microsoft.com/library/microsoft.windowsazure.storage.retrypolicies.iextendedretrypolicy.evaluate.aspx).</span><span class="sxs-lookup"><span data-stu-id="bb080-701">For more details, see [IExtendedRetryPolicy.Evaluate Method](http://msdn.microsoft.com/library/microsoft.windowsazure.storage.retrypolicies.iextendedretrypolicy.evaluate.aspx).</span></span>

<span data-ttu-id="bb080-702">Die folgenden Tabellen enthalten die Standardeinstellungen für die integrierten Wiederholungsrichtlinien.</span><span class="sxs-lookup"><span data-stu-id="bb080-702">The following tables show the default settings for the built-in retry policies.</span></span>

<span data-ttu-id="bb080-703">**Anforderungsoptionen**</span><span class="sxs-lookup"><span data-stu-id="bb080-703">**Request options**</span></span>

| <span data-ttu-id="bb080-704">**Einstellung**</span><span class="sxs-lookup"><span data-stu-id="bb080-704">**Setting**</span></span> | <span data-ttu-id="bb080-705">**Standardwert**</span><span class="sxs-lookup"><span data-stu-id="bb080-705">**Default value**</span></span> | <span data-ttu-id="bb080-706">**Bedeutung**</span><span class="sxs-lookup"><span data-stu-id="bb080-706">**Meaning**</span></span> |
| --- | --- | --- |
| <span data-ttu-id="bb080-707">MaximumExecutionTime</span><span class="sxs-lookup"><span data-stu-id="bb080-707">MaximumExecutionTime</span></span> | <span data-ttu-id="bb080-708">Keine</span><span class="sxs-lookup"><span data-stu-id="bb080-708">None</span></span> | <span data-ttu-id="bb080-709">Maximale Ausführungszeit für die Anforderung, einschließlich aller möglichen Wiederholungsversuche.</span><span class="sxs-lookup"><span data-stu-id="bb080-709">Maximum execution time for the request, including all potential retry attempts.</span></span> <span data-ttu-id="bb080-710">Ist diese Option nicht angegeben, ist die für diese Anforderung zulässige Zeitspanne unendlich.</span><span class="sxs-lookup"><span data-stu-id="bb080-710">If it is not specified, then the amount of time that a request is permitted to take is unlimited.</span></span> <span data-ttu-id="bb080-711">Das heißt, es kann sein, dass die Anforderung nicht reagiert.</span><span class="sxs-lookup"><span data-stu-id="bb080-711">In other words, the request might hang.</span></span> |
| <span data-ttu-id="bb080-712">ServerTimeout</span><span class="sxs-lookup"><span data-stu-id="bb080-712">ServerTimeout</span></span> | <span data-ttu-id="bb080-713">Keine</span><span class="sxs-lookup"><span data-stu-id="bb080-713">None</span></span> | <span data-ttu-id="bb080-714">Server-Timeout-Intervall für die Anforderung (der Wert wird in Sekunden gerundet).</span><span class="sxs-lookup"><span data-stu-id="bb080-714">Server timeout interval for the request (value is rounded to seconds).</span></span> <span data-ttu-id="bb080-715">Wenn nicht angegeben, wird der Standardwert für alle Anforderungen an den Server verwendet.</span><span class="sxs-lookup"><span data-stu-id="bb080-715">If not specified, it will use the default value for all requests to the server.</span></span> <span data-ttu-id="bb080-716">In der Regel ist die beste Option, diese Einstellung auszulassen, sodass die Standardeinstellung des Servers verwendet wird.</span><span class="sxs-lookup"><span data-stu-id="bb080-716">Usually, the best option is to omit this setting so that the server default is used.</span></span> | 
| <span data-ttu-id="bb080-717">LocationMode</span><span class="sxs-lookup"><span data-stu-id="bb080-717">LocationMode</span></span> | <span data-ttu-id="bb080-718">Keine</span><span class="sxs-lookup"><span data-stu-id="bb080-718">None</span></span> | <span data-ttu-id="bb080-719">Wenn das Speicherkonto mit der Replikationsoption des Lesezugriffs auf georedundanten Speicher (RA-GRS) erstellt wird, können Sie mit dem Speicherortmodus bestimmen, welche Stelle die Anforderung erhalten soll.</span><span class="sxs-lookup"><span data-stu-id="bb080-719">If the storage account is created with the Read access geo-redundant storage (RA-GRS) replication option, you can use the location mode to indicate which location should receive the request.</span></span> <span data-ttu-id="bb080-720">Wenn zum Beispiel **PrimaryThenSecondary** angegeben ist, werden Anforderungen immer zuerst an den primären Standort gesendet.</span><span class="sxs-lookup"><span data-stu-id="bb080-720">For example, if **PrimaryThenSecondary** is specified, requests are always sent to the primary location first.</span></span> <span data-ttu-id="bb080-721">Wenn eine Anforderung fehlschlägt, wird sie an den sekundären Standort gesendet.</span><span class="sxs-lookup"><span data-stu-id="bb080-721">If a request fails, it is sent to the secondary location.</span></span> |
| <span data-ttu-id="bb080-722">RetryPolicy</span><span class="sxs-lookup"><span data-stu-id="bb080-722">RetryPolicy</span></span> | <span data-ttu-id="bb080-723">ExponentialPolicy</span><span class="sxs-lookup"><span data-stu-id="bb080-723">ExponentialPolicy</span></span> | <span data-ttu-id="bb080-724">Details zu jeder Option finden Sie weiter unten.</span><span class="sxs-lookup"><span data-stu-id="bb080-724">See below for details of each option.</span></span> |

<span data-ttu-id="bb080-725">**Exponentielle Richtlinie**</span><span class="sxs-lookup"><span data-stu-id="bb080-725">**Exponential policy**</span></span> 

| <span data-ttu-id="bb080-726">**Einstellung**</span><span class="sxs-lookup"><span data-stu-id="bb080-726">**Setting**</span></span> | <span data-ttu-id="bb080-727">**Standardwert**</span><span class="sxs-lookup"><span data-stu-id="bb080-727">**Default value**</span></span> | <span data-ttu-id="bb080-728">**Bedeutung**</span><span class="sxs-lookup"><span data-stu-id="bb080-728">**Meaning**</span></span> |
| --- | --- | --- |
| <span data-ttu-id="bb080-729">maxAttempt</span><span class="sxs-lookup"><span data-stu-id="bb080-729">maxAttempt</span></span> | <span data-ttu-id="bb080-730">3</span><span class="sxs-lookup"><span data-stu-id="bb080-730">3</span></span> | <span data-ttu-id="bb080-731">Anzahl der Wiederholungsversuche.</span><span class="sxs-lookup"><span data-stu-id="bb080-731">Number of retry attempts.</span></span> |
| <span data-ttu-id="bb080-732">deltaBackoff</span><span class="sxs-lookup"><span data-stu-id="bb080-732">deltaBackoff</span></span> | <span data-ttu-id="bb080-733">4 Sekunden</span><span class="sxs-lookup"><span data-stu-id="bb080-733">4 seconds</span></span> | <span data-ttu-id="bb080-734">Backoff-Intervall zwischen den Wiederholungen.</span><span class="sxs-lookup"><span data-stu-id="bb080-734">Back-off interval between retries.</span></span> <span data-ttu-id="bb080-735">Vielfache dieser Zeitspanne, einschließlich eines Zufallselements, werden für weitere Wiederholungsversuche verwendet.</span><span class="sxs-lookup"><span data-stu-id="bb080-735">Multiples of this timespan, including a random element, will be used for subsequent retry attempts.</span></span> |
| <span data-ttu-id="bb080-736">MinBackoff</span><span class="sxs-lookup"><span data-stu-id="bb080-736">MinBackoff</span></span> | <span data-ttu-id="bb080-737">3 Sekunden</span><span class="sxs-lookup"><span data-stu-id="bb080-737">3 seconds</span></span> | <span data-ttu-id="bb080-738">Wird zu allen Wiederholungsintervallen, die aus deltaBackoff berechnet werden, hinzugefügt.</span><span class="sxs-lookup"><span data-stu-id="bb080-738">Added to all retry intervals computed from deltaBackoff.</span></span> <span data-ttu-id="bb080-739">Dieser Wert kann nicht geändert werden.</span><span class="sxs-lookup"><span data-stu-id="bb080-739">This value cannot be changed.</span></span>
| <span data-ttu-id="bb080-740">MaxBackoff</span><span class="sxs-lookup"><span data-stu-id="bb080-740">MaxBackoff</span></span> | <span data-ttu-id="bb080-741">120 Sekunden</span><span class="sxs-lookup"><span data-stu-id="bb080-741">120 seconds</span></span> | <span data-ttu-id="bb080-742">MaxBackoff wird verwendet, wenn das berechnete Wiederholungsintervall größer als MaxBackoff ist.</span><span class="sxs-lookup"><span data-stu-id="bb080-742">MaxBackoff is used if the computed retry interval is greater than MaxBackoff.</span></span> <span data-ttu-id="bb080-743">Dieser Wert kann nicht geändert werden.</span><span class="sxs-lookup"><span data-stu-id="bb080-743">This value cannot be changed.</span></span> |

<span data-ttu-id="bb080-744">**Lineare Richtlinie**</span><span class="sxs-lookup"><span data-stu-id="bb080-744">**Linear policy**</span></span>

| <span data-ttu-id="bb080-745">**Einstellung**</span><span class="sxs-lookup"><span data-stu-id="bb080-745">**Setting**</span></span> | <span data-ttu-id="bb080-746">**Standardwert**</span><span class="sxs-lookup"><span data-stu-id="bb080-746">**Default value**</span></span> | <span data-ttu-id="bb080-747">**Bedeutung**</span><span class="sxs-lookup"><span data-stu-id="bb080-747">**Meaning**</span></span> |
| --- | --- | --- |
| <span data-ttu-id="bb080-748">maxAttempt</span><span class="sxs-lookup"><span data-stu-id="bb080-748">maxAttempt</span></span> | <span data-ttu-id="bb080-749">3</span><span class="sxs-lookup"><span data-stu-id="bb080-749">3</span></span> | <span data-ttu-id="bb080-750">Anzahl der Wiederholungsversuche.</span><span class="sxs-lookup"><span data-stu-id="bb080-750">Number of retry attempts.</span></span> |
| <span data-ttu-id="bb080-751">deltaBackoff</span><span class="sxs-lookup"><span data-stu-id="bb080-751">deltaBackoff</span></span> | <span data-ttu-id="bb080-752">30 Sekunden</span><span class="sxs-lookup"><span data-stu-id="bb080-752">30 seconds</span></span> | <span data-ttu-id="bb080-753">Backoff-Intervall zwischen den Wiederholungen.</span><span class="sxs-lookup"><span data-stu-id="bb080-753">Back-off interval between retries.</span></span> |

### <a name="retry-usage-guidance"></a><span data-ttu-id="bb080-754">Gebrauchsanleitung Wiederholungen</span><span class="sxs-lookup"><span data-stu-id="bb080-754">Retry usage guidance</span></span>
<span data-ttu-id="bb080-755">Beachten Sie die folgenden Richtlinien beim Zugriff auf Azure-Speicherdienste mit der Speicherclient-API:</span><span class="sxs-lookup"><span data-stu-id="bb080-755">Consider the following guidelines when accessing Azure storage services using the storage client API:</span></span>

* <span data-ttu-id="bb080-756">Verwenden der integrierten Wiederholungsrichtlinien aus dem Microsoft.WindowsAzure.Storage.RetryPolicies-Namespace, wo sie für Ihre Anforderungen geeignet sind.</span><span class="sxs-lookup"><span data-stu-id="bb080-756">Use the built-in retry policies from the Microsoft.WindowsAzure.Storage.RetryPolicies namespace where they are appropriate for your requirements.</span></span> <span data-ttu-id="bb080-757">In den meisten Fällen sind diese Richtlinien ausreichend.</span><span class="sxs-lookup"><span data-stu-id="bb080-757">In most cases, these policies will be sufficient.</span></span>
* <span data-ttu-id="bb080-758">Verwenden der **ExponentialRetry** Richtlinie in Batchvorgängen, Hintergrundaufgaben oder nicht interaktiven Szenarios.</span><span class="sxs-lookup"><span data-stu-id="bb080-758">Use the **ExponentialRetry** policy in batch operations, background tasks, or non-interactive scenarios.</span></span> <span data-ttu-id="bb080-759">In diesen Szenarien können Sie in der Regel dem Dienst mehr Zeit für die Wiederherstellung  zur Verfügung stellen - dadurch erhöht sich die Wahrscheinlichkeit, dass der Vorgang schließlich erfolgreich ist.</span><span class="sxs-lookup"><span data-stu-id="bb080-759">In these scenarios, you can typically allow more time for the service to recover—with a consequently increased chance of the operation eventually succeeding.</span></span>
* <span data-ttu-id="bb080-760">Geben Sie die Eigenschaft **MaximumExecutionTime** des **RequestOptions**-Parameters an, um die Gesamtausführungszeit zu beschränken. Berücksichtigen Sie bei der Auswahl eines Timeoutwerts aber den Typ und die Größe des Vorgangs.</span><span class="sxs-lookup"><span data-stu-id="bb080-760">Consider specifying the **MaximumExecutionTime** property of the **RequestOptions** parameter to limit the total execution time, but take into account the type and size of the operation when choosing a timeout value.</span></span>
* <span data-ttu-id="bb080-761">Wenn Sie eine benutzerdefinierte Wiederholung implementieren müssen, vermeiden Sie das Erstellen von Wrappern um die Speicherclienten-Klassen.</span><span class="sxs-lookup"><span data-stu-id="bb080-761">If you need to implement a custom retry, avoid creating wrappers around the storage client classes.</span></span> <span data-ttu-id="bb080-762">Verwenden Sie stattdessen die Funktionen zum Erweitern der vorhandenen Richtlinien über die **IExtendedRetryPolicy** Schnittstelle.</span><span class="sxs-lookup"><span data-stu-id="bb080-762">Instead, use the capabilities to extend the existing policies through the **IExtendedRetryPolicy** interface.</span></span>
* <span data-ttu-id="bb080-763">Bei Verwendung des Lesezugriffs auf georedundante Speicher (RA-GRS) können Sie **LocationMode** verwenden, um zu bestimmen, dass Wiederholungsversuche nicht auf die sekundäre schreibgeschützte Kopie des Speichers zugreifen sollen, wenn der primäre Zugriff fehlschlägt.</span><span class="sxs-lookup"><span data-stu-id="bb080-763">If you are using read access geo-redundant storage (RA-GRS) you can use the **LocationMode** to specify that retry attempts will access the secondary read-only copy of the store should the primary access fail.</span></span> <span data-ttu-id="bb080-764">Bei Verwendung dieser Option müssen Sie jedoch sicherstellen, dass die Anwendung erfolgreich mit Daten arbeiten kann, die möglicherweise veraltet sind, wenn die Replikation vom primären Speicher noch nicht abgeschlossen wurde.</span><span class="sxs-lookup"><span data-stu-id="bb080-764">However, when using this option you must ensure that your application can work successfully with data that may be stale if the replication from the primary store has not yet completed.</span></span>

<span data-ttu-id="bb080-765">Erwägen Sie, mit den folgenden Einstellungen für Wiederholungsvorgänge zu beginnen.</span><span class="sxs-lookup"><span data-stu-id="bb080-765">Consider starting with following settings for retrying operations.</span></span> <span data-ttu-id="bb080-766">Hierbei handelt es sich um allgemeine Einstellungen und Sie sollten die Vorgänge überwachen und die Werte entsprechend Ihrem Szenario optimieren.</span><span class="sxs-lookup"><span data-stu-id="bb080-766">These are general purpose settings, and you should monitor the operations and fine tune the values to suit your own scenario.</span></span>  

| <span data-ttu-id="bb080-767">**Context**</span><span class="sxs-lookup"><span data-stu-id="bb080-767">**Context**</span></span> | <span data-ttu-id="bb080-768">**Beispiel-Ziel E2E<br />Maximale Wartezeit**</span><span class="sxs-lookup"><span data-stu-id="bb080-768">**Sample target E2E<br />max latency**</span></span> | <span data-ttu-id="bb080-769">**Wiederholungsrichtlinie**</span><span class="sxs-lookup"><span data-stu-id="bb080-769">**Retry policy**</span></span> | <span data-ttu-id="bb080-770">**Einstellungen**</span><span class="sxs-lookup"><span data-stu-id="bb080-770">**Settings**</span></span> | <span data-ttu-id="bb080-771">**Werte**</span><span class="sxs-lookup"><span data-stu-id="bb080-771">**Values**</span></span> | <span data-ttu-id="bb080-772">**So funktioniert's**</span><span class="sxs-lookup"><span data-stu-id="bb080-772">**How it works**</span></span> |
| --- | --- | --- | --- | --- | --- |
| <span data-ttu-id="bb080-773">Interaktiv, Benutzeroberfläche</span><span class="sxs-lookup"><span data-stu-id="bb080-773">Interactive, UI,</span></span><br /><span data-ttu-id="bb080-774">oder Vordergrund</span><span class="sxs-lookup"><span data-stu-id="bb080-774">or foreground</span></span> |<span data-ttu-id="bb080-775">2 Sekunden</span><span class="sxs-lookup"><span data-stu-id="bb080-775">2 seconds</span></span> |<span data-ttu-id="bb080-776">Linear</span><span class="sxs-lookup"><span data-stu-id="bb080-776">Linear</span></span> |<span data-ttu-id="bb080-777">maxAttempt</span><span class="sxs-lookup"><span data-stu-id="bb080-777">maxAttempt</span></span><br /><span data-ttu-id="bb080-778">deltaBackoff</span><span class="sxs-lookup"><span data-stu-id="bb080-778">deltaBackoff</span></span> |<span data-ttu-id="bb080-779">3</span><span class="sxs-lookup"><span data-stu-id="bb080-779">3</span></span><br /><span data-ttu-id="bb080-780">500 ms</span><span class="sxs-lookup"><span data-stu-id="bb080-780">500 ms</span></span> |<span data-ttu-id="bb080-781">Versuch 1 – Verzögerung 500 ms</span><span class="sxs-lookup"><span data-stu-id="bb080-781">Attempt 1 - delay 500 ms</span></span><br /><span data-ttu-id="bb080-782">Versuch 2 – Verzögerung 500 ms</span><span class="sxs-lookup"><span data-stu-id="bb080-782">Attempt 2 - delay 500 ms</span></span><br /><span data-ttu-id="bb080-783">Versuch 3 – Verzögerung 500 ms</span><span class="sxs-lookup"><span data-stu-id="bb080-783">Attempt 3 - delay 500 ms</span></span> |
| <span data-ttu-id="bb080-784">Hintergrund</span><span class="sxs-lookup"><span data-stu-id="bb080-784">Background</span></span><br /><span data-ttu-id="bb080-785">oder Batch</span><span class="sxs-lookup"><span data-stu-id="bb080-785">or batch</span></span> |<span data-ttu-id="bb080-786">30 Sekunden</span><span class="sxs-lookup"><span data-stu-id="bb080-786">30 seconds</span></span> |<span data-ttu-id="bb080-787">Exponentiell</span><span class="sxs-lookup"><span data-stu-id="bb080-787">Exponential</span></span> |<span data-ttu-id="bb080-788">maxAttempt</span><span class="sxs-lookup"><span data-stu-id="bb080-788">maxAttempt</span></span><br /><span data-ttu-id="bb080-789">deltaBackoff</span><span class="sxs-lookup"><span data-stu-id="bb080-789">deltaBackoff</span></span> |<span data-ttu-id="bb080-790">5</span><span class="sxs-lookup"><span data-stu-id="bb080-790">5</span></span><br /><span data-ttu-id="bb080-791">4 Sekunden</span><span class="sxs-lookup"><span data-stu-id="bb080-791">4 seconds</span></span> |<span data-ttu-id="bb080-792">Versuch 1 – Verzögerung ca. 3 Sek.</span><span class="sxs-lookup"><span data-stu-id="bb080-792">Attempt 1 - delay ~3 sec</span></span><br /><span data-ttu-id="bb080-793">Versuch 2 – Verzögerung ca. 7 Sek.</span><span class="sxs-lookup"><span data-stu-id="bb080-793">Attempt 2 - delay ~7 sec</span></span><br /><span data-ttu-id="bb080-794">Versuch 3 – Verzögerung ca. 15 Sek.</span><span class="sxs-lookup"><span data-stu-id="bb080-794">Attempt 3 - delay ~15 sec</span></span> |

### <a name="telemetry"></a><span data-ttu-id="bb080-795">Telemetrie</span><span class="sxs-lookup"><span data-stu-id="bb080-795">Telemetry</span></span>
<span data-ttu-id="bb080-796">Wiederholungsversuche werden in einer **TraceSource** protokolliert.</span><span class="sxs-lookup"><span data-stu-id="bb080-796">Retry attempts are logged to a **TraceSource**.</span></span> <span data-ttu-id="bb080-797">Sie müssen einen **TraceListener** konfigurieren, um die Ereignisse zu erfassen und diese in ein geeignetes Zielprotokoll zu schreiben.</span><span class="sxs-lookup"><span data-stu-id="bb080-797">You must configure a **TraceListener** to capture the events and write them to a suitable destination log.</span></span> <span data-ttu-id="bb080-798">Sie können **TextWriterTraceListener** oder **XmlWriterTraceListener** zum Schreiben der Daten in eine Protokolldatei, **EventLogTraceListener** zum Schreiben in ein Windows-Ereignisprotokoll oder **EventProviderTraceListener** zum Schreiben von Daten in das ETW-Subsystem verwenden.</span><span class="sxs-lookup"><span data-stu-id="bb080-798">You can use the **TextWriterTraceListener** or **XmlWriterTraceListener** to write the data to a log file, the **EventLogTraceListener** to write to the Windows Event Log, or the **EventProviderTraceListener** to write trace data to the ETW subsystem.</span></span> <span data-ttu-id="bb080-799">Sie können auch automatisches Leeren des Puffers und den Ausführlichkeitsgrad der Ereignisse konfigurieren, die protokolliert werden (z. B. Fehler, Warnung, Information und ausführlich).</span><span class="sxs-lookup"><span data-stu-id="bb080-799">You can also configure auto-flushing of the buffer, and the verbosity of events that will be logged (for example, Error, Warning, Informational, and Verbose).</span></span> <span data-ttu-id="bb080-800">Weitere Informationen finden Sie unter [Clientseitige Protokollierung mit der .NET Storage Client Library](http://msdn.microsoft.com/library/azure/dn782839.aspx).</span><span class="sxs-lookup"><span data-stu-id="bb080-800">For more information, see [Client-side Logging with the .NET Storage Client Library](http://msdn.microsoft.com/library/azure/dn782839.aspx).</span></span>

<span data-ttu-id="bb080-801">Vorgänge können eine **OperationContext**-Instanz erhalten, die ein **Retrying**-Ereignis verfügbar macht, das zum Anfügen von benutzerdefinierter Telemetrielogik verwendet werden kann.</span><span class="sxs-lookup"><span data-stu-id="bb080-801">Operations can receive an **OperationContext** instance, which exposes a **Retrying** event that can be used to attach custom telemetry logic.</span></span> <span data-ttu-id="bb080-802">Weitere Informationen finden Sie unter [OperationContext.Ereignis Erneuter Versuch](http://msdn.microsoft.com/library/microsoft.windowsazure.storage.operationcontext.retrying.aspx).</span><span class="sxs-lookup"><span data-stu-id="bb080-802">For more information, see [OperationContext.Retrying Event](http://msdn.microsoft.com/library/microsoft.windowsazure.storage.operationcontext.retrying.aspx).</span></span>

### <a name="examples"></a><span data-ttu-id="bb080-803">Beispiele</span><span class="sxs-lookup"><span data-stu-id="bb080-803">Examples</span></span>
<span data-ttu-id="bb080-804">Im folgenden Codebeispiel wird veranschaulicht, wie Sie zwei **TableRequestOptions** -Instanzen mit verschiedenen Wiederholungseinstellungen erstellen; eine für interaktive Anforderungen und eine für Anforderungen im Hintergrund.</span><span class="sxs-lookup"><span data-stu-id="bb080-804">The following code example shows how to create two **TableRequestOptions** instances with different retry settings; one for interactive requests and one for background requests.</span></span> <span data-ttu-id="bb080-805">Im Beispiel werden dann diese beiden Richtlinien auf dem Client festgelegt, sodass sie für alle Anforderungen gelten und außerdem wird die interaktive Strategie für eine bestimmte Anforderung festgelegt, damit sie die auf den Client angewandten Standardeinstellungen überschreibt.</span><span class="sxs-lookup"><span data-stu-id="bb080-805">The example then sets these two retry policies on the client so that they apply for all requests, and also sets the interactive strategy on a specific request so that it overrides the default settings applied to the client.</span></span>

```csharp
using System;
using System.Threading.Tasks;
using Microsoft.WindowsAzure.Storage;
using Microsoft.WindowsAzure.Storage.RetryPolicies;
using Microsoft.WindowsAzure.Storage.Table;

namespace RetryCodeSamples
{
    class AzureStorageCodeSamples
    {
        private const string connectionString = "UseDevelopmentStorage=true";

        public async static Task Samples()
        {
            var storageAccount = CloudStorageAccount.Parse(connectionString);

            TableRequestOptions interactiveRequestOption = new TableRequestOptions()
            {
                RetryPolicy = new LinearRetry(TimeSpan.FromMilliseconds(500), 3),
                // For Read-access geo-redundant storage, use PrimaryThenSecondary.
                // Otherwise set this to PrimaryOnly.
                LocationMode = LocationMode.PrimaryThenSecondary,
                // Maximum execution time based on the business use case. 
                MaximumExecutionTime = TimeSpan.FromSeconds(2)
            };

            TableRequestOptions backgroundRequestOption = new TableRequestOptions()
            {
                // Client has a default exponential retry policy with 4 sec delay and 3 retry attempts
                // Retry delays will be approximately 3 sec, 7 sec, and 15 sec
                MaximumExecutionTime = TimeSpan.FromSeconds(30),
                // PrimaryThenSecondary in case of Read-access geo-redundant storage, else set this to PrimaryOnly
                LocationMode = LocationMode.PrimaryThenSecondary
            };

            var client = storageAccount.CreateCloudTableClient();
            // Client has a default exponential retry policy with 4 sec delay and 3 retry attempts
            // Retry delays will be approximately 3 sec, 7 sec, and 15 sec
            // ServerTimeout and MaximumExecutionTime are not set

            {
                // Set properties for the client (used on all requests unless overridden)
                // Different exponential policy parameters for background scenarios
                client.DefaultRequestOptions = backgroundRequestOption;
                // Linear policy for interactive scenarios
                client.DefaultRequestOptions = interactiveRequestOption;
            }

            {
                // set properties for a specific request
                var stats = await client.GetServiceStatsAsync(interactiveRequestOption, operationContext: null);
            }

            {
                // Set up notifications for an operation
                var context = new OperationContext();
                context.ClientRequestID = "some request id";
                context.Retrying += (sender, args) =>
                {
                    /* Collect retry information */
                };
                context.RequestCompleted += (sender, args) =>
                {
                    /* Collect operation completion information */
                };
                var stats = await client.GetServiceStatsAsync(null, context);
            }
        }
    }
}
```

### <a name="more-information"></a><span data-ttu-id="bb080-806">Weitere Informationen</span><span class="sxs-lookup"><span data-stu-id="bb080-806">More information</span></span>
* [<span data-ttu-id="bb080-807">Empfehlungen zur Azure Storage-Clientbibliothek Wiederholungsrichtlinie</span><span class="sxs-lookup"><span data-stu-id="bb080-807">Azure Storage Client Library Retry Policy Recommendations</span></span>](https://azure.microsoft.com/blog/2014/05/22/azure-storage-client-library-retry-policy-recommendations/)
* [<span data-ttu-id="bb080-808">Storage Client Library 2.0 – Implementieren von Wiederholungsrichtlinien</span><span class="sxs-lookup"><span data-stu-id="bb080-808">Storage Client Library 2.0 – Implementing Retry Policies</span></span>](http://gauravmantri.com/2012/12/30/storage-client-library-2-0-implementing-retry-policies/)

## <a name="general-rest-and-retry-guidelines"></a><span data-ttu-id="bb080-809">Allgemeine REST-und Wiederholungsrichtlinien</span><span class="sxs-lookup"><span data-stu-id="bb080-809">General REST and retry guidelines</span></span>
<span data-ttu-id="bb080-810">Berücksichtigen Sie beim Zugriff auf die Dienste von Azure oder von Drittanbietern Folgendes:</span><span class="sxs-lookup"><span data-stu-id="bb080-810">Consider the following when accessing Azure or third party services:</span></span>

* <span data-ttu-id="bb080-811">Verwenden Sie einen systematischen Ansatz zur Verwaltung von Wiederholungen, vielleicht als wiederverwendbaren Code, sodass Sie eine einheitliche Methodik für alle Clients und alle Lösungen anwenden können.</span><span class="sxs-lookup"><span data-stu-id="bb080-811">Use a systematic approach to managing retries, perhaps as reusable code, so that you can apply a consistent methodology across all clients and all solutions.</span></span>
* <span data-ttu-id="bb080-812">Erwägen Sie die Verwendung eines Wiederholungsframeworks, wie z. B. den Anwendungsblock zur Handhabung vorübergehender Fehler, um Wiederholungen zu verwalten, wenn der Zieldienst oder der Client nicht über integrierte Wiederholungsmechanismen verfügt.</span><span class="sxs-lookup"><span data-stu-id="bb080-812">Consider using a retry framework such as the Transient Fault Handling Application Block to manage retries if the target service or client has no built-in retry mechanism.</span></span> <span data-ttu-id="bb080-813">Dies hilft Ihnen bei der Implementierung eines konsistenten Wiederholungsverhaltens, und kann eine geeignete Standardwiederholungsstrategie für den Zieldienst bereitstellen.</span><span class="sxs-lookup"><span data-stu-id="bb080-813">This will help you implement a consistent retry behavior, and it may provide a suitable default retry strategy for the target service.</span></span> <span data-ttu-id="bb080-814">Sie müssen aber unter Umständen einen benutzerdefinierten Wiederholungscode für Dienste erstellen, die kein Standardverhalten aufweisen und nicht auf Ausnahmen vertrauen, um vorübergehende Fehler anzuzeigen, oder wenn Sie eine **Retry-Response**-Antwort verwenden möchten, um das Wiederholungsverhalten zu verwalten.</span><span class="sxs-lookup"><span data-stu-id="bb080-814">However, you may need to create custom retry code for services that have non-standard behavior, that do not rely on exceptions to indicate transient failures, or if you want to use a **Retry-Response** reply to manage retry behavior.</span></span>
* <span data-ttu-id="bb080-815">Die vorübergehende Erkennungslogik hängt von der tatsächlichen Client-API ab, die Sie verwenden, um die REST-Aufrufe aufzurufen.</span><span class="sxs-lookup"><span data-stu-id="bb080-815">The transient detection logic will depend on the actual client API you use to invoke the REST calls.</span></span> <span data-ttu-id="bb080-816">Einige Clients, z.B. die neuere **HttpClient**-Klasse, lösen keine Ausnahmen für abgeschlossene Anforderungen mit einem nicht erfolgreichen HTTP-Statuscode aus.</span><span class="sxs-lookup"><span data-stu-id="bb080-816">Some clients, such as the newer **HttpClient** class, will not throw exceptions for completed requests with a non-success HTTP status code.</span></span> <span data-ttu-id="bb080-817">Dies verbessert die Leistung, verhindert jedoch die Verwendung des Anwendungsblocks zur Handhabung vorübergehender Fehler.</span><span class="sxs-lookup"><span data-stu-id="bb080-817">This improves performance but prevents the use of the Transient Fault Handling Application Block.</span></span> <span data-ttu-id="bb080-818">In diesem Fall können Sie den Aufruf der REST-API mit einem Code umschließen, der Ausnahmen für nicht erfolgreiche HTTP-Statuscodes erzeugt, die dann vom Block verarbeitet werden können.</span><span class="sxs-lookup"><span data-stu-id="bb080-818">In this case you could wrap the call to the REST API with code that produces exceptions for non-success HTTP status codes, which can then be processed by the block.</span></span> <span data-ttu-id="bb080-819">Alternativ können Sie einen anderen Mechanismus für die Durchführung der Wiederholungen verwenden.</span><span class="sxs-lookup"><span data-stu-id="bb080-819">Alternatively, you can use a different mechanism to drive the retries.</span></span>
* <span data-ttu-id="bb080-820">Der vom Dienst zurückgegebene HTTP-Statuscode kann dabei helfen, zu erkennen, ob der Fehler vorübergehend ist.</span><span class="sxs-lookup"><span data-stu-id="bb080-820">The HTTP status code returned from the service can help to indicate whether the failure is transient.</span></span> <span data-ttu-id="bb080-821">Möglicherweise müssen Sie die Ausnahmen, die von einem Client oder dem Wiederholungsframework erzeugt wurden untersuchen, um auf den Statuscode zuzugreifen oder um den entsprechenden Ausnahmetyp bestimmen zu können.</span><span class="sxs-lookup"><span data-stu-id="bb080-821">You may need to examine the exceptions generated by a client or the retry framework to access the status code or to determine the equivalent exception type.</span></span> <span data-ttu-id="bb080-822">Die folgenden HTTP-Codes geben in der Regel an, dass eine Wiederholung geeignet ist:</span><span class="sxs-lookup"><span data-stu-id="bb080-822">The following HTTP codes typically indicate that a retry is appropriate:</span></span>
  * <span data-ttu-id="bb080-823">408 Anforderungstimeout</span><span class="sxs-lookup"><span data-stu-id="bb080-823">408 Request Timeout</span></span>
  * <span data-ttu-id="bb080-824">429 – Zu viele Anforderungen</span><span class="sxs-lookup"><span data-stu-id="bb080-824">429 Too Many Requests</span></span>
  * <span data-ttu-id="bb080-825">500 Interner Serverfehler</span><span class="sxs-lookup"><span data-stu-id="bb080-825">500 Internal Server Error</span></span>
  * <span data-ttu-id="bb080-826">502 Ungültiges Gateway</span><span class="sxs-lookup"><span data-stu-id="bb080-826">502 Bad Gateway</span></span>
  * <span data-ttu-id="bb080-827">503 Dienst nicht verfügbar</span><span class="sxs-lookup"><span data-stu-id="bb080-827">503 Service Unavailable</span></span>
  * <span data-ttu-id="bb080-828">504 Gateway-Timeout</span><span class="sxs-lookup"><span data-stu-id="bb080-828">504 Gateway Timeout</span></span>
* <span data-ttu-id="bb080-829">Wenn Sie Ihre Wiederholungslogik auf Ausnahmen basieren, wird in der Regel durch Folgendes angezeigt, dass ein vorübergehender Fehler besteht, da keine Verbindung hergestellt werden konnte:</span><span class="sxs-lookup"><span data-stu-id="bb080-829">If you base your retry logic on exceptions, the following typically indicate a transient failure where no connection could be established:</span></span>
  * <span data-ttu-id="bb080-830">WebExceptionStatus.ConnectionClosed</span><span class="sxs-lookup"><span data-stu-id="bb080-830">WebExceptionStatus.ConnectionClosed</span></span>
  * <span data-ttu-id="bb080-831">WebExceptionStatus.ConnectFailure</span><span class="sxs-lookup"><span data-stu-id="bb080-831">WebExceptionStatus.ConnectFailure</span></span>
  * <span data-ttu-id="bb080-832">WebExceptionStatus.Timeout</span><span class="sxs-lookup"><span data-stu-id="bb080-832">WebExceptionStatus.Timeout</span></span>
  * <span data-ttu-id="bb080-833">WebExceptionStatus.RequestCanceled</span><span class="sxs-lookup"><span data-stu-id="bb080-833">WebExceptionStatus.RequestCanceled</span></span>
* <span data-ttu-id="bb080-834">Im Fall des Status „Dienst nicht verfügbar“ weist der Dienst unter Umständen auf die entsprechende Verzögerung vor der Wiederholung im **Retry-After** -Antwort-Header oder in einem anderen benutzerdefinierten Header hin.</span><span class="sxs-lookup"><span data-stu-id="bb080-834">In the case of a service unavailable status, the service might indicate the appropriate delay before retrying in the **Retry-After** response header or a different custom header.</span></span> <span data-ttu-id="bb080-835">Dienste können auch zusätzliche Informationen als benutzerdefinierte Header oder in den Inhalt der Antwort eingebettet senden.</span><span class="sxs-lookup"><span data-stu-id="bb080-835">Services might also send additional information as custom headers, or embedded in the content of the response.</span></span> <span data-ttu-id="bb080-836">Der Anwendungsblock zur Handhabung vorübergehender Fehler kann keine Standard- oder benutzerdefinierte „Retry-after“-Header verwenden.</span><span class="sxs-lookup"><span data-stu-id="bb080-836">The Transient Fault Handling Application Block cannot use the standard or any custom “retry-after” headers.</span></span>
* <span data-ttu-id="bb080-837">Führen Sie keine Wiederholung für Statuscodes durch, die Clientfehler (Fehler im Bereich 4xx) darstellen, mit Ausnahme eines Anforderungstimeouts 408.</span><span class="sxs-lookup"><span data-stu-id="bb080-837">Do not retry for status codes representing client errors (errors in the 4xx range) except for a 408 Request Timeout.</span></span>
* <span data-ttu-id="bb080-838">Testen Sie Ihre Wiederholungsstrategien und Mechanismen gründlich unter verschiedenen Bedingungen, wie z. B. verschiedenen Netzwerkzuständen und sich ändernden Systemlastverteilungen.</span><span class="sxs-lookup"><span data-stu-id="bb080-838">Thoroughly test your retry strategies and mechanisms under a range of conditions, such as different network states and varying system loadings.</span></span>

### <a name="retry-strategies"></a><span data-ttu-id="bb080-839">Wiederholungsstrategien</span><span class="sxs-lookup"><span data-stu-id="bb080-839">Retry strategies</span></span>
<span data-ttu-id="bb080-840">Im Folgenden sind die typischen Arten von Wiederholungsstrategie-Intervallen dargestellt:</span><span class="sxs-lookup"><span data-stu-id="bb080-840">The following are the typical types of retry strategy intervals:</span></span>

* <span data-ttu-id="bb080-841">**Exponential:**</span><span class="sxs-lookup"><span data-stu-id="bb080-841">**Exponential**.</span></span> <span data-ttu-id="bb080-842">Eine Wiederholungsrichtlinie, mit der eine angegebene Anzahl von Wiederholungsversuchen durchgeführt wird, bei der ein zufälliger exponentieller Backoff-Ansatz zur Ermittlung des Intervalls zwischen den Wiederholungsversuchen verwendet wird.</span><span class="sxs-lookup"><span data-stu-id="bb080-842">A retry policy that performs a specified number of retries, using a randomized exponential back off approach to determine the interval between retries.</span></span> <span data-ttu-id="bb080-843">Beispiel: </span><span class="sxs-lookup"><span data-stu-id="bb080-843">For example:</span></span>

    ```csharp
    var random = new Random();

    var delta = (int)((Math.Pow(2.0, currentRetryCount) - 1.0) *
                random.Next((int)(this.deltaBackoff.TotalMilliseconds * 0.8),
                (int)(this.deltaBackoff.TotalMilliseconds * 1.2)));
    var interval = (int)Math.Min(checked(this.minBackoff.TotalMilliseconds + delta),
                    this.maxBackoff.TotalMilliseconds);
    retryInterval = TimeSpan.FromMilliseconds(interval);
    ```

* <span data-ttu-id="bb080-844">**Incremental:**</span><span class="sxs-lookup"><span data-stu-id="bb080-844">**Incremental**.</span></span> <span data-ttu-id="bb080-845">Eine Wiederholungsstrategie mit einer angegebenen Anzahl von Wiederholungsversuchen und einem inkrementellen Zeitintervall zwischen den Wiederholungen.</span><span class="sxs-lookup"><span data-stu-id="bb080-845">A retry strategy with a specified number of retry attempts and an incremental time interval between retries.</span></span> <span data-ttu-id="bb080-846">Beispiel: </span><span class="sxs-lookup"><span data-stu-id="bb080-846">For example:</span></span>

    ```csharp
    retryInterval = TimeSpan.FromMilliseconds(this.initialInterval.TotalMilliseconds +
                    (this.increment.TotalMilliseconds * currentRetryCount));
    ```

* <span data-ttu-id="bb080-847">**LinearRetry:**</span><span class="sxs-lookup"><span data-stu-id="bb080-847">**LinearRetry**.</span></span> <span data-ttu-id="bb080-848">Eine Wiederholungsrichtlinie, bei der eine angegebene Anzahl von Wiederholungen durchgeführt und ein angegebenes festes Zeitintervall zwischen den Wiederholungen verwendet wird.</span><span class="sxs-lookup"><span data-stu-id="bb080-848">A retry policy that performs a specified number of retries, using a specified fixed time interval between retries.</span></span> <span data-ttu-id="bb080-849">Beispiel: </span><span class="sxs-lookup"><span data-stu-id="bb080-849">For example:</span></span>

    ```csharp
    retryInterval = this.deltaBackoff;
    ```

### <a name="transient-fault-handling-with-polly"></a><span data-ttu-id="bb080-850">Behandeln von vorübergehenden Fehlern mit Polly</span><span class="sxs-lookup"><span data-stu-id="bb080-850">Transient fault handling with Polly</span></span>
<span data-ttu-id="bb080-851">[Polly][polly] ist eine Bibliothek zur programmgesteuerten Behandlung von Wiederholungsversuchen und [Schutzschalter][circuit-breaker]-Strategien.</span><span class="sxs-lookup"><span data-stu-id="bb080-851">[Polly][polly] is a library to programatically handle retries and [circuit breaker][circuit-breaker] strategies.</span></span> <span data-ttu-id="bb080-852">Das Polly-Projekt ist ein Mitglied der [.NET Foundation][dotnet-foundation].</span><span class="sxs-lookup"><span data-stu-id="bb080-852">The Polly project is a member of the [.NET Foundation][dotnet-foundation].</span></span> <span data-ttu-id="bb080-853">Für Dienste, bei denen der Client Wiederholungsversuche nicht nativ unterstützt, ist Polly eine zulässige Alternative. Es ist hierbei nicht erforderlich, benutzerdefinierten Code für die Wiederholungsversuche zu schreiben, dessen richtige Implementierung schwierig sein kann.</span><span class="sxs-lookup"><span data-stu-id="bb080-853">For services where the client does not natively support retries, Polly is a valid alternative and avoids the need to write custom retry code, which can be hard to implement correctly.</span></span> <span data-ttu-id="bb080-854">Polly bietet auch eine Möglichkeit zum Überwachen des Auftretens von Fehlern, damit Sie Wiederholungsversuche protokollieren können.</span><span class="sxs-lookup"><span data-stu-id="bb080-854">Polly also provides a way to trace errors when they occur, so that you can log retries.</span></span>


<!-- links -->

[adal]: /azure/active-directory/develop/active-directory-authentication-libraries
[autorest]: https://github.com/Azure/autorest/tree/master/docs
[circuit-breaker]: ../patterns/circuit-breaker.md
[ConnectionPolicy.RetryOptions]: https://msdn.microsoft.com/library/azure/microsoft.azure.documents.client.connectionpolicy.retryoptions.aspx
[dotnet-foundation]: https://dotnetfoundation.org/
[polly]: http://www.thepollyproject.org
[redis-cache-troubleshoot]: /azure/redis-cache/cache-how-to-troubleshoot
[SearchIndexClient]: https://msdn.microsoft.com/library/azure/microsoft.azure.search.searchindexclient.aspx
[SearchServiceClient]: https://msdn.microsoft.com/library/microsoft.azure.search.searchserviceclient.aspx


### <a name="more-information"></a><span data-ttu-id="bb080-858">Weitere Informationen</span><span class="sxs-lookup"><span data-stu-id="bb080-858">More information</span></span>
* [<span data-ttu-id="bb080-859">Verbindungsstabilität</span><span class="sxs-lookup"><span data-stu-id="bb080-859">Connection Resiliency</span></span>](/ef/core/miscellaneous/connection-resiliency)
* [<span data-ttu-id="bb080-860">Data Points – EF Core 1.1</span><span class="sxs-lookup"><span data-stu-id="bb080-860">Data Points - EF Core 1.1</span></span>](https://msdn.microsoft.com/magazine/mt745093.aspx)


