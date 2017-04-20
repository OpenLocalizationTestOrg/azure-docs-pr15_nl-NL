<properties
   pageTitle="Gegevens laden uit SQL Server in Azure SQL Data Warehouse (SSIS) | Microsoft Azure"
   description="Ziet u hoe u een SQL Server Integration Services (SSIS)-pakket om gegevens van een groot aantal gegevensbronnen te SQL Data Warehouse verplaatsen maken."
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
   ms.author="lodipalm;sonyama;barbkess"/>

# <a name="load-data-from-sql-server-into-azure-sql-data-warehouse-ssis"></a>Gegevens laden uit SQL Server in Azure SQL Data Warehouse (SSIS)

> [AZURE.SELECTOR]
- [SSIS](sql-data-warehouse-load-from-sql-server-with-integration-services.md)
- [PolyBase](sql-data-warehouse-load-from-sql-server-with-polybase.md)
- [BCP](sql-data-warehouse-load-from-sql-server-with-bcp.md)


Maak een SQL Server Integration Services (SSIS)-pakket om gegevens te laden uit SQL Server in Azure SQL Data Warehouse. U kunt desgewenst herstructureren, transformeren en de gegevens opruimen als dit de SSIS-gegevensstroom passeert.

In deze zelfstudie zal u het volgende doen:

- Maak een nieuw Integration Services project in Visual Studio.
- Verbinding maken met gegevensbronnen, zoals SQL Server (als een gegevensbron) en SQL Data Warehouse (als doel).
- Ontwerp van een SSIS-pakket dat gegevens worden uit de bron in de bestemming geladen.
- Voer het SSIS-pakket om de gegevens te laden.

Deze zelfstudie gebruikt SQL Server als de gegevensbron. SQL Server kan worden uitgevoerd on-premises of in een Azure virtuele machines.

## <a name="basic-concepts"></a>Basisbegrippen

Het pakket is de eenheid van werk in de SSIS. Gerelateerde pakketten worden gegroepeerd in projecten. U maken projecten en ontwerp-pakketten in Visual Studio met SQL Server Data Tools. Het ontwerpproces is een visuele proces waarin u slepen en neerzetten van onderdelen van de Toolbox naar het ontwerpvlak, deze verbinding te maken, en hun eigenschappen instellen. Nadat u uw pakket, kunt u desgewenst deze implementeert bij SQL Server voor volledig beheer, bewaken en beveiliging.

## <a name="options-for-loading-data-with-ssis"></a>Opties voor het laden van gegevens met de SSIS

SQL Server Integration Services (SSIS) is een flexibele verzameling hulpprogramma's die een verscheidenheid aan opties voor verbinding maakt met, en het laden van gegevens in SQL Data Warehouse biedt.

1. Met de bestemming van een ADO NET verbinding maken met SQL Data Warehouse. In deze zelfstudie wordt de bestemming van een ADO NET omdat deze de minste configuratieopties heeft.
2. Met een OLE DB-bestemming verbinding maken met SQL Data Warehouse. Deze optie mogelijk iets betere prestaties dan de bestemming van de NET ADO leveren.
3. Gebruik de taak Azure Blob uploaden om de gegevens in Azure-blobopslag fase. Gebruikt u de taak SQL SSIS uitvoeren aan een script Polybase die de gegevens in SQL Data Warehouse laadt starten. Deze optie biedt de beste prestaties van de drie opties hier wordt vermeld. Als u de taak Azure Blob uploaden, het [Microsoft SQL Server 2016 Integration Services Feature Pack voor Azure][]te downloaden. Zie voor meer informatie over Polybase, [PolyBase handleiding][].

## <a name="before-you-start"></a>Voordat u begint

Als u wilt deze zelfstudie stapsgewijs, hebt u het volgende nodig:

1. **SQL Server integratieservices (SSIS)**. SSIS is een onderdeel van SQL Server en is een evaluatieversie of een gelicentieerde versie van SQL Server vereist. Als u een evaluatieversie van SQL Server-2016 Preview, raadpleegt u [SQL Server evaluaties][].
2. **Visual Studio**. Als u de gratis Visual Studio 2015 Community Edition, raadpleegt u [Visual Studio-Community][].
3. **SQL Server Data Tools voor Visual Studio (SSDT)**. Als u SQL Server Data Tools voor Visual Studio-2015, raadpleegt u [Downloaden SQL Server Data Tools (SSDT)][].
4. **Voorbeeldgegevens**. Deze zelfstudie wordt gebruikgemaakt van voorbeeldgegevens die zijn opgeslagen in SQL Server in de voorbeelddatabase AdventureWorks als de brongegevens in SQL Data Warehouse worden geladen. Als u de voorbeelddatabase AdventureWorks, Zie [AdventureWorks 2014 steekproef Databases][].
5. **A SQL Data Warehouse database en machtigingen**. Deze zelfstudie is verbonden met een exemplaar van SQL Data Warehouse en gegevens laadt in deze. U moet gemachtigd om een tabel te maken en om gegevens te laden.
6. **Een firewallregel**. U moet een firewallregel maken op SQL Data Warehouse met het IP-adres van uw lokale computer, voordat u gegevens naar de SQL-Data Warehouse uploaden kunt.

