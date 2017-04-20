<properties
    pageTitle="Uw eerste gegevens factory (REST) bouwen | Microsoft Azure"
    description="In deze zelfstudie maakt u een voorbeeld Azure gegevens Factory pijplijn gegevens Factory REST API gebruiken."
    services="data-factory"
    documentationCenter=""
    authors="spelluru"
    manager="jhubbard"
    editor="monicar"
/>

<tags
    ms.service="data-factory"
    ms.workload="data-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="hero-article"
    ms.date="08/16/2016"
    ms.author="spelluru"/>

# <a name="tutorial-build-your-first-azure-data-factory-using-data-factory-rest-api"></a>Zelfstudie: Uw eerste Azure gegevens factory met gegevens Factory REST API maken
> [AZURE.SELECTOR]
- [Overzicht en vereisten](data-factory-build-your-first-pipeline.md)
- [Azure-portal](data-factory-build-your-first-pipeline-using-editor.md)
- [Visual Studio](data-factory-build-your-first-pipeline-using-vs.md)
- [PowerShell](data-factory-build-your-first-pipeline-using-powershell.md)
- [Resourcemanager-sjabloon](data-factory-build-your-first-pipeline-using-arm.md)
- [REST API](data-factory-build-your-first-pipeline-using-rest-api.md)

In dit artikel, kunt u gegevens Factory REST API gebruiken om te maken van uw eerste Azure gegevens factory.

