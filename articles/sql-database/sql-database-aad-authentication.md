<properties
   pageTitle="Verbinding maken met SQL-Database of SQL datawarehouse via Azure Active Directory-verificatie | Microsoft Azure"
   description="Leer hoe u verbinding maken met SQL-Database via Azure Active Directory-verificatie."
   services="sql-database"
   documentationCenter=""
   authors="BYHAM"
   manager="jhubbard"
   editor=""
   tags=""/>

<tags
   ms.service="sql-database"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="data-management"
   ms.date="09/16/2016"
   ms.author="rick.byham@microsoft.com"/>

# <a name="connecting-to-sql-database-or-sql-data-warehouse-by-using-azure-active-directory-authentication"></a>Verbinding maken met SQL-Database of SQL datawarehouse met Azure Active Directory-verificatie

Azure Active Directory-verificatie is een om verbinding te maken met Microsoft Azure SQL-Database en [SQL Data Warehouse](../sql-data-warehouse/sql-data-warehouse-overview-what-is.md) met behulp van identiteiten in Azure Active Directory (Azure AD). Verificatie van de Azure Active Directory, kunt u de identiteit van de databasegebruikers en andere Microsoft-services op één centrale locatie centraal beheren. Centraal beheer van de ID vormt een handig als u wilt beheren van gebruikers en machtigingen beheren eenvoudiger. Voordelen zijn onder andere het volgende:

- Deze biedt een alternatief voor SQL Server-verificatie.
- Houdt de verspreiding van gebruikersidentiteit voor databaseservers.
- Wachtwoord draaiing op één plaats kunt
- Klanten kunnen werken met externe (AAD) groepen machtigingen beheren.
- Dit kunt opslaan wachtwoorden voorkomen door het inschakelen van geïntegreerde Windows-verificatie en andere vormen van verificatie wordt ondersteund door Azure Active Directory.
- Azure Active Directory-verificatie wordt opgenomen databasegebruikers om identiteiten op het databaseniveau van de te verifiëren.
- Azure Active Directory ondersteunt token gebaseerde verificatie voor toepassingen verbinden met SQL-Database.
- Azure Active Directory-verificatie ondersteunt ADFS (domeinfederatie) of systeemeigen gebruikerswachtwoord verificatie voor een lokale Azure Active Directory zonder de Domeinsynchronisatie van het.  
- Azure Active Directory ondersteunt verbindingen vanuit SQL Server Management Studio die gebruikmaken van Active Directory universele verificatie, waaronder meervoudige verificatie MFA ().  MFA bevat sterke verificatie met een aantal eenvoudige controleopties, telefoongesprek, SMS-bericht, smartcards met vastmaken of vanuit de melding van de mobiele app. Zie [SSMS ondersteuning voor Azure AD MFA met SQL-Database en SQL Data Warehouse](sql-database-ssms-mfa-authentication.md)voor meer informatie.

De volgende configuratiestappen uit bevatten de volgende procedures om configureren en gebruiken van Azure Active Directory-verificatie.

