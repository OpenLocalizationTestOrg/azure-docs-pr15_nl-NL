<properties 
    pageTitle="Woordenlijst op basis van Sentiment analyse | Microsoft Azure" 
    description="Woordenlijst op basis van Sentiment analyse" 
    services="machine-learning" 
    documentationCenter="" 
    authors="pengxia" 
    manager="jhubbard" 
    editor="cgronlun"/>

<tags 
    ms.service="machine-learning" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/16/2016" 
    ms.author="pengxia"/> 



#<a name="lexicon-based-sentiment-analysis"></a>Woordenlijst op basis van Sentiment analyse 

Hoe kunt u gebruikers meningen en meten attitudes naar merken of onderwerpen in sociale netwerken, zoals Facebook in berichten, tweets, revisies, enzovoort.? Sentiment analyse biedt een methode voor het analyseren van deze vragen.


[AZURE.INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

Er zijn meestal twee methoden voor sentiment analyse. Een een algoritme gecontroleerde onderwijs gebruikt en de andere kan worden behandeld als een werkende onderwijs. Een model classificatie een algoritme gecontroleerde leren over het algemeen gebaseerd op een grote geannoteerde corpus. De nauwkeurigheid voornamelijk is gebaseerd op de kwaliteit van de aantekening en meestal het trainingsproces erg lang duurt. Behalve dat, als we de algoritme van de toepast op een ander domein, het resultaat is meestal niet goed. Vergeleken met toezicht learning, op basis van een woordenlijst werkende learning gebruik een woordenlijst sentiment, waarvoor een corpus grote gegevens opslaan en training niet - waardoor het hele proces sneller. 

Onze [service](https://datamarket.azure.com/dataset/aml_labs/lexicon_based_sentiment_analysis) is gebaseerd op de MPQA subjectieve aspect woordenlijst (http://mpqa.cs.pitt.edu/lexicons/subj_lexicon/), namelijk een van de meest gebruikte subjectieve aspect lexicons. Er zijn 5097 negatieve en 2533 positieve woorden in MPQA. En al deze woorden worden gemarkeerd als hoog of laag polariteit. De hele corpus is onder algemeen GNU openbare licentie. De webservice kan worden toegepast op korte zinnen, zoals tweets en Facebook-berichten. 

>Deze webservice kan worden gebruikt door gebruikers – mogelijk via een mobiele app, via een website of zelfs op een lokale computer bijvoorbeeld. Maar het doel van de web-service is ook als een voorbeeld van hoe Azure Machine Learning kan worden gebruikt om te maken van webservices boven op R code moet fungeren. Met een paar programmacoderegels en R en klikken van een knop binnen Azure Machine Learning Studio, worden een experiment gemaakt met R-code en als een webservice hebt gepubliceerd. De webservice kan vervolgens worden gepubliceerd naar de Azure Marketplace en verbruikt door gebruikers en apparaten kant van de wereld zitten zonder infrastructuur-instellingen van de auteur van de webservice.

##<a name="consumption-of-web-service"></a>Verbruik van webservice

De invoergegevens kan de tekst, maar de webservice beter werkt met korte zinnen. De uitvoer is een numerieke waarde tussen 1 en 1. Elke waarde onder 0 aanduiding dat de sentiment van de tekst negatief; positief als hierboven 0. De absolute waarde van het resultaat geeft de sterkte van de bijbehorende sentiment. 

>Deze service, is zoals die worden gehost op de Azure Marketplace, een OData-dienst. Deze mag worden aangeroepen via de POST of GET-methoden. 

Er zijn verschillende manieren van het gebruik van de service geautomatiseerd voortgezet (een voorbeeld-app is [hier](http://microsoftazuremachinelearning.azurewebsites.net/)).

###<a name="starting-c-code-for-web-service-consumption"></a>C#-code voor verbruik van web-service starten:

    public class ScoreResult
    {
            [DataMember]
            public double result
            {
                get;
                set;
            }
    }

    void main()
    {
            using (var wb = new WebClient())
            {
                var acitionUri = new Uri("PutAPIURLHere,e.g.https://api.datamarket.azure.com/..../v1/Score");
                DataServiceContext ctx = new DataServiceContext(acitionUri);
                var cred = new NetworkCredential("PutEmailAddressHere", "ChangeToAPIKey");
                var cache = new CredentialCache();
    
                cache.Add(acitionUri, "Basic", cred);
                ctx.Credentials = cache;
                var query = ctx.Execute<ScoreResult>(acitionUri, "POST", true, new BodyOperationParameter("Text", TextBox1.Text));
                ScoreResult scoreResult = query.ElementAt(0);
                double result = scoreResult.result;
            }
    }



De invoer is "Vandaag is een goede dag." De uitvoer is '1', waarmee wordt aangegeven een positief sentiment die is gekoppeld aan de invoer zin. 

##<a name="creation-of-web-service"></a>Het maken van een webservice
>Deze webservice is gemaakt met Azure Machine Learning. Voor een gratis proefversie, evenals inleidende video's over het maken van experimenten en [publicerende webservices](machine-learning-publish-a-machine-learning-web-service.md), raadpleegt u [azure.com/ml](http://azure.com/ml). Hieronder vindt u een schermafbeelding van het experiment die de web-service en voorbeeld-code voor elk van de modules binnen het experiment hebt gemaakt.


Van binnen Azure Machine training, is een nieuwe lege experiment gemaakt. De onderstaande afbeelding ziet u de stroom experiment woordenlijst gebaseerde sentiment analyse. Het bestand "sent_dict.csv" is de MPQA subjectieve aspect woordenlijst en is ingesteld als een van de invoer van [R-Script uitvoeren][execute-r-script]. Een andere invoer is een steekproef controleren van de gegevensset voor het controleren van Amazon voor test, waar we selectie, de kolom naam wijzigt, en bewerkingen te splitsen. We een pakket hash gebruiken voor het opslaan van de woordenlijst subjectieve aspect in het geheugen en het proces score berekening versnellen. De hele tekst worden ge? exeerd door 'tm' pakket en vergeleken met het woord in de woordenlijst sentiment. Ten slotte moet een score worden berekend door het gewicht van elk woord dat subjectieve toe te voegen in de tekst. 

###<a name="experiment-flow"></a>Experiment stroom:

![stroom experimenteren][2]


####<a name="module-1"></a>Module 1:
    
    # Map 1-based optional input ports to variables
    sent_dict_data<- maml.mapInputPort(1) # class: data.frame
    dataset2 <- maml.mapInputPort(2) # class: data.frame
 
   # <a name="install-hash-package"></a>Hash-pakket installeren
    install.packages("src/hash_2.2.6.zip", lib = ".", repos = NULL, verbose = TRUE)
    success <- library("hash", lib.loc = ".", logical.return = TRUE, verbose = TRUE)
    library(tm)
    library(stringr)

    #create sentiment dictionary
    negation_word <- c("not","nor", "no")
    result <- c()
    sent_dict <- hash()
    sent_dict <- hash(sent_dict_data$word, sent_dict_data$polarity)

    #  Compute sentiment score for each document
    for (m in 1:nrow(dataset2)){
    polarity_ratio <- 0
    polarity_total <- 0
    not <- 0
    sentence <- tolower(dataset2[m,1])
    if (nchar(sentence) > 0){
        token_array <- scan_tokenizer(sentence)
        for (j in 1:length(token_array)){
            word = str_replace_all(token_array[j], "[^[:alnum:]]", "")
            for (k in 1:length(negation_word)){
              if (word == negation_word[k]){
                not <- (not+1) %% 2

              }
            }
            if (word != ""){
                if (!is.null(sent_dict[[word]])){
                  polarity_ratio <- polarity_ratio + (-2*not+1)*sent_dict[[word]]
                  polarity_total <- polarity_total + abs(sent_dict[[word]])
                }
            }
          
        }
    }
    if (polarity_total > 0){
        result <- c(result, polarity_ratio/polarity_total)
    }else{
        result<- c(result,0)
    }
    }

    # Sample operation
    data.set <- data.frame(result)

    # Select data.frame to be sent to the output Dataset port
    maml.mapOutputPort("data.set")
    


##<a name="limitations"></a>Beperkingen

Vanuit een perspectief algoritme is woordenlijst gebaseerde sentiment analyse een algemene sentiment analysis tool, die mogelijk beter dan de classificatie-methode voor bepaalde velden niet uitvoeren. Negatief maken om het probleem wordt niet goed behandeld. We hardcode woorden met verschillende ontkenning ons programma, maar een betere manier is een woordenlijst ontkenning gebruik en enkele regels maken. De webservice voert beter op korte en eenvoudige zinnen, zoals tweets en Facebook-berichten, dan op lange en ingewikkelde zinnen zoals Amazon beoordelingen. 

##<a name="faq"></a>FAQ
Zie voor veelgestelde vragen over verbruik van de webservice of publiceren naar de Azure Marketplace [hier](machine-learning-marketplace-faq.md).

[1]: ./media/machine-learning-r-csharp-lexicon-based-sentiment-analysis/sentiment_analysis_1.png
[2]: ./media/machine-learning-r-csharp-lexicon-based-sentiment-analysis/sentiment_analysis_2.png


<!-- Module References -->
[execute-r-script]: https://msdn.microsoft.com/library/azure/30806023-392b-42e0-94d6-6b775a6e0fd5/

 
