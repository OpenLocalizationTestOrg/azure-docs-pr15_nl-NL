<properties 
    pageTitle="De status van de sessie met cache in Azure App Service Azure bestand Vgx." 
    description="Informatie over het gebruik van de Service van de Cache Azure ter ondersteuning van caching van ASP.NET sessie staat." 
    services="app-service\web" 
    documentationCenter=".net" 
    authors="Rick-Anderson" 
    manager="wpickett" 
    editor="none"/>

<tags 
    ms.service="app-service-web" 
    ms.workload="na" 
    ms.tgt_pltfrm="na" 
    ms.devlang="dotnet" 
    ms.topic="get-started-article" 
    ms.date="06/27/2016" 
    ms.author="riande"/>


# <a name="session-state-with-azure-redis-cache-in-azure-app-service"></a>De status van de sessie met cache in Azure App Service Azure bestand Vgx.


In dit onderwerp wordt uitgelegd hoe u de Service Azure bestand Vgx. Cache voor sessie staat.

Als u uw ASP.NET-web-app gebruikt sessie staat, moet u voor het configureren van een externe sessie staat-provider (de Service bestand Vgx. Cache of een provider voor SQL Server sessie staat). Als u session-status en een externe provider niet gebruikt, bent u beperkt op één exemplaar van uw web-app. De Service Cache bestand Vgx. is de snelste en eenvoudigste om in te schakelen.

[AZURE.INCLUDE [app-service-web-to-api-and-mobile](../../includes/app-service-web-to-api-and-mobile.md)] 

