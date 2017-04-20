<properties 
   pageTitle="Aan de slag met Azure gegevens Lake Analytics Azure-opdrachtregel | Microsoft Azure" 
   description="Informatie over het gebruik van de opdrachtregel Azure te maken van een account voor gegevensopslag Lake, maakt u een gegevens Lake Analytics-taak met I-SQL en de taak verstuurt. " 
   services="data-lake-analytics" 
   documentationCenter="" 
   authors="edmacauley" 
   manager="jhubbard" 
   editor="cgronlun"/>
 
<tags
   ms.service="data-lake-analytics"
   ms.devlang="na"
   ms.topic="hero-article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data" 
   ms.date="05/16/2016"
   ms.author="edmaca"/>

# <a name="tutorial-get-started-with-azure-data-lake-analytics-using-azure-command-line-interface-cli"></a>Zelfstudie: aan de slag met Azure gegevens Lake analytische mogelijkheden op basis van Azure-Interface met opdrachtregel (CLI)

[AZURE.INCLUDE [get-started-selector](../../includes/data-lake-analytics-selector-get-started.md)]


Informatie over het gebruik van Azure CLI Azure gegevens Lake Analytics-accounts maken en verzenden van taken die moeten worden gegevens Lake Analytics-accounts gegevens Lake Analytics taken definiëren in [I-SQL](data-lake-analytics-u-sql-get-started.md). Zie [overzicht van de Azure gegevens Lake Analytics](data-lake-analytics-overview.md)voor meer informatie over gegevens Lake Analytics.

In deze zelfstudie wordt u een taak die leest een tabblad gescheiden waarden ()-bestand en zet deze in een bestand met door komma's gescheiden waarden (CSV) ontwikkelen. Ga via de dezelfde zelfstudie met andere ondersteunde hulpprogramma's, te klikken op de tabbladen boven aan deze sectie.

##<a name="prerequisites"></a>Vereisten voor

Voordat u deze zelfstudie begint, hebt u het volgende:

- **Een Azure-abonnement**. Zie [Azure krijgen gratis proefversie](https://azure.microsoft.com/pricing/free-trial/).
- **Azure CLI**. Zie [installeren en configureren van Azure CLI](../xplat-cli-install.md).
    - Download en installeer de **voorlopige versie** [Azure CLI hulpprogramma's](https://github.com/MicrosoftBigData/AzureDataLake/releases) om te voltooien van deze demo.
- **Verificatie**via de volgende opdracht uit:

        azure login
    Zie voor meer informatie over verificatie via een account voor werk of school, [verbinding maken met een Azure-abonnement van de Azure CLI](../xplat-cli-connect.md).
- **Overschakelen naar de modus Azure resourcemanager**, met de volgende opdracht:

        azure config mode arm
        
## <a name="create-data-lake-analytics-account"></a>Gegevens Lake Analytics-account maken

Voordat u kunt alle taken uitvoeren, moet u een gegevens Lake Analytics-account hebben. Als u wilt een gegevens Lake Analytics-account hebt gemaakt, moet u het volgende opgeven:

- **Azure resourcegroep**: A gegevens Lake Analytics-account moet worden gemaakt in een groep Azure Resource. [Azure resourcemanager](../azure-resource-manager/resource-group-overview.md) kunt u werken met de resources in uw toepassing als een groep. U kunt implementeren, bijwerken of verwijderen alle bronnen voor uw toepassing een eenmalige, gecoördineerde betrekking heeft.  

    De resource opsommen van groepen in uw abonnement:
    
        azure group list 
    
    Een nieuwe resourcegroep maken:

        azure group create -n "<Resource Group Name>" -l "<Azure Location>"

- **Gegevens Lake Analytics-accountnaam**
- **Locatie**: een van de Azure datacenters die ondersteuning biedt voor gegevens Lake Analytics.
- **Standaard gegevens Lake account**: elke gegevens Lake Analytics-account heeft een standaardaccount voor gegevens Lake.

    Overzicht van de bestaande gegevens Lake-account:
    
        azure datalake store account list

    Een nieuwe gegevens Lake-account maken:

        azure datalake store account create "<Data Lake Store Account Name>" "<Azure Location>" "<Resource Group Name>"

    > [AZURE.NOTE] De accountnaam gegevens Lake mag alleen kleine letters en cijfers bevatten.



**Een gegevens Lake Analytics-account maken**

        azure datalake analytics account create "<Data Lake Analytics Account Name>" "<Azure Location>" "<Resource Group Name>" "<Default Data Lake Account Name>"

        azure datalake analytics account list
        azure datalake analytics account show "<Data Lake Analytics Account Name>"          

![Gegevens Lake Analytics weergeven account](./media/data-lake-analytics-get-started-cli/data-lake-analytics-show-account-cli.png)

> [AZURE.NOTE] De gegevens Lake Analytics-accountnaam mag alleen kleine letters en cijfers bevatten.


## <a name="upload-data-to-data-lake-store"></a>Gegevens uploaden naar Lake gegevensopslag

In deze zelfstudie wordt u enkele logboeken zoeken verwerken.  Het logboek zoeken kan worden opgeslagen in gegevens Lake store of Azure-blobopslag. 

De Portal Azure biedt een gebruikersinterface voor sommige voorbeeldbestanden gegevens kopiëren naar het gegevens Lake standaardaccount, waaronder een logboekbestand zoeken. Zie [de brongegevens voorbereiden](data-lake-analytics-get-started-portal.md#prepare-source-data) om de gegevens te uploaden naar het standaardaccount voor gegevensopslag Lake.

Als u wilt uploaden van bestanden met cli, gebruik de volgende opdracht uit:

    azure datalake store filesystem import "<Data Lake Store Account Name>" "<Path>" "<Destination>"
    azure datalake store filesystem list "<Data Lake Store Account Name>" "<Path>"

Gegevens Lake Analytics ook toegang tot Azure-blobopslag.  Zie [gebruik van de Azure CLI met Azure-opslag](../storage/storage-azure-cli.md)voor het uploaden van gegevens met Azure-blobopslag.

## <a name="submit-data-lake-analytics-jobs"></a>Gegevens Lake Analytics taken indienen

De gegevens Lake Analytics-taken zijn in de I-SQL-taal geschreven. Zie [aan de slag met I-SQL-taal](data-lake-analytics-u-sql-get-started.md) en [I-SQL-Naslaggids](http://go.microsoft.com/fwlink/?LinkId=691348)meer informatie over het I-SQL.

**Een script van de taak gegevens Lake analyses maken**

- Maak een tekstbestand met de volgende I-SQL-script en sla het tekstbestand op uw werkstation:

        @searchlog =
            EXTRACT UserId          int,
                    Start           DateTime,
                    Region          string,
                    Query           string,
                    Duration        int?,
                    Urls            string,
                    ClickedUrls     string
            FROM "/Samples/Data/SearchLog.tsv"
            USING Extractors.Tsv();
        
        OUTPUT @searchlog   
            TO "/Output/SearchLog-from-Data-Lake.csv"
        USING Outputters.Csv();

    Deze I-SQL-script leest het bronbestand van de gegevens met **Extractors.Tsv()**en maakt vervolgens een CSV-bestand met **Outputters.Csv()**. 
    
    De twee paden niet worden gewijzigd, tenzij u het bronbestand naar een andere locatie kopiëren.  Gegevens Lake Analytics maakt de uitvoermap als deze niet bestaat.
    
    Het is eenvoudiger relatieve paden gebruiken voor bestanden die zijn opgeslagen in standaardprogramma gegevens Lake accounts. U kunt ook absolute paden gebruiken.  Bijvoorbeeld 
    
        adl://<Data LakeStorageAccountName>.azuredatalakestore.net:443/Samples/Data/SearchLog.tsv
        
    U moet absolute paden gebruiken voor toegang tot bestanden in gekoppelde opslag-accounts.  De syntaxis voor bestanden die zijn opgeslagen in de gekoppelde opslag van Azure-account is:
    
        wasb://<BlobContainerName>@<StorageAccountName>.blob.core.windows.net/Samples/Data/SearchLog.tsv

    >[AZURE.NOTE] Azure Blob container met openbare BLOB's of openbare containers toegangsmachtigingen worden momenteel niet ondersteund.      

    
**De taak indienen**


    azure datalake analytics job create  "<Data Lake Analytics Account Name>" "<Job Name>" "<Script>"
    
    
De volgende opdrachten kunnen worden gebruikt om een lijst met taken, de taakdetails van de en de taken annuleren:

    azure datalake analytics job cancel "<Data Lake Analytics Account Name>" "<Job Id>"
    azure datalake analytics job list "<Data Lake Analytics Account Name>"
    azure datalake analytics job show "<Data Lake Analytics Account Name>" "<Job Id>"

Nadat de taak is voltooid, kunt u de volgende cmdlets gebruiken voor een overzicht van het bestand en download het bestand:
    
    azure datalake store filesystem list "<Data Lake Store Account Name>" "/Output"
    azure datalake store filesystem export "<Data Lake Store Account Name>" "/Output/SearchLog-from-Data-Lake.csv" "<Destination>"
    azure datalake store filesystem read "<Data Lake Store Account Name>" "/Output/SearchLog-from-Data-Lake.csv" <Length> <Offset>

## <a name="see-also"></a>Zie ook

- Klik op het tabblad-selectors boven aan de pagina overzicht van de dezelfde zelfstudie met een ander hulpprogramma.
- Een complexe query's, raadpleegt u [analyseren Website Logboeken door middel van Azure gegevens Lake analyses](data-lake-analytics-analyze-weblogs.md).
- Zie [ontwikkelen I--SQL-scripts met gegevens Lake Tools for Visual Studio](data-lake-analytics-data-lake-tools-get-started.md)om te beginnen ontwikkelen van toepassingen I-SQL.
- Zie [aan de slag met Azure gegevens Lake Analytics U SQL - taal](data-lake-analytics-u-sql-get-started.md)voor meer I-SQL.
- Zie [Beheren Azure gegevens Lake analyses met behulp van Azure Portal](data-lake-analytics-manage-use-portal.md)voor beheertaken.
- Als u een overzicht van gegevens Lake analyses, Zie [overzicht van de Azure gegevens Lake Analytics](data-lake-analytics-overview.md).

