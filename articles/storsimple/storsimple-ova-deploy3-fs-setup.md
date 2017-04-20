<properties
   pageTitle="Implementeren StorSimple virtuele matrix 3: het virtuele apparaat als bestandsserver instellen"
   description="Deze derde zelfstudie in de virtuele matrix StorSimple implementatie Hiermee geeft u een virtueel apparaat als bestandsserver instellen."
   services="storsimple"
   documentationCenter="NA"
   authors="alkohli"
   manager="carmonm"
   editor=""/>

<tags
   ms.service="storsimple"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="05/26/2016"
   ms.author="alkohli"/>

# <a name="deploy-storsimple-virtual-array---set-up-as-file-server"></a>Implementeren StorSimple virtuele matrix - Set up als bestandsserver

![](./media/storsimple-ova-deploy3-fs-setup/fileserver4.png)

## <a name="introduction"></a>Inleiding 

In dit artikel is van toepassing op Microsoft Azure StorSimple virtuele matrix (ook wel bekend als de StorSimple on-premises implementatie virtueel apparaat of een virtueel apparaat StorSimple) actieve maart 2016 beschikbaarheid (GA) van de algemene release. In dit artikel wordt beschreven hoe eerste configuratie uitvoeren, uw bestandsserver StorSimple registreren, voltooit de configuratie apparaat en maken en verbinding maken met SMB aandelen. Dit is het laatste artikel in de reeks van implementatie zelfstudies vereist om uw virtuele matrix volledig te implementeren als een bestandsserver of een iSCSI-server.

De installatie en configuratie-proces kan ongeveer 10 minuten duren om te voltooien.


## <a name="setup-prerequisites"></a>Vereisten instellen

Controleer het volgende voordat u configureert en van uw virtuele StorSimple-apparaat instellen:

-   U hebt deze is ingericht een virtueel apparaat en verbonden met deze zoals wordt beschreven in de [voorziening een virtuele matrix StorSimple in Hyper-V](storsimple-ova-deploy2-provision-hyperv.md) of [een virtuele matrix StorSimple in VMware inrichten](storsimple-ova-deploy2-provision-vmware.md).

