<properties
    pageTitle="Azure AD Connect in Microsoft Cloud Duitsland"
    description="Azure AD Connect wordt uw on-premises implementatie-mappen integreren met Azure Active Directory. Hiermee kunt u een algemene identiteit verstrekken voor Office 365 en Azure SaaS toepassingen die zijn geïntegreerd met Azure AD."
    keywords="Inleiding naar Azure AD Connect, Azure AD Connect overzicht van wat Azure AD Connect is active directory, Duitsland, zwart bos installeren"
    services="active-directory"
    documentationCenter=""
    authors="billmath"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="09/08/2016"
    ms.author="billmath"/>

#<a name="azure-ad-connect-in-microsoft-cloud-germany---public-preview"></a>Azure AD Connect in Microsoft Cloud Duitsland - Public Preview

## <a name="introduction"></a>Inleiding
Azure AD Connect biedt synchronisatie tussen uw lokale Active Directory en Azure Active Directory.
Veel van de scenario's in [Microsoft Cloud Duitsland](https://www.microsoft.com/de-de/cloud/deutschland/default.aspx) kunnen momenteel, moeten worden uitgevoerd door de operator. Wanneer u met Microsoft Cloud Duitsland, moet u zich bewust zijn van de volgende opties:


- De volgende URL's moeten worden geopend op een proxyserver voor synchronisatie met succes voordoen:
    - *. microsoftonline.de
    - *. windows.net
    - + Certificaatintrekkingslijsten

- Wanneer u zich bij uw Azure AD-adreslijst aanmelden, moet u een account in het domein onmicrosoft.de.
- De volgende functies zijn niet beschikbaar:
    - Azure AD Connect systeemstatus
    - Automatische updates
    - Wachtwoord write-backs

## <a name="download"></a>Downloaden
U kunt Azure AD Connect downloaden van het blad Azure AD Connect binnen de portal.  Gebruik de onderstaande instructies om te zoeken van het blad Azure AD Connect.

### <a name="the-azure-ad-connect-blade"></a>De Azure AD Connect Blade

Zodra u zich hebt aangemeld bij de Azure-portal, het volgende doen:

1. Ga naar bladeren
2.  Selecteer Azure Active Directory
3.  Selecteer Azure AD Connect

Zie de volgende:

![Azure AD Connect Blade](media\active-directory-aadconnect-germany\germany1.png)

 
De volgende tabel worden de functies die wordt weergegeven in het blad.


Titel|Beschrijving|
----- | ----- |
SYNCHRONISATIESTATUS|Laten we eens weet u of de synchronisatie is ingeschakeld of uitgeschakeld.|
LAATSTE SYNCHRONISATIE|De laatste keer een succesvolle synchronisatie is voltooid.|
FEDERATIEVE DOMEINEN|Toont het aantal federatieve domeinen die momenteel zijn geconfigureerd.|


## <a name="installation"></a>Installatie
Als u wilt Azure AD Connect hebt geïnstalleerd, kunt u de documentatie [hier](active-directory-aadconnect.md#install-azure-ad-connect).

## <a name="advanced-features-and-additional-information"></a>Geavanceerde functies en aanvullende informatie
Voor aanvullende informatie en instructies voor het aangepaste instellingen of geavanceerde configuraties, begint u met de [integratie van uw on-premises implementatie identiteiten met Azure Active Directory](active-directory-aadconnect.md).  Deze pagina vindt u informatie en koppelingen naar aanvullende richtlijnen.
