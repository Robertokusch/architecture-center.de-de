---
title: 'CAF: Übersicht über die Disziplin „Sicherheitsbaseline“'
titleSuffix: Microsoft Cloud Adoption Framework for Azure
ms.service: architecture-center
ms.subservice: enterprise-cloud-adoption
ms.custom: governance
ms.date: 02/11/2019
description: Übersicht über die Disziplin „Sicherheitsbaseline“
author: BrianBlanchard
layout: LandingPage
ms.topic: landing-page
ms.openlocfilehash: 189fbd93371055f6aec63323988138f5bf90fd30
ms.sourcegitcommit: c053e6edb429299a0ad9b327888d596c48859d4a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/20/2019
ms.locfileid: "58247281"
---
# <a name="caf-security-baseline-discipline-overview"></a>CAF: Übersicht über die Disziplin „Sicherheitsbaseline“

„Sicherheitsbaseline“ ist eine der [fünf Disziplinen von Cloud Governance](../governance-disciplines.md) innerhalb des [CAF-Governancemodells](../overview.md). Sicherheit ist Bestandteil jeder IT-Bereitstellung, und durch die Cloud ergeben sich besondere Sicherheitsaspekte. Viele Unternehmen unterliegen gesetzlichen Bestimmungen, die den Schutz sensibler Daten zu einer wichtigen organisatorischen Priorität machen, wenn es um Cloudtransformation geht. Das Erkennen potenzieller Sicherheitsbedrohungen für Ihre Cloudumgebung und das Festlegen von Prozessen und Verfahren für den Umgang mit diesen Bedrohungen sollte für jedes IT- oder Cybersicherheitsteam Priorität haben. Mit der Disziplin „Sicherheitsbaseline“ wird sichergestellt, dass technische Anforderungen und Sicherheitseinschränkungen konsistent auf Cloudumgebungen angewendet werden, sobald sich diese Anforderungen stellen.

> [!NOTE]
> Governance der Sicherheitsbaseline ersetzt nicht die vorhandenen IT-Teams, Prozesse und Verfahren, die Ihre Organisation zum Sichern von in der Cloud bereitgestellten Ressourcen verwendet. Der Hauptzweck dieser Disziplin ist es, sicherheitsrelevante Geschäftsrisiken zu identifizieren und den für die Sicherheitsinfrastruktur zuständigen IT-Mitarbeitern eine Anleitung zur Risikominderung zu geben. Bei der Entwicklung von Governancerichtlinien und -prozessen ist darauf zu achten, dass die relevanten IT-Teams in Ihre Planungs- und Überprüfungsprozesse einbezogen werden.

In diesem Artikel wird der Ansatz zur Entwicklung einer Disziplin „Sicherheitsbaseline“ als Teil Ihrer Cloud Governance-Strategie beschrieben. Die wichtigste Zielgruppe für diese Anleitung sind die Cloudarchitekten Ihrer Organisation und andere Mitglieder Ihres Cloud Governance-Teams. Die Entscheidungen, Richtlinien und Prozesse, die sich aus dieser Disziplin ergeben, sollten jedoch die Einbeziehung von und Diskussion mit relevanten Mitgliedern Ihrer IT- und Sicherheitsteams beinhalten, insbesondere der technischen Führungskräfte, die für die Implementierung von Netzwerk-, Verschlüsselungs- und Identitätsdiensten verantwortlich sind.

Die richtigen Sicherheitsentscheidungen zu treffen, ist entscheidend für den Erfolg Ihrer Cloudbereitstellungen und größeren Geschäftserfolg. Wenn Ihre Organisation nicht über internes Fachwissen im Bereich Cybersicherheit verfügt, können Sie externe Sicherheitsberater als Bestandteil dieser Disziplin einbeziehen. Sie können auch [Microsoft Consulting Services](https://www.microsoft.com/enterprise/services), den Cloudeinführungsdienst [Microsoft FastTrack](https://azure.microsoft.com/programs/azure-fasttrack/) oder andere externe Experten für die Cloudeinführung einbeziehen, um Probleme im Zusammenhang mit dieser Disziplin zu besprechen.

## <a name="policy-statements"></a>Richtlinienanweisungen

Umsetzbare Richtlinienanweisungen und die daraus resultierenden Architekturanforderungen bilden die Grundlage für eine Disziplin „Sicherheitsbaseline“. Beispiele für Richtlinienanweisungen finden Sie im Artikel über [Richtlinienanweisungen für Sicherheitsbaseline](./policy-statements.md). Diese Beispiele können als Ausgangspunkt für die Governancerichtlinien Ihrer Organisation dienen.

> [!CAUTION]
> Die Beispielrichtlinien basieren auf allgemeinen Kundenerfahrungen. Zur besseren Anpassung dieser Richtlinien an spezifische Anforderungen für Cloud Governance führen Sie die folgenden Schritte aus, um Richtlinienanweisungen zu erstellen, die Ihre individuellen Geschäftsanforderungen erfüllen.

## <a name="developing-security-baseline-governance-policy-statements"></a>Entwickeln von Richtlinienanweisungen für Governance der Sicherheitsbaseline

Die folgenden sechs Schritte enthalten Beispiele und mögliche Optionen, die bei der Entwicklung von Governance der Sicherheitsbaseline zu berücksichtigen sind. Verwenden Sie die einzelnen Schritte als Ausgangspunkte für Diskussionen innerhalb Ihres Cloud Governance-Teams und mit betroffenen Geschäfts-, IT- und Sicherheitsteams in Ihrer Organisation, um die Richtlinien und Prozesse festzulegen, die zur Minimierung sicherheitsrelevanter Risiken erforderlich sind.

<!-- markdownlint-disable MD033 -->

<ul class="panelContent cardsE">
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
                        <h3>Vorlage für Sicherheitsbaseline</h3>
                        <p class="x-hidden-focus">Laden Sie die Vorlage zur Dokumentation einer Disziplin „Sicherheitsbaseline“ herunter.</p>
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
                        <p class="x-hidden-focus">Machen Sie sich mit den Motiven und Risiken vertraut, die häufig mit der Disziplin „Sicherheitsbaseline“ verbunden sind.</p>
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
                        <p class="x-hidden-focus">Indikatoren, um zu verstehen, ob es der richtige Zeitpunkt ist, in die Disziplin „Sicherheitsbaseline“ zu investieren.</p>
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
                        <p class="x-hidden-focus">Empfohlene Prozesse zur Unterstützung der Richtlinieneinhaltung in der Disziplin „Sicherheitsbaseline“.</p>
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
                        <p class="x-hidden-focus">Azure-Dienste, die zur Unterstützung der Disziplin „Sicherheitsbaseline“ implementiert werden können.</p>
                    </div>
                </div>
            </div>
        </div>
    </a>
</li>
</ul>

<!-- markdownlint-enable MD033 -->

## <a name="next-steps"></a>Nächste Schritte

Beginnen Sie mit der Bewertung von Geschäftsrisiken in einem bestimmten Umfeld.

> [!div class="nextstepaction"]
> [Grundlegendes zu Geschäftsrisiken](./business-risks.md)
