<properties 
   pageTitle="StorSimple momentopname Manager implementeren | Microsoft Azure"
   description="Informatie over het downloaden en installeren van de Manager van een StorSimple momentopname, een MMC-module voor het beheren van StorSimple gegevens protection en back-up-functies."
   services="storsimple"
   documentationCenter="NA"
   authors="SharS"
   manager="carmonm"
   editor="" />
<tags 
   ms.service="storsimple"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="TBD"
   ms.date="05/24/2016"
   ms.author="v-sharos" />

# <a name="deploy-the-storsimple-snapshot-manager-mmc-snap-in"></a>De module StorSimple momentopname Manager MMC implementeren

## <a name="overview"></a>Overzicht

De beheerder van de momentopname StorSimple is een Microsoft Management Console (MMC)-module waardoor het eenvoudiger gegevensbescherming en back-beheer in een Microsoft Azure StorSimple-omgeving wordt. Met StorSimple momentopname Manager, kunt u Microsoft Azure StorSimple on-premises implementatie beheren en cloud opslag alsof het een volledig geïntegreerde opslagsysteem, dus back-up- en herstelprocessen vereenvoudigen en kosten. 

Deze zelfstudie wordt beschreven configuratievereisten, evenals de procedures voor het installeren, verwijderen en een upgrade StorSimple momentopname Manager.

>[AZURE.NOTE] 
>
>- U kunt StorSimple momentopname Manager niet gebruiken voor het beheren van Microsoft Azure StorSimple virtuele matrices (ook wel bekend als StorSimple on-premises implementatie virtuele apparaten).
>
>- Als u van plan bent om te StorSimple Update 2 installeren op uw apparaat StorSimple, zorg ervoor dat u naar de meest recente versie van StorSimple momentopname Manager downloaden en installeren **voordat u StorSimple Update 2 installeert**. De meest recente versie van StorSimple momentopname Manager is compatibel en werkt met alle versies van Microsoft Azure StorSimple. Als u de vorige versie van StorSimple momentopname Manager gebruikt, moet u deze (u hoeft niet naar de vorige versie verwijderen voordat u de nieuwe versie installeert) bij te werken.

## <a name="storsimple-snapshot-manager-installation"></a>Installatie StorSimple momentopname Manager

StorSimple momentopname Manager kan worden geïnstalleerd op computers met de Windows Server 2008 R2 SP1, Windows Server 2012 of Windows Server 2012 R2-besturingssysteem. Op servers met Windows 2008 R2, moet u ook Windows Server 2008 SP1 en Windows Management Framework 3.0 installeren. 

Controleer voordat u installeert of upgrade van de module StorSimple momentopname Manager voor Microsoft Management Console (MMC), of dat de server van apparaat en host Microsoft Azure StorSimple correct zijn geconfigureerd. 

## <a name="configure-prerequisites"></a>Vereisten voor configureren

De volgende stappen bieden een overzicht van configuratietaken uit te voeren dat u moet worden voltooid voordat u de StorSimple momentopname Manager installeren. Zie [Deploy uw on-premises implementatie StorSimple apparaat](storsimple-deployment-walkthrough.md)voor volledige configuratie van Microsoft Azure StorSimple en setup informatie, inclusief systeemvereisten en stapsgewijze instructies.

