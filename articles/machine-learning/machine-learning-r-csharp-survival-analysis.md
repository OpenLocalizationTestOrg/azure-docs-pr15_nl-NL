<properties 
    pageTitle="Voor analyse met Azure Machine Learning | Microsoft Azure" 
    description="Voor analyse gebeurtenis exemplaar kans" 
    services="machine-learning" 
    documentationCenter="" 
    authors="zhangya" 
    manager="jhubbard" 
    editor="cgronlun"/>

<tags 
    ms.service="machine-learning" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/21/2016" 
    ms.author="zhangya"/> 


#<a name="survival-analysis"></a>Voor analyse 

Onder veel scenario's is het belangrijkste resultaat onder assessment de tijd op een gebeurtenis belangrijke. Met andere woorden, de vraag "wanneer deze gebeurtenis plaatsvindt?" wordt gevraagd. Voorbeelden, kunt u beter situaties waarin de gegevens de verstreken tijd (dagen, jaren, reiskosten, enzovoort worden) tot de gebeurtenis belangrijke (ziekte relapse, PhD mate ontvangen, werking toetsenblok is mislukt) plaatsvindt. Elk exemplaar in de gegevens staat voor een bepaald object (een geduld, een leerling/student, een auto, enzovoort).


[AZURE.INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

Deze [webservice]( https://datamarket.azure.com/dataset/aml_labs/survivalanalysis) vindt u antwoorden op de vraag "Wat is de kans dat de gebeurtenis belangrijke bij het tijd n voor object x? optreden" Deze webservice kunnen doordat een model van de analyse voor gebruikers moeten opgeven om het model trainen en test deze. Het onderwerp van het experiment is de lengte van de verstreken tijd model totdat de belangrijke gebeurtenis. 

>Deze webservice kan worden gebruikt door gebruikers – mogelijk via een mobiele app, via een website of zelfs op een lokale computer, bijvoorbeeld. Maar het doel van de web-service is ook als een voorbeeld van hoe Azure Machine Learning kan worden gebruikt om te maken van webservices boven op R code moet fungeren. Met een paar programmacoderegels en R en klikken van een knop binnen Azure Machine Learning Studio, worden een experiment gemaakt met R-code en als een webservice hebt gepubliceerd. De webservice kan vervolgens worden gepubliceerd naar de Azure Marketplace en verbruikt door gebruikers en apparaten kant van de wereld zitten zonder infrastructuur-instellingen van de auteur van de webservice.  

##<a name="consumption-of-web-service"></a>Verbruik van webservice

Het schema invoergegevens van de webservice wordt weergegeven in de volgende tabel. Zes soorten informatie nodig zijn als invoer: training gegevens, gegevens testen, tijd van belang, de index van "time" dimensie, de index van "gebeurtenis" dimensie en de typen variabelen (doorlopend of factor). De trainingsgegevens wordt weergegeven met een tekenreeks, waarbij de rijen worden gescheiden door komma en de kolommen worden gescheiden door puntkomma. Het aantal functies van de gegevens is flexibel. Alle elementen in de ingevoerde tekenreeks moet numeriek zijn. In de trainingsgegevens geeft de dimensie "time" het aantal eenheden (dagen, jaren, reiskosten, enzovoort) is verstreken sinds het beginpunt van de studie (een geduld ontvangen medicijnen behandeling-programma's, een leerling/student begin PhD studie, een auto starten om te worden gestuurd, enz.) tot de gebeurtenis belangrijke (de geduld terugkeren naar medicijnen gebruik, de studenten het verkrijgen van de mate PhD, de auto ondervindt werking toetsenblok is mislukt enz.) Deze gebeurtenis vindt plaats. De dimensie 'gebeurtenis' wordt aangegeven of de belangrijke gebeurtenis aan het einde van het onderzoek. Een waarde van "gebeurtenis = 1" betekent dat de gebeurtenis belangrijke op het moment dat wordt aangegeven door de dimensie "time"; "gebeurtenis = 0 ' betekent dat de gebeurtenis belangrijke niet door de tijd die wordt aangegeven door de dimensie"time"heeft plaatsgevonden.

- trainingdata - een tekenreeks. Rijen worden gescheiden door komma en kolommen worden gescheiden door puntkomma. Elke rij bevat "tijddimensie", "gebeurtenis" dimensie en voorspelde variabelen.
- testingdata - één rij met gegevens die voorspelde variabelen voor een bepaald object bevat.
- time_of_interest - de verstreken tijd van rente n.
- index_time - de kolomindex van de "time" dimensie (1 vanaf).
- index_event - de kolomindex van de 'gebeurtenis' dimensie (1 vanaf).
- variable_types - een tekenreeks met puntkomma's als scheidingstekens in deze. 0 vertegenwoordigt continue variabelen en 1 factor variabelen.


De uitvoer is de kans van een gebeurtenis plaatsvindt door een bepaalde tijd. 

>Deze service, is zoals die worden gehost op de Azure Marketplace, een OData-dienst. Deze mag worden aangeroepen via de POST of GET-methoden. 

Er zijn verschillende manieren van het gebruik van de service geautomatiseerd voortgezet (een voorbeeld-app is [hier](http://microsoftazuremachinelearning.azurewebsites.net/SurvivalAnalysis.aspx)). 

###<a name="starting-c-code-for-web-service-consumption"></a>C#-code voor verbruik van web-service starten:
    public class Input
    {
            public string trainingdata;
            public string testingdata;
            public string timeofinterest;
            public string indextime;
            public string indexevent;
            public string variabletypes;
    }

    public AuthenticationHeaderValue CreateBasicHeader(string username, string password)
    {
            byte[] byteArray = System.Text.Encoding.UTF8.GetBytes(username + ":" + password);
            return new AuthenticationHeaderValue("Basic", Convert.ToBase64String(byteArray));
    }
    
    void Main()
    {
            var input = new Input() { trainingdata = TextBox1.Text, testingdata = TextBox2.Text, timeofinterest = TextBox3.Text, indextime = TextBox4.Text, indexevent = TextBox5.Text, variabletypes = TextBox6.Text };
            var json = JsonConvert.SerializeObject(input);
            var acitionUri = "PutAPIURLHere,e.g.https://api.datamarket.azure.com/..../v1/Score";
            var httpClient = new HttpClient();
    
            httpClient.DefaultRequestHeaders.Authorization = CreateBasicHeader("PutEmailAddressHere", "ChangeToAPIKey");
            httpClient.DefaultRequestHeaders.Accept.Add(new MediaTypeWithQualityHeaderValue("application/json"));
    
            var response = httpClient.PostAsync(acitionUri, new StringContent(json));
            var result = response.Result.Content;
            var scoreResult = result.ReadAsStringAsync().Result;
    }




De interpretatie van deze toets wordt als volgt bepaald. Als het doel van de gegevens is om de verstreken tijd totdat de terug naar medicijnen gebruik voor de patiënten die een van de twee behandeling-programma's hebt ontvangen. De uitvoer web service gelezen: voor patiënten die wordt 35 jaar oud, met vorige productie medicijn behandeling 2 maal, duurt het programma lang huis behandeling en met zowel heroïne als cocaïne gebruik, de kans van terugkeren naar de gebruik medicijnen 95.64% van dag 500 is.

##<a name="creation-of-web-service"></a>Het maken van een webservice

>Deze webservice is gemaakt met Azure Machine Learning. Voor een gratis proefversie, evenals inleidende video's over het maken van experimenten en [publicerende webservices](machine-learning-publish-a-machine-learning-web-service.md), raadpleegt u [azure.com/ml](http://azure.com/ml). Hieronder vindt u een schermafbeelding van het experiment die de web-service en voorbeeld-code voor elk van de modules binnen het experiment hebt gemaakt.

Van binnen Azure Machine training, een nieuwe lege experiment is gemaakt en twee [R-Script uitvoeren] [ execute-r-script] modules zijn verzameld naar de werkruimte. Het gegevensschema is gemaakt met een eenvoudig [R-Script uitvoeren][execute-r-script], waarin het schema invoergegevens voor de webservice definieert. Deze module vervolgens is gekoppeld aan het tweede [R-Script uitvoeren] [ execute-r-script] module, van werk belangrijke. Deze module biedt voorverwerking van gegevens, model bouwen en voorspellingen. In de stap gegevens de voorverwerking, is de invoergegevens voorgesteld door een lange tekenreeks getransformeerd en dat is geconverteerd naar een gegevensframe. Het pakket van een externe R "survival_2.37-7.zip" is eerst stap in het model building geïnstalleerd voor de uitvoering van voor analyse. De functie "coxph" wordt vervolgens uitgevoerd na een reeks gegevensverwerking taken. De details van de functie "coxph" voor voor analyse kunnen worden gelezen uit de documentatie R. In de stap koppeling de optie tekstvoorspelling een testen exemplaar wordt ingevoerd in het model ervaren met de functie 'surfit' en de kromme voor voor dit testen exemplaar wordt gegenereerd als "curve" variabele. Ten slotte worden de kans van de tijd van belang opgehaald. 

###<a name="experiment-flow"></a>Experiment stroom:

![stroom experimenteren][1]

####<a name="module-1"></a>Module 1:

    #Data schema with example data (replaced with data from web service)
    trainingdata="53;1;29;0;0;3,79;1;34;0;1;2,45;1;27;0;1;1,37;1;24;0;1;1,122;1;30;0;1;1,655;0;41;0;0;1,166;1;30;0;0;3,227;1;29;0;0;3,805;0;30;0;0;1,104;1;24;0;0;1,90;1;32;0;0;1,373;1;26;0;0;1,70;1;36;0;0;1”
    testingdata="35;2;1;1"
    time_of_interest="500"
    index_time="1"
    index_event="2"
    
    # 0 - continuous; 1 -  factor
    variable_types="0;0;1;1"

    sampleInput=data.frame(trainingdata,testingdata,time_of_interest,index_time,index_event,variable_types)

    maml.mapOutputPort("sampleInput"); #send data to output port
    
####<a name="module-2"></a>Module 2:

    #Read data from input port
    data <- maml.mapInputPort(1) 
    colnames(data) <- c("trainingdata","testingdata","time_of_interest","index_time","index_event","variable_types")

    # Preprocessing training data
    traindingdata=data$trainingdata
    y=strsplit(as.character(data$trainingdata),",")
    n_row=length(unlist(y))
    z=sapply(unlist(y), strsplit, ";", simplify = TRUE)
    mydata <- data.frame(matrix(unlist(z), nrow=n_row, byrow=T), stringsAsFactors=FALSE)
    n_col=ncol(mydata)

    # Preprocessing testing data
    testingdata=as.character(data$testingdata)
    testingdata=unlist(strsplit(testingdata,";"))

    # Preprocessing other input parameters
    time_of_interest=data$time_of_interest
    time_of_interest=as.numeric(as.character(time_of_interest))
    index_time = data$index_time
    index_event = data$index_event
    variable_types = data$variable_types

    # Necessary R packages
    install.packages("src/packages_survival/survival_2.37-7.zip",lib=".",repos=NULL,verbose=TRUE)
    library(survival)

    # Prepare to build model
    attach(mydata)

    for (i in 1:n_col){ mydata[,i]=as.numeric(mydata[,i])} 
    d_time=paste("X",index_time,sep = "")
    d_event=paste("X",index_event,sep = "")
    v_time_event <- c(d_time,d_event)
    v_predictors = names(mydata)[!(names(mydata) %in% v_time_event)]

    variable_types = unlist(strsplit(as.character(variable_types),";"))

    len = length(v_predictors)
    c="" # Construct the execution string
    for (i in 1:len){
    if(i==len){
    if(variable_types[i]!=0){ c=paste(c, "factor(",v_predictors[i],")",sep="")}
     else{ c=paste(c, v_predictors[i])}
    }else{
    if(variable_types[i]!=0){c=paste(c, "factor(",v_predictors[i],") + ",sep="")}
    else{c=paste(c, v_predictors[i],"+")}
    }
    }
    f=paste("coxph(Surv(",d_time,",",d_event,") ~")
    f=paste(f,c)
    f=paste(f,", data=mydata )")

    # Fit a Cox proportional hazards model and get the predicted survival curve for a testing instance 
    fit=eval(parse(text=f))

    testingdata = as.data.frame(matrix(testingdata, ncol=len,byrow = TRUE),stringsAsFactors=FALSE)
    names(testingdata)=v_predictors
    for (i in 1:len){ testingdata[,i]=as.numeric(testingdata[,i])}

    curve=survfit(fit,testingdata)

    # Based on user input, find the event occurrence probability
    position_closest=which.min(abs(prob_event$time - time_of_interest))

    if(prob_event[position_closest,"time"]==time_of_interest){# exact match
    output=prob_event[position_closest,"prob"]
    }else{# not exact match
    if(time_of_interest>max(prob_event$time)){
    output=prob_event[position_closest,"prob"]
    }else if(time_of_interest<min(prob_event$time)){
    output=prob_event[position_closest,"prob"]
    }else{output=(prob_event[position_closest,"prob"]+prob_event[position_closest+1,"prob"])/2}
    }

    #Pull out results to send to web service
    output=paste(round(100*output, 2), "%") 
    maml.mapOutputPort("output"); #output port




##<a name="limitations"></a>Beperkingen

Deze webservice werken alleen numerieke waarden als functie variabelen (kolommen). De kolom "event" kan alleen de waarde 0 of 1 duren. De kolom "time" moet een positief geheel getal.

##<a name="faq"></a>FAQ
Zie voor veelgestelde vragen over verbruik van de webservice of publiceren naar de Azure Marketplace [hier](machine-learning-marketplace-faq.md).

[1]: ./media/machine-learning-r-csharp-survival-analysis/survive_img2.png


<!-- Module References -->
[execute-r-script]: https://msdn.microsoft.com/library/azure/30806023-392b-42e0-94d6-6b775a6e0fd5/
 
