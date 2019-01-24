---
title: Anleitung zu dienstspezifischen Wiederholungsmechanismen
titleSuffix: Best practices for cloud applications
description: Spezifische Dienstanleitung für die Festlegung des Wiederholungsmechanismus.
author: dragon119
ms.date: 08/13/2018
ms.topic: best-practice
ms.service: architecture-center
ms.subservice: cloud-fundamentals
ms.custom: seodec18
ms.openlocfilehash: d99c63b9cb5f2ed7ffcd869b5b8ac7910b9dabe3
ms.sourcegitcommit: 1b50810208354577b00e89e5c031b774b02736e2
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/23/2019
ms.locfileid: "54487136"
---
# <a name="retry-guidance-for-specific-services"></a><span data-ttu-id="8c5ef-103">Wiederholungsanleitung für bestimmte Dienste</span><span class="sxs-lookup"><span data-stu-id="8c5ef-103">Retry guidance for specific services</span></span>

<span data-ttu-id="8c5ef-104">Die meisten Azure-Dienste und Client-SDKs enthalten einen Wiederholungsmechanismus.</span><span class="sxs-lookup"><span data-stu-id="8c5ef-104">Most Azure services and client SDKs include a retry mechanism.</span></span> <span data-ttu-id="8c5ef-105">Diese unterscheiden sich jedoch, da jeder Dienst unterschiedliche Merkmale und Anforderungen hat und somit jeder Wiederholungsmechanismus für einen bestimmten Dienst optimiert ist.</span><span class="sxs-lookup"><span data-stu-id="8c5ef-105">However, these differ because each service has different characteristics and requirements, and so each retry mechanism is tuned to a specific service.</span></span> <span data-ttu-id="8c5ef-106">Dieses Handbuch fasst die Wiederholungsmechanismusfunktionen für die Mehrzahl der Azure-Dienste zusammen und enthält Informationen, mit denen Sie die Wiederholungsmechanismus für diesen Dienst verwenden, anpassen oder erweitern können.</span><span class="sxs-lookup"><span data-stu-id="8c5ef-106">This guide summarizes the retry mechanism features for the majority of Azure services, and includes information to help you use, adapt, or extend the retry mechanism for that service.</span></span>

<span data-ttu-id="8c5ef-107">Allgemeine Anweisungen zum Behandeln von vorübergehenden Fehlern und für Wiederholungsverbindungen und Operationen für Dienste und Ressourcen finden Sie unter [Wiederholungsanleitung](./transient-faults.md).</span><span class="sxs-lookup"><span data-stu-id="8c5ef-107">For general guidance on handling transient faults, and retrying connections and operations against services and resources, see [Retry guidance](./transient-faults.md).</span></span>

<span data-ttu-id="8c5ef-108">In der folgende Tabelle werden die Wiederholungsfunktionen für die in dieser Anleitung beschriebenen Azure-Dienste zusammengefasst.</span><span class="sxs-lookup"><span data-stu-id="8c5ef-108">The following table summarizes the retry features for the Azure services described in this guidance.</span></span>

