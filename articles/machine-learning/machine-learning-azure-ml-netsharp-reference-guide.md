<properties 
    pageTitle="Handleiding voor de Net # Neural netwerken specificatietaal | Microsoft Azure" 
    description="Syntaxis voor de Net # neural netwerken specificatietaal, samen met voorbeelden over het maken van een aangepaste neural netwerk-model in Microsoft Azure ML Net met#" 
    services="machine-learning" 
    documentationCenter="" 
    authors="jeannt" 
    manager="jhubbard" 
    editor="cgronlun"/>

<tags 
    ms.service="machine-learning" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/12/2016" 
    ms.author="jeannt"/>



# <a name="guide-to-net-neural-network-specification-language-for-azure-machine-learning"></a>Gids voor Net # neural netwerk specificatietaal voor Azure Machine Learning

## <a name="overview"></a>Overzicht
NET # is een taal die zijn ontwikkeld door Microsoft die wordt gebruikt om te definiëren neural netwerkarchitecturen voor neural netwerk modules in Microsoft Azure Machine Learning. In dit artikel leert u het:  

-   Basisbegrippen voor neural netwerken
-   Neural netwerkvereisten en het definiëren van de primaire onderdelen
-   De syntaxis en het trefwoorden van de taal van de Net # specificatie
-   Voorbeelden van aangepaste neural netwerken die zijn gemaakt met Net# 
    
[AZURE.INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]  

## <a name="neural-network-basics"></a>Basisbeginselen van neural netwerk
De netwerkstructuur van een neural bestaat uit ***knooppunten*** die zijn ingedeeld in ***lagen***, en gewogen ***verbindingen*** (of ***randen***) tussen de knooppunten. De verbindingen zijn directionele en elke verbinding heeft een ***bron*** en een knooppunten ***bestemming*** .  

Elke ***trainable laag*** (een verborgen of een laag uitvoer) heeft een of meer ***verbinding pakketten***. Een verbinding bundel bestaat uit een bronlaag en een specificatie van de verbindingen vanuit deze bronlaag. Alle verbindingen in een bepaald bundel delen dezelfde ***bronlaag*** en dezelfde ***bestemming laag***. In Net #, wordt beschouwd als een bundel verbinding bij van de bundel bestemming laag.  
 
NET # ondersteunt diverse soorten verbinding pakketten, waarmee u de aanpassen van de invoer manier zijn toegewezen aan verborgen lagen en toegewezen aan de uitvoer van.   

De standaard- of standaard bundel is een **volledige bundel**, waarbij elk knooppunt in de bronlaag is verbonden met elk knooppunt in de laag bestemming.  

Daarnaast kunnen ondersteunt Net # de volgende vier typen geavanceerde verbinding-pakketten:  

-   **Gefilterde pakketten**. De gebruiker kan een predikaat definiëren met behulp van de locaties van de bron laag en de bestemming laag knooppunten. Knooppunten zijn verbonden wanneer het predikaat waar is.
-   **Convolutional pakketten**. De gebruiker kunt kleine groepen van knooppunten in de bronlaag definiëren. Elk knooppunt in de laag bestemming is verbonden met een groep van knooppunten in de bronlaag.
-   **Groepsgewijze pakketten** en **reactie normaliseren pakketten**. Dit zijn vergelijkbaar met convolutional pakketten in dat de gebruiker kleine groepen van knooppunten in de bronlaag definieert. Het verschil is dat het gewicht van de randen in deze pakketten niet trainable zijn. In plaats daarvan wordt een vooraf gedefinieerde functie toegepast op de bronwaarden voor knooppunt om te bepalen de waarde van bestemming knooppunt.  

Definiëren met behulp van Net # de structuur van een neural netwerk, maakt het mogelijk om te definiëren complexe structuren zoals diepblauwe neural-netwerken te gebruiken of convoluties willekeurige dimensies bekend zijn om te leren op gegevens zoals afbeelding, audio of video te verbeteren.  

## <a name="supported-customizations"></a>Ondersteunde aanpassingen
De architectuur van neural netwerk-modellen die u in Azure Machine Learning maakt kan uitgebreid met behulp van Net # worden aangepast. U kunt:  

-   Verborgen lagen maken en beheren van het aantal knooppunten in elke laag.
-   Opgeven hoe lagen met elkaar worden verbonden.
-   Speciale connectivity structuren, zoals convoluties en dikte delen pakketten definiëren.
-   Geef de activering van de verschillende functies.  

