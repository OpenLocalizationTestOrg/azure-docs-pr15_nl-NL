<properties
    pageTitle="Importeren/exporteren gebruiken om gegevens te brengen met Blob Storage | Microsoft Azure"
    description="Informatie over het maken van importeren en exporteren van taken in de klassieke Azure-portal om gegevens als u wilt blob storage te brengen."
    authors="renashahmsft"
    manager="aungoo"
    editor="tysonn"
    services="storage"
    documentationCenter=""/>

<tags
    ms.service="storage"
    ms.workload="storage"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/18/2016"
    ms.author="renash"/>


# <a name="use-the-microsoft-azure-importexport-service-to-transfer-data-to-blob-storage"></a>Gebruik van de Service van Microsoft Azure importeren/exporteren om gegevens te brengen met Blob storage

## <a name="overview"></a>Overzicht

Azure importeren/exporteren Service kunt u grote hoeveelheden gegevens veilig overbrengen met Azure-blobopslag door vaste schijven een Azure Datacenter voor verzending. U kunt ook deze service gegevens van Azure-blobopslag overbrengen naar vaste schijven en verzenden naar uw on-premises implementatie-site. Deze service is geschikt in situaties waarin u wilt overbrengen van verschillende TBs van gegevens voor of van Azure, maar het uploaden of downloaden via het netwerk niet mogelijk gecombineerd vanwege beperkte bandbreedte of hoog netwerkkosten is.

De service is vereist dat vaste schijven BitLocker versleuteld voor de beveiliging van uw gegevens moet zijn. De service ondersteunt klassieke opslag accounts aanwezig zijn in alle gebieden van openbare Azure. U moet vaste schijven naar een van de ondersteunde locaties opgegeven verderop in dit artikel verzenden.

In dit artikel leert u meer informatie over de service Azure importeren/exporteren en hoe u schijven voor uw gegevens kopiëren naar en vanuit Azure-blobopslag verzenden.

