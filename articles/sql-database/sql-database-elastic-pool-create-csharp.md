<properties
    pageTitle="Een toepassingen elastische database maken met C# | Microsoft Azure"
    description="Gebruik C#-database-technieken voor het maken van een resourcegroep die scalable elastische database in Azure SQL-Database, zodat u resources met andere veel databases delen kunt."
    services="sql-database"
    documentationCenter=""
    authors="stevestein"
    manager="jhubbard"
    editor=""/>

<tags
    ms.service="sql-database"
    ms.devlang="NA"
    ms.topic="get-started-article"
    ms.tgt_pltfrm="csharp"
    ms.workload="data-management"
    ms.date="10/04/2016"
    ms.author="sstein"/>

# <a name="create-an-elastic-database-pool-with-cx23"></a>Een toepassingen elastische database maken met C & #x 23;

> [AZURE.SELECTOR]
- [Azure-portal](sql-database-elastic-pool-create-portal.md)
- [PowerShell](sql-database-elastic-pool-create-powershell.md)
- [C#](sql-database-elastic-pool-create-csharp.md)


In dit artikel wordt beschreven hoe u C# om te maken van een Azure SQL database elastische toepassingen met de [Azure SQL Database-bibliotheek voor .NET](https://www.nuget.org/packages/Microsoft.Azure.Management.Sql). Zie maken voor een zelfstandig SQL-database [C# gebruiken om een SQL-database met de SQL-Database-bibliotheek voor .NET te maken](sql-database-get-started-csharp.md).

De bibliotheek Azure SQL-Database voor .NET biedt een [Resourcemanager Azure](../azure-resource-manager/resource-group-overview.md)-API die de [Resourcemanager gebaseerde SQL Database REST API terugloopt](https://msdn.microsoft.com/library/azure/mt163571.aspx)gebaseerd.

>[AZURE.NOTE] Veel nieuwe functies van de SQL-Database worden alleen ondersteund wanneer u het [Azure resourcemanager implementatiemodel](../azure-resource-manager/resource-group-overview.md), gebruikt zodat u altijd de meest recente moet gebruiken **Azure SQL Database Management bibliotheek voor .NET ([documenten](https://msdn.microsoft.com/library/azure/mt349017.aspx) | [NuGet pakket](https://www.nuget.org/packages/Microsoft.Azure.Management.Sql))**. De oudere [klassieke implementatiemodel op basis van bibliotheken](https://www.nuget.org/packages/Microsoft.WindowsAzure.Management.Sql) worden ondersteund voor achterwaartse compatibiliteit alleen, zodat we raden u aan om de nieuwere resourcemanager op basis van bibliotheken.

U voltooit de stappen in dit artikel, moet u het volgende:

- Een Azure-abonnement. Als u gewoon nodig een Azure-abonnement hebt op **Gratis ACCOUNT** boven aan deze pagina en keert u terug naar het voltooien van dit artikel.
- Visual Studio. Zie voor een gratis exemplaar van Visual Studio, de pagina [Visual Studio-Downloads](https://www.visualstudio.com/downloads/download-visual-studio-vs) .


## <a name="create-a-console-app-and-install-the-required-libraries"></a>Een console-app maken en installeer de vereiste bibliotheken

1. Visual Studio starten.
2. Klik op **bestand** > **nieuwe** > **Project**.
3. Maak een C#- **Console-toepassing** en noem deze: *SqlElasticPoolConsoleApp*


Als u wilt een SQL-database maken met C#, laadt u de vereiste management-bibliotheken (via de [beheerconsole pakket](http://docs.nuget.org/Consume/Package-Manager-Console)):

1. Klik op **Extra** > **NuGet Package Manager** > **Package Manager-Console**.
2. Type `Install-Package Microsoft.Azure.Management.Sql –Pre` voor het installeren van de [Bibliotheek van Microsoft Azure SQL-Management](https://www.nuget.org/packages/Microsoft.Azure.Management.Sql).
3. Type `Install-Package Microsoft.Azure.Management.ResourceManager –Pre` voor het installeren van de [Microsoft Azure resourcemanager bibliotheek](https://www.nuget.org/packages/Microsoft.Azure.Management.ResourceManager).
4. Type `Install-Package Microsoft.Azure.Common.Authentication –Pre` voor het installeren van de [Microsoft Azure algemene verificatie-bibliotheek](https://www.nuget.org/packages/Microsoft.Azure.Common.Authentication). 



> [AZURE.NOTE] De voorbeelden in dit artikel gebruik van een synchroon formulier van elke API-aanvraag en blokkeren totdat de voltooiing van de REST van de onderliggende service bellen. Er zijn asynchrone methoden beschikbaar.


## <a name="create-a-sql-elastic-database-pool---c-example"></a>Maken van een groep met SQL-database elastische - C#-voorbeeld

In het onderstaande voorbeeld wordt gemaakt van een resourcegroep, server, firewallregel, elastische van toepassingen en maakt vervolgens een SQL-database in de groep. Zie, [een service hoofdsom voor toegang tot resources maken](#create-a-service-principal-to-access-resources) om de `_subscriptionId, _tenantId, _applicationId, and _applicationSecret` variabelen.

De inhoud van **Program.cs** vervangen door de volgende handelingen uit en bijwerken de `{variables}` met de app-waarden (Neem niet de `{}`).


```
using Microsoft.Azure;
using Microsoft.Azure.Management.ResourceManager;
using Microsoft.Azure.Management.ResourceManager.Models;
using Microsoft.Azure.Management.Sql;
using Microsoft.Azure.Management.Sql.Models;
using Microsoft.IdentityModel.Clients.ActiveDirectory;
using System;

namespace SqlElasticPoolConsoleApp
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

        static string _poolName = "{pool-name}";
        static string _poolEdition = "{Standard}";
        static int _poolDtus = {100};
        static int _databaseMinDtus = {10};
        static int _databaseMaxDtus = {100};

        static string _databaseName = "{elasticdbfromcsarticle}";



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

            Console.WriteLine("Elastic pool...");
            ElasticPoolCreateOrUpdateResponse epr = CreateOrUpdateElasticDatabasePool(_sqlMgmtClient, _resourceGroupName, _serverName, _poolName, _poolEdition, _poolDtus, _databaseMinDtus, _databaseMaxDtus);
            Console.WriteLine("Elastic pool: " + epr.ElasticPool.Id);

            Console.WriteLine("Database...");
            DatabaseCreateOrUpdateResponse dbr = CreateOrUpdateDatabase(_sqlMgmtClient, _resourceGroupName, _serverName, _databaseName, _poolName);
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



        static ElasticPoolCreateOrUpdateResponse CreateOrUpdateElasticDatabasePool(SqlManagementClient sqlMgmtClient, string resourceGroupName, string serverName, string poolName, string poolEdition, int poolDtus, int databaseMinDtus, int databaseMaxDtus)
        {
            // Retrieve the server that will host this elastic pool
            Server currentServer = sqlMgmtClient.Servers.Get(resourceGroupName, serverName).Server;

            // Create elastic pool: configure create or update parameters and properties explicitly
            ElasticPoolCreateOrUpdateParameters newPoolParameters = new ElasticPoolCreateOrUpdateParameters()
            {
                Location = currentServer.Location,
                Properties = new ElasticPoolCreateOrUpdateProperties()
                {
                    Edition = poolEdition,
                    Dtu = poolDtus,
                    DatabaseDtuMin = databaseMinDtus,
                    DatabaseDtuMax = databaseMaxDtus
                }
            };

            // Create the pool
            var newPoolResponse = sqlMgmtClient.ElasticPools.CreateOrUpdate(resourceGroupName, serverName, poolName, newPoolParameters);
            return newPoolResponse;
        }




        static DatabaseCreateOrUpdateResponse CreateOrUpdateDatabase(SqlManagementClient sqlMgmtClient, string resourceGroupName, string serverName, string databaseName, string poolName)
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
                    ElasticPoolName = poolName
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
```





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

- [Uw groep beheren](sql-database-elastic-pool-manage-csharp.md)
- [Elastische taken maken](sql-database-elastic-jobs-overview.md): elastische taken kunnen u T-SQL-scripts uitvoeren op een willekeurig aantal databases in een groep.
- [Schalen met Azure SQL-Database](sql-database-elastic-scale-introduction.md): elastische Databasehulpmiddelen gebruiken voor het schalen af.

## <a name="additional-resources"></a>Aanvullende informatie

- [SQL-Database](https://azure.microsoft.com/documentation/services/sql-database/)
- [Azure Resource Management-API 's](https://msdn.microsoft.com/library/azure/dn948464.aspx)
