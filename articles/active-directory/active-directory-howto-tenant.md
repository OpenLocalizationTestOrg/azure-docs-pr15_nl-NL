<properties
    pageTitle="Hoe kom ik aan een Azure AD-tenant | Microsoft Azure"
    description="Hoe kom ik aan een Azure Active Directory-tenant voor het registreren en bouwen van toepassingen."
    services="active-directory"
    documentationCenter=""
    authors="dstrockis"
    manager="terrylan"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="hero-article"
    ms.date="09/28/2015"
    ms.author="dastrock"/>

# <a name="how-to-get-an-azure-active-directory-tenant"></a>Hoe kom ik aan een Azure Active Directory-tenant

Een [tenant](https://msdn.microsoft.com/library/azure/jj573650.aspx#BKMK_WhatIsAnAzureADTenant) in Azure Active Directory (Azure AD) is een voorbeeld van een organisatie.  Dit is een speciale instantie van de Azure AD-service die een organisatie ontvangt en eigenaar is van wanneer deze zich registreert voor een Microsoft-cloudservice zoals Azure, Microsoft Intune of Office 365.  Elke Azure AD-tenant is afzonderlijke uit andere Azure AD-tenants.  

Een tenant bevinden zich de gebruikers in een bedrijf en de informatie over deze - hun wachtwoorden, profiel gebruikersgegevens, machtigingen, enzovoort.  Deze bevat ook groepen, -toepassingen en andere gegevens die betrekking hebben op een organisatie en de beveiliging.

Als u wilt dat Azure AD-gebruikers kunnen Meld u aan bij uw toepassing, moet u uw toepassing registreren in een tenant eigen.  Het publiceren van een toepassing in een Azure AD-tenant is **absoluut gratis**.  In feite maakt de meeste ontwikkelaars verschillende tenants en toepassingen voor experimenten, ontwikkeling, waarin u tijdelijk opslaat en testdoeleinden.  Organisaties die zich aanmeldt bij en gebruiken van uw toepassing kunnen desgewenst kiezen om het aanschaffen van licenties te profiteren van geavanceerde directory-functies.

Zo is, hoe ga u over het aanvragen van een Azure AD-tenant?  Het proces mogelijk een klein verschillende als u:

- [Hebt u een bestaand Office 365-abonnement](#use-an-existing-office-365-subscription)
- [Een bestaand Azure-abonnement dat is gekoppeld aan een Microsoft-Account hebt](#use-an-msa-azure-subscription)
- [Een bestaand Azure-abonnement dat is gekoppeld aan een organisatie-account hebt](#use-an-organizational-azure-subscription)
- [Hebt u geen van de bovenstaande & wilt helemaal opnieuw beginnen](#start-from-scratch)

## <a name="use-an-existing-office-365-subscription"></a>Gebruik van een bestaand Office 365-abonnement
Als u een bestaand Office 365-abonnement hebt, hebt u al een Azure AD-tenant! U kunt aanmelden bij de [portal van Azure](https://portal.azure.com) met uw O365-account en aan de slag met Azure AD.

## <a name="use-an-msa-azure-subscription"></a>Gebruik een MSA Azure-abonnement
Als u eerder voor hebben geregistreerd een Azure-abonnement met de afzonderlijke Microsoft-Account, al hebt u een tenant!  Wanneer u zich hebt aangemeld bij de [Portal van Azure](https://portal.azure.com), wordt u automatisch aangemeld bij de standaard-tenant. U mag gebruiken van deze tenant naar wens - bent, maar u mogelijk wilt maken van een organisatie-beheerdersaccount.

Klik hiervoor als volgt te werk.  U kunt ook Maak een nieuwe tenant en maken van een beheerder in die tenant een soortgelijke proces volgen.

1.  Meld u aan bij de [Portal van Azure](https://portal.azure.com) met uw account afzonderlijke
2.  Ga naar de sectie "Azure Active Directory" van de portal (gevonden in de linkernavigatiebalk, onder **Meer Services**)
3.  U moet automatisch worden aangemeld bij de "Default Directory", als dit niet kunt u mappen schakelen door te klikken op de naam van uw account in de rechterbovenhoek.
4.  Kies in de sectie **Snelle taken** met **een gebruiker toevoegen**.
5.  Geef in het formulier gebruiker toevoegen de volgende details letten:

    - Naam: (Kies een geschikte waarde)
    - Gebruikersnaam in te voeren: (Kies een gebruikersnaam in te voeren voor deze beheerder)
    - Profiel: (doorvoeren in de juiste waarden voor de voornaam, achternaam naam, functie en afdeling)
    - Functie: Globale beheerder van

6.  Wanneer u het formulier toevoegen hebt voltooid, en het tijdelijke wachtwoord voor de nieuwe administratieve gebruiker ontvangt, moet u dit wachtwoord opnemen als moet u zich aanmeldt met deze nieuwe gebruiker om te kunnen het wachtwoord wijzigen. U kunt het wachtwoord ook rechtstreeks naar de gebruiker en met een alternatief e-mailbericht verzenden.
7.  Klik op **maken** om te maken van de nieuwe gebruiker.
8.  Als u wilt wijzigen van het tijdelijke wachtwoord, meld u aan bij [https://login.microsoftonline.com](https://login.microsoftonline.com) met deze nieuwe gebruikersaccount en het wachtwoord desgevraagd wijzigen.


## <a name="use-an-organizational-azure-subscription"></a>Gebruik van een organisatie-Azure-abonnement
Als u hebt een abonnement op Azure eerder aangemeld met uw organisatie-account, al hebt u een tenant!  In de [Portal van Azure](https://portal.azure.com), moet u een tenant zoeken wanneer u navigeren naar "Meer Services" en "Azure Active Directory."  U zijn mag gebruiken van deze tenant wens naar. 


## <a name="start-from-scratch"></a>Helemaal opnieuw beginnen
Als u alle bovenstaande onzin aan u is, geen probleem.  Ga naar [https://account.windowsazure.com/organization](https://account.windowsazure.com/organization) te registreren voor Azure met een nieuwe organisatie.  Wanneer u het proces hebt uitgevoerd, hebt u uw eigen Azure AD-tenant met de naam van het domein dat u hebt gekozen tijdens het aanmelden omhoog.  Klik in de [Portal van Azure](https://portal.azure.com)kunt u uw tenant zoeken door in de linkerpagina nav. naar "Azure Active Directory" navigeren

Als onderdeel van het proces van het zich registreert voor Azure, moet u op te geven van creditcardgegevens.  U kunt doorgaan met betrouwbaarheid - moet u niet betalen voor toepassingen publiceren in Azure AD of maken van nieuwe tenants.
