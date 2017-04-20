<properties
    pageTitle="SQL-Database zelfstudie: een SQL-database maken | Microsoft Azure"
    description="Informatie over het instellen van een logische SQL Database-server, server firewallregel, SQL-database en voorbeeldgegevens. Ook informatie over het verbinding maken met clienthulpprogramma's, gebruikers configureren en een regel van de firewall database instellen."
    keywords="SQL-database zelfstudie een sql-database maken"
    services="sql-database"
    documentationCenter=""
    authors="CarlRabeler"
    manager="jhubbard"
    editor=""/>


<tags
    ms.service="sql-database"
    ms.workload="data-management"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="hero-article"
    ms.date="09/07/2016"
    ms.author="carlrab"/>


# <a name="sql-database-tutorial-create-a-sql-database-in-minutes-by-using-the-azure-portal"></a>SQL-Database zelfstudie: een SQL-database maken in minuten met behulp van de Azure-portal

> [AZURE.SELECTOR]
- [Azure-portal](sql-database-get-started.md)
- [C#](sql-database-get-started-csharp.md)
- [PowerShell](sql-database-get-started-powershell.md)

In deze zelfstudie leert u hoe de Azure portal om te gebruiken:

- Maak een Azure SQL-database met voorbeeldgegevens.
- Maak een firewallregel serverniveau voor één IP-adres of voor een bereik van IP-adressen.

U kunt deze dezelfde taken uitvoeren via [C#](sql-database-get-started-csharp.md) of via [PowerShell](sql-database-get-started-powershell.md).

[AZURE.INCLUDE [Login](../../includes/azure-getting-started-portal-login.md)]

<a name="create-logical-server-bk"></a>

## <a name="create-your-first-azure-sql-database"></a>Uw eerste Azure SQL-database maken 

1. Als u momenteel niet verbonden bent, sluit u bij de [portal van Azure](http://portal.azure.com).
2. Klik op **Nieuw**, klikt u op **gegevens + opslagruimte**en zoek **SQL-Database**.

    ![Nieuwe sql-database 1](./media/sql-database-get-started/sql-database-new-database-1.png)

3. Klik op **SQL-Database** om te openen van het blad SQL-Database. De inhoud op deze blade varieert afhankelijk van het nummer van uw abonnementen en uw bestaande objecten (zoals bestaande servers).

    ![Nieuwe sql-database 2](./media/sql-database-get-started/sql-database-new-database-2.png)

4. Geef een naam voor de eerste database - zoals 'mijn-database' in het tekstvak **naam van de Database** . Een groen vinkje geeft aan dat u de naam van een geldige hebt opgegeven.

    ![Nieuwe sql-database 3](./media/sql-database-get-started/sql-database-new-database-3.png)

5. Als u meerdere abonnementen hebt, selecteert u een abonnement.
6. Klik onder **resourcegroep**, klik op **Nieuw** en geef een naam voor uw eerste resourcegroep - zoals "Mijn--resourcegroep". Een groen vinkje geeft aan dat u de naam van een geldige hebt opgegeven.

    ![Nieuwe sql-database 4](./media/sql-database-get-started/sql-database-new-database-4.png)

7. Klik onder **Selecteer bron**, klik op **voorbeeld** en klik onder **Selecteer voorbeeld** op **AdventureWorksLT [V12]**.

    ![Nieuwe sql-database 5](./media/sql-database-get-started/sql-database-new-database-5.png)

8. Klik op **configureren vereist instellingen**onder **Server**.

    ![Nieuwe sql-database 6](./media/sql-database-get-started/sql-database-new-database-6.png)

9. Klik op het blad Server op **een nieuwe server maken**. Een Azure SQL-database is gemaakt in een serverobject, die een nieuwe server of een bestaande server kan zijn.

    ![Nieuwe sql-database 7](./media/sql-database-get-started/sql-database-new-database-7.png)

10. Bekijk het blad **nieuwe server** als u wilt weten welke gegevens die u moet bieden voor uw nieuwe server.

    ![Nieuwe sql-database 8](./media/sql-database-get-started/sql-database-new-database-8.png)

11. Geef een naam voor uw eerste server - zoals 'Mijn nieuwe-server-object' in het tekstvak **de naam van de Server** . Een groen vinkje geeft aan dat u de naam van een geldige hebt opgegeven.

    ![Nieuwe sql-database 9](./media/sql-database-get-started/sql-database-new-database-9.png)
 
12. Geef onder **Server admin aanmelden**, een gebruikersnaam in te voeren voor de beheerder-aanmelding voor deze server - zoals "Mijn--beheerdersaccount". Deze aanmelding is bekend als de server principal login. Een groen vinkje geeft aan dat u de naam van een geldige hebt opgegeven.

    ![Nieuwe sql-database 10](./media/sql-database-get-started/sql-database-new-database-10.png)

13. Klik onder **wachtwoord** en **Bevestig het wachtwoord**een wachtwoord opgeven voor de server principal aanmeldingsaccount - zoals "p@ssw0rd1". Een groen vinkje geeft aan dat u een geldig wachtwoord hebt opgegeven.

    ![Nieuwe sql-database 11](./media/sql-database-get-started/sql-database-new-database-11.png)
 
14. Schakel in het vak **locatie**, een geschikt zijn voor uw locatie - zoals "Australië Oost" Datacenter.

    ![Nieuwe sql-database 12](./media/sql-database-get-started/sql-database-new-database-12.png)

15. Klik onder ** V12 maken server (meest recente update), zoals u ziet dat u alleen de optie voor het maken van een huidige versie van Azure SQL-server hebt.

    ![Nieuwe sql-database 13](./media/sql-database-get-started/sql-database-new-database-13.png)

16. Zoals u ziet dat het selectievakje voor **toestaan azure-services voor toegang tot server** standaard is geselecteerd en hier niet worden gewijzigd. Dit is een geavanceerde optie. U kunt deze instelling wijzigen in de firewall serverinstellingen voor deze serverobject, hoewel voor de meeste scenario's dit niet nodig is.

    ![Nieuwe sql-database 14](./media/sql-database-get-started/sql-database-new-database-14.png)

17. Klik op het nieuwe blad van de server, Controleer uw selecties en klik vervolgens op **selecteren** om deze nieuwe server voor uw nieuwe database te selecteren.

    ![Nieuwe sql-database 15](./media/sql-database-get-started/sql-database-new-database-15.png)

18. Klik op het blad SQL-Database, klikt u onder **prijzen laag**, klikt u op **Standaard S2** en klik vervolgens op **eenvoudige** om de minst dure prijzen laag voor de eerste database te selecteren. U kunt de prijzen laag later altijd wijzigen.

    ![Nieuwe sql-database 16](./media/sql-database-get-started/sql-database-new-database-16.png)

19. Controleer uw selecties op het blad SQL-Database en klik vervolgens op **maken** om uw eerste server en database maken. De waarden die u hebt opgegeven worden gevalideerd en implementatie wordt gestart.

    ![Nieuwe sql-database 17](./media/sql-database-get-started/sql-database-new-database-17.png)

20. Klik op de werkbalk portal klikt u op de items **meldingen** als de status van uw implementatie wilt controleren.

    ![Nieuwe sql-database 18](./media/sql-database-get-started/sql-database-new-database-18.png)

>[AZURE.IMPORTANT]Wanneer implementatie is voltooid, worden uw nieuwe Azure SQL-server en database gemaakt in Azure wordt aangegeven. Niet is mogelijk verbinding maken met uw nieuwe server of de database met behulp van hulpmiddelen voor SQL Server totdat u een regel van de firewall server als u wilt openen, de firewall SQL-Database om verbindingen vanuit buiten Azure te maken.

[AZURE.INCLUDE [Create server firewall rule](../../includes/sql-database-create-new-server-firewall-portal.md)]

## <a name="next-steps"></a>Volgende stappen
Nu dat u klaar bent met deze zelfstudie SQL-Database en een database gemaakt met voorbeeldgegevens, u klaar om te verkennen bent met behulp van uw favoriete hulpmiddelen.

- Als u bekend bent met Transact-SQL en SQL Server Management Studio (SSMS), leert u hoe u [verbinding maken en een SQL-database met SSMS query](sql-database-connect-query-ssms.md).

- Als u Excel weten, leert u hoe u [verbinding maken met een SQL-database in Azure wordt aangegeven met Excel](sql-database-connect-excel.md).

- Als u klaar om te beginnen met kleurcodering bent, kiest u uw programmeertaal bij [verbinding bibliotheken voor SQL-Database en SQL Server](sql-database-libraries.md).

- Als u verplaatsen van uw lokale SQL Server-databases naar Azure wilt, raadpleegt u [migreren van een database met SQL-Database](sql-database-cloud-migrate.md) voor meer informatie.

- Als u laden sommige gegevens in een nieuwe tabel uit een CSV-bestand wilt met behulp van het hulpprogramma voor de BCP, raadpleegt u de [gegevens naar SQL-Database uit een CSV-bestand met BCP laden](sql-database-load-from-csv-with-bcp.md).

- Als u wilt laten beginnen met het verkennen van Azure SQL Database-beveiliging, raadpleegt u [aan de slag met beveiliging](sql-database-get-started-security.md)


## <a name="additional-resources"></a>Aanvullende informatie

[Wat is de SQL-Database?](sql-database-technical-overview.md)
