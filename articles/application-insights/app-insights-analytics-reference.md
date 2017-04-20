<properties 
    pageTitle="Verwijzing in Analytics in toepassing inzichten | Microsoft Azure" 
    description="Overzicht van de instructies in analyses, de krachtige zoekfunctie van toepassing inzichten. " 
    services="application-insights" 
    documentationCenter=""
    authors="alancameronwills" 
    manager="douge"/>

<tags 
    ms.service="application-insights" 
    ms.workload="tbd" 
    ms.tgt_pltfrm="ibiza" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="10/27/2016" 
    ms.author="awills"/>

# <a name="reference-for-analytics"></a>Overzicht van Analytics

[Analytics](app-insights-analytics.md) is de krachtige zoekfunctie van [Toepassing inzichten](app-insights-overview.md). Deze pagina's worden de querytaal Analytics beschreven.

> [AZURE.NOTE] [Uitproberen analyses van onze gesimuleerd gegevens](https://analytics.applicationinsights.io/demo) als uw app is niet gegevens inzicht krijgen in toepassing nog verzendt.

## <a name="index"></a>Index


**Laat** [laat](#let-clause)


**Query's en operatoren** [tellen](#count-operator)  |  [evalueren](#evaluate-operator) | [uitbreiden](#extend-operator) | [join](#join-operator) | [limiet](#limit-operator) | [mvexpand](#mvexpand-operator) | [parseren](#parse-operator) | [project](#project-operator) | [project opslag](#project-away-operator) | [bereik](#range-operator) | [verkleinen](#reduce-operator) | [richtlijn weergegeven](#render-directive) | [beperken component](#restrict-clause) | [sorteren](#sort-operator) | [samenvatten](#summarize-operator) | [duren](#take-operator) | [boven](#top-operator) | [boven geneste](#top-nested-operator) | [union](#union-operator) | [waar](#where-operator) | [waar in](#where-in-operator)

**Aggregaties** [een](#any)  |  [argmax](#argmax) | [argmin](#argmin) | [avg](#avg) | [buildschema](#buildschema) | [tellen](#count) | [aantal.Als](#countif) | [DBAANTAL](#dcount) | [dcountif](#dcountif) | [makelist](#makelist) | [makeset](#makeset) | [max](#max) | [min](#min) | [percentiel](#percentile) | [percentielen](#percentiles) | [percentilesw](#percentilesw) | [percentilew](#percentilew) | [stdev](#stdev) | [som](#sum) | [variantie](#variance)

**Hoeken** [Booleaanse letterlijke waarden](#boolean-literals)  |  [Booleaans operatoren](#boolean-operators) | [webcasts](#casts) | [scalaire vergelijkingen](#scalar-comparisons) | [gettype](#gettype) | [hash](#hash) | [iff](#iff) | [isnotnull](#isnotnull) | [isnull](#isnull) | [notnull](#notnull) | [toscalar](#toscalar)

**Getallen** [Rekenkundige operators](#arithmetic-operators)  |  [Numerieke letterlijke waarden](#numeric-literals) | [abs](#abs) | [opslaglocatie](#bin) | [exp](#exp) | [afronden.beneden](#floor) | [gamma](#gamma) | [log](#log) | [ASELECT](#rand) | [WORTEL](#sqrt) | [todouble](#todouble) | [toint](#toint) | [tolong](#tolong)

**Datum en tijd** [Datum en tijd expressies](#date-and-time-expressions)  |  [Datum en letterlijke tijdwaarden](#date-and-time-literals) | [geleden](#ago) | [datepart](#datepart) | [dayofmonth](#dayofmonth) | [dag van de week](#dayofweek) | [dayofyear](#dayofyear) | [endofday](#endofday) | [endofmonth](#endofmonth) | [endofweek](#endofweek) | [endofyear](#endofyear) | [getmonth](#getmonth) | [getyear](#getyear) | [nu](#now) | [startofday](#startofday) | [startofmonth](#startofmonth) | [startofweek](#startofweek) | [startofyear](#startofyear) | [todatetime](#todatetime) | [totimespan](#totimespan) | [weekofyear](#weekofyear)

**Tekenreeks** [GUID 's](#guids)  |  [Letterlijke afgeschermd](#obfuscated-string-literals) | [Letterlijke](#string-literals) | [tekenreeksvergelijkingen](#string-comparisons) | [countof](#countof) | [extraheren](#extract) | [isempty](#isempty) | [isnotempty](#isnotempty) | [notempty](#notempty)| [parseurl](#parseurl) | [vervangen](#replace) | [splitsen](#split) | [strcat](#strcat) | [strlen](#strlen) | [subtekenreeks](#substring) | [tolower](#tolower) | [toupper](#toupper)

**Matrices, objecten en dynamisch** [Matrix en object letterlijke waarden](#array-and-object-literals)  |  [Dynamische objectfuncties](#dynamic-object-functions) | [dynamische objecten in laten componenten](#dynamic-objects-in-let-clauses) | [JSON pad expressies](#json-path-expressions) | [namen](#names) | [arraylength](#arraylength) | [extractjson](#extractjson) | [parsejson](#parsejson) | [bereik](#range) | [todynamic](#todynamic) | [treepath](#treepath)



## <a name="let"></a>Laat

### <a name="let-clause"></a>Laat component

**Tabelvorm laten - naamgeving van een tabel**

    let recentReqs = requests | where timestamp > ago(3d); 
    recentReqs | count

**Scalaire laten - naam van een**

    let interval = 3d; 
    requests | where timestamp > ago(interval)

**Lambda laten - naamgeving van een functie**

    let Recent = 
       (interval:timespan) { requests | where timestamp > ago(interval) };
    Recent(3h) | count

    let us_date = (t:datetime) { strcat(getmonth(t),'/',dayofmonth(t),'/',getyear(t)) }; 
    requests | summarize count() by bin(timestamp, 1d) | project count_, day=us_date(timestamp)

Een laat-component wordt de [naam](#names) van een gebonden aan een tabelvormige resultaat, een scalaire waarde of een functie. De component is een voorvoegsel aan een query en het bereik van de binding die query is. (Laat niet zelf een formule naar de naam van onderdelen die u later in uw sessie gebruiken.)

**Syntaxis**

    let name = scalar_constant_expression ; query

    let name = query ; query

    let name = (parameterName : type [, ...]) { plain_query }; query

    let name = (parameterName : type [, ...]) { scalar_expression }; query

* *type:* `bool`, `int`, `long`, `double`, `string`, `timespan`, `datetime`, `guid`,[`dynamic`](#dynamic-type)
* *plain_query:* Een query niet worden voorafgegaan door een laat-component.

**Voorbeelden**

    let rows = (n:long) { range steps from 1 to n step 1 };
    rows(10) | ...


Selfservice deelnemen:

    let Recent = events | where timestamp > ago(7d);
    Recent | where name contains "session_started" 
  	| project start = timestamp, session_id
  	| join (Recent 
        | where name contains "session_ended" 
        | project stop = timestamp, session_id)
      on session_id
  	| extend duration = stop - start 


## <a name="queries-and-operators"></a>Query's en operatoren

Een query over uw telemetrielogboek bestaat uit een verwijzing naar een bron-stream, gevolgd door een pijplijn met filters. Bijvoorbeeld:


```AIQL
requests // The request table starts this pipeline.
| where client_City == "London" // filter the records
   and timestamp > ago(3d)
| count 
```
    
Alle filters voorafgegaan door het sluisteken `|` is een exemplaar van een *operator*, met enkele parameters. De invoer voor de operator is de tabel die het resultaat van de voorgaande pijplijn. In de meeste gevallen zijn parameters [scalaire expressies](#scalars) over de kolommen van de invoer. In sommige gevallen, de parameters zijn de namen van de invoer kolommen en in sommige gevallen, is de parameter een tweede tabel. Het resultaat van een query is altijd een tabel, zelfs als deze slechts één kolom en één rij heeft.

Query's mogelijk één regeleinden bevatten, maar door een lege regel worden beëindigd. Ze kunnen opmerkingen tussen bevatten `//` en het einde van de regel.

Een query kan worden voorafgegaan door een of meer [componenten laten](#let-clause), waarin hoeken, tabellen of functies die kunnen worden gebruikt in de query definiëren.

```AIQL

    let interval = 3d ;
    let city = "London" ;
    let req = (city:string) {
      requests
      | where client_City == city and timestamp > ago(interval) };
    req(city) | count
```

> `T`wordt gebruikt in de onderstaande voorbeelden van query om de vorige tabel voor de verkooppijplijn of bron aan te geven.
> 

### <a name="count-operator"></a>aantal operator

De `count` operator geeft als resultaat het aantal records (rijen) in de invoer Recordset.

**Syntaxis**

    T | count

**Argumenten**

* *T*: de tabelgegevens waarvan de records die moeten worden geteld.

**Geeft als resultaat**

Deze functie retourneert een tabel met een enkele record en de kolom van het type `long`. De waarde van de enige cel is het aantal records in *T*. 

**Voorbeeld**

```AIQL
requests | count
```

### <a name="evaluate-operator"></a>operator evalueren

`evaluate`is een methode extensie waarmee gespecialiseerde algoritmen moet worden toegevoegd aan de query's.

`evaluate`moet de laatste operator in de query-pijplijn (behalve voor een mogelijk `render`). Deze worden niet weergegeven in de hoofdtekst van een functie.

[evalueren autocluster](#evaluate-autocluster) | [evalueren mandje](#evaluate-basket) | [evalueren diffpatterns](#evaluate-diffpatterns) | [extractcolumns evalueren](#evaluate-extractcolumns)

#### <a name="evaluate-autocluster"></a>autocluster evalueren

     T | evaluate autocluster()

AutoCluster vindt u algemene patronen van afzonderlijke kenmerken (dimensies) in de gegevens en de resultaten van de oorspronkelijke query (of het nu gaat 100 of 100 k rijen) naar een klein aantal patronen wordt kleiner. AutoCluster is ontwikkeld om u te helpen met het analyseren van mislukte pogingen (bijvoorbeeld uitzonderingen, loopt), maar mogelijk op een gefilterde gegevensverzameling kunnen werken. 

**Syntaxis**

    T | evaluate autocluster( arguments )

**Geeft als resultaat**

AutoCluster retourneert een (meestal kleine) patronen die gedeelten van de gegevens met gedeelde algemene waarden over meerdere afzonderlijke kenmerken vastleggen. Elke patroon wordt voorgesteld door een rij in de resultaten. 

De eerste twee kolommen zijn het aantal en het percentage van rijen uit de oorspronkelijke query die door het patroon zijn geregistreerd. De overige kolommen van de oorspronkelijke query en hun waarde is een bepaalde waarde uit de kolom of ' *' waarbij de waarden van variabelen. 

Houd er rekening mee dat de patronen niet gescheiden zijn: ze kunnen overlappende en meestal de oorspronkelijke rijen niet besproken. Enkele rijen mag niet vallen onder een patroon.

**Tips**

* Gebruik `where` en `project` in de invoer pijp verkleinen van de gegevens naar een gewoon wat u geïnteresseerd bent.
* Wanneer u een interessante rij hebt gevonden, wilt u mogelijk inzoomen op verder door toe te voegen aan de specifieke waarden wilt opzoeken uw `where` filter.

**Argumenten (optioneel)**

* `output=all | values | minimal` 

    De opmaak van de resultaten. De kolommen tellen en percentage wordt altijd weergegeven in de zoekresultaten. 

 * `all`-alle kolommen van de invoer worden uitgevoerd
 * `values`-gefilterd op kolommen met slechts ' *' in de zoekresultaten
 * `minimal`-kolommen die identiek zijn voor alle rijen in de oorspronkelijke query ook gefilterd. 


* `min_percent=`*dubbele* (standaard: 1)

    De minimale percentage licenties voor de gegenereerde rijen.

    Voorbeeld:`T | evaluate autocluster("min_percent=5.5")`


* `num_seeds=`*int* (standaard: 25) 

    Het aantal zaden bepaalt het aantal punten van de eerste lokale zoekopdracht van het algoritme gebruiken. In sommige gevallen, afhankelijk van de structuur van de gegevens, verhoogt het aantal zaden het getal (of kwaliteit) van de resultaten tot en met verbeterde zoeken ruimte aan langzamer query verhouding. Het argument num_seeds heeft zodat resultaten in beide richtingen zodat deze onder 5 verlagen verwaarloosbaar prestatieverbeteringen krijgt en groeiende boven 50 zelden meer patronen genereert.

    Voorbeeld:`T | evaluate autocluster("num_seeds=50")`


* `size_weight=`*0 < dubbele < 1*+ (standaard: 0,5)

    Hebt u enkele controle over de verhouding tussen Algemeen (hoge dekking) en informatieve (veel gedeelde waarden). Toeneemt size_weight meestal minder patronen en elke patroon krijgt een grotere percentage bedekt. Meestal size_weight verlagen levert specifiekere patronen met meer gedeelde waarden en kleinere percentage waarborgen. De achter de schermen formule een gewogen meetkundig gemiddelde tussen de genormaliseerde algemene score en informatieve score met size_weight en 1-size_weight als de lijndikten is. 

    Voorbeeld:`T | evaluate autocluster("size_weight=0.8")`


* `weight_column=`*kolomnaam*

    Acht elke rij in het vak Invoerbereik op basis van het opgegeven gewicht (al dan niet standaard heeft elke rij een dikte van de '1'), algemene gebruik van een kolom gewicht is om te houden account steekproeven of bucketing/aggregatie van de gegevens die al is ingesloten in elke rij.

    Voorbeeld:`T | evaluate autocluster("weight_column=sample_Count")` 



#### <a name="evaluate-basket"></a>mandje evalueren

     T | evaluate basket()

Mandje vindt u alle veelgebruikte patronen van afzonderlijke kenmerken (dimensies) in de gegevens en retourneert alle veelgebruikte patronen die de drempelwaarde voor de frequentie in de oorspronkelijke query doorgegeven. Mandje gegarandeerd alle veelgebruikte patronen zoeken in de gegevens, maar is niet gegarandeerd polynomiale runtime-hebt. De runtime van de query is lineaire in het aantal rijen, maar in sommige gevallen mogelijk exponentiële in het aantal kolommen (dimensies). Mandje is gebaseerd op de algoritme van de Apriori oorspronkelijk is ontwikkeld voor mandje analyse datamining. 

**Geeft als resultaat**

Alle patronen weergegeven in meer dan een opgegeven breuk (standaard 0,05) van de gebeurtenissen.

**Argumenten (optioneel)**


* `threshold=`*0.015 < dubbele < 1* (standaard: 0,05) 

    Hiermee stelt u de minimale verhouding tussen de rijen beschouwd veelgebruikte (patronen met kleinere verhouding niet wordt geretourneerd).

    Voorbeeld:`T | evaluate basket("threshold=0.02")`


* `weight_column=`*kolomnaam*

    Acht elke rij in het vak Invoerbereik op basis van het opgegeven gewicht (al dan niet standaard heeft elke rij een dikte van de '1'), algemene gebruik van een kolom gewicht is om te houden account steekproeven of bucketing/aggregatie van de gegevens die al is ingesloten in elke rij.

    Voorbeeld: T | basket("weight_column=sample_Count") evalueren


* `max_dims=`*1 < int* (standaard: 5)

    Hiermee stelt u het maximale aantal niet-gerelateerde dimensies per mandje, beperkt al dan niet standaard naar het verkleinen van de query-runtime.


* `output=minimize` | `all` 

    De opmaak van de resultaten. De kolommen tellen en percentage wordt altijd weergegeven in de zoekresultaten.

 * `minimize`-gefilterd op kolommen met slechts ' *' in de zoekresultaten.
 * `all`-alle kolommen van de invoer worden uitgevoerd.




#### <a name="evaluate-diffpatterns"></a>diffpatterns evalueren

     requests | evaluate diffpatterns("split=success")

Diffpatterns verschilt van twee gegevensverzamelingen van dezelfde structuur en vindt u patronen van afzonderlijke kenmerken (dimensies) die de beschrijving van de verschillen tussen de twee gegevensverzamelingen. Diffpatterns is ontwikkeld om u te helpen met het analyseren van mislukte pogingen (bijvoorbeeld door vergelijken fouten op fouten in een bepaald tijdsbestek), maar mogelijk verschillen tussen een twee gegevensverzamelingen van dezelfde structuur kunt vinden. 

**Syntaxis**

`T | evaluate diffpatterns("split=`*BinaryColumn*`" [, arguments] )`

**Geeft als resultaat**

Diffpatterns retourneert een (meestal kleine) patronen die verschillende gedeelten van de gegevens in de twee paren vastleggen (dat wil zeggen een patroon vastleggen van een grote percentage van de rijen in de eerste gegevensverzameling en laag percentage van de rijen in de tweede set). Elke patroon wordt voorgesteld door een rij in de resultaten.

De eerste vier kolommen zijn het aantal en het percentage van rijen uit de oorspronkelijke query die door het patroon in elke set zijn geregistreerd, de vijfde kolom is het verschil (in percentagepunten absolute) tussen de twee paren. De overige kolommen van de oorspronkelijke query en hun waarde is een bepaalde waarde uit de kolom of * waarbij de waarden van variabelen. 

Houd er rekening mee dat de patronen niet verschillen onderling gebruiken: ze kunnen overlappende en meestal de oorspronkelijke rijen niet besproken. Enkele rijen mag niet vallen onder een patroon.

**Tips**

* Gebruik waar en project op de invoer pijp verkleinen van de gegevens naar een gewoon wat u geïnteresseerd bent.

* Wanneer u een interessante rij hebt gevonden, wilt u mogelijk inzoomen op verder door de specifieke waarden toe te voegen aan uw waar filteren.

**Argumenten**

* `split=`*kolomnaam* (vereist)

    De kolom moet exact twee waarden bevatten. Klik zo nodig, zoals een kolom maken:

    `requests | extend fault = toint(resultCode) >= 500` <br/>
    `| evaluate diffpatterns("split=fault")`

* `target=`*tekenreeks*

    Hiermee wordt aan het algoritme alleen zoekt patronen die hoger percentage in de doelgegevensset hebben, het doel moet een van de twee waarden van de kolommen splitsen.

    `requests | evaluate diffpatterns("split=success", "target=false")`

* `threshold=`*0.015 < dubbele < 1* (standaard: 0,05) 

    Hiermee stelt u de minimale patroon (snede) verschil tussen de twee paren.

    `requests | evaluate diffpatterns("split=success", "threshold=0.04")`

* `output=minimize | all`

    De opmaak van de resultaten. De kolommen tellen en percentage wordt altijd weergegeven in de zoekresultaten. 

 * `minimize`-gefilterd op kolommen met slechts ' *' in de zoekresultaten
 * `all`-alle kolommen van de invoer worden uitgevoerd

* `weight_column=`*kolomnaam*

    Acht elke rij in het vak Invoerbereik op basis van het opgegeven gewicht (al dan niet standaard elke rij heeft een dikte van de '1'). Er wordt een vaak gebruikt van een kolom gewicht rekening account steekproeven of bucketing/aggregatie van de gegevens die al is ingesloten in elke rij.

    `requests | evaluate autocluster("weight_column=itemCount")`






#### <a name="evaluate-extractcolumns"></a>extractcolumns evalueren

     exceptions | take 1000 | evaluate extractcolumns("details=json") 

Extractcolumns wordt gebruikt om een tabel met meerdere eenvoudige kolommen die dynamisch opgehaalde afmelden bij (semi) gestructureerde kolommen op basis van hun type zijn verrijken. Dit ondersteunt momenteel json kolommen alleen beide dynamisch en serialisatie van jsons tekenreeks.


* `max_columns=`*int* (standaard: 10) 

    Het aantal nieuwe toegevoegde kolommen dynamische is en kan het zijn zeer grote (werkelijk dit is het aantal unieke sleutels in alle json-records) zodat we de gegevens moet beperkt. De nieuwe kolommen worden gesorteerd in aflopende volgorde op basis van hun frequentie en snel aan de max_columns worden toegevoegd aan de tabel.

    `T | evaluate extractcolumns("json_column_name=json", "max_columns=30")`


* `min_percent=`*dubbele* (standaard: 10.0) 

    Een andere manier om nieuwe kolommen beperken door kolommen waarvan frequentie lager dan min_percent is negeren.

    `T | evaluate extractcolumns("json_column_name=json", "min_percent=60")`


* `add_prefix=`*BOOL* (standaard: waar) 

    Als de waarde true wordt de naam van het complexe kolom worden toegevoegd als een voorvoegsel voor de opgehaalde kolommen namen.


* `prefix_delimiter=`*tekenreeks* (standaard: "_") 

    Als add_prefix = waar deze parameter definieert het scheidingsteken dat wordt gebruikt voor het samenvoegen van de namen van de nieuwe kolommen.

    `T | evaluate extractcolumns("json_column_name=json",` <br/>
    `"add_prefix=true", "prefix_delimiter=@")`


* `keep_original=`*BOOL* (standaard: ONWAAR) 

    Als de waarde true worden, de oorspronkelijke (json)-kolommen in de uitvoertabel bewaard.


* `output=query | table` 

    De opmaak van de resultaten. 

 * `table`-De uitvoer is dezelfde tabel als ontvangen minus het opgegeven invoer kolommen plus de nieuwe kolommen die zijn opgehaald uit de invoer kolommen.
 * `query`-De uitvoer is een tekenreeks die de query die u aanbrengen wilt in het resultaat als tabel. 




### <a name="extend-operator"></a>operator uitbreiden

     T | extend duration = stopTime - startTime

Een of meer berekende kolommen toevoegen aan een tabel. 


**Syntaxis**

    T | extend ColumnName = Expression [, ...]

**Argumenten**

* *T:* De invoertabel.
* *ColumnName:* De naam van een toe te voegen kolommen. [Namen](#names) zijn hoofdlettergevoelig en kunnen bevatten alfabetisch, numeriek of '_' tekens. Gebruik `['...']` of `["..."]` aan de offerte trefwoorden of namen met andere tekens.
* *Expressie:* Een berekening op de bestaande kolommen.

**Geeft als resultaat**

Een kopie van de invoertabel, met de opgegeven extra kolommen.

**Tips**

* Gebruik [`project`](#project-operator) in plaats daarvan desgewenst kunt u ook neer te zetten of de naam van een aantal kolommen wijzigen.
* Gebruik geen `extend` gewoon om een kortere naam gebruiken in een lange expressie. `...| extend x = anonymous_user_id_from_client | ... func(x) ...` 

    De systeemeigen kolommen van de tabel zijn geïndexeerd; de naam van uw nieuwe Hiermee definieert u een extra kolom die niet is geïndexeerd, zodat de query waarschijnlijk is trager.

**Voorbeeld**

```AIQL
traces
| extend
    Age = now() - timestamp
```


### <a name="join-operator"></a>join-operator

    Table1 | join (Table2) on CommonColumn

De rijen van twee tabellen samengevoegd door overeenkomende waarden van de opgegeven kolom.


**Syntaxis**

    Table1 | join [kind=Kind] (Table2) on CommonColumn [, ...]

**Argumenten**

* *Tabel1* - 'links kant' van de join definieert.
* *Tabel2* - 'rechts' van de join definieert. Een geneste query-expressie die een tabel wordt kan zijn.
* *CommonColumn* - een kolom met dezelfde naam in de twee tabellen.
* *Type* - geeft aan hoe rijen uit de twee tabellen moeten worden overeengestemd.

**Geeft als resultaat**

Een tabel met:

* Een kolom voor elke kolom in elk van de twee tabellen, inclusief de overeenkomende sleutels. De kolommen van de rechterkant automatisch gewijzigd als er op de naam is strijdig.
* Een rij voor elke overeenkomst tussen de invoer tabellen. Een overeenkomst is een van de ene tabel met dezelfde waarde voor alle geselecteerde rij de `on` velden als een rij in een andere tabel. 

* `Kind`niet-opgegeven

    Slechts één rij vanaf de linkerkant bij elkaar worden gezocht voor elke waarde van de `on` sleutel. De uitvoer bevat een rij voor elke overeenstemt met deze rij met rijen aan de rechterkant.

* `Kind=inner`
 
     Er is een rij in de uitvoer voor elke combinatie van overeenkomende rijen uit de linker- en.

* `kind=leftouter`(or `kind=rightouter` or `kind=fullouter`)

     Naast de binnenste overeenkomsten is het een rij voor elke rij op de links (en/of rechts), zelfs als er geen overeenkomst. In dat geval bevatten de niet-gerelateerde records uitvoer cellen null-waarden.

* `kind=leftanti`

     Geeft alle records vanaf de linkerkant die geen overeenkomsten vanaf de rechterkant. De resultatentabel bevat alleen de kolommen vanaf de linkerkant. 
 
Als er meerdere rijen met dezelfde waarde voor die velden, krijgt u rijen voor alle combinaties.

**Tips**

Voor de beste prestaties:

* Gebruik `where` en `project` verkleinen van het aantal rijen en kolommen in de invoer tabellen, voordat de `join`. 
* Als u één tabel altijd kleiner is dan de andere, gebruikt u deze als (piped) links van de join definieert.
* De kolommen voor de join-overeenkomst moeten dezelfde naam hebben. Gebruik de operator project zo nodig om de naam van een kolom in een van de tabellen te.

**Voorbeeld**

Activiteiten ophalen uit een logboek in welke sommige vermeldingen markeren het begin en einde van een activiteit worden verlengd. 

```AIQL
    let Events = MyLogTable | where type=="Event" ;
    Events
  	| where Name == "Start"
  	| project Name, City, ActivityId, StartTime=timestamp
  	| join (Events
           | where Name == "Stop"
           | project StopTime=timestamp, ActivityId)
        on ActivityId
  	| project City, ActivityId, StartTime, StopTime, Duration, StopTime, StartTime

```


### <a name="limit-operator"></a>limiet operator

     T | limit 5

Ze keert terug naar het opgegeven aantal rijen uit de invoertabel. Er is geen garantie welke records worden geretourneerd. (Gebruik om bepaalde records te retourneren [`top`](#top-operator).)

**Alias**`take`

**Syntaxis**

    T | limit NumberOfRows


**Tips**

`Take`is een eenvoudige en efficiënte manier om een voorbeeld van uw resultaten te zien wanneer u interactief werkt. Let erop dat het geen garantie naar bepaalde rijen produceren, of als u wilt deze in een bepaalde volgorde kunnen produceren.

Er is een impliciete limiet van het aantal rijen die worden geretourneerd naar de mailclient gebruikt, zelfs als u niet gebruikt `take`. Als u wilt deze limiet til, gebruikt u de `notruncation` client verzoek optie.



### <a name="mvexpand-operator"></a>mvexpand operator

    T | mvexpand listColumn 

Zodanig dat elk item een aparte rij heeft, wordt een lijst met een (JSON) dynamisch getypt cel uitgebreid. Alle andere cellen in een uitgevouwen rij worden gedupliceerd. 

(Zie ook [`summarize makelist`](#summarize-operator) waarin de tegenovergestelde werkt.)

**Voorbeeld**

Stel dat de invoertabel is:

|A:int|B:String|D:Dynamic|
|---|---|---|
|1|'Hallo'|{"sleutel": "value"}|
|2|"wereld"|[0,1, "k", "w"]|

    mvexpand D

Resultaat luidt als volgt:

|A:int|B:String|D:Dynamic|
|---|---|---|
|1|'Hallo'|{"sleutel": "value"}|
|2|"wereld"|0|
|2|"wereld"|1|
|2|"wereld"|"k"|
|2|"wereld"|"w"|


**Syntaxis**

    T | mvexpand  [bagexpansion=(bag | array)] ColumnName [limit Rowlimit]

    T | mvexpand  [bagexpansion=(bag | array)] [Name =] ArrayExpression [to typeof(Typename)] [limit Rowlimit]

**Argumenten**

* *ColumnName:* In het resultaat, matrices in de benoemde kolom uitgevouwen naar meerdere rijen. 
* *ArrayExpression:* Een expressie die een matrix zodat. Als dit formulier wordt gebruikt, wordt een nieuwe kolom wordt toegevoegd en wordt het bestaande bestand blijft behouden.
* *Naam:* Een naam voor de nieuwe kolom.
* *Typename:* Zet de uitgevouwen expressie naar een bepaald type
* *Gedurende:* Het maximum aantal rijen die zijn gegenereerd op basis van elke rij van de oorspronkelijke. De standaardinstelling is 128.

**Geeft als resultaat**

Meerdere rijen voor elk van de waarden in een matrix in de benoemde kolom of in de matrix-expressie.

De uitgevouwen kolom heeft altijd dynamische type. Gebruik een cast zoals `todatetime()` of `toint()` als u wilt berekenen waarden of samenvoegen.

Twee manieren eigenschap zak uitbreidingen voorwaarden wordt voldaan:

* `bagexpansion=bag`: Eigenschappenverzamelingen zijn uitgevouwen in één invoer Eigenschappenverzamelingen. Dit is de standaard-uitbreiding.
* `bagexpansion=array`: Eigenschappenverzamelingen zijn uitgevouwen in twee elementen `[` *sleutel*`,`*waarde* `]` structuren, matrix uniforme toegang geeft tot sleutels en waarden (evenals, bijvoorbeeld een aggregatie unieke aantallen uitgevoerd op de eigenschapnamen). 

**Voorbeelden**


    exceptions | take 1 
  	| mvexpand details[0]

Een uitzonderingsrecord wordt gesplitst in rijen voor elk item in het veld details.



### <a name="parse-operator"></a>parseren van operator

    T | parse "I got 2 socks for my birthday when I was 63 years old" 
    with * "got" counter:long " " present "for" * "was" year:long *


    T | parse kind=relaxed
          "I got no socks for my birthday when I was 63 years old" 
    with * "got" counter:long " " present "for" * "was" year:long * 

    T |  parse kind=regex "I got socks for my 63rd birthday" 
    with "(I|She) got" present "for .*?" year:long * 

Haalt de waarden van een tekenreeks bevat. De beschikking eenvoudige of gewone expressie wordt aangepast.

**Syntaxis**

    T | parse [kind=regex|relaxed] SourceText 
        with [Match | Column [: Type [*]] ]  ...

**Argumenten**

* `T`: De invoertabel.
* `kind`: 
 * `simple`(standaard): de `Match` tekenreeksen zijn gewone tekenreeksen.
 * `relaxed`: als de tekst niet als het type van een kolom parseren, de kolom is ingesteld op null-waarden en het parseren blijft 
 * `regex`: de `Match` tekenreeksen zijn reguliere expressies.
* `Text`: Een kolom of andere expressie die resulteert in of kan worden omgezet in een tekenreeks.
* *Vergelijken:* Overeenkomen met het volgende gedeelte van de tekenreeks, en deze verwijderen.
* *Kolom:* Het volgende gedeelte van de tekenreeks toewijzen aan deze kolom. De kolom wordt gemaakt als deze nog niet bestaat.
* *Type:* Het volgende gedeelte van de tekenreeks parseert als het opgegeven type, zoals int, datum, dubbel. 


**Geeft als resultaat**

De invoertabel, uitgebreide op basis van de lijst met kolommen.

De elementen in de `with` component worden vergeleken met de brontekst breken. Elk element chews uit een deel van de brontekst: 

* Een letterlijke tekenreeks of gewone expressie verplaatst de overeenkomende cursor door de lengte van de overeenkomen.
* In een regex te analyseren, een reguliere expressie de beschikking over de operator minimalisering '?' zo snel mogelijk verplaatsen naar de volgende overeenkomen.
* De naam van een kolom met een type parseert de tekst als het opgegeven type. Tenzij type = beperkte, een mislukte parseren wordt ongeldig overeenkomen met het hele patroon.
* De naam van een kolom zonder een type of met het type 'string', wordt het minimum aantal tekens om te gaan naar de volgende overeenkomen gekopieerd.
* ' *' Hiermee slaat u het minimum aantal tekens om te gaan naar de volgende overeenkomen. U kunt '*' aan het begin en einde van het patroon, of na een type dan tekenreeks of tussen overeenkomen.

Alle elementen in een patroon parseren moet overeenkomen met correct; anders worden geen resultaten gemaakt. Deze regel geldt dat als rekening = beperkte, als een getypte variabele parseren mislukt, de rest van het parseren blijft.

**Voorbeelden**

*Eenvoudige:*

```AIQL

// Test without reading a table:
 range x from 1 to 1 step 1 
 | parse "I got 2 socks for my birthday when I was 63 years old" 
    with 
     *   // skip until next match
     "got" 
     counter: long // read a number
     " " // separate fields
     present // copy string up to next match
     "for" 
     *  // skip until next match
     "was" 
     year:long // parse number
     *  // skip rest of string
```

x | item | presenteren | Jaar
---|---|---|---
1 | 2 | SOCKS | 63

*Versoepeld:*

Wanneer de invoer een juiste overeenkomst voor elke getypte kolom bevat, een beperkte parseren dezelfde resultaten produceert als het een eenvoudige parseren. Maar als een van de getypte kolommen niet juist parseren, een beperkte parseren doorgaat met het verwerken van de rest van het patroon, dat een eenvoudige parseren stopt of niet meer genereren van een van de resultaten.


```AIQL

// Test without reading a table:
 range x from 1 to 1 step 1 
 | parse kind="relaxed"
        "I got several socks for my birthday when I was 63 years old" 
    with 
     *   // skip until next match
     "got" 
     counter: long // read a number
     " " // separate fields
     present // copy string up to next match
     "for" 
     *  // skip until next match
     "was" 
     year:long // parse number
     *  // skip rest of string
```


x  | presenteren | Jaar
---|---|---
1 |  SOCKS | 63


*Regex:*

```AIQL

// Run a test without reading a table:
range x from 1 to 1 step 1 
// Test string:
| extend s = "Event: NotifySliceRelease (resourceName=Scheduler, totalSlices=27, sliceNumber=16, lockTime=02/17/2016 08:41, releaseTime=02/17/2016 08:41:00, previousLockTime=02/17/2016 08:40:00)" 
// Parse it:
| parse kind=regex s 
  with ".*?[a-zA-Z]*=" resource 
       ", total.*?sliceNumber=" slice:long *
       "lockTime=" lock
       ",.*?releaseTime=" release 
       ",.*?previousLockTime=" previous:date 
       ".*\\)"
| project-away x, s
```

resource | segment | vergrendelen | Release | vorige
---|---|---|---|---
Planner | 16 | 17-02-2016 08:41:00 | 17-02-2016 08:41 | 2016-02-17T08:40:00Z

### <a name="project-operator"></a>Project-operator

    T | project cost=price*quantity, price

Selecteer de kolommen die u wilt opnemen, hernoemen of weghalen en nieuwe berekende kolommen invoegen. De volgorde van de kolommen in het resultaat is opgegeven door de volgorde van de argumenten. Alleen de kolommen die zijn opgegeven in de argumenten zijn opgenomen in het resultaat: alle andere in het vak Invoerbereik worden niet weergegeven.  (Zie ook `extend`.)


**Syntaxis**

    T | project ColumnName [= Expression] [, ...]

**Argumenten**

* *T:* De invoertabel.
* *ColumnName:* De naam van een kolom in de uitvoer moet worden weergegeven. Als er geen *expressie*, moet een kolom met die naam weergegeven in het vak invoerbereik. [Namen](#names) zijn hoofdlettergevoelig en kunnen bevatten alfabetisch, numeriek of '_' tekens. Gebruik `['...']` of `["..."]` aan de offerte trefwoorden of namen met andere tekens.
* *Expressie:* Optionele scalaire expressie die verwijst naar de invoer kolommen. 

    Is het mogelijk om terug te keren van een nieuwe berekende kolom met dezelfde naam als een bestaande kolom in het vak invoerbereik.

**Geeft als resultaat**

Een tabel met de kolommen die als argumenten en zo veel mogelijk benoemd rijen als de invoertabel.

**Voorbeeld**

Het volgende voorbeeld ziet u verschillende soorten bewerkingen die kunnen worden uitgevoerd met de `project` operator. De invoertabel `T` heeft drie kolommen van het type `int`: `A`, `B`, en `C`. 

```AIQL
T
| project
    X=C,               // Rename column C to X
    A=2*B,             // Calculate a new column A from the old B
    C=strcat("-",tostring(C)), // Calculate a new column C from the old C
    B=2*B,              // Calculate a new column B from the old B
    ['where'] = client_City // rename, using a keyword as a column name
```

### <a name="project-away-operator"></a>Project-opslag operator

    T | project-away column1, column2, ...

Opgegeven kolommen uitsluiten. Het resultaat bevat alle invoer kolommen, behalve de die u een naam geven.

### <a name="range-operator"></a>bereikoperator

    range LastWeek from ago(7d) to now() step 1d

Genereert een tabel met één kolom met waarden. Zoals u ziet dat er een input pijplijn geen. 

|LastWeek|
|---|
|2015-12-05 09:10:04.627|
|2015-12-06 09:10:04.627|
|...|
|2015-12-12-09:10:04.627|



**Syntaxis**

    range ColumnName from Start to Stop step Step

**Argumenten**

* *ColumnName:* De naam van de uit één kolom in de uitvoertabel.
* *Starten:* De kleinste waarde in de uitvoer.
* *Stoppen:* De hoogste waarde in de uitvoer (of een afhankelijk van de hoogste waarde, als *stap* over deze waarde stappen) wordt gegenereerd.
* *Stap:* Het verschil tussen twee opeenvolgende waarden. 

De argumenten moeten numerieke, datum of tijdspanne waarden. Ze kunnen niet verwijzen naar de kolommen van een tabel. (Als u berekenen van het bereik op basis van een invoertabel wilt, gebruik de [bereik, *functie*](#range), wellicht met de [operator mvexpand](#mvexpand-operator).) 

**Geeft als resultaat**

Een tabel met één kolom *ColumnName*, waarvan de waarden *starten*, *Start zijn*genoemd + *stap*... tot en met *stoppen*.

**Voorbeeld**  

```AIQL
range Steps from 1 to 8 step 3
```

Een tabel met één kolom genoemd `Steps` waarvan het type `long` en waarvan de waarden `1`, `4`, en `7`.

**Voorbeeld**

    range LastWeek from bin(ago(7d),1d) to now() step 1d

Een tabel met middernacht bij de afgelopen zeven dagen. De functie opslaglocatie (floor) Hiermee reduceert u elke keer naar het begin van de dag.

**Voorbeeld**  

```AIQL
range timestamp from ago(4h) to now() step 1m
| join kind=fullouter
  (traces
      | where timestamp > ago(4h)
      | summarize Count=count() by bin(timestamp, 1m)
  ) on timestamp
| project Count=iff(isnull(Count), 0, Count), timestamp
| render timechart  
```

Ziet u hoe de `range` operator kan worden gebruikt voor het maken van een klein, ad-hoc, dimensietabel die vervolgens wordt gebruikt in te voeren nullen waarvan de brongegevens geen waarden bevat.

### <a name="reduce-operator"></a>operator verkleinen

    exceptions | reduce by outerMessage

Pogingen doen om te groeperen soortgelijke records. Voor elke groep, de operator Hiermee kunt u de `Pattern` vermoedelijk uitstekend beschreven die groep, en de `Count` met records in die groep.


![](./media/app-insights-analytics-reference/reduce.png)

**Syntaxis**

    T | reduce by  ColumnName [ with threshold=Threshold ]

**Argumenten**

* *ColumnName:* De kolom te onderzoeken. Dit moet zijn van het tekenreekstype.
* *Drempelwaarde:* Een waarde in het bereik {0..1}. Standaard is 0,001. Voor grote invoeritems moet drempel klein. 

**Geeft als resultaat**

Twee kolommen, `Pattern` en `Count`. In veel gevallen worden patroon een volledige waarde uit de kolom. In sommige gevallen, kan deze algemene termen identificeren en vervangen van de variabele delen met ' *'.

Bijvoorbeeld het resultaat van `reduce by city` , bevatten mogelijk: 

|Patroon | Tellen |
|---|---|
| SAN * | 5182 |
| Saint * | 2846 |
| Moskou | 3726 |
| \*-op-\* | 2730 |
| Parijs | 27163 |


### <a name="render-directive"></a>Richtlijn weergegeven

    T | render [ table | timechart  | barchart | piechart ]

Render wordt u omgeleid zodat de laag presentatie het weergeven van de tabel. Het laatste element van de pijp moet worden. Het is een alternatief voor het gebruik van de besturingselementen op de weergave, zodat u kunt een query opslaan met een bepaalde presentatie-methode.

### <a name="restrict-clause"></a>component beperken 

Hiermee wordt de set beschikbaar voor de operatoren die volgt tabelnamen. Bijvoorbeeld:

    let e1 = requests | project name, client_City;
    let e2 =  requests | project name, success;
    // Exclude predefined tables from the union:
    restrict access to (e1, e2);
    union * |  take 10 

### <a name="sort-operator"></a>de operator sorteren 

    T | sort by country asc, price desc

De rijen van de invoertabel sorteren in volgorde door een of meer kolommen.

**Alias**`order`

**Syntaxis**

    T  | sort by Column [ asc | desc ] [ `,` ... ]

**Argumenten**

* *T:* De tabel invoer om te sorteren.
* *Kolom:* Kolom met *T* door waarop u wilt sorteren. Het type van de waarden moet numerieke, datum, tijd of tekenreeks.
* `asc`Sorteren op in oplopende volgorde, van laag naar hoog. De standaardinstelling is `desc`, aflopend hoog naar laag.

**Voorbeeld**

```AIQL
Traces
| where ActivityId == "479671d99b7b"
| sort by Timestamp asc
```
Alle rijen in de tabel Traces waarvoor een specifiek `ActivityId`, gesorteerd op hun tijdstempel.

### <a name="summarize-operator"></a>operator samenvatten

Genereert een tabel die de inhoud van de invoertabel is samengevoegd.
 
    requests
  	| summarize count(), avg(duration), makeset(client_City) 
      by client_CountryOrRegion

Een tabel waarin het aantal, gemiddelde van de aanvraag en set steden in elk land. Er is een rij in de uitvoer voor elk land distinct. De uitvoerkolommen bevatten het aantal, gemiddelde duur, steden en land. Alle andere invoer kolommen worden genegeerd.


    T | summarize count() by price_range=bin(price, 10.0)

Een tabel waarin hoeveel items staan met prijzen in elk interval [0,10.0], [10.0,20.0], enzovoort. In dit voorbeeld heeft een kolom voor de telling en een voor het prijsbereik. Alle andere invoer kolommen worden genegeerd.


**Syntaxis**

    T | summarize
         [  [ Column = ] Aggregation [ `,` ... ] ]
         [ by
            [ Column = ] GroupExpression [ `,` ... ] ]

**Argumenten**

* *Kolom:* Optionele naam voor een resultaatkolom. Standaardwaarden naar een naam afgeleid van de expressie. [Namen](#names) zijn hoofdlettergevoelig en kunnen bevatten alfabetisch, numeriek of '_' tekens. Gebruik `['...']` of `["..."]` aan de offerte trefwoorden of namen met andere tekens.
* *Aggregatie:* Een oproep naar een aggregatiefunctie zoals `count()` of `avg()`, met de kolomnamen als argumenten. Zie [aggregaties](#aggregations).
* *GroupExpression:* Een expressie over de kolommen, waarmee een reeks unieke waarden. Meestal is een van beide een kolomnaam die al beschikt over een beperkt aantal waarden, of `bin()` met een numerieke of tijd kolom als argument. 

Als u een expressie numerieke of tijd zonder `bin()`, Analytics dit automatisch toegepast met een interval van `1h` voor momenten, of `1.0` voor getallen.

Als u niet een *GroupExpression,* wordt de hele tabel wordt samengevat in een rij één uitvoer.



**Geeft als resultaat**

De invoer rijen zijn gerangschikt in groepen met dezelfde waarde van de `by` expressies. Vervolgens wordt de opgegeven aggregatiefuncties berekend boven elke groep, een rij voor elke groep produceren. Het resultaat bevat de `by` kolommen en ook ten minste één kolom voor elk berekend aggregatie. (Sommige aggregatiefuncties retourneren meerdere kolommen).

Het resultaat is hetzelfde aantal rijen zijn in verschillende combinaties van `by` waarden. Als u samenvatten over bereiken van numerieke waarden wilt, gebruikt u `bin()` te bereiken beperken op afzonderlijke waarden.

**Opmerking**

Hoewel u willekeurige expressies voor de aggregatie en de groepering expressies opgeeft kunt, is het efficiënter te gebruiken van eenvoudige kolomnamen of toepassen `bin()` naar een numerieke kolom.



### <a name="take-operator"></a>operator uitvoeren

Alias van [limiet](#limit-operator)


### <a name="top-operator"></a>Top-operatoren

    T | top 5 by Name desc nulls first

Geeft als resultaat de eerste *N* -records die op de opgegeven kolommen zijn gesorteerd.


**Syntaxis**

    T | top NumberOfRows by Sort_expression [ `asc` | `desc` ] [`nulls first`|`nulls last`] [, ... ]

**Argumenten**

* *Aantal_rijen:* Het aantal rijen van *T* om terug te keren.
* *Sort_expression:* Een expressie waarmee de rijen sorteren. Dit is meestal alleen de kolomnaam van een. U kunt meer dan één sort_expression opgeven.
* `asc`of `desc` (de standaardinstelling) wordt mogelijk weergegeven aan een besturingselement of de selectie daadwerkelijk vanuit de "onder" of "boven" van het bereik.
* `nulls first`of `nulls last` besturingselementen waar null-waarden worden weergegeven. `First`is de standaardinstelling voor `asc`, `last` is de standaardinstelling voor `desc`.


**Tips**

`top 5 by name`Oppervlakkig gezien lijken equivalent is aan `sort by name | take 5`. Echter sneller en altijd in geeft resultaten worden gesorteerd dat `take` zorgt ervoor dat geen garantie.

### <a name="top-nested-operator"></a>Top-geneste operator

    requests 
  	| top-nested 5 of name by count()  
    , top-nested 3 of performanceBucket by count() 
    , top-nested 3 of client_CountryOrRegion by count()
  	| render barchart 

Hiërarchische resultaten, waarbij elk niveau een Inzoomoptie uit het niveau van de vorige is oplevert. Het is handig voor het beantwoorden van vragen die geluid zoals "wat de bovenste 5 aanvragen zijn voor elk van deze, wat de prestaties van het bovenste 3 gerangschikte verzamelingen zijn en voor elk van deze, die zijn de belangrijkste 3 landen de aanvragen vandaan?"

**Syntaxis**

   T | N van kolom begin genest voor aggregatie [,...]

**Argumenten**

* N:int - aantal rijen om te vragen of doorgeven naar een hoger niveau. In een query met drie niveaus, waarbij N staat voor 5, 3 en 3, is het totale aantal rijen 45.
* KOLOM - een kolom groeperen op voor aggregatie. 
* AGGREGATIE - een [Aggregatiefunctie](#aggregations) toe te passen op elke groep van rijen. De resultaten van deze aggregaties bepaalt de bovenste groepen moet worden weergegeven.


### <a name="union-operator"></a>de verenigingsoperator

     Table1 | union Table2, Table3

Hiermee gaat twee of meer tabellen en geeft als resultaat de rijen van alle labels. 

**Syntaxis**

    T | union [ kind= inner | outer ] [ withsource = ColumnName ] Table2 [ , ...]  

    union [ kind= inner | outer ] [ withsource = ColumnName ] Table1, Table2 [ , ...]  

**Argumenten**

* *Tabel1*, *tabel2* ...
 *  De naam van een tabel, zoals `requests`, of een tabel die is gedefinieerd in een die wordt [component laten](#let-clause); of
 *  Een query-expressie, zoals`(requests | where success=="True")`
 *  Een reeks tabellen die zijn opgegeven met een jokerteken. Bijvoorbeeld `e*` zou de vereniging van de tabellen die zijn gedefinieerd in de vorige laten componenten waarvan de naam van start is gegaan met "e", samen met de uitzonderingentabel '' formulier.
* `kind`: 
 * `inner`-Het resultaat heeft de subset van kolommen die voor alle invoer tabellen gelden.
 * `outer`-Het resultaat heeft de kolommen die optreden in een van de invoer. Cellen die niet zijn gedefinieerd door een invoer rij zijn ingesteld op `null`.
* `withsource=`*ColumnName:* Als u opgeeft, bevat de uitvoer een kolom met de naam van *ColumnName* waarvan de waarde wordt aangegeven welke brontabel elke rij heeft toegevoegd.

**Geeft als resultaat**

Een tabel met zoveel rijen als er zijn in de invoer tabellen en zoveel kolommen als er zijn unieke kolomnamen in de invoer.

**Voorbeeld**

```AIQL

let ttrr = requests | where timestamp > ago(1h);
let ttee = exceptions | where timestamp > ago(1h);
union tt* | count
```
Union van alle tabellen waarvan de naam "tt" begint.


**Voorbeeld**

```AIQL

union withsource=SourceTable kind=outer Query, Command
| where Timestamp > ago(1d)
| summarize dcount(UserId)
```
Het aantal afzonderlijke gebruikers die u hebt een geproduceerd een `exceptions` gebeurtenis of een `traces` gebeurtenis via de oude dag. In het resultaat, wordt de kolom 'Brontabel' van "Query" of "Opdracht" aangegeven.

```AIQL
exceptions
| where Timestamp > ago(1d)
| union withsource=SourceTable kind=outer 
   (Command | where Timestamp > ago(1d))
| summarize dcount(UserId)
```

Deze efficiënter versie oplevert hetzelfde resultaat. Elke tabel Hiermee worden gefilterd voordat u de Unie maakt.

### <a name="where-operator"></a>waar operator

     requests | where resultCode==200

Hiermee filtert u een tabel naar de deelverzameling met rijen die voldoen aan een predikaat.

**Alias**`filter`

**Syntaxis**

    T | where Predicate

**Argumenten**

* *T:* De tabelvormige invoer waarvan de records die moeten worden gefilterd.
* *Predikaatfunctie:* A `boolean` [expressie](#boolean) over de kolommen van *T*. Deze wordt geëvalueerd voor elke rij in *T*.

**Geeft als resultaat**

Rijen in *T* waarvoor *predikaatfunctie* is `true`.

**Tips**

De snelste prestaties opvragen:

* **Gebruik eenvoudige vergelijkingen** tussen kolomnamen en constanten. ('Constante' betekent constante voor de tabel - zodat `now()` en `ago()` zijn in orde en dus worden scalaire waarden toegewezen met een [ `let` component](#let-clause).)

    Bijvoorbeeld liever `where Timestamp >= ago(1d)` naar `where floor(Timestamp, 1d) == ago(1d)`.

* **Simplest eerste voorwaarden**: als er meerdere componenten conjoined met `and`, plaatsen eerst de clausules waarbij u gebruikmaakt van slechts één kolom. Dus `Timestamp > ago(1d) and OpId == EventId` is beter dan de andere leren kennen.


**Voorbeeld**

```AIQL
traces
| where Timestamp > ago(1h)
    and Source == "Kuskus"
    and ActivityId == SubActivityIt 
```

Records die niet ouder zijn dan 1 uur staan, en twee kolommen met dezelfde waarde hebben afkomstig zijn uit de bron 'Kuskus' genoemd. 

Zoals u ziet dat we op te de vergelijking tussen twee kolommen laatst, nemen er kan geen gebruikmaken van de index en forceert u een gescande afbeelding.


### <a name="where-in-operator"></a>waar in-operator

    requests | where resultCode !in (200, 201)

    requests | where resultCode in (403, 404)

**Syntaxis**

    T | where col in (expr1, expr2, ...)
    T | where col !in (expr1, expr2, ...)

**Argumenten**

* `col`: Een kolom in de tabel.
* `expr1`...: Een lijst met scalaire expressies.

Gebruik `in` wordt gebruikt om op te nemen alleen rijen waarin `col` gelijk is aan een van de expressies `expr1...`.

Gebruik `!in` om op te nemen alleen rijen waarin `col` niet gelijk is aan een van de expressies `expr1...`.  


## <a name="aggregations"></a>Aggregaties

Aggregaties zijn functies die worden gebruikt om waarden in groepen die zijn gemaakt in de [bewerking samenvatten](#summarize-operator)te combineren. In deze query is dcount() bijvoorbeeld een aggregatiefunctie:

    requests | summarize dcount(name) by success

### <a name="any"></a>een 

    any(Expression)

Willekeurig wordt één rij van de groep geselecteerd en geeft als resultaat de waarde van de opgegeven expressie.

Dit is bijvoorbeeld handig als een van de kolommen heeft een groot aantal vergelijkbare waarden (bijvoorbeeld in een kolom "fouttekst") en u wilt die kolom eenmaal per een unieke waarde van de sleutel samengestelde groep voorbeeld. 

**Voorbeeld**  

```

traces 
| where timestamp > now(-15min)  
| summarize count(), any(message) by operation_Name 
| top 10 by count_level desc 
```

<a name="argmin"></a>
<a name="argmax"></a>
### <a name="argmin-argmax"></a>argmin, argmax

    argmin(ExprToMinimize, * | ExprToReturn  [ , ... ] )
    argmax(ExprToMaximize, * | ExprToReturn  [ , ... ] ) 

Geeft als resultaat een rij in de groep die *ExprToMaximize*wordt geminimaliseerd/maximaliseert en geeft als resultaat de waarde van *ExprToReturn* (of `*` om terug te keren de hele rij).

**Tip**: automatisch namen van de kolommen doorgegeven via worden gewijzigd. Controleren de resultaten gebruiken om ervoor te zorgen dat u gebruikt de juiste namen, `take 5` voordat u de resultaten in een andere operator pipe.

**Voorbeelden**

Voor elke naam verzoek weergeven wanneer de langste aanvraag is opgetreden:

    requests | summarize argmax(duration, timestamp) by name

De details van de langste aanvraag, niet alleen de tijdstempel weergeven:

    requests | summarize argmax(duration, *) by name


De laagste waarde van elke meting, samen met de tijdstempel en andere gegevens vinden:

    metrics 
  	| summarize minValue=argmin(value, *) 
      by name


![](./media/app-insights-analytics-reference/argmin.png)
 


### <a name="avg"></a>Avg

    avg(Expression)

Berekent het gemiddelde van de *expressie* in de groep.

### <a name="buildschema"></a>buildschema

    buildschema(DynamicExpression)

Geeft als resultaat de minimale schema die eerst worden alle waarden van *DynamicExpression*. 

Het type van de kolom parameter moet `dynamic` -een matrix of een eigenschap zak. 

**Voorbeeld**

    exceptions | summarize buildschema(details)

Resultaat:

    { "`indexer`":
     {"id":"string",
       "parsedStack":
       { "`indexer`": 
         {  "level":"int",
            "assembly":"string",
            "fileName":"string",
            "method":"string",
            "line":"int"
         }},
      "outerId":"string",
      "message":"string",
      "type":"string",
      "rawStack":"string"
    }}

U ziet dat `indexer` wordt gebruikt om waar u een numerieke index moet gebruiken. Voor dit schema, sommige geldige paden zou worden (mits dat deze indexen voorbeeld zijn bereik):

    details[0].parsedStack[2].level
    details[0].message
    arraylength(details)
    arraylength(details[0].parsedStack)

**Voorbeeld**

Wordt ervan uitgegaan dat de invoer kolom heeft drie dynamische waarden:

| |
|---|
|`{"x":1, "y":3.5}`
|`{"x":"somevalue", "z":[1, 2, 3]}`
|`{"y":{"w":"zzz"}, "t":["aa", "bb"], "z":["foo"]}`


De resulterende schema zou zijn:

    { 
      "x":["int", "string"], 
      "y":["double", {"w": "string"}], 
      "z":{"`indexer`": ["int", "string"]}, 
      "t":{"`indexer`": "string"} 
    }

Het schema vertellen die:

* Het object hoofdmap is een container met vier eigenschappen met de naam x, y, z en t.
* De eigenschap met de naam van de 'x' die kan worden van het type 'integer' of van het type 'tekenreeks'.
* De eigenschap 'y' die uit van het type "double" kan de naam of een andere container met een eigenschap "w" van het type 'tekenreeks' genoemd.
* De ``indexer`` trefwoord geeft aan of "z" en "t" matrices zijn.
* Elk item in de matrix "z" is een integer of een tekenreeks.
* "t" is een matrix van tekenreeksen.
* Elke eigenschap is impliciet optioneel en een matrix kan leeg zijn.

##### <a name="schema-model"></a>Schema model

De syntaxis van de resulterende schema is:

    Container ::= '{' Named-type* '}';
    Named-type ::= (name | '"`indexer`"') ':' Type;
    Type ::= Primitive-type | Union-type | Container;
    Union-type ::= '[' Type* ']';
    Primitive-type ::= "int" | "string" | ...;

Ze zijn vergelijkbaar zijn met een subset van de machineschrift type aantekeningen, gecodeerd als een dynamische waarde. In machineschrift zou het voorbeeldschema:

    var someobject: 
    { 
      x?: (number | string), 
      y?: (number | { w?: string}), 
      z?: { [n:number] : (int | string)},
      t?: { [n:number]: string } 
    }


### <a name="count"></a>tellen

    count([ Predicate ])

Retourneert een telling van rijen waarvoor *predikaatfunctie* resulteert in `true`. Als er geen *predikaatfunctie* is opgegeven, geeft als resultaat het totale aantal records in de groep. 

**Tip Perf**: Gebruik `summarize count(filter)` in plaats van`where filter | summarize count()`

> [AZURE.NOTE] Voorkom count() gebruiken om te vinden van het aantal serviceaanvragen, uitzonderingen of andere gebeurtenissen die hebben plaatsgevonden. Wanneer [steekproeven](app-insights-sampling.md) betrekking heeft is, wordt het aantal gegevenspunten blijven behouden in de toepassing inzichten kleiner dan het aantal oorspronkelijke gebeurtenissen worden. Gebruik in plaats daarvan `summarize sum(itemCount)...`. De eigenschap itemCount geeft het aantal oorspronkelijke gebeurtenissen die worden aangegeven met elk gegevenspunt behouden.

### <a name="countif"></a>aantal.Als

    countif(Predicate)

Retourneert een telling van rijen waarvoor *predikaatfunctie* resulteert in `true`.

**Tip Perf**: Gebruik `summarize countif(filter)` in plaats van`where filter | summarize count()`

> [AZURE.NOTE] Vermijd het gebruik van countif() om het aantal serviceaanvragen, uitzonderingen of andere gebeurtenissen die hebben plaatsgevonden. Wanneer [steekproeven](app-insights-sampling.md) betrekking heeft is, kan minder dan het aantal werkelijke gebeurtenissen door het aantal gegevenspunten zijn. Gebruik in plaats daarvan `summarize sum(itemCount)...`. De eigenschap itemCount geeft het aantal oorspronkelijke gebeurtenissen die worden aangegeven met elk gegevenspunt behouden.

### <a name="dcount"></a>DBAANTAL

    dcount( Expression [ ,  Accuracy ])

Geeft als resultaat een schatting van het aantal unieke waarden van *Expr* in de groep. (U kunt afzonderlijke waarden gebruiken [`makeset`](#makeset).)

*Nauwkeurigheid*, als u opgeeft, wordt de verhouding tussen snelheid en nauwkeurigheid.

 * `0`= de minste nauwkeurige en snelste berekening.
 * `1`de standaardinstelling, wat inhoudt dat de tijd nauwkeurigheid en berekening; informatie over 0,8%-fout.
 * `2`= meest nauwkeurige en laagst mogelijke berekening; informatie over 0,4%-fout.

**Voorbeeld**

    pageViews 
  	| summarize cities=dcount(client_City) 
      by client_CountryOrRegion

![](./media/app-insights-analytics-reference/dcount.png)


### <a name="dcountif"></a>dcountif

    dcountif( Expression, Predicate [ ,  Accuracy ])

Geeft als resultaat een schatting van het aantal unieke waarden van *Expr* van rijen in de groep waarvan *predikaatfunctie* waar is. (U kunt afzonderlijke waarden gebruiken [`makeset`](#makeset).)

*Nauwkeurigheid*, als u opgeeft, wordt de verhouding tussen snelheid en nauwkeurigheid.

 * `0`= de minste nauwkeurige en snelste berekening.
 * `1`de standaardinstelling, wat inhoudt dat de tijd nauwkeurigheid en berekening; informatie over 0,8%-fout.
 * `2`= meest nauwkeurige en laagst mogelijke berekening; informatie over 0,4%-fout.

**Voorbeeld**

    pageViews 
  	| summarize cities=dcountif(client_City, client_City startswith "St") 
      by client_CountryOrRegion


### <a name="makelist"></a>makelist

    makelist(Expr [ ,  MaxListSize ] )

Geeft als resultaat een `dynamic` (JSON) matrix van alle waarden van *Expr* in de groep. 

* *MaxListSize* is een optionele geheel getal limiet voor het maximum aantal elementen die het resultaat (de standaardinstelling is *128*).

### <a name="makeset"></a>makeset

    makeset(Expression [ , MaxSetSize ] )

Geeft als resultaat een `dynamic` (JSON) matrix van de set unieke waarden die *Expr* in de groep neemt. (Tip: als u wilt alleen de unieke waarden tellen, gebruikt u [`dcount`](#dcount).)
  
*  *MaxSetSize* is een optionele geheel getal limiet voor het maximum aantal elementen die het resultaat (de standaardinstelling is *128*).

**Voorbeeld**

    pageViews 
  	| summarize cities=makeset(client_City) 
      by client_CountryOrRegion

![](./media/app-insights-analytics-reference/makeset.png)

Zie ook de [ `mvexpand` operator](#mvexpand-operator) voor de tegenovergestelde functie.


### <a name="max-min"></a>Max, min

    max(Expr)

Berekent het maximale aantal van *Expr*.
    
    min(Expr)

Berekent het minimum van *Expr*.

**Tip**: Hiermee wordt de min of max op een eigen - bijvoorbeeld, de hoogste of laagste prijs. Maar als u wilt dat andere kolommen in de rij - bijvoorbeeld de naam van de leverancier met de laagste prijs - [argmin of argmax](#argmin-argmax)gebruiken.


<a name="percentile"></a>
<a name="percentiles"></a>
<a name="percentilew"></a>
<a name="percentilesw"></a>
### <a name="percentile-percentiles-percentilew-percentilesw"></a>percentiel, percentielen, percentilew, percentilesw

    percentile(Expression, Percentile)

Geeft als resultaat een schatting voor de *expressie* van het opgegeven percentiel in de groep. De nauwkeurigheid, is afhankelijk van de dichtheid van de populatie in de regio van het percentiel.
    
    percentiles(Expression, Percentile1 [ , Percentile2 ...] )

Zoals `percentile()`, maar berekent een aantal percentielwaarden (dit is sneller dan afzonderlijk berekenen van elke percentiel).

    percentilew(Expression, WeightExpression, Percentile)

Gewogen percentiel. Gebruik deze opdracht voor vooraf geaggregeerde gegevens.  `WeightExpression`een geheel getal dat wordt aangegeven hoeveel oorspronkelijke rijen worden aangegeven met elke rij met geaggregeerde is.

    percentilesw(Expression, WeightExpression, Percentile1, [, Percentile2 ...])

Zoals `percentilew()`, maar een aantal percentielwaarden berekent.

**Voorbeelden**


De waarde van `duration` die groter is dan 95% van de steekproef ingestelde en kleiner is dan 5% van de steekproef is ingesteld, voor de naam van elke aanvraag berekend:

    request 
  	| summarize percentile(duration, 95)
      by name

Weglaat ' door... ' om te berekenen voor de hele tabel.

Tegelijk verschillende percentielen voor verschillende verzoek namen berekenen:

    
    requests 
  	| summarize 
        percentiles(duration, 5, 20, 50, 80, 95) 
      by name

![](./media/app-insights-analytics-reference/percentiles.png)

De resultaten blijkt dat voor de aanvraag /Events/Index, 5% van aanvragen gereageerd in minder dan 2.44s, helft van deze in 3.52s, en 5% zijn trager dan 6.85s.

Meerdere Statistieken berekenen:

    requests 
  	| summarize 
        count(), 
        avg(Duration),
        percentiles(Duration, 5, 50, 95)
      by name

#### <a name="weighted-percentiles"></a>Gewogen percentielen

Gebruik de functies gewogen percentiel in gevallen waarin de gegevens vooraf samengevoegd zijn. 

Stel uw app voert duizenden bewerkingen per seconde en u wilt weten hun latentie. De eenvoudige oplossing is om een toepassing inzichten verzoek of aangepaste gebeurtenis voor elke bewerking te genereren. Hiermee maakt een groot aantal verkeer, hoewel geavanceerde steekproeven kracht zou verkleinen. Maar u toch wilt implementeren van een nog beter oplossing: u schrijft wat code in uw app om samen te voegen van de gegevens voordat u deze toepassing inzicht verzendt. De samengevoegde samenvatting worden verzonden met regelmatige tussenpozen, het tarief weer dat gegevens naar een paar punten per minuut misschien wordt afgetrokken.

Uw code gaat een reeks latentie afmetingen (in milliseconden). Bijvoorbeeld:
    
     { 15, 12, 2, 21, 2, 5, 35, 7, 12, 22, 1, 15, 18, 12, 26, 7 }

Deze telt de afmetingen in de opslaglocaties van de volgende:`{ 10, 20, 30, 40, 50, 100 }`

Regelmatig, dit zorgt ervoor dat een reeks TrackEvent bellen, één voor elke Emmertje, met aangepaste afmetingen in elk gesprek: 

    foreach (var latency in bins.Keys)
    { telemetry.TrackEvent("latency", null, 
         new Dictionary<string, double>
         ({"latency", latency}, {"opCount", bins[latency]}}); }

In Analytics ziet u een dergelijke groep van gebeurtenissen als volgt:

`opCount` | `latency`| betekenis
---|---|---
8 | 10 | = 8 bewerkingen in de opslaglocatie 10ms
6 | 20 | = 6 bewerkingen in de opslaglocatie 20 MS
3 | 30 | = 3 bewerkingen in de opslaglocatie 30ms
1 | 40 | = 1 bewerkingen in de opslaglocatie 40ms

Als u een nauwkeurig overzicht van de oorspronkelijke verdeling van gebeurtenis vertragingstijden, gebruiken we `percentilesw`:

    customEvents | summarize percentilesw(latency, opCount, 20, 50, 80)

De resultaten zijn hetzelfde als wanneer we had zonder opmaak gebruikt `percentiles` op de oorspronkelijke set afmetingen.

> [AZURE.NOTE] Gewogen liggen niet van toepassing op [dat gegevens](app-insights-sampling.md), waarbij elke steekproef rij een steekproef van de oorspronkelijke rijen, in plaats van een opslaglocatie vertegenwoordigt. De gewone percentiel-functies zijn geschikt voor de steekproef gegevens.

#### <a name="estimation-error-in-percentiles"></a>Schatting fout in percentielen

De aggregatie percentielen bevat een niet-geheel exacte waarde aan de hand van de [T-overzicht](https://github.com/tdunning/t-digest/blob/master/docs/t-digest-paper/histo.pdf). 

Enkele belangrijke punten: 

* De grenzen van de fout schatting varieert met de waarde van de gevraagde percentiel. De beste nauwkeurigheid is aan het einde van [0..100] wilt verkleinen, percentielen 0 en 100 zijn de exacte minimum- en maximumwaarden van de verdeling. De nauwkeurigheid afneemt geleidelijk naar het midden van de schaal. Er is op de mediaan slechtste en is beperkt tot 1%. 
* Fout grenzen zijn waargenomen in de rangorde, niet op de waarde. Stel dat percentiel (X, 50) als resultaat gegeven waarde van Xm. De schatting zorgt ervoor dat ten minste 49% en maximaal 51% van de waarden van X kleiner dan Xm zijn. Er is geen theoretische limiet voor het verschil tussen Xm en werkelijke gemiddelde waarde van X.

### <a name="stdev"></a>STDEV

     stdev(Expr)

Geeft als resultaat de standaarddeviatie van *Expr* boven de groep.

### <a name="variance"></a>variantie

    variance(Expr)

Retourneert de variantie van *Expr* boven de groep.

### <a name="sum"></a>som

    sum(Expr)

Geeft als resultaat de som van *Expr* boven de groep.                      


## <a name="scalars"></a>Hoeken

[webcasts](#casts) | [vergelijkingen](#scalar-comparisons)
<br/>
[GETTYPE](#gettype) | [hash](#hash) | [iff](#iff) |  [isnull](#isnull) | [isnotnull](#isnotnull) | [notnull](#notnull) | [toscalar](#toscalar)

De ondersteunde typen zijn:

| Type      | Als u meer namen   | Equivalente .NET-type |
| --------- | -------------------- | -------------------- |
| `bool`    | `boolean`            | `System.Boolean`     |
| `datetime`| `date`               | `System.DateTime`    |
| `dynamic` |                      | `System.Object`      |
| `guid`    | `uuid`, `uniqueid`   | `System.Guid`        |
| `int`     |                      | `System.Int32`       |
| `long`    |                      | `System.Int64`       |
| `double`  | `real`               | `System.Double`      |
| `string`  |                      | `System.String`      |
| `timespan`| `time`               | `System.TimeSpan`    |

### <a name="casts"></a>Webcasts

U kunt van het ene type geconverteerd naar een andere. In het algemeen, als de conversie relevant is, werkt het:

    todouble(10), todouble("10.6")
    toint(10.6) == 11
    floor(10.6) == 10
    toint("200")
    todatetime("2016-04-28 13:02")
    totimespan("1.5d"), totimespan("1.12:00:00")
    toguid("00000000-0000-0000-0000-000000000000")
    tostring(42.5)
    todynamic("{a:10, b:20}")

Controleren of een tekenreeks kan worden geconverteerd naar een specifiek type:

    iff(notnull(todouble(customDimensions.myValue)),
       ..., ...)

### <a name="scalar-comparisons"></a>Scalaire vergelijkingen

||
---|---
`<` |Minder
`<=`|Minder of gelijk aan
`>` |Groter
`>=`|Groter of gelijk aan
`<>`|Niet gelijk is aan
`!=`|Niet gelijk is aan 
`in`| Rechter operand is een matrix (dynamische) en linker operand is gelijk aan een van de elementen ervan.
`!in`| Rechter operand is een matrix (dynamische) en links operand niet gelijk is aan een van de elementen ervan.




### <a name="gettype"></a>GETTYPE

**Geeft als resultaat**

Een tekenreeks die het onderliggende opslagtype van het argument om één vertegenwoordigt. Dit is vooral handig wanneer u waarden identificatie hebt `dynamic`: in dit geval `gettype()` hoe een waarde gecodeerd worden weergegeven.

**Voorbeelden**

|||
---|---
`gettype("a")` |`"string" `
`gettype(111)` |`"long" `
`gettype(1==1)` |`"int8"`
`gettype(now())` |`"datetime" `
`gettype(1s)` |`"timespan" `
`gettype(parsejson('1'))` |`"int" `
`gettype(parsejson(' "abc" '))` |`"string" `
`gettype(parsejson(' {"abc":1} '))` |`"dictionary"` 
`gettype(parsejson(' [1, 2, 3] '))` |`"array"` 
`gettype(123.45)` |`"real" `
`gettype(guid(12e8b78d-55b4-46ae-b068-26d7a0080254))` |`"guid"` 
`gettype(parsejson(''))` |`"null"`
`gettype(1.2)==real` | `true`

### <a name="hash"></a>hash

**Syntaxis**

    hash(source [, mod])

**Argumenten**

* *bron*: de bron scalaire de hash wordt berekend op.
* *rest*: de modulo waarde moet worden toegepast op het hash-resultaat.

**Geeft als resultaat**

De waarde xxhash (lang) van de opgegeven scalaire modulo de opgegeven rest waarde (indien opgegeven).

**Voorbeelden**

```
hash("World")                   // 1846988464401551951
hash("World", 100)              // 51 (1846988464401551951 % 100)
hash(datetime("2015-01-01"))    // 1380966698541616202
```
### <a name="iff"></a>IFF

De `iff()` functie evalueert het eerste argument (het predikaat) en retourneert de waarde van de tweede of derde argumenten afhankelijk of de predikaat zich `true` of `false`. De tweede en derde argumenten moeten van hetzelfde type.

**Syntaxis**

    iff(predicate, ifTrue, ifFalse)


**Argumenten**

* *predikaat:* Een expressie die resulteert in een `boolean` waarde.
* *ifTrue:* Een expressie die wordt geëvalueerd en de bijbehorende waarde geretourneerd van de functie als *predikaat* resulteert het in `true`.
* *ifFalse:* Een expressie die wordt geëvalueerd en de bijbehorende waarde geretourneerd van de functie als *predikaat* resulteert het in `false`.

**Geeft als resultaat**

Deze functie retourneert de waarde van *ifTrue* als *predikaat* resulteert in `true`, of de waarde van *ifFalse* anders.

**Voorbeeld**

```
iff(floor(timestamp, 1d)==floor(now(), 1d), "today", "anotherday")
```

<a name="isnull"/></a>
<a name="isnotnull"/></a>
<a name="notnull"/></a>
### <a name="isnull-isnotnull-notnull"></a>IsNull, isnotnull, notnull

    isnull(parsejson("")) == true

Hiermee gaat één argument en wordt aangegeven of het is null.

**Syntaxis**


    isnull([value])


    isnotnull([value])


    notnull([value])  // alias for isnotnull

**Geeft als resultaat**

Waar of ONWAAR afhankelijk van of u de wordt de waarde is null of niet null.


|x|IsNull(x)
|---|---
| "" | ONWAAR
|'x' | ONWAAR
|parsejson("")|waar
|parsejson("[]")|ONWAAR
|parsejson("{}")|ONWAAR

**Voorbeeld**

    T | where isnotnull(PossiblyNull) | count

U ziet dat er zijn andere manieren om dit effect te bereiken:

    T | summarize count(PossiblyNull)

### <a name="toscalar"></a>toscalar

Evalueert een query of een expressie en geeft het resultaat als een enkele waarde. Deze functie is handig voor gefaseerde berekeningen; bijvoorbeeld het totale aantal gebeurtenissen berekenen en vervolgens gebruiken als een basislijn.

**Syntaxis**

    toscalar(query)
    toscalar(scalar)

**Geeft als resultaat**

Het argument geëvalueerd. Als het argument een tabel is, geeft als resultaat de eerste kolom van de eerste rij. (Goede gewoonte is dat het argument slechts één kolom en rij heeft rangschikken.)

**Voorbeeld**

```AIQL

    // Get the count of requests 5 days ago:
    let baseline = toscalar(requests  
        | where floor(timestamp, 1d) == floor(ago(5d),1d) | count);
    // List the counts relative to that baseline:
    requests | summarize daycount = count() by floor(timestamp, 1d)  
  	| extend relative = daycount - baseline
```




### <a name="boolean-literals"></a>Booleaanse letterlijke waarden

    true == 1
    false == 0
    gettype(true) == "int8"
    typeof(bool) == typeof(int8)

### <a name="boolean-operators"></a>Booleaans operatoren

    and 
    or 

    

## <a name="numbers"></a>Getallen

[abs](#abs) | [bin](#bin) | [exp](#exp) | [floor](#floor) | [gamma](#gamma) |[log](#log) | [rand](#rand) | [range](#range) | [sqrt](#sqrt) 
| [todouble](#todouble) | [toint](#toint) | [tolong](#tolong)

### <a name="numeric-literals"></a>Numerieke letterlijke waarden

|||
|---|---
|`42`|`long`
|`42.0`|`real`

### <a name="arithmetic-operators"></a>Rekenkundige operators

|| |
|---|-------------|
| + | Toevoegen         |
| - | Aftrekken    |
| * | Vermenigvuldigen    |
| / | Delen      |
| % | Modulo      |
||
|`<` |Minder
|`<=`|Minder of gelijk aan
|`>` |Groter
|`>=`|Groter of gelijk aan
|`<>`|Niet gelijk is aan
|`!=`|Niet gelijk is aan 


### <a name="abs"></a>ABS

**Syntaxis**

    abs(x)

**Argumenten**

* x - een geheel getal, reëel of tijdspanne

**Geeft als resultaat**

    iff(x>0, x, -x)

<a name="bin"></a><a name="floor"></a>
### <a name="bin-floor"></a>opslaglocatie, verdieping

Rondt waarden omlaag naar een geheel getal veelvoud is van een bepaalde opslaglocatie grootte. Veel gebruikt in de [`summarize by`](#summarize-operator) query. Als er een verspreid verzameling waarden, worden ze gegroepeerd in een kleinere set specifieke waarden.

Alias `floor`.

**Syntaxis**

     bin(value, roundTo)
     floor(value, roundTo)

**Argumenten**

* *waarde:* Een getal, datum of tijdspanne. 
* *roundTo:* De "opslaglocatie grootte". Een getal, datum of tijdspanne die *is deelbaar op*. 

**Geeft als resultaat**

Het dichtstbijzijnde veelvoud van *roundTo* onder *waarde*.  
 
    (toint((value/roundTo)-0.5)) * roundTo

**Voorbeelden**

Expressie | Resultaat
---|---
`bin(4.5, 1)` | `4.0`
`bin(time(16d), 7d)` | `14d`
`bin(datetime(1953-04-15 22:25:07), 1d)`|  `datetime(1953-04-15)`


De volgende expressie berekent een histogram van duur, met een grootte Emmertje van 1 seconde:

```AIQL

    T | summarize Hits=count() by bin(Duration, 1s)
```

### <a name="exp"></a>EXP

    exp(v)   // e raised to the power v
    exp2(v)  // 2 raised to the power v
    exp10(v) // 10 raised to the power v


### <a name="floor"></a>Floor

Een alias voor [`bin()`](#bin).

### <a name="gamma"></a>gamma

De [gamma-functie](https://en.wikipedia.org/wiki/Gamma_function)

**Syntaxis**

    gamma(x)

**Argumenten**

* *x:* Een reëel getal

Voor positieve gehele getallen, `gamma(x) == (x-1)!` bijvoorbeeld `gamma(5) == 4 * 3 * 2 * 1`.

Zie ook [loggamma](#loggamma).


### <a name="log"></a>log

    log(v)    // Natural logarithm of v
    log2(v)   // Logarithm base 2 of v
    log10(v)  // Logarithm base 10 of v


`v`moet u een reëel getal > 0. Anders wordt Null-waarden geretourneerd.

### <a name="loggamma"></a>loggamma


De natuurlijke logaritme van de absolute waarde van de [gamma-functie](#gamma).

**Syntaxis**

    loggamma(x)

**Argumenten**

* *x:* Een reëel getal


### <a name="rand"></a>ASELECT

Een willekeurige getallen.

* `rand()`-een reëel getal tussen 0,0 en 1,0
* `rand(n)`-een geheel getal tussen 0 en n-1




### <a name="sqrt"></a>WORTEL

De functie vierkantswortel weer te geven.  

**Syntaxis**

    sqrt(x)

**Argumenten**

* *x:* Een reëel getal > = 0.

**Geeft als resultaat**

* Een positief getal dat`sqrt(x) * sqrt(x) == x`
* `null`Als het argument een negatief getal is of niet worden omgezet in een `real` waarde. 




### <a name="toint"></a>toint

    toint(100)        // cast from long
    toint(20.7) == 21 // nearest int from double
    toint(20.4) == 20 // nearest int from double
    toint("  123  ")  // parse string
    toint(a[0])       // cast from dynamic
    toint(b.c)        // cast from dynamic

### <a name="tolong"></a>ToLong

    tolong(20.7) == 21 // conversion from double
    tolong(20.4) == 20 // conversion from double
    tolong("  123  ")  // parse string
    tolong(a[0])       // cast from dynamic
    tolong(b.c)        // cast from dynamic


### <a name="todouble"></a>ToDouble

    todouble(20) == 20.0 // conversion from long or int
    todouble(" 12.34 ")  // parse string
    todouble(a[0])       // cast from dynamic
    todouble(b.c)        // cast from dynamic



## <a name="date-and-time"></a>Datum en tijd


[geleden](#ago) | [dayofmonth](#dayofmonth) | [dag van de week](#dayofweek) |  [dayofyear](#dayofyear) |[datepart](#datepart) | [endofday](#endofday) | [endofmonth](#endofmonth) | [endofweek](#endofweek) | [endofyear](#endofyear) | [getmonth](#getmonth)|  [getyear](#getyear) | [nu](#now) | [startofday](#startofday) | [startofmonth](#startofmonth) | [startofweek](#startofweek) | [startofyear](#startofyear) | [todatetime](#todatetime) | [totimespan](#totimespan) | [weekofyear](#weekofyear)

### <a name="date-and-time-literals"></a>Datum en tijd letterlijke waarden

|||
---|---
**datum /**|
`datetime("2015-12-31 23:59:59.9")`<br/>`datetime("2015-12-31")`|Tijden worden altijd in UTC. Weglaten van de datum, kunt een tijd vandaag.
`now()`|De huidige tijd.
`now(`-*tijdspanne*`)`|`now()-`*tijdspanne*
`ago(`*tijdspanne*`)`|`now()-`*tijdspanne*
**tijdspanne**|
`2d`|twee dagen
`1.5h`|1,5 uur 
`30m`|30 minuten
`10s`|10 seconden
`0.1s`|0,1 seconde
`100ms`| 100 milliseconden
`10microsecond`|
`1tick`|100ns
`time("15 seconds")`|
`time("2")`| twee dagen
`time("0.12:34:56.7")`|`0d+12h+34m+56.7s`

### <a name="date-and-time-expressions"></a>Datum en tijd expressies

Expressie |Resultaat
---|---
`datetime("2015-01-02") - datetime("2015-01-01")`| `1d`
`datetime("2015-01-01") + 1d`| `datetime("2015-01-02")`
`datetime("2015-01-01") - 1d`| `datetime("2014-12-31")`
`2h * 24` | `2d`
`2d` / `2h` | `24`
`datetime("2015-04-15T22:33") % 1d` | `timespan("22:33")`
`bin(datetime("2015-04-15T22:33"), 1d)` | `datetime("2015-04-15T00:00")`
||
`<` |Minder
`<=`|Minder of gelijk aan
`>` |Groter
`>=`|Groter of gelijk aan
`<>`|Niet gelijk is aan
`!=`|Niet gelijk is aan 




### <a name="ago"></a>geleden

Hiermee verwijdert u het opgegeven interval van de huidige tijd van de UTC-klok. Zoals `now()`, deze functie kan meerdere keren worden gebruikt in een instructie en de UTC-kloktijd waarnaar wordt verwezen wordt niet hetzelfde zijn voor alle exemplaren.

**Syntaxis**

    ago(a_timespan)

**Argumenten**

* *a_timespan*: Interval om af te trekken van de huidige tijd van de UTC-klok (`now()`).

**Geeft als resultaat**

    now() - a_timespan

**Voorbeeld**

Alle rijen met een tijdstempel in de afgelopen uur:

```AIQL

    T | where timestamp > ago(1h)
```

### <a name="datepart"></a>DatePart

    datepart("Day", datetime(2015-12-14)) == 14

Haalt een bepaald gedeelte van een datum als een geheel getal.

**Syntaxis**

    datepart(part, datetime)

**Argumenten**

* `part:String`-{'Jaar', "Maand", 'Dag', 'Uur', "Minuut", "Tweede", "Milliseconden", "Microseconde", "Nanoseconden"}
* `datetime`

**Geeft als resultaat**

Lang dat staat voor het opgegeven gedeelte.


### <a name="dayofmonth"></a>DayOfMonth

    dayofmonth(datetime("2016-05-15")) == 15 

Het rangtelwoord van de dag van de maand.

**Syntaxis**

    dayofmonth(a_date)

**Argumenten**

* `a_date`: A `datetime`.


### <a name="dayofweek"></a>dag van de week

    dayofweek(datetime("2015-12-14")) == 1d  // Monday

Het gehele getal aantal dagen sinds de voorafgaande zondag, als een `timespan`.

**Syntaxis**

    dayofweek(a_date)

**Argumenten**

* `a_date`: A `datetime`.

**Geeft als resultaat**

De `timespan` sinds middernacht aan het begin van de voorafgaande zondag, naar beneden afgerond tot een geheel getal van dagen.

**Voorbeelden**

```AIQL
dayofweek(1947-11-29 10:00:05)  // time(6.00:00:00), indicating Saturday
dayofweek(1970-05-11)           // time(1.00:00:00), indicating Monday
```

### <a name="dayofyear"></a>DAYOFYEAR

    dayofyear(datetime("2016-05-31")) == 152 
    dayofyear(datetime("2016-01-01")) == 1 

Het rangtelwoord van de dag in het jaar.

**Syntaxis**

    dayofyear(a_date)

**Argumenten**

* `a_date`: A `datetime`.

<a name="endofday"></a><a name="endofweek"></a><a name="endofmonth"></a><a name="endofyear"></a>
### <a name="endofday-endofweek-endofmonth-endofyear"></a>endofday, endofweek, endofmonth, endofyear

    dt = datetime("2016-05-23 12:34")

    endofday(dt) == 2016-05-23T23:59:59.999
    endofweek(dt) == 2016-05-28T23:59:59.999 // Saturday
    endofmonth(dt) == 2016-05-31T23:59:59.999 
    endofyear(dt) == 2016-12-31T23:59:59.999 


### <a name="getmonth"></a>getMonth

Het getal maand (1-12) ophalen uit een datetime.

**Voorbeeld**

    ... | extend month = getmonth(datetime(2015-10-12))

    --> month == 10

### <a name="getyear"></a>getyear

Krijgen het jaar van een datum/tijd.

**Voorbeeld**

    ... | extend year = getyear(datetime(2015-10-12))

    --> year == 2015

### <a name="now"></a>nu

    now()
    now(-2d)

De huidige UTC klok, (optioneel) tijdverschil door een bepaalde tijdspanne. Deze functie kan meerdere keren worden gebruikt in een instructie en wordt de kloktijd waarnaar wordt verwezen naar alle exemplaren hetzelfde.

**Syntaxis**

    now([offset])

**Argumenten**

* *verschuiving:* A `timespan`, toegevoegd aan de huidige klok UTC-tijd. Standaard: 0.

**Geeft als resultaat**

De huidige tijd van de klok UTC als een `datetime`.

    now() + offset

**Voorbeeld**

Bepaalt het interval sinds de gebeurtenis die wordt aangeduid met het predikaat:

```AIQL
T | where ... | extend Elapsed=now() - timestamp
```

<a name="startofday"></a><a name="startofweek"></a><a name="startofmonth"></a><a name="startofyear"></a>
### <a name="startofday-startofweek-startofmonth-startofyear"></a>startofday, startofweek, startofmonth, startofyear

    date=datetime("2016-05-23 12:34:56")

    startofday(date) == datetime("2016-05-23")
    startofweek(date) == datetime("2016-05-22") // Sunday
    startofmonth(date) == datetime("2016-05-01")
    startofyear(date) == datetime("2016-01-01")



### <a name="todatetime"></a>ToDateTime

Alias `datetime()`.

     todatetime("2016-03-28")
     todatetime("03/28/2016")
     todatetime("2016-03-28 14:34:00")
     todatetime("03/28/2016 2:34pm")
     todatetime("2016-03-28T14:34.5Z")
     todatetime(a[0]) 
     todatetime(b.c) 

Controleer of een tekenreeks met een ongeldige datum is:

     iff(notnull(todatetime(customDimensions.myDate)),
         ..., ...)


### <a name="totimespan"></a>ToTimeSpan

Alias `timespan()`.

    totimespan("21d")
    totimespan("21h")
    totimespan(request.duration)

### <a name="weekofyear"></a>weekofyear

    weekofyear(datetime("2016-05-14")) == 21
    weekofyear(datetime("2016-01-03")) == 1
    weekofyear(datetime("2016-12-31")) == 53

Het resultaat van de gehele getal staat voor het weeknummer door het ISO-8601 standaard. De eerste dag van een week is zondag en de eerste week van het jaar is de week waarop de eerste donderdag van het jaar bevat. (De laatste dag van een jaar daarom enkele van de dagen van de week 1 van het volgende jaar bevatten, of de eerste dagen bevatten enkele van de week 52 of 53 van het vorige jaar).


## <a name="string"></a>Tekenreeks

[countof](#countof) | [extraheren](#extract) | [extractjson](#extractjson)  | [isempty](#isempty) | [isnotempty](#isnotempty) | [notempty](#notempty) | [parseurl](#parseurl) | [vervangen](#replace) | [splitsen](#split) | [strcat](#strcat) | [strlen](#strlen) | [subtekenreeks](#substring) | [tolower](#tolower) | [tostring](#tostring) | [toupper](#toupper)


### <a name="string-literals"></a>Letterlijke tekenreeksen

De regels zijn hetzelfde als in JavaScript.

Tekenreeksen mogelijk tussen in enkele of dubbele aanhalingstekens. 

Backslash (`\`) wordt gebruikt om aan te geven, zoals escape-tekens `\t` (tabblad), `\n` (nieuwe regel) en exemplaren van het overkoepelende aanhalingsteken.

* `'this is a "string" literal in single \' quotes'`
* `"this is a 'string' literal in double \" quotes"`
* `@"C:\backslash\not\escaped\with @ prefix"`

### <a name="obfuscated-string-literals"></a>Verborgen letterlijke

Verborgen letterlijke zijn tekenreeksen die Analytics zichtbaar bij het uitvoeren van de tekenreeks (bijvoorbeeld bij aanwijzen). Het proces codeversleuteling vervangt alle verborgen tekens door een begin (`*`) teken.

Als u wilt een verborgen tekenreeks formulier Voeg `h` of 'H'. Bijvoorbeeld:

```
h'hello'
h@'world' 
h"hello"
```

### <a name="string-comparisons"></a>Tekenreeksvergelijkingen

Operator|Beschrijving|Hoofdlettergevoelige|Waar voorbeeld
---|---|---|---
`==`|Is gelijk aan |Ja| `"aBc" == "aBc"`
`<>` `!=`|Niet gelijk is aan|Ja| `"abc" <> "ABC"`
`=~`|Is gelijk aan |Nee| `"abc" =~ "ABC"`
`!~`|Niet gelijk is aan |Nee| `"aBc" !~ "xyz"`
`has`|Rechts-rechter aan de clientzijde (RHS) is een hele term in de linker-rechter aan de clientzijde (LHS)|Nee| `"North America" has "america"`
`!has`|RHS is niet een volledige term in LHS|Nee|`"North America" !has "amer"` 
`hasprefix`|RHS is een voorvoegsel van een term in LHS|Nee|`"North America" hasprefix "ame"`
`!hasprefix`|RHS is niet een voorvoegsel van een term in LHS|Nee|`"North America" !hasprefix "mer"`
`hassuffix`|RHS is een term in LHS achtervoegsel|Nee|`"North America" hassuffix "rth"`
`!hassuffix`|RHS is niet achtervoegsel van een term in LHS|Nee|`"North America" !hassuffix "mer"`
`contains` | RHS vindt plaats als een subtekenreeks van LHS|Nee| `"FabriKam" contains "BRik"`
`!contains`| RHS treedt niet op in LHS|Nee| `"Fabrikam" !contains "xyz"`
`containscs` | RHS vindt plaats als een subtekenreeks van LHS|Ja| `"FabriKam" contains "Kam"`
`!containscs`| RHS treedt niet op in LHS|Ja| `"Fabrikam" !contains "Kam"`
`startswith`|RHS is een eerste subtekenreeks van LHS.|Nee|`"Fabrikam" startswith "fab"`
`!startswith`|RHS is niet een eerste subtekenreeks van LHS.|Nee|`"Fabrikam" !startswith "abr"`
`endswith`|RHS is een terminal subtekenreeks van LHS.|Nee|`"Fabrikam" endswith "kam"`
`!endswith`|RHS is niet een terminal subtekenreeks van LHS.|Nee|`"Fabrikam" !endswith "ka"`
`matches regex`|LHS bevat een overeenkomst met RHS|Ja| `"Fabrikam" matches regex "b.*k"`
`in`|Gelijk aan een van de elementen|Ja|`"abc" in ("123", "345", "abc")`
`!in`|Niet gelijk aan een van de elementen|Ja|`"bc" !in ("123", "345", "abc")`

Gebruik `has` of `in` als u op de aanwezigheid van een volledig lexicale term testen bent - dat wil zeggen een symbool of een alfanumeriek word wordt gevormd door niet-alfanumerieke tekens of begin of einde van veld. `has`voert sneller `contains`, `startswith` of `endswith`. De eerste van deze query's sneller wordt uitgevoerd:

    EventLog | where continent has "North" | count;
    EventLog | where continent contains "nor" | count





### <a name="countof"></a>countof

    countof("The cat sat on the mat", "at") == 3
    countof("The cat sat on the mat", @"\b.at\b", "regex") == 3

Telt het aantal exemplaren van een subtekenreeks in een tekenreeks. Gewone tekenreeks overeenkomsten kunnen overlappen; Regex overeenkomsten doen niet.

**Syntaxis**

    countof(text, search [, kind])

**Argumenten**

* *tekst:* Een tekenreeks.
* *Zoeken:* De gewone tekenreeks of gewone expressie om aan te passen in *tekst*.
* *type:* `"normal"|"regex"` Standaard `normal`. 

**Geeft als resultaat**

Het aantal keren dat de zoekreeks kan worden afgestemd in de container. Gewone tekenreeks overeenkomsten kunnen overlappen; Regex overeenkomsten doen niet.

**Voorbeelden**

|||
|---|---
|`countof("aaa", "a")`| 3 
|`countof("aaaa", "aa")`| 3 (niet 2!)
|`countof("ababa", "ab", "normal")`| 2
|`countof("ababa", "aba")`| 2
|`countof("ababa", "aba", "regex")`| 1
|`countof("abcabc", "a.c", "regex")`| 2
    



### <a name="extract"></a>uitpakken

    extract("x=([0-9.]+)", 1, "hello x=45.6|wo") == "45.6"

Een overeenkomst met een [gewone expressie](#regular-expressions) ophalen uit een tekenreeks. (Optioneel) converteert deze vervolgens de opgehaalde subtekenreeks naar het opgegeven type.

**Syntaxis**

    extract(regex, captureGroup, text [, typeLiteral])

**Argumenten**

* *regex:* Een [normale expressie](#regular-expressions).
* *captureGroup:* Een positieve `int` constante die aangeeft dat de groep vastleggen worden geëxtraheerd. Er staat een 0 voor de volledige overeenkomst, namelijk 1 voor de waarde die overeenkomt met de eerste '(' haakje')' in de normale expressie, 2 of meer voor de volgende haakjes.
* *tekst:* A `string` om te zoeken.
* *typeLiteral:* Een optionele type letterlijke waarde (bijvoorbeeld `typeof(long)`). Als u opgeeft, wordt de opgehaalde subtekenreeks wordt geconverteerd naar dit type. 

**Geeft als resultaat**

Als *regex* een overeenkomst wordt gevonden in *tekst*: de subtekenreeks vergeleken met de opgegeven vastleggen groep *captureGroup*, (optioneel) geconverteerd naar *typeLiteral*.

Als er geen overeenkomst, of de conversie mislukt: `null`. 

**Voorbeelden**

Het voorbeeld van een tekenreeks `Trace` wordt gezocht naar een definitie voor `Duration`. De overeenkomst wordt geconverteerd naar `real`, en vervolgens deze vermenigvuldigd met een tijdconstante (`1s`) zodat `Duration` van het type `timespan`. In dit voorbeeld is gelijk aan 123.45 seconden:

```AIQL
...
| extend Trace="A=1, B=2, Duration=123.45, ..."
| extend Duration = extract("Duration=([0-9.]+)", 1, Trace, typeof(real)) * time(1s) 
```

In dit voorbeeld is gelijk aan `substring(Text, 2, 4)`:

```AIQL
extract("^.{2,2}(.{4,4})", 1, Text)
```

<a name="notempty"></a>
<a name="isnotempty"></a>
<a name="isempty"></a>
### <a name="isempty-isnotempty-notempty"></a>IsEmpty, isnotempty, notempty

    isempty("") == true

Waar als het argument een lege tekenreeks is of null is.
Zie ook [isnull](#isnull).


**Syntaxis**

    isempty([value])


    isnotempty([value])


    notempty([value]) // alias of isnotempty

**Geeft als resultaat**

Geeft aan of het argument een lege tekenreeks of isnull.

|x|IsEmpty(x)
|---|---
| "" | waar
|'x' | ONWAAR
|parsejson("")|waar
|parsejson("[]")|ONWAAR
|parsejson("{}")|ONWAAR


**Voorbeeld**


    T | where isempty(fieldName) | count


### <a name="parseurl"></a>ParseURL

Een URL in de onderdelen splitsen.

**Syntaxis**

    parseurl(urlstring)

**Argumenten**

* *urlstring:* EEN URL.

**Geeft als resultaat**

Een object met de onderdelen als tekenreeksen.

**Voorbeeld**

    parseurl("http://user:pass@contoso.com/icecream/buy.aspx?a=1&b=2#tag")

    {
    "Scheme" : "http",
    "Host" : "contoso.com",
    "Port" : "80",
    "Path" : "/icecream/buy.aspx",
    "Username" : "user",
    "Password" : "pass",
    "Query Parameters" : {"a":"1","b":"2"},
    "Fragment" : "tag"
    }

### <a name="replace"></a>vervangen

Alle regex overeenkomsten vervangen door een andere tekenreeks.

**Syntaxis**

    replace(regex, rewrite, text)

**Argumenten**

* *regex:* De [reguliere expressies](https://github.com/google/re2/wiki/Syntax) naar *tekst*wilt zoeken. Opname groepen haakjes' (' ')' kan bevatten. 
* *herschreven fragment:* De vervangende regex voor elk van de zoekwaarde die zijn aangebracht door *matchingRegex*. Gebruik `\0` om te verwijzen naar de volledige overeenkomst, namelijk `\1` voor de eerste groep vastleggen `\2` enzovoort voor groepen van de volgende vastleggen.
* *tekst:* Een tekenreeks.

**Geeft als resultaat**

*tekst* na het vervangen van alle overeenkomsten van *regex* met evaluaties van *herschreven fragment*. Komt overeen met elkaar niet overlappen.

**Voorbeeld**

Deze instructie:

```AIQL
range x from 1 to 5 step 1
| extend str=strcat('Number is ', tostring(x))
| extend replaced=replace(@'is (\d+)', @'was: \1', str)
```

Heeft de volgende resultaten:

| x    | Str | vervangen|
|---|---|---|
| 1    | Getal is 1.000000  | Getal is: 1.000000|
| 2    | Getal is 2.000000  | Getal is: 2.000000|
| 3    | Getal is 3.000000  | Getal is: 3.000000|
| 4    | Getal is 4.000000  | Getal is: 4.000000|
| 5    | Getal is 5.000000  | Getal is: 5.000000|
 



### <a name="split"></a>splitsen

    split("aaa_bbb_ccc", "_") == ["aaa","bbb","ccc"]

Wordt gesplitst een opgegeven tekenreeks op basis van een bepaald scheidingsteken en geeft als resultaat een tekenreeksmatrix met de subreeksen conatined. (Optioneel) een specifieke subtekenreeks kan worden geretourneerd als bestaat.

**Syntaxis**

    split(source, delimiter [, requestedIndex])

**Argumenten**

* *bron*: de brontekenreeks die gesplitste op basis van de opgegeven scheidingsteken.
* *een scheidingsteken*: het scheidingsteken dat wordt gebruikt om te splitsen de brontekenreeks.
* *requestedIndex*: een optioneel op nul gebaseerde index `int`. Als biedt, bevat de geretourneerde tekenreeksmatrix de gevraagde subtekenreeks als bestaat. 

**Geeft als resultaat**

Een tekenreeksmatrix die de subreeksen van de opgegeven brontekenreeks die worden gescheiden door het opgegeven scheidingsteken bevat.

**Voorbeelden**

```
split("aa_bb", "_")           // ["aa","bb"]
split("aaa_bbb_ccc", "_", 1)  // ["bbb"]
split("", "_")                // [""]
split("a__b")                 // ["a","","b"]
split("aabbcc", "bb")         // ["aa","cc"]
```




### <a name="strcat"></a>strcat

    strcat("hello", " ", "world")

Hiermee wordt tussen 1 en 16-argumenten, die tekenreeksen moeten zijn.

### <a name="strlen"></a>strlen

    strlen("hello") == 5

De lengte van een tekenreeks.

### <a name="substring"></a>subtekenreeks

    substring("abcdefg", 1, 2) == "bc"

Een subtekenreeks ophalen uit een bepaalde brontekenreeks vanaf een bepaalde index. U kunt ook kan de lengte van de gevraagde subtekenreeks worden opgegeven.

**Syntaxis**

    substring(source, startingIndex [, length])

**Argumenten**

* *bron:* De brontekenreeks die de subtekenreeks wordt opgehaald uit.
* *startingIndex:* De nul beginpositie van de gevraagde subtekenreeks.
* *lengte:* Een optionele parameter die kan worden gebruikt om het gewenste aantal tekens in de subtekenreeks opgeven. 

**Geeft als resultaat**

Een subtekenreeks van de opgegeven tekenreeks. De subtekenreeks begint op tekenpositie startingIndex (vanaf nul) en doorgaan naar het einde van de tekenreeks of lengte tekens als opgegeven.

**Voorbeelden**

```
substring("123456", 1)        // 23456
substring("123456", 2, 2)     // 34
substring("ABCD", 0, 2)       // AB
```

### <a name="tolower"></a>tolower

    tolower("HELLO") == "hello"

Converteert een tekenreeks in kleine letters.

### <a name="toupper"></a>ToUpper

    toupper("hello") == "HELLO"

Converteert een tekenreeks naar hoofdletters.



### <a name="guids"></a>GUID 's

    guid(00000000-1111-2222-3333-055567f333de)


## <a name="arrays-objects-and-dynamic"></a>Matrices, objecten en dynamisch

[letterlijke waarden](#dynamic-literals) | [toewijzing](#casting-dynamic-objects) | [operatoren](#operators) | [laten componenten](#dynamic-objects-in-let-clauses)
<br/>
[arraylength](#arraylength) | [extractjson](#extractjson) | [parsejson](#parsejson) | [bereik](#range) | [treepath](#treepath) | [todynamic](#todynamic) | [zip](#zip)


Hier is het resultaat van een query op een uitzondering toepassing inzichten. De waarde in `details` is van een matrix.

![](./media/app-insights-analytics-reference/310.png)

**Indexeren:** Index matrices en objecten net als in JavaScript:

    exceptions | take 1
  	| extend 
        line = details[0].parsedStack[0].line,
        stackdepth = arraylength(details[0].parsedStack)

* Maar gebruiken `arraylength` en andere Analytics-functies (niet '.length'!)

**Die** In sommige gevallen moet worden beschouwd van een element dat u hebt geëxtraheerd uit een object, omdat het type kan variëren. Bijvoorbeeld `summarize...to` moet een specifiek type:

    exceptions 
  	| summarize count() 
      by toint(details[0].parsedStack[0].line)

    exceptions 
  	| summarize count() 
      by tostring(details[0].parsedStack[0].assembly)

**Letterlijke waarden** Als u wilt een expliciete matrix of een eigenschap zak object maken, moet u het schrijven als een tekenreeks met JSON en cast:

    todynamic('[{"x":"1", "y":"32"}, {"x":"6", "y":"44"}]')


**mvexpand:** Als u wilt trekken de eigenschappen van een object in aparte rijen, gebruikt u mvexpand:

    exceptions | take 1 
  	| mvexpand details[0].parsedStack[0]


![](./media/app-insights-analytics-reference/410.png)


**treepath:** U vindt alle paden in een complexe object:

    exceptions | take 1 | project timestamp, details 
  	| extend path = treepath(details) 
  	| mvexpand path


![](./media/app-insights-analytics-reference/420.png)

**buildschema:** De minimale schema die eerst worden alle waarden van de expressie in de tabel zoeken:

    exceptions | summarize buildschema(details)

Resultaat:

    { "`indexer`":
     {"id":"string",
       "parsedStack":
       { "`indexer`": 
         {  "level":"int",
            "assembly":"string",
            "fileName":"string",
            "method":"string",
            "line":"int"
         }},
      "outerId":"string",
      "message":"string",
      "type":"string",
      "rawStack":"string"
    }}

U ziet dat `indexer` wordt gebruikt om waar u een numerieke index moet gebruiken. Voor dit schema, sommige geldige paden zou worden (mits dat deze indexen voorbeeld zijn bereik):

    details[0].parsedStack[2].level
    details[0].message
    arraylength(details)
    arraylength(details[0].parsedStack)



### <a name="array-and-object-literals"></a>Matrix en object letterlijke waarden

Als u wilt een letterlijke dynamische maken, gebruikt u `parsejson` (alias `todynamic`) met een JSON tekenreeks van de argumenten:

* `parsejson('[43, 21, 65]')`-een matrix van getallen
* `parsejson('{"name":"Alan", "age":21, "address":{"street":432,"postcode":"JLK32P"}}')` 
* `parsejson('21')`-één waarde van het dynamische type die een getal bevat
* `parsejson('"21"')`-één waarde van het dynamische type met een tekenreeks

Opmerking dat de JSON in tegenstelling tot JavaScript, het gebruik van dubbele aanhalingstekens verplicht (`"`) tekenreeksen. Daarom is het meestal gemakkelijker naar een tekenreeks JSON-codering van letterlijke waarden met één-offertes offerte (`'`).

In dit voorbeeld een dynamische waarde maakt en gebruikt vervolgens de velden:

```

T
| extend person = parsejson('{"name":"Alan", "age":21, "address":{"street":432,"postcode":"JLK32P"}}')
| extend n = person.name, add = person.address.street
```


### <a name="dynamic-object-functions"></a>Dynamische objectfuncties

|||
|---|---|
| *waarde* `in` *matrix*| Waar als er een element van de *matrix is* die *waarde* ==<br/>`where City in ('London', 'Paris', 'Rome')`
| *waarde* `!in` *matrix*| Waar als er geen element van de *matrix is* die *waarde* ==
|[`arraylength(`matrix`)`](#arraylength)| Null-waarden als dit niet een matrix wordt
|[`extractjson(`pad, object`)`](#extractjson)|Pad gebruikt om te navigeren naar object.
|[`parsejson(`bron`)`](#parsejson)| Een tekenreeks JSON verandert in een dynamisch object.
|[`range(`uit, stap`)`](#range)| Een matrix van waarden
|[`mvexpand`listColumn](#mvexpand-operator) | Wordt overgenomen door een rij voor elke waarde in een lijst in een bepaalde cel.
|[`summarize buildschema(`kolom`)`](#buildschema) |Het type schema afleidt uit kolominhoud
|[`summarize makelist(`kolom`)`](#makelist)| Groepen rijen effent en plaatst de waarden van de kolom in een matrix.
|[`summarize makeset(`kolom`)`](#makeset) | Effent groepen rijen en plaatst de waarden van de kolom in een matrix, zonder dupliceren.

### <a name="dynamic-objects-in-let-clauses"></a>Dynamische objecten in laten componenten


[Laat componenten](#let-clause) store dynamische waarden als tekenreeksen, zodat deze twee componenten zijn equivalente en beide nodig de `parsejson` (of `todynamic`) voordat het wordt gebruikt:

    let list1 = '{"a" : "somevalue"}';
    let list2 = parsejson('{"a" : "somevalue"}');

    T | project parsejson(list1).a, parsejson(list2).a




### <a name="arraylength"></a>arraylength

Het aantal elementen in een dynamische matrix.

**Syntaxis**

    arraylength(array)

**Argumenten**

* *matrix:* A `dynamic` waarde.

**Geeft als resultaat**

Het aantal elementen in de *matrix*, of `null` als *matrix* niet een matrix is.

**Voorbeelden**

```
arraylength(parsejson('[1, 2, 3, "four"]')) == 4
arraylength(parsejson('[8]')) == 1
arraylength(parsejson('[{}]')) == 1
arraylength(parsejson('[]')) == 0
arraylength(parsejson('{}')) == null
arraylength(parsejson('21')) == null
```



### <a name="extractjson"></a>extractjson

    extractjson("$.hosts[1].AvailableMB", EventText, typeof(int))

Een opgegeven element afmelden bij een JSON-tekst met een padexpressie krijgen. De opgehaalde tekenreeks (optioneel) converteren naar een specifiek type.


**Syntaxis**

```

    string extractjson(jsonPath, dataSource) 
    resulttype extractjson(jsonPath, dataSource, typeof(resulttype))
```


**Geeft als resultaat**

Deze functie kunt u een query JsonPath uitvoeren in de gegevensbron die een geldige JSON-tekenreeks, (optioneel) die waarde converteren naar een ander type afhankelijk van het derde argument bevat.



**Voorbeeld**

De notatie [haak] en stip notatie zijn gelijk:

    ... | extend AvailableMB = extractjson("$.hosts[1].AvailableMB", EventText, typeof(int)) | ...

    ... | extend AvailableMD = extractjson("$['hosts'][1]['AvailableMB']", EventText, typeof(int)) | ...



**Tips voor betere prestaties**

* Where-componenten vóór het gebruik van toepassen`extractjson()`
* Kunt u overwegen een overeenkomst reguliere expressies met [extraheren](#extract) in plaats daarvan. Dit kan zeer veel sneller worden uitgevoerd en is van kracht als de JSON wordt geproduceerd van een sjabloon.
* Gebruik `parsejson()` als u wilt extraheren van meer dan één waarde uit de JSON.
* Overweeg om de JSON geparseerde bij opname door te declareren van het type van de kolom dynamisch.

### <a name="json-path-expressions"></a>Pad van de JSON-expressies

|||
|---|---|
|`$`|Hoofdmap object|
|`@`|Huidige object|
|`[0]`|Matrix subscript|
|`.`of`[0]` | Onderliggende|

*(We niet momenteel implementeren jokertekens, recursieniveau op te geven, Unie of segmenten.)*




### <a name="parsejson"></a>parsejson

Interpreteert een `string` als een [JSON waarde](http://json.org/)) en retourneert de waarde als `dynamic`. Is het bovenliggende over het gebruik van `extractjson()` wanneer u moet uitpakken van meer dan één element van een samengestelde JSON-object.

**Syntaxis**

    parsejson(json)

**Argumenten**

* *json:* Een document JSON.

**Geeft als resultaat**

Een object van het type `dynamic` opgegeven door de *json*.

**Voorbeeld**

In het volgende voorbeeld, wanneer `context_custom_metrics` is een `string` dat er als volgt uitziet: 

```
{"duration":{"value":118.0,"count":5.0,"min":100.0,"max":150.0,"stdDev":0.0,"sampledValue":118.0,"sum":118.0}}
```

Klik in het volgende fragment haalt de waarde van de `duration` slot in het object en vanaf dat deze twee sleuven haalt `duration.value` en  `duration.min` (`118.0` en `110.0`respectievelijk).

```AIQL
T
| ...
| extend d=parsejson(context_custom_metrics) 
| extend duration_value=d.duration.value, duration_min=d["duration"]["min"]
```



### <a name="range"></a>bereik

De `range()` functie (niet te verwarren met de `range` operator) genereert een dynamische matrix vasthouden van een reeks waarden tussenafstand.

**Syntaxis**

    range(start, stop, step)

**Argumenten**

* *starten:* De waarde van het eerste element in de resulterende matrix. 
* *stoppen:* De waarde van het laatste element in de resulterende matrix, of de kleinste waarde die groter is dan het laatste element in de resulterende matrix en binnen een geheel getal veelvoud is van *stap* *begint*.
* *stap:* Het verschil tussen twee opeenvolgende elementen van de matrix.

**Voorbeelden**

Het volgende voorbeeld retourneert `[1, 4, 7]`:

```AIQL
range(1, 8, 3)
```

Het volgende voorbeeld wordt een matrix vasthouden van alle dagen in het jaar 2015:

```AIQL

    range(datetime(2015-01-01), datetime(2015-12-31), 1d)
```

### <a name="todynamic"></a>todynamic

    todynamic('{"a":"a1", "b":["b1", "b2"]}')

Converteert een tekenreeks naar een dynamische waarde.

### <a name="treepath"></a>treepath

    treepath(dynamic_object)

Opgesomd alle pad expressies die bladeren in een dynamisch object identificeren. 

**Geeft als resultaat**

Een matrix van pad expressies.

**Voorbeelden**

    treepath(parsejson('{"a":"b", "c":123}')) 
    =>       ["['a']","['c']"]
    treepath(parsejson('{"prop1":[1,2,3,4], "prop2":"value2"}'))
    =>       ["['prop1']","['prop1'][0]","['prop2']"]
    treepath(parsejson('{"listProperty":[100,200,300,"abcde",{"x":"y"}]}'))
    =>       ["['listProperty']","['listProperty'][0]","['listProperty'][0]['x']"]

Opmerking deze "[0]" geeft aan dat de aanwezigheid van een matrix, maar geeft niet de index die wordt gebruikt door een specifiek pad.

### <a name="zip"></a>Postcode

    zip(list1, list2, ...)

Combineert een reeks lijsten op een lijst met tupels.

* `list1...`: Een lijst met waarden

**Voorbeelden**

    zip(parsejson('[1,3,5]'), parsejson('[2,4,6]'))
    => [ [1,2], [3,4], [5,6] ]

    
    zip(parsejson('[1,3,5]'), parsejson('[2,4]'))
    => [ [1,2], [3,4], [5,null] ]


### <a name="names"></a>Namen

Namen mag maximaal 1024 tekens lang zijn. Ze zijn hoofdlettergevoelig en kunnen bevatten letters, cijfers en onderstrepingstekens (`_`). 

Een naam met offerte ['... '] or [" ... "] naar het andere tekens of gebruiken van een trefwoord als een naam. Bijvoorbeeld:

```AIQL

    requests | 
    summarize  ["distinct urls"] = dcount(name) // non-alphanumerics
    by  ['where'] = client_City, // using a keyword as a name
        ['outcome!'] = success // non-alphanumerics
```


|||
|---|---|
|[' pad\\file\n\'x\''] | Gebruik \ escape-tekens|
|["d-e.=/f#\n"] | |
|[@'path\file'] | Geen Hiermee heft u - \ is letterlijke|
|[@"\now& vervolgens\"] | |
|[waar] | Een taal-trefwoord als een naam gebruiken|

[AZURE.INCLUDE [app-insights-analytics-footer](../../includes/app-insights-analytics-footer.md)]


