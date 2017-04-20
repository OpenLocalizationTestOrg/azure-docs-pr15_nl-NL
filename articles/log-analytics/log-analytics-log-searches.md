<properties
    pageTitle="Meld u zoekopdrachten in Log Analytics | Microsoft Azure"
    description="Zoekopdrachten in het logboek kunnen u om te combineren en relateren machine gegevens uit meerdere bronnen in uw omgeving."
    services="log-analytics"
    documentationCenter=""
    authors="bandersmsft"
    manager="jwhit"
    editor=""/>

<tags
    ms.service="log-analytics"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/10/2016"
    ms.author="banders"/>

# <a name="log-searches-in-log-analytics"></a>Log zoekopdrachten in Log Analytics

De basis van Log Analytics wordt de functie voor het zoeken van log waarmee u om te combineren en relateren machine gegevens uit meerdere bronnen in uw omgeving. Oplossingen zijn ook mogelijk gemaakt door log zoeken om u aan de doelstellingen weer te geven rond een bepaalde probleemgebied gedraaid.

Klik op de pagina, kunt u een query maken en vervolgens als u zoekt, u kunt de resultaten filteren met behulp van facet-besturingselementen. U kunt ook geavanceerde query's kunt transformeren, filteren en rapport maken op uw resultaten.

Algemene log zoekquery's weergeven op de meeste oplossing-pagina's. U kunt overal in de OMS-console, klikt u op tegels of Zoom op andere items details van het item bekijken via de zoekfunctie log.

In deze zelfstudie hebt we begeleiden tot en met voorbeelden voor alle de basisbeginselen wanneer u log zoeken gebruikt.

We beginnen met eenvoudige, praktische voorbeelden en klik vervolgens op deze maken zodat u inzicht in de praktijk gevallen over het gebruik van de syntaxis van de inzichten die u wilt extraheren uit de gegevens kunt ophalen.

Nadat u vertrouwd met zoektechnieken hebt, kunt u de [Log Analytics Meld zoeken verwijzing](log-analytics-search-reference.md)kunt bekijken.

## <a name="use-basic-filters"></a>Basisfilters gebruiken

Het eerste wat u moet weten is dat het eerste deel van een zoekopdracht query voordat een "|" verticale sluisteken, is altijd een *filter*. U kunt zien ervan als een WHERE-component in TSQL--bepaalt *welke* subset met gegevens om op te halen uit de OMS gegevensopslag. Zoeken in de gegevensopslag is grotendeels over het opgeven van de kenmerken van de gegevens die u extraheren, wilt zodat u natuurlijke dat een query met de WHERE-component starten wilt.

De belangrijkste filters die kunt u zijn *trefwoorden*, zoals 'fout' of 'time-out' of de naam van een computer. Dit soort eenvoudige query's retourneren doorgaans verschillende vormen van gegevens binnen de dezelfde resultatenset. Dit is omdat Log Analytics verschillende *typen* in het systeem heeft.


### <a name="to-conduct-a-simple-search"></a>Een eenvoudige zoekopdracht uitvoeren
1. Klik op **Log zoeken**in de portal OMS.  
    ![tegel voor zoeken](./media/log-analytics-log-searches/oms-overview-log-search.png)
2. Typ in het queryveld `error` en klik vervolgens op **Zoeken**.  
    ![Fout bij zoeken](./media/log-analytics-log-searches/oms-search-error.png)  
    Bijvoorbeeld: de query voor `error` geretourneerd 100.000 **gebeurtenis** records (die worden verzameld door Log-beheer), 18 **ConfigurationAlert** records (gegenereerd door configuratie Assessment) en 12 **ConfigurationChange** records (vastgelegd door de wijzigingen bijhouden) in de volgende afbeelding.   
    ![zoekresultaten](./media/log-analytics-log-searches/oms-search-results01.png)  

Deze filters zijn niet echt object typen/klassen. *Type* is alleen een label, of een eigenschap of een tekenreeks/naam/categorie, die is gekoppeld aan een deel van de gegevens. Sommige documenten in het systeem zijn gelabeld als **Type: ConfigurationAlert** en enkele worden gecodeerd als **Type: Perf**of **Type: gebeurtenis**, enzovoort. Elke zoekresultaat, document, record of vermelding wordt weergegeven de onbewerkte eigenschappen en de bijbehorende waarden voor elk van die gegevens van gegevens en kunt u deze veldnamen opgeven in het filter als u wilt ophalen van alleen de records waarvan het veld die waarde bevat.

*Type* is in feite niet meer dan een veld gemaakt waarmee alle records hebt, kunt u niet anders uit andere velden. Dit werd vastgesteld op basis van de waarde van het veld Type. Deze record heeft een andere vorm of vorm. Tussen twee haakjes, **Type = Perf**, of **Type = gebeurtenis** is ook de syntaxis van de die u nodig hebt voor meer informatie over om query's voor prestatiegegevens of gebeurtenissen.

U kunt een dubbele punt (:) of een gelijkteken (=) gebruiken na de veldnaam en vóór de waarde. **Type: gebeurtenis** en **Type = gebeurtenis** zijn equivalent in de zin, kunt u de gewenste stijl.

Ja, als het Type = Perf records bevatten een veld met de naam 'CounterName' en vervolgens kunt u een query die lijkt op `Type=Perf CounterName="% Processor Time"`.

Hiermee kunt u alleen de prestatiegegevens waar de naam van de teller prestaties "% processortijd" is.

### <a name="to-search-for-processor-time-performance-data"></a>Als u wilt zoeken voor processor performance tijdgegevens
- Typ in het veld van de query zoeken`Type=Perf CounterName="% Processor Time"`

