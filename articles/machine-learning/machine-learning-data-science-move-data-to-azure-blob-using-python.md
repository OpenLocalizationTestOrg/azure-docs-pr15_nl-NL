<properties
    pageTitle="Gegevens verplaatsen naar en vanuit Azure-blobopslag Python met | Microsoft Azure"
    description="Gegevens verplaatsen naar en vanuit Python met Azure-blobopslag"
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
    ms.date="09/14/2016"
    ms.author="bradsev" />

# <a name="move-data-to-and-from-azure-blob-storage-using-python"></a>Gegevens verplaatsen naar en vanuit Python met Azure-blobopslag

Dit onderwerp wordt beschreven hoe u een lijst met, uploaden en downloaden BLOB's de Python-API gebruiken. Met de Python API die beschikbaar zijn in Azure SDK, kunt u het volgende doen:

- Een container maken
- Een blob uploaden naar een container
- BLOB's downloaden
- Lijst van de BLOB's in een container
- Een blob verwijderen

Zie voor meer informatie over het gebruik van de API Python [het gebruik van de Blob Storage-Service van Python](../storage/storage-python-how-to-use-blob-storage.md).

Richtlijnen voor technologieën die worden gebruikt om gegevens te verplaatsen naar en/of van Azure-blobopslag hier zijn gekoppeld:

[AZURE.INCLUDE [blob-storage-tool-selector](../../includes/machine-learning-blob-storage-tool-selector.md)]


> [AZURE.NOTE] Als u VM die is ingesteld met de scripts die is verstrekt door [gegevens wetenschappelijke virtuele machines in Azure](machine-learning-data-science-virtual-machines.md)gebruikt, is klikt u vervolgens AzCopy al geïnstalleerd op de VM.

> [AZURE.NOTE] Voor een volledige Inleiding tot Azure-blobopslag, raadpleegt u [Azure Blob basisbeginselen](../storage/storage-dotnet-how-to-use-blobs.md) en [Azure Blob-Service](https://msdn.microsoft.com/library/azure/dd179376.aspx).


## <a name="prerequisites"></a>Vereisten voor

In dit document wordt ervan uitgegaan dat u een Azure-abonnement, een opslag-account en de bijbehorende opslag-sleutel voor dat account hebt. Voordat u gegevens uploaden/downloaden, moet u uw Azure opslag naam en account accountsleutel weten.

- Als u wilt een Azure-abonnement hebt ingesteld, raadpleegt u de [gratis proefversie voor één maand](https://azure.microsoft.com/pricing/free-trial/).

- Zie voor instructies over het maken van een opslag-account en om het account en belangrijke informatie [over Azure opslag-accounts](../storage/storage-create-storage-account.md).


## <a name="upload-data-to-blob"></a>Gegevens uploaden naar Blob

Het codefragment van de volgende aan de bovenkant van een code Python waarin u wilt via programmacode toegang hebben tot Azure opslag toevoegen:

    from azure.storage.blob import BlobService

Het object **BlobService** kunt u werken met containers en BLOB's. De volgende code maakt een BlobService-object met de naam en account-toets voor de account voor opslag. Accountnaam en accountsleutel vervangen door uw reële account en van toetsen.

    blob_service = BlobService(account_name="<your_account_name>", account_key="<your_account_key>")

De volgende methoden gebruiken om gegevens te uploaden naar een blob:

1. zet\_blokkeren\_blob\_uit\_pad (de inhoud van een bestand vanaf het opgegeven pad uploads)
2. zet\_block_blob\_uit\_bestand (uploads de inhoud van een al geopende bestand/stroom)
3. zet\_blokkeren\_blob\_uit\_bytes (uploads een matrix van bytes)
4. zet\_blokkeren\_blob\_uit\_tekst (de waarde van de opgegeven tekst met de opgegeven codering uploads)

De volgende code uploads een lokaal bestand aan een container:

    blob_service.put_block_blob_from_path("<your_container_name>", "<your_blob_name>", "<your_local_file_name>")

De volgende code uploads alle bestanden (exclusief mappen) in een lokale map met blob storage:

    from azure.storage.blob import BlobService
    from os import listdir
    from os.path import isfile, join

    # Set parameters here
    ACCOUNT_NAME = "<your_account_name>"
    ACCOUNT_KEY = "<your_account_key>"
    CONTAINER_NAME = "<your_container_name>"
    LOCAL_DIRECT = "<your_local_directory>"     

    blob_service = BlobService(account_name=ACCOUNT_NAME, account_key=ACCOUNT_KEY)
    # find all files in the LOCAL_DIRECT (excluding directory)
    local_file_list = [f for f in listdir(LOCAL_DIRECT) if isfile(join(LOCAL_DIRECT, f))]

    file_num = len(local_file_list)
    for i in range(file_num):
        local_file = join(LOCAL_DIRECT, local_file_list[i])
        blob_name = local_file_list[i]
        try:
            blob_service.put_block_blob_from_path(CONTAINER_NAME, blob_name, local_file)
        except:
            print "something wrong happened when uploading the data %s"%blob_name


## <a name="download-data-from-blob"></a>Gegevens downloaden uit Blob

De volgende methoden gebruiken om gegevens downloaden uit een blob:
1. krijg\_blob\_naar\_pad
2. krijg\_blob\_naar\_bestand
3. krijg\_blob\_naar\_bytes
4. krijg\_blob\_naar\_tekst

Deze methoden die de benodigde logische groepen te verdelen wanneer de grootte van de gegevens groter is dan 64 MB uitvoeren.

De volgende code wordt de inhoud van een blob in een container naar een lokaal bestand gedownload:

    blob_service.get_blob_to_path("<your_container_name>", "<your_blob_name>", "<your_local_file_name>")

De volgende code downloads alle BLOB's uit een container. Deze lijst gebruikt\_BLOB's voor de lijst met beschikbare BLOB's in de container en downloadt u deze naar een lokale map.

    from azure.storage.blob import BlobService
    from os.path import join

    # Set parameters here
    ACCOUNT_NAME = "<your_account_name>"
    ACCOUNT_KEY = "<your_account_key>"
    CONTAINER_NAME = "<your_container_name>"
    LOCAL_DIRECT = "<your_local_directory>"     

    blob_service = BlobService(account_name=ACCOUNT_NAME, account_key=ACCOUNT_KEY)

    # List all blobs and download them one by one
    blobs = blob_service.list_blobs(CONTAINER_NAME)
    for blob in blobs:
        local_file = join(LOCAL_DIRECT, blob.name)
        try:
            blob_service.get_blob_to_path(CONTAINER_NAME, blob.name, local_file)
        except:
            print "something wrong happened when downloading the data %s"%blob.name
