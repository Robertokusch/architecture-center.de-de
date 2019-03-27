---
title: 'CAF: Disziplin „Kostenverwaltung“'
description: Erläuterung der Kostenverwaltung in Bezug auf Cloud Governance
titleSuffix: Microsoft Cloud Adoption Framework for Azure
ms.service: architecture-center
ms.subservice: enterprise-cloud-adoption
ms.custom: governance
ms.date: 02/11/2019
author: BrianBlanchard
layout: LandingPage
ms.topic: landing-page
ms.openlocfilehash: a86b1a75da57cec9c9aaf88abb70049f70731561
ms.sourcegitcommit: c053e6edb429299a0ad9b327888d596c48859d4a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/20/2019
ms.locfileid: "58243701"
---
# <a name="cost-management-discipline-overview"></a>Übersicht über die Disziplin „Kostenverwaltung“

Kostenverwaltung ist eine der [fünf Disziplinen zur Cloud Governance](../governance-disciplines.md) innerhalb des [CAF-Governancemodells](../overview.md). Für viele Kunden ist die Kostenkontrolle bei der Einführung von Cloudtechnologien ein wichtiges Thema. Das Gleichgewicht zwischen Leistungsanforderungen, Einführungsrhythmus und Kosten der Clouddienste kann eine Herausforderung darstellen. Dies ist besonders relevant bei großen Unternehmenstransformationen, bei denen Cloudtechnologien implementiert werden. In diesem Abschnitt wird der Ansatz zur Entwicklung einer Disziplin „Kostenverwaltung“ als Teil einer Cloud Governance-Strategie beschrieben.  

> [!NOTE]
> Die Kontrolle der Kostenverwaltung ersetzt nicht die bestehenden Geschäftsteams, Buchhaltungspraktiken und -verfahren, die an der finanziellen Verwaltung der IT-bezogenen Kosten in Ihrem Unternehmen beteiligt sind. Der Hauptzweck dieser Disziplin besteht darin, potenzielle Cloudrisiken im Zusammenhang mit IT-Ausgaben zu identifizieren und den Geschäfts- und IT-Teams, die für die Bereitstellung und Verwaltung von Cloudbereitstellungen verantwortlich sind, eine Anleitung zur Risikominderung zu geben. Bei der Entwicklung von Governancerichtlinien und -prozessen ist darauf zu achten, dass die relevanten Geschäfts- und IT-Mitarbeiter in Ihre Planungs- und Überprüfungsprozesse einbezogen werden.

Die wichtigste Zielgruppe für diese Anleitung sind die Cloudarchitekten Ihres Unternehmens und andere Mitglieder Ihres Cloud Governance-Teams. Die Entscheidungen, Richtlinien und Prozesse, die sich aus dieser Disziplin ergeben, sollten jedoch die Einbeziehung von und Diskussion mit relevanten Mitgliedern Ihrer Geschäfts- und IT-Teams beinhalten, insbesondere diejenigen Führungskräften, die für den Besitz, die Verwaltung und die Bezahlung von cloudbasierten Workloads verantwortlich sind.

## <a name="policy-statements"></a>Richtlinienanweisungen

Umsetzbare Richtlinienanweisungen und die daraus resultierenden Architekturanforderungen bilden die Grundlage für eine Disziplin „Kostenverwaltung“. Beispiele für Richtlinienanweisungen finden Sie im Artikel über [Richtlinienanweisungen zur Kostenverwaltung](./policy-statements.md). Diese Beispiele können als Ausgangspunkt für die Governancerichtlinien Ihres Unternehmens dienen.

> [!CAUTION]
> Die Beispielrichtlinien basieren auf allgemeinen Kundenerfahrungen. Zur besseren Anpassung dieser Richtlinien an spezifische Anforderungen zur Cloud Governance führen Sie die folgenden Schritte aus, um Richtlinienanweisungen zu erstellen, die Ihre individuellen Geschäftsanforderungen erfüllen.

## <a name="developing-cost-management-governance-policy-statements"></a>Entwickeln von Richtlinienanweisungen zur Kontrolle der Kostenverwaltung

Die folgenden sechs Schritte helfen Ihnen bei der Definition von Governancerichtlinien zur Kostenkontrolle in Ihrer Umgebung.

<!-- markdownlint-disable MD033 -->

