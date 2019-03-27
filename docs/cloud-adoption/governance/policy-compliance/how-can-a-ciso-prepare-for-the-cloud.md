---
title: 'Framework für die Cloudeinführung (Cloud Adoption Framework, CAF): CISO-Bereitschaft'
description: Wie kann sich ein CISO auf die Cloud vorbereiten?
author: BrianBlanchard
ms.date: 10/03/2018
ms.openlocfilehash: a4535163990797decdacdacdcb6a33f0118366e9
ms.sourcegitcommit: c053e6edb429299a0ad9b327888d596c48859d4a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/20/2019
ms.locfileid: "58238343"
---
# <a name="ciso-cloud-readiness-guide"></a>Leitfaden für die CISO-Cloudbereitschaft

Empfehlungen von Microsoft wie beispielsweise das Azure Cloud Adoption Framework (CAF) sind nicht dafür konzipiert, die einzigartigen Sicherheitsbedingungen von Tausenden von Unternehmen zu ermitteln, die von dieser Dokumentation unterstützt werden, oder als Anleitung für diese Bedingungen zu dienen. Bei der Migration in die Cloud wird die Rolle des Chief Information Security Officer bzw. Chief Information Security Office (CISO) nicht durch Cloudtechnologien verdrängt. Ganz im Gegenteil: Der CISO und das CISO-Team sind noch besser und enger integriert. Für diese Anleitung wird vorausgesetzt, dass Sie mit CISO-Prozessen vertraut sind und diese modernisieren möchten, um eine Cloudtransformation zu ermöglichen.

Die Einführung der Cloud ermöglicht die Verwendung von Diensten, die in herkömmlichen IT-Umgebungen nur selten berücksichtigt wurden. Self-Service- oder automatisierte Bereitstellungen werden häufig von für die Anwendungsentwicklung zuständigen oder anderen IT-Teams ausgeführt, die traditionell nicht an der Produktionsbereitstellung beteiligt sind. In einigen Organisationen verfügen Geschäftsbenutzer ebenfalls über Self-Service-Funktionen. Dadurch können neue Sicherheitsanforderungen entstehen, die in lokalen Bereitstellungen nicht benötigt wurden. Die zentralisierte Sicherheit stellt eine größere Herausforderung dar, und häufig sind IT-Abteilung und Geschäftsbereiche gemeinsam dafür verantwortlich. Dieser Artikel kann einen CISO bei der Vorbereitung auf diesen Ansatz und der Umsetzung einer inkrementellen Governance unterstützen.

## <a name="how-can-the-ciso-prepare-for-the-cloud"></a>Wie kann sich ein CISO auf die Cloud vorbereiten?

Wie die meisten Richtlinien zeigen auch Sicherheits- und Governancerichtlinien die Tendenz, organisch zu wachsen. Wenn Sicherheitsincidents auftreten, formen sie die Richtlinie, um die Benutzer zu informieren und die Wahrscheinlichkeit einer Wiederholung zu reduzieren. Dieser Ansatz ist zwar natürlich, führt aber zur Überfrachtung von Richtlinien und zu technischen Abhängigkeiten. Cloud Transformation Journeys bieten eine einzigartige Gelegenheit, Richtlinien zu modernisieren und zurückzusetzen. Bei der Vorbereitung auf eine Transformation Journey können CISOs sofortigen und messbaren Nutzen schaffen, indem sie sich aktiv an einer [Richtlinienüberprüfung](./what-is-a-cloud-policy-review.md) beteiligen.

Bei einer solchen Überprüfung besteht die Rolle des CISO darin, für ein sicheres Gleichgewicht zwischen den Einschränkungen vorhandener Richtlinien/Compliancefeatures und der verbesserten Sicherheitslage von Cloudanbietern zu sorgen. Der Fortschritt lässt sich auf unterschiedliche Weise messen: Ein häufiges Maß ist die Anzahl von Sicherheitsrichtlinien, die sicher an den Cloudanbieter ausgelagert werden können.

**Übertragen von Sicherheitsrisiken**: Wenn Dienste in IaaS-Hostingmodelle (Infrastructure-as-a-Service) verlagert werden, kann aus geschäftlicher Sicht von einem geringeren direkten Risiko in Bezug auf die Hardwarebereitstellung ausgegangen werden. Das Risiko ist nicht verschwunden, sondern wird an den Cloudanbieter übertragen. Wenn das Verfahren eines Cloudanbieters für die Hardwarebereitstellung das gleiche Maß an Risikominderung in einem sicheren, wiederholbaren Prozess bereitstellt, wird das Risiko der Hardwarebereitstellung aus der Unternehmensrichtlinie entfernt. Sie kann jetzt durch eine neue Richtlinie zur Überprüfung dieser Prozesse ersetzt werden, aber das Ausführungsrisiko wird umverteilt, wodurch das Sicherheitsrisiko insgesamt sinkt.

