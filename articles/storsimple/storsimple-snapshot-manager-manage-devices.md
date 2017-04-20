<properties 
   pageTitle="Apparaten met StorSimple momentopname Manager beheren | Microsoft Azure"
   description="Wordt beschreven hoe u met de module StorSimple momentopname Manager MMC kunt verbinding maken en StorSimple apparaten beheren."
   services="storsimple"
   documentationCenter=""
   authors="SharS"
   manager="carmonm"
   editor="" />
<tags 
   ms.service="storsimple"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="04/18/2016"
   ms.author="v-sharos" />

# <a name="use-storsimple-snapshot-manager-to-connect-and-manage-storsimple-devices"></a>StorSimple momentopname Manager gebruiken om verbinding te en StorSimple apparaten beheren

## <a name="overview"></a>Overzicht

U kunt knooppunten in het deelvenster StorSimple momentopname Manager **bereik** geïmporteerde StorSimple apparaatgegevens controleren en vernieuw verbonden opslagapparaten. Als u het knooppunt **apparaten** klikt, kunt u bovendien een lijst met verbonden apparaten en de bijbehorende statusinformatie in het deelvenster **resultaten** zien.

![Verbonden apparaten](./media/storsimple-snapshot-manager-manage-devices/HCS_SSM_connect_devices.png)

**Afbeelding 1: StorSimple momentopname Manager verbonden apparaat** 

