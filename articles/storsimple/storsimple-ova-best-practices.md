<properties 
   pageTitle="Aanbevolen procedures voor StorSimple virtuele matrix | Microsoft Azure"
   description="Beschrijft de aanbevolen procedures voor implementatie en het beheer van de virtuele StorSimple-matrix."
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
   ms.date="10/18/2016"
   ms.author="alkohli" />

# <a name="storsimple-virtual-array-best-practices"></a>Aanbevolen procedures voor StorSimple virtuele matrix

## <a name="overview"></a>Overzicht

Microsoft Azure StorSimple virtuele matrix is een geïntegreerde opslag-oplossing die worden beheerd opslagtaken tussen een on-premises implementatie van een virtueel apparaat uitgevoerd in een hypervisor en Microsoft Azure-cloudopslag. StorSimple virtuele matrix is een efficiënte, efficiënt alternatief voor de fysieke 8000 reeks-matrix. De virtuele matrix kan worden uitgevoerd op de infrastructuur van uw bestaande hypervisor, kunt u zowel de iSCSI-bestanden en het SMB-protocollen en is zeer geschikt voor scenario's voor office remote office/tak. Ga naar [Microsoft Azure StorSimple overzicht](https://www.microsoft.com/en-us/server-cloud/products/storsimple/overview.aspx)voor meer informatie over de StorSimple-oplossingen.

In dit artikel worden de aanbevolen procedures geïmplementeerd tijdens de eerste configuratie, implementatie en beheer van de virtuele StorSimple-matrix. Deze aanbevolen procedures bieden gevalideerde richtlijnen voor het instellen en beheren van uw virtuele matrix. In dit artikel is gericht op de IT-beheerders die implementeren en beheren van de virtuele matrices in hun datacenters.

U wordt aangeraden periodieke evaluatie van de aanbevolen procedures om ervoor te zorgen uw apparaat, dat zich nog in naleving wanneer wijzigingen worden aangebracht in de stroom instelling of -bewerking. U moet eventuele problemen optreden tijdens de uitvoering van deze aanbevolen procedures op uw virtuele matrix, [contact opnemen met Microsoft ondersteuning](storsimple-contact-microsoft-support.md) voor hulp.

## <a name="configuration-best-practices"></a>Aanbevolen procedures voor configuratie 

Deze aanbevolen procedures duidelijk de richtlijnen die nodig hebt bij de eerste configuratie en de implementatie van de virtuele matrices te volgen. Deze aanbevolen procedures bevatten die betrekking hebben op de inrichting van de virtuele machine, groepsbeleidsinstellingen, formaat wijzigen, de netwerken instellen opslag-accounts configureren en waarden voor aandelen en volumes voor de virtuele matrix maken. 

### <a name="provisioning"></a>Inrichten 

