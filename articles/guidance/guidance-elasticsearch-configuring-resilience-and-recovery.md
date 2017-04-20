<properties
   pageTitle="Flexibiliteit en herstel configureren op Elasticsearch op Azure"
   description="Overwegingen voor tolerantie en herstelbestanden voor Elasticsearch."
   services=""
   documentationCenter="na"
   authors="dragon119"
   manager="bennage"
   editor=""
   tags=""/>

<tags
   ms.service="guidance"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="09/22/2016"
   ms.author="masashin"/>
   
# <a name="configuring-resilience-and-recovery-on-elasticsearch-on-azure"></a>Flexibiliteit en herstel configureren op Elasticsearch op Azure

[AZURE.INCLUDE [pnp-header](../../includes/guidance-pnp-header-include.md)]

In dit artikel maakt [deel uit van een reeks](guidance-elasticsearch.md). 

Een belangrijke functie van Elasticsearch is de ondersteuning die deze voor tolerantie in het geval van knooppunten en/of netwerk partition gebeurtenissen biedt. Replicatie is het meest opvallende manier waarin u de tolerantie van een cluster verbeteren kunt, zodat Elasticsearch om ervoor te zorgen dat meer dan één exemplaar van een gegevensitem beschikbaar op verschillende knooppunten is geval één knooppunt niet toegankelijk worden moet. Als een knooppunt tijdelijk niet beschikbaar is wordt, kunnen andere knooppunten met kopieën van gegevens uit het ontbrekende knooppunt de ontbrekende gegevens kunnen dienen totdat het probleem is opgelost. In het geval van een langere periode uitgifte, het ontbrekende knooppunt kan worden vervangen door een nieuwe record en Elasticsearch kunt de gegevens terugzetten naar het nieuwe knooppunt vanaf de replica's.

Hier wordt de tolerantie en herstelbestanden opties beschikbaar voor communicatie met Elasticsearch wanneer die worden gehost in Azure wordt aangegeven en beschrijven van enkele belangrijke aspecten van een Elasticsearch cluster waarmee u rekening houden moet om te minimaliseren van de kans op verlies van gegevens en uitgebreide gegevens herstel tijden samenvatten.

In dit artikel is ook ziet u enkele steekproef tests die zijn uitgevoerd om weer te geven van de effecten van verschillende soorten fouten voor een cluster Elasticsearch en hoe het systeem moet reageren zoals wordt hersteld.

Een cluster Elasticsearch wordt replica's voor het behoud van beschikbaarheid en lees prestaties te verbeteren. Replica's moeten worden opgeslagen op verschillende VMs uit de primaire shards die ze repliceren. De bedoeling is dat als de gegevensknooppunt hostingprovider VM mislukt of niet meer beschikbaar is, het systeem kan blijven functioneren met de VMs vasthouden van de replica's.

## <a name="using-dedicated-master-nodes"></a>Speciale basispagina knooppunten gebruiken

Een van de knooppunten in een cluster Elasticsearch is gekozen als het knooppunt basispagina. Het doel van dit knooppunt is cluster management bewerkingen zoals uit te voeren:

- Mislukte knooppunten detecteren en overgaan naar replica's.

- Shards naar het knooppunt werkbelasting verdelen verplaatsen.

- Shards herstellen wanneer een knooppunt weer online is gebracht.

