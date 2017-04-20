<properties
   pageTitle="Gegevens globaal met DocumentDB distribueren | Microsoft Azure"
   description="Meer informatie over de wereld-schaal geografische-replicatie, failover en gegevens herstel met globale databases van Azure DocumentDB, een volledig beheerde NoSQL database-service."
   services="documentdb"
   documentationCenter=""
   authors="kiratp"
   manager="jhubbard"
   editor=""/>

<tags
   ms.service="documentdb"
   ms.devlang="multiple"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="08/15/2016"
   ms.author="kipandya"/>
   
   
# <a name="distribute-data-globally-with-documentdb"></a>Gegevens globaal met DocumentDB distribueren

> [AZURE.NOTE] Globale verdeling van DocumentDB databases is algemeen beschikbaar en is automatisch ingeschakeld voor uw nieuwe DocumentDB-accounts. We werken om in te schakelen globale verdeling voor alle bestaande accounts, maar in de tussentijd, indien distributielijsten ingeschakeld voor uw account gewenst, neem [contact opnemen met ondersteuning](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) en we wordt inschakelen voor u nu.

Azure DocumentDB is bedoeld om te voldoen aan de behoeften van IoT toepassingen die bestaat uit miljoenen globaal verdeelde apparaten en internet schaal toepassingen die ervoor zorgen dat ten zeerste heeft gereageerd ervaringen gebruikers kant van de wereld zitten. Deze databasesystemen betrokken de uitdaging van lage latentie access bereiken met toepassingsgegevens uit meerdere geografische regio's met een duidelijke gegevens consistentie en beschikbaarheid garanties. Als een globaal gedistribueerde database-systeem, wordt de globale distributie van gegevens in DocumentDB vereenvoudigd door te bieden volledig beheerde, meerdere landen/regio database-accounts die wissen compromissen nodig zijn tussen consistentie, beschikbaarheid en prestaties, alle bijbehorende garanties bieden. DocumentDB database accounts worden geleverd met hoge beschikbaarheid, één cijfer ms vertragingstijden, meerdere [niveaus goed gedefinieerde consistentie] [consistency], regionale overname met meerdere homing API's en de mogelijkheid om te elastically schaal doorvoer en opslag overal ter wereld. 

  
## <a name="configuring-multi-region-accounts"></a>Meerdere landen/regio-accounts configureren

Configureren van uw account DocumentDB aan de overal ter wereld nieuwe schaal kan worden uitgevoerd in een handomdraai via de [portal van Azure](documentdb-portal-global-replication.md). U hoeft te selecteert u het niveau van de juiste consistentie tussen verschillende niveaus van de ondersteunde duidelijke consistentie en niet het getal van Azure regio's koppelen aan uw databaseaccount is. DocumentDB consistentie niveaus bieden wissen compromissen nodig zijn tussen specifieke consistentie garantie en prestaties. 

![DocumentDB biedt meerdere, ook (beperkte) consistentie modellen waaruit u kunt kiezen gedefinieerd][1]

(Beperkte) consistentie modellen waaruit u kunt kiezen DocumentDB biedt meerdere, ook worden gedefinieerd.

Het niveau van de juiste consistentie selecteren afhankelijk van de gegevens consistentie uw applicaties garanderen. DocumentDB automatisch gerepliceerd van uw gegevens over alle opgegeven regio's en de consistentie die u hebt geselecteerd voor uw databaseaccount garandeert. 


## <a name="using-multi-region-failover"></a>Gebruik van meerdere landen/regio failover 

Azure DocumentDB is mogen transparant failover database accounts op meerdere Azure regio's – de nieuwe [meervoudige homing API's] [ developingwithmultipleregions] garantie dat uw app kunt blijven gebruiken een logische eindpunt en ononderbroken door de overname. Failover door u wordt gecontroleerd, mits de flexibiliteit onderbrengen van uw database op account in de gebeurtenis een van de reeks mislukken voorwaarden optreden, met inbegrip van toepassing, infrastructuur, service of regionale fouten (reële of gesimuleerd). Bij een DocumentDB regionale storing wordt de service, transparant mislukt via uw databaseaccount en uw toepassing nog steeds toegang tot gegevens zonder beschikbaarheid kwijt te raken. Terwijl DocumentDB [99,99% beschikbaarheid serviceovereenkomsten biedt][sla], kunt u van uw toepassing beëindigen aan einde beschikbaarheid eigenschappen testen door een regionale fout beide, [via een programma] simuleren[ arm] en via de Portal Azure.