StorSimple virtuele matrix is een virtuele machine (VM) deze is ingericht op de hypervisor (Hyper-V of VMware) van de hostserver. Zorg ervoor dat uw host mogen opslagquota voldoende bronnen is bij de inrichting van de virtuele machine. Ga naar de [minimale resourcevereisten](storsimple-ova-deploy2-provision-hyperv.md#step-1-ensure-that-the-host-system-meets-minimum-virtual-device-requirements) voor het inrichten van een matrix voor meer informatie. 

De volgende aanbevolen procedures implementeren bij de inrichting van de virtuele matrix:


|                        | Hyper-V                                                                                                                                        | VMware                                                                                                               |
|------------------------|------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------|
| **VM type**   | **Generatie 2** VM voor gebruik met Windows Server 2012 of hoger en een afbeelding van de *.vhdx* . <br></br> **Generatie 1** VM voor gebruik met een Windows Server 2008 of hoger en een afbeelding van de *VHD* .                                                                                                              | VM versie 8-11 gebruiken wanneer u een afbeelding van de *.vmdk* gebruikt.                                                                      |
| **Geheugentype**            | Als de **statische geheugen**configureren. <br></br> Gebruik niet de optie voor **dynamisch geheugen** .            |                                                    |
| **Gegevenstype Schijfopruiming**         | Inrichten als **dynamisch gegevensniveaus uitvouwen**.<br></br> **Vaste grootte** duurt lang. <br></br> Gebruik niet de optie **differentiëren** .                                                                                                                   | Gebruik de optie **dunne inrichten** .                                                                                      |
| **Wijziging van gegevens Schijfopruiming** | Uitbreiding of verkleinen is niet toegestaan. Een poging hiertoe leidt tot het verlies van de lokale gegevens op apparaat.                       | Uitbreiding of verkleinen is niet toegestaan. Een poging hiertoe leidt tot het verlies van de lokale gegevens op apparaat. |

### <a name="sizing"></a>Formaatgrepen

Wanneer u het formaat van uw StorSimple virtuele matrix, de volgende factoren overwegen:

- Lokale reservering voor volumes of waarden voor aandelen. Ongeveer 12% van de ruimte is gereserveerd op de lokale laag voor elk ingerichte doorverbonden volume of delen. 10% van de ruimte is ongeveer ook gereserveerd voor een lokaal vastgemaakte volume voor bestandssysteem.
- Momentopname belasting. Ongeveer is 15% ruimte op de lokale laag gereserveerd voor momentopnamen.
- Behoefte Hiermee herstelt. Als herstellen als een nieuwe bewerking doen, moet de benodigde ruimte voor herstellen hoekformaatgreep administratief. Terugzetten is gereed om te een delen of het volume van dezelfde grootte of groter.
- Buffer moet worden toegewezen voor een onverwachte groei.

Op basis van de voorgaande factoren, kunnen de vereisten van de formaatgrepen worden weergegeven door de volgende vergelijking:

`Total usable local disk size = (Total provisioned locally pinned volume/share size including space for file system) + (Max (local reservation for a volume/share) for all tiered volumes/share) + (Local reservation for all tiered volumes/shares)`

`Data disk size = Total usable local disk size + Snapshot overhead + buffer for unexpected growth or new share or volume`


De volgende voorbeelden laten zien hoe u kunt het formaat van een virtuele matrix op basis van uw vereisten.

#### <a name="example-1"></a>Voorbeeld 1:
Klik op uw virtuele matrix, die u wilt kunnen 

- inrichten van een 2 TB doorverbonden volume of delen.
- inrichten van een 1 TB doorverbonden volume of delen.
- inrichten van een 300 GB van lokaal vastgemaakte volume of delen.


Laat ons bereken voor de voorgaande volumes of waarden voor aandelen, de ruimtevereisten op de lokale laag. 

Eerst voor elk doorverbonden volume-aandeel zou lokale reservering gelijk is aan 12% van de grootte van het volume/delen. Voor het lokaal vastgemaakte volume/delen is lokale reservering 10% van de grootte lokaal vastgemaakte volume/delen (naast de ingerichte grootte). In dit voorbeeld die u nodig hebt

- Lokale reservering 240 GB (voor een 2 TB doorverbonden volume/delen)
- Lokale reservering 120 GB (voor een 1 TB doorverbonden volume/delen)
- 330 GB voor lokaal vastgemaakte volume of delen (10% van lokale reservering toevoegen aan de grootte van 300 GB deze is ingericht)

De totale vereiste ruimte op de lokale laag dusverre is: 240 GB + 120 GB + 330 GB = 690 GB.

Tweede, moeten we ten minste hoeveelheid ruimte op de lokale laag als de grootste één reservering. Deze extra bedrag wordt gebruikt voor het geval u wilt terugzetten uit een momentopname van de cloud. In dit voorbeeld wordt de grootste lokale reservering 330 GB (inclusief reservering voor bestandssysteem), zodat u dat bij de 690 GB: 690 GB + 330 GB = 1020 GB toevoegen.
Als we de volgende aanvullende Hiermee herstelt hebt uitgevoerd, kunnen we altijd de ruimte uit het vorige terugzetten vrijgemaakt.

We nodig derde, 15% van de totale lokale ruimte dusverre voor de opslag van lokale momentopnamen, zodat alleen 85% van deze beschikbaar is. In dit voorbeeld dat is rond 1020 GB = 0.85&ast;ingerichte gegevens schijf TB. Ja, de gegevensschijf ingerichte zou (1020&ast;(1/0.85)) = 1200 GB = 1,20 TB ~ 1,25 TB (afronden naar het dichtstbijzijnde kwartiel)

Waarbij in onverwachte groei en nieuwe Hiermee herstelt, moet u een lokale schijf van rond inrichten 1,25-1,5 TB.

> [AZURE.NOTE] Ook aangeraden dat de lokale schijf dun is ingericht. Deze aanbeveling is omdat de ruimte herstellen is alleen nodig wanneer u gegevens wilt terugzetten die ouder is dan vijf dagen. Itemniveau herstel kunt u gegevens voor de laatste vijf dagen herstellen zonder de extra ruimte voor herstellen.

#### <a name="example-2"></a>Voorbeeld 2: 
Klik op uw virtuele matrix, die u wilt kunnen 

- inrichten van een 2 TB volume doorverbonden
- het volume van een 300 GB lokaal vastgemaakt inrichten

Op basis van 12% van lokale ruimte reservering voor trapsgewijze volumes/waarden voor aandelen en 10% voor lokaal vastgemaakte volumes/waarden voor aandelen, die we nodig

- Lokale reservering 240 GB (voor 2 TB doorverbonden volume/delen)
- 330 GB voor lokaal vastgemaakte volume of delen (10% van lokale reservering toevoegen aan de ruimte 300 GB deze is ingericht)

De totale ruimte nodig op de lokale laag is: 240 GB + 330 GB = 570 GB

De minimale lokale benodigde ruimte voor herstellen is 330 GB. 

15% van de totale schijf wordt gebruikt voor de opslag van momentopnamen, zodat alleen 0.85 beschikbaar is. Zo is, de grootte van de schijf is (900&ast;(1/0.85)) = 1,06 TB ~ 1,25 TB (afronden naar het dichtstbijzijnde kwartiel) 

Waarbij u rekening houdt een onverwachte groei, kunt u een lokale schijf 1,25-1,5 TB inrichten.


### <a name="group-policy"></a>Groepsbeleid

Groepsbeleid is een infrastructuur waarmee u kunt implementeren specifieke configuraties voor gebruikers en computers. Groepsbeleidsinstellingen zijn opgenomen in Groepsbeleid-objecten (GPO's), die zijn gekoppeld aan de volgende Active Directory Domain Services (AD DS)-containers: sites, domeinen of organisatie (eenheden). 

Als uw virtuele matrix een domein behoren is, kunnen GPO's worden toegepast. Deze GPO's kunnen toepassingen, zoals de antivirussoftware die de werking van de StorSimple virtuele matrix kan een negatieve invloed installeren.

Daarom, raden we u:

-   Zorg ervoor dat uw virtuele matrix zich in een eigen organisatie-eenheid voor Active Directory. 

-   Zorg ervoor dat er geen GPO (GPO's) worden toegepast op uw virtuele matrix. U kunt overname om ervoor te zorgen dat de virtuele matrix (onderliggende knooppunten) niet automatisch een GPO's van de bovenliggende neemt blokkeren. Ga naar [overname blokkeren](https://technet.microsoft.com/library/cc731076.aspx)voor meer informatie.


### <a name="networking"></a>Netwerken

De configuratie van het netwerk voor uw virtuele matrix vindt plaats via de lokale web UI. Een virtueel netwerk-interface is ingeschakeld door de hypervisor waarin de virtuele matrix is ingericht. Gebruik de pagina [Netwerkinstellingen](storsimple-ova-deploy3-fs-setup.md) voor het configureren van de virtuele netwerk interface IP-adres, subnet en gateway.  U kunt ook de primaire en secundaire DNS-server, tijdinstellingen en optioneel proxy-instellingen configureren voor uw apparaat. Grootste deel van de netwerkconfiguratie is een eenmalige instelling. Bekijk de [StorSimple netwerken vereisten](storsimple-ova-system-requirements.md#networking-requirements) vóór het implementeren van de virtuele matrix.

Wanneer u uw virtuele matrix implementeert, raden we u aan dat u deze aanbevolen procedures volgen:

-   Zorg ervoor dat het netwerk waarin de virtuele matrix altijd wordt geïmplementeerd de capaciteit bij opslagquota 5 Mbps Internet bandbreedte (of meer). 

    -   Internetbandbreedte nodig varieert afhankelijk van uw werkbelasting kenmerken en de snelheid van gegevens wijzigen.

    -   De gegevenswijziging van die kan worden verwerkt is rechtstreeks evenredig aan uw bandbreedte Internet. Als u bijvoorbeeld bij het nemen van een back-up een 5 Mbps bandbreedte geschikt voor een gegevenswijziging van ongeveer 18 GB acht uur. Met viermaal drukken meer bandbreedte (20 Mbps), kunt u viermaal drukken meer gegevens wijzigen (72 GB) verwerken. 

-   Controleer of de verbinding met Internet is altijd beschikbaar. Enkel geval of onbetrouwbare internetverbindingen naar de apparaten kunnen dit leiden tot een verlies van toegang tot gegevens in de cloud en kunnen resulteren in een niet-ondersteunde configuratie.

-   Als u van plan bent om te implementeren van uw apparaat als iSCSI-server: 
    -   Het is raadzaam de optie **ophalen IP-adres automatisch** (DHCP) uit te schakelen. 
    -   Statische IP-adressen configureren. U moet een primaire en een secundaire DNS-server configureren.

    -   Als u meerdere netwerkinterfaces die aan uw virtuele array, alleen de eerste netwerkinterface (al dan niet standaard, is deze interface het **Ethernet**) kan de cloud hebt bereikt. Om te bepalen het type verkeer is toegestaan, kunt u meerdere virtuele netwerkinterfaces maken op uw virtuele matrix (geconfigureerd als een iSCSI-server) en die interfaces verbinden met verschillende subnetten.

-   Alleen de cloud-bandbreedte beperken (gebruikt door de virtuele matrix), configureren op de router of de firewall beperken. Als u beperken in uw hypervisor definieert, wordt deze alle protocollen iSCSI en SMB inclusief in plaats van alleen de cloud-bandbreedte beperken. 

-   Zorg ervoor dat moment de synchronisatie voor hypervisors is ingeschakeld. Als uw virtuele matrix met behulp van Hyper-V, worden selecteren in de Manager Hyper-V, gaat u naar **instellingen &gt; Integration Services**, en zorg ervoor dat de **synchronisatie van de tijd** is ingeschakeld.

### <a name="storage-accounts"></a>Opslag-accounts

StorSimple virtuele matrix kan worden gekoppeld aan een account één opslag. Dit account opslag kan een account automatisch gegenereerde opslag, een account in hetzelfde abonnement als de service, of een account opslag gerelateerd aan een ander abonnement. Zie voor meer informatie het [beheren van opslag-accounts voor uw virtuele matrix](storsimple-ova-manage-storage-accounts.md).

Gebruik de volgende aanbevelingen voor opslag-accounts die zijn gekoppeld aan uw virtuele matrix.

-   Bij het koppelen van meerdere virtuele matrices met een account één opslag, factor de maximale capaciteit (64 TB) voor een virtuele matrix en de maximale grootte (500 TB) voor een opslag-account. Hiermee wordt het aantal volledige grootte virtuele matrices die gekoppeld aan dit account opslagruimte met ongeveer 7 worden kunnen beperkt.

-   Bij het maken van een nieuw account voor de opslag
    -   Het is raadzaam dat u deze hebt gemaakt in de regio dichtst bevindt bij het externe office/kantoor waar uw StorSimple virtuele matrix wordt geïmplementeerd op vertragingstijden minimaliseren.

    -   Houd er rekening mee dat u een account opslag niet tussen de verschillende regio's verplaatsen. U kunt ook een service niet verplaatsen in abonnementen.

    -   Gebruik een opslag-account dat wordt geïmplementeerd redundantie tussen de datacenters. Geografische redundante opslag (GRS), Zone overtollige opslag (ZRS) en lokaal overtollige opslag (LRS) worden alle ondersteund voor gebruik met uw virtuele matrix. Ga naar [Azure opslag herhaling](../storage/storage-redundancy.md)voor meer informatie over de verschillende typen opslag-accounts.


### <a name="shares-and-volumes"></a>Waarden voor aandelen en volumes

Voor uw StorSimple virtuele matrix, kunt u waarden voor aandelen inrichten wanneer deze is geconfigureerd als een bestandsserver en volumes wanneer geconfigureerd als een iSCSI-server. De aanbevolen procedures voor het maken van de waarden voor aandelen en volumes zijn gerelateerd aan de grootte en het type geconfigureerd.

#### <a name="volumeshare-size"></a>Volume/delen grootte

Klik op uw virtuele matrix, kunt u waarden voor aandelen inrichten wanneer deze is geconfigureerd als een bestandsserver en volumes wanneer geconfigureerd als een iSCSI-server. De aanbevolen procedures voor het maken van de waarden voor aandelen en volumes koppelen aan de grootte en het type geconfigureerd. 

Houd bij de inrichting van de waarden voor aandelen of volumes op uw apparaat virtuele rekening met de volgende aanbevolen procedures.

-   De grootte ten opzichte van de ingerichte grootte van een doorverbonden delen kunnen van invloed zijn op de trapsgewijs prestaties. Werken met grote bestanden kan resulteren in een traag laag af. Tijdens het werken met grote bestanden, wordt u aangeraden dat het grootste bestand kleiner dan 3% van de grootte van het delen is.

-   Een maximum van 16 volumes/aandelen kan worden gemaakt in de virtuele matrix. Als lokaal vastgemaakt, worden de volumes/aandelen tussen 50 GB tot 2 TB. Als doorverbonden, wordt de volumes/aandelen moet tussen 500 GB tot 20 TB. 

-   Bij het maken van een volume, factor in de verwachte gegevens verbruik, evenals de toekomstige groei. Terwijl het volume kan niet later worden uitgevouwen, kunt u altijd terugzetten in grotere hoeveelheden.

-   Zodra het volume is gemaakt, kunt u de grootte van het volume op StorSimple niet verkleinen.
   
-   Bij het schrijven naar een doorverbonden volume op StorSimple, als de volumegegevens van een bepaalde drempel (ten opzichte van de lokale ruimte voor het volume) bereikt, wordt de IO vertraagd. Doorlopende schrijven naar dit volume vertraagt de IO aanzienlijk. Hoewel u kunt schrijven naar een doorverbonden volume voorbij de ingerichte capaciteit (we niet actief stop de gebruiker in schrijven voorbij de ingerichte capaciteit), kunt u een waarschuwingsbericht om het effect dat u hebt oversubscribed te zien. Als u de waarschuwing ziet, het is noodzakelijk dat u corrigerende zoals verwijderen de volumegegevens maatregelen of het volume op een groter volume terugzetten (volume uitbreiding wordt momenteel niet ondersteund).

-   Voor noodgevallen herstel gebruik gevallen, zoals het aantal toegestane waarden voor aandelen/volume 16 is en het maximum aantal van waarden voor aandelen/hoeveelheden dat kan worden verwerkt parallel ook 16 is, heeft het aantal waarden voor aandelen/volume geen invloed op uw vrijgegeven Productieorder en RTOs. 

#### <a name="volumeshare-type"></a>Volume/delen type

StorSimple ondersteunt twee volume/delen typen op basis van het gebruik: lokaal vastgemaakt en doorverbonden. Lokaal vastgemaakte volumes/waarden voor aandelen zijn dik deze is ingericht dat de doorverbonden volumes/aandelen dun is ingericht. 

Het is raadzaam dat u de volgende aanbevolen procedures implementeren tijdens het configureren van StorSimple volumes/waarden voor aandelen:

-   Geef het volume dat op basis van de werklast die u implementeren wilt voordat u een volume maken. Lokaal vastgemaakte volumes voor werkbelasting waarvoor lokale garanties met gegevens (ook tijdens een storing cloud) en die zijn vereist lage cloud vertragingstijden gebruiken. Nadat u een volume op uw virtuele matrix maakt, u kunt het volumetype niet wijzigen vanuit lokaal vastgemaakt tot doorverbonden of *omgekeerd*. Als u bijvoorbeeld lokaal vastgemaakte volumes maken bij het distribueren van de SQL-werkbelastingen of werkbelasting host voor virtuele machines (VMs); gelaagde volumes voor bestand delen werkbelasting gebruiken.

-   Schakel de optie voor minder veelgebruikte archivering gegevens bij het afhandelen van grote bestanden. Deduplication segment groter van 512 K wordt gebruikt wanneer deze optie is ingeschakeld voor de overdracht van gegevens in de cloud versnellen.

#### <a name="volume-format"></a>Volume-indeling

Nadat u op de server iSCSI StorSimple volumes hebt gemaakt, moet u geïnitialiseerd, koppelen en de hoeveelheden opmaken. Deze bewerking wordt uitgevoerd op de host die is gekoppeld aan uw apparaat StorSimple. Volgende aanbevolen procedures wordt aanbevolen wanneer koppelen en opmaak volumes op de StorSimple-host.

-   Een snelle indeling op alle StorSimple volumes uitvoeren.

-   Wanneer u een volume StorSimple opmaakt, gebruikt u een toewijzing-grootte (Australië) van 64 KB (de standaardinstelling is 4 KB). De 64 KB Australië is gebaseerd op tests gedaan intern voor algemene StorSimple werkbelasting en andere werkbelasting.

-   Wanneer u met de StorSimple virtuele matrix geconfigureerd als een iSCSI-server, gebruik geen spanned volumes of dynamische schijven als deze volumes of schijven worden niet ondersteund door StorSimple.

#### <a name="share-access"></a>Delen van access

Volg deze richtlijnen bij het maken van waarden voor aandelen op uw bestandsserver virtuele matrix:

-   Wanneer u maakt een delen, moet u een groep toewijzen als de beheerder van een delen in plaats van één gebruiker.

-   Nadat de optie voor delen is gemaakt door het bewerken van de waarden voor aandelen tot en met Windows Verkenner, kunt u de NTFS-machtigingen beheren.

#### <a name="volume-access"></a>Het volume van access

Tijdens het configureren van de hoeveelheden iSCSI op uw StorSimple virtuele matrix, is het is belangrijk voor toegangsbeheer, waar nodig. Als u wilt bepalen welke hostservers toegang heeft tot volumes, maken en access control-records (ACRs) koppelen aan StorSimple delen.

Gebruik de volgende aanbevolen procedures tijdens het configureren van ACRs voor StorSimple volumes:

-   Altijd ten minste één ACR aan een volume koppelen.

-   Meerdere ACRs alleen definiëren in een geclusterde omgeving.

-   Zorg ervoor dat het volume niet beschikbaar is in een manier die waar gelijktijdig door meer dan één niet-gegroepeerde host kunnen worden geopend als voor meer informatie over het toewijzen van meer dan één ACR aan een volume. Als u meerdere ACRs hebt toegewezen aan een volume, verschijnt een waarschuwing u moet controleren van uw configuratie.

### <a name="data-security-and-encryption"></a>, Gegevensbeveiliging en codering

Uw StorSimple virtuele matrix heeft gegevens beveiliging en codering functies die de vertrouwelijkheid en de integriteit van uw gegevens zorgen. Wanneer u met deze functies, is het aanbevolen dat u deze aanbevolen procedures volgen: 

-   Definieer een cloud opslag versleutelingssleutel om te genereren AES-256 versleuteling voordat de gegevens uit uw virtuele matrix wordt verzonden naar de cloud. Deze sleutel is niet vereist als uw gegevens te is versleuteld. De toets worden gegenereerd en veilig met een managementsysteem key zoals [Azure belangrijke kluis](../key-vault/key-vault-whatis.md)blijft.

-   Tijdens het configureren van het account opslag via de StorSimple Manager-service, zorg er dan voor dat de SSL-modus voor het maken van een beveiligd kanaal voor communicatie tussen uw apparaat StorSimple en de cloud in te schakelen.

-   Genereren de sleutels voor uw opslag-accounts (via de Azure Storage-service) geregeld in account voor wijzigingen voor toegang tot op basis van de gewijzigde lijst met beheerders.

-   Gegevens op uw virtuele matrix is gecomprimeerd en deduplicated voordat deze wordt verzonden naar Azure. Het gebruik van de gegevens Deduplication rol-service op uw Windows Server-host aanbevolen niet.


## <a name="operational-best-practices"></a>Aanbevolen procedures

De aanbevolen procedures zijn richtlijnen die moeten worden gevolgd tijdens de dagelijkse management of de werking van de virtuele matrix. Deze procedures begeleidende specifieke beheertaken zoals het maken van back-ups, terugzetten uit een back-reeks, uitvoering van een failover, te deactiveren en verwijderen van de matrix, systeemgebruik en servicestatus bewaken, en uit te voeren virus worden op uw virtuele matrix.

### <a name="backups"></a>Back-ups

De gegevens op uw virtuele matrix back-up in de cloud op twee manieren, wordt automatisch een standaard dagelijkse back-up van het hele apparaat op 22:30 of via een handmatige back-up op aanvraag starten. Standaard het apparaat automatisch gemaakt dagelijkse cloud momentopnamen van alle gegevens die zich op is geïnstalleerd. Ga naar de [back-up van uw StorSimple virtuele matrix](storsimple-ova-backup.md)voor meer informatie.

De frequentie en het bewaarbeleid die is gekoppeld aan de standaard back-ups kunnen niet worden gewijzigd, maar u kunt de tijd waarop de dagelijkse back-ups elke dag worden geïnitieerd configureren. Tijdens het configureren van de starttijd van de automatische back-ups, is het aanbevolen dat:

-   Uw back-ups plannen voor rustige uren. Back-begintijd moet niet overeenkomen met veel host IO.

-   Een handmatige back-up op aanvraag starten bij het plannen van een apparaat failover of voorafgaand aan het onderhoudsvenster om de gegevens op uw virtuele matrix te beveiligen te voeren.

### <a name="restore"></a>Herstellen

U kunt terugzetten uit een back-up op twee manieren instellen: herstellen naar een ander volume of delen of herstellen een itemniveau (alleen beschikbaar in een virtuele matrix geconfigureerd als een bestandsserver). Itemniveau herstel kunt u moet een gedetailleerde herstel van bestanden en mappen van een wolk back-up van alle aandelen op het apparaat StorSimple. Ga naar het [herstellen van een back-up](storsimple-ova-restore.md)voor meer informatie.

Houd rekening met de volgende richtlijnen bij het uitvoeren van een herstellen:

-   Uw StorSimple virtuele matrix biedt geen ondersteuning voor in-place herstellen. Dit is echter gemakkelijk bereikt door een proces: ruimte toe te voegen op de virtuele matrix en zet naar een ander volume/delen.

-   Wanneer terugzetten uit een lokale volume, houd er rekening mee zijn de terugzetten een langdurige bewerking. Hoewel het volume snel online komt mogelijk, blijft de gegevens worden gehydrateerd op de achtergrond.

-   Het volumetype blijft hetzelfde tijdens het terugzetten. Het volume van een doorverbonden naar een ander doorverbonden volume is hersteld en een lokaal vastgemaakte volume naar een ander lokaal vastgemaakt volume.

-   Als u een volume of een delen herstellen uit een back-up wilt instellen, als de terugzettaak mislukt, een doeltoepassing volume of delen nog steeds worden gemaakt in de portal. Het is belangrijk dat u dit niet-gebruikte doelvolume verwijderen of in de portal delen te minimaliseren ten eventuele toekomstige problemen die voortvloeien uit met dit element.

### <a name="failover-and-disaster-recovery"></a>Failover en noodgevallen herstel

Een failover apparaat kunt u uw gegevens uit een *bron* -apparaat in het datacenter migreren naar *een ander apparaat zich bevindt in dezelfde of een andere geografische locatie* . De overname van het apparaat is voor het hele apparaat. Tijdens een overname verandert de cloud-gegevens voor het bronapparaat eigendom met die van het doelapparaat.

Voor uw StorSimple virtuele matrix, kunt u alleen niet via een andere virtuele matrix beheerd door de dezelfde StorSimple Manager-service. Een failover naar een apparaat 8000 reeks of een matrix beheerd door een andere StorSimple Manager-service (dan het account voor het bronapparaat) is niet toegestaan. Meer informatie over aandachtspunten voor de failover, gaat u naar de [vereisten voor de overname van het apparaat](storsimple-ova-failover-dr.md).

Bij het uitvoeren van een fail via voor uw virtuele matrix, houd u het volgende in gedachten:

-   Voor een geplande failover is dit een aanbevolen aanbevolen wordt om het uitvoeren van alle volumes/aandelen offline voordat u de overname wordt gestart. Volg de instructies van besturingssysteem / regiospecifieke kunt u de offline volumes/aandelen op de host eerste en maakt u die offline op het virtuele apparaat.

-   Voor een server noodgevallen Bestandsherstel (DR), wordt u aangeraden het doelapparaat te koppelen aan hetzelfde domein als de bron zodat de sharemachtigingen automatisch opgelost worden. Alleen de overname op een apparaat in hetzelfde domein wordt ondersteund in deze release.

-   Zodra de DR is voltooid, wordt het bronapparaat automatisch verwijderd. Hoewel het apparaat niet langer beschikbaar is, verbruikt de virtuele machine die u deze is ingericht op het hostsysteem nog steeds bronnen. Het is raadzaam dat u deze virtuele machine uit uw hostsysteem verwijderen om te voorkomen dat de kosten die.

-   Rekening met het volgende zelfs als de overname mislukt, **de gegevens is altijd veilig in de cloud**. Houd rekening met de volgende drie scenario's waarin de overname is niet voltooid:

    -   Er is een fout opgetreden in de eerste fasen van de overname zoals wanneer de oude DR-controles worden uitgevoerd. In dit geval is uw apparaat nog steeds bruikbaar. U kunt de overname op hetzelfde apparaat opnieuw.

    -   Er is een fout opgetreden tijdens de werkelijke failoverproces. In dit geval wordt het doelapparaat onbruikbaar gemarkeerd. U moet inrichten en u kunt ook een ander doel virtuele matrix configureren en gebruiken die voor failover.

    -   De overname was voltooid volgen die de Bronapparaat is verwijderd, maar het doelapparaat problemen heeft en u geen toegang tot gegevens. De gegevens in de cloud nog steeds veilig is en eenvoudig kan worden opgehaald door te maken van een andere virtuele matrix en vervolgens als een apparaat gebruiken voor de DR.

### <a name="deactivate"></a>Deactiveren

Wanneer u een virtuele matrix StorSimple hebt gedeactiveerd, kunt u de verbinding tussen het apparaat en de bijbehorende StorSimple Manager-service verbreekt. Deactiveren niet **ongedaan** maken en kan niet ongedaan worden gemaakt. Een gedeactiveerd apparaat kan niet worden geregistreerd met de service StorSimple Manager opnieuw. Ga naar [Deactiveer en verwijder uw StorSimple virtuele matrix](storsimple-deactivate-and-delete-device.md)voor meer informatie.

Houd rekening met de volgende aanbevolen procedures wanneer uw virtuele matrix deactiveren:

-   Een cloud momentopname van alle gegevens voordat u een virtueel apparaat deactiveren. Wanneer u een virtuele matrix hebt gedeactiveerd, gaat alle lokaal apparaatgegevens verloren. Een momentopname van de cloud, kunt u gegevens in een later stadium herstellen.

-   Voordat u een virtuele matrix StorSimple hebt gedeactiveerd, moet u om te stoppen of clients en hosts die afhankelijk van het apparaat zijn verwijderen.

-   Een gedeactiveerd apparaat verwijderen als u niet langer gebruikt zodat deze niet kosten toenemen.

### <a name="monitoring"></a>Cmdlets voor controle

Om ervoor te zorgen dat uw StorSimple virtuele matrix zich in een doorlopend orde zijn, moet u controleren van de matrix en zorg ervoor dat u gegevens van het systeem zoals meldingen wilt ontvangen. Als u wilt controleren van de algemene status in de virtuele matrix, implementeren de volgende aanbevolen procedures:

- Controle om bij te houden van het gebruik van de schijf van de schijf van de gegevens virtuele matrix, evenals de schijf OS configureren Als met Hyper-V, kunt u een combinatie van systeem Center VM Manager (SCVMM) en System Center Operations Manager (SCOM) gebruiken om de virtualization hosts te houden.   

- E-mailmeldingen configureren op uw virtuele matrix om waarschuwingen te verzenden op bepaalde niveaus gebruik.                                                                                                                                                                                                

### <a name="index-search-and-virus-scan-applications"></a>Index zoeken en virussen scannen toepassingen

Gegevens uit de lokale laag in de cloud Microsoft Azure kunt automatisch trapsgewijs door een virtueel StorSimple-matrix. Wanneer een toepassing zoals een indexzoekopdracht of een viruscontrole wordt gebruikt om de gegevens die zijn opgeslagen op StorSimple scannen, moet u ervoor te zorgen dat de cloud-gegevens niet ophalen toegankelijk en binnengehaald terug naar de lokale laag.

Het is raadzaam dat u de volgende aanbevolen procedures implementeren tijdens het configureren van de index zoeken of virus gescande afbeelding op uw virtuele matrix:

-   Alle bewerkingen automatisch geconfigureerde volledige scan uitschakelen.

-   Voor trapsgewijze volumes, de index zoeken of virus scan toepassing configureren voor een incrementele scan uitvoeren. Dit wilt scannen alleen de nieuwe gegevens waarschijnlijk die zich op de lokale laag. De gegevens die in de cloud is doorverbonden is niet toegankelijk tijdens een incrementele bewerking.

-   Controleer of de juiste zoekresultaten filters en instellingen zijn geconfigureerd, zodat alleen de beoogde soorten bestanden ophalen gescande. Bijvoorbeeld afbeeldingsbestanden (JPEG, GIF en TIFF) en technische tekeningen moet niet worden gecontroleerd tijdens de incrementele of volledige index opnieuw opbouwen.

Als Windows indexeren proces gebruikt, volgt u deze richtlijnen:

-   Gebruik de Windows-indexering niet voor trapsgewijze volumes, zoals deze grote hoeveelheden gegevens (TBs) vanuit de cloud terughalen als de index moet regelmatig opnieuw gemaakt. Opnieuw ophalen van alle bestandstypen als u wilt indexeren van de inhoud daarvan.

-   Gebruik de Windows proces voor het lokaal vastgemaakte volumes indexeren zoals dit alleen toegang gegevens op de lokale lagen index tot zou (de cloud-gegevens niet worden gebruikt) te maken.

### <a name="byte-range-locking"></a>Byte bereik vergrendelen

Toepassingen kunnen u een bepaald bereik van bytes vergrendelen binnen de bestanden. Als op de toepassingen die aan uw StorSimple schrijft byte bereik vergrendelen is ingeschakeld, werkt klikt u vervolgens trapsgewijs schakelen niet op uw virtuele matrix. Voor de trapsgewijs schakelen als u wilt werken, moeten alle gebieden van de bestanden toegankelijk worden ontgrendeld. Byte bereik vergrendelen wordt met doorverbonden delen op uw virtuele matrix niet ondersteund.

Aanbevolen maatregelen om aan te bieden byte bereik vergrendelen omvatten:

-   Bytebereik vergrendelen in uw toepassingslogica uitschakelen.

-   Lokaal gebruikt vastgemaakt volumes (in plaats van doorverbonden) voor de gegevens die zijn gekoppeld aan deze toepassing. Lokaal vastgemaakte volumes niet trapsgewijs in de cloud.

-   Wanneer gebruik volumes lokaal vastgemaakt met byte-bereik vergrendelen is ingeschakeld, kan het volume online komt voordat het herstellen voltooid is. In deze processen, moet u de terugzetten volledig wachten.

## <a name="multiple-arrays"></a>Meerdere matrices

Meerdere virtuele matrices moet mogelijk worden geïmplementeerd in account voor een groeiende werkset van gegevens die naar de cloud dus dat dit gevolgen heeft de prestaties van het apparaat kan Mors. In deze processen is het beste schaal apparaten als de werkset in omvang groeit. U moet hiervoor een of meer apparaten moet worden opgeteld in de on-premises implementatie-Datacenter. Wanneer u de apparaten toevoegt, kunt u het volgende doen:

-   De huidige set met gegevens splitsen.
-   Nieuwe werkbelasting dashboard implementeren naar nieuwe apparaten.
-   Als meerdere virtuele matrices implementeert, is het aanbevolen dat van taakverdeling oogpunt de matrix verdelen over verschillende hypervisor hosts.

-  Meerdere virtuele matrices (indien geconfigureerd als een bestandsserver of een server iSCSI) kunnen worden geïmplementeerd in een Namespace bestand systeem verdeeld. Ga naar [Distributed bestand System Namespace-oplossing met hybride Cloud opslag Implementatiehandleiding](https://www.microsoft.com/download/details.aspx?id=45507)voor gedetailleerde stappen. Verdeelde bestand systeem replicatie wordt momenteel niet aanbevolen voor gebruik met de virtuele matrix. 


## <a name="see-also"></a>Zie ook
Informatie over het [beheren van uw StorSimple virtuele matrix](storsimple-ova-manager-service-administration.md) via de StorSimple Manager-service.
