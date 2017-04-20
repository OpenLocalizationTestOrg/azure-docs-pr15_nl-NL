<properties
pageTitle="Meerdere modellen maken op basis van een experiment | Microsoft Azure"
description="PowerShell gebruiken om u te maken van meerdere Machine Learning-modellen en web-service eindpunten met dezelfde algoritme, maar andere training gegevenssets."
services="machine-learning"
documentationCenter=""
authors="hning86"
manager="jhubbard"
editor="cgronlun"/>

<tags
ms.service="machine-learning"
ms.workload="data-services"
ms.tgt_pltfrm="na"
ms.devlang="na"
ms.topic="article"
ms.date="10/03/2016"
ms.author="garye;haining"/>

# <a name="create-many-machine-learning-models-and-web-service-endpoints-from-one-experiment-using-powershell"></a>Veel Machine Learning-modellen en web service eindpunten maken van een experiment via PowerShell

Hier volgt een veelvoorkomend machine learning-probleem: U wilt maken van veel modellen die hebben dezelfde werkstroom training en dezelfde algoritme gebruiken, maar andere training gegevenssets als invoer. Dit artikel leest u hoe u dit bij het op schaal in Azure Machine Learning Studio met alleen een enkele experiment.

Stel dat u eigenaar bent van een globale fietsen huur franchise bedrijf. U wilt maken van een regressiemodel voor het voorspellen van de huur vraag op basis van historische gegevens. U 1000 huur locaties kant van de wereld zitten hebt en u een gegevensset voor elke locatie met belangrijke functies zoals datum, tijd, weer en verkeer die specifiek zijn voor elke locatie hebt verzameld.

U kunt uw model met één keer een samengevoegde versie van alle gegevenssets alle locaties kan trainen. Maar omdat elk van uw locaties een unieke omgeving heeft, een betere benadering zou zijn om te trainen uw regressiemodel afzonderlijk met behulp van de gegevensset voor elke locatie. Op die manier elk ervaren model kan rekening houden de verschillende store formaten, volume, Geografie, populatie, fietsen beschrijvende-verkeer-omgeving, *enzovoort*.

Die mogelijk de beste manier, maar u niet wilt maken van 1000 training experimenten in Azure Machine Learning met ieder een die een unieke locatie voorstelt. Alternatieve manieren een problematisch taak, het is ook heel niet efficiënt lijkt, omdat elk experiment dezelfde onderdelen, behalve voor de gegevensset training hebben zou.

Gelukkig kunt we dit doen met de [Azure Machine Learning bijscholing API](machine-learning-retrain-models-programmatically.md) en de taak met [Azure Machine Learning PowerShell](machine-learning-powershell-module.md)automatiseren.

> [AZURE.NOTE] Als u ons voorbeeld sneller worden uitgevoerd, verlagen we het aantal plaatsen van 1000 tot en met 10. Maar de dezelfde beginselen en procedures van toepassing op 1000 locaties. Het enige verschil is dat als u wilt trainen van 1000 gegevenssets u waarschijnlijk om na te denken van de uitvoering van de volgende PowerShell-scripts parallel. Hoe u dat doen valt buiten het bereik van dit artikel, maar u voorbeelden van PowerShell multithreading kunt vinden op Internet.  

## <a name="set-up-the-training-experiment"></a>Het experiment training instellen