##<a id="createcache"></a>De Cache maken
Volg [deze aanwijzingen](../cache-dotnet-how-to-use-azure-redis-cache.md#create-cache) om u te maken van de cache.

##<a id="configureproject"></a>Het pakket RedisSessionStateProvider NuGet toevoegen aan uw web-app
Installeer de NuGet `RedisSessionStateProvider` pakket.  Gebruik van de volgende opdracht uit om te installeren vanuit de beheerconsole pakket (**Hulpmiddelen voor** > **NuGet Package Manager** > **Package Manager-Console**):

  `PM> Install-Package Microsoft.Web.RedisSessionStateProvider`
  
Om te installeren vanuit de **Hulpmiddelen voor** > **NuGet Package Manager** > **NugGet-pakketten beheren voor oplossing**, zoeken naar `RedisSessionStateProvider`.

Zie de [NuGet RedisSessionStateProvider pagina](http://www.nuget.org/packages/Microsoft.Web.RedisSessionStateProvider/ ) en [de cache-client configureren](../cache-dotnet-how-to-use-azure-redis-cache.md#NuGet)voor meer informatie.

##<a id="configurewebconfig"></a>Het bestand Web.Config aanpassen
Naast het constructie verwijzingen voor Cache aanbrengt, worden het pakket NuGet stub vermeldingen opgeteld in het bestand *web.config* . 

1. Open de *web.config* en Ga naar de het element **sessionState** .

1. Voer de waarden voor `host`, `accessKey`, `port` (de SSL-poort moet 6380), en stel `SSL` naar `true`. Deze waarden kunnen worden opgehaald uit het blad [Azure-Portal](http://go.microsoft.com/fwlink/?LinkId=529715) voor uw exemplaar van de cache. Zie [verbinding maken met de cache](../cache-dotnet-how-to-use-azure-redis-cache.md#connect-to-cache)voor meer informatie. Houd er rekening mee dat de niet-SSL-poort al dan niet standaard voor nieuwe cache is uitgeschakeld. Zie de sectie [Toegang poorten](https://msdn.microsoft.com/library/azure/dn793612.aspx#AccessPorts) in het onderwerp [een cache in Azure bestand Vgx. Cache configureren](https://msdn.microsoft.com/library/azure/dn793612.aspx) voor meer informatie over het inschakelen van de niet-SSL-poort. De volgende markeringen ziet u de wijzigingen in het bestand *web.config* specifiek de wijzigingen in *poort*, *host*, accessKey*, en *ssl *.

          <system.web>;
            <customErrors mode="Off" />;
            <authentication mode="None" />;
            <compilation debug="true" targetFramework="4.5" />;
            <httpRuntime targetFramework="4.5" />;
            <sessionState mode="Custom" customProvider="RedisSessionProvider">;
              <providers>;  
                  <!--<add name="RedisSessionProvider" 
                    host = "127.0.0.1" [String]
                    port = "" [number]
                    accessKey = "" [String]
                    ssl = "false" [true|false]
                    throwOnError = "true" [true|false]
                    retryTimeoutInMilliseconds = "0" [number]
                    databaseId = "0" [number]
                    applicationName = "" [String]
                  />;-->;
                 <add name="RedisSessionProvider" 
                      type="Microsoft.Web.Redis.RedisSessionStateProvider" 
                      port="6380"
                      host="movie2.redis.cache.windows.net" 
                      accessKey="m7PNV60CrvKpLqMUxosC3dSe6kx9nQ6jP5del8TmADk=" 
                      ssl="true" />;
              <!--<add name="MySessionStateStore" type="Microsoft.Web.Redis.RedisSessionStateProvider" host="127.0.0.1" accessKey="" ssl="false" />;-->;
              </providers>;
            </sessionState>;
          </system.web>;


##<a id="usesessionobject"></a>Het sessieobject gebruiken in Code
De laatste stap is om te beginnen met het object sessie in uw ASP.NET-code. U toevoegen objecten aan de status van de sessie met behulp van de methode **Session.Add** . Deze methode wordt sleutel-waardeparen voor de opslag van items in de cache voor sessie staat.

    string strValue = "yourvalue";
    Session.Add("yourkey", strValue);

De volgende code haalt deze waarde sessie plaats.

    object objValue = Session["yourkey"];
    if (objValue != null)
       strValue = (string)objValue; 

U kunt ook de Cache bestand Vgx. in cacheobjecten gebruiken in uw web-app. Zie [MVC film app met Azure bestand Vgx. Cache in 15 minuten](https://azure.microsoft.com/blog/2014/06/05/mvc-movie-app-with-azure-redis-cache-in-15-minutes/)voor meer informatie.
Zie voor meer informatie over het gebruik van de status van de ASP.NET-sessie [ASP.NET sessie staat overzicht][].

>[AZURE.NOTE] Als u aan de slag met Azure App Service wilt voordat u zich registreert voor een Azure-account, gaat u naar de [App-Service probeert](http://go.microsoft.com/fwlink/?LinkId=523751), waar u direct een tijdelijk starter in de browser in de App-Service maken kunt. Geen creditcards vereist; geen verplichtingen.

## <a name="whats-changed"></a>Wat er gewijzigd
* Zie voor een handleiding voor het wijzigen van Websites naar App Service: [Azure App-Service en de invloed op bestaande Azure-Services](http://go.microsoft.com/fwlink/?LinkId=529714)

  *Door [Rick Anderson](https://twitter.com/RickAndMSFT)*
  
  [installed the latest]: http://www.windowsazure.com/downloads/?sdk=net  
  [Status van de sessie ASP.NET-overzicht]: http://msdn.microsoft.com/library/ms178581.aspx

  [NewIcon]: ./media/web-sites-dotnet-session-state-caching/CacheScreenshot_NewButton.png
  [NewCacheDialog]: ./media/web-sites-dotnet-session-state-caching/CachingScreenshot_CreateOptions.png
  [CacheIcon]: ./media/web-sites-dotnet-session-state-caching/CachingScreenshot_CacheIcon.png
  [NuGetDialog]: ./media/web-sites-dotnet-session-state-caching/CachingScreenshot_NuGet.png
  [OutputConfig]: ./media/web-sites-dotnet-session-state-caching/CachingScreenshot_OC_WebConfig.png
  [CacheConfig]: ./media/web-sites-dotnet-session-state-caching/CachingScreenshot_CacheConfig.png
  [EndpointURL]: ./media/web-sites-dotnet-session-state-caching/CachingScreenshot_EndpointURL.png
  [ManageKeys]: ./media/web-sites-dotnet-session-state-caching/CachingScreenshot_ManageAccessKeys.png
 
