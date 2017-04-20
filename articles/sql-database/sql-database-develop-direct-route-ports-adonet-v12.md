<properties 
    pageTitle="Behalve 1433 voor SQL-Database | Microsoft Azure"
    description="Clientverbindingen van ADO.NET met Azure SQL Database V12 soms de proxy overslaan en rechtstreeks ermee aan de database. Poorten dan 1433 worden belangrijke."
    services="sql-database"
    documentationCenter=""
    authors="MightyPen"
    manager="jhubbard"
    editor="" />


<tags 
    ms.service="sql-database" 
    ms.workload="drivers"
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="08/17/2016"
    ms.author="annemill"/>


# <a name="ports-beyond-1433-for-adonet-45-and-sql-database-v12"></a>Behalve 1433 voor ADO.NET 4.5 en SQL-Database V12


In dit onderwerp worden de wijzigingen die Azure SQL Database V12 naar het gedrag van de verbinding van klanten die gebruikmaken van ADO.NET 4.5 of een latere versie brengt.


## <a name="v11-of-sql-database-port-1433"></a>V11: van de SQL-Database: poort 1433


Als uw clientprogramma gebruikmaakt van ADO.NET 4.5 verbinding maken en de query met V11: SQL-Database, is de interne volgorde als volgt:


1. ADO.NET probeert verbinding maken met SQL-Database.

2. ADO.NET poort 1433 gebruikt om te bellen van een module middleware en de middleware verbinding maakt met SQL-Database.

3. SQL-Database stuurt de reactie terug naar de middleware, die het antwoord op ADO.NET met poort 1433 doorstuurt.


**Terminologie:** De voorgaande reeks door te zeggen dat ADO.NET met SQL-Database samenwerkt met behulp van de *proxy routeren*worden beschreven. Als er geen middleware gaat, werd er gezegd dat de *directe route* is gebruikt.


## <a name="v12-of-sql-database-outside-vs-inside"></a>V12 van SQL-Database: buiten de vs binnen


Voor verbindingen met V12, moeten we vragen of uw clientprogramma wordt uitgevoerd *binnen* of *buiten* de grens Azure cloud. De subsecties bespreken twee veelvoorkomende scenario's.


#### <a name="outside-client-runs-on-your-desktop-computer"></a>*Buitenkant:* Client wordt uitgevoerd op uw desktopcomputer


Poort 1433 is de enige poort die moet zijn geopend op uw desktopcomputer waarop de clienttoepassing SQL-Database.


#### <a name="inside-client-runs-on-azure"></a>*Binnen:* Client wordt uitgevoerd op Azure


Als de klant wordt uitgevoerd binnen de grens Azure cloud, gebruikt de functie wat we een *rechtstreekse route* interactie met de SQL-databaseserver kunt bellen. Nadat u een verbinding tot stand is gebracht, verder interactie tussen de client en database gebruikmaakt van geen middleware-proxy.


De volgorde is als volgt:


1. ADO.NET 4,5 (of later) een kort interactie met de cloud Azure Start en een dynamisch geïdentificeerd poortnummer ontvangt.
 - Het poortnummer in dynamisch geïdentificeerd is in het bereik van 11000-11999 of 14000-14999.

2. ADO.NET maakt vervolgens verbinding met de SQL-databaseserver rechtstreeks met geen middleware ertussen.

3. Query's die rechtstreeks naar de database worden verzonden en resultaten worden geretourneerd rechtstreeks naar de client.


Zorg ervoor dat de poort bereiken van 11000-11999 en 14000-14999 op uw Azure clientcomputer zijn beschikbaar voor 4.5 ADO.NET-client interacties met de SQL-Database V12 links.

- Poorten in het bereik moet met name vrij van andere uitgaande upblokkering.

- De **Windows Firewall met geavanceerde beveiliging** besturingselementen op uw VM Azure, de poortinstellingen voor de.
 - U kunt de [gebruikersinterface van de firewall van](http://msdn.microsoft.com/library/cc646023.aspx) een regel waarvoor u het **TCP-** protocol samen met een poortbereik met de syntaxis van de zoals **11000-11999 opgeven**wilt toevoegen.


## <a name="version-clarifications"></a>Versie toelichting


In dit gedeelte verduidelijkt de bijnamen die naar productversies van een verwijzen. De lijst bevat ook enkele koppelingen tussen versies tussen producten.


#### <a name="adonet"></a>ADO.NET


- ADO.NET 4.0 ondersteunt het protocol TDS 7.3, maar niet 7.4.
- ADO.NET 4.5 en hoger ondersteunt het protocol TDS 7.4.


#### <a name="sql-database-v11-and-v12"></a>SQL-Database V11: en V12


De client verbinding verschillen tussen V11: SQL-Database en V12 zijn gemarkeerd in dit onderwerp.


*Notitie:* De Transact-SQL-instructie `SELECT @@version;` geeft als resultaat een waarde die beginnen met een getal zoals '11.' of '12.', en de die overeenkomen met de versienamen van onze van V11: en V12 voor SQL-Database.


## <a name="related-links"></a>Verwante koppelingen


- ADO.NET 4.6 is uitgebracht op 20 juli 2015. Een blog aankondiging uit het .NET-team vindt u [hier](http://blogs.msdn.com/b/dotnet/archive/2015/07/20/announcing-net-framework-4-6.aspx).


- ADO.NET 4.5 is uitgebracht op 15 augustus 2012. Een blog aankondiging uit het .NET-team vindt u [hier](http://blogs.msdn.com/b/dotnet/archive/2012/08/15/announcing-the-release-of-net-framework-4-5-rtm-product-and-source-code.aspx).
 - Een blogbericht over ADO.NET 4.5.1 vindt u [hier](http://blogs.msdn.com/b/dotnet/archive/2013/06/26/announcing-the-net-framework-4-5-1-preview.aspx).


- [Vervolgkeuzelijst TDS protocol-versie](http://www.freetds.org/userguide/tdshistory.htm)


- [SQL-Database ontwikkelen-overzicht](sql-database-develop-overview.md)


- [Azure SQL Database-firewall](sql-database-firewall-configure.md)


- [Hoe u: firewallinstellingen configureren op SQL-Database](sql-database-configure-firewall-settings.md)

