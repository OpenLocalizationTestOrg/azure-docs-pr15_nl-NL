<properties 
    pageTitle="Met behulp van PM2 configuratie voor NodeJS in Web-Apps op Linux | Microsoft Azure" 
    description="Met behulp van PM2 configuratie voor NodeJS in Web-Apps op Linux" 
    keywords="Azure-app-service, in de browser, nodejs, pm2, linux, oss"
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

# <a name="using-pm2-configuration-for-nodejs-in-web-apps-on-linux"></a>Met behulp van PM2 configuratie voor Node.js in Web-Apps op Linux

Als u de stapel toepassing op Node.js voor Web-Apps op Linux instelt, krijgt u de optie voor het instellen van een bestand met Node.js opstarten op zoals weergegeven in de onderstaande afbeelding.

![][1]

U kunt deze naar een

-   Geef het opstartscript voor uw app Node.js (bijvoorbeeld: /bin/server.js)
-   Het configuratiebestand PM2 gebruiken voor uw app Node.js opgeven (bijvoorbeeld: /foo/process.json)

 >[AZURE.NOTE] Als u wilt dat uw processen knooppunt opnieuw wordt opgestart wanneer bepaalde bestanden worden gewijzigd, moet u PM2 configuratie gebruiken. Anders uw toepassing wordt niet opnieuw opstarten nadat deze wijzigingsmeldingen van items, zoals continue implementatie ontvangt wanneer uw toepassingscode verandert.

U kunt de Node.js- [proces bestand documentatie](http://pm2.keymetrics.io/docs/usage/application-declaration/) voor alle opties controleren, maar hieronder volgt een voorbeeld van wat u als het bestand process.json gebruiken wilt

        {
          "name"        : "worker",
          "script"      : "/bin/server.js",
          "instances"   : 1,
          "merge_logs"  : true,
          "log_date_format" : "YYYY-MM-DD HH:mm Z",
          "watch": ["/bin/server.js", "foo.txt"],
          "watch_options": {
            "followSymlinks": true,
            "usePolling"   : true,
            "interval"    : 5
          }
        }

Belangrijke punten onthouden in deze configuratie zijn 

-   De eigenschap "script" geeft van uw toepassing start script.
-   De eigenschap "exemplaren" geeft hoeveel exemplaren van het knooppunt proces voor het starten. Als u uw toepassing op VM grotere die u meerdere cores hebt worden uitgevoerd, die u wilt uw resources maximaliseren door in te stellen een hogere waarde hier.
-   De matrix "controle" Hiermee geeft u alle bestanden voor wie wijzigen die u wilt om uw processen knooppunt opnieuw te starten.
-   Voor de "watch_options" moet u momenteel "usePolling" opgeven als waar vanwege de manier waarop die de toepassingsinhoud is gekoppeld.


## <a name="next-steps"></a>Volgende stappen ##

* [Wat is de App-Service op Linux?](./app-service-linux-intro.md)

<!--Image references-->
[1]: ./media/app-service-linux-using-nodejs-pm2/nodejs-startup-file.png