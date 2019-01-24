---
title: Auswählen einer Technologie zur Verarbeitung von natürlicher Sprache
description: ''
author: zoinerTejada
ms.date: 02/12/2018
ms.topic: guide
ms.service: architecture-center
ms.subservice: cloud-fundamentals
ms.openlocfilehash: 20dc51e661befcc09dd1751b031d445ff2b9fa1a
ms.sourcegitcommit: 1b50810208354577b00e89e5c031b774b02736e2
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/23/2019
ms.locfileid: "54486487"
---
# <a name="choosing-a-natural-language-processing-technology-in-azure"></a><span data-ttu-id="acdcd-102">Auswählen einer Technologie zur Verarbeitung von natürlicher Sprache in Azure</span><span class="sxs-lookup"><span data-stu-id="acdcd-102">Choosing a natural language processing technology in Azure</span></span>

<span data-ttu-id="acdcd-103">Die Freiformtextverarbeitung wird für Dokumente durchgeführt, die Absätze mit Text enthalten. Normalerweise dient dies der Suchunterstützung, wird aber auch für andere Aufgaben zur Verarbeitung von natürlicher Sprache (Natural Language Processing, NLP) verwendet, z.B. Stimmungsanalyse, Themenerkennung, Spracherkennung, Schlüsselwortextraktion und Dokumentkategorisierung.</span><span class="sxs-lookup"><span data-stu-id="acdcd-103">Free-form text processing is performed against documents containing paragraphs of text, typically for the purpose of supporting search, but is also used to perform other natural language processing (NLP) tasks such as sentiment analysis, topic detection, language detection, key phrase extraction, and document categorization.</span></span> <span data-ttu-id="acdcd-104">In diesem Artikel geht es um die Auswahlmöglichkeiten in Bezug auf Technologie, die der Unterstützung von NLP-Aufgaben dient.</span><span class="sxs-lookup"><span data-stu-id="acdcd-104">This article focuses on the technology choices that act in support of the NLP tasks.</span></span>

<!-- markdownlint-disable MD026 -->

## <a name="what-are-your-options-when-choosing-an-nlp-service"></a><span data-ttu-id="acdcd-105">Welche Optionen stehen Ihnen bei der Auswahl eines NLP-Diensts zur Verfügung?</span><span class="sxs-lookup"><span data-stu-id="acdcd-105">What are your options when choosing an NLP service?</span></span>

<!-- markdownlint-enable MD026 -->

<span data-ttu-id="acdcd-106">In Azure verfügen die folgenden Dienste über Funktionen zur Verarbeitung natürlicher Sprache:</span><span class="sxs-lookup"><span data-stu-id="acdcd-106">In Azure, the following services provide natural language processing (NLP) capabilities:</span></span>

- [<span data-ttu-id="acdcd-107">Azure HDInsight mit Spark und Spark MLlib</span><span class="sxs-lookup"><span data-stu-id="acdcd-107">Azure HDInsight with Spark and Spark MLlib</span></span>](/azure/hdinsight/spark/apache-spark-overview)
- [<span data-ttu-id="acdcd-108">Azure Databricks</span><span class="sxs-lookup"><span data-stu-id="acdcd-108">Azure Databricks</span></span>](/azure/azure-databricks/what-is-azure-databricks)
- [<span data-ttu-id="acdcd-109">Microsoft Cognitive Services</span><span class="sxs-lookup"><span data-stu-id="acdcd-109">Microsoft Cognitive Services</span></span>](/azure/cognitive-services/welcome)

## <a name="key-selection-criteria"></a><span data-ttu-id="acdcd-110">Wichtige Auswahlkriterien</span><span class="sxs-lookup"><span data-stu-id="acdcd-110">Key selection criteria</span></span>

<span data-ttu-id="acdcd-111">Beantworten Sie zunächst die folgenden Fragen, um die Auswahlmöglichkeiten einzuschränken:</span><span class="sxs-lookup"><span data-stu-id="acdcd-111">To narrow the choices, start by answering these questions:</span></span>

