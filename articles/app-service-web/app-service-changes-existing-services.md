<properties
    pageTitle="Azure App-Service en de gevolgen voor bestaande Azure services"
    description="Dit artikel wordt uitgelegd wat de nieuwe Azure App-Service en de bijbehorende functies voor invloed op bestaande services in Azure wordt aangegeven."
    services="app-service"
    documentationCenter=""
    authors="yochay"
    manager="nirma"
    editor=""/>

<tags
    ms.service="app-service"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="02/12/2016"
    ms.author="yochayk"/>


# <a name="azure-app-service-and-existing-azure-services"></a>Azure App-Service en bestaande Azure services

In dit artikel worden de wijzigingen in bestaande Azure services als onderdeel van de verschillende services van Azure samengebracht in [Azure App-Service](https://azure.microsoft.com/services/app-service/), een nieuwe geïntegreerde aanbod.

[AZURE.INCLUDE [app-service-web-to-api-and-mobile](../../includes/app-service-web-to-api-and-mobile.md)]

## <a name="overview"></a>Overzicht

[Azure App-Service](https://azure.microsoft.com/services/app-service/) is een nieuwe en unieke cloudservice waarmee ontwikkelaars web en mobiele apps voor elk platform en elk apparaat maken. App-Service is een geïntegreerde oplossing ontworpen voor stroomlijnen herhaalde coding functies, integreren met enterprise en SaaS systemen en bedrijfsprocessen automatiseren tijdens het tegemoetkomen aan uw behoeften voor beveiliging, betrouwbaarheid en schaalbaarheid.

App Service levert met de volgende bestaande Azure services - [Websites](https://azure.microsoft.com/services/websites/), [Mobile-Services](https://azure.microsoft.com/services/mobile-services/)en [Biztalk Services](https://azure.microsoft.com/services/biztalk-services/) een service voor eenmalige gecombineerde bij het toevoegen van krachtige nieuwe mogelijkheden.  App Service kunt u voor het hosten van de volgende typen van de app:

-   WebApps
-   Mobiele Apps
-   API-Apps
-   Logica Apps

De volgende tabel wordt uitgelegd hoe bestaande Azure services toewijzen aan App Service en de beschikbaar in het app-typen.

<table>
<thead>
<tr class="header">
<th align="left", style="width:10%">Bestaande Azure-Service</th>
<th align="left", style="width:10%">Azure App-Service</th>
<th align="left", style="width:80%">Wat is gewijzigd</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left">Azure-Websites</td>
<td align="left">WebApps</td>
<td align="left"><li>Voor Websites van Azure is de App Service strikt beperkt tot het wijzigen van de naam Websites in Web Apps.
<p><li>Uw bestaande exemplaren van Websites zijn nu de Web-Apps in de App-Service.</p>
<p><li>U kunt toegang tot uw bestaande websites via de <a href="http://go.microsoft.com/fwlink/?LinkId=529715">Portal van Azure</a>, waar u alle bestaande sites onder <em>Web Apps vindt</em>.</p>
<p><li><em>Webonderdelen hostingprovider plannen</em> is nu <em>App-Service plannen</em>. Een <em>App-Service plannen</em> kunt elk gewenst type app App-Service, zoals Web, Mobile, logica of API apps hosten.</p>
<p><li>Azure WebApps van de App-Service is in het algemeen beschikbaarheid.</p>
<p><li><a href="http://azure.microsoft.com/services/app-service/web/">Meer informatie over Web Apps</a>.</p></td>
</tr>
<tr class="even">
<td align="left">Azure mobiele Services</td>
<td align="left">Mobiele Apps</td>
<td align="left"><p><li>Mobile-Services blijven beschikbaar als een zelfstandig product-service en blijven volledig wordt ondersteund.</p>
<p><li>Mobile-Apps is een type app in de App-Service, die de functionaliteit van de Mobile-Services en meer integreert.</p>
<p><li>Het is eenvoudig migreren <a href="http://go.microsoft.com/fwlink/?LinkID=724279&clcid=0x409">van mobiele Services naar Mobile-Apps</a>.</p>
<p><li>Als onderdeel van de App Service krijgen Mobile-Apps nieuwe mogelijkheden voorbij Mobile-Services, zoals integratie met on-premises implementatie en SaaS systemen, tijdelijke sleuven, WebJobs, beter schaalopties en meer.</p>
<p><li><a href="http://azure.microsoft.com/services/app-service/mobile/">Meer informatie over Mobile-Apps</a>.</p>
</tr>
<tr class="odd">
<td align="left"></td>
<td align="left">API-Apps</td>
<td align="left">
<p><li>API-Apps is een nieuwe app-type in de App-Service waarmee u gemakkelijk maken en gebruiken van API's in de cloud.</p>
<p><li><a href="http://azure.microsoft.com/services/app-service/api/">Meer informatie over API Apps</a>.</p></td>
</tr>
<tr class="even">
<td align="left"></td>
<td align="left">Logica Apps</td>
<td align="left">
<p><li>Logica Apps is een nieuwe app-type in de App-Service waarmee u gemakkelijk automatiseren bedrijfsproces.</p>
<p><li><a href="http://azure.microsoft.com/services/app-service/logic/">Meer informatie over logica Apps</a>.</p></td>
</tr>
<tr class="odd">
<td align="left">Azure BizTalk-Services</td>
<td align="left">BizTalk-API-Apps</td>
<td align="left">
<li><p>BizTalk Services blijven beschikbaar als een zelfstandig product-service en blijven volledig wordt ondersteund.</p>
<li><p>De mogelijkheden van BizTalk-Services integreren in de App Service als API Apps zodat gebruikers uitvoeren van de integratie van bedrijfstoepassingen B2B scenario's en integratie met een van de app-typen in de App Service</p>
<li><p>Met logica Apps kunt u nu werkstromen maken met een visuele ontwerp bedrijfsprocessen automatiseren</p></td>
</tr>
</tbody>
</table>

Meer informatie, gaat u naar [App servicedocumentatie](https://azure.microsoft.com/documentation/services/app-service/).
