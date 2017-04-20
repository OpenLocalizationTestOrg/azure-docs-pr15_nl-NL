<properties 
    pageTitle="API-Apps Inleiding | Microsoft Azure" 
    description="Lees hoe Azure App Service helpt u ontwikkelt, host, en RESTful API's in beslag nemen." 
    services="app-service\api" 
    documentationCenter=".net" 
    authors="tdykstra" 
    manager="wpickett" 
    editor=""/>

<tags 
    ms.service="app-service-api" 
    ms.workload="web" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="get-started-article" 
    ms.date="08/23/2016" 
    ms.author="rachelap"/>

# <a name="api-apps-overview"></a>Overzicht van de API-Apps

API-apps in Azure App-Service biedt functies die het gemakkelijker maken om te ontwikkelen, host, en API's in de cloud en on-premises gebruiken. Met de API apps krijgt u enterprise grade beveiliging, eenvoudige toegangsbeheer, hybride connectivity, automatische SDK generatie en naadloze integratie met [Logica Apps](../app-service-logic/app-service-logic-what-are-logic-apps.md).

[Azure App-Service](../app-service/app-service-value-prop-what-is.md) is een volledig beheerde platform voor het web, mobiele, scenario's en integratie. API-Apps is een van de vier app typen aangeboden door [Azure App-Service](../app-service/app-service-value-prop-what-is.md).

![App-typen in Azure App-Service](./media/app-service-api-apps-why-best-platform/appservicesuite.png)

## <a name="why-use-api-apps"></a>Het nut van API-Apps?

Hier volgen enkele belangrijke functies van API-Apps:

- **Overbrengen van uw bestaande API als-is** -u niet hoeft te wijzigen van de code in uw bestaande API's om te profiteren van API-Apps--alleen uw code implementeren naar een API-app. Uw API kunt elke taal of framework wordt ondersteund door de App-Service, inclusief ASP.NET en C#, Java PHP, Node.js en Python gebruiken.

- **Eenvoudig verbruik** - geïntegreerde ondersteuning voor [Swagger API metagegevens](http://swagger.io/) kunt u eenvoudig verbruikbare door verschillende clients uw API's.  Automatisch clientcode genereren voor uw API's in verschillende talen, inclusief C#, Java en Javascript. Eenvoudig configureren [CORS](app-service-api-cors-consume-javascript.md) zonder te wijzigen van uw code. Zie [App Service API Apps metagegevens voor API discovery en code generatie](app-service-api-metadata.md) en [verbruiken een API-app in JavaScript CORS gebruiken](app-service-api-cors-consume-javascript.md)voor meer informatie. 

- **Eenvoudige toegangsbeheer** - een API-app beschermen tegen niet-geverifieerde toegang zonder wijzigingen aan uw code. Ingebouwde verificatieservices secure API's voor access met andere services of met clients dat staat voor gebruikers. Ondersteunde identiteitsproviders zijn Azure Active Directory, Facebook, Twitter, Google en Microsoft-Account. Clients kunnen gebruiken voor Active Directory-verificatie bibliotheek (ADAL) of de SDK van de Mobile-Apps. Zie [verificatie en autorisatie voor API-Apps in Azure App-Service](app-service-api-authentication.md)voor meer informatie.

- **Integratie van visual Studio** - specifieke tools in Visual Studio stroomlijnen de werklast van maken, implementeert, door andere, foutopsporing en API apps beheren. Zie [de SDK Azure punt 2.8.1 voor .NET aankondigen](/blog/announcing-azure-sdk-2-8-1-for-net/)voor meer informatie.

- **Integratie met logica Apps** - API-apps die u maakt, kan worden gebruikt door de [App Service logica Apps](../app-service-logic/app-service-logic-what-are-logic-apps.md).  Zie [werken met uw aangepaste API die worden gehost op App-Service met logica apps](../app-service-logic/app-service-logic-custom-hosted-api.md) en [nieuwe schema versie 2015-08-01-preview](../app-service-logic/app-service-logic-schema-2015-08-01.md)voor meer informatie.

Daarnaast kunt een API-app profiteren van functies in de [Web Apps](../app-service-web/app-service-web-overview.md) en [Mobile-Apps](../app-service-mobile/app-service-mobile-value-prop.md). Het omgekeerde geldt ook: als u een WebApp of mobiele app voor het hosten van een API gebruikt, dit kunt profiteren van Apps API-functies zoals Swagger metagegevens voor client code genereren en CORS voor toegang tussen domeinen via de browser. De enige verschil tussen de drie app typen (API, web, mobiele) is de naam en het pictogram dat wordt gebruikt voor hen in de portal van Azure.

## <a name="whats-the-difference-between-api-apps-and-azure-api-management"></a>Wat is het verschil tussen API-Apps en Azure API Management?

API-Apps en [Azure API Management](../api-management/api-management-key-concepts.md) zijn de bijbehorende services:

* API Management is over het beheren van API's. U ervoor zorgen dat de front-end van een API Management om te controleren en vertraging gebruik op een API, bewerken en uitvoer, diverse API's samenvoegen tot één eindpunt, enzovoort. De API's wordt beheerd, kunnen een willekeurige plaats worden gehost.
* API-Apps is over het hosten van API's. De service bevat functies waarmee ontwikkelen en in beslag nemen API's, maar het niet de soorten monitoring, te beperken, bewerken of consolideren die Management API bevat. Als u niet nodig API beheerfuncties hebt, kunt u API's hosten in API-apps zonder API te beheren.

Hier ziet u een diagram dat API Management wordt gebruikt voor API's die worden gehost in API-apps en elders weergeeft.

![Azure API Management en API-Apps](./media/app-service-api-apps-why-best-platform/apia-apim.png)

Sommige functies van API Management en API-Apps hebt vergelijkbare functies.  Bijvoorbeeld kunnen beide CORS ondersteuning automatiseren. Wanneer u de twee services samen gebruikt, zou u API Management gebruiken voor CORS aangezien deze als de front-end tot uw API-apps fungeert. 

## <a name="getting-started"></a>Aan de slag

Als u wilt beginnen met API Apps door het voorbeeldcode wordt geïmplementeerd op een, raadpleegt u de zelfstudie voor ongeacht framework u liever:

* [ASP.NET](app-service-api-dotnet-get-started.md) 
* [Node.js](app-service-api-nodejs-api-app.md) 
* [Java](app-service-api-java-api-app.md) 

Om te vragen stellen over API-apps, een thread in het [forum over API-Apps](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureAPIApps)te starten. 
