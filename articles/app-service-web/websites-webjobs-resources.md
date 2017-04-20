<properties 
    pageTitle="Azure WebJobs documentatie resources" 
    description="Leerbronnen voor het gebruik van Azure WebJobs en de SDK van Azure WebJobs aanbevolen." 
    services="app-service" 
    documentationCenter=".net" 
    authors="tdykstra" 
    manager="wpickett" 
    editor="jimbe"/>

<tags 
    ms.service="app-service" 
    ms.workload="na" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="10/28/2016" 
    ms.author="tdykstra"/>

# <a name="azure-webjobs-documentation-resources"></a>Azure WebJobs documentatie resources

## <a name="overview"></a>Overzicht

In dit onderwerp is gekoppeld aan de documentatie over het gebruik van Azure WebJobs en de SDK van Azure WebJobs. Azure WebJobs bieden een eenvoudige manier om het uitvoeren van scripts of programma's als achtergrondprocessen in de context van een [App Service WebApp, API-app of mobiele app](../app-service/app-service-value-prop-what-is.md). U kunt uploaden en uitvoeren van een uitvoerbaar bestand zoals als cmd, bat, exe (.NET), ps1, v, php, kopiëren, js en oppervlak. Deze programma's worden uitgevoerd als WebJobs volgens een schema (cron) of doorlopend.

Het doel van de [WebJobs SDK](websites-webjobs-resources.md) is om te vereenvoudigen de code die u voor algemene taken schrijft dat een WebJob kunt uitvoeren, zoals de verwerking van afbeeldingen, in de wachtrij processing, RSS aggregatie onderhoud van bestanden en e-mailberichten verzenden. De WebJobs SDK heeft ingebouwde functies voor het werken met Azure opslagcapaciteit en de Service Bus, voor het plannen van taken en foutafhandeling en voor groot aantal andere gebruikelijke scenario's. Daarnaast ontworpen moeten extensible en er is een [bron opslagplaats voor extensies openen](https://github.com/Azure/azure-webjobs-sdk-extensions/wiki/Binding-Extensions-Overview). [Azure-functies](../azure-functions/functions-overview.md) (die momenteel in preview) is gebaseerd op een versie van de WebJobs SDK die met C#-script, Node.js en andere talen werkt. 

Maken, implementeert en beheren van WebJobs is naadloze met geïntegreerde tooling in Visual Studio. U kunt WebJobs maken op basis van sjablonen, publiceren en beheren (klaar/stoppen/monitor/foutopsporing) ze. 

Het dashboard WebJobs in de portal van Azure biedt krachtige beheermogelijkheden waarmee u volledige controle over de uitvoering van WebJobs, waaronder de mogelijkheid om aan te roepen afzonderlijke functies binnen WebJobs. Het dashboard worden ook functie runtimes en logboekregistratie uitvoer weergegeven. 

##<a name="getstarted"></a>Aan de slag met WebJobs en de SDK WebJobs

