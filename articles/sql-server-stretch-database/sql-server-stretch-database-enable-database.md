<properties
    pageTitle="Uitrekken Database inschakelen voor een database | Microsoft Azure"
    description="Leer hoe u een database configureren voor uitrekken Database."
    services="sql-server-stretch-database"
    documentationCenter=""
    authors="douglaslMS"
    manager="jhubbard"
    editor=""/>

<tags
    ms.service="sql-server-stretch-database"
    ms.workload="data-management"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/05/2016"
    ms.author="douglasl"/>

# <a name="enable-stretch-database-for-a-database"></a>Uitrekken Database inschakelen voor een database

Een bestaande database configureren voor de Database uitrekken, selecteert u **taken | Uitrekken | Inschakelen** voor een database in SQL Server Management Studio om de wizard **Database inschakelen voor uitrekken** te openen. U kunt ook Transact\-SQL-Database uitrekken inschakelen voor een database.

Als u **taken selecteert | Uitrekken | Inschakelen** voor een afzonderlijke tabel en u nog niet is ingeschakeld de database voor uitrekken Database, de wizard voor het configureren van de database voor uitrekken Database en laat u tabellen selecteren als onderdeel van het proces. Volg de stappen in dit onderwerp in plaats van de stappen in de [Database in het uitrekken inschakelen voor een tabel](sql-server-stretch-database-enable-database.md).

Db uitrekken Database inschakelt voor een database of een tabel vereist\_eigenaarsmachtigingen. Uitrekken Database inschakelt voor een database, zijn ook BESTURINGSELEMENT DATABASE machtigingen nodig.

 >   [AZURE.NOTE] Als u de Database uitrekken uitschakelt, moet u later onthouden dat uitrekken Database uitschakelen voor een tabel of voor een database niet het externe object verwijdert. Als u verwijderen van de externe tabel of de externe database wilt, hebt u zet deze neer met behulp van de Azure beheerportal. De externe objecten blijven Azure kosten totdat u deze handmatig verwijderen.

## <a name="before-you-get-started"></a>Voordat u begint

-   Voordat u een database voor uitrekken configureren, wordt u aangeraden dat u de uitrekken Database Advisor om te identificeren databases en tabellen die in aanmerking voor uitrekken komen die worden uitgevoerd. De uitrekken Database Advisor kunt u ook blokkeren problemen identificeren. Zie [identificeren databases en tabellen voor uitrekken Database](sql-server-stretch-database-identify-databases.md)voor meer informatie.

-   Bekijk de [beperkingen voor uitrekken Database](sql-server-stretch-database-limitations.md).

