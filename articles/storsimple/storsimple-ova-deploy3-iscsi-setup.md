<properties 
   pageTitle="Installatie van de server StorSimple virtuele matrix iSCSI | Microsoft Azure"
   description="Beschreven hoe u de eerste configuratie uitvoeren, uw StorSimple iSCSI-server registreren en Apparaatinstelling voltooien."
   services="storsimple"
   documentationCenter="NA"
   authors="alkohli"
   manager="carmonm"
   editor="" />
<tags 
   ms.service="storsimple"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="TBD"
   ms.date="07/18/2016"
   ms.author="alkohli" />


# <a name="deploy-storsimple-virtual-array--set-up-your-virtual-device-as-an-iscsi-server"></a>Implementeren StorSimple virtuele matrix – uw virtuele apparaat als een server iSCSI instellen

![Processtroom iSCSI](./media/storsimple-ova-deploy3-iscsi-setup/iscsi4.png)

## <a name="overview"></a>Overzicht

Deze zelfstudie implementatie is van toepassing op de Microsoft Azure StorSimple virtuele matrix (ook wel bekend als het StorSimple on-premises implementatie virtuele apparaat of het virtuele apparaat StorSimple) actieve maart 2016 algemene beschikbaarheid (GA) versie. Deze zelfstudie beschreven hoe u de eerste configuratie uitvoeren, uw StorSimple iSCSI-server registreren, voltooit de configuratie apparaat en maak vervolgens, koppelen, geïnitialiseerd en formatteren op uw StorSimple virtueel apparaat iSCSI-server. De gegevens van de instelling StorSimple in dit artikel is alleen van toepassing op StorSimple virtuele matrices. 

De procedures duurt ongeveer 30 minuten hier op 1 uur staan om te voltooien. De gegevens die zijn gepubliceerd in dit artikel is alleen van toepassing op StorSimple virtuele matrices.

## <a name="setup-prerequisites"></a>Vereisten instellen

Controleer het volgende voordat u configureert en van uw virtuele StorSimple-apparaat instellen:

- U hebt deze is ingericht een virtueel apparaat en verbonden met deze zoals is beschreven in [Implementeren StorSimple virtuele matrix - bepaling een virtuele matrix in Hyper-V](storsimple-ova-deploy2-provision-hyperv.md) of [Implementeren StorSimple virtuele matrix - bepaling een virtuele matrix in VMware](storsimple-ova-deploy2-provision-vmware.md).

