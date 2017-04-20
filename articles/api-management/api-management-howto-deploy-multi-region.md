<properties
    pageTitle="Hoe u een exemplaar van Azure API Management service implementeren naar meerdere Azure regio 's"
    description="Leer hoe u een exemplaar van Azure API Management service implementeren naar meerdere Azure regio's." 
    services="api-management"
    documentationCenter=""
    authors="steved0x"
    manager="erikre"
    editor=""/>

<tags
    ms.service="api-management"
    ms.workload="mobile"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/25/2016"
    ms.author="sdanie"/>

# <a name="how-to-deploy-an-azure-api-management-service-instance-to-multiple-azure-regions"></a>Hoe u een exemplaar van Azure API Management service implementeren naar meerdere Azure regio 's

API Management ondersteunt meerdere landen/regio-implementatie waarmee API uitgevers naar een enkel API-beheerservice verdelen over een willekeurig aantal gewenste Azure regio's. Dit vermindert verzoek latentie waargenomen door geografisch verspreid API consumenten en verbetert de servicebeschikbaarheid van de ook als één regio offline gaat. 

Wanneer een API Management-service in eerste instantie gemaakt is, bevat slechts één [eenheid][] en bevindt zich in een enkel Azure regio, die is opgegeven als de primaire regio. Extra gebieden kunnen eenvoudig worden toegevoegd via Azure klassieke Portal. API Management gateway-server is geïmplementeerd in elke regio en verkeer call worden doorgestuurd naar de dichtstbijzijnde gateway. Als u een gebied offline gaat, wordt het verkeer automatisch opnieuw Services naar de volgende eerstvolgende gateway. 

> [AZURE.IMPORTANT] Meerdere landen/regio-implementatie is alleen beschikbaar in de **[Premium][]** -laag.

## <a name="add-region"> </a>Een exemplaar van de service API Management implementeren naar een nieuwe regio

Als u wilt beginnen, klikt u op **beheren** in de klassieke Azure-Portal voor uw service API Management. Hiermee gaat u naar de publisher-API-beheerportal.

![Publisher-portal][api-management-management-console]

>Als u een exemplaar van de service API Management nog niet hebt gemaakt, raadpleegt u [een exemplaar van de service API Management maken][] in deze zelfstudie [aan de slag met Azure API Management][] .

Ga naar het tabblad **schaal** in klassieke Azure-Portal voor uw exemplaar van de service API Management. 

![Tabblad schaal][api-management-scale-service]

Als u wilt implementeren naar een nieuwe gebied, klik op de vervolgkeuzelijst onder de primaire regio en kiest u een gebied in de lijst.

![Regio toevoegen][api-management-add-region]

Zodra de regio is geselecteerd, kiest u het aantal eenheden voor de nieuwe regio in de vervolgkeuzelijst eenheid.

![Eenheden opgeven][api-management-select-units]

Nadat de gewenste regio's en de eenheden die zijn geconfigureerd, klikt u op **Opslaan**.

## <a name="remove-region"> </a>Een exemplaar van de service API Management verwijderen uit een regio

Als u wilt een exemplaar van de service API Management verwijderen uit een regio, Ga naar het tabblad **schaal** in klassieke Azure-Portal voor uw exemplaar van de service API Management. 

![Tabblad schaal][api-management-scale-service]

Klik op de **X** rechts van het gewenste gebied wilt verwijderen.  

![Gebied verwijderen][api-management-remove-region]

Wanneer de gewenste regio's worden verwijderd, klikt u op **Opslaan**.


[api-management-management-console]: ./media/api-management-howto-deploy-multi-region/api-management-management-console.png

[api-management-scale-service]: ./media/api-management-howto-deploy-multi-region/api-management-scale-service.png
[api-management-add-region]: ./media/api-management-howto-deploy-multi-region/api-management-add-region.png
[api-management-select-units]: ./media/api-management-howto-deploy-multi-region/api-management-select-units.png
[api-management-remove-region]: ./media/api-management-howto-deploy-multi-region/api-management-remove-region.png

[Een exemplaar van de service API Management maken]: api-management-get-started.md#create-service-instance
[Aan de slag met Azure API Management]: api-management-get-started.md

[Deploy an API Management service instance to a new region]: #add-region
[Delete an API Management service instance from a region]: #remove-region

[eenheid]: http://azure.microsoft.com/pricing/details/api-management/
[Premium]: http://azure.microsoft.com/pricing/details/api-management/

