<properties 
   pageTitle="CHAP configureren voor uw apparaat StorSimple | Microsoft Azure"
   description="Wordt beschreven hoe de vraag CHAP Handshake Authentication Protocol () op een apparaat StorSimple configureren."
   services="storsimple"
   documentationCenter=""
   authors="alkohli"
   manager="carmonm"
   editor="" />
<tags 
   ms.service="storsimple"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="TBD"
   ms.date="08/17/2016"
   ms.author="alkohli" />

# <a name="configure-chap-for-your-storsimple-device"></a>CHAP configureren voor uw apparaat StorSimple

Deze zelfstudie wordt uitgelegd hoe CHAP configureren voor uw apparaat StorSimple. De procedure gedetailleerde in dit artikel is van toepassing op StorSimple 8000 reeks, evenals StorSimple 1200 apparaten.

CHAP staat voor uitdaging handshakeverificatieprotocol. Het is gebruikte door servers voor het valideren van de identiteit van externe clients verificatiemethode. De verificatie is gebaseerd op een gedeelde wachtwoord of geheim. CHAP kan zijn in één richting (één richting) of onderlinge (bidirectionele). Eén CHAP is wanneer de doellijst wordt een initiator geverifieerd. Onderlinge of omgekeerde CHAP, moet aan de andere kant, het doel verifiëren het beginpunt en vervolgens het beginpunt het doel te verifiëren. Initiator verificatie kan worden uitgevoerd zonder doel-verificatie. Target verificatie kan echter worden geïmplementeerd alleen als initiator verificatie ook wordt geïmplementeerd. 

Beste, wordt u aangeraden dat u CHAP gebruiken om iSCSI-beveiliging te verbeteren.

>[AZURE.NOTE] Houd er rekening mee dat IPSEC wordt momenteel niet ondersteund op StorSimple apparaten.

De CHAP-instellingen op het apparaat StorSimple kunnen worden geconfigureerd in de volgende manieren:

- Één- of één richting verificatie

- Bidirectionele of onderlinge of reverse-verificatie

In elk van deze gevallen moet de portal voor het apparaat en de server iSCSI-initiatorsoftware worden geconfigureerd. De gedetailleerde stappen voor deze configuratie worden beschreven in de volgende zelfstudie.

## <a name="unidirectional-or-one-way-authentication"></a>Één- of één richting verificatie

In één richting authenticatie, wordt het doel geverifieerd het beginpunt. Deze verificatie is vereist dat u de instellingen van de initiator CHAP op het apparaat StorSimple en de iSCSI-initiatorsoftware op de host configureren. De gedetailleerde procedures voor het StorSimple apparaat en de Windows-host worden nu beschreven.

#### <a name="to-configure-your-device-for-one-way-authentication"></a>Voor het configureren van uw apparaat gebruiken voor één richting verificatie

1. Klik op het tabblad **configureren** in de klassieke Azure portal, klik op de pagina **apparaten** .

    ![CHAP Initiator](./media/storsimple-configure-chap/IC740943.png)

2. Schuif omlaag op deze pagina en klik in de sectie **CHAP Initiator** :
                                                    
    1. Geef de gebruikersnaam van een voor uw CHAP initiator.

    2. Een wachtwoord voor uw CHAP initiator opgeven.

         > [AZURE.IMPORTANT] De naam van de gebruiker CHAP moet minder dan 233 tekens bevatten. Het wachtwoord CHAP moet liggen tussen 12 en 16 tekens bevatten. Een langere gebruikersnaam of wachtwoord resulteert in een verificatie mislukt op de Windows-host.
    
    3. Bevestig het wachtwoord.

4. Klik op **Opslaan**. Een bevestigingsbericht wordt weergegeven. Klik op **OK** als de wijzigingen wilt opslaan.

#### <a name="to-configure-one-way-authentication-on-the-windows-host-server"></a>Eén verificatie configureren op de host van Windows server

