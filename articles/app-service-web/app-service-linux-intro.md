<properties 
    pageTitle="Inleiding tot het App-Service op Linux | Microsoft Azure" 
    description="Meer informatie over de App-Service op Linux." 
    keywords="Azure-app-service, linux, oss"
    services="app-service" 
    documentationCenter="" 
    authors="naziml" 
    manager="wpickett" 
    editor=""/>

<tags 
    ms.service="app-service" 
    ms.workload="na" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="10/10/2016" 
    ms.author="naziml"/>

# <a name="introduction-to-app-service-on-linux"></a>Inleiding tot het App-Service op Linux
App-Service op Linux is momenteel in de proefversie van openbare en actieve WebApps native ondersteunt op Linux. 

## <a name="overview"></a>Overzicht ##
Klanten kunt App Service op Linux host WebApps native op Linux naar ondersteunde toepassing stapels. De volgende functies sectie bevat de stapels momenteel ondersteunde toepassing.

## <a name="features"></a>Functies ##
App-Service op Linux ondersteunt momenteel het volgende toepassing stapels

- Node.js
- PHP

Klanten kunnen hun toepassingen met implementeren

- FTP.
- Lokale cijfer.
- GitHub of BitBucket.

Voor de toepassing schaalbaarheid


- Klanten kunnen schalen dat hun WebApp omhoog en omlaag door te wijzigen van de laag in de App-Service plannen. 
- Klanten kunnen schalen dat hun toepassingen af en hun app over meerdere exemplaren binnen de grenzen van hun SKU uitvoeren.

Voor Kudu werkt enkele van de basisfunctionaliteit

- Omgeving.
- Implementaties.
- Eenvoudige console.

## <a name="limitations"></a>Beperkingen ##

De Azure beheerportal wordt alleen momenteel ondersteunde functies voor App-Service op Linux weergeven en de rest wordt verborgen. Als het team meer functies zodat we worden behouden na te denken dit op de beheerportal. Sommige functies, zoals VNET integratie en AAD / verificatie van derden of Kudu site extensies momenteel werken niet. Maar terwijl we deze werken krijgen we onze documentatie en blog over wijzigingen bijgewerkt.

Deze openbare preview is momenteel alleen beschikbaar in de volgende regio 's

-   West VS.
-   West Europa.
-   Zuidoost-Azië.

Web-app op Linux wordt alleen ondersteund in een specifiek App Service-abonnementen en heeft geen een laag gratis of gedeeld. App-serviceplannen voor gewone en Linux-WebApps ook zijn elkaar wederzijds uitsluiten, zodat u een Linux-web-app in een niet-Linux app-abonnement maken kunt.

WebApp op Linux moet worden gemaakt in een resourcegroep die geen niet-Linux-WebApps in dezelfde regio.

Klanten verwachten vanwege het gebrek overlappende hergebruik van de WebApps, dat een kleine downtime in het geval van een web-app opnieuw hebt gestart. 

## <a name="next-steps"></a>Volgende stappen ##

Volg de volgende koppelingen aan de slag met App-Service op Linux. Post de vragen en problemen op [onze forum](https://social.msdn.microsoft.com/forums/azure/home?forum=windowsazurewebsitespreview).

* [Maken van WebApps in de App-Service op Linux](./app-service-linux-how-to-create-a-web-app.md)
* [Met behulp van PM2 configuratie voor Node.js in Web-Apps op Linux](./app-service-linux-using-nodejs-pm2.md)

