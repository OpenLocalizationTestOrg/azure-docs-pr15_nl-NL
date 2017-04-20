<properties
   pageTitle="Handleiding voor het gebruik van PolyBase in SQL Data Warehouse | Microsoft Azure"
   description="Richtlijnen en aanbevelingen voor het gebruik van PolyBase in SQL Data Warehouse scenario's."
   services="sql-data-warehouse"
   documentationCenter="NA"
   authors="ckarst"
   manager="barbkess"
   editor=""/>

<tags
   ms.service="sql-data-warehouse"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-services"
   ms.date="06/30/2016"
   ms.author="cakarst;barbkess;sonyama"/>


# <a name="guide-for-using-polybase-in-sql-data-warehouse"></a>Handleiding voor het gebruik van PolyBase in SQL Data Warehouse

Deze handleiding biedt praktische informatie voor het gebruik van PolyBase in SQL Data Warehouse.

Als u wilt beginnen, raadpleegt u de zelfstudie [laden gegevens met PolyBase][] .


## <a name="rotating-storage-keys"></a>Opslag toetsen draaien

U wilt te bezoeken de access-sleutel wijzigen met uw blob storage vanwege de beveiliging.

De meest elegante manier voor deze taak is om te volgen dit proces 'draaien de toetsen' genoemd. U misschien opgevallen dat er twee opslag sleutels voor uw blob storage-account. Dit is zodat u kunt overstappen

Uw Azure opslag account sleutels draaien is een eenvoudige drie stappen

1. Tweede database beperkt referentie op basis van de secundaire opslag toegangstoets maken
2. Tweede externe gegevensbron vooraf worden gebaseerd op deze nieuwe referentie maken
3. Sleep en de externe tabel(len) die wijst naar de nieuwe externe gegevensbron maken

Wanneer u bent overgestapt alle externe tabellen met de nieuwe externe gegevensbron en u kunt het opschonen taken uitvoeren:

1. Decoratieve eerste gegevens uit externe bron
2. De eerste database decoratieve beperkt op basis van de primaire opslag toegangstoets referentie
3. Meld u aan bij Azure en wilt u de volgende keer dat de primaire toegangstoets genereren

## <a name="query-azure-blob-storage-data"></a>Query Azure blob storage-gegevens
Query's op externe tabellen gebruiken de tabelnaam alsof deze een relationele tabel is.

```sql
-- Query Azure storage resident data via external table.
SELECT * FROM [ext].[CarSensor_Data]
;
```

> [AZURE.NOTE] Een query op een externe tabel kan mislukken met de fout *'Query afgebroken--het maximale aantal negeren drempel is bereikt bij het lezen van een externe bron'*. Hiermee wordt aangegeven dat uw externe gegevens *dirty* records bevat. Een record wordt beschouwd als 'dirty' als het werkelijke gegevens typen/aantal kolommen niet overeenkomen met de kolomdefinities van de externe tabel of als de gegevens niet aan de opgegeven externe-bestandsindeling voldoet. Zorg ervoor dat uw externe tabel en de definities voor het opmaken van externe bestand juist zijn en uw externe gegevens aan deze definities voldoen u lost dit probleem. Als een subset van externe gegevensrecords zijn gewijzigd, kunt u deze records voor uw query's met behulp van de opties negeren in externe tabel DDL maken negeren.


## <a name="load-data-from-azure-blob-storage"></a>Gegevens van Azure-blobopslag laden
In dit voorbeeld laadt gegevens van Azure-blobopslag met SQL Data Warehouse-database.

Gegevens rechtstreeks opslaat, wordt de tijd voor het doorverbinden van gegevens voor query's verwijderd. Gegevens met een index columnstore opslaat, wordt de prestaties van query's voor gegevensanalyse query's per maximaal 10 x verbeterd.

In dit voorbeeld wordt de instructie CREATE tabel AS SELECT gebruikt om gegevens te laden. De nieuwe tabel krijgt de kolommen met de naam in de query. Deze overneemt de gegevenstypen van deze kolommen van de definitie van de externe tabel.

