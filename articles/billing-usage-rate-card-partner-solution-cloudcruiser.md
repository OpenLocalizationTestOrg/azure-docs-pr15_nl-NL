<properties
   pageTitle="Cloud Cruiser met Microsoft Azure facturering API-integratie | Microsoft Azure"
   description="Biedt een unieke perspectief van Microsoft Azure facturering partner Cloud Cruiser, op hun ervaringen de Azure facturering API's integreren in hun product.  Dit is vooral handig voor klanten met Azure of Cloud Cruiser die met/evalueren Cloud Cruiser voor Microsoft Azure Pack bent geïnteresseerd."
   services=""
   documentationCenter=""
   authors="BryanLa"
   manager="mbaldwin"
   editor=""
   tags="billing"
   />

<tags
   ms.service="billing"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="billing"
   ms.date="09/08/2016"
   ms.author="mobandyo;sirishap;bryanla"/>

# <a name="cloud-cruiser-and-microsoft-azure-billing-api-integration"></a>Cloud Cruiser met Microsoft Azure facturering API-integratie

In dit artikel wordt beschreven hoe de gegevens die worden verzameld uit het nieuwe Microsoft Azure facturering API's in de Cloud Cruiser kan worden gebruikt voor de werkstroom kosten simulatie en analyse.

## <a name="azure-ratecard-api"></a>Azure RateCard-API
De API RateCard verschaft tarief gegevens van Azure. Nadat u hebt met de juiste referenties verifiëren, kunt u de API voor het verzamelen van metagegevens over de beschikbare services op Azure, samen met de tarieven die is gekoppeld aan uw bieden id zoeken

Hieronder ziet u een reactie voorbeeld van de API met de prijzen voor de A0 exemplaar (Windows):

    {
        "MeterId": "0e59ad56-03e5-4c3d-90d4-6670874d7e29",
        "MeterName": "Compute Hours",
        "MeterCategory": "Virtual Machines",
        "MeterSubCategory": "A0 VM (Windows)",
        "Unit": "Hours",
        "MeterRates":
        {
            "0": 0.029
        },
        "EffectiveDate": "2014-08-01T00:00:00Z",
        "IncludedQuantity": 0.0,
        "MeterStatus": "Active"
    },

### <a name="cloud-cruisers-interface-to-azure-ratecard-api"></a>De Interface van Cruiser naar Azure RateCard API cloud
Cloud Cruiser kunt gebruikmaken van de RateCard API-gegevens op verschillende manieren. Voor dit artikel wordt wordt weergegeven hoe deze kan worden gebruikt om te IaaS werkbelasting kosten simulatie en analyse.

U kunt zien deze use-case, stel een werklast van de verschillende exemplaren die zijn uitgevoerd op Microsoft Azure Pack (WAP). Het doel is dit dezelfde werkbelasting op Azure simuleren en schatten van de kosten van dergelijke migratie doen. Om te maken van deze simulaties, zijn er twee belangrijkste taken moet worden uitgevoerd:

1. **Importeren en proces die de gegevens van de API RateCard worden verzameld.** Deze taak wordt ook uitgevoerd op de werkmappen, waarbij de uittreksel uit de API RateCard getransformeerd en die zijn gepubliceerd naar een nieuw tarief-abonnement. Deze nieuwe tarieven-abonnement wordt op de simulaties gebruikt voor het schatten van de Azure prijzen.

2. **Normaliseren WAP services en Azure services voor IaaS.** Standaard WAP services zijn gebaseerd op de afzonderlijke resources (CPU, de grootte van geheugen, schijfgrootte, enzovoort) bij Azure services zijn gebaseerd op exemplaar grootte (A0, A1, A2, enzovoort). Deze eerste taak kan worden uitgevoerd door de Cloud Cruiser ETL engine, genaamd werkmappen, waar deze resources op de exemplaar grootte, vergelijkbaar met Azure exemplaar services zijn gekoppeld.

### <a name="import-data-from-the-ratecard-api"></a>Gegevens importeren uit de RateCard-API

