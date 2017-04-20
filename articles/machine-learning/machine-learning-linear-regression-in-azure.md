<properties 
    pageTitle="Gebruik van de lineaire regressie in Machine Learning | Microsoft Azure" 
    description="Een vergelijking van lineaire regressie-modellen in Excel en in Azure Machine Learning Studio" 
    metaKeywords="" 
    services="machine-learning" 
    documentationCenter="" 
    authors="garyericson" 
    manager="jhubbard" 
    editor="cgronlun"  />

<tags 
    ms.service="machine-learning" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/09/2016" 
    ms.author="kbaroni;garye" />

# <a name="using-linear-regression-in-azure-machine-learning"></a>Lineaire regressie in Azure Machine Learning gebruiken

> *Kate Baroni* en *Ben Boatman* zijn enterprise oplossing architecten in Microsoft gegevens inzichten Center Excellence. In dit artikel beschrijven ze hun ervaring migreren van een bestaande regressie-analyse-suite naar een cloud-oplossing met Azure Machine Learning.  
 
&nbsp; 
  
[AZURE.INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]  
 
## <a name="goal"></a>Doel:

Onze project de slag met twee doelstellingen in gedachten:  

1. Bekijk gebruiksanalyses gebruiken om de nauwkeurigheid van de maandelijkse inkomsten prognoses van onze organisatie te verbeteren  
2. Azure ML gebruiken om te bevestigen, optimaliseren, snelheid, vergroten en schaal van onze resultaten.  

Zoals veel bedrijven doorloopt onze organisatie een maandelijkse inkomsten prognoses proces. Het kleine team van bedrijfsanalisten is onderbroken met het gebruik van Machine Learning ter ondersteuning van het proces en voorspellen nauwkeurigheid te verbeteren.  Het team besteed verschillende maanden verzamelen van gegevens uit meerdere bronnen en de gegevenskenmerken uitvoeren van statistische analyses belangrijke kenmerken die relevant zijn voor de services verkoop prognoses identificeren.  Volgende stappen is om te beginnen prototypen statistische regressie modellen op de gegevens in Excel.  Binnen een paar weken hebben we een Excel-regressie-model dat is feit dat het huidige veld en Financiën prognoses processen. Dit is het resultaat van de voorspelling basislijn geworden.  


De volgende stap namen we vervolgens onze blog analytics via overschakelen op Azure ML om vast te stellen hoe Azure ML van de blog prestaties verbeteren kan.


## <a name="achieving-predictive-performance-parity"></a>Bekijk prestaties overeenkomst bereiken

Onze eerste prioriteit is om te bereiken overeenkomst tussen Azure ML en Excel regressie-modellen.  De exacte dezelfde gegevens en dezelfde splitsing uitgedrukt voor training en gegevens we wilt bereiken blog prestaties overeenkomst tussen Excel en Azure ML testen.   In eerste instantie is mislukt. De Excel-objectmodel ver-loop van het model Azure ML.   De fout is vanwege een gebrek aan begrip van de instelling grondtal hulpmiddel in Azure ML. Na een synchronisatie met het productteam Azure ML we een beter begrip van de instelling vereist voor onze gegevenssets base ervaring en overeenkomst tussen de twee modellen bereikt.  

### <a name="create-regression-model-in-excel"></a>Regressiemodel in Excel maken
Onze regressie Excel gebruikt het standaard lineaire regressie-model zijn gevonden in de Analysis ToolPak voor Excel. 

We berekend *gemiddelde Absolute % fout* en gebruikt deze als de prestaties eenheid voor het model.  Dit duurde drie maanden bij een werkmodel met Excel aankomt.  We overgebracht veel van de informatie in het Azure ML experiment waarin uiteindelijk handig in vereisten is.

### <a name="create-comparable-experiment-in-azure-machine-learning"></a>Vergelijkbare experiment in Azure Machine Learning maken  
We deze stappen om u te maken van onze experiment in Azure ML gevolgd:  

