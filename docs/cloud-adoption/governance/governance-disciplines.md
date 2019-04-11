---
title: 'CAF: Die fünf Disziplinen der Cloud-Governance'
titleSuffix: Microsoft Cloud Adoption Framework for Azure
ms.service: architecture-center
ms.subservice: enterprise-cloud-adoption
ms.custom: governance
ms.date: 02/11/2019
description: Übersicht über die Governanceinhalte in CAF
author: BrianBlanchard
layout: LandingPage
ms.topic: landing-page
ms.openlocfilehash: 80bdfc6f55b76fc3ae57d8fc25ce68c385eaa009
ms.sourcegitcommit: c053e6edb429299a0ad9b327888d596c48859d4a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/20/2019
ms.locfileid: "58241681"
---
# <a name="the-five-disciplines-of-cloud-governance"></a>Die fünf Disziplinen der Cloud-Governance

<!-- markdownlint-disable MD033 -->

<ul class="panelContent cardsI">
<li style="display: flex; flex-direction: column;">
    <div class="cardSize">
        <div class="cardPadding" style="padding-bottom:10px;">
            <div class="card" style="padding-bottom:10px;">
                <div class="cardText" style="padding-left:0px;">
Jede Änderung von Geschäftsprozessen oder Technologieplattformen birgt Risiken. Cloud Governance-Teams, deren Mitglieder manchmal auch als Cloudverwalter bezeichnet werden, haben die Aufgabe, diese Risiken mit minimaler Unterbrechung der Einführungs- oder Innovationsanstrengungen zu minimieren.<br/><br/>Das CAF-Governancemodell steuert diese Entscheidungen (unabhängig von der gewählten Cloudplattform), indem es sich auf die <a href="#corporate-policy">Entwicklung der Unternehmensrichtlinien</a> und <a href="#disciplines-of-cloud-governance">Disziplinen von Cloud Governance</a> konzentriert. <a href="#actionable-journeys">Umsetzbare Entwurfsleitfäden</a> zeigen dieses Modell mithilfe von Azure-Diensten. Dieser Artikel dient als Einstiegsseite für die fünf Disziplinen des CAF-Governancemodells.
                </div>
            </div>
        </div>
    </div>
</li>
<li style="display: flex; flex-direction: column;">
    <a href="../_images/operational-transformation-govern-highres.png" style="display: flex; flex-direction: column; flex: 1 0 auto;">
        <div class="cardSize">
            <div class="cardPadding" style="padding-bottom:10px;">
                <div class="card" style="padding-bottom:10px;">
                    <div class="cardText" style="padding-left:0px;">
<img src="../_images/operational-transformation-govern-highres.png" alt="Diagram of the CAF governance model: Corporate policy and governance disciplines">
<br>
<i>Abbildung 1: Diagramm der Unternehmensrichtlinie und der fünf Disziplinen von Cloud Governance</i>
                    </div>
                </div>
            </div>
        </div>
    </a>
</li>
</ul>

<!-- markdownlint-enable MD033 -->

## <a name="disciplines-of-cloud-governance"></a>Disziplinen von Cloud Governance

Für jeden Cloudanbieter gibt es allgemeine Governance-Disziplinen, die als Leitfaden dienen können, um Richtlinien zu formulieren und Toolketten anzupassen. Diese Disziplinen sind ausschlaggebend für Entscheidungen über den richtigen Automatisierungsgrad und die Durchsetzung von Unternehmensrichtlinien bei Cloudanbietern.

<!-- markdownlint-disable MD033 -->

<ul class="panelContent cardsA">
<li style="display: flex; flex-direction: column;">
    <a href="./cost-management/overview.md" style="display: flex; flex-direction: column; flex: 1 0 auto;">
        <div class="cardSize" style="flex: 1 0 auto; display: flex;">
            <div class="cardPadding" style="display: flex;">
                <div class="card">
                    <div class="cardImageOuter">
                        <div class="cardImage">
                            <img src="../_images/governance/cost-management.png" class="x-hidden-focus"/>
                        </div>
                    </div>
                    <div class="cardText">
                        <h3>Cost Management</h3>
                        <p>Die Kosten sind für Cloudbenutzer ein Hauptanliegen. Entwickeln Sie Richtlinien zur Kostenkontrolle für alle Cloudplattformen.</p>
                    </div>
                </div>
            </div>
        </div>
    </a>
