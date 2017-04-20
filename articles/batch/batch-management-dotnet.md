<properties
    pageTitle="Resourcebeheer met Batch Management .NET-account | Microsoft Azure"
    description="Maken, verwijderen en wijzigen van Azure Batch account resources met de Batch Management .NET-bibliotheek."
    services="batch"
    documentationCenter=".net"
    authors="mmacy"
    manager="timlt"
    editor=""
    tags="azure-resource-manager"/>

<tags
    ms.service="batch"
    ms.devlang="multiple"
    ms.topic="article"
    ms.tgt_pltfrm="vm-windows"
    ms.workload="big-compute"
    ms.date="10/19/2016"
    ms.author="marsma"/>

# <a name="manage-azure-batch-accounts-and-quotas-with-batch-management-net"></a>Azure Batch accounts en quota met Batch Management .NET beheren

> [AZURE.SELECTOR]
- [Azure-portal](batch-account-create-portal.md)
- [Batch Management .NET](batch-management-dotnet.md)

U kunt verminderen onderhoud belasting in uw toepassingen Azure Batch met behulp van de [Batch Management.NET] [ api_mgmt_net] bibliotheek automatiseren Batch-account maken, verwijderen, key management en quotum discovery.

- **Maken en verwijderen van de Batchaccounts** binnen elke regio. Als, als een onafhankelijke softwareleverancier (ISV) zoals u een service voor uw klanten waarin elk een aparte Batch-account is toegewezen voor billing doeleinden, kunt u mogelijkheden voor het maken en verwijderen van het account toevoegen aan uw klant-portal.
- **Ophalen en opnieuw genereren account sleutels** via een programma voor een van de Batch-accounts. Hiermee kunt u akkoord gaat met beveiligingsbeleid voor apparaten die periodiek rollover of verstrijken van account toetsen afdwingen. Als er meerdere Batch-accounts in verschillende Azure gebieden, automatisering van dit proces rollover Hiermee verhoogt u de efficiëntie van uw oplossing.
- **Quota voor account controleren** en breng de verschillende proefversie-en-fout bij het vaststellen welke Batch-accounts hebt welke grenzen. Groepen maken of berekeningscluster knooppunten toe te voegen, kunt u proactief waar aanpassen door uw account quota's controleren voordat u begint met taken, of wanneer deze berekenen resources worden gemaakt. U kunt bepalen welke accounts vereisen quotum verhogen voordat het toewijzen van extra bronnen in die accounts.
- **Functies van andere services Azure combineren** voor een volledige management ervaring--met behulp van de Batch Management .NET, [Azure Active Directory][aad_about], en de [Resourcemanager Azure] [ resman_overview] samen in dezelfde toepassing. Met behulp van deze functies en hun API's, krijgt u een frictionless verificatie-ervaring, de mogelijkheid om te maken en verwijderen van resourcegroepen en de mogelijkheden die zijn voor een oplossing voor end-to-end hierboven wordt beschreven.

> [AZURE.NOTE] Terwijl u in dit artikel bevat informatie over het programma beheer van uw accounts Batch, sleutels en quota, kunt u ook veel van deze activiteiten uitvoeren met behulp van de [Azure portal][azure_portal]. Zie [een Batch Azure-account maken met behulp van de Azure portal](batch-account-create-portal.md) en [quota en -limieten voor de Batch Azure-service](batch-quota-limit.md)voor meer informatie.

## <a name="create-and-delete-batch-accounts"></a>Batch-accounts maken en verwijderen

Zoals vermeld, is een van de primaire functies van de Batch Management API Batchaccounts in een Azure gebied maken en verwijderen. Gebruik hiervoor [BatchManagementClient.Account.CreateAsync] [ net_create] en [DeleteAsync][net_delete], of hun synchroon tegenhangers van Excel.

Het volgende codefragment een account maakt, wordt de nieuwe account opgehaald van de Batch-service en wordt deze daarna verwijderd. In dit fragment en de andere in dit artikel `batchManagementClient` een volledig geïnitialiseerd exemplaar van [BatchManagementClient][net_mgmt_client].

