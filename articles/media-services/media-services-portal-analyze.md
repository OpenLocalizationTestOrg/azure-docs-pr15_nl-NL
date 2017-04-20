<properties
    pageTitle="Uw media met behulp van de Azure portal analyseren | Microsoft Azure"
    description="In dit onderwerp wordt beschreven hoe uw media met Media-analyses media processors (MPs) met behulp van de Azure portal verwerken."
    services="media-services"
    documentationCenter=""
    authors="Juliako"
    manager="erikre"
    editor=""/>

<tags
    ms.service="media-services"
    ms.workload="media"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/24/2016"
    ms.author="juliako"/>


# <a name="analyze-your-media-using-the-azure-portal"></a>Uw media met behulp van de Azure portal analyseren

> [AZURE.NOTE] Als u wilt deze zelfstudie hebt voltooid, moet u een Azure-account. Zie [Azure gratis proefversie](https://azure.microsoft.com/pricing/free-trial/)voor meer informatie. 

## <a name="overview"></a>Overzicht

Azure Media Services Analytics is een verzameling spraak en visuele onderdelen (aan de schaal van de onderneming, naleving, beveiliging en wereldwijde bereik) die het gemakkelijker voor bedrijven en ondernemingen om te leiden sneller inzicht krijgen in hun videobestanden. Zie voor meer gedetailleerde overzicht van Azure Media Services Analytics [in dit](media-services-analytics-overview.md) onderwerp. 

In dit onderwerp wordt beschreven hoe uw media met Media-analyses media processors (MPs) met behulp van de Azure portal verwerken. Media Analytics MPs produceren MP4-bestanden of JSON-bestanden. Als een Mediaprocessor geproduceerd een MP4-bestand, kunt u het bestand geleidelijk downloaden. Als een Mediaprocessor geproduceerd een JSON-bestand, kunt u het bestand downloaden van de Azure-blobopslag. 

## <a name="choose-an-asset-that-you-want-to-analyze"></a>Kies een actief dat u wilt analyseren 
 
1. Selecteer in de [portal van Azure](https://portal.azure.com/)uw Azure Media Services-account.
2. Selecteer in het venster **instellingen van** **activa**.  
.
    ![Video's te analyseren](./media/media-services-portal-analyze/media-services-portal-analyze001.png)

2. Selecteer het element dat u wilt analyseren en druk op de knop **analyseren** .
        
    ![Video's te analyseren](./media/media-services-portal-analyze/media-services-portal-analyze002.png)

3. Selecteer de processor in het venster **media-activa proces met Media-analyses** . 

    De rest van het artikel wordt uitgelegd waarom en het gebruik van elke processor. 
   
4. Druk **maken** een taak naar het begin op.

## <a name="azure-media-indexer"></a>Azure Media indexering

De processor van **Azure Media indexering** media kunt u media-bestanden en inhoud doorzoekbaar maken, evenals gesloten closed captioning nummers genereren. In deze sectie bevat enkele details over opties die u voor deze MP opgeven kunt.

![Video's te analyseren](./media/media-services-portal-analyze/media-services-portal-analyze003.png)

### <a name="language"></a>Taal

De natuurlijke taal worden herkend in het multimediabestand. Bijvoorbeeld Engels of Spaans. 

### <a name="captions"></a>Bijschriften

U kunt een bijschrift-indeling die worden gegenereerd van uw inhoud. Een taak indexing kunt ondertiteling bestanden genereren in de volgende indelingen:  

- **Sami**
- **TTML**
- **WebVTT**

Gesloten bijschrift (CC) bestanden in de volgende indelingen kunnen worden gebruikt om audio-en videobestanden toegankelijk te maken voor personen met de disability horen.

### <a name="aib-file"></a>AIB-bestand

Selecteer deze optie als u wilt de Audio Index Blob-bestand voor gebruik met de aangepaste SQL Server-IFilter genereren. Zie voor meer informatie [in dit](https://azure.microsoft.com/blog/using-aib-files-with-azure-media-indexer-and-sql-server/) blog.

### <a name="keywords"></a>Trefwoorden

Selecteer deze optie als u wilt laten maken van een XML-bestand van trefwoorden. Dit bestand bevat trefwoorden opgehaald uit de spraak-inhoud, met frequentie en verschuiving gegevens.

### <a name="job-name"></a>De naam van de taak

Een beschrijvende naam waarmee u de taak te identificeren. [In dit](media-services-portal-check-job-progress.md) artikel wordt beschreven hoe u de voortgang van een taak kan controleren. 

### <a name="output-file"></a>Uitvoerbestand

Een beschrijvende naam waarmee u de inhoud van de uitvoer identificeren. 

## <a name="azure-media-hyperlapse"></a>Azure Media Hyperlapse

Azure Media Hyperlapse is een MP die vloeiend verstreken tijd video's worden gemaakt van de eerste persoon of actie-camera-inhoud.  Zie voor meer informatie [in dit](media-services-hyperlapse-content.md) onderwerp. In deze sectie bevat enkele details over opties die u voor deze MP opgeven kunt.

![Video's te analyseren](./media/media-services-portal-analyze/media-services-portal-analyze004.png)

### <a name="speed"></a>Snelheid 

Geef de snelheid waarmee u de invoer video wilt versnellen. De uitvoer is een constante en verstreken tijd weergave van de video invoer.

### <a name="job-name"></a>De naam van de taak

Een beschrijvende naam waarmee u de taak te identificeren. [In dit](media-services-portal-check-job-progress.md) artikel wordt beschreven hoe u de voortgang van een taak kan controleren. 

### <a name="output-file"></a>Uitvoerbestand

Een beschrijvende naam waarmee u de inhoud van de uitvoer identificeren. 

## <a name="azure-media-face-detector"></a>Azure Media nominale detectie

De **Azure Media nominale detectie** Mediaprocessor (MP) kunt u tellen, verplaatsingen bijhouden en zelfs meter deelname aan de doelgroep en reactie via analyseert expressies. Deze service bevat twee functies: 

- **Nominale detectie**

    Nominale detectie worden gevonden en wordt bijgehouden menselijke vlakken binnen een video. Meerdere vlakken kunnen worden gedetecteerd en vervolgens worden bijgehouden terwijl ze, met de metagegevens van de tijd en locatie geretourneerd door een JSON-bestand bewegen. Tijdens bijhouden wordt geprobeerd een consistente ID toekennen aan de dezelfde nominale terwijl de persoon wordt navigeren op scherm, zelfs als ze zijn ondervindt hinder van obstakels of kort het frame laat.

    >[AZURE.NOTE]Deze services geen gelaatsherkenning uitgevoerd. Een persoon die het frame verlaat of ondervindt voor verandert hinder van obstakels te lang krijgt een nieuwe ID wanneer ze terugkeren.

- **Emoties detectie**
    
    Emoties detectie is een optioneel onderdeel van het nominale detectie Media Processor die resulteert in analyse op meerdere emotionele kenmerken van de vlakken aangetroffen, inclusief gelukkig, sadness, bang, volgt uitzien en meer. 

![Video's te analyseren](./media/media-services-portal-analyze/media-services-portal-analyze005.png)

### <a name="detection-mode"></a>Modus voor detectie

Een van de volgende modi kan worden gebruikt door de processor:

- nominale detectie
- per nominale emoties detectie
- statistische emoties detectie

### <a name="job-name"></a>De naam van de taak

Een beschrijvende naam waarmee u de taak te identificeren. [In dit](media-services-portal-check-job-progress.md) artikel wordt beschreven hoe u de voortgang van een taak kan controleren. 

### <a name="output-file"></a>Uitvoerbestand

Een beschrijvende naam waarmee u de inhoud van de uitvoer identificeren. 

## <a name="azure-media-motion-detector"></a>Azure Media beweging detectie

De **Azure Media beweging detectie** Mediaprocessor (MP) kunt u secties belangrijke binnen een video anders lange en probleemloze efficiënt te identificeren. Beweging detectie kan worden gebruikt op statische camera-opnamen om aan te geven van gedeelten van de video waar beweging is opgetreden. Een JSON-bestand met een metagegevens met tijdstempels en het selectiegebied waarop de gebeurtenis heeft plaatsgevonden wordt gegenereerd.

Deze technologie is gericht op video-feeds voor een waardepapier, mogen beweging verdeelt in relevante gebeurtenissen en onrechte zoals schaduwen en belichtingswijzigingen. Hiermee kunt u beveiligingsmeldingen vanuit camera-feeds zonder spam met eindeloos niet relevant gebeurtenissen, kunnen worden geëxtraheerd momenten belangrijke in erg lang toezicht video's te genereren.

![Video's te analyseren](./media/media-services-portal-analyze/media-services-portal-analyze006.png)

## <a name="azure-media-video-thumbnails"></a>Azure Media Video miniaturen

Deze processor kunt u samenvattingen van lange video's door te selecteren automatisch interessante fragmenten uit de bronvideo maken. Dit is handig als u voorzien van een kort overzicht van wat u wilt kunt verwachten in een lange video. Zie voor gedetailleerde informatie en voorbeelden [Azure Media Video miniaturen gebruiken om te maken van een Video-samenvatting](media-services-video-summarization.md)

![Video's te analyseren](./media/media-services-portal-analyze/media-services-portal-analyze008.png)

### <a name="job-name"></a>De naam van de taak

Een beschrijvende naam waarmee u de taak te identificeren. [In dit](media-services-portal-check-job-progress.md) artikel wordt beschreven hoe u de voortgang van een taak kan controleren. 

### <a name="output-file"></a>Uitvoerbestand

Een beschrijvende naam waarmee u de inhoud van de uitvoer identificeren. 


##<a name="next-steps"></a>Volgende stappen

Bekijk Media Services leerpaden.

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Feedback geven

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]


