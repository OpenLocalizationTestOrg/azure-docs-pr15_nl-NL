<properties
   pageTitle="Uw apparaat StorSimple 8600 installeren | Microsoft Azure"
   description="Wordt beschreven hoe uitpakken, rek mountains en uw apparaat StorSimple 8600 kabel voordat u implementeert en configureert de software."
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
   ms.date="10/24/2016"
   ms.author="alkohli" />

# <a name="unpack-rack-mount-and-cable-your-storsimple-8600-device"></a>Uitpakken, rek-koppelen, en uw apparaat StorSimple 8600 kabel

## <a name="overview"></a>Overzicht
Uw Microsoft Azure StorSimple 8600 is een dubbele insluiting apparaat en bestaat uit een primaire en een insluiting EBOD. Deze zelfstudie wordt uitgelegd hoe u uitpakken, rek en kabel de 8600 StorSimple apparaat hardware voordat u de software StorSimple configureren.

## <a name="unpack-your-storsimple-8600-device"></a>Uw apparaat StorSimple 8600 uitpakken

De volgende stappen bieden wissen, gedetailleerde instructies voor het uitpakken van uw StorSimple 8600 opslag-apparaat. Dit apparaat wordt geleverd bij twee vakken, één telefoonnummer voor de primaire insluiting en een andere voor de EBOD insluiting. Deze twee vakken worden vervolgens in één vak geplaatst.

### <a name="prepare-to-unpack-your-device"></a>De voorbereiding uitpakken van uw apparaat

Lees de volgende informatie voordat u uw apparaat uitpakken.


![Waarschuwingspictogram](./media/storsimple-safety/IC740879.png)![dik gewicht pictogram](./media/storsimple-8600-hardware-installation/HCS_HeavyWeight_Icon.png) **Waarschuwing!**

1. Zorg ervoor dat er twee personen die beschikbaar zijn voor het beheren van het gewicht van het apparaat als u deze handmatig worden verwerkt. Een volledig geconfigureerde insluiting kan maximaal 32 kg (70 kg.) wegen.
1. Plaats het vak op een plat vlak niveau.

De volgende stappen voor het uitpakken van uw apparaat vervolgens voltooien.

#### <a name="to-unpack-your-device"></a>Uw apparaat uitpakken

1. Het vak en de verpakking schuim voor crushes, delen, water schade of andere duidelijke schade controleren. Als het vak of de verpakking ernstige is beschadigd, kunnen het vak niet worden geopend. Neem [contact op met Microsoft ondersteuning](storsimple-contact-microsoft-support.md) om te beoordelen of het apparaat in goede staat is.

2. Open het outer en maakt u van de twee vakken die overeenkomt met primaire en EBOD bijlagen. U kunt nu de primaire en EBOD bijlagen uitpakken. De volgende afbeelding ziet de uitgepakte weergave van een van de bijlagen.

    ![Uw opslagapparaat uitpakken](./media/storsimple-8600-hardware-installation/HCSUnpackyour4Udevice.png)

    **Uitgepakte weergave van uw opslagapparaat**

     Label | Beschrijving
     ----- | -------------
     1     | Vak verpakking
     2     | SA's kabels (in Bureau-accessoires en kabels systeemvak)
     3     | Klik onder schuim
     4     | Apparaat
     5     | Bovenste schuim
     6     | Accessoire vak

3. Na het uitpakken van de twee vakken, zorg dat u hebt:

  - 1 primaire insluiting (de primaire insluiting en EBOD insluiting zijn in twee afzonderlijke vakken)
  - 1 EBOD insluiting
  - 4 stroomsnoeren, 2 in elk vak
  - 2 SA's kabels (aan de primaire insluiting verbinden met EBOD insluiting)
  - 1 snijden Ethernet-kabel
  - 2 seriële console kabels
  - 1 seriële-USB-conversieprogramma voor seriële toegang
  - 4 QSFP-naar-SFP + adapters voor gebruik met 10 GbE netwerkinterfaces
  - 2 rek mountains Kit (4 kant rails met hardware, 2 voor de primaire insluiting en EBOD insluiting koppelen), 1 in elk vak
  - Aan de slag te gaan documentatie

    Als u een van de bovenstaande, items niet hebt ontvangen [contact opnemen met Microsoft ondersteuning](storsimple-contact-microsoft-support.md).  

