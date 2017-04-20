<properties 
   pageTitle="Aan de slag met Lake gegevensopslag | Azure" 
   description="Gebruik van de portal maken van een account voor gegevensopslag Lake en in de gegevensopslag Lake eenvoudige bewerkingen uitvoeren" 
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
   ms.date="09/13/2016"
   ms.author="nitinme"/>

# <a name="get-started-with-azure-data-lake-store-using-the-azure-portal"></a>Aan de slag met Azure Lake gegevensopslag met behulp van de Azure-Portal

> [AZURE.SELECTOR]
- [Portal](data-lake-store-get-started-portal.md)
- [PowerShell](data-lake-store-get-started-powershell.md)
- [.NET SDK](data-lake-store-get-started-net-sdk.md)
- [Java SDK](data-lake-store-get-started-java-sdk.md)
- [REST API](data-lake-store-get-started-rest-api.md)
- [Azure CLI](data-lake-store-get-started-cli.md)
- [Node.js](data-lake-store-manage-use-nodejs.md)

Informatie over het gebruiken van de Azure-Portal een Azure Lake gegevensopslag-account maken en uitvoeren van basisbewerkingen bijvoorbeeld maakt mappen, gegevensbestanden uploaden en downloaden en verwijderen van uw account, enzovoort. Zie voor meer informatie over gegevensopslag Lake, [Overzicht van Azure Lake gegevensopslag](data-lake-store-overview.md).

## <a name="prerequisites"></a>Vereisten voor

Voordat u deze zelfstudie begint, hebt u het volgende:

- **Een Azure-abonnement**. Zie [Azure krijgen gratis proefversie](https://azure.microsoft.com/pricing/free-trial/).

## <a name="do-you-learn-fast-with-videos"></a>Vind u meer snel met video's?

Bekijk de volgende video's om het aan de slag met Lake gegevensopslag.

* [Een account voor gegevensopslag Lake maken](https://mix.office.com/watch/1k1cycy4l4gen)
* [Gegevens in Lake gegevensopslag via de Verkenner gegevens beheren](https://mix.office.com/watch/icletrxrh6pc)

## <a name="create-an-azure-data-lake-store-account"></a>Maak een account Azure Lake gegevensopslag

1. Aanmelden bij de nieuwe [Azure-Portal](https://portal.azure.com).

2. Klik op **Nieuw** **gegevens + opslagruimte**op en klik vervolgens op **Azure Lake gegevensopslag**. Lees de informatie in het blad **Azure Lake gegevensopslag** en klik vervolgens op **maken** in de linkerbenedenhoek van het blad.

3. Geef in het blad **Nieuw Lake gegevensopslag** de waarden zoals weergegeven in de onderstaande schermopname:

    ![Een nieuwe gegevensopslag voor Lake Azure-account maken] (./media/data-lake-store-get-started-portal/ADL.Create.New.Account.png "Een nieuwe Azure gegevens Lake-account maken")

    - **Abonnement**. Selecteer het abonnement waaronder dat u wilt maken van een nieuw account voor gegevensopslag Lake.
    - **Resourcegroep**. Selecteer een bestaande resourcegroep of klik op **een resourcegroep maken** om een account maakt. Een resourcegroep is een container met verwante resources voor een toepassing. Zie voor meer informatie [Resourcegroepen in Azure wordt aangegeven](azure-resource-manager/resource-group-overview.md#resource-groups).
    - **Locatie**: Selecteer een locatie waar u het account voor gegevensopslag Lake maken.

4. Selecteer **vastmaken aan Startboard** desgewenst kunt u het account voor gegevensopslag Lake moeten toegankelijk zijn vanuit de Startboard.

5. Klik op **maken**. Als u ervoor kiest om een opslaglocatie van het account naar de startboard te, worden geklikt gaat u terug naar de startboard en ziet u de voortgang van uw Lake gegevensopslag voor het inrichten. Zodra het account voor gegevensopslag Lake is ingericht, ziet u het blad account omhoog.

6. Vouw de **Essentials** -omlaag om te zien dat de informatie over uw account voor gegevensopslag Lake, zoals de resource groepeer de informatie is een onderdeel van, de locatie, enzovoort. Klik op het pictogram **Snel starten** om te zien van koppelingen naar andere bronnen die betrekking hebben op Lake gegevensopslag.

    ![Uw Azure Lake gegevensopslag-account] (./media/data-lake-store-get-started-portal/ADL.Account.QuickStart.png "Uw Azure gegevens Lake-account")

## <a name="createfolder"></a>Mappen maken in de gegevensopslag Lake Azure-account

U kunt mappen maken bij uw account voor gegevensopslag Lake om te beheren en opslag van gegevens.

1. Open het Lake gegevensopslag-account dat u zojuist hebt gemaakt. In het linkerdeelvenster, klikt u op **Bladeren**, klikt u op **Meer gegevensopslag**en klik vervolgens op het blad Lake gegevensopslag op de naam van het account waarmee u wilt mappen maken. Als u het account naar de startboard vastgemaakt, klikt u op de tegel voor dat account.

2. Klik in uw account Lake gegevensopslag blade, op **Data Explorer**.

    ![Mappen maken in Lake gegevensopslag-account] (./media/data-lake-store-get-started-portal/ADL.Create.Folder.png "Mappen maken in Lake gegevensopslag-account")

3. Klik op **Nieuwe map**in uw account Lake gegevensopslag blade, voer een naam voor de nieuwe map en klik vervolgens op **OK**.
    
    ![Mappen maken in Lake gegevensopslag-account] (./media/data-lake-store-get-started-portal/ADL.Folder.Name.png "Mappen maken in Lake gegevensopslag-account")
    
    De nieuwe map worden, vermeld in het blad **Data Explorer** . U kunt geneste mappen tot elk gewenst niveau maken.

    ![Mappen maken in gegevens Lake-account] (./media/data-lake-store-get-started-portal/ADL.New.Directory.png "Mappen maken in gegevens Lake-account")


## <a name="uploaddata"></a>Gegevens uploaden naar de gegevensopslag Lake Azure-account

Bij een account Azure Lake gegevensopslag rechtstreeks op het hoogste niveau of naar een map die u in het account hebt gemaakt, kunt u uw gegevens uploaden. Volg de stappen om een bestand uploaden naar een submap van het **Data Explorer** -blad in de schermopname onder. In dit schermopname, wordt het bestand geüpload naar een submap die wordt weergegeven in de breadcrumbs (gemarkeerd in een rood vak).

Als u enkele voorbeeldgegevens uploaden zoekt, kunt u de map **Ambulances gegevens** krijgen van de [Azure gegevens Lake cijfer opslagplaats](https://github.com/MicrosoftBigData/usql/tree/master/Examples/Samples/Data/AmbulanceData).

![Gegevens uploaden] (./media/data-lake-store-get-started-portal/ADL.New.Upload.File.png "Gegevens uploaden")


## <a name="properties"></a>Eigenschappen en acties die beschikbaar zijn op de opgeslagen gegevens

Klik op het nieuw toegevoegde bestand te openen van het blad **Eigenschappen** . De eigenschappen van het bestand en de acties die u op het bestand uitvoeren kunt zijn beschikbaar in deze blade. U kunt ook het volledige pad kopiëren naar bestand in uw account Azure Lake gegevensopslag, gemarkeerd in het rood vak in de onderstaande schermopname.

![Eigenschappen van de gegevens] (./media/data-lake-store-get-started-portal/ADL.File.Properties.png "Eigenschappen van de gegevens")

* Klik op **voorbeeld** om te bekijken van het bestand, rechtstreeks vanuit de browser. U kunt de opmaak van de voorbeeldversie ook opgeven. Klik op **voorbeeld**, klik op **Opmaak** in het blad **PQ-update** en geef in het blad voor de **Preview-bestandsindeling** de opties zoals het aantal rijen wilt weergeven, codering moet worden gebruikt, een scheidingsteken kunt gebruiken, enzovoort.

  ![Voorbeeld-bestandsindeling] (./media/data-lake-store-get-started-portal/ADL.File.Preview.png "Voorbeeld-bestandsindeling")

* Klik op **Download** het bestand te downloaden naar uw computer.

* Klik op **de naam van bestand** de naam van het bestand.

* Klik op **bestand verwijderen** om het bestand te verwijderen.


## <a name="secure-your-data"></a>Uw gegevens beveiligen

U kunt de gegevens die zijn opgeslagen in uw Azure Lake gegevensopslag-account met Azure Active Directory en toegangsbeheer (ACL's) beveiligen. Zie voor instructies over hoe u dat doen, [gegevens beveiligen in Azure Lake gegevensopslag](data-lake-store-secure-data.md).


## <a name="delete-azure-data-lake-store-account"></a>Account verwijderen Azure Lake gegevensopslag

Als een account Azure Lake gegevensopslag, uit uw blade Lake gegevensopslag verwijderen, klikt u op **verwijderen**. Om te bevestigen, wordt u gevraagd om in te voeren, de naam van het account dat u wilt verwijderen. Voer de naam van het account en klik vervolgens op **verwijderen**.

![Delete gegevens Lake-account] (./media/data-lake-store-get-started-portal/ADL.Delete.Account.png "Delete gegevens Lake-account")


## <a name="next-steps"></a>Volgende stappen

- [Beveiliging van gegevens in Lake gegevensopslag](data-lake-store-secure-data.md)
- [Azure gegevens Lake Analytics gebruiken met Lake gegevensopslag](../data-lake-analytics/data-lake-analytics-get-started-portal.md)
- [Azure HDInsight gebruiken met Lake gegevensopslag](data-lake-store-hdinsight-hadoop-use-portal.md)
- [Diagnostische logboeken toegang voor gegevensopslag Lake](data-lake-store-diagnostic-logs.md)
