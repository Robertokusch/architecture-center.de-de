---
title: 'Erläuterungen: Was ist Cloud-Governance?'
description: Enthält eine Beschreibung des Konzepts zur Ressourcenkontrolle für Azure und die Cloud.
author: petertay
ms.openlocfilehash: 63b04089aad5fc736641f8aaa6ff5247ea8ba13e
ms.sourcegitcommit: b3d74d8a89b2224fc796ce0e89cea447af43a0d4
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/11/2018
ms.locfileid: "35291114"
---
# <a name="explainer-what-is-cloud-resource-governance"></a>Erläuterungen: Was ist die Cloudressourcenkontrolle?

In der Erläuterung [Wie funktioniert Azure?](azure-explainer.md) haben Sie gelernt, dass es sich bei Azure um eine Sammlung von Servern und Netzwerkhardwarekomponenten handelt, auf denen im Auftrag von Benutzern virtualisierte Hardware und Software ausgeführt wird. Dank Azure kann die Entwicklungs- und IT-Abteilung Ihrer Organisation flexibel arbeiten, weil Ressourcen auf einfache Weise erstellt, gelesen, aktualisiert und gelöscht werden können.

Wenn Entwickler uneingeschränkten Zugriff auf Ressourcen haben, können sie zwar sehr flexibel vorgehen, aber es können sich auch unerwünschte Kosten ergeben. Beispielsweise kann für ein Entwicklungsteam die Bereitstellung einer Reihe von Ressourcen zu Testzwecken genehmigt werden, wobei dann nach Abschluss des Testens aber vergessen wird, diese zu löschen. Für diese Ressourcen fallen weiterhin Kosten an, obwohl die Nutzung nicht mehr genehmigt oder erforderlich ist. 

Die Lösung dieses Problems ist die Kontrolle (**Governance**) des Ressourcenzugriffs. „Governance“ bezieht sich auf das fortlaufende Verwalten, Überwachen und Überprüfen der Nutzung von Azure-Ressourcen, um die Ziele und Anforderungen Ihrer Organisation zu erfüllen. 

Da diese Ziele und Anforderungen für jede Organisation einzigartig sind, gibt es keinen allgemeingültigen Governance-Ansatz. Stattdessen werden von Azure zwei primäre Governance-Tools implementiert, die **ressourcenbasierte Zugriffssteuerung (RBAC)** und eine **Ressourcenrichtlinie**. Es liegt dann jeweils im Ermessen der Organisation, wie das Governance-Modell für die Nutzung dieser Komponenten entworfen wird.

Per RBAC werden Rollen definiert, und Rollen definieren die Funktionen für einen Benutzer, der der Rolle zugewiesen ist. Mit der Rolle **Besitzer** werden beispielsweise alle Funktionen (Erstellen, Lesen, Aktualisieren und Löschen) für eine Ressource aktiviert, während bei der Rolle **Leser** nur die Funktion „Lesen“ aktiviert wird. Für Rollen kann ein weiter Bereich definiert werden, der für viele Ressourcentypen gilt, oder ein enger Bereich, der nur für einige gilt. 

Über Ressourcenrichtlinien werden Regeln für die Ressourcenerstellung definiert. Per Ressourcenrichtlinie kann beispielsweise die SKU einer VM auf eine bestimmte vorab genehmigte Größe begrenzt werden. Oder eine Ressourcenrichtlinie kann das Hinzufügen eines Tags mit einer Kostenstelle erzwingen, wenn die Anforderung zum Erstellen der Ressource gesendet wird. 

Beim Konfigurieren dieser Tools ist es wichtig, die Governance und die Flexibilität der Organisation gegeneinander abzuwägen. Je restriktiver Ihre Governance-Richtlinie ist, desto weniger flexibel können Ihre Entwickler und IT-Experten arbeiten. Dies liegt daran, dass für eine restriktivere Governance-Richtlinie unter Umständen mehr manuelle Schritte erforderlich sind, z.B. das Erzwingen der Dateneingabe in ein Formular durch einen Entwickler oder das Senden einer E-Mail an eine Person im Governance-Team mit der Bitte, eine Ressource zu erstellen. Das Governance-Team verfügt nur über endliche Möglichkeiten, und es kann zu einem Rückstau kommen. Dies kann zu unproduktiven Entwicklungsteams führen, die warten müssen, bis ihre Ressourcen erstellt werden, und für nicht mehr benötigte Ressourcen können Kosten anfallen, während auf das Löschen gewartet wird.

## <a name="next-steps"></a>Nächste Schritte

Ihr nächster Schritt bei der Einführung von Azure besteht darin, dass Sie sich [mit der digitalen Identität in Azure](tenant-explainer.md) vertraut machen und [Ihren ersten Benutzer in Azure AD erstellen][docs-add-users-to-aad].

<!-- Links -->

[docs-add-users-to-aad]: /azure/active-directory/add-users-azure-active-directory?toc=/azure/architecture/cloud-adoption-guide/toc.json