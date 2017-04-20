<properties
    pageTitle="Opslag van Azure Premium: Ontwerpen voor de prestaties | Microsoft Azure"
    description="Krachtige toepassingen met Azure Premium Storage ontwerpen. Premium opslagruimte biedt krachtige, lage latentie schijfondersteuning voor ik/O-intensief werkbelasting op Azure virtuele Machines uitgevoerd."
    services="storage"
    documentationCenter="na"
    authors="aungoo-msft"
    manager="tadb"
    editor="tysonn" />

<tags
    ms.service="storage"
    ms.workload="storage"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/18/2016"
    ms.author="aungoo"/>

# <a name="azure-premium-storage-design-for-high-performance"></a>Opslag van Azure Premium: Ontwerp voor krachtige

## <a name="overview"></a>Overzicht  
In dit artikel bevat richtlijnen voor het samenstellen van krachtige toepassingen met Azure Premium opslag. Hier kunt u de instructies in dit document gecombineerd met aanbevolen procedures prestaties van toepassing op technologieën die worden gebruikt door de toepassing. Om te illustreren de richtlijnen, hebben we SQL Server wordt uitgevoerd op de Premium-opslag als voorbeeld in het gehele document gebruikt.

Hoewel we prestaties scenario's voor de laag opslagruimte in dit artikel, moet u de toepassingslaag optimaliseren. Als u een SharePoint-Farm op Azure Premium Storage host, kunt u bijvoorbeeld de SQL Server-voorbeelden van dit artikel gebruiken optimaliseren van de database-server. Daarnaast kunnen optimaliseren van de SharePoint-Farm webserver en toepassingsserver om de meeste prestaties.

In dit artikel wordt antwoord volgen Veelgestelde vragen over het optimaliseren van de toepassingsprestaties op Azure Premium opslag, help

-   Hoe kan ik de toepassingsprestaties van uw meten?  
-   Waarom ziet u niet verwacht krachtige?  
-   Welke factoren van invloed op de prestaties van uw toepassingen op Premium opslagruimte?  
-   Hoe deze factoren invloed hebben op prestaties van uw toepassing op Premium opslagruimte?  
-   Hoe kunt u optimaliseren voor IO's / s, bandbreedte en latentie?  

We hebben deze richtlijnen die specifiek voor de Premium-opslag omdat werkbelasting waarop Premium opslag zeer gevoelige prestaties zijn. We hebben voorbeelden opgegeven wanneer van toepassing. U kunt enkele van deze richtlijnen ook toepassen op toepassingen op IaaS VMs met standaard schijven.

