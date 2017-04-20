<properties
    pageTitle="Microsoft Authenticator-app voor mobiele telefoons | Microsoft Azure"
    description="Informatie over het upgraden naar de meest recente versie van Azure verificator."
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

# <a name="microsoft-authenticator"></a>Microsoft verificator

De app Microsoft Authenticator biedt een extra beveiligingsniveau in uw Azure-account (bijvoorbeeld bsimon@contoso.onmicrosoft.com), werkaccount voor uw on-premises (bijvoorbeeld bsimon@contoso.com), of uw Microsoft-account (bijvoorbeeld bsimon@outlook.com).

De app werkt op twee manieren:

- **Melding**. De app kan helpen voorkomen dat onbevoegden toegang tot accounts en frauduleuze transacties stoppen door te drukken een melding naar uw smartphone of tablet. Gewoon de melding weergeven en als dat zo legitieme is, selecteert u **verifiëren**. Anders kunt u **weigeren**. Zie het gebruik van de weigeren en fraude rapportfunctie voor meervoudige verificatie voor informatie over meldingen weigeren.

- **Wachtwoord met verificatiecode**. De app kan worden gebruikt als een software-token genereren een verificatiecode OAuth. Voert u de code die is verstrekt door de app in het aanmeldingsscherm, samen met de gebruikersnaam en wachtwoord, wanneer u wordt gevraagd. De verificatiecode biedt een tweede vorm van verificatie.

Met de versie van de app Microsoft Authenticator, wordt de oude Azure verificator-app vervangen.  De app Azure verificator nog steeds werkt, maar als u besluit om naar de nieuwe Microsoft Authenticator-app te gaan, in dit artikel u kan helpen.  

## <a name="install-the-app"></a>De app installeren