1. Op de server van de Windows-host, start u de iSCSI-Initiator.

2. Klik in het venster **iSCSI Initiator eigenschappen** de volgende stappen uitvoeren:
                                                    
    1. Klik op het tabblad **detectie** .

        ![iSCSI initiator eigenschappen](./media/storsimple-configure-chap/IC740944.png)

    2. Klik op **ontdekken Portal**.

3. Klik in het dialoogvenster **Kennismaken met doel-Portal** :
                                                    
    1. Geef het IP-adres van uw apparaat.

    3. Klik op **Geavanceerd**.

        ![Kennismaken met doelportal](./media/storsimple-configure-chap/IC740945.png)

4. Klik in het dialoogvenster **Advanced Settings** :
                                                    
    1. Schakel het selectievakje **inschakelen CHAP aanmelden** .

    2. Geef de gebruikersnaam die u hebt opgegeven voor het beginpunt CHAP in de klassieke portal in het veld **naam** .

    3. In het veld **geheim van het doel** , kunt u het wachtwoord die u hebt opgegeven voor het beginpunt CHAP in de klassieke portal opgeven.

    4. Klik op **OK**.

        ![Geavanceerde instellingen algemeen](./media/storsimple-configure-chap/IC740946.png)

5. Klik op het tabblad **doelen** van het venster **iSCSI Initiator eigenschappen** de apparaatstatus worden weergegeven als **verbonden**. Als u een apparaat StorSimple 1200 gebruikt, klikt u vervolgens elke volume wordt worden gekoppeld als een iSCSI-doel zoals hieronder wordt weergegeven. Daarom moet stap 3 en 4 voor elk volume worden herhaald.

    ![Volumes gekoppeld als afzonderlijke doelen](./media/storsimple-configure-chap/chap4.png)

    > [AZURE.IMPORTANT] Als u de naam van de iSCSI wijzigt, wordt de nieuwe naam voor de nieuwe iSCSI-sessies worden gebruikt. Nieuwe instellingen niet worden gebruikt voor bestaande sessies totdat u zich afmeldt en aanmelden opnieuw.