Gaan we een voorbeeld [training experimenteren](https://gallery.cortanaintelligence.com/Experiment/Bike-Rental-Training-Experiment-1) die we al hebt gemaakt in de [Galerie met Cortana Intelligence](http://gallery.cortanaintelligence.com)gebruiken. Dit experiment in uw werkruimte [Azure Machine Learning Studio](https://studio.azureml.net) geopend.

>[AZURE.NOTE] Om te kunnen volgen samen met het volgende voorbeeld, wilt u mogelijk een standaard-werkruimte in plaats van een gratis Groove-werkruimte gebruiken. We één eindpunt maakt voor elke klant - voor een totaal van 10 eindpunten - en waarvoor u een standaard-werkruimte aangezien een gratis werkruimte beperkt tot 3 eindpunten is. Als u alleen een gratis werkruimte hebt, moet u alleen de onderstaande om te staan voor alleen 3 locaties scripts wijzigen.

Het experiment wordt een module **Gegevens importeren** om de training gegevensset *customer001.csv* importeren uit een Azure opslag-account. Stel dat we hebben die training gegevenssets van alle fietsen huur locaties worden verzameld en opgeslagen in dezelfde blob storage locatie met bestandsnamen die variëren van *rentalloc001.csv* naar *rentalloc10.csv*.

![afbeelding](./media/machine-learning-create-models-and-endpoints-with-powershell/reader-module.png)

Houd er rekening mee dat een module **Web Service uitvoer** is toegevoegd aan de module **Trein Model** .
Wanneer dit experiment is geïmplementeerd als een webservice, het eindpunt dat is gekoppeld aan dat uitvoer het ervaren model in de indeling van een bestand .ilearner wordt geretourneerd.

Bedenk ook dat u stelt een web-service-parameter voor de URL die in de module **Gegevens importeren** wordt gebruikt. Deze manier kunnen wij de parameter gebruiken om op te geven van afzonderlijke training gegevenssets om te trainen van het model voor elke locatie.
Er zijn andere manieren die we kan dit hebt gedaan, zoals een SQL-query gebruiken met een parameter van de service web naar gegevens ophalen uit een SQL Azure-database of te gebruiken om een module **Web Service invoer** in een gegevensset om aan te geven de webservice.

![afbeelding](./media/machine-learning-create-models-and-endpoints-with-powershell/web-service-output.png)

Nu kunnen we dit training experiment de standaard waarde *rental001.csv* gebruiken als de gegevensset training uitvoeren. Als u de uitvoer van de module **evalueren** weergeven (Klik op de uitvoer en selecteer **visualiseren**), kunt u zien we een goede prestaties van *AUC* krijgen 0.91 =. Nu we gaan een webservice afmelden bij dit experiment training implementeren.

## <a name="deploy-the-training-and-scoring-web-services"></a>De training en webservices scoren implementeren

Als u wilt implementeren de webservice training, klikt u op de knop **Webservice instellen** onder het tekenpapier experiment en selecteer **Webservice implementeren**. Roep deze webservice '"fietsen huur Training'.

Nu moeten we de score webservice implementeren.
We kunnen hiervoor **Webservice instellen** Klik onder het tekenpapier en selecteer **Blog webservice**. Hiermee maakt u een score experiment.
Moeten we een paar secundaire aanpassen zodat u deze werken als een webservice, zoals het label kolom "CT" verwijderen vanaf de invoergegevens en de uitvoer naar alleen de exemplaar-id en de bijbehorende beperken voorspeld waarde.

Als u wilt opslaan uzelf die kunnen worden geopend, kunt u gewoon het [blog experiment](https://gallery.cortanaintelligence.com/Experiment/Bike-Rental-Predicative-Experiment-1) openen in de galerie die al is voorbereid.

Als u wilt implementeren de webservice, het blog experiment uitvoeren en klik op de knop **Webservice implementeren** onder het tekenpapier. De score webservice "Fietsen huur scoren" naam ".

## <a name="create-10-identical-web-service-endpoints-with-powershell"></a>10 eindpunten van een identieke webservice maken met PowerShell

Deze webservice wordt geleverd met een standaardeindpunt. Maar we niet als geïnteresseerd bent in de standaardeindpunt aangezien niet worden bijgewerkt. Wat er moet doen is het opzetten van 10 extra eindpunten, één voor elke locatie. Dit doen we met PowerShell.

Eerst instellen we onze PowerShell-omgeving:

    Import-Module .\AzureMLPS.dll
    # Assume the default configuration file exists and is properly set to point to the valid Workspace.
    $scoringSvc = Get-AmlWebService | where Name -eq 'Bike Rental Scoring'
    $trainingSvc = Get-AmlWebService | where Name -eq 'Bike Rental Training'

Voer de volgende PowerShell-opdracht:

    # Create 10 endpoints on the scoring web service.
    For ($i = 1; $i -le 10; $i++){
        $seq = $i.ToString().PadLeft(3, '0');
        $endpointName = 'rentalloc' + $seq;
        Write-Host ('adding endpoint ' + $endpointName + '...')
        Add-AmlWebServiceEndpoint -WebServiceId $scoringSvc.Id -EndpointName $endpointName -Description $endpointName     
    }

Nu we 10 eindpunten hebt gemaakt en worden alle hetzelfde ervaren model training op *customer001.csv*bevatten. U kunt deze kunt weergeven in de beheerportal Azure.

![afbeelding](./media/machine-learning-create-models-and-endpoints-with-powershell/created-endpoints.png)

## <a name="update-the-endpoints-to-use-separate-training-datasets-using-powershell"></a>De eindpunten als u wilt gebruiken afzonderlijk training gegevenssets via PowerShell bijwerken

De volgende stap is het bijwerken van de eindpunten met modellen uniek training op elke afzonderlijke klantgegevens. Maar eerst moeten we deze modellen van de webservice **Fietsen huur Training** produceren. In dit artikel gaat u terug naar de webservice **Fietsen huur Training** . Moeten we het eindpunt BES 10 tijden met 10 verschillende training gegevenssets bellen om te kunnen produceren 10 verschillende modellen. We gebruiken de PowerShell-cmdlet **InovkeAmlWebServiceBESEndpoint** u dit wilt doen.

U moet ook om uw referenties voor uw account blob storage in `$configContent`, dat wil zeggen de velden `AccountName`, `AccountKey` en `RelativeLocation`. De `AccountName` zijn de accountnamen van uw, zoals gezien in de **Klassieke Azure-beheerportal** (tabblad*opslag* ). Wanneer u op een account opslag, op de `AccountKey` kunt u vinden door op de knop **Toegangstoetsen beheren** onder te drukken en de *Primaire sleutel van Access*kopiëren. De `RelativeLocation` is het pad ten opzichte van uw opslag waar een nieuw model worden opgeslagen. Bijvoorbeeld het pad `hai/retrain/bike_rental/` in het volgende wordt verwezen naar een container met de naam script `hai`, en `/retrain/bike_rental/` submappen zijn. Momenteel, u kunt geen submappen via de portal UI maken, maar er zijn [Verschillende Azure opslag Explorers](../storage/storage-explorers.md) waarmee u kunt doen. Het wordt aanbevolen dat u een nieuwe container in uw opslag maken voor de opslag van de nieuwe ervaren modellen (.ilearner-bestanden) als volgt: de opslagpagina, klikt u op de knop **toevoegen** aan de onderkant en noem deze `retrain`. In het overzicht, de noodzakelijke wijzigingen aan het onderstaande script betrekking hebben op `AccountName`, `AccountKey` en `RelativeLocation` (:`"retrain/model' + $seq + '.ilearner"`).

    # Invoke the retraining API 10 times
    # This is the default (and the only) endpoint on the training web service
    $trainingSvcEp = (Get-AmlWebServiceEndpoint -WebServiceId $trainingSvc.Id)[0];
    $submitJobRequestUrl = $trainingSvcEp.ApiLocation + '/jobs?api-version=2.0';
    $apiKey = $trainingSvcEp.PrimaryKey;
    For ($i = 1; $i -le 10; $i++){
        $seq = $i.ToString().PadLeft(3, '0');
        $inputFileName = 'https://bostonmtc.blob.core.windows.net/hai/retrain/bike_rental/BikeRental' + $seq + '.csv';
        $configContent = '{ "GlobalParameters": { "URI": "' + $inputFileName + '" }, "Outputs": { "output1": { "ConnectionString": "DefaultEndpointsProtocol=https;AccountName=<myaccount>;AccountKey=<mykey>", "RelativeLocation": "hai/retrain/bike_rental/model' + $seq + '.ilearner" } } }';
        Write-Host ('training regression model on ' + $inputFileName + ' for rental location ' + $seq + '...');
        Invoke-AmlWebServiceBESEndpoint -JobConfigString $configContent -SubmitJobRequestUrl $submitJobRequestUrl -ApiKey $apiKey
    }

>[AZURE.NOTE] Het eindpunt BES is de enige ondersteunde modus voor deze bewerking. Records kunnen niet worden gebruikt voor het maken van ervaren modellen.

Zoals u ziet boven, in plaats van het bouwen van 10 verschillende BES taak configuratie json bestanden, we dynamisch in plaats daarvan de configuratietekenreeks maken en deze feed aan de *jobConfigString* -parameter van de cmdlet **InvokeAmlWebServceBESEndpoint** , omdat er echt hoeft geen kopie te bewaren op schijf.

Als alles goed gaat, na verloop van tijd ziet u 10 .ilearner-bestanden, uit *model001.ilearner* naar *model010.ilearner*, in uw account Azure opslag. Nu we onze 10 score eindpunten van een webservice met deze modellen met de PowerShell-cmdlet **Patch-AmlWebServiceEndpoint** bijwerken. Vergeet niet opnieuw kunt dat we alleen patch de eindpunten van de niet-standaard die we via programmacode eerder hebt gemaakt.

    # Patch the 10 endpoints with respective .ilearner models
    $baseLoc = 'http://bostonmtc.blob.core.windows.net/'
    $sasToken = '<my_blob_sas_token>'
    For ($i = 1; $i -le 10; $i++){
        $seq = $i.ToString().PadLeft(3, '0');
        $endpointName = 'rentalloc' + $seq;
        $relativeLoc = 'hai/retrain/bike_rental/model' + $seq + '.ilearner';
        Write-Host ('Patching endpoint ' + $endpointName + '...');
        Patch-AmlWebServiceEndpoint -WebServiceId $scoringSvc.Id -EndpointName $endpointName -ResourceName 'Bike Rental [trained model]' -BaseLocation $baseLoc -RelativeLocation $relativeLoc -SasBlobToken $sasToken
    }

Dit moet vrij snel uitgevoerd. Wanneer de uitvoering is voltooid, wordt we hebt gemaakt 10 blog eindpunten van een webservice, elk met een ervaren model uniek training op de specifieke gegevensset op een Huurlocatie, alles van een experiment van één training. U kunt dit controleren, kunt u proberen deze met de cmdlet **InvokeAmlWebServiceRRSEndpoint** eindpunten bellen zodat iedereen over de dezelfde invoergegevens en u kunt verwachten verschillende tekstvoorspelling om resultaten te bekijken omdat de modellen zijn ervaren met verschillende training sets.

## <a name="full-powershell-script"></a>Volledige PowerShell-script

Hier ziet de vermelding van de volledige broncode:

    Import-Module .\AzureMLPS.dll
    # Assume the default configuration file exists and properly set to point to the valid workspace.
    $scoringSvc = Get-AmlWebService | where Name -eq 'Bike Rental Scoring'
    $trainingSvc = Get-AmlWebService | where Name -eq 'Bike Rental Training'

    # Create 10 endpoints on the scoring web service
    For ($i = 1; $i -le 10; $i++){
        $seq = $i.ToString().PadLeft(3, '0');
        $endpointName = 'rentalloc' + $seq;
        Write-Host ('adding endpoint ' + $endpontName + '...')
        Add-AmlWebServiceEndpoint -WebServiceId $scoringSvc.Id -EndpointName $endpointName -Description $endpointName     
    }

    # Invoke the retraining API 10 times to produce 10 regression models in .ilearner format
    $trainingSvcEp = (Get-AmlWebServiceEndpoint -WebServiceId $trainingSvc.Id)[0];
    $submitJobRequestUrl = $trainingSvcEp.ApiLocation + '/jobs?api-version=2.0';
    $apiKey = $trainingSvcEp.PrimaryKey;
    For ($i = 1; $i -le 10; $i++){
        $seq = $i.ToString().PadLeft(3, '0');
        $inputFileName = 'https://bostonmtc.blob.core.windows.net/hai/retrain/bike_rental/BikeRental' + $seq + '.csv';
        $configContent = '{ "GlobalParameters": { "URI": "' + $inputFileName + '" }, "Outputs": { "output1": { "ConnectionString": "DefaultEndpointsProtocol=https;AccountName=<myaccount>;AccountKey=<mykey>", "RelativeLocation": "hai/retrain/bike_rental/model' + $seq + '.ilearner" } } }';
        Write-Host ('training regression model on ' + $inputFileName + ' for rental location ' + $seq + '...');
        Invoke-AmlWebServiceBESEndpoint -JobConfigString $configContent -SubmitJobRequestUrl $submitJobRequestUrl -ApiKey $apiKey
    }

    # Patch the 10 endpoints with respective .ilearner models
    $baseLoc = 'http://bostonmtc.blob.core.windows.net/'
    $sasToken = '?test'
    For ($i = 1; $i -le 10; $i++){
        $seq = $i.ToString().PadLeft(3, '0');
        $endpointName = 'rentalloc' + $seq;
        $relativeLoc = 'hai/retrain/bike_rental/model' + $seq + '.ilearner';
        Write-Host ('Patching endpoint ' + $endpointName + '...');
        Patch-AmlWebServiceEndpoint -WebServiceId $scoringSvc.Id -EndpointName $endpointName -ResourceName 'Bike Rental [trained model]' -BaseLocation $baseLoc -RelativeLocation $relativeLoc -SasBlobToken $sasToken
    }
