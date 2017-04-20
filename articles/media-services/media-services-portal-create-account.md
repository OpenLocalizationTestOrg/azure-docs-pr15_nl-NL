<properties
    pageTitle=" Maak een Azure Media Services-account in de portal van Azure | Microsoft Azure"
    description="Deze zelfstudie leert u de stappen voor het maken van een Azure Media Services-account in de portal van Azure."
    services="media-services"
    documentationCenter=""
    authors="Juliako"
    manager="erikre"
    editor=""/>

<tags
    ms.service="media-services"
    ms.workload="media"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="10/24/2016"
    ms.author="juliako"/>


# <a name="create-an-azure-media-services-account-using-the-azure-portal"></a>Maak een Azure Media Services-account met behulp van de Azure portal

> [AZURE.SELECTOR]
- [Portal](media-services-portal-create-account.md)
- [PowerShell](media-services-manage-with-powershell.md)
- [REST](http://msdn.microsoft.com/library/azure/dn194267.aspx)

> [AZURE.NOTE] Als u wilt deze zelfstudie hebt voltooid, moet u een Azure-account. Zie [Azure gratis proefversie](https://azure.microsoft.com/pricing/free-trial/)voor meer informatie. 

De Azure portal biedt een manier om snel een Azure Media Services (AMS)-account maken. U kunt uw account gebruiken voor toegang tot Media-Services waarmee u kunt opslaan, versleutelen, coderen, beheren en streamen media-inhoud in Azure wordt aangegeven. Tijdens het maken van een Media Services-account, u ook een gekoppeld opslag-account maken (of gebruik een bestaande) in hetzelfde geografische gebied, als de Media Services-account.

In dit artikel worden enkele veelvoorkomende concepten uitgelegd en ziet u hoe u een Media Services-account maken met de portal van Azure.

## <a name="concepts"></a>Concepten

Toegang krijgen tot het Media Services vereist twee gekoppelde accounts:

- Een Media Services-account. Uw account hebt u toegang tot een set met cloudgebaseerde Media Services die beschikbaar in Azure zijn. Een Media Services-account slaat geen werkelijke media-inhoud. In plaats daarvan deze metagegevens over de mediainhoud en media verwerking van taken opgeslagen in uw account. Op het moment dat u een account maakt, moet u een beschikbare Media Services regio selecteren. Het gebied dat u selecteert is een datacenter dat de metagegevens-records voor uw account worden opgeslagen.

    Beschikbare Media Services (AMS) regio's bevatten de volgende handelingen uit: Noord Europa, West Europa, West Amerikaans, Oost Amerikaans, Zuidoost-Azië, Oost-Azië, Japan West Japan Oost. Media Services gebruikt geen affiniteit groepen.
    
    AMS is nu ook beschikbaar in de volgende datacenters: Brazilië Zuid, India West India Zuid en India centraal. U kunt nu de portal van Azure Media-Service-accounts maken en verschillende taken die hier worden beschreven. Live-codering is niet ingeschakeld in deze datacenters. Verder, zijn niet alle typen gereserveerde eenheden codering beschikbaar in deze datacenters.
    
    - Brazilië Zuid: Alleen standaard en eenvoudige codering gereserveerde eenheden beschikbaar zijn.
    - India West India Zuid: 

- Een account Azure opslag. Opslag accounts moeten zich bevinden in hetzelfde geografische gebied, als de Media Services-account. Wanneer u een Media Services-account hebt gemaakt, kunt u een bestaand opslag-account kiezen in dezelfde regio of kunt u een nieuw account voor de opslag in dezelfde regio. Als u een Media Services-account verwijdert, worden de BLOB's in uw account gerelateerde opslag niet verwijderd.

## <a name="create-an-ams-account"></a>Maak een account AMS

De stappen in deze sectie laten zien hoe een AMS-account maken.

1. Meld u aan bij de [portal van Azure](https://portal.azure.com/).
2. Klik op **+ nieuwe** > **Web + Mobile** > **mediaservices**.

    ![Mediaservices maken](./media/media-services-portal-vod-get-started/media-services-new1.png)

3. Voer de vereiste waarden in **MEDIA SERVICES-ACCOUNT maken** .

    ![Mediaservices maken](./media/media-services-portal-vod-get-started/media-services-new3.png)
    
    1. Voer de naam van de nieuwe AMS-account in het vak **Naam van het Account**. De naam van een Media Services-account is alle kleine letters cijfers of letters zonder spaties, en is 3 tot en met 24 tekens.
    2. Selecteer in abonnement, tussen de verschillende Azure abonnementen waartoe u toegang hebt.
    
    2. Selecteer de nieuwe of bestaande resource in **Resourcegroep**.  Een resourcegroep is een verzameling resources die levenscyclus, machtigingen en beleid delen. Meer informatie [hier](azure-resource-manager/resource-group-overview.md#resource-groups).
    3. Selecteer in de **locatie**, de geografische regio die wordt gebruikt voor de opslag van de media en metagegevens records voor uw Media Services-account. Dit gebied wordt gebruikt om te verwerken en media streamen. Alleen de beschikbare Media Services regio's weergegeven in het vak van de vervolgkeuzelijst. 
    
    3. Selecteer in de **Opslag-Account**, een opslag-account om u te bieden-blobopslag van het media-inhoud van uw Media Services-account. U kunt een bestaand account voor de opslag in hetzelfde geografische gebied, als uw account Media Services selecteren of kunt u een opslag-account maken. Een nieuwe opslag-account is gemaakt in dezelfde regio. De regels voor opslag-accountnamen zijn hetzelfde als voor Media Services-accounts.

        Meer informatie over de opslag [hier](storage-introduction.md).

    4. Selecteer **vastmaken aan dashboard** om te zien van de voortgang van de account-implementatie.
    
7. Klik op **maken** onder aan het formulier.

    Zodra het account is gemaakt, verandert de status **uitgevoerd**. 

    ![Media Services-instellingen](./media/media-services-portal-vod-get-started/media-services-settings.png)

    Voor het beheren van uw account AMS (bijvoorbeeld, video's uploaden, coderen van activa, taakvoortgang bewaken) Gebruik het venster **Instellingen** .

## <a name="manage-keys"></a>Toetsen beheren

Moet u de naam van het account en de primaire sleutel gegevens via programmacode toegang tot de Media Services-account.

1. Selecteer uw account in de portal Azure. 

    Het venster **Instellingen** wordt weergegeven aan de rechterkant. 

2. Selecteer in het venster **instellingen van** **toetsen**. 

    De vensters **sleutels beheren** ziet u de naam van het account en de primaire en secundaire sleutels wordt weergegeven. 
3. Druk op de knop kopiëren om de waarden te kopiëren.
    
    ![Media Services-toetsen](./media/media-services-portal-vod-get-started/media-services-keys.png)

## <a name="next-steps"></a>Volgende stappen

U kunt nu bestanden uploaden naar uw account AMS. Zie [bestanden uploaden](media-services-portal-upload-files.md)voor meer informatie.

## <a name="media-services-learning-paths"></a>Media Services leerpaden

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Feedback geven

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]


