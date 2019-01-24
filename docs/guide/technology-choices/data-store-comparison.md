---
title: Kriterien für die Auswahl eines Datenspeichers
titleSuffix: Azure Application Architecture Guide
description: Übersicht über Azure-Computeoptionen.
author: MikeWasson
ms.date: 06/01/2018
ms.topic: guide
ms.service: architecture-center
ms.subservice: reference-architecture
ms.custom: seojan19
ms.openlocfilehash: f7c19501b9f28db3892285b5f35a33f02edd87ab
ms.sourcegitcommit: 1b50810208354577b00e89e5c031b774b02736e2
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/23/2019
ms.locfileid: "54488136"
---
# <a name="criteria-for-choosing-a-data-store"></a><span data-ttu-id="214e2-103">Kriterien für die Auswahl eines Datenspeichers</span><span class="sxs-lookup"><span data-stu-id="214e2-103">Criteria for choosing a data store</span></span>

<span data-ttu-id="214e2-104">Azure unterstützt zahlreiche Typen von Datenspeicherlösungen mit jeweils unterschiedlichen Features und Funktionen.</span><span class="sxs-lookup"><span data-stu-id="214e2-104">Azure supports many types of data storage solutions, each providing different features and capabilities.</span></span> <span data-ttu-id="214e2-105">In diesem Artikel werden die Vergleichskriterien beschrieben, die Sie bei der Bewertung eines Datenspeichers anwenden sollten.</span><span class="sxs-lookup"><span data-stu-id="214e2-105">This article describes the comparison criteria you should use when evaluating a data store.</span></span> <span data-ttu-id="214e2-106">Sie sollen Ihnen dabei helfen, zu bestimmen, welche Datenspeichertypen die Anforderungen Ihrer Lösung erfüllen können.</span><span class="sxs-lookup"><span data-stu-id="214e2-106">The goal is to help you determine which data storage types can meet your solution's requirements.</span></span>

## <a name="general-considerations"></a><span data-ttu-id="214e2-107">Allgemeine Überlegungen</span><span class="sxs-lookup"><span data-stu-id="214e2-107">General Considerations</span></span>

<span data-ttu-id="214e2-108">Sammeln Sie für den Vergleich zunächst möglichst viele der folgenden Informationen zu den Anforderungen für Ihre Daten.</span><span class="sxs-lookup"><span data-stu-id="214e2-108">To start your comparison, gather as much of the following information as you can about your data needs.</span></span> <span data-ttu-id="214e2-109">Anhand dieser Informationen können Sie bestimmen, welche Datenspeichertypen Ihren Anforderungen gerecht werden.</span><span class="sxs-lookup"><span data-stu-id="214e2-109">This information will help you to determine which data storage types will meet your needs.</span></span>

### <a name="functional-requirements"></a><span data-ttu-id="214e2-110">Funktionsanforderungen</span><span class="sxs-lookup"><span data-stu-id="214e2-110">Functional requirements</span></span>

- <span data-ttu-id="214e2-111">**Datenformat:**</span><span class="sxs-lookup"><span data-stu-id="214e2-111">**Data format**.</span></span> <span data-ttu-id="214e2-112">Welche Arten von Daten möchten Sie speichern?</span><span class="sxs-lookup"><span data-stu-id="214e2-112">What type of data are you intending to store?</span></span> <span data-ttu-id="214e2-113">Zu den allgemeinen Typen gehören Transaktionsdaten, JSON-Objekte, Telemetriedaten, Suchindizes oder Flatfiles.</span><span class="sxs-lookup"><span data-stu-id="214e2-113">Common types include transactional data, JSON objects, telemetry, search indexes, or flat files.</span></span>

- <span data-ttu-id="214e2-114">**Datengröße:**</span><span class="sxs-lookup"><span data-stu-id="214e2-114">**Data size**.</span></span> <span data-ttu-id="214e2-115">Wie umfangreich sind die Entitäten, die Sie speichern möchten?</span><span class="sxs-lookup"><span data-stu-id="214e2-115">How large are the entities you need to store?</span></span> <span data-ttu-id="214e2-116">Müssen diese Entitäten als einzelnes Dokument beibehalten werden, oder können sie auf mehrere Dokumente, Tabellen, Sammlungen usw. aufgeteilt werden?</span><span class="sxs-lookup"><span data-stu-id="214e2-116">Will these entities need to be maintained as a single document, or can they be split across multiple documents, tables, collections, and so forth?</span></span>

- <span data-ttu-id="214e2-117">**Skalierung und Struktur:**</span><span class="sxs-lookup"><span data-stu-id="214e2-117">**Scale and structure**.</span></span> <span data-ttu-id="214e2-118">Welche Gesamtmenge an Speicherkapazität benötigen Sie?</span><span class="sxs-lookup"><span data-stu-id="214e2-118">What is the overall amount of storage capacity you need?</span></span> <span data-ttu-id="214e2-119">Gehen Sie davon aus, dass Sie die Daten partitionieren?</span><span class="sxs-lookup"><span data-stu-id="214e2-119">Do you anticipate partitioning your data?</span></span>

- <span data-ttu-id="214e2-120">**Datenbeziehungen:**</span><span class="sxs-lookup"><span data-stu-id="214e2-120">**Data relationships**.</span></span> <span data-ttu-id="214e2-121">Müssen Ihre Daten 1:n- oder m:n-Beziehungen unterstützen?</span><span class="sxs-lookup"><span data-stu-id="214e2-121">Will your data need to support one-to-many or many-to-many relationships?</span></span> <span data-ttu-id="214e2-122">Sind die Beziehungen selbst ein wichtiger Teil der Daten?</span><span class="sxs-lookup"><span data-stu-id="214e2-122">Are relationships themselves an important part of the data?</span></span> <span data-ttu-id="214e2-123">Müssen Sie Daten innerhalb des gleichen DataSets oder aus externen DataSets verknüpfen oder auf andere Weise kombinieren?</span><span class="sxs-lookup"><span data-stu-id="214e2-123">Will you need to join or otherwise combine data from within the same dataset, or from external datasets?</span></span>

- <span data-ttu-id="214e2-124">**Konsistenzmodell:**</span><span class="sxs-lookup"><span data-stu-id="214e2-124">**Consistency model**.</span></span> <span data-ttu-id="214e2-125">Wie wichtig ist es, dass in einem Knoten durchgeführte Aktualisierungen in anderen Knoten angezeigt werden, damit weitere Änderungen vorgenommen werden können?</span><span class="sxs-lookup"><span data-stu-id="214e2-125">How important is it for updates made in one node to appear in other nodes, before further changes can be made?</span></span> <span data-ttu-id="214e2-126">Können Sie letztlich Konsistenz akzeptieren?</span><span class="sxs-lookup"><span data-stu-id="214e2-126">Can you accept eventual consistency?</span></span> <span data-ttu-id="214e2-127">Benötigen Sie ACID-Garantien für Transaktionen?</span><span class="sxs-lookup"><span data-stu-id="214e2-127">Do you need ACID guarantees for transactions?</span></span>

- <span data-ttu-id="214e2-128">**Schemaflexibilität:**</span><span class="sxs-lookup"><span data-stu-id="214e2-128">**Schema flexibility**.</span></span> <span data-ttu-id="214e2-129">Welche Art von Schemas wenden Sie auf Ihre Daten an?</span><span class="sxs-lookup"><span data-stu-id="214e2-129">What kind of schemas will you apply to your data?</span></span> <span data-ttu-id="214e2-130">Verwenden Sie ein festes Schema, ein Schema bei Schreibvorgängen oder ein Schema bei Lesevorgängen?</span><span class="sxs-lookup"><span data-stu-id="214e2-130">Will you use a fixed schema, a schema-on-write approach, or a schema-on-read approach?</span></span>

