---
title: 'Framework für die Cloudeinführung (Cloud Adoption Framework, CAF): Leitfaden zur Entscheidungsfindung für die Identitätsverwaltung'
titleSuffix: Microsoft Cloud Adoption Framework for Azure
ms.service: architecture-center
ms.subservice: enterprise-cloud-adoption
ms.custom: governance
ms.date: 02/11/2019
description: Erfahren Sie mehr über die Identitätsverwaltung als wichtigen Aspekt bei Azure-Migrationen.
author: rotycenh
ms.openlocfilehash: 511e27c7d63968417a7eb47764d2e7768496d34f
ms.sourcegitcommit: c053e6edb429299a0ad9b327888d596c48859d4a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/20/2019
ms.locfileid: "58242771"
---
# <a name="identity-decision-guide"></a>Leitfaden zur Entscheidungsfindung für die Identitätsverwaltung

In allen Umgebungen, seien es lokale, Hybrid- oder reine Cloudumgebungen, muss die IT-Abteilung genau bestimmen, welche Administratoren, Benutzer und Gruppen Zugriff auf Ressourcen haben. Dienste für die Identitäts- und Zugriffsverwaltung (Identity and Access Management, IAM) ermöglichen Ihnen das Verwalten der Zugriffssteuerung in der Cloud.

![Abbildung der Identitätsoptionen mit zunehmender Komplexität entsprechend den nachstehenden weiterführenden Links](../../_images/discovery-guides/discovery-guide-identity.png)

