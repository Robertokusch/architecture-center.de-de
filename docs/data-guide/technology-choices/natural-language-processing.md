---
title: Auswählen einer Technologie zur Verarbeitung von natürlicher Sprache
description: ''
author: zoinerTejada
ms.date: 02/12/2018
ms.topic: guide
ms.service: architecture-center
ms.subservice: cloud-fundamentals
ms.openlocfilehash: 918ac19aeba36ce695c30896113a4c00e2f7c488
ms.sourcegitcommit: c053e6edb429299a0ad9b327888d596c48859d4a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/20/2019
ms.locfileid: "58245901"
---
# <a name="choosing-a-natural-language-processing-technology-in-azure"></a><span data-ttu-id="8e238-102">Auswählen einer Technologie zur Verarbeitung von natürlicher Sprache in Azure</span><span class="sxs-lookup"><span data-stu-id="8e238-102">Choosing a natural language processing technology in Azure</span></span>

<span data-ttu-id="8e238-103">Die Freiformtextverarbeitung wird für Dokumente durchgeführt, die Absätze mit Text enthalten. Normalerweise dient dies der Suchunterstützung, wird aber auch für andere Aufgaben zur Verarbeitung von natürlicher Sprache (Natural Language Processing, NLP) verwendet, z.B. Stimmungsanalyse, Themenerkennung, Spracherkennung, Schlüsselwortextraktion und Dokumentkategorisierung.</span><span class="sxs-lookup"><span data-stu-id="8e238-103">Free-form text processing is performed against documents containing paragraphs of text, typically for the purpose of supporting search, but is also used to perform other natural language processing (NLP) tasks such as sentiment analysis, topic detection, language detection, key phrase extraction, and document categorization.</span></span> <span data-ttu-id="8e238-104">In diesem Artikel geht es um die Auswahlmöglichkeiten in Bezug auf Technologie, die der Unterstützung von NLP-Aufgaben dient.</span><span class="sxs-lookup"><span data-stu-id="8e238-104">This article focuses on the technology choices that act in support of the NLP tasks.</span></span>

<!-- markdownlint-disable MD026 -->

## <a name="what-are-your-options-when-choosing-an-nlp-service"></a><span data-ttu-id="8e238-105">Welche Optionen stehen Ihnen bei der Auswahl eines NLP-Diensts zur Verfügung?</span><span class="sxs-lookup"><span data-stu-id="8e238-105">What are your options when choosing an NLP service?</span></span>

<!-- markdownlint-enable MD026 -->

<span data-ttu-id="8e238-106">In Azure verfügen die folgenden Dienste über Funktionen zur Verarbeitung natürlicher Sprache:</span><span class="sxs-lookup"><span data-stu-id="8e238-106">In Azure, the following services provide natural language processing (NLP) capabilities:</span></span>

- [<span data-ttu-id="8e238-107">Azure HDInsight mit Spark und Spark MLlib</span><span class="sxs-lookup"><span data-stu-id="8e238-107">Azure HDInsight with Spark and Spark MLlib</span></span>](/azure/hdinsight/spark/apache-spark-overview)
- [<span data-ttu-id="8e238-108">Azure Databricks</span><span class="sxs-lookup"><span data-stu-id="8e238-108">Azure Databricks</span></span>](/azure/azure-databricks/what-is-azure-databricks)
- [<span data-ttu-id="8e238-109">Microsoft Cognitive Services</span><span class="sxs-lookup"><span data-stu-id="8e238-109">Microsoft Cognitive Services</span></span>](/azure/cognitive-services/welcome)

## <a name="key-selection-criteria"></a><span data-ttu-id="8e238-110">Wichtige Auswahlkriterien</span><span class="sxs-lookup"><span data-stu-id="8e238-110">Key selection criteria</span></span>

