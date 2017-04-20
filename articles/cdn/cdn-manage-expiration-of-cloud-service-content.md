<properties
 pageTitle="Het beheren van verlooptijd van Azure-Apps/Cloud webservices, ASP.NET en IIS-inhoud in Azure CDN | Microsoft Azure"
 description="Wordt beschreven hoe u de vervaldatum van cloud-service in Azure CDN beheren"
 services="cdn"
 documentationCenter=".NET"
 authors="camsoper"
 manager="erikre"
 editor=""/>
<tags
 ms.service="cdn"
 ms.workload="media"
 ms.tgt_pltfrm="na"
 ms.devlang="dotnet"
 ms.topic="article"
 ms.date="09/19/2016"
 ms.author="casoper"/>

# <a name="how-to-manage-expiration-of-azure-web-appscloud-services-aspnet-or-iis-content-in-azure-cdn"></a>Het verlopen van Azure Web Apps/Cloud Services, ASP.NET of IIS-inhoud in Azure CDN beheren

> [AZURE.SELECTOR]
- [Azure Web Apps-Cloudservices, ASP.NET of IIS](cdn-manage-expiration-of-cloud-service-content.md)
- [Azure blob Storage-service](cdn-manage-expiration-of-blob-content.md)

Bestanden uit een webserver openbaar origin kunnen worden opgeslagen in Azure CDN totdat de time to live (TTL) is verstreken.  De TTL wordt bepaald door de [ *Cache-Control* kop](http://www.w3.org/Protocols/rfc2616/rfc2616-sec14.html#sec14.9) in het HTTP-antwoord van de originele server.  In dit artikel wordt beschreven hoe instelt `Cache-Control` berichtkoppen van Azure Web Apps, Azure-Cloudservices ASP.NET-toepassingen en Internet Information Services-sites, die al op dezelfde manier worden geconfigureerd.

>[AZURE.TIP] U kunt geen TTL instellen op een bestand.  In dit geval de Azure CDN een standaard-TTL van zeven dagen automatisch toegepast.
>
>Zie voor meer informatie over de werking van Azure CDN snelheid van toegang tot bestanden en andere bronnen, [Azure CDN overzicht](./cdn-overview.md).

## <a name="setting-cache-control-headers-in-configuration"></a>Instelling Cachebeheer kopteksten in configuratie

Naar statische inhoud, zoals afbeeldingen en opmaakmodellen, kunt u de updatefrequentie bepalen doordat de **applicationHost.config** of **web.config** -bestanden voor uw webtoepassing.  Het element **system.webServer\staticContent\clientCache** in het configuratiebestand wordt ingesteld de `Cache-Control` koptekst voor uw inhoud. Voor **web.config**, de configuratie-instellingen is van invloed op alles in de map en alle submappen, tenzij opgeheven op het niveau van de submap.  U kunt bijvoorbeeld een standaardwaarde instellen time to live in de hoofdmap dat alle statische inhoud in cache opslaan voor 3 dagen, maar hebben een submap met meer variabele inhoud met een instelling van 6 uur.  Voor **applicationHost.config**, wordt in alle toepassingen op de site worden beïnvloed, maar kunnen worden overschreven in **web.config** -bestanden in de toepassingen.

De volgende XML-ziet en voorbeeld van de instelling **clientCache** om op te geven van een maximale duur van 3 dagen:  

```xml
<configuration>
    <system.webServer>
        <staticContent>
            <clientCache cacheControlMode="UseMaxAge" cacheControlMaxAge="3.00:00:00" />
        </staticContent>
    </system.webServer>
</configuration>
```

Precisie **UseMaxAge** voegt een `Cache-Control: max-age=<nnn>` koptekst aan het antwoord op basis van de waarde die is opgegeven in het kenmerk **CacheControlMaxAge** . De opmaak van het interval is voor het kenmerk **cacheControlMaxAge** is <days>. <hours>:<min>:<sec>. Zie voor meer informatie over het knooppunt **clientCache** , [Client-Cache <clientCache> ](http://www.iis.net/ConfigReference/system.webServer/staticContent/clientCache).  

## <a name="setting-cache-control-headers-in-code"></a>Instelling Cachebeheer kopteksten in Code

U kunt de CDN via programmacode caching van probleem met de eigenschap **HttpResponse.Cache** instellen voor ASP.NET-toepassingen. Zie voor meer informatie over de eigenschap **HttpResponse.Cache** , [HttpResponse.Cache eigenschap](http://msdn.microsoft.com/library/system.web.httpresponse.cache.aspx) en [HttpCachePolicy Class](http://msdn.microsoft.com/library/system.web.httpcachepolicy.aspx).  

Als u via programmacode toepassingsinhoud in ASP.NET in cache, zorg ervoor dat de inhoud is gemarkeerd als cache door in te stellen HttpCacheability op *openbaar*wilt. Controleer ook of een cache validatie is ingesteld. De cache validatie kan zijn een laatst gewijzigd tijdstempel instellen door de ondersteuning voor SetLastModified of een etag-waarde instellen door de ondersteuning voor SetETag. (Optioneel) u kunt ook een verlooptijd cache opgeven door te bellen SetExpires of u kunt vertrouwen op de standaard-cache heuristiek eerder in dit document beschreven.  

Voer bijvoorbeeld de volgende naar inhoud voor één uur cache:  

```csharp
// Set the caching parameters.
Response.Cache.SetExpires(DateTime.Now.AddHours(1));
Response.Cache.SetCacheability(HttpCacheability.Public);
Response.Cache.SetLastModified(DateTime.Now);
```

## <a name="next-steps"></a>Volgende stappen

- [Lees meer over het element **clientCache**](http://www.iis.net/ConfigReference/system.webServer/staticContent/clientCache)
- [Lees de documentatie voor de eigenschap **HttpResponse.Cache**](http://msdn.microsoft.com/library/system.web.httpresponse.cache.aspx) 
- [Lees de documentatie voor de **HttpCachePolicy Class**](http://msdn.microsoft.com/library/system.web.httpcachepolicy.aspx).  
