<properties 
    pageTitle="Gegevens verkennen in Azure-blobopslag met Pandas | Microsoft Azure" 
    description="Hoe u de gegevens die zijn opgeslagen in Azure blob container met Pandas verkennen." 
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
    ms.date="09/13/2016" 
    ms.author="bradsev" /> 

#<a name="explore-data-in-azure-blob-storage-with-pandas"></a>Gegevens verkennen in Azure-blobopslag met Pandas

In dit document wordt uitgelegd hoe u gegevens die zijn opgeslagen in Azure blob container met [Pandas](http://pandas.pydata.org/) Python pakket verkennen.

Het volgende **menu** koppelingen naar onderwerpen waarin wordt beschreven hoe u met hulpmiddelen voor gegevens verkennen van verschillende opslagomgevingen. Deze taak is een stap in het [Proces van gegevens wetenschappelijk]().

[AZURE.INCLUDE [cap-explore-data-selector](../../includes/cap-explore-data-selector.md)]


## <a name="prerequisites"></a>Vereisten voor
In dit artikel wordt ervan uitgegaan dat u hebt:

* Een account Azure opslag gemaakt. Als u instructies nodig hebt, raadpleegt u [een opslag van Azure-account maken](../storage/storage-create-storage-account.md#create-a-storage-account)
* Uw gegevens opgeslagen in een Azure blob storage-account. Als u instructies nodig hebt, raadpleegt u [Moving gegevens naar en vanuit Azure Storage](../storage/storage-moving-data.md)

## <a name="load-the-data-into-a-pandas-dataframe"></a>De gegevens laadt in een DataFrame Pandas
Als u wilt verkennen en een gegevensset bewerken, moet deze eerst worden gedownload uit de bron blob naar een lokaal bestand, klik in een DataFrame Pandas kan worden geladen. Hier volgen de stappen te volgen voor deze procedure:

1. Download de gegevens van Azure blob met het volgende Python voorbeeld met blob-service. De variabele in de volgende code vervangen door uw specifieke waarden: 

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

##<a name="blob-dataexploration"></a>Voorbeelden van gegevens verkennen met Pandas

Hier volgen enkele voorbeelden van manieren om gegevens onderzoeken met Pandas:

1. Het **aantal rijen en kolommen** controleren 

        print 'the size of the data is: %d rows and  %d columns' % dataframe_blobdata.shape

2. **Controleren** van de eerste of laatste paar **rijen** in de volgende gegevensset:

        dataframe_blobdata.head(10)
        
        dataframe_blobdata.tail(10)

3. Controleer het **gegevenstype** dat elke kolom is geïmporteerd als met de volgende code
    
        for col in dataframe_blobdata.columns:
            print dataframe_blobdata[col].name, ':\t', dataframe_blobdata[col].dtype

4. De **eenvoudige stat** voor de kolommen in de gegevensset als volgt controleren
 
        dataframe_blobdata.describe()
    
5. Het aantal vermeldingen voor elke kolomwaarde als volgt bekijken

        dataframe_blobdata['<column_name>'].value_counts()

6. **De ontbrekende waarden tellen** versus het werkelijke aantal items in elke kolom met de volgende code

        miss_num = dataframe_blobdata.shape[0] - dataframe_blobdata.count()
        print miss_num
     
7.  Als u een **ontbrekende waarden** voor een specifieke kolom in de gegevens hebt, kunt u deze als volgt plaatst:

        dataframe_blobdata_noNA = dataframe_blobdata.dropna()
        dataframe_blobdata_noNA.shape

    Er is een andere manier om het ontbrekende waarden vervangen met de modusfunctie:
    
        dataframe_blobdata_mode = dataframe_blobdata.fillna({'<column_name>':dataframe_blobdata['<column_name>'].mode()[0]})        

8. Een **histogram** tekeningen met variabel aantal opslaglocaties wilt uitzetten van de verdeling van een variabele maken 
    
        dataframe_blobdata['<column_name>'].value_counts().plot(kind='bar')
        
        np.log(dataframe_blobdata['<column_name>']+1).hist(bins=50)
    
9. Bekijk **correlatie** tussen variabelen een scatterplot of de ingebouwde correlatiefunctie gebruiken

        #relationship between column_a and column_b using scatter plot
        plt.scatter(dataframe_blobdata['<column_a>'], dataframe_blobdata['<column_b>'])
        
        #correlation between column_a and column_b
        dataframe_blobdata[['<column_a>', '<column_b>']].corr()