<span data-ttu-id="8e238-111">Beantworten Sie zunächst die folgenden Fragen, um die Auswahlmöglichkeiten einzuschränken:</span><span class="sxs-lookup"><span data-stu-id="8e238-111">To narrow the choices, start by answering these questions:</span></span>

- <span data-ttu-id="8e238-112">Möchten Sie vorgefertigte Modelle verwenden?</span><span class="sxs-lookup"><span data-stu-id="8e238-112">Do you want to use prebuilt models?</span></span> <span data-ttu-id="8e238-113">Wenn ja, können Sie erwägen, die APIs von Microsoft Cognitive Services zu verwenden.</span><span class="sxs-lookup"><span data-stu-id="8e238-113">If yes, consider using the APIs offered by Microsoft Cognitive Services.</span></span>

- <span data-ttu-id="8e238-114">Müssen Sie benutzerdefinierte Modelle anhand eines großen Korpus mit Textdaten trainieren?</span><span class="sxs-lookup"><span data-stu-id="8e238-114">Do you need to train custom models against a large corpus of text data?</span></span> <span data-ttu-id="8e238-115">Wenn ja, können Sie die Verwendung von Azure HDInsight mit Spark MLlib und Spark NLP erwägen.</span><span class="sxs-lookup"><span data-stu-id="8e238-115">If yes, consider using Azure HDInsight with Spark MLlib and Spark NLP.</span></span>

- <span data-ttu-id="8e238-116">Benötigen Sie spezielle NLP-Funktionen, z.B. Tokenisierung, Wortstammerkennung, Lemmatisierung und Vorkommenshäufigkeit/Inverse Dokumenthäufigkeit (Term Frequency/Inverse Document Frequency, TF/IDF)?</span><span class="sxs-lookup"><span data-stu-id="8e238-116">Do you need low-level NLP capabilities like tokenization, stemming, lemmatization, and term frequency/inverse document frequency (TF/IDF)?</span></span> <span data-ttu-id="8e238-117">Wenn ja, können Sie die Verwendung von Azure HDInsight mit Spark MLlib und Spark NLP erwägen.</span><span class="sxs-lookup"><span data-stu-id="8e238-117">If yes, consider using Azure HDInsight with Spark MLlib and Spark NLP.</span></span>

- <span data-ttu-id="8e238-118">Benötigen Sie einfache, allgemeine NLP-Funktionen wie Entitäts- und Absichtsidentifizierung, Themenerkennung, Rechtschreibprüfung oder Standpunktanalyse?</span><span class="sxs-lookup"><span data-stu-id="8e238-118">Do you need simple, high-level NLP capabilities like entity and intent identification, topic detection, spell check, or sentiment analysis?</span></span> <span data-ttu-id="8e238-119">Wenn ja, können Sie erwägen, die APIs von Microsoft Cognitive Services zu verwenden.</span><span class="sxs-lookup"><span data-stu-id="8e238-119">If yes, consider using the APIs offered by Microsoft Cognitive Services.</span></span>

## <a name="capability-matrix"></a><span data-ttu-id="8e238-120">Funktionsmatrix</span><span class="sxs-lookup"><span data-stu-id="8e238-120">Capability matrix</span></span>

<span data-ttu-id="8e238-121">In den folgenden Tabellen sind die Hauptunterschiede in Bezug auf die Funktionen zusammengefasst.</span><span class="sxs-lookup"><span data-stu-id="8e238-121">The following tables summarize the key differences in capabilities.</span></span>

### <a name="general-capabilities"></a><span data-ttu-id="8e238-122">Allgemeine Funktionen</span><span class="sxs-lookup"><span data-stu-id="8e238-122">General capabilities</span></span>

