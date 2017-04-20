<properties
    pageTitle="Aan de slag met SQL-Databases gegevens synchroniseren"
    description="Deze zelfstudie kunt u aan de slag met het synchroniseren in de Azure SQL-gegevens (Preview)."
    services="sql-database"
    documentationCenter=""
    authors="jennieHubbard"
    manager="jhubbard"
    editor=""/>

<tags
    ms.service="sql-database"
    ms.workload="data-management"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="07/11/2016"
    ms.author="jhubbard"/>


#<a name="getting-started-with-azure-sql-data-sync-preview"></a>Aan de slag met de synchronisatie van Azure SQL-gegevens (Preview)
In deze zelfstudie leert u de grondbeginselen van Azure SQL gegevens synchroniseren met behulp van de Azure klassieke Portal.

Deze zelfstudie wordt ervan uitgegaan minimale ervaring met SQL Server en Azure SQL-Database. In deze zelfstudie maakt u een groep van de synchronisatie hybride (SQL Server en SQL-Database exemplaren) volledig geconfigureerd en gesynchroniseerd op de planning die u instellen.

> [AZURE.NOTE] De volledige technische documentatie instellen voor Azure SQL-gegevens synchroniseren, voorheen zich bevindt op MSDN, is beschikbaar als een PDF-bestand. Download deze [hier](http://download.microsoft.com/download/4/E/3/4E394315-A4CB-4C59-9696-B25215A19CEF/SQL_Data_Sync_Preview.pdf).

## <a name="step-1-connect-to-the-azure-sql-database"></a>Stap 1: Verbinding maken met de SQL Azure-Database

1. Meld u aan bij de [klassieke Portal](http://manage.windowsazure.com).

2. Klik op **SQL-DATABASES** in het linkerdeelvenster.

3. Klik op **SYNCHRONISEREN** onder aan de pagina. Wanneer u op SYNCHRONISEREN klikt, verschijnt er een lijst met wat die u toevoegen kunt - **Nieuwe synchronisatie groep** en **Nieuwe synchronisatie-Agent**.

4. Als u wilt de wizard nieuwe SQL gegevens synchroniseren Agent starten, klikt u op **Nieuwe synchronisatie-Agent**.

5. Als u een agent voordat, **Klik op downloaden hier**nog niet hebt toegevoegd.

    ![Image1](./media/sql-database-get-started-sql-data-sync/SQLDatabaseScreen-Figure1.PNG)


## <a name="step-2-add-a-client-agent"></a>Stap 2: Een Client-Agent toevoegen
Deze stap is alleen vereist als u wilt een lokale SQL Server-database opgenomen in uw groep synchronisatie hebt. Gaat u verder met stap 4 als uw groep synchronisatie alleen exemplaren van SQL-Database heeft.

<a id="InstallRequiredSoftware"></a>
### <a name="step-2a-install-the-required-software"></a>Stap 2a: de vereiste software installeren
Zorg ervoor dat u hebt de volgende handelingen uit op de computer waarop het installeren van de Client-Agent is geïnstalleerd.

- **.NET framework 4.0**

 .NET Framework 4.0 installeren vanaf [hier](http://go.microsoft.com/fwlink/?linkid=205836).

- **Microsoft SQL Server 2008 R2 SP1 System CLR Types (x86)**

 De Microsoft SQL Server 2008 R2 SP1 System CLR Types (x86) installeren vanaf [hier](http://www.microsoft.com/download/en/details.aspx?id=26728)

- **Microsoft SQL Server 2008 R2 SP1 gedeeld Management objecten (x86)**

 Microsoft SQL Server 2008 R2 SP1 gedeeld Management Objects (x86) installeren vanaf [hier](http://www.microsoft.com/download/en/details.aspx?id=26728)



<a id="InstallClient"></a>
### <a name="step-2b-install-a-new-client-agent"></a>Stap 2b: een nieuwe Client-Agent installeren

Volg de instructies in het [installeren van een Client-Agent (synchronisatie van de SQL-gegevens)](http://download.microsoft.com/download/4/E/3/4E394315-A4CB-4C59-9696-B25215A19CEF/SQL_Data_Sync_Preview.pdf) de agent installeren.



<a id="RegisterSSDb"></a>
### <a name="step-2c-finish-the-new-sql-data-sync-agent-wizard"></a>Stap 2c: voltooien van de wizard nieuwe SQL gegevens synchroniseren Agent

1.  Ga terug naar de wizard nieuwe SQL gegevens synchroniseren Agent.
2.  De agent een duidelijke naam geeft.
3.  In de vervolgkeuzelijst, selecteer de **regio** (Datacenter) voor het hosten van deze agent.
4.  In de vervolgkeuzelijst, selecteer het **abonnement** voor het hosten van deze agent.
5.  Klik op de pijl naar rechts.



## <a name="step-3-register-a-sql-server-database-with-the-client-agent"></a>Stap 3: Een SQL Server-database met de Client-Agent registreren

Nadat de Client-Agent is geïnstalleerd, registreren elke lokale SQL Server-database die u wilt opnemen in een groep synchroniseren met de agent.
Als u wilt een database met de agent hebt geregistreerd, volgt u de instructies bij het [registreren van een SQL Server-Database met een Client-Agent](http://download.microsoft.com/download/4/E/3/4E394315-A4CB-4C59-9696-B25215A19CEF/SQL_Data_Sync_Preview.pdf).



## <a name="step-4-create-a-sync-group"></a>Stap 4: Een synchronisatie-groep maken


<a id="StartNewSGWizard"></a>
### <a name="step-4a-start-the-new-sync-group-wizard"></a>Stap 4a: Start de wizard nieuwe synchronisatie-groep

1.  Ga terug naar de [klassieke Portal](http://manage.windowsazure.com).
2.  Klik op **SQL-DATABASES**.
3.  Klik op **SYNCHRONISEREN toevoegen** onder aan de pagina en selecteer nieuwe groep van de synchronisatie van het vel.

    ![Image2](./media/sql-database-get-started-sql-data-sync/NewSyncGroup-Figure2.png)



<a id=""></a>
### <a name="step-4b-enter-the-basic-settings"></a>Stap 4b: Voer de basisinstellingen


1.  Voer een zinvolle naam voor de synchronisatie-groep.
2.  In de vervolgkeuzelijst, selecteer de **regio** (Data Center) voor het hosten van deze groep synchroniseren.
3. Klik op de pijl naar rechts.

    ![Image3](./media/sql-database-get-started-sql-data-sync/NewSyncGroupName-Figure3.PNG)


<a id="DefineHubDB"></a>
### <a name="step-4c-define-the-sync-hub"></a>Stap 4c: de synchronisatie-hub definiëren

1. In de vervolgkeuzelijst, selecteert u het exemplaar van de SQL-Database als de hub van de groep synchronisatie moet fungeren.
2. Voer de referenties voor dit exemplaar van de SQL-Database - **HUB-gebruikersnaam** en **Wachtwoord van de HUB**.
3. Wacht totdat SQL gegevens synchroniseren met de gebruikersnaam en wachtwoord bevestigen. Hier ziet u een groen vinkje aan de rechterkant van het wachtwoord wordt weergegeven wanneer de referenties worden bevestigd.
4. In de vervolgkeuzelijst, selecteert u het beleid **CONFLICTOPLOSSING** .

 **Hub Wins** - er een wijziging naar het schrijven van de database hub naar de verwijzing-databases geschreven, verwijzen naar overschrijven wijzigingen in dezelfde databaserecord. Functioneel, betekent dit dat de eerste wijziging naar de hub geschreven aan de andere databases doorgegeven.


 **Client Wins** - wijzigingen naar de hub geschreven overschreven door wijzigingen in de verwijzing databases. Functioneel, betekent dit dat de laatste wijziging naar de hub geschreven het apparaat dat bewaard en doorgegeven aan de andere databases.

5.  Klik op de pijl naar rechts.

    ![Image4](./media/sql-database-get-started-sql-data-sync/NewSyncGroupHub-Figure4.PNG)

<a id="AddRefDB"></a>
### <a name="step-4d-add-a-reference-database"></a>Stap 4d: een verwijzing database toevoegen


Herhaal deze stap voor elke extra database die u wilt toevoegen aan de groep synchroniseren.

1. In de vervolgkeuzelijst, selecteert u de database om toe te voegen.

    Databases in de vervolgkeuzelijst bevatten beide SQL Server-databases die zijn geregistreerd met de agent en exemplaren van de SQL-Database.
2.  Voer de referenties voor deze database - **gebruikersnaam** en **wachtwoord**.
3.  In de vervolgkeuzelijst, selecteert u de **Richting van de synchronisatie** van deze database.

    **Bidirectionele** - wijzigingen in de database verwijzing naar de hub-database worden geschreven en wijzigingen in de hub-database naar de verwijzing database zijn geschreven.

    **Synchroniseren op basis van de Hub** - de database ontvangt updates van de Hub. Deze verzendt geen wijzigingen naar de Hub.

    **Synchroniseren met de Hub** - de database worden updates naar de Hub. Wijzigingen in de Hub worden niet op deze database opgeslagen.

4.  Klik op het vinkje in de rechterbenedenhoek van de wizard om de synchronisatie-groep maken. Wacht totdat SQL gegevens synchroniseren met de referenties bevestigen. Een groen vinkje geeft aan dat de referenties zijn bevestigd.

5.  Klik nogmaals op het vinkje. U keert terug naar de pagina **synchronisatie** onder SQL-Databases. Deze groep synchronisatie wordt nu weergegeven met uw andere groepen synchroniseren en agenten.

    ![Image5](./media/sql-database-get-started-sql-data-sync/NewSyncGroupReference-Figure5.PNG)


## <a name="step-5-define-the-data-to-sync"></a>Stap 5: De gegevens wilt synchroniseren definiëren

Synchronisatie van Azure SQL-gegevens kunt u Selecteer tabellen en kolommen die u wilt synchroniseren. Desgewenst kunt u ook een kolom filteren, zodat u dat alleen rijen met specifieke waarden (zoals leeftijd > = 65) synchroniseren, gebruikt u de portal gegevens synchroniseren van SQL Azure en selecteert u de documentatie bij de tabellen, kolommen en rijen te synchroniseren om de gegevens te definiëren om te synchroniseren.

1.  Ga terug naar de [klassieke Portal](http://manage.windowsazure.com).
2.  Klik op **SQL-DATABASES**.
3.  Klik op het tabblad **synchronisatie** .
4.  Klik op de naam van deze groep synchroniseren.
5.  Klik op het tabblad **Regels voor synchronisatie** .
6.  Selecteer de database die u wilt opgeven van het schema van de groep synchroniseren.
7.  Klik op de pijl naar rechts.
8.  Klik op **SCHEMA vernieuwen**.
9.  Voor elke tabel in de database, selecteert u de kolommen als in de synchronisatie wilt opnemen.
    - Kolommen met niet-ondersteunde gegevenstypen kunnen niet worden geselecteerd.
    - Als er geen kolommen in een tabel zijn geselecteerd, wordt de tabel niet opgenomen in de groep synchroniseren.
    - Klik op selecteren onder aan het scherm om te selecteren/selectie opheffen alle tabellen.
10. Klik op **Opslaan**en klik wachten om door de synchronisatie-groep om te voltooien inrichten.
11. Als u wilt teruggaan naar de gegevens synchroniseren aantekening toevoegen, klikt u op de back-pijl in de linkerbovenhoek van het scherm (boven de naam van de synchronisatie-groep).

    ![Image6](./media/sql-database-get-started-sql-data-sync/NewSyncGroupSyncRules-Figure6.PNG)

## <a name="step-6-configure-your-sync-group"></a>Stap 6: De synchronisatie-groep configureren

U kunt een groep synchroniseren altijd synchroniseren door te klikken op SYNCHRONISEREN onderaan in de openingspagina van gegevens synchroniseren.
Als u wilt synchroniseren, moet u de synchronisatie-groep configureren.

1.  Ga terug naar de [klassieke Portal](http://manage.windowsazure.com).
2.  Klik op **SQL-DATABASES**.
3.  Klik op het tabblad **synchronisatie** .
4.  Klik op de naam van deze groep synchroniseren.
5.  Klik op het tabblad **configureren** .
6.  **AUTOMATISCHE SYNCHRONISATIE**
    - Als u wilt configureren de synchronisatie-groep waaraan u wilt synchroniseren op een interval instellen, klikt u op **aan**. U kunt nog steeds op aanvraag synchroniseren door te klikken op SYNCHRONISEREN.
    - Klik op **uit** om het configureren van de synchronisatie-groep waaraan u wilt synchroniseren alleen wanneer u op SYNCHRONISEREN klikt.
7.  **FREQUENTIE VAN SYNCHRONISATIE**
    - Als automatische synchronisatie ingeschakeld is, stelt u de synchronisatiefrequentie. De frequentie moet liggen tussen 5 minuten en 1 maand.
8.  Klik op **Opslaan**.

![Image7](./media/sql-database-get-started-sql-data-sync/NewSyncGroupConfigure-Figure7.PNG)

Gefeliciteerd. U kunt een groep voor synchronisatie met zowel een exemplaar van de SQL-Database en een SQL Server-database hebt gemaakt.

## <a name="next-steps"></a>Volgende stappen
Voor meer informatie over de SQL-Database en SQL gegevens synchroniseren Zie:

* [De volledige SQL gegevens synchroniseren technische documentatie downloaden](http://download.microsoft.com/download/4/E/3/4E394315-A4CB-4C59-9696-B25215A19CEF/SQL_Data_Sync_Preview.pdf)
* [Overzicht van de SQL-Database](sql-database-technical-overview.md)
* [Beheer van productlevenscyclus database](https://msdn.microsoft.com/library/jj907294.aspx)
 

 