Cloud Cruiser werkmappen bieden een automatische manier voor het verzamelen en verwerken van gegevens uit de API RateCard.  ETL (uitpakken-transformatie-laden) werkmappen kunnen u voor het configureren van de siteverzameling, transformeren en publiceren van gegevens in de Cloud Cruiser-database.

Elke werkmap kan een of meerdere siteverzamelingen, zodat u kunt gegevens uit verschillende bronnen naar aanvulling of uitbreiding van de gegevens over zoekgebruik relateren hebben. De volgende twee schermafbeeldingen weergeven hoe u een nieuwe *siteverzameling* maakt in een bestaande werkmap, en importeren gegevens in de *siteverzameling* uit de API RateCard:

![Afbeelding 1: een nieuwe verzameling][1]

![Afbeelding 2: gegevens importeren uit de nieuwe siteverzameling][2]

Na het importeren van gegevens in de werkmap, is het mogelijk om meerdere stappen en transformatie processen, om te wijzigen en modelleren van de gegevens te maken. In dit voorbeeld omdat we alleen geïnteresseerd infrastructuur-als-een-Service (IaaS) via de stappen transformatie kunnen we onnodige rijen verwijderen of records, gerelateerd aan services dan IaaS.

De volgende schermafbeelding ziet u de transformatie stappen gebruikt voor het verwerken van de gegevens die worden verzameld uit RateCard API:

![Afbeelding 3 - transformatie stappen voor het verwerken van verzamelde gegevens uit RateCard-API][3]

### <a name="defining-new-services-and-rate-plans"></a>Het definiëren van nieuwe Services en tarief van abonnementen

Er zijn verschillende manieren om te definiëren services op Cloud Cruiser. Een van de opties is voor het importeren van de services uit de gebruiksgegevens. Deze methode wordt gebruikt tijdens het werken met openbare wolken, waar de services die al zijn gedefinieerd door de provider.

Een tarief plannen is een verzameling tarieven of prijzen die kunnen worden toegepast op de verschillende services, op basis van begindatums of groep klanten, uit andere opties. Tarieven kunnen ook worden gebruikt in de Cloud Cruiser om simulatie of 'What-if'-scenario's, kunnen begrijpen hoe wijzigingen in services van invloed kunnen zijn op de totale kosten van een werkbelasting te maken.

In dit voorbeeld wordt we de gegevens van de service uit de RateCard-API gebruiken om te definiëren van nieuwe services in de Cloud Cruiser. Op dezelfde manier, kunnen we de tarieven die is gekoppeld aan de services gebruiken om te maken van een nieuwe tarieven plannen op Cloud Cruiser.

Aan het einde van het proces transformatie is het mogelijk te maken van een nieuwe stap en de gegevens uit de API RateCard publiceren als nieuwe services en -tarieven.

![Afbeelding 4 - de gegevens uit de API RateCard publiceren als nieuwe Services en -tarieven][4]

### <a name="verify-azure-services-and-rates"></a>Controleer of Azure en -kosten

Na de services en -tarieven publiceren, kunt u de lijst met geïmporteerde services in de Cloud Cruiser *Services* tabblad controleren:

![Afbeelding 5: de nieuwe Services verifiëren][5]

Klik op het tabblad *Tarief van abonnement* kunt u het nieuwe tarieven plan genoemd 'AzureSimulation' met de tarieven geïmporteerd uit de API RateCard controleren.

![Afbeelding 6 - verifiëren van de nieuwe tarieven plannen en de bijbehorende tarieven][6]

### <a name="normalize-wap-and-azure-services"></a>Normaliseren WAP en Azure Services

Standaard bevat WAP gebruiksinformatie op basis van het gebruik van berekeningscluster, geheugen en netwerk resources. In de Cloud Cruiser, kunt u uw services op basis van rechtstreeks op de toewijzing of datalimiet gebruik van deze resources. U kunt bijvoorbeeld een eenvoudige tarief instellen voor elk uur aan CPU-gebruik of kosten in rekening de GB geheugen toegewezen aan een exemplaar.

