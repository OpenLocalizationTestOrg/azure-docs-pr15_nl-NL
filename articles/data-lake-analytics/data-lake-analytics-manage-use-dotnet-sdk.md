<properties 
   pageTitle="Azure gegevens Lake analyses met behulp van Azure .NET SDK beheren | Azure" 
   description="Leer hoe u gegevens Lake Analytics-taken en gegevensbronnen en gebruikers beheren. " 
   services="data-lake-analytics" 
   documentationCenter="" 
   authors="mumian" 
   manager="jhubbard" 
   editor="cgronlun"/>
 
<tags
   ms.service="data-lake-analytics"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data" 
   ms.date="09/23/2016"
   ms.author="jgao"/>

# <a name="manage-azure-data-lake-analytics-using-azure-net-sdk"></a>Azure gegevens Lake analyses met behulp van Azure .NET SDK beheren

[AZURE.INCLUDE [manage-selector](../../includes/data-lake-analytics-selector-manage.md)]

Informatie over het beheren van Azure gegevens Lake Analytics-accounts, gegevensbronnen, gebruikers en taken met behulp van de Azure .NET SDK. Management als onderwerpen wilt bekijken met een ander hulpprogramma, klikt u op het tabblad van bovenstaande selecteren.

**Vereisten voor**

Voordat u deze zelfstudie begint, hebt u het volgende:

- **Een Azure-abonnement**. Zie [Azure krijgen gratis proefversie](https://azure.microsoft.com/pricing/free-trial/).


<!-- ################################ -->
<!-- ################################ -->


## <a name="connect-to-azure-data-lake-analytics"></a>Verbinding maken met gegevens van Azure Lake Analytics

U moet de volgende Nuget-pakketten:

    Install-Package Microsoft.Rest.ClientRuntime.Azure.Authentication -Pre
    Install-Package Microsoft.Azure.Common 
    Install-Package Microsoft.Azure.Management.ResourceManager -Pre
    Install-Package Microsoft.Azure.Management.DataLake.Analytics -Pre


Het volgende voorbeeld ziet u hoe u verbinding maken met Azure en vermelden van de bestaande gegevens Lake Analytics-accounts onder uw Azure-abonnement.

    using System;
    using System.Collections.Generic;
    using System.Threading;

    using Microsoft.Rest;
    using Microsoft.Rest.Azure.Authentication;

    using Microsoft.Azure.Management.ResourceManager;
    using Microsoft.Azure.Management.DataLake.Store;
    using Microsoft.Azure.Management.DataLake.Analytics;
    using Microsoft.Azure.Management.DataLake.Analytics.Models;

    namespace ConsoleAcplication1
    {
        class Program
        {

            private const string SUBSCRIPTIONID = "<Enter Your Azure Subscription ID>";
            private const string CLIENTID = "1950a258-227b-4e31-a9cf-717495945fc2";
            private const string DOMAINNAME = "common"; // Replace this string with the user's Azure Active Directory tenant ID or domain name, if needed.

            private static DataLakeAnalyticsAccountManagementClient _adlaClient;

            private static void Main(string[] args)
            {

                var creds = AuthenticateAzure(DOMAINNAME, CLIENTID);

                _adlaClient = new DataLakeAnalyticsAccountManagementClient(creds);
                _adlaClient.SubscriptionId = SUBSCRIPTIONID;

                var adlaAccounts = ListADLAAccounts();

                Console.WriteLine("You have %i Data Lake Analytics account(s).", adlaAccounts.Count);
                for (int i = 0; i < adlaAccounts.Count; i ++)
                {
                    Console.WriteLine(adlaAccounts[i].Name);
                }

                System.Console.WriteLine("Press ENTER to continue");
                System.Console.ReadLine();
            }

            public static ServiceClientCredentials AuthenticateAzure(
            string domainName,
            string nativeClientAppCLIENTID)
            {
                // User login via interactive popup
                SynchronizationContext.SetSynchronizationContext(new SynchronizationContext());
                // Use the client ID of an existing AAD "Native Client" application.
                var activeDirectoryClientSettings = ActiveDirectoryClientSettings.UsePromptOnly(nativeClientAppCLIENTID, new Uri("urn:ietf:wg:oauth:2.0:oob"));
                return UserTokenProvider.LoginWithPromptAsync(domainName, activeDirectoryClientSettings).Result;
            }

            public static List<DataLakeAnalyticsAccount> ListADLAAccounts()
            {
                var response = _adlaClient.Account.List();
                var accounts = new List<DataLakeAnalyticsAccount>(response);

                while (response.NextPageLink != null)
                {
                    response = _adlaClient.Account.ListNext(response.NextPageLink);
                    accounts.AddRange(response);
                }

                return accounts;
            }
        }
    }


## <a name="manage-accounts"></a>Accounts beheren

Voordat u alle gegevens Lake Analytics-taken, moet u een gegevens Lake Analytics-account hebt. In tegenstelling tot Azure HDInsight betaalt niet u voor een Analytics-account wanneer dit een taak niet wordt uitgevoerd.  U betaalt alleen voor de tijd waarop een taak wordt uitgevoerd.  Zie [Overzicht van Azure Data Lake Analytics](data-lake-analytics-overview.md)voor meer informatie.  

###<a name="create-accounts"></a>Accounts maken

U moet een resourcebeheer Azure-groep en een account voor gegevensopslag Lake voordat u in het onderstaande voorbeeld kunt uitvoeren.

De volgende code ziet u hoe u een resourcegroep maken:

    public static async Task<ResourceGroup> CreateResourceGroupAsync(
        ServiceClientCredentials credential,
        string groupName,
        string subscriptionId,
        string location)
    {

        Console.WriteLine("Creating the resource group...");
        var resourceManagementClient = new ResourceManagementClient(credential)
        { SubscriptionId = subscriptionId };
        var resourceGroup = new ResourceGroup { Location = location };
        return await resourceManagementClient.ResourceGroups.CreateOrUpdateAsync(groupName, resourceGroup);
    }

De volgende code ziet u hoe u een account voor gegevensopslag Lake maken:

    var adlsParameters = new DataLakeStoreAccount(location: _location);
    _adlsClient.Account.Create(_resourceGroupName, _adlsAccountName, adlsParameters);

De volgende code ziet u hoe u een gegevens Lake Analytics-account kunt maken:

    var defaultAdlsAccount = new List<DataLakeStoreAccountInfo> { new DataLakeStoreAccountInfo(adlsAccountName, new DataLakeStoreAccountInfoProperties()) };
    var adlaProperties = new DataLakeAnalyticsAccountProperties(defaultDataLakeStoreAccount: adlsAccountName, dataLakeStoreAccounts: defaultAdlsAccount);
    var adlaParameters = new DataLakeAnalyticsAccount(properties: adlaProperties, location: location);
    var adlaAccount = _adlaClient.Account.Create(resourceGroupName, adlaAccountName, adlaParameters);

###<a name="list-accounts"></a>Lijst met accounts

Zie [verbinding maken met gegevens van Azure Lake Analytics](#connect_to_azure_data_lake_analytics).

###<a name="find-an-account"></a>Een account zoeken

Als u een object van een lijst met gegevens Lake Analytics-accounts, kunt u de volgende handelingen uit om een rekening te vinden:

    Predicate<DataLakeAnalyticsAccount> accountFinder = (DataLakeAnalyticsAccount a) => { return a.Name == adlaAccountName; };
    var myAdlaAccount = adlaAccounts.Find(accountFinder);

###<a name="delete-data-lake-analytics-accounts"></a>Gegevens Lake Analytics-accounts verwijderen

Het volgende codefragment Hiermee verwijdert u een gegevens Lake Analytics-account:

    _adlaClient.Account.Delete(resourceGroupName, adlaAccountName);

<!-- ################################ -->
<!-- ################################ -->
## <a name="manage-account-data-sources"></a>Account gegevensbronnen beheren

Gegevens Lake Analytics worden momenteel ondersteund in de volgende gegevensbronnen:

- [Azure-Lake gegevensopslag](../data-lake-store/data-lake-store-overview.md)
- [Azure-opslag](../storage/storage-introduction.md)

Wanneer u een Analytics-account hebt gemaakt, wijst u een account Azure Lake gegevensopslag het standaardaccount voor de opslag. Het standaardaccount voor gegevensopslag Lake wordt gebruikt voor de opslag van taak metagegevens en taak controlelogboeken bijhouden. Nadat u een Analytics-account hebt gemaakt, kunt u extra Lake gegevensopslag accounts en/of de opslag van Azure-account toevoegen. 

### <a name="find-the-default-data-lake-store-account"></a>Het standaardaccount voor gegevensopslag Lake zoeken

Zie zoeken een account in dit artikel om de gegevens Lake Analytics-rekening te vinden. Gebruik het volgende:

    string adlaDefaultDataLakeStoreAccountName = myAccount.Properties.DefaultDataLakeStoreAccount;


## <a name="use-azure-resource-manager-groups"></a>Azure resourcemanager groepen gebruiken

Toepassingen zijn meestal bestaat uit veel onderdelen, bijvoorbeeld een web-app, database, database-server, opslag en 3e partijen services. Azure resourcemanager kunt u werken met de resources in uw toepassing als een groep, een resourcegroep Azure genoemd. U kunt implementeren, bijwerken, controleren of alle bronnen voor uw toepassing in één, gecoördineerde bewerking verwijderen. Een sjabloon te gebruiken voor implementatie en die sjabloon voor verschillende omgevingen zoals testen, ontwikkel- en kunt werken. U kunt de facturering voor uw organisatie verduidelijken door de samengevouwen kosten voor de hele groep weer te geven. Zie [Azure resourcemanager overzicht](../azure-resource-manager/resource-group-overview.md)voor meer informatie. 

Een gegevens Lake Analytics-service kan de volgende onderdelen bevatten:

- Azure gegevens Lake Analytics-account
- Vereiste standaardaccount voor Azure Lake gegevensopslag
- Aanvullende Azure gegevens Lake opslag-accounts
- Extra opslagruimte van Azure-accounts

U kunt deze onderdelen onder één resourcebeheer groep zodat u ze gemakkelijker kunt beheren.

![Azure gegevens Lake Analytics-account en opslag](./media/data-lake-analytics-manage-use-portal/data-lake-analytics-arm-structure.png)

Een gegevens Lake Analytics-account en de afhankelijke opslag-accounts moeten worden geplaatst in het dezelfde Azure Datacenter.
De groep resourcebeheer kan echter zich bevinden in een ander datacenter.  

##<a name="see-also"></a>Zie ook 

- [Overzicht van Microsoft Azure-gegevens Lake Analytics](data-lake-analytics-overview.md)
- [Aan de slag met gegevens Lake analyses met behulp van Azure portal](data-lake-analytics-get-started-portal.md)
- [Azure gegevens Lake analyses met behulp van Azure portal beheren](data-lake-analytics-manage-use-portal.md)
- [Controleren en problemen met Azure gegevens Lake Analytics-taken met behulp van Azure portal](data-lake-analytics-monitor-and-troubleshoot-jobs-tutorial.md)

