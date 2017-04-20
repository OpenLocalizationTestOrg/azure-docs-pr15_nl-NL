<properties
    pageTitle="Tien wat u in de wetenschappelijke gegevens VM doen kunt | Microsoft Azure"
    description="Verschillende gegevens verkennen en modellering taak op de wetenschappelijke gegevens VM uitvoeren."
    services="machine-learning"
    documentationCenter=""
    authors="bradsev"
    manager="jhubbard"
    editor="cgronlun"  />

<tags
    ms.service="machine-learning"
    ms.workload="data-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/29/2016"
    ms.author="gokuma;weig;bradsev" />

# <a name="ten-things-you-can-do-on-the-data-science-virtual-machine"></a>Tien wat die u in de wetenschappelijke gegevens VM doen kunt

De Microsoft gegevens wetenschappelijke VM (DSVM) is een krachtige wetenschappelijke ontwikkelomgeving waarmee u verschillende gegevens verkennen en modellering taken uitvoeren. De omgeving al wordt geleverd en-klare en geleverd met diverse populaire analytics hulpmiddelen voor gegevens die u eenvoudig kunt u snel aan de slag met uw analyse voor On-premises Cloud of hybride implementaties. De DSVM nauw met veel Azure services en is kunnen lezen en gegevens die al is opgeslagen op Azure in Azure SQL Data Warehouse, Azure gegevens Lake, Azure opslag of in DocumentDB verwerken. Dit kan ook gebruikmaken van andere analytics-hulpprogramma's zoals Azure Machine Learning en Azure gegevens Factory.


In dit artikel begeleiden we u bij het gebruik van uw DSVM om verschillende gegevens wetenschappelijke taken uitvoeren van en werken met andere Azure-services. Hier volgen enkele dingen die u op de DSVM kunt doen:

1. Gegevens verkennen en modellen lokaal op de Server in de Microsoft-R, Python gebruiken DSVM ontwikkelen
2. Een notitieblok Jupyter gebruiken om te experimenteren met uw gegevens in een browser Python 2, 3, Python, Microsoft R met een klaar enterprise-versie van R ontworpen voor schaalbaarheid en prestaties
3. Modellen die zijn gemaakt met behulp van R en Python op Azure Machine Learning zodat clienttoepassingen toegang hebt tot uw modellen die met een eenvoudige web services-interface mogelijk te maken
4. Uw Azure resources via Azure portal of Powershell beheren
5. Opslagruimte in beslag uitbreiden en grote gegevenssets delen / code in uw hele team door te maken van een bestandsopslag Azure als een mogelijk station op uw DSVM
6. Code delen met uw team met Github en toegang tot uw bibliotheek met de vooraf geïnstalleerde cijfer clients - cijfer we vaker doen, cijfer grafische gebruikersinterface.
7. Toegang tot verschillende Azure gegevens en analyses services zoals Azure-blobopslag, Azure gegevens Lake, Azure HDInsight (Hadoop), Azure DocumentDB, Azure SQL Data Warehouse & databases
8. Rapporten en dashboard met de Power BI Desktop vooraf geïnstalleerd op de DSVM bouwen en implementeren op de cloud
9. Dynamisch schalen dat uw DSVM de behoeften van uw project
10. Aanvullende hulpmiddelen voor installeren op uw VM   


