
<!--
includes/sql-database-create-new-database-portal.md

Latest Freshness check:  2016-04-11 , carlrab.

As of circa 2016-04-11, the following topics might include this include:
articles/sql-database/sql-database-get-started-tutorial.md

-->
## <a name="create-a-new-azure-sql-database"></a>Een nieuwe Azure SQL-database maken

Gebruik de volgende stappen in de portal van Azure maken van een nieuwe Azure SQL-database op een nieuw of bestaand Azure SQL-Database logische server.

1. Als u momenteel niet verbonden bent, sluit u bij de [portal van Azure](http://portal.azure.com).
2. Klik op **Nieuw**, typ **SQL-Database**en klik vervolgens op **SQL-Database (nieuwe database)**.

     ![Nieuwe database](./media/sql-database-create-new-database-portal/sql-database-create-new-database-portal-1.png)

3. Klik op **SQL-Database (nieuwe database)**.

     ![Nieuwe database](./media/sql-database-create-new-database-portal/sql-database-create-new-database-portal-2.png)

4. Klik op **maken** om een nieuwe database maken in de SQL-Database-service.

     ![Nieuwe database](./media/sql-database-create-new-database-portal/sql-database-create-new-database-portal-3.png)

5. Geef de waarden voor de volgende servereigenschappen:

 - Databasenaam
 - Abonnement: Dit geldt alleen als u meerdere abonnementen hebt.
 - Resourcegroep: als u gaat voor het eerst, gebruikt u de resourcegroep van de logische server.
 - SELECT bron: U kunt een lege database, voorbeeldgegevens of een back-up van Azure-database. Zie de koppelingen aan het einde van dit artikel als u wilt migreren van een lokale SQL Server-database of gegevens met behulp van het hulpprogramma voor de BCP laadt.
 - Server: Een nieuw of bestaand logische server.
 - Aanmelden bij Server-beheerder
 - Wachtwoord
 - Prijzen laag: als u gaat voor het eerst, gebruikt u de standaardwaarde S0.
 - Sortering: Dit geldt alleen als een lege database is gekozen.

        ![New database](./media/sql-database-create-new-database-portal/sql-database-create-new-database-portal-4.png)

6.  Klik op **maken**. In het systeemvak ziet u dat de implementatie is gestart.

     ![Nieuwe database](./media/sql-database-create-new-database-portal/sql-database-create-new-database-portal-5.png)

7. Wacht totdat de implementatie om te voltooien voordat u verdergaat met de volgende stap.

     ![Nieuwe database](./media/sql-database-create-new-database-portal/sql-database-create-new-database-portal-6.png)