Wechseln Sie zu: [Ermitteln der Anforderung an die Identitätsintegration](#determine-identity-integration-requirements) | [Cloudnativ](#cloud-baseline) | [Verzeichnissynchronisierung](#directory-synchronization) | [In der Cloud gehostete Domänendienste](#cloud-hosted-domain-services) | [Active Directory-Verbunddienste (AD FS)](#active-directory-federation-services) | [Sich weiterentwickelnde Identitätsintegration](#evolving-identity-integration) | [Weitere Informationen](#learn-more)

Es gibt in einer Cloudumgebung mehrere Möglichkeiten der Identitätsverwaltung, die sich hinsichtlich Kosten und Komplexität unterscheiden. Ein wesentlicher Faktor bei der Strukturierung Ihrer cloudbasierten Identitätsverwaltungsdienste ist der Grad der Integration in Ihre bestehende lokale Identitätsverwaltungsinfrastruktur.

In Azure bietet Azure Active Directory (Azure AD) eine grundlegende Ebene der Zugriffssteuerung und Identitätsverwaltung für Cloudressourcen. Wenn die Active Directory-Infrastruktur (AD) Ihrer Organisation jedoch eine komplexe Gesamtstruktur oder angepasste Organisationseinheiten aufweist, können Ihre cloudbasierten Workloads eine Verzeichnissynchronisierung mit Azure AD erfordern, um einen einheitlichen Satz von Identitäten, Gruppen und Rollen zwischen der lokalen und der Cloudumgebung zu gewährleisten. Darüber hinaus kann die Unterstützung von Anwendungen, die von veralteten Authentifizierungsmechanismen abhängen, die Bereitstellung von Active Directory Domain Services (AD DS) in der Cloud erfordern.

## <a name="determine-identity-integration-requirements"></a>Bestimmen der Anforderungen an die Identitätsintegration

| Frage | Reine Cloudlösung | Verzeichnissynchronisierung | In der Cloud gehostete Domänendienste | Active Directory-Verbunddienste (AD FS) |
|------|------|------|------|------|
| Fehlt Ihnen derzeit ein lokaler Verzeichnisdienst? | Ja | Nein  | Nein  | Nein  |
| Benötigen Ihre Workloads einen gemeinsamen Satz von Benutzern und Gruppen zwischen der Cloud und der lokalen Umgebung? | Nein  | Ja | Nein  | Nein  |
| Sind Ihre Workloads von veralteten Authentifizierungsmechanismen wie Kerberos oder NTLM abhängig? | Nein  | Nein  | Ja | Ja |
| Wird einmaliges Anmelden über mehrere Identitätsanbieter benötigt? | Nein  | Nein  | Nein  | Ja |

Im Rahmen der Planung Ihrer Migration zu Azure müssen Sie entscheiden, wie Sie Ihre bestehenden Identitätsverwaltungs- und Cloudidentitätsdienste am besten integrieren. Es folgen gängige Integrationsszenarien.

### <a name="cloud-baseline"></a>Reine Cloudlösung

Azure AD ist das ein natives System zur Identitäts- und Zugriffsverwaltung (IAM), über das Benutzer und Gruppen auf der Azure-Plattform Zugriff auf Verwaltungsfunktionen erhalten. Wenn es in Ihrem Unternehmen keine nennenswerte lokale Identitätsverwaltungslösung gibt und Sie planen, Workloads so zu migrieren, dass sie mit cloudbasierten Authentifizierungsmechanismen kompatibel sind, sollten Sie mit der Entwicklung Ihrer Identitätsinfrastruktur mit Azure AD als Basis beginnen.

**Annahmen für eine reine Cloudlösung**. Für den Einsatz einer rein cloudbasierten Identitätsinfrastruktur wird Folgendes angenommen:

- Ihre cloudbasierten Ressourcen weisen keine Abhängigkeiten von lokalen Verzeichnisdiensten oder Active Directory-Servern auf, oder Workloads können so geändert werden, dass diese Abhängigkeiten entfallen.
- Die zu migrierenden Anwendungs- oder Dienstworkloads unterstützen entweder Authentifizierungsmechanismen, die mit Azure AD kompatibel sind, oder können problemlos so geändert werden, dass diese unterstützt werden. Azure AD greift auf internetfähige Authentifizierungsmechanismen wie SAML, OAuth und OpenID Connect zurück. Bestehende Workloads, die von veralteten Authentifizierungsmethoden mit Protokollen wie Kerberos oder NTLM abhängen, müssen möglicherweise vor der Migration in die Cloud gemäß dem Cloudbasismuster angepasst werden.

> [!TIP]
> Durch die vollständige Migration Ihrer Identitätsdienste zu Azure AD entfällt die Notwendigkeit, Ihre eigene Identitätsinfrastruktur zu pflegen, wodurch Ihre IT-Verwaltung erheblich vereinfacht wird.
>
> Azure AD ist jedoch kein vollständiger Ersatz für eine herkömmliche lokale Active Directory-Infrastruktur. Verzeichnisfunktionen wie veraltete Authentifizierungsmethoden, Computerverwaltung oder Gruppenrichtlinien sind möglicherweise nicht ohne Bereitstellen zusätzlicher Tools oder Dienste in der Cloud verfügbar.
>
> Wenn Sie Ihre lokalen Identitäten oder Domänendienste in Ihre Cloudbereitstellungen integrieren müssen, finden Sie weitere Informationen in den Mustern zur Verzeichnissynchronisierung und zu den von der Cloud gehosteten Domänendiensten, die im Folgenden erläutert werden.

### <a name="directory-synchronization"></a>Verzeichnissynchronisierung

Für Organisationen mit einer bestehenden lokalen Active Directory-Infrastruktur ist die Verzeichnissynchronisierung oft die beste Lösung, um die bestehende Benutzer- und Zugriffsverwaltung aufrechtzuerhalten und gleichzeitig die erforderlichen IAM-Funktionen für die Verwaltung von Cloudressourcen bereitzustellen. Bei diesem Prozess werden Verzeichnisinformationen kontinuierlich zwischen Azure AD und lokalen Verzeichnisdiensten repliziert, wodurch gemeinsame Anmeldeinformationen für Benutzer und ein einheitliches Identitäts-, Rollen- und Berechtigungssystem in der gesamten Organisation ermöglicht werden.

Hinweis: Organisationen, die Office 365 eingeführt haben, haben möglicherweise bereits die [Verzeichnissynchronisierung](/office365/enterprise/set-up-directory-synchronization) zwischen ihrer lokalen Active Directory-Infrastruktur und Azure Active Directory eingerichtet.

**Annahmen zur Verzeichnissynchronisierung**. Für den Einsatz einer synchronisierten Identitätslösung wird Folgendes angenommen:

- Sie benötigen in Ihrer Cloud- und lokalen IT-Infrastruktur einen gemeinsamen Satz von Benutzerkonten und -gruppen.
- Ihre lokalen Identitätsdienste unterstützen die Replikation mit Azure AD.

> [!TIP]
> Alle cloudbasierten Workloads, die von veralteten Authentifizierungsmechanismen abhängen, welche von lokalen Servern mit Active Directory und nicht von Azure AD unterstützt werden, erfordern weiterhin entweder eine Verbindung mit lokalen Domänendiensten oder virtuellen Servern in der Cloudumgebung, die diese Dienste bereitstellen. Bei Nutzung lokaler Identitätsdienste entstehen auch Abhängigkeiten von der Konnektivität zwischen der Cloud und lokalen Netzwerken.

### <a name="cloud-hosted-domain-services"></a>In der Cloud gehostete Domänendienste

Angenommen, Sie haben Workloads, die von einer anspruchsbasierten Authentifizierung mit älteren Protokollen wie Kerberos oder NTLM abhängen, und diese Workloads nicht so angepasst werden können, dass sie moderne Authentifizierungsprotokolle wie SAML oder OAuth und OpenID Connect unterstützen. Dann müssen Sie möglicherweise einige Ihrer Domänendienste als Teil Ihrer Cloudbereitstellung in die Cloud migrieren.

Dieses Muster erfordert die Bereitstellung virtueller Computer mit ausgeführtem Active Directory in Ihren cloudbasierten virtuellen Netzwerken, um Active Directory Domain Services (AD DS) für Ressourcen in der Cloud bereitzustellen. Alle bestehenden Anwendungen und Dienste, die in Ihr Cloudnetzwerk migrieren, sollten in der Lage sein, nach geringfügigen Änderungen diese von der Cloud gehosteten Verzeichnisserver zu nutzen.

Es ist wahrscheinlich, dass Ihre bestehenden Verzeichnisse und Domänendienste weiterhin in Ihrer lokalen Umgebung verwendet werden. In diesem Szenario wird empfohlen, dass Sie auch die Verzeichnissynchronisierung verwenden, um einen gemeinsamen Satz von Benutzern und Rollen sowohl in der Cloud- als auch in der lokalen Umgebung bereitzustellen.

**Annahmen für in der Cloud gehostete Domänendienste**. Für eine Verzeichnismigration wird Folgendes angenommen:

- Ihre Workloads hängen von der anspruchsbasierten Authentifizierung mit Protokollen wie Kerberos oder NTLM ab.
- Die virtuellen Computer Ihrer Workloads müssen für die Verwaltung oder Anwendung von Active Directory-Gruppenrichtlinien der Domäne beitreten.

> [!TIP]
> Während eine Verzeichnismigration in Verbindung mit in der Cloud gehosteten Domänendiensten eine große Flexibilität bei der Migration bestehender Workloads bietet, erhöht das Hosting virtueller Computer in Ihrem virtuellen Cloudnetzwerk zur Bereitstellung dieser Dienste die Komplexität Ihrer IT-Verwaltungsaufgaben. Mit zunehmender Erfahrung mit der Migration in die Cloud sollten Sie den langfristigen Wartungsbedarf für das Hosting dieser Server untersuchen. Prüfen Sie, ob die Anpassung bestehender Workloads aus Gründen der Kompatibilität mit Cloudidentitätsanbietern wie Azure Active Directory den Bedarf an diesen in der Cloud gehosteten Servern verringern kann.

### <a name="active-directory-federation-services"></a>Active Directory-Verbunddienste (AD FS)

Über den Identitätsverbund werden Vertrauensbeziehungen zwischen mehreren Identitätsverwaltungssystemen aufgebaut, um gemeinsame Authentifizierungs- und Autorisierungsfunktionen zu ermöglichen. Sie können dann Funktionen für einmaliges Anmelden in mehreren Domänen innerhalb Ihrer Organisation oder Identitätssysteme unterstützen, die von Ihren Kunden oder Geschäftspartnern verwaltet werden.

Azure AD unterstützt den Verbund lokaler Active Directory-Domänen mithilfe von [Active Directory-Verbunddienste (AD FS)](/azure/active-directory/hybrid/how-to-connect-fed-whatis). In der Referenzarchitektur [Erweiterung von AD FS auf Azure](../../../reference-architectures/identity/adfs.md) sehen Sie, wie dies in Azure erfolgen kann.

## <a name="evolving-identity-integration"></a>Weiterentwicklung der Identitätsintegration

Die Identitätsintegration ist ein iterativer Vorgang. Möglicherweise möchten Sie für eine erste Bereitstellung mit einer cloudnativen Lösung mit einer kleinen Anzahl von Benutzern und entsprechenden Rollen beginnen. Wenn Sie mehr Erfahrung mit der Migration haben, erwägen Sie den Einsatz eines Verbundmodells oder eine vollständige Verzeichnismigration Ihrer lokalen Identitätsdienste in die Cloud. Überprüfen Sie Ihre Identitätsstrategie bei jeder Iteration Ihres Migrationsprozesses.

## <a name="learn-more"></a>Weitere Informationen

Nachstehend finden Sie weitere Informationen zu Identitätsdiensten auf der Azure-Plattform.

- [Azure AD](https://azure.microsoft.com/services/active-directory). Azure AD bietet cloudbasierte Identitätsdienste. Es ermöglicht Ihnen die Verwaltung des Zugriffs auf Ihre Azure-Ressourcen und das Steuern der Identitätsverwaltung, Geräteregistrierung, Benutzerbereitstellung, Anwendungszugriffssteuerung und Datensicherung.
- [Azure AD Connect](/azure/active-directory/hybrid/whatis-hybrid-identity): Mit dem Tool Azure AD Connect können Sie Azure AD-Instanzen mit Ihren bestehenden Identitätsverwaltungslösungen verbinden und so Ihr bestehendes Verzeichnis in der Cloud synchronisieren.
- [Rollenbasierte Zugriffssteuerung](/azure/role-based-access-control/overview) (RBAC). Azure AD bietet RBAC für die effiziente und sichere Verwaltung des Zugriffs auf Ressourcen auf Verwaltungsebene. Aufträge und Aufgaben sind in Rollen organisiert und Benutzer diesen Rollen zugewiesen. Mit RBAC können Sie steuern, wer Zugriff auf eine Ressource hat und welche Aktionen ein Benutzer auf diese Ressource anwenden kann.
- [Azure AD Privileged Identity Management](/azure/active-directory/privileged-identity-management/pim-configure) (PIM). PIM reduziert die Offenlegungszeit von Ressourcenzugriffsrechten und verschafft Ihnen durch Berichte und Warnungen einen besseren Überblick über deren Nutzung. Hierfür werden Benutzern Beschränkungen auferlegt, indem ihnen Berechtigungen nur gemäß des Just-in-Time-Prinzips (JIT) zugeteilt oder nur für eine kurze Dauer zugewiesen und danach automatisch widerrufen werden.
- [Integrieren lokaler Active Directory-Domänen in Azure Active Directory](../../../reference-architectures/identity/azure-ad.md). Diese Referenzarchitektur bietet ein Beispiel für die Verzeichnissynchronisierung zwischen lokalen Active Directory-Domänen und Azure AD.
- [Erweitern von Active Directory Domain Services (AD DS) auf Azure](../../../reference-architectures/identity/adds-extend-domain.md). Diese Referenzarchitektur bietet ein Beispiel für den Einsatz von AD DS-Servern zur Erweiterung von Domänendiensten auf cloudbasierte Ressourcen.
- [Erweitern von Active Directory Federation Services (AD FS) auf Azure](../../../reference-architectures/identity/adfs.md). In dieser Referenzarchitektur wird Active Directory-Verbunddienste (AD FS) konfiguriert, um Authentifizierung und Autorisierung im Verbund mit Ihrem Azure AD-Verzeichnis durchzuführen.

## <a name="next-steps"></a>Nächste Schritte

Erfahren Sie, wie Sie die Erzwingung von Richtlinien in der Cloud realisieren.

> [!div class="nextstepaction"]
> [Richtlinienerzwingung](../policy-enforcement/overview.md)