## <a name="scaling-across-the-planet"></a>Schaalbaarheid over wereld
DocumentDB kunt u onafhankelijk doorvoer inrichten en gebruiken van de opslagruimte voor elke siteverzameling DocumentDB met een schaal, globaal in alle regio's die zijn gekoppeld aan uw databaseaccount. Een verzameling DocumentDB is automatisch globaal distributed en beheerd op alle van de regio's die zijn gekoppeld aan uw databaseaccount. Collecties in uw databaseaccount kunnen worden verdeeld over een van de Azure regio's waarin de [DocumentDB-service is beschikbaar][serviceregions]. 

De gegevensdoorvoer gekocht en de opslagruimte voor elke siteverzameling DocumentDB verbruikt is automatisch ingericht in alle regio's gelijkmatig. Hiermee kan de toepassing naadloos schaal over de wereldbol [betaalt alleen voor de gegevensdoorvoer en opslagruimte u binnen elk uur gebruikt][pricing]. Bijvoorbeeld als u hebt deze is ingericht 2 miljoen RUs voor een siteverzameling DocumentDB, krijgt klikt u vervolgens elke regio die is gekoppeld aan uw databaseaccount 2 miljoen RUs voor deze siteverzameling. Dit wordt geïllustreerd hieronder.

![Schaal doorvoer voor een siteverzameling DocumentDB over de vier gebieden][2]

DocumentDB garandeert < 10 ms lezen en < 15 ms vertragingstijden bij P99 schrijven. De lees aanvragen datacenter van de rand om te garanderen de [consistentie vereisten die u hebt geselecteerd]voor het nooit tijdspanne[consistency]. De schrijft zijn altijd quorum vastgelegde lokaal voordat ze worden bevestigd aan de clients. Elke databaseaccount is geconfigureerd met schrijven regio prioriteit. Het gebied dat is opgegeven met de hoogste prioriteit fungeert als het huidige schrijven gebied voor het account. Alle SDK's doorsturen transparant schrijven naar de database-account naar het huidige gebied voor schrijven. 

Ten slotte, aangezien DocumentDB volledig [schema-agnostic is] [ vldb] -u dat nooit hoeft te beheren/bijwerkt schema's maken of secundaire indexen bang over meerdere datacenters. Uw [SQL-query's] [ sqlqueries] blijven werken terwijl uw toepassing en gegevensmodellen gaat u verder met het ontwikkelen. 


## <a name="enabling-global-distribution"></a>Globale verdeling inschakelen 

U kunt beslissen dat uw gegevens lokaal of globaal gedistribueerd door een van beide koppelen aan een of meer Azure regio's met een DocumentDB database account. U kunt toevoegen of verwijderen van regio's aan uw databaseaccount op elk gewenst moment. Zie [het uitvoeren van algemene databasereplicatie DocumentDB met behulp van de Azure portal](documentdb-portal-global-replication.md)voor informatie over het inschakelen van globale verdeling met behulp van de portal. Zie [ontwikkelen met accounts voor meerdere landen/regio DocumentDB](documentdb-developing-with-multiple-regions.md)distributielijsten via programmacode inschakelen.

## <a name="next-steps"></a>Volgende stappen

Meer informatie over de distributie gegevens globaal met DocumentDB in de volgende artikelen:

* [Inrichting doorvoer en opslag voor een siteverzameling] [throughputandstorage]
* [Als u meerdere homing API's via REST. .NET, Java, Python en knooppunt SDK 's] [developingwithmultipleregions]
* [Consistentie niveaus in DocumentDB] [consistency]
* [Beschikbaarheid serviceovereenkomsten] [sla]
* [Databaseaccount beheren] [manageaccount]

[1]: ./media/documentdb-distribute-data-globally/consistency-tradeoffs.png
[2]: ./media/documentdb-distribute-data-globally/collection-regions.png

<!--Reference style links - using these makes the source content way more readable than using inline links-->
[pcolls]: documentdb-partition-data.md
[consistency]: documentdb-consistency-levels.md
[consistencytradeooffs]: ./documentdb-consistency-levels/#consistency-levels-and-tradeoffs
[developingwithmultipleregions]: documentdb-developing-with-multiple-regions.md
[createaccount]: documentdb-create-account.md
[manageaccount]: documentdb-manage-account.md
[manageaccount-consistency]: documentdb-manage-account.md#consistency
[throughputandstorage]: documentdb-manage.md
[arm]: documentdb-automation-resource-manager-cli.md
[regions]: https://azure.microsoft.com/regions/
[serviceregions]: https://azure.microsoft.com/en-us/regions/#services 
[pricing]: https://azure.microsoft.com/pricing/details/documentdb/
[sla]: https://azure.microsoft.com/support/legal/sla/documentdb/ 
[vldb]: http://www.vldb.org/pvldb/vol8/p1668-shukla.pdf
[sqlqueries]: documentdb-sql-query.md

