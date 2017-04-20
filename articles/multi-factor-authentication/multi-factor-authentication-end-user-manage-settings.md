<properties
    pageTitle="De twee stappen verificatie-instellingen beheren | Microsoft Azure"
    description="Beheren hoe u Azure meervoudige verificatie, inclusief het wijzigen van uw contactgegevens of het configureren van uw apparaten gebruiken."
    services="multi-factor-authentication"
    keywords = "meervoudige verificatie-client, verificatieprobleem, correlatie-ID"
    documentationCenter=""
    authors="kgremban"
    manager="femila"
    editor="yossib"/>

<tags
    ms.service="multi-factor-authentication"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/10/2016"
    ms.author="kgremban"/>

# <a name="manage-your-settings-for-two-step-verification"></a>Uw instellingen voor verificatie in twee stappen beheren

In dit artikel vindt u antwoorden op vragen over het bijwerken van instellingen voor verificatie of meervoudige verificatie in twee stappen. Als u hebt met het aanmelden bij uw account problemen, raadpleegt [u problemen met verificatie in twee stappen](multi-factor-authentication-end-user-troubleshoot.md) voor probleemoplossing.


## <a name="where-to-find-the-settings-page"></a>Waar vind ik de instellingenpagina
Afhankelijk van hoe uw bedrijf Azure meervoudige verificatie instellen, zijn er enkele locaties waar u uw instellingen zoals uw telefoonnummer wijzigen kunt.

Als uw IT-beheerder verzonden de URL van een specifieke of stappen voor het beheren van verificatie in twee stappen, volgt u deze instructies. De volgende instructies moeten werken anders, voor iedereen anders. Als u deze stappen maar dezelfde opties niet ziet, betekent dit dat uw werk of school hun eigen portal wordt aangepast. Vraag uw beheerder voor de koppeling naar uw Azure meervoudige verificatie-portal.


1. Meld u aan bij [https://myapps.microsoft.com](https://myapps.microsoft.com)  
2. Aan de bovenkant, selecteer **profiel**.  
3. Selecteer **Extra beveiliging verificatie**.  

    ![Apps](./media/multi-factor-authentication-end-user-manage/myapps1.png)

4. De pagina extra beveiliging verificatie wordt geladen met uw instellingen.

    ![Proofup](./media/multi-factor-authentication-end-user-manage-myapps/proofup.png)


## <a name="i-want-to-change-my-phone-number-or-add-a-secondary-number"></a>Ik wil mijn telefoonnummer wijzigen of een secundaire nummer toevoegen

Het is belangrijk is voor het configureren van een telefoonnummer secundaire verificatie.  Omdat uw telefoonnummer en uw mobiele app zich waarschijnlijk op de telefoon dezelfde, is het secundaire telefoonnummer dat u de enige manier is mogelijk om terug te gaan naar uw account bij uw telefoon is verbroken of diefstal.

> [AZURE.NOTE]
> Als u geen toegang tot uw telefoonnummer hebben en meer met uw account in hulp nodig hebt, raadpleegt u onze help-onderwerpen in [problemen met verificatie in twee stappen](multi-factor-authentication-end-user-troubleshoot.md).

**Uw primaire telefoonnummer wijzigen:**  

1. Selecteer het tekstvak met uw huidige telefoonnummer en deze met uw nieuwe telefoonnummer bewerken op de pagina extra beveiliging verificatie.  
2. Selecteer **Opslaan**.  
3. Als dit het nummer dat u voor uw voorkeur verificatieoptie is gebruikt, hebt u voor de verificatie van het nieuwe nummer voordat u deze kunt opslaan.  


**Een secundaire telefoonnummer toevoegen:**  

1. Klik op de verificatiepagina extra beveiliging, schakel het selectievakje in naast **alternatieve verificatie-telefoon.**  
2. Voer uw secundaire telefoonnummer in het tekstvak.  
3. Selecteer **Opslaan** en de wijzigingen zijn voltooid.  


## <a name="how-do-i-clean-up-microsoft-authenticator-from-my-old-device-and-move-to-a-new-one"></a>Hoe Microsoft Authenticator opschonen van mijn oude apparaat en ik verplaatsen naar een nieuw?
Wanneer u de app van uw apparaat verwijderen of het apparaat opnieuw instellen, wordt de activering op de back-end niet verwijderd. U moet de stappen in de [verplaatsen naar een nieuwe apparaat](multi-factor-authentication-microsoft-authenticator.md#how-to-move-to-the-new-microsoft-authenticator-app)gebruiken.

## <a name="next-steps"></a>Volgende stappen
- Informeer tips voor probleemoplossing en help op [problemen met verificatie in twee stappen](multi-factor-authentication-end-user-troubleshoot.md)
- [Appwachtwoorden](multi-factor-authentication-end-user-app-passwords.md) voor apps die geen ondersteuning voor verificatie in twee stappen bieden instellen.
