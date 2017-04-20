<properties 
    pageTitle="Gebruik van castLabs moet geven Widevine licenties voor Azure Media Services | Microsoft Azure" 
    description="In dit artikel wordt beschreven hoe u Azure Media Services (AMS) kunt gebruiken voor het leveren van een stroom die dynamisch door AMS met zowel PlayReady als Widevine DRMs is versleuteld. De licentie PlayReady afkomstig van Media Services PlayReady licentieserver en Widevine licentie wordt geleverd door de licentieserver castLabs." 
    services="media-services" 
    documentationCenter="" 
    authors="Mingfeiy" 
    manager="erikre" 
    editor=""/>

<tags 
    ms.service="media-services" 
    ms.workload="media" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/26/2016"  
    ms.author="Mingfeiy;willzhan;Juliako"/>


#<a name="using-castlabs-to-deliver-widevine-licenses-to-azure-media-services"></a>CastLabs moet geven Widevine licenties voor Azure Media Services gebruiken

> [AZURE.SELECTOR]
- [Axinom](media-services-axinom-integration.md)
- [castLabs](media-services-castlabs-integration.md)

##<a name="overview"></a>Overzicht

In dit artikel wordt beschreven hoe u Azure Media Services (AMS) kunt gebruiken voor het leveren van een stroom die dynamisch door AMS met zowel PlayReady als Widevine DRMs is versleuteld. De licentie PlayReady afkomstig van Media Services PlayReady licentieserver en Widevine licentie wordt geleverd door de licentieserver **castLabs** .

Afspelen streaming inhoud die zijn beveiligd met CENC (PlayReady en/of Widevine), kunt u [Azure Media Player](http://amsplayer.azurewebsites.net/azuremediaplayer.html). Zie [AMP document](http://amp.azure.net/libs/amp/latest/docs/) voor meer informatie.

In het volgende diagram ziet u een hoog niveau Azure Media Services en castLabs integratiearchitectuur.

![integratie](./media/media-services-castlabs-integration/media-services-castlabs-integration.png)

##<a name="typical-system-set-up"></a>Normale systeem instellen

- Media-inhoud wordt opgeslagen in AMS.
- Toets-id's van inhoud sleutels zijn opgeslagen in zowel castLabs als AMS.
- castLabs en AMS hebben token verificatie ingebouwd in. De volgende secties worden verificatietokens. 
- Wanneer het clientaanvragen van een voor stream de video, wordt de inhoud dynamisch versleuteld met **Algemene versleuteling** (CENC) en dynamisch verpakt door AMS vloeiende Streaming en-streepje. Wij bieden ook PlayReady M2TS elementaire stream versleuteling voor HLS streaming protocol.
- PlayReady-licentie is opgehaald uit de licentieserver AMS en Widevine licentie wordt opgehaald uit de licentieserver castLabs. 
- Media Player wordt automatisch bepaald welke licentie om op te halen op basis van de client-platforms. 

##<a name="authentication-token-generation-for-getting-a-license"></a>Verificatie token genereren om een licentie voor

Zowel castLabs als AMS ondersteuning voor token JWT (JSON Web Token)-indeling die wordt gebruikt een licentie toe te staan. 

###<a name="jwt-token-in-ams"></a>JWT token in AMS 

De volgende tabel wordt beschreven JWT token in AMS. 

Uitgever|Uitgever tekenreeks van de gekozen Secure Token Service (STS)
---|---
Publiek|Publiek tekenreeks van de gebruikte STS
Op claims|Een reeks claims
Niet voor|Geldigheid van het token starten
Verloopt|Einde geldigheid van het token
SigningCredentials|De sleutel die wordt gedeeld in PlayReady licentieserver, castLabs licentieserver en STS, wordt u symmetric of asymmetrische sleutel.

###<a name="jwt-token-in-castlabs"></a>JWT token in castLabs

De volgende tabel wordt beschreven JWT token in castLabs. 

Naam|Beschrijving
---|---
optData|Een JSON-tekenreeks die de informatie over u bevat. 
CRT|Een JSON-tekenreeks met informatie over het actief, de licentierechten info en afspelen.
IAT|De huidige datum/tijd in epoche.
jti|Een unieke id over deze token (elke token kan slechts eenmaal worden gebruikt in het systeem castLabs).

##<a name="sample-solution-set-up"></a>Voorbeeld-oplossing instellen 

De [oplossing](https://github.com/AzureMediaServicesSamples/CastlabsIntegration) bestaat uit twee projecten:

-   Een console-app die DRM beperkingen instellen voor al geconsumeerde activa, voor zowel PlayReady en Widevine kan worden gebruikt.
-   Een webtoepassing die handen af beveiligingstokens die kunnen worden beschouwd als een zeer VEREENVOUDIGD versie van een STS.


Gebruik de consoletoepassing:

1.  Wijzig de app.config om AMS referenties, castLabs referenties, STS-configuratie en gedeelde sleutel in te stellen.
2.  Upload activa naar AMS.
3.  De UUID krijgen van de geüploade activa en lijn 32 in het bestand Program.cs wijzigen:

         var objIAsset = _context.Assets.Where(x => x.Id == "nb:cid:UUID:dac53a5d-1500-80bd-b864-f1e4b62594cf").FirstOrDefault();

4.  Gebruik een item-id voor de naamgeving van de activa in het systeem castLabs (44 lijn in het bestand Program.cs).

    U moet item-id instellen voor **castLabs**; Dit moet een unieke, alfanumerieke tekenreeks.

5.  Voer het programma.


Gebruik van de Web-toepassing (STS):

1.  De web.config wijzigen in setup castlabs verkoper-ID, de STS-configuratie en de gedeelde sleutel.
2.  Dashboard implementeren naar Azure Websites.
3.  Navigeer naar de website.

##<a name="playing-back-a-video"></a>Een video afspelen

Naar een video die zijn versleuteld met algemene versleuteling (PlayReady en/of Widevine) afspelen, kunt u de [Azure Media Player](http://amsplayer.azurewebsites.net/azuremediaplayer.html). Wanneer de app console actief is, wordt de inhoud sleutel-ID en de URL bestandenlijst gekopieerd.

1.  Open een nieuw tabblad en uw STS starten: http://[yourStsName].azurewebsites.net/api/token/assetid/[yourCastLabsAssetId]/contentkeyid/[thecontentkeyid].
2.  Ga naar [Azure MediaPlayer](http://amsplayer.azurewebsites.net/azuremediaplayer.html).
3.  Plak de URL van de streaming.
4.  Klik op het selectievakje **Geavanceerde opties** .
5.  Selecteer in de vervolgkeuzelijst **beveiliging** PlayReady en/of Widevine.
6.  Plak de token die u hebt ontvangen van uw STS in het tekstvak Token. 
    
    De licentieserver castLab hoeft niet de ' vruchtdragende = "voorvoegsel vóór het token. Dus verwijder die voordat u het token verzendt.
7.  Werk de speler.
8.  De video moet worden afgespeeld.


##<a name="media-services-learning-paths"></a>Media Services leerpaden

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Feedback geven

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]
