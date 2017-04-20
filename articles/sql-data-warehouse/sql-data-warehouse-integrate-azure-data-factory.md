<properties
   pageTitle="Azure gegevens Factory gebruiken met SQL datawarehouse | Microsoft Azure"
   description="Tips voor het gebruik van Azure gegevens Factory (ADF) met Azure SQL Data Warehouse voor het ontwikkelen van oplossingen."
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
   ms.date="08/08/2016"
   ms.author="lodipalm;barbkess;sonyama"/>

# <a name="use-azure-data-factory-with-sql-data-warehouse"></a>Azure gegevens Factory gebruiken met SQL datawarehouse

Azure gegevens Factory biedt een volledig beheerde methode voor de overdracht van gegevens en uitvoeren van opgeslagen procedures op SQL Data Warehouse orchestrating.  Hierdoor kunt u eenvoudiger instellen en planning complexe extraheren transformeren en laden (ETL) procedures met SQL Data Warehouse. Zie de [documentatie Factory van Azure-gegevens][]voor een gedetailleerde overzicht van Azure gegevens Factory.

## <a name="data-movement"></a>Gegevens verplaatsen

Azure gegevens Factory kunt verplaatsing van gegevens tussen on-premises bronnen en de verschillende services van Azure.  Algehele, huidige integratie met Azure gegevens Factory ondersteunt gegevens verplaatst naar en van de volgende locaties:

+ Azure-blobopslag
+ Azure SQL-Database
+ On-premises SQL Server
+ SQL Server op IaaS

Voor meer informatie over het instellen van een gegevens kopiëren activiteit Zie [kopiëren van gegevens met Azure gegevens Factory][]

## <a name="stored-procedures"></a>Opgeslagen Procedures
 Op dezelfde manier die deze kan worden gebruikt om de overdracht van gegevens plannen, worden Azure gegevens Factory ook goedkeuringen door de uitvoering van opgeslagen procedures gebruikt.  Hiermee kunt complexere pijpleidingen moet worden gemaakt en breidt de mogelijkheden van Azure gegevens Factory mogelijkheid om te profiteren van de rekenkundige kracht van SQL Data Warehouse.

## <a name="next-steps"></a>Volgende stappen
Zie voor een overzicht van de integratie, [SQL Data Warehouse integratie overzicht][].
Zie voor meer tips voor de ontwikkeling, [SQL Data Warehouse ontwikkelen-overzicht][].

<!--Image references-->

<!--Article references-->

[Gegevens met Factory van Azure-gegevens kopiëren]: ../data-factory/data-factory-data-movement-activities.md
[SQL Data Warehouse ontwikkelen-overzicht]: ./sql-data-warehouse-overview-develop.md
[Overzicht van de SQL Data Warehouse-integratie]: ./sql-data-warehouse-overview-integrate.md

<!--MSDN references-->

<!--Other Web references-->
[Azure Data Factory-documentatie]:https://azure.microsoft.com/documentation/services/data-factory/

