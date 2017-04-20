<properties
    pageTitle="Azure aanmelding bij MFA ervaring met Azure meervoudige verificatie"
    description="Deze pagina, vindt u instructies over waar kunt u om de verschillende aanmelding bij methoden beschikbaar voor communicatie met Azure MFA weer te geven."
    keywords="gebruikersverificatie, aanmeldervaring, aanmelden met een mobiele telefoon, aanmelden met een telefoon op kantoor"
    services="multi-factor-authentication"
    documentationCenter=""
    authors="kgremban"
    manager="femila"
    editor="curtland"/>

<tags
    ms.service="multi-factor-authentication"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/22/2016"
    ms.author="kgremban"/>

# <a name="the-sign-in-experience-with-azure-multi-factor-authentication"></a>De aanmelden ervaring met Azure meervoudige verificatie
> [AZURE.NOTE]  De volgende documentatie op deze pagina ziet u een typisch aanmeldervaring.  Zie [problemen met Azure meervoudige verificatie](multi-factor-authentication-end-user-manage-settings.md) voor hulp bij het aanmelden



## <a name="what-will-your-sign-in-experience-be"></a>Wat is uw aanmelden ervaring?
Afhankelijk van hoe u zich aanmeldt en meervoudige verificatie gebruikt, wordt uw ervaring verschillen.  In deze sectie, we informatie vindt over wat u kunt verwachten wanneer u zich aanmeldt.  Kies de die het beste wordt beschreven wat u doet:


Wat ben je aan het doen?|Beschrijving
:------------- | :------------- |
[Aanmelden met mobiel of office](#signing-in-with-mobile-or-office-phone) | Dit is wat u kunt verwachten niet aanmelden via uw telefoon mobiel of office.
[Aanmelden met de melding met Microsoft Authenticator-app](#signing-in-with-the-microsoft-authenticator-app-using-notification) | Dit is wat u kunt verwachten gebruik van de app Microsoft Authenticator met meldingen.
[Aanmelden met de verificatiecode in die met Microsoft Authenticator-app](#signing-in-with-the-microsoft-authenticator-app-using-verification-code)|Dit is wat u kunt verwachten gebruik van de Microsoft-Authenticator thapp met een verificatiecode.
[Aanmelden met een andere methode](#signing-in-with-an-alternate-method)|Hier ziet u wat u kunt verwachten als u wilt een alternatieve methode gebruiken.

## <a name="signing-in-with-mobile-or-office-phone"></a>Aanmelden met mobiel of office

De volgende informatie beschrijft de ervaring van het gebruik van meervoudige verificatie met mobiel of office.

### <a name="to-sign-in-with-a-call-to-your-office-or-mobile-phone"></a>Aan te melden met een oproep door naar uw kantoor of mobiele telefoon

- Aanmelden bij een toepassing of service zoals Office 365 met uw gebruikersnaam en wachtwoord.
- Microsoft belt u.

![Microsoft-oproepen](./media/multi-factor-authentication-end-user-signin-phone/call.png)

- Beantwoord de telefoon en druk op de toets #.

![Answer](./media/multi-factor-authentication-end-user-signin-phone/phone.png)

- U moet nu zijn aangemeld.</li>

## <a name="signing-in-with-the-microsoft-authenticator-app-using-notification"></a>Aanmelden met de melding met Microsoft Authenticator-app

De volgende informatie beschrijft de ervaring van het gebruik van meervoudige verificatie met de app Microsoft Authenticator wanneer u een melding gestuurd.

### <a name="to-sign-in-with-a-notification-sent-the-microsoft-authenticator-app"></a>Om aan te melden met een melding verzonden de app Microsoft Authenticator

- Aanmelden bij een toepassing of service zoals Office 365 met uw gebruikersnaam en wachtwoord.
- Microsoft stuurt een melding.

![Microsoft wordt melding gestuurd](./media/multi-factor-authentication-end-user-signin-app-notify/notify.png)


- Beantwoord de telefoon en druk op de toets verifiëren.  Als uw bedrijf vereist is voor een PINCODE wordt u gevraagd voor deze hier.

![Controleer of](./media/multi-factor-authentication-end-user-signin-app-notify/phone.png)

![Setup](./media/multi-factor-authentication-end-user-first-time-mobile-app/scan3.png)

- U moet nu zijn aangemeld.


## <a name="signing-in-with-the-microsoft-authenticator-app-using-verification-code"></a>Aanmelden met de verificatiecode in die met Microsoft Authenticator-app

De volgende informatie beschrijft de ervaring van het gebruik van meervoudige verificatie met de app Microsoft Authenticator wanneer u deze met een verificatiecode gebruikt.

### <a name="to-sign-in-using-a-verification-code-with-the-microsoft-authenticator-app"></a>Aanmelden met een verificatiecode in die met de app Microsoft Authenticator

- Aanmelden bij een toepassing of service zoals Office 365 met uw gebruikersnaam en wachtwoord.
- Microsoft vraagt u om een verificatiecode.

![Voer de verificatiecode](./media/multi-factor-authentication-end-user-signin-app-verify/verify.png)

- Open de app Microsoft Authenticator op uw telefoon en voer de code in het vak waar u zich wilt aanmelden.

![Code ophalen](./media/multi-factor-authentication-end-user-signin-app-verify/phone.png)



- U moet nu zijn aangemeld.


## <a name="signing-in-with-an-alternate-method"></a>Aanmelden met een andere methode


De volgende sectie wordt uitgelegd hoe u aan te melden met een andere methode wanneer uw primaire methode mogelijk niet beschikbaar.

### <a name="to-sign-in-with-an-alternate-method"></a>Aan te melden met een andere methode

- Aanmelden bij een toepassing of service zoals Office 365 met uw gebruikersnaam en wachtwoord.
- Selecteer een optie hiërarchie gebruiken.  U bent presenteren met een aantal andere opties. Het nummer er kan worden gebaseerd op hoeveel u ingesteld hebt.

![Alternatieve methode gebruiken](./media/multi-factor-authentication-end-user-signin-alt/alt.png)

- Kies een andere methode en meld u aan.