-   Gegevens overzet uitrekken Database naar Azure. U moet dus hebben een Azure-account en een abonnement voor facturering. Om een Azure-account, [klikt u hier](http://azure.microsoft.com/pricing/free-trial/).

-   De gegevens van de verbinding en meld u een nieuwe Azure server maken of selecteren van een bestaande Azure server nodig hebt.

## <a name="EnableTSQLServer"></a>Let op: Uitrekken Database op de server inschakelen
Voordat u uitrekken Database op een database of een tabel inschakelen kunt, moet u deze op de lokale server inschakelen. Deze bewerking moet de systeembeheerder of pas machtigingen.

-   Als u de vereiste beheerdersmachtigingen hebt, configureert met de wizard **Database inschakelen voor uitrekken** de server voor uitrekken.

-   Als u niet de vereiste machtigingen hebt, een beheerder moet de optie handmatig inschakelen door te voeren **sp\_configureren** voordat u de wizard uitvoeren, of een beheerder heeft de wizard uit te voeren.

Als u handmatig uitrekken Database op de server, voert u **sp\_configureren** en schakel de optie **externe gegevens archiveren** . Het volgende voorbeeld wordt de optie **externe gegevens archiveren** door de waarde 1.

```
EXEC sp_configure 'remote data archive' , '1';
GO

RECONFIGURE;
GO
```
Zie [configureren de externe gegevens archiveren Server configuratieoptie](https://msdn.microsoft.com/library/mt143175.aspx) en [sp_configure (Transact-SQL)](https://msdn.microsoft.com/library/ms188787.aspx)voor meer informatie.

## <a name="Wizard"></a>Gebruik de wizard uitrekken Database inschakelen voor een database
Voor informatie over de Database inschakelen voor de Wizard uitrekken, inclusief de informatie die u hebt om in te voeren en de opties die u moet maken, raadpleegt u [aan de slag door de Database inschakelen voor uitrekken Wizard uit te voeren](sql-server-stretch-database-wizard.md).

## <a name="EnableTSQLDatabase"></a>Gebruik Transact\-SQL-Database uitrekken inschakelen voor een database
Voordat u uitrekken Database op afzonderlijke tabellen inschakelen kunt, moet u deze inschakelen voor de database.

Db uitrekken Database inschakelt voor een database of een tabel vereist\_eigenaarsmachtigingen. Uitrekken Database inschakelt voor een database, zijn ook BESTURINGSELEMENT DATABASE machtigingen nodig.

1.  Voordat u begint, kiest u een bestaande Azure server voor de gegevens die uitrekken Database migreert of maak een nieuwe Azure server.

2.  Klik op de server Azure door een firewallregel te maken met het bereik van de IP-adres van de SQL-Server waarmee SQL Server communiceren met de externe server.

    U kunt de waarden die u nodig hebt en de firewallregel maken door te proberen verbinding maken met de server Azure van Object Explorer in SQL Server Management Studio (SSMS) terugvinden. SSMS helpt u bij het maken van de regel door het volgende dialoogvenster dat al de vereiste waarden voor IP-adres bevat te openen.

    ![Een firewallregel maken in SSMS][FirewallRule]

3.  Voor meer informatie over het configureren van een SQL Server-database voor uitrekken Database wordt de database heeft tot de database outmodel toets. De database outmodel toets beveiligt de referenties die uitrekken Database verbinding maakt met de externe database. Hier volgt een voorbeeld die een nieuwe database outmodel-sleutel gemaakt.

    ```tsql
    USE <database>;
    GO

    CREATE MASTER KEY ENCRYPTION BY PASSWORD ='<password>';
    GO
    ```

    Zie [OUTMODEL sleutel maken (Transact-SQL)](https://msdn.microsoft.com/library/ms174382.aspx) en [Database outmodel sleutel maken](https://msdn.microsoft.com/library/aa337551.aspx)voor meer informatie over de database outmodel-toets.

4.  Wanneer u een database voor uitrekken Database configureren, die u moet een referentie voor uitrekken Database wilt gebruiken voor communicatie tussen de aan lokale SQL Server en de externe Azure-server opgeven. U hebt twee opties.

    -   U kunt een referentie beheerder opgeeft.

        -   Als u de Database uitrekken inschakelt door de wizard uit te voeren, kunt u de referentie op dat moment.

        -   Als u van plan bent om in te schakelen uitrekken Database door **DATABASE wijzigen**uit te voeren, die u moet de referentie handmatig voordat u de **DATABASE wijzigen** om in te schakelen uitrekken Database maken.

        Hier volgt een voorbeeld die Hiermee maakt u een nieuwe referentie.

        ```tsql
        CREATE DATABASE SCOPED CREDENTIAL <db_scoped_credential_name>
            WITH IDENTITY = '<identity>' , SECRET = '<secret>';
        GO
        ```

        Zie voor meer informatie over de referentie [Maken DATABASE beperkt referentie (Transact-SQL)](https://msdn.microsoft.com/library/mt270260.aspx). De referentie maken, moet u machtigingen voor een referentie wijzigen.

    -   Met de externe Azure server te communiceren wanneer de volgende voorwaarden alle waar zijn kunt u een federatieve serviceaccount voor de SQL-Server.

        -   De serviceaccount waarmee het exemplaar van SQL Server wordt uitgevoerd, is een domeinaccount.

        -   Het domeinaccount deel uitmaakt van een domein waarvan Active Directory wordt gekoppeld aan Azure Active Directory.

        -   De externe Azure server is geconfigureerd voor het ondersteunen van Azure Active Directory-verificatie.

        -   De serviceaccount waarmee het exemplaar van SQL Server wordt uitgevoerd is als een dbmanager of systeembeheerder account op de externe Azure server geconfigureerd.

5.  Als u wilt een database configureren voor uitrekken Database, voert u de opdracht DATABASE wijzigen.

    1.  Voor het argument SERVER, geef de naam van een bestaande Azure-server, inclusief de `.database.windows.net` deel van de naam \- bijvoorbeeld `MyStretchDatabaseServer.database.windows.net`.

    2.  Een bestaande beheerder referentie voorzien van het argument referentie of opgeven federatieve\_SERVICE\_ACCOUNT = ON. Het volgende voorbeeld wordt een bestaande referentie.

    ```tsql
    ALTER DATABASE <database name>
        SET REMOTE_DATA_ARCHIVE = ON
            (
                SERVER = '<server_name>',
                CREDENTIAL = <db_scoped_credential_name>
            ) ;
    GO
    ```

## <a name="next-steps"></a>Volgende stappen
-   [Uitrekken Database inschakelen voor een tabel](sql-server-stretch-database-enable-table.md) voor het inschakelen van extra tabellen.

-   [Monitor uitrekken Database](sql-server-stretch-database-monitor.md) om de status van de gegevensmigratie weer te geven.

-   [Onderbreken en hervatten uitrekken Database](sql-server-stretch-database-pause.md)

-   [Beheren en problemen met uitrekken Database](sql-server-stretch-database-manage.md)

-   [Back-up uitrekken ingeschakelde databases](sql-server-stretch-database-backup.md)

## <a name="see-also"></a>Zie ook

[Databases en tabellen voor uitrekken Database identificeren](sql-server-stretch-database-identify-databases.md)

[ALTER opties voor het instellen van DATABASE (Transact-SQL)](https://msdn.microsoft.com/library/bb522682.aspx)

[FirewallRule]: ./media/sql-server-stretch-database-enable-database/firewall.png