</li>
<li style="display: flex; flex-direction: column;">
    <a href="./security-baseline/overview.md" style="display: flex; flex-direction: column; flex: 1 0 auto;">
        <div class="cardSize" style="flex: 1 0 auto; display: flex;">
            <div class="cardPadding" style="display: flex;">
                <div class="card">
                    <div class="cardImageOuter">
                        <div class="cardImage">
                            <img src="../_images/governance/security-baseline.png" class="x-hidden-focus"/>
                        </div>
                    </div>
                    <div class="cardText">
                        <h3>Sicherheitsbaseline</h3>
                        <p>Sicherheit ist ein komplexes und persönliches Thema, das für jedes Unternehmen einzigartig ist. Sobald die Sicherheitsanforderungen festgelegt sind, wenden Cloud Governance-Richtlinien und -Durchsetzung diese Anforderungen auf Netzwerk-, Daten- und Ressourcenkonfigurationen an.</p>
                    </div>
                </div>
            </div>
        </div>
    </a>
</li>
<li style="display: flex; flex-direction: column;">
    <a href="./identity-baseline/overview.md" style="display: flex; flex-direction: column; flex: 1 0 auto;">
        <div class="cardSize" style="flex: 1 0 auto; display: flex;">
            <div class="cardPadding" style="display: flex;">
                <div class="card">
                    <div class="cardImageOuter">
                        <div class="cardImage">
                            <img src="../_images/governance/identity-baseline.png" class="x-hidden-focus"/>
                        </div>
                    </div>
                    <div class="cardText">
                        <h3>Identitätsbaseline</h3>
                        <p>Inkonsistenzen bei der Anwendung von Identitätsanforderungen können das Risiko von Sicherheitsverletzungen erhöhen. Die Identitätsbaselinedisziplin konzentriert sich auf Möglichkeiten zum Sicherstellen, dass die Identität bei der Cloudeinführung konsistent angewendet wird.</p>
                    </div>
                </div>
            </div>
        </div>
    </a>
</li>
<li style="display: flex; flex-direction: column;">
    <a href="./resource-consistency/overview.md" style="display: flex; flex-direction: column; flex: 1 0 auto;">
        <div class="cardSize" style="flex: 1 0 auto; display: flex;">
            <div class="cardPadding" style="display: flex;">
                <div class="card">
                    <div class="cardImageOuter">
                        <div class="cardImage">
                            <img src="../_images/governance/resource-consistency.png" class="x-hidden-focus"/>
                        </div>
                    </div>
                    <div class="cardText">
                        <h3>Ressourcenkonsistenz</h3>
                        <p>Cloudvorgänge hängen von der Konsistenz in der Ressourcenkonfiguration ab. Durch Governancetools können Ressourcen konsistent konfiguriert werden, um Risiken im Zusammenhang mit Onboarding, Abweichung, Ermittelbarkeit und Wiederherstellung zu minimieren.</p>
                    </div>
                </div>
            </div>
        </div>
    </a>
</li>
<li style="display: flex; flex-direction: column;">
    <a href="./deployment-acceleration/overview.md" style="display: flex; flex-direction: column; flex: 1 0 auto;">
        <div class="cardSize" style="flex: 1 0 auto; display: flex;">
            <div class="cardPadding" style="display: flex;">
                <div class="card">
                    <div class="cardImageOuter">
                        <div class="cardImage">
                            <img src="../_images/governance/deployment-acceleration.png" class="x-hidden-focus"/>
                        </div>
                    </div>
                    <div class="cardText">
                        <h3>Beschleunigung der Bereitstellung</h3>
                        <p>Zentralisierung, Standardisierung und Konsistenz bei Bereitstellungs- und Konfigurationsansätzen verbessern die Governancepraktiken. Wenn diese Aspekte über cloudbasierte Governancetools bereitgestellt werden, schaffen sie einen Cloudfaktor, der die Bereitstellungsaktivitäten beschleunigen kann.</p>
                    </div>
                </div>
            </div>
        </div>
    </a>
</li>
</ul>

<!-- markdownlint-enable MD033 -->