<ul  class="panelContent cardsE">
<li style="display: flex; flex-direction: column;">
    <a href="./template.md">
        <div class="cardSize">
            <div class="cardPadding" >
                <div class="card" >
                    <div class="cardImageOuter">
                        <div class="cardImage">
                            <img src="../../_images/governance/process-template.png" class="x-hidden-focus"/>
                        </div>
                    </div>
                    <div class="cardText" style="padding-left:0px;">
                        <h3>Kostenverwaltungsvorlage</h3>
                        <p class="x-hidden-focus">Laden Sie die Vorlage zur Dokumentation einer Disziplin „Kostenverwaltung“ herunter.</p>
                    </div>
                </div>
            </div>
        </div>
    </a>
</li><li style="display: flex; flex-direction: column;">
    <a href="./business-risks.md">
        <div class="cardSize">
            <div class="cardPadding" >
                <div class="card" >
                    <div class="cardImageOuter">
                        <div class="cardImage">
                            <img src="../../_images/governance/process-risks.png" class="x-hidden-focus"/>
                        </div>
                    </div>
                    <div class="cardText" style="padding-left:0px;">
                        <h3>Geschäftsrisiken</h3>
                        <p class="x-hidden-focus">Machen Sie sich mit den Motiven und Risiken vertraut, die häufig mit der Disziplin „Kostenverwaltung“ verbunden sind.</p>
                    </div>
                </div>
            </div>
        </div>
    </a>
</li>
<li style="display: flex; flex-direction: column;">
    <a href="./metrics-tolerance.md">
        <div class="cardSize">
            <div class="cardPadding" >
                <div class="card" >
                    <div class="cardImageOuter">
                        <div class="cardImage">
                            <img src="../../_images/governance/process-metrics.png" class="x-hidden-focus"/>
                        </div>
                    </div>
                    <div class="cardText" style="padding-left:0px;">
                        <h3>Indikatoren und Metriken</h3>
                        <p class="x-hidden-focus">Indikatoren, um zu verstehen, ob es der richtige Zeitpunkt ist, in die Disziplin „Kostenverwaltung“ zu investieren.</p>
                    </div>
                </div>
            </div>
        </div>
    </a>
</li>
<li style="display: flex; flex-direction: column;">
    <a href="./compliance-processes.md">
        <div class="cardSize">
            <div class="cardPadding" >
                <div class="card" >
                    <div class="cardImageOuter">
                        <div class="cardImage">
                            <img src="../../_images/governance/process-enforce.png" class="x-hidden-focus"/>
                        </div>
                    </div>
                    <div class="cardText" style="padding-left:0px;">
                        <h3>Prozesse zur Einhaltung von Richtlinien</h3>
                        <p class="x-hidden-focus">Empfohlene Prozesse zur Unterstützung der Richtlinieneinhaltung in der Disziplin „Kostenverwaltung“.</p>
                    </div>
                </div>
            </div>
        </div>
    </a>
</li>
<li style="display: flex; flex-direction: column;">
    <a href="./discipline-improvement.md">
        <div class="cardSize">
            <div class="cardPadding" >
                <div class="card" >
                    <div class="cardImageOuter">
                        <div class="cardImage">
                            <img src="../../_images/governance/process-maturity.png" class="x-hidden-focus"/>
                        </div>
                    </div>
                    <div class="cardText" style="padding-left:0px;">
                        <h3>Einsatzreife</h3>
                        <p class="x-hidden-focus">Stimmen Sie die Einsatzreife der Cloudverwaltung mit den Phasen der Cloudeinführung ab.</p>
                    </div>
                </div>
            </div>
        </div>
    </a>
</li>
<li style="display: flex; flex-direction: column;">
    <a href="./toolchain.md">
        <div class="cardSize">
            <div class="cardPadding" >
                <div class="card" >
                    <div class="cardImageOuter">
                        <div class="cardImage">
                            <img src="../../_images/governance/process-toolchain.png" class="x-hidden-focus"/>
                        </div>
                    </div>
                    <div class="cardText" style="padding-left:0px;">
                        <h3>Toolkette</h3>
                        <p class="x-hidden-focus">Azure-Dienste, die zur Unterstützung der Disziplin „Kostenverwaltung“ implementiert werden können.</p>
                    </div>
                </div>
            </div>
        </div>
    </a>
</li>
</ul>

## <a name="next-steps"></a>Nächste Schritte

Beginnen Sie mit der Bewertung von Geschäftsrisiken in einem bestimmten Umfeld.

> [!div class="nextstepaction"]
> [Grundlegendes zu Geschäftsrisiken](./business-risks.md)

<!-- markdownlint-enable MD033 -->
