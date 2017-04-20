<properties
   pageTitle="Azure functies overzicht | Microsoft Azure"
   description="Meer informatie over het gebruik van functies van Azure asynchroon werkbelasting optimaliseren in minuten."
   services="functions"
   documentationCenter="na"
   authors="mattchenderson"
   manager="erikre"
   editor=""
   tags=""
   keywords="Azure-functies, functies, verwerking van de gebeurtenis, webhooks, dynamische berekeningscluster, als u kiest architectuur"/>

<tags
   ms.service="functions"
   ms.devlang="multiple"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="multiple"
   ms.workload="na"
   ms.date="08/29/2016"
   ms.author="cfowler;mahender;glenga"/>
   
   
# <a name="azure-functions-overview"></a>Overzicht van Azure functies

Azure functies is een oplossing voor het uitvoeren van eenvoudig kleine stukjes code of 'functies,' in de cloud. U kunt alleen de code die u nodig voor het probleem bij de hand, hebt zonder dat een toepassing voor de hele of de infrastructuur uit te voeren schrijven. Hiermee ontwikkeling efficiënter, en kunt u uw taal ontwikkeling van keuze, zoals C#, F #, Node.js, Python of PHP. Alleen voor de uitvoering van uw code en vertrouwen Azure betalen aan de nieuwe schaal naar wens.

In dit onderwerp biedt een overzicht van Azure-functies. Als u wilt meteen en aan de slag met Azure-functies, begint u met [uw eerste Azure-functie maken](functions-create-first-azure-function.md). Als u voor meer technische informatie over functies zoeken wilt, raadpleegt u [Naslaginformatie voor ontwikkelaars](functions-reference.md).

## <a name="features"></a>Functies

Hier volgen enkele belangrijke functies van Azure-functies:
    
