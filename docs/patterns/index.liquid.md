---
title: Cloudentwurfsmuster
titleSuffix: Azure Architecture Center
description: Cloudentwurfsmuster für Microsoft Azure
keywords: Azure
author: dragon119
ms.date: 12/10/2018
ms.custom: seodec18
ms.openlocfilehash: 873d4cf02690a2cc3ffe4f35b044dedf70700fb5
ms.sourcegitcommit: 680c9cef945dff6fee5e66b38e24f07804510fa9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/04/2019
ms.locfileid: "54011022"
---
# <a name="cloud-design-patterns"></a><span data-ttu-id="6bc81-104">Cloudentwurfsmuster</span><span class="sxs-lookup"><span data-stu-id="6bc81-104">Cloud Design Patterns</span></span>

[!INCLUDE [header](../../_includes/header.md)]

<span data-ttu-id="6bc81-105">Diese Entwurfsmuster können Ihnen dabei helfen, zuverlässige, skalierbare und sichere Anwendungen in der Cloud zu erstellen.</span><span class="sxs-lookup"><span data-stu-id="6bc81-105">These design patterns are useful for building reliable, scalable, secure applications in the cloud.</span></span>

<span data-ttu-id="6bc81-106">Für jedes Muster werden das durch das Muster gelöste Problem, Überlegungen zum Anwenden des Musters und ein Beispiel auf der Grundlage von Microsoft Azure beschrieben.</span><span class="sxs-lookup"><span data-stu-id="6bc81-106">Each pattern describes the problem that the pattern addresses, considerations for applying the pattern, and an example based on Microsoft Azure.</span></span> <span data-ttu-id="6bc81-107">Die meisten der Muster enthalten Codebeispiele oder -ausschnitte, die die Implementierung des Musters in Azure veranschaulichen.</span><span class="sxs-lookup"><span data-stu-id="6bc81-107">Most of the patterns include code samples or snippets that show how to implement the pattern on Azure.</span></span> <span data-ttu-id="6bc81-108">Der Großteil der Muster ist jedoch für alle verteilten Systeme relevant, unabhängig davon, ob sie in Azure oder auf anderen Cloudplattformen gehostet werden.</span><span class="sxs-lookup"><span data-stu-id="6bc81-108">However, most of the patterns are relevant to any distributed system, whether hosted on Azure or on other cloud platforms.</span></span>

## <a name="problem-areas-in-the-cloud"></a><span data-ttu-id="6bc81-109">Problembereiche in der Cloud</span><span class="sxs-lookup"><span data-stu-id="6bc81-109">Problem areas in the cloud</span></span>

<!-- markdownlint-disable MD033 -->

<ul id="categories" class="panel">
<span data-ttu-id="6bc81-110">{%- for category in categories %}</span><span class="sxs-lookup"><span data-stu-id="6bc81-110">{%- for category in categories %}</span></span>
    <li>
    <span data-ttu-id="6bc81-111">{% include 'pattern-category-card' %}</span><span class="sxs-lookup"><span data-stu-id="6bc81-111">{% include 'pattern-category-card' %}</span></span>
    </li>
<span data-ttu-id="6bc81-112">{%- endfor %}</span><span class="sxs-lookup"><span data-stu-id="6bc81-112">{%- endfor %}</span></span>
</ul>

<!-- markdownlint-enable MD033 -->

## <a name="catalog-of-patterns"></a><span data-ttu-id="6bc81-113">Musterkatalog</span><span class="sxs-lookup"><span data-stu-id="6bc81-113">Catalog of patterns</span></span>

| <span data-ttu-id="6bc81-114">Muster</span><span class="sxs-lookup"><span data-stu-id="6bc81-114">Pattern</span></span> | <span data-ttu-id="6bc81-115">Zusammenfassung</span><span class="sxs-lookup"><span data-stu-id="6bc81-115">Summary</span></span> |
|---------|---------|
|         |         |

<span data-ttu-id="6bc81-116">{%- for pattern in patterns %} | [{{ pattern.title }}](./{{ pattern.file }}) | {{ pattern.description }} | {%- endfor %}</span><span class="sxs-lookup"><span data-stu-id="6bc81-116">{%- for pattern in patterns %} | [{{ pattern.title }}](./{{ pattern.file }}) | {{ pattern.description }} | {%- endfor %}</span></span>
