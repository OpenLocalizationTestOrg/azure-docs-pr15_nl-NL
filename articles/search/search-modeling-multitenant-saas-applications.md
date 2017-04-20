<properties
    pageTitle="Modeling Multitenancy in Azure zoeken | Microsoft Azure | De zoekservice gehoste cloud"
    description="Meer informatie over algemene ontwerppatronen voor multitenant SaaS-toepassingen bij het gebruik van Azure zoeken."
    services="search"
    manager="jhubbard"
    authors="ashmaka"
    documentationCenter=""/>

<tags
    ms.service="search"
    ms.devlang="NA"
    ms.workload="search"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.date="10/26/2016"
    ms.author="ashmaka"/>

# <a name="design-patterns-for-multitenant-saas-applications-and-azure-search"></a>Ontwerpen patronen voor multitenant SaaS-toepassingen en Azure zoeken

Een toepassing voor de multitenant is een die dezelfde services en mogelijkheden biedt tot een willekeurig aantal tenants wie kan zien of delen van de gegevens uit een andere tenant. In dit document wordt beschreven hoe tenant moeten worden geïsoleerd strategieën voor het multitenant toepassingen die zijn gemaakt met Azure zoeken.

## <a name="azure-search-concepts"></a>Azure zoeken concepten
Als een oplossing zoeken-als-een-service kunnen Azure zoeken ontwikkelaars ervaringen uitgebreide zoeken naar toepassingen toevoegen zonder een infrastructuur beheren of dat u een expert in zoeken. Gegevens worden geüpload naar de service en klik vervolgens in de cloud zijn opgeslagen. Eenvoudige aanvragen tot de API voor het zoeken van Azure gebruikt, kunnen de gegevens vervolgens worden gewijzigd en gezocht. In [dit artikel](http://aka.ms/whatisazsearch)vindt u een overzicht van de service. Voordat u bespreekt ontwerppatronen, is het belangrijk om te begrijpen sommige concepten in Azure zoeken.

### <a name="search-services-indexes-fields-and-documents"></a>Services, indexen, velden en documenten zoeken
Wanneer u met Azure zoeken, wordt een abonnement op een _zoekservice_. Als u gegevens wordt geüpload naar Azure zoeken, wordt deze opgeslagen in een _index_ binnen de zoekservice. Er zijn een aantal indexen binnen een enkele service. Als u wilt gebruiken de vertrouwde concepten van databases, kan de zoekservice worden vergeleken met een database terwijl de indexen binnen een service kunnen worden vergeleken met tabellen in een database.

Elke index binnen een search-service heeft een eigen schema, die is gedefinieerd door een aantal aanpasbare _velden_. Gegevens worden toegevoegd aan een Azure-zoekindex in de vorm van afzonderlijke _documenten_. Elk document moet worden geüpload naar een bepaalde index en van die index schema moet aanpassen. Wanneer u gegevens met behulp van Azure zoeken zoekt, worden de volledige tekst zoekquery's worden uitgegeven ten opzichte van een bepaalde index.  Als u wilt vergelijken deze concepten met die van een database, velden kunnen worden vergeleken met kolommen in een tabel en documenten kunnen worden vergeleken met rijen.

### <a name="scalability"></a>Schaalbaarheid
Een Azure Search-service in de standaard [prijzen van laag](https://azure.microsoft.com/pricing/details/search/) kunt schaal in twee dimensies: opslag en beschikbaarheid.
* _Partities_ kan worden toegevoegd aan de opslag van een zoekservice vergroten.
* _Replica's_ kan worden toegevoegd aan een service om uit te breiden de doorvoer van aanvragen dat een search-service kan worden verwerkt.

Toevoegen en verwijderen partities en replica's aan, kan de capaciteit van de zoekservice naar toenemen met de hoeveelheid gegevens en het verkeer naar de vereisten van toepassing. In de volgorde van een zoekservice naar een gelezen [SLA](https://azure.microsoft.com/support/legal/sla/search/v1_0/)bereiken, moeten twee replica's. In de volgorde van een service om een alleen-lezen-schrijven [SLA](https://azure.microsoft.com/support/legal/sla/search/v1_0/), moeten drie replica's.


### <a name="service-and-index-limits-in-azure-search"></a>Beperkingen voor de service en index in Azure zoeken
Er zijn een paar verschillende [prijzen van lagen](https://azure.microsoft.com/pricing/details/search/) in Azure zoeken, elk van de lagen heeft verschillende [limieten en quota](search-limits-quotas-capacity.md). Sommige van deze limieten zijn op het niveau service, enkele zijn op de index-niveau, en andere partition-niveau.


|                                  | Eenvoudige     | Standard1   | Standard2   | Standard3   | Standard3 HD  |
|----------------------------------|-----------|-------------|-------------|-------------|---------------|
| Maximum aantal replica's per Service     | 3         | 12          | 12          | 12          | 12            |
| Maximum aantal partities per Service   | 1         | 12          | 12          | 12          | 1             |
| Maximale Zoeken eenheden (replica's * partities) per Service | 3         | 36          | 36          | 36          | 36 (maximaal 3 partities)            |
| Maximum aantal documenten per Service    | 1 miljoen | 180 miljoen | 720 miljoen | 1,4 miljard | 600 miljoen   |
| Maximale opslagruimte per Service      | 2 GB      | 300 GB      | 1.2 TB      | 2,4 TB      | 600 GB        |
| Maximum aantal documenten per Partition  | 1 miljoen | 15 miljoen  | 60 miljoen  | 120 miljoen | 200 miljoen   |
| Maximale opslagruimte per Partition    | 2 GB      | 25 GB       | 100 GB      | 200 GB      | 200 GB        |
| Maximale indexen per Service      | 5         | 50          | 200         | 200         | 3000 (max 1000 indexen/partition)          |


#### <a name="s3-high-density"></a>S3 Hoge dichtheid '
In van Azure zoekactie S3 prijzen laag is er een optie voor de modus Hoog dichtheid (HD) is speciaal ontworpen voor multitenant scenario's. In veel gevallen wordt het benodigde ter ondersteuning van een groot aantal kleinere tenants onder een service voor eenmalige om te profiteren van de voordelen van eenvoudig en kosten efficiency.

S3 HD kan voor de veel kleine indexen moeten worden verpakt onder het beheer van een één zoekservice door de mogelijkheid uit te breiden out indexen met partities voor de mogelijkheid voor het hosten van meer indexen in een service voor eenmalige handelsdag.

Een S3-service kan concrete invulling te geven, hebben tussen 1 en 200 indexen waarop samen kunnen maximaal 1,4 miljard documenten. Een HD S3 echter afzonderlijke indexen om te gaan alleen maximaal 1 miljoen documenten wilt toestaan, maar kan maximaal 1000 indexen per partition (maximaal 3000 per service) met het totale document aantal 200 miljoen per partition verwerken (maximaal 600 miljoen per service).



## <a name="considerations-for-multitenant-applications"></a>Overwegingen voor multitenant toepassingen
Multitenant toepassingen distribueren effectief bronnen tussen de tenants behoud van enkele niveau van de privacy tussen de verschillende tenants. Er zijn een paar overwegingen bij het ontwerpen van de architectuur voor dergelijke toepassing:

* _Moeten worden geïsoleerd tenant:_ Toepassingen ontwikkelaars moeten passende maatregelen om ervoor te zorgen dat geen tenants hebt geverifieerd of toegang tot de gegevens van andere tenants ongewenste. Na het perspectief van gegevensprivacy vereisen tenant moeten worden geïsoleerd strategieën effectieve beheer van gedeelde resources en bescherming tegen ruis neighbors.
* _Resourcekosten cloud:_ Net als met een andere toepassing, moeten de softwareoplossingen kosten Concurrentieanalyse als onderdeel van een toepassing voor de multitenant blijven.
* _Gebruiksgemak bewerkingen:_ Wanneer u een multitenant architectuur ontwikkelt, is de invloed op de bewerkingen en de complexiteit van de toepassing belang. Azure zoeken heeft een [99,9% SLA](https://azure.microsoft.com/support/legal/sla/search/v1_0/).
* _Globale milieu:_ Mogelijk moet multitenant toepassingen effectief dienen tenants die zijn verdeeld over de wereldbol.
* _Schaalbaarheid:_ Softwareontwikkelaars op moet letten hoe ze tussen onderhouden van een voldoende laag niveau van de toepassing complexiteit en ontwerpen van de toepassing aan de nieuwe schaal met aantal tenants en de grootte van de gegevens en de werklast tenants afstemmen.

Azure Search biedt een paar grenzen die kunnen worden gebruikt om de gegevens en de werklast tenants isoleren.

## <a name="modeling-multitenancy-with-azure-search"></a>Modeling multitenancy met Azure zoeken
In het geval van een scenario voor multitenant ontwikkelaar van de toepassing verbruikt een of meer zoekservices en delen van hun tenants tussen services, indexen of beide. Azure zoeken heeft een paar algemene patronen wanneer een multitenant scenario modeling:

1. _Index per pachter:_ Elke tenant heeft een eigen index binnen een zoekservice die wordt gedeeld met andere tenants.
1. _Service per pachter:_ Elke tenant heeft een eigen speciale Azure-zoekservice, hoogste niveau van gegevens en werkbelasting scheiding aanbod.
1. _Combinatie van beide:_ Grotere, meer-actieve tenants toegewezen speciale services terwijl kleinere tenants afzonderlijke indexen binnen gedeelde services zijn toegewezen.

## <a name="1-index-per-tenant"></a>1. indexeren per pachter
![Een portrayal van het model index per tenant](./media/search-modeling-multitenant-saas-applications/azure-search-index-per-tenant.png)

In een model index per tenant innemen meerdere tenants een enkel Azure-zoekservice waar elke tenant eigen index heeft.

Tenants bereiken gegevens moeten worden geïsoleerd omdat alle aanvragen zoeken en document bewerkingen worden uitgegeven op het indexniveau van een in Azure zoeken. In de toepassingsobjectlaag is de chatstatus nodig naar het verkeer door de verschillende tenants in de juiste indexen tijdens het beheren van bronnen op het serviceniveau van de ook over alle tenants.

Een belangrijke kenmerk van het model index per tenant is de mogelijkheid om de ontwikkelaar van de toepassing naar oversubscribe van de capaciteit van een zoekservice tussen tenants van de toepassing. Als de tenants een ongelijke verdeling van werkbelasting, kan de optimale combinatie van tenants worden verdeeld over een zoekservice indexen aangepast aan een aantal ten zeerste actief, systeembronnen tenants terwijl tegelijk een behoefte van minder actieve tenants. De verhouding is het niet van het model afhandelen van situaties waarin elke tenant gelijktijdig ten zeerste actief is.

Het model index per tenant de basis vormt voor een model van de variabele kosten, waar een hele Azure-zoekservice vooraf is gekocht en klik vervolgens met tenants ingevuld. In deze sectie geeft voor niet-gebruikte capaciteit voor proefabonnementen en gratis accounts.

Voor toepassingen met een globale milieu, het model index per tenant mogelijk niet de efficiëntste. Als een toepassing tenants zijn verdeeld over de hele wereld, is het mogelijk dat een afzonderlijke service nodig voor elke regio waarin kosten voor elk van deze mogelijk dupliceren.

Azure zoeken kan de schaal van de afzonderlijke indexen zowel het totale aantal indexen om te laten groeien. Als een juiste prijzen worden de laag gekozen, is de partities en replica's kunnen worden toegevoegd aan de hele zoekservice wanneer een afzonderlijke index binnen de service te groot opslag of-verkeer is toegestaan.

Als het totale aantal indexen te voor een enkele service groot heeft een andere service zodat het nieuwe tenants in te richten. Als indexen worden verplaatst naar de zoekservices moeten als nieuwe services worden toegevoegd, moet de gegevens uit de index handmatig worden gekopieerd vanuit één index naar de andere zoals Azure zoeken is niet toegestaan voor een index moeten worden verplaatst.


## <a name="2-service-per-tenant"></a>2. Service per pachter
![Een portrayal van het model service per tenant](./media/search-modeling-multitenant-saas-applications/azure-search-service-per-tenant.png)

In een service per tenant-architectuur heeft elke tenant eigen search-service.

In dit model, wordt in de toepassing de maximale hoeveelheid moeten worden geïsoleerd voor de tenants bereikt. Elke service heeft specifiek opslagruimte en doorvoer voor het verwerken van de zoekopdracht, evenals afzonderlijke API toetsen.

Het model service per tenant is een effectieve keuze voor toepassingen waarop elke tenant heeft een grote milieu of de werklast heeft variëren van de tenant naar de andere tenant, zoals resources niet door verschillende tenants werkbelasting worden gedeeld.

Een service per tenant model biedt ook het voordeel van een kostprijsmodel overzichtelijk, vaste. Er is geen vooraf investering in een hele zoekservice totdat er een tenant doorvoert, maar de kosten per tenant hoger zijn dan een index per tenant-model is.

Het model service per tenant is een efficiënte keuze voor toepassingen met een globale milieu. Met geografisch verspreid tenants kan het gemakkelijk van elke tenant-service in de juiste regio.

De uitdagingen in schaalbaarheid van dit patroon ontstaan wanneer afzonderlijke tenants bedrijf te groot wordt de service. Azure zoeken ondersteunt geen momenteel een upgrade van de prijzen laag van een search-service, zodat alle gegevens zou moeten handmatig worden gekopieerd naar een nieuwe service.

## <a name="3-mixing-both-models"></a>3. mengen beide modellen
Een ander patroon voor multitenancy modeling is index per tenant zowel service per tenant strategieën mengen.

Door het gebruik van de twee patronen, kunnen de grootste tenants van een toepassing speciale services innemen terwijl de behoefte van minder actieve, kleinere tenants indexen in een gedeelde service kan innemen. Dit model zorgt ervoor dat het grootste tenants consistente krachtige van de service terwijl de kleinere tenants beschermen tegen een ruis neighbors hebben.

Uitvoering van deze strategie vertrouwt echter prognose voorspellen welke tenants vereist een speciale service versus een index in een gedeelde service. Toepassing complexiteit Hiermee verhoogt u met de nodig voor het beheren van beide van deze multitenancy modellen.

## <a name="achieving-even-finer-granularity"></a>Zelfs weer specifieker bereiken
De bovenstaande ontwerppatronen aan het model multitenant scenario's in zoekresultaten voor een Azure wordt ervan uitgegaan dat een uniform bereik, waarbij elke tenant is voor een hele exemplaar van een toepassing. Toepassingen kunnen echter soms veel kleinere bereiken verwerken.

Als modellen service per tenant en index per tenant niet voldoende kleine bereiken zijn, is het mogelijk om een index een AutoAanpassen zelfs beter specifieker bereiken modelleren.

Als u wilt dat een enkele index die zich anders gedragen voor andere klant eindpunten, kan een veld worden toegevoegd aan een index waarmee wordt aangegeven dat een bepaalde waarde voor elke mogelijke klant. Telkens wanneer die een client Azure zoeken aanroept om de query of wijzigen van een index, de juiste waarde voor dat veld met Azure van [filter](https://msdn.microsoft.com/library/azure/dn798921.aspx) zoekmogelijkheden query tegelijk Hiermee geeft u de code van de clienttoepassing.

Deze methode kunnen worden gebruikt om de functionaliteit van afzonderlijke gebruikersaccounts, aparte machtigingsniveaus, bereiken en zelfs helemaal scheiden toepassingen.

## <a name="next-steps"></a>Volgende stappen
Azure tijdens het zoeken is een aantrekkelijke keuze voor veel toepassingen, [meer informatie over de krachtige mogelijkheden van de service](http://aka.ms/whatisazsearch). Bij de evaluatie van de verschillende ontwerppatronen voor multitenant toepassingen, kunt u de [verschillende prijzen lagen](https://azure.microsoft.com/pricing/details/search/) en de desbetreffende [service limieten](search-limits-quotas-capacity.md) voor de beste maat Azure zoeken passend maken van de toepassing werkbelasting en architecturen allerlei.

Vragen over het zoeken van Azure en multitenant scenario's kunnen worden doorgestuurd naar azuresearch_contact@microsoft.com.
