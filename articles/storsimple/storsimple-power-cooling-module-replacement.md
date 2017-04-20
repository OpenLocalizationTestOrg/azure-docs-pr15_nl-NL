<properties 
   pageTitle="Vervang een PCM op uw apparaat StorSimple | Microsoft Azure"
   description="Dit artikel wordt uitgelegd hoe verwijderen en vervangen van de kracht en koeling Module (PCM) op uw apparaat StorSimple"
   services="storsimple"
   documentationCenter=""
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

# <a name="replace-a-power-and-cooling-module-on-your-storsimple-device"></a>Een Power en koeling Module op uw apparaat StorSimple vervangen

## <a name="overview"></a>Overzicht

De kracht en koeling Module (PCM) in uw Microsoft Azure StorSimple-apparaat kunt u bestaat uit een voeding en koelventilatoren die worden beheerd door de primaire en EBOD bijlagen. Er is slechts één model van PCM die is gecertificeerd voor elke insluiting. De primaire insluiting is gecertificeerd voor een 764 W PCM en de ruimte EBOD is gecertificeerd voor een 580 W PCM. Hoewel de PCMs voor de primaire insluiting en de ruimte EBOD verschillend zijn, wordt de procedure vervangende identiek is.

Deze zelfstudie wordt uitgelegd hoe u:

- Verwijderen van een PCM
- Een vervangende PCM installeren

>[AZURE.IMPORTANT] Controleer de gegevens van de veiligheid in [StorSimple hardware onderdeel vervangende](storsimple-hardware-component-replacement.md)voordat verwijderen en vervangen door een PCM.

## <a name="before-you-replace-a-pcm"></a>Voordat u een PCM vervangen

Let op van de volgende belangrijke problemen voordat u uw PCM vervangen:

- Als de voeding van de PCM mislukt, laat u de beschadigde module hebt geïnstalleerd, maar de kabel is geleverd power verwijderen. De ventilator blijft power ontvangt van de bijlage en gaat u verder met het juiste koeling bieden. Als de ventilator mislukt, moet de PCM direct worden vervangen.

- Voordat u de PCM verwijdert, door de kracht met de PCM verbreken door het uitschakelen van de belangrijkste schakeloptie (indien aanwezig) of door de kabel is geleverd power fysiek verwijderen. Hiermee wordt een waarschuwing weergegeven naar uw systeem dat een afsluiten power handen is.

- Zorg ervoor dat de andere PCM functionele voor voortdurende Systeembewerking voordat de beschadigde PCM vervangen. Een beschadigde PCM moet worden vervangen door een volledig operationele PCM zo snel mogelijk.

- PCM module vervangende duurt slechts enkele minuten, maar deze moet worden voltooid binnen 10 minuten na het verwijderen van de mislukte PCM om te voorkomen dat oververhitting.

- Houd er rekening mee dat de 764 W PCM-modules van vervangende, verzonden vanuit de fabriek de module back-up kleine niet bevatten. U moet de kleine verwijderen uit uw defect PCM en voeg deze toe aan de module vervangende voordat het uitvoeren van de vervangende. Zie voor meer informatie, [verwijderen](storsimple-battery-replacement.md)en voeg een back-kleine-module.


## <a name="remove-a-pcm"></a>Verwijderen van een PCM

Volg deze instructies wanneer u klaar bent voor een Power en koeling Module (PCM) verwijderen uit uw Microsoft Azure StorSimple-apparaat.

>[AZURE.NOTE] Voordat u uw PCM verwijdert, Controleer of u een juiste vervangende (764 W voor de primaire insluiting) of 580 W voor de EBOD insluiting.

#### <a name="to-remove-a-pcm"></a>Verwijderen van een PCM

1. In de klassieke Azure portal, klikt u op **apparaten** > **onderhoud** > **Hardware Status**. Controleer de status van de onderdelen PCM onder **Gedeelde onderdelen** geven aan welke PCM is mislukt:

     - Als een voeding in PCM 0 is mislukt, zijn de status van **Voeding in PCM 0** rood.

     - Als een voeding in PCM 1 is mislukt, zijn de status van **Voeding in PCM 1** rood.

     - Als de ventilator in PCM 1 is mislukt, zijn de status van **koeling 0 voor PCM 0** of **1 koeling voor PCM 0** rood.

2. Zoek de mislukte PCM aan de achterzijde van de primaire insluiting. Als u een model 8600 uitvoert, zijn bepalen u de primaire insluiting door te kijken het systeem eenheid id-nummer weergegeven op de voorgrond Configuratiescherm LED-scherm. De standaardinstelling die eenheid-ID op de primaire insluiting weergegeven is **00**, terwijl de eenheid-ID in de ruimte EBOD weergegeven standaardwaarde **01 is**. Het volgende diagram en de volgende tabel wordt uitgelegd het front deelvenster van het LED-scherm.

    ![ID van het systeem op voorgrond OPS Configuratiescherm](./media/storsimple-power-cooling-module-replacement/IC740991.png)

     **Afbeelding 1** Voorste deelvenster van het apparaat  

  	|Label|Beschrijving|
  	|:---|:-----------|
  	|1|Knop Dempen|
  	|2|Systeem power|
  	|3|Module foutenstructuuranalyse|
  	|4|Logische foutenstructuuranalyse|
  	|5|Eenheid-ID weergeven|

