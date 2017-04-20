<properties
   pageTitle="Gebruik van Microsoft Azure en inschakelen van RateCard-API's Cloudyn ITFM om klanten te bieden | Microsoft Azure"
   description="Biedt een unieke perspectief van Microsoft Azure facturering partner Cloudyn, op hun ervaringen de Azure facturering API's integreren in hun product.  Dit is vooral handig voor klanten met Azure of Cloudyn die met/evalueren Cloudyn voor Azure Services bent geïnteresseerd."
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

# <a name="microsoft-azure-usage-and-ratecard-apis-enable-cloudyn-to-provide-itfm-for-customers"></a>Gebruik van Microsoft Azure en RateCard API's inschakelen Cloudyn ITFM om klanten te bieden

Cloudyn, een Microsoft-partner ontwikkelen en een toonaangevende leverancier van cloud beheermogelijkheden, is gekozen voor een persoonlijke voorbeeld van het nieuwe Microsoft Azure Resourcegebruik en RateCard APIs.  De API gebruik biedt toegang tot geschatte Azure verbruik van gegevens voor een abonnement. De API RateCard biedt voltooid prijsinformatie van alle Azure services, voor niet - Enterprise Agreement EA klanten. Geïntegreerd samen, bieden deze API's voor de basis van de volledige informatie om invoer in een hulpmiddel IT financiële Management (ITFM) zoals die Cloudyn gegeven.

## <a name="introduction"></a>Inleiding

De zogenaamde "vermenigvuldiging' van gegevens uit de API voor gebruik met gegevens uit de API RateCard (gebruik [eenheden] [$unit] prijs = gedetailleerde gebruik en kosten) wordt gemaakt van de meest gedetailleerde, nauwkeurige en betrouwbare factureringsgegevens beschikbaar voor Azure vandaag.

![ITFM overzicht][1]

Gebruik deze API's bevat belangrijke informatie op gebruik en kosten, Cloudyn te analyseren klant-accounts op een eenvoudige, programma manier en verschillende ITFM taken uitvoeren voor de klanten zodat uw klanten.

