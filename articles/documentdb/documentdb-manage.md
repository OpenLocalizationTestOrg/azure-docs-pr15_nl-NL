<properties
    pageTitle="DocumentDB opslagcapaciteit en de prestaties | Microsoft Azure" 
    description="Meer informatie over de opslag van gegevens en opslag van documenten in DocumentDB en hoe u DocumentDB aan de capaciteitsbehoeften van uw toepassing kunt schalen." 
    keywords="opslag van documenten"
    services="documentdb" 
    authors="syamkmsft" 
    manager="jhubbard" 
    editor="cgronlun" 
    documentationCenter=""/>

<tags 
    ms.service="documentdb" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="08/18/2016" 
    ms.author="syamk"/>

# <a name="learn-about-storage-and-predictable-performance-provisioning-in-documentdb"></a>Meer informatie over de opslag en overzichtelijk prestaties inrichting in DocumentDB
Azure DocumentDB is een volledig beheerde, scalable document gerichte NoSQL databaseservice voor JSON-documenten. Met DocumentDB moet u niet verhuren virtuele machines, software implementeren of databases controleren. DocumentDB is dat wordt beheerd en continu gecontroleerd door Microsoft engineers voor het leveren van de beschikbaarheid van de wereld-klasse, prestaties en gegevensbescherming.  