3. De controle LED aan de achterzijde van de primaire insluiting kan ook worden gebruikt om aan te geven van de beschadigde PCM. Zie de volgende diagram en tabellen voor meer informatie over het gebruik van de LED's om te zoeken van de beschadigde PCM. Bijvoorbeeld als de LED die overeenkomt met de **Ventilator mislukt** licht, is de ventilator mislukt. Op dezelfde manier als de LED die overeenkomt met **AC Fail** licht, is de voeding mislukt. 

    ![Backplane van apparaat PCM controleren LED](./media/storsimple-power-cooling-module-replacement/IC740992.png)

     **Afbeelding 2** Achterzijde van PCM met LED

  	|Label|Beschrijving|
  	|:---|:-----------|
  	|1|AC power is mislukt|
  	|2|Ventilator is mislukt|
  	|3|Kleine foutenstructuuranalyse|
  	|4|PCM OK|
  	|5|Domeincontroller power is mislukt|
  	|6|Kleine orde|

4. Raadpleeg het volgende diagram van de achterzijde van het apparaat StorSimple om te zoeken van de mislukte PCM-module. PCM 0 is aan de linkerkant en PCM 1 is aan de rechterkant. De onderstaande tabel wordt uitgelegd dat de modules.

     ![Backplane van apparaat primaire insluiting modules](./media/storsimple-power-cooling-module-replacement/IC740994.png)

     **Afbeelding 3** Achterzijde van apparaat instellen met Plug-ins 

  	|Label|Beschrijving|
  	|:---|:-----------|
  	|1|PCM 0|
  	|2|PCM 1|
  	|3|Controller 0|
  	|4|Controller 1|

5. De beschadigde PCM uitschakelen en de verbinding verbreken de power levering kabel is. U kunt nu de PCM verwijderen.

6. Het slot en de zijkant van de greep PCM tussen uw duim en wijsvinger te vatten en om de greep open ze verondersteld.

    ![Greep van de PCM openen](./media/storsimple-power-cooling-module-replacement/IC740995.png)

    **Afbeelding 4** De greep PCM openen

7. De greep houvastOpening en verwijder de PCM.

    ![Verwijderen van apparaat PCM](./media/storsimple-power-cooling-module-replacement/IC740996.png)

    **Afbeelding 5** De PCM verwijderen

## <a name="install-a-replacement-pcm"></a>Een vervangende PCM installeren

Volg deze instructies voor het installeren van een PCM in uw StorSimple-apparaat. Zorg ervoor dat u de module back-up kleine vóór het installeren van de vervangende PCM (geldt voor alleen 764 W PCMs) hebt ingevoegd. Zie voor meer informatie, [verwijderen](storsimple-battery-replacement.md)en voeg een back-kleine-module.

#### <a name="to-install-a-pcm"></a>Voor het installeren van een PCM

1. Controleer of u de juiste vervangende PCM voor deze bijlage. De primaire insluiting moet een 764 W PCM en de ruimte EBOD moet een 580 W PCM. U moet niet proberen om de 580 W PCM in de primaire ruimte, of de 764 W PCM in de ruimte EBOD te gebruiken. De volgende afbeelding ziet u waar u deze informatie over het label dat wordt aangebracht op de PCM identificeren.

    ![Apparaat PCM Label](./media/storsimple-power-cooling-module-replacement/IC740973.png)

    **Afbeelding 6** PCM label

2. Controleren op de bijlage, met bepaalde aandacht voor de verbindingslijnen beschadigd. 
                                        
    >[AZURE.NOTE] **Installeer de module niet als een verbindingslijn pincodes zijn gebogen.**

3. Dia met de greep PCM in de open stand, de module in de ruimte.

    ![Installatie van apparaat PCM](./media/storsimple-power-cooling-module-replacement/IC740975.png)

    **Afbeelding 7** Installatie van de PCM

4. De greep PCM handmatig af te sluiten. U kunt een klik moet horen, zoals het slot greep alleen mogelijk. 
                                        
    >[AZURE.NOTE] Om ervoor te zorgen dat de verbindingslijn pennen hebt ingeschakeld, kunt u voorzichtig tug op de greep zonder het slot los te laten. Als de PCM dia's af, betekent dit dat het slot is afgesloten voordat de verbindingslijnen betrokken blijft.

5. Verbinding maken met de voeding kon naar de bron power en naar de PCM.

6. De druk reliëf balen beveiligen. 

7. De PCM inschakelen.

8. Controleer of dat de vervangende geslaagd is: Ga in de Azure klassieke portal van uw service StorSimple Manager naar **apparaten** > **onderhoud** > **Hardware Status**. De status van de PCM moet zijn groen onder **Gedeelde onderdelen**. 
                                        
    >[AZURE.NOTE] Het kan enkele minuten duren voordat de vervangende PCM volledig geïnitialiseerd duren.

## <a name="next-steps"></a>Volgende stappen

Meer informatie over [StorSimple hardware onderdeel vervangende](storsimple-hardware-component-replacement.md).