| | <span data-ttu-id="8e238-123">Azure HDInsight</span><span class="sxs-lookup"><span data-stu-id="8e238-123">Azure HDInsight</span></span> | <span data-ttu-id="8e238-124">Microsoft Cognitive Services</span><span class="sxs-lookup"><span data-stu-id="8e238-124">Microsoft Cognitive Services</span></span> |
| --- | --- | --- |
| <span data-ttu-id="8e238-125">Bereitstellung von vortrainierten Modellen als Dienst</span><span class="sxs-lookup"><span data-stu-id="8e238-125">Provides pretrained models as a service</span></span> | <span data-ttu-id="8e238-126">Nein </span><span class="sxs-lookup"><span data-stu-id="8e238-126">No</span></span> | <span data-ttu-id="8e238-127">Ja</span><span class="sxs-lookup"><span data-stu-id="8e238-127">Yes</span></span> |
| <span data-ttu-id="8e238-128">REST-API</span><span class="sxs-lookup"><span data-stu-id="8e238-128">REST API</span></span> | <span data-ttu-id="8e238-129">Ja</span><span class="sxs-lookup"><span data-stu-id="8e238-129">Yes</span></span> | <span data-ttu-id="8e238-130">Ja</span><span class="sxs-lookup"><span data-stu-id="8e238-130">Yes</span></span> |
| <span data-ttu-id="8e238-131">Programmierbarkeit</span><span class="sxs-lookup"><span data-stu-id="8e238-131">Programmability</span></span> | <span data-ttu-id="8e238-132">Python, Scala, Java</span><span class="sxs-lookup"><span data-stu-id="8e238-132">Python, Scala, Java</span></span> | <span data-ttu-id="8e238-133">C#, Java, Node.js, Python, PHP, Ruby</span><span class="sxs-lookup"><span data-stu-id="8e238-133">C#, Java, Node.js, Python, PHP, Ruby</span></span> |
| <span data-ttu-id="8e238-134">Unterstützung der Verarbeitung von umfangreichen Datasets und großen Dokumenten</span><span class="sxs-lookup"><span data-stu-id="8e238-134">Support processing of big data sets and large documents</span></span> | <span data-ttu-id="8e238-135">Ja</span><span class="sxs-lookup"><span data-stu-id="8e238-135">Yes</span></span> | <span data-ttu-id="8e238-136">Nein </span><span class="sxs-lookup"><span data-stu-id="8e238-136">No</span></span> |

### <a name="low-level-natural-language-processing-capabilities"></a><span data-ttu-id="8e238-137">Funktionen zur speziellen Verarbeitung von natürlicher Sprache</span><span class="sxs-lookup"><span data-stu-id="8e238-137">Low-level natural language processing capabilities</span></span>

