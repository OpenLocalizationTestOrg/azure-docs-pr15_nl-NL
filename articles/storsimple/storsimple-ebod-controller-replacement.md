<properties 
   pageTitle="Vervang een controller StorSimple EBOD | Microsoft Azure"
   description="Dit artikel wordt uitgelegd hoe verwijderen en vervangen van een of beide EBOD controllers op een apparaat StorSimple 8600."
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

# <a name="replace-an-ebod-controller-on-your-storsimple-device"></a>Een controller EBOD op uw apparaat StorSimple vervangen

## <a name="overview"></a>Overzicht

Deze zelfstudie wordt uitgelegd hoe u een defect EBOD controllermodule op uw apparaat Microsoft Azure StorSimple vervangen. Als u wilt vervangen een EBOD controller-module, moet u:

- De beschadigde EBOD controller verwijderen
- Een nieuwe EBOD controller installeren

Houd rekening met de volgende informatie voordat u begint:

- Lege EBOD modules moeten worden ingevoegd in alle niet-gebruikte sleuven. De ruimte cool wordt niet correct als een slot geopend blijft.

- De controller EBOD direct-verwisselbare is en kan worden verwijderd of vervangen. Verwijder een mislukte module niet totdat er een vervangende. Wanneer u het proces vervangende start, moet u deze eindigen binnen 10 minuten.

>[AZURE.IMPORTANT] Voordat u probeert te verwijderen of verplaatsen van elk onderdeel StorSimple, zorg dat u de [veiligheid pictogram conventies](storsimple-safety.md#safety-icon-conventions) en andere [voorzorgsmaatregelen](storsimple-safety.md)worden gecontroleerd.

## <a name="remove-an-ebod-controller"></a>Een controller EBOD verwijderen

Voordat de mislukte EBOD controllermodule in uw apparaat StorSimple worden vervangen, zorg dat de andere EBOD controller-module actieve en actief is. De volgende procedure en de volgende tabel wordt uitgelegd hoe de EBOD controller-module verwijderen.

#### <a name="to-remove-an-ebod-module"></a>Een module EBOD verwijderen

1. Open de portal van Azure klassieke.

2. Navigeer naar **apparaten** > **onderhoud** > **Hardware Status**, en controleer of de status van de LED voor de actieve EBOD controller-module is groen en de LED voor de mislukte EBOD controllermodule rood is.

3. Zoek de mislukte EBOD controllermodule aan het einde van het apparaat.

4. Verwijder de kabels die verbinding maken met de EBOD controller-module op de controller alvorens de module EBOD uit het systeem.

5. Noteer de exacte SA's-poort van de EBOD controller-module die is gekoppeld aan de controller uit. U moet het systeem naar deze configuratie herstellen nadat u de module EBOD vervangen. 

    >[AZURE.NOTE] Dit is meestal poort A, die is gelabeld als **Host in** in het volgende diagram.

    ![Backplane van EBOD controller](./media/storsimple-ebod-controller-replacement/IC741049.png)

     **Afbeelding 1** Achterzijde van EBOD module

  	|Label|Beschrijving|
  	|:----|:----------|
  	|1|Foutenstructuuranalyse LED|
  	|2|Power LED|
  	|3|SA's verbindingslijnen|
  	|4|SA's LED 's|
  	|5|Seriële poorten factory alleen voor gebruik|
  	|6|Poort een (Host in)|
  	|7|Poort B (Host af)|
  	|8|Poort C (alleen Factory-gebruik)|

## <a name="install-a-new-ebod-controller"></a>Een nieuwe EBOD controller installeren

De volgende procedure en de volgende tabel wordt uitgelegd hoe een EBOD controller-module hebt geïnstalleerd op uw apparaat StorSimple.

#### <a name="to-install-an-ebod-controller"></a>Een controller EBOD installeren

1. Het apparaat EBOD van beschadiging, met name naar de interface-connector controleren. Installeer de nieuwe EBOD controller niet als een pincodes zijn gebogen.

2. Schuif de module met de vergrendelingen in de open stand, in de ruimte totdat de vergrendelingen deelnemen.

    ![EBOD controller installeren](./media/storsimple-ebod-controller-replacement/IC741050.png)

    **Afbeelding 2**  Installatie van de EBOD controller-module

3. Sluit het slot. U kunt een klik moet horen, zoals het slot alleen mogelijk.

    ![EBOD slot vrijgeven](./media/storsimple-ebod-controller-replacement/IC741047.png)

    **Afbeelding 3**  Het EBOD module slot sluiten

4. Sluit de kabels. Gebruik de precieze configuratie die aanwezig zijn voordat u de vervangende was. Zie het volgende diagram en tabel voor meer informatie over het koppelen van de kabels.

    ![Uw apparaat 4U voor power kabel](./media/storsimple-ebod-controller-replacement/IC770723.png)

    **Afbeelding 4**. Opnieuw verbinding maakt kabels

  	|Label|Beschrijving|
  	|:----|:----------|
  	|1|Primaire insluiting|
  	|2|PCM 0|
  	|3|PCM 1|
  	|4|Controller 0|
  	|5|Controller 1|
  	|6|EBOD controller 0|
  	|7|EBOD controller 1|
  	|8|EBOD insluiting|
  	|9|Power verdeling eenheden|

## <a name="next-steps"></a>Volgende stappen

Meer informatie over [StorSimple hardware onderdeel vervangende](storsimple-hardware-component-replacement.md).
