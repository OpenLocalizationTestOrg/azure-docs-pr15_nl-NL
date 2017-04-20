<properties 
    pageTitle="Hoe u de schaal van een App in een App Service-omgeving" 
    description="Schaalbaarheid van een app in een App Service-omgeving" 
    services="app-service" 
    documentationCenter="" 
    authors="ccompy" 
    manager="stefsch" 
    editor="jimbe"/>

<tags 
    ms.service="app-service" 
    ms.workload="na" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="10/17/2016" 
    ms.author="ccompy"/>

# <a name="scaling-apps-in-an-app-service-environment"></a>Schaalbaarheid van apps in een App Service-omgeving #

Er gewoonlijk drie dingen die kunt u de schaal in de App-Service van Azure:

- prijzen van abonnement
- grootte van de werknemer 
- het aantal exemplaren.

In een ASE is niet nodig om te selecteren of wijzigen van het abonnement waarop prijzen.  Met functies is het al op een Premium prijzen van de mogelijkheid niveau.  

Met betrekking tot de grootte van de werknemer, kan de beheerder ASE toewijzen aan de grootte van de resource berekeningscluster moet worden gebruikt voor elke werknemer-toepassingen.  Dat betekent dat u kunt medewerker toepassingen 1 hebben met P4 berekeningscluster resources en werknemer van toepassingen 2 met P1 berekenen resources, indien gewenst.  Hoeven niet in volgorde van de grootte.  Gedetailleerde informatie om de grootte en hun prijzen Zie het document hier [Azure App serviceprijzen][AppServicePricing].  Dit verlaat de schaalopties voor WebApps en App-Service van abonnement in een App-Service-omgeving moet zijn:

- werknemer van toepassingen selectie
- het aantal exemplaren

Een item wijzigen vindt plaats via de juiste gebruikersinterface voor uw ASE App Service abonnementen gehost weergegeven.  

![][1]

U kunt geen schalen van uw ASP voorbij het aantal beschikbare berekeningscluster resources in de werknemer-toepassingen die uw ASP is in.  Als u resources uit de betreffende groep werknemer moet berekenen moet u uw beheerder ASE ze toevoegt.  Voor informatie over het opnieuw configureren van uw ASE Lees de informatie hier: [het configureren van een App Service-omgeving][HowtoConfigureASE].  U kunt ook om te profiteren van de functies ASE automatisch schalen om toe te voegen capaciteit op basis van planning of aan de doelstellingen.  Voor meer informatie over het configureren van automatisch schalen voor de ASE omgeving zelf [automatisch schalen voor een App-Service-omgeving configureren]Zie[ASEAutoscale].

U kunt meerdere app maken service-abonnementen die berekeningscluster resources uit andere werknemer van toepassingen gebruiken of kunt u dezelfde werknemer van toepassingen gebruiken.  Berekent de resources bijvoorbeeld als u (10) beschikbaar berekeningscluster resources in werknemer van toepassingen 1 hebt, kunt u een app-abonnement (6) berekeningscluster bronnen maken en een tweede app-service plannen die wordt gebruikt (4).

### <a name="scaling-the-number-of-instances"></a>Schaalbaarheid van het aantal exemplaren ###

Wanneer u eerst uw web-app in een omgeving van de Service-App maken wordt gestart 1 exemplaar.  U kunt vervolgens schalen af in extra exemplaren verstrekken aanvullende berekeningscluster resources voor uw app.   

Als uw ASE onvoldoende capaciteit, heeft dan dit heel eenvoudig is.  Gaat u naar uw App-Service plannen waarin de sites die u wilt vergroten en selecteer schaal.  Hiermee wordt de gebruikersinterface waarin u kunt handmatig de schaal instellen voor uw ASP of automatisch schalen regels configureren voor uw ASP geopend.  Handmatig schaalfactor van Stel uw app gewoon de ***schaal door*** aan ***een exemplaar van het aantal die ik handmatig invoeren***.  Hier kunt Sleep de schuifregelaar naar de gewenste hoeveelheid of invoeren in het vak naast de schuifregelaar.  

![][2] 