* [Inleiding tot Azure WebJobs](http://www.hanselman.com/blog/IntroducingWindowsAzureWebJobs.aspx)
* [Azure WebJobs prima zijn en u moet beginnen met het gebruik van deze nu!](http://www.troyhunt.com/2015/01/azure-webjobs-are-awesome-and-you.html) (Blogbericht door te zoeken met Troy.)
* [Azure WebJobs functies](/blog/2014/10/22/webjobs-goes-into-full-production/)
* [Wat is de WebJobs SDK](websites-dotnet-webjobs-sdk.md)
* [Achtergrond taken richtlijnen door Microsoft patronen en procedures](/documentation/articles/best-practices-background-jobs/)
* [De 1.1.0 aankondigen RTM van Microsoft Azure WebJobs SDK](/blog/azure-webjobs-sdk-1-1-0-rtm/)
* [Aan de slag met de Azure WebJobs SDK](websites-dotnet-webjobs-sdk-get-started.md)
* [Het gebruik van Azure wachtrij opslagruimte met de WebJobs SDK](websites-dotnet-webjobs-sdk-storage-queues-how-to.md)
* [Hoe u met de WebJobs SDK Azure-blobopslag](websites-dotnet-webjobs-sdk-storage-blobs-how-to.md)
* [Hoe u met de WebJobs SDK Azure-tabelopslag](websites-dotnet-webjobs-sdk-storage-tables-how-to.md)
* [Het gebruik van Azure Service Bus met de WebJobs SDK](websites-dotnet-webjobs-sdk-service-bus.md)
* [Hoe u met de WebJobs SDK, met voorbeelden voor GitHub, IFTTT en HTTP WebHooks](https://github.com/Azure/azure-webjobs-sdk-extensions/wiki/WebHooks-Walkthrough)
* [Azure WebJobs SDK Snelzoekgids (PDF-bestand downloaden)](http://go.microsoft.com/fwlink/?LinkID=524028&clcid=0x409)
* [WebJobs instellingen documentatie in GitHub](https://github.com/projectkudu/kudu/wiki/Web-jobs).
* Video 's
    * [WebJobs en de SDK WebJobs](http://channel9.msdn.com/Shows/Cloud+Cover/Episode-153-WebJobs-with-Pranav-Rastogi?utm_source=dlvr.it&utm_medium=twitter)
    * [Azure WebJobs videoreeks op kanaal 9](http://channel9.msdn.com/Tags/azurefridaywebjobs)
    * [Inleiding tot WebJobs gereedschap voor Visual Studio](http://channel9.msdn.com/Shows/Web+Camps+TV/Introducing-WebJobs-Tooling-for-Visual-Studio-with-Brady-Gaster) 
    * [WebJobs Tooling en foutopsporing op afstand](http://channel9.msdn.com/Shows/Web+Camps+TV/WebJobs-GA-Series-Episode-1-WebJobs-Tooling-with-Brady-Gaster)
    * [Azure WebJobs bijwerken met Pranav Rastogi - uitbreiden in versie 1.1](https://channel9.msdn.com/Shows/Cloud+Cover/Episode-183-Azure-WebJobs-Update-with-Pranav-Rastogi)

Zie ook de volgende secties op [WebJobs implementeert](#deploy) en [testen en foutopsporing WebJobs](#debug).

##<a name="deploy"></a>WebJobs implementeren

* [Hoe u de implementatie van Azure WebJobs gebruik van Visual Studio](websites-dotnet-deploy-webjobs.md)
* [Het implementeren van WebJobs met behulp van de Azure-Portal](web-sites-create-web-jobs.md)
* [Opdrachtregel of doorlopend levering van Azure WebJobs inschakelen](https://azure.microsoft.com/blog/2014/08/18/enabling-command-line-or-continuous-delivery-of-azure-webjobs/)
* [Een .NET-console-app implementeren naar Azure met WebJobs cijfer](http://blog.amitapple.com/post/73574681678/git-deploy-console-app/)
* [Een WebJob F # implementeren naar Azure](http://blogs.msdn.com/b/dave_crooks_dev_blog/archive/2015/02/18/deploying-f-web-job-to-azure.aspx)
* [Aangepaste services implementeren als Azure Webjobs](http://withouttheloop.com/articles/2015-06-23-deploying-custom-services-as-azure-webjobs/)
* Video 's
    * [Inleiding tot WebJobs gereedschap voor Visual Studio](http://channel9.msdn.com/Shows/Web+Camps+TV/Introducing-WebJobs-Tooling-for-Visual-Studio-with-Brady-Gaster) 
    * [WebJobs Tooling en foutopsporing op afstand](http://channel9.msdn.com/Shows/Web+Camps+TV/WebJobs-GA-Series-Episode-1-WebJobs-Tooling-with-Brady-Gaster) 

##<a name="schedule"></a>WebJobs plannen

* [Het dialoogvenster van Azure WebJob toevoegen](websites-dotnet-deploy-webjobs.md#configure)
* [Een geplande WebJob maken in de Portal van Azure](web-sites-create-web-jobs.md#CreateScheduled)
* [Een project scheduler om een WebJob te koppelen](http://blog.davidebbo.com/2015/05/scheduled-webjob.html)
* [Azure WebJobs plannen met cron expressies](http://blog.amitapple.com/post/2015/06/scheduling-azure-webjobs/)
* [Afzonderlijke WebJob functies met de WebJobs SDK TimerTrigger plannen](websites-dotnet-webjobs-sdk.md#schedule)

##<a name="debug"></a>Testen en problemen oplossen WebJobs

* [Nieuwe ontwikkelaars en foutopsporing onderdelen voor Azure WebJobs in Visual Studio](http://blogs.msdn.com/b/webdev/archive/2014/11/12/new-developer-and-debugging-features-for-azure-webjobs-in-visual-studio.aspx)
* [Het Dashboard WebJobs bekijken](websites-dotnet-webjobs-sdk-get-started.md#view-the-webjobs-sdk-dashboard)
* [Hoe u met de WebJobs SDK logboeken schrijven en deze weergeven in het Dashboard](websites-dotnet-webjobs-sdk-storage-queues-how-to.md#logs)
* [Externe foutopsporing WebJobs](web-sites-dotnet-troubleshoot-visual-studio.md#remotedebugwj)
* [Wie die blob geschreven?](http://blogs.msdn.com/b/jmstall/archive/2014/02/19/who-wrote-that-blob.aspx) 
* [Hostingprovider interactieve code in de Cloud](http://blogs.msdn.com/b/jmstall/archive/2014/04/26/hosting-interactive-code-in-the-cloud.aspx)
* [Doelcellen toevoegen aan Azure WebJobs](http://blogs.msdn.com/b/mcsuksoldev/archive/2014/09/04/adding-trace-to-azure-web-sites-and-web-jobs.aspx)
* [Bewaken, diagnosticeren en problemen met Microsoft Azure Storage](../storage/storage-monitoring-diagnosing-troubleshooting.md)
* Video 's
    * [WebJobs Tooling en foutopsporing op afstand](http://channel9.msdn.com/Shows/Web+Camps+TV/WebJobs-GA-Series-Episode-1-WebJobs-Tooling-with-Brady-Gaster) 

##<a name="scale"></a>Schaalbaarheid van WebJobs

* [Schaalbaarheid van de webtoepassing met Azure-Websites](http://msdn.microsoft.com/magazine/dn786914.aspx)
* [Azure App-Service: uitbreidt enorme schaal bij de hand-voor-Business-WebApps](https://channel9.msdn.com/Events/Build/2014/3-626). Voorbladen schaalbaarheid van WebApps gebruiken met WebJobs, inclusief de WebJobs SDK.
* Video 's
    * [WebJobs schalen](http://channel9.msdn.com/Shows/Azure-Friday/Azure-WebJobs-105-Scaling-out-Web-Jobs)

##<a name="additional"></a>Extra WebJobs bronnen

* [Azure WebJobs GA blogbericht door Magnus Mårtensson](http://magnusmartensson.com/azure-webjobs-ga)
* [Powershell Web taken uitvoeren op Azure App-Service](http://blogs.msdn.com/b/nicktrog/archive/2014/01/22/running-powershell-web-jobs-on-azure-websites.aspx)
* [Resourceblad wanneer uw Azure geactiveerd WebJobs is voltooid](http://blog.amitapple.com/post/2014/03/webjobs-notification/)
* [Eenvoudige back-up van Web App-bewaarbeleid met WebJobs](https://azure.microsoft.com/blog/2014/04/28/simple-web-site-backup-retention-policy-with-webjobs/)
* [Azure WebApps en Cloudservices traag op eerste verzoek](http://wp.sjkp.dk/windows-azure-websites-and-cloud-services-slow-on-first-request/). Ziet u hoe u met WebJobs simuleren AlwaysOn functie is alleen beschikbaar voor de standaard prijzen laag.
* [WebJobs correcte afsluiten](http://blog.amitapple.com/post/2014/05/webjobs-graceful-shutdown/#.U72Il_5OWUl). Voor WebJobs SDK correcte afsluiten, Zie [correcte afsluiten](websites-dotnet-webjobs-sdk-storage-queues-how-to.md#graceful).)
* [Back-ups met Azure WebJobs & AzCopy automatiseren](http://markjbrown.com/azure-webjobs-azcopy/)
* Video 's
    * [Azure WebJobs-video's door Magnus Mårtensson](https://www.youtube.com/playlist?list=PLqp1ZOYYUSd81yEzMYLTw8cz91wx_LU9r)
    * [Azure WebJobs videoreeks op kanaal 9](http://channel9.msdn.com/Tags/azurefridaywebjobs)

##<a name="additionalsdk"></a>Extra WebJobs SDK bronnen

* [Releaseopmerkingen voor WebJobs SDK](https://github.com/Azure/azure-webjobs-sdk/wiki/Release-Notes)
* [WebJobs SDK-broncode](https://github.com/Azure/azure-webjobs-sdk)
* [Broncode WebJobs SDK extensies](https://github.com/Azure/azure-webjobs-sdk-extensions), met [gedetailleerde uitleg van het uitbreidbaarheidsmodel](https://github.com/Azure/azure-webjobs-sdk-extensions/wiki/Binding-Extensions-Overview).  
* [WebJobs SDK Script-broncode](https://github.com/Azure/azure-webjobs-sdk-script/) (gebruikt voor [Azure functies](../azure-functions/functions-overview.md))
* [WebJob FREB bestanden uploaden met Azure Storage met de WebJobs SDK](http://thenextdoorgeek.com/post/WAWS-WebJob-to-upload-FREB-files-to-Azure-Storage-using-the-WebJobs-SDK)
* [Azure webjobs buiten Azure host, met de logboekregistratie voordelen van een Azure gehost webjob](http://bypassion.dk/?p=510)
* [Samenstellen van een hulpmiddel voor het importeren van gegevens met Azure WebJobs](http://www.freshconsulting.com/building-data-import-tool-azure-webjobs/)
* [Overzicht van Azure functies](../azure-functions/functions-overview.md)
* Video 's
    * [Azure WebJobs videoreeks op kanaal 9](http://channel9.msdn.com/Tags/azurefridaywebjobs)

##<a name="samples"></a>Voorbeeld van de WebJob-toepassingen

* [Voorbeeldtoepassingen verstrekt door het team WebJobs op GitHub](https://github.com/azure/azure-webjobs-sdk-samples)
* [Eenvoudige Azure Web App met WebJobs Backend met de WebJobs SDK](http://code.msdn.microsoft.com/Simple-Azure-Website-with-b4391eeb)
* [SiteMonitR](http://code.msdn.microsoft.com/SiteMonitR-dd4fcf77). Gebruik van de geplande en gebeurtenisgestuurde WebJobs ziet. Zie het blogbericht [de SiteMonitR met behulp van Azure WebJobs SDK PowerPoint opnieuw moet maken](http://www.bradygaster.com/post/rebuilding-the-sitemonitr-using-windows-azure-webjobs).

##<a name="blogs"></a>Blogs

* [Azure-blog](/blog)
* [Amit Apple blog](http://blog.amitapple.com/). Als u richt zich op WebJobs (niet de SDK).
* [Magnus Mårtensson van blog](http://magnusmartensson.com/)

##<a name="gethelp"></a>Hulp bij het WebJobs

* [StackOverflow voor WebJobs](http://stackoverflow.com/questions/tagged/azure-webjobs)
* [StackOverflow voor WebJobs SDK](http://stackoverflow.com/questions/tagged/azure-webjobssdk)
* [StackOverflow voor Azure-functies](http://stackoverflow.com/questions/tagged/azure-functions)
* [Azure en ASP.NET-forum](http://forums.asp.net/1247.aspx)
* [Azure App Service Web Apps-forum](http://social.msdn.microsoft.com/Forums/azure/home?forum=windowsazurewebsitespreview)
* [Azure Web Apps gebruiker spraak-site](https://feedback.azure.com/forums/169385-websites/)
* [Twitter](http://twitter.com/). Gebruik de hashtag #AzureWebJobs.
* [Een WebJobs bug of probleem rapporteren](https://github.com/projectkudu/kudu/wiki/Reporting-WebJobs-issues)

