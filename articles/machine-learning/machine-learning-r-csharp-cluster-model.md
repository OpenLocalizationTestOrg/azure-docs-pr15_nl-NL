<properties 
    pageTitle="Cluster Model | Microsoft Azure" 
    description="Clustermodel" 
    services="machine-learning" 
    documentationCenter="" 
    authors="FrancescaLazzeri" 
    manager="jhubbard" 
    editor="cgronlun"/>

<tags 
    ms.service="machine-learning" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="10/17/2016" 
    ms.author="lazzeri"/> 


#<a name="cluster-model"></a>Clustermodel    

Hoe kunnen we groepen van de creditcard kaarthouders gedrag voorspellen om te kunnen het risico boete uitschakelen creditcard uitgevers beperken? Hoe kunnen we groepen van karakter kenmerken van werknemers definiëren om te verbeteren de prestaties op het werk? Hoe kunnen artsen patiënten classificeren in groepen op basis van de kenmerken van hun ziekten? In principe, kunnen alle van deze vragen worden beantwoord tot en met cluster analyse.   


[AZURE.INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)] 
   
Cluster analyse classificeert een reeks observaties in twee of meer elkaar wederzijds uitsluiten onbekend groepen op basis van de combinaties van variabelen. Het doel van cluster analyse is te vinden van een systeem van observaties, meestal personen of hun eigenschappen, ordenen in groepen, waar leden van de groepen eigenschappen gemeen delen. Deze [service](https://datamarket.azure.com/dataset/aml_labs/k_cluster_model) gebruikt de methodologie van het K-geeft, een veelgebruikte clustering techniek, met cluster willekeurige gegevens in groepen. Deze webservice duurt de gegevens en het aantal k clusters als invoer en genereert voorspellingen van welke van het k-groepen waartoe elke observaties behoort. 

>Deze webservice kan worden gebruikt door gebruikers – mogelijk via een mobiele app, via een website of zelfs op een lokale computer, bijvoorbeeld. Maar het doel van de web-service is ook als een voorbeeld van hoe Azure Machine Learning kan worden gebruikt om te maken van webservices boven op R code moet fungeren. Met een paar programmacoderegels en R en klikken van een knop binnen Azure Machine Learning Studio, worden een experiment gemaakt met R-code en als een webservice hebt gepubliceerd. De webservice kan vervolgens worden gepubliceerd naar de Azure Marketplace en verbruikt door gebruikers en apparaten kant van de wereld zitten zonder infrastructuur-instellingen van de auteur van de webservice.  

##<a name="consumption-of-web-service"></a>Verbruik van webservice   
Deze webservice waarmee de gegevens gegroepeerd in een set k groepen en Hiermee kunt u de toewijzing aan een groep voor elke rij. De webservice verwacht de eindgebruiker gegevens invoeren als een tekenreeks waar rijen worden gescheiden door een komma (,) en kolommen worden gescheiden door een puntkomma (;). De webservice verwacht 1 rij per keer. Een voorbeeld dataset kan er als volgt uit:

![Voorbeeldgegevens][1]

Stel dat de gebruiker wilt scheiden van deze gegevens in 3 elkaar wederzijds uitsluiten groepen. De gegevensinvoer voor de bovenstaande gegevensset zou de volgende handelingen uit: waarde = "10; 5; 2,18; 1; 6,7; 5; 5,22; 3; 4,12; 2; 1,10; 3; 4"; k = "3". De uitvoer is de voorspelde groepslidmaatschap voor elk van de rijen.

>Deze service, is zoals die worden gehost op de Azure Marketplace, een OData-dienst. Deze mag worden aangeroepen via de POST of GET-methoden. 

Er zijn verschillende manieren van het gebruik van de service geautomatiseerd voortgezet (een voorbeeld-app is [hier](http://microsoftazuremachinelearning.azurewebsites.net/ClusterModel.aspx )).

###<a name="starting-c-code-for-web-service-consumption"></a>C#-code voor verbruik van web-service starten:

    public class Input
    {
            public string value;
            public string k;
    }
    
    public AuthenticationHeaderValue CreateBasicHeader(string username, string password)
    {
            byte[] byteArray = System.Text.Encoding.UTF8.GetBytes(username + ":" + password);
            return new AuthenticationHeaderValue("Basic", Convert.ToBase64String(byteArray));
    }
    
    void Main()
    {
            var input = new Input() { value = TextBox1.Text, k = TextBox2.Text };
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

Van binnen Azure Machine training, een nieuwe lege experiment is gemaakt en twee [R-Script uitvoeren] [ execute-r-script] modules binnengehaald naar de werkruimte. Het gegevensschema is gemaakt met een eenvoudig [R-Script uitvoeren][execute-r-script]. Vervolgens het gegevensschema is gekoppeld aan de sectie cluster model, opnieuw gemaakt met een [R-Script uitvoeren][execute-r-script]. In het [R-Script uitvoeren] [ execute-r-script] gebruikt voor het clustermodel, de webservice vervolgens maakt gebruik van de functie 'k-geeft', dat wil vooraf gedefinieerde in de [R-Script uitvoeren zeggen] [ execute-r-script] van Azure Machine Learning.    
   

     
![Stroom van experiment][3]

####<a name="module-1"></a>Module 1: 
    #Enter the input data as a string 
    mydata <- data.frame(value = "1; 3; 5; 6; 7; 7, 5; 5; 6; 7; 2; 1, 3; 7; 2; 9; 56; 6, 1; 4; 5; 26; 4; 23, 15; 35; 6; 7; 12; 1, 32; 51; 62; 7; 21; 1", k=5, stringsAsFactors=FALSE)
    
    maml.mapOutputPort("mydata");     
    

####<a name="module-2"></a>Module 2:
    # Map 1-based optional input ports to variables
    mydata <- maml.mapInputPort(1) # class: data.frame

    data.split <- strsplit(mydata[1,1], ",")[[1]]
    data.split <- sapply(data.split, strsplit, ";", simplify = TRUE)
    data.split <- sapply(data.split, strsplit, ";", simplify = TRUE)
    data.split <- as.data.frame(t(data.split))

    data.split <- data.matrix(data.split)
    data.split <- data.frame(data.split)

    # K-Means cluster analysis
    fit <- kmeans(data.split, mydata$k) # k-cluster solution

    # Get cluster means 
    aggregate(data.split,by=list(fit$cluster),FUN=mean)
    # Append cluster assignment
    mydatafinal <- data.frame(t(fit$cluster))
    n_col=ncol(mydatafinal)
    colnames(mydatafinal) <- paste("V",1:n_col,sep="")

    # Select data.frame to be sent to the output Dataset port
    maml.mapOutputPort("mydatafinal");
   
 
##<a name="limitations"></a>Beperkingen
Dit is een zeer eenvoudige voorbeeld van een webservice. Zoals u kunt zien van het bovenstaande voorbeeldcode, geen fout vangen is geïmplementeerd en wordt de service wordt ervan uitgegaan dat alles is een doorlopend variabele (geen bestaan functies toegestaan), als de service alleen invoeritems numerieke waarden op het moment van het maken van deze webservice. Bovendien omgaat de service momenteel met beperkte gegevensgrootte, vervaldatum naar de aanvraag en respons aard van de web-service-oproep en het feit dat het model is wordt aanpassen aan elke keer dat de webservice wordt genoemd. 

##<a name="faq"></a>FAQ
Zie voor veelgestelde vragen over verbruik van de webservice of publiceren naar de Azure Marketplace [hier](machine-learning-marketplace-faq.md).

[1]: ./media/machine-learning-r-csharp-cluster-model/cluster-img1.png
[2]: ./media/machine-learning-r-csharp-cluster-model/cluster-img2.png
[3]: ./media/machine-learning-r-csharp-cluster-model/cluster-img3.png


<!-- Module References -->
[execute-r-script]: https://msdn.microsoft.com/library/azure/30806023-392b-42e0-94d6-6b775a6e0fd5/
 