- <span data-ttu-id="acdcd-112">Möchten Sie vorgefertigte Modelle verwenden?</span><span class="sxs-lookup"><span data-stu-id="acdcd-112">Do you want to use prebuilt models?</span></span> <span data-ttu-id="acdcd-113">Wenn ja, können Sie erwägen, die APIs von Microsoft Cognitive Services zu verwenden.</span><span class="sxs-lookup"><span data-stu-id="acdcd-113">If yes, consider using the APIs offered by Microsoft Cognitive Services.</span></span>

- <span data-ttu-id="acdcd-114">Müssen Sie benutzerdefinierte Modelle anhand eines großen Korpus mit Textdaten trainieren?</span><span class="sxs-lookup"><span data-stu-id="acdcd-114">Do you need to train custom models against a large corpus of text data?</span></span> <span data-ttu-id="acdcd-115">Wenn ja, können Sie die Verwendung von Azure HDInsight mit Spark MLlib und Spark NLP erwägen.</span><span class="sxs-lookup"><span data-stu-id="acdcd-115">If yes, consider using Azure HDInsight with Spark MLlib and Spark NLP.</span></span>

- <span data-ttu-id="acdcd-116">Benötigen Sie spezielle NLP-Funktionen, z.B. Tokenisierung, Wortstammerkennung, Lemmatisierung und Vorkommenshäufigkeit/Inverse Dokumenthäufigkeit (Term Frequency/Inverse Document Frequency, TF/IDF)?</span><span class="sxs-lookup"><span data-stu-id="acdcd-116">Do you need low-level NLP capabilities like tokenization, stemming, lemmatization, and term frequency/inverse document frequency (TF/IDF)?</span></span> <span data-ttu-id="acdcd-117">Wenn ja, können Sie die Verwendung von Azure HDInsight mit Spark MLlib und Spark NLP erwägen.</span><span class="sxs-lookup"><span data-stu-id="acdcd-117">If yes, consider using Azure HDInsight with Spark MLlib and Spark NLP.</span></span>

- <span data-ttu-id="acdcd-118">Benötigen Sie einfache, allgemeine NLP-Funktionen wie Entitäts- und Absichtsidentifizierung, Themenerkennung, Rechtschreibprüfung oder Standpunktanalyse?</span><span class="sxs-lookup"><span data-stu-id="acdcd-118">Do you need simple, high-level NLP capabilities like entity and intent identification, topic detection, spell check, or sentiment analysis?</span></span> <span data-ttu-id="acdcd-119">Wenn ja, können Sie erwägen, die APIs von Microsoft Cognitive Services zu verwenden.</span><span class="sxs-lookup"><span data-stu-id="acdcd-119">If yes, consider using the APIs offered by Microsoft Cognitive Services.</span></span>

## <a name="capability-matrix"></a><span data-ttu-id="acdcd-120">Funktionsmatrix</span><span class="sxs-lookup"><span data-stu-id="acdcd-120">Capability matrix</span></span>

<span data-ttu-id="acdcd-121">In den folgenden Tabellen sind die Hauptunterschiede in Bezug auf die Funktionen zusammengefasst.</span><span class="sxs-lookup"><span data-stu-id="acdcd-121">The following tables summarize the key differences in capabilities.</span></span>

### <a name="general-capabilities"></a><span data-ttu-id="acdcd-122">Allgemeine Funktionen</span><span class="sxs-lookup"><span data-stu-id="acdcd-122">General capabilities</span></span>

