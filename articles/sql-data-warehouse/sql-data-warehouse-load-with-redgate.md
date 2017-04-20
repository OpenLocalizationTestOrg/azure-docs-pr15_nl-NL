<properties
   pageTitle="Redgate van gegevens Platform Studio gebruiken om gegevens te laden in SQL Data Warehouse | Microsoft Azure"
   description="Informatie over het gebruik van de Redgate gegevens Platform Studio voor gegevens magazijnscenario's."
   services="sql-data-warehouse"
   documentationCenter="NA"
   authors="twounder"
   manager="barbkess"
   editor=""/>

<tags
   ms.service="sql-data-warehouse"
   ms.devlang="NA"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-services"
   ms.date="10/13/2016"
   ms.author="mausher;barbkess"/>


# <a name="load-data-with-redgate-data-platform-studio"></a>Gegevens met Redgate gegevens Platform Studio laden

> [AZURE.SELECTOR]
- [Redgate](sql-data-warehouse-load-with-redgate.md)
- [Gegevens Factory](sql-data-warehouse-get-started-load-with-azure-data-factory.md)
- [PolyBase](sql-data-warehouse-get-started-load-with-polybase.md)
- [BCP](sql-data-warehouse-load-with-bcp.md)

Deze zelfstudie ziet u hoe u [De Redgate gegevens Platform Studio](http://www.red-gate.com/products/azure-development/data-platform-studio/) (DPS) gebruiken om gegevens te verplaatsen van een on-premises SQL-Server naar Azure SQL Data Warehouse. Gegevens Platform Studio geldt de meest geschikte compatibiliteit reparaties en optimalisaties toe, zodat u de snelste manier om aan de slag met SQL Data Warehouse.

> [AZURE.NOTE] [Redgate](http://www.red-gate.com) is een ervaren Microsoft-partner die diverse hulpprogramma's voor SQL Server biedt. Deze functie in gegevens Platform Studio is beschikbaar gesteld vrij voor commerciële en niet-commercieel gebruik.

## <a name="before-you-begin"></a>Voordat u begint
### <a name="create-or-identify-resources"></a>Maak of resources identificeren

Voordat u deze zelfstudie begint, moet u beschikken over:

- **On-premises SQL Server-Database**: de gegevens die u wilt importeren naar SQL Data Warehouse moeten afkomstig zijn uit een on-premises SQL Server (versie 2008R2 of hoger). Gegevens Platform Studio kan gegevens importeren, rechtstreeks vanuit een Azure SQL-Database of tekstbestanden.
- **Azure opslag-Account**: gegevens Platform Studio fasen de gegevens in Azure-blobopslag voordat het laden in het SQL Data Warehouse. Het account opslag moet de implementatiemodel ' resourcemanager ' (de standaardinstelling) gebruiken in plaats van de 'Klassieke' implementatie-model. Als u niet een opslag-account hebt, informatie over het maken van een opslag-account. 
- **SQL Data Warehouse**: deze zelfstudie drukt, verplaatst de gegevens van on-premises SQL Server naar SQL Data Warehouse, zodat er moet een data-warehouse online. Als u nog een data-warehouse, informatie over het maken van een Azure SQL Data Warehouse.

> [AZURE.NOTE] Prestaties zijn verbeterd als de opslagruimte-account en datawarehouse zijn gemaakt in dezelfde regio.

## <a name="step-1-sign-in-to-data-platform-studio-with-your-azure-account"></a>Stap 1: Aanmelden bij gegevens Platform Studio met uw Azure-account
Open de webbrowser en navigeer naar de website van de [Gegevens Platform Studio](https://www.dataplatformstudio.com/) . Zich aanmelden met hetzelfde Azure-account waarmee u het account en gegevens van een opslagplaats gemaakt. Als uw e-mailadres gekoppeld aan een werk- of schoolaccount en een Microsoft-account is, moet u het account dat toegang tot uw resources heeft kiezen.

> [AZURE.NOTE] Als dit de eerste keer is het gebruik van gegevens Platform Studio, wordt u gevraagd de toepassing machtigen voor het beheren van uw Azure resources.

## <a name="step-2-start-the-import-wizard"></a>Stap 2: De Wizard importeren starten
Selecteer in het hoofdscherm van DPS het importeren naar Azure SQL Data Warehouse koppeling om de wizard importeren te starten.

![][1]

## <a name="step-3-install-the-data-platform-studio-gateway"></a>Stap 3: De gegevens Platform Studio Gateway installeren
Als u wilt verbinden met uw on-premises SQL Server-database, moet u de Gateway DPS installeren. De gateway is een client-agent die biedt toegang tot uw on-premises omgeving en geüpload naar uw account opslag haalt de gegevens. Uw gegevens passeert nooit van Redgate-servers. De Gateway installeren:

1.  Klik op de koppeling **Gateway maken**
2. Download en installeer de Gateway met het installatieprogramma van de meegeleverde

![][2]

> [AZURE.NOTE] De Gateway kan worden geïnstalleerd op een computer met het netwerktoegang tot de gegevensbron SQL Server-database. Deze gebruikmaakt van de SQL Server-database met Windows-verificatie met de referenties van de huidige gebruiker.

Zodra geïnstalleerd, de Gateway-status wordt gewijzigd in verbonden en u kunt op volgende.

## <a name="step-4-identify-the-source-database"></a>Stap 4: De brondatabase identificeren
Voer in het tekstvak *Servernaam Typ* de naam van de server waarop uw database zich bevindt en selecteer **volgende**. Selecteer vervolgens de database die u wilt importeren van gegevens uit in de vervolgkeuzelijst.

![][3]

DPS Hiermee wordt gecontroleerd met de geselecteerde database voor tabellen te importeren. Standaard wordt met DPS alle tabellen in de database geïmporteerd. Selecteer of deselecteer van tabellen door de koppeling alle tabellen uitvouwen. Selecteer de knop Volgende om vooruit te gaan.

## <a name="step-5-choose-a-storage-account-to-stage-the-data"></a>Stap 5: Kies een opslag-account om de gegevens van fase
DPS vraagt u naar een locatie voor de gegevens van fase. Kies een bestaand account voor de opslag van uw abonnement en selecteer **volgende**.

> [AZURE.NOTE] DPS maakt u een nieuwe blob container in de door u gekozen opslag-account en een afzonderlijke map gebruiken voor elke importeren.

![][4]

## <a name="step-6-select-a-data-warehouse"></a>Stap 6: Selecteer een data-warehouse
Selecteer vervolgens een online [Data Warehouse van Azure SQL](http://aka.ms/sqldw) -database om de gegevens in te importeren. Als u uw database hebt geselecteerd, moet u Voer de referenties verbinding maken met de database en klik op **volgende**.

![][5]

> [AZURE.NOTE] DPS samengevoegd de bron-gegevenstabellen met DataWarehouse. DPS wordt u gewaarschuwd als de naam van de tabel is vereist om te overschrijven bestaande tabellen in het datawarehouse. U kunt desgewenst verwijderen eventuele bestaande objecten in het datawarehouse door te tikken verwijderen alle bestaande objecten vóór het importeren.

## <a name="step-7-import-the-data"></a>Stap 7: De gegevens importeren
DPS bevestigt dat u wilt de gegevens importeren. Klik op de knop Start importeren om te beginnen met het importeren van gegevens.

![][6]

Een visualisatie die de voortgang wordt van extraheren en het uploaden van de gegevens uit de on-premises SQL Server en de voortgang van de invoer in SQL Data Warehouse weergeven DPS

![][7]

Nadat de import voltooid is, wordt DPS toont een samenvatting van het importeren van gegevens en een rapport wijzigen van de compatibiliteit-oplossingen die zijn uitgevoerd.

![][8]

## <a name="next-steps"></a>Volgende stappen
Als u wilt uw gegevens binnen SQL Data Warehouse verkennen, begint u met bekijken:

- [Query Azure SQL datawarehouse (Visual Studio)][]
- [Gegevens met Power BI visualiseren][]

Voor meer informatie over de Redgate gegevens Platform Studio:

- [Ga naar de startpagina DPS](http://www.dataplatformstudio.com/)
- [Bekijk een demo van DPS op Channel9](https://channel9.msdn.com/Blogs/cloud-with-a-silver-lining/Loading-data-into-Azure-SQL-Datawarehouse-with-Redgate-Data-Platform-Studio)

Zie voor een overzicht van andere manieren om te migreren en uw gegevens in SQL Data Warehouse laden:

- [Uw oplossing migreren naar SQL Data Warehouse][]
- [Gegevens laadt in Azure SQL Data Warehouse](./sql-data-warehouse-overview-load.md)

Zie voor meer tips voor de ontwikkeling, [SQL Data Warehouse ontwikkelen-overzicht](./sql-data-warehouse-overview-develop.md).

<!--Image references-->
[1]: media/sql-data-warehouse-redgate/2016-10-05_15-59-56.png
[2]: media/sql-data-warehouse-redgate/2016-10-05_11-16-07.png
[3]: media/sql-data-warehouse-redgate/2016-10-05_11-17-46.png
[4]: media/sql-data-warehouse-redgate/2016-10-05_11-20-41.png
[5]: media/sql-data-warehouse-redgate/2016-10-05_11-31-24.png
[6]: media/sql-data-warehouse-redgate/2016-10-05_11-32-20.png
[7]: media/sql-data-warehouse-redgate/2016-10-05_11-49-53.png
[8]: media/sql-data-warehouse-redgate/2016-10-05_12-57-10.png

<!--Article references-->
[Query Azure SQL datawarehouse (Visual Studio)]: ./sql-data-warehouse-query-visual-studio.md
[Gegevens met Power BI visualiseren]: ./sql-data-warehouse-get-started-visualize-with-power-bi.md
[Uw oplossing migreren naar SQL Data Warehouse]: ./sql-data-warehouse-overview-migrate.md
[Load data into Azure SQL Data Warehouse]: ./sql-data-warehouse-overview-load.md
[SQL Data Warehouse development overview]: ./sql-data-warehouse-overview-develop.md