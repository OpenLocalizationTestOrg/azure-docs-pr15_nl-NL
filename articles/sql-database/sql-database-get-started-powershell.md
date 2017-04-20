<properties
    pageTitle="Nieuwe installatie van SQL-Database met PowerShell | Microsoft Azure"
    description="Lees meer over het maken van een SQL-database met PowerShell. Algemene database-configuratietaken kunnen worden beheerd via PowerShell-cmdlets."
    keywords="maken van nieuwe sql-database, database instellen"
    services="sql-database"
    documentationCenter=""
    authors="stevestein"
    manager="jhubbard"
    editor="cgronlun"/>

<tags
    ms.service="sql-database"
    ms.devlang="NA"
    ms.topic="hero-article"
    ms.tgt_pltfrm="powershell"
    ms.workload="data-management"
    ms.date="08/19/2016"
    ms.author="sstein"/>

# <a name="create-a-sql-database-and-perform-common-database-setup-tasks-with-powershell-cmdlets"></a>Een SQL-database maken en uitvoeren van algemene database configuratietaken met PowerShell-cmdlets


> [AZURE.SELECTOR]
- [Azure-portal](sql-database-get-started.md)
- [PowerShell](sql-database-get-started-powershell.md)
- [C#](sql-database-get-started-csharp.md)



Informatie over het maken van een SQL-database met behulp van de PowerShell-cmdlets. (Zie [een nieuwe elastische database-toepassingen met PowerShell maken](sql-database-elastic-pool-create-powershell.md)voor het maken van elastische databases,.)


[AZURE.INCLUDE [Start your PowerShell session](../../includes/sql-database-powershell.md)]

## <a name="database-setup-create-a-resource-group-server-and-firewall-rule"></a>Database-instellingen: een resourcegroep, server en firewallregel maken

Nadat u toegang hebt tot de cmdlets voor uw abonnement op geselecteerde Azure uitgevoerd, is de volgende stap tot stand brengen resourcegroep waarop de server waar de database wordt gemaakt. U kunt de volgende opdracht voor het gebruik van de locatie waarop geldig die u kiest u bewerken. **Uitvoeren (Get-AzureRmLocation | WHERE-Object {_ $. Providers - eq "Microsoft.Sql"}). Locatie** om een lijst met geldige locaties.

Voer de volgende opdracht uit een resourcegroep maken:

    New-AzureRmResourceGroup -Name "resourcegroupsqlgsps" -Location "westus"


### <a name="create-a-server"></a>Maak een server

SQL-databases worden gemaakt in Azure SQL Database-servers. Voer de [nieuw-AzureRmSqlServer] (https://msdn.microsoft.com/library/azure/mt603715(v=azure.300\).aspx) een server maken. De naam voor de server moet uniek zijn voor alle Azure SQL Database-servers. Als de servernaam al gebruikt wordt, krijgt u een fout. Ook handig om te weten is dat deze opdracht enkele minuten duren kan om te voltooien. U kunt de opdracht voor het gebruik van een geldige locatie die u kiest bewerken, maar moet u de dezelfde locatie die u voor de resourcegroep in de vorige stap hebt gemaakt gebruikt.

    New-AzureRmSqlServer -ResourceGroupName "resourcegroupsqlgsps" -ServerName "server1" -Location "westus" -ServerVersion "12.0"

Wanneer u deze opdracht uitvoert, wordt u gevraagd om uw gebruikersnaam en wachtwoord. Niet, voert u uw Azure referenties. Voer in plaats daarvan de gebruikersnaam en wachtwoord om te maken als de serverbeheerder van de. Het script onderaan in dit artikel leest hoe de referenties voor de server instellen in code.

De server-gegevens worden weergegeven nadat de server is gemaakt.

### <a name="configure-a-server-firewall-rule-to-allow-access-to-the-server"></a>Een regel server firewall toestaan van toegang tot de server configureren

Voor toegang tot de server, moet u tot stand brengen van een firewallregel. Voer de [nieuw-AzureRmSqlServerFirewallRule] (https://msdn.microsoft.com/library/azure/mt603860(v=azure.300\).aspx) command, worden vervangen door de begin- en IP-adressen met geldige waarden voor uw computer.

    New-AzureRmSqlServerFirewallRule -ResourceGroupName "resourcegroupsqlgsps" -ServerName "server1" -FirewallRuleName "rule1" -StartIpAddress "192.168.0.0" -EndIpAddress "192.168.0.0"

De details van de regel firewall weergegeven nadat u de regel is gemaakt.

Als u wilt toestaan dat andere Azure services voor toegang tot de server, een firewallregel toevoegen en stel de StartIpAddress en de EndIpAddress op 0.0.0.0. Deze regel staat Azure verkeer van eventuele Azure abonnement om toegang te krijgen tot de server.

Zie [Azure SQL Database-Firewall](sql-database-firewall-configure.md)voor meer informatie.


## <a name="create-a-sql-database"></a>Een SQL-database maken

U hebt nu een resourcegroep, een server en een firewallregel geconfigureerd zodat u toegang hebt tot de server.

De volgende [nieuw-AzureRmSqlDatabase] (https://msdn.microsoft.com/library/azure/mt619339(v=azure.300\).aspx) opdracht maakt u een SQL-database (leeg) in de laag Standard service, met een prestatieniveau S1:


    New-AzureRmSqlDatabase -ResourceGroupName "resourcegroupsqlgsps" -ServerName "server1" -DatabaseName "database1" -Edition "Standard" -RequestedServiceObjectiveName "S1"


De informatie over de database worden weergegeven nadat de database is gemaakt.

## <a name="create-a-sql-database-powershell-script"></a>Een SQL-database PowerShell-script maken

Het volgende PowerShell-script Hiermee maakt u een SQL-database en alle bijbehorende afhankelijke resources. Alles vervangen `{variables}` met waarden die specifiek zijn voor uw abonnement en resources (de **{}** verwijderen wanneer u uw waarden instellen).

    # Sign in to Azure and set the subscription to work with
    $SubscriptionId = "{subscription-id}"

    Add-AzureRmAccount
    Set-AzureRmContext -SubscriptionId $SubscriptionId

    # CREATE A RESOURCE GROUP
    $resourceGroupName = "{group-name}"
    $rglocation = "{Azure-region}"
    
    New-AzureRmResourceGroup -Name $resourceGroupName -Location $rglocation
    
    # CREATE A SERVER
    $serverName = "{server-name}"
    $serverVersion = "12.0"
    $serverLocation = "{Azure-region}"
    
    $serverAdmin = "{server-admin}"
    $serverPassword = "{server-password}" 
    $securePassword = ConvertTo-SecureString –String $serverPassword –AsPlainText -Force
    $serverCreds = New-Object –TypeName System.Management.Automation.PSCredential –ArgumentList $serverAdmin, $securePassword
    
    $sqlDbServer = New-AzureRmSqlServer -ResourceGroupName $resourceGroupName -ServerName $serverName -Location $serverLocation -ServerVersion $serverVersion -SqlAdministratorCredentials $serverCreds
    
    # CREATE A SERVER FIREWALL RULE
    $ip = (Test-Connection -ComputerName $env:COMPUTERNAME -Count 1 -Verbose).IPV4Address.IPAddressToString
    $firewallRuleName = '{rule-name}'
    $firewallStartIp = $ip
    $firewallEndIp = $ip
    
    $fireWallRule = New-AzureRmSqlServerFirewallRule -ResourceGroupName $resourceGroupName -ServerName $serverName -FirewallRuleName $firewallRuleName -StartIpAddress $firewallStartIp -EndIpAddress $firewallEndIp
    
    
    # CREATE A SQL DATABASE
    $databaseName = "{database-name}"
    $databaseEdition = "{Standard}"
    $databaseSlo = "{S0}"
    
    $sqlDatabase = New-AzureRmSqlDatabase -ResourceGroupName $resourceGroupName -ServerName $serverName -DatabaseName $databaseName -Edition $databaseEdition -RequestedServiceObjectiveName $databaseSlo
    
   
    # REMOVE ALL RESOURCES THE SCRIPT JUST CREATED
    #Remove-AzureRmResourceGroup -Name $resourceGroupName






## <a name="next-steps"></a>Volgende stappen
Nadat u een SQL-database maken en uitvoeren van eenvoudige database configuratietaken, kunt u het volgende:

- [SQL-Database met PowerShell beheren](sql-database-manage-powershell.md)
- [Verbinding maken met SQL-Database met SQL Server Management Studio en uitvoeren van een steekproef T-SQL-query](sql-database-connect-query-ssms.md)


## <a name="additional-resources"></a>Aanvullende informatie

- [Azure SQL Database-Cmdlets] (https://msdn.microsoft.com/library/azure/mt574084(v=azure.300\).aspx)
- [Azure SQL-Database](https://azure.microsoft.com/documentation/services/sql-database/)