CREATE tabel AS SELECT is een zeer zodat Transact-SQL-instructie die de gegevens op alle berekeningscluster knooppunten van uw SQL Data Warehouse parallel worden geladen.  Deze oorspronkelijk is ontwikkeld voor de massively parallelle verwerking (MPP)-engine in analysesysteem Platform en bevindt zich nu in SQL Data Warehouse.

```sql
-- Load data from Azure blob storage to SQL Data Warehouse

CREATE TABLE [dbo].[Customer_Speed]
WITH
(   
    CLUSTERED COLUMNSTORE INDEX
,   DISTRIBUTION = HASH([CarSensor_Data].[CustomerKey])
)
AS
SELECT *
FROM   [ext].[CarSensor_Data]
;
```

Zie [maken tabel AS SELECT (Transact-SQL)][].

## <a name="create-statistics-on-newly-loaded-data"></a>Statistieken maken voor nieuwe geladen gegevens

Azure SQL Data Warehouse biedt nog geen ondersteuning auto maken of bijwerken statistieken jaarlijks automatisch.  Om te krijgen de beste prestaties van uw query's, het is belangrijk dat statistieken worden gemaakt voor alle kolommen van alle tabellen na de eerste laden of belangrijke wijzigingen optreden in de gegevens.  Zie het onderwerp [Statistieken][] in de groep ontwikkelen van onderwerpen voor een gedetailleerde uitleg van statistieken.  Hierna ziet u een snel voorbeeld van het maken van statistieken over de tabellen bevatten geladen in dit voorbeeld is.

```sql
create statistics [SensorKey] on [Customer_Speed] ([SensorKey]);
create statistics [CustomerKey] on [Customer_Speed] ([CustomerKey]);
create statistics [GeographyKey] on [Customer_Speed] ([GeographyKey]);
create statistics [Speed] on [Customer_Speed] ([Speed]);
create statistics [YearMeasured] on [Customer_Speed] ([YearMeasured]);
```

## <a name="export-data-to-azure-blob-storage"></a>Gegevens exporteren naar Azure-blobopslag
In dit gedeelte ziet hoe u gegevens exporteren vanuit SQL Data Warehouse met Azure-blobopslag. Dit voorbeeld wordt CREATE externe tabel AS SELECT die is een zeer zodat de gegevens parallel exporteren vanuit alle berekeningscluster knooppunten Transact-SQL-instructie.

Het volgende voorbeeld wordt een externe tabel Weblogs2014 met kolomdefinities van de en gegevens uit dbo. Weblogs tabel. De tabeldefinitie van de externe wordt opgeslagen in SQL Data Warehouse en de resultaten van de SELECT-instructie worden geëxporteerd naar de map '/ archiveren/log2014 /' onder de container blob is opgegeven door de gegevensbron. De gegevens worden geëxporteerd in de bestandsindeling van de opgegeven tekst.

```sql
CREATE EXTERNAL TABLE Weblogs2014 WITH
(
    LOCATION='/archive/log2014/',
    DATA_SOURCE=azure_storage,
    FILE_FORMAT=text_file_format
)
AS
SELECT
    Uri,
    DateRequested
FROM
    dbo.Weblogs
WHERE
    1=1
    AND DateRequested > '12/31/2013'
    AND DateRequested < '01/01/2015';
```


## <a name="working-around-the-polybase-utf-8-requirement"></a>Werken rond de vereiste PolyBase UTF-8
Ondersteunt het laden van gegevensbestanden die UTF-8 zijn-codering op presenteren PolyBase. Als u gebruikmaakt van UTF-8 wordt het ook ondersteuning laden van gegevens die ASCII-codering is dezelfde tekencodering als ASCII-PolyBase. Echter PolyBase biedt geen ondersteuning voor andere tekencodering zoals UTF-16 / Unicode of uitgebreide ASCII-tekens. Uitgebreide ASCII bevat tekens met accenten zoals de umlaut die veel gebruikt in het Duits wordt.

U kunt deze vereiste omzeilen is het beste antwoord te schrijven opnieuw UTF-8-codering.

Er zijn verschillende manieren u dit wilt doen. Hieronder ziet u twee methoden via Powershell:

### <a name="simple-example-for-small-files"></a>Eenvoudig voorbeeld voor kleine bestanden

Hieronder ziet u een eenvoudige één regel Powershell-script die Hiermee maakt u het bestand.

