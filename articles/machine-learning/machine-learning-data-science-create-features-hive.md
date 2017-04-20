<properties
    pageTitle="Functies voor gegevens maken in een Hadoop-cluster met query's in component | Microsoft Azure"
    description="Voorbeelden van component-query's die functies in de gegevens die zijn opgeslagen in een cluster Azure HDInsight Hadoop genereren."
    services="machine-learning"
    documentationCenter=""
    authors="bradsev"
    manager="jhubbard"
    editor="cgronlun"  />

<tags
    ms.service="machine-learning"
    ms.workload="data-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/19/2016"
    ms.author="hangzh;bradsev" />


#<a name="create-features-for-data-in-an-hadoop-cluster-using-hive-queries"></a>Functies voor gegevens in een Hadoop-cluster met component query's maken

In dit document ziet hoe u functies voor gegevens die zijn opgeslagen in een Azure HDInsight Hadoop-cluster met component query's maken. Ingesloten component door de gebruiker gedefinieerde functies (UDF), u de scripts waarvoor vindt deze component query's gebruiken

De bewerkingen die nodig zijn voor het maken van functies kunnen vergt veel geheugen zijn. De prestaties van query's component meer kritiek in dat geval wordt en kan worden verbeterd door het optimaliseren van bepaalde parameters. Het afstemmen van deze parameters wordt in de sectie definitief besproken.

