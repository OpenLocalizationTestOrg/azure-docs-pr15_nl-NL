<properties 
   pageTitle="Vervang een controller StorSimple-apparaat | Microsoft Azure"
   description="Dit artikel wordt uitgelegd hoe verwijderen en vervangen van een of beide controller modules op uw apparaat StorSimple."
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

# <a name="replace-a-controller-module-on-your-storsimple-device"></a>Een controllermodule op uw apparaat StorSimple vervangen

## <a name="overview"></a>Overzicht

Deze zelfstudie wordt uitgelegd hoe verwijderen en vervangen van een of beide controller modules in een StorSimple-apparaat. Dit wordt ook de onderliggende logica voor de enkelvoudige en dubbele controller vervangende scenario's beschreven.

>[AZURE.NOTE] Voordat u een vervangende controller uitvoert, is het raadzaam dat u altijd uw controllerfirmware naar de nieuwste versie bijwerkt.
>
>Om te voorkomen dat schade aan uw apparaat StorSimple, niet verwijdert de controller totdat de LED's worden weergegeven als een van de volgende opties:
>
>- Alle gaan volgen uit.
>- LED-3, ![groen vinkje pictogram](./media/storsimple-controller-replacement/HCS_GreenCheckIcon.png), en ![rood kruis pictogram](./media/storsimple-controller-replacement/HCS_RedCrossIcon.png) knipperen, en LED 0 en LED 7 komen **aan**.

De volgende tabel ziet u de ondersteunde controller vervangende scenario's.

