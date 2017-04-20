<properties
    pageTitle="Maken en gebruiken van een interne taakverdeling met een App-Service-omgeving | Microsoft Azure"
    description="Maken en gebruiken van een ASE met een ILB"
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

# <a name="using-an-internal-load-balancer-with-an-app-service-environment"></a>Een interne taakverdeling gebruikt met een App Service-omgeving #

De functie App Service Environments(ASE) is de optie voor een Premium-service van Azure App-Service biedt een verbeterde de mogelijkheid die niet beschikbaar is in de stempelen meerdere tenant.  De functie ASE implementeert in feite de App Azure-Service in uw Azure virtuele Network(VNet).  Een beter inzicht in de mogelijkheden die door de App serviceomgevingen lezen de [Wat is een App-Service-omgeving] worden aangeboden krijgen[ WhatisASE] documentatie.  Als u niet weet wie de voordelen van het besturingssysteem in een VNet lezen de [Azure virtuele netwerk Veelgestelde vragen over][virtualnetwork].  


## <a name="overview"></a>Overzicht ##


Een ASE kan worden geïmplementeerd met een toegankelijk eindpunt internet of met een IP-adres in uw VNet.  Voordat u het IP-adres instellen op een adres VNet moet u uw ASE met een interne laden Balancer(ILB) implementeren.  Wanneer uw ASE is geconfigureerd met een ILB moet u het volgende opgeven:

- uw eigen domein of subdomein.  Als u deze eenvoudig, dit document wordt ervan uitgegaan subdomein maar kunt u deze in beide gevallen.  
- het certificaat voor HTTPS
- DNS-beheer voor uw subdomein.  


U kunt tegenprestatie dingen zoals doen:

- host intranet-toepassingen, zoals bedrijfstoepassingen, veilig in de cloud waartoe u toegang tot en met een Site op Site- of ExpressRoute VPN krijgen
- host apps in de cloud die niet worden weergegeven in openbare DNS-servers
- internet geïsoleerd backend apps waarin uw front-end-apps veilig met integreren kunnen maken


#### <a name="disabled-functionality"></a>Uitgeschakelde functionaliteit ####

Er zijn enkele dingen die u niet uitvoeren wanneer u een ASE ILB gebruikt.  Deze zaken opnemen:

- IPSSL gebruiken
- IP-adressen toewijzen aan specifieke apps
- kopen en het gebruik van een certificaat met een app via de portal.  U kunt natuurlijk nog steeds certificaten rechtstreeks bij een certificeringsinstantie ophalen en deze versie gebruiken met uw apps, net niet via de portal van Azure.


## <a name="creating-an-ilb-ase"></a>Een ASE ILB maken ##

Maken van een ASE ILB verschilt niet veel van een ASE normaal maken.  Voor een grondigere discussie over het maken van een ASE [hoe u een App-Service-omgeving maakt lezen][HowtoCreateASE].  Het proces voor het maken van een ASE ILB is dezelfde tussen een VNet maken tijdens het ASE maken of selecteren van een bestaande VNet.  Een ASE ILB maken: 

1.  Selecteer in de portal van Azure **Nieuw Web -> + Mobile -> App-omgeving**
2.  Selecteer uw abonnement
3.  Selecteer of maak een resourcegroep
4.  Selecteer of maak een VNet
5.  Een subnet maken als een VNet selecteren
6.  Selecteer **virtuele netwerklocatie-> VNet configuratie** en het VIP Type ingesteld op interne
7.  Geef subdomeinnaam (dit is het subdomein dat wordt gebruikt voor apps die zijn gemaakt in deze ASE)
8.  Selecteer Ok en vervolgens te maken


![][1]


Er is een VNet configuratie-optie binnen het blad Virtual Network.  Hiermee kunt u kiezen uit een externe VIP of interne VIP.  De standaardinstelling is extern.  Als u wel ingesteld op extern hebt gebruik uw ASE een toegankelijke VIP van internet.  Als u interne selecteert, wordt uw ASE worden geconfigureerd met een ILB op een IP-adres binnen uw VNet.  


Nadat u interne selecteert, de mogelijkheid meer IP-adressen toevoegen aan uw ASE wordt verwijderd en in plaats daarvan moet u het subdomein van de ASE bieden.  De naam van de ASE wordt in een ASE met een externe VIP gebruikt in het subdomein voor apps in die ASE hebt gemaakt.  
Als uw ASE heet ***contosotest*** en uw app in dat ASE heet ***mytest*** vervolgens het subdomein zou van de indeling ***contosotest.p.azurewebsites.net*** en de URL voor deze app zou ***mytest.contosotest.p.azurewebsites.net***.  
Als u het VIP Type ingesteld op interne, wordt de naam van uw ASE niet gebruikt in het subdomein voor de ASE.  U opgeven het subdomein expliciet.  Als een subdomein ***contoso.corp.net*** is en u een app hebt aangebracht in dat ASE ***timereporting benoemde*** zou ***timereporting.contoso.corp.net***zijn op de URL voor deze app.


## <a name="apps-in-an-ilb-ase"></a>Apps in een ASE ILB ##