U kunt aan de slag met DocumentDB door te [maken van een databaseaccount](documentdb-create-account.md) en een [DocumentDB database](documentdb-create-database.md) via de [portal van Azure](https://portal.azure.com/). DocumentDB databases zijn beschikbaar in eenheden van solid-state drive (SSD) back-opslag en doorvoer. Deze opslageenheden is ingericht door te [maken van database verzamelingen](documentdb-create-collection.md) binnen uw databaseaccount, elke siteverzameling met gereserveerde doorvoer die kan worden vergroot omhoog of omlaag op elk gewenst moment om te voldoen aan de vereisten van uw toepassing. 

Als uw toepassing groter is dan uw gereserveerde doorvoer voor een of meerdere siteverzamelingen, aanvragen beperkt op basis van de per siteverzameling. Dit betekent dat sommige aanvragen terwijl de anderen kunnen worden vertraagd kans van slagen.

Dit artikel bevat een overzicht van de resources en parameters die beschikbaar zijn voor het beheren van capaciteit en opslag van gegevens plannen. 

## <a name="database-account"></a>Databaseaccount
Als de abonnee van een Azure, kunt u een of meer DocumentDB database-accounts voor het beheren van uw database resources inrichten. Elk abonnement is gekoppeld aan een enkele Azure abonnement. 

DocumentDB accounts kunnen worden gemaakt via de [portal van Azure](documentdb-create-account.md)of met behulp van [een sjabloon ARM of Azure CLI](documentdb-automation-resource-manager-cli.md).

## <a name="databases"></a>Databases
Een database voor eenmalige DocumentDB kan vrijwel een onbeperkte hoeveelheid document opslagruimte gegroepeerd in verzamelingen bevatten. Verzamelingen bieden prestaties moeten worden geïsoleerd - elke siteverzameling kan worden ingericht met doorvoer die niet worden gedeeld met andere siteverzamelingen in de dezelfde database of het account. Een database DocumentDB is elastische grootte, die variëren van GB opslagruimte voor TBs van SSD back-document en ingerichte doorvoer. In tegenstelling tot een traditionele RDBMS database een database in DocumentDB is niet beperkt tot één computer en kan meerdere computers of clusters beslaan.  

Met DocumentDB, zoals u wilt schalen dat uw toepassingen, kunt u meer verzamelingen of databases of beide. Databases kunnen worden gemaakt via de [portal van Azure](documentdb-create-database.md) of via een van de [DocumentDB SDK's](documentdb-dotnet-samples.md).   

## <a name="database-collections"></a>Database-verzamelingen
Elke DocumentDB-database kan een of meer siteverzamelingen bevatten. Verzamelingen fungeren als partities ten zeerste beschikbare gegevens voor de opslag van documenten en verwerking. Elke siteverzameling kunt documenten met heterogene schema opslaan. Automatische indexering van DocumentDB en querymogelijkheden kunnen u eenvoudig filteren en documenten op te halen. Een verzameling vindt u het bereik voor de uitvoering opslagcapaciteit en de query document. Een siteverzameling is ook een domein transactie voor alle documenten die zijn opgenomen in deze. Verzamelingen worden toegewezen doorvoer op basis van de waarde in de portal van Azure of via de SDK's instellen. 

Verzamelingen worden automatisch ondergebracht in een of meer fysieke servers door DocumentDB. Wanneer u een siteverzameling maakt, kunt u de ingerichte doorvoer tussen aanvraag eenheden per seconde en een belangrijke eigenschap partition opgeven. De waarde van deze eigenschap wordt gebruikt door DocumentDB distributie van documenten tussen partities en route aanvragen zoals query's. De waarde van de partition sleutel fungeert ook als de transactiegrens voor opgeslagen procedures en triggers. Elke siteverzameling heeft een gereserveerde hoeveelheid doorvoer specifiek aan die verzameling, die niet worden gedeeld met andere siteverzamelingen in hetzelfde account. U kunt er daarom schalen out van uw toepassing zowel opslag-en doorvoer. 

Verzamelingen kunnen worden gemaakt via de [portal van Azure](documentdb-create-collection.md) of via een van de [DocumentDB SDK's](documentdb-sdk-dotnet.md).   
 
## <a name="request-units-and-database-operations"></a>Database of eenheden aanvragen

Wanneer u een siteverzameling maakt, kunt u reserveren doorvoer tussen [aanvraag eenheden (RU)](documentdb-request-units.md) per seconde. In plaats daarvan van nadenkt over en beheren van hardware resources, u kunt een **aanvraag eenheid** zien als een één maateenheid voor de resources moeten verschillende databasebewerkingen uitvoeren en service-opdracht van een toepassing. Het lezen van een document 1 KB verbruikt de dezelfde 1 RU ongeacht het aantal items die zijn opgeslagen in de verzameling of het aantal gelijktijdige aanvragen tegelijkertijd worden uitgevoerd. Alle aanvragen tegen DocumentDB, met inbegrip van complexe bewerkingen, zoals SQL-query's hebben een overzichtelijk RU waarde die kan worden bepaald ontwikkeling tegelijk. Als u de grootte van uw documenten en de frequentie van elke bewerking (lezen, schrijven en query's) om te ondersteunen voor uw toepassing, kunt u de exacte hoeveelheid doorvoersnelheid behoeften van uw toepassing inrichten en schaal van de database omhoog en omlaag prestaties van uw behoeften. 

Elke siteverzameling kan worden gereserveerd met doorvoersnelheid in blokken 100 verzoek eenheden per seconde, van honderden maximaal miljoenen verzoek eenheden per seconde. De ingerichte doorvoer kan worden aangepast voor de duur van een siteverzameling zich aanpassen aan de veranderende verwerking behoeften en patronen van uw toepassing openen. Zie [prestaties DocumentDB](documentdb-performance-levels.md)voor meer informatie. 

>[AZURE.IMPORTANT] Verzamelingen zijn factureerbare entiteiten. De kosten wordt bepaald door de ingerichte doorvoer van de verzameling gemeten in eenheden van de aanvraag per seconde samen met de totale verbruikte opslag in gigabytes. 

Hoeveel verzoek eenheden wordt gebruiken in een bepaalde bewerking zoals invoegen, verwijderen, query of opgeslagen procedure worden uitgevoerd? Een eenheid aanvraag is een genormaliseerde maateenheid voor de verwerking van de kosten van aanvragen. Het lezen van een document 1 KB is 1 RU, maar een nieuw vergaderverzoek wilt invoegen, vervangen of verwijderen van hetzelfde document wordt verbruikt meer verwerking van de service en daardoor meer verzoek eenheden. Elk antwoord van de service bestaat uit een aangepaste header (`x-ms-request-charge`) die de aanvraag eenheden verbruikt voor het verzoek-rapporten. Deze koptekst is ook toegankelijk is via de [SDK's](documentdb-sdk-dotnet.md). In de SDK .NET is [RequestCharge](https://msdn.microsoft.com/library/azure/dn933057.aspx#P:Microsoft.Azure.Documents.Client.ResourceResponse`1.RequestCharge) een eigenschap van het object [ResourceResponse](https://msdn.microsoft.com/library/azure/dn799209.aspx) . Als u wilt voor het schatten van uw behoeften doorvoer voordat u een enkele oproep plaatst, kunt u de [capaciteit teamplanner](documentdb-request-units.md#estimating-throughput-needs) om u te helpen met deze schatting. 

>[AZURE.NOTE] De basislijn van 1 verzoek eenheid voor een document 1 KB overeenkomt met een eenvoudige ophalen van het document met [Sessie consistentie](documentdb-consistency-levels.md). 

Er zijn verschillende factoren die van invloed zijn op de eenheden die zijn verzoek voor een bewerking tegen een account van de database DocumentDB verbruikt. Deze factoren zijn:

- Grootte van het document. Als grootte van document de eenheden die zijn verbruikt om te lezen of schrijven van dat de gegevens worden ook vergroten vergroot.
- Aantal eigenschappen. Uitgaande standaard indexeren van alle eigenschappen, de eenheden die zijn verbruikt u schrijft een document, neemt toe naarmate de eigenschap aantal toeneemt.
- De consistentie van de gegevens. Wanneer u gegevens consistentie niveaus van sterke of Staleness gebonden, wordt u aanvullende eenheden verbruikt om documenten te lezen.
- Geïndexeerde eigenschappen. Een index-beleid op elke verzameling bepaalt welke eigenschappen al dan niet standaard zijn geïndexeerd. U kunt uw aanvraag eenheidsverbruik verkleinen door het aantal geïndexeerde eigenschappen te beperken. 
- Document indexeren. Al dan niet standaard die elk document wordt automatisch geïndexeerd, wordt u minder verzoek eenheden gebruiken als u niet wilt indexeren enkele van uw documenten.

Zie [DocumentDB verzoek eenheden](documentdb-request-units.md)voor meer informatie. 

Hier is bijvoorbeeld een tabel waarop u het aantal eenheden van de aanvraag inrichten met drie verschillende document formaten (1KB, 4KB en 64KB) en op twee verschillende prestatieniveaus (500 gelezen/tweede + 100 schrijft/tweede en 500 gelezen/tweede + 500 schrijft/tweede). De consistentie van de gegevens aan de sessie is geconfigureerd en het indexeren beleid is ingesteld op geen.

<table border="0" cellspacing="0" cellpadding="0">
    <tbody>
        <tr>
            <td valign="top"><p><strong>Grootte van document</strong></p></td>
            <td valign="top"><p><strong>Gelezen/tweede</strong></p></td>
            <td valign="top"><p><strong>Schrijft/tweede</strong></p></td>
            <td valign="top"><p><strong>Eenheden aanvragen</strong></p></td>
        </tr>
        <tr>
            <td valign="top"><p>1 KB</p></td>
            <td valign="top"><p>500</p></td>
            <td valign="top"><p>100</p></td>
            <td valign="top"><p>(500 *1) + (100* 5) = 1000 RU/s</p></td>
        </tr>
        <tr>
            <td valign="top"><p>1 KB</p></td>
            <td valign="top"><p>500</p></td>
            <td valign="top"><p>500</p></td>
            <td valign="top"><p>(500 *5) + (100* 5) = 3000 RU/s</p></td>
        </tr>
        <tr>
            <td valign="top"><p>4 KB</p></td>
            <td valign="top"><p>500</p></td>
            <td valign="top"><p>100</p></td>
            <td valign="top"><p>(500 *1.3) + (100* 7) = 1,350 RU/s</p></td>
        </tr>
        <tr>
            <td valign="top"><p>4 KB</p></td>
            <td valign="top"><p>500</p></td>
            <td valign="top"><p>500</p></td>
            <td valign="top"><p>(500 *1.3) + (500* 7) = 4,150 RU/s</p></td>
        </tr>
        <tr>
            <td valign="top"><p>64 KB</p></td>
            <td valign="top"><p>500</p></td>
            <td valign="top"><p>100</p></td>
            <td valign="top"><p>(500 *10) + (100* 48) = 9,800 RU/s</p></td>
        </tr>
        <tr>
            <td valign="top"><p>64 KB</p></td>
            <td valign="top"><p>500</p></td>
            <td valign="top"><p>500</p></td>
            <td valign="top"><p>(500 *10) + (500* 48) = 29.000 RU/s</p></td>
        </tr>
    </tbody>
</table>

Query's, opgeslagen procedures en triggers verbruikt verzoek eenheden op basis van de complexiteit van de bewerkingen die wordt uitgevoerd. Als u uw toepassing ontwikkelt, controleren de koptekst van de aanvraag boete voor meer informatie over hoe elke bewerking verzoek eenheid capaciteit verbruikt.  


## <a name="choice-of-consistency-level-and-throughput"></a>Keuze van consistentie niveau en doorvoer
De keuze van standaard consistentie niveau is invloed op de doorvoer en latentie. Zowel via programmacode als via de portal van Azure, kunt u het niveau van de consistentie standaard instellen. U kunt ook het niveau van de consistentie op basis van de per verzoek negeren. Het niveau van de consistentie is standaard ingesteld op **sessie**monotone lezen/schrijven en lees uw garanties schrijven. Sessie consistentie is ideaal voor gebruiker centraal toepassingen en bevat een ideale verhouding tussen de consistentie en prestaties voor-en nadelen.    

Zie voor instructies over het wijzigen van uw niveau consistentie op de Azure-portal, [het beheren van een DocumentDB-Account](documentdb-manage-account.md#consistency). Of voor meer informatie over de consistentie niveaus, raadpleegt u [werken met consistentie niveaus](documentdb-consistency-levels.md).

## <a name="provisioned-document-storage-and-index-overhead"></a>Ingerichte document opslagcapaciteit en de index realiseren
DocumentDB ondersteunt het maken van één partition zowel gepartitioneerde collecties. Elke partition in DocumentDB ondersteunt tot 10 GB SSD back-opslag. De 10GB document opslag bevat de documenten plus de opslagruimte voor de index. Standaard is een verzameling DocumentDB geconfigureerd voor alle documenten automatisch indexeren zonder expliciet een secundaire indexen of schema. Op basis van toepassingen gebruiken DocumentDB, is de realiseren typische index tussen 2-20%. De indexing-technologie die wordt gebruikt door DocumentDB zorgt ervoor dat ongeacht de waarden van de eigenschappen, de index realiseren niet meer dan meer dan 80% van de grootte van de documenten met de standaardinstellingen. 

Standaard worden alle documenten door DocumentDB automatisch geïndexeerd. Echter als u de index realiseren verfijnen wilt, kunt u kiezen om in te trekken dat bepaalde documenten worden geïndexeerd op het moment van invoegen of het vervangen van een document, zoals wordt beschreven in [DocumentDB indexing beleid](documentdb-indexing-policies.md). U kunt een verzameling DocumentDB als u wilt uitsluiten van alle documenten binnen de verzameling van indexering configureren. U kunt ook een verzameling DocumentDB als u wilt indexeren selectief alleen bepaalde eigenschappen of netwerkpaden door jokertekens JSON documenten, zoals wordt beschreven in het [configureren van de indexing-beleid van een siteverzameling](documentdb-indexing-policies.md#configuring-the-indexing-policy-of-a-collection)configureren. Als u de eigenschappen of documenten ook verbetert de schrijfbewerkingen – wat betekent dat u gebruikt minder verzoek eenheden.   

## <a name="next-steps"></a>Volgende stappen

Als u wilt blijven leren over de werking van DocumentDB, Zie [partitionering en schaal in Azure DocumentDB](documentdb-partition-data.md).

Zie voor instructies over het controleren van prestaties op de Azure-portal, [Monitor een DocumentDB-account](documentdb-monitor-accounts.md). Zie [prestatieniveaus in DocumentDB](documentdb-performance-levels.md)voor meer informatie over het kiezen van prestaties voor siteverzamelingen.
 