|Hoofdletters/kleine letters|Vervangende scenario|Toepasselijk procedure|
|:---|:-------------------|:-------------------|
|1|Één domeincontroller is defect, de andere controller orde en actief is.|[Eén controller vervangende](#replace-a-single-controller), waarin de [logica achter een vervangende één controller](#single-controller-replacement-logic), evenals de [vervangende stappen](#single-controller-replacement-steps).|
|2|Beide de controllers is mislukt en moeten worden vervangen. Het chassis, schijven, and.disk insluiting in orde zijn.|[Schermafbeelding van twee controller vervangende](#replace-both-controllers), waarin de [logica achter een vervangende dubbele controller](#dual-controller-replacement-logic), evenals de [vervangende stappen](#dual-controller-replacement-steps). |
|3|Van hetzelfde apparaat of vanaf andere apparaten worden omgewisseld. Het deelvenster chassis, schijven en schijf bijlagen in orde zijn.|Het waarschuwingsbericht van een slot verkeerde combinaties wordt weergegeven.|
|4|Eén controller ontbreekt en de andere controller mislukt.|[Schermafbeelding van twee controller vervangende](#replace-both-controllers), waarin de [logica achter een vervangende dubbele controller](#dual-controller-replacement-logic), evenals de [vervangende stappen](#dual-controller-replacement-steps).|
|5|Een of beide is mislukt. U geen toegang tot het apparaat via de seriële console of de Windows PowerShell externe communicatie.|[Contact opnemen met Microsoft ondersteuning](storsimple-contact-microsoft-support.md) voor een handmatige controller vervangende procedure.|
|6|De controllers bestaan uit een ander buildversie, die mogelijk vanwege:<ul><li>Controllers hebben een andere softwareversie.</li><li>Controllers hebben een andere firmwareversie.</li></ul>|Als de versies van de software controller verschillen, wordt de logica vervangende die wordt gedetecteerd en updates van de versie van de software op de controller vervangende.<br><br>Als de controller firmware-versies verschillende zijn en de oude firmwareversie is **niet** automatisch worden uitgebreid, een waarschuwingsbericht wordt weergegeven in de portal van Azure klassieke. U moet zoeken naar updates en installeer de updates firmware.</br></br>Als de controller firmware-versies verschillende zijn en de oude firmwareversie automatisch upgrade is, op de controller vervangende logica dit wordt gedetecteerd en nadat de controller wordt gestart, de firmware wordt automatisch bijgewerkt.|

U moet een controllermodule verwijderen als deze is mislukt. Een of beide de controller modules kunnen mislukken, hetgeen kan leiden tot een enkele controller vervangende of dubbele controller vervangende. Zie de volgende onderwerpen voor vervangende procedures en de logica daarachter:

- [Vervang één controller](#replace-a-single-controller)
- [Beide controllers vervangen](#replace-both-controllers)
- [Een controller verwijderen](#remove-a-controller)
- [Een controller invoegen](#insert-a-controller)
- [Het actieve controller op uw apparaat identificeren](#identify-the-active-controller-on-your-device)

>[AZURE.IMPORTANT] Controleer de gegevens van de veiligheid in [StorSimple hardware onderdeel vervangende](storsimple-hardware-component-replacement.md)voordat verwijderen en vervangen door een controller.

## <a name="replace-a-single-controller"></a>Vervang één controller

Wanneer een van de twee op Microsoft Azure StorSimple apparaat is mislukt, is defect of ontbreekt, moet u één controller vervangen. 

### <a name="single-controller-replacement-logic"></a>Eén controller vervangende logica

In één controller vervanging, moet u eerst de mislukte controller verwijderen. (De resterende controller in het apparaat is de actieve controller.) Wanneer u de vervangende controller invoegt, gebeurt het volgende:

1. De vervangende controller wordt onmiddellijk gestart communicatie met het apparaat StorSimple.

2. Een momentopname van de virtuele harde schijf (VHD) voor de actieve controller wordt gekopieerd van de besturing vervangende.

3. De momentopname is gewijzigd, zodat wanneer de vervangende controller wordt gestart vanaf deze VHD, wordt deze herkend als een stand-by controller.

4. Wanneer de wijzigingen aangebracht hebt, wordt de vervangende controller als de stand-by controller gestart.

5. Wanneer de beide controllers actief is, wordt het cluster geleverd online.

### <a name="single-controller-replacement-steps"></a>Eén controller vervangende stappen

Voer de volgende stappen uit als een van de in uw apparaat Microsoft Azure StorSimple mislukt. (De andere controller moet actief en wordt uitgevoerd. Als beide controllers mislukt of er problemen optreden, gaat u naar [dubbele controller vervangende stappen](#dual-controller-replacement-steps).)

>[AZURE.NOTE] Het kan 30-45 minuten voor de controller opnieuw opstarten en volledig herstellen uit de procedure van één controller vervangende duren. De totale tijd voor de gehele procedure, is inclusief de kabels koppelen ongeveer 2 uur.

#### <a name="to-remove-a-single-failed-controller-module"></a>Als u wilt verwijderen van één is mislukt controllermodule

1. In de klassieke Azure portal, gaat u naar de StorSimple Manager-service, klikt u op het tabblad **apparaten** en klik vervolgens op de naam van het apparaat dat u wilt controleren.

2. Ga naar **onderhoud > hardwarestatus**. De status van Controller 0 of 1 Controller moet rood, waarmee wordt aangegeven met een fout.

    >[AZURE.NOTE] De mislukte controller in één controller vervanging is altijd een stand-by controller.

3. Afbeelding 1 en de volgende tabel om te zoeken van de controllermodule mislukte gebruiken.  

    ![Backplane van apparaat primaire insluiting modules](./media/storsimple-controller-replacement/IC740994.png)

    **Afbeelding 1** Achterzijde van StorSimple-apparaat

  	|Label|Beschrijving|
  	|:----|:----------|
  	|1|PCM 0|
  	|2|PCM 1|
  	|3|Controller 0|
  	|4|Controller 1|

4. Klik op de mislukte controller, de verbonden netwerkkabels uit de gegevenspoorten te verwijderen. Als u een 8600 model gebruikt, moet u ook de SA's kabels die de controller verbinden met de controller EBOD verwijderen.

5. Volg de stappen in het [verwijderen van een controller](#remove-a-controller) de mislukte controller verwijderen. 

6. Installeer de vervangende factory in de dezelfde slot waaruit de mislukte controller is verwijderd. Hiermee wordt de logica van één controller vervangende geactiveerd. Zie [één controller vervangende logica](#single-controller-replacement-logic)voor meer informatie.

7. Terwijl de logica van één controller vervangende op de achtergrond verloopt, sluit u de kabels. Handelingen kunt verrichten precies dezelfde manier dat ze zijn verbonden voordat u de vervanging van alle de kabels verbindt.

8. Nadat de controller opnieuw is opgestart, Controleer de **status van de Controller** en de **status Cluster** in de portal van Azure klassieke om te bevestigen dat de controller terug naar de orde zijn en stand-by staat.

>[AZURE.NOTE] Als u het apparaat via de seriële console controleren wilt, ziet u mogelijk meerdere opnieuw is opgestart terwijl de controller uit de procedure vervangende worden hersteld. Wanneer het seriële console-menu wordt weergegeven, weet u dat de vervangende voltooid is. Als het menu binnen twee uur van het starten van de vervangende controller niet wordt weergegeven, neemt u [contact opnemen met Microsoft ondersteuning](storsimple-contact-microsoft-support.md).

## <a name="replace-both-controllers"></a>Beide controllers vervangen

Als beide controllers op het apparaat dat Microsoft Azure StorSimple is mislukt, zijn defect of ontbreken, moet u beide controllers vervangen. 

### <a name="dual-controller-replacement-logic"></a>Schermafbeelding van twee controller vervangende logica

In een vervangende dubbele controller u eerst verwijderen beide mislukte controllers en voegt u vervanging. Als de twee vervangende controllers wordt geplaatst, gebeurt het volgende:

1. De vervangende controller in slot 0, wordt het volgende gecontroleerd:
 
   1. Is dit de huidige versies van de firmware en software via?

   2. Dit is een onderdeel van het cluster?

   3. De peer-controller uitgevoerd en is het gegroepeerde?
                            
    Als geen van deze voorwaarden voldaan wordt, de controller Hiermee wordt gezocht naar de meest recente dagelijkse back-up (zich in de **nonDOMstorage** op station S). De meest recente momentopname van de VHD de controller opgehaald uit de back-up.

2. De controller in slot 0 wordt de momentopname om de afbeelding zelf.

3. De controller in slot 1 is ondertussen wacht van controller 0 de installatiekopieën voltooien en de starten.

4. Nadat de controller 0 wordt gestart, vastgesteld controller 1 het cluster gemaakt door controller 0, wat inhoudt dat de logica van één controller vervangende geactiveerd. Zie [één controller vervangende logica](#single-controller-replacement-logic)voor meer informatie.

5. Achteraf, beide domeincontrollers uitvoert en het cluster online komt.

>[AZURE.IMPORTANT] Na een vervangende dubbele controller nadat het StorSimple-apparaat is geconfigureerd, is het essentiële dat u een handmatig back-up van het apparaat maakt. Dagelijkse apparaat configuratie back-ups worden niet geactiveerd tot nadat 24 uur is verstreken. Werken met [Microsoft ondersteuning](storsimple-contact-microsoft-support.md) om te vragen de handmatige back-up van uw apparaat.

### <a name="dual-controller-replacement-steps"></a>Schermafbeelding van twee controller vervangende stappen

Deze werkstroom is vereist als beide van de controllers in uw apparaat Microsoft Azure StorSimple is mislukt. Dit kan gebeuren in een datacenter waarin de koelsysteem werkt niet en daardoor beide de controllers mislukt binnen een korte termijn. Afhankelijk van of het apparaat StorSimple of is uitgeschakeld en of u een 8600 gebruikt of een model 8100, is een andere set stappen vereist.

>[AZURE.IMPORTANT] Op 1 uur staan voor de controller opnieuw opstarten en volledig herstellen uit een schermafbeelding van twee controller vervangende procedure, kan dit 45 minuten duren. De totale tijd voor de gehele procedure, is inclusief de kabels koppelen ongeveer 2,5 uur.

#### <a name="to-replace-both-controller-modules"></a>Voor het vervangen van beide controller-modules

1. Als het apparaat is uitgeschakeld, wordt deze stap overslaan en gaat u verder met de volgende stap. Als het apparaat is ingeschakeld, gebruikt u de uitschakelen van het apparaat.
                                        
    1. Als u een model 8600 gebruikt, het primaire insluiting eerste uitschakelen en klik vervolgens onder de insluiting EBOD uitschakelen.

    2. Wacht totdat het apparaat volledig is afgesloten. Alle LED's aan de achterzijde van het apparaat is uitgeschakeld.

2. Verwijder de netwerkkabels die met de gegevenspoorten verbonden zijn. Als u een 8600 model gebruikt, moet u ook de SA's kabels die de primaire insluiting verbinden met de EBOD insluiting verwijderen.

3. Verwijder beide controllers van het apparaat StorSimple. Zie [een controller verwijderen](#remove-a-controller)voor meer informatie.

4. De vervangende factory voor Controller 0 eerst invoegen en voeg Controller-1. Zie [een controller invoegen](#insert-a-controller)voor meer informatie. Hierdoor wordt de dubbele controller vervangende logica. Zie [dubbele controller vervangende logica](#dual-controller-replacement-logic)voor meer informatie.

5. Terwijl de logica van de vervangende controller op de achtergrond verloopt, sluit u de kabels. Handelingen kunt verrichten precies dezelfde manier dat ze zijn verbonden voordat u de vervanging van alle de kabels verbindt. Zie de gedetailleerde instructies voor het model in de kabel aan de sectie van uw apparaat van [uw apparaat StorSimple 8100 installeren](storsimple-8100-hardware-installation.md) of [uw apparaat StorSimple 8600 installeren](storsimple-8600-hardware-installation.md).

6. Schakel op het apparaat StorSimple. Als u een model 8600 gebruikt:

    1. Zorg ervoor dat de insluiting EBOD eerste is ingeschakeld.

    2. Wacht totdat de insluiting EBOD wordt uitgevoerd.

    3. De primaire insluiting inschakelen.

    4. Nadat de eerste domeincontroller opnieuw is opgestart en in orde zijn is, wordt het systeem wordt uitgevoerd.

    >[AZURE.NOTE] Als u het apparaat via de seriële console controleren wilt, ziet u mogelijk meerdere opnieuw is opgestart terwijl de controller uit de procedure vervangende worden hersteld. Wanneer het seriële console-menu wordt weergegeven, weet u dat de vervangende voltooid is. Als het menu binnen 2,5 uur van het starten van de vervangende controller niet wordt weergegeven, neemt u [contact opnemen met Microsoft ondersteuning](storsimple-contact-microsoft-support.md).

## <a name="remove-a-controller"></a>Een controller verwijderen

Gebruik de volgende procedure een controllermodule defect uit uw apparaat StorSimple verwijderen.

>[AZURE.NOTE] De volgende afbeeldingen zijn geschikt voor controller 0. Voor controller 1, zou deze worden omgekeerd.

#### <a name="to-remove-a-controller-module"></a>Een controllermodule verwijderen

1. Het slot module tussen uw duim en wijsvinger te vatten.

2. Voorzichtig verondersteld uw duim en wijsvinger samen om vrij te geven het slot controller.

    ![Controller slot vrijgeven](./media/storsimple-controller-replacement/IC741047.png)

    **Afbeelding 2** Controller slot vrijgeven

2. Gebruik het slot als ingang naar de controller uit het chassis dia.

    ![Schuifregelaar controller uit chassis](./media/storsimple-controller-replacement/IC741048.png)

    **Afbeelding 3** Schuif de controller uit het chassis

## <a name="insert-a-controller"></a>Een controller invoegen

Gebruik de volgende procedure voor het installeren van een fabriek meegeleverde controllermodule nadat u een defect module uit uw apparaat StorSimple verwijderen.

#### <a name="to-install-a-controller-module"></a>Een controllermodule installeren

1. Controleer of er momenteel geen schade aan de verbindingslijnen interface is. Installeer de module niet als een van de pennen verbindingslijn gebogen of beschadigd.

2. Schuif de controllermodule in het chassis terwijl het slot volledig is uitgebracht. 

    ![Schuifregelaar controller in chassis](./media/storsimple-controller-replacement/IC741053.png)

    **Afbeelding 4** Schuifregelaar controller in het chassis

3. Met de controllermodule ingevoegd, begint het slot terwijl u de controllermodule push in het chassis sluiten. Het slot wordt uitgevoerd op de controller begeleiden naar de juiste plaats.

    ![Controller slot sluiten](./media/storsimple-controller-replacement/IC741054.png)

    **Afbeelding 5** Het slot controller sluiten

4. U klaar bent met het wanneer het slot wordt vastgemaakt naar de juiste plaats. De LED **OK** moet nu op.  

    >[AZURE.NOTE] Het kan maximaal 5 minuten duren voordat de controller en de LED te activeren.

5. Om te bevestigen dat de vervangende geslaagd, in de portal van Azure klassieke is, gaat u naar **apparaten** > **onderhoud** > **Hardware Status**, en zorg ervoor dat zowel de controller 0 als de controller 1 in orde zijn (status is groen).

## <a name="identify-the-active-controller-on-your-device"></a>Het actieve controller op uw apparaat identificeren

Zijn er veel situaties, zoals de eerste keer apparaat registratie of controller vervangende, waarvoor u het actieve controller vinden op een apparaat StorSimple. Het actieve controller verwerkt alle schijf firmware en netwerken bewerkingen. U kunt een van de volgende methoden gebruiken om aan te geven van de actieve controller uit:

- [De portal van Azure klassieke gebruiken om te identificeren van de actieve controller](#use-the-azure-classic-portal-to-identify-the-active-controller)

- [Windows PowerShell voor StorSimple gebruiken om aan te geven van de actieve controller](#use-windows-powershell-for-storsimple-to-identify-the-active-controller)

- [Het apparaat fysiek om te identificeren van de actieve controller controleren](#check-the-physical-device-to-identify-the-active-controller)

Elk van deze procedures is nu beschreven.

### <a name="use-the-azure-classic-portal-to-identify-the-active-controller"></a>De portal van Azure klassieke gebruiken om te identificeren van de actieve controller

Ga in de portal voor Azure klassieke naar **apparaten** > **onderhoud**en gaat u naar de sectie **Controllers** . U kunt hier controleren welke controller actief is.

![Actieve controller in Azure klassieke portal identificeren](./media/storsimple-controller-replacement/IC752072.png)

**Afbeelding 6** Azure klassieke portal met de actieve controller

### <a name="use-windows-powershell-for-storsimple-to-identify-the-active-controller"></a>Windows PowerShell voor StorSimple gebruiken om aan te geven van de actieve controller

Wanneer u rechtstreeks toegang uw apparaat via de seriële console tot, wordt een bannerbericht weergegeven. Het bannerbericht bevat eenvoudige apparaatgegevens, zoals het model, de naam, de geïnstalleerde versie en de status van de controller die u toegang krijgt tot. De volgende afbeelding ziet u een voorbeeld van een bannerbericht:

![Seriële bannerbericht](./media/storsimple-controller-replacement/IC741098.png)

**Afbeelding 7** Bericht waarin controller 0 als actief banner

U kunt het bannerbericht gebruiken om te bepalen of de controller die er verbinding is met actieve of passieve.

### <a name="check-the-physical-device-to-identify-the-active-controller"></a>Het apparaat fysiek om te identificeren van de actieve controller controleren

Als u wilt identificeren de actieve controller op uw apparaat, zoek de blauwe LED boven de gegevens 5 poort aan de achterzijde van de primaire insluiting.

Als deze LED is knipperende, de controller actief is en de andere bevindt zich in de modus Stand-by. Gebruik het volgende diagram en de volgende tabel als hulpmiddel.

![Apparaat primaire insluiting backplane met dataports](./media/storsimple-controller-replacement/IC741055.png)

**Figuur 8** Achterzijde van primaire insluiting met gegevenspoorten en controle LED 's

|Label|Beschrijving|
|:----|:----------|
|1-6|GEGEVENS 0 – 5 netwerkpoorten|
|7|Blauwe LED|


## <a name="next-steps"></a>Volgende stappen

Meer informatie over [StorSimple hardware onderdeel vervangende](storsimple-hardware-component-replacement.md).