| | <span data-ttu-id="acdcd-123">Azure HDInsight</span><span class="sxs-lookup"><span data-stu-id="acdcd-123">Azure HDInsight</span></span> | <span data-ttu-id="acdcd-124">Microsoft Cognitive Services</span><span class="sxs-lookup"><span data-stu-id="acdcd-124">Microsoft Cognitive Services</span></span> |
| --- | --- | --- |
| <span data-ttu-id="acdcd-125">Bereitstellung von vortrainierten Modellen als Dienst</span><span class="sxs-lookup"><span data-stu-id="acdcd-125">Provides pretrained models as a service</span></span> | <span data-ttu-id="acdcd-126">Nein </span><span class="sxs-lookup"><span data-stu-id="acdcd-126">No</span></span> | <span data-ttu-id="acdcd-127">JA</span><span class="sxs-lookup"><span data-stu-id="acdcd-127">Yes</span></span> |
| <span data-ttu-id="acdcd-128">REST-API</span><span class="sxs-lookup"><span data-stu-id="acdcd-128">REST API</span></span> | <span data-ttu-id="acdcd-129">JA</span><span class="sxs-lookup"><span data-stu-id="acdcd-129">Yes</span></span> | <span data-ttu-id="acdcd-130">JA</span><span class="sxs-lookup"><span data-stu-id="acdcd-130">Yes</span></span> |
| <span data-ttu-id="acdcd-131">Programmierbarkeit</span><span class="sxs-lookup"><span data-stu-id="acdcd-131">Programmability</span></span> | <span data-ttu-id="acdcd-132">Python, Scala, Java</span><span class="sxs-lookup"><span data-stu-id="acdcd-132">Python, Scala, Java</span></span> | <span data-ttu-id="acdcd-133">C#, Java, Node.js, Python, PHP, Ruby</span><span class="sxs-lookup"><span data-stu-id="acdcd-133">C#, Java, Node.js, Python, PHP, Ruby</span></span> |
| <span data-ttu-id="acdcd-134">Unterstützung der Verarbeitung von umfangreichen Datasets und großen Dokumenten</span><span class="sxs-lookup"><span data-stu-id="acdcd-134">Support processing of big data sets and large documents</span></span> | <span data-ttu-id="acdcd-135">JA</span><span class="sxs-lookup"><span data-stu-id="acdcd-135">Yes</span></span> | <span data-ttu-id="acdcd-136">Nein </span><span class="sxs-lookup"><span data-stu-id="acdcd-136">No</span></span> |

### <a name="low-level-natural-language-processing-capabilities"></a><span data-ttu-id="acdcd-137">Funktionen zur speziellen Verarbeitung von natürlicher Sprache</span><span class="sxs-lookup"><span data-stu-id="acdcd-137">Low-level natural language processing capabilities</span></span>

