<properties 
    pageTitle="Gegevens wetenschappelijke op de Linux wetenschappelijke virtuele Machine gegevens | Microsoft Azure" 
    description="Hoe u verschillende algemene gegevens wetenschappelijke taken met de Linux gegevens wetenschappelijke VM uitvoeren." 
    services="machine-learning"
    documentationCenter="" 
    authors="bradsev" 
    manager="jhubbard" 
    editor="cgronlun"/>

<tags 
    ms.service="machine-learning" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/12/2016" 
    ms.author="bradsev;paulsh" />


# <a name="data-science-on-the-linux-data-science-virtual-machine"></a>Gegevens wetenschappelijke op de Linux wetenschappelijke virtuele Machine gegevens

In dit scenario wordt beschreven hoe u verschillende algemene gegevens wetenschappelijke taken met de Linux gegevens wetenschappelijke VM uitvoeren. De Linux gegevens wetenschappelijke VM (DSVM) is beschikbaar op Azure die is een verzameling hulpprogramma's veelgebruikte voor gegevens analyses en machine learning vooraf geïnstalleerd op de afbeelding van een virtuele machine. De belangrijkste softwareonderdelen zijn gespecificeerde in het onderwerp van de [voorziening de Linux gegevens wetenschappelijke virtuele Machine](machine-learning-data-science-linux-dsvm-intro.md) . De afbeelding VM kunt u gemakkelijk aan de slag gegevens wetenschappelijke in minuten, zonder dat u moet installeren en configureren van elk van de hulpmiddelen voor afzonderlijk doen. U kunt eenvoudig schaal van de VM, indien nodig en stoppen wanneer u niet in gebruik. Deze resource staat dus elastisch en kosten-efficiënte. 

