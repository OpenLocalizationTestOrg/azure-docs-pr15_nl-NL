<properties
    pageTitle="Foutcodes SQL - database verbindingsfout | Microsoft Azure"
    description="Meer informatie over foutcodes SQL voor SQL-Database client-toepassingen, zoals veelvoorkomende fouten in database-verbinding, problemen met de database kopiëren en algemene fouten. "
    keywords="SQL-foutcode, access sql, database verbindingsfout, sql-foutcodes"
    services="sql-database"
    documentationCenter=""
    authors="annemill"
    manager="jhubbard"
    editor="" />


<tags
    ms.service="sql-database"
    ms.workload="drivers"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="07/12/2016"
    ms.author="annemill"/>


# <a name="sql-error-codes-for-sql-database-client-applications-database-connection-error-and-other-issues"></a>SQL-foutcodes voor SQL-Database-clienttoepassingen: Database verbindingsfout en andere problemen


<!--
Old Title on MSDN:  Error Messages (Azure SQL Database)
ShortId on MSDN:  ff394106.aspx
Dx 4cff491e-9359-4454-bd7c-fb72c4c452ca
-->


In dit artikel bevat een overzicht van de SQL-foutcodes voor SQL-Database-clienttoepassingen, inclusief database-verbindingsfouten, tijdelijke fouten (ook wel tijdelijke fouten genoemd), resource beheermodel fouten, problemen met de database kopiëren, elastische van toepassingen en andere fouten. De meeste categorieën zijn bepaalde met Azure SQL-Database en niet van toepassing op Microsoft SQL Server.

## <a name="database-connection-errors-transient-errors-and-other-temporary-errors"></a>Database-verbindingsfouten, tijdelijke fouten en andere tijdelijke fouten

De volgende tabel wordt de SQL-foutcodes voor verbinding verlies fouten en andere tijdelijke fouten die kunnen optreden wanneer uw toepassing probeert te krijgen tot SQL-Database. Zie voor aan de slag zelfstudies over het verbinding maken met Azure SQL-Database, [verbinding maken met Azure SQL-Database](sql-database-libraries.md).

### <a name="most-common-database-connection-errors-and-transient-fault-errors"></a>Meest voorkomende fouten in database-verbinding en tijdelijke foutenstructuuranalyse

De Azure infrastructuur heeft de mogelijkheid om te servers dynamisch configureren wanneer dik werkbelasting zich in de SQL-Database-service voordoen.  Dit dynamische gedrag kan ertoe leiden dat uw clientprogramma kwijtraken van de verbinding met SQL-Database. Dit soort foutstatus is een *tijdelijke foutenstructuuranalyse*genoemd.

Als uw clientprogramma opnieuw logica heeft, kan deze probeert te opnieuw verbinding te maken nadat u de tijd tijdelijk foutenstructuuranalyse zelf worden gecorrigeerd hebt.  Het is raadzaam dat u 5 seconden voordat u uw eerste opnieuw uitstellen. Opnieuw na een vertraging korter dan 5 seconden risico's bedolven onder de cloudservice. Voor elke opeenvolgende pogingen de vertraging groter de sterk toe, moet worden gemaakt met een maximum van 60 seconden.

Tijdelijke veroorzaakt fouten bestandenlijst meestal als een van de volgende foutberichten vanuit uw programma:

- Database < db_name > op server < Azure_instance > is momenteel niet beschikbaar. Probeer de verbinding later. Als het probleem zich blijft voordoen, neem contact op met de klantenondersteuning en geef de sessie tracering-ID van < session_id >

- Database < db_name > op server < Azure_instance > is momenteel niet beschikbaar. Probeer de verbinding later. Als het probleem zich blijft voordoen, neem contact op met de klantenondersteuning en geef de sessie tracering-ID van < session_id >. (Microsoft SQL Server, fout: 40613)

- Een bestaande verbinding is verbroken door de externe host.

- System.Data.Entity.Core.EntityCommandExecutionException: Er is een fout opgetreden tijdens het uitvoeren van de opdracht definitie. Zie de binnenste uitzondering voor meer informatie. ---> System.Data.SqlClient.SqlException: een transport niveau-fout opgetreden bij het ontvangen van de resultaten van de server. (provider: sessieprovider, fout: 19 - fysieke verbinding kan niet worden gebruikt)