## <a name="integrating-cloudyn-with-the-ratecard-and-usage-apis"></a>Cloudyn integreren met de RateCard en gebruik API 's
De API RateCard zijn verschillende invoerparameters--zoals regio info, valuta en landinstelling--vereist, maar het belangrijkst is OfferDurableID, waarmee het type aanbod van de klant Azure gebruikt (Pay-as-you-Go, oudere 6 en 12 maanden resourcebeperkingen-abonnementen, MSDN aanbiedingen, MPN aanbiedingen, aanbiedingen en anderen). De OfferDurableID vindt u in het [gebruik van de Azure en facturering portal](https://account.windowsazure.com/Subscriptions), onder de "bieden-ID" voor het opgegeven abonnement.

Zich registreert voor [Cloudyn voor Azure](https://www.cloudyn.com/microsoft-azure/) -services, kunnen klanten hun code OfferDurableID, waarmee Cloudyn om op te halen relevante prijsgegevens via de API RateCard toevoegen.  Informatie over de verschillende soorten aanbiedingen vindt u een de detailpagina van [Microsoft Azure](https://azure.microsoft.com/support/legal/offer-details/) -bieden.

![Overzicht van Cloudyn ITFM Engine][2]

Cloudyn gebruik zowel de gebruik en de RateCard APIs's behalve de API van Azure prestaties, maakt u aanvullende lagen van visualisatie, analyses, waarschuwingen, rapportage, kostenbeheer en uitvoerbare aanbevelingen biedt Azure klanten een betrouwbare enterprise cloud ITFM hulpmiddel.

## <a name="cloudyn-itfm-use-cases-enabled-by-usage-and-ratecard-api-integration"></a>Cloudyn ITFM gevallen ingeschakeld door gebruik en RateCard API-integratie gebruiken
Algemene Cloudyn ITFM gebruik gevallen ingeschakeld door gebruik en RateCard APIs opnemen:

+ **Kosten-analyse** - kunt cloud kosten worden verbroken naar beneden af op een systeemeigen kenmerk dimensie (provider, service, account, regio, enzovoort). Het gebruik van de Azure en de RateCard APIs hiervan een eenvoudige taak, doordat de meest uitgesplitste uitsplitsing van gebruik en kostengegevens per account, die vervolgens gegroepeerd en gefilterd op Cloudyn en aan de gebruiker, in de vorm van een afbeelding of een tabelvormige gepresenteerd.

![Kosten van analyse van-cirkeldiagram][3]

+ **Kosten toewijzing 360** - Hiermee Financiën en IT-beheerders om de verdeling van de werkelijke kosten, de stuurprogramma's en de ontwikkeling van hun cloud-implementatie. Deze verder kan managers eenvoudig koppelen implementatie uitgaven bedrijfseenheden afdelingen, regio's en meer, leveren ongekende inzicht krijgen in de cloud kosten en om enterprise terugboekingen en showbacks te vergemakkelijken. Het gebruik van de Azure en de RateCard APIs fungeren als invoer voor Cloudyn van kosten toewijzing engine, die de API's aanvult door methoden en bedrijfslogica voor het toewijzen van resources voor niet-gelabelde of untaggable te definiëren.

![Kosten toewijzing 360 grafiek][4]

+ **Cost-Effective hoekformaatgreep** - bevat rechts formaatfunctie aanbevelingen voor weinig werk virtuele machines, zodat de uitgaven van de klant op te groot of te weinig ingerichte computers wordt beperkt. Dit gebeurt met onderzoeken VM CPU en RAM aan de doelstellingen (via prestaties API), uren van runtime (via gebruik API) en kosten (via RateCard API). Cloudyn rechts formaatfunctie aanbevelingen op basis van weinig werk processor of RAM bronnen (prestaties), en geschatte spaargeld berekend vermenigvuldigd met de prijs delta (RateCard) tussen de VMs door de werkelijke tijd-gebruik (gebruik) van de computer weinig werk.

![Kosten effectieve formaat wijzigen][5]

+ **Cloud overbrengen aanbevelingen** - biedt financiële advies aan cloud overbrengen. Wordt de huidige kosten van een gebruiker van cloud-resources die zijn geïmplementeerd op primaire cloud leveranciers, en wordt deze vergeleken met de kosten van een equivalente implementatie op Azure. Vervolgens wordt uitgesplitste, per resource, financieel gebaseerde aanbevelingen Azure overbrengen. Na beoordeling van de overeenkomstige implementatie vereist op Azure (op basis van prestaties maatstaven en gebruiker voorkeuren), gebruikt Cloudyn de RateCard-API de kosten van de overeenkomstige implementatie op Azure wordt berekend.

+ **Rapporten over de werkstroomprestaties** - ingeschakeld door de prestaties van Azure API, deze rapporten bieden een matrix van functies van processor en RAM gebruik optimalisatie aanbevelingen. Hierna ziet u een exemplaar gebruik rapportvoorbeeld, exemplaar uitgesplitste presenteren door gemiddelde CPU-gebruik is.

![Rapporten over de werkstroomprestaties][6]

+ **Categoriemanager** - een krachtige functie in Cloudyn die volgorde naar ongeordende cloud resources brengt. Gebruikers krijgen het vrij hun eigen unieke categorieën (tags) voor het effectieve meten en rapporten maken die is loopt gelijk met de bedrijfsvoering. Bovendien kunnen gebruikers eenvoudig regelen en categoriseren inconsistente labelen (dat wil zeggen typefouten en andere verschillen) en niet-gelabelde resources voor nauwkeurige kosten afschrijving automatisch detecteren.

![Categorie Manager][7]

## <a name="video"></a>Video

Hier ziet u een korte video waarin wordt getoond hoe een Azure-klant Cloudyn voor Azure en de Azure facturering API's, inzichten die hun verbruiksgegevens Azure krijgen kunt gebruiken.

> [AZURE.VIDEO cloudyn-provides-cloud-itfm-tools-via-microsoft-azure-apis]


## <a name="next-steps"></a>Volgende stappen

+ Start een gratis proefversie [Cloudyn voor Azure](https://www.cloudyn.com/microsoft-azure/) om te zien hoe u kosten transparantie met aangepaste rapportage en analyse kunt aanvragen voor uw Microsoft Azure cloud-implementatie.
+ Zie [inzicht in uw verbruik van Microsoft Azure krijgen](billing-usage-rate-card-overview.md) voor een overzicht van de Azure Resourcegebruik en de RateCard APIs.
+ Bekijk de [Azure facturering REST API-naslaggids](https://msdn.microsoft.com/library/azure/1ea5b323-54bb-423d-916f-190de96c6a3c) voor meer informatie over beide API's, die deel van de reeks API's die zijn opgegeven door de Azure Resource Manager uitmaken.
+ Als u wilt om in te zoomen rechts in de steekproef-code, raadpleegt u onze Microsoft Azure facturering API Code Samples op [Voorbeelden van Azure-Code](https://azure.microsoft.com/documentation/samples/?term=billing).

## <a name="learn-more"></a>Meer informatie
+ Meer informatie over Microsoft Azure Enterprise Agreement (EA) aanbiedingen, gaat u naar [Azure licenties voor de onderneming] (https://azure.microsoft.com/pricing/enterprise-agreement/)
+ Zie het artikel [Azure resourcemanager overzicht](azure-resource-manager/resource-group-overview.md) voor meer informatie over de resourcemanager van Azure.
+ Voor meer informatie over de suite met hulpprogramma's die nodig is om te helpen inzicht in de cloud krijgt besteedt, Raadpleeg Gartner artikel [Market handleiding voor IT financiële Management (ITFM) hulpmiddelen](http://www.gartner.com/technology/reprints.do?id=1-212F7AL&ct=140909&st=sb).

<!--Image references-->
[1]: ./media/billing-usage-rate-card-partner-solution-cloudyn/Cloudyn-ITFM-Overview.png
[2]: ./media/billing-usage-rate-card-partner-solution-cloudyn/Cloudyn-ITFM-Engine-Overview.png
[3]: ./media/billing-usage-rate-card-partner-solution-cloudyn/Cloudyn-Cost-Analysis-Pie-Chart.png
[4]: ./media/billing-usage-rate-card-partner-solution-cloudyn/Cloudyn-Cost-Allocation-360-Chart.png
[5]: ./media/billing-usage-rate-card-partner-solution-cloudyn/Cloudyn-Cost-Effective-Sizing.png
[6]: ./media/billing-usage-rate-card-partner-solution-cloudyn/Cloudyn-Performance-Reports.png
[7]: ./media/billing-usage-rate-card-partner-solution-cloudyn/Cloudyn-Category-Manager.png
