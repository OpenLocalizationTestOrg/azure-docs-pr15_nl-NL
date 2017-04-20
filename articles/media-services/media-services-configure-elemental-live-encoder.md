<properties 
    pageTitle="De encoder elementaire Live als u wilt verzenden van een live één bitsnelheid configureren | Microsoft Azure" 
    description="Dit onderwerp wordt uitgelegd hoe u de encoder elementaire Live een één bitsnelheid om naar te verzenden AMS kanalen die zijn ingeschakeld voor het coderen van live configureert." 
    services="media-services" 
    documentationCenter="" 
    authors="cenkdin" 
    manager="erikre" 
    editor=""/>

<tags 
    ms.service="media-services" 
    ms.workload="media" 
    ms.tgt_pltfrm="na" 
    ms.devlang="ne" 
    ms.topic="article" 
    ms.date="10/12/2016"
    ms.author="cenkdin;anilmur;juliako"/>

#<a name="use-the-elemental-live-encoder-to-send-a-single-bitrate-live-stream"></a>Een live één bitsnelheid verzenden via de encoder elementaire Live

> [AZURE.SELECTOR]
- [Vorm van het element Live](media-services-configure-elemental-live-encoder.md)
- [Tricaster](media-services-configure-tricaster-live-encoder.md)
- [Wirecast](media-services-configure-wirecast-live-encoder.md)
- [FMLE](media-services-configure-fmle-live-encoder.md)

