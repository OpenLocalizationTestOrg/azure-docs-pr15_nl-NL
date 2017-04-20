<properties
   pageTitle="StorSimple begint (U2) | Microsoft Azure"
   description="Dit artikel wordt uitgelegd hoe u kunt toevoegen, wijzigen, bewaken en StorSimple volumes verwijderen en hoe u ze offline halen indien nodig."
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
   ms.workload="NA"
   ms.date="10/28/2016"
   ms.author="alkohli" />

# <a name="use-the-storsimple-manager-service-to-manage-volumes-update-2"></a>Gebruik van de StorSimple Manager-service voor het beheren van volumes (Update 2)

[AZURE.INCLUDE [storsimple-version-selector-manage-volumes](../../includes/storsimple-version-selector-manage-volumes.md)]

## <a name="overview"></a>Overzicht

Deze zelfstudie wordt uitgelegd hoe u het gebruik van de service StorSimple Manager maken en beheren van volumes op de StorSimple en StorSimple virtuele apparaat met 2 is geïnstalleerd.

De StorSimple Manager-service is een uitbreiding in de portal van Azure klassieke waarmee u uw oplossing StorSimple beheren vanuit een één webservice-interface. Naast het beheren van volumes, kunt u de service StorSimple Manager maken en StorSimple services beheren, weergeven en apparaten beheren, waarschuwingen, weergeven en bekijken en beheren van back-beleid en de back-catalogus.

## <a name="volume-types"></a>Volumetypen

StorSimple volumes kunnen zijn:

- **Lokaal vastgemaakt volumes**: gegevens in deze hoeveelheden blijven op de lokale StorSimple apparaat allen tijde.
- **Tiered volumes**: gegevens in deze hoeveelheden kunt Mors in de cloud.

Het volume van een archivering is een type gelaagde volume. De deduplication segment groter gebruikt voor archivering volumes kunt het apparaat wilt overbrengen groter segmenten van gegevens naar de cloud. 

