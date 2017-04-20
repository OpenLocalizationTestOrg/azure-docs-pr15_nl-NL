<properties 
   pageTitle="MPIO configureren op uw StorSimple virtuele matrix host | Microsoft Azure"
   description="Wordt beschreven hoe paden i/o-(MPIO) configureren voor uw StorSimple is virtual matrix verbonden met een host met Windows Server 2012 R2."
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
   ms.date="10/04/2016"
   ms.author="alkohli" />

# <a name="configure-multipath-io-on-windows-server-host-for-the-storsimple-virtual-array"></a>MPIO op Windows Server-host voor de virtuele matrix StorSimple configureren

## <a name="overview"></a>Overzicht

In dit artikel wordt beschreven hoe MPIO-functie (MPIO) installeren op uw Windows Server-host, specifieke configuratie-instellingen voor alleen-StorSimple volumes toepassen en controleer of MPIO voor StorSimple volumes. De procedure wordt ervan uitgegaan dat uw StorSimple 1200 virtuele matrix met twee netwerkinterfaces is verbonden met een Windows Server-host met twee netwerkinterfaces. De informatie in dit artikel geldt alleen voor de virtuele matrix. Voor informatie over StorSimple 8000 reeks apparaten, Ga naar [MPIO configureren voor StorSimple host](storsimple-configure-mpio-windows-server.md). 

