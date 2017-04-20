 <properties
    pageTitle="Het gebruik van batchen Azure SQL Database-toepassingsprestaties te verbeteren"
    description="Het onderwerp bevat bewijs die batchen databasebewerkingen enorm imroves de snelheid en schaalbaarheid van uw Azure SQL Database-toepassingen. Hoewel deze batchen technieken voor een SQL Server-database werken, wordt de nadruk in dit artikel ligt op Azure."
    services="sql-database"
    documentationCenter="na"
    authors="annemill"
    manager="jhubbard"
    editor="" />


<tags
    ms.service="sql-database"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="data-management"
    ms.date="07/12/2016"
    ms.author="annemill" />

# <a name="how-to-use-batching-to-improve-sql-database-application-performance"></a>Het gebruik van batchen SQL-Database toepassingsprestaties te verbeteren

Bewerkingen met Azure SQL-Database aanzienlijk batchen verbetert de prestaties en schaalbaarheid van uw toepassingen. Als u wilt weten over de voordelen, behandelt het eerste deel van dit artikel enkele steekproef testresultaten die opeenvolgende en batch aanvragen voor een SQL-Database vergelijken. De rest van het artikel ziet u de technieken, scenario's en overwegingen bij het succes batchen in uw Azure-toepassingen gebruiken.

**Auteurs**: Jason Roth, Silvano Coriani, Theo Swanson (volledige schaal 180 Inc.)

**Revisoren**: Conor Cunningham, Michael Thomassy

## <a name="why-is-batching-important-for-sql-database"></a>Waarom is batchen belangrijke voor SQL-Database?
Batchen oproepen naar een externe service is een bekende strategie voor het vergroten van prestaties en schaalbaarheid. Er worden opgelost verwerking kosten aan een interacties met een externe service, zoals serialisatie, netwerk doorverbinden en deserialisatie. Groot aantal afzonderlijke transacties in één batch, wordt deze kosten geminimaliseerd.

In dit artikel, die we wilt onderzoeken welke gevolgen verschillende SQL-Database batchen strategieën en scenario's. Hoewel deze strategieën ook belang voor on-premises implementatie-toepassingen die gebruikmaken van SQL Server zijn, zijn er verschillende redenen voor het markeren van het gebruik van batchen voor SQL-Database:

- Is er mogelijk groter netwerklatentie bij het openen van de SQL-Database, vooral als u toegang krijgt SQL-Database van buiten het dezelfde Microsoft Azure-datacenter tot.
- De multitenant kenmerken van de SQL-Database betekent dat de efficiëntie van de gegevens access laag is afgestemd op de algehele schaalbaarheid van de database. SQL-Database moeten alle één tenant/gebruikers belasten database bronnen die u wilt het nadeel van andere tenants voorkomen. SQL-Database kunt reactie op gebruik boven vooraf gedefinieerde quota, doorvoer verkleinen of beantwoorden met uitzonderingen beperken. Efficiëntie, zoals batchen, kunnen u meer werkzaamheden voor SQL-Database alvorens deze limieten uitvoeren. 
- Batchen werkt ook voor architecturen die meerdere databases (sharding) gebruiken. De efficiëntie van uw interactie met per eenheid van de database is nog steeds een belangrijke rol bij uw algehele schaalbaarheid. 

Een van de voordelen van het gebruik van de SQL-Database is geen hebt voor het beheren van de servers die als host van de database. Echter betekent deze beheerde infrastructuur ook dat u ter overweging anders database optimalisaties toe. Ter verbetering van de database-infrastructuur voor hardware of netwerk kunt u niet langer zoeken. Microsoft Azure Hiermee stelt u deze omgevingen. De belangrijkste gebied dat u kunt bepalen is hoe uw toepassing moet samenwerken met SQL-Database. Batchen is een van deze optimalisaties. 

Het eerste deel van het papier worden onderzocht, batchen bladwijzers voor .NET-toepassingen die gebruikmaken van de SQL-Database. De laatste twee secties duidelijk batchen richtlijnen en scenario's.

## <a name="batching-strategies"></a>Strategieën batchen

### <a name="note-about-timing-results-in-this-topic"></a>Houd er rekening mee over tijdsinstellingen resultaten in dit onderwerp
>[AZURE.NOTE] Resultaten zijn niet benchmarks, maar zijn bedoeld om weer te geven **relatieve prestaties**. Tijdsinstellingen op basis van een gemiddelde van ten minste 10 test wordt uitgevoerd. Bewerkingen zijn wordt ingevoegd in een lege tabel. Deze tests zijn gemeten pre-V12, en ze niet per se komen overeen met doorvoer die mogelijk optreden in een V12-database met de nieuwe [Servicelagen](sql-database-service-tiers.md). De relatieve voordeel van de batchen techniek zou hetzelfde moeten zijn.

### <a name="transactions"></a>Transacties
Het lijkt vreemd om te beginnen met een beoordeling van batchen door transacties bespreekt. Maar het gebruik van aan de clientzijde transacties heeft een subtiel batchen effect voor servers, dat de prestaties verbetert. En transacties kunnen worden toegevoegd met slechts een paar regels met code, zodat ze bieden een snelle manier om de prestaties van opeenvolgende bewerkingen te verbeteren.

Houd rekening met de volgende C#-code met een reeks invoegen en bewerkingen op een eenvoudige tabel bijwerken.

    List<string> dbOperations = new List<string>();
    dbOperations.Add("update MyTable set mytext = 'updated text' where id = 1");
    dbOperations.Add("update MyTable set mytext = 'updated text' where id = 2");
    dbOperations.Add("update MyTable set mytext = 'updated text' where id = 3");
    dbOperations.Add("insert MyTable values ('new value',1)");
    dbOperations.Add("insert MyTable values ('new value',2)");
    dbOperations.Add("insert MyTable values ('new value',3)");

De volgende ADO.NET-code opeenvolging kunt u deze bewerkingen uitvoeren.

    using (SqlConnection connection = new SqlConnection(CloudConfigurationManager.GetSetting("Sql.ConnectionString")))
    {
        conn.Open();
    
        foreach(string commandString in dbOperations)
        {
            SqlCommand cmd = new SqlCommand(commandString, conn);
            cmd.ExecuteNonQuery();                   
        }
    }

