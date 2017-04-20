<properties
   pageTitle="Gegevens analyseren met Azure Machine Learning | Microsoft Azure"
   description="Gebruik Azure Machine Learning maken van een blog machine learning-model op basis van gegevens die zijn opgeslagen in Azure SQL Data Warehouse."
   services="sql-data-warehouse"
   documentationCenter="NA"
   authors="kevinvngo"
   manager="barbkess"
   editor=""/>

<tags
   ms.service="sql-data-warehouse"
   ms.devlang="NA"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-services"
   ms.date="09/14/2016"
   ms.author="kevin;barbkess;sonyama"/>

# <a name="analyze-data-with-azure-machine-learning"></a>Gegevens analyseren met Azure Machine Learning

> [AZURE.SELECTOR]
- [Power BI](sql-data-warehouse-get-started-visualize-with-power-bi.md)
- [Azure Machine Learning](sql-data-warehouse-get-started-analyze-with-azure-machine-learning.md)
- [Visual Studio](sql-data-warehouse-query-visual-studio.md)
- [Sqlcmd](sql-data-warehouse-get-started-connect-sqlcmd.md) 

Deze zelfstudie gebruikt Azure Machine Learning voor het maken van een blog machine learning-model op basis van gegevens die zijn opgeslagen in Azure SQL Data Warehouse. Specifiek, dit Hiermee wordt een gerichte marketingcampagne voor Adventure Works, de winkel fietsen op voorspellingen te doen als de klant waarschijnlijk het kopen van een fietsen al dan niet is.

> [AZURE.VIDEO integrating-azure-machine-learning-with-azure-sql-data-warehouse]


## <a name="prerequisites"></a>Vereisten voor
Als u wilt deze zelfstudie stapsgewijs, hebt u het volgende nodig:

- Een SQL-Data Warehouse AdventureWorksDW voorbeeldgegevens vooraf geïnstalleerd. Als u wilt inrichten dit, Zie [maken een SQL-Data Warehouse][] en kies de voorbeeldgegevens laden. Als u al een data-warehouse hebt, maar ik heb geen voorbeeldgegevens, kunt u [handmatig voorbeeldgegevens laden][].

## <a name="1-get-data"></a>1. gegevens ophalen
De gegevens zich in de weergave dbo.vTargetMail in de database AdventureWorksDW. Deze gegevens lezen:

1. Meld u aan bij [Azure Machine Learning studio][] en klik op mijn experimenten.
2. Klik op **+ Nieuw** en selecteer **Lege Experiment**.
3. Voer een naam voor uw experiment: Marketing gericht.
4. Sleep de module **Reader** vanuit het deelvenster modules in het canvas.
5. Geef de details van uw database SQL Data Warehouse in het deelvenster Eigenschappen.
6. Geef de database- **query** als u wilt de gegevens van belang lezen.

```sql
SELECT [CustomerKey]
  ,[GeographyKey]
  ,[CustomerAlternateKey]
  ,[MaritalStatus]
  ,[Gender]
  ,cast ([YearlyIncome] as int) as SalaryYear
  ,[TotalChildren]
  ,[NumberChildrenAtHome]
  ,[EnglishEducation]
  ,[EnglishOccupation]
  ,[HouseOwnerFlag]
  ,[NumberCarsOwned]
  ,[CommuteDistance]
  ,[Region]
  ,[Age]
  ,[BikeBuyer]
FROM [dbo].[vTargetMail]
```

Voer het experiment door te klikken op **uitvoeren** onder het tekenpapier experiment.
![Het experiment uitvoeren][1]


Nadat het experiment is voltooid kon worden voltooid, klikt u op de uitvoerpoort onderaan in de module Reader en selecteer **visualiseren** om de geïmporteerde gegevens weer te geven.
![Geïmporteerde gegevens weergeven][3]


## <a name="2-clean-the-data"></a>2. de gegevens wissen
Als u wilt wissen die de gegevens, zet u enkele kolommen die niet relevant voor het model zijn. Dit wilt doen:

1. Sleep de module **Project kolommen** in het canvas.
2. Klik op **starten kolom Gegevenskiezer** in het deelvenster Eigenschappen om op te geven welke kolommen u wilt neerzetten.
![Project-kolommen][4]

3. Twee kolommen uitsluiten: CustomerAlternateKey en Geografiesleutel.
![Onnodige kolommen verwijderen][5]


## <a name="3-build-the-model"></a>3. het model maken
We de gegevens 80-20 worden gesplitst: 80% om te trainen een machine learning-model en 20% om te testen van het model. We maakt gebruik van de "Twee-Class" algoritmen voor dit probleem binaire indeling.

