<properties 
    pageTitle="Machine Learning-app: afwijking detectie Service | Microsoft Azure" 
    description="Afwijking detectie API is een voorbeeld die zijn gemaakt met Microsoft Azure Machine Learning die afwijkingen vastgesteld in de reeks tijdgegevens met numerieke waarden die gelijkmatig zijn verdeeld in tijd." 
    services="machine-learning" 
    documentationCenter="" 
    authors="alokkirpal" 
    manager="jhubbard"
    editor="cgronlun" /> 

<tags 
    ms.service="machine-learning" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.tgt_pltfrm="na" 
    ms.workload="multiple" 
    ms.date="10/11/2016" 
    ms.author="alokkirpal"/>


# <a name="machine-learning-anomaly-detection-service"></a>Machine Learning afwijking detectie Service#

##<a name="overview"></a>Overzicht

[Afwijking detectie API](https://datamarket.azure.com/dataset/aml_labs/anomalydetection) is een voorbeeld die zijn gemaakt met Azure Machine Learning die afwijkingen vastgesteld in de reeks tijdgegevens met numerieke waarden die gelijkmatig zijn verdeeld in tijd. 

Deze API kan de volgende soorten afwijkende patronen detecteren in tijd reeksgegevens:

* **Positieve en negatieve trends**: bijvoorbeeld wanneer geheugengebruik in een neerwaartse computing controleren mogelijk interessant omdat deze mogelijk indicatieve van een geheugen zijn verdwenen,

* **Wijzigingen in het dynamische bereik van waarden**: bijvoorbeeld als u de uitzonderingen door een cloudservice controleren, wijzigingen in het dynamische bereik van waarden kunnen aangeven instabiliteit in de status van de service, en

* **Bereikt en daalt**: bijvoorbeeld als u het aantal mislukte aanmelding in een service of aantal uitgecheckte bestanden verwijderen in een e-commerce site controleren, pieken of Spanningsdips kunnen aangeven abnormale gedrag.

Deze machine learning-detectoren voor het bijhouden van wijzigingen in waarden gedurende een bepaalde tijd en rapport lopende wijzigingen in de bijbehorende waarden als afwijking scores. Niet vereist ad hoc drempelwaarde optimaliseren en hun scores kunnen worden gebruikt om te bepalen ONWAAR positieve rente. De afwijking detectie API handig in verschillende scenario's is zoals service controle door het bijhouden van KPI's na verloop van tijd gebruik monitoring tot en met de doelstellingen zoals aantal zoekopdrachten, aantallen klikken, prestatiecontroles door items zoals geheugen, CPU, bestand worden gelezen, enzovoort na verloop van tijd.

De afwijking detectie aanbod wordt geleverd met handige hulpmiddelen om u te helpen. 

* De [webtoepassing](http://anomalydetection-aml.azurewebsites.net/) kunt u evalueren en de resultaten van afwijking detectie API's op uw gegevens visualiseren. 

* De [voorbeeldcode](http://adresultparser.codeplex.com/) ziet hoe u via een programma voor toegang tot de API en de resultaten in C# parseren.

>[AZURE.NOTE]
>Probeer **afwijking inzichten die IT-oplossing** mogelijk gemaakt door [Deze API](https://datamarket.azure.com/dataset/aml_labs/anomalydetection)
>
>Om dit complete oplossing geïmplementeerd in uw Azure abonnement <a href="https://gallery.cortanaintelligence.com/Solution/Anomaly-Detection-Pre-Configured-Solution-1" target="_blank"> **Begin hier >**</a>


##<a name="api-definition"></a>API definitie

De service biedt een API REST gebaseerde via HTTPS die kan worden gebruikt op verschillende manieren, met inbegrip van een webpagina of mobiele-toepassing, R, Python, Excel, enzovoort.  U uw tijd reeksgegevens verzenden naar deze service via een oproep REST API en het uitvoeren van een combinatie van de drie afwijking typen die hierboven is beschreven. De service wordt uitgevoerd op het Azure Machine Learning-platform, dat wordt aangepast aan uw zakelijke behoeften naadloos en sla's bevat.

###<a name="headers"></a>Kopteksten
Zorgen ervoor dat u de juiste koppen in uw aanvraag, ziet er als volgt:

    Authorization: Basic <creds>
    Accept: application/json
               
    Where <creds> = ConvertToBase64(“AccountKey:” + yourActualAccountKey);  

U kunt uw accountsleutel vinden van uw account in de [Azure Datamarket](https://datamarket.azure.com/account/keys). 

###<a name="score-api"></a>Score-API

De API Score wordt gebruikt voor het uitvoeren van de afwijking detectie van niet-seizoensgebonden tijd reeksgegevens. De API wordt een aantal afwijking detectoren uitgevoerd op de gegevens en hun scores afwijking retourneert. De onderstaande afbeelding ziet u een voorbeeld van afwijkingen die de API Score kan detecteren. Deze tijdreeks heeft 2 distinct niveau wijzigingen en 3 pieken. De rode puntjes weergeven de tijd waarop de wijzigingen in het wordt aangetroffen, terwijl de zwarte puntjes de gevonden pieken weergeven.

![Score-API][1]
    
**URL**

    https://api.datamarket.azure.com/data.ashx/aml_labs/anomalydetection/v2/Score 

**Voorbeeld van de aanvraag hoofdtekst**

In de onderstaande uitnodiging verzenden we een tijdreeks met gegevenspunten uit 3/1/2016 tot en met 3/10/2016 en parameters van de Prikker detectoren (Zie afgekapte) tot de API voor detectie als een van deze gegevenspunten afwijkende. 

    {
      "data": [
        [ "9/21/2014 11:05:00 AM", "3" ],
        [ "9/21/2014 11:10:00 AM", "9.09" ],
        [ "9/21/2014 11:15:00 AM", "0" ]
      ],
      "params": {
        "tspikedetector.sensitivity": "3",
        "zspikedetector.sensitivity": "3",
        "trenddetector.sensitivity": "3.25",
        "bileveldetector.sensitivity": "3.25",
        "postprocess.tailRows": "2"
      }
    }

Het antwoord op dit is: 

    {
      "odata.metadata":"https://api.datamarket.azure.com/data.ashx/aml_labs/anomalydetection/v2/$metadata#AnomalyDetection.FrontEndService.Models.AnomalyDetectionResult",
      "ADOutput":"{
        "ColumnNames":["Time","Data","TSpike","ZSpike","rpscore","rpalert","tscore","talert"],
        "ColumnTypes":["DateTime","Double","Double","Double","Double","Int32","Double","Int32"],
        "Values":[
          ["9/21/2014 11:10:00 AM","9.09","0","0","-1.07030497733224","0","-0.884548154298423","0"],
          ["9/21/2014 11:15:00 AM","0","0","0","-1.05186237440962","0","-1.173800281031","0"]
        ]
      }"
    }

###<a name="scorewithseasonality-api"></a>ScoreWithSeasonality API

De API ScoreWithSeasonality wordt gebruikt voor het uitvoeren van afwijking detectie op tijdreeks waarvoor seizoensgebonden patronen. Deze API is handig om te bepalen de deviaties in seizoensgebonden patronen.  

De volgende afbeelding ziet een voorbeeld van afwijkingen gedetecteerd in een tijdreeks seizoensgebonden. De tijdreeks heeft één Prikker (de 1e zwarte stip), twee Spanningsdips (het 2e zwarte stip en een aan het einde) en één niveau wijzigen (rode stip). Houd er rekening mee dat zowel de dip in het midden van de tijdreeks en wijzigingen in het alleen discernable zijn nadat onderdelen seizoensgebonden zijn verwijderd uit de reeks.

![Periodieke variaties API][2]

**URL**

    https://api.datamarket.azure.com/data.ashx/aml_labs/anomalydetection/v2/ScoreWithSeasonality 

**Voorbeeld van de aanvraag hoofdtekst**

In de onderstaande uitnodiging verzenden we een tijdreeks met gegevenspunten uit 3/1/2016 tot en met 3/10/2016 en parameters (Zie afgekapte) tot de API voor detectie als een van deze gegevenspunten afwijkende. 

    {
      "data": [
        [ "9/21/2014 11:10:00 AM", "9" ],
        [ "9/21/2014 11:15:00 AM", "12" ],
        [ "9/21/2014 11:20:00 AM", "10" ]
      ],
      "params": {
        "preprocess.aggregationInterval": "0",
        "preprocess.aggregationFunc": "mean",
        "preprocess.replaceMissing": "lkv",
        "postprocess.tailRows": "1",
        "zspikedetector.sensitivity": "3",
        "tspikedetector.sensitivity": "3",
        "upleveldetector.sensitivity": "3.25",
        "bileveldetector.sensitivity": "3.25",
        "trenddetector.sensitivity": "3.25",
        "detectors.historywindow": "500",
        "seasonality.enable": "true",
        "seasonality.transform": "deseason",
        "seasonality.numSeasonality": "1"
      }
    }

Het antwoord op dit is: 

    {
        "odata.metadata": "https://api.datamarket.azure.com/data.ashx/aml_labs/anomalydetection/v2/$metadata#AnomalyDetection.FrontEndService.Models.AnomalyDetectionResult",
        "ADOutput": "{
            "ColumnNames":["Time","OriginalData","ProcessedData","TSpike","ZSpike","PScore","PAlert","RPScore","RPAlert","TScore","TAlert"],
            "ColumnTypes":["DateTime","Double","Double","Double","Double","Double","Int32","Double","Int32","Double","Int32"],
            "Values":[
                ["9/21/201411: 20: 00AM","10","10","0","0","-1.30229513613974","0","-1.30229513613974","0","-1.173800281031","0"]
            ]
        }"
    }

###<a name="detectors"></a>Detectoren

De afwijking detectie API ondersteunt detectoren in 3 categorieën. In de volgende tabel vindt u meer informatie over specifieke invoerparameters en uitvoer voor elke detectie.

|Detectie categorie|Detectie|Beschrijving|Invoerparameters|Uitvoer
|---|---|---|---|---|
|Prikker detectoren|TSpike detectie|Detecteren pieken en Spanningsdips op basis van uiterst de waarden zijn van de eerste en de derde kwartielen|*tspikedetector.sensitivity:* duurt geheel getal in het bereik 1-10, standaard: 3; Hoge waarden wordt onderschept meer extreme waarden en waardoor minder gevoelige|TSpike: binaire waarden: '1' als een Prikker/dip wordt aangetroffen, '0' anders|
||ZSpike detectie|Detecteren pieken en Spanningsdips op basis van hoe ver de datapoints van hun gemiddelde waarde zijn|*zspikedetector.sensitivity:* geheel getal in het bereik 1-10, standaard nemen: 3; Hoge waarden wordt onderschept meer extreme waarden waardoor het minder gevoelige|ZSpike: binaire waarden: '1' als een Prikker/dip wordt aangetroffen, '0' anders|
|Traag Trend detectie|Traag Trend detectie|Detecteren traag positieve trend aan de hand van de gevoeligheid instellen|*trenddetector.sensitivity:* drempel bij detectie score (standaard: 3,25, 3,25 tot en met 5 is een redelijk bereik tot Selecteer deze optie uit; Hoe hoger minder gevoelige)|TScore: zwevende getal dat staat voor afwijking score op trend|
|Detectoren niveau wijzigen|De wijzigingen in één richting detectie|Detecteren omhoog niveau wijzigen aan de hand van de gevoeligheid instellen|*upleveldetector.sensitivity:* drempel bij detectie score (standaard: 3,25, 3,25 tot en met 5 is een redelijk bereik tot Selecteer deze optie uit; Hoe hoger minder gevoelige)|PScore: zwevende getal dat staat voor afwijking score op Omhoog niveau wijzigen|
||Detectie van bidirectionele niveau wijzigen|Zowel omhoog en omlaag niveau wijzigen aan de hand van de set gevoeligheid detecteren|*bileveldetector.sensitivity:* drempel bij detectie score (standaard: 3,25, 3,25 tot en met 5 is een redelijk bereik tot Selecteer deze optie uit; Hoe hoger minder gevoelige)|RPScore: zwevende getal dat afwijking score op omhoog en omlaag niveau wijzigen

###<a name="parameters"></a>Parameters

Meer gedetailleerde informatie over deze invoerparameters wordt weergegeven in de onderstaande tabel:

|Invoerparameters|Beschrijving|Standaardinstelling|Type|Geldige bereik|Voorgestelde bereik|
|---|---|---|---|---|---|
|preprocess.aggregationInterval|Aggregatie interval in seconden voor het verzamelen van input tijdreeks|0 (geen aggregatie wordt uitgevoerd)|geheel getal|0: aggregatie, > 0 anders overslaan|5 minuten tot 1 dag, tijdreeks afhankelijke
|preprocess.aggregationFunc|Functie gebruikt voor het verzamelen van gegevens in de opgegeven AggregationInterval|gemiddelde|opgesomd|gemiddelde, som, lengte|N/B|
|preprocess.replaceMissing|Waarden die worden gebruikt om te rekenen ontbrekende gegevens|lkv (laatste waarde bekend)|opgesomd|nul, lkv, gemiddelde|N/B|
|detectors.historyWindow|Geschiedenis (in het aantal gegevenspunten) die wordt gebruikt voor afwijking score berekenen|500|geheel getal|10-2000|Tijdreeks afhankelijke|
|upleveldetector.Sensitivity|Gevoeligheid voor omhoog niveau wijzigen uit. |3,25|dubbele|Geen|3,25-5(Lesser values mean more sensitive)|
|bileveldetector.Sensitivity|Gevoeligheid voor bidirectionele niveau wijzigen uit. |3,25|dubbele|Geen|3,25-5(Lesser values mean more sensitive)|
|trenddetector.Sensitivity|De gevoeligheid voor positieve trend detectie. |3,25|dubbele|Geen|3,25-5(Lesser values mean more sensitive)|
|tspikedetector.Sensitivity |Gevoeligheid voor TSpike detectie|3|geheel getal|1-10|3-5(Lesser values mean more sensitive)|
|zspikedetector.Sensitivity |Gevoeligheid voor ZSpike detectie|3|geheel getal|1-10 |3-5(Lesser values mean more sensitive)|
|seasonality.Enable|Opgeven of periodieke variaties analyse wordt uitgevoerd|waar|Booleaanse waarde|waar, ONWAAR|Tijdreeks afhankelijke|
|seasonality.numSeasonality |Maximum aantal periodiek maal moeten worden gedetecteerd|1|geheel getal|1, 2|1-2|
|seasonality.transform |Of seizoensgebonden (en) trend onderdelen worden verwijderd voordat afwijking detectie is toegepast|deseason|opgesomd|geen, deseason, deseasontrend|N/B|
|postprocess.tailRows |Nummer van de meest recente gegevenspunten moeten worden behouden in de resultaten uitvoer|0|geheel getal|0 (laten alle gegevenspunten), of geef het aantal punten in zoekresultaten|N/B|

###<a name="output"></a>Uitvoer
De API alle detectoren op uw gegevens van de reeks tijd wordt uitgevoerd en geeft als resultaat afwijking scores en binaire Prikker indicatoren voor elk opsommingsteken in de tijd. De onderstaande tabel bevat uitvoer van de API. 

|Uitvoer|Beschrijving|
|---|---|
|Tijd|Tijdstempels van onbewerkte gegevens of gegevens geaggregeerde (en/of) impliciete als aggregatie (en/of) ontbreken gegevens begrip wordt toegepast|
|OriginalData|Waarden van onbewerkte gegevens of gegevens geaggregeerde (en/of) impliciete als aggregatie (en/of) ontbreken gegevens begrip wordt toegepast|
|ProcessedData|Een van de volgende handelingen uit: <ul><li>Seizoengezuiverd tijdreeks als aanzienlijk periodieke variaties is opgetreden en deseason optie is geselecteerd.</li><li>seizoenen aangepast en detrended tijdreeks als aanzienlijk periodieke variaties is opgetreden en deseasontrend optie is geselecteerd</li><li>anders is het resultaat hetzelfde als OriginalData</li>|
|TSpike|Binaire indicator om aan te geven of een Prikker wordt gedetecteerd door TSpike detectie|
|ZSpike|Binaire indicator om aan te geven of een Prikker wordt gedetecteerd door ZSpike detectie|
|Pscore|Een zwevende getal dat staat voor afwijking score op Omhoog niveau wijzigen|
|Palert|1/0-waarde die aangeeft dat er is een omhoog niveau wijzigen op basis van de invoer gevoeligheid afwijking|
|RPScore|Een zwevende getal dat afwijking score bidirectionele niveau wijzigen|
|RPAlert|1/0-waarde die aangeeft dat er is een niveau bidirectionele wijzigen op basis van de invoer gevoeligheid afwijking|
|TScore|Een zwevende getal dat afwijking score op positieve trend|
|TAlert|1/0-waarde die aangeeft dat er is een positieve trend afwijking op basis van de invoer gevoeligheid|


Deze uitvoer kan worden geparseerd met een [eenvoudige parser](https://adresultparser.codeplex.com/) : voorbeeld van code waarin u hoe leert u verbinding maken met de API en de uitvoer parseren heeft. De afwijkingen gedetecteerd kunnen worden weergegeven op een dashboard en/of doorgegeven aan menselijke experts voor corrigerende maatregelen of systemen voor kaartverkoop geïntegreerd.


[1]: ./media/machine-learning-apps-anomaly-detection/anomaly-detection-score.png
[2]: ./media/machine-learning-apps-anomaly-detection/anomaly-detection-seasonal.png

 

 