De beste manier om het optimaliseren van deze code wordt bepaalde formulier van aan de clientzijde batchen van deze oproepen wordt geïmplementeerd. Maar er is een eenvoudige manier om de prestaties van deze code vergroten door gewoon de volgorde van oproepen voor tekstterugloop in een transactie. Hier ziet u dezelfde code die wordt gebruikt een transactie.

    using (SqlConnection connection = new SqlConnection(CloudConfigurationManager.GetSetting("Sql.ConnectionString")))
    {
        conn.Open();
        SqlTransaction transaction = conn.BeginTransaction();
    
        foreach (string commandString in dbOperations)
        {
            SqlCommand cmd = new SqlCommand(commandString, conn, transaction);
            cmd.ExecuteNonQuery();
        }
    
        transaction.Commit();
    }

Transacties worden werkelijk in beide in deze voorbeelden gebruikt. In het eerste voorbeeld is elke afzonderlijke gesprek een impliciete transactie. In het tweede voorbeeld terugloopt een expliciete transactie alle de oproepen. Per de documentatie van de [vooraf geschreven transactielogboek](https://msdn.microsoft.com/library/ms186259.aspx), logboekrecords naar de schijf verwijderd bij de transactie is doorgevoerd. Door verdere oproepen te nemen in een transactie, kan het schrijven naar het transactielogboek zodat dit totdat de transactie doorgevoerd is. U in feite inschakelt batchen voor het schrijven naar transactielogboek van de server.

De volgende tabel ziet u enkele ad-hoc-testresultaten. De tests uitgevoerd de opeenvolgende invoegt met en zonder transacties. Voor meer perspectief, de eerste reeks tests uitgevoerd op afstand vanuit een laptop met de database in Microsoft Azure. De tweede reeks tests hebt uitgevoerd vanuit een cloudservice en database dat beide was opgeslagen binnen het dezelfde Microsoft Azure-datacenter (West Verenigde Staten). De volgende tabel ziet u de duur in milliseconden van opeenvolgende wordt ingevoegd met en zonder transacties.

**On-Premises naar Azure**:

| Bewerkingen | Geen transactie ([ms) | Transactie ([ms) |
|---|---|---|
| 1 | 130 | 402 |
| 10 | 1208 | 1226 |
| 100 | 12662 | 10395 |
| 1000 | 128852 | 102917 |


**Azure-Azure (dezelfde datacenter)**:

| Bewerkingen | Geen transactie ([ms) | Transactie ([ms) |
|---|---|---|
| 1 | 21 | 26 |
| 10 | 220 | 56 |
| 100 | 2145 | 341 |
| 1000 | 21479 | 2756 |

>[AZURE.NOTE] Resultaten zijn niet benchmarks. Zie de [Opmerking over de timing resultaten in dit onderwerp](#note-about-timing-results-in-this-topic).

Op basis van de vorige testresultaten, één bewerking voor tekstterugloop in een transactie neemt af prestaties. Maar het aantal bewerkingen binnen een transactie te vergroten, de prestatieverbetering bij het meer wordt gemarkeerd. Het verschil prestaties is ook opvallender wanneer alle bewerkingen binnen het Microsoft Azure-datacenter plaatsvinden. De verbeterde latentie van het gebruik van de SQL-Database van buiten het Microsoft Azure-datacenter overshadows de prestatieverbetering van het gebruik van transacties.

Hoewel het gebruik van transacties kunt prestaties, gaat u verder met [Aanbevolen procedures voor transacties en verbindingen toekijken](https://msdn.microsoft.com/library/ms187484.aspx). Zorg dat de transactie zo kort mogelijk en sluit de databaseverbinding nadat het werk dat is voltooid. Het gebruik van de instructie in het vorige voorbeeld zorgt ervoor dat de verbinding is verbroken wanneer de blokkering van de volgende code is voltooid.

Het vorige voorbeeld bevat dat u een lokale transactie aan een ADO.NET-code, met twee regels toevoegen kunt. Transacties bieden een snelle manier om te verbeteren de prestaties van code waarmee u opeenvolgende invoegen, bijwerken en verwijderen van bewerkingen. Echter snelste prestaties, kunt u de code verder naar Profiteer van aan de clientzijde batchen, zoals parameters tabelwaarden te wijzigen.

Zie voor meer informatie over transacties in ADO.NET, [Lokale transacties in ADO.NET](https://msdn.microsoft.com/library/vstudio/2k2hy99x.aspx).

### <a name="table-valued-parameters"></a>Tabelwaarden parameters
Parameters tabelwaarden ondersteuning voor de gebruiker gedefinieerde tabeltypen als parameters in Transact-SQL-instructies, opgeslagen procedures en functies. Deze aan de clientzijde batchen techniek kunt u meerdere rijen met gegevens binnen de parameter tabelwaarden verzenden. Als u wilt gebruiken tabelwaarden parameters, moet u eerst een tabelweergavetype definiëren. De volgende Transact-SQL-instructie Hiermee maakt u een tabelweergavetype met de naam **MyTableType**.

    CREATE TYPE MyTableType AS TABLE 
    ( mytext TEXT,
      num INT );
 

In code maakt u een **gegevenstabel** met de exacte dezelfde naam en type van het tabeltype. Een parameter in een tekstquery opgeven deze **gegevenstabel** of procedureaanroep opgeslagen. Het volgende voorbeeld ziet deze techniek:

    using (SqlConnection connection = new SqlConnection(CloudConfigurationManager.GetSetting("Sql.ConnectionString")))
    {
        connection.Open();
    
        DataTable table = new DataTable();
        // Add columns and rows. The following is a simple example.
        table.Columns.Add("mytext", typeof(string));
        table.Columns.Add("num", typeof(int));    
        for (var i = 0; i < 10; i++)
        {
            table.Rows.Add(DateTime.Now.ToString(), DateTime.Now.Millisecond);
        }
    
        SqlCommand cmd = new SqlCommand(
            "INSERT INTO MyTable(mytext, num) SELECT mytext, num FROM @TestTvp",
            connection);
                    
        cmd.Parameters.Add(
            new SqlParameter()
            {
                ParameterName = "@TestTvp",
                SqlDbType = SqlDbType.Structured,
                TypeName = "MyTableType",
                Value = table,
            });
    
        cmd.ExecuteNonQuery();
    }

In het vorige voorbeeld, voegt het object **SqlCommand** rijen uit een parameter tabelwaarden **@TestTvp**. Het eerder gemaakte **gegevenstabel** -object is toegewezen aan deze parameter met de methode **SqlCommand.Parameters.Add** . De invoegt in een oproep aanzienlijk batchen Hiermee verhoogt u de prestaties over opeenvolgende wordt ingevoegd.

Ter verbetering van het vorige voorbeeld verder, een opgeslagen procedure te gebruiken in plaats van een opdracht op tekst gebaseerde. Een opgeslagen procedure waarmee de parameter **SimpleTestTableType** tabelwaarden Hiermee maakt u de volgende opdracht uit Transact-SQL.

    CREATE PROCEDURE [dbo].[sp_InsertRows] 
    @TestTvp as MyTableType READONLY
    AS
    BEGIN
    INSERT INTO MyTable(mytext, num) 
    SELECT mytext, num FROM @TestTvp
    END
    GO

Wijzig de declaratie **SqlCommand** object in het vorige codevoorbeeld als volgt uit.

    SqlCommand cmd = new SqlCommand("sp_InsertRows", connection);
    cmd.CommandType = CommandType.StoredProcedure;

In de meeste gevallen hebt tabelwaarden parameters zelfde of betere prestaties dan andere batchen technieken. Parameters tabelwaarden zijn vaak voorkeur, omdat ze flexibeler dan de andere opties. Andere technieken, zoals SQL bulksgewijs kopiëren, vergunningsaanvragen bijvoorbeeld alleen het invoegen van nieuwe rijen. Maar met tabelwaarden parameters, kunt u logica gebruiken in de opgeslagen procedure om te bepalen welke rijen updates en die zijn ingevoegd. Het tabeltype kan ook worden gewijzigd aanduiding van een 'Bewerking'-kolom waarin wordt aangegeven of de opgegeven rij moet worden ingevoegd, bijgewerkt of verwijderd.

De volgende tabel ziet u ad-hoc-testresultaten voor het gebruik van parameters tabelwaarden (in milliseconden).

| Bewerkingen | On-Premises naar Azure ([ms)  | Azure dezelfde datacenter ([ms) |
|---|---|---|
| 1 | 124 | 32 |
| 10 | 131 | 25 |
| 100 | 338 | 51 |
| 1000 | 2615 | 382 |
| 10000 | 23830 | 3586 |

>[AZURE.NOTE] Resultaten zijn niet benchmarks. Zie de [Opmerking over de timing resultaten in dit onderwerp](#note-about-timing-results-in-this-topic).

De prestatieverbetering uit batchen is opvallen. In de vorige opeenvolgende test duurde 1000 bewerkingen 129 seconden buiten het datacenter en 21 seconden van binnen het datacenter. Maar met tabelwaarden parameters, 1000 bewerkingen alleen 2.6 seconden buiten het datacenter en 0,4 seconden binnen het datacenter duren.

Zie voor meer informatie over de tabelwaarden parameters, [Table-Valued Parameters](https://msdn.microsoft.com/library/bb510489.aspx).

### <a name="sql-bulk-copy"></a>SQL bulksgewijs kopiëren
SQL bulksgewijs kopiëren is een andere manier om grote hoeveelheden gegevens in een doeldatabase invoegen. .NET-toepassingen kunnen u de klas **SqlBulkCopy** gebruiken om uit te voeren bulkbewerkingen invoegen. **SqlBulkCopy** is vergelijkbaar in functie met het hulpprogramma voor, **Bcp.exe**of de Transact-SQL-instructie, **BULKSGEWIJS invoegen**. Het volgende voorbeeld ziet u hoe bulksgewijs kopiëren de rijen in de bron **gegevenstabel**, tabel, naar de doeltabel in SQL Server, MyTable.

    using (SqlConnection connection = new SqlConnection(CloudConfigurationManager.GetSetting("Sql.ConnectionString")))
    {
        connection.Open();
    
        using (SqlBulkCopy bulkCopy = new SqlBulkCopy(connection))
        {
            bulkCopy.DestinationTableName = "MyTable";
            bulkCopy.ColumnMappings.Add("mytext", "mytext");
            bulkCopy.ColumnMappings.Add("num", "num");
            bulkCopy.WriteToServer(table);
        }
    }

Er zijn enkele zaken waar bulksgewijs kopiëren heeft de voorkeur boven tabelwaarden parameters. Zie de vergelijkingstabel met parameters die tabelwaarden versus bewerkingen BULKSGEWIJS invoegen in het onderwerp [Table-Valued Parameters](https://msdn.microsoft.com/library/bb510489.aspx).

De volgende ad-hoc-testresultaten weergeven de prestaties van batchen met **SqlBulkCopy** (in milliseconden).

| Bewerkingen | On-Premises naar Azure ([ms)  | Azure dezelfde datacenter ([ms) |
|---|---|---|
| 1 | 433 | 57 |
| 10 | 441 | 32 |
| 100 | 636 | 53 |
| 1000 | 2535 | 341 |
| 10000 | 21605 | 2737 |

>[AZURE.NOTE] Resultaten zijn niet benchmarks. Zie de [Opmerking over de timing resultaten in dit onderwerp](#note-about-timing-results-in-this-topic).

In de batch kleinere, ver-de klasse **SqlBulkCopy** loop van de parameters gebruiken tabelwaarden. Echter uitgevoerd **SqlBulkCopy** 12-31% sneller dan de tabelwaarden parameters voor de tests van 1000 en 10.000 rijen. **SqlBulkCopy** is een goede keuze voor batch wordt ingevoegd, zoals tabelwaarden parameters, met name wanneer vergeleken met de prestaties van bewerkingen niet batch verwerkt.

Zie voor meer informatie over bulksgewijs kopiëren in ADO.NET, [Kopie bulkbewerkingen in SQL Server](https://msdn.microsoft.com/library/7ek5da1a.aspx).

### <a name="multiple-row-parameterized-insert-statements"></a>Meerdere rijen met parameters INSERT-instructies
Een alternatief voor kleine batches is invoeginstructie te maken om grote met parameters die meerdere rijen invoegen. Het volgende voorbeeld ziet u deze techniek.

    using (SqlConnection connection = new SqlConnection(CloudConfigurationManager.GetSetting("Sql.ConnectionString")))
    {
        connection.Open();
    
        string insertCommand = "INSERT INTO [MyTable] ( mytext, num ) " +
            "VALUES (@p1, @p2), (@p3, @p4), (@p5, @p6), (@p7, @p8), (@p9, @p10)";
    
        SqlCommand cmd = new SqlCommand(insertCommand, connection);
    
        for (int i = 1; i <= 10; i += 2)
        {
            cmd.Parameters.Add(new SqlParameter("@p" + i.ToString(), "test"));
            cmd.Parameters.Add(new SqlParameter("@p" + (i+1).ToString(), i));
        }
    
        cmd.ExecuteNonQuery();
    }
 

In dit voorbeeld is bedoeld om weer te geven van de basisprincipes. Een scenario voor meer natuurlijke zou doorlopen van de vereiste entiteiten de queryreeks en de opdrachtparameters tegelijk samenstellen. U zijn beperkt tot een totaal van 2100 queryparameters, zodat dit houdt in dat het totale aantal rijen dat op deze manier kan worden verwerkt.

De volgende ad-hoc-testresultaten weergeven de prestaties van dit type instructie insert (in milliseconden).

| Bewerkingen | Tabelwaarden parameters ([ms) | Één instructie invoegen ([ms) |
|---|---|---|
| 1 | 32 | 20 |
| 10 | 30 | 25 |
| 100 | 33 | 51 |

>[AZURE.NOTE] Resultaten zijn niet benchmarks. Zie de [Opmerking over de timing resultaten in dit onderwerp](#note-about-timing-results-in-this-topic).

Deze methode is iets snellere voor batches die minder dan 100 rijen zijn. Hoewel de verbetering klein is, is is deze techniek een andere optie die u mogelijk in uw scenario specifieke toepassing.

### <a name="dataadapter"></a>DataAdapter
De klas **DataAdapter** kunt u een **gegevensset** object wijzigen en vervolgens dient de wijzigingen zoals invoegen, bijwerken en verwijderen. Als u de **DataAdapter** op deze manier gebruikt, is het belangrijk te weten dat de afzonderlijke oproepen voor elke afzonderlijke bewerking worden aangebracht. Prestaties verbeteren door de eigenschap **UpdateBatchSize** voor het aantal bewerkingen die batch moeten worden geplaatst tegelijk te gebruiken. Zie [Uitvoering Batch bewerkingen gebruiken DataAdapters](https://msdn.microsoft.com/library/aadf8fk2.aspx)voor meer informatie.

### <a name="entity-framework"></a>Entiteit framework
Entiteit Framework ondersteunt geen momenteel batchen. Verschillende ontwikkelaars in de community hebt geprobeerd om te laten zien tijdelijke oplossingen, zoals de methode **SaveChanges** overschrijven. Maar de oplossingen zijn meestal complexe en aangepaste met de toepassing en het gegevensmodel. Het entiteit Framework codeplex project heeft momenteel een discussiepagina op een verzoek om deze functie. Zie [Vergaderingsnotities ontwerp: 2 augustus 2012](http://entityframework.codeplex.com/wikipage?title=Design%20Meeting%20Notes%20-%20August%202%2c%202012)deze discussie.

### <a name="xml"></a>XML
We leven dat is het belangrijk om computers nader bekeken XML als een batchen strategie voor de volledigheid. Het gebruik van XML heeft echter geen voordelen ten opzichte van andere methoden en verschillende nadelen. De methode is vergelijkbaar met tabelwaarden parameters, maar een XML-bestand of een tekenreeks wordt doorgegeven aan een opgeslagen procedure in plaats van een tabel door gebruiker gedefinieerd. De opgeslagen procedure parseert de opdrachten in de opgeslagen procedure.

Er zijn verschillende nadelen deze methode:

- Werken met XML lastig en fout vatbaar.
- Parseren van de XML-gegevens voor de database kan CPU veel zijn.
- Deze methode is in de meeste gevallen langzamer dan tabelwaarden parameters.

Het gebruik van XML voor batch query's wordt niet aanbevolen om deze redenen.

## <a name="batching-considerations"></a>Batchen overwegingen
De volgende secties vindt meer informatie vinden voor het gebruik van batchen in SQL-Database-toepassingen.

### <a name="tradeoffs"></a>Compromissen nodig zijn
Afhankelijk van uw architectuur, kunt batchen gebruikmaakt van een verhouding tussen de prestaties en tolerantie. Stel het scenario waar uw rol onverwacht afgesloten wordt. Als u één rij met gegevens kwijtraakt, is de invloed kleiner is dan de invloed van een grote reeks niet-verzonden rijen verliezen. Er is een groter risico wanneer u rijen buffer voordat ze worden verzonden met de database in een opgegeven tijdvenster valt.

Evalueren vanwege dit verhouding, het type bewerkingen die u batch. Meer serieus batch (grotere batches en langere tijd windows) met gegevens die minder essentieel.

### <a name="batch-size"></a>Batchgrootte
In onze tests is er meestal geen voordeel van grote batches splitsen in kleinere stukken. Deze onderverdeling vaak geleid in feite trager dan één grote batch versturen. Stel een scenario waarbij u rijen wilt invoegen 1000. De volgende tabel ziet u hoe lang het duurt om de tabelwaarden parameters gebruiken om in te voegen 1000 rijen na de deling in kleinere batches.

| Batchgrootte | Iteraties | Tabelwaarden parameters ([ms) |
| -------- | --- | --- |
| 1000 | 1 | 347 |
| 500 | 2 | 355 |
| 100 | 10 | 465 |
| 50 | 20 | 630 |

>[AZURE.NOTE] Resultaten zijn niet benchmarks. Zie de [Opmerking over de timing resultaten in dit onderwerp](#note-about-timing-results-in-this-topic).

U kunt zien dat de beste prestaties voor 1000 rijen is deze allemaal tegelijk verzenden. Bevatte kleine prestaties toe aan een batch 10000 rij splitsen in twee batches van 5000 in andere tests (niet u hier ziet). Maar de tabelschema voor deze tests is vrij eenvoudig, dus u moet tests uitvoeren op uw specifieke gegevens en de grootte van de batch om te controleren of deze resultaten.

Een andere factor u rekening moet houden is dat als de totale batch te groot, SQL-Database mogelijk beperken en doorvoeren van de batch weigeren. Test uw specifieke scenario om te bepalen of er een ideaal batchgrootte voor het beste resultaat. Controleer de batchgrootte configureerbare gedurende runtime om in te schakelen snelle aanpassingen op basis van prestaties of fouten.

Tot slot saldo vanaf de grootte van de batch met de risico's die zijn gekoppeld aan batchen. Als er tijdelijke fouten of de rol is mislukt, kunt u de gevolgen van het opnieuw of dat u de gegevens in de batch kwijtraakt.

### <a name="parallel-processing"></a>Parallelle verwerking
Wat gebeurt er als u de methode van het verkleinen van de batchgrootte hebt gemaakt, maar meerdere threads gebruikt voor het uitvoeren van het werk? Klik nogmaals Wees onze tests uit dat verschillende kleiner meerdere threads batches meestal uitgevoerd slechter dan één groter batch. De volgende test probeert 1000 rijen invoegen in een of meer parallelle batches. Deze test ziet u hoe meer tegelijk batches afgenomen daadwerkelijk prestaties.

| Batchgrootte [iteraties] | Twee threads ([ms) | Vier threads ([ms) | Zes threads ([ms) |
| -------- | --- | --- | --- |
| 1000 [1] | 277 | 315 | 266 |
| 500 [2] | 548 | 278 | 256 |
| 250 [4] | 405 | 329 | 265 |
| 100 [10] | 488 | 439 | 391 |

>[AZURE.NOTE] Resultaten zijn niet benchmarks. Zie de [Opmerking over de timing resultaten in dit onderwerp](#note-about-timing-results-in-this-topic).

Er zijn verschillende mogelijke redenen voor de verminderde prestaties vanwege parallellisme:

- Er zijn meerdere tegelijk netwerk oproepen in plaats van één.
- Meerdere bewerkingen op één tabel kunnen resulteren in een conflict en blokkeren.
- Er zijn algemene die is gekoppeld aan multithreading.
- De kosten van het openen van meerdere verbindingen belangrijker is dan het voordeel van parallelle verwerking.

Wanneer u verschillende tabellen of databases, is het mogelijk om te zien krijgen met deze strategie prestaties. Database sharding of overkoepelende organisaties zou een scenario voor deze methode. Sharding beschikt over meerdere databases en andere gegevens stuurt naar elke database. Als u elke kleine batch wordt ingebeld bij een andere database, kan vervolgens de bewerkingen uit te voeren parallel efficiënter zijn. De prestatieverbetering is echter niet aanzienlijk wilt gebruiken als basis voor een besluit database sharding gebruiken in uw oplossing.

In sommige ontwerpen, parallelle uitvoering van kleinere batches kan leiden tot verbeterde doorvoer van aanvragen in een systeem onder laden. In dit geval Hoewel sneller verwerkingstijd van één batch groter is, het verwerken van meerdere batches parallel mogelijk efficiënter.

Als u parallelle uitvoering gebruikt, kunt u het maximum aantal werknemer threads bepalen. Een kleiner getal kan leiden tot minder conflict en een sneller worden uitgevoerd. U kunt wellicht ook de extra laden die volgt op de doeldatabase zowel in verbindingen en transacties geplaatst.

### <a name="related-performance-factors"></a>Gerelateerde prestatiefactoren
Normale richtlijnen voor prestaties van de database heeft ook gevolgen batchen. Voeg bijvoorbeeld prestaties verminderen voor tabellen die beschikt over een grote primaire sleutel of veel niet-gegroepeerde indexen.

Als de tabelwaarden parameters gebruiken een opgeslagen procedure, kunt u de opdracht **SET NOCOUNT ON** aan het begin van de procedure. Deze instructie onderdrukt het rendement van het aantal van de desbetreffende rijen in de procedure. Echter in onze tests, het gebruik van de **SET NOCOUNT ON** heeft geen effect of afgenomen prestaties. De opgeslagen testprocedure is eenvoudig met een enkel **Invoegen** command uit de tabelwaarden-parameter. Het is mogelijk dat complexere opgeslagen procedures van deze instructie profiteren wilt. Maar niet wordt ervan uitgegaan dat **SET NOCOUNT ON** automatisch toevoegen aan uw opgeslagen procedure prestaties verbetert. Als u wilt weten over het effect, test u de opgeslagen procedure met en zonder de **SET NOCOUNT ON** -instructie.

## <a name="batching-scenarios"></a>Batchen scenario 's
De volgende secties wordt beschreven hoe tabelwaarden parameters gebruiken in drie scenario's van toepassing. Het eerste scenario ziet u hoe buffer en batchen kunnen samenwerken. Het tweede scenario verbeterde prestaties door detailsectie bewerkingen in één opgeslagen procedure gesprek te voeren. Het uiteindelijke scenario ziet hoe u de tabelwaarden parameters gebruiken in een bewerking "UPSERT".

### <a name="buffering"></a>Buffer
Hoewel er enkele scenario's die zijn van de hand liggende candidate zijn voor batchen, zijn er veel scenario's die profiteren kunnen van batchen door vertraagde verwerking. Vertraagde verwerking wordt echter ook een groter risico dat de gegevens verloren in het geval van een onverwachte fout is. Het is belangrijk om te begrijpen dit risico en houd rekening met de gevolgen.

Stel een webtoepassing waarin de navigatiegeschiedenis van elke gebruiker worden bijgehouden. Op elke paginaverzoek, kan de toepassing een database belt u bij het opnemen van de gebruiker paginaweergave maken. Maar betere prestaties en schaalbaarheid kunnen worden bereikt door de gebruikers navigatie activiteiten buffer en vervolgens deze gegevens wordt verzonden naar de database in batches. U kunt de update van de database door de verstreken tijd en/of de grootte kunt activeren. Een regel kan bijvoorbeeld opgeven dat de batch moet worden verwerkt na 20 seconden of wanneer de buffer 1000 items bereikt.

Het volgende voorbeeld wordt [Reactieve extensies - Rx](https://msdn.microsoft.com/data/gg577609) gebruikt om buffer gebeurtenissen die door een controle klasse te verwerken. Wanneer de buffer of een time-out is bereikt, de batch van gebruikersgegevens wordt verzonden naar de database met een parameter tabelwaarden.

De volgende NavHistoryData klasse model de gegevens van de gebruiker-navigatie. Deze bevat basisgegevens zoals de gebruikers-id, de URL geopend en de tijd van toegang.

    public class NavHistoryData
    {
        public NavHistoryData(int userId, string url, DateTime accessTime)
        { UserId = userId; URL = url; AccessTime = accessTime; }
        public int UserId { get; set; }
        public string URL { get; set; }
        public DateTime AccessTime { get; set; }
    }

De NavHistoryDataMonitor-klasse is verantwoordelijk voor buffer de gebruikersgegevens van de navigatie met de database. De presentatie bevat een methode, RecordUserNavigationEntry, die door een gebeurtenis **OnAdded** voldoet. De volgende code toont de logica constructor die Rx gebruikt om een waarneembare verzameling op basis van de gebeurtenis te maken. Deze zich abonneert op deze waarneembare verzameling met de methode Buffer. De overbelasting geeft aan dat de buffer moet worden verzonden elke 20 seconden of 1000 posten.

    public NavHistoryDataMonitor()
    {
        var observableData =
            Observable.FromEventPattern<NavHistoryDataEventArgs>(this, "OnAdded");
    
        observableData.Buffer(TimeSpan.FromSeconds(20), 1000).Subscribe(Handler);           
    }

De handler converteert alle buffer items naar een type tabelwaarden en dit type vervolgens doorgeven naar een opgeslagen procedure waarmee de batch worden verwerkt. De volgende code ziet u de volledige definitie voor de NavHistoryDataEventArgs zowel de klassen NavHistoryDataMonitor.

    public class NavHistoryDataEventArgs : System.EventArgs
    {
        public NavHistoryDataEventArgs(NavHistoryData data) { Data = data; }
        public NavHistoryData Data { get; set; }
    }
    
    public class NavHistoryDataMonitor
    {
        public event EventHandler<NavHistoryDataEventArgs> OnAdded;
    
        public NavHistoryDataMonitor()
        {
            var observableData =
                Observable.FromEventPattern<NavHistoryDataEventArgs>(this, "OnAdded");
    
            observableData.Buffer(TimeSpan.FromSeconds(20), 1000).Subscribe(Handler);           
        }
    
        public void RecordUserNavigationEntry(NavHistoryData data)
        {    
            if (OnAdded != null)
                OnAdded(this, new NavHistoryDataEventArgs(data));
        }
    
        protected void Handler(IList<EventPattern<NavHistoryDataEventArgs>> items)
        {
            DataTable navHistoryBatch = new DataTable("NavigationHistoryBatch");
            navHistoryBatch.Columns.Add("UserId", typeof(int));
            navHistoryBatch.Columns.Add("URL", typeof(string));
            navHistoryBatch.Columns.Add("AccessTime", typeof(DateTime));
            foreach (EventPattern<NavHistoryDataEventArgs> item in items)
            {
                NavHistoryData data = item.EventArgs.Data;
                navHistoryBatch.Rows.Add(data.UserId, data.URL, data.AccessTime);
            }
    
            using (SqlConnection connection = new SqlConnection(CloudConfigurationManager.GetSetting("Sql.ConnectionString")))
            {
                connection.Open();
    
                SqlCommand cmd = new SqlCommand("sp_RecordUserNavigation", connection);
                cmd.CommandType = CommandType.StoredProcedure;
    
                cmd.Parameters.Add(
                    new SqlParameter()
                    {
                        ParameterName = "@NavHistoryBatch",
                        SqlDbType = SqlDbType.Structured,
                        TypeName = "NavigationHistoryTableType",
                        Value = navHistoryBatch,
                    });
    
                cmd.ExecuteNonQuery();
            }
        }
    }

Als u wilt deze buffer klasse gebruiken, maakt de toepassing een statische NavHistoryDataMonitor-object. Telkens wanneer die een gebruiker toegang heeft tot een pagina, roept de toepassing de methode NavHistoryDataMonitor.RecordUserNavigationEntry. De buffer logica verloopt in deze vermeldingen verzenden naar de database in batches kunt verrichten.

### <a name="master-detail"></a>Hoofdsectie detailsectie
Parameters tabelwaarden zijn handig voor eenvoudige invoegen-scenario's. Dit kan echter zijn moeilijker naar batch wordt ingevoegd waarbij u gebruikmaakt van meer dan één tabel. Het 'hoofd-of detailsectie'-scenario is een goed voorbeeld. De basispagina tabel bevat de primaire entiteit. Een of meer detailtabellen opslaan meer gegevens over de entiteit. In dit scenario afdwingen externe-sleutelrelaties de relatie van details voor een unieke basispagina entiteit. Houd rekening met een vereenvoudigde versie van een tabel PurchaseOrder en de bijbehorende OrderDetail-tabel. De volgende Transact-SQL Hiermee maakt u de tabel PurchaseOrder met vier kolommen: order-id, orderdatum klantnummer en Status.

    CREATE TABLE [dbo].[PurchaseOrder](
    [OrderID] [int] IDENTITY(1,1) NOT NULL,
    [OrderDate] [datetime] NOT NULL,
    [CustomerID] [int] NOT NULL,
    [Status] [nvarchar](50) NOT NULL,
     CONSTRAINT [PrimaryKey_PurchaseOrder] 
    PRIMARY KEY CLUSTERED ( [OrderID] ASC ))

Elke order bevat een of meer kopen van een product. Deze informatie is opgenomen in de tabel PurchaseOrderDetail. De volgende Transact-SQL Hiermee maakt u de tabel PurchaseOrderDetail vijf kolommen: order-id, OrderDetailID product-id, prijs per eenheid en OrderQty.

    CREATE TABLE [dbo].[PurchaseOrderDetail](
    [OrderID] [int] NOT NULL,
    [OrderDetailID] [int] IDENTITY(1,1) NOT NULL,
    [ProductID] [int] NOT NULL,
    [UnitPrice] [money] NULL,
    [OrderQty] [smallint] NULL,
     CONSTRAINT [PrimaryKey_PurchaseOrderDetail] PRIMARY KEY CLUSTERED 
    ( [OrderID] ASC, [OrderDetailID] ASC ))

De kolom order-id in de tabel PurchaseOrderDetail moet verwijzen naar een order uit de tabel PurchaseOrder. De volgende definitie van een refererende sleutel Hiermee past u deze beperking.

    ALTER TABLE [dbo].[PurchaseOrderDetail]  WITH CHECK ADD 
    CONSTRAINT [FK_OrderID_PurchaseOrder] FOREIGN KEY([OrderID])
    REFERENCES [dbo].[PurchaseOrder] ([OrderID])

Pas tabelwaarden parameters gebruiken, moet u één door gebruiker gedefinieerd tabeltype voor elke doeltabel hebben.

    CREATE TYPE PurchaseOrderTableType AS TABLE 
    ( OrderID INT,
      OrderDate DATETIME,
      CustomerID INT,
      Status NVARCHAR(50) );
    GO
    
    CREATE TYPE PurchaseOrderDetailTableType AS TABLE 
    ( OrderID INT,
      ProductID INT,
      UnitPrice MONEY,
      OrderQty SMALLINT );
    GO

Definieer vervolgens een opgeslagen procedure waarin tabellen van deze diagramtypen. Deze procedure kan een toepassing lokaal batch een set orders en orderdetails in één aanroep. De volgende Transact-SQL biedt de declaratie voltooid opgeslagen procedure voor dit voorbeeld van de volgorde aanschaffen.

    CREATE PROCEDURE sp_InsertOrdersBatch (
    @orders as PurchaseOrderTableType READONLY,
    @details as PurchaseOrderDetailTableType READONLY )
    AS
    SET NOCOUNT ON;
    
    -- Table that connects the order identifiers in the @orders
    -- table with the actual order identifiers in the PurchaseOrder table
    DECLARE @IdentityLink AS TABLE ( 
    SubmittedKey int, 
    ActualKey int, 
    RowNumber int identity(1,1)
    );
     
          -- Add new orders to the PurchaseOrder table, storing the actual
    -- order identifiers in the @IdentityLink table   
    INSERT INTO PurchaseOrder ([OrderDate], [CustomerID], [Status])
    OUTPUT inserted.OrderID INTO @IdentityLink (ActualKey)
    SELECT [OrderDate], [CustomerID], [Status] FROM @orders ORDER BY OrderID;
    
    -- Match the passed-in order identifiers with the actual identifiers
    -- and complete the @IdentityLink table for use with inserting the details
    WITH OrderedRows As (
    SELECT OrderID, ROW_NUMBER () OVER (ORDER BY OrderID) As RowNumber 
    FROM @orders
    )
    UPDATE @IdentityLink SET SubmittedKey = M.OrderID
    FROM @IdentityLink L JOIN OrderedRows M ON L.RowNumber = M.RowNumber;
    
    -- Insert the order details into the PurchaseOrderDetail table, 
          -- using the actual order identifiers of the master table, PurchaseOrder
    INSERT INTO PurchaseOrderDetail (
    [OrderID],
    [ProductID],
    [UnitPrice],
    [OrderQty] )
    SELECT L.ActualKey, D.ProductID, D.UnitPrice, D.OrderQty
    FROM @details D
    JOIN @IdentityLink L ON L.SubmittedKey = D.OrderID;
    GO

In dit voorbeeld de lokaal gedefinieerde @IdentityLink tabel bevat de werkelijke order-id-waarden uit de nieuw ingevoegde rijen. Deze order-id's is het verschil tussen de tijdelijke order-id-waarden in de @orders en @details tabelwaarden parameters. Daarom de @IdentityLink tabel maakt vervolgens verbinding met de order-id-waarden uit de @orders -parameter voor de reële order-id-waarden voor de nieuwe rijen in de tabel PurchaseOrder. Na deze stap geeft de @IdentityLink tabel kunt vergemakkelijken het invoegen van de details van bestelling met de werkelijke order-id die voldoet aan de refererende sleutel beperking.

Deze opgeslagen procedure kan worden gebruikt vanaf code of andere gesprekken Transact-SQL. Zie het gedeelte tabelwaarden parameters van dit artikel voor een codevoorbeeld. De volgende Transact-SQL ziet hoe u de sp_InsertOrdersBatch bellen.

    declare @orders as PurchaseOrderTableType
    declare @details as PurchaseOrderDetailTableType
    
    INSERT @orders 
    ([OrderID], [OrderDate], [CustomerID], [Status])
    VALUES(1, '1/1/2013', 1125, 'Complete'),
    (2, '1/13/2013', 348, 'Processing'),
    (3, '1/12/2013', 2504, 'Shipped')
    
    INSERT @details
    ([OrderID], [ProductID], [UnitPrice], [OrderQty])
    VALUES(1, 10, $11.50, 1),
    (1, 12, $1.58, 1),
    (2, 23, $2.57, 2),
    (3, 4, $10.00, 1)
    
    exec sp_InsertOrdersBatch @orders, @details

Deze oplossing kunt elke batch gehanteerd order-id-waarden die bij 1 beginnen. Deze tijdelijke order-id-waarden de relaties in de batch beschreven, maar de werkelijke order-id-waarden worden bepaald op het moment van de invoegbewerking. U kunt meerdere keren dezelfde instructies in het vorige voorbeeld worden uitgevoerd en unieke orders genereren in de database. Overweeg meer code of database logica waardoor dubbele orders bij gebruik van deze methode batchen daarom.

In dit voorbeeld ziet u dat nog meer complexe database-bewerkingen uitvoeren, zoals detailsectie bewerkingen uitvoeren, batch kunnen worden verzameld met tabelwaarden parameters.

### <a name="upsert"></a>UPSERT
Een ander batchen scenario heeft betrekking op tegelijk bijwerken van bestaande rijen en nieuwe rijen invoegen. Deze bewerking wordt ook wel een bewerking "UPSERT" (update + insert) genoemd. In plaats van afzonderlijke gesprekken invoegen en bijwerken, de instructie samenvoegen meest geschikt is voor deze taak. De samenvoegen-instructie kunt uitvoeren van beide invoegen en bewerkingen in één aanroep bijwerken.

Parameters tabelwaarden kunnen worden gebruikt met de instructie samenvoegen om uit te voeren updates en wordt ingevoegd. Stel dat u een eenvoudigere werknemerstabel met de volgende kolommen: veld Werknemer-id, voornaam, achternaam, SocialSecurityNumber:

    CREATE TABLE [dbo].[Employee](
    [EmployeeID] [int] IDENTITY(1,1) NOT NULL,
    [FirstName] [nvarchar](50) NOT NULL,
    [LastName] [nvarchar](50) NOT NULL,
    [SocialSecurityNumber] [nvarchar](50) NOT NULL,
     CONSTRAINT [PrimaryKey_Employee] PRIMARY KEY CLUSTERED 
    ([EmployeeID] ASC ))
 
In dit voorbeeld kunt u het feit dat de SocialSecurityNumber uniek zijn voor het samenvoegen van meerdere werknemers. U maakt eerst het tabeltype door gebruiker gedefinieerd:

    CREATE TYPE EmployeeTableType AS TABLE 
    ( Employee_ID INT,
      FirstName NVARCHAR(50),
      LastName NVARCHAR(50),
      SocialSecurityNumber NVARCHAR(50) );
    GO

Vervolgens maken van een opgeslagen procedure of code schrijven die de instructie samenvoegen gebruikt de update uitvoeren en invoegen. Het volgende voorbeeld wordt de instructie samenvoegen op een parameter tabelwaarden @employees, van het type EmployeeTableType. De inhoud van de @employees tabel worden hier niet weergegeven.

    MERGE Employee AS target
    USING (SELECT [FirstName], [LastName], [SocialSecurityNumber] FROM @employees) 
    AS source ([FirstName], [LastName], [SocialSecurityNumber])
    ON (target.[SocialSecurityNumber] = source.[SocialSecurityNumber])
    WHEN MATCHED THEN 
    UPDATE SET
    target.FirstName = source.FirstName, 
    target.LastName = source.LastName
    WHEN NOT MATCHED THEN
       INSERT ([FirstName], [LastName], [SocialSecurityNumber])
       VALUES (source.[FirstName], source.[LastName], source.[SocialSecurityNumber]);

Zie de documentatie en voorbeelden voor het samenvoegen van overzicht voor meer informatie. Hoewel het werk dat dezelfde kan worden uitgevoerd in een opgeslagen meerdere stappen procedureaanroep met scheiden invoegen en updatebewerkingen, de instructie samenvoegen is efficiënter. Databasecode kunt ook Transact-SQL-oproepen die de instructie samenvoegen gebruiken rechtstreeks zonder twee oproepen van de database voor de INVOEG- en UPDATE maken.

## <a name="recommendation-summary"></a>Aanbeveling samenvatting

De volgende lijst bevat een overzicht van de batchen aanbevelingen in dit onderwerp wordt besproken:

- Gebruik buffer en batchen om uit te breiden de prestaties en schaalbaarheid van SQL-Database-toepassingen.
- Begrijpen welke compromissen nodig zijn tussen batchen/buffer en tolerantie. Tijdens een rol is mislukt, kan de kans op pesterijen verliezen een onverwerkte reeks belangrijke bedrijfsinformatie wegen tegen het voordeel van de prestaties van het batchen.
- Wilt u alle oproepen naar de database binnen een één datacenter verkleinen van latentie behouden.
- Als u een bepaalde batchen techniek kiest, wordt tabelwaarden parameters bieden de beste prestaties en flexibiliteit.
- Volg deze algemene richtlijnen voor het snelste prestaties invoegen maar testen van uw scenario:
    - Gebruik een eenmalige opdracht met parameters voor < 100 rijen.
    - Voor < 1000 rijen, tabelwaarden parameters te gebruiken.
    - Voor > = 1000 rijen, SqlBulkCopy gebruiken.
- Voor bijwerken en verwijderen van de bewerkingen, tabelwaarden parameters gebruiken met de opgeslagen procedure logica die de juiste bewerking op elke rij in de tabel-parameter bepaalt.
- Richtlijnen voor batch grootte:
    - Gebruik de grootste batch grootte die geschikt is voor uw toepassing en vereisten voor bedrijven.
    - Saldo vanaf de prestatieverbetering van grote batches met de risico's van tijdelijke of fatale fouten. Wat is het resultaat is van nieuwe pogingen of verlies van de gegevens in de batch? 
    - Test de grootte van de grootste partij om te bevestigen dat SQL-Database wordt niet weigeren.
    - Maak configuratie-instellingen in dat besturingselement batchen, zoals de batchgrootte van de of het buffer tijdvenster. Deze instellingen bieden flexibiliteit. U kunt het batchen gedrag in productie wijzigen zonder de cloudservice opnieuw te distribueren.
- Voorkom parallelle uitvoering van batches die in één tabel in één database werken. Als u één batch verdelen over meerdere werknemer threads kiest, voert u tests om te bepalen de ideaal aantal threads. Na een niet opgegeven drempel, meer threads wordt invloed hebben op prestaties in plaats van verhogen.
- Houd rekening met buffer op grootte en de tijd als een manier voor het implementeren van batchen voor meer scenario's.

## <a name="next-steps"></a>Volgende stappen

In dit artikel is gericht op hoe databaseontwerp en technieken die betrekking hebben op batchen kleurcodering de prestaties en schaalbaarheid verbeteren kunnen. Maar dit is maar één factor in uw algemene strategie. Zie voor meer manieren om te verbeteren de prestaties en schaalbaarheid [Azure SQL-Database prestaties richtlijnen voor één databases](sql-database-performance-guidance.md) en [Aandachtspunten voor de prijs en prestaties voor een elastische database-toepassingen](sql-database-elastic-pool-guidance.md).