De regels automatisch schalen ASP in een ASE werken hetzelfde als normaal.  U kunt ***CPU Percentage*** onder ***schaal door te*** selecteren en automatisch schalen regels maken voor uw ASP op basis van Processor Percentage of kunt u complexere regels met behulp van ***regels voor planning en prestaties***.  Om te zien gedetailleerde informatie over het configureren van de handleiding voor het gebruik van automatisch schalen hier [schaal van een app in Azure App Service][AppScale]. 


### <a name="worker-pool-selection"></a>Werknemer van toepassingen selectie ###

Zoals hierboven is, wordt de selectie van de groep werknemer toegankelijk vanaf de ASP-gebruikersinterface.  Open het blad voor de ASP die u wilt schalen en selecteer werknemer van toepassingen.  U ziet alle de werknemer van toepassingen die u hebt geconfigureerd in uw App-omgeving.  Als er slechts één werknemer van toepassingen vervolgens ziet u alleen de ene groep vermeld.  Als u wilt wijzigen welke werknemer van toepassingen uw ASP is in, selecteert u de werknemer-toepassingen die u wilt dat uw App-Service plannen om naar te gaan.  

![][3]

Voordat u uw ASP uit één werknemer groep naar de andere verplaatst is het belangrijk om ervoor te zorgen dat u wordt geschikt zijn voor uw ASP.  In de lijst met groepen van werknemer, niet alleen de naam van de werknemer-toepassingen wordt weergegeven maar u kunt ook zien hoeveel werknemers zijn beschikbaar in die groep werknemer.  Zorg ervoor dat er voldoende exemplaren die beschikbaar zijn voor uw App-Service plannen bevatten.  Als u resources in de werknemer-groep die u verplaatsen wilt naar meer nodig hebt berekenen, klikt u vervolgens uw systeembeheerder ASE om ze te verkrijgen.  

> [AZURE.NOTE] Een ASP uit één werknemer groep veroorzaakt verkoudheid verplaatsen begint van de apps in ASP.  Hierdoor kunt aanvragen traag terwijl uw app koudwatersystemen gestart op de nieuwe berekeningscluster resources is.  Het koudwatersystemen starten kan worden voorkomen met behulp van de [toepassing warme omhoog mogelijkheid] [ AppWarmup] in Azure App-Service.  De initialisatie van de toepassing module in het artikel beschreven werkt ook voor het starten van koudwatersystemen omdat het initialisatieproces ook aangeroepen wordt wanneer apps koudwatersystemen gestart op nieuwe berekeningscluster resources zijn. 

## <a name="getting-started"></a>Aan de slag

Als u wilt beginnen met App-serviceomgevingen, Zie [Hoe naar maken een App-omgeving][HowtoCreateASE]

Zie [Azure App Service]voor meer informatie over het platform Azure App-Service,[AzureAppService].

<!--Image references-->
[1]: ./media/app-service-web-scale-a-web-app-in-an-app-service-environment/aseappscale-aspblade.png
[2]: ./media/app-service-web-scale-a-web-app-in-an-app-service-environment/aseappscale-manualscale.png
[3]: ./media/app-service-web-scale-a-web-app-in-an-app-service-environment/aseappscale-sizescale.png

<!--Links-->
[WhatisASE]: http://azure.microsoft.com/documentation/articles/app-service-app-service-environment-intro/
[ScaleWebapp]: http://azure.microsoft.com/documentation/articles/web-sites-scale/
[HowtoCreateASE]: http://azure.microsoft.com/documentation/articles/app-service-web-how-to-create-an-app-service-environment/
[HowtoConfigureASE]: http://azure.microsoft.com/documentation/articles/app-service-web-configure-an-app-service-environment/
[CreateWebappinASE]: http://azure.microsoft.com/documentation/articles/app-service-web-how-to-create-a-web-app-in-an-ase/
[Appserviceplans]: http://azure.microsoft.com/documentation/articles/azure-web-sites-web-hosting-plans-in-depth-overview/
[AppServicePricing]: http://azure.microsoft.com/pricing/details/app-service/ 
[AzureAppService]: http://azure.microsoft.com/documentation/articles/app-service-value-prop-what-is/
[ASEAutoscale]: http://azure.microsoft.com/documentation/articles/app-service-environment-auto-scale/
[AppScale]: http://azure.microsoft.com/documentation/articles/web-sites-scale/
[AppWarmup]: http://ruslany.net/2015/09/how-to-warm-up-azure-web-app-during-deployment-slots-swap/
