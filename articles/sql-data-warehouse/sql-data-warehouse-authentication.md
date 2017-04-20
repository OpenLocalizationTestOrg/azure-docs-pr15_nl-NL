<properties
   pageTitle="Verificatie van Azure SQL datawarehouse | Microsoft Azure"
   description="Azure Active Directory (AAD) en SQL Server-verificatie naar Azure SQL Data Warehouse."
   services="sql-data-warehouse"
   documentationCenter=""
   authors="byham"
   manager="barbkess"
   editor=""
   tags=""/>

<tags
   ms.service="sql-data-warehouse"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="data-management"
   ms.date="09/24/2016"
   ms.author="rickbyh;barbkess;sonyama"/>

# <a name="authentication-to-azure-sql-data-warehouse"></a>Verificatie van Azure SQL datawarehouse

> [AZURE.SELECTOR]
- [Beveiligingsoverzicht](sql-data-warehouse-overview-manage-security.md)
- [Verificatie](sql-data-warehouse-authentication.md)
- [Versleuteling (Portal)](sql-data-warehouse-encryption-tde.md)
- [Versleuteling (T-SQL)](sql-data-warehouse-encryption-tde-tsql.md)

Als u wilt verbinden met SQL Data Warehouse, moet u beveiligingsreferenties voor verificatiedoeleinden doorgeven. Na het maken van een verbinding, worden bepaalde instellingen zijn geconfigureerd als onderdeel van het starten van uw query-sessie.  

Zie voor meer informatie over beveiligings- en het inschakelen van verbindingen met uw gegevenswarehouse, [Secure een database in SQL Data Warehouse][].

## <a name="sql-authentication"></a>SQL-verificatie
Als u wilt verbinden met SQL Data Warehouse, moet u de onderstaande informatie opgeven:

- Volledig gekwalificeerde servernaam
- SQL-verificatie opgeven
- Gebruikersnaam
- Wachtwoord
- Standaard-database (optioneel)

Standaard wordt uw verbinding maken met *de hoofddatabase* en niet de gebruikersdatabase. Als u wilt verbinden met uw gebruikersdatabase, kunt u twee dingen doen:

- Geef de standaard-database tijdens de registratie van de server met de SQL Server Object Explorer in SSDT, SSMS, of in uw toepassing-verbindingsreeks. Bijvoorbeeld de parameter InitialCatalog voor een ODBC-verbinding.
- Markeer de database voordat u een sessie maakt in SSDT.

> [AZURE.NOTE] De Transact-SQL-instructie **Gebruik MijnDatabase;** wordt niet ondersteund voor het wijzigen van de database van een verbinding. Voor hulp verbinding maken met SQL Data Warehouse met SSDT, raadpleegt u het artikel [Query met Visual Studio][] .

## <a name="azure-active-directory-aad-authentication"></a>Azure Active Directory (AAD)-verificatie

[Azure Active Directory] [ What is Azure Active Directory] verificatie is een om verbinding te maken met Microsoft Azure SQL Data Warehouse met behulp van identiteiten in Azure Active Directory (Azure AD). Verificatie van de Azure Active Directory, kunt u de identiteit van de databasegebruikers en andere Microsoft-services op één centrale locatie centraal beheren. Centraal beheer van de ID vormt een handig SQL Data Warehouse gebruikers beheren en eenvoudiger machtigingen beheren. 

### <a name="benefits"></a>Voordelen

Azure Active Directory-voordelen zijn onder andere:

- Biedt een alternatief voor SQL Server-verificatie.
- Houdt de verspreiding van gebruikersidentiteit voor databaseservers.
- Wachtwoord draaiing op één plaats kunt
- Databasemachtigingen voor het gebruik van externe (AAD)-groepen beheren.
- Opslaan van wachtwoorden voorkomt doordat geïntegreerde Windows-verificatie en andere vormen van verificatie wordt ondersteund door Azure Active Directory.
- Gebruik opgenomen gebruikers van de database om identiteiten op het databaseniveau van de te verifiëren.
- Ondersteunt token gebaseerde verificatie voor toepassingen die verbinding maken met SQL Data Warehouse.
- Ondersteunt meervoudige verificatie via Active Directory-universele verificatie voor SQL Server Management Studio. Zie [SSMS ondersteuning voor Azure AD MFA met SQL-Database en SQL Data Warehouse](../sql-database/sql-database-ssms-mfa-authentication.md)voor een beschrijving van meervoudige verificatie.

> [AZURE.NOTE] Azure Active Directory is nog steeds relatief nieuw en heeft enkele beperkingen. Om ervoor te zorgen dat Azure Active Directory een goede aanpassen voor uw omgeving is, Zie [Azure AD voorzieningen en beperkingen][], specifiek de aanvullende overwegingen.

### <a name="configuration-steps"></a>Volgende configuratiestappen uit

Volg deze stappen om de verificatie van de Azure Active Directory configureren.

1. Maken en invullen in een Azure Active Directory
2. Optioneel: Koppelen of de active directory die momenteel is gekoppeld aan uw Azure-abonnement wijzigen
3. Maak een Azure Active Directory-beheerder voor Azure SQL Data Warehouse.
4. De clientcomputers configureren
5. Container databasegebruikers in uw database toegewezen aan Azure AD identiteiten maken
6. Verbinding maken met uw gegevenswarehouse via Azure AD identiteiten

Azure Active Directory-gebruikers worden momenteel niet weergegeven in SSDT Object Explorer. Als een tijdelijke oplossing de gebruikers in [sys.database_principals](https://msdn.microsoft.com/library/ms187328.aspx)te bekijken.
  
### <a name="find-the-details"></a>De details
- Voltooi de gedetailleerde stappen. De gedetailleerde stappen voor het configureren en gebruiken van Azure Active Directory-verificatie zijn bijna identiek voor Azure SQL-Database en Azure SQL Data Warehouse. De gedetailleerde stappen in het onderwerp [verbinding maken met SQL-Database of SQL Data Warehouse met behulp van Azure Active Directory-verificatie](../sql-database/sql-database-aad-authentication.md).
- Aangepaste databaserollen maken en gebruikers toevoegen aan de rollen. Vervolgens gedetailleerde machtigingen verlenen voor de rollen. Zie [Aan de slag met Database Engine machtigingen](https://msdn.microsoft.com/library/mt667986.aspx)voor meer informatie.

## <a name="next-steps"></a>Volgende stappen

Als u wilt beginnen bij het controleren van uw gegevens BTW-records met Visual Studio en andere toepassingen, de [Query met Visual Studio][]weer te geven.

<!-- Article references -->
[Een database in SQL Data Warehouse beveiligen]: ./sql-data-warehouse-overview-manage-security.md
[Query met Visual Studio]: ./sql-data-warehouse-query-visual-studio.md
[What is Azure Active Directory]: ../active-directory/active-directory-whatis.md
[Azure AD-voorzieningen en beperkingen]: ../sql-database/sql-database-aad-authentication.md#azure-ad-features-and-limitations
