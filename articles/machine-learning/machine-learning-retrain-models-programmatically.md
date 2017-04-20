<properties
    pageTitle="Machine Learning-modellen via programmacode Retrain | Microsoft Azure"
    description="Leer hoe u via een programma een model retrain en de webservice als u wilt gebruiken het zojuist ervaren model in Azure Machine Learning bijwerken."
    services="machine-learning"
    documentationCenter=""
    authors="raymondlaghaeian"
    manager="jhubbard"
    editor="cgronlun"/>

<tags
    ms.service="machine-learning"
    ms.workload="data-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/10/2016"
    ms.author="raymondl;garye;v-donglo"/>


# <a name="retrain-machine-learning-models-programmatically"></a>Retrain Machine Learning-modellen via programmacode  

In dit scenario leert u hoe u via programmacode retrain een Azure Machine Learning webservice met C# en de Machine Learning Batch Execution-service.

Zodra u hebt het model retrained, wordt in de volgende scenario het bijwerken van het model in uw blog webservice weergeven:

- Als u een webservice klassieke in de portal Machine Learning-webservices geïmplementeerd, raadpleegt u [een webservice klassieke Retrain](machine-learning-retrain-a-classic-web-service.md). 
- Als u een nieuwe webservice geïmplementeerd, raadpleegt u [een nieuwe webservice gebruik van de cmdlets Machine Learning Management Retrain](machine-learning-retrain-new-web-service-using-powershell.md).

Zie [een Machine Learning-Model Retrain](machine-learning-retrain-machine-learning-model.md)voor een overzicht van het opnieuw opleiden proces.

Als u beginnen met uw bestaande webservice van nieuwe Azure resourcemanager die zijn gebaseerd wilt, raadpleegt u [een bestaande blog webservice Retrain](machine-learning-retrain-existing-resource-manager-based-web-service.md).

## <a name="create-a-training-experiment"></a>Maken van een experiment training
 
In dit voorbeeld gewenste "voorbeeld 5: trein, Test, evalueren voor binaire indeling: volwassen gegevensset" van de Microsoft Azure Machine Learning-voorbeelden. 
    
Het experiment maken:

1.  Meld u aan bij naar Microsoft Azure Machine Learning Studio. 
2.  In de rechterbenedenhoek van het dashboard, klikt u op **Nieuw**.
3.  Selecteer in de Microsoft-Samples Sample 5.
4.  De naam van het experiment, boven aan het tekenpapier experiment, selecteert u de naam experiment "voorbeeld 5: trein, Test, evalueren voor binaire indeling: volwassen gegevensset".
5.  Type telling Model.
6.  Aan de onderkant van het experiment canvas, klikt u op **uitvoeren**.
7.  Klik op **instellen webservice** en selecteer **Retraining-webservice**. 

    ![Eerste experiment.][2]

Diagram 2: De eerste experiment.

## <a name="create-a-predictive-experiment-and-publish-as-a-web-service"></a>Een blog experiment maken en publiceren als een webservice  

U maakt vervolgens een Experiment Predicative.

1.  Aan de onderkant van het papier experiment, klik op **Webservice instellen** en selecteer **Blog webservice**. Hiermee slaat het model als een ervaren Model en wordt toegevoegd web service en uitvoer modules. 
2.  Klik op **uitvoeren**. 
3.  Nadat het experiment is voltooid, klikt u op **Webservice implementeren [klassieke]** of **Implementeren webservice [Nieuw]**.

## <a name="deploy-the-training-experiment-as-a-training-web-service"></a>Het experiment training te implementeren als een webservice Training

Als u wilt het ervaren model retrain, moet u het training experiment die u hebt gemaakt als een webservice Retraining implementeren. Deze webservice moet een module *Web Service uitvoer* is verbonden met de * [Trein Model] [ train-model] * module, moeten kunnen maken van nieuwe ervaren modellen.

1. Als u wilt teruggaan naar het experiment training, klik u op het pictogram experimenten in het linkerdeelvenster, op het experiment met de naam telling Model.  
2. Typ in het vak Zoeken Experiment zoekitems webservice. 
3. Sleep een module *Web Service invoer* naar het tekenpapier experiment en verbindt u de uitvoer naar de module *Wissen.Control ontbrekende gegevens* .  Dit zorgt ervoor dat uw gegevens opnieuw opleiden hetzelfde als de trainingsgegevens van uw oorspronkelijke worden verwerkt.
4. Sleep twee *webservice uitvoer* modules naar het tekenpapier experiment. De uitvoer van de module *Trein Model* verbinden met de ene en de uitvoer van de module *Evalueren Model* naar de andere. De uitvoer van de web-service voor **Trein Model** ontstaat het nieuwe ervaren model. De uitvoer die zijn bijgevoegd bij **Model evalueren** geeft als resultaat van de module uitvoer, dat wil de prestatieresultaten zeggen.
5. Klik op **uitvoeren**. 

