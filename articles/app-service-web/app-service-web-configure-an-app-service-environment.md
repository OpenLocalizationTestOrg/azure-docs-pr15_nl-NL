<properties
    pageTitle="Het configureren van een App Service-omgeving | Microsoft Azure"
    description="Configuratie, beheer en controle van App-serviceomgevingen"
    services="app-service"
    documentationCenter=""
    authors="ccompy"
    manager="stefsch"
    editor=""/>

<tags
    ms.service="app-service"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/17/2016"
    ms.author="ccompy"/>


# <a name="configuring-an-app-service-environment"></a>Een App Service-omgeving configureren #

## <a name="overview"></a>Overzicht ##

Op hoog niveau, wordt een Azure App Service-omgeving bestaat uit verschillende hoofdonderdelen:

- Resources die worden uitgevoerd in de App-omgeving die worden gehost service berekenen
- Opslag
- Een database
- Een virtueel netwerk Classic(V1) of Resource Manager(V2) Azure (VNet) 
- Een subnet met de App-omgeving die worden gehost service wordt uitgevoerd in deze

### <a name="compute-resources"></a>Resources berekenen

U gebruikt de berekeningscluster resources voor uw vier resourcegroepen.  Elke App Service-omgeving (ASE) heeft een set van front-einden en drie mogelijke werknemer van toepassingen. U hoeft te gebruiken van alle drie werknemer van toepassingen--als u wilt, kunt u alleen gebruiken een of twee.

De hosts in de resourcegroepen (voorste uiteinden en werknemers) zijn niet rechtstreeks toegankelijk met tenants. U kunt geen Remote Desktop Protocol (RDP) kunt u hiermee verbinding maken, wijzigen de inrichting van of gedragen zich als beheerder van deze.

U kunt instellen hoeveelheid bronnen voor toepassingen en -grootte. In een ASE moet u vier grootte opties, die P1 tot en met P4 bevatten. Zie de [App Service prijzen](../app-service/app-service-value-prop-what-is.md)voor meer informatie over deze bestandsgrootten en hun prijzen.
De hoeveelheid of de tekengrootte wijzigen, is een bewerking schaal genoemd.  Slechts één schaal bewerking kan worden uitgevoerd op een moment.

**Voorgrond eindigt**: de front-einden zijn de HTTP-/ HTTPS-eindpunten voor uw apps in uw ASE worden geplaatst. U kunt werkbelasting niet uitvoeren in de front-einden.

- Een ASE begint met twee P2s, welke is voldoende voor ontwikkelaar/testen werkbelasting en laag niveau productie werkbelasting. Wij raden P3s voor normaal naar dik productie werkbelasting.
- Voor normaal tot dik productie werkbelasting, is het raadzaam dat er ten minste vier P3s om ervoor te zorgen er voldoende voorste uiteinden wanneer gepland onderhoud plaatsvindt. Gepland onderhoudsactiviteiten brengt omlaag één front-end tegelijk. Hierdoor totale beschikbare front capaciteit tijdens onderhoudsactiviteiten.
- U kunt een nieuw exemplaar van de front niet direct toevoegen. Ze kunnen volgen tussen 2 tot en met 3 uur duren.
- Voor verdere schaal instellen, moet u het percentage van de processor, geheugenpercentage en actieve aanvragen aan de doelstellingen voor de front-toepassingen controleren. Als de percentages CPU of geheugen boven 70 procent wanneer u zich P3s, voegt u meer voorste uiteinden toe. Als de waarde van de actieve aanvragen af naar 15.000 aan 20.000 aanvragen per front-end van gemiddelden, moet u ook meer voorste uiteinden toevoegen. De algehele doel is om te houden van processor en geheugen percentages onderstaande 70% en actieve aanvragen gemiddelde naar onder 15.000 aanvragen per voorgrond beëindigen wanneer u werkt met het P3s.  