1. Maak en een Azure Active Directory te vullen.
2. Controleer of de database op Azure SQL Database V12. (Niet nodig voor SQL Data Warehouse.)
3. Optioneel: Koppelen of wijzig de active directory die momenteel is gekoppeld aan uw Azure-abonnement.
4. Maak een Azure Active Directory-beheerder voor Azure SQL Server- of [Azure SQL Data Warehouse](https://azure.microsoft.com/services/sql-data-warehouse/).
5. Configureer uw clientcomputers.
6. Container databasegebruikers in uw database toegewezen aan Azure AD identiteiten maken.
7. Verbinding maken met uw database met behulp van Azure AD identiteiten.


## <a name="trust-architecture"></a>Architectuur vertrouwen

In het volgende op hoog niveau diagram bevat een overzicht van de oplossingsarchitectuur van het gebruik van Azure AD-verificatie met Azure SQL-Database. Dezelfde concepten die van toepassing op SQL Data Warehouse. Ter ondersteuning van Azure Active Directory systeemeigen gebruikerswachtwoord, alleen de Cloud-deel en Azure AD/Azure SQL-Database wordt beschouwd als. Ter ondersteuning van federatieve verificatie (of gebruikerswachtwoord voor Windows-referenties), de communicatie met ADFS blokkeren is vereist. De pijlen geven communicatiepaden.

![AAD auth-diagram][1]

In het volgende diagram wordt aangegeven de Federatie, vertrouwen en hostingprovider relaties die een client verbinding maken met een database door in te dienen een token toestaan. Het token wordt geverifieerd door een Azure AD en wordt vertrouwd door de database. Klant 1 kan een Azure Active Directory met systeemeigen gebruikers of een Azure Active Directory met federatieve gebruikers vertegenwoordigen. Klant 2 vertegenwoordigt een mogelijke oplossing inclusief geïmporteerde gebruikers. in dit voorbeeld die afkomstig zijn uit een federatieve Azure Active Directory met ADFS wordt gesynchroniseerd met Azure Active Directory. Het is belangrijk om te begrijpen dat toegang tot een database met behulp van Azure AD-verificatie is vereist dat het hostingprovider abonnement gekoppeld aan de Azure Active Directory is. Hetzelfde abonnement moet worden gebruikt voor het maken van de SQL-Server die de host Azure SQL-Database of SQL Data Warehouse.

![relatie van abonnement][2]

## <a name="administrator-structure"></a>Structuur van de beheerder

Als Azure AD-verificatie gebruikt, zijn er twee Administrator-accounts voor de SQL-Database-server. de oorspronkelijke SQL Server-beheerder en de Azure AD-beheerder. Dezelfde concepten die van toepassing op SQL Data Warehouse. Alleen de beheerder op basis van een Azure AD-account kunt maken van de eerste databasegebruiker van Azure AD opgenomen in de gebruikersdatabase van een. De aanmelding Azure AD-beheerder kan zijn een Azure AD-gebruiker of een Azure AD-groep. Als de beheerder een groepsaccount is, kan deze worden gebruikt door alle leden van de groep, om meerdere Azure AD-beheerders voor de SQL Server-instantie. Groepsaccount gebruiken als een beheerder wordt de beheerbaarheid verbeterd doordat u centraal toevoegen en verwijderen van leden in Azure AD zonder te wijzigen van de gebruikers of de machtigingen in SQL-Database. Slechts één Azure AD-beheerder (een gebruiker of groep) kan op elk gewenst moment worden geconfigureerd.

![structuur van de beheerder][3]

## <a name="permissions"></a>Machtigingen

Als u wilt maken van nieuwe gebruikers, moet u de `ALTER ANY USER` machtiging in de database. De `ALTER ANY USER` machtiging kan worden toegewezen aan elke databasegebruiker. De `ALTER ANY USER` machtiging ook wordt gehouden op de server administrator-accounts en databasegebruikers met de `CONTROL ON DATABASE` of `ALTER ON DATABASE` machtiging voor die database, en door leden van de `db_owner` databaserol.

Als u wilt een container databasegebruiker in Azure SQL-Database of SQL Data Warehouse maken, moet u verbinding maken met de database met een Azure AD-identiteit. Als u wilt de eerste container databasegebruiker hebt gemaakt, moet u verbinding met de database met behulp van een Azure Active Directory-beheerder (die de eigenaar van de database is). Dit is geïllustreerd in stap 4 en 5 hieronder. De Azure Active Directory-verificatie is alleen mogelijk als de Azure Active Directory-beheerder is gemaakt voor Azure SQL-Database of Data Warehouse van SQL server. Als de Azure Active Directory-beheerder is verwijderd van de server, bestaande Azure Active Directory-gebruikers die zijn gemaakt eerder in SQL Server kunnen niet meer verbinding maken met de database met hun Azure Active Directory-referenties.

## <a name="azure-ad-features-and-limitations"></a>Azure AD-voorzieningen en beperkingen

De volgende leden van Azure Active Directory kunnen worden ingericht in Azure SQL Server- of SQL Data Warehouse:

- Systeemeigen leden: een lid in Azure AD hebt gemaakt in de beheerde domein of in een klant-domein. Zie [toevoegen de naam van uw eigen domein naar Azure AD](../active-directory/active-directory-add-domain.md)voor meer informatie.

- Leden van het domein federatieve: lid zijn gemaakt in Azure AD met een gefedereerd domein. Zie [Microsoft Azure nu ondersteunt Federatie met Windows Server Active Directory](https://azure.microsoft.com/blog/2012/11/28/windows-azure-now-supports-federation-with-windows-server-active-directory/)voor meer informatie.

- Geïmporteerde leden van andere Azure voor Active Directory's die zijn op leden van een ingebouwde of federatieve domein.

- Active Directory-groepen gemaakt als beveiligingsgroepen.

Microsoft-accounts (zoals outlook.com, hotmail.com, live.com) of andere gastaccounts (bijvoorbeeld gmail.com, yahoo.com) worden niet ondersteund. Als u zich bij [https://login.live.com](https://login.live.com) met het account en wachtwoord aanmelden kunt, gebruikt u een Microsoft-account, wordt niet ondersteund voor Azure AD-verificatie voor Azure SQL-Database of Azure SQL Data Warehouse.

### <a name="additional-considerations"></a>Aanvullende overwegingen

- Als u wilt verbeteren beheerbaarheid, is het raadzaam inrichten van een speciale Azure Active Directory-groep als beheerder.
- Slechts één Azure AD-beheerder (een gebruiker of groep) kan worden geconfigureerd voor een Azure SQL Server- of Azure SQL Data Warehouse op elk gewenst moment.
- Alleen een Azure Active Directory-beheerder voor SQL Server kunt in eerste instantie verbinding maken met de Azure SQL Server of Azure SQL Data Warehouse Azure Active Directory-account gebruikt. De Active Directory-beheerder kan de volgende gebruikers van de Azure Active Directory-database configureren.
- Het is raadzaam time-out voor de verbinding wordt ingesteld op 30 seconden.
- SQL Server 2016 Management Studio en SQL Server Data Tools voor Visual Studio 2015 (versie 14.0.60311.1April 2016 of hoger) ondersteunen van Azure Active Directory-verificatie. (Azure Active Directory-verificatie wordt ondersteund door de **.NET Framework-gegevensprovider voor SQL Server**; bij minimaal versie .NET Framework 4.6). De nieuwste versies van deze hulpprogramma's en gegevens laag toepassingen (DAC en .bacpac) kunnen dus Azure Active Directory-verificatie gebruiken.
- [ODBC-versie 13,1](https://www.microsoft.com/download/details.aspx?id=53339) ondersteunt Azure Active Directory-verificatie echter `bcp.exe` kan geen verbinding maken met Azure Active Directory-verificatie omdat hierin worden gebruikt met een oudere ODBC-provider.
- `sqlcmd`begin van de Azure Active Directory-verificatie met versie 13,1 verkrijgbaar via het [Downloadcentrum](http://go.microsoft.com/fwlink/?LinkID=825643)ondersteunt.  
- SQL Server Data Tools voor Visual Studio-2015 moet ten minste de April 2016-versie van de hulpmiddelen voor gegevens (versie 14.0.60311.1). Azure Active Directory-gebruikers worden momenteel niet weergegeven in SSDT Object Explorer. Als een tijdelijke oplossing de gebruikers in [sys.database_principals](https://msdn.microsoft.com/library/ms187328.aspx)te bekijken.
- [Microsoft JDBC stuurprogramma 6.0 voor SQL Server](https://www.microsoft.com/en-us/download/details.aspx?id=11774) ondersteunt Azure Active Directory-verificatie. Zie ook [de verbindingseigenschappen instellen](https://msdn.microsoft.com/library/ms378988.aspx).
- PolyBase verifiëren niet via Azure Active Directory-verificatie.
- Sommige hulpprogramma's zoals BI en Excel worden niet ondersteund.
- Azure Active Directory-verificatie wordt ondersteund voor SQL-Database door de Azure portal bladen van een **Database importeren** en **Exporteren Database** . Importeren en exporteren met Azure Active Directory-verificatie wordt ook ondersteund uit de PowerShell-opdracht.


## <a name="1-create-and-populate-an-azure-ad"></a>1. maken en een Azure AD vullen

Maak een Azure Active Directory en deze te vullen met gebruikers en groepen. De Azure Active Directory, kan het oorspronkelijke domein van Azure AD beheerde domein zijn. De Azure Active Directory kan ook worden voor een lokale Active Directory Domain Services die is gefedereerd met de Azure Active Directory.

Zie [integratie van uw on-premises implementatie identiteiten met Azure Active Directory](../active-directory/active-directory-aadconnect.md), [de naam van uw eigen domein naar Azure AD toevoegen](../active-directory/active-directory-add-domain.md), [Microsoft Azure nu ondersteunt Federatie met Windows Server Active Directory](https://azure.microsoft.com/blog/2012/11/28/windows-azure-now-supports-federation-with-windows-server-active-directory/), [beheer van het telefoonboek van uw Azure AD](https://msdn.microsoft.com/library/azure/hh967611.aspx)en [Manage Azure AD via Windows PowerShell](https://msdn.microsoft.com/library/azure/jj151815.aspx)voor meer informatie.

## <a name="2-ensure-your-sql-database-is-version-12"></a>2. zorgen dat uw SQL-Database is versie 12

Azure Active Directory-verificatie wordt ondersteund in de meest recente V12 van de SQL-Database. Zie [Wat is er nieuw in de meest recente SQL-Database bijwerken V12](sql-database-v12-whats-new.md)voor informatie over de SQL-Database V12 en of deze beschikbaar, in uw regio is. Deze stap is niet nodig is voor Azure SQL Data Warehouse omdat SQL Data Warehouse alleen beschikbaar in V12 is.

Als u een bestaande database hebt, controleert u of dat deze wordt gehost in SQL-Database V12 door te verbinden met de database (bijvoorbeeld met SQL Server Management Studio) en het uitvoeren van `SELECT @@VERSION;`. De verwachte uitvoer voor een database in SQL-Database V12 is ten minste **Microsoft SQL Azure (RTM) - 12.0**. Als uw database niet wordt gehost in SQL-Database V12, raadpleegt u [plannen en voorbereiden voor het bijwerken naar SQL-Database V12](sql-database-v12-plan-prepare-upgrade.md), en Ga naar de klassieke Azure-Portal als u wilt migreren van de database naar SQL-Database V12.

U kunt ook een nieuwe database maken in SQL-Database V12 volgens de stappen in [de eerste Azure SQL-database maken](sql-database-get-started.md). **Tip**: Lees de volgende stap voordat u een abonnement voor uw nieuwe database.

## <a name="3-optional-associate-or-change-the-active-directory-that-is-currently-associated-with-your-azure-subscription"></a>3. optioneel: Koppelen of de active directory die momenteel is gekoppeld aan uw Azure-abonnement wijzigen

Als u wilt uw database koppelen aan de map Azure AD voor uw organisatie, de map een vertrouwde map maken voor het Azure abonnement waarop de database. Zie [hoe Azure-abonnementen zijn gekoppeld aan Azure AD](https://msdn.microsoft.com/library/azure/dn629581.aspx)voor meer informatie.

**Aanvullende informatie:** Elke Azure abonnement heeft een vertrouwensrelatie met een Azure AD-exemplaar. Dit betekent dat vertrouwde die map om te verifiëren van gebruikers, services en apparaten. Meerdere abonnementen dezelfde map kunnen vertrouwen, maar een abonnement vertrouwensrelaties slechts één map. U kunt zien welke map wordt vertrouwd door uw abonnement onder het tabblad **Instellingen** op [https://manage.windowsazure.com/](https://manage.windowsazure.com/). Deze vertrouwensrelatie dat een abonnement met een map heeft is anders dan bij de relatie die is voorzien van een abonnement met alle andere bronnen in Azure (websites, databases en dergelijke), die beter zoals onderliggende resources van een abonnement. Als een abonnement is verlopen, klikt u vervolgens toegang tot de andere resources die zijn gekoppeld aan het abonnement ook drukt, stopt. Maar de adreslijst in Azure wordt aangegeven blijft, en u kunt een ander abonnement koppelen aan die map en gaat u verder met de directorygebruikers beheren. Zie voor meer informatie over resources, [lidmaatschap resource toegang krijgen tot in Azure wordt aangegeven](https://msdn.microsoft.com/library/azure/dn584083.aspx).

De volgende procedures bieden stapsgewijze instructies voor het wijzigen van de bijbehorende map voor een bepaald abonnement.

1. Verbinding maken met uw [Klassieke Azure-Portal](https://manage.windowsazure.com/) met behulp van de beheerder van de Azure-abonnement.
2. Klik op de linker banner, selecteer **Instellingen**.
3. Uw abonnementen weergegeven in het scherm instellingen. Als het gewenste abonnement niet wordt weergegeven, klik op **abonnementen** aan de bovenkant, de **FILTER door DIRECTORY** vervolgkeuzelijst en selecteer de map waarin uw abonnementen en klik vervolgens op **toepassen**.

    ![Selecteer abonnement][4]
4. Klik in het gebied **Instellingen** op uw abonnement en klik vervolgens op **ADRESLIJST bewerken** onder aan de pagina.

    ![AD-instellingen-portal][5]
5. Selecteer de Azure Active Directory die is gekoppeld aan uw SQL Server- of SQL Data Warehouse in het vak **ADRESLIJST bewerken** en klik vervolgens op de pijl voor volgende.

    ![bewerken-directory-selecteren][6]
6. Controleer in het dialoogvenster voor **bevestigen** directory-toewijzing die "**alle CO-beheerders worden verwijderd.**"

    ![bewerken directory bevestigen][7]
7. Klik op het selectievakje om opnieuw te laden van de portal.

> [AZURE.NOTE] Wanneer u de adreslijst, toegang tot alle CO-beheerders, Azure AD-gebruikers en groepen, wijzigen en resource directory-back-gebruikers zijn verwijderd en ze niet langer toegang hebben tot dit abonnement of de bijbehorende bronnen. Alleen kunt, als de servicebeheerder van een, u toegang voor principes op basis van de nieuwe map. Deze wijziging kan veel tijd doorgeven aan alle resources duren. Als de map, ook wijzigt, de Azure AD-beheerder voor SQL-Database en SQL Data Warehouse en niet toestaan van toegang tot de database voor een bestaande Azure AD-gebruikers. De beheerder Azure AD moeten opnieuw instellen (zoals hieronder beschreven) en nieuwe Azure AD-gebruikers moeten worden gemaakt.

## <a name="4-create-an-azure-ad-administrator-for-azure-sql-server"></a>4. een Azure AD-beheerder voor Azure SQL Server maken

Elke Azure SQL-Server (die fungeert als host een SQL-Database of SQL Data Warehouse) begint met een beheerdersaccount van één server die de beheerder van de gehele Azure SQL-Server. Een tweede beheerder van de SQL Server moet worden gemaakt, die een Azure AD-account. Deze hoofdsom wordt gemaakt als een container databasegebruiker in de hoofddatabase. Als beheerders, server administrator-accounts zijn leden van **de db_owner in de gebruikersdatabase van elke** en invoeren van elke gebruikersdatabase als de gebruiker **dbo** . Zie voor meer informatie over de server administrator-accounts, [Databases beheren en aanmeldingen in Azure SQL-Database](sql-database-manage-logins.md) en de sectie **aanmeldingen en gebruikers** van [Azure SQL Database-richtlijnen voor beveiliging en beperkingen](sql-database-security-guidelines.md).

Wanneer u met Azure Active Directory met geografische herhaling, moet de beheerder van de Azure Active Directory worden geconfigureerd voor zowel de primaire en secundaire servers. Als een server geen beheerder van de Azure Active Directory, klikt u vervolgens ontvangen Azure Active Directory-aanmeldingen en gebruikers een ' kan geen verbinding"met serverfout.

> [AZURE.NOTE] Gebruikers die niet zijn gebaseerd op een Azure AD-account (inclusief het beheerdersaccount Azure SQL Server), kan Azure AD-gebruikers, maken, omdat ze bent niet gemachtigd om deze te valideren voorgestelde databasegebruikers met de Azure AD.

### <a name="provision-an-azure-active-directory-administrator-for-your-azure-sql-server-by-using-the-azure-portal"></a>Een Azure Active Directory-beheerder voor uw Azure SQL-Server inrichten met behulp van de Azure-portal

1. Klik in de [portal van Azure](https://portal.azure.com/)in de rechterbovenhoek op de verbinding met een vervolgkeuzelijst van mogelijke actieve mappen. Kies de juiste Active Directory als het standaardapparaat Azure AD. Deze stap met Active Directory met Azure SQL Server ervoor te zorgen dat het hetzelfde abonnement wordt gebruikt voor zowel de koppeling van het abonnement gekoppeld Azure AD en SQL Server. (De Azure SQL-Server kan worden hosten Azure SQL-Database of Azure SQL Data Warehouse.)

    ![Kies ad][8]
2. Selecteer **SQL-servers**op de linker banner, selecteer uw **SQL server**en klik vervolgens in het blad **SQL Server** aan de bovenkant op **Instellingen**.

    ![AD-instellingen][9]
3. Klik in het blad **Instellingen** op ** Active Directory-beheerder.
4. Klik op **Active Directory-beheerder**in het blad **Active Directory-beheerder** en klik op **beheerder instellen**aan de bovenkant.
5. Selecteer de gebruiker of groep moet een beheerder in het blad **toevoegen beheerder** , zoekt u een gebruiker, en klik vervolgens op **selecteren**. (Het blad Active Directory-beheerder ziet u alle leden en groepen van uw Active Directory. Gebruikers of groepen die worden grijs weergegeven kunnen niet worden geselecteerd, omdat ze niet worden ondersteund als Azure AD-beheerders. (Zie de lijst met ondersteunde beheerders in **Azure AD-voorzieningen en beperkingen** bovenstaande.) Rolgebaseerd toegangsbeheer (RBAC) geldt alleen voor de portal en wordt niet doorgegeven aan SQL Server.
6. Aan de bovenkant van het blad **Active Directory-beheerder** , klikt u op **Opslaan**.
    ![Kies beheerder][10]

    Het proces van het wijzigen van de beheerder kan enkele minuten duren. De beheerder van het nieuwe verschijnt in het vak **Active Directory-beheerder** .

> [AZURE.NOTE] Bij het instellen van de Azure AD-beheerder, kan niet de nieuwe naam voor de beheerder (gebruiker of groep) al aanwezig zijn in de virtuele hoofddatabase als een gebruiker van SQL Server-verificatie. Als deze aanwezig zijn, mislukt de beheerdersinstallatie Azure AD; de aanmaakdatum terugdraaien en wordt aangegeven dat deze een beheerder (naam) al bestaat. Aangezien een SQL Server-verificatie gebruiker geen deel van de Azure AD uitmaakt, wordt een planning op verbinding maken met de server met Azure AD-verificatie mislukt.

Als u wilt verwijderen later beheerder boven aan het blad **Active Directory-beheerder** op **beheerder verwijderen**en klik vervolgens op **Opslaan**.

### <a name="provision-an-azure-ad-administrator-for-azure-sql-server-by-using-powershell"></a>Inrichten van een Azure AD-beheerder voor Azure SQL Server via PowerShell

Als u wilt uitvoeren PowerShell-cmdlets, moet u beschikken over Azure PowerShell geïnstalleerd en uit te voeren. Zie [installeren en configureren van Azure PowerShell](../powershell-install-configure.md)voor gedetailleerde informatie.

Uitvoeren om het inrichten van een Azure AD-beheerder de volgende Azure PowerShell-opdrachten:

- Toevoegen AzureRmAccount
- Selecteer AzureRmSubscription


Cmdlets voor gebruikt inrichten en beheren van Azure AD-beheerder:

| Naam van cmdlet                                       | Beschrijving                                                                                                     |
|---------------------------------------------------|-----------------------------------------------------------------------------------------------------------------|
| [Set-AzureRmSqlServerActiveDirectoryAdministrator](https://msdn.microsoft.com/library/azure/mt603544.aspx)    | Hiermee wordt voorzien waarmee een Azure Active Directory-beheerder voor Azure SQL Server- of Azure SQL Data Warehouse. (Moet zijn van het huidige abonnement.) |
| [Verwijderen AzureRmSqlServerActiveDirectoryAdministrator](https://msdn.microsoft.com/library/azure/mt619340.aspx) | Hiermee verwijdert u een Azure Active Directory-beheerder voor Azure SQL Server- of Azure SQL Data Warehouse. |
| [Get-AzureRmSqlServerActiveDirectoryAdministrator](https://msdn.microsoft.com/library/azure/mt603737.aspx)    | Geeft als resultaat informatie over een Azure Active Directory-beheerder momenteel is geconfigureerd voor het Azure SQL Server- of Azure SQL Data Warehouse. |

Gebruik PowerShell-opdracht get-help voor meer details voor elk van deze opdrachten, bijvoorbeeld ``get-help Set-AzureRmSqlServerActiveDirectoryAdministrator``.

De volgende script bepalingen een Azure AD-beheerdersgroep benoemde **DBA_Group** (object-id `40b79501-b343-44ed-9ce7-da4c8cc7353f`) voor de **demo_server** server in een resourcegroep met de naam **groep-23**:

```
Set-AzureRmSqlServerActiveDirectoryAdministrator –ResourceGroupName "Group-23"
–ServerName "demo_server" -DisplayName "DBA_Group"
```

De **weergavenaam** invoerparameter accepteert de weergavenaam van Azure AD of de User Principal Name. Bijvoorbeeld ``DisplayName="John Smith"`` en ``DisplayName="johns@contoso.com"``. Azure AD groepen alleen de Azure AD wordt weergavenaam ondersteund.

> [AZURE.NOTE] De opdracht Azure PowerShell ```Set-AzureRmSqlServerActiveDirectoryAdministrator``` voorkomt niet dat u de inrichting van Azure AD-beheerders voor niet-ondersteunde gebruikers. Een niet-ondersteunde gebruiker kan worden ingericht, maar kan geen verbinding maken met een database. (Zie de lijst met ondersteunde beheerders in **Azure AD-voorzieningen en beperkingen** bovenstaande.)

Het volgende voorbeeld wordt de optioneel **object-id**:

```
Set-AzureRmSqlServerActiveDirectoryAdministrator –ResourceGroupName "Group-23"
–ServerName "demo_server" -DisplayName "DBA_Group" -ObjectId "40b79501-b343-44ed-9ce7-da4c8cc7353f"
```

> [AZURE.NOTE] De Azure AD- **object-id** is vereist als de **naam** niet uniek is. De **object-id** en **weergavenaam** om waarden te vinden, gebruikt u de sectie Active Directory van Azure klassieke Portal en de eigenschappen van een gebruiker of groep weergeven.

Het volgende voorbeeld retourneert informatie over de huidige Azure AD-beheerder voor Azure SQL Server:

```
Get-AzureRmSqlServerActiveDirectoryAdministrator –ResourceGroupName "Group-23" –ServerName "demo_server" | Format-List
```

Beheerder van de Azure AD Hiermee verwijdert u het volgende voorbeeld:
```
Remove-AzureRmSqlServerActiveDirectoryAdministrator -ResourceGroupName "Group-23" –ServerName "demo_server"
```

U kunt ook een Azure Active Directory-beheerder inrichten met behulp van de REST API's. Zie [Service Management REST API Reference en bewerkingen voor Azure SQL-Databases bewerkingen voor Azure SQL-Databases](https://msdn.microsoft.com/library/azure/dn505719.aspx) voor meer informatie

## <a name="5-configure-your-client-computers"></a>5. de clientcomputers configureren

Klik op alle clientcomputers, waaruit toepassingen of gebruikers verbinding maken met Azure SQL-Database of Azure SQL Data Warehouse gebruik van Azure AD-identiteiten, moet u de volgende software installeren:

- .NET framework 4.6 of hoger uit [https://msdn.microsoft.com/library/5a4x27ek.aspx](https://msdn.microsoft.com/library/5a4x27ek.aspx).
- Azure Active Directory-verificatie-bibliotheek voor SQL Server (**ADALSQL. DLL**) is beschikbaar in meerdere talen (x86 en amd64) vanuit het Downloadcentrum van [Microsoft Active Directory verificatie bibliotheek voor Microsoft SQL Server](http://www.microsoft.com/download/details.aspx?id=48742).

### <a name="tools"></a>Hulpmiddelen voor

- De vereiste .NET Framework 4.6 voldoet aan de installatie van [SQL Server 2016 Management Studio](https://msdn.microsoft.com/library/mt238290.aspx) of [SQL Server Data Tools voor Visual Studio-2015](https://msdn.microsoft.com/library/mt204009.aspx) .
- SSMS installaties de x86 versie van **ADALSQL. DLL**.
- SSDT installeert de amd64-versie van **ADALSQL. DLL**.
- De meest recente Visual Studio uit [Visual Studio-Downloads](https://www.visualstudio.com/downloads/download-visual-studio-vs) voldoet aan de vereiste .NET Framework 4.6, maar niet de vereiste amd64-versie van **ADALSQL geïnstalleerd. DLL**.

## <a name="6-create-contained-database-users-in-your-database-mapped-to-azure-ad-identities"></a>6. container databasegebruikers in uw database toegewezen aan Azure AD identiteiten maken

### <a name="about-contained-database-users"></a>Over container databasegebruikers

Azure Active Directory-verificatie is vereist voor databasegebruikers wilt maken, databasegebruikers van de container. Een container databasegebruiker op basis van een identiteit Azure AD is een databasegebruiker die geen een aanmelding in de hoofddatabase, en die wordt toegewezen aan een identiteit in de Azure AD-map die is gekoppeld aan de database. De identiteit Azure AD kan zijn een individuele gebruiker-account of een groep. Voor meer informatie over databasegebruikers van de container, raadpleegt u [Uw Database-draagbare opgenomen databasegebruikers aanbrengen](https://msdn.microsoft.com/library/ff929188.aspx).

> [AZURE.NOTE] Databasegebruikers (met uitzondering van beheerders) kunnen niet worden gemaakt met behulp van portal. RBAC rollen niet worden overgebracht naar SQL Server, SQL-Database of SQL Data Warehouse. Azure RBAC rollen worden gebruikt voor het beheren van Azure Resources en worden niet toegepast op databasemachtigingen. De rol **Inzender van SQL Server** verleent bijvoorbeeld geen toegang tot het verbinding maken met de SQL-Database of SQL Data Warehouse. De toegangsmachtiging is verleend rechtstreeks in de database met Transact-SQL-instructies.

### <a name="connect-to-the-user-database-or-data-warehouse-by-using-sql-server-management-studio-or-sql-server-data-tools"></a>Verbinding maken met de gebruiker database of data warehouse met behulp van SQL Server Management Studio of SQL Server Data Tools

Om te bevestigen van de Azure AD-beheerder is goed hebt ingesteld, verbinding maken met **de hoofddatabase met de Azure AD-beheerdersaccount** .
Als u wilt inrichten van een Azure AD gebaseerde opgenomen databasegebruiker (behalve de beheerder van de server die eigenaar is van de database), verbinding maken met de database met een Azure AD-identiteit die toegang tot de database heeft.

> [AZURE.IMPORTANT] Ondersteuning voor Azure Active Directory-verificatie is beschikbaar met [SQL Server 2016 Management Studio](https://msdn.microsoft.com/library/mt238290.aspx) en [SQL Server Data Tools](https://msdn.microsoft.com/library/mt204009.aspx) in Visual Studio-2015. De augustus 2016-versie van SSMS bevat ook de ondersteuning voor universele verificatie van Active Directory, waarmee beheerders meervoudige verificatie met een telefoongesprek, de SMS-bericht, smartcards met vastmaken of vanuit de melding van de mobiele app vereisen.

#### <a name="connect-using-active-directory-integrated-authentication"></a>Verbinding maken met Active Directory geïntegreerde verificatie

Gebruik deze methode als u bent aangemeld bij Windows met behulp van de Azure Active Directory-referenties van een gefedereerd domein.

1. Start Management Studio of hulpmiddelen voor gegevens en selecteer in het dialoogvenster **verbinding maken met de Server** (of **verbinding maken met Database Engine**) in het vak **verificatie** **Geïntegreerde verificatie van Active Directory**. Geen wachtwoord nodig is, of omdat uw bestaande referenties worden weergegeven voor de verbinding kan worden opgenomen.
    ![Selecteer geïntegreerde AD-verificatie][11]

2. Klik op de knop **Opties** en typ de naam van de database die u verbinding wilt maken in het vak **verbinding maken met database** op de pagina **Eigenschappen van verbinding** .
    ![Selecteer de databasenaam van de][13]


#### <a name="connect-using-active-directory-password-authentication"></a>Verbinding maken met Active Directory-wachtwoordverificatie

Gebruik deze methode als u verbinding maakt met een Azure AD-UPN gebruik van de Azure AD domein beheerd. U kunt dit ook gebruiken voor federatieve account zonder toegang tot het domein, bijvoorbeeld als u extern werkt.

Gebruik deze methode als u bent aangemeld bij Windows met referenties van een domein dat niet is gekoppeld met Azure of wanneer u met behulp van Azure AD Azure AD-verificatie op basis van de eerste of het domein van de client.

1. Start Management Studio of hulpmiddelen voor gegevens en selecteer in het dialoogvenster **verbinding maken met de Server** (of **verbinding maken met Database Engine**) in het vak **verificatie** **Active Directory-wachtwoordverificatie**.
2. Typ in het vak **gebruikersnaam** uw gebruikersnaam Azure Active Directory in de indeling **username@domain.com**. Dit moet een van de Azure Active Directory-account of een account van een domein Federatie vormen met de Azure Active Directory.
3. Typ in het vak **wachtwoord** het wachtwoord voor uw gebruikers voor de Azure Active Directory-account of gefedereerd domeinaccount.
    ![Selecteer AD-wachtwoordverificatie][12]

4. Klik op de knop **Opties** en typ de naam van de database die u verbinding wilt maken in het vak **verbinding maken met database** op de pagina **Eigenschappen van verbinding** . (Zie de afbeelding in de vorige optie.)


### <a name="create-an-azure-ad-contained-database-user-in-a-user-database"></a>Een databasegebruiker Azure AD opgenomen in de gebruikersdatabase van een maken

Als u wilt maken van een Azure AD gebaseerde opgenomen databasegebruiker (behalve de beheerder van de server die eigenaar is van de database), verbinding maken met de database met een Azure AD-identiteit, als een gebruiker met ten minste de machtiging voor **Elke gebruiker wijzigen** . Gebruik vervolgens de volgende Transact-SQL-syntaxis:

    CREATE USER <Azure_AD_principal_name>
    FROM EXTERNAL PROVIDER;


*Azure_AD_principal_name* zijn de UPN van een Azure AD-gebruiker of de weergavenaam voor een Azure AD-groep.

**Voorbeelden:** Een container database te maken dat staat voor een Azure AD gebruiker federatieve of beheerd domeingebruiker:

    CREATE USER [bob@contoso.com] FROM EXTERNAL PROVIDER;
    CREATE USER [alice@fabrikam.onmicrosoft.com] FROM EXTERNAL PROVIDER;

Als u wilt een container databasegebruiker dat staat voor een Azure AD of gefedereerd domeingroep hebt gemaakt, bieden u de weergavenaam van een beveiligingsgroep:

    CREATE USER [ICU Nurses] FROM EXTERNAL PROVIDER;

Een container databasegebruiker dat staat voor een toepassing waarmee verbinding maakt met een token Azure AD maken:

    CREATE USER [appName] FROM EXTERNAL PROVIDER;

Zie voor meer informatie over het maken van de container databasegebruikers op basis van Azure Active Directory-identiteiten [CREATE USER (Transact-SQL)](http://msdn.microsoft.com/library/ms173463.aspx).


> [AZURE.NOTE] Het verwijderen van de Azure Active Directory-beheerder voor Azure SQL Server Hiermee voorkomt u dat iedere gebruiker Azure AD-verificatie verbinding met de server. Indien nodig, onbruikbaar Azure AD-gebruikers kunnen handmatig worden verwijderd door de beheerder van een SQL-Database.

Wanneer u een databasegebruiker maakt, wordt die gebruiker ontvangt van de machtiging **VERBINDEN** en verbinding kunt maken met die database als lid van de **openbare** rol. De enige machtigingen die beschikbaar zijn voor de gebruiker zijn in eerste instantie alle machtigingen voor de **openbare** rol of machtigingen aan een Windows-groepen die ze lid zijn van zijn verleend. Nadat u een Azure AD gebaseerde opgenomen databasegebruiker inrichten, kunt u de gebruiker extra machtigingen verlenen, het op dezelfde manier als u een ander type gebruiker toestemming verlenen. Meestal machtigingen toewijzen om te databaserollen en gebruikers toevoegen aan de rollen. Zie [Database Engine machtiging basisbeginselen](http://social.technet.microsoft.com/wiki/contents/articles/4433.database-engine-permission-basics.aspx)voor meer informatie. Zie voor meer informatie over specifieke functies van de SQL-Database, [Databases beheren en aanmeldingen in Azure SQL-Database](sql-database-manage-logins.md).
Een gefedereerd domein-gebruiker die is geïmporteerd in een domein beheren, moet de identiteit beheerde domein gebruiken.

> [AZURE.NOTE] Azure AD-gebruikers zijn gemarkeerd in de databasemetagegevens met type E (EXTERNAL_USER) en voor groepen met type X (EXTERNAL_GROUPS). Zie [sys.database_principals](https://msdn.microsoft.com/library/ms187328.aspx)voor meer informatie.


## <a name="7-connect-by-using-azure-ad-identities"></a>7. verbinding maken met behulp van Azure AD identiteiten

Verificatie van Azure Active Directory worden ondersteund in de volgende manieren verbinden met een database met behulp van Azure AD identiteiten:

- Geïntegreerde Windows-verificatie
- Gebruik een Azure AD-UPN en een wachtwoord
- Gebruik van de toepassing token verificatie

### <a name="71-connecting-using-integrated-windows-authentication"></a>7.1. Verbinding maken met geïntegreerde (Windows)-verificatie

Als u wilt gebruiken geïntegreerde Windows-verificatie, moet de Active Directory van uw domein worden gekoppeld aan Azure Active Directory. De clienttoepassing (of een service) verbinden met de database moet worden uitgevoerd op een computer domein behoren onder van een gebruiker domeinreferenties.

Als u wilt verbinden met een database met behulp van geïntegreerde verificatie en een Azure AD-identiteit, moet het trefwoord verificatie in de verbindingsreeks van de database zijn ingesteld op Active Directory geïntegreerde. In het onderstaande C#-codevoorbeeld gebruikt ADO .NET.

    string ConnectionString =
    @"Data Source=n9lxnyuzhv.database.windows.net; Authentication=Active Directory Integrated; Initial Catalog=testdb;";
    SqlConnection conn = new SqlConnection(ConnectionString);
    conn.Open();

Opmerking dat de verbinding trefwoord tekenreeks ``Integrated Security=True`` wordt niet ondersteund om verbinding te maken met Azure SQL-Database.
Houd er rekening mee dat bij het maken van een ODBC-verbinding u moeten spaties verwijderen en stel verificatie naar 'ActiveDirectoryIntegrated'.

### <a name="72-connecting-with-an-azure-ad-principal-name-and-a-password"></a>7.2. Verbinding maken met een Azure AD-UPN en een wachtwoord
Als u wilt verbinden met een database met behulp van geïntegreerde verificatie en een Azure AD-identiteit, moet het trefwoord verificatie zijn ingesteld op Active Directory-wachtwoord. De verbindingsreeks moet gebruikers-ID/UID en wachtwoord/PWD trefwoorden en waarden bevatten. In het onderstaande C#-codevoorbeeld gebruikt ADO .NET.

    string ConnectionString =
      @"Data Source=n9lxnyuzhv.database.windows.net; Authentication=Active Directory Password; Initial Catalog=testdb;  UID=bob@contoso.onmicrosoft.com; PWD=MyPassWord!";
    SqlConnection conn = new SqlConnection(ConnectionString);
    conn.Open();

Meer informatie over Azure AD-verificatiemethoden behulp van de beschikbare voor voorbeelden van demo code in [Azure AD verificatie GitHub Demo](https://github.com/Microsoft/sql-server-samples/tree/master/samples/features/security/azure-active-directory-auth).


### <a name="73-connecting-with-an-azure-ad-token"></a>7.3 verbinding maken met een token Azure AD
Deze verificatiemethode staat middelste laag services verbinding maken met Azure SQL-Database of Azure SQL Data Warehouse door het verkrijgen van een token van Azure Active Directory (AAD). Geavanceerde scenario's, inclusief certificaatverificatie kunt. Vier basisstappen als token Azure AD-verificatie wilt gebruiken, moet u uitvoeren:

1. Uw toepassing registreren met Azure Active Directory en de client-id ophalen voor uw code. 
2. Maak een databasegebruiker die de toepassing vertegenwoordigt. (Voltooid eerder in stap 6).
3. Een digitaal certificaat maken op de client computer wordt uitgevoerd de toepassing.
4. Het certificaat toegevoegd als een sleutel voor uw toepassing.

Voorbeeld-verbindingsreeks:

```
string ConnectionString =@"Data Source=n9lxnyuzhv.database.windows.net; Initial Catalog=testdb;"
SqlConnection conn = new SqlConnection(ConnectionString);
connection.AccessToken = "Your JWT token"
conn.Open();
```

Zie [SQL Server Beveiligingsblog](https://blogs.msdn.microsoft.com/sqlsecurity/2016/02/09/token-based-authentication-support-for-azure-sql-db-using-azure-ad-auth/)voor meer informatie.

### <a name="connecting-with-sqlcmd"></a>Verbinding maken met sqlcmd  
De volgende beweringen, verbinding maken met behulp van versie 13,1 van sqlcmd, verkrijgbaar via het [Downloadcentrum](http://go.microsoft.com/fwlink/?LinkID=825643).

```
sqlcmd -S Target_DB_or_DW.testsrv.database.windows.net  -G  
sqlcmd -S Target_DB_or_DW.testsrv.database.windows.net -U bob@contoso.com -P MyAADPassword -G -l 30
```

## <a name="see-also"></a>Zie ook

[Databases en aanmeldingen in Azure SQL-Database beheren](sql-database-manage-logins.md)

[Databasegebruikers van de container](https://msdn.microsoft.com/library/ff929071.aspx)

[Maak gebruiker (Transact-SQL)](http://msdn.microsoft.com/library/ms173463.aspx)


<!--Image references-->

[1]: ./media/sql-database-aad-authentication/1aad-auth-diagram.png
[2]: ./media/sql-database-aad-authentication/2subscription-relationship.png
[3]: ./media/sql-database-aad-authentication/3admin-structure.png
[4]: ./media/sql-database-aad-authentication/4select-subscription.png
[5]: ./media/sql-database-aad-authentication/5ad-settings-portal.png
[6]: ./media/sql-database-aad-authentication/6edit-directory-select.png
[7]: ./media/sql-database-aad-authentication/7edit-directory-confirm.png
[8]: ./media/sql-database-aad-authentication/8choose-ad.png
[9]: ./media/sql-database-aad-authentication/9ad-settings.png
[10]: ./media/sql-database-aad-authentication/10choose-admin.png
[11]: ./media/sql-database-aad-authentication/11connect-using-int-auth.png
[12]: ./media/sql-database-aad-authentication/12connect-using-pw-auth.png
[13]: ./media/sql-database-aad-authentication/13connect-to-db.png