- <span data-ttu-id="214e2-131">**Parallelität:**</span><span class="sxs-lookup"><span data-stu-id="214e2-131">**Concurrency**.</span></span> <span data-ttu-id="214e2-132">Welche Art von Parallelitätsmechanismus möchten Sie beim Aktualisieren und Synchronisieren von Daten verwenden?</span><span class="sxs-lookup"><span data-stu-id="214e2-132">What kind of concurrency mechanism do you want to use when updating and synchronizing data?</span></span> <span data-ttu-id="214e2-133">Werden in der Anwendung viele Aktualisierungen durchgeführt, die potenziell zu Konflikten führen?</span><span class="sxs-lookup"><span data-stu-id="214e2-133">Will the application perform many updates that could potentially conflict.</span></span> <span data-ttu-id="214e2-134">Wenn dies der Fall ist, benötigen Sie unter Umständen die Datensatzsperrung und Steuerung für pessimistische Parallelität.</span><span class="sxs-lookup"><span data-stu-id="214e2-134">If so, you may require record locking and pessimistic concurrency control.</span></span> <span data-ttu-id="214e2-135">Können Sie alternativ die Steuerung für optimistische Parallelität unterstützen?</span><span class="sxs-lookup"><span data-stu-id="214e2-135">Alternatively, can you support optimistic concurrency controls?</span></span> <span data-ttu-id="214e2-136">Wenn ja, ist eine einfache zeitstempelbasierte Parallelitätssteuerung ausreichend. Oder benötigen Sie die erweiterte Funktion der Parallelitätssteuerung mit mehreren Versionen?</span><span class="sxs-lookup"><span data-stu-id="214e2-136">If so, is simple timestamp-based concurrency control enough, or do you need the added functionality of multi-version concurrency control?</span></span>

- <span data-ttu-id="214e2-137">**Datenverschiebung:**</span><span class="sxs-lookup"><span data-stu-id="214e2-137">**Data movement**.</span></span> <span data-ttu-id="214e2-138">Müssen in Ihrer Lösung ETL-Aufgaben durchgeführt werden, um Daten in andere Speicher oder Data Warehouses zu verschieben?</span><span class="sxs-lookup"><span data-stu-id="214e2-138">Will your solution need to perform ETL tasks to move data to other stores or data warehouses?</span></span>

- <span data-ttu-id="214e2-139">**Datenlebenszyklus:**</span><span class="sxs-lookup"><span data-stu-id="214e2-139">**Data lifecycle**.</span></span> <span data-ttu-id="214e2-140">Werden die Daten einmal geschrieben und häufig gelesen?</span><span class="sxs-lookup"><span data-stu-id="214e2-140">Is the data write-once, read-many?</span></span> <span data-ttu-id="214e2-141">Können sie in Cool oder Cold Storage verschoben werden?</span><span class="sxs-lookup"><span data-stu-id="214e2-141">Can it be moved into cool or cold storage?</span></span>

- <span data-ttu-id="214e2-142">**Andere unterstützte Features:**</span><span class="sxs-lookup"><span data-stu-id="214e2-142">**Other supported features**.</span></span> <span data-ttu-id="214e2-143">Benötigen Sie andere spezifische Features, z.B. Schemaüberprüfung, Aggregation, Indizierung, Volltextsuche, MapReduce oder andere Abfragefunktionen?</span><span class="sxs-lookup"><span data-stu-id="214e2-143">Do you need any other specific features, such as schema validation, aggregation, indexing, full-text search, MapReduce, or other query capabilities?</span></span>

### <a name="non-functional-requirements"></a><span data-ttu-id="214e2-144">Nicht funktionsbezogene Anforderungen</span><span class="sxs-lookup"><span data-stu-id="214e2-144">Non-functional requirements</span></span>

- <span data-ttu-id="214e2-145">**Leistung und Skalierbarkeit:**</span><span class="sxs-lookup"><span data-stu-id="214e2-145">**Performance and scalability**.</span></span> <span data-ttu-id="214e2-146">Wie lauten Ihre Anforderungen an die Datenleistung?</span><span class="sxs-lookup"><span data-stu-id="214e2-146">What are your data performance requirements?</span></span> <span data-ttu-id="214e2-147">Haben Sie spezielle Anforderungen an Datenerfassungs- und Datenverarbeitungsraten?</span><span class="sxs-lookup"><span data-stu-id="214e2-147">Do you have specific requirements for data ingestion rates and data processing rates?</span></span> <span data-ttu-id="214e2-148">Welche zulässigen Antwortzeiten für die Abfrage und Aggregation der Daten nach der Erfassung werden benötigt?</span><span class="sxs-lookup"><span data-stu-id="214e2-148">What are the acceptable response times for querying and aggregation of data once ingested?</span></span> <span data-ttu-id="214e2-149">Auf welche Größe muss der Datenspeicher zentral hochskaliert werden können?</span><span class="sxs-lookup"><span data-stu-id="214e2-149">How large will you need the data store to scale up?</span></span> <span data-ttu-id="214e2-150">Ist Ihre Workload eher leseintensiv oder schreibintensiv?</span><span class="sxs-lookup"><span data-stu-id="214e2-150">Is your workload more read-heavy or write-heavy?</span></span>

- <span data-ttu-id="214e2-151">**Zuverlässigkeit:**</span><span class="sxs-lookup"><span data-stu-id="214e2-151">**Reliability**.</span></span> <span data-ttu-id="214e2-152">Welche Gesamt-SLA muss unterstützt werden?</span><span class="sxs-lookup"><span data-stu-id="214e2-152">What overall SLA do you need to support?</span></span> <span data-ttu-id="214e2-153">Welche Fehlertoleranzebene müssen Sie für Datenconsumer bereitstellen?</span><span class="sxs-lookup"><span data-stu-id="214e2-153">What level of fault-tolerance do you need to provide for data consumers?</span></span> <span data-ttu-id="214e2-154">Welche Art von Sicherungs-und Wiederherstellungsfunktionen benötigen Sie?</span><span class="sxs-lookup"><span data-stu-id="214e2-154">What kind of backup and restore capabilities do you need?</span></span>

- <span data-ttu-id="214e2-155">**Replikation:**</span><span class="sxs-lookup"><span data-stu-id="214e2-155">**Replication**.</span></span> <span data-ttu-id="214e2-156">Müssen Ihre Daten auf mehrere Replikate oder Regionen verteilt werden?</span><span class="sxs-lookup"><span data-stu-id="214e2-156">Will your data need to be distributed among multiple replicas or regions?</span></span> <span data-ttu-id="214e2-157">Welche Art von Datenreplikationsfunktionen benötigen Sie?</span><span class="sxs-lookup"><span data-stu-id="214e2-157">What kind of data replication capabilities do you require?</span></span>

- <span data-ttu-id="214e2-158">**Einschränkungen:**</span><span class="sxs-lookup"><span data-stu-id="214e2-158">**Limits**.</span></span> <span data-ttu-id="214e2-159">Entsprechen die Einschränkungen eines bestimmten Datenspeichers Ihren Anforderungen an die Skalierung, die Anzahl der Verbindungen und den Durchsatz?</span><span class="sxs-lookup"><span data-stu-id="214e2-159">Will the limits of a particular data store support your requirements for scale, number of connections, and throughput?</span></span>

### <a name="management-and-cost"></a><span data-ttu-id="214e2-160">Verwaltung und Kosten</span><span class="sxs-lookup"><span data-stu-id="214e2-160">Management and cost</span></span>

- <span data-ttu-id="214e2-161">**Verwalteter Dienst:**</span><span class="sxs-lookup"><span data-stu-id="214e2-161">**Managed service**.</span></span> <span data-ttu-id="214e2-162">Verwenden Sie nach Möglichkeit einen verwalteten Datendienst, es sei denn, Sie benötigen bestimmte Funktionen, die nur in einem IaaS-gehosteten Datenspeicher enthalten sind.</span><span class="sxs-lookup"><span data-stu-id="214e2-162">When possible, use a managed data service, unless you require specific capabilities that can only be found in an IaaS-hosted data store.</span></span>

- <span data-ttu-id="214e2-163">**Regionale Verfügbarkeit:**</span><span class="sxs-lookup"><span data-stu-id="214e2-163">**Region availability**.</span></span> <span data-ttu-id="214e2-164">Ist der Dienst im Fall von verwalteten Diensten in allen Azure-Regionen verfügbar?</span><span class="sxs-lookup"><span data-stu-id="214e2-164">For managed services, is the service available in all Azure regions?</span></span> <span data-ttu-id="214e2-165">Muss Ihre Lösung in bestimmten Azure-Regionen gehostet werden?</span><span class="sxs-lookup"><span data-stu-id="214e2-165">Does your solution need to be hosted in certain Azure regions?</span></span>