1.  De gegevensset als een csv-bestand naar Azure ML (zeer klein-bestand) hebt geüpload
2.  Een nieuwe experiment hebt gemaakt en gebruikt u de [Kolommen selecteren in de gegevensset] [ select-columns] module om te selecteren van dezelfde gegevensfuncties gebruikt in Excel   
3.  Gebruikt u de [Gesplitste gegevens] [ split] module (met *Relatieve expressie* modus) worden de gegevens verdeeld door exacte dezelfde trein sets zoals is gebeurd in Excel  
4.  Geëxperimenteerd met een [Lineaire regressie] [ linear-regression] module (alleen standaardopties), beschreven en de resultaten aan onze Excel regressiemodel vergeleken

### <a name="review-initial-results"></a>De eerste resultaten bekijken
Aanvankelijk het Excel-model duidelijk ver-loop van het model Azure ML:  

|   |Excel|Azure ML|
|---|:---:|:---:|
|Prestaties|   |  |
|<ul style="list-style-type: none;"><li>Aangepast R-kwadraat</li></ul>| 0.96 |N/B|
|<ul style="list-style-type: none;"><li>Coëfficiënt van <br />Bepaling</li></ul>|N/B|   0.78<br />(lage nauwkeurigheid)|
|Absolute Error betekenen |  $9. 5M|  $ 19.4 M|
|Gemiddelde van Absolute Error (%)|   6.03%|  12.2%

Als we onze proces en de resultaten door de ontwikkelaars en de gegevens wetenschappers uitgevoerd op het team van Azure ML, aangeboden ze snel enkele nuttige tips.  

* Wanneer u een [Lineaire regressie] gebruiken[ linear-regression] module in Azure ML twee methoden zijn beschikbaar:
    *  Online afkomst van de kleurovergang: Zijn mogelijk meer geschikt zijn voor grotere schaal problemen
    *  Gewone kleinste kwadraten: Dit is de methode denken wanneer ze lineaire regressie horen. Voor kleine gegevenssets mag gewone kleinste kwadraten meer optimale kiezen.
*  Kunt u de parameter L2-regularisatie gewicht om prestaties te verbeteren. Dit is ingesteld op 0,001 al dan niet standaard en voor onze kleine gegevensverzameling, we Stel deze in op 0,005 prestaties te verbeteren.    

### <a name="mystery-solved"></a>Mystery kan worden opgelost!
Als we de aanbevelingen hebt toegepast, bereikt we de dezelfde basislijn in Azure ML als met Excel:   

|| Excel|Azure ML (initiaal)|Azure ML met kleinste kwadraten|
|---|:---:|:---:|:---:|
|Gelabelde waarde  |Werkelijke waarden (numeriek)|dezelfde|dezelfde|
|Ingesteld  |Excel-gegevens > regressie-analyse >|Lineaire regressie.|Lineaire regressie|
|Opties voor ingesteld|N/B|Standaardwaarden|gewone kleinste kwadraten<br />L2 0,005 =|
|Gegevensverzameling|26 rijen, 3 onderdelen, 1 label.   Alle numerieke.|dezelfde|dezelfde|
|Gesplitste: trein|Excel ervaren in de eerste 18 rijen, getest in de laatste 8 rijen.|dezelfde|dezelfde|
|Gesplitste: Test|Excel regressie-formule is toegepast op de laatste 8 rijen|dezelfde|dezelfde|
|**Prestaties**||||
|Aangepast R-kwadraat|0.96|N/B||
|Correlatiecoëfficiënt|N/B|0.78|0.952049|
|Absolute Error betekenen |$9. 5M|$ 19.4 M|$9. 5M|
|Gemiddelde van Absolute Error (%)|<span style="background-color: 00FF00;">6.03%</span>|12.2%|<span style="background-color: 00FF00;">6.03%</span>|

