---
title: Cloudentwurfsmuster
titleSuffix: Azure Architecture Center
description: Cloudentwurfsmuster für Microsoft Azure
keywords: Azure
author: dragon119
ms.date: 12/10/2018
ms.topic: design-pattern
ms.service: architecture-center
ms.subservice: cloud-fundamentals
ms.custom: seodec18
ms.openlocfilehash: 4229531366f1b0c3257384694cf4358da9e63177
ms.sourcegitcommit: c053e6edb429299a0ad9b327888d596c48859d4a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/20/2019
ms.locfileid: "58241481"
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
