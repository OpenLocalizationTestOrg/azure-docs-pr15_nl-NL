<properties 
    pageTitle="Use-Case - klant profielen" 
    description="Leer hoe Azure gegevens Factory wordt gebruikt voor het maken van een werkstroom gegevensgestuurde (verkooppijplijn) om te spellen klanten profiel." 
    services="data-factory" 
    documentationCenter="" 
    authors="sharonlo101" 
    manager="jhubbard" 
    editor="monicar"/>

<tags 
    ms.service="data-factory" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/06/2016" 
    ms.author="shlo"/>

# <a name="use-case---customer-profiling"></a>Use-Case - klant profielen

Azure gegevens Factory is een van de vele services gebruikt voor het implementeren van de Suite Cortana bedrijfsinformatie van oplossing accelerators.  Voor meer informatie over Cortana bedrijfsinformatie, gaat u naar [Cortana Intelligence-Suite](http://www.microsoft.com/cortanaanalytics). In dit document beschrijven we een eenvoudige use-case waarmee u aan de slag met informatie over hoe Azure gegevens Factory algemene analytics problemen kunt oplossen.

U hoeft te openen en probeer deze eenvoudige use-case is een [Azure-abonnement](https://azure.microsoft.com/pricing/free-trial/).  U kunt een steekproef dat deze use-case implementeert door de stappen in het artikel [voorbeelden](data-factory-samples.md) implementeren.

## <a name="scenario"></a>Scenario

Contoso is een spellen bedrijf waarmee spellen voor meerdere platforms: consoles, handje gehouden apparaten en personal computers (pc's). Als deze spelen spelers bevat, wordt grote hoeveelheid logboekgegevens geproduceerd waarin de gebruikspatronen, spellen stijl en voorkeuren van de gebruiker worden bijgehouden.  In combinatie met demografische, regionale, en productgegevens, Contoso analyses om te helpen ze over het verbeteren van spelers ervaring kunt uitvoeren en doel ze over upgrades en spel aankopen doet. 

Van Contoso doel is om te verkopen/cross-Upsell verkoopkansen op basis van de spellen geschiedenis van de spelers identificeren en aantrekkelijke functies toevoegen aan station zakelijke groei en klanten een betere ervaring beschikken. Voor deze use-case gebruiken we een bedrijf spellen als een voorbeeld van een bedrijf. Het bedrijf wil haar spellen op basis van spelers gedrag optimaliseren. Deze beginselen toepassen op een bedrijf dat wil deelnemen van zijn klanten rond de goederen en diensten en hun klanten ervaring te verbeteren.

## <a name="challenges"></a>Uitdagingen


## <a name="solution-overview"></a>Overzicht van oplossingen

Dit eenvoudige use-case kan worden gebruikt als een voorbeeld van hoe u Azure gegevens Factory kunt nemen, voorbereiden transformeren, analyseren en publiceren van gegevens.

![End-to-end-werkstroom](./media/data-factory-customer-profiling-usecase/EndToEndWorkflow.png)

Deze afbeelding ziet hoe de pijpleidingen gegevens weergegeven in de portal van Azure nadat ze zijn geïmplementeerd.

1.  De **PartitionGameLogsPipeline** leest de onbewerkte spel gebeurtenissen van blobopslag en maakt een partities op basis van het jaar, maand en dag.
2.  De **EnrichGameLogsPipeline** gepartitioneerde spel gebeurtenissen met geografische code referentiegegevens deelneemt aan de vergadering en de gegevens door het IP-adressen toewijzen aan de bijbehorende geografische-locaties cursussen.
3.  De pijplijn **AnalyzeMarketingCampaignPipeline** beschikt over de uitgebreide gegevens en verwerkt deze met de gegevens reclame de uiteindelijke uitvoer met marketing campaign effectiviteit maken.

In dit voorbeeld wordt de Data Factory goedkeuringen activiteiten die invoergegevens, transformeren en proces de gegevens kopiëren en de uiteindelijke gegevens met een Azure SQL-Database gebruikt.  U kunt ook het netwerk van pijpleidingen gegevens visualiseren, beheren en hun status van de gebruikersinterface controleert.

## <a name="benefits"></a>Voordelen

Door het optimaliseren van de gebruikersprofiel analytics en deze aan de bedrijfsdoelstellingen uitlijnen, kan spellen bedrijf snel gebruikspatronen verzamelen en de effectiviteit van marketingcampagnes analyseren.




