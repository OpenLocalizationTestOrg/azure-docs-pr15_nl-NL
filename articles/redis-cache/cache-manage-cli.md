<properties 
    pageTitle="Het maken en beheren van Azure bestand Vgx. Cache voor de opdrachtregel Azure (Azure CLI) | Microsoft Azure" 
    description="Leer hoe u de CLI Azure installeert op een willekeurig platform, hoe u dit verbinding maken met uw Azure-account gebruikt en hoe maken en beheren van een bestand Vgx. cache van de Azure CLI." 
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
    ms.date="09/15/2016" 
    ms.author="sdanie"/>

# <a name="how-to-create-and-manage-azure-redis-cache-using-the-azure-command-line-interface-azure-cli"></a>Het maken en beheren van Azure bestand Vgx. Cache voor de opdrachtregel Azure (Azure CLI)

> [AZURE.SELECTOR]
- [PowerShell](cache-howto-manage-redis-cache-powershell.md)
- [Azure CLI](cache-manage-cli.md)

De CLI Azure is een uitstekende manier om de infrastructuur van uw Azure beheren vanuit elk platform. In dit artikel leest u hoe maken en beheren van uw Azure bestand Vgx. Cache-sessies voor het gebruik van de Azure CLI.

## <a name="prerequisites"></a>Vereisten voor

Als u wilt maken en beheren van Azure bestand Vgx. Cache exemplaren Azure CLI gebruiken, moet u de volgende stappen uitvoeren.