- U hebt de sleutel voor de registratie van de StorSimple Manager-service die u hebt gemaakt als StorSimple virtuele apparaten wilt beheren. Zie voor meer informatie **stap 2: de sleutel voor de registratie ophalen** in [Implementeren StorSimple virtuele matrix - voorbereiden van de portal](storsimple-ova-deploy1-portal-prep.md#step-2-get-the-service-registration-key).

- Als dit het tweede of volgende virtuele apparaat dat u in een bestaande StorSimple Manager-service registreren wilt, kunt u de service gegevenssleutel nodig hebt. Deze sleutel is gegenereerd wanneer het eerste apparaat is geregistreerd met deze service. Als u deze toets kwijt, raadpleegt u **de sleutel service gegevens ophalen** [De Web-gebruikersinterface voor het beheren van uw StorSimple virtuele matrix](storsimple-ova-web-ui-admin.md#get-the-service-data-encryption-key)wordt gebruikt.

## <a name="step-by-step-setup"></a>Stapsgewijze instellingen 

Gebruik de volgende stapsgewijze instructies instellen en configureren van uw StorSimple virtuele apparaat:

-  [Stap 1: Voltooit de configuratie van de gebruikersinterface lokale web en registreren van uw apparaat](#step-1-complete-the-local-web-ui-setup-and-register-your-device)
-  [Stap 2: De vereiste apparaatinstelling voltooien](#step-2-complete-the-required-device-setup)
-  [Stap 3: Een volume toevoegen](#step-3-add-a-volume)
-  [Stap 4: Koppelen, geïnitialiseerd en een volume opmaken](#step-4-mount-initialize-and-format-a-volume)  

## <a name="step-1-complete-the-local-web-ui-setup-and-register-your-device"></a>Stap 1: Voltooit de configuratie van de gebruikersinterface lokale web en registreren van uw apparaat 

#### <a name="to-complete-the-setup-and-register-the-device"></a>De installatie is voltooid en registreren van het apparaat

1. Open een browservenster en verbinding maken met het web UI door te typen:

    `https://<ip-address of network interface>`

    De URL van de verbinding in de vorige stap hebt genoteerd gebruiken. Hier ziet u een fout weergegeven met de mededeling dat u er een probleem met het beveiligingscertificaat van de website. Klik op **Doorgaan naar deze webpagina**.

    ![Fout in beveiligingscertificaat](./media/storsimple-ova-deploy3-iscsi-setup/image3.png)

2. Meld u aan bij de webversie van gebruikersinterface van uw virtueel apparaat als **StorSimpleAdmin**. Voer het wachtwoord van het apparaat-beheerder die u hebt gewijzigd in stap 3: het virtuele apparaat starten in [Implementeren StorSimple virtuele matrix - bepaling een virtueel apparaat in Hyper-V](storsimple-ova-deploy2-provision-hyperv.md) of [Implementeren StorSimple virtuele matrix - bepaling een virtueel apparaat in VMware](storsimple-ova-deploy2-provision-vmware.md).

    ![Aanmeldingspagina](./media/storsimple-ova-deploy3-iscsi-setup/image4.png)

3. U wordt naar **de startpagina van** worden geleid. De verschillende instellingen die nodig is om te configureren en het virtuele apparaat met de service StorSimple Manager registreren voor deze pagina wordt beschreven. Houd er rekening mee dat het **netwerk van instellingen**, **Web proxy-instellingen**en **tijdinstellingen** optioneel zijn. De instellingen voor alleen vereist zijn **Apparaatinstellingen** en **Cloud-instellingen**.

    ![Startpagina](./media/storsimple-ova-deploy3-iscsi-setup/image5.png)

4. Klik op de pagina **netwerkinstellingen** onder **Netwerkinterfaces**wordt gegevens 0 automatisch geconfigureerd voor u. Elke netwerkinterface is standaard ingesteld automatisch ophalen van een IP-adres (DHCP). Daarom wordt een IP-adres, subnet, en gateway automatisch toegewezen (voor IPv4 en IPv6).

    Als u van plan bent om te implementeren van uw apparaat als iSCSI-server (voor het inrichten van blokkeren opslag), aangeraden dat u de optie **ophalen IP-adres automatisch** uitschakelen en statische IP-adressen configureren.

    ![De pagina met netwerkinstellingen](./media/storsimple-ova-deploy3-iscsi-setup/image6.png)

    Als u meer dan één netwerkinterface tijdens de inrichting van het apparaat hebt toegevoegd, kunt u deze hier configureren. Opmerking dat u kunt de netwerkinterface van uw configureren als IPv4 alleen of als IPv4 en IPv6. IPv6-alleen configuraties worden niet ondersteund.

5. DNS-servers zijn vereist, omdat ze worden gebruikt als uw apparaat wordt geprobeerd om te communiceren met uw cloud storage-service-providers of voor het oplossen van uw apparaat met de naam als deze is geconfigureerd als een bestandsserver. Op de pagina **netwerkinstellingen** onder de **DNS-servers**:

    1. Een primaire en secundaire DNS-server wordt automatisch geconfigureerd. Als u kiest voor het configureren van statische IP-adressen, kunt u DNS-servers. Beschikbaarheid, is het raadzaam een primaire en een secundaire DNS-server te configureren.

    2. Klik op **toepassen**. Hiermee wordt toepassen en de netwerkinstellingen valideren.

6. Klik op de pagina **Instellingen** :

    1. Een unieke **naam** toewijzen aan uw apparaat. Deze naam 1-15 tekens en brief, cijfers en afbreekstreepjes kan bevatten.

    2. Klik op het pictogram **iSCSI server** ![iSCSI serverpictogram](./media/storsimple-ova-deploy3-iscsi-setup/image7.png) voor het **Type** apparaat dat u maakt. Een iSCSI-server kunt u blokkeren om opslagruimte te creëren.

    3. Opgeven of u wilt dat dit apparaat moeten domein behoren. Als uw apparaat een iSCSI-server is, is klikt u vervolgens deelnemen aan het domein optioneel. Als u ervoor kiest om uw iSCSI-server niet te koppelen aan een domein, klikt u op **toepassen**, wachten om door de instellingen om te worden toegepast en gaat u verder met de volgende stap.

        Als u deelnemen aan het apparaat naar een domein wilt. Voer een **domeinnaam**en klik vervolgens op **toepassen**.

        > [AZURE.NOTE] Als uw server iSCSI toevoegen aan een domein, zorg ervoor dat uw virtuele matrix bevindt zich in een eigen organisatie-eenheid voor Microsoft Azure Active Directory en geen GPO (GPO) zijn toegepast.

    5. Een dialoogvenster wordt weergegeven. Voer de domeinreferenties van uw in de opgegeven notatie. Klik op het pictogram controleren ![pictogram controleren](./media/storsimple-ova-deploy3-iscsi-setup/image15.png). De domeinreferenties moeten worden gecontroleerd. U ziet een foutbericht wordt weergegeven als de referenties onjuist zijn.

        ![referenties](./media/storsimple-ova-deploy3-iscsi-setup/image8.png)

    6. Klik op **toepassen**. Hiermee wordt toepassen en de apparaatinstellingen valideren.
 
7. (Optioneel) configureren uw web-proxyserver. Hoewel de configuratie van de web-proxy optioneel is, worden de dat als u een webproxy gebruikt, u alleen u deze hier configureert kunt.

    ![webproxy configureren](./media/storsimple-ova-deploy3-iscsi-setup/image9.png)

    Klik op de pagina **webproxy** :

    1. De **URL van de Web-proxy** in deze indeling: *http://host-IP adres* of *FDQN:Port nummer*. Houd er rekening mee dat HTTPS-URL's worden niet ondersteund.

    2. Geef **verificatie** als **eenvoudige** of **geen**.

    3. Als u verificatie gebruikt, moet u ook een **gebruikersnaam** en **wachtwoord**op te geven.

    4. Klik op **toepassen**. Zo valideren en past u de geconfigureerde web proxy-instellingen.
 
8. (Optioneel) de tijdinstellingen voor uw apparaat, zoals tijdzone en het primaire en secundaire NTP-servers configureren. NTP-servers zijn vereist omdat uw apparaat tijd synchroniseren moet, zodat deze met de cloud-serviceproviders verifiëren kan.

    ![Tijd instellen](./media/storsimple-ova-deploy3-iscsi-setup/image10.png)

    Klik op de pagina **Instellingen voor tijd** :

    1. Selecteer de **tijdzone** op basis van de geografische locatie waarin het apparaat wordt wordt geïmplementeerd in de vervolgkeuzelijst. De standaardtijdzone voor uw apparaat is PST-bestand. Uw apparaat, worden deze tijdzone gebruiken voor alle geplande bewerkingen.

    2. Een **primaire NTP-server** voor uw apparaat opgeven of accepteer de standaardwaarde van time.windows.com. Zorg ervoor dat uw netwerk NTP-verkeer is toegestaan voor het doorsturen van uw datacenter met Internet is toegestaan.

    3. Geef desgewenst een **secundaire NTP-server** voor uw apparaat.

    4. Klik op **toepassen**. Zo valideren en past u de geconfigureerde tijd-instellingen.

9. Configureer de cloud-instellingen voor uw apparaat. In deze stap geeft u Voltooi de configuratie lokaal apparaat en klikt u vervolgens het apparaat met uw StorSimple Manager-service registreren.

    1. Voer de **registersleutel voor de Service** die u hebt ontvangen in **stap 2: de sleutel voor de registratie ophalen** in [Implementeren StorSimple virtuele matrix - voorbereiden van de Portal](storsimple-ova-deploy1-portal-prep.md#step-2-get-the-service-registration-key).

    2. Als dit niet het eerste apparaat dat u met deze service wilt registreren, moet u de **Service gegevens versleutelingssleutel**opgeven. Deze sleutel is vereist voor de toets van de registratie service om extra apparaten met de service StorSimple Manager registreren. Voor meer informatie raadpleegt u [de sleutel service gegevens](storsimple-ova-web-ui-admin.md#get-the-service-data-encryption-key) ophalen op uw lokale web UI.

    3. Klik op **registreren**. Hiermee wordt het apparaat opnieuw starten. Mogelijk moet u wachten op een 2-3 minuten voordat het apparaat dat is geregistreerd. Nadat het apparaat opnieuw is opgestart, wordt u naar de pagina aanmelden worden geleid.

       ![Register-apparaat](./media/storsimple-ova-deploy3-iscsi-setup/image11.png)

10. Ga terug naar de portal van Azure klassieke. Klik op de pagina **apparaten** controleren dat het apparaat dat is verbonden met de service door te zoeken naar de status. Status van het apparaat moet **actief**zijn.

    ![Pagina apparaten](./media/storsimple-ova-deploy3-iscsi-setup/image12.png)

## <a name="step-2-complete-the-required-device-setup"></a>Stap 2: De vereiste apparaatinstelling voltooien

U voltooit de configuratie van het apparaat van uw apparaat StorSimple, moet u:

- Selecteer een account opslagruimte aan uw apparaat wilt koppelen.

- Kies versleutelingsinstellingen voor de gegevens die wordt verzonden naar cloud.

De volgende stappen uitvoeren in de klassieke Azure-portal om de configuratie vereist apparaat te voltooien.

#### <a name="to-complete-the-minimum-device-setup"></a>De minimale apparaat-installatie te voltooien

1. Selecteer het apparaat dat u zojuist hebt gemaakt op de pagina **apparaten** . Dit apparaat wilt weergegeven als **actief**. Klik op de pijl naast de apparaatnaam van het en klik vervolgens op **Snel starten**.

    ![Pagina apparaten](./media/storsimple-ova-deploy3-iscsi-setup/image13.png)

2. Klik op **volledige Apparaatinstelling** Start de wizard van de apparaat configureren.

    ![Wizard apparaat configureren](./media/storsimple-ova-deploy3-iscsi-setup/image14.png)

3. Ga als volgt te werk in de wizard van het apparaat configureren, klik op de pagina **Basisinstellingen** :

   1. Geef een opslag-account moet worden gebruikt met uw apparaat. In dit abonnement, kunt u een bestaand account voor de opslag van de vervolgkeuzelijst of kunt u **meer toevoegen** aan een account kiezen uit een ander abonnement.

   2. Definieer de versleutelingsinstellingen voor alle gegevens in rust dat wordt verzonden naar de cloud. (StorSimple wordt AES-256-versleuteling gebruikt.) Voor het coderen van uw gegevens, selecteert u het selectievakje **inschakelen cloud opslag-codering** . Voer een cloud opslag-versleuteling die 32 tekens bevat. Opnieuw op de toets om het te bevestigen.

   3. Klik op het pictogram controleren ![pictogram controleren](./media/storsimple-ova-deploy3-iscsi-setup/image15.png).

    ![Basisinstellingen](./media/storsimple-ova-deploy3-iscsi-setup/image16.png)

    De instellingen zijn nu bijgewerkt. Nadat de instellingen zijn bijgewerkt, is de knop van de setup voltooid apparaat niet beschikbaar. U gaat terug naar de pagina apparaat **Snel starten** .                                                        

>[AZURE.NOTE]U kunt alle andere apparaatinstellingen op elk gewenst moment wijzigen door de pagina **configureren** te openen.

## <a name="step-3-add-a-volume"></a>Stap 3: Een volume toevoegen

De volgende stappen uitvoeren in de portal van Azure klassieke een volume maken.

#### <a name="to-create-a-volume"></a>Een volume maken

1. Klik op **toevoegen een volume**op de pagina apparaat **Snel starten** . Hiermee start u de wizard volume toevoegen.

2. Ga als volgt te werk in de wizard een volume, klik onder **Basisinstellingen**toevoegen:

    1. Een unieke naam voor het volume van de opgeven. De naam moet een tekenreeks die 3 tot en met 127 tekens bevat.

    2. Geef een beschrijving voor het volume. De omschrijving zorgt ervoor dat de volume-eigenaars achterhaald.

    3. Selecteer een gebruikstype voor het volume. Het gebruikstype **Tiered volume** kan zijn of **lokaal vastgemaakt volume.** (**Tiered volume** is de standaardinstelling). Selecteer voor werkbelastingen waarvoor lokale garanties, lage vertragingstijden en betere prestaties, **lokaal vastgemaakt** **volume**. Voor alle andere gegevens, selecteert u **Tiered** **volume**.

        Het volume van een lokaal vastgemaakte dik is ingericht en zorgt ervoor dat de primaire gegevens in het volume blijft staan in het apparaat en Mors geen in de cloud. Als u een lokaal vastgemaakte volume maakt, wordt het apparaat controleren op beschikbare ruimte op de lokale niveaus voor het inrichten van een volume van de gewenste grootte. Maken van een lokaal vastgemaakte volume mogelijk nodig lopen van bestaande gegevens van het apparaat in de cloud en de tijd om te maken van het volume is mogelijk lang. De totale tijd, is afhankelijk van de grootte van het ingerichte volume, beschikbare netwerkbandbreedte en de gegevens op uw apparaat.

        Het volume van een doorverbonden echter dun is ingericht en zeer snel kan worden gemaakt. Wanneer u een doorverbonden volume maakt, ongeveer 10% van de ruimte op de lokale laag is ingericht en 90% van de ruimte in de cloud is ingericht. Bijvoorbeeld als u een volume 1 TB deze is ingericht, 100 GB zou bevinden zich in de lokale ruimte en 900 GB zou worden gebruikt in de cloud wanneer de lagen gegevens. Dit betekent beurtelings is dat als u afmelden bij de lokale ruimte op het apparaat uitvoeren niet kunt u een doorverbonden delen inrichten (omdat de 10% niet beschikbaar is).

    4. Geef de ingerichte capaciteit voor het volume van de. Houd er rekening mee dat de opgegeven capaciteit moet kleiner zijn dan de beschikbare capaciteit. Als u een doorverbonden volume maakt, moet de grootte tussen 500 GB en 5 TB. Geef voor een lokaal vastgemaakte volume, de grootte van een volume tussen 50 en 500 GB. De beschikbare capaciteit als een handleiding voor het inrichten van een volume gebruiken. Als de beschikbare lokale capaciteit 0 GB, wordt u is niet toegestaan voor het inrichten van een lokaal vastgemaakte of het volume van een doorverbonden.

        ![Basisinstellingen](./media/storsimple-ova-deploy3-iscsi-setup/image17.png)

    5. Klik op het pijlpictogram ![pijlpictogram](./media/storsimple-ova-deploy3-iscsi-setup/image18.png) om te gaan naar de volgende pagina.

3. Klik op de pagina **Aanvullende instellingen** moet u een nieuwe access besturingselement record (ACR) toevoegen:

    1. Geef een **naam** voor uw ACR.

    2. Klik onder **iSCSI initiatornaam**, bieden de iSCSI Qualified Name (IQN) van uw Windows-host. Als u niet de IQN hebt, gaat u naar de [bijlage A: Get de IQN van een Windows Server-host](#appendix-a-get-the-iqn-of-a-windows-server-host).

    3. Het is raadzaam een back-up van standaard door het selectievakje **een standaard back-up voor dit volume inschakelen** in te schakelen. De standaard back-up maakt een beleid dat wordt uitgevoerd om 22:30 elke dag (apparaat tijd) en Hiermee maakt u een momentopname cloud van dit volume.

        ![aanvullende instellingen](./media/storsimple-ova-deploy3-iscsi-setup/image19.png)

    4. Klik op het pictogram controleren ![pictogram controleren](./media/storsimple-ova-deploy3-iscsi-setup/image15.png). Hiermee start u de taak voor het maken van volume. Hier ziet u een voortgang van de volgende strekking weergegeven.

        ![van voortgangsbericht](./media/storsimple-ova-deploy3-iscsi-setup/image20.png)

        Een volume wordt gemaakt met de opgegeven instellingen. Standaard wordt cmdlets voor controle en back-up worden ingeschakeld voor het volume.

    5. Om te bevestigen dat het volume is gemaakt, gaat u naar de pagina **Volumes** . Hier ziet u het volume weergegeven.

        ![](./media/storsimple-ova-deploy3-iscsi-setup/image21.png)

## <a name="step-4-mount-initialize-and-format-a-volume"></a>Stap 4: Koppelen, geïnitialiseerd en een volume opmaken

De volgende stappen om te koppelen, geïnitialiseerd en StorSimple begint op een Windows Server-host uitvoeren.

#### <a name="to-mount-initialize-and-format-a-volume"></a>Als u wilt koppelen, geïnitialiseerd en een volume opmaken

1. Start het beginpunt van Microsoft iSCSI.

2. Klik in het venster **iSCSI Initiator eigenschappen** op het tabblad **Discovery** op **Ontdekken Portal**.

    ![kennismaken met portal](./media/storsimple-ova-deploy3-iscsi-setup/image22.png)

3. In het dialoogvenster **Kennismaken met doel-Portal** opgeven van het IP-adres van uw iSCSI ingeschakelde-netwerkinterface en klik vervolgens op **OK**.

    ![IP-adres](./media/storsimple-ova-deploy3-iscsi-setup/image23.png)

4. Klik in het venster **iSCSI Initiator eigenschappen** op het tabblad **doelen** , zoek de **Discovered doelen**. (Elk volume worden een ontdekt doel.) Status van het apparaat moet worden weergegeven als **inactief wordt gezet**.

    ![ontdekt doelen](./media/storsimple-ova-deploy3-iscsi-setup/image24.png)

5. Selecteer een apparaat en klik op **verbinding maken**. Nadat het apparaat is aangesloten, moet de status om **verbonden**te wijzigen. (Zie voor meer informatie over het gebruik van het beginpunt van Microsoft iSCSI [installeren en configureren van Microsoft iSCSI Initiator] [1].

    ![Selecteer doelapparaat](./media/storsimple-ova-deploy3-iscsi-setup/image25.png)

6. Op uw Windows-host, drukt u op de Windows-logotoets + X en klik vervolgens op **uitvoeren**.

7. Typ in het dialoogvenster **uitvoeren** **Diskmgmt.msc**. Klik op **OK**en het dialoogvenster **Schijfopruiming Management** wordt weergegeven. Het rechterdeelvenster, wordt de hoeveelheden weergegeven op de host.

8. Klik in het venster **Schijfopruiming Management** verschijnt de gekoppelde hoeveelheden zoals wordt weergegeven in de volgende afbeelding. Met de rechtermuisknop op het ontdekt volume (Klik op de schijfnaam van de), en klik vervolgens op **Online**.

    ![Schijfbeheer van de](./media/storsimple-ova-deploy3-iscsi-setup/image26.png)

9. Klik met de rechtermuisknop en selecteer **Geïnitialiseerd schijf**.

    ![geïnitialiseerd schijf 1](./media/storsimple-ova-deploy3-iscsi-setup/image27.png)

10. Selecteer de schijven geïnitialiseerd in het dialoogvenster en klik vervolgens op **OK**.

    ![geïnitialiseerd schijf 2](./media/storsimple-ova-deploy3-iscsi-setup/image28.png)

11. De wizard Nieuw eenvoudig Volume wordt gestart. Selecteer een schijfgrootte en klik vervolgens op **volgende**.

    ![wizard Nieuw volume 1](./media/storsimple-ova-deploy3-iscsi-setup/image29.png)

12. Een letter toewijzen aan het volume en klik vervolgens op **volgende**.

    ![wizard Nieuw volume 2](./media/storsimple-ova-deploy3-iscsi-setup/image30.png)

13. Voer de parameters als u wilt opmaken, het volume. **Op Windows Server, wordt alleen NTFS ondersteund.** Stel de Australië tot 64 kB. Geef een label voor het volume van de. Dit is een aanbevolen wordt aanbevolen om deze naam identiek zijn aan de naam van het volume die u op uw StorSimple virtuele apparaat hebt opgegeven. Klik op **volgende**.

    ![wizard Nieuw volume 3](./media/storsimple-ova-deploy3-iscsi-setup/image31.png)

14. Controleer de waarden voor het volume van de en klik vervolgens op **Voltooien**.

    ![wizard Nieuw volume 4](./media/storsimple-ova-deploy3-iscsi-setup/image32.png)

    De hoeveelheden wordt weergegeven als **Online** op de pagina **Schijf beheren** .

    ![online volumes](./media/storsimple-ova-deploy3-iscsi-setup/image33.png)

## <a name="next-steps"></a>Volgende stappen

Informatie over het gebruik van de lokale web gebruikersinterface voor het [beheren van uw StorSimple virtuele matrix](storsimple-ova-web-ui-admin.md).

## <a name="appendix-a-get-the-iqn-of-a-windows-server-host"></a>Bijlage A: krijgen de IQN van een Windows Server-host

Voer de volgende stappen uit als u de iSCSI Qualified Name (IQN) van een Windows-host met Windows Server 2012.

#### <a name="to-get-the-iqn-of-a-windows-host"></a>Om de IQN van een Windows-host

1. Start het beginpunt van Microsoft iSCSI op uw Windows-host.

2. Klik in het venster **iSCSI Initiator eigenschappen** op het tabblad **configuratie** , selecteer en kopieer de tekenreeks uit het veld **Initiatornaam** .

    ![iSCSI initiator eigenschappen](./media/storsimple-ova-deploy3-iscsi-setup/image34.png)

2. Sla deze tekenreeks.

<!--Reference link-->
[1]: https://technet.microsoft.com/library/ee338480(WS.10).aspx