## <a name="step-1-create-a-new-integration-services-project"></a>Stap 1: Maak een nieuw Integration Services-project

1. Start Visual Studio-2015.
2. Selecteer op het menu **bestand** **nieuwe | Project**.
3. Navigeer naar de **geïnstalleerd | Sjablonen | Business Intelligence | Services voor adreslijstintegratie** projecttypen.
4. Selecteer **Integration Services Project**. Verstrek waarden voor de **naam** en **locatie**en selecteer vervolgens **OK**.

Visual Studio wordt geopend en maakt een nieuw Integration Services (SSIS)-project. Visual Studio wordt geopend de ontwerpfunctie voor het één nieuwe SSIS-pakket (Package.dtsx) in het project. U ziet het volgende schermgebieden:

- Aan de linkerkant de **werkset** van SSIS-onderdelen.
- In het midden, het ontwerpvlak, met meerdere tabbladen. Meestal gebruikt u ten minste de **Control Flow** en de **Data Flow** tabbladen.
- Aan de rechterkant, de **Oplossing Explorer** en de deelvensters **Eigenschappen** .

    ![][01]

## <a name="step-2-create-the-basic-data-flow"></a>Stap 2: De eenvoudige gegevensstroom maken

1. Sleep een taak Data Flow van de Toolbox naar het midden van het ontwerpvlak (op het tabblad **Control Flow** ).

    ![][02]

2. Dubbelklik op de Data Flow taak om te schakelen naar het tabblad Data Flow.
3. Sleep een ADO.NET-bron naar het ontwerpvlak van de lijst een andere bron in de werkset. Verander de naam naar **SQL Server-gegevensbron** in het deelvenster **Eigenschappen** met de bron-adapter nog steeds is geselecteerd.
4. Sleep een ADO.NET-bestemming naar het ontwerpvlak onder de ADO.NET-bron in de lijst andere bestemmingen in de werkset bevindt. Verander de naam op **SQL-DW bestemming** in het deelvenster **Eigenschappen** met de waarde van doel-adapter nog steeds is geselecteerd.

    ![][09]

## <a name="step-3-configure-the-source-adapter"></a>Stap 3: De bron-adapter configureren

1. Dubbelklik op de bron-adapter om de **ADO.NET-Broneditor**te openen.

    ![][03]

2. Klik op het tabblad **Connection Manager** van de **ADO.NET-Broneditor**klikt u op de knop **Nieuw** naast de lijst **ADO.NET verbinding manager** om het dialoogvenster **Configureren ADO.NET Connection Manager** openen en verbindingsinstellingen voor de SQL Server-database waaruit deze zelfstudie wordt geladen gegevens te maken.

    ![][04]

3. Klik op de knop **Nieuw** om te openen in het dialoogvenster **Connection Manager** en maak een nieuwe gegevensverbinding in het dialoogvenster **Configureren ADO.NET Connection Manager** .

    ![][05]

4. Klik in het dialoogvenster **Connection Manager** het volgende doen.

    1. Selecteer de SqlClient-gegevensprovider voor **Provider**.
    2. Voer de naam van de SQL Server voor **servernaam**.
    3. Selecteer in de sectie **Meld u aan bij de server** of verificatiegegevens invoeren.
    4. Selecteer de voorbeelddatabase AdventureWorks in de sectie **verbinding maken met een database** .
    5. Klik op **testverbinding**.
    
        ![][06]
    
    6. Klik op **OK** om terug te keren naar het dialoogvenster **Connection Manager** in het dialoogvenster dat de resultaten van de verbindingstest-rapporten.
    7. Klik op **OK** om terug te keren naar het dialoogvenster **Configureren ADO.NET Connection Manager** in het dialoogvenster **Connection Manager** .
 
