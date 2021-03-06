<properties 
    pageTitle="De encoder Telestream Wirecast als u wilt verzenden van een live één bitsnelheid configureren | Microsoft Azure" 
    description="Dit onderwerp wordt uitgelegd hoe u de Wirecast live encoder een één bitsnelheid om naar te verzenden AMS kanalen die zijn ingeschakeld voor het coderen van live configureert. " 
    services="media-services" 
    documentationCenter="" 
    authors="Juliako" 
    manager="erikre" 
    editor=""/>

<tags 
    ms.service="media-services" 
    ms.workload="media" 
    ms.tgt_pltfrm="na" 
    ms.devlang="ne" 
    ms.topic="article" 
    ms.date="10/12/2016"
    ms.author="juliako;cenkdin;anilmur"/>

#<a name="use-the-wirecast-encoder-to-send-a-single-bitrate-live-stream"></a>Een live één bitsnelheid verzenden via de encoder Wirecast

> [AZURE.SELECTOR]
- [Wirecast](media-services-configure-wirecast-live-encoder.md)
- [Vorm van het element Live](media-services-configure-elemental-live-encoder.md)
- [Tricaster](media-services-configure-tricaster-live-encoder.md)
- [FMLE](media-services-configure-fmle-live-encoder.md)

Dit onderwerp wordt uitgelegd hoe u de [Telestream Wirecast](http://www.telestream.net/wirecast/overview.htm) live encoder een één bitsnelheid om naar te verzenden AMS kanalen die zijn ingeschakeld voor het coderen van live configureert.  Zie [werken met kanalen die voor het uitvoeren van Live coderen met Azure Media Services geschikt zijn](media-services-manage-live-encoder-enabled-channels.md)voor meer informatie.

Deze zelfstudie leert hoe u het beheren van Azure Media Services (AMS) via Azure Media Services Explorer (AMSE) hulpmiddel. Dit hulpmiddel kan alleen worden uitgevoerd op Windows-PC. Als u op de Mac of Linux, gebruikt u de Azure-portal maken van [kanalen](media-services-portal-creating-live-encoder-enabled-channel.md#create-a-channel) en [programma's](media-services-portal-creating-live-encoder-enabled-channel.md#create-and-manage-a-program).


##<a name="prerequisites"></a>Vereisten voor

- [Maak een Azure Media Services-account](media-services-portal-create-account.md)
- Controleer of er een Streaming-eindpunt met ten minste één streaming eenheid toegewezen. Zie voor meer informatie, [Streaming eindpunten beheren in een Media Services-Account](media-services-portal-manage-streaming-endpoints.md)
- Installeer de meest recente versie van het hulpprogramma [AMSE](https://github.com/Azure/Azure-Media-Services-Explorer) .
- Het hulpmiddel starten en verbinding maken met uw account AMS.

##<a name="tips"></a>Tips

- Indien mogelijk, gebruikt u een internetverbinding ' hardwired '.
- Een goede vuistregel bij het bepalen van bandbreedtevereisten is aan de streaming bitsnelheid Dubbelklik. Dit is niet verplicht vereist, wordt het helpen om het effect van overbezet netwerk.
- Wanneer u met behulp van software gebaseerd encoders, sluit u uit alle onnodige programma's.


## <a name="create-a-channel"></a>Een kanaal maken

1.  Ga naar het tabblad **Live** in het hulpmiddel AMSE en klik met de rechtermuisknop in het gebied voor het kanaal. Selecteer **maken kanaal...** in het menu.

![wirecast](./media/media-services-wirecast-live-encoder/media-services-wirecast1.png)

2. Geef een kanaalnaam op, het beschrijvingsveld is optioneel. Kanaalinstellingen, selecteer onder **standaard** voor de optie Live coderen met het invoer-Protocol dat is ingesteld op **RTMP**. Laat u alle andere instellingen zoals is.


Controleer of dat het **starten van het nieuwe kanaal nu** is geselecteerd.

3. Klik op **kanaal maken**.
![wirecast](./media/media-services-wirecast-live-encoder/media-services-wirecast2.png)

>[AZURE.NOTE] Het kanaal kan duren zolang 20 minuten om te beginnen.

Terwijl het kanaal wordt gestart, kunt u [de encoder configureren](media-services-configure-wirecast-live-encoder.md#configure_wirecast_rtmp).

>[AZURE.IMPORTANT] Houd er rekening mee dat facturering wordt gestart zodra kanaal in gereedheid gaat. Zie voor meer informatie [van kanaal Staten](media-services-manage-live-encoder-enabled-channels.md#states).

##<a id=configure_wirecast_rtmp></a>De encoder Telestream Wirecast configureren

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


###<a name="configuration-steps"></a>Volgende configuratiestappen uit

1. Open de toepassing Telestream Wirecast op de computer wordt gebruikt en voor RTMP streaming instellen.
2. De uitvoer configureren door naar het tabblad **uitvoer** navigeren en **Uitvoerinstellingen...**te selecteren.
    
    Controleer of dat de **Bestemming voor de uitvoer** is ingesteld op **RTMP-Server**.
3. Klik op **OK**.
4. Stel **het doelveld moeten **Azure Media Services**** op de pagina-instellingen.
 
    Het profiel codering is vooraf geselecteerde naar **Azure H.264 720 p 16:9 (1280 x 720)**. Als u wilt deze instellingen aanpast, selecteer het tandwielpictogram aan de rechterkant van de vervolgkeuzelijst omlaag en kiest u **Nieuwe vooraf ingestelde**.

    ![wirecast](./media/media-services-wirecast-live-encoder/media-services-wirecast3.png)

5. Vooraf ingestelde encoder configureren.

    Naam van de vooraf ingestelde en controleren van de volgende aanbevolen instellingen:

    **Video**
    
    - Encoder: MainConcept H.264
    - Frames per seconde: 30
    - Gemiddelde bits tarief: 5000 kbits/sec (kan worden aangepast op basis van netwerkbeperkingen)
    - Profiel: Hoofdgegeven
    - Belangrijke frame elke: 60 frames

    **Audio**

    - Tarief van doel-bits: 192 kbits/sec
    - Tarief van de steekproef: 44,100 kHz
     
    ![wirecast](./media/media-services-wirecast-live-encoder/media-services-wirecast4.png)

6. Druk op **Opslaan**.

    Het veld codering bevat nu het nieuwe profiel beschikbaar voor selectie. 

    Controleer of dat het nieuwe profiel is geselecteerd.

7. Invoer-URL van het kanaal krijgen om te kunnen toewijzen aan het Wirecast **RTMP eindpunt**.
    
    Ga terug naar het hulpmiddel AMSE en controleren of de voltooiingsstatus kanaal. Zodra de status is gewijzigd van **starten** **uitgevoerd**, kunt u de invoer-URL kunt krijgen.
      
    Als het kanaal actief is, klik met de rechtermuisknop op de kanaalnaam, navigeer naar beneden af op hover op **Invoer-URL kopiëren naar het Klembord** en selecteer vervolgens **Primaire invoer-URL**.  
    
    ![wirecast](./media/media-services-wirecast-live-encoder/media-services-wirecast6.png)

8. Klik in het venster Wirecast **Uitvoerinstellingen** plakt u deze informatie in het veld **adres** van de sectie uitvoer en de naam van een gegevensstroom toewijzen. 


    ![wirecast](./media/media-services-wirecast-live-encoder/media-services-wirecast5.png)

9. Selecteer **OK**.

10. Bevestig in het hoofdscherm van **Wirecast** invoerbronnen voor video en audio klaar bent en vervolgens op de **Stream** in de bovenste linkerpagina hoek.

    ![wirecast](./media/media-services-wirecast-live-encoder/media-services-wirecast7.png)

>[AZURE.IMPORTANT]Voordat u op **Stream**, u **moet** ervoor zorgen dat het kanaal klaar is. 
>Zorg er ook niet naar het kanaal gereed verlaten zonder een invoer bijdrage feed langer dan > 15 minuten.

##<a name="test-playback"></a>Test afspelen
  
1. Ga naar de knop AMSE en klik met de rechtermuisknop op het kanaal te testen. Plaats de muisaanwijzer over het **afspelen van het voorbeeld** in het menu en selecteer **met Azure Media Player**.  

    ![wirecast](./media/media-services-wirecast-live-encoder/media-services-wirecast8.png)

Als de stream in Windows media player wordt weergegeven, heeft klikt u vervolgens de encoder correct is geconfigureerd als u wilt verbinden met AMS. 

Als een fout wordt ontvangen, het kanaal moeten opnieuw worden ingesteld en instellingen voor encoder aangepast. Zie het onderwerp van de [problemen oplossen](media-services-troubleshooting-live-streaming.md) voor instructies.  

##<a name="create-a-program"></a>Maak een programma

1. Zodra het kanaal afspelen is bevestigd, maakt u een programma. Klik op het tabblad **Live** in het hulpmiddel AMSE Klik met de rechtermuisknop in het gebied voor het programma en selecteer **Nieuwe programma maken**.  

    ![wirecast](./media/media-services-wirecast-live-encoder/media-services-wirecast9.png)

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
