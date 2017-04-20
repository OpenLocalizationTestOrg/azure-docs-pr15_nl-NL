<properties
   pageTitle="Verbinding maken met SQL Azure datawarehouse | Microsoft Azure"
   description="Hoe u vindt de server en de naam van de verbindingstekenreeks voor uw naar Azure SQL Data Warehouse"
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
   ms.date="09/26/2016"
   ms.author="sonyama;barbkess"/>

# <a name="connect-to-azure-sql-data-warehouse"></a>Verbinding maken met SQL Azure datawarehouse

In dit artikel vindt u verbinding maken met SQL Data Warehouse voor de eerste keer.

## <a name="find-your-server-name"></a>De servernaam van de zoeken

De eerste stap bij het verbinding maken met SQL Data Warehouse is weten hoe u het vinden van de naam van de server.  De naam van de server in het volgende voorbeeld is bijvoorbeeld sample.database.windows.net. De volledig gekwalificeerde servernaam zoeken:

1. Ga naar de [Azure-portal][].
2. Klik op **SQL-databases** 
3. Klik op de database die u verbinding wilt maken.
4. Zoek de naam van de volledige server.

    ![De naam van de volledige server][1]

## <a name="supported-drivers-and-connection-strings"></a>Ondersteunde stuurprogramma's en verbindingstekenreeksen

Azure SQL Data Warehouse ondersteunt [ADO.NET][], [ODBC][] [PHP][]en [JDBC][]. Klik op een van de voorgaande stuurprogramma's zoeken naar de meest recente versie en documentatie. Als u wilt Genereer automatisch de verbindingsreeks voor het stuurprogramma dat u gebruikt van de Azure-portal, kunt u klikken op de **database-verbindingstekenreeksen weergeven** uit het voorgaande voorbeeld.  Volgende zijn ook enkele voorbeelden van hoe een verbindingsreeks voor elk stuurprogramma eruitziet.

> [AZURE.NOTE] Houd rekening met de time-out van de verbinding wordt ingesteld op 300 seconden toe te staan dat de verbinding korte perioden niet beschikbaar worden bewaard.

### <a name="adonet-connection-string-example"></a>ADO.NET verbinding tekenreeks voorbeeld

```C#
Server=tcp:{your_server}.database.windows.net,1433;Database={your_database};User ID={your_user_name};Password={your_password_here};Encrypt=True;TrustServerCertificate=False;Connection Timeout=30;
```

### <a name="odbc-connection-string-example"></a>ODBC-verbinding tekenreeks voorbeeld

```C#
Driver={SQL Server Native Client 11.0};Server=tcp:{your_server}.database.windows.net,1433;Database={your_database};Uid={your_user_name};Pwd={your_password_here};Encrypt=yes;TrustServerCertificate=no;Connection Timeout=30;
```

### <a name="php-connection-string-example"></a>Voorbeeld van PHP verbinding

```PHP
Server: {your_server}.database.windows.net,1433 \r\nSQL Database: {your_database}\r\nUser Name: {your_user_name}\r\n\r\nPHP Data Objects(PDO) Sample Code:\r\n\r\ntry {\r\n   $conn = new PDO ( \"sqlsrv:server = tcp:{your_server}.database.windows.net,1433; Database = {your_database}\", \"{your_user_name}\", \"{your_password_here}\");\r\n    $conn->setAttribute( PDO::ATTR_ERRMODE, PDO::ERRMODE_EXCEPTION );\r\n}\r\ncatch ( PDOException $e ) {\r\n   print( \"Error connecting to SQL Server.\" );\r\n   die(print_r($e));\r\n}\r\n\rSQL Server Extension Sample Code:\r\n\r\n$connectionInfo = array(\"UID\" => \"{your_user_name}\", \"pwd\" => \"{your_password_here}\", \"Database\" => \"{your_database}\", \"LoginTimeout\" => 30, \"Encrypt\" => 1, \"TrustServerCertificate\" => 0);\r\n$serverName = \"tcp:{your_server}.database.windows.net,1433\";\r\n$conn = sqlsrv_connect($serverName, $connectionInfo);
```

### <a name="jdbc-connection-string-example"></a>Voorbeeld van JDBC verbinding

```Java
jdbc:sqlserver://yourserver.database.windows.net:1433;database=yourdatabase;user={your_user_name};password={your_password_here};encrypt=true;trustServerCertificate=false;hostNameInCertificate=*.database.windows.net;loginTimeout=30;
```

## <a name="connection-settings"></a>Verbindingsinstellingen

SQL Data Warehouse gestandaardiseerd enkele instellingen die tijdens de verbinding en objecten maken. Deze instellingen kunnen niet worden overschreven en onder andere:

| Database-instelling       | Waarde                        |
| :--------------------- | :--------------------------- |
| [ANSI_NULLS][]         | AAN                           |
| [QUOTED_IDENTIFIERS][] | AAN                           |
| [DATUMNOTATIE][]         | MDJ                          |
| [DATEFIRST][]          | 7                            |

## <a name="next-steps"></a>Volgende stappen

Als u wilt koppelen en query met Visual Studio, de [Query met Visual Studio][]weer te geven. Zie voor meer informatie over de verificatieopties voor, [verificatie voor Azure SQL Data Warehouse][].

<!--Articles-->
[Query met Visual Studio]: ./sql-data-warehouse-query-visual-studio.md
[Verificatie van Azure SQL datawarehouse]: ./sql-data-warehouse-authentication.md

<!--MSDN references-->
[ADO.NET]: https://msdn.microsoft.com/library/e80y5yhx(v=vs.110).aspx
[ODBC]: https://msdn.microsoft.com/library/jj730314.aspx
[PHP]: https://msdn.microsoft.com/library/cc296172.aspx?f=255&MSPPError=-2147217396
[JDBC]: https://msdn.microsoft.com/library/mt484311(v=sql.110).aspx
[ANSI_NULLS]: https://msdn.microsoft.com/library/ms188048.aspx
[QUOTED_IDENTIFIERS]: https://msdn.microsoft.com/library/ms174393.aspx
[DATUMNOTATIE]: https://msdn.microsoft.com/library/ms189491.aspx
[DATEFIRST]: https://msdn.microsoft.com/library/ms181598.aspx

<!--Other-->
[Azure-portal]: https://portal.azure.com

<!--Image references-->
[1]: media/sql-data-warehouse-connect-overview/get-server-name.png


