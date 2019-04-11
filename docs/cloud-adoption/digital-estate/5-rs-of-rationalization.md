---
title: 'Framework für die Cloudeinführung (Cloud Adoption Framework, CAF): Die fünf R der Rationalisierung'
titleSuffix: Microsoft Cloud Adoption Framework for Azure
ms.service: architecture-center
ms.subservice: enterprise-cloud-adoption
ms.custom: governance
description: Beschreibt die Optionen, bei der Rationalisierung eines digitales Umfelds zur Verfügung stehen.
author: BrianBlanchard
ms.date: 12/10/2018
ms.topic: guide
ms.openlocfilehash: 25987e790df5c82ad2c8296f1be1dabc949f1629
ms.sourcegitcommit: c053e6edb429299a0ad9b327888d596c48859d4a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/20/2019
ms.locfileid: "58243301"
---
# <a name="the-5-rs-of-rationalization"></a><span data-ttu-id="ed836-103">Die fünf R der Rationalisierung</span><span class="sxs-lookup"><span data-stu-id="ed836-103">The 5 Rs of rationalization</span></span>

<span data-ttu-id="ed836-104">Cloudrationalisierung bezeichnet die Untersuchung von Ressourcen, um die bestmögliche Migrations- oder Modernisierungsmethode für die jeweilige Ressource in der Cloud zu ermitteln.</span><span class="sxs-lookup"><span data-stu-id="ed836-104">Cloud Rationalization is the process of evaluating assets to determine the best approach to migrating or modernizing each asset in the cloud.</span></span> <span data-ttu-id="ed836-105">Weitere Informationen zum Rationalisierungsprozess finden Sie unter [Worum handelt es sich bei digitalen Ressourcen?](overview.md).</span><span class="sxs-lookup"><span data-stu-id="ed836-105">For more information about the process of rationalization, see [What is a digital estate?](overview.md)</span></span>

<span data-ttu-id="ed836-106">Die hier aufgeführten fünf R der Rationalisierung beschreiben die gängigsten Optionen für die Rationalisierung.</span><span class="sxs-lookup"><span data-stu-id="ed836-106">The "5 Rs of rationalization" listed here describe the most common options for rationalization.</span></span>

## <a name="rehost"></a><span data-ttu-id="ed836-107">Rehosten</span><span class="sxs-lookup"><span data-stu-id="ed836-107">Rehost</span></span>

<span data-ttu-id="ed836-108">Auch „Lift & Shift“ genannt. Beim Rehosten wird die Ressource im aktuellen Zustand zum gewählten Cloudanbieter migriert. Die Architektur bleibt dabei größtenteils unverändert.</span><span class="sxs-lookup"><span data-stu-id="ed836-108">Also known as "lift and shift," a rehost effort moves the current state asset to the chosen cloud provider, with minimal change to overall architecture.</span></span>

<span data-ttu-id="ed836-109">Mögliche Gründe:</span><span class="sxs-lookup"><span data-stu-id="ed836-109">Common drivers could include:</span></span>

* <span data-ttu-id="ed836-110">Senkung der Investitionskosten</span><span class="sxs-lookup"><span data-stu-id="ed836-110">Reduce CapEx</span></span>
* <span data-ttu-id="ed836-111">Freigabe von Platz im Datencenter</span><span class="sxs-lookup"><span data-stu-id="ed836-111">Free up datacenter space</span></span>
* <span data-ttu-id="ed836-112">Schnelle Amortisierung der Cloud</span><span class="sxs-lookup"><span data-stu-id="ed836-112">Quick cloud ROI</span></span>

<span data-ttu-id="ed836-113">Faktoren für die quantitative Analyse:</span><span class="sxs-lookup"><span data-stu-id="ed836-113">Quantitative analysis factors:</span></span>

* <span data-ttu-id="ed836-114">VM-Größe (CPU, Arbeitsspeicher, Speicherplatz)</span><span class="sxs-lookup"><span data-stu-id="ed836-114">VM size (CPU, memory, storage)</span></span>
* <span data-ttu-id="ed836-115">Abhängigkeiten (Netzwerkdatenverkehr)</span><span class="sxs-lookup"><span data-stu-id="ed836-115">Dependencies (network traffic)</span></span>
* <span data-ttu-id="ed836-116">Ressourcenkompatibilität</span><span class="sxs-lookup"><span data-stu-id="ed836-116">Asset compatibility</span></span>

<span data-ttu-id="ed836-117">Faktoren für die qualitative Analyse:</span><span class="sxs-lookup"><span data-stu-id="ed836-117">Qualitative analysis factors:</span></span>

