---
title: 'CAF: Tools für Identitätsbaseline in Azure'
titleSuffix: Microsoft Cloud Adoption Framework for Azure
ms.service: architecture-center
ms.subservice: enterprise-cloud-adoption
ms.custom: governance
ms.date: 02/11/2019
description: Tools für Identitätsbaseline in Azure
author: BrianBlanchard
ms.openlocfilehash: 81b0fa9cfee597da98d8b983fb155eac82d97bf8
ms.sourcegitcommit: 273e690c0cfabbc3822089c7d8bc743ef41d2b6e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/08/2019
ms.locfileid: "55901922"
---
# <a name="identity-baseline-tools-in-azure"></a>Tools für Identitätsbaseline in Azure

[Identitätsbaseline](overview.md) ist eine der [fünf Cloud Governance-Disziplinen](../governance-disciplines.md). Diese Disziplin konzentriert sich auf Methoden zur Erstellung von Richtlinien, die die Konsistenz und Kontinuität der Benutzeridentitäten gewährleisten, unabhängig davon, welcher Cloudanbieter die Anwendung oder den Workload hostet.

Die folgenden Tools sind im Ermittlungshandbuch zur Hybrididentität enthalten.

**Active Directory (lokal):** Active Directory ist der im Unternehmen am häufigsten zum Speichern und Validieren von Benutzeranmeldeinformationen verwendete Identitätsanbieter.

**Azure Active Directory:** Eine SaaS-Entsprechung (Software-as-a-Service) zu Active Directory, die im Verbund mit einem lokalen Active Directory verwendet werden kann.

**Active Directory (IaaS):** Eine Instanz der Active Directory-Anwendung, die auf einem virtuellen Computer in Azure ausgeführt wird.

Die Identität ist die Steuerebene für IT-Sicherheit. Die Authentifizierung ist für eine Organisation daher der Zugriffsschutz für die Cloud. Organisationen benötigen eine Steuerebene für die Identität, mit der ihre Sicherheit gestärkt und die Cloud-Apps vor Eindringlingen geschützt werden.

## <a name="cloud-authentication"></a>Cloudauthentifizierung

Die Wahl der richtigen Authentifizierungsmethode ist die erste Hürde für Organisationen, die ihre Apps in die Cloud verschieben möchten.

Wenn Sie diese Methode wählen, übernimmt Azure AD die Anmeldung für Benutzer. In Verbindung mit dem nahtlosen einmaligen Anmelden (SSO) können sich Benutzer bei Cloud-Apps anmelden, ohne ihre Anmeldeinformationen erneut eingeben zu müssen. Bei der Cloudauthentifizierung können Sie zwischen zwei Optionen wählen:

**Azure AD-Kennworthashsynchronisierung**: Dies ist der einfachste Weg, die Authentifizierung für lokale Verzeichnisobjekte in Azure AD zu ermöglichen. Diese Methode kann auch mit jeder beliebigen anderen Methode als Backupfailover-Authentifizierungsmethode verwendet werden, falls Ihr lokaler Server ausfällt.

**Azure AD-Pass-Through-Authentifizierung**: Es wird eine persistente Kennwortüberprüfung für Azure AD-Authentifizierungsdienste bereitgestellt, indem ein Software-Agent verwendet wird, der auf einem oder mehreren lokalen Servern ausgeführt wird.

> [!NOTE]
> Unternehmen mit einer Sicherheitsanforderung zur sofortigen Durchsetzung von lokalen Benutzerkontozuständen, Kennwortrichtlinien und Anmeldezeiten sollten die Verwendung der Pass-Through-Authentifizierungsmethode in Betracht ziehen.

**Verbundauthentifizierung:**

Wenn Sie diese Methode wählen, übergibt Azure AD den Authentifizierungsprozess zur Validierung des Benutzerkennworts an ein separates vertrauenswürdiges Authentifizierungssystem, z.B. an eine lokale Instanz von Active Directory-Verbunddienste (AD FS) oder einen vertrauenswürdigen Drittanbieter von Verbunddiensten.

Der Artikel [Auswählen der richtigen Authentifizierungsmethode für Azure Active Directory](/azure/security/azure-ad-choose-authn) enthält eine Entscheidungsstruktur, die Ihnen bei der Auswahl der besten Lösung für Ihr Unternehmen hilft.

