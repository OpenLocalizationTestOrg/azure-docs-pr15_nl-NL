<properties
    pageTitle="Beheer van Azure bestand Vgx. Cache met Azure PowerShell | Microsoft Azure"
    description="Leer hoe u beheerderstaken uitvoeren voor Azure bestand Vgx. Cache via Azure PowerShell."
    services="redis-cache"
    documentationCenter="" 
    authors="steved0x" 
    manager="douge" 
    editor=""/>

<tags
    ms.service="cache" 
    ms.workload="tbd" 
    ms.tgt_pltfrm="cache-redis" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="10/25/2016" 
    ms.author="sdanie"/>

# <a name="manage-azure-redis-cache-with-azure-powershell"></a>Beheer van Cache met Azure PowerShell Azure bestand Vgx.

> [AZURE.SELECTOR]
- [PowerShell](cache-howto-manage-redis-cache-powershell.md)
- [Azure CLI](cache-manage-cli.md)

In dit onderwerp ziet u hoe om uit te voeren algemene taken zoals maken, bijwerken en schaal uw Azure bestand Vgx. Cache-sessies, hoe u toegangstoetsen genereren en hoe u informatie weergeven over uw cache. Zie [Azure bestand Vgx. Cache cmdlets](https://msdn.microsoft.com/library/azure/mt634513.aspx)voor een volledige lijst van Azure bestand Vgx. Cache PowerShell-cmdlets.

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-rm-include.md)][klassieke model](../resource-manager-deployment-model.md#classic-deployment-characteristics).

## <a name="prerequisites"></a>Vereisten voor

Als u Azure PowerShell al hebt geïnstalleerd, moet u Azure PowerShell versie 1.0.0 hebben of hoger. U kunt de versie van Azure PowerShell die u hebt geïnstalleerd met deze opdracht bij de opdrachtprompt Azure PowerShell controleren.

    Get-Module azure | format-table version

[AZURE.INCLUDE [powershell-preview](../../includes/powershell-preview-inline-include.md)]

Eerst moet u aanmelden bij Azure met deze opdracht.

    Login-AzureRmAccount

Geef het e-mailadres van uw Azure-account en het wachtwoord in het dialoogvenster Microsoft Azure aanmelden.

Vervolgens als u meerdere Azure abonnementen hebt, moet u uw Azure abonnement instellen. Een overzicht van uw huidige abonnement, moet u deze opdracht uitvoeren.

    Get-AzureRmSubscription | sort SubscriptionName | Select SubscriptionName

Als u wilt opgeven van het abonnement, moet u de volgende opdracht uitvoeren. Klik in het volgende voorbeeld wordt de naam van het abonnement is `ContosoSubscription`.

    Select-AzureRmSubscription -SubscriptionName ContosoSubscription

Voordat u Windows PowerShell met Azure resourcemanager gebruiken kunt, moet u het volgende:

- Windows PowerShell, versie 3.0 of 4.0. Als u wilt de versie van Windows PowerShell hebt gevonden, typ:`$PSVersionTable` en controleer of de waarde van `PSVersion` 3.0 of 4.0. Een compatibele om versie te installeren, raadpleegt u [Windows Management Framework 3.0](http://www.microsoft.com/download/details.aspx?id=34595) of [Windows Management Framework 4.0](http://www.microsoft.com/download/details.aspx?id=40855).

Als u gedetailleerde help voor elke cmdlet er in deze zelfstudie, gebruikt u de cmdlet Get-Help.

    Get-Help <cmdlet-name> -Detailed

Als u bijvoorbeeld voor Help-informatie voor de `New-AzureRmRedisCache` cmdlet, type:

    Get-Help New-AzureRmRedisCache -Detailed

### <a name="how-to-connect-to-azure-government-cloud-or-azure-china-cloud"></a>Verbinding maken met Azure overheid Cloud of Azure China Cloud

Omgeving is standaard de Azure `AzureCloud`, die het exemplaar van de globale Azure cloud vertegenwoordigt. Als u wilt verbinden met een ander exemplaar, gebruikt u de `Add-AzureRmAccount` command met de `-Environment` of -`EnvironmentName` opdrachtregel veranderen met de gewenste omgeving of de naam van de omgeving.

Als u wilt zien in de lijst met beschikbare omgevingen, de `Get-AzureRmEnvironment` cmdlet.

### <a name="to-connect-to-the-azure-government-cloud"></a>Verbinding maken met de Cloud met Azure overheid

Als u wilt verbinden met de Cloud met Azure overheid, een van de volgende opdrachten te gebruiken.

    Add-AzureRMAccount -EnvironmentName AzureUSGovernment

of

    Add-AzureRmAccount -Environment (Get-AzureRmEnvironment -Name AzureUSGovernment)

Als u wilt een cache maken in de Cloud met Azure overheid, een van de volgende locaties te gebruiken.

-   USGov Virginia
-   USGov Iowa

Zie voor meer informatie over de Azure overheid Cloud, [Microsoft Azure overheids](https://azure.microsoft.com/features/gov/) - en [Microsoft Azure Government-handleiding voor ontwikkelaars](../azure-government-developer-guide.md).

### <a name="to-connect-to-the-azure-china-cloud"></a>Verbinding maken met de Cloud met Azure China

Als u wilt verbinden met de Cloud met Azure China, moet u een van de volgende opdrachten gebruiken.

    Add-AzureRMAccount -EnvironmentName AzureChinaCloud

of

    Add-AzureRmAccount -Environment (Get-AzureRmEnvironment -Name AzureChinaCloud)

Als u wilt een cache maken in de Cloud met Azure China, een van de volgende locaties te gebruiken.

-   China Oost
-   China Noord

Zie voor meer informatie over de Azure China Cloud, [AzureChinaCloud voor Azure beheerd door 21Vianet in China](http://www.windowsazure.cn/).

### <a name="properties-used-for-azure-redis-cache-powershell"></a>Eigenschappen die worden gebruikt voor PowerShell van Azure Cache bestand Vgx.

De volgende tabel bevat eigenschappen en beschrijvingen voor veelgebruikte parameters bij het maken en beheren van uw Azure bestand Vgx. Cache-sessies via Azure PowerShell.

| Parameter          | Beschrijving                                                                                                                                                                                                        | Standaard  |
|--------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------|
| Naam               | Naam van de cache                                                                                                                                                                                                  |          |
| Locatie           | Locatie van de cache                                                                                                                                                                                              |          |
| ResourceGroupName  | Resources de naam van de groep waarin de cache maken                                                                                                                                                                   |          |
| Grootte               | De grootte van de cache. Geldige waarden zijn: P1, P2, P3, P4, C0, C1, C2, C3, C4, C5, C6, 250MB, 1GB, 2,5 GB, 6 GB, 13 GB, 26 GB, 53 GB                                                                     | 1GB      |
| ShardCount         | Het aantal shards maken bij het maken van een premium-cache met cluster ingeschakeld. Geldige waarden zijn: 1, 2, 3, 4, 5, 6, 7, 8, 9, 10                                                                                                      |          |
| SKU                | Hiermee geeft u de SKU van de cache. Geldige waarden zijn: eenvoudige, standaard, Premium                                                                                                                                         | Standaard |
| RedisConfiguration | Hiermee geeft u een bestand Vgx. configuratie-instellingen. Zie voor meer informatie over de instellingen in de volgende [Eigenschappen van RedisConfiguration](#redisconfiguration-properties) -tabel. |          |
| EnableNonSslPort   | Geeft aan of de niet-SSL-poort is ingeschakeld.                                                                                                                                                                     | ONWAAR    |
| MaxMemoryPolicy    | Deze parameter is afgeschaft - RedisConfiguration in plaats hiervan te gebruiken.                                                                                                                                              |          |
| StaticIP           | Wanneer de cache in een VNET host, geeft een uniek IP-adres in het subnet voor de cache. Als u niet wordt opgegeven, wordt een gekozen voor u uit het subnet.                                                                                                                     |          |
| Subnet             | Wanneer de cache in een VNET host, geeft de naam van het subnet waarin de cache implementeren.                                                                                                                  |          |
| VirtualNetwork     | Wanneer de cache in een VNET host, geeft u de resource-ID van de VNET waarin de cache implementeren.                                                                                                             |          |
| KeyType            | Hiermee geeft u welke toegangstoets te genereren tijdens het vernieuwen van toegangstoetsen. Geldige waarden zijn: primaire, secundaire |  |                                                                                                                                                                                                              |          |


### <a name="redisconfiguration-properties"></a>RedisConfiguration eigenschappen

| Eigenschap                      | Beschrijving                                                                                                          | Prijzen van lagen       |
|-------------------------------|----------------------------------------------------------------------------------------------------------------------|---------------------|
| RDB back-up is ingeschakeld            | Of het [bestand Vgx. gegevens permanente](cache-how-to-premium-persistence.md) is ingeschakeld                                     | Alleen Premium        |
| RDB-opslag-verbindingsreeks | De verbindingsreeks in het account opslag voor het [bestand gegevens permanente Vgx.](cache-how-to-premium-persistence.md)       | Alleen Premium        |
| RDB-back-up-frequentie          | De back-frequentie voor het [bestand gegevens permanente Vgx.](cache-how-to-premium-persistence.md)                               | Alleen Premium        |
| maxmemory gereserveerd            | Hiermee configureert u het [geheugen gereserveerd](cache-configure.md#maxmemory-policy-and-maxmemory-reserved) voor niet-cache processen | Standard en Premium |
| maxmemory-beleid              | Hiermee configureert u de [verwijdering beleid](cache-configure.md#maxmemory-policy-and-maxmemory-reserved) voor de cache           | Alle prijzen lagen   |
| hoogte-keyspace-gebeurtenissen        | [Keyspace meldingen](cache-configure.md#keyspace-notifications-advanced-settings) configureert                     | Standard en Premium |
| hash-max-ziplist-vermeldingen      | Hiermee configureert u [geheugen optimaliseren](http://redis.io/topics/memory-optimization) voor kleine statistische-gegevenstypen          | Standard en Premium |
| hash-max-ziplist-waarde        | Hiermee configureert u [geheugen optimaliseren](http://redis.io/topics/memory-optimization) voor kleine statistische-gegevenstypen          | Standard en Premium |
| set-max-intset-vermeldingen        | Hiermee configureert u [geheugen optimaliseren](http://redis.io/topics/memory-optimization) voor kleine statistische-gegevenstypen          | Standard en Premium |
| zset-max-ziplist-vermeldingen      | Hiermee configureert u [geheugen optimaliseren](http://redis.io/topics/memory-optimization) voor kleine statistische-gegevenstypen          | Standard en Premium |
| zset-max-ziplist-waarde        | Hiermee configureert u [geheugen optimaliseren](http://redis.io/topics/memory-optimization) voor kleine statistische-gegevenstypen          | Standard en Premium |
| databases                     | Hiermee configureert u het aantal databases. Deze eigenschap kan worden geconfigureerd alleen op cache maken.                          | Standard en Premium |

## <a name="to-create-a-redis-cache"></a>Maken van een Cache bestand Vgx.

Nieuwe exemplaren van Azure bestand Vgx. Cache zijn gemaakt met de cmdlet [New-AzureRmRedisCache](https://msdn.microsoft.com/library/azure/mt634517.aspx) .

>[AZURE.IMPORTANT] De eerste keer dat u een bestand Vgx. cache maken in een abonnement met behulp van de Azure portal, de portal registreert de `Microsoft.Cache` naamruimte voor dit abonnement. Als u probeert te maken van de eerste bestand Vgx. cache in een abonnement via PowerShell, moet u eerst registreren die naamruimte met de volgende opdracht. anders cmdlets zoals `New-AzureRmRedisCache` en `Get-AzureRmRedisCache` mislukt.
>
>`Register-AzureRmResourceProvider -ProviderNamespace "Microsoft.Cache"`

Voor een overzicht van beschikbare parameters en de bijbehorende beschrijvingen voor `New-AzureRmRedisCache`, voer de volgende opdracht.

    PS C:\> Get-Help New-AzureRmRedisCache -detailed
    
    NAME
        New-AzureRmRedisCache
    
    SYNOPSIS
        Creates a new redis cache.
    
    
    SYNTAX
        New-AzureRmRedisCache -Name <String> -ResourceGroupName <String> -Location <String> [-RedisVersion <String>]
        [-Size <String>] [-Sku <String>] [-MaxMemoryPolicy <String>] [-RedisConfiguration <Hashtable>] [-EnableNonSslPort
        <Boolean>] [-ShardCount <Integer>] [-VirtualNetwork <String>] [-Subnet <String>] [-StaticIP <String>]
        [<CommonParameters>]
    
    
    DESCRIPTION
        The New-AzureRmRedisCache cmdlet creates a new redis cache.
    
    
    PARAMETERS
        -Name <String>
            Name of the redis cache to create.
    
        -ResourceGroupName <String>
            Name of resource group in which to create the redis cache.
    
        -Location <String>
            Location in which to create the redis cache.
    
        -RedisVersion <String>
            RedisVersion is deprecated and will be removed in future release.
    
        -Size <String>
            Size of the redis cache. The default value is 1GB or C1. Possible values are P1, P2, P3, P4, C0, C1, C2, C3,
            C4, C5, C6, 250MB, 1GB, 2.5GB, 6GB, 13GB, 26GB, 53GB.
    
        -Sku <String>
            Sku of redis cache. The default value is Standard. Possible values are Basic, Standard and Premium.
    
        -MaxMemoryPolicy <String>
            The 'MaxMemoryPolicy' setting has been deprecated. Please use 'RedisConfiguration' setting to set
            MaxMemoryPolicy. e.g. -RedisConfiguration @{"maxmemory-policy" = "allkeys-lru"}
    
        -RedisConfiguration <Hashtable>
            All Redis Configuration Settings. Few possible keys: rdb-backup-enabled, rdb-storage-connection-string,
            rdb-backup-frequency, maxmemory-reserved, maxmemory-policy, notify-keyspace-events, hash-max-ziplist-entries,
            hash-max-ziplist-value, set-max-intset-entries, zset-max-ziplist-entries, zset-max-ziplist-value, databases.
    
        -EnableNonSslPort <Boolean>
            EnableNonSslPort is used by Azure Redis Cache. If no value is provided, the default value is false and the
            non-SSL port will be disabled. Possible values are true and false.
    
        -ShardCount <Integer>
            The number of shards to create on a Premium Cluster Cache.
    
        -VirtualNetwork <String>
            The exact ARM resource ID of the virtual network to deploy the redis cache in. Example format: /subscriptions/{
            subid}/resourceGroups/{resourceGroupName}/providers/Microsoft.ClassicNetwork/VirtualNetworks/{vnetName}
    
        -Subnet <String>
            Required when deploying a redis cache inside an existing Azure Virtual Network.
    
        -StaticIP <String>
            Required when deploying a redis cache inside an existing Azure Virtual Network.
    
        <CommonParameters>
            This cmdlet supports the common parameters: Verbose, Debug,
            ErrorAction, ErrorVariable, WarningAction, WarningVariable,
            OutBuffer, PipelineVariable, and OutVariable. For more information, see
            about_CommonParameters (http://go.microsoft.com/fwlink/?LinkID=113216).

U maakt een cache met standaardparameters die, door de volgende opdracht uit te voeren.

    New-AzureRmRedisCache -ResourceGroupName myGroup -Name mycache -Location "North Central US"

`ResourceGroupName`, `Name`, en `Location` zijn vereiste parameters, maar de rest zijn optioneel en standaardwaarden hebben. De vorige opdracht uitvoert, maakt een standaard SKU Azure bestand Vgx. Cache-instantie met de opgegeven naam, locatie en resourcegroep, die is 1 GB groot de niet-SSL-poort uitgeschakeld.

Als u wilt een premium-cache maken, Geef een grootte van P1 (6 GB - 60 GB), P2 (13 GB - 130 GB), P3 (26 GB - 260 GB), of P4 (53 GB - 530 GB). Als u wilt inschakelen cluster, Geef een shard tellen met de `ShardCount` parameter. Het volgende voorbeeld wordt een P1 premium-cache met 3 shards. Een P1 premium-cache is 6 GB groot en omdat we drie shards opgegeven de totale grootte 18 GB (Gigabytes 3 x 6) is.

    New-AzureRmRedisCache -ResourceGroupName myGroup -Name mycache -Location "North Central US" -Sku Premium -Size P1 -ShardCount 3

Om op te geven waarden voor de `RedisConfiguration` parameter, plaatst u de waarden in `{}` als sleutelwaarde/paren zoals `@{"maxmemory-policy" = "allkeys-random", "notify-keyspace-events" = "KEA"}`. Het volgende voorbeeld wordt een standaard 1 GB cache met `allkeys-random` maxmemory beleid en keyspace meldingen geconfigureerd met `KEA`. Zie voor meer informatie [Keyspace meldingen (Geavanceerd)](cache-configure.md#keyspace-notifications-advanced-settings) en [Maxmemory-beleid en maxmemory-gereserveerde](cache-configure.md#maxmemory-policy-and-maxmemory-reserved).

    New-AzureRmRedisCache -ResourceGroupName myGroup -Name mycache -Location "North Central US" -RedisConfiguration @{"maxmemory-policy" = "allkeys-random", "notify-keyspace-events" = "KEA"}

<a name="databases"></a>
## <a name="to-configure-the-databases-setting-during-cache-creation"></a>Voor het configureren van de databases instellen tijdens het maken van de cache

De `databases` instelling kan worden geconfigureerd alleen tijdens het maken van de cache. Het volgende voorbeeld wordt een premium P3 (26 GB) cache met 48 databases met de cmdlet [New-AzureRmRedisCache](https://msdn.microsoft.com/library/azure/mt634517.aspx) .

    New-AzureRmRedisCache -ResourceGroupName myGroup -Name mycache -Location "North Central US" -Sku Premium -Size P3 -RedisConfiguration @{"databases" = "48"}

Voor meer informatie over de `databases` eigenschap, raadpleegt u [standaard Azure bestand Vgx. Cache serverconfiguratie](cache-configure.md#default-redis-server-configuration). Zie voor meer informatie over het maken van een cache met de cmdlet [New-AzureRmRedisCache](https://msdn.microsoft.com/library/azure/mt634517.aspx) de vorige sectie [om te maken van een bestand Vgx. Cache](#to-create-a-redis-cache) .

## <a name="to-update-a-redis-cache"></a>Bij een cache bestand Vgx.

Azure bestand Vgx. Cache exemplaren worden bijgewerkt met de cmdlet [Set-AzureRmRedisCache](https://msdn.microsoft.com/library/azure/mt634518.aspx) .

Voor een overzicht van beschikbare parameters en de bijbehorende beschrijvingen voor `Set-AzureRmRedisCache`, voer de volgende opdracht.

    PS C:\> Get-Help Set-AzureRmRedisCache -detailed
    
    NAME
        Set-AzureRmRedisCache
    
    SYNOPSIS
        Set redis cache updatable parameters.
    
    SYNTAX
        Set-AzureRmRedisCache -Name <String> -ResourceGroupName <String> [-Size <String>] [-Sku <String>]
        [-MaxMemoryPolicy <String>] [-RedisConfiguration <Hashtable>] [-EnableNonSslPort <Boolean>] [-ShardCount
        <Integer>] [<CommonParameters>]
    
    DESCRIPTION
        The Set-AzureRmRedisCache cmdlet sets redis cache parameters.
    
    PARAMETERS
        -Name <String>
            Name of the redis cache to update.
    
        -ResourceGroupName <String>
            Name of the resource group for the cache.
    
        -Size <String>
            Size of the redis cache. The default value is 1GB or C1. Possible values are P1, P2, P3, P4, C0, C1, C2, C3,
            C4, C5, C6, 250MB, 1GB, 2.5GB, 6GB, 13GB, 26GB, 53GB.
    
        -Sku <String>
            Sku of redis cache. The default value is Standard. Possible values are Basic, Standard and Premium.
    
        -MaxMemoryPolicy <String>
            The 'MaxMemoryPolicy' setting has been deprecated. Please use 'RedisConfiguration' setting to set
            MaxMemoryPolicy. e.g. -RedisConfiguration @{"maxmemory-policy" = "allkeys-lru"}
    
        -RedisConfiguration <Hashtable>
            All Redis Configuration Settings. Few possible keys: rdb-backup-enabled, rdb-storage-connection-string,
            rdb-backup-frequency, maxmemory-reserved, maxmemory-policy, notify-keyspace-events, hash-max-ziplist-entries,
            hash-max-ziplist-value, set-max-intset-entries, zset-max-ziplist-entries, zset-max-ziplist-value.
    
        -EnableNonSslPort <Boolean>
            EnableNonSslPort is used by Azure Redis Cache. The default value is null and no change will be made to the
            currently configured value. Possible values are true and false.
    
        -ShardCount <Integer>
            The number of shards to create on a Premium Cluster Cache.
    
        <CommonParameters>
            This cmdlet supports the common parameters: Verbose, Debug,
            ErrorAction, ErrorVariable, WarningAction, WarningVariable,
            OutBuffer, PipelineVariable, and OutVariable. For more information, see
            about_CommonParameters (http://go.microsoft.com/fwlink/?LinkID=113216).

De `Set-AzureRmRedisCache` cmdlet kan worden gebruikt voor het bijwerken van eigenschappen zoals `Size`, `Sku`, `EnableNonSslPort`, en de `RedisConfiguration` waarden. 

De volgende opdracht werkt het maxmemory-beleid voor het bestand Vgx. Cache myCache met de naam.

    Set-AzureRmRedisCache -ResourceGroupName "myGroup" -Name "myCache" -RedisConfiguration @{"maxmemory-policy" = "allkeys-random"}

<a name="scale"></a>
## <a name="to-scale-a-redis-cache"></a>Aan de nieuwe schaal een cache bestand Vgx.

`Set-AzureRmRedisCache`kunnen worden gebruikt om de schaal van een cache Azure bestand Vgx. exemplaar wanneer de `Size`, `Sku`, of `ShardCount` eigenschappen zijn gewijzigd. 

>[AZURE.NOTE]Schaalbaarheid van een cache via PowerShell is onderhevig aan dezelfde limieten en richtlijnen als schaalbaarheid van een cache van de Azure-portal. U kunt schalen naar een ander prijzen laag met de volgende beperkingen.
>
>-  U niet kunt schalen uit een hogere prijzen laag naar een lagere prijzen niveau.
>    -    U kunt geen schalen vanuit een **Premium** -cache naar beneden af op een **standaard** - of een **eenvoudige** cache.
>    -    U kunt geen schalen uit een **standaard** cache naar beneden af op een **eenvoudige** cache.
>-  Kunt u de schaal van een **eenvoudige** cache naar een **standaard** cache, maar u kunt het formaat niet wijzigen op hetzelfde moment. Als u een andere grootte nodig hebt, kunt u de volgende schaal bewerking naar de gewenste grootte kunt doen.
>-  U kunt geen schalen uit een **eenvoudige** cache rechtstreeks naar een **Premium** -cache. U moet de schaal van **eenvoudige** naar **standaard** één schaal betrekking heeft en vervolgens van **standaard** naar **Premium** in de volgende schaal bewerking.
>-  U niet kunt schaal van een groter omlaag naar de **C0 (250 MB)** grootte.
>
>Lees [hoe u de schaal Azure bestand Vgx. Cache](cache-how-to-scale.md)voor meer informatie.

Het volgende voorbeeld ziet u hoe u de schaal van een cache met de naam `myCache` naar een cache 2,5 GB. Houd er rekening mee dat deze opdracht voor zowel een Basic of een standaard cache werkt.

    Set-AzureRmRedisCache -ResourceGroupName myGroup -Name myCache -Size 2.5GB

Nadat u deze opdracht wordt uitgevoerd, de status van de cache als resultaat gegeven (vergelijkbaar met bellen `Get-AzureRmRedisCache`). Houd er rekening mee dat het `ProvisioningState` is `Scaling`.

    PS C:\> Set-AzureRmRedisCache -Name myCache -ResourceGroupName myGroup -Size 2.5GB
    
    
    Name               : mycache
    Id                 : /subscriptions/12ad12bd-abdc-2231-a2ed-a2b8b246bbad4/resourceGroups/mygroup/providers/Mi
                         crosoft.Cache/Redis/mycache
    Location           : South Central US
    Type               : Microsoft.Cache/Redis
    HostName           : mycache.redis.cache.windows.net
    Port               : 6379
    ProvisioningState  : Scaling
    SslPort            : 6380
    RedisConfiguration : {[maxmemory-policy, volatile-lru], [maxmemory-reserved, 150], [notify-keyspace-events, KEA],
                         [maxmemory-delta, 150]...}
    EnableNonSslPort   : False
    RedisVersion       : 3.0
    Size               : 1GB
    Sku                : Standard
    ResourceGroupName  : mygroup
    PrimaryKey         : ....
    SecondaryKey       : ....
    VirtualNetwork     :
    Subnet             :
    StaticIP           :
    TenantSettings     : {}
    ShardCount         :

Wanneer de schaal bewerking voltooid is, de `ProvisioningState` wordt gewijzigd in `Succeeded`. Als u wilt aanbrengen van de volgende schaal bewerking, bijvoorbeeld het wijzigen van eenvoudig naar standaard en klik vervolgens wijzigen van de grootte, moet u wachten totdat de vorige bewerking voltooid is of er een vergelijkbaar met het volgende foutbericht.

    Set-AzureRmRedisCache : Conflict: The resource '...' is not in a stable state, and is currently unable to accept the update request.

## <a name="to-get-information-about-a-redis-cache"></a>Voor informatie over een cache bestand Vgx.

U kunt informatie over een cache met de cmdlet [Get-AzureRmRedisCache](https://msdn.microsoft.com/library/azure/mt634514.aspx) ophalen.

Voor een overzicht van beschikbare parameters en de bijbehorende beschrijvingen voor `Get-AzureRmRedisCache`, voer de volgende opdracht.

    PS C:\> Get-Help Get-AzureRmRedisCache -detailed
    
    NAME
        Get-AzureRmRedisCache
    
    SYNOPSIS
        Gets details about a single cache or all caches in the specified resource group or all caches in the current
        subscription.
    
    SYNTAX
        Get-AzureRmRedisCache [-Name <String>] [-ResourceGroupName <String>] [<CommonParameters>]
    
    DESCRIPTION
        The Get-AzureRmRedisCache cmdlet gets the details about a cache or caches depending on input parameters. If both
        ResourceGroupName and Name parameters are provided then Get-AzureRmRedisCache will return details about the
        specific cache name provided.
    
        If only ResourceGroupName is provided than it will return details about all caches in the specified resource group.
    
        If no parameters are given than it will return details about all caches the current subscription.
    
    PARAMETERS
        -Name <String>
            The name of the cache. When this parameter is provided along with ResourceGroupName, Get-AzureRmRedisCache
            returns the details for the cache.
    
        -ResourceGroupName <String>
            The name of the resource group that contains the cache or caches. If ResourceGroupName is provided with Name
            then Get-AzureRmRedisCache returns the details of the cache specified by Name. If only the ResourceGroup
            parameter is provided, then details for all caches in the resource group are returned.
    
        <CommonParameters>
            This cmdlet supports the common parameters: Verbose, Debug,
            ErrorAction, ErrorVariable, WarningAction, WarningVariable,
            OutBuffer, PipelineVariable, and OutVariable. For more information, see
            about_CommonParameters (http://go.microsoft.com/fwlink/?LinkID=113216).

Als u wilt teruggaan naar informatie over alle cache in het huidige abonnement, uitvoeren `Get-AzureRmRedisCache` zonder parameters.

    Get-AzureRmRedisCache

Als u wilt teruggaan naar informatie over alle cache in een bepaalde resourcegroep, uitvoeren `Get-AzureRmRedisCache` met de `ResourceGroupName` parameter.

    Get-AzureRmRedisCache -ResourceGroupName myGroup

Als u wilt teruggaan naar informatie over een specifieke cache, uitvoeren `Get-AzureRmRedisCache` met de `Name` parameter met de naam van de cache, en de `ResourceGroupName` parameter met de resourcegroep met die cache.

    PS C:\> Get-AzureRmRedisCache -Name myCache -ResourceGroupName myGroup
    
    Name               : mycache
    Id                 : /subscriptions/12ad12bd-abdc-2231-a2ed-a2b8b246bbad4/resourceGroups/myGroup/providers/Mi
                         crosoft.Cache/Redis/mycache
    Location           : South Central US
    Type               : Microsoft.Cache/Redis
    HostName           : mycache.redis.cache.windows.net
    Port               : 6379
    ProvisioningState  : Succeeded
    SslPort            : 6380
    RedisConfiguration : {[maxmemory-policy, volatile-lru], [maxmemory-reserved, 62], [notify-keyspace-events, KEA],
                         [maxclients, 1000]...}
    EnableNonSslPort   : False
    RedisVersion       : 3.0
    Size               : 1GB
    Sku                : Standard
    ResourceGroupName  : myGroup
    VirtualNetwork     :
    Subnet             :
    StaticIP           :
    TenantSettings     : {}
    ShardCount         :

## <a name="to-retrieve-the-access-keys-for-a-redis-cache"></a>Om op te halen de toegangstoetsen voor een cache bestand Vgx.

Als u wilt ophalen de toegangstoetsen voor de cache, kunt u de cmdlet [Get-AzureRmRedisCacheKey](https://msdn.microsoft.com/library/azure/mt634516.aspx) .

Voor een overzicht van beschikbare parameters en de bijbehorende beschrijvingen voor `Get-AzureRmRedisCacheKey`, voer de volgende opdracht.

    PS C:\> Get-Help Get-AzureRmRedisCacheKey -detailed
    
    NAME
        Get-AzureRmRedisCacheKey
    
    SYNOPSIS
        Gets the accesskeys for the specified redis cache.
    
    
    SYNTAX
        Get-AzureRmRedisCacheKey -Name <String> -ResourceGroupName <String> [<CommonParameters>]
    
    DESCRIPTION
        The Get-AzureRmRedisCacheKey cmdlet gets the access keys for the specified cache.
    
    PARAMETERS
        -Name <String>
            Name of the redis cache.
    
        -ResourceGroupName <String>
            Name of the resource group for the cache.
    
        <CommonParameters>
            This cmdlet supports the common parameters: Verbose, Debug,
            ErrorAction, ErrorVariable, WarningAction, WarningVariable,
            OutBuffer, PipelineVariable, and OutVariable. For more information, see
            about_CommonParameters (http://go.microsoft.com/fwlink/?LinkID=113216).

Bellen om op te halen de sleutels voor de cache, de `Get-AzureRmRedisCacheKey` cmdlet en naam van de cache van de naam van het resourceveld groep waarin de cache doorgeeft.

    PS C:\> Get-AzureRmRedisCacheKey -Name myCache -ResourceGroupName myGroup
    
    PrimaryKey   : b2wdt43sfetlju4hfbryfnregrd9wgIcc6IA3zAO1lY=
    SecondaryKey : ABhfB757JgjIgt785JgKH9865eifmekfnn649303JKL=

## <a name="to-regenerate-access-keys-for-your-redis-cache"></a>Te genereren toegangstoetsen voor de cache bestand Vgx.

Als u wilt de toegangstoetsen voor de cache genereren, kunt u de cmdlet [New-AzureRmRedisCacheKey](https://msdn.microsoft.com/library/azure/mt634512.aspx) .

Voor een overzicht van beschikbare parameters en de bijbehorende beschrijvingen voor `New-AzureRmRedisCacheKey`, voer de volgende opdracht.

    PS C:\> Get-Help New-AzureRmRedisCacheKey -detailed
    
    NAME
        New-AzureRmRedisCacheKey
    
    SYNOPSIS
        Regenerates the access key of a redis cache.
    
    SYNTAX
        New-AzureRmRedisCacheKey -Name <String> -ResourceGroupName <String> -KeyType <String> [-Force] [<CommonParameters>]
    
    DESCRIPTION
        The New-AzureRmRedisCacheKey cmdlet regenerate the access key of a redis cache.
    
    PARAMETERS
        -Name <String>
            Name of the redis cache.
    
        -ResourceGroupName <String>
            Name of the resource group for the cache.
    
        -KeyType <String>
            Specifies whether to regenerate the primary or secondary access key. Possible values are Primary or Secondary.
    
        -Force
            When the Force parameter is provided, the specified access key is regenerated without any confirmation prompts.
    
        <CommonParameters>
            This cmdlet supports the common parameters: Verbose, Debug,
            ErrorAction, ErrorVariable, WarningAction, WarningVariable,
            OutBuffer, PipelineVariable, and OutVariable. For more information, see
            about_CommonParameters (http://go.microsoft.com/fwlink/?LinkID=113216).
    
Als u wilt de primaire of secundaire-toets voor de cache genereren, bellen de `New-AzureRmRedisCacheKey` cmdlet en doorgeven in de naam van de resourcegroep, en opgeven `Primary` of `Secondary` voor de `KeyType` parameter. In het volgende voorbeeld wordt de secundaire toegangstoets voor een cache opnieuw wordt gegenereerd.

    PS C:\> New-AzureRmRedisCacheKey -Name myCache -ResourceGroupName myGroup -KeyType Secondary
    
    Confirm
    Are you sure you want to regenerate Secondary key for redis cache 'myCache'?
    [Y] Yes  [N] No  [S] Suspend  [?] Help (default is "Y"): Y
    
    
    PrimaryKey   : b2wdt43sfetlju4hfbryfnregrd9wgIcc6IA3zAO1lY=
    SecondaryKey : c53hj3kh4jhHjPJk8l0jji785JgKH9865eifmekfnn6=

## <a name="to-delete-a-redis-cache"></a>Verwijderen van een cache bestand Vgx.

Als u wilt een bestand Vgx. cache verwijderen, gebruikt u de cmdlet [Verwijderen-AzureRmRedisCache](https://msdn.microsoft.com/library/azure/mt634515.aspx) .

Voor een overzicht van beschikbare parameters en de bijbehorende beschrijvingen voor `Remove-AzureRmRedisCache`, voer de volgende opdracht.

    PS C:\> Get-Help Remove-AzureRmRedisCache -detailed
    
    NAME
        Remove-AzureRmRedisCache
    
    SYNOPSIS
        Remove redis cache if exists.
    
    SYNTAX
        Remove-AzureRmRedisCache -Name <String> -ResourceGroupName <String> [-Force] [-PassThru] [<CommonParameters>
    
    DESCRIPTION
        The Remove-AzureRmRedisCache cmdlet removes a redis cache if it exists.
    
    PARAMETERS
        -Name <String>
            Name of the redis cache to remove.
    
        -ResourceGroupName <String>
            Name of the resource group of the cache to remove.
    
        -Force
            When the Force parameter is provided, the cache is removed without any confirmation prompts.
    
        -PassThru
            By default Remove-AzureRmRedisCache removes the cache and does not return any value. If the PassThru par
            is provided then Remove-AzureRmRedisCache returns a boolean value indicating the success of the operatio
    
        <CommonParameters>
            This cmdlet supports the common parameters: Verbose, Debug,
            ErrorAction, ErrorVariable, WarningAction, WarningVariable,
            OutBuffer, PipelineVariable, and OutVariable. For more information, see
            about_CommonParameters (http://go.microsoft.com/fwlink/?LinkID=113216).

Klik in het volgende voorbeeld wordt de cache met de naam `myCache` wordt verwijderd.

    PS C:\> Remove-AzureRmRedisCache -Name myCache -ResourceGroupName myGroup
    
    Confirm
    Are you sure you want to remove redis cache 'myCache'?
    [Y] Yes  [N] No  [S] Suspend  [?] Help (default is "Y"): Y


## <a name="to-import-a-redis-cache"></a>Het importeren van een cache bestand Vgx.

U kunt gegevens importeren in een Azure bestand Vgx. Cache exemplaar met de `Import-AzureRmRedisCache` cmdlet.

>[AZURE.IMPORTANT] Importeren/exporteren is alleen beschikbaar voor [premium laag](cache-premium-tier-intro.md) cache. Zie voor meer informatie over het importeren/exporteren, [importeren en exporteren van gegevens in de Cache van Azure bestand Vgx.](cache-how-to-import-export-data.md).

Voor een overzicht van beschikbare parameters en de bijbehorende beschrijvingen voor `Import-AzureRmRedisCache`, voer de volgende opdracht.

    PS C:\> Get-Help Import-AzureRmRedisCache -detailed
    
    NAME
        Import-AzureRmRedisCache
    
    SYNOPSIS
        Import data from blobs to Azure Redis Cache.
    
    
    SYNTAX
        Import-AzureRmRedisCache -Name <String> -ResourceGroupName <String> -Files <String[]> [-Format <String>] [-Force]
        [-PassThru] [<CommonParameters>]
    
    
    DESCRIPTION
        The Import-AzureRmRedisCache cmdlet imports data from the specified blobs into Azure Redis Cache.
    
    
    PARAMETERS
        -Name <String>
            The name of the cache.
    
        -ResourceGroupName <String>
            The name of the resource group that contains the cache.
    
        -Files <String[]>
            SAS urls of blobs whose content should be imported into the cache.
    
        -Format <String>
            Format for the blob.  Currently "rdb" is the only supported, with other formats expected in the future.
    
        -Force
            When the Force parameter is provided, import will be performed without any confirmation prompts.
    
        -PassThru
            By default Import-AzureRmRedisCache imports data in cache and does not return any value. If the PassThru
            parameter is provided then Import-AzureRmRedisCache returns a boolean value indicating the success of the
            operation.
    
        <CommonParameters>
            This cmdlet supports the common parameters: Verbose, Debug,
            ErrorAction, ErrorVariable, WarningAction, WarningVariable,
            OutBuffer, PipelineVariable, and OutVariable. For more information, see
            about_CommonParameters (http://go.microsoft.com/fwlink/?LinkID=113216).
    

De volgende opdracht importeert gegevens uit de opgegeven door de uri SA's in bestand Vgx. Cache van Azure blob.


    PS C:\>Import-AzureRmRedisCache -ResourceGroupName "resourceGroupName" -Name "cacheName" -Files @("https://mystorageaccount.blob.core.windows.net/mycontainername/blobname?sv=2015-04-05&sr=b&sig=caIwutG2uDa0NZ8mjdNJdgOY8%2F8mhwRuGNdICU%2B0pI4%3D&st=2016-05-27T00%3A00%3A00Z&se=2016-05-28T00%3A00%3A00Z&sp=rwd") -Force

## <a name="to-export-a-redis-cache"></a>Als u wilt exporteren van een cache bestand Vgx.

U kunt gegevens exporteren vanuit een bestand Vgx. Azure-Cache exemplaar met de `Export-AzureRmRedisCache` cmdlet.

>[AZURE.IMPORTANT] Importeren/exporteren is alleen beschikbaar voor [premium laag](cache-premium-tier-intro.md) cache. Zie voor meer informatie over het importeren/exporteren, [importeren en exporteren van gegevens in de Cache van Azure bestand Vgx.](cache-how-to-import-export-data.md).

Voor een overzicht van beschikbare parameters en de bijbehorende beschrijvingen voor `Export-AzureRmRedisCache`, voer de volgende opdracht.

    PS C:\> Get-Help Export-AzureRmRedisCache -detailed
    
    NAME
        Export-AzureRmRedisCache
    
    SYNOPSIS
        Exports data from Azure Redis Cache to a specified container.
    
    
    SYNTAX
        Export-AzureRmRedisCache -Name <String> -ResourceGroupName <String> -Prefix <String> -Container <String> [-Format
        <String>] [-PassThru] [<CommonParameters>]
    
    
    DESCRIPTION
        The Export-AzureRmRedisCache cmdlet exports data from Azure Redis Cache to a specified container.
    
    
    PARAMETERS
        -Name <String>
            The name of the cache.
    
        -ResourceGroupName <String>
            The name of the resource group that contains the cache.
    
        -Prefix <String>
            Prefix to use for blob names.
    
        -Container <String>
            SAS url of container where data should be exported.
    
        -Format <String>
            Format for the blob.  Currently "rdb" is the only supported, with other formats expected in the future.
    
        -PassThru
            By default Export-AzureRmRedisCache does not return any value. If the PassThru parameter is provided
            then Export-AzureRmRedisCache returns a boolean value indicating the success of the operation.
    
        <CommonParameters>
            This cmdlet supports the common parameters: Verbose, Debug,
            ErrorAction, ErrorVariable, WarningAction, WarningVariable,
            OutBuffer, PipelineVariable, and OutVariable. For more information, see
            about_CommonParameters (http://go.microsoft.com/fwlink/?LinkID=113216).


De volgende opdracht Hiermee worden gegevens geëxporteerd vanuit een exemplaar van de Azure bestand Vgx. Cache in de container opgegeven door de SA's-uri.


        PS C:\>Export-AzureRmRedisCache -ResourceGroupName "resourceGroupName" -Name "cacheName" -Prefix "blobprefix"
        -Container "https://mystorageaccount.blob.core.windows.net/mycontainer?sv=2015-04-05&sr=c&sig=HezZtBZ3DURmEGDduauE7
        pvETY4kqlPI8JCNa8ATmaw%3D&st=2016-05-27T00%3A00%3A00Z&se=2016-05-28T00%3A00%3A00Z&sp=rwdl"

## <a name="to-reboot-a-redis-cache"></a>Opnieuw opstarten een cache bestand Vgx.

U kunt opstarten uw Azure bestand Vgx. Cache exemplaar gebruiken de `Reset-AzureRmRedisCache` cmdlet.

>[AZURE.IMPORTANT] Opnieuw opstarten is alleen beschikbaar voor [premium laag](cache-premium-tier-intro.md) cache. Zie voor meer informatie over de cache opgestart, [beheer van de Cache - start opnieuw op](cache-administration.md#reboot).

Voor een overzicht van beschikbare parameters en de bijbehorende beschrijvingen voor `Reset-AzureRmRedisCache`, voer de volgende opdracht.

    PS C:\> Get-Help Reset-AzureRmRedisCache -detailed
    
    NAME
        Reset-AzureRmRedisCache
    
    SYNOPSIS
        Reboot specified node(s) of an Azure Redis Cache instance.
    
    
    SYNTAX
        Reset-AzureRmRedisCache -Name <String> -ResourceGroupName <String> -RebootType <String> [-ShardId <Integer>]
        [-Force] [-PassThru] [<CommonParameters>]
    
    
    DESCRIPTION
        The Reset-AzureRmRedisCache cmdlet reboots the specified node(s) of an Azure Redis Cache instance.
    
    
    PARAMETERS
        -Name <String>
            The name of the cache.
    
        -ResourceGroupName <String>
            The name of the resource group that contains the cache.
    
        -RebootType <String>
            Which node to reboot. Possible values are "PrimaryNode", "SecondaryNode", "AllNodes".
    
        -ShardId <Integer>
            Which shard to reboot when rebooting a premium cache with clustering enabled.
    
        -Force
            When the Force parameter is provided, reset will be performed without any confirmation prompts.
    
        -PassThru
            By default Reset-AzureRmRedisCache does not return any value. If the PassThru parameter is provided
            then Reset-AzureRmRedisCache returns a boolean value indicating the success of the operation.
    
        <CommonParameters>
            This cmdlet supports the common parameters: Verbose, Debug,
            ErrorAction, ErrorVariable, WarningAction, WarningVariable,
            OutBuffer, PipelineVariable, and OutVariable. For more information, see
            about_CommonParameters (http://go.microsoft.com/fwlink/?LinkID=113216).
    

De volgende opdracht opnieuw is opgestart beide knooppunten van de opgegeven cache.

    
        PS C:\>Reset-AzureRmRedisCache -ResourceGroupName "resourceGroupName" -Name "cacheName" -RebootType "AllNodes"
        -Force
    

## <a name="next-steps"></a>Volgende stappen

Meer informatie over het gebruik van Windows PowerShell met Azure, raadpleegt u de volgende bronnen:

- [Azure bestand Vgx. Cache cmdlet documentatie op MSDN](https://msdn.microsoft.com/library/azure/mt634513.aspx)
- [Azure resourcemanager Cmdlets](http://go.microsoft.com/fwlink/?LinkID=394765): Leer hoe u de cmdlets gebruiken in de module AzureResourceManager.
- [Resourcegebruik groepen voor het beheren van uw Azure resources](../resource-group-template-deploy-portal.md): meer informatie over het maken en beheren van resourcegroepen in de portal van Azure.
- [Azure-blog](http://blogs.msdn.com/windowsazure): meer informatie over nieuwe functies in Azure wordt aangegeven.
- [Windows PowerShell-blog](http://blogs.msdn.com/powershell): meer informatie over nieuwe functies in Windows PowerShell.
- ["Hoi Scripting man!" Blog](http://blogs.technet.com/b/heyscriptingguy/): echte tips en trucs krijgen van de Windows PowerShell-community.
