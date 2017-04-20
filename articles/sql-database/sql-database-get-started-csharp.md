<properties
    pageTitle="Probeer SQL-Database: Gebruik C# maken van een SQL-database | Microsoft Azure"
    description="SQL-Database voor het ontwikkelen van SQL- en C#-apps probeert en een Azure SQL-Database maken met C# met behulp van de SQL-Database-bibliotheek voor .NET."
    keywords="Probeer sql, sql c#"   
    services="sql-database"
    documentationCenter=""
    authors="stevestein"
    manager="jhubbard"
    editor="cgronlun"/>

<tags
   ms.service="sql-database"
   ms.devlang="NA"
   ms.topic="hero-article"
   ms.tgt_pltfrm="csharp"
   ms.workload="data-management"
   ms.date="10/04/2016"
   ms.author="sstein"/>

# <a name="try-sql-database-use-c-to-create-a-sql-database-with-the-sql-database-library-for-net"></a>Probeer SQL-Database: Gebruik C# naar een SQL-database maken met de SQL-Database-bibliotheek voor .NET


> [AZURE.SELECTOR]
- [Azure-portal](sql-database-get-started.md)
- [C#](sql-database-get-started-csharp.md)
- [PowerShell](sql-database-get-started-powershell.md)

Leer hoe u met C# een Azure SQL-database maken met de [Bibliotheek van Microsoft Azure SQL Management voor .NET](https://www.nuget.org/packages/Microsoft.Azure.Management.Sql). In dit artikel wordt beschreven hoe een één database maken met SQL- en C#. Zie [een toepassingen elastische database maken](sql-database-elastic-pool-create-csharp.md)elastische database groepen maken.

De bibliotheek Azure SQL Database Management voor .NET biedt een [Resourcemanager Azure](../azure-resource-manager/resource-group-overview.md)-API die de [Resourcemanager gebaseerde SQL Database REST API terugloopt](https://msdn.microsoft.com/library/azure/mt163571.aspx)gebaseerd.

>[AZURE.NOTE] Veel nieuwe functies van de SQL-Database worden alleen ondersteund wanneer u het [Azure resourcemanager implementatiemodel](../azure-resource-manager/resource-group-overview.md), gebruikt zodat u altijd de meest recente moet gebruiken **Azure SQL Database Management bibliotheek voor .NET ([documenten](https://msdn.microsoft.com/library/azure/mt349017.aspx) | [NuGet pakket](https://www.nuget.org/packages/Microsoft.Azure.Management.Sql))**. De oudere [klassieke implementatiemodel op basis van bibliotheken](https://www.nuget.org/packages/Microsoft.WindowsAzure.Management.Sql) worden ondersteund voor achterwaartse compatibiliteit alleen, zodat we raden u aan om de nieuwere resourcemanager op basis van bibliotheken.

U voltooit de stappen in dit artikel, moet u het volgende:

- Een Azure-abonnement. Als u gewoon nodig een Azure-abonnement hebt op **Gratis ACCOUNT** boven aan deze pagina en keert u terug naar het voltooien van dit artikel.
- Visual Studio. Zie voor een gratis exemplaar van Visual Studio, de pagina [Visual Studio-Downloads](https://www.visualstudio.com/downloads/download-visual-studio-vs) .

>[AZURE.NOTE] In dit artikel wordt een nieuwe, lege SQL-database gemaakt. Wijzig de methode *CreateOrUpdateDatabase(...)* in het onderstaande voorbeeld voor het kopiëren van databases, de schaal van databases aanpassen, een database maken in een groep, enzovoort. Zie klassen [DatabaseCreateMode](https://msdn.microsoft.com/library/microsoft.azure.management.sql.models.databasecreatemode.aspx) en [DatabaseProperties](https://msdn.microsoft.com/library/microsoft.azure.management.sql.models.databaseproperties.aspx) voor meer informatie.



## <a name="create-a-console-app-and-install-the-required-libraries"></a>Een console-app maken en installeer de vereiste bibliotheken

1. Visual Studio starten.
2. Klik op **bestand** > **nieuwe** > **Project**.
3. Maak een C#- **Console-toepassing** en noem deze: *SqlDbConsoleApp*


Als u wilt een SQL-database maken met C#, laadt u de vereiste management-bibliotheken (via de [beheerconsole pakket](http://docs.nuget.org/Consume/Package-Manager-Console)):

1. Klik op **Extra** > **NuGet Package Manager** > **Package Manager-Console**.
2. Type `Install-Package Microsoft.Azure.Management.Sql –Pre` voor het installeren van de meest recente [Microsoft Azure SQL Management bibliotheek](https://www.nuget.org/packages/Microsoft.Azure.Management.Sql).
3. Type `Install-Package Microsoft.Azure.Management.ResourceManager –Pre` voor het installeren van de [Microsoft Azure resourcemanager bibliotheek](https://www.nuget.org/packages/Microsoft.Azure.Management.ResourceManager).
4. Type `Install-Package Microsoft.Azure.Common.Authentication –Pre` voor het installeren van de [Microsoft Azure algemene verificatie-bibliotheek](https://www.nuget.org/packages/Microsoft.Azure.Common.Authentication). 



> [AZURE.NOTE] De voorbeelden in dit artikel gebruik van een synchroon formulier van elke API-aanvraag en blokkeren totdat de voltooiing van de REST van de onderliggende service bellen. Er zijn asynchrone methoden beschikbaar.


## <a name="create-a-sql-database-server-firewall-rule-and-sql-database---c-example"></a>Een SQL-databaseserver, firewallregel en SQL-database - C#-voorbeeld maken

In het onderstaande voorbeeld wordt een resourcegroep, server, firewallregel en een SQL-database gemaakt. Zie, [een service hoofdsom voor toegang tot resources maken](#create-a-service-principal-to-access-resources) om de `_subscriptionId, _tenantId, _applicationId, and _applicationSecret` variabelen.

De inhoud van **Program.cs** vervangen door de volgende handelingen uit en bijwerken de `{variables}` met de app-waarden (Neem niet de `{}`).


    using Microsoft.Azure;
    using Microsoft.Azure.Management.ResourceManager;
    using Microsoft.Azure.Management.ResourceManager.Models;
    using Microsoft.Azure.Management.Sql;
    using Microsoft.Azure.Management.Sql.Models;
    using Microsoft.IdentityModel.Clients.ActiveDirectory;
    using System;
    
    namespace SqlDbConsoleApp
    {
    class Program
       {
        // For details about these four (4) values, see
        // https://azure.microsoft.com/documentation/articles/resource-group-authenticate-service-principal/
        static string _subscriptionId = "{xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx}";
        static string _tenantId = "{xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx}";
        static string _applicationId = "{xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx}";
        static string _applicationSecret = "{your-password}";

        // Create management clients for the Azure resources your app needs to work with.
        // This app works with Resource Groups, and Azure SQL Database.
        static ResourceManagementClient _resourceMgmtClient;
        static SqlManagementClient _sqlMgmtClient;

        // Authentication token
        static AuthenticationResult _token;

        // Azure resource variables
        static string _resourceGroupName = "{resource-group-name}";
        static string _resourceGrouplocation = "{Azure-region}";

        static string _serverlocation = _resourceGrouplocation;
        static string _serverName = "{server-name}";
        static string _serverAdmin = "{server-admin-login}";
        static string _serverAdminPassword = "{server-admin-password}";

        static string _firewallRuleName = "{firewall-rule-name}";
        static string _startIpAddress = "{0.0.0.0}";
        static string _endIpAddress = "{255.255.255.255}";

        static string _databaseName = "{dbfromcsarticle}";
        static string _databaseEdition = DatabaseEditions.Basic;
        static string _databasePerfLevel = ""; // "S0", "S1", and so on here for other tiers


        static void Main(string[] args)
        {
            // Authenticate:
            _token = GetToken(_tenantId, _applicationId, _applicationSecret);
            Console.WriteLine("Token acquired. Expires on:" + _token.ExpiresOn);

            // Instantiate management clients:
            _resourceMgmtClient = new ResourceManagementClient(new Microsoft.Rest.TokenCredentials(_token.AccessToken));
            _sqlMgmtClient = new SqlManagementClient(new TokenCloudCredentials(_subscriptionId, _token.AccessToken));


            Console.WriteLine("Resource group...");
            ResourceGroup rg = CreateOrUpdateResourceGroup(_resourceMgmtClient, _subscriptionId, _resourceGroupName, _resourceGrouplocation);
            Console.WriteLine("Resource group: " + rg.Id);


            Console.WriteLine("Server...");
            ServerGetResponse sgr = CreateOrUpdateServer(_sqlMgmtClient, _resourceGroupName, _serverlocation, _serverName, _serverAdmin, _serverAdminPassword);
            Console.WriteLine("Server: " + sgr.Server.Id);

            Console.WriteLine("Server firewall...");
            FirewallRuleGetResponse fwr = CreateOrUpdateFirewallRule(_sqlMgmtClient, _resourceGroupName, _serverName, _firewallRuleName, _startIpAddress, _endIpAddress);
            Console.WriteLine("Server firewall: " + fwr.FirewallRule.Id);

            Console.WriteLine("Database...");
            DatabaseCreateOrUpdateResponse dbr = CreateOrUpdateDatabase(_sqlMgmtClient, _resourceGroupName, _serverName, _databaseName, _databaseEdition, _databasePerfLevel);
            Console.WriteLine("Database: " + dbr.Database.Id);


            Console.WriteLine("Press any key to continue...");
            Console.ReadKey();
        }

        static ResourceGroup CreateOrUpdateResourceGroup(ResourceManagementClient resourceMgmtClient, string subscriptionId, string resourceGroupName, string resourceGroupLocation)
        {
            ResourceGroup resourceGroupParameters = new ResourceGroup()
            {
                Location = resourceGroupLocation,
            };
            resourceMgmtClient.SubscriptionId = subscriptionId;
            ResourceGroup resourceGroupResult = resourceMgmtClient.ResourceGroups.CreateOrUpdate(resourceGroupName, resourceGroupParameters);
            return resourceGroupResult;
        }

        static ServerGetResponse CreateOrUpdateServer(SqlManagementClient sqlMgmtClient, string resourceGroupName, string serverLocation, string serverName, string serverAdmin, string serverAdminPassword)
        {
            ServerCreateOrUpdateParameters serverParameters = new ServerCreateOrUpdateParameters()
            {
                Location = serverLocation,
                Properties = new ServerCreateOrUpdateProperties()
                {
                    AdministratorLogin = serverAdmin,
                    AdministratorLoginPassword = serverAdminPassword,
                    Version = "12.0"
                }
            };
            ServerGetResponse serverResult = sqlMgmtClient.Servers.CreateOrUpdate(resourceGroupName, serverName, serverParameters);
            return serverResult;
        }


        static FirewallRuleGetResponse CreateOrUpdateFirewallRule(SqlManagementClient sqlMgmtClient, string resourceGroupName, string serverName, string firewallRuleName, string startIpAddress, string endIpAddress)
        {
            FirewallRuleCreateOrUpdateParameters firewallParameters = new FirewallRuleCreateOrUpdateParameters()
            {
                Properties = new FirewallRuleCreateOrUpdateProperties()
                {
                    StartIpAddress = startIpAddress,
                    EndIpAddress = endIpAddress
                }
            };
            FirewallRuleGetResponse firewallResult = sqlMgmtClient.FirewallRules.CreateOrUpdate(resourceGroupName, serverName, firewallRuleName, firewallParameters);
            return firewallResult;
        }



        static DatabaseCreateOrUpdateResponse CreateOrUpdateDatabase(SqlManagementClient sqlMgmtClient, string resourceGroupName, string serverName, string databaseName, string databaseEdition, string databasePerfLevel)
        {
            // Retrieve the server that will host this database
            Server currentServer = sqlMgmtClient.Servers.Get(resourceGroupName, serverName).Server;

            // Create a database: configure create or update parameters and properties explicitly
            DatabaseCreateOrUpdateParameters newDatabaseParameters = new DatabaseCreateOrUpdateParameters()
            {
                Location = currentServer.Location,
                Properties = new DatabaseCreateOrUpdateProperties()
                {
                    CreateMode = DatabaseCreateMode.Default,
                    Edition = databaseEdition,
                    RequestedServiceObjectiveName = databasePerfLevel
                }
            };
            DatabaseCreateOrUpdateResponse dbResponse = sqlMgmtClient.Databases.CreateOrUpdate(resourceGroupName, serverName, databaseName, newDatabaseParameters);
            return dbResponse;
        }



        private static AuthenticationResult GetToken(string tenantId, string applicationId, string applicationSecret)
        {
            AuthenticationContext authContext = new AuthenticationContext("https://login.windows.net/" + tenantId);
            _token = authContext.AcquireToken("https://management.core.windows.net/", new ClientCredential(applicationId, applicationSecret));
            return _token;
        }
      }
    }





## <a name="create-a-service-principal-to-access-resources"></a>Een service hoofdsom voor toegang tot bronnen maken

Het volgende PowerShell-script Hiermee maakt u de toepassing AD (Active Directory) en de service-hoofdsom die we nodig hebt om te verifiëren van onze C#-app. De waarden die we nodig voor de voorgaande C#-steekproef uitgevoerd. Zie [Azure PowerShell gebruiken om te maken van een service principal toegang krijgen tot bronnen](../resource-group-authenticate-service-principal.md)voor gedetailleerde informatie.

   
    # Sign in to Azure.
    Add-AzureRmAccount
    
    # If you have multiple subscriptions, uncomment and set to the subscription you want to work with.
    #$subscriptionId = "{xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx}"
    #Set-AzureRmContext -SubscriptionId $subscriptionId
    
    # Provide these values for your new AAD app.
    # $appName is the display name for your app, must be unique in your directory.
    # $uri does not need to be a real uri.
    # $secret is a password you create.
    
    $appName = "{app-name}"
    $uri = "http://{app-name}"
    $secret = "{app-password}"
    
    # Create a AAD app
    $azureAdApplication = New-AzureRmADApplication -DisplayName $appName -HomePage $Uri -IdentifierUris $Uri -Password $secret
    
    # Create a Service Principal for the app
    $svcprincipal = New-AzureRmADServicePrincipal -ApplicationId $azureAdApplication.ApplicationId
    
    # To avoid a PrincipalNotFound error, I pause here for 15 seconds.
    Start-Sleep -s 15
    
    # If you still get a PrincipalNotFound error, then rerun the following until successful. 
    $roleassignment = New-AzureRmRoleAssignment -RoleDefinitionName Contributor -ServicePrincipalName $azureAdApplication.ApplicationId.Guid
    
    
    # Output the values we need for our C# application to successfully authenticate
    
    Write-Output "Copy these values into the C# sample app"
    
    Write-Output "_subscriptionId:" (Get-AzureRmContext).Subscription.SubscriptionId
    Write-Output "_tenantId:" (Get-AzureRmContext).Tenant.TenantId
    Write-Output "_applicationId:" $azureAdApplication.ApplicationId.Guid
    Write-Output "_applicationSecret:" $secret



## <a name="next-steps"></a>Volgende stappen
Nu dat u hebt geprobeerd SQL-Database en instellen van een database maken met C#, bent u gereed voor de volgende artikelen:

- [Verbinding maken met SQL-Database met SQL Server Management Studio en uitvoeren van een steekproef T-SQL-query](sql-database-connect-query-ssms.md)

## <a name="additional-resources"></a>Aanvullende informatie

- [SQL-Database](https://azure.microsoft.com/documentation/services/sql-database/)
- [Database Class](https://msdn.microsoft.com/library/azure/microsoft.azure.management.sql.models.database.aspx)




<!--Image references-->
[1]: ./media/sql-database-get-started-csharp/aad.png
[2]: ./media/sql-database-get-started-csharp/permissions.png
[3]: ./media/sql-database-get-started-csharp/getdomain.png
[4]: ./media/sql-database-get-started-csharp/aad2.png
[5]: ./media/sql-database-get-started-csharp/aad-applications.png
[6]: ./media/sql-database-get-started-csharp/add.png
[7]: ./media/sql-database-get-started-csharp/add-application.png
[8]: ./media/sql-database-get-started-csharp/add-application2.png
[9]: ./media/sql-database-get-started-csharp/clientid.png