Wenn Lösungen PaaS- oder SaaS-Modelle (Platform-as-a-Service bzw. Software-as-a-Service) nutzen, können weitere Risiken minimiert, übertragen und ersetzt werden. Wenn das Risiko sicher an einen Cloudanbieter verlagert wird, können auch die Kosten für Ausführung, Überwachung und Durchsetzung von Sicherheitsrichtlinien oder anderen Compliancerichtlinien sicher reduziert werden.

**Wachstumsorientierte Mentalität**: Veränderungen können beängstigend sein – sowohl für die Personen, die sich um die geschäftliche Seite kümmern, als auch für diejenigen, die für die technische Implementierung zuständig sind. Wenn ein CISO die Mentalität in einer Organisation verändern kann, haben wir festgestellt, dass diese natürlichen Ängste durch ein erhöhtes Interesse an Sicherheit und Richtlinieneinhaltung ersetzt werden. Indem Teams mit einer wachstumsorientierten Mentalität an eine [Richtlinienüberprüfung](./what-is-a-cloud-policy-review.md), eine Transformation Journey oder eine einfache Überprüfung der Implementierung herangehen, können sie schnell Erfolge erzielen, ohne dabei auf ein faires und verwaltbares Risikoprofil verzichten zu müssen.

## <a name="resources-for-the-chief-information-security-officer"></a>Ressourcen für den Chief Information Security Officer

Kenntnisse der Cloud sind von grundlegender Bedeutung für eine [Richtlinienüberprüfung](./what-is-a-cloud-policy-review.md) mit wachstumsorientierter Mentalität. Die folgenden Ressourcen unterstützen CISOs dabei, den Sicherheitsstatus der Microsoft Azure-Plattform besser zu verstehen.

Ressourcen zur Sicherheitsplattform:

* [Sicherheitsentwicklungszyklus, interne Überwachungen](https://www.microsoft.com/sdl/)
* [Obligatorische Sicherheitsschulung, Hintergrundüberprüfungen](https://downloads.cloudsecurityalliance.org/star/self-assessment/StandardResponsetoRequestforInformationWindowsAzureSecurityPrivacy.docx)
* [Penetrationstests, Angriffserkennung, DDoS, Überwachungen und Protokollierung](https://www.microsoft.com/trustcenter/Security/AuditingAndLogging)
* [Hochmodernes Rechenzentrum](https://www.microsoft.com/cloud-platform/global-datacenters), physische Sicherheit, [sicheres Netzwerk](/azure/security/security-network-overview)
* [Microsoft Azure Security Response in the Cloud](https://aka.ms/SecurityResponsePaper) (Sicherheitsreaktion von Microsoft Azure in der Cloud)

Datenschutz und Kontrolle:

* [Ständiges Verwalten Ihrer Daten](https://www.microsoft.com/trustcenter/Privacy/You-own-your-data)
* [Kontrolle des Datenspeicherorts](https://www.microsoft.com/trustcenter/Privacy/Where-your-data-is-located)
* [Bereitstellen des Datenzugriff zu Ihren Bedingungen](https://www.microsoft.com/trustcenter/Privacy/Who-can-access-your-data-and-on-what-terms)
* [Antworten für Justizbehörden](https://www.microsoft.com/trustcenter/Privacy/Responding-to-govt-agency-requests-for-customer-data)
* [Strenge Datenschutzstandards](https://www.microsoft.com/TrustCenter/Privacy/We-set-and-adhere-to-stringent-standards)

Compliance:

* [Trust Center](https://www.microsoft.com/trustcenter/default.aspx)
* [Hub für allgemeine Kontrollen](https://www.microsoft.com/trustcenter/Common-Controls-Hub)
* [Checkliste zur Kaufprüfung von Clouddiensten](https://www.microsoft.com/trustcenter/Compliance/Due-Diligence-Checklist)
* [Compliance nach Dienst, Standort und Branche](https://www.microsoft.com/trustcenter/Compliance/default.aspx)

Transparenz:

* [Wie Microsoft Kundendaten in Azure-Diensten schützt](https://www.microsoft.com/trustcenter/Transparency/default.aspx)
* [Wie Microsoft den Datenspeicherort in Azure-Diensten verwaltet](https://azuredatacentermap.azurewebsites.net/)
* [Wer kann bei Microsoft unter welchen Bedingungen auf Ihre Daten zugreifen?](https://www.microsoft.com/trustcenter/Privacy/Who-can-access-your-data-and-on-what-terms)
* [Wie Microsoft Kundendaten in Azure-Diensten schützt](https://www.microsoft.com/trustcenter/Transparency/default.aspx)
* [Überprüfen der Zertifizierung für Azure-Dienste, Transparenzhub](https://www.microsoft.com/trustcenter/Compliance/default.aspx)

## <a name="next-steps"></a>Nächste Schritte

Der erste Schritt bei jeder Governancestrategie ist eine [Richtlinienüberprüfung](./what-is-a-cloud-policy-review.md). [Richtlinie und Compliance](./overview.md) ist ein nützlicher Leitfaden für Ihre Richtlinienüberprüfung.

> [!div class="nextstepaction"]
> [Vorbereiten auf eine Richtlinienüberprüfung](./what-is-a-cloud-policy-review.md)