**Werknemers**: de werknemers zijn waar uw apps werkelijk uitvoeren. Wanneer u van uw App-Service plannen wilt verkleinen, maakt die het gebruik van werknemers in de groep gekoppelde werknemer.

- U kunt geen direct werknemers toevoegen. Ze kunt uitvoeren vanuit 2 tot 3 uur duren, ongeacht het aantal worden toegevoegd.
- Schaalbaarheid van de grootte van een berekeningscluster resource voor alle toepassingen duurt 2 tot en met 3 uur per update domain. Er zijn 20 update domeinen in een ASE. Als u de schaal van de grootte van het berekenen van een medewerker van toepassingen met 10 exemplaren aangepast, is het duurt tussen 30 20 uur om te voltooien.
- Als u de grootte van de bronnen voor berekeningscluster die worden gebruikt in een groep werknemer wijzigt, wordt u koudwatersystemen begint van de apps die worden uitgevoerd in die groep werknemer.

De snelste manier om te wijzigen van de grootte van de resource berekenen van een medewerker van toepassingen die niet actief is voor apps is:

- Het aantal exemplaar 0 verkleinen. Het duurt ongeveer 30 minuten uw exemplaren de toewijzing.
- Selecteer de nieuwe berekeningscluster grootte en het aantal exemplaren. U kunt duurt het tussen 2 tot en met 3 uur om te voltooien.

Als uw apps berekeningscluster resource groter vereist, niet kunt u profiteren van de vorige richtlijnen. In plaats van de grootte van de werknemer van toepassingen die als host deze apps fungeert wijzigen, kunt u een andere werknemer toepassingen met werknemers van de gewenste grootte te vullen en uw apps naar die groep verplaatsen.

- De extra exemplaren van de grootte van de benodigde berekeningscluster maken in een andere werknemer-toepassingen. Dit zal duren van 2 tot 3 uur.
- Opnieuw toewijzen uw App-Service-abonnementen waarop de apps die voor een groter aan de groep nieuw geconfigureerde werknemer nodig zijn. Dit is een snelle bewerking moet minder dan een minuut in beslag nemen.  
- De eerste werknemer van toepassingen verkleinen als u niet meer nodig hebt die niet-gebruikte exemplaren. Deze bewerking duurt ongeveer 30 minuten om te voltooien.

**AutoScaling**: een van de hulpmiddelen die u voor het beheren van uw verbruik van berekeningscluster kunnen helpen autoscaling is. U kunt autoscaling voor front of werknemer van toepassingen. U kunt doen zoals grotere uw exemplaren van elk type groep in de ochtend en verkleinen in de buiten kantooruren. Of misschien kunt u exemplaren toevoegen wanneer het aantal werknemers die beschikbaar in een groep werknemer zijn lager is dan een bepaalde drempel.

Als u wilt u automatisch schalen regels rond de doelstellingen van de groep resource berekeningscluster instellen en houd rekening met het moment dat de inrichting van is vereist. Zie voor meer informatie over autoscaling App serviceomgevingen, [automatisch schalen in een omgeving App-Service configureren][ASEAutoscale].

### <a name="storage"></a>Opslag

Elke ASE is geconfigureerd met 500 GB opslagruimte. Deze ruimte wordt gebruikt in alle apps in de ASE. Deze opslagruimte is een onderdeel van de ASE en momenteel kan niet worden veranderd als u wilt gebruiken, uw opslagruimte. Als u aanpassingen aan de routering van virtueel netwerk- of beveiliging aanbrengt, moet u nog steeds toegang geeft tot Azure Storage-- of de ASE niet goed.

### <a name="database"></a>Database

De database bevat de informatie die definieert de omgeving, evenals de details van de apps die erin worden uitgevoerd. Dit is ook een deel van het abonnement Azure aangehouden. Het is niet iets dat u hebt een directe mogelijkheid om te bewerken. Als u aanpassingen aan de routering van virtueel netwerk- of beveiliging aanbrengt, moet u nog steeds toegang geeft tot SQL Azure-- of de ASE niet goed.

