<properties
   pageTitle="Gedistribueerde transacties in de cloud-databases"
   description="Overzicht van elastische databasetransacties met Azure SQL-Database"
   services="sql-database"
   documentationCenter=""
   authors="torsteng"
   manager="jhubbard"
   editor="torsteng"/>

<tags
   ms.service="sql-database"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="sql-database"
   ms.date="05/27/2016"
   ms.author="torsteng"/>

# <a name="distributed-transactions-across-cloud-databases"></a>Gedistribueerde transacties in de cloud-databases

Elastische databasetransacties voor Azure SQL-Database (SQL DB) kunnen u uitvoeren van transacties die meerdere databases in SQL DB's beslaan. Elastische databasetransacties voor SQL DB beschikbaar zijn voor .NET-toepassingen met behulp van ADO .NET en integreren met de vertrouwde programmeren met de klassen [System.Transaction](https://msdn.microsoft.com/library/system.transactions.aspx) . Als u de bibliotheek, raadpleegt u [.NET Framework 4.6.1 (Web Installer)](https://www.microsoft.com/download/details.aspx?id=49981).

On-premises vereist zoals een scenario meestal Microsoft Distributed Transaction Coordinator (MSDTC) uitgevoerd. Aangezien MSDTC niet beschikbaar voor Platform-als-een-Service-toepassing in Azure wordt aangegeven is, is de mogelijkheid om te coördineren gedistribueerde transacties nu rechtstreeks geïntegreerd in SQL DB. Toepassingen kunnen verbinding maken met een SQL-Database aan gedistribueerde transacties starten en een van de databases wordt de gedistribueerde transactie transparant coördineren zoals wordt weergegeven in de volgende afbeelding. 

  ![Gedistribueerde transacties met Azure SQL-Database met elastische databasetransacties ][1]

## <a name="common-scenarios"></a>Gebruikelijke scenario 's

Elastische databasetransacties voor SQL DB mogelijk dat toepassingen atomische wijzigingen aanbrengen in de gegevens die zijn opgeslagen in een aantal verschillende SQL-Databases. Het voorbeeld is gericht op aan de clientzijde ontwikkeling ervaringen in C# en .NET. Een serverzijde-ervaring met T-SQL is gepland voor een later tijdstip.  
Elastische databasetransacties zijn bedoeld voor de volgende scenario's:

* Meerdere databasetoepassingen in Azure wordt aangegeven: met dit scenario gegevens is verticaal partities over meerdere databases in SQL DB zodanig dat verschillende soorten gegevens zich op andere databases bevinden. Sommige bewerkingen vragen om wijzigingen in de gegevens die in twee of meer databases wordt bewaard. De toepassing gebruikt elastische databasetransacties coördineren van de wijzigingen in databases en ervoor zorgen atomiciteit.

* Een laptopgeheugen databasetoepassingen in Azure wordt aangegeven: met dit scenario de gegevenslaag gebruikt de [elastische Database clientbibliotheek](sql-database-elastic-database-client-library.md) of self-sharding horizontaal partitioneren de gegevens over de verschillende databases in SQL-database. Eén laten opvallen use-case is dat u voor atomische wijzigingen voor een toepassing voor een laptopgeheugen meerdere tenant wanneer wijzigingen tenants omvatten. Denk na bijvoorbeeld van een overgang van de ene naar de andere beide die zich op andere databases. Een tweede hoofdletters/kleine letters is fijnmazige sharding zo ins dat u nodig hebt voor een grote tenant die op zijn beurt meestal houdt in dat sommige atomaire bewerkingen moet zich uitstrekt over meerdere databases die wordt gebruikt voor dezelfde tenant. Een derde hoofdletters/kleine letters is atomaire updates om te verwijzen naar gegevens die worden gerepliceerd naar databases. Atomaire, uitgevoerde, bewerkingen langs deze regels kunnen nu worden gecoördineerde over meerdere databases met preview.
Bevestiging in elastische databasetransacties gebruiken om ervoor te zorgen transactie atomiciteit over databases. Het is een goede geschikte optie voor transacties waarbij u gebruikmaakt van minder dan 100 databases per keer binnen een transactie. Deze limieten gelden niet, maar een kan verwachten prestaties en succes eenheidstarieven voor elastische databasetransacties afnemen wanneer deze limieten overschrijden.


