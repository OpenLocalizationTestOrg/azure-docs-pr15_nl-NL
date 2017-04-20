<properties
    pageTitle="Prestatieoverwegingen voor Azure verkeer Manager | Microsoft Azure"
    description="Meer informatie over prestaties op verkeer Manager en hoe u de prestaties van uw website testen bij gebruik van verkeer Manager"
    services="traffic-manager"
    documentationCenter=""
    authors="sdwheeler"
    manager="carmonm"
    editor=""
/>
<tags
    ms.service="traffic-manager"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="infrastructure-services"
    ms.date="10/11/2016"
    ms.author="sewhee"
/>

# <a name="performance-considerations-for-traffic-manager"></a>Prestatieoverwegingen voor verkeer Manager

Deze pagina wordt uitgelegd Prestatieoverwegingen met verkeer Manager. Bekijk de volgende scenario:

U hebt exemplaren van uw website in de regio's WestUS en Oost-Aziatische. Een van de exemplaren is de statuscontrole voor het verkeer manager-test verbroken. Toepassing-verkeer is doorgestuurd naar de orde regio. Deze failover naar verwachting maar prestaties kan een probleem op basis van de latentie van het verkeer nu reizen naar een verre gebied.

## <a name="how-traffic-manager-works"></a>De werking van verkeer Manager

De enige invloed op de prestaties die verkeer Manager op uw website hebben kunnen is de eerste DNS-zoekopdracht. Een DNS-aanvraag voor de naam van uw profiel verkeer Manager wordt uitgevoerd door de server van Microsoft DNS hoofdsite waarop de zone trafficmanager.net. Verkeer Manager wordt gevuld en regelmatig bijgewerkt, de Microsoft DNS-hoofdsite servers op basis van het beleid verkeer Manager en de resultaten van de test. Geen DNS-query's worden dus ook tijdens de eerste DNS-zoekopdracht, naar verkeer Manager worden verzonden.

Verkeer Manager bestaat uit de volgende van verschillende onderdelen: DNS-servers, een service API de opslaglaag en een eindpunt service controleren naam. Als een onderdeel van de service verkeer Manager mislukt, is er geen invloed op de DNS-naam die is gekoppeld aan uw profiel verkeer Manager. De records in de Microsoft DNS-servers blijven ongewijzigd. Eindpunt cmdlets voor controle en bijwerken van DNS-echter niet plaatsvindt. Verkeer Manager dus niet mogen DNS zodat deze verwijzen naar uw site failover wanneer uw primaire site uitvalt bijwerken.

DNS-naamresolutie snel en resultaten in cache opslaan. De snelheid van de eerste DNS-zoekopdracht, is afhankelijk van de DNS-servers van die de client om te zetten gebruikt. Een client kan meestal een DNS-zoekopdracht binnen ~ 50 ms uitvoeren. De resultaten van de zoekactie zijn opgeslagen in de cache voor de duur van de DNS-Time to live (TTL). De standaard-TTL voor verkeer Manager is 300 seconden.

Verkeer doorloopt via verkeer Manager niet. Zodra de DNS-zoekopdracht is voltooid, heeft de client een IP-adres van een exemplaar van uw website. De client verbinding maakt rechtstreeks naar dit adres en tot en met verkeer Manager niet overgedragen. Het verkeer Manager-beleid die u kiest heeft geen invloed op de DNS-prestaties. Een prestaties routering-methode kan echter een negatieve invloed hebben de toepassingservaring. Bijvoorbeeld: als uw beleid verkeer van Noord-Amerika naar een exemplaar is gehost in Azië omleidingen, mogelijk de netwerklatentie voor die sessies een prestatieprobleem leiden.

## <a name="measuring-traffic-manager-performance"></a>Verkeer Manager prestaties meten