### <a name="network"></a>Netwerk

De VNet die wordt gebruikt met uw ASE kan zijn dat u hebt aangebracht wanneer u de ASE gemaakt of dat u tijd vooraf hebt aangebracht. Wanneer u het subnet tijdens het maken van ASE maakt, is deze dwingt het ASE moeten in dezelfde resourcegroep als het virtuele netwerk. Als u de resourcegroep die wordt gebruikt door uw ASE voor afwijken van uw VNet nodig hebt, moet u uw ASE met een resource manager-sjabloon maken.

Er gelden enkele beperkingen voor het virtuele netwerk dat wordt gebruikt voor een ASE:
 
- Het virtuele netwerk moet een regionale virtueel netwerk.
- Er moet een subnet met 8 of meer adressen waar de ASE wordt geïmplementeerd.
- Nadat een subnet wordt gebruikt voor het hosten van een ASE, kan het adresbereik van de subnet kan niet worden gewijzigd. Om die reden is het raadzaam dat het subnet ten minste 64 adressen eventuele toekomstige groei voor ASE bevat.
- Kunnen er niets in het subnet, maar de ASE.

In tegenstelling tot de gehoste service met de ASE, het [virtuele netwerk] [ virtualnetwork] en subnet zijn onder Gebruikersbeheer.  U kunt uw virtuele netwerk via het virtuele netwerk UI of PowerShell beheren.  Een ASE kan worden geïmplementeerd in een Classic of Resource Manager VNet.  De portal en API ervaringen zijn iets anders tussen Classic en resourcemanager VNets maar de ASE-ervaring is dezelfde.

De VNet die wordt gebruikt voor het hosten van een ASE kunt beide particuliere RFC1918 IP-adressen of het openbare IP-adressen kunt gebruiken.  Als u wilt gebruiken een IP-bereik dat niet wordt gedekt door RFC1918 (10.0.0.0/8, 172.16.0.0/12, 192.168.0.0/16) en vervolgens u dient te maken van uw VNet en subnet moet worden gebruikt door uw ASE ligt ASE maken.