Als nodig is, kunt u het volume van lokale naar doorverbonden of doorverbonden naar lokale uit. Ga naar [het volumetype wijzigen](#change-the-volume-type)voor meer informatie.

### <a name="locally-pinned-volumes"></a>Lokaal vastgemaakte volumes

Lokaal vastgemaakte volumes zijn volledig ingerichte volumes die geen gegevens van laag naar de cloud uitvoeren, waardoor er lokale garandeert voor primaire gegevens, onafhankelijk van cloud connectivity. Gegevens op lokaal vastgemaakte volumes is niet deduplicated en gecomprimeerd. momentopnamen van lokaal vastgemaakte hoeveelheden zijn echter deduplicated. 

Lokaal vastgemaakte volumes is volledig ingericht; Daarom moet er voldoende ruimte is op uw apparaat wanneer u berekende velden maakt. U kunt lokaal vastgemaakte volumes snel aan de maximale grootte van 8 TB op het apparaat StorSimple 8100 en 20 TB op het apparaat 8600 inrichten. StorSimple behoudt de resterende lokale ruimte op het apparaat voor momentopnamen, metagegevens en gegevensverwerking. U kunt de tekengrootte van een lokaal vastgemaakte volume aan hoeveel ruimte maximaal beschikbaar, maar u kunt geen de grootte van een volume eenmaal hebt gemaakt.

Wanneer u een lokaal vastgemaakte volume maakt, wordt de beschikbare ruimte voor het maken van trapsgewijze hoeveelheden verminderd. Het omgekeerde geldt ook: hebt u een bestaand doorverbonden volume, de ruimte beschikbaar voor het maken van lokaal vastgemaakt volumes worden lager is dan de bovenstaande maximale grenzen. Raadpleeg de [Veelgestelde vragen over lokaal vastgemaakte volumes](storsimple-local-volume-faq.md)voor meer informatie over lokale volumes.   

### <a name="tiered-volumes"></a>Gelaagde volumes

Gelaagde volumes zijn dun ingerichte volumes waarin de veelgebruikte gegevens blijft lokale op het apparaat en minder veelgebruikte gegevens automatisch is doorverbonden naar de cloud. Dunne inrichting is een virtualization-technologie waarin beschikbare opslagruimte overschrijdt fysieke bronnen wordt weergegeven. In plaats van voldoende opslagruimte tevoren reserveren, StorSimple gebruikt dunne inrichting net voldoende ruimte om te voldoen aan vereisten voor huidige toe te wijzen. De elastische aard van cloudopslag vergemakkelijkt deze methode omdat StorSimple kunt verhogen of cloudopslag verlagen om aan veranderende voldoen.

Als u het volume van het trapsgewijze voor archivering gegevens gebruikt, wijzigingen het selectievakje **dit volume voor minder veelgebruikte archivering gegevens gebruiken** in de grootte van het segment deduplication voor het volume van de aan 512 MB. Als u niet deze optie inschakelt dit, wordt het bijbehorende doorverbonden volume de grootte van een segment van 64 KB gebruikt. Deduplication segment groter kan het apparaat versnellen de overdracht van grote archivering gegevens in de cloud.

>[AZURE.NOTE] Archivering volumes die zijn gemaakt met een oude Update 2-versie van StorSimple meer geïmporteerd als doorverbonden met het selectievakje archivering geselecteerd.

### <a name="provisioned-capacity"></a>Ingerichte capaciteit

Raadpleeg de volgende tabel voor maximale ingerichte capaciteit voor elk type apparaat en volume. (Houd er rekening mee dat lokaal vastgemaakte volumes niet beschikbaar op een virtueel apparaat zijn.)

|             | Maximale doorverbonden volumegrootte | Maximum lokaal vastgemaakt volumegrootte |
|-------------|----------------------------|------------------------------------|
| **Fysieke apparaten** |       |       |
| 8100                 | 64 TB | 8 TB |
| 8600                 | 64 TB | 20 TB |
| **Virtuele apparaten**  |       |       |
| 8010                | 30 TB | N/B   |
| 8020               | 64 TB | N/B   |

## <a name="the-volumes-page"></a>De pagina Volumes

De pagina **Volumes** kunt u voor het beheren van de opslagruimte hoeveelheden die op de Microsoft Azure StorSimple apparaat gebruiken voor uw initiators (servers) is ingericht. De lijst van hoeveelheden wordt weergegeven op uw apparaat StorSimple.

 ![Volumes pagina](./media/storsimple-manage-volumes-u2/VolumePage.png)

Een volume bestaat uit een reeks kenmerken:

- **De naam van de volume** : een beschrijvende naam die moet uniek zijn en helpt Identificeer het volume. Deze naam wordt ook gebruikt in controlerapporten wanneer u op een specifieke volume filteren.

- **Status** : kan zijn online of offline. Als een volume offline is, is het niet zichtbaar voor initiators (servers) dat toegang hebben tot het volume gebruiken.

- **Capaciteit** – Hiermee geeft u de totale hoeveelheid gegevens die kunnen worden opgeslagen door het beginpunt (server). Lokaal vastgemaakt volumes volledig is ingericht en bevinden zich op het apparaat StorSimple. Gelaagde volumes dun is ingericht en de gegevens is deduplicated. Met dun ingerichte delen, uw apparaat niet vooraf fysiek opslagcapaciteit intern of toewijzen in de cloud op basis van de volumecapaciteit van de geconfigureerde. De capaciteit van het volume is toegewezen en verbruikte op aanvraag.

- **Type** , wordt aangegeven of het volume **Tiered** (de standaardinstelling) of **lokaal vastgemaakt**.

- **Back-up** – wordt aangegeven of een back-up standaardbeleid voor het volume bestaat.

- **Access** – Hiermee geeft u de initiators (servers) dat toegang hebben tot dit volume. Het volume zien initiators die geen deel uitmaken van access besturingselement record (ACR) die is gekoppeld aan het volume niet.

- **Controle** – Hiermee wordt het al dan niet een volume moet worden gecontroleerd. Een volume heeft monitoring standaard ingeschakeld wanneer deze is gemaakt. Controle wordt echter, om een volume-klonen worden uitgeschakeld. Volg de instructies in de [Monitor een volume](#monitor-a-volume)om in te schakelen voor een volume monitoring. 

Voer de stappen in deze zelfstudie wilt u de volgende taken uitvoeren:

- Een volume toevoegen 
- Een volume wijzigen 
- Het volumetype wijzigen
- Een volume verwijderen 
- Een volume offline te zetten 
- Monitor met een volume 

## <a name="add-a-volume"></a>Een volume toevoegen

U [een volume gemaakt](storsimple-deployment-walkthrough-u2.md#step-6-create-a-volume) tijdens de implementatie van uw oplossing StorSimple. Toevoegen van een volume is een soortgelijke procedure.

#### <a name="to-add-a-volume"></a>Een volume toevoegen

1. Selecteer het apparaat, dubbelklikt u erop en klik vervolgens op het tabblad **Volume Containers** op de pagina **apparaten** .

2. Selecteer een container volume in de lijst en dubbelklik erop voor toegang tot de hoeveelheden die is gekoppeld aan de container.

3. Klik op **toevoegen** aan de onderkant van de pagina. De toevoegen een volume-wizard wordt gestart.

     ![Volume-wizard Basisinstellingen toevoegen](./media/storsimple-manage-volumes-u2/TieredVolEx.png)

4. Ga als volgt te werk in de wizard een volume, klik onder **Basisinstellingen**toevoegen:

  1. Geef een **naam** voor het volume van de.
  2. Selecteer een **Type voor gebruik** in de vervolgkeuzelijst. Voor werkbelasting waarvoor gegevens moeten beschikbaar lokaal op het apparaat te allen tijde, selecteert u **Lokaal vastgemaakt**. Voor alle andere typen gegevens, selecteert u **Tiered**. (**Tiered** is de standaardinstelling).
  3. Als u **Tiered** in stap 2 hebt geselecteerd, kunt u het selectievakje **dit volume voor minder veelgebruikte archivering gegevens gebruiken** voor het configureren van een archivering volume selecteren.
  4. Voer de **Capaciteit deze is ingericht** voor het volume van de in GB of TB. Zie [Provisioned capaciteit](#provisioned-capacity) voor maximale grootte voor elk type apparaat en volume. Bekijk de **Beschikbare capaciteit** om te bepalen hoeveel opslagruimte op uw apparaat dat is wel beschikbaar is.

5. Klik op het pijlpictogram![Pijlpictogram](./media/storsimple-manage-volumes-u2/HCS_ArrowIcon.png). Als u een lokaal vastgemaakte volume configureert, ziet u het volgende bericht weergegeven.

    ![Volume type bericht wijzigen](./media/storsimple-manage-volumes-u2/LocalVolEx.png)
   
5. Klik op het pijlpictogram ![pijlpictogram](./media/storsimple-manage-volumes-u2/HCS_ArrowIcon.png)opnieuw om te gaan naar de pagina **Extra instellingen** .

    ![Volume wizard aanvullende instellingen toevoegen](./media/storsimple-manage-volumes-u2/AddVolume2.png)<br>

6. Klik onder **Aanvullende instellingen**, moet u een nieuwe access besturingselement record (ACR) toevoegen:
  
  1. Selecteer een access-record van het besturingselement (ACR) in de vervolgkeuzelijst. U kunt ook een nieuwe ACR toevoegen. ACRs bepalen welke hosts toegang heeft tot begint door de host IQN overeenkomen met die wordt vermeld in de record. Als u een ACR niet opgeeft, ziet u het volgende bericht weergegeven.

        ![ACR opgeven](./media/storsimple-manage-volumes-u2/SpecifyACR.png)

  2. Het is raadzaam dat u het selectievakje **inschakelen van een standaard back-up voor dit volume** selecteert.
  3. Klik op het pictogram controleren ![Pictogram controleren](./media/storsimple-manage-volumes-u2/HCS_CheckIcon.png) het volume te maken met de opgegeven instellingen.

Het volume van de nieuwe is nu klaar voor gebruik.

>[AZURE.NOTE] Als u een lokaal vastgemaakte volume maken en vervolgens een ander lokaal vastgemaakte volume maken direct achteraf, het volume maken taken opeenvolging uitgevoerd. De eerste taak voor het maken van de volume moet zijn voltooid voordat de volgende taak voor het maken van de volume kan.

## <a name="modify-a-volume"></a>Een volume wijzigen

Wijzig een volume wanneer u zich wilt uitvouwen of de hosts die toegang het volume tot te wijzigen.

> [AZURE.IMPORTANT] 
>
> - Als u de grootte van het volume op het apparaat wijzigt, moet de grootte van het volume op de host ook worden gewijzigd. 
> - De host aan de clientzijde stappen die hier worden beschreven, zijn geschikt voor Windows Server 2012 (2012R2). Procedures voor het Linux of andere besturingssystemen host zullen afwijken. Raadpleeg de instructies van uw host besturingssysteem wanneer u het volume op een host met een ander besturingssysteem wilt wijzigen. 

#### <a name="to-modify-a-volume"></a>Voor het wijzigen van een volume

1. Selecteer het apparaat, dubbelklikt u erop en klik vervolgens op het tabblad **Volume Containers** op de pagina **apparaten** .

2. Selecteer een container volume in de lijst en dubbelklik erop om de hoeveelheden die is gekoppeld aan de container weer te geven.

3. Selecteer een volume en onderaan op de pagina, klikt u op **wijzigen**. Hiermee start u de wizard van de volume wijzigen.

4. Klik in de wizard Modify volume onder **Basisinstellingen**kunt u het volgende doen:

  - De **naam**bewerken.
  - Het **Type voor gebruik** in converteren lokaal vastgemaakt doorverbonden naar of uit doorverbonden naar lokaal vastgemaakt (Zie [het volumetype wijzigen](#change-the-volume-type) voor meer informatie).
  - Vergroot de **capaciteit ingericht**. De **Capaciteit deze is ingericht** kan alleen worden verhoogd. Nadat deze is gemaakt, kunt u een volume niet verkleinen.

5. Klik onder **Aanvullende instellingen**kunt u de ACR, mits het volume offline is. Als het volume online is, moet u eerst offline halen. Raadpleeg de stappen in [een volume offline nemen](#take-a-volume-offline) voordat u de ACR wijzigen.

    > [AZURE.NOTE] U kunt de optie **een standaard back-up inschakelen** voor het volume niet wijzigen.

6. Sla de wijzigingen door te klikken op het pictogram controleren ![selectievakje-pictogram](./media/storsimple-manage-volumes-u2/HCS_CheckIcon.png). De portal van Azure klassieke weergegeven een bijwerken volume-bericht. Een bericht wordt weergegeven wanneer het volume is bijgewerkt.

7. Als u een volume zijn uitvouwen, voert u de volgende stappen op uw computer met Windows-host:

   1. Ga naar de **Computer Management** ->**schijf Management**.
   2. Met de rechtermuisknop op de **Schijf Management** en selecteer **Schijven**.
   3. Selecteer in de lijst met schijven, het volume die u hebt bijgewerkt, klik met de rechtermuisknop en selecteer vervolgens **Volume uitbreiden**. De wizard Volume uitbreiden wordt gestart. Klik op **volgende**.
   4. Voltooi de wizard, de standaardwaarden accepteren. Wanneer de wizard voltooid is, moet het volume de verbeterde grootte weergegeven.

    >[AZURE.NOTE] Als u een lokaal vastgemaakte volume uitvouwen en vouwt u vastgemaakt een ander lokaal volume direct achteraf, de taken van de uitbreiding volume opeenvolging uitvoeren. De eerste volume Uitbreidingstaak moet zijn voltooid voordat de volgende volume Uitbreidingstaak kan worden gestart.

![Video beschikbaar](./media/storsimple-manage-volumes-u2/Video_icon.png) **Video beschikbaar**

Klik op een video die laat hoe zien u een volume uitvouwen bekijken [hier](https://azure.microsoft.com/documentation/videos/expand-a-storsimple-volume/).

## <a name="change-the-volume-type"></a>Het volumetype wijzigen

U wijzigen van het volumetype uit doorverbonden naar lokaal vastgemaakt of van lokaal vastgemaakt tot doorverbonden. Deze conversie moet echter niet een veelgebruikte exemplaar. Enkele redenen voor het converteren van een volume dan zijn doorverbonden naar lokaal vastgemaakt:

- Lokale garanties met betrekking tot de beschikbaarheid van gegevens en prestaties
- Verwijderen van de cloud vertragingstijden en verbindingsproblemen met de cloud.

Dit zijn meestal kleine bestaand volume die u wilt openen, vaak. Een lokaal vastgemaakte volume is volledig ingericht wanneer deze is gemaakt. Als u een doorverbonden volume naar een lokaal vastgemaakte volume converteert, controleert StorSimple of dat er voldoende ruimte is op uw apparaat voordat u begint de conversie. Als u onvoldoende ruimte hebt, ontvangt u een fout en de bewerking wordt geannuleerd. 

> [AZURE.NOTE] Voordat u de conversie van een doorverbonden naar lokaal vastgemaakt, zorg dat u rekening houden met de ruimtevereisten van uw andere werkbelasting. 

U wilt misschien een lokaal vastgemaakte volume naar een doorverbonden volume wijzigen als u extra ruimte voor het inrichten van ander volume nodig. Wanneer u het volume lokaal vastgemaakte doorverbonden, de beschikbare capaciteit op het apparaat toeneemt door de grootte van de uitgebrachte capaciteit converteren. Als verbindingsproblemen voorkomen de conversie van een volume van de lokale type naar het type gelaagde dat, wordt het lokale volume eigenschappen van een doorverbonden volume vertonen totdat de conversie is voltooid. Dit komt omdat sommige gegevens in de cloud terechtgekomen mogelijk. Deze gemorst gegevens blijft lokale inneemt op het apparaat dat kan geen vrijmaken totdat de bewerking wordt gestart en worden voltooid.

>[AZURE.NOTE] Converteren van een volume, kunt u de enige tijd duren en u kunt een conversie niet annuleren nadat deze is gestart. Het volume blijft online tijdens de conversie, en back-ups die u kunt uitvoeren, maar u niet kunt uitvouwen of het volume herstellen terwijl de conversie plaatsvindt.  

Conversie van een doorverbonden naar een lokaal vastgemaakte volume kan Apparaatprestaties nadelig beïnvloeden. Daarnaast kunnen kunnen de volgende factoren het langer duren die nodig is voor het voltooien van de conversie:

- Er is onvoldoende bandbreedte.

- Er is geen huidige back-up.

Om te minimaliseren de effecten die deze factoren kunnen hebben:

- Controleer uw bandbreedte beleidsregels beperken en zorg ervoor dat er een speciale 40 Mbps bandbreedte beschikbaar is.
- De conversie rustige uren.
- Een cloud momentopname voordat u de conversie begint.

Als u meerdere volumes (ondersteunende verschillende werkbelastingen) converteren wilt, moet u het volumeconversie prioriteren zodat grotere prioriteit hoeveelheden eerst worden geconverteerd. Bijvoorbeeld, moet u volumes die als virtuele machines (VMs host) of volumes met SQL-werkbelastingen converteren voordat u volumes met werkbelasting in het bestand delen converteren.

#### <a name="to-change-the-volume-type"></a>Het volumetype wijzigen

1. Selecteer het apparaat, dubbelklikt u erop en klik vervolgens op het tabblad **Volume Containers** op de pagina **apparaten** .

2. Selecteer een container volume in de lijst en dubbelklik erop om de hoeveelheden die is gekoppeld aan de container weer te geven.

3. Selecteer een volume en onderaan op de pagina, klikt u op **wijzigen**. Hiermee start u de wizard van de volume wijzigen.

4. Klik op de pagina **Basisinstellingen** wijzigen het gebruikstype door te selecteren van het nieuwe type in de vervolgkeuzelijst **Gebruikstype** .

    - Als u het type naar **lokaal vastgemaakt**wijzigt, worden StorSimple moeten worden gecontroleerd of er voldoende capaciteit is.
    - Als u het type in **Tiered verandert** en dit volume worden gebruikt voor archivering gegevens, selecteert u het selectievakje **dit volume voor minder veelgebruikte archivering gegevens gebruiken** .

        ![Selectievakje archief](./media/storsimple-manage-volumes-u2/ModifyTieredVolEx.png)

5. Klik op het pijlpictogram ![pijlpictogram](./media/storsimple-manage-volumes-u2/HCS_ArrowIcon.png) om naar de pagina **Extra instellingen** te gaan. Als u een lokaal vastgemaakte volume configureert, wordt het volgende bericht weergegeven.

    ![Volume type bericht wijzigen](./media/storsimple-manage-volumes-u2/ModifyLocalVolEx.png)

6. Klik op het pijlpictogram ![pijlpictogram](./media/storsimple-manage-volumes-u2/HCS_ArrowIcon.png) opnieuw om door te gaan.

7. Klik op het pictogram controleren ![Pictogram controleren](./media/storsimple-manage-volumes-u2/HCS_CheckIcon.png) Start het conversieproces. De Azure-portal weergegeven een bijwerken volume-bericht. Een bericht wordt weergegeven wanneer het volume is bijgewerkt.

## <a name="take-a-volume-offline"></a>Een volume offline te zetten

Mogelijk moet u een volume offline te zetten wanneer u van plan bent om te wijzigen of verwijderen. Als een volume offline is, is deze niet beschikbaar voor alleen-lezen-toegang. U moet het volume offline nemen op de host en klik op het apparaat. 

#### <a name="to-take-a-volume-offline"></a>Naar een volume offline te zetten

1. Zorg ervoor dat het betreffende volume zich niet in gebruik voordat deze offline wordt gezet.

2. Het volume offline op de host eerste duren. Hierdoor wordt een mogelijke risico van beschadiging op het volume. Voor bepaalde stappen, raadpleegt u de instructies voor uw hostbesturingssysteem.

3. Nadat de host offline is, maakt u het volume op het apparaat offline door de volgende stappen uit te voeren:

  1. Selecteer het apparaat, dubbelklikt u erop en klik vervolgens op het tabblad **Volume Containers** op de pagina **apparaten** . Het tabblad **Volume Containers** lijsten in tabelvorm alle volume-containers die zijn gekoppeld aan het apparaat.
  2. Selecteer een container volume en klikt u erop om de lijst met alle de hoeveelheden binnen de container weer te geven.
  3. Selecteer een volume en klik op **offline halen**.
  4. Wanneer u hierom wordt gevraagd om bevestiging, klikt u op **Ja**. Het volume moet nu offline.

    Nadat u een volume offline is, wordt de optie **Weer Online** beschikbaar.

> [AZURE.NOTE] De opdracht **Offline nemen** wordt een aanvraag verzonden naar het apparaat naar het volume offline te zetten. Als hosts nog steeds het volume gebruikt, wordt het resultaat is verbroken verbindingen, maar niet het volume offline halen, mislukt. 

## <a name="delete-a-volume"></a>Een volume verwijderen

> [AZURE.IMPORTANT] Alleen als het offline is, kunt u een volume verwijderen.

Voer de volgende stappen uit als u wilt verwijderen van een volume.

#### <a name="to-delete-a-volume"></a>Een volume verwijderen

1. Selecteer het apparaat, dubbelklikt u erop en klik vervolgens op het tabblad **Volume Containers** op de pagina **apparaten** .

2. Selecteer de container volume waarop het volume die u wilt verwijderen. Klik op de container volume voor toegang tot de pagina **Volumes** .

3. Alle de hoeveelheden die is gekoppeld aan deze container worden weergegeven in een tabelvormige indeling. Controleer de status van het volume die u wilt verwijderen. Als het volume die u wilt verwijderen niet offline is, offline te zetten deze eerst de stappen in [een volume offline nemen](#take-a-volume-offline).

4. Nadat het volume offline is, klikt u op **verwijderen** onder aan de pagina.

5. Wanneer u hierom wordt gevraagd om bevestiging, klikt u op **Ja**. Het volume worden nu verwijderd en de pagina **Volumes** de bijgewerkte lijst van hoeveelheden binnen de container wordt weergegeven.

    >[AZURE.NOTE]Als u een lokaal vastgemaakte volume verwijdert, worden de beschikbare schijfruimte voor nieuwe volumes mogelijk niet direct bijgewerkt. De lokale schijfruimte door de Service StorSimple Manager regelmatig bijgewerkt. Het is raadzaam wachten op een paar minuten voordat u probeert te maken van het nieuwe volume.<br> Ook als u een lokaal vastgemaakte volume verwijderen en verwijder vervolgens een ander lokaal vastgemaakt volume onmiddellijk daarna de volume-verwijdering taken uitgevoerd opeenvolging. De eerste taak voor volume moet zijn voltooid voordat de volgende taak voor volume kan.
 
## <a name="monitor-a-volume"></a>Monitor met een volume

Controle, kunt u voor het verzamelen van ik/O-gegevens voor een volume. Cmdlets voor controle is voor de eerste 32 hoeveelheden die u maakt standaard ingeschakeld. Cmdlets voor controle van extra hoeveelheden is standaard uitgeschakeld. Cmdlets voor controle van gekloonde hoeveelheden is ook standaard uitgeschakeld.

De volgende stappen als u wilt in- of uitschakelen voor een volume cmdlets voor controle uitvoeren.

#### <a name="to-enable-or-disable-volume-monitoring"></a>-Of uitschakelen volume bewaken

1. Selecteer het apparaat, dubbelklikt u erop en klik vervolgens op het tabblad **Volume Containers** op de pagina **apparaten** .

2. Selecteer de volume-container waarin het volume zich bevindt en klik vervolgens op de container volume voor toegang tot de pagina **Volumes** .

3. Alle de hoeveelheden die is gekoppeld aan deze container worden weergegeven in de weergave in tabelvorm. Klik op en selecteer het volume of het volume klonen.

4. Onderaan op de pagina, klikt u op **wijzigen**.

5. Selecteer in de wizard Volume wijzigen, klikt u onder **Basisinstellingen** **in** - of **uitschakelen** in de vervolgkeuzelijst **controle** .

## <a name="next-steps"></a>Volgende stappen

- Leer hoe u [een volume StorSimple klonen](storsimple-clone-volume.md).

- Informatie over het [gebruik van de StorSimple Manager-service voor het beheren van uw apparaat StorSimple](storsimple-manager-service-administration.md).

 
