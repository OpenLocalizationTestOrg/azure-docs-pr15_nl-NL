### <a name="prerequisites"></a>Vereisten voor
- Een Azure-account; u kunt een [gratis account](https://azure.microsoft.com/free) maken
- Een [Azure SQL-Database](../articles/sql-database/sql-database-get-started.md) maken met de verbindingsgegevens, zoals de servernaam, de naam van de database en de gebruikersnaam en wachtwoord. Deze informatie is opgenomen in de verbindingsreeks van de SQL-Database:
  
    Server = tcp:*yoursqlservername*. database.windows.net,1433;Initial catalogus =*yourqldbname*; Gegevens voor beveiliging persistent = ONWAAR; Gebruikers-ID = {your_username}; Wachtwoord = {your_password}; Voor MultipleActiveResultSets = ONWAAR; Versleutelen = ONWAAR zijn. TrustServerCertificate = ONWAAR; Time-out = 30;

    Lees meer over [Azure SQL-Databases](https://azure.microsoft.com/services/sql-database).

> [AZURE.NOTE] Wanneer u een Azure SQL-Database maakt, kunt u ook de voorbeelddatabases wordt geleverd bij SQL maken. 



Voordat u uw Azure SQL-Database in een app logica, verbinding maken met uw SQL-Database. U kunt dit eenvoudig doen binnen uw app logica op de Azure-portal.  

Verbinding maken met uw Azure SQL-Database met de volgende stappen:  

1. Een logica-app maken. Klik in de ontwerpfunctie logica Apps een trigger toevoegen en vervolgens een actie toevoegen. Selecteer **Microsoft weergeven beheerde API's** in de vervolgkeuzelijst en voer 'sql' in het zoekvak. Selecteer een van de acties:  

    ![Stap voor SQL Azure verbinding maken](./media/connectors-create-api-sqlazure/sql-actions.png)

2. Als u geen verbindingen met SQL-Database nog niet eerder hebt gemaakt, kunt u wordt gevraagd om de verbindingsgegevens:  

    ![Stap voor SQL Azure verbinding maken](./media/connectors-create-api-sqlazure/connection-details.png) 

3. Geef de details van de SQL-Database. Eigenschappen met een sterretje zijn vereist.

    | Eigenschap | Meer informatie |
|---|---|
| Gateway-Verbind | Laat u dit niet inschakelt. Dit wordt gebruikt wanneer u verbinding maakt met een lokale SQL Server. |
| Verbindingsnaam * | Voer een naam voor de verbinding. | 
| De naam van de SQL-Server * | Voer de naam van de server; welke ongeveer zo *servername.database.windows.net*is. Naam van de server is weergegeven in de SQL-Database-eigenschappen in de portal van Azure en ook weergegeven in de verbindingstekenreeks. | 
| De naam van de SQL-Database * | Voer de naam die u hebt gekregen van uw SQL-Database. Dit wordt weergegeven in de SQL-Database-eigenschappen in de verbindingstekenreeks: initiële catalogus =*yoursqldbname*. | 
| Gebruikersnaam * | Voer de gebruikersnaam die u hebt gemaakt tijdens de SQL-Database is gemaakt. Dit wordt weergegeven in de SQL-Database-eigenschappen in de portal van Azure. | 
| Wachtwoord * | Voer het wachtwoord die u hebt gemaakt tijdens de SQL-Database is gemaakt. | 

    Deze referenties worden gebruikt voor het toestaan van uw app logica verbinding maakt en toegang tot uw SQL-gegevens. Als voltooid, is uw verbindingsgegevens er ongeveer als volgt uit:  

    ![Stap voor SQL Azure verbinding maken](./media/connectors-create-api-sqlazure/sample-connection.png) 

4. Selecteer **maken**. 

5. Zoals u ziet dat de verbinding is gemaakt. Nu kunt u doorgaan met de overige stappen in uw app logica: 

    ![Stap voor SQL Azure verbinding maken](./media/connectors-create-api-sqlazure/table.png)