U kunt ook specifieker en gebruiken **exemplaarnaam = _ 'Totaal'** in de query, namelijk een Windows prestatie-item. U kunt ook een facet en een andere **veldwaarde:**selecteren. Het filter wordt automatisch toegevoegd aan het filter in de query-balk. U kunt dit zien in de volgende afbeelding. Er wordt aangegeven waar u moet klikken om toe te voegen **exemplaarnaam: '_Totaal'** aan de query zonder iets te typen.

![facet zoeken](./media/log-analytics-log-searches/oms-search-facet.png)

De query wordt nu`Type=Perf CounterName="% Processor Time" InstanceName="_Total"`

In dit voorbeeld u hoeft niet te geven **Type = Perf** om dit resultaat. Omdat de velden CounterName en exemplaarnaam zich alleen voor records van het Type = Perf, de query is specifiek genoeg is om terug te keren dezelfde resultaten als het langer, vorige relatie:
```
CounterName="% Processor Time" InstanceName="_Total"
```

Dit komt omdat alle filters in de query die in *en* met elkaar worden geëvalueerd. Effectief, hoe meer velden die u toevoegt aan het criterium, krijgt u kleiner, specifieke en verfijnd resultaten.

Bijvoorbeeld: de query `Type=Event EventLog="Windows PowerShell"` is gelijk aan `Type=Event AND EventLog="Windows PowerShell"`. Het resultaat alle gebeurtenissen die zijn aangemeld en die worden verzameld uit het gebeurtenissenlogboek van Windows PowerShell. Als u een filter meerdere keren toevoegen door meermaals selecteren de dezelfde facet, en vervolgens het probleem zuiver cosmetische is--deze mogelijk onbelangrijke e-mail de zoekbalk, maar het nog steeds dezelfde resultaten resultaat omdat de impliciete operator en er altijd wordt.

U kunt de impliciete operator en gemakkelijk met behulp van een operator niet expliciet omkeren. Bijvoorbeeld:

`Type:Event NOT(EventLog:"Windows PowerShell")`of het equivalent `Type=Event EventLog!="Windows PowerShell"` rentabiliteit van alle gebeurtenissen in alle andere logboeken die niet het logboek met Windows PowerShell zijn.

Of, kunt u andere Booleaanse operator, zoals 'Of'. De volgende query resulteert records waarvoor het gebeurtenislogboek een toepassing of systeem is.

```
EventLog=Application OR EventLog=System
```

Met de bovenstaande query, krijgt u de vermeldingen voor beide logboeken in het hetzelfde resultaat.

Echter schakelt u het of laat de impliciete en op hun plaats staan, wordt klikt u vervolgens de volgende query geen produceren resultaten worden omdat er niet een gebeurtenislogboek-vermelding die bij beide logboeken hoort. Elke vermelding gebeurtenislogboek is opgenomen in slechts één van de twee logboeken.

```
EventLog=Application EventLog=System
```


## <a name="use-additional-filters"></a>Aanvullende filters gebruiken

De volgende query resulteert vermeldingen voor 2 gebeurtenislogboeken voor alle computers die gegevens hebt verzonden.

```
EventLog=Application OR EventLog=System
```

![zoekresultaten](./media/log-analytics-log-searches/oms-search-results03.png)

Selecteer een van de velden of filters beperkt query toevoegen aan een specifieke computer, met uitzondering van alle andere bestanden. De resulterende query zou er dan ongeveer als volgt te werk.

```
EventLog=Application OR EventLog=System Computer=SERVER1.contoso.com
```

Die gelijk is aan de volgende opties, vanwege de impliciete AND.

```
EventLog=Application OR EventLog=System AND Computer=SERVER1.contoso.com
```

Beide query's wordt in de volgende expliciete volgorde geëvalueerd. Opmerking de haakjes.

```
(EventLog=Application OR EventLog=System) AND Computer=SERVER1.contoso.com
```

Net als het veld gebeurtenislogboek, kunt u gegevens alleen voor een set specifieke computers ophalen door toe te voegen of. Bijvoorbeeld:

```
(EventLog=Application OR EventLog=System) AND (Computer=SERVER1.contoso.com OR Computer=SERVER2.contoso.com OR Computer=SERVER3.contoso.com)
```

Op dezelfde manier dit de volgende query resulteert **% CPU-tijd** voor alleen de geselecteerde twee computers.

```
CounterName="% Processor Time"  AND InstanceName="_Total" AND (Computer=SERVER1.contoso.com OR Computer=SERVER2.contoso.com)
```


### <a name="boolean-operators"></a>Booleaans operatoren
Met datetime en numerieke velden, kunt u zoeken naar waarden die *groter is dan*, met *lager dan*, en *kleiner dan of gelijk*. U kunt eenvoudig operatoren, zoals >, <>, =, < =,! = in de query-zoekbalk.


U kunt een specifieke gebeurtenislogboek zoeken voor een bepaalde periode. De afgelopen 24 uur wordt bijvoorbeeld weergegeven met de volgende symbool expressie.

```
EventLog=System TimeGenerated>NOW-24HOURS
```


#### <a name="to-search-using-a-boolean-operator"></a>Als u wilt zoeken met behulp van een Booleaanse operator
- Typ in het veld van de query zoeken`EventLog=System TimeGenerated>NOW-24HOURS"`  
    ![zoeken met Booleaanse waarde](./media/log-analytics-log-searches/oms-search-boolean.png)

