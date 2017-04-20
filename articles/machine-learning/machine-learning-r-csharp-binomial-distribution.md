<properties 
    pageTitle="Binomiale verdeling Suite | Microsoft Azure" 
    description="Binomiale verdeling Suite" 
    services="machine-learning" 
    documentationCenter="" 
    authors="ireiter" 
    manager="jhubbard" 
    editor="cgronlun"/>

<tags 
    ms.service="machine-learning" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="10/04/2016" 
    ms.author="ireiter"/> 


#<a name="binomial-distribution-suite"></a>Binomiale verdeling Suite




De binomiaalverdeling-Suite is een verzameling steekproef-webservices ([Binomiale genereren](https://datamarket.azure.com/dataset/aml_labs/bdg5), [Kans Rekenmachine]( https://datamarket.azure.com/dataset/aml_labs/bdp4), [Kwantiel Rekenmachine]( https://datamarket.azure.com/dataset/aml_labs/bdq5)) die bij het oplossen van genereren en omgaan met binomiale onderzoeken. De services kunnen u genereert een binomiale verdeling reeks een willekeurige lengte, berekenen van quantiles gegeven afwezigheidsberichten kans en het berekenen van de kans van een bepaald kwantiel. Elk van de services verschillende uitvoer op basis van de geselecteerde service genereert (Zie onderstaande beschrijving). De binomiaalverdeling-Suite is gebaseerd op de R functies qbinom, rbinom en pbinom, die van het R stat-pakket uitmaken deel. 


[AZURE.INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

>Deze webservices kunnen worden gebruikt door gebruikers – potentieel rechtstreeks op marketplace, via een mobiele app, via een website of zelfs op een lokale computer, bijvoorbeeld. Maar het doel van de web-service is ook als een voorbeeld van hoe Azure Machine Learning kan worden gebruikt om te maken van webservices boven op R code moet fungeren. Met een paar programmacoderegels en R en klikken van een knop binnen Azure Machine Learning Studio, worden een experiment gemaakt met R-code en als een webservice hebt gepubliceerd. De webservice vervolgens kan worden gepubliceerd naar de Azure Marketplace en overal ter wereld worden gebruikt door gebruikers en apparaten: geen infrastructuur-instelling van de auteur van de webservice is vereist.

##<a name="consumption-of-web-service"></a>Verbruik van webservice
De binomiaalverdeling-Suite bevat de volgende 3 services.

###<a name="binomial-distribution-quantile-calculator"></a>Binomiale verdeling kwantiel Rekenmachine
Deze service 4 argumenten van een normale verdeling geaccepteerd en de bijbehorende kwantiel berekend.
De invoer argumenten zijn:

- p - één samengevoegd kans van meerdere experimenten.  
- grootte - het aantal experimenten.
- kans - de kans van slagen van een proefversie.
- Kant - L voor het onderste gedeelte van de verdeling gebruikt, U voor de bovenzijde van de verdeling. 

De uitvoer van de service is de berekende kwantiel die is gekoppeld aan de opgegeven kans.

###<a name="binomial-distribution-probability-calculator"></a>Binomiale verdeling kans Rekenmachine
Deze service 4 argumenten van een binomiale verdeling en de bijbehorende kans berekend.
De invoer argumenten zijn:

- q - een enkel kwantiel van een gebeurtenis met binomiale verdeling. 
- grootte - het aantal experimenten.
- kans - de kans van slagen van een proefversie.
- kant - L voor het onderste gedeelte van de verdeling gebruikt, U voor de bovenzijde van de verdeling of E die gelijk is aan een enkele aantal gunstige uitkomsten.

De uitvoer van de service is de berekende kans dat is gekoppeld aan de opgegeven kwantiel.

###<a name="binomial-distribution-generator"></a>Binomiale verdeling genereren
Deze service 3 argumenten van een binomiale verdeling geaccepteerd en genereert een willekeurige volgorde van de getallen die binomially zijn verdeeld. De volgende argumenten dienen ernaar binnen het verzoek:

- n - aantal waarnemingen. 
- grootte - aantal experimenten.
- kans - kans van slagen.

De uitvoer van de service is een reeks lengte n met een binomiale verdeling op basis van de argumenten grootte en kans.

>Deze service, is zoals die worden gehost op de Azure Marketplace, een OData-dienst. Deze mag worden aangeroepen via de POST of GET-methoden. 

Er zijn verschillende manieren van het gebruik van de service geautomatiseerd voortgezet (voorbeeld apps zijn hier: [genereren](http://microsoftazuremachinelearning.azurewebsites.net/BinomialDistributionGenerator.aspx), [Kans Rekenmachine](http://microsoftazuremachinelearning.azurewebsites.net/BinomialDistributionProbabilityCalculator.aspx), [Kwantiel Rekenmachine](http://microsoftazuremachinelearning.azurewebsites.net/BinomialDistributionQuantileCalculator)). 

###<a name="starting-c-code-for-web-service-consumption"></a>C#-code voor verbruik van web-service starten:

###<a name="binomial-distribution-quantile-calculator"></a>Binomiale verdeling kwantiel Rekenmachine
    public class Input
    {
            public string p;
            public string size;
            public string prob;
            public string side;
    }
    
    public AuthenticationHeaderValue CreateBasicHeader(string username, string password)
    {
            byte[] byteArray = System.Text.Encoding.UTF8.GetBytes(username + ":" + password);
            return new AuthenticationHeaderValue("Basic", Convert.ToBase64String(byteArray));
    }
    
    void main()
    {
            var input = new Input() { p = TextBox1.Text, size = TextBox2.Text, prob = TextBox3.Text, side = TextBox4.Text };
            var json = JsonConvert.SerializeObject(input);
            var acitionUri = "PutAPIURLHere,e.g.https://api.datamarket.azure.com/..../v1/Score";
            var httpClient = new HttpClient();
    
            httpClient.DefaultRequestHeaders.Authorization = CreateBasicHeader("PutEmailAddressHere", "ChangeToAPIKey");
            httpClient.DefaultRequestHeaders.Accept.Add(new MediaTypeWithQualityHeaderValue("application/json"));
    
            var response = httpClient.PostAsync(acitionUri, new StringContent(json));
            var result = response.Result.Content;
            var scoreResult = result.ReadAsStringAsync().Result;
    }

###<a name="binomial-distribution-probability-calculator"></a>Binomiale verdeling kans Rekenmachine
    public class Input
    {
            public string q;
            public string size;
            public string prob;
            public string side;
    }
    
    public AuthenticationHeaderValue CreateBasicHeader(string username, string password)
    {
            byte[] byteArray = System.Text.Encoding.UTF8.GetBytes(username + ":" + password);
            return new AuthenticationHeaderValue("Basic", Convert.ToBase64String(byteArray));
    }
    
    void Main()
    {
            var input = new Input() { q = TextBox1.Text, size = TextBox2.Text, prob = TextBox3.Text, side = TextBox4.Text };
            var json = JsonConvert.SerializeObject(input);
            var acitionUri = " PutAPIURLHere,e.g.https://api.datamarket.azure.com/..../v1/Score";
            var httpClient = new HttpClient();
    
            httpClient.DefaultRequestHeaders.Authorization = CreateBasicHeader("PutEmailAddressHere", "ChangeToAPIKey");
            httpClient.DefaultRequestHeaders.Accept.Add(new MediaTypeWithQualityHeaderValue("application/json"));
    
            var response = httpClient.PostAsync(acitionUri, new StringContent(json));
            var result = response.Result.Content;
            var scoreResult = result.ReadAsStringAsync().Result;
    }


###<a name="binomial-distribution-generator"></a>Binomiale verdeling genereren
    public class Input
    {
            public string n;
            public string size;
            public string p;
    }
    
    public AuthenticationHeaderValue CreateBasicHeader(string username, string password)
    {
            byte[] byteArray = System.Text.Encoding.UTF8.GetBytes(username + ":" + password);
            return new AuthenticationHeaderValue("Basic", Convert.ToBase64String(byteArray));
    }
    
    void Main()
    {
            var input = new Input() { n = TextBox1.Text, size = TextBox2.Text, p = TextBox3.Text };
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

###<a name="binomial-distribution-quantile-calculator"></a>Binomiale verdeling kwantiel Rekenmachine

![Werkruimte maken][4]

####<a name="module-1"></a>Module 1:
    #data schema with example data (replaced with data from web service)
    data.set=data.frame(p=0.1,size=10,prob=.5,side='L');
    maml.mapOutputPort("data.set"); #send data to output port
####<a name="module-2"></a>Module 2:

    dataset1 <- maml.mapInputPort(1) # class: data.frame
    param = dataset1
    if (param$p < 0 ) {
    print('Bad input: p must be between 0 and 1')
    param$p = 0
    } else if (param$p > 1) {
    print('Bad input: p must be between 0 and 1')
    param$p = 1
    }

    if (param$prob < 0 ) {
    print('Bad input: prob must be between 0 and 1')
    param$prob = 0
    } else if (param$prob > 1) {
    print('Bad input: prob must be between 0 and 1')
    param$prob = 1
    }

    quantile = qbinom(param$p,size=param$size,prob=param$prob)
    df = data.frame(x=1:param$size, prob=dbinom(1:param$size, param$size, prob=param$prob))
    quantile

    if (param$side == 'U'){
    quantile = qbinom(param$p,size=param$size,prob=param$prob,lower.tail = F)
    band=subset(df,x>quantile)
    } else if (param$side =='L') {
    quantile = qbinom(param$p,size=param$size,prob=param$prob,lower.tail = T)
    band=subset(df,x<=quantile)
    } else {
    print("Invalid side choice")
    }

    output = as.data.frame(quantile)
    
    # Select data.frame to be sent to the output Dataset port
    maml.mapOutputPort("output");


###<a name="binomial-distribution-probability-calculator"></a>Binomiale verdeling kans Rekenmachine

![Werkruimte maken][5]

####<a name="module-1"></a>Module 1:

    #data schema with example data (replaced with data from web service)
    data.set=data.frame(q=5,size=10,prob=.5,side='L');
    maml.mapOutputPort("data.set"); #send data to output port


####<a name="module-2"></a>Module 2:
    dataset1 <- maml.mapInputPort(1) # class: data.frame
    param = dataset1
    prob = pbinom(param$q,size=param$size,prob=param$prob)
    prob.eq = dbinom(param$q,size=param$size,prob=param$prob)
    df = data.frame(x=1:param$size, prob=dbinom(1:param$size, param$size, prob=param$prob))
    prob

    if (param$side == 'U'){
    prob = 1 - prob
    band=subset(df,x>param$q)
    } else if (param$side =='E') {
    prob = prob.eq
    band=subset(df,x==param$q)
    } else if (param$side =='L') {
    prob = prob
    band=subset(df,x<=param$q)
    } else {
    print("Invalid side choice")
    }

    output = as.data.frame(prob)
    
    # Select data.frame to be sent to the output Dataset port
    maml.mapOutputPort("output");

###<a name="binomial-distribution-generator"></a>Binomiale verdeling genereren

![Werkruimte maken][6]

####<a name="module-1"></a>Module 1:

    #data schema with example data (replaced with data from web service)
    data.set=data.frame(n=50,size=10,p=.5);
    maml.mapOutputPort("data.set"); #send data to output port

####<a name="module-2"></a>Module 2:
    dataset1 <- maml.mapInputPort(1) # class: data.frame
    param = dataset1
    dist = rbinom(param$n,param$size,param$p)

    output = as.data.frame(t(dist))
    
    # Select data.frame to be sent to the output Dataset port
    maml.mapOutputPort("output");

##<a name="limitations"></a>Beperkingen 
Dit zijn zeer eenvoudige voorbeelden rond de binomiale verdeling. Zoals u kunt zien van het bovenstaande voorbeeldcode, wordt weinig fout vangen geïmplementeerd.

##<a name="faq"></a>FAQ
Zie voor veelgestelde vragen over verbruik van de webservice of publiceren naar de Azure Marketplace [hier](machine-learning-marketplace-faq.md).


[1]: ./media/machine-learning-r-csharp-binomial-distribution/binomial_1.png

[2]: ./media/machine-learning-r-csharp-binomial-distribution/binomial_2.png

[3]: ./media/machine-learning-r-csharp-binomial-distribution/binomial_3.png

[4]: ./media/machine-learning-r-csharp-binomial-distribution/binomial_4.png

[5]: ./media/machine-learning-r-csharp-binomial-distribution/binomial_5.png

[6]: ./media/machine-learning-r-csharp-binomial-distribution/binomial_6.png
 
