<properties 
    pageTitle="Multidimensionale lineaire regressie | Microsoft Azure" 
    description="Multidimensionale lineaire regressie" 
    services="machine-learning" 
    documentationCenter="" 
    authors="jaymathe" 
    manager="jhubbard" 
    editor="cgronlun"/>

<tags 
    ms.service="machine-learning" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/14/2016" 
    ms.author="jaymathe"/> 


#<a name="multivariate-linear-regression"></a>Multidimensionale lineaire regressie   
 

 
Stel dat u hebt een gegevensset en wilt snel een afhankelijke variabele (y) voorspellen voor elke gebruiker (i) op basis van onafhankelijke variabelen. Lineaire regressie is een populaire statistische techniek voor deze voorspellingen. Hier wordt de afhankelijke variabele y een doorlopend waarde uitgegaan.  


[AZURE.INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]  

Een eenvoudige scenario mogelijk waar de onderzoeker probeert te voorspellen van het gewicht van een persoon (y) op basis van hun hoogte (x). Een scenario voor geavanceerdere mogelijk waar de onderzoeker vindt u aanvullende informatie voor de desbetreffende persoon (zoals gewicht, geslacht, raceauto) en probeert te voorspellen van het gewicht van de persoon. Deze [webservice]( https://datamarket.azure.com/dataset/aml_labs/multivariate_regression) past het model lineaire regressie tot de gegevens en voert de voorspelde waarde (y) voor elk van de opmerkingen in de gegevens.

>Deze webservice kan worden gebruikt door gebruikers – mogelijk via een mobiele app, via een website of zelfs op een lokale computer, bijvoorbeeld. Maar het doel van de web-service is ook als een voorbeeld van hoe Azure Machine Learning kan worden gebruikt om te maken van webservices boven op R code moet fungeren. Met een paar programmacoderegels en R en klikken van een knop binnen Azure Machine Learning Studio, worden een experiment gemaakt met R-code en als een webservice hebt gepubliceerd. De webservice kan vervolgens worden gepubliceerd naar de Azure Marketplace en verbruikt door gebruikers en apparaten kant van de wereld zitten zonder infrastructuur-instellingen van de auteur van de webservice.  

##<a name="consumption-of-web-service"></a>Verbruik van webservice  
Deze webservice biedt de voorspelde waarden van de afhankelijke variabele op basis van de onafhankelijke variabelen voor alle van de opmerkingen. De webservice verwacht de eindgebruiker gegevens invoeren als een tekenreeks waar rijen worden gescheiden door een komma (,) en kolommen worden gescheiden door een puntkomma (;). De webservice verwacht 1 rij per keer en de eerste kolom worden de afhankelijke variabele verwacht. Een voorbeeld dataset kan er als volgt uit:

![Voorbeeldgegevens][1]

Observaties zonder een afhankelijke variabele moeten worden ingevoerd als 'NA' voor y. De gegevensinvoer voor de bovenstaande gegevensset zou de volgende tekenreeks: "10; 5; 2,18; 1; 6,6; 5.3; 2.1,7; 5; 5,22; 3; 4,12; 2; 1 NB; 3; 4". De uitvoer is de voorspelde waarde voor elk van de rijen op basis van de onafhankelijke variabelen. 

>Deze service, is zoals die worden gehost op de Azure Marketplace, een OData-dienst. Deze mag worden aangeroepen via de POST of GET-methoden. 

Er zijn verschillende manieren van het gebruik van de service geautomatiseerd voortgezet (een voorbeeld-app is [hier](http://microsoftazuremachinelearning.azurewebsites.net/MultipleLinearRegressionService.aspx )).

###<a name="starting-c-code-for-web-service-consumption"></a>C#-code voor verbruik van web-service starten:

    public class Input
    {
            public string value;
    }
    
    public AuthenticationHeaderValue CreateBasicHeader(string username, string password)
    {
            byte[] byteArray = System.Text.Encoding.UTF8.GetBytes(username + ":" + password);
            return new AuthenticationHeaderValue("Basic", Convert.ToBase64String(byteArray));
    }
    
    void Main()
    {
            var input = new Input() { value = TextBox1.Text };
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


Van binnen Azure Machine training, een nieuwe lege experiment is gemaakt en twee [R-Script uitvoeren] [ execute-r-script] modules zijn verzameld naar de werkruimte. Deze webservice wordt uitgevoerd van een Azure Machine Learning-experiment met een onderliggende R-script. Er zijn 2-onderdelen naar dit experiment: schemadefinitie, en training model + scoren. De eerste module Hiermee definieert u de verwachte structuur van de invoer gegevensset, waarbij de eerste variabele de afhankelijke variabele en de resterende variabelen onafhankelijk zijn. De tweede module past een algemene lineaire regressie-model van de invoergegevens.  
  
![Stroom van experiment][3]

####<a name="module-1"></a>Module 1:
 
####<a name="schema-definition"></a>Schemadefinitie  
    data <- data.frame(value = "1;2;3,4;5;6,7;8;9", stringsAsFactors=FALSE) maml.mapOutputPort("data");  

####<a name="module-2"></a>Module 2:
####<a name="lm-modeling"></a>LM modellering   
    data <- maml.mapInputPort(1) # class: data.frame  
  
    data.split <- strsplit(data[1,1], ",")[[1]]  
    data.split <- sapply(data.split, strsplit, ";", simplify = TRUE)  
    data.split <- sapply(data.split, strsplit, ";", simplify = TRUE)  
    data.split <- as.data.frame(t(data.split)) 
    data.split <- data.matrix(data.split) 
    data.split <- data.frame(data.split) 
    model <- lm(data.split)  

    out=data.frame(predict(model,data.split))  
    out <- data.frame(t(out))

    maml.mapOutputPort("out");  
 
##<a name="limitations"></a>Beperkingen
Dit is een zeer eenvoudige voorbeeld van een meerdere lineaire regressie-webservice. Zoals u kunt zien van het bovenstaande voorbeeldcode, geen fout vangen is geïmplementeerd en wordt de service wordt ervan uitgegaan dat alles is een doorlopend variabele (geen bestaan functies toegestaan), als de service alleen invoeritems numerieke waarden op het moment van het maken van deze webservice. Bovendien omgaat de service momenteel met beperkte gegevensgrootte, vervaldatum naar de aanvraag en respons aard van de web-service-oproep en het feit dat het model is wordt aanpassen aan elke keer dat de webservice wordt genoemd. 

##<a name="faq"></a>FAQ
Zie voor veelgestelde vragen over verbruik van de webservice of publiceren naar de Azure Marketplace [hier](machine-learning-marketplace-faq.md).

[1]: ./media/machine-learning-r-csharp-multivariate-linear-regression/multireg-img1.png
[2]: ./media/machine-learning-r-csharp-multivariate-linear-regression/multireg-img2.png
[3]: ./media/machine-learning-r-csharp-multivariate-linear-regression/multireg-img3.png


<!-- Module References -->
[execute-r-script]: https://msdn.microsoft.com/library/azure/30806023-392b-42e0-94d6-6b775a6e0fd5/
 
