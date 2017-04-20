<properties
   pageTitle="Logboeken aan de Website door middel van Azure gegevens Lake analyses analyseren | Azure"
   description="Informatie over het analyseren van gegevens Lake analyses met Logboeken aan de website. "
   services="data-lake-analytics"
   documentationCenter=""
   authors="edmacauley"
   manager="jhubbard"
   editor="cgronlun"/>

<tags
   ms.service="data-lake-analytics"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data"
   ms.date="05/16/2016"
   ms.author="edmaca"/>

# <a name="tutorial-analyze-website-logs-using-azure-data-lake-analytics"></a>Zelfstudie: Logboeken aan de Website door middel van Azure gegevens Lake analyses analyseren

Informatie over het analyseren van logboeken aan de website van het gebruik van gegevens Lake Analytics, met name op controleren welke verwijzingen opgetreden tijdens de op fouten wanneer ze willen Bezoek de website.

>[AZURE.NOTE] Als u alleen zien van de toepassing werken wilt, bespaart u tijd [Gebruik Azure gegevens Lake Analytics interactieve zelfstudies](data-lake-analytics-use-interactive-tutorials.md)doorlopen. Deze zelfstudie is gebaseerd op hetzelfde scenario en dezelfde code. Het doel van deze zelfstudie is te geven ontwikkelaars de ervaring van maken en uitvoeren van een toepassing gegevens Lake analyses van begin tot eind.

## <a name="prerequisites"></a>Vereisten:

- **Visual Studio-2015, Visual Studio 2013 4, of Visual Studio 2012 met visuele C++ geïnstalleerd bijwerken**.
- **Microsoft Azure SDK voor .NET versie 2.5 of hoger**.  Installeren met behulp van het [installatieprogramma van de Web-platform](http://www.microsoft.com/web/downloads/platform.aspx).
- **[Gegevens Lake Tools voor Visual Studio](http://aka.ms/adltoolsvs)**.

    Wanneer Data Lake Tools voor Visual Studio is geïnstalleerd, ziet u een menu **Gegevens Lake** in Visual Studio:

    ![I-SQL Visual Studio-menu](./media/data-lake-analytics-data-lake-tools-get-started/data-lake-analytics-data-lake-tools-menu.png)

- **Basiskennis van gegevens Lake analyses en de hulpmiddelen voor gegevens Lake voor Visual Studio**. Als u wilt beginnen, raadpleegt u:

    - [Aan de slag met Azure gegevens Lake analyses met behulp van Azure-Portal](data-lake-analytics-get-started-portal.md).
    - [Ontwikkel I-SQL-script met gegevens Lake tools for Visual Studio](data-lake-analytics-data-lake-tools-get-started.md).

- **Een gegevens Lake Analytics-account.**  Zie [een Azure gegevens Lake Analytics-account maken](data-lake-analytics-get-started-portal.md#create_adl_analytics_account).

    De hulpmiddelen voor gegevens Lake biedt geen ondersteuning maken gegevens Lake Analytics-accounts.  U hoeft dus te maken met behulp van de Azure-Portal, Azure PowerShell, .NET SDK of Azure CLI.
- **Upload de voorbeeldgegevens naar het gegevens Lake Analytics-account.** Zie [SearchLog.tsv uploaden naar het standaardaccount voor gegevensopslag Lake](data-lake-analytics-get-started-portal.md#update-data-to-the-default-adl-storage-account).

    Als u wilt een taak gegevens Lake analyses uitvoeren, moet u enkele gegevens. Hoewel de hulpmiddelen voor gegevens Lake uploaden gegevens ondersteunt, kunt u de portal wilt gebruiken voor het uploaden van de voorbeeldgegevens om deze zelfstudie te vereenvoudigen kunt volgen.

## <a name="connect-to-azure"></a>Verbinding maken met Azure

Voordat u kunt maken en testen van elke I-SQL-scripts, moet u eerst verbinding maken met Azure.

**Verbinding maken met gegevens Lake Analytics**

1. Open Visual Studio.
2. Klik op **Opties en instellingen**in het menu **Gegevens Lake** .
4. Klik op **Sign In**of de **Gebruiker wijzigen** als iemand heeft aangemeld, en volg de instructies.
5. Klik op **OK** om het dialoogvenster Opties en instellingen te sluiten.

**Om te bladeren van uw gegevens Lake Analytics-accounts**

1. Open in Visual Studio **Server Explorer** door druk op **CTRL + ALT + S**.
2. Vouw **Azure**in **Server Explorer**en vouwt u **Gegevens Lake Analytics**. Er wordt een lijst met uw gegevens Lake Analytics-accounts als er een. U kunt gegevens Lake Analytics-accounts maken van de studio. Zie [Aan de slag met Azure gegevens Lake analyses met behulp van Azure-Portal](data-lake-analytics-get-started-portal.md) of [Aan de slag met Azure gegevens Lake Analytics via Azure PowerShell](data-lake-analytics-get-started-powershell.md)een account maken.

## <a name="develop-u-sql-application"></a>I-SQL-toepassing ontwikkelen

Een I-SQL-toepassing is voornamelijk een script I-SQL. Zie meer informatie over het I-SQL, [aan de slag met I-SQL](data-lake-analytics-u-sql-get-started.md).

U kunt de gebruiker gedefinieerde operators toevoeging toevoegen aan de toepassing.  Voor meer informatie raadpleegt [U-SQL ontwikkelen door de gebruiker gedefinieerde operatoren voor gegevens Lake Analytics-taken](data-lake-analytics-u-sql-develop-user-defined-operators.md).

**Maken en indienen van een taak gegevens Lake Analytics**

1. Klik in het menu **bestand** op **Nieuw**en klik vervolgens op **Project**.
2. Selecteer het type Project I-SQL.

    ![nieuwe I-SQL Visual Studio-project](./media/data-lake-analytics-data-lake-tools-get-started/data-lake-analytics-data-lake-tools-new-project.png)

3. Klik op **OK**. Een oplossing maakt in Visual studio met een Script.usql-bestand.
4. Voer de volgende script in het bestand Script.usql:

        // Create a database for easy reuse, so you don't need to read from a file every time.
        CREATE DATABASE IF NOT EXISTS SampleDBTutorials;

        // Create a Table valued function. TVF ensures that your jobs fetch data from the weblog file with the correct schema.
        DROP FUNCTION IF EXISTS SampleDBTutorials.dbo.WeblogsView;
        CREATE FUNCTION SampleDBTutorials.dbo.WeblogsView()
        RETURNS @result TABLE
        (
            s_date DateTime,
            s_time string,
            s_sitename string,
            cs_method string,
            cs_uristem string,
            cs_uriquery string,
            s_port int,
            cs_username string,
            c_ip string,
            cs_useragent string,
            cs_cookie string,
            cs_referer string,
            cs_host string,
            sc_status int,
            sc_substatus int,
            sc_win32status int,
            sc_bytes int,
            cs_bytes int,
            s_timetaken int
        )
        AS
        BEGIN

            @result = EXTRACT
                s_date DateTime,
                s_time string,
                s_sitename string,
                cs_method string,
                cs_uristem string,
                cs_uriquery string,
                s_port int,
                cs_username string,
                c_ip string,
                cs_useragent string,
                cs_cookie string,
                cs_referer string,
                cs_host string,
                sc_status int,
                sc_substatus int,
                sc_win32status int,
                sc_bytes int,
                cs_bytes int,
                s_timetaken int
            FROM @"/Samples/Data/WebLog.log"
            USING Extractors.Text(delimiter:' ');
            RETURN;
        END;

        // Create a table for storing referrers and status
        DROP TABLE IF EXISTS SampleDBTutorials.dbo.ReferrersPerDay;
        @weblog = SampleDBTutorials.dbo.WeblogsView();
        CREATE TABLE SampleDBTutorials.dbo.ReferrersPerDay
        (
            INDEX idx1
            CLUSTERED(Year ASC)
            PARTITIONED BY HASH(Year)
        ) AS

        SELECT s_date.Year AS Year,
            s_date.Month AS Month,
            s_date.Day AS Day,
            cs_referer,
            sc_status,
            COUNT(DISTINCT c_ip) AS cnt
        FROM @weblog
        GROUP BY s_date,
                cs_referer,
                sc_status;

    Als u wilt weten over de I-SQL, Zie [aan de slag met gegevens Lake Analytics I-SQL-taal](data-lake-analytics-u-sql-get-started.md).    

5. Een nieuwe I-SQL-script toevoegen aan uw project en voer de volgende gegevens:

        // Query the referrers that ran into errors
        @content =
            SELECT *
            FROM SampleDBTutorials.dbo.ReferrersPerDay
            WHERE sc_status >=400 AND sc_status < 500;

        OUTPUT @content
        TO @"/Samples/Outputs/UnsuccessfulResponses.log"
        USING Outputters.Tsv();

6. Ga terug naar het eerste I-SQL-script en naast de knop **verzenden** , geeft u uw Analytics-account.
7. Vanuit **Solution Explorer**, **Script.usql**met de rechtermuisknop op en klik vervolgens op **Script maken**. Controleer of de resultaten in het deelvenster Uitvoer.
8. Vanuit **Solution Explorer**, **Script.usql**met de rechtermuisknop op en klik op **Script indienen**.
9. Controleer of dat het **Analytics-Account** is degene waar u de taak uitvoeren en klik vervolgens op **verzenden**. Resultaten van indienen en taakkoppeling zijn beschikbaar in de hulpmiddelen voor gegevens Lake voor Visual Studio, resulteert dit venster wanneer de indiening is voltooid.
10. Wacht totdat de taak is voltooid.  Als de taak is mislukt, wordt deze waarschijnlijk het bronbestand ontbreekt.  Zie de vereiste sectie van deze zelfstudie. Zie voor meer informatie over probleemoplossing [controleren en problemen met Azure gegevens Lake Analytics taken](data-lake-analytics-monitor-and-troubleshoot-jobs-tutorial.md).

    Wanneer de taak is voltooid, ziet u het volgende scherm:

    ![gegevens lake analytics analyseren weblogs website Logboeken](./media/data-lake-analytics-analyze-weblogs/data-lake-analytics-analyze-weblogs-job-completed.png)

11. Herhaal stap 7 - 10 nu voor **Script1.usql**.

>[AZURE.NOTE]U niet kunt lezen of schrijven naar een I-SQL-tabel die is gemaakt of gewijzigd in hetzelfde script.  Daarom heb ik twee scripts gebruiken voor dit voorbeeld.

**Om de uitvoer van de taak weer te geven**

1. Van **Server Explorer** **Azure**uitvouwen, **Gegevens Lake Analytics**uitvouwen, uitvouwen van uw gegevens Lake Analytics-account, **Opslag Accounts**uitvouwen, met de rechtermuisknop op het standaardaccount voor gegevensopslag Lake, en klik op **Explorer**.
2.  Dubbelklik op **voorbeelden** om de map te openen en dubbelklik vervolgens op **uitvoer**.
3.  Dubbelklik op **UnsuccessfulResponsees.log**.
4.  U kunt ook dubbelklikken op het uitvoerbestand binnen de grafiekweergave van de taak om te navigeren rechtstreeks naar de uitvoer.

## <a name="see-also"></a>Zie ook

Als u wilt beginnen met gegevens Lake analyses met verschillende hulpmiddelen, raadpleegt u:

- [Aan de slag met gegevens Lake analyses met behulp van Azure-Portal](data-lake-analytics-get-started-portal.md)
- [Aan de slag met gegevens Lake Analytics via Azure PowerShell](data-lake-analytics-get-started-powershell.md)
- [Aan de slag met gegevens Lake analyses met .NET SDK](data-lake-analytics-get-started-net-sdk.md)

Bekijk meer ontwikkeling onderwerpen:

- [I-SQL-scripts met Data Lake Tools voor Visual Studio ontwikkelen](data-lake-analytics-data-lake-tools-get-started.md)
- [Aan de slag met Azure gegevens Lake Analytics U SQL-taal](data-lake-analytics-u-sql-get-started.md)
- [I-SQL door de gebruiker gedefinieerde operatoren voor taken van de gegevens Lake analyses ontwikkelen](data-lake-analytics-u-sql-develop-user-defined-operators.md)
