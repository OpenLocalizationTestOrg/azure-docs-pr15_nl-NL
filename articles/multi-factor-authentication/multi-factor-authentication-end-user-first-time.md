<properties
    pageTitle="Verificatie in twee stappen voor mijn werk of school-account instellen"
    description="Wanneer uw bedrijf Azure meervoudige verificatie configureert, wordt u gevraagd te registreren voor verificatie in twee stappen. Leer hoe u deze instellen. "
    services="multi-factor-authentication"
    keywords="het gebruik van azure-directory, active directory in de cloud, active directory-zelfstudie"
    documentationCenter=""
    authors="kgremban"
    manager="femila"
    editor="pblachar"/>

<tags
    ms.service="multi-factor-authentication"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/10/2016"
    ms.author="kgremban"/>

# <a name="set-up-my-account-for-two-step-verification"></a>Mijn account voor verificatie in twee stappen instellen

Verificatie in twee stappen is een stap voor extra beveiliging die helpt bij de bescherming van uw account door deze moeilijker voor andere personen wilt afbreken. Als u in dit artikel leest, hebt u waarschijnlijk een e-mailbericht van uw werk- of schoolaccount beheerder over meervoudige verificatie. Of u wellicht geprobeerd aan te melden en krijg een bericht waarin u wordt gevraagd om in te stellen extra beveiliging verificatie. Als dit is de hoofdletters/kleine letters, die **u niet kunt aanmelden totdat u het automatisch - registratieproces hebt voltooid**.

