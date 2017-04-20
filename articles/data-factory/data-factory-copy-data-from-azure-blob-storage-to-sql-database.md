<properties
    pageTitle="Kopieer gegevens uit Blob Storage met SQL-Database | Microsoft Azure"
    description="Deze zelfstudie ziet u hoe kopie activiteit in een pijplijn Azure gegevens Factory gebruiken om gegevens te kopiëren uit Blob storage met SQL-database."
    Keywords="BLOB sql, blobopslag, gegevens kopiëren"
    services="data-factory"
    documentationCenter=""
    authors="spelluru"
    manager="jhubbard"
    editor="monicar"/>

<tags
    ms.service="data-factory"
    ms.workload="data-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article" 
    ms.date="09/26/2016"
    ms.author="spelluru"/>

# <a name="copy-data-from-blob-storage-to-sql-database-using-data-factory"></a>Gegevens uit Blob Storage met SQL-Database met Data Factory kopiëren 
> [AZURE.SELECTOR]
- [Overzicht en vereisten](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md)
- [Wizard kopiëren](data-factory-copy-data-wizard-tutorial.md)
- [Azure-portal](data-factory-copy-activity-tutorial-using-azure-portal.md)
- [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md)
- [PowerShell](data-factory-copy-activity-tutorial-using-powershell.md)
- [Azure resourcemanager-sjabloon](data-factory-copy-activity-tutorial-using-azure-resource-manager-template.md)
- [REST API](data-factory-copy-activity-tutorial-using-rest-api.md)
- [.NET-API](data-factory-copy-activity-tutorial-using-dotnet-api.md)


In deze zelfstudie kunt u een factory gegevens maken met een pijplijn gegevens van Blob storage kopiëren met SQL-database.

De activiteit kopie kunt u de verplaatsing van gegevens uitvoeren in fabriek van Azure-gegevens. Is uitgerust met een globaal beschikbare service die gegevens tussen verschillende opgeslagen in de gegevens op een veilige, betrouwbare en scalable manier kunt kopiëren. Zie [Gegevens verkeer activiteiten](data-factory-data-movement-activities.md) artikel voor meer informatie over de activiteit kopiëren.  

> [AZURE.NOTE] Zie het artikel [Inleiding tot Factory van Azure-gegevens](data-factory-introduction.md) voor een gedetailleerd overzicht van de gegevens Factory-service.

##<a name="prerequisites-for-the-tutorial"></a>Vereisten voor de zelfstudie
Voordat u deze zelfstudie begint, hebt u het volgende:

- **Azure-abonnement**.  Als u niet een abonnement hebt, kunt u een gratis proefabonnement-account maken in een paar minuten. Zie het artikel [Gratis proefversie](http://azure.microsoft.com/pricing/free-trial/) voor meer informatie.
- **Azure opslag-Account**. U kunt de blobopslag gebruiken als een **bron** -gegevensopslag in deze zelfstudie. Als u niet een Azure opslag-account hebt, raadpleegt u het artikel [een opslag-account maken](../storage/storage-create-storage-account.md#create-a-storage-account) voor stapsgewijze instructies voor het maken van een.
- **Azure SQL-Database**. U kunt een Azure SQL-database gebruiken als een gegevensopslag **bestemming** in deze zelfstudie. Als u geen een Azure SQL-database die u in deze zelfstudie gebruiken kunt hebt, raadpleegt u [het maken en configureren van een Azure SQL-Database](../sql-database/sql-database-get-started.md) maken.
- **SQL Server 2012-2014 of Visual Studio-2013**. U SQL Server Management Studio of Visual Studio gebruiken om een voorbeelddatabase te maken en de resultaatgegevens in de database.  

## <a name="collect-blob-storage-account-name-and-key"></a>Blob-opslagaccountnaam en van toetsen verzamelen 
Moet u de naam van het account en de accountsleutel van uw account Azure opslag moet deze zelfstudie. Houd er rekening mee omlaag **accountnaam** en **accountsleutel** voor uw account Azure opslag.

1. Meld u aan bij de [portal van Azure](https://portal.azure.com/).
2. Klik op **meer services** in het linkermenu en selecteer **Opslag-Accounts**.

    ![Bladeren - opslag-accounts](media\data-factory-copy-data-from-azure-blob-storage-to-sql-database\browse-storage-accounts.png)
3. Selecteer het **Azure opslag-account** dat u wilt gebruiken in deze zelfstudie in het blad **Opslag-Accounts** .
4. Selecteer de koppeling **toegangstoetsen** onder **Instellingen**.
5.  Klik op de knop **kopiëren** (afbeelding) naast het tekstvak **opslagaccountnaam** en opslaan en ergens plakken (bijvoorbeeld: in een tekstbestand).
6. Herhaal de vorige stap om te kopiëren of noteer de **sleutel1**.
    
    ![Toegangstoets opslag](media\data-factory-copy-data-from-azure-blob-storage-to-sql-database\storage-access-key.png)
7. Sluit alle bladen door te klikken op **X**.

## <a name="collect-sql-server-database-user-names"></a>SQL server, database, gebruikersnamen verzamelen
Moet u de namen van Azure SQL server-database en gebruiker moet deze zelfstudie. Houd er rekening mee omlaag namen van de **server**en **database** **gebruiker** voor uw Azure SQL-database.

1. In de **Azure-portal**, klik op **meer services** aan de linkerkant en selecteer **SQL-databases**.
2. Selecteer de **database** die u wilt gebruiken in deze zelfstudie in de **SQL-databases blade**. Let op de **naam van de database**.  
3. Klik op **Eigenschappen** onder **Instellingen**in het blad **SQL-database** .
4. Houd er rekening mee omlaag de waarden voor **De naam van de SERVER** en **Aanmelden bij SERVER-beheerder**.
5. Sluit alle bladen door te klikken op **X**.

## <a name="allow-azure-services-to-access-sql-server"></a>Azure services voor toegang tot SQL server toestaan 
Zorg ervoor **dat **toegang geeft tot Azure services** ingeschakeld voor uw Azure SQL-server zodat de gegevens Factory-service toegang hebt tot uw Azure SQL-server** . Om te verifiëren en schakel deze instelling, voert u de volgende stappen uit:

1. Klik op **meer services** hub aan de linkerkant en klik op **SQL-servers**.
2. Selecteer uw server en **Firewall** onder **Instellingen**op. 
4. Klik op **op** voor **toegang tot Azure services toestaan**in het blad **firewallinstellingen** .
5. Sluit alle bladen door te klikken op **X**.

## <a name="prepare-blob-storage-and-sql-database"></a>Blob Storage en SQL-Database voorbereiden 
Bereid nu uw Azure-blobopslag en Azure SQL-database voor de zelfstudie door de volgende stappen uit te voeren:  

1. Kladblok starten, plak de volgende tekst en opslaan als **emp.txt** naar **C:\ADFGetStarted** -map op uw harde schijf.

        John, Doe
        Jane, Doe

2. Hulpprogramma's zoals [Azure opslag Explorer](https://azurestorageexplorer.codeplex.com/) gebruiken voor het maken van de container **adftutorial** en het **emp.txt** -bestand uploaden naar de container.

    ![Azure opslag Explorer. Kopieer gegevens uit Blob storage met SQL-database](./media/data-factory-copy-data-from-azure-blob-storage-to-sql-database/getstarted-storage-explorer.png)
3. Gebruik het volgende SQL-script om te maken van de tabel **emp** in uw Azure SQL-Database.  


        CREATE TABLE dbo.emp
        (
            ID int IDENTITY(1,1) NOT NULL,
            FirstName varchar(50),
            LastName varchar(50),
        )
        GO

        CREATE CLUSTERED INDEX IX_emp_ID ON dbo.emp (ID);

    **Als u SQL Server 2012-2014 geïnstalleerd op uw computer hebt:** Volg de instructies uit [stap 2: verbinding maken met SQL-Database van het beheren van Azure SQL-Database met SQL Server Management Studio](../sql-database/sql-database-manage-azure-ssms.md#Step2) artikel verbinding maken met uw Azure SQL-server en de SQL-script uitvoeren. In dit artikel worden de [klassieke Azure-portal](http://manage.windowsazure.com), niet de [nieuwe portal van Azure](https://portal.azure.com)gebruikt voor het configureren van de firewall voor een Azure SQL-server.

    Als de klant is niet toegestaan voor toegang tot de Azure SQL-server, moet u voor het configureren van de firewall voor uw Azure SQL-server om toegang te krijgen van uw computer (IP-adres). Zie [dit artikel](../sql-database/sql-database-configure-firewall-settings.md) voor stapsgewijze instructies voor het configureren van de firewall voor uw Azure SQL-server.

U kunt de vereisten hebt voltooid. U kunt een factory voor gegevens met een van de volgende manieren maken. Klik op een van de tabbladen aan de bovenkant of de volgende koppelingen om uit te voeren de zelfstudie.     

- [Azure-portal](data-factory-copy-activity-tutorial-using-azure-portal.md)
- [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md)
- [PowerShell](data-factory-copy-activity-tutorial-using-powershell.md)
- [REST API](data-factory-copy-activity-tutorial-using-rest-api.md)
- [.NET-API](data-factory-copy-activity-tutorial-using-dotnet-api.md)
- [Wizard kopiëren](data-factory-copy-data-wizard-tutorial.md)