-   U hebt de sleutel voor de registratie van de StorSimple Manager-service die u hebt gemaakt als StorSimple virtuele apparaten wilt beheren. Zie voor meer informatie [stap 2: de sleutel voor de registratie ophalen](storsimple-ova-deploy1-portal-prep.md#step-2-get-the-service-registration-key) voor StorSimple virtuele matrix.

-   Als dit het tweede of volgende virtuele apparaat dat u in een bestaande StorSimple Manager-service registreren wilt, kunt u de service gegevenssleutel nodig hebt. Deze sleutel is gegenereerd wanneer het eerste apparaat is geregistreerd met deze service. Als u deze toets kwijt, raadpleegt u de [krijgen van de service gegevenssleutel](storsimple-ova-web-ui-admin.md#get-the-service-data-encryption-key) voor uw StorSimple virtuele matrix.

## <a name="step-by-step-setup"></a>Stapsgewijze instellingen

Gebruik de volgende stapsgewijze instructies instellen en configureren van uw virtuele StorSimple-apparaat.

## <a name="step-1-complete-the-local-web-ui-setup-and-register-your-device"></a>Stap 1: Voltooit de configuratie van de gebruikersinterface lokale web en registreren van uw apparaat 


#### <a name="to-complete-the-setup-and-register-the-device"></a>De installatie is voltooid en registreren van het apparaat

1.  Open een browservenster en verbinding maken met het lokale web UI. Type: 

    `https://<ip-address of network interface>`

    De URL van de verbinding in de vorige stap hebt genoteerd gebruiken. Hier ziet u een foutbericht dat aangeeft dat er een probleem met het beveiligingscertificaat van de website is. Klik op **Doorgaan naar deze webpagina**.

    ![](./media/storsimple-ova-deploy3-fs-setup/image2.png)

1.  Meld u aan bij de webversie van gebruikersinterface van uw virtueel apparaat als **StorSimpleAdmin**. Voer het wachtwoord van het apparaat-beheerder die u hebt gewijzigd in stap 3: het virtuele apparaat in [een virtuele matrix StorSimple in Hyper-V voorziening](storsimple-ova-deploy2-provision-hyperv.md) of in [een virtuele matrix StorSimple in VMware voorziening](storsimple-ova-deploy2-provision-vmware.md)starten.

    ![](./media/storsimple-ova-deploy3-fs-setup/image3.png)

1.  U wordt naar **de startpagina van** worden geleid. De verschillende instellingen die nodig is om te configureren en het virtuele apparaat met de service StorSimple Manager registreren voor deze pagina wordt beschreven. Houd er rekening mee dat het **netwerk van instellingen**, **Web proxy-instellingen**en **tijdinstellingen** optioneel zijn. De instellingen voor alleen vereist zijn **Apparaatinstellingen** en **Cloud-instellingen**.

    ![](./media/storsimple-ova-deploy3-fs-setup/image4.png)

1.  De pagina **netwerkinstellingen** onder **Netwerkinterfaces**wordt gegevens 0 automatisch geconfigureerd voor u. Elke netwerkinterface is standaard ingesteld automatisch ophalen van IP-adres (DHCP). Daarom wordt een IP-adres, subnet en gateway automatisch toegewezen (voor IPv4 en IPv6).

    ![](./media/storsimple-ova-deploy3-fs-setup/image5.png)

    Als u meer dan één netwerkinterface tijdens de inrichting van het apparaat hebt toegevoegd, kunt u deze hier configureren. Opmerking dat u kunt de netwerkinterface van uw configureren als IPv4 alleen of als IPv4 en IPv6. IPv6-alleen configuraties worden niet ondersteund.

1.  DNS-servers zijn vereist, omdat ze als uw apparaat wordt geprobeerd om te communiceren met uw cloud storage-service-providers of voor het oplossen van uw apparaat worden gebruikt door de naam wanneer geconfigureerd als een bestandsserver. Op de pagina **netwerkinstellingen** onder de **DNS-servers**:

    1.  Een primaire en secundaire DNS-server wordt automatisch geconfigureerd. Als u kiest voor het configureren van statische IP-adressen, kunt u DNS-servers. Beschikbaarheid, is het raadzaam een primaire en een secundaire DNS-server te configureren.

    2.  Klik op **toepassen**. Hiermee wordt toepassen en de netwerkinstellingen valideren.

2.  Op de pagina **Instellingen** :

    1.  Een unieke **naam** toewijzen aan uw apparaat. Deze naam 1-15 tekens en brief, cijfers en afbreekstreepjes kan bevatten.

    2.  Klik op het pictogram **bestandsserver** ![](./media/storsimple-ova-deploy3-fs-setup/image6.png) voor het **Type** apparaat dat u maakt. Een bestandsserver kunt u gedeelde mappen maken.

    3.  Als uw apparaat een bestandsserver is, moet u het apparaat aan een domein toevoegen. Geef een **domeinnaam**.

    1.  Klik op **toepassen**.

2.  Een dialoogvenster wordt weergegeven. Voer de domeinreferenties van uw in de opgegeven notatie. Klik op het pictogram van het selectievakje. De domeinreferenties moeten worden gecontroleerd. U ziet een foutbericht wordt weergegeven als de referenties onjuist zijn.

    ![](./media/storsimple-ova-deploy3-fs-setup/image7.png)

1.  Klik op **toepassen**. Hiermee wordt toepassen en de apparaatinstellingen valideren.

    ![](./media/storsimple-ova-deploy3-fs-setup/image8.png)

    > [AZURE.NOTE]
    > 
    > Zorg ervoor dat uw virtuele matrix bevindt zich in een eigen organisatie-eenheid voor Active Directory en geen GPO (GPO) worden toegepast of overgenomen. Groepsbeleid mag toepassingen, zoals antivirussoftware installeren op de virtuele StorSimple-matrix. Installatie van extra software wordt niet ondersteund en kan leiden tot beschadiging. 

1.  (Optioneel) configureren uw web-proxyserver. Hoewel de configuratie van de web-proxy optioneel is, worden de dat als u een webproxy gebruikt, u alleen u deze hier configureert kunt.

    ![](./media/storsimple-ova-deploy3-fs-setup/image9.png)

    Op de pagina **webproxy** :

    1.  De **URL van de Web-proxy** in deze indeling: *http://&lt;host-IP-adres of de FULLY&gt;: poortnummer*. Houd er rekening mee dat HTTPS-URL's worden niet ondersteund.

    2.  Geef **verificatie** als **eenvoudige** of **geen**.

    3.  Als verificatie gebruikt, moet u ook een **gebruikersnaam** en **wachtwoord**op te geven.

    4.  Klik op **toepassen**. Zo valideren en past u de geconfigureerde web proxy-instellingen.

1.  (Optioneel) de tijdinstellingen voor uw apparaat, zoals tijdzone en het primaire en secundaire NTP-servers configureren. NTP-servers zijn vereist omdat uw apparaat tijd synchroniseren moet, zodat deze met de cloud-serviceproviders verifiëren kan.

    ![](./media/storsimple-ova-deploy3-fs-setup/image10.png)

    Op de pagina **Instellingen voor tijd** :

    1.  Selecteer de **tijdzone** op basis van de geografische locatie waarin het apparaat wordt wordt geïmplementeerd in de vervolgkeuzelijst. De standaardtijdzone voor uw apparaat is PST-bestand. Uw apparaat, worden deze tijdzone gebruiken voor alle geplande bewerkingen.

    2.  Een **primaire NTP-server** voor uw apparaat opgeven of accepteer de standaardwaarde van time.windows.com. Zorg ervoor dat uw netwerk NTP-verkeer is toegestaan voor het doorsturen van uw datacenter met Internet is toegestaan.

    3.  Geef desgewenst een **secundaire NTP-server** voor uw apparaat.

    4.  Klik op **toepassen**. Zo valideren en past u de geconfigureerde tijd-instellingen.

1.  Configureer de cloud-instellingen voor uw apparaat. In deze stap geeft u Voltooi de configuratie lokaal apparaat en klikt u vervolgens het apparaat met uw StorSimple Manager-service registreren.

    1.  Voer de **registersleutel voor de Service** die u hebt ontvangen in [stap 2: ophalen van de sleutel voor de registratie](storsimple-ova-deploy1-portal-prep.md#step-2-get-the-service-registration-key) voor StorSimple virtuele matrix.

    2.  Deze stap overslaan als dit uw eerste apparaat registreren met deze service en Ga naar de volgende stap. Als dit niet het eerste apparaat dat u met deze service wilt registreren, moet u de **Service gegevens versleutelingssleutel**opgeven. Deze sleutel is vereist voor de toets van de registratie service om extra apparaten met de service StorSimple Manager registreren. Raadpleeg de [sleutel versleuteling gegevens](storsimple-ova-web-ui-admin.md#get-the-service-data-encryption-key) ophalen op uw lokale web UI voor meer informatie.

    3.  Klik op **registreren**. Hiermee wordt het apparaat opnieuw starten. Mogelijk moet u wachten op een 2-3 minuten voordat het apparaat dat is geregistreerd. Nadat het apparaat opnieuw is opgestart, wordt u naar de pagina aanmelden worden geleid.

        ![](./media/storsimple-ova-deploy3-fs-setup/image13.png)
    

1.  Ga terug naar de portal van Azure klassieke. Klik op de pagina **apparaten** controleren dat het apparaat dat is verbonden met de service door te zoeken naar de status. Status van het apparaat moet **actief**zijn.

![](./media/storsimple-ova-deploy3-fs-setup/image12.png)

## <a name="step-2-complete-the-required-device-setup"></a>Stap 2: De vereiste apparaatinstelling voltooien

U voltooit de configuratie van het apparaat van uw apparaat StorSimple, moet u:

-   Selecteer een account opslagruimte aan uw apparaat wilt koppelen.

-   Kies versleutelingsinstellingen voor de gegevens die wordt verzonden naar cloud.

De volgende stappen uitvoeren in de [klassieke Azure-portal](https://manage.windowsazure.com/) om de configuratie vereist apparaat te voltooien.

#### <a name="to-complete-the-minimum-device-setup"></a>De minimale apparaat-installatie te voltooien

1.  Selecteer het apparaat dat u zojuist hebt gemaakt op de pagina **apparaten** . Dit apparaat wilt weergegeven als **actief**. Klik op de pijl ten opzichte van de naam van het apparaat en klik vervolgens op **Snel starten**.

2.  Klik op **volledige Apparaatinstelling** Start de wizard van de apparaat configureren.

3.  Ga als volgt te werk in de wizard van de apparaat configureren op de pagina **Basisinstellingen** :

    1.  Geef een opslag-account moet worden gebruikt met uw apparaat. U kunt een bestaand account voor de opslag in dit abonnement in de vervolgkeuzelijst of **meer toevoegen** aan een account kiezen uit een ander abonnement opgeven.

    2.  Definieer de versleutelingsinstellingen voor alle de gegevens in rust (AES-versleuteling gebruikt) dat wordt verzonden naar de cloud. Voor het coderen van uw gegevens, Controleer de keuzelijst met invoervak naar **cloud opslag versleuteling toets inschakelen**. Voer een cloud opslag-versleuteling die 32 tekens bevat. Opnieuw op de toets om het te bevestigen. Een 256-bits AES-sleutel worden gebruikt met de gebruiker gedefinieerde sleutel voor versleuteling.

    3.  Klik op het pictogram van het selectievakje ![](./media/storsimple-ova-deploy3-fs-setup/image15.png).

        ![](./media/storsimple-ova-deploy3-fs-setup/image16.png)

De instellingen zijn nu bijgewerkt. Nadat de instellingen zijn bijgewerkt, wordt de knop van de setup voltooid apparaat grijs weergegeven. U gaat terug naar de pagina apparaat **Snel starten** .

 ![](./media/storsimple-ova-deploy3-fs-setup/image17.png)


> [AZURE.NOTE]                                                              
>
> U kunt alle andere apparaatinstellingen op elk gewenst moment wijzigen door de pagina **configureren** te openen.

## <a name="step-3-add-a-share"></a>Stap 3: Een delen toevoegen

De volgende stappen uitvoeren in de [portal van Azure klassieke](https://manage.windowsazure.com/) een share te maken.

#### <a name="to-create-a-share"></a>Een share maken

1.  Klik op **toevoegen een delen**op de pagina apparaat **Snel starten** . Hiermee start u de wizard delen toevoegen.

    ![](./media/storsimple-ova-deploy3-fs-setup/image17.png)

1.  Ga als volgt te werk op de pagina **Basisinstellingen** :

    1.  Geef een unieke naam voor uw delen. De naam moet een tekenreeks die 3 tot en met 127 tekens bevat.

    2.  (Optioneel) Geef een beschrijving voor de optie voor delen. De omschrijving zorgt ervoor dat de eigenaren delen achterhaald.

    3.  Selecteer een gebruikstype voor de optie voor delen. Het gebruikstype zijn **Tiered** of **lokaal vastgemaakt**, met doorverbonden wordt standaard. Voor werkbelastingen waarvoor lokale garanties, lage vertragingstijden en betere prestaties, selecteert u een **lokaal vastgemaakt** delen. Voor alle andere gegevens, selecteert u het delen van een **Tiered** .

    Een lokaal vastgemaakte delen dik is ingericht en zorgt ervoor dat de primaire gegevens op de optie voor delen lokaal op het apparaat blijft en Mors geen in de cloud. Een doorverbonden delen is echter dun ingericht. Wanneer u een doorverbonden delen maakt, 10% van de ruimte op de lokale laag is ingericht en 90% van de ruimte in de cloud is ingericht. Bijvoorbeeld als u een volume 1 TB deze is ingericht, 100 GB zou bevinden zich in de lokale ruimte en 900 GB zou worden gebruikt in de cloud wanneer de lagen gegevens. Dit betekent beurtelings als u afmelden bij de lokale ruimte op het apparaat, kunt u een doorverbonden delen niet kunt inrichten.

1.  Geef de ingerichte capaciteit voor uw delen. Houd er rekening mee dat de opgegeven capaciteit moet kleiner zijn dan de beschikbare capaciteit. Als een doorverbonden delen gebruikt, moet de grootte delen tussen 500 GB en 20 TB. Voor een lokaal vastgemaakte delen, Geef een grootte delen tussen 50 GB en 2 TB. Gebruik de beschikbare capaciteit als een handleiding voor het inrichten van een delen. Als de beschikbare lokale capaciteit 0 GB is, wordt klik u niet toegestaan voor het inrichten van lokale of doorverbonden aandelen.

    ![](./media/storsimple-ova-deploy3-fs-setup/image18.png)

1.  Klik op het pijlpictogram ![](./media/storsimple-ova-deploy3-fs-setup/image19.png) om te gaan naar de volgende pagina.

1.  De pagina **Aanvullende instellingen** moet u machtigingen toewijzen aan de gebruiker of de groep die toegang heeft tot deze delen. Geef de naam van de gebruiker of de groep in *<john@contoso.com>* opmaken. Het is raadzaam dat u een groep (in plaats van één gebruiker) toe te staan dat beheerdersbevoegdheden voor toegang tot deze waarden voor aandelen gebruiken. Nadat u de machtigingen hier hebt toegewezen, kunt u vervolgens Windows Verkenner gebruiken voor het wijzigen van deze machtigingen.

    ![](./media/storsimple-ova-deploy3-fs-setup/image20.png)

1.  Klik op het pictogram van het selectievakje ![](./media/storsimple-ova-deploy3-fs-setup/image21.png). Een share wordt gemaakt met de opgegeven instellingen. Standaard wordt cmdlets voor controle en back-up worden ingeschakeld voor de optie voor delen.

## <a name="step-4-connect-to-the-share"></a>Stap 4: Verbinding maken met de optie voor delen

U moet nu verbinding maken met de share(s) die u in de vorige stap hebt gemaakt. Deze stappen uitvoeren op uw Windows Server-host.

#### <a name="to-connect-to-the-share"></a>Verbinding maken met de optie voor delen

1.  Druk op ![](./media/storsimple-ova-deploy3-fs-setup/image22.png) + R. Geef in het venster uitvoeren op de * \\ * als het pad, kunt vervangen *server bestandsnaam* de naam van het apparaat dat u aan uw bestandsserver toegewezen. Klik op **OK**.

    ![](./media/storsimple-ova-deploy3-fs-setup/image23.png)

2.  Dit wordt Explorer geopend. U moet nu mogelijk om de waarden voor aandelen die u hebt gemaakt als mappen weer te geven. Selecteer en dubbelklik op een delen (map) om de inhoud weer te geven.

    ![](./media/storsimple-ova-deploy3-fs-setup/image24.png)

3.  U kunt nu bestanden toevoegen aan deze waarden voor aandelen en een back-up uitvoeren.

![videopictogram](./media/storsimple-ova-deploy3-fs-setup/video_icon.png) **Video beschikbaar**

Bekijk de video om te zien hoe u kunt configureren en een virtuele matrix StorSimple registreren als een bestandsserver.

> [AZURE.VIDEO configure-a-storsimple-virtual-array]

## <a name="next-steps"></a>Volgende stappen

Informatie over het gebruik van de lokale web gebruikersinterface voor het [beheren van uw StorSimple virtuele matrix](storsimple-ova-web-ui-admin.md).
