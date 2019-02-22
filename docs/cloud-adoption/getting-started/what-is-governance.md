---
title: 'CAF: Was ist die Cloudressourcenkontrolle?'
description: Erläuterung zur Cloudressourcenkontrolle in Azure
author: petertaylor9999
ms.date: 2/11/2019
ms.openlocfilehash: ec8b0b04ac8a4782c215359cf907c3c092ae2f4d
ms.sourcegitcommit: 273e690c0cfabbc3822089c7d8bc743ef41d2b6e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/08/2019
ms.locfileid: "55897948"
---
<!-- markdownlint-disable MD026 -->

# <a name="what-is-cloud-resource-governance"></a>Was ist die Cloudressourcenkontrolle?

Unter [Wie funktioniert Azure?](what-is-azure.md) haben Sie gelernt, dass es sich bei Azure um eine Sammlung von Servern und Netzwerkhardwarekomponenten handelt, auf denen im Auftrag von Benutzern virtualisierte Hardware und Software ausgeführt wird. Dank Azure kann die Entwicklungs- und IT-Abteilung Ihrer Organisation flexibel arbeiten, weil Ressourcen auf einfache Weise erstellt, gelesen, aktualisiert und gelöscht werden können.

Wenn Entwickler uneingeschränkten Zugriff auf Ressourcen haben, können sie zwar sehr flexibel vorgehen, aber es können sich auch unerwünschte Kosten ergeben. Beispielsweise kann für ein Entwicklungsteam die Bereitstellung einer Reihe von Ressourcen zu Testzwecken genehmigt werden, wobei dann nach Abschluss des Testens aber vergessen wird, diese zu löschen. Für diese Ressourcen fallen weiterhin Kosten an, obwohl die Nutzung nicht mehr genehmigt oder erforderlich ist.

Die Lösung dieses Problems ist die Kontrolle (**Governance**) des Ressourcenzugriffs. „Governance“ bezieht sich auf das fortlaufende Verwalten, Überwachen und Überprüfen der Nutzung von Azure-Ressourcen, um die Ziele und Anforderungen Ihrer Organisation zu erfüllen.

> [!VIDEO https://www.microsoft.com/en-us/videoplayer/embed/RE2ii94]

Da diese Ziele und Anforderungen für jede Organisation einzigartig sind, gibt es keinen allgemeingültigen Governance-Ansatz. Stattdessen werden von Azure zwei primäre Governance-Tools implementiert: die **rollenbasierte Zugriffssteuerung** (Role Based Access Control, RBAC) und eine **Ressourcenrichtlinie**. Es liegt dann jeweils im Ermessen der Organisation, wie das Governance-Modell für die Nutzung dieser Komponenten entworfen wird.

Per RBAC werden Rollen definiert, und Rollen definieren die Funktionen für einen Benutzer, der der Rolle zugewiesen ist. Mit der Rolle **Besitzer** werden beispielsweise alle Funktionen (Erstellen, Lesen, Aktualisieren und Löschen) für eine Ressource aktiviert, während bei der Rolle **Leser** nur die Funktion „Lesen“ aktiviert wird. Für Rollen kann ein weiter Bereich definiert werden, der für viele Ressourcentypen gilt, oder ein enger Bereich, der nur für einige gilt.

Über Ressourcenrichtlinien werden Regeln für die Ressourcenerstellung definiert. Per Ressourcenrichtlinie kann beispielsweise die SKU einer VM auf eine bestimmte vorab genehmigte Größe begrenzt werden. Oder eine Ressourcenrichtlinie kann das Hinzufügen eines Tags mit einer Kostenstelle erzwingen, wenn die Anforderung zum Erstellen der Ressource gesendet wird.

Beim Konfigurieren dieser Tools ist es wichtig, die Governance und die Flexibilität der Organisation gegeneinander abzuwägen. Je restriktiver Ihre Governance-Richtlinie ist, desto weniger flexibel können Ihre Entwickler und IT-Experten arbeiten. Dies liegt daran, dass für eine restriktivere Governance-Richtlinie unter Umständen mehr manuelle Schritte erforderlich sind, z.B. das Erzwingen der Dateneingabe in ein Formular durch einen Entwickler oder das Senden einer E-Mail an eine Person im Governance-Team mit der Bitte, eine Ressource zu erstellen. Das Governance-Team verfügt nur über endliche Möglichkeiten, und es kann zu einem Rückstau kommen. Dies kann zu unproduktiven Entwicklungsteams führen, die warten müssen, bis ihre Ressourcen erstellt werden, und für nicht mehr benötigte Ressourcen können Kosten anfallen, während auf das Löschen gewartet wird.

## <a name="next-steps"></a>Nächste Schritte

Nachdem Sie sich nun mit dem Konzept der Kontrolle von Cloudressourcen vertraut gemacht haben, können Sie sich über das Verwalten des Ressourcenzugriffs in Azure informieren.

> [!div class="nextstepaction"]
> [Informationen zum Ressourcenzugriff in Azure](azure-resource-access.md)
