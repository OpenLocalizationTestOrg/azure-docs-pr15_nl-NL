<properties 
    pageTitle="Wat is Team gegevens wetenschap proces?  | Microsoft Azure" 
    description="Het Team gegevens wetenschap proces is een systematische methode voor het samenstellen van intelligente toepassingen die gebruikmaken van geavanceerde analyses." 
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


# <a name="what-is-the-team-data-science-process-tdsp"></a>Wat is het Team gegevens wetenschap proces (TDSP)?

Het [Team gegevens wetenschap proces (TDSP)](data-science-process-overview.md) biedt een systematische aanpak tot het bouwen van intelligente toepassingen waarmee teams van gegevens wetenschappers om samen te werken effectief gedurende de volledige levenscyclus van activiteiten die nodig zijn om te schakelen deze toepassingen in de producten. De TDSP vindt u een overzicht van een reeks stappen met **informatie** over het definiëren van het probleem, instellen van de hulpmiddelen en omgeving nodig relevante gegevens analyseren, maken en bekijk modellen geëvalueerd en vervolgens deze modellen in bedrijfstoepassingen te implementeren. 

Hier volgen de stappen in **Team gegevens wetenschap proces**:  

![Initiaal-werkstroom](./media/machine-learning-data-science-the-cortana-analytics-process/CAP-workflow.png)

Het proces is **iteratieve**: het lidmaatschap van nieuwe en bestaande of verfijningen in het model wordt uitgebreid en vereist hoeven eerder voltooid in de reeks stappen aanpassen. Bestaande organisatie-ontwikkeling en processen voor projectplanning zijn **eenvoudig aangepast** voor gebruik met de TDSP gedefinieerde reeks stappen. 