Een app maken in een ASE ILB is hetzelfde als het maken van een app in een ASE normaal.  

1. Selecteer in de portal van Azure **Nieuw Web -> + Mobile -> Web** of **Mobile** of **API-App**
2. Voer de naam van de app
2. Selecteer abonnement
3. Selecteer of maak de resourcegroep
4. Selecteer of maak App Service Plan(ASP).  Als het maken van een nieuwe ASP uw ASE selecteert als de locatie en selecteer vervolgens de werknemer-toepassingen die u wilt dat uw ASP moet worden gemaakt.  Wanneer u de ASP maakt kunt u uw ASE selecteert als de locatie en de werknemer-toepassingen.  Wanneer u de naam van de app ziet u dat het subdomein onder de appnaam van de wordt vervangen door het subdomein voor uw ASE.   
5. Selecteer maken.  Als u wilt dat de app op uw dashboard, moet u het selectievakje **vastmaken aan dashboard** selecteren.  

![][2]


Klik onder de appnaam van de wordt naam van het subdomein bijgewerkt, zodat het subdomein van uw ASE.  


## <a name="post-ilb-ase-creation-validation"></a>Bericht ILB ASE maken gegevensvalidatie ##

Een ASE ILB is iets anders dan de niet - ILB-ASE.  Als al vermeld moet u uw eigen DNS wordt beheerd en u moet ook uw eigen certificaat bieden voor HTTPS-verbindingen.  


Nadat u uw ASE gemaakt ziet u dat het subdomein het subdomein ziet u u hebt opgegeven en er wordt een nieuw item in het menu van de **instelling** **ILB certificaat**genoemd.  De ASE wordt gemaakt met een zelfondertekend certificaat waardoor het eenvoudiger om te testen HTTPS.  De portal kunt u die u nodig hebt om aan te bieden van uw eigen certificaat voor HTTPS nagaan, maar dit is om een certificaat dat bij een subdomein hoort hebt u aan te sturen.  

![][3]


Als u gewoon dingen uitprobeert en niet hoe weet u een digitaal certificaat maken, kunt u de IIS MMC console-toepassing een zelf-ondertekend certificaat maken.  U kunt deze exporteren als een .pfx-bestand en uploadt dit in de gebruikersinterface van het certificaat ILB zodra deze is gemaakt. Als u een site opent beveiligd met een zelfondertekend certificaat, geeft uw browser u een waarschuwing dat de site die u toegang krijgt tot niet is beveiligd is vanwege het feit dat niet het certificaat te valideren.  Als u wilt voorkomen dat waarschuwing moet u een correct ondertekende certificaat dat overeenkomt met een subdomein en heeft een reeks vertrouwen die wordt herkend door uw browser.

![][6]

Als u wilt proberen de stroom met uw eigen certificaten en HTTP- en HTTPS toegang tot uw ASE testen:

1.  Ga naar ASE UI nadat ASE is gemaakt **ASE-instellingen > ILB certificaten ->**
2.  Stel ILB certificaat door in te schakelen pfx-certificaatbestand en wachtwoord opgeven.  Gaat even verwerkingstijd en het bericht dat een schaal bewerking uitgevoerd wordt worden weergegeven.
3.  Het adres ILB krijgen voor uw ASE (**ASE-eigenschappen > virtuele IP-adres ->**)
4.  Een WebApp maakt in ASE na het maken 
5.  Maak een VM als u niet een dergelijk in die VNET (niet in hetzelfde subnet als het einde ASE of items)
6.  DNS voor uw subdomein instellen.  U kunt een jokerteken gebruikt met een subdomein in uw DNS- of als u wilt doen enkele eenvoudige tests uitvoeren, bewerkt het hostsbestand op uw VM om in te stellen van de naam van de web-app voor VIP IP-adres.  Als uw ASE had naam van het subdomein. ilbase.com en dat u de web-app mytestapp gemaakt zodat deze zou worden gedaan mytestapp.ilbase.com en stel vervolgens die in uw Hostsbestand.  (In Windows het Hostsbestand is bij C:\Windows\System32\drivers\etc\)
7.  Een browser op die VM gebruiken en Ga naar http://mytestapp.ilbase.com (of alle gegevens die de naam van uw web-app zich met uw subdomein)
8.  Een browser op die VM gebruiken en Ga naar https://mytestapp.ilbase.com die u het gebrek aan beveiliging accepteren moet als een zelfondertekend certificaat gebruikt.  


Het IP-adres voor uw ILB is in uw eigenschappen vermeld als het virtuele IP-adres

![][4]


## <a name="using-an-ilb-ase"></a>Een ASE ILB gebruiken ##

#### <a name="network-security-groups"></a>Beveiligingsgroepen netwerk ####

Een ASE ILB kunnen moeten worden geïsoleerd netwerk voor uw apps als de apps niet toegankelijk of zelfs bekend zijn met internet zijn.  Dit is een uitstekende voor het hosten van intranetsites zoals bedrijfstoepassingen.  Wanneer u moet het beperken van toegang zelfs kunt verder u netwerk beveiliging Groups(NSGs) voor toegangsbeheer op het netwerkniveau van het. 


