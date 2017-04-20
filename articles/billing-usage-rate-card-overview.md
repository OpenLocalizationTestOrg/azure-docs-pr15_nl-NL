<properties
   pageTitle="Inzicht in uw verbruik van Microsoft Azure krijgen | Microsoft Azure"
   description="Biedt een overzicht van de Azure facturering gebruik en RateCard APIs's die worden gebruikt voor inzichten in verbruik van Azure en trends."
   services=""
   documentationCenter=""
   authors="BryanLa"
   manager="mbaldwin"
   editor=""
   tags="billing"/>

<tags
   ms.service="billing"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="billing"
   ms.date="08/16/2016"
   ms.author="mobandyo;bryanla"/>

# <a name="gain-insights-into-your-microsoft-azure-resource-consumption"></a>Inzicht in uw verbruik van Microsoft Azure krijgen

Klanten en partners vragen om de mogelijkheid om te voorspellen en hun Azure kostenbeheer nauwkeurig.  Terwijl ze van een Capex aan een model Opex, moeten ze ook de mogelijkheid om te doen showback versus financiële analyse, evenals modus leiden in schatting en facturering, met name voor grote cloud implementaties bieden.

De rente kaart-API's en Azure Resourcegebruik besproken in dit artikel adres moet doordat nieuwe inzichten in uw verbruik van Azure resources.  

## <a name="introducing-the-azure-resource-usage-and-ratecard-apis"></a>Inleiding tot de RateCard API's en Azure Resourcegebruik

De Azure Resourcegebruik en de RateCard APIs worden geïmplementeerd als een Resource-Provider, als onderdeel van de serie API's die worden aangeboden door de Azure Resource Manager.  

### <a name="azure-resource-usage-api-preview"></a>Azure Resourcegebruik API (Preview)
Klanten en partners kunnen de API Azure Resource gebruik toegang tot hun gegevens geschatte Azure verbruik gebruiken. De functies zijn:

- **Toegangsbeheer op basis van rollen van azure** - klanten en partners kunt hun clienttoegangsbeleid configureren op de [portal van Azure](https://portal.azure.com) of via [Azure PowerShell-cmdlets](powershell-install-configure.md) om op te geven welke gebruikers of toepassingen toegang tot gegevens over het gebruik van het abonnement krijgen kunnen. Bellers moeten standaard Azure Active Directory-tokens gebruiken voor verificatie. De beller moet ook worden toegevoegd aan de lezer, eigenaar of Inzender rol voor toegang tot de gebruiksgegevens voor een bepaald Azure abonnement.

- **Per uur of dagelijkse aggregaties** - Bellers kunnen aangeven of ze hun Azure gebruiksgegevens in per uur gerangschikte verzamelingen of dagelijkse gerangschikte verzamelingen. De standaardinstelling is dagelijkse.

- **Exemplaar metagegevens (inclusief resource labels)** – exemplaar niveau details zoals de volledig gekwalificeerde resource-uri (/subscriptions/ {abonnements-id} /..), samen met de resource groep informatie- en resourcekalenders tags krijgt in de reactie. Hiermee wordt klanten jokertekenelement help en gebruik via programmacode toewijzen door de desbetreffende tags, gebruik zaken zoals cross-opladen in.

- **Resource metagegevens** - Resourcedetails zoals de naam van de meter, meter categorie, meter subcategorie, per eenheid en regio, wordt ook in het antwoord, geven de bellers beter inzicht van wat verbruikte doorgegeven. We werkt ook als u wilt uitlijnen resource metagegevens terminologie via de portal Azure Azure gebruik CSV, EA CSV- en andere openbare ervaringen, zodat klanten die u wilt relateren van gegevens over ervaringen facturering.

- **Gebruik voor alle aanbieding typen** – gebruiksgegevens zijn toegankelijk voor alle typen Pay-as-you-go, MSDN monetaire resourcebeperkingen, monetaire krediet en EA waaronder bieden.

### <a name="azure-resource-ratecard-api-preview"></a>Azure Resource RateCard API (Preview)
Klanten en partners kunnen de Azure Resource RateCard API gebruiken om de lijst met beschikbare Azure bronnen samen met geschatte prijsinformatie voor elk label. De functies zijn:

- **Toegangsbeheer op basis van rollen van azure** - klanten en partners kunt hun clienttoegangsbeleid configureren op de [portal van Azure](https://portal.azure.com) of via [Azure PowerShell-cmdlets](powershell-install-configure.md) om op te geven welke gebruikers of toepassingen toegang tot de gegevens RateCard krijgen kunnen. Bellers moeten standaard Azure Active Directory-tokens gebruiken voor verificatie. De beller moet ook worden toegevoegd aan de lezer, eigenaar of Inzender rol voor toegang tot de gebruiksgegevens voor een bepaald Azure abonnement.

- **Ondersteuning voor Pay-as-you-go, MSDN, monetaire resourcebeperkingen, en monetaire krediet biedt (EA niet ondersteund)** - deze API, vindt u informatie van Azure aanbieding niveau rente, versus abonnement.  De beller van deze API moet de aanbieding gegevens Resourcedetails en tarieven doorgeven.  Zoals EA aanbiedingen hebt aangepast tarieven per registratie, kunnen we geen de tarieven EA op dit moment.

## <a name="scenarios"></a>Scenario 's

Hier volgen enkele scenario's beschreven die mogelijk met de combinatie van het gebruik en de APIs RateCard worden gemaakt:

- **Azure besteedt tijdens de maand** - klanten de beschikking over het gebruik en RateCard APIs in combinatie om beter inzicht krijgen in de cloud besteedt tijdens de maand, door een analyse van het uur- en vol met gebruik en boete maakt een schatting.

- **Waarschuwingen instellen** – klanten en partners kunt resource- of monetaire gebaseerde waarschuwingen instellen op hun verbruik cloud door het geschatte verbruik en boete schatting met het gebruik en de API RateCard.

- **Predict factuur** – klanten en partners hun geschatte verbruik kunt ophalen en cloud besteed en machine learning algoritmen om te voorspellen wat hun factuur aan het einde van de factureringscyclus zou zijn van toepassing.

- **Oude verbruik kostenanalyse** -klanten kunnen ook gebruiken de API RateCard te voorspellen hoeveel hun factuur zou als deze zijn door hun werkbelasting kan verplaatsen naar Azure, mits gewenst gebruik getallen. Als klanten bestaande werkbelasting in andere wolken of privé wolken hebt, kunnen ze ook hun gebruik met de Azure toewijzen tarieven om een betere schatting van hun geschatte Azure besteden. Dit biedt een betere weergave van wat kan worden verkregen via de [Azure prijzen Rekenmachine](https://azure.microsoft.com/pricing/calculator/), zoals (bijvoorbeeld) onze partners facturering de mogelijkheid om te draaien op aanbieding en vergelijken/contrast tussen verschillende aanbieding typen voorbij Pay-As-You-Go, inclusief monetaire resourcebeperkingen en monetaire krediet bieden. Daarnaast bieden de API's de mogelijkheid om te kostenwijzigingen schatting per regio, zodat het type what-if-analyses vereist voor implementatie beslissingen, zoals de resources in verschillende DCs overal ter wereld rechtstreeks van invloed op de totale kosten hebben kunnen.

- **Wat-als-analyse** -

    - Klanten en partners kunnen bepalen of het meest efficiënt uitvoeren van hun werkbelasting in een andere regio of op een andere configuratie van de Azure resource zou zijn. Azure resource kosten kunnen verschillen op basis van de Azure regio waarin ze worden uitgevoerd en in deze sectie geeft klanten en partners om kosten optimalisaties toe.

    - Klanten en partners kunnen ook als een ander type van Azure aanbieding een betere tarief op een Azure resource geeft bepalen.

## <a name="partner-solutions"></a>Partneroplossingen

[Gebruik van Microsoft Azure en RateCard API's inschakelen Cloudyn naar ITFM bieden voor klanten](billing-usage-rate-card-partner-solution-cloudyn.md) beschrijving van de integratie-ervaring door Azure facturering API partner [Cloudyn](https://www.cloudyn.com/microsoft-azure/)worden aangeboden.  In dit artikel bevat een gedetailleerd overzicht van hun ervaringen, inclusief een korte video waarin wordt getoond hoe een Azure-klant kunt Cloudyn en de Azure facturering API's inzicht krijgen in winst van hun verbruiksgegevens Azure.

[Cloud Cruiser en facturering API integratie van Microsoft Azure](billing-usage-rate-card-partner-solution-cloudcruiser.md) wordt beschreven hoe [de Cloud Cruiser Express voor Azure Pack](http://www.cloudcruiser.com/partners/microsoft/) werkt rechtstreeks in de portal WAP klanten naadloos operationele en financiële aspecten van hun Microsoft Azure persoonlijke of gehoste openbare cloud beheren vanuit één gebruikersinterface inschakelen.   

## <a name="next-steps"></a>Volgende stappen
+ Bekijk de [Azure facturering REST API-naslaggids](https://msdn.microsoft.com/library/azure/1ea5b323-54bb-423d-916f-190de96c6a3c) voor meer informatie over beide API's, die deel van de reeks API's die zijn opgegeven door de Azure Resource Manager uitmaken.
+ Als u wilt om in te zoomen rechts in de steekproef-code, raadpleegt u onze Microsoft Azure facturering API Code Samples op [Voorbeelden van Azure-Code](https://azure.microsoft.com/documentation/samples/?term=billing).

## <a name="learn-more"></a>Meer informatie
+ Zie het artikel [Azure resourcemanager overzicht](azure-resource-manager/resource-group-overview.md) voor meer informatie over de resourcemanager van Azure.
+ Voor meer informatie over de suite met hulpprogramma's die nodig is om te helpen inzicht in de cloud krijgt besteedt, Raadpleeg Gartner artikel [Market handleiding voor IT financiële Management (ITFM) hulpmiddelen](http://www.gartner.com/technology/reprints.do?id=1-212F7AL&ct=140909&st=sb).
