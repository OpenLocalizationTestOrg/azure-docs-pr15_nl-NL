<properties
   pageTitle="Azure Stream Analytics gebruiken met SQL datawarehouse | Microsoft Azure"
   description="Tips voor het gebruik van Azure Stream analyses met Azure SQL Data Warehouse voor het ontwikkelen van oplossingen."
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

# <a name="use-azure-stream-analytics-with-sql-data-warehouse"></a>Azure Stream Analytics gebruiken met SQL datawarehouse

Azure Stream Analytics is een volledig beheerde service lage latentie, ten zeerste beschikbaar, scalable complexe gebeurtenis verwerking leveren via streaming van gegevens in de cloud. U kunt de basisbegrippen door te lezen [Inleiding tot Azure Stream Analytics][]. U kunt vervolgens informatie over het maken van een end-to-end-oplossing met Stream Analytics volgens de zelfstudie [aan de slag met Azure Stream Analytics][] .

In dit artikel leert u hoe u uw database Azure SQL Data Warehouse gebruikt als een sink uitvoer voor uw taken stoom Analytics.

## <a name="prerequisites"></a>Vereisten voor

Eerst worden uitgevoerd door de volgende stappen in de zelfstudie [aan de slag met Azure Stream Analytics][] .  

1. De invoer van een gebeurtenis Hub maken
2. Configureren en gebeurtenis genereren-toepassing start
3. Een taak Stream Analytics inrichten
4. Geef de invoer van de taak en query

Maak een Data Warehouse van Azure SQL-database

## <a name="specify-job-output-azure-sql-data-warehouse-database"></a>Taak-uitvoer opgeven: Azure Data Warehouse van de SQL-database

### <a name="step-1"></a>Stap 1

In uw taak Stream Analytics **uitvoer** vanaf de bovenkant van de pagina op en klik vervolgens op **Uitvoer toevoegen**.

### <a name="step-2"></a>Stap 2

Selecteer SQL-Database en klik op volgende.

![][add-output]

### <a name="step-3"></a>Stap 3
Voer de volgende waarden op de volgende pagina:

- *Uitvoeralias*: Voer een beschrijvende naam voor de uitvoer van deze taak.
- *Abonnement*:
    - Als uw SQL Data Warehouse-database hetzelfde als de taak Stream Analytics-abonnement is, selecteert u gebruik SQL-Database in de huidige abonnement.
    - Als de database zich in een ander abonnement, selecteert u gebruik SQL-Database uit een ander abonnement.
- *Database*: Geef de naam van een doeldatabase.
- *Servernaam*: Geef de naam van de server voor de database die u zojuist hebt opgegeven. U kunt de klassieke Azure-Portal gebruiken om dit te vinden.

![][server-name]

- *Gebruikersnaam in te voeren*: de gebruikersnaam van een account met schrijfmachtigingen voor de database opgeven.
- *Wachtwoord*: het wachtwoord opgeven voor de opgegeven gebruikersaccount.
- *Tabel*: de naam van de doeltabel in de database opgeven.

![][add-database]

### <a name="step-4"></a>Stap 4

Klik op de knop controleren om toe te voegen van de uitvoer van deze taak en om te bevestigen dat Stream Analytics succes verbinding met de database maken kunt.

![][test-connection]

Wanneer de verbinding met de database is gemaakt, ziet u een melding onderaan in de portal. U kunt klikken op verbinding testen onderaan om de verbinding met de database te testen.

## <a name="next-steps"></a>Volgende stappen

Zie voor een overzicht van de integratie, [SQL Data Warehouse integratie overzicht][].

Zie voor meer tips voor de ontwikkeling, [SQL Data Warehouse ontwikkelen-overzicht][].

<!--Image references-->

[add-output]: ./media/sql-data-warehouse-integrate-azure-stream-analytics/add-output.png
[server-name]: ./media/sql-data-warehouse-integrate-azure-stream-analytics/dw-server-name.png
[add-database]: ./media/sql-data-warehouse-integrate-azure-stream-analytics/add-database.png
[test-connection]: ./media/sql-data-warehouse-integrate-azure-stream-analytics/test-connection.png

<!--Article references-->

[Inleiding tot Azure Stream Analytics]: ../stream-analytics/stream-analytics-introduction.md
[Aan de slag met Azure Stream Analytics]: ../stream-analytics/stream-analytics-get-started.md
[SQL Data Warehouse ontwikkelen-overzicht]:  ./sql-data-warehouse-overview-develop.md
[Overzicht van de SQL Data Warehouse-integratie]:  ./sql-data-warehouse-overview-integrate.md

<!--MSDN references-->

<!--Other Web references-->
[Azure Stream Analytics documentation]: http://azure.microsoft.com/documentation/services/stream-analytics/
