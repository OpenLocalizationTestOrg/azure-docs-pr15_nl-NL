<properties
   pageTitle="Azure Machine Learning gebruiken met SQL datawarehouse | Microsoft Azure"
   description="Zelfstudie voor het gebruik van Azure Machine Learning met Azure SQL Data Warehouse voor het ontwikkelen van oplossingen."
   services="sql-data-warehouse"
   documentationCenter="NA"
   authors="kevinvngo"
   manager="barbkess"
   editor=""/>

<tags
   ms.service="sql-data-warehouse"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-services"
   ms.date="08/16/2016"
   ms.author="kevin;barbkess;sonyama"/>

# <a name="use-azure-machine-learning-with-sql-data-warehouse"></a>Azure Machine Learning gebruiken met SQL datawarehouse

Azure Machine Learning is een volledig beheerde blog analytics-service die u kunt gebruiken om te maken van blog modellen ten opzichte van uw gegevens in SQL Data Warehouse en vervolgens publiceren als bij de hand-naar-gebruiken webservices. U kunt informatie over de basisbeginselen van het blog analyses en machine learning door te lezen [Inleiding tot Machine Learning op Azure][].  Vervolgens kunt u lezen hoe maken, trainen, score en testen van een machine learning-model met de [zelfstudie experimenteren maken][].

In dit artikel leert u hoe u kunt als volgt met behulp van de [Azure Machine Learning Studio][]:

- Gegevens lezen in uw database te maken, trainen en een blog model score
- Gegevens naar uw database schrijven


## <a name="read-data-from-sql-data-warehouse"></a>Gegevens uit SQL Data Warehouse lezen

We worden gegevens van de tabel Product in de database AdventureWorksDW lezen.

### <a name="step-1"></a>Stap 1

Een nieuwe experiment starten door te klikken op + Nieuw onderaan in het venster Machine Learning Studio EXPERIMENT selecteren en selecteer vervolgens lege experimenteren. Selecteer de naam van de standaard experiment boven aan het tekenpapier en wijzig de naam herkenbare, bijvoorbeeld fiets prijs tekstvoorspelling.

### <a name="step-2"></a>Stap 2

Zoek naar de lezer module in het palet van gegevenssets en modules aan de linkerkant van het tekenpapier experiment. Sleep de module naar het tekenpapier experiment.
![][drag_reader]

### <a name="step-3"></a>Stap 3

Selecteer de module Reader en vul het deelvenster Eigenschappen.

1. Selecteer Azure SQL-Database als de gegevensbron.
2. Naam van de database-server: Typ de naam van de server. U kunt de [Azure-portal][] gebruiken om dit te vinden.

![][server_name]

3. Databasenaam: Typ de naam van een database op de server die u zojuist hebt opgegeven.
4. Server-account gebruikersnaam: Typ de naam van een account met machtigingen voor de database.
5. Wachtwoord voor een gebruikersaccount server: het wachtwoord opgeven voor de opgegeven gebruikersaccount.
6. Een servercertificaat geaccepteerd: Gebruik deze optie (minder veilig) als u overslaan reviseren van het site-certificaat wilt voordat u uw gegevens lezen.
7. Databasequery's: Voer een SQL-instructie waarmee de gegevens die u wilt lezen wordt beschreven. In dit geval leest we gegevens uit de tabel Product met behulp van de volgende query.


```SQL
SELECT ProductKey, EnglishProductName, StandardCost,
        ListPrice, Size, Weight, DaysToManufacture,
        Class, Style, Color
FROM dbo.DimProduct;
```

![][reader_properties]

### <a name="step-4"></a>Stap 4

1. Voer het experiment door te klikken op uitvoeren onder het tekenpapier experiment.
2. Wanneer het experiment klaar is, heeft de lezer-module een groen vinkje om aan te geven dat deze is voltooid. Zoals u ziet ook de status uitgevoerd in de rechterbovenhoek voltooid.

![][run]

3. Als u wilt de geïmporteerde gegevens wordt weergegeven, klikt u op de uitvoerpoort onderaan in de auto gegevensset en selecteer visualiseren.


## <a name="create-train-and-score-a-model"></a>Maken, trainen en een model score

Nu kunt u deze dataset naar:

- Maken van een Model: gegevens en functies te definiëren
- Het model trainen: kiezen en een algoritme learning toepassen
- Score en testen van het model: nieuwe fiets prijs voorspellen


![][model]

Meer informatie over het maken, training, score en test u een machine learning-model gebruiken de [maken experimenteren zelfstudie][].

## <a name="write-data-to-azure-sql-data-warehouse"></a>Gegevens naar Azure SQL Data Warehouse schrijven

We worden de resultatenset aan ProductPriceForecast tabel in de database AdventureWorksDW geschreven.

### <a name="step-1"></a>Stap 1

Zoek naar de schrijver module in het palet van gegevenssets en modules aan de linkerkant van het tekenpapier experiment. Sleep de module naar het tekenpapier experiment.

![][drag_writer]

### <a name="step-2"></a>Stap 2

Selecteer de module schrijver en vul het deelvenster Eigenschappen.

1. Selecteer Azure SQL-Database als de gegevensbestemming van de.
2. Naam van de database-server: Typ de naam van de server. U kunt de [Azure-portal][] gebruiken om dit te vinden.
3. Databasenaam: Typ de naam van een database op de server die u zojuist hebt opgegeven.
4. Server-account gebruikersnaam: Typ de naam van een account met schrijfmachtigingen voor de database.
5. Wachtwoord voor een gebruikersaccount server: het wachtwoord opgeven voor de opgegeven gebruikersaccount.
6. Een servercertificaat (onveilige) geaccepteerd: Selecteer deze optie als u niet wilt weergeven van het certificaat.
7. Door komma's gescheiden lijst met kolommen wilt opslaan: een lijst van de gegevensset of resultaat kolommen die u wilt uitvoeren.
8. Tabelnaam van gegevens: de naam van de gegevenstabel opgeven.
9. Door komma's gescheiden lijst met gegevenstabel kolommen: Geef de kolomnamen in de nieuwe tabel gebruiken. De kolomnamen kunnen afwijken van de kleuren die in de gegevensset bron, maar u moet hetzelfde aantal kolommen hier die u voor de uitvoertabel definieert worden vermeld.
10. Aantal rijen geschreven per SQL Azure-bewerking: kunt u het aantal rijen die naar een SQL-database in één bewerking worden geschreven.

![][writer_properties]

### <a name="step-3"></a>Stap 3

1. Voer het experiment door te klikken op uitvoeren onder het tekenpapier experiment.
2. Wanneer het experiment is voltooid, worden alle modules een groen vinkje om aan te geven dat ze voltooid hebben.

## <a name="next-steps"></a>Volgende stappen

Zie voor meer tips voor de ontwikkeling, [SQL Data Warehouse ontwikkelen-overzicht][].

<!--Image references-->

[drag_reader]: ./media/sql-data-warehouse-integrate-azure-machine-learning/ml-drag-reader.png
[server_name]: ./media/sql-data-warehouse-integrate-azure-machine-learning/dw-server-name.png
[reader_properties]: ./media/sql-data-warehouse-integrate-azure-machine-learning/ml-reader-properties.png
[run]: ./media/sql-data-warehouse-integrate-azure-machine-learning/ml-finished-running.png
[model]: ./media/sql-data-warehouse-integrate-azure-machine-learning/ml-create-train-score-model.png
[drag_writer]: ./media/sql-data-warehouse-integrate-azure-machine-learning/ml-drag-writer.png
[writer_properties]: ./media/sql-data-warehouse-integrate-azure-machine-learning/ml-writer-properties.png

<!--Article references-->

[SQL Data Warehouse ontwikkelen-overzicht]: ./sql-data-warehouse-overview-develop.md
[Experiment zelfstudie maken]: https://azure.microsoft.com/documentation/articles/machine-learning-create-experiment/
[Inleiding tot machine learning op Azure]: https://azure.microsoft.com/documentation/articles/machine-learning-what-is-machine-learning/
[Azure Machine Learning Studio]: https://studio.azureml.net/Home
[Azure-portal]: https://portal.azure.com/

<!--MSDN references-->

<!--Other Web references-->

[Azure Machine Learning documentation]: http://azure.microsoft.com/documentation/services/machine-learning/
