<properties
    pageTitle="Hoe u scenario's bepalen en plannen voor geavanceerde gegevens analyseverwerking | Microsoft Azure"
    description="Plan voor geavanceerde analyses op basis van een reeks belangrijke vragen."
    services="machine-learning"
    documentationCenter=""
    authors="bradsev"
    manager="jhubbard"
    editor="cgronlun" />

<tags
    ms.service="machine-learning"
    ms.workload="data-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/19/2016"
    ms.author="bradsev" />


# <a name="how-to-identify-scenarios-and-plan-for-advanced-analytics-data-processing"></a>Het identificeren van scenario's en het gebruik van geavanceerde analyses gegevensverwerking plannen

Welke resources moet u van plan bent om op te nemen bij het instellen van een omgeving geavanceerde analyses processing op een gegevensset doen? Dit artikel bevat een aantal vragen om te vragen die helpt de taken en relevante resources identificeren uw scenario. De volgorde van hoofdstappen voor blog analytics geeft een overzicht van [Wat is het Team gegevens wetenschap proces (TDSP)?](data-science-process-overview.md). Elk van deze stappen moeten worden specifieke resources voor de taken die relevant zijn voor uw specifieke scenario. De belangrijkste vragen om aan te geven van uw scenario betrekking hebben op gegevens logistiek, kenmerken, de kwaliteit van de gegevenssets, en de hulpprogramma's en talen die u wilt de analyse uitvoeren.