De volgende stap is rek-mountains uw apparaat.

## <a name="rack-mount-your-storsimple-8600-device"></a>Uw apparaat StorSimple 8600 rek-koppeling

Volg de stappen om uw StorSimple 8600 opslag-apparaat installeren in een standaard 19-inch rek met voorste en achterste berichten. Dit apparaat wordt geleverd met twee gedeelten: een primaire insluiting en een insluiting EBOD. Deze moeten worden rek-gekoppeld.

De installatie bestaat uit meerdere stappen voor elk van die wordt beschreven in de volgende procedures uit.

> [AZURE.IMPORTANT]
StorSimple apparaten moet rek bevestigde voor een goede werking.



### <a name="site-preparation"></a>Voorbereiding van de site

De bijlagen moeten worden geïnstalleerd in een standaard 19-inch rek met zowel voorste en achterste berichten. Gebruik de volgende procedure rek-installatie voorbereiden.

#### <a name="to-prepare-the-site-for-rack-installation"></a>Voor het voorbereiden van de site voor rek-installatie

1. Zorg ervoor dat de primaire en EBOD bijlagen zijn verwerken veilig op een oppervlak plat, stabiele en niveau werk (of soortgelijke).

2. Controleer of de site waar u van plan bent om in te stellen heeft standaard AC power uit een onafhankelijke bron of een rek Power verdeling eenheid (PDU) met een noodvoeding (UPS).

3. Zorg ervoor dat één 4U (2 X 2U) slot is beschikbaar op het rek waarin u van plan bent om te koppelen van de bijlagen.

![Waarschuwingspictogram](./media/storsimple-safety/IC740879.png)![dik gewicht pictogram](./media/storsimple-8600-hardware-installation/HCS_HeavyWeight_Icon.png) **Waarschuwing!**

 Zorg ervoor dat er twee personen die beschikbaar zijn voor het beheren van het gewicht als u de Apparaatinstelling handmatig verwerkt. Een volledig geconfigureerde insluiting kan maximaal 32 kg (70 kg.) wegen.

### <a name="rack-prerequisites"></a>Rek vereisten

De bijlagen zijn ontworpen voor installatie in een standaard 19-inch rek cab met:

- Minimale diepteas van 27.84 inch van rek te boeken
- Maximale dikte van de 32 kg voor het apparaat
- Maximaal achterste druk van 5 Pascal (0,5 mm water toelichtingen)

### <a name="rack-mounting-rail-kit"></a>Rek-koppelen railkit

Een reeks koppelen rails krijgt voor gebruik met de 19-inch rek-kluis. De spoorstaven is getest voor het verwerken van het gewicht maximale insluiting. Deze rails kan ook installatie van meerdere bijlagen zonder kwaliteitsverlies ruimte binnen het rek. Installeer eerst de EBOD insluiting.

#### <a name="to-install-the-ebod-enclosure-on-the-rails"></a>De ruimte EBOD installeren op de spoorstaven

