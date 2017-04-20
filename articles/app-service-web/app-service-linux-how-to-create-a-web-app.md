<properties 
    pageTitle="Het maken van een Web-App met App-Service op Linux | Microsoft Azure" 
    description="Web-app maken werkstroom voor het App-Service op Linux." 
    keywords="Azure-app-service, in de browser, linux, oss"
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

# <a name="create-a-web-app-with-app-service-on-linux"></a>Een Web-App met App-Service op Linux maken

## <a name="using-the-management-portal-to-create-your-web-app"></a>De beheerportal gebruiken om te maken van uw web-app
Uw Web-App maken op Linux in de [beheerportal](https://portal.azure.com) , zoals wordt weergegeven in de onderstaande afbeelding, kunt u beginnen.

![][1]

Wanneer u de onderstaande opties hebt geselecteerd, worden weergegeven het blad maken zoals wordt weergegeven in de onderstaande afbeelding. 

![][2]

-   Geef uw web-app een naam.
-   Kies een bestaande groep voor de Resource of een nieuw account te maken. (Zie regio's beschikbaar in de [sectie beperkingen](./app-service-linux-intro.md)).
-   Kies een bestaand app-abonnement of maak een nieuw een (Zie app service abonnement opmerkingen in de [sectie beperkingen](./app-service-linux-intro.md)). 
-   Kies de toepassing stapel die u wilt gebruiken. U krijgt kunt kiezen tussen verschillende versies van Node.js en PHP. 

Nadat u de app hebt gemaakt hebt, kunt u de stapel toepassing wijzigen in de toepassingsinstellingen, zoals wordt weergegeven in de onderstaande afbeelding.

![][3]

## <a name="deploying-your-web-app"></a>Uw web-app implementeren

Optie 'distributieopties' in de beheerportal kiest, kunt u de optie een opslagplaats cijfer of een bibliotheek GitHub lokaal gebruiken om te implementeren van uw toepassing. Zijn de instructies daarna op dezelfde manier naar een niet-Linux-web-app en volgt u de instructies in de [lokale implementatie van cijfer](./app-service-deploy-local-git.md) of onze [continue implementatie](./app-service-continuous-deployment.md) -artikel voor GitHub.

U kunt ook FTP gebruiken voor het uploaden van uw toepassing aan uw site. U kunt de FTP-eindpunt voor uw web-app downloaden in de sectie met diagnostische gegevens logboeken op zoals weergegeven in de onderstaande afbeelding.

![][4]


## <a name="next-steps"></a>Volgende stappen ##

* [Wat is de App-Service op Linux?](./app-service-linux-intro.md)
* [Met behulp van PM2 configuratie voor Node.js in Web-Apps op Linux](./app-service-linux-using-nodejs-pm2.md)

<!--Image references-->
[1]: ./media/app-service-linux-how-to-create-a-web-app/top-level-create.png
[2]: ./media/app-service-linux-how-to-create-a-web-app/create-blade.png
[3]: ./media/app-service-linux-how-to-create-a-web-app/application-settings-change-stack.png
[4]: ./media/app-service-linux-how-to-create-a-web-app/diagnostic-logs-ftp.png
