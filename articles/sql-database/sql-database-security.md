<properties
   pageTitle="SQL-Database beveiliging-overzicht"
   description="Meer informatie over de beveiliging van Azure SQL-Database en SQL Server, met inbegrip van de verschillen tussen de cloud en SQL Server on-premises als het gaat om verificatie, autorisatie, beveiliging van verbindingen, codering en naleving."
   services="sql-database"
   documentationCenter=""
   authors="tmullaney"
   manager="jhubbard"
   editor=""/>

<tags
   ms.service="sql-database"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-management"
   ms.date="06/09/2016"
   ms.author="thmullan;jackr"/>


# <a name="securing-your-sql-database"></a>Beveiliging van uw SQL-Database

## <a name="overview"></a>Overzicht

In dit artikel worden de basisbeginselen van het beveiligen van de gegevenslaag van een toepassing via Azure SQL-Database. Met name in dit artikel krijgt u de slag met informatiebronnen voor het beperken van toegang, gegevens beveiligen en bewaken activiteiten op een database in de [aan de slag met SQL-Database zelfstudie](sql-database-get-started.md)hebt gemaakt. Zie voor een volledig overzicht van de beveiligingsfuncties beschikbaar zijn op alle versies van SQL, het [Beveiligingscentrum voor SQL Server-Database Engine en Azure SQL-Database](https://msdn.microsoft.com/library/bb510589). Aanvullende informatie is ook beschikbaar in de [beveiligings- en Azure SQL-Database technische whitepaper](https://download.microsoft.com/download/A/C/3/AC305059-2B3F-4B08-9952-34CDCA8115A9/Security_and_Azure_SQL_Database_White_paper.pdf) (PDF).

## <a name="connection-security"></a>Beveiliging van de verbinding

Beveiliging van de verbinding verwijst naar hoe u beperken en beveiligde verbindingen met uw database via firewallregels en verbinding versleutelen.

Firewallregels worden gebruikt door de server en de database naar verbindingspogingen van IP-adressen die niet expliciet whitelisted zijn negeren. Als u wilt dat uw toepassing of het openbare IP-adres van de clientcomputer om te proberen verbinding maakt met een nieuwe database, moet u eerst een serverniveau firewallregel met de klassieke Azure-Portal, REST API of PowerShell maken. Als een goede gewoonte, moet u het IP-adresbereiken toegestaan via uw server-firewall zo veel mogelijk te beperken. Zie [Azure SQL Database-Firewall](https://msdn.microsoft.com/library/ee621782)voor meer informatie.

Alle verbindingen met Azure SQL-Database versleuteling (SSL/TLS) bij alle tijden terwijl gegevens 'tijdens overdracht"en naar de database is. In de verbindingsreeks van de toepassing, moet u parameters voor het coderen van de verbinding en *niet* te vertrouwen het servercertificaat (dit doet u als u de verbindingsreeks afmelden bij de Portal van Azure-klassieke kopieert), anders kan de verbinding wordt niet controleren of de identiteit van de server en is wel vatbaar voor "man-in-het-midden" aanvallen. Voor het stuurprogramma ADO.NET, bijvoorbeeld deze parameters voor de verbindingsreeks zijn **versleutelen = True** en **TrustServerCertificate = False**. Zie [Azure SQL Database-verbinding versleuteling en certificaatverificatie](https://msdn.microsoft.com/library/azure/ff394108#encryption)voor meer informatie.


## <a name="authentication"></a>Verificatie

Verificatie verwijst naar het hoe u uw identiteit aantonen wanneer u verbinding maakt met de database. SQL-Database ondersteunt twee soorten verificatie:

 - **SQL-verificatie**, waarbij een gebruikersnaam en wachtwoord
 - **Azure Active Directory-verificatie**, dat wordt beheerd door Azure Active Directory identiteiten gebruikt en wordt ondersteund voor beheerde en geïntegreerde domeinen

Wanneer u de logische server voor uw database hebt gemaakt, kunt u een "-Serverbeheer" aanmelding opgegeven met een gebruikersnaam en wachtwoord. Deze referenties gebruikt, kunt u verifiëren met een database op die server als de database-eigenaar of "dbo." Als u gebruiken van Azure Active Directory-verificatie wilt, moet u een andere server voor beheerders genoemd de "Azure AD-beheerder' die is toegestaan voor het beheren van Azure AD-gebruikers en groepen maken. Deze beheerders kan ook alle bewerkingen die de beheerder van een gewone server kunt uitvoeren. Zie [verbinding maken met SQL Database met behulp van Azure Active Directory-verificatie](sql-database-aad-authentication.md) voor stapsgewijze instructies voor het maken van een Azure AD-beheerder om in te schakelen van Azure Active Directory-verificatie.

Als een goede gewoonte moet uw toepassing een ander account gebruiken om te verifiëren--deze manier kunt u de machtigingen om de toepassing te beperken en de risico's van schadelijke activiteit verkleinen wanneer uw toepassingscode vatbaar voor een SQL-injectieaanvallen is. De aanbevolen werkwijze is het opzetten van een [databasegebruiker opgenomen](https://msdn.microsoft.com/library/ff929188), zodat uw app om te verifiëren rechtstreeks naar één database. U kunt een container databasegebruiker die gebruikmaakt van SQL-verificatie door het uitvoeren van de volgende T-SQL-opdracht terwijl verbonden met uw gebruikersdatabase als de server beheerder login maken:

```
CREATE USER ApplicationUser WITH PASSWORD = 'strong_password'; -- SQL Authentication
```

Als u een Azure AD-beheerder hebt gemaakt, kunt u een container databasegebruiker die gebruikmaakt van Azure Active Directory-verificatie door het uitvoeren van de volgende T-SQL-opdracht terwijl verbonden met uw gebruikersdatabase als de beheerder van de Azure AD maken:

```
CREATE USER [Azure_AD_principal_name | Azure_AD_group_display_name] FROM EXTERNAL PROVIDER; -- Azure Active Directory Authentication
```

In beide gevallen moet de verbindingsreeks van de toepassing de referenties van deze gebruiker, in plaats van de server beheerder login, verbinding maken met de database opgeven.

Zie voor meer informatie over het verifiëren van met een SQL-Database, [Databases beheren en aanmeldingen in Azure SQL-Database](sql-database-manage-logins.md).


## <a name="authorization"></a>Autorisatie
Autorisatie verwijst naar wat u in een Azure SQL-Database doen kunt en dit wordt bepaald door uw gebruikersaccount rollidmaatschappen en machtigingen. Als een goede gewoonte, moet u gebruikers de minimaal benodigde bevoegdheden verlenen. Azure SQL-Database is dit eenvoudig worden beheerd met de rollen in T-SQL:

```
ALTER ROLE db_datareader ADD MEMBER ApplicationUser; -- allows ApplicationUser to read data
ALTER ROLE db_datawriter ADD MEMBER ApplicationUser; -- allows ApplicationUser to write data
```

De server-beheerdersaccount die u verbinding met maakt is een lid van db_owner, die toestemming te ondernemen binnen de database heeft. Sla dit account voor het implementeren van schema upgrades en andere beheertaken uit te voeren. Gebruik het account "ApplicationUser" met beperkte machtigingen op uw toepassing in de database verbinding maken met het minimaal benodigde bevoegdheden nodig zijn voor uw toepassing.

Er zijn andere manieren verder te beperken wat een gebruiker kan doen met Azure SQL-Database:

* [Databaserollen](https://msdn.microsoft.com/library/ms189121) dan db_datareader en db_datawriter kan worden gebruikt om krachtige toepassingen gebruikersaccounts of minder krachtige management accounts te maken.
* Gedetailleerde [machtigingen](https://msdn.microsoft.com/library/ms191291) kunt u bepalen welke bewerkingen die u kunt doen op afzonderlijke kolommen, tabellen, weergaven, procedures en andere objecten in de database.
* [Imitatie](https://msdn.microsoft.com/library/vstudio/bb669087) en [module-ondertekening](https://msdn.microsoft.com/library/bb669102) kunnen worden gebruikt voor het veilig machtigingen tijdelijk te verhogen.
* [Beveiliging op gebruikersniveau rij](https://msdn.microsoft.com/library/dn765131) kan worden gebruikt beperken welke rijen van een gebruiker kunt openen.
* [Gegevens Masking](sql-database-dynamic-data-masking-get-started.md) kan worden gebruikt om te beperken gevoelige gegevens weergeven.
* [Opgeslagen procedures](https://msdn.microsoft.com/library/ms190782) kan worden gebruikt om de acties die kunnen worden uitgevoerd op de database te beperken.

Databases en logische servers beheren vanaf de Portal van Azure-klassieke of gebruiken van de Azure resourcemanager-API wordt bepaald door uw portal gebruikersaccount roltoewijzingen. Zie voor meer informatie over dit onderwerp [Rolgebaseerd toegangsbeheer in Azure-Portal](../active-directory./role-based-access-control-configure.md).


## <a name="encryption"></a>Versleuteling

Azure SQL-Database kan bijdragen aan beveiliging van uw gegevens door te coderen van uw gegevens als dit "at rest' of die zijn opgeslagen in databasebestanden en back-ups, [transparante gegevens](http://go.microsoft.com/fwlink/?LinkId=526242)-versleuteling gebruikt. Voor uw database versleutelen, als de eigenaar van een database koppelen en uitvoeren:

```
ALTER DATABASE [AdventureWorks] SET ENCRYPTION ON;
```

Voor andere manieren om te versleutelen van uw gegevens geheimen, kunt u in het volgende overwegen:

* [Celniveau versleuteling](https://msdn.microsoft.com/library/ms179331.aspx) specifieke kolommen of zelfs cellen met gegevens coderen met verschillende versleuteling toetsen.
* Als u een Hardware Security Module of Centraal beheer van de belangrijkste hiërarchie versleuteling nodig, kunt u overwegen [Azure toets kluis met SQL Server in een VM Azure](http://blogs.technet.com/b/kv/archive/2015/01/12/using-the-key-vault-for-sql-server-encryption.aspx).
* [Altijd versleuteld](https://msdn.microsoft.com/library/mt163865.aspx) (in preview) maakt versleuteling transparante naar toepassingen en maakt clients gevoelige gegevens in clienttoepassingen zonder de toetsen versleuteling delen met SQL-Database versleutelen.

## <a name="auditing"></a>Controle

Het bijhouden en databasegebeurtenissen controleren kunt u voor het behoud van regelgeving en verdachte activiteit identificeren. Controle van de SQL-Database, kunt dat u op record gebeurtenissen in uw database een controle Meld u aan uw opslagruimte van Azure-account. Controle van de SQL-Database ook geïntegreerd met Microsoft Power BI te vergemakkelijken Inzoomen op rapporten en analyses. Zie [aan de slag met het controleren van de SQL-Database](sql-database-auditing-get-started.md)voor meer informatie.

SQL-Database bedreiging detectie biedt een extra beveiliging boven op controle. U kunt reageren op threats schiet door te geven beveiligingsmeldingen over afwijkende activiteiten. Zie [aan de slag met SQL Database bedreiging detectie](sql-database-threat-detection-get-started.md)voor meer informatie.  

## <a name="compliance"></a>Naleving

Naast de bovenstaande voorzieningen en functies waarmee u kunt uw toepassing die voldoen aan diverse naleving veiligheidsvereisten, Azure SQL-Database ook deel uitmaakt van de normale controles en ten opzichte van een aantal naleving worden is gecertificeerd. Zie voor meer informatie het [Vertrouwenscentrum in Microsoft Azure](https://azure.microsoft.com/support/trust-center/), waar u de meest recente lijst met [SQL-Database naleving certificaten](https://azure.microsoft.com/support/trust-center/services/)kunt vinden.