In dit voorbeeld om te kunnen vergelijken kosten tussen WAP en Azure, moeten we het Resourcegebruik op WAP in pakketten, die vervolgens kan worden toegewezen aan Azure services aggregeren. Deze transformatie kan eenvoudig worden geïmplementeerd in de werkmappen:

![Afbeelding 7 - WAP gegevens normaliseren services transformeren][7]

De laatste stap op de werkmap is de gegevens in de Cloud Cruiser database publiceren. De gegevens over zoekgebruik is nu tijdens deze stap gebundelde in services (die zijn toegewezen aan de Azure-Services) en gekoppeld aan standaardtarieven maken van de kosten.

Nadat de werkmap is voltooid, kunt u de verwerking van de gegevens, automatiseren door het toevoegen van een taak voor de planner en geven van de frequentie en de tijd voor de werkmap om uit te voeren.

![Figuur 8 - werkmap plannen][8]

### <a name="create-reports-for-workload-cost-simulation-analysis"></a>Rapporten maken voor een werkbelasting kosten simulatie analyse

Nadat de gebruik worden verzameld en kosten in de Cloud Cruiser database zijn geladen, kunnen we gebruikmaken van de module Cloud Cruiser inzichten als u wilt maken van de werklast kosten simulatie die we nodig hebben.

Om u te illustreren dit scenario, wordt het volgende rapport gemaakt:

![Kosten-vergelijking][9]

De bovenste grafiek bevat een kostenvergelijking voor services, de prijs van het uitvoeren van de werklast voor elke specifieke service tussen WAP (Donkerblauw) en Azure (Lichtblauw) vergelijken.

De onder-grafiek dezelfde gegevens worden weergegeven, maar verbroken omlaag door de afdeling. Hiermee worden de kosten voor elke afdeling hun werkbelasting uitvoeren op WAP en Azure, samen met het verschil tussen de notitieblokken op de balk te sparen (groen).

## <a name="azure-usage-api"></a>Azure gebruik API


### <a name="introduction"></a>Inleiding

Microsoft geïntroduceerd onlangs de Azure gebruik API, waarin abonnees via programmacode ophalen van gegevens over zoekgebruik inzichten in hun verbruik krijgen. Dit is een goed nieuws voor Cruiser Cloud-klanten die u van de beschikbaar via deze API rijkere gegevensset profiteren kunt.

Cloud Cruiser kunt gebruikmaken van de integratie met de API voor gebruik op verschillende manieren. De granulatie (per uur gebruiksinformatie) en metagegevens resourcegegevens beschikbaar via de API bevat de benodigde gegevensset ter ondersteuning van flexibele Showback of financiële modellen. 

In deze zelfstudie wordt we een voorbeeld van hoe Cloud Cruiser van de gegevens gebruik API profiteren kunt presenteren. Specifieke taken wordt we een resourcegroep maken op Azure, labels voor de structuur van de account koppelen en het proces van het ophalen en verwerking van de gegevens van de tag in de Cloud Cruiser beschrijven.
 
De uiteindelijke doel is om te kunnen maken van rapporten zoals de volgende en kunnen om kosten en verbruik op basis van de structuur van de account gevuld met de labels te analyseren.

![Afbeelding 10 - rapport met verdeling-labels gebruiken][10]

### <a name="microsoft-azure-tags"></a>Microsoft Azure-labels

De gegevens die beschikbaar zijn via de API voor het gebruik van Azure bevat niet alleen informatie over het energieverbruik, maar ook resource metagegevens, inclusief eventuele tags die is gekoppeld aan. Tags bieden een gemakkelijke manier organiseren van uw resources, maar om effectief, moet u ervoor te zorgen dat:

- Labels worden correct toegepast op de resources tegelijk inrichten
- Labels worden correct op het proces Showback/financiële gebruikt om te koppelen van het gebruik van de organisatie-account structuur.

