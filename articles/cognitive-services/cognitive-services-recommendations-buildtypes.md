<properties
    pageTitle="Opbouwen bestandstypen en -Model kwaliteit: aanbevelingen API | Microsoft Azure"
    description="Azure Machine Learning aanbevelingen--aan de slag met"
    services="cognitive-services"
    documentationCenter=""
    authors="luiscabrer"
    manager="jhubbard"
    editor="cgronlun"/>

<tags
    ms.service="cognitive-services"
    ms.workload="data-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/20/2016"
    ms.author="luisca"/>

#  <a name="build-types-and-model-quality"></a>Bestandstypen en -model kwaliteit maken #

<a name="TypeofBuilds"></a>
## <a name="supported-build-types"></a>Typen ondersteunde opbouwen ##

Twee opbouwen typen worden momenteel ondersteund door de API aanbevelingen: *aanbeveling* en *FBT*. Elk is geïntegreerd met verschillende algoritmen en elk sterke heeft. In dit document wordt beschreven elk van deze builds en technieken voor het vergelijken van de kwaliteit van de modellen die zijn gegenereerd.

Als u nog niet gedaan hebt, wordt u aangeraden dat u het [aan de slag-handleiding](cognitive-services-recommendations-quick-start.md)worden voltooid.

<a name="RecommendationBuild"></a>
### <a name="recommendation-build-type"></a>Aanbeveling opbouwen type ###

