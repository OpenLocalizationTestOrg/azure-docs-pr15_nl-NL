<properties 
    pageTitle="Verschil in verhoudingen Test | Microsoft Azure" 
    description="Verschil in de verhoudingen wilt testen" 
    services="machine-learning" 
    documentationCenter="" 
    authors="aniedea" 
    manager="jhubbard" 
    editor="cgronlun"/>

<tags 
    ms.service="machine-learning" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/12/2016" 
    ms.author="aniedea"/> 


#<a name="difference-in-proportions-test"></a>Verschil in de verhoudingen wilt testen


Zijn er twee verhoudingen statistisch verschillende? Stel dat een gebruiker wil vergelijken twee films vaststellen of een film een aanzienlijk hoger deel van heeft 'leuk' wanneer vergeleken met de andere. Met een grote steekproeven, kan er een significantie verschil tussen de verhoudingen 0,50 en 0,51. Met een kleine steekproef, er mogelijk niet voldoende gegevens om te bepalen of deze verhoudingen ander zijn. 


[AZURE.INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

Deze [webservice]( https://datamarket.azure.com/dataset/aml_labs/prop_test) pleegt een test hypothese van het verschil in twee verhoudingen op basis van de invoer van de gebruiker van het aantal gunstige uitkomsten en het totale aantal experimenten voor de groepen 2 vergelijking. In een voorbeeldscenario, kan deze webservice worden aangeroepen vanuit binnen een film vergelijking app, waarbij de gebruiker of een van de films echt 'leuk vindt is' vaker dan de andere, op basis van filmclassificaties.

>Deze webservice kan worden gebruikt door gebruikers – mogelijk via een mobiele app, via een website of zelfs op een lokale computer, bijvoorbeeld. Maar het doel van de web-service is ook als een voorbeeld van hoe Azure Machine Learning kan worden gebruikt om te maken van webservices boven op R code moet fungeren. Met een paar programmacoderegels en R en klikken van een knop binnen Azure Machine Learning Studio, worden een experiment gemaakt met R-code en als een webservice hebt gepubliceerd. De webservice kan vervolgens worden gepubliceerd naar de Azure Marketplace en verbruikt door gebruikers en apparaten kant van de wereld zitten zonder infrastructuur-instellingen van de auteur van de webservice.


##<a name="consumption-of-web-service"></a>Verbruik van webservice

Deze service accepteert 4 argumenten en heeft een hypothese testen van de verhoudingen wilt behouden.

De invoer argumenten zijn:

* Successes1 - aantal geslaagde gebeurtenissen in de steekproef 1.
* Successes2 - aantal geslaagde gebeurtenissen in de steekproef 2.
* Total1 - grootte van voorbeeld 1.
* Total2 - grootte van de steekproef 2.

De uitvoer van de service is het resultaat van de hypothese testen samen met de chi-square statistische, say\f, p-waarde en verhoudingen in voorbeeld 1/2 en betrouwbaarheidsinterval grenzen.

>Deze service, is zoals die worden gehost op de Azure Marketplace, een OData-dienst. Deze mag worden aangeroepen via de POST of GET-methoden. 

Er zijn verschillende manieren van het gebruik van de service geautomatiseerd voortgezet (een voorbeeld-app is [hier](http://microsoftazuremachinelearning.azurewebsites.net/DifferenceInProportionsTest.aspx )).

###<a name="starting-c-code-for-web-service-consumption"></a>C#-code voor verbruik van web-service starten:

    public class Input
    {
            public string successes1;
            public string successes2;
            public string total1;
            public string total2;
    }
    
    public AuthenticationHeaderValue CreateBasicHeader(string username, string password)
    {
            byte[] byteArray = System.Text.Encoding.UTF8.GetBytes(username + ":" + password);
            return new AuthenticationHeaderValue("Basic", Convert.ToBase64String(byteArray));
    }

    void Main()
    {
            var input = new Input() { successes1 = TextBox1.Text, successes2 = TextBox2.Text, total1 = TextBox3.Text, total2 = TextBox4.Text };
            var json = JsonConvert.SerializeObject(input);
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

Van binnen Azure Machine training, een nieuwe lege experiment is gemaakt met twee [R-Script uitvoeren] [ execute-r-script] modules. In de eerste module die het gegevensschema is gedefinieerd, terwijl de tweede module gebruikt de opdracht prop.test in R de test hypothese voor 2 verhoudingen wilt behouden. 


###<a name="experiment-flow"></a>Experiment stroom:

![Stroom van experiment][2]


####<a name="module-1"></a>Module 1:
    ####Schema definition  
    data.set=data.frame(successes1=50,successes2=60,total1=100,total2=100);
    maml.mapOutputPort("data.set"); #send data to output port
    dataset1 = maml.mapInputPort(1) #read data from input port
    

####<a name="module-2"></a>Module 2:

    test=prop.test(c(dataset1$successes1[1],dataset1$successes2[1]),c(dataset1$total1[1],dataset1$total2[1])) #conduct hypothesis test

    result=data.frame(
    result=ifelse(test$p.value<0.05,"The proportions are different!",
    "The proportions aren't different statistically."),
    ChiSquarestatistic=round(test$statistic,2),df=test$parameter,
    pvalue=round(test$p.value,4),
    proportion1=round(test$estimate[1],4),
    proportion2=round(test$estimate[2],4),
    confintlow=round(test$conf.int[1],4),
    confinthigh=round(test$conf.int[2],4)) 

    maml.mapOutputPort("result"); #output port
    

##<a name="limitations"></a>Beperkingen 

Dit is een zeer eenvoudige voorbeeld voor een test van verschil in 2 verhoudingen wilt behouden. Zoals u kunt zien van het bovenstaande voorbeeldcode, geen fout vangen is geïmplementeerd en wordt de service wordt ervan uitgegaan dat alle variabelen continue.

##<a name="faq"></a>FAQ
Zie voor veelgestelde vragen over verbruik van de webservice of publiceren naar de Azure Marketplace [hier](machine-learning-marketplace-faq.md).

[1]: ./media/machine-learning-r-csharp-difference-in-two-proportions/hyptest-img1.png
[2]: ./media/machine-learning-r-csharp-difference-in-two-proportions/hyptest-img2.png


<!-- Module References -->
[execute-r-script]: https://msdn.microsoft.com/library/azure/30806023-392b-42e0-94d6-6b775a6e0fd5/
 
