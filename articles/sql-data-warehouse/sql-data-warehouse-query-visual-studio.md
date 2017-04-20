<properties
   pageTitle="Query Azure SQL datawarehouse (Visual Studio) | Microsoft Azure"
   description="Query SQL datawarehouse met Visual Studio."
   services="sql-data-warehouse"
   documentationCenter="NA"
   authors="sonyam"
   manager="barbkess"
   editor=""/>

<tags
   ms.service="sql-data-warehouse"
   ms.devlang="NA"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-services"
   ms.date="06/16/2016"
   ms.author="sonyama;barbkess"/>

# <a name="query-azure-sql-data-warehouse-visual-studio"></a>Query Azure SQL datawarehouse (Visual Studio)

> [AZURE.SELECTOR]
- [Power BI](sql-data-warehouse-get-started-visualize-with-power-bi.md)
- [Azure Machine Learning](sql-data-warehouse-get-started-analyze-with-azure-machine-learning.md)
- [Visual Studio](sql-data-warehouse-query-visual-studio.md)
- [Sqlcmd](sql-data-warehouse-get-started-connect-sqlcmd.md) 

Gebruik Visual Studio op query Azure SQL Data Warehouse in slechts een paar minuten. Deze methode heeft de extensie SQL Server Data Tools (SSDT) in Visual Studio. 

## <a name="prerequisites"></a>Vereisten voor

Als u wilt deze zelfstudie hebt gebruikt, hebt u het volgende nodig:

+ Een bestaande SQL datawarehouse. Zie [maken een SQL-Data Warehouse][]om een.
+ SSDT voor Visual Studio. Hebt u Visual Studio, al u waarschijnlijk volgt. Zie [Visual Studio installeren en SSDT][]voor installatie-instructies en opties.
+ De volledig gekwalificeerde SQL server-naam. Dit vinden? Zie [verbinding maken met SQL Data Warehouse][].

## <a name="1-connect-to-your-sql-data-warehouse"></a>1. verbinding maken met uw SQL datawarehouse

1. Open Visual Studio 2013 of 2015.
2. SQL Server-Object Explorer openen. Selecteer hiertoe **weergave** > **SQL Server-Object Explorer**.

    ![SQL Server-Object Explorer][1]

3. Klik op het pictogram van het **Toevoegen van SQL Server** .

    ![SQL Server toevoegen][2]

4. Vul de velden in de verbinding maken met het venster Server.

    ![Verbinding maken met de Server][3]

    - **De naam van de server**. Voer de **servernaam** die eerder zijn geïdentificeerd.
    - **Verificatie**. Selecteer **SQL Server** of **geïntegreerde Active Directory-verificatie**.
    - **Gebruikersnaam** en **wachtwoord**. Voer de gebruikersnaam en wachtwoord als SQL Server-verificatie is geselecteerd.
    - Klik op **verbinding maken**.

5. Als u wilt verkennen, uitvouwen uw Azure SQL-server. U kunt de databases die zijn gekoppeld aan de server weergeven. AdventureWorksDW als u wilt zien van de tabellen in de voorbeelddatabase uitvouwen.

    ![AdventureWorksDW verkennen][4]

## <a name="2-run-a-sample-query"></a>2. steekproef query's uitvoeren

Nu dat is een verbinding met uw database gemaakt er, laten we een query te schrijven.

1. Met de rechtermuisknop op de database in SQL Server-Object Explorer.

2. Selecteer **nieuwe Query**. Een nieuwe queryvenster wordt geopend.

    ![Nieuwe query][5]

3. Kopieer deze TSQL-query in het queryvenster:

    ```sql
    SELECT COUNT(*) FROM dbo.FactInternetSales;
    ```

4. De query uitvoert. Klik hiertoe klikt u op de groene pijl of gebruik de volgende snelkoppeling: `CTRL` + `SHIFT` + `E`.

    ![Query uitvoeren][6]

5. Bekijk de queryresultaten. In dit voorbeeld bevat de tabel FactInternetSales 60398 rijen.

    ![Queryresultaten][7]

## <a name="next-steps"></a>Volgende stappen

Nu dat u kunt verbinding maken en query, kunt u [de gegevens met PowerBI visualiseren][].

Zie [verifiëren naar SQL Data Warehouse][]configureren van uw omgeving voor verificatie van Azure Active Directory.

<!--Arcticles-->
[Verbinding maken met SQL datawarehouse]: sql-data-warehouse-connect-overview.md
[Een SQL-datawarehouse maken]: sql-data-warehouse-get-started-provision.md
[Installatie van Visual Studio en SSDT]: sql-data-warehouse-install-visual-studio.md
[Verifiëren naar SQL datawarehouse]: sql-data-warehouse-authentication.md
[de gegevens met PowerBI visualiseren]: sql-data-warehouse-get-started-visualize-with-power-bi.md  

<!--Other-->
[Azure portal]: https://portal.azure.com

<!--Image references-->

[1]: media/sql-data-warehouse-query-visual-studio/open-ssdt.png
[2]: media/sql-data-warehouse-query-visual-studio/add-server.png
[3]: media/sql-data-warehouse-query-visual-studio/connection-dialog.png
[4]: media/sql-data-warehouse-query-visual-studio/explore-sample.png
[5]: media/sql-data-warehouse-query-visual-studio/new-query2.png
[6]: media/sql-data-warehouse-query-visual-studio/run-query.png
[7]: media/sql-data-warehouse-query-visual-studio/query-results.png