De Excel-coëfficiënten vergeleken bovendien ook met de functie lijndikten in het Azure ervaren model:

||Excel coëfficiënten.|Azure functie lijndikten|
|---|:---:|:---:|
|Snijpunt/afwijking|19470209.88|19328500|
|Functie A|0.832653063|0.834156|
|Functie B|11071967.08|11007300|
|Functie C|25383318.09|25140800|

## <a name="next-steps"></a>Volgende stappen

We wilde Azure ML webservice vanuit Excel gebruiken.  Onze bedrijfsanalisten, is afhankelijk van Excel en we een manier om te bellen van de Azure ML-webservice met een rij met gegevens van Excel en deze de voorspelde waarde terugkeren naar Excel nodig.   

We ook wilde optimaliseren van onze model, met de opties en algoritmen die beschikbaar zijn in Azure ML.

### <a name="integration-with-excel"></a>Integratie met Excel
De oplossing was mogelijk onze Azure ML regressiemodel maken door te maken van een webservice uit het ervaren model.  Binnen enkele minuten, de webservice is gemaakt en we Roep deze kan rechtstreeks vanuit Excel om een voorspelde inkomsten waarde te retourneren.    

De sectie *Web Services-Dashboard* bevat een downloadbare Excel-werkmap.  De werkmap wordt geleverd vooraf opgemaakte met de API en schemabestanden webservicegegevens ingesloten.   Als u op *Excel-werkmap downloaden*klikt, wordt geopend en kunt u het opslaan naar uw lokale computer.    

![][1]
 
Met de werkmap is geopend, kopieert u de vooraf gedefinieerde parameters in de sectie met blauwe Parameter zoals hieronder wordt weergegeven.  Als de parameters worden ingevoerd, wordt Excel ziet u bij de webservice AzureML en de voorspelde behaald worden etiketten wordt weergegeven in de sectie met groene voorspelde waarden.  De werkmap blijft maken van prognoses voor parameters op basis van uw ervaren model voor alle rij-items onder Parameters hebt ingevoerd.   Zie [gebruik van een webservice Azure Machine Learning uit Excel](machine-learning-consuming-from-excel.md)voor meer informatie over het gebruik van deze functie. 

![][2]
 
### <a name="optimization-and-further-experiments"></a>Optimalisatie en verder experimenten
Nu dat we een basislijn gehad met onze Excel-model, verplaatst we verder naar het optimaliseren van onze Azure ML lineaire regressiemodel.  We de module [onderdelen selecteren op basis van een Filter] gebruikt[ filter-based-feature-selection] om te verbeteren van onze selectie van de oorspronkelijke gegevens elementen en deze heeft ons geholpen een prestatieverbetering van 4,6% bereiken Absolute Error bedoelt.   We gebruiken deze functie die kan ons weken opslaan in de gegevenskenmerken naar de juiste set met functies om te gebruiken voor modellering zoeken doorlopen voor toekomstige projecten.  

Volgende we van plan bent om op te nemen extra algoritmen zoals [Bayesiaanse] [ bayesian-linear-regression] of [Verhoogd beslissingsbomen] [ boosted-decision-tree-regression] in ons experiment om te vergelijken prestaties.    

Als u experimenteren met regressie wilt, is een goede gegevensset om te proberen de gegevensset energie Efficiency regressie voorbeeld, waarin een groot aantal numerieke kenmerken heeft. De gegevensset is opgegeven als onderdeel van de steekproef gegevenssets in ML Studio.  U kunt allerlei zal het leren van modules voorspellen verwarming laden of koeling laden.  De onderstaande grafiek is dat een vergelijking van de prestaties van verschillende regressie leert ten opzichte van het energieverbruik gegevensset voorspellingen te doen voor de doeltoepassing variabele koeling laden: 

