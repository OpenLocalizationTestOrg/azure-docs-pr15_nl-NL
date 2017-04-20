<properties 
    pageTitle="Prognoses - ETS + STL | Microsoft Azure" 
    description="Prognoses - ETS + STL" 
    services="machine-learning" 
    documentationCenter="" 
    authors="xueshanz" 
    manager="jhubbard" 
    editor="cgronlun"/>

<tags 
    ms.service="machine-learning" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="08/17/2016" 
    ms.author="yijichen"/> 

#<a name="forecasting---ets--stl"></a>Prognoses - ETS + STL  

Deze [webservice]( https://datamarket.azure.com/dataset/aml_labs/demand_forecast) implementeert seizoensgebonden Trend uitgevouwen (STL) en (Exponential Triple Smoothing) modellen voorspellingen op basis van de historische gegevens door de gebruiker opgegeven produceren. Neemt de aanvraag voor een specifiek product dit jaar? Kan ik mijn productverkopen voorspellen voor de feestdagen Kerstmis, zodat ik mijn voorraad effectief kunt plannen? Prognose modellen zijn voor het adres van deze vragen. Gegeven in de afgelopen gegevens, onderzoeken deze modellen verborgen trends en periodieke variaties aan toekomstige trends voorspellen. 


[AZURE.INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)] 
 
>Deze webservice kan worden gebruikt door gebruikers – mogelijk via een mobiele app, via een website of zelfs op een lokale computer, bijvoorbeeld. Maar het doel van de web-service is ook als een voorbeeld van hoe Azure Machine Learning kan worden gebruikt om te maken van webservices boven op R code moet fungeren. Met een paar programmacoderegels en R en klikken van een knop binnen Azure Machine Learning Studio, worden een experiment gemaakt met R-code en als een webservice hebt gepubliceerd. De webservice kan vervolgens worden gepubliceerd naar de Azure Marketplace en verbruikt door gebruikers en apparaten kant van de wereld zitten zonder infrastructuur-instellingen van de auteur van de webservice.  
 
##<a name="consumption-of-web-service"></a>Verbruik van webservice 

Deze service 4 argumenten geaccepteerd en de prognoses berekend.
De invoer argumenten zijn:

* Frequentie - geeft de frequentie van de onbewerkte gegevens (dagelijks/wekelijks/maandelijks/studieprestaties/jaarlijks).
* Horizon - toekomstige prognose tijdsbestek.
* Datum - toevoegen in de nieuwe tijd reeksgegevens voor de tijd.
* De waarde - toevoegen in de nieuwe tijd reeks gegevenswaarden.

De uitvoer van de service is de berekende voorspelde waarden.
 
Voorbeeldinvoer mogelijk: 

* Frequentie - 12
* Horizon - 12
* Datum - 15-1-2012; 15-2-2012 15-3-2012; 4-15-2012; 15-5-2012; 15-6-2012; 15-7-2012; 8 / 15-2012; 15-9-2012; 15-10-2012; 15-11-2012; 12-15-2012; 1/15/2013; 2/15/2013 15/3/2013; 4/15/2013; 5/15/2013; 6/15/2013; 7/15/2013; 8 / 15/2013; 9/15/2013; 10/15/2013; 11/15/2013; 12/15/2013; 15-1-2014; 15-2-2014 15-3-2014; 4/15/2014; 15-5-2014; 15-6-2014; 15-7-2014; 8 / 15-2014; 15-9-2014
* Waarde - 3.479; 3,68; 3.832; 3.941; 3.797; 3.586; 3.508; 3.731; 3.915; 3.844; 3.634; 3.549; 3.557; 3.785; 3.782; 3.601; 3.544; 3.556; 3.65; 3.709; 3.682; 3.511; 3.429; 3.51; 3.523; 3.525; 3.626; 3.695; 3.711; 3.711; 3.693; 3.571; 3.509

>Deze service, is zoals die worden gehost op de Azure Marketplace, een OData-dienst. Deze mag worden aangeroepen via de POST of GET-methoden. 