```csharp
// Create a new Batch account
await batchManagementClient.Account.CreateAsync("MyResourceGroup",
    "mynewaccount",
    new BatchAccountCreateParameters() { Location = "West US" });

// Get the new account from the Batch service
AccountResource account = await batchManagementClient.Account.GetAsync(
    "MyResourceGroup",
    "mynewaccount");

// Delete the account
await batchManagementClient.Account.DeleteAsync("MyResourceGroup", account.Name);
```

> [AZURE.NOTE] Toepassingen die gebruikmaken van de Batch Management .NET-bibliotheek en de klasse BatchManagementClient vereisen **service-beheerder** of **coadministrator** toegang tot het abonnement dat eigenaar is van de Batch-account te beheren. Zie voor meer informatie de sectie [Azure Active Directory](#azure-active-directory) en de [AccountManagement] [ acct_mgmt_sample] voorbeeld.

## <a name="retrieve-and-regenerate-account-keys"></a>Ophalen en account toetsen genereren

Primaire en secundaire account toetsen van elk account Batch binnen uw abonnement verkrijgen met behulp van [ListKeysAsync][net_list_keys]. U kunt deze toetsen genereren met behulp van [RegenerateKeyAsync][net_regenerate_keys].

```csharp
// Get and print the primary and secondary keys
BatchAccountListKeyResult accountKeys =
    await batchManagementClient.Account.ListKeysAsync(
        "MyResourceGroup",
        "mybatchaccount");
Console.WriteLine("Primary key:   {0}", accountKeys.Primary);
Console.WriteLine("Secondary key: {0}", accountKeys.Secondary);

// Regenerate the primary key
BatchAccountRegenerateKeyResponse newKeys =
    await batchManagementClient.Account.RegenerateKeyAsync(
        "MyResourceGroup",
        "mybatchaccount",
        new BatchAccountRegenerateKeyParameters() {
            KeyName = AccountKeyType.Primary
            });
```

> [AZURE.TIP] U kunt een werkstroom voor het gestroomlijnde verbinding maken voor uw toepassingen management. Eerst moet u een account-toets voor de Batch-account dat u wilt beheren met [ListKeysAsync][net_list_keys]. Gebruik vervolgens deze toets wanneer initialisatie van de Batch .NET-bibliotheek [BatchSharedKeyCredentials] [ net_sharedkeycred] class, die wordt gebruikt bij geïnitialiseerd [BatchClient][net_batch_client].

## <a name="check-azure-subscription-and-batch-account-quotas"></a>Azure-abonnement en Batch account quota's controleren

Azure abonnementen en de afzonderlijke Azure services zoals Batch alle standaard quota's hebt die het aantal bepaalde diensten daarin beperken. Zie [Azure-abonnement en limieten van de service, quota's en beperkingen](./../azure-subscription-service-limits.md)voor de standaardquota voor Azure abonnementen. Zie [quota en beperkingen voor de Batch Azure-service](batch-quota-limit.md)voor de standaardquota van de Batch-service. Met behulp van de Batch Management .NET-bibliotheek, kunt u deze quota's controleren in uw toepassingen. Hiermee kunt u toewijzing besluiten te nemen voordat u accounts toevoegen of berekenen van resources zoals toepassingen en knooppunten berekenen.

### <a name="check-an-azure-subscription-for-batch-account-quotas"></a>Een Azure-abonnement voor Batch account quota's controleren

Voordat u een account Batch maken in een gebied, kunt u uw Azure-abonnement om te zien of u nog een account toevoegen in dat gebied controleren.

In het onderstaande codefragment, gebruiken we eerst [BatchManagementClient.Account.ListAsync] [ net_mgmt_listaccounts] om een verzameling alle Batchaccounts die zich in een abonnement. Nadat u we deze siteverzameling hebt ontvangen, bepalen we met hoeveel accounts zijn in de doelregio. Vervolgens we [BatchManagementClient.Subscriptions gebruiken] [ net_mgmt_subscriptions] downloaden van het quotum voor het account van Batch en bepalen hoeveel accounts (indien aanwezig) kunnen worden gemaakt in die regio.

