<properties
    pageTitle="Inleiding tot opslag | Microsoft Azure"
    description="Een overzicht van Azure opslag, van Microsoft online gegevensopslag in de cloud. Informatie over het gebruik van de beste oplossing van de beschikbare cloud-opslag in uw toepassingen."
    services="storage"
    documentationCenter=""
    authors="tamram"
    manager="carmonm"
    editor="tysonn"/>

<tags
    ms.service="storage"
    ms.workload="storage"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="10/25/2016"
    ms.author="tamram"/>

# <a name="introduction-to-microsoft-azure-storage"></a>Inleiding tot Microsoft Azure-opslag

## <a name="overview"></a>Overzicht

Azure opslag is de cloud-opslag-oplossing voor moderne toepassingen die afhankelijk zijn van de levensduur, beschikbaarheid en schaalbaarheid om te voldoen aan de behoeften van hun klanten. Lees dit artikel, ontwikkelaars, IT-professionals en zakelijke beslissing vind makers informatie over:

- Wat Azure opslag is en hoe u gebruik kunt maken in de cloud, mobiele, server en desktoptoepassingen kunt uitvoeren
- Welke soorten gegevens die u kunt opslaan met de services Azure Storage: blob-gegevens (object), NoSQL tabelgegevens, berichten en bestandsshares.
- Hoe toegang tot uw gegevens in een Azure-opslag wordt beheerd
- Hoe uw gegevens van Azure Storage duurzame via redundantie en replicatie wordt uitgevoerd
- Waar kunt u vervolgens naar uw eerste Azure Storage-toepassing maken

Zie [aan de slag met Azure opslagruimte in vijf minuten](storage-getting-started-guide.md)om aan de slag met Azure opslagmedia snel.