| | <span data-ttu-id="8e238-138">Azure HDInsight</span><span class="sxs-lookup"><span data-stu-id="8e238-138">Azure HDInsight</span></span> | <span data-ttu-id="8e238-139">Microsoft Cognitive Services</span><span class="sxs-lookup"><span data-stu-id="8e238-139">Microsoft Cognitive Services</span></span> |  
| --- | --- | --- |
| <span data-ttu-id="8e238-140">Tokenizer</span><span class="sxs-lookup"><span data-stu-id="8e238-140">Tokenizer</span></span> | <span data-ttu-id="8e238-141">Ja (Spark NLP)</span><span class="sxs-lookup"><span data-stu-id="8e238-141">Yes (Spark NLP)</span></span> | <span data-ttu-id="8e238-142">Ja (API für linguistische Analyse)</span><span class="sxs-lookup"><span data-stu-id="8e238-142">Yes (Linguistic Analysis API)</span></span> |
| <span data-ttu-id="8e238-143">Wortstammerkennung</span><span class="sxs-lookup"><span data-stu-id="8e238-143">Stemmer</span></span> | <span data-ttu-id="8e238-144">Ja (Spark NLP)</span><span class="sxs-lookup"><span data-stu-id="8e238-144">Yes (Spark NLP)</span></span> | <span data-ttu-id="8e238-145">Nein </span><span class="sxs-lookup"><span data-stu-id="8e238-145">No</span></span> |
| <span data-ttu-id="8e238-146">Lemmatisierung</span><span class="sxs-lookup"><span data-stu-id="8e238-146">Lemmatizer</span></span> | <span data-ttu-id="8e238-147">Ja (Spark NLP)</span><span class="sxs-lookup"><span data-stu-id="8e238-147">Yes (Spark NLP)</span></span> | <span data-ttu-id="8e238-148">Nein </span><span class="sxs-lookup"><span data-stu-id="8e238-148">No</span></span> |
| <span data-ttu-id="8e238-149">Satzteilmarkierung</span><span class="sxs-lookup"><span data-stu-id="8e238-149">Part of speech tagging</span></span> | <span data-ttu-id="8e238-150">Ja (Spark NLP)</span><span class="sxs-lookup"><span data-stu-id="8e238-150">Yes (Spark NLP)</span></span> | <span data-ttu-id="8e238-151">Ja (API für linguistische Analyse)</span><span class="sxs-lookup"><span data-stu-id="8e238-151">Yes (Linguistic Analysis API)</span></span> |
| <span data-ttu-id="8e238-152">Vorkommenshäufigkeit/Inverse Dokumenthäufigkeit (Term Frequency/Inverse Document Frequency, TF/IDF)</span><span class="sxs-lookup"><span data-stu-id="8e238-152">Term frequency/inverse-document frequency (TF/IDF)</span></span> | <span data-ttu-id="8e238-153">Ja (Spark MLlib)</span><span class="sxs-lookup"><span data-stu-id="8e238-153">Yes (Spark MLlib)</span></span> | <span data-ttu-id="8e238-154">Nein </span><span class="sxs-lookup"><span data-stu-id="8e238-154">No</span></span> |
| <span data-ttu-id="8e238-155">Zeichenfolgenähnlichkeit – Berechnung der Edit-Distanz</span><span class="sxs-lookup"><span data-stu-id="8e238-155">String similarity&mdash;edit distance calculation</span></span> | <span data-ttu-id="8e238-156">Ja (Spark MLlib)</span><span class="sxs-lookup"><span data-stu-id="8e238-156">Yes (Spark MLlib)</span></span> | <span data-ttu-id="8e238-157">Nein </span><span class="sxs-lookup"><span data-stu-id="8e238-157">No</span></span> |
| <span data-ttu-id="8e238-158">N-Gramm-Berechnung</span><span class="sxs-lookup"><span data-stu-id="8e238-158">N-gram calculation</span></span> | <span data-ttu-id="8e238-159">Ja (Spark MLlib)</span><span class="sxs-lookup"><span data-stu-id="8e238-159">Yes (Spark MLlib)</span></span> | <span data-ttu-id="8e238-160">Nein </span><span class="sxs-lookup"><span data-stu-id="8e238-160">No</span></span> |
| <span data-ttu-id="8e238-161">Stoppwortentfernung</span><span class="sxs-lookup"><span data-stu-id="8e238-161">Stop word removal</span></span> | <span data-ttu-id="8e238-162">Ja (Spark MLlib)</span><span class="sxs-lookup"><span data-stu-id="8e238-162">Yes (Spark MLlib)</span></span> | <span data-ttu-id="8e238-163">Nein </span><span class="sxs-lookup"><span data-stu-id="8e238-163">No</span></span> |

### <a name="high-level-natural-language-processing-capabilities"></a><span data-ttu-id="8e238-164">Funktionen zur allgemeinen Verarbeitung von natürlicher Sprache</span><span class="sxs-lookup"><span data-stu-id="8e238-164">High-level natural language processing capabilities</span></span>

