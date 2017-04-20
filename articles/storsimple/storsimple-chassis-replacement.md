<properties 
   pageTitle="Vervang het chassis op een apparaat StorSimple | Microsoft Azure"
   description="Beschrijving van het verwijderen en vervangen het chassis voor uw primaire insluiting StorSimple of EBOD insluiting."
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

# <a name="replace-the-chassis-on-your-storsimple-device"></a>Het chassis op uw apparaat StorSimple vervangen

## <a name="overview"></a>Overzicht

Deze zelfstudie wordt uitgelegd hoe verwijderen en vervangen van een chassis in een StorSimple 8000 reeks-apparaat. Het model StorSimple 8100 is een apparaat één insluiting (één chassis), terwijl de 8600 een dubbele insluiting-apparaat (twee chassis is). Voor een model 8600, zijn er mogelijk twee chassis die niet meer in het apparaat: het chassis voor de primaire insluiting of het chassis voor de EBOD insluiting.

In beide gevallen is de vervangende chassis die wordt geleverd door Microsoft leeg. Geen Power en koeling Modules (PCMs), controller modules, effen staat schijfstations (SSD), vaste schijven (harde schijven) of EBOD modules, worden opgenomen.

>[AZURE.IMPORTANT] Controleer de gegevens van de veiligheid in [StorSimple hardware onderdeel vervangende](storsimple-hardware-component-replacement.md)voordat verwijderen en vervangen van het chassis.

## <a name="remove-the-chassis"></a>Het chassis verwijderen

De volgende stappen als u wilt verwijderen van het chassis op uw apparaat StorSimple uitvoeren.

#### <a name="to-remove-a-chassis"></a>Een chassis verwijderen

1. Zorg ervoor dat het apparaat StorSimple is afgesloten en alle gegevensbronnen in de power is verbroken.

2. Verwijder alle netwerk- en SA's kabels, indien van toepassing.

3. Verwijder de eenheid uit het rek.

4. Elk van de stations verwijderen en noteer de sleuven waarvan ze zijn verwijderd. Zie [de harde schijf verwijderen](storsimple-disk-drive-replacement.md#remove-the-disk-drive)voor meer informatie.

5. Klik op de bijlage EBOD (als dit het chassis die is mislukt is), de EBOD controller-modules verwijderen. Zie [een controller EBOD verwijderen](storsimple-ebod-controller-replacement.md#remove-an-ebod-controller)voor meer informatie. 

    Klik op de primaire insluiting (als dit het chassis die is mislukt is), verwijder de controllers en noteer de sleuven waarvan ze zijn verwijderd. Zie [een controller verwijderen](storsimple-controller-replacement.md#remove-a-controller)voor meer informatie.

## <a name="install-the-chassis"></a>Het chassis installeren

De volgende stappen als u wilt het chassis installeren op uw apparaat StorSimple uitvoeren.

#### <a name="to-install-a-chassis"></a>Een chassis installeren

1. Koppel het chassis in het rek. Zie [uw apparaat StorSimple 8100 rek-mountains](storsimple-8100-hardware-installation.md#rack-mount-your-storsimple-8100-device) of [uw apparaat StorSimple 8600 rek-koppeling](storsimple-8600-hardware-installation.md#rack-mount-your-storsimple-8600-device)voor meer informatie.

2. Nadat het chassis is gekoppeld in het rek, installeert u de controller-modules op dezelfde positie die ze eerder in zijn geïnstalleerd.

3. Installeer de stations in dezelfde posities en sleuven die ze eerder in zijn geïnstalleerd.

    >[AZURE.NOTE] Het is raadzaam dat u eerst de SSD in de sleuven installeren en installeer de harde schijven.

2. Bij het apparaat dat is gekoppeld in het rek en de onderdelen die is geïnstalleerd, sluit u het apparaat naar de juiste power-bronnen en schakelt u het apparaat. Zie [uw apparaat StorSimple 8100-kabel](storsimple-8100-hardware-installation.md#cable-your-storsimple-8100-device) of [kabel uw apparaat StorSimple 8600](storsimple-8600-hardware-installation.md#cable-your-storsimple-8600-device)voor meer informatie.

## <a name="next-steps"></a>Volgende stappen

Meer informatie over [StorSimple hardware onderdeel vervangende](storsimple-hardware-component-replacement.md).