```csharp
// Get a collection of all Batch accounts within the subscription
BatchAccountListResponse listResponse =
        await batchManagementClient.Account.ListAsync(new AccountListParameters());
IList<AccountResource> accounts = listResponse.Accounts;
Console.WriteLine("Total number of Batch accounts under subscription id {0}:  {1}",
    creds.SubscriptionId,
    accounts.Count);

// Get a count of all accounts within the target region
string region = "westus";
int accountsInRegion = accounts.Count(o => o.Location == region);

// Get the account quota for the specified region
SubscriptionQuotasGetResponse quotaResponse = await batchManagementClient.Subscriptions.GetSubscriptionQuotasAsync(region);
Console.WriteLine("Account quota for {0} region: {1}", region, quotaResponse.AccountQuota);

// Determine how many accounts can be created in the target region
Console.WriteLine("Accounts in {0}: {1}", region, accountsInRegion);
Console.WriteLine("You can create {0} accounts in the {1} region.", quotaResponse.AccountQuota - accountsInRegion, region);
```

In het bovenstaande, fragment `creds` is een exemplaar van [TokenCloudCredentials][azure_tokencreds]. Een voorbeeld van dit object maken, raadpleegt u de [AccountManagement] [ acct_mgmt_sample] voorbeeld op GitHub.

### <a name="check-a-batch-account-for-compute-resource-quotas"></a>Een account Batch voor berekeningscluster resource quota's controleren

Voordat u berekeningscluster resources in de Batch-oplossing met groter wordende, kunt u controleren om ervoor te zorgen de bronnen die u wilt toewijzen, won't overschrijdt de quota van het account. In het onderstaande codefragment we de quotumgegevens voor de Batch rekening met de naam afdrukken `mybatchaccount`. In uw eigen-toepassing, kunt u deze informatie gebruiken om te bepalen of de extra resources worden gemaakt door het account kan worden verwerkt.

```csharp
// First obtain the Batch account
BatchAccountGetResponse getResponse =
    await batchManagementClient.Account.GetAsync("MyResourceGroup", "mybatchaccount");
AccountResource account = getResponse.Resource;

// Now print the compute resource quotas for the account
Console.WriteLine("Core quota: {0}", account.Properties.CoreQuota);
Console.WriteLine("Pool quota: {0}", account.Properties.PoolQuota);
Console.WriteLine("Active job and job schedule quota: {0}", account.Properties.ActiveJobAndJobScheduleQuota);
```

> [AZURE.IMPORTANT] Hoewel er standaardquota voor Azure abonnementen en -services, kunnen u veel van deze limieten verheven een verzoek verzenden in de [portal van Azure][azure_portal]. Zie bijvoorbeeld [quota en beperkingen voor de Batch Azure-service](batch-quota-limit.md) voor instructies over het vergroten van de quota voor uw Batch-account.

## <a name="batch-management-net-azure-ad-and-resource-manager"></a>Management .NET, Azure AD batch en resourcemanager

Wanneer u met de Batch Management .NET-bibliotheek werkt, u meestal ook gebruiken [Azure Active Directory] [ aad_about] (Azure AD) en de [Resourcemanager Azure][resman_overview]. Hieronder beschreven gebruikt in het voorbeeldproject zowel Azure Active Directory en de resourcemanager terwijl de Batch Management .NET-API laat.

### <a name="azure-active-directory"></a>Azure Active Directory

Azure zelf Azure AD wordt gebruikt voor de verificatie van de klanten, servicebeheerders en organisatie-gebruikers. In de context van Batch Management .NET, kunt u Azure AD gebruiken om te verifiëren van een beheerder van het abonnement of een collega beheerder. Hierdoor wordt de bibliotheek management opvragen van de Batch-service en de bewerkingen die in dit artikel worden beschreven.