>[AZURE.NOTE] Aanvullende gebruikskosten geldt voor veel van de extra opslagruimte en analyses gegevensservices die in dit artikel. Raadpleeg de pagina [Azure prijzen](https://azure.microsoft.com/pricing/) voor meer informatie.


**Vereisten voor**

- Moet u een Azure-abonnement. U kunt zich registreren voor een gratis proefabonnement [hier](https://azure.microsoft.com/free/).

- Instructies voor het inrichten van een gegevens wetenschappelijke virtuele Machine op de Azure-portal zijn beschikbaar bij het [maken van een virtuele machine](https://portal.azure.com/#create/microsoft-ads.standard-data-science-vmstandard-data-science-vm).

## <a name="1-explore-data-and-develop-models-using-microsoft-r-server-or-python"></a>1. gegevens verkennen en ontwikkelen van Microsoft R Server of Python modellen

U kunt de talen zoals R en Python moet uw gegevens analytics direct op de DSVM.

Voor R, kunt u een IDE "Revolutie R Enterprise 8.0" die kan worden gevonden op het startmenu of het bureaublad. Microsoft biedt aanvullende bibliotheken boven aan het geopende bron/CRAN-R om in te schakelen scalable analyses en de mogelijkheid om groter is dan de grootte van het geheugen toegestaan op parallelle gedeelde analyse wijze gegevens te analyseren. U kunt ook een IDE R van uw keuze zoals [RStudio](https://www.rstudio.com/products/rstudio-desktop/)installeren.

Voor Python, kunt u een IDE zoals Visual Studio Community Edition waarvoor de Python Tools voor Visual Studio (PTVS) extensie vooraf geïnstalleerd. Alleen een eenvoudige Python 2.7 is standaard ingesteld op PTVS (zonder een analytics-bibliotheek zoals SciKit, Pandas). Om te kunnen inschakelen Anaconda Python 2.7 en 3.5, moet u als volgt te werk:

* Aangepaste omgevingen voor elke versie maken door te schuiven naar **Extra** -> **Python hulpmiddelen voor** -> **Python omgevingen** en vervolgens te klikken op "**+ aangepaste**' in de Visual Studio 2015 Community Edition
* Geef een beschrijving en de omgeving voorvoegsel paden instellen als *c:\anaconda* voor Anaconda Python 2.7 of *c:\anaconda\envs\py35* voor Anaconda Python 3.5
* Klik op **Automatisch detecteren** in en klik vervolgens **toepassen** om op te slaan van de omgeving.

Hier ziet u hoe de instelling van de aangepaste omgeving eruitziet in Visual Studio.

![PTVS-instelling](./media/machine-learning-data-science-vm-do-ten-things/PTVSSetup.png)

Zie de [documentatie van PTVS](https://github.com/Microsoft/PTVS/wiki/Selecting-and-Installing-Python-Interpreters#hey-i-already-have-an-interpreter-on-my-machine-but-ptvs-doesnt-seem-to-know-about-it) voor meer informatie over het maken van Python omgevingen.

Nu u zijn ingesteld om een nieuwe Python project te maken. Ga naar **bestand** -> **Nieuw** -> **Project** -> **Python** en selecteer het type Python-toepassing die u maakt. U kunt de Python-omgeving voor het huidige project instellen op de gewenste versie (Anaconda 2.7 of 3.5): met de rechtermuisknop op de **Python-omgeving**, selecteert u **Toevoegen/verwijderen Python omgevingen**en selecteer vervolgens de gewenste-omgeving wilt koppelen aan het project. U vindt meer informatie over het werken met PTVS op de pagina van de [documentatie](https://github.com/Microsoft/PTVS/wiki) product.

## <a name="2-using-a-jupyter-notebook-to-explore-and-model-your-data-with-python-or-r"></a>2. gebruik van een notitieblok Jupyter te verkennen en modelleren van uw gegevens met Python of R

Het notitieblok Jupyter is een krachtige omgeving met een browser gebaseerde "IDE" voor verkennen en modellering. U kunt Python 2, 3 Python of R (bron openen en de Microsoft-R-Server) in een notitieblok Jupyter.

Aan de Jupyter notitieblok Klik op het pictogram van het menu start starten / bureaubladpictogram getiteld **Jupyter notitieblok**. Klik op de DSVM u kunt ook zoeken naar ' https://localhost:9999 / ' voor toegang tot het notitieblok Jupiter. Als wordt u gevraagd een wachtwoord, gebruikt u de instructies in de sectie ***een sterk wachtwoord op de server Jupyter-notitieblok maken*** van de [voorziening Microsoft gegevens wetenschappelijke VM](machine-learning-data-science-provision-vm.md) onderwerp een sterk wachtwoord voor toegang tot het Jupyter-notitieblok maken. 

Nadat u het notitieblok hebt geopend, ziet u een map met een paar voorbeeld-notitieblokken die vooraf worden verpakt in de DSVM zijn. U kunt nu het volgende doen:

- Klik op het notitieblok om te zien van de code.
- elke cel uitvoeren door op **SHIFT + ENTER**te drukken.
- het hele notitieblok uitvoeren door te klikken op de **cel** -> **uitvoeren**
- een nieuw notitieblok maken door te klikken op het pictogram Jupyter (links bovenhoek) en vervolgens op de knop **Nieuw** aan de rechterkant te klikken en vervolgens kiezen de taal van het notitieblok (ook wel bekend als kernels).   


>[AZURE.NOTE] Momenteel wordt ondersteund Python 2.7, Python 3.5 en R. De kernel R ondersteunt hoeft te programmeren in zowel bron openen R, evenals de onderneming scalable Microsoft R-Server.   


Als u in het notitieblok vindt u op uw gegevens, het model maken en kies het model uw keuze van bibliotheken met test.


## <a name="3-build-models-using-r-or-python-and-operationalize-them-using-azure-machine-learning"></a>3. modellen met R of Python bouwen en ze met Azure Machine Learning mogelijk maken

Zodra u hebt gemaakt en uw model gevalideerd worden de volgende stap meestal te implementeren in bedrijf is. Dit geeft de klant de modelvoorspellingen op een realtime of op basis van batch modus roepen-toepassingen. Azure Machine Learning biedt een om als u wilt mogelijk een model dat is ingebouwd in R of Python maken.

Wanneer u uw model in Azure Machine Learning mogelijk maken, wordt een webservice waarmee clients REST-gesprekken die in invoerparameters doorgeven en ontvangen van voorspellingen uit het model als uitvoer weergegeven.   


>[AZURE.NOTE] Als u nog geen geregistreerd voor AzureML, u een gratis werkruimte of een standaard-werkruimte verkrijgen kunt door vanaf de startpagina [AzureML Studio](https://studio.azureml.net/) en te klikken op 'Aan de slag'.   


### <a name="build-and-operationalize-python-models"></a>Modellen opbouwen en Python mogelijk maken

Hier ziet u een codefragment ontwikkeld in een notitieblok van Python Jupyter die een eenvoudige model met de bibliotheek SciKit meer maakt.

    #IRIS classification
    from sklearn import datasets
    from sklearn import svm
    clf = svm.SVC()
    iris = datasets.load_iris()
    X, y = iris.data, iris.target
    clf.fit(X, y)

De methode voor het implementeren van uw modellen python naar Azure Machine Learning terugloopt van de voorspelling van het model in een functie en wordt deze verfraaid met verstrekt door de vooraf geïnstalleerde Azure Machine Learning python bibliotheek kenmerken die geven uw Azure Machine Learning-werkruimte-ID, API-sleutel en de invoer en parameters terug te keren.  

    from azureml import services
    @services.publish(workspaceid, auth_token)
    @services.types(sep_l = float, sep_w = float, pet_l=float, pet_w=float)
    @services.returns(int) #0, or 1, or 2
    def predictIris(sep_l, sep_w, pet_l, pet_w):
    inputArray = [sep_l, sep_w, pet_l, pet_w]
    return clf.predict(inputArray)

Een client kan nu bellen naar de webservice. Zijn er gemak aanvullende inhoud die de REST API aanvragen maken. Hier volgt een voorbeeldcode voor het gebruik van de webservice.

    # Consume through web service URL and keys
    from azureml import services
    @services.service(url, api_key)
    @services.types(sep_l = float, sep_w = float, pet_l=float, pet_w=float)
    @services.returns(float)
    def IrisPredictor(sep_l, sep_w, pet_l, pet_w):
    pass

    IrisPredictor(3,2,3,4)


>[AZURE.NOTE] De bibliotheek Azure Machine Learning wordt alleen ondersteund op Python 2.7 momenteel.   


### <a name="build-and-operationalize-r-models"></a>Modellen opbouwen en R mogelijk maken

R-modellen die zijn gebaseerd op de gegevens wetenschappelijke virtuele Machine of elders op Azure Machine Learning op een wijze die is vergelijkbaar met hoe deze klaar is voor Python, kunt u implementeren. Haar zijn de stappen:

- Maak een bestand settings.json zoals hieronder aan uw werkruimte-ID en auth token.
- Schrijf een Wikkel voor het model van de functie voorspellen.
- bellen ```publishWebService``` in de bibliotheek Azure Machine Learning om door te geven in de Wikkel van de functie.  

Hier ziet u de procedure en code fragmenten die kunnen worden gebruikt om instellen, maken, publiceren en gebruiken van een model als een webservice in Azure Machine Learning.

#### <a name="setup"></a>Setup

1.  Het pakket AzureML R installeren door te typen ```install.packages("AzureML")``` in revolutie R Enterprise 8.0 IDE of uw IDE R.
2.  Download RTools vanuit [hier](https://cran.r-project.org/bin/windows/Rtools/). Moet u eerst het zip-programma in het pad (en benoemde zip.exe) naar het mogelijk maken van uw R-pakket in AzureML.
3.  Maak een settings.json bestand onder een map met de naam ```.azureml``` onder de basismap en voer de parameters uit uw werkruimte Azure ML:

Settings.JSON bestandsstructuur:

    {"workspace":{
    "id"                  : "ENTER YOUR AZUREML WORKSPACE ID",
    "authorization_token" : "ENTER YOUR AZUREML AUTH TOKEN"
    }}


#### <a name="build-a-model-in-r-and-publish-it-in-azure-ml"></a>Een model in R maken en te publiceren in Azure ML

    library(AzureML)
    ws <- workspace(config="~/.azureml/settings.json")

    if(!require("lme4")) install.packages("lme4")
    library(lme4)
    set.seed(1)
    train <- sleepstudy[sample(nrow(sleepstudy), 120),]
    m <- lm(Reaction ~ Days + Subject, data = train)

    # Define a prediction function to publish based on the model:
    sleepyPredict <- function(newdata){
        predict(m, newdata=newdata)
    }

    ep <- publishWebService(ws, fun = sleepyPredict, name="sleepy lm", inputSchema = sleepstudy, data.frame=TRUE)

#### <a name="consume-the-model-deployed-in-azure-ml"></a>Het model dat is geïmplementeerd in Azure ML gebruiken

Het model van een clienttoepassing in beslag neemt, gebruiken we de Azure Machine Learning-bibliotheek opzoeken de gepubliceerde webservice met behulp van de naam van de `services` API-oproep om te bepalen van het eindpunt. Vervolgens u alleen roept de `consume` , functie en geven in het gegevensframe om te worden voorspeld.
De volgende code wordt gebruikt voor het model als een Azure Machine Learning-webservice die zijn gepubliceerd in beslag nemen.


    library(AzureML)
    library(lme4)
    ws <- workspace(config="~/.azureml/settings.json")

    s <-  services(ws, name = "sleepy lm")
    s <- tail(s, 1) # use the last published function, in case of duplicate function names

    ep <- endpoints(ws, s)

    # OK, try this out, and compare with raw data
    ans = consume(ep, sleepstudy)$ans

Meer informatie over de bibliotheek Azure Machine Learning R vindt u [hier](https://cran.r-project.org/web/packages/AzureML/AzureML.pdf).


## <a name="4-administer-your-azure-resources-using-azure-portal-or-powershell"></a>4. uw Azure resources via Azure portal of Powershell beheren

De DSVM wordt niet alleen kunt u uw analytics-oplossing lokaal maken op de virtuele machine, maar er wordt een ook hebt u toegang tot services op van Microsoft Azure cloud. Azure biedt diverse berekeningscluster, opslag, data analytics-services en andere services die u kunt beheren en openen vanuit uw DSVM.

Als u wilt uw Azure-abonnement en cloud-resources beheren kunt u uw browser gebruiken en wijs de [Azure-portal](https://portal.azure.com). U kunt ook Azure Powershell gebruiken voor het beheer van de resources via een script en uw Azure-abonnement.
U kunt Azure Powershell uitvoeren via een snelkoppeling op het bureaublad of vanuit het startmenu met de naam "Microsoft Azure Powershell". Zie [Microsoft Azure Powershell-documentatie](../powershell-azure-resource-manager.md) voor meer informatie over hoe u de resources met behulp van Windows Powershell-scripts en uw Azure-abonnement kunt beheren.


## <a name="5-extend-your-storage-space-with-a-shared-file-system"></a>5. de opslagruimte met een gedeelde bestandssysteem uitbreiden

Gegevens wetenschappers kunnen delen grote gegevenssets, code of andere bronnen in het team. De DSVM zelf is bijna 70GB beschikbare ruimte. Als u wilt uitbreiden uw opslag, kunt u het bestand Azure-Service en dit hetzij koppelen op de DSVM of toegang toe via een REST API.   


>[AZURE.NOTE] De maximale ruimte van het delen van Azure bestand-Service is 5TB en maximale bestandsgrootte die afzonderlijke is 1TB.   


U kunt Azure Powershell gebruiken om een share Azure bestand-Service te maken. Hier ziet u het script uitvoeren onder Azure PowerShell voor het maken van een service van Azure bestandsshare.

    # Authenticate to Azure.
    Login-AzureRmAccount
    # Select your subscription
    Get-AzureRmSubscription –SubscriptionName "<your subscription name>" | Select-AzureRmSubscription
    # Create a new resource group.
    New-AzureRmResourceGroup -Name <dsvmdatarg>
    # Create a new storage account. You can reuse existing storage account if you wish.
    New-AzureRmStorageAccount -Name <mydatadisk> -ResourceGroupName <dsvmdatarg> -Location "<Azure Data Center Name For eg. South Central US>" -Type "Standard_LRS"
    # Set your current working storage account
    Set-AzureRmCurrentStorageAccount –ResourceGroupName "<dsvmdatarg>" –StorageAccountName <mydatadisk>

    # Create a Azure File Service Share
    $s = New-AzureStorageShare <<teamsharename>>
    # Create a directory under the FIle share. You can give it any name
    New-AzureStorageDirectory -Share $s -Path <directory name>
    # List the share to confirm that everything worked
    Get-AzureStorageFile -Share $s


Nu dat u een Azure bestandsshare hebt gemaakt, kunt u deze kunt koppelen in een virtuele machine in Azure wordt aangegeven. Het wordt ten zeerste aanbevolen dat de VM zich in dezelfde Azure Datacenter als de opslagruimte-account om te voorkomen latentie en gegevens doorverbinden kosten. Hier ziet u de opdrachten voor het station koppelen op de DSVM die u op Azure Powershell uitvoeren kunt.


    # Get storage key of the storage account that has the Azure file share from Azure portal. Store it securely on the VM to avoid prompted in next command.
    cmdkey /add:<<mydatadisk>>.file.core.windows.net /user:<<mydatadisk>> /pass:<storage key>

    # Mount the Azure file share as Z: drive on the VM. You can chose another drive letter if you wish
    net use z:  \\<mydatadisk>.file.core.windows.net\<<teamsharename>>


Nu kunt u dit station openen, zoals u zou doen met een normale station op de VM.

## <a name="6-share-code-with-your-team-using-github"></a>6. code delen met uw team met Github

Github is een code-opslagplaats waar u een groot aantal voorbeelden van code en bronnen voor verschillende hulpmiddelen voor verschillende technologieën gedeeld door de ontwikkelaar community gebruiken kunt vinden. Het cijfer gebruikt als de technologie voor het bijhouden en opslaan van versies van de codebestanden. Github is ook een platform waarop u uw eigen bibliotheek opslaan van de gedeelde code en de documentatie van uw team, implementeren versiebeheer en ook bepalen wie toegang hebt tot weergeven en code bijdragen kunt maken. Ga naar de [Github help-pagina's](https://help.github.com/) voor meer informatie over het gebruik van cijfer. U kunt Github gebruiken als een van de manieren om samen te werken met uw team, gebruikt u code die zijn ontwikkeld door de community en code bijdragen terug naar de community.

De DSVM al wordt geleverd met clienthulpprogramma's geladen op beide opdrachtregel als CRL interface voor toegang tot Github opslagplaats. Het hulpmiddel lijn van de opdracht om te werken met cijfer en Github heet cijfer we vaker doen. Visual Studio is geïnstalleerd op de DSVM heeft het cijfer selectievakje extensies. U vindt opstart pictogrammen voor deze programma's op het startmenu en op het bureaublad.

Code downloaden van een Github opslagplaats u gebruikt de ```git clone``` opdracht. Bijvoorbeeld om te downloaden gegevens wetenschappelijke opslagplaats door Microsoft is gepubliceerd naar de huidige map u de volgende opdracht kunt uitvoeren als u in ```git-bash```.

    git clone https://github.com/Azure/Azure-MachineLearning-DataScience.git

Visual Studio, kunt u dezelfde klonen bewerking uitvoeren. De-schermafbeelding hieronder ziet u hoe u toegang tot cijfer en Github hulpmiddelen in Visual Studio.

![Cijfer in Visual Studio](./media/machine-learning-data-science-vm-do-ten-things/VSGit.PNG)


U vindt meer informatie over het gebruik van cijfer voor gebruik met uw bibliotheek Github uit verschillende bronnen die beschikbaar zijn op github.com. Het [blad cheats](https://training.github.com/kit/downloads/github-git-cheat-sheet.pdf) is een handige verwijzing.


## <a name="7-access-various-azure-data-and-analytics-services"></a>7. toegang tot verschillende Azure-gegevens en analyses services

### <a name="azure-blob"></a>Azure Blob

Azure blob is een betrouwbare, min kosten cloudopslag voor gegevens groot en kleine. Laat ons bekijken hoe u gegevens naar Azure Blob en access-gegevens die zijn opgeslagen in een Azure Blob verplaatsen kunt.

**Vereiste**

- **Uw Azure Blob storage-account maken van [Azure-portal](https://portal.azure.com).**

![Create_Azure_Blob](./media/machine-learning-data-science-vm-do-ten-things/Create_Azure_Blob.PNG)


- Bevestig dat het vooraf geïnstalleerde opdrachtregel AzCopy hulpmiddel wordt aangetroffen op ```C:\Program Files (x86)\Microsoft SDKs\Azure\AzCopy\azcopy.exe```. U kunt de map met de azcopy.exe aan uw omgevingsvariabele PATH om te voorkomen dat het volledige pad typen wanneer u zich met dit hulpmiddel kunt toevoegen. Raadpleeg voor meer informatie over de functie AzCopy [AzCopy documentatie](../storage/storage-use-azcopy.md)

- Start het hulpprogramma Azure opslag Explorer. Dit kan worden gedownload van [Microsoft Azure opslag Explorer](http://storageexplorer.com/). 

![AzureStorageExplorer_v4](./media/machine-learning-data-science-vm-do-ten-things/AzureStorageExplorer_v4.png)


**Verplaats gegevens van VM naar Azure Blob: AzCopy**

Als gegevens wilt verplaatsen tussen uw lokale bestanden en -blobopslag, kunt u AzCopy in opdrachtregel of PowerShell:

    AzCopy /Source:C:\myfolder /Dest:https://<mystorageaccount>.blob.core.windows.net/<mycontainer> /DestKey:<storage account key> /Pattern:abc.txt

Vervang **C:\myfolder** naar het pad waar het bestand is opgeslagen, **mystorageaccount** naar uw blob opslagaccountnaam, **mycontainer** naar de containernaam van de, **opslag accountsleutel** om uw blob storage-toegangstoets. U vindt de referenties van uw opslag-account in [Azure-portal](https://portal.azure.com).

![StorageAccountCredential_v2](./media/machine-learning-data-science-vm-do-ten-things/StorageAccountCredential_v2.png)

AzCopy opdracht uitvoeren in PowerShell of vanaf een opdrachtprompt. Hier ziet u enkele voorbeeld van syntaxis van de opdracht AzCopy:


    # Copy *.sql from local machine to a Azure Blob
    "C:\Program Files (x86)\Microsoft SDKs\Azure\AzCopy\azcopy" /Source:"c:\Aaqs\Data Science Scripts" /Dest:https://[ENTER STORAGE ACCOUNT].blob.core.windows.net/[ENTER CONTAINER] /DestKey:[ENTER STORAGE KEY] /S /Pattern:*.sql

    # Copy back all files from Azure Blob container to Local machine

    "C:\Program Files (x86)\Microsoft SDKs\Azure\AzCopy\azcopy" /Dest:"c:\Aaqs\Data Science Scripts\temp" /Source:https://[ENTER STORAGE ACCOUNT].blob.core.windows.net/[ENTER CONTAINER] /SourceKey:[ENTER STORAGE KEY] /S



Nadat u uw AzCopy-opdracht om te kopiëren naar een Azure blob uitvoert ziet u uw bestand ziet u omhoog in Azure opslag Explorer kort.

![AzCopy_run_finshed_Storage_Explorer_v3](./media/machine-learning-data-science-vm-do-ten-things/AzCopy_run_finshed_Storage_Explorer_v3.png)


**Verplaats gegevens van VM naar Azure Blob: Azure opslag Explorer**

U kunt ook gegevens uit het lokale bestand uploaden in uw VM met Azure Opslagverkenner:

- Als u wilt uploaden gegevens aan een container, selecteert u de doelcontainer en klik op de knop **uploaden** .![](./media/machine-learning-data-science-vm-do-ten-things/storage-accounts.png)
- Klik op de **…** aan de rechterkant van het selectievakje **bestanden** , selecteer een of meerdere bestanden uploaden vanaf het bestandssysteem en klik op **uploaden** om te beginnen met het uploaden van bestanden.![](./media/machine-learning-data-science-vm-do-ten-things/upload-files-to-blob.png)


**Gegevens uit Azure Blob lezen: AML reader module**

In Azure Machine Learning Studio kunt u een **module gegevens importeren** naar gegevens uit uw blob lezen.


![AML_ReaderBlob_Module_v3](./media/machine-learning-data-science-vm-do-ten-things/AML_ReaderBlob_Module_v3.png)


**Gegevens uit Azure Blob lezen: Python ODBC**

U kunt **BlobService** bibliotheek gegevens rechtstreeks uit blob in een programma Jupyter notitieblok of nieuwe Python lezen.

Importeer eerst vereiste pakketten:

    import pandas as pd
    from pandas import Series, DataFrame
    import numpy as np
    import matplotlib.pyplot as plt
    from time import time
    import pyodbc
    import os
    from azure.storage.blob import BlobService
    import tables
    import time
    import zipfile
    import random

Vervolgens uw accountreferenties Azure Blob invoegtoepassing en gegevens uit Blob lezen:

    CONTAINERNAME = 'xxx'
    STORAGEACCOUNTNAME = 'xxxx'
    STORAGEACCOUNTKEY = 'xxxxxxxxxxxxxxxx'
    BLOBNAME = 'nyctaxidataset/nyctaxitrip/trip_data_1.csv'
    localfilename = 'trip_data_1.csv'
    LOCALDIRECTORY = os.getcwd()
    LOCALFILE =  os.path.join(LOCALDIRECTORY, localfilename)

    #download from blob
    t1 = time.time()
    blob_service = BlobService(account_name=STORAGEACCOUNTNAME,account_key=STORAGEACCOUNTKEY)
    blob_service.get_blob_to_path(CONTAINERNAME,BLOBNAME,LOCALFILE)
    t2 = time.time()
    print(("It takes %s seconds to download "+BLOBNAME) % (t2 - t1))

    #unzipping downloaded files if needed
    #with zipfile.ZipFile(ZIPPEDLOCALFILE, "r") as z:
    #    z.extractall(LOCALDIRECTORY)

    df1 = pd.read_csv(LOCALFILE, header=0)
    df1.columns = ['medallion','hack_license','vendor_id','rate_code','store_and_fwd_flag','pickup_datetime','dropoff_datetime','passenger_count','trip_time_in_secs','trip_distance','pickup_longitude','pickup_latitude','dropoff_longitude','dropoff_latitude']
    print 'the size of the data is: %d rows and  %d columns' % df1.shape

De gegevens worden gelezen als een kader van gegevens:

![IPNB_data_readin](./media/machine-learning-data-science-vm-do-ten-things/IPNB_data_readin.PNG)


### <a name="azure-data-lake"></a>Azure gegevens Lake

Azure Lake gegevensopslag is een opslagplaats hyper-schaal voor groot gegevens analytics werkbelasting en compatibel zijn met Hadoop Distributed bestand System (HDFS). Dit werkt met zowel het Hadoop-systeem en de Analytics Lake van Azure-gegevens. U leert hoe u kunt gegevens verplaatsen naar de Azure Lake gegevensopslag en analyses door middel van Azure gegevens Lake analyses uitvoeren.

**Vereiste**

- Maak uw Azure gegevens Lake Analytics in [Azure-portal](https://portal.azure.com).

![Azure_Data_Lake_Create_v2](./media/machine-learning-data-science-vm-do-ten-things/Azure_Data_Lake_Create_v2.png)


- De **Hulpmiddelen voor gegevens Lake Azure** in **Visual Studio** gevonden op deze [koppeling](https://www.microsoft.com/download/details.aspx?id=49504) is al geïnstalleerd op de Visual Studio-Community-Edition, dat wil op de virtuele machine zeggen. Na de eerste Visual Studio en logboekregistratie in uw Azure-abonnement, ziet u uw Azure gegevens Analytics-account en opslag in het linkerdeelvenster van Visual Studio.

![Azure_Data_Lake_PlugIn_v2](./media/machine-learning-data-science-vm-do-ten-things/Azure_Data_Lake_PlugIn_v2.PNG)


**Verplaats gegevens van VM naar gegevens Lake: Azure Data Lake Explorer**

U kunt **Azure Data Lake Explorer** gebruiken voor het uploaden van gegevens uit de lokale bestanden in uw VM met gegevens Lake storage.

![Azure_Data_Lake_UploadData](./media/machine-learning-data-science-vm-do-ten-things/Azure_Data_Lake_UploadData.PNG)

U kunt ook een pijplijn gegevens de verplaatsing van uw gegevens of naar Azure gegevens Lake productionize maken met de [Factory(ADF) van Azure-gegevens](https://azure.microsoft.com/services/data-factory/). We raadpleegt u in dit [artikel](https://azure.microsoft.com/blog/creating-big-data-pipelines-using-azure-data-lake-and-azure-data-factory/) als u de stappen voor het maken van de gegevens pijpleidingen.

**Gegevens van Azure Blob naar gegevens Lake gelezen: I-SQL**

Als uw gegevens zich in Azure-blobopslag bevinden, kunt u de gegevens rechtstreeks lezen van opslag in Azure blob in U SQL-query. Controleer voordat u uw I-SQL-query opstelt, of dat uw blob storage-account is gekoppeld aan uw Lake Azure-gegevens. Ga naar **Azure-portal**, uw dashboard Azure gegevens Lake analyses, klik op **Gegevensbron toevoegen**, selecteer opslagtype met **Azure Storage** en sluit uw Azure-Opslagaccountnaam en van toetsen. Vervolgens is mogelijk om te verwijzen naar de gegevens die zijn opgeslagen in de opslagruimte-account.

![](./media/machine-learning-data-science-vm-do-ten-things/Link_Blob_to_ADLA_v2.PNG)


Visual Studio, kunt u gegevens uit blobopslag lezen, gegevens manipuleren doen, aanbevelen technische functie en de resulterende gegevens Azure gegevens Lake of Azure-blobopslag. Wanneer u verwijst naar de gegevens in blobopslag, gebruikt u **wasb: / /**; Wanneer u verwijst naar de gegevens in Azure gegevens Lake, gebruikt u **swbhdfs: / /**

![](./media/machine-learning-data-science-vm-do-ten-things/USQL_Read_Blob_v2.PNG)

U kunt de volgende I-SQL-query's in Visual Studio:

    @a =
        EXTRACT medallion string,
                hack_license string,
                vendor_id string,
                rate_code string,
                store_and_fwd_flag string,
                pickup_datetime string,
                dropoff_datetime string,
                passenger_count int,
                trip_time_in_secs double,
                trip_distance double,
                pickup_longitude string,
                pickup_latitude string,
                dropoff_longitude string,
                dropoff_latitude string

        FROM "wasb://<Container name>@<Azure Blob Storage Account Name>.blob.core.windows.net/<Input Data File Name>"
        USING Extractors.Csv();

    @b =
        SELECT vendor_id,
        COUNT(medallion) AS cnt_medallion,
        SUM(passenger_count) AS cnt_passenger,
        AVG(trip_distance) AS avg_trip_dist,
        MIN(trip_distance) AS min_trip_dist,
        MAX(trip_distance) AS max_trip_dist,
        AVG(trip_time_in_secs) AS avg_trip_time
        FROM @a
        GROUP BY vendor_id;

    OUTPUT @b   
    TO "swebhdfs://<Azure Data Lake Storage Account Name>.azuredatalakestore.net/<Folder Name>/<Output Data File Name>"
    USING Outputters.Csv();

    OUTPUT @b   
    TO "wasb://<Container name>@<Azure Blob Storage Account Name>.blob.core.windows.net/<Output Data File Name>"
    USING Outputters.Csv();



Nadat u uw query is verzonden naar de server, kunt u een diagram met de status van uw taak wordt weergegeven.

![](./media/machine-learning-data-science-vm-do-ten-things/USQL_Job_Status.PNG)


**Gegevens in de gegevens Lake query: I-SQL**

Nadat de gegevensset is ingenomen in Azure gegevens Lake, kunt u [I-SQL-taal](../data-lake-analytics/data-lake-analytics-u-sql-get-started.md) query en de gegevens verkennen. I-SQL-taal is vergelijkbaar met de T-SQL, maar sommige functies uit de C# worden gecombineerd, zodat gebruikers aangepaste modules, door de gebruiker gedefinieerde functies en enzovoort schrijven kunnen. U kunt de scripts gebruiken in de vorige stap.

Na de query wordt verzonden naar de server, tripdata_summary. CSV vindt u binnenkort in **Azure Data Lake Explorer**, u mogelijk voorbeeld van de gegevens door met de rechtermuisknop op het bestand.

![](./media/machine-learning-data-science-vm-do-ten-things/USQL_create_summary.png)

De bestandsinformatie wilt bekijken:

![](./media/machine-learning-data-science-vm-do-ten-things/USQL_tripdata_summary.png)


### <a name="hdinsight-hadoop-clusters"></a>HDInsight Hadoop Clusters

Azure HDInsight is een beheerde Apache Hadoop, een HBase en Storm-service in de cloud. U kunt eenvoudig werken met Azure HDInsight clusters van de gegevens wetenschappelijke virtuele machine.

**Vereiste**

- Uw Azure Blob storage-account maken van [Azure-portal](https://portal.azure.com). Dit account opslag wordt gebruikt voor het opslaan van gegevens voor HDInsight clusters.

![](./media/machine-learning-data-science-vm-do-ten-things/Create_Azure_Blob.PNG)

- Azure HDInsight Hadoop Clusters via [Azure-portal](machine-learning-data-science-customize-hadoop-cluster.md) aanpassen

  - Het opslag-account dat is gemaakt met uw cluster HDInsight wanneer deze is gemaakt, moet u koppelen. Dit account opslag wordt gebruikt voor toegang tot gegevens die binnen het cluster kan worden verwerkt.

![](./media/machine-learning-data-science-vm-do-ten-things/Create_HDI_v4.PNG)

  - Nadat deze is gemaakt, moet u naar het hoofd knooppunt van het cluster **Externe toegang** inschakelen. Onthoud dat de RAS-referenties die u hier opgeeft (anders dan die zijn opgegeven voor het cluster bij het is gemaakt): u moet ze onderstaande.

![](./media/machine-learning-data-science-vm-do-ten-things/Create_HDI_dashboard_v3.PNG)

  - Een werkruimte Azure ML maken. Uw Machine Learning-experimenten worden opgeslagen in deze ML-werkruimte. Selecteer de gemarkeerde opties in de Portal zoals wordt weergegeven in de onderstaande schermafbeelding.

![](./media/machine-learning-data-science-vm-do-ten-things/Create_ML_Space.PNG)


  - Voer vervolgens de parameters voor uw werkruimte Azure ML

![](./media/machine-learning-data-science-vm-do-ten-things/Create_ML_Space_step2_v2.PNG)

  - Gegevens met behulp van IPython notitieblok uploaden. Vereiste pakketten eerst te importeren, sluit referenties, een database maken in uw account voor de opslag en gegevens met HDI clusters laden.


        #Import required Packages
        import pyodbc
        import time as time
        import json
        import os
        import urllib
        import urllib2
        import warnings
        import re
        import pandas as pd
        import matplotlib.pyplot as plt
        from azure.storage.blob import BlobService
        warnings.filterwarnings("ignore", category=UserWarning, module='urllib2')


        #Create the connection to Hive using ODBC
        SERVER_NAME='xxx.azurehdinsight.net'
        DATABASE_NAME='nyctaxidb'
        USERID='xxx'
        PASSWORD='xxxx'
        DB_DRIVER='Microsoft Hive ODBC Driver'
        driver = 'DRIVER={' + DB_DRIVER + '}'
        server = 'Host=' + SERVER_NAME + ';Port=443'
        database = 'Schema=' + DATABASE_NAME
        hiveserv = 'HiveServerType=2'
        auth = 'AuthMech=6'
        uid = 'UID=' + USERID
        pwd = 'PWD=' + PASSWORD
        CONNECTION_STRING = ';'.join([driver,server,database,hiveserv,auth,uid,pwd])
        connection = pyodbc.connect(CONNECTION_STRING, autocommit=True)
        cursor=connection.cursor()


        #Create Hive database and tables
        queryString = "create database if not exists nyctaxidb;"
        cursor.execute(queryString)

        queryString = """
                        create external table if not exists nyctaxidb.trip
                        (
                            medallion string,
                            hack_license string,
                            vendor_id string,
                            rate_code string,
                            store_and_fwd_flag string,
                            pickup_datetime string,
                            dropoff_datetime string,
                            passenger_count int,
                            trip_time_in_secs double,
                            trip_distance double,
                            pickup_longitude double,
                            pickup_latitude double,
                            dropoff_longitude double,
                            dropoff_latitude double)  
                        PARTITIONED BY (month int)
                        ROW FORMAT DELIMITED FIELDS TERMINATED BY ',' lines terminated by '\\n'
                        STORED AS TEXTFILE LOCATION 'wasb:///nyctaxidbdata/trip' TBLPROPERTIES('skip.header.line.count'='1');
                    """
        cursor.execute(queryString)

        queryString = """
                        create external table if not exists nyctaxidb.fare
                        (
                            medallion string,
                            hack_license string,
                            vendor_id string,
                            pickup_datetime string,
                            payment_type string,
                            fare_amount double,
                            surcharge double,
                            mta_tax double,
                            tip_amount double,
                            tolls_amount double,
                            total_amount double)
                        PARTITIONED BY (month int)
                        ROW FORMAT DELIMITED FIELDS TERMINATED BY ',' lines terminated by '\\n'
                        STORED AS TEXTFILE LOCATION 'wasb:///nyctaxidbdata/fare' TBLPROPERTIES('skip.header.line.count'='1');
                    """
        cursor.execute(queryString)


        #Upload data from blob storage to HDI cluster
        for i in range(1,13):
            queryString = "LOAD DATA INPATH 'wasb:///nyctaxitripraw2/trip_data_%d.csv' INTO TABLE nyctaxidb2.trip PARTITION (month=%d);"%(i,i)
            cursor.execute(queryString)
            queryString = "LOAD DATA INPATH 'wasb:///nyctaxifareraw2/trip_fare_%d.csv' INTO TABLE nyctaxidb2.fare PARTITION (month=%d);"%(i,i)  
            cursor.execute(queryString)


- U kunt ook in dit [scenario](machine-learning-data-science-process-hive-walkthrough.md) tevens Taxi gegevens uploaden naar HDI cluster volgen. Belangrijke stappen zijn:

    - AzCopy: ingepakte CSV van openbare blob downloaden naar uw lokale map
    - AzCopy: uitgepakt CSV van uit lokale map aan HDI cluster uploaden
    - Meld u aan bij het hoofd knooppunt van Hadoop cluster en voorbereiden voor experimentele gegevensanalyse

Nadat de gegevens zijn geladen aan HDI cluster, kunt u uw gegevens in Azure opslag Explorer kunt controleren. En u een database nyctaxidb die is gemaakt in HDI cluster hebben.


**Gegevens verkennen: component query's in Python**

Omdat de gegevens zich in Hadoop cluster, kunt u het pakket pyodbc verbinding maken met Hadoop Clusters en query database via een component moet u doen verkennen en technische aanbevelen. U kunt de bestaande tabellen die u hebt gemaakt in de vereiste stap weergeven.

    queryString = """
        show tables in nyctaxidb2;
        """
    pd.read_sql(queryString,connection)


![](./media/machine-learning-data-science-vm-do-ten-things/Python_View_Existing_Tables_Hive_v3.PNG)

Bekijk het aantal records in elke maand en de frequentie van Gekantelde of niet in de tabel reis:

    queryString = """
        select month, count(*) from nyctaxidb.trip group by month;
        """
    results = pd.read_sql(queryString,connection)

    %matplotlib inline

    results.columns = ['month', 'trip_count']
    df = results.copy()
    df.index = df['month']
    df['trip_count'].plot(kind='bar')


![](./media/machine-learning-data-science-vm-do-ten-things/Exploration_Number_Records_by_Month_v3.PNG)


    queryString = """
        SELECT tipped, COUNT(*) AS tip_freq
        FROM
        (
            SELECT if(tip_amount > 0, 1, 0) as tipped, tip_amount
            FROM nyctaxidb.fare
        )tc
        GROUP BY tipped;
        """
    results = pd.read_sql(queryString,connection)

    results.columns = ['tipped', 'trip_count']
    df = results.copy()
    df.index = df['tipped']
    df['trip_count'].plot(kind='bar')


![](./media/machine-learning-data-science-vm-do-ten-things/Exploration_Frequency_tip_or_not_v3.PNG)

We kunnen ook de afstand tussen ophalen locatie en dropoff locatie te berekenen en vergelijk deze met de reis-afstand.

    queryString = """
                    select pickup_longitude, pickup_latitude, dropoff_longitude, dropoff_latitude, trip_distance, trip_time_in_secs,
                        3959*2*2*atan((1-sqrt(1-pow(sin((dropoff_latitude-pickup_latitude)
                        *radians(180)/180/2),2)-cos(pickup_latitude*radians(180)/180)
                        *cos(dropoff_latitude*radians(180)/180)*pow(sin((dropoff_longitude-pickup_longitude)*radians(180)/180/2),2)))
                        /sqrt(pow(sin((dropoff_latitude-pickup_latitude)*radians(180)/180/2),2)
                        +cos(pickup_latitude*radians(180)/180)*cos(dropoff_latitude*radians(180)/180)*
                        pow(sin((dropoff_longitude-pickup_longitude)*radians(180)/180/2),2))) as direct_distance
                        from nyctaxidb.trip
                        where month=1
                            and pickup_longitude between -90 and -30
                            and pickup_latitude between 30 and 90
                            and dropoff_longitude between -90 and -30
                            and dropoff_latitude between 30 and 90;
                """
    results = pd.read_sql(queryString,connection)
    results.head(5)


![](./media/machine-learning-data-science-vm-do-ten-things/Exploration_compute_pickup_dropoff_distance_v2.PNG)

    results.columns = ['pickup_longitude', 'pickup_latitude', 'dropoff_longitude',
                       'dropoff_latitude', 'trip_distance', 'trip_time_in_secs', 'direct_distance']
    df = results.loc[results['trip_distance']<=100] #remove outliers
    df = df.loc[df['direct_distance']<=100] #remove outliers
    plt.scatter(df['direct_distance'], df['trip_distance'])


![](./media/machine-learning-data-science-vm-do-ten-things/Exploration_direct_distance_trip_distance_v2.PNG)


Nu we een gedownsampled (1%) reeks gegevens voorbereiden voor modellering. We kunnen deze gegevens gebruiken in AML reader module.


        queryString = """
        create  table if not exists nyctaxi_downsampled_dataset_testNEW (
        medallion string,
        hack_license string,
        vendor_id string,
        rate_code string,
        store_and_fwd_flag string,
        pickup_datetime string,
        dropoff_datetime string,
        pickup_hour string,
        pickup_week string,
        weekday string,
        passenger_count int,
        trip_time_in_secs double,
        trip_distance double,
        pickup_longitude double,
        pickup_latitude double,
        dropoff_longitude double,
        dropoff_latitude double,
        direct_distance double,
        payment_type string,
        fare_amount double,
        surcharge double,
        mta_tax double,
        tip_amount double,
        tolls_amount double,
        total_amount double,
        tipped string,
        tip_class string
        )
        row format delimited fields terminated by ','
        lines terminated by '\\n'
        stored as textfile;
        """
        cursor.execute(queryString)

        --- now insert contents of the join into the above internal table

        queryString = """
        insert overwrite table nyctaxi_downsampled_dataset_testNEW
        select
        t.medallion,
        t.hack_license,
        t.vendor_id,
        t.rate_code,
        t.store_and_fwd_flag,
        t.pickup_datetime,
        t.dropoff_datetime,
        hour(t.pickup_datetime) as pickup_hour,
        weekofyear(t.pickup_datetime) as pickup_week,
        from_unixtime(unix_timestamp(t.pickup_datetime, 'yyyy-MM-dd HH:mm:ss'),'u') as weekday,
        t.passenger_count,
        t.trip_time_in_secs,
        t.trip_distance,
        t.pickup_longitude,
        t.pickup_latitude,
        t.dropoff_longitude,
        t.dropoff_latitude,
        t.direct_distance,
        f.payment_type,
        f.fare_amount,
        f.surcharge,
        f.mta_tax,
        f.tip_amount,
        f.tolls_amount,
        f.total_amount,
        if(tip_amount>0,1,0) as tipped,
        if(tip_amount=0,0,
        if(tip_amount>0 and tip_amount<=5,1,
        if(tip_amount>5 and tip_amount<=10,2,
        if(tip_amount>10 and tip_amount<=20,3,4)))) as tip_class
        from
        (
        select
        medallion,
        hack_license,
        vendor_id,
        rate_code,
        store_and_fwd_flag,
        pickup_datetime,
        dropoff_datetime,
        passenger_count,
        trip_time_in_secs,
        trip_distance,
        pickup_longitude,
        pickup_latitude,
        dropoff_longitude,
        dropoff_latitude,
        3959*2*2*atan((1-sqrt(1-pow(sin((dropoff_latitude-pickup_latitude)
        radians(180)/180/2),2)-cos(pickup_latitude*radians(180)/180)
        *cos(dropoff_latitude*radians(180)/180)*pow(sin((dropoff_longitude-pickup_longitude)*radians(180)/180/2),2)))
        /sqrt(pow(sin((dropoff_latitude-pickup_latitude)*radians(180)/180/2),2)
        +cos(pickup_latitude*radians(180)/180)*cos(dropoff_latitude*radians(180)/180)*pow(sin((dropoff_longitude-pickup_longitude)*radians(180)/180/2),2))) as direct_distance,
        rand() as sample_key

        from trip
        where pickup_latitude between 30 and 90
            and pickup_longitude between -90 and -30
            and dropoff_latitude between 30 and 90
            and dropoff_longitude between -90 and -30
        )t
        join
        (
        select
        medallion,
        hack_license,
        vendor_id,
        pickup_datetime,
        payment_type,
        fare_amount,
        surcharge,
        mta_tax,
        tip_amount,
        tolls_amount,
        total_amount
        from fare
        )f
        on t.medallion=f.medallion and t.hack_license=f.hack_license and t.pickup_datetime=f.pickup_datetime
        where t.sample_key<=0.01
        """
        cursor.execute(queryString)

Na verloop van tijd ziet u dat de gegevens is geladen in Hadoop clusters:

    queryString = """
        select * from nyctaxi_downsampled_dataset limit 10;
        """
    cursor.execute(queryString)
    pd.read_sql(queryString,connection)


![](./media/machine-learning-data-science-vm-do-ten-things/DownSample_Data_For_Modeling_v2.PNG)


**Gegevens lezen van HDI met AML: reader module**

U kunt ook de module **reader** in AML studio toegang heeft tot de database in Hadoop cluster. Sluit de referenties van uw HDI clusters en Azure opslag-Account en is mogelijk om machine learning-modellen database gebruikt in HDI clusters te maken.

![](./media/machine-learning-data-science-vm-do-ten-things/AML_Reader_Hive.PNG)

De scored gegevensset kan vervolgens worden weergegeven:

![](./media/machine-learning-data-science-vm-do-ten-things/AML_Model_Results.PNG)


### <a name="azure-sql-data-warehouse--databases"></a>Azure SQL Data Warehouse & databases

Azure SQL Data Warehouse is een gegevenswarehouse elastische als een service met SQL Server-ervaring voor ondernemingen.

U kunt uw Azure SQL Data Warehouse inrichten volgens de instructies in dit [artikel](../sql-data-warehouse/sql-data-warehouse-get-started-provision.md). Nadat u uw Azure SQL Data Warehouse inrichten, kunt u deze [procedure](machine-learning-data-science-process-sqldw-walkthrough.md) moet gegevens uploaden, te verkennen en het gebruik van gegevens binnen de SQL-Data Warehouse modellering.

#### <a name="azure-documentdb"></a>Azure DocumentDB

Azure DocumentDB is een database NoSQL in de cloud. Dit kunt u werken met documenten zoals JSON en kunt u op te slaan en de documenten opvragen.

U moet de volgende stappen per materiaal voor toegang tot DocumentDB uit de DSVM doen.

1. DocumentDB Python SDK installeren (uitvoeren ```pip install pydocumentdb``` vanaf opdrachtprompt)
1. DocumentDB-account en Document DB database maken van [Azure-portal](https://portal.azure.com)
1. "DocumentDB Migratiehulpmiddel" vanaf [hier](http://www.microsoft.com/downloads/details.aspx?FamilyID=cda7703a-2774-4c07-adcc-ad02ddc1a44d) gedownload en uitgepakt naar een map van uw keuze
1. Importeer JSON-gegevens (Vulkaan gegevens) opgeslagen op een [openbare blob](https://cahandson.blob.core.windows.net/samples/volcano.json) in DocumentDB met het volgen van de opdrachtparameters naar het hulpprogramma voor migratie (dtui.exe uit de map waarin u het Migratiehulpmiddel DocumentDB hebt geïnstalleerd). Voer de bron en locatieparameters uit het volgende afstemmen.

    /s:JsonFile /s.Files:https://cahandson.blob.core.windows.net/samples/volcano.json /t:DocumentDBBulk /t.ConnectionString:AccountEndpoint=https://[DocDBAccountName].documents.azure.com:443/; AccountKey = [[belangrijke]; Database = Vulkaan /t.Collection:volcano1

Nadat u de gegevens importeren, kunt u Ga naar Jupyter en open het notitieblok getiteld *DocumentDBSample* waarin python code voor toegang tot DocumentDB en voer enkele eenvoudige query's uitvoeren. U meer informatie over DocumentDB kunt via de service- [documentatiepagina](https://azure.microsoft.com/documentation/learning-paths/documentdb/)


## <a name="8-build-reports-and-dashboard-using-the-power-bi-desktop"></a>8. het maken van rapporten en dashboards die de Power BI Desktop gebruiken

Laat ons het Vulkaan JSON-bestand dat hebt gezien in het bovenstaande voorbeeld DocumentDB in Power BI om het nut van visuele inzicht krijgen in de gegevens te visualiseren. Gedetailleerde stappen zijn beschikbaar in het [artikel Power BI](../documentdb/documentdb-powerbi-visualize.md). De hoogste niveau stappen zijn hieronder:

1. Power BI Desktop openen en daar "Gegevens ophalen". Geef de URL als: https://cahandson.blob.core.windows.net/samples/volcano.json
2. U ziet dat de JSON-records die als een lijst worden geïmporteerd
3. De lijst converteren naar een tabel, zodat PowerBI met dezelfde werken kunt
4. De kolommen uitvouwen door te klikken op het pictogram voor het uitvouwen (de knop met het pictogram 'pijl-links en een pijl naar rechts' aan de rechterkant van de kolom)
5. Zoals u ziet dat zich in een recordveld "". Vouw de record en selecteert u alleen de coördinaten. Coördinaten is een lijstkolom
6. Een nieuwe kolom voor de coördinaten lijstkolom converteren naar een aparte LatLong-kolom van door komma's, het samenvoegen van de twee elementen in het lijstveld coördinaten is met de formule toevoegen ```Text.From([coordinates]{1})&","&Text.From([coordinates]{0})```.
7. Ten slotte converteren de ```Elevation``` kolom aan decimaal getal en selecteer de **sluiten** en **toepassen**.

In plaats van de bovenstaande stappen, kunt u de volgende code dat scripts de bovenstaande stappen in de geavanceerde Editor in PowerBI waarmee u de gegevenstransformaties schrijven in een querytaal out plakken.


    let
        Source = Json.Document(Web.Contents("https://cahandson.blob.core.windows.net/samples/volcano.json")),
        #"Converted to Table" = Table.FromList(Source, Splitter.SplitByNothing(), null, null, ExtraValues.Error),
        #"Expanded Column1" = Table.ExpandRecordColumn(#"Converted to Table", "Column1", {"Volcano Name", "Country", "Region", "Location", "Elevation", "Type", "Status", "Last Known Eruption", "id"}, {"Volcano Name", "Country", "Region", "Location", "Elevation", "Type", "Status", "Last Known Eruption", "id"}),
        #"Expanded Location" = Table.ExpandRecordColumn(#"Expanded Column1", "Location", {"coordinates"}, {"coordinates"}),
        #"Added Custom" = Table.AddColumn(#"Expanded Location", "LatLong", each Text.From([coordinates]{1})&","&Text.From([coordinates]{0})),
        #"Changed Type" = Table.TransformColumnTypes(#"Added Custom",{{"Elevation", type number}})
    in
        #"Changed Type"



U hebt nu de gegevens in uw gegevensmodel Power BI. Uw Power BI-bureaublad moet er uitzien zoals hieronder wordt weergegeven.

![](./media/machine-learning-data-science-vm-do-ten-things/PowerBIVolcanoData.png)

U kunt beginnen met het samenstellen van rapporten en visualisaties met het gegevensmodel. U kunt de stappen in dit [artikel voor Power BI](../documentdb/documentdb-powerbi-visualize.md#build-the-reports) om een rapport te maken. Het eindresultaat is een rapport dat ziet er als volgt uit.

![Power BI Desktop rapportweergave - Power BI-connector](./media/machine-learning-data-science-vm-do-ten-things/power_bi_connector_pbireportview2.png)

## <a name="9-dynamically-scale-your-dsvm-to-meet-your-project-needs"></a>9. dynamisch schalen dat uw DSVM de behoeften van uw project

U kunt schalen omhoog en omlaag de DSVM om te voldoen aan de projectbehoeften van uw. Als u niet nodig hebt voor het gebruik van de VM in de buiten kantooruren of in het weekend, kunt u alleen de VM vanaf de [portal van Azure](https://portal.azure.com)afsluiten.

>[AZURE.NOTE]  U wordt berekeningscluster kosten in rekening gebracht als u alleen de knop van het besturingssysteem afsluiten op de VM.  

Als u moet een grote schaal analyse verwerken en nodig meer CPU en/of geheugen en/of schijf capaciteit vindt u een grote selectie van VM formaten CPU-cores, het geheugen en schijftypen (inclusief effen staat stations) die voldoen aan uw berekeningscluster en budgettaire behoeften. De volledige lijst met VMs samen met hun per uur berekenen prijzen is beschikbaar op de pagina [Azure virtuele Machines prijzen](https://azure.microsoft.com/pricing/details/virtual-machines/) .

Op dezelfde manier als uw behoefte aan VM verwerkingscapaciteit Hiermee reduceert (bijvoorbeeld: u een primaire werkbelasting verplaatst naar een Hadoop of een cluster), kunt u de schaal het cluster van de [Azure-portal](https://portal.azure.com) en gaan om de instellingen van uw exemplaar VM af. Hier ziet u een schermafbeelding.


![](./media/machine-learning-data-science-vm-do-ten-things/VMScaling.PNG)


## <a name="10-install-additional-tools-on-your-virtual-machine"></a>10. aanvullende hulpmiddelen voor installeren op uw VM

We hebben verschillende hulpprogramma's die wij het kunnen ook veel van de algemene gegevens analytics nodig heeft adres verpakt en dat moet u tijd kunt besparen door te voorkomen dat u hoeft te installeren en configureren van uw omgevingen één voor één en bespaar door te betalen alleen voor resources die u gebruikt.

U kunt gebruikmaken van andere Azure gegevens en analyses services profiel in dit artikel voor het verbeteren van uw analytics-omgeving. Wij begrijpen dat in sommige gevallen uw behoeften mogelijk extra hulpprogramma's, inclusief enkele technieken, bedrijfseigen van derden. U hebt volledige beheerdersbevoegdheden op de virtuele machine voor het installeren van nieuwe hulpmiddelen die u nodig hebt. U kunt ook extra pakketten installeren in Python en R die niet vooraf zijn geïnstalleerd. Voor Python gebruikt u een ```conda``` of ```pip```. Voor R kunt u de ```install.packages()``` console in de R of de IDE gebruiken en kies "**pakketten** -> **Pakketten installeren**'.

## <a name="summary"></a>Overzicht
Dit zijn slechts enkele dingen die u op de Microsoft wetenschappelijke virtuele Machine gegevens kunt doen. Er zijn veel meer dingen die u doen kunt om dit te maken een effectieve analytics-omgeving.
