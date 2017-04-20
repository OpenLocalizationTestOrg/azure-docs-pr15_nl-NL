<properties
    pageTitle="Een SQL-verbindingsfout, tijdelijke fout oplossen | Microsoft Azure"
    description="Leer hoe u problemen met, een diagnose stellen bij en voorkomen dat een SQL-verbinding of een tijdelijke fout in Azure SQL-Database. "
    keywords="SQL-verbinding, verbindingsreeks, verbindingsproblemen, tijdelijke fout, verbindingsfout"
    services="sql-database"
    documentationCenter=""
    authors="dalechen"
    manager="felixwu"
    editor=""/>

<tags
    ms.service="sql-database"
    ms.workload="sql-database"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/20/2016"
    ms.author="daleche"/>


# <a name="troubleshoot-diagnose-and-prevent-sql-connection-errors-and-transient-errors-for-sql-database"></a>Problemen met, een diagnose stellen bij en SQL-verbindingsfouten en tijdelijke fouten te voorkomen voor SQL-Database

In dit artikel wordt beschreven hoe voorkomen, problemen met een diagnose stellen bij en verklein verbindingsfouten en tijdelijke fouten die de clienttoepassing wordt aangetroffen wanneer deze met Azure SQL-Database samenwerkt. Informatie over het configureren van opnieuw logica, de verbindingsreeks en andere verbindingsinstellingen aanpassen.

<a id="i-transient-faults" name="i-transient-faults"></a>

## <a name="transient-errors-transient-faults"></a>Tijdelijke fouten tijdelijke

Een tijdelijke fout - ook tijdelijke foutenstructuuranalyse - heeft een onderliggende oorzaak binnenkort om op te lossen zelf. Een incidentele oorzaak van tijdelijke fouten is wanneer het Azure systeem snel naar wordt verplaatst hardware resources betere taakverdeling verschillende werkbelasting. Meest van deze gebeurtenissen nieuwe configuratie voltooid vaak in minder dan 60 seconden. Tijdens deze nieuwe configuratie tijdspanne, u moet mogelijk verbindingsproblemen met Azure SQL-Database. Toepassingen verbinden met Azure SQL-Database moeten worden gemaakt naar deze tijdelijke fouten verwachten, deze opnieuw logica implementeren in hun code, in plaats van deze naar klasnotitieblokken weergegeven aan gebruikers toepassingsfouten te verwerken.

Als uw clientprogramma ADO.NET gebruikt wordt, wordt het programma gemeld over de tijdelijke fout door de weggooien van een **SqlException**. De eigenschap **getal** kan worden vergeleken in de lijst met tijdelijke fouten bij de bovenkant van het onderwerp: [SQL-foutcodes voor SQL-Database-clienttoepassingen](sql-database-develop-error-messages.md).

<a id="connection-versus-command" name="connection-versus-command"></a>

### <a name="connection-versus-command"></a>Verbinding versus opdracht

U moet de SQL-verbinding opnieuw of stand brengen van deze opnieuw, afhankelijk van het volgende:

* **Een tijdelijke fout optreedt tijdens verbinding proberen**: de verbinding moet opnieuw worden geprobeerd na enkele seconden vertragen.

* **Een tijdelijke fout optreedt tijdens een SQL-query-opdracht**: de opdracht moet niet opnieuw worden direct geprobeerd. In plaats daarvan na een vertraging moet de vers verbinding maken. Vervolgens de opdracht opnieuw kan worden verzonden.


<a id="j-retry-logic-transient-faults" name="j-retry-logic-transient-faults"></a>

### <a name="retry-logic-for-transient-errors"></a>Probeer de logica voor tijdelijke fouten


Clientprogramma's die soms een tijdelijke fout ondervindt zijn krachtiger wanneer ze opnieuw logica bevatten.


Wanneer uw programma met Azure SQL Database via een 3e partijen middleware communiceert, inquire met de leverancier of de middleware opnieuw logica voor tijdelijke fouten bevat.

