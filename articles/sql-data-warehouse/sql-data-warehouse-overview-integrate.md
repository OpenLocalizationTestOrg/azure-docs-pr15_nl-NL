<properties
   pageTitle="Geïntegreerde oplossingen bouwen met SQL Data Warehouse | Microsoft Azure"
   description="Hulpprogramma's en partners met oplossingen die zijn geïntegreerd met SQL Data Warehouse. "
   services="sql-data-warehouse"
   documentationCenter="NA"
   authors="lodipalm"
   manager="barbkess"
   editor=""/>

<tags
   ms.service="sql-data-warehouse"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-services"
   ms.date="05/31/2016"
   ms.author="lodipalm;barbkess;sonyama"/>

#<a name="leverage-other-services-with-sql-data-warehouse"></a>Gebruikmaken van andere services met SQL Data Warehouse
Naast de belangrijkste functies kunnen SQL Data Warehouse gebruikers om te profiteren van veel van de andere services in Azure ernaast.  Specifiek, hebben we stappen nauw integreren met de volgende momenteel genomen:

+ Power BI
+ Azure gegevens Factory
+ Azure Machine Learning
+ Azure Stream Analytics

We werken als u wilt communiceren met meer services via het Azure-systeem.

##<a name="power-bi"></a>Power BI
Integratie van Power BI kunt u gebruikmaken van de rekenkracht van SQL Data Warehouse met de dynamische rapportage en visualisatie van Power BI. Integratie van Power BI bevat momenteel:

+ **Direct Connect**: een meer geavanceerde verbinding met het logische naar beneden duwen tegen SQL Data Warehouse.  Hierdoor sneller analyse op grotere schaal.
+ **Open in Power BI**: de knop 'Openen in Power BI' geeft exemplaar informatie aan Power BI, waardoor een naadloze verbinding.

Zie [integreren met Power BI](./sql-data-warehouse-integrate-power-bi.md) of de [Power BI-documentatie](http://blogs.msdn.com/b/powerbi/archive/2015/06/24/exploring-azure-sql-data-warehouse-with-power-bi.aspx) voor meer informatie.

##<a name="azure-data-factory"></a>Azure gegevens Factory
Azure gegevens Factory Geeft gebruikers een beheerde platform complexe uitpakken laden pijpleidingen maken.  SQL Data Warehouse-integratie met Azure gegevens Factory omvat het volgende:

+ **Opgeslagen Procedures**: de uitvoering van opgeslagen procedures op SQL Data Warehouse goedkeuringen.
+ **Kopie**: gebruik ADF om gegevens te verplaatsen naar SQL Data Warehouse.  Deze bewerking kunt van ADF standaardgegevens verkeer om of PolyBase achter de gebruiken. 

Zie [integreren met Azure gegevens Factory](./sql-data-warehouse-integrate-azure-data-factory.md) of de [Azure gegevens Factory-documentatie](https://azure.microsoft.com/documentation/services/data-factory/) voor meer informatie.

##<a name="azure-machine-learning"></a>Azure Machine Learning
Azure Machine Learning is een volledig beheerde analytics-service waarmee gebruikers kunnen complexe modellen die gebruikmaken van een groot aantal blog hulpprogramma's maken.  SQL Data Warehouse wordt ondersteund als zowel een bron- en doeltabellen voor deze modellen met de volgende functionaliteit:

+ **Lezen van gegevens:** Station modellen op schaal met behulp van de T-SQL tegen SQL Data Warehouse.
+ **Wegschrijven van gegevens:** Wijzigingen uit een model terug naar SQL Data Warehouse.

Zie [integreren met Azure Machine Learning](./sql-data-warehouse-integrate-azure-machine-learning.md) of de [Azure Machine Learning-documentatie](https://azure.microsoft.com/services/machine-learning/) voor meer informatie.

##<a name="azure-stream-analytics"></a>Azure Stream Analytics
Azure Stream Analytics is een complexe, volledig beheerde infrastructuur voor verbruikt gebeurtenisgegevens die zijn gegenereerd op basis van Azure gebeurtenis Hub en verwerkt.  Integratie met SQL Data Warehouse kan gegevensstromen effectief worden verwerkt en opgeslagen samen met relationele gegevens grondigere, meer geavanceerde analyse inschakelen.  

+ **Vervullen van uitvoer:** Uitvoer van Stream Analytics taken rechtstreeks naar SQL Data Warehouse verzenden.

Zie [integreren met Azure Stream Analytics](./sql-data-warehouse-integrate-azure-stream-analytics.md) of de [Azure Stream Analytics-documentatie](https://azure.microsoft.com/documentation/services/stream-analytics/) voor meer informatie.

<!--Image references-->

<!--Article references-->
[development overview]: sql-data-warehouse-overview-develop/

[Azure Data Factory]: sql-data-warehouse-integrate-azure-data-factory.md
[Azure Machine Learning]: sql-data-warehouse-integrate-azure-machine-learning.md
[Azure Stream Analytics]: sql-data-warehouse-integrate-azure-stream-analytics.md
[Power BI]: sql-data-warehouse-integrate-power-bi.md
[Partners]: sql-data-warehouse-partner-business-intelligence.md

<!--MSDN references-->

<!--Other Web references-->