|Model|Absolute Error betekenen|Gemiddelde van de hoofdsite het kwadraat van fout|Relatieve, Absolute Error|Ten opzichte van het kwadraat van fout|Correlatiecoëfficiënt
|---|---|---|---|---|---
|Gestimuleerd beslissingsstructuur|0.930113|1.4239|0.106647|0.021662|0.978338
|Lineaire regressie (kleurovergang afkomst)|2.035693|2.98006|0.233414|0.094881|0.905119
|Neural netwerk regressie|1.548195|2.114617|0.177517|0.047774|0.952226
|Lineaire regressie (gewone kleinste kwadraten)|1.428273|1.984461|0.163767|0.042074|0.957926  

## <a name="key-takeaways"></a>Belangrijke Takeaways 

Hebt u geleerd uitziet door van actieve Excel regressie en Azure Machine Learning proeven parallel. Het model basislijn maken in Excel en vergelijken met modellen met Azure ML [Lineaire regressie] [ linear-regression] ertoe bijgedragen ons leert u Azure ML en we ontdekt verkoopkansen gegevens selectie en model prestaties te verbeteren.         

Ook gevonden dat het is raadzaam te gebruiken [op basis van een Filter-onderdelen selecteren] [ filter-based-feature-selection] te versnellen tekstvoorspelling toekomstige projecten.  Functieselectie toepassen op uw gegevens, kunt u een verbeterde model maken in Azure ML met betere algehele prestaties. 

De mogelijkheid om over te brengen de blog analytische prognoses van Azure ML naar Excel systemically kunnen aanzienlijk in de mogelijkheid geven succes resultaten een brede bedrijven publiek.     


## <a name="resources"></a>Resources
Enkele bronnen worden voor helpen die u met regressie werken vermeld:  

* Regressie in Excel.  Als u regressie in Excel nooit hebt geprobeerd, deze zelfstudie kunt u heel gemakkelijk: [http://www.excel-easy.com/examples/regression.html](http://www.excel-easy.com/examples/regression.html)
* Regressie tegenover prognoses.  Tyler Chessman geschreven een blog-artikel wordt uitgelegd hoe reeks prognoses in Excel, die een goede beginners omschrijving van lineaire regressie bevat uitvoeren op tijden. [http://sqlmag.com/SQL-Server-Analysis-Services/Understanding-Time-Series-forecasting-Concepts](http://sqlmag.com/sql-server-analysis-services/understanding-time-series-forecasting-concepts)  
*   Gewone kleinste kwadraten lineaire regressie: Fouten, problemen en eventuele problemen.  Voor een inleiding en een discussie regressie: [http://www.clockbackward.com/2009/06/18/ordinary-least-squares-linear-regression-flaws-problems-and-pitfalls/](http://www.clockbackward.com/2009/06/18/ordinary-least-squares-linear-regression-flaws-problems-and-pitfalls/ )

[1]: ./media/machine-learning-linear-regression-in-azure/machine-learning-linear-regression-in-azure-1.png
[2]: ./media/machine-learning-linear-regression-in-azure/machine-learning-linear-regression-in-azure-2.png


<!-- Module References -->
[bayesian-linear-regression]: https://msdn.microsoft.com/library/azure/ee12de50-2b34-4145-aec0-23e0485da308/
[boosted-decision-tree-regression]: https://msdn.microsoft.com/library/azure/0207d252-6c41-4c77-84c3-73bdf1ac5960/
[filter-based-feature-selection]: https://msdn.microsoft.com/library/azure/918b356b-045c-412b-aa12-94a1d2dad90f/
[linear-regression]: https://msdn.microsoft.com/library/azure/31960a6f-789b-4cf7-88d6-2e1152c0bd1a/
[select-columns]: https://msdn.microsoft.com/library/azure/1ec722fa-b623-4e26-a44e-a50c6d726223/
[split]: https://msdn.microsoft.com/library/azure/70530644-c97a-4ab6-85f7-88bf30a8be5f/
 
