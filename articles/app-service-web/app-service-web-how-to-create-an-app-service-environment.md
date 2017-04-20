<properties 
    pageTitle="Het maken van een App Service-omgeving" 
    description="Beschrijving van de stroom maken voor app-serviceomgevingen" 
    services="app-service" 
    documentationCenter="" 
    authors="ccompy" 
    manager="stefsch" 
    editor=""/>

<tags 
    ms.service="app-service" 
    ms.workload="web" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/22/2016" 
    ms.author="ccompy"/>

# <a name="how-to-create-an-app-service-environment"></a>Het maken van een App Service-omgeving #

### <a name="overview"></a>Overzicht ###

App Service omgevingen (ASE) zijn de optie voor een Premium-service van Azure App-Service biedt de mogelijkheid van een verbeterde die niet beschikbaar is in de stempelen meerdere tenant.  De functie ASE implementeert in feite de App-Service van Azure virtuele netwerk van een klant.  Een beter inzicht in de mogelijkheden die door de App serviceomgevingen lezen de [Wat is een App-Service-omgeving] worden aangeboden krijgen[ WhatisASE] documentatie.

### <a name="before-you-create-your-ase"></a>Voordat u uw ASE maken ###

Het is belangrijk om te weten wat die u kunt niet wijzigen.  Er zijn die elementen die u niet over uw ASE wijzigen nadat deze is gemaakt:

- Locatie
- Abonnement
- Resourcegroep
- VNet gebruikt
- Subnet gebruikt 
- Subnet grootte

Wanneer een VNet te kiezen en op te geven van een subnet, zorgen hoeft u groot genoeg voor eventuele toekomstige groei.  

### <a name="creating-an-app-service-environment"></a>Maken van een App Service-omgeving ###

Er zijn twee manieren voor toegang tot de aanmaakdatum ASE UI.  Het kan zijn gevonden door te zoeken in de Azure Marketplace ***App-omgeving*** of door nieuw te doorlopen -> Web + Mobile -> App-omgeving.  Een ASE maken:

1. Geef de naam van uw ASE.  De naam die is opgegeven voor de ASE wordt gebruikt voor de apps die is gemaakt in de ASE.  Als naam van de ASE appsvcenvdemo vervolgens wordt de subdomeinnaam. *appsvcenvdemo.p.azurewebsites.net*.  Als u een app met de naam *mytestapp* dus gemaakt zou vervolgens deze worden gebruikt bij *mytestapp.appsvcenvdemo.p.azurewebsites.net*.  U kunt witruimte niet gebruiken op de naam van uw ASE.  Als u hoofdletters in de naam gebruikt, zijn de domeinnaam de totale kleine letters versie van die naam.  Als u een ILB vervolgens de naam van uw ASE wordt niet gebruikt in een subdomein maar in plaats daarvan expliciet is opgegeven tijdens het maken van ASE

    ![][1]

2. Selecteer uw abonnement.  Het abonnement dat is gebruikt voor uw ASE is ook een die alle apps in die ASE worden gemaakt met.  U kunt uw ASE in een VNet die zich in een ander abonnement plaatsen

3. Selecteer of geef een nieuwe resourcegroep.  De resourcegroep die wordt gebruikt voor uw ASE moeten hetzelfde die wordt gebruikt voor uw VNet.  Als u een bestaande VNet selecteert wordt vervolgens de selectie van de groep resources voor uw ASE worden bijgewerkt met die van uw VNet.

    ![][2]

4. Controleer uw selecties Virtual Network en locatie.  U kunt een nieuwe VNet maken of selecteren van een bestaande VNet.  Als u een nieuwe VNet selecteren en vervolgens kunt u een naam en locatie opgeven. De nieuwe VNet heeft het adres bereik 192.268.250.0/23 en een subnet **standaard** die is gedefinieerd als 192.168.250.0/24 met de naam.  U kunt ook gewoon een vooraf bestaande Classic of Resource Manager VNet selecteren.  De selectie VIP-Type bepaalt als uw ASE rechtstreeks zijn toegankelijk vanaf internet (extern) of als de site gebruikmaakt van een verdeling interne laden (ILB).  Voor meer informatie over deze gelezen [met van een interne taakverdeling met een App-Service-omgeving][ILBASE].  Als u een type VIP extern selecteert kunt u hoeveel extern IP-adressen in het systeem wordt gemaakt met voor IPSSL doeleinden kunt selecteren.  Als u interne selecteert moet u het subdomein die worden gebruikt door uw ASE opgeven.  ASEs kan worden geïmplementeerd in virtuele netwerken die *beide* openbare adresbereiken, *of* RFC1918 adres spaties (dat wil zeggen gebruiken persoonlijke adressen).  Pas een virtueel netwerk met een openbare adresbereiken gebruiken, moet u de VNet tijd vooraf maken.  Wanneer u een bestaande VNet selecteert moet u een nieuwe subnet tijdens het maken van ASE maken.  **U kunt een vooraf gemaakte subnet niet gebruiken in de portal.  Als u uw ASE met een sjabloon van de manager resource maakt, kunt u een ASE maken met een bestaande subnet.**  Een ASE maken via een sjabloon gebruiken met de inhoud hier [voor het maken van een App-Service-omgeving van sjabloon] [ ILBAseTemplate] en u kunt [maken van een ILB App Service-omgeving van sjabloon][ASEfromTemplate].