De functie MPIO in Windows Server helpt opbouwen ten zeerste beschikbaar, fouttolerantie opslagconfiguraties. MPIO overtollige fysieke padonderdelen gebruikt, adapters, kabels en schakelopties, logische paden tussen de server en het opslagapparaat maken. Als er een onderdeel is mislukt is, veroorzaakt door een logische pad mislukt, wordt de logica meerdere paden gebruiken een alternatief pad voor I/O, zodat toepassingen nog steeds toegang hun gegevens tot kunnen. Bovendien afhankelijk van uw configuratie MPIO kunt ook de prestaties verbeteren door het selectievakje laden opnieuw te verdelen over alle deze paden. Zie [overzicht van MPIO](https://technet.microsoft.com/library/cc725907.aspx "MPIO overzicht en functies")voor meer informatie.  

Voor de hoge beschikbaarheid van de oplossing StorSimple, configureert u MPIO op de Windows Server-hosts verbonden met uw StorSimple 1200 virtuele matrix (ook wel bekend als het on-premises implementatie virtuele apparaat). De hostservers mogelijk vervolgens zonder een koppeling, netwerk of interface is mislukt. 

U moet als volgt te werk om te MPIO configureren: 

- Vereisten voor de configuratie

- Stap 1: Installatiefout MPIO op de Windows Server-host

- Stap 2: Configureer MPIO voor StorSimple volumes

- Stap 3: Mountains StorSimple volumes op de host

Elk van de bovenstaande stappen uit wordt in de volgende secties besproken.


## <a name="prerequisites"></a>Vereisten voor

In dit gedeelte details over de vereisten voor de configuratie voor de host van Windows Server en uw virtuele matrix.

### <a name="on-windows-server-host"></a>Op Windows Server-host

-  Zorg ervoor dat de host van uw Windows Server 2 netwerkinterfaces ingeschakeld heeft.


### <a name="on-storsimple-virtual-array"></a>Klik op StorSimple virtuele matrix

- De virtuele matrix moet worden geconfigureerd als een iSCSI-server. Zie meer informatie, [virtuele matrix als een server iSCSI instellen](storsimple-ova-deploy3-iscsi-setup.md). Een of meer netwerkinterfaces moeten worden ingeschakeld voor de matrix.   

- De netwerkinterfaces op uw virtuele matrix moeten bereikbaar uit de Windows Server-host.

- Één of meer moeten worden gemaakt op uw StorSimple virtuele matrix. Meer informatie raadpleegt u [toevoegen een volume](storsimple-ova-deploy3-iscsi-setup.md#step-3-add-a-volume) op uw StorSimple 1200 virtuele matrix. In deze procedure wordt 3 delen (2 lokaal vastgemaakte en 1 doorverbonden volume zoals hieronder) op de virtuele matrix gemaakt.
    
    ![mpio0](./media/storsimple-ova-configure-mpio-windows-server/mpio0.png)

### <a name="hardware-configuration-for-storsimple-virtual-array"></a>Hardwareconfiguratie voor StorSimple virtuele matrix

De onderstaande afbeelding ziet u de hardwareconfiguratie voor maximale beschikbaarheid en meerdere paden taakverdeling gebruiken voor uw Windows Server host en StorSimple virtuele matrix die in deze procedure worden gebruikt.  

![hardwareconfiguratie MPIO](./media/storsimple-ova-configure-mpio-windows-server/1200hardwareconfig.png)

Zoals u in de bovenstaande afbeelding:

- Uw StorSimple virtuele matrix deze is ingericht op Hyper-V is een enkel knooppunt actieve apparaat geconfigureerd als een iSCSI-server.

- Twee virtueel netwerk-interfaces zijn ingeschakeld op de matrix. Controleer in het lokale web UI van uw 1200 virtuele matrix, of dat er twee netwerkinterfaces zijn ingeschakeld door te schuiven **netwerkinstellingen** , zoals hieronder wordt weergegeven:

    ![Netwerkinterfaces 1200 ingeschakeld](./media/storsimple-ova-configure-mpio-windows-server/mpio9.png)
    
    Houd rekening met de IPv4-adressen van de ingeschakelde netwerkinterfaces (Ethernet, Ethernet 2 al dan niet standaard) en opslaan voor later gebruik op de host.

- Twee netwerkinterfaces zijn ingeschakeld op uw Windows Server-host. Als de verbonden interfaces voor host en matrix die zich in dezelfde subnet, wordt er 4 paden beschikbaar. Dit is de hoofdletters/kleine letters in deze procedure. Als de netwerkinterface van elke op de matrix en host-interface in een ander IP-subnet (en niet worden omgeleid), wordt klikt u vervolgens alleen 2 paden wel beschikbaar.

## <a name="step-1-install-mpio-on-the-windows-server-host"></a>Stap 1: Installatiefout MPIO op de Windows Server-host

MPIO is een optionele functie op Windows Server en wordt niet standaard geïnstalleerd. Deze moet worden geïnstalleerd als een functie via Serverbeheer. Als u wilt deze functie installeren op uw Windows Server-host, voert u de volgende procedure uit.

[AZURE.INCLUDE [storsimple-install-mpio-windows-server-host](../../includes/storsimple-install-mpio-windows-server-host.md)]


## <a name="step-2-configure-mpio-for-storsimple-volumes"></a>Stap 2: Configureer MPIO voor StorSimple volumes

MPIO moet worden geconfigureerd voor StorSimple volumes identificeren. Als u wilt configureren MPIO herkend StorSimple volumes, moet u de volgende stappen uitvoeren.

[AZURE.INCLUDE [storsimple-configure-mpio-volumes](../../includes/storsimple-configure-mpio-volumes.md)]

## <a name="step-3-mount-storsimple-volumes-on-the-host"></a>Stap 3: Mountains StorSimple volumes op de host

Nadat MPIO is geconfigureerd op Windows Server, wordt gemaakt in de matrix StorSimple volume kunnen worden gekoppeld en kunt vervolgens profiteren van MPIO voor redundantie. Als u wilt een volume koppelen, moet u de volgende stappen uitvoeren.

#### <a name="to-mount-volumes-on-the-host"></a>Volumes koppelen op de host

1. Open het venster **iSCSI Initiator eigenschappen** op de Windows Server-host. Klik op **Serverbeheer > Dashboard > hulpmiddelen > iSCSI Initiator**.
2. Klik op het tabblad Discovery in het dialoogvenster **iSCSI Initiator eigenschappen** en klik vervolgens op **Doel-Portal ontdekken**.
3. Ga als volgt te werk in het dialoogvenster **Kennismaken met doel-Portal** :
    
    - Voer het IP-adres van het eerste ingeschakeld netwerk op uw StorSimple virtuele matrix. Al dan niet standaard zou dit **Ethernet**. 
    - Klik op **OK** om terug te keren naar het dialoogvenster **iSCSI Initiator eigenschappen** .

    >[AZURE.IMPORTANT] **Als u een privé-netwerk voor iSCSI-verbindingen gebruikt, voert u het IP-adres van de poort die is gekoppeld aan het particuliere netwerk.**

4. Herhaal stap 2-3 voor een tweede netwerkinterface (bijvoorbeeld Ethernet-2) in de matrix. 

5. Selecteer het tabblad **doelen** in het dialoogvenster **iSCSI Initiator eigenschappen** . Voor uw virtuele matrix ziet u elk oppervlak volume als doel onder **Gedetecteerde doelen**. In dit geval zou drie doelen (overeenkomend drie volume) worden ontdekt.

    ![mpio1](./media/storsimple-ova-configure-mpio-windows-server/mpio1.png)

6. Klik op **verbinding maken** om te maken van een iSCSI-sessie met uw StorSimple-matrix. Een **verbinding maken met doel** -dialoogvenster wordt weergegeven. Schakel het selectievakje **meerdere paden inschakelen** . Klik op **Geavanceerd**.

    ![mpio2](./media/storsimple-ova-configure-mpio-windows-server/mpio2.png)

8. Ga als volgt te werk in het dialoogvenster **Advanced Settings** :                                       
    -    Selecteer op de vervolgkeuzelijst **Lokale Adapter** **Microsoft iSCSI Initiator**.
    -    Selecteer op de vervolgkeuzelijst **Initiator IP** het IP-adres van de host.
    -    Op de **Portal van Target** IP-vervolgkeuzelijst, selecteert u de IP van matrix interface.
    -    Klik op **OK** om terug te keren naar het dialoogvenster **iSCSI Initiator eigenschappen** .

    ![mpio3](./media/storsimple-ova-configure-mpio-windows-server/mpio3.png)

9. Klik op **Eigenschappen**. 

    ![mpio4](./media/storsimple-ova-configure-mpio-windows-server/mpio4.png)
10. Klik in het dialoogvenster **Eigenschappen** op **Sessie toevoegen**.

    ![mpio5](./media/storsimple-ova-configure-mpio-windows-server/mpio5.png)

10. Selecteer het selectievakje **meerdere paden inschakelen** in het dialoogvenster **verbinding maken met doel** . Klik op **Geavanceerd**.
11. Klik in het dialoogvenster **Advanced Settings** :                                        
    -  Selecteer op de vervolgkeuzelijst **lokale adapter** Microsoft iSCSI Initiator.
    -  Selecteer op de vervolgkeuzelijst **Initiator IP** het IP-adres dat overeenkomt met de host. In dit geval u verbinding maakt twee netwerkinterfaces op de matrix met een interface één netwerk op de host. Daarom is deze interface hetzelfde als die voor de eerste sessie.
    -  Selecteer op de vervolgkeuzelijst **Target Portal IP** het IP-adres voor de tweede gegevens-interface ingeschakeld op de matrix.
    -  Klik op **OK** om terug te keren naar het dialoogvenster van iSCSI Initiator eigenschappen. U kunt een tweede sessie hebt toegevoegd aan het doel.

        ![mpio11](./media/storsimple-ova-configure-mpio-windows-server/mpio11.png)

    - Na het toevoegen van de gewenste sessies (paden) in het dialoogvenster **iSCSI Initiator eigenschappen** , selecteer het doel en klik op **Eigenschappen**. Opmerking de vier sessie id's die met het pad mogelijke permutaties overeenkomen op het tabblad sessies van het dialoogvenster **Eigenschappen** . Als u een sessie, schakel het selectievakje naast een sessie-id en klik vervolgens op **verbinding verbreken**.
 
    - Als apparaten wilt weergeven in sessies gepresenteerd, selecteer het tabblad **apparaten** . Als u wilt de instelling MPIO voor een geselecteerde apparaat is geconfigureerd, klikt u op **MPIO**. De **
    -  Details** dialoogvenster wordt weergegeven. Klik op de **MPIO** tabblad, kunt u de juiste **beleid voor taakverdeling** instellingen. U kunt ook weergeven de **actief** of **stand-by ** padtype.

10. Herhaal stap 8-11 extra sessies (paden) toevoegen aan het doel. Met twee interfaces op de host en twee in de virtuele matrix, kunt u een totaal van vier sessies voor elke doelsite toevoegen. 

    ![mpio14](./media/storsimple-ova-configure-mpio-windows-server/mpio14.png)

11. U moet Herhaal deze stappen voor elk volume (oppervlak als doel).

    ![mpio15](./media/storsimple-ova-configure-mpio-windows-server/mpio15.png)

12. **Beheer van de Computer** openen door te schuiven naar **Serverbeheer > Dashboard > Computer Management**. Klik in het linkerdeelvenster op **opslag > beheer van de schijf**. Het volume (s) op de StorSimple virtuele matrix gemaakt die zichtbaar zijn voor deze host wordt weergegeven onder **Beheer van de schijf** als nieuwe schijven.

13. De schijf geïnitialiseerd en maak een nieuw volume. Selecteer een toewijzing eenheidsgrootte (Australië) van 64 KB tijdens de opmaak. Herhaal het proces voor de beschikbare hoeveelheden.

    ![Schijfbeheer van de](./media/storsimple-ova-configure-mpio-windows-server/mpio20.png)

14. Klik onder **Beheer van de schijf**met de rechtermuisknop op de **schijf** en selecteer **Eigenschappen**.

15. Klik op het tabblad **MPIO** in het dialoogvenster **Meerdere paden schijf apparaateigenschappen** .

    ![Schijfeigenschappen van de](./media/storsimple-ova-configure-mpio-windows-server/mpio21.png)

16. In de sectie **DSV naam** , klikt u op **Details** en controleer of dat de parameters zijn ingesteld op de standaardparameters. De standaardparameters zijn:

    - Pad verifiëren periode = 30
    - Probeer tellen = 3
    - Bob periode verwijdert = 20
    - Herhalingsinterval = 1
    - Pad verifiëren ingeschakeld = uitgeschakeld.

    >[AZURE.NOTE] **De standaardparameters niet wijzigen.**


## <a name="next-steps"></a>Volgende stappen

Meer informatie over het [gebruik van de StorSimple Manager-service voor het beheren van uw StorSimple virtuele matrix](storsimple-ova-manager-service-administration.md).
 