Zie onderstaande [Vervolgstappen](#next-steps) voor meer informatie over hulpmiddelen, bibliotheken en andere resources voor het werken met Azure-opslag.

## <a name="what-is-azure-storage"></a>Wat is Azure opslag?

Cloud computing kunt nieuwe scenario's voor toepassingen waarbij scalable, duurzame en maximaal beschikbare opslagruimte voor hun gegevens – dat is precies waarom Microsoft Azure Storage ontwikkeld. Naast het geboden die het mogelijk voor ontwikkelaars op grootschalige toepassingen voor de ondersteuning van nieuwe scenario's, Azure opslag ook kunt u het fundament opslag voor Azure virtuele Machines, een verdere testament naar de stabiliteit.

Azure opslag is sterk schaalbare, zodat u kunt opslaan en proces honderden van TB aan gegevens ter ondersteuning van de grote gegevens scenario's wetenschappelijke, financiële analyse en mediatoepassingen vereist. Of u de kleine hoeveelheden gegevens die zijn vereist voor een website voor professionals en kleine bedrijven kunt opslaan. Waar uw behoeften vallen, betaalt u alleen voor de gegevens die u hebt opgeslagen. Azure opslagruimte momenteel worden opgeslagen tientallen trillions unieke klant-objecten en miljoenen aanvragen per seconde gemiddeld verwerkt.

Azure opslag is elastische, zodat u kunt toepassingen voor een groot publiek van de globale ontwerpen en schalen dat deze toepassingen naar wens - de hoeveelheid gegevens die zijn opgeslagen en het aantal aanvragen ten opzichte van deze. U betaalt alleen voor wat u gebruikt en alleen als u deze gebruikt.

Azure opslag gebruikt een auto-partitioneren systeem die automatisch laden-saldi uw gegevens op basis van verkeer. Dit betekent dat als de tijd aan uw toepassing vergroten, Azure Storage automatisch toegewezen de juiste bronnen van deze doelstellingen.

Azure opslag is toegankelijk zijn vanuit een willekeurige plek in de wereld van elk type van toepassing, ongeacht of deze wordt uitgevoerd in de cloud, op het bureaublad, op een on-premises implementatie-server of op een mobiele of tablet-apparaat. U kunt Azure-opslag in mobiele scenario's waarin de toepassing bevat een subset van gegevens op het apparaat en deze synchroniseert met een volledige reeks gegevens die zijn opgeslagen in de cloud.

Azure opslag ondersteunt klanten met behulp van een uitgebreide reeks besturingssystemen (met inbegrip van Windows en Linux) en een verscheidenheid aan programming talen (inclusief .NET, Java, Node.js, Python, Ruby, PHP en C++ en mobiele programming talen) voor de ontwikkeling van handige. Azure opslag beschrijft ook gegevensbronnen via eenvoudige REST API's, die beschikbaar zijn voor elke kunnen verzenden en ontvangen van gegevens via HTTP-/ HTTPS-client.

Azure Premium opslagruimte biedt krachtige, lage latentie schijfondersteuning voor i/o-intensief werkbelasting op Azure virtuele Machines uitgevoerd. Met Azure Premium opslag, kunt u meerdere permanente gegevensschijven als bijlage toevoegen aan een virtuele machine en deze configureren om te voldoen aan de vereisten van uw prestaties. Elke gegevensschijf wordt ondersteund door een schijf SSD in Azure Premium opslag voor maximale o prestaties. Zie [Premium opslag: High-Performance-opslag voor Azure virtuele machines werkbelasting](storage-premium-storage.md) voor meer informatie.

## <a name="introducing-the-azure-storage-services"></a>Inleiding tot de opslagservices Azure

Azure opslagruimte biedt de volgende vier diensten: Blob storage, Table storage wachtrij opslag en bestandsopslag.

- BLOB Storage ongestructureerde objectgegevens worden opgeslagen. Een blob mag elk gewenst type tekst of binaire gegevens, zoals een document, mediabestand of installatieprogramma van toepassing zijn. Blobopslag is ook Object opslag genoemd.
- Table Storage opgeslagen gestructureerde gegevenssets. Tabelopslag is een NoSQL sleutel-kenmerk gegevensopslag, waarmee voor snelle ontwikkeling en snelle toegang tot grote hoeveelheden gegevens.
- Wachtrij opslagruimte biedt betrouwbare messaging voor communicatie tussen onderdelen van cloudservices en voor de verwerking van de werkstroom.
- Bestandsopslag biedt gedeelde opslag van oudere toepassingen via het standaard SMB-protocol. Azure virtuele machines en cloudservices kunnen gegevens uit een bestand delen in toepassingsonderdelen via gekoppelde waarden voor aandelen en on-premises implementatie toepassingen hebben toegang tot gegevens uit een bestand in een delen via de service bestand REST API.

Een account Azure opslag is een veilige account waarmee u toegang hebt tot de services in Azure opslag. Uw opslag-account bevat de unieke naamruimte voor uw opslagruimte resources. De onderstaande afbeelding ziet u de relaties tussen de opslagbronnen Azure in een opslag-account:

![Azure opslag Resources](./media/storage-introduction/storage-concepts.png)

[AZURE.INCLUDE [storage-account-types-include](../../includes/storage-account-types-include.md)]

[AZURE.INCLUDE [storage-versions-include](../../includes/storage-versions-include.md)]

## <a name="blob-storage"></a>-Blobopslag

Voor gebruikers met grote hoeveelheden gegevens ongestructureerde object om op te slaan in de cloud, biedt blobopslag een efficiënt en scalable oplossing. U kunt blobopslag gebruiken om inhoud te slaan, zoals:

- Documenten
- Sociale gegevens zoals foto's, video's, muziek en blogs
- Back-ups van bestanden, computers, databases en apparaten
- Afbeeldingen en tekst voor webtoepassingen
- Configuratiegegevens voor cloud-toepassingen
- Grote gegevens, zoals Logboeken en andere grote gegevenssets

Elke blob is ingedeeld in een container. Containers bieden ook een handige manier beveiligingsbeleid voor apparaten toewijzen aan groepen van objecten. Een account opslag bevatten een willekeurig aantal containers en een container een willekeurig aantal BLOB's, tot aan de limiet van de capaciteit 500 TB van het account opslag kan bevatten.  

BLOB storage aanbiedingen drie soorten BLOB's, BLOB's blokkeren, BLOB's en pagina BLOB's (schijven) toevoegen.

- Blok BLOB's zijn geoptimaliseerd voor streaming en cloud-objecten op te slaan en zijn een goede keuze voor het opslaan van documenten, media-bestanden, back-ups enzovoort.
- Toevoegen BLOB's zijn vergelijkbaar met blok BLOB's, maar zijn geoptimaliseerd voor bewerkingen toevoegen. Een toevoegquery blob kan alleen worden bijgewerkt met een nieuw blok toe te voegen aan het einde. Toevoegen BLOB's zijn een goede keuze voor scenario's zoals logboekregistratie, waar nieuwe gegevens moeten worden geschreven alleen naar het einde van de blob.
- Pagina BLOB's zijn geoptimaliseerd voor het weergeven van IaaS schijven en ondersteunende willekeurig schrijft en mogelijk maximaal 1 TB grootte. Een netwerk Azure virtuele machines gekoppeld IaaS schijf een VHD opgeslagen als een pagina-blob is.

Voor zeer grote gegevenssets waar netwerk beperkingen uploaden of downloaden van gegevens met Blob storage via het netwerk onrealistische maken, kunt u een harde schijf naar Microsoft om te importeren of exporteren van gegevens rechtstreeks vanuit het datacenter verzenden. Zie [werken met de Service Microsoft Azure importeren/exporteren om gegevens als u wilt Blob Storage te brengen](storage-import-export-service.md).

## <a name="table-storage"></a>-Tabelopslag

Moderne toepassingen vraag vaak gegevensopslag met grotere schaalbaarheid en flexibiliteit dan vorige gegenereerde items van de software die is vereist. Tabelopslag biedt ten zeerste beschikbaar, sterk schaalbare opslag, zodat de schaal van de toepassing automatisch aanpassen kan om te voldoen aan de gebruikersvraag. Tabelopslag is Microsofts NoSQL sleutel/kenmerk store, maar er een schemaless ontwerp en andere uit traditionele relationele databases. Met een gegevensarchief schemaless is het eenvoudig kunt aanpassen van uw gegevens als de behoeften van uw toepassing evolve. Tabelopslag is eenvoudig te gebruiken, zodat ontwikkelaars snel toepassingen kunnen maken. Toegang tot gegevens is snel en efficiënt voor allerlei soorten toepassingen.  Tabelopslag is meestal aanzienlijk lager in kosten dan traditionele SQL voor soortgelijke hoeveelheden gegevens.

Tabelopslag is een toets-kenmerk winkel, wat betekent dat elke waarde in een tabel met de naam van een getypte eigenschap is opgeslagen. De naam van de eigenschap kan worden gebruikt voor het filteren en het opgeven van selectiecriteria. Een verzameling eigenschappen en bijbehorende waarden bestaat uit een entiteit. Aangezien Table storage schemaless, twee entiteiten in dezelfde tabel kunnen verschillende verzamelingen van eigenschappen bevatten en deze eigenschappen kunnen worden verschillende typen.

U kunt Table storage gebruiken om op te slaan flexibele gegevenssets, zoals gebruikersgegevens voor webtoepassingen, adresboeken, gegevens van een apparaat en een ander type metagegevens die uw service vereist.  U kunt een willekeurig aantal entiteiten opslaan in een tabel en een account opslag mogelijk niet het getal van tabellen, tot aan de capaciteitslimiet van de van het account opslagruimte bevatten.

Net als BLOB's en wachtrijen, ontwikkelaars kunnen beheren en toegang tot tabel opslagruimte met standaard REST protocollen, maar ook ondersteunt een subset van de OData-protocol, Table storage vereenvoudigen geavanceerde mogelijkheden query- en inschakelen van zowel JSON en AtomPub (XML gebaseerd) indelingen.

Voor vandaag Internet-e-toepassingen bieden NoSQL databases zoals Table storage een populaire alternatief voor traditionele relationele databases.

## <a name="queue-storage"></a>Wachtrij opslag

Bij het ontwerpen van toepassingen voor schaal, zijn toepassingsonderdelen vaak ontkoppelde, zodat ze onafhankelijk schaal kunnen aanpassen. Wachtrij opslag kunt u een oplossing van een betrouwbare SMS voor asynchrone communicatie tussen toepassingsonderdelen, ongeacht of ze worden uitgevoerd in de cloud, op het bureaublad, op een on-premises implementatie-server of op een mobiel apparaat. Wachtrij opslag ondersteunt ook asynchroon taken beheren en het bouwen van proces werkstromen.

Een account opslag kan een willekeurig aantal wachtrijen bevatten. Een wachtrij kan een willekeurig aantal berichten, tot aan de capaciteitslimiet van de van het account opslagruimte bevatten. Afzonderlijke berichten zijn mogelijk maximaal 64 KB grootte.

## <a name="file-storage"></a>Bestanden opslaan

Azure bestandsopslag biedt cloudgebaseerde SMB bestandsshares, zodat u kunt oudere toepassingen die afhankelijk van bestandsshares naar Azure snel en zonder dure herschreven zijn migreren. Met Azure bestandsopslag, kunnen applicaties Azure virtuele machines of cloudservices een bestandsshare in de cloud, koppelen, net zoals een bureaubladtoepassing een typisch SMB delen koppelt. Een willekeurig aantal toepassingsonderdelen kunt en vervolgens koppelen en het delen van de opslagruimte bestand tegelijk gebruiken.

Aangezien een bestandsshare opslag een standaard SMB-bestandsshare is,-toepassingen in Azure toegang heeft tot gegevens in de optie voor delen via bestand systeem I/O APIs. Ontwikkelaars kunnen daarom gebruikmaken van hun bestaande code en vaardigheden om te migreren van bestaande toepassingen. IT-professionals kunnen PowerShell-cmdlets gebruiken om te maken, koppelen en bestandsshares opslag beheren als onderdeel van het beheer van Azure-toepassingen.

Als de andere Azure opslagservices beschrijft bestandsopslag een REST API voor toegang tot gegevens in een delen. On-premises-toepassingen kunnen de bestandsopslag REST API voor toegang tot gegevens in een bestandsshare bellen. Op deze manier een onderneming kunt kiezen voor het migreren van Azure sommige oudere toepassingen en doorgaan met anderen binnen hun eigen organisatie. Houd er rekening mee dat een bestandsshare koppelen is alleen mogelijk voor toepassingen die wordt uitgevoerd in Azure wordt aangegeven; een on-premises implementatie-toepassing mogelijk alleen toegang tot het bestand delen via de REST API.

Gedistribueerde toepassingen kunnen bestandsopslag ook gebruiken om te slaan en nuttige toepassingsgegevens en ontwikkeling en testen hulpmiddelen voor delen. Bijvoorbeeld een toepassing mogelijk configuratiebestanden opslaan en diagnostische gegevens zoals Logboeken aan de doelstellingen en vastlopen wordt in een bestand opslag delen, zodat ze beschikbaar zijn voor meerdere virtuele machines of rollen. Ontwikkelaars en beheerders kunnen hulpprogramma's die ze nodig hebben om te maken of beheren van een toepassing in een bestandsshare opslagruimte die beschikbaar is voor alle onderdelen, worden opgeslagen in plaats van ze de installatie op elke VM of exemplaar van de rol.

## <a name="access-to-blob-table-queue-and-file-resources"></a>Toegang tot Blob, tabel, wachtrij en bestand Resources

Standaard alleen de accounteigenaar opslag toegang tot bronnen in de opslagruimte-account. Voor de beveiliging van uw gegevens, moet elke aanvraag ten opzichte van resources in uw account worden geverifieerd. Verificatie is afhankelijk van een model Shared Key. BLOB's kunnen ook worden geconfigureerd voor ondersteuning van anonieme verificatie.

Uw opslag-account is toegewezen twee privé toegangstoetsen maken die worden gebruikt voor verificatie. Hebt twee sleutels zorgt ervoor dat uw toepassing beschikbaar blijft wanneer u regelmatig de toetsen algemene key management veiligheidsoverwegingen opnieuw genereren.

Als u hoeft toe te staan dat gebruikers beheerde toegang tot uw resources opslag, kunt u een gedeelde access-handtekening maken. Een gedeelde access-handtekening (SA's) is een token die kan worden toegevoegd aan een URL waarmee gedelegeerd toegang tot een resource opslag. Iedereen die beschikt over de token hebben toegang tot de bron die deze met de deze Hiermee geeft u de machtigingen voor de periode dat deze geldig is verwijst. Begin met versie 2015-04-05, Azure Storage ondersteunt twee soorten gedeelde toegang handtekeningen: service-SA's en SA's-account.

De service SA's gedelegeerd toegang tot een bron in slechts één van de opslagservices: de service Blob, wachtrij, tabel of bestand.

Een account SA's gedelegeerd toegang tot bronnen in een of meer van de storage-services. U kunt de toegang tot de service level-bewerkingen die niet beschikbaar voor communicatie met een service SA's zijn delegeren. U kunt ook toegang tot lezen, schrijven en verwijderingen op blob containers, tabellen, wachtrijen en bestandsshares die niet zijn toegestaan met een service SA's delegeren.

Tot slot kunt u opgeven dat een container en de BLOB's of een specifieke blob die algemeen toegankelijk zijn. Wanneer u aangeeft dat een container of blob openbare is, iedereen kan worden gelezen anoniem; geen verificatie is vereist.  Openbare containers en BLOB's zijn handig voor de weergave van resources zoals media en documenten die worden gehost op websites.  Als u wilt verlagen netwerklatentie voor een algemene publiek, kunt u blob-gegevens die worden gebruikt door websites met de CDN Azure cache.

Zie [Gebruik van gedeelde Access handtekeningen (SA's)](storage-dotnet-shared-access-signature-part-1.md) voor meer informatie over gedeelde toegang handtekeningen. Zie [anonieme leestoegang tot containers en BLOB's beheren](storage-manage-access-to-resources.md) en [verificatie voor de opslag Azure Services](https://msdn.microsoft.com/library/azure/dd179428.aspx) voor meer informatie over de beveiligde toegang tot uw account opslag.

## <a name="replication-for-durability-and-high-availability"></a>Replicatie voor levensduur en betere beschikbaarheid

De gegevens in uw account van Microsoft Azure opslag is altijd om ervoor te zorgen levensduur en betere beschikbaarheid gerepliceerd. Replicatie wordt gekopieerd van uw gegevens, binnen de dezelfde Datacenter, of naar een tweede Datacenter, dat afhankelijk van de optie herhaling die u kiest. Replicatie uw gegevens beschermen en behoudt u uw toepassing-up-tijd in het geval van problemen met de tijdelijke hardware. Als uw gegevens worden gerepliceerd naar een tweede Datacenter, beveiligt dat ook u uw gegevens ten opzichte van een volledige uitval in de primaire locatie.

Replicatie zorgt ervoor dat uw account opslag voldoet aan de [Service Level Agreement (SLA) voor opslag,](https://azure.microsoft.com/support/legal/sla/storage/) zelfs licht fouten. Zie dat de SLA voor informatie over Azure opslag garandeert voor levensduur en beschikbaarheid. 

Wanneer u een opslag-account hebt gemaakt, kunt u een van de volgende replicatieopties selecteren:  

- **Lokaal overtollige opslagruimte (LRS).** Lokaal redundante opslag onderhoudt drie kopieën van uw gegevens. LRS wordt drie keer binnen één datacenter in één regio gerepliceerd. LRS beveiligt uw gegevens van problemen met de normale hardware, maar niet vanuit het mislukken van één datacenter.  

    LRS wordt aangeboden met een korting. Voor maximale levensduur, wordt u aangeraden die u geografische-redundante opslag gebruikt, hieronder beschreven.


- **Zone-redundante opslagruimte (ZRS).** Zone-redundante opslag onderhoudt drie kopieën van uw gegevens. Er is ZRS gerepliceerd drie keer in twee of drie faciliteiten, binnen een enkel of tussen twee regio's, van hoger levensduur dan LRS leveren. ZRS zorgt ervoor dat uw gegevens duurzame binnen één regio.  

    ZRS biedt een hoger niveau van de levensduur dan LRS; echter voor maximale levensduur raadzaam is dat u geografische-redundante opslag gebruikt, hieronder beschreven.  

    > [AZURE.NOTE] ZRS is momenteel alleen beschikbaar voor blok BLOB's en is alleen ondersteund voor versies 2014-02-14 en hoger.
    >
    > Zodra u hebt gemaakt van uw account voor opslagruimte en ZRS geselecteerd, kunt u deze met een ander type replicatie niet converteren of omgekeerd.

- **Geografische-redundante opslag (GRS)**. GRS onderhoudt zes kopieën van uw gegevens. Met GRS, uw gegevens drie keer binnen de primaire regio is gerepliceerd en tevens driemaal in een secundaire gebied honderden van mijl weg van het primaire regio leveren van het hoogste niveau van levensduur worden gerepliceerd. Bij een storing bij de primaire regio wordt Azure Storage failover naar de tweede regio. GRS zorgt ervoor dat uw gegevens duurzame in twee afzonderlijke regio's.

    Zie voor informatie over primaire en secundaire koppelingen per regio, [Azure regio's](https://azure.microsoft.com/regions/).

- **Leestoegang geografische-redundante opslag (AB-GRS)**. Geografische-redundante opslag leestoegang uw gegevens worden gerepliceerd naar een secundaire geografische locatie en biedt ook alleen toegang tot uw gegevens in de secundaire locatie. Geografische-redundante opslag van leestoegang kunt u toegang tot uw gegevens uit de primaire of de secundaire locatie, in het geval dat een locatie niet meer beschikbaar. Leestoegang geografische-redundante opslag is de standaardoptie voor uw account opslag al dan niet standaard wanneer u deze hebt gemaakt. 

    > [AZURE.IMPORTANT] U kunt wijzigen hoe uw gegevens worden gerepliceerd nadat uw opslag-account is gemaakt, tenzij u hebt opgegeven ZRS wanneer u het account hebt gemaakt. Bedenk wel dat u in rekening de overdracht van een extra eenmalige gegevens gebracht mogelijk als u van LRS naar GRS of AB-GRS overstapt kosten.

Zie [Azure Storage replicatie](storage-redundancy.md) voor meer informatie over de opslag replicatie-opties.

Zie [Azure opslag prijzen](https://azure.microsoft.com/pricing/details/storage/)voor prijsgegevens voor opslag-account herhaling. Zie [Azure regio's](https://azure.microsoft.com/regions/#services) voor meer informatie over welke services beschikbaar in elke regio zijn.

Zie voor architectuur meer informatie over levensduur met Azure opslagmedia, [SOSP papier - Azure Storage: A ten zeerste beschikbaar Cloudopslagservice met sterke consistentie](http://blogs.msdn.com/b/windowsazurestorage/archive/2011/11/20/windows-azure-storage-a-highly-available-cloud-storage-service-with-strong-consistency.aspx).


## <a name="transferring-data-to-and-from-azure-storage"></a>Gegevens overbrengen naar en vanuit Azure opslag

U kunt het hulpprogramma AzCopy gebruiken om te kopiëren blob, bestand en gegevens in een tabel in uw account opslag of meerdere opslag-accounts. Zie [overbrengen van gegevens met het hulpprogramma AzCopy-opdrachtregelopties](storage-use-azcopy.md) voor meer informatie.

AzCopy is gebouwd de [Azure gegevens verkeer bibliotheek](https://www.nuget.org/packages/Microsoft.Azure.Storage.DataMovement/), die is momenteel beschikbaar in de Preview-versie.

De Azure importeren/exporteren-service biedt een manier om blobgegevens importeren in of blob-gegevens exporteren vanuit uw opslag-account via een harde schijf schijf verzonden naar het beheercentrum Azure-gegevens. Zie voor meer informatie over het importeren/exporteren-service [gebruiken de Service Microsoft Azure importeren/exporteren naar gegevens met Blob Storage overbrengen](storage-import-export-service.md).

## <a name="pricing"></a>Prijzen

[AZURE.INCLUDE [storage-account-billing-include](../../includes/storage-account-billing-include.md)]

## <a name="storage-apis-libraries-and-tools"></a>Opslag-API's, bibliotheken en hulpprogramma 's

Azure opslag resources kunnen worden geopend door een taal die HTTP/HTTPS-verzoeken kunt aanbrengen. Azure opslagruimte biedt bovendien programming bibliotheken voor verschillende populaire talen. Deze bibliotheken vereenvoudigen veel aspecten van het werken met Azure opslagruimte op de details zoals synchrone en asynchrone aanroep voor de verwerking, batchen bewerkingen, uitzondering management, automatische nieuwe pogingen, operationele gedrag enzovoort. Bibliotheken zijn momenteel beschikbaar zijn voor de volgende talen en platforms, met anderen in de pijplijn:

### <a name="azure-storage-data-services"></a>Azure opslag-gegevensservices

- [Opslagservices REST API](http://msdn.microsoft.com/library/azure/dd179355.aspx)
- [Opslag clientbibliotheek voor .NET, Windows Phone en Windows Runtime](https://www.nuget.org/packages/WindowsAzure.Storage/)
- [Bibliotheek voor opslag-Client voor C++](https://github.com/Azure/azure-storage-cpp)
- [Opslag clientbibliotheek voor Java/Android](/develop/java/)
- [Bibliotheek voor opslag-Client voor Node.js](http://dl.windowsazure.com/nodestoragedocs/index.html)
- [Bibliotheek voor opslag-Client voor PHP](/develop/php/)
- [Bibliotheek voor opslag-Client voor Ruby](/develop/ruby/)
- [Bibliotheek voor opslag-Client voor Python](/develop/python/)
- [Opslag-Cmdlets voor PowerShell 1.0](https://msdn.microsoft.com/library/azure/mt269418.aspx)

### <a name="azure-storage-management-services"></a>Azure Storage Management-Services

- [Opslag Resource Provider REST API verwijzing](https://msdn.microsoft.com/library/azure/mt163683.aspx)
- [Opslag Resource Provider clientbibliotheek voor .NET](https://msdn.microsoft.com/library/azure/mt131037.aspx)
- [Opslag Resource Provider Cmdlets voor PowerShell 1.0](https://msdn.microsoft.com/library/azure/mt607151.aspx)
- [Opslag Service Management REST API (klassieke)](https://msdn.microsoft.com/library/azure/ee460790.aspx)

### <a name="azure-storage-data-movement-services"></a>Azure opslag verkeer gegevensservices

- [Opslag importeren/exporteren Service REST API](https://msdn.microsoft.com/library/azure/dn529096.aspx)
- [Opslag gegevens verkeer clientbibliotheek voor .NET](https://www.nuget.org/packages/Microsoft.Azure.Storage.DataMovement/)

### <a name="tools-and-utilities"></a>Hulpprogramma's en hulpprogramma 's

- [Azure opslag Explorer](http://go.microsoft.com/fwlink/?LinkID=822673&clcid=0x409)
- [Azure opslag clienthulpprogramma 's](storage-explorers.md)
- [Azure SDK's en hulpprogramma's voor](https://azure.microsoft.com/tools/)
- [Azure opslag Emulator](http://www.microsoft.com/download/details.aspx?id=43709)
- [Azure PowerShell](../powershell-install-configure.md)
- [Hulpprogramma voor de opdrachtregel AzCopy](http://aka.ms/downloadazcopy)

## <a name="next-steps"></a>Volgende stappen

Meer informatie over de opslag van Azure, bekijk het volgende materiaal:

### <a name="documentation"></a>Documentatie

- [Azure opslag documentatie](https://azure.microsoft.com/documentation/services/storage/)

### <a name="for-administrators"></a>Voor beheerders

- [Gebruik van Azure PowerShell met Azure opslag](storage-powershell-guide-full.md)
- [Gebruik van Azure CLI met Azure opslag](storage-azure-cli.md)

### <a name="for-net-developers"></a>Voor .NET-ontwikkelaars

- [Aan de slag met Azure-blobopslag met .NET](storage-dotnet-how-to-use-blobs.md)
- [Aan de slag met Azure-tabelopslag met .NET](storage-dotnet-how-to-use-tables.md)
- [Aan de slag met Azure wachtrij opslagruimte met .NET](storage-dotnet-how-to-use-queues.md)
- [Aan de slag met Azure bestandsopslag in Windows](storage-dotnet-how-to-use-files.md)

### <a name="for-javaandroid-developers"></a>Voor ontwikkelaars Java/Android

- [Het gebruik van Java-blobopslag](storage-java-how-to-use-blob-storage.md)
- [Het gebruik van Java-tabelopslag](storage-java-how-to-use-table-storage.md)
- [Het gebruik van de wachtrij opslag van Java](storage-java-how-to-use-queue-storage.md)
- [Het gebruik van bestandsopslag van Java](storage-java-how-to-use-file-storage.md)

### <a name="for-nodejs-developers"></a>Voor ontwikkelaars van Node.js

- [Het gebruik van Node.js-blobopslag](storage-nodejs-how-to-use-blob-storage.md)
- [Het gebruik van Table storage uit Node.js](storage-nodejs-how-to-use-table-storage.md)
- [Het gebruik van de wachtrij opslag van Node.js](storage-nodejs-how-to-use-queues.md)

### <a name="for-php-developers"></a>Voor PHP-ontwikkelaars

- [Het gebruik van PHP-blobopslag](storage-php-how-to-use-blobs.md)
- [Het gebruik van Table storage uit PHP](storage-php-how-to-use-table-storage.md)
- [Het gebruik van de wachtrij opslag van PHP](storage-php-how-to-use-queues.md)

### <a name="for-ruby-developers"></a>Voor Ruby ontwikkelaars

- [Het gebruik van Ruby-blobopslag](storage-ruby-how-to-use-blob-storage.md)
- [Het gebruik van Ruby-tabelopslag](storage-ruby-how-to-use-table-storage.md)
- [Hoe wachtrij opslag van Ruby gebruiken](storage-ruby-how-to-use-queue-storage.md)

### <a name="for-python-developers"></a>Voor ontwikkelaars Python

- [Het gebruik van Python-blobopslag](storage-python-how-to-use-blob-storage.md)
- [Het gebruik van Table storage uit Python](storage-python-how-to-use-table-storage.md)
- [Het gebruik van de wachtrij opslag van Python](storage-python-how-to-use-queue-storage.md)
- [Het gebruik van bestandsopslag uit Python](storage-python-how-to-use-file-storage.md)
