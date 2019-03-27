---
title: 'Framework für die Cloudeinführung (Cloud Adoption Framework, CAF): Cost Management-Tools in Azure'
titleSuffix: Microsoft Cloud Adoption Framework for Azure
ms.service: architecture-center
ms.subservice: enterprise-cloud-adoption
ms.custom: governance
ms.date: 02/11/2019
description: Cost Management-Tools in Azure
author: BrianBlanchard
ms.openlocfilehash: 58dfa604863f704fd9b9fbb8d0693447cecdaf84
ms.sourcegitcommit: c053e6edb429299a0ad9b327888d596c48859d4a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/20/2019
ms.locfileid: "58246601"
---
# <a name="cost-management-tools-in-azure"></a>Cost Management-Tools in Azure

[Cost Management](overview.md) ist eine der [fünf Disziplinen der Cloudgovernance](../governance-disciplines.md). Diese Disziplin konzentriert sich auf Möglichkeiten zum Einrichten von Plänen für Cloudausgaben, zum Zuordnen von Cloudbudgets, zur Überwachung und Durchsetzung von Cloudbudgets, zum Erkennen kostspieliger Abweichungen und zum Anpassen des Cloudgovernanceplans, wenn die tatsächlichen Ausgaben falsch ausgerichtet sind.

Im Folgenden ist eine Liste der nativen Azure-Tools aufgeführt, die zur Ausreifung der Richtlinien und Prozesse beitragen können, die diese Governancedisziplin unterstützen.

|  | [Azure-Portal](https://azure.microsoft.com/features/azure-portal/)  | [Azure Cost Management](/azure/cost-management/overview-cost-mgt)  | [Azure EA-Inhaltspaket](/power-bi/service-connect-to-azure-enterprise)  | [Azure Policy](/azure/governance/policy/overview) |
|---------|---------|---------|---------|---------|
|Enterprise Agreement erforderlich?     | Nein          | Ja (nicht erforderlich bei [Cloudyn](/azure/cost-management/overview))         | Ja         | Nein          |
|Budgetkontrolle     | Nein          | Ja         | Nein         | Ja         |
|Überwachen der Ausgaben für eine einzelne Ressource    | Ja         | Ja         | Ja         | Nein          |
|Überwachen der Ausgaben für mehrere Ressourcen    | Nein          | Ja        | Ja         | Nein          |
|Steuern der Ausgaben für eine einzelne Ressource     | Ja – manuelle Größenanpassung         | Ja         | Nein         | Ja         |
|Durchsetzen der Ausgaben für mehrere Ressourcen    | Nein          | Ja         | Nein         | Ja         |
|Überwachen und Erkennen von Trends     | Ja – eingeschränkt         | Ja        | Ja         | Nein          |
|Erkennen von Anomalien in den Ausgaben     | Nein          | Ja        | Ja         | Nein         |
|Kommunizieren von Abweichungen     | Nein         | Ja        | Ja        | Nein         |
