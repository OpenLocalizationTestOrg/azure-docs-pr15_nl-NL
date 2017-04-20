<properties
   pageTitle="Richtlijnen voor de beveiliging van Azure SQL-Database en beperkingen | Microsoft Azure"
   description="Meer informatie over Microsoft Azure SQL Database-richtlijnen en beperkingen ten aanzien van beveiliging."
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
   ms.date="10/18/2016"
   ms.author="rickbyh"/>

# <a name="azure-sql-database-security-guidelines-and-limitations"></a>Richtlijnen voor beveiliging van Azure SQL-Database en beperkingen

In dit onderwerp worden de richtlijnen van Microsoft Azure SQL-Database en beperkingen ten aanzien van beveiliging. Houd rekening met de volgende punten bij het beheren van de beveiliging van uw Azure SQL-Databases.

## <a name="access-to-the-virtual-master-database"></a>Toegang tot de virtuele hoofddatabase

Meestal hoeft alleen beheerders van de toegang tot de hoofddatabase. Regelmatige toegang tot de gebruikersdatabase van elke moet tot en met de container geen beheerder databasegebruikers in elke database gemaakt. Wanneer u databasegebruikers van de container gebruikt, hoeft u niet te aanmeldingen maken in de hoofddatabase. Zie voor meer informatie [Opgenomen databasegebruikers - waardoor uw Database draagbare](https://msdn.microsoft.com/library/ff929188.aspx).


## <a name="firewall"></a>Firewall

De SQL Server-firewall, die voor de hele Azure SQL-Server als bereik meestal via de portal is geconfigureerd en moet de IP-adressen die worden gebruikt door beheerders alleen toelaten. Verbinding maken met de gebruikersdatabase van een een binnen bereik van een database als firewallregel wilt maken dat wordt geopend de benodigde bereik van IP-adressen voor elke database, en gebruik vervolgens de [sp_set_database_firewall_rule](https://msdn.microsoft.com/library/dn270010.aspx) Transact-SQL-instructie.

De Azure SQL Database-service is alleen beschikbaar via de TCP-poorten 1433. Voor toegang tot een SQL-Database vanaf uw computer, moet u ervoor zorgen dat uw firewall van de computer client uitgaande TCP-communicatie op TCP-poort 1433 is toegestaan. Als u niet nodig is voor andere toepassingen, blokkeren binnenkomende verbindingen op TCP-poorten 1433. 

Als onderdeel van het verbindingsproces, verbindingen van Azure virtuele machines omgeleid naar een ander IP-adres en poort unieke voor elke werknemer rol. Het poortnummer is in het bereik van 11000 tot 11999. Zie voor meer informatie over TCP-poorten, [behalve 1433 voor ADO.NET 4.5 en V12 voor SQL-Database](sql-database-develop-direct-route-ports-adonet-v12.md).


## <a name="authentication"></a>Verificatie

Active Directory-verificatie gebruiken (geïntegreerde beveiliging) indien mogelijk. Zie [verbinding maken met SQL Database met behulp van Azure Active Directory-verificatie](sql-database-aad-authentication.md)en [kiezen van een verificatiemodus](https://msdn.microsoft.com/library/ms144284.aspx) in SQL Server Books Online voor informatie over het configureren van AD-verificatie. 

Gebruik container databasegebruikers om de schaalbaarheid verbeteren. Zie voor meer informatie [Opgenomen databasegebruikers - waardoor uw Database draagbare](https://msdn.microsoft.com/library/ff929188.aspx), [CREATE USER (Transact-SQL)](https://technet.microsoft.com/library/ms173463.aspx)en [Databases opgenomen](https://technet.microsoft.com/library/ff929071.aspx).

De Database-Engine wordt gesloten verbindingen die niet actief geweest meer dan 30 minuten gedurende. De verbinding login opnieuw moet voordat deze kan worden gebruikt.

Continu actieve verbindingen met SQL-Database (uitgevoerd door de Database Engine) reauthorization vereisen op minstens elke 10 uur. De Database-Engine probeert reauthorization met het oorspronkelijk verzonden wachtwoord en geen invoer van de gebruiker is vereist. Prestatieoverwegingen wanneer u een wachtwoord opnieuw is ingesteld in SQL-Database, de verbinding is niet worden geverifieerd, zelfs als de verbinding wordt hersteld vanwege groepsgewijze. Dit is het verschil tussen het gedrag van lokale SQL Server. Als het wachtwoord is gewijzigd nadat de verbinding is in eerste instantie geautoriseerd, wordt de verbinding moet worden beëindigd en wordt een nieuwe verbinding gemaakt met het nieuwe wachtwoord. Een gebruiker met de machtiging DATABASEVERBINDING BEËINDIGEN kan een verbinding met SQL-Database expliciet eindigen met de opdracht [verwijderen](https://msdn.microsoft.com/library/ms173730.aspx) .

## <a name="logins-and-users"></a>Aanmeldingen en gebruikers

Er zijn beperkingen bij het beheren van aanmeldingen en gebruikers in SQL-Database.


- U mag worden verbonden met **de hoofddatabase** bij het uitvoeren van de ``CREATE/ALTER/DROP DATABASE`` instructies. -De databasegebruiker in de hoofddatabase die overeenkomt met de hoofdsom login serverniveau niet mag worden gewijzigd of verwijderd. 
- Nederlandse is de standaardtaal van de serverniveau principal login.
- Alleen de beheerders (serverniveau principal login of Azure AD-beheerder) en de leden van de rol van de database **dbmanager** in **de hoofddatabase** gemachtigd om uit te voeren de ``CREATE DATABASE`` en ``DROP DATABASE`` instructies.
- U mag worden verbonden met de hoofddatabase bij het uitvoeren van de ``CREATE/ALTER/DROP LOGIN`` instructies. Echter aanmeldingen gebruiken wordt afgeraden. Gebruik in plaats hiervan de container databasegebruikers.
- Als u wilt koppelen aan een database, moet u de naam van de database in de verbindingstekenreeks opgeven.
- Alleen de hoofdsom login serverniveau en de leden van de rol van de database **loginmanager** in **de hoofddatabase** gemachtigd zijn om uit te voeren de ``CREATE LOGIN``, ``ALTER LOGIN``, en ``DROP LOGIN`` instructies.
- Bij het uitvoeren van de ``CREATE/ALTER/DROP LOGIN`` en ``CREATE/ALTER/DROP DATABASE`` instructies in een ADO.NET-toepassing, met parameters opdrachten is niet toegestaan. Zie [opdrachten en Parameters](https://msdn.microsoft.com/library/ms254953.aspx)voor meer informatie.
- Bij het uitvoeren van de ``CREATE/ALTER/DROP DATABASE`` en ``CREATE/ALTER/DROP LOGIN`` overzichten, elk van deze instructies moet de alleen-instructie in een batch Transact-SQL. Anders optreedt een fout. Bijvoorbeeld de volgende Transact-SQL Hiermee wordt gecontroleerd of de database bestaat. Indien aanwezig, een ``DROP DATABASE`` instructie heet de database wilt verwijderen. Omdat de ``DROP DATABASE`` instructie is niet de alleen-instructie in de batch, Transact-SQL-instructie resulteert uitvoeren van de volgende handelingen uit in een fout.

```
IF EXISTS (SELECT [name]
           FROM   [sys].[databases]
           WHERE  [name] = N'database_name')
     DROP DATABASE [database_name];
GO
```

- Bij het uitvoeren van de ``CREATE USER`` instructie met de ``FOR/FROM LOGIN`` optie, moet de alleen-instructie in een batch Transact-SQL.
- Bij het uitvoeren van de ``ALTER USER`` instructie met de ``WITH LOGIN`` optie, moet de alleen-instructie in een batch Transact-SQL.
- Naar ``CREATE/ALTER/DROP`` een gebruiker moet de ``ALTER ANY USER`` machtiging voor de database.
- Wanneer de eigenaar van de databaserol van een probeert toevoegen of verwijderen van een andere databasegebruiker of naar de databaserol van deze, de volgende fout kan optreden: **gebruiker of rol 'Naam' bestaat niet in deze database.** Deze fout treedt op omdat de gebruiker niet zichtbaar voor de eigenaar bent is. U lost dit probleem op door verlenen de roleigenaar van de de ``VIEW DEFINITION`` machtiging voor de gebruiker. 

Zie voor meer informatie over aanmeldingen en gebruikers [beheren databases en aanmeldingen in Azure SQL-Database](sql-database-manage-logins.md).


## <a name="see-also"></a>Zie ook

[Azure SQL Database-Firewall](sql-database-firewall-configure.md)

[Hoe u: firewallinstellingen (Azure SQL-Database) configureren](sql-database-configure-firewall-settings.md)

[Databases en aanmeldingen in Azure SQL-Database beheren](sql-database-manage-logins.md)

[Beveiligingscentrum voor SQL Server-Database Engine en Azure SQL-Database](https://msdn.microsoft.com/library/bb510589)
