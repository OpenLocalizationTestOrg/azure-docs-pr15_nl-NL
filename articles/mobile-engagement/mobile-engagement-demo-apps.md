<properties
    pageTitle="Azure Mobile betrokkenheid demo-app | Microsoft Azure"
    description="Wordt beschreven waar u wilt downloaden en het gebruik van de voordelen van het gebruik van Azure Mobile betrokkenheid demo-app"
    services="mobile-engagement"
    documentationCenter="mobile"
    authors="piyushjo"
    manager="erikre"
    editor="" />

<tags
    ms.service="mobile-engagement"
    ms.workload="mobile"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="06/10/2016"
    ms.author="piyushjo" />

# <a name="azure-mobile-engagement-demo-app"></a>Azure Mobile betrokkenheid demo-app

We hebt een Azure Mobile betrokkenheid demo-app voor **iOS**, **Android**en **Windows** -platforms om te zoeken naar nuttige informatiebronnen en meer informatie over Mobile betrokkenheid gepubliceerd.

De app helpt u bij:

- Gemakkelijk vinden nuttige koppelingen naar mobiele betrokkenheid bronnen, zoals video's, documentatie, het ondersteuningsforum en waar kunt u die functieverzoeken verzenden.
- Voorbeeld-meldingen voor ervaring die worden ondersteund door mobiele betrokkenheid voor ideeën voor uw eigen mobiele toepassingen.
- Gebruik de implementatie van een verwijzing bij het bestuderen van het implementeren van mobiele betrokkenheid naar uw eigen-app. U leert:

    - Analytics gegevens verzamelen.
    - Geavanceerde melding scenario's van typen zoals *verspreide volledig scherm* of *pop-*implementeren.
    - Enquêtes en polls implementeren.
    - Implementeer gegevens en push stille push-scenario's.   

## <a name="app-installation"></a>App-installatie
Deze app is beschikbaar in de volgende app winkels:

- **Windows universele demo-app**:

    - Download de app in de [Windows-App opslaan](https://www.microsoft.com/en-us/store/apps/azure-mobile-engagement/9nblggh4qmh2).
    - De app is ontwikkeld als een app voor Windows 10 universele. De broncode is beschikbaar op [GitHub](https://github.com/Azure/azure-mobile-engagement-app-windows).

- **iOS demo app**:

    - Download de app in de [Apple store](https://itunes.apple.com/us/app/azure%20mobile%20engagement/id1105090090).
    - De app is iOS Swift ontwikkeld. De broncode is beschikbaar op [GitHub](https://github.com/Azure/azure-mobile-engagement-app-ios).

- **Android demo app**:

    - Download de app in de [Google Play store](https://play.google.com/store/apps/details?id=com.microsoft.azure.engagement).
    - De broncode is beschikbaar op [GitHub](https://github.com/Azure/azure-mobile-engagement-app-android).

![Windows universele demo-app][1]

![iOS demo app][2]
![Android demo-app][3]


## <a name="usage"></a>Gebruik

U kunt deze app gebruiken in de volgende manieren:

**De app op uw apparaat downloaden uit de toepassing store koppelingen (eerder):**

>[AZURE.IMPORTANT] U hoeft geen een Azure-account of moet de app verbinden met back-enddatabase. De app werkt onafhankelijk.

- Nadat u de app op uw apparaat hebt, kunt u de koppelingen in het menu tabel aan de linkerkant de handige bronnen over Mobile betrokkenheid doorlopen.
- Er is toegevoegd de [service van RSS-feed](https://aka.ms/azmerssfeed) in deze toepassing zodat u altijd over de meest recente updates hebt bijgewerkt.
- U kunt ook de steekproef melding scenario's te veel van het type meldingen die worden ondersteund door mobiele betrokkenheid voor elk platform doorlopen. Deze meldingen kunnen worden ervaren lokaal--die is, kunt u de knoppen op de schermen om aan te geven de meldingen-ervaring, die gelijk is aan de meldingen die wordt verzonden vanuit het platform Mobile betrokkenheid op.

![App-menu voor Windows][4]

![App-menu voor iOS][5]
![App-menu voor Android][6]

**Download de broncode uit de GitHub koppelingen (eerder):**

- Nadat u de code hebt gedownload, kunt u deze in de desbetreffende ontwikkelomgeving--XCode voor iOS, Android Studio voor Android en Visual Studio voor Windows openen.
- Vervolgens moet u onze [SDK integratie basisstappen](mobile-engagement-windows-store-dotnet-get-started.md) volgen zodat u zich kunt deze app verbinden met een eigen exemplaar van de back-enddatabase Mobile betrokkenheid.
    - U moet een verbindingsreeks configureren in de app.
    - U moet ook de push notification platform voor de app configureren.
- U ziet dat de app zelf met mobiele betrokkenheid is geïmplementeerd. Als u de app opent nadat u deze naar de back-end verbinding hebt, moet u dus kan de gebruikerssessie, activiteiten, gebeurtenissen, enzovoort, op het tabblad **Beeldscherm** zien.
- U kunt ook wel meldingen voor deze app verzenden vanuit uw eigen exemplaar Mobile betrokkenheid, in plaats van lokale meldingen.
    - Hier kunt u uw apparaat toevoegen als een testapparaat met behulp van het menu-item van **het apparaat-ID ophalen** in de app. Hiermee geeft u een apparaat-ID die u vervolgens als een testapparaat met uw exemplaar van de back-enddatabase platform registreren.

    ![Apparaat-ID in Windows][7]

    ![Apparaat-ID in iOS][8]
    ![apparaat-ID op android-apparaat][9]

## <a name="key-features-of-the-demo-app"></a>Belangrijkste functies van de demo-app

- Zoals eerder is vermeld met deze app, hebt u de belangrijkste bronnen voor Mobile betrokkenheid in uw hand. U kunt gaan via de koppelingen in het linkermenu.

- U kunt nog steeds ondervindt out-van-app-meldingen voor elk platform. Deze meldingen kunnen worden bezorgd als **melding alleen**waar te klikken op de melding gewoon wordt geopend een systeemeigen scherm van de toepassing (met behulp van **uitgebreide koppelen**)-- of als een **aankondiging met webonderdelen**, waar u extra HTML-inhoud kunt geven uit de back-end Mobile betrokkenheid moet worden weergegeven wanneer de melding wordt geklikt.

    ![Out-van-app-meldingen][29]

- Op iOS moet u de app om te zien van de kant-van-app of systeem push-meldingen sluiten. U kunt de uitvoering voor het toevoegen van **Actieknoppen bevinden**, zoals de bouwstenen die worden toegevoegd aan deze melding out-van-app voor *Feedback* en *delen* (zodat de gebruiker actie rechts van de melding zelf uitvoeren kan) bekijken.

    ![Out-van-app-meldingen in iOS][11] ![Weergave van de melding out-van-app op iOS][14]

- Op android-apparaat toevoegt de opties die worden ondersteund met meerdere regels tekst (**Groot tekst**) of een melding illustratie (**Totaalbeeld**) op de melding, samen met de **Actieknoppen bevinden** (zoals ondersteund door iOS).

    ![Out-van-app-meldingen op android-apparaat][12] ![Out-van-app melding weergeven op android-apparaat][15]

- Klik op Windows 10 kunt u zien hoe de meldingen kijkt u op de PC. Deze melding wordt ook weergegeven in het Windows 10- **Melding Center**. Er is geen ondersteuning voor het toevoegen van **Actieknoppen** op het moment dat in de Windows SDK.

    ![Out-van-app-meldingen in Windows][10] ![De weergave out-van-app in Windows][13]

- U kunt nog steeds ondervindt standaard "app" meldingen voor elk platform. Dit is a twee stappen-ervaring, waarbij een **melding** venster eerst worden weergegeven. Wanneer u erop klikt, wordt deze geopend van een volledig scherm **aankondiging**, zoals weergegeven in de volgende schermafbeelding. De inhoud van deze aankondiging afkomstig zijn uit uw exemplaar van de back-enddatabase Mobile betrokkenheid. De SDK bevat de sjablonen voor zowel meldingen en aankondigingen. U kunt deze eenvoudig aanpassen zoals in deze demo-app met het toevoegen van onze logo en kleuren.  

    ![App-meldingen in Windows][16]

    ![App-meldingen in iOS][17]  ![App-meldingen op android-apparaat][18]

    **iOS**, **Android**

- U kunt ook de functie **categorie** van Mobile betrokkenheid gebruiken om aan te passen deze standaard-ervaring. In de demo-app hebt we twee algemene manier om te wijzigen van de ervaring van de kennisgevingen gedemonstreerd. Houd er rekening mee dat de categorie-functie is nog niet ondersteund in de Windows SDK.

    **Volledige schermweergave verspreide:**

    ![Melding van de app - verspreide categorie][30]

    ![Verspreide categorie op iOS][21]  ![Verspreide categorie op android-apparaat][22]

    **Pop-upmelding:**

    ![Melding van de app - pop-categorie][31]

    ![Pop-upmelding op iOS][19]   ![Pop-upmelding op android-apparaat][20]

**iOS**, **Android**

- Mobile betrokkenheid ondersteunt ook een speciale type app melding **Polls**genoemd. Hiermee kunt u snel enquêtes naar uw gesegmenteerde app-gebruikers wordt gestuurd. U kunt vragen en opties voor elke vraag zoals in de volgende schermafbeelding toevoegen. Hiermee wordt vervolgens krijgen weergegeven als een melding in de app aan de app-gebruiker.   

    ![Peiling meldingen][32]

    ![Enquête in Windows][26]

    ![Enquête op iOS][27]   ![Enquête op android-apparaat][28]

**iOS**, **Android**

- Mobile betrokkenheid ondersteunt ook stille **Gegevens Push** -meldingen verzenden. Met deze meldingen, kunt u gegevens verzendt van uw service (zoals de JSON gegevens in het volgende voorbeeld) die u kunt verwerken in uw app en enkele actie uitvoeren. Een voorbeeld is hoe we wijzigt de prijs van een item selectief met behulp van gegevens push-meldingen.

    ![Melding op Data push][33]

    ![Melding op Data push in Windows][23]

    ![Melding op Data push op iOS][24]  ![Melding op Data push op android-apparaat][25]

**iOS**, **Android**

> [AZURE.NOTE] U kunt stapsgewijze instructies voor een van deze meldingen weergeven door te klikken op **Klik hier voor instructies over het verzenden van deze meldingen voor mobiele betrokkenheid platform** op een willekeurig scherm van de steekproef melding.


[1]: ./media/mobile-engagement-demo-apps/home-windows.png
[2]: ./media/mobile-engagement-demo-apps/home-ios.png
[3]: ./media/mobile-engagement-demo-apps/home-android.png
[4]: ./media/mobile-engagement-demo-apps/menu-windows.png
[5]: ./media/mobile-engagement-demo-apps/menu-ios.png
[6]: ./media/mobile-engagement-demo-apps/menu-android.png
[7]: ./media/mobile-engagement-demo-apps/device-id-windows.png
[8]: ./media/mobile-engagement-demo-apps/device-id-ios.png
[9]: ./media/mobile-engagement-demo-apps/device-id-android.png
[10]: ./media/mobile-engagement-demo-apps/out-of-app-windows.png
[11]: ./media/mobile-engagement-demo-apps/out-of-app-ios.png
[12]: ./media/mobile-engagement-demo-apps/out-of-app-android.png
[13]: ./media/mobile-engagement-demo-apps/out-of-app-display-windows.png
[14]: ./media/mobile-engagement-demo-apps/out-of-app-display-ios.png
[15]: ./media/mobile-engagement-demo-apps/out-of-app-display-android.png
[16]: ./media/mobile-engagement-demo-apps/in-app-windows.png
[17]: ./media/mobile-engagement-demo-apps/in-app-ios.png
[18]: ./media/mobile-engagement-demo-apps/in-app-android.png
[19]: ./media/mobile-engagement-demo-apps/pop-up-ios.png
[20]: ./media/mobile-engagement-demo-apps/pop-up-android.png
[21]: ./media/mobile-engagement-demo-apps/interstitial-ios.png
[22]: ./media/mobile-engagement-demo-apps/interstitial-android.png
[23]: ./media/mobile-engagement-demo-apps/data-push-windows.png
[24]: ./media/mobile-engagement-demo-apps/data-push-ios.png
[25]: ./media/mobile-engagement-demo-apps/data-push-android.png
[26]: ./media/mobile-engagement-demo-apps/survey-windows.png
[27]: ./media/mobile-engagement-demo-apps/survey-ios.png
[28]: ./media/mobile-engagement-demo-apps/survey-android.png
[29]: ./media/mobile-engagement-demo-apps/out-of-app.png
[30]: ./media/mobile-engagement-demo-apps/in-app-interstitial.png
[31]: ./media/mobile-engagement-demo-apps/in-app-pop-up.png
[32]: ./media/mobile-engagement-demo-apps/notification-poll.png
[33]: ./media/mobile-engagement-demo-apps/notification-data-push.png