- <span data-ttu-id="214e2-166">**Übertragbarkeit:**</span><span class="sxs-lookup"><span data-stu-id="214e2-166">**Portability**.</span></span> <span data-ttu-id="214e2-167">Müssen Ihre Daten zu lokalen oder externen Rechenzentren oder zu anderen Cloudhostingumgebungen migriert werden?</span><span class="sxs-lookup"><span data-stu-id="214e2-167">Will your data need to migrated to on-premises, external datacenters, or other cloud hosting environments?</span></span>

- <span data-ttu-id="214e2-168">**Lizenzierung:**</span><span class="sxs-lookup"><span data-stu-id="214e2-168">**Licensing**.</span></span> <span data-ttu-id="214e2-169">Bevorzugen Sie einen proprietären gegenüber einem OSS-Lizenztyp?</span><span class="sxs-lookup"><span data-stu-id="214e2-169">Do you have a preference of a proprietary versus OSS license type?</span></span> <span data-ttu-id="214e2-170">Gibt es andere externe Einschränkungen im Hinblick auf den zu verwendenden Lizenztyp?</span><span class="sxs-lookup"><span data-stu-id="214e2-170">Are there any other external restrictions on what type of license you can use?</span></span>

- <span data-ttu-id="214e2-171">**Gesamtkosten:**</span><span class="sxs-lookup"><span data-stu-id="214e2-171">**Overall cost**.</span></span> <span data-ttu-id="214e2-172">Wie hoch sind die Gesamtkosten für die Verwendung des Diensts in Ihrer Lösung?</span><span class="sxs-lookup"><span data-stu-id="214e2-172">What is the overall cost of using the service within your solution?</span></span> <span data-ttu-id="214e2-173">Wie viele Instanzen müssen ausgeführt werden, um Ihren Anforderungen an die Betriebszeit und den Durchsatz gerecht zu werden?</span><span class="sxs-lookup"><span data-stu-id="214e2-173">How many instances will need to run, to support your uptime and throughput requirements?</span></span> <span data-ttu-id="214e2-174">Berücksichtigen Sie bei dieser Berechnung auch die Betriebskosten.</span><span class="sxs-lookup"><span data-stu-id="214e2-174">Consider operations costs in this calculation.</span></span> <span data-ttu-id="214e2-175">Ein Grund für verwaltete Dienste sind die reduzierten Betriebskosten.</span><span class="sxs-lookup"><span data-stu-id="214e2-175">One reason to prefer managed services is the reduced operational cost.</span></span>

- <span data-ttu-id="214e2-176">**Kosteneffizienz:**</span><span class="sxs-lookup"><span data-stu-id="214e2-176">**Cost effectiveness**.</span></span> <span data-ttu-id="214e2-177">Können Sie Ihre Daten partitionieren, um sie kosteneffektiver zu speichern?</span><span class="sxs-lookup"><span data-stu-id="214e2-177">Can you partition your data, to store it more cost effectively?</span></span> <span data-ttu-id="214e2-178">Können beispielsweise umfangreiche Objekte aus einer kostenintensiven relationalen Datenbank in einen Objektspeicher verschoben werden?</span><span class="sxs-lookup"><span data-stu-id="214e2-178">For example, can you move large objects out of an expensive relational database into an object store?</span></span>

### <a name="security"></a><span data-ttu-id="214e2-179">Sicherheit</span><span class="sxs-lookup"><span data-stu-id="214e2-179">Security</span></span>

- <span data-ttu-id="214e2-180">**Sicherheit**.</span><span class="sxs-lookup"><span data-stu-id="214e2-180">**Security**.</span></span> <span data-ttu-id="214e2-181">Welche Art von Verschlüsselung benötigen Sie?</span><span class="sxs-lookup"><span data-stu-id="214e2-181">What type of encryption do you require?</span></span> <span data-ttu-id="214e2-182">Benötigen Sie die Verschlüsselung ruhender Daten?</span><span class="sxs-lookup"><span data-stu-id="214e2-182">Do you need encryption at rest?</span></span> <span data-ttu-id="214e2-183">Welche Authentifizierungsmethode möchten Sie zur Herstellung einer Verbindung mit den Daten verwenden?</span><span class="sxs-lookup"><span data-stu-id="214e2-183">What authentication mechanism do you want to use to connect to your data?</span></span>

- <span data-ttu-id="214e2-184">**Überwachung:**</span><span class="sxs-lookup"><span data-stu-id="214e2-184">**Auditing**.</span></span> <span data-ttu-id="214e2-185">Welche Art von Überwachungsprotokoll müssen Sie generieren?</span><span class="sxs-lookup"><span data-stu-id="214e2-185">What kind of audit log do you need to generate?</span></span>

- <span data-ttu-id="214e2-186">**Netzwerkanforderungen:**</span><span class="sxs-lookup"><span data-stu-id="214e2-186">**Networking requirements**.</span></span> <span data-ttu-id="214e2-187">Muss der Zugriff auf Ihre Daten über andere Netzwerkressourcen beschränkt oder auf andere Weise verwaltet werden?</span><span class="sxs-lookup"><span data-stu-id="214e2-187">Do you need to restrict or otherwise manage access to your data from other network resources?</span></span> <span data-ttu-id="214e2-188">Darf nur innerhalb der Azure-Umgebung auf die Daten zugegriffen werden?</span><span class="sxs-lookup"><span data-stu-id="214e2-188">Does data need to be accessible only from inside the Azure environment?</span></span> <span data-ttu-id="214e2-189">Müssen die Daten über bestimmte IP-Adressen oder Subnetze zugänglich sein?</span><span class="sxs-lookup"><span data-stu-id="214e2-189">Does the data need to be accessible from specific IP addresses or subnets?</span></span> <span data-ttu-id="214e2-190">Müssen sie über Anwendungen oder Dienste zugänglich sein, die lokal oder in anderen externen Rechenzentren gehostet werden?</span><span class="sxs-lookup"><span data-stu-id="214e2-190">Does it need to be accessible from applications or services hosted on-premises or in other external datacenters?</span></span>

### <a name="devops"></a><span data-ttu-id="214e2-191">DevOps</span><span class="sxs-lookup"><span data-stu-id="214e2-191">DevOps</span></span>

- <span data-ttu-id="214e2-192">**Fähigkeiten:**</span><span class="sxs-lookup"><span data-stu-id="214e2-192">**Skill set**.</span></span> <span data-ttu-id="214e2-193">Ist Ihr Team besonders vertraut mit der Verwendung bestimmter Programmiersprachen, Betriebssysteme oder anderen Technologien?</span><span class="sxs-lookup"><span data-stu-id="214e2-193">Are there particular programming languages, operating systems, or other technology that your team is particularly adept at using?</span></span> <span data-ttu-id="214e2-194">Stellen andere dagegen potenziell eine Schwierigkeit für das Team dar?</span><span class="sxs-lookup"><span data-stu-id="214e2-194">Are there others that would be difficult for your team to work with?</span></span>

- <span data-ttu-id="214e2-195">**Clients:** Besteht eine gute Clientunterstützung für Ihre Entwicklungssprachen?</span><span class="sxs-lookup"><span data-stu-id="214e2-195">**Clients** Is there good client support for your development languages?</span></span>

<span data-ttu-id="214e2-196">In den folgenden Abschnitten werden verschiedene Datenspeichermodelle in Bezug auf das Workloadprofil, die Datentypen und Beispiele für Anwendungsfälle verglichen.</span><span class="sxs-lookup"><span data-stu-id="214e2-196">The following sections compare various data store models in terms of workload profile, data types, and example use cases.</span></span>

## <a name="relational-database-management-systems-rdbms"></a><span data-ttu-id="214e2-197">Managementsysteme für relationale Datenbanken (RDBMS)</span><span class="sxs-lookup"><span data-stu-id="214e2-197">Relational database management systems (RDBMS)</span></span>

<!-- markdownlint-disable MD033 -->