5. Klik op **OK** om terug te keren naar de **ADO.NET-Broneditor**in het dialoogvenster **Configureren ADO.NET Connection Manager** .
6. Selecteer de tabel **Sales.SalesOrderDetail** in de **ADO.NET-Broneditor**in de lijst **naam van de tabel of de weergave** .

    ![][07]

7. Klik op **voorbeeld** om te zien van de eerste 200 rijen met gegevens in de brontabel in het dialoogvenster **Queryresultaten Preview** .

    ![][08]

8. Klik op **sluiten** om terug te keren naar de **ADO.NET-Broneditor**in het dialoogvenster **Queryresultaten Preview** .
9. Klik in de **ADO.NET-Broneditor**, klikt u op **OK** om te Voltooi de configuratie van de gegevensbron.

## <a name="step-4-connect-the-source-adapter-to-the-destination-adapter"></a>Stap 4: De bron-adapter koppelen aan de waarde van doel-adapter

1. Selecteer de bron-adapter op het ontwerpoppervlak.
2. Selecteer de blauwe pijl die uit de bron-adapter breidt en sleep deze naar de bestemming editor totdat de App naar de juiste plaats.

    ![][10]

    In een normale SSIS-pakket gebruikt u een aantal andere onderdelen van de SSIS Toolbox tussen het bron- en de bestemming herstructureren, transformeren en gegevens als dit de SSIS-gegevensstroom passeert verwijderen. Als u wilt houden in dit voorbeeld zo eenvoudig mogelijk, we verbinding maakt met de bron rechtstreeks naar de bestemming.

## <a name="step-5-configure-the-destination-adapter"></a>Stap 5: De bestemming-adapter configureren

1. Dubbelklik op de bestemming adapter om de **ADO.NET bestemming Editor**te openen.

    ![][11]

2. Klik op het tabblad **Connection Manager** van de **ADO.NET bestemming Editor**op de knop **Nieuw** naast de lijst **Connection manager** om het dialoogvenster **Configureren ADO.NET Connection Manager** openen en verbindingsinstellingen voor de Data Warehouse van Azure SQL-database waarin deze zelfstudie wordt geladen gegevens te maken.
3. Klik op de knop **Nieuw** om te openen in het dialoogvenster **Connection Manager** en maak een nieuwe gegevensverbinding in het dialoogvenster **Configureren ADO.NET Connection Manager** .
4. Klik in het dialoogvenster **Connection Manager** het volgende doen.
    1. Selecteer de SqlClient-gegevensprovider voor **Provider**.
    2. Voer de naam van de SQL Data Warehouse voor **servernaam**.
    3. In de sectie **Meld u aan bij de server** Selecteer **Gebruik SQL Server-verificatie** en voer verificatie-informatie.
    4. Selecteer een bestaande SQL Data Warehouse-database in de sectie **verbinding maken met een database** .
    5. Klik op **testverbinding**.
    6. Klik op **OK** om terug te keren naar het dialoogvenster **Connection Manager** in het dialoogvenster dat de resultaten van de verbindingstest-rapporten.
    7. Klik op **OK** om terug te keren naar het dialoogvenster **Configureren ADO.NET Connection Manager** in het dialoogvenster **Connection Manager** .
5. Klik op **OK** om terug te keren naar de **ADO.NET bestemming Editor**in het dialoogvenster **Configureren ADO.NET Connection Manager** .
6. Klik op **Nieuw** naast de lijst **gebruiken een tabel of weergave** om het dialoogvenster **Tabel maken** als u wilt een nieuwe bestemming-tabel maken met een lijst met kolommen die overeenkomt met de brontabel te openen in de **ADO.NET bestemming Editor**.

    ![][12a]

7. Klik in het dialoogvenster **Tabel maken** door de volgende dingen te doen.

    1. Wijzig de naam van de doeltabel in **verkooporderdetail**.
    2. Verwijder de kolom **rowguid** . Het gegevenstype van de **unieke id** wordt niet ondersteund in SQL Data Warehouse.
    3. Het gegevenstype van de kolom **LineTotal** omzetten in **geld**. Het gegevenstype **decimaal** wordt niet ondersteund in SQL Data Warehouse. Zie voor informatie over ondersteunde gegevenstypen [CREATE TABLE (Azure SQL Data Warehouse, Parallel Data Warehouse)][].
    
        ![][12b]
    
    4. Klik op **OK** om de tabel te maken en terug te keren naar de **ADO.NET bestemming Editor**.

8. Selecteer in de **ADO.NET bestemming Editor**, het tabblad **toewijzingen** om te zien hoe de kolommen in de bron zijn toegewezen aan kolommen in de bestemming.

    ![][13]