[AZURE.INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

## <a name="logistic-questions-data-locations-and-movement"></a>Logistieke vragen: locaties van de gegevens en verplaatsing
De logistieke vragen betrekking hebben op de locatie van de **gegevensbron**, de **doelgroep bestemming** in Azure wordt aangegeven en vereisten voor het verplaatsen van de gegevens, inclusief de planning, het bedrag en de resources die nodig zijn. Mogelijk moet u de gegevens moeten worden enkele malen verplaatst tijdens het analytics. Er is een algemene scenario lokale om gegevens te verplaatsen in een vorm van opslagruimte op Azure en vervolgens naar Machine Learning Studio.

1. **Wat is uw gegevensbron?** Is het lokale of in de cloud? Bijvoorbeeld:
    - De gegevens openbaar beschikbaar is binnen een HTTP-adres.
    - De gegevens zich bevinden in een LAN/bestandslocatie.
    - De gegevens zich in een SQL Server-database.
    - De gegevens worden opgeslagen in een container Azure opslag

2. **Wat is de bestemming van de Azure?** Waar moet dit worden verwerkt of modeling? Bijvoorbeeld:
    - Azure-blobopslag
    - SQL Azure-databases
    - SQL Server Azure VM
    - HDInsight (Hadoop op Azure) of component tabellen
    - Azure Machine Learning
    - Mogelijk Azure virtuele vaste schijven.

3. **Hoe gaat u om de gegevens te verplaatsen?** De procedures en bronnen die beschikbaar zijn om te nemen of gegevens laadt in een groot aantal verschillende opslag en verwerking van omgevingen weergegeven in de volgende onderwerpen.

    -  [Gegevens laadt in omgevingen met opslag van analysegegevens](machine-learning-data-science-ingest-data.md)
    -  [Uw trainingsgegevens naar Azure Machine Learning Studio uit verschillende gegevensbronnen importeren](machine-learning-data-science-import-data.md).

4. **Moet de gegevens worden worden verplaatst regelmatig of gewijzigd tijdens de migratie?** Overweeg het gebruik van Azure gegevens Factory (ADF) wanneer de gegevens moet worden continu gemigreerd, met name als een hybride scenario dat toegang zoekt tot beide on-premises en cloud resources betrokken is, of wanneer de gegevens is verwerkt of moet worden gewijzigd of bedrijfslogica toegevoegd in de loop van wordt gemigreerd. Zie voor meer informatie, [gegevens van een on-premises SQL-server naar SQL Azure wordt aangegeven met Azure gegevens Factory verplaatsen](machine-learning-data-science-move-sql-azure-adf.md)

5. **Hoeveel van de gegevens is moeten worden verplaatst naar Azure?** Gegevenssets die zeer grote zijn mogelijk langer zijn dan de opslagcapaciteit van bepaalde omgevingen. Zie het onderwerp van de maximale grootte voor Machine Learning Studio in het volgende gedeelte voor een voorbeeld. In dit geval mogelijk een voorbeeld van de gegevens worden gebruikt tijdens de analyse. Een gegevensset in diverse Azure omgevingen, Zie voor meer informatie over hoe u omlaag gepaarde steekproeven [voorbeeldgegevens in het Team gegevens wetenschap proces](machine-learning-data-science-sample-data.md).


## <a name="data-characteristics-questions-type-format-and-size"></a>Gegevens kenmerken vragen: type, indeling en grootte
Deze vragen zijn toets tot het plannen van uw opslag en verwerken omgevingen, die zijn geschikt zijn voor verschillende gegevenstypen en elk van die bepaalde beperkingen hebben.

1. **Wat zijn de gegevenstypen?** Bijvoorbeeld:
    - Numerieke
    - Bestaan
    - Tekenreeksen
    - Binaire

2. **Hoe is uw gegevens opgemaakt?** Bijvoorbeeld:
    - Door komma's gescheiden (CSV) of tabs gescheiden (TSV) platte bestanden
    - Wel of niet gecomprimeerd
    - Azure BLOB 's
    - Hadoop-component tabellen
    - SQL Server-tabellen

2. **Hoe groot is uw gegevens?**
    - Kleine: minder dan 2GB
    - Gemiddeld: Groter zijn dan 2GB en minder dan 10GB
    - Grote: Groter is dan 10GB

Neem de Azure Machine Learning Studio-omgeving bijvoorbeeld:

- Voor een lijst met de gegevensindelingen en de typen die worden ondersteund door Azure Machine Learning Studio, Zie de sectie [gegevens notaties en gegevens gegevenstypen die worden ondersteund](machine-learning-data-science-import-data.md#data-formats-and-data-types-supported) .
- Zie voor informatie over beperkingen van de gegevens voor Azure Machine Learning Studio, de **hoe groot kan de gegevensset zijn voor mijn modules?** gedeelte van het [importeren en exporteren van gegevens voor Machine Learning](machine-learning-faq.md#machine-learning-studio-questions)

Zie [Azure-abonnement en limieten van de Service, quota's en beperkingen](../azure-subscription-service-limits.md)voor informatie over de beperkingen van andere Azure services gebruikt in het proces analyses.

## <a name="data-quality-questions-exploration-and-pre-processing"></a>Gegevens kwaliteit vragen: verkennen en oude verwerking

1. **Wat weet u over uw gegevens?** Gegevens verkennen wanneer u moet een informatie over de basiskenmerken krijgen. Wat patronen en trends weer deze voorwerpen, wat uitschieters is heeft of het aantal waarden zijn verdwenen. Deze stap is het belangrijk is voor het bepalen van de mate van oude verwerking nodig voor formulering hypothesen die kunnen suggestie voor de meest geschikte functies of typ analyse en voor het opstellen van plannen voor aanvullende gegevens verzamelen. Beschrijvende statistiek berekenen en uitzetten van visualisaties zijn nuttige technieken voor gegevens inspectie. Zie [gegevens verkennen in het Team gegevens wetenschap proces](machine-learning-data-science-explore-data.md)voor meer informatie over het verkennen van een gegevensset in diverse omgevingen met Azure.

2. **Vereist de gegevens voorverwerking of moet worden opgeschoond?**
Voorverwerking en opschonen van gegevens zijn belangrijke taken die meestal moeten worden uitgevoerd voordat gegevensset effectief kan worden gebruikt voor machine learning. Onbewerkte gegevens is vaak ruis en onbetrouwbare en waarden ontbreekt. Met behulp van dergelijke gegevens voor modellering kan misleidende resultaten opleveren. Zie [taken voor het voorbereiden van gegevens voor uitgebreide machine learning](machine-learning-data-science-prepare-data.md)voor een beschrijving.

## <a name="tools-and-languages-questions"></a>Hulpprogramma's en talen vragen
Zijn er veel opties hier afhankelijk van welke talen en ontwikkeling omgevingen of hulpmiddelen voor u nodig hebt, of meest conformable gebruikt.

1. **Welke talen wilt u gebruiken voor analyse?**  
    - R
    - Python
    - SQL

2. **Welke hulpprogramma's moet u gebruiken voor gegevensanalyse?**
    - [Microsoft Azure Powershell](powershell-install-configure.md) - een scripttaal gebruikt voor het beheer van uw Azure resources in een scripttaal.
    - [Azure Machine Learning Studio](machine-learning-what-is-ml-studio/)
    - [Revolutie Analytics](http://www.revolutionanalytics.com/revolution-r-open)
    - [RStudio](http://www.rstudio.com)
    - [Python Tools voor Visual Studio](http://microsoft.github.io/PTVS/)
    - [Anaconda](https://www.continuum.io/why-anaconda)
    - [Jupyter notitieblokken](http://jupyter.org/)
    - [Microsoft Power BI](http://powerbi.microsoft.com)


## <a name="identify-your-advanced-analytics-scenario"></a>Uw scenario geavanceerde analyses identificeren
Nadat u de vragen in de vorige sectie hebt beantwoord, bent u gereed om te bepalen welk scenario beste past de zaak. Scenario's voor de steekproef worden omlijnd in [scenario's voor geavanceerde analyses in Azure Machine Learning](machine-learning-data-science-plan-sample-scenarios.md).