| | <span data-ttu-id="acdcd-138">Azure HDInsight</span><span class="sxs-lookup"><span data-stu-id="acdcd-138">Azure HDInsight</span></span> | <span data-ttu-id="acdcd-139">Microsoft Cognitive Services</span><span class="sxs-lookup"><span data-stu-id="acdcd-139">Microsoft Cognitive Services</span></span> |  
| --- | --- | --- |
| <span data-ttu-id="acdcd-140">Tokenizer</span><span class="sxs-lookup"><span data-stu-id="acdcd-140">Tokenizer</span></span> | <span data-ttu-id="acdcd-141">Ja (Spark NLP)</span><span class="sxs-lookup"><span data-stu-id="acdcd-141">Yes (Spark NLP)</span></span> | <span data-ttu-id="acdcd-142">Ja (API für linguistische Analyse)</span><span class="sxs-lookup"><span data-stu-id="acdcd-142">Yes (Linguistic Analysis API)</span></span> |
| <span data-ttu-id="acdcd-143">Wortstammerkennung</span><span class="sxs-lookup"><span data-stu-id="acdcd-143">Stemmer</span></span> | <span data-ttu-id="acdcd-144">Ja (Spark NLP)</span><span class="sxs-lookup"><span data-stu-id="acdcd-144">Yes (Spark NLP)</span></span> | <span data-ttu-id="acdcd-145">Nein </span><span class="sxs-lookup"><span data-stu-id="acdcd-145">No</span></span> |
| <span data-ttu-id="acdcd-146">Lemmatisierung</span><span class="sxs-lookup"><span data-stu-id="acdcd-146">Lemmatizer</span></span> | <span data-ttu-id="acdcd-147">Ja (Spark NLP)</span><span class="sxs-lookup"><span data-stu-id="acdcd-147">Yes (Spark NLP)</span></span> | <span data-ttu-id="acdcd-148">Nein </span><span class="sxs-lookup"><span data-stu-id="acdcd-148">No</span></span> |
| <span data-ttu-id="acdcd-149">Satzteilmarkierung</span><span class="sxs-lookup"><span data-stu-id="acdcd-149">Part of speech tagging</span></span> | <span data-ttu-id="acdcd-150">Ja (Spark NLP)</span><span class="sxs-lookup"><span data-stu-id="acdcd-150">Yes (Spark NLP)</span></span> | <span data-ttu-id="acdcd-151">Ja (API für linguistische Analyse)</span><span class="sxs-lookup"><span data-stu-id="acdcd-151">Yes (Linguistic Analysis API)</span></span> |
| <span data-ttu-id="acdcd-152">Vorkommenshäufigkeit/Inverse Dokumenthäufigkeit (Term Frequency/Inverse Document Frequency, TF/IDF)</span><span class="sxs-lookup"><span data-stu-id="acdcd-152">Term frequency/inverse-document frequency (TF/IDF)</span></span> | <span data-ttu-id="acdcd-153">Ja (Spark MLlib)</span><span class="sxs-lookup"><span data-stu-id="acdcd-153">Yes (Spark MLlib)</span></span> | <span data-ttu-id="acdcd-154">Nein </span><span class="sxs-lookup"><span data-stu-id="acdcd-154">No</span></span> |
| <span data-ttu-id="acdcd-155">Zeichenfolgenähnlichkeit – Berechnung der Edit-Distanz</span><span class="sxs-lookup"><span data-stu-id="acdcd-155">String similarity&mdash;edit distance calculation</span></span> | <span data-ttu-id="acdcd-156">Ja (Spark MLlib)</span><span class="sxs-lookup"><span data-stu-id="acdcd-156">Yes (Spark MLlib)</span></span> | <span data-ttu-id="acdcd-157">Nein </span><span class="sxs-lookup"><span data-stu-id="acdcd-157">No</span></span> |
| <span data-ttu-id="acdcd-158">N-Gramm-Berechnung</span><span class="sxs-lookup"><span data-stu-id="acdcd-158">N-gram calculation</span></span> | <span data-ttu-id="acdcd-159">Ja (Spark MLlib)</span><span class="sxs-lookup"><span data-stu-id="acdcd-159">Yes (Spark MLlib)</span></span> | <span data-ttu-id="acdcd-160">Nein </span><span class="sxs-lookup"><span data-stu-id="acdcd-160">No</span></span> |
| <span data-ttu-id="acdcd-161">Stoppwortentfernung</span><span class="sxs-lookup"><span data-stu-id="acdcd-161">Stop word removal</span></span> | <span data-ttu-id="acdcd-162">Ja (Spark MLlib)</span><span class="sxs-lookup"><span data-stu-id="acdcd-162">Yes (Spark MLlib)</span></span> | <span data-ttu-id="acdcd-163">Nein </span><span class="sxs-lookup"><span data-stu-id="acdcd-163">No</span></span> |

### <a name="high-level-natural-language-processing-capabilities"></a><span data-ttu-id="acdcd-164">Funktionen zur allgemeinen Verarbeitung von natürlicher Sprache</span><span class="sxs-lookup"><span data-stu-id="acdcd-164">High-level natural language processing capabilities</span></span>