In het voorbeeldproject hieronder de Azure [Active Directory-verificatie-bibliotheek] wordt vermeld[ aad_adal] (ADAL) wordt gebruikt dat wordt gevraagd de gebruiker voor hun Microsoft-referenties. Wanneer de service-beheerder of coadministrator referenties worden geleverd, kunt de toepassing Azure query voor een lijst met abonnementen-- en kunt maken en zowel resourcegroepen en Batchaccounts verwijderen.

### <a name="resource-manager"></a>Resourcemanager

Wanneer u Batch-accounts met de Batch Management .NET-bibliotheek maken, meestal maakt u deze in een [resourcegroep][resman_overview]. U kunt de resourcegroep via programmacode maken met behulp van de [ResourceManagementClient] [ resman_client] class in [Resourcemanager .NET] [ resman_api] bibliotheek. Of u kunt een account toevoegen aan een bestaande resourcegroep die u hebt gemaakt met het eerder via de [portal van Azure][azure_portal].

## <a name="sample-project-on-github"></a>Voorbeeld van project op GitHub

Als u wilt Batch Management .NET in actie zien, raadpleegt u de [AccountManagment] [ acct_mgmt_sample] steekproef project op GitHub. Deze consoletoepassing ziet u de maken en het gebruik van [BatchManagementClient] [ net_mgmt_client] en [ResourceManagementClient][resman_client]. U ziet ook het gebruik van de Azure [Active Directory verificatie Library] [ aad_adal] (ADAL), die is vereist door beide clients.

