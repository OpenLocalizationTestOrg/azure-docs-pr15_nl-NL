<properties
    pageTitle="Gegevens verplaatsen naar en vanuit Azure-blobopslag | Microsoft Azure"
    description="Gegevens verplaatsen naar en vanuit Azure-blobopslag"
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
    ms.author="bradsev;sachouks" />

# <a name="move-data-to-and-from-azure-blob-storage"></a>Gegevens verplaatsen naar en vanuit Azure-blobopslag

[AZURE.INCLUDE [cap-ingest-data-selector](../../includes/cap-ingest-data-selector.md)]

Richtlijnen voor technologieën die worden gebruikt om gegevens te verplaatsen naar en/of van Azure-blobopslag hier zijn gekoppeld:

[AZURE.INCLUDE [blob-storage-tool-selector](../../includes/machine-learning-blob-storage-tool-selector.md)]
 
Welke methode voor het meest geschikt voor u is, is afhankelijk van uw scenario. De [scenario's voor geavanceerde analyses in Azure Machine Learning](machine-learning-data-science-plan-sample-scenarios.md) -artikel vindt u de bronnen die u nodig voor een groot aantal gegevens wetenschappelijke werkstromen die worden gebruikt in het proces van geavanceerde analyses hebt bepalen.

> [AZURE.NOTE] Voor een volledige Inleiding tot Azure-blobopslag, raadpleegt u [Azure Blob basisbeginselen](../storage/storage-dotnet-how-to-use-blobs.md) en [Azure Blob-Service](https://msdn.microsoft.com/library/azure/dd179376.aspx).

Als alternatief, kunt u [Factory van Azure-gegevens](https://azure.microsoft.com/services/data-factory/) naar: 

- maken en plannen van een pijplijn die gedownload van de gegevens van Azure-blobopslag 
- doorgegeven aan een gepubliceerde Azure Machine Learning-webservice 
- de resultaten Bekijk analyses, ontvangen en 
- Upload de resultaten naar opslag. 

Zie [de blog pijpleidingen maken met Azure gegevens Factory en Azure Machine Learning](../data-factory/data-factory-azure-ml-batch-execution-activity.md)voor meer informatie.

## <a name="prerequisites"></a>Vereisten voor

In dit document wordt ervan uitgegaan dat u een Azure-abonnement, een opslag-account en de bijbehorende opslag-sleutel voor dat account hebt. Voordat u gegevens uploaden/downloaden, moet u uw Azure opslag naam en account accountsleutel weten.

- Als u wilt een Azure-abonnement hebt ingesteld, raadpleegt u de [gratis proefversie voor één maand](https://azure.microsoft.com/pricing/free-trial/).
- Zie voor instructies over het maken van een opslag-account en om het account en belangrijke informatie [over Azure opslag-accounts](../storage/storage-create-storage-account.md).