Het type van de build aanbeveling wordt gebruikt matrix factoriseren aanbevelingen. Deze genereert [latente functie](https://en.wikipedia.org/wiki/Latent_variable) vectoren op basis van uw transacties om te beschrijven van elk item en gebruikt u vervolgens deze latente vectoren om te vergelijken van items die vergelijkbaar zijn.

Als u het model op basis van aankopen in uw elektronische winkel trainen en geef een telefoon Lumia 650 als invoer voor het model, retourneert het model een reeks items die vaak worden aangeschaft door personen die zijn waarschijnlijk het aanschaffen van een telefoon Lumia 650. De items zijn mogelijk niet op de bijbehorende. In dit voorbeeld is het mogelijk dat het model andere telefoons wordt geretourneerd, omdat de personen die de 650 Lumia zoals mogelijk andere telefoons zoals.

Het bouwen aanbeveling heeft twee mogelijkheden waardoor deze fraaie:

* *Het bouwen aanbeveling ondersteunt *koudwatersystemen item* plaatsing **

Items die ik heb geen aanzienlijk gebruik worden koudwatersystemen items genoemd. Als u een verzending van een telefoon die u nooit verkocht voordat u hebt ontvangt, kan geen het systeem bijvoorbeeld aanbevelingen voor dit product op transacties alleen afleiden. Dit betekent dat het systeem moet Leer van informatie over het product zelf.

Als u gebruiken koudwatersystemen artikelen worden geplaatst wilt, moet u functies informatie voor elk van de items in de catalogus te verstrekken. Hieronder ziet u wat de eerste paar regels van de catalogus kunnen er uitzien als (notitie de belangrijkste = waarde opmaak voor de functies).

>Pro2 6CX 00001, oppervlak-, oppervlak,, Type Hardware opslag = = 128 GB geheugen = 4G, fabrikant = Microsoft

>73H-00013, activeren Xbox 360, spellen,, Type = Software, taal Engels, beoordeling = = volwaardige

>Bezwaar 0F05, Minecraft Xbox 360, spellen,, * Type = Software, taal = Spaans, beoordeling = jeugd

U moet ook de volgende opbouwen parameters instellen:

| Parameter maken         | Notities
|------------------     |-----------
|*useFeaturesInModel*     | Ingesteld op **waar**.  Geeft aan als functies kunnen worden gebruikt om de aanbeveling model verbeteren.
|*allowColdItemPlacement*   | Stel op **true**. Geeft aan als de aanbeveling ook koudwatersystemen items via de functie overeenkomsten moet push.
| *modelingFeatureList*   | Door komma's gescheiden lijst met functienamen als u wilt gebruiken in het bouwen aanbeveling voor het verbeteren van de aanbeveling. Bijvoorbeeld: "taal, opslag" voor het voorgaande voorbeeld.

**Het bouwen aanbeveling ondersteunt gebruiker aanbevelingen**

Een opbouwen aanbeveling ondersteunt [aanbevelingen van de gebruiker](https://westus.dev.cognitive.microsoft.com/docs/services/Recommendations.V4.0/operations/56f30d77eda5650db055a3dd). Dit betekent dat deze kunt persoonlijke aanbevelingen voor gebruikers die op basis van hun geschiedenis transactie. Voor aanbevelingen van de gebruiker, kunt u de gebruikers-ID of de recente geschiedenis van transacties voor die gebruiker opgeven.

Een klassiek voorbeeld van de plaats waar u mogelijk om toe te passen aanbevelingen van de gebruiker bij het aanmelden op de welkomstpagina is. U kunt er inhoud die is van toepassing op de specifieke gebruiker promoveren.

U kunt ook een aanbevelingen opbouwen type toepassen wanneer de gebruiker bevindt zich bijna om af te rekenen. Op dat moment hebt u de lijst met items die de gebruiker bevindt zich bijna aan te schaffen en kunt u aanbevelingen op basis van de huidige market mandje opgeeft.

#### <a name="recommendations-build-parameters"></a>Aanbevelingen maken parameters

| Naam  |   Beschrijving |    Type <br>  geldige waarden <br> (standaardwaarde)
|-------|-------------------|------------------
| *NumberOfModelIterations* |   Het aantal iteraties die het model wordt uitgevoerd, is doorgevoerd door de algehele berekeningscluster tijd en de nauwkeurigheid model. Hoe hoger het getal, hoe nauwkeuriger het model, maar de tijd berekeningscluster duurt langer.  |   Geheel getal, een <br>  10 tot 50 <br>(40)
| *NumberOfModelDimensions* |   Het aantal dimensies betrekking heeft op het aantal functies van die het model probeert om binnen uw gegevens te vinden. Het aantal dimensies met groter wordende kunt beter optimaliseren van de resultaten in kleinere clusters. Te veel dimensies kunnen echter het model niet vinden correlatie tussen items. |  Geheel getal, een <br> 10 tot en met 40 <br>(20) |
| *ItemCutOffLowerBound* |  Hiermee definieert u de minimale aantal gebruik punten die een item zou moeten hebben om deze als onderdeel van het model worden beschouwd. |        Geheel getal, een <br> 2 of meer <br> (2) |
| *ItemCutOffUpperBound* |  Hiermee definieert u het maximum aantal gebruik punten die een item zou moeten hebben om deze als onderdeel van het model worden beschouwd. |  Geheel getal, een <br>2 of meer<br> (2147483647) |
|*UserCutOffLowerBound* |   Hiermee definieert u het minimum aantal transacties die een gebruiker moet hebben uitgevoerd om te worden beschouwd als onderdeel van het model. | Geheel getal, een <br> 2 of meer <br> (2)
| *UserCutOffUpperBound* |  Hiermee definieert u het maximum aantal transacties die een gebruiker moet hebben uitgevoerd om te worden beschouwd als onderdeel van het model. | Geheel getal, een <br>2 of meer <br> (2147483647)|
| *UseFeaturesInModel* |    Geeft aan als functies kunnen worden gebruikt om de aanbeveling model verbeteren. |     Booleaanse waarde<br> Standaard: waar
|*ModelingFeatureList* |    Door komma's gescheiden lijst met functienamen moet worden gebruikt in het bouwen aanbeveling voor het verbeteren van de aanbeveling. Dit is afhankelijk van de functies die belangrijk zijn. |    Tekenreeks, maximaal 512 tekens
| *AllowColdItemPlacement* |    Geeft aan als de aanbeveling ook koudwatersystemen items via de functie overeenkomsten moet push. | Booleaanse waarde <br> Standaard: ONWAAR
| *EnableFeatureCorrelation*    | Geeft aan als functies kunnen worden gebruikt in redeneren. | Booleaanse waarde <br> Standaard: ONWAAR
| *ReasoningFeatureList* |  Door komma's gescheiden lijst met functienamen moet worden gebruikt voor redeneren zinnen, zoals aanbeveling uitleg. Dit is afhankelijk van de functies die belangrijk voor klanten zijn. | Tekenreeks, maximaal 512 tekens
| *EnableU2I* | Persoonlijke aanbevelingen, ook wel genoemd gebruiker artikel (U2I) aanbevelingen inschakelen. | Booleaanse waarde <br>Standaard: waar
|*EnableModelingInsights* | Hiermee definieert u of offline evaluatie moet worden uitgevoerd voor het verzamelen van modellering inzichten (dat wil zeggen de precisie van formules en verscheidenheid doelstellingen). Als ingesteld op waar, een subset van de gegevens niet gebruikt worden voor training, omdat deze worden gereserveerd moet voor het testen van het model. Meer informatie over [offline evaluaties](#OfflineEvaluation). | Booleaanse waarde <br> Standaard: ONWAAR
| *SplitterStrategy* | Als inschakelen modeling inzichten is ingesteld op *waar*, is dit hoe de gegevens voor evaluatie moet worden opgesplitst.  | Tekenreeks, *RandomSplitter* of *LastEventSplitter* <br>Standaard: RandomSplitter


<a name="FBTBuild"></a>
### <a name="fbt-build-type"></a>FBT opbouwen type ###

Het vaak gekochte samen (FBT) bouwen biedt een analyse die telt het aantal keren twee of drie verschillende producten samen aan elkaar worden opgeteld. Deze de sets op basis van een overeenkomsten-functie (**samen exemplaren**, **Jaccard**, **til**) wordt gesorteerd.

Als het ware **Jaccard** en **til** manieren om te normaliseren de collega exemplaren.  Dit betekent dat de items die wordt geretourneerd alleen als ze waar aangeschaft samen met het item zaad.

In ons voorbeeld Lumia 650 telefoon worden telefoon X alleen als de telefoon X is gekocht in dezelfde sessie als de telefoon Lumia 650 geretourneerd. Omdat dit mogelijk niet waarschijnlijk, zou we items aanvulling op de 650 Lumia moeten worden geretourneerd; verwachten bijvoorbeeld, de schermbeveiliging van een of een adapter voor de 650 Lumia.

Twee items wordt momenteel uitgegaan in dezelfde sessie worden aangeschaft als deze in een transactie met de dezelfde gebruikersnaam en -tijdstempel voorkomen.

FBT builds ondersteunen geen koudwatersystemen items, omdat per definitie ze te kopen in dezelfde transactie twee artikelen verwachten. Terwijl FBT builds sets van items (groepen van drie cijfers) retourneren kunnen, niet wordt ondersteund persoonlijke aanbevelingen omdat ze een enkel zaad-item als invoer accepteren.


#### <a name="fbt-build-parameters"></a>FBT opbouwen parameters

| Naam  |   Beschrijving |       Type  <br> geldige waarden <br> (standaardwaarde)
|-------|---------------|-----------------------
| *FbtSupportThreshold* | Hoe conservatieve is van het model. Het aantal collega exemplaren van items voor modellering beschouwd. |  Geheel getal, een <br> 3-50 <br> (6)
| *FbtMaxItemSetSize* | Het aantal items in een veelgebruikte set bounds.| Geheel getal  <br> 2-3 <br> (2)
| *FbtMinimalScore* | Minimale score die een veelgebruikte set hebt nodig in de geretourneerde resultaten moeten worden opgenomen. Hoe hoger hoe beter. | Dubbeltik <br> 0 en hoger <br> (0)
| *FbtSimilarityFunction* | Hiermee definieert u de functie overeenkomsten moet worden gebruikt door de opbouwen. **Til** meer gericht serendipity en **Jaccard** is een inbreuk op tussen de twee **collega exemplaar** meer voorspelbaarheid gericht. | Tekenreeks  <br>  <i>cooccurrence, lift, jaccard</i><br> Standaard: <i>jaccard</i>

<a name="SelectBuild"></a>
## <a name="build-evaluation-and-selection"></a>Evaluatie en selectie maken ##

Deze richtlijnen waarmee u kunt bepalen of u moet een opbouwen aanbevelingen of een opbouwen FBT gebruiken, maar biedt geen een definitieve antwoord in zaken waar u een van deze kan gebruiken. Ook, zelfs als u weet dat u wilt een FBT opbouwen van Excel gebruikt, u mogelijk nog steeds wilt kiezen **Jaccard** of **til** als de functie overeenkomsten.

De beste manier om te selecteren tussen twee verschillende versies is om te testen ze in de praktijk (online evaluatie) en een tarief conversie voor de verschillende versies bijhouden. Het tarief weer dat conversie kan worden gemeten op basis van de aanbeveling klikken, het werkelijke aantal aankopen aanbevelingen weergegeven, of zelfs op de werkelijke aankoopbedragen wanneer de verschillende aanbevelingen zijn weergegeven. U kunt uw conversie tarief metrisch op basis van uw zakelijke doelstelling selecteren.

In sommige gevallen wilt u mogelijk het model offline evalueren voordat u deze in productie hebt opgeslagen. Terwijl u offline evaluatie is geen vervanging voor online evaluatie, kan deze dienen als een meting.

<a name="OfflineEvaluation"></a>
## <a name="offline-evaluation"></a>Offline evaluatie  ##

Het doel van een evaluatie met offline is om te voorspellen precisie (het aantal gebruikers dat een van de aanbevolen items te kopen) en de uiteenlopende aanbevelingen (het aantal items die worden aanbevolen).
Als onderdeel van de evaluatie van de precisie van formules en verscheidenheid aan de doelstellingen, het systeem vindt u een voorbeeld van gebruikers en de transacties voor die gebruikers in twee groepen worden gesplitst: de gegevensset training en de gegevensset test.

> [AZURE.NOTE]Als u wilt gebruiken offline aan de doelstellingen, moet u tijdstempels in uw gegevens over zoekgebruik hebben.
> Tijdgegevens is verplicht voor het gebruik correct tussen training en test gegevenssets splitsen.

> Offline evaluatie mogelijk ook niet rendement resultaten voor kleine gebruik bestanden. Voor de evaluatie moeten uitgebreide, moet er een minimum aan 1000 gebruik punten in de gegevensset test.

<a name="Precision"></a>
### <a name="precision-at-k"></a>Precisie-op-k ###
De volgende tabel geeft de uitvoer van de precisie-op-k offline evaluatie.

| K | 1 | 2 | 3 |   4 |     5
|---|---|---|---|---|---|
|Percentage |   13,75 | 18.04   | 21 |  24.31 | 26.61
|Gebruikers in testen |    10.000 |    10.000 |    10.000 |    10.000 |    10.000
|Gebruikers die worden beschouwd als | 10.000 |    10.000 |    10.000 |    10.000 |    10.000
|Gebruikers niet meegenomen | 0 | 0 | 0 | 0 | 0

#### <a name="k"></a>K
In de vorige tabel staat *k* voor het aantal aanbevelingen weergegeven naar de klant. De tabel wordt als volgt: "Als slechts één aanbeveling is gedurende de testperiode aan de klanten getoond, alleen 13,75 van de gebruikers zou hebt aangeschaft die aanbeveling." Deze instructie is gebaseerd op verondersteld dat het model is ervaren met gegevens van de aankoop. Een andere manier zegt dit is dat de precisie bij 1 13,75.

U ziet dat als meer items worden weergegeven naar de klant, de kans van de klant aanschaffen van een aanbevolen item toeneemt. Voor de voorgaande experiment, de kans bijna wordt verdubbeld tot 26.61% wanneer 5 items worden aanbevolen.

#### <a name="percentage"></a>Percentage
Het percentage van de gebruikers die met ten minste één van de aanbevelingen *k* gecommuniceerd wordt weergegeven. Het percentage wordt berekend door het delen van het aantal gebruikers dat met ten minste één aanbeveling gecommuniceerd met het totale aantal gebruikers als beschouwd. Zie gebruikers in aanmerking voor meer informatie.

#### <a name="users-in-test"></a>Gebruikers in testen
Gegevens in deze rij staat voor het totale aantal gebruikers in de gegevensset test.

#### <a name="users-considered"></a>Gebruikers die worden beschouwd als
Een gebruiker wordt alleen als het systeem aanbevolen ten minste *k* items op basis van het model dat is gegenereerd met behulp van de gegevensset training.

#### <a name="users-not-considered"></a>Gebruikers niet meegenomen
Gegevens in deze rij staan voor alle gebruikers niet meegenomen. De gebruikers die ten minste niet hebben ontvangen *k* aanbevolen items.

Gebruiker niet meegenomen = gebruikers in test – gebruikers beschouwd als

<a name="Diversity"></a>
### <a name="diversity"></a>Verscheidenheid ###
Verscheidenheid aan de doelstellingen meten het type items aanbevolen. De volgende tabel geeft de uitvoer van de offline evaluatie verscheidenheid.

|Emmertje percentiel |    0-90|  90-99| 99-100
|------------------|--------|-------|---------
|Percentage        | 34.258 | 55.127| 10.615


Totaal van items die worden aanbevolen: 100.000

Unieke items die worden aanbevolen: 954

#### <a name="percentile-buckets"></a>Percentiel gerangschikte verzamelingen
Elke Emmertje percentiel wordt voorgesteld door een reeks (minimum- en maximumwaarden waarden die tussen 0 en 100 liggen). De items dicht bij 100 zijn de meest populaire items en de items dicht bij 0 de minste populair zijn. Bijvoorbeeld als de percentagewaarde voor de 99-100 percentiel Emmertje 10.6 is, betekent dit dat 10.6 procent van de aanbevelingen als resultaat alleen de bovenste één procent meest populaire items gegeven. De percentiele Emmertje minimale waarde is inclusief en de maximumwaarde exclusief, behalve voor 100 is.
#### <a name="unique-items-recommended"></a>Unieke items aanbevolen
De unieke items aanbevolen metrisch ziet u het aantal unieke items die zijn geretourneerd voor evaluatie.
#### <a name="total-items-recommended"></a>Totaal aantal objecten aanbevolen
De totale items aanbevolen metrisch toont het aantal items die worden aanbevolen. Mogelijk dubbele waarden.

<a name="ImplementingEvaluation"></a>
### <a name="offline-evaluation-metrics"></a>Offline evaluatie aan de doelstellingen ###
De precisie van formules en verscheidenheid aan offline de doelstellingen zijn mogelijk nuttig wanneer u op welke maken om te gebruiken. Opbouwen keer, als onderdeel van de desbetreffende FBT of aanbeveling bouwen parameters:

-   Stel de parameter van de build *enableModelingInsights* op **true**.
-   Selecteer desgewenst de *splitterStrategy* (beide *RandomSplitter* of *LastEventSplitter*).
*RandomSplitter* wordt gesplitst de gebruiksgegevens in de trein en test wordt ingesteld op basis van het opgegeven *randomSplitterParameters* test percentage en willekeurige waarden invullen.
*LastEventSplitter* wordt gesplitst de gebruiksgegevens in de trein en wordt op basis van de laatste transactie voor elke gebruiker testen.

Hiermee wordt een opbouwen die gebruikmaakt van slechts een subset van de gegevens voor training en de rest van de gegevens gebruikt om te berekenen van de doelstellingen van de evaluatie activeren.  Nadat de build is voltooid, om de uitvoer van de evaluatie, moet u de [Get opbouwen aan de doelstellingen API](https://westus.dev.cognitive.microsoft.com/docs/services/Recommendations.V4.0/operations/577eaa75eda565095421666f), doorgeven van de desbetreffende *model* en *buildId*bellen.

 Hieronder volgt de JSON-uitvoer voor de evaluatie van de steekproef.


    {
     "Result": {
     "precisionItemRecommend": null,
     "precisionUserRecommend": {
      "precisionMetrics": [
        {
          "k": 1,
          "percentage": 13.75,
          "usersInTest": 10000,
          "usersConsidered": 10000,
          "usersNotConsidered": 0
        },
        {
          "k": 2,
          "percentage": 18.04,
          "usersInTest": 10000,
          "usersConsidered": 10000,
          "usersNotConsidered": 0
        },
        {
          "k": 3,
          "percentage": 21.0,
          "usersInTest": 10000,
          "usersConsidered": 10000,
          "usersNotConsidered": 0
        },
        {
          "k": 4,
          "percentage": 24.31,
          "usersInTest": 10000,
          "usersConsidered": 10000,
          "usersNotConsidered": 0
        },
        {
          "k": 5,
          "percentage": 26.61,
          "usersInTest": 10000,
          "usersConsidered": 10000,
          "usersNotConsidered": 0
        }
      ],
      "error": null
    },
    "diversityItemRecommend": null,
    "diversityUserRecommend": {
      "percentileBuckets": [
        {
          "min": 0,
          "max": 90,
          "percentage": 34.258
        },
        {
          "min": 90,
          "max": 99,
          "percentage": 55.127
        },
        {
          "min": 99,
          "max": 100,
          "percentage": 10.615
        }
      ],
      "totalItemsRecommended": 100000,
      "uniqueItemsRecommended": 954,
      "uniqueItemsInTrainSet": null,
      "error": null
      }
     },
    "Id": 1,
    "Exception": null,
    "Status": 5,
    "IsCanceled": false,
    "IsCompleted": true,
    "CreationOptions": 0,
    "AsyncState": null,
    "IsFaulted": false
    }