Hoewel u het tijdsinterval grafisch bepalen kunt en de meeste tijden u misschien wilt doen die, zijn er voordelen met inbegrip van een tijdfilter rechtstreeks in de query. Bijvoorbeeld dit een prima oplossing met dashboards waar u de tijd voor elke tegel, ongeacht de *globale* tijd Gegevenskiezer op de dashboardpagina kunt negeren. Zie voor meer informatie [Belangrijk is tijd in het Dashboard](http://cloudadministrator.wordpress.com/2014/10/19/system-center-advisor-restarted-time-matters-in-dashboard-part-6/).

Houd er rekening mee dat u voor het *snijpunt* van de twee resultaten krijgt wanneer filteren op tijd, tijdsperioden: die is opgegeven in de portal OMS (S1) en de presentatie die is opgegeven in de query (S2).

![snijpunt](./media/log-analytics-log-searches/oms-search-intersection.png)

Dit betekent, als de perioden elkaar niet snijden, bijvoorbeeld in de portal OMS waar u **deze week** kiezen en in de query waarin u de **vorige week**definiëren, klik er is geen snijpunt en ontvangt u geen resultaten worden.

Vergelijkingsoperatoren gebruikt voor het veld TimeGenerated zijn ook handig zijn in andere situaties. Bijvoorbeeld, met numerieke velden.

Bijvoorbeeld doorgegeven dat de configuratie Assessment waarschuwingen de volgende ernst waarden hebben:

- 0 = informatie
- 1 = waarschuwing
- 2 = kritiek

U kunt een query voor waarschuwingen en kritieke waarschuwingen en ook uitsluiten informatieve bestanden met de volgende query:

```
Type=ConfigurationAlert  Severity>=1
```


U kunt ook bereik query's gebruiken. Dit betekent dat u kunt het begin en einde bereik van waarden in een reeks weergeven. Bijvoorbeeld desgewenst kunt u gebeurtenissen uit het gebeurtenissenlogboek van Operations Manager waar de EventID groter dan of gelijk is aan 2100 maar niet groter zijn dan 2199 is wilt vervolgens de volgende query retourneren.

```
Type=Event EventLog="Operations Manager" EventID:[2100..2199]
```


>[AZURE.NOTE] De syntaxis van de bereik moet u het scheidingsteken van de veldwaarde: dubbele punt (:) is en *niet* het gelijkteken (=). Zet de laagste en de hoogste einde van het bereik tussen vierkante haken en Scheid deze met twee punten (.).

## <a name="manipulate-search-results"></a>Zoekresultaten manipuleren

Wanneer u gegevens zoekt, gaat u uw zoekopdracht te verfijnen en hebben een goede niveau van de controle over de resultaten wilt. Wanneer de resultaten worden opgehaald, kunt u opdrachten omgezet kunt toepassen.

Opdrachten in het logboek analyse zoekacties *moet* volgen na de verticale sluisteken (|). Een filter moet altijd het eerste deel van een queryreeks. Definieert de gegevensset waarmee u werkt en vervolgens "buizen" deze resultaten in een opdracht. U kunt vervolgens de pijp extra opdrachten toe te voegen. Dit is losjes vergelijkbaar met de Windows PowerShell-pijplijn.

In het algemeen, de taal van de zoekopdracht Log Analytics wordt geprobeerd om te volgen PowerShell stijl en richtlijnen zodat u deze vergelijkbaar met de IT-professionals, en om te leren vergemakkelijken.

Opdrachten hebben namen van woorden, zodat u gemakkelijk kunt zien wat ze doen.  

### <a name="sort"></a>Sorteren

De opdracht sorteren kunt u de sorteervolgorde op een of meer velden definiëren. Zelfs als u niet, al dan niet standaard gebruikt, wordt een tijd aflopende volgorde afgedwongen. De meest recente resultaten zijn altijd boven aan de lijst met zoekresultaten. Dit betekent dat wanneer u een zoekopdracht met uitvoert `Type=Event EventID=1234` wat echt wordt uitgevoerd voor u is:

```
Type=Event EventID=1234 **| Sort TimeGenerated desc**
```

Dat is omdat dit is het type ervaring die u bekend met in Logboeken bent. Bijvoorbeeld, in de Windows-Logboeken.

U kunt sorteren gebruiken om te wijzigen van de manier waarop de resultaten worden geretourneerd. De volgende voorbeelden wordt getoond hoe dit werkt.

```
Type=Event EventID=1234 | Sort TimeGenerated asc
```

```
Type=Event EventID=1234 | Sort Computer asc
```

```
Type=Event EventID=1234 | Sort Computer asc,TimeGenerated desc
```


De eenvoudige voorbeelden hierboven wordt u opdrachten werking--ze de vorm van de resultaten die het filter geretourneerd wijzigen.

### <a name="limit-and-top"></a>Limiet en bovenkant
Een andere minder bekende opdracht is LIMIET. Limiet is een werkwoord PowerShell-achtige. Limiet is functioneel gelijk is aan de opdracht TOP. De dezelfde resultaten worden geretourneerd door de volgende query's.

```
Type=Event EventID=600 | Limit 1
```

```
Type=Event EventID=600 | Top 1
```


#### <a name="to-search-using-top"></a>Als u wilt zoeken boven gebruiken
- Typ in het veld van de query zoeken`Type=Event EventID=600 | Top 1`   
    ![Zoeken boven](./media/log-analytics-log-searches/oms-search-top.png)

In de bovenstaande afbeelding er zijn 358 duizend records met EventID = 600. De velden, aspecten en filters aan de linkerkant weergeven altijd informatie over de resultaten geretourneerd *door het filter deel* van de query, het gedeelte voordat u een sluisteken is. Het deelvenster **resultaten** retourneert alleen de meest recente 1 resultaat, omdat deze opdracht vorm en de resultaten getransformeerd.

### <a name="select"></a>Selecteer

De SELECT-opdracht lijkt veel zoals Select-Object in PowerShell. Het resultaat gefilterde resultaten die niet op mijn van hun oorspronkelijke eigenschappen. In plaats daarvan deze Hiermee selecteert u alleen de eigenschappen die u opgeeft.

#### <a name="to-run-a-search-using-the-select-command"></a>Een zoekprogramma's via de select-opdracht uitvoeren

1. Typ in zoeken `Type=Event` en klik vervolgens op **Zoeken**.
2. Klik op **+ meer weergeven** in een van de resultaten om de eigenschappen die de resultaten te bekijken.
3. Selecteer een aantal die expliciet en de query wordt gewijzigd in `Type=Event | Select Computer,EventID,RenderedDescription`.  
    ![Selecteer zoeken](./media/log-analytics-log-searches/oms-search-select.png)

Dit is de opdracht is vooral handig als u wilt zoeken uitvoer en kiest u alleen de delen van gegevens die echt van belang voor uw uitleg die geen mailserver vaak de volledige record is. Dit is ook handig als records van verschillende typen *enkele* algemene eigenschappen, maar niet *alle* van hun eigenschappen kunnen worden gebruikt. De, u kunt genereren uitvoer die meer natuurlijk op een tabel lijkt of werkt prima wanneer geëxporteerd naar een CSV-bestand en klik vervolgens massaged in Excel.



## <a name="use-the-measure-command"></a>Gebruik de opdracht maateenheid

EENHEID is een van de beste opslaan opdrachten in Log Analytics zoekopdrachten. U kunt statistische *functies* toepassen op uw gegevens en statistische resultaten die zijn gegroepeerd op een bepaald veld. Er zijn meerdere statistische functies die ondersteuning biedt voor maateenheid.

### <a name="measure-count"></a>Count() meten

De eerste statistische functie om te werken met en een van de eenvoudigste voor meer informatie over is de functie *count()* .

Resultaten van een zoekquery, zoals `Type=Event`, filters, ook wel genoemd aspecten aan de linkerkant van de lijst met zoekresultaten weergeven. De filters weergeven de verdeling van de waarden met een bepaald veld voor de resultaten in de zoekopdracht uitgevoerd.

![zoeken maateenheid tellen](./media/log-analytics-log-searches/oms-search-measure-count01.png)

Bijvoorbeeld, in de bovenstaande afbeelding ziet u het veld van de **Computer** en worden weergegeven die binnen de bijna 739 duizend gebeurtenissen in de resultaten zijn 68 unieke en afzonderlijke waarden voor het veld **Computer** in deze records. De tegel alleen ziet u de bovenste 5, welke de meest voorkomende 5 waarden die in de velden van de **Computer** zijn geschreven), gesorteerd op het aantal documenten met deze specifieke waarde in dat veld. In de afbeelding ziet u dat-tussen de gebeurtenissen die bijna 369 duizend – 90 duizend afkomstig zijn van de computer OpsInsights04.contoso.com, 83 duizend vanaf de computer DB03.contoso.com, enzovoort.