Er zijn verschillende websites die u gebruiken kunt voor meer informatie over de prestatie en werking van een profiel verkeer Manager. Veel van deze sites zijn gratis, maar u moet mogelijk beperkingen. Sommige sites bieden uitgebreide controle en rapporten voor een bedrag.

De hulpmiddelen op deze sites maateenheid DNS vertragingstijden en op de opgelost IP-adressen voor client locaties overal ter wereld. Meest deze tools niet opslaan in de resultaten van de DNS-cache. Daarom weergeven de hulpmiddelen voor de volledige DNS-zoekopdracht telkens wanneer die een toets wordt uitgevoerd. Wanneer u vanuit uw eigen client test, u alleen de prestaties de volledige DNS-opzoeken eenmaal tijdens de TTL-duur.

## <a name="sample-tools-to-measure-dns-performance"></a>Voorbeeld hulpmiddelen voor het meten van DNS-prestaties

- [SolveDNS](http://www.solvedns.com/dns-comparison/)

    SolveDNS biedt een groot aantal hulpprogramma's voor prestaties. Het hulpmiddel DNS-vergelijking kunt u zien hoe lang het duurt voor het oplossen van de naam van uw DNS- en hoe die zich verhoudt tot andere DNS-serviceproviders.

- [WebSitePulse](http://www.websitepulse.com/help/tools.php)

    Een van de eenvoudigste hulpmiddelen is WebSitePulse. Voer de URL als u wilt zien van de DNS-resolutie tijd, eerste Byte laatste Byte en andere prestatiestatistieken. U kunt kiezen uit drie verschillende test locaties. In dit voorbeeld ziet u dat de uitvoering van het eerste ziet u dat DNS-lookup 0.204 sec heeft.

    ![pulse1](./media/traffic-manager-performance-considerations/traffic-manager-web-site-pulse.png)

    Omdat in cache opslaan van de resultaten, de tweede test voor hetzelfde verkeer Manager eindpunt de DNS-zoekopdracht gaat 0.002 sec.

    ![pulse2](./media/traffic-manager-performance-considerations/traffic-manager-web-site-pulse2.png)

- [CA App synthetische Monitor](https://asm.ca.com/en/checkit.php)

    Voorheen bekend als het hulpmiddel Watchmouse selectievakje Website, deze site u de DNS-resolutie tijd weergeven uit meerdere geografische regio's tegelijk. Voer de URL als u wilt zien van de DNS-resolutie tijd, verbindingstijd en snelheid van verschillende geografische locaties. Gebruik deze toets om te zien welke gehoste service voor verschillende locaties overal ter wereld wordt geretourneerd.

    ![pulse1](./media/traffic-manager-performance-considerations/traffic-manager-web-site-watchmouse.png)

- [Pingdom](http://tools.pingdom.com/)

    Dit hulpmiddel bevat prestatiestatistieken voor elk element van een pagina met webonderdelen. Het tabblad analyseren van de pagina ziet u het percentage van de tijd die aan de DNS-zoekopdracht besteed.

- [Wat is mijn DNS?](http://www.whatsmydns.net/)

    Deze site een DNS-zoekopdracht betekent van 20 verschillende locaties en de resultaten worden weergegeven op een kaart.

- [Graven Web-Interface](http://www.digwebinterface.com)

    Deze site ziet u gedetailleerdere informatie over DNS-inclusief CNAME-records en een records. Controleer of u de 'Vullen met kleur uitvoer' en 'Statistieken' onder Opties en 'Alles' onder Naamservers te selecteren.

## <a name="next-steps"></a>Volgende stappen

[Over verkeer Manager verkeer routeren methoden](traffic-manager-routing-methods.md)

[Test uw instellingen voor verkeer Manager](traffic-manager-testing-settings.md)

[Bewerkingen op verkeer Manager (REST API overzicht)](http://go.microsoft.com/fwlink/?LinkId=313584)

[Azure verkeer Manager-Cmdlets](http://go.microsoft.com/fwlink/p/?LinkId=400769)