<table>
<tr><td><span data-ttu-id="214e2-198"><strong>Workload</strong></span><span class="sxs-lookup"><span data-stu-id="214e2-198"><strong>Workload</strong></span></span></td>
    <td>
        <ul>
            <li><span data-ttu-id="214e2-199">Die Erstellung neuer Datensätze sowie die Aktualisierung vorhandener Daten werden regelmäßig durchgeführt.</span><span class="sxs-lookup"><span data-stu-id="214e2-199">Both the creation of new records and updates to existing data happen regularly.</span></span></li>
            <li><span data-ttu-id="214e2-200">Mehrere Vorgänge müssen in einer einzelnen Transaktion abgeschlossen werden.</span><span class="sxs-lookup"><span data-stu-id="214e2-200">Multiple operations have to be completed in a single transaction.</span></span></li>
            <li><span data-ttu-id="214e2-201">Aggregationsfunktionen sind für Kreuztabellen erforderlich.</span><span class="sxs-lookup"><span data-stu-id="214e2-201">Requires aggregation functions to perform cross-tabulation.</span></span></li>
            <li><span data-ttu-id="214e2-202">Starke Integration in Berichtstools ist erforderlich.</span><span class="sxs-lookup"><span data-stu-id="214e2-202">Strong integration with reporting tools is required.</span></span></li>
            <li><span data-ttu-id="214e2-203">Beziehungen werden mithilfe von Datenbankeinschränkungen erzwungen.</span><span class="sxs-lookup"><span data-stu-id="214e2-203">Relationships are enforced using database constraints.</span></span></li>
            <li><span data-ttu-id="214e2-204">Die Abfrageleistung wird mithilfe von Indizes optimiert.</span><span class="sxs-lookup"><span data-stu-id="214e2-204">Indexes are used to optimize query performance.</span></span></li>
            <li><span data-ttu-id="214e2-205">Ermöglicht den Zugriff auf bestimmte Teilmengen von Daten.</span><span class="sxs-lookup"><span data-stu-id="214e2-205">Allows access to specific subsets of data.</span></span></li>
        </ul>
    </td>
</tr>
<tr><td><span data-ttu-id="214e2-206"><strong>Datentyp</strong></span><span class="sxs-lookup"><span data-stu-id="214e2-206"><strong>Data type</strong></span></span></td>
    <td>
        <ul>
            <li><span data-ttu-id="214e2-207">Daten sind stark normalisiert.</span><span class="sxs-lookup"><span data-stu-id="214e2-207">Data is highly normalized.</span></span></li>
            <li><span data-ttu-id="214e2-208">Datenbankschemas sind erforderlich und werden erzwungen.</span><span class="sxs-lookup"><span data-stu-id="214e2-208">Database schemas are required and enforced.</span></span></li>
            <li><span data-ttu-id="214e2-209">m:n-Beziehungen zwischen Datenentitäten in der Datenbank.</span><span class="sxs-lookup"><span data-stu-id="214e2-209">Many-to-many relationships between data entities in the database.</span></span></li>
            <li><span data-ttu-id="214e2-210">Einschränkungen werden im Schema definiert und gelten für alle Daten in der Datenbank.</span><span class="sxs-lookup"><span data-stu-id="214e2-210">Constraints are defined in the schema and imposed on any data in the database.</span></span></li>
            <li><span data-ttu-id="214e2-211">Daten erfordern eine hohe Integrität.</span><span class="sxs-lookup"><span data-stu-id="214e2-211">Data requires high integrity.</span></span> <span data-ttu-id="214e2-212">Indizes und Beziehungen müssen genau verwaltet werden.</span><span class="sxs-lookup"><span data-stu-id="214e2-212">Indexes and relationships need to be maintained accurately.</span></span></li>
            <li><span data-ttu-id="214e2-213">Daten erfordern eine starke Konsistenz.</span><span class="sxs-lookup"><span data-stu-id="214e2-213">Data requires strong consistency.</span></span> <span data-ttu-id="214e2-214">Transaktionen werden so durchgeführt, dass sichergestellt wird, dass alle Daten für alle Benutzer und Prozesse 100 % konsistent sind.</span><span class="sxs-lookup"><span data-stu-id="214e2-214">Transactions operate in a way that ensures all data are 100% consistent for all users and processes.</span></span></li>
            <li><span data-ttu-id="214e2-215">Die Größe der einzelnen Dateneinträge soll klein bis mittelgroß sein.</span><span class="sxs-lookup"><span data-stu-id="214e2-215">Size of individual data entries is intended to be small to medium-sized.</span></span></li>
        </ul>
    </td>
</tr>
<tr><td><span data-ttu-id="214e2-216"><strong>Beispiele</strong></span><span class="sxs-lookup"><span data-stu-id="214e2-216"><strong>Examples</strong></span></span></td>
    <td>
        <ul>
            <li><span data-ttu-id="214e2-217">Branchenspezifisch (Personalverwaltung, Customer Relationship Management, Enterprise Resource Planning)</span><span class="sxs-lookup"><span data-stu-id="214e2-217">Line of business  (human capital management, customer relationship management, enterprise resource planning)</span></span></li>
            <li><span data-ttu-id="214e2-218">Bestandsverwaltung</span><span class="sxs-lookup"><span data-stu-id="214e2-218">Inventory management</span></span></li>
            <li><span data-ttu-id="214e2-219">Berichtsdatenbank</span><span class="sxs-lookup"><span data-stu-id="214e2-219">Reporting database</span></span></li>
            <li><span data-ttu-id="214e2-220">Buchhaltung</span><span class="sxs-lookup"><span data-stu-id="214e2-220">Accounting</span></span></li>
            <li><span data-ttu-id="214e2-221">Asset-Management</span><span class="sxs-lookup"><span data-stu-id="214e2-221">Asset management</span></span></li>
            <li><span data-ttu-id="214e2-222">Fondsmanagement</span><span class="sxs-lookup"><span data-stu-id="214e2-222">Fund management</span></span></li>
            <li><span data-ttu-id="214e2-223">Bestellungsverwaltung</span><span class="sxs-lookup"><span data-stu-id="214e2-223">Order management</span></span></li>
        </ul>
    </td>
</tr>
</table>

## <a name="document-databases"></a><span data-ttu-id="214e2-224">Dokumentdatenbanken</span><span class="sxs-lookup"><span data-stu-id="214e2-224">Document databases</span></span>

<table>
<tr><td><span data-ttu-id="214e2-225"><strong>Workload</strong></span><span class="sxs-lookup"><span data-stu-id="214e2-225"><strong>Workload</strong></span></span></td>
    <td>
        <ul>
            <li><span data-ttu-id="214e2-226">Allgemeiner Zweck.</span><span class="sxs-lookup"><span data-stu-id="214e2-226">General purpose.</span></span></li>
            <li><span data-ttu-id="214e2-227">Einfüge- und Aktualisierungsvorgänge werden häufig durchgeführt.</span><span class="sxs-lookup"><span data-stu-id="214e2-227">Insert and update operations are common.</span></span> <span data-ttu-id="214e2-228">Die Erstellung neuer Datensätze sowie die Aktualisierung vorhandener Daten werden regelmäßig durchgeführt.</span><span class="sxs-lookup"><span data-stu-id="214e2-228">Both the creation of new records and updates to existing data happen regularly.</span></span></li>
            <li><span data-ttu-id="214e2-229">Keine objektrelationalen Impedanzabweichungen.</span><span class="sxs-lookup"><span data-stu-id="214e2-229">No object-relational impedance mismatch.</span></span> <span data-ttu-id="214e2-230">Dokumente können besser mit den im Anwendungscode verwendeten Objektstrukturen abgeglichen werden.</span><span class="sxs-lookup"><span data-stu-id="214e2-230">Documents can better match the object structures used in application code.</span></span></li>
            <li><span data-ttu-id="214e2-231">Optimistische Parallelität wird häufiger verwendet.</span><span class="sxs-lookup"><span data-stu-id="214e2-231">Optimistic concurrency is more commonly used.</span></span></li>
            <li><span data-ttu-id="214e2-232">Daten müssen durch die verarbeitende Anwendung geändert und verarbeitet werden.</span><span class="sxs-lookup"><span data-stu-id="214e2-232">Data must be modified and processed by consuming application.</span></span></li>
            <li><span data-ttu-id="214e2-233">Für die Daten ist ein Index für mehrere Felder erforderlich.</span><span class="sxs-lookup"><span data-stu-id="214e2-233">Data requires index on multiple fields.</span></span></li>
            <li><span data-ttu-id="214e2-234">Einzelne Dokumente werden abgerufen und als einzelner Block geschrieben.</span><span class="sxs-lookup"><span data-stu-id="214e2-234">Individual documents are retrieved and written as a single block.</span></span></li>
    </td>