-   U moet een Azure-account hebt. Als u deze niet hebt, kunt u een [gratis account](https://azure.microsoft.com/pricing/free-trial/) slechts een paar minuten.
-   [Installeren van de Azure CLI](../xplat-cli-install.md).
-   Verbinding maken met de installatie van Azure CLI met een persoonlijk Azure-account of met een werk of school Azure-account en meld u aan de Azure CLI gebruiken de `azure login` opdracht. Als u wilt weten over de verschillen en kies, Zie [verbinding maken met een Azure abonnement vanaf de opdrachtregel van Azure (Azure CLI)](../xplat-cli-connect.md).
-   Schakelen voordat u een van de volgende opdrachten, de CLI Azure resourcemanager modus door te voeren de `azure config mode arm` opdracht. Zie [de resourcemanager Azure-modus instellen](../xplat-cli-azure-resource-manager.md#set-the-azure-resource-manager-mode)voor meer informatie.

## <a name="redis-cache-properties"></a>Bestand Vgx Cache eigenschappen.

De volgende eigenschappen worden gebruikt bij het maken en bijwerken van bestand Vgx. Cache exemplaren.

| Eigenschap            | Schakeloptie                                  | Beschrijving                                                                                                                                                                                                                                          |
|---------------------|-----------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| naam                | -n,--naam                              | Naam van de Cache bestand Vgx..                                                                                                                                                                                                                             |
| resourcegroep      | -g,--resource-groep                    | Naam van het resourceveld groep.                                                                                                                                                                                                                          |
| locatie            | -l,--locatie                          | Locatie cache maken.                                                                                                                                                                                                                            |
| grootte                | -z,--grootte                              | De grootte van de Cache bestand Vgx.. Geldige waarden: [C0, C1, C2, C3, C4, C5, C6, P1, P2, P3, P4]                                                                                                                                                                  |
| SKU                 | -x,--sku                               | Bestand Vgx. SKU. Een van: [eenvoudige, standaard, Premium]                                                                                                                                                                                             |
| EnableNonSslPort    | -e,--inschakelen niet-ssl-poorten               | De eigenschap EnableNonSslPort van de Cache bestand Vgx.. Deze markering toevoegen als u wilt de niet-SSL-poort voor de cache inschakelen                                                                                                                                    |
| Bestand Vgx configuratie. | -c,--bestand Vgx.-configuratie               | Bestand Vgx. configuratie. Een reeks JSON opgemaakt configuratiesleutels en waarden hier invoeren. Opmaak: "{" ":""," ":" "}"                                                                                                                                     |
| Bestand Vgx configuratie. | -f,--bestand Vgx.-configuratie-bestand          | Bestand Vgx. configuratie. Typ het pad van een bestand met de configuratiesleutels en waarden hier. Indeling voor de bestandsvermelding: {"": "","": ""}                                                                                                                |
| Shard tellen         | -r,--shard tellen                       | Het aantal Shards maken op een Cache Premium Cluster met cluster.                                                                                                                                                                               |
| Virtual Network     | -v,--virtueel-netwerk                   | Wanneer de cache in een VNET host, geeft u de exacte ARM resource-ID van het virtuele netwerk als u wilt implementeren van de cache bestand Vgx. in. Van de voorbeeldindeling: /subscriptions/{subid}/resourceGroups/{resourceGroupName}/Microsoft.ClassicNetwork/VirtualNetworks/vnet1 |
| type sleutel            | -t,--toets-type                          | Type-toets om te verlengen. Geldige waarden: [primaire, secundaire]                                                                                                                                                                                             |
| StaticIP            | -p,--statische ip-< statische ip->             | Wanneer de cache in een VNET host, geeft een uniek IP-adres in het subnet voor de cache. Als u niet wordt opgegeven, wordt een gekozen voor u uit het subnet.                                                                                                |
| Subnet              | t,--subnet<subnet>                    | Wanneer de cache in een VNET host, geeft de naam van het subnet waarin de cache implementeren.                                                                                                                                                    |
| VirtualNetwork      | -v,--virtueel netwerk < virtuele-netwerk > | Wanneer de cache in een VNET host, geeft u de exacte ARM resource-ID van het virtuele netwerk als u wilt implementeren van de cache bestand Vgx. in. Van de voorbeeldindeling: /subscriptions/{subid}/resourceGroups/{resourceGroupName}/Microsoft.ClassicNetwork/VirtualNetworks/vnet1 |
| Abonnement        | -s,--abonnement                      | De id van het abonnement.                                                                                                                                                                                                                         |

## <a name="see-all-redis-cache-commands"></a>Zie opdrachten voor alle Cache bestand Vgx.

Als u wilt zien van alle Cache bestand Vgx. opdrachten en parameters, gebruikt u de `azure rediscache -h` opdracht.

    C:\>azure rediscache -h
    help:    Commands to manage your Azure Redis Cache(s)
    help:
    help:    Create a Redis Cache
    help:      rediscache create [--name <name> --resource-group <resource-group> --location <location> [options]]
    help:
    help:    Delete an existing Redis Cache
    help:      rediscache delete [--name <name> --resource-group <resource-group> ]
    help:
    help:    List all Redis Caches within your Subscription or Resource Group
    help:      rediscache list [options]
    help:
    help:    Show properties of an existing Redis Cache
    help:      rediscache show [--name <name> --resource-group <resource-group>]
    help:
    help:    Change settings of an existing Redis Cache
    help:      rediscache set [--name <name> --resource-group <resource-group> --redis-configuration <redis-configuration>/--redis-configuration-file <redisConfigurationFile>]
    help:
    help:    Renew the authentication key for an existing Redis Cache
    help:      rediscache renew-key [--name <name> --resource-group <resource-group> ]
    help:
    help:    Lists Primary and Secondary key of an existing Redis Cache
    help:      rediscache list-keys [--name <name> --resource-group <resource-group>]
    help:
    help:    Options:
    help:      -h, --help  output usage information
    help:
    help:    Current Mode: arm (Azure Resource Management)

## <a name="create-a-redis-cache"></a>Een bestand Vgx. Cache maken

Als u wilt maken van een bestand Vgx. Cache, gebruik de volgende opdracht uit:

    azure rediscache create [--name <name> --resource-group <resource-group> --location <location> [options]]

Voor meer informatie over deze opdracht, voert u de `azure rediscache create -h` opdracht.

    C:\>azure rediscache create -h
    help:    Create a Redis Cache
    help:
    help:    Usage: rediscache create [--name <name> --resource-group <resource-group> --location <location> [options]]
    help:
    help:    Options:
    help:      -h, --help                                               output usage information
    help:      -v, --verbose                                            use verbose output
    help:      -vv                                                      more verbose with debug output
    help:      --json                                                   use json output
    help:      -n, --name <name>                                        Name of the Redis Cache.
    help:      -g, --resource-group <resource-group>                    Name of the Resource Group
    help:      -l, --location <location>                                Location to create cache.
    help:      -z, --size <size>                                        Size of the Redis Cache. Valid values: [C0, C1, C2, C3, C4, C5, C6, P1, P2, P3, P4]
    help:      -x, --sku <sku>                                          Redis SKU. Should be one of : [Basic, Standard, Premium]
    help:      -e, --enable-non-ssl-port                                EnableNonSslPort property of the Redis Cache. Add this flag if you want to enable the Non SSL Port for your cache
    help:      -c, --redis-configuration <redis-configuration>          Redis Configuration. Enter a JSON formatted string of configuration keys and values here. Format:"{"<key1>":"<value1>","<key2>":"<value2>"}"
    help:      -f, --redis-configuration-file <redisConfigurationFile>  Redis Configuration. Enter the path of a file containing configuration keys and values here. Format for the file entry: {"<key1>":"<value1>","<key2>":"<value2>"}
    help:      -r, --shard-count <shard-count>                          Number of Shards to create on a Premium Cluster Cache
    help:      -v, --virtual-network <virtual-network>                  The exact ARM resource ID of the virtual network to deploy the redis cache in. Example format: /subscriptions/{subid}/resourceGroups/{resourceGroupName}/Microsoft.ClassicNetwork/VirtualNetworks/vnet1
    help:      -t, --subnet <subnet>                                    Required when deploying a redis cache inside an existing Azure Virtual Network
    help:      -p, --static-ip <static-ip>                              Required when deploying a redis cache inside an existing Azure Virtual Network
    help:      -s, --subscription <id>                                  the subscription identifier
    help:
    help:    Current Mode: arm (Azure Resource Management)

## <a name="delete-an-existing-redis-cache"></a>Verwijderen van een bestaande Cache voor het bestand Vgx.

Als u wilt een bestand Vgx. Cache verwijderen, gebruikt u de volgende opdracht uit:

    azure rediscache delete [--name <name> --resource-group <resource-group> ]

Voor meer informatie over deze opdracht, voert u de `azure rediscache delete -h` opdracht.

    C:\>azure rediscache delete -h
    help:    Delete an existing Redis Cache
    help:
    help:    Usage: rediscache delete [--name <name> --resource-group <resource-group> ]
    help:
    help:    Options:
    help:      -h, --help                             output usage information
    help:      -v, --verbose                          use verbose output
    help:      -vv                                    more verbose with debug output
    help:      --json                                 use json output
    help:      -n, --name <name>                      Name of the Redis Cache.
    help:      -g, --resource-group <resource-group>  Name of the Resource Group under which the cache exists
    help:      -s, --subscription <subscription>      the subscription identifier
    help:
    help:    Current Mode: arm (Azure Resource Management)

## <a name="list-all-redis-caches-within-your-subscription-or-resource-group"></a>Een lijst met alle bestand Vgx. cache binnen uw abonnement of een resourcegroep

U kunt alle bestand Vgx. cache binnen uw abonnement of de resourcegroep gebruiken de volgende opdracht uit:

    azure rediscache list [options]

Voor meer informatie over deze opdracht, voert u de `azure rediscache list -h` opdracht.

    C:\>azure rediscache list -h
    help:    List all Redis Caches within your Subscription or Resource Group
    help:
    help:    Usage: rediscache list [options]
    help:
    help:    Options:
    help:      -h, --help                             output usage information
    help:      -v, --verbose                          use verbose output
    help:      -vv                                    more verbose with debug output
    help:      --json                                 use json output
    help:      -g, --resource-group <resource-group>  Name of the Resource Group
    help:      -s, --subscription <subscription>      the subscription identifier
    help:
    help:    Current Mode: arm (Azure Resource Management)

## <a name="show-properties-of-an-existing-redis-cache"></a>Eigenschappen van een bestaand bestand Vgx. Cache weergeven

Als u wilt weergeven Eigenschappen van een bestaand bestand Vgx. Cache, gebruik de volgende opdracht uit:

    azure rediscache show [--name <name> --resource-group <resource-group>]

Voor meer informatie over deze opdracht, voert u de `azure rediscache show -h` opdracht.

    C:\>azure rediscache show -h
    help:    Show properties of an existing Redis Cache
    help:
    help:    Usage: rediscache show [--name <name> --resource-group <resource-group>]
    help:
    help:    Options:
    help:      -h, --help                             output usage information
    help:      -v, --verbose                          use verbose output
    help:      -vv                                    more verbose with debug output
    help:      --json                                 use json output
    help:      -n, --name <name>                      Name of the Redis Cache.
    help:      -g, --resource-group <resource-group>  Name of the Resource Group
    help:      -s, --subscription <subscription>      the subscription identifier
    help:
    help:    Current Mode: arm (Azure Resource Management)

<a name="scale"></a>
## <a name="change-settings-of-an-existing-redis-cache"></a>Instellingen van een bestaand bestand Vgx. Cache wijzigen

Instellingen van een bestaand bestand Vgx. Cache wilt wijzigen door de volgende opdracht te gebruiken:

    azure rediscache set [--name <name> --resource-group <resource-group> --redis-configuration <redis-configuration>/--redis-configuration-file <redisConfigurationFile>]

Voor meer informatie over deze opdracht, voert u de `azure rediscache set -h` opdracht.

    C:\>azure rediscache set -h
    help:    Change settings of an existing Redis Cache
    help:
    help:    Usage: rediscache set [--name <name> --resource-group <resource-group> --redis-configuration <redis-configuration>/--redis-configuration-file <redisConfigurationFile>]
    help:
    help:    Options:
    help:      -h, --help                                               output usage information
    help:      -v, --verbose                                            use verbose output
    help:      -vv                                                      more verbose with debug output
    help:      --json                                                   use json output
    help:      -n, --name <name>                                        Name of the Redis Cache.
    help:      -g, --resource-group <resource-group>                    Name of the Resource Group
    help:      -c, --redis-configuration <redis-configuration>          Redis Configuration. Enter a JSON formatted string of configuration keys and values here.
    help:      -f, --redis-configuration-file <redisConfigurationFile>  Redis Configuration. Enter the path of a file containing configuration keys and values here.
    help:      -s, --subscription <subscription>                        the subscription identifier
    help:
    help:    Current Mode: arm (Azure Resource Management)

## <a name="renew-the-authentication-key-for-an-existing-redis-cache"></a>De verificatie-toets voor een bestaand bestand Vgx. Cache verlengen

Als u wilt de verificatie-toets voor een bestaand bestand Vgx. Cache verlengen, gebruikt u de volgende opdracht uit:

    azure rediscache renew-key [--name <name> --resource-group <resource-group> --key-type <key-type>]

Geef `Primary` of `Secondary` voor `key-type`.

Voor meer informatie over deze opdracht, voert u de `azure rediscache renew-key -h` opdracht.

    C:\>azure rediscache renew-key -h
    help:    Renew the authentication key for an existing Redis Cache
    help:
    help:    Usage: rediscache renew-key [--name <name> --resource-group <resource-group> ]
    help:
    help:    Options:
    help:      -h, --help                             output usage information
    help:      -v, --verbose                          use verbose output
    help:      -vv                                    more verbose with debug output
    help:      --json                                 use json output
    help:      -n, --name <name>                      Name of the Redis Cache.
    help:      -g, --resource-group <resource-group>  Name of the Resource Group under which cache exists
    help:      -t, --key-type <key-type>              type of key to renew. Valid values are: 'Primary', 'Secondary'.
    help:      -s, --subscription <subscription>      the subscription identifier
    help:
    help:    Current Mode: arm (Azure Resource Management)

## <a name="list-primary-and-secondary-keys-of-an-existing-redis-cache"></a>Lijst met primaire en secundaire sleutels van een bestaande Cache voor het bestand Vgx.

Te lijst toetsen primaire en secundaire van een bestaand bestand Vgx. Cache, gebruikt u de volgende opdracht uit:

    azure rediscache list-keys [--name <name> --resource-group <resource-group>]

Voor meer informatie over deze opdracht, voert u de `azure rediscache list-keys -h` opdracht.

    C:\>azure rediscache list-keys -h
    help:    Lists Primary and Secondary key of an existing Redis Cache
    help:
    help:    Usage: rediscache list-keys [--name <name> --resource-group <resource-group>]
    help:
    help:    Options:
    help:      -h, --help                             output usage information
    help:      -v, --verbose                          use verbose output
    help:      -vv                                    more verbose with debug output
    help:      --json                                 use json output
    help:      -n, --name <name>                      Name of the Redis Cache.
    help:      -g, --resource-group <resource-group>  Name of the Resource Group under which Cache exists
    help:      -s, --subscription <subscription>      the subscription identifier
    help:
    help:    Current Mode: arm (Azure Resource Management)