Als u wilt gebruiken NSGs verder beperken van toegang moet u om ervoor te zorgen dat u volgt niet de overname van de communicatie die de ASE nodig heeft om te kunnen werken.  Hoewel de HTTP-/ HTTPS-toegang alleen via de ILB die worden gebruikt door de ASE wordt is nog steeds de ASE afhankelijk van de resource buiten de VNet.  Om te zien welke netwerktoegang is nog steeds vereist Zoek op de gegevens in het document op [Inkomende verkeer naar een App-Service-omgeving bepalen] [ ControlInbound] en het document op [Gegevens in de netwerkconfiguratie voor App serviceomgevingen met ExpressRoute][ExpressRoute].  


Voor het configureren van uw NSGs die u weten het IP-adres dat wordt gebruikt door Azure moet voor het beheren van uw ASE.  IP-adres is ook het uitgaande IP-adres van uw ASE als dit zorgt ervoor dat internetaanvragen.  Om te zoeken in dit IP-mailadres gaat u naar **Instellingen -> Eigenschappen** en zoek de **Uitgaande IP-adres**.  

![][5]


#### <a name="general-ilb-ase-management"></a>Algemeen ILB ASE management ####

Het beheren van een ASE ILB is grotendeels hetzelfde als een ASE normaal beheren.  U moet de schaal van uw groepen werknemer als host meer ASP-exemplaren en schaal van de front-end-servers u omgaat met grotere hoeveelheden HTTP-/ HTTPS-verkeer is toegestaan.  Voor algemene informatie over het beheren van de configuratie van een ASE, raadpleegt u het document op [een App-Service-omgeving configureren][ASEConfig].  


De extra management items zijn Certificaatbeheer en DNS-beheer.  U moet aanvragen en het certificaat voor HTTPS na het maken van ILB ASE uploaden en vervang deze voordat het verloopt.  Omdat Azure eigenaar is van het grondtal domein kunnen wij certificaten bieden voor ASEs met een externe VIP.  Aangezien het subdomein dat wordt gebruikt door een ASE ILB van alles zijn kan, moet u uw eigen certificaat bieden voor HTTPS. 


#### <a name="dns-configuration"></a>DNS-configuratie ####

Wanneer u een externe VIP de DNS-records wordt beheerd door Azure.  Een app hebt gemaakt in uw ASE wordt automatisch toegevoegd aan Azure DNS, dat wil een openbare DNS zeggen.  In een ASE ILB moet u uw eigen DNS wordt beheerd.  Voor een bepaald subdomein zoals contoso.corp.net moet u A DNS-records die verwijzen naar uw ILB-mailadres voor maken:

    * 
    *.SCM ftp publiceren 


## <a name="getting-started"></a>Aan de slag
Alle artikelen en hoe-bevindt zich voor de App serviceomgevingen zijn beschikbaar in het [Leesmij-bestand voor omgevingen met toepassingen Service](../app-service/app-service-app-service-environments-readme.md)aan.

Als u wilt beginnen met App-serviceomgevingen, raadpleegt u [Inleiding tot App serviceomgevingen][WhatisASE]

Zie voor meer informatie over het platform Azure App-Service, [Azure App-Service][AzureAppService].

[AZURE.INCLUDE [app-service-web-whats-changed](../../includes/app-service-web-whats-changed.md)]

[AZURE.INCLUDE [app-service-web-try-app-service](../../includes/app-service-web-try-app-service.md)]
 

<!--Image references-->
[1]: ./media/app-service-environment-with-internal-load-balancer/ilbase-createilbase.png
[2]: ./media/app-service-environment-with-internal-load-balancer/ilbase-createapp.png
[3]: ./media/app-service-environment-with-internal-load-balancer/ilbase-newase.png
[4]: ./media/app-service-environment-with-internal-load-balancer/ilbase-vip.png
[5]: ./media/app-service-environment-with-internal-load-balancer/ilbase-externalvip.png
[6]: ./media/app-service-environment-with-internal-load-balancer/ilbase-ilbcertificate.png

<!--Links-->
[WhatisASE]: http://azure.microsoft.com/documentation/articles/app-service-app-service-environment-intro/
[HowtoCreateASE]: http://azure.microsoft.com/documentation/articles/app-service-web-how-to-create-an-app-service-environment/
[ControlInbound]: http://azure.microsoft.com/documentation/articles/app-service-app-service-environment-control-inbound-traffic/
[virtualnetwork]: https://azure.microsoft.com/documentation/articles/virtual-networks-faq/
[AppServicePricing]: http://azure.microsoft.com/pricing/details/app-service/
[AzureAppService]: http://azure.microsoft.com/documentation/articles/app-service-value-prop-what-is/
[ASEAutoscale]: http://azure.microsoft.com/documentation/articles/app-service-environment-auto-scale/
[ExpressRoute]: http://azure.microsoft.com/documentation/articles/app-service-app-service-environment-network-configuration-expressroute/
[vnetnsgs]: http://azure.microsoft.com/documentation/articles/virtual-networks-nsg/
[ASEConfig]: http://azure.microsoft.com/documentation/articles/app-service-web-configure-an-app-service-environment/
