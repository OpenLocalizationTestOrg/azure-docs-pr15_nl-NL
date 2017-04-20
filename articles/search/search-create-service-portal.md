<properties
    pageTitle="Maak een zoekopdracht Azure-service met behulp van de Portal Azure | Microsoft Azure | De zoekservice gehoste cloud"
    description="Leer hoe u een Azure-zoekservice met behulp van de Portal Azure inrichten."
    services="search"
    manager="jhubbard"
    authors="ashmaka"
    documentationCenter=""/>

<tags
    ms.service="search"
    ms.devlang="NA"
    ms.workload="search"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.date="08/29/2016"
    ms.author="ashmaka"/>

# <a name="create-an-azure-search-service-using-the-azure-portal"></a>Maak een zoekopdracht Azure-service met behulp van de Azure-Portal

Deze handleiding helpt u bij het het proces voor het maken van (of inrichten) een Azure-zoekservice met behulp van de [Azure-Portal](https://portal.azure.com/).

Deze handleiding wordt ervan uitgegaan dat u al een Azure-abonnement hebt en kunt Meld u aan bij de Portal Azure.

## <a name="find-azure-search-in-the-azure-portal"></a>Azure zoeken te vinden in de Portal van Azure
1. Ga naar de [Azure-Portal](https://portal.azure.com/) en meld u aan.
1. Klik op het plus-teken ('en ') in de linkerbovenhoek.
2. Selecteer **gegevens + opslagruimte**.
3. Selecteer **Azure zoeken**.

![](./media/search-create-service-portal/find-search.png)

## <a name="pick-a-service-name-and-url-endpoint-for-your-service"></a>Kies een servicenaam en het eindpunt van de URL voor uw service
1. De servicenaam van uw wordt deel uitmaken van uw Azure zoekservice van eindpunt URL die wordt u hoe u uw gesprekken API voor het beheren en het gebruik van de zoekservice.
2. Typ de servicenaam van uw in het veld **URL** . De naam van de service:
  * alleen moet bevatten kleine letters, cijfers of streepjes ("-")
  * een streepje niet gebruiken ("-") als de eerst 2 tekens of de laatste willekeurig teken
  * mogen geen opeenvolgende streepjes bevatten (";")
  * beperkte tussen 2 en 60 tekens lang is


## <a name="select-a-subscription-where-you-will-keep-your-service"></a>Selecteer een abonnement waar u uw service wordt behouden
Als u meer dan één abonnement hebt, kunt u selecteren welke monitor bevat deze Azure Search-service.

## <a name="select-a-resource-group-for-your-service"></a>Selecteer een resourcegroep voor uw service
Een nieuwe resourcegroep maken of selecteren van een bestaande eigenschap. Een resourcegroep is een verzameling Azure services en resources die samen worden gebruikt. Bijvoorbeeld als u Azure Search indexeren van een SQL-database gebruikt, moeten klikt u vervolgens beide services deel uitmaken van dezelfde resourcegroep.

## <a name="select-the-location-where-your-service-will-be-hosted"></a>Selecteer de locatie waar u uw service wordt gehost
Als een Azure-service is Azure zoeken beschikbaar voor het in datacenters overal ter wereld worden gehost. Houd er rekening mee dat [prijzen kunnen verschillen](https://azure.microsoft.com/pricing/details/search/) per Geografie.

## <a name="select-your-pricing-tier"></a>Selecteer uw prijzen laag
[Azure zoeken is momenteel verkrijgbaar in meerdere prijzen lagen](https://azure.microsoft.com/pricing/details/search/): gratis, Basic of standaard. Elke laag heeft een eigen [capaciteit en -limieten](search-limits-quotas-capacity.md). Zie [kiezen een prijzen laag of SKU](search-sku-tier.md) voor instructies.

In dit geval hebt we de standaard laag voor onze service gekozen.

## <a name="select-the-create-button-to-provision-your-service"></a>Selecteer de knop 'Maken' voor het inrichten van uw service

![](./media/search-create-service-portal/create-service.png)

## <a name="scale-your-service"></a>De schaal van uw service aanpassen

Nadat uw service wordt ingericht, kunt u deze aan uw wensen schalen. Als u de standaard laag voor de zoekservice van Azure hebt gekozen, kunt u uw service schaal in twee dimensies: replica's en partities. Als u de eenvoudige laag hebt gekozen, kunt u alleen replica's toevoegen.

*__Partities__* toestaan uw service om te slaan en te doorzoeken meer documenten.

*__Replica's__* toestaan uw service u omgaat met een hogere belasting van zoekopdrachten - [een service vereist 2 replica's een alleen-lezen SLA bereiken en 3 replica's een alleen-lezen/schrijven SLA bereiken vereist](https://azure.microsoft.com/support/legal/sla/search/v1_0/).

1. Ga naar uw Azure zoekservice van management blade in de Portal Azure.
2. Selecteer in het blad **Instellingen** **schaal**.
3. U kunt uw service schalen door toe te voegen replica's of partities.
  * U kunt geen uw service eerdere 36 Zoeken eenheden schalen. Het totale aantal eenheden zoeken is het product van uw replica's en partities (replica's * partities = totale Zoeken eenheden).
  * Als u de eenvoudige laag hebt gekozen, kunt u alleen schalen met 3 replica's. Eenvoudige services zijn gekoppeld aan een enkele partition.

![](./media/search-create-service-portal/scale-service.png)

## <a name="next"></a>Volgende
Nadat een Azure Search-service is geïnstalleerd, bent u klaar om te [definiëren van een Azure zoekindex](search-what-is-an-index.md) zodat u kunt uploaden en uw gegevens te zoeken.

Zie [aan de slag met Azure zoeken in de portal](search-get-started-portal.md) voor een snelle zelfstudie.