```PowerShell
Get-Content <input_file_name> -Encoding Unicode | Set-Content <output_file_name> -Encoding utf8
```

Terwijl dit een eenvoudige manier is om de gegevens opnieuw te coderen is het echter in geen geval de efficiëntste. De io streaming voorbeeld hieronder lijkt veel, veel sneller en heeft hetzelfde resultaat.

### <a name="io-streaming-example-for-larger-files"></a>Voorbeeld IO Streaming voor grotere bestanden

In het onderstaande voorbeeld is complexer maar omdat deze de rijen met gegevens uit de bron te activeren streamt tot stand is veel efficiënter. Gebruik deze methode voor grotere bestanden.

```PowerShell
#Static variables
$ascii = [System.Text.Encoding]::ASCII
$utf16le = [System.Text.Encoding]::Unicode
$utf8 = [System.Text.Encoding]::UTF8
$ansi = [System.Text.Encoding]::Default
$append = $False

#Set source file path and file name
$src = [System.IO.Path]::Combine("C:\input_file_path\","input_file_name.txt")

#Set source file encoding (using list above)
$src_enc = $ansi

#Set target file path and file name
$tgt = [System.IO.Path]::Combine("C:\output_file_path\","output_file_name.txt")

#Set target file encoding (using list above)
$tgt_enc = $utf8

$read = New-Object System.IO.StreamReader($src,$src_enc)
$write = New-Object System.IO.StreamWriter($tgt,$append,$tgt_enc)

while ($read.Peek() -ne -1)
{
    $line = $read.ReadLine();
    $write.WriteLine($line);
}
$read.Close()
$read.Dispose()
$write.Close()
$write.Dispose()
```

## <a name="next-steps"></a>Volgende stappen
Zie voor meer informatie over het verplaatsen van gegevens naar SQL Data Warehouse, het [overzicht van de migratie data][].

<!--Image references-->

<!--Article references-->
[Load data with bcp]: ./sql-data-warehouse-load-with-bcp.md
[Gegevens met PolyBase laden]: ./sql-data-warehouse-get-started-load-with-polybase.md
[Statistieken]: ./sql-data-warehouse-tables-statistics.md
[overzicht van de migratie Data]: ./sql-data-warehouse-overview-migrate.md

<!--MSDN references-->
[supported source/sink]: https://msdn.microsoft.com/library/dn894007.aspx
[copy activity]: https://msdn.microsoft.com/library/dn835035.aspx
[SQL Server destination adapter]: https://msdn.microsoft.com/library/ms141095.aspx
[SSIS]: https://msdn.microsoft.com/library/ms141026.aspx

[CREATE EXTERNAL DATA SOURCE (Transact-SQL)]: https://msdn.microsoft.com/library/dn935022.aspx
[Maken externe BESTANDSINDELING (Transact-SQL)]: https://msdn.microsoft.com/library/dn935026) .aspx [maken externe tabel (Transact-SQL)]: https://msdn.microsoft.com/library/dn935021.aspxx

[DROP EXTERNAL DATA SOURCE (Transact-SQL)]: https://msdn.microsoft.com/library/mt146367.aspx
[DROP EXTERNAL FILE FORMAT (Transact-SQL)]: https://msdn.microsoft.com/library/mt146379.aspx
[DROP EXTERNAL TABLE (Transact-SQL)]: https://msdn.microsoft.com/library/mt130698.aspx

[TABEL als selecteren (Transact-SQL) maken]: https://msdn.microsoft.com/library/mt204041.aspx
[INSERT...SELECT (Transact-SQL)]: https://msdn.microsoft.com/library/ms174335.aspx
[CREATE MASTER KEY (Transact-SQL)]: https://msdn.microsoft.com/library/ms174382.aspx
[CREATE CREDENTIAL (Transact-SQL)]: https://msdn.microsoft.com/library/ms189522.aspx
[CREATE DATABASE SCOPED CREDENTIAL (Transact-SQL)]: https://msdn.microsoft.com/library/mt270260.aspx
[DROP CREDENTIAL (Transact-SQL)]: https://msdn.microsoft.com/library/ms189450.aspx

<!-- External Links -->