De app Microsoft Authenticator is beschikbaar voor [Windows Phone-](http://go.microsoft.com/fwlink/?Linkid=825071), [Android](http://go.microsoft.com/fwlink/?Linkid=825072)en [IOS](http://go.microsoft.com/fwlink/?Linkid=825073).

## <a name="add-accounts-to-the-app"></a>Accounts toevoegen aan de app

Voor elk account die u wilt toevoegen aan de app Microsoft Authenticator, een van de volgende procedures te gebruiken.

### <a name="add-an-account-to-the-app-by-using-the-qr-code-scanner"></a>Een account toevoegen aan de app met behulp van de scanner QR-code

1. Ga naar het scherm beveiliging verificatie-instellingen.  Zie [uw beveiligingsinstellingen wijzigen](multi-factor-authentication-end-user-manage-settings.md)voor informatie over hoe u dit scherm te openen.

2. Selecteer **configureren**.

    ![De knop configureren op het scherm beveiliging verificatie-instellingen](./media/multi-factor-authentication-azure-authenticator/azureauthe.png)

    Hiermee opent u een scherm met een QR-code op is geïnstalleerd.

    ![Scherm vindt u de QR-code](./media/multi-factor-authentication-azure-authenticator/barcode2.png)

3. Open de app Microsoft verificator. Selecteer op het scherm **accounts** **+**, en geef vervolgens dat u wilt toevoegen van een account voor werk- of schoolaccount.

    ![Het scherm accounts met een plusteken (+)](./media/multi-factor-authentication-azure-authenticator/addaccount3.png)

    ![Scherm om aan te geven van een account voor werk of school](./media/multi-factor-authentication-end-user-first-time-mobile-app/scan.png)

4. Gebruik van de camera om te scan de QR-code en selecteer vervolgens **Gereed** om te sluiten van het scherm QR-code.

    ![Scherm voor het scannen van een QR-code](./media/multi-factor-authentication-end-user-first-time-mobile-app/scan2.png)

    Als de camera niet goed werkt is, kunt u de QR-code en de URL handmatig invoeren. Zie [een account bij de app handmatig toevoegen](#add-an-account-to-the-app-manually)voor meer informatie.

5. Wacht tot het account is geactiveerd. Wanneer de activering is voltooid, selecteert u **Contact mij**.  Hiermee verzendt een melding of een verificatiecode in die naar uw telefoon.  Selecteer **verifiëren**.

    ![Scherm waarin u verifiëren aan te melden selecteren](./media/multi-factor-authentication-end-user-first-time-mobile-app/verify.png)

6. Als uw organisatie nodig een PINCODE heeft voor het goedkeuren van aanmeldingsproblemen verificatie, voert u deze.

    ![Vak voor het invoeren van een PINCODE](./media/multi-factor-authentication-end-user-first-time-mobile-app/scan3.png)

7. Nadat de vermelding van de PINCODE is voltooid, selecteert u als volgt te **sluiten**. Nu moet uw verificatie werken.
8. Het is raadzaam dat u uw mobiele telefoonnummer invoeren in het geval u geen toegang meer tot uw app. Geef uw land uit de vervolgkeuzelijst en voer uw mobiele telefoonnummer in het vak naast de landnaam van het. Selecteer **volgende**.
9. U hebt nu uw contactmethode ingesteld. Nu is het tijd voor het instellen van appwachtwoorden voor niet-browser-apps, zoals Outlook 2010 of eerder. Als u deze apps niet gebruikt, selecteert u **klaar**. Ga anders verder met de volgende stap.

    ![Scherm voor het maken van een appwachtwoord](./media/multi-factor-authentication-end-user-first-time-mobile-app/step4.png)

10. Als u niet-browser apps gebruikt, de meegeleverde appwachtwoord kopiëren en plak het wachtwoord in uw apps. Zie voor instructies voor afzonderlijke apps zoals Outlook en Lync, het wijzigen van het wachtwoord in uw e-mailbericht naar het appwachtwoord en hoe u het wachtwoord in uw toepassing naar het appwachtwoord wijzigen.
11. Selecteer **Gereed**.

U ziet nu het nieuwe account op het scherm **accounts** .

![Scherm accounts](./media/multi-factor-authentication-azure-authenticator/accounts.png)

### <a name="add-an-account-to-the-app-manually"></a>Een account handmatig toevoegen aan de app

1. Ga naar het scherm beveiliging verificatie-instellingen.  Zie [uw beveiligingsinstellingen wijzigen](multi-factor-authentication-end-user-manage-settings.md)voor informatie over hoe u dit scherm te openen.

2. Selecteer **configureren**.

    ![De knop configureren op het scherm beveiliging verificatie-instellingen](./media/multi-factor-authentication-azure-authenticator/azureauthe.png)

    Hiermee opent u een scherm met een QR-code op is geïnstalleerd.  Houd rekening met de code en de URL.

    ![Scherm vindt u de QR-code en de URL](./media/multi-factor-authentication-azure-authenticator/barcode2.png)

3. Open de app Microsoft verificator. Selecteer op het scherm **accounts** **+**, en geef vervolgens dat u wilt toevoegen van een account voor werk- of schoolaccount.

    ![Het scherm accounts met een plusteken (+)](./media/multi-factor-authentication-azure-authenticator/addaccount3.png)

    ![Scherm om aan te geven van een account voor werk of school](./media/multi-factor-authentication-end-user-first-time-mobile-app/scan.png)

4. Selecteer de **code handmatig invoeren**in de scanner.

    ![Scherm voor het scannen van een QR-code](./media/multi-factor-authentication-end-user-first-time-mobile-app/scan2.png)

5. Voer de code en de URL in de gewenste selectievakjes in de app.

    ![Scherm voor het invoeren van code en URL](./media/multi-factor-authentication-azure-authenticator/manual.png)

    ![Scherm voor het invoeren van code en URL](./media/multi-factor-authentication-end-user-first-time-mobile-app/addaccount2.png)

6. Wacht tot het account is geactiveerd. Wanneer de activering is voltooid, selecteert u **Contact mij**. Hiermee verzendt een melding of een verificatiecode in die naar uw telefoon. Selecteer **verifiëren**.

U ziet nu het nieuwe account op het scherm **accounts** .

![Scherm accounts](./media/multi-factor-authentication-azure-authenticator/accounts.png)

### <a name="add-an-account-to-the-app-by-using-touch-id"></a>Een account toevoegen aan de app met behulp van aanraakschermen-ID

De app Microsoft Authenticator op iOS ondersteunt Touch-ID.  Azure meervoudige verificatie kunnen organisaties een PINCODE vereisen voor apparaten. Met Raak-ID moeten iOS-gebruikers niet een PINCODE opgeven. In plaats daarvan kunnen hun vingerafdruk scannen en selecteer **goedkeuren**.

Het is eenvoudig Raak-ID met Microsoft Authenticator instellen. U voert u een normale verificatie uitdaging met een PINCODE. Als uw apparaat Raak-ID ondersteunt, wordt Microsoft Authenticator deze automatisch ingesteld voor dat account.

![Verificatie van aanraakschermen-ID instellen](./media/multi-factor-authentication-azure-authenticator/touchid1.png)

Uit dat punt als u bent vereist voor de verificatie van uw aanmeldingsproblemen, u, selecteert u de melding ontvangen push en scan uw vingerafdruk in plaats van uw pincode.

![Push-bericht](./media/multi-factor-authentication-azure-authenticator/touchid2.png)

## <a name="uninstall-the-old-azure-authentication-app"></a>De oude Azure verificatie-app verwijderen

Nadat u alle toegevoegde accounts aan de nieuwe app hebt toegevoegd, kunt u de oude app verwijderen uit uw telefoon.

## <a name="delete-an-account"></a>Een account verwijderen

Een account verwijderen uit de app Microsoft Authenticator, selecteer het account en selecteer vervolgens **verwijderen**.

![Knop verwijderen](./media/multi-factor-authentication-azure-authenticator/remove.png)