> [AZURE.IMPORTANT] U kunt maken en beheren van importeren en exporteren van taken voor klassieke opslag via de portal klassieke of de [service importeren/exporteren REST API's](http://go.microsoft.com/fwlink/?LinkID=329099). Resourcemanager opslag accounts worden niet ondersteund op dit moment.

## <a name="when-should-i-use-the-azure-importexport-service"></a>Wanneer moet ik de service Azure importeren/exporteren gebruiken?

Kunt u overwegen Azure importeren/exporteren service tijdens het uploaden of downloaden van gegevens via het netwerk is te langzaam of aan extra netwerkbandbreedte is veel te duur.

U kunt deze service gebruiken in scenario's zoals:

- Gegevens in de cloud migreren: naar grote hoeveelheden gegevens Azure snel en effectief kosten.
- Inhoud van de verdeling: gegevens snel te verzenden aan uw klant-sites.
- Back-up: Duren back-ups van uw on-premises gegevens op te slaan in Azure-blobopslag.
- Herstel van gegevens: herstellen grote hoeveelheid gegevens die zijn opgeslagen in blobopslag en bezorgd bij uw on-premises locatie.

## <a name="pre-requisites"></a>Minimumvereisten

In dit gedeelte hebben we de minimumvereisten voor het gebruik van deze service vereist vermeld. Controleer de kosten zorgvuldig door voordat u uw stations levert.

### <a name="storage-account"></a>Opslag-account

U moet een bestaand Azure-abonnement en een of meer **klassieke** opslag accounts gebruik van de service importeren/exporteren. Elke taak kan worden gebruikt om gegevens naar of slechts één klassieke opslag-account te brengen. Een taak één importeren/exporteren niet met andere woorden, meerdere omvatten meerdere opslag-accounts. Zie voor informatie over het maken van een nieuw account voor de opslag, [het maken van een opslag-Account](storage-create-storage-account.md#create-a-storage-account).

### <a name="blob-types"></a>BLOB typen

U kunt importeren/exporteren van Azure-service gebruiken om gegevens te kopiëren naar **blok** BLOB's of **pagina** BLOB's. Omgekeerd, kunt u alleen **blokkeren** BLOB's, BLOB van **pagina's** of **toevoegquery** BLOB's exporteren uit Azure opslag met deze service.

### <a name="job"></a>Taak

Om te beginnen met het proces van het importeren of exporteren uit Blob storage, moet u eerst een taak maken. Een taak is een importeren of een taak exporteren:

- Maak een project importeren als u overbrengen van gegevens die u hebt wilt on-premises implementatie naar BLOB's in uw account Azure opslag.
- Maak een taak voor het exporteren als u wilt overbrengen van gegevens die momenteel zijn opgeslagen als BLOB's in uw account opslag naar vaste schijven die naar u worden verzonden.

Wanneer u een taak maakt, melden u de service importeren/exporteren dat u een of meer vaste schijven een Azure Datacenter voor wordt verzending.

- Voor een project importeren, wordt u vaste schijven met uw gegevens verzending.
- Voor een taak voor het exporteren, wordt u lege vaste schijven verzending.
- U kunt maximaal 10 vaste schijven per project verzenden.

U kunt maken van een import- of taak via de [portal Classic](https://manage.windowsazure.com/) of de [Azure opslag importeren/exporteren REST API](http://go.microsoft.com/fwlink/?LinkID=329099)exporteren.

### <a name="client-tool"></a>Hulpprogramma-client

De eerste stap bij het maken van een project **importeren** is voor het voorbereiden van uw station die worden verzonden voor importeren. Als u wilt voorbereiden op uw harde schijf, moet u deze verbinden met een lokale server en het hulpprogramma Azure importeren/exporteren Client uitgevoerd op de lokale server. Dit hulpmiddel client kunt u uw gegevens kopiëren naar het station, coderen van de gegevens op het station met BitLocker en het station logboek bestanden genereren.

De bestanden logboek slaan basisinformatie over de taak en het station zoals station serieel getal en opslagaccountnaam. Dit bestand logboek wordt niet opgeslagen op de schijf. Deze worden gebruikt tijdens het importeren taak maken. Meer informatie over het maken van taken worden stap voor stap gegeven verderop in dit artikel.

Het clienthulpprogramma is alleen compatibel met 64-bits Windows-besturingssysteem. Zie het gedeelte van het [besturingssysteem](#operating-system) voor specifieke OS versies worden ondersteund.

Download de meest recente versie van het [hulpprogramma Azure importeren/exporteren-client](http://go.microsoft.com/fwlink/?LinkID=301900&clcid=0x409). Voor meer informatie over het gebruik van het hulpprogramma Azure importeren/exporteren, raadpleegt u [Naslaginformatie Azure op importeren/exporteren](http://go.microsoft.com/fwlink/?LinkId=329032).

### <a name="hard-disk-drives"></a>Vaste schijven

Alleen 3,5 inch SATA II/III interne vaste schijven worden ondersteund voor gebruik met de service importeren/exporteren. U kunt vaste schijven maximaal 10TB.
Voor taken importeren, worden alleen het eerste gegevensvolume op de schijf verwerkt. Het gegevensvolume moet worden opgemaakt met NTFS.
Wanneer het kopiëren van gegevens naar de harde schijf, kunt u deze rechtstreeks via een verbindingslijn SATA koppelen of u kunt extern met een externe SATA II/III USB-adapter toevoegen. We raden u aan met een van de volgende externe SATA II/III USB-adapters:

- Anker 68UPSATAA - 02BU
- Anker 68UPSHHDS-BU
- Startech SATADOCK22UE
- Sharkoon QuickPort XT CH

Als er een conversieprogramma die hierboven niet wordt genoemd, probeert u kunt het hulpprogramma voor het importeren/exporteren van Azure met uw conversieprogramma voor het voorbereiden van het station en kijk of deze werkt voordat u een ondersteunde conversieprogramma aanschaft uitvoeren.

> [AZURE.IMPORTANT] Externe vaste schijven die worden meegeleverd bij een ingebouwde USB-adapter worden niet ondersteund door deze service. Bovendien kan niet de schijf binnen het omhulsel van een externe harde schijf worden gebruikt; Stuur geen externe harde schijven.

### <a name="encryption"></a>Versleuteling

De gegevens op de schijf moet worden gecodeerd BitLocker station-versleuteling gebruikt. Hiermee beveiligt u uw gegevens tijdens overdracht.

Voor taken importeren zijn er twee manieren naar de versleuteling wordt uitgevoerd. De eerste manier is via de / parameter versleutelen wanneer het clienthulpprogramma tijdens voorbereiding van station uitgevoerd. De tweede manier is om te schakelen BitLocker-versleuteling handmatig op de schijf en geven van de sleutel in de opdrachtregel van client hulpmiddel tijdens voorbereiding van station.

Voor exporteren, nadat u uw gegevens wordt gekopieerd naar de stations de service wordt het station versleutelen met BitLocker voordat u terug naar u verzenden. De sleutel krijgt u via de portal klassieke.  

### <a name="operating-system"></a>Besturingssysteem

U kunt een van de volgende 64-bits besturingssystemen gebruiken voor het voorbereiden van de harde schijf met het hulpprogramma Azure importeren/exporteren voordat u het station Azure verzendt:

Windows 7 Enterprise, Windows 7 Ultimate, Windows 8 Pro, Enterprise, Windows 8, Windows 8.1 Pro, ondernemings-Windows 8.1, Windows 10<sup>1</sup>, Windows Server 2008 R2, Windows Server 2012, Windows Server 2012 R2. Deze besturingssystemen ondersteunt BitLocker stationsversleuteling.

> [AZURE.IMPORTANT] <sup>1</sup> Als u een computer met Windows 10 gebruikt voor het voorbereiden van uw harde schijf, download u de nieuwste versie van het hulpprogramma Azure importeren/exporteren.

### <a name="locations"></a>Locaties

De service Azure importeren/exporteren ondersteunt kopiëren van gegevens en naar alle openbare Azure opslag-accounts. Vaste schijven naar een van de volgende locaties, kunt u verzenden. Als uw account opslagruimte op een openbare Azure locatie die hier niet wordt opgegeven, wordt een alternatieve verzendlocatie krijgt wanneer u de taak via de portal Classic of het importeren/exporteren REST API maakt.

Ondersteunde verzending locaties:

- Oost-VS

- West VS

- Oost-Amerikaanse 2

- Central US

- Centraal Noord-Amerikaanse

- Zuid-Central US

- Noord-Europa

- West Europa

- Oost-Azië

- Zuidoost-Azië

- Australië Oost

- Australië Zuidoost

- Japan West

- Japan Oost

- Centraal India


### <a name="shipping"></a>Verzending

**Verzending stations Datacenter voor de:**

Bij het maken van een project importeren of exporteren, krijgt u een verzendadres van een van de ondersteunde locaties naar uw schijven verzenden. Het verzendadres opgegeven hangt af van de locatie van uw opslag-account, maar deze mogelijk niet hetzelfde als de opslaglocatie van het account.

U kunt vervoerders zoals FedEx, knop compatibiliteitsvoorkeuren voor Excel, UPS of de postdienst ons uw schijven naar het verzendadres verzenden.

**Verzending stations vanuit het beheercentrum van gegevens:**

Bij het maken van een project importeren of exporteren, moet u opgeven dat een adres van afzender voor Microsoft levert de stations terug nadat uw project voltooid is. Zorg ervoor dat u opgeeft dat een geldig adres van de afzender om te voorkomen dat vertragingen in verwerking.

U moet een geldige FedEx of knop compatibiliteitsvoorkeuren voor Excel carrier ook opgeven dat moet worden gebruikt door Microsoft voor het verzenden van de stations terug-account. Een accountnummer FedEx is vereist voor het verzenden van stations terug vanaf de Verenigde Staten en Europa locaties. Een nummer voor de knop compatibiliteitsvoorkeuren voor Excel is vereist voor het verzenden van stations terug van Azië en Australië locaties. U kunt een [FedEx](http://www.fedex.com/us/oadr/) (voor de Verenigde Staten en Europa) of de [knop compatibiliteitsvoorkeuren voor Excel](http://www.dhl.com/) (Azië en Australië) carrier account maken als u geen hebt. Als u al een accountnummer carrier hebt, controleert u of geldig is.

Klik in uw pakketten verzendt, moet u de voorwaarden op [Servicevoorwaarden voor Microsoft Azure](https://azure.microsoft.com/support/legal/services-terms/)volgen.

> [AZURE.IMPORTANT] Houd er rekening mee dat de fysieke media die u levert mogelijk moet cross internationale randen. U bent verantwoordelijk voor dat uw fysieke media en de gegevens zijn geïmporteerd en/of de toepasselijke wetgeving geëxporteerd. Voordat u de fysieke media levert, informeert u bij uw adviseurs om te bevestigen dat uw media en gegevens kunnen wettelijk Datacenter voor de geïdentificeerd worden verzonden. Hierdoor om ervoor te zorgen dat Microsoft tijdig worden bereikt. Een pakket dat internationale randen snijden moet bijvoorbeeld een handelsfactuur vergezeld met het pakket (behalve als kruist randen binnen de Europese Unie). U kunt een opgevuld kopie van de handelsfactuur van carrier website afdrukken. Voorbeeld van de commerciële factuur zijn [knop compatibiliteitsvoorkeuren voor Excel-handelsfactuur] (http://invoice-template.com/wp-content/uploads/dhl-commercial-invoice-template.pdf) of een [FedEx handelsfactuur](http://images.fedex.com/downloads/shared/shipdocuments/blankforms/commercialinvoice.pdf). Zorg ervoor dat Microsoft niet is opgegeven als de exporteur.

## <a name="how-does-the-azure-importexport-service-work"></a>Hoe werkt de service Azure importeren/exporteren?

U kunt gegevens uitwisselen tussen uw on-premises-site en het gebruik van de service Azure importeren/exporteren door taken te maken en verzenden van vaste schijven een Azure Datacenter voor Azure-blobopslag. Elke harde schijf die u verzendt, is gekoppeld aan één taak. Elke taak is gekoppeld aan een account één opslag. Raadpleeg de [sectie minimumvereisten](#pre-requisites) zorgvuldig voor meer informatie over de details van deze service, zoals ondersteund blob typen, schijftypen, locaties en verzending.

In deze sectie beschrijven maar wij op hoog niveau de stappen voor het importeren en exporteren van taken. We bieden stapsgewijze instructies voor het maken van een importeren en exporteren van taak later in de [sectie snel starten](#quick-start).

### <a name="inside-an-import-job"></a>Binnen een project importeren

Een project importeren omvat op hoog niveau, de volgende stappen:

- Hiermee bepaalt u de gegevens die moeten worden geïmporteerd en het aantal stations die u nodig hebt.
- Geef aan dat de bestemming BLOB's voor uw gegevens in blobopslag.
- Gebruik het hulpprogramma Azure importeren/exporteren naar uw gegevens naar een of meer vaste schijven kopiëren en deze te versleutelen met BitLocker.  
- Maak een project importeren in uw doel klassieke opslag-account via de portal klassieke of de REST API voor importeren/exporteren. Als de portal klassieke gebruikt, kunt u het station logboek-bestanden uploaden.
- Geef het adres van de afzender en carrier accountnummer moet worden gebruikt voor de stations terug naar u verzenden.
- De harde schijf schijven naar het verzendadres tijdens het maken van taken verzenden.
- Werk de bezorging getal in de details van de taak importeren bijhouden en indienen van het project importeren.
- Stations zijn ontvangen en verwerkt in het midden van de Azure-gegevens.
- Stations worden verzonden met uw account carrier het adres van afzender die beschikbaar zijn in de taak importeren.

    ![Afbeelding 1:Import taak stroom](./media/storage-import-export-service/importjob.png)


### <a name="inside-an-export-job"></a>Binnen een taak voor het exporteren

Een taak voor het exporteren omvat op hoog niveau, de volgende stappen:

- Hiermee bepaalt u de gegevens die moeten worden geëxporteerd en het aantal stations die u nodig hebt.
- Geef aan dat de bron BLOB's of de container paden van uw gegevens in blobopslag.
- Maak een taak voor het exporteren in uw bron-opslag-account via de portal klassieke of de REST API voor importeren/exporteren.
- Geef de bron BLOB's of de container paden van uw gegevens in de taak exporteren.
- Geef het adres van de afzender en carrier accountnummer voor moet worden gebruikt voor de stations terug naar u verzenden.
- De harde schijf schijven naar het verzendadres tijdens het maken van taken verzenden.
- Werk de bezorging van het bijhouden van getal in de details van de taak exporteren en de taak exporteren verstuurt.
- De stations zijn ontvangen en verwerkt in het midden van de Azure-gegevens.
- De stations zijn versleuteld met BitLocker; de toetsen zijn beschikbaar via de portal klassieke.  
- De stations worden verzonden met uw account carrier het adres van afzender die beschikbaar zijn in de taak importeren.

    ![Afbeelding 2:Export taak stroom](./media/storage-import-export-service/exportjob.png)

### <a name="viewing-your-job-status"></a>Uw taakstatus weergeven

U kunt bijhouden van de status van het importeren of exporteren van taken in de klassieke-portal. Ga naar uw account opslag in de klassieke-portal en klik op het tabblad **Importeren/exporteren** . Een lijst van uw taken wordt op de pagina weergegeven. U kunt de lijst op taakstatus, Taaknaam, taaktype of volgnummer filteren.

U ziet een van de volgende taak statussen afhankelijk van waar uw harde schijf in het proces is.

| Taakstatus   | Beschrijving                                                                                                                                                                                                                                                                                                                                                                        |
|:-------------|:-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Maken     | Uw taak is gemaakt, maar u uw verzendingsgegevens nog niet hebt opgegeven.                                                                                                                                                                                                                                                                                                |
| Verzending     | Uw taak is gemaakt en u uw verzendingsgegevens hebt opgegeven. **Opmerking**: als het station wordt afgeleverd bij het beheercentrum Azure gegevens, de status nog steeds 'Verzending' voor enige tijd kan weergeven. Zodra de service wordt gestart kopiëren van uw gegevens, wordt de status wordt gewijzigd in "Overdragen". Als u wilt meer informatie over status van uw station bekijken, kunt u de REST API voor importeren/exporteren. |
| Doorschakelen | Uw gegevens worden overgebracht van de harde schijf (voor een project importeren) of op uw harde schijf (voor een taak voor het exporteren).                                                                                                                                                                                                                                                                 |
| Verpakking    | De overdracht van uw gegevens is voltooid en de harde schijf wordt voorbereid voor verzending naar u.                                                                                                                                                                                                                                                                             |
| Voltooien     | De harde schijf is terug naar u verzonden.                                                                                                                                                                                                                                                                                                                                      |

### <a name="time-to-process-job"></a>Tijd aan proces project

De hoeveelheid tijd die nodig is voor een taak importeren/exporteren afhankelijk van andere factoren zoals tijd van verzending varieert, proces vervullen type, type en de grootte van de gegevens die wordt gekopieerd en de grootte van de opgegeven schijven. Een SLA beschikt niet over door de Service importeren/exporteren. U kunt de REST API gebruiken om de taakvoortgang nauwer te houden. Er is een parameter voor percentage voltooid in de lijst taken bewerking waarmee de vermelding van de voortgang van kopiëren. Bereiken met ons als u een schatting om een tijd kritieke importeren/exporteren taak te voltooien.

### <a name="pricing"></a>Prijzen

**Station kosten voor de verwerking**

Er zijn station afhandeling kosten voor elk station verwerkt als onderdeel van het importeren of exporteren van de taak. Zie de informatie op de [Prijzen van Azure importeren/exporteren](https://azure.microsoft.com/pricing/details/storage-import-export/).

**Kosten van verzending**

Wanneer u stations naar Azure verzendt, kunt u de kosten van verzending betalen aan de vervoerder. Wanneer u Microsoft geeft als resultaat de stations voor u, de verzending kosten in rekening wordt gebracht voor de carrier rekening die u op het moment van het maken van taken hebt opgegeven.

**Transactiekosten**

Er zijn geen transactiekosten bij het importeren van gegevens in blobopslag. De kosten die worden standaard egress zijn van toepassing wanneer gegevens worden geëxporteerd vanuit blobopslag. Zie voor meer informatie over transactiekosten, [gegevens overbrengen prijzen.](https://azure.microsoft.com/pricing/details/data-transfers/)

## <a name="quick-start"></a>Snel aan de slag

In dit gedeelte worden wij bieden stapsgewijze instructies voor het maken van een import- en een taak voor het exporteren. Zorg ervoor dat u alle de [minimumvereisten](#pre-requisites) voldoen aan voordat u verdergaat.

## <a name="how-to-create-an-import-job"></a>Het maken van een project importeren?

Een taak importeren als gegevens wilt kopiëren naar uw account Azure opslag van vaste schijven door het verzenden van een of meer stations met de gegevens naar het midden van de opgegeven gegevens maken. De taak importeren geeft informatie over vaste schijven, gegevens worden gekopieerd, opslag-account en verzending informatie bij de service Azure importeren/exporteren. Een project importeren maakt, wordt een drie stappen. Bereid eerst uw stations met de functie van de client Azure importeren/exporteren. Tweede, een importeren taak met behulp van de portal klassieke verzenden. Derde de schijven naar het verzendadres tijdens het maken van taken verzenden en de verzending info in de details van uw project bijwerken.   

> [AZURE.IMPORTANT] U kunt slechts één taak per account opslag indienen. Elk station dat u verzendt, kan worden geïmporteerd aan één opslag-account. Stel dat u wilt importeren van gegevens in twee opslag-accounts. U moet afzonderlijke vaste schijven gebruiken voor elk account opslag en afzonderlijke projecten per opslag-account maken.

### <a name="prepare-your-drives"></a>Uw stations voorbereiden

De eerste stap bij het importeren van gegevens met behulp van de Azure importeren/exporteren-service is voor het voorbereiden van uw stations met de functie van de client Azure importeren/exporteren. Volg de onderstaande stappen voor het voorbereiden van uw stations.

1.  Geef aan dat de gegevens die moeten worden geïmporteerd. Dit kan mappen en bestanden van de zelfstandige versie op de lokale server of een netwerkshare zijn.  

2.  Hiermee bepaalt u het aantal stations die afhankelijk van de totale grootte van de gegevens die u nodig hebt. Het vereiste aantal 3,5 inch SATA II/III vaste schijven aanschaffen.

3.  Het doel opslag-account, container, virtuele mappen en BLOB's identificeren.

4.  Hiermee bepaalt u de mappen en/of zelfstandige-bestanden die wordt gekopieerd naar elke harde schijf.

5.  Gebruik het [Hulpprogramma Azure-importeren/exporteren](http://go.microsoft.com/fwlink/?LinkID=301900&clcid=0x409) in uw gegevens kopiëren naar een of meer vaste schijven.

    - Sessies met kopiëren als u wilt uw gegevens uit de bron naar de vaste schijven kopiëren wordt gemaakt door het hulpprogramma Azure importeren/exporteren. Het hulpmiddel kunt één map samen met de bijbehorende submappen of één bestand kopiëren in een sessie één kopie.

    - Mogelijk moet u meerdere kopie sessies als de brongegevens zich uitstrekt over meerdere mappen.

    - Elke harde schijf die u voorbereidt moet ten minste één kopieersessie.

6.  Kunt u de / versleutelen parameter om in te schakelen BitLocker-versleuteling op de harde schijf. U kunt ook kan u ook BitLocker-versleuteling handmatig op de harde schijf en de toets opgeven wanneer u het uitvoert.

7.  Het hulpprogramma Azure importeren/exporteren genereert een station logboek-bestand voor elk station als deze is voorbereid. Het station logboek-bestand is opgeslagen op uw lokale computer, niet op het station zelf. Wanneer u de taak importeren maakt, wordt u het logboek-bestand uploaden. Een station logboek-bestand bevat de ID van de schijf en de toets BitLocker, evenals andere informatie over het station.
**Belangrijk**: elke harde schijf die u voorbereidt zijn ingevoegd om een logboek-bestand. Wanneer u de taak van het importeren met behulp van de portal klassieke maakt, moet u alle logboek-bestanden van de stations die deel van die taak importeren uitmaken uploaden. Stations zonder logboek bestanden niet worden verwerkt.

8.  Wijzig de gegevens op de vaste schijven of het logboek-bestand niet na het voltooien van schijfvoorbereiding van de.

Hieronder vindt u de opdrachten en voorbeelden voor het voorbereiden van de harde schijf met clienthulpprogramma Azure importeren/exporteren.

Azure importeren/exporteren client hulpmiddel PrepImport opdracht voor de eerste sessie kopiëren naar een map kopiëren:

    WAImportExport PrepImport /sk:<StorageAccountKey> /csas:<ContainerSas> /t: <TargetDriveLetter> [/format] [/silentmode] [/encrypt] [/bk:<BitLockerKey>] [/logdir:<LogDirectory>] /j:<JournalFile> /id:<SessionId> /srcdir:<SourceDirectory> /dstdir:<DestinationBlobVirtualDirectory> [/Disposition:<Disposition>] [/BlobType:<BlockBlob|PageBlob>] [/PropertyFile:<PropertyFile>] [/MetadataFile:<MetadataFile>]

**Voorbeeld:**

In het onderstaande voorbeeld we kopieert alle bestanden en mappen van H:\Video naar de harde schijf die is gekoppeld aan X: sub. De gegevens worden geïmporteerd naar de bestemming opslag rekening is opgegeven door de opslagruimte accountsleutel en in de opslagruimte zogenaamd video. Als de container opslag niet aanwezig is, kunt u het wordt gemaakt. Deze opdracht wordt ook opmaken en de doel-hardeschijfstation wilt versleutelen.

    WAImportExport.exe PrepImport /j:FirstDrive.jrn /id:Video1 /logdir:c:\logs /sk:storageaccountkey /t:x /format /encrypt /srcdir:H:\Video1 /dstdir:video/ /MetadataFile:c:\WAImportExport\SampleMetadata.txt

Azure importeren/exporteren client hulpmiddel PrepImport opdracht voor volgende kopie sessies voor het kopiëren van een map:

    WAImportExport PrepImport /j:<JournalFile> /id:<SessionId> /srcdir:<SourceDirectory> /dstdir:<DestinationBlobVirtualDirectory> [/Disposition:<Disposition>] [/BlobType:<BlockBlob|PageBlob>] [/PropertyFile:<PropertyFile>] [/MetadataFile:<MetadataFile>]

Voor sessies met latere kopiëren naar dezelfde harde schijf, dezelfde logboek bestandsnaam opgeven en geef een nieuwe sessie-ID; Er is niet nodig op te geven van de sleutel en doel-station voor de account voor opslag opnieuw, noch voor het opmaken of het station versleutelen. In dit voorbeeld zijn we de map H:\Photo en de mappen sub kopiëren naar hetzelfde doelstation, in de opslagruimte zogenaamd foto.

    WAImportExport.exe PrepImport /j:FirstDrive.jrn /id:Photo /srcdir:H:\Photo /dstdir:photo/ /MetadataFile:c:\WAImportExport\SampleMetadata.txt

Het hulpprogramma Azure importeren/exporteren client PrepImport opdracht voor de eerste sessie kopiëren naar een bestand kopiëren:

    WAImportExport PrepImport /sk:<StorageAccountKey> /csas:<ContainerSas> /t: <TargetDriveLetter> [/format] [/silentmode] [/encrypt] [/bk:<BitLockerKey>] [/logdir:<LogDirectory>] /j:<JournalFile> /id:<SessionId> /srcfile:<SourceFile> /dstblob:<DestinationBlobPath> [/Disposition:<Disposition>] [/BlobType:<BlockBlob|PageBlob>] [/PropertyFile:<PropertyFile>] [/MetadataFile:<MetadataFile>]

Het hulpprogramma Azure importeren/exporteren client PrepImport opdracht voor sessies met latere kopiëren naar een bestand kopiëren:

    WAImportExport PrepImport /j:<JournalFile> /id:<SessionId> /srcfile:<SourceFile> /dstblob:<DestinationBlobPath> [/Disposition:<Disposition>] [/BlobType:<BlockBlob|PageBlob>] [/PropertyFile:<PropertyFile>] [/MetadataFile:<MetadataFile>]

**Onthouden**: standaard als blok BLOB's de gegevens worden geïmporteerd. U kunt de parameter /BlobType gebruiken om gegevens als een BLOB pagina's te importeren. Bijvoorbeeld als u VHD-bestanden die als schijven wordt gekoppeld aan een VM Azure importeert, moet u deze importeren als BLOB van pagina's. Als u niet zeker welke blob Typ om te gebruiken weet, kunt u /blobType:auto en we kunt u het juiste type bepalen. In dit geval alle vhd en vhdx bestanden als BLOB van pagina's worden geïmporteerd en de rest als blok BLOB's worden geïmporteerd.

Zie meer informatie over het gebruik van het hulpprogramma Azure importeren/exporteren client in [Vaste schijven voorbereiden voor importeren](https://msdn.microsoft.com/library/dn529089.aspx).

Bovendien kunt u verwijzen naar de [Voorbeeldwerkstroom naar vaste schijven voorbereiden voor een project importeren](https://msdn.microsoft.com/library/dn529097.aspx) voor meer gedetailleerde stapsgewijze instructies.  

### <a name="create-the-import-job"></a>Maken van de taak importeren

1.  Als u uw harde schijf hebt voorbereid, Ga naar uw account opslag in de [klassieke-portal](https://manage.windowsazure.com) en weergeven van het Dashboard. Klik onder **Snelle bekijken**, klikt u op **maken een project importeren**. Bekijk de stappen en schakel het selectievakje in om aan te geven dat u uw harde schijf hebt gemaakt en dat u het station logboek-bestand beschikbaar hebt.

2.  Geef in stap 1, contactgegevens voor de persoon die verantwoordelijk is voor deze taak importeren en een geldig adres van afzender. Als u opslaan uitgebreide logboekgegevens voor de taak importeren wilt, controleert u de optie voor het **opslaan van de uitgebreide log in mijn 'waimportexport' blob container**.

3.  Upload de station logboek-bestanden die u hebt gekregen tijdens de stap van de voorbereidende station in stap 2. U moet een bestand voor elk station die u hebt voorbereid uploaden.

    ![De functie importeren - stap 3 maken](./media/storage-import-export-service/import-job-03.png)

4.  Voer een beschrijvende naam voor de taak importeren in stap 3. Houd er rekening mee dat de naam die u invoert alleen kleine letters, cijfers, streepjes bevat mogelijk en onderstrepingstekens, moet beginnen met een letter en mag geen spaties bevatten. Gebruikt u de naam die u uw taken bijhouden wilt terwijl ze in voortgang en zodra ze zijn voltooid.

    Selecteer vervolgens uw gegevens center regio in de lijst. Het gegevensgebied center wordt aangegeven de Datacenter en het adres waarnaar u uw pakket moet verzenden. Zie de veelgestelde vragen hieronder voor meer informatie.

5.  Selecteer uw afzender carrier in de lijst en voer uw accountnummer carrier in stap 4. Microsoft gebruikt deze account naar de schijven terug naar u verzenden zodra uw project importeren voltooid is.

    Als u uw volgnummer hebt, selecteer uw vervoerder in de lijst en voer uw volgnummer.

    Als u niet een volgnummer nog hebt, kiest u **ik geef de gegevens van mijn verzending voor deze taak importeren nadat ik mijn pakket hebt verzonden**, klikt u vervolgens het importproces te voltooien.

6. Als u wilt invoeren uw volgnummer nadat u uw pakket, terug te keren naar de pagina **Importeren/exporteren** voor uw account opslag in de portal klassieke hebt verzonden Selecteer uw taak in de lijst en kies **Verzending Info**. Navigeren door de wizard en voer uw volgnummer in stap 2.

    Als de volgnummer niet binnen 2 weken na het maken van de taak wordt bijgewerkt, verloopt de taak.

    Als de taak in de stand maken, verzending of overdragen is, kunt u ook uw accountnummer carrier in stap 2 van de wizard bijwerken. Als u de taak hebt in de verpakking staat, kunt u uw accountnummer carrier voor die taak niet bijwerken.

7. U kunt de taakvoortgang van uw bijhouden op het dashboard van de portal. Zie wat betekent de taakstatus van elke in de vorige sectie door te [bekijken van uw taakstatus](#viewing-your-job-status).

## <a name="how-to-create-an-export-job"></a>Het maken van een taak exporteren?

Maak een taak voor het exporteren de service importeren/exporteren melding dat u hebt verzenden een of meer lege stations Datacenter voor de zodat gegevens kunnen worden geëxporteerd vanuit uw account, opslag naar de stations en de stations en vervolgens naar u worden verzonden.

### <a name="prepare-your-drives"></a>Uw stations voorbereiden

Volgende oude controles worden aanbevolen voor het voorbereiden van uw stations voor een taak voor het exporteren:

1. Het aantal schijven die nodig zijn met de waarde van het hulpprogramma Azure importeren/exporteren PreviewExport opdracht controleren. Zie [Station gebruik afdrukvoorbeelden bekijken voor een taak exporteren](https://msdn.microsoft.com/library/azure/dn722414.aspx)voor meer informatie. U kunt bekijken schijf gebruik voor de BLOB's die u hebt geselecteerd, op basis van de grootte van de stations die u wilt gebruiken.

2. Controleer of u kunt alleen-lezen/schrijven naar de harde schijf die worden verzonden voor de taak exporteren.

### <a name="create-the-export-job"></a>Maken van de taak exporteren

1.  Maken van een taak voor het exporteren, Ga naar uw account opslag in de [klassieke-portal](https://manage.windowsazure.com)en weergeven van het Dashboard. Klik onder **Snelle bekijken**, klikt u op **een taak exporteren maken** en volg de stappen in de wizard.

2.  Geef de contactgegevens voor de persoon die verantwoordelijk zijn voor deze taak exporteren in stap 2. Als u opslaan uitgebreide logboekgegevens voor de taak exporteren wilt, controleert u de optie voor het **opslaan van de uitgebreide log in mijn 'waimportexport' blob container**.

3.  Opgeven welke blob-gegevens die u wilt exporteren vanuit uw account opslagruimte aan uw leeg station of stations in stap 3. U kunt alle blob-gegevens in het account opslag exporteren of u kunt opgeven welke BLOB's of sets BLOB's als u wilt exporteren.

    Als u wilt opgeven een blob exporteren, de Gegevenskiezer **Gelijk aan** , en geeft u het relatieve pad naar de blob, beginnend met de containernaam van de. Gebruik *$root* om op te geven van de hoofdsite container.

    Om op te geven alle BLOB's beginnen met een voorvoegsel, gebruikt u de kiezer **Begint met** en geef het voorvoegsel, beginnend met een slash '/'. Het voorvoegsel mogelijk het voorvoegsel van de containernaam van de, de containernaam van de volledige of de containernaam van de volledige gevolgd door het voorvoegsel van de naam van de blob.

    De volgende tabel ziet u voorbeelden van geldige blob paden:

    Gegevenskiezer|BLOB pad|Beschrijving
    ---|---|---
    Begint met|/|Hiermee exporteert u alle BLOB's in de opslagruimte-account
    Begint met|/$root /|Hiermee exporteert u alle BLOB's in de container hoofdsite
    Begint met|/Book|Hiermee exporteert u alle BLOB's in een container die met voorvoegsel **adresboek begint**
    Begint met|/Music/|Hiermee exporteert u alle BLOB's in de container **muziek**
    Begint met|/ muziek/liefde|Hiermee exporteert u alle BLOB's in de container **muziek** die met het voorvoegsel **graag mee werkt beginnen**
    Gelijk aan|$root/logo.bmp|Exports blob **logo.bmp** in de container hoofdsite
    Gelijk aan|videos/Story.mp4|Exports blob **story.mp4** in container **video 's**

    U kunt de paden blob in geldige indelingen om fouten te voorkomen tijdens de verwerking moet opgeven, zoals in deze schermafbeelding.

    ![Taak voor het exporteren - stap 3 maken](./media/storage-import-export-service/export-job-03.png)


4.  Voer een beschrijvende naam voor de taak exporteren in stap 4. De naam die u invoert mogen alleen kleine letters, cijfers, streepjes en onderstrepingstekens, moet beginnen met een letter en mag geen spaties bevatten.

    Het gegevensgebied beheercentrum geeft aan het datacenter waarnaar u uw pakket moet verzenden. Zie de veelgestelde vragen hieronder voor meer informatie.

5.  Selecteer uw afzender carrier in de lijst in stap 5 en voer uw accountnummer carrier. Microsoft gebruikt deze account aan uw schijven terug naar u verzenden zodra uw project exporteren voltooid is.

    Als u uw volgnummer hebt, selecteer uw vervoerder in de lijst en voer uw volgnummer.

    Als u niet een volgnummer nog hebt, kiest u **ik geef de gegevens van mijn verzending voor deze taak exporteren nadat ik mijn pakket hebt verzonden**, voert u het exportproces.

6. Als u wilt invoeren uw volgnummer nadat u uw pakket, terug te keren naar de pagina **Importeren/exporteren** voor uw account opslag in de portal klassieke hebt verzonden Selecteer uw taak in de lijst en kies **Verzending Info**. Navigeren door de wizard en voer uw volgnummer in stap 2.

    Als de volgnummer niet binnen 2 weken na het maken van de taak wordt bijgewerkt, verloopt de taak.

    Als de taak in de stand maken, verzending of overdragen is, kunt u ook uw accountnummer carrier in stap 2 van de wizard bijwerken. Als u de taak hebt in de verpakking staat, kunt u uw accountnummer carrier voor die taak niet bijwerken.

    > [AZURE.NOTE] Als de blob wilt exporteren in gebruik op het moment van kopiëren naar de harde schijf is, wordt Azure importeren/exporteren service een momentopname van de blob en de momentopname kopiëren.

7.  U kunt de taakvoortgang van uw bijhouden op het dashboard in de klassieke-portal. Zie wat betekent de taakstatus van elke in de vorige sectie "uw taakstatus weergeven' op.

8.  Nadat u de stations met uw geëxporteerde gegevens ontvangt, kunt u deze kunt weergeven en de BitLocker sleutels die zijn gegenereerd door de service voor uw harde schijf kopiëren. Ga naar uw account opslag in de klassieke-portal en klik op het tabblad importeren/exporteren. Selecteer uw taak voor het exporteren in de lijst en klik op de knop weergave toetsen. De toetsen BitLocker worden weergegeven, zoals hieronder wordt weergegeven:

    ![Weergave BitLocker sleutels voor de taak voor het exporteren](.\media\storage-import-export-service\export-job-bitlocker-keys.png)

Ga via het gedeelte Veelgestelde vragen onder terwijl deze de meest voorkomende vragen die klanten optreden bij het gebruik van deze service bedekt.

## <a name="frequently-asked-questions"></a>Veelgestelde vragen ##


**Hoe lang duurt het kopiëren van mijn gegevens nadat de stations het datacenter bereikt?**

De tijd om te kopiëren varieert afhankelijk van andere factoren zoals taaktype, type en het formaat van de gegevens die wordt gekopieerd, grootte van schijven biedt, en bestaande werkbelasting. Deze kan variëren van een paar dagen aan een paar weken, afhankelijk van deze factoren. Daarom moeilijk te bieden een algemene raming. De service probeert te optimaliseren van uw werk door te kopiëren van meerdere stations parallel indien mogelijk. Als u een taak van de kritieke importeren/exporteren tijd hebt, contact maken met ons voor een schatting.

**Wanneer moet ik Azure-Service voor importeren/exporteren gebruiken?**
Een rekening moet houden via Azure importeren exporteren als uploaden of downloaden via netwerk duurt ongeveer als meer dan zeven dagen schatting. U kunt berekenen hoe lang het duurt met behulp van een online Rekenmachine of door te [downloaden van](https://github.com/Azure-Samples/storage-dotnet-import-export-job-management/archive/master.zip) het account bevinden in onze Azure importeren exporteren REST API steekproef in Azure voorbeelden opslagplaats bij [Gegevens overbrengen snelheid Rekenmachine](https://github.com/Azure-Samples/storage-dotnet-import-export-job-management/blob/master/DataTransferSpeedCalculator.html). Dit is geen berekening van een exacte, maar alleen een ruw aanduiding.

**Kan ik de service Azure importeren/exporteren gebruiken met een resourcemanager opslag-account?**

Nee, niet kunt u gegevens kopiëren naar of een resourcemanager opslag account rechtstreeks met de service Azure importeren/exporteren. De service biedt alleen ondersteuning voor klassieke opslag-accounts. Ondersteuning voor resourcemanager opslag-account wordt binnenkort beschikbaar. Als alternatief, kunt u gegevens importeren in een klassieke opslag-account en migreren naar uw resourcemanager opslag-account via [AzCopy](storage-use-azcopy.md) of CopyBlob.

**Kan ik kopiëren Azure-bestanden met de service Azure importeren/exporteren?**

Nee, de importeren/exporteren van Azure-service biedt alleen ondersteuning voor blok BLOB's en pagina BLOB's. Alle andere soorten opslag inclusief Azure-bestanden, tabellen en wachtrijen worden niet ondersteund.

**Is de service Azure importeren/exporteren beschikbaar voor CSP abonnementen?**

Nee, de service Azure importeren/exporteren ondersteunt geen CSP abonnementen. De ondersteuning wordt in de toekomst toegevoegd.

**Kan ik het overslaan station voorbereiden voor een project importeren of kan ik een station voorbereiden zonder te kopiëren?**

Een station dat u verzenden wilt voor het importeren van gegevens u moet voorbereiden met de functie van de client Azure importeren/exporteren. U moet de client gebruiken om gegevens te kopiëren naar het station.

**Heb ik nodig om uit te voeren schijfvoorbereiding van een bij het maken van een taak voor het exporteren?**

Geen, maar sommige oude controles wordt aanbevolen. Het aantal schijven die nodig zijn met de waarde van het hulpprogramma Azure importeren/exporteren PreviewExport opdracht controleren. Zie [Station gebruik afdrukvoorbeelden bekijken voor een taak exporteren](https://msdn.microsoft.com/library/azure/dn722414.aspx)voor meer informatie. U kunt bekijken schijf gebruik voor de BLOB's die u hebt geselecteerd, op basis van de grootte van de stations die u wilt gebruiken. Ook controleren dat u kunt lezen en schrijven naar de harde schijf die worden verzonden voor de taak exporteren.

**Wat gebeurt er als ik een harde schijf die niet voldoet per ongeluk verzenden naar de ondersteunde vereisten?**

De Azure Datacenter retourneert het station dat niet voldoet aan de ondersteunde vereisten voor u. Als er slechts enkele van de stations in het pakket overeenkomen met de ondersteuningsvereisten, deze stations worden verwerkt en wordt de stations die niet aan de voorwaarden worden geretourneerd voor u.

**Kan ik mijn taak annuleren?**

U kunt een taak annuleren wanneer de status ervan is maken of verzending.

**Hoe lang kan ik de status van voltooide taken weergeven in de klassieke portal?**

U kunt het veld status voor voltooide taken voor 90 dagen weergeven. Voltooide taken worden verwijderd na 90 dagen.

**Wat moet ik doen als ik wil importeren of exporteren van meer dan 10 stations?**

Eén importeren of exporteren taak kan verwijzen naar slechts 10 stations in één taak voor de service importeren/exporteren. Als u meer dan 10 schijven verzenden wilt, kunt u meerdere taken maken. Stations die zijn gekoppeld aan dezelfde taak moeten samen in hetzelfde pakket worden verzonden.

**Kan ik een USB-SATA-adapter die zich niet in de aanbevolen lijst gebruiken?**

Als er een conversieprogramma die hierboven niet wordt genoemd, probeert u kunt het hulpprogramma voor het importeren/exporteren van Azure met uw conversieprogramma voor het voorbereiden van het station en kijk of deze werkt voordat u een ondersteunde conversieprogramma aanschaft uitvoeren.

**De service de stations voordat deze wordt geretourneerd opmaken?**

Nee. Alle stations zijn versleuteld met BitLocker.

**Kan ik stations voor importeren/exporteren taken kopen bij Microsoft?**

Nee. U moet verzenden van uw eigen stations voor beide importeren en exporteren van taken.

**Nadat de taak importeren is voltooid, wat wordt mijn gegevens eruit in het opslag-account? Mijn mapstructuur behouden?**

Als u een harde schijf voorbereidt voor een project importeren, de bestemming is opgegeven door de /dstdir: parameter. Dit is de bestemmingscontainer in de opslagruimte account waaraan u gegevens uit de harde schijf wordt gekopieerd. Virtuele mappen worden gemaakt voor mappen van de harde schijf en BLOB's worden gemaakt voor bestanden in deze bestemmingscontainer.

**Als het station bestanden die al aanwezig in Mijn opslag-account heeft, de service overschreven bestaande BLOB's in Mijn opslag-account?**

Als u het station voorbereidt, kunt u opgeven of de doelbestanden moeten worden overschreven of /Disposition genegeerde met de parameter genoemd: < naam | Nee-overschrijven | overschrijven >. Standaard de service wordt de naam van de nieuwe bestanden wijzigen in plaats van bestaande BLOB's overschrijven.

**Het hulpprogramma Azure importeren/exporteren client compatibel is met 32-bits besturingssystemen?**
Nee. Het clienthulpprogramma is alleen compatibel met 64-bits Windows-besturingssystemen. Raadpleeg de sectie besturingssystemen voor servers in de [minimumvereisten](#pre-requisites) voor een volledige lijst met ondersteunde OS versies.

**Moet ik iets anders dan de harde schijf in mijn pakket opnemen?**

Verzend alleen uw vaste schijven. Neem niet items zoals voeding levering kon of USB-kabels.

**Moet ik mijn schijven met FedEx of knop compatibiliteitsvoorkeuren voor Excel verzenden?**

U kunt schijven Datacenter voor de met een bekende carrier zoals FedEx, knop compatibiliteitsvoorkeuren voor Excel, UPS of US Postal Service verzenden. Echter voor het verzenden van de stations terug naar u vanaf de Datacenter, moet u opgeven een accountnummer FedEx in de Verenigde Staten en de Europese Unie, of een getal van de knop compatibiliteitsvoorkeuren voor Excel-account in de regio's Azië en Australië.

**Gelden er beperkingen met mijn station internationaal verzending?**

Houd er rekening mee dat de fysieke media die u levert mogelijk moet cross internationale randen. U bent verantwoordelijk voor dat uw fysieke media en de gegevens zijn geïmporteerd en/of de toepasselijke wetgeving geëxporteerd. Voordat u de fysieke media levert, informeert u bij uw adviseur zijn om te bevestigen dat uw media en gegevens kunnen wettelijk Datacenter voor de geïdentificeerd worden verzonden. Hierdoor om ervoor te zorgen dat Microsoft tijdig worden bereikt.

**Wanneer u een taak maakt, is het verzendadres een locatie die verschilt van mijn opslaglocatie van het account. Wat moet ik doen?**

Sommige opslaglocaties voor account zijn toegewezen aan alternatieve verzending locaties. Eerder kunnen beschikbare verzending locaties ook worden tijdelijk toegewezen aan andere locaties. Altijd controleren het verzendadres opgegeven tijdens het maken van de taak voordat u uw stations levert.

**Waarom wordt mijn taakstatus in de klassieke portal Zeg 'verzending' wanneer de website Carrier mijn pakket ziet wordt bezorgd?**

De status in de portal klassieke gewijzigd van verzending naar overbrengen wanneer het station processing wordt gestart. Als het station heeft gekregen van de functie, maar niet verwerken heeft gestart, wordt uw taakstatus back-upfunctie weergegeven.

**Tijdens het verzenden van mijn station, de vervoerder wordt gevraagd om de naam van contactpersoon data center en telefoonnummer. Wat moet ik bieden?**

Het telefoonnummer dat is voor u beschikbaar tijdens het maken van de taak. Als u de naam van een contactpersoon nodig hebt, neem contact op waimportexport@microsoft.com en we krijgt u deze informatie.

**Kan ik de service Azure importeren/exporteren naar PST-postvakken en SharePoint-gegevens kopiëren naar O365 gebruiken?**

Raadpleeg [importeren PST-bestanden of SharePoint-gegevens naar Office 365](https://technet.microsoft.com/library/ms.o365.cc.ingestionhelp.aspx).

**Kan ik de service Azure importeren/exporteren offline Mijn back-ups kopiëren naar de Azure back-up-Service gebruiken?**

Raadpleeg [de werkstroom Offline back-up in Azure back-up](../backup/backup-azure-backup-import-export.md).

## <a name="see-also"></a>Zie ook:

- [Bij het instellen van het hulpprogramma Azure importeren/exporteren-client](https://msdn.microsoft.com/library/dn529112.aspx)

- [Gegevens met het hulpprogramma voor de opdrachtregel AzCopy overbrengen](storage-use-azcopy.md)

- [Voorbeeld van Azure importeren exporteren REST API](https://azure.microsoft.com/documentation/samples/storage-dotnet-import-export-job-management/)
