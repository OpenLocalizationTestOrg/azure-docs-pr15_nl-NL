<properties 
    pageTitle="Voorbeeldgegevens in Azure blob storage | Microsoft Azure" 
    description="Voorbeeldgegevens in Azure-blobopslag" 
    services="machine-learning,storage" 
    documentationCenter="" 
    authors="bradsev" 
    manager="jhubbard" 
    editor="cgronlun" />

<tags 
    ms.service="machine-learning" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/19/2016" 
    ms.author="fashah;garye;bradsev" /> 

#<a name="heading"></a>Voorbeeldgegevens in Azure blob storage


In dit document behandelt steekproeven gegevens opgeslagen in Azure-blobopslag door deze via een programma te downloaden en vervolgens meting met procedures in Python geschreven.

**Waarom voorbeeld van uw gegevens?**
Als de gegevensset die u wilt analyseren groot is, is het meestal een goed idee om de gegevens om deze in een kleinere maar vertegenwoordiger en beter beheerbare grootte omlaag gepaarde steekproeven. Dit vergemakkelijkt gegevens lidmaatschap, te verkennen en functie engineering. De rol in het Cortana Analytics-proces is het inschakelen van snel prototypen van de functies voor gegevensverwerking en machine learning-modellen.

Het **menu** onder koppelingen naar onderwerpen waarin wordt uitgelegd hoe u een voorbeeld van gegevens uit verschillende opslagomgevingen. 

[AZURE.INCLUDE [cap-sample-data-selector](../../includes/cap-sample-data-selector.md)]

Deze taak steekproeven is een stap in het [Team gegevens wetenschap proces (TDSP)](https://azure.microsoft.com/documentation/learning-paths/cortana-analytics-process/).


## <a name="download-and-down-sample-data"></a>Downloaden en omlaag-voorbeeldgegevens
1. Download de gegevens van Azure-blobopslag met de blob-service van de volgende code Python: 

        from azure.storage.blob import BlobService
        import tables
        
        STORAGEACCOUNTNAME= <storage_account_name>
        STORAGEACCOUNTKEY= <storage_account_key>
        LOCALFILENAME= <local_file_name>        
        CONTAINERNAME= <container_name>
        BLOBNAME= <blob_name>

        #download from blob
        t1=time.time()
        blob_service=BlobService(account_name=STORAGEACCOUNTNAME,account_key=STORAGEACCOUNTKEY)
        blob_service.get_blob_to_path(CONTAINERNAME,BLOBNAME,LOCALFILENAME)
        t2=time.time()
        print(("It takes %s seconds to download "+blobname) % (t2 - t1))

2. Gegevens lezen in een Pandas-gegevens-frame uit het bestand dat is gedownload hierboven.

        import pandas as pd

        #directly ready from file on disk
        dataframe_blobdata = pd.read_csv(LOCALFILE)

3. Omlaag gepaarde steekproeven met behulp van de gegevens de `numpy`van `random.choice` als volgt:

        # A 1 percent sample
        sample_ratio = 0.01 
        sample_size = np.round(dataframe_blobdata.shape[0] * sample_ratio)
        sample_rows = np.random.choice(dataframe_blobdata.index.values, sample_size)
        dataframe_blobdata_sample = dataframe_blobdata.ix[sample_rows]

Nu kunt u werken met de bovenstaande gegevensframe met het voorbeeld 1 procent voor nadere uitleg en functie generatie.

##<a name="heading"></a>Gegevens uploaden en te lezen in Azure Machine Learning

De volgende code kunt u de gegevens omlaag gepaarde steekproeven en rechtstreeks in Azure ML te gebruiken:

1. Het gegevensframe naar een lokaal bestand schrijven

        dataframe.to_csv(os.path.join(os.getcwd(),LOCALFILENAME), sep='\t', encoding='utf-8', index=False)

2. Het lokale bestand uploaden naar een Azure blob met de volgende code:

        from azure.storage.blob import BlobService
        import tables

        STORAGEACCOUNTNAME= <storage_account_name>
        LOCALFILENAME= <local_file_name>
        STORAGEACCOUNTKEY= <storage_account_key>
        CONTAINERNAME= <container_name>
        BLOBNAME= <blob_name>

        output_blob_service=BlobService(account_name=STORAGEACCOUNTNAME,account_key=STORAGEACCOUNTKEY)    
        localfileprocessed = os.path.join(os.getcwd(),LOCALFILENAME) #assuming file is in current working directory
        
        try:
       
        #perform upload
        output_blob_service.put_block_blob_from_path(CONTAINERNAME,BLOBNAME,localfileprocessed)
        
        except:         
            print ("Something went wrong with uploading to the blob:"+ BLOBNAME)

3. De gegevens lezen van de Azure blob ML Azure- [Gegevens importeren](https://msdn.microsoft.com/library/azure/4e1b0fe6-aded-4b3f-a36f-39b8862b9004/) gebruiken, zoals in de onderstaande afbeelding:
 
![lezer blob](./media/machine-learning-data-science-sample-data-blob/reader_blob.png)

 