* <span data-ttu-id="ed836-118">Änderungstoleranz</span><span class="sxs-lookup"><span data-stu-id="ed836-118">Tolerance for change</span></span>
* <span data-ttu-id="ed836-119">Geschäftliche Prioritäten</span><span class="sxs-lookup"><span data-stu-id="ed836-119">Business priorities</span></span>
* <span data-ttu-id="ed836-120">Kritische Unternehmensereignisse</span><span class="sxs-lookup"><span data-stu-id="ed836-120">Critical business events</span></span>
* <span data-ttu-id="ed836-121">Prozessabhängigkeiten</span><span class="sxs-lookup"><span data-stu-id="ed836-121">Process dependencies</span></span>

## <a name="refactor"></a><span data-ttu-id="ed836-122">Refactoring</span><span class="sxs-lookup"><span data-stu-id="ed836-122">Refactor</span></span>

<span data-ttu-id="ed836-123">PaaS-Optionen (Platform-as-a-Service) können zur Senkung der Betriebskosten zahlreicher Anwendungen beitragen.</span><span class="sxs-lookup"><span data-stu-id="ed836-123">Platform as a service (PaaS) options can reduce operational costs associated with many applications.</span></span> <span data-ttu-id="ed836-124">Unter Umständen empfiehlt es sich, eine Anwendung geringfügig umzugestalten, um sie an ein PaaS-basiertes Modell anzupassen.</span><span class="sxs-lookup"><span data-stu-id="ed836-124">It can be prudent to slightly refactor an application to fit a PaaS based model.</span></span>

<span data-ttu-id="ed836-125">„Umgestalten“ bezieht sich auch auf den Anwendungsentwicklungsprozess der Codeumgestaltung, um eine Anwendung für neue Geschäftschancen fit zu machen.</span><span class="sxs-lookup"><span data-stu-id="ed836-125">Refactor also refers to the application development process of refactoring code to allow an application to deliver on new business opportunities.</span></span>

<span data-ttu-id="ed836-126">Mögliche Gründe:</span><span class="sxs-lookup"><span data-stu-id="ed836-126">Common drivers could include:</span></span>

* <span data-ttu-id="ed836-127">Schnellere und kürzere Updates</span><span class="sxs-lookup"><span data-stu-id="ed836-127">Faster, shorter, updates</span></span>
* <span data-ttu-id="ed836-128">Codeportabilität</span><span class="sxs-lookup"><span data-stu-id="ed836-128">Code portability</span></span>
* <span data-ttu-id="ed836-129">Höhere Cloudeffizienz (Ressourcen, Geschwindigkeit, Kosten)</span><span class="sxs-lookup"><span data-stu-id="ed836-129">Greater cloud efficiency (resources, speed, cost)</span></span>

<span data-ttu-id="ed836-130">Faktoren für die quantitative Analyse:</span><span class="sxs-lookup"><span data-stu-id="ed836-130">Quantitative analysis factors:</span></span>

* <span data-ttu-id="ed836-131">Größe der Anwendungsressource (CPU, Arbeitsspeicher, Speicherplatz)</span><span class="sxs-lookup"><span data-stu-id="ed836-131">Application asset size (CPU, memory, storage)</span></span>
* <span data-ttu-id="ed836-132">Abhängigkeiten (Netzwerkdatenverkehr)</span><span class="sxs-lookup"><span data-stu-id="ed836-132">Dependencies (network traffic)</span></span>
* <span data-ttu-id="ed836-133">Benutzerdatenverkehr (Seitenaufrufe, Verweildauer auf der Seite, Ladezeit)</span><span class="sxs-lookup"><span data-stu-id="ed836-133">User traffic (page views, time on page, load time)</span></span>
* <span data-ttu-id="ed836-134">Entwicklungsplattform (Sprachen, Datenplattform, Dienste der mittleren Ebene)</span><span class="sxs-lookup"><span data-stu-id="ed836-134">Development platform (languages, data platform, middle tier services)</span></span>

<span data-ttu-id="ed836-135">Faktoren für die qualitative Analyse:</span><span class="sxs-lookup"><span data-stu-id="ed836-135">Qualitative analysis factors:</span></span>

* <span data-ttu-id="ed836-136">Laufende Unternehmensinvestitionen</span><span class="sxs-lookup"><span data-stu-id="ed836-136">Continued business investments</span></span>
* <span data-ttu-id="ed836-137">Burstoptionen/-zeitpläne</span><span class="sxs-lookup"><span data-stu-id="ed836-137">Bursting options/timelines</span></span>
* <span data-ttu-id="ed836-138">Geschäftsprozessabhängigkeiten</span><span class="sxs-lookup"><span data-stu-id="ed836-138">Business process dependencies</span></span>

