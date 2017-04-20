<properties 
   pageTitle="StorSimple hardware onderdeel vervangende | Microsoft Azure"
   description="Wordt beschreven hoe veilig vervangen de PCMs, kleine controller modules, EBOD controllers, schijfstations en chassis van een StorSimple-apparaat."
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
   ms.date="10/11/2016"
   ms.author="alkohli" />

# <a name="storsimple-hardware-component-replacement"></a>StorSimple hardware onderdeel vervangende

## <a name="overview"></a>Overzicht

De zelfstudies voor hardware onderdeel vervangende beschrijven de hardware-onderdelen van uw apparaat met Microsoft Azure StorSimple 8000 reeks en stapsgewijze instructies voor het verwijderen en vervang deze velden. In dit artikel worden de pictogrammen veiligheid beschreven, bevat verwijzingen naar de gedetailleerde zelfstudies en lijsten van de onderdelen die vervangen zijn.

>[AZURE.IMPORTANT] Voordat u probeert te verwijderen of verplaatsen van elk onderdeel StorSimple, zorg dat u de [veiligheid pictogram conventies](#safety-icon-conventions) en andere [voorzorgsmaatregelen](storsimple-safety.md)worden gecontroleerd.
 
### <a name="safety-icon-conventions"></a>Veiligheid pictogram conventies

De volgende tabel beschrijft de veiligheid pictogrammen in deze zelfstudies. Aandacht besteden aan deze pictogrammen veiligheid terwijl u de stappen voor het Apparaatonderdelen verwijderen en vervangen doorloopt.

| Pictogram | Tekst | Aanvullende informatie |
|:---- |:---- |:-----------|
|![Waarschuwingspictogram](./media/storsimple-hardware-component-replacement/Warning.png)| **GEVAAR!** | Geeft een gevaarlijke situatie die als niet voorkomen, in overlijden of ernstige schade resulteert. Dit woord signaal is beperkt tot de meest extreme omstandigheden.|
|![Waarschuwingspictogram](./media/storsimple-hardware-component-replacement/Warning.png)| **WAARSCHUWING!** | Geeft een gevaarlijke situatie dat, als u niet voorkomen, leiden overlijden of ernstige schade tot kan.|
|![Waarschuwingspictogram](./media/storsimple-hardware-component-replacement/Caution.png)| **LET OP!** |Geeft een gevaarlijke situatie dat, als u niet voorkomen, leiden kleine of regelmatige schade tot kan.|
|![Kennisgeving-pictogram](./media/storsimple-hardware-component-replacement/NoticeIcon.png)| **OPMERKING:** | Geeft aan beschouwd als belangrijk, maar niet gevaar-gerelateerde gegevens.|
|![Pictogram voor elektriciteit gebruik](./media/storsimple-hardware-component-replacement/Electric.png) | **Elektrische gebruik gevaar** | Geeft aan hoge voltage.|
|![Veel gewicht-pictogram](./media/storsimple-hardware-component-replacement/Weight.png)| **Veel gewicht**| |
|![Geen Gebruikerspictogram onderhouden onderdelen](./media/storsimple-hardware-component-replacement/NoUserServiceableParts.png)| **Geen onderdelen voor het onderhouden van gebruiker** | Geen toegang tot tenzij de juiste manier te trainen.|
|![Pictogram van de instructies lezen](./media/storsimple-hardware-component-replacement/ReadInstructions.png)|**Alle instructies als eerste lezen**| |
|![Pictogram voor een tip gevaar](./media/storsimple-hardware-component-replacement/TipHazard.png)|**Tip gevaar**| |

### <a name="before-you-begin"></a>Voordat u begint

Vertrouwd raken met de veiligheidsinformatie over uw apparaat en veiligheid pictogrammen in deze zelfstudie gebruikt. Ga naar [veilig installeren en gebruiken van uw apparaat StorSimple](storsimple-safety.md) voor volledige informatie. Zorg ervoor dat de [voorzorgsmaatregelen](storsimple-safety.md#handling-precautions) bekijken voordat u uw apparaat StorSimple verwerken. 

Voordat u probeert een component vervangen, kunt u de volgende informatie.

![Waarschuwingspictogram](./media/storsimple-hardware-component-replacement/Warning.png) ![elektrische gebruik pictogram](./media/storsimple-hardware-component-replacement/Electric.png) **Waarschuwing!** 

- Moet u zichzelf correct met behulp van een elektrostatische nakoming of antistatische mat wanneer modules en de onderdelen van uw apparaat StorSimple verwerken.

- Een circuits geen raken. Gebruik de opgegeven grepen en hulplijnen tijdens het verwerken van onderdelen die mogelijk hebt die worden aangeboden circuits.

![Waarschuwingspictogram](./media/storsimple-hardware-component-replacement/Warning.png) ![zoals u ziet, pictogram](./media/storsimple-hardware-component-replacement/NoticeIcon.png) **kennisgeving:**

Wanneer u een module, **laat u nooit een lege bay aan de achterzijde van de ruimte**vervangen. Schaf een vervangende of lege module voordat u het probleem deel verwijdert.

## <a name="hardware-component-replacement-procedures"></a>Hardware onderdeel vervangende procedures

Uw apparaat van de reeks StorSimple 8000 bestaat uit verschillende Plug-ins in de primaire en/of EBOD bijlagen. De 8100 heeft een één primaire insluiting, terwijl de 8600 een dubbele insluiting-apparaat met een primaire insluiting en een insluiting EBOD is.

De hardwareonderdelen van de belangrijkste in uw apparaat worden in de volgende tabellen samengevat. Klik op de koppeling in de kolom **vervangende procedure** om naar de bijbehorende zelfstudie te gaan.

|Onderdelen|# Presenteren|Invoegtoepassing module?|Vervangende procedure
|:---------|:--------|:--------------|:---------------------|
| Chassis|1|Nee|[Het chassis op uw apparaat StorSimple vervangen](storsimple-chassis-replacement.md) |
|Primaire domeincontrollers|2|Ja| [Een controllermodule op uw apparaat StorSimple vervangen](storsimple-controller-replacement.md) |
|764W Power en koelingsmodules (PCMs)|2|Ja| [Een Power en koeling Module op uw apparaat StorSimple vervangen](storsimple-power-cooling-module-replacement.md) |
|Back-up kleine|2|Ja| [De module back-up kleine op uw apparaat StorSimple vervangen](storsimple-battery-replacement.md) |
|Schijfstations|12|Ja| [Een schijf op uw apparaat StorSimple vervangen](storsimple-disk-drive-replacement.md) |

**Tabel 1** Onderdelen van de netwerkhardware in de primaire ruimte

De primaire ruimte en de EBOD ruimte afwijken in hun i/o-modules. Bovendien hebben de PCMs verschillende vermogen. De PCMs in de primaire ruimte zijn 764 W, die in de ruimte EBOD 580 zijn W. De PCMs in de primaire ruimte bevatten ook een back-kleine-module.

|Onderdelen|# Presenteren|Invoegtoepassing module?| Vervangende procedure
|:---------|:--------|:--------------|:---------------------|
|Chassis|1|Nee| [Het chassis op uw apparaat StorSimple vervangen](storsimple-chassis-replacement.md) |
|EBOD controllers|2|Ja| [Een controller EBOD op uw apparaat StorSimple vervangen](storsimple-ebod-controller-replacement.md) |
|580 w bij Power en koelingsmodules (PCMs)|2|Ja| [Een Power en koeling Module op uw apparaat StorSimple vervangen](storsimple-power-cooling-module-replacement.md) |
|Schijfstations|12|Ja| [Een schijf op uw apparaat StorSimple vervangen](storsimple-disk-drive-replacement.md) |

**Tabel 2** Onderdelen van de netwerkhardware in de ruimte EBOD

De Plug-ins op het apparaat zijn gemarkeerd in de volgende voorste en achterste diagrammen. U kunt deze diagrammen gebruiken om te bepalen de locatie van de verschillende Plug-ins als een vervangende vereist is. De front-diagram ziet u de schijfstations, en de achterzijde diagrammen van de ruimte EBOD en de primaire insluiting weergeven de Plug-ins.

![Frontplane van apparaat instellen met schijfstations](./media/storsimple-hardware-component-replacement/IC741028.png)

**Afbeelding 1** Voorkant van het apparaat

|Label|Beschrijving|
|:----|:----------|
|0 - 11|Schijfstations (in totaal 12)|

De primaire insluiting zowel de insluiting EBOD hebben station carrier modules. Het chassis bevinden zich twaalf 3.5" schijfstations gerangschikt in een indeling met 3 bij 4.

![Backplane van apparaat primaire insluiting modules](./media/storsimple-hardware-component-replacement/IC740994.png)

**Afbeelding 2** Achterzijde van de primaire insluiting

|Label|Beschrijving|
|:----|:----------|
|1|PCM 0|
|2|PCM 1|
|3|Controller 0|
|4|Controller 1|

![Backplane van apparaat EBOD insluiting Plug-ins](./media/storsimple-hardware-component-replacement/IC769599.png)

**Afbeelding 3** Achterzijde van de ruimte EBOD

|Label|Beschrijving|
|:----|:----------|
|1|PCM 0|
|2|PCM 1|
|3|EBOD Controller 0|
|4|EBOD Controller 1|

## <a name="field-replaceable-units"></a>Veld vervangen eenheden

Het volgende veld vervangen eenheden (replaceable) zijn beschikbaar voor uw apparaat StorSimple:

- Chassis (met inbegrip van het deelvenster geïntegreerde bewerkingen)

- 764 W AC PCM

- 580 W AC PCM

- Harde schijf die u met station carrier module

- Controllermodule

- EBOD controllermodule

- Back-up kleine module

- Rek railkit koppelen

Neem [contact op met Microsoft ondersteuning](storsimple-contact-microsoft-support.md) als u wilt sorteren op een van deze eenheden vervangende.

## <a name="next-steps"></a>Volgende stappen

Controleer alle [veiligheidsinformatie](storsimple-safety.md) voordat u probeert een StorSimple hardwarecomponent vervangen.