Beide van de volgende vereisten is lastige, met name wanneer er sprake is van een handmatig proces op de voorziening of opladen in kant. Onjuist getypte, onjuiste of zelfs ontbrekende labels komen veelvoorkomende klachten van klanten bij met labels en deze fouten kan leven op Bezig met laden kant zeer moeilijk maken.

Met de nieuwe Azure gebruik API kan Cloud Cruiser sociaalnetwerklabels resourcegegevens halen, en via een geavanceerde ETL hulpprogramma werkmappen genoemd, oplossen deze veelvoorkomende fouten in sociaalnetwerklabels. Via transformatie met behulp van reguliere expressies en gegevens correlatie, kunt Cloud Cruiser onjuist gelabelde resources identificeren en de juiste tags toepassen ervoor zorgen dat de juiste koppeling van de bronnen aan de consument.

Klik op de geladen kant, Cloud Cruiser automatisch de Showback/financiële en kunt gebruikmaken van de labelgegevens om te koppelen van de gebruik naar de juiste gebruiker (afdeling, afdeling, Project, enzovoort). Deze automatisering biedt een enorme verbetering en kunt ervoor zorgen dat een consistente en kunt controleren geladen proces.
 

### <a name="creating-a-resource-group-with-tags-on-microsoft-azure"></a>Een resourcegroep maken met markeringen in Microsoft Azure
De eerste stap in deze zelfstudie is het opzetten van een resourcegroep in de portal Azure maakt u nieuwe tags wilt koppelen aan de resources. In dit voorbeeld wordt de volgende codes maakt: de eigenaar van de afdeling, omgeving, Project.

De volgende schermafbeelding ziet u een steekproef resourcegroep met de bijbehorende tags.

![Afbeelding 11 - resourcegroep met bijbehorende code op Azure-portal][11]

De volgende stap is het ophalen van de gegevens uit de API voor gebruik in de Cloud Cruiser. De API gebruik vindt momenteel u antwoorden in de indeling van JSON. Hier volgt een voorbeeld van de gegevens die zijn opgehaald:


    {
      "id": "/subscriptions/bb678b04-0e48-4b44-XXXX-XXXXXXX/providers/Microsoft.Commerce/UsageAggregates/Daily_BRSDT_20150623_0000",
      "name": "Daily_BRSDT_20150623_0000",
      "type": "Microsoft.Commerce/UsageAggregate",
      "properties":
      {
        "subscriptionId": "bb678b04-0e48-4b44-XXXX-XXXXXXXXX",
        "usageStartTime": "2015-06-22T00:00:00+00:00",
        "usageEndTime": "2015-06-23T00:00:00+00:00",
        "meterName": "Compute Hours",
        "meterRegion": "",
        "meterCategory": "Virtual Machines",
        "meterSubCategory": "Standard_D1 VM (Non-Windows)",
        "unit": "Hours",
        "instanceData": "{\"Microsoft.Resources\":{\"resourceUri\":\"/subscriptions/bb678b04-0e48-4b44-XXXX-XXXXXXXX/resourceGroups/DEMOUSAGEAPI/providers/Microsoft.Compute/virtualMachines/MyDockerVM\",\"location\":\"eastus\",\"tags\":{\"Department\":\"Sales\",\"Project\":\"Demo Usage API\",\"Environment\":\"Test\",\"Owner\":\"RSE\"},\"additionalInfo\":{\"ImageType\":\"Canonical\",\"ServiceType\":\"Standard_D1\"}}}",
        "meterId": "e60caf26-9ba0-413d-a422-6141f58081d6",
        "infoFields": {},
        "quantity": 8

      },
    },


### <a name="import-data-from-the-usage-api-into-cloud-cruiser"></a>Gegevens importeren vanuit de API voor gebruik in de Cloud Cruiser

Cloud Cruiser werkmappen bieden een automatische manier voor het verzamelen en verwerken van gegevens uit de API gebruik. Een werkmap met ETL (uitpakken-transformatie-laden), kunt u voor het configureren van de siteverzameling, transformeren en publiceren van gegevens in de Cloud Cruiser-database.