Vervolgens moet u het experiment training implementeren als een webservice die een ervaren model en model evaluatieresultaten oplevert. Hiervoor zijn de volgende set acties afhankelijk van het of u werkt met een webservice klassieke of een nieuwe webservice.  
  
**Klassieke webservice**

Aan de onderkant van het papier experiment, klik op **Webservice instellen** en selecteer **Webservice implementeren [klassieke]**. De webservice **Dashboard** wordt weergegeven met de API-sleutel en de help-pagina API voor Batch Execution. Alleen de Batch Execution methode kan worden gebruikt voor het maken van ervaren-modellen.

**Nieuwe webservice**

Aan de onderkant van het papier experiment, klik op **Webservice instellen** en selecteer **Implementeren webservice [Nieuw]**. De portal Web Service Azure Machine Learning-webservices wordt geopend aan de pagina service Deploy. Typ een naam voor uw webservice en kies een betalingsmethode-abonnement en klik op **Deploy**. Alleen de Batch Execution methode kan voor het maken van ervaren modellen worden gebruikt

In beide gevallen nadat experiment actief is, is voltooid ziet de resulterende werkstroom er als volgt:

![Resulterende werkstroom nadat uitvoeren.][4]

Diagram 3: Met een werkstroom nadat uitgevoerd als resultaat.

## <a name="retrain-the-model-with-new-data-using-bes"></a>Het model met nieuwe gegevens met BES Retrain

In dit voorbeeld zijn C# wilt u de toepassing opnieuw opleiden maken. U kunt ook de code van de steekproef Python of R gebruiken voor het uitvoeren van deze taak.

De bijscholing-API's bellen:

1. Maak een C#-Console-toepassing in Visual Studio (nieuwe Project -> Windows -> bureaublad consoletoepassing >).
2.  Meld u aan bij de portal Machine Learning-webservice.
3.  Als u met een webservice klassieke werkt, klikt u op **Klassieke webservices**.
    1.  Klik op de webservice waarmee die u werkt.
    2.  Klik op de standaardeindpunt.
    3.  Klik op **gebruiken**.
    4.  Klik onder aan de pagina **verbruiken** in de sectie **Voorbeeldcode** op **Batch**.
    5.  Ga verder met stap 5 van deze procedure.
4.  Als u met een nieuwe webservice werkt, klikt u op **Webservices**.
    1.  Klik op de webservice waarmee die u werkt.
    2.  Klik op **gebruiken**.
    3.  Klik onder aan de pagina verbruiken in de sectie **Voorbeeldcode** op **Batch**.
5.  De steekproef C#-code voor Batchuitvoering Kopieer en plak deze in het bestand Program.cs ervoor te zorgen dat de naamruimte blijft behouden.

