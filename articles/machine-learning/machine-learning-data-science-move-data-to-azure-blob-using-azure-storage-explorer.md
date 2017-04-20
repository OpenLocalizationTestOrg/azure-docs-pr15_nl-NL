<properties 
    pageTitle="Gegevens verplaatsen naar en vanuit Azure-blobopslag met Azure Opslagverkenner | Microsoft Azure" 
    description="Gegevens verplaatsen naar en vanuit Azure-blobopslag met Azure Opslagverkenner" 
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
    ms.date="08/31/2016"
    ms.author="bradsev" />

# <a name="move-data-to-and-from-azure-blob-storage-using-azure-storage-explorer"></a>Gegevens verplaatsen naar en vanuit Azure-blobopslag met Azure Opslagverkenner

Azure opslag Explorer is een gratis hulpprogramma van Microsoft waarmee u kunt werken met gegevens van Azure-opslag in Windows, Mac OS en Linux. In dit onderwerp wordt beschreven hoe u dit gebruikt om te uploaden en downloaden van gegevens van Azure-blobopslag. Het hulpmiddel kan worden gedownload van [Microsoft Azure opslag Explorer](http://storageexplorer.com/).

Richtlijnen voor technologieën die worden gebruikt om gegevens te verplaatsen naar en/of van Azure-blobopslag hier zijn gekoppeld:
 
[AZURE.INCLUDE [blob-storage-tool-selector](../../includes/machine-learning-blob-storage-tool-selector.md)]   

 
> [AZURE.NOTE] Als u VM die is ingesteld met de scripts die is verstrekt door [gegevens wetenschappelijke virtuele machines in Azure](machine-learning-data-science-virtual-machines.md)gebruikt, is klikt u vervolgens Azure opslag Explorer al geïnstalleerd op de VM.
 
> [AZURE.NOTE] Voor een volledige Inleiding tot Azure-blobopslag, raadpleegt u [Azure Blob basisbeginselen](../storage/storage-dotnet-how-to-use-blobs.md) en [Azure Blob-Service](https://msdn.microsoft.com/library/azure/dd179376.aspx).   

## <a name="prerequisites"></a>Vereisten voor

In dit document wordt ervan uitgegaan dat u een Azure-abonnement, een opslag-account en de bijbehorende opslag-sleutel voor dat account hebt. Voordat u gegevens uploaden/downloaden, moet u uw Azure opslag naam en account accountsleutel weten. 

- Als u wilt een Azure-abonnement hebt ingesteld, raadpleegt u de [gratis proefversie voor één maand](https://azure.microsoft.com/pricing/free-trial/).
- Zie voor instructies over het maken van een opslag-account en om het account en belangrijke informatie [over Azure opslag-accounts](../storage/storage-create-storage-account.md). Noteer de access-toets voor de opslag-account als u deze toets verbinding maken met het account met het hulpprogramma Azure opslag Explorer nodig.
- Het hulpprogramma Azure opslag Explorer kan worden gedownload van [Microsoft Azure opslag Explorer](http://storageexplorer.com/). Accepteer de standaardinstellingen tijdens de installatie.


<a id="explorer"></a>
## <a name="use-azure-storage-explorer"></a>Azure opslag Explorer gebruiken 

Hoe u gegevens met behulp van Azure opslag Explorer uploaden/downloaden van de volgende stappen uit het document. 

1.  Start Microsoft Azure opslag Explorer.
2.  Als u wilt de wizard **Meld u aan bij uw account...** weer, selecteert u **Azure Accountinstellingen** pictogram, klikt u vervolgens **een account toevoegen** en voert u referenties. ![](./media/machine-learning-data-science-move-data-to-azure-blob-using-azure-storage-explorer/add-an-azure-store-account.png)
3.  Als u wilt de wizard **verbinding maken met Azure Storage** weer, selecteert u het pictogram **verbinding maken met Azure opslag** . ![](./media/machine-learning-data-science-move-data-to-azure-blob-using-azure-storage-explorer/connect-to-azure-storage-1.png)
4. Voer de access-toets van uw account Azure opslagruimte op de **verbinding maken met Azure Storage** wizard en klik op **volgende**. ![](./media/machine-learning-data-science-move-data-to-azure-blob-using-azure-storage-explorer/connect-to-azure-storage-2.png)
5. Voer opslagaccountnaam in het vak **naam van het Account** en selecteer **volgende**. ![](./media/machine-learning-data-science-move-data-to-azure-blob-using-azure-storage-explorer/attach-external-storage.png)
6. De toegevoegde opslag-mailaccount moet nu worden vermeld. U maakt een container blob in een opslag-account door met de rechtermuisknop op het knooppunt **Blob Containers** in dat account, selecteer **Blob Container maken**en voer een naam.
7. Als u wilt uploaden gegevens aan een container, selecteert u de doelcontainer en klik op de knop **uploaden** .![](./media/machine-learning-data-science-move-data-to-azure-blob-using-azure-storage-explorer/storage-accounts.png)
8. Klik op de **…** aan de rechterkant van het selectievakje **bestanden** , selecteer een of meerdere bestanden uploaden vanaf het bestandssysteem en klik op **uploaden** om te beginnen met het uploaden van bestanden.![](./media/machine-learning-data-science-move-data-to-azure-blob-using-azure-storage-explorer/upload-files-to-blob.png)
7. Downloaden van gegevens, de blob selecteren in de bijbehorende container downloaden en klik op **downloaden**. ![](./media/machine-learning-data-science-move-data-to-azure-blob-using-azure-storage-explorer/download-files-from-blob.png)


