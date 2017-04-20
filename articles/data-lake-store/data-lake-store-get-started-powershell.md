<properties
   pageTitle="Aan de slag met Lake gegevensopslag | Azure"
   description="Azure PowerShell gebruiken om te maken van een account voor gegevensopslag Lake en eenvoudige bewerkingen uitvoeren"
   services="data-lake-store"
   documentationCenter=""
   authors="nitinme"
   manager="jhubbard"
   editor="cgronlun"/>

<tags
   ms.service="data-lake-store"
   ms.devlang="na"
   ms.topic="hero-article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data"
   ms.date="10/04/2016"
   ms.author="nitinme"/>

# <a name="get-started-with-azure-data-lake-store-using-azure-powershell"></a>Aan de slag met Azure Lake gegevensopslag via Azure PowerShell

> [AZURE.SELECTOR]
- [Portal](data-lake-store-get-started-portal.md)
- [PowerShell](data-lake-store-get-started-powershell.md)
- [.NET SDK](data-lake-store-get-started-net-sdk.md)
- [Java SDK](data-lake-store-get-started-java-sdk.md)
- [REST API](data-lake-store-get-started-rest-api.md)
- [Azure CLI](data-lake-store-get-started-cli.md)
- [Node.js](data-lake-store-manage-use-nodejs.md)

Leer hoe u Azure PowerShell gebruiken om te maken van een account Azure Lake gegevensopslag en daar basisbewerkingen bijvoorbeeld maakt mappen, gegevensbestanden uploaden en downloaden en verwijderen van uw account, enzovoort. Zie voor meer informatie over gegevensopslag Lake, [Overzicht van Lake gegevensopslag](data-lake-store-overview.md).

## <a name="prerequisites"></a>Vereisten voor

Voordat u deze zelfstudie begint, hebt u het volgende:

* **Een Azure-abonnement**. Zie [Azure krijgen gratis proefversie](https://azure.microsoft.com/pricing/free-trial/).

* **Azure PowerShell 1.0 of groter**. Lees [hoe u installeren en configureren van Azure PowerShell](../powershell-install-configure.md).

## <a name="authentication"></a>Verificatie

In dit artikel wordt het eenvoudiger verificatie met Lake gegevensopslag, waarbij u wordt gevraagd om uw Azure-account gebruikt. Het toegangsniveau voor gegevensopslag van Lake account en een nieuw bestandssysteem vervolgens onder het toegangsniveau van de gebruiker is aangemeld. Er zijn echter andere methoden ook om te verifiëren met Lake gegevensopslag, welke **eindgebruikers** of **service-naar-service-verificatie**. Zie voor instructies en meer informatie over het om te verifiëren, [verifiëren met Lake gegevensopslag Azure Active Directory](data-lake-store-authenticate-using-active-directory.md).

## <a name="create-an-azure-data-lake-store-account"></a>Maak een account Azure Lake gegevensopslag

1. Een nieuw Windows PowerShell-venster openen vanaf uw bureaublad, en voer de volgende fragment Meld u aan bij uw Azure-account en de provider Lake gegevensopslag registreren het abonnement instellen. Wanneer u wordt gevraagd aan te melden, zorg er dan voor dat u zich hebt aangemeld als een van de admininistrators/eigenaar van het abonnement:

        # Log in to your Azure account
        Login-AzureRmAccount

        # List all the subscriptions associated to your account
        Get-AzureRmSubscription

        # Select a subscription
        Set-AzureRmContext -SubscriptionId <subscription ID>

        # Register for Azure Data Lake Store
        Register-AzureRmResourceProvider -ProviderNamespace "Microsoft.DataLakeStore"


2. Een account Azure Lake gegevensopslag is gekoppeld aan een resourcegroep Azure. Beginnen met het maken van een resourcegroep Azure.

        $resourceGroupName = "<your new resource group name>"
        New-AzureRmResourceGroup -Name $resourceGroupName -Location "East US 2"

    ![Een resourcegroep Azure maken] (./media/data-lake-store-get-started-powershell/ADL.PS.CreateResourceGroup.png "Een resourcegroep Azure maken")

2. Maak een account Azure Lake gegevensopslag. De naam die u opgeeft moet alleen kleine letters en cijfers bevatten.

        $dataLakeStoreName = "<your new Data Lake Store name>"
        New-AzureRmDataLakeStoreAccount -ResourceGroupName $resourceGroupName -Name $dataLakeStoreName -Location "East US 2"

    ![Een account Azure Lake gegevensopslag maken] (./media/data-lake-store-get-started-powershell/ADL.PS.CreateADLAcc.png "Een account Azure Lake gegevensopslag maken")

3. Controleer of het account is gemaakt.

        Test-AzureRmDataLakeStoreAccount -Name $dataLakeStoreName

    De uitvoer hiervoor moet **waar**zijn.

## <a name="create-directory-structures-in-your-azure-data-lake-store"></a>Mapstructuren maken in uw Lake van Azure gegevensopslag

U kunt mappen maken bij uw account Azure Lake gegevensopslag om te beheren en opslag van gegevens.

1. Geef een hoofdmap.

        $myrootdir = "/"

2. Maak een nieuwe map genaamd **mynewdirectory** onder het opgegeven toegangspunt.

        New-AzureRmDataLakeStoreItem -Folder -AccountName $dataLakeStoreName -Path $myrootdir/mynewdirectory

3. Controleer of de nieuwe map is gemaakt.

        Get-AzureRmDataLakeStoreChildItem -AccountName $dataLakeStoreName -Path $myrootdir

    Dit moet een uitvoer als volgt weergegeven:

    ![Controleer of de map] (./media/data-lake-store-get-started-powershell/ADL.PS.Verify.Dir.Creation.png "Controleer of de map")


## <a name="upload-data-to-your-azure-data-lake-store"></a>Gegevens uploaden naar uw Lake van Azure gegevensopslag

U kunt uw gegevens uploaden naar Lake gegevensopslag rechtstreeks op het hoogste niveau of naar een map die u hebt gemaakt in het account. De onderstaande fragmenten demonstreren het uploaden van enkele voorbeeldgegevens naar de map (**mynewdirectory**) die u in de vorige sectie hebt gemaakt.

Als u enkele voorbeeldgegevens uploaden zoekt, kunt u de map **Ambulances gegevens** krijgen van de [Azure gegevens Lake cijfer opslagplaats](https://github.com/MicrosoftBigData/usql/tree/master/Examples/Samples/Data/AmbulanceData). Download het bestand en opslaan in een lokale map op uw computer op, zoals C:\sampledata\.

    Import-AzureRmDataLakeStoreItem -AccountName $dataLakeStoreName -Path "C:\sampledata\vehicle1_09142014.csv" -Destination $myrootdir\mynewdirectory\vehicle1_09142014.csv


## <a name="rename-download-and-delete-data-from-your-data-lake-store"></a>Naam en gegevens verwijderen uit uw Lake gegevensopslag downloaden

De naam van een bestand, gebruikt u de volgende opdracht:

    Move-AzureRmDataLakeStoreItem -AccountName $dataLakeStoreName -Path $myrootdir\mynewdirectory\vehicle1_09142014.csv -Destination $myrootdir\mynewdirectory\vehicle1_09142014_Copy.csv

Gebruik de volgende opdracht uit een bestand te downloaden:

    Export-AzureRmDataLakeStoreItem -AccountName $dataLakeStoreName -Path $myrootdir\mynewdirectory\vehicle1_09142014_Copy.csv -Destination "C:\sampledata\vehicle1_09142014_Copy.csv"

Als u wilt een bestand verwijderen, gebruikt u de volgende opdracht:

    Remove-AzureRmDataLakeStoreItem -AccountName $dataLakeStoreName -Paths $myrootdir\mynewdirectory\vehicle1_09142014_Copy.csv

Wanneer u wordt gevraagd, voert u **Y** om het item te verwijderen. Als er meer dan één bestand wilt verwijderen, kunt u alle paden gescheiden door komma opgeven.

    Remove-AzureRmDataLakeStoreItem -AccountName $dataLakeStoreName -Paths $myrootdir\mynewdirectory\vehicle1_09142014.csv, $myrootdir\mynewdirectoryvehicle1_09142014_Copy.csv

## <a name="delete-your-azure-data-lake-store-account"></a>Uw account Azure Lake gegevensopslag verwijderen

Gebruik de volgende opdracht uit uw account voor gegevensopslag Lake te verwijderen.

    Remove-AzureRmDataLakeStoreAccount -Name $dataLakeStoreName

Wanneer u wordt gevraagd, typt u **Y** als het account wilt verwijderen.


## <a name="next-steps"></a>Volgende stappen

- [Beveiliging van gegevens in Lake gegevensopslag](data-lake-store-secure-data.md)
- [Azure gegevens Lake Analytics gebruiken met Lake gegevensopslag](../data-lake-analytics/data-lake-analytics-get-started-portal.md)
- [Azure HDInsight gebruiken met Lake gegevensopslag](data-lake-store-hdinsight-hadoop-use-portal.md)
