
<properties
    pageTitle="Wijzigen van de Azure Active Directory-tenant in Azure RemoteApp | Microsoft Azure"
    description="Informatie over het wijzigen van de Azure Active Directory-tenant die is gekoppeld aan Azure RemoteApp"
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



# <a name="change-the-azure-active-directory-tenant-in-azure-remoteapp"></a>Wijzigen van de Azure Active Directory-tenant in Azure RemoteApp

> [AZURE.IMPORTANT]
> Azure RemoteApp is wordt gestopt. Lees de [aankondiging](https://go.microsoft.com/fwlink/?linkid=821148) voor meer informatie.

Azure RemoteApp gebruikt Azure Active Directory (Azure AD) toe te staan dat de toegang van gebruikers. De alleen Azure AD-tenant die u in Azure RemoteApp gebruiken kunt is het account dat is gekoppeld aan het abonnement dat Azure. U kunt het abonnement gekoppeld weergeven op de pagina **Instellingen** in de portal. Bekijk de kolom **Directory** op het tabblad **abonnementen** .

> [AZURE.NOTE] Deze wijziging te kunnen uitvoeren, moet u eerst alle gebruikers verwijderen uit de bestaande Azure Active Directory-tenant uit alle Azure RemoteApp collecties. Klik hiertoe Ga naar de Portal Azure, gaat u naar het tabblad **Azure RemoteApp** en open elke Azure RemoteApp-siteverzameling. Ga naar het tabblad **gebruikers** en gebruikers die deel uitmaakt van uw huidige Azure Active Directory-tenant verwijderen. Herhaal dit voor alle bestaande Azure RemoteApp collecties. Zonder u dit doet, is niet mogelijk om te maken of patch siteverzamelingen.

Als u gebruiken van een andere tenant wilt, gebruikt u deze stappen kunt u de koppeling met uw abonnement:

1. Klik in de portal door een Azure AD-gebruikers waartoe u toegang hebt verleend tot Azure RemoteApp verzamelingen te verwijderen. (Zie de notitie boven voor stapsgewijze instructies over hoe u dit doet.)


2. Stel een Microsoft-account (voorheen een Live ID) als de Service-beheerder. (Niet weet Als u de servicebeheerder al aanwezig zijn? U vindt door te klikken op **beheerders-instellingen >**.) Hier is nu hoe u die wijzigen:
    1. Klik op de gebruiker in de rechterbovenhoek en klik vervolgens op **Mijn factuur**.
    2. Klik op het abonnement. Vervolgens op de pagina nieuwe Schuif omlaag en klik op **details van abonnement bewerken** in de rechterkant. (Sorteren van de middelste rechtsonder wat er als die helpt u te vinden.)
    3. Typ het Microsoft-account voor de gebruiker die worden de service-beheerder moet.

3. Nu, meld u af bij de portal en meld u weer aan met het Microsoft-account dat u hebt opgegeven in de vorige stap.


4. Klikt u op **nieuwe App -> Services -> Active Directory -> Directory -> aangepaste maken**.
5. Kies onder **Directory**, **de bestaande map gebruiken**. We gaan moet u nu zich afmeldt bij de portal, dus kies **dat ik ben klaar om te worden nu afgemeld**.
6. Je weer aanmelden bij de portal als globale beheerder van de map die u wilt toevoegen. (Als u niet al een globale beheerder zijn, kunt u zich na een ronde van aanmelden en vervolgens op Afmelden.)
7. U wordt gevraagd wanneer u zich aanmeldt als u wilt gebruiken van uw bestaande AD-tenant met uw abonnement. Klik op **Doorgaan**en klik vervolgens op **nu afmelden**.
5. Meld u weer aan opnieuw en Ga terug naar **Instellingen -> abonnementen**. Selecteer uw abonnement en klik vervolgens op **Adreslijst bewerken**. Selecteer de Azure AD-tenant die u wilt gebruiken.



U kunt nu gebruik het nieuwe Azure AD-tenant toegang tot het Azure abonnement te beheren en configureren van de toegang van gebruikers in Azure RemoteApp.
