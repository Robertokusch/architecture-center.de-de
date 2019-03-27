---
title: Erstellen von Microservices in Azure
description: 'Entwerfen, Erstellen und Betreiben von Microservices-Architekturen in Azure'
ms.date: 03/07/2019
layout: LandingPage
ms.topic: landing-page
---

# <a name="building-microservices-on-azure"></a>Erstellen von Microservices in Azure

<!-- markdownlint-disable MD033 -->

<img src="../_images/microservices.svg" style="float:left; margin-top:8px; margin-right:8px; max-width: 80px; max-height: 80px;"/>

Microservices sind ein beliebtes Architekturkonzept für die Erstellung robuster, hochgradig skalierbarer, unabhängig bereitstellbarer Anwendungen, die sich schnell weiterentwickeln lassen. Eine erfolgreiche Microservices-Architektur erfordert jedoch eine andere Herangehensweise an das Entwerfen und Erstellen von Anwendungen.

<ul  class="panelContent cardsZ">
<li style="display: flex; flex-direction: column;">
    <a href="./introduction.md" style="display: flex; flex-direction: column; flex: 1 0 auto;">
        <div class="cardSize" style="flex: 1 0 auto; display: flex;">
            <div class="cardPadding" style="display: flex;">
                <div class="card">
                    <div class="cardText">
                        <h3>Was sind Microservices?</h3>
                        <p>Inwiefern unterscheiden sich Microservices von anderen Architekturen, und wann sollten sie verwendet werden?</p>
                    </div>
                </div>
            </div>
        </div>
    </a>
</li>
<li style="display: flex; flex-direction: column;">
    <a href="../guide/architecture-styles/microservices.md" style="display: flex; flex-direction: column; flex: 1 0 auto;">
        <div class="cardSize" style="flex: 1 0 auto; display: flex;">
            <div class="cardPadding" style="display: flex;">
                <div class="card">
                    <div class="cardText">
                        <h3>Architekturstil für Microservices</h3>
                        <p>Allgemeine Übersicht über die Microservices-Architektur</p>
                    </div>
                </div>
            </div>
        </div>
    </a>
</li>
</ul>

## <a name="examples-of-microservices-architectures"></a>Beispiele für Microservices-Architekturen

<ul  class="panelContent cardsZ">
<li style="display: flex; flex-direction: column;">
    <a href="../example-scenario/infrastructure/service-fabric-microservices.md" style="display: flex; flex-direction: column; flex: 1 0 auto;">
        <div class="cardSize" style="flex: 1 0 auto; display: flex;">
            <div class="cardPadding" style="display: flex;">
                <div class="card">
                    <div class="cardText">
                        <h3>Zerlegen monolithischer Anwendungen mithilfe von Service Fabric</h3>
                        <p>Ein iterativer Ansatz für die Zerlegung einer ASP.NET-Website in Microservices.</p>
                    </div>
                </div>
            </div>
        </div>
    </a>
</li>
<li style="display: flex; flex-direction: column;">
    <a href="../example-scenario/data/ecommerce-order-processing.md" style="display: flex; flex-direction: column; flex: 1 0 auto;">
        <div class="cardSize" style="flex: 1 0 auto; display: flex;">
            <div class="cardPadding" style="display: flex;">
                <div class="card">
                    <div class="cardText">
                        <h3>Skalierbare Auftragsverarbeitung in Azure</h3>
                        <p>Auftragsverarbeitung mit einem funktionalen, über Microservices implementierten Programmiermodell.</p>
                    </div>
                </div>
            </div>
        </div>
    </a>
</li>
</ul>

## <a name="build-a-microservices-application"></a>Erstellen einer Microservices-Anwendung

<ul  class="panelContent cardsZ">
<li style="display: flex; flex-direction: column;">
    <a href="./model/domain-analysis.md" style="display: flex; flex-direction: column; flex: 1 0 auto;">
        <div class="cardSize" style="flex: 1 0 auto; display: flex;">
            <div class="cardPadding" style="display: flex;">
                <div class="card">
                    <div class="cardText">
                        <h3>Verwenden der Domänenanalyse zur Modellierung von Microservices</h3>
                        <p>Nutzen Sie die Domänenanalyse, um Ihre Microservice-Grenzen zu definieren und häufige Probleme zu vermeiden.</p>
                    </div>
                </div>
            </div>
        </div>
    </a>