In dit artikel kunt u uw **werk- of schoolaccount**instellen. Als u inschakelen verificatie in twee stappen voor uw eigen, persoonlijke Microsoft-account wilt, leest u [over verificatie in twee stappen](https://support.microsoft.com/help/12408/microsoft-account-about-two-step-verification).

## <a name="determine-how-you-will-use-multi-factor-authentication"></a>Bepalen hoe u meervoudige verificatie wordt gebruikt

Verificatie in twee stappen werkt met de mededeling dat er voor twee soorten identificatie wanneer u zich aanmeldt. Eerst vragen we voor uw gebruikersnaam en wachtwoord zoals u gewend bent. Vervolgens Bevestig we contactpersonen een telefoon die we weten tot u en u behoort dat de aanmeldingspoging is legitiem.  

Om te beginnen met het installatieproces wil aanmelden bij uw account, zoals u gewoonlijk doet. Als uw beheerder heeft uw account voor verificatie in twee stappen geconfigureerd, wordt u gevraagd om te beginnen met het automatisch inschrijven. Met dit proces beginnen door te klikken op **stelt u dit nu.**

![Setup](./media/multi-factor-authentication-end-user-first-time/first.png)

De eerste vraag in het registratieproces is hoe u dat wij contact met u opnemen. Bekijk de opties in de tabel en het gebruik van de koppelingen om te gaan naar de instellingsstappen voor elke methode.

| Contactmethode | Beschrijving |
| --- | --- |
[De mobiele app](#use-a-mobile-app-as-the-contact-method) | - **Meldingen voor verificatie ontvangen.** Deze optie geeft een melding naar de verificator-app op uw smartphone of tablet. Weergeven van de melding en, als dat zo legitieme is, selecteert u **verifiëren** in de app. Uw werk of school mogelijk vereisen dat u een PINCODE opgeven voordat u verifiëren.<br>- **Gebruik de verificatiecode.** In deze modus genereert de app verificator een verificatiecode die elke 30 seconden wordt bijgewerkt. Voer de meest recente verificatiecode in de interface aanmelden.<br>De app Microsoft Authenticator is beschikbaar voor [Windows Phone-](http://go.microsoft.com/fwlink/?Linkid=825071), [Android](http://go.microsoft.com/fwlink/?Linkid=825072)en [IOS](http://go.microsoft.com/fwlink/?Linkid=825073). |
[Mobiele telefoongesprek of de tekst](#use-your-mobile-phone-as-the-contact-method) | - **Telefoongesprek** geplaatst een geautomatiseerd spraaksysteem oproep naar het telefoonnummer dat u opgeeft. Beantwoord de oproep en druk op # in het toetsenblok van de telefoon om te verifiëren.<br>- **SMS-bericht** eindigt SMS-bericht met een verificatiecode. Na de vraag in de tekst, de SMS-bericht beantwoorden of Voer de verificatiecode die is opgegeven in de interface van aanmeldingsproblemen. |  
[Office-telefoongesprek](#use-your-office-phone-as-the-contact-method) | Geplaatst een geautomatiseerd spraaksysteem oproep naar het telefoonnummer dat u opgeeft. Beantwoord de oproep en op # drukt in het toetsenblok van de telefoon om te verifiëren. |

## <a name="use-a-mobile-app-as-the-contact-method"></a>Een mobiele app als de contactmethode gebruiken

Met deze methode is vereist dat u een verificator-app op uw telefoon of tablet installeren. De stappen in dit artikel zijn gebaseerd op de app Microsoft Authenticator, dat beschikbaar voor [Windows Phone-](http://go.microsoft.com/fwlink/?Linkid=825071), [Android](http://go.microsoft.com/fwlink/?Linkid=825072)en [IOS is](http://go.microsoft.com/fwlink/?Linkid=825073).

1. Selecteer **de mobiele app** in de vervolgkeuzelijst.
2. Selecteer **meldingen ontvangen voor verificatie** of **verificatiecode gebruiken**en selecteer **instellen**.

    ![Extra beveiliging verificatie scherm](./media/multi-factor-authentication-end-user-first-time-mobile-app/mobileapp.png)

3. Klik op uw telefoon of tablet, opent u de app en selecteer **+** een account toevoegen. (Selecteer de drie puntjes, klikt u vervolgens op **account toevoegen**op Android-apparaten.)
4. Opgeven dat u wilt toevoegen van een account voor werk- of schoolaccount. Hiermee opent u de QR-code scanner op uw telefoon. Als de camera niet goed werkt is, kunt u uw bedrijfsgegevens handmatig invoeren. Zie [een account handmatig toevoegen](#add-an-account-manually)voor meer informatie.  
5. Scan de QR-code-afbeelding die wordt weergegeven met het scherm voor het configureren van de mobiele app.  Selecteer **Gereed** om te sluiten van het scherm QR-code.  

    ![QR-code scherm](./media/multi-factor-authentication-end-user-first-time-mobile-app/scan2.png)

6. Wanneer de activering is voltooid op de telefoon, selecteert u **Contact mij**.  Deze stap verzendt een melding of een verificatiecode in die naar uw telefoon. Selecteer **verifiëren**.  
7. Als uw organisatie nodig een PINCODE heeft voor het goedkeuren van aanmeldingsproblemen verificatie, voert u deze.

    ![Vak voor het invoeren van een PINCODE](./media/multi-factor-authentication-end-user-first-time-mobile-app/scan3.png)

8. Nadat de vermelding van de PINCODE is voltooid, selecteert u als volgt te **sluiten**. Nu moet uw verificatie werken.
9. Het is raadzaam dat u uw mobiele telefoonnummer invoeren in het geval u geen toegang meer tot uw mobiele app. Geef uw land uit de vervolgkeuzelijst en voer uw mobiele telefoonnummer in het vak naast de landnaam van het. Selecteer **volgende**.
10. In dit stadium wordt u gevraagd voor het instellen van appwachtwoorden voor niet-browser-apps zoals Outlook 2010 of ouder of de systeemeigen e-mail-app op Apple-apparaten. Dit is omdat sommige apps verificatie in twee stappen niet ondersteunen. Als u deze apps niet gebruikt, klikt u op **voltooid** en de rest van de stappen overslaan.
11. Als u deze apps gebruikt, wordt het appwachtwoord kopie geleverd en plak deze in uw toepassing in plaats van uw wachtwoord regelmatig. U kunt hetzelfde appwachtwoord gebruiken voor meerdere apps. Voor meer informatie, [helpen met appwachtwoorden].
12. Klik op **Gereed**.


### <a name="add-an-account-manually"></a>Handmatig een account toevoegen
Volg deze stappen als u wilt een account toevoegen aan de mobiele app handmatig in plaats van de QR-lezer.

1. Selecteer de knop **account handmatig invoeren** .  
2. Voer de code en de URL die beschikbaar zijn op dezelfde pagina waarin u ziet u de streepjescode. Deze informatie vindt u in de vakken **Code** en **URL** op de mobiele app.

    ![Setup](./media/multi-factor-authentication-end-user-first-time-mobile-app/barcode2.png)

3. Wanneer de activering is voltooid, selecteert u **Contact mij**. Deze stap verzendt een melding of een verificatiecode in die naar uw telefoon. Selecteer **verifiëren**.

## <a name="use-your-mobile-phone-as-the-contact-method"></a>Uw mobiele telefoon gebruiken als de contactmethode

1. Selecteer **Verificatie telefoon** in de vervolgkeuzelijst.  

    ![Setup](./media/multi-factor-authentication-end-user-first-time-mobile-phone/phone.png)  

2. Kies uw land in de vervolgkeuzelijst en voer uw mobiele telefoonnummer.
3. Selecteer de methode die u wilt gebruiken met uw mobiele telefoon - tekst of het gesprek.
4. Selecteer **contactpersoon mij** om te controleren of uw telefoonnummer. Afhankelijk van de modus die u hebt geselecteerd, wordt voor berichten of bellen. Volg de instructies op het scherm en selecteert u **verifiëren**.
5. In dit stadium wordt u gevraagd voor het instellen van appwachtwoorden voor niet-browser-apps zoals Outlook 2010 of ouder of de systeemeigen e-mail-app op Apple-apparaten. Dit is omdat sommige apps verificatie in twee stappen niet ondersteunen. Als u deze apps niet gebruikt, klikt u op **voltooid** en de rest van de stappen overslaan.
6. Als u deze apps gebruikt, wordt het appwachtwoord kopie geleverd en plak deze in uw toepassing in plaats van uw wachtwoord regelmatig. U kunt hetzelfde appwachtwoord gebruiken voor meerdere apps. Voor meer informatie, [helpen met appwachtwoorden].
7. Klik op **Gereed**.

## <a name="use-your-office-phone-as-the-contact-method"></a>Uw telefoon op kantoor gebruiken als de contactmethode

1. Selecteer **Telefoon op kantoor** in de vervolgkeuzelijst  

    ![Setup](./media/multi-factor-authentication-end-user-first-time-office-phone/office.png)  

2. Het vak telefoonnummer wordt automatisch ingevuld met de contactgegevens van uw bedrijf. Als het getal onjuist of ontbreekt is, vraagt u uw beheerder om wijzigingen aanbrengen.
4. Selecteer **contactpersoon mij** om te controleren of uw telefoonnummer en we uw nummer bellen. Volg de instructies op het scherm en selecteert u **verifiëren**.
5. In dit stadium wordt u gevraagd voor het instellen van appwachtwoorden voor niet-browser-apps zoals Outlook 2010 of ouder of de systeemeigen e-mail-app op Apple-apparaten. Dit is omdat sommige apps verificatie in twee stappen niet ondersteunen. Als u deze apps niet gebruikt, klikt u op **voltooid** en de rest van de stappen overslaan.
6. Als u deze apps gebruikt, wordt het appwachtwoord kopie geleverd en plak deze in uw toepassing in plaats van uw wachtwoord regelmatig. U kunt hetzelfde appwachtwoord gebruiken voor meerdere apps. Zie [Wat zijn Appwachtwoorden](multi-factor-authentication-end-user-app-passwords.md)voor meer informatie.
7. Klik op **Gereed**.

## <a name="next-steps"></a>Volgende stappen

- Uw voorkeur opties en [beheren van uw instellingen voor verificatie in twee stappen](multi-factor-authentication-end-user-manage-settings.md) wijzigen
- [Appwachtwoorden](multi-factor-authentication-end-user-app-passwords.md) voor systeemeigen apparaat-apps die geen ondersteuning voor verificatie in twee stappen bieden instellen.
- Bekijk de [app Microsoft verificator](multi-factor-authentication-microsoft-authenticator.md) voor snelle, beveiligde verificatie zelfs wanneer er geen cel-service.
