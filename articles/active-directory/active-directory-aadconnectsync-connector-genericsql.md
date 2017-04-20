<properties
   pageTitle="Algemene SQL-Connector | Microsoft Azure"
   description="In dit artikel wordt beschreven hoe Microsofts algemene SQL-Connector configureren."
   services="active-directory"
   documentationCenter=""
   authors="AndKjell"
   manager="femila"
   editor=""/>

<tags
   ms.service="active-directory"
   ms.workload="identity"
   ms.tgt_pltfrm="na"
   ms.devlang="na"
   ms.topic="article"
   ms.date="08/30/2016"
   ms.author="billmath"/>

# <a name="generic-sql-connector-technical-reference"></a>Algemene SQL-Connector technische verwijzing

In dit artikel worden de algemene SQL-Connector. Het artikel is van toepassing op de volgende producten:

- Microsoft identiteitsbeheer 2016 (MIM2016)
- Forefront identiteit Manager 2010 R2 (FIM2010R2)
    -   Moet hotfix 4.1.3671.0 of hoger [KB3092178](https://support.microsoft.com/kb/3092178)gebruiken.

De verbindingslijn is voor MIM2016 en FIM2010R2, een downloaden via het [Microsoft Downloadcentrum](http://go.microsoft.com/fwlink/?LinkId=717495).

Zie het artikel [Algemene SQL-Connector stapsgewijze](active-directory-aadconnectsync-connector-genericsql-step-by-step.md) deze Connector in actie.

## <a name="overview-of-the-generic-sql-connector"></a>Overzicht van de algemene SQL-Connector

De algemene SQL-Connector kunt u de synchronisatieservice integreren met een databasequery die ODBC-connectiviteit biedt.  

Hoog niveau vanuit het perspectief, van worden de volgende functies ondersteund door de huidige versie van de verbindingslijn:

Functie | Ondersteuning
--- | ---
Verbonden gegevensbron | De verbindingslijn wordt met alle 64-bits ODBC-stuurprogramma's ondersteund. Het is getest met de volgende items: <li>Microsoft SQL Server en SQL Azure wordt aangegeven</li><li>IBM DB2 10.x</li><li>IBM DB2 9.x</li><li>Oracle 10 en 11 g</li><li>MySQL 5.x</li>
Scenario 's   | <li>Beheer van productlevenscyclus object</li><li>Wachtwoordbeheer</li>
Bewerkingen | <li>Volledige importeren en Delta importeren en exporteren</li><li>Voor exporteren: Toevoegen, verwijderen, bijwerken en vervangen</li><li>Wachtwoord instellen, wachtwoord wijzigen</li>
Schema | <li>Dynamische detectie van objecten en kenmerken</li>

### <a name="prerequisites"></a>Vereisten voor
Voordat u de verbindingslijn gebruiken, te controleren of u de volgende handelingen uit op de synchronisatieserver:

- 4.5.2 van Microsoft .NET Framework of hoger
- 64-bits ODBC-stuurprogramma's-client

### <a name="permissions-in-connected-data-source"></a>Machtigingen in verbonden gegevensbron
Als u wilt maken of geen van de ondersteunde taken uitvoeren in de algemene SQL-connector, moet u het volgende hebben:

- db_datareader
- db_datawriter

### <a name="ports-and-protocols"></a>Poorten en protocollen
Voor de poorten vereist voor het ODBC-stuurprogramma om te werken en de documentatie van de databaseleverancier.

## <a name="create-a-new-connector"></a>Een nieuwe verbindingslijn maken
Als u wilt maken van een algemene SQL-connector, selecteer u in **Synchronisatieservice** **Management Agent** en **maken**. Selecteer de verbindingslijn **algemene SQL (Microsoft)** .

![CreateConnector](./media/active-directory-aadconnectsync-connector-genericsql/createconnector.png)

### <a name="connectivity"></a>Connectiviteit
De verbindingslijn gebruikt een ODBC DSN-bestand voor connectivity. Het DSN-bestand met **ODBC-gegevensbronnen** zijn gevonden in het startmenu onder **Beheerprogramma's**maken. In het beheerprogramma, door een **DSN-bestand** te maken, zodat deze kan worden verstrekt aan de verbindingslijn.

![CreateConnector](./media/active-directory-aadconnectsync-connector-genericsql/connectivity.png)

Het scherm Connectivity is de eerste wanneer u een nieuwe algemene SQL-verbindingslijn maakt. Eerst moet u de onderstaande informatie opgeven:

- DSN-bestandspad
- Verificatie
    - Gebruikersnaam in te voeren
    - Wachtwoord

De database moet een van deze verificatiemethoden ondersteund:

- **Windows-verificatie**: de verificatie database de Windows-referenties gebruikt om te controleren of de gebruiker. De gebruikersnaam en wachtwoord opgegeven wordt gebruikt om te verifiëren met de database. Dit account moet machtigingen voor de database.
- **SQL-verificatie**: de verificatie database doeleinden de gebruikersnaam en wachtwoord gedefinieerd het scherm Connectivity verbinding maken met de database. Als u de gebruiker naam/wachtwoord in het DSN-bestand opslaat, hebben de referenties die zijn opgegeven in het scherm Connectivity prioriteit.
- **Azure SQL Database-verificatie**: Zie voor meer informatie [verbinding maken met SQL Database met behulp van Azure Active Directory-verificatie](..\sql-database\sql-database-aad-authentication.md).

**DN is anker**: als u deze optie selecteert, de DN ook als het ankerkenmerk wordt gebruikt. Dit kan worden gebruikt voor een eenvoudige implementatie, maar ook heeft de volgende beperking:

-   Connector ondersteunt slechts één objecttype. De kenmerken van een verwijzing kunnen dus alleen verwijzen naar hetzelfde objecttype.

**Type exporteren: Object vervangen**: tijdens het exporteren als alleen bepaalde kenmerken hebt gewijzigd, het gehele object met alle kenmerken is geëxporteerd en vervangt de bestaande object.

### <a name="schema-1-detect-object-types"></a>Schema 1 (objecttypen bepalen)
Op deze pagina gaat u configureren hoe de verbindingslijn dient te vinden van de verschillende typen in de database.

Elke objecttype wordt gepresenteerd als een partition en configuratie van verdere op **partities configureren en hiërarchieën**.

![schema1a](./media/active-directory-aadconnectsync-connector-genericsql/schema1a.png)

**Objecttype detectiemethode**: de verbindingslijn ondersteunt deze object type detectiemethoden.

- **Vaste waarde**: U de lijst met objecttypen met een door komma's gescheiden lijst opgeeft. Bijvoorbeeld: `User,Group,Department`.  
![schema1b](./media/active-directory-aadconnectsync-connector-genericsql/schema1b.png)
- **Tabel/weergave/opgeslagen Procedure**: de naam van de tabel/weergave/opgeslagen procedure en klik vervolgens op de naam van de kolom die de lijst met objecttypen opgeven. Als u een opgeslagen procedure gebruikt, klikt u vervolgens Daarnaast bieden parameters voor deze in de indeling **[naam]: [richting]: [waarde]**. Geef elke parameter op een aparte regel (Gebruik Ctrl + Enter om een nieuwe regel).  
![schema1c](./media/active-directory-aadconnectsync-connector-genericsql/schema1c.png)
- **SQL-Query**: deze optie kunt u een SQL-query die één kolom met objecttypen, bijvoorbeeld retourneert geven `SELECT [Column Name] FROM TABLENAME`. De resulterende kolom moet van het type tekenreeks (varchar).

### <a name="schema-2-detect-attribute-types"></a>Schema 2 (bepalen kenmerktypen)
Op deze pagina gaat u configureren hoe de kenmerknamen en typen wilt worden gedetecteerd. De configuratieopties worden weergegeven voor elke objecttype gedetecteerd op de vorige pagina.

![schema2a](./media/active-directory-aadconnectsync-connector-genericsql/schema2a.png)

**Type kenmerk detectiemethode**: de verbindingslijn ondersteunt deze methoden voor detectie kenmerk met elk objecttype gevonden in het scherm Schema 1.

- **Tabel/weergave/opgeslagen Procedure**: Geef de naam van de tabel/weergave/opgeslagen procedure die moet worden gebruikt voor het kenmerknamen zoeken. Als u een opgeslagen procedure gebruikt, klikt u vervolgens Daarnaast bieden parameters voor deze in de indeling **[naam]: [richting]: [waarde]**. Geef elke parameter op een aparte regel (Gebruik Ctrl + Enter om een nieuwe regel). Als u wilt de namen in een kenmerk met meerdere waarden worden gevonden, vindt u een door komma's gescheiden lijst met tabellen of weergaven. Met meerdere waarden scenario's worden niet ondersteund wanneer bovenliggende en onderliggende tabel hebben dezelfde kolomnamen.
- **SQL-query**: deze optie kunt u een SQL-query die één kolom met kenmerknamen van de, bijvoorbeeld retourneert geven `SELECT [Column Name] FROM TABLENAME`. De resulterende kolom moet van het type tekenreeks (varchar).

### <a name="schema-3-define-anchor-and-dn"></a>Schema 3 (definiëren anker en DN)
Deze pagina kunt u anker en DN-kenmerk voor elk objecttype gevonden configureren. U kunt meerdere kenmerk tot het anker uniek selecteren.

![schema3a](./media/active-directory-aadconnectsync-connector-genericsql/schema3a.png)

- Met meerdere waarden en Booleaanse kenmerken worden niet weergegeven.
- Hetzelfde kenmerk niet gebruiken voor DN en anker, tenzij **DN is anker** is geselecteerd op de pagina Connectivity.
- Als op de pagina Verbindingsmogelijkheden **DN is anker** is geselecteerd, moet deze pagina alleen het DN-kenmerk. Dit kenmerk zou ook worden gebruikt als het ankerkenmerk.
![schema3b](./media/active-directory-aadconnectsync-connector-genericsql/schema3b.png)

### <a name="schema-4-define-attribute-type-reference-and-direction"></a>Schema 4 (definiëren kenmerktype, verwijzing en richting)
Deze pagina kunt u het kenmerktype, zoals geheel getal, binair getal of Booleaanse waarde en richting voor elk kenmerk configureren. Alle kenmerken van de pagina **schema 2** zijn opgenomen inclusief met meerdere waarden kenmerken.

![schema4a](./media/active-directory-aadconnectsync-connector-genericsql/schema4a.png)

- **Gegevenstype**: gebruikt voor het toewijzen van het kenmerktype aan deze typen wel aangeduid met de synchronisatie-engine. De standaardinstelling is hetzelfde type als u wilt gebruiken in het schema SQL gedetecteerd, maar DateTime- en verwijzingsfuncties zijn niet gemakkelijk detecteerbare. Voor gebruikers met computers moet u opgeven van de **notatie DateTime** of de **verwijzing**.
- **Richting**: U kunt de richting kenmerk instellen om te importeren, exporteren of ImportExport. ImportExport is standaard.
![schema4b](./media/active-directory-aadconnectsync-connector-genericsql/schema4b.png)

Notities:

- Als een kenmerktype niet detecteerbare doorvoeren en snel aanvullen is, wordt het gegevenstype tekenreeks.
- **Geneste tabellen** kunnen worden beschouwd als één kolom databasetabellen. Oracle slaat de rijen van een geneste tabel in een willekeurige volgorde. Wanneer u de geneste tabel in een variabele PL/SQL ophaalt, worden de rijen echter opeenvolgende subscripts beginnen bij 1 gegeven. Die geeft u matrix-achtige toegang tot afzonderlijke rijen.
- **VARRYS** worden niet ondersteund in de verbindingslijn.

### <a name="schema-5-define-partition-for-reference-attributes"></a>Schema 5 (partition definiëren voor verwijzing kenmerken)
U configureert voor alle verwijzing kenmerken welke partition (objecttype) een kenmerk naar verwijst op deze pagina.

![schema5](./media/active-directory-aadconnectsync-connector-genericsql/schema5.png)

Als u de **DN anker wordt**gebruikt, moet u hetzelfde objecttype gebruiken als dat waarin die u verwijst uit. U kan niet verwijzen naar een ander objecttype.

### <a name="global-parameters"></a>Globale Parameters
De pagina globale Parameters wordt gebruikt voor het configureren van Delta importeren, datum-/ tijdnotatie en wachtwoordmethode.

![globalparameters1](./media/active-directory-aadconnectsync-connector-genericsql/globalparameters1.png)

De algemene SQL-Connector ondersteunt de volgende methoden voor Delta importeren:

- **Trigger**: Zie [Delta weergaven met Triggers genereren](https://technet.microsoft.com/library/cc708665.aspx).
- **Watermerk**: een algemene aanpak die kan worden gebruikt met elke database. De query watermerk is ingevuld op basis van de leverancier van de database. Een kolom watermerk moet presenteren op elke tabel/weergave die wordt gebruikt. Deze kolom wordt ingevoegd moet bijhouden en updates voor de tabellen als en de afhankelijke (meerdere waarden of een onderliggend) tabellen. De klok tussen synchronisatieservice en de database-server moeten worden gesynchroniseerd. Als dit niet het geval is, wordt mogelijk worden sommige items in de import delta weggelaten.  
Beperking:
    - Watermerk strategie biedt geen verwijderde ondersteuningsobjecten.
- **Momentopname**: (werkt alleen met Microsoft SQL Server) [genereren Delta weergaven met momentopnamen](https://technet.microsoft.com/library/cc720640.aspx)
- **Wijzigingen bijhouden**: (werkt alleen met Microsoft SQL Server) [over het bijhouden van wijzigingen](https://msdn.microsoft.com/library/bb933875.aspx)  
Beperkingen:
    - Anker & DN-kenmerk moeten deel uitmaken van primaire sleutel voor het geselecteerde object in de tabel.
    - SQL-query wordt niet ondersteund tijdens het importeren en exporteren met wijzigingen bijhouden.

**Aanvullende Parameters**: Geef de tijdzone van de Database Server die aangeeft waar uw Database-server zich bevindt. Deze waarde wordt gebruikt voor de ondersteuning van de verschillende indelingen van datum / tijd-kenmerken.

De verbindingslijn slaat altijd datum- en datum-tijd in UTC-indeling. Als u wilt kunnen correct converteren de datum en tijden, wordt de tijdzone van de database-server en de indeling die wordt gebruikt moet worden opgegeven. De opmaak moet worden uitgedrukt in .net-indeling.

Tijdens het exporteren moet elke datum-tijd-kenmerk worden opgegeven om de verbindingslijn in UTC-tijd-indeling.

![globalparameters2](./media/active-directory-aadconnectsync-connector-genericsql/globalparameters2.png)

**Wachtwoord configuratie**: de connector biedt wachtwoord synchronisatie mogelijkheden en ondersteunt instellen en het wachtwoord wijzigen.

De verbindingslijn biedt twee methoden om te ondersteunen Wachtwoordsynchronisatie:

- **Opgeslagen Procedure**: deze methode is vereist twee opgeslagen procedures ter ondersteuning van wijziging & instellen wachtwoord. Typ alle parameters voor het toevoegen en wijzigen van de bewerking wachtwoord in **Wachtwoord SP instellen** en **Wijzigen wachtwoord SP** Parameters respectievelijk als per onder voorbeeld.
![globalparameters3](./media/active-directory-aadconnectsync-connector-genericsql/globalparameters3.png)
- **Wachtwoord extensie**: deze methode wachtwoord extensie DLL (u moet de naam van de extensie-DLL die de interface [IMAExtensible2Password](https://msdn.microsoft.com/library/microsoft.metadirectoryservices.imaextensible2password.aspx) implementeert bieden) is vereist. Wachtwoord extensie constructie moet worden geplaatst in extensie map, zodat de verbindingslijn de DLL gedurende runtime kunt laden.
![globalparameters4](./media/active-directory-aadconnectsync-connector-genericsql/globalparameters4.png)

U moet ook de wachtwoordbeheer op de pagina **Configureren extensie** inschakelen.
![globalparameters5](./media/active-directory-aadconnectsync-connector-genericsql/globalparameters5.png)

### <a name="configure-partitions-and-hierarchies"></a>Partities en hiërarchieën configureren
Selecteer op de pagina partities en hiërarchieën alle objecttypen. Elk object is een eigen partition.

![partitions1](./media/active-directory-aadconnectsync-connector-genericsql/partitions1.png)

U kunt ook de waarden die zijn gedefinieerd op de pagina **Connectivity** of **Globale Parameters** negeren.

![partitions2](./media/active-directory-aadconnectsync-connector-genericsql/partitions2.png)

### <a name="configure-anchors"></a>Ankers configureren
Deze pagina is alleen-lezen, aangezien het anker is al gedefinieerd. Het geselecteerde ankerkenmerk worden altijd toegevoegd met het objecttype om ervoor te zorgen dat unieke blijft over objecttypen.

![ankers](./media/active-directory-aadconnectsync-connector-genericsql/anchors.png)

## <a name="configure-run-step-parameter"></a>Stap uitvoeren Parameter configureren
Deze stappen zijn geconfigureerd op de profielen van de uitvoeren op de verbindingslijn. Deze configuraties doen de werkelijke hoeveelheid werk van de gegevens importeren en exporteren.

### <a name="full-and-delta-import"></a>Volledige en Delta importeren
Algemene SQL-Connector ondersteuning volledig en Delta importeren met behulp van deze methoden:

- Tabel
- Weergave
- Opgeslagen Procedure
- SQL-Query

![runstep1](./media/active-directory-aadconnectsync-connector-genericsql/runstep1.png)

**Tabelweergave /**  
Als u wilt importeren met meerdere waarden kenmerken voor een object, moet u de naam van de tabel/weergave door komma's gescheiden in een **tabel/weergave van de naam van meerdere waarden** en de desbetreffende joinvoorwaarden in de **Join-voorwaarde** voorzien van de bovenliggende tabel.

Voorbeeld: Wilt u het object werknemer en alle bijbehorende met meerdere waarden kenmerken importeren. Er zijn twee tabellen, met de naam werknemer (hoofdtabel) en afdeling (meerdere waarden).
Ga als volgt:

- Type **werknemer** in **Tabel/weergave/SP**.
- Typ afdeling in **naam van meerdere waarden tabel/weergaven**.
- Typ de join-voorwaarde tussen werknemer & afdeling **Join-voorwaarde**, bijvoorbeeld `Employee.DEPTID=Department.DepartmentID`.
![runstep2](./media/active-directory-aadconnectsync-connector-genericsql/runstep2.png)

**Opgeslagen procedures**  
![runstep3](./media/active-directory-aadconnectsync-connector-genericsql/runstep3.png)

- Als u veel gegevens hebt, is het raadzaam willen implementeren paginering met uw opgeslagen Procedures.
- Voor uw opgeslagen Procedure ter ondersteuning van paginering, moet u Index begin en einde Index op te geven. Zie: [efficiënt paginering van grote hoeveelheden gegevens](https://msdn.microsoft.com/library/bb445504.aspx).
- @StartIndexen @EndIndex inconsistente worden vervangen door de desbetreffende paginaformaat die is geconfigureerd op de pagina **Stap configureren** . Bijvoorbeeld wanneer de verbindingslijn Hiermee worden opgehaald van de eerste pagina en de paginagrootte is ingesteld 500, in een dergelijke situatie @StartIndex zou dan 1 en @EndIndex 500. Deze waarden vergroten wanneer verbindingslijn haalt de volgende pagina's en wijzigt u de @StartIndex & @EndIndex waarde.
- Als u wilt uitvoeren die zijn opgeslagen Procedure met parameters, bieden de parameters in `[Name]:[Direction]:[Value]` opmaken. Elke parameter invoert op een aparte regel (Gebruik Ctrl + Enter om een nieuwe regel).
- Algemene SQL-connector ondersteunt ook importbewerking uit gekoppelde Servers in Microsoft SQL Server. Als de gegevens moet worden opgehaald uit een tabel in een gekoppelde server, moet tabel in de vorm worden verstrekt:`[ServerName].[Database].[Schema].[TableName]`
- Algemene SQL-Connector ondersteunt alleen objecten die vergelijkbaar structuur hebben (zowel alias naam en hetzelfde gegevenstype) tussen gegevens en schema's detectie van stappen uitvoeren. Het geselecteerde object uit schema en meegeleverde informatie aan uitvoeren stap wordt weergegeven, klikt u vervolgens is SQL verbindingslijn ter ondersteuning van dit type scenario's beschreven.

**SQL-Query**  
![runstep4](./media/active-directory-aadconnectsync-connector-genericsql/runstep4.png)

![runstep5](./media/active-directory-aadconnectsync-connector-genericsql/runstep5.png)

- Meerdere query's niet ondersteund voor resultaatset.
- SQL-query ondersteunt de paginering en geef de begin-Index en einde Index als een variabele ter ondersteuning van paginering.

### <a name="delta-import"></a>Delta importeren
![runstep6](./media/active-directory-aadconnectsync-connector-genericsql/runstep6.png)

Delta importeren configuratie is vereist voor sommige complexere configuratie vergeleken met volledige importeren.

- Als u de Trigger of een momentopname methode voor het bijhouden van deltawijzigingen kiest, moet u vervolgens geschiedenistabel of een momentopname database in vak **geschiedenistabel of een momentopname databasenaam** opgeven.
- U moet ook join-voorwaarde tussen geschiedenistabel en bovenliggende tabel, bijvoorbeeld opgeven`Employee.ID=History.EmployeeID`
- Als u wilt bijhouden de transactie op de bovenliggende tabel uit de geschiedenistabel, moet u de naam van de kolom met de bewerkingsinformatie (toevoegen/bijwerken/verwijderen) opgeven.
- Als u een watermerk voor het bijhouden van deltawijzigingen kiest, moet u vervolgens de naam van de kolom met de bewerkingsinformatie in **De naam van de kolom markeren Water**opgeven.
- De kolom **typekenmerk wijzigen** is vereist voor het wijzigingstype. Deze kolom kaarten een wijziging die wordt weergegeven in de primaire tabel of met meerdere waarden op het wijzigingstype in de weergave delta. Deze kolom bevatten het type Modify_Attribute wijzigen voor het kenmerk niveau wijzigen of een toevoegen, wijzigen of verwijderen type voor een object op gebruikersniveau wijzigingstype wijzigen. Als er een ander nummer dan de standaardwaarde toevoegen, wijzigen of verwijderen, moet u deze waarden met deze optie kunt definiëren.

### <a name="export"></a>Exporteren
![runstep7](./media/active-directory-aadconnectsync-connector-genericsql/runstep7.png)

Ondersteuning voor algemene SQL-Connector exporteren met vier ondersteunde methoden, zoals:

- Tabel
- Weergave
- Opgeslagen Procedure
- SQL-Query

**Tabelweergave /**  
Als u de optie/tabelweergave kiest, klikt u vervolgens de verbindingslijn de desbetreffende query's om uit te voeren de Export wordt gegenereerd.

**Opgeslagen procedures**  
![runstep8](./media/active-directory-aadconnectsync-connector-genericsql/runstep8.png)

Als u de opgeslagen Procedure optie kiest, moet exporteren drie verschillende opgeslagen procedures invoegen/bijwerken/verwijderen bewerkingen uit te voeren.

- **SP-naam toevoegen**: deze SP wordt uitgevoerd als u een willekeurig object kunt verbindingslijn om in te voegen in de desbetreffende tabel.
- **SP-naam bijwerken**: deze SP wordt uitgevoerd als u een willekeurig object kunt connector voor bijwerken in de desbetreffende tabel.
- **SP-naam verwijderen**: deze SP wordt uitgevoerd als u een willekeurig object kunt connector voor verwijdering in de desbetreffende tabel.
- Het kenmerk is geselecteerd in het schema gebruikt als een parameterwaarde op te geven aan de opgeslagen procedure. Bijvoorbeeld `EmployeeName: INPUT: @EmployeeName` (EmployeeName is geselecteerd in het schema verbindingslijn en de verbindingslijn vervangt de overeenkomstige waarde terwijl u exporteren)
- Als u wilt uitvoeren opgeslagen procedure met parameters, bieden parameters in `[Name]:[Direction]:[Value]` opmaken. Elke parameter invoert op een aparte regel (Gebruik Ctrl + Enter om een nieuwe regel).

**SQL-query**  
![runstep9](./media/active-directory-aadconnectsync-connector-genericsql/runstep9.png)

Als u de optie SQL-query kiest, moet exporteren drie verschillende query's invoegen/bijwerken/verwijderen bewerkingen uit te voeren.

- **Query invoegen**: deze query wordt uitgevoerd als u een willekeurig object kunt verbindingslijn om in te voegen in de desbetreffende tabel.
- **Query bijwerken**: deze query wordt uitgevoerd als u een willekeurig object kunt connector voor bijwerken in de desbetreffende tabel.
- **Verwijderquery**: deze query wordt uitgevoerd als u een willekeurig object kunt connector voor verwijdering in de desbetreffende tabel.
- Kenmerk uit het schema gebruikt als een parameterwaarde op de query, bijvoorbeeld geselecteerd`Insert into Employee (ID, Name) Values (@ID, @EmployeeName)`

## <a name="troubleshooting"></a>Problemen oplossen

-   Zie voor informatie over het inschakelen van logboekregistratie om op te lossen de verbindingslijn, [hoe u ETW tracering inschakelen voor verbindingslijnen](http://go.microsoft.com/fwlink/?LinkId=335731).
