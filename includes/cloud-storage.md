#<a name="data-management-and-business-analytics"></a>Gegevensbeheer en Business Analytics

Beheren en analyseren van gegevens in de cloud is net zo belangrijk als ergens anders. Als u wilt dat u dit kunt doen, biedt Azure een reeks technologieën voor het werken met relationele en niet-relationele gegevens. In dit artikel maakt u kennis met elk van de opties. 

##<a name="table-of-contents"></a>Inhoudsopgave      

- [-Blobopslag](#blob)
- [Een DBMS uitgevoerd in een virtuele Machine](#dbinvm)
- [SQL-Database](#sqldb)
    - [Synchronisatie van de SQL-gegevens](#datasync)
    - [Rapportage van SQL-gegevens met virtuele Machines](#datarpt)
- [Table Storage](#tblstor)
- [Hadoop](#hadoop)

## <a name="blob"></a>-Blobopslag

Het woord "blob" is kort voor "BLOB" en wordt deze exact beschreven wat een blob is: een verzameling binaire gegevens. BLOB's zijn nog een nuttig Hoewel deze eenvoudige. [Afbeelding 1](#Fig1) ziet u de basisbeginselen van Azure-blobopslag.

<a name="Fig1"></a>![Diagram van BLOB 's][blobs]
 
**Afbeelding 1: Azure-blobopslag opslaan van binaire gegevens - BLOB's - in containers.**

Als u wilt BLOB's gebruiken, moet u eerst een Azure *opslag-account*maken. Als onderdeel van dit, moet u het Azure datacenter waarin de objecten die u samenstelt met dit account wordt opgeslagen opgeven. Waar deze zich bevindt, wordt elke blob die u maakt hoort bij sommige container in uw account opslag. Voor toegang tot een blob, biedt een toepassing een URL met het formulier:

http://&lt;*StorageAccount*&gt;.blob.core.windows.net/&lt;*Container*&gt;/&lt;*BlobName*&gt;

&lt;*StorageAccount* &gt; is een unieke id toegewezen wanneer een nieuw account voor de opslag is gemaakt, terwijl &lt; *Container* &gt; en &lt; *BlobName* &gt; zijn de namen van een specifieke container en een blob in die container. 

Azure biedt twee verschillende soorten BLOB's. De opties zijn:

- BLOB's, die elk kan maximaal 200 GB aan gegevens bevatten *blokkeren* . Als de naam wordt gesuggereerd, wordt een blok blob onderverdeeld in een aantal blokken. Als een fout optreedt tijdens het overzetten van een blok blob, kan opnieuw worden doorgestuurd hervatten met de meest recente blokkeren in plaats van de hele blob opnieuw te verzenden. Blok BLOB's zijn een helemaal algemene aanpak opslag en ze het type van de meest gebruikte blob vandaag bent.

- *Pagina* BLOB's, dat zo groot op één terabyte kunnen zijn. Pagina BLOB's zijn ontworpen voor RAM en dus elkaar in een aantal pagina's is verdeeld. Een toepassing is gratis te lezen en schrijven van afzonderlijke pagina's willekeurig in de blob. In Azure virtuele Machines, bijvoorbeeld VMs die u maakt gebruiken BLOB van pagina's als permanente opslag voor zowel de OS schijven en de gegevensschijven.

Of u blok BLOB's of pagina BLOB's kiest, toepassingen toegang heeft tot blob-gegevens op verschillende manieren. De opties onder andere het volgende:

- Rechtstreeks via een RESTful (dat wil zeggen op basis van een HTTP) toegang tot protocol. Zowel Azure-toepassingen en externe toepassingen, waaronder apps met on-premises, kunnen deze optie gebruiken.
- De bibliotheek Azure opslag Client, die beschikt u over een interface meer ontwikkelaars-vriendelijke boven aan het onbewerkte RESTful blob access protocol. Nogmaals, zowel Azure-toepassingen en externe toepassingen hebben toegang tot BLOB's met deze bibliotheek.
- Azure-sticks, geen optie waarmee een Azure-toepassing een pagina-blob behandelen als een lokaal station met een NTFS gebruiken. Met de toepassing, is de pagina blob ziet eruit als een gewone Windows-bestandssysteem geopend met standaard bestand I/O. Ja, gelezen en geschreven zijn verzonden naar de onderliggende pagina blob waarmee het Azure-station. 

Als u wilt voorkomen problemen met de hardware en beschikbaarheid te verbeteren, is elke blob gerepliceerd in drie computers in een Azure datacenter. Schrijven naar een blob bijgewerkt alle drie exemplaren, zodat u later lezen niet inconsistente resultaten weergegeven. U kunt ook opgeven dat de gegevens van een blob moeten worden gekopieerd naar een andere Azure datacenter in de dezelfde geografische maar ten minste 500 mijl afwezig. Deze kopiëren, genaamd *geografische herhaling*, gebeurt er dan binnen enkele minuten van een update van de blob en het is handig voor herstel.

Gegevens in BLOB's kunnen ook worden aangebracht beschikbaar via de Azure *Inhoud bezorging netwerk (CDN)*. Door caching kopieën van blob-gegevens op tientallen servers overal ter wereld, kunt de CDN snellere toegang tot informatie die meerdere keren wordt geopend. 

Eenvoudige zoals ze zijn, zijn BLOB's in de juiste keuze in veel gevallen. Opslaan en streaming van video en audio zijn duidelijke voorbeelden, zoals back-ups en andere soorten gegevens archiveren zijn. Ontwikkelaars kunnen ook BLOB's voor elk type ongestructureerde gegevens die ze willen gebruiken. Met een eenvoudige manier om te slaan en toegang tot de binaire gegevens kan verrassend handig zijn.


## <a name="dbinvm"></a>Een DBMS uitgevoerd in een virtuele Machine

Veel toepassingen vertrouwen vandaag op een soort management DBMS (database system). Relationele systemen zoals SQL Server zijn de meestgebruikte keuze, maar niet-relationele benaderingen, ook wel bekend als *NoSQL* technologieën elke dag populairder ontvangt. Als u wilt dat de cloud-toepassingen gebruiken van deze opties voor het beheer van gegevens, kunt u een DBMS uitvoeren met Azure virtuele Machines (relationele of NoSQL) in een VM. [Afbeelding 2](#Fig2) ziet u hoe dit eruitziet met SQL Server.

<a name="Fig2"></a>![Diagram van SQL Server in een virtuele Machine][SQLSvr-vm]
 
**Afbeelding 2: Azure virtuele Machines kunt een DBMS uitgevoerd in een VM, met permanente verstrekt door BLOB's.**

Voor ontwikkelaars en databasebeheerders van de eruitziet dit scenario veel dezelfde software uitgevoerd in hun eigen datacenter. In het voorbeeld hieronder, bijvoorbeeld bijna alle mogelijkheden van SQL Server kan worden gebruikt en er volledige beheerderstoegang tot het systeem. U hebt de verantwoordelijkheid voor het beheer van de database-server, natuurlijk ook alsof deze lokaal zijn uitgevoerd.

[Afbeelding 2](#Fig2) wordt weergegeven, uw databases worden weergegeven op de lokale schijf van de server wordt uitgevoerd in VM kunnen worden opgeslagen. Achter de echter is elk van deze schijven geschreven naar een Azure blob. (Dit is vergelijkbaar met het gebruik van een SAN in uw eigen datacenter, met een blob daardoor veel als een LUN.) Als met een Azure blob, de gegevens erin wordt drie keer binnen gerepliceerd een datacenter en, als u dit, geografische gerepliceerd naar een ander datacenter in dezelfde regio aanvraagt. Het is ook mogelijk om te gebruiken van de opties zoals SQL Server-database voor verbeterde betrouwbaarheid spiegelen. 

Een andere manier om het gebruik van SQL Server in een VM is om te maken van een hybride-toepassing, waar de gegevens worden opgeslagen op Azure terwijl de toepassingslogica wordt uitgevoerd voor on-premises implementatie. Bijvoorbeeld wellicht dit zinvol wanneer toepassingen die worden uitgevoerd op meerdere locaties of op verschillende mobiele apparaten moeten dezelfde gegevens deelt. Als u de communicatie tussen de cloud-database en on-premises logica eenvoudiger, kunt een organisatie Azure Virtual Network een VPN (VPN) verbinding maken tussen een Azure datacenter en een eigen datacenter van de on-premises implementatie.


## <a name="sqldb"></a>SQL-Database

Voor veel personen is een DBMS uitgevoerd in een VM de eerste optie die bedenken kunt voor het beheren van gestructureerde gegevens in de cloud. Het is niet alleen de mogelijkheid, echter evenmin deze altijd de beste keus. In sommige gevallen, het beheren van gegevens met behulp van een Platform als een Service (PaaS) benadering relevant meer. Azure biedt een PaaS technologie met de naam van de SQL-Database waarmee u deze stap herhalen voor relationele gegevens. [Afbeelding 3](#Fig3) ziet u deze optie. 

<a name="Fig3"></a>![Diagram van een SQL-Database][SQL-db]
 
**Afbeelding 3: SQL-Database biedt een gedeelde PaaS relationele storage-service.**

SQL-Database geven niet elke klant een eigen fysieke exemplaar van SQL Server. In plaats daarvan biedt het een service meerdere tenant, een logische SQL Database-server voor elke klant. Alle klanten delen de opslag en rekenkracht capaciteit waarmee de service. En als u met de Blob Storage, zorgen dat alle gegevens in SQL-Database is opgeslagen op drie afzonderlijke computers binnen een Azure datacenter, zodat uw databases ingebouwde beschikbaarheid (HA).

Met een toepassing eruit SQL-Database veel SQL Server. Toepassingen kunnen actie-SQL-query's op relationele tabellen, gebruik T-SQL opgeslagen procedures en transacties uitvoeren op meerdere tabellen. En omdat toepassingen toegang SQL-Database via het protocol tabelvormige gegevens Stream (TDS), hetzelfde protocol gebruikt tot voor toegang tot SQL Server, ze kunnen werken met gegevens met behulp van entiteit Framework, ADO.NET JDBC en andere vertrouwde data access-interfaces. 

Maar omdat SQL-Database een cloudservice die wordt uitgevoerd in Azure datacenters is, u niet nodig hebt voor het beheren van een van de fysieke aspecten van het systeem, zoals gebruik van de schijf. Moet u ook geen zorgen over het bijwerken van de software of andere laag niveau beheertaken voor de verwerking te maken. Nog steeds besturingselementen voor elke klantorganisatie zijn eigen databases, natuurlijk, inclusief hun schema's en gebruikersaanmeldingen, maar veel van de toegepast beheertaken voor u klaar bent. 

Terwijl de SQL-Database eruit ziet veel SQL Server voor toepassingen, werken niet het precies hetzelfde als een DBMS uitvoeren op een fysieke of virtuele machine. Omdat deze op gedeelde hardware wordt uitgevoerd, worden de prestaties is met de belasting van die hardware door alle van zijn klanten. Dit betekent dat de prestaties van, bijvoorbeeld een opgeslagen procedure in SQL-Database van één dag naar de volgende variëren kan. 

Tegenwoordig kunnen kunt SQL-Database u een database voor het vasthouden van maximaal 150 gigabytes maken. Als u werken met grotere databases wilt, biedt de service een optie genaamd *Federatie*. Klik hiertoe maakt de databasebeheerder van een twee of meer *leden Federatie*, die elk een aparte database maken met een eigen schema is. Gegevens wordt verdeeld over deze leden, iets vaak waarnaar wordt verwezen als *sharding*, met elk lid van een unieke *Federatie sleutel*toegewezen. Een toepassing problemen SQL-query's ten opzichte van deze gegevens door het opgeven van de toets Federatie waarmee het lid Federatie de query moeten het doel. Dit kunt met behulp van een traditionele relationele benadering met grote hoeveelheden gegevens. Wees altijd er voor-en nadelen; query's noch transacties kunnen Federatie leden, bijvoorbeeld beslaan. Maar wanneer een relationele PaaS-service de beste keuze is en deze voor-en nadelen toegestaan zijn, een goede oplossing kan zijn als u een SQL-Federatie gebruiken.

SQL-Database kunnen worden gebruikt door van toepassingen Azure of elders, zoals in uw on-premises implementatie-datacenter. Dit is het handig voor cloud-toepassingen die moeten van relationele gegevens, evenals on-premises implementatie toepassingen die u profiteren kunt van de gegevens opslaat in de cloud. Een mobiele toepassing kan zijn afhankelijk van de SQL-Database voor het beheren van gedeelde relationele gegevens, bijvoorbeeld als mogelijk een voorraad-toepassing die wordt uitgevoerd op meerdere wederverkopers overal ter wereld.

Na te denken over SQL-Database verheft een duidelijke (en belangrijke) probleem: wanneer moet u SQL Server in een VM en wanneer SQL-Database is een goede keuze worden uitgevoerd? Zoals u gewend bent, zijn er voor-en nadelen en dus welke methode u betere is afhankelijk van uw vereisten. 

Er is een eenvoudige manier om na te denken over het SQL-Database weergeven als voor de nieuwe toepassingen, terwijl SQL Server in een VM een betere keus is wanneer u een bestaande on-premises implementatie toepassing in de cloud verplaatst. Het kan handig zijn nagaan deze beschikking in een meer fijnmazige manier, maar ook zijn. Bijvoorbeeld: SQL-Database is eenvoudig te gebruiken, omdat er minimale instellen en beheren. Maar bieden prestaties met SQL Server in een VM kan hebben - het is niet een gedeelde service - en daarmee ook niet-federatief database groter dan SQL-Database. SQL-Database maakt nog steeds, ingebouwde herhaling van gegevens en verwerking, effectief waardoor u een hoge beschikbaarheid DBMS met zeer klein werk. Terwijl de SQL Server kunt u meer controle en een enigszins uitgebreidere set opties, is het SQL-Database eenvoudiger instellen en aanzienlijk minder werk te beheren.

Ten slotte, is het belangrijk te weten dat de SQL-Database niet de enige PaaS gegevensservice beschikbaar op Azure is. Microsoft-partners bieden ook andere opties. Bijvoorbeeld biedt ClearDB een MySQL PaaS aanbod, terwijl Cloudant een optie voor NoSQL verkoopt. Gegevensservices PaaS de juiste oplossing in veel gevallen zijn, en deze aanpak voor het beheren van gegevens is een belangrijk onderdeel van Azure.


### <a name="datasync"></a>Synchronisatie van de SQL-gegevens

Terwijl de SQL-Database drie kopieën van elke database binnen een één datacenter van de Azure behouden, niet wordt deze automatisch gegevens repliceren tussen Azure datacenters. In plaats daarvan, biedt deze synchroniseren in de SQL-gegevens, een service die u kunt u dit wilt doen. [Afbeelding 4](#Fig4) ziet u hoe dit eruitziet.

<a name="Fig4"></a>![Diagram van de synchronisatie van de SQL-gegevens][SQL-datasync]
 
**Afbeelding 4: SQL gegevens synchroniseren synchroniseert gegevens in SQL-Database met gegevens in andere Azure en on-premises datacenters.**

Als het diagram wordt weergegeven, kunt SQL gegevens synchroniseren gegevens op verschillende locaties synchroniseren. Stel dat u gebruikt een toepassing in meerdere Azure datacenters, bijvoorbeeld met gegevens die zijn opgeslagen in SQL-Database. Synchronisatie van de SQL-gegevens kunt u die gegevens te synchroniseren. Synchronisatie van de SQL-gegevens kunt ook gegevens tussen een Azure datacenter en een exemplaar van SQL Server wordt uitgevoerd in een datacenter van de on-premises implementatie synchroniseren. Dit kan zijn handig voor zowel een lokale kopie van de gegevens die worden gebruikt door on-premises implementatie-toepassingen en de kopie van een wolk die worden gebruikt door toepassingen op Azure. En hoewel dit niet wordt weergegeven in de afbeelding, synchronisatie van de SQL-gegevens kunnen ook worden gebruikt om gegevens tussen SQL-Database en SQL Server wordt uitgevoerd in een VM op Azure of ergens anders te synchroniseren.

Synchronisatie bidirectionele kan zijn en u bepalen precies welke gegevens worden gesynchroniseerd en hoe vaak deze klaar. (Synchronisatie tussen databases niet atomaire, - er is echter altijd ten minste enige tijd duren.) En maar deze wordt gebruikt, het instellen van synchronisatie met SQL gegevens synchroniseren is geheel configuratie gebaseerde; Er is geen code te schrijven.


### <a name="datarpt"></a>Rapportage van SQL-gegevens met virtuele Machines

Nadat u een database gegevens bevat, wilt iemand waarschijnlijk rapporten maken met die gegevens. Azure kan worden uitgevoerd SQL Server Reporting Services (SSRS) in Azure virtuele Machines die functioneel gelijk is aan lokale SQL Server Reporting Services wordt uitgevoerd. Vervolgens kunt u SSRS rapporten uitvoeren op gegevens die zijn opgeslagen in een Azure SQL-Database.  [Afbeelding 5](#Fig5) ziet de werking van het proces.

<a name="Fig5"></a>![Diagram van de SQL-rapportage][SQL-report]
 
**Afbeelding 5: SQL Server Reporting Services is uitgevoerd in een Azure virtuele Machines biedt reporting services voor gegevens in SQL-Database. .**

Voordat u een gebruiker kan zien van een rapport, definieert iemand wat dat rapport ziet er als (stap 1). Met SQL Server Reporting Services op een VM, dit kunt doen met twee methoden: SQL Server Data Tools, onderdeel van SQL Server 2012 of de voorafgaande taak, Business Intelligence (BI) Development Studio. Als u met de SQL Server Reporting Services, worden deze rapportdefinities in het rapport RDL (Definition Language) uitgedrukt. Nadat u de RDL-bestanden voor een rapport hebt gemaakt, worden ze geüpload naar een VM in de cloud (stap 2). De rapportdefinitie is nu klaar voor gebruik.

Een gebruiker van de toepassing wordt vervolgens het rapport (stap 3). De toepassing dit verzoek worden doorgegeven aan de SSRS VM (stap 4), die de SQL-Database of andere gegevensbronnen om de benodigde gegevens van contactpersonen (stap 5). SSRS worden deze gegevens gebruikt en de relevante RDL-bestanden worden weergegeven door het rapport (stap 6), klikt u vervolgens het rapport wordt met de toepassing (stap 7), die wordt deze weergegeven voor de gebruiker (stap 8).

Het insluiten van een rapport in een toepassing, de enige optie is niet het geval, scenario. Het is ook mogelijk om te bekijken van rapporten in Report Manager op de VM, SharePoint op de VM of op andere manieren. Rapporten kunnen ook worden gecombineerd, met één rapport met een koppeling naar een andere.

SQL Server Reporting Services op een VM Azure kunt u volledige functionaliteit als rapportage oplossing in de cloud. Rapporten kunnen elke gegevensbron wordt ondersteund door SSRS gebruiken. Toepassingen en rapporten kunnen opnemen ingesloten code of samenstellen ter ondersteuning van aangepast gedrag. Rapport worden uitgevoerd en de weergave zijn snel omdat rapport serverinhoud en -engine samen op dezelfde virtuele server worden uitgevoerd.



## <a name="tblstor"></a>Table Storage

Relationele gegevens is handig in de meeste gevallen, maar het is niet altijd de juiste keuze. Als uw toepassing snel en eenvoudig toegang tot zeer grote hoeveelheden losjes gestructureerde gegevens moet, bijvoorbeeld een relationele database werkt mogelijk niet goed. Een NoSQL-technologie is waarschijnlijk een betere optie.

Azure-tabelopslag is een voorbeeld van dit soort NoSQL methode. Nadat u de naam, biedt geen Table Storage ondersteuning van standaard relationele tabellen. In plaats daarvan bedoeld deze voor als een *sleutelwaarde opslaan*, een reeks gegevens koppelen aan een bepaalde toets en vervolgens de toegang tot een toepassing laten die gegevens doordat de toets. [Afbeelding 6](#Fig6) ziet u de basisbeginselen.

<a name="Fig6"></a>![Diagram van-tabelopslag][SQL-tblstor]
 
**Afbeelding 6: Azure Table Storage is een sleutelwaarde/winkel biedt snel en eenvoudig toegang tot grote hoeveelheden gegevens.**

Zoals BLOB's is elke tabel gekoppeld aan een account Azure opslag. Tabellen zijn ook veel benoemde zoals BLOB's, met een URL van het formulier

http://&lt;*StorageAccount*&gt;.table.core.windows.net/&lt;*tabelnaam*&gt;

Als de afbeelding ziet u, is elke tabel in een aantal partities, elk van die kan worden opgeslagen op een aparte computer verdeeld. (Dit is een soort sharding, net als met SQL-Federatie.) Een tabel met het RESTful OData-protocol of de bibliotheek Azure opslag Client toegang tot zowel Azure-toepassingen en toepassingen die ergens anders worden uitgevoerd.

Elke partition in een tabel bevat een aantal *entiteiten*, elk met maximaal 255 *Eigenschappen*. Elke eigenschap heeft een naam, een type (zoals binair getal, Bool, DateTime, integer of tekenreeks) en een waarde. In tegenstelling tot relationele opslag, deze tabellen hebben geen vaste schema en dus verschillende entiteiten in dezelfde tabel kunnen eigenschappen met verschillende typen bevatten. Één entiteit mogelijk alleen een tekenreekseigenschap met een naam, bijvoorbeeld terwijl een andere entiteit in dezelfde tabel heeft twee Int eigenschappen met een klant-id-nummer en de classificatie van een creditcard.

Als u wilt identificeren in een bepaalde entiteit binnen een tabel, biedt een toepassing van deze entiteit-toets. De toets bestaat uit twee delen: een *partitiesleutel* waarmee een specifieke partition en een *rijsleutel* die een entiteit binnen die partition aangeeft. In [afbeelding 6](#Fig6), bijvoorbeeld de client vraagt de entiteit met partition A en rijsleutel 3 en Table Storage geeft als resultaat die entiteit, inclusief alle eigenschappen bevat.

Deze structuur kunt tabellen groot - één tabel kan maximaal 100 TB aan gegevens - bevatten en het biedt snel toegang tot de bijbehorende gegevens. Dit ook kunt u beperkingen, echter. Er is bijvoorbeeld biedt geen ondersteuning voor transacties updates die tabellen of zelfs partities in één tabel omvatten. Een reeks updates aan een tabel kan alleen worden gegroepeerd in een atomic-transacties als alle van de betrokken entiteiten in de dezelfde partition zijn. Er wordt ook geen manier om query's in een tabel op basis van de waarde van de eigenschappen, noch is er ondersteuning voor joins in meerdere tabellen. En in tegenstelling tot relationele databases, tabellen biedt geen ondersteuning voor opgeslagen procedures hebben.

Azure-tabelopslag is een goede keuze voor toepassingen die snelle, goedkope toegang tot grote hoeveelheden losjes gestructureerde gegevens hebben. Bijvoorbeeld, een Internet-toepassing die worden opgeslagen profielgegevens voor een groot aantal gebruikers tabellen gebruiken. Snelle toegang is belangrijk in dit geval en de toepassing waarschijnlijk de volledige kracht van SQL niet nodig. Krijgt van deze functionaliteit voor het krijgen van de snelheid en de grootte kunt soms zinvol en Table Storage is alleen de juiste oplossing voor enkele problemen.


## <a name="hadoop"></a>Hadoop

Organisaties hebben datawarehouses samenstellen tientallen jaren te horen. Deze collecties van gegevens, meestal die zijn opgeslagen in relationele tabellen om mensen te werken met om te zien van gegevens op verschillende manieren. Met SQL Server, bijvoorbeeld wordt meestal hulpmiddelen gebruiken zoals SQL Server Analysis Services u dit wilt doen.

Maar Stel dat u wilt analyse van niet-relationele gegevens uitvoeren. Uw gegevens kan duren veel formulieren: gegevens uit sensoren of RFID-codes, logboekbestanden in server-farms, clickstream-gegevens die zijn gemaakt met de webtoepassingen van afbeeldingen van medische diagnostische hulpmiddelen en meer. Deze gegevens mogelijk ook grote, te groot voor effectief worden gebruikt met een traditionele datawarehouse. Problemen met grote gegevens als volgt, niet vaak voorkomen slechts een paar jaar geleden nu wel heel normaal.

Dit soort groot gegevens analyseren, onze industrie nog grotendeels resultaat heeft geleverd op één oplossing: de technologie open source Hadoop. Hadoop wordt uitgevoerd op een cluster van fysieke of virtuele machines verspreiding van de gegevens die deze op op deze computers werkt en parallel verwerkt. De meer machines Hadoop heeft om te gebruiken, des te sneller kunt voltooien datgene werkt dit doet.

Dit soort probleem is een natuurlijke geschikte optie voor de openbare cloud. In plaats van een bord van on-premises implementatie servers die mogelijk inactief veel van de tijd, Hadoop uitgevoerd in de cloud, kunt u maken (en betaald wordt voor) VMs onderhouden alleen wanneer u deze nodig hebt. Beter nog, informatie en meer van de grote gegevens die u wilt analyseren met Hadoop is gemaakt in de cloud, zodat u de problemen van deze verplaatsen. Als u deze synergie, biedt Microsoft een Hadoop-service op Azure. [Afbeelding 7](#Fig7) ziet u de belangrijkste onderdelen van deze service.

<a name="Fig7"></a>![Diagram van hadoop][hadoop]

**Afbeelding 7: Hadoop op Azure voert taken uit MapReduce die gegevens parallel met meerdere virtuele machines verwerken.**

Als u wilt gebruiken Hadoop op Azure, vragen u eerst dit cloud-platform een cluster Hadoop maken die het aantal VMs die u nodig hebt. Het instellen van een Hadoop-cluster uzelf is een niet-alledaagse taak en dus laten Azure doen voor u relevant is. Wanneer u klaar bent met het cluster, u deze afsluiten. Er is niet nodig om te betalen voor berekeningscluster resources die u niet gebruikt.

Een Hadoop-toepassing, ook wel genoemd een *taak*, wordt een programming model bekend als *MapReduce*gebruikt. Als de afbeelding wordt weergegeven, de logica voor een taak MapReduce wordt uitgevoerd tegelijk over veel VMs. Gegevensverwerking parallel, kunt Hadoop analyseren gegevens veel sneller dan één computer oplossingen.

Op Azure, de gegevens een taak werkt op MapReduce wordt meestal bewaard in blobopslag. In Hadoop verwacht MapReduce taken echter gegevens worden opgeslagen in de *Hadoop Distributed bestand System (HDFS)*. HDFS is vergelijkbaar met Blob Storage in enkele manieren; Deze gegevens worden gerepliceerd over meerdere fysieke servers, bijvoorbeeld. In plaats van deze functionaliteit dupliceren, stelt Hadoop op Azure-blobopslag tot en met de API HDFS, in plaats daarvan als de afbeelding ziet. Terwijl de logica in een taak MapReduce denkt dat deze toegang heeft tot gewone HDFS-bestanden dat, wordt de taak is in feite werken met gegevens streamen ernaar uit BLOB's. En als u wilt de hoofdletters/kleine letters waarin meerdere taken worden uitgevoerd op basis van de gegevens wordt ondersteund, Hadoop op Azure ook toestaan gegevens van BLOB's te kopiëren naar volledige HDFS uitgevoerd in de VMs. 

MapReduce taken zijn meestal geschreven in Java vandaag, een aanpak die ondersteuning biedt voor Hadoop op Azure. Microsoft heeft ondersteuning voor het maken van MapReduce taken in andere talen, waaronder C#, F # en JavaScript ook toegevoegd. Het doel is om deze technologie groot gegevens makkelijker toegankelijk zijn voor een grotere groep ontwikkelaars te voeren.

Samen met HDFS en MapReduce bevat Hadoop andere technologieën die mensen die gegevens analyseren zonder het schrijven van een taak MapReduce zelf toestaan. Varken is bijvoorbeeld een hoog niveau taal ontworpen voor het analyseren van grote gegevens, terwijl de component biedt een SQL-achtige taal HiveQL genoemd. Varken zowel component daadwerkelijk MapReduce taken die HDFS gegevens verwerken genereren, maar ze deze complexiteit van hun gebruikers verbergen. Beide worden geleverd bij Hadoop op Azure.

Microsoft biedt ook een HiveQL-stuurprogramma voor Excel. Een Excel-invoegtoepassing gebruikt, kunnen bedrijfsanalisten maken HiveQL query's (en dus MapReduce taken) rechtstreeks vanuit Excel, en vervolgens verwerken en het gebruik van PowerPivot en andere Excel-invoegtoepassingen resultaten visualiseren. Hadoop op Azure bevat andere technologieën, zoals de machine learning-bibliotheken Mahout, het graph mining systeem Pegasus en meer.

Groot gegevensanalyse is belangrijk en dus Hadoop ook belangrijk is. Dankzij Hadoop als beheerde service op Azure, samen met koppelingen naar vertrouwde hulpprogramma's zoals Excel, beoogd Microsoft deze technologie toegankelijk te maken een uitgebreidere set gebruikers.

Breder, gegevens van alle soorten belangrijk is. Daarom Azure bevat tal van opties voor gegevens bedrijfsprocessen te beheren en analyses. Ongeacht welke toepassing die u probeert te maken, is het mogelijk dat vindt u iets in dit cloud-platform die geschikt is voor u.

[blobs]: ./media/cloud-storage/Data_01_Blobs.png
[SQLSvr-vm]: ./media/cloud-storage/Data_02_SQLSvrVM.png
[SQL-db]: ./media/cloud-storage/Data_03_SQLdb.png
[SQL-datasync]: ./media/cloud-storage/Data_04_SQLDataSync.png
[SQL-report]: ./media/cloud-storage/Data_05_SQLReporting.png
[SQL-tblstor]: ./media/cloud-storage/Data_06_TblStorage.png
[hadoop]: ./media/cloud-storage/Data_07_Hadoop.png
