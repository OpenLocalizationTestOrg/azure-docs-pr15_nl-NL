<properties
    pageTitle="Excel verbinden met SQL-Database | Microsoft Azure"
    description="Leer hoe u Microsoft Excel verbinden met Azure SQL-database in de cloud. Gegevens importeren in Excel voor rapportage en gegevens verkennen."
    services="sql-database"
    keywords="verbinding maken met excel naar sql, het importeren van gegevens naar excel"
    documentationCenter=""
    authors="joseidz"
    manager="jhubbard"
    editor=""/>


<tags
    ms.service="sql-database"
    ms.workload="data-management"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="07/05/2016"
    ms.author="joseidz"/>


# <a name="sql-database-tutorial-connect-excel-to-an-azure-sql-database-and-create-a-report"></a>SQL-Database zelfstudie: Excel verbinden met een Azure SQL-database en een rapport maken

> [AZURE.SELECTOR]
- [Visual Studio](sql-database-connect-query.md)
- [SSMS](sql-database-connect-query-ssms.md)
- [Excel](sql-database-connect-excel.md)

Leer hoe u Excel verbinden met een SQL-database in de cloud zodat u kunt gegevens importeren en tabellen en grafieken op basis van waarden in de database maken. In deze zelfstudie u de verbinding tussen Excel en een databasetabel wilt instellen, sla het bestand met gegevens en de verbindingsgegevens voor Excel en vervolgens een draaigrafiek te maken van de databasewaarden.

U moet een SQL-database in Azure wordt aangegeven voordat u begint. Als u deze niet hebt, raadpleegt u [uw eerste SQL-database maken](sql-database-get-started.md) om een database maken met de voorbeeldgegevens omhoog en worden uitgevoerd in een paar minuten. In dit artikel wordt u voorbeeldgegevens in Excel importeert van dit artikel, maar kunt u dezelfde stappen volgen op uw eigen gegevens.