Het pakket Nuget Microsoft.AspNet.WebApi.Client zoals opgegeven in de opmerkingen toevoegen. De verwijzing naar Microsoft.WindowsAzure.Storage.dll wilt toevoegen, moet u mogelijk eerst de clientbibliotheek voor services van Microsoft Azure-opslag installeren. Zie [Windows opslagservices](https://www.nuget.org/packages/WindowsAzure.Storage)voor meer informatie.

### <a name="update-the-apikey-declaration"></a>De declaratie apikey bijwerken

Zoek de declaratie **apikey** .

    const string apiKey = "abc123"; // Replace this with the API key for the web service

Zoek de primaire sleutel in de sectie **eenvoudige verbruik info** van de pagina **verbruiken** en kopiëren naar de declaratie **apikey** .

### <a name="update-the-azure-storage-information"></a>De opslag van Azure-gegevens bijwerken

De code van de steekproef BES uploads van een bestand vanaf een lokaal station (bijvoorbeeld 'C:\temp\CensusIpnput.csv') met Azure Storage, verwerkt en de resultaten gegevens terug worden geschreven met Azure Storage.  

Als u wilt uitvoeren van deze taak, moet u de opslagruimte naam, sleutel en container accountgegevens ophalen voor uw account opslag in de klassieke Azure-portal en de bijbehorende waarden bijwerken in de code. 

1. Meld u aan bij de klassieke Azure-portal.
1. Klik in de kolom linker navigatiegedeelte op **opslag**.
1. Selecteer in de lijst met accounts van opslag, een voor de opslag van het retrained model.
1. Klik onder aan de pagina, op **Toegangstoetsen beheren**.
1. Kopiëren en de **Primaire sleutel van Access** opslaan en sluit het dialoogvenster. 
1. Aan de bovenkant van de pagina, klikt u op **Containers**.
1. Selecteer een bestaande container of een nieuw account te maken en sla de naam.

Zoek de declaraties *StorageAccountName*, *StorageAccountKey*en *StorageContainerName* en de waarden die u hebt opgeslagen vanaf de portal van Azure bijwerken.

    const string StorageAccountName = "mystorageacct"; // Replace this with your Azure Storage Account name
    const string StorageAccountKey = "a_storage_account_key"; // Replace this with your Azure Storage Key
    const string StorageContainerName = "mycontainer"; // Replace this with your Azure Storage Container name
            
Ook moet u ervoor zorgen dat bestand is beschikbaar op de locatie die u in de code opgeeft. 

### <a name="specify-the-output-location"></a>De uitvoerlocatie van de opgeven

Wanneer u de uitvoerlocatie opgeeft in de nettolading aanvragen, moet de extensie van het bestand dat is opgegeven in *RelativeLocation* als ilearner worden opgegeven. 

Zie het volgende voorbeeld:

    Outputs = new Dictionary<string, AzureBlobDataReference>() {
        {
            "output1",
            new AzureBlobDataReference()
            {
                ConnectionString = storageConnectionString,
                RelativeLocation = string.Format("{0}/output1results.ilearner", StorageContainerName) /*Replace this with the location you would like to use for your output file, and valid file extension (usually .csv for scoring results, or .ilearner for trained models)*/
            }
        },

>[AZURE.NOTE] Het is mogelijk dat de namen van uw uitvoerlocaties afwijken van de instellingen in dit scenario op basis van de volgorde waarin u de web-service uitvoer modules hebt toegevoegd. Aangezien u dit training experiment met twee uitvoer hebt ingesteld, zijn de resultaten opslag locatiegegevens voor beide.  

![Uitvoer bijscholing][6]

Diagram 4: Bijscholing uitvoer.

## <a name="evaluate-the-retraining-results"></a>De opnieuw opleiden resultaten evalueren
 
Wanneer u de toepassing uitvoert, bevat de uitvoer de URL en SA's token nodig voor toegang tot de evaluatieresultaten.

U kunt de prestatieresultaten van het retrained model weergeven door het combineren van de *BaseLocation*, *RelativeLocation*en *SasBlobToken* uit het resultaat voor *output2* (zoals weergegeven in de voorgaande opnieuw opleiden uitvoer afbeelding) en de volledige URL te plakken in de adresbalk van de browser.  

Bekijk de resultaten om te bepalen of het zojuist ervaren model goed genoeg voor het vervangen van het bestaande bestand uitvoert.

Kopieer de *BaseLocation*, *RelativeLocation*en *SasBlobToken* uit het resultaat, wordt u deze gebruiken tijdens het opnieuw opleiden.

## <a name="next-steps"></a>Volgende stappen

Als u de blog webservice geïmplementeerd door te klikken op **Webservice implementeren [klassieke]**, raadpleegt u [een webservice klassieke Retrain](machine-learning-retrain-a-classic-web-service.md).

Als u de blog webservice geïmplementeerd door te klikken op **Implementeren webservice [Nieuw]**, raadpleegt u [een nieuwe webservice gebruik van de cmdlets Machine Learning Management Retrain](machine-learning-retrain-new-web-service-using-powershell.md).

<!-- Retrain a New web service using the Machine Learning Management REST API -->


[1]: ./media/machine-learning-retrain-models-programmatically/machine-learning-retrain-models-programmatically-IMAGE01.png
[2]: ./media/machine-learning-retrain-models-programmatically/machine-learning-retrain-models-programmatically-IMAGE02.png
[3]: ./media/machine-learning-retrain-models-programmatically/machine-learning-retrain-models-programmatically-IMAGE03.png
[4]: ./media/machine-learning-retrain-models-programmatically/machine-learning-retrain-models-programmatically-IMAGE04.png
[5]: ./media/machine-learning-retrain-models-programmatically/machine-learning-retrain-models-programmatically-IMAGE05.png
[6]: ./media/machine-learning-retrain-models-programmatically/machine-learning-retrain-models-programmatically-IMAGE06.png
[7]: ./media/machine-learning-retrain-models-programmatically/machine-learning-retrain-models-programmatically-IMAGE07.png


<!-- Module References -->
[train-model]: https://msdn.microsoft.com/library/azure/5cc7053e-aa30-450d-96c0-dae4be720977/