</tr>
<tr><td><span data-ttu-id="214e2-235"><strong>Datentyp</strong></span><span class="sxs-lookup"><span data-stu-id="214e2-235"><strong>Data type</strong></span></span></td>
    <td>
        <ul>
            <li><span data-ttu-id="214e2-236">Daten können auf denormalisierte Weise verwaltet werden.</span><span class="sxs-lookup"><span data-stu-id="214e2-236">Data can be managed in de-normalized way.</span></span></li>
            <li><span data-ttu-id="214e2-237">Die Größe der einzelnen Dokumentdaten ist relativ gering.</span><span class="sxs-lookup"><span data-stu-id="214e2-237">Size of individual document data is relatively small.</span></span></li>
            <li><span data-ttu-id="214e2-238">Für jeden Dokumenttyp kann ein eigenes Schema verwendet werden.</span><span class="sxs-lookup"><span data-stu-id="214e2-238">Each document type can use its own schema.</span></span></li>
            <li><span data-ttu-id="214e2-239">Dokumente können optionale Felder enthalten.</span><span class="sxs-lookup"><span data-stu-id="214e2-239">Documents can include optional fields.</span></span></li>
            <li><span data-ttu-id="214e2-240">Dokumentdaten sind teilweise strukturiert, d.h., die Datentypen der einzelnen Felder sind nicht streng definiert.</span><span class="sxs-lookup"><span data-stu-id="214e2-240">Document data is semi-structured, meaning that data types of each field are not strictly defined.</span></span></li>
            <li><span data-ttu-id="214e2-241">Die Datenaggregation wird unterstützt.</span><span class="sxs-lookup"><span data-stu-id="214e2-241">Data aggregation is supported.</span></span></li>
        </ul>
    </td>
</tr>
<tr><td><span data-ttu-id="214e2-242"><strong>Beispiele</strong></span><span class="sxs-lookup"><span data-stu-id="214e2-242"><strong>Examples</strong></span></span></td>
    <td>
        <ul>
            <li><span data-ttu-id="214e2-243">Produktkatalog</span><span class="sxs-lookup"><span data-stu-id="214e2-243">Product catalog</span></span></li>
            <li><span data-ttu-id="214e2-244">Benutzerkonten</span><span class="sxs-lookup"><span data-stu-id="214e2-244">User accounts</span></span></li>
            <li><span data-ttu-id="214e2-245">Stückliste</span><span class="sxs-lookup"><span data-stu-id="214e2-245">Bill of materials</span></span></li>
            <li><span data-ttu-id="214e2-246">Personalisierung</span><span class="sxs-lookup"><span data-stu-id="214e2-246">Personalization</span></span></li>
            <li><span data-ttu-id="214e2-247">Content Management</span><span class="sxs-lookup"><span data-stu-id="214e2-247">Content management</span></span></li>
            <li><span data-ttu-id="214e2-248">Betriebsdaten</span><span class="sxs-lookup"><span data-stu-id="214e2-248">Operations data</span></span></li>
            <li><span data-ttu-id="214e2-249">Bestandsverwaltung</span><span class="sxs-lookup"><span data-stu-id="214e2-249">Inventory management</span></span></li>
            <li><span data-ttu-id="214e2-250">Transaktionsverlaufsdaten</span><span class="sxs-lookup"><span data-stu-id="214e2-250">Transaction history data</span></span></li>
            <li><span data-ttu-id="214e2-251">Materialisierte Sicht anderer NoSQL-Datenspeicher.</span><span class="sxs-lookup"><span data-stu-id="214e2-251">Materialized view of other NoSQL stores.</span></span> <span data-ttu-id="214e2-252">Ersetzt Datei-/Blob-Indizierung.</span><span class="sxs-lookup"><span data-stu-id="214e2-252">Replaces file/BLOB indexing.</span></span></li>
        </ul>
    </td>
</tr>
</table>

## <a name="keyvalue-stores"></a><span data-ttu-id="214e2-253">Schlüssel-Wert-Speicher</span><span class="sxs-lookup"><span data-stu-id="214e2-253">Key/value stores</span></span>

<table>
<tr><td><span data-ttu-id="214e2-254"><strong>Workload</strong></span><span class="sxs-lookup"><span data-stu-id="214e2-254"><strong>Workload</strong></span></span></td>
    <td>
        <ul>
            <li><span data-ttu-id="214e2-255">Die Identifizierung und der Zugriff auf Daten erfolgen mithilfe eines einzigen ID-Schlüssels, z.B. über ein Wörterbuch.</span><span class="sxs-lookup"><span data-stu-id="214e2-255">Data is identified and accessed using a single ID key, like a dictionary.</span></span></li>
            <li><span data-ttu-id="214e2-256">Extrem skalierbar.</span><span class="sxs-lookup"><span data-stu-id="214e2-256">Massively scalable.</span></span></li>
            <li><span data-ttu-id="214e2-257">Keine Verknüpfungen, Sperren oder Unions sind erforderlich.</span><span class="sxs-lookup"><span data-stu-id="214e2-257">No joins, lock, or unions are required.</span></span></li>
            <li><span data-ttu-id="214e2-258">Keine Aggregationsmechanismen werden verwendet.</span><span class="sxs-lookup"><span data-stu-id="214e2-258">No aggregation mechanisms are used.</span></span></li>
            <li><span data-ttu-id="214e2-259">Sekundäre Indizes werden generell nicht verwendet.</span><span class="sxs-lookup"><span data-stu-id="214e2-259">Secondary indexes are generally not used.</span></span></li>
        </ul>
    </td>
</tr>
<tr><td><span data-ttu-id="214e2-260"><strong>Datentyp</strong></span><span class="sxs-lookup"><span data-stu-id="214e2-260"><strong>Data type</strong></span></span></td>
    <td>
        <ul>
            <li><span data-ttu-id="214e2-261">Die Datengröße ist tendenziell umfangreich.</span><span class="sxs-lookup"><span data-stu-id="214e2-261">Data size tends to be large.</span></span></li>
            <li><span data-ttu-id="214e2-262">Jeder Schlüssel ist einem einzelnen Wert zugeordnet, d.h. einem nicht verwalteten Datenblob.</span><span class="sxs-lookup"><span data-stu-id="214e2-262">Each key is associated with a single value, which is an unmanaged data BLOB.</span></span></li>
            <li><span data-ttu-id="214e2-263">Keine Schemas werden erzwungen.</span><span class="sxs-lookup"><span data-stu-id="214e2-263">There is no schema enforcement.</span></span></li>
            <li><span data-ttu-id="214e2-264">Keine Beziehungen zwischen Entitäten.</span><span class="sxs-lookup"><span data-stu-id="214e2-264">No relationships between entities.</span></span></li>
        </ul>
    </td>
</tr>
<tr><td><span data-ttu-id="214e2-265"><strong>Beispiele</strong></span><span class="sxs-lookup"><span data-stu-id="214e2-265"><strong>Examples</strong></span></span></td>
    <td>
        <ul>
            <li><span data-ttu-id="214e2-266">Datenzwischenspeicherung</span><span class="sxs-lookup"><span data-stu-id="214e2-266">Data caching</span></span></li>
            <li><span data-ttu-id="214e2-267">Sitzungsverwaltung</span><span class="sxs-lookup"><span data-stu-id="214e2-267">Session management</span></span></li>
            <li><span data-ttu-id="214e2-268">Benutzereinstellungs- und Benutzerprofilverwaltung</span><span class="sxs-lookup"><span data-stu-id="214e2-268">User preference and profile management</span></span></li>
            <li><span data-ttu-id="214e2-269">Produktempfehlungen und Ad-Serving</span><span class="sxs-lookup"><span data-stu-id="214e2-269">Product recommendation and ad serving</span></span></li>
            <li><span data-ttu-id="214e2-270">Wörterbücher</span><span class="sxs-lookup"><span data-stu-id="214e2-270">Dictionaries</span></span></li>
        </ul>
    </td>
</tr>
</table>

## <a name="graph-databases"></a><span data-ttu-id="214e2-271">Diagrammdatenbanken</span><span class="sxs-lookup"><span data-stu-id="214e2-271">Graph databases</span></span>

