<properties
    pageTitle="Gegevens verplaatsen naar en vanuit Azure-blobopslag met AzCopy | Microsoft Azure"
    description="Gegevens verplaatsen naar en vanuit AzCopy met Azure-blobopslag"
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

# <a name="move-data-to-and-from-azure-blob-storage-using-azcopy"></a>Gegevens verplaatsen naar en vanuit AzCopy met Azure-blobopslag

AzCopy is een hulpprogramma voor de opdrachtregel ontworpen voor uploaden, downloaden en kopiëren van gegevens en naar Microsoft Azure blob, bestand en tabelopslag.

Zie [Aan de slag met het hulpprogramma AzCopy-opdrachtregel](../storage/storage-use-azcopy.md)voor instructies over het installeren van AzCopy en aanvullende informatie over het gebruik van deze met het Azure platform.

Richtlijnen voor technologieën die worden gebruikt om gegevens te verplaatsen naar en/of van Azure-blobopslag hier zijn gekoppeld:

[AZURE.INCLUDE [blob-storage-tool-selector](../../includes/machine-learning-blob-storage-tool-selector.md)]


> [AZURE.NOTE] Als u VM die is ingesteld met de scripts die is verstrekt door [gegevens wetenschappelijke virtuele machines in Azure](machine-learning-data-science-virtual-machines.md)gebruikt, is klikt u vervolgens AzCopy al geïnstalleerd op de VM.

> [AZURE.NOTE] Voor een volledige Inleiding tot Azure-blobopslag, raadpleegt u [Azure Blob basisbeginselen](../storage/storage-dotnet-how-to-use-blobs.md) en [Azure Blob-Service](https://msdn.microsoft.com/library/azure/dd179376.aspx).


## <a name="prerequisites"></a>Vereisten voor

In dit document wordt ervan uitgegaan dat u een Azure-abonnement, een opslag-account en de bijbehorende opslag-sleutel voor dat account hebt. Voordat u gegevens uploaden/downloaden, moet u uw Azure opslag naam en account accountsleutel weten.

- Als u wilt een Azure-abonnement hebt ingesteld, raadpleegt u de [gratis proefversie voor één maand](https://azure.microsoft.com/pricing/free-trial/).

- Zie voor instructies over het maken van een opslag-account en om het account en belangrijke informatie [over Azure opslag-accounts](../storage/storage-create-storage-account.md).


## <a name="run-azcopy-commands"></a>Opdrachten uitvoeren AzCopy

Als u wilt uitvoeren AzCopy opdrachten, open een opdrachtvenster en navigeer naar de map AzCopy installeren op uw computer, waarin de uitvoerbare AzCopy.exe zich bevindt. 

De syntaxis van de eenvoudige voor AzCopy opdrachten luidt als volgt:

    AzCopy /Source:<source> /Dest:<destination> [Options]

>[AZURE.NOTE] U kunt de installatielocatie AzCopy toevoegen aan uw systeempad en voer vervolgens de opdrachten vanuit een andere map. Standaard is AzCopy *% ProgramFiles(x86) %\Microsoft SDKs\Azure\AzCopy* of *%ProgramFiles%\Microsoft SDKs\Azure\AzCopy*geïnstalleerd.

## <a name="upload-files-to-an-azure-blob"></a>Bestanden uploaden naar een Azure blob

Als u wilt een bestand uploaden, gebruikt u de volgende opdracht:

    # Upload from local file system
    AzCopy /Source:<your_local_directory> /Dest: https://<your_account_name>.blob.core.windows.net/<your_container_name> /DestKey:<your_account_key> /S


## <a name="download-files-from-an-azure-blob"></a>Bestanden downloaden van een Azure blob

Als u wilt een bestand downloaden via een Azure blob, gebruikt u de volgende opdracht:

    # Downloading blobs to local file system
    AzCopy /Source:https://<your_account_name>.blob.core.windows.net/<your_container_name>/<your_sub_directory_at_blob>  /Dest:<your_local_directory> /SourceKey:<your_account_key> /Pattern:<file_pattern> /S


## <a name="transfer-blobs-between-azure-containers"></a>BLOB's overbrengen van Azure containers

Als u wilt overbrengen BLOB's tussen Azure containers, gebruikt u de volgende opdracht uit:

    # Transferring blobs between Azure containers
    AzCopy /Source:https://<your_account_name1>.blob.core.windows.net/<your_container_name1>/<your_sub_directory_at_blob1> /Dest:https://<your_account_name2>.blob.core.windows.net/<your_container_name2>/<your_sub_directory_at_blob2> /SourceKey:<your_account_key1> /DestKey:<your_account_key2> /Pattern:<file_pattern> /S

    <your_account_name>: your storage account name
    <your_account_key>: your storage account key
    <your_container_name>: your container name
    <your_sub_directory_at_blob>: the sub directory in the container
    <your_local_directory>: directory of local file system where files to be uploaded from or the directory of local file system files to be downloaded to
    <file_pattern>: pattern of file names to be transferred. The standard wildcards are supported


## <a name="tips-for-using-azcopy"></a>Tips voor het gebruik van AzCopy

> [AZURE.TIP]   
> 1. Wanneer uploads **uploaden van** bestanden, */S* bestanden recursief. Bestanden in submappen zonder deze parameter niet die zijn geüpload.  
> 2. Als u **gedownload** bestand, */S* Hiermee wordt gezocht in de container recursief tot alle bestanden in de opgegeven map en submappen of alle bestanden die overeenkomen met het opgegeven patroon in de opgegeven map en submappen, worden gedownload.  
> 3.  U kunt geen een **specifieke blob-bestand** downloaden met de parameter */Source* opgeven. Geef de naam van de bestand blob downloaden van de parameter */Pattern* met een specifieke bestand te downloaden. Parameter **/S** kan worden gebruikt om te laten AzCopy zoekt u een bestand naam patroon recursief. Zonder de parameter patroon downloads AzCopy alle bestanden in die map.