| | <span data-ttu-id="acdcd-165">Azure HDInsight</span><span class="sxs-lookup"><span data-stu-id="acdcd-165">Azure HDInsight</span></span> | <span data-ttu-id="acdcd-166">Microsoft Cognitive Services</span><span class="sxs-lookup"><span data-stu-id="acdcd-166">Microsoft Cognitive Services</span></span> |
| --- | --- | --- |
| <span data-ttu-id="acdcd-167">Entitäts-/Absichtsidentifizierung und -extraktion</span><span class="sxs-lookup"><span data-stu-id="acdcd-167">Entity/intent identification & extraction</span></span> | <span data-ttu-id="acdcd-168">Nein </span><span class="sxs-lookup"><span data-stu-id="acdcd-168">No</span></span> | <span data-ttu-id="acdcd-169">Ja (Language Understanding Intelligent Service-API, LUIS-API)</span><span class="sxs-lookup"><span data-stu-id="acdcd-169">Yes (Language Understanding Intelligent Service (LUIS) API)</span></span> |
| <span data-ttu-id="acdcd-170">Themenerkennung</span><span class="sxs-lookup"><span data-stu-id="acdcd-170">Topic detection</span></span> | <span data-ttu-id="acdcd-171">Ja (Spark NLP)</span><span class="sxs-lookup"><span data-stu-id="acdcd-171">Yes (Spark NLP)</span></span> | <span data-ttu-id="acdcd-172">Ja (Textanalyse-API)</span><span class="sxs-lookup"><span data-stu-id="acdcd-172">Yes (Text Analytics API)</span></span> |
| <span data-ttu-id="acdcd-173">Rechtschreibprüfung</span><span class="sxs-lookup"><span data-stu-id="acdcd-173">Spell checking</span></span> | <span data-ttu-id="acdcd-174">Ja (Spark NLP)</span><span class="sxs-lookup"><span data-stu-id="acdcd-174">Yes (Spark NLP)</span></span> | <span data-ttu-id="acdcd-175">Ja (Bing-Rechtschreibprüfungs-API)</span><span class="sxs-lookup"><span data-stu-id="acdcd-175">Yes (Bing Spell Check API)</span></span> |
| <span data-ttu-id="acdcd-176">Stimmungsanalyse</span><span class="sxs-lookup"><span data-stu-id="acdcd-176">Sentiment analysis</span></span> | <span data-ttu-id="acdcd-177">Ja (Spark NLP)</span><span class="sxs-lookup"><span data-stu-id="acdcd-177">Yes (Spark NLP)</span></span> | <span data-ttu-id="acdcd-178">Ja (Textanalyse-API)</span><span class="sxs-lookup"><span data-stu-id="acdcd-178">Yes (Text Analytics API)</span></span> |
| <span data-ttu-id="acdcd-179">Spracherkennung</span><span class="sxs-lookup"><span data-stu-id="acdcd-179">Language detection</span></span> | <span data-ttu-id="acdcd-180">Nein </span><span class="sxs-lookup"><span data-stu-id="acdcd-180">No</span></span> | <span data-ttu-id="acdcd-181">Ja (Textanalyse-API)</span><span class="sxs-lookup"><span data-stu-id="acdcd-181">Yes (Text Analytics API)</span></span> |
| <span data-ttu-id="acdcd-182">Unterstützt neben Englisch noch mehrere andere Sprachen</span><span class="sxs-lookup"><span data-stu-id="acdcd-182">Supports multiple languages besides English</span></span> | <span data-ttu-id="acdcd-183">Nein </span><span class="sxs-lookup"><span data-stu-id="acdcd-183">No</span></span> | <span data-ttu-id="acdcd-184">Ja (variiert je nach API)</span><span class="sxs-lookup"><span data-stu-id="acdcd-184">Yes (varies by API)</span></span> |

## <a name="see-also"></a><span data-ttu-id="acdcd-185">Weitere Informationen</span><span class="sxs-lookup"><span data-stu-id="acdcd-185">See also</span></span>

[<span data-ttu-id="acdcd-186">Verarbeitung natürlicher Sprache</span><span class="sxs-lookup"><span data-stu-id="acdcd-186">Natural language processing</span></span>](../scenarios/natural-language-processing.md)