<table>
<tr><td><span data-ttu-id="214e2-272"><strong>Workload</strong></span><span class="sxs-lookup"><span data-stu-id="214e2-272"><strong>Workload</strong></span></span></td>
    <td>
        <ul>
            <li><span data-ttu-id="214e2-273">Die Beziehungen zwischen Datenelementen sind sehr komplex und erfordern viele Hops zwischen zugehörigen Datenelementen.</span><span class="sxs-lookup"><span data-stu-id="214e2-273">The relationships between data items are very complex, involving many hops between related data items.</span></span></li>
            <li><span data-ttu-id="214e2-274">Die Beziehungen zwischen Datenelementen sind dynamisch und ändern sich mit der Zeit.</span><span class="sxs-lookup"><span data-stu-id="214e2-274">The relationship between data items are dynamic and change over time.</span></span></li>
            <li><span data-ttu-id="214e2-275">Beziehungen zwischen Objekten sind bevorzugte Beziehungen, bei denen keine Fremdschlüssel und Verknüpfungen durchlaufen werden müssen.</span><span class="sxs-lookup"><span data-stu-id="214e2-275">Relationships between objects are first-class citizens, without requiring foreign-keys and joins to traverse.</span></span></li>
        </ul>
    </td>
</tr>
<tr><td><span data-ttu-id="214e2-276"><strong>Datentyp</strong></span><span class="sxs-lookup"><span data-stu-id="214e2-276"><strong>Data type</strong></span></span></td>
    <td>
        <ul>
            <li><span data-ttu-id="214e2-277">Daten bestehen aus Knoten und Beziehungen.</span><span class="sxs-lookup"><span data-stu-id="214e2-277">Data is comprised of nodes and relationships.</span></span></li>
            <li><span data-ttu-id="214e2-278">Knoten ähneln Tabellenzeilen oder JSON-Dokumenten.</span><span class="sxs-lookup"><span data-stu-id="214e2-278">Nodes are similar to table rows or JSON documents.</span></span></li>
            <li><span data-ttu-id="214e2-279">Beziehungen sind genau so wichtig wie Knoten und werden direkt in der Abfragesprache verfügbar gemacht.</span><span class="sxs-lookup"><span data-stu-id="214e2-279">Relationships are just as important as nodes, and are exposed directly in the query language.</span></span></li>
            <li><span data-ttu-id="214e2-280">Zusammengesetzte Objekte, z.B. eine Person mit mehreren Telefonnummern, werden meist in separate kleinere Knoten unterteilt und mit durchlaufbaren Beziehungen kombiniert.</span><span class="sxs-lookup"><span data-stu-id="214e2-280">Composite objects, such as a person with multiple phone numbers, tend to be broken into separate, smaller nodes, combined with traversable relationships</span></span> </li>
        </ul>
    </td>
</tr>
<tr><td><span data-ttu-id="214e2-281"><strong>Beispiele</strong></span><span class="sxs-lookup"><span data-stu-id="214e2-281"><strong>Examples</strong></span></span></td>
    <td>
        <ul>
            <li><span data-ttu-id="214e2-282">Organigramme</span><span class="sxs-lookup"><span data-stu-id="214e2-282">Organization charts</span></span></li>
            <li><span data-ttu-id="214e2-283">Social Graphs</span><span class="sxs-lookup"><span data-stu-id="214e2-283">Social graphs</span></span></li>
            <li><span data-ttu-id="214e2-284">Betrugserkennung</span><span class="sxs-lookup"><span data-stu-id="214e2-284">Fraud detection</span></span></li>
            <li><span data-ttu-id="214e2-285">Analytics</span><span class="sxs-lookup"><span data-stu-id="214e2-285">Analytics</span></span></li>
            <li><span data-ttu-id="214e2-286">Empfehlungs-Engines</span><span class="sxs-lookup"><span data-stu-id="214e2-286">Recommendation engines</span></span></li>
        </ul>
    </td>
</tr>
</table>

## <a name="column-family-databases"></a><span data-ttu-id="214e2-287">Spaltenfamilien-Datenbanken</span><span class="sxs-lookup"><span data-stu-id="214e2-287">Column-family databases</span></span>

<table>
<tr><td><span data-ttu-id="214e2-288"><strong>Workload</strong></span><span class="sxs-lookup"><span data-stu-id="214e2-288"><strong>Workload</strong></span></span></td>
    <td>
        <ul>
            <li><span data-ttu-id="214e2-289">In den meisten Column-Family-Datenbanken werden Schreibvorgänge extrem schnell durchgeführt.</span><span class="sxs-lookup"><span data-stu-id="214e2-289">Most column-family databases perform write operations extremely quickly.</span></span></li>
            <li><span data-ttu-id="214e2-290">Aktualisierungs- und Löschvorgänge sind selten.</span><span class="sxs-lookup"><span data-stu-id="214e2-290">Update and delete operations are rare.</span></span></li>
            <li><span data-ttu-id="214e2-291">Bietet Zugriff mit hohem Durchsatz und niedriger Latenz.</span><span class="sxs-lookup"><span data-stu-id="214e2-291">Designed to provide high throughput and low-latency access.</span></span></li>
            <li><span data-ttu-id="214e2-292">Unterstützt den einfachen Abfragezugriff auf eine bestimmte Gruppe von Feldern in einem wesentlich größeren Datensatz.</span><span class="sxs-lookup"><span data-stu-id="214e2-292">Supports easy query access to a particular set of fields within a much larger record.</span></span></li>
            <li><span data-ttu-id="214e2-293">Extrem skalierbar.</span><span class="sxs-lookup"><span data-stu-id="214e2-293">Massively scalable.</span></span></li>
        </ul>
    </td>
</tr>
<tr><td><span data-ttu-id="214e2-294"><strong>Datentyp</strong></span><span class="sxs-lookup"><span data-stu-id="214e2-294"><strong>Data type</strong></span></span></td>
    <td>
        <ul>
            <li><span data-ttu-id="214e2-295">Daten werden in Tabellen gespeichert, die aus einer Schlüsselspalte und einer oder mehreren Spaltenfamilien bestehen.</span><span class="sxs-lookup"><span data-stu-id="214e2-295">Data is stored in tables consisting of a key column and one or more column families.</span></span></li>
            <li><span data-ttu-id="214e2-296">Bestimmte Spalten können nach einzelnen Zeilen variieren.</span><span class="sxs-lookup"><span data-stu-id="214e2-296">Specific columns can vary by individual rows.</span></span></li>
            <li><span data-ttu-id="214e2-297">Der Zugriff auf einzelne Zellen erfolgt über get- und put-Befehle.</span><span class="sxs-lookup"><span data-stu-id="214e2-297">Individual cells are accessed via get and put commands</span></span></li>
            <li><span data-ttu-id="214e2-298">Mehrere Zeilen werden unter Verwendung eines scan-Befehls zurückgegeben.</span><span class="sxs-lookup"><span data-stu-id="214e2-298">Multiple rows are returned using a scan command.</span></span></li>
        </ul>
    </td>
</tr>
<tr><td><span data-ttu-id="214e2-299"><strong>Beispiele</strong></span><span class="sxs-lookup"><span data-stu-id="214e2-299"><strong>Examples</strong></span></span></td>
    <td>
        <ul>
            <li><span data-ttu-id="214e2-300">Empfehlungen</span><span class="sxs-lookup"><span data-stu-id="214e2-300">Recommendations</span></span></li>
            <li><span data-ttu-id="214e2-301">Personalisierung</span><span class="sxs-lookup"><span data-stu-id="214e2-301">Personalization</span></span></li>
            <li><span data-ttu-id="214e2-302">Sensordaten</span><span class="sxs-lookup"><span data-stu-id="214e2-302">Sensor data</span></span></li>
            <li><span data-ttu-id="214e2-303">Telemetrie</span><span class="sxs-lookup"><span data-stu-id="214e2-303">Telemetry</span></span></li>
            <li><span data-ttu-id="214e2-304">Nachrichten</span><span class="sxs-lookup"><span data-stu-id="214e2-304">Messaging</span></span></li>
            <li><span data-ttu-id="214e2-305">Analysen sozialer Medien</span><span class="sxs-lookup"><span data-stu-id="214e2-305">Social media analytics</span></span></li>
            <li><span data-ttu-id="214e2-306">Webanalysen</span><span class="sxs-lookup"><span data-stu-id="214e2-306">Web analytics</span></span></li>
            <li><span data-ttu-id="214e2-307">Aktivitätsüberwachung</span><span class="sxs-lookup"><span data-stu-id="214e2-307">Activity monitoring</span></span></li>
            <li><span data-ttu-id="214e2-308">Wetter- und andere Zeitreihendaten</span><span class="sxs-lookup"><span data-stu-id="214e2-308">Weather and other time-series data</span></span></li>
        </ul>
    </td>
</tr>
</table>

