---
title: Migrieren einer Web-App zu einer API-basierten Architektur
titleSuffix: Azure Example Scenarios
description: Modernisieren Sie eine ältere Webanwendung mithilfe von Azure API Management.
author: begim
ms.date: 09/13/2018
ms.topic: example-scenario
ms.service: architecture-center
ms.subservice: example-scenario
ms.custom: fasttrack
social_image_url: /azure/architecture/example-scenario/apps/media/architecture-apim-api-scenario.png
ms.openlocfilehash: cf3d4b7ed7fce04e6688f68e382caeec78abd100
ms.sourcegitcommit: 3b15d65e7c35a19506e562c444343f8467b6a073
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/25/2019
ms.locfileid: "54907961"
---
# <a name="migrating-a-legacy-web-application-to-an-api-based-architecture-on-azure"></a>Migrieren einer älteren Webanwendung zu einer API-basierten Architektur in Azure

Ein E-Commerce-Unternehmen in der Reisebranche modernisiert seinen bestehenden browserbasierten Softwarestapel. Der vorhandene Stapel ist größtenteils monolithisch, das Unternehmen hat jedoch einige [SOAP-basierte HTTP-Dienste][soap], die aus einem aktuellen Projekt stammen. Das Unternehmen erwägt, zusätzliche Einnahmequellen zu erschließen, um Teile des von ihm entwickelten internen geistigen Eigentums gewinnbringend zu nutzen.

Zu den Zielen für das Projekt zählen der Abbau technischer Schulden, die Verbesserung der laufenden Wartung und die Beschleunigung der Featureentwicklung mit weniger Regressionsfehlern. Bei dem Projekt wird zur Vermeidung von Risiken ein iterativer Prozess eingesetzt, und einige Schritte werden parallel ausgeführt:

- Das Entwicklungsteam wird das Anwendungs-Back-End modernisieren, das aus auf VMs gehosteten relationalen Datenbanken besteht.
- Das interne Entwicklungsteam wird neue Geschäftsfunktionen entwickeln, die über neue HTTP-APIs verfügbar gemacht werden.
- Ein externes Entwicklungsteam wird eine neue browserbasierte Benutzeroberfläche erstellen, die in Azure gehostet wird.

Neue Anwendungsfeatures werden in mehreren Phasen bereitgestellt. Diese Features werden die vorhandene (lokal gehostete) browserbasierte Client/Server-Benutzeroberfläche, die gegenwärtig für das E-Commerce-Geschäft des Unternehmens genutzt wird, schrittweise ersetzen.

Das Managementteam möchte unnötige Modernisierungen vermeiden. Außerdem möchte es die Kontrolle über den Umfang und die Kosten des Projekts behalten. Aus diesem Grund hat sich das Unternehmen entschieden, die vorhandenen SOAP-HTTP-Dienste beizubehalten. Zudem sollen Änderungen an der vorhandenen Benutzeroberfläche auf ein Minimum beschränkt werden. Mit [Azure API Management (APIM)][apim] können viele der Anforderungen und Auflagen des Projekts erfüllt werden.

## <a name="architecture"></a>Architecture

![Architekturdiagramm][architecture]

Die neue Benutzeroberfläche wird als PaaS-Anwendung (Platform-as-a-Service) in Azure gehostet und ist sowohl von vorhandenen als auch neuen HTTP-APIs abhängig. Diese APIs werden einen besseren Satz von Schnittstellen enthalten, die die Leistung verbessern, die Integration vereinfachen und zukünftige Erweiterbarkeit unterstützen.

### <a name="components-and-security"></a>Komponenten und Sicherheit

1. Die vorhandene lokale Webanwendung wird die vorhandenen lokalen Webdienste weiterhin direkt nutzen.
2. Aufrufe von der vorhandenen Web-App an die vorhandenen HTTP-Dienste bleiben unverändert. Diese Aufrufe erfolgen intern im Unternehmensnetzwerk.
3. Eingehende Aufrufe werden von Azure an die vorhandenen internen Dienste gesendet:
    - Das Sicherheitsteam verwendet den [sicheren Transport (HTTPS/SSL)][apim-ssl], um zuzulassen, dass Datenverkehr von der APIM-Instanz durch die Unternehmensfirewall zu den vorhandenen lokalen Diensten geleitet wird.
    - Das operative Team lässt eingehende Aufrufe der Dienste nur von der APIM-Instanz zu. Innerhalb der Unternehmensnetzwerkumgebung wird die [IP-Adresse der APIM-Instanz auf die Whitelist gesetzt][apim-whitelist-ip], um diese Anforderung zu erfüllen.
    - In der Anforderungspipeline der lokalen HTTP-Dienste wird ein neues Modul konfiguriert (um **nur** externe Verbindungen zu verarbeiten), das [ein von APIM bereitgestelltes Zertifikat][apim-mutualcert-auth] überprüft.
