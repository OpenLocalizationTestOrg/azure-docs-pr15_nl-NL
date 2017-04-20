<properties
   pageTitle="Gegevens uit Azure opslag BLOB's kopiëren naar Lake gegevensopslag | Microsoft Azure"
   description="AdlCopy-hulpprogramma met gegevens van Azure opslag BLOB's kopiëren naar Lake gegevensopslag"
   services="data-lake-store"
   documentationCenter=""
   authors="nitinme"
   manager="jhubbard"
   editor="cgronlun"/>

<tags
   ms.service="data-lake-store"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data"
   ms.date="10/05/2016"
   ms.author="nitinme"/>

# <a name="copy-data-from-azure-storage-blobs-to-data-lake-store"></a>Gegevens van Azure opslag BLOB's kopiëren naar Lake gegevensopslag

> [AZURE.SELECTOR]
- [DistCp gebruiken](data-lake-store-copy-data-wasb-distcp.md)
- [AdlCopy gebruiken](data-lake-store-copy-data-azure-storage-blob.md)

Azure Lake gegevensopslag biedt een hulpmiddel opdrachtregel, [AdlCopy](http://aka.ms/downloadadlcopy), om gegevens te kopiëren uit de volgende bronnen:

* Van opslag in Azure BLOB's in Lake gegevensopslag. U kunt AdlCopy niet gebruiken om gegevens uit Lake gegevensopslag naar opslag van Azure BLOB's kopiëren.

* Tussen twee Azure Lake gegevensopslag-accounts. 

U kunt ook het hulpprogramma AdlCopy gebruiken in twee verschillende modi:

* **Zelfstandige**, waar het hulpmiddel Lake gegevensopslag resources gebruikt de taak uit te voeren.

* **Een gegevens Lake Analytics-account**, waar de eenheden die zijn toegewezen aan uw account Lake analyses van gegevens worden gebruikt om de bewerking kopiëren. U wilt mogelijk gebruik deze optie wanneer u op zoek bent naar de kopie-taken uitvoeren praat.

##<a name="prerequisites"></a>Vereisten voor

Voordat u in dit artikel begint, hebt u het volgende:

- **Een Azure-abonnement**. Zie [Azure krijgen gratis proefversie](https://azure.microsoft.com/pricing/free-trial/).

- **Azure opslag BLOB's** container met enkele gegevens.

- **Azure gegevens Lake Analytics-account (optioneel)** - Zie [aan de slag met Azure gegevens Lake Analytics](../data-lake-analytics/data-lake-analytics-get-started-portal.md) voor instructies over het maken van een account voor gegevensopslag Lake.

- **AdlCopy hulpmiddel**. Installeer het hulpmiddel AdlCopy uit [http://aka.ms/downloadadlcopy](http://aka.ms/downloadadlcopy).

## <a name="syntax-of-the-adlcopy-tool"></a>De syntaxis van de functie AdlCopy

De volgende syntaxis gebruiken om te werken met het hulpmiddel AdlCopy

    AdlCopy /Source <Blob or Data Lake Store source> /Dest <Data Lake Store destination> /SourceKey <Key for Blob account> /Account <Data Lake Analytics account> /Unit <Number of Analytics units> /Pattern 

De parameters in de syntaxis van de worden hieronder beschreven:

| Optie    | Beschrijving                                                                                                                                                                                                                                                                                                                                                                                                          |
|-----------|------------|
| Bron    | Hiermee geeft de locatie van de brongegevens in de opslag van Azure blob. De bron is een container blob, een blob of een ander account voor gegevensopslag Lake.                                                                                                                                                                                                                                                                                                 |
| Dest      | Hiermee geeft u de bestemming Lake gegevensopslag om naar te kopiëren.                                                                                                                                                                                                                                                                                                                                                                |
| SourceKey | Hiermee geeft u de opslagruimte access-toets voor de opslag van Azure blob bron. Dit is vereist alleen als de bron een container blob of een blob is.                                                                                                                                                                                                                                                                                                                                                 |
| Account   | **Optioneel**. Gebruik deze optie als u wilt gebruiken van Azure gegevens Lake Analytics-account de kopie-taak uit te voeren. Als u de optie /Account in de syntaxis van de gebruiken maar geen een gegevens Lake Analytics-account opgeeft, wordt een standaardaccount in AdlCopy gebruikt de taak uit te voeren. Ook als u deze optie gebruikt, moet u toevoegen de bron (Azure opslag Blob)- en doeltabellen (Azure gegevensopslag-Lake) als gegevensbronnen voor uw gegevens Lake Analytics-account.  |
| Eenheden     |  Hiermee geeft u het aantal gegevens Lake Analytics-eenheden dat wordt gebruikt voor het project kopiëren. Deze optie is verplicht als u de optie **/Account** gebruiken om op te geven van de gegevens Lake Analytics-account.
| Patroon   | Hiermee geeft u een patroon regex waarmee wordt aangegeven welke BLOB's of bestanden om te kopiëren. AdlCopy gebruikt hoofdlettergevoelig overeenkomende. Het standaardpatroon dat wordt gebruikt wanneer er geen patroon is opgegeven, is om alle items te kopiëren. Meerdere bestand patronen opgeven wordt niet ondersteund.                                                                                                                                                                                                                                                                                                                                               



## <a name="use-adlcopy-as-standalone-to-copy-data-from-an-azure-storage-blob"></a>AdlCopy (als zelfstandig product) gebruiken om gegevens te kopiëren vanuit een opslag van Azure blob

1. Open een opdrachtprompt en navigeer naar de map waarin AdlCopy wordt geïnstalleerd, meestal `%HOMEPATH%\Documents\adlcopy`.

2. Voer de volgende opdracht kopiëren van een specifieke blob van de Broncontainer naar een gegevensopslag Lake:

        AdlCopy /source https://<source_account>.blob.core.windows.net/<source_container>/<blob name> /dest swebhdfs://<dest_adls_account>.azuredatalakestore.net/<dest_folder>/ /sourcekey <storage_account_key_for_storage_container>

    Bijvoorbeeld:

        AdlCopy /source https://mystorage.blob.core.windows.net/mycluster/HdiSamples/HdiSamples/WebsiteLogSampleData/SampleLog/909f2b.log /dest swebhdfs://mydatalakestore.azuredatalakestore.net/mynewfolder/ /sourcekey uJUfvD6cEvhfLoBae2yyQf8t9/BpbWZ4XoYj4kAS5Jf40pZaMNf0q6a8yqTxktwVgRED4vPHeh/50iS9atS5LQ==

    
    >[AZURE.NOTE] De syntaxis van de bovenstaande Hiermee geeft u het bestand moet worden gekopieerd naar een map in het account voor gegevensopslag Lake. AdlCopy gereedschap maakt u een map als de naam van de opgegeven map niet bestaat.

    U wordt gevraagd om in te voeren van de referenties voor de Azure abonnement waaronder u uw account voor gegevensopslag Lake hebt. Hier ziet u een uitvoer ongeveer als volgt uit:

        Initializing Copy.
        Copy Started.
        100% data copied.
        Finishing Copy.
        Copy Completed. 1 file copied.

3. U kunt ook alle BLOB's uit een container kopiëren naar de gegevensopslag Lake rekening met de volgende opdracht:

        AdlCopy /source https://<source_account>.blob.core.windows.net/<source_container>/ /dest swebhdfs://<dest_adls_account>.azuredatalakestore.net/<dest_folder>/ /sourcekey <storage_account_key_for_storage_container>        

    Bijvoorbeeld:

        AdlCopy /Source https://mystorage.blob.core.windows.net/mycluster/example/data/gutenberg/ /dest adl://mydatalakestore.azuredatalakestore.net/mynewfolder/ /sourcekey uJUfvD6cEvhfLoBae2yyQf8t9/BpbWZ4XoYj4kAS5Jf40pZaMNf0q6a8yqTxktwVgRED4vPHeh/50iS9atS5LQ==

## <a name="use-adlcopy-as-standalone-to-copy-data-from-another-data-lake-store-account"></a>AdlCopy (als zelfstandig product) gebruiken om gegevens te kopiëren vanuit een ander account voor gegevensopslag Lake

U kunt ook AdlCopy gebruiken om gegevens tussen twee Lake gegevensopslag accounts te kopiëren.

1. Open een opdrachtprompt en navigeer naar de map waarin AdlCopy is geïnstalleerd, meestal `%HOMEPATH%\Documents\adlcopy`.

2. Voer de volgende opdracht naar een bepaald bestand van de ene Lake gegevensopslag account naar een andere kopiëren.

        AdlCopy /Source adl://<source_adls_account>.azuredatalakestore.net/<path_to_file> /dest adl://<dest_adls_account>.azuredatalakestore.net/<path>/

    Bijvoorbeeld:

        AdlCopy /Source adl://mydatastore.azuredatalakestore.net/mynewfolder/909f2b.log /dest adl://mynewdatalakestore.azuredatalakestore.net/mynewfolder/

    >[AZURE.NOTE] De syntaxis van de bovenstaande Hiermee geeft u het bestand moet worden gekopieerd naar een map in de bestemming Lake gegevensopslag-account. AdlCopy gereedschap maakt u een map als de naam van de opgegeven map niet bestaat.

    U wordt gevraagd om in te voeren van de referenties voor de Azure abonnement waaronder u uw account voor gegevensopslag Lake hebt. Hier ziet u een uitvoer ongeveer als volgt uit:

        Initializing Copy.
        Copy Started.|
        100% data copied.
        Finishing Copy.
        Copy Completed. 1 file copied.

3. De volgende opdracht wordt alle bestanden in een specifieke map in de bron Lake gegevensopslag account kopiëren naar een map in de bestemming Lake gegevensopslag-account.

        AdlCopy /Source adl://mydatastore.azuredatalakestore.net/mynewfolder/ /dest adl://mynewdatalakestore.azuredatalakestore.net/mynewfolder/

## <a name="use-adlcopy-with-data-lake-analytics-account-to-copy-data"></a>AdlCopy (met gegevens Lake Analytics-account) gebruiken om gegevens te kopiëren

U kunt ook uw gegevens Lake Analytics-account de taak AdlCopy om de gegevens van Azure opslag BLOB's kopiëren naar Lake gegevensopslag uit te voeren. Meestal gebruikt u deze optie wanneer de gegevens moeten worden verplaatst in het bereik van gigabytes en TB en u wilt dat de prestaties van betere en overzichtelijk doorvoer.

Als u uw gegevens Lake Analytics-account met AdlCopy wilt kopiëren uit een Azure opslag Blob, moet de bron (Azure opslag Blob) worden toegevoegd als een gegevensbron voor uw gegevens Lake Analytics-account. Zie [gegevensbronnen beheren gegevens Lake Analytics-account](..//data-lake-analytics/data-lake-analytics-manage-use-portal.md#manage-account-data-sources)voor instructies over het toevoegen van extra gegevensbronnen met uw gegevens Lake Analytics-account.

>[AZURE.NOTE] Als u vanuit een ander account Azure Lake gegevensopslag als de bron een gegevens Lake Analytics-account kopieert, hoeft u niet aan het account voor gegevensopslag Lake koppelen aan de gegevens Lake Analytics-account. De vereiste naar de store bron koppelen aan de gegevens Lake Analytics-account is alleen als de bron een opslag van Azure-account is.

Voer de volgende opdracht uit een opslag van Azure blob kopiëren naar een Lake gegevensopslag-account via gegevens Lake Analytics-account:

    AdlCopy /source https://<source_account>.blob.core.windows.net/<source_container>/<blob name> /dest swebhdfs://<dest_adls_account>.azuredatalakestore.net/<dest_folder>/ /sourcekey <storage_account_key_for_storage_container> /Account <data_lake_analytics_account> /Unit <number_of_data_lake_analytics_units_to_be_used>

Bijvoorbeeld:

    AdlCopy /Source https://mystorage.blob.core.windows.net/mycluster/example/data/gutenberg/ /dest swebhdfs://mydatalakestore.azuredatalakestore.net/mynewfolder/ /sourcekey uJUfvD6cEvhfLoBae2yyQf8t9/BpbWZ4XoYj4kAS5Jf40pZaMNf0q6a8yqTxktwVgRED4vPHeh/50iS9atS5LQ== /Account mydatalakeanalyticaccount /Units 2


Op dezelfde manier, voer de volgende opdracht uit een opslag van Azure blob kopiëren naar een Lake gegevensopslag-account via gegevens Lake Analytics-account:

    AdlCopy /Source adl://mysourcedatalakestore.azuredatalakestore.net/mynewfolder/ /dest adl://mydestdatastore.azuredatalakestore.net/mynewfolder/ /Account mydatalakeanalyticaccount /Units 2

## <a name="use-adlcopy-to-copy-data-using-pattern-matching"></a>AdlCopy gebruiken om te kopiëren van gegevens met behulp van jokertekens

In dit gedeelte leert u hoe u AdlCopy gebruiken om gegevens te kopiëren van een bron (we gebruiken in het voorbeeld hieronder Azure opslag Blob) naar een bestemming Lake gegevensopslag account met behulp van jokertekens. U kunt bijvoorbeeld de onderstaande stappen kopiëren van alle bestanden met de extensie .csv van het bron-blob naar de bestemming.

1. Open een opdrachtprompt en navigeer naar de map waarin AdlCopy is geïnstalleerd, meestal `%HOMEPATH%\Documents\adlcopy`.

2. Voer de volgende opdracht om te kopiëren van alle bestanden met de extensie *.csv van een specifieke blob uit de Broncontainer naar een gegevensopslag Lake:

        AdlCopy /source https://<source_account>.blob.core.windows.net/<source_container>/<blob name> /dest swebhdfs://<dest_adls_account>.azuredatalakestore.net/<dest_folder>/ /sourcekey <storage_account_key_for_storage_container> /Pattern *.csv

    Bijvoorbeeld:

        AdlCopy /source https://mystorage.blob.core.windows.net/mycluster/HdiSamples/HdiSamples/FoodInspectionData/ /dest adl://mydatalakestore.azuredatalakestore.net/mynewfolder/ /sourcekey uJUfvD6cEvhfLoBae2yyQf8t9/BpbWZ4XoYj4kAS5Jf40pZaMNf0q6a8yqTxktwVgRED4vPHeh/50iS9atS5LQ== /Pattern *.csv

    
## <a name="billing"></a>Facturering

* Als u het hulpprogramma AdlCopy als zelfstandig product gebruiken wordt u gefactureerd voor egress kosten voor het verplaatsen van gegevens, als de bron opslag van Azure-account niet in hetzelfde gebied, als de Lake gegevensopslag is.

* Als u het hulpmiddel AdlCopy met uw gegevens Lake Analytics-account gebruikt, worden standaard [gegevens Lake Analytics billing tarieven](https://azure.microsoft.com/pricing/details/data-lake-analytics/) toepassen.

## <a name="considerations-for-using-adlcopy"></a>Overwegingen bij het gebruik van AdlCopy

* AdlCopy (voor versie 1.0.5), ondersteunt kopiëren van gegevens uit de bronnen die gezamenlijk meer dan duizenden bestanden en mappen hebben. Echter als u problemen hebt met het kopiëren van een grote gegevensset, kunt u de bestanden/mappen in verschillende submappen distribueren en gebruiken het pad naar deze submappen als bron in plaats daarvan.

## <a name="next-steps"></a>Volgende stappen

- [Beveiliging van gegevens in Lake gegevensopslag](data-lake-store-secure-data.md)
- [Azure gegevens Lake Analytics gebruiken met Lake gegevensopslag](../data-lake-analytics/data-lake-analytics-get-started-portal.md)
- [Azure HDInsight gebruiken met Lake gegevensopslag](data-lake-store-hdinsight-hadoop-use-portal.md)