Als u wilt de toepassing van de steekproef worden uitgevoerd, moet u eerst registreren deze met Azure AD met behulp van de Azure-portal. Volg de stappen in de sectie [een toepassing toe te voegen](../active-directory/active-directory-integrating-applications.md#adding-an-application) in [de toepassingen integratie met Azure Active Directory] [ aad_integrate] bevindt zich Default Directory om de toepassing van de steekproef binnen uw eigen account te registreren. Zorg ervoor dat u **Systeemeigen clienttoepassing** voor het type toepassing te selecteren en kunt u een geldige URI (zoals `http://myaccountmanagementsample`) voor de **URI omleiden**--niet hoeft te worden van een reële-eindpunt.

Na het toevoegen van uw toepassing, delegeren het **Beheer van een Access-Azure-Service, als de organisatie** machtigingen voor de *Windows Azure Service Management API* -toepassing in de instellingen van de toepassing in de portal:

![Machtigingen voor een toepassing in Azure-portal][2]

> [AZURE.TIP] Als **Management-API voor Windows Azure-Service** niet wordt weergegeven onder *machtigingen voor andere toepassingen*, **toepassing toevoegen**, selecteer **Management-API voor Windows Azure-Service**, klik op het vinkje. Klik, delegeren machtigingen zoals hierboven.

Nadat u kunt de toepassing hebt toegevoegd, zoals hierboven is beschreven, bijwerken `Program.cs` in de [AccountManagment] [ acct_mgmt_sample] steekproef project met van uw toepassing URI omleiden en Client-ID. U kunt deze waarden vinden op het tabblad **configureren** van uw toepassing:

![Toepassingsconfiguratie van de in Azure-portal][3]

De [AccountManagment] [ acct_mgmt_sample] voorbeeldtoepassing ziet u de volgende bewerkingen:

1. In het bezit van een beveiligingstoken van Azure AD met behulp van [ADAL][aad_adal]. Als de gebruiker is niet al aangemeld, wordt deze of hun Azure referenties op te geven.
2. Met behulp van het beveiligingstoken uit Azure AD opgehaald, maakt u een [SubscriptionClient] [ resman_subclient] op query Azure voor een lijst met abonnementen die is gekoppeld aan het account. In deze sectie geeft selectie van een abonnement als er meerdere zijn gevonden.
3. Maak een referenties-object dat is gekoppeld aan de geselecteerde abonnement.
4. Maken van [ResourceManagementClient] [ resman_client] met behulp van de referenties.
5. Gebruik [ResourceManagementClient] [ resman_client] een resourcegroep maken.
6. Gebruik [BatchManagementClient] [ net_mgmt_client] verschillende Batch account bewerkingen uit te voeren:
  - Maak een Batch-account in de resourcegroep nieuw.
  - Krijgen de nieuwe account van de Batch-service.
  - De account te gebruiken voor het nieuwe account afdrukken.
  - Een nieuwe primaire sleutel voor het account opnieuw te genereren.
  - De Quotuminformatie voor het account afdrukken.
  - De Quotuminformatie voor het abonnement afdrukken.
  - Alle accounts in het abonnement afdrukken.
  - Nieuwe account verwijderen.
7. Verwijder de resourcegroep.

Voordat u de nieuwe Batch account- en resourcekalenders groep verwijdert, kunt u zowel in de [portal van Azure]controleren[azure_portal]:

![Azure-portal weergeven van het resourceveld groep en de Batch-account][1]
<br />
*Azure-portal met nieuwe resourcegroep en Batch-account*

[aad_about]: ../active-directory/active-directory-whatis.md "Wat is Azure Active Directory?"
[aad_adal]: ../active-directory/active-directory-authentication-libraries.md
[aad_auth_scenarios]: ../active-directory/active-directory-authentication-scenarios.md "Verificatie-scenario's voor Azure AD"
[aad_integrate]: ../active-directory/active-directory-integrating-applications.md "Toepassingen integreren met Azure Active Directory"
[acct_mgmt_sample]: https://github.com/Azure/azure-batch-samples/tree/master/CSharp/AccountManagement
[api_net]: http://msdn.microsoft.com/library/azure/mt348682.aspx
[api_mgmt_net]: https://msdn.microsoft.com/library/azure/mt463120.aspx
[azure_portal]: http://portal.azure.com
[azure_storage]: https://azure.microsoft.com/services/storage/
[azure_tokencreds]: https://msdn.microsoft.com/library/azure/microsoft.windowsazure.tokencloudcredentials.aspx
[batch_explorer_project]: https://github.com/Azure/azure-batch-samples/tree/master/CSharp/BatchExplorer
[net_batch_client]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.batchclient.aspx
[net_list_keys]: https://msdn.microsoft.com/library/azure/microsoft.azure.management.batch.accountoperationsextensions.listkeysasync.aspx
[net_create]: https://msdn.microsoft.com/library/azure/microsoft.azure.management.batch.accountoperationsextensions.createasync.aspx
[net_delete]: https://msdn.microsoft.com/library/azure/microsoft.azure.management.batch.accountoperationsextensions.deleteasync.aspx
[net_regenerate_keys]: https://msdn.microsoft.com/library/azure/microsoft.azure.management.batch.accountoperationsextensions.regeneratekeyasync.aspx
[net_sharedkeycred]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.auth.batchsharedkeycredentials.aspx
[net_mgmt_client]: https://msdn.microsoft.com/library/azure/microsoft.azure.management.batch.batchmanagementclient.aspx
[net_mgmt_subscriptions]: https://msdn.microsoft.com/library/azure/microsoft.azure.management.batch.batchmanagementclient.subscriptions.aspx
[net_mgmt_listaccounts]: https://msdn.microsoft.com/library/azure/microsoft.azure.management.batch.iaccountoperations.listasync.aspx
[resman_api]: https://msdn.microsoft.com/library/azure/mt418626.aspx
[resman_client]: https://msdn.microsoft.com/library/azure/microsoft.azure.management.resources.resourcemanagementclient.aspxs
[resman_subclient]: https://msdn.microsoft.com/library/azure/microsoft.azure.subscriptions.subscriptionclient.aspx
[resman_overview]: ../azure-resource-manager/resource-group-overview.md

[1]: ./media/batch-management-dotnet/portal-01.png
[2]: ./media/batch-management-dotnet/portal-02.png
[3]: ./media/batch-management-dotnet/portal-03.png
