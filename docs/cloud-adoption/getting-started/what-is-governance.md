---
title: 'CAF: Was ist die Cloudressourcenkontrolle?'
titleSuffix: Microsoft Cloud Adoption Framework for Azure
ms.service: architecture-center
ms.subservice: enterprise-cloud-adoption
ms.date: 02/11/2019
description: Erläuterung zur Cloudressourcenkontrolle in Azure
author: petertaylor9999
ms.openlocfilehash: 0989a5aad8a6290cce07fd71771c690bbd615e0d
ms.sourcegitcommit: 579c39ff4b776704ead17a006bf24cd4cdc65edd
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/17/2019
ms.locfileid: "59068853"
---
<!-- markdownlint-disable MD026 -->

# <a name="what-is-cloud-resource-governance"></a>Was ist die Cloudressourcenkontrolle?

Unter [Wie funktioniert Azure?](what-is-azure.md) haben Sie gelernt, dass es sich bei Azure um eine Sammlung von Servern und Netzwerkhardwarekomponenten handelt, auf denen im Auftrag von Benutzern virtualisierte Hardware und Software ausgeführt wird. Dank Azure kann die Anwendungsentwicklungs- und IT-Abteilung Ihrer Organisation flexibel arbeiten, weil Ressourcen auf einfache Weise erstellt, gelesen, aktualisiert und gelöscht werden können.

Während der uneingeschränkte Zugriff auf Ressourcen Entwickler sehr agil werden lässt, kann dieser jedoch auch zu unerwarteten Kosten führen. Beispielsweise kann für ein Entwicklungsteam die Bereitstellung einer Reihe von Ressourcen zu Testzwecken genehmigt werden, wobei dann nach Abschluss des Testens aber vergessen wird, diese zu löschen. Für diese Ressourcen fallen weiterhin Kosten an, selbst wenn sie nicht mehr genehmigt oder erforderlich sind.

Die Lösung ist die Kontrolle (bzw. Governance) des Ressourcenzugriffs. **Governance** bezieht sich auf das laufende Verwalten, Überwachen und Überprüfen der Nutzung von Azure-Ressourcen zum Erfüllen der Anforderungen Ihrer Organisation.

<!-- markdownlint-disable MD034 -->

> [!VIDEO https://www.microsoft.com/en-us/videoplayer/embed/RE2ii94]

<!-- markdownlint-enable MD034 -->

Diese Anforderungen sind für jedes Unternehmen spezifisch, weshalb ein Allzweckansatz für Governance nicht hilfreich ist. Es liegt stattdessen im Ermessen der Organisation, wie ihr Governancemodell mithilfe der beiden primären Governancetools von Azure umgesetzt wird: **Rollenbasierte Zugriffssteuerung** (Role Based Access Control, RBAC) und **Ressourcenrichtlinie**.

Per RBAC werden Rollen definiert, und Rollen legen die Möglichkeiten aller Benutzer fest, die der jeweiligen Rolle zugewiesen sind. Mit der Rolle **Besitzer** werden beispielsweise Vollzugriffsberechtigungen (Erstellen, Lesen, Aktualisieren und Löschen) für eine Ressource erteilt, während bei der Rolle **Leser** nur die Berechtigung „Lesen“ erteilt wird. Für Rollen kann ein weiter Bereich definiert werden, der für viele Ressourcentypen gilt, oder ein enger Bereich, der nur für einige gilt.

Über Ressourcenrichtlinien werden Regeln für die Ressourcenerstellung definiert. Per Ressourcenrichtlinie kann beispielsweise die SKU eines virtuellen Computers (VM) auf eine bestimmte vorab genehmigte Größe begrenzt werden. Eine weitere Ressourcenrichtlinie kann das Hinzufügen eines Tags für eine zugewiesene Kostenstelle erzwingen, wenn die Anforderung zum Erstellen der Ressource gesendet wird.

Bei der Konfiguration dieser Tools ist es wichtig, Governance und Agilität in der Organisation in Einklang zu bringen. Je restriktiver Ihre Governancerichtlinie ist, desto weniger agil sind Ihre Entwickler und IT-Fachkräfte. Eine restriktivere Governancerichtlinie erfordert ggf. mehr manuelle Schritte, z.B. das Erzwingen der Dateneingabe in ein Formular durch einen Entwickler oder das Senden einer E-Mail an ein Mitglied des Governanceteams mit der Bitte, eine Ressource manuell zu erstellen. Das Governanceteam verfügt über begrenzte Kapazitäten und kann zu einem Engpass werden. Dies führt dazu, dass Entwicklungsteams unproduktiv auf die Erstellung ihrer Ressourcen warten oder nicht benötigte Ressourcen Kosten verursachen, bevor sie gelöscht werden.

## <a name="next-steps"></a>Nächste Schritte

Nachdem Sie sich nun mit dem Konzept der Kontrolle von Cloudressourcen vertraut gemacht haben, können Sie sich über das Verwalten des Ressourcenzugriffs in Azure informieren.

> [!div class="nextstepaction"]
> [Informationen zum Ressourcenzugriff in Azure](azure-resource-access.md)