Im Folgenden finden Sie eine Tabelle der nativen Tools, die bei der Verfeinerung der Richtlinien und Prozesse beitragen können, die diese Governancedisziplin unterstützen.

<!-- markdownlint-disable MD033 -->

|Aspekt|Kennworthashsynchronisierung + nahtloses einmaliges Anmelden|Passthrough-Authentifizierung + nahtloses einmaliges Anmelden|Verbund mit AD FS|
|:-----|:-----|:-----|:-----|
|Wo findet Authentifizierung statt?|In der Cloud|In der Cloud nach einem sicheren Kennwortüberprüfungsaustausch mit dem lokalen Authentifizierungs-Agent|Lokal|
|Welche lokalen Serveranforderungen gibt es über das Bereitstellungssystem hinaus: Azure AD Connect?|Keine|Ein Server für jeden zusätzlichen Authentifizierungs-Agent|Mindestens zwei AD FS-Server<br><br>Mindestens zwei WAP-Server im Umkreis-/DMZ-Netzwerk|
|Welche lokalen Anforderungen hinsichtlich Internet und Netzwerk gibt es über das Bereitstellungssystem hinaus?|Keine|[Ausgehender Internetzugriff](/azure/active-directory/hybrid/how-to-connect-pta-quick-start) von den Servern, auf denen Authentifizierung-Agents ausgeführt werden|[Eingehender Internetzugriff](/windows-server/identity/ad-fs/overview/ad-fs-requirements) auf WAP-Server im Umkreisnetzwerk<br><br>Eingehender Netzwerkzugriff auf AD FS-Server von WAP-Servern im Umkreisnetzwerk<br><br>Netzwerklastenausgleich|
|Ist ein SSL-Zertifikat erforderlich?|Nein |Nein |Ja|
|Gibt es eine Systemüberwachungslösung?|Nicht erforderlich|Agent-Status, bereitgestellt von [Azure Active Directory Admin Center](/azure/active-directory/hybrid/tshoot-connect-pass-through-authentication)|[Azure AD Connect Health](/azure/active-directory/hybrid/how-to-connect-health-adfs)|
|Erhalten Benutzer einmaliges Anmelden für Cloudressourcen über Geräte, die in die Domäne eingebunden sind und zum Unternehmensnetzwerk gehören?|Ja, mit [nahtlosem einmaligen Anmelden](/azure/active-directory/hybrid/how-to-connect-sso)|Ja, mit [nahtlosem einmaligen Anmelden](/azure/active-directory/hybrid/how-to-connect-sso)|Ja|
|Welche Anmeldetypen werden unterstützt?|Benutzerprinzipalname + Kennwort<br><br>Integrierte Windows-Authentifizierung mit [nahtlosem einmaligen Anmelden](/azure/active-directory/hybrid/how-to-connect-sso)<br><br>[Alternative Anmelde-ID](/azure/active-directory/hybrid/how-to-connect-install-custom)|Benutzerprinzipalname + Kennwort<br><br>Integrierte Windows-Authentifizierung mit [nahtlosem einmaligen Anmelden](/azure/active-directory/hybrid/how-to-connect-sso)<br><br>[Alternative Anmelde-ID](/azure/active-directory/hybrid/how-to-connect-pta-faq)|Benutzerprinzipalname + Kennwort<br><br>sAMAccountName + Kennwort<br><br>Integrierte Windows-Authentifizierung<br><br>[Zertifikat- und Smartcard-Authentifizierung](/windows-server/identity/ad-fs/operations/configure-user-certificate-authentication)<br><br>[Alternative Anmelde-ID](/windows-server/identity/ad-fs/operations/configuring-alternate-login-id)|
|Wird Windows Hello for Business unterstützt?|[Modell der schlüsselbasierten Vertrauensstellung](/windows/security/identity-protection/hello-for-business/hello-identity-verification)<br><br>[Modell der Zertifikatvertrauensstellung mit Intune](https://blogs.technet.microsoft.com/microscott/setting-up-windows-hello-for-business-with-intune/)|[Modell der schlüsselbasierten Vertrauensstellung](/windows/security/identity-protection/hello-for-business/hello-identity-verification)<br><br>[Modell der Zertifikatvertrauensstellung mit Intune](https://blogs.technet.microsoft.com/microscott/setting-up-windows-hello-for-business-with-intune/)|[Modell der schlüsselbasierten Vertrauensstellung](/windows/security/identity-protection/hello-for-business/hello-identity-verification)<br><br>[Modell der Zertifikatvertrauensstellung](/windows/security/identity-protection/hello-for-business/hello-key-trust-adfs)|
|Welche Optionen gibt es für die mehrstufige Authentifizierung?|[Azure MFA](/azure/multi-factor-authentication/)<br><br>[Benutzerdefinierte Steuerelemente mit bedingtem Zugriff*](/azure/active-directory/conditional-access/controls#custom-controls-1)|[Azure MFA](/azure/multi-factor-authentication/)<br><br>[Benutzerdefinierte Steuerelemente mit bedingtem Zugriff*](/azure/active-directory/conditional-access/controls#custom-controls-1)|[Azure MFA](/azure/multi-factor-authentication/)<br><br>[Azure MFA-Server](/azure/active-directory/authentication/howto-mfaserver-deploy)<br><br>[MFA über Drittanbieter](/windows-server/identity/ad-fs/operations/configure-additional-authentication-methods-for-ad-fs)<br><br>[Benutzerdefinierte Steuerelemente mit bedingtem Zugriff*](/azure/active-directory/conditional-access/controls#custom-controls-1)|
|Welche Benutzerkontenstatus werden unterstützt?|Deaktivierte Konten<br>(bis zu 30 Minuten Verzögerung)|Deaktivierte Konten<br><br>Konto gesperrt<br><br>Konto abgelaufen<br><br>Kennwort abgelaufen<br><br>Anmeldestunden|Deaktivierte Konten<br><br>Konto gesperrt<br><br>Konto abgelaufen<br><br>Kennwort abgelaufen<br><br>Anmeldestunden|
|Welche Optionen für bedingten Zugriff gibt es?|[Bedingter Zugriff auf Azure AD](/azure/active-directory/active-directory-conditional-access-azure-portal)|[Bedingter Zugriff auf Azure AD](/azure/active-directory/active-directory-conditional-access-azure-portal)|[Bedingter Zugriff auf Azure AD](/azure/active-directory/active-directory-conditional-access-azure-portal)<br><br>[AD FS-Anspruchsregeln](https://adfshelp.microsoft.com/AadTrustClaims/ClaimsGenerator)|
|Wird Blockieren älterer Protokolle unterstützt?|[Ja](/azure/active-directory/active-directory-conditional-access-conditions#legacy-authentication)|[Ja](/azure/active-directory/active-directory-conditional-access-conditions#legacy-authentication)|[Ja](/windows-server/identity/ad-fs/operations/access-control-policies-w2k12)|
|Können das Logo, das Bild und die Beschreibung auf den Anmeldeseiten angepasst werden?|[Ja, mit Azure AD Premium](/azure/active-directory/customize-branding)|[Ja, mit Azure AD Premium](/azure/active-directory/customize-branding)|[Ja](/azure/active-directory/connect/active-directory-aadconnect-federation-management#customlogo)|
|Welche erweiterten Szenarien werden unterstützt?|[Intelligente Kennwortsperrung](/azure/active-directory/active-directory-secure-passwords)<br><br>[Berichte über kompromittierte Anmeldeinformationen](/azure/active-directory/active-directory-reporting-risk-events)|[Intelligente Kennwortsperrung](/azure/active-directory/connect/active-directory-aadconnect-pass-through-authentication-smart-lockout)|Authentifizierungssystem mit geringer Wartezeit für mehrere Standorte<br><br>[AD FS-Extranetsperre](/windows-server/identity/ad-fs/operations/configure-ad-fs-extranet-soft-lockout-protection)<br><br>[Integration in Identitätssysteme von Drittanbietern](/azure/active-directory/connect/active-directory-aadconnect-federation-compatibility)|

<!-- markdownlint-enable MD033 -->

> [!NOTE]
> Der bedingte Zugriff in Azure AD über benutzerdefinierte Steuerelemente unterstützt zurzeit keine Geräteregistrierung.

## <a name="next-steps"></a>Nächste Schritte

Das [Hybrididentitätsframework für digitale Transformation](https://resources.office.com/ww-landing-M365E-EMS-IDAM-Hybrid-Identity-WhitePaper.html?LCID=EN-US) beschreibt eine Reihe von Kombinationen und Lösungen für die Auswahl und Integration jeder dieser Komponenten.

Mit dem [Azure AD Connect-Tool](https://aka.ms/aadconnectwiz) können Sie Ihre lokalen Verzeichnisse in Azure AD integrieren.