Zie voor meer informatie van de syntaxis van de taal specificatie [Structuur specificatie](#Structure-specifications).  
 
Zie [voorbeelden](#Examples-of-Net#-usage)voor voorbeelden van neural netwerken voor enkele veelgebruikte machine learning-taken, uit Simplex () aan complexe, definieert.  

## <a name="general-requirements"></a>Algemene vereisten
-   Moet er precies één uitvoer laag, ten minste één invoer laag en nul of meer verborgen lagen. 
-   Elke laag heeft een vast aantal knooppunten, conceptueel in een rechthoekig matrix met willekeurige afmetingen worden gerangschikt. 
-   Invoer lagen hebben geen bijbehorende ervaren parameters en vertegenwoordigen het punt waar exemplaargegevens invoert in het netwerk. 
-   Trainable lagen (de verborgen- en uitvoerbereik lagen) hebt ervaren parameters, bekend als systematische en lijndikten gekoppeld. 
-   De bron- en doeltabellen knooppunten moeten zich in verschillende lagen. 
-   Verbindingen moet acyclische; Er kunnen niet met andere woorden, een reeks verbindingen voorloop terug naar het bronknooppunt oorspronkelijke.
-   De uitvoer laag mogen niet een bronlaag van een verbinding bundel.  

## <a name="structure-specifications"></a>Specificaties voor structuur
Een specificatie van de structuur neural netwerk bestaat uit drie secties: de **declaratie van de constante**, de **laag declaratie**, de **declaratie van verbinding**. Er is ook een sectie optioneel **declaratie delen** . De secties kunnen worden opgegeven in een willekeurige volgorde.  

## <a name="constant-declaration"></a>Declaratie van de constante 
Een constante declaratie is optioneel. Deze biedt een manier om het definiëren van de waarden die ergens anders in de definitie neural netwerk worden gebruikt. De declaratie-instructie bestaat uit een id gevolgd door een gelijkteken (=) en een waardenexpressie.   

De volgende instructie wordt bijvoorbeeld een constante **x**gedefinieerd:  


    Const X = 28;  

Als u twee of meer constanten tegelijk definieert, moet u de id-namen en waarden tussen accolades en Scheid deze met behulp van puntkomma's. Bijvoorbeeld:  

    Const { X = 28; Y = 4; }  

De rechterkant van de toewijzingsexpressie voor elke is een geheel getal, een reëel getal, een Booleaanse waarde (True of False) of een wiskundige uitdrukking. Bijvoorbeeld:  

    Const { X = 17 * 2; Y = true; }  

## <a name="layer-declaration"></a>Laag declaratie
De declaratie laag is vereist. Deze Hiermee definieert u de grootte en de bron van de laag, waaronder de verbinding pakketten en kenmerken. De declaratie-instructie begint met de naam van de laag (input, verborgen of uitvoer), gevolgd door de afmetingen van de laag (een tupel van positieve getallen). Bijvoorbeeld:  

    input Data auto;
    hidden Hidden[5,20] from Data all;
    output Result[2] from Hidden all;  

-   Het product van de afmetingen is het aantal knooppunten in de laag. In dit voorbeeld zijn er twee dimensies [5,20], wat betekent dat er 100 knooppunten in de laag.
-   De lagen kunnen zijn opgegeven in een willekeurige volgorde, klikt u met één uitzondering: als meer dan één invoer laag is gedefinieerd, de volgorde van functies in de invoergegevens moet overeenkomen met de volgorde waarin ze zijn gedeclareerd.  


Als u wilt opgeven dat het aantal knooppunten in een laag automatisch worden bepaald, gebruikt u het trefwoord **automatisch** . Het trefwoord **automatisch** heeft verschillende effecten, afhankelijk van de laag:  

-   In een verklaring invoer laag is het aantal knooppunten het aantal onderdelen in de invoergegevens.
-   In een verklaring verborgen laag is het aantal knooppunten het nummer dat is opgegeven door de parameterwaarde voor **aantal verborgen knooppunten**. 
-   In een declaratie uitvoer laag is het aantal knooppunten 2 voor twee-klasse-indeling, 1 voor regressie en gelijk aan het aantal knooppunten uitvoer voor multiclass indeling.   

De definitie van de volgende netwerk kunt bijvoorbeeld de grootte van alle lagen automatisch wordt bepaald:  

    input Data auto;
    hidden Hidden auto from Data all;
    output Result auto from Hidden all;  


Een laag declaratie voor een trainable laag (de verborgen of uitvoerbereik lagen) kunt desgewenst de uitvoer, functie (ook wel een functie activering genoemd), namelijk **sigmoid** voor classificatie-modellen en **lineaire** voor regressie-modellen. (Zelfs als u de standaard gebruikt, u kunt expliciet aangeven de functie activering desgewenst voor duidelijkheid.)

De volgende uitvoer-functies worden ondersteund:  

-   sigmoid
-   lineaire
-   softmax
-   rlinear
-   vierkante
-   WORTEL
-   srlinear
-   ABS
-   TANH 
-   brlinear  

De volgende declaratie wordt bijvoorbeeld de functie **softmax** gebruikt:  

    output Result [100] softmax from Hidden all;  

## <a name="connection-declaration"></a>Verbinding declaratie
Zodra u definieert de trainable laag, moet u verbindingen tussen de lagen die u hebt gedefinieerd declareren. De verbinding bundel declaratie begint met het trefwoord **uit**, gevolgd door de naam van de bundel bronlaag-en het soort bundel verbinding te maken.   

Op dit moment voorwaarden vijf soorten verbinding pakketten wordt voldaan:  

-   **Volledige** pakketten, aangegeven door het trefwoord **alle**
-   **Gefilterde** pakketten, aangegeven door het trefwoord **waar**, gevolgd door een predikaatfunctie expressie
-   **Convolutional** -pakketten, aangegeven door het trefwoord **convolve**, gevolgd door de kenmerken Convolutie
-   **Pooling** -pakketten, aangegeven door de trefwoorden **max van toepassingen** of het **gemiddelde van de groep**
-   **Antwoord normaliseren** -pakketten, aangegeven door het trefwoord **antwoord norm**      

## <a name="full-bundles"></a>Volledige pakketten  

Een volledige verbinding bundel bevat een verbinding uit elk knooppunt in de bronlaag voor elk knooppunt in de laag bestemming. Dit is het type standaard netwerkverbinding.  

## <a name="filtered-bundles"></a>Gefilterde pakketten
Een gefilterde verbinding bundel specificatie bevat een veel predikaatfunctie, uitgedrukt syntaxis, zoals een C# lambda-expressie. Het volgende voorbeeld worden twee gefilterde pakketten gedefinieerd:  

    input Pixels [10, 20];
    hidden ByRow[10, 12] from Pixels where (s,d) => s[0] == d[0];
    hidden ByCol[5, 20] from Pixels where (s,d) => abs(s[1] - d[1]) <= 1;  

-   In het predicaat voor _ByRow_, **s** is een parameter dat staat voor een index in de rechthoekige matrix van knooppunten van de invoer laag, _Pixels_, en **d** is een parameter die een index in de matrix van knooppunten van de verborgen laag, _ByRow_vertegenwoordigt. Het type van zowel **s** en **d** is een tupel van gehele getallen van twee lengte. Conceptueel, **s** bereiken via alle paren van gehele getallen met _0 < = s [0] < 10_ en _0 < = s[1] < 20_, en **d** bereiken via alle paren van gehele getallen, met _0 < = [0] d < 10_ en _0 < = d[1] < 12_. 
-   Aan de rechterkant van de predikaatfunctie expressie, moet u er een voorwaarde is. In dit voorbeeld voor elke waarde van **s** en **d** zodat de voorwaarde waar is, er is een rand van het knooppunt van de laag bron naar het knooppunt van de laag bestemming. Dus deze filterexpressie wordt aangegeven dat de bundel een verbinding uit het knooppunt gedefinieerd door **s** naar het knooppunt gedefinieerd door **d** in alle gevallen waarbij s [0] gelijk aan [0] d is bevat.  

Desgewenst kunt u een reeks lijndikten voor een gefilterde bundel opgeven. De waarde voor het kenmerk **lijndikten** moet een tupel met zwevende komma waarden met een lengte die overeenkomt met het aantal verbindingen die zijn gedefinieerd door de bundel. Standaard worden willekeurig lijndikten gegenereerd.  

Gewichtswaarden zijn gegroepeerd op de bestemming knooppuntindex. Als het eerste bestemmingsknooppunt is aangesloten op K bron knooppunten, zijn de eerste _K_ -elementen van de tupel **lijndikten** dat wil zeggen de gewichten voor het eerste bestemmingsknooppunt in de volgorde van de gegevensbron index. Dit geldt ook voor de resterende knooppunten van de bestemming.  

Het is mogelijk om op te geven lijndikten direct als constante waarden. Bijvoorbeeld als u de lijndikten eerder hebt geleerd, kunt u ze als constanten gebruik de volgende syntaxis:

    const Weights_1 = [0.0188045055, 0.130500451, ...]


## <a name="convolutional-bundles"></a>Convolutional pakketten
Wanneer de trainingsgegevens een homogene structuur bevat, veelgebruikte convolutional verbindingen zijn voor meer informatie over de uitgebreide functies van de gegevens. Bijvoorbeeld in afbeeldings-, audio- of videobestanden, ruimte of tijdelijke dimensionaliteit kunnen gegevens zijn vrij uniform.  

Convolutional pakketten gebruiken rechthoekige **kernels** die door de afmetingen worden geschoven. Elke kernel definieert in feite een set lijndikten toegepast in lokale groepen, genoemd **kernel-toepassingen**. Elke toepassing kernel overeenkomt met een knooppunt in de bronlaag, waarnaar wordt verwezen als het **centrale knooppunt**. Het gewicht van een kernel worden tussen veel verbindingen gedeeld. In een convolutional bundel, elke kernel rechthoekige is en alle kernel-toepassingen zijn dezelfde grootte.  

Convolutional pakketten ondersteunen de volgende kenmerken:

**InputShape** Hiermee definieert u de dimensionaliteit van de bronlaag voor de toepassing van deze convolutional bundel. De waarde moet een tupel van positieve gehele getallen. Het product van de gehele getallen moet gelijk zijn aan het aantal knooppunten in de bronlaag, maar anders hoeft niet zodat deze overeenkomt met de dimensionaliteit voor de bronlaag zijn gedeclareerd. De lengte van deze tupel wordt de waarde **ariteit** voor de convolutional bundel. (Meestal ariteit verwijst naar het aantal argumenten of operanden die een functie kan duren.)  

Gebruik de kenmerken **KernelShape**, **Stapsgewijze**, **Opvulling**, **LowerPad**en **UpperPad**definiëren van de vorm en de locatie van de kernels:   

-   **KernelShape**: (vereist) definieert de dimensionaliteit van elke kernel voor de convolutional bundel. De waarde moet een tupel van positieve getallen met een lengte die gelijk is aan de ariteit is van de bundel. Elk onderdeel van dit tupel moet niet groter is dan het bijbehorende onderdeel van **InputShape**. 
-   **Stapsgewijze**: (optioneel) definieert u de schuifregelaar stap grootte van de Convolutie (één stapgrootte voor elke dimensie), die de afstand tussen de centrale knooppunten is. De waarde moet een tupel van positieve getallen met een lengte waarmee de ariteit is van de bundel. Elk onderdeel van dit tupel moet niet groter is dan het bijbehorende onderdeel van **KernelShape**. De standaardwaarde is een tupel met alle onderdelen gelijk is aan een. 
-   **Delen**: (optioneel) het gewicht delen voor elke dimensie van de Convolutie definieert. De waarde mag één Booleaanse waarde of een tupel van Booleaanse waarden met een lengte waarmee de ariteit is van de bundel. Een enkele Booleaanse waarde uitgebreid worden van een tupel van de juiste lengte met alle onderdelen gelijk is aan de opgegeven waarde. De standaardwaarde is een tupel die uit alle waar waarden bestaat. 
-   **MapCount**: (optioneel) bevat het nummer van de functie voor de convolutional bundel kaarten. De waarde mag een één positief geheel getal of een tupel van positieve getallen met een lengte waarmee de ariteit is van de bundel. Een waarde één geheel getal is uitgebreid om te worden van een tupel van de juiste lengte met de eerste onderdelen gelijk is aan de opgegeven waarde en alle resterende onderdelen gelijk aan een. De standaardwaarde is een. Het totale aantal functie kaarten is het product van de onderdelen van de tupel. De waarbij van dit totaal aantal over de onderdelen bepaalt hoe de functie kaart waarden in de bestemming knooppunten worden gegroepeerd. 
-   **Lijndikten**: (optioneel) het eerste lijndikten voor de bundel definieert. De waarde moet een tupel van zwevende komma waarden met een lengte die het aantal keren kernels is het aantal lijndikten per kernel, zoals gedefinieerd verderop in dit artikel. De standaard-lijndikten worden willekeurig gegenereerd.  

Er zijn twee sets met eigenschappen waarmee opvulling, de eigenschappen die elkaar wederzijds uitsluiten wordt:

-   **Opvulling**: (optioneel) bepaald of de invoer moet worden opgevuld met behulp van een **standaard opvulling kleurenschema**. De waarde één Booleaanse waarde kan zijn kan, of dit een tupel van Booleaanse waarden met een lengte waarmee de ariteit is van de bundel. Een enkele Booleaanse waarde uitgebreid worden van een tupel van de juiste lengte met alle onderdelen gelijk is aan de opgegeven waarde. Als de waarde voor een dimensie waar is, wordt de bron logisch opgevuld in die dimensie met cellen nul waarde ter ondersteuning van extra kernel-toepassingen, zodat de centrale knooppunten van de eerste en laatste kernels in die dimensie de eerste en laatste knooppunten in die dimensie in de bronlaag zijn. Dus, het aantal "pop" knooppunten in elke dimensie wordt automatisch bepaald, zodat deze past precies _(InputShape [d] - 1) / stapsgewijze [d] + 1_ kernels in de bronlaag gevuld. Als de waarde voor een dimensie ONWAAR is, wordt de kernels zijn gedefinieerd zodat het aantal knooppunten aan elke kant die zijn weggelaten, hetzelfde (tot en met een verschil van 1 wordt). De standaardwaarde van dit kenmerk is een tupel met alle onderdelen gelijk is aan ONWAAR.
-   **UpperPad** en **LowerPad**: (optioneel) geven betere controle over de hoeveelheid opvulling gebruiken. **Belangrijke:** Deze kenmerken kunnen worden gedefinieerd en alleen als de eigenschap **Opvulling** hierboven wordt ***niet*** gedefinieerd. De waarden moet tupels met lengte berekend die de ariteit is van de bundel zijn gehele getallen. Wanneer u deze kenmerken zijn opgegeven, wordt "pop" knooppunten worden toegevoegd aan de laagste en de hoogste uiteinden van elke dimensie van de invoer laag. Het aantal knooppunten toegevoegd aan de laagste en de hoogste uiteinden in elke dimensie wordt bepaald door **LowerPad**[i] en **UpperPad**[i]. Om ervoor te zorgen dat kernels alleen "reële" knooppunten en niet mag "pop" knooppunten overeenkomen, is de volgende voorwaarden voldaan:
    -   Elk onderdeel van **LowerPad** moet strikt kleiner is dan KernelShape [d] / 2. 
    -   Elk onderdeel van **UpperPad** niet groter zijn dan KernelShape [d] / 2. 
    -   De standaardwaarde van deze kenmerken is een tupel met alle onderdelen gelijk is aan 0. 

De instelling **Opvulling** = true kunt zoveel opvulling nodig is om te houden van de 'center"van de kernel binnen de"reële"invoer. Hiermee wordt de berekening enigszins voor het berekenen van de grootte. In het algemeen wordt de grootte _D_ wordt berekend als _D = (I - K) / S + 1_, waar _ik_ de grootte van de invoer, is _K_ de grootte van de kernel is, _S_ is de stride, en _/_ deling van de gehele getal (afronden naar nul) is. Als u UpperPad instellen = [1, 1], de grootte van de invoer _ik_ is effectief 29, en dus _D = (29-5) / 2 + 1 = 13_. Echter als **Opvulling** = waar, in principe _ik_ krijgt tegenaan omhoog door _K - 1_; Daarom _D = ((28 + 4) - 5) / 2 + 1 = 27 / 2 + 1 = 13 + 1 = 14_. Door het opgeven van waarden voor **UpperPad** en **LowerPad** dat u veel meer controle over de opvulling dan als u zojuist hebt ingesteld **Opvulling** = true.

Raadpleeg de volgende artikelen voor meer informatie over convolutional-netwerken te gebruiken en hun toepassingen:  

-   [http://deeplearning.NET/Tutorial/lenet.HTML](http://deeplearning.net/tutorial/lenet.html )
-   [http://Research.Microsoft.com/pubs/68920/icdar03.PDF](http://research.microsoft.com/pubs/68920/icdar03.pdf) 
-   [http://People.csail.MIT.edu/jvb/Papers/cnn_tutorial.PDF](http://people.csail.mit.edu/jvb/papers/cnn_tutorial.pdf)  

## <a name="pooling-bundles"></a>Groepsgewijze pakketten
Een **bundel groepsgewijze** is van toepassing geometrie die vergelijkbaar is met convolutional connectivity, maar het knooppunt bronwaarden vooraf gedefinieerde functies gebruikt om de waarde van bestemming knooppunt te verkrijgen. Groepering pakketten hebben dus geen trainable status (lijndikten of noodzakelijk). Groepering pakketten ondersteuning voor de convolutional kenmerken behalve **delen**, **MapCount**en **lijndikten**.  

Meestal de kernels samengevat door aangrenzende groepering eenheden elkaar niet overlappen. Als stap [d] gelijk aan KernelShape [d] in elke dimensie is, is de laag verkregen de traditionele lokale groepering laag, die meestal in convolutional neural netwerken gebruikt wordt. Elk bestemmingsknooppunt berekent het maximale aantal of het gemiddelde van de activiteiten van de kernel in de bronlaag.  

Het volgende voorbeeld ziet u een groepering bundel: 

    hidden P1 [5, 12, 12]
      from C1 max pool {
        InputShape  = [ 5, 24, 24];
        KernelShape = [ 1,  2,  2];
        Stride      = [ 1,  2,  2];
      }  

-   De ariteit is van de bundel is 3 (de lengte van de tupels **InputShape**, **KernelShape**en **Stapsgewijze**). 
-   Het aantal knooppunten in de bronlaag is _5 *24* 24 = 2880_. 
-   Dit is een traditionele lokale groepering laag omdat **KernelShape** en **Stapsgewijze** gelijk zijn. 
-   Het aantal knooppunten in de laag bestemming is _5 *12* 12 = 1440_.  
    
Raadpleeg de volgende artikelen voor meer informatie over groeperen lagen:  

-   [http://www.cs.Toronto.edu/~hinton/absps/imagenet.PDF](http://www.cs.toronto.edu/~hinton/absps/imagenet.pdf) (Sectie 3.4)
-   [http://CS.Nyu.edu/~koray/publis/lecun-iscas-10.PDF](http://cs.nyu.edu/~koray/publis/lecun-iscas-10.pdf) 
-   [http://CS.Nyu.edu/~koray/publis/jarrett-iccv-09.PDF](http://cs.nyu.edu/~koray/publis/jarrett-iccv-09.pdf)
    
## <a name="response-normalization-bundles"></a>Antwoord normaliseren pakketten
**Antwoord normaliseren** is een lokale normaliseren kleurenschema dat is geïntroduceerd door Geoffrey Hinton, en al, in het papier [ImageNet Classiﬁcation met uitgebreide Convolutional Neural-netwerken te gebruiken](http://www.cs.toronto.edu/~hinton/absps/imagenet.pdf). Antwoord normaliseren wordt gebruikt bij generalisatie in neural hebben. Wanneer een neuron wordt uitgevoerd op een van zeer hoog Activeringsniveau, wordt het Activeringsniveau van de omringende neurons door een lokale antwoord normaliseren laag onderdrukt. Dit gebeurt met behulp van drie parameters (***α***, ***β***en ***k***) en een convolutional structuur (of groep vorm). Elke neuron in de bestemming laag ***y*** overeenkomt met een neuron ***x*** in de bronlaag. Het Activeringsniveau van ***y*** wordt gegeven door de volgende formule, waarin ***f*** is het Activeringsniveau van een neuron en ***Nx*** is de kernel (of de set met de neurons in de groep van ***x***), zoals gedefinieerd door de structuur van de volgende convolutional:  

![][1]  

Antwoord normaliseren pakketten ondersteuning voor de convolutional kenmerken behalve **delen**, **MapCount**en **lijndikten**.  
 
-   Als de kernel neurons in dezelfde map als ***x bevat***, wordt het kleurenschema normaliseren genoemd naar **dezelfde map normaliseren**. Als u wilt dezelfde map normaliseren definiëren, moet de eerste coördinaten in **InputShape** de waarde 1.
-   Als de kernel neurons in dezelfde ruimte positie als ***x bevat***, maar de neurons in andere kaarten zijn, heet het kleurenschema normaliseren **over kaarten normaliseren**. Dit type antwoord normaliseren implementeert een formulier van de zijkanten maar geïnspireerd op het type gevonden in reële neurons, concurrenten voor groot activering niveaus mee neuron uitvoer van berekend op de verschillende kaarten maken. Als u wilt definiëren over kaarten normaliseren, moet de eerste coördinaten een geheel getal groter is dan één en niet groter is dan het aantal kaarten en de rest van de coördinaten moet hebben de waarde 1.  

Aangezien een vooraf gedefinieerde functie antwoord normaliseren pakketten op knooppunt bronwaarden om te bepalen de waarde van bestemming knooppunt toepassen, hebben ze geen trainable status (lijndikten of noodzakelijk).   

**Waarschuwing**: de knooppunten in de laag bestemming komen overeen met neurons die de centrale knooppunten van de kernels zijn. Als KernelShape [d] een oneven getal is, klikt u vervolgens bijvoorbeeld _KernelShape [d] / 2_ overeenkomt met het knooppunt centraal kernel. Als _KernelShape [d]_ even getal is, is het centrale knooppunt _KernelShape [d] / 2-1_. Daarom als **Opvulling**[d] ONWAAR, de eerste en de laatste is _KernelShape [d] / 2_ knooppunten geen corresponderende knooppunten in de laag bestemming. Als u wilt voorkomen dat deze situatie, definiëren **Opvulling** als [waar, waar,..., waar].  

De kenmerken vier hierboven, ondersteunen antwoord normaliseren pakketten ook de volgende kenmerken:  

-   **Alfa**: (vereist) Hiermee wordt een drijvende-waarde die met ***α*** in de vorige formule overeenkomt. 
-   **Bèta**: (vereist) Hiermee wordt een drijvende-waarde die met ***β*** in de vorige formule overeenkomt. 
-   **Verschuiving**: (optioneel) Hiermee wordt een drijvende-waarde die met ***k*** in de vorige formule overeenkomt. Standaardwaarde 1.  

Het volgende voorbeeld wordt een antwoord normaliseren bundel met deze kenmerken gedefinieerd:  

    hidden RN1 [5, 10, 10]
      from P1 response norm {
        InputShape  = [ 5, 12, 12];
        KernelShape = [ 1,  3,  3];
        Alpha = 0.001;
        Beta = 0.75;
      }  

-   De bronlaag bevat vijf plattegronden, elk voorzien van een aof dimensie van 12 x 12, totaalbedrag in 1440 knooppunten. 
-   De waarde van **KernelShape** geeft aan dat dit een dezelfde map normaliseren laag, waar een rechthoek 3 x 3 bevindt zich in de groep. 
-   De standaardwaarde van **Opvulling** ONWAAR is, dus de laag bestemming slechts 10 knooppunten heeft in elke dimensie. Toevoegen als u wilt opnemen één knooppunt in de bestemming laag die met elk knooppunt in de bronlaag overeenkomt, opvulling = [waar, waar, waar]; en wijzig de grootte van RN1 [5, 12, 12].  

## <a name="share-declaration"></a>Declaratie delen 
NET # ondersteunt (optioneel) meerdere pakketten met gedeelde lijndikten definiëren. Het gewicht van een twee pakketten kunnen worden gedeeld als hun structuren hetzelfde zijn. De syntaxis van de volgende definieert pakketten met gedeelde lijndikten:  

    share-declaration:
        share    {    layer-list    }
        share    {    bundle-list    }
       share    {    bias-list    }
    
    layer-list:
        layer-name    ,    layer-name
        layer-list    ,    layer-name
    
    bundle-list:
       bundle-spec    ,    bundle-spec
        bundle-list    ,    bundle-spec
    
    bundle-spec:
       layer-name    =>     layer-name
    
    bias-list:
        bias-spec    ,    bias-spec
        bias-list    ,    bias-spec
    
    bias-spec:
        1    =>    layer-name
    
    layer-name:
        identifier  

Zo geeft de volgende delen-declaratie de namen van lagen, die aangeeft dat systematische en lijndikten moeten worden gedeeld:  

    Const {
      InputSize = 37;
      HiddenSize = 50;
    }
    input {
      Data1 [InputSize];
      Data2 [InputSize];
    }
    hidden {
      H1 [HiddenSize] from Data1 all;
      H2 [HiddenSize] from Data2 all;
    }
    output Result [2] {
      from H1 all;
      from H2 all;
    }
    share { H1, H2 } // share both weights and biases  

-   De invoer functies worden ondergebracht in twee gelijke grote invoer lagen. 
-   De verborgen lagen berekent u hogere niveau functies op de twee invoer lagen. 
-   Het delen-declaratie geeft aan dat _H1_ en _H2_ op dezelfde manier van de desbetreffende invoeritems moeten worden berekend.  
 
U kunt ook kan deze worden opgegeven met twee afzonderlijke delen-declaraties als volgt:  

    share { Data1 => H1, Data2 => H2 } // share weights  

<!-- -->

    share { 1 => H1, 1 => H2 } // share biases  

U kunt de korte vorm alleen als de lagen een enkel bundel bevatten. In het algemeen, is delen mogelijk alleen als de structuur van de relevante identiek zijn is, zodat ze dezelfde grootte, dezelfde convolutional geometrie, enzovoort.  

## <a name="examples-of-net-usage"></a>Voorbeelden van Net # gebruik
Hier vindt enkele voorbeelden van hoe u Net # kunt verborgen lagen toevoegen, de manier waarop verborgen lagen interactie met andere lagen en convolutional netwerken definiëren.   

### <a name="define-a-simple-custom-neural-network-hello-world-example"></a>Een eenvoudig aangepaste neural netwerk definiëren: ' Hallo ' voorbeeld
Dit eenvoudige voorbeeld wordt gedemonstreerd hoe u een neural netwerk model met één verborgen laag maakt.  

    input Data auto;
    hidden H [200] from Data all;
    output Out [10] sigmoid from H all;  

Het voorbeeld van enkele basisopdrachten als volgt:  

-   De eerste regel definieert de invoer laag (ook wel _gegevens_genoemd). Wanneer u het trefwoord **automatisch** gebruikt, bevat het neural netwerk automatisch alle kolommen van de functie in de invoer voorbeelden. 
-   De tweede regel wordt gemaakt van de verborgen laag. De naam _H_ is toegewezen aan de verborgen laag, waarvoor 200 knooppunten. Deze laag is volledig verbonden met de invoer laag.
-   De derde regel Hiermee definieert u de uitvoer laag (met de naam _O_), met 10 uitvoer knooppunten. Als het neural netwerk wordt gebruikt voor classificatie, moet u er één uitvoer knooppunt per klasse is. Het trefwoord **sigmoid** wordt aangegeven dat de functie uitvoer wordt toegepast op de laag uitvoer.   

### <a name="define-multiple-hidden-layers-computer-vision-example"></a>Meerdere verborgen lagen definiëren: computer vision voorbeeld
Het volgende voorbeeld wordt het definiëren van een iets meer complexe neural netwerk, met meerdere aangepaste verborgen lagen.  

    // Define the input layers 
    input Pixels [10, 20];
    input MetaData [7];
    
    // Define the first two hidden layers, using data only from the Pixels input
    hidden ByRow [10, 12] from Pixels where (s,d) => s[0] == d[0];
    hidden ByCol [5, 20] from Pixels where (s,d) => abs(s[1] - d[1]) <= 1;
    
    // Define the third hidden layer, which uses as source the hidden layers ByRow and ByCol
    hidden Gather [100] 
    {
      from ByRow all;
      from ByCol all;
    }
    
    // Define the output layer and its sources
    output Result [10]  
    {
      from Gather all;
      from MetaData all;
    }  

In dit voorbeeld ziet u enkele functies van de taal van de specificatie neural netwerken te gebruiken:  

-   De structuur heeft twee invoer lagen, _Pixels_ en _metagegevens_.
-   De laag _Pixels_ is een bronlaag voor twee verbinding-pakketten, met bestemming lagen, _ByRow_ en _ByCol_.
-   De lagen _verzamelen_ en het _resultaat_ zijn bestemming lagen in meerdere verbinding-pakketten.
-   De uitvoer laag, _resultaat_, is een laag bestemming in twee verbinding-pakketten; een met het tweede niveau verborgen (verzamelen) als een laag bestemming en de andere met de invoer laag (metagegevens) als een laag bestemming.
-   De verborgen lagen, _ByRow_ en _ByCol_, kunt u opgeven dat gefilterde connectiviteit met predikaatfunctie expressies. Nog nauwkeuriger, het knooppunt in _ByRow_ aan [x, y] is verbonden met de knooppunten in _Pixels_ waarvoor de eerste index coördinaten gelijk is aan de eerste coördinaten van het knooppunt, x. Daarnaast het knooppunt in _ByCol aan [x, y] is verbonden met de knooppunten in _Pixels_ met de tweede index coördineren binnen een van de tweede coördinaten van het knooppunt, y.  

### <a name="define-a-convolutional-network-for-multiclass-classification-digit-recognition-example"></a>Een netwerk met een convolutional voor multiclass classificatie definiëren: cijfers herkenning voorbeeld
De definitie van de volgende netwerk is ontworpen te herkennen van getallen en ziet u enkele geavanceerde technieken voor het aanpassen van een neural netwerk.  

    input Image [29, 29];
    hidden Conv1 [5, 13, 13] from Image convolve 
    {
       InputShape  = [29, 29];
       KernelShape = [ 5,  5];
       Stride      = [ 2,  2];
       MapCount    = 5;
    }
    hidden Conv2 [50, 5, 5]
    from Conv1 convolve 
    {
       InputShape  = [ 5, 13, 13];
       KernelShape = [ 1,  5,  5];
       Stride      = [ 1,  2,  2];
       Sharing     = [false, true, true];
       MapCount    = 10;
    }
    hidden Hid3 [100] from Conv2 all;
    output Digit [10] from Hid3 all;  


-   De structuur heeft één invoer laag, _afbeelding_.
-   Het trefwoord **convolve** wordt aangegeven dat de lagen met de naam _Conv1_ en _Conv2_ convolutional lagen zijn. Elk van deze declaraties laag wordt gevolgd door een lijst met de kenmerken Convolutie.
-   Het net heeft een derde verborgen laag, _Hid3_, die volledig is verbonden met de tweede verborgen laag, _Conv2_.
-   De uitvoer laag, _cijfers_, is alleen voor de derde verborgen laag, _Hid3_verbonden. Het trefwoord **alle** wordt aangegeven dat de laag uitvoer volledig is verbonden met _Hid3_.
-   De ariteit is van de Convolutie is drie (de lengte van de tupels **InputShape**, **KernelShape** **Stapsgewijze**en **delen**). 
-   Het aantal lijndikten per kernel is _1 + **KernelShape**\[0] * * *KernelShape**\[1]* **KernelShape**\[2] = 1 + 1 *5* 5 = 26. Of 26 * 50 = 1300_.
-   U kunt als volgt de knooppunten in elke laag die verborgen berekenen:
    -   **NodeCount**\[0] = (5 - 1) / 1 + 1 = 5.
    -   **NodeCount**\[1] = (13 tot en met 5) / 2 + 1 = 5. 
    -   **NodeCount**\[2] = (13 tot en met 5) / 2 + 1 = 5. 
-   Het totale aantal knooppunten kan worden berekend met behulp van de opgegeven dimensionaliteit van de laag, [50, 5, 5], als volgt: _ **MapCount** * * *NodeCount**\[0]* **NodeCount**\[1] * * *NodeCount**\[2] = 10* 5 *5* 5_
-   Omdat **delen**[d] ONWAAR is alleen voor _d == 0_, het aantal kernels is _ **MapCount** * * *NodeCount**\[0] = 10* 5 = 50_. 


## <a name="acknowledgements"></a>Bevestigingen

De taal Net # voor het aanpassen van de architectuur van neural netwerken is bij Microsoft ontwikkeld door Shon Katzenberger (ontwerper, Machine Learning) en Alexey Kamenev (Software-engineeren, Microsoft Research). Dit is intern gebruikt voor machine learning-projecten en toepassingen die variëren van detectie van de afbeelding naar tekst analytics. Zie voor meer informatie [Neural hebben in Azure ML - Inleiding tot Net #](http://blogs.technet.com/b/machinelearning/archive/2015/02/16/neural-nets-in-azure-ml-introduction-to-net.aspx)


[1]:./media/machine-learning-azure-ml-netsharp-reference-guide/formula_large.gif
 
