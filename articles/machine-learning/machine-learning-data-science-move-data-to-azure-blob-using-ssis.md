<properties
    pageTitle="Gegevens verplaatsen of naar Azure-blobopslag van SSIS-connectors | Microsoft Azure"
    description="Gegevens verplaatsen of naar Azure-blobopslag van SSIS-connectors."
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

# <a name="move-data-to-or-from-azure-blob-storage-using-ssis-connectors"></a>Gegevens verplaatsen of naar Azure-blobopslag van SSIS-connectors

De [SQL Server Integration Services Feature Pack voor Azure](https://msdn.microsoft.com/library/mt146770.aspx) biedt onderdelen verbinding maken met Azure doorverbinden gegevens tussen Azure en on-premises gegevensbronnen en procesgegevens die zijn opgeslagen in Azure wordt aangegeven.

Richtlijnen voor technologieën die worden gebruikt om gegevens te verplaatsen naar en/of van Azure-blobopslag hier zijn gekoppeld:

[AZURE.INCLUDE [blob-storage-tool-selector](../../includes/machine-learning-blob-storage-tool-selector.md)]


Klanten hebt on-premises gegevens verplaatst naar de cloud, kunnen ze openen vanuit een Azure-service om te profiteren van de volledige kracht van de suite van Azure technologieën. Het kan worden gebruikt, bijvoorbeeld in Azure Machine Learning of op een cluster HDInsight.

Dit is meestal worden de eerste stap voor het walkthroughs [SQL](machine-learning-data-science-process-sql-walkthrough.md) versus [HDInsight](machine-learning-data-science-process-hive-walkthrough.md) .

Zie voor een discussie canonieke scenario's beschreven die SSIS gebruiken voor het uitvoeren van zakelijke behoeften algemene in scenario's de integratie van de hybride gegevens, [met SQL Server Integration Services Feature Pack voor Azure meer doen](http://blogs.msdn.com/b/ssis/archive/2015/06/25/doing-more-with-sql-server-integration-services-feature-pack-for-azure.aspx) blog.

> [AZURE.NOTE] Voor een volledige Inleiding tot Azure-blobopslag, raadpleegt u [Azure Blob basisbeginselen](../storage/storage-dotnet-how-to-use-blobs.md) en [Azure Blob-Service](https://msdn.microsoft.com/library/azure/dd179376.aspx).

## <a name="prerequisites"></a>Vereisten voor

Als u wilt de stappen in dit artikel beschreven uitvoeren, moet u een Azure-abonnement en een instellen opslag Azure-account hebben. Uw Azure opslag naam en account accountsleutel uploaden of downloaden van gegevens, moet u weten.

- Als u wilt een **Azure-abonnement**hebt ingesteld, raadpleegt u de [gratis proefversie voor één maand](https://azure.microsoft.com/pricing/free-trial/).

- Zie voor instructies over het maken van een **account voor opslagruimte** en om van account en belangrijke informatie voor [over Azure opslag-accounts](../storage/storage-create-storage-account.md).


Als u wilt de **SSIS verbindingslijnen**gebruiken, moet u het volgende downloaden:

- **SQL Server-2014 of 2016 standaard (of hoger)**: Installeer SQL Server Integration Services bevat.
- **Microsoft SQL Server-2014 of 2016 Integration Services Feature Pack voor Azure**: deze kunnen worden gedownload, respectievelijk van [SQL Server 2014 Integration Services](http://www.microsoft.com/download/details.aspx?id=47366) en [SQL Server 2016 Integration Services](https://www.microsoft.com/download/details.aspx?id=49492) pagina's.

> [AZURE.NOTE] SSIS met SQL Server is geïnstalleerd, maar het is niet inbegrepen in de Express-versie. Zie voor informatie over welke toepassingen zijn opgenomen in verschillende versies van SQL Server, [Edities van SQL Server](http://www.microsoft.com/en-us/server-cloud/products/sql-server-editions/)

Zie voor trainingsmateriaal op SSIS, [Handen op Training voor SSIS](http://www.microsoft.com/download/details.aspx?id=20766)

Gebruik voor meer informatie over hoe u omhoog en voorlopig SISS maken van eenvoudige extractie, transformatie en uitpak-pakketten, Zie [SSIS zelfstudie: een eenvoudige ETL-pakket maken](https://msdn.microsoft.com/library/ms169917.aspx).

## <a name="download-nyc-taxi-dataset"></a>TEVENS Taxi gegevensset downloaden  
Het beschreven voorbeeld kunt u hier een openbaar gegevensset--de gegevensset [Tevens Taxi reizen](http://www.andresmh.com/nyctaxitrips/) gebruikt. De gegevensset bestaat uit ongeveer 173 miljoen taxi ritjes in tevens in het jaar 2013. Er zijn twee soorten gegevens: reis details gegevens en tarief gegevens. Als er een bestand voor elke maand is, zijn er 24 bestanden in alle, die elk ongeveer 2GB niet gecomprimeerd is.


## <a name="upload-data-to-azure-blob-storage"></a>Gegevens uploaden naar Azure-blobopslag
Als u wilt verplaatsen gegevens met behulp van de SSIS-functiepakket vanuit on-premises met Azure-blobopslag, gebruiken we een exemplaar van de [**Azure Blob uploaden taak**](https://msdn.microsoft.com/library/mt146776.aspx), zoals hier wordt weergegeven:

![configureren-gegevens-wetenschappelijke-vm](./media/machine-learning-data-science-move-data-to-azure-blob-using-ssis/ssis-azure-blob-upload-task.png)


De parameters die de taak wordt gebruikt, worden hier beschreven:


Veld|Beschrijving|
----------------------|----------------|
**AzureStorageConnection**|Hiermee geeft u een bestaande Azure opslag Connection Manager of Hiermee maakt u een nieuwe naam die naar een Azure opslag-account die verwijst verwijst naar waar de blob-bestanden worden gehost.|
**BlobContainer**|Hiermee geeft u de naam van de container blob waarin de geüploade bestanden als BLOB's.|
**BlobDirectory**|Hiermee geeft u de blob-map waarin het geüploade bestand is opgeslagen als een blob blokkeren. De map blob is een virtuele hiërarchische structuur. Als de blob al bestaat, it ia vervangen.|
**LocalDirectory**|Hiermee geeft u de lokale map met de bestanden te uploaden.|
**FileName**|Hiermee geeft u een filter naam om bestanden met de opgegeven naampatroon te selecteren. Bijvoorbeeld MySheet\*.xls\* bestanden zoals MySheet001.xls en MySheetABC.xlsx bevat|
**TimeRangeFrom/TimeRangeTo**|Hiermee geeft u een filter tijd bereik. Bestanden gewijzigde na *TimeRangeFrom* en vóór *TimeRangeTo* zijn opgenomen.|


> [AZURE.NOTE] De referenties **AzureStorageConnection** moeten correct, maar de **BlobContainer** moet bestaan voordat de overdracht wordt uitgevoerd.

## <a name="download-data-from-azure-blob-storage"></a>Gegevens downloaden uit Azure-blobopslag

Als u wilt gegevens downloaden uit Azure-blobopslag naar on-premises opslagruimte met SSIS, gebruikt u een exemplaar van de [Azure Blob uploaden taak](https://msdn.microsoft.com/library/mt146779.aspx).

##<a name="more-advanced-ssis-azure-scenarios"></a>Meer geavanceerde SSIS-Azure-scenario 's
We opmerking hier dat het SSIS-functiepakket voor meer complexe stromen moeten worden verwerkt door verpakking taken bij elkaar is toegestaan. De blobgegevens kan bijvoorbeeld feed rechtstreeks in een cluster HDInsight, waarvan de uitvoer kan worden gedownload terug naar een blob en vervolgens naar de on-premises opslag. SSIS component en varken taken kunnen worden uitgevoerd op een HDInsight cluster van extra SSIS-connectors:

- U kunt een component-script uitvoeren op een Azure HDInsight cluster met SSIS [Azure HDInsight component taak](https://msdn.microsoft.com/library/mt146771.aspx)te gebruiken.
- U kunt een varken script uitvoeren op een Azure HDInsight cluster met SSIS [Azure HDInsight varken taak](https://msdn.microsoft.com/library/mt146781.aspx)te gebruiken.