Zie voor voorbeelden van programmacode van opnieuw logica:

- [Bibliotheken van de verbinding voor SQL-Database en SQL Server](sql-database-libraries.md) 
- [Acties verbindingsfouten en tijdelijke oplossing in SQL-Database](sql-database-connectivity-issues.md)

Een discussie *blokkeren van de periode* voor clients die gebruikmaken van ADO.NET is beschikbaar in [SQL Server-verbinding groepsgewijze (ADO.NET)](http://msdn.microsoft.com/library/8xx3tyca.aspx).

### <a name="transient-fault-error-codes"></a>Foutcodes tijdelijke foutenstructuuranalyse

De volgende fouten zijn tijdelijke en opnieuw moeten worden uitgevoerd in de toepassingslogica 

| Foutcode | Ernst | Beschrijving |
| ---: | ---: | :--- |
| 4060 | 16 | Database niet openen "% & #x2a; ls" aangevraagd door de login. De aanmelding is mislukt. |
|40197|17|Het is een fout bij het verwerken van uw aanvraag kunt invullen aangetroffen. Probeer het opnieuw. Foutcode %d.<br/><br/>U ontvangt deze fout wanneer de service niet beschikbaar vanwege software of hardware-upgrades, problemen met de hardware of andere problemen die failover is. De foutcode (%d) die zijn ingesloten in het bericht van fout 40197 biedt extra informatie over het soort mislukt of failover die zijn aangebracht. Enkele voorbeelden van de fout codes zijn ingesloten in het bericht van fout 40197 zijn 40020, 40143 40166 en 40540.<br/><br/>Opnieuw verbinding maken met uw SQL-databaseserver, wordt u automatisch verbinding met een correct kopie van uw database. Uw toepassing moet 40197, het logboek met publicatiefouten de ingesloten foutcode (%d) in het bericht voor probleemoplossing onderschept en opnieuw verbinding te maken met SQL-Database, totdat de resources beschikbaar zijn en de verbinding tot stand is gebracht opnieuw.|
|40501|20|De service is bezet. Probeer het verzoek na 10 seconden. Incident-ID: %ls. Code: %d.<br/><br/>*Notitie:* Zie voor meer informatie:<br/>• [Azure SQL-Database resource limieten](sql-database-resource-limits.md).
|40613|17|Database '% & #x2a; ls' op server '% & #x2a; ls' is momenteel niet beschikbaar. Probeer de verbinding later. Als het probleem zich blijft voordoen, neem contact op met de klantenondersteuning en geef de sessie tracering-ID van '% & #x2a; ls'.|
|49918|16|Aanvraag verwerken niet. Onvoldoende bronnen aanvraag is verwerkt.<br/><br/>De service is bezet. Probeer de aanvraag later opnieuw. |
|49919|16|Proces kan geen aanvraag maken of bijwerken. Te veel bewerkingen maken of bijwerken in voortgang voor abonnement "% ld".<br/><br/>De service is bezet verwerking van meerdere maken of bijwerken van aanvragen voor uw abonnement of de server. Aanvragen worden momenteel geblokkeerd voor optimalisatie van de resource. Query [sys.dm_operation_status](https://msdn.microsoft.com/library/dn270022.aspx) voor wachtende bewerkingen. Wacht totdat in behandeling maken of bijwerken aanvragen voltooid zijn of verwijderen van een van uw openstaande verzoeken en probeer het later opnieuw uw aanvraag kunt invullen. |
|49920|16|Aanvraag verwerken niet. Te veel bewerkingen uitgevoerd voor abonnement "% ld".<br/><br/>De service is bezet verwerkt meerdere aanvragen voor dit abonnement. Aanvragen worden momenteel geblokkeerd voor optimalisatie van de resource. Query [sys.dm_operation_status](https://msdn.microsoft.com/library/dn270022.aspx) voor bewerkingsstatus. Wachten totdat aanvragen in behandeling zijn voltooid of verwijderen van een van uw openstaande verzoeken en probeer het later opnieuw uw aanvraag kunt invullen. |

## <a name="database-copy-errors"></a>Fouten in de database kopiëren

De volgende fouten ziet kunnen zich voordoen bij het kopiëren van een database in Azure SQL-Database. Zie [een Azure SQL-Database kopiëren](sql-database-copy.md)voor meer informatie.


|Foutcode|Ernst|Beschrijving|
|---:|---:|:---|
|40635|16|Client met IP-adres '% & #x2a; ls' tijdelijk is uitgeschakeld.|
|40637|16|Maak databasekopie is uitgeschakeld.|
|40561|16|Databasekopie is mislukt. De bron- of doellijst database bestaat niet.|
|40562|16|Databasekopie is mislukt. De brondatabase is gegaan.|
|40563|16|Databasekopie is mislukt. De doeldatabase is gegaan.|
|40564|16|Databasekopie is mislukt vanwege een interne fout. Target database weghalen en probeer het opnieuw.|
|40565|16|Databasekopie is mislukt. Niet meer dan 1 gelijktijdige database kopiëren uit dezelfde bron is toegestaan. Target database weghalen en probeer het later opnieuw.|
|40566|16|Databasekopie is mislukt vanwege een interne fout. Target database weghalen en probeer het opnieuw.|
|40567|16|Databasekopie is mislukt vanwege een interne fout. Target database weghalen en probeer het opnieuw.|
|40568|16|Databasekopie is mislukt. Brondatabase heeft niet meer beschikbaar. Target database weghalen en probeer het opnieuw.|
|40569|16|Databasekopie is mislukt. Doeldatabase is niet meer beschikbaar. Target database weghalen en probeer het opnieuw.|
|40570|16|Databasekopie is mislukt vanwege een interne fout. Target database weghalen en probeer het later opnieuw.|
|40571|16|Databasekopie is mislukt vanwege een interne fout. Target database weghalen en probeer het later opnieuw.|

## <a name="resource-governance-errors"></a>Resource beheermodel fouten

De volgende fouten worden veroorzaakt door overtollige gebruiken van resources tijdens het werken met Azure SQL-Database. Bijvoorbeeld:

- Een transactie is te lang geopend.
- Een transactie is te veel vergrendelingen.
- Een toepassing is er te veel geheugen.
- Een toepassing te veel verbruikt `TempDb` ruimte.

Verwante onderwerpen:

* Meer gedetailleerde informatie vindt u hier: [Azure SQL-Database resource limieten](sql-database-resource-limits.md)

|Foutcode|Ernst|Beschrijving|
|---:|---:|:---|
|10928|20|Resource-ID: %d. De limiet %s voor de database is %d en is bereikt. Zie [http://go.microsoft.com/fwlink/?LinkId=267637](http://go.microsoft.com/fwlink/?LinkId=267637)voor meer informatie.<br/><br/>De Resource-ID wordt aangegeven dat de resource die de limiet is bereikt. Voor werk-threads, de Resource-ID = 1. Voor de Resource-ID-sessies = 2.<br/><br/>*Notitie:* Zie voor meer informatie over deze fout en hoe u deze kunt oplossen:<br/>• [Azure SQL-Database resource limieten](sql-database-resource-limits.md). |
|10929|20|Resource-ID: %d. De minimale garantie %s is %d, maximumlimiet %d is en de huidige gebruik voor de database is %d. De server is echter momenteel bezet voor de ondersteuning van aanvragen die groter zijn dan %d voor deze database. Zie [http://go.microsoft.com/fwlink/?LinkId=267637](http://go.microsoft.com/fwlink/?LinkId=267637)voor meer informatie. Anders, probeer het later opnieuw.<br/><br/>De Resource-ID wordt aangegeven dat de resource die de limiet is bereikt. Voor werk-threads, de Resource-ID = 1. Voor de Resource-ID-sessies = 2.<br/><br/>*Notitie:* Zie voor meer informatie over deze fout en hoe u deze kunt oplossen:<br/>• [Azure SQL-Database resource limieten](sql-database-resource-limits.md).|
|40544|20|De database heeft zijn grootte-quotum bereikt. Gegevens Partition of verwijderen neerzetten indexen of Raadpleeg de documentatie voor mogelijke oplossingen.|
|40549|16|Sessie is beëindigd omdat er een langdurige transactie. Probeer uw transactie verkorten.|
|40550|16|De sessie is beëindigd omdat er te veel vergrendelingen heeft verkregen. Probeer te lezen of wijzigen van minder rijen in één transactie.|
|40551|16|De sessie is beëindigd vanwege overtollige `TEMPDB` gebruik. Probeer de query om te verkleinen van het gebruik van de schijfruimte tijdelijke tabel te wijzigen.<br/><br/>*Tip:* Als u tijdelijke objecten gebruikt, meer ruimte in de `TEMPDB` database door tijdelijke objecten slepen en neerzetten nadat ze niet meer nodig voor de sessie zijn zijn.|
|40552|16|De sessie is beëindigd vanwege overtollige transactie log ruimtegebruik. Probeer minder rijen in één transactie wijzigen.<br/><br/>*Tip:* Als u uitvoeren bulksgewijs ingevoegd gebruik van de `bcp.exe` hulpprogramma of de `System.Data.SqlClient.SqlBulkCopy` klasse, gebruikt u de `-b batchsize` of `BatchSize` opties voor het beperken van het aantal rijen gekopieerd naar de server in elke transactie. Als u opnieuw van een index met maken de `ALTER INDEX` instructie, gebruikt u de `REBUILD WITH ONLINE = ON` optie.|
|40553|16|De sessie is beëindigd vanwege overtollige geheugengebruik. Probeer de query te verwerken minder rijen te wijzigen.<br/><br/>*Tip:* Het aantal te verminderen `ORDER BY` en `GROUP BY` bewerkingen in uw Transact-SQL-code Hiermee reduceert u de geheugenvereisten van uw query.|

## <a name="elastic-pool-errors"></a>Elastische groep fouten

De volgende fouten ziet betrekking hebben op maken en Elastics van toepassingen gebruiken.

| Foutnummer | ErrorSeverity | ErrorFormat | ErrorInserts | ErrorCause | ErrorCorrectiveAction |
| :-- | :-- | :-- | :-- | :-- | :-- |
| 1132 | EX_RESOURCE | De elastische toepassingen heeft de opslaglimiet bereikt. Het gebruik van de opslagruimte voor de elastische van toepassingen niet langer zijn dan (%d) MB. | Elastische ruimte de limiet in MB. | Probeert te schrijven van gegevens met een database als de opslaglimiet van de elastische resourcegroep is bereikt. | Geef Houd rekening met de DTUs van de elastische toepassingen indien mogelijk om te kunnen de opslaglimiet vergroten, verkleinen van de opslag die worden gebruikt door afzonderlijke databases binnen de elastische toepassingen met groter wordende of databases verwijderen uit de elastische groep. |
| 10929 | EX_USER | De minimale garantie %s is %d, maximumlimiet %d is en de huidige gebruik voor de database is %d. De server is echter momenteel bezet voor de ondersteuning van aanvragen die groter zijn dan %d voor deze database. Zie [http://go.microsoft.com/fwlink/?LinkId=267637](http://go.microsoft.com/fwlink/?LinkId=267637) voor hulp. Anders, probeer het later opnieuw. | DTU min per database. DTU max per database | Het totale aantal gelijktijdige werknemers (aanvragen) in alle databases in de groep elastische poging tot het groter is dan de limiet van de. | Geef Houd rekening met de DTUs van de elastische toepassingen indien mogelijk om te verhogen van de limiet voor de werknemer met groter wordende of databases verwijderen uit de elastische groep. |
| 40844 | EX_USER | Database '%ls' op Server '%ls' een '%ls' edition-database in een elastische toepassingen is en geen continue kopie relatie kunt hebben. | naam van de database, database edition, de naam van server | Een opdracht StartDatabaseCopy wordt uitgevoerd voor een niet-premium db in een elastische toepassingen. | Binnenkort beschikbaar |
| 40857 | EX_USER | Elastische van toepassingen niet gevonden voor de server: '%ls', elastische groepsnaam: '%ls'. | naam van de server. naam elastische toepassingen | Opgegeven elastische toepassingen bestaat niet in de opgegeven server. | Geef de naam van een geldige elastische toepassingen. |
| 40858 | EX_USER | Elastische toepassingen '%ls' bestaat al in de server: '%ls' | naam van elastische groep servernaam | Er bestaat al opgegeven elastische toepassingen in de opgegeven logische server. | Geef de naam van nieuwe elastische toepassingen. |
| 40859 | EX_USER | Elastische toepassingen biedt geen ondersteuning voor servicelaag '%ls'. | elastische groep servicelaag | Laag opgegeven service wordt niet ondersteund voor het inrichten van elastische toepassingen. | Geef de juiste edition of laat servicelaag leeg om het gebruik van de standaard-servicelaag. |
| 40860 | EX_USER | Combinatie van elastische groep '%ls' en service doelstelling '%ls' is ongeldig. | de naam van de elastische toepassingen; niveau doelstelling servicenaam | Elastische van toepassingen en service doelstelling samen kan worden opgegeven alleen als doel van de service is opgegeven als 'ElasticPool'. | Geef de juiste combinatie van elastische van toepassingen en service doelstelling. |
| 40861 | EX_USER | De database-editie ' %. *ls kunnen niet worden anders dan de elastische groep service-laag, dat wil zeggen ' %.* ls. | database edition, elastische groep servicelaag | De database-editie verschilt van de laag elastische groep-service. | Geef niet een database-editie dat anders dan de elastische groep servicelaag is.  Houd er rekening mee dat de database-editie niet hoeft te worden opgegeven. |
| 40862 | EX_USER | Elastische groepnaam moet opgeven als het doel van de service elastische groep is opgegeven. | Geen | Elastische groep service doelstelling geeft een elastische van toepassingen niet uniek. | Geef de naam van de elastische toepassingen als het doel van de service elastische van toepassingen gebruiken. |
| 40864 | EX_USER | De DTUs voor de elastische toepassingen moet ten minste (%d) DTUs voor servicelaag ' % * ls. | DTUs voor elastische toepassingen; elastische groep servicelaag. | Wilt de DTUs voor de elastische toepassingen beneden de ondergrens instellen. | Probeer het opnieuw instellen van de DTUs voor de elastische resourcegroep aan ten minste de ondergrens. |
| 40865 | EX_USER | De DTUs voor de elastische van toepassingen niet langer zijn dan (%d) DTUs voor servicelaag ' % * ls. | DTUs voor elastische toepassingen; elastische groep servicelaag. | Wilt de DTUs voor de elastische toepassingen boven de maximale limiet instellen. | Probeer het opnieuw instellen van de DTUs voor de elastische toepassingen op niet groter is dan de maximale limiet. |
| 40867 | EX_USER | Het maximum aantal per database DTU moet zich op minimaal (%d) voor de servicelaag ' % * ls. | DTU max per database. elastische groep servicelaag | Wilt de max DTU per database onder de ondersteunde limiet instellen. | Overweeg het gebruik van de elastische groep servicelaag die ondersteuning biedt voor de gewenste instelling. |
| 40868 | EX_USER | Het maximum aantal per database DTU niet langer zijn dan (%d) voor de servicelaag ' % * ls. | DTU max per database. elastische groep servicelaag. | U probeert om in te stellen van de max DTU per database de ondersteunde limiet overschrijdt. | Overweeg het gebruik van de elastische groep servicelaag die ondersteuning biedt voor de gewenste instelling. |
| 40870 | EX_USER | De min DTU per database niet langer zijn dan (%d) voor de servicelaag ' % * ls. | DTU min per database. elastische groep servicelaag. | U probeert om in te stellen van de min DTU per database de ondersteunde limiet overschrijdt. | Overweeg het gebruik van de elastische groep servicelaag die ondersteuning biedt voor de gewenste instelling. |
| 40873 | EX_USER | Het aantal databases (%d) en DTU min per database (%d) niet langer zijn dan de DTUs van de elastische resourcegroep (%d). | Getal-databases in elastische groep. DTU min per database. DTUs van elastische resourcegroep. | U probeert om op te geven DTU min voor databases in de elastische toepassingen die groter is dan de DTUs van de elastische toepassingen. | Houd rekening met de DTUs van de elastische van toepassingen, vergroten of verkleinen van de min DTU per database, of het aantal databases in de groep elastische verkleinen. |
| 40877 | EX_USER | Een elastische toepassingen kan niet worden verwijderd, tenzij deze geen databases. | Geen | De elastische toepassingen bevat een of meer databases en kunnen daarom niet worden verwijderd. | Verwijder de databases uit de groep elastische om te verwijderen. |
| 40881 | EX_USER | De elastische toepassingen ' % * ls tellen is bereikt database.  De limiet van de database voor de elastische van toepassingen niet langer zijn dan (%d) voor een elastische toepassingen met (%d) DTUs. | Naam van elastische toepassingen; database-limiet van elastische resourcegroep; e DTUs voor resourcegroep. | U probeert te maken of database toevoegen aan elastische groep wanneer de limiet van de database van de elastische resourcegroep is bereikt. | Geef Houd rekening met de DTUs van de elastische toepassingen indien mogelijk om te verhogen van de limiet voor de database met groter wordende of databases verwijderen uit de elastische groep. |
| 40889 | EX_USER | De DTUs of de opslaglimiet voor de elastische toepassingen ' % * ls kunnen niet worden verminderd aangezien die niet voldoende opslagruimte voor de databases opgeven wilt. | De naam van elastische toepassingen. | Wilt de opslaglimiet van de elastische toepassingen onder het gebruik van de opslagruimte verkleinen. | Houd rekening met het gebruik van de opslag van afzonderlijke databases in de groep elastische verkleinen of databases verwijdert uit de groep om u te verkleinen van de DTUs of de opslaglimiet. |
| 40891 | EX_USER | De min DTU per database (%d) niet langer zijn dan de max DTU per database (%d). | DTU min per database. DTU max per database. | U probeert om in te stellen van de min DTU per database die hoger zijn dan de max DTU per database. | Controleer of dat het min DTU per databases niet groter is dan de max DTU per database. |
| TBD | EX_USER | De maximale grootte voor een afzonderlijke database in een groep met elastische niet langer zijn dan de maximale grootte toegestaan door ' % * ls service laag elastische toepassingen. | elastische groep servicelaag | De maximale grootte voor de database overschrijdt de maximale grootte toegestaan door de service-laag elastische toepassingen. | Stel de maximale grootte van de database binnen de grenzen van de maximale grootte toegestaan door de service-laag elastische toepassingen. |

Verwante onderwerpen:

* [Maken van een elastische database-toepassingen (C#)](sql-database-elastic-pool-create-csharp.md) 
* [Beheren een elastische database-toepassingen (C#)](sql-database-elastic-pool-manage-csharp.md). 
* [Een elastische database-toepassingen (PowerShell) maken](sql-database-elastic-pool-create-powershell.md) 
* [Beeldscherm en beheren van een elastische database-toepassingen (PowerShell)](sql-database-elastic-pool-manage-powershell.md).

## <a name="general-errors"></a>Algemene fouten

De volgende fouten ziet vallen niet in een vorige categorieën.

|Foutcode|Ernst|Beschrijving|
|---:|---:|:---|
|15006|16|<AdministratorLogin>is geen geldige naam omdat deze ongeldige tekens bevat.|
|18452|14|Aanmelding is mislukt. De aanmelding afkomstig is van een niet-vertrouwde domein en kunnen niet worden gebruikt met Windows authentication.%. & #x2a; ls (Windows-aanmeldingen worden niet ondersteund in deze versie van SQL Server.)|
|18456|14|Aanmelding voor gebruiker ' % & #x2a;ls'.%. & #x2a; ls %. & #x2a; ls (de aanmelding voor gebruiker "% & #x2a; ls". Wijzigen van het wachtwoord is mislukt. Wachtwoord wijzigen tijdens het aanmelden wordt niet ondersteund in deze versie van SQL Server.)|
|18470|14|Aanmelding voor gebruiker '% & #x2a; ls'. Reden: Het account is disabled.%. & #x2a; ls|
|40014|16|Meerdere databases kunnen niet worden gebruikt in dezelfde transactie.|
|40054|16|Tabellen zonder een gegroepeerde index worden niet ondersteund in deze versie van SQL Server. Een gegroepeerde index maken en probeer het opnieuw.|
|40133|15|Deze bewerking wordt niet ondersteund in deze versie van SQL Server.|
|40506|16|Opgegeven beveiligings-id is ongeldig voor deze versie van SQL Server.|
|40507|16|' % & #x2a; ls kunnen niet worden aangeroepen met parameters in deze versie van SQL Server.|
|40508|16|USE-instructie wordt niet ondersteund als u wilt schakelen tussen databases. Met een nieuwe verbinding verbinding maken met een andere database.|
|40510|16|Instructie '% & #x2a; ls' wordt niet ondersteund in deze versie van SQL Server|
|40511|16|Ingebouwde functie '% & #x2a; ls' wordt niet ondersteund in deze versie van SQL Server.|
|40512|16|Afgeschaft '%ls' wordt niet ondersteund in deze versie van SQL Server.|
|40513|16|Server variabele '% & #x2a; ls' wordt niet ondersteund in deze versie van SQL Server.|
|40514|16|'%ls' wordt niet ondersteund in deze versie van SQL Server.|
|40515|16|Verwijzing naar de naam van de database en/of de server in '% & #x2a; ls' wordt niet ondersteund in deze versie van SQL Server.|
|40516|16|Globale temp objecten worden niet ondersteund in deze versie van SQL Server.|
|40517|16|Trefwoord of instructie optie '% & #x2a; ls' wordt niet ondersteund in deze versie van SQL Server.|
|40518|16|De opdracht DBCC '% & #x2a; ls' wordt niet ondersteund in deze versie van SQL Server.|
|40520|16|Beveiligbare class '% S_MSG' wordt niet ondersteund in deze versie van SQL Server.|
|40521|16|Beveiligbare class '% S_MSG' wordt niet ondersteund in het serverbereik in deze versie van SQL Server.|
|40522|16|De hoofdsom '% & #x2a; ls' databasetype wordt niet ondersteund in deze versie van SQL Server.|
|40523|16|Impliciete gebruiker '% & #x2a; ls' maken wordt niet ondersteund in deze versie van SQL Server. De gebruiker expliciet maken voordat u deze gebruikt.|
|40524|16|Gegevenstype '% & #x2a; ls' wordt niet ondersteund in deze versie van SQL Server.|
|40525|16|MET '%.ls' wordt niet ondersteund in deze versie van SQL Server.|
|40526|16|' % & #x2a; ls rijensetvoorziening niet worden ondersteund in deze versie van SQL Server.|
|40527|16|Gekoppelde servers worden niet ondersteund in deze versie van SQL Server.|
|40528|16|Gebruikers kunnen niet worden toegewezen aan certificaten, asymmetrische sleutels of Windows-aanmeldingen in deze versie van SQL Server.|
|40529|16|Ingebouwde functie '% & #x2a; ls' in imitatie context niet ondersteund in deze versie van SQL Server.|
|40532|11|Server kan niet worden geopend "% & #x2a; ls" aangevraagd door de login. De aanmelding is mislukt.|
|40553|16|De sessie is beëindigd vanwege overtollige geheugengebruik. Probeer de query te verwerken minder rijen te wijzigen.<br/><br/>*Notitie:* Het aantal te verminderen `ORDER BY` en `GROUP BY` bewerkingen in uw Transact-SQL-code vermindert de geheugenvereisten van uw query.|
|40604|16|Kan geen CREATE/ALTER DATABASE omdat Hiermee zou het quotum van de server.|
|40606|16|Databases koppelen wordt niet ondersteund in deze versie van SQL Server.|
|40607|16|Windows-aanmeldingen worden niet ondersteund in deze versie van SQL Server.|
|40611|16|Servers kunnen maximaal 128 firewallregels gedefinieerd hebben.|
|40614|16|Begin IP-adres van de firewallregel niet langer zijn dan de laatste IP-adres.|
|40615|16|Kan de server '{0}' aangevraagd door de aanmelding niet openen. Client met IP-adres '{1}' is niet toegestaan voor toegang tot de server.  Als u toegang, gebruikt u de Portal van de SQL-Database of sp_set_firewall_rule worden uitgevoerd op de hoofddatabase te maken van een firewallregel voor deze IP-adres of adresbereiken.  Het duurt maximaal vijf minuten zodat deze wijziging van kracht.|
|40617|16|De naam van de firewall-regel die met begint <rule name> te lang is. Maximumlengte is 128.|
|40618|16|De naam van de firewall regel mogen niet leeg zijn.|
|40620|16|De aanmelding voor gebruiker "% & #x2a; ls". Wijzigen van het wachtwoord is mislukt. Wachtwoord wijzigen tijdens het aanmelden wordt niet ondersteund in deze versie van SQL Server.|
|40627|20|Bewerking op server '{0}' en '{1}'-database wordt uitgevoerd. Wacht enkele minuten en probeer het opnieuw.|
|40630|16|Wachtwoordvalidatie is mislukt. Het wachtwoord voldoet niet beleidsvereisten omdat het te kort.|
|40631|16|Het wachtwoord dat u hebt opgegeven is te lang. Het wachtwoord moet niet meer dan 128 tekens bevatten.|
|40632|16|Wachtwoordvalidatie is mislukt. Het wachtwoord voldoet niet beleidsvereisten omdat deze niet complex genoeg is.|
|40636|16|Een gereserveerde databasenaam niet gebruiken '% & #x2a; ls' in deze bewerking.|
|40638|16|Ongeldige abonnements-id < abonnements-id >. Abonnement bestaat niet.|
|40639|16|Aanvraag voldoet niet aan schema: <schema error>.|
|40640|20|De server is een onverwachte uitzondering opgetreden.|
|40641|16|De opgegeven locatie is ongeldig.|
|40642|17|De server is momenteel bezet. Probeer het later opnieuw.|
|40643|16|De waarde van de opgegeven x-ms-versie kop is ongeldig.|
|40644|14|Toegang tot het opgegeven abonnement Autoriseer is mislukt.|
|40645|16|Servernaam <servername> kan niet leeg of null. Dit kan alleen bestaan uit een kleine letters 'a'-"z", de cijfers 0-9 en het afbreekstreepje. Het afbreekstreepje mogelijk niet overlappings- of in de naam wordt vastgelegd.|
|40646|16|Abonnements-ID kan niet leeg zijn.|
|40647|16|Abonnement < abonnement-id heeft geen server servernaam.|
|40648|17|Te veel aanvragen zijn uitgevoerd. Probeer het later.|
|40649|16|Ongeldige inhoudstype is opgegeven. Alleen toepassings-xml wordt ondersteund.|
|40650|16|Abonnement < abonnements-id > niet bestaat of niet gereed is voor de bewerking is.|
|40651|16|Kan geen server maken omdat het abonnement < abonnements-id > is uitgeschakeld.|
|40652|16|Kan verplaatsen of het maken van de server. Abonnement < abonnements-id > wordt server quotum overschreden.|
|40671|17|De communicatiefout tussen de gateway en de management-service. Probeer het later.|
|40852|16|Database kan niet worden geopend ' %. *ls op server ' %.* ls aangevraagd door de login. Toegang tot de database is alleen toegestaan met een beveiligingsvoorzieningen verbindingsreeks. Voor toegang tot deze database, wijzig uw verbindingstekenreeksen bevatten secure in de FQDN-naam - 'servernaam'.database.windows van de server moet .net naar 'servernaam'.database worden gewijzigd. `secure`. windows.net.|
|45168|16|De SQL Azure-systeem is onder laden en een bovengrens in gelijktijdige DB CRUD-bewerkingen voor één server is plaatsen (bijvoorbeeld-database maken). De server die is opgegeven in het foutbericht heeft het maximum aantal verbindingen overschreden. Probeer het later opnieuw.|
|45169|16|De SQL azure-systeem is onder laden en een bovengrens van het aantal gelijktijdige server CRUD-bewerkingen voor een één abonnement is plaatsen (bijvoorbeeld server maken). Het abonnement dat is opgegeven in het foutbericht wordt weergegeven, het maximum aantal verbindingen heeft overschreden en de aanvraag is geweigerd. Probeer het later opnieuw.|

## <a name="related-links"></a>Verwante koppelingen

- [Azure SQL Database-algemeen beperkingen en richtlijnen](sql-database-general-limitations.md)
- [Limieten van de resource Azure SQL-Database](sql-database-resource-limits.md)
