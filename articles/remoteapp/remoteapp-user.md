<properties
    pageTitle="Een gebruiker toevoegen aan uw Azure RemoteApp-verzameling | Microsoft Azure"
    description="Leer hoe u gebruikers toevoegt aan uw Azure RemoteApp-verzameling"
    services="remoteapp"
    documentationCenter=""
    authors="lizap"
    manager="mbaldwin" />

<tags
    ms.service="remoteapp"
    ms.workload="compute"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/15/2016"
    ms.author="elizapo" />

# <a name="how-to-add-a-user-to-your-azure-remoteapp-collection"></a>Hoe u een gebruiker toevoegen aan de verzameling Azure RemoteApp

> [AZURE.IMPORTANT]
> Azure RemoteApp is wordt gestopt. Lees de [aankondiging](https://go.microsoft.com/fwlink/?linkid=821148) voor meer informatie.

Voordat u uw gebruikers kunnen zien en uw apps gebruiken in Azure RemoteApp, hebt u deze om toegang te verlenen aan uw verzameling. Dit is het gemakkelijk gedeelte: klik op het tabblad **Gebruikerstoegang** Voer de gegevens van het account voor de gebruiker en klik op het vinkje.

De accountgegevens hebt u nodig? Dat is afhankelijk van het type van de siteverzameling u hebt gemaakt (cloud of hybride) en of u gebruikmaakt van Office 365 ProPlus in die verzameling.

## <a name="supported-user-identities"></a>Ondersteunde gebruikersidentiteit

De verschillende typen (cloud versus hybride) ondersteuning voor het gebruik van andere gebruikersidentiteiten voor toegang tot toepassingen.  

Voor een hybride verzameling RemoteApp moet u een infrastructuur voor Active Directory-domein instellen op de lokale en een Azure Active Directory-tenant met Directory-integratie (en desgewenst eenmalige aanmelding). Daarnaast moet u enkele Active Directory-objecten maken in de on-premises adreslijst.  

Voor een RemoteApp cloud verzameling kan een gebruiker met Azure Active Directory identiteiten ondersteunen de toegang van gebruikers worden toegekend aan RemoteApp om op te nemen van Microsoft-Accounts.  Zie de onderstaande tabel.

Office 365-gebruikers zijn Azure Active Directory-gebruikers. Als ze Azure Active Directory hybride hebben, kunnen Directory-accounts gesynchroniseerd ze gebruikerstoegang worden verleend in een RemoteApp hybride implementatie.   

U kunt deze tabel gebruiken als een beknopt overzicht waarvan de identiteit wordt ondersteund in uw siteverzameling en wat de Active Directory-vereisten zijn.

|Gebruikersaccounts |Cloud   |Hybride|
|--------------|--------|------|
|Microsoft-Account|     Ja|    Nee|
|Azure Active Directory (Azure AD)| | |
|Azure AD cloud    |Ja    |Nee |
|ADsync met Wachtwoordsynchronisatie  |Ja    |Ja    |
|ADsync zonder Wachtwoordsynchronisatie|  Ja |Nee |
|ADsync met AD FS  |Ja    |Ja    |
|[3e derden Azure ondersteund identiteitsproviders](https://msdn.microsoft.com/library/azure/jj679342.aspx)  (voorbeeld Ping) |Ja    |Ja|
|Meervoudige verificatie    |Ja    |Ja    |

[Meer informatie](remoteapp-ad.md) over het configureren van Active Directory voor RemoteApp raadplegen.


> [AZURE.NOTE] De Azure Active Directory-gebruikers moeten zich in de tenant dat is gekoppeld aan uw abonnement. (U kunt weergeven en wijzigen van uw abonnement op het tabblad **Instellingen** in de portal. Zie [de Azure Active Directory-tenant wijzigen die wordt gebruikt door RemoteApp](remoteapp-changetenant.md) voor meer informatie.)

## <a name="office-365-proplus-user-account-information"></a>Office 365-account van ProPlus gebruikersgegevens
Als u de Office 365 ProPlus Sjabloonafbeelding in uw siteverzameling *of* als u een aangepaste afbeelding die gebruikmaakt van Office 365 hebt gemaakt, kunt u zijn alleen toegestaan om toe te voegen van Azure Active Directory-gebruikers die Office 365-abonnement voor het standaarddomein van uw abonnement hebt. Zie [Office 365 gebruiken met Azure RemoteApp](remoteapp-o365.md) voor meer informatie.
