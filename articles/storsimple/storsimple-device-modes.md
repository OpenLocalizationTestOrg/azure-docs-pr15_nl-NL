<properties 
   pageTitle="Wijzigen van de modus StorSimple-apparaat | Microsoft Azure"
   description="De opties voor de StorSimple apparaat beschreven en wordt uitgelegd hoe u Windows PowerShell voor StorSimple voor het wijzigen van de apparatuurmodus gebruiken."
   services="storsimple"
   documentationCenter=""
   authors="alkohli"
   manager="carmonm"
   editor="" />
<tags 
   ms.service="storsimple"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="06/17/2016"
   ms.author="alkohli" />

# <a name="change-the-device-mode-on-your-storsimple-device"></a>De apparatuurmodus op uw apparaat StorSimple wijzigen

Dit artikel bevat een korte beschrijving van de verschillende modi waarin uw StorSimple-apparaat kunt werken. Uw apparaat StorSimple kan functioneren in drie modi: normaal, onderhoud en herstel. 

Lees dit artikel en weet u:

- Welke StorSimple apparaat opties voor de zijn
- Hoe om te bepalen welke modus het apparaat StorSimple is in
- Het wijzigen van de normale onderhoudsmodus en *omgekeerd*


De bovenstaande beheertaken kunnen alleen worden uitgevoerd via de Windows PowerShell-interface van uw apparaat StorSimple.

## <a name="about-storsimple-device-modes"></a>Over de StorSimple apparaat modi

Uw apparaat StorSimple kunt werken in de modus voor normaal, onderhoud of herstellen. Elk van deze modi wordt kort hieronder beschreven.

### <a name="normal-mode"></a>Normale modus

Dit is gedefinieerd als de normale operationele modus voor een volledig geconfigureerde StorSimple-apparaat. Uw apparaat moet worden standaard in de normale modus.

### <a name="maintenance-mode"></a>Onderhoudsmodus

Het apparaat StorSimple moet soms onderhoud modus worden geplaatst. Deze modus kunt u onderhoud uitvoeren op het apparaat en installeer storend updates, zoals die betrekking hebben op de schijf firmware.

U kunt het systeem in de onderhoudsmodus alleen via de Windows PowerShell plaatsen voor StorSimple. Alle i/o-aanvragen worden in deze modus onderbroken. Services zoals niet-vluchtige RAM geheugen (NVRAM) of de clusteringservice worden ook gestopt. Beide de controllers opnieuw worden gestart wanneer u voert u deze modus openen of sluiten. Wanneer u de onderhoudsmodus afsluit, worden alle services wordt hervat en moeten orde. Dit kan een paar minuten duren.

>[AZURE.NOTE]**Onderhoudsmodus wordt alleen ondersteund op een apparaat naar behoren werkt. Deze wordt niet ondersteund op een apparaat waarin u een of beide van de controllers niet functioneren.**
</br>

### <a name="recovery-mode"></a>Herstelmodus

Herstelmodus kan worden aangeduid met "Veilige modus van Windows met netwerkondersteuning". De herstelmodus alleen mogelijk het team van Microsoft Support en kan ze diagnostische hulpprogramma's uitvoeren op het systeem. Het primaire doel van herstelmodus is de systeemlogboeken ophalen.

Als uw systeem herstel gaat, moet u contact opnemen met Microsoft Support voor de volgende stappen. Ga naar [Contact opnemen met Microsoft ondersteuning](storsimple-contact-microsoft-support.md)voor meer informatie.

>[AZURE.NOTE] **U kunt het apparaat in herstelmodus plaatsen. Als het apparaat goed werkt, is herstelmodus wordt geprobeerd om het apparaat in de modus waarin Microsoft Support-medewerkers, deze kunnen bekijken.**

## <a name="determine-storsimple-device-mode"></a>StorSimple apparaatmodus bepalen

#### <a name="to-determine-the-current-device-mode"></a>Om te bepalen de huidige apparatuurmodus

1. Meld u aan bij de seriële console apparaat volgens de stappen in [Gebruik stopverf verbinding maken met de seriële apparaat-console](storsimple-deployment-walkthrough.md#use-putty-to-connect-to-the-device-serial-console).

2. Bekijk de banner bericht in het menu seriële console van het apparaat. Dit bericht expliciet wordt aangegeven of het apparaat in de modus onderhoud of herstel. Als het bericht bevat geen specifieke gegevens die betrekking hebben op de systeem-modus, wordt het apparaat is in de normale modus.

## <a name="change-the-storsimple-device-mode"></a>De modus van StorSimple apparaat wijzigen 

Als u wilt uitvoeren onderhoud of onderhoud modus updates wilt installeren, kunt u het apparaat StorSimple plaatsen in de onderhoudsmodus (van normale modus). De volgende procedures wilt invoeren of afsluiten onderhoudsmodus uitvoeren.

> [AZURE.IMPORTANT] Voordat u de onderhoudsmodus invoert, controleert u of beide apparaatcontrollers orde via de **Hardware Status** op de pagina **onderhoud** in de portal van Azure klassieke. Als een of beide van de controllers in orde is, moet u contact op met Microsoft Support voor de volgende stappen. Ga naar [Contact opnemen met Microsoft ondersteuning](storsimple-contact-microsoft-support.md)voor meer informatie.

#### <a name="to-enter-maintenance-mode"></a>Modus voor onderhoud

1. Meld u aan bij de seriële console apparaat volgens de stappen in [Gebruik stopverf verbinding maken met de seriële apparaat-console](storsimple-deployment-walkthrough.md#use-putty-to-connect-to-the-device-serial-console).

2. Kies optie 1, **aanmelden met de volledige toegang**in het menu seriële console. Wanneer u wordt gevraagd, kunt u het **apparaat beheerderswachtwoord**opgeven. Het standaardwachtwoord is: `Password1`.

3. Typ bij de opdrachtprompt 

    `Enter-HcsMaintenanceMode`

4. Hier ziet u een waarschuwingsbericht met de mededeling dat onderhoudsmodus worden alle i/o-aanvragen verstoort de verbinding met de Azure klassieke portal server en wordt u gevraagd om bevestiging. Typ **Y** onderhoud modus.

5. Beide controllers opnieuw. Wanneer u het opnieuw opstarten is voltooid, wordt een ander bericht weergegeven dat aangeeft dat het apparaat in de onderhoudsmodus voor is.


#### <a name="to-exit-maintenance-mode"></a>Onderhoudsmodus afsluit

1. Meld u aan bij de seriële apparaat-console. Controleer of vanuit de banner-bericht dat uw apparaat in de onderhoudsmodus voor is.

2. Typ bij de opdrachtprompt:

    `Exit-HcsMaintenanceMode`

3. Een waarschuwing ziet en een bevestigingsbericht wordt weergegeven. Typ **Y** onderhoudsmodus afsluit.

4. Beide controllers opnieuw. Wanneer u het opnieuw opstarten is voltooid, wordt een ander bericht weergegeven dat aangeeft dat het apparaat in de normale modus is.


## <a name="next-steps"></a>Volgende stappen

Leer hoe u [Normaal en onderhoud modus updates toepassen](storsimple-update-device.md) op uw apparaat StorSimple.