>[AZURE.IMPORTANT]Voordat u begint, bekijk de [implementatie, Configuratiecontrolelijst](storsimple-deployment-walkthrough.md#deployment-configuration-checklist) en en [vereisten voor implementatie](storsimple-deployment-walkthrough.md#deployment-prerequisites) in [uw on-premises implementatie StorSimple apparaat Deploy](storsimple-deployment-walkthrough.md).
> <br>
 
### <a name="before-you-install-storsimple-snapshot-manager"></a>Voordat u StorSimple momentopname Manager installeren

1. Uitpakken, koppelen en verbindt u het apparaat dat Microsoft Azure StorSimple zoals is beschreven in de [installatie van uw apparaat StorSimple 8100](storsimple-8100-hardware-installation.md) of [uw apparaat StorSimple 8600](storsimple-8600-hardware-installation.md).

2. Zorg ervoor dat uw hostcomputer een van de volgende besturingssystemen wordt uitgevoerd:

    - Windows Server 2008 R2 (op servers met Windows 2008 R2, moet u ook installeren Windows Server 2008 SP1 en Windows Management Framework 3.0)
    - Windows Server 2012
    - Windows Server 2012 R2
 
    Voor een virtueel StorSimple-apparaat moet de host van een Microsoft Azure Virtual Machine. 

3. Zorg ervoor dat u voldoet aan de vereisten voor de configuratie van Microsoft Azure StorSimple. Ga naar de [implementatie vereisten](storsimple-deployment-walkthrough.md#deployment-prerequisites)voor meer informatie.

4. Sluit het apparaat aan de host en de eerste configuratie uitvoeren. Ga naar de [implementatiestappen](storsimple-deployment-walkthrough.md#deployment-steps)voor meer informatie.

5. Volumes maken op het apparaat, toewijzen aan de host en controleer of de host kunt koppelen en deze gebruiken. StorSimple momentopname Manager ondersteunt de volgende typen van hoeveelheden: 

    - Standaardvolumes
    - Eenvoudige volumes
    - Dynamische volumes
    - Dynamische mirrorvolumes (RAID 1)
    - Cluster gedeelde clustervolumes
 
    Voor informatie over het maken van volumes op StorSimple apparaat of een virtueel apparaat StorSimple, gaat u naar [stap 6: een volume maken](storsimple-deployment-walkthrough.md#step-6-create-a-volume), in [uw on-premises implementatie StorSimple apparaat Deploy](storsimple-deployment-walkthrough.md).

## <a name="install-a-new-storsimple-snapshot-manager"></a>Een nieuwe StorSimple momentopname Manager installeren

Voordat u installeert StorSimple momentopname Manager, zorg dat de hoeveelheden u hebt gemaakt op het apparaat StorSimple of StorSimple virtueel apparaat zijn gekoppeld, geïnitialiseerd en zoals is beschreven in [configureren vereisten](#configure-prerequisites)die zijn opgemaakt.

>[AZURE.IMPORTANT]
>
>- Voor een virtueel StorSimple-apparaat moet de host van een Microsoft Azure Virtual Machine. 
>
>- De host moet worden uitgevoerd Windows 2008 R2, Windows Server 2012 of Windows Server 2012 R2. Als uw server wordt uitgevoerd van Windows Server 2008 R2, moet u ook Windows Server 2008 SP1 en Windows Management Framework 3.0 installeren.
>
>- Voordat u het apparaat naar StorSimple momentopname Manager kunt verbinden, moet u een iSCSI-verbinding van de host naar het apparaat StorSimple configureren.

Volg deze stappen om een nieuwe installatie van StorSimple momentopname Manager te voltooien. Als u een upgrade installeert, gaat u naar [upgraden of StorSimple momentopname Manager opnieuw installeren](#upgrade-or-reinstall-storsimple-snapshot-manager).

- Stap 1: StorSimple momentopname Manager installeren 
- Stap 2: StorSimple momentopname Manager koppelen aan een apparaat 
- Stap 3: Controleer of de verbinding met het apparaat 

###<a name="step-1-install-storsimple-snapshot-manager"></a>Stap 1: StorSimple momentopname Manager installeren

Gebruik de volgende stappen uit om te installeren StorSimple momentopname Manager.

#### <a name="to-install-storsimple-snapshot-manager"></a>StorSimple momentopname Manager installeren

1. De software StorSimple momentopname Manager (Ga naar [StorSimple momentopname Manager](https://www.microsoft.com/download/details.aspx?id=44220) in het Microsoft Download Center) downloaden en sla deze lokaal op de host.

2. Verkenner met de rechtermuisknop op de gecomprimeerde map en klik vervolgens op **alle uitpakken**.

3. Typ in het venster **gecomprimeerde extraheren (Zipped) mappen** in het vak **Selecteer een bestemming en bestanden ophalen** of blader naar het pad waar u archiveren wilt naartoe moet worden geëxtraheerd. 

       >[AZURE.IMPORTANT] U moet StorSimple momentopname Manager installeren op station C:.
 
4. Schakel het selectievakje **weergeven uitgepakte bestanden als alles compleet is** , en klik op **ophalen**.

    ![Dialoogvenster bestanden ophalen](./media/storsimple-snapshot-manager-deployment/HCS_SSM_extract_files.png) 

4. Wanneer het uitpakken is voltooid, wordt de doelmap geopend. Dubbelklik op het pictogram van de toepassing setup dat wordt weergegeven in de doelmap.

5. Wanneer het bericht is **Setup voltooid** wordt weergegeven, klikt u op **sluiten**. U moet het pictogram StorSimple momentopname Manager op uw bureaublad zien.

    ![bureaubladpictogram](./media/storsimple-snapshot-manager-deployment/HCS_SSM_desktop_icon.png) 

### <a name="step-2-connect-storsimple-snapshot-manager-to-a-device"></a>Stap 2: StorSimple momentopname Manager koppelen aan een apparaat

Gebruik de volgende stappen StorSimple momentopname Manager verbinden met een StorSimple-apparaat.

#### <a name="to-connect-storsimple-snapshot-manager-to-a-device"></a>StorSimple momentopname Manager aansluiten op een apparaat

1. Klik op het pictogram StorSimple momentopname Manager op uw bureaublad. Het venster StorSimple momentopname Manager wordt weergegeven. Het venster bevat het deelvenster met een **bereik** , een deelvenster met **resultaten** en een deelvenster **Documentacties** . 

    ![Manager van StorSimple momentopname-gebruikersinterface](./media/storsimple-snapshot-manager-deployment/HCS_SSM_gui_panes.png) 

    - Het deelvenster **bereik** (het linkerdeelvenster) bevat een overzicht van knooppunten in een boomstructuur georganiseerd. U kunt bepaalde Vouw om een weergave of de specifieke gegevens betrekking hebben op dat knooppunt te selecteren. Klik op het pijlpictogram wilt uitvouwen of samenvouwen van een knooppunt. Met de rechtermuisknop op een item in het deelvenster **bereik** om een lijst met beschikbare acties voor dat item weer te geven. 

    - Het deelvenster met **resultaten** (het middelste deelvenster) bevat gedetailleerde statusinformatie over het knooppunt, weergave of gegevens die u hebt geselecteerd in het deelvenster **bereik** .

    - Het deelvenster **Acties** bevat de bewerkingen die u kunt uitvoeren op het knooppunt, weergave of gegevens die u hebt geselecteerd in het deelvenster **bereik** .

    Zie voor een gedetailleerde beschrijving van de gebruikersinterface StorSimple momentopname Manager, [StorSimple momentopname Manager-gebruikersinterface](storsimple-use-snapshot-manager.md).

2. Klik in het deelvenster **bereik** met de rechtermuisknop op het knooppunt **apparaten** en klik vervolgens op **een apparaat configureren**. Het dialoogvenster **configureren een apparaat** wordt weergegeven.

    ![Een apparaat configureren](./media/storsimple-snapshot-manager-deployment/HCS_SSM_config_device.png) 

3. Selecteer het IP-adres van de Microsoft Azure StorSimple apparaat of een virtueel apparaat in de keuzelijst **apparaat** . Typ in het tekstvak **wachtwoord** het wachtwoord StorSimple momentopname Manager die u hebt gemaakt voor het apparaat in de portal van Azure klassieke. Klik op **OK**.

4. StorSimple momentopname Manager zoekt u naar het apparaat dat u identificeren. Als het apparaat dat beschikbaar is, voegt StorSimple momentopname Manager een verbinding. U kunt [de verbinding met het apparaat controleren](#to-verify-the-connection) om te bevestigen dat de verbinding is toegevoegd.

    Als het apparaat om een reden niet beschikbaar is, retourneert StorSimple momentopname Manager een foutbericht wordt weergegeven. Klik op **OK** om het foutbericht te sluiten en klik vervolgens op **Annuleren** om het dialoogvenster **een apparaat configureren** te sluiten.

5. Wanneer deze verbinding met een apparaat maakt, geïmporteerd StorSimple momentopname Manager elke groep het volume is geconfigureerd voor het apparaat, mits de volume-groep Back-ups is gekoppeld. Volume-groepen waarvoor geen bijbehorende back-ups niet geïmporteerd. Daarnaast kunnen back-beleidsregels die zijn gemaakt voor een groep volume niet geïmporteerd. Als u wilt zien van de geïmporteerde groepen, met de rechtermuisknop op het **Volume groepen** bovenste knooppunt in het deelvenster **bereik** en klik op **de wisselknop geïmporteerd groepen**.

### <a name="step-3-verify-the-connection-to-the-device"></a>Stap 3: Controleer of de verbinding met het apparaat

Gebruik de volgende stappen uit om te bevestigen dat StorSimple momentopname Manager is verbonden met het apparaat StorSimple.

#### <a name="to-verify-the-connection"></a>Om te controleren of de verbinding

1. Klik op het knooppunt **apparaten** in het deelvenster **bereik** .

    ![Apparaatstatus is StorSimple momentopname Manager](./media/storsimple-snapshot-manager-deployment/HCS_SSM_Device_status.png) 

2. Controleer het deelvenster **resultaten** : 

   - Als een groene indicator wordt weergegeven op het pictogram van het apparaat en **beschikbaar** in de kolom **Status** wordt weergegeven, is klikt u vervolgens het apparaat aangesloten. 

   - Als een rode indicator wordt weergegeven op het pictogram van het apparaat en niet beschikbaar in de kolom **Status** wordt weergegeven, is klikt u vervolgens het apparaat niet verbonden. 

   - Als het **vernieuwen** in de kolom **Status** wordt weergegeven, klikt u vervolgens ophalen StorSimple momentopname Manager volume-groepen en de bijbehorende back-ups voor een verbonden apparaat.

## <a name="upgrade-or-reinstall-storsimple-snapshot-manager"></a>Upgraden of StorSimple momentopname Manager opnieuw installeren

U moet StorSimple momentopname Manager volledig verwijderen voordat u upgraden of de software opnieuw installeren. 

Voordat u opnieuw installeert StorSimple momentopname Manager, back-up van de bestaande StorSimple momentopname Manager-database op de host. Hiermee wordt opgeslagen gegevens over de back-beleid en configuratie zodat u deze gegevens gemakkelijk uit back-up terugzetten kunt.

Volg deze stappen als u een upgrade of StorSimple momentopname Manager te installeren:

- Stap 1: Verwijder StorSimple momentopname Manager 
- Stap 2: Back-up van de StorSimple momentopname Manager-database 
- Stap 3: StorSimple momentopname Manager opnieuw installeren en de database herstellen 

### <a name="step-1-uninstall-storsimple-snapshot-manager"></a>Stap 1: Verwijder StorSimple momentopname Manager

Gebruik de volgende stappen om te StorSimple momentopname Manager te verwijderen.

#### <a name="to-uninstall-storsimple-snapshot-manager"></a>StorSimple momentopname Manager verwijderen

1. Klik op de hostcomputer, open het **Configuratiescherm**op **programma's**en klik vervolgens op **programma's en onderdelen**.

2. Klik in het linkerdeelvenster op **een programma verwijderen of wijzigen**.

3. Met de rechtermuisknop op **StorSimple momentopname Manager**en klik vervolgens op **verwijderen**.

4. Hiermee start u het installatieprogramma van StorSimple momentopname Manager. Klik op **Instellingen wijzigen**en klik vervolgens op **verwijderen**.

    >[AZURE.NOTE] Als er een MMC-processen worden uitgevoerd op de achtergrond, zoals StorSimple momentopname Manager of schijf Management, het verwijderen, mislukt en ontvangt u een bericht te sluiten van alle exemplaren van MMC voordat u probeert te verwijderen van het programma. Selecteer **toepassingen automatisch te sluiten en opnieuw openen nadat setup voltooid is**en klik vervolgens op **OK**.
 
5. Wanneer dit proces is voltooid, verschijnt er een **Setup Successful** bericht. Klik op **sluiten**.

### <a name="step-2-back-up-the-storsimple-snapshot-manager-database"></a>Stap 2: Back-up van de StorSimple momentopname Manager-database

Gebruik de volgende stappen uit om te maken en sla een kopie van de StorSimple momentopname Manager-database.

#### <a name="to-back-up-the-database"></a>Back-up van de database

1. De Microsoft StorSimple Management-Service stoppen:

   1. Start Server Manager.

   2. Selecteer op het Dashboard Serverbeheer, klikt u op het menu **Extra** **Services**.

   3. Selecteer op de pagina **Services** **Microsoft StorSimple Management-Service**.

   4. Klik in het rechterdeelvenster onder **StorSimple Management-Service van Microsoft**, op **de service stoppen**.

        ![De StorSimple Manager-service stoppen](./media/storsimple-snapshot-manager-deployment/HCS_SSM_stop_service.png)

2. Blader naar C:\ProgramData\Microsoft\StorSimple\BACatalog. 

    >[AZURE.NOTE] ProgramData is een verborgen map.

3. De catalogus XML-bestand hebt gevonden, kopieert u het bestand en de kopie opslaan in een veilige locatie of in de cloud.

    ![StorSimple catalogus met back-bestand](./media/storsimple-snapshot-manager-deployment/HCS_SSM_bacatalog.png)

4. De Microsoft StorSimple Management-Service opnieuw: 

    1. Selecteer op het Dashboard Serverbeheer, klikt u op het menu **Extra** **Services**.

    2. Selecteer de **Microsoft StorSimple Management**supportpakket op de pagina **Services** .

    3. Klik in het rechterdeelvenster onder **StorSimple Management-Service van Microsoft**, op **de service opnieuw starten**. 

### <a name="step-3-reinstall-storsimple-snapshot-manager-and-restore-the-database"></a>Stap 3: StorSimple momentopname Manager opnieuw installeren en de database herstellen

Volg de stappen in [een nieuwe StorSimple momentopname Manager installeren](#install-a-new-storsimple-snapshot-manager)om opnieuw StorSimple momentopname Manager, te installeren. Gebruik vervolgens de volgende procedure de StorSimple momentopname Manager-database terugzetten.

#### <a name="to-restore-the-database"></a>De database terugzetten

1. De Microsoft StorSimple Management-Service stoppen:

    1. Start Server Manager.

    2. Selecteer op het Dashboard Serverbeheer, klikt u op het menu **Extra** **Services**.

    3. Selecteer op de pagina **Services** **Microsoft StorSimple Management-Service**.

    4. Klik in het rechterdeelvenster onder **StorSimple Management-Service van Microsoft**, op **de service stoppen**.

2. Blader naar C:\ProgramData\Microsoft\StorSimple\BACatalog. 

     >[AZURE.NOTE] ProgramData is een verborgen map.

3. De catalogus XML-bestand verwijderen en vervangen door de versie die u eerder hebt opgeslagen.

4. De Microsoft StorSimple Management-Service opnieuw: 

    1. Selecteer op het Dashboard Serverbeheer, klikt u op het menu **Extra** **Services**.

    2. Selecteer op de pagina **Services** **Microsoft StorSimple Management-Service**.

    3. Klik in het rechterdeelvenster onder **StorSimple Management-Service van Microsoft**, op **de service opnieuw starten**.

## <a name="next-steps"></a>Volgende stappen

- Meer informatie over StorSimple momentopname Manager, gaat u naar [Wat is StorSimple momentopname Manager?](storsimple-what-is-snapshot-manager.md).

- Meer informatie over de gebruikersinterface StorSimple momentopname Manager, gaat u naar [de gebruikersinterface StorSimple momentopname Manager](storsimple-use-snapshot-manager.md).

- Meer informatie over het gebruik van StorSimple momentopname Manager, gaat u naar [Gebruik StorSimple momentopname Manager kunt u uw oplossing StorSimple beheren](storsimple-snapshot-manager-admin.md).
