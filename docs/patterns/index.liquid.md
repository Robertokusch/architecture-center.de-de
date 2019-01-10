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
# <a name="cloud-design-patterns"></a>Cloudentwurfsmuster

[!INCLUDE [header](../../_includes/header.md)]

Diese Entwurfsmuster können Ihnen dabei helfen, zuverlässige, skalierbare und sichere Anwendungen in der Cloud zu erstellen.

Für jedes Muster werden das durch das Muster gelöste Problem, Überlegungen zum Anwenden des Musters und ein Beispiel auf der Grundlage von Microsoft Azure beschrieben. Die meisten der Muster enthalten Codebeispiele oder -ausschnitte, die die Implementierung des Musters in Azure veranschaulichen. Der Großteil der Muster ist jedoch für alle verteilten Systeme relevant, unabhängig davon, ob sie in Azure oder auf anderen Cloudplattformen gehostet werden.

## <a name="problem-areas-in-the-cloud"></a>Problembereiche in der Cloud

<!-- markdownlint-disable MD033 -->

<ul id="categories" class="panel">
{%- for category in categories %}
    <li>
    {% include 'pattern-category-card' %}
    </li>
{%- endfor %}
</ul>

<!-- markdownlint-enable MD033 -->

## <a name="catalog-of-patterns"></a>Musterkatalog

| Muster | Zusammenfassung |
|---------|---------|
|         |         |

{%- for pattern in patterns %} | [{{ pattern.title }}](./{{ pattern.file }}) | {{ pattern.description }} | {%- endfor %}