Afhankelijk van uw selecties **weergave** ziet u **het deelvenster met** de volgende informatie over elk apparaat. (Klik voor meer informatie over het configureren van een weergave, Ga naar het [menu Beeld](storsimple-use-snapshot-manager.md#view-menu).


| Resultatenkolom  |Beschrijving          |
|:----------------|:--------------------| 
| Naam            | De naam van het apparaat zoals geconfigureerd in de klassieke Azure-portal|
| Model           | Het getal model van het apparaat|
| Versie         | De versie van de software op het apparaat is geïnstalleerd |
| Status          | Of het apparaat dat beschikbaar is |
| Het laatst is gesynchroniseerd     | Datum en tijd waarop het apparaat dat het laatst is gesynchroniseerd |
| Serienummer      | Het seriële getal voor het apparaat |
 
Als u met de rechtermuisknop op het knooppunt **apparaten** in het deelvenster **bereik** , kunt u kiezen uit de volgende bewerkingen uit:

- Toevoegen of vervangen van een apparaat 
- Verbinding maken met een apparaat en controleer of de invoer 
- Verbonden apparaten vernieuwen 

Als u het knooppunt **apparaten** en klik vervolgens met de rechtermuisknop op de apparaatnaam van een in het deelvenster **resultaten** , kunt u kiezen uit de volgende bewerkingen uit:

- Een apparaat verifiëren 
- Details weergeven-apparaat 
- Een apparaat vernieuwen 
- De apparaatconfiguratie van een verwijderen 
- Het wachtwoord van een apparaat wijzigen

>[AZURE.NOTE] Al deze acties zijn ook beschikbaar in het deelvenster **Acties** .
 
Deze zelfstudie wordt uitgelegd hoe u StorSimple momentopname Manager verbinding maken en beheren van apparaten en de volgende taken uitvoeren:

- Toevoegen of vervangen van een apparaat 
- Verbinding maken met een apparaat en controleer of de invoer 
- Verbonden apparaten vernieuwen 
- Een apparaat verifiëren 
- Details weergeven-apparaat 
- Vernieuwen van een afzonderlijke-apparaat 
- De apparaatconfiguratie van een verwijderen 
- Een verlopen apparaatwachtwoord wijzigen
- Een apparaat vervangen

>[AZURE.NOTE] Ga naar [StorSimple momentopname Manager-gebruikersinterface](storsimple-use-snapshot-manager.md)voor algemene informatie over het gebruik van de interface StorSimple momentopname beheer.


## <a name="add-or-replace-a-device"></a>Toevoegen of vervangen van een apparaat

Gebruik de volgende procedure toevoegen of vervangen van een StorSimple-apparaat.

#### <a name="to-add-or-replace-a-device"></a>Toevoegen of vervangen van een apparaat

1. Klik op het bureaublad pictogram om StorSimple momentopname Manager te starten.

2. Klik in het deelvenster **bereik** met de rechtermuisknop op het knooppunt **apparaten** en klik vervolgens op **een apparaat configureren**. Het dialoogvenster **configureren een apparaat** wordt weergegeven.

    ![Een apparaat StorSimple configureren](./media/storsimple-snapshot-manager-manage-devices/HCS_SSM_config_device.png) 

3. Selecteer het IP-adres van het apparaat of een virtueel apparaat in de vervolgkeuzelijst **apparaat** . 

4. Typ in het tekstvak **wachtwoord** het wachtwoord StorSimple momentopname Manager die u hebt gemaakt voor het apparaat in de portal van Azure klassieke. Klik op **OK**. StorSimple momentopname Manager zoekt u naar het apparaat dat u identificeren. 

    - Als het apparaat dat beschikbaar is, voegt StorSimple momentopname Manager een verbinding. 

    - Als het apparaat om een reden niet beschikbaar is, retourneert StorSimple momentopname Manager een foutbericht wordt weergegeven. Klik op **OK** om het foutbericht te sluiten en klik vervolgens op **Annuleren** om het dialoogvenster **een apparaat configureren** te sluiten.

## <a name="connect-a-device-and-verify-imports"></a>Verbinding maken met een apparaat en controleer of de invoer

Gebruik de volgende procedure om te koppelen van een apparaat StorSimple en verifieer dat bestaande volume groepen die back-ups hebt verbonden zijn geïmporteerd.

#### <a name="to-connect-a-device-and-verify-imports"></a>Verbinding maken met een apparaat en controleer of de invoer

1. Verbinding maken met een apparaat naar StorSimple momentopname Manager, volgt u de instructies in het vak toevoegen of vervangen van een apparaat. Wanneer deze verbinding met een apparaat maakt, reageert StorSimple momentopname Manager als volgt:

    - Als het apparaat om een reden niet beschikbaar is, retourneert StorSimple momentopname Manager een foutbericht wordt weergegeven. 

   - Als het apparaat dat beschikbaar is, voegt StorSimple momentopname Manager een verbinding. Wanneer u het apparaat selecteren, deze wordt weergegeven in het deelvenster **resultaten** en het statusveld wordt aangegeven dat is het apparaat **beschikbaar**. StorSimple momentopname Manager importeert de volume-groepen die is geconfigureerd voor het apparaat, mits de volume-groepen back-ups hebt gekoppeld. Back-beleid niet geïmporteerd. Volume-groepen waarvoor geen bijbehorende back-ups niet geïmporteerd.

2. Klik op het bureaublad pictogram om StorSimple momentopname Manager te starten.

3. Met de rechtermuisknop op het bovenste knooppunt in het deelvenster **bereik** en klik vervolgens op **De wisselknop invoer weergeven**.

    ![Selecteer de wisselknop invoer weergeven](./media/storsimple-snapshot-manager-manage-devices/HCS_SSM_Toggle_Imports_Display.png) 

4. Het dialoogvenster **In-/ uitschakelen invoer beeldscherm** wordt weergegeven, met de status van de geïmporteerde volume-groepen en back-ups. Klik op **OK**. 

Nadat de volume-groepen en back-ups geïmporteerd zijn, kunt u StorSimple momentopname Manager te beheren, net zoals u zou volume-groepen en back-ups die u hebt gemaakt en geconfigureerd met StorSimple momentopname Manager beheren. 

## <a name="refresh-connected-devices"></a>Verbonden apparaten vernieuwen

Gebruik de volgende procedure voor het synchroniseren van de verbonden StorSimple-apparaten met StorSimple momentopname Manager.

####<a name="to-refresh-connected-devices"></a>Voor het vernieuwen verbonden apparaten

1. Klik op het bureaublad pictogram om StorSimple momentopname Manager te starten.

2. Klik in het deelvenster **bereik** met de rechtermuisknop op **apparaten**en klik vervolgens op **Apparaten vernieuwen**. Deze optie wordt gesynchroniseerd de verbonden apparaten met StorSimple momentopname Manager zodat u kunt het volume-groepen en back-ups, inclusief eventuele recente toevoegingen weergeven. 

    ![De apparaten StorSimple vernieuwen](./media/storsimple-snapshot-manager-manage-devices/HCS_SSM_Refresh_devices.png)
 
De actie **Vernieuwen apparaten** worden eventuele nieuwe volume-groepen en alle bijbehorende back-ups opgehaald uit verbonden apparaten. In tegenstelling tot de **volumes scannen** actie beschikbaar zijn voor het knooppunt **Volumes** herstelt **Apparaten vernieuwen** geen het back-register.

## <a name="authenticate-a-device"></a>Een apparaat verifiëren

Gebruik de volgende procedure een StorSimple-apparaat met StorSimple momentopname Manager te verifiëren.

#### <a name="to-authenticate-a-device"></a>Een apparaat te verifiëren

1. Klik op het bureaublad pictogram om StorSimple momentopname Manager te starten.

2. Klik in het deelvenster **bereik** op **apparaten**.

3. Klik in het deelvenster **resultaten** met de rechtermuisknop op de naam van het apparaat en klik vervolgens op **verifiëren**.

4. Het dialoogvenster **verifiëren** wordt weergegeven. Typ het apparaatwachtwoord en klik vervolgens op **OK**.

    ![Het dialoogvenster verifiëren](./media/storsimple-snapshot-manager-manage-devices/HCS_SSM_Authenticate.png) 
 
## <a name="view-device-details"></a>Details weergeven-apparaat

Gebruik de volgende procedure om te bekijken van de details van een apparaat StorSimple en, indien nodig het apparaat met StorSimple momentopname Manager synchroniseren.

#### <a name="to-view-and-resynchronize-device-details"></a>Om te bekijken en Apparaatdetails van het opnieuw te synchroniseren

1. Klik op het bureaublad pictogram om StorSimple momentopname Manager te starten.

2. Klik in het deelvenster **bereik** op **apparaten**.

3. Klik in het deelvenster **resultaten** met de rechtermuisknop op de naam van het apparaat en klik vervolgens op **Details**. 

4. het dialoogvenster **Details van apparaat** wordt weergegeven. In dit vak ziet u de naam, model, versie, geeft het seriële getal, status en doel iSCSI gekwalificeerde naam (IQN), en laatste synchronisatiedatum en tijd. 

   - Klik op **synchroniseren** als u wilt synchroniseren van het apparaat.

   - Klik op **OK** of op **Annuleren** om het dialoogvenster te sluiten.

    ![Apparaatdetails](./media/storsimple-snapshot-manager-manage-devices/HCS_SSM_Device_details.png) 
 
## <a name="refresh-an-individual-device"></a>Vernieuwen van een afzonderlijke-apparaat

Gebruik de volgende procedure een afzonderlijke StorSimple-apparaat met StorSimple momentopname Manager opnieuw te synchroniseren.

#### <a name="to-refresh-a-device"></a>Een apparaat vernieuwen

1. Klik op het bureaublad pictogram om StorSimple momentopname Manager te starten. 

2. Klik in het deelvenster **bereik** op **apparaten**. 

3. Klik in het deelvenster **resultaten** met de rechtermuisknop op de naam van het apparaat en klik vervolgens op **Vernieuwen apparaat**. Deze optie wordt gesynchroniseerd het apparaat met StorSimple momentopname Manager. 

## <a name="delete-a-device-configuration"></a>De apparaatconfiguratie van een verwijderen

Gebruik de volgende procedure om te verwijderen van een afzonderlijke StorSimple apparaatconfiguratie vanuit StorSimple momentopname Manager.

#### <a name="to-delete-a-device-configuration"></a>Configuratie van een verwijderen

1. Klik op het bureaublad pictogram om StorSimple momentopname Manager te starten. 

2. Klik in het deelvenster **bereik** op **apparaten**. 

3. Klik in het deelvenster **resultaten** met de rechtermuisknop op de naam van het apparaat en klik vervolgens op **verwijderen**. 

4. Het volgende bericht wordt weergegeven. Klik op **Ja** om te verwijderen van de configuratie of klikt u op **Nee** om de verwijdering te annuleren.

    ![Apparaatconfiguratie verwijderen](./media/storsimple-snapshot-manager-manage-devices/HCS_SSM_DeleteDevice.png)

## <a name="change-an-expired-device-password"></a>Een verlopen apparaatwachtwoord wijzigen

U kunt een wachtwoord om te verifiëren van een StorSimple-apparaat met StorSimple momentopname Manager moet invoeren. U kunt dit wachtwoord instellen wanneer u de Windows PowerShell-interface gebruiken voor het instellen van het apparaat. Het wachtwoord kunt echter zijn verlopen. Als dit gebeurt, kunt u de Azure klassieke portal het wachtwoord wijzigen. Vervolgens gaat u omdat het apparaat is geconfigureerd in StorSimple momentopname-beheer voordat het wachtwoord is verlopen, u het apparaat in StorSimple momentopname Manager opnieuw verifiëren moet. 

#### <a name="to-change-the-expired-password"></a>Het verlopen wachtwoord wijzigen

1. Klik in de klassieke Azure portal door de service StorSimple Manager te starten.

2. Klik op **apparaten** > **configureren** voor het apparaat.

3. Schuif omlaag naar de sectie StorSimple momentopname Manager. Typ het wachtwoord dat is 14-15 tekens. Zorg ervoor dat het wachtwoord een combinatie van hoofdletters, kleine letters, cijfers en speciale tekens bevat.

4. Voer het wachtwoord te bevestigen.

5. Klik op **Opslaan** onder aan de pagina.

#### <a name="to-re-authenticate-the-device"></a>Het apparaat opnieuw worden geverifieerd

1. StorSimple momentopname Manager starten.

2. Klik in het deelvenster **bereik** op **apparaten**. Er verschijnt een lijst met geconfigureerde apparaten in het deelvenster **resultaten** . 

3. Selecteer het apparaat, klik met de rechtermuisknop en klik vervolgens op **verifiëren**.

4. Typ het nieuwe wachtwoord in het venster **verifiëren** . 

5. Selecteer het apparaat, klik met de rechtermuisknop en selecteer **vernieuwen apparaat**. Deze optie wordt gesynchroniseerd het apparaat met StorSimple momentopname Manager. 

## <a name="replace-a-failed-device"></a>Een apparaat vervangen

Als een apparaat StorSimple mislukt en is vervangen door een apparaat stand-by (uitvalbeveiliging), gebruikt u de volgende stappen verbinding maken met het nieuwe apparaat en de bijbehorende back-ups weergeven.

#### <a name="to-connect-to-a-new-device-after-failover"></a>Verbinding maken met een nieuwe apparaat na failover

1. De iSCSI-verbinding met het nieuwe apparaat configureren. Voor instructies, gaat u naar ' stap 7: Mountains, initialisatie en opmaak een volume "in [Deploy uw on-premises implementatie StorSimple-apparaat](storsimple-deployment-walkthrough-u2.md). 

>[AZURE.NOTE] Als het nieuwe StorSimple apparaat hetzelfde IP-adres als de oude heeft, is het mogelijk dat u kunt verbinding maken met de oude configuratie. 

2. De Microsoft StorSimple Management-Service stoppen:

    1. Start Server Manager.

    2. Selecteer op het Dashboard Serverbeheer, klikt u op het menu **Extra** **Services**. 

    3. Selecteer de **Microsoft StorSimple Management-Service**in het venster **Services** . 

    4. Klik in het rechterdeelvenster onder **StorSimple Management-Service van Microsoft**, op **de service stoppen**. 

3. De configuratie-informatie die betrekking hebben op het oude apparaat verwijderen: 

    1. Blader in Bestandenverkenner naar C:\ProgramData\Microsoft\StorSimple\BACatalog. 

    2. Verwijder de bestanden in de map BACatalog. 

4. De Microsoft StorSimple Management-Service opnieuw: 

    1. Selecteer op het Dashboard Serverbeheer, klikt u op het menu **Extra** **Services**. 

    2. Selecteer de **Microsoft StorSimple Management-Service**in het venster **Services** . 

    3. Klik in het rechterdeelvenster onder **StorSimple Management-Service van Microsoft**, op **de service opnieuw starten**. 

5. StorSimple momentopname Manager starten. 

6. Als u wilt configureren de nieuwe StorSimple-apparaat, voert u de stappen in stap 2: verbinding maken met een apparaat StorSimple in [Implementeren StorSimple momentopname Manager](storsimple-snapshot-manager-deployment.md). 

7. Met de rechtermuisknop op het bovenste knooppunt in het deelvenster **bereik** (StorSimple momentopname Manager in het voorbeeld) en klik vervolgens op **De wisselknop invoer weergeven**. 

8. Een bericht wordt weergegeven wanneer de geïmporteerde volume-groepen en back-ups zichtbaar zijn in StorSimple momentopname Manager. Klik op **OK**. 

## <a name="next-steps"></a>Volgende stappen

- Informatie over het [gebruik van StorSimple momentopname Manager kunt u uw oplossing StorSimple beheren](storsimple-snapshot-manager-admin.md).
- Informatie over het [gebruik van StorSimple momentopname Manager weergeven en beheren van volumes](storsimple-snapshot-manager-manage-volumes.md).

