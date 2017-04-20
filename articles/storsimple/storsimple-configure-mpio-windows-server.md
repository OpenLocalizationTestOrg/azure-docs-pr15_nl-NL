<properties 
   pageTitle="MPIO configureren voor uw apparaat StorSimple | Microsoft Azure"
   description="Wordt beschreven hoe paden i/o-(MPIO) configureren voor uw apparaat StorSimple is verbonden met een host met Windows Server 2012 R2."
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
   ms.workload="NA"
   ms.date="08/17/2016"
   ms.author="alkohli" />

# <a name="configure-multipath-io-for-your-storsimple-device"></a>MPIO configureren voor uw apparaat StorSimple

Microsoft ondersteuning voor de functie paden i/o-(MPIO) in Windows Server naar help opbouwen ten zeerste beschikbaar, fouttolerantie SAN configuraties ingebouwd. MPIO overtollige fysieke padonderdelen gebruikt, adapters, kabels en schakelopties, logische paden tussen de server en het opslagapparaat maken. Als er een onderdeel is mislukt is, veroorzaakt door een logische pad mislukt, wordt de logica meerdere paden gebruiken een alternatief pad voor I/O, zodat toepassingen nog steeds toegang hun gegevens tot kunnen. Bovendien afhankelijk van uw configuratie MPIO kunt ook de prestaties verbeteren door het selectievakje laden opnieuw te verdelen over alle deze paden. Zie [overzicht van MPIO](https://technet.microsoft.com/library/cc725907.aspx "MPIO overzicht en functies")voor meer informatie.  

Voor de hoge beschikbaarheid van de oplossing StorSimple, moet MPIO op uw apparaat StorSimple zijn geconfigureerd. Wanneer MPIO is geïnstalleerd op uw hostservers met Windows Server 2012 R2, wordt de servers mogelijk vervolgens zonder een koppeling, netwerk of interface is mislukt. 

MPIO is een optionele functie op Windows Server en wordt niet standaard geïnstalleerd. Deze moet worden geïnstalleerd als een functie via Serverbeheer. In dit onderwerp worden de stappen u moet volgen om te installeren en gebruiken van de functie MPIO op een host met Windows Server 2012 R2 en verbonden met een fysieke StorSimple-apparaat.

>[AZURE.NOTE] **Deze procedure geldt voor alleen StorSimple 8000 reeks. MPIO is momenteel niet ondersteund op een virtueel StorSimple-apparaat.**

U moet als volgt te werk om te MPIO configureren op uw apparaat StorSimple:

- Stap 1: Installatiefout MPIO op de Windows Server-host

- Stap 2: Configureer MPIO voor StorSimple volumes

- Stap 3: Mountains StorSimple volumes op de host

- Stap 4: MPIO configureren voor maximale beschikbaarheid en taakverdeling

Elk van de bovenstaande stappen uit wordt in de volgende secties besproken.

## <a name="step-1-install-mpio-on-the-windows-server-host"></a>Stap 1: Installatiefout MPIO op de Windows Server-host

Als u wilt deze functie installeren op uw Windows Server-host, voert u de volgende procedure uit.

#### <a name="to-install-mpio-on-the-host"></a>MPIO installeren op de host

1. Open Serverbeheer op uw Windows Server-host. Standaard wordt Serverbeheer gestart wanneer een lid van de groep Administrators zich aanmeldt bij een computer waarop Windows Server 2012 R2 of Windows Server 2012 is. Als de Server Manager nog niet is geopend, klikt u op **starten > Serverbeheer**.
![Serverbeheer](./media/storsimple-configure-mpio-windows-server/IC740997.png)
2. Klik op **Serverbeheer > Dashboard > functies en onderdelen toevoegen**. Hiermee start de wizard **rollen toevoegen en functies** .
![Rollen en functies Wizard 1 toevoegen](./media/storsimple-configure-mpio-windows-server/IC740998.png)
3. Ga als volgt te werk in de wizard **functies en functies toevoegen** :

    - Klik op **volgende**op de pagina **voordat u begint** .
    - Accepteer de standaardinstelling van **rollen gebaseerde of functies gebaseerde** installatie op de pagina **installatietype selecteren** . Klik op **volgende**. ![Rollen en functies Wizard 2 toevoegen](./media/storsimple-configure-mpio-windows-server/IC740999.png)
    - Kies **een server uit de groep server selecteren**op de pagina **Selecteer doelserver** . De hostserver moet automatisch worden gedetecteerd. Klik op **volgende**.
    - Klik op **volgende**op de pagina **Selecteer serverrollen** .
    - Klik op de pagina **onderdelen van de** **MPIO**selecteren en klik op **volgende**. ![Rollen en functies Wizard 5 toevoegen](./media/storsimple-configure-mpio-windows-server/IC741000.png)
    - Klik op de pagina **Bevestig installatie selecties** Bevestig de selectie en selecteer **opnieuw de doelserver automatisch indien nodig**, zoals hieronder wordt weergegeven. Klik op **installeren**. ![Rollen en functies Wizard 8 toevoegen](./media/storsimple-configure-mpio-windows-server/IC741001.png)
    - U krijgt wanneer de installatie voltooid is. Klik op **sluiten** om de wizard te sluiten. ![Rollen en functies Wizard 9 toevoegen](./media/storsimple-configure-mpio-windows-server/IC741002.png)

## <a name="step-2-configure-mpio-for-storsimple-volumes"></a>Stap 2: Configureer MPIO voor StorSimple volumes

MPIO moet worden geconfigureerd voor StorSimple volumes identificeren. Als u wilt configureren MPIO herkend StorSimple volumes, moet u de volgende stappen uitvoeren.

#### <a name="to-configure-mpio-for-storsimple-volumes"></a>MPIO configureren voor StorSimple volumes

1. Open de **MPIO-configuratie**. Klik op **Serverbeheer > Dashboard > hulpmiddelen > MPIO**.

2. Selecteer het tabblad **Kennismaken met meerdere paden** in het dialoogvenster **Eigenschappen voor MPIO** .

3. Selecteer **ondersteuning voor iSCSI-apparaten toevoegen**en klik vervolgens op **toevoegen**.  
![Eigenschappen voor MPIO kennismaken met meerdere paden](./media/storsimple-configure-mpio-windows-server/IC741003.png)

4. Start opnieuw op de server wanneer u wordt gevraagd.
5. Klik op het tabblad **MPIO apparaten** in het dialoogvenster **Eigenschappen voor MPIO** . Klik op **toevoegen**.
    </br>![MPIO eigenschappen MPIO apparaten](./media/storsimple-configure-mpio-windows-server/IC741004.png)
6. Voer in het dialoogvenster **MPIO-ondersteuning toevoegen** onder **Hardware-ID apparaat**serienummer van uw apparaat. U kunt het seriële getal van apparaat openen door toegang tot uw StorSimple Manager-service, en navigeren naar **apparaten > Dashboard**. Het seriële getal van apparaat wordt weergegeven in de rechterkant deelvenster **Snelle bekijken** van het dashboard van het apparaat.
    </br>![Ondersteuning voor MPIO toevoegen](./media/storsimple-configure-mpio-windows-server/IC741005.png)
7. Start opnieuw op de server wanneer u wordt gevraagd.

## <a name="step-3-mount-storsimple-volumes-on-the-host"></a>Stap 3: Mountains StorSimple volumes op de host

Nadat MPIO is geconfigureerd op Windows Server, wordt volume (s) die zijn gemaakt op het apparaat StorSimple kunnen worden gekoppeld en kunt vervolgens profiteren van MPIO voor redundantie. Als u wilt een volume koppelen, moet u de volgende stappen uitvoeren.

#### <a name="to-mount-volumes-on-the-host"></a>Volumes koppelen op de host

1. Open het venster **iSCSI Initiator eigenschappen** op de Windows Server-host. Klik op **Serverbeheer > Dashboard > hulpmiddelen > iSCSI Initiator**.
2. Klik op het tabblad Discovery in het dialoogvenster **iSCSI Initiator eigenschappen** en klik vervolgens op **Doel-Portal ontdekken**.
3. Ga als volgt te werk in het dialoogvenster **Kennismaken met doel-Portal** :
    
    - Geef het IP-adres van de gegevens-poort van uw apparaat StorSimple (Voer bijvoorbeeld gegevens 0).
    - Klik op **OK** om terug te keren naar het dialoogvenster **iSCSI Initiator eigenschappen** .

    >[AZURE.IMPORTANT] **Als u een privé-netwerk voor iSCSI-verbindingen gebruikt, voert u het IP-adres van de poort die is gekoppeld aan het particuliere netwerk.**

4. Herhaal stap 2-3 voor een tweede netwerkinterface (bijvoorbeeld gegevens 1) op uw apparaat. Houd er rekening mee dat deze interfaces moeten worden ingeschakeld voor iSCSI. Zie meer informatie over dit [netwerk via interfaces zoals wijzigen](storsimple-modify-device-config.md#modify-network-interfaces).
5. Selecteer het tabblad **doelen** in het dialoogvenster **iSCSI Initiator eigenschappen** . Hier ziet u het StorSimple apparaat doel IQN onder **Gedetecteerde doelen**.
 ![iSCSI Initiator eigenschappen doelen tabblad](./media/storsimple-configure-mpio-windows-server/IC741007.png)
6. Klik op **verbinding maken** om te maken van een iSCSI-sessie met uw apparaat StorSimple. Een **verbinding maken met doel** -dialoogvenster wordt weergegeven.

7. Selecteer het selectievakje **meerdere paden inschakelen** in het dialoogvenster **verbinding maken met doel** . Klik op **Geavanceerd**.

8. Ga als volgt te werk in het dialoogvenster **Advanced Settings** :                                       
    -    Selecteer op de vervolgkeuzelijst **Lokale Adapter** **Microsoft iSCSI Initiator**.
    -    Selecteer op de vervolgkeuzelijst **Initiator IP** het IP-adres van de host.
    -    Op de **Portal van Target** IP-vervolgkeuzelijst, selecteert u de IP van apparaatinterface.
    -    Klik op **OK** om terug te keren naar het dialoogvenster **iSCSI Initiator eigenschappen** .

9. Klik op **Eigenschappen**. Klik in het dialoogvenster **Eigenschappen** op **Sessie toevoegen**.
10. Selecteer het selectievakje **meerdere paden inschakelen** in het dialoogvenster **verbinding maken met doel** . Klik op **Geavanceerd**.
11. Klik in het dialoogvenster **Advanced Settings** :                                        
    -  Selecteer op de vervolgkeuzelijst **lokale adapter** Microsoft iSCSI Initiator.
    -  Selecteer op de vervolgkeuzelijst **Initiator IP** het IP-adres dat overeenkomt met de host. In dit geval verbinding u twee netwerkinterfaces op het apparaat dat met een interface één netwerk op de host. Daarom is deze interface hetzelfde als die voor de eerste sessie.
    -  Selecteer op de vervolgkeuzelijst **Target Portal IP** het IP-adres voor de tweede gegevens-interface op het apparaat is ingeschakeld.
    -  Klik op **OK** om terug te keren naar het dialoogvenster van iSCSI Initiator eigenschappen. U kunt een tweede sessie hebt toegevoegd aan het doel.

12. **Beheer van de Computer** openen door te schuiven naar **Serverbeheer > Dashboard > Computer Management**. Klik in het linkerdeelvenster op **opslag > beheer van de schijf**. Het volume (s) die zijn gemaakt op het apparaat StorSimple die zichtbaar zijn voor deze host wordt weergegeven onder **Beheer van de schijf** als nieuwe schijven.

13. De schijf geïnitialiseerd en maak een nieuw volume. Tijdens de opmaak, selecteert u de grootte van een blokkeren van 64 KB.
![Schijfbeheer van de](./media/storsimple-configure-mpio-windows-server/IC741008.png)
14. Klik onder **Beheer van de schijf**met de rechtermuisknop op de **schijf** en selecteer **Eigenschappen**.
15. In het StorSimple Model ### **Meerdere paden schijf apparaateigenschappen** dialoogvenster, klikt u op het tabblad **MPIO** .
![StorSimple 8100 meerdere paden schijf DeviceProp.](./media/storsimple-configure-mpio-windows-server/IC741009.png)

16. In de sectie **DSV naam** , klikt u op **Details** en controleer of dat de parameters zijn ingesteld op de standaardparameters. De standaardparameters zijn:

    - Pad verifiëren periode = 30
    - Probeer tellen = 3
    - Bob periode verwijdert = 20
    - Herhalingsinterval = 1
    - Pad verifiëren ingeschakeld = uitgeschakeld.


>[AZURE.NOTE] **De standaardparameters niet wijzigen.**

## <a name="step-4-configure-mpio-for-high-availability-and-load-balancing"></a>Stap 4: MPIO configureren voor maximale beschikbaarheid en taakverdeling

Voor meerdere paden op basis van beschikbaarheid en taakverdeling, moeten meerdere sessies handmatig worden toegevoegd aan de andere beschikbare paden declareren. Bijvoorbeeld als de host heeft twee interfaces verbonden met SAN en het apparaat heeft twee interfaces die zijn verbonden met SAN, moet u vier sessies geconfigureerd met het juiste pad permutaties (slechts twee sessies moeten worden uitgevoerd als elke gegevens interface en hetzelfde hostinterface in een ander IP-subnet is en niet geschikt is).

>[AZURE.IMPORTANT] **Het is raadzaam dat u niet door elkaar 1 GbE en 10 GbE netwerk-interfaces. Als u twee netwerkinterfaces gebruikt, moet beide interfaces het identieke type.**

De volgende procedure wordt beschreven hoe sessies wanneer een apparaat StorSimple met twee netwerkinterfaces is verbonden met een host met twee netwerkinterfaces toevoegen.

### <a name="to-configure-mpio-for-high-availability-and-load-balancing"></a>MPIO configureren voor maximale beschikbaarheid en taakverdeling

1. Het doel te doorzoeken: Klik in het dialoogvenster **iSCSI Initiator eigenschappen** op het tabblad **Discovery** op **Ontdekken Portal**.
2. Voer in het dialoogvenster **verbinding maken met doel** het IP-adres van een van de apparaat netwerk-interfaces.
3. Klik op **OK** om terug te keren naar het dialoogvenster **iSCSI Initiator eigenschappen** .

4. Selecteer het tabblad **doelen** in het dialoogvenster **iSCSI Initiator eigenschappen** , het ontdekt doel markeren en klik op **verbinding maken**. Het dialoogvenster **verbinding maken met de doellijst** wordt weergegeven.

5. Klik in het dialoogvenster **verbinding maken met doel** :
    
    - Laat het standaard geselecteerde doel instellen voor **deze verbinding toevoegen** aan de lijst met favoriete doelen. Hiermee maakt het apparaat dat automatisch wordt geprobeerd om de verbinding opnieuw telkens wanneer deze computer opnieuw is opgestart te starten.
    - Schakel het selectievakje **meerdere paden inschakelen** .
    - Klik op **Geavanceerd**.

6. Klik in het dialoogvenster **Advanced Settings** :
    - Selecteer op de vervolgkeuzelijst **Lokale Adapter** **Microsoft iSCSI Initiator**.
    - Selecteer op de vervolgkeuzelijst **Initiator IP** het IP-adres van de host.
    - Selecteer op de vervolgkeuzelijst **Target Portal IP** het IP-adres van de interface van de gegevens op het apparaat is ingeschakeld.
    - Klik op **OK** om terug te keren naar het dialoogvenster van iSCSI Initiator eigenschappen.

7. Klik op **Eigenschappen**en klik in het dialoogvenster **Eigenschappen** op **Sessie toevoegen**.

8. Schakel het selectievakje **meerdere paden inschakelen** in het dialoogvenster **verbinding maken met de doeltoepassing** en klik vervolgens op **Geavanceerd**.

9. Klik in het dialoogvenster **Advanced Settings** :
    1. Selecteer op de vervolgkeuzelijst **lokale adapter** **Microsoft iSCSI Initiator**.
    2. Selecteer op de vervolgkeuzelijst **Initiator IP** het IP-adres dat overeenkomt met de tweede interface op de host.
    3. Selecteer op de vervolgkeuzelijst **Target Portal IP** het IP-adres voor de tweede gegevens-interface op het apparaat is ingeschakeld.
    4. Klik op **OK** om terug te keren naar het dialoogvenster **iSCSI Initiator eigenschappen** . U hebt nu een tweede sessie toegevoegd aan het doel.

10. Herhaal stap 8-10 als u wilt toevoegen van extra sessies (paden) naar de doelsite. Met twee interfaces op de host en twee op het apparaat, kunt u een totaal van vier sessies toevoegen.

11. Na het toevoegen van de gewenste sessies (paden) in het dialoogvenster **iSCSI Initiator eigenschappen** , selecteer het doel en klik op **Eigenschappen**. Opmerking de vier sessie id's die met het pad mogelijke permutaties overeenkomen op het tabblad sessies van het dialoogvenster **Eigenschappen** . Als u een sessie, schakel het selectievakje naast een sessie-id en klik vervolgens op **verbinding verbreken**.

12. Als apparaten wilt weergeven in sessies gepresenteerd, selecteer het tabblad **apparaten** . Als u wilt de instelling MPIO voor een geselecteerde apparaat is geconfigureerd, klikt u op **MPIO**. Het dialoogvenster **Details van apparaat** wordt weergegeven. Klik op het tabblad **MPIO** kunt u de juiste **Beleid voor taakverdeling** -instellingen. U kunt ook de **actieve** **stand-by** pad van het type of weergeven.

## <a name="next-steps"></a>Volgende stappen

Meer informatie over het [gebruik van de service StorSimple Manager als u wilt wijzigen van de configuratie van uw StorSimple-apparaten](storsimple-modify-device-config.md).
 