4. Für die neue API gilt Folgendes:
    - Die neue API wird nur über die APIM-Instanz verfügbar gemacht, die die API-Fassade bereitstellt. Auf die neue API wird nicht direkt zugegriffen.
    - Die neue API wird als eine [Azure-PaaS-Web-API-App][azure-api-apps] entwickelt und veröffentlicht.
    - Die neue API wird auf die Whitelist gesetzt (über [Web-App-Einstellungen][azure-appservice-ip-restrict]), um nur die [virtuelle IP (VIP) der APIM-Instanz][apim-faq-vip] zu akzeptieren.
    - Die neue API wird mit aktiviertem sicheren Transport/SSL-Protokoll in Azure-Web-Apps gehostet.
    - Für die neue API ist die Autorisierung aktiviert. Diese wird von [Azure App Service][azure-appservice-auth] mithilfe von Azure Active Directory und OAuth2 bereitgestellt.
5. Die neue browserbasierte Webanwendung ist für die vorhandene HTTP-API **und** die neue API von der Azure API Management-Instanz abhängig.

Die APIM-Instanz wird so konfiguriert, dass sie die älteren HTTP-Dienste einem neuen API-Vertrag zuordnet. Der neuen Webbenutzeroberfläche ist die Integration einer Gruppe von älteren Diensten/APIs und neuen APIs daher nicht bekannt. Das Projektteam wird in der Zukunft schrittweise Funktionen zu den neuen APIs portieren und die ursprünglichen Dienste außer Betrieb setzen. Diese Änderungen werden in der APIM-Konfiguration vorgenommen, sodass die Front-End-Benutzeroberfläche davon nicht betroffen ist und Neuentwicklungen vermieden werden.

### <a name="alternatives"></a>Alternativen

- Wenn die Organisation eine Migration ihrer vollständigen Infrastruktur (einschließlich der VMs, die ältere Anwendungen hosten) zu Azure planen würde, wäre APIM dennoch eine gute Option, da der Dienst als Fassade für alle adressierbaren HTTP-Endpunkte fungieren kann.
- Wenn sich der Kunde dafür entschieden hätte, die vorhandenen Endpunkte privat zu halten und nicht öffentlich verfügbar zu machen, könnte die API Management-Instanz mit einem [virtuellen Azure-Netzwerk (VNET)][azure-vnet] verknüpft werden:
  - In einem [Azure-Lift & Shift-Szenario][azure-vm-lift-shift], das direkt mit dem bereitgestellten virtuellen Azure-Netzwerk verknüpft ist, könnte der Kunde den Back-End-Dienst direkt über private IP-Adressen adressieren.
  - Im lokalen Szenario könnte die API Management-Instanz eine private Verbindung über [ein Azure-VPN-Gateway und eine Site-to-Site-IPSec-VPN-Verbindung][azure-vpn] oder [ ExpressRoute][azure-er] mit dem internen Dienst herstellen, wodurch ein [Hybridszenario mit Azure und einer lokalen Umgebung][azure-hybrid] entstehen würde.
- Die API Management-Instanz kann privat gehalten werden, indem sie im internen Modus bereitgestellt wird. Die Bereitstellung könnte dann mit einer [Azure Application Gateway][azure-appgw]-Instanz verwendet werden, um den öffentlichen Zugriff für einige APIs zu ermöglichen, während andere APIs weiterhin nur intern zugänglich sind. Weitere Informationen finden Sie unter [Verbinden von API Management mit einem VNET im internen Modus][apim-vnet-internal].

> [!NOTE]
> Allgemeine Informationen zum Verbinden von API Management mit einem VNET finden Sie [hier][apim-vnet].

### <a name="availability-and-scalability"></a>Verfügbarkeit und Skalierbarkeit

- Azure API Management kann [horizontal hochskaliert][apim-scaleout] werden, indem Sie einen Tarif auswählen und anschließend Einheiten hinzufügen.
- Die Skalierung erfolgt auch durch [automatische Skalierung][apim-autoscale].
- Die [Bereitstellung in mehreren Regionen][apim-multi-regions] ermöglicht die Verwendung von Failoveroptionen und ist im [Premium-Tarif][apim-pricing] verfügbar.
- Erwägen Sie die [Integration in Azure Application Insights][azure-apim-ai]. Dadurch stehen Ihnen über [Azure Monitor][azure-mon] auch Metriken für die Überwachung zur Verfügung.