| <span data-ttu-id="8c5ef-109">**Service**</span><span class="sxs-lookup"><span data-stu-id="8c5ef-109">**Service**</span></span> | <span data-ttu-id="8c5ef-110">**Wiederholungsfunktionen**</span><span class="sxs-lookup"><span data-stu-id="8c5ef-110">**Retry capabilities**</span></span> | <span data-ttu-id="8c5ef-111">**Richtlinienkonfiguration**</span><span class="sxs-lookup"><span data-stu-id="8c5ef-111">**Policy configuration**</span></span> | <span data-ttu-id="8c5ef-112">**Umfang**</span><span class="sxs-lookup"><span data-stu-id="8c5ef-112">**Scope**</span></span> | <span data-ttu-id="8c5ef-113">**Telemetriefunktionen**</span><span class="sxs-lookup"><span data-stu-id="8c5ef-113">**Telemetry features**</span></span> |
| --- | --- | --- | --- | --- |
| <span data-ttu-id="8c5ef-114">**[Azure Active Directory](#azure-active-directory)**</span><span class="sxs-lookup"><span data-stu-id="8c5ef-114">**[Azure Active Directory](#azure-active-directory)**</span></span> |<span data-ttu-id="8c5ef-115">Nativ in ADAL-Bibliothek</span><span class="sxs-lookup"><span data-stu-id="8c5ef-115">Native in ADAL library</span></span> |<span data-ttu-id="8c5ef-116">Eingebettet in ADAL-Bibliothek</span><span class="sxs-lookup"><span data-stu-id="8c5ef-116">Embeded into ADAL library</span></span> |<span data-ttu-id="8c5ef-117">Intern</span><span class="sxs-lookup"><span data-stu-id="8c5ef-117">Internal</span></span> |<span data-ttu-id="8c5ef-118">Keine</span><span class="sxs-lookup"><span data-stu-id="8c5ef-118">None</span></span> |
| <span data-ttu-id="8c5ef-119">**[Cosmos DB](#cosmos-db)**</span><span class="sxs-lookup"><span data-stu-id="8c5ef-119">**[Cosmos DB](#cosmos-db)**</span></span> |<span data-ttu-id="8c5ef-120">Systemeigen im Dienst</span><span class="sxs-lookup"><span data-stu-id="8c5ef-120">Native in service</span></span> |<span data-ttu-id="8c5ef-121">Nicht konfigurierbar</span><span class="sxs-lookup"><span data-stu-id="8c5ef-121">Non-configurable</span></span> |<span data-ttu-id="8c5ef-122">Global</span><span class="sxs-lookup"><span data-stu-id="8c5ef-122">Global</span></span> |<span data-ttu-id="8c5ef-123">TraceSource</span><span class="sxs-lookup"><span data-stu-id="8c5ef-123">TraceSource</span></span> |
| <span data-ttu-id="8c5ef-124">**Data Lake Store**</span><span class="sxs-lookup"><span data-stu-id="8c5ef-124">**Data Lake Store**</span></span> |<span data-ttu-id="8c5ef-125">Systemeigen in Client</span><span class="sxs-lookup"><span data-stu-id="8c5ef-125">Native in client</span></span> |<span data-ttu-id="8c5ef-126">Nicht konfigurierbar</span><span class="sxs-lookup"><span data-stu-id="8c5ef-126">Non-configurable</span></span> |<span data-ttu-id="8c5ef-127">Einzelne Vorgänge</span><span class="sxs-lookup"><span data-stu-id="8c5ef-127">Individual operations</span></span> |<span data-ttu-id="8c5ef-128">Keine</span><span class="sxs-lookup"><span data-stu-id="8c5ef-128">None</span></span> |
| <span data-ttu-id="8c5ef-129">**[Event Hubs](#event-hubs)**</span><span class="sxs-lookup"><span data-stu-id="8c5ef-129">**[Event Hubs](#event-hubs)**</span></span> |<span data-ttu-id="8c5ef-130">Systemeigen in Client</span><span class="sxs-lookup"><span data-stu-id="8c5ef-130">Native in client</span></span> |<span data-ttu-id="8c5ef-131">Programmgesteuert</span><span class="sxs-lookup"><span data-stu-id="8c5ef-131">Programmatic</span></span> |<span data-ttu-id="8c5ef-132">Client</span><span class="sxs-lookup"><span data-stu-id="8c5ef-132">Client</span></span> |<span data-ttu-id="8c5ef-133">Keine</span><span class="sxs-lookup"><span data-stu-id="8c5ef-133">None</span></span> |
| <span data-ttu-id="8c5ef-134">**[IoT Hub](#iot-hub)**</span><span class="sxs-lookup"><span data-stu-id="8c5ef-134">**[IoT Hub](#iot-hub)**</span></span> |<span data-ttu-id="8c5ef-135">Nativ im Client SDK</span><span class="sxs-lookup"><span data-stu-id="8c5ef-135">Native in client SDK</span></span> |<span data-ttu-id="8c5ef-136">Programmgesteuert</span><span class="sxs-lookup"><span data-stu-id="8c5ef-136">Programmatic</span></span> |<span data-ttu-id="8c5ef-137">Client</span><span class="sxs-lookup"><span data-stu-id="8c5ef-137">Client</span></span> |<span data-ttu-id="8c5ef-138">Keine</span><span class="sxs-lookup"><span data-stu-id="8c5ef-138">None</span></span> |
| <span data-ttu-id="8c5ef-139">**[Redis Cache](#azure-redis-cache)**</span><span class="sxs-lookup"><span data-stu-id="8c5ef-139">**[Redis Cache](#azure-redis-cache)**</span></span> |<span data-ttu-id="8c5ef-140">Systemeigen in Client</span><span class="sxs-lookup"><span data-stu-id="8c5ef-140">Native in client</span></span> |<span data-ttu-id="8c5ef-141">Programmgesteuert</span><span class="sxs-lookup"><span data-stu-id="8c5ef-141">Programmatic</span></span> |<span data-ttu-id="8c5ef-142">Client</span><span class="sxs-lookup"><span data-stu-id="8c5ef-142">Client</span></span> |<span data-ttu-id="8c5ef-143">TextWriter</span><span class="sxs-lookup"><span data-stu-id="8c5ef-143">TextWriter</span></span> |
| <span data-ttu-id="8c5ef-144">**[Search](#azure-search)**</span><span class="sxs-lookup"><span data-stu-id="8c5ef-144">**[Search](#azure-search)**</span></span> |<span data-ttu-id="8c5ef-145">Systemeigen in Client</span><span class="sxs-lookup"><span data-stu-id="8c5ef-145">Native in client</span></span> |<span data-ttu-id="8c5ef-146">Programmgesteuert</span><span class="sxs-lookup"><span data-stu-id="8c5ef-146">Programmatic</span></span> |<span data-ttu-id="8c5ef-147">Client</span><span class="sxs-lookup"><span data-stu-id="8c5ef-147">Client</span></span> |<span data-ttu-id="8c5ef-148">ETW oder benutzerdefiniert</span><span class="sxs-lookup"><span data-stu-id="8c5ef-148">ETW or Custom</span></span> |
| <span data-ttu-id="8c5ef-149">**[Service Bus](#service-bus)**</span><span class="sxs-lookup"><span data-stu-id="8c5ef-149">**[Service Bus](#service-bus)**</span></span> |<span data-ttu-id="8c5ef-150">Systemeigen in Client</span><span class="sxs-lookup"><span data-stu-id="8c5ef-150">Native in client</span></span> |<span data-ttu-id="8c5ef-151">Programmgesteuert</span><span class="sxs-lookup"><span data-stu-id="8c5ef-151">Programmatic</span></span> |<span data-ttu-id="8c5ef-152">Namespace-Manager, Messaging Factory und Client</span><span class="sxs-lookup"><span data-stu-id="8c5ef-152">Namespace Manager, Messaging Factory, and Client</span></span> |<span data-ttu-id="8c5ef-153">ETW</span><span class="sxs-lookup"><span data-stu-id="8c5ef-153">ETW</span></span> |
| <span data-ttu-id="8c5ef-154">**[Service Fabric](#service-fabric)**</span><span class="sxs-lookup"><span data-stu-id="8c5ef-154">**[Service Fabric](#service-fabric)**</span></span> |<span data-ttu-id="8c5ef-155">Systemeigen in Client</span><span class="sxs-lookup"><span data-stu-id="8c5ef-155">Native in client</span></span> |<span data-ttu-id="8c5ef-156">Programmgesteuert</span><span class="sxs-lookup"><span data-stu-id="8c5ef-156">Programmatic</span></span> |<span data-ttu-id="8c5ef-157">Client</span><span class="sxs-lookup"><span data-stu-id="8c5ef-157">Client</span></span> |<span data-ttu-id="8c5ef-158">Keine</span><span class="sxs-lookup"><span data-stu-id="8c5ef-158">None</span></span> |
| <span data-ttu-id="8c5ef-159">**[SQL-Datenbank mit ADO.NET](#sql-database-using-adonet)**</span><span class="sxs-lookup"><span data-stu-id="8c5ef-159">**[SQL Database with ADO.NET](#sql-database-using-adonet)**</span></span> |[<span data-ttu-id="8c5ef-160">Polly</span><span class="sxs-lookup"><span data-stu-id="8c5ef-160">Polly</span></span>](#transient-fault-handling-with-polly) |<span data-ttu-id="8c5ef-161">Deklarativ und programmatisch</span><span class="sxs-lookup"><span data-stu-id="8c5ef-161">Declarative and programmatic</span></span> |<span data-ttu-id="8c5ef-162">Einzelne Anweisungen oder Codeblöcke</span><span class="sxs-lookup"><span data-stu-id="8c5ef-162">Single statements or blocks of code</span></span> |<span data-ttu-id="8c5ef-163">Benutzerdefiniert</span><span class="sxs-lookup"><span data-stu-id="8c5ef-163">Custom</span></span> |
| <span data-ttu-id="8c5ef-164">**[SQL-Datenbank mit Entity Framework](#sql-database-using-entity-framework-6)**</span><span class="sxs-lookup"><span data-stu-id="8c5ef-164">**[SQL Database with Entity Framework](#sql-database-using-entity-framework-6)**</span></span> |<span data-ttu-id="8c5ef-165">Systemeigen in Client</span><span class="sxs-lookup"><span data-stu-id="8c5ef-165">Native in client</span></span> |<span data-ttu-id="8c5ef-166">Programmgesteuert</span><span class="sxs-lookup"><span data-stu-id="8c5ef-166">Programmatic</span></span> |<span data-ttu-id="8c5ef-167">Global pro AppDomain</span><span class="sxs-lookup"><span data-stu-id="8c5ef-167">Global per AppDomain</span></span> |<span data-ttu-id="8c5ef-168">Keine</span><span class="sxs-lookup"><span data-stu-id="8c5ef-168">None</span></span> |
| <span data-ttu-id="8c5ef-169">**[SQL-Datenbank mit Entity Framework Core](#sql-database-using-entity-framework-core)**</span><span class="sxs-lookup"><span data-stu-id="8c5ef-169">**[SQL Database with Entity Framework Core](#sql-database-using-entity-framework-core)**</span></span> |<span data-ttu-id="8c5ef-170">Systemeigen in Client</span><span class="sxs-lookup"><span data-stu-id="8c5ef-170">Native in client</span></span> |<span data-ttu-id="8c5ef-171">Programmgesteuert</span><span class="sxs-lookup"><span data-stu-id="8c5ef-171">Programmatic</span></span> |<span data-ttu-id="8c5ef-172">Global pro AppDomain</span><span class="sxs-lookup"><span data-stu-id="8c5ef-172">Global per AppDomain</span></span> |<span data-ttu-id="8c5ef-173">Keine</span><span class="sxs-lookup"><span data-stu-id="8c5ef-173">None</span></span> |
| <span data-ttu-id="8c5ef-174">**[Storage](#azure-storage)**</span><span class="sxs-lookup"><span data-stu-id="8c5ef-174">**[Storage](#azure-storage)**</span></span> |<span data-ttu-id="8c5ef-175">Systemeigen in Client</span><span class="sxs-lookup"><span data-stu-id="8c5ef-175">Native in client</span></span> |<span data-ttu-id="8c5ef-176">Programmgesteuert</span><span class="sxs-lookup"><span data-stu-id="8c5ef-176">Programmatic</span></span> |<span data-ttu-id="8c5ef-177">Clientvorgänge und einzelne Vorgänge</span><span class="sxs-lookup"><span data-stu-id="8c5ef-177">Client and individual operations</span></span> |<span data-ttu-id="8c5ef-178">TraceSource</span><span class="sxs-lookup"><span data-stu-id="8c5ef-178">TraceSource</span></span> |

> [!NOTE]
> <span data-ttu-id="8c5ef-179">Für die meisten der integrierten Azure-Mechanismen gibt es derzeit keine Möglichkeit, unterschiedliche Wiederholungsrichtlinien für verschiedene Arten von Fehlern oder Ausnahmen anzuwenden.</span><span class="sxs-lookup"><span data-stu-id="8c5ef-179">For most of the Azure built-in retry mechanisms, there is currently no way apply a different retry policy for different types of error or exception.</span></span> <span data-ttu-id="8c5ef-180">Konfigurieren Sie eine Richtlinie mit optimalem Leistungs- und Verfügbarkeitsdurchschnitt.</span><span class="sxs-lookup"><span data-stu-id="8c5ef-180">You should configure a policy that provides the optimum average performance and availability.</span></span> <span data-ttu-id="8c5ef-181">Eine Möglichkeit zur Optimierung der Richtlinie ist die Analyse von Protokolldateien, um den Typ der vorübergehenden Fehler zu bestimmen, die auftreten.</span><span class="sxs-lookup"><span data-stu-id="8c5ef-181">One way to fine-tune the policy is to analyze log files to determine the type of transient faults that are occurring.</span></span>

<!-- markdownlint-disable MD024 MD033 -->

## <a name="azure-active-directory"></a><span data-ttu-id="8c5ef-182">Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="8c5ef-182">Azure Active Directory</span></span>

<span data-ttu-id="8c5ef-183">Azure Active Directory (Azure AD) ist eine umfassende Cloud-Lösung für die Identitäts- und Zugriffsverwaltung, die zentrale Verzeichnisdienste, eine erweiterte Identitätsgovernance, Sicherheit und Zugriffsverwaltung von Anwendungen kombiniert.</span><span class="sxs-lookup"><span data-stu-id="8c5ef-183">Azure Active Directory (Azure AD) is a comprehensive identity and access management cloud solution that combines core directory services, advanced identity governance, security, and application access management.</span></span> <span data-ttu-id="8c5ef-184">Azure AD bietet Entwicklern auf der Grundlage von zentralen Richtlinien und Regeln auch eine Plattform für die Identitätsverwaltung zur Bereitstellung einer Zugriffssteuerung für deren Anwendungen.</span><span class="sxs-lookup"><span data-stu-id="8c5ef-184">Azure AD also offers developers an identity management platform to deliver access control to their applications, based on centralized policy and rules.</span></span>

> [!NOTE]
> <span data-ttu-id="8c5ef-185">Eine Anleitung zu Wiederholungen für Endpunkte der verwalteten Dienstidentität finden Sie unter [Verwenden der verwalteten Dienstidentität (Managed Service Identity, MSI) eines virtuellen Azure-Computers für den Tokenabruf](/azure/active-directory/managed-service-identity/how-to-use-vm-token#error-handling).</span><span class="sxs-lookup"><span data-stu-id="8c5ef-185">For retry guidance on Managed Service Identity endpoints, see [How to use an Azure VM Managed Service Identity (MSI) for token acquisition](/azure/active-directory/managed-service-identity/how-to-use-vm-token#error-handling).</span></span>

### <a name="retry-mechanism"></a><span data-ttu-id="8c5ef-186">Wiederholungsmechanismus</span><span class="sxs-lookup"><span data-stu-id="8c5ef-186">Retry mechanism</span></span>

<span data-ttu-id="8c5ef-187">In der Active Directory-Authentifizierungsbibliothek (ADAL) ist ein integrierter Wiederholungsmechanismus für Azure Active Directory enthalten.</span><span class="sxs-lookup"><span data-stu-id="8c5ef-187">There is a built-in retry mechanism for Azure Active Directory in the Active Directory Authentication Library (ADAL).</span></span> <span data-ttu-id="8c5ef-188">Zur Vermeidung von unerwarteten Sperrungen empfehlen wir, für Drittanbieterbibliotheken und -anwendungscode **keine** Wiederholungsversuche zum Herstellen von fehlgeschlagenen Verbindungen durchzuführen, sondern die Verarbeitung von Wiederholungsversuchen ADAL zu überlassen.</span><span class="sxs-lookup"><span data-stu-id="8c5ef-188">To avoid unexpected lockouts, we recommend that third party libraries and application code do **not** retry failed connections, but allow ADAL to handle retries.</span></span>

### <a name="retry-usage-guidance"></a><span data-ttu-id="8c5ef-189">Gebrauchsanleitung Wiederholungen</span><span class="sxs-lookup"><span data-stu-id="8c5ef-189">Retry usage guidance</span></span>

<span data-ttu-id="8c5ef-190">Berücksichtigen Sie Bei Verwendung von Azure Active Directory die folgenden Richtlinien:</span><span class="sxs-lookup"><span data-stu-id="8c5ef-190">Consider the following guidelines when using Azure Active Directory:</span></span>

- <span data-ttu-id="8c5ef-191">Verwenden Sie nach Möglichkeit die ADAL-Bibliothek und die integrierte Unterstützung für Wiederholungsversuche.</span><span class="sxs-lookup"><span data-stu-id="8c5ef-191">When possible, use the ADAL library and the built-in support for retries.</span></span>
- <span data-ttu-id="8c5ef-192">Wenn Sie die REST-API für Azure Active Directory verwenden, wiederholen Sie den Vorgang, wenn der Ergebniscode 429 (Zu viele Anforderungen) lautet oder das Ergebnis ein Fehler im Bereich 5xx ist.</span><span class="sxs-lookup"><span data-stu-id="8c5ef-192">If you are using the REST API for Azure Active Directory, retry the operation if the result code is 429 (Too Many Requests) or an error in the 5xx range.</span></span> <span data-ttu-id="8c5ef-193">Bei anderen Fehlern den Vorgang nicht wiederholen.</span><span class="sxs-lookup"><span data-stu-id="8c5ef-193">Do not retry for any other errors.</span></span>
- <span data-ttu-id="8c5ef-194">Eine exponentielle Backoff-Richtlinie wird für die Verwendung in Batch-Szenarien mit Azure Active Directory empfohlen.</span><span class="sxs-lookup"><span data-stu-id="8c5ef-194">An exponential back-off policy is recommended for use in batch scenarios with Azure Active Directory.</span></span>

<span data-ttu-id="8c5ef-195">Erwägen Sie, mit den folgenden Einstellungen für Wiederholungsvorgänge zu beginnen.</span><span class="sxs-lookup"><span data-stu-id="8c5ef-195">Consider starting with the following settings for retrying operations.</span></span> <span data-ttu-id="8c5ef-196">Hierbei handelt es sich um allgemeine Einstellungen und Sie sollten die Vorgänge überwachen und die Werte entsprechend Ihrem Szenario optimieren.</span><span class="sxs-lookup"><span data-stu-id="8c5ef-196">These are general purpose settings, and you should monitor the operations and fine tune the values to suit your own scenario.</span></span>

| <span data-ttu-id="8c5ef-197">**Context**</span><span class="sxs-lookup"><span data-stu-id="8c5ef-197">**Context**</span></span> | <span data-ttu-id="8c5ef-198">**Beispiel-Ziel E2E<br />Maximale Wartezeit**</span><span class="sxs-lookup"><span data-stu-id="8c5ef-198">**Sample target E2E<br />max latency**</span></span> | <span data-ttu-id="8c5ef-199">**Wiederholungsstrategie**</span><span class="sxs-lookup"><span data-stu-id="8c5ef-199">**Retry strategy**</span></span> | <span data-ttu-id="8c5ef-200">**Einstellungen**</span><span class="sxs-lookup"><span data-stu-id="8c5ef-200">**Settings**</span></span> | <span data-ttu-id="8c5ef-201">**Werte**</span><span class="sxs-lookup"><span data-stu-id="8c5ef-201">**Values**</span></span> | <span data-ttu-id="8c5ef-202">**So funktioniert's**</span><span class="sxs-lookup"><span data-stu-id="8c5ef-202">**How it works**</span></span> |
| --- | --- | --- | --- | --- | --- |
| <span data-ttu-id="8c5ef-203">Interaktiv, Benutzeroberfläche</span><span class="sxs-lookup"><span data-stu-id="8c5ef-203">Interactive, UI,</span></span><br /><span data-ttu-id="8c5ef-204">oder Vordergrund</span><span class="sxs-lookup"><span data-stu-id="8c5ef-204">or foreground</span></span> |<span data-ttu-id="8c5ef-205">2 Sek</span><span class="sxs-lookup"><span data-stu-id="8c5ef-205">2 sec</span></span> |<span data-ttu-id="8c5ef-206">FixedInterval</span><span class="sxs-lookup"><span data-stu-id="8c5ef-206">FixedInterval</span></span> |<span data-ttu-id="8c5ef-207">Anzahl der Wiederholungen</span><span class="sxs-lookup"><span data-stu-id="8c5ef-207">Retry count</span></span><br /><span data-ttu-id="8c5ef-208">Wiederholungsintervall</span><span class="sxs-lookup"><span data-stu-id="8c5ef-208">Retry interval</span></span><br /><span data-ttu-id="8c5ef-209">Erster schneller Wiederholungsversuch</span><span class="sxs-lookup"><span data-stu-id="8c5ef-209">First fast retry</span></span> |<span data-ttu-id="8c5ef-210">3</span><span class="sxs-lookup"><span data-stu-id="8c5ef-210">3</span></span><br /><span data-ttu-id="8c5ef-211">500 ms</span><span class="sxs-lookup"><span data-stu-id="8c5ef-211">500 ms</span></span><br /><span data-ttu-id="8c5ef-212">true</span><span class="sxs-lookup"><span data-stu-id="8c5ef-212">true</span></span> |<span data-ttu-id="8c5ef-213">Versuch 1 – Verzögerung 0 Sek.</span><span class="sxs-lookup"><span data-stu-id="8c5ef-213">Attempt 1 - delay 0 sec</span></span><br /><span data-ttu-id="8c5ef-214">Versuch 2 – Verzögerung 500 ms</span><span class="sxs-lookup"><span data-stu-id="8c5ef-214">Attempt 2 - delay 500 ms</span></span><br /><span data-ttu-id="8c5ef-215">Versuch 3 – Verzögerung 500 ms</span><span class="sxs-lookup"><span data-stu-id="8c5ef-215">Attempt 3 - delay 500 ms</span></span> |
| <span data-ttu-id="8c5ef-216">Hintergrund oder </span><span class="sxs-lookup"><span data-stu-id="8c5ef-216">Background or</span></span><br /><span data-ttu-id="8c5ef-217">Batch</span><span class="sxs-lookup"><span data-stu-id="8c5ef-217">batch</span></span> |<span data-ttu-id="8c5ef-218">60 Sekunden</span><span class="sxs-lookup"><span data-stu-id="8c5ef-218">60 sec</span></span> |<span data-ttu-id="8c5ef-219">ExponentialBackoff</span><span class="sxs-lookup"><span data-stu-id="8c5ef-219">ExponentialBackoff</span></span> |<span data-ttu-id="8c5ef-220">Anzahl der Wiederholungen</span><span class="sxs-lookup"><span data-stu-id="8c5ef-220">Retry count</span></span><br /><span data-ttu-id="8c5ef-221">Min. Backoff</span><span class="sxs-lookup"><span data-stu-id="8c5ef-221">Min back-off</span></span><br /><span data-ttu-id="8c5ef-222">Max. Backoff</span><span class="sxs-lookup"><span data-stu-id="8c5ef-222">Max back-off</span></span><br /><span data-ttu-id="8c5ef-223">Delta-Backoff</span><span class="sxs-lookup"><span data-stu-id="8c5ef-223">Delta back-off</span></span><br /><span data-ttu-id="8c5ef-224">Erster schneller Wiederholungsversuch</span><span class="sxs-lookup"><span data-stu-id="8c5ef-224">First fast retry</span></span> |<span data-ttu-id="8c5ef-225">5</span><span class="sxs-lookup"><span data-stu-id="8c5ef-225">5</span></span><br /><span data-ttu-id="8c5ef-226">0 Sek.</span><span class="sxs-lookup"><span data-stu-id="8c5ef-226">0 sec</span></span><br /><span data-ttu-id="8c5ef-227">60 Sekunden</span><span class="sxs-lookup"><span data-stu-id="8c5ef-227">60 sec</span></span><br /><span data-ttu-id="8c5ef-228">2 Sek</span><span class="sxs-lookup"><span data-stu-id="8c5ef-228">2 sec</span></span><br /><span data-ttu-id="8c5ef-229">false</span><span class="sxs-lookup"><span data-stu-id="8c5ef-229">false</span></span> |<span data-ttu-id="8c5ef-230">Versuch 1 – Verzögerung 0 Sek.</span><span class="sxs-lookup"><span data-stu-id="8c5ef-230">Attempt 1 - delay 0 sec</span></span><br /><span data-ttu-id="8c5ef-231">Versuch 2 – Verzögerung ca. 2 Sek.</span><span class="sxs-lookup"><span data-stu-id="8c5ef-231">Attempt 2 - delay ~2 sec</span></span><br /><span data-ttu-id="8c5ef-232">Versuch 3 – Verzögerung ca. 6 Sek.</span><span class="sxs-lookup"><span data-stu-id="8c5ef-232">Attempt 3 - delay ~6 sec</span></span><br /><span data-ttu-id="8c5ef-233">Versuch 4 – Verzögerung ca. 14 Sek.</span><span class="sxs-lookup"><span data-stu-id="8c5ef-233">Attempt 4 - delay ~14 sec</span></span><br /><span data-ttu-id="8c5ef-234">Versuch 5 – Verzögerung ca. 30 Sek.</span><span class="sxs-lookup"><span data-stu-id="8c5ef-234">Attempt 5 - delay ~30 sec</span></span> |

### <a name="more-information"></a><span data-ttu-id="8c5ef-235">Weitere Informationen</span><span class="sxs-lookup"><span data-stu-id="8c5ef-235">More information</span></span>

- <span data-ttu-id="8c5ef-236">[Azure Active Directory-Authentifizierungsbibliotheken][adal]</span><span class="sxs-lookup"><span data-stu-id="8c5ef-236">[Azure Active Directory Authentication Libraries][adal]</span></span>

## <a name="cosmos-db"></a><span data-ttu-id="8c5ef-237">Cosmos DB</span><span class="sxs-lookup"><span data-stu-id="8c5ef-237">Cosmos DB</span></span>

<span data-ttu-id="8c5ef-238">Cosmos DB ist eine vollständig verwaltete Datenbank mit mehreren Modellen, die schemalose JSON-Daten unterstützt.</span><span class="sxs-lookup"><span data-stu-id="8c5ef-238">Cosmos DB is a fully-managed multi-model database that supports schema-less JSON data.</span></span> <span data-ttu-id="8c5ef-239">Sie bietet konfigurierbare und zuverlässige Leistung, systemeigene JavaScript-Transaktionsverarbeitung und ist für die Cloud mit elastischer Skalierung konzipiert.</span><span class="sxs-lookup"><span data-stu-id="8c5ef-239">It offers configurable and reliable performance, native JavaScript transactional processing, and is built for the cloud with elastic scale.</span></span>

### <a name="retry-mechanism"></a><span data-ttu-id="8c5ef-240">Wiederholungsmechanismus</span><span class="sxs-lookup"><span data-stu-id="8c5ef-240">Retry mechanism</span></span>

<span data-ttu-id="8c5ef-241">Die `DocumentClient`-Klasse führt bei Fehlversuchen automatisch Wiederholungsversuche durch.</span><span class="sxs-lookup"><span data-stu-id="8c5ef-241">The `DocumentClient` class automatically retries failed attempts.</span></span> <span data-ttu-id="8c5ef-242">Konfigurieren Sie zum Festlegen der Anzahl von Wiederholungsversuchen und der maximalen Wartezeit [ConnectionPolicy.RetryOptions].</span><span class="sxs-lookup"><span data-stu-id="8c5ef-242">To set the number of retries and the maximum wait time, configure [ConnectionPolicy.RetryOptions].</span></span> <span data-ttu-id="8c5ef-243">Ausnahmen, die vom Client ausgelöst werden, unterliegen entweder nicht der Wiederholungsrichtlinie oder sind keine vorübergehenden Fehler.</span><span class="sxs-lookup"><span data-stu-id="8c5ef-243">Exceptions that the client raises are either beyond the retry policy or are not transient errors.</span></span>

<span data-ttu-id="8c5ef-244">Wenn Cosmos DB den Client einschränkt, wird ein HTTP 429-Fehler zurückgegeben.</span><span class="sxs-lookup"><span data-stu-id="8c5ef-244">If Cosmos DB throttles the client, it returns an HTTP 429 error.</span></span> <span data-ttu-id="8c5ef-245">Überprüfen Sie den Statuscode unter `DocumentClientException`.</span><span class="sxs-lookup"><span data-stu-id="8c5ef-245">Check the status code in the `DocumentClientException`.</span></span>

### <a name="policy-configuration"></a><span data-ttu-id="8c5ef-246">Richtlinienkonfiguration</span><span class="sxs-lookup"><span data-stu-id="8c5ef-246">Policy configuration</span></span>

<span data-ttu-id="8c5ef-247">Die folgende Tabelle enthält die Standardeinstellungen für die `RetryOptions`-Klasse.</span><span class="sxs-lookup"><span data-stu-id="8c5ef-247">The following table shows the default settings for the `RetryOptions` class.</span></span>

| <span data-ttu-id="8c5ef-248">Einstellung</span><span class="sxs-lookup"><span data-stu-id="8c5ef-248">Setting</span></span> | <span data-ttu-id="8c5ef-249">Standardwert</span><span class="sxs-lookup"><span data-stu-id="8c5ef-249">Default value</span></span> | <span data-ttu-id="8c5ef-250">BESCHREIBUNG</span><span class="sxs-lookup"><span data-stu-id="8c5ef-250">Description</span></span> |
| --- | --- | --- |
| <span data-ttu-id="8c5ef-251">MaxRetryAttemptsOnThrottledRequests</span><span class="sxs-lookup"><span data-stu-id="8c5ef-251">MaxRetryAttemptsOnThrottledRequests</span></span> |<span data-ttu-id="8c5ef-252">9</span><span class="sxs-lookup"><span data-stu-id="8c5ef-252">9</span></span> |<span data-ttu-id="8c5ef-253">Die maximale Anzahl von Wiederholungsversuchen bei einem Fehlschlagen der Anforderung, weil Cosmos DB die Rateneinschränkung auf den Client angewendet hat.</span><span class="sxs-lookup"><span data-stu-id="8c5ef-253">The maximum number of retries if the request fails because Cosmos DB applied rate limiting on the client.</span></span> |
| <span data-ttu-id="8c5ef-254">MaxRetryWaitTimeInSeconds</span><span class="sxs-lookup"><span data-stu-id="8c5ef-254">MaxRetryWaitTimeInSeconds</span></span> |<span data-ttu-id="8c5ef-255">30</span><span class="sxs-lookup"><span data-stu-id="8c5ef-255">30</span></span> |<span data-ttu-id="8c5ef-256">Die maximale Wiederholungszeit in Sekunden.</span><span class="sxs-lookup"><span data-stu-id="8c5ef-256">The maximum retry time in seconds.</span></span> |

### <a name="example"></a><span data-ttu-id="8c5ef-257">Beispiel</span><span class="sxs-lookup"><span data-stu-id="8c5ef-257">Example</span></span>

```csharp
DocumentClient client = new DocumentClient(new Uri(endpoint), authKey); ;
var options = client.ConnectionPolicy.RetryOptions;
options.MaxRetryAttemptsOnThrottledRequests = 5;
options.MaxRetryWaitTimeInSeconds = 15;
```

### <a name="telemetry"></a><span data-ttu-id="8c5ef-258">Telemetrie</span><span class="sxs-lookup"><span data-stu-id="8c5ef-258">Telemetry</span></span>

<span data-ttu-id="8c5ef-259">Wiederholungsversuche werden als unstrukturierte Ablaufverfolgungsmeldungen über eine .NET **TraceSource**protokolliert.</span><span class="sxs-lookup"><span data-stu-id="8c5ef-259">Retry attempts are logged as unstructured trace messages through a .NET **TraceSource**.</span></span> <span data-ttu-id="8c5ef-260">Sie müssen einen **TraceListener** konfigurieren, um die Ereignisse zu erfassen und diese in ein geeignetes Zielprotokoll zu schreiben.</span><span class="sxs-lookup"><span data-stu-id="8c5ef-260">You must configure a **TraceListener** to capture the events and write them to a suitable destination log.</span></span>

<span data-ttu-id="8c5ef-261">Wenn Sie Ihrer Datei „App.config“ beispielsweise Folgendes hinzugefügt haben, werden die Ablaufverfolgungen in einer Textdatei an demselben Speicherort wie die ausführbare Datei generiert:</span><span class="sxs-lookup"><span data-stu-id="8c5ef-261">For example, if you add the following to your App.config file, traces will be generated in a text file in the same location as the executable:</span></span>

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

## <a name="event-hubs"></a><span data-ttu-id="8c5ef-262">Event Hubs</span><span class="sxs-lookup"><span data-stu-id="8c5ef-262">Event Hubs</span></span>

<span data-ttu-id="8c5ef-263">Azure Event Hubs ist ein hyperskalierter Dienst für die Erfassung von Telemetriedaten, der Millionen von Ereignissen sammelt, transformiert und speichert.</span><span class="sxs-lookup"><span data-stu-id="8c5ef-263">Azure Event Hubs is a hyper-scale telemetry ingestion service that collects, transforms, and stores millions of events.</span></span>

### <a name="retry-mechanism"></a><span data-ttu-id="8c5ef-264">Wiederholungsmechanismus</span><span class="sxs-lookup"><span data-stu-id="8c5ef-264">Retry mechanism</span></span>

<span data-ttu-id="8c5ef-265">Das Wiederholungsverhalten in der Azure Event Hubs-Clientbibliothek wird von der `RetryPolicy`-Eigenschaft in der `EventHubClient`-Klasse gesteuert.</span><span class="sxs-lookup"><span data-stu-id="8c5ef-265">Retry behavior in the Azure Event Hubs Client Library is controlled by the `RetryPolicy` property on the `EventHubClient` class.</span></span> <span data-ttu-id="8c5ef-266">Die Standardrichtlinie führt Wiederholungsversuche mit exponentiell ansteigender Wartezeit durch, wenn Azure Event Hub eine vorübergehende Ausnahme vom Typ `EventHubsException` oder `OperationCanceledException` zurückgibt.</span><span class="sxs-lookup"><span data-stu-id="8c5ef-266">The default policy retries with exponential backoff when Azure Event Hub returns a transient `EventHubsException` or an `OperationCanceledException`.</span></span>

### <a name="example"></a><span data-ttu-id="8c5ef-267">Beispiel</span><span class="sxs-lookup"><span data-stu-id="8c5ef-267">Example</span></span>

```csharp
EventHubClient client = EventHubClient.CreateFromConnectionString("[event_hub_connection_string]");
client.RetryPolicy = RetryPolicy.Default;
```

### <a name="more-information"></a><span data-ttu-id="8c5ef-268">Weitere Informationen</span><span class="sxs-lookup"><span data-stu-id="8c5ef-268">More information</span></span>

<span data-ttu-id="8c5ef-269">[.NET Standard client library for Azure Event Hubs](https://github.com/Azure/azure-event-hubs-dotnet) (.NET Standard-Clientbibliothek für Azure Event Hubs)</span><span class="sxs-lookup"><span data-stu-id="8c5ef-269">[.NET Standard client library for Azure Event Hubs](https://github.com/Azure/azure-event-hubs-dotnet)</span></span>

## <a name="iot-hub"></a><span data-ttu-id="8c5ef-270">IoT Hub</span><span class="sxs-lookup"><span data-stu-id="8c5ef-270">IoT Hub</span></span>

<span data-ttu-id="8c5ef-271">Azure IoT Hub ist ein Dienst zum Verbinden, Überwachen und Verwalten von Geräten für die Entwicklung von IoT-Anwendungen (Internet of Things, Internet der Dinge).</span><span class="sxs-lookup"><span data-stu-id="8c5ef-271">Azure IoT Hub is a service for connecting, monitoring, and managing devices to develop Internet of Things (IoT) applications.</span></span>

### <a name="retry-mechanism"></a><span data-ttu-id="8c5ef-272">Wiederholungsmechanismus</span><span class="sxs-lookup"><span data-stu-id="8c5ef-272">Retry mechanism</span></span>

<span data-ttu-id="8c5ef-273">Mit dem Azure IoT-Geräte-SDK können Fehler im Netzwerk, im Protokoll oder in der Anwendung ermittelt werden.</span><span class="sxs-lookup"><span data-stu-id="8c5ef-273">The Azure IoT device SDK can detect errors in the network, protocol, or application.</span></span> <span data-ttu-id="8c5ef-274">Basierend auf dem Fehlertyp überprüft das SDK, ob eine Wiederholung erforderlich ist.</span><span class="sxs-lookup"><span data-stu-id="8c5ef-274">Based on the error type, the SDK checks whether a retry needs to be performed.</span></span> <span data-ttu-id="8c5ef-275">Ist der Fehler *behebbar*, beginnt das SDK unter Verwendung der Wiederholungsrichtlinie mit der Wiederholung.</span><span class="sxs-lookup"><span data-stu-id="8c5ef-275">If the error is *recoverable*, the SDK begins to retry using the configured retry policy.</span></span>

<span data-ttu-id="8c5ef-276">Die standardmäßige Wiederholungsrichtlinie ist *exponentielles Backoff mit zufälligem Jitter*, sie kann jedoch konfiguriert werden.</span><span class="sxs-lookup"><span data-stu-id="8c5ef-276">The default retry policy is *exponential back-off with random jitter*, but it can be configured.</span></span>

### <a name="policy-configuration"></a><span data-ttu-id="8c5ef-277">Richtlinienkonfiguration</span><span class="sxs-lookup"><span data-stu-id="8c5ef-277">Policy configuration</span></span>

<span data-ttu-id="8c5ef-278">Die Richtlinienkonfiguration unterscheidet sich je nach Sprache.</span><span class="sxs-lookup"><span data-stu-id="8c5ef-278">Policy configuration differs by language.</span></span> <span data-ttu-id="8c5ef-279">Weitere Informationen finden Sie unter [Verwalten von Konnektivität und zuverlässigem Messaging mithilfe von Azure IoT Hub-Geräte-SDKs](/azure/iot-hub/iot-hub-reliability-features-in-sdks#retry-policy-apis).</span><span class="sxs-lookup"><span data-stu-id="8c5ef-279">For more details, see [IoT Hub retry policy configuration](/azure/iot-hub/iot-hub-reliability-features-in-sdks#retry-policy-apis).</span></span>

### <a name="more-information"></a><span data-ttu-id="8c5ef-280">Weitere Informationen</span><span class="sxs-lookup"><span data-stu-id="8c5ef-280">More information</span></span>

- [<span data-ttu-id="8c5ef-281">Verwalten von Konnektivität und zuverlässigem Messaging mithilfe von Azure IoT Hub-Geräte-SDKs</span><span class="sxs-lookup"><span data-stu-id="8c5ef-281">IoT Hub retry policy</span></span>](/azure/iot-hub/iot-hub-reliability-features-in-sdks)
- [<span data-ttu-id="8c5ef-282">Erkennen und Behandeln von Problemen bei der Trennung von Geräteverbindungen mit Azure IoT Hub</span><span class="sxs-lookup"><span data-stu-id="8c5ef-282">Troubleshoot IoT Hub device disconnection</span></span>](/azure/iot-hub/iot-hub-troubleshoot-connectivity)

## <a name="azure-redis-cache"></a><span data-ttu-id="8c5ef-283">Azure Redis Cache</span><span class="sxs-lookup"><span data-stu-id="8c5ef-283">Azure Redis Cache</span></span>

<span data-ttu-id="8c5ef-284">Azure Redis Cache ist ein schneller Datenzugriff und ein Cache Service mit niedriger Latenz, der auf dem beliebten Open Source-Redis Cache basiert.</span><span class="sxs-lookup"><span data-stu-id="8c5ef-284">Azure Redis Cache is a fast data access and low latency cache service based on the popular open source Redis Cache.</span></span> <span data-ttu-id="8c5ef-285">Er ist sicher, von Microsoft verwaltet und ist von jeder Anwendung in Azure zugänglich.</span><span class="sxs-lookup"><span data-stu-id="8c5ef-285">It is secure, managed by Microsoft, and is accessible from any application in Azure.</span></span>

<span data-ttu-id="8c5ef-286">Die Anleitung in diesem Abschnitt beruht auf der Verwendung des StackExchange.Redis-Clients für den Zugriff auf den Cache.</span><span class="sxs-lookup"><span data-stu-id="8c5ef-286">The guidance in this section is based on using the StackExchange.Redis client to access the cache.</span></span> <span data-ttu-id="8c5ef-287">Eine Liste der anderer geeigneter Clients finden Sie auf der [Redis-Website](https://redis.io/clients), diese haben möglicherweise unterschiedliche Wiederholungsmechanismen.</span><span class="sxs-lookup"><span data-stu-id="8c5ef-287">A list of other suitable clients can be found on the [Redis website](https://redis.io/clients), and these may have different retry mechanisms.</span></span>

<span data-ttu-id="8c5ef-288">Beachten Sie, dass der StackExchange.Redis Client Multiplex über eine einzige Verbindung verwendet.</span><span class="sxs-lookup"><span data-stu-id="8c5ef-288">Note that the StackExchange.Redis client uses multiplexing through a single connection.</span></span> <span data-ttu-id="8c5ef-289">Die empfohlene Verwendung ist das Erstellen einer Instanz des Clients beim Starten der Anwendung und die Verwendung dieser Instanz für alle Vorgänge im Cache.</span><span class="sxs-lookup"><span data-stu-id="8c5ef-289">The recommended usage is to create an instance of the client at application startup and use this instance for all operations against the cache.</span></span> <span data-ttu-id="8c5ef-290">Aus diesem Grund wird die Verbindung mit dem Cache nur einmal hergestellt und die Anleitungen in diesem Abschnitt beziehen sich auf die Wiederholungsrichtlinie für die anfängliche Verbindung - und nicht für jeden Vorgang, der auf den Cache zugreift.</span><span class="sxs-lookup"><span data-stu-id="8c5ef-290">For this reason, the connection to the cache is made only once, and so all of the guidance in this section is related to the retry policy for this initial connection—and not for each operation that accesses the cache.</span></span>

### <a name="retry-mechanism"></a><span data-ttu-id="8c5ef-291">Wiederholungsmechanismus</span><span class="sxs-lookup"><span data-stu-id="8c5ef-291">Retry mechanism</span></span>

<span data-ttu-id="8c5ef-292">Der StackExchange.Redis-Client verwendet eine Verbindungs-Manager-Klasse, die über eine Reihe von Optionen konfiguriert ist, z.B.:</span><span class="sxs-lookup"><span data-stu-id="8c5ef-292">The StackExchange.Redis client uses a connection manager class that is configured through a set of options, including:</span></span>

- <span data-ttu-id="8c5ef-293">**ConnectRetry**:</span><span class="sxs-lookup"><span data-stu-id="8c5ef-293">**ConnectRetry**.</span></span> <span data-ttu-id="8c5ef-294">Die Anzahl, wie häufig erneut versucht wird, eine fehlgeschlagene Verbindung mit dem Cache herzustellen.</span><span class="sxs-lookup"><span data-stu-id="8c5ef-294">The number of times a failed connection to the cache will be retried.</span></span>
- <span data-ttu-id="8c5ef-295">**ReconnectRetryPolicy**:</span><span class="sxs-lookup"><span data-stu-id="8c5ef-295">**ReconnectRetryPolicy**.</span></span> <span data-ttu-id="8c5ef-296">Die Wiederholungsstrategie, die verwendet werden soll.</span><span class="sxs-lookup"><span data-stu-id="8c5ef-296">The retry strategy to use.</span></span>
- <span data-ttu-id="8c5ef-297">**ConnectTimeout**:</span><span class="sxs-lookup"><span data-stu-id="8c5ef-297">**ConnectTimeout**.</span></span> <span data-ttu-id="8c5ef-298">Die maximale Wartezeit in Millisekunden.</span><span class="sxs-lookup"><span data-stu-id="8c5ef-298">The maximum waiting time in milliseconds.</span></span>

### <a name="policy-configuration"></a><span data-ttu-id="8c5ef-299">Richtlinienkonfiguration</span><span class="sxs-lookup"><span data-stu-id="8c5ef-299">Policy configuration</span></span>

<span data-ttu-id="8c5ef-300">Wiederholungsrichtlinien werden programmgesteuert durch Festlegen der Optionen für den Client konfiguriert, bevor eine Verbindung mit dem Cache hergestellt wird.</span><span class="sxs-lookup"><span data-stu-id="8c5ef-300">Retry policies are configured programmatically by setting the options for the client before connecting to the cache.</span></span> <span data-ttu-id="8c5ef-301">Dies kann durch das Erstellen einer Instanz der **ConfigurationOptions**-Klasse, das Auffüllen ihrer Eigenschaften und die Übergabe an die **Connect**-Methode erreicht werden.</span><span class="sxs-lookup"><span data-stu-id="8c5ef-301">This can be done by creating an instance of the **ConfigurationOptions** class, populating its properties, and passing it to the **Connect** method.</span></span>

<span data-ttu-id="8c5ef-302">Die integrierten Klassen unterstützen die lineare (konstante) Verzögerung und exponentiell ansteigende Wartezeiten mit zufälligen Wiederholungsintervallen.</span><span class="sxs-lookup"><span data-stu-id="8c5ef-302">The built-in classes support linear (constant) delay and exponential backoff with randomized retry intervals.</span></span> <span data-ttu-id="8c5ef-303">Sie können auch eine benutzerdefinierte Wiederholungsrichtlinie erstellen, indem Sie die **IReconnectRetryPolicy**-Schnittstelle implementieren.</span><span class="sxs-lookup"><span data-stu-id="8c5ef-303">You can also create a custom retry policy by implementing the **IReconnectRetryPolicy** interface.</span></span>

<span data-ttu-id="8c5ef-304">Im folgenden Beispiel wird eine Wiederholungsstrategie mit exponentiell ansteigender Wartezeit konfiguriert.</span><span class="sxs-lookup"><span data-stu-id="8c5ef-304">The following example configures a retry strategy using exponential backoff.</span></span>

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

<span data-ttu-id="8c5ef-305">Alternativ können Sie die Optionen als Zeichenfolge angeben und diese an die **Connect** Methode übergeben.</span><span class="sxs-lookup"><span data-stu-id="8c5ef-305">Alternatively, you can specify the options as a string, and pass this to the **Connect** method.</span></span> <span data-ttu-id="8c5ef-306">Beachten Sie, dass die **ReconnectRetryPolicy**-Eigenschaft auf diese Weise nicht festgelegt werden kann, sondern nur per Code.</span><span class="sxs-lookup"><span data-stu-id="8c5ef-306">Note that the **ReconnectRetryPolicy** property cannot be set this way, only through code.</span></span>

```csharp
var options = "localhost,connectRetry=3,connectTimeout=2000";
ConnectionMultiplexer redis = ConnectionMultiplexer.Connect(options, writer);
```

<span data-ttu-id="8c5ef-307">Es ist auch möglich, Optionen direkt beim Herstellen der Verbindung mit dem Cache anzugeben.</span><span class="sxs-lookup"><span data-stu-id="8c5ef-307">You can also specify options directly when you connect to the cache.</span></span>

```csharp
var conn = ConnectionMultiplexer.Connect("redis0:6380,redis1:6380,connectRetry=3");
```

<span data-ttu-id="8c5ef-308">Weitere Informationen finden Sie unter [Configuration](https://stackexchange.github.io/StackExchange.Redis/Configuration) (Konfiguration) in der StackExchange.Redis-Dokumentation.</span><span class="sxs-lookup"><span data-stu-id="8c5ef-308">For more information, see [Stack Exchange Redis Configuration](https://stackexchange.github.io/StackExchange.Redis/Configuration) in the StackExchange.Redis documentation.</span></span>

<span data-ttu-id="8c5ef-309">Die folgende Tabelle zeigt die Standardeinstellungen für die integrierte Wiederholungsrichtlinie.</span><span class="sxs-lookup"><span data-stu-id="8c5ef-309">The following table shows the default settings for the built-in retry policy.</span></span>

| <span data-ttu-id="8c5ef-310">**Context**</span><span class="sxs-lookup"><span data-stu-id="8c5ef-310">**Context**</span></span> | <span data-ttu-id="8c5ef-311">**Einstellung**</span><span class="sxs-lookup"><span data-stu-id="8c5ef-311">**Setting**</span></span> | <span data-ttu-id="8c5ef-312">**Standardwert**</span><span class="sxs-lookup"><span data-stu-id="8c5ef-312">**Default value**</span></span><br /><span data-ttu-id="8c5ef-313">(v 1.2.2)</span><span class="sxs-lookup"><span data-stu-id="8c5ef-313">(v 1.2.2)</span></span> | <span data-ttu-id="8c5ef-314">**Bedeutung**</span><span class="sxs-lookup"><span data-stu-id="8c5ef-314">**Meaning**</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="8c5ef-315">ConfigurationOptions</span><span class="sxs-lookup"><span data-stu-id="8c5ef-315">ConfigurationOptions</span></span> |<span data-ttu-id="8c5ef-316">ConnectRetry</span><span class="sxs-lookup"><span data-stu-id="8c5ef-316">ConnectRetry</span></span><br /><br /><span data-ttu-id="8c5ef-317">ConnectTimeout</span><span class="sxs-lookup"><span data-stu-id="8c5ef-317">ConnectTimeout</span></span><br /><br /><span data-ttu-id="8c5ef-318">SyncTimeout</span><span class="sxs-lookup"><span data-stu-id="8c5ef-318">SyncTimeout</span></span><br /><br /><span data-ttu-id="8c5ef-319">ReconnectRetryPolicy</span><span class="sxs-lookup"><span data-stu-id="8c5ef-319">ReconnectRetryPolicy</span></span> |<span data-ttu-id="8c5ef-320">3</span><span class="sxs-lookup"><span data-stu-id="8c5ef-320">3</span></span><br /><br /><span data-ttu-id="8c5ef-321">Maximal 5000 ms plus SyncTimeout</span><span class="sxs-lookup"><span data-stu-id="8c5ef-321">Maximum 5000 ms plus SyncTimeout</span></span><br /><span data-ttu-id="8c5ef-322">1000</span><span class="sxs-lookup"><span data-stu-id="8c5ef-322">1000</span></span><br /><br /><span data-ttu-id="8c5ef-323">LinearRetry 5000 ms</span><span class="sxs-lookup"><span data-stu-id="8c5ef-323">LinearRetry 5000 ms</span></span> |<span data-ttu-id="8c5ef-324">Die Anzahl der Wiederholungen von Verbindungsversuchen während des Erstverbindungsvorgangs.</span><span class="sxs-lookup"><span data-stu-id="8c5ef-324">The number of times to repeat connect attempts during the initial connection operation.</span></span><br /><span data-ttu-id="8c5ef-325">Zeitlimit (ms) für Verbindungen.</span><span class="sxs-lookup"><span data-stu-id="8c5ef-325">Timeout (ms) for connect operations.</span></span> <span data-ttu-id="8c5ef-326">Keine Verzögerung zwischen den Wiederholungsversuchen.</span><span class="sxs-lookup"><span data-stu-id="8c5ef-326">Not a delay between retry attempts.</span></span><br /><span data-ttu-id="8c5ef-327">Zeit (ms), um synchrone Vorgänge zu ermöglichen.</span><span class="sxs-lookup"><span data-stu-id="8c5ef-327">Time (ms) to allow for synchronous operations.</span></span><br /><br /><span data-ttu-id="8c5ef-328">Wiederholung jeweils nach 5000 ms.</span><span class="sxs-lookup"><span data-stu-id="8c5ef-328">Retry every 5000 ms.</span></span>|

> [!NOTE]
> <span data-ttu-id="8c5ef-329">Für synchrone Vorgänge kann `SyncTimeout` einen Beitrag zur End-to-End-Wartezeit leisten, aber wenn der Wert zu niedrig festgelegt wird, kann es zu übermäßigen Zeitüberschreitungen kommen.</span><span class="sxs-lookup"><span data-stu-id="8c5ef-329">For synchronous operations, `SyncTimeout` can add to the end-to-end latency, but setting the value too low can cause excessive timeouts.</span></span> <span data-ttu-id="8c5ef-330">Weitere Informationen finden Sie unter [Problembehandlung für Azure Redis Cache][redis-cache-troubleshoot].</span><span class="sxs-lookup"><span data-stu-id="8c5ef-330">See [How to troubleshoot Azure Redis Cache][redis-cache-troubleshoot].</span></span> <span data-ttu-id="8c5ef-331">Im Allgemeinen ist es ratsam, die Verwendung von synchronen Vorgängen zu vermeiden und stattdessen asynchrone Vorgänge zu verwenden.</span><span class="sxs-lookup"><span data-stu-id="8c5ef-331">In general, avoid using synchronous operations, and use asynchronous operations instead.</span></span> <span data-ttu-id="8c5ef-332">Weitere Informationen finden Sie unter [Pipelines und Multiplexers](https://github.com/StackExchange/StackExchange.Redis/blob/master/docs/PipelinesMultiplexers.md).</span><span class="sxs-lookup"><span data-stu-id="8c5ef-332">For more information see [Pipelines and Multiplexers](https://github.com/StackExchange/StackExchange.Redis/blob/master/docs/PipelinesMultiplexers.md).</span></span>

### <a name="retry-usage-guidance"></a><span data-ttu-id="8c5ef-333">Gebrauchsanleitung Wiederholungen</span><span class="sxs-lookup"><span data-stu-id="8c5ef-333">Retry usage guidance</span></span>

<span data-ttu-id="8c5ef-334">Berücksichtigen Sie bei Verwendung von Azure Redis Cache die folgenden Richtlinien :</span><span class="sxs-lookup"><span data-stu-id="8c5ef-334">Consider the following guidelines when using Azure Redis Cache:</span></span>

- <span data-ttu-id="8c5ef-335">Der StackExchange Redis-Client verwaltet seine eigene Wiederholungen, aber nur beim Herstellen einer Verbindung mit dem Cache beim ersten Starten der Anwendung.</span><span class="sxs-lookup"><span data-stu-id="8c5ef-335">The StackExchange Redis client manages its own retries, but only when establishing a connection to the cache when the application first starts.</span></span> <span data-ttu-id="8c5ef-336">Sie können das Verbindungstimeout, die Anzahl von Wiederholungsversuchen und die Dauer zwischen den Wiederholungen zur Herstellung dieser Verbindung konfigurieren, aber die Wiederholungsrichtlinie gilt nicht für Vorgänge für den Cache.</span><span class="sxs-lookup"><span data-stu-id="8c5ef-336">You can configure the connection timeout, the number of retry attempts, and the time between retries to establish this connection, but the retry policy does not apply to operations against the cache.</span></span>
- <span data-ttu-id="8c5ef-337">Anstatt eine große Anzahl von Wiederholungsversuchen zu verwenden, sollten Sie auf die ursprüngliche Datenquelle zurückgreifen.</span><span class="sxs-lookup"><span data-stu-id="8c5ef-337">Instead of using a large number of retry attempts, consider falling back by accessing the original data source instead.</span></span>

### <a name="telemetry"></a><span data-ttu-id="8c5ef-338">Telemetrie</span><span class="sxs-lookup"><span data-stu-id="8c5ef-338">Telemetry</span></span>

<span data-ttu-id="8c5ef-339">Sie können Informationen zu Verbindungen (aber nicht für andere Vorgänge) mit einem **TextWriter**sammeln.</span><span class="sxs-lookup"><span data-stu-id="8c5ef-339">You can collect information about connections (but not other operations) using a **TextWriter**.</span></span>

```csharp
var writer = new StringWriter();
ConnectionMultiplexer redis = ConnectionMultiplexer.Connect(options, writer);
```

<span data-ttu-id="8c5ef-340">Ein Beispiel für die dadurch generierte Ausgabe ist unten dargestellt.</span><span class="sxs-lookup"><span data-stu-id="8c5ef-340">An example of the output this generates is shown below.</span></span>

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

### <a name="examples"></a><span data-ttu-id="8c5ef-341">Beispiele</span><span class="sxs-lookup"><span data-stu-id="8c5ef-341">Examples</span></span>

<span data-ttu-id="8c5ef-342">Im folgenden Codebeispiel wird eine konstante (lineare) Verzögerung zwischen Wiederholungsversuchen konfiguriert, wenn der StackExchange.Redis-Client initialisiert wird.</span><span class="sxs-lookup"><span data-stu-id="8c5ef-342">The following code example configures a constant (linear) delay between retries when initializing the StackExchange.Redis client.</span></span> <span data-ttu-id="8c5ef-343">In diesem Beispiel wird veranschaulicht, wie die Konfiguration mit einer **ConfigurationOptions**-Instanz festgelegt wird.</span><span class="sxs-lookup"><span data-stu-id="8c5ef-343">This example shows how to set the configuration using a **ConfigurationOptions** instance.</span></span>

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

<span data-ttu-id="8c5ef-344">Im nächsten Beispiel wird die Konfiguration durch Angeben der Optionen als Zeichenfolge festgelegt.</span><span class="sxs-lookup"><span data-stu-id="8c5ef-344">The next example sets the configuration by specifying the options as a string.</span></span> <span data-ttu-id="8c5ef-345">Das Verbindungstimeout ist der maximale Zeitraum für das Warten auf die Verbindungsherstellung mit dem Cache und nicht die Verzögerung zwischen Wiederholungsversuchen.</span><span class="sxs-lookup"><span data-stu-id="8c5ef-345">The connection timeout is the maximum period of time to wait for a connection to the cache, not the delay between retry attempts.</span></span> <span data-ttu-id="8c5ef-346">Beachten Sie, dass die **ReconnectRetryPolicy**-Eigenschaft nur per Code festgelegt werden kann.</span><span class="sxs-lookup"><span data-stu-id="8c5ef-346">Note that the **ReconnectRetryPolicy** property can only be set by code.</span></span>

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

<span data-ttu-id="8c5ef-347">Weitere Beispiele finden Sie unter [Konfiguration](https://github.com/StackExchange/StackExchange.Redis/blob/master/docs/Configuration.md) auf der Projektwebsite.</span><span class="sxs-lookup"><span data-stu-id="8c5ef-347">For more examples, see [Configuration](https://github.com/StackExchange/StackExchange.Redis/blob/master/docs/Configuration.md) on the project website.</span></span>

### <a name="more-information"></a><span data-ttu-id="8c5ef-348">Weitere Informationen</span><span class="sxs-lookup"><span data-stu-id="8c5ef-348">More information</span></span>

- [<span data-ttu-id="8c5ef-349">Redis-Website</span><span class="sxs-lookup"><span data-stu-id="8c5ef-349">Redis website</span></span>](https://redis.io/)

## <a name="azure-search"></a><span data-ttu-id="8c5ef-350">Azure Search</span><span class="sxs-lookup"><span data-stu-id="8c5ef-350">Azure Search</span></span>

<span data-ttu-id="8c5ef-351">Azure Search kann dafür verwendet werden, nützliche und anspruchsvolle Suchfunktionen zu einer Website oder Anwendung hinzuzufügen, Suchergebnisse schnell und einfach zu optimieren und umfassende und fein abgestimmte Rangmodelle zu erstellen.</span><span class="sxs-lookup"><span data-stu-id="8c5ef-351">Azure Search can be used to add powerful and sophisticated search capabilities to a website or application, quickly and easily tune search results, and construct rich and fine-tuned ranking models.</span></span>

### <a name="retry-mechanism"></a><span data-ttu-id="8c5ef-352">Wiederholungsmechanismus</span><span class="sxs-lookup"><span data-stu-id="8c5ef-352">Retry mechanism</span></span>

<span data-ttu-id="8c5ef-353">Das Wiederholungsverhalten im Azure Search SDK wird mit der `SetRetryPolicy`-Methode in den Klassen [SearchServiceClient] und [SearchIndexClient] gesteuert.</span><span class="sxs-lookup"><span data-stu-id="8c5ef-353">Retry behavior in the Azure Search SDK is controlled by the `SetRetryPolicy` method on the [SearchServiceClient] and [SearchIndexClient] classes.</span></span> <span data-ttu-id="8c5ef-354">Bei der Standardrichtlinie werden Wiederholungsversuche mit exponentiellem Backoff (exponentiell ansteigende Wartezeiten) durchgeführt, wenn Azure Search eine 5xx- oder 408-Antwort (Anforderungstimeout) zurückgibt.</span><span class="sxs-lookup"><span data-stu-id="8c5ef-354">The default policy retries with exponential backoff when Azure Search returns a 5xx or 408 (Request Timeout) response.</span></span>

### <a name="telemetry"></a><span data-ttu-id="8c5ef-355">Telemetrie</span><span class="sxs-lookup"><span data-stu-id="8c5ef-355">Telemetry</span></span>

<span data-ttu-id="8c5ef-356">Nachverfolgung mit ETW oder per Registrierung eines benutzerdefinierten Ablaufverfolgungsanbieters.</span><span class="sxs-lookup"><span data-stu-id="8c5ef-356">Trace with ETW or by registering a custom trace provider.</span></span> <span data-ttu-id="8c5ef-357">Weitere Informationen finden Sie in der [AutoRest-Dokumentation][autorest].</span><span class="sxs-lookup"><span data-stu-id="8c5ef-357">For more information, see the [AutoRest documentation][autorest].</span></span>

## <a name="service-bus"></a><span data-ttu-id="8c5ef-358">Service Bus</span><span class="sxs-lookup"><span data-stu-id="8c5ef-358">Service Bus</span></span>

<span data-ttu-id="8c5ef-359">Service Bus ist ein Cloud  Messaging-Plattform, die lose gekoppelten Nachrichtenaustausch mit verbesserter Skalierbarkeit und Stabilität für die Komponenten einer Anwendung bietet, egal ob in der Cloud oder lokal gehostet.</span><span class="sxs-lookup"><span data-stu-id="8c5ef-359">Service Bus is a cloud messaging platform that provides loosely coupled message exchange with improved scale and resiliency for components of an application, whether hosted in the cloud or on-premises.</span></span>

### <a name="retry-mechanism"></a><span data-ttu-id="8c5ef-360">Wiederholungsmechanismus</span><span class="sxs-lookup"><span data-stu-id="8c5ef-360">Retry mechanism</span></span>

<span data-ttu-id="8c5ef-361">Service Bus implementiert Wiederholungen mithilfe von Implementierungen der [RetryPolicy](/dotnet/api/microsoft.servicebus.retrypolicy) -Basisklasse.</span><span class="sxs-lookup"><span data-stu-id="8c5ef-361">Service Bus implements retries using implementations of the [RetryPolicy](/dotnet/api/microsoft.servicebus.retrypolicy) base class.</span></span> <span data-ttu-id="8c5ef-362">Alle Service Bus-Clients machen eine **RetryPolicy**-Eigenschaft verfügbar, die auf eine der Implementierungen der **RetryPolicy**-Basisklasse festgelegt werden kann.</span><span class="sxs-lookup"><span data-stu-id="8c5ef-362">All of the Service Bus clients expose a **RetryPolicy** property that can be set to one of the implementations of the **RetryPolicy** base class.</span></span> <span data-ttu-id="8c5ef-363">Die integrierten Implementierungen sind:</span><span class="sxs-lookup"><span data-stu-id="8c5ef-363">The built-in implementations are:</span></span>

- <span data-ttu-id="8c5ef-364">Die [RetryExponential-Klasse](/dotnet/api/microsoft.servicebus.retryexponential).</span><span class="sxs-lookup"><span data-stu-id="8c5ef-364">The [RetryExponential class](/dotnet/api/microsoft.servicebus.retryexponential).</span></span> <span data-ttu-id="8c5ef-365">Dies macht Eigenschaften verfügbar, die das Backoff-Intervall, die Anzahl der Wiederholungsversuche und die **TerminationTimeBuffer** -Eigenschaft steuern, die verwendet wird, um die Gesamtzeit für die Ausführung des Vorgangs zu beschränken.</span><span class="sxs-lookup"><span data-stu-id="8c5ef-365">This exposes properties that control the back-off interval, the retry count, and the **TerminationTimeBuffer** property that is used to limit the total time for the operation to complete.</span></span>

- <span data-ttu-id="8c5ef-366">Die [NoRetry-Klasse](/dotnet/api/microsoft.servicebus.noretry).</span><span class="sxs-lookup"><span data-stu-id="8c5ef-366">The [NoRetry class](/dotnet/api/microsoft.servicebus.noretry).</span></span> <span data-ttu-id="8c5ef-367">Dies wird verwendet, wenn Wiederholungen auf der Ebene des Service Bus-API Levels  nicht erforderlich sind, z. B. wenn Wiederholungen von einem anderen Prozess als Teil eines Batches oder als Vorgang mit mehreren Schritten verwaltet werden.</span><span class="sxs-lookup"><span data-stu-id="8c5ef-367">This is used when retries at the Service Bus API level are not required, such as when retries are managed by another process as part of a batch or multiple step operation.</span></span>

<span data-ttu-id="8c5ef-368">Service Bus-Aktionen können einen Reihe von Ausnahmen zurückgeben, wie unter [Service Bus-Messagingausnahmen](/azure/service-bus-messaging/service-bus-messaging-exceptions) aufgeführt.</span><span class="sxs-lookup"><span data-stu-id="8c5ef-368">Service Bus actions can return a range of exceptions, as listed in [Service Bus messaging exceptions](/azure/service-bus-messaging/service-bus-messaging-exceptions).</span></span> <span data-ttu-id="8c5ef-369">Die Liste enthält Informationen darüber, ob diese für die Wiederholung des Vorgangs geeignet ist.</span><span class="sxs-lookup"><span data-stu-id="8c5ef-369">The list provides information about which if these indicate that retrying the operation is appropriate.</span></span> <span data-ttu-id="8c5ef-370">Angenommen, eine **ServerBusyException** gibt an, dass der Client für eine bestimmte Zeitspanne warten sollte, bevor der Vorgang wiederholt wird.</span><span class="sxs-lookup"><span data-stu-id="8c5ef-370">For example, a **ServerBusyException** indicates that the client should wait for a period of time, then retry the operation.</span></span> <span data-ttu-id="8c5ef-371">Das Auftreten einer **ServerBusyException** bewirkt auch, dass Service Bus in einen anderen Modus wechselt, in dem den berechneten Wiederholungsverzögerungen zusätzliche 10 Sekunden hinzugefügt werden.</span><span class="sxs-lookup"><span data-stu-id="8c5ef-371">The occurrence of a **ServerBusyException** also causes Service Bus to switch to a different mode, in which an extra 10-second delay is added to the computed retry delays.</span></span> <span data-ttu-id="8c5ef-372">Dieser Modus wird nach kurzer Zeit zurückgesetzt.</span><span class="sxs-lookup"><span data-stu-id="8c5ef-372">This mode is reset after a short period.</span></span>

<span data-ttu-id="8c5ef-373">Die vom Service Bus zurückgegebenen Ausnahmen machen die **IsTransient**-Eigenschaft verfügbar, die angibt, ob der Client den Vorgang wiederholen soll.</span><span class="sxs-lookup"><span data-stu-id="8c5ef-373">The exceptions returned from Service Bus expose the **IsTransient** property that indicates if the client should retry the operation.</span></span> <span data-ttu-id="8c5ef-374">Die integrierte **RetryExponential**-Richtlinie stützt sich auf die **IsTransient**-Eigenschaft in der **MessagingException**-Klasse, die die Basisklasse für alle Service Bus-Ausnahmen ist.</span><span class="sxs-lookup"><span data-stu-id="8c5ef-374">The built-in **RetryExponential** policy relies on the **IsTransient** property in the **MessagingException** class, which is the base class for all Service Bus exceptions.</span></span> <span data-ttu-id="8c5ef-375">Bei der Erstellung benutzerdefinierter Implementierungen der **RetryPolicy**-Basisklasse können Sie eine Kombination aus Ausnahmetyp und **IsTransient**-Eigenschaft verwenden, um eine genauere Kontrolle über Wiederholungsaktionen zu ermöglichen.</span><span class="sxs-lookup"><span data-stu-id="8c5ef-375">If you create custom implementations of the **RetryPolicy** base class you could use a combination of the exception type and the **IsTransient** property to provide more fine-grained control over retry actions.</span></span> <span data-ttu-id="8c5ef-376">Sie können z.B. eine **QuotaExceededException** erkennen und Maßnahmen ergreifen, um die Warteschlange zu leeren, bevor erneut versucht wird, darüber eine Nachricht zu senden.</span><span class="sxs-lookup"><span data-stu-id="8c5ef-376">For example, you could detect a **QuotaExceededException** and take action to drain the queue before retrying sending a message to it.</span></span>

### <a name="policy-configuration"></a><span data-ttu-id="8c5ef-377">Richtlinienkonfiguration</span><span class="sxs-lookup"><span data-stu-id="8c5ef-377">Policy configuration</span></span>

<span data-ttu-id="8c5ef-378">Wiederholungsrichtlinien werden programmgesteuert festgelegt und können als Standardrichtlinie für einen **NamespaceManager** und für eine **MessagingFactory** oder einzeln für jeden Messaging Client festgelegt werden.</span><span class="sxs-lookup"><span data-stu-id="8c5ef-378">Retry policies are set programmatically, and can be set as a default policy for a **NamespaceManager** and for a **MessagingFactory**, or individually for each messaging client.</span></span> <span data-ttu-id="8c5ef-379">Um die Standard-Wiederholungsrichtlinie für die Messaging-Sitzung festzulegen, legen Sie die **RetryPolicy** von **NamespaceManager** fest.</span><span class="sxs-lookup"><span data-stu-id="8c5ef-379">To set the default retry policy for a messaging session you set the **RetryPolicy** of the **NamespaceManager**.</span></span>

```csharp
namespaceManager.Settings.RetryPolicy = new RetryExponential(minBackoff: TimeSpan.FromSeconds(0.1),
                                                                maxBackoff: TimeSpan.FromSeconds(30),
                                                                maxRetryCount: 3);
```

<span data-ttu-id="8c5ef-380">Legen Sie zum Festlegen der Standard-Wiederholungsrichtlinie für alle Clients einer Messaging-Factory die **RetryPolicy** von **MessagingFactory** fest.</span><span class="sxs-lookup"><span data-stu-id="8c5ef-380">To set the default retry policy for all clients created from a messaging factory, you set the **RetryPolicy** of the **MessagingFactory**.</span></span>

```csharp
messagingFactory.RetryPolicy = new RetryExponential(minBackoff: TimeSpan.FromSeconds(0.1),
                                                    maxBackoff: TimeSpan.FromSeconds(30),
                                                    maxRetryCount: 3);
```

<span data-ttu-id="8c5ef-381">Um die Standard-Wiederholungsrichtlinie für einen Messaging Client festzulegen oder die Standardrichtlinie zu überschreiben, legen Sie seine **RetryPolicy** -Eigenschaft mit einer Instanz der erforderlichen Richtlinienklasse fest:</span><span class="sxs-lookup"><span data-stu-id="8c5ef-381">To set the retry policy for a messaging client, or to override its default policy, you set its **RetryPolicy** property using an instance of the required policy class:</span></span>

```csharp
client.RetryPolicy = new RetryExponential(minBackoff: TimeSpan.FromSeconds(0.1),
                                            maxBackoff: TimeSpan.FromSeconds(30),
                                            maxRetryCount: 3);
```

<span data-ttu-id="8c5ef-382">Die Wiederholungsrichtlinie kann auf der individuellen Vorgangsebene festgelegt werden.</span><span class="sxs-lookup"><span data-stu-id="8c5ef-382">The retry policy cannot be set at the individual operation level.</span></span> <span data-ttu-id="8c5ef-383">Sie gilt für alle Vorgänge des Messagin-Client.</span><span class="sxs-lookup"><span data-stu-id="8c5ef-383">It applies to all operations for the messaging client.</span></span>
<span data-ttu-id="8c5ef-384">Die folgende Tabelle zeigt die Standardeinstellungen für die integrierte Wiederholungsrichtlinie.</span><span class="sxs-lookup"><span data-stu-id="8c5ef-384">The following table shows the default settings for the built-in retry policy.</span></span>

| <span data-ttu-id="8c5ef-385">Einstellung</span><span class="sxs-lookup"><span data-stu-id="8c5ef-385">Setting</span></span> | <span data-ttu-id="8c5ef-386">Standardwert</span><span class="sxs-lookup"><span data-stu-id="8c5ef-386">Default value</span></span> | <span data-ttu-id="8c5ef-387">Bedeutung</span><span class="sxs-lookup"><span data-stu-id="8c5ef-387">Meaning</span></span> |
|---------|---------------|---------|
| <span data-ttu-id="8c5ef-388">Richtlinie</span><span class="sxs-lookup"><span data-stu-id="8c5ef-388">Policy</span></span> | <span data-ttu-id="8c5ef-389">Exponentiell</span><span class="sxs-lookup"><span data-stu-id="8c5ef-389">Exponential</span></span> | <span data-ttu-id="8c5ef-390">Exponentielles Backoff.</span><span class="sxs-lookup"><span data-stu-id="8c5ef-390">Exponential back-off.</span></span> |
| <span data-ttu-id="8c5ef-391">MinimalBackoff</span><span class="sxs-lookup"><span data-stu-id="8c5ef-391">MinimalBackoff</span></span> | <span data-ttu-id="8c5ef-392">0</span><span class="sxs-lookup"><span data-stu-id="8c5ef-392">0</span></span> | <span data-ttu-id="8c5ef-393">Das minimale Backoff-Intervall.</span><span class="sxs-lookup"><span data-stu-id="8c5ef-393">Minimum back-off interval.</span></span> <span data-ttu-id="8c5ef-394">Dieser Wert wird zum Wiederholungsintervall addiert, das auf der Grundlage von „deltaBackoff“ berechnet wurde.</span><span class="sxs-lookup"><span data-stu-id="8c5ef-394">This is added to the retry interval computed from deltaBackoff.</span></span> |
| <span data-ttu-id="8c5ef-395">MaximumBackoff</span><span class="sxs-lookup"><span data-stu-id="8c5ef-395">MaximumBackoff</span></span> | <span data-ttu-id="8c5ef-396">30 Sekunden</span><span class="sxs-lookup"><span data-stu-id="8c5ef-396">30 seconds</span></span> | <span data-ttu-id="8c5ef-397">Das maximale Backoff-Intervall.</span><span class="sxs-lookup"><span data-stu-id="8c5ef-397">Maximum back-off interval.</span></span> <span data-ttu-id="8c5ef-398">„MaximumBackoff“ wird verwendet, wenn das berechnete Wiederholungsintervall größer als „MaxBackoff“ ist.</span><span class="sxs-lookup"><span data-stu-id="8c5ef-398">MaximumBackoff is used if the computed retry interval is greater than MaxBackoff.</span></span> |
| <span data-ttu-id="8c5ef-399">DeltaBackoff</span><span class="sxs-lookup"><span data-stu-id="8c5ef-399">DeltaBackoff</span></span> | <span data-ttu-id="8c5ef-400">3 Sekunden</span><span class="sxs-lookup"><span data-stu-id="8c5ef-400">3 seconds</span></span> | <span data-ttu-id="8c5ef-401">Backoff-Intervall zwischen den Wiederholungen.</span><span class="sxs-lookup"><span data-stu-id="8c5ef-401">Back-off interval between retries.</span></span> <span data-ttu-id="8c5ef-402">Ein Vielfaches dieser Zeitspanne wird für nachfolgende Wiederholungen verwendet.</span><span class="sxs-lookup"><span data-stu-id="8c5ef-402">Multiples of this timespan will be used for subsequent retry attempts.</span></span> |
| <span data-ttu-id="8c5ef-403">TimeBuffer</span><span class="sxs-lookup"><span data-stu-id="8c5ef-403">TimeBuffer</span></span> | <span data-ttu-id="8c5ef-404">5 Sekunden</span><span class="sxs-lookup"><span data-stu-id="8c5ef-404">5 seconds</span></span> | <span data-ttu-id="8c5ef-405">Der Beendigungszeitpuffer für die Wiederholung.</span><span class="sxs-lookup"><span data-stu-id="8c5ef-405">The termination time buffer associated with the retry.</span></span> <span data-ttu-id="8c5ef-406">Wiederholungsversuche werden abgebrochen, wenn die verbleibende Zeit kleiner als „TimeBuffer“ ist.</span><span class="sxs-lookup"><span data-stu-id="8c5ef-406">Retry attempts will be abandoned if the remaining time is less than TimeBuffer.</span></span> |
| <span data-ttu-id="8c5ef-407">MaxRetryCount</span><span class="sxs-lookup"><span data-stu-id="8c5ef-407">MaxRetryCount</span></span> | <span data-ttu-id="8c5ef-408">10</span><span class="sxs-lookup"><span data-stu-id="8c5ef-408">10</span></span> | <span data-ttu-id="8c5ef-409">Die maximale Anzahl von Warnungen.</span><span class="sxs-lookup"><span data-stu-id="8c5ef-409">The maximum number of retries.</span></span> |
| <span data-ttu-id="8c5ef-410">ServerBusyBaseSleepTime</span><span class="sxs-lookup"><span data-stu-id="8c5ef-410">ServerBusyBaseSleepTime</span></span> | <span data-ttu-id="8c5ef-411">10 Sekunden</span><span class="sxs-lookup"><span data-stu-id="8c5ef-411">10 seconds</span></span> | <span data-ttu-id="8c5ef-412">Wenn es sich bei der zuletzt aufgetretenen Ausnahme um **ServerBusyException** handelt, wird dieser Wert zum berechneten Wiederholungsintervall addiert.</span><span class="sxs-lookup"><span data-stu-id="8c5ef-412">If the last exception encountered was **ServerBusyException**, this value will be added to the computed retry interval.</span></span> <span data-ttu-id="8c5ef-413">Dieser Wert kann nicht geändert werden.</span><span class="sxs-lookup"><span data-stu-id="8c5ef-413">This value cannot be changed.</span></span> |

### <a name="retry-usage-guidance"></a><span data-ttu-id="8c5ef-414">Gebrauchsanleitung Wiederholungen</span><span class="sxs-lookup"><span data-stu-id="8c5ef-414">Retry usage guidance</span></span>

<span data-ttu-id="8c5ef-415">Berücksichtigen Sie bei Verwendung von Service Bus die folgenden Richtlinien:</span><span class="sxs-lookup"><span data-stu-id="8c5ef-415">Consider the following guidelines when using Service Bus:</span></span>

- <span data-ttu-id="8c5ef-416">Implementieren Sie bei Verwendung der integrierten **RetryExponential** -Implementierung keinen Fallbackvorgang, da die Richtlinie auf Server Busy-Ausnahmen reagiert und automatisch in einen entsprechenden Wiederholungsmodus wechselt.</span><span class="sxs-lookup"><span data-stu-id="8c5ef-416">When using the built-in **RetryExponential** implementation, do not implement a fallback operation as the policy reacts to Server Busy exceptions and automatically switches to an appropriate retry mode.</span></span>
- <span data-ttu-id="8c5ef-417">Service Bus unterstützt eine Funktion namens Paired Namespace, mit der automatisches Failover zu einer Backup-Warteschlange in einem separaten Namespace implementiert wird, falls die Warteschlange im primären Namespace fehlschlägt.</span><span class="sxs-lookup"><span data-stu-id="8c5ef-417">Service Bus supports a feature called Paired Namespaces, which implements automatic failover to a backup queue in a separate namespace if the queue in the primary namespace fails.</span></span> <span data-ttu-id="8c5ef-418">Nachrichten aus der sekundären Warteschlange können an die primäre Warteschlange gesendet werden, wenn diese wiederhergestellt wird.</span><span class="sxs-lookup"><span data-stu-id="8c5ef-418">Messages from the secondary queue can be sent back to the primary queue when it recovers.</span></span> <span data-ttu-id="8c5ef-419">Mit dieser Funktion können vorübergehender Fehler behandelt werden.</span><span class="sxs-lookup"><span data-stu-id="8c5ef-419">This feature helps to address transient failures.</span></span> <span data-ttu-id="8c5ef-420">Weitere Informationen finden Sie unter [Asynchrone Nachrichtenmuster und Hochverfügbarkeit](/azure/service-bus-messaging/service-bus-async-messaging).</span><span class="sxs-lookup"><span data-stu-id="8c5ef-420">For more information, see [Asynchronous Messaging Patterns and High Availability](/azure/service-bus-messaging/service-bus-async-messaging).</span></span>

<span data-ttu-id="8c5ef-421">Erwägen Sie, mit den folgenden Einstellungen für Wiederholungsvorgänge zu beginnen.</span><span class="sxs-lookup"><span data-stu-id="8c5ef-421">Consider starting with following settings for retrying operations.</span></span> <span data-ttu-id="8c5ef-422">Hierbei handelt es sich um allgemeine Einstellungen und Sie sollten die Vorgänge überwachen und die Werte entsprechend Ihrem Szenario optimieren.</span><span class="sxs-lookup"><span data-stu-id="8c5ef-422">These are general purpose settings, and you should monitor the operations and fine tune the values to suit your own scenario.</span></span>

| <span data-ttu-id="8c5ef-423">Kontext</span><span class="sxs-lookup"><span data-stu-id="8c5ef-423">Context</span></span> | <span data-ttu-id="8c5ef-424">Beispiel für maximale Wartezeit</span><span class="sxs-lookup"><span data-stu-id="8c5ef-424">Example maximum latency</span></span> | <span data-ttu-id="8c5ef-425">Wiederholungsrichtlinie</span><span class="sxs-lookup"><span data-stu-id="8c5ef-425">Retry policy</span></span> | <span data-ttu-id="8c5ef-426">Einstellungen</span><span class="sxs-lookup"><span data-stu-id="8c5ef-426">Settings</span></span> | <span data-ttu-id="8c5ef-427">So funktioniert's</span><span class="sxs-lookup"><span data-stu-id="8c5ef-427">How it works</span></span> |
|---------|---------|---------|---------|---------|
| <span data-ttu-id="8c5ef-428">Interaktiv, Benutzeroberfläche oder Vordergrund</span><span class="sxs-lookup"><span data-stu-id="8c5ef-428">Interactive, UI, or foreground</span></span> | <span data-ttu-id="8c5ef-429">2 Sekunden\*</span><span class="sxs-lookup"><span data-stu-id="8c5ef-429">2 seconds\*</span></span>  | <span data-ttu-id="8c5ef-430">Exponentiell</span><span class="sxs-lookup"><span data-stu-id="8c5ef-430">Exponential</span></span> | <span data-ttu-id="8c5ef-431">MinimumBackoff = 0</span><span class="sxs-lookup"><span data-stu-id="8c5ef-431">MinimumBackoff = 0</span></span> <br/> <span data-ttu-id="8c5ef-432">MaximumBackoff = 30 s</span><span class="sxs-lookup"><span data-stu-id="8c5ef-432">MaximumBackoff = 30 sec.</span></span> <br/> <span data-ttu-id="8c5ef-433">DeltaBackoff = 300 ms</span><span class="sxs-lookup"><span data-stu-id="8c5ef-433">DeltaBackoff = 300 msec.</span></span> <br/> <span data-ttu-id="8c5ef-434">TimeBuffer = 300 ms</span><span class="sxs-lookup"><span data-stu-id="8c5ef-434">TimeBuffer = 300 msec.</span></span> <br/> <span data-ttu-id="8c5ef-435">MaxRetryCount = 2</span><span class="sxs-lookup"><span data-stu-id="8c5ef-435">MaxRetryCount = 2</span></span> | <span data-ttu-id="8c5ef-436">1. Versuch: Verzögerung: 0 s</span><span class="sxs-lookup"><span data-stu-id="8c5ef-436">Attempt 1: Delay 0 sec.</span></span> <br/> <span data-ttu-id="8c5ef-437">2. Versuch: Verzögerung: ~300 ms</span><span class="sxs-lookup"><span data-stu-id="8c5ef-437">Attempt 2: Delay ~300 msec.</span></span> <br/> <span data-ttu-id="8c5ef-438">3. Versuch: Verzögerung: ~900 ms</span><span class="sxs-lookup"><span data-stu-id="8c5ef-438">Attempt 3: Delay ~900 msec.</span></span> |
| <span data-ttu-id="8c5ef-439">Hintergrund oder Batch</span><span class="sxs-lookup"><span data-stu-id="8c5ef-439">Background or batch</span></span> | <span data-ttu-id="8c5ef-440">30 Sekunden</span><span class="sxs-lookup"><span data-stu-id="8c5ef-440">30 seconds</span></span> | <span data-ttu-id="8c5ef-441">Exponentiell</span><span class="sxs-lookup"><span data-stu-id="8c5ef-441">Exponential</span></span> | <span data-ttu-id="8c5ef-442">MinimumBackoff = 1</span><span class="sxs-lookup"><span data-stu-id="8c5ef-442">MinimumBackoff = 1</span></span> <br/> <span data-ttu-id="8c5ef-443">MaximumBackoff = 30 s</span><span class="sxs-lookup"><span data-stu-id="8c5ef-443">MaximumBackoff = 30 sec.</span></span> <br/> <span data-ttu-id="8c5ef-444">DeltaBackoff = 1,75 s</span><span class="sxs-lookup"><span data-stu-id="8c5ef-444">DeltaBackoff = 1.75 sec.</span></span> <br/> <span data-ttu-id="8c5ef-445">TimeBuffer = 5 s</span><span class="sxs-lookup"><span data-stu-id="8c5ef-445">TimeBuffer = 5 sec.</span></span> <br/> <span data-ttu-id="8c5ef-446">MaxRetryCount = 3</span><span class="sxs-lookup"><span data-stu-id="8c5ef-446">MaxRetryCount = 3</span></span> | <span data-ttu-id="8c5ef-447">1. Versuch: Verzögerung: ~1 s</span><span class="sxs-lookup"><span data-stu-id="8c5ef-447">Attempt 1: Delay ~1 sec.</span></span> <br/> <span data-ttu-id="8c5ef-448">2. Versuch: Verzögerung: ~3 s</span><span class="sxs-lookup"><span data-stu-id="8c5ef-448">Attempt 2: Delay ~3 sec.</span></span> <br/> <span data-ttu-id="8c5ef-449">3. Versuch: Verzögerung: ~6 ms</span><span class="sxs-lookup"><span data-stu-id="8c5ef-449">Attempt 3: Delay ~6 msec.</span></span> <br/> <span data-ttu-id="8c5ef-450">4. Versuch: Verzögerung: ~13 ms</span><span class="sxs-lookup"><span data-stu-id="8c5ef-450">Attempt 4: Delay ~13 msec.</span></span> |

<span data-ttu-id="8c5ef-451">\*Ohne Berücksichtigung der Verzögerung, die hinzukommt, wenn eine Antwort vom Typ „Server ausgelastet“ empfangen wird.</span><span class="sxs-lookup"><span data-stu-id="8c5ef-451">\* Not including additional delay that is added if a Server Busy response is received.</span></span>

### <a name="telemetry"></a><span data-ttu-id="8c5ef-452">Telemetrie</span><span class="sxs-lookup"><span data-stu-id="8c5ef-452">Telemetry</span></span>

<span data-ttu-id="8c5ef-453">Service Bus protokollieren Wiederholungen als ETW-Ereignisse mit einer **EventSource**.</span><span class="sxs-lookup"><span data-stu-id="8c5ef-453">Service Bus logs retries as ETW events using an **EventSource**.</span></span> <span data-ttu-id="8c5ef-454">Sie müssen einen **EventListener** an die Ereignisquelle anfügen, um die Ereignisse zu erfassen und in Performance-Viewer anzuzeigen oder diese in ein geeignetes Ziel-Protokoll schreiben.</span><span class="sxs-lookup"><span data-stu-id="8c5ef-454">You must attach an **EventListener** to the event source to capture the events and view them in Performance Viewer, or write them to a suitable destination log.</span></span> <span data-ttu-id="8c5ef-455">Die Wiederholungsereignisse sind im folgenden Format:</span><span class="sxs-lookup"><span data-stu-id="8c5ef-455">The retry events are of the following form:</span></span>

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

### <a name="examples"></a><span data-ttu-id="8c5ef-456">Beispiele</span><span class="sxs-lookup"><span data-stu-id="8c5ef-456">Examples</span></span>

<span data-ttu-id="8c5ef-457">Das folgende Codebeispiel zeigt, wie Sie die Wiederholungsrichtlinie festlegen für:</span><span class="sxs-lookup"><span data-stu-id="8c5ef-457">The following code example shows how to set the retry policy for:</span></span>

- <span data-ttu-id="8c5ef-458">Einen Namespace-Manager.</span><span class="sxs-lookup"><span data-stu-id="8c5ef-458">A namespace manager.</span></span> <span data-ttu-id="8c5ef-459">Die Richtlinie gilt für alle Vorgänge dieses Managers und kann nicht durch einzelne Vorgänge überschrieben werden.</span><span class="sxs-lookup"><span data-stu-id="8c5ef-459">The policy applies to all operations on that manager, and cannot be overridden for individual operations.</span></span>
- <span data-ttu-id="8c5ef-460">Eine Messaging-Factory.</span><span class="sxs-lookup"><span data-stu-id="8c5ef-460">A messaging factory.</span></span> <span data-ttu-id="8c5ef-461">Die Richtlinie gilt für alle Clients, die von dieser Factory erstellt wurden, und kann bei der Erstellung einzelner Clients nicht überschrieben werden.</span><span class="sxs-lookup"><span data-stu-id="8c5ef-461">The policy applies to all clients created from that factory, and cannot be overridden when creating individual clients.</span></span>
- <span data-ttu-id="8c5ef-462">Ein einzelner Messaging Client.</span><span class="sxs-lookup"><span data-stu-id="8c5ef-462">An individual messaging client.</span></span> <span data-ttu-id="8c5ef-463">Nachdem ein Client erstellt wurde, können Sie die Wiederholungsrichtlinie für diesen festlegen.</span><span class="sxs-lookup"><span data-stu-id="8c5ef-463">After a client has been created, you can set the retry policy for that client.</span></span> <span data-ttu-id="8c5ef-464">Die Richtlinie gilt für alle Vorgänge auf dem Client.</span><span class="sxs-lookup"><span data-stu-id="8c5ef-464">The policy applies to all operations on that client.</span></span>

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

### <a name="more-information"></a><span data-ttu-id="8c5ef-465">Weitere Informationen</span><span class="sxs-lookup"><span data-stu-id="8c5ef-465">More information</span></span>

- [<span data-ttu-id="8c5ef-466">Asynchrone Nachrichtenmuster und Hochverfügbarkeit</span><span class="sxs-lookup"><span data-stu-id="8c5ef-466">Asynchronous messaging patterns and high availability</span></span>](/azure/service-bus-messaging/service-bus-async-messaging)

## <a name="service-fabric"></a><span data-ttu-id="8c5ef-467">Service Fabric</span><span class="sxs-lookup"><span data-stu-id="8c5ef-467">Service Fabric</span></span>

<span data-ttu-id="8c5ef-468">Die Verteilung von Reliable Services in einem Service Fabric-Cluster dient als Schutz vor den meisten potenziellen vorübergehenden Fehlern, die in diesem Artikel beschrieben wurden.</span><span class="sxs-lookup"><span data-stu-id="8c5ef-468">Distributing reliable services in a Service Fabric cluster guards against most of the potential transient faults discussed in this article.</span></span> <span data-ttu-id="8c5ef-469">Einige vorübergehende Fehler sind aber weiterhin möglich.</span><span class="sxs-lookup"><span data-stu-id="8c5ef-469">Some transient faults are still possible, however.</span></span> <span data-ttu-id="8c5ef-470">Für den Naming Service kann beispielsweise gerade eine Routingänderung durchgeführt werden, wenn er eine Anforderung erhält, sodass eine Ausnahme ausgelöst wird.</span><span class="sxs-lookup"><span data-stu-id="8c5ef-470">For example, the naming service might be in the middle of a routing change when it gets a request, causing it to throw an exception.</span></span> <span data-ttu-id="8c5ef-471">Wenn dieselbe Anforderung 100 Millisekunden später eintrifft, ist der Vorgang wahrscheinlich erfolgreich.</span><span class="sxs-lookup"><span data-stu-id="8c5ef-471">If the same request comes 100 milliseconds later, it will probably succeed.</span></span>

<span data-ttu-id="8c5ef-472">Intern verwaltet Service Fabric diese Art von vorübergehenden Fehlern.</span><span class="sxs-lookup"><span data-stu-id="8c5ef-472">Internally, Service Fabric manages this kind of transient fault.</span></span> <span data-ttu-id="8c5ef-473">Sie können einige Einstellungen konfigurieren, indem Sie beim Einrichten Ihrer Dienste die `OperationRetrySettings`-Klasse verwenden.</span><span class="sxs-lookup"><span data-stu-id="8c5ef-473">You can configure some settings by using the `OperationRetrySettings` class while setting up your services.</span></span> <span data-ttu-id="8c5ef-474">Der folgende Code enthält hierzu ein Beispiel.</span><span class="sxs-lookup"><span data-stu-id="8c5ef-474">The following code shows an example.</span></span> <span data-ttu-id="8c5ef-475">In den meisten Fällen sollte dies nicht erforderlich sein, sodass die Standardeinstellungen verwendet werden können.</span><span class="sxs-lookup"><span data-stu-id="8c5ef-475">In most cases, this should not be necessary, and the default settings will be fine.</span></span>

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

### <a name="more-information"></a><span data-ttu-id="8c5ef-476">Weitere Informationen</span><span class="sxs-lookup"><span data-stu-id="8c5ef-476">More information</span></span>

- [<span data-ttu-id="8c5ef-477">Behandlung von Remotingausnahmen</span><span class="sxs-lookup"><span data-stu-id="8c5ef-477">Remote exception handling</span></span>](/azure/service-fabric/service-fabric-reliable-services-communication-remoting#remoting-exception-handling)

## <a name="sql-database-using-adonet"></a><span data-ttu-id="8c5ef-478">SQL-Datenbank mit ADO.NET</span><span class="sxs-lookup"><span data-stu-id="8c5ef-478">SQL Database using ADO.NET</span></span>

<span data-ttu-id="8c5ef-479">SQL-Datenbank ist eine gehostete SQL-Datenbank, die in unterschiedlichen Größen und als Standard (freigegeben) und Premium (nicht freigegebenen)-Dienst verfügbar ist.</span><span class="sxs-lookup"><span data-stu-id="8c5ef-479">SQL Database is a hosted SQL database available in a range of sizes and as both a standard (shared) and premium (non-shared) service.</span></span>

### <a name="retry-mechanism"></a><span data-ttu-id="8c5ef-480">Wiederholungsmechanismus</span><span class="sxs-lookup"><span data-stu-id="8c5ef-480">Retry mechanism</span></span>

<span data-ttu-id="8c5ef-481">SQL-Datenbank hat keine integrierte Unterstützung für Wiederholungen, wenn auf diese mit ADO.NET zugegriffen wird.</span><span class="sxs-lookup"><span data-stu-id="8c5ef-481">SQL Database has no built-in support for retries when accessed using ADO.NET.</span></span> <span data-ttu-id="8c5ef-482">Allerdings können die Rückgabecodes von Anforderungen verwendet werden, um zu bestimmen, warum eine Anforderung fehlgeschlagen ist.</span><span class="sxs-lookup"><span data-stu-id="8c5ef-482">However, the return codes from requests can be used to determine why a request failed.</span></span> <span data-ttu-id="8c5ef-483">Weitere Informationen zur SQL-Datenbank-Drosselung finden Sie unter [Ressourceneinschränkungen für Azure SQL-Datenbank](/azure/sql-database/sql-database-resource-limits).</span><span class="sxs-lookup"><span data-stu-id="8c5ef-483">For more information about SQL Database throttling, see [Azure SQL Database resource limits](/azure/sql-database/sql-database-resource-limits).</span></span> <span data-ttu-id="8c5ef-484">Eine Liste mit relevanten Fehlercodes finden Sie unter [SQL-Fehlercodes für SQL-Datenbank-Clientanwendungen](/azure/sql-database/sql-database-develop-error-messages).</span><span class="sxs-lookup"><span data-stu-id="8c5ef-484">For a list of relevant error codes, see [SQL error codes for SQL Database client applications](/azure/sql-database/sql-database-develop-error-messages).</span></span>

<span data-ttu-id="8c5ef-485">Sie können die Polly-Bibliothek verwenden, um Wiederholungen für SQL-Datenbank zu implementieren.</span><span class="sxs-lookup"><span data-stu-id="8c5ef-485">You can use the Polly library to implement retries for SQL Database.</span></span> <span data-ttu-id="8c5ef-486">Weitere Informationen finden Sie unter [Behandeln von vorübergehenden Fehlern mit Polly](#transient-fault-handling-with-polly).</span><span class="sxs-lookup"><span data-stu-id="8c5ef-486">See [Transient fault handling with Polly](#transient-fault-handling-with-polly).</span></span>

### <a name="retry-usage-guidance"></a><span data-ttu-id="8c5ef-487">Gebrauchsanleitung Wiederholungen</span><span class="sxs-lookup"><span data-stu-id="8c5ef-487">Retry usage guidance</span></span>

<span data-ttu-id="8c5ef-488">Beachten Sie den Zugriff auf SQL-Datenbank mit ADO.NET die folgenden Richtlinien:</span><span class="sxs-lookup"><span data-stu-id="8c5ef-488">Consider the following guidelines when accessing SQL Database using ADO.NET:</span></span>

- <span data-ttu-id="8c5ef-489">Wählen Sie die entsprechende Dienstoption (freigegeben oder Premium).</span><span class="sxs-lookup"><span data-stu-id="8c5ef-489">Choose the appropriate service option (shared or premium).</span></span> <span data-ttu-id="8c5ef-490">Bei einer freigegebenen Instanz kann es möglicherweise zu längeren Verbindungsverzögerungen als üblich und Drosselung durch die Verwendung des gemeinsam genutzten Servers durch andere Mandanten kommen.</span><span class="sxs-lookup"><span data-stu-id="8c5ef-490">A shared instance may suffer longer than usual connection delays and throttling due to the usage by other tenants of the shared server.</span></span> <span data-ttu-id="8c5ef-491">Wenn Vorgänge mit vorhersagbarerer Leistung und zuverlässig geringe Latenz erforderlich sind, sollten Sie die Premium-Option wählen.</span><span class="sxs-lookup"><span data-stu-id="8c5ef-491">If more predictable performance and reliable low latency operations are required, consider choosing the premium option.</span></span>
- <span data-ttu-id="8c5ef-492">Stellen Sie sicher, dass Sie Wiederholungen auf der entsprechenden Ebene oder im entsprechenden Bereich durchführen, um nicht-idempotente Operationen zu vermeiden, die Dateninkonsistenzen verursachen.</span><span class="sxs-lookup"><span data-stu-id="8c5ef-492">Ensure that you perform retries at the appropriate level or scope to avoid non-idempotent operations causing inconsistency in the data.</span></span> <span data-ttu-id="8c5ef-493">Im Idealfall sollten alle Vorgänge idempotent sein, sodass sie ohne Inkonsistenzen wiederholt werden können.</span><span class="sxs-lookup"><span data-stu-id="8c5ef-493">Ideally, all operations should be idempotent so that they can be repeated without causing inconsistency.</span></span> <span data-ttu-id="8c5ef-494">Wenn dies nicht der Fall ist, sollte die Wiederholung auf einer Ebene oder in einem Bereich durchgeführt werden, die ermöglichen, dass alle zugehörigen Änderungen rückgängig gemacht werden können, wenn ein Vorgang fehlschlägt; zum Beispiel im Transaktionsbereich.</span><span class="sxs-lookup"><span data-stu-id="8c5ef-494">Where this is not the case, the retry should be performed at a level or scope that allows all related changes to be undone if one operation fails; for example, from within a transactional scope.</span></span> <span data-ttu-id="8c5ef-495">Weitere Informationen finden Sie unter [Clouddienst-Grundlagen Datenzugriffsebene - Handhabung vorübergehender Fehler](https://social.technet.microsoft.com/wiki/contents/articles/18665.cloud-service-fundamentals-data-access-layer-transient-fault-handling.aspx#Idempotent_Guarantee).</span><span class="sxs-lookup"><span data-stu-id="8c5ef-495">For more information, see [Cloud Service Fundamentals Data Access Layer – Transient Fault Handling](https://social.technet.microsoft.com/wiki/contents/articles/18665.cloud-service-fundamentals-data-access-layer-transient-fault-handling.aspx#Idempotent_Guarantee).</span></span>
- <span data-ttu-id="8c5ef-496">Eine Strategie mit festgelegtem Intervall wird für die Verwendung mit Azure SQL-Datenbank nicht empfohlen, mit Ausnahme von interaktiven Szenarien, wo nur wenige Wiederholungen in sehr kurzen Intervallen vorkommen.</span><span class="sxs-lookup"><span data-stu-id="8c5ef-496">A fixed interval strategy is not recommended for use with Azure SQL Database except for interactive scenarios where there are only a few retries at very short intervals.</span></span> <span data-ttu-id="8c5ef-497">Ziehen Sie stattdessen eine exponentielle Backoff-Strategie für die meisten Szenarien in Betracht.</span><span class="sxs-lookup"><span data-stu-id="8c5ef-497">Instead, consider using an exponential back-off strategy for the majority of scenarios.</span></span>
- <span data-ttu-id="8c5ef-498">Wählen Sie einen geeigneten Wert für die Verbindungs- und Befehls-Timeouts bei der Definition von Verbindungen.</span><span class="sxs-lookup"><span data-stu-id="8c5ef-498">Choose a suitable value for the connection and command timeouts when defining connections.</span></span> <span data-ttu-id="8c5ef-499">Ein zu kurzes Timeout kann zu vorzeitigen Fehlern bei Verbindungen führen, wenn die Datenbank ausgelastet ist.</span><span class="sxs-lookup"><span data-stu-id="8c5ef-499">Too short a timeout may result in premature failures of connections when the database is busy.</span></span> <span data-ttu-id="8c5ef-500">Ein zu langes Timeout kann verhindern, dass die Wiederholungslogik ordnungsgemäß funktioniert, da sie vor der Erkennung einer fehlgeschlagenen Verbindung zu lange wartet.</span><span class="sxs-lookup"><span data-stu-id="8c5ef-500">Too long a timeout may prevent the retry logic working correctly by waiting too long before detecting a failed connection.</span></span> <span data-ttu-id="8c5ef-501">Der Wert für das Timeout ist eine Komponente der End-to-End-Latenz. Sie wird effektiv der Wiederholungsverzögerung hinzugefügt, die in der Wiederholungsrichtlinie für jeden Wiederholungsversuch festgelegt ist.</span><span class="sxs-lookup"><span data-stu-id="8c5ef-501">The value of the timeout is a component of the end-to-end latency; it is effectively added to the retry delay specified in the retry policy for every retry attempt.</span></span>
- <span data-ttu-id="8c5ef-502">Schließen Sie die Verbindung nach einer bestimmten Anzahl von Wiederholungen, selbst wenn Sie eine exponentielle Backoff-Wiederholungslogik verwenden, und wiederholen Sie den Vorgang auf einer neuen Verbindung.</span><span class="sxs-lookup"><span data-stu-id="8c5ef-502">Close the connection after a certain number of retries, even when using an exponential back off retry logic, and retry the operation on a new connection.</span></span> <span data-ttu-id="8c5ef-503">Das mehrmalige Wiederholen des gleichen Vorgangs für dieselbe Verbindung kann ein Faktor sein, der zu Verbindungsproblemen beiträgt.</span><span class="sxs-lookup"><span data-stu-id="8c5ef-503">Retrying the same operation multiple times on the same connection can be a factor that contributes to connection problems.</span></span> <span data-ttu-id="8c5ef-504">Beispiele für dieses Verfahrens finden Sie unter [Clouddienst-Grundlagen Datenzugriffsebene - Handhabung vorübergehender Fehler](https://social.technet.microsoft.com/wiki/contents/articles/18665.cloud-service-fundamentals-data-access-layer-transient-fault-handling.aspx).</span><span class="sxs-lookup"><span data-stu-id="8c5ef-504">For an example of this technique, see [Cloud Service Fundamentals Data Access Layer – Transient Fault Handling](https://social.technet.microsoft.com/wiki/contents/articles/18665.cloud-service-fundamentals-data-access-layer-transient-fault-handling.aspx).</span></span>
- <span data-ttu-id="8c5ef-505">Wenn Verbindungspooling verwendet wird (Standardeinstellung), besteht die Möglichkeit, dass auch nach dem Schließen und erneuten Öffnen einer Verbindung dieselbe Verbindung aus dem Pool ausgewählt wird.</span><span class="sxs-lookup"><span data-stu-id="8c5ef-505">When connection pooling is in use (the default) there is a chance that the same connection will be chosen from the pool, even after closing and reopening a connection.</span></span> <span data-ttu-id="8c5ef-506">Wenn dies der Fall ist, ist ein Verfahren zu Behebung das Aufrufen der **ClearPool**-Methode der **SqlConnection**-Klasse, um die Verbindung als nicht wiederverwendbar zu markieren.</span><span class="sxs-lookup"><span data-stu-id="8c5ef-506">If this is the case, a technique to resolve it is to call the **ClearPool** method of the **SqlConnection** class to mark the connection as not reusable.</span></span> <span data-ttu-id="8c5ef-507">Allerdings sollten Sie dies erst nach mehreren fehlgeschlagenen Verbindungsversuchen tun und nur, wenn die spezifische Klasse des vorübergehende Fehler wie z. B. SQL-Timeouts (Fehlercode: -2) im Zusammenhang mit fehlerhaften Verbindungen auftritt.</span><span class="sxs-lookup"><span data-stu-id="8c5ef-507">However, you should do this only after several connection attempts have failed, and only when encountering the specific class of transient failures such as SQL timeouts (error code -2) related to faulty connections.</span></span>
- <span data-ttu-id="8c5ef-508">Wenn der Datenzugriffscode als **TransactionScope** -Instanzen initiierte Transaktionen verwendet, sollte die Wiederholungslogik erneut eine Verbindung herstellen und einen neuen Transaktionsbereich initiieren.</span><span class="sxs-lookup"><span data-stu-id="8c5ef-508">If the data access code uses transactions initiated as **TransactionScope** instances, the retry logic should reopen the connection and initiate a new transaction scope.</span></span> <span data-ttu-id="8c5ef-509">Aus diesem Grund sollte der wiederholbare Codeblock den gesamten Bereich der Transaktion umfassen.</span><span class="sxs-lookup"><span data-stu-id="8c5ef-509">For this reason, the retryable code block should encompass the entire scope of the transaction.</span></span>

<span data-ttu-id="8c5ef-510">Erwägen Sie, mit den folgenden Einstellungen für Wiederholungsvorgänge zu beginnen.</span><span class="sxs-lookup"><span data-stu-id="8c5ef-510">Consider starting with following settings for retrying operations.</span></span> <span data-ttu-id="8c5ef-511">Hierbei handelt es sich um allgemeine Einstellungen und Sie sollten die Vorgänge überwachen und die Werte entsprechend Ihrem Szenario optimieren.</span><span class="sxs-lookup"><span data-stu-id="8c5ef-511">These are general purpose settings, and you should monitor the operations and fine tune the values to suit your own scenario.</span></span>

| <span data-ttu-id="8c5ef-512">**Context**</span><span class="sxs-lookup"><span data-stu-id="8c5ef-512">**Context**</span></span> | <span data-ttu-id="8c5ef-513">**Beispiel-Ziel E2E<br />Maximale Wartezeit**</span><span class="sxs-lookup"><span data-stu-id="8c5ef-513">**Sample target E2E<br />max latency**</span></span> | <span data-ttu-id="8c5ef-514">**Wiederholungsstrategie**</span><span class="sxs-lookup"><span data-stu-id="8c5ef-514">**Retry strategy**</span></span> | <span data-ttu-id="8c5ef-515">**Einstellungen**</span><span class="sxs-lookup"><span data-stu-id="8c5ef-515">**Settings**</span></span> | <span data-ttu-id="8c5ef-516">**Werte**</span><span class="sxs-lookup"><span data-stu-id="8c5ef-516">**Values**</span></span> | <span data-ttu-id="8c5ef-517">**So funktioniert's**</span><span class="sxs-lookup"><span data-stu-id="8c5ef-517">**How it works**</span></span> |
| --- | --- | --- | --- | --- | --- |
| <span data-ttu-id="8c5ef-518">Interaktiv, Benutzeroberfläche</span><span class="sxs-lookup"><span data-stu-id="8c5ef-518">Interactive, UI,</span></span><br /><span data-ttu-id="8c5ef-519">oder Vordergrund</span><span class="sxs-lookup"><span data-stu-id="8c5ef-519">or foreground</span></span> |<span data-ttu-id="8c5ef-520">2 Sek</span><span class="sxs-lookup"><span data-stu-id="8c5ef-520">2 sec</span></span> |<span data-ttu-id="8c5ef-521">FixedInterval</span><span class="sxs-lookup"><span data-stu-id="8c5ef-521">FixedInterval</span></span> |<span data-ttu-id="8c5ef-522">Anzahl der Wiederholungen</span><span class="sxs-lookup"><span data-stu-id="8c5ef-522">Retry count</span></span><br /><span data-ttu-id="8c5ef-523">Wiederholungsintervall</span><span class="sxs-lookup"><span data-stu-id="8c5ef-523">Retry interval</span></span><br /><span data-ttu-id="8c5ef-524">Erster schneller Wiederholungsversuch</span><span class="sxs-lookup"><span data-stu-id="8c5ef-524">First fast retry</span></span> |<span data-ttu-id="8c5ef-525">3</span><span class="sxs-lookup"><span data-stu-id="8c5ef-525">3</span></span><br /><span data-ttu-id="8c5ef-526">500 ms</span><span class="sxs-lookup"><span data-stu-id="8c5ef-526">500 ms</span></span><br /><span data-ttu-id="8c5ef-527">true</span><span class="sxs-lookup"><span data-stu-id="8c5ef-527">true</span></span> |<span data-ttu-id="8c5ef-528">Versuch 1 – Verzögerung 0 Sek.</span><span class="sxs-lookup"><span data-stu-id="8c5ef-528">Attempt 1 - delay 0 sec</span></span><br /><span data-ttu-id="8c5ef-529">Versuch 2 – Verzögerung 500 ms</span><span class="sxs-lookup"><span data-stu-id="8c5ef-529">Attempt 2 - delay 500 ms</span></span><br /><span data-ttu-id="8c5ef-530">Versuch 3 – Verzögerung 500 ms</span><span class="sxs-lookup"><span data-stu-id="8c5ef-530">Attempt 3 - delay 500 ms</span></span> |
| <span data-ttu-id="8c5ef-531">Hintergrund</span><span class="sxs-lookup"><span data-stu-id="8c5ef-531">Background</span></span><br /><span data-ttu-id="8c5ef-532">oder Batch</span><span class="sxs-lookup"><span data-stu-id="8c5ef-532">or batch</span></span> |<span data-ttu-id="8c5ef-533">30 Sek</span><span class="sxs-lookup"><span data-stu-id="8c5ef-533">30 sec</span></span> |<span data-ttu-id="8c5ef-534">ExponentialBackoff</span><span class="sxs-lookup"><span data-stu-id="8c5ef-534">ExponentialBackoff</span></span> |<span data-ttu-id="8c5ef-535">Anzahl der Wiederholungen</span><span class="sxs-lookup"><span data-stu-id="8c5ef-535">Retry count</span></span><br /><span data-ttu-id="8c5ef-536">Min. Backoff</span><span class="sxs-lookup"><span data-stu-id="8c5ef-536">Min back-off</span></span><br /><span data-ttu-id="8c5ef-537">Max. Backoff</span><span class="sxs-lookup"><span data-stu-id="8c5ef-537">Max back-off</span></span><br /><span data-ttu-id="8c5ef-538">Delta-Backoff</span><span class="sxs-lookup"><span data-stu-id="8c5ef-538">Delta back-off</span></span><br /><span data-ttu-id="8c5ef-539">Erster schneller Wiederholungsversuch</span><span class="sxs-lookup"><span data-stu-id="8c5ef-539">First fast retry</span></span> |<span data-ttu-id="8c5ef-540">5</span><span class="sxs-lookup"><span data-stu-id="8c5ef-540">5</span></span><br /><span data-ttu-id="8c5ef-541">0 Sek.</span><span class="sxs-lookup"><span data-stu-id="8c5ef-541">0 sec</span></span><br /><span data-ttu-id="8c5ef-542">60 Sekunden</span><span class="sxs-lookup"><span data-stu-id="8c5ef-542">60 sec</span></span><br /><span data-ttu-id="8c5ef-543">2 Sek</span><span class="sxs-lookup"><span data-stu-id="8c5ef-543">2 sec</span></span><br /><span data-ttu-id="8c5ef-544">false</span><span class="sxs-lookup"><span data-stu-id="8c5ef-544">false</span></span> |<span data-ttu-id="8c5ef-545">Versuch 1 – Verzögerung 0 Sek.</span><span class="sxs-lookup"><span data-stu-id="8c5ef-545">Attempt 1 - delay 0 sec</span></span><br /><span data-ttu-id="8c5ef-546">Versuch 2 – Verzögerung ca. 2 Sek.</span><span class="sxs-lookup"><span data-stu-id="8c5ef-546">Attempt 2 - delay ~2 sec</span></span><br /><span data-ttu-id="8c5ef-547">Versuch 3 – Verzögerung ca. 6 Sek.</span><span class="sxs-lookup"><span data-stu-id="8c5ef-547">Attempt 3 - delay ~6 sec</span></span><br /><span data-ttu-id="8c5ef-548">Versuch 4 – Verzögerung ca. 14 Sek.</span><span class="sxs-lookup"><span data-stu-id="8c5ef-548">Attempt 4 - delay ~14 sec</span></span><br /><span data-ttu-id="8c5ef-549">Versuch 5 – Verzögerung ca. 30 Sek.</span><span class="sxs-lookup"><span data-stu-id="8c5ef-549">Attempt 5 - delay ~30 sec</span></span> |

> [!NOTE]
> <span data-ttu-id="8c5ef-550">Die End-to-End-Latenzziele setzen das Standardtimeout für Verbindungen mit dem Dienst voraus.</span><span class="sxs-lookup"><span data-stu-id="8c5ef-550">The end-to-end latency targets assume the default timeout for connections to the service.</span></span> <span data-ttu-id="8c5ef-551">Wenn Sie längere Verbindungstimeouts angeben, wird die End-to-End-Latenz durch diese zusätzliche Zeit für jeden Wiederholungsversuch erweitert.</span><span class="sxs-lookup"><span data-stu-id="8c5ef-551">If you specify longer connection timeouts, the end-to-end latency will be extended by this additional time for every retry attempt.</span></span>

### <a name="examples"></a><span data-ttu-id="8c5ef-552">Beispiele</span><span class="sxs-lookup"><span data-stu-id="8c5ef-552">Examples</span></span>

<span data-ttu-id="8c5ef-553">In diesem Abschnitt wird veranschaulicht, wie Sie Polly zum Zugreifen auf Azure SQL-Datenbank verwenden können, indem Sie eine Reihe von Wiederholungsrichtlinien verwenden, die in der `Policy`-Klasse konfiguriert sind.</span><span class="sxs-lookup"><span data-stu-id="8c5ef-553">This section shows how you can use Polly to access Azure SQL Database using a set of retry policies configured in the `Policy` class.</span></span>

<span data-ttu-id="8c5ef-554">Der folgende Code enthält eine Erweiterungsmethode für die `SqlCommand`-Klasse, mit der `ExecuteAsync` mit exponentiell ansteigender Wartezeit aufgerufen wird.</span><span class="sxs-lookup"><span data-stu-id="8c5ef-554">The following code shows an extension method on the `SqlCommand` class that calls `ExecuteAsync` with exponential backoff.</span></span>

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

<span data-ttu-id="8c5ef-555">Diese asynchrone Erweiterungsmethode kann wie unten beschrieben verwendet werden.</span><span class="sxs-lookup"><span data-stu-id="8c5ef-555">This asynchronous extension method can be used as follows.</span></span>

```csharp
var sqlCommand = sqlConnection.CreateCommand();
sqlCommand.CommandText = "[some query]";

using (var reader = await sqlCommand.ExecuteReaderWithRetryAsync())
{
    // Do something with the values
}
```

### <a name="more-information"></a><span data-ttu-id="8c5ef-556">Weitere Informationen</span><span class="sxs-lookup"><span data-stu-id="8c5ef-556">More information</span></span>

- [<span data-ttu-id="8c5ef-557">Clouddienst-Grundlagen Datenzugriffsebene - Handhabung vorübergehender Fehler.</span><span class="sxs-lookup"><span data-stu-id="8c5ef-557">Cloud Service Fundamentals Data Access Layer – Transient Fault Handling</span></span>](https://social.technet.microsoft.com/wiki/contents/articles/18665.cloud-service-fundamentals-data-access-layer-transient-fault-handling.aspx)

<span data-ttu-id="8c5ef-558">Eine allgemeine Anleitung dazu, wie Sie für SQL-Datenbank den größtmöglichen Nutzen erzielen, finden Sie unter [Anleitung zu Leistung und Flexibilität von Azure SQL-Datenbanken](https://social.technet.microsoft.com/wiki/contents/articles/3507.windows-azure-sql-database-performance-and-elasticity-guide.aspx).</span><span class="sxs-lookup"><span data-stu-id="8c5ef-558">For general guidance on getting the most from SQL Database, see [Azure SQL Database performance and elasticity guide](https://social.technet.microsoft.com/wiki/contents/articles/3507.windows-azure-sql-database-performance-and-elasticity-guide.aspx).</span></span>

## <a name="sql-database-using-entity-framework-6"></a><span data-ttu-id="8c5ef-559">SQL-Datenbank mit Entity Framework 6</span><span class="sxs-lookup"><span data-stu-id="8c5ef-559">SQL Database using Entity Framework 6</span></span>

<span data-ttu-id="8c5ef-560">SQL-Datenbank ist eine gehostete SQL-Datenbank, die in unterschiedlichen Größen und als Standard (freigegeben) und Premium (nicht freigegebenen)-Dienst verfügbar ist.</span><span class="sxs-lookup"><span data-stu-id="8c5ef-560">SQL Database is a hosted SQL database available in a range of sizes and as both a standard (shared) and premium (non-shared) service.</span></span> <span data-ttu-id="8c5ef-561">Entity Framework ist eine objektrelationale Zuordnung, die .NET Entwicklern die  Arbeit mit relationalen Daten mithilfe von domänenspezifischen Objekten ermöglicht.</span><span class="sxs-lookup"><span data-stu-id="8c5ef-561">Entity Framework is an object-relational mapper that enables .NET developers to work with relational data using domain-specific objects.</span></span> <span data-ttu-id="8c5ef-562">Es entfällt die Notwendigkeit für den Großteil des Datenzugriffs-Codes, den Entwickler normalerweise schreiben müssen.</span><span class="sxs-lookup"><span data-stu-id="8c5ef-562">It eliminates the need for most of the data-access code that developers usually need to write.</span></span>

### <a name="retry-mechanism"></a><span data-ttu-id="8c5ef-563">Wiederholungsmechanismus</span><span class="sxs-lookup"><span data-stu-id="8c5ef-563">Retry mechanism</span></span>

<span data-ttu-id="8c5ef-564">Wenn auf SQL-Datenbank über Entity Framework 6.0 (oder eine höhere Version) zugegriffen wird, werden Wiederholungen über einen Mechanismus namens [Verbindungsstabilität/Wiederholungslogik](/ef/ef6/fundamentals/connection-resiliency/retry-logic) unterstützt.</span><span class="sxs-lookup"><span data-stu-id="8c5ef-564">Retry support is provided when accessing SQL Database using Entity Framework 6.0 and higher through a mechanism called [Connection resiliency / retry logic](/ef/ef6/fundamentals/connection-resiliency/retry-logic).</span></span> <span data-ttu-id="8c5ef-565">Die wichtigsten Features des Wiederholungsmechanismus sind:</span><span class="sxs-lookup"><span data-stu-id="8c5ef-565">The main features of the retry mechanism are:</span></span>

- <span data-ttu-id="8c5ef-566">Die primäre Abstraktion ist die **IDbExecutionStrategy** -Schnittstelle.</span><span class="sxs-lookup"><span data-stu-id="8c5ef-566">The primary abstraction is the **IDbExecutionStrategy** interface.</span></span> <span data-ttu-id="8c5ef-567">Diese Benutzeroberfläche:</span><span class="sxs-lookup"><span data-stu-id="8c5ef-567">This interface:</span></span>
  - <span data-ttu-id="8c5ef-568">Definiert die synchrone und asynchrone Methoden zum **Ausführen**\*.</span><span class="sxs-lookup"><span data-stu-id="8c5ef-568">Defines synchronous and asynchronous **Execute**\* methods.</span></span>
  - <span data-ttu-id="8c5ef-569">Definiert Klassen, die direkt verwendet oder in einem Datenbankkontext als eine Standardstrategie konfiguriert werden können, einem Anbieternamen oder einem Anbieternamen und Servernamen zugeordnet werden können.</span><span class="sxs-lookup"><span data-stu-id="8c5ef-569">Defines classes that can be used directly or can be configured on a database context as a default strategy, mapped to provider name, or mapped to a provider name and server name.</span></span> <span data-ttu-id="8c5ef-570">Wenn sie für einen Kontext konfiguriert ist, treten Wiederholungen auf der Ebene der einzelnen Datenbankvorgängen auf, von denen es möglicherweise mehrere für einen angegebenen Kontext gibt.</span><span class="sxs-lookup"><span data-stu-id="8c5ef-570">When configured on a context, retries occur at the level of individual database operations, of which there might be several for a given context operation.</span></span>
  - <span data-ttu-id="8c5ef-571">Definiert, wann und wie versucht wird, einen fehlgeschlagene Verbindung erneut herzustellen.</span><span class="sxs-lookup"><span data-stu-id="8c5ef-571">Defines when to retry a failed connection, and how.</span></span>
- <span data-ttu-id="8c5ef-572">Sie umfasst mehrere integrierte Implementierungen für die **IDbExecutionStrategy** -Schnittstelle:</span><span class="sxs-lookup"><span data-stu-id="8c5ef-572">It includes several built-in implementations of the **IDbExecutionStrategy** interface:</span></span>
  - <span data-ttu-id="8c5ef-573">Standard - keine Wiederholung.</span><span class="sxs-lookup"><span data-stu-id="8c5ef-573">Default - no retrying.</span></span>
  - <span data-ttu-id="8c5ef-574">Standard für SQL-Datenbank (automatisch) - kein erneuter Versuch, es werden jedoch Ausnahmen überprüft und in der SQL-Datenbank-Strategie mit der Empfehlung zur Verwendung eingebunden.</span><span class="sxs-lookup"><span data-stu-id="8c5ef-574">Default for SQL Database (automatic) - no retrying, but inspects exceptions and wraps them with suggestion to use the SQL Database strategy.</span></span>
  - <span data-ttu-id="8c5ef-575">Standardeinstellung für SQL-Datenbank - exponentiell (von der Basisklasse geerbt) plus SQL-Datenbank Erkennungslogik.</span><span class="sxs-lookup"><span data-stu-id="8c5ef-575">Default for SQL Database - exponential (inherited from base class) plus SQL Database detection logic.</span></span>
- <span data-ttu-id="8c5ef-576">Sie implementiert eine exponentielle Backoff-Strategie, die einen Zufallsgenerator enthält.</span><span class="sxs-lookup"><span data-stu-id="8c5ef-576">It implements an exponential back-off strategy that includes randomization.</span></span>
- <span data-ttu-id="8c5ef-577">Die integrierten Wiederholungsklassen sind zustandsbehaftet und nicht threadsicher.</span><span class="sxs-lookup"><span data-stu-id="8c5ef-577">The built-in retry classes are stateful and are not thread safe.</span></span> <span data-ttu-id="8c5ef-578">Sie können jedoch wiederverwendet werden, nachdem der aktuelle Vorgang abgeschlossen ist.</span><span class="sxs-lookup"><span data-stu-id="8c5ef-578">However, they can be reused after the current operation is completed.</span></span>
- <span data-ttu-id="8c5ef-579">Wenn die angegebene Wiederholungsanzahl überschritten wird, werden die Ergebnisse in einer neue Ausnahme umschlossen.</span><span class="sxs-lookup"><span data-stu-id="8c5ef-579">If the specified retry count is exceeded, the results are wrapped in a new exception.</span></span> <span data-ttu-id="8c5ef-580">Sie bringt die aktuelle Ausnahme nicht zum Vorschein.</span><span class="sxs-lookup"><span data-stu-id="8c5ef-580">It does not bubble up the current exception.</span></span>

### <a name="policy-configuration"></a><span data-ttu-id="8c5ef-581">Richtlinienkonfiguration</span><span class="sxs-lookup"><span data-stu-id="8c5ef-581">Policy configuration</span></span>

<span data-ttu-id="8c5ef-582">Beim Zugriff auf SQL-Datenbank mit Entity Framework 6.0 und höher wird Wiederholungsunterstützung  geboten.</span><span class="sxs-lookup"><span data-stu-id="8c5ef-582">Retry support is provided when accessing SQL Database using Entity Framework 6.0 and higher.</span></span> <span data-ttu-id="8c5ef-583">Wiederholungsrichtlinien werden programmgesteuert konfiguriert.</span><span class="sxs-lookup"><span data-stu-id="8c5ef-583">Retry policies are configured programmatically.</span></span> <span data-ttu-id="8c5ef-584">Die Konfiguration kann nicht pro Vorgang geändert werden.</span><span class="sxs-lookup"><span data-stu-id="8c5ef-584">The configuration cannot be changed on a per-operation basis.</span></span>

<span data-ttu-id="8c5ef-585">Wenn Sie eine Strategie im Kontext als Standard konfigurieren, geben Sie eine Funktion an, die bei Bedarf eine neue Strategie erstellt.</span><span class="sxs-lookup"><span data-stu-id="8c5ef-585">When configuring a strategy on the context as the default, you specify a function that creates a new strategy on demand.</span></span> <span data-ttu-id="8c5ef-586">Der folgende Code zeigt, wie Sie eine Wiederholungskonfigurationsklasse erstellen können, die die **DbConfiguration** -Basisklasse erweitert.</span><span class="sxs-lookup"><span data-stu-id="8c5ef-586">The following code shows how you can create a retry configuration class that extends the **DbConfiguration** base class.</span></span>

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

<span data-ttu-id="8c5ef-587">Sie können diese dann mit der **SetConfiguration**-Methode der **DbConfiguration**-Instanz als Standardwiederholungsstrategie für alle Vorgänge festlegen, wenn die Anwendung gestartet wird.</span><span class="sxs-lookup"><span data-stu-id="8c5ef-587">You can then specify this as the default retry strategy for all operations using the **SetConfiguration** method of the **DbConfiguration** instance when the application starts.</span></span> <span data-ttu-id="8c5ef-588">In der Standardeinstellung erkennt EF die Konfigurationsklasse automatisch und verwenden diese.</span><span class="sxs-lookup"><span data-stu-id="8c5ef-588">By default, EF will automatically discover and use the configuration class.</span></span>

```csharp
DbConfiguration.SetConfiguration(new BloggingContextConfiguration());
```

<span data-ttu-id="8c5ef-589">Sie können die Wiederholungskonfigurationsklasse für einen Kontext angeben, indem Sie die Kontextklasse mit einem **DbConfigurationType** -Attribut kommentieren.</span><span class="sxs-lookup"><span data-stu-id="8c5ef-589">You can specify the retry configuration class for a context by annotating the context class with a **DbConfigurationType** attribute.</span></span> <span data-ttu-id="8c5ef-590">Wenn Sie jedoch über nur eine Konfigurationsklasse verfügen, wird EF diese verwenden, ohne dass der Kontext kommentieren werden muss.</span><span class="sxs-lookup"><span data-stu-id="8c5ef-590">However, if you have only one configuration class, EF will use it without the need to annotate the context.</span></span>

```csharp
[DbConfigurationType(typeof(BloggingContextConfiguration))]
public class BloggingContext : DbContext
```

<span data-ttu-id="8c5ef-591">Wenn Sie unterschiedliche Wiederholungsstrategien für bestimmte Vorgänge verwenden oder Wiederholungen für bestimmte Vorgänge deaktivieren müssen, können Sie eine Konfigurationsklasse erstellen, mit der Sie Strategien anhalten oder  austauschen können, indem ein Flag in **CallContext**setzen.</span><span class="sxs-lookup"><span data-stu-id="8c5ef-591">If you need to use different retry strategies for specific operations, or disable retries for specific operations, you can create a configuration class that allows you to suspend or swap strategies by setting a flag in the **CallContext**.</span></span> <span data-ttu-id="8c5ef-592">Die Konfigurationsklasse kann dieses Flag verwenden, um Strategien zu wechseln oder die Strategie zu deaktivieren, die Sie bereitstellen und eine Standardstrategie verwenden.</span><span class="sxs-lookup"><span data-stu-id="8c5ef-592">The configuration class can use this flag to switch strategies, or disable the strategy you provide and use a default strategy.</span></span> <span data-ttu-id="8c5ef-593">Weitere Informationen finden Sie unter [Anhalten der Ausführungsstrategie](/ef/ef6/fundamentals/connection-resiliency/retry-logic#workaround-suspend-execution-strategy) (EF6 und höher).</span><span class="sxs-lookup"><span data-stu-id="8c5ef-593">For more information, see [Suspend Execution Strategy](/ef/ef6/fundamentals/connection-resiliency/retry-logic#workaround-suspend-execution-strategy) (EF6 onwards).</span></span>

<span data-ttu-id="8c5ef-594">Eine andere Technik für die Verwendung von bestimmten Wiederholungsstrategien für einzelne Vorgänge ist das Erstellen einer Instanz der erforderlichen Strategieklasse und das Liefern der gewünschten Einstellungen mithilfe von Parametern.</span><span class="sxs-lookup"><span data-stu-id="8c5ef-594">Another technique for using specific retry strategies for individual operations is to create an instance of the required strategy class and supply the desired settings through parameters.</span></span> <span data-ttu-id="8c5ef-595">Rufen Sie dann die **ExecuteAsync** -Methode auf.</span><span class="sxs-lookup"><span data-stu-id="8c5ef-595">You then invoke its **ExecuteAsync** method.</span></span>

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

<span data-ttu-id="8c5ef-596">Die einfachste Möglichkeit zum Verwenden einer **DbConfiguration**-Klasse ist es, diese in der gleichen Assembly wie die **DbContext**-Klasse zu platzieren.</span><span class="sxs-lookup"><span data-stu-id="8c5ef-596">The simplest way to use a **DbConfiguration** class is to locate it in the same assembly as the **DbContext** class.</span></span> <span data-ttu-id="8c5ef-597">Dies ist jedoch nicht geeignet, wenn der gleiche Kontext in verschiedenen Szenarios benötigt wird, wie z. B. bei anderen interaktiven Strategien und Hintergrundwiederholungsstrategien.</span><span class="sxs-lookup"><span data-stu-id="8c5ef-597">However, this is not appropriate when the same context is required in different scenarios, such as different interactive and background retry strategies.</span></span> <span data-ttu-id="8c5ef-598">Wenn die unterschiedlichen Kontexte in separaten AppDomains ausgeführt werden, können Sie die integrierte Unterstützung für das Angeben von Konfigurationsklassen in der Konfigurationsdatei verwenden oder explizit mithilfe von Code festlegen.</span><span class="sxs-lookup"><span data-stu-id="8c5ef-598">If the different contexts execute in separate AppDomains, you can use the built-in support for specifying configuration classes in the configuration file or set it explicitly using code.</span></span> <span data-ttu-id="8c5ef-599">Wenn die unterschiedlichen Kontexte in derselben AppDomain ausgeführt werden müssen, ist eine benutzerdefinierte Lösung erforderlich.</span><span class="sxs-lookup"><span data-stu-id="8c5ef-599">If the different contexts must execute in the same AppDomain, a custom solution will be required.</span></span>

<span data-ttu-id="8c5ef-600">Weitere Informationen finden Sie unter [Codebasierte Konfiguration (EF6 und höher)](/ef/ef6/fundamentals/configuring/code-based).</span><span class="sxs-lookup"><span data-stu-id="8c5ef-600">For more information, see [Code-Based Configuration](/ef/ef6/fundamentals/configuring/code-based) (EF6 onwards).</span></span>

<span data-ttu-id="8c5ef-601">Die folgende Tabelle zeigt die Standardeinstellungen für die integrierte Wiederholungsrichtlinie bei Verwendung von EF6.</span><span class="sxs-lookup"><span data-stu-id="8c5ef-601">The following table shows the default settings for the built-in retry policy when using EF6.</span></span>

| <span data-ttu-id="8c5ef-602">Einstellung</span><span class="sxs-lookup"><span data-stu-id="8c5ef-602">Setting</span></span> | <span data-ttu-id="8c5ef-603">Standardwert</span><span class="sxs-lookup"><span data-stu-id="8c5ef-603">Default value</span></span> | <span data-ttu-id="8c5ef-604">Bedeutung</span><span class="sxs-lookup"><span data-stu-id="8c5ef-604">Meaning</span></span> |
|---------|---------------|---------|
| <span data-ttu-id="8c5ef-605">Richtlinie</span><span class="sxs-lookup"><span data-stu-id="8c5ef-605">Policy</span></span> | <span data-ttu-id="8c5ef-606">Exponentiell</span><span class="sxs-lookup"><span data-stu-id="8c5ef-606">Exponential</span></span> | <span data-ttu-id="8c5ef-607">Exponentielles Backoff.</span><span class="sxs-lookup"><span data-stu-id="8c5ef-607">Exponential back-off.</span></span> |
| <span data-ttu-id="8c5ef-608">MaxRetryCount</span><span class="sxs-lookup"><span data-stu-id="8c5ef-608">MaxRetryCount</span></span> | <span data-ttu-id="8c5ef-609">5</span><span class="sxs-lookup"><span data-stu-id="8c5ef-609">5</span></span> | <span data-ttu-id="8c5ef-610">Die maximale Anzahl von Warnungen.</span><span class="sxs-lookup"><span data-stu-id="8c5ef-610">The maximum number of retries.</span></span> |
| <span data-ttu-id="8c5ef-611">MaxDelay</span><span class="sxs-lookup"><span data-stu-id="8c5ef-611">MaxDelay</span></span> | <span data-ttu-id="8c5ef-612">30 Sekunden</span><span class="sxs-lookup"><span data-stu-id="8c5ef-612">30 seconds</span></span> | <span data-ttu-id="8c5ef-613">Die maximale Verzögerung zwischen Wiederholungsversuchen.</span><span class="sxs-lookup"><span data-stu-id="8c5ef-613">The maximum delay between retries.</span></span> <span data-ttu-id="8c5ef-614">Dieser Wert hat keine Auswirkung auf die Berechnung der Verzögerungsreihen.</span><span class="sxs-lookup"><span data-stu-id="8c5ef-614">This value does not affect how the series of delays are computed.</span></span> <span data-ttu-id="8c5ef-615">Er definiert lediglich eine Obergrenze.</span><span class="sxs-lookup"><span data-stu-id="8c5ef-615">It only defines an upper bound.</span></span> |
| <span data-ttu-id="8c5ef-616">DefaultCoefficient</span><span class="sxs-lookup"><span data-stu-id="8c5ef-616">DefaultCoefficient</span></span> | <span data-ttu-id="8c5ef-617">1 Sekunde</span><span class="sxs-lookup"><span data-stu-id="8c5ef-617">1 second</span></span> | <span data-ttu-id="8c5ef-618">Der Koeffizient für die Berechnung des exponentiellen Backoffs.</span><span class="sxs-lookup"><span data-stu-id="8c5ef-618">The coefficient for the exponential back-off computation.</span></span> <span data-ttu-id="8c5ef-619">Dieser Wert kann nicht geändert werden.</span><span class="sxs-lookup"><span data-stu-id="8c5ef-619">This value cannot be changed.</span></span> |
| <span data-ttu-id="8c5ef-620">DefaultRandomFactor</span><span class="sxs-lookup"><span data-stu-id="8c5ef-620">DefaultRandomFactor</span></span> | <span data-ttu-id="8c5ef-621">1.1</span><span class="sxs-lookup"><span data-stu-id="8c5ef-621">1.1</span></span> | <span data-ttu-id="8c5ef-622">Der Multiplikator zum Hinzufügen einer beliebigen Verzögerung für die einzelnen Einträge.</span><span class="sxs-lookup"><span data-stu-id="8c5ef-622">The multiplier used to add a random delay for each entry.</span></span> <span data-ttu-id="8c5ef-623">Dieser Wert kann nicht geändert werden.</span><span class="sxs-lookup"><span data-stu-id="8c5ef-623">This value cannot be changed.</span></span> |
| <span data-ttu-id="8c5ef-624">DefaultExponentialBase</span><span class="sxs-lookup"><span data-stu-id="8c5ef-624">DefaultExponentialBase</span></span> | <span data-ttu-id="8c5ef-625">2</span><span class="sxs-lookup"><span data-stu-id="8c5ef-625">2</span></span> | <span data-ttu-id="8c5ef-626">Der Multiplikator für die Berechnung der nächsten Verzögerung.</span><span class="sxs-lookup"><span data-stu-id="8c5ef-626">The multiplier used to calculate the next delay.</span></span> <span data-ttu-id="8c5ef-627">Dieser Wert kann nicht geändert werden.</span><span class="sxs-lookup"><span data-stu-id="8c5ef-627">This value cannot be changed.</span></span> |

### <a name="retry-usage-guidance"></a><span data-ttu-id="8c5ef-628">Gebrauchsanleitung Wiederholungen</span><span class="sxs-lookup"><span data-stu-id="8c5ef-628">Retry usage guidance</span></span>

<span data-ttu-id="8c5ef-629">Beachten Sie den Zugriff auf SQL-Datenbank mit EF6 die folgenden Richtlinien:</span><span class="sxs-lookup"><span data-stu-id="8c5ef-629">Consider the following guidelines when accessing SQL Database using EF6:</span></span>

- <span data-ttu-id="8c5ef-630">Wählen Sie die entsprechende Dienstoption (freigegeben oder Premium).</span><span class="sxs-lookup"><span data-stu-id="8c5ef-630">Choose the appropriate service option (shared or premium).</span></span> <span data-ttu-id="8c5ef-631">Bei einer freigegebenen Instanz kann es möglicherweise zu längeren Verbindungsverzögerungen als üblich und Drosselung durch die Verwendung des gemeinsam genutzten Servers durch andere Mandanten kommen.</span><span class="sxs-lookup"><span data-stu-id="8c5ef-631">A shared instance may suffer longer than usual connection delays and throttling due to the usage by other tenants of the shared server.</span></span> <span data-ttu-id="8c5ef-632">Wenn Vorgänge mit vorhersagbarer Leistung und zuverlässig geringe Latenz erforderlich sind, sollten Sie die Premium-Option wählen.</span><span class="sxs-lookup"><span data-stu-id="8c5ef-632">If predictable performance and reliable low latency operations are required, consider choosing the premium option.</span></span>

- <span data-ttu-id="8c5ef-633">Eine Strategie mit festgelegtem Intervall wird für die Verwendung mit Azure SQL-Datenbank nicht empfohlen.</span><span class="sxs-lookup"><span data-stu-id="8c5ef-633">A fixed interval strategy is not recommended for use with Azure SQL Database.</span></span> <span data-ttu-id="8c5ef-634">Verwenden Sie stattdessen eine exponentielle Backoff-Strategie, da der Dienst möglicherweise überlastet ist und längere Verzögerungen mehr Zeit für die Wiederherstellung einräumen.</span><span class="sxs-lookup"><span data-stu-id="8c5ef-634">Instead, use an exponential back-off strategy because the service may be overloaded, and longer delays allow more time for it to recover.</span></span>

- <span data-ttu-id="8c5ef-635">Wählen Sie einen geeigneten Wert für die Verbindungs- und Befehls-Timeouts bei der Definition von Verbindungen.</span><span class="sxs-lookup"><span data-stu-id="8c5ef-635">Choose a suitable value for the connection and command timeouts when defining connections.</span></span> <span data-ttu-id="8c5ef-636">Stützen Sie das Timeout auf Ihren Geschäftslogikentwurf und auf Tests.</span><span class="sxs-lookup"><span data-stu-id="8c5ef-636">Base the timeout on both your business logic design and through testing.</span></span> <span data-ttu-id="8c5ef-637">Sie müssen diesen Wert möglicherweise mit der Zeit ändern, wenn sich die Datenmengen oder Geschäftsprozesse ändern.</span><span class="sxs-lookup"><span data-stu-id="8c5ef-637">You may need to modify this value over time as the volumes of data or the business processes change.</span></span> <span data-ttu-id="8c5ef-638">Ein zu kurzes Timeout kann zu vorzeitigen Fehlern bei Verbindungen führen, wenn die Datenbank ausgelastet ist.</span><span class="sxs-lookup"><span data-stu-id="8c5ef-638">Too short a timeout may result in premature failures of connections when the database is busy.</span></span> <span data-ttu-id="8c5ef-639">Ein zu langes Timeout kann verhindern, dass die Wiederholungslogik ordnungsgemäß funktioniert, da sie vor der Erkennung einer fehlgeschlagenen Verbindung zu lange wartet.</span><span class="sxs-lookup"><span data-stu-id="8c5ef-639">Too long a timeout may prevent the retry logic working correctly by waiting too long before detecting a failed connection.</span></span> <span data-ttu-id="8c5ef-640">Der Wert für das Timeout ist eine Komponente der End-to-End-Latenz, auch es nicht leicht ist zu ermitteln, wie viele Befehle beim Speichern des Kontexts ausgeführt werden.</span><span class="sxs-lookup"><span data-stu-id="8c5ef-640">The value of the timeout is a component of the end-to-end latency, although you cannot easily determine how many commands will execute when saving the context.</span></span> <span data-ttu-id="8c5ef-641">Sie können das Standardtimeout ändern, indem Sie die Einstellung der **CommandTimeout**-Eigenschaft der **DbContext**-Instanz festlegen.</span><span class="sxs-lookup"><span data-stu-id="8c5ef-641">You can change the default timeout by setting the **CommandTimeout** property of the **DbContext** instance.</span></span>

- <span data-ttu-id="8c5ef-642">Entity Framework unterstützt Wiederholungskonfigurationen, die in Konfigurationsdateien definiert sind.</span><span class="sxs-lookup"><span data-stu-id="8c5ef-642">Entity Framework supports retry configurations defined in configuration files.</span></span> <span data-ttu-id="8c5ef-643">Für maximale Flexibilität in Azure sollten Sie allerdings die Konfiguration in der Anwendung  programmgesteuert erstellen.</span><span class="sxs-lookup"><span data-stu-id="8c5ef-643">However, for maximum flexibility on Azure you should consider creating the configuration programmatically within the application.</span></span> <span data-ttu-id="8c5ef-644">Die einzelnen Parameter für die Wiederholungsrichtlinien, wie z. B. die Anzahl der Wiederholungen und die Wiederholungsintervalle können in der Dienstkonfigurationsdatei gespeichert und zur Laufzeit verwendet werden, um die entsprechenden Richtlinien zu erstellen.</span><span class="sxs-lookup"><span data-stu-id="8c5ef-644">The specific parameters for the retry policies, such as the number of retries and the retry intervals, can be stored in the service configuration file and used at runtime to create the appropriate policies.</span></span> <span data-ttu-id="8c5ef-645">Dadurch können die Einstellungen darin geändert werden, ohne dass die Anwendung neu gestartet werden muss.</span><span class="sxs-lookup"><span data-stu-id="8c5ef-645">This allows the settings to be changed without requiring the application to be restarted.</span></span>

<span data-ttu-id="8c5ef-646">Erwägen Sie, mit den folgenden Einstellungen für Wiederholungsvorgänge zu beginnen.</span><span class="sxs-lookup"><span data-stu-id="8c5ef-646">Consider starting with the following settings for retrying operations.</span></span> <span data-ttu-id="8c5ef-647">Sie können die Verzögerung zwischen den Wiederholungen nicht angeben (sie ist als eine exponentielle Sequenz festgelegt und generiert).</span><span class="sxs-lookup"><span data-stu-id="8c5ef-647">You cannot specify the delay between retry attempts (it is fixed and generated as an exponential sequence).</span></span> <span data-ttu-id="8c5ef-648">Sofern Sie keine benutzerdefinierte Wiederholungsstrategie erstellen, können Sie wie hier gezeigt nur die maximalen Werte angeben.</span><span class="sxs-lookup"><span data-stu-id="8c5ef-648">You can specify only the maximum values, as shown here; unless you create a custom retry strategy.</span></span> <span data-ttu-id="8c5ef-649">Hierbei handelt es sich um allgemeine Einstellungen und Sie sollten die Vorgänge überwachen und die Werte entsprechend Ihrem Szenario optimieren.</span><span class="sxs-lookup"><span data-stu-id="8c5ef-649">These are general purpose settings, and you should monitor the operations and fine tune the values to suit your own scenario.</span></span>

| <span data-ttu-id="8c5ef-650">**Context**</span><span class="sxs-lookup"><span data-stu-id="8c5ef-650">**Context**</span></span> | <span data-ttu-id="8c5ef-651">**Beispiel-Ziel E2E<br />Maximale Wartezeit**</span><span class="sxs-lookup"><span data-stu-id="8c5ef-651">**Sample target E2E<br />max latency**</span></span> | <span data-ttu-id="8c5ef-652">**Wiederholungsrichtlinie**</span><span class="sxs-lookup"><span data-stu-id="8c5ef-652">**Retry policy**</span></span> | <span data-ttu-id="8c5ef-653">**Einstellungen**</span><span class="sxs-lookup"><span data-stu-id="8c5ef-653">**Settings**</span></span> | <span data-ttu-id="8c5ef-654">**Werte**</span><span class="sxs-lookup"><span data-stu-id="8c5ef-654">**Values**</span></span> | <span data-ttu-id="8c5ef-655">**So funktioniert's**</span><span class="sxs-lookup"><span data-stu-id="8c5ef-655">**How it works**</span></span> |
| --- | --- | --- | --- | --- | --- |
| <span data-ttu-id="8c5ef-656">Interaktiv, Benutzeroberfläche</span><span class="sxs-lookup"><span data-stu-id="8c5ef-656">Interactive, UI,</span></span><br /><span data-ttu-id="8c5ef-657">oder Vordergrund</span><span class="sxs-lookup"><span data-stu-id="8c5ef-657">or foreground</span></span> |<span data-ttu-id="8c5ef-658">2 Sekunden</span><span class="sxs-lookup"><span data-stu-id="8c5ef-658">2 seconds</span></span> |<span data-ttu-id="8c5ef-659">Exponentiell</span><span class="sxs-lookup"><span data-stu-id="8c5ef-659">Exponential</span></span> |<span data-ttu-id="8c5ef-660">MaxRetryCount</span><span class="sxs-lookup"><span data-stu-id="8c5ef-660">MaxRetryCount</span></span><br /><span data-ttu-id="8c5ef-661">MaxDelay</span><span class="sxs-lookup"><span data-stu-id="8c5ef-661">MaxDelay</span></span> |<span data-ttu-id="8c5ef-662">3</span><span class="sxs-lookup"><span data-stu-id="8c5ef-662">3</span></span><br /><span data-ttu-id="8c5ef-663">750 ms</span><span class="sxs-lookup"><span data-stu-id="8c5ef-663">750 ms</span></span> |<span data-ttu-id="8c5ef-664">Versuch 1 – Verzögerung 0 Sek.</span><span class="sxs-lookup"><span data-stu-id="8c5ef-664">Attempt 1 - delay 0 sec</span></span><br /><span data-ttu-id="8c5ef-665">Versuch 2 – Verzögerung 750 ms</span><span class="sxs-lookup"><span data-stu-id="8c5ef-665">Attempt 2 - delay 750 ms</span></span><br /><span data-ttu-id="8c5ef-666">Versuch 3 - Verzögerung 750 ms</span><span class="sxs-lookup"><span data-stu-id="8c5ef-666">Attempt 3 – delay 750 ms</span></span> |
| <span data-ttu-id="8c5ef-667">Hintergrund</span><span class="sxs-lookup"><span data-stu-id="8c5ef-667">Background</span></span><br /> <span data-ttu-id="8c5ef-668">oder Batch</span><span class="sxs-lookup"><span data-stu-id="8c5ef-668">or batch</span></span> |<span data-ttu-id="8c5ef-669">30 Sekunden</span><span class="sxs-lookup"><span data-stu-id="8c5ef-669">30 seconds</span></span> |<span data-ttu-id="8c5ef-670">Exponentiell</span><span class="sxs-lookup"><span data-stu-id="8c5ef-670">Exponential</span></span> |<span data-ttu-id="8c5ef-671">MaxRetryCount</span><span class="sxs-lookup"><span data-stu-id="8c5ef-671">MaxRetryCount</span></span><br /><span data-ttu-id="8c5ef-672">MaxDelay</span><span class="sxs-lookup"><span data-stu-id="8c5ef-672">MaxDelay</span></span> |<span data-ttu-id="8c5ef-673">5</span><span class="sxs-lookup"><span data-stu-id="8c5ef-673">5</span></span><br /><span data-ttu-id="8c5ef-674">12 Sekunden</span><span class="sxs-lookup"><span data-stu-id="8c5ef-674">12 seconds</span></span> |<span data-ttu-id="8c5ef-675">Versuch 1 – Verzögerung 0 Sek.</span><span class="sxs-lookup"><span data-stu-id="8c5ef-675">Attempt 1 - delay 0 sec</span></span><br /><span data-ttu-id="8c5ef-676">Versuch 2 – Verzögerung ca. 1 Sek.</span><span class="sxs-lookup"><span data-stu-id="8c5ef-676">Attempt 2 - delay ~1 sec</span></span><br /><span data-ttu-id="8c5ef-677">Versuch 3 – Verzögerung ca. 3 Sek.</span><span class="sxs-lookup"><span data-stu-id="8c5ef-677">Attempt 3 - delay ~3 sec</span></span><br /><span data-ttu-id="8c5ef-678">Versuch 4 – Verzögerung ca. 7 Sek.</span><span class="sxs-lookup"><span data-stu-id="8c5ef-678">Attempt 4 - delay ~7 sec</span></span><br /><span data-ttu-id="8c5ef-679">Versuch 5 – Verzögerung ca. 12 Sek.</span><span class="sxs-lookup"><span data-stu-id="8c5ef-679">Attempt 5 - delay 12 sec</span></span> |

> [!NOTE]
> <span data-ttu-id="8c5ef-680">Die End-to-End-Latenzziele setzen das Standardtimeout für Verbindungen mit dem Dienst voraus.</span><span class="sxs-lookup"><span data-stu-id="8c5ef-680">The end-to-end latency targets assume the default timeout for connections to the service.</span></span> <span data-ttu-id="8c5ef-681">Wenn Sie längere Verbindungstimeouts angeben, wird die End-to-End-Latenz durch diese zusätzliche Zeit für jeden Wiederholungsversuch erweitert.</span><span class="sxs-lookup"><span data-stu-id="8c5ef-681">If you specify longer connection timeouts, the end-to-end latency will be extended by this additional time for every retry attempt.</span></span>

### <a name="examples"></a><span data-ttu-id="8c5ef-682">Beispiele</span><span class="sxs-lookup"><span data-stu-id="8c5ef-682">Examples</span></span>

<span data-ttu-id="8c5ef-683">Im folgenden Codebeispiel wird eine einfachen Datenzugriffslösung definiert, die Entity Framework verwendet.</span><span class="sxs-lookup"><span data-stu-id="8c5ef-683">The following code example defines a simple data access solution that uses Entity Framework.</span></span> <span data-ttu-id="8c5ef-684">Eine bestimmte Wiederholungsstrategie wird durch die Definition einer Instanz einer Klasse mit dem Namen **BlogConfiguration** festgelegt, die **DbConfiguration** erweitert.</span><span class="sxs-lookup"><span data-stu-id="8c5ef-684">It sets a specific retry strategy by defining an instance of a class named **BlogConfiguration** that extends **DbConfiguration**.</span></span>

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

<span data-ttu-id="8c5ef-685">Weitere Beispiele zur Verwendung des Entity Framework Wiederholungsmechanismus finden Sie unter [Verbindungsstabilität / Wiederholungslogik](/ef/ef6/fundamentals/connection-resiliency/retry-logic).</span><span class="sxs-lookup"><span data-stu-id="8c5ef-685">More examples of using the Entity Framework retry mechanism can be found in [Connection Resiliency / Retry Logic](/ef/ef6/fundamentals/connection-resiliency/retry-logic).</span></span>

### <a name="more-information"></a><span data-ttu-id="8c5ef-686">Weitere Informationen</span><span class="sxs-lookup"><span data-stu-id="8c5ef-686">More information</span></span>

- [<span data-ttu-id="8c5ef-687">Anleitung zu Leistung und Flexibilität von Azure SQL-Datenbanken</span><span class="sxs-lookup"><span data-stu-id="8c5ef-687">Azure SQL Database performance and elasticity guide</span></span>](https://social.technet.microsoft.com/wiki/contents/articles/3507.windows-azure-sql-database-performance-and-elasticity-guide.aspx)

## <a name="sql-database-using-entity-framework-core"></a><span data-ttu-id="8c5ef-688">SQL-Datenbank mit Entity Framework Core</span><span class="sxs-lookup"><span data-stu-id="8c5ef-688">SQL Database using Entity Framework Core</span></span>

<span data-ttu-id="8c5ef-689">[Entity Framework Core](/ef/core/) ist eine objektrelationale Zuordnung, die .NET Core-Entwicklern die Arbeit mit Daten mithilfe von domänenspezifischen Objekten ermöglicht.</span><span class="sxs-lookup"><span data-stu-id="8c5ef-689">[Entity Framework Core](/ef/core/) is an object-relational mapper that enables .NET Core developers to work with data using domain-specific objects.</span></span> <span data-ttu-id="8c5ef-690">Es entfällt die Notwendigkeit für den Großteil des Datenzugriffs-Codes, den Entwickler normalerweise schreiben müssen.</span><span class="sxs-lookup"><span data-stu-id="8c5ef-690">It eliminates the need for most of the data-access code that developers usually need to write.</span></span> <span data-ttu-id="8c5ef-691">Diese Version von Entity Framework wurde von Grund auf neu geschrieben und erbt nicht automatisch alle Features von EF6.x.</span><span class="sxs-lookup"><span data-stu-id="8c5ef-691">This version of Entity Framework was written from the ground up, and doesn't automatically inherit all the features from EF6.x.</span></span>

### <a name="retry-mechanism"></a><span data-ttu-id="8c5ef-692">Wiederholungsmechanismus</span><span class="sxs-lookup"><span data-stu-id="8c5ef-692">Retry mechanism</span></span>

<span data-ttu-id="8c5ef-693">Wenn auf SQL-Datenbank über Entity Framework Core zugegriffen wird, werden Wiederholungen über einen Mechanismus namens [Verbindungsstabilität](/ef/core/miscellaneous/connection-resiliency) unterstützt.</span><span class="sxs-lookup"><span data-stu-id="8c5ef-693">Retry support is provided when accessing SQL Database using Entity Framework Core through a mechanism called [connection resiliency](/ef/core/miscellaneous/connection-resiliency).</span></span> <span data-ttu-id="8c5ef-694">Verbindungsstabilität wurde mit EF Core 1.1.0 eingeführt.</span><span class="sxs-lookup"><span data-stu-id="8c5ef-694">Connection resiliency was introduced in EF Core 1.1.0.</span></span>

<span data-ttu-id="8c5ef-695">Die primäre Abstraktion ist die `IExecutionStrategy`-Schnittstelle.</span><span class="sxs-lookup"><span data-stu-id="8c5ef-695">The primary abstraction is the `IExecutionStrategy` interface.</span></span> <span data-ttu-id="8c5ef-696">Die Ausführungsstrategie für SQL Server, einschließlich SQL Azure, ist über die Ausnahmetypen informiert, die wiederholt werden können, und verfügt über sinnvolle Standardwerte für maximale Wiederholungsversuche, Verzögerung zwischen Wiederholungen usw.</span><span class="sxs-lookup"><span data-stu-id="8c5ef-696">The execution strategy for SQL Server, including SQL Azure, is aware of the exception types that can be retried and has sensible defaults for maximum retries, delay between retries, and so on.</span></span>

### <a name="examples"></a><span data-ttu-id="8c5ef-697">Beispiele</span><span class="sxs-lookup"><span data-stu-id="8c5ef-697">Examples</span></span>

<span data-ttu-id="8c5ef-698">Der folgende Code ermöglicht automatische Wiederholungen beim Konfigurieren des DbContext-Objekts, das eine Sitzung mit der Datenbank repräsentiert.</span><span class="sxs-lookup"><span data-stu-id="8c5ef-698">The following code enables automatic retries when configuring the DbContext object, which represents a session with the database.</span></span>

```csharp
protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
{
    optionsBuilder
        .UseSqlServer(
            @"Server=(localdb)\mssqllocaldb;Database=EFMiscellanous.ConnectionResiliency;Trusted_Connection=True;",
            options => options.EnableRetryOnFailure());
}
```

<span data-ttu-id="8c5ef-699">Der folgende Code veranschaulicht, wie Sie eine Transaktion mit automatischen Wiederholungsversuchen ausführen, indem Sie eine Ausführungsstrategie verwenden.</span><span class="sxs-lookup"><span data-stu-id="8c5ef-699">The following code shows how to execute a transaction with automatic retries, by using an execution strategy.</span></span> <span data-ttu-id="8c5ef-700">Die Transaktion wird in einem Delegaten definiert.</span><span class="sxs-lookup"><span data-stu-id="8c5ef-700">The transaction is defined in a delegate.</span></span> <span data-ttu-id="8c5ef-701">Wenn ein vorübergehender Fehler auftritt, wird der Delegat von der Ausführungsstrategie erneut aufgerufen.</span><span class="sxs-lookup"><span data-stu-id="8c5ef-701">If a transient failure occurs, the execution strategy will invoke the delegate again.</span></span>

```csharp
using (var db = new BloggingContext())
{
    var strategy = db.Database.CreateExecutionStrategy();

    strategy.Execute(() =>
    {
        using (var transaction = db.Database.BeginTransaction())
        {
            db.Blogs.Add(new Blog { Url = "https://blogs.msdn.com/dotnet" });
            db.SaveChanges();

            db.Blogs.Add(new Blog { Url = "https://blogs.msdn.com/visualstudio" });
            db.SaveChanges();

            transaction.Commit();
        }
    });
}
```

## <a name="azure-storage"></a><span data-ttu-id="8c5ef-702">Azure Storage</span><span class="sxs-lookup"><span data-stu-id="8c5ef-702">Azure Storage</span></span>

<span data-ttu-id="8c5ef-703">Azure Storage-Dienste umfassen Tabellen- und Blobspeicher sowie Dateien und Speicherwarteschlangen.</span><span class="sxs-lookup"><span data-stu-id="8c5ef-703">Azure Storage services include table and blob storage, files, and storage queues.</span></span>

### <a name="retry-mechanism"></a><span data-ttu-id="8c5ef-704">Wiederholungsmechanismus</span><span class="sxs-lookup"><span data-stu-id="8c5ef-704">Retry mechanism</span></span>

<span data-ttu-id="8c5ef-705">Wiederholungen treten auf individueller REST-Vorgangsebene auf und sind ein wesentlicher Bestandteil der Client-API-Implementierung.</span><span class="sxs-lookup"><span data-stu-id="8c5ef-705">Retries occur at the individual REST operation level and are an integral part of the client API implementation.</span></span> <span data-ttu-id="8c5ef-706">Das Client-Speicher SDK verwendet Klassen, die die [IExtendedRetryPolicy Schnittstelle](/dotnet/api/microsoft.windowsazure.storage.retrypolicies.iextendedretrypolicy)implementieren.</span><span class="sxs-lookup"><span data-stu-id="8c5ef-706">The client storage SDK uses classes that implement the [IExtendedRetryPolicy Interface](/dotnet/api/microsoft.windowsazure.storage.retrypolicies.iextendedretrypolicy).</span></span>

<span data-ttu-id="8c5ef-707">Es gibt verschiedene Implementierungen der Schnittstelle.</span><span class="sxs-lookup"><span data-stu-id="8c5ef-707">There are different implementations of the interface.</span></span> <span data-ttu-id="8c5ef-708">Speicherclients können Richtlinien auswählen, die speziell für den Zugriff auf Tabellen, Blobs und Warteschlangen entworfen wurden.</span><span class="sxs-lookup"><span data-stu-id="8c5ef-708">Storage clients can choose from policies specifically designed for accessing tables, blobs, and queues.</span></span> <span data-ttu-id="8c5ef-709">Jede Implementierung verwendet eine andere Wiederholungsstrategie, die im Wesentlichen das Wiederholungsintervall und andere Details definiert.</span><span class="sxs-lookup"><span data-stu-id="8c5ef-709">Each implementation uses a different retry strategy that essentially defines the retry interval and other details.</span></span>

<span data-ttu-id="8c5ef-710">Die integrierten Klassen bieten Unterstützung für linear (konstante Verzögerung) und exponentiell mit Zufallsgenerator-Wiederholungsintervallen.</span><span class="sxs-lookup"><span data-stu-id="8c5ef-710">The built-in classes provide support for linear (constant delay) and exponential with randomization retry intervals.</span></span> <span data-ttu-id="8c5ef-711">Es gibt auch eine Richtlinie für keine Wiederholung, wenn ein anderer Prozess Wiederholungen auf höherer Ebene behandelt.</span><span class="sxs-lookup"><span data-stu-id="8c5ef-711">There is also a no retry policy for use when another process is handling retries at a higher level.</span></span> <span data-ttu-id="8c5ef-712">Allerdings können Sie Ihre eigenen Wiederholungsklassen implementieren, wenn Sie spezielle Anforderungen haben, die von den integrierten Klassen nicht bereitgestellt werden.</span><span class="sxs-lookup"><span data-stu-id="8c5ef-712">However, you can implement your own retry classes if you have specific requirements not provided by the built-in classes.</span></span>

<span data-ttu-id="8c5ef-713">Abwechselnde Wiederholungen wechseln zwischen primären und sekundären Speicherdienststandorten, wenn Sie Lesezugriff auf georedundante Speicher (RA-GRS) verwenden und das Ergebnis der Anforderung ist ein wiederholbarer Fehler.</span><span class="sxs-lookup"><span data-stu-id="8c5ef-713">Alternate retries switch between primary and secondary storage service location if you are using read access geo-redundant storage (RA-GRS) and the result of the request is a retryable error.</span></span> <span data-ttu-id="8c5ef-714">Weitere Informationen finden Sie unter [Redundanzoptionen für Azure Storage](/azure/storage/common/storage-redundancy) .</span><span class="sxs-lookup"><span data-stu-id="8c5ef-714">See [Azure Storage Redundancy Options](/azure/storage/common/storage-redundancy) for more information.</span></span>

### <a name="policy-configuration"></a><span data-ttu-id="8c5ef-715">Richtlinienkonfiguration</span><span class="sxs-lookup"><span data-stu-id="8c5ef-715">Policy configuration</span></span>

<span data-ttu-id="8c5ef-716">Wiederholungsrichtlinien werden programmgesteuert konfiguriert.</span><span class="sxs-lookup"><span data-stu-id="8c5ef-716">Retry policies are configured programmatically.</span></span> <span data-ttu-id="8c5ef-717">Eine typische Vorgehensweise ist das Erstellen und Ausfüllen von **TableRequestOptions**-, **BlobRequestOptions**-, **FileRequestOptions**- oder **QueueRequestOptions**-Instanzen.</span><span class="sxs-lookup"><span data-stu-id="8c5ef-717">A typical procedure is to create and populate a **TableRequestOptions**, **BlobRequestOptions**, **FileRequestOptions**, or **QueueRequestOptions** instance.</span></span>

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

<span data-ttu-id="8c5ef-718">Die Anforderungsoptioneninstanz kann dann auf dem Client festgelegt werden und alle Operationen mit dem Client verwenden die angegebene Anforderungsoptionen.</span><span class="sxs-lookup"><span data-stu-id="8c5ef-718">The request options instance can then be set on the client, and all operations with the client will use the specified request options.</span></span>

```csharp
client.DefaultRequestOptions = interactiveRequestOption;
var stats = await client.GetServiceStatsAsync();
```

<span data-ttu-id="8c5ef-719">Sie können die Client-Anforderungsoptionen überschreiben, indem Sie eine ausgefüllte Instanz der Anforderungsoptionsklasse als Parameter an Vorgangsmethoden übergeben.</span><span class="sxs-lookup"><span data-stu-id="8c5ef-719">You can override the client request options by passing a populated instance of the request options class as a parameter to operation methods.</span></span>

```csharp
var stats = await client.GetServiceStatsAsync(interactiveRequestOption, operationContext: null);
```

<span data-ttu-id="8c5ef-720">Sie verwenden eine **OperationContext** -Instanz, um den auszuführenden Code anzugeben, wenn eine Wiederholung auftritt und ein Vorgang abgeschlossen wurde.</span><span class="sxs-lookup"><span data-stu-id="8c5ef-720">You use an **OperationContext** instance to specify the code to execute when a retry occurs and when an operation has completed.</span></span> <span data-ttu-id="8c5ef-721">Dieser Code kann Informationen über den Vorgang zur Verwendung in Protokollen und Telemetrie erfassen.</span><span class="sxs-lookup"><span data-stu-id="8c5ef-721">This code can collect information about the operation for use in logs and telemetry.</span></span>

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

<span data-ttu-id="8c5ef-722">Zusätzlich zur Angabe, ob ein Fehler für die Wiederholung geeignet ist, geben die erweiterten Wiederholungsrichtlinien ein **RetryContext** -Objekt zurück, das folgendes angibt: die Anzahl der Wiederholungen, die Ergebnisse der letzten Anforderung, ob die nächste Wiederholung im primären oder sekundären Standort ausgeführt werden (siehe Tabelle unten).</span><span class="sxs-lookup"><span data-stu-id="8c5ef-722">In addition to indicating whether a failure is suitable for retry, the extended retry policies return a **RetryContext** object that indicates the number of retries, the results of the last request, whether the next retry will happen in the primary or secondary location (see table below for details).</span></span> <span data-ttu-id="8c5ef-723">Die Eigenschaften des **RetryContext** Objekts können verwendet werden, um zu entscheiden, ob und wann eine Wiederholung versucht wird.</span><span class="sxs-lookup"><span data-stu-id="8c5ef-723">The properties of the **RetryContext** object can be used to decide if and when to attempt a retry.</span></span> <span data-ttu-id="8c5ef-724">Weitere Informationen finden Sie unter [IExtendedRetryPolicy.Evaluate-Methode](/dotnet/api/microsoft.windowsazure.storage.retrypolicies.iextendedretrypolicy.evaluate).</span><span class="sxs-lookup"><span data-stu-id="8c5ef-724">For more details, see [IExtendedRetryPolicy.Evaluate Method](/dotnet/api/microsoft.windowsazure.storage.retrypolicies.iextendedretrypolicy.evaluate).</span></span>

<span data-ttu-id="8c5ef-725">Die folgenden Tabellen enthalten die Standardeinstellungen für die integrierten Wiederholungsrichtlinien.</span><span class="sxs-lookup"><span data-stu-id="8c5ef-725">The following tables show the default settings for the built-in retry policies.</span></span>

<span data-ttu-id="8c5ef-726">**Anforderungsoptionen:**</span><span class="sxs-lookup"><span data-stu-id="8c5ef-726">**Request options:**</span></span>

| <span data-ttu-id="8c5ef-727">**Einstellung**</span><span class="sxs-lookup"><span data-stu-id="8c5ef-727">**Setting**</span></span> | <span data-ttu-id="8c5ef-728">**Standardwert**</span><span class="sxs-lookup"><span data-stu-id="8c5ef-728">**Default value**</span></span> | <span data-ttu-id="8c5ef-729">**Bedeutung**</span><span class="sxs-lookup"><span data-stu-id="8c5ef-729">**Meaning**</span></span> |
| --- | --- | --- |
| <span data-ttu-id="8c5ef-730">MaximumExecutionTime</span><span class="sxs-lookup"><span data-stu-id="8c5ef-730">MaximumExecutionTime</span></span> | <span data-ttu-id="8c5ef-731">Keine</span><span class="sxs-lookup"><span data-stu-id="8c5ef-731">None</span></span> | <span data-ttu-id="8c5ef-732">Maximale Ausführungszeit für die Anforderung, einschließlich aller möglichen Wiederholungsversuche.</span><span class="sxs-lookup"><span data-stu-id="8c5ef-732">Maximum execution time for the request, including all potential retry attempts.</span></span> <span data-ttu-id="8c5ef-733">Ist diese Option nicht angegeben, ist die für diese Anforderung zulässige Zeitspanne unendlich.</span><span class="sxs-lookup"><span data-stu-id="8c5ef-733">If it is not specified, then the amount of time that a request is permitted to take is unlimited.</span></span> <span data-ttu-id="8c5ef-734">Das heißt, es kann sein, dass die Anforderung nicht reagiert.</span><span class="sxs-lookup"><span data-stu-id="8c5ef-734">In other words, the request might hang.</span></span> |
| <span data-ttu-id="8c5ef-735">ServerTimeout</span><span class="sxs-lookup"><span data-stu-id="8c5ef-735">ServerTimeout</span></span> | <span data-ttu-id="8c5ef-736">Keine</span><span class="sxs-lookup"><span data-stu-id="8c5ef-736">None</span></span> | <span data-ttu-id="8c5ef-737">Server-Timeout-Intervall für die Anforderung (der Wert wird in Sekunden gerundet).</span><span class="sxs-lookup"><span data-stu-id="8c5ef-737">Server timeout interval for the request (value is rounded to seconds).</span></span> <span data-ttu-id="8c5ef-738">Wenn nicht angegeben, wird der Standardwert für alle Anforderungen an den Server verwendet.</span><span class="sxs-lookup"><span data-stu-id="8c5ef-738">If not specified, it will use the default value for all requests to the server.</span></span> <span data-ttu-id="8c5ef-739">In der Regel ist die beste Option, diese Einstellung auszulassen, sodass die Standardeinstellung des Servers verwendet wird.</span><span class="sxs-lookup"><span data-stu-id="8c5ef-739">Usually, the best option is to omit this setting so that the server default is used.</span></span> |
| <span data-ttu-id="8c5ef-740">LocationMode</span><span class="sxs-lookup"><span data-stu-id="8c5ef-740">LocationMode</span></span> | <span data-ttu-id="8c5ef-741">Keine</span><span class="sxs-lookup"><span data-stu-id="8c5ef-741">None</span></span> | <span data-ttu-id="8c5ef-742">Wenn das Speicherkonto mit der Replikationsoption des Lesezugriffs auf georedundanten Speicher (RA-GRS) erstellt wird, können Sie mit dem Speicherortmodus bestimmen, welche Stelle die Anforderung erhalten soll.</span><span class="sxs-lookup"><span data-stu-id="8c5ef-742">If the storage account is created with the Read access geo-redundant storage (RA-GRS) replication option, you can use the location mode to indicate which location should receive the request.</span></span> <span data-ttu-id="8c5ef-743">Wenn zum Beispiel **PrimaryThenSecondary** angegeben ist, werden Anforderungen immer zuerst an den primären Standort gesendet.</span><span class="sxs-lookup"><span data-stu-id="8c5ef-743">For example, if **PrimaryThenSecondary** is specified, requests are always sent to the primary location first.</span></span> <span data-ttu-id="8c5ef-744">Wenn eine Anforderung fehlschlägt, wird sie an den sekundären Standort gesendet.</span><span class="sxs-lookup"><span data-stu-id="8c5ef-744">If a request fails, it is sent to the secondary location.</span></span> |
| <span data-ttu-id="8c5ef-745">RetryPolicy</span><span class="sxs-lookup"><span data-stu-id="8c5ef-745">RetryPolicy</span></span> | <span data-ttu-id="8c5ef-746">ExponentialPolicy</span><span class="sxs-lookup"><span data-stu-id="8c5ef-746">ExponentialPolicy</span></span> | <span data-ttu-id="8c5ef-747">Details zu jeder Option finden Sie weiter unten.</span><span class="sxs-lookup"><span data-stu-id="8c5ef-747">See below for details of each option.</span></span> |

<span data-ttu-id="8c5ef-748">**Exponentielle Richtlinie:**</span><span class="sxs-lookup"><span data-stu-id="8c5ef-748">**Exponential policy:**</span></span>

| <span data-ttu-id="8c5ef-749">**Einstellung**</span><span class="sxs-lookup"><span data-stu-id="8c5ef-749">**Setting**</span></span> | <span data-ttu-id="8c5ef-750">**Standardwert**</span><span class="sxs-lookup"><span data-stu-id="8c5ef-750">**Default value**</span></span> | <span data-ttu-id="8c5ef-751">**Bedeutung**</span><span class="sxs-lookup"><span data-stu-id="8c5ef-751">**Meaning**</span></span> |
| --- | --- | --- |
| <span data-ttu-id="8c5ef-752">maxAttempt</span><span class="sxs-lookup"><span data-stu-id="8c5ef-752">maxAttempt</span></span> | <span data-ttu-id="8c5ef-753">3</span><span class="sxs-lookup"><span data-stu-id="8c5ef-753">3</span></span> | <span data-ttu-id="8c5ef-754">Anzahl der Wiederholungsversuche.</span><span class="sxs-lookup"><span data-stu-id="8c5ef-754">Number of retry attempts.</span></span> |
| <span data-ttu-id="8c5ef-755">deltaBackoff</span><span class="sxs-lookup"><span data-stu-id="8c5ef-755">deltaBackoff</span></span> | <span data-ttu-id="8c5ef-756">4 Sekunden</span><span class="sxs-lookup"><span data-stu-id="8c5ef-756">4 seconds</span></span> | <span data-ttu-id="8c5ef-757">Backoff-Intervall zwischen den Wiederholungen.</span><span class="sxs-lookup"><span data-stu-id="8c5ef-757">Back-off interval between retries.</span></span> <span data-ttu-id="8c5ef-758">Vielfache dieser Zeitspanne, einschließlich eines Zufallselements, werden für weitere Wiederholungsversuche verwendet.</span><span class="sxs-lookup"><span data-stu-id="8c5ef-758">Multiples of this timespan, including a random element, will be used for subsequent retry attempts.</span></span> |
| <span data-ttu-id="8c5ef-759">MinBackoff</span><span class="sxs-lookup"><span data-stu-id="8c5ef-759">MinBackoff</span></span> | <span data-ttu-id="8c5ef-760">3 Sekunden</span><span class="sxs-lookup"><span data-stu-id="8c5ef-760">3 seconds</span></span> | <span data-ttu-id="8c5ef-761">Wird zu allen Wiederholungsintervallen, die aus deltaBackoff berechnet werden, hinzugefügt.</span><span class="sxs-lookup"><span data-stu-id="8c5ef-761">Added to all retry intervals computed from deltaBackoff.</span></span> <span data-ttu-id="8c5ef-762">Dieser Wert kann nicht geändert werden.</span><span class="sxs-lookup"><span data-stu-id="8c5ef-762">This value cannot be changed.</span></span>
| <span data-ttu-id="8c5ef-763">MaxBackoff</span><span class="sxs-lookup"><span data-stu-id="8c5ef-763">MaxBackoff</span></span> | <span data-ttu-id="8c5ef-764">120 Sekunden</span><span class="sxs-lookup"><span data-stu-id="8c5ef-764">120 seconds</span></span> | <span data-ttu-id="8c5ef-765">MaxBackoff wird verwendet, wenn das berechnete Wiederholungsintervall größer als MaxBackoff ist.</span><span class="sxs-lookup"><span data-stu-id="8c5ef-765">MaxBackoff is used if the computed retry interval is greater than MaxBackoff.</span></span> <span data-ttu-id="8c5ef-766">Dieser Wert kann nicht geändert werden.</span><span class="sxs-lookup"><span data-stu-id="8c5ef-766">This value cannot be changed.</span></span> |

<span data-ttu-id="8c5ef-767">**Lineare Richtlinie:**</span><span class="sxs-lookup"><span data-stu-id="8c5ef-767">**Linear policy:**</span></span>

| <span data-ttu-id="8c5ef-768">**Einstellung**</span><span class="sxs-lookup"><span data-stu-id="8c5ef-768">**Setting**</span></span> | <span data-ttu-id="8c5ef-769">**Standardwert**</span><span class="sxs-lookup"><span data-stu-id="8c5ef-769">**Default value**</span></span> | <span data-ttu-id="8c5ef-770">**Bedeutung**</span><span class="sxs-lookup"><span data-stu-id="8c5ef-770">**Meaning**</span></span> |
| --- | --- | --- |
| <span data-ttu-id="8c5ef-771">maxAttempt</span><span class="sxs-lookup"><span data-stu-id="8c5ef-771">maxAttempt</span></span> | <span data-ttu-id="8c5ef-772">3</span><span class="sxs-lookup"><span data-stu-id="8c5ef-772">3</span></span> | <span data-ttu-id="8c5ef-773">Anzahl der Wiederholungsversuche.</span><span class="sxs-lookup"><span data-stu-id="8c5ef-773">Number of retry attempts.</span></span> |
| <span data-ttu-id="8c5ef-774">deltaBackoff</span><span class="sxs-lookup"><span data-stu-id="8c5ef-774">deltaBackoff</span></span> | <span data-ttu-id="8c5ef-775">30 Sekunden</span><span class="sxs-lookup"><span data-stu-id="8c5ef-775">30 seconds</span></span> | <span data-ttu-id="8c5ef-776">Backoff-Intervall zwischen den Wiederholungen.</span><span class="sxs-lookup"><span data-stu-id="8c5ef-776">Back-off interval between retries.</span></span> |

### <a name="retry-usage-guidance"></a><span data-ttu-id="8c5ef-777">Gebrauchsanleitung Wiederholungen</span><span class="sxs-lookup"><span data-stu-id="8c5ef-777">Retry usage guidance</span></span>

<span data-ttu-id="8c5ef-778">Beachten Sie die folgenden Richtlinien beim Zugriff auf Azure-Speicherdienste mit der Speicherclient-API:</span><span class="sxs-lookup"><span data-stu-id="8c5ef-778">Consider the following guidelines when accessing Azure storage services using the storage client API:</span></span>

- <span data-ttu-id="8c5ef-779">Verwenden der integrierten Wiederholungsrichtlinien aus dem Microsoft.WindowsAzure.Storage.RetryPolicies-Namespace, wo sie für Ihre Anforderungen geeignet sind.</span><span class="sxs-lookup"><span data-stu-id="8c5ef-779">Use the built-in retry policies from the Microsoft.WindowsAzure.Storage.RetryPolicies namespace where they are appropriate for your requirements.</span></span> <span data-ttu-id="8c5ef-780">In den meisten Fällen sind diese Richtlinien ausreichend.</span><span class="sxs-lookup"><span data-stu-id="8c5ef-780">In most cases, these policies will be sufficient.</span></span>

- <span data-ttu-id="8c5ef-781">Verwenden der **ExponentialRetry** Richtlinie in Batchvorgängen, Hintergrundaufgaben oder nicht interaktiven Szenarios.</span><span class="sxs-lookup"><span data-stu-id="8c5ef-781">Use the **ExponentialRetry** policy in batch operations, background tasks, or non-interactive scenarios.</span></span> <span data-ttu-id="8c5ef-782">In diesen Szenarien können Sie in der Regel dem Dienst mehr Zeit für die Wiederherstellung  zur Verfügung stellen - dadurch erhöht sich die Wahrscheinlichkeit, dass der Vorgang schließlich erfolgreich ist.</span><span class="sxs-lookup"><span data-stu-id="8c5ef-782">In these scenarios, you can typically allow more time for the service to recover—with a consequently increased chance of the operation eventually succeeding.</span></span>

- <span data-ttu-id="8c5ef-783">Geben Sie die Eigenschaft **MaximumExecutionTime** des **RequestOptions**-Parameters an, um die Gesamtausführungszeit zu beschränken. Berücksichtigen Sie bei der Auswahl eines Timeoutwerts aber den Typ und die Größe des Vorgangs.</span><span class="sxs-lookup"><span data-stu-id="8c5ef-783">Consider specifying the **MaximumExecutionTime** property of the **RequestOptions** parameter to limit the total execution time, but take into account the type and size of the operation when choosing a timeout value.</span></span>

- <span data-ttu-id="8c5ef-784">Wenn Sie eine benutzerdefinierte Wiederholung implementieren müssen, vermeiden Sie das Erstellen von Wrappern um die Speicherclienten-Klassen.</span><span class="sxs-lookup"><span data-stu-id="8c5ef-784">If you need to implement a custom retry, avoid creating wrappers around the storage client classes.</span></span> <span data-ttu-id="8c5ef-785">Verwenden Sie stattdessen die Funktionen zum Erweitern der vorhandenen Richtlinien über die **IExtendedRetryPolicy** Schnittstelle.</span><span class="sxs-lookup"><span data-stu-id="8c5ef-785">Instead, use the capabilities to extend the existing policies through the **IExtendedRetryPolicy** interface.</span></span>

- <span data-ttu-id="8c5ef-786">Bei Verwendung des Lesezugriffs auf georedundante Speicher (RA-GRS) können Sie **LocationMode** verwenden, um zu bestimmen, dass Wiederholungsversuche nicht auf die sekundäre schreibgeschützte Kopie des Speichers zugreifen sollen, wenn der primäre Zugriff fehlschlägt.</span><span class="sxs-lookup"><span data-stu-id="8c5ef-786">If you are using read access geo-redundant storage (RA-GRS) you can use the **LocationMode** to specify that retry attempts will access the secondary read-only copy of the store should the primary access fail.</span></span> <span data-ttu-id="8c5ef-787">Bei Verwendung dieser Option müssen Sie jedoch sicherstellen, dass die Anwendung erfolgreich mit Daten arbeiten kann, die möglicherweise veraltet sind, wenn die Replikation vom primären Speicher noch nicht abgeschlossen wurde.</span><span class="sxs-lookup"><span data-stu-id="8c5ef-787">However, when using this option you must ensure that your application can work successfully with data that may be stale if the replication from the primary store has not yet completed.</span></span>

<span data-ttu-id="8c5ef-788">Erwägen Sie, mit den folgenden Einstellungen für Wiederholungsvorgänge zu beginnen.</span><span class="sxs-lookup"><span data-stu-id="8c5ef-788">Consider starting with following settings for retrying operations.</span></span> <span data-ttu-id="8c5ef-789">Hierbei handelt es sich um allgemeine Einstellungen und Sie sollten die Vorgänge überwachen und die Werte entsprechend Ihrem Szenario optimieren.</span><span class="sxs-lookup"><span data-stu-id="8c5ef-789">These are general purpose settings, and you should monitor the operations and fine tune the values to suit your own scenario.</span></span>

| <span data-ttu-id="8c5ef-790">**Context**</span><span class="sxs-lookup"><span data-stu-id="8c5ef-790">**Context**</span></span> | <span data-ttu-id="8c5ef-791">**Beispiel-Ziel E2E<br />Maximale Wartezeit**</span><span class="sxs-lookup"><span data-stu-id="8c5ef-791">**Sample target E2E<br />max latency**</span></span> | <span data-ttu-id="8c5ef-792">**Wiederholungsrichtlinie**</span><span class="sxs-lookup"><span data-stu-id="8c5ef-792">**Retry policy**</span></span> | <span data-ttu-id="8c5ef-793">**Einstellungen**</span><span class="sxs-lookup"><span data-stu-id="8c5ef-793">**Settings**</span></span> | <span data-ttu-id="8c5ef-794">**Werte**</span><span class="sxs-lookup"><span data-stu-id="8c5ef-794">**Values**</span></span> | <span data-ttu-id="8c5ef-795">**So funktioniert's**</span><span class="sxs-lookup"><span data-stu-id="8c5ef-795">**How it works**</span></span> |
| --- | --- | --- | --- | --- | --- |
| <span data-ttu-id="8c5ef-796">Interaktiv, Benutzeroberfläche</span><span class="sxs-lookup"><span data-stu-id="8c5ef-796">Interactive, UI,</span></span><br /><span data-ttu-id="8c5ef-797">oder Vordergrund</span><span class="sxs-lookup"><span data-stu-id="8c5ef-797">or foreground</span></span> |<span data-ttu-id="8c5ef-798">2 Sekunden</span><span class="sxs-lookup"><span data-stu-id="8c5ef-798">2 seconds</span></span> |<span data-ttu-id="8c5ef-799">Linear</span><span class="sxs-lookup"><span data-stu-id="8c5ef-799">Linear</span></span> |<span data-ttu-id="8c5ef-800">maxAttempt</span><span class="sxs-lookup"><span data-stu-id="8c5ef-800">maxAttempt</span></span><br /><span data-ttu-id="8c5ef-801">deltaBackoff</span><span class="sxs-lookup"><span data-stu-id="8c5ef-801">deltaBackoff</span></span> |<span data-ttu-id="8c5ef-802">3</span><span class="sxs-lookup"><span data-stu-id="8c5ef-802">3</span></span><br /><span data-ttu-id="8c5ef-803">500 ms</span><span class="sxs-lookup"><span data-stu-id="8c5ef-803">500 ms</span></span> |<span data-ttu-id="8c5ef-804">Versuch 1 – Verzögerung 500 ms</span><span class="sxs-lookup"><span data-stu-id="8c5ef-804">Attempt 1 - delay 500 ms</span></span><br /><span data-ttu-id="8c5ef-805">Versuch 2 – Verzögerung 500 ms</span><span class="sxs-lookup"><span data-stu-id="8c5ef-805">Attempt 2 - delay 500 ms</span></span><br /><span data-ttu-id="8c5ef-806">Versuch 3 – Verzögerung 500 ms</span><span class="sxs-lookup"><span data-stu-id="8c5ef-806">Attempt 3 - delay 500 ms</span></span> |
| <span data-ttu-id="8c5ef-807">Hintergrund</span><span class="sxs-lookup"><span data-stu-id="8c5ef-807">Background</span></span><br /><span data-ttu-id="8c5ef-808">oder Batch</span><span class="sxs-lookup"><span data-stu-id="8c5ef-808">or batch</span></span> |<span data-ttu-id="8c5ef-809">30 Sekunden</span><span class="sxs-lookup"><span data-stu-id="8c5ef-809">30 seconds</span></span> |<span data-ttu-id="8c5ef-810">Exponentiell</span><span class="sxs-lookup"><span data-stu-id="8c5ef-810">Exponential</span></span> |<span data-ttu-id="8c5ef-811">maxAttempt</span><span class="sxs-lookup"><span data-stu-id="8c5ef-811">maxAttempt</span></span><br /><span data-ttu-id="8c5ef-812">deltaBackoff</span><span class="sxs-lookup"><span data-stu-id="8c5ef-812">deltaBackoff</span></span> |<span data-ttu-id="8c5ef-813">5</span><span class="sxs-lookup"><span data-stu-id="8c5ef-813">5</span></span><br /><span data-ttu-id="8c5ef-814">4 Sekunden</span><span class="sxs-lookup"><span data-stu-id="8c5ef-814">4 seconds</span></span> |<span data-ttu-id="8c5ef-815">Versuch 1 – Verzögerung ca. 3 Sek.</span><span class="sxs-lookup"><span data-stu-id="8c5ef-815">Attempt 1 - delay ~3 sec</span></span><br /><span data-ttu-id="8c5ef-816">Versuch 2 – Verzögerung ca. 7 Sek.</span><span class="sxs-lookup"><span data-stu-id="8c5ef-816">Attempt 2 - delay ~7 sec</span></span><br /><span data-ttu-id="8c5ef-817">Versuch 3 – Verzögerung ca. 15 Sek.</span><span class="sxs-lookup"><span data-stu-id="8c5ef-817">Attempt 3 - delay ~15 sec</span></span> |

### <a name="telemetry"></a><span data-ttu-id="8c5ef-818">Telemetrie</span><span class="sxs-lookup"><span data-stu-id="8c5ef-818">Telemetry</span></span>

<span data-ttu-id="8c5ef-819">Wiederholungsversuche werden in einer **TraceSource** protokolliert.</span><span class="sxs-lookup"><span data-stu-id="8c5ef-819">Retry attempts are logged to a **TraceSource**.</span></span> <span data-ttu-id="8c5ef-820">Sie müssen einen **TraceListener** konfigurieren, um die Ereignisse zu erfassen und diese in ein geeignetes Zielprotokoll zu schreiben.</span><span class="sxs-lookup"><span data-stu-id="8c5ef-820">You must configure a **TraceListener** to capture the events and write them to a suitable destination log.</span></span> <span data-ttu-id="8c5ef-821">Sie können **TextWriterTraceListener** oder **XmlWriterTraceListener** zum Schreiben der Daten in eine Protokolldatei, **EventLogTraceListener** zum Schreiben in ein Windows-Ereignisprotokoll oder **EventProviderTraceListener** zum Schreiben von Daten in das ETW-Subsystem verwenden.</span><span class="sxs-lookup"><span data-stu-id="8c5ef-821">You can use the **TextWriterTraceListener** or **XmlWriterTraceListener** to write the data to a log file, the **EventLogTraceListener** to write to the Windows Event Log, or the **EventProviderTraceListener** to write trace data to the ETW subsystem.</span></span> <span data-ttu-id="8c5ef-822">Sie können auch automatisches Leeren des Puffers und den Ausführlichkeitsgrad der Ereignisse konfigurieren, die protokolliert werden (z. B. Fehler, Warnung, Information und ausführlich).</span><span class="sxs-lookup"><span data-stu-id="8c5ef-822">You can also configure auto-flushing of the buffer, and the verbosity of events that will be logged (for example, Error, Warning, Informational, and Verbose).</span></span> <span data-ttu-id="8c5ef-823">Weitere Informationen finden Sie unter [Clientseitige Protokollierung mit der .NET Storage Client Library](/rest/api/storageservices/Client-side-Logging-with-the-.NET-Storage-Client-Library).</span><span class="sxs-lookup"><span data-stu-id="8c5ef-823">For more information, see [Client-side Logging with the .NET Storage Client Library](/rest/api/storageservices/Client-side-Logging-with-the-.NET-Storage-Client-Library).</span></span>

<span data-ttu-id="8c5ef-824">Vorgänge können eine **OperationContext**-Instanz erhalten, die ein **Retrying**-Ereignis verfügbar macht, das zum Anfügen von benutzerdefinierter Telemetrielogik verwendet werden kann.</span><span class="sxs-lookup"><span data-stu-id="8c5ef-824">Operations can receive an **OperationContext** instance, which exposes a **Retrying** event that can be used to attach custom telemetry logic.</span></span> <span data-ttu-id="8c5ef-825">Weitere Informationen finden Sie unter [OperationContext.Ereignis Erneuter Versuch](/dotnet/api/microsoft.windowsazure.storage.operationcontext.retrying).</span><span class="sxs-lookup"><span data-stu-id="8c5ef-825">For more information, see [OperationContext.Retrying Event](/dotnet/api/microsoft.windowsazure.storage.operationcontext.retrying).</span></span>

### <a name="examples"></a><span data-ttu-id="8c5ef-826">Beispiele</span><span class="sxs-lookup"><span data-stu-id="8c5ef-826">Examples</span></span>

<span data-ttu-id="8c5ef-827">Im folgenden Codebeispiel wird veranschaulicht, wie Sie zwei **TableRequestOptions** -Instanzen mit verschiedenen Wiederholungseinstellungen erstellen; eine für interaktive Anforderungen und eine für Anforderungen im Hintergrund.</span><span class="sxs-lookup"><span data-stu-id="8c5ef-827">The following code example shows how to create two **TableRequestOptions** instances with different retry settings; one for interactive requests and one for background requests.</span></span> <span data-ttu-id="8c5ef-828">Im Beispiel werden dann diese beiden Richtlinien auf dem Client festgelegt, sodass sie für alle Anforderungen gelten und außerdem wird die interaktive Strategie für eine bestimmte Anforderung festgelegt, damit sie die auf den Client angewandten Standardeinstellungen überschreibt.</span><span class="sxs-lookup"><span data-stu-id="8c5ef-828">The example then sets these two retry policies on the client so that they apply for all requests, and also sets the interactive strategy on a specific request so that it overrides the default settings applied to the client.</span></span>

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

### <a name="more-information"></a><span data-ttu-id="8c5ef-829">Weitere Informationen</span><span class="sxs-lookup"><span data-stu-id="8c5ef-829">More information</span></span>

- <span data-ttu-id="8c5ef-830">[Azure Storage Client Library Retry Policy Recommendations](https://azure.microsoft.com/blog/2014/05/22/azure-storage-client-library-retry-policy-recommendations/) (Azure Storage-Clientbibliothek: Empfehlungen für die Wiederholungsrichtlinie)</span><span class="sxs-lookup"><span data-stu-id="8c5ef-830">[Azure Storage client Library retry policy recommendations](https://azure.microsoft.com/blog/2014/05/22/azure-storage-client-library-retry-policy-recommendations/)</span></span>

- <span data-ttu-id="8c5ef-831">[Storage Client Library 2.0 – Implementing Retry Policies](https://gauravmantri.com/2012/12/30/storage-client-library-2-0-implementing-retry-policies/) (Storage Client Library 2.0: Implementieren von Wiederholungsrichtlinien)</span><span class="sxs-lookup"><span data-stu-id="8c5ef-831">[Storage Client Library 2.0 – Implementing retry policies](https://gauravmantri.com/2012/12/30/storage-client-library-2-0-implementing-retry-policies/)</span></span>

## <a name="general-rest-and-retry-guidelines"></a><span data-ttu-id="8c5ef-832">Allgemeine REST-und Wiederholungsrichtlinien</span><span class="sxs-lookup"><span data-stu-id="8c5ef-832">General REST and retry guidelines</span></span>

<span data-ttu-id="8c5ef-833">Berücksichtigen Sie beim Zugriff auf die Dienste von Azure oder von Drittanbietern Folgendes:</span><span class="sxs-lookup"><span data-stu-id="8c5ef-833">Consider the following when accessing Azure or third party services:</span></span>

- <span data-ttu-id="8c5ef-834">Verwenden Sie einen systematischen Ansatz zur Verwaltung von Wiederholungen, vielleicht als wiederverwendbaren Code, sodass Sie eine einheitliche Methodik für alle Clients und alle Lösungen anwenden können.</span><span class="sxs-lookup"><span data-stu-id="8c5ef-834">Use a systematic approach to managing retries, perhaps as reusable code, so that you can apply a consistent methodology across all clients and all solutions.</span></span>

- <span data-ttu-id="8c5ef-835">Verwenden Sie ggf. ein Wiederholungsframework (beispielsweise [Polly][polly]), um Wiederholungen zu verwalten, wenn der Zieldienst oder der Client über keinen integrierten Wiederholungsmechanismus verfügt.</span><span class="sxs-lookup"><span data-stu-id="8c5ef-835">Consider using a retry framework such as [Polly][polly] to manage retries if the target service or client has no built-in retry mechanism.</span></span> <span data-ttu-id="8c5ef-836">Dies hilft Ihnen bei der Implementierung eines konsistenten Wiederholungsverhaltens, und kann eine geeignete Standardwiederholungsstrategie für den Zieldienst bereitstellen.</span><span class="sxs-lookup"><span data-stu-id="8c5ef-836">This will help you implement a consistent retry behavior, and it may provide a suitable default retry strategy for the target service.</span></span> <span data-ttu-id="8c5ef-837">Sie müssen aber unter Umständen einen benutzerdefinierten Wiederholungscode für Dienste erstellen, die kein Standardverhalten aufweisen und nicht auf Ausnahmen vertrauen, um vorübergehende Fehler anzuzeigen, oder wenn Sie eine **Retry-Response**-Antwort verwenden möchten, um das Wiederholungsverhalten zu verwalten.</span><span class="sxs-lookup"><span data-stu-id="8c5ef-837">However, you may need to create custom retry code for services that have non-standard behavior, that do not rely on exceptions to indicate transient failures, or if you want to use a **Retry-Response** reply to manage retry behavior.</span></span>

- <span data-ttu-id="8c5ef-838">Die vorübergehende Erkennungslogik hängt von der tatsächlichen Client-API ab, die Sie verwenden, um die REST-Aufrufe aufzurufen.</span><span class="sxs-lookup"><span data-stu-id="8c5ef-838">The transient detection logic will depend on the actual client API you use to invoke the REST calls.</span></span> <span data-ttu-id="8c5ef-839">Einige Clients, z.B. die neuere **HttpClient**-Klasse, lösen keine Ausnahmen für abgeschlossene Anforderungen mit einem nicht erfolgreichen HTTP-Statuscode aus.</span><span class="sxs-lookup"><span data-stu-id="8c5ef-839">Some clients, such as the newer **HttpClient** class, will not throw exceptions for completed requests with a non-success HTTP status code.</span></span>

- <span data-ttu-id="8c5ef-840">Der vom Dienst zurückgegebene HTTP-Statuscode kann dabei helfen, zu erkennen, ob der Fehler vorübergehend ist.</span><span class="sxs-lookup"><span data-stu-id="8c5ef-840">The HTTP status code returned from the service can help to indicate whether the failure is transient.</span></span> <span data-ttu-id="8c5ef-841">Möglicherweise müssen Sie die Ausnahmen, die von einem Client oder dem Wiederholungsframework erzeugt wurden untersuchen, um auf den Statuscode zuzugreifen oder um den entsprechenden Ausnahmetyp bestimmen zu können.</span><span class="sxs-lookup"><span data-stu-id="8c5ef-841">You may need to examine the exceptions generated by a client or the retry framework to access the status code or to determine the equivalent exception type.</span></span> <span data-ttu-id="8c5ef-842">Die folgenden HTTP-Codes geben in der Regel an, dass eine Wiederholung geeignet ist:</span><span class="sxs-lookup"><span data-stu-id="8c5ef-842">The following HTTP codes typically indicate that a retry is appropriate:</span></span>
  
  - <span data-ttu-id="8c5ef-843">408 Anforderungstimeout</span><span class="sxs-lookup"><span data-stu-id="8c5ef-843">408 Request Timeout</span></span>
  - <span data-ttu-id="8c5ef-844">429 – Zu viele Anforderungen</span><span class="sxs-lookup"><span data-stu-id="8c5ef-844">429 Too Many Requests</span></span>
  - <span data-ttu-id="8c5ef-845">500 Interner Serverfehler</span><span class="sxs-lookup"><span data-stu-id="8c5ef-845">500 Internal Server Error</span></span>
  - <span data-ttu-id="8c5ef-846">502 Ungültiges Gateway</span><span class="sxs-lookup"><span data-stu-id="8c5ef-846">502 Bad Gateway</span></span>
  - <span data-ttu-id="8c5ef-847">503 Dienst nicht verfügbar</span><span class="sxs-lookup"><span data-stu-id="8c5ef-847">503 Service Unavailable</span></span>
  - <span data-ttu-id="8c5ef-848">504 Gateway-Timeout</span><span class="sxs-lookup"><span data-stu-id="8c5ef-848">504 Gateway Timeout</span></span>

- <span data-ttu-id="8c5ef-849">Wenn Sie Ihre Wiederholungslogik auf Ausnahmen basieren, wird in der Regel durch Folgendes angezeigt, dass ein vorübergehender Fehler besteht, da keine Verbindung hergestellt werden konnte:</span><span class="sxs-lookup"><span data-stu-id="8c5ef-849">If you base your retry logic on exceptions, the following typically indicate a transient failure where no connection could be established:</span></span>

  - <span data-ttu-id="8c5ef-850">WebExceptionStatus.ConnectionClosed</span><span class="sxs-lookup"><span data-stu-id="8c5ef-850">WebExceptionStatus.ConnectionClosed</span></span>
  - <span data-ttu-id="8c5ef-851">WebExceptionStatus.ConnectFailure</span><span class="sxs-lookup"><span data-stu-id="8c5ef-851">WebExceptionStatus.ConnectFailure</span></span>
  - <span data-ttu-id="8c5ef-852">WebExceptionStatus.Timeout</span><span class="sxs-lookup"><span data-stu-id="8c5ef-852">WebExceptionStatus.Timeout</span></span>
  - <span data-ttu-id="8c5ef-853">WebExceptionStatus.RequestCanceled</span><span class="sxs-lookup"><span data-stu-id="8c5ef-853">WebExceptionStatus.RequestCanceled</span></span>

- <span data-ttu-id="8c5ef-854">Im Fall des Status „Dienst nicht verfügbar“ weist der Dienst unter Umständen auf die entsprechende Verzögerung vor der Wiederholung im **Retry-After** -Antwort-Header oder in einem anderen benutzerdefinierten Header hin.</span><span class="sxs-lookup"><span data-stu-id="8c5ef-854">In the case of a service unavailable status, the service might indicate the appropriate delay before retrying in the **Retry-After** response header or a different custom header.</span></span> <span data-ttu-id="8c5ef-855">Dienste können auch zusätzliche Informationen als benutzerdefinierte Header oder in den Inhalt der Antwort eingebettet senden.</span><span class="sxs-lookup"><span data-stu-id="8c5ef-855">Services might also send additional information as custom headers, or embedded in the content of the response.</span></span>

- <span data-ttu-id="8c5ef-856">Führen Sie keine Wiederholung für Statuscodes durch, die Clientfehler (Fehler im Bereich 4xx) darstellen, mit Ausnahme eines Anforderungstimeouts 408.</span><span class="sxs-lookup"><span data-stu-id="8c5ef-856">Do not retry for status codes representing client errors (errors in the 4xx range) except for a 408 Request Timeout.</span></span>

- <span data-ttu-id="8c5ef-857">Testen Sie Ihre Wiederholungsstrategien und Mechanismen gründlich unter verschiedenen Bedingungen, wie z. B. verschiedenen Netzwerkzuständen und sich ändernden Systemlastverteilungen.</span><span class="sxs-lookup"><span data-stu-id="8c5ef-857">Thoroughly test your retry strategies and mechanisms under a range of conditions, such as different network states and varying system loadings.</span></span>

### <a name="retry-strategies"></a><span data-ttu-id="8c5ef-858">Wiederholungsstrategien</span><span class="sxs-lookup"><span data-stu-id="8c5ef-858">Retry strategies</span></span>

<span data-ttu-id="8c5ef-859">Im Folgenden sind die typischen Arten von Wiederholungsstrategie-Intervallen dargestellt:</span><span class="sxs-lookup"><span data-stu-id="8c5ef-859">The following are the typical types of retry strategy intervals:</span></span>

- <span data-ttu-id="8c5ef-860">**Exponential:**</span><span class="sxs-lookup"><span data-stu-id="8c5ef-860">**Exponential**.</span></span> <span data-ttu-id="8c5ef-861">Eine Wiederholungsrichtlinie, mit der eine angegebene Anzahl von Wiederholungsversuchen durchgeführt wird, bei der ein zufälliger exponentieller Backoff-Ansatz zur Ermittlung des Intervalls zwischen den Wiederholungsversuchen verwendet wird.</span><span class="sxs-lookup"><span data-stu-id="8c5ef-861">A retry policy that performs a specified number of retries, using a randomized exponential back off approach to determine the interval between retries.</span></span> <span data-ttu-id="8c5ef-862">Beispiel: </span><span class="sxs-lookup"><span data-stu-id="8c5ef-862">For example:</span></span>

    ```csharp
    var random = new Random();

    var delta = (int)((Math.Pow(2.0, currentRetryCount) - 1.0) *
                random.Next((int)(this.deltaBackoff.TotalMilliseconds * 0.8),
                (int)(this.deltaBackoff.TotalMilliseconds * 1.2)));
    var interval = (int)Math.Min(checked(this.minBackoff.TotalMilliseconds + delta),
                    this.maxBackoff.TotalMilliseconds);
    retryInterval = TimeSpan.FromMilliseconds(interval);
    ```

- <span data-ttu-id="8c5ef-863">**Incremental:**</span><span class="sxs-lookup"><span data-stu-id="8c5ef-863">**Incremental**.</span></span> <span data-ttu-id="8c5ef-864">Eine Wiederholungsstrategie mit einer angegebenen Anzahl von Wiederholungsversuchen und einem inkrementellen Zeitintervall zwischen den Wiederholungen.</span><span class="sxs-lookup"><span data-stu-id="8c5ef-864">A retry strategy with a specified number of retry attempts and an incremental time interval between retries.</span></span> <span data-ttu-id="8c5ef-865">Beispiel: </span><span class="sxs-lookup"><span data-stu-id="8c5ef-865">For example:</span></span>

    ```csharp
    retryInterval = TimeSpan.FromMilliseconds(this.initialInterval.TotalMilliseconds +
                    (this.increment.TotalMilliseconds * currentRetryCount));
    ```

- <span data-ttu-id="8c5ef-866">**LinearRetry:**</span><span class="sxs-lookup"><span data-stu-id="8c5ef-866">**LinearRetry**.</span></span> <span data-ttu-id="8c5ef-867">Eine Wiederholungsrichtlinie, bei der eine angegebene Anzahl von Wiederholungen durchgeführt und ein angegebenes festes Zeitintervall zwischen den Wiederholungen verwendet wird.</span><span class="sxs-lookup"><span data-stu-id="8c5ef-867">A retry policy that performs a specified number of retries, using a specified fixed time interval between retries.</span></span> <span data-ttu-id="8c5ef-868">Beispiel: </span><span class="sxs-lookup"><span data-stu-id="8c5ef-868">For example:</span></span>

    ```csharp
    retryInterval = this.deltaBackoff;
    ```

### <a name="transient-fault-handling-with-polly"></a><span data-ttu-id="8c5ef-869">Behandeln von vorübergehenden Fehlern mit Polly</span><span class="sxs-lookup"><span data-stu-id="8c5ef-869">Transient fault handling with Polly</span></span>

<span data-ttu-id="8c5ef-870">[Polly][polly] ist eine Bibliothek zur programmgesteuerten Behandlung von Wiederholungsversuchen und [Schutzschalter](../patterns/circuit-breaker.md)-Strategien.</span><span class="sxs-lookup"><span data-stu-id="8c5ef-870">[Polly][polly] is a library to programatically handle retries and [circuit breaker](../patterns/circuit-breaker.md) strategies.</span></span> <span data-ttu-id="8c5ef-871">Das Polly-Projekt ist ein Mitglied der [.NET Foundation][dotnet-foundation].</span><span class="sxs-lookup"><span data-stu-id="8c5ef-871">The Polly project is a member of the [.NET Foundation][dotnet-foundation].</span></span> <span data-ttu-id="8c5ef-872">Für Dienste, bei denen der Client Wiederholungsversuche nicht nativ unterstützt, ist Polly eine zulässige Alternative. Es ist hierbei nicht erforderlich, benutzerdefinierten Code für die Wiederholungsversuche zu schreiben, dessen richtige Implementierung schwierig sein kann.</span><span class="sxs-lookup"><span data-stu-id="8c5ef-872">For services where the client does not natively support retries, Polly is a valid alternative and avoids the need to write custom retry code, which can be hard to implement correctly.</span></span> <span data-ttu-id="8c5ef-873">Polly bietet auch eine Möglichkeit zum Überwachen des Auftretens von Fehlern, damit Sie Wiederholungsversuche protokollieren können.</span><span class="sxs-lookup"><span data-stu-id="8c5ef-873">Polly also provides a way to trace errors when they occur, so that you can log retries.</span></span>

### <a name="more-information"></a><span data-ttu-id="8c5ef-874">Weitere Informationen</span><span class="sxs-lookup"><span data-stu-id="8c5ef-874">More information</span></span>

- [<span data-ttu-id="8c5ef-875">Verbindungsstabilität</span><span class="sxs-lookup"><span data-stu-id="8c5ef-875">Connection resiliency</span></span>](/ef/core/miscellaneous/connection-resiliency)
- [<span data-ttu-id="8c5ef-876">Data Points – EF Core 1.1</span><span class="sxs-lookup"><span data-stu-id="8c5ef-876">Data Points - EF Core 1.1</span></span>](https://msdn.microsoft.com/magazine/mt745093.aspx)

<!-- links -->

[adal]: /azure/active-directory/develop/active-directory-authentication-libraries
[autorest]: https://github.com/Azure/autorest/tree/master/docs
[ConnectionPolicy.RetryOptions]: https://msdn.microsoft.com/library/azure/microsoft.azure.documents.client.connectionpolicy.retryoptions.aspx
[dotnet-foundation]: https://dotnetfoundation.org/
[polly]: http://www.thepollyproject.org
[redis-cache-troubleshoot]: /azure/redis-cache/cache-how-to-troubleshoot
[SearchIndexClient]: https://msdn.microsoft.com/library/azure/microsoft.azure.search.searchindexclient.aspx
[SearchServiceClient]: https://msdn.microsoft.com/library/microsoft.azure.search.searchserviceclient.aspx