Elke werkmap kan een of meer siteverzamelingen hebben. Hiermee kunt u gegevens uit verschillende bronnen naar aanvulling of uitbreiding van de gegevens over zoekgebruik relateren. In dit voorbeeld wordt een nieuw blad maakt in de werkmap Azure-sjabloon (_UsageAPI)_ en een nieuwe _siteverzameling_ om informatie te importeren vanuit de API gebruik instellen.

![Afbeelding 3: gegevens over zoekgebruik API geïmporteerd in het blad UsageAPI][12]

U ziet dat deze werkmap al andere bladen services importeren uit Azure (_ImportServices_), en de gebruiksgegevens van de API facturering (_PublishData_) verwerken.

Volgende we de gebruik-API gebruiken om te vullen van het blad _UsageAPI_ en de gegevens met de verbruik van gegevens uit de API facturering op het blad _PublishData_ relateren.

### <a name="processing-the-tag-information-from-the-usage-api"></a>Verwerking van de tag gegevens uit de API gebruik

We gaan na het importeren van gegevens in de werkmap, transformatie stappen maken in het blad _UsageAPI_ verwerking van de gegevens uit de API. Eerste stap is het gebruik van een processor 'JSON splitsen' in de desbetreffende tags extraheren uit één veld en vervolgens velden maken voor elk van deze (afdeling, Project, eigenaar en omgeving).

![Afbeelding 4 - nieuwe velden voor de labelgegevens maken][13]

Zoals u ziet de service "Netwerken" ontbreekt de labelgegevens (geel vak), maar we kunt controleren of dat deze deel van dezelfde resourcegroep uitmaakt door te kijken in het veld _ResourceGroupName_ . Aangezien we de labels voor de andere resources uit de resourcegroep hebben, kunnen we deze gegevens gebruiken het ontbrekende labels toepassen op deze resource later in het proces.

De volgende stap is het opzetten van een opzoektabel koppelen aan de gegevens van de labels naar de _ResourceGroupName_. Deze opzoektabel wordt worden gebruikt op de volgende stap voor het gebruik van gegevens met labelinformatie verrijken.

### <a name="adding-the-tag-information-to-the-consumption-data"></a>De labelinformatie toevoegen aan het gebruik van gegevens

We kunnen nu gaan naar het blad _PublishData_ , die de gebruiksgegevens van de API facturering verwerkt, en de velden opgehaald uit de desbetreffende tags hebt toegevoegd. Dit proces wordt uitgevoerd door te kijken in de opzoektabel die is gemaakt in de vorige stap de _ResourceGroupName_ gebruiken als de sleutel voor zoekopdrachten.

![Afbeelding 5: de structuur van de account met de gegevens uit de zoekacties vullen][14]

Zoals u ziet dat de juiste account structuurvelden voor de service "Netwerken" zijn toegepast, het probleem op te lossen met de ontbrekende tags. We ook de account structuurvelden voor resources dan ons doel resourcegroep met "Overig" ingevuld om te kunnen onderscheiden op de rapporten.

Nu moeten alleen we een stap voor het publiceren van de gegevens over zoekgebruik toevoegen. Tijdens deze stap wordt de juiste tarieven die voor elke service die is gedefinieerd op het plannen van onze tarief worden toegepast op de informatie over het gebruik, met de resulterende kosten in de database geladen.

Het beste onderdeel is dat u alleen dit proces eenmaal doorlopen. Wanneer de werkmap is voltooid, hoeft u te toe te voegen aan de planner en wordt deze per uur of dagelijks uitgevoerd op het geplande tijdstip. Is het alleen een kwestie van nieuwe rapporten maken of aanpassen van bestaande ideeën, om te kunnen de gegevens als u betekenisvolle inzichten vanuit de cloud-gebruik analyseren.

### <a name="next-steps"></a>Volgende stappen