## <a name="installation-and-migration"></a>Installatie- en migratie

De mogelijkheden voor elastische databasetransacties in SQL DB zijn opgenomen in updates voor de bibliotheken .NET System.Data.dll en System.Transactions.dll. De dll-bestanden Zorg ervoor dat doorvoeren in twee fasen zo nodig om ervoor te zorgen atomiciteit wordt gebruikt. Als u wilt beginnen met het ontwikkelen van toepassingen gebruiken elastische databasetransacties, [.NET Framework 4.6.1](https://www.microsoft.com/download/details.aspx?id=49981) of een latere versie te installeren. Wanneer u zich in een eerdere versie van .NET framework, transacties om te promoveren tot een gedistribueerde transactie, mislukt en wordt een uitzondering worden gegenereerd.

Na de installatie, kunt u de gedistribueerde transactie API's gebruiken in System.Transactions met verbindingen met SQL-DB. Als u bestaande MSDTC toepassingen deze opties hebt, gewoon bouw die tabellen opnieuw uw bestaande toepassingen voor .NET 4.6 na de installatie van de 4.6.1 Framework. Als uw projecten .NET 4.6 doelgerichte, ze worden automatisch de bijgewerkte dll-bestanden van de nieuwe versie van Framework gebruikt en nu slagen gedistribueerde transactie die API u in combinatie met verbindingen met SQL-DB belt.

Houd er rekening mee dat elastische databasetransacties is niet vereist installatie van MSDTC. Elastische databasetransacties worden in plaats daarvan rechtstreeks beheerd door en haaienvin SQL DB. Hierdoor aanzienlijk wordt vereenvoudigd cloud-scenario's aangezien een implementatie van MSDTC niet nodig is is voor gebruik van gedistribueerde transacties met SQL-DB. 4 van sectie wordt uitgelegd uitgebreider hoe elastische databasetransacties en de vereiste .NET framework samen met de cloud-toepassingen Azure implementeren.

## <a name="development-experience"></a>Development-ervaring

### <a name="multi-database-applications"></a>Meerdere databasetoepassingen

Het volgende voorbeeld wordt de vertrouwde programmeren gebruikt met .NET System.Transactions. De klas TransactionScope tot stand brengt een de transactie in .NET. (Een 'de transactie"is een die zich in de huidige thread bevindt.) Alle verbindingen in de TransactionScope geopend deelnemen aan de transactie. Als verschillende databases deelnemen, wordt de transactie is automatisch beschikbaar tot een gedistribueerde transactie. Het resultaat van de transactie wordt bepaald door het instellen van het bereik om uit te voeren om aan te geven van een doorvoeren.

    using (var scope = new TransactionScope())
    {
        using (var conn1 = new SqlConnection(connStrDb1))
        {
            conn1.Open();
            SqlCommand cmd1 = conn1.CreateCommand();
            cmd1.CommandText = string.Format("insert into T1 values(1)");
            cmd1.ExecuteNonQuery();
        }

        using (var conn2 = new SqlConnection(connStrDb2))
        {
            conn2.Open();
            var cmd2 = conn2.CreateCommand();
            cmd2.CommandText = string.Format("insert into T2 values(2)");
            cmd2.ExecuteNonQuery();
        }

        scope.Complete();
    }

### <a name="sharded-database-applications"></a>Een laptopgeheugen databasetoepassingen
 
Elastische databasetransacties voor SQL DB ondersteunen ook coördineren van gedistribueerde transacties waar u de methode OpenConnectionForKey van de bibliotheek van de client elastische database gebruiken om te openen voor een scaled out gegevens verbindingen laag. Houd rekening met zaken waar u moet transacties consistentie voor wijzigingen in verschillende verschillende sharding sleutelwaarden garanderen. Verbindingen met het hosten van de verschillende sharding-sleutelwaarden shards zijn brokered OpenConnectionForKey gebruiken. De verbindingen kunnen verwijzen naar andere shards in het algemeen zijn, zodat een gedistribueerde transactie ervoor zorgen dat transacties garanties vereist. Het volgende voorbeeld ziet u deze methode. Het wordt ervan uitgegaan dat een variabele met de naam van shardmap wordt gebruikt om aan te geven van een kaart shard van de bibliotheek van de client elastische database:

    using (var scope = new TransactionScope())
    {
        using (var conn1 = shardmap.OpenConnectionForKey(tenantId1, credentialsStr))
        {
            conn1.Open();
            SqlCommand cmd1 = conn1.CreateCommand();
            cmd1.CommandText = string.Format("insert into T1 values(1)");
            cmd1.ExecuteNonQuery();
        }
        
        using (var conn2 = shardmap.OpenConnectionForKey(tenantId2, credentialsStr))
        {
            conn2.Open();
            var cmd2 = conn2.CreateCommand();
            cmd2.CommandText = string.Format("insert into T1 values(2)");
            cmd2.ExecuteNonQuery();
        }

        scope.Complete();
    }


## <a name="net-installation-for-azure-cloud-services"></a>.NET-installatie oplossen voor Cloudservices van Azure

Azure bevat verschillende aanbiedingen naar host .NET-toepassingen. Een vergelijking van de verschillende aanbiedingen is beschikbaar in de [App-Azure-Service, Cloudservices, en virtuele Machines vergelijking](../app-service-web/choose-web-site-cloud-service-vm.md). Als de Gast OS de aanbod kleiner dan .NET 4.6.1 die zijn vereist voor elastische transacties is, moet u de Gast OS upgraden naar 4.6.1. 

Voor Services van Azure-App, worden upgrades naar de Gast OS momenteel niet ondersteund. Voor Azure virtuele Machines gewoon Meld u aan bij de VM en voert u het installatieprogramma voor de meest recente .NET framework. Voor Azure-Cloudservices moet u de installatie van een nieuwere versie van .NET in de opstarttaken van uw implementatie opnemen. De concepten en stappen worden beschreven in [.NET installeren op een rol Cloud-Service](../cloud-services/cloud-services-dotnet-install-dotnet.md).  

Houd er rekening mee dat het installatieprogramma voor .NET 4.6.1 meer tijdelijke tijdens de bootstrapping op Azure cloudservices dan het installatieprogramma voor .NET 4.6 wellicht nodig. Om ervoor te zorgen goed wilt installeren, moet u vergroten tijdelijke opslagruimte voor uw Azure cloudservice in uw bestand ServiceDefinition.csdef in de sectie LocalResources en de omgevingsinstellingen van uw taak opstarten, zoals wordt weergegeven in het onderstaande voorbeeld:

    <LocalResources>
    ...
        <LocalStorage name="TEMP" sizeInMB="5000" cleanOnRoleRecycle="false" />
        <LocalStorage name="TMP" sizeInMB="5000" cleanOnRoleRecycle="false" />
    </LocalResources>
    <Startup>
        <Task commandLine="install.cmd" executionContext="elevated" taskType="simple">
            <Environment>
        ...
                <Variable name="TEMP">
                    <RoleInstanceValue xpath="/RoleEnvironment/CurrentInstance/LocalResources/LocalResource[@name='TEMP']/@path" />
                </Variable>
                <Variable name="TMP">
                    <RoleInstanceValue xpath="/RoleEnvironment/CurrentInstance/LocalResources/LocalResource[@name='TMP']/@path" />
                </Variable>
            </Environment>
        </Task>
    </Startup>
    
## <a name="transactions-across-multiple-servers"></a>Transacties voor meerdere servers

Elastische databasetransacties worden ondersteund voor verschillende logische servers in Azure SQL-Database. Meerdere logische server grenzen, moeten de deelnemende servers eerst worden ingevoerd in een relatie onderlinge communicatie. Zodra de relatie communicatie is gebracht, kan elke database in een van de twee servers elastische transacties van databases uit de andere server kan deelnemen. Met transacties die meer dan twee logische servers in beslag nemen, moet een relatie communicatie op hun plaats staan voor elk paar logische servers.

Gebruik de volgende PowerShell-cmdlets voor het beheren van de communicatie tussen servers relaties voor elastische databasetransacties:

* **Nieuw-AzureRmSqlServerCommunicationLink**: Gebruik deze cmdlet om een nieuwe communicatie-relatie tussen twee logische servers in Azure SQL-database maken. De relatie is symmetric hetgeen betekent dat beide servers transacties met de andere server kunnen starten.
* **Get-AzureRmSqlServerCommunicationLink**: Gebruik deze cmdlet om bestaande communicatie relaties en hun eigenschappen te halen.
* **Verwijderen AzureRmSqlServerCommunicationLink**: Gebruik deze cmdlet om het verwijderen van een bestaande communicatie-relatie. 


## <a name="monitoring-transaction-status"></a>Transactiestatus controleren

Gebruik dynamische Management weergaven (DMVs) in SQL DB monitor status en voortgang van uw lopende elastische databasetransacties. Alle DMVs die betrekking hebben op transacties zijn relevant voor gedistribueerde transacties in SQL-database. U kunt de bijbehorende lijst met DMVs hier vinden: [transactie gerelateerde dynamische Management weergaven en functies (Transact-SQL)](https://msdn.microsoft.com/library/ms178621.aspx).
 
Deze DMVs zijn met name handig:

* **sys.dm\_transactie\_actieve\_transacties**: lijsten momenteel actief transacties en hun status. De kolom UOW (per eenheid van werk) kan vaststellen om wat de verschillende onderliggende transacties die deel uitmaakt van dezelfde gedistribueerde transactie. Alle transacties binnen dezelfde gedistribueerde transactie uitvoeren dezelfde UOW waarde. Zie de [documentatie DMV](https://msdn.microsoft.com/library/ms174302.aspx) voor meer informatie.
* **sys.dm\_transactie\_database\_transacties**: biedt extra informatie over transacties, zoals de plaatsing van de transactie in het logboek. Zie de [documentatie DMV](https://msdn.microsoft.com/library/ms186957.aspx) voor meer informatie.
* **sys.dm\_transactie\_vergrendelingen**: vindt u informatie over de vergrendelingen die momenteel zijn ingesteld door lopende transacties. Zie de [documentatie DMV](https://msdn.microsoft.com/library/ms190345.aspx) voor meer informatie.

## <a name="limitations"></a>Beperkingen 

De volgende beperkingen zijn momenteel van toepassing op elastische databasetransacties in SQL-database:

* Alleen transacties over databases in SQL DB worden ondersteund. Andere [X / Open XA](https://en.wikipedia.org/wiki/X/Open_XA) resource providers en databases buiten SQL DB geen deel uitmaken van elastische databasetransacties. Dit betekent dat elastische databasetransacties kunnen geen on-premises SQL Server en Azure SQL-Databases uitrekken. Voor gedistribueerde transacties on-premises, gaat u verder met het MSDTC gebruiken. 
* Alleen client samengestelde transacties van een toepassing voor .NET worden ondersteund. Aan de clientzijde ondersteuning voor T-SQL zoals BEGIN DISTRIBUTED TRANSACTION is gepland, maar nog niet beschikbaar. 
* Alleen databases op Azure SQL DB V12 worden ondersteund.
* Transacties op WCF-services worden niet ondersteund. Stel, hebt u een WCF-service-methode die wordt uitgevoerd van een transactie. Als u het gesprek binnen een transactiebereik, mislukt als een [System.ServiceModel.ProtocolException](https://msdn.microsoft.com/library/system.servicemodel.protocolexception).

## <a name="additional-resources"></a>Aanvullende informatie

Nog niet via elastische database mogelijkheden voor uw Azure-toepassingen? Bekijk onze [Documentatieoverzicht](https://azure.microsoft.com/documentation/learning-paths/sql-database-elastic-scale/). Neem contact maken met ons op het [forum van de SQL-Database](http://social.msdn.microsoft.com/forums/azure/home?forum=ssdsgetstarted) en voor functieverzoeken verzenden, voeg ze naar de [SQL-Database feedback-forum](https://feedback.azure.com/forums/217321-sql-database/)voor vragen.

<!--Image references-->
[1]: ./media/sql-database-elastic-transactions-overview/distributed-transactions.png



