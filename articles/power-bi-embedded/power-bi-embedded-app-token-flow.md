<properties
   pageTitle="Verificatie en machtiging met Power BI ingesloten"
   description="Verificatie en machtiging met Power BI ingesloten"
   services="power-bi-embedded"
   documentationCenter=""
   authors="guyinacube"
   manager="erikre"
   editor=""
   tags=""/>
<tags
   ms.service="power-bi-embedded"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="powerbi"
   ms.date="10/04/2016"
   ms.author="asaxton"/>

# <a name="authenticating-and-authorizing-with-power-bi-embedded"></a>Verificatie en machtiging met Power BI ingesloten

De Power BI ingesloten service gebruikt **toetsen** en **App Tokens** voor verificatie en machtiging, in plaats van expliciete eindgebruikers verificatie. In dit model beheert uw toepassing verificatie en machtiging voor uw eindgebruikers. Wanneer dat nodig is, wordt uw app maakt en stuurt van de App-Tokens waarin staat dat onze service om de gevraagde rapport weer te geven. Hoewel u nog steeds kunt, geen dit ontwerp uw app Azure Active Directory gebruiken voor gebruikersverificatie en autorisatie vereist.

## <a name="two-ways-to-authenticate"></a>Twee manieren om te verifiëren

**Toets** - kunt u de toetsen voor alle gesprekken met Power BI ingesloten REST API gebruiken. De toetsen vindt u in de **portal van Azure** door te klikken op **alle instellingen** en klik vervolgens op **toegangstoetsen**. Uw sleutel altijd beschouwd als een wachtwoord. Deze toetsen heeft machtigingen voor het maken van een REST API bellen op een bepaalde werkruimte-siteverzameling.

Voor informatie over het gebruik van een sleutel op een gesprek REST toevoegen de volgende autorisatie kop:            

    Authorization: AppKey {your key}

**App token** - App tokens worden gebruikt voor alle insluiten aanvragen. Ze bent ontworpen om te worden uitgevoerd aan de clientzijde, zodat ze tot één rapport beperkt bent en aanbevolen procedures voor het instellen van een verlooptijd is.

App tokens zijn een JWT (JSON Web Token) die door een van uw sleutels is ondertekend.

Uw app-token bevatten de volgende argumenten:

| Claimen      | Beschrijving        |
|--------------|------------|
| **ver**      | De versie van de app-token. 0.2.0 is de huidige versie.       |
| **AUD**      | De geadresseerde van het token. Voor Power BI ingesloten gebruik: "https://analysis.windows.net/powerbi/api".  |
| **ISS**      |  Een tekenreeks die aangeeft dat u de toepassing die het token verleend.    |
| **type**     | Het type app token dat wordt gemaakt. Huidige het enige ondersteunde type is **insluiten**.   |
| **draadloze**      | Naam van de siteverzameling werkruimte het token wordt uitgegeven.  |
| **WID**      | Werkruimte-ID het token wordt uitgegeven.  |
| **verwijderen**      | Rapporteer dat het token-ID wordt uitgegeven.     |
| **gebruikersnaam** (optioneel) |  Met RLS hebt gebruikt, is dit een tekenreeks waarmee u de gebruiker bepalen kunt wanneer RLS regels toepassen. |
| **rollen** (optioneel)   |   Een tekenreeks met de rollen Selecteer wanneer rij-beveiliging op gebruikersniveau regels toepassen. Als meer dan één functie doorgeeft, moeten ze worden doorgegeven als een matrix String.    |
| **EXP** (optioneel)    |   Geeft de tijd waarin het token verloopt. Deze moeten worden doorgegeven als Unix tijdstempels.   |
| **NBF** (optioneel)    |   Geeft de tijd waarin het token begint geldig. Deze moeten worden doorgegeven als Unix tijdstempels.   |

Een voorbeeld app token er als volgt uit:

![](media\power-bi-embedded-app-token-flow\power-bi-embedded-app-token-flow-sample-coded.png)


Als decoderen, wordt dit er als volgt:

![](media\power-bi-embedded-app-token-flow\power-bi-embedded-app-token-flow-sample-decoded.png)


## <a name="heres-how-the-flow-works"></a>Hier ziet u de werking van de stroom

1. Kopieer de API-toetsen in uw toepassing. U kunt de toetsen krijgen in **Azure-Portal**.

    ![](media\powerbi-embedded-get-started-sample\azure-portal.png)

2. Token bevestigingen een claimen en heeft een verlooptijd.

    ![](media\powerbi-embedded-get-started-sample\power-bi-embedded-token-2.png)

3. Token wordt ondertekend met een API-toegangstoetsen.

    ![](media\powerbi-embedded-get-started-sample\power-bi-embedded-token-3.png)

4. Het aanvragen van de gebruikers voor een rapport weergeven.

    ![](media\powerbi-embedded-get-started-sample\power-bi-embedded-token-4.png)

5.  Token is gevalideerd met een API-toegangstoetsen.

    ![](media\powerbi-embedded-get-started-sample\power-bi-embedded-token-5.png)

6.  Power BI ingesloten is een rapport verzonden naar de gebruiker.

    ![](media\powerbi-embedded-get-started-sample\power-bi-embedded-token-6.png)

Nadat **Power BI ingesloten** een rapport aan de gebruiker stuurt, kan de gebruiker het rapport bekijken in uw aangepaste app. Als u het [analyseren van verkoop gegevens PBIX voorbeeld](http://download.microsoft.com/download/1/4/E/14EDED28-6C58-4055-A65C-23B4DA81C4DE/Analyzing_Sales_Data.pbix)hebt geïmporteerd, de steekproef web-app er bijvoorbeeld als volgt:

![](media\powerbi-embedded-get-started-sample\sample-web-app.png)

## <a name="see-also"></a>Zie ook
- [Aan de slag met Microsoft Power BI ingesloten voorbeeld](power-bi-embedded-get-started-sample.md)
- [Veelvoorkomende scenario's voor Microsoft Power BI ingesloten](power-bi-embedded-scenarios.md)
- [Aan de slag met Microsoft Power BI ingesloten](power-bi-embedded-get-started.md)
