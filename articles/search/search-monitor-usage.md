<properties 
   pageTitle="Gebruik en statistieken in een Azure-zoekservice controleren | Microsoft Azure | De zoekservice gehoste cloud" 
   description="Resource-verbruik en index grootte voor zoekprogramma's Azure, een gehoste cloud-zoekservice op Microsoft Azure bijhouden." 
   services="search" 
   documentationCenter="" 
   authors="HeidiSteen" 
   manager="jhubbard" 
   editor=""
   tags="azure-portal"/>

<tags
   ms.service="search"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="required" 
   ms.date="05/17/2016"
   ms.author="heidist"/>

# <a name="monitor-usage-and-statistics-in-an-azure-search-service"></a>Gebruik controleren en statistieken in een zoekservice van Azure

Het bijhouden van de groei van indexen en grootte van document, kunt u proactief capaciteit aanpassen voordat u kunt u door de bovengrens die u hebt gedefinieerd voor uw service. 

Als u wilt controleren Resourcegebruik, telt en statistieken worden aangepast voor uw service kunnen eenvoudig worden weergegeven in de [Portal van Azure](https://portal.azure.com), maar u kunt ook de informatie verzamelen via een programma als u een hulpprogramma voor het beheer van aangepaste service. In dit artikel behandelt de stappen voor beide technieken.

U kunt de nieuwe functie met analytics-verkeer is toegestaan voor zoeken ook gebruiken om inzicht te krijgen in activiteit op het indexniveau van de. Ga naar [Zoeken verkeer Analytics voor zoekprogramma's Azure](search-traffic-analytics.md) aan de slag.

##<a name="view-counts-and-metrics-in-the-portal"></a>Telt en statistieken in de portal weergeven 

1. Meld u aan bij de [Portal van Azure](https://portal.azure.com). 

2. Open de servicedashboard van uw Azure Search-service. Tegels voor de service vindt u op de startpagina of kunt u de service nodig van bladeren bladeren in de JumpBar. Zie [maken een service](search-create-service-portal.md) voor stapsgewijze instructies.

De sectie gebruik bevat een meter waarin u welk deel van beschikbare resources zijn momenteel in gebruik.

  ![][1]

Intrekken dat de gedeelde service een maximum van één replica heeft en partitioneren elk. Daarnaast kunnen 10.000 documenten kan alleen worden ondersteund in totaal of 50 MB aan gegevens, afhankelijk van wat zich het eerste voordoet.

##<a name="get-index-statistics-using-the-rest-api"></a>Wees indexstatistieken met behulp van de REST API

De Azure zoeken REST API zowel de .NET SDK programma toegang bieden tot service aan de doelstellingen.  Als u een index laden van Azure SQL-Database of DocumentDB, een extra API [indexeerfuncties](https://msdn.microsoft.com/library/azure/dn946891.aspx) is beschikbaar om de getallen die u nodig hebt. 

  + [Indexstatistieken opvragen](https://msdn.microsoft.com/library/azure/dn798942.aspx)
  + [Aantal documenten](https://msdn.microsoft.com/library/azure/dn798924.aspx)
  + [Indexering Status ophalen](https://msdn.microsoft.com/library/azure/dn946884.aspx)

## <a name="next-steps"></a>Volgende stappen

Bekijk [limieten en capaciteit](search-limits-quotas-capacity.md) om te bepalen de combinatie van partities en replica's die u nodig hebt als bestaande capaciteit onvoldoende is. 

Ga naar [uw zoekservice op Microsoft Azure beheren](search-manage.md) voor meer informatie over het servicebeheer van de.

<!--Image references-->
[1]: ./media/search-monitor-usage/AzureSearch-Monitor1.PNG




 
