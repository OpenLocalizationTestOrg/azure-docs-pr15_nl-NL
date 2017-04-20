<properties 
   pageTitle="Uw apparaat StorSimple inschakelen of uitschakelen | Microsoft Azure"
   description="Wordt uitgelegd hoe u een nieuw StorSimple apparaat inschakelen, een apparaat dat is afgesloten of verloren power inschakelen en een actieve apparaat uitschakelen."
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
   ms.date="08/23/2016"
   ms.author="alkohli" />

# <a name="turn-your-storsimple-device-on-or-off"></a>Uw apparaat StorSimple inschakelen of uitschakelen 

## <a name="overview"></a>Overzicht

Een apparaat met Microsoft Azure StorSimple afsluiten is niet vereist als onderdeel van de werking van het normale systeem. Mogelijk moet u een nieuw apparaat of een apparaat met een worden afgesloten inschakelen. In het algemeen wordt is een afsluiten vereist in gevallen waarin u moet vervangen mislukte hardware, fysiek een eenheid verplaatsen of een apparaat afmelden bij service uitvoeren. Deze zelfstudie wordt de vereiste procedure voor het in- en afsluiten van uw apparaat StorSimple in verschillende scenario's beschreven.

De volgende tabel worden verschillende scenario's voor het inschakelen van en het afsluiten van uw apparaat StorSimple en bevat koppelingen naar de desbetreffende procedures.