Voorbeelden van de query's die worden weergegeven zijn specifiek voor de [Tevens Taxi reis gegevens](http://chriswhong.com/open-data/foil_nyc_taxi/) scenario's zijn ook beschikbaar in [Github opslagplaats](https://github.com/Azure/Azure-MachineLearning-DataScience/tree/master/Misc/DataScienceProcess/DataScienceScripts). Deze query's al gegevensschema opgegeven hebt en klaar bent om te worden verzonden om uit te voeren. Klik in de sectie definitief worden ook parameters die gebruikers afstemmen kunnen zodat de prestaties van query's component kan worden verbeterd besproken.

[AZURE.INCLUDE [cap-create-features-data-selector](../../includes/cap-create-features-selector.md)]In dit **menu** koppelingen naar onderwerpen waarin wordt uitgelegd hoe u functies voor gegevens maken in verschillende omgevingen. Deze taak is een stap in het [Team gegevens wetenschap proces (TDSP)](https://azure.microsoft.com/documentation/learning-paths/cortana-analytics-process/).


## <a name="prerequisites"></a>Vereisten voor
In dit artikel wordt ervan uitgegaan dat u hebt:

* Een account Azure opslag gemaakt. Als u instructies nodig hebt, raadpleegt u [een opslag van Azure-account maken](../storage/storage-create-storage-account.md#create-a-storage-account)
* Deze is ingericht een aangepaste Hadoop-cluster met de service HDInsight.  Als u instructies nodig hebt, raadpleegt u [Azure HDInsight Hadoop Clusters aanpassen voor geavanceerde analyses](machine-learning-data-science-customize-hadoop-cluster.md).
* De gegevens is geüpload naar component tabellen in Azure HDInsight Hadoop clusters. Als dit niet heeft, volg dan [maken en laden gegevens aan component tabellen](machine-learning-data-science-move-hive-tables.md) om te uploaden van de gegevens eerst naar component tabellen.
* Externe toegang tot het cluster ingeschakeld. Als u instructies nodig hebt, raadpleegt u [toegang tot het hoofd knooppunt van Hadoop Cluster](machine-learning-data-science-customize-hadoop-cluster.md#headnode).


##<a name="hive-featureengineering"></a>Functie generatie

In deze sectie worden verschillende voorbeelden van de manieren waarin functies kunnen genereren met component query's beschreven. Nadat u aanvullende functies hebt gegenereerd, kunt u deze als kolommen toevoegen aan de bestaande tabel of een nieuwe tabel maken met de aanvullende functies en de primaire sleutel, die vervolgens kan worden verbonden met de oorspronkelijke tabel. Hier volgen de voorbeelden:

1. [Frequentie op basis van functie generatie](#hive-frequencyfeature)
2. [Risico's van bestaan variabelen in binaire indeling](#hive-riskfeature)
3. [Functies extraheren uit Datetime Field](#hive-datefeatures)
4. [Functies extraheren uit tekstveld](#hive-textfeatures)
5. [Afstand tussen GPS-coördinaten berekenen](#hive-gpsdistance)

###<a name="hive-frequencyfeature"></a>Frequentie op basis van functie generatie

Vaak is het handig voor het berekenen van de frequentie van de niveaus van een bestaan variabele of de frequentie van bepaalde combinaties van niveaus van meerdere bestaan variabelen. Gebruikers kunnen het volgende script gebruiken voor het berekenen van deze frequentie:

        select
            a.<column_name1>, a.<column_name2>, a.sub_count/sum(a.sub_count) over () as frequency
        from
        (
            select
                <column_name1>,<column_name2>, count(*) as sub_count
            from <databasename>.<tablename> group by <column_name1>, <column_name2>
        )a
        order by frequency desc;


###<a name="hive-riskfeature"></a>Risico's van bestaan variabelen in binaire indeling

In binaire indeling moeten we niet-numerieke bestaan variabelen converteren naar numerieke functies wanneer de modellen voor alleen die wordt gebruikt numerieke functies. Dit is een numerieke risico van elk niveau van de niet-numerieke vervangen door. In dit gedeelte weergeven we enkele algemene component query's die de waarden van het risico (log kans) van een bestaan variabele berekenen.


        set smooth_param1=1;
        set smooth_param2=20;
        select
            <column_name1>,<column_name2>,
            ln((sum_target+${hiveconf:smooth_param1})/(record_count-sum_target+${hiveconf:smooth_param2}-${hiveconf:smooth_param1})) as risk
        from
            (
            select
                <column_nam1>, <column_name2>, sum(binary_target) as sum_target, sum(1) as record_count
            from
                (
                select
                    <column_name1>, <column_name2>, if(target_column>0,1,0) as binary_target
                from <databasename>.<tablename>
                )a
            group by <column_name1>, <column_name2>
            )b

In dit voorbeeld variabelen `smooth_param1` en `smooth_param2` zijn ingesteld op vloeiend de risico's waarden berekend op basis van de gegevens. Risico's hebben een bereik tussen -Inf en inf-bestand. Een risico's > 0, geeft aan dat de kans dat het doel gelijk aan 1 is groter dan 0,5 is.

Na het risico wordt tabel berekend, wordt gebruikers risico waarden kunnen toewijzen aan een tabel met de tabel risico het samenvoegen van deze. De component gekoppelde query is opgegeven in de vorige sectie.

###<a name="hive-datefeatures"></a>Functies extraheren uit Datetime Fields

Component wordt geleverd met een reeks UDF's voor het verwerken van datetime-velden. In component, de standaardnotatie voor datum-/ is ' jjjj-MM-dd 00:00:00 "('1970-01-01-12:21:32 ' bijvoorbeeld). In deze sectie zien we voorbeelden die de dag van een maand, de maand van een datum/tijd-veld extraheren en andere voorbeelden die een datetime-tekenreeks in een indeling converteren andere dan de standaardnotatie in een datetime-tekenreeks in standaardprogramma opmaken.

        select day(<datetime field>), month(<datetime field>)
        from <databasename>.<tablename>;

Deze query component wordt ervan uitgegaan dat de *& #60; veld datetime >* is in de standaardnotatie voor datum/tijd.

Als een datum/tijd-veld niet in de standaardnotatie is, moet u eerst converteert u het veld datetime naar Unix-tijdstempel en klikt u vervolgens de Unix-tijdstempel omzetten in een datetime-tekenreeks die in de standaardnotatie. Wanneer de datum/tijd is in de indeling, kunnen gebruikers het ingesloten datetime UDF's worden geëxtraheerd functies toepassen.

        select from_unixtime(unix_timestamp(<datetime field>,'<pattern of the datetime field>'))
        from <databasename>.<tablename>;

In deze query als de *& #60; veld datetime >* heeft patroon zoals *26-03-2015 12:04:39*, de *' & #60; patroon van het veld datetime >'* moet `'MM/dd/yyyy HH:mm:ss'`. Als u wilt testen, kunnen gebruikers uitvoeren

        select from_unixtime(unix_timestamp('05/15/2015 09:32:10','MM/dd/yyyy HH:mm:ss'))
        from hivesampletable limit 1;

De *hivesampletable* in deze query is standaard geïnstalleerd op alle Azure HDInsight Hadoop clusters al dan niet standaard wanneer de clusters is ingericht.


###<a name="hive-textfeatures"></a>Functies extraheren uit tekstvelden

Wanneer de component tabel een tekstveld met een tekenreeks met woorden die worden gescheiden door spaties bevat, haalt de volgende query de lengte van de tekenreeks en het aantal woorden in de tekenreeks.

        select length(<text field>) as str_len, size(split(<text field>,' ')) as word_num
        from <databasename>.<tablename>;

###<a name="hive-gpsdistance"></a>Afstand tussen sets met GPS-coördinaten berekenen

De query die is opgegeven in deze sectie kan rechtstreeks worden toegepast op de tevens Taxi reis gegevens. Het doel van deze query is voor het toepassen van een ingesloten wiskundige functies in component te genereren van functies.

De velden die worden gebruikt in deze query zijn de coördinaten GPS ophalen en dropoff locaties, met de naam *ophalen\_lengtegegevens*, *ophalen\_breedtegraad*, *dropoff\_lengtegegevens*, en *dropoff\_breedtegraad*. De query's die de directe afstand tussen de coördinaten ophalen en dropoff berekenen zijn:

        set R=3959;
        set pi=radians(180);
        select pickup_longitude, pickup_latitude, dropoff_longitude, dropoff_latitude,
            ${hiveconf:R}*2*2*atan((1-sqrt(1-pow(sin((dropoff_latitude-pickup_latitude)
            *${hiveconf:pi}/180/2),2)-cos(pickup_latitude*${hiveconf:pi}/180)
            *cos(dropoff_latitude*${hiveconf:pi}/180)*pow(sin((dropoff_longitude-pickup_longitude)*${hiveconf:pi}/180/2),2)))
            /sqrt(pow(sin((dropoff_latitude-pickup_latitude)*${hiveconf:pi}/180/2),2)
            +cos(pickup_latitude*${hiveconf:pi}/180)*cos(dropoff_latitude*${hiveconf:pi}/180)*
            pow(sin((dropoff_longitude-pickup_longitude)*${hiveconf:pi}/180/2),2))) as direct_distance
        from nyctaxi.trip
        where pickup_longitude between -90 and 0
        and pickup_latitude between 30 and 90
        and dropoff_longitude between -90 and 0
        and dropoff_latitude between 30 and 90
        limit 10;

De wiskundige vergelijkingen die de afstand tussen twee GPS-coördinaten berekenen vindt u op de site <a href="http://www.movable-type.co.uk/scripts/latlong.html" target="_blank">MovableType Scripts</a> geschreven door Peter Lapisu. In zijn Javascript, de functie `toRad()` is alleen *lat_or_lon*pi/180*, waarin graden naar radialen converteert. Hier *lat_or_lon * is de breedtegraad of lengtegraad. Aangezien component geen de functie biedt `atan2`, maar biedt de functie `atan`, de `atan2` functie wordt geïmplementeerd door `atan` functie in de bovenstaande component query met de definitie die beschikbaar zijn in <a href="http://en.wikipedia.org/wiki/Atan2" target="_blank">Wikipedia</a>.

![Werkruimte maken](./media/machine-learning-data-science-create-features-hive/atan2new.png)

Een volledige lijst van ingesloten UDF's u in de sectie **Ingebouwde functies** in de <a href="https://cwiki.apache.org/confluence/display/Hive/LanguageManual+UDF#LanguageManualUDF-MathematicalFunctions" target="_blank">Apache component wiki vindt</a>-component).  

## <a name="tuning"></a>Geavanceerde onderwerpen: verfijnen component Parameters Query snelheid verbeteren

De standaardinstellingen voor de parameter van component cluster is mogelijk niet geschikt is voor de component query's en de gegevens die de query's worden verwerkt. In dit gedeelte bespreken we enkele parameters die gebruikers kunnen afstemmen de prestaties van component query's verbeteren. Gebruikers moeten de parameter optimaliseren van query's voordat de query's van gegevensverwerking toevoegen.

1. **Java opslagruimte ruimte**: voor query's met betrekking tot grote gegevenssets deelneemt, of lange records processing, **bijna geen ruimte meer opslagruimte** is een van de algemene fout. Dit kan worden geconfigureerd door in te stellen van parameters *mapreduce.map.java.opts* en *mapreduce.task.io.sort.mb* naar de gewenste waarden. Hier volgt een voorbeeld:

        set mapreduce.map.java.opts=-Xmx4096m;
        set mapreduce.task.io.sort.mb=-Xmx1024m;


    Deze parameter 4GB geheugen op Java opslagruimte ruimte toewijst, en ook maakt sorteren efficiënter door het toewijzen van meer geheugen voor deze. Dit is een goed idee om af te spelen met deze toewijzingen als er een taak is mislukt fouten met betrekking tot opslagruimte ruimte.

2. **DFS blokkeren grootte** : deze parameter stelt het kleinste element van de gegevens die het bestandssysteem worden opgeslagen. Als u bijvoorbeeld als u de grootte van de blok DFS 128MB, klikt u vervolgens alle gegevens van de grootte kleiner is dan en tot 128MB opgeslagen in één blok, terwijl de gegevens die groter is dan 128MB extra blokken is toegewezen. Het formaat van een zeer klein blok treedt op grote algemene in Hadoop sinds het knooppunt naam heeft voor veel meer aanvragen voor het zoeken van de relevante blokkering die betrekking hebben op het bestand. Een aanbevolen instelling als betrekking tot gigabytes (of groter) gegevens zijn:

        set dfs.block.size=128m;

3. **Optimaliseren deelnemen aan de bewerking in component** : terwijl join-bewerkingen in het kader van de map/verkleinen meestal in de verkleinen fase soms plaatsvinden enorm winst kunnen worden bereikt door het plannen van joins in de map fase (ook wel 'mapjoins' genoemd). Als u wilt leiden component hiervoor zo veel mogelijk, kunt we instellen:

        set hive.auto.convert.join=true;

4. **Het aantal mappers aan component precisie** : terwijl Hadoop kan de gebruiker voor het instellen van het aantal verkleiningstoestellen, het aantal mappers is meestal niet worden ingesteld door de gebruiker. Een slag waarmee dat sommige mate van besturingselement op dit nummer is de Hadoop-variabelen, *mapred.min.split.size* en *mapred.max.split.size* als de grootte van elke taak map kiezen wordt bepaald door:

        num_maps = max(mapred.min.split.size, min(mapred.max.split.size, dfs.block.size))

    De standaardwaarde van *mapred.min.split.size* is meestal 0, die van *mapred.max.split.size* **Long.MAX** is en die van *dfs.block.size* 64 MB. Als we kunnen bekijken, gegeven de gegevensgrootte, deze parameters door 'instellingen' optimaliseren ze kunnen wij om het aantal mappers gebruikt af te stellen.

5. Hieronder worden enkele andere meer **Geavanceerde opties** voor het optimaliseren van component prestaties. Deze toestaan voor het configureren van de toegewezen als u wilt toewijzen en taken te verkleinen geheugen en is handig in het afstemmen van de prestaties. Houd er rekening mee dat het *mapreduce.reduce.memory.mb* mag niet groter is dan de grootte van geheugen van elke werknemer knooppunt in het Hadoop-cluster.

        set mapreduce.map.memory.mb = 2048;
        set mapreduce.reduce.memory.mb=6144;
        set mapreduce.reduce.java.opts=-Xmx8192m;
        set mapred.reduce.tasks=128;
        set mapred.tasktracker.reduce.tasks.maximum=128;
