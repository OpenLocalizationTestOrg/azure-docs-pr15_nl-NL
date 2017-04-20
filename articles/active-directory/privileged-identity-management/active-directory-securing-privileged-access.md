<properties
    pageTitle="Bevoegdheden toegang beveiligen in Azure AD | Microsoft Azure"
    description="Een onderwerp waarin wordt uitgelegd de benaderingen voor de beveiliging van bevoegdheden toegang via Azure, Azure Active Directory en Microsoft Online Services."
    services="active-directory"
    documentationCenter=""
    authors="kgremban"
    manager="femila"
    editor="mwahl"/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/26/2016"
    ms.author="kgremban"/>


# <a name="securing-privileged-access-in-azure-ad"></a>Bevoegdheden toegang beveiligen in Azure AD

Bevoegdheden toegang beveiligen, is een kritieke eerste stap om te helpen beveiligen van bedrijfsactiva in een modern organisatie. Bevoegdheden accounts zijn items die beheren en IT-systemen beheren. Cyber hackers afstemmen deze accounts toegang tot de gegevens en systemen van een organisatie. Wilt beveiligen bevoegdheden access, moet u de accounts en systemen van de kans op pesterijen wordt blootgesteld aan een kwaadwillende gebruiker isoleren.

Meer gebruikers zijn gestart bevoegdheden om toegang te krijgen tot en met cloudservices. Dit kunt globale beheerders van Office 365, Azure abonnement beheerders en gebruikers met beheerderstoegang in VMs of op SaaS apps opnemen.

U wordt aangeraden dat u Volg dit overzicht [bevoegdheden](https://technet.microsoft.com/library/mt631194.aspx)toegang beveiligen.

Voor klanten die Azure Active Directory voor het beheren van toegang tot Azure, Office 365, of andere Microsoft-services en toepassingen, wordt deze beginselen toepassen of gebruikersaccounts worden beheerd en geverifieerd door Active Directory of in Azure Active Directory. De volgende secties vindt meer informatie over Azure AD-functies voor de ondersteuning van bevoegdheden toegang beveiligen.

## <a name="multi-factor-authentication"></a>Meervoudige verificatie

Als u wilt vergroten de beveiliging van beheerder verificatie, moet u meervoudige verificatie (MFA) voordat het toekennen van de bevoegdheden vereisen. MFA is een methode om te controleren wie u bent die is vereist voor het gebruik van meer dan alleen een gebruikersnaam en wachtwoord. Een tweede laag van beveiliging op gebruiker aanmeldingen en transacties krijgen.

Azure meervoudige verificatie helpt beschermen toegang tot gegevens en toepassingen tijdens de vergadering gebruiker aanvraag voor een eenvoudig proces aanmelden. Sterke verificatie via een bereik van eenvoudig controleopties inclusief telefoongesprekken, SMS-berichten, meldingen voor mobiele app, of verificatiecode en 3e partijen EED tokens levert.

Zie de volgende video voor een overzicht van de werking van Azure meervoudige verificatie.

>[AZURE.VIDEO windows-azure-multi-factor-authentication]

Zie [MFA voor Office 365 en MFA voor Azure](https://blogs.technet.microsoft.com/ad/2014/02/11/mfa-for-office-365-and-mfa-for-azure/)voor meer informatie.

## <a name="time-bound-privileges"></a>Tijd-gebonden bevoegdheden

Sommige organisaties kunnen het gebeuren dat ze te veel gebruikers in veel bevoegdheden rollen hebben. Een gebruiker kan zijn toegevoegd aan de rol voor een specifieke activiteit, zoals te registreren voor een service, maar deze bevoegdheden vaak daarna niet gebruiken.

Verlaag het moment weergeven van bevoegdheden en vergroten uw zicht op hun gebruiken door gebruikers beperken tot alleen op hun bevoegdheden slechts in de tijd (JUST), het duurt desgewenst een taak uit te voeren. Voor de Azure Active Directory en Microsoft Online Services, kunt u [Azure AD bevoegdheden identiteitsbeheer (PIM)](http://aka.ms/AzurePIM).


![PIM dashboard][2]


## <a name="attack-detection"></a>Aanval detectie

[Azure Active Directory identiteitsbeveiliging](../active-directory-identityprotection.md) biedt een geconsolideerde weergave in de risico's en potentiële problemen invloed zijn op de identiteit van uw organisatie. Op basis van risico's, berekent identiteitsbeveiliging een niveau van de gebruiker risico voor elke gebruiker, zodat u kunt de risico's gebaseerde beleidsregels voor als u wilt beveiligen automatisch de identiteit van uw organisatie configureren. Dit beleid, samen met andere voorwaardelijke toegang tot besturingselementen voor verstrekt door Azure Active Directory en EMS, kunnen automatisch de gebruiker blokkeert of suggesties die opnieuw instellen van wachtwoorden en meervoudige verificatie afdwingen bevatten.

![Beveiliging van Azure AD-identiteit][3]

## <a name="conditional-access"></a>Voorwaardelijke toegang

Met voorwaardelijke toegangsbeheer controles Azure Active Directory de specifieke voorwaarden die u kiest u bij het verifiëren van een gebruiker, vóór het toestaan van toegang tot een toepassing. Zodra deze voorwaarden is voldaan, wordt de gebruiker is geverifieerd, en toegang hebben tot de toepassing.


![Regels voor voorwaardelijke toegang met MFA instellen][4]


## <a name="related-articles"></a>Verwante artikelen

- [Azure meervoudige verificatie](../../multi-factor-authentication/multi-factor-authentication-get-started-cloud.md) inschakelen
- [Azure AD-rechten identiteitsbeheer](../active-directory-privileged-identity-management-configure.md) inschakelen
- [Azure AD-identiteit beveiliging](../active-directory-identityprotection.md) inschakelen
- [Voorwaardelijke access-besturingselementen](../active-directory-conditional-access.md) inschakelen


Zie de sectie "verantwoordelijkheden voor de klant en routekaart" van het document [Microsoft Cloud Security voor Enterprise Architects](http://aka.ms/securecustomer) voor meer informatie over het samenstellen van een volledige beveiliging routekaart. Neem contact op met uw Microsoft-vertegenwoordiger voor meer informatie over het Microsoft-services om u te helpen met deze onderwerpen aantrekkelijke, of gaat u naar onze [pagina over Cybersecurity oplossingen](https://www.microsoft.com/microsoftservices/campaigns/cybersecurity-protection.aspx).

<!--Image references-->
[1]: ../media/active-directory-privileged-identity-management-configure/Search_PIM.png
[2]: ../media/active-directory-privileged-identity-management-configure/PIM_Dash.png
[3]: ../media/active-directory-identityprotection/29.png
[4]: ../media/active-directory-conditional-access/conditionalaccess-saas-apps.png