### <a name="details"></a>Meer informatie ###

Een ASE is gemaakt met voorste eindigt 2 en 2 werknemers.  De Front eindigt fungeren als de HTTP-/ HTTPS-eindpunten en verkeer worden verzonden naar de werknemers die de rollen die als host van uw apps.   U kunt de hoeveelheid aanpassen nadat de ASE is gemaakt en kunt ook automatisch schalen regels op deze resourcegroepen instellen.  Voor meer informatie rond handmatig schalen, beheren en controleren van een App-Service-omgeving gaat u naar: [het configureren van een App-Service-omgeving][ASEConfig] 

Alleen de één ASE kunt bestaat niet in het subnet die worden gebruikt door de ASE.  Het subnet kan niet worden gebruikt voor andere naam heeft dan de ASE

### <a name="after-app-service-environment-creation"></a>Nadat de App-omgeving is gemaakt ###

U kunt na het maken van ASE aanpassen:

- Aantal voorste eindigt (minimale: 2)
- Het aantal werknemers (minimale: 2)
- Aantal IP-adressen die beschikbaar zijn voor IP SSL
- Resource-tekengrootten in de voorste eindigt of werknemers berekenen (front-end minimale grootte is P2)


Er zijn meer details rond handmatige schaalbaarheid, beheer en de App serviceomgevingen hier controle: [het configureren van een App-Service-omgeving][ASEConfig] 

Voor meer informatie over autoscaling is hier een handleiding: [automatisch schalen voor een App-Service-omgeving configureren][ASEAutoscale]

Er zijn extra afhankelijkheden die niet beschikbaar voor aanpassing zoals de database en opslag zijn.  Deze zijn afgehandeld door Azure en worden geleverd met het systeem.  De systeem-opslag ondersteunt maximaal 500 GB voor de hele App Service-omgeving en de database wordt aangepast met Azure naar wens door de schaal van het systeem.


## <a name="getting-started"></a>Aan de slag
Alle artikelen en hoe-bevindt zich voor de App serviceomgevingen zijn beschikbaar in het [Leesmij-bestand voor omgevingen met toepassingen Service](../app-service/app-service-app-service-environments-readme.md)aan.

Als u wilt beginnen met App-serviceomgevingen, raadpleegt u [Inleiding tot App serviceomgevingen][WhatisASE]

Zie voor meer informatie over het platform Azure App-Service, [Azure App-Service][AzureAppService].

[AZURE.INCLUDE [app-service-web-whats-changed](../../includes/app-service-web-whats-changed.md)]

[AZURE.INCLUDE [app-service-web-try-app-service](../../includes/app-service-web-try-app-service.md)]
 

<!--Image references-->
[1]: ./media/app-service-web-how-to-create-an-app-service-environment/asecreate-basecreateblade.png
[2]: ./media/app-service-web-how-to-create-an-app-service-environment/asecreate-vnetcreation.png

<!--Links-->
[WhatisASE]: http://azure.microsoft.com/documentation/articles/app-service-app-service-environment-intro/
[ASEConfig]: http://azure.microsoft.com/documentation/articles/app-service-web-configure-an-app-service-environment/
[AppServicePricing]: http://azure.microsoft.com/pricing/details/app-service/ 
[AzureAppService]: http://azure.microsoft.com/documentation/articles/app-service-value-prop-what-is/ 
[ASEAutoscale]: http://azure.microsoft.com/documentation/articles/app-service-environment-auto-scale/
[ILBASE]: http://azure.microsoft.com/documentation/articles/app-service-environment-with-internal-load-balancer/
[ILBAseTemplate]: http://azure.microsoft.com/documentation/templates/201-web-app-ase-create/
[ASEfromTemplate]: http://azure.microsoft.com/documentation/articles/app-service-app-service-environment-create-ilb-ase-resourcemanager/