Dit onderwerp wordt uitgelegd hoe u de encoder [Elementaire Live](http://www.elementaltechnologies.com/products/elemental-live) een één bitsnelheid om naar te verzenden AMS kanalen die zijn ingeschakeld voor het coderen van live configureert.  Zie [werken met kanalen die voor het uitvoeren van Live coderen met Azure Media Services geschikt zijn](media-services-manage-live-encoder-enabled-channels.md)voor meer informatie.

Deze zelfstudie leert hoe u het beheren van Azure Media Services (AMS) via Azure Media Services Explorer (AMSE) hulpmiddel. Dit hulpmiddel kan alleen worden uitgevoerd op Windows-PC. Als u op de Mac of Linux, gebruikt u de Azure-portal maken van [kanalen](media-services-portal-creating-live-encoder-enabled-channel.md#create-a-channel) en [programma's](media-services-portal-creating-live-encoder-enabled-channel.md#create-and-manage-a-program).

##<a name="prerequisites"></a>Vereisten voor

- Moet eerder hebt gewerkt van het gebruik van elementaire Live web-interface voor het maken van live gebeurtenissen.
- [Maak een Azure Media Services-account](media-services-portal-create-account.md)
- Controleer of er een Streaming-eindpunt met ten minste één streaming eenheid toegewezen. Zie [Streaming eindpunten in een Media Services-Account beheren](media-services-portal-manage-streaming-endpoints.md)voor meer informatie.
- Installeer de meest recente versie van het hulpprogramma [AMSE](https://github.com/Azure/Azure-Media-Services-Explorer) .
- Het hulpmiddel starten en verbinding maken met uw account AMS.

##<a name="tips"></a>Tips

- Indien mogelijk, gebruikt u een internetverbinding ' hardwired '.
- Een goede vuistregel bij het bepalen van bandbreedtevereisten is aan de streaming bitsnelheid Dubbelklik. Dit is niet verplicht vereist, wordt het helpen om het effect van overbezet netwerk.
- Wanneer u met behulp van software gebaseerd encoders, sluit u uit alle onnodige programma's.

## <a name="elemental-live-with-rtp-ingest"></a>Elementaire Live met RTP nemen

In dit gedeelte ziet hoe u de elementaire Live encoder die een live één bitsnelheid via RTP verzendt configureren.  Zie [MPEG-TS stream via RTP](media-services-manage-live-encoder-enabled-channels.md#channel)voor meer informatie.

### <a name="create-a-channel"></a>Een kanaal maken

1.  Ga naar het tabblad **Live** in het hulpmiddel AMSE en klik met de rechtermuisknop in het gebied voor het kanaal. Selecteer **maken kanaal...** in het menu.

![Elementaire](./media/media-services-elemental-live-encoder/media-services-elemental1.png)

2. Geef een kanaalnaam op, het beschrijvingsveld is optioneel. Kanaalinstellingen, selecteer onder **standaard** voor de optie Live coderen met het invoer-Protocol dat is ingesteld op **RTP (MPEG-TS)**. Laat u alle andere instellingen zoals is.


Controleer of dat het **starten van het nieuwe kanaal nu** is geselecteerd.

3. Klik op **kanaal maken**.
![Elementaire](./media/media-services-elemental-live-encoder/media-services-elemental12.png)

>[AZURE.NOTE] Het kanaal kan duren zolang 20 minuten om te beginnen.

Terwijl het kanaal wordt gestart, kunt u [de encoder configureren](media-services-configure-elemental-live-encoder.md#configure_elemental_rtp).

>[AZURE.IMPORTANT] Houd er rekening mee dat facturering wordt gestart zodra kanaal in gereedheid gaat. Zie voor meer informatie [van kanaal Staten](media-services-manage-live-encoder-enabled-channels.md#states).

###<a id=configure_elemental_rtp></a>De encoder elementaire Live configureren 

In deze zelfstudie gebruikt de volgende uitvoerinstellingen. De rest van deze sectie wordt configuratiestappen uitgebreider beschreven. 

**Video**:
 
- Codec: H.264 
- Profiel: Hoog (niveau 4.0) 
- Bitsnelheid: 5000 k 
- Sleutelframe: 2 seconden ingedrukt (60 seconden) 
- Tarief kader: 30
 
**Audio**:

- Codec: AAC (LC) 
- Bitsnelheid: 192 k 
- Tarief van de steekproef: 44,1 kHz


####<a name="configuration-steps"></a>Volgende configuratiestappen uit

1. Navigeer naar de **Elementaire Live** web-interface en de encoder voor **UDP/TS** streaming instellen. 

2. Nadat een nieuwe gebeurtenis is gemaakt, schuif omlaag naar de uitvoer-groepen en voeg de **UDP/TS** uitvoer-groep. 

3. Maak een nieuwe uitvoer door **nieuwe Stream** selecteren en vervolgens te klikken op **Uitvoer toevoegen**.  
    
    ![Elementaire](./media/media-services-elemental-live-encoder/media-services-elemental13.png)
    
    >[AZURE.NOTE] Het wordt aanbevolen dat de elementaire gebeurtenis de tijdcode ingesteld op 'Systeemklok' om te helpen de encoder opnieuw verbinding te maken bij een fout stream heeft.

4. Nu de uitvoer is gemaakt, klikt u op **Stream toevoegen**. De instellingen voor uitvoer kunnen nu worden geconfigureerd. 
5. Schuif omlaag naar de "Stream 1' die is gemaakt, klik op het tabblad **Video** aan de linkerkant en vouwen van de sectie **Geavanceerd** -instellingen. 

    ![Elementaire](./media/media-services-elemental-live-encoder/media-services-elemental4.png)

    Hoewel elementaire Live een groot aantal beschikbare aan te passen heeft, worden de volgende instellingen worden aanbevolen voor aan de slag met streaming naar AMS. 
    
    - Oplossing: 1280 x 720 
    - Framesnelheid: 30 
    - GOP grootte: 60 frames 
    - Modus interliniëren: geleidelijke 
    - Bitsnelheid: 5000000 bits/s (dit kan worden aangepast op basis van netwerkbeperkingen) 
    

    ![Elementaire](./media/media-services-elemental-live-encoder/media-services-elemental5.png)

6. Invoer-URL van het kanaal krijgen.
    
    Ga terug naar het hulpmiddel AMSE en controleren of de voltooiingsstatus kanaal. Zodra de status is gewijzigd van **starten** **uitgevoerd**, kunt u de invoer-URL kunt krijgen.
      
    Als het kanaal actief is, klik met de rechtermuisknop op de kanaalnaam, navigeer naar beneden af op hover op **Invoer-URL kopiëren naar het Klembord** en selecteer vervolgens **Primaire invoer-URL**.  
    
    ![Elementaire](./media/media-services-elemental-live-encoder/media-services-elemental6.png)
    
1. Plak deze informatie in het veld **Primaire doel** van de vorm van het element. Alle andere instellingen kunnen blijven de standaardwaarde.
    
    ![Elementaire](./media/media-services-elemental-live-encoder/media-services-elemental14.png)

    Voor extra redundantie, herhaalt u deze stappen de secundaire invoer-URL door te maken van een tabblad dat afzonderlijk "Uitvoer" voor UDP/TS Streaming.
    
7. Klik op **maken** (als u een nieuwe gebeurtenis is gemaakt) of **bijwerken** (als het bewerken van een bestaande gebeurtenis) en gaat u verder met het starten van de encoder. 

>[AZURE.IMPORTANT]Voordat u op **starten** op de elementaire Live web-interface, u **moet** ervoor zorgen dat het kanaal klaar is. 
>Zorg er ook niet te laat u het kanaal in gereedheid zonder een gebeurtenis langer dan > 15 minuten.

Nadat de stream actief geweest 30 seconden, Ga terug naar het AMSE hulpmiddel en test afspelen.  

###<a name="test-playback"></a>Test afspelen
  
1. Ga naar de knop AMSE en klik met de rechtermuisknop op het kanaal te testen. Plaats de muisaanwijzer over het **afspelen van het voorbeeld** in het menu en selecteer **met Azure Media Player**.  

    ![Elementaire](./media/media-services-elemental-live-encoder/media-services-elemental8.png)

Als de stream in Windows media player wordt weergegeven, heeft klikt u vervolgens de encoder correct is geconfigureerd als u wilt verbinden met AMS. 

Als een fout wordt ontvangen, het kanaal moeten opnieuw worden ingesteld en instellingen voor encoder aangepast. Zie het onderwerp van de [problemen oplossen](media-services-troubleshooting-live-streaming.md) voor instructies.   

###<a name="create-a-program"></a>Maak een programma

1. Zodra het kanaal afspelen is bevestigd, maakt u een programma. Klik op het tabblad **Live** in het hulpmiddel AMSE Klik met de rechtermuisknop in het gebied voor het programma en selecteer **Nieuwe programma maken**.  

    ![Elementaire](./media/media-services-elemental-live-encoder/media-services-elemental9.png)

2. Naam van het programma en wijzig indien nodig de **Archief venster lengte** (die standaard 4 uur). U kunt ook een opslaglocatie opgeven of laat als het standaardapparaat.  
3. Schakel het selectievakje **nu het programma te starten** .
4. Klik op **programma maken**.  
  
    Opmerking: Programma maken sneller dan kanaal maken.    
 
5. Wanneer het programma wordt uitgevoerd, bevestig afspelen door te klikken met de rechtermuisknop op het programma en navigeren naar **afspelen de programma's** en vervolgens te selecteren **met Azure Media Player**.  
6. Zodra bevestigd, met de rechtermuisknop op het programma opnieuw en selecteert u **de uitvoer-URL naar het Klembord kopiëren** (of deze gegevens ophalen met de optie **programma-informatie en instellingen** in het menu). 

De stream is nu klaar om te worden ingesloten in een speler of verdeeld over een doelgroep voor persoonlijke weergave.  

## <a name="troubleshooting"></a>Problemen oplossen

Zie het onderwerp van de [problemen oplossen](media-services-troubleshooting-live-streaming.md) voor instructies. 


##<a name="media-services-learning-paths"></a>Media Services leerpaden

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Feedback geven

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]