Omdat deze mogelijkheid de App-Service van Azure uw virtuele netwerk wordt, betekent dit dat uw apps die worden gehost in uw ASE hebt nu toegang tot resources die rechtstreeks beschikbaar via de ExpressRoute of site-naar-site virtuele particuliere netwerken (VPN's) worden aangebracht. De apps die binnen uw App Service-omgeving zijn nodig geen aanvullende netwerken functies voor toegang tot bronnen die beschikbaar zijn bij het virtuele netwerk die als host uw App Service-omgeving fungeert. Dit betekent dat u niet VNET integratie of een RAS-hybride gebruiken moet voor toegang tot resources, of niet verbonden met het netwerk van uw virtuele. U kunt beide van deze functies nog steeds gebruiken hoewel voor toegang tot bronnen in netwerken te gebruiken die niet zijn gekoppeld aan uw virtuele netwerk.

U kunt bijvoorbeeld VNET integratie integreren met een virtueel netwerk die zich in uw abonnement, maar niet is gekoppeld aan het virtuele netwerk dat uw ASE zich in. U kunt nog steeds ook hybride verbindingen voor toegang tot bronnen die in andere netwerken zijn, net zoals u normaal gesproken kunt gebruiken.  

Als u uw virtuele netwerk is geconfigureerd met een VPN-ExpressRoute hebt, moet u zich bewust zijn van een aantal de routering behoeften die een ASE heeft. Er zijn enkele gebruiker gedefinieerde route (UDR)-configuraties die incompatibel met een ASE zijn. Voor meer informatie over het uitvoeren van een ASE in een virtueel netwerk met ExpressRoute, raadpleegt [uitvoeren van een App-Service-omgeving in een virtueel netwerk met ExpressRoute][ExpressRoute].

#### <a name="securing-inbound-traffic"></a>Binnenkomende verkeer beveiligen

Er zijn twee primaire methoden voor het beheren van binnenkomende verkeer naar uw ASE.  U kunt netwerk beveiliging Groups(NSGs) gebruiken om te bepalen welke IP adressen toegang heeft tot uw ASE zoals hier wordt beschreven [instellen met binnenkomende verkeer in een App-Service-omgeving](app-service-app-service-environment-control-inbound-traffic.md) en u kunt ook uw ASE configureren met een interne laden Balancer(ILB).  Deze functies kunnen ook samen worden gebruikt als u wilt beperken van toegang tot een NSGs naar uw ASE ILB.

Wanneer u een ASE maakt, wordt deze een VIP maken in uw VNet.  Er zijn twee VIP typen, interne en externe.  Wanneer u een ASE met een externe VIP maken vervolgens zijn uw apps in uw ASE toegankelijk via een internet geschikt IP-adres. Wanneer u interne uw ASE selecteert, worden geconfigureerd met een ILB en worden niet rechtstreeks internet toegankelijk zijn.  Een ASE ILB nog steeds een externe VIP vereist, maar het wordt alleen gebruikt voor Azure toegang voor beheer en onderhoud.  

Tijdens het maken van ILB ASE die u kunt het subdomein dat wordt gebruikt door de ASE ILB bieden en wordt hebt voor het beheren van uw eigen DNS-records voor het subdomein dat u opgeeft.  Omdat u de naam van het subdomein dat u ook nodig hebt voor het beheren van het certificaat dat wordt gebruikt voor HTTPS-toegang instellen.  Na het maken van de ASE wordt u gevraagd om te leveren van het certificaat.  Voor meer informatie over het maken en gebruiken van een ASE ILB gelezen [met van een interne taakverdeling met een App-Service-omgeving][ILBASE]. 


## <a name="portal"></a>Portal

U kunt beheren en uw App Service-omgeving controleren met behulp van de gebruikersinterface in de portal van Azure. Als u een ASE hebt, klikt u vervolgens bent u waarschijnlijk het symbool voor de App-Service op uw sidebar zien. Dit symbool wordt gebruikt om aan te geven van de Service-omgevingen App in de portal van Azure:

![Symbool voor App-Service-omgeving][1]

Als u wilt openen in de gebruikersinterface waarin alleoffice van uw App serviceomgevingen, kunt u het pictogram gebruiken of selecteert u de dubbele punthaak (">" symbool) onderaan in de zijbalk App serviceomgevingen selecteren. Als u een van de ASEs wordt vermeld, opent u de gebruikersinterface die wordt gebruikt om te controleren en deze te beheren.

![Gebruikersinterface voor controle en beheer van uw App Service-omgeving][2]

Deze eerste blade ziet u enkele eigenschappen van uw ASE, samen met een metrische grafiek per resourcegroep. Enkele eigenschappen die worden weergegeven in de blokkering **Essentials** zijn ook hyperlinks waarop het blad dat is gekoppeld aan dit wordt geopend. Bijvoorbeeld, kunt u de **Virtuele** netwerknaam om de gebruikersinterface die is gekoppeld aan het virtuele netwerk waarop uw ASE wordt uitgevoerd in te openen. **App-Service-abonnementen** en **Apps** geopend bladen die deze items die in uw ASE vermelden.  

### <a name="monitoring"></a>Cmdlets voor controle

De grafieken kunnen u allerlei prestatiegegevens in elke resourcegroep. Voor de front-toepassingen, kunt u het gemiddelde processor en geheugen controleren. Voor werknemer van toepassingen, kunt u de hoeveelheid die is gebruikt en het aantal dat is beschikbaar controleren.

Meerdere App-Service plannen kunnen maken gebruik van de werknemers in een resourcegroep die werknemer. De werklast is niet verdeeld op dezelfde manier als met de front-servers, dus het gebruik van processor en geheugen niet veel leveren heeft op het gebied nuttige informatie. Het is belangrijker om bij te houden hoeveel werknemers die u hebt gebruikt en zijn beschikbaar, met name als u dit systeem voor andere gebruikers beheert.  

U kunt ook alle de criteria die kunnen worden bijgehouden in de grafieken gebruiken voor het instellen van waarschuwingen. Instellen van waarschuwingen hier werkt hetzelfde als ergens anders in de App-Service. U kunt een melding instellen vanuit het **waarschuwingen** UI gedeelte of inzoomen op parameters UI en **Melding toevoegen**te selecteren.

![Aan de doelstellingen UI][3]

De criteria die werden alleen besproken zijn de doelstellingen van de App-omgeving. Er zijn ook aan de doelstellingen die beschikbaar op het niveau van de App Service-abonnement zijn. Dit is waar voor controle van processor en geheugen zorgt ervoor dat een groot aantal komen.

In een ASE zijn alle van de App Service-abonnementen speciale App Service-abonnementen. Dit betekent dat het alleen apps die worden uitgevoerd op de hosts toegewezen aan dat App serviceplan vind ik de apps in die App serviceplan. Om te zien details van uw App Service-abonnement, Geef uw App serviceplan vanaf een van de lijsten in de gebruikersinterface van ASE of **Blader App Service-abonnementen** (waarin u ze alle).   

### <a name="settings"></a>Instellingen

Binnen het blad ASE is er een sectie met **Instellingen** die diverse belangrijke mogelijkheden bevat:

**Instellingen** > **Eigenschappen**: het blad **Instellingen** automatisch wordt geopend wanneer u uw blade ASE weer. Aan de bovenkant is **Eigenschappen**. Er zijn een aantal items in taken die overtollige zijn aan wat u in **Essentials**te zien, maar wat is bijzonder nuttig zijn is **Virtual IP-adres**, evenals de **Uitgaande IP-adressen**.

![Instellingen blade en eigenschappen][4]

**Instellingen** > **IP-adressen**: wanneer u een IP-Secure Sockets Layer (SSL)-app in uw ASE maakt, moet u een SSL IP-adres. Voor het verkrijgen van een, moet uw ASE IP SSL adressen die deze eigenaar is van die kunnen worden toegewezen. Wanneer een ASE is gemaakt, wordt er één SSL IP-adres voor dit doel, maar u kunt meer toevoegen. Er is een boete voor aanvullende IP SSL adressen, zoals wordt weergegeven in de [App-Service prijzen] [ AppServicePricing] (in het gedeelte over het SSL-verbindingen). De extra prijs is de prijs IP SSL.

**Instellingen** > **Front-Endpool** / **Werknemer van toepassingen**: elk van deze resource van toepassingen bladen biedt de mogelijkheid om informatie te zien alleen op deze resourcegroep, naast de besturingselementen als volledig wilt verkleinen dat resourcegroep.  

Het grondtal blad voor elke resourcegroep vindt u een grafiek met doelstellingen voor die resourcegroep. Net als met de grafieken uit het blad ASE eventueel dieper op de grafiek en instellen van waarschuwingen naar wens. Een waarschuwing instellen van het blad ASE voor een specifieke resourcegroep, doet hetzelfde als u dit doen vanuit de resourcegroep. In het blad werknemer groep **Instellingen** , u toegang hebt tot alle Apps of App Service-abonnementen die in deze groep werknemer worden uitgevoerd.

![Instellingen voor werknemer van toepassingen UI][5]

### <a name="portal-scale-capabilities"></a>Mogelijkheden voor portal schaal  

Er zijn drie schaal bewerkingen:

- Aantal IP-adressen in de ASE die beschikbaar voor IP SSL gebruik zijn wijzigen.
- De grootte van de resource berekeningscluster die wordt gebruikt in een resourcegroep wijzigen.
- Aantal berekeningscluster resources die worden gebruikt in een resourcegroep, handmatig of via autoscaling wijzigen.

Er zijn drie manieren om te bepalen hoeveel servers die u in uw resourcegroepen hebt in de portal:

- Een bewerking schaal van het belangrijkste ASE blad aan de bovenkant. U kunt meerdere schaal configuratiewijzigingen aanbrengen aan de front-end en werknemer van toepassingen. Deze worden toegepast als een enkele bewerking.
- Een bewerking handmatige schaal van het individuele resource groep **schaal** blad, dat wil onder **Instellingen zeggen**.
- AutoScaling u uit het blad individuele resource groep **schaal** hebt ingesteld.

Als u wilt gebruiken met de bewerking schaal op het blad ASE, Sleep de schuifregelaar naar de hoeveelheid en sla. Deze gebruikersinterface ondersteunt ook de grootte wijzigen.  

![Schaal UI][6]

Als u wilt de handmatig of automatisch schalen mogelijkheden gebruiken in een specifieke resourcegroep, gaat u naar **Instellingen** > **Front-Endpool** / **Werknemer van toepassingen** zo nodig. Open de groep die u wilt wijzigen. Ga naar **Instellingen** > **schaal af** of **Instellingen** > **vergroten**. Het blad **Schalen Out** kunt u bepalen exemplaar hoeveelheid. Kunt u de grootte van de resource **Schaal omhoog** .  

![Schaalinstellingen UI][7]

## <a name="fault-tolerance-considerations"></a>Aandachtspunten voor de fouttolerantie

U kunt een App-Service-omgeving om maximaal 55 totale berekeningscluster resources te gebruiken. Van deze 55 berekeningscluster resources, kan alleen 50 worden gebruikt voor host werkbelasting. De reden hiervoor is tweeledig. Er is een minimum van 2 front berekeningscluster resources.  Die verlaat maximaal 53 ter ondersteuning van de toewijzing voor de werknemer-toepassingen. Kan alleen worden aangeboden fouttolerantie, moet u beschikken over een extra berekeningscluster resource die is toegewezen op basis van de volgende regels:

- Elke werknemer van toepassingen moet ten minste 1 extra berekeningscluster resource die niet beschikbaar is voor een werkbelasting worden toegewezen.
- Wanneer de hoeveelheid berekeningscluster resources in een groep werknemer boven een bepaalde waarde gaat, klikt u vervolgens is een andere resource van berekeningscluster vereist voor fouttolerantie. Dit is niet het geval in de front-groep.

Binnen elke groep één werknemer zijn de vereisten fouttolerantie die voor een bepaalde waarde van X toegewezen resources aan een werknemer-groep:

- Als X tussen 2 en 20, is de hoeveelheid best berekeningscluster bronnen die u voor werkbelasting gebruiken kunt X-1.
- Als X tussen 21 en 40, is de hoeveelheid bruikbaar berekeningscluster bronnen die u voor werkbelasting gebruiken kunt X-2.
- Als X tussen 41 en 53, is de hoeveelheid best berekeningscluster bronnen die u voor werkbelasting gebruiken kunt X-3.

De minimale milieu heeft 2 front-servers en 2 werknemers.  Met de bovenstaande beweringen Klik volgen hier enkele voorbeelden om aan te geven:  

- Hebt u 30 werknemers in een één groep, kan 28 deze worden gebruikt voor host werkbelasting.
- Als u 2 werknemers in een enkele groep hebt, kan 1 worden gebruikt voor host werkbelasting.
- Hebt u 20 werknemers in een één groep, kan 19 worden gebruikt voor host werkbelasting.  
- Hebt u 21 werknemers in een één groep, kan vervolgens nog steeds alleen 19 worden gebruikt voor host werkbelasting.  

Het aspect fouttolerantie belangrijk is, maar u moet deze rekening mee houden terwijl u de schaal van boven bepaalde drempels. Als u meer capaciteit van 20 exemplaren toevoegen wilt, klikt u vervolgens gaat u naar 22 of hoger omdat 21 eventuele meer capaciteit niet toevoegen. Dit geldt gaan boven 40, waar het nummer van de volgende die worden toegevoegd capaciteit 42 is.  

## <a name="deleting-an-app-service-environment"></a>Verwijderen van een App Service-omgeving ##

Als u verwijderen van een App-Service-omgeving wilt, klikt u vervolgens gebruiken de actie **verwijderen** boven aan het blad App-omgeving. Wanneer u dit doet, wordt u gevraagd om in te voeren, de naam van uw omgeving van de Service-App te bevestigen dat u echt wilt u dit wilt doen. Houd er rekening mee dat wanneer u een App-Service-omgeving verwijdert, u alle als u ook de inhoud erin verwijderen.  

![Een App Service-omgeving UI verwijderen][9]  

## <a name="getting-started"></a>Aan de slag

Als u wilt beginnen met App-serviceomgevingen, raadpleegt u [het maken van een App-Service-omgeving](app-service-web-how-to-create-an-app-service-environment.md).

Zie voor meer informatie over het platform Azure App-Service, [Azure App-Service](../app-service/app-service-value-prop-what-is.md).

[AZURE.INCLUDE [app-service-web-whats-changed](../../includes/app-service-web-whats-changed.md)]

[AZURE.INCLUDE [app-service-web-try-app-service](../../includes/app-service-web-try-app-service.md)]

<!--Image references-->
[1]: ./media/app-service-web-configure-an-app-service-environment/ase-icon.png
[2]: ./media/app-service-web-configure-an-app-service-environment/aseconfig-aseblade.png
[3]: ./media/app-service-web-configure-an-app-service-environment/aseconfig-poolchart.png
[4]: ./media/app-service-web-configure-an-app-service-environment/aseconfig-properties.png
[5]: ./media/app-service-web-configure-an-app-service-environment/aseconfig-poolblade.png
[6]: ./media/app-service-web-configure-an-app-service-environment/aseconfig-scalecommand.png
[7]: ./media/app-service-web-configure-an-app-service-environment/aseconfig-poolscale.png
[8]: ./media/app-service-web-configure-an-app-service-environment/aseconfig-pricingtiers.png
[9]: ./media/app-service-web-configure-an-app-service-environment/aseconfig-deletease.png

<!--Links-->
[WhatisASE]: http://azure.microsoft.com/documentation/articles/app-service-app-service-environment-intro/
[Appserviceplans]: http://azure.microsoft.com/documentation/articles/azure-web-sites-web-hosting-plans-in-depth-overview/
[HowtoCreateASE]: http://azure.microsoft.com/documentation/articles/app-service-web-how-to-create-an-app-service-environment/
[HowtoScale]: http://azure.microsoft.com/documentation/articles/app-service-web-scale-a-web-app-in-an-app-service-environment/
[ControlInbound]: http://azure.microsoft.com/documentation/articles/app-service-app-service-environment-control-inbound-traffic/
[virtualnetwork]: https://azure.microsoft.com/documentation/articles/virtual-networks-faq/
[AppServicePricing]: http://azure.microsoft.com/pricing/details/app-service/
[AzureAppService]: http://azure.microsoft.com/documentation/articles/app-service-value-prop-what-is/
[ASEAutoscale]: http://azure.microsoft.com/documentation/articles/app-service-environment-auto-scale/
[ExpressRoute]: http://azure.microsoft.com/documentation/articles/app-service-app-service-environment-network-configuration-expressroute/
[ILBASE]: http://azure.microsoft.com/documentation/articles/app-service-environment-with-internal-load-balancer/