## <a name="rearchitect"></a><span data-ttu-id="ed836-139">Rearchitect (Überarbeiten)</span><span class="sxs-lookup"><span data-stu-id="ed836-139">Rearchitect</span></span>

<span data-ttu-id="ed836-140">Einige ältere Anwendungen sind aufgrund von architekturbezogenen Entscheidungen, die bei der Entwicklung der Anwendung getroffen wurden, nicht mit Cloudanbietern kompatibel.</span><span class="sxs-lookup"><span data-stu-id="ed836-140">Some aging applications aren't compatible with cloud providers because of the architectural decisions made when the application was built.</span></span> <span data-ttu-id="ed836-141">In diesem Fall muss die Anwendung vor der Transformation ggf. überarbeitet werden.</span><span class="sxs-lookup"><span data-stu-id="ed836-141">In these cases, the application may need to be rearchitected before transformation.</span></span>

<span data-ttu-id="ed836-142">Anwendungen, die zwar mit der Cloud kompatibel, aber keine nativen Cloudanwendungen sind, können unter Umständen kostengünstiger und effizienter betrieben werden, wenn die Lösung überarbeitet und in eine native Cloudanwendung umgewandelt wird.</span><span class="sxs-lookup"><span data-stu-id="ed836-142">In other cases, applications that are cloud compatible, but not cloud native benefits, may produce cost efficiencies and operational efficiencies by rearchitecting the solution to be a cloud native application.</span></span>

<span data-ttu-id="ed836-143">Mögliche Gründe:</span><span class="sxs-lookup"><span data-stu-id="ed836-143">Common drivers could include:</span></span>

* <span data-ttu-id="ed836-144">Skalierbarkeit und Flexibilität der Anwendung</span><span class="sxs-lookup"><span data-stu-id="ed836-144">Application scale and agility</span></span>
* <span data-ttu-id="ed836-145">Einfachere Einführung neuer Cloudfunktionen</span><span class="sxs-lookup"><span data-stu-id="ed836-145">Easier adoption of new cloud capabilities</span></span>
* <span data-ttu-id="ed836-146">Verwendung verschiedener Technologiestapel</span><span class="sxs-lookup"><span data-stu-id="ed836-146">Mix of technology stacks</span></span>

<span data-ttu-id="ed836-147">Faktoren für die quantitative Analyse:</span><span class="sxs-lookup"><span data-stu-id="ed836-147">Quantitative analysis factors:</span></span>

* <span data-ttu-id="ed836-148">Größe der Anwendungsressource (CPU, Arbeitsspeicher, Speicherplatz)</span><span class="sxs-lookup"><span data-stu-id="ed836-148">Application asset size (CPU, memory, storage)</span></span>
* <span data-ttu-id="ed836-149">Abhängigkeiten (Netzwerkdatenverkehr)</span><span class="sxs-lookup"><span data-stu-id="ed836-149">Dependencies (network traffic)</span></span>
* <span data-ttu-id="ed836-150">Benutzerdatenverkehr (Seitenaufrufe, Verweildauer auf der Seite, Ladezeit)</span><span class="sxs-lookup"><span data-stu-id="ed836-150">User traffic (page views, time on page, load time)</span></span>
* <span data-ttu-id="ed836-151">Entwicklungsplattform (Sprachen, Datenplattform, Dienste der mittleren Ebene)</span><span class="sxs-lookup"><span data-stu-id="ed836-151">Development platform (languages, data platform, middle tier services)</span></span>

<span data-ttu-id="ed836-152">Faktoren für die qualitative Analyse:</span><span class="sxs-lookup"><span data-stu-id="ed836-152">Qualitative analysis factors:</span></span>

* <span data-ttu-id="ed836-153">Steigende Unternehmensinvestitionen</span><span class="sxs-lookup"><span data-stu-id="ed836-153">Growing business investments</span></span>
* <span data-ttu-id="ed836-154">Betriebskosten</span><span class="sxs-lookup"><span data-stu-id="ed836-154">Operational costs</span></span>
* <span data-ttu-id="ed836-155">Potenzielle Feedbackschleifen und DevOps-Investitionen</span><span class="sxs-lookup"><span data-stu-id="ed836-155">Potential feedback loops and DevOps investments</span></span>

## <a name="rebuild"></a><span data-ttu-id="ed836-156">Neu erstellen</span><span class="sxs-lookup"><span data-stu-id="ed836-156">Rebuild</span></span>

<span data-ttu-id="ed836-157">In manchen Szenarien ist die Kluft, die für eine Weiterverwendung einer Anwendung überwunden werden muss, zu groß, um weitere Investitionen zu rechtfertigen.</span><span class="sxs-lookup"><span data-stu-id="ed836-157">In some scenarios, the delta that must be overcome to carry forward an application can be too large to justify further investment.</span></span> <span data-ttu-id="ed836-158">Das gilt insbesondere für Anwendungen, die inzwischen nicht mehr unterstützt werden und nicht mehr die Anforderungen der aktuellen Geschäftsprozesse erfüllen.</span><span class="sxs-lookup"><span data-stu-id="ed836-158">This is especially true for applications that used to meet the needs of the business, but are now unsupported or misaligned with how the business processes are executed today.</span></span> <span data-ttu-id="ed836-159">In diesem Fall wird eine neue Codebasis erstellt, die auf einen [cloudnativen](https://azure.microsoft.com/overview/cloudnative/) Ansatz ausgerichtet ist.</span><span class="sxs-lookup"><span data-stu-id="ed836-159">In this case, a new code base is created to align with a [cloud native](https://azure.microsoft.com/overview/cloudnative/) approach.</span></span>

<span data-ttu-id="ed836-160">Mögliche Gründe:</span><span class="sxs-lookup"><span data-stu-id="ed836-160">Common drivers could include:</span></span>

* <span data-ttu-id="ed836-161">Beschleunigung von Innovationen</span><span class="sxs-lookup"><span data-stu-id="ed836-161">Accelerate innovation</span></span>
* <span data-ttu-id="ed836-162">Schnellere App-Erstellung</span><span class="sxs-lookup"><span data-stu-id="ed836-162">Build apps faster</span></span>
* <span data-ttu-id="ed836-163">Senkung der Betriebskosten</span><span class="sxs-lookup"><span data-stu-id="ed836-163">Reduce operational cost</span></span>

<span data-ttu-id="ed836-164">Faktoren für die quantitative Analyse:</span><span class="sxs-lookup"><span data-stu-id="ed836-164">Quantitative analysis factors:</span></span>

* <span data-ttu-id="ed836-165">Größe der Anwendungsressource (CPU, Arbeitsspeicher, Speicherplatz)</span><span class="sxs-lookup"><span data-stu-id="ed836-165">Application asset size (CPU, memory, storage)</span></span>
* <span data-ttu-id="ed836-166">Abhängigkeiten (Netzwerkdatenverkehr)</span><span class="sxs-lookup"><span data-stu-id="ed836-166">Dependencies (network traffic)</span></span>
* <span data-ttu-id="ed836-167">Benutzerdatenverkehr (Seitenaufrufe, Verweildauer auf der Seite, Ladezeit)</span><span class="sxs-lookup"><span data-stu-id="ed836-167">User traffic (page views, time on page, load time)</span></span>
* <span data-ttu-id="ed836-168">Entwicklungsplattform (Sprachen, Datenplattform, Dienste der mittleren Ebene)</span><span class="sxs-lookup"><span data-stu-id="ed836-168">Development platform (languages, data platform, middle tier services)</span></span>

<span data-ttu-id="ed836-169">Faktoren für die qualitative Analyse:</span><span class="sxs-lookup"><span data-stu-id="ed836-169">Qualitative analysis Factors:</span></span>

* <span data-ttu-id="ed836-170">Sinkende Endbenutzerzufriedenheit</span><span class="sxs-lookup"><span data-stu-id="ed836-170">Declining end user satisfaction</span></span>
* <span data-ttu-id="ed836-171">Einschränkung von Geschäftsprozessen aufgrund des Funktionsumfangs</span><span class="sxs-lookup"><span data-stu-id="ed836-171">Business processes limited by functionality</span></span>
* <span data-ttu-id="ed836-172">Potenzielle Verbesserungen bei Kosten, Erfahrung oder Umsatz</span><span class="sxs-lookup"><span data-stu-id="ed836-172">Potential cost, experience, or revenue gains</span></span>

## <a name="replace"></a><span data-ttu-id="ed836-173">Replace</span><span class="sxs-lookup"><span data-stu-id="ed836-173">Replace</span></span>

<span data-ttu-id="ed836-174">Lösungen werden in der Regel mit der besten Technologie und Methode implementiert, die zum Zeitpunkt der Implementierung zur Verfügung steht.</span><span class="sxs-lookup"><span data-stu-id="ed836-174">Solutions are generally implemented using the best technology and approach available at the time.</span></span> <span data-ttu-id="ed836-175">Manchmal erfüllen SaaS-Anwendungen (Software-as-a-Service) sämtliche Funktionsanforderungen der gehosteten Anwendung.</span><span class="sxs-lookup"><span data-stu-id="ed836-175">In some cases, software as a service (SaaS) applications can meet all of the functionality required of the hosted application.</span></span> <span data-ttu-id="ed836-176">In diesem Fall kann eine Workload zukünftig ersetzt werden, wodurch sie bei der Transformation praktisch nicht weiter berücksichtigt werden muss.</span><span class="sxs-lookup"><span data-stu-id="ed836-176">In these scenarios, a workload could be slated for future replacement, effectively removing it from the transformation effort.</span></span>

<span data-ttu-id="ed836-177">Mögliche Gründe:</span><span class="sxs-lookup"><span data-stu-id="ed836-177">Common drivers could include:</span></span>

* <span data-ttu-id="ed836-178">Standardisierung auf der Grundlage bewährter Branchenmethoden</span><span class="sxs-lookup"><span data-stu-id="ed836-178">Standardize around industry-best practices</span></span>
* <span data-ttu-id="ed836-179">Beschleunigung der Einführung geschäftsprozessbasierter Ansätze</span><span class="sxs-lookup"><span data-stu-id="ed836-179">Accelerate adoption of business process driven approaches</span></span>
* <span data-ttu-id="ed836-180">Umverteilung von Entwicklungsinvestitionen für Anwendungen, die Alleinstellungsmerkmale oder Wettbewerbsvorteile generieren</span><span class="sxs-lookup"><span data-stu-id="ed836-180">Reallocate development investments into applications that create competitive differentiation or advantages</span></span>

<span data-ttu-id="ed836-181">Faktoren für die quantitative Analyse:</span><span class="sxs-lookup"><span data-stu-id="ed836-181">Quantitative analysis factors:</span></span>

* <span data-ttu-id="ed836-182">Senkung der allgemeinen Betriebskosten</span><span class="sxs-lookup"><span data-stu-id="ed836-182">General operating cost reductions</span></span>
* <span data-ttu-id="ed836-183">VM-Größe (CPU, Arbeitsspeicher, Speicherplatz)</span><span class="sxs-lookup"><span data-stu-id="ed836-183">VM size (CPU, memory, storage)</span></span>
* <span data-ttu-id="ed836-184">Abhängigkeiten (Netzwerkdatenverkehr)</span><span class="sxs-lookup"><span data-stu-id="ed836-184">Dependencies (network traffic)</span></span>
* <span data-ttu-id="ed836-185">Auszumusternde Ressourcen</span><span class="sxs-lookup"><span data-stu-id="ed836-185">Assets to be retired</span></span>

<span data-ttu-id="ed836-186">Faktoren für die qualitative Analyse:</span><span class="sxs-lookup"><span data-stu-id="ed836-186">Qualitative analysis factors:</span></span>

* <span data-ttu-id="ed836-187">Kosten-Nutzen-Analyse der aktuellen Architektur im Vergleich zu einer SaaS-Lösung</span><span class="sxs-lookup"><span data-stu-id="ed836-187">Cost benefit analysis of the current architecture versus a SaaS solution</span></span>
* <span data-ttu-id="ed836-188">Geschäftsprozesszuordnungen</span><span class="sxs-lookup"><span data-stu-id="ed836-188">Business process maps</span></span>
* <span data-ttu-id="ed836-189">Datenschemas</span><span class="sxs-lookup"><span data-stu-id="ed836-189">Data schemas</span></span>
* <span data-ttu-id="ed836-190">Benutzerdefinierte oder automatisierte Prozesse</span><span class="sxs-lookup"><span data-stu-id="ed836-190">Custom or automated processes</span></span>

## <a name="next-steps"></a><span data-ttu-id="ed836-191">Nächste Schritte</span><span class="sxs-lookup"><span data-stu-id="ed836-191">Next steps</span></span>

<span data-ttu-id="ed836-192">Die fünf R der Rationalisierung können auf digitale Ressourcen angewendet werden, um Rationalisierungsentscheidungen hinsichtlich des zukünftigen Zustands der einzelnen Anwendungen zu treffen.</span><span class="sxs-lookup"><span data-stu-id="ed836-192">Collectively, these 5 Rs of Rationalization can be applied to a digital estate to make rationalization decisions regarding the future state of each application.</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="ed836-193">Worum handelt es sich bei digitalen Ressourcen?</span><span class="sxs-lookup"><span data-stu-id="ed836-193">What is a digital estate?</span></span>](overview.md)