De stappen in het proces zijn in diagrammen en klik in de [TDSP leerpad](https://azure.microsoft.com/documentation/learning-paths/cortana-analytics-process/) gekoppeld en hieronder beschreven.  

## <a name="preparation-steps"></a>Voorbereidende stappen 

## <a name="p1-plan-the-analytics-project"></a>P1. Het project analytics plannen 

Een project analytics starten door uw zakelijke doelstellingen en problemen te definiëren. Ze zijn opgegeven in de **zakelijke behoeften**. Een centrale doel van deze stap is om aan te geven van de variabelen voor belangrijke zakelijke (verkoop voorspellen of de kans van een volgorde wordt frauduleuze, bijvoorbeeld) die de analyse moet voorspellen om te voldoen aan deze vereisten is voldaan. Extra planning is vervolgens meestal essentieel voor het ontwikkelen van een inzicht in de **gegevensbronnen** die nodig zijn voor het adres van de doelstellingen van het project uit een analytical perspectief. Gebeurt niet, bijvoorbeeld om te zoeken dat bestaande systemen voor het verzamelen en meld u extra soorten gegevens om het probleem te bereiken van de doelen van het project nodig hebt. Zie [uw omgeving voor het Team gegevens wetenschap proces plannen](machine-learning-data-science-plan-your-environment.md) en [scenario's voor geavanceerde analyses in Azure Machine Learning](machine-learning-data-science-plan-sample-scenarios.md)voor instructies.  

## <a name="p2-setup-analytics-environment"></a>P2. Setup analytics-omgeving 

Een analytics-omgeving voor het Team gegevens wetenschap proces bestaat uit verschillende onderdelen: 

- **gegevens werkruimten** waar de gegevens voor analyse en modellering, is gefaseerde 
- een **processing infrastructuur** voor voorverwerking, verkennen en de gegevens wilt modelleren
- een **runtime-infrastructuur** voor de modellen voor analytical mogelijk te maken en uitvoeren van de intelligente clienttoepassingen die gebruik de modellen voor.  

De analytics-infrastructuur dat moet worden ingesteld uitmaakt vaak deel van een omgeving die losstaat van core operationele systemen. Maar dit meestal maakt gebruik van gegevens uit meerdere systemen binnen de onderneming als videobronnen buiten het bedrijf. De analytics-infrastructuur is zuiver cloudgebaseerde, of een on-premises implementatie-installatie, of een hybride van de twee. Zie Opties voor het [instellen van gegevens wetenschappelijke omgevingen voor gebruik in het Team gegevens wetenschap proces](machine-learning-data-science-environment-setup.md).

## <a name="analytics-steps"></a>Analytics stappen:  

## <a name="1-ingest-data-into-the-analytical-environment"></a>1. nemen gegevens in het analytical milieu 

De eerste stap is om te zorgen dat de relevante gegevens uit verschillende bronnen, hetzij van binnen of van buiten de onderneming, in een analytisch omgevingen waar de gegevens kan worden verwerkt. De **Opmaak** van de gegevens aan de bron kan afwijken van de indeling van de bestemming. Dus sommige gegevenstransformatie mogelijk ook moet worden uitgevoerd door de opname tooling. Zie Opties voor het [laden gegevens in de opslagomgevingen van analysegegevens](machine-learning-data-science-ingest-data.md)

Naast het eerste innemen van gegevens moeten veel intelligente toepassingen de gegevens regelmatig vernieuwen als onderdeel van een lopende learning-proces. Dit kunt doen door het instellen van een **gegevens verkooppijplijn** of de werkstroom. Hiermee maakt deel uit van het iteratieve deel van het proces met PowerPoint opnieuw moet maken en opnieuw evaluatie van de analytical modellen die worden gebruikt door de intelligente toepassing implementeren van de oplossing. Zie bijvoorbeeld de [gegevens van een on-premises SQL-server naar SQL Azure wordt aangegeven met Azure gegevens Factory verplaatsen](machine-learning-data-science-move-sql-azure-adf.md).


## <a name="2-explore-and-pre-process-data"></a>2. verkennen en gegevens vooraf verwerken 

De volgende stap is voor een beter begrip van de gegevens met wordt onderzocht haar **samenvattingsgegevens** , relaties en met behulp van technieken zoals **visualisatie**. Dit is ook waar problemen van **kwaliteit van de gegevens** en integriteit, zoals ontbrekende waarden, gegevenstype niet overeenkomt en inconsistente gegevensrelaties worden verwerkt. Vooraf verwerking transformaties worden gebruikt om op te schonen de onbewerkte gegevens voor verdere analyse en modellering kan plaatsvinden. Zie [taken voor het voorbereiden van gegevens voor uitgebreide machine learning](machine-learning-data-science-prepare-data.md)voor een beschrijving.


## <a name="3-develop-features"></a>3. ontwikkelen functies 

Gegevens wetenschappers, in samenwerking met domein experts, moeten de functies die de belangrijke eigenschappen van de gegevensset vastleggen en die best kunnen worden gebruikt om de belangrijke zakelijke variabelen die zijn geïdentificeerd tijdens het plannen voorspellen identificeren. Deze nieuwe functies kunnen worden afgeleid van bestaande gegevens of kunnen aanvullende gegevens te verzamelen nodig. Dit proces **engineering van de functie** wordt genoemd en is een van de belangrijkste stappen in het bouwen van een effectieve blog analysesysteem. Deze stap is vereist voor een creatieve combinatie van domein expertise en de inzichten die uit de stap gegevens verkennen opgehaald. Zie [technische het starten van Team gegevens wetenschappelijke aanbevelen](machine-learning-data-science-create-features.md)voor instructies.


## <a name="4-create-predictive-models"></a>4. blog modellen maken 

Gegevens wetenschappers maken analytical modellen voorspellen van de belangrijkste variabelen die wordt aangeduid met de zakelijke vereisten gedefinieerd in de planning stap gebruik van gegevens die is opgeschoond en featurized. Machine ondersteuning learning voor meerdere **modeling algoritmen** die wordt toegepast op een groot aantal zaken. Zie [het kiezen van algoritmen voor Team Azure Machine Learning](machine-learning-algorithm-choice.md)voor instructies.

Gegevens wetenschappers moeten kiezen voor het meest geschikte model voor hun taak tekstvoorspelling en het is niet ongebruikelijk dat resultaten van meerdere modellen worden gecombineerd moeten voor de beste resultaten. De invoergegevens voor modellering is meestal willekeurig onderverdeeld in drie delen:

- een gegevensset training 
- een gegevensgroep gegevensvalidatie 
- een gegevensgroep testen 

De modellen voor worden gemaakt met behulp van de **gegevensverzameling training**. De optimale combinatie van modellen (met parameters afgestemd) is ingeschakeld door de modellen gebruikt en de fouten tekstvoorspelling voor de **gegevensgroep validatie**meten. Ten slotte wordt het **testen van de gegevensverzameling** gebruikt om de prestaties van het door u gekozen model op onafhankelijke gegevens die niet is gebruikt om te trainen of het model valideren evalueren.  Zie [hoe u het model prestaties in Azure Machine Learning evalueren](machine-learning-evaluate-model-performance.md)voor procedures.


## <a name="5-deploy-and-consume-models"></a>5. implementeren en gebruiken van gegevensmodellen 

Als we een set modellen die ook uitvoert hebben, wordt dit steekt nogal **geoperationaliseerd** voor andere toepassingen gebruiken. Afhankelijk van de zakelijke vereisten, voorspellingen bestaan in **realtime** of op basis van de **batch** . Als u wilt worden geoperationaliseerd, de modellen voor moeten worden weergegeven met een **API-interface openen** die gemakkelijk vanuit verschillende toepassingen verbruikt is zoals online-website, -spreadsheets, dashboards of bedrijven en back-end-toepassingen. Zie [Deploy een Azure Machine Learning-webservice](machine-learning-publish-a-machine-learning-web-service.md).

## <a name="summary-and-next-steps"></a>Samenvatting en de volgende stappen

Het [Team gegevens wetenschap proces](https://azure.microsoft.com/documentation/learning-paths/cortana-analytics-process/) is gebaseerd op als een reeks stappen herhaald die **geven richtlijnen** voor de taken die nodig zijn voor het gebruik van geavanceerde analyses maken van een intelligente toepassingen. Elke stap bevat ook informatie over het gebruik van verschillende Microsoft-technologieën om de taken beschreven te voltooien. 

Tijdens het TDSP schrijft niet voor voor specifieke soorten **documentatie** onderdelen, dit is een goede gewoonte om de resultaten van de gegevens verkennen, modellering en evaluatie van een document en om op te slaan van de code die relevant zijn, zodat de analyse kunt herhaald als vereist. Hierdoor kunnen ook hergebruik van het werk analytics wanneer u werkt aan andere toepassingen die betrekking hebben op personen soortgelijke gegevens en tekstvoorspelling taken.

Volledige end-to-end-scenario's die de stappen in het proces voor **specifieke scenario's** demonstreren zijn ook beschikbaar. Zie, bijvoorbeeld:

- [Het proces van het Team gegevens wetenschappelijke in actie: SQL Server gebruiken](machine-learning-data-science-process-sql-walkthrough.md)
- [Het Team gegevens wetenschap proces in actie: HDInsight Hadoop clusters met](machine-learning-data-science-process-hive-walkthrough.md).
- [Gegevens wetenschappelijk met een op Azure HD.mdnsight](machine-learning-data-science-spark-overview.md)
- [Scalable gegevens wetenschappelijke in Azure gegevens Lake: een end-to-end-scenario](machine-learning-data-science-process-data-lake-walkthrough.md)

 