| | <span data-ttu-id="8e238-165">Azure HDInsight</span><span class="sxs-lookup"><span data-stu-id="8e238-165">Azure HDInsight</span></span> | <span data-ttu-id="8e238-166">Microsoft Cognitive Services</span><span class="sxs-lookup"><span data-stu-id="8e238-166">Microsoft Cognitive Services</span></span> |
| --- | --- | --- |
| <span data-ttu-id="8e238-167">Entitäts-/Absichtsidentifizierung und -extraktion</span><span class="sxs-lookup"><span data-stu-id="8e238-167">Entity/intent identification and extraction</span></span> | <span data-ttu-id="8e238-168">Nein </span><span class="sxs-lookup"><span data-stu-id="8e238-168">No</span></span> | <span data-ttu-id="8e238-169">Ja (Language Understanding Intelligent Service-API, LUIS-API)</span><span class="sxs-lookup"><span data-stu-id="8e238-169">Yes (Language Understanding Intelligent Service (LUIS) API)</span></span> |
| <span data-ttu-id="8e238-170">Themenerkennung</span><span class="sxs-lookup"><span data-stu-id="8e238-170">Topic detection</span></span> | <span data-ttu-id="8e238-171">Ja (Spark NLP)</span><span class="sxs-lookup"><span data-stu-id="8e238-171">Yes (Spark NLP)</span></span> | <span data-ttu-id="8e238-172">Ja (Textanalyse-API)</span><span class="sxs-lookup"><span data-stu-id="8e238-172">Yes (Text Analytics API)</span></span> |
| <span data-ttu-id="8e238-173">Rechtschreibprüfung</span><span class="sxs-lookup"><span data-stu-id="8e238-173">Spell checking</span></span> | <span data-ttu-id="8e238-174">Ja (Spark NLP)</span><span class="sxs-lookup"><span data-stu-id="8e238-174">Yes (Spark NLP)</span></span> | <span data-ttu-id="8e238-175">Ja (Bing-Rechtschreibprüfungs-API)</span><span class="sxs-lookup"><span data-stu-id="8e238-175">Yes (Bing Spell Check API)</span></span> |
| <span data-ttu-id="8e238-176">Stimmungsanalyse</span><span class="sxs-lookup"><span data-stu-id="8e238-176">Sentiment analysis</span></span> | <span data-ttu-id="8e238-177">Ja (Spark NLP)</span><span class="sxs-lookup"><span data-stu-id="8e238-177">Yes (Spark NLP)</span></span> | <span data-ttu-id="8e238-178">Ja (Textanalyse-API)</span><span class="sxs-lookup"><span data-stu-id="8e238-178">Yes (Text Analytics API)</span></span> |
| <span data-ttu-id="8e238-179">Spracherkennung</span><span class="sxs-lookup"><span data-stu-id="8e238-179">Language detection</span></span> | <span data-ttu-id="8e238-180">Nein </span><span class="sxs-lookup"><span data-stu-id="8e238-180">No</span></span> | <span data-ttu-id="8e238-181">Ja (Textanalyse-API)</span><span class="sxs-lookup"><span data-stu-id="8e238-181">Yes (Text Analytics API)</span></span> |
| <span data-ttu-id="8e238-182">Unterstützt neben Englisch noch mehrere andere Sprachen</span><span class="sxs-lookup"><span data-stu-id="8e238-182">Supports multiple languages besides English</span></span> | <span data-ttu-id="8e238-183">Nein </span><span class="sxs-lookup"><span data-stu-id="8e238-183">No</span></span> | <span data-ttu-id="8e238-184">Ja (variiert je nach API)</span><span class="sxs-lookup"><span data-stu-id="8e238-184">Yes (varies by API)</span></span> |

## <a name="see-also"></a><span data-ttu-id="8e238-185">Weitere Informationen</span><span class="sxs-lookup"><span data-stu-id="8e238-185">See also</span></span>

[<span data-ttu-id="8e238-186">Verarbeitung natürlicher Sprache</span><span class="sxs-lookup"><span data-stu-id="8e238-186">Natural language processing</span></span>](../scenarios/natural-language-processing.md)