U moet u overwegen speciale basispagina knooppunten in kritieke clusters en zorg ervoor dat er 3 speciale knooppunten waarvan alleen de rol is moeten basispagina. Deze configuratie minder intensief resourcewerk die deze knooppunten moeten uitvoeren (zij niet opslag van gegevens of afhandeling van query's) en helpt te verbeteren clusterstabiliteit. Alleen een van deze knooppunten worden gekozen, maar de anderen een kopie van de systeemstatus moeten bevatten en kunnen overnemen moet het gekozen outmodel dan mislukt.

## <a name="controlling-high-availability-with-azure--update-domains-and-fault-domains"></a>Beschikbaarheid met Azure-update domeinen en foutenstructuuranalyse domeinen beheren 

Verschillende VMs kunnen delen dezelfde fysieke hardware. Een enkel rek kunt een aantal VMs hosten in een Azure datacenter, en alle volgende VMs delen een gemeenschappelijke power bron- en netwerk schakeloptie. Een storing rek-niveau kunt daarom van invloed zijn op een aantal VMs. Het concept van domeinen voor foutenstructuuranalyse Azure gebruikt om te proberen te verspreiden dit risico. Een domein foutenstructuuranalyse komt ongeveer overeen met een groep VMs die hetzelfde rek delen. Om ervoor te zorgen dat een fout rek-niveau niet vastlopen knooppunt en de knooppunten alle bijbehorende replica's tegelijk te houden, moet u ervoor zorgen dat de VMs zijn verdeeld over foutenstructuuranalyse domeinen.

Daarnaast kunnen VMs naar worden gebracht omlaag door de [Azure stof Controller](https://azure.microsoft.com/documentation/videos/fabric-controller-internals-building-and-updating-high-availability-apps/) gepland onderhoud en besturingssysteem upgrades uitvoeren. Azure wijst VMs als u wilt bijwerken van domeinen. Als een gebeurtenis gepland onderhoud, kunnen alleen VMs in een domein één update, zal plaatsvinden op elk gewenst moment. VMs in andere update-domeinen blijven uitgevoerd totdat de VMs in het update-domein is bijgewerkt opnieuw online worden gebracht. Daarom moet u ook voor zorgen dat de VMs hostingprovider knooppunten en hun replica's behoren naar andere update-domeinen waar mogelijk.

> [AZURE.NOTE] Zie voor meer informatie over foutenstructuuranalyse domeinen en update domeinen [beheren de beschikbaarheid van virtuele machines][].

U kunt geen expliciet een VM naar een specifieke update domain en foutenstructuuranalyse domein toewijzen. Deze toewijzing wordt bepaald door Azure wanneer VMs worden gemaakt. U kunt echter opgeven dat VMs moeten worden gemaakt als onderdeel van een set beschikbaarheid. VMs in bepaalde beschikbaarheid worden verdeeld over de update domeinen en foutenstructuuranalyse domeinen. Als u handmatig VMs maakt, wordt elke beschikbaarheid van de set met twee foutenstructuuranalyse domeinen en vijf update domeinen door Azure gemaakt. VMs zijn toegewezen aan deze domeinen foutenstructuuranalyse en bijwerken van domeinen functioneert afronden, zoals verder VMs is ingericht, als volgt: 

| VM | Domein voor foutenstructuuranalyse | Domein bijwerken |
|----|--------------|---------------|
|  1 |            0 |             0 |
|  2 |            1 |             1 |
|  3 |            0 |             2 |
|  4 |            1 |             3 |
|  5 |            0 |             4 |
|  6 |            1 |             0 |
|  7 |            0 |             1 |

> [AZURE.IMPORTANT] Als u met bronbeheer Azure VMs maakt, kan elke set beschikbaarheid maximaal 3 foutenstructuuranalyse domeinen en 20 update domeinen worden toegewezen. Dit is een goede reden voor het gebruik van de Resource-Manager.

In het algemeen, plaats alle VMs die hetzelfde doel in bepaalde beschikbaarheid dienen, maar de beschikbaarheid van de verschillende sets maken voor VMs die verschillende functies uitvoeren. Met dit betekent dat u rekening moet houden Elasticsearch maken ten minste afzonderlijk beschikbaarheid wordt ingesteld voor:

- VMs gegevensknooppunten hostingprovider.
- VMs client knooppunten hostingprovider (als u deze gebruikt).
- VMs basispagina knooppunten hostingprovider.

Daarnaast moet u ervoor zorgen dat elke knooppunt in een cluster is op de hoogte van de update domain en foutenstructuuranalyse domein waarbij deze hoort. Deze informatie kan helpen om ervoor te zorgen dat Elasticsearch niet shards en hun replica's in de dezelfde foutenstructuuranalyse maken en bijwerken van domeinen minimaliseren de mogelijkheid van een shard en bijbehorende replica's tegelijkertijd omlaag worden gebracht. U kunt een knooppunt Elasticsearch als u wilt spiegelen van de verdeling hardware van het cluster door te configureren [shard toewijzing chatstatus](https://www.elastic.co/guide/en/elasticsearch/reference/current/allocation-awareness.html#allocation-awareness)configureren. U kunt bijvoorbeeld een paar aangepaste knooppunt kenmerken naam *faultDomain* en *updateDomain* in het bestand elasticsearch.yml als volgt definiëren:

```yaml
node.faultDomain: \${FAULTDOMAIN}
node.updateDomain: \${UPDATEDOMAIN}
```

In dit geval de kenmerken worden ingesteld met de waarden die zijn gehouden de * \${FAULTDOMAIN}* en * \${UPDATEDOMAIN}* omgevingsvariabelen wanneer Elasticsearch wordt gestart. U moet ook de volgende items toevoegen aan het bestand Elasticsearch.yml om aan te geven dat *faultDomain* en *updateDomain* toegewezen chatstatus kenmerken zijn en geef de sets met geldige waarden voor deze kenmerken:

```yaml
cluster.routing.allocation.awareness.force.updateDomain.values: 0,1,2,3,4
cluster.routing.allocation.awareness.force.faultDomain.values: 0,1
cluster.routing.allocation.awareness.attributes: updateDomain, faultDomain
```

U kunt shard toewijzing chatstatus gebruiken in combinatie met [shard toewijzing filteren](https://www.elastic.co/guide/en/elasticsearch/reference/2.0/shard-allocation-filtering.html#shard-allocation-filtering) expliciet opgeven welke knooppunten kunnen hosten shards voor een bepaald index.

Als u schalen dan het aantal foutenstructuuranalyse domeinen en domeinen van de update in een set beschikbaarheid wilt, kunt u VMs maken in de beschikbaarheid van de extra sets. U moet echter begrijpen knooppunten in de beschikbaarheid van de verschillende sets kunnen worden gehouden die omlaag voor onderhoud tegelijk. Probeer om ervoor te zorgen dat elke shard en ten minste één van de replica's bevinden zich in dezelfde beschikbaarheid set.

> [AZURE.NOTE] Er is momenteel een limiet van 100 VMs per beschikbaarheid instellen. Zie [Azure-abonnement en limieten van de service, quota's en beperkingen](../azure-subscription-service-limits.md)voor meer informatie.

### <a name="backup-and-restore"></a>Back-up en herstellen

Gebruik van replica's biedt geen volledige bescherming tegen volledige uitval (zoals per ongeluk verwijderen van de hele cluster). U moet ervoor zorgen dat u regelmatig een back-up van deze gegevens in een cluster en dat er een geprobeerd en getest strategie voor het systeem terugzetten vanuit deze back-ups.

Gebruik de Elasticsearch [momentopname en deze herstellen API's](https://www.elastic.co/guide/en/elasticsearch/reference/current/modules-snapshots.html) : elastische niet hoofdletters deze. >> back-up maken en terugzetten van indexen. Momentopnamen kunnen worden opgeslagen in een gedeelde bestandssysteem. U kunt ook Plug-ins beschikbaar zijn die kunt momentopnamen schrijven op het Hadoop distributed file system (HDFS) (de [invoegtoepassing HDFS](https://github.com/elasticsearch/elasticsearch-hadoop/tree/master/repository-hdfs)) of op Azure opslag (de [Azure-invoegtoepassing](https://github.com/elasticsearch/elasticsearch-cloud-azure#azure-repository)).

Houd rekening met de volgende punten wanneer de momentopname opslag om te selecteren:

- [Azure bestandsopslag](https://azure.microsoft.com/services/storage/files/) kunt u een gedeelde bestandssysteem die toegankelijk is vanuit alle knooppunten implementeren.

- De invoegtoepassing voor HDFS alleen gebruiken als u Elasticsearch worden uitgevoerd in combinatie met Hadoop.

- De invoegtoepassing voor HDFS, moet u voor het uitschakelen van de beveiligingsinstellingen van Java uitgevoerd binnen het Elasticsearch-exemplaar van de Java virtual machine (JVM).

- De invoegtoepassing voor HDFS ondersteunt een bestandssysteem HDFS-compatibele, mits de juiste Hadoop-configuratie wordt gebruikt met Elasticsearch.

  
## <a name="handling-intermittent-connectivity-between-nodes"></a>Afhandeling niet altijd verbinding tussen knooppunten

Netwerk af en toe vlot, VM opnieuw is opgestart na dagelijks onderhoud aan het datacenter en andere soortgelijke gebeurtenissen kunnen leiden tot knooppunten te worden tijdelijk niet beschikbaar. In deze situaties, waarbij de gebeurtenis waarschijnlijk korte levensduur is, de realiseren van de shards opnieuw tweemaal voorkomt in snel achter elkaar (één keer als de fout is gedetecteerd en opnieuw wanneer het knooppunt zichtbaar voor de hoofdlijst) kan worden omgezet in een aanzienlijk realiseren die van invloed op prestaties. U kunt voorkomen dat tijdelijke knooppunt inaccessibility veroorzaakt door het model, het cluster door in te stellen opnieuw uit te de *vertraagd\_time-out* eigenschap van een index, of voor alle indexen. Het onderstaande voorbeeld wordt de vertraging op 5 minuten:

```http
PUT /_all/settings
{
    "settings": {
    "index.unassigned.node_left.delayed_timeout": "5m"
    }
}
```

Zie [uitstellen toegewezen wanneer een knooppunt verlaat](https://www.elastic.co/guide/en/elasticsearch/reference/current/delayed-allocation.html)voor meer informatie.

In een netwerk dat vatbaar voor onderbrekingen, kunt u ook de parameters die een model om terug te detecteren wanneer een ander knooppunt niet meer toegankelijke is configureren wijzigen. Deze parameters maken deel uit van de [zen discovery](https://www.elastic.co/guide/en/elasticsearch/reference/current/modules-discovery-zen.html#modules-discovery-zen) -module Elasticsearch voorzien en kunt u deze instellen in het bestand Elasticsearch.yml. Bijvoorbeeld, is de parameter *discovery.zen.fd.ping.retries* geeft aan hoe vaak een basispagina knooppunt probeert te ping van een ander knooppunt in het cluster voordat u besluit dat deze is mislukt. Deze parameter standaard ingesteld op 3, maar kunt u deze als volgt wijzigen:

```yaml
discovery.zen.fd.ping_retries: 6
```

## <a name="controlling-recovery"></a>Herstel bepalen

Wanneer de verbinding met een knooppunt na een fout is hersteld, moet een shards op dat knooppunt worden hersteld als u wilt ze weer up-to-date. Standaard herstelt Elasticsearch shards in de volgende volgorde:

- Door de aanmaakdatum van een omgekeerde index. Nieuwere indexen worden hersteld voordat u oudere indexen.

- Door de indexnaam van de omgekeerde. Indexen met namen die alfanumeriek groter dan anderen zijn worden eerst hersteld.

Als u enkele indexen zijn belangrijker dan anderen, maar niet overeenkomen met deze criteria die u kunt de volgorde van indexen negeren door in te stellen van de eigenschap *index.priority* . Indexen met een hogere waarde voor deze eigenschap wordt hersteld voordat indexen die een lagere waarde hebben:

```http
PUT low_priority_index
{
    "settings": {
        "index.priority": 1
    }
}

PUT high_priority_index
{
    "settings": {
        "index.priority": 10
    }
}
```

Zie [Index herstel prioriteit](https://www.elastic.co/guide/en/elasticsearch/reference/2.0/recovery-prioritization.html#recovery-prioritization)voor meer informatie.

U kunt controleren het herstelproces voor een of meer indexen met de * \_herstel* API:

```http
GET /high_priority_index/_recovery?pretty=true
```

Zie [Indexen herstel](https://www.elastic.co/guide/en/elasticsearch/reference/current/indices-recovery.html#indices-recovery)voor meer informatie.

> [AZURE.NOTE] Een cluster met shards die moeten worden hersteld heeft een status van *gele* om aan te geven dat niet alle shards momenteel beschikbaar zijn. Wanneer alle shards beschikbaar zijn, wordt de status cluster moet terugkeren naar *groen*. Een cluster met een *rode* status geeft aan dat een of meer shards fysiek ontbreken, kan het nodig is om gegevens te herstellen uit een back-up zijn.

## <a name="preventing-split-brain"></a>Gesplitste hersenen voorkomen 

Een gesplitste hersenen kan optreden als de verbindingen tussen knooppunten mislukt. Als een basispagina knooppunt niet bereikbaar aan een deel van het cluster wordt, een verkiezingsuitslagen wordt uitgevoerd in het netwerksegment dat nog moet beschikbaar en een ander knooppunt worden de hoofdlijst. In een cluster onjuist geconfigureerd is het mogelijk voor elk onderdeel van het cluster hebben verschillende modellen gegevensinconsistenties of beschadigde bestanden. Dit verschijnsel wordt genoemd in een *gesplitste hersenen*.

U kunt de kans van een gesplitste hersenen verlagen door configureren voor de *minimale\_basispagina\_knooppunten* eigenschap van de rollengroep discovery-module, in het bestand elasticsearch.yml. Deze eigenschap wordt aangegeven hoeveel knooppunten zijn beschikbaar voor het inschakelen van de verkiezingsuitslagen van een model. Het volgende voorbeeld wordt de waarde van deze eigenschap op 2:

```yaml
discovery.zen.minimum_master_nodes: 2
```

Deze waarde moet worden ingesteld op het laagste grootste deel van het aantal knooppunten die kunnen voldoen aan de basispagina rol. Als uw cluster heeft 3 basispagina knooppunten, bijvoorbeeld *minimale\_basispagina\_knooppunten* moet worden ingesteld op 2. Als er 5 basispagina knooppunten, *minimale\_basispagina\_knooppunten* moet worden ingesteld op 3. U kunt een oneven getal van de basispagina knooppunten in het ideale geval nodig hebt.

> [AZURE.NOTE] Het is mogelijk voor een gesplitste hersenen naar optreden als er meerdere basispagina knooppunten in hetzelfde cluster tegelijk worden gestart. Dit exemplaar is niet vaak voorkomen, kunt u dit voorkomen door knooppunten serie beginnen met een korte vertraging (5 seconden) tussen elk. 

## <a name="handling-rolling-updates"></a>Afhandeling lopende updates

Als u een software upgrade uitvoert naar knooppunten uzelf (zoals migreren naar een nieuwere versie of de uitvoering van een patch), moet u mogelijk inzetten op afzonderlijke knooppunten die moeten worden ze offline nemen terwijl de rest van het cluster beschikbaar blijft. In dit geval kunt u het volgende proces implementeren. 

1. Controleer of dat die shard u opnieuw toewijzen voldoende is vertraagd als u wilt voorkomen dat het gekozen outmodel shards vanaf een ontbrekende knooppunt over de rest van het cluster opnieuw. Standaard shard herverdeling voor 1 minuut is vertraagd, maar u kunt de duur verlengen als een knooppunt waarschijnlijk niet beschikbaar is voor een langere periode. Het volgende voorbeeld wordt de vertraging op 5 minuten verhoogd:

    ```http
    PUT /_all/_settings
    {
        "settings": {
            "index.unassigned.node_left.delayed_timeout": "5m"
        }
    }
    ```

    > [AZURE.IMPORTANT] U kunt ook shard herverdeling volledig uitschakelen door in te stellen van de *cluster.routing.allocation.enable* van het cluster op *geen*. U moet echter voorkomen met behulp van deze methode als nieuwe indexen waarschijnlijk worden gemaakt hebben terwijl het knooppunt offline is, zoals dit leiden de indextoewijzing tot kan mislukt resulteert in een cluster met rode status.

2. Stop Elasticsearch op het knooppunt worden bijgehouden. Als Elasticsearch wordt uitgevoerd als een service, kunt u mogelijk het proces stoppen op een gecontroleerde manier met behulp van een opdracht besturingssysteem gebruikt. Het volgende voorbeeld ziet u hoe de service Elasticsearch op één knooppunt waarop Ubuntu stoppen:

    ```bash
    service elasticsearch stop
    ```

    U kunt ook de API afsluiten rechtstreeks op het knooppunt gebruiken:

    ```http
    POST /_cluster/nodes/_local/_shutdown
    ```

3. De benodigde onderhoud aan het knooppunt uitvoeren

4. Start het knooppunt opnieuw en wacht totdat deze deel te nemen aan het cluster.

5. Shard toewijzing opnieuw inschakelen:

    ```http
    PUT /_cluster/settings
    {
        "transient": {
            "cluster.routing.allocation.enable": "all"
        }
    }
    ```

> [AZURE.NOTE] Als u voor het behoud van meer dan één knooppunt moet, herhaalt u stappen 2&ndash;4 op elk knooppunt voordat u opnieuw inschakelen shard toegewezen.

Als u kunt, beëindigt u indexering van nieuwe gegevens tijdens dit proces. Hierdoor minimaliseren herstel wanneer knooppunten terug online gezet en opnieuw deelnemen aan het cluster.

Houd rekening met automatische updates naar items zoals JVM (in het ideale geval uitschakelen automatische updates voor deze items), met name wanneer uitgevoerd Elasticsearch onder Windows. De Java update-agent automatisch de meest recente versie van Java kunt downloaden, maar mogelijk Elasticsearch te worden gestart om de update zijn doorgevoerd. Dit kan resulteren in ongecoördineerde tijdelijk verlies van knooppunten, afhankelijk van hoe de Java-Update-agent is geconfigureerd. Dit kan resulteren in verschillende exemplaren van Elasticsearch in hetzelfde cluster met verschillende versies van de JVM waardoor compatibiliteitsproblemen.

## <a name="testing-and-analyzing-elasticsearch-resilience-and-recovery"></a>Testen en analyseren Elasticsearch flexibiliteit en herstellen

Dit onderwerp vindt een reeks tests die zijn uitgevoerd als u wilt evalueren de flexibiliteit en herstel van een Elasticsearch cluster met drie gegevensknooppunten en drie basispagina knooppunten.

De volgende scenario's zijn getest: 

- Knooppunt is mislukt en opnieuw starten zonder verlies van gegevens. Een gegevensknooppunt wordt gestopt en 5 minuten door te gaan. Elasticsearch is geconfigureerd voor het niet opnieuw toewijzen ontbrekende shards in dit interval, zodat er geen aanvullende I/O wordt weergegeven in het shards navigeren. Wanneer het knooppunt opnieuw opstart, kunt u met het herstelproces de shards op dat knooppunt terug up-to-date.

- Fout bij het knooppunt met ernstige gegevensverlies. Een gegevensknooppunt wordt gestopt en de gegevens die deze bevat deze de vorm van een ernstige schijf is mislukt wordt gewist. Het knooppunt is vervolgens opnieuw gestart (na 5 minuten), effectief die fungeert als vervanging voor het oorspronkelijke knooppunt. Het herstelproces is vereist opnieuw maken van de ontbrekende gegevens voor dit knooppunt en mogelijk shards gehouden op andere knooppunten verplaatsen.

- Knooppunt is mislukt en opnieuw starten zonder verlies van gegevens, maar met shard herverdeling. Een gegevensknooppunt wordt gestopt en de shards die deze bevat andere knooppunten worden herverdeeld. Het knooppunt vervolgens opnieuw is opgestart en meer herverdeling treedt op als u wilt vastleggen van het cluster.

- Schuivend updates. Elk knooppunt in de cluster wordt gestopt en opnieuw is opgestart na een korte interval simuleren machines opnieuw worden gestart nadat een software-update. Slechts één knooppunt is gestopt op elk gewenst moment. Shards worden niet herverdeeld terwijl een knooppunt niet beschikbaar is.

Elk scenario is onderhevig aan de dezelfde werklast, met inbegrip van een combinatie van gegevens opname taken, aggregaties en filter query's terwijl knooppunten zijn die u hebt gemaakt offline en hersteld. Het bulksgewijs bewerkingen invoegen in de werklast elk 1000 documenten opgeslagen en zijn uitgevoerd op één index terwijl de aggregaties en filter query's gebruikt als u een afzonderlijke index met verschillende miljoenen documenten. Dit was om in te schakelen van de prestaties van query's afzonderlijk beoordeeld worden vanuit het bulksgewijs invoegen. Elke index bevat vijf shards en één replica.

De volgende secties samenvatten de resultaten van deze tests, waarbij elke verminderde prestaties tijdens het knooppunt offline is of wordt hersteld en eventuele fouten die zijn verzonden. De resultaten worden weergegeven als geïllustreerd, waarbij de punten die een of meer knooppunten zijn ontbreken en schatting van de tijd voor het systeem volledig herstellen en bereiken van een soortgelijk niveau van prestaties en die aanwezig zijn voordat u de knooppunten offline kan worden gehaald was.

> [AZURE.NOTE] De test leidsels gebruikt voor het uitvoeren van deze tests zijn online beschikbaar. U kunt aanpassen en deze leidsels gebruiken om te controleren of de flexibiliteit en herstelmogelijkheden van uw eigen clusterconfiguraties. Zie [de geautomatiseerde Elasticsearch tolerantie tests uitvoeren][]voor meer informatie.

## <a name="node-failure-and-restart-with-no-data-loss-results"></a>Knooppunt is mislukt en opnieuw starten zonder verlies van gegevens: resultaten

<!-- TODO; reformat this pdf for display inline --> 

De resultaten van deze test worden weergegeven in het bestand [ElasticsearchRecoveryScenario1.pdf](https://github.com/mspnp/azure-guidance/blob/master/figures/Elasticsearch/ElasticSearchRecoveryScenario1.pdf). Profiel van de prestaties van de werkbelasting en fysieke bronnen voor elk knooppunt weergeven de grafieken in het cluster. Het eerste deel van de grafieken weergeven het systeem normaal gedurende ongeveer 20 minuten, waarna knooppunt 0 is afgesloten voor 5 minuten voordat u opnieuw worden gestart. De statistieken voor een verdere 20 minuten worden geïllustreerd; het systeem duurt ongeveer 10 minuten te herstellen en stabiliseren. Dit wordt geïllustreerd door de transactie tarieven en de reactietijden voor de verschillende werkbelastingen.

Houd rekening met de volgende punten:

- Tijdens de test zijn geen fouten gerapporteerd. Er zijn geen gegevens is verbroken en alle bewerkingen is voltooid.

- De transactie eenheidstarieven voor alle drie soorten bewerking (bulksgewijs invoegen, samenvoegquery en filterquery) verwijderd en de gemiddelde antwoord tijden is groter, terwijl het knooppunt 0 is offline.

- Tijdens de periode herstel, de transactie tarieven en -antwoord tijd voor de samenvoegquery en filterquery zijn geleidelijk bewerkingen teruggezet. De prestaties voor bulksgewijs invoegen hersteld voor een korte terwijl voordat effect. Dit is echter waarschijnlijk vanwege de hoeveelheid gegevens die worden veroorzaakt door de index gebruikt door de bulksgewijs invoegen om te laten groeien, en de transactie tarieven voor deze bewerking is zichtbaar voor naar vertragen voordat knooppunt 0, offline wordt is.

- In de grafiek CPU-gebruik voor knooppunt 0 ziet u verminderde activiteit tijdens de herstelfase dit is vanwege de verbeterde schijf en activiteit op het netwerk die worden veroorzaakt door het herstelproces is om, het knooppunt heeft geen gegevens die is gemist offline achterhalen en bijwerken van de shards die deze bevat.

- De shards voor de indexen niet precies worden gedistribueerd gelijkmatig op alle knooppunten. Er zijn twee indexen met 5 shards en 1 replica elke, waardoor een totaal van 20 shards. Twee knooppunten wordt daarom 6 shards bevatten, terwijl de andere twee 7 elke houdt. Dit is duidelijk in de CPU-gebruik grafieken in de eerste fase van 20 minuten, knooppunt 0 is minder druk bezet is dan de andere twee. Nadat het herstelproces is voltooid, lijkt sommige overstappen op te treden als knooppunt 2 lijkt het knooppunt meer licht geladen.

    
## <a name="node-failure-with-catastrophic-data-loss-results"></a>Knooppunt mislukt met ernstige gegevensverlies: resultaten

<!-- TODO; reformat this pdf for display inline -->

De resultaten van deze test worden weergegeven in het bestand [ElasticsearchRecoveryScenario2.pdf](https://github.com/mspnp/azure-guidance/blob/master/figures/Elasticsearch/ElasticSearchRecoveryScenario2.pdf). Zoals u met de eerste test, ziet het eerste deel van de grafieken het systeem normaal gedurende ongeveer 20 minuten, op welke punt knooppunt is 0 afgesloten voor 5 minuten. Tijdens dit interval, wordt de gegevens Elasticsearch op dit knooppunt verwijderd, wordt simuleren fatale gegevensverlies, voordat u opnieuw worden gestart. Volledig herstel wordt 12-15 minuten duren voordat de niveaus van de prestaties van de test eerder worden hersteld.

Houd rekening met de volgende punten:

- Tijdens de test zijn geen fouten gerapporteerd. Er zijn geen gegevens is verbroken en alle bewerkingen is voltooid.

- De transactie eenheidstarieven voor alle drie soorten bewerking (bulksgewijs invoegen, samenvoegquery en filterquery) verwijderd en de gemiddelde antwoord tijden is groter, terwijl het knooppunt 0 is offline. Het profiel van de prestaties van de test is op dit moment vergelijkbaar met het eerste scenario. Dit is niet verrassend als tot dat moment weer, de scenario's zijn hetzelfde.

- Gedurende de periode herstel zijn de transactie tarieven en -antwoord tijd teruggezet, hoewel tijdens deze periode er veel meer volatiliteit in de cijfers is. Dit is waarschijnlijk vanwege de extra werk dat de knooppunten in het cluster uitvoeren wilt, worden de gegevens als u wilt herstellen van het ontbrekende shards. Deze extra werk is duidelijk de CPU-gebruik, schijfactiviteit en netwerk activiteit grafieken.

- De grafiek CPU-gebruik voor knooppunten 0 en 1 worden verminderde activiteit weergegeven tijdens de herstelfase dat dit vanwege de verbeterde schijf en activiteit op het netwerk die worden veroorzaakt door het herstelproces is. In het eerste scenario, alleen het knooppunt wordt hersteld tentoongesteld dit probleem, maar in dit scenario lijkt deze waarschijnlijk dat het grootste deel van de ontbrekende gegevens voor knooppunt 0 wordt teruggezet uit knooppunt 1.

- De o-activiteit voor knooppunt 0 is werkelijk verminderd in vergelijking met het eerste scenario. Dit kan zijn vanwege de i/o-efficiëntie van de gegevens voor een hele shard in plaats van de reeks kleinere i/o-aanvragen vereist om over te brengen van een bestaande shard up-to-date te kopiëren.

- Netwerkactiviteiten voor alle drie knooppunten aangegeven lichtflitsen activiteit gegevens wordt verzonden en ontvangen tussen knooppunten. In scenario 1, wordt alleen knooppunt 0 tentoongesteld als veel netwerkactiviteit, maar deze activiteit leek te worden opgelopen voor een langere periode. Dit verschil mogelijk opnieuw vanwege de efficiëntie van de volledige gegevens voor een shard verzenden als een enkele aanvraag in plaats van de reeks kleinere aanvragen ontvangen wanneer een shard herstellen.

## <a name="node-failure-and-restart-with-shard-reallocation-results"></a>Knooppunt is mislukt en opnieuw starten met shard herverdeling: resultaten

<!-- TODO; reformat this pdf for display inline -->

Het bestand [ElasticsearchRecoveryScenario3.pdf](https://github.com/mspnp/azure-guidance/blob/master/figures/Elasticsearch/ElasticSearchRecoveryScenario3.pdf) ziet u de resultaten van deze test. Als het eerste deel van de grafieken weergeven met de eerst te testen, het systeem normaal gedurende ongeveer 20 minuten, op welke punt knooppunt is 0 afgesloten voor 5 minuten. Op dit moment probeert het cluster Elasticsearch de ontbrekende shards opnieuw te maken en de shards opnieuw op de resterende knooppunten. Na 5 minuten knooppunt 0 weer online is gebracht, en nogmaals het cluster heeft de shards opnieuw uit te. Prestaties is hersteld na 12-15 minuten.

Houd rekening met de volgende punten:

- Tijdens de test zijn geen fouten gerapporteerd. Er zijn geen gegevens is verbroken en alle bewerkingen is voltooid.

- De transactie eenheidstarieven voor alle drie soorten bewerking (bulksgewijs invoegen, samenvoegquery en filterquery) genegeerd en wordt de gemiddelde antwoord tijden verhoogd aanzienlijk terwijl het knooppunt 0 is offline vergeleken met de vorige twee tests. Dit is vanwege de activiteit verbeterde cluster opnieuw maken van het ontbrekende shards en het cluster opnieuw te produceren: de verhoogd cijfers voor schijf en netwerk activiteit voor knooppunten 1 en 2 in deze periode.

- Gedurende de periode nadat het knooppunt 0 weer on line, blijven de transactie tarieven en -antwoord tijd vluchtige.

- De CPU-gebruik en schijf activiteit grafieken voor knooppunt 0 toont zeer verminderde eerste actie tijdens de herstelfase. Dit komt doordat nu knooppunt 0 geen gegevens levert. Na een periode van ongeveer 5 minuten, het knooppunt barst in werking < RBC: dit mij hardop snort hebt aangebracht. Ik ben niet binnenkort jarig is met een betere manier dit echter zegt.  >> zoals door de plotselinge stijging netwerk, schijf en processor activiteit. Dit is waarschijnlijk veroorzaakt door het cluster shards verdelen over knooppunten. Knooppunt 0 wordt op normale activiteit weergegeven.
  
## <a name="rolling-updates-results"></a>Updates schuivend: resultaten

<!-- TODO; reformat this pdf for display inline -->

De resultaten van deze test, in het bestand [ElasticsearchRecoveryScenario4.pdf](https://github.com/mspnp/azure-guidance/blob/master/figures/Elasticsearch/ElasticSearchRecoveryScenario4.pdf), weergeven hoe elk knooppunt is offline te gaan en vervolgens teruggebracht omhoog opnieuw achter elkaar Elke knooppunt is afgesloten voor 5 minuten voordat u opnieuw worden gestart, waarna het volgende knooppunt in volgorde is gestopt.

Houd rekening met de volgende punten:

- Terwijl elk knooppunt gaat, wordt de prestaties met doorvoer en reactie tijden redelijk zelfs blijft.

- Schijfactiviteit neemt toe voor elk knooppunt voor een korte tijdnotatie deze opnieuw online is gebracht. Dit is waarschijnlijk vanwege het herstelproces schuivend doorsturen wijzigingen die zijn opgetreden terwijl het knooppunt ingedrukt is.

- Wanneer een knooppunt is verbroken, pieken in netwerkactiviteiten voorkomen in de resterende knooppunten. Pieken ook optreden wanneer een knooppunt opnieuw is opgestart.

- Nadat het laatste knooppunt herhaald wordt, wordt een periode van aanzienlijk volatiliteit ingevoerd. Dit is meestal veroorzaakt door het herstelproces te synchroniseren van wijzigingen op elk knooppunt en zorg ervoor dat alle replica's en hun bijbehorende shards consistent zijn. Op een bepaald, deze hoeveelheid oorzaken opeenvolgende bulksgewijs bewerkingen time-out invoegen en mislukt. De fouten gerapporteerd in elk geval zijn:

```
Failure -- BulkDataInsertTest17(org.apache.jmeter.protocol.java.sampler.JUnitSampler$AnnotatedTestCase): java.lang.AssertionError: failure in bulk execution:
[1]: index [systwo], type [logs], id [AVEg0JwjRKxX_sVoNrte], message [UnavailableShardsException[[systwo][2] Primary shard is not active or isn't assigned to a known node. Timeout: [1m], request: org.elasticsearch.action.bulk.BulkShardRequest@787cc3cd]]

```

Volgende experimenten blijkt dat Kennismaking met een vertraging van een paar minuten tussen elk knooppunt functioneert is verwijderd deze fout, zodat deze was waarschijnlijk die worden veroorzaakt door aantal conflicten tussen het herstelproces te verschillende knooppunten tegelijk herstellen en de bulksgewijs bewerkingen duizenden van nieuwe documenten wilt invoegen.


## <a name="summary"></a>Overzicht

De tests die uitgevoerd aangegeven dat:

- Elasticsearch is uiterst robuuste naar de meest voorkomende manieren kunnen optreden in een cluster is mislukt.

- Elasticsearch kunt snel herstellen als een goed ontworpen cluster onderhevig fatale gegevensverlies op een knooppunt is. Dit kan gebeuren als u Elasticsearch als gegevens wilt opslaan met kortstondige storage configureren en het knooppunt is vervolgens door de service na een herstart. Deze resultaten blijkt dat zelfs in dit geval de risico's van het gebruik van kortstondige opslag zijn waarschijnlijk overtroffen door het nut van prestaties die klasse opslag.

- In de eerste drie scenario's, geen fouten opgetreden in gelijktijdige bulksgewijs invoegen, aggregatie en filter query werkbelasting knooppunt is die u hebt gemaakt offline en hersteld.

- Alleen het laatste scenario aangegeven potentieel gegevensverlies en dit verlies beïnvloed alleen nieuwe gegevens wordt toegevoegd. Het is raadzaam in toepassingen uitvoering van de opname van de gegevens wilt beperken deze waarschijnlijkheid door opnieuw invoegbewerkingen die niet zijn als het type fout gerapporteerd hoogstwaarschijnlijk worden tijdelijke.

- De resultaten van de laatste test ook worden weergegeven of als u gepland onderhoud van de knooppunten in een cluster uitvoert, prestaties profiteren als u enkele minuten tussen de werking van de tabtoets één knooppunt en de volgende toestaat. In een niet-geplande situatie (zoals het datacenter knooppunten hergebruik na het uitvoeren van een update van het besturingssysteem), hebt u minder controle over hoe en wanneer knooppunten zijn uitgeschakeld en opnieuw gestart. Het feit dat ontstaat als Elasticsearch probeert te herstellen van de status van de cluster na opeenvolgende knooppunt bijvoorbeeld kan resulteren in-outs en fouten. 

[De beschikbaarheid van virtuele Machines beheren]: ../articles/virtual-machines/virtual-machines-linux-manage-availability.md
[De geautomatiseerde Elasticsearch tolerantie Tests uitvoeren]: guidance-elasticsearch-running-automated-resilience-tests.md