Voordat u begint, als u nog niet eerder met Premium opslag, lees eerst de [Premium opslag: High-Performance-opslag voor Azure virtuele machines werkbelasting](storage-premium-storage.md) artikel en [Azure Premium opslag schaalbaarheid en prestaties doelen](storage-scalability-targets.md#premium-storage-accounts).

## <a name="application-performance-indicators"></a>Toepassing Performance Indicators  
We gaan of een toepassing wordt uitgevoerd, ook al dan niet gebruik van prestaties indicatoren zoals, hoe snel een toepassing een aanvraag hoeveel gegevens een toepassing wordt verwerkt per aanvraag wordt verwerkt, hoeveel aanvragen de verwerking van een toepassing in een bepaalde periode, hoe lang die een gebruiker er moet wachten om een antwoord na hun aanvraag is. De technische termen voor deze indicatoren prestaties zijn, IO's / s, doorvoer of bandbreedte en latentie.

In dit gedeelte bespreken we de algemene performance indicator in de context van de Premium-opslag. Klik in de volgende sectie verzamelen toepassingsvereisten, leert u hoe u deze performance indicators voor uw toepassing meten. Verderop in prestaties optimaliseren leert u over de factoren die van deze performance indicators en aanbevelingen geoptimaliseerd.

## <a name="iops"></a>IO 'S/S  
IO's / s getal is van aanvragen die uw toepassing worden verzonden naar de schijven in één seconde. Een bewerking invoer/uitvoer kan lezen of schrijven, opeenvolgende of willekeurig. OLTP-toepassingen zoals een website online detailhandel moeten veel gelijktijdige aanvragen direct verwerken. Verzoek van de gebruiker zijn invoegen en bijwerken van intensief databasetransacties, waarin de toepassing snel moet verwerken. Daarom moeten OLTP toepassingen zeer hoog IO's / s. Deze toepassingen verwerken miljoenen kleine en willekeurige IO aanvragen. Als u bijvoorbeeld een toepassing hebt, moet u de toepassing infrastructuur kunt optimaliseren voor IO's / s ontwerpen. In het gedeelte, *Prestaties optimaliseren*wordt besproken in detail alle factoren waarmee u rekening houden moet hoog IO's / s ophalen.

Wanneer u een premium opslagschijf toegevoegd aan uw hoge schaal VM, Azure voorzieningen voor u een gegarandeerd aantal IO's / s aan de hand van de specificatie van de schijf. Een schijf P30 bepalingen bijvoorbeeld 5000 IO's / s. Elke hoog schaal VM grootte heeft ook een specifieke IO's / s limiet die dit kan vangen. Een standaard GS5 VM bevat bijvoorbeeld 80.000 IO's / s beperken.

## <a name="throughput"></a>Doorvoer  
Is de hoeveelheid gegevens die uw toepassing worden verzonden naar de schijven in een opgegeven interval doorvoer of de bandbreedte. Als uw toepassing invoer/uitvoer bewerkingen met grote IO eenheid grootten uitvoert, moet hoge doorvoer. Data warehouse toepassingen meestal actie-scan intensief bewerkingen die toegang tot grote delen van gegevens op een moment en bulkbewerkingen meest worden uitgevoerd. Met andere woorden, moet deze programma's sneller worden verwerkt. Als u bijvoorbeeld een toepassing hebt, moet u de infrastructuur kunt optimaliseren worden verwerkt ontwerpen. Klik in de volgende sectie wordt besproken in detail de factoren u hiervoor moet afstemmen.

Wanneer u een premium opslagschijf toegevoegd aan een hoge schaal VM, Azure bepalingen doorvoer aan de hand van de specificatie van die schijf. Een schijf P30 bepalingen bijvoorbeeld 200 MB per tweede schijf doorvoer. Elke hoog schaal VM grootte heeft ook als specifieke doorvoer limiet die dit kan vangen. Standaard GS5 VM bevat bijvoorbeeld een maximale capaciteit van 2.000 MB per seconde.

Er is een relatie tussen doorvoer en IO's / s zoals wordt weergegeven in de onderstaande formule.

![](media/storage-premium-storage-performance/image1.png)

Daarom is het belangrijk om te bepalen de optimale doorvoer en IO's / s-waarden die moeten worden uw toepassing. Als u probeert te optimaliseren een, wordt de andere ook beïnvloed. Verderop, *Prestaties optimaliseren*bespreken we in daarna nog meer informatie over het optimaliseren van IO's / s en doorvoer.

## <a name="latency"></a>Latentie  
Latentie is de tijd die nodig is een toepassing één aanvraag, verzenden naar schijven voor het verzenden en ontvangen het antwoord op de client. Dit is een kritieke maateenheid voor de prestaties van een toepassing naast IO's / s en doorvoer. De latentie van een premium-opslag-schijf is de tijd die nodig zijn voor het ophalen van de gegevens voor een aanvraag en deze terug naar de toepassing te communiceren. Premium opslagruimte biedt consistente lage vertragingstijden. Als u alleen-lezen-host caching op schijven voor premium inschakelt, kunt u veel lagere gelezen latentie verkrijgen. Bespreken we Caching van schijf in verderop uitgebreider van de *Toepassingsprestaties optimaliseren*.

Wanneer u uw toepassing hoger IO's / s en doorvoer optimaliseert, wordt dit invloed op de latentie van uw toepassing. Na het optimaliseren van de toepassing evalueren u altijd de latentie van de toepassing niet onverwachte hoge latentie gedrag te voorkomen.

## <a name="gather-application-performance-requirements"></a>Verzamel toepassing prestaties vereisten  
De eerste stap bij het ontwerpen van krachtige toepassingen op Azure Premium opslag is voor meer informatie over de vereisten van de prestaties van uw toepassing. Nadat u de prestatie-eisen verzamelen, kunt u uw toepassing naar de meest optimale prestaties optimaliseren.

In de vorige sectie, we de gemeenschappelijke indicatoren van prestaties, IO's / s, doorvoer en latentie uitgelegd. Welke van deze indicatoren prestaties zijn essentieel voor uw toepassing voor het leveren van de gebruikerservaring voor de gewenste, moet u identificeren. Bijvoorbeeld hoog IO's / s de belangrijkste naar miljoenen transacties verwerken in een tweede OLTP-toepassingen. Dat hoge doorvoer kritieke voor het verwerken van grote hoeveelheden gegevens in een tweede Data Warehouse-toepassingen is. Erg laag latentie is essentieel voor realtime-toepassingen zoals live video streaming van websites.

Vervolgens meet de maximale prestaties vereisten van uw toepassing overal in de levensduur. Gebruik de controlelijst steekproef onder als een begin. De vereisten voor maximale prestaties tijdens normaal, piek en rustige uren werkbelasting perioden opnemen. Door te identificeren vereisten voor alle werkbelasting niveaus, is mogelijk om te bepalen de algehele prestaties vereiste van uw toepassing. De normale werklast van de website van een e-commerce zijn bijvoorbeeld de transacties die deze tijdens de meeste dagen in een jaar fungeert. De werklast piek van de website zijn de transacties die deze tijdens de feestdagen of speciale verkoop gebeurtenissen fungeert. De werklast piek is meestal ervaren gedurende een beperkte periode, maar uw toepassing naar de normale werking van twee of meer malen schaal kunt vereisen. Ontdek de 50 percentiel, 90 percentiel en 99 percentiel vereisten. Hiermee kunt een uitschieters in de prestatie-eisen uitfilteren en kunt u de inspanningen zich richten op optimaliseren voor de juiste waarden.

**Controlelijst voor de vereisten van de toepassing prestaties**

| **Vereisten voor prestaties** | **50 percentiel** | **90 percentiel** | **99 percentiel** |
|---|---|---|---|
| Max. Transacties per seconde | | | |
| % Alleen bewerkingen            | | | |
| % Schrijven bewerkingen           | | | |
| % Willekeurig bewerkingen          | | | |
| % Opeenvolgende bewerkingen      | | | |
| Grootte van de aanvraag IO              | | | |
| Gemiddelde doorvoer           | | | |
| Max. Doorvoer              | | | |
| Min. Latentie                 | | | |
| Latentie van gemiddeld              | | | |
| Max. CPU                     | | | |
| Gemiddelde CPU                  | | | |
| Max. Geheugen                  | | | |
| Gemiddelde geheugen               | | | |
| Wachtrij diepteas                  | | | |

>**Belangrijke opmerking:**  
>U rekening moet houden schaalbaarheid van deze getallen op basis van de verwachte toekomstige groei van uw toepassing. Dit is een goed idee om plannen voor groei ligt tijd, omdat het lastiger zijn kan om te wijzigen van de infrastructuur voor later verbeteren.

Als u een bestaande toepassing hebt en u wilt verplaatsen naar de Premium-opslag, moet u eerst de controlelijst hierboven voor de bestaande toepassing maken. Vervolgens een prototype van de toepassing op Premium opslag maken en ontwerpen van de toepassing op basis van richtlijnen die worden beschreven in de *Toepassingsprestaties optimaliseren* in verderop in dit document. Het volgende gedeelte worden de hulpmiddelen die u kunt de prestatiemetingen verzamelen.

Een controlelijst maken die vergelijkbaar is met uw bestaande toepassing voor de prototype. Met Benchmarking hulpmiddelen kunt u de werkbelasting simuleren en prestaties over de toepassing prototype meten. Zie de sectie over [Benchmarking](#benchmarking) voor meer informatie. Hierdoor kunt u bepalen of de Premium-opslag kunt voldoen aan of groter zijn dan uw vereisten voor de prestaties van toepassing. Vervolgens kunt u dezelfde richtlijnen voor de productietoepassing implementeren.

### <a name="counters-to-measure-application-performance-requirements"></a>Items toepassing prestaties vereisten meten  
De beste manier om de prestaties vereisten van uw toepassing, meten is prestaties-hulpprogramma's verstrekt door het besturingssysteem van de server. U kunt PerfMon voor Windows en iostat gebruiken voor Linux. Deze hulpmiddelen vastleggen items die overeenkomt met elke maateenheid die in de bovenstaande sectie worden beschreven. Wanneer uw toepassing wordt uitgevoerd de normaal, piek en rustige uren werkbelasting, moet u de waarden van deze items vastleggen.

De items voor prestatiecontrole zijn beschikbaar voor processor, geheugen en elke logische schijf en de fysieke schijf van de server. Wanneer u schijven voor premium met een VM gebruikt, de fysieke schijf-items zijn voor elke premium opslag-schijf en items voor logische schijf zijn voor elk volume gemaakt op de premium-schijven. U moet de waarden voor de schijven die als host van uw toepassing werkbelasting vastleggen. Als er een toewijzing van één tussen logische en fysieke schijven, kunt u verwijzen naar fysieke schijf-items; Anders raadpleegt u de items voor logische schijf. De opdracht iostat genereert op Linux, een rapport CPU- en gebruik. De schijf gebruik rapport bevat statistieken per fysieke apparaat of partition. Als u een database-server met de gegevens en log op afzonderlijke schijven hebt, kunt u deze gegevens voor beide schijven verzamelen. Onder de tabel items voor schijven, processor en geheugen wordt beschreven:

| Item | Beschrijving | PerfMon | Iostat |
|---|---|---|---|
| **IO's / s of transacties per seconde** | Aantal i/o-aanvragen uitgegeven naar de schijf opslagruimte per seconde. | Schijf lezen per seconde <br> Schijf schrijven per seconde | tps <br> r/s <br> w/s |
| **Schijf lezen en schrijven** | % van lezen en schrijven bewerkingen die zijn uitgevoerd op de schijf. | Percentage schijf alleen tijd <br> Percentage schijf schrijven tijd | r/s <br> w/s |
| **Doorvoer** | Hoeveelheid gegevens lezen van of naar de schijf per seconde geschreven. | Schijf gelezen Bytes per seconde <br> Geschreven Bytes/seconde | kB_read/s <br> kB_wrtn/s |
| **Latentie** | Totale tijd in beslag een verzoek voor een schijf IO. | Gemiddelde schijf sec/gelezen <br> Gemiddelde schijf sec/schrijven | Wacht tot <br> svctm |
| **IO grootte** | De grootte van I/O aanvraagt problemen die moeten worden de schijven. | Gemiddelde gelezen Bytes <br> Gemiddelde geschreven Bytes | avgrq-sz |
| **Wachtrij diepteas** | Het aantal uitstaande I/O aanvragen wachten op formulier lezen of schrijven naar de opslagschijf. | Huidige lengte | avgqu-sz |
| **Max. Geheugen** | Geheugen vereist voor het uitvoeren van toepassing soepel | Percentage toegewezen Bytes wordt gebruikt | Vmstat gebruiken |
| **Max. CPU** | Bedrag CPU die zijn vereist voor het uitvoeren van toepassing soepel | % Processortijd | % util |

Meer informatie over [iostat](http://linuxcommand.org/man_pages/iostat1.html) en [PerfMon](https://msdn.microsoft.com/library/aa645516.aspx).


## <a name="optimizing-application-performance"></a>Prestaties optimaliseren  
De belangrijkste factoren die van invloed op de prestaties van een toepassing wordt uitgevoerd Premium opslag zijn aard van IO aanvragen, VM grootte, de grootte van de schijf, aantal schijven, Caching van schijf, Multithreading en wachtrij diepteas. Sommige van deze factoren kunt u bepalen met knoppen verstrekt door het systeem. De meeste toepassingen mogelijk kunt u een optie om de grootte van de IO en wachtrij diepteas direct wijzigen. Als u SQL Server gebruikt, niet kunt u bijvoorbeeld de IO grootte en wachtrij diepte kiezen. SQL Server kiest de optimale IO grootte wachtrij diepteas waarden en om de meeste prestaties. Het verdient voor meer informatie over de effecten van beide soorten factoren voor de prestaties van uw toepassingen, zodat u de juiste resources prestaties behoeften kunt inrichten.

In deze sectie, raadpleegt u de toepassing vereisten controlelijst die u hebt gemaakt, om te bepalen hoeveel u moet uw toepassing optimaliseren. Op basis van die, is mogelijk om te bepalen welke factoren van deze sectie moet u om af te stellen. Te wonen de effecten van elke factor op de prestaties van uw toepassingen, benchmarking hulpprogramma's worden uitgevoerd op installatieprogramma van de toepassing. Raadpleeg de sectie [Benchmarking](#Benchmarking) aan het einde van dit artikel voor stapsgewijze instructies voor het algemene benchmarking hulpprogramma's uitvoeren op Windows en Linux VMs.

### <a name="optimizing-iops-throughput-and-latency-at-a-glance"></a>IO's / s, doorvoer en latentie optimaliseren in een oogopslag  
De onderstaande tabel bevat een overzicht van alle prestatiefactoren en de stappen voor het optimaliseren van IO's / s, doorvoer en latentie. De secties die volgen dit overzicht vindt u elk factor is veel meer diepteas.

| | **IO 'S/S** | **Doorvoer** | **Latentie** |
|---|---|---|---|
| **Voorbeeldscenario** | Enterprise OLTP toepassing zeer hoog transacties per tweede tarief vereisen.                                                                                                                                 | Enterprise gegevensopslag toepassing verwerking grote hoeveelheden gegevens. | In de buurt van realtime-toepassingen waarbij direct antwoorden op aanvragen van gebruikers, zoals online spellen. |
| Prestatiefactoren  | | | |
| **IO grootte** | Kleinere IO oplevert hoger IO's / s.                                                                                                                                                                           | Groter IO naar oplevert sneller worden verwerkt. | |
| **VM grootte** | De grootte van een VM die IO's / s groter is dan het vereiste voor uw toepassing biedt gebruiken. Zie VM grootte en de beperkingen van hun IO's / s hier. | De grootte van een VM met doorvoer limiet groter is dan het vereiste voor uw toepassing gebruiken. Zie VM grootte en de beperkingen van hun doorvoer hier. | De grootte van een VM dat aanbiedingen limieten groter is dan het vereiste voor uw toepassing schaal gebruiken. Zie VM grootte en hun limieten hier. |
| **Grootte van de schijf** | Gebruik de grootte van een schijf die IO's / s groter is dan het vereiste voor uw toepassing biedt. Zie Schijfopruiming grootte en de beperkingen van hun IO's / s hier. | De schijfgrootte van een met doorvoer limiet groter is dan het vereiste voor uw toepassing gebruiken. Zie Schijfopruiming grootte en de beperkingen van hun doorvoer hier. | De schijfgrootte van een dat aanbiedingen limieten groter is dan het vereiste voor uw toepassing schaal gebruiken. Zie Schijfopruiming grootte en hun limieten hier. |
| **VM en schijf schaal limieten** | IO's / s limiet van de grootte van de VM gekozen moet groter zijn dan totale IO's / s aangestuurd door premium schijven gekoppeld. | Doorvoer limiet van de grootte van de VM gekozen moet groter zijn dan totale doorvoer aangestuurd door premium schijven gekoppeld. | Beperkingen voor de schaal van de grootte van de VM gekozen moeten groter zijn dan totale schaal beperkingen voor bijgevoegde premium opslag schijven. |
| **Schijfcaching van** | Alleen-lezen-Cache inschakelen voor premium schijven met alleen dik bewerkingen hoger lezen IO's / s ophalen. | | Alleen-lezen-Cache inschakelen voor premium schijven met klaar dik bewerkingen om erg laag gelezen vertragingstijden. |
| **Gesegmenteerd schijf te verdelen** | Gebruik van meerdere schijven en ze samen om een gecombineerde hoger IO's / s en doorvoer limiet stripe. Houd er rekening mee dat de gecombineerde limiet per VM hoger zijn dan de gecombineerde limieten van bijgevoegde premium schijven moet zijn. | |
| **Streepgrootte** | Kleinere streep voor willekeurig kleine IO patroon weergegeven in OLTP-toepassingen. Gebruik bijvoorbeeld Streepgrootte van 64KB voor SQL Server OLTP-toepassing. | Grotere streep voor opeenvolgende grote IO patroon weergegeven in Data Warehouse-toepassingen. Gebruik bijvoorbeeld 256KB streep's voor SQL Server Data warehouse toepassing. | |
| **Multithreading** | Multithreading naar een hoger aantal aanvragen push naar Premium-opslag die tot hogere IO's / s en doorvoer leiden wordt gebruikt. Stel bijvoorbeeld op SQL Server een hoge MAXDOP waarde naar meer CPU's toewijst aan SQL Server. | |
| **Wachtrij diepteas** | Grotere wachtrijdiepte levert hoger IO's / s. | Grotere wachtrijdiepte levert sneller worden verwerkt. | Kleinere wachtrijdiepte levert lagere vertragingstijden. |

## <a name="nature-of-io-requests"></a>Aard van IO aanvragen  
Een opdracht IO is een maateenheid invoer/uitvoer bewerking die uw toepassing zult uitvoeren. De aard van IO aanvragen, willekeurige of sequentiële, identificeren kunt lezen of schrijven, klein of groot is, u de vereisten voor de prestaties van uw toepassing. Is het belangrijk is voor meer informatie over de aard van IO aanvragen om de juiste beslissingen bij het ontwerpen van de infrastructuur van uw toepassingen.

IO grootte is een van de belangrijkste factoren. De grootte IO is de grootte van de invoer/uitvoer bewerkingsaanvraag gegenereerd door uw toepassing. De grootte IO heeft een aanzienlijk invloed op de prestaties met name op het IO's / s en de bandbreedte die de toepassing kan bereiken. De volgende formule wordt de relatie tussen IO's / s, IO grootte en de bandbreedte/doorvoer.  
    ![](media/storage-premium-storage-performance/image1.png)

Sommige toepassingen kunnen u de grootte van hun IO, wijzigen en sommige-toepassingen niet. Bijvoorbeeld: SQL Server bepaalt de optimale IO grootte zelf en biedt geen gebruikers met een knoppen om deze te wijzigen. Aan de andere kant Oracle vindt u een parameter met de naam [DB\_blokkeren\_grootte](https://docs.oracle.com/cd/B19306_01/server.102/b14211/iodesign.htm#i28815) die u kunt configureren met de grootte van de i/o-verzoek van de database.

Als u een toepassing kunnen niet worden de IO-grootte wijzigen, gebruikt u de richtlijnen in dit artikel de KPI die het meest relevant is voor uw toepassing optimaliseren. Bijvoorbeeld:

-   Een toepassing OLTP genereert miljoenen kleine en willekeurige IO aanvragen. Als u wilt deze soort IO aanvragen verwerken, moet u de infrastructuur van uw toepassing hoger IO's / s ophalen ontwerpen.  
-   Een toepassing voor gegevensopslag genereert aanvragen voor grote en opeenvolgende IO. Als u wilt deze soort IO aanvragen verwerken, moet u de infrastructuur van uw toepassing kom ik aan hoger bandbreedte of doorvoer ontwerpen.

Als u een toepassing, waarmee u de grootte IO wijzigen, gebruikt u deze vuistregel voor de grootte IO naast de richtlijnen van andere prestaties,

-   Kleinere IO hoger IO's / s ophalen. Bijvoorbeeld: 8 KB voor een toepassing OLTP.  
-   Grotere IO om sneller bandbreedte/worden verwerkt. Bijvoorbeeld 1024 KB voor een data warehouse-servicetoepassing.

Hier volgt een voorbeeld van hoe u de IO's / s en doorvoer/bandbreedte voor uw toepassing kan berekenen. Houd rekening met een toepassing via een schijf P30. Het maximale aantal IO's / s en doorvoer/bandbreedte een schijf P30 kunt bereiken is 5000 IO's / s en 200 MB per seconde respectievelijk. Als uw toepassing is de maximale IO's / s van de schijf P30 vereist en u een kleinere IO zoals 8 KB gebruiken, is de resulterende bandbreedte is mogelijk om nu 40 MB per seconde. Als uw toepassing is de maximale doorvoer/bandbreedte van P30 schijf vereist en u een groter IO zoals 1024 KB gebruiken, de resulterende IO's / s wordt wel kleiner, 200 IO's / s. De grootte IO daarom afstemmen zodat deze voldoet aan de vereisten voor het IO's / s en doorvoer/bandbreedte van beide uw toepassing. Onderstaande tabel bevat een overzicht van de verschillende IO grootten en hun bijbehorende IO's / s- en doorvoer voor een schijf P30.

| **Het vereiste voor toepassingen** | **I/o-grootte** | **IO 'S/S** | **Doorvoer/bandbreedte** |
|-----------------------------|--------------|----------|--------------------------|
| Max IO's / s                    | 8 KB         | 5.000    | 40 MB per seconde         |
| Max doorvoer              | 1024 KB      | 200      | 200 MB per seconde        |
| Max Throughput + hoog IO's / s  | 64 KB        | 3200    | 200 MB per seconde        |
| Max IOPS + hoge doorvoer  | 32 KB        | 5.000    | 160 MB per seconde        |

Als u IO's / s en bandbreedte die hoger zijn dan de maximale waarde van een schijf één premium-opslag, gebruikt u meerdere premium schijven gestreept samen. Bijvoorbeeld streep twee P30 schijven kom ik aan een gecombineerde IO's / s van 10.000 IO's / s of een gecombineerde capaciteit van 400 MB per seconde. Zoals wordt beschreven in de volgende sectie, moet u de grootte van een VM die ondersteuning biedt voor de gecombineerde IO's / s en doorvoer schijf.

>**Opmerking:**  
>IO's / s of doorvoer de andere ook vergroot te vergroten, zorg er dan voor dat u doorvoer of IO's / s limieten van de schijf of VM niet raken wanneer een vergroten.

Als u wilt de effecten van IO grootte van de toepassingsprestaties wonen, kunt u benchmarking's uitvoeren op uw VM en schijven. Meerdere toets uitgevoerd maken en gebruiken van verschillende IO grootte voor elke uitvoeren om de gevolgen weer te geven. Raadpleeg de sectie [Benchmarking](#Benchmarking) aan het einde van dit artikel voor meer informatie.

## <a name="high-scale-vm-sizes"></a>Hoge schaal VM grootte  
Wanneer u begint met het ontwerpen van een toepassing, is een van de eerste dingen te doen, kiest u een VM voor het hosten van uw toepassing. Premium opslagruimte biedt hoge schaal VM puntgrootten die toepassingen waarbij hoger rekenkracht en een hoge lokale schijf o prestaties kunnen worden uitgevoerd. Deze VMs bieden sneller processors, een hogere geheugen-naar-core-breedteverhouding en een Solid-State station (SSD) voor de lokale schijf. Voorbeelden van hoge schaal VMs ondersteunende Premium opslag zijn de reeks DS, DSv2 en GS VMs.

Hoge schaal VMs zijn beschikbaar in verschillende grootten met een verschillend aantal CPU cores, geheugen, OS en grootte van de tijdelijke schijf. De grootte van elke VM, heeft ook maximum aantal gegevensschijven die u aan de VM kunt koppelen. Daarom de door u gekozen VM grootte is van invloed op hoeveel bewerkingen, geheugen en opslagcapaciteit is beschikbaar voor uw toepassing. Deze heeft ook gevolgen voor het berekenen en opslag van kosten. Hieronder vindt u bijvoorbeeld de specificaties van de grootste VM grootte in een reeks DS, DSv2 reeks en een reeks GS:

| VM grootte | CPU-kernen | Geheugen | VM schijfgrootte | Max. gegevensschijven | Cachegrootte | IO 'S/S | Bandbreedte Cache IO limieten |
|---|---|---|---|---|---|---|---|
| Standard_DS14 | 16 | 112 GB | OS = 1023 GB <br> Lokale SSD = 224 GB | 32 | 576 GB | 50.000 IO 'S/S <br> 512 MB per seconde | 4000 IO's / s en 33 MB per seconde |
| Standard_GS5 | 32 | 448 GB | OS = 1023 GB <br> Lokale SSD = 896 GB | 64 | 4224 GB | 80.000 IO 'S/S <br> 2000 MB per seconde | 5000 IO's / s en 50 MB per seconde |

Als u wilt weergeven op een volledige lijst van alle beschikbare Azure VM formaten, raadpleegt u [Windows VM grootten](../virtual-machines/virtual-machines-windows-sizes.md) of [Linux VM tekengrootten](../virtual-machines/virtual-machines-linux-sizes.md). Kies een grootte VM die kunt vergaderen en aanpassen aan uw vereisten voor de prestaties gewenste toepassing. Daarnaast rekening volgen belangrijke overwegingen bij het kiezen van VM grootte.


*Limieten voor schaal*  
De maximale IO's / s-limieten per VM en per schijf zijn verschillende en onafhankelijk van elkaar. Zorg ervoor dat de toepassing IO's / s is sturende binnen de grenzen van de VM, evenals de premium-schijven gekoppeld. Anders ervaart prestaties beperken.

Stel als voorbeeld, een vereiste toepassing een maximum van 4000 IO's / s. Hiervoor kunt u een schijf P30 op een VM DS1 inrichten. De schijf P30 kan maximaal 5.000 IO's / s bieden. De VM DS1 is echter beperkt tot 3200 IO's / s. Daarom de toepassingsprestaties wordt worden beperkt door de limiet VM bij 3200 IO's / s en wordt er anders werken prestaties. Als u wilt voorkomen dat deze situatie, kies een VM en schijf grootte dat beide aan toepassingsvereisten voldoet.

*Kosten van bewerking*  
In veel gevallen is het mogelijk dat de totale kosten van bewerkingen op basis van de Premium-opslag lager is dan standaard opslag.

Stel een toepassing 16.000 IO's / s vereisen. Als u wilt bereiken deze prestaties, moet u een standaard\_D14 Azure IaaS VM, die een maximum IO's / s van 16.000 met 32 standaard de 1TB schijven kunt geven. Elke 1TB opslagruimte op standaard-schijf kunt bereiken van maximaal 500 IO's / s. De geschatte kosten van deze VM per maand zijn $1,570. De maandelijkse kosten van 32 standaard schijven is $1,638. De geschatte totale maandelijkse kosten zijn $3,208.

Echter als u dezelfde toepassing op Premium opslag gehost, moet u een kleinere VM en minder schijven premium, zodat de totale kosten wordt beperkt. Een standaard\_DS13 VM kunt voldoen aan de 16.000 IO's / s vereiste met vier P30 schijven. De VM DS13 heeft een maximum IO's / s van 25,600 en elke schijf P30 heeft een maximum IO's / s van 5.000. Algemene, deze configuratie 5.000 x 4 = 20.000 kunt bereiken IO's / s. De geschatte kosten van deze VM per maand zijn $1,003. De maandelijkse kosten van vier P30 premium schijven is $544.34. De geschatte totale maandelijkse kosten zijn $1,544.

Onderstaande tabel bevat een overzicht van de verdeling van de kosten van dit scenario voor de standaardweergave en de Premium-opslag.

| | **Standaard** | **Premium** |
|---|---|---|
| **Kosten van VM per maand** | $1,570.58 (standaard\_D14)   | $1,003.66 (standaard\_DS13) |
| **Kosten van schijven per maand** | $1,638.40 (32 x 1 TB schijven) | $544.34 (4 x P30 schijven) |
| **Totale kosten per maand**  | $3,208.98 | $1,544.34 |

*Linux Distros*  

Met Azure Premium opslag krijgt u hetzelfde niveau van prestaties voor VMs met Windows en Linux. Microsoft ondersteuning voor veel soorten van Linux distros en ziet u de volledige lijst [hier](../virtual-machines/virtual-machines-linux-endorsed-distros.md). Het is belangrijk te weten dat andere distros meer voor verschillende soorten werkbelasting geschikt zijn. Hier ziet u verschillende niveaus van prestaties afhankelijk van de distro die uw werkzaamheden wordt uitgevoerd op. Test de distros Linux met uw toepassing en kiest u degene die het meest geschikt.


Wanneer u zich Linux met Premium opslag, schakelt u de meest recente updates over de vereiste stuurprogramma's om ervoor te zorgen krachtige.

## <a name="premium-storage-disk-sizes"></a>Opslaggrootte voor Schijfopruiming Premium  
Azure Premium opslagruimte biedt drie schijfgrootten momenteel. De grootte van elke schijf geldt een limiet andere schaal voor IO's / s, bandbreedte en opslag. Kies rechts Premium opslagschijf grootte afhankelijk van de toepassingsvereisten en de hoge schaal VM grootte. De onderstaande tabel ziet u de grootte van de drie schijven en hun mogelijkheden.

| **Schijftype**       | **P10**           | **P20**           | **P30**           |
|---------------------|-------------------|-------------------|-------------------|
| Grootte van de schijf           | 128 toevoegen           | 512 toevoegen           | 1024 verwijderen (1 TB)   |
| IO's / s per schijf       | 500               | 2300              | 5000              |
| Doorvoer per schijf | 100 MB per seconde | 150 MB per seconde | 200 MB per seconde |

Hoeveel schijven u kiest, is afhankelijk van de schijf grootte gekozen. U kunt één P30 schijf of meerdere P10 schijven om te voldoen aan uw toepassing vereiste. Account overwegingen bij het maken van de keuze onderstaande rekening.

*Schaal limieten (IO's / s en doorvoer)*  
De IO's / s en doorvoer limieten van de grootte van elke Premium-schijf is verschillen van de limieten van de schaal VM. Zorg ervoor dat de totale IO's / s en doorvoer uit de schijven binnen grenzen van de schaal van de door u gekozen VM grootte.

Bijvoorbeeld als een toepassing vereiste maximaal 250 MB/sec doorvoer is en u een VM DS4 gebruikt met één P30 schijf. De VM DS4 kan maximaal 256 MB/sec doorvoer geven. Een enkele P30 schijf heeft echter doorvoer limiet van 200 MB/sec. De toepassing wordt daarom worden beperkt op 200 MB/sec vanwege de limiet. Als u wilt deze limiet opheffen, inrichten van meer dan één gegevensschijven voor VM.

>**Opmerking:**  
>Lezen door de cache served worden niet opgenomen in de schijf IO's / s en doorvoer, dus niet onderworpen zijn aan de schijf limieten. Cache heeft de afzonderlijke IO's / s en doorvoer limiet per VM.
>
>Bijvoorbeeld zijn in eerste instantie uw lezen en schrijven 60MB/sec 40MB/sec respectievelijk en. Na verloop van tijd de cache warms omhoog en meer en meer gelezen fungeert uit de cache. U kunt vervolgens hoger schrijven doorvoer krijgen van de schijf.

*Aantal schijven*  
Hiermee bepaalt u het aantal schijven u moet door vast te stellen toepassingsvereisten. De grootte van elke VM heeft ook een limiet van het aantal schijven die u aan de VM kunt koppelen. Dit is meestal tweemaal het aantal cores. Zorg ervoor dat de VM-grootte die u kiest het aantal schijven nodig kunt ondersteunt.

Onthoud dat de Premium-opslag-schijven hebben hogere prestatiemogelijkheden vergeleken met standaard schijven. Als u uw toepassing van Azure IaaS VM standaard opslagruimte met Premium Storage gebruiken migreert, wordt u daarom waarschijnlijk minder premium schijven om de prestaties dezelfde of hoger voor uw toepassing moet.

## <a name="disk-caching"></a>Schijfcaching van  
Hoge schaal VMs die gebruikmaken van Azure Premium opslagruimte hebben een met meerdere niveaus in de cache technologie BlobCache genoemd. Een combinatie van de VM RAM en de lokale SSD BlobCache gebruikt voor in cache opslaan. Deze cache is beschikbaar voor de permanente schijven Premium en de lokale VM-schijven. Standaard is deze instelling cache ingesteld op alleen-lezen/schrijven voor OS schijven als alleen-lezen voor gegevensschijven die worden gehost op Premium-opslag. Met Schijfopruiming caching ingeschakeld op de schijven Premium, kunt de hoge schaal VMs zeer hoog niveaus van de basisprestaties van dat groter is dan de onderliggende schijfprestaties bereiken.

>[AZURE.WARNING] De instelling voor de cache van een Azure-schijf wijzigen losgekoppeld en opnieuw gevoegd de doelschijf. Als dit de schijf besturingssysteem werkt is, kan de VM opnieuw is opgestart. Stop alle toepassingen/services die mogelijk worden beïnvloed door deze verstoringen voordat de instelling voor de schijf-cache wijzigen.

Meer informatie over de werking van BlobCache, raadpleegt u de binnenkant [Azure Premium Storage](https://azure.microsoft.com/blog/azure-premium-storage-now-generally-available-2/) blogbericht.

Het is belangrijk om in te schakelen cache op de juiste set schijven. Of u moet inschakelen schijf in cache opslaan op een schijf premium of niet wordt afhankelijk van het patroon werkbelasting die schijf wordt worden verwerkt. Onderstaande tabel ziet u de standaard cache-instellingen voor OS en gegevens schijven.

| **Schijftype** | **Cache standaardinstelling** |
|---|---|
| OS Schijfopruiming | ReadWrite |
| Gegevens Schijfopruiming | Geen |

Hier volgen de aanbevolen schijf cache-instellingen voor gegevensschijven

| **Cache-instelling schijf** | **Aanbeveling op wanneer deze instelling gebruiken** |
|---|---|
| Geen | Configureer host-cache als geen voor schrijven alleen-lezen en schrijven veel schijven. |
| Alleen-lezen | Host-cache configureren als alleen-lezen voor alleen-lezen en alleen-lezen-schrijven schijven. |
| ReadWrite | Host-cache als ReadWrite configureren alleen als uw toepassing op correcte wijze in de cache opgeslagen gegevens schrijven naar permanente schijven als dat nodig is. |

*Alleen-lezen*  
U kunt alleen-lezen caching van gegevens van de Premium-opslag schijven configureert, lage gelezen latentie bereiken en zeer hoog gelezen IO's / s en doorvoer krijgen voor uw toepassing. Dit is einddatum twee redenen

1.  Lezen uitgevoerd vanuit cache, dat wil op de VM geheugen en lokale SSD zeggen, zijn veel sneller dan lezen van de gegevensschijf, dat wil op de Azure-blobopslag zeggen.  
2.  Premium opslag worden niet meegeteld voor het lezen van cache, richting van de schijf IO's / s en doorvoer bediend. Uw toepassing dus kunnen bereiken hoger totale IO's / s en doorvoer.

*ReadWrite*  
Standaard hebben de schijven OS ReadWrite caching ingeschakeld. Ondersteuning voor ReadWrite caching van gegevens ook schijven hebben we onlangs toegevoegd. Als u ReadWrite caching gebruikt, moet u een goede manier om het wegschrijven van de gegevens uit cache naar permanente schijven hebben. SQL Server verwerkt, bijvoorbeeld in de cache opgeslagen gegevens schrijft naar de permanente schijven op een eigen. ReadWrite cache gebruikt met een toepassing die persistent maken de vereiste gegevens niet verwerkt kan leiden tot verlies van gegevens als de VM loopt vast.

Als voorbeeld, kunt u deze richtlijnen toepassen op SQL Server volgt, wordt uitgevoerd op de Premium-opslag

1.  "Alleen-lezen" cache op premium schijven hostingprovider gegevensbestanden configureren.  
    een.  De fast leest uit cache lagere de tijd van de query SQL Server aangezien data-pagina's zijn veel sneller opgehaald uit de cache vergeleken met rechtstreeks vanuit de gegevens schijven.  
    b.  Ophalen leest uit de cache, betekent dat er extra doorvoer beschikbaar is via premium gegevensschijven. SQL Server kunt gebruiken van deze extra doorvoer richting ophalen meer gegevens-pagina's en andere bewerkingen, zoals back-up/herstellen, batch laadtijd en index wordt opnieuw gemaakt.  
2.  Configureren "Geen" cache op premium schijven voor de logboekbestanden hostingprovider.  
    een.  Logboekbestanden hebben hoofdzakelijk schrijven veel bewerkingen. Ze kunnen daarom niet gebruikmaken van de cache voor alleen-lezen.

## <a name="disk-striping"></a>Gesegmenteerd schijf te verdelen  
Wanneer een hoge schaal die VM met verschillende premium permanente schijven is gekoppeld, kunnen u de schijven samen gestreept als u wilt aggregeren hun IO's / s, bandbreedte en opslagcapaciteit.

In Windows, kunt u opslagruimte spaties streep schijven samen. In een groep, moet u één kolom voor elke schijf configureren. Anders kunt de algehele prestaties van gestreepte volume lager zijn dan verwacht, vanwege ongelijkmatige spreiding van verkeer op de schijven.

Belangrijk: Met Serverbeheer UI, kunt u het totale aantal kolommen maximaal 8 voor een gestreepte volume instellen. Bij het toevoegen van meer dan 8 schijven, kunt u de PowerShell gebruiken om het volume te maken. Via PowerShell, kunt u instellen het aantal kolommen gelijk is aan het aantal schijven. Als er 16 schijven in een set één streep; bijvoorbeeld Geef 16 kolommen in de *Aantal_kolommen* -parameter van de *Nieuw VirtualDisk* PowerShell-cmdlet.


Gebruik het hulpprogramma MDADM streep schijven samen op Linux. Raadpleeg voor gedetailleerde stappen op gesegmenteerd te verdelen schijven op Linux [Software RAID configureren op Linux](../virtual-machines/virtual-machines-linux-configure-raid.md).


*Streepgrootte*  
Een belangrijke configuratie in schijfsegmentering is de Streepgrootte van de. Het formaat van de streep of blokgrootte is de kleinste hoeveelheid gegevens die toepassing op een gestreepte volume beantwoorden kunt. De Streepgrootte die u configureert, is afhankelijk van het type-toepassing en het patroon van de aanvraag. Als u de Streepgrootte van het verkeerde kiest, kan dit leiden tot IO uitlijning, die naar anders werken prestaties van uw toepassing leidt.

Als een aanvraag IO is gegenereerd door uw toepassing groter dan de schijf Streepgrootte is, schrijft het opslagsysteem bijvoorbeeld deze buiten de grenzen van de eenheid streep op meer dan één schijf. Wanneer het tijd voor toegang tot die gegevens is, heeft dit te zoeken in meer dan één streep eenheden het verzoek te voltooien. De cumulatieve werking van dit gedrag kan leiden tot aanzienlijke prestatieverbeteringen verslechtering van. Aan de andere kant, als de grootte van de aanvraag IO kleiner is dan Streepgrootte, en er willekeurig, de aanvragen IO mogelijk optellen op dezelfde schijf veroorzaakt door een vertraging veroorzaken en uiteindelijk daardoor verslechteren de prestaties IO.


Afhankelijk van het type werkbelasting uw toepassing wordt uitgevoerd, kiest u de Streepgrootte van een juiste. Voor willekeurig kleine IO aanvragen, gebruikt u een kleinere streep. Terwijl voor grote opeenvolgende IO gebruik aanvragen streep groter. Ontdek de streep grootte aanbevelingen voor de toepassing die dat u wilt uitvoeren op de Premium-opslag. Configureren voor SQL Server, Streepgrootte van 64KB voor OLTP werkbelasting en 256KB voor magazijnbeheer werkbelasting gegevens. Zie [prestaties aanbevolen procedures voor SQL Server Azure VMs](../virtual-machines/virtual-machines-windows-sql-performance.md#disks-and-performance-considerations) voor meer informatie.


>**Opmerking:**  
>U kunt samen maximaal 32 schijven voor premium in een reeks DS VM en 64 premium schijven in een reeks GS VM stripe.

## <a name="multi-threading"></a>Multithreading  
Azure heeft Premium opslag platform moeten massively parallelle ontworpen. Een toepassing voor meerdere threads toestaan realiseert daarom veel betere prestaties dan een single thread-toepassing. Een toepassing voor meerdere threads toestaan zijn taken opgesplitst over meerdere threads en efficiëntie van de uitvoering verhogen met behulp van de VM en schijf bronnen voor het maximale aantal.

Bijvoorbeeld: als uw toepassing wordt uitgevoerd op een enkele core VM met twee threads, de CPU kunt schakelen tussen de twee threads naar efficiëntie. Terwijl een thread op een schijf IO om uit te voeren wacht, wordt de CPU kunt overschakelen naar de andere thread. Op deze manier kunnen twee threads uitvoeren meer dan één thread zou doen. Als de VM meer dan één core heeft, deze verder wordt kleiner actieve tijd aangezien elke core taken parallel kunt uitvoeren.

U mogelijk niet kunnen wijzigen van de manier waarop een gebruiksklare toepassing één implementeert thread of multithreading. SQL Server is bijvoorbeeld kunnen meerdere CPU en meerdere core verwerken. SQL Server echter wordt bepaald welke omstandigheden wordt deze gebruikmaken van een of meer threads te verwerken van een query. Deze kan uitvoeren van query's en indexen met multithreading maken. Voor een query waarbij grote tabellen koppelen en het sorteren van gegevens om terug te gaan naar de gebruiker, SQL Server waarschijnlijk meerdere threads gebruikt. Een gebruiker beheren niet echter of SQL Server wordt uitgevoerd voor een query met één thread of meerdere threads.

Configuratie-instellingen die u wijzigen kunt om dit van invloed zijn op er zijn multithreading of parallelle verwerking van een toepassing. Bijvoorbeeld voor het geval de SQL Server is de maximale mate van parallellisme configuratie. Verwerking van deze instelling genoemd MAXDOP, kunt u het maximum aantal processors die SQL Server kunt gebruiken wanneer parallelle configureren. U kunt MAXDOP configureren voor afzonderlijke query's of indexbewerkingen. Dit is nuttig als u wilt saldo vanaf resources van uw systeem voor een toepassing voor de kritieke prestaties.

Stel dat uw toepassing met SQL Server een grote query en een indexbewerking op hetzelfde moment wordt uitgevoerd. Laat ons is ervan uitgegaan dat u wilt dat de indexbewerking meer zodat vergeleken met de grote query. In dat geval kunt u MAXDOP waarde van de indexbewerking die hoger zijn dan de waarde MAXDOP voor de query instellen. Op deze manier heeft SQL Server meer aantal processors die deze kunt gebruikmaken van voor de indexbewerking vergeleken met het aantal processors die deze op de grote query kunt opslagquota. Onthoud dat u het aantal threads SQL Server wordt gebruikt voor elke bewerking zijn niet van invloed. U kunt bepalen het maximum aantal processors wordt toegewezen voor multithreading.

Meer informatie over [Graden van parallellisme](https://technet.microsoft.com/library/ms188611.aspx) in SQL Server. Bekijk deze instellingen die van invloed op multithreading in uw toepassing en de bijbehorende configuraties optimaliseren.

## <a name="queue-depth"></a>Wachtrij diepteas  
De wachtrij diepteas of de wachtrijlengte of de grootte van de wachtrij is het aantal in behandeling zijnde IO aanvragen in het systeem. De waarde van wachtrij diepteas bepaalt hoeveel IO bewerkingen door uw toepassing uitlijnen kan, waarin de schijven wordt verwerkt. Dit van invloed op alle de drie toepassing performance indicators die we in dit artikel te weten, IO's / s, doorvoer en latentie besproken.

Wachtrij diepteas en multithreading bent nauw verwante. De waarde wachtrij diepteas wordt aangegeven hoeveel multithreading kan worden bereikt door de toepassing. Als de wachtrijdiepte groot is, toepassing gelijktijdig kan worden uitgevoerd meer bewerkingen, dat wil zeggen meer multithreading. Als de wachtrijdiepte klein, hoewel bij een toepassing meerdere threads toestaan is, heeft dit niet voldoende aanvragen voor uitvoering gelijktijdige uitgelijnd.

Meestal uit de boekenplank toepassingen kunt u niet wijzigen van de diepteas wachtrij omdat als ingestelde onjuist meer schade dan goed doet. Toepassingen, wordt de juiste waarde van de diepteas wachtrij om de optimale prestaties ingesteld. Het is echter belangrijk is voor meer informatie over dit concept zodat u van prestatieproblemen met uw toepassing oplossen kunt. U kunt ook de effecten van de diepteas wachtrij zien door benchmarking hulpprogramma's uit te voeren op uw systeem.

Sommige toepassingen bieden instellingen de wachtrijdiepte beïnvloeden. Bijvoorbeeld, beschreven de instelling van de (maximale mate van parallellisme) MAXDOP in SQL Server in de vorige sectie. MAXDOP is een manier om te beïnvloeden wachtrij diepteas en multithreading, hoewel het niet rechtstreeks de waarde wachtrij diepteas van SQL Server verandert.

*Hoge wachtrij diepteas*  
Een diepte hoge wachtrij lijnen u meer bewerkingen op de schijf. De schijf bekend de volgende aanvraag in de wachtrij tijd vooraf. De schijf kan dus bewerkingen tijd vooraf plannen en deze in een reeks voor een optimale verwerken. Aangezien de toepassing meer aanvragen naar de schijf verzendt is, kan de schijf meer parallelle IOs verwerken. Uiteindelijk kunnen de toepassing bereiken hoger IO's / s. Aangezien de verwerking van toepassing is meer aanvragen, wordt de totale doorvoer van de toepassing ook wordt verhoogd.

Meestal een toepassing maximumdoorvoer met 8 kunt bereiken-16 + uitstaande IOs per aangesloten schijf. Als een diepte wachtrij een is toepassing wordt niet voldoende IOs drukken aan het systeem en minder hoeveelheid worden verwerkt in een bepaalde termijn. Met andere woorden, minder doorvoer.

Bijvoorbeeld in SQL Server, de MAXDOP-waarde voor een query toevoegen aan de '4' wordt geïnformeerd SQL Server om maximaal vier cores de query te gebruiken. SQL Server bepaalt wat de beste wachtrij diepteas waarde en het aantal cores voor het uitvoeren van query is.

*Wachtrij optimale diepteas*  
Zeer hoog wachtrij diepteas waarde, heeft ook de nadelen. Als wachtrij diepteas waarde te hoog is, probeert de toepassing te sturen van zeer hoog IO's / s. Tenzij toepassing permanente schijven met voldoende ingerichte IO's / s heeft, kan dit een negatieve invloed op de toepassing vertragingstijden. Formule, ziet u de relatie tussen IO's / s en latentie wachtrij diepteas in.  
    ![](media/storage-premium-storage-performance/image6.png)

Voor elke hoge waarde, maar voor een optimale waarde, die voldoende IO's / s voor de toepassing kan bieden handhaven vertragingstijden, moet u niet wachtrij diepteas configureren. Als de toepassing latentie moet worden 1 milliseconden, de wachtrijdiepte nodig is om te bereiken 5000 IO's / s is bijvoorbeeld, QD = 5000 x 0,001 = 5.

*Wachtrijdiepte voor gestreepte Volume*  
Voor een gestreepte volume, het behoud van een wachtrijdiepte hoog genoeg zodanig zijn dat elke schijf een piek wachtrijdiepte afzonderlijk heeft. Stel dat u een toepassing waardoor een wachtrijdiepte van 2 en 4 schijven er zijn in de streep. De twee IO aanvragen worden doorgestuurd naar twee schijven en resterende twee schijven worden niet actief geweest. De wachtrijdiepte daarom configureren zodat alle schijven bezet kunnen zijn. Formule hieronder ziet u hoe bepaalt u de wachtrijdiepte van gestreepte hoeveelheden.  
    ![](media/storage-premium-storage-performance/image7.png)

## <a name="throttling"></a>Beperken  
Azure Premium opslag bepalingen opgegeven aantal IO's / s en doorvoer afhankelijk van de VM grootte en de schijf puntgrootten die u kiest. Als uw toepassing probeert aan te melden IO's / s of doorvoer station boven deze limieten van wat de VM of de schijf kunt afhandelen, wordt deze door Premium opslag beperken. Dit doet in de vorm van de prestaties verslechteren in uw toepassing. Dit kan betekenen hoger latentie, verlaag doorvoer of lager in IO's / s. Als Premium opslag niet beperken wordt, wordt uw toepassing kan volledig mislukt door meer dan wat de bronnen waarmee bereikt kunnen zijn. Zo is, om prestatieproblemen te voorkomen vanwege beperken, altijd voldoende resources voor uw toepassing te inrichten. Rekening wat we besproken in de VM grootte en de schijf grootten hierboven. Benchmarks, is de beste manier om te bepalen wat u moet uw hosttoepassing resources.

## <a name="benchmarking"></a>Benchmarks  
Benchmarks, is het proces van verschillende werkbelastingen op uw toepassing simuleren en de toepassingsprestaties voor elke werkbelasting meten. De stappen in een eerdere sectie gebruikt, hebt u de toepassing prestatie-eisen verzameld. Door te voeren benchmarking hulpmiddelen op de VMs de toepassing hostingprovider, kunt u de prestaties die uw toepassing met Premium opslag bereiken kunt bepalen. In deze sectie bieden we u voorbeelden van een standaard DS14 VM deze is ingericht met Azure Premium schijven benchmarks.

We hebben gemeenschappelijke benchmarking hulpmiddelen Iometer en FIO, respectievelijk gebruikt voor Windows en Linux. Deze hulpmiddelen brengen gang meerdere threads simuleren een producenten soortgelijke werkbelasting en meet de systeemprestaties wilt verbeteren. U kunt ook een parameters zoals blok-grootte en wachtrij diepteas, waarin u normaal gesproken niet voor een toepassing wijzigen configureren met de hulpmiddelen. Hiermee kunt u meer flexibiliteit om de maximale prestaties op hoog schaal VM deze is ingericht met premium schijven voor verschillende soorten toepassing werkbelasting te sturen. Voor meer dat informatie over elke benchmarking hulpmiddel Bezoek [Iometer](http://www.iometer.org/) en [FIO](http://freecode.com/projects/fio).

Wilt u de onderstaande voorbeelden, een standaard DS14 VM maken en 11 Premium schijven als bijlage toevoegen aan de VM. 11 schijven, 10 schijven configureren met host caching "Geen" en deze in een volume genaamd NoCacheWrites stripe. Host caching als 'Alleen-lezen' op de resterende schijf configureren en maak een volume CacheReads aangeroepen met deze schijf. Met deze configuratie, is mogelijk om de maximale lezen en schrijven prestaties van een standaard DS14 VM weer te geven. Ga naar [maken en opslag van de Premium-account voor een virtuele machine gegevensschijf gebruiken](storage-premium-storage.md#create-and-use-a-premium-storage-account-for-a-virtual-machine-data-disk)voor meer informatie over het maken van een VM DS14 met premium schijven.

*De cache warm*  
De schijf met alleen-lezen-host caching kunnen geven hogere IO's / s dan de limiet. Als u deze maximale gelezen prestaties uit de cache host, moet eerst u warme de cache van deze schijf. Dit zorgt ervoor dat het alleen IOs welke benchmarking hulpmiddel wordt station op CacheReads volume daadwerkelijk de cache en niet de schijf rechtstreeks raakt. Het resultaat van de cache treffers van extra IOP uit de cache voor één ingeschakeld Schijfopruiming.

>**Belangrijk:**  
>U moet de cache warme voordat u benchmarks, telkens wanneer VM opnieuw is opgestart.

#### <a name="iometer"></a>Iometer   
[Het hulpmiddel Iometer downloaden](http://sourceforge.net/projects/iometer/files/iometer-stable/2006-07-27/iometer-2006.07.27.win32.i386-setup.exe/download) in de VM.

*Testbestand*  
Iometer gebruikt een testbestand dat is opgeslagen op het volume waarop u de benchmarking test wordt uitgevoerd. Deze stations gelezen en geschreven aan dit testbestand de schijf meten IO's / s en doorvoer. Iometer Hiermee maakt u deze testbestand als u dit niet hebt ingevoerd. Een 200GB-testbestand iobw.tst met de naam op de hoeveelheden CacheReads en NoCacheWrites maken.

*Specificaties voor Access*  
De specificaties, aanvragen IO grootte, % alleen-lezen/schrijven, willekeurige/opeenvolgende % zijn geconfigureerd met het tabblad 'Specificaties voor Access' in Iometer. Een access-specificatie te maken voor elk van de scenario's hieronder beschreven. Maak de access-specificaties en 'Opslaan' met een geschikte naam zoals – RandomWrites\_8K, RandomReads\_8K. Selecteer de bijbehorende specificatie wanneer u zich de Testscenario.

Een voorbeeld van access-specificaties voor maximale schrijven IO's / s scenario hieronder ziet  
    ![](media/storage-premium-storage-performance/image8.png)

*Specificaties voor maximale IO's / s-Test*  
Gebruiken om aan te tonen maximale IO's / s, kleinere verzoek. Gebruik 8 kB verzoek grootte en specificaties voor willekeurig schrijft en leest maken.

| Specificatie van Access | Grootte van de aanvraag | Willekeurige % | Alleen % |
|----------------------|--------------|----------|--------|
| RandomWrites\_8K     | 8 KB           | 100      | 0      |
| RandomReads\_8K      | 8 KB           | 100      | 100    |

*Specificaties voor maximumdoorvoer testen*  
Om aan te tonen maximumdoorvoer, gebruikt u de aanvraag groter. Gebruik van 64 kB verzoek grootte en specificaties voor willekeurig schrijft en leest maken.

| Specificatie van Access | Grootte van de aanvraag | Willekeurige % | Alleen % |
|----------------------|--------------|----------|--------|
| RandomWrites\_64 kB    | 64 KB          | 100      | 0      |
| RandomReads\_64 kB     | 64 KB          | 100      | 100    |

*De Iometer-test*  
De onderstaande stappen om een warme cache uitvoeren

1.  Maken van twee specificaties voor access met waarden die hieronder worden weergegeven

  	| Naam              | Grootte van de aanvraag | Willekeurige % | Alleen % |
  	|-------------------|--------------|----------|--------|
  	| RandomWrites\_1MB | 1MB          | 100      | 0      |
  	| RandomReads\_1MB  | 1MB          | 100      | 100    |

2.  Voer de test Iometer voor geïnitialiseerd cache schijf met de volgende parameters. Drie werknemer threads voor het doelvolume van het en een wachtrijdiepte 128 gebruiken. Stel de duur "Runtime" van de test aan 2hrs op het tabblad 'Setup testen'.

  	| Scenario              | Het Volume van de doelsite | Naam              | Duur |
  	|-----------------------|---------------|-------------------|----------|
  	| Cache-schijf geïnitialiseerd | CacheReads    | RandomWrites\_1MB | 2hrs     |

3.  Voer de test Iometer voor warm omhoog cache schijf met de volgende parameters. Drie werknemer threads voor het doelvolume van het en een wachtrijdiepte 128 gebruiken. Stel de duur "Runtime" van de test aan 2hrs op het tabblad 'Setup testen'.

  	| Scenario           | Het Volume van de doelsite | Naam             | Duur |
  	|--------------------|---------------|------------------|----------|
  	| Warme van Cache schijf | CacheReads    | RandomReads\_1MB | 2hrs     |

Na de cache op schijf is temperatuur, gaat u verder met de test-scenario's hieronder ziet u. Als wilt uitvoeren de test Iometer, gebruikt u ten minste drie werknemer threads voor **elk** doelvolume. Voor elke werknemer thread, selecteert u het doelvolume van het, wachtrij diepteas instellen en selecteer een van de opgeslagen test-specificaties, zoals wordt weergegeven in de onderstaande tabel, de bijbehorende Testscenario uitvoeren. Verwachte resultaten ziet de tabel ook voor IO's / s en doorvoer wanneer deze tests uitgevoerd. Voor alle scenario's, wordt de grootte van een kleine IO van 8KB en een diepte hoge wachtrij 128 gebruikt.

| Testscenario      | Het Volume van de doelsite | Naam              | Resultaat       |
|--------------------|---------------|-------------------|--------------|
| Max. Lees IO's / s     | CacheReads    | RandomWrites\_8K  | 50.000 IO 'S/S  |
| Max. IO's / s schrijven    | NoCacheWrites | RandomReads\_8K   | 64.000 IO 'S/S  |
| Max. Gecombineerde IO's / s | CacheReads    | RandomWrites\_8K  | 100.000 IO 'S/S |
|                    | NoCacheWrites | RandomReads\_8K   |              |
| Max. MB/sec lezen   | CacheReads    | RandomWrites\_64 kB | 524 MB/sec   |
| Max. MB/sec schrijven  | NoCacheWrites | RandomReads\_64 kB  | 524 MB/sec   |
| Gecombineerde MB/sec    | CacheReads    | RandomWrites\_64 kB | 1000 MB/sec  |
|                    | NoCacheWrites | RandomReads\_64 kB  |              |

Hieronder vindt u schermafbeeldingen van het Iometer testresultaten voor gecombineerde IO's / s en doorvoer scenario's.

*Gecombineerde lezen en schrijven maximale IO's / s*  
![](media/storage-premium-storage-performance/image9.png)

*Gecombineerde lezen en schrijven maximumdoorvoer*  
![](media/storage-premium-storage-performance/image10.png)

### <a name="fio"></a>FIO  
FIO is een populaire hulpprogramma naar vergelijkende opslag op de VMs Linux. Dit is de flexibiliteit om te selecteren van verschillende IO grootte, opeenvolgende of willekeurig lezen en schrijft. Deze Hiermee werknemer threads of processen, de opgegeven i/o-bewerkingen uit te voeren. Het type i/o-bewerkingen die elke werknemer thread moet uitvoeren taakbestanden gebruiken, kunt u opgeven. We hebben gemaakt één taakbestand per scenario weergegeven in de onderstaande voorbeelden. U kunt de specificaties in deze taakbestanden naar verschillende werkbelastingen waarop Premium opslag gebruikelijke wijzigen. We gebruiken een standaard DS-14 VM **Ubuntu**uitgevoerd in de voorbeelden. Gebruik dezelfde instelling wordt beschreven in het begin van de [sectie Benchmarking](#Benchmarking) en warme de cache voordat u de vergelijkend onderzoek.

Voordat u begint, [FIO downloaden](https://github.com/axboe/fio) en op uw virtuele computer installeren.

Voer de volgende opdracht voor Ubuntu,

        apt-get install fio

We vier werknemer threads voor het schrijven bewerkingen en vier werknemer threads gebruiken voor een alleen-bewerkingen voor de schijven. De werknemers schrijven wordt verkeer sturende op het volume "nocache" 10 schijven met cache is ingesteld op "Geen heeft". De werknemers lezen wordt verkeer sturende op het volume "readcache" 1 schijf met cache is ingesteld op 'Alleen-lezen' heeft.

*Maximale schrijven IO's / s*  
De taakbestand maken met de volgende specificaties maximale schrijven IO's / s ophalen. Noem deze "fiowrite.ini".

```
[global]
size=30g
direct=1
iodepth=256
ioengine=libaio
bs=8k

[writer1]
rw=randwrite
directory=/mnt/nocache
[writer2]
rw=randwrite
directory=/mnt/nocache
[writer3]
rw=randwrite
directory=/mnt/nocache
[writer4]
rw=randwrite
directory=/mnt/nocache
```

Opmerking de volgen belangrijke zaken aan bod die loopt gelijk met de richtlijnen voor het ontwerpen in de vorige gedeelten besproken zijn. Deze specificaties zijn essentieel voor maximale IO's / s, station  
-   Een hoge wachtrijdiepte van 256.  
-   De grootte van een kleine blokkeren van 8KB.  
-   Meerdere threads uitvoering van willekeurige schrijft.

Voer de volgende opdracht op de toets FIO voor 30 seconden gang  

    sudo fio --runtime 30 fiowrite.ini

Terwijl de test wordt uitgevoerd, is mogelijk om het aantal schrijven IO's / s de VM en Premium schijven geeft weer te geven. Zoals u in het onderstaande voorbeeld, is de maximale schrijven IO's / s limiet van 50.000 IO's / s geven van de VM DS14.  
    ![](media/storage-premium-storage-performance/image11.png)

*Maximale gelezen IO's / s*  
De taakbestand maken met de volgende specificaties maximale gelezen IO's / s ophalen. Noem deze "fioread.ini".

```
[global]
size=30g
direct=1
iodepth=256
ioengine=libaio
bs=8k

[reader1]
rw=randread
directory=/mnt/readcache
[reader2]
rw=randread
directory=/mnt/readcache
[reader3]
rw=randread
directory=/mnt/readcache
[reader4]
rw=randread
directory=/mnt/readcache
```

Opmerking de volgen belangrijke zaken aan bod die loopt gelijk met de richtlijnen voor het ontwerpen in de vorige gedeelten besproken zijn. Deze specificaties zijn essentieel voor maximale IO's / s, station

-   Een hoge wachtrijdiepte van 256.  
-   De grootte van een kleine blokkeren van 8KB.  
-   Meerdere threads uitvoering van willekeurige schrijft.

Voer de volgende opdracht op de toets FIO voor 30 seconden gang

    sudo fio --runtime 30 fioread.ini

Terwijl de test wordt uitgevoerd, is mogelijk om het aantal lezen IO's / s de VM en Premium schijven geeft weer te geven. Zoals u in het onderstaande voorbeeld, levert de VM DS14 meer dan 64.000 gelezen IO's / s. Dit is een combinatie van de schijf en de prestaties cache.  
    ![](media/storage-premium-storage-performance/image12.png)

*Maximum lezen en schrijven IO's / s*  
De taakbestand maken met de volgende specificaties om maximale gecombineerd lezen en schrijven IO's / s. Noem deze "fioreadwrite.ini".

```
[global]
size=30g
direct=1
iodepth=128
ioengine=libaio
bs=4k

[reader1]
rw=randread
directory=/mnt/readcache
[reader2]
rw=randread
directory=/mnt/readcache
[reader3]
rw=randread
directory=/mnt/readcache
[reader4]
rw=randread
directory=/mnt/readcache

[writer1]
rw=randwrite
directory=/mnt/nocache
rate_iops=12500
[writer2]
rw=randwrite
directory=/mnt/nocache
rate_iops=12500
[writer3]
rw=randwrite
directory=/mnt/nocache
rate_iops=12500
[writer4]
rw=randwrite
directory=/mnt/nocache
rate_iops=12500
```

Opmerking de volgen belangrijke zaken aan bod die loopt gelijk met de richtlijnen voor het ontwerpen in de vorige gedeelten besproken zijn. Deze specificaties zijn essentieel voor maximale IO's / s, station

-   Een hoge wachtrijdiepte 128.  
-   De grootte van een kleine blokkeren van 4KB.  
-   Meerdere threads uitvoering van willekeurige gelezen en geschreven.

Voer de volgende opdracht op de toets FIO voor 30 seconden gang

    sudo fio --runtime 30 fioreadwrite.ini

Terwijl de test wordt uitgevoerd, is mogelijk om te zien uit hoeveel gecombineerde lezen en schrijven IO's / s de VM en Premium schijven levert. Zoals u in het onderstaande voorbeeld, is de VM DS14 geven van meer dan 100.000 gecombineerde lezen en schrijven IO's / s. Dit is een combinatie van de schijf en de prestaties cache.  
    ![](media/storage-premium-storage-performance/image13.png)

*Maximum gecombineerd doorvoer*  
Als u het maximale aantal gecombineerd lezen en schrijven doorvoer, gebruikt een grotere blokkeren en grote wachtrij diepteas met meerdere threads uitvoering lezen en schrijven. U kunt de grootte van een blokkeren van 64KB en wachtrij diepteas 128 gebruiken.

## <a name="next-steps"></a>Volgende stappen  

Meer informatie over de opslag van Azure Premium:

- [Premium-opslag: High-Performance opslag voor Azure virtuele machines werkbelasting](storage-premium-storage.md)  

Meer artikelen over prestaties aanbevolen procedures voor SQL Server voor gebruikers van SQL Server:

- [Prestaties aanbevolen procedures voor SQL Server in Azure virtuele Machines](../virtual-machines/virtual-machines-windows-sql-performance.md)
- [Azure Premium opslagruimte biedt de beste prestaties voor SQL Server in Azure VM](http://blogs.technet.com/b/dataplatforminsider/archive/2015/04/23/azure-premium-storage-provides-highest-performance-for-sql-server-in-azure-vm.aspx) 
