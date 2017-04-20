<properties
   pageTitle="Aan de slag met Lake gegevensopslag met meerdere platforms opdracht lijn interface | Microsoft Azure"
   description="Azure platforms-opdrachtregel gebruiken om te maken van een account voor gegevensopslag Lake en eenvoudige bewerkingen uitvoeren"
   services="data-lake-store"
   documentationCenter=""
   authors="nitinme"
   manager="jhubbard"
   editor="cgronlun"/>

<tags
   ms.service="data-lake-store"
   ms.devlang="na"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data"
   ms.date="09/27/2016"
   ms.author="nitinme"/>

# <a name="get-started-with-azure-data-lake-store-using-azure-command-line"></a>Aan de slag met Azure Lake gegevensopslag via Azure-opdrachtregel

> [AZURE.SELECTOR]
- [Portal](data-lake-store-get-started-portal.md)
- [PowerShell](data-lake-store-get-started-powershell.md)
- [.NET SDK](data-lake-store-get-started-net-sdk.md)
- [Java SDK](data-lake-store-get-started-java-sdk.md)
- [REST API](data-lake-store-get-started-rest-api.md)
- [Azure CLI](data-lake-store-get-started-cli.md)
- [Node.js](data-lake-store-manage-use-nodejs.md)

Informatie over het gebruiken van Azure opdracht lijn interface een Azure Lake gegevensopslag-account maken en uitvoeren van basisbewerkingen bijvoorbeeld maakt mappen, gegevensbestanden uploaden en downloaden en verwijderen van uw account, enzovoort. Zie voor meer informatie over gegevensopslag Lake, [Overzicht van Lake gegevensopslag](data-lake-store-overview.md).

De CLI Azure is geïmplementeerd in Node.js. Dit kan worden gebruikt op een willekeurig platform die ondersteuning biedt voor Node.js, met inbegrip van Windows, Mac en Linux. De CLI Azure is bron openen. De broncode wordt beheerd in GitHub bij <a href= "https://github.com/azure/azure-xplat-cli">https://github.com/azure/azure-xplat-cli</a>. In dit artikel beschreven hoe u met alleen de CLI Azure met Lake gegevensopslag. Zie voor een algemene gids over het gebruik van Azure CLI, [het gebruik van de Azure CLI] [azure-command-line-tools].


##<a name="prerequisites"></a>Vereisten voor

Voordat u in dit artikel begint, hebt u het volgende:

- **Een Azure-abonnement**. Zie [Azure krijgen gratis proefversie](https://azure.microsoft.com/pricing/free-trial/).

- **Azure CLI** - Zie [installeren en configureren van de Azure CLI](../xplat-cli-install.md) voor installatie en configuratie. Zorg ervoor dat u de computer opnieuw opstarten nadat u de CLI hebt geïnstalleerd.

## <a name="authentication"></a>Verificatie

In dit artikel wordt het eenvoudiger verificatie gebruikt met Lake gegevensopslag, waar u zich als een gebruiker eindgebruikers aanmelden. Het toegangsniveau voor gegevensopslag van Lake account en een nieuw bestandssysteem vervolgens onder het toegangsniveau van de gebruiker is aangemeld. Er zijn echter andere methoden ook om te verifiëren met Lake gegevensopslag, welke **eindgebruikers** of **service-naar-service-verificatie**. Zie voor instructies en meer informatie over het om te verifiëren, [verifiëren met Lake gegevensopslag Azure Active Directory](data-lake-store-authenticate-using-active-directory.md).

##<a name="login-to-your-azure-subscription"></a>Meld u aan bij uw Azure-abonnement

1. Volg de stappen beschreven in [verbinding maken met een Azure abonnement vanaf de opdrachtregel van Azure (Azure CLI)](../xplat-cli-connect.md) en verbinden met uw abonnement via de `azure login` methode.

2. Lijst van de abonnementen die zijn gekoppeld aan uw account gebruiken de `azure account list` opdracht.

        info:    Executing command account list
        data:    Name              Id                                    Current
        data:    ----------------  ------------------------------------  -------
        data:    Azure-sub-1       ####################################  true
        data:    Azure-sub-2       ####################################  false

    Uit de bovenstaande uitvoer **Azure-sub-1** is ingeschakeld en het andere abonnement is **Azure-sub-2**. 

3. Selecteer het abonnement dat u werken wilt onder. Als u werken onder het Azure-sub-2-abonnement wilt, gebruikt u de `azure account set` opdracht.

        azure account set Azure-sub-2


## <a name="create-an-azure-data-lake-store-account"></a>Maak een account Azure Lake gegevensopslag

Open een opdrachtprompt, shell of een sessie en voer de volgende opdrachten.

2. Overschakelen naar resourcemanager Azure-modus met de volgende opdracht:

        azure config mode arm


5. Maak een nieuwe resourcegroep. Geef in de volgende opdracht uit, de parameterwaarden die u wilt gebruiken.

        azure group create <resourceGroup> <location>

    Als de naam van de locatie spaties bevat, kunt u deze tussen aanhalingstekens geplaatst. Bijvoorbeeld "Oost US 2'.

5. Maak de Lake gegevensopslag-account.

        azure datalake store account create <dataLakeStoreAccountName> <location> <resourceGroup>

## <a name="create-folders-in-your-data-lake-store"></a>Mappen maken in uw Lake gegevensopslag

U kunt mappen maken bij uw account Azure Lake gegevensopslag om te beheren en opslag van gegevens. Gebruik de volgende opdracht uit om een map 'mynewfolder' genoemd in de hoofdmap van de Lake gegevensopslag te maken.

    azure datalake store filesystem create <dataLakeStoreAccountName> <path> --folder

Bijvoorbeeld:

    azure datalake store filesystem create mynewdatalakestore /mynewfolder --folder

## <a name="upload-data-to-your-data-lake-store"></a>Gegevens uploaden naar uw Lake gegevensopslag

U kunt uw gegevens uploaden naar Lake gegevensopslag rechtstreeks op het hoogste niveau of naar een map die u hebt gemaakt in het account. De onderstaande fragmenten demonstreren het uploaden van enkele voorbeeldgegevens naar de map (**mynewfolder**) die u in de vorige sectie hebt gemaakt.

Als u enkele voorbeeldgegevens uploaden zoekt, kunt u de map **Ambulances gegevens** krijgen van de [Azure gegevens Lake cijfer opslagplaats](https://github.com/MicrosoftBigData/usql/tree/master/Examples/Samples/Data/AmbulanceData). Download het bestand en opslaan in een lokale map op uw computer op, zoals C:\sampledata\.

    azure datalake store filesystem import <dataLakeStoreAccountName> "<source path>" "<destination path>"

Bijvoorbeeld:

    azure datalake store filesystem import mynewdatalakestore "C:\SampleData\AmbulanceData\vehicle1_09142014.csv" "/mynewfolder/vehicle1_09142014.csv"


## <a name="list-files-in-data-lake-store"></a>Lijst met bestanden in Lake gegevensopslag

Gebruik de volgende opdracht uit voor een overzicht van de bestanden in een account voor gegevensopslag Lake.

    azure datalake store filesystem list <dataLakeStoreAccountName> <path>

Bijvoorbeeld:

    azure datalake store filesystem list mynewdatalakestore /mynewfolder

De uitvoer van deze moet er ongeveer als volgt te werk:

    info:    Executing command datalake store filesystem list
    data:    accessTime: 1446245025257
    data:    blockSize: 268435456
    data:    group: NotSupportYet
    data:    length: 1589881
    data:    modificationTime: 1446245105763
    data:    owner: NotSupportYet
    data:    pathSuffix: vehicle1_09142014.csv
    data:    permission: 777
    data:    replication: 0
    data:    type: FILE
    data:    ------------------------------------------------------------------------------------
    info:    datalake store filesystem list command OK

## <a name="rename-download-and-delete-data-from-your-data-lake-store"></a>Naam en gegevens verwijderen uit uw Lake gegevensopslag downloaden

* **Om een bestand te wijzigen**, gebruikt u de volgende opdracht:

        azure datalake store filesystem move <dataLakeStoreAccountName> <path/old_file_name> <path/new_file_name>

    Bijvoorbeeld:

        azure datalake store filesystem move mynewdatalakestore /mynewfolder/vehicle1_09142014.csv /mynewfolder/vehicle1_09142014_copy.csv

* **Een bestand wilt downloaden**, gebruikt u de volgende opdracht uit. Zorg ervoor dat de bestemmingspad dat u al bestaat.

        azure datalake store filesystem export <dataLakeStoreAccountName> <source_path> <destination_path>

    Bijvoorbeeld:

        azure datalake store filesystem export mynewdatalakestore /mynewfolder/vehicle1_09142014_copy.csv "C:\mysampledata\vehicle1_09142014_copy.csv"

* **Een bestand te verwijderen**, gebruikt u de volgende opdracht:

        azure datalake store filesystem delete <dataLakeStoreAccountName> <path>

    Bijvoorbeeld:

        azure datalake store filesystem delete mynewdatalakestore /mynewfolder/vehicle1_09142014_copy.csv

    Wanneer u wordt gevraagd, voert u **Y** om het item te verwijderen.

## <a name="view-the-access-control-list-for-a-folder-in-data-lake-store"></a>De toegangsbeheerlijst voor een map weergeven in Lake gegevensopslag

Gebruik de volgende opdracht uit om weer te geven van de ACL's op een map Lake gegevensopslag. In de huidige versie kunnen ACL's alleen worden ingesteld op de hoofdsite van de Lake gegevensopslag. Zo is, zijn de parameter path onderstaande altijd hoofdmap (/).

    azure datalake store permissions show <dataLakeStoreName> <path>

Bijvoorbeeld:

    azure datalake store permissions show mynewdatalakestore /


## <a name="delete-your-data-lake-store-account"></a>Uw account voor gegevensopslag Lake verwijderen

Gebruik de volgende opdracht uit een account voor gegevensopslag Lake verwijderen.

    azure datalake store account delete <dataLakeStoreAccountName>

Bijvoorbeeld:

    azure datalake store account delete mynewdatalakestore

Wanneer u wordt gevraagd, typt u **Y** als het account wilt verwijderen.


## <a name="next-steps"></a>Volgende stappen

- [Beveiliging van gegevens in Lake gegevensopslag](data-lake-store-secure-data.md)
- [Azure gegevens Lake Analytics gebruiken met Lake gegevensopslag](../data-lake-analytics/data-lake-analytics-get-started-portal.md)
- [Azure HDInsight gebruiken met Lake gegevensopslag](data-lake-store-hdinsight-hadoop-use-portal.md)


[azure-command-line-tools]: ../xplat-cli-install.md
