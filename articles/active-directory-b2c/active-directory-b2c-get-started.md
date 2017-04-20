<properties
    pageTitle="Azure Active Directory-B2C: Maak een tenant Azure Active Directory B2C | Microsoft Azure"
    description="Een onderwerp voor het maken van een tenant Azure Active Directory B2C"
    services="active-directory-b2c"
    documentationCenter=""
    authors="swkrish"
    manager="mbaldwin"
    editor="bryanla"/>

<tags
    ms.service="active-directory-b2c"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.topic="article"
    ms.devlang="na"
    ms.date="08/30/2016"
    ms.author="swkrish"/>

# <a name="azure-active-directory-b2c-create-an-azure-ad-b2c-tenant"></a>Azure Active Directory-B2C: Maak een tenant Azure AD B2C

Als u wilt gaan gebruiken van Microsoft Azure Active Directory (Azure AD) B2C, volgt u de drie stappen in dit artikel.

## <a name="step-1-sign-up-for-an-azure-subscription"></a>Stap 1: Registreren voor een Azure-abonnement

Als u al een Azure-abonnement hebt, kunt u deze stap overslaan. Als dat niet zo is, registreren voor een [Azure-abonnement](../active-directory/sign-up-organization.md) en toegang krijgen tot Azure AD B2C.

## <a name="step-2-create-an-azure-ad-b2c-tenant"></a>Stap 2: Maak een tenant Azure AD B2C

Gebruik de volgende stappen uit om te maken van een nieuwe B2C van Azure AD-tenant. Momenteel kunnen niet B2C functies worden ingeschakeld in uw bestaande tenants.

1. Meld u aan bij de [portal van Azure klassieke](https://manage.windowsazure.com/) als de beheerder van abonnement. Dit is de dezelfde werk- of schoolaccount of hetzelfde Microsoft-account dat u gebruikt om u te registreren voor Azure.
2. Klik op **nieuwe** > **App Services** > **Active Directory** > **Directory** > **aangepaste maken**.

    ![Schermafbeelding van het starten van een tenant maken](./media/active-directory-b2c-get-started/new-directory.png)

3. Kies de **naam**, de **Domeinnaam** en het **land of regio** voor uw tenant.
4. Schakel de optie met de mededeling **Dit is een B2C-map dat**.
5. Klik op het vinkje om de bewerking te voltooien.

    ![Schermafbeelding van het selectievakje een B2C-map maken](./media/active-directory-b2c-get-started/create-b2c-directory.png)

6. Uw tenant is nu gemaakt en wordt weergegeven in de Active Directory-extensie. U ook een globale beheerder van de tenant gemaakt. U kunt andere globale beheerders toevoegen zoals vereist.

    > [AZURE.IMPORTANT]
    Als u van plan bent een tenant B2C gebruiken voor een productie-app, leest u het artikel op [productie-schaal versus preview B2C tenants](active-directory-b2c-reference-tenant-type.md). Merk op dat er zijn bekende problemen wanneer u een bestaande B2C-tenant verwijderen en opnieuw met dezelfde domeinnaam maken. U moet een tenant B2C maken met een andere domeinnaam.

## <a name="step-3-navigate-to-the-b2c-features-blade-on-the-azure-portal"></a>Stap 3: Ga naar het blad B2C functies op de Azure-portal

1. Navigeer naar de Active Directory-extensie op de navigatiebalk aan de linkerkant.
2. Zoek uw tenant onder het tabblad **map** en klikt u erop.
3. Klik op het tabblad **configureren** .
4. Klik op de koppeling **Instellingen voor B2C beheren** in de sectie **B2C beheer** .

    ![Schermafbeelding van directory-configuratie voor B2C](./media/active-directory-b2c-get-started/b2c-directory-configure-tab.png)

5. De Azure-portal met het B2C functies blad met wordt geopend in een nieuw browsertabblad of -venster.

    > [AZURE.IMPORTANT]
    Het kan maximaal 2-3 minuten duren voordat uw tenant toegankelijk is op de Azure-portal. Deze stappen opnieuw nadat het enige tijd wordt moet ik dit oplossen. Als dat niet zo is, neem contact op met ondersteuning.

6. Kies vastmaken deze blade naar uw Startboard voor eenvoudige toegang. (Het hulpmiddel pincode is gemarkeerd in het rood in de rechterbovenhoek van het blad functies.)

    ![Schermafbeelding van het blad B2C-functies](./media/active-directory-b2c-get-started/b2c-features-blade.png)

    > [AZURE.NOTE]
    U kunt gebruikers en groepen, configuratie van selfservice-wachtwoord opnieuw instellen en bedrijf huisstijl functies van de tenant in de [klassieke Azure-portal](https://manage.windowsazure.com/)beheren.

## <a name="easy-access-to-the-b2c-features-blade-on-the-azure-portal"></a>Eenvoudig toegang tot het blad B2C functies op de Azure-portal

Voor een betere zichtbaarheid, hebt we een snelkoppeling toegevoegd aan het blad B2C functies op de Azure-portal.

1. Meld u aan bij de Azure-portal als globale beheerder van uw tenant B2C. Als u al bent aangemeld bij een andere tenant, zet u tenants (aan de rechterbovenhoek).
2. Klik op **Bladeren** op de navigatiebalk van de linkerpagina.
3. Klik op **Azure AD B2C** voor toegang tot het blad B2C-functies.

    ![Schermafbeelding van bladeren naar B2C functies blade](./media/active-directory-b2c-get-started/b2c-browse.png)

## <a name="next-steps"></a>Volgende stappen

Meer informatie over het registreren van een toepassing met Azure AD B2C en bouwen van een toepassing snel starten door te lezen [Azure Active Directory B2C: uw toepassing registreren](active-directory-b2c-app-registration.md).