Wat gebeurt er als u wilt zien van alle waarden, aangezien de tegel alleen alleen de belangrijkste 5 toont?

Dit is wat de opdracht maateenheid kunt doen met de functie count(). Deze functie niet alle parameters gebruiken. U opgeven alleen het veld waarop u wilt groeperen: het veld **Computer** in dit geval:

`Type=Event | Measure count() by Computer`

![zoeken maateenheid tellen](./media/log-analytics-log-searches/oms-search-measure-count-computer.png)

**Computer** is echter alleen een veld dat wordt gebruikt *in* elk deel van gegevens: Er zijn geen relationele databases betrokken en er is geen afzonderlijke **Computer** object waar u ook bent. Alleen de waarden *in* de gegevens kunt beschrijven welke entiteit gegenereerd ze, en een aantal andere kenmerken en aspecten van de gegevens, dus de term *facet*. U kunt echter net zo goed door andere velden te groeperen. Omdat de oorspronkelijke resultaten van bijna 739 duizend gebeurtenissen die zijn omgeleide naar de opdracht maateenheid ook een veld met de naam van **EventID hebben**, kunt u dezelfde methode om te groeperen op dat veld en een aantal gebeurtenissen per EventID toepassen:

```
Type=Event | Measure count() by EventID
```

Als u niet geïnteresseerd bent in het aantal werkelijke records die een bepaalde waarde bevatten, maar in plaats daarvan als u alleen een lijst met de waarden zelf wilt, u een *Selecteer* opdracht aan het einde van deze en alleen toevoegen kunt selecteert u de eerste kolom:

```
Type=Event | Measure count() by EventID | Select EventID
```

U kunt meer geavanceerde en sorteer vooraf de resultaten in de query of u kunt alleen de kolommen in het raster te klikken.

```
Type=Event | Measure count() by EventID | Select EventID | Sort EventID asc
```

#### <a name="to-search-using-measure-count"></a>Als u wilt zoeken met maateenheid tellen

- Typ in het veld van de query zoeken`Type=Event | Measure count() by EventID`
- Toevoegen `| Select EventID` naar het einde van de query.
- Tot slot toevoegen `| Sort EventID asc` naar het einde van de query.


Er zijn enkele belangrijke punten ziet en benadrukken:

Eerst zijn de weergegeven resultaten niet de oorspronkelijke onbewerkte resultaten meer. In plaats daarvan deze samengevoegde resultaten – zijn in principe groepen met resultaten. Dit geen probleem, maar u moet kennen dat u bent interactie met een sterk afwijkt shape met gegevens die verschilt van de oorspronkelijke onbewerkte vorm die al doende grond van de aggregatie/statistische functie wordt gemaakt.