## <a name="deploy-the-scenario"></a>Bereitstellen des Szenarios

Der erste Schritt ist das [Erstellen einer Azure API Management-Instanz im Portal][apim-create].

Alternativ können Sie eine der vorhandenen Azure Resource Manager-[Schnellstartvorlagen][azure-quickstart-templates-apim] auswählen, die für Ihren spezifischen Anwendungsfall geeignet ist.

## <a name="pricing"></a>Preise

API Management ist in vier Tarifen erhältlich: Developer, Basic, Standard und Premium. Ausführliche Informationen zu den Unterschieden zwischen den einzelnen Tarifen finden Sie in der [API Management-Preisübersicht][apim-pricing].

Kunden können API Management skalieren, indem sie Einheiten hinzufügen und entfernen. Die Kapazität jeder Einheit ist vom Tarif abhängig.

> [!NOTE]
> Der Developer-Tarif kann zur Evaluierung der API Management-Funktionen verwendet werden. Der Developer-Tarif sollte nicht für die Produktion verwendet werden.

Im [Azure-Preisrechner][pricing-calculator] können Sie die voraussichtlichen Kosten für Ihre speziellen Bereitstellungsanforderungen ermitteln, indem Sie die Anzahl von Skalierungseinheiten und App Service-Instanzen ändern.

## <a name="related-resources"></a>Zugehörige Ressourcen

Zu Azure API Management stehen Ihnen eine umfangreiche [Dokumentation und Referenzartikel][apim] zur Verfügung.

<!-- links -->

[architecture]: ./media/architecture-apim-api-scenario.png
[apim-create]: /azure/api-management/get-started-create-service-instance
[apim-git]: /azure/api-management/api-management-configuration-repository-git
[apim-multi-regions]: /azure/api-management/api-management-howto-deploy-multi-region
[apim-autoscale]: /azure/api-management/api-management-howto-autoscale
[apim-scaleout]: /azure/api-management/upgrade-and-scale
[azure-apim-ai]: /azure/api-management/api-management-howto-app-insights
[azure-ai]: /azure/application-insights/
[azure-mon]: /azure/monitoring-and-diagnostics/monitoring-overview
[azure-appgw]: /azure/application-gateway/application-gateway-introduction
[apim-vnet-internal]: /azure/api-management/api-management-howto-integrate-internal-vnet-appgateway
[apim-vnet]: /azure/api-management/api-management-using-with-vnet
[azure-hybrid]: /azure/architecture/reference-architectures/hybrid-networking/
[azure-er]: /azure/expressroute/expressroute-introduction
[azure-vpn]: /azure/vpn-gateway/vpn-gateway-howto-site-to-site-resource-manager-portal
[azure-vnet]: /azure/virtual-network/virtual-networks-overview
[azure-appservice-auth]: /azure/app-service/app-service-authentication-overview#identity-providers
[apim-faq-vip]: /azure/api-management/api-management-faq#is-the-api-management-gateway-ip-address-constant-can-i-use-it-in-firewall-rules
[azure-appservice-ip-restrict]: /azure/app-service/app-service-ip-restrictions
[azure-api-apps]: /azure/app-service/
[apim-ssl]: /azure/api-management/api-management-howto-manage-protocols-ciphers
[apim-mutualcert-auth]: /azure/api-management/api-management-howto-mutual-certificates
[apim-whitelist-ip]: /azure/api-management/api-management-faq#is-the-api-management-gateway-ip-address-constant-can-i-use-it-in-firewall-rules
[anti-corruption-layer-pattern]: /azure/architecture/patterns/anti-corruption-layer
[apim]: /azure/api-management/api-management-key-concepts
[apim-api-design-guidance]: /azure/architecture/best-practices/api-design
[visualstudio-youtube-solid-design]: https://youtu.be/agkWYPUcLpg
[azure-vm-lift-shift]: https://azure.microsoft.com/resources/azure-virtual-datacenter-lift-and-shift-guide/
[standard-pricing-calc]: https://azure.com/e/
[premium-pricing-calc]: https://azure.com/e/
[apim-pricing]: https://azure.microsoft.com/pricing/details/api-management/
[azure-quickstart-templates-apim]: https://azure.microsoft.com/resources/templates/?term=API+Management&pageNumber=1
[soap]: https://en.wikipedia.org/wiki/SOAP
[pricing-calculator]: https://azure.com/e/0e916a861fac464db61342d378cc0bd6