## <a name="prerequisites"></a>Vereisten voor
- [Zelfstudie overzicht](data-factory-build-your-first-pipeline.md) artikel en voer de **vereiste** stappen uit.
- [Omslaan](https://curl.haxx.se/dlwiz/) op uw computer installeren. U het hulpmiddel KRUL gebruiken met andere opdrachten te maken van een fabriek gegevens. 
- Volg de instructies van [dit artikel](../resource-group-create-service-principal-portal.md) naar: 
    1. Een webtoepassing met de naam **ADFGetStartedApp** in Azure Active Directory maken.
    2. Krijg **cliënt-ID** en **geheime sleutel**. 
    3. **Tenant ID**ophalen. 
    4. De toepassing **ADFGetStartedApp** aan de rol **Inzender Factory** toewijzen.  
- [Azure PowerShell](../powershell-install-configure.md)installeren.  
- **PowerShell** starten en uitvoeren van de volgende opdracht uit. Azure PowerShell geopend houden tot het einde van deze zelfstudie. Als u sluiten en opnieuw opent, moet u de opdrachten opnieuw uitvoeren.
    1. **Login-AzureRmAccount** uitvoeren en voer de gebruikersnaam en wachtwoord waarmee u zich aanmelden bij de portal van Azure.  
    2. Voer **Get-AzureRmSubscription** om weer te geven van alle abonnementen voor dit account.
    3. **Get-AzureRmSubscription - SubscriptionName NameOfAzureSubscription uitvoeren | Set-AzureRmContext** om te selecteren van het abonnement waaraan u wilt werken. **NameOfAzureSubscription** vervangen door de naam van uw Azure-abonnement. 
3. Maak een Azure resourcegroep met de naam **ADFTutorialResourceGroup** door de volgende opdracht uit te voeren in de PowerShell:  

        New-AzureRmResourceGroup -Name ADFTutorialResourceGroup  -Location "West US"

    Enkele van de stappen in deze zelfstudie wordt ervan uitgegaan dat u met de naam ADFTutorialResourceGroup resourcegroep. Als u een resourcegroep gebruikt, moet u de naam van de resourcegroep in plaats van ADFTutorialResourceGroup in deze zelfstudie gebruiken.

## <a name="create-json-definitions"></a>Definities van JSON maken
Maak volgen JSON-bestanden in de map waarin curl.exe zich bevindt. 

### <a name="datafactoryjson"></a>DataFactory.JSON 
> [AZURE.IMPORTANT] Naam moet uniek, zodat u voorvoegsel kunt/achtervoegsel ADFCopyTutorialDF zodat het een unieke naam. 

    {  
        "name": "FirstDataFactoryREST",  
        "location": "WestUS"
    }  

### <a name="azurestoragelinkedservicejson"></a>azurestoragelinkedservice.JSON
> [AZURE.IMPORTANT] **Accountnaam** en **accountkey** vervangen door de naam en van toetsen van uw account Azure opslag. Als u wilt leren hoe u uw opslagruimte toegangstoets, raadpleegt u [weergave, kopiëren en opnieuw genereren opslag toegangstoetsen](../storage/storage-create-storage-account.md#view-copy-and-regenerate-storage-access-keys).

    {
        "name": "AzureStorageLinkedService",
        "properties": {
            "type": "AzureStorage",
            "typeProperties": {
                "connectionString": "DefaultEndpointsProtocol=https;AccountName=<accountname>;AccountKey=<accountkey>"
            }
        }
    }


### <a name="hdinsightondemandlinkedservicejson"></a>hdinsightondemandlinkedservice.JSON

    {
        "name": "HDInsightOnDemandLinkedService",
        "properties": {
            "type": "HDInsightOnDemand",
            "typeProperties": {
                "version": "3.2",
                "clusterSize": 1,
                "timeToLive": "00:30:00",
                "linkedServiceName": "AzureStorageLinkedService"
            }
        }
    }

De volgende tabel vindt u beschrijvingen voor de JSON-eigenschappen in het fragment gebruikt:

| Eigenschap | Beschrijving |
| :------- | :---------- |
| Versie | Geeft aan dat de versie van de HDInsight 3,2 worden gemaakt. | 
| ClusterSize | De grootte van het cluster HDInsight. | 
| TimeToLive | Geeft aan dat het niet-actieve tijd voor het cluster HDInsight voordat deze wordt verwijderd. |
| linkedServiceName | Hiermee geeft u de opslag-account dat wordt gebruikt voor de opslag van de logboekbestanden die zijn gegenereerd met HDInsight |

Houd rekening met de volgende punten: 

- De gegevens fabriek wordt een HDInsight **op basis van Windows** cluster voor u gemaakt met de bovenstaande JSON. U kunt ook het maken van een HDInsight **Linux gebaseerde** cluster hebben. Zie [Op aanvraag HDInsight gekoppelde Service](data-factory-compute-linked-services.md#azure-hdinsight-on-demand-linked-service) voor meer informatie. 
- U kunt **uw eigen HDInsight cluster** in plaats van een cluster op aanvraag-HDInsight. Zie [Gekoppelde HDInsight-Service](data-factory-compute-linked-services.md#azure-hdinsight-linked-service) voor meer informatie.
- Het cluster HDInsight Hiermee maakt u een **standaard-container** in de blobopslag die u hebt opgegeven in de JSON (**linkedServiceName**). HDInsight worden deze container niet verwijderd wanneer het cluster wordt verwijderd. Dit probleem is inherent aan het ontwerp. Met op aanvraag HDInsight gekoppeld-service, een HDInsight cluster wordt gemaakt telkens wanneer een segment is verwerkt, tenzij er een bestaande is live cluster (**timeToLive**) en wordt verwijderd wanneer de verwerking is voltooid.

    Als u meer segmenten worden verwerkt, ziet u veel containers in uw Azure-blobopslag. Als u niet nodig ze hebt voor het oplossen van problemen met de taken, wilt u mogelijk verwijdert u deze als u wilt doen om de opslag te beperken. De namen van deze containers een patroon volgen: "adf**yourdatafactoryname**-**linkedservicename**- datetimestamp". Gebruik hulpprogramma's zoals [Microsoft opslag Explorer](http://storageexplorer.com/) containers in uw Azure-blobopslag verwijderen.

Zie [Op aanvraag HDInsight gekoppelde Service](data-factory-compute-linked-services.md#azure-hdinsight-on-demand-linked-service) voor meer informatie. 

### <a name="inputdatasetjson"></a>inputdataset.JSON

    {
        "name": "AzureBlobInput",
        "properties": {
            "type": "AzureBlob",
            "linkedServiceName": "AzureStorageLinkedService",
            "typeProperties": {
                "fileName": "input.log",
                "folderPath": "adfgetstarted/inputdata",
                "format": {
                    "type": "TextFormat",
                    "columnDelimiter": ","
                }
            },
            "availability": {
                "frequency": "Month",
                "interval": 1
            },
            "external": true,
            "policy": {}
        }
    }


De JSON Hiermee definieert u een gegevensset met de naam **AzureBlobInput**, waarmee de invoergegevens voor een activiteit in de pijplijn. Daarnaast er wordt aangegeven dat de invoergegevens bevindt zich in de blob zogenaamd **adfgetstarted** en de map met de naam van **inputdata**.

De volgende tabel vindt u beschrijvingen voor de JSON-eigenschappen in het fragment gebruikt:

| Eigenschap | Beschrijving |
| :------- | :---------- |
| type | De eigenschap type is ingesteld op AzureBlob omdat de gegevens zich bevinden in Azure-blobopslag. |  
| linkedServiceName | verwijst naar de StorageLinkedService die u eerder hebt gemaakt. |
| fileName | Deze eigenschap is optioneel. Als u deze eigenschap weglaat, worden alle bestanden uit de mappad gekozen. In dit geval wordt alleen de input.log verwerkt. |
| type | De logboekbestanden zijn in tekstindeling, zodat we tekstopmaak gebruiken. | 
| columnDelimiter | kolommen in de logboekbestanden worden gescheiden door een kommateken () |
| frequentie/interval | de frequentie is ingesteld op maand en het interval is 1, wat betekent dat de invoer segmenten maandelijks beschikbaar zijn. | 
| externe | Deze eigenschap is ingesteld op waar als de invoergegevens niet wordt gegenereerd door de gegevens Factory-service. | 

### <a name="outputdatasetjson"></a>outputdataset.JSON

    {
        "name": "AzureBlobOutput",
        "properties": {
            "type": "AzureBlob",
            "linkedServiceName": "AzureStorageLinkedService",
            "typeProperties": {
                "folderPath": "adfgetstarted/partitioneddata",
                "format": {
                    "type": "TextFormat",
                    "columnDelimiter": ","
                }
            },
            "availability": {
                "frequency": "Month",
                "interval": 1
            }
        }
    }

De JSON Hiermee definieert u een gegevensset met de naam **AzureBlobOutput**, waarmee de uitvoergegevens voor een activiteit in de pijplijn. Daarnaast geeft aan dat de resultaten worden opgeslagen in de blob zogenaamd **adfgetstarted** en de map met de naam van **partitioneddata**. De sectie **beschikbaarheid** geeft aan dat de gegevensset uitvoer is geproduceerd op maandbasis.

### <a name="pipelinejson"></a>Pipeline.JSON
> [AZURE.IMPORTANT] Vervang **storageaccountname** door de naam van uw account Azure opslag. 


    {
        "name": "MyFirstPipeline",
        "properties": {
            "description": "My first Azure Data Factory pipeline",
            "activities": [{
                "type": "HDInsightHive",
                "typeProperties": {
                    "scriptPath": "adfgetstarted/script/partitionweblogs.hql",
                    "scriptLinkedService": "AzureStorageLinkedService",
                    "defines": {
                        "inputtable": "wasb://adfgetstarted@<stroageaccountname>.blob.core.windows.net/inputdata",
                        "partitionedtable": "wasb://adfgetstarted@<stroageaccountname>t.blob.core.windows.net/partitioneddata"
                    }
                },
                "inputs": [{
                    "name": "AzureBlobInput"
                }],
                "outputs": [{
                    "name": "AzureBlobOutput"
                }],
                "policy": {
                    "concurrency": 1,
                    "retry": 3
                },
                "scheduler": {
                    "frequency": "Month",
                    "interval": 1
                },
                "name": "RunSampleHiveActivity",
                "linkedServiceName": "HDInsightOnDemandLinkedService"
            }],
            "start": "2016-07-10T00:00:00Z",
            "end": "2016-07-11T00:00:00Z",
            "isPaused": false
        }
    }

In het fragment JSON maakt u een pijplijn die bestaat uit één activiteit die component om gegevens te verwerken op een cluster HDInsight gebruikt.

De component-scriptbestand, **partitionweblogs.hql**, is opgeslagen in de opslag van Azure-account (opgegeven door de scriptLinkedService, **StorageLinkedService**genoemd), en in de map **script** in de container **adfgetstarted**.

Hiermee geeft u de sectie **definieert** runtime-instellingen die worden doorgegeven aan het script component als component configuratiewaarden (bijvoorbeeld ${hiveconf: inputtable}, {hiveconf:partitionedtable} $).

De eigenschappen van het **begin** en **einde** van de pijplijn Hiermee geeft u de actieve periode van de pijplijn.

In de activiteit JSON, kunt u opgeven dat het script component wordt uitgevoerd op de opgegeven door de **linkedServiceName** – **HDInsightOnDemandLinkedService**berekeningscluster.

> [AZURE.NOTE] Zie [anatomie van een pijplijn](data-factory-create-pipelines.md#anatomy-of-a-pipeline) voor meer informatie over de JSON-eigenschappen in het voorgaande voorbeeld gebruikt. 

## <a name="set-global-variables"></a>Globale variabelen instellen

Uitvoeren in Azure PowerShell, de volgende opdrachten na de waarden vervangen door uw eigen:

> [AZURE.IMPORTANT] Zie [vereisten voor](#prerequisites) de sectie voor instructies over het gebruik van de client-ID, geheim client, tenant-ID en abonnement-ID.   

    $client_id = "<client ID of application in AAD>"
    $client_secret = "<client key of application in AAD>"
    $tenant = "<Azure tenant ID>";
    $subscription_id="<Azure subscription ID>";

    $rg = "ADFTutorialResourceGroup"
    $adf = "FirstDataFactoryREST"



## <a name="authenticate-with-aad"></a>Verificatie met AAD

    $cmd = { .\curl.exe -X POST https://login.microsoftonline.com/$tenant/oauth2/token  -F grant_type=client_credentials  -F resource=https://management.core.windows.net/ -F client_id=$client_id -F client_secret=$client_secret };
    $responseToken = Invoke-Command -scriptblock $cmd;
    $accessToken = (ConvertFrom-Json $responseToken).access_token;
    
    (ConvertFrom-Json $responseToken) 



## <a name="create-data-factory"></a>Gegevens factory maken

In deze stap maakt u een Azure-gegevens Factory met de naam **FirstDataFactoryREST**. Een factory gegevens kan een of meer pijpleidingen bevatten. Een pijplijn kan een of meer activiteiten in deze hebben. Bijvoorbeeld een activiteit kopie gegevens uit een bron kopiëren naar een gegevensopslag bestemming en de activiteit in een HDInsight component naar component script uitvoeren om gegevens te transformeren. Voer de volgende opdrachten voor het maken van de gegevens fabriek: 

1. De opdracht toewijzen aan variabele met de naam **cmd**. 

    Bevestig dat de naam van de gegevens fabriek u hier opgeeft (ADFCopyTutorialDF) overeenkomt met de naam die is opgegeven in de **datafactory.json**. 

        $cmd = {.\curl.exe -X PUT -H "Authorization: Bearer $accessToken" -H "Content-Type: application/json" --data “@datafactory.json” https://management.azure.com/subscriptions/$subscription_id/resourcegroups/$rg/providers/Microsoft.DataFactory/datafactories/FirstDataFactoryREST?api-version=2015-10-01};
2. De opdracht met **Roep-opdracht**uitvoeren.

        $results = Invoke-Command -scriptblock $cmd;
3. De resultaten bekijken. Als de gegevens fabriek is gemaakt, ziet u de JSON voor de fabriek gegevens in de **resultaten**; anders wordt een foutbericht weergegeven.  

        Write-Host $results

Houd rekening met de volgende punten:
 
- De naam van de Azure gegevens fabriek moet uniek zijn. Als u de fout in de resultaten ziet: **gegevens factory naam "FirstDataFactoryREST" is niet beschikbaar**, voer de volgende stappen uit:  
    1. Wijzig de naam (bijvoorbeeld yournameFirstDataFactoryREST) in het bestand **datafactory.json** . Zie [Gegevens Factory - regels voor naamgeving van](data-factory-naming-rules.md) onderwerp voor naming regels voor Data Factory-onderdelen.
    2. Klik in de eerste opdracht waar de variabele **$cmd** een waarde is toegewezen, FirstDataFactoryREST vervangen door de nieuwe naam en de opdracht uitvoeren. 
    3. Voer de volgende twee opdrachten aan te roepen de REST API voor het maken van de gegevens fabriek en de resultaten van de bewerking afdrukken. 
- Als u wilt gegevens Factory-exemplaren maken, moet u een inzender/beheerder van de Azure-abonnement
- De naam van de gegevens fabriek kan worden geregistreerd als een DNS-naam in de toekomst en dus worden openbaar zichtbaar.
- Als het foutbericht: '**dit abonnement niet is geregistreerd als u wilt gebruiken naamruimte Microsoft.DataFactory**', een van de volgende handelingen uit en probeer opnieuw te publiceren: 

    - Voer de volgende opdracht voor het registreren van de gegevens Factory-provider in Azure PowerShell: 
        
            Register-AzureRmResourceProvider -ProviderNamespace Microsoft.DataFactory
    
        U kunt de volgende opdracht uit om te bevestigen dat de gegevens fabriek provider is geregistreerd uitvoeren: 
    
            Get-AzureRmResourceProvider
    - Meld u aan met het Azure abonnement in de [portal van Azure](https://portal.azure.com) en navigeer naar een gegevens Factory-blade (of) maken van een fabriek gegevens in de portal van Azure. Deze actie worden automatisch de provider voor u geregistreerd.

Voordat u een pijplijn maakt, moet u eerst een paar gegevens Factory entiteiten maken. U gekoppelde services om gegevens winkels/berekent de koppeling naar uw gegevensopslag, invoer definiëren en uitvoer gegevenssets om gegevens in gekoppelde gegevens winkels voor te stellen voor het eerst maken. 

## <a name="create-linked-services"></a>Gekoppelde services maken 
In deze stap geeft u een uw opslagruimte van Azure-account en een op aanvraag Azure HDInsight cluster koppeling naar uw gegevens factory. De opslag van Azure-account bevat de invoer- en uitvoerbereik gegevens voor de pijplijn in dit voorbeeld. De HDInsight gekoppeld-service wordt gebruikt om uit te voeren component script die zijn opgegeven in de activiteit van de pijplijn in dit voorbeeld. 

### <a name="create-azure-storage-linked-service"></a>Azure gekoppeld opslagservice maken
In deze stap geeft koppelen u uw opslagruimte van Azure-account aan uw factory gegevens. Met deze zelfstudie kunt u hetzelfde opslag van Azure-account gebruiken om op te slaan invoer/uitvoergegevens en de HQL script-bestand.

1. De opdracht toewijzen aan variabele met de naam **cmd**. 

        $cmd = {.\curl.exe -X PUT -H "Authorization: Bearer $accessToken" -H "Content-Type: application/json" --data “@azurestoragelinkedservice.json” https://management.azure.com/subscriptions/$subscription_id/resourcegroups/$rg/providers/Microsoft.DataFactory/datafactories/$adf/linkedservices/AzureStorageLinkedService?api-version=2015-10-01};
2. De opdracht met **Roep-opdracht**uitvoeren.
 
        $results = Invoke-Command -scriptblock $cmd;
3. De resultaten bekijken. Als de gekoppelde service is gemaakt, ziet u de JSON voor de gekoppelde service in de **resultaten**; anders wordt een foutbericht weergegeven.
  
        Write-Host $results

### <a name="create-azure-hdinsight-linked-service"></a>Azure HDInsight gekoppeld service maken
In deze stap kunt u een op aanvraag HDInsight cluster koppeling naar uw gegevens factory. Het cluster HDInsight wordt automatisch gemaakt tijdens runtime en is verwijderd nadat deze verwerking en niet-actieve voor de opgegeven tijdsduur is voltooid. U kunt uw eigen HDInsight cluster in plaats van een cluster op aanvraag-HDInsight. Zie [Gekoppelde Services berekenen](data-factory-compute-linked-services.md) voor meer informatie.  

1. De opdracht toewijzen aan variabele met de naam **cmd**.
 
        $cmd = {.\curl.exe -X PUT -H "Authorization: Bearer $accessToken" -H "Content-Type: application/json" --data "@hdinsightondemandlinkedservice.json" https://management.azure.com/subscriptions/$subscription_id/resourcegroups/$rg/providers/Microsoft.DataFactory/datafactories/$adf/linkedservices/hdinsightondemandlinkedservice?api-version=2015-10-01};
2. De opdracht met **Roep-opdracht**uitvoeren.

        $results = Invoke-Command -scriptblock $cmd;
3. De resultaten bekijken. Als de gekoppelde service is gemaakt, ziet u de JSON voor de gekoppelde service in de **resultaten**; anders wordt een foutbericht weergegeven.  

        Write-Host $results

## <a name="create-datasets"></a>Gegevenssets maken
In deze stap maakt u gegevenssets om te staan voor de invoer en uitvoer van gegevens voor de verwerking van de component. Deze gegevenssets raadpleegt u de **StorageLinkedService** die u eerder in deze zelfstudie hebt gemaakt. De gekoppelde service wordt verwezen in een opslag van Azure-account en gegevenssets container, map, bestandsnaam opgeven in de opslagruimte waarin invoer en gegevens.   

### <a name="create-input-dataset"></a>Invoer gegevensset maken
In deze stap geeft maken u de invoer gegevensset invoergegevens die zijn opgeslagen in de Azure-blobopslag bevatten te.

1. De opdracht toewijzen aan variabele met de naam **cmd**. 

        $cmd = {.\curl.exe -X PUT -H "Authorization: Bearer $accessToken" -H "Content-Type: application/json" --data "@inputdataset.json" https://management.azure.com/subscriptions/$subscription_id/resourcegroups/$rg/providers/Microsoft.DataFactory/datafactories/$adf/datasets/AzureBlobInput?api-version=2015-10-01};
2. De opdracht met **Roep-opdracht**uitvoeren.

        $results = Invoke-Command -scriptblock $cmd;
3. De resultaten bekijken. Als de gegevensset is gemaakt, ziet u de JSON voor de gegevensset in de **resultaten**; anders wordt een foutbericht weergegeven.
  
        Write-Host $results
### <a name="create-output-dataset"></a>Uitvoer gegevensset maken
In deze stap geeft maken u de uitvoer gegevensset bevatten die zijn opgeslagen in de Azure-blobopslag uitvoergegevens te.

1. De opdracht toewijzen aan variabele met de naam **cmd**.
 
        $cmd = {.\curl.exe -X PUT -H "Authorization: Bearer $accessToken" -H "Content-Type: application/json" --data "@outputdataset.json" https://management.azure.com/subscriptions/$subscription_id/resourcegroups/$rg/providers/Microsoft.DataFactory/datafactories/$adf/datasets/AzureBlobOutput?api-version=2015-10-01};
2. De opdracht met **Roep-opdracht**uitvoeren.

        $results = Invoke-Command -scriptblock $cmd;
3. De resultaten bekijken. Als de gegevensset is gemaakt, ziet u de JSON voor de gegevensset in de **resultaten**; anders wordt een foutbericht weergegeven.
  
        Write-Host $results 

## <a name="create-pipeline"></a>Verkooppijplijn maken
In deze stap kunt u uw eerste verkooppijplijn maken met een activiteit **HDInsightHive** . Invoer segment maandelijks beschikbaar is (frequentie: maand, interval: 1), uitvoer segment maandelijks is geproduceerd en de eigenschap scheduler voor de activiteit wordt ook ingesteld op maandelijks. De instellingen voor de gegevensset uitvoer en de activiteit planner moeten overeenkomen met. Uitvoer gegevensset is momenteel wat stations het schema, zodat u kunt een gegevensset uitvoer maken moet, zelfs als de activiteit geen uitvoer levert. Als de activiteit niet wordt elke invoer duren, kunt u doorgaan met het maken van de invoer gegevensset.  

Bevestig dat u het bestand **input.log** in de map **adfgetstarted/inputdata** in de Azure-blobopslag ziet en voer de volgende opdracht om te implementeren van de pijplijn. Aangezien de ** **begin** - en** eindtijden worden ingesteld in het verleden en **isPaused** is ingesteld op ONWAAR, wordt de pijplijn (activiteit in de pijplijn) uitgevoerd direct nadat u implementeren. 

1. De opdracht toewijzen aan variabele met de naam **cmd**.
 
        $cmd = {.\curl.exe -X PUT -H "Authorization: Bearer $accessToken" -H "Content-Type: application/json" --data "@pipeline.json" https://management.azure.com/subscriptions/$subscription_id/resourcegroups/$rg/providers/Microsoft.DataFactory/datafactories/$adf/datapipelines/MyFirstPipeline?api-version=2015-10-01};
2. De opdracht met **Roep-opdracht**uitvoeren.

        $results = Invoke-Command -scriptblock $cmd;
3. De resultaten bekijken. Als de gegevensset is gemaakt, ziet u de JSON voor de gegevensset in de **resultaten**; anders wordt een foutbericht weergegeven.  

        Write-Host $results
5. Gefeliciteerd, u uw eerste verkooppijplijn via Azure PowerShell hebt gemaakt.

## <a name="monitor-pipeline"></a>Monitor verkooppijplijn
In deze stap kunt u gegevens Factory REST API gebruiken om de segmenten worden geproduceerd door de pijplijn te houden.

    $ds ="AzureBlobOutput"

    $cmd = {.\curl.exe -X GET -H "Authorization: Bearer $accessToken" https://management.azure.com/subscriptions/$subscription_id/resourcegroups/$rg/providers/Microsoft.DataFactory/datafactories/$adf/datasets/$ds/slices?start=1970-01-01T00%3a00%3a00.0000000Z"&"end=2016-08-12T00%3a00%3a00.0000000Z"&"api-version=2015-10-01};

    $results2 = Invoke-Command -scriptblock $cmd;

    IF ((ConvertFrom-Json $results2).value -ne $NULL) {
        ConvertFrom-Json $results2 | Select-Object -Expand value | Format-Table
    } else {
            (convertFrom-Json $results2).RemoteException
    }


> [AZURE.IMPORTANT] 
> Maken van een op aanvraag HDInsight cluster duurt meestal ergens (ongeveer 20 minuten). Daarom verwachten de pijplijn te nemen **ongeveer 30 minuten** verwerkingstijd van het segment.  

Voer de Roep-opdracht en de volgende totdat u het segment in **klaar** staat of **mislukt** staat ziet. Wanneer het segment in gereed is, controleert u of de map **partitioneddata** in de container **adfgetstarted** in uw blobopslag voor de uitvoergegevens.  Het maken van een op aanvraag HDInsight cluster wordt gewoonlijk duurt enige tijd.

![uitvoergegevens](./media/data-factory-build-your-first-pipeline-using-rest-api/three-ouptut-files.png)

> [AZURE.IMPORTANT] De invoer bestand wordt verwijderd wanneer het segment wordt verwerkt. Daarom desgewenst kunt u het segment opnieuw uit te voeren of de zelfstudie opnieuw doen het invoer bestand uploaden (input.log) naar de map inputdata van de container adfgetstarted.

U kunt ook Azure-portal gebruiken om te controleren van segmenten en eventuele problemen oplossen. Zie [controleren met behulp van Azure portal pijpleidingen](data-factory-build-your-first-pipeline-using-editor.md##monitor-pipeline) details.  

## <a name="summary"></a>Overzicht 
In deze zelfstudie kunt u een factory Azure gegevens om gegevens te verwerken met een component script op een HDInsight hadoop cluster gemaakt. U gebruikt de gegevens Factory-Editor in de portal van Azure moet de volgende stappen uit:  

1.  Een Azure **gegevens factory**gemaakt.
2.  Twee **gekoppelde services**gemaakt:
    1.  **Azure Storage** gekoppeld service uw Azure-blobopslag die met de bestanden van de invoer/uitvoer fabriek gegevens koppelen.
    2.  **Azure HDInsight** op aanvraag gekoppelde service een op aanvraag HDInsight Hadoop cluster koppelen aan de fabriek gegevens. Azure gegevens Factory Hiermee maakt u een HDInsight Hadoop cluster just-in-time verwerken van invoergegevens en het verkrijgen van uitvoergegevens. 
3.  Gemaakt twee **gegevenssets**, welke invoer- en uitvoerbereik gegevens voor HDInsight component activiteit in de pijplijn beschrijven. 
4.  Een **verkooppijplijn** gemaakt met de activiteit in een **HDInsight-component** . 

## <a name="next-steps"></a>Volgende stappen
In dit artikel, kunt u een pijplijn hebt gemaakt met een transformatie activiteit (HDInsight) die een script component op een op aanvraag Azure HDInsight cluster wordt uitgevoerd. Zie informatie over het gebruik van de activiteit in een kopie gegevens kopiëren van een Azure Blob naar SQL Azure, [Zelfstudie: gegevens uit een Azure Blob naar SQL Azure kopiëren](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md).

## <a name="see-also"></a>Zie ook
| Onderwerp | Beschrijving |
| :---- | :---- |
| [Naslag voor Data Factory REST API](https://msdn.microsoft.com/library/azure/dn906738.aspx) |  Zie uitgebreide documentatie op Data Factory-cmdlets |
| [Gegevens transformatie activiteiten](data-factory-data-transformation-activities.md) | In dit artikel vindt u een lijst met gegevens transformatie activiteiten (zoals HDInsight component transformatie u in deze zelfstudie gebruikt) worden ondersteund door Factory van Azure-gegevens. |
| [Plannings- en kan worden uitgevoerd](data-factory-scheduling-and-execution.md) | In dit artikel wordt uitgelegd dat de plannings- en execution aspecten van Azure gegevens Factory-toepassingsmodel. |
| [Pijpleidingen](data-factory-create-pipelines.md) | Dit artikel vindt u meer informatie over pijpleidingen en activiteiten in Factory van Azure-gegevens en hoe ze worden gebruikt om samen te stellen end-to-end gegevensgestuurde werkstromen voor uw scenario of bedrijf. |
| [Gegevenssets](data-factory-create-datasets.md) | Dit artikel vindt u meer informatie over gegevenssets in fabriek van Azure-gegevens.
| [Monitor en beheren pijpleidingen met Azure portal bladen](data-factory-monitor-manage-pipelines.md) | In dit artikel wordt beschreven hoe bewaken, beheren en fouten opsporen in uw pijpleidingen met Azure portal bladen. |
| [Bewaken en pijpleidingen met Monitoring App beheren](data-factory-monitor-manage-app.md) | In dit artikel wordt beschreven hoe bewaken, beheren en fouten opsporen in pijpleidingen met behulp van de controle Management-App. 