Tweede, retourneert **maateenheid tellen** momenteel alleen de bovenste 100 afzonderlijke resultaten. Deze beperking geldt niet voor de andere statistische functies. Zo is, moet u meestal eerst een nauwkeuriger filter gebruiken om te zoeken naar specifieke items voordat u de maateenheid count() toepast.

## <a name="use-the-max-and-min-functions-with-the-measure-command"></a>Gebruik van de functies max en min met de opdracht maateenheid

Er zijn verschillende scenario's waarbij **Maateenheid Max()** en **Maateenheid zijn** handig zijn. Echter, omdat elke functie tegenovergestelde van elkaar we Max() wordt illustreren en kunt u experimenteren met zijn op uw eigen.

Als u een query voor beveiligingsgebeurtenissen uitvoeren, hebben ze een **niveau** -eigenschap kan variëren. Bijvoorbeeld:

```
Type=SecurityEvent
```

![zoeken maateenheid tellen starten](./media/log-analytics-log-searches/oms-search-measure-max01.png)

Als u wilt weergeven van de hoogste waarde voor de beveiliging alle kunt gebeurtenissen gegeven een gemeenschappelijke Computer, de group by-veld, u

```
Type=ConfigurationAlert | Measure Max(Level) by Computer
```

![zoeken maateenheid max computer](./media/log-analytics-log-searches/oms-search-measure-max02.png)

Deze wordt weergegeven dat voor de computers waarop bijvoorbeeld **niveau** records, meestal ze hebben ten minste 8 effenen, zoals veel een niveau van 16.

```
Type=ConfigurationAlert | Measure Max(Severity) by Computer
```

![zoeken maateenheid maximum aantal keer gegenereerd computer](./media/log-analytics-log-searches/oms-search-measure-max03.png)

Deze functie werkt ook met de getallen, maar deze werkt ook met de DateTime-velden. Het is handig om het laatste of de meest recente tijdstempel van een tekstgedeelte gegevens geïndexeerde voor elke computer te controleren. Bijvoorbeeld: wanneer is de meest recente beveiligingsgebeurtenis gemeld voor elke computer?

```
Type=ConfigurationChange | Measure Max(TimeGenerated) by Computer
```

## <a name="use-the-avg-function-with-the-measure-command"></a>Gebruik de functie avg met de opdracht maateenheid

De Avg() statistische functie gebruikt met maateenheid kunt u de gemiddelde waarde voor een veld, en de Groepsresultaten op het veld dezelfde of een andere berekenen. Dit is handig in een aantal zaken, zoals prestatiegegevens.

We beginnen met prestatiegegevens. Houd er rekening mee dat OMS momenteel prestatie-items voor zowel Windows en Linux machines verzamelt.

Als u wilt zoeken naar *alle* prestatiegegevens, is de eenvoudigste query:

```
Type=Perf
```

![avg start zoeken](./media/log-analytics-log-searches/oms-search-avg01.png)

Het eerste wat u ziet is dat Log Analytics u drie perspectieven ziet: lijst waarin u waarin de werkelijke records achter de grafieken; Tabel waarin een tabelweergave prestaties itemgegevens; aan de doelstellingen, geeft en grafieken voor de items.

Er zijn twee sets van velden gemarkeerd die het volgende aangeven in de bovenstaande afbeelding:

- De eerste set identificeert Windows prestaties Itemnaam, de naam van het Object en exemplaarnaam in het queryfilter. Dit zijn de velden die u waarschijnlijk meestal als aspecten/filters gebruikt wordt
- **Tegenwaarde** is de werkelijke waarde van het item. In dit voorbeeld wordt de waarde *75*.
- **TimeGenerated** is 12:51, in de 24-uursnotatie.

Hier ziet u een weergave van de parameters in een grafiek.

![avg start zoeken](./media/log-analytics-log-searches/oms-search-avg02.png)

Na het lezen over het opnemen shape Perf en lees informatie over andere zoektechnieken, kunt u maateenheid Avg() dit type numerieke gegevens aggregeren.

Hier volgt een eenvoudig voorbeeld:

```
Type=Perf  ObjectName:Processor  InstanceName:_Total  CounterName:"% Processor Time" | Measure Avg(CounterValue) by Computer
```

![avg samplevalue zoeken](./media/log-analytics-log-searches/oms-search-avg03.png)

In dit voorbeeld selecteert u de totaaltijd CPU-prestaties teller en gemiddelde door Computer. Als u uw resultaten om alleen de laatste 6 uur te beperken wilt, kunt u het besturingselement van het filter tijd gebruiken of opgeven in uw query als volgt:

```
Type=Perf  ObjectName:Processor  InstanceName:_Total  CounterName:"% Processor Time" TimeGenerated>NOW-6HOURS | Measure Avg(CounterValue) by Computer
```

### <a name="to-search-using-the-avg-function-with-the-measure-command"></a>Als u wilt zoeken met de functie avg met de opdracht maateenheid
- Typ in het vak Zoeken query `Type=Perf  ObjectName:Processor  InstanceName:_Total  CounterName:"% Processor Time" TimeGenerated>NOW-6HOURS | Measure Avg(CounterValue) by Computer`.


U kunt aggregeren en gegevens *over* computers relateren. Stel dat er een set hosts in sommige sorteren van farm waarbij elk knooppunt is gelijk aan andere een ze alleen dezelfde van werk en typ laden ongeveer moet worden verdeeld. U kunt de items die al in een gaat u met de volgende query en gemiddelden krijgen voor de hele farm kan ophalen. U kunt starten door te kiezen de computers met het volgende voorbeeld:

```
Type=Perf AND (Computer="AzureMktg01" OR Computer="AzureMktg02" OR Computer="AzureMktg03")
```

Nu dat u de computers hebt, u ook alleen wilt selecteren, twee key performance indicators (KPI's): % CPU-gebruik en % vrije schijfruimte. Zo is, wordt dit gedeelte van de query wordt:

```
Type=Perf InstanceName:_Total  ((ObjectName:Processor AND CounterName:"% Processor Time") OR (ObjectName="LogicalDisk" AND CounterName="% Free Space")) AND TimeGenerated>NOW-4HOURS
```

U kunt nu computers en items met het volgende voorbeeld toevoegen:

```
Type=Perf InstanceName:_Total  ((ObjectName:Processor AND CounterName:"% Processor Time") OR (ObjectName="LogicalDisk" AND CounterName="% Free Space")) AND TimeGenerated>NOW-4HOURS AND (Computer="AzureMktg01" OR Computer="AzureMktg02" OR Computer="AzureMktg03")
```

Omdat er een zeer specifieke selectie, kan de opdracht **maateenheid Avg()** retourneren het gemiddelde niet door de computer, maar in de farm, door gewoon te groeperen door CounterName. Bijvoorbeeld:

```
Type=Perf  InstanceName:_Total  ((ObjectName:Processor AND CounterName:"% Processor Time") OR (ObjectName="LogicalDisk" AND CounterName="% Free Space")) AND TimeGenerated>NOW-4HOURS AND (Computer="AzureMktg01" OR Computer="AzureMktg02" OR Computer="AzureMktg03") | Measure Avg(CounterValue) by CounterName
```

Het resultaat is een handige compacte weergave van een aantal KPI's van uw omgeving.

![zoeken avg groeperen](./media/log-analytics-log-searches/oms-search-avg04.png)


U kunt eenvoudig de query in een dashboard. U kunt bijvoorbeeld de zoekopdracht opslaan en een dashboard maken vanuit het *Web Farm KPI's*met de naam. Zie meer informatie over het gebruik van dashboards [maken een aangepaste dashboard in Log Analytics](log-analytics-dashboards.md).

![zoeken avg dashboard](./media/log-analytics-log-searches/oms-search-avg05.png)

### <a name="use-the-sum-function-with-the-measure-command"></a>De functie SOM gebruiken met de opdracht maateenheid

De functie SOM is vergelijkbaar met andere functies van de opdracht maateenheid. Hier ziet u een voorbeeld over het gebruik van de functie SOM [W3C IIS logboeken](http://blogs.msdn.com/b/dmuscett/archive/2014/09/20/w3c-iis-logs-search-in-system-center-advisor-limited-preview.aspx)zoeken in Microsoft Azure operationele inzichten.

U kunt Max() en zijn gebruiken met getallen, datums en tijden en tekenreeksen. Met tekenreeksen, ze alfabetisch worden gesorteerd en u eerste en laatste.

U kunt bladwijzers echter niet gebruiken met andere naam heeft dan numerieke velden. Dit geldt ook voor Avg().

### <a name="use-the-percentile-function-with-the-measure-command"></a>Gebruik de functie percentiel met de opdracht maateenheid

De functie percentiel is vergelijkbaar met Avg() of bladwijzers in dat u deze alleen voor numerieke velden kunt gebruiken. U kunt een percentiel tussen 1 en 99 op een numeriek veld. U kunt ook zowel **percentiel** als **pct** opdrachten gebruiken. Hier volgen enkele voorbeelden:  

```
Type:Perf CounterName:"DiskTransers/sec" |measure percentile95(CurrentValue) by Computer
```
```
Type:Perf ObjectName=LogicalDisk CounterName="Current Disk Queue Length" Computer="MyComputerName" | measure pct65(CurrentValue) by InstanceName
```

## <a name="use-the-where-command"></a>Gebruik de where opdracht

De waar opdracht zoals een filter werkt, maar deze kan worden toegepast in de pijplijn verder geaggregeerde om resultaten te filteren dat is geproduceerd door een maateenheid voor de opdracht – in plaats van onbewerkte resultaten die zijn gefilterd aan het begin van een query.

Bijvoorbeeld:

```
Type=Perf  CounterName="% Processor Time"  InstanceName="_Total" | Measure Avg(CounterValue) as AVGCPU by Computer
```

U kunt een ander recht streepje toevoegen "|" teken en de opdracht om alleen computers waarvan gemiddelde CPU groter dan 80%, met het volgende voorbeeld is waar:

```
Type=Perf  CounterName="% Processor Time"  InstanceName="_Total" | Measure Avg(CounterValue) as AVGCPU by Computer | Where AVGCPU>80
```

Als u bekend met Microsoft System Center - Operations Manager bent, kunt u zien waar u de opdracht management pack genoemd. Als het voorbeeld een regel zijn, het eerste deel van de query zou de gegevensbron en waar u de opdracht zou de voorwaarde-detectie.

U kunt de query gebruiken als een tegel in **Mijn Dashboard**, als een monitor sorteerniveaus, om te zien wanneer computer CPU's zijn overbezette. Zie meer informatie over dashboards [maken een aangepaste dashboard in Log Analytics](log-analytics-dashboards.md). U kunt ook maken en gebruiken van dashboards maken met de mobiele app. Zie [OMS Mobile-App ](http://www.windowsphone.com/en-us/store/app/operational-insights/4823b935-83ce-466c-82bb-bd0a3f58d865)voor meer informatie. In de tegels onder twee van de volgende afbeelding ziet u het beeldscherm waarmee een lijst weergegeven en als een getal. In principe, wilt u altijd het getal nul en de lijst leeg. Anders wordt de voorwaarde voor een waarschuwing aangegeven. Indien nodig kunt u deze bekijken op welke computers onder druk zijn.

![mobiele dashboard](./media/log-analytics-log-searches/oms-search-mobile.png)

## <a name="use-the-in-operator"></a>Gebruik de operator in

De operator *IN* samen met *NOT IN* kunt u subsearches, welke zoekopdracht met een andere zoekopdracht als argument gebruiken. Ze zijn opgenomen in de accolades binnen een andere *primaire* of *outer* zoeken. Het resultaat van een subsearch, vaak een lijst met verschillende resultaten, wordt klikt u vervolgens als een argument in de primaire zoeken gebruikt.

U kunt subsearches gebruiken om aan te passen subsets van uw gegevens dat u kunt geen beschrijven rechtstreeks in een zoekexpressie, maar die kan worden gegenereerd op basis van een zoekopdracht. Bijvoorbeeld als u geïnteresseerd bent in één zoeken gebruiken om te vinden van alle gebeurtenissen van *computers ontbrekende beveiligingsupdates*, moet u bij het ontwerpen van een subsearch waarmee eerst worden geïdentificeerd die *computers ontbrekende beveiligingsupdates* voordat er gebeurtenissen die deel uitmaken van die hosts.

Ja, kan u uitdrukken *computers ontbrekende momenteel vereiste beveiligingsupdates* met de volgende query:

```
Type:Update UpdateState=Needed Optional=false Classification="Security Updates" TimeGenerated>NOW-24HOURS | measure count() by Computer
```    

![Klik IN het voorbeeld van zoeken](./media/log-analytics-log-searches/oms-search-in01-revised.png)

Nadat u de lijst hebt, kunt u de zoekopdracht als een inner zoeken naar de lijst met computers ontstaan in een buitenste (primair) zoeken, dat voor gebeurtenissen voor die computers eruitziet. U doet dit door als u de binnenste zoekopdracht accolades en de resultaten invoeropties als mogelijke waarden voor een filterveld in de buitenste zoekprogramma's via de IN-operator. De query er als zou uitzien:

```
Type=Event Computer IN {Type:Update UpdateState=Needed Optional=false Classification="Security Updates" TimeGenerated>NOW-24HOURS | measure count() by Computer}
```
![Klik IN het voorbeeld van zoeken](./media/log-analytics-log-searches/oms-search-in02-revised.png)


Ook kennisgeving het tijdfilter gebruikt in het binnenste zoeken omdat het systeem Update Assessment een momentopname van alle computers elke 24 uur maakt. U kunt de binnenste query meer licht en nauwkeurig aanbrengen door alleen zoeken naar een dag. De buitenste zoeken in plaats daarvan wordt gebruikt de tijdzone in de gebruikersinterface van gebeurtenissen ophalen van de laatste zeven dagen. Zie [Booleaans operatoren](#boolean-operators) voor meer informatie over tijd operatoren.

Omdat u echt alleen gebruiken de resultaten van de binnenste zoekopdracht als filter waarde voor de buitenste sneltoets, kunt u nog steeds opdrachten in de buitenste zoekopdracht toepassen. U kunt bijvoorbeeld nog steeds de bovenstaande gebeurtenissen met een andere maateenheid voor de opdracht groeperen:

```
Type=Event Computer IN {Type:Update UpdateState=Needed Optional=false Classification="Security Updates" TimeGenerated>NOW-24HOURS | measure count() by Computer} | measure count() by Source
```

![Klik IN het voorbeeld van zoeken](./media/log-analytics-log-searches/oms-search-in03-revised.png)


U wilt in het algemeen wordt de binnenste query snel wordt uitgevoerd, omdat Log Analytics-service aan de clientzijde time-out voor deze heeft en ook in een kleine hoeveelheid resultaten te retourneren. Als de binnenste query meer resultaten worden geretourneerd, de lijst met resultaten afgekapt, waarin zou kunnen veroorzaken met de buitenste zoekopdracht tot onjuiste resultaten worden geretourneerd.

Een andere regel is dat de binnenste zoekopdracht momenteel hoeft op te geven van de *samengevoegde* resultaten. Met andere woorden, moet de opdracht van een *maateenheid* ; bevatten u kunt geen onbewerkte resultaten momenteel feed in een buitenste zoeken naar.

Daarnaast kunnen er slechts één IN-operator en moet dat het laatste filter in de query. Meerdere IN operatoren kunnen niet worden of zou – zo in wezen voorkomt u dat meerdere subsearches uitvoeren: het belangrijkste is dat slechts één sub/binnenste tijdens het zoeken is mogelijk voor elke outer zoeken.

IN veel soorten gecorreleerde zoekopdrachten kan ook niet met deze limieten en Hiermee kunt u opgeven of een vergelijkbare groepen zoals computers, gebruikers of bestanden – ongeacht de velden in uw gegevens bevatten. Hier vindt u meer voorbeelden:

**Alle updates ontbreken in computers waarop de instelling voor automatische updates is uitgeschakeld**

```
Type=Update UpdateState=Needed Optional=false Computer IN {Type=UpdateSummary WindowsUpdateSetting=Manual | measure count() by Computer} | measure count() by KBID
```

**Alle fouten uit computers met SQL Server (= waar SQL beoordeling is uitgevoerd)**

```
Type=Event EventLevelName=error Computer IN {Type=SQLAssessmentRecommendation | measure count() by Computer}
```

**Alle beveiligingsgebeurtenissen van computers die domeincontrollers zijn (= waar AD beoordeling is uitgevoerd)**

```
Type=SecurityEvent Computer IN { Type=ADAssessmentRecommendation | measure count() by Computer }
```

**Welke andere accounts hebt aangemeld bij dezelfde computers waarop account BACONLAND\jochan heeft aangemeld?**

```
Type=SecurityEvent EventID=4624   Account!="BACONLAND\\jochan" Computer IN { Type=SecurityEvent EventID=4624   Account="BACONLAND\\jochan" | measure count() by Computer } | measure count() by Account
```

## <a name="use-the-distinct-command"></a>Gebruik de opdracht distinct

De naam geeft al, biedt deze opdracht een lijst met unieke waarden voor een veld. Verrassend eenvoudige, maar heel handig is. Hetzelfde kan worden bereikt met maateenheid count() opdracht ook, zoals hieronder wordt weergegeven.

```
Type=Event | Measure count() by Computer
```

![Voorbeeld van de opdracht DISTINCT zoeken](./media/log-analytics-log-searches/oms-search-distinct01-revised.png)

Als u geïnteresseerd bent is echter slechts een lijst met unieke waarden en niet het aantal documenten met waarden en klik vervolgens DISTINCT kan bieden doorzichtiger en beter leesbaar uitvoer en kortere standaardsyntaxis, zoals hieronder wordt weergegeven.

```
Type=Event | Distinct Computer
```
![Voorbeeld van de opdracht DISTINCT zoeken](./media/log-analytics-log-searches/oms-search-distinct02-revised.png)

## <a name="use-the-countdistinct-function-with-the-measure-command"></a>Gebruik de functie countdistinct met de opdracht maateenheid
De functie countdistinct telt het aantal unieke waarden binnen elke groep. Dit kan bijvoorbeeld het aantal unieke computers voor rapportage voor elk Type worden gebruikt:

```
* | measure countdistinct(Computer) by Type
```

![OMS-countdistinct](./media/log-analytics-log-searches/oms-countdistinct.png)

## <a name="use-the-measure-interval-command"></a>Gebruik de maateenheid interval-opdracht
Met kunt Realtime prestatiegegevens verzamelen, u verzamelen en visualiseren van elk prestatiemeteritem in Log Analytics. De query **Type: Perf** duizenden metrische grafieken retourneert eenvoudig in te voeren op basis van het aantal items en servers in uw omgeving Log Analytics. Met op aanvraag metrische aggregatie, kunt u de doelstellingen van de algehele in uw omgeving voor een hoog niveau en uitgebreide kennismaking op meer gedetailleerde gegevens dat u wilt bekijken.

Stel dat u wilt weten wat is de gemiddelde CPU op al uw computers. Kijken naar het gemiddelde CPU voor elke computer mogelijk niet handig omdat resultaten mogelijk krijgen vloeiend moeten worden gemaakt. Als u wilt zoeken naar meer informatie, kunt u het resultaat in een kleinere tijd venster segmenten gedownload en kijkt u naar een tijdreeks aggregeren over verschillende dimensies. Bijvoorbeeld: u kunt uitvoeren het per uur gemiddelde van CPU-gebruik al alle uw computers als volgt:

```
Type:Perf CounterName="% Processor Time" InstanceName="_Total" | measure avg(CounterValue) by Computer Interval 1HOUR
```

![de gemiddelde interval maateenheid](./media/log-analytics-log-searches/oms-measure-avg-interval.png)

Standaard wordt deze resultaten worden weergegeven in een lijndiagram met meerdere reeksen interactieve.  Deze grafiek ondersteunt reeks schakelen (met de y-as schaling aanpassen), zoomen en aanwijzen.  De tabel weergaveoptie is nog steeds beschikbaar is voor het weergeven van de onbewerkte gegevens zo nodig.

U kunt ook andere velden groeperen. In dit voorbeeld ik zoek op de % items voor specifieke computers en ik wil weten wat het percentielen per uur 70 van elk item is:

```
Type:Perf Computer=beefpatty4 CounterName=%* InstanceName=_Total | measure percentile70(CounterValue) by CounterName Interval 1HOUR
```
Hierbij moet u onthouden is deze query's worden niet beperkt tot prestatie-items. U kunt deze toepassen op een meetwaarde. In dit voorbeeld zoek ik op de W3C IIS-logboeken. Ik wil weten wat is de maximale tijd die het duurt voor een interval 5 minuten voor het verwerken van elk verzoek om:

```
Type:W3CIISLog | measure max(TimeTaken) by csMethod Interval 5MINUTES
```

### <a name="use-multiple-aggregates-in-one-query"></a>Meerdere aggregaties in een query gebruiken
U kunt meerdere statistische componenten opgeven in een maateenheid voor de opdracht.  Elk kan afzonderlijk alias zijn.  Er is geen opgegeven een alias is de naam van het resulterende veld als de statistische functie die is gebruikt (dat wil zeggen "avg(CounterValue)" voor avg(CounterValue)).

 ```
Type=WireData | measure avg(ReceivedBytes), avg(SentBytes) by Direction interval 1hour
```
![OMS-multiaggregates1](./media/log-analytics-log-searches/oms-multiaggregates1.png)

Hier volgt een ander voorbeeld:
 ```
* | measure countdistinct(Computer) as Computers, count() as TotalRecords by Type
```


## <a name="next-steps"></a>Volgende stappen

Zie voor meer informatie over log zoekopdrachten:

- [Aangepaste velden in Log Analytics](log-analytics-custom-fields.md) gebruiken om uit te breiden log zoekopdrachten.
- Bekijk het [logboek Analytics zoeken verwijzing Meld u aan](log-analytics-search-reference.md) om weer te geven op alle zoekvelden en aspecten beschikbaar in Log Analytics.