|Scenario|Naslaginformatie|
|:-------|:---------------|
|Een nieuwe apparaat inschakelen|[Een nieuwe apparaat inschakelen](#turn-on-a-new-device)<ul><li>[Nieuwe apparaat instellen met alleen primaire insluiting](#new-device-with-primary-enclosure-only)</li><li>[Nieuwe apparaat instellen met EBOD insluiting](#new-device-with-ebod-enclosure)</li></ul>|
|Een apparaat inschakelen na afsluiten|[Een apparaat inschakelen na afsluiten](#turn-on-a-device-after-shutdown)<ul><li>[Apparaat instellen met alleen primaire insluiting](#device-with-primary-enclosure-only)</li><li>[Apparaat instellen met EBOD insluiting](#device-with-ebod-enclosure)</li></ul>|
|Een apparaat na een verlies power inschakelen|[Een apparaat na een verlies power inschakelen](#turn-on-a-device-after-a-power-loss)<ul><li>[Apparaat instellen met alleen primaire insluiting](#8100)</li><li>[Apparaat instellen met EBOD insluiting](#8600)</li></ul>|
|Een apparaat na de primaire insluiting inschakelen en EBOD verbinding wordt verbroken|[Een apparaat inschakelen nadat de primaire en EBOD insluiting verbinding wordt verbroken](#turn-on-a-device-after-the-primary-and-ebod-enclosure-connection-is-lost)|
|Een actieve apparaat afsluiten|[Een actieve apparaat uitschakelen](#turn-off-a-running-device)<ul><li>[Apparaat instellen met alleen primaire insluiting](#8100a)</li><li>[Apparaat instellen met EBOD insluiting](#8600a)</li></ul>|

## <a name="turn-on-a-new-device"></a>Een nieuwe apparaat inschakelen

De stappen voor het inschakelen van een apparaat StorSimple voor de eerste keer verschillen afhankelijk van of het apparaat is een 8100 of een 8600-model. De 8100 heeft een één primaire insluiting, terwijl de 8600 een twee-insluiting-apparaat met een primaire insluiting en een insluiting EBOD is. De gedetailleerde stappen voor beide modellen worden in de volgende secties beschreven.

- [Nieuwe apparaat instellen met alleen primaire insluiting](#new-device-with-primary-enclosure-only)

- [Nieuwe apparaat instellen met EBOD insluiting](#new-device-with-ebod-enclosure)

### <a name="new-device-with-primary-enclosure-only"></a>Nieuwe apparaat instellen met alleen primaire insluiting

Het model StorSimple 8100 is een apparaat één insluiting. Uw apparaat bevat overtollige Power en koeling Modules (PCMs). Zowel PCMs moet worden geïnstalleerd en verbonden met verschillende power bronnen om ervoor te zorgen beschikbaarheid.

De volgende stappen om uw apparaat gebruiken voor power kabel uitvoeren.

[AZURE.INCLUDE [storsimple-cable-8100-for-power](../../includes/storsimple-cable-8100-for-power.md)]

>[AZURE.NOTE]Voor volledige Apparaatinstelling en instructies kabel, gaat u naar [uw apparaat StorSimple 8100 installeren](storsimple-8100-hardware-installation.md). Zorg ervoor dat u de instructies precies.

### <a name="new-device-with-ebod-enclosure"></a>Nieuwe apparaat instellen met EBOD insluiting

Het model StorSimple 8600 heeft een primaire insluiting zowel een insluiting EBOD. U moet hiervoor de eenheden die aan elkaar worden bekabelde voor seriële bijgevoegd SCSI (SA's)-connectiviteit en power.

Wanneer u dit apparaat voor het eerst instelt, voer de stappen voor het SA's kabel eerst en vult u de stappen voor het power kabel.

[AZURE.INCLUDE [storsimple-sas-cable-8600](../../includes/storsimple-sas-cable-8600.md)]

[AZURE.INCLUDE [storsimple-cable-8600-for-power](../../includes/storsimple-cable-8600-for-power.md)]

>[AZURE.NOTE]Voor volledige Apparaatinstelling en instructies kabel, gaat u naar [uw apparaat StorSimple 8600 installeren](storsimple-8600-hardware-installation.md). Zorg ervoor dat u de instructies precies.

## <a name="turn-on-a-device-after-shutdown"></a>Een apparaat inschakelen na afsluiten

De stappen voor het inschakelen van een apparaat StorSimple nadat deze is afgesloten verschillen afhankelijk van of het apparaat is een 8100 of een 8600-model. De 8100 heeft een één primaire insluiting, terwijl de 8600 een twee-insluiting-apparaat met een primaire insluiting en een insluiting EBOD is.

- [Apparaat instellen met alleen primaire insluiting](#device-with-primary-enclosure-only)

- [Apparaat instellen met EBOD insluiting](#device-with-ebod-enclosure)

### <a name="device-with-primary-enclosure-only"></a>Apparaat instellen met alleen primaire insluiting

Gebruik de volgende procedure om in te schakelen op een apparaat StorSimple met een primaire insluiting en geen insluiting EBOD na afsluiten.

#### <a name="to-turn-on-a-device-with-a-primary-enclosure-only"></a>U kunt een apparaat met een primaire insluiting alleen inschakelen

1. Zorg ervoor dat de kracht schakelt u op beide Power en koeling Modules (PCMs) in de stand uit. Als de schakelopties zich niet in de stand uit, spiegelen ze in de stand uit en wacht totdat de lichten om uit te gaan.

2. Schakel het apparaat door de schakelopties power op beide PCMs naar de stand spiegelen. Het apparaat te schakelen.

3. Controleer het volgende om te controleren of het apparaat is volledig op:

    1. De OK LED's op beide modules PCM zijn groen.

    2. De status LED's op beide domeincontrollers zijn groen.

    3. De blauwe LED op een van de is knipperende, waarmee wordt aangegeven dat de controller actief is.

    Als aan al deze voorwaarden is voldaan, klikt u vervolgens is uw apparaat beschadigd. Neem [contact op met Microsoft ondersteuning](storsimple-contact-microsoft-support.md).

### <a name="device-with-ebod-enclosure"></a>Apparaat instellen met EBOD insluiting

Gebruik de volgende procedure om in te schakelen op een apparaat StorSimple met een primaire insluiting en een insluiting EBOD na afsluiten. Elke stap precies zoals wordt beschreven in de reeks uitvoeren. Verlies van gegevens kan leiden tot niet doet.

#### <a name="to-turn-on-a-device-with-a-primary-and-an-ebod-enclosure"></a>U kunt een apparaat met een primaire en een insluiting EBOD inschakelen

1. Zorg dat de insluiting EBOD is verbonden met de primaire insluiting. Zie [uw apparaat StorSimple 8600 installeren](storsimple-8600-hardware-installation.md)voor meer informatie.

2. Zorg ervoor dat de kracht en koeling Modules (PCMs) op EBOD zowel primaire bijlagen zijn in de stand uit. Als de schakelopties zich niet in de stand uit, spiegelen ze in de stand uit en wacht totdat de lichten om uit te gaan.

3. Schakel de insluiting EBOD eerste door de schakelopties power op beide PCMs naar de stand spiegelen. De PCM LED's moet groene. Een groene EBOD controller LED op deze eenheid wordt aangegeven dat de insluiting EBOD ingeschakeld is.

4. Schakel de primaire insluiting door de schakelopties power op beide PCMs naar de stand spiegelen. Het gehele systeem moet nu op.

5. Controleer of de SA's LED's zijn groen, waarin zorgt ervoor dat de verbinding tussen de ruimte EBOD en de primaire insluiting gemaakt is.

## <a name="turn-on-a-device-after-a-power-loss"></a>Een apparaat na een verlies power inschakelen

Een stroom uitvalt of onderbroken kan een apparaat StorSimple afsluiten. De stroom uitvalt kan op een van de voorraad power of beide voorraad power optreden. De stappen verschillen afhankelijk van of het apparaat een 8100 of een 8600-model is. De 8100 heeft een één primaire insluiting, terwijl de 8600 een twee-insluiting-apparaat met een primaire insluiting en een insluiting EBOD is. Dit onderwerp vindt de herstelprocedure voor elk scenario.

- [Apparaat instellen met alleen primaire insluiting](#8100)

- [Apparaat instellen met EBOD insluiting](#8600)

### <a name="device-with-primary-enclosure-only-a-name8100"></a>Apparaat instellen met alleen primaire insluiting<a name="8100">

Het systeem kan de normale werking blijven als er stroomonderbrekingen naar een van de voorraad power. Echter om ervoor te zorgen betere beschikbaarheid van het apparaat, herstel power de voeding zo snel mogelijk.

Als er een stroom uitvalt of power onderbroken op beide voeding, wordt het systeem zich in een ordelijk en gecontroleerde wijze afgesloten. Zodra de kracht is hersteld, verandert het systeem automatisch op.

### <a name="device-with-ebod-enclosure-a-name8600"></a>Apparaat instellen met EBOD insluiting<a name="8600">

#### <a name="power-loss-on-one-power-supply"></a>Stroomonderbrekingen op één power opgeven

Het systeem kan de normale werking blijven als er stroomonderbrekingen naar een van de voorraad power op de primaire insluiting of de insluiting EBOD. Echter om ervoor te zorgen betere beschikbaarheid van het apparaat, zet power de voeding zo snel mogelijk.

#### <a name="power-loss-on-both-power-supplies-on-primary-and-ebod-enclosures"></a>Stroomonderbrekingen op beide voorraad power op primair en EBOD bijlagen

Als er een stroom uitvalt of power onderbroken op beide voeding, de insluiting EBOD onmiddellijk wordt afgesloten en de primaire insluiting in een ordelijk en gecontroleerde wijze wordt afgesloten. Wanneer u power is hersteld, wordt het toestel automatisch gestart.

Als de kracht handmatig is uitgeschakeld, maakt u de volgende stappen uit om te zetten power naar het systeem.

1. De ruimte EBOD inschakelen.

2. Nadat de insluiting EBOD ingeschakeld is, schakel het primaire insluiting.

### <a name="power-loss-on-both-power-supplies-on-ebod-enclosure"></a>Stroomonderbrekingen op beide voorraad power op EBOD insluiting

Wanneer u uw kabels instelt, moet u ervoor zorgen dat de EBOD nooit alleen is verbonden met een afzonderlijke PDU. Als de EBOD en primaire insluiting niet op hetzelfde moment, wordt het systeem herstellen.

Als er slechts de insluiting EBOD op beide voeding mislukt, worden niet automatisch het systeem herstellen. Voert de volgende stappen uit te schakelen op het systeem en de orde zijn te herstellen:

1. Als de primaire insluiting is ingeschakeld, kunt u schakelen zowel Power en koeling Modules (PCMs) uit.

2. Wacht enkele minuten duren voordat het systeem af te sluiten.

3. De ruimte EBOD inschakelen.

4. Nadat de insluiting EBOD ingeschakeld is, schakel het primaire insluiting.

## <a name="turn-on-a-device-after-the-primary-and-ebod-enclosure-connection-is-lost"></a>Een apparaat inschakelen nadat de primaire en EBOD insluiting verbinding wordt verbroken

Als de verbinding verbroken tussen de stand-by controller en de bijbehorende EBOD-controller wordt, blijft het apparaat werken. Als de verbinding tussen de actieve system-controller en de bijbehorende EBOD-controller verbroken is, failover moet worden uitgevoerd en het apparaat moet blijven bewerken als normale.

Als beide kabels seriële bijgevoegd SCSI (SA's) worden verwijderd of de verbinding tussen de ruimte EBOD en de primaire insluiting is verbroken, werkt het apparaat niet meer. Nu de volgende stappen uitvoeren.

### <a name="to-turn-on-the-device-after-connection-is-lost"></a>Het apparaat inschakelen nadat de verbinding is verbroken

1. Toegang tot de achterzijde van het apparaat.

2. Als de kabelverbinding SA's tussen de ruimte EBOD en de primaire insluiting niet meer werkt, worden alle SA's lane LED's op de ruimte EBOD uitschakelen.

3. Sluit u zowel Power en koeling Modules (PCMs) op de ruimte EBOD en de primaire insluiting.

4. Wacht totdat alle gaan op de achterzijde van beide de bijlagen uitschakelen.

5. Plaats de kabels SA's, en ervoor zorgen dat er een goede verbinding tussen de ruimte EBOD en de primaire insluiting is.

6. Schakel de insluiting EBOD eerste door beide schakelopties PCM naar de stand spiegelen.

7. Zorg ervoor dat de insluiting EBOD is ingeschakeld door te schakelen dat de groene LED ingeschakeld is.

8. De primaire insluiting inschakelen.

9. Zorg ervoor dat de primaire insluiting is ingeschakeld door te schakelen dat de controller groene LED ingeschakeld is.

10. Controleer of de EBOD insluiting verbinding met de primaire insluiting goede door te schakelen dat de SA's baan LED's (vier per EBOD controller) zijn alle ON.

>[AZURE.IMPORTANT] Als de kabels SA's defect zijn of de verbinding tussen de ruimte EBOD en de primaire insluiting niet goed is, is wanneer u het systeem inschakelt, wordt deze herstel gaat. Neem [contact op met Microsoft ondersteuning](storsimple-contact-microsoft-support.md) als dit gebeurt.

## <a name="turn-off-a-running-device"></a>Een actieve apparaat uitschakelen

Een actieve StorSimple apparaat moet worden afgesloten als dit wordt verplaatst, die u hebt gemaakt buiten bedrijf, of een probleem veroorzaakt onderdeel dat moet worden vervangen heeft. De stappen verschillen afhankelijk van of het apparaat StorSimple een 8100 of een 8600-model. De 8100 heeft een één primaire insluiting, terwijl de 8600 een twee-insluiting-apparaat met een primaire insluiting en een insluiting EBOD is. In dit gedeelte worden de stappen voor het afsluiten van een lopende apparaat.

- [Apparaat instellen met primaire insluiting](#8100a)

- [Apparaat instellen met EBOD insluiting](#8600a)

### <a name="device-with-primary-enclosure-a-name8100a"></a>Apparaat instellen met primaire insluiting<a name="8100a"> 

Als u wilt in een ordelijk en gecontroleerde wijze op het apparaat wordt afgesloten, kunt u dit doen via de portal van Azure klassieke of via de Windows PowerShell voor StorSimple. 

>[AZURE.IMPORTANT] Niet worden afgesloten een actieve apparaat met behulp van de knop power aan de achterzijde van het apparaat.
>
>Voordat u het apparaat afsluit, zorg dat alle apparaatonderdelen in orde zijn. Ga in de portal voor Azure klassieke naar **apparaten** > **onderhoud** > **Hardware Status**, en controleer of de status van alle onderdelen groene. Dit geldt alleen voor een correct systeem. Als het systeem is wordt afsluiten om het probleem veroorzaakt onderdeel vervangt, u ziet een mislukte (rood) of niet beschikbaar is weergegeven (geel) status voor het desbetreffende onderdeel in de **Status van de Hardware**.

Wanneer u de Windows PowerShell voor StorSimple of de Azure klassieke-portal, volg de stappen in [een apparaat StorSimple afgesloten](storsimple-manage-device-controller.md#shut-down-a-storsimple-device). 

### <a name="device-with-ebod-enclosure-a-name8600a"></a>Apparaat instellen met EBOD insluiting<a name="8600a">

>[AZURE.IMPORTANT] Zorg ervoor dat alle apparaatonderdelen in orde zijn voordat u de primaire ruimte en de ruimte EBOD afsluit. Ga in de portal voor Azure klassieke naar **apparaten** > **onderhoud** > **Hardware Status**, en controleer of alle onderdelen in orde zijn.

#### <a name="to-shut-down-a-running-device-with-ebod-enclosure"></a>Een actieve apparaat met EBOD insluiting afsluiten

1. Volg de stappen die zijn vermeld in het [afsluiten van een apparaat StorSimple](storsimple-manage-device-controller.md#shut-down-a-storsimple-device) voor de primaire insluiting.

2. Nadat de primaire insluiting wordt afgesloten, de EBOD afgesloten door spiegelen beide stroom uit en schakelt u koeling Module (PCM).

3. Om te bevestigen dat de EBOD heeft afgesloten, Controleer of alle gaan op de achtergrond van de ruimte EBOD uitschakelen.

>[AZURE.NOTE] De kabels SA's die worden gebruikt voor de ruimte EBOD verbinden met de primaire insluiting moeten niet worden verwijderd totdat nadat het systeem is afgesloten.

## <a name="next-steps"></a>Volgende stappen

[Contact opnemen met Microsoft ondersteuning](storsimple-contact-microsoft-support.md) als u problemen bij het inschakelen van of als u een apparaat StorSimple afsluit.