+ Raadpleeg de Cloud Cruiser online [documentatie](http://docs.cloudcruiser.com/) (geldige aanmelden vereist) voor uitgebreide instructies over het maken van Cloud Cruiser werkmappen en -rapporten.  Voor meer informatie over Cloud Cruiser, neem contact op met [info@cloudcruiser.com](mailto:info@cloudcruiser.com).
+ Zie [inzicht in uw verbruik van Microsoft Azure krijgen](billing-usage-rate-card-overview.md) voor een overzicht van de Azure Resourcegebruik en de RateCard APIs.
+ Bekijk de [Azure facturering REST API-naslaggids](https://msdn.microsoft.com/library/azure/1ea5b323-54bb-423d-916f-190de96c6a3c) voor meer informatie over beide API's, die deel van de reeks API's die zijn opgegeven door de Azure Resource Manager uitmaken.
+ Als u wilt om in te zoomen rechts in de steekproef-code, raadpleegt u onze Microsoft Azure facturering API Code Samples op [Voorbeelden van Azure-Code](https://azure.microsoft.com/documentation/samples/?term=billing).

### <a name="learn-more"></a>Meer informatie
+ Zie het artikel [Azure resourcemanager overzicht](azure-resource-manager/resource-group-overview.md) voor meer informatie over de resourcemanager van Azure.

<!--Image references-->
 
[1]: ./media/billing-usage-rate-card-partner-solution-cloudcruiser/Create-New-Workbook-Collection.png "Afbeelding 1: een nieuwe verzameling"
[2]: ./media/billing-usage-rate-card-partner-solution-cloudcruiser/Import-Data-From-RateCard.png "Afbeelding 2: gegevens importeren uit de nieuwe siteverzameling"
[3]: ./media/billing-usage-rate-card-partner-solution-cloudcruiser/Transformation-Steps-Process-RateCard-Data.png "Afbeelding 3 - transformatie stappen voor het verwerken van verzamelde gegevens uit RateCard-API"
[4]: ./media/billing-usage-rate-card-partner-solution-cloudcruiser/Publish-RateCard-Data-New-Services-Rates.png "Afbeelding 4 - de gegevens uit de API RateCard publiceren als nieuwe Services en -tarieven"
[5]: ./media/billing-usage-rate-card-partner-solution-cloudcruiser/Verify-Azure-Services-And-Pricing1.png "Afbeelding 5: de nieuwe Services verifiëren"
[6]: ./media/billing-usage-rate-card-partner-solution-cloudcruiser/Verify-Azure-Services-And-Pricing2.png "Afbeelding 6 - verifiëren van de nieuwe tarieven plannen en de bijbehorende tarieven"
[7]: ./media/billing-usage-rate-card-partner-solution-cloudcruiser/Transforming-WAP-Normalize-Services.png "Afbeelding 7 - WAP gegevens normaliseren services transformeren"
[8]: ./media/billing-usage-rate-card-partner-solution-cloudcruiser/Workbook-Scheduling.png "Figuur 8 - werkmap plannen"
[9]: ./media/billing-usage-rate-card-partner-solution-cloudcruiser/Workload-Cost-Simulation-Report.png "Afbeelding 9 - voorbeeldrapport voor het scenario werkbelasting kosten-vergelijking"
[10]: ./media/billing-usage-rate-card-partner-solution-cloudcruiser/1_ReportWithTags.png "Afbeelding 10 - rapport met verdeling-labels gebruiken"
[11]: ./media/billing-usage-rate-card-partner-solution-cloudcruiser/2_ResourceGroupsWithTags.png "Afbeelding 11 - resourcegroep met bijbehorende code op Azure-portal"
[12]: ./media/billing-usage-rate-card-partner-solution-cloudcruiser/3_ImportIntoUsageAPISheet.png "Figuur 12 - API gebruiksgegevens geïmporteerd in het blad UsageAPI"
[13]: ./media/billing-usage-rate-card-partner-solution-cloudcruiser/4_NewTagField.png "Afbeelding 13 - nieuwe velden voor de labelgegevens maken"
[14]: ./media/billing-usage-rate-card-partner-solution-cloudcruiser/5_PopulateAccountStructure.png "Figuur 14 - vullen de structuur van de account met de gegevens uit de zoekacties"