De taken van de wetenschappelijke gegevens is geïllustreerd in deze stapsgewijze instructies volgen de stappen in het [Team gegevens wetenschap proces](https://azure.microsoft.com/documentation/learning-paths/data-science-process/). Dit proces biedt een systematische aanpak voor gegevens wetenschappelijke waardoor teams van gegevens wetenschappers om effectief gedurende de levenscyclus van het bouwen van intelligente toepassingen zijn samen te werken. Het proces van de wetenschappelijke gegevens bevat ook een iteratieve basisstructuur voor gegevens wetenschappelijke die kan worden gevolgd door een persoon.

We analyseren de gegevensset [spambase](https://archive.ics.uci.edu/ml/datasets/spambase) in dit scenario. Dit is een reeks e-mailberichten die zijn gemarkeerd als spam of ham (wat betekent deze zijn niet spam), en bevat ook enkele statistieken over de inhoud van het e-mailberichten. De opgenomen statistieken worden besproken in de volgende maar één sectie. 


## <a name="prerequisites"></a>Vereisten voor

Voordat u een Linux gegevens wetenschappelijke virtuele Machine gebruiken kunt, moet u hebt het volgende:

- Een **Azure-abonnement**. Als u nog een, raadpleegt u [vandaag uw gratis Azure account maken](https://azure.microsoft.com/free/).
- Een [**Linux gegevens wetenschappelijke VM**](https://azure.microsoft.com/marketplace/partners/microsoft-ads/linux-data-science-vm). Zie voor informatie over de inrichting van deze VM, [inrichten de Linux gegevens wetenschappelijke virtuele Machine](machine-learning-data-science-linux-dsvm-intro.md). 
- [X2Go](http://wiki.x2go.org/doku.php) geïnstalleerd op uw computer en een sessie XFCE geopend. Zie voor informatie over het installeren en configureren van een **X2Go-client** [installeren en configureren van X2Go-client](machine-learning-data-science-linux-dsvm-intro.md#Installing-and-configuring-X2Go-client). 
- Een **AzureML-account**. Als u een registreren voor de nieuwe record op de [startpagina van AzureML](https://studio.azureml.net/)nog geen hebt. Er is een gratis gebruik laag om u aan de slag te helpen.


## <a name="download-the-spambase-dataset"></a>Downloaden van de gegevensset spambase

De gegevensset [spambase](https://archive.ics.uci.edu/ml/datasets/spambase) is een kleinere set gegevens alleen 4601 voorbeelden bevat. Dit is een handige grootte wilt gebruiken wanneer u aan te tonen dat enkele van de belangrijkste functies van de gegevens wetenschappelijke VM zoals blijft de resourcevereisten bescheiden.

>[AZURE.NOTE] In dit scenario is gemaakt op een D2 v2 grootte Linux gegevens wetenschappelijke virtuele Machine. Deze grootte DSVM is kan de procedures in dit scenario verwerken.

Als u meer opslagruimte nodig hebt, kunt u extra schijven maken en koppel deze aan uw VM. Deze schijven gebruikt permanente Azure opslag, zodat hun gegevens behouden blijft, zelfs wanneer de server vanwege het formaat is door de service of is afgesloten. Als u wilt toevoegen van een schijf en toevoegen aan uw VM, volgt u de instructies in [een schijf voor een VM Linux toevoegen](../virtual-machines/virtual-machines-linux-add-disk.md). Volgende stappen uit de Azure opdrachtregelinterface (Azure CLI), die al is geïnstalleerd op de DSVM gebruiken. Deze procedures is dus mogelijk helemaal uit de VM zelf. Er is een andere optie om uit te breiden opslag gebruik [Azure-bestanden](../storage/storage-how-to-use-files-linux.md).

Als u wilt downloaden van de gegevens, opent u een terminal-venster en deze opdracht uitvoert:

    wget http://archive.ics.uci.edu/ml/machine-learning-databases/spambase/spambase.data

Het gedownloade bestand heeft geen een veldnamenrij, zodat we een ander bestand die beschikt over een koptekst maken. Deze opdracht om te maken van een bestand met de juiste koppen uitvoeren:

    echo 'word_freq_make, word_freq_address, word_freq_all, word_freq_3d,word_freq_our, word_freq_over, word_freq_remove, word_freq_internet,word_freq_order, word_freq_mail, word_freq_receive, word_freq_will,word_freq_people, word_freq_report, word_freq_addresses, word_freq_free,word_freq_business, word_freq_email, word_freq_you, word_freq_credit,word_freq_your, word_freq_font, word_freq_000, word_freq_money,word_freq_hp, word_freq_hpl, word_freq_george, word_freq_650, word_freq_lab,word_freq_labs, word_freq_telnet, word_freq_857, word_freq_data,word_freq_415, word_freq_85, word_freq_technology, word_freq_1999,word_freq_parts, word_freq_pm, word_freq_direct, word_freq_cs, word_freq_meeting,word_freq_original, word_freq_project, word_freq_re, word_freq_edu,word_freq_table, word_freq_conference, char_freq_semicolon, char_freq_leftParen,char_freq_leftBracket, char_freq_exclamation, char_freq_dollar, char_freq_pound, capital_run_length_average,capital_run_length_longest, capital_run_length_total, spam' > headers

De twee bestanden die samen met de opdracht daarna samengevoegd:

    cat spambase.data >> headers
    mv headers spambaseHeaders.data

De gegevensset heeft verschillende soorten statistieken op elke e-mailbericht: 

- Kolommen zoals ***word\_freq\_WORD*** geven het percentage van woorden in de e-mail die overeenkomen met *WORD*. Bijvoorbeeld als *word\_freq\_zorg* is van 1, en vervolgens 1% van alle woorden in het e-mailbericht *maken zijn*. 
- Kolommen zoals ***teken\_freq\_teken*** geven het percentage van alle tekens in het e-mailbericht die *teken waren*. 
- ***kapitaal\_uitvoeren\_lengte\_langste*** is de langste duur van een reeks hoofdletters. 
- ***kapitaal\_uitvoeren\_lengte\_gemiddelde*** is de gemiddelde lengte van alle reeksen van hoofdletters. 
- ***kapitaal\_uitvoeren\_lengte\_totale*** is de totale duur van alle reeksen van hoofdletters. 
- ***spam*** wordt aangegeven of het e-mailbericht is beschouwd als spam of niet (1 = spam, 0 = niet spam).


## <a name="explore-the-dataset-with-microsoft-r-open"></a>Kennismaken met de gegevensset met Microsoft R openen

Laten we de gegevens controleren en handelingen enkele eenvoudige machine learning-met R. De gegevens wetenschappelijke VM wordt geleverd met [Microsoft R openen](https://mran.revolutionanalytics.com/open/) vooraf geïnstalleerd. De bibliotheken meerdere threads wiskundige in deze versie van R bieden betere prestaties dan verschillende single thread-versies. Microsoft R openen bevat ook reproduceerbaarheid met behulp van een momentopname van de bibliotheek CRAN-pakket.

Als u kopieën van de codevoorbeelden in deze procedure gebruikt, klonen de **Azure-Machine-Learning-gegevens-wetenschappelijke** -bibliotheek met het cijfer, vooraf geïnstalleerd op de VM. Vanaf de opdrachtregel cijfer uitvoeren:

    git clone https://github.com/Azure/Azure-MachineLearning-DataScience.git

Een terminal-venster openen en beginnen met een nieuwe R-sessie met de interactieve R-console.

>[AZURE.NOTE] U kunt ook RStudio gebruiken voor de volgende procedures uit. Als u wilt RStudio hebt geïnstalleerd, Voer u deze opdracht aan een terminal uit:`./Desktop/DSVM\ tools/installRStudio.sh`

Als u wilt de gegevens importeren en stel de omgeving, voert u het:

    data <- read.csv("spambaseHeaders.data")
    set.seed(123)

Samenvatting statistieken over elke kolom bekijken:

    summary(data)

Voor een andere weergave van de gegevens:

    str(data)

Hier ziet u het type elke variabele en de eerste paar waarden in de gegevensset. 

De kolom *spam* als een geheel getal is gelezen, maar het is daadwerkelijk een bestaan variabele (of factor). Het type instellen:

    data$spam <- as.factor(data$spam)

Gebruik het pakket [ggplot2](http://ggplot2.org/) , een populaire grafische bibliotheek voor R die al is geïnstalleerd op de VM sommige experimentele analyse. Houd er rekening mee vanaf de overzichtsgegevens die eerder, wordt weergegeven dat er samenvattingsgegevens op de frequentie van het teken uitroepteken. Laten we eens uitzetten deze frequentie hier met de volgende opdrachten:

    library(ggplot2)
    ggplot(data) + geom_histogram(aes(x=char_freq_exclamation), binwidth=0.25)

Aangezien de nul balk de tekeningen scheeftrekken is, verberg deze:

    email_with_exclamation = data[data$char_freq_exclamation > 0, ]
    ggplot(email_with_exclamation) + geom_histogram(aes(x=char_freq_exclamation), binwidth=0.25)

Er is een niet-alledaagse dichtheid dan 1 waarin u geïnteresseerd. Bekijk alleen die gegevens in:

    ggplot(data[data$char_freq_exclamation > 1, ]) + geom_histogram(aes(x=char_freq_exclamation), binwidth=0.25)

Vervolgens splitsen door spam tegenover ham:

    ggplot(data[data$char_freq_exclamation > 1, ], aes(x=char_freq_exclamation)) +
    geom_density(lty=3) +
    geom_density(aes(fill=spam, colour=spam), alpha=0.55) +
    xlab("spam") +
    ggtitle("Distribution of spam \nby frequency of !") +
    labs(fill="spam", y="Density")

In deze voorbeelden, moeten u soortgelijke curve van de andere kolommen om te verkennen van de gegevens in deze inschakelen.


## <a name="train-and-test-an-ml-model"></a>Trainen en een model ML testen

Nu we een paar machine learning-modellen om te classificeren van het e-mailberichten in de gegevensset als met reeks of ham trainen. We een boomstructuur beslissing en een willekeurig bos-model in deze sectie trainen en test vervolgens de nauwkeurigheid van hun voorspellingen. 

>[AZURE.NOTE] Het rpart (recursieve partitioneren en regressie bomen)-pakket gebruikt in de volgende code is al geïnstalleerd op de gegevens wetenschappelijke VM.


Eerste, laten we de gegevensset splitsen in sets van training en testen:

    rnd <- runif(dim(data)[1])
    trainSet = subset(data, rnd <= 0.7)
    testSet = subset(data, rnd > 0.7)

En maak een beslissingsstructuur om te classificeren van het e-mailberichten.

    require(rpart)
    model.rpart <- rpart(spam ~ ., method = "class", data = trainSet)
    plot(model.rpart)
    text(model.rpart)

Dit is het resultaat:

![1](./media/machine-learning-data-science-linux-dsvm-walkthrough/decision-tree.png)

Om te bepalen hoe goed worden uitgevoerd op de trainingsset, gebruikt u de volgende code:

    trainSetPred <- predict(model.rpart, newdata = trainSet, type = "class")
    t <- table(`Actual Class` = trainSet$spam, `Predicted Class` = trainSetPred)
    accuracy <- sum(diag(t))/sum(t)
    accuracy

Om te bepalen hoe goed worden uitgevoerd op de toets instellen:

    testSetPred <- predict(model.rpart, newdata = testSet, type = "class")
    t <- table(`Actual Class` = testSet$spam, `Predicted Class` = testSetPred)
    accuracy <- sum(diag(t))/sum(t)
    accuracy

Laten we eens proberen ook een willekeurig bos-model. Willekeurige forests een groot aantal beslissingsbomen trainen en uitvoer van een klasse die is de modus van de indeling van alle beslissingsbomen afzonderlijke. Ze bieden een krachtiger machine learning-methode terwijl ze voor het feit van een beslissing structuur model naar een gegevensset training overfit corrigeren. 

    require(randomForest)
    trainVars <- setdiff(colnames(data), 'spam')
    model.rf <- randomForest(x=trainSet[, trainVars], y=trainSet$spam)

    trainSetPred <- predict(model.rf, newdata = trainSet[, trainVars], type = "class")
    table(`Actual Class` = trainSet$spam, `Predicted Class` = trainSetPred)

    testSetPred <- predict(model.rf, newdata = testSet[, trainVars], type = "class")
    t <- table(`Actual Class` = testSet$spam, `Predicted Class` = testSetPred)
    accuracy <- sum(diag(t))/sum(t)
    accuracy


## <a name="deploy-a-model-to-azure-ml"></a>Een model implementeert in Azure ML

[Azure Machine Learning-Studio](https://studio.azureml.net/) (AzureML) is een cloudservice die kunt u heel gemakkelijk bouwen en implementeren van blog analytics-modellen. Een van de nette functies van AzureML is de mogelijkheid om te publiceren van elke functie R als een webservice. Het pakket AzureML R kunt gemakkelijk doen rechtstreeks vanuit onze R-sessie op de DSVM implementatie. 

Als u wilt de beschikking structuur code uit de vorige sectie implementeren, moet u aanmelden bij Azure Machine Learning Studio. Moet u uw werkruimte-ID en een Autorisatietoken naar sigh in. Deze waarden zoeken en de variabelen AzureML met hen geïnitialiseerd:

Selecteer **Instellingen** in het menu van de linkerkant. Houd er rekening mee uw **WERKRUIMTE-ID**. ![2](./media/machine-learning-data-science-linux-dsvm-walkthrough/workspace-id.png)

**Autorisatie Tokens** selecteren in het algemene menu en noteer uw **Primaire autorisatie Token**. ![3](./media/machine-learning-data-science-linux-dsvm-walkthrough/workspace-token.png)

Het pakket **AzureML** laden en stel vervolgens waarden van de variabelen met uw token en werkruimte-ID in uw R-sessie op de DSVM:


    require(AzureML)
    wsAuth = "<authorization-token>"
    wsID = "<workspace-id>"


Laten we het model om deze demo gemakkelijker te implementeren vereenvoudigen. Kies de drie variabelen in de beslissingsstructuur dichtst bevindt bij de hoofdmap en een nieuwe structuur met alleen de drie variabelen maken:

    colNames <- c("char_freq_dollar", "word_freq_remove", "word_freq_hp", "spam")
    smallTrainSet <- trainSet[, colNames]
    smallTestSet <- testSet[, colNames]
    model.rpart <- rpart(spam ~ ., method = "class", data = smallTrainSet)

We nodig een tekstvoorspelling-functie die ervoor zorgt dat de functies als invoer retourneert de voorspelde waarden:

    predictSpam <- function(char_freq_dollar, word_freq_remove, word_freq_hp) {
        predictDF <- predict(model.rpart, data.frame("char_freq_dollar" = char_freq_dollar,
        "word_freq_remove" = word_freq_remove, "word_freq_hp" = word_freq_hp))
        return(colnames(predictDF)[apply(predictDF, 1, which.max)])
    }

De functie predictSpam publiceren naar AzureML met de functie **publishWebService** : 

    spamWebService <- publishWebService("predictSpam",
        "spamWebService",
        list("char_freq_dollar"="float", "word_freq_remove"="float","word_freq_hp"="float"),
        list("spam"="int"),
        wsID, wsAuth)

Deze functie de functie **predictSpam** duurt, Hiermee maakt u een webservice met **spamWebService** met gedefinieerde ingangen en uitvoer van de naam en geeft als resultaat informatie over het nieuwe eindpunt.

Details van de gepubliceerde webservice, met inbegrip van het eindpunt API bekijken en de toegangstoetsen met de opdracht:

    spamWebService[[2]]

Proberen op de eerste 10 rijen van de test instellen:

    consumeDataframe(spamWebService$endpoints[[1]]$PrimaryKey, spamWebService$endpoints[[1]]$ApiLocation, smallTestSet[1:10, 1:3])


## <a name="use-other-tools-available"></a>Andere hulpprogramma's gebruiken

De overige secties aangegeven hoe enkele van de hulpmiddelen op de Linux gegevens wetenschappelijke VM is geïnstalleerd. Hier volgt de lijst met hulpprogramma's besproken:

- XGBoost
- Python
- Jupyterhub
- Rammelaar
- PostgreSQL & eekhoorn SQL
- SQL Server datawarehouse


## <a name="xgboost"></a>XGBoost

[XGBoost](https://xgboost.readthedocs.org/en/latest/) is een functie waarmee u een snel en nauwkeurig gestimuleerd structuur-implementatie.

    require(xgboost)
    data <- read.csv("spambaseHeaders.data")
    set.seed(123)

    rnd <- runif(dim(data)[1])
    trainSet = subset(data, rnd <= 0.7)
    testSet = subset(data, rnd > 0.7)

    bst <- xgboost(data = data.matrix(trainSet[,0:57]), label = trainSet$spam, nthread = 2, nrounds = 2, objective = "binary:logistic")

    pred <- predict(bst, data.matrix(testSet[, 0:57]))
    accuracy <- 1.0 - mean(as.numeric(pred > 0.5) != testSet$spam)
    print(paste("test accuracy = ", accuracy))

XGBoost kunt ook aanroepen vanuit python of een opdrachtregel.

## <a name="python"></a>Python

Voor de ontwikkeling met Python, zijn de onderzoeken Anaconda Python 2.7 en 3.5 in de DSVM geïnstalleerd. 

>[AZURE.NOTE] De verdeling Anaconda bevat [Condas](http://conda.pydata.org/docs/index.html), die kunnen worden gebruikt om het maken van aangepaste omgevingen voor Python die u hebt verschillende versies en/of -pakketten in deze geïnstalleerd.

Laten we eens lezen in enkele van de gegevensset spambase en classificeer van het e-mailberichten met ondersteuning vector machines in scikit-informatie over:

    import pandas
    from sklearn import svm    
    data = pandas.read_csv("spambaseHeaders.data", sep = ',\s*')
    X = data.ix[:, 0:57]
    y = data.ix[:, 57]
    clf = svm.SVC()
    clf.fit(X, y)

Voorspellingen maken:

    clf.predict(X.ix[0:20, :])

Als u wilt zien hoe een eindpunt AzureML publiceren, laten we Maak een eenvoudigere model de drie variabelen zoals toen we het model R eerder zijn gepubliceerd. 

    X = data.ix[["char_freq_dollar", "word_freq_remove", "word_freq_hp"]]
    y = data.ix[:, 57]
    clf = svm.SVC()
    clf.fit(X, y)

Het model publiceren naar AzureML:

    # Publish the model.
    workspace_id = "<workspace-id>"
    workspace_token = "<workspace-token>"
    from azureml import services
    @services.publish(workspace_id, workspace_token)
    @services.types(char_freq_dollar = float, word_freq_remove = float, word_freq_hp = float)
    @services.returns(int) # 0 or 1
    def predictSpam(char_freq_dollar, word_freq_remove, word_freq_hp):
        inputArray = [char_freq_dollar, word_freq_remove, word_freq_hp]
        return clf.predict(inputArray)

    # Get some info about the resulting model.
    predictSpam.service.url
    predictSpam.service.api_key

    # Call the model
    predictSpam.service(1, 1, 1)

>[AZURE.NOTE] Dit is alleen beschikbaar voor python 2.7 en nog niet wordt ondersteund op 3.5. Uitvoeren met **/anaconda/bin/python2.7**.


## <a name="jupyterhub"></a>Jupyterhub

De verdeling Anaconda in de DSVM wordt geleverd voor een notitieblok Jupyter, een platforms-omgeving wilt delen Python, R of Julia code en analyse. Het notitieblok Jupyter wordt geopend met JupyterHub. U aanmelden met uw lokale Linux-gebruikersnaam en wachtwoord bij ***https://\<VM DNS naam of het IP-adres\>: 8000 /***. Alle configuratiebestanden voor JupyterHub vindt u in de directory **/etc/jupyterhub**.

Verschillende steekproeven notitieblokken zijn al geïnstalleerd op de VM:

- Zie de [IntroToJupyterPython.ipynb](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Data-Science-Virtual-Machine/Samples/Notebooks/IntroToJupyterPython.ipynb) voor een steekproef Python notitieblok.
- Zie [IntroTutorialinR](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Data-Science-Virtual-Machine/Samples/Notebooks/IntroTutorialinR.ipynb) voor een voorbeeld **R** -notitieblok.
- Zie de [IrisClassifierPyMLWebService](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Data-Science-Virtual-Machine/Samples/Notebooks/IrisClassifierPyMLWebService.ipynb) voor een ander voorbeeld **Python** notitieblok.

>[AZURE.NOTE] De taal Julia is ook beschikbaar vanaf de opdrachtregel op de Linux gegevens wetenschappelijke VM.


## <a name="rattle"></a>Rammelaar

[Rattle](https://cran.r-project.org/web/packages/rattle/index.html) (de R Analytical hulpmiddel naar informatie over eenvoudig) is een grafische R-hulpprogramma voor datamining. Er wordt een intuïtieve interface waarmee u gemakkelijk laden, verkennen, en transformeer gegevens en bouwen en modellen evalueren.  Het artikel [Rattle: A Data Mining grafische gebruikersinterface voor R](https://journal.r-project.org/archive/2009-2/RJournal_2009-2_Williams.pdf) biedt stapsgewijze instructies die de functies worden.

Installeer en start Rammelaar met de volgende opdrachten:

    if(!require("rattle")) install.packages("rattle")
    require(rattle)
    rattle()

>[AZURE.NOTE] Installatie is niet vereist in de DSVM. Maar Rammelaar mogelijk gevraagd u bij het installeren van extra pakketten wanneer deze wordt geladen.

Rammelaar gebruikt een interface op basis van een tabblad. De meeste van de tabbladen overeenkomen met de stappen in de [Gegevens wetenschap proces](https://azure.microsoft.com/documentation/learning-paths/data-science-process/), zoals gegevens laden of ermee. Het proces van de wetenschappelijke gegevens loopt van links naar rechts door de tabbladen. Maar het laatste tabblad in een logboek van de macro's starten met Rammelaar R-opdrachten bevat. 


Laden en configureren van de gegevensset:

- Als u wilt het bestand hebt geladen, selecteert u het tabblad **gegevens** en klik vervolgens 
- Kies de Gegevenskiezer naast de **bestandsnaam** en kies van **spambaseHeaders.data**. 
- Naar het bestand laden. Selecteer **uitvoeren** in de bovenste rij van de knoppen. Hier ziet u een overzicht van elke kolom, inclusief het geïdentificeerd gegevenstype, of u nu een invoer, een doeltoepassing of ander type variabele en het aantal unieke waarden.
- Rammelaar heeft de kolom **spam** correct geïdentificeerd als het doel. Selecteer de kolom spam en stel vervolgens het **Doel-gegevenstype** naar **Categoric**.

Aan de gegevens exporteren: 

- Selecteer het tabblad **verkennen** . 
- Klik op **Overzicht** **Execute**, om te zien vindt u informatie over de typen variabelen en sommige samenvatting statistieken worden aangepast. 
- Als u wilt bekijken van andere soorten statistieken over elke variabele, selecteer andere opties zoals **Beschrijf** of de **Basisbeginselen**.

Het tabblad **verkennen** kunt u veel begrijpelijke manier mee waarnemingspunten genereren. Een histogram van de gegevens uitzetten:


- Selecteer **onderzoeken**.
- Controleer **Histogram** voor **word_freq_remove** en **word_freq_you**.
- Selecteer **uitvoeren**. Hier ziet u beide waarnemingspunten dichtheid in een enkel graph-venster waar duidelijk is dat het woord 'u' wordt weergegeven vaker in e-mailberichten dan "verwijderen".

De correlatie-waarnemingspunten zijn ook interessante. U een account maakt:


- Kies de optie **correlatie** als het **Type**, klikt u vervolgens 
- Selecteer **uitvoeren**. 
- Rammelaar wordt u gewaarschuwd raadt maximaal 40 variabelen. Selecteer **Ja** om weer te geven van de tekening. 

Er zijn enkele interessante correlatie die zich voordoen: "technologieën" ten zeerste gerelateerd is aan "PK" en 'praktijkoefeningen', bijvoorbeeld. Deze is ook ten zeerste gerelateerd aan '650', omdat het netnummer van de gegevensset donors 650.

De numerieke waarden voor de correlatie tussen woorden zijn beschikbaar in het venster verkennen. Opmerking, bijvoorbeeld dat "technologie" negatief gerelateerd is aan "uw" en 'geld' interessant is.

Rammelaar kunt transformeren van de gegevensset om af te handelen enkele veelvoorkomende problemen. Bijvoorbeeld, kunt dit u functies oorspronkelijke, rekenen ontbrekende waarden verwerken uitschieters en variabelen of observaties met ontbrekende gegevens verwijderen. Regels van de koppeling tussen opmerkingen en/of variabelen kan ook door Rammelaar worden aangegeven. Deze tabbladen zijn buiten het bereik voor deze inleidende Stapsgewijze instructies.

Rammelaar kunt ook cluster analyse uitvoeren. Laten we uitsluiten sommige functies om de uitvoer gemakkelijker te lezen. Kies op het tabblad **gegevens** , **negeren** naast elk van de variabelen behalve deze tien hoogste items:

- word_freq_hp
- word_freq_technology
- word_freq_george
- word_freq_remove
- word_freq_your
- word_freq_dollar
- word_freq_money
- capital_run_length_longest
- word_freq_business
- spam

Ga terug naar het tabblad **Cluster** Kies **KMeans**en het *aantal* ingesteld op 4. Wordt **uitgevoerd**. De resultaten worden weergegeven in het uitvoervenster. Één cluster hoog frequentie van "george" en "PK" heeft en is waarschijnlijk een betrouwbare e-mailadres.

Een eenvoudige beslissing structuur machine learning-model maken: 

- Selecteer het tabblad **Model** 
- Kies **structuur** als het **Type**. 
- Selecteer **uitvoeren** om de structuur in tekstvorm in het uitvoervenster weer te geven. 
- Selecteer de knop **Tekenen** om weer te geven van een grafische versie. Dit lijkt lijkt veel op de structuur die we verkregen eerder met *rpart*.

Een van de nette functies van Rammelaar is de mogelijkheid voor het uitvoeren van verschillende machine learning methoden en ze snel te evalueren. Dit is de procedure:

- Kies **Alles** voor het **Type**. 
- Selecteer **uitvoeren**. 
- U kunt een één **Type**, zoals **SVM**, klikt u op en de resultaten bekijken nadat deze is voltooid. 
- U kunt ook de prestaties van de modellen op de validatie instellen met behulp van het tabblad **evalueren** vergelijken. Bijvoorbeeld leest de selectie **Fout Matrix** u de verwarring matrix, algemene fout en gemiddelde class fout voor elk model op de set gegevensvalidatie. 
- U kunt ook ROC bochten uitzetten, gevoeligheidsanalyse uitvoeren en andere soorten model evaluaties doen.

Zodra u klaar modellen bent, selecteert u de tab **logboek** om weer te geven van de R-code uitvoeren door Rammelaar tijdens de sessie. U kunt de knop **exporteren** op te slaan. 

>[AZURE.NOTE] Er is een fout in de huidige versie van Rammelaar. Als u wilt wijzigen van het script of deze later uw stappen herhalen gebruiken, moet u een teken # vóór het *exporteren van dit logboek...* invoegen in de tekst van het logboek. 


## <a name="postgresql--squirrel-sql"></a>PostgreSQL & eekhoorn SQL

De DSVM wordt geleverd met PostgreSQL is geïnstalleerd. PostgreSQL is een geavanceerde, open source relationele database. Hier vindt u het laden van onze gegevensset spam in PostgreSQL en deze vervolgens query.

Voordat u de gegevens laadt kunt, moet u toestaan van wachtwoordverificatie van het van de localhost. Bij een opdrachtprompt:

    sudo gedit /var/lib/pgsql/data/pg_hba.conf

Aan de onderkant van het configuratiebestand zijn meerdere lijnen die de toegestane verbindingen Detailstijlen:

    # "local" is for Unix domain socket connections only
    local   all             all                                     trust
    # IPv4 local connections:
    host    all             all             127.0.0.1/32            ident
    # IPv6 local connections:
    host    all             all             ::1/128                 ident

Wijzig de regel "IPv4-lokale verbindingen" md5 wordt gebruikt in plaats van ident, zodat we kunt Meld u aan met een gebruikersnaam en wachtwoord:

    # IPv4 local connections:
    host    all             all             127.0.0.1/32            md5

En de postgres-service opnieuw starten:

    sudo systemctl restart postgresql

Als u wilt starten psql, een interactief terminal voor PostgreSQL, als de gebruiker ingebouwde postgres, voer de volgende opdracht uit de opdrachtprompt:

    sudo -u postgres psql

Maak een nieuwe gebruikersaccount, de dezelfde gebruikersnaam gebruiken als de Linux account dat u momenteel bent aangemeld als en geef deze een wachtwoord:

    CREATE USER <username> WITH CREATEDB;
    CREATE DATABASE <username>;
    ALTER USER <username> password '<password>';
    \quit

Meld u vervolgens op psql als uw gebruikers:

    psql

En de gegevens in een nieuwe database te importeren:

    CREATE DATABASE spam;
    \c spam
    CREATE TABLE data (word_freq_make real, word_freq_address real, word_freq_all real, word_freq_3d real,word_freq_our real, word_freq_over real, word_freq_remove real, word_freq_internet real,word_freq_order real, word_freq_mail real, word_freq_receive real, word_freq_will real,word_freq_people real, word_freq_report real, word_freq_addresses real, word_freq_free real,word_freq_business real, word_freq_email real, word_freq_you real, word_freq_credit real,word_freq_your real, word_freq_font real, word_freq_000 real, word_freq_money real,word_freq_hp real, word_freq_hpl real, word_freq_george real, word_freq_650 real, word_freq_lab real,word_freq_labs real, word_freq_telnet real, word_freq_857 real, word_freq_data real,word_freq_415 real, word_freq_85 real, word_freq_technology real, word_freq_1999 real,word_freq_parts real, word_freq_pm real, word_freq_direct real, word_freq_cs real, word_freq_meeting real,word_freq_original real, word_freq_project real, word_freq_re real, word_freq_edu real,word_freq_table real, word_freq_conference real, char_freq_semicolon real, char_freq_leftParen real,char_freq_leftBracket real, char_freq_exclamation real, char_freq_dollar real, char_freq_pound real, capital_run_length_average real, capital_run_length_longest real, capital_run_length_total real, spam integer);
    \copy data FROM /home/<username>/spambase.data DELIMITER ',' CSV;
    \quit

Nu, laten we de gegevens en sommige query's met **Eekhoorn SQL**, een grafische hulpmiddel waarmee u kunt met databases via een JDBC-stuurprogramma werken verkennen.

Als u wilt beginnen, start u eekhoorn SQL in het menu toepassingen. Het stuurprogramma instellen:

- Selecteer **Windows**, klikt u vervolgens **weergave-stuurprogramma's**. 
- Met de rechtermuisknop op de **PostgreSQL** en selecteer **Stuurprogramma wijzigen**. 
- Selecteer **Extra Class pad**en vervolgens op **toevoegen**. 
- Voer ***/usr/share/java/jdbcdrivers/postgresql-9.4.1208.jre6.jar*** voor de **bestandsnaam** en 
- Selecteer **openen**.
- Kies lijst stuurprogramma's, selecteer **org.postgresql.Driver** in de **Klassenaam**, en selecteer **OK**.

De verbinding met de lokale server instellen:
 
- Selecteer **Windows**, klikt u vervolgens **weergeven aliassen.** 
- Kies de **+** knop om te maken van een nieuwe alias. 
- Noem deze *Spam database*, kies **PostgreSQL** in de vervolgkeuzelijst **stuurprogramma** .
- Stel de URL op *jdbc:postgresql://localhost/spam*. 
- Voer uw *gebruikersnaam* en *wachtwoord*. 
- Klik op **OK**. 
- U opent het venster **verbinding** , dubbelklikt u op de alias ***Spam-database*** . 
- Selecteer **verbinding maken**.

Sommige query's uitvoeren:

- Selecteer het tabblad **SQL** .
- Voer een eenvoudige query zoals `SELECT * from data;` in het tekstvak query boven aan het tabblad SQL. 
- Druk op **Ctrl + Enter** uit te voeren. Standaard retourneert eekhoorn SQL de eerste 100 rijen uit uw query. 

Zijn er veel meer query's die u kunt uitvoeren als u wilt deze gegevens verkennen. Bijvoorbeeld hoe is de frequentie van het word- *Controleer* het verschil tussen spam en ham?

    SELECT avg(word_freq_make), spam from data group by spam;

Of wat zijn de kenmerken van de e-mail die vaak *3d bevatten*?

    SELECT * from data order by word_freq_3d desc;

De meeste e-mailberichten met een hoge exemplaar van *3d* zijn gehouden spam, dus is het mogelijk een handige functie voor het samenstellen van een blog model om te classificeren van het e-mailberichten.

Als u uitvoeren machine learning met gegevens die zijn opgeslagen in een PostgreSQL-database wilt, kunt u overwegen [MADlib](http://madlib.incubator.apache.org/).

## <a name="sql-server-data-warehouse"></a>SQL Server datawarehouse

Azure SQL Data Warehouse is een cloudgebaseerde, schalen database kunnen grote hoeveelheden gegevens, relationele en niet-relationele verwerken. Zie voor meer informatie [Wat Azure SQL Data Warehouse is?](../sql-data-warehouse/sql-data-warehouse-overview-what-is.md)

Als u wilt verbinden met DataWarehouse en de tabel maakt, voer de volgende opdracht uit vanaf de opdrachtprompt:

    sqlcmd -S <server-name>.database.windows.net -d <database-name> -U <username> -P <password> -I

Klik bij de prompt sqlcmd:

    CREATE TABLE spam (word_freq_make real, word_freq_address real, word_freq_all real, word_freq_3d real,word_freq_our real, word_freq_over real, word_freq_remove real, word_freq_internet real,word_freq_order real, word_freq_mail real, word_freq_receive real, word_freq_will real,word_freq_people real, word_freq_report real, word_freq_addresses real, word_freq_free real,word_freq_business real, word_freq_email real, word_freq_you real, word_freq_credit real,word_freq_your real, word_freq_font real, word_freq_000 real, word_freq_money real,word_freq_hp real, word_freq_hpl real, word_freq_george real, word_freq_650 real, word_freq_lab real,word_freq_labs real, word_freq_telnet real, word_freq_857 real, word_freq_data real,word_freq_415 real, word_freq_85 real, word_freq_technology real, word_freq_1999 real,word_freq_parts real, word_freq_pm real, word_freq_direct real, word_freq_cs real, word_freq_meeting real,word_freq_original real, word_freq_project real, word_freq_re real, word_freq_edu real,word_freq_table real, word_freq_conference real, char_freq_semicolon real, char_freq_leftParen real,char_freq_leftBracket real, char_freq_exclamation real, char_freq_dollar real, char_freq_pound real, capital_run_length_average real, capital_run_length_longest real, capital_run_length_total real, spam integer) WITH (CLUSTERED COLUMNSTORE INDEX, DISTRIBUTION = ROUND_ROBIN);
    GO

Kopieer de gegevens met bcp:

    bcp spam in spambaseHeaders.data -q -c -t  ',' -S <server-name>.database.windows.net -d <database-name> -U <username> -P <password> -F 1 -r "\r\n"

>[AZURE.NOTE] De regeleinden in het gedownloade bestand zijn Windows-stijl, maar bcp verwacht UNIX-stijl, zodat we nodig om te laten weten bcp die met de vlag - r.

En query met sqlcmd:

    select top 10 spam, char_freq_dollar from spam;
    GO

U kunt ook zoeken met eekhoorn SQL. Volg dezelfde stappen voor de PostgreSQL, met het Microsoft MSSQL Server JDBC-stuurprogramma, die u in ***/usr/share/java/jdbcdrivers/sqljdbc42.jar vinden kunt***.

## <a name="next-steps"></a>Volgende stappen

Zie voor een overzicht van onderwerpen waarmee u de taken waaruit het proces gegevens wetenschappelijke in Azure wordt aangegeven, [Team gegevens wetenschap proces](http://aka.ms/datascienceprocess).

Zie voor een beschrijving van andere end-to-end-scenario's die de stappen in het proces van het Team gegevens wetenschappelijke voor specifieke scenario's demonstreren [Team gegevens wetenschap proces walkthroughs](data-science-process-walkthroughs.md). De walkthroughs illustreren ook hoe u het combineren van cloud en on-premises hulpprogramma's en services in een werkstroom of verkooppijplijn een intelligente toepassing maken.

