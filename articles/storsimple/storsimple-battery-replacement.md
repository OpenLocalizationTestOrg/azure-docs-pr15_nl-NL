<properties 
   pageTitle="Vervang de kleine op een apparaat StorSimple | Microsoft Azure"
   description="Wordt beschreven hoe verwijderen, vervangen en voor het behoud van de module back-up kleine op uw apparaat StorSimple."
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
   ms.date="08/17/2016"
   ms.author="alkohli" />

# <a name="replace-the-backup-battery-module-on-your-storsimple-device"></a>De module back-up kleine op uw apparaat StorSimple vervangen

## <a name="overview"></a>Overzicht

De primaire insluiting Power en koeling Module (PCM) gebruiken op uw apparaat Microsoft Azure StorSimple heeft een taalpakket extra kleine. Deze pack biedt power zodat het apparaat StorSimple gegevens kunt opslaan als er verlies van AC kracht en het primaire insluiting. Dit kleine pack is de *back-up kleine module*genoemd. De module back-up kleine bestaat alleen voor de primaire ruimte in uw StorSimple-apparaat (de insluiting EBOD bevat geen een back-kleine-module). 

Deze zelfstudie wordt uitgelegd hoe u:

- Verwijderen van de back-kleine-module 
- Een nieuwe back-kleine-module hebt geïnstalleerd
- Voor het behoud van de back-kleine-module

>[AZURE.IMPORTANT] Controleer voordat u verwijderen en vervangen door een back-kleine-module, de veiligheid-gegevens in de [Inleiding tot StorSimple hardware onderdeel vervangende](storsimple-hardware-component-replacement.md).

## <a name="remove-the-backup-battery-module"></a>Verwijderen van de back-kleine-module

De back-kleine-module voor uw apparaat StorSimple is een veld-vervangen eenheid. Voordat deze wordt geïnstalleerd in de PCM, kan de module kleine moet worden opgeslagen in de originele verpakking. De volgende stappen als u wilt verwijderen van de back-up kleine uitvoeren.

#### <a name="to-remove-the-backup-battery-module"></a>Verwijderen van de back-kleine-module

1. In de klassieke Azure portal, gaat u naar **apparaten** > **onderhoud** > **Hardware Status**. Klik onder **Gedeelde onderdelen**, kijkt u naar de status van de computer.

2. Geef aan dat de PCM waarin de kleine is mislukt. Afbeelding 1 ziet u de achterzijde van het apparaat StorSimple.

    ![Backplane van apparaat primaire insluiting Modules](./media/storsimple-battery-replacement/IC740994.png)

    **Afbeelding 1** Achterzijde van hoofdapparaat met PCM en controller modules

  	|Label|Beschrijving|
  	|:----|:----------|
  	|1|PCM 0|
  	|2|PCM 1|
  	|3|Controller 0|
  	|4|Controller 1|

    Zoals u ziet door het getal 3 in de afbeelding 2, kunt u de controle indicator LED PCM 0 die overeenkomt met naar **Kleine foutenstructuuranalyse** moet branden.

    ![Backplane van apparaat PCM Monitoring Indicator-LED 's](./media/storsimple-battery-replacement/IC740992.png)

    **Afbeelding 2** Achterzijde van de controle LED met PCM

  	|Label|Beschrijving|
  	|:---|:-----------|
  	|1|AC power is mislukt|
  	|2|Ventilator is mislukt|
  	|3|Kleine foutenstructuuranalyse|
  	|4|PCM OK|
  	|5|Domeincontroller power is mislukt|
  	|6|Kleine orde|

3. Als u wilt verwijderen de PCM met een mislukte kleine, de stappen in [een PCM verwijderen](storsimple-power-cooling-module-replacement.md#remove-a-pcm)uit te voeren.

4. Met de PCM verwijderd, til Voeg alles snel aan de kleine verwijderen en de greep van de module kleine omhoog draaien zoals aangegeven in de volgende afbeelding.

    ![Kleine verwijderen uit PCM](./media/storsimple-battery-replacement/IC741019.png)

    **Afbeelding 3** De kleine verwijderen uit de PCM

5. Plaats de module in het veld vervangen eenheid verpakking.

6. De defect eenheid terugkeren naar Microsoft voor goede en afhandelen.

## <a name="install-a-new-backup-battery-module"></a>Een nieuwe back-kleine-module hebt geïnstalleerd

De volgende stappen om te kunnen installeren van de vervangende kleine module in het PCM in de primaire ruimte aan uw apparaat StorSimple uitvoeren.

#### <a name="to-install-the-battery-module"></a>De module kleine installeren

1. Plaats de module back-up kleine in de juiste richting in de PCM.

2. Houd de greep van de module kleine helemaal aan de verbindingslijn.

3. Vervang de PCM in de primaire ruimte door de richtlijnen in [een Power en koeling Module op uw apparaat StorSimple vervangen](storsimple-power-cooling-module-replacement.md).

4. Nadat de vervangende voltooid is, gaat u naar **apparaten** > **onderhoud** > **Hardware Status** in de portal van Azure klassieke. Controleer of de status van de kleine om ervoor te zorgen dat de installatie voltooid is. Een groene status geeft aan dat het kleine correct is.

## <a name="maintain-the-backup-battery-module"></a>Voor het behoud van de back-kleine-module

In uw apparaat StorSimple biedt de module back-up kleine power op de controller tijdens een power verlies gebeurtenis. Dit kan het apparaat StorSimple om op te slaan van belangrijke gegevens voordat u op een gecontroleerde manier afsluiten. Met twee volledig opgeladen batterijen in de PCMs, kan twee opeenvolgende verlies gebeurtenissen worden verwerkt door het systeem.

In de portal voor Azure klassieke de **Hardware Status** op de pagina **onderhoud** geeft aan of de kleine is defect of het einde van de levenscyclus bijna is bereikt. De status kleine wordt aangegeven door **kleine in PCM 0** of **kleine in PCM 1** onder **Gedeelde onderdelen**. Deze pagina wordt weergegeven een **GEDEGRADEERD** staat voor het einde van de levenscyclus bijna is bereikt en **mislukt** voor het einde van de levenscyclus bereikt. 

>[AZURE.NOTE] De kleine kunt **mislukt** melden wanneer deze gewoon moet betalen.
 
Als de status **GEDEGRADEERD** wordt weergegeven, raden we aan de volgende cursus van actie:

- Het systeem kan zich een recente power verlies of mogelijk periodiek onderhouden met de batterijen. Houd rekening met het systeem voor 12 uur voordat u verdergaat.

    - Als de status nog steeds **GEDEGRADEERD** na 12 uur continue verbinding tot de macht van AC met de controllers en PCMs uitgevoerd is, klikt u vervolgens moet de kleine worden vervangen. Neem [contact op met Microsoft ondersteuning](storsimple-contact-microsoft-support.md) voor een vervangende back-kleine-module.

    - Als de status OK na 12 uur wordt, de kleine functioneert en deze alleen een boete voor onderhoud nodig.

- Als er een gekoppeld kwaliteitsverlies AC power niet is en de PCM is ingeschakeld en tot de macht van AC verbonden, moet de kleine worden vervangen. [Contact opnemen met Microsoft ondersteuning](storsimple-contact-microsoft-support.md) om een vervangende back-kleine-module.

>[AZURE.IMPORTANT] Beschikken over de mislukte kleine op basis van de nationale en regionale voorschriften. 

## <a name="next-steps"></a>Volgende stappen

Meer informatie over [StorSimple hardware onderdeel vervangende](storsimple-hardware-component-replacement.md).
