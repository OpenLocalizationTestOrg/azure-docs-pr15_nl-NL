<properties 
    pageTitle="StorSimple indicatoren | Microsoft Azure" 
    description="Beschrijft de luminescentiedioden (LED's) en de hoorbare signalen gebruikt voor het controleren van de status van het apparaat StorSimple."
    services="storsimple"
    documentationCenter="NA"
    authors="alkohli"
    manager="carmonm"
    editor="" />
 <tags 
    ms.service="storsimple"
    ms.devlang="NA"
    ms.topic="article"
    ms.tgt_pltfrm="NA"
    ms.workload="TBD"
    ms.date="08/18/2016"
    ms.author="alkohli" />

# <a name="use-storsimple-monitoring-indicators-to-manage-your-device"></a>StorSimple indicatoren voor het beheren van uw apparaat gebruiken   

## <a name="overview"></a>Overzicht

Uw apparaat StorSimple bevat Lichtdioden (LED's) en alarmen die u gebruiken kunt om de modules en de algemene status van het apparaat StorSimple te bewaken. De controle indicatoren vindt u op de hardware-onderdelen van de primaire ruimte van het apparaat en de ruimte EBOD. De controle indicatoren kunnen LED's of hoorbare alarmen zijn.

Er zijn drie LED Staten gebruikt om aan te geven van de status van een module: groen, groen, rood-geel of rood oranje knipperende.  

- Groen LED's bevatten een in orde, operationele status.  
- Knipperende groen naar rood oranje LED's vertegenwoordigen de aanwezigheid van niet-kritieke voorwaarden die mogelijk tussenkomst van de gebruiker.  
- Rood oranje LED's wordt aangegeven dat er een kritieke foutenstructuuranalyse aanwezig zijn in de module.  

De rest van dit artikel worden de verschillende controleren indicator LED's, de locaties op het apparaat StorSimple, de apparaatstatus is gebaseerd op het LED-Staten en alle bijbehorende hoorbare alarmen.

## <a name="front-panel-indicator-leds"></a>Deelvenster LED voren

Het front deelvenster, ook bekend als de *bewerkingen Configuratiescherm* of *ops deelvenster*, wordt de statistische status van alle modules weergegeven in het systeem. Het deelvenster aan de voorgrond is gelijk aan de primaire StorSimple en de ruimte EBOD en hieronder wordt geïllustreerd.  

   ![Apparaat instellen][1]
 
Het deelvenster aan de voorzijde bevat de volgende punten:  

1. Knop Dempen
2. Power indicator LED (rood-groen-geel)
3. Module foutenstructuuranalyse indicator onder leiding van een (op rood-oranje/uit)
4. Logische foutenstructuuranalyse indicator onder leiding van een (op rood-oranje/uit
5. Eenheid-ID weergeven  

Het belangrijkste verschil tussen de voorste Configuratiescherm LED's voor het apparaat en referenties voor de ruimte EBOD is het **Systeem eenheid Identification Number** weergegeven op het LED-scherm. De ID van de standaard-eenheid weergegeven op het apparaat is **00**, terwijl de ID van de standaard-eenheid weergegeven op de ruimte EBOD **01**. Hiermee kunt u snel onderscheid maken tussen het apparaat en de ruimte EBOD wanneer het apparaat is ingeschakeld. Als uw apparaat is uitgeschakeld, gebruikt u de informatie in het [inschakelen van een nieuwe apparaat](storsimple-turn-device-on-or-off.md#turn-on-a-new-device) kunt onderscheiden van het apparaat van de ruimte EBOD.  

## <a name="front-panel-led-status"></a>Voorste Configuratiescherm LED-status  

Gebruik de volgende tabel om aan te geven van de status die wordt aangegeven door het LED's op de voorgrond deelvenster voor het apparaat dat of de insluiting EBOD.  

|Systeem power | Module foutenstructuuranalyse | Logische foutenstructuuranalyse | Waarschuwing | Status|
|-------------|---------------|-----------------|-------|-------|
|Rood-oranje | UITSCHAKELEN     | UITSCHAKELEN | N/B | AC power verloren zijn gegaan, op back-up power of AC power op werkzaam en de controller modules zijn verwijderd.|
|Groen | AAN | AAN | N/B | OPS Configuratiescherm power op (5s) staat testen|
|Groen | UITSCHAKELEN | UITSCHAKELEN | N/B | Power op alle functies van goede|
|Groen | AAN |N/B | PCM foutenstructuuranalyse LED's, ventilator foutenstructuuranalyse LED 's | Een PCM foutenstructuuranalyse, ventilator foutenstructuuranalyse, boven of onder temperatuur|
| Groen | AAN | N/B | I/o-module LED 's  | Een controller module foutenstructuuranalyse|
| Groen | AAN | N/B | N/B | Insluiting logica foutenstructuuranalyse|
| Groen | Flash | N/B | Status van de module onder leiding van een op controllermodule. PCM foutenstructuuranalyse LED's, ventilator foutenstructuuranalyse LED 's | Type van onbekende domeincontroller module hebt geïnstalleerd, I2C bus mislukt, controller module belangrijke product data (VPD)-configuratiefout |

## <a name="power-cooling-module-pcm-indicator-leds"></a>Power koeling module (PCM) LED   

Power koeling module (PCM) LED vindt u op de achtergrond van de primaire insluiting of EBOD insluiting in elke module PCM. In dit onderwerp wordt beschreven hoe de volgende LED's gebruiken om te controleren van de status van uw apparaat StorSimple.  

- PCM LED's voor de primaire insluiting
- PCM LED's voor de ruimte EBOD

## <a name="pcm-leds-for-the-primary-enclosure"></a>PCM LED's voor de primaire insluiting  

Het apparaat StorSimple heeft een 764W PCM module met een extra kleine. De volgende afbeelding ziet u het deelvenster LED voor het apparaat.  

   ![PCM LED's op de primaire insluiting][2]

LED legenda:

1. AC power is mislukt
2. Ventilator is mislukt
3. Kleine foutenstructuuranalyse
4. PCM OK
5. Domeincontroller is mislukt
6. Kleine goede  

De status van de PCM wordt aangegeven in het deelvenster LED. Het apparaat PCM LED-deelvenster heeft zes LED's. Vier van de volgende LED's weergeven de status van de voeding en de ventilator. De resterende twee LED's geven de status van de back-up kleine module in het PCM. U kunt de volgende tabellen gebruiken om te bepalen de status van de PCM.  

### <a name="pcm-indicator-leds-for-power-supply-and-fan"></a>PCM LED voor voeding en ventilator
| Status | PCM OK (groen) | AC fail (geel) | Ventilator fail (geel) | Domeincontroller fail (geel) |
|--------|----------------|-----------------------|------------------|----------------------|
| Geen power AC (naar insluiting) | UITSCHAKELEN | UITSCHAKELEN | UITSCHAKELEN | UITSCHAKELEN|
| Geen AC power (alleen deze PCM) | UITSCHAKELEN | AAN | UITSCHAKELEN | AAN |
| AC presenteren aan PCM - OK     | AAN | UITSCHAKELEN | UITSCHAKELEN | UITSCHAKELEN |
| PCM fail (ventilator fail) | UITSCHAKELEN | UITSCHAKELEN | AAN | N/B |
| PCM foutenstructuuranalyse (via amp, via spanning, via de huidige) | UITSCHAKELEN | AAN | AAN | AAN |
| PCM (ventilator afmelden bij tolerantie) | AAN | UITSCHAKELEN | UITSCHAKELEN | AAN |
| Modus Stand-by | Knipperende | UITSCHAKELEN | UITSCHAKELEN | UITSCHAKELEN |
| PCM firmware downloaden | UITSCHAKELEN | Knipperende | Knipperende | Knipperende |

### <a name="pcm-indicator-leds-for-the-backup-battery"></a>PCM LED voor de back-up kleine  

| Status | Kleine goed (groen) | Kleine foutenstructuuranalyse (oranje) |
|--------|----------------------|-----------------------|
| Kleine niet aanwezig | UITSCHAKELEN | UITSCHAKELEN |
| Kleine presenteren en opgeladen | AAN | UITSCHAKELEN |
| Kleine opladen of onderhoud nakoming | Knipperende | UITSCHAKELEN |
| Kleine "zachte" fout (herstelbare) | UITSCHAKELEN | Knipperende |
| Kleine "harde" fout (niet-herstelbare) | UITSCHAKELEN | AAN |
| Kleine disarmed | Knipperende | UITSCHAKELEN |

## <a name="pcm-leds-for-the-ebod-enclosure"></a>PCM LED's voor de ruimte EBOD  

De ruimte EBOD heeft een PCM 580 w bij en geen extra kleine. Het deelvenster PCM voor de ruimte EBOD heeft indicator LED's alleen voor de voorraad power en de ventilator. De volgende afbeelding ziet u deze LED's.

   ![PCM LED's op de ruimte EBOD][3] 
 
U kunt de volgende tabel om te bepalen de status van de PCM.  

| Status | PCM OK (groen) | AC fail (geel) | Ventilator fail (geel) | Domeincontroller fail (geel) |
|--------|---------------|------------------------|------------------|----------------------|
| Geen power AC (naar insluiting) | UITSCHAKELEN | UITSCHAKELEN | UITSCHAKELEN | UITSCHAKELEN |
| Geen AC power (alleen deze PCM) | UITSCHAKELEN | AAN | UITSCHAKELEN | AAN |
| AC presenteren aan PCM – OK | AAN | UITSCHAKELEN | UITSCHAKELEN | UITSCHAKELEN |
| PCM fail (ventilator fail) | UITSCHAKELEN | UITSCHAKELEN | AAN | X |
| PCM foutenstructuuranalyse (via amp, via spanning, via de huidige | UITSCHAKELEN | AAN | AAN | AAN |
| PCM (ventilator afmelden bij tolerantie) | AAN | UITSCHAKELEN | UITSCHAKELEN | AAN |
| Stand-by model | Knipperende | UITSCHAKELEN | UITSCHAKELEN | UITSCHAKELEN |
| PCM firmware downloaden | UITSCHAKELEN | Knipperende | Knipperende | Knipperende |

## <a name="controller-module-indicator-leds"></a>Controller-module LED  

Het apparaat StorSimple bevat LED's voor de primaire controller en de EBOD controller modules.   

### <a name="monitoring-leds-for-the-primary-controller"></a>LED's voor de primaire controller bewaken
De volgende afbeelding kunt u de LED's op de primaire controller herkennen. (Alle onderdelen staan in de afdrukstand steun.)  

   ![LED's - primaire controller bewaken][4]
 
Gebruik de volgende tabel om te bepalen of de controllermodule correct functioneert.  

### <a name="controller-indicator-leds"></a>Controller LED  

| LED | Beschrijving                                                                            
|---- | ----------- |
| ID LED (blauw) | Geeft aan dat de module is worden geïdentificeerd. Als de blauwe LED is knipperende op een actieve controller, klikt u vervolgens de controller uit de actieve controller en de andere waarde is de stand-by controller. Zie [identificeren de actieve controller op uw apparaat](storsimple-controller-replacement.md#identify-the-active-controller-on-your-device)voor meer informatie. |
| Foutenstructuuranalyse LED (oranje) | Geeft een fout in de controller uit.        
| OK LED (groen) | Constante groene geeft aan dat de controller is het OK. Knipperende groen, wordt een controller VPD configuratiefout aangegeven. |
| SA's activiteit LED's (groen) | Constante groene geeft aan een verbinding met geen huidige activiteiten. Knipperende groen, geeft aan dat de verbinding heeft lopende activiteit. |
| Ethernet status LED 's | Rechts geeft koppeling/netwerk activiteit: (constante groen) koppeling actief (knipperende groen) activiteit netwerk. Klik links geeft Netwerksnelheid: (geel) 1000 Mb/s (groen) 100 Mb/s en (uit) 10 Mb/s. Afhankelijk van het Onderdeelmodel, worden deze light mogelijk knipperen zelfs als de netwerkinterface is niet ingeschakeld. |
| BERICHT LED 's | Geeft de voortgang opstarten wanneer de controller is ingeschakeld. Als het apparaat StorSimple mislukt om op te starten, helpt dit LED Microsoft Support identificeren van het punt in het opstarten waarop de fout is opgetreden. |

>[AZURE.IMPORTANT] 
Als de fout LED licht, is er een probleem met de controllermodule die mogelijk worden opgelost door opnieuw te starten de controller uit. Neem contact op met Microsoft Support als dit probleem niet is verholpen wanneer u opnieuw starten van de controller uit.  


### <a name="monitoring-leds-for-the-ebod-ebod-enclosure"></a>Controle-LED's voor de EBOD (EBOD insluiting)  

Elke de 6 Gb/s SAS EBOD bevat LED's die de status ervan aangeven, zoals wordt weergegeven in de volgende afbeelding.  

  ![LED's - EBOD insluiting bewaken][5]

Gebruik de volgende tabel om te bepalen of de EBOD controller-module normaal werkt.  

### <a name="ebod-controller-module-indicator-leds"></a>De LED voor de EBOD controller-module  

|Status | I/o-module OK (groen) | I/o-module foutenstructuuranalyse (oranje) | Host poortactiviteit (groen) |
|-------|----------------------|-------------------------------|----------------------------|
| Controllermodule OK | AAN | UITSCHAKELEN | - |
| Controller module foutenstructuuranalyse | UITSCHAKELEN | AAN | - |
| Geen verbinding met externe host poort | - | - | UITSCHAKELEN |
| Externe host poortverbinding – geen activiteit | - | - | AAN |
| Externe host poortverbinding - activiteit | - | - | Knipperende |
| Controller module metagegevens fout | Knipperende | - | - |

## <a name="disk-drive-indicator-leds-for-the-primary-enclosure-and-ebod-enclosure"></a>Schijf LED voor de primaire insluiting en EBOD insluiting

Het apparaat StorSimple heeft schijfstations bevinden in zowel de primaire ruimte en de ruimte EBOD. Elke schijf met monitoring indicator LED's, zoals is beschreven in deze sectie. 

Voor de schijfstations, de stationsstatus wordt aangegeven door een groene LED en een rood oranje LED bevestigd aan de voorzijde van elke station carrier-module. De volgende afbeelding ziet u deze LED's.

  ![Schijf LED 's][6]
 
Gebruik de volgende tabel om de status van elke schijf, die op zijn beurt van invloed op het algemene voorste deelvenster LED status te bepalen.  

### <a name="disk-drive-indicator-leds-for-the-ebod-enclosure"></a>Schijf LED voor de ruimte EBOD  

| Status | Activiteit OK LED (groen) | Foutenstructuuranalyse LED (rood-geel) | Dat is gekoppeld ops Configuratiescherm LED |
|-------|--------------------------|----------------------|-------------------------|
| Geen station hebt geïnstalleerd | UITSCHAKELEN | UITSCHAKELEN | Geen |
| Station geïnstalleerd en operationele | Knipperende aan/uit met activiteit | X | Geen |
| SE-Services (SCSI insluiting's) apparaat identiteit instellen | AAN | Knipperende 1 seconde op/1 tweede uitschakelen | Geen |
| SE apparaat foutenstructuuranalyse bits instellen | AAN | AAN | Logische foutenstructuuranalyse (rood) |
| Fout in Power besturingselement | UITSCHAKELEN | AAN | Module foutenstructuuranalyse (rood) |

## <a name="audible-alarms"></a>Hoorbare alarmen  

Een apparaat StorSimple bevat hoorbare alarmen die is gekoppeld aan de primaire insluiting zowel de EBOD insluiting. Een hoorbare waarschuwing bevindt zich op de voorgrond deelvenster (ook wel bekend als in het deelvenster ops) van beide bijlagen. De hoorbare waarschuwing geeft aan wanneer een voorwaarde foutenstructuuranalyse aanwezig is. De volgende voorwaarden wordt de waarschuwing activeren:  

- Ventilator foutenstructuuranalyse of mislukt
- Spanning buiten het bereik
- Boven of onder temperatuur voorwaarde
- Thermische overloop
- Systeem foutenstructuuranalyse
- Logische foutenstructuuranalyse
- Power levering foutenstructuuranalyse
- Verwijderen van een macht koeling module (PCM)  

De volgende tabel beschrijft de verschillende waarschuwing Staten.  

### <a name="alarm-states"></a>Waarschuwing Staten  

| Waarschuwing staat | Actie | Actie met de knop Dempen ingedrukt |
|------------|---------|---------------------------------|
| S0 | Normale modus: stille | Pieptoon, tweemaal |
| S1 | Foutenstructuuranalyse modus: 1 seconde op/1 tweede uitschakelen | Overgang naar S2 of S3 (Zie Opmerkingen) |
| S2 | Modus herinneren: door onregelmatige pieptoon | Geen |
| S3 | Uitgeschakelde modus: stille | Geen |
| S4 | Kritieke foutenstructuuranalyse modus: doorlopend waarschuwing | Niet beschikbaar: dempen niet actief |

> [AZURE.NOTE] 

>  - Waarschuwing status S1, als u niet op dempen binnen 2 minuten, overgangen de status automatisch S2 of S3.  
>  - Waarschuwing Staten S1 naar S4 terug naar S0 nadat de voorwaarde foutenstructuuranalyse is uitgeschakeld.  
>  - Kritieke foutenstructuuranalyse staat S4 kan worden ingevoerd uit een andere staat.  

U kunt de hoorbare waarschuwing dempen door op de knop Dempen in het deelvenster ops te drukken. Automatische dempen gebeurt na twee minuten als het geluid te dempen, niet handmatig wordt beheerd. Wanneer de waarschuwing gedempt, blijft geluid met korte door onregelmatige pieptoon om aan te geven dat het probleem blijft optreden. De waarschuwing niet stille wanneer alle problemen zijn uitgeschakeld.  

De volgende tabel beschrijft de verschillende waarschuwing voorwaarden.  

### <a name="alarm-conditions"></a>Waarschuwing voorwaarden  

| Status | Ernst | Waarschuwing | OPS LED-scherm |
|--------|---------|--------|----------------|
| PCM waarschuwing – verlies van stroom van de domeincontroller vanaf een enkel PCM | Foutenstructuuranalyse – niet kwaliteitsverlies redundantie | S1 | Module foutenstructuuranalyse|
|PCM waarschuwing – verlies van stroom van de domeincontroller vanaf een enkel PCM | Foutenstructuuranalyse – verlies van redundantie | S1 | Module foutenstructuuranalyse |
| PCM ventilator fail | Foutenstructuuranalyse – verlies van redundantie | S1 | Module foutenstructuuranalyse |
| SBB module gedetecteerd PCM foutenstructuuranalyse | Foutenstructuuranalyse | S1 | Module foutenstructuuranalyse |
| PCM verwijderd | Configuratiefout | Geen | Module foutenstructuuranalyse |
| Configuratiefout insluiting | Foutenstructuuranalyse – kritieke | S1 | Module foutenstructuuranalyse |
| Lage temperatuur melding | Waarschuwing | S1 | Module foutenstructuuranalyse |
| Hoge temperatuur melding | Waarschuwing | S1 | Module foutenstructuuranalyse |
| Via de temperatuur | Foutenstructuuranalyse – kritieke | S1 | Module foutenstructuuranalyse |
| I2C bus is mislukt | Foutenstructuuranalyse – verlies van redundantie | S1 | Module foutenstructuuranalyse |
| Het deelvenster OPS communicatiefout (I2C) | Foutenstructuuranalyse – kritieke     | S1 | Module foutenstructuuranalyse |
| Controllerfout | Foutenstructuuranalyse – kritieke | S1 | Module foutenstructuuranalyse |
| SBB interface module foutenstructuuranalyse | Foutenstructuuranalyse – kritieke | S1 | Module foutenstructuuranalyse |
| SBB interface module foutenstructuuranalyse – geen functioneel modules resterende | Foutenstructuuranalyse – kritieke | S4 | Module foutenstructuuranalyse |
| SBB interfacemodule verwijderd | Waarschuwing | Geen | Module foutenstructuuranalyse |
| Station power besturingselement foutenstructuuranalyse | Waarschuwing – niet kwaliteitsverlies station power | S1 | Module foutenstructuuranalyse |
| Station power besturingselement foutenstructuuranalyse | Foutenstructuuranalyse – kritieke; verlies van station power | S1 | Module foutenstructuuranalyse |
| Station verwijderd | Waarschuwing | Geen | Module foutenstructuuranalyse |
| Onvoldoende power beschikbaar | Waarschuwing | geen | Module foutenstructuuranalyse |

## <a name="next-steps"></a>Volgende stappen

Meer informatie over [StorSimple hardwareonderdelen en status](storsimple-monitor-hardware-status.md).

[1]: ./media/storsimple-monitoring-indicators/storsimple-monitoring-indicators-IMAGE01.png
[2]: ./media/storsimple-monitoring-indicators/storsimple-monitoring-indicators-IMAGE02.png
[3]: ./media/storsimple-monitoring-indicators/storsimple-monitoring-indicators-IMAGE03.png
[4]: ./media/storsimple-monitoring-indicators/storsimple-monitoring-indicators-IMAGE04.png
[5]: ./media/storsimple-monitoring-indicators/storsimple-monitoring-indicators-IMAGE05.png
[6]: ./media/storsimple-monitoring-indicators/storsimple-monitoring-indicators-IMAGE06.png

 