1. Sleep de module **splitsen** in het canvas.
2. Voer 0,8 voor deel van de rijen in de eerste uitvoer gegevensset in het deelvenster Eigenschappen.
![Gegevens splitsen in training en test instellen][6]
3. Sleep de module die **Twee-klasse verhoogd beslissingsstructuur** in het canvas.
4. Sleep de module **Trein Model** naar het tekenpapier en geef de invoer. Klik vervolgens op **kolom Gegevenskiezer starten** in het deelvenster Eigenschappen.
      - Eerst input: ML algoritme.
      - Tweede input: gegevens aan de algoritme van de trainen op.
![Verbinding maken met de module trein Model][7]
5. Selecteer de kolom **BikeBuyer** als de kolom te voorspellen.
![Selecteer de kolom te voorspellen][8]


## <a name="4-score-the-model"></a>4. score het model
We wordt nu testen hoe het model uitvoeren met testgegevens. We worden de algoritme van onze keuze met een ander algoritme om te zien uitvoert, wordt er beter vergeleken.

1. Sleep **Score Model** module in het canvas.
    Eerst input: model tweede invoer training: gegevens testen ![Score het model][9]
2. De **Twee klasse Bayes punt Machine** naar het tekenpapier experiment slepen. We wordt vergelijken hoe dit algoritme ten opzichte van de op twee-klasse verhoogd beslissingsstructuur uitvoert.
3. Kopieer en plak de modules trein Model en Score Model in het canvas.
4. Sleep de module **Model evalueren** in het canvas om te vergelijken van de twee algoritmen.
5. **Voer** het experiment.
![Het experiment uitvoeren][10]
6. De uitvoerpoort onderaan in de module evalueren Model en klik op visualiseren.
![Evaluatieresultaten visualiseren][11]

De opgegeven criteria zijn de ROC curve, precisie-intrekken diagram en lift curve. Deze gegevens bekijkt, ziet dat het eerste model beter dan de tweede tabel worden uitgevoerd. Nagaan wat is het eerste model voorspeld, klik op uitvoerpoort van het Model Score, en klik op visualiseren.
![Score resultaten visualiseren][12]

Hier ziet u twee of meer kolommen toegevoegd aan uw gegevensset test.

- Een score voorzien kansen: de kans dat een klant een looptijd fietsen is.
- Labels van een score voorzien: de classificatie uitgevoerd door het model, fietsen koper (1), al dan niet (0). Deze kans drempel voor labeling tot 50% is ingesteld en kan worden aangepast.

Vergelijking van de kolom BikeBuyer (werkelijk) met de Labels van een score voorzien (voorspellen), kunt u zien hoe u ook het model heeft uitgevoerd. Als de volgende stappen, kunt u dit model voorspellingen voor nieuwe klanten en publiceren van dit model als een webservice of resultaten terugschrijven naar SQL Data Warehouse.

## <a name="next-steps"></a>Volgende stappen

Meer informatie over het samenstellen van blog machine learning-modellen, raadpleegt u [Inleiding tot Machine Learning op Azure][].

<!--Image references-->
[1]: media/sql-data-warehouse-get-started-analyze-with-azure-machine-learning/img1_reader.png
[2]: media/sql-data-warehouse-get-started-analyze-with-azure-machine-learning/img2_visualize.png
[3]: media/sql-data-warehouse-get-started-analyze-with-azure-machine-learning/img3_readerdata.png
[4]: media/sql-data-warehouse-get-started-analyze-with-azure-machine-learning/img4_projectcolumns.png
[5]: media/sql-data-warehouse-get-started-analyze-with-azure-machine-learning/img5_columnselector.png
[6]: media/sql-data-warehouse-get-started-analyze-with-azure-machine-learning/img6_split.png
[7]: media/sql-data-warehouse-get-started-analyze-with-azure-machine-learning/img7_train.png
[8]: media/sql-data-warehouse-get-started-analyze-with-azure-machine-learning/img8_traincolumnselector.png
[9]: media/sql-data-warehouse-get-started-analyze-with-azure-machine-learning/img9_score.png
[10]: media/sql-data-warehouse-get-started-analyze-with-azure-machine-learning/img10_evaluate.png
[11]: media/sql-data-warehouse-get-started-analyze-with-azure-machine-learning/img11_evalresults.png
[12]: media/sql-data-warehouse-get-started-analyze-with-azure-machine-learning/img12_scoreresults.png


<!--Article references-->
[Azure Machine Learning-studio]:https://studio.azureml.net/
[Inleiding tot Machine Learning op Azure]:https://azure.microsoft.com/documentation/articles/machine-learning-what-is-machine-learning/
[voorbeeldgegevens handmatig laden]: sql-data-warehouse-load-sample-databases.md
[Een SQL-datawarehouse maken]: sql-data-warehouse-get-started-provision.md