U moet ook een exemplaar van Excel. In dit artikel worden de [Microsoft Excel-2016](https://products.office.com/en-US/)gebruikt.

## <a name="connect-excel-to-a-sql-database-and-create-an-odc-file"></a>Excel verbinden met een SQL-database en maak een odc-bestand

1.  Als u wilt verbinden met Excel met SQL-database, openen van Excel en maak een nieuwe werkmap of open een bestaand Excel-werkmap.

2.  Klik op **gegevens**in de menubalk boven aan de pagina op **Uit een andere bron**en klik vervolgens op **Van SQL Server**.

    ![Gegevensbron selecteren: Excel verbinden met SQL-database.](./media/sql-database-connect-excel/excel_data_source.png)

    De Wizard Gegevensverbinding wordt geopend.

3.  Typ in het dialoogvenster **verbinding maken met een databaseserver** de SQL-Database **servernaam** die u wilt verbinden met in het formulier <*servernaam*>**. database.windows.net**. Bijvoorbeeld: **adworkserver.database.windows.net**.

4.  Klik onder **aanmeldingsreferenties**, klikt u op **de volgende gebruikersnaam en wachtwoord**, typ de **Gebruikersnaam** en **wachtwoord** dat u voor de SQL-databaseserver instellen wanneer u deze hebt gemaakt en klik vervolgens op **volgende**.

    ![Typ de referenties voor de naam en u aanmelden](./media/sql-database-connect-excel/connect-to-server.png)

    > [AZURE.TIP] U mogelijk niet om verbinding te afhankelijk van uw netwerkomgeving, of u kunt de verbinding mogelijk verloren als de SQL-databaseserver verkeer van uw IP-adres van client niet is toegestaan. Ga naar de [portal van Azure](https://portal.azure.com/)op SQL-servers, klik op de server, klikt u op firewall onder instellingen en uw IP-adres van client toevoegen. Lees [hoe u de firewallinstellingen configureren](sql-database-configure-firewall-settings.md) voor meer informatie.

5. Selecteer in het dialoogvenster **Database en tabel selecteren** de database die u werken wilt met in de lijst en klik op de tabellen of weergaven die u werken wilt met (we hebben **vGetAllCategories**gekozen), en klik op **volgende**.

    ![Een database en tabel selecteren.](./media/sql-database-connect-excel/select-database-and-table.png)

    Het dialoogvenster **bestand opslaan en voltooien** wordt geopend, waar u bevatten informatie over het Office-database-verbinding (*.odc)-bestand dat Excel wordt gebruikt. U kunt de standaardwaarden verlaten of aanpassen van de gewenste opties.

6. U kunt de standaardwaarden verlaten, maar u kunt met name noteert u de **Bestandsnaam** . Een **Beschrijving**, een **Beschrijvende naam**en **Trefwoorden zoeken** kunnen u en andere gebruikers Vergeet niet wat u verbinding maakt met en zoek de verbinding. Klik op **altijd proberen dit bestand als gegevens wilt vernieuwen te gebruiken** als u wilt dat verbindingsgegevens die zijn opgeslagen in het ODC-bestand, zodat deze kan worden bijgewerkt wanneer u verbinding met deze maken en klik vervolgens op **Voltooien**.

    ![Een ODC-bestand op te slaan](./media/sql-database-connect-excel/save-odc-file.png)

    Het dialoogvenster **gegevens importeren** wordt weergegeven.

## <a name="import-the-data-into-excel-and-create-a-pivot-chart"></a>De gegevens importeren in Excel en een draaigrafiek maken
Nu dat u de verbinding hebt gemaakt en het bestand met gegevens en verbinding hebt gemaakt, u leest om de gegevens te importeren.

1. In het dialoogvenster **Gegevens importeren** op de optie die u wilt gebruiken voor uw gegevens presenteren in het werkblad en klik vervolgens op **OK**. We hebben gekozen **draaigrafiek**. U kunt er ook voor kiezen om te maken van een **Nieuw werkblad** of naar **deze gegevens toevoegen aan een gegevensmodel**. Zie voor meer informatie over gegevensmodellen [maken een gegevensmodel in Excel](https://support.office.com/article/Create-a-Data-Model-in-Excel-87E7A54C-87DC-488E-9410-5C75DBCB0F7B). Klik op **Eigenschappen** om te verkennen van informatie over het ODC-bestand dat u in de vorige stap hebt gemaakt en kiest u opties voor het vernieuwen van de gegevens.

    ![Het kiezen van de opmaak van gegevens in Excel](./media/sql-database-connect-excel/import-data.png)

    Het werkblad heeft nu een lege draaitabel en de grafiek.

8. Selecteer onder **Draaitabelvelden**alle-selectievakjes voor de velden die u wilt weergeven.

    ![Databaserapport configureren.](./media/sql-database-connect-excel/power-pivot-results.png)

> [AZURE.TIP]Als u verbinden met andere Excel-werkmappen en werkbladen in de database wilt, klik op **gegevens**, klikt u op **verbindingen**, klikt u op **toevoegen**, kiest u de verbinding die u hebt gemaakt in de lijst en klik vervolgens op **openen**.
> ![Een verbinding openen vanuit een andere werkmap](./media/sql-database-connect-excel/open-from-another-workbook.png)

## <a name="next-steps"></a>Volgende stappen

- Leer hoe u [verbinding maken met SQL-Database met SQL Server Management Studio](sql-database-connect-query-ssms.md) voor Geavanceerd zoeken en analyse.
- Meer informatie over de voordelen van [elastische toepassingen](sql-database-elastic-pool.md).
- Informatie over het [maken van een webtoepassing die verbinding met SQL-Database op de back-enddatabase maakt](../app-service-web/web-sites-dotnet-deploy-aspnet-mvc-app-membership-oauth-sql-database.md).
