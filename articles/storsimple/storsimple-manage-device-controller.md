<properties
   pageTitle="StorSimple apparaatcontrollers beheren | Microsoft Azure"
   description="Leer hoe u stoppen, opnieuw, afgesloten of opnieuw instellen van uw apparaatcontrollers StorSimple."
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
   ms.date="10/11/2016"
   ms.author="alkohli" />

# <a name="manage-your-storsimple-device-controllers"></a>Uw apparaatcontrollers StorSimple beheren

## <a name="overview"></a>Overzicht

Deze zelfstudie worden de verschillende bewerkingen die kunnen worden uitgevoerd op uw apparaatcontrollers StorSimple. De controllers in uw apparaat StorSimple zijn overtollige (peer) domeincontrollers in een actieve-passieve-configuratie. Op een bepaald moment slechts één controller actief is en alle schijf en netwerk bewerkingen wordt verwerkt. De andere controller is in een passieve modus. Als de actieve controller mislukt, actief de passieve controller automatisch.

Deze zelfstudie omvat stapsgewijze instructies voor het beheren van apparaten met behulp van de:

- Sectie van **de onderhoudspagina voor in de service StorSimple Manager** **controllers**
- Windows PowerShell voor StorSimple.

Het is raadzaam dat u de apparaatcontrollers via de StorSimple Manager-service beheren. Als u een actie kan alleen worden uitgevoerd met behulp van Windows PowerShell voor StorSimple, kunt u de zelfstudie OK.

Na het lezen van deze zelfstudie, wordt u zichtbaar mogen zijn:

- Opnieuw opstarten of een StorSimple apparaat controller afsluiten
- Een apparaat StorSimple afsluiten
- Uw apparaat StorSimple fabrieksinstellingen herstellen


## <a name="restart-or-shut-down-a-single-controller"></a>Opnieuw opstarten of één controller afsluiten

Een controller opnieuw opstarten of afsluiten is niet vereist als onderdeel van de werking van het normale systeem. Afsluitbewerkingen voor een controller één apparaat gelden alleen in gevallen waarin een apparaat hardware-onderdeel moet worden vervangen. Een herstart controller kan ook in een situatie waarin prestaties wordt beïnvloed door overtollige geheugengebruik of een probleem veroorzaakt controller nodig zijn. Ook wellicht moet u start een domeincontroller na een geslaagde controller vervanging, als u wilt inschakelen en testen van de controller uit vervangen.

Opnieuw starten van een apparaat is niet verbonden initiators, ervan uitgaande dat er dat de passieve controller is beschikbaar wordt verplaatst. Als een passieve controller niet is beschikbaar is of is uitgeschakeld uitgeschakeld, klikt u opnieuw starten van de actieve controller kan dit leiden tot de verstoring van service en downtime.

> [AZURE.IMPORTANT]

> - **Een actieve controller moet nooit fysiek worden verwijderd als dit in een verlies van redundantie en een verbeterde kans op pesterijen downtime resulteert.**