Er zijn verschillende manieren van het gebruik van de service geautomatiseerd voortgezet (een voorbeeld-app is [hier](http://microsoftazuremachinelearning.azurewebsites.net/StlEtsForecasting.aspx )).

###<a name="starting-c-code-for-web-service-consumption"></a>C#-code voor verbruik van web-service starten:

    public class Input
    {
            public string frequency;
            public string horizon;
            public string date;
            public string value;
    }
    
    public AuthenticationHeaderValue CreateBasicHeader(string username, string password)
    {
            byte[] byteArray = System.Text.Encoding.UTF8.GetBytes(username + ":" + password);
            return new AuthenticationHeaderValue("Basic", Convert.ToBase64String(byteArray));
    }
    
    void Main()
    {
            var input = new Input() { frequency = TextBox1.Text, horizon = TextBox2.Text, date = TextBox3.Text, value = TextBox4.Text };         var json = JsonConvert.SerializeObject(input);
            var acitionUri = "PutAPIURLHere,e.g.https://api.datamarket.azure.com/..../v1/Score";
            var httpClient = new HttpClient();
    
            httpClient.DefaultRequestHeaders.Authorization = CreateBasicHeader("PutEmailAddressHere", "ChangeToAPIKey");
            httpClient.DefaultRequestHeaders.Accept.Add(new MediaTypeWithQualityHeaderValue("application/json"));
    
            var response = httpClient.PostAsync(acitionUri, new StringContent(json));
            var result = response.Result.Content;
            var scoreResult = result.ReadAsStringAsync().Result;
    }


##<a name="creation-of-web-service"></a>Het maken van een webservice 

>Deze webservice is gemaakt met Azure Machine Learning. Voor een gratis proefversie, evenals inleidende video's over het maken van experimenten en [publicerende webservices](machine-learning-publish-a-machine-learning-web-service.md), raadpleegt u [azure.com/ml](http://azure.com/ml). Hieronder vindt u een schermafbeelding van het experiment die de web-service en voorbeeld-code voor elk van de modules binnen het experiment hebt gemaakt.

Van binnen Azure Machine training, is een nieuwe lege experiment gemaakt. Voorbeeld invoergegevens is geüpload met een gegevensschema vooraf gedefinieerde. Een [R-Script uitvoeren] die zijn gekoppeld aan het gegevensschema van is[ execute-r-script] module, die wordt gegenereerd STL en ETS modellen prognoses met behulp van 'stl', 'ets', en 'voorspellen' functies uit R. 

###<a name="experiment-flow"></a>Experiment stroom:

![Stroom van experiment][2]

####<a name="module-1"></a>Module 1:
 
    # Add in the CSV file with the data in the format shown below 
![Voorbeeldgegevens][3]   

####<a name="module-2"></a>Module 2:

    # Data input
    data <- maml.mapInputPort(1) # class: data.frame
    library(forecast)
    
    # Preprocessing
    colnames(data) <- c("frequency", "horizon", "dates", "values")
    dates <- strsplit(data$dates, ";")[[1]]
    values <- strsplit(data$values, ";")[[1]]
    
    dates <- as.Date(dates, format = '%m/%d/%Y')
    values <- as.numeric(values)
    
    # Fit a time series model
    train_ts<- ts(values, frequency=data$frequency)
    fit1 <- stl(train_ts,  s.window="periodic")
    train_model <- forecast(fit1, h = data$horizon, method = 'ets')
    plot(train_model)
    
    # Produce forcasting
    train_pred <- round(train_model$mean,2)
    data.forecast <- as.data.frame(t(train_pred))
    colnames(data.forecast) <- paste("Forecast", 1:data$horizon, sep="")
    
    # Data output
    maml.mapOutputPort("data.forecast");

##<a name="limitations"></a>Beperkingen 

Dit is een zeer eenvoudige voorbeeld voor ETS + STL prognoses. Zoals u kunt zien van het bovenstaande voorbeeldcode, wordt geen fout vangen is geïmplementeerd en wordt de service wordt ervan uitgegaan dat alle variabelen continue/positieve waarden en de frequentie een geheel getal groter is dan 1 moet. De lengte van de datum en de waarde vectoren moet hetzelfde en de lengte van de tijdreeks moet groter zijn dan 2 * frequentie. De datum-variabele moet voldoen aan de notatie 'dd-mm-jjjj'.

##<a name="faq"></a>FAQ
Zie voor veelgestelde vragen over verbruik van de webservice of publiceren naar de Azure Marketplace [hier](machine-learning-marketplace-faq.md).

[1]: ./media/machine-learning-r-csharp-retail-demand-forecasting/retail-img1.png
[2]: ./media/machine-learning-r-csharp-retail-demand-forecasting/retail-img2.png
[3]: ./media/machine-learning-r-csharp-retail-demand-forecasting/retail-img3.png


<!-- Module References -->
[execute-r-script]: https://msdn.microsoft.com/library/azure/30806023-392b-42e0-94d6-6b775a6e0fd5/
 