## <a name="search-engine-databases"></a><span data-ttu-id="214e2-309">Suchmaschinen-Datenbanken</span><span class="sxs-lookup"><span data-stu-id="214e2-309">Search engine databases</span></span>

<table>
<tr><td><span data-ttu-id="214e2-310"><strong>Workload</strong></span><span class="sxs-lookup"><span data-stu-id="214e2-310"><strong>Workload</strong></span></span></td>
    <td>
        <ul>
            <li><span data-ttu-id="214e2-311">Indizierung von Daten aus mehreren Quellen und Diensten.</span><span class="sxs-lookup"><span data-stu-id="214e2-311">Indexing data from multiple sources and services.</span></span></li>
            <li><span data-ttu-id="214e2-312">Abfragen werden ad hoc durchgeführt und können komplex sein.</span><span class="sxs-lookup"><span data-stu-id="214e2-312">Queries are ad-hoc and can be complex.</span></span></li>
            <li><span data-ttu-id="214e2-313">Aggregation ist erforderlich.</span><span class="sxs-lookup"><span data-stu-id="214e2-313">Requires aggregation.</span></span></li>
            <li><span data-ttu-id="214e2-314">Volltextsuche ist erforderlich.</span><span class="sxs-lookup"><span data-stu-id="214e2-314">Full text search is required.</span></span></li>
            <li><span data-ttu-id="214e2-315">Ad-hoc-Self-Service-Abfragen sind erforderlich.</span><span class="sxs-lookup"><span data-stu-id="214e2-315">Ad hoc self-service query is required.</span></span></li>
            <li><span data-ttu-id="214e2-316">Datenanalyse mit Index für alle Felder ist erforderlich.</span><span class="sxs-lookup"><span data-stu-id="214e2-316">Data analysis with index on all fields is required.</span></span></li>
        </ul>
    </td>
</tr>
<tr><td><span data-ttu-id="214e2-317"><strong>Datentyp</strong></span><span class="sxs-lookup"><span data-stu-id="214e2-317"><strong>Data type</strong></span></span></td>
    <td>
        <ul>
            <li><span data-ttu-id="214e2-318">Teilweise strukturiert oder unstrukturiert</span><span class="sxs-lookup"><span data-stu-id="214e2-318">Semi-structured or unstructured</span></span></li>
            <li><span data-ttu-id="214e2-319">Text</span><span class="sxs-lookup"><span data-stu-id="214e2-319">Text</span></span></li>
            <li><span data-ttu-id="214e2-320">Text mit Verweis auf strukturierte Daten</span><span class="sxs-lookup"><span data-stu-id="214e2-320">Text with reference to structured data</span></span></li>
        </ul>
    </td>
</tr>
<tr><td><span data-ttu-id="214e2-321"><strong>Beispiele</strong></span><span class="sxs-lookup"><span data-stu-id="214e2-321"><strong>Examples</strong></span></span></td>
    <td>
        <ul>
            <li><span data-ttu-id="214e2-322">Produktkataloge</span><span class="sxs-lookup"><span data-stu-id="214e2-322">Product catalogs</span></span></li>
            <li><span data-ttu-id="214e2-323">Websitesuche</span><span class="sxs-lookup"><span data-stu-id="214e2-323">Site search</span></span></li>
            <li><span data-ttu-id="214e2-324">Protokollierung</span><span class="sxs-lookup"><span data-stu-id="214e2-324">Logging</span></span></li>
            <li><span data-ttu-id="214e2-325">Analytics</span><span class="sxs-lookup"><span data-stu-id="214e2-325">Analytics</span></span></li>
            <li><span data-ttu-id="214e2-326">Shopping-Websites</span><span class="sxs-lookup"><span data-stu-id="214e2-326">Shopping sites</span></span></li>
        </ul>
    </td>
</tr>
</table>

## <a name="data-warehouse"></a><span data-ttu-id="214e2-327">Data Warehouse</span><span class="sxs-lookup"><span data-stu-id="214e2-327">Data warehouse</span></span>

<table>
<tr><td><span data-ttu-id="214e2-328"><strong>Workload</strong></span><span class="sxs-lookup"><span data-stu-id="214e2-328"><strong>Workload</strong></span></span></td>
    <td>
        <ul>
            <li><span data-ttu-id="214e2-329">Datenanalysen</span><span class="sxs-lookup"><span data-stu-id="214e2-329">Data analytics</span></span></li>
            <li><span data-ttu-id="214e2-330">Enterprise BI</span><span class="sxs-lookup"><span data-stu-id="214e2-330">Enterprise BI</span></span></li>
        </ul>
    </td>
</tr>
<tr><td><span data-ttu-id="214e2-331"><strong>Datentyp</strong></span><span class="sxs-lookup"><span data-stu-id="214e2-331"><strong>Data type</strong></span></span></td>
    <td>
        <ul>
            <li><span data-ttu-id="214e2-332">Verlaufsdaten aus mehreren Quellen.</span><span class="sxs-lookup"><span data-stu-id="214e2-332">Historical data from multiple sources.</span></span></li>
            <li><span data-ttu-id="214e2-333">In der Regel denormalisiert in einem Schema vom Typ &quot;Stern&quot; oder &quot;Schneeflocke&quot;, das aus Fakten- und Dimensionstabellen besteht.</span><span class="sxs-lookup"><span data-stu-id="214e2-333">Usually denormalized in a &quot;star&quot; or &quot;snowflake&quot; schema, consisting of fact and dimension tables.</span></span></li>
            <li><span data-ttu-id="214e2-334">Wird normalerweise mit neuen Daten auf Basis eines Zeitplans geladen.</span><span class="sxs-lookup"><span data-stu-id="214e2-334">Usually loaded with new data on a scheduled basis.</span></span></li>
            <li><span data-ttu-id="214e2-335">Dimensionstabellen enthalten häufig mehrere Verlaufsversionen einer Entität, die als <em>langsam veränderliche Dimension</em> bezeichnet wird.</span><span class="sxs-lookup"><span data-stu-id="214e2-335">Dimension tables often include multiple historic versions of an entity, referred to as a <em>slowly changing dimension</em>.</span></span></li>
        </ul>
    </td>
</tr>
<tr><td><span data-ttu-id="214e2-336"><strong>Beispiele</strong></span><span class="sxs-lookup"><span data-stu-id="214e2-336"><strong>Examples</strong></span></span></td>
    <td><span data-ttu-id="214e2-337">Enterprise Data Warehouse, das Daten für analytische Modelle, Berichte und Dashboards umfasst</span><span class="sxs-lookup"><span data-stu-id="214e2-337">An enterprise data warehouse that provides data for analytical models, reports, and dashboards.</span></span>
    </td>
</tr>
</table>

## <a name="time-series-databases"></a><span data-ttu-id="214e2-338">Zeitreihendatenbanken</span><span class="sxs-lookup"><span data-stu-id="214e2-338">Time series databases</span></span>

<table>
<tr><td><span data-ttu-id="214e2-339"><strong>Workload</strong></span><span class="sxs-lookup"><span data-stu-id="214e2-339"><strong>Workload</strong></span></span></td>
    <td>
        <ul>
            <li><span data-ttu-id="214e2-340">Der überwiegende Anteil der Vorgänge sind Schreibvorgänge (95 bis 99%).</span><span class="sxs-lookup"><span data-stu-id="214e2-340">An overwhelming proportion of operations (95-99%) are writes.</span></span></li>
            <li><span data-ttu-id="214e2-341">Datensätze werden generell sequenziell in zeitlicher Reihenfolge angefügt.</span><span class="sxs-lookup"><span data-stu-id="214e2-341">Records are generally appended sequentially in time order.</span></span></li>
            <li><span data-ttu-id="214e2-342">Aktualisierungen sind selten.</span><span class="sxs-lookup"><span data-stu-id="214e2-342">Updates are rare.</span></span></li>
            <li><span data-ttu-id="214e2-343">Löschvorgänge werden massenweise und in zusammenhängenden Blöcken oder Datensätzen durchgeführt.</span><span class="sxs-lookup"><span data-stu-id="214e2-343">Deletes occur in bulk, and are made to contiguous blocks or records.</span></span></li>
            <li><span data-ttu-id="214e2-344">Leseanforderungen können größer als der verfügbare Arbeitsspeicher sein.</span><span class="sxs-lookup"><span data-stu-id="214e2-344">Read requests can be larger than available memory.</span></span></li>
            <li><span data-ttu-id="214e2-345">Üblicherweise erfolgen mehrere Lesevorgänge gleichzeitig.</span><span class="sxs-lookup"><span data-stu-id="214e2-345">It&#39;s common for multiple reads to occur simultaneously.</span></span></li>
            <li><span data-ttu-id="214e2-346">Daten werden sequenziell in aufsteigender oder absteigender zeitlicher Reihenfolge gelesen.</span><span class="sxs-lookup"><span data-stu-id="214e2-346">Data is read sequentially in either ascending or descending time order.</span></span></li>
        </ul>
    </td>
</tr>
<tr><td><span data-ttu-id="214e2-347"><strong>Datentyp</strong></span><span class="sxs-lookup"><span data-stu-id="214e2-347"><strong>Data type</strong></span></span></td>
    <td>
        <ul>
            <li><span data-ttu-id="214e2-348">Ein Zeitstempel wird als primärer Schlüssel und Sortiermechanismus verwendet.</span><span class="sxs-lookup"><span data-stu-id="214e2-348">A time stamp that is used as the primary key and sorting mechanism.</span></span></li>
            <li><span data-ttu-id="214e2-349">Messungen ab dem Eintrag oder Beschreibungen des Eintrags.</span><span class="sxs-lookup"><span data-stu-id="214e2-349">Measurements from the entry or descriptions of what the entry represents.</span></span></li>
            <li><span data-ttu-id="214e2-350">Mit Tags werden zusätzliche Informationen zum Typ oder Ursprung sowie andere Informationen zum Eintrag definiert.</span><span class="sxs-lookup"><span data-stu-id="214e2-350">Tags that define additional information about the type, origin, and other information about the entry.</span></span></li>
        </ul>
    </td>
</tr>
<tr><td><span data-ttu-id="214e2-351"><strong>Beispiele</strong></span><span class="sxs-lookup"><span data-stu-id="214e2-351"><strong>Examples</strong></span></span></td>
    <td>
        <ul>
            <li><span data-ttu-id="214e2-352">Überwachungs- und Ereignistelemetrie</span><span class="sxs-lookup"><span data-stu-id="214e2-352">Monitoring and event telemetry.</span></span></li>
            <li><span data-ttu-id="214e2-353">Sensor- oder andere IoT-Daten</span><span class="sxs-lookup"><span data-stu-id="214e2-353">Sensor or other IoT data.</span></span></li>
        </ul>
    </td>
</tr>
</table>

## <a name="object-storage"></a><span data-ttu-id="214e2-354">Objektspeicher</span><span class="sxs-lookup"><span data-stu-id="214e2-354">Object storage</span></span>

<table>
<tr><td><span data-ttu-id="214e2-355"><strong>Workload</strong></span><span class="sxs-lookup"><span data-stu-id="214e2-355"><strong>Workload</strong></span></span></td>
    <td>
        <ul>
            <li><span data-ttu-id="214e2-356">Identifizierung erfolgt nach Schlüssel.</span><span class="sxs-lookup"><span data-stu-id="214e2-356">Identified by key.</span></span></li>
            <li><span data-ttu-id="214e2-357">Auf Objekte kann öffentlich oder privat zugegriffen werden.</span><span class="sxs-lookup"><span data-stu-id="214e2-357">Objects may be publicly or privately accessible.</span></span></li>
            <li><span data-ttu-id="214e2-358">Inhalte sind normalerweise Ressourcen, z.B. eine Tabelle, ein Bild oder eine Videodatei.</span><span class="sxs-lookup"><span data-stu-id="214e2-358">Content is typically an asset such as a spreadsheet, image, or video file.</span></span></li>
            <li><span data-ttu-id="214e2-359">Inhalte müssen dauerhaft (persistent) sein und sich außerhalb von Anwendungsebenen oder virtuellen Computern befinden.</span><span class="sxs-lookup"><span data-stu-id="214e2-359">Content must be durable (persistent), and external to any application tier or virtual machine.</span></span></li>
        </ul>
    </td>
</tr>
<tr><td><span data-ttu-id="214e2-360"><strong>Datentyp</strong></span><span class="sxs-lookup"><span data-stu-id="214e2-360"><strong>Data type</strong></span></span></td>
    <td>
        <ul>
            <li><span data-ttu-id="214e2-361">Die Datengröße ist umfangreich.</span><span class="sxs-lookup"><span data-stu-id="214e2-361">Data size is large.</span></span></li>
            <li><span data-ttu-id="214e2-362">Blobdaten</span><span class="sxs-lookup"><span data-stu-id="214e2-362">Blob data.</span></span></li>
            <li><span data-ttu-id="214e2-363">Wert ist nicht transparent.</span><span class="sxs-lookup"><span data-stu-id="214e2-363">Value is opaque.</span></span></li>
        </ul>
    </td>
</tr>
<tr><td><span data-ttu-id="214e2-364"><strong>Beispiele</strong></span><span class="sxs-lookup"><span data-stu-id="214e2-364"><strong>Examples</strong></span></span></td>
    <td>
        <ul>
            <li><span data-ttu-id="214e2-365">Bilder, Videos, Office-Dokumente, PDF-Dateien</span><span class="sxs-lookup"><span data-stu-id="214e2-365">Images, videos, office documents, PDFs</span></span></li>
            <li><span data-ttu-id="214e2-366">CSS, Skripts, CSV</span><span class="sxs-lookup"><span data-stu-id="214e2-366">CSS, Scripts, CSV</span></span></li>
            <li><span data-ttu-id="214e2-367">Statisches HTML, JSON</span><span class="sxs-lookup"><span data-stu-id="214e2-367">Static HTML, JSON</span></span></li>
            <li><span data-ttu-id="214e2-368">Protokoll- und Überwachungsdateien</span><span class="sxs-lookup"><span data-stu-id="214e2-368">Log and audit files</span></span></li>
            <li><span data-ttu-id="214e2-369">Datenbanksicherungen</span><span class="sxs-lookup"><span data-stu-id="214e2-369">Database backups</span></span></li>
        </ul>
    </td>
</tr>
</table>

## <a name="shared-files"></a><span data-ttu-id="214e2-370">Freigegebene Dateien</span><span class="sxs-lookup"><span data-stu-id="214e2-370">Shared files</span></span>

<table>
<tr><td><span data-ttu-id="214e2-371"><strong>Workload</strong></span><span class="sxs-lookup"><span data-stu-id="214e2-371"><strong>Workload</strong></span></span></td>
    <td>
        <ul>
            <li><span data-ttu-id="214e2-372">Migration aus vorhandenen Apps, die mit dem Dateisystem interagieren.</span><span class="sxs-lookup"><span data-stu-id="214e2-372">Migration from existing apps that interact with the file system.</span></span></li>
            <li><span data-ttu-id="214e2-373">Erfordert eine SMB-Schnittstelle.</span><span class="sxs-lookup"><span data-stu-id="214e2-373">Requires SMB interface.</span></span></li>
        </ul>
    </td>
</tr>
<tr><td><span data-ttu-id="214e2-374"><strong>Datentyp</strong></span><span class="sxs-lookup"><span data-stu-id="214e2-374"><strong>Data type</strong></span></span></td>
    <td>
        <ul>
            <li><span data-ttu-id="214e2-375">Dateien in einer hierarchischen Gruppe von Ordnern.</span><span class="sxs-lookup"><span data-stu-id="214e2-375">Files in a hierarchical set of folders.</span></span></li>
            <li><span data-ttu-id="214e2-376">Zugänglich über E/A-Standardbibliotheken.</span><span class="sxs-lookup"><span data-stu-id="214e2-376">Accessible with standard I/O libraries.</span></span></li>
        </ul>
    </td>
</tr>
<tr><td><span data-ttu-id="214e2-377"><strong>Beispiele</strong></span><span class="sxs-lookup"><span data-stu-id="214e2-377"><strong>Examples</strong></span></span></td>
    <td>
        <ul>
            <li><span data-ttu-id="214e2-378">Legacydateien</span><span class="sxs-lookup"><span data-stu-id="214e2-378">Legacy files</span></span></li>
            <li><span data-ttu-id="214e2-379">Freigegebener Inhalt zugänglich innerhalb verschiedener virtueller Computer oder App-Instanzen</span><span class="sxs-lookup"><span data-stu-id="214e2-379">Shared content accessible among a number of VMs or app instances</span></span></li>
        </ul>
    </td>
</tr>
</table>

<!-- markdownlint-enable MD033 -->
