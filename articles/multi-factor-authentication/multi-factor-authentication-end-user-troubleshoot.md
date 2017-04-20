<properties
    pageTitle="Problemen met verificatie in twee stappen | Microsoft Azure"
    description="In dit document, gebruikers informatie vindt over wat u moet doen als ze optreden met Azure meervoudige verificatie problemen."
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

# <a name="having-trouble-with-two-step-verification"></a>Ondervindt u problemen met verificatie in twee stappen

Dit artikel worden enkele problemen die met verificatie in twee stappen optreden kunnen. Als het probleem hier niet opgenomen is, Geef gedetailleerde feedback in de sectie opmerkingen zodat wij het kunnen verbeteren.

## <a name="i-lost-my-phone-or-it-was-stolen"></a>Ik ben mijn telefoon of deze is gestolen

Er zijn twee manieren om terug te gaan bij uw account. De eerste is aan te melden met uw telefoonnummer alternatieve verificatie als u deze hebt ingesteld. Het tweede is om te vragen uw beheerder om uw instellingen te wissen.

Als uw telefoon is verbroken of gestolen, ook aangeraden dat u hebt uw beheerder van uw appwachtwoorden opnieuw instellen en schakel het selectievakje een apparaten onthouden. Als uw beheerder niet precies hoe u dit doen, verwijst u ze naar dit artikel: [gebruikers beheren en apparaten](multi-factor-authentication-manage-users-and-devices.md#delete-users-existing-app-passwords).


### <a name="use-an-alternate-phone-number"></a>Een alternatief telefoonnummer gebruiken

Als u hebt ingesteld met meerdere opties voor verificatie, waaronder een secundaire telefoonnummer of een verificator-app op een ander apparaat, kunt u een van de volgende aan te melden.

Als u zich aanmeldt met het alternatieve nummer, als volgt te werk:

1. Meld u aan zoals u gewend bent.
2. Wanneer u wordt gevraagd om te verder te controleren of uw account, kies **een functie invoegen**.

    ![Hiërarchie](./media/multi-factor-authentication-end-user-manage/differentverification.png)

3. Selecteer het telefoonnummer dat u toegang hebt.

    ![Alternatief telefoonnummer](./media/multi-factor-authentication-end-user-manage/altphone2.png)

4. Nadat u weer aan uw account, [uw instellingen beheren bent](multi-factor-authentication-end-user-manage-settings.md) om te wijzigen van uw authenticatie telefoonnummer.

>[AZURE.IMPORTANT]
>Het is belangrijk is voor het configureren van een telefoonnummer secundaire verificatie. Als uw telefoonnummer en uw mobiele app op de telefoon dezelfde, u moet een derde optie als uw telefoon is verbroken of gestolen.

### <a name="clear-your-settings"></a>Wis uw instellingen

Als u een telefoonnummer secundaire verificatie niet hebt geconfigureerd, hebt u contact op met uw beheerder voor hulp. Deze instellingen van uw wissen zodat de volgende keer dat u zich aanmeldt, wordt u gevraagd naar [instellen van uw account](multi-factor-authentication-end-user-first-time.md) opnieuw zijn.


## <a name="i-am-not-receiving-a-text-or-call-on-my-phone"></a>Ik ontvang geen een tekst of bellen op mijn telefoon

Er zijn verschillende redenen waarom u mogelijk wil aanmelden, maar niet de tekst- of telefoongesprek ontvangen. Als u hebt ontvangen teksten of telefoongesprekken voeren met uw telefoon in het verleden, is dit mogelijk een probleem met de phone-provider, niet met uw account. Zorg ervoor dat u goede cel signaal hebt en als u probeert te ontvangen een tekstbericht voor zorgen dat uw abonnement telefoon- en service ondersteuning voor SMS-berichten.

Als u enkele minuten voor een tekst of het gesprek hebt gewacht, is de snelste manier om bij uw account om te proberen een andere optie.

1. Selecteer **gebruiken dialoogvenster functie invoegen** op de pagina die de verificatie van uw wacht.

    ![Hiërarchie](./media/multi-factor-authentication-end-user-troubleshoot/diff_option.png)

2. Selecteer de telefoon getal of bezorging methode dat u wilt gebruiken.

    Als u meerdere verificatie-codes ontvangen, wordt alleen de nieuwste versie werkt.

Als u een andere methode die zijn geconfigureerd niet hebt, neem contact op met uw beheerder en vraag of om uw instellingen te wissen. De volgende keer dat u zich aanmeldt, wordt u gevraagd om in te [stellen meervoudige verificatie](multi-factor-authentication-end-user-first-time.md) opnieuw.


Als u vaak vertragingen vanwege een ongeldige cel signaal hebt, kunt dat u de [Microsoft-verificator app](multi-factor-authentication-microsoft-authenticator.md) gebruiken op uw smartphone. De app willekeurig beveiligingscodes waarmee u zich aanmelden kunt genereren en deze codes niet een cel signaal of internet-verbinding vereisen.


## <a name="app-passwords-are-not-working"></a>Appwachtwoorden werken niet

Controleer eerst of u het appwachtwoord correct hebt ingevoerd.  Als dit nog niet werkt kunt aanmelden en [een nieuw appwachtwoord maken](multi-factor-authentication-end-user-app-passwords.md).  Als dit niet werkt, contact op met uw beheerder en hen kunt [verwijderen van uw bestaande appwachtwoorden](multi-factor-authentication-manage-users-and-devices.md#delete-users-existing-app-passwords) en u vervolgens een nieuwe id maken.

## <a name="i-didnt-find-an-answer-to-my-problem"></a>Ik kan een antwoord op mijn probleem niet vinden.

Als u deze stappen hebt geprobeerd, maar zijn nog steeds problemen optreden wordt uitgevoerd, neemt u contact op met uw beheerder of de persoon die meervoudige verificatie voor u instellen. Ze moeten kunnen u helpen.

Daarnaast kunt u een vraag in het [Azure AD-Forums](https://social.msdn.microsoft.com/forums/azure/home?forum=WindowsAzureAD) of [contact opnemen met ondersteuning](https://support.microsoft.com/contactus) posten en we moet reageren op uw probleem zodra we kunt.

Als u contact opnemen met ondersteuning, moet u de volgende gegevens bevatten:

- **Gebruikers-ID** : Wat is het e-mailadres dat u hebt geprobeerd te zich aanmeldt?
- **Algemene beschrijving van de fout** : welke exacte foutbericht bedoelde u zien?  Als er geen foutbericht wordt weergegeven, wordt de onverwacht gedrag dat u hebt gezien, tijdens detail beschreven.
- **Pagina** – welke pagina u op wanneer u de fout hebt gezien waren (omvatten de URL)?
- **Foutcode** - de specifieke foutcode die u ontvangt.
- **Sessie-id** - de specifieke sessie-id die u ontvangt.
- **Correlatie-ID** : Wat is de correlatie-id-code gegenereerd wanneer de gebruiker de fout hebt gezien.
- **Tijdstempel** : Wat is de exacte datum en tijd dat u de fout hebt gezien (omvatten het tijdzoneverschil)?

Veel van deze informatie vindt u op de pagina aanmelden. Wanneer u niet controleren of uw aanmeldingsproblemen in tijd, selecteert u **details weergeven**.

![Meer informatie over fout aanmelden](./media/multi-factor-authentication-end-user-troubleshoot/view_details.png)

Deze gegevens kunt ons uw probleem zo snel mogelijk oplossen.

## <a name="related-topics"></a>Verwante onderwerpen
- [Uw instellingen voor verificatie in twee stappen beheren](multi-factor-authentication-end-user-manage-settings.md)  
- [Veelgestelde vragen over Microsoft Authenticator-toepassing](multi-factor-authentication-app-faq.md)