<a id="principles-for-retry" name="principles-for-retry"></a>

#### <a name="principles-for-retry"></a>Beginselen voor opnieuw


- Een poging om een verbinding te openen, moet opnieuw worden geprobeerd als de fout tijdelijke is.


- Een SQL-instructie SELECT die niet met een tijdelijke fout moet niet opnieuw worden geprobeerd rechtstreeks.
 - In plaats daarvan een vers verbinding tot stand brengen en probeer opnieuw te selecteren.


- Wanneer een UPDATE van de SQL-instructie met een tijdelijke fout mislukt, moet fris verbinding voordat de UPDATE wordt herhaald.
 - De logica opnieuw moet ervoor zorgen dat u de gehele databasetransactie is voltooid, of die de hele transactie wordt hersteld.


#### <a name="other-considerations-for-retry"></a>Meer informatie over het opnieuw


- Een batch-programma dat automatisch wordt gestart na werkuren en die worden voltooid voordat ochtend, vast helemaal geduld met lang tijdsintervallen tussen de herhalingspogingen.


- Het programma voor een gebruikersinterface moet account voor de menselijke feit te geven, na het te lang wachten.
 - De oplossing moet echter niet worden opnieuw om de paar seconden, omdat dit beleid grote het systeem met aanvragen hoeveelheden kan.


#### <a name="interval-increase-between-retries"></a>Grotere interval tussen nieuwe pogingen



Het is raadzaam dat u 5 seconden voordat u uw eerste opnieuw uitstellen. Opnieuw na een vertraging korter dan 5 seconden risico's bedolven onder de cloudservice. Voor elke opeenvolgende pogingen de vertraging groter de sterk toe, moet worden gemaakt met een maximum van 60 seconden.

