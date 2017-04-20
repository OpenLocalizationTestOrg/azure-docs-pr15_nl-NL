<properties 
    pageTitle="Normale standaardverdeling Web Service Suite | Microsoft Azure" 
    description="Normale standaardverdeling Web Service Suite" 
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

#<a name="normal-distribution-suite"></a>Normale standaardverdeling Suite



De normale standaardverdeling-Suite is een verzameling steekproef-webservices ([genereren]( https://datamarket.azure.com/dataset/aml_labs/ndg7), [Kwantiel Rekenmachine]( https://datamarket.azure.com/dataset/aml_labs/ndq5), [Kans Rekenmachine]( https://datamarket.azure.com/dataset/aml_labs/ndp5)) die bij het oplossen van genereren en afhandelen normaal onderzoeken. De services kunnen een normale standaardverdeling reeks lengte, quantiles uit een bepaalde waarschijnlijkheid berekenen en berekenen van de kans van een bepaald kwantiel genereren. Elk van de services verschillende uitvoer op basis van de geselecteerde service genereert (Zie onderstaande beschrijving). De normale standaardverdeling-Suite is gebaseerd op de R functies qnorm, rnorm en pnorm, die van R stat-pakket uitmaken deel.


[AZURE.INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

>Deze webservice kan worden gebruikt door gebruikers – mogelijk via een mobiele app, via een website of zelfs op een lokale computer, bijvoorbeeld. Maar het doel van de web-service is ook als een voorbeeld van hoe Azure Machine Learning kan worden gebruikt om te maken van webservices boven op R code moet fungeren. Met een paar programmacoderegels en R en klikken van een knop binnen Azure Machine Learning Studio, worden een experiment gemaakt met R-code en als een webservice hebt gepubliceerd. De webservice kan vervolgens worden gepubliceerd naar de Azure Marketplace en verbruikt door gebruikers en apparaten kant van de wereld zitten zonder infrastructuur-instellingen van de auteur van de webservice.  
 

##<a name="consumption-of-web-service"></a>Verbruik van webservice
De normale standaardverdeling-Suite bevat de volgende 3 services.

###<a name="normal-distribution-quantile-calculator"></a>Normale standaardverdeling kwantiel Rekenmachine
Deze service 4 argumenten van een normale verdeling geaccepteerd en de bijbehorende kwantiel berekend.

De invoer argumenten zijn:

* p - een enkel kans van een gebeurtenis met normale verdeling. 
* Gemiddelde - het gemiddelde van de normale standaardverdeling.
* SD - de standaarddeviatie van de normale standaardverdeling. 
* Kant - L voor het onderste gedeelte van de verdeling en U voor de bovenzijde van de verdeling.

De uitvoer van de service is de berekende kwantiel die is gekoppeld aan de opgegeven kans.

###<a name="normal-distribution-probability-calculator"></a>Normale standaardverdeling kans Rekenmachine
Deze service 4 argumenten van een normale verdeling en de bijbehorende kans berekend.

De invoer argumenten zijn:

* q - een enkel kwantiel van een gebeurtenis met normale verdeling. 
* Gemiddelde - het gemiddelde van de normale standaardverdeling.
* SD - de standaarddeviatie van de normale standaardverdeling. 
* Kant - L voor het onderste gedeelte van de verdeling en U voor de bovenzijde van de verdeling.

De uitvoer van de service is de berekende kans dat is gekoppeld aan de opgegeven kwantiel.

###<a name="normal-distribution-generator"></a>Normale standaardverdeling genereren
Deze service 3 argumenten van een normale verdeling geaccepteerd en genereert een willekeurige volgorde van de getallen die zijn normaal wordt verdeeld. De volgende argumenten dienen ernaar binnen het verzoek:

* n - het aantal waarnemingen. 
* gemiddelde - het gemiddelde van de normale standaardverdeling.
* SD - de standaarddeviatie van de normale standaardverdeling. 

De uitvoer van de service is een reeks lengte n met een normale verdeling op basis van de argumenten gemiddelde en sd.

>Deze service, is zoals die worden gehost op de Azure Marketplace, een OData-dienst. Deze mag worden aangeroepen via de POST of GET-methoden. 

Er zijn verschillende manieren van het gebruik van de service geautomatiseerd voortgezet (voorbeeld apps zijn hier: [genereren](http://microsoftazuremachinelearning.azurewebsites.net/NormalDistributionGenerator.aspx), [Kans Rekenmachine](http://microsoftazuremachinelearning.azurewebsites.net/NormalDistributionProbabilityCalculator.aspx), [Kwantiel Rekenmachine](http://microsoftazuremachinelearning.azurewebsites.net/NormalDistributionQuantileCalculator.aspx)).

###<a name="starting-c-code-for-web-service-consumption"></a>C#-code voor verbruik van web-service starten:

###<a name="normal-distribution-quantile-calculator"></a>Normale standaardverdeling kwantiel Rekenmachine
    public class Input
    {
            public string p;
            public string mean;
            public string sd;
            public string side;
    }
    
    public AuthenticationHeaderValue CreateBasicHeader(string username, string password)
    {
            byte[] byteArray = System.Text.Encoding.UTF8.GetBytes(username + ":" + password);
            return new AuthenticationHeaderValue("Basic", Convert.ToBase64String(byteArray));
    }
    
    void Main()
    {
            var input = new Input() { p = TextBox1.Text, mean = TextBox2.Text, sd = TextBox3.Text, side = TextBox4.Text };
            var json = JsonConvert.SerializeObject(input);
            var acitionUri = "PutAPIURLHere,e.g.https://api.datamarket.azure.com/..../v1/Score";
            var httpClient = new HttpClient();
    
            httpClient.DefaultRequestHeaders.Authorization = CreateBasicHeader("PutEmailAddressHere", "ChangeToAPIKey");
            httpClient.DefaultRequestHeaders.Accept.Add(new MediaTypeWithQualityHeaderValue("application/json"));
    
            var response = httpClient.PostAsync(acitionUri, new StringContent(json));
            var result = response.Result.Content;
            var scoreResult = result.ReadAsStringAsync().Result;
    }


###<a name="normal-distribution-probability-calculator"></a>Normale standaardverdeling kans Rekenmachine
    public class Input
    {
            public string q;
            public string mean;
            public string sd;
            public string side;
    }
    
    public AuthenticationHeaderValue CreateBasicHeader(string username, string password)
    {
            byte[] byteArray = System.Text.Encoding.UTF8.GetBytes(username + ":" + password);
            return new AuthenticationHeaderValue("Basic", Convert.ToBase64String(byteArray));
    }
    
    void Main()
    {
            var input = new Input() { q = TextBox1.Text, mean = TextBox2.Text, sd = TextBox3.Text, side = TextBox4.Text };
            var json = JsonConvert.SerializeObject(input);
            var acitionUri = "PutAPIURLHere,e.g.https://api.datamarket.azure.com/..../v1/Score";
            var httpClient = new HttpClient();
    
            httpClient.DefaultRequestHeaders.Authorization = CreateBasicHeader("PutEmailAddressHere", "ChangeToAPIKey");
            httpClient.DefaultRequestHeaders.Accept.Add(new MediaTypeWithQualityHeaderValue("application/json"));
    
            var response = httpClient.PostAsync(acitionUri, new StringContent(json));
            var result = response.Result.Content;
            var scoreResult = result.ReadAsStringAsync().Result;
    }

###<a name="normal-distribution-generator"></a>Normale standaardverdeling genereren
    public class Input
    {
            public string n;
            public string mean;
            public string sd;
    }
    
    public AuthenticationHeaderValue CreateBasicHeader(string username, string password)
    {
            byte[] byteArray = System.Text.Encoding.UTF8.GetBytes(username + ":" + password);
            return new AuthenticationHeaderValue("Basic", Convert.ToBase64String(byteArray));
    }
    
    void Main()
    {
            var input = new Input() { n = TextBox1.Text, mean = TextBox2.Text, sd = TextBox3.Text };
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
>Deze webservice is gemaakt met Azure Machine Learning. Voor een gratis proefversie, evenals inleidende video's over het maken van experimenten en [publicerende webservices](machine-learning-publish-a-machine-learning-web-service.md), raadpleegt u [azure.com/ml](http://azure.com/ml). 

Hieronder vindt u een schermafbeelding van het experiment die de web-service en voorbeeld-code voor elk van de modules binnen het experiment hebt gemaakt.

###<a name="normal-distribution-quantile-calculator"></a>Normale standaardverdeling kwantiel Rekenmachine

Experiment stroom:

![Stroom van experiment][2]
 
    #Data schema with example data (replaced with data from web service)
    data.set=data.frame(p=0.1,mean=0,sd=1,side='L');
    maml.mapOutputPort("data.set"); #send data to output port
    
    # Map 1-based optional input ports to variables
    dataset1 <- maml.mapInputPort(1) # class: data.frame

    param = dataset1
    if (param$p < 0 ) {
    print('Bad input: p must be between 0 and 1')
    param$p = 0
    } else if (param$p > 1) {
    print('Bad input: p must be between 0 and 1')
    param$p = 1
    }
    q = qnorm(param$p,mean=param$mean,sd=param$sd)

    if (param$side == 'U'){
    q = 2* param$mean - q
    } else if (param$side =='L') {
    q = q
    } else {
    print("Invalid side choice")
    }

    output = as.data.frame(q)
    
    # Select data.frame to be sent to the output Dataset port
    maml.mapOutputPort("output");
    
###<a name="normal-distribution-probability-calculator"></a>Normale standaardverdeling kans Rekenmachine
Experiment stroom:

![Stroom van experiment][3]
 
    #Data schema with example data (replaced with data from web service)
    data.set=data.frame(q=-1,mean=0,sd=1,side='L');
    maml.mapOutputPort("data.set"); #send data to output port
    
    # Map 1-based optional input ports to variables
    dataset1 <- maml.mapInputPort(1) # class: data.frame

    param = dataset1
    prob = pnorm(param$q,mean=param$mean,sd=param$sd)

    if (param$side == 'U'){
    prob = 1 - prob
    } else if (param$side =='B') {
    prob = ifelse(prob<=0.5,prob * 2, 1)
    } else if (param$side =='L') {
    prob = prob
    } else {
    print("Invalid side choice")
    }

    output = as.data.frame(prob)
    
    # Select data.frame to be sent to the output Dataset port
    maml.mapOutputPort("output");
    
###<a name="normal-distribution-generator"></a>Normale standaardverdeling genereren
Experiment stroom:

![Stroom van experiment][4]

    #Data schema with example data (replaced with data from web service)
    data.set=data.frame(n=50,mean=0,sd=1);
    maml.mapOutputPort("data.set"); #send data to output port
    
    # Map 1-based optional input ports to variables
    dataset1 <- maml.mapInputPort(1) # class: data.frame

    param = dataset1
    dist = rnorm(param$n,mean=param$mean,sd=param$sd)

    output = as.data.frame(t(dist))

    # Select data.frame to be sent to the output Dataset port
    maml.mapOutputPort("output");

##<a name="limitations"></a>Beperkingen 

Dit zijn zeer eenvoudige voorbeelden rond de normale verdeling. Zoals u kunt zien van het bovenstaande voorbeeldcode, wordt weinig fout vangen geïmplementeerd.

##<a name="faq"></a>FAQ
Zie voor veelgestelde vragen over verbruik van de webservice of publiceren naar de Azure Marketplace [hier](machine-learning-marketplace-faq.md).

[1]: ./media/machine-learning-r-csharp-normal-distribution/normal-img1.png
[2]: ./media/machine-learning-r-csharp-normal-distribution/normal-img2.png
[3]: ./media/machine-learning-r-csharp-normal-distribution/normal-img3.png
[4]: ./media/machine-learning-r-csharp-normal-distribution/normal-img4.png
 
