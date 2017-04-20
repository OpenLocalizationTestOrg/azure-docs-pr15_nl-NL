

#<a name="metadata-for-azure-technical-articles"></a>Metagegevens voor Azure technische artikelen

Alle Azure technische artikelen bevatten twee metagegevens secties - een sectie-eigenschappen en een sectie tags. De sectie eigenschappen kunt u sommige website automatisering-SEO-items, terwijl de sectie labels een groot aantal interne inhoud melden kunt. Beide secties zijn vereist.

- [Syntaxis]
- [Gebruik]
- [Kenmerken en waarden voor de sectie eigenschappen]
- [Kenmerken en waarden voor de sectie labels]

##<a name="syntax"></a>Syntaxis

De sectie eigenschappen gebruikt de volgende syntaxis:

    <properties
       pageTitle="Page title that displays in search results and the browser tab | Microsoft Azure"
       description="Article description that will be displayed on landing pages and in most search results"
       services="service-name"
       documentationCenter="dev-center-name"
       authors="GitHub-alias-of-only-one-author"
       manager="manager-alias"
       editor=""
       tags="optional"
       keywords="For use by SEO champs only. Separate terms with commas. Check with your SEO champ before you change content in this article containing these terms."/>

De sectie tags gebruikt de volgende syntaxis:

    <tags
       ms.service="required"
       ms.devlang="may be required"
       ms.topic="article"
       ms.tgt_pltfrm="may be required"
       ms.workload="na"
       ms.date="mm/dd/yyyy"
       ms.author="Your MSFT alias or your full email address;semicolon separates two or more"/>

##<a name="usage"></a>Gebruik

- De elementnaam van het en kenmerknamen zijn hoofdlettergevoelig.
- De <properties> sectie moet de eerste regel van het bestand.
- Laat een lege regel na elke metagegevenssectie en vóór de titel van de pagina om ervoor te zorgen dat de paginatitel wordt correct geconverteerd naar HTML tijdens het publicatieproces.

## <a name="attributes-and-values-for-the-properties-section"></a>Kenmerken en waarden voor de sectie eigenschappen

![](./media/article-metadata/checkmark-small.png)**Paginatitel**: vereist; belangrijk voor SEO. De tekst voor dit kenmerk weergegeven in het browsertabblad en als titel in een zoekresultaat. Gebruik 55-60 tekens, inclusief spaties en met inbegrip van de site-id *| Microsoft Azure* (ingevoerd als: recht streepje ruimte Microsoft Azure ruimte voor is).  De paginatitel moet afwijken van de H1.

![](./media/article-metadata/checkmark-small.png)**Beschrijving**: vereist; belangrijke voor SEO (relevantie) en de functionaliteit van de site. De omschrijving moet ten minste 125 tekens lang op 155 tekens maximale inclusief spaties. Omschrijving van het doel van uw inhoud zodat klanten weten of u om deze te kiezen uit een lijst met zoekresultaten. De waarde luidt als volgt:

- Deze tekst kan worden weergegeven als de beschrijving of abstracte alinea in de zoekresultaten op Google.
- Deze tekst wordt weergegeven in [de resultaten van de index artikel](https://azure.microsoft.com/documentation/articles/).

![](./media/article-metadata/checkmark-small.png)**Services**: vereist voor artikelen die betrekking op een service hebben. Deze waarde wordt ofter de "service witruimte' genoemd. Lijst met alle toepasselijke services, gescheiden door komma's. De eerste service die u wilt vermelden, wordt de navigatie breadcrumbs voor de pagina en het linker navigatiegedeelte op die wordt weergegeven met de pagina sturen.

In de artikelen die zowel een services-waarde en een waarde documentationCenter opgeeft, wordt de waarde van de services de ' breadcrumb ' station. Extra waarden die u lijst wordt weergegeven als labels in de gepubliceerde artikel. Waarden:

- Active directory
- actief-directory-b2c
- actief-directory-ds
- App-service\api
- API-beheer
- App-service
- App-servic\mobile
- App-service\web
- App-service\logic
- toepassing-gateway
- toepassing-inzichten
- automatisering
- Azure-portal
- Azure-resourcemanager
- Azure-stack
- back-up maken
- batch
- Tips
- BizTalk-services
- cache
- CDN
- cloud-services
- catalogus met gegevens
- gegevens fabriek
- gegevens-lake-analyses
- gegevens-lake-store
- devtest-testomgeving
- DNS
- documentdb
- expressroute
- gebeurtenis-hubs
- hdinsight
- IOT-hub
- toets-kluis
- verdeling van belasting
- machine-training
- Marketplace
- Media-services
- Mobile-betrokkenheid
- Mobile-services
- meerdere-factor-verificatie
- melding-hubs
- operationele inzichten
- bewerkingen-management-suite
- powerapps
- herstel-manager
- bestand Vgx.-cache
- RemoteApp
- Information Rights management
- planner
- zoeken
- beveiliging in het midden
- Service-bus
- Service-stof
- site-herstel
- SQL-database
- SQL-data-warehouse
- SQL-rapportage
- opslag
- Store
- storsimple
- Stream-analyses
- verkeer-manager
- virtuele machines
- virtuele-netwerk
- Visual studio online
- VPN-gateway
- websites

![](./media/article-metadata/checkmark-small.png)**documentationCenter**: vereist voor ontwikkelaar centraal artikelen aanbevolen aanbevolen via een ontwikkelaar-beheercentrum. Geef de één-ontwikkelaar beheercentrum of de taal die van toepassing op het artikel. De waarde die u een lijst met wordt de navigatie breadcrumbs voor de pagina sturen. In de artikelen die zowel een services-waarde en een waarde documentationCenter opgeeft, wordt de waarde van de services de ' breadcrumb ' station. Waarden:

- **.NET**
- **nodejs**
- **Java**
- **PHP**
- **Python**
- **Ruby**
- **mobiel**: afgeschaft. Specifieke mobiel platform te vervangen.
- **IOS**: Verifing deze nieuwe waarde
- **android**: deze nieuwe waarde verifiëren
- **Windows**: deze nieuwe waarde verifiëren
- **xamarin**: deze nieuwe waarde verifiëren

![](./media/article-metadata/checkmark-small.png)**auteurs**: verplicht, slechts één waarde. Lijst met het account GitHub voor het artikel kleine of middelgrote onderneming of primaire auteur. Dit kenmerk schijfstations de naamregel op de gepubliceerde artikel. Een lijst met slechts één, ondanks de meervoudige naam van het kenmerk.

![](./media/article-metadata/checkmark-small.png)**Manager**: als u een inzender Microsoft zijn vereist. Lijst met de e-mailalias van de inhoud van de publicerende beheerder voor het gebied technologie. Als u een inzender community, het kenmerk zonder deze leeg zodat we kunnen invullen.

![](./media/article-metadata/checkmark-small.png)**Editor**: niet gebruikt. Gebruik deze niet voor andere doeleinden.

![](./media/article-metadata/checkmark-small.png)**tags**: optioneel. Alleen als u wilt een koppeling onder het artikel ' breadcrumb ' naar de pagina artikel index (http://azure.microsoft.com/documentation/articles/) inschakelen voor een prefiltered lijst met artikelen die overeenkomen met een van de goedgekeurde waarden toevoegen. Deze waarden zijn bedoeld om er zelf een toe om te groeperen inhoud wanneer u de groepering van inhoud is niet service / regiospecifieke. Deze tags kunnen ook worden verstrekt labels die wordt aangegeven de technologie stapel die het artikel is van toepassing op. Deze waarde **betekent niet** ondersteuning voor vrije vorm labels of hashtags; de labels moeten zijn ingeschakeld op de site. U kunt meerdere tags waarden naar een artikel, gescheiden door komma's opgeven. De goedgekeurde waarden zijn:

  - architectuur
  - Azure-resourcemanager
  - Azure-service-beheer
  - facturering
  - MySQL

![](./media/article-metadata/checkmark-small.png)**trefwoorden**: optioneel. Voor gebruik door alleen SEO-champs. Scheid termen door komma's. **Neem contact op met uw SEO soepel verlopen voordat u inhoud in dit artikel met deze termen verwijderen of wijzigen.** Dit kenmerk records trefwoorden het SEO soepel verlopen heeft aangewezen en bijhoudt ter verbetering van de rangorde van zoeken. De trefwoorden kunnen niet worden weergegeven in de gepubliceerde HTML. Gegevensvalidatie is niet vereist voor dit kenmerk.

## <a name="attributes-and-values-for-the-tags-section"></a>Kenmerken en waarden voor de sectie labels

![](./media/article-metadata/checkmark-small.png)**MS.service**: vereist. Hiermee geeft u de Azure-service, hulpmiddel of functie die het artikel is van toepassing op. Één waarde per pagina.

 Als u een pagina is van toepassing op meerdere services, kiest u de service waarin het meest rechtstreeks van toepassing is; een artikel die gebruikmaakt van een app die worden gehost op websites om te laten zien Service Bus functionaliteit moet bijvoorbeeld hebben de waarde van de **service-bus** in plaats van die van **websites**. Als u een pagina is van toepassing op meerdere services gelijkmatig, kiest u **meerdere**. Als u een pagina geldt niet voor alle services (dit is niet vaak voorkomen), kiest u **NB**.

 - **Active directory**
 - **actief-directory-b2c**
 - **actief-directory-ds**
 - **API-beheer**
 - **App-service**: geldt alleen voor algemene conceptuele materiaal op App-Service
 - **App-service-api**
 - **App-service-logica**
 - **App-service-mobile**
 - **App-service-web**
 - **toepassing-inzichten**
 - **toepassing-gateway**
 - **automatisering**
 - **Azure-resourcemanager**
 - **Azure-beveiliging**
 - **Azure-stack**
 - **back-up maken**
 - **batch**
 - **Tips**
 - **BizTalk-services**
 - **facturering**
 - **cache**
 - **CDN**
 - **cloud-services**
 - **catalogus met gegevens**
 - **gegevens-lake-store**
 - **gegevens-lake-analyses**
 - **devtest-testomgeving**
 - **expressroute**
 - **hdinsight**
 - **Internet-van-dingen**
 - **IOT-hub**
 - **toets-kluis**
 - **machine-training**
 - **Marketplace**: artikelen over de Azure marketplace
 - **Media-services**
 - **Mobile-betrokkenheid**
 - **Mobile-services**
 - **meerdere-factor-verificatie**
 - **meerdere**: de pagina is van toepassing op meerdere services gelijkmatig
 - **NB**: de pagina geldt niet voor alle services (niet vaak voorkomen)
 - **melding-hubs**
 - **operationele inzichten**
 - **powerapps**
 - **herstel-manager**
 - **bestand Vgx.-cache**
 - **RemoteApp**
 - **Information Rights management**
 - **planner**
 - **beveiliging in het midden**
 - **Service-bus**
 - **Service-stof**
 - **site-herstel**: voorheen herstel-services
 - **SQL-database**
 - **SQL-data-warehouse**
 - **SQL-rapportage**
 - **opslag**
 - **Opslaan**: artikelen over services beschikbaar via de Store Azure
 - **storsimple**
 - **verkeer-manager**
 - **virtuele machines**
 - **virtuele-netwerk**
 - **Visual studio online**
 - **VPN-gateway**
 - **websites**

![](./media/article-metadata/checkmark-small.png)**MS.devlang**: vereist. Hiermee geeft u de programmeertaal die het artikel is van toepassing op. Één waarde per pagina.

 Als u een pagina is van toepassing op twee programming talen gelijkmatig, kiest u **meerdere**. Als een pagina hoofdzakelijk conceptuele wordt en de inhoud meestal van toepassing op meerdere talen is, kiest u **meerdere**. Als een pagina niet bedoeld is voor ontwikkelaars en de programming talen niet relevant is, kiest u **NB**. Gebruik **rest api** REST API-naslaginformatie identificeren.

 - **cpp**
 - **DotNet**
 - **Java**
 - **JavaScript**
 - **meerdere**: de pagina is van toepassing op meerdere programming talen gelijkmatig.
 - **NB**: de pagina is niet hebt samengesteld ontwikkelaars en niet specifiek is voor eventuele programming talen.
 - **nodejs**
 - **doelstelling-c**
 - **PHP**
 - **Python**
 - **rest-api**
 - **Ruby**


![](./media/article-metadata/checkmark-small.png)**MS.topic**: vereist. Specificaties typt u het onderwerp. De meeste nieuwe pagina's die zijn gemaakt door inzenders is artikel of een verwijzing.

 - **artikel**: een conceptuele onderwerp, zelfstudie, functieoverzicht of andere niet-naslagartikel

 - **campagne-pagina**: alleen Azure.com.  Een pagina die is speciaal ontworpen als een aantekening toevoegen voor externe campagnes en maakt geen deel uit van de primaire IA-site  Niet mogen worden gebruikt voor documentatie artikelen of gewone doc lossen van pagina's.  Voorbeelden: azure.microsoft.com/develop/net/aspnet/; Azure.Microsoft.com/Develop/Mobile/IOS/

 - **ontwikkelaar-beheercentrum--startpagina**: alleen Azure.com.  Een ontwikkelaar startpagina, bijvoorbeeld centreren/ontwikkelen/net /

 - **Get-gestart-artikel**: toewijzen aan artikelen die worden aanbevolen in de sectie aan de slag of overzicht van het linker navigatiegedeelte voor een service.

 - **prominente afbeelding-artikel**: een "prominente" zelfstudie die is ontworpen voor een inleiding geven in een service of functie waarbij wordt gestart met de service snel bezoekers en stations gratis proefversie Aanmeldingsadres-ups en MSDN activeringen. Deze waarde toewijzen alleen naar artikelen die worden aanbevolen boven aan de openingspagina van documentatie voor uw service.

 - **startpagina**: startpagina van het bovenste niveau documentatie. Alleen zijn er twee: azure.microsoft.com/documentation/ en msdn.microsoft.com/library/azure/

 - **index pagina**: tweede niveau lossen van pagina's voor het programmeren talen, services of functies. Deze zijn verdeeld over Azure.com en de bibliotheek en worden gebruikt als invoer wordt verwezen voor meer informatie over bereik informatie. Voorbeelden: http://azure.microsoft.com/develop/mobile/resources-wp8/, http://msdn.microsoft.com/library/azure/jj673460.aspx, http://msdn.microsoft.com/library/azure/hh689864.aspx

 - **infographic-pagina**: alleen Azure.com. Een pagina met een geschikt voor browser infographic of poster, bijvoorbeeld http://azure.microsoft.com/documentation/infographics/windows-azure/

 - **Overzicht**: een API verwijzing pagina (inclusief REST API) of PowerShell-cmdlet verwijzing pagina

 - **Service-startpagina**: alleen Azure.com.  Een service-startpagina van document, bijvoorbeeld /documentation/services/virtual-machines /

 - **site-sectie-start-pagina**: alleen Azure.com. Een "startpagina' voor een bepaald type inhoud op azure.com voorbeelden: http://azure.microsoft.com/documentation/infographics/, http://azure.microsoft.com/documentation/scripts/, http://azure.microsoft.com/documentation/videos/home/, http://azure.microsoft.com/downloads/

 - **video-pagina**: alleen Azure.com.  Een pagina waarop u een video, bijvoorbeeld http://azure.microsoft.com/documentation/videos/azure-webjobs-hosting-testing-net/ functies

![](./media/article-metadata/checkmark-small.png)**MS.tgt_pltfrm**: vereist. Hiermee geeft u het doelplatform, bijvoorbeeld Windows, Linux, Windows Phone-, iOS, Android of speciale cache platforms. Één waarde per pagina. Deze waarde wordt **NB** zijn voor de meeste onderwerpen behalve mobile en virtuele machines.

 - **cache-tot-rol**
 - **cache-veelvoud**
 - **cache-bestand Vgx.**
 - **cache-service**
 - **cache-gedeeld**
 - **vanaf de opdrachtregel-line-interface**
 - **Ibiza**: inhoud op basis van de portal Ibiza. Gebruik deze alleen in gevallen waarin de functie besproken beschikbaar via de portal Ibiza zowel de huidige portal is.
 - **Mobile-android**: alleen nu Azure.com
 - **Mobile-HTML-**: alleen nu Azure.com
 - **Mobile-ios**: alleen nu Azure.com
 - **Mobile-kindle**: alleen nu Azure.com
 - **Mobile-veelvoud**
 - **Mobile-nokia-x**: alleen nu Azure.com
 - **Mobile-phonegap**: alleen nu Azure.com
 - **Mobile-sencha**: alleen nu Azure.com
 - **windows Mobile**: Azure.com alleen nu; Windows Universal
 - **mobiele-windows-telefoon**
 - **Mobile-windows-store**
 - **Mobile-xamarin**: Azure.com alleen nu; Xamarin alle platforms
 - **Mobile-xamarin-android**: alleen nu Azure.com
 - **Mobile-xamarin-ios**: alleen nu Azure.com
 - **meerdere**: de pagina is van toepassing op meerdere platforms gelijkmatig
 - **NB**: de aanduiding van een platform is niet van toepassing op deze pagina
 - **PowerShell**
 - **VM-linux**
 - **VM-veelvoud**
 - **VM-windows**
 - **VM-windows-sharepoint**
 - **VM-windows-sql-server**
 - **tegenover--introductie**: geeft aan wat het paginagroep tegenover aan de slag. Tag toegevoegd 12-1-14.
 - **tegenover-wat-is er gebeurd**: de pagina tegenover aan de slag wat is er gebeurd aangeeft. Tag toegevoegd 12-1-14.

![](./media/article-metadata/checkmark-small.png)**MS.workload**: vereist. Hiermee geeft u de Azure werklast die de pagina is van toepassing op. Een bepaalde tekenreeks alleen per artikel.

**8/6/15 bijwerken** De waarde ms.workload wordt door een xls, niet de waarde in het bestand .md toegewezen. De waarde ms.workload is nog steeds vereist voor validatie tot de functie kan worden bijgewerkt. Die werken wordt nu gepland.  Gebruik **'na'** als de waarde voor nu.

![](./media/article-metadata/checkmark-small.png)**MS.date**: vereist. Hiermee geeft u de datum die het artikel voor het laatst is bekeken voor relevantie te verkrijgen, nauwkeurigheid juiste schermopnamen en koppelingen werken. Voer de datum in de notatie dd-mm-jjjj. Deze datum wordt ook weergegeven op de gepubliceerde artikel als de laatste datum van de bijgewerkte.

![](./media/article-metadata/checkmark-small.png)**MS.Author**: vereist. Hiermee geeft u de auteur (s) die zijn gekoppeld aan het onderwerp. Interne rapporten (zoals fris) gebruikt deze waarde om de juiste auteur (s) koppelen aan het artikel. Als u wilt opgeven van meerdere waarden moet u, gescheiden door puntkomma's. Microsoft aliassen of volledige e-mailadressen worden geaccepteerd. De lengte mag niet meer dan 200 tekens.


###<a name="contributors-guide-links"></a>Inzenders de handleiding voor koppelingen

- [Van overzichtsartikel](./../README.md)
- [Index van artikelen](./contributor-guide-index.md)


<!--Anchors-->
[Syntaxis]: #syntax
[Gebruik]: #usage
[Kenmerken en waarden voor de sectie eigenschappen]: #attributes-and-values-for-the-properties-section
[Kenmerken en waarden voor de sectie labels]: #attributes-and-values-for-the-tags-section
