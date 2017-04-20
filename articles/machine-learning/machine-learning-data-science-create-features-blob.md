<properties
    pageTitle="Maken van functies voor Azure blob storage gegevens met behulp van Panda | Microsoft Azure"
    description="Het maken van functies voor gegevens die zijn opgeslagen in de container Azure blob met het pakket Panda Python."
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
    ms.author="bradsev;garye" />

#<a name="create-features-for-azure-blob-storage-data-using-panda"></a>Functies voor Azure blob storage gegevens met behulp van Panda maken

In dit document ziet hoe u functies voor gegevens die zijn opgeslagen in Azure blob container met het [Pandas](http://pandas.pydata.org/) Python-pakket maken. Na het laden van de gegevens in een frame van de gegevens Panda overzichten, wordt deze bestaan functies Python scripts met indicatorwaarden en functies binning genereren.

[AZURE.INCLUDE [cap-create-features-data-selector](../../includes/cap-create-features-selector.md)]In dit **menu** koppelingen naar onderwerpen waarin wordt uitgelegd hoe u functies voor gegevens maken in verschillende omgevingen. Deze taak is een stap in het [Team gegevens wetenschap proces (TDSP)](https://azure.microsoft.com/documentation/learning-paths/cortana-analytics-process/).


## <a name="prerequisites"></a>Vereisten voor

In dit artikel wordt ervan uitgegaan dat u een Azure blob storage-account hebt gemaakt en uw gegevens hebt opgeslagen. Als u de instructies voor het instellen van een account nodig hebt, raadpleegt u [een opslag van Azure-account maken](../storage/storage-create-storage-account.md#create-a-storage-account)


## <a name="load-the-data-into-a-pandas-data-frame"></a>De gegevens laadt in een frame Pandas-gegevens
Om te verkennen en een gegevensset bewerken, moet deze uit de bron blob worden gedownload naar een lokaal bestand die kan vervolgens in een frame Pandas-gegevens worden geladen. Hier volgen de stappen te volgen voor deze procedure:

1. Download de gegevens van Azure blob met de volgende Python code met blob-service. De variabele in de volgende code vervangen door uw specifieke waarden:

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


2. Lees de gegevens in een Pandas-gegevens-frame uit het gedownloade bestand.

        #LOCALFILE is the file path
        dataframe_blobdata = pd.read_csv(LOCALFILE)

U bent nu klaar om te verkennen van de gegevens en functies op deze dataset genereren.

##<a name="blob-featuregen"></a>Functie generatie

De volgende twee secties hoe genereren bestaan functies met indicatorwaarden en binning functies Python-scripts gebruiken.

###<a name="blob-countfeature"></a>Indicatorwaarde op basis van functie generatie

Bestaan functies kunnen als volgt worden gemaakt:

1. De verdeling van de kolom bestaan controleren:

        dataframe_blobdata['<categorical_column>'].value_counts()

2. Indicator voor waarden voor elk van de kolomwaarden genereren

        #generate the indicator column
        dataframe_blobdata_identity = pd.get_dummies(dataframe_blobdata['<categorical_column>'], prefix='<categorical_column>_identity')

3. Deelnemen aan de indicatorkolom met de oorspronkelijke gegevensframe

            #Join the dummy variables back to the original data frame
            dataframe_blobdata_with_identity = dataframe_blobdata.join(dataframe_blobdata_identity)

4. De oorspronkelijke variabele zelf verwijderen:

        #Remove the original column rate_code in df1_with_dummy
        dataframe_blobdata_with_identity.drop('<categorical_column>', axis=1, inplace=True)

###<a name="blob-binningfeature"></a>Binning functie generatie

Voor het genereren van binned functies, gaan we als volgt:

1. Een reeks kolommen die u wilt de bin-een numerieke kolom toevoegen

        bins = [0, 1, 2, 4, 10, 40]
        dataframe_blobdata_bin_id = pd.cut(dataframe_blobdata['<numeric_column>'], bins)

2. Converteren naar een reeks Boole-variabelen binning

        dataframe_blobdata_bin_bool = pd.get_dummies(dataframe_blobdata_bin_id, prefix='<numeric_column>')

3. Tot slot deelnemen aan de pop variabelen terug naar de oorspronkelijke gegevensframe

        dataframe_blobdata_with_bin_bool = dataframe_blobdata.join(dataframe_blobdata_bin_bool)

##<a name="sql-featuregen"></a>Terugschrijven van gegevens naar Azure blob en gebruiken in Azure Machine Learning

Nadat u de gegevens hebt verkend en de functies nodig hebt gemaakt, kunt u de gegevens uploaden (partijen of featurized) naar een Azure blob en deze gebruiken in Azure Machine Learning met de volgende stappen: Houd er rekening mee dat aanvullende functies kunnen worden gemaakt in de Azure Machine Learning Studio ook.
1. Het gegevensframe naar een lokaal bestand schrijven

        dataframe.to_csv(os.path.join(os.getcwd(),LOCALFILENAME), sep='\t', encoding='utf-8', index=False)

2. Upload de gegevens naar Azure blob als volgt:

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
            print ("Something went wrong with uploading blob:"+BLOBNAME)

3. Nu kunnen de gegevens worden gelezen van het gebruik van de module Azure Machine Learning- [Gegevens importeren](https://msdn.microsoft.com/library/azure/4e1b0fe6-aded-4b3f-a36f-39b8862b9004/) , zoals wordt weergegeven in het scherm onderstaande blob:

![lezer blob](./media/machine-learning-data-science-process-data-blob/reader_blob.png)