Een discussie *blokkeren van de periode* voor clients die gebruikmaken van ADO.NET is beschikbaar in [SQL Server-verbinding groepsgewijze (ADO.NET)](http://msdn.microsoft.com/library/8xx3tyca.aspx).

U kunt ook een maximum aantal pogingen instellen voordat zelf door het programma is beëindigd.


#### <a name="code-samples-with-retry-logic"></a>Voorbeelden van de code met opnieuw logica


Codevoorbeelden met opnieuw logica, op verschillende talen, zijn beschikbaar:

- [Bibliotheken van de verbinding voor SQL-Database en SQL Server](sql-database-libraries.md)


<a id="k-test-retry-logic" name="k-test-retry-logic"></a>

#### <a name="test-your-retry-logic"></a>Test uw logica opnieuw


Als u wilt testen uw logica opnieuw, moet u simuleren of een fout veroorzaken dan kunnen worden gecorrigeerd terwijl het programma nog actief is.


##### <a name="test-by-disconnecting-from-the-network"></a>Test door het netwerk verbreekt


Een manier die u kunt de logica opnieuw testen is de clientcomputer met het netwerk verbreken, terwijl het programma wordt uitgevoerd. De fout is:

- **SqlException.Number** = 11001
- Bericht: ' geen host is onbekend'


Als onderdeel van de eerste nieuwe pogingen, kan uw programma de spelfout corrigeren en op te lossen.


Als u dit praktische, verwijder u uw computer via het netwerk voordat u uw programma start. Het programma herkent vervolgens een parameter runtime waardoor het programma kan worden:

1. Tijdelijk 11001 toevoegen aan de lijst met fouten u rekening moet houden als tijdelijk.
2. De eerste verbinding zoals u gewend bent proberen.
3. Nadat de fout is opgetreden, moet u 11001 verwijderen uit de lijst.
4. Een melding dat de gebruiker moet de computer op het netwerk aansluiten weergeven.
 - Plaats de aanwijzer verder worden uitgevoerd met behulp van de **Console.ReadLine** -methode of een dialoogvenster met de knop OK. De gebruiker op de Enter-toets drukt nadat de computer is aangesloten op het netwerk.
5. Opnieuw verbinding probeert te, success verwacht.


##### <a name="test-by-misspelling-the-database-name-when-connecting"></a>Test door de databasenaam spelfout wanneer u verbinding maakt


Het programma kan de gebruikersnaam vóór de eerste poging opzettelijk fout hebt gespeld. De fout is:

- **SqlException.Number** = 18456
- Bericht: 'aanmelden is mislukt voor gebruiker 'WRONG_MyUserName'."


Als onderdeel van de eerste nieuwe pogingen, kan uw programma de spelfout corrigeren en op te lossen.


Als u dit praktische, kan het programma een runtime-parameter waardoor het programma kan worden herkend:

1. Tijdelijk 18456 toevoegen aan de lijst met fouten u rekening moet houden als tijdelijk.
2. Voeg opzettelijk 'WRONG_' toe aan de gebruikersnaam in te voeren.
3. Nadat de fout is opgetreden, moet u 18456 verwijderen uit de lijst.
4. Verwijder 'WRONG_' uit de gebruikersnaam in te voeren.
5. Opnieuw verbinding probeert te, success verwacht.

<a id="net-sqlconnection-parameters-for-connection-retry" name="net-sqlconnection-parameters-for-connection-retry"></a>

### <a name="net-sqlconnection-parameters-for-connection-retry"></a>.NET SqlConnection parameters voor het opnieuw verbinding


Als uw clientprogramma een verbinding met Azure SQL-Database maakt met behulp van .NET Framework klasse **System.Data.SqlClient.SqlConnection**, moet u .NET 4.6.1 of hoger zodat u van de functie van de opnieuw verbinding gebruikmaken kunt. Details van de functie zijn [hier](http://go.microsoft.com/fwlink/?linkid=393996).


<!--
2015-11-30, FwLink 393996 points to dn632678.aspx, which links to a downloadable .docx related to SqlClient and SQL Server 2014.
-->


Als u de [verbindingsreeks](http://msdn.microsoft.com/library/System.Data.SqlClient.SqlConnection.connectionstring.aspx) voor het object **SqlConnection** samenstelt, moet u de waarden tussen de volgende parameters coördineren:

- ConnectRetryCount &nbsp; &nbsp; *(de standaardinstelling is 1. Bereik is 0 tot en met 255.)*
- ConnectRetryInterval &nbsp; &nbsp; *(standaard is 1 seconde. Bereik is 1 tot en met 60.)*
- Time-out &nbsp; &nbsp; *(standaardwaarde is 15 seconden. Bereik is 0 tot en met 2147483647)*


De door u gekozen-waarden moet specifiek, de volgende gelijke waar:

- Time-out = ConnectRetryCount * ConnectionRetryInterval

Bijvoorbeeld, als het aantal = 3 en interval = 10 seconden, een-out van alleen 29 seconden niet helemaal, het systeem voldoende tijd vrij voor de 3e en definitief opnieuw op verbinding maken krijgt: 29 < 3 * 10.

<a id="connection-versus-command" name="connection-versus-command"></a>

### <a name="connection-versus-command"></a>Verbinding versus opdracht


De parameters **ConnectRetryCount** en **ConnectRetryInterval** laat uw **SqlConnection** object verbinding maken met bewerking opnieuw uitvoeren zonder vertellen of proberen alles uit uw programma, zoals het besturingselement aan uw programma wordt geretourneerd. De nieuwe pogingen kunnen zich voordoen in de volgende situaties:

- mySqlConnection.Open methode gesprek
- mySqlConnection.Execute methode gesprek

Er is een geldt de volgende werkwijze. Als een tijdelijke fout optreedt terwijl de *query* wordt uitgevoerd, uw **SqlConnection** -object heeft geen verbinding maken met bewerking opnieuw uitvoeren en deze zeker is niet opnieuw uw query. Echter gecontroleerd **SqlConnection** zeer snel de verbinding voor uw query voor uitvoering verzenden. Als de snelle controleren worden gedetecteerd een verbindingsprobleem, pogingen **SqlConnection** de connect-bewerking. Als de poging is geslaagd, wordt u query voor uitvoering verzonden.


#### <a name="should-connectretrycount-be-combined-with-application-retry-logic"></a>Moeten ConnectRetryCount worden gecombineerd met de toepassing opnieuw logica?

Stel dat uw toepassing robuuste aangepaste opnieuw logica heeft. Deze mogelijk probeer opnieuw verbinding maken met 4 keer. Als u **ConnectRetryInterval** en **ConnectRetryCount** = 3 aan uw verbindingsreeks, verhoogt u het aantal nieuwe pogingen tot en met 4 * 3 = 12 pogingen. U mogelijk niet van plan bent die een groot aantal pogingen.

<a id="a-connection-connection-string" name="a-connection-connection-string"></a>

## <a name="connections-to-azure-sql-database"></a>Verbindingen met Azure SQL-Database

<a id="c-connection-string" name="c-connection-string"></a>

### <a name="connection-connection-string"></a>Verbinding: De verbindingsreeks


De verbindingsreeks nodig om verbinding te maken met Azure SQL-Database verschilt enigszins van de tekenreeks om verbinding te maken met Microsoft SQL Server. U kunt de verbindingsreeks kopiëren voor de database van de [Azure-Portal](https://portal.azure.com/).


[AZURE.INCLUDE [sql-database-include-connection-string-20-portalshots](../../includes/sql-database-include-connection-string-20-portalshots.md)]


<a id="b-connection-ip-address" name="b-connection-ip-address"></a>

### <a name="connection-ip-address"></a>Verbinding: IP-adres


U moet de SQL-databaseserver accepteer mededeling van het IP-adres van de computer waarop uw clientprogramma configureren. U doen dit door de firewallinstellingen via de [Portal van Azure](https://portal.azure.com/)te bewerken.


Als u voor het configureren van het IP-adres vergeet, mislukt het programma met een handige foutbericht wordt weergegeven waar het benodigde IP-adres staat.


[AZURE.INCLUDE [sql-database-include-ip-address-22-v12portal](../../includes/sql-database-include-ip-address-22-v12portal.md)]


Zie voor meer informatie: [hoe: firewallinstellingen configureren op SQL-Database](sql-database-configure-firewall-settings.md)


<a id="c-connection-ports" name="c-connection-ports"></a>

### <a name="connection-ports"></a>Verbinding: poorten


Meestal hoeft u alleen om ervoor te zorgen dat poort 1433 voor uitgaande communicatie, klik op de computer waarop u clientprogramma geopend is.


Bijvoorbeeld wanneer uw clientprogramma wordt gehost op een Windows-computer, kunt het Windows Firewall op de host u poort 1433 openen:


1. Open het Configuratiescherm
2. &gt;Alle Configuratiescherm-onderdelen
3. &gt;Windows Firewall
4. &gt;Geavanceerde instellingen
5. &gt;Uitgaande regels
6. &gt;Acties
7. &gt;Nieuwe regel


Als wordt uw clientprogramma wordt gehost op een Azure virtuele machine (VM), moet u lezen:<br/>[Poorten voorbij 1433 voor ADO.NET 4.5 en SQL-Database V12](sql-database-develop-direct-route-ports-adonet-v12.md).


Zie voor achtergrondinformatie over cofiguration poorten en IP-adres,: [Azure SQL Database-firewall](sql-database-firewall-configure.md)


<a id="d-connection-ado-net-4-5" name="d-connection-ado-net-4-5"></a>

### <a name="connection-adonet-461"></a>Verbinding: ADO.NET 4.6.1


Als uw programma ADO.NET-klassen als **System.Data.SqlClient.SqlConnection** verbinding maken met Azure SQL-Database, wordt aangeraden u .NET Framework versie 4.6.1 gebruiken of hoger.


ADO.NET 4.6.1:

- Voor Azure SQL-Database is het verbeterde betrouwbaarheid wanneer u een verbinding met de methode **SqlConnection.Open** opent. De methode **Open** bevat nu aanbevolen hoeveelheid opnieuw regelingen in antwoord op tijdelijke fouten, voor bepaalde fouten binnen de time-out van verbinding.
- Ondersteunt groepsgewijze. Dit geldt ook voor een efficiënte verificatie die het connection-object het programma hebt optreedt.



Wanneer u een connection-object uit een verbindingsgroep gebruikt, wordt u aangeraden dat uw programma de verbinding tijdelijk sluiten als deze niet onmiddellijk gebruiken. Een verbinding opnieuw te openen is niet duur de manier waarop u een nieuwe verbinding is.


Als u ADO.NET 4.0 werkt of eerder, we raden u een upgrade uitvoeren naar de meest recente ADO.NET.

- Vanaf November 2015 verlengt kunt u [ADO.NET 4.6.1 downloaden](http://blogs.msdn.com/b/dotnet/archive/2015/11/30/net-framework-4-6-1-is-now-available.aspx).


<a id="e-diagnostics-test-utilities-connect" name="e-diagnostics-test-utilities-connect"></a>

## <a name="diagnostics"></a>Diagnostische hulpprogramma 's

<a id="d-test-whether-utilities-can-connect" name="d-test-whether-utilities-can-connect"></a>

### <a name="diagnostics-test-whether-utilities-can-connect"></a>Diagnostische hulpprogramma's: Test of hulpprogramma's kunnen worden verbonden


Als uw programma kunt verbinden met Azure SQL-Database is verbroken, is één diagnostische optie om te proberen om te communiceren met een programma hulpprogramma. In het ideale geval zou het hulpprogramma verbinding maken met behulp van dezelfde bibliotheek dat het programma gebruikt.


Op een Windows-computer, kunt u proberen deze hulpprogramma's:

- SQL Server Management Studio (ssms.exe), waarmee verbinding met ADO.NET.
- Sqlcmd.exe verbinding maakt met behulp van [ODBC](http://msdn.microsoft.com/library/jj730308.aspx).


Zodra u verbinding hebt, test u of een korte selecteert u SQL-query werkt.


<a id="f-diagnostics-check-open-ports" name="f-diagnostics-check-open-ports"></a>

### <a name="diagnostics-check-the-open-ports"></a>Diagnostische hulpprogramma's: Controleer de open poorten


Stel dat u vermoedt dat verbindingen worden verbroken door problemen met de poort. U kunt een hulpprogramma voor het rapporten op de poortconfiguraties uitvoeren op uw computer.


Klik op Linux kunnen het handig zijn de volgende hulpprogramma's:

- `netstat -nap`
- `nmap -sS -O 127.0.0.1`
 - (De Voorbeeldwaarde als u uw IP-adres wilt wijzigen).


In Windows de [PortQry.exe](http://www.microsoft.com/download/details.aspx?id=17148) kan het handig zijn hulpprogramma. Hier volgt een voorbeeld uitvoering dat een query wordt uitgevoerd de situatie poort op een Azure SQL Database-server en die is uitgevoerd op een laptopcomputer:


```
[C:\Users\johndoe\]
>> portqry.exe -n johndoesvr9.database.windows.net -p tcp -e 1433

Querying target system called:
 johndoesvr9.database.windows.net

Attempting to resolve name to IP address...
Name resolved to 23.100.117.95

querying...
TCP port 1433 (ms-sql-s service): LISTENING

[C:\Users\johndoe\]
>>
```


<a id="g-diagnostics-log-your-errors" name="g-diagnostics-log-your-errors"></a>

### <a name="diagnostics-log-your-errors"></a>Diagnostische hulpprogramma's: Meld u aan uw fouten


Een tussentijds probleem is soms best dat door de detectie van een algemene patroon via dagen of weken.


De klant kunt helpen bij een diagnose door het vastleggen van alle fouten die worden aangetroffen. U mogelijk de logboekvermeldingen correlatie met foutgegevens die Azure SQL-Database intern zelf Logboeken.


Enterprise bibliotheek 6 (EntLib60) biedt beheerde klassen om u te helpen met de logboekregistratie:

- [5 - zo eenvoudig als het een logboek vallen: de blokkering van de toepassing logboekregistratie gebruiken](http://msdn.microsoft.com/library/dn440731.aspx)


<a id="h-diagnostics-examine-logs-errors" name="h-diagnostics-examine-logs-errors"></a>

### <a name="diagnostics-examine-system-logs-for-errors"></a>Diagnostische hulpprogramma's: Bekijk de logboekbestanden systeem op fouten


Hier volgen enkele Transact-SQL SELECT-instructies die querylogboeken van fout en andere informatie.


| Query van het logboek | Beschrijving |
| :-- | :-- |
| `SELECT e.*`<br/>`FROM sys.event_log AS e`<br/>`WHERE e.database_name = 'myDbName'`<br/>`AND e.event_category = 'connectivity'`<br/>`AND 2 >= DateDiff`<br/>&nbsp;&nbsp;`(hour, e.end_time, GetUtcDate())`<br/>`ORDER BY e.event_category,`<br/>&nbsp;&nbsp;`e.event_type, e.end_time;` | De weergave [sys.event_log](http://msdn.microsoft.com/library/dn270018.aspx) biedt informatie over afzonderlijke gebeurtenissen, waaronder enkele die ertoe tijdelijke fouten of connectivity fouten leiden kunnen.<br/><br/>U kunt de **start_time** of **end_time** waarden in het ideale geval relateren met informatie over wanneer uw clientprogramma problemen ervaren.<br/><br/>**TIP:** U moet verbinding maken met **de hoofddatabase uitvoeren** . |
| `SELECT c.*`<br/>`FROM sys.database_connection_stats AS c`<br/>`WHERE c.database_name = 'myDbName'`<br/>`AND 24 >= DateDiff`<br/>&nbsp;&nbsp;`(hour, c.end_time, GetUtcDate())`<br/>`ORDER BY c.end_time;` | De weergave [sys.database_connection_stats](http://msdn.microsoft.com/library/dn269986.aspx) biedt geaggregeerde aantallen gebeurtenistypen, voor meer diagnostische gegevens.<br/><br/>**TIP:** U moet verbinding maken met **de hoofddatabase uitvoeren** . |

<a id="d-search-for-problem-events-in-the-sql-database-log" name="d-search-for-problem-events-in-the-sql-database-log"></a>

### <a name="diagnostics-search-for-problem-events-in-the-sql-database-log"></a>Diagnostische hulpprogramma's: Zoeken voor probleem gebeurtenissen in het logboek SQL-Database


U kunt zoeken naar de vermeldingen over probleem gebeurtenissen in het logboek van Azure SQL-Database. Probeer de volgende Transact-SQL SELECT-instructie in **de hoofddatabase** :


```
SELECT
   object_name
  ,CAST(f.event_data as XML).value
      ('(/event/@timestamp)[1]', 'datetime2')                      AS [timestamp]
  ,CAST(f.event_data as XML).value
      ('(/event/data[@name="error"]/value)[1]', 'int')             AS [error]
  ,CAST(f.event_data as XML).value
      ('(/event/data[@name="state"]/value)[1]', 'int')             AS [state]
  ,CAST(f.event_data as XML).value
      ('(/event/data[@name="is_success"]/value)[1]', 'bit')        AS [is_success]
  ,CAST(f.event_data as XML).value
      ('(/event/data[@name="database_name"]/value)[1]', 'sysname') AS [database_name]
FROM
  sys.fn_xe_telemetry_blob_target_read_file('el', null, null, null) AS f
WHERE
  object_name != 'login_event'  -- Login events are numerous.
  and
  '2015-06-21' < CAST(f.event_data as XML).value
        ('(/event/@timestamp)[1]', 'datetime2')
ORDER BY
  [timestamp] DESC
;
```


#### <a name="a-few-returned-rows-from-sysfnxetelemetryblobtargetreadfile"></a>Een aantal geretourneerde rijen uit sys.fn_xe_telemetry_blob_target_read_file


Is de optie volgende wat een geretourneerde rij als volgt uitzien. Het null-waarden weergegeven zijn vaak niet null zijn in andere rijen.


```
object_name                   timestamp                    error  state  is_success  database_name

database_xml_deadlock_report  2015-10-16 20:28:01.0090000  NULL   NULL   NULL        AdventureWorks
```


<a id="l-enterprise-library-6" name="l-enterprise-library-6"></a>

## <a name="enterprise-library-6"></a>Bibliotheek voor Enterprise 6


Enterprise bibliotheek 6 (EntLib60) is een kader van .NET klassen waarmee u implementeren robuuste clients van cloudservices, een van de plaats waar de Azure SQL Database-service is. Onderwerpen die specifiek voor elk gebied waarin EntLib60 kunt helpen eerste website, kunt u vinden:

- [Bibliotheek voor Enterprise 6 – April 2013](http://msdn.microsoft.com/library/dn169621%28v=pandp.60%29.aspx)


Opnieuw logica voor het afhandelen van tijdelijke fouten is een gebied waarin EntLib60 kunt helpen:

- [4 - perseverance, geheim van alle successen: de blokkering van de toepassing tijdelijke foutenstructuuranalyse Afhandeling gebruiken](http://msdn.microsoft.com/library/dn440719%28v=pandp.60%29.aspx)


> [AZURE.NOTE] De broncode voor EntLib60 is beschikbaar voor openbare [downloaden](http://go.microsoft.com/fwlink/p/?LinkID=290898). Microsoft heeft geen plannen om verder functie-updates of onderhoud updates naar EntLib.

<a id="entlib60-classes-for-transient-errors-and-retry" name="entlib60-classes-for-transient-errors-and-retry"></a>

### <a name="entlib60-classes-for-transient-errors-and-retry"></a>EntLib60 klassen voor tijdelijke fouten en probeer het opnieuw


De volgende EntLib60 klassen zijn met name handig om opnieuw logica. Alles dit zijn of verdere onder de naamruimte **Microsoft.Practices.EnterpriseLibrary.TransientFaultHandling**:

*In de naamruimte* *Microsoft.Practices.EnterpriseLibrary.TransientFaultHandling* *:*

- **RetryPolicy** class
 - Methode **ExecuteAction**


- **ExponentialBackoff** class


- **SqlDatabaseTransientErrorDetectionStrategy** class


- **ReliableSqlConnection** class
 - **ExecuteCommand** methode


In de naamruimte **Microsoft.Practices.EnterpriseLibrary.TransientFaultHandling.TestSupport**:

- **AlwaysTransientErrorDetectionStrategy** class

- **NeverTransientErrorDetectionStrategy** class


Hier vindt u koppelingen naar informatie over EntLib60:

- Gratis [downloaden van het adresboek: handleiding voor ontwikkelaars naar Microsoft Enterprise-bibliotheek, 2e Edition](http://www.microsoft.com/download/details.aspx?id=41145)

- Aanbevolen procedures: [algemene richtlijnen probeer](../best-practices-retry-general.md) een uitstekende informatie over het opnieuw logica heeft.

- NuGet downloaden van de [Bibliotheek in de Enterprise - toepassing blokkeren 6.0 tijdelijke foutenstructuuranalyse verwerken](http://www.nuget.org/packages/EnterpriseLibrary.TransientFaultHandling/)

<a id="entlib60-the-logging-block" name="entlib60-the-logging-block"></a>

### <a name="entlib60-the-logging-block"></a>EntLib60: De logboekregistratie blokkeren


- De blokkering logboekregistratie is een zeer flexibele en configureerbare oplossing waarmee u kunt:
 - Maken en opslaan van berichten in het logboek in een groot aantal locaties.
 - Categoriseren en filteren van berichten.
 - Contextuele informatie die is handig voor foutopsporing en tracering en voor controle en algemene logboekregistratie vereisten verzamelen.


- De blokkering logboekregistratie abstracts de functionaliteit voor logboekregistratie van de bestemming log zodat de toepassingscode consistente, ongeacht de locatie en het type doeltoepassing logboekregistratie archief is.


Zie voor meer informatie: [5: als gemakkelijk als vallen uit een logboek: met de toepassing logboekregistratie blok](https://msdn.microsoft.com/library/dn440731%28v=pandp.60%29.aspx)

<a id="entlib60-istransient-method-source-code" name="entlib60-istransient-method-source-code"></a>

### <a name="entlib60-istransient-method-source-code"></a>EntLib60 IsTransient methode-broncode


Vervolgens is uit de klasse **SqlDatabaseTransientErrorDetectionStrategy** , de C#-broncode voor de methode **IsTransient** . De broncode verduidelijkt welke fouten werden beschouwd als tijdelijke en tot slot opnieuw vanaf April 2013.

Talloze **//comment** regels hebt verwijderd uit deze kopiëren naar de leesbaarheid te benadrukken.


```
public bool IsTransient(Exception ex)
{
  if (ex != null)
  {
    SqlException sqlException;
    if ((sqlException = ex as SqlException) != null)
    {
      // Enumerate through all errors found in the exception.
      foreach (SqlError err in sqlException.Errors)
      {
        switch (err.Number)
        {
            // SQL Error Code: 40501
            // The service is currently busy. Retry the request after 10 seconds.
            // Code: (reason code to be decoded).
          case ThrottlingCondition.ThrottlingErrorNumber:
            // Decode the reason code from the error message to
            // determine the grounds for throttling.
            var condition = ThrottlingCondition.FromError(err);

            // Attach the decoded values as additional attributes to
            // the original SQL exception.
            sqlException.Data[condition.ThrottlingMode.GetType().Name] =
              condition.ThrottlingMode.ToString();
            sqlException.Data[condition.GetType().Name] = condition;

            return true;

          case 10928:
          case 10929:
          case 10053:
          case 10054:
          case 10060:
          case 40197:
          case 40540:
          case 40613:
          case 40143:
          case 233:
          case 64:
            // DBNETLIB Error Code: 20
            // The instance of SQL Server you attempted to connect to
            // does not support encryption.
          case (int)ProcessNetLibErrorCode.EncryptionNotSupported:
            return true;
        }
      }
    }
    else if (ex is TimeoutException)
    {
      return true;
    }
    else
    {
      EntityException entityException;
      if ((entityException = ex as EntityException) != null)
      {
        return this.IsTransient(entityException.InnerException);
      }
    }
  }

  return false;
}
```


## <a name="next-steps"></a>Volgende stappen

- Voor het oplossen van andere veelvoorkomende problemen van de Azure SQL Database-verbinding, gaat u naar [problemen verbinding oplossen met Azure SQL-Database](sql-database-troubleshoot-common-connection-issues.md).

- [SQL Server groepsgewijze (ADO.NET)](http://msdn.microsoft.com/library/8xx3tyca.aspx)


- [*Bezig met opnieuw proberen* is dat een 2.0 Apache licentie algemene Bezig met opnieuw bibliotheek, geschreven in **Python**, om te vereenvoudigen gedrag opnieuw toe te voegen op vrijwel elk element.](https://pypi.python.org/pypi/retrying)