* **Keuze van taal** - schrijven functies met C#, F #, Node.js, Python, PHP, batch, we vaker doen, Java of uitvoerbare bestanden.
* **Betalen per gebruik prijzen model** - betaald wordt alleen voor de bestede tijd uitvoering van de programmacode. De optie dynamische App-Service plannen in de [sectie prijzen](#pricing) hieronder weergegeven.  
* **Uw eigen afhankelijkheden weer** - functies ondersteunt NuGet en NPM, zodat u uw favoriete bibliotheken kunt gebruiken.  
* **Geïntegreerde beveiliging** - functies beveiligen HTTP is geactiveerd met OAuth-providers zoals Azure Active Directory, Facebook, Twitter, Google en Microsoft-Account.  
* **Vereenvoudigd integratie** - eenvoudig gebruikmaken van Azure services en abonnementen voor de software-als-een-service (SaaS). Zie de [sectie integraties](#integrations) hieronder voor enkele voorbeelden.  
* **Flexibele development** - Code van de functies rechts in de portal of doorlopend integratie instellen en implementeren van uw code via GitHub, Visual Studio Team Services en andere [Hulpmiddelen voor de ontwikkeling ondersteund](../app-service-web/web-sites-deploy.md#deploy-using-an-ide).  
* De **open-source** - runtime van de functies is open source en [beschikbaar op GitHub](https://github.com/azure/azure-webjobs-sdk-script).  

## <a name="what-can-i-do-with-functions"></a>Wat kan ik doen met de functies?

Azure functies is een goede oplossing voor de van verwerkingsgegevens, systemen, integratie met de internet-van-zaken aan bod (IoT) werkt en bouwen van eenvoudige API's en microservices. Houd rekening met functies voor taken, zoals een afbeelding of volgorde verwerken, onderhoud van bestanden, langdurige taken die u wilt uitvoeren in een achtergrondthread of voor alle taken die u wilt uitvoeren op een planning. 

Functies bevat sjablonen om mee aan de slag met belangrijke scenario's, waaronder de volgende:

* **BlobTrigger** - opslag van proces Azure BLOB's wanneer ze worden toegevoegd aan containers. U kunt dit gebruiken voor de grootte van afbeeldingen aanpassen.
* **EventHubTrigger** - reageren op gebeurtenissen die wordt afgeleverd in een Hub Azure-gebeurtenis. Met name handig in toepassing instrumentation, gebruiker ervaring of werkstroom verwerking en scenario's voor Internet van dingen (IoT).
* **Algemene webhook** - proces webhook HTTP vraagt van elke service die ondersteuning biedt voor webhooks.
* **GitHub webhook** - reageren op gebeurtenissen die zich bij uw opslagplaatsen GitHub voordoen. Zie [maken een webhook of API-functie](functions-create-a-web-hook-or-api-function.md)voor een voorbeeld.
* **HTTPTrigger** - Trigger uw code met behulp van een HTTP-aanvraag wordt uitgevoerd.
* **QueueTrigger** - reageren op berichten die in een wachtrij Azure Storage aankomen. Zie voor een voorbeeld [maken een Azure-functie die wordt gebonden aan een Azure-service](functions-create-an-azure-connected-function.md).
* **ServiceBusQueueTrigger** - uw code verbinden met andere services Azure of on-premises services door te luisteren naar berichtenwachtrijen. 
* **ServiceBusTopicTrigger** - uw code verbinden met andere services Azure of on-premises services door u te abonneren naar onderwerpen. 
* **TimerTrigger** - opruimen of andere batchtaken uit te voeren op een vooraf gedefinieerde planning. Zie [een gebeurtenis verwerken van de functie maken](functions-create-an-event-processing-function.md)voor een voorbeeld.

Azure functies ondersteunt *triggers*, die zijn manieren om te beginnen uitvoering van de code, en *Bindingen*, die zijn manieren om te vereenvoudigen coderen voor invoer- en uitvoerbereik gegevens. Zie voor een gedetailleerde beschrijving van de triggers en bindingen vindt u Azure-functies, [Azure functies triggers en naslaginformatie voor ontwikkelaars van bindingen](functions-triggers-bindings.md).


## <a name="integrations"></a>Integraties

Azure functies geïntegreerd met een groot aantal Azure en 3e derden services. U kunt deze aan uw functie activeren en uitvoering starten of als invoer dienen en uitvoer voor uw code. De volgende service integraties worden door Azure-functies ondersteund. 

* Azure DocumentDB
* Azure gebeurtenis Hubs 
* Azure Mobile-Apps (tabellen)
* Azure melding Hubs
* Azure-Service Bus (wachtrijen en onderwerpen)
* Azure opslag (blob, wachtrijen en tabellen) 
* GitHub (webhooks)
* On-premises implementatie (via Service Bus)

## <a name="pricing"></a>Hoeveel kost functies kosten?

Azure functies heeft twee soorten abonnementen prijzen, kiest u een die het beste aan uw wensen voldoet: 

* **Dynamische hostingprovider abonnement** - wanneer uw functie wordt uitgevoerd, Azure vindt u alle benodigde rekenkundige bronnen. U hoeft te bang resourcebeheer en u betaalt alleen voor het moment dat de programmacode wordt uitgevoerd. Volledige prijsgegevens zijn beschikbaar op de [pagina functies prijzen](/pricing/details/functions). 

* **App-serviceplan** - uw functies net als uw web, mobile en API-apps worden uitgevoerd. Als u al App Service voor uw andere toepassingen gebruiken, kunt u uw functies uitvoeren op hetzelfde plan gratis. Uitgebreide informatie vindt u op de [pagina prijzen van App-Service](/pricing/details/app-service/).

Zie voor meer informatie over de functies schaalbaarheid, [hoe u de schaal van Azure-functies](functions-scale.md).

##<a name="next-steps"></a>Volgende stappen

+ [Maak uw eerste Azure-functie](functions-create-first-azure-function.md)  
Meteen in en maak uw eerste functie met de Snelstartgids Azure-functies. 
+ [Azure naslaginformatie voor ontwikkelaars van functies](functions-reference.md)  
Biedt meer technische informatie over de functies van Azure-runtime en een verwijzing voor kleurcodering aan functies en triggers en bindingen definiëren.
+ [Azure functies testen](functions-test-a-function.md)  
Diverse hulpprogramma's en technieken voor het testen van de functies beschreven.
+ [Hoe u de schaal van Azure-functies](functions-scale.md)  
Wordt beschreven hoe service-abonnementen die beschikbaar zijn met Azure-functies, inclusief de dynamische serviceplan en hoe u om de juiste abonnement te kiezen. 
+ [Meer informatie over Azure App-Service](../app-service/app-service-value-prop-what-is.md)  
Azure-functies worden gebruikt in het platform Azure App-Service voor de belangrijkste functies zoals implementaties, omgevingsvariabelen en diagnostische gegevens. 