9. Klik op **OK** om te Voltooi de configuratie van de gegevensbron.

## <a name="step-6-run-the-package-to-load-the-data"></a>Stap 6: Voer het pakket om de gegevens te laden

Het pakket uitvoeren door te klikken op de knop **Start** op de werkbalk of door een van de opties **uitvoeren** in het menu **Foutopsporing** te selecteren.

Als het pakket gestart wordt, ziet u gele draaiende wielen om aan te geven van de activiteit, evenals het aantal rijen dusverre verwerkt.

![][14]

Wanneer het pakket is voltooid, ziet u groene vinkjes om aan te geven success, evenals het totale aantal rijen met gegevens uit de bron op de bestemming geladen.

![][15]

Gefeliciteerd! U hebt SQL Server Integration Services is gebruikt om gegevens te laden in Azure SQL Data Warehouse.

## <a name="next-steps"></a>Volgende stappen

- Meer informatie over de SSIS-gegevensstroom. Begin hier: [Data Flow][].
- Leer hoe u fouten opsporen en oplossen van uw pakketten rechtstreeks in de ontwerpomgeving. Begin hier: [Hulpprogramma's voor probleemoplossing voor pakket ontwikkeling][].
- Leer hoe u het SSIS-projecten en pakketten implementeren naar Integration Services-Server of naar een andere opslaglocatie. Begin hier: [implementatie van projecten en -pakketten][].

<!-- Image references -->
[01]:  ./media/sql-data-warehouse-load-from-sql-server-with-integration-services/ssis-designer-01.png
[02]:  ./media/sql-data-warehouse-load-from-sql-server-with-integration-services/ssis-data-flow-task-02.png
[03]:  ./media/sql-data-warehouse-load-from-sql-server-with-integration-services/ado-net-source-03.png
[04]:  ./media/sql-data-warehouse-load-from-sql-server-with-integration-services/ado-net-connection-manager-04.png
[05]:  ./media/sql-data-warehouse-load-from-sql-server-with-integration-services/ado-net-connection-05.png
[06]:  ./media/sql-data-warehouse-load-from-sql-server-with-integration-services/test-connection-06.png
[07]:  ./media/sql-data-warehouse-load-from-sql-server-with-integration-services/ado-net-source-07.png
[08]:  ./media/sql-data-warehouse-load-from-sql-server-with-integration-services/preview-data-08.png
[09]:  ./media/sql-data-warehouse-load-from-sql-server-with-integration-services/source-destination-09.png
[10]:  ./media/sql-data-warehouse-load-from-sql-server-with-integration-services/connect-source-destination-10.png
[11]:  ./media/sql-data-warehouse-load-from-sql-server-with-integration-services/ado-net-destination-11.png
[12a]: ./media/sql-data-warehouse-load-from-sql-server-with-integration-services/destination-query-before-12a.png
[12b]: ./media/sql-data-warehouse-load-from-sql-server-with-integration-services/destination-query-after-12b.png
[13]:  ./media/sql-data-warehouse-load-from-sql-server-with-integration-services/column-mapping-13.png
[14]:  ./media/sql-data-warehouse-load-from-sql-server-with-integration-services/package-running-14.png
[15]:  ./media/sql-data-warehouse-load-from-sql-server-with-integration-services/package-success-15.png

<!-- Article references -->

<!-- MSDN references -->
[PolyBase handleiding]: https://msdn.microsoft.com/library/mt143171.aspx
[Downloaden van SQL Server Data Tools (SSDT)]: https://msdn.microsoft.com/library/mt204009.aspx
[TABEL (Azure SQL datawarehouse, Parallel datawarehouse) maken]: https://msdn.microsoft.com/library/mt203953.aspx
[Gegevensstroom]: https://msdn.microsoft.com/library/ms140080.aspx
[Hulpprogramma's voor probleemoplossing voor de ontwikkeling van een pakket]: https://msdn.microsoft.com/library/ms137625.aspx
[Implementatie van projecten en -pakketten]: https://msdn.microsoft.com/library/hh213290.aspx

<!--Other Web references-->
[Microsoft SQL Server 2016 integratieservices-functiepakket voor Azure]: http://go.microsoft.com/fwlink/?LinkID=626967
[SQL Server evaluaties]: https://www.microsoft.com/en-us/evalcenter/evaluate-sql-server-2016
[Visual Studio-Community]: https://www.visualstudio.com/en-us/products/visual-studio-community-vs.aspx
[AdventureWorks 2014 steekproef Databases]: https://msftdbprodsamples.codeplex.com/releases/view/125550
