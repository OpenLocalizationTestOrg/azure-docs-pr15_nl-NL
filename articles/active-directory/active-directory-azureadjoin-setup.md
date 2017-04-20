<properties
    pageTitle="Deelnemen aan Azure AD instellen voor uw gebruikers | Microsoft Azure"
    description="Dit artikel wordt uitgelegd hoe beheerders kunnen instellen Azure AD deelnemen aan voor on-premises adreslijst en apparaatregistratie."
    services="active-directory"
    documentationCenter=""
    authors="femila"
    manager="swadhwa"
    editor=""
    tags="azure-classic-portal"/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="09/27/2016"
    ms.author="femila"/>

# <a name="setting-up-azure-ad-join-in-your-organization"></a>Bij het instellen van Azure AD deelnemen in uw organisatie

Voordat u ingesteld Azure Active Directory deelnemen (Azure AD deelnemen aan), moet u handmatig beheerde accounts maken in Azure AD of synchroniseren van uw on-premises adreslijst van gebruikers in de cloud.

Gedetailleerde instructies voor het synchroniseren van uw on-premises gebruikers naar Azure AD valt [integreren met Azure Active Directory identiteiten on-premises implementatie](active-directory-aadconnect.md).


Handmatig wilt maken en beheren van gebruikers in Azure AD, raadpleegt u [Gebruikersbeheer in Azure AD](https://msdn.microsoft.com/library/azure/hh967609.aspx).

## <a name="set-up-device-registration"></a>Apparaatregistratie instellen
1. Meld u aan bij de Azure-portal als beheerder.
2. Selecteer in het linkerdeelvenster, **Active Directory**.
3. Selecteer het telefoonboek van uw op het tabblad **map** .
4. Selecteer het tabblad **configureren** .
5. Ga naar de sectie **Devices** .
6. Klik op het tabblad **apparaten** het volgende instellen:  
   * **MAXIMUM aantal van apparaten PER gebruiker**: Selecteer het maximum aantal apparaten die een gebruiker in Azure AD hebben kan.  Als een gebruiker dit quotum bereikt, is deze niet mogelijk met het toevoegen van extra apparaten totdat een of meer van de bestaande apparaten worden verwijderd.
   * **Vereisen meervoudige AUTH naar JOIN apparaten**: instellen of gebruikers moeten een tweede verificatie factor hun apparaat te koppelen aan Azure AD bieden. Zie voor meer informatie over Azure meervoudige verificatie, de [aan de slag met Azure meervoudige verificatie in de cloud](..\multi-factor-authentication\multi-factor-authentication-get-started-cloud.md).
   * **Gebruikers kunnen AZURE AD JOIN apparaten**: Selecteer de gebruikers en groepen die zijn toegestaan om apparaten te koppelen aan Azure AD.
   * **Extra beheerders op AZURE AD die zijn GEKOPPELD apparaten**: met Azure AD Premium of de Enterprise mobiliteit Suite (EMS), kunt u kiezen welke gebruikers krijgen lokale beheerdersrechten bij het apparaat. Globale beheerders en eigenaren van het apparaat verleend lokale beheerdersrechten al dan niet standaard.

<center>![Instellen van apparaat regisration](./media/active-directory-azureadjoin/active-directory-aadjoin-configure-devices.png)</center>

Nadat u hebt ingesteld Azure AD deelnemen aan voor uw gebruikers, kunnen ze verbinding maken met Azure AD tot en met zijn of haar zakelijke of persoonlijke apparaten.

Hier volgen de drie scenario's die kunt u dat uw gebruikers kunnen deelnemen aan Azure AD instellen:

- Gebruikers deelnemen aan een bedrijf eigendom apparaat rechtstreeks aan Azure AD.
- Gebruikers domein-join een apparaat eigendom bij het bedrijf met de lokale Active Directory en klikt u vervolgens het apparaat op Azure AD.
- Gebruikers toevoegen accounts voor werk- of schoolaccount met Windows op een persoonlijk apparaat

## <a name="additional-information"></a>Aanvullende informatie
* [Windows 10 voor de onderneming: manieren om apparaten voor werk](active-directory-azureadjoin-windows10-devices-overview.md)
* [Cloud mogelijkheden naar Windows 10-apparaten via Azure Active Directory deelnemen uitbreiden](active-directory-azureadjoin-user-upgrade.md)
* [Meer informatie over het gebruik scenario's voor het deelnemen aan Azure AD](active-directory-azureadjoin-deployment-aadjoindirect.md)
* [Een domein behoren apparaten verbinden met Azure AD voor Windows 10 ervaringen](active-directory-azureadjoin-devices-group-policy.md)
* [Deelnemen aan Azure AD instellen](active-directory-azureadjoin-setup.md)