Ga naar [aanvullende overwegingen](#additional-considerations)voor meer informatie over het configureren van CHAP op de Windows-host-server.


## <a name="bidirectional-or-mutual-authentication"></a>Bidirectionele of onderlinge verificatie

Klik in bidirectionele verificatie, wordt het doel geverifieerd het beginpunt en klikt u vervolgens het beginpunt wordt geverifieerd door het doel. U moet hiervoor de gebruiker de instellingen van de initiator CHAP, evenals de omgekeerde CHAP-instellingen configureren op het apparaat en iSCSI-initiatorsoftware op de host. De volgende procedures beschrijven de stappen voor het onderlinge verificatie configureren op het apparaat en klik op de Windows-host.

#### <a name="to-configure-your-device-for-mutual-authentication"></a>Voor het configureren van uw apparaat voor onderlinge verificatie

1. Klik op het tabblad **configureren** in de klassieke Azure portal, klik op de pagina **apparaten** .

    ![CHAP doel](./media/storsimple-configure-chap/IC740948.png)

2. Schuif omlaag op deze pagina en klik in de sectie **CHAP Target** :
                                                    
    1. Geef een **omgekeerde CHAP gebruikersnaam in te voeren** voor uw apparaat.

    2. Een **omgekeerde CHAP wachtwoord** voor uw apparaat opgeven.

    3. Bevestig het wachtwoord.

3. In de sectie **CHAP Initiator** :
                                                
    1. Geef een **gebruikersnaam in te voeren** voor uw apparaat.

    1. Een **wachtwoord** opgeven voor uw apparaat.

    3. Bevestig het wachtwoord.

4. Klik op **Opslaan**. Een bevestigingsbericht wordt weergegeven. Klik op **OK** als de wijzigingen wilt opslaan.

#### <a name="to-configure-bidirectional-authentication-on-the-windows-host-server"></a>Bidirectionele verificatie configureren op de host van Windows server

1. Op de server van de Windows-host, start u de iSCSI-Initiator.

2. Klik in het venster **iSCSI Initiator eigenschappen** klikt u op het tabblad **configuratie** .

3. Klik op **CHAP**.

4. Klik in het dialoogvenster **iSCSI onderlinge CHAP initiatorgeheim** :
                                                    
    1. Typ het **Omgekeerde CHAP wachtwoord** dat u in de portal van Azure klassieke hebt geconfigureerd.

    2. Klik op **OK**.

        ![iSCSI onderlinge CHAP initiatorgeheim](./media/storsimple-configure-chap/IC740949.png)

5. Klik op het tabblad **doelen** .

6. Klik op de knop **verbinding maken** . 

7. Klik in het dialoogvenster **Verbinding naar doel** op **Geavanceerd**.

8. Klik in het dialoogvenster **Geavanceerde eigenschappen** :
                                                    
    1. Schakel het selectievakje **inschakelen CHAP aanmelden** .

    2. Geef de gebruikersnaam die u hebt opgegeven voor het beginpunt CHAP in de klassieke portal in het veld **naam** .

    3. In het veld **geheim van het doel** , kunt u het wachtwoord die u hebt opgegeven voor het beginpunt CHAP in de klassieke portal opgeven.

    4. Schakel het selectievakje **onderlinge verificatie uitvoeren** .

        ![Geavanceerde instellingen onderlinge verificatie](./media/storsimple-configure-chap/IC740950.png)

    5. Klik op **OK** om de configuratie CHAP te voltooien
     
Ga naar [aanvullende overwegingen](#additional-considerations)voor meer informatie over het configureren van CHAP op de Windows-host-server.

## <a name="additional-considerations"></a>Aanvullende overwegingen

De functie **Snel verbinding maken** biedt geen ondersteuning voor verbindingen die CHAP ingeschakeld bevatten. Controleer of de knop **Connect** is beschikbaar op het tabblad **doelen** verbinding maken met een doel te gebruiken bij CHAP is ingeschakeld.

![Verbinding maken met doel](./media/storsimple-configure-chap/IC740947.png)

Selecteer het selectievakje **toevoegen aan de lijst met favoriete doelen** in het dialoogvenster **verbinding maken met doel** die worden weergegeven. Dit zorgt ervoor dat telkens wanneer de computer opnieuw is opgestart, wordt geprobeerd de verbinding met de iSCSI favoriete doelen herstellen.

## <a name="errors-during-configuration"></a>Fouten tijdens het configureren van

Als uw configuratie CHAP onjuist is, klikt u vervolgens bent u waarschijnlijk een foutbericht **verificatie mislukt** .

## <a name="verification-of-chap-configuration"></a>Verificatie van CHAP configuratie

U kunt controleren dat CHAP wordt gebruikt door de volgende stappen uit.

#### <a name="to-verify-your-chap-configuration"></a>Voor de verificatie van uw configuratie CHAP

1. Klik op **favoriete doelen**.

2. Selecteer het doel waarvoor u verificatie hebt ingeschakeld.

3. Klik op **Details**.

    ![iSCSI-initiator eigenschappen favoriete doelen](./media/storsimple-configure-chap/IC740951.png)

4. Noteer de vermelding in het veld **verificatie** in het dialoogvenster **Details van favoriete doel** . Als de configuratie geslaagd is, moet deze zeggen **CHAP**.

    ![Favoriet doel-details](./media/storsimple-configure-chap/IC740952.png)

## <a name="next-steps"></a>Volgende stappen

- Meer informatie over [StorSimple beveiliging](storsimple-security.md).

- Meer informatie over het [gebruik van de StorSimple Manager-service voor het beheren van uw apparaat StorSimple](storsimple-manager-service-administration.md).