2. Deze stap alleen uitvoeren als binnenste rails niet op uw apparaat zijn geïnstalleerd. Meestal zijn de binnenste spoorstaven in de fabriek geïnstalleerd. Als rails niet zijn geïnstalleerd, installeert u de dia's links-spoor en rechts-rail klikt u vervolgens op de zijkanten van het chassis insluiting. Ze toevoegen met van zes metrische bouten aan elke kant. Om u te helpen met de afdrukstand, de rail dia's zijn gemarkeerd **LH – voorgrond** en **RH – voorgrond**en de zijde die is aangebracht vanaf de achterzijde van de ruimte heeft een Tapse einde.

    ![Rail dia's koppelen aan insluiting chassis](./media/storsimple-8600-hardware-installation/HCSAttachingRailSlidestoEnclosureChassis.png)

    **Rail dia's koppelen aan de zijkanten van de ruimte**

    Label | Beschrijving
    ----- | -----------
    1     | M 3 x 4 knop hoofd bouten
    2     | Chassis dia 's

3. De rail links en rechts rail stroombaan toevoegen aan de rek cab verticale leden. De haakjes zijn gemarkeerd **LH** **RH**en **deze kant omhoog** om u te helpen u bij het juiste afdrukstand.

4. Zoek de pennen rail aan de voor- en achterzijde van de vergadering rail. De rail om passend te maken tussen de rek-berichten en de pennen invoegen in de voorste en achterste rek-bericht verticale lid gaten uitbreiden. Zorg ervoor dat de vergadering rail niveau.

5. Secure de constructie rail op het rek verticale leden met behulp van twee van de metrische bouten opgegeven. Gebruik een installatie op de voorgrond en een aan de achterzijde.

6. Herhaal deze stappen voor de andere rail-vergadering.

     ![Rail dia's koppelen aan rek cab](./media/storsimple-8600-hardware-installation/HCSAttachingRailSlidestoRackCabinet.png)

    **Rail stroombaan koppelen aan het rek**

     Label | Beschrijving
     ----- | -----------
     1     | Installatie clamping
     2     | Vierkant-gat voorste rek bericht installatie
     3     | Voorste rail locatie pincodes links
     4     | Installatie clamping
     5     | Links achterste rail locatie pincodes

### <a name="mounting-the-ebod-enclosure-in-the-rack"></a>De ruimte EBOD in het rek koppelen

Met de rackrails die net zijn geïnstalleerd, voer de volgende stappen uit om te koppelen van de ruimte EBOD in het rek uit.

#### <a name="to-mount-the-ebod-enclosure"></a>De ruimte EBOD koppelen

1. Met een assistent, de insluiting til en uitlijnen met de rackrails.

2. Zorgvuldig de ruimte in de spoorstaven invoegen, en klik vervolgens doorgeven volledig in het rek cab.

    ![Apparaat invoegen in het rek](./media/storsimple-8600-hardware-installation/HCSInsertingDeviceintheRack.png)

    **De ruimte in het rek koppelen**

3. Verwijder de linker- en rechts voorste flens caps door te trekken van de gratis caps. De caps flens uitlijnen gewoon naar de flenzen.

4. De ruimte in het rek beveiligen door het installeren van één meegeleverde kruiskopschroevendraaier hoofd-installatie via elke flens, de linker- en.

4. Installeer de caps flens door te drukken ze naar de gewenste positie en deze naar de juiste plaats uitlijnen.

     ![Flens caps installeren](./media/storsimple-8600-hardware-installation/HCSInstallingFlangeCaps.png)

    **Installatie van de flens caps**

     Label | Beschrijving
     ----- | -----------
     1     | Insluiting fixeerpunt installatie


### <a name="mounting-the-primary-enclosure-in-the-rack"></a>De primaire ruimte in het rek koppelen

Nadat u klaar bent met het koppelen van de ruimte EBOD, moet u de primaire insluiting volg daarbij dezelfde stappen koppelen.

> [AZURE.NOTE]
>
> - Het is mogelijk dat een paar lege plaatsen in het rek tussen de primaire insluiting en de ruimte EBOD.
> - Gebruik de meegeleverde 2m SA's-kabel aan de primaire insluiting verbinden met de EBOD insluiting.
> - Er zijn geen beperkingen bij de relatieve positie van het hoofd eenheid naar de eenheid EBOD. Daarom de primaire insluiting kan worden geplaatst in de bovenste slot en de onderstaande EBOD-ruimte, of vice versa.

De volgende stap is kabel van uw apparaat gebruiken voor power-, netwerk- en seriële toegang.

## <a name="cable-your-storsimple-8600-device"></a>Uw apparaat StorSimple 8600 kabel

De volgende procedures wordt uitgelegd hoe u uw apparaat StorSimple 8600 voor power-, netwerk- en seriële verbindingen een kabel.

### <a name="prerequisites"></a>Vereisten voor

Voordat u begint met de kabel van uw apparaat, moet u:

- Uw primaire insluiting en de ruimte EBOD volledig uitgepakt
- 4 voeding kon (2 elke voor de primaire en de ruimte EBOD) die bij uw apparaat is geleverd
- 2 SA's kabels opgegeven bij het apparaat naar de insluiting EBOD verbinden met de primaire insluiting
- Toegang tot 2 Power verdeling eenheden (PDU's) (aanbevolen)
- Netwerkkabels
- Seriële kabels opgegeven
- Seriële-USB-conversieprogramma met het juiste stuurprogramma geïnstalleerd op uw PC (indien nodig)
- 4 QSFP verstrekt-naar-SFP + adapters voor gebruik met 10 GbE netwerkinterfaces
- [Hardware ondersteund voor de 10 GbE netwerkinterfaces op uw apparaat StorSimple](storsimple-supported-hardware-for-10-gbe-network-interfaces.md)

### <a name="sas-and-power-cabling"></a>SA's en Power-kabel

Het apparaat heeft een primaire insluiting zowel een insluiting EBOD. U moet hiervoor de eenheden die aan elkaar worden bekabelde voor seriële bijgevoegd SCSI (SA's)-connectiviteit en power.

Wanneer u dit apparaat voor het eerst instelt, voer de stappen voor het SA's kabel eerst en vult u de stappen voor het power kabel.

[AZURE.INCLUDE [storsimple-cable-8600-for-SAS](../../includes/storsimple-sas-cable-8600.md)]

[AZURE.INCLUDE [storsimple-cable-8600-for-power](../../includes/storsimple-cable-8600-for-power.md)]

### <a name="network-cabling"></a>Netwerkkabels

Uw apparaat is in een actief stand-by-configuratie: op elk gewenst één controllermodule actief is en stand-by verwerking van alle schijf en netwerk bewerkingen terwijl de andere controllermodule is. Als een controller een fout optreedt, wordt de stand-by controller onmiddellijk wordt geactiveerd en alle schijf en netwerken bewerkingen blijft.

Als u wilt deze failover overtollige controller wordt ondersteund, moet u het netwerk van uw apparaat kabel, zoals wordt weergegeven in de volgende stappen uit.

#### <a name="to-cable-for-network-connection"></a>Naar de kabel voor netwerkverbinding

1. Het apparaat heeft zes netwerkinterfaces op elke controller: vier 1 GB/s en twee 10 GB/s Ethernet-poorten. Raadpleeg de volgende afbeelding om aan te geven van de gegevenspoorten op de backplane van uw apparaat.

     ![Backplane van 8600-apparaat](./media/storsimple-8600-hardware-installation/HCSBackplaneof2UDevicewithPortsLabeled.jpg)

    **Achterzijde van uw apparaat met de gegevenspoorten**

     Label   | Beschrijving
     ------- | -----------
     0,1,4,5 |  1 via interfaces zoals GbE netwerk
     2,3     | 10 GbE netwerkinterfaces
     6       | Seriële poorten



1. Zie het volgende diagram voor netwerkkabels. (De minimale netwerkconfiguratie door ononderbroken blauwe lijn wordt weergegeven. Voor beschikbaarheid en prestaties, wordt aanvullende configuratie vereist weergegeven als stippellijnen.)

![Uw apparaat 4U voor netwerk-kabel](./media/storsimple-8600-hardware-installation/HCSCableYour4UDeviceforNetwork.png)

**Netwerkkabels voor uw apparaat**

Label | Beschrijving
----- | -----------
A    | LAN met internettoegang
B    | Controller 0
C    | PCM 0
D    | Controller 1
E    | PCM 1
F    | EBOD controller 0
G    | EBOD controller 1
H, IK  | Hosts (bijvoorbeeld bestandsservers)
0 tot en met 5  | Netwerkinterfaces
6    | Primaire insluiting
7    | EBOD insluiting

Wanneer het apparaat kabel, wordt de minimale configuratie vereist:


- Ten minste twee netwerkinterfaces verbonden op elke controller met één voor cloudtoegang en één voor iSCSI. De gegevens 0 poort is automatisch ingeschakeld en geconfigureerd via de seriële console van het apparaat. Uitzondering van de gegevens 0 moet een andere gegevenspoort ook worden geconfigureerd door de Azure klassieke portal. In dit geval 0-gegevens koppelen aan de primaire LAN (netwerk met internettoegang) het poortnummer. De andere gegevenspoorten kunnen niet worden verbonden met SAN/iSCSI LAN (VLAN) segment van het netwerk, afhankelijk van de juiste rol.

- Identieke interfaces op elke controller verbonden met hetzelfde netwerk zodat deze zeker beschikbaar als een controller storing. Bijvoorbeeld als u besluit om gegevens 0 en 3 van de gegevens voor een van de verbinding, moet u verbinding maken met de bijbehorende gegevens 0 en 3 voor gegevens op de andere controller uit.

Houd er rekening mee voor beschikbaarheid en prestaties:


- Indien mogelijk, een paar van netwerkinterface voor cloudtoegang (1 GbE) en configureren nog een paar voor iSCSI (10 GbE aanbevolen) op elke controller.

- Indien mogelijk, sluit u netwerkinterfaces op elke controller en twee verschillende schakelopties zodat deze zeker beschikbaar ten opzichte van een schakeloptie is mislukt. De afbeelding ziet u de twee 10 GbE netwerk interfaces, gegevens 2 en 3 voor gegevens, vanuit elk controller verbonden met twee verschillende schakelopties. Voor meer informatie raadpleegt u het **Netwerkinterfaces** onder de [beschikbaarheid vereisten voor uw apparaat StorSimple](storsimple-system-requirements.md#high-availability-requirements-for-storsimple).

>[AZURE.NOTE] Als SFP + plaatsen met uw 10 GbE netwerk-interfaces, gebruikt u de meegeleverde QSFP-SFP + adapters. Ga naar [ondersteunde hardware voor de 10 GbE netwerkinterfaces op uw apparaat StorSimple](storsimple-supported-hardware-for-10-gbe-network-interfaces.md)voor meer informatie.

### <a name="serial-port-cabling"></a>Seriële poort kabel

De volgende stappen om de seriële poort kabel uitvoeren.

#### <a name="to-cable-for-serial-connection"></a>Naar de kabel voor seriële verbinding

1. Het apparaat heeft een seriële poort op elke controller die wordt aangegeven door een pictogram gereedschap. Als u wilt zoeken naar de seriële poorten, raadpleegt u de afbeelding waarin de gegevens poorten op de achtergrond van uw apparaat.

2. Geef aan dat de actieve controller op uw apparaat backplane. Een knipperende blauwe LED wordt aangegeven dat de controller actief is.

3. Gebruik de meegeleverde seriële kabel (indien nodig, de USB-serieel conversieprogramma voor uw laptop), en uw computer (met terminalemulatie bij het apparaat) of console verbinden met de seriële poort van de actieve controller.

4. De seriële-USB-stuurprogramma's (geleverd bij het apparaat) op uw computer installeren.

5. De seriële verbinding instellen als volgt:
   - 115.200 baud
   - van 8-gegevensbits
   - 1 stop bits
   - Geen overeenkomst
   - Stroom besturingselement is ingesteld op **geen**

6. Controleer of de verbinding functioneert door te drukken op de console op Enter. Een seriële console-menu moet worden weergegeven.

> [AZURE.NOTE] **Lights-Out Management:** Zorg ervoor dat de seriële verbindingen beide controller altijd zijn verbonden met een seriële console-switch of een vergelijkbare postvakken van apparatuur als het apparaat is geïnstalleerd in de computerruimte van een met beperkte toegang of in een externe datacenter. In deze sectie geeft out-van-band-afstandsbediening en ondersteuning voor bewerkingen voor het geval netwerk-onderbreking of onverwachte fouten.

U hebt voltooid kabel van uw apparaat gebruiken voor power netwerktoegang en seriële verbinding. De volgende stap is voor het configureren van de software op uw apparaat.

## <a name="next-steps"></a>Volgende stappen

U bent nu klaar om te [implementeren en configureren van uw on-premises implementatie StorSimple-apparaat](storsimple-deployment-walkthrough-u2.md).