> - De volgende procedure geldt alleen voor het StorSimple fysieke apparaat. Voor informatie over het starten, stoppen en opnieuw starten van het virtuele apparaat, raadpleegt u [werken met het virtuele apparaat](storsimple-virtual-device-u2.md#work-with-the-storsimple-virtual-device).

U kunt opnieuw opstarten of een enkel apparaat controller afsluiten met behulp van de Azure klassieke portal van de Manager StorSimple service of de Windows PowerShell voor StorSimple

Als u wilt uw apparaatcontrollers beheren vanuit de Azure klassieke-portal, kunt u de volgende stappen uitvoeren.

#### <a name="to-restart-or-shut-down-a-controller-in-classic-portal"></a>Opnieuw te starten of afsluiten van een controller in klassieke-portal

1. Navigeer naar **apparaten > onderhoud**.

1. Ga naar de **Status van de Hardware** en controleer of de status van beide de controllers op uw apparaat is **orde**.

    ![Controleer of StorSimple apparaatcontrollers in orde zijn](./media/storsimple-manage-device-controller/IC766017.png)

1. Vanaf de onderkant van **de onderhoudspagina voor** , klikt u op **Domeincontrollers beheren**.

    ![StorSimple apparaatcontrollers beheren](./media/storsimple-manage-device-controller/IC766018.png)</br>

    >[AZURE.NOTE] Als u **Domeincontrollers beheren**niet ziet, moet u updates installeren. Zie [uw apparaat StorSimple bijwerken](storsimple-update-device.md)voor meer informatie.

1. Ga als volgt te werk in het dialoogvenster **Instellingen van de Controller wijzigen** :
    1. Selecteer in de vervolgkeuzelijst **Select Controller** de controller uit die u wilt beheren. De opties zijn Controller 0 en 1 Controller. Deze controllers zijn ook die zijn geïdentificeerd als actieve of passieve.

        >[AZURE.NOTE] Een controller kan niet worden beheerd als deze is niet beschikbaar is of is uitgeschakeld uitgeschakeld en deze niet wordt weergegeven in de vervolgkeuzelijst.

    2. Kies in de vervolgkeuzelijst **Actie selecteren** **opnieuw opstarten controller** of **controller afsluiten**.

        ![Start StorSimple apparaat passieve domeincontroller](./media/storsimple-manage-device-controller/IC766020.png)
    3. Klik op het pictogram controleren ![Pictogram controleren](./media/storsimple-manage-device-controller/IC740895.png).

Hiermee wordt opnieuw opstarten of de controller afsluiten. De onderstaande tabel bevat een overzicht van de details van wat er gebeurt afhankelijk van de selecties die u hebt aangebracht in het dialoogvenster **Controller-instellingen wijzigen** .  


|Selectie #|Als u cookies...|Dit gebeurt.|
|---|---|---|
|1.|Start de passieve domeincontroller.|Een taak wordt gemaakt om de controller opnieuw te starten en u krijgt nadat de taak is gemaakt. Hiermee wordt de controller opnieuw starten initiëren. U kunt het proces opnieuw starten via controleren **Service > Dashboard > bewerking logboeken bekijken** en klik vervolgens filteren op parameters die specifiek zijn voor uw service.|
|2.|Start de actieve domeincontroller.|Ziet u de volgende waarschuwing: "Als u het actieve controller opstarten, het apparaat, mislukt boven aan de passieve controller. Wilt u om door te gaan?" </br>Als u wilt doorgaan met deze bewerking, de volgende stappen wordt niet identiek zijn aan die worden gebruikt om de passieve controller opnieuw te starten (Zie selectie 1).|
|3.|De passieve controller afgesloten.|Ziet u het volgende bericht: 'wanneer afsluiten voltooid is, moet u naar de knop power push op de controller te schakelen. Weet u zeker dat u wilt deze controller afsluiten?" </br>Als u wilt doorgaan met deze bewerking, de volgende stappen wordt niet identiek zijn aan die worden gebruikt om de passieve controller opnieuw te starten (Zie selectie 1).|
|4.|Het actieve controller afgesloten.|Ziet u het volgende bericht: 'wanneer afsluiten voltooid is, moet u naar de knop power push op de controller te schakelen. Weet u zeker dat u wilt deze controller afsluiten?" </br>Als u wilt doorgaan met deze bewerking, de volgende stappen wordt niet identiek zijn aan die worden gebruikt om de passieve controller opnieuw te starten (Zie selectie 1).|


#### <a name="to-restart-or-shut-down-a-controller-in-windows-powershell-for-storsimple"></a>Opnieuw te starten of een controller in Windows PowerShell voor StorSimple afsluiten
De volgende stappen als u wilt afsluiten of opnieuw opstarten één controller op uw apparaat StorSimple van de Azure klassieke portal uitvoeren.


1. Toegang tot het apparaat met behulp van de seriële console of een telnetsessie vanuit een externe computer. Verbinding maken met Controller 0 of 1 Controller volgens de stappen in [Gebruik stopverf verbinding maken met de seriële apparaat-console](storsimple-deployment-walkthrough.md#use-putty-to-connect-to-the-device-serial-console).

1. Kies optie 1, **aanmelden met de volledige toegang**in het menu seriële console.

1. Klik in het bericht banner noteert u de controller er verbinding is met (Controller 0 of Controller-1) en het actieve of de passieve controller uit (stand-by).
    - Typ het volgende als u één controller, bij de prompt wilt afsluiten:

        `Stop-HcsController`

        Hiermee sluit u de controller uit die u met verbonden zijn. Als u stopt met het actieve controller, wordt klikt u vervolgens het niet via de passieve controller voordat deze wordt afgesloten.
    - Typ het volgende om een controller, bij de prompt opnieuw te starten:

        `Restart-HcsController`

        Hiermee wordt de controller uit die u met verbonden zijn opnieuw. Als u het actieve controller opnieuw opstart, wordt het niet via de passieve controller uit voordat u het opnieuw starten.


## <a name="shut-down-a-storsimple-device"></a>Een apparaat StorSimple afsluiten

In deze sectie wordt uitgelegd hoe een voorlopig of een mislukte StorSimple hulpmiddel uit een externe computer afgesloten. Een apparaat is uitgeschakeld na de apparaatcontrollers afgesloten. Een afsluiting van het apparaat is gereed wanneer het apparaat fysiek wordt verplaatst of afmelden bij de service is die u hebt gemaakt.

> [AZURE.IMPORTANT] Voordat u op het apparaat wordt afgesloten, kunt u de status van de Apparaatonderdelen controleren. Navigeer naar **apparaten > onderhoud > Hardware Status** en controleer of de LED-status van alle onderdelen groene. Alleen een correct apparaat heeft een groene status. Als uw apparaat is wordt afsluiten om het probleem veroorzaakt onderdeel vervangt, ziet u een mislukte (rood) of de status van een anders werken (geel) voor de desbetreffende onderdelen.

#### <a name="to-shut-down-a-storsimple-device"></a>Een apparaat StorSimple afsluiten

1. Gebruik de procedure [opnieuw opstarten of afsluiten van een controller](#restart-or-shut-down-a-single-controller) herkennen en afsluiten het passieve controller op uw apparaat. U kunt deze bewerking uitvoeren in de portal van Azure klassieke of in Windows PowerShell voor StorSimple.
2. Herhaal deze stap de actieve controller afsluiten.
3. U moet nu kijkt u naar het vorige vlak van het apparaat. Nadat de twee controllers volledig worden afgesloten, moet de status LED's op beide de domeincontrollers rood knipperende. Als u het apparaat volledig uitschakelen op dit moment moet, spiegelen de schakelopties power zowel Power als koeling Modules (PCMs) op in de stand uit. Dit moet het apparaat uitschakelen.


<!--#### To shut down a StorSimple device in Windows PowerShell for StorSimple

1. Connect to the serial console of the StorSimple device by following the steps in [Use PuTTY to connect to the device serial console](storsimple-deployment-walkthrough.md#use-putty-to-connect-to-the-serial-console).

1. In the serial console menu, verify from the banner message that the controller you are connected to is the passive controller. If you are connected to the active controller, disconnect from this controller and connect to the other controller.

1. In the serial console menu, choose option 1, **log in with full access**.

1. At the prompt, type:

    `Stop-HCSController`

    This should shut down the current controller. To verify whether the shutdown has finished, check the back of the device. The controller status LED should be solid red.

1. Repeat steps 1 through 4 to connect to the active controller and then shut it down.

1. After both the controllers are completely shut down, the status LEDs on both should be blinking red. If you need to turn off the device completely at this time, flip the power switches on both Power and Cooling Modules (PCMs) to the OFF position.-->

## <a name="reset-the-device-to-factory-default-settings"></a>Het apparaat opnieuw standaardinstellingen

> [AZURE.IMPORTANT] Als u het moet herstellen van uw apparaat standaardinstellingen, neemt u contact op met Microsoft Support. De onderstaande procedure moet worden gebruikt alleen in combinatie met Microsoft Support.

Deze procedure wordt beschreven hoe u uw apparaat Microsoft Azure StorSimple standaardinstellingen via Windows PowerShell voor StorSimple opnieuw in te stellen.
Verwijdert alle gegevens en instellingen opnieuw instellen van een apparaat uit het hele cluster al dan niet standaard.

De volgende stappen als u uw apparaat Microsoft Azure StorSimple standaardinstellingen opnieuw wilt uitvoeren:

### <a name="to-reset-the-device-to-default-settings-in-windows-powershell-for-storsimple"></a>Het apparaat naar standaardinstellingen in Windows PowerShell voor StorSimple opnieuw in te stellen

1. Het apparaat rechtstreeks toegang tot via de seriële console. Controleer het bericht banner om ervoor te zorgen dat u met de actieve controller verbonden bent.

1. Kies optie 1, **aanmelden met de volledige toegang**in het menu seriële console.

1. Typ bij de prompt de volgende opdracht uit het hele cluster, verwijdert alle gegevens, metagegevens en controller instellingen opnieuw in te stellen:

    `Reset-HcsFactoryDefault`

    Als u in plaats daarvan herstellen één controller, gebruikt u de cmdlet [Beginwaarden-HcsFactoryDefault](http://technet.microsoft.com/library/dn688132.aspx) met de `-scope` parameter.)

    Het systeem opnieuw meerdere keren. U krijgt wanneer de beginwaarden is voltooid. Afhankelijk van het systeemmodel, kan dit 45-60 minuten voor een 8100 apparaat en een 8600 om te voltooien van dit proces 60-90 minuten duren.

    > [AZURE.TIP]

    > - Als u Update 1.2 gebruikt of eerder met de `–SkipFirmwareVersionCheck` -parameter voor het controleren van de versie firmware overslaan (anders ziet u een firmware overeenkomen: Factory Beginwaarden kan niet worden voortgezet vanwege een probleem in de firmware-versies. ).

    > - De procedure van de beginwaarden factory kan mislukken voor StorSimple-apparaten die Update 1 of 1.1 worden uitgevoerd in de portal voor de overheid en een vervangende succesvolle enkele of dubbele controller (met vervangende controllers die zijn verzonden met oude Update 1-software) hebt uitgevoerd. Dit gebeurt wanneer de fabriek opnieuw afbeelding is gevalideerd voor de aanwezigheid van een bestand SHA1 op de controller uit die niet bestaat voor oude Update 1-software. Als u dat deze factory opnieuw mislukt ziet, neemt u contact op met Microsoft Support om u te helpen met de volgende stappen. Dit probleem is niet zichtbaar met vervangende controllers die zijn verzonden vanuit de fabriek met Update 1 of later software.


## <a name="questions-and-answers-about-managing-device-controllers"></a>Vragen en antwoorden over het beheren van apparaatcontrollers

In dit gedeelte samenvatten we enkele veelgestelde vragen met betrekking tot beheren StorSimple apparaatcontrollers.

**Q.** Wat gebeurt er als beide de controllers op mijn apparaat in orde en gedraaide zijn op en ik opnieuw opstarten of de actieve controller afsluiten?

**A.** Als u zowel de controllers op uw apparaat zijn in orde en gedraaide, wordt u gevraagd om bevestiging. U kunt:

- **Start de actieve domeincontroller** – u wordt geïnformeerd dat opnieuw starten van een actieve controller wordt dat het apparaat naar de passieve controller mislukken. De controller opnieuw gestart.

- **Een actieve controller afgesloten** – krijgt u een melding dat een actieve controller afgesloten downtime treden. U moet ook de knop power push op het apparaat om te schakelen op de controller uit.

**Q.** Wat gebeurt er als de passieve controller op mijn apparaat niet beschikbaar is of is uitgeschakeld is uit en ik opnieuw opstarten of de actieve controller afsluiten?

**A.** Als de passieve controller op uw apparaat is niet beschikbaar is of is uitgeschakeld uitgeschakeld en u wilt:

- **Start de actieve domeincontroller** – u wordt geïnformeerd dat doorgaan met de bewerking in een tijdelijke onderbreking van de service resulteert en wordt u gevraagd om bevestiging.

- **Een actieve controller afgesloten** – u wordt geïnformeerd dat doorgaan met de bewerking leidt tot downtime en dat u moet de knop power push op een of beide domeincontrollers inschakelen van het apparaat. U wordt gevraagd om bevestiging.

**Q.** Wanneer de controller opnieuw opstarten of afsluiten doet mislukt verlopen?

**A.** Opnieuw opstarten of afsluiten van een controller mislukt mogelijk als:

- Een update van het apparaat wordt uitgevoerd.

- Een herstart controller, is al bezig.

- Een controller afsluiten, is al bezig.

**Q.** Hoe kunt u duidelijk als een controller is opgestart of afgesloten?

**A.** U kunt de controller-status op de pagina Onderhoud controleren. De controller-status wordt aangegeven of een controller is opgestart of afgesloten. De pagina waarschuwingen bevatten ook een informatieve waarschuwing als de controller is opgestart of afgesloten. De controller opnieuw opstarten en afsluiten bewerkingen zijn ook vastgelegd in de bewerking. Ga naar het [weergeven van de logboeken aan de bewerking](storsimple-service-dashboard.md#view-the-operations-logs)voor meer informatie over bewerking Logboeken.

**Q.** Is er een invloed op de i/o grond controller failover?

**A.** De TCP-verbindingen tussen initiators en actieve controller worden opnieuw ingesteld grond controller failover, maar wordt pas gemaakt wanneer de passieve controller wordt ervan uitgegaan bewerking. Er zijn mogelijk een pauze tijdelijk (minder dan 30 seconden) in o-activiteit tussen initiators en het apparaat in de loop van deze bewerking.

**Q.** Hoe kan ik mijn controller naar service-nadat deze is afgesloten en verwijderd retourneren?

**A.** Als u wilt teruggaan naar een controller bij service, moet u dit invoegen in het chassis zoals is beschreven in [een controllermodule op uw apparaat StorSimple vervangen](storsimple-controller-replacement.md).

## <a name="next-steps"></a>Volgende stappen

- Als u problemen met uw StorSimple apparaatcontrollers die u niet kunt oplossen ondervindt met de procedures in deze zelfstudie [contact opnemen met Microsoft ondersteuning](storsimple-contact-microsoft-support.md)weergegeven.

- Meer informatie over het gebruik van de StorSimple Manager-service, gaat u naar [de StorSimple Manager-service voor het beheren van uw apparaat StorSimple gebruiken](storsimple-manager-service-administration.md).