</li>
<li style="display: flex; flex-direction: column;">
    <a href="../reference-architectures/microservices/aks.md" style="display: flex; flex-direction: column; flex: 1 0 auto;">
        <div class="cardSize" style="flex: 1 0 auto; display: flex;">
            <div class="cardPadding" style="display: flex;">
                <div class="card">
                    <div class="cardText">
                        <h3>Referenzarchitektur für Azure Kubernetes Service (AKS)</h3>
                        <p>Diese Referenzarchitektur zeigt eine einfache AKS-Konfiguration, die als Ausgangspunkt für die meisten Bereitstellungen dienen kann.</p>
                    </div>
                </div>
            </div>
        </div>
    </a>
</li>
<li style="display: flex; flex-direction: column;">
    <a href="../reference-architectures/microservices/service-fabric.md" style="display: flex; flex-direction: column; flex: 1 0 auto;">
        <div class="cardSize" style="flex: 1 0 auto; display: flex;">
            <div class="cardPadding" style="display: flex;">
                <div class="card">
                    <div class="cardText">
                        <h3>Referenzarchitektur für Azure Service Fabric</h3>
                        <p>Diese Referenzarchitektur zeigt eine empfohlene Konfiguration, die als Ausgangspunkt für die meisten Bereitstellungen dienen kann.</p>
                    </div>
                </div>
            </div>
        </div>
    </a>
</li>
<li style="display: flex; flex-direction: column;">
    <a href="./design/index.md" style="display: flex; flex-direction: column; flex: 1 0 auto;">
        <div class="cardSize" style="flex: 1 0 auto; display: flex;">
            <div class="cardPadding" style="display: flex;">
                <div class="card">
                    <div class="cardText">
                        <h3>Entwerfen einer Microservices-Architektur</h3>
                        <p>Diese Artikel enthalten ausführlichere Informationen zur Erstellung einer Microservices-Anwendung auf der Grundlage einer Referenzimplementierung mit Azure Kubernetes Service (AKS).</p>
                    </div>
                </div>
            </div>
        </div>
    </a>
</li>
<li style="display: flex; flex-direction: column;">
    <a href="./design/patterns.md" style="display: flex; flex-direction: column; flex: 1 0 auto;">
        <div class="cardSize" style="flex: 1 0 auto; display: flex;">
            <div class="cardPadding" style="display: flex;">
                <div class="card">
                    <div class="cardText">
                        <h3>Entwurfsmuster</h3>
                        <p>Eine Reihe hilfreicher Entwurfsmuster für Microservices.</p>
                    </div>
                </div>
            </div>
        </div>
    </a>
</li>
</ul>

## <a name="operate-microservices-in-production"></a>Betreiben von Microservices in der Produktionsumgebung

<ul  class="panelContent cardsZ">
<li style="display: flex; flex-direction: column;">
    <a href="./logging-monitoring.md" style="display: flex; flex-direction: column; flex: 1 0 auto;">
        <div class="cardSize" style="flex: 1 0 auto; display: flex;">
            <div class="cardPadding" style="display: flex;">
                <div class="card">
                    <div class="cardText">
                        <h3>Protokollierung und Überwachung</h3>
                        <p>Protokollierung und Überwachung sind aufgrund der verteilten Struktur von Microservices-Architekturen besonders wichtig.</p>
                    </div>
                </div>
            </div>
        </div>
    </a>
</li>
<li style="display: flex; flex-direction: column;">
    <a href="./ci-cd.md" style="display: flex; flex-direction: column; flex: 1 0 auto;">
        <div class="cardSize" style="flex: 1 0 auto; display: flex;">
            <div class="cardPadding" style="display: flex;">
                <div class="card">
                    <div class="cardText">
                        <h3>Continuous Integration und Continuous Deployment</h3>
                        <p>Continuous Integration und Continuous Delivery (CI/CD) sind für eine erfolgreiche Verwendung von Microservices unverzichtbar.</p>
                    </div>
                </div>
            </div>
        </div>
    </a>
</li>
</ul>
