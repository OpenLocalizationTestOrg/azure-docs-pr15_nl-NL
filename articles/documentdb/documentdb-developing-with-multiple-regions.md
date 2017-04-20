<properties
   pageTitle="Ontwikkelen met meerdere gebieden in DocumentDB | Microsoft Azure"
   description="Leer hoe u toegang tot uw gegevens in meerdere gebieden van Azure DocumentDB, een volledig beheerde NoSQL database-service."
   services="documentdb"
   documentationCenter=""
   authors="kiratp"
   manager="jhubbard"
   editor=""/>

<tags
   ms.service="documentdb"
   ms.devlang="multiple"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="10/25/2016"
   ms.author="kipandya"/>
   
# <a name="developing-with-multi-region-documentdb-accounts"></a>Ontwikkelen met DocumentDB-accounts voor meerdere landen/regio

> [AZURE.NOTE] Globale verdeling van DocumentDB databases is algemeen beschikbaar en is automatisch ingeschakeld voor uw nieuwe DocumentDB-accounts. We werken om in te schakelen globale verdeling voor alle bestaande accounts, maar in de tussentijd, indien distributielijsten ingeschakeld voor uw account gewenst, neem [contact opnemen met ondersteuning](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) en we wordt inschakelen voor u nu.

Om optimaal te profiteren van [globale verdeling](documentdb-distribute-data-globally.md), wordt de lijst geordende voorkeur van regio's worden gebruikt voor het document bewerkingen uitvoeren in clienttoepassingen kunnen opgeven. Dit kunt doen door in te stellen van het verbindingsbeleid. Op basis van de configuratie van de Azure DocumentDB-account, huidige regionale beschikbaarheid en de voorkeur-lijst die is opgegeven, wordt het meest optimale eindpunt gekozen door de SDK uit te voeren schrijven bewerkingen lezen. 

Deze lijst voorkeuren is opgegeven tijdens initialisatie van een verbinding via de DocumentDB-client SDK's. De SDK's is een optionele parameter "PreferredLocations" accepteren dat wil zeggen een geordende lijst van Azure regio's.

De SDK stuurt automatisch dat alle schrijven naar de huidige schrijven regio. 

Alle lees worden verzonden naar de eerste beschikbare regio in de lijst PreferredLocations. Als het verzoek mislukt, wordt de client mislukt omlaag in de lijst de volgende regio, enzovoort. 

De client die SDK 's alleen te probeert lezen van de gebieden opgegeven in PreferredLocations. Zo is, bijvoorbeeld als de Database-Account beschikbaar in drie regio's is, maar de client alleen twee van de niet-schrijven regio's voor PreferredLocations bevat, klikt u vervolgens geen lees moeten worden verzonden het gebied schrijven, zelfs wanneer een failover.

De toepassing kunt controleren of de huidige schrijven eindpunt en lees eindpunt gekozen door de SDK door twee eigenschappen, WriteEndpoint en ReadEndpoint, beschikbaar in SDK versie 1.8 en hierboven te controleren. 

Als de eigenschap PreferredLocations niet is ingesteld, worden alle aanvragen worden served uit het huidige schrijven tweede regio. 


## <a name="net-sdk"></a>.NET SDK
De SDK kan worden gebruikt zonder codewijzigingen. In dit geval de SDK automatisch stuurt beide lees en schrijft naar de huidige schrijven regio. 

In versie 1,8 en hoger van de .NET SDK bevat de ConnectionPolicy-parameter voor de constructor DocumentClient een eigenschap Microsoft.Azure.Documents.ConnectionPolicy.PreferredLocations genoemd. Deze eigenschap is van het type siteverzameling `<string>` en een lijst met regionamen moet bevatten. De tekenreekswaarden zijn opgemaakt per de kolom naam van de regio in de [Regio's Azure]  [ regions] pagina, zonder spaties voor of na de eerste en laatste teken respectievelijk.

De huidige schrijven en lees eindpunten zijn respectievelijk beschikbaar in DocumentClient.WriteEndpoint en DocumentClient.ReadEndpoint.

> [AZURE.NOTE] De URL's voor de eindpunten niet beschouwd als lange levensduur constanten. De service, kan deze op elk gewenst moment bijwerken. Deze wijziging worden automatisch verwerkt in de SDK.

    // Getting endpoints from application settings or other configuration location
    Uri accountEndPoint = new Uri(Properties.Settings.Default.GlobalDatabaseUri);
    string accountKey = Properties.Settings.Default.GlobalDatabaseKey;

    //Setting read region selection preference 
    connectionPolicy.PreferredLocations.Add(LocationNames.WestUS); // first preference
    connectionPolicy.PreferredLocations.Add(LocationNames.EastUS); // second preference
    connectionPolicy.PreferredLocations.Add(LocationNames.NorthEurope); // third preference

    // initialize connection
    DocumentClient docClient = new DocumentClient(
        accountEndPoint,
        accountKey,
        connectionPolicy);

    // connect to DocDB 
    await docClient.OpenAsync().ConfigureAwait(false);


## <a name="nodejs-javascript-and-python-sdks"></a>NodeJS, JavaScript en Python SDK 's
De SDK kan worden gebruikt zonder codewijzigingen. De SDK wordt in dit geval automatisch rechtstreekse zowel lezen en schrijven naar de huidige schrijven regio. 

In-versie 1,8 en hoger van elke SDK de ConnectionPolicy-parameter voor de constructor DocumentClient een nieuwe eigenschap genoemd DocumentClient.ConnectionPolicy.PreferredLocations. Dit is de parameter is een matrix van reeksen waarmee een lijst met regionamen. De namen zijn opgemaakt per de kolom naam van de regio in de [Regio's Azure]  [ regions] pagina. U kunt ook de vooraf gedefinieerde constanten gebruiken in het gemak object AzureDocuments.Regions

De huidige schrijven en lees eindpunten zijn respectievelijk beschikbaar in DocumentClient.getWriteEndpoint en DocumentClient.getReadEndpoint.

> [AZURE.NOTE] De URL's voor de eindpunten niet beschouwd als lange levensduur constanten. De service, kan deze op elk gewenst moment bijwerken. De SDK is, wordt deze wijziging automatisch verwerken.

Hieronder volgt een codevoorbeeld voor NodeJS/Javascript. Python en Java volgen hetzelfde patroon.

    // Creating a ConnectionPolicy object
    var connectionPolicy = new DocumentBase.ConnectionPolicy();
    
    // Setting read region selection preference, in the following order -
    // 1 - West US
    // 2 - East US
    // 3 - North Europe
    connectionPolicy.PreferredLocations = ['West US', 'East US', 'North Europe'];
    
    // initialize the connection
    var client = new DocumentDBClient(host, { masterKey: masterKey }, connectionPolicy);


## <a name="rest"></a>REST 
Zodra het databaseaccount van een beschikbaar is gesteld in meerdere regio's, kunnen clients query's de beschikbaarheid door een GET-aanvraag op de volgende URI uit te voeren.

    https://{databaseaccount}.documents.azure.com/

De service retourneert een lijst met regio's en hun bijbehorende DocumentDB eindpunt URI's voor de replica's. Het huidige gebied voor schrijven wordt aangegeven in de reactie. De client kunt als volgt het juiste eindpunt voor alle verdere REST API aanvragen selecteren.

Voorbeeld antwoord

    {
        "_dbs": "//dbs/",
        "media": "//media/",
        "writableLocations": [
            {
                "Name": "West US",
                "DatabaseAccountEndpoint": "https://globaldbexample-westus.documents.azure.com:443/"
            }
        ],
        "readableLocations": [
            {
                "Name": "East US",
                "DatabaseAccountEndpoint": "https://globaldbexample-eastus.documents.azure.com:443/"
            }
        ],
        "MaxMediaStorageUsageInMB": 2048,
        "MediaStorageUsageInMB": 0,
        "ConsistencyPolicy": {
            "defaultConsistencyLevel": "Session",
            "maxStalenessPrefix": 100,
            "maxIntervalInSeconds": 5
        },
        "addresses": "//addresses/",
        "id": "globaldbexample",
        "_rid": "globaldbexample.documents.azure.com",
        "_self": "",
        "_ts": 0,
        "_etag": null
    }


-   OPSLAG en boeken en verwijderen aanvragen moeten gaat u naar de opgegeven schrijven URI
-   Alle haalt en andere aanvragen alleen-lezen (bijvoorbeeld query's) mogelijk gaat u naar een eindpunt van de keuze van de klant

Schrijf aanvragen voor alleen-lezen regio's, mislukt met HTTP-foutcode 403 ('niet toegestaan").

Als het gebied schrijven na de eerste discovery-fase van de klant wijzigt, mislukt schrijft de vorige schrijven regio met HTTP-foutcode 403 ('niet toegestaan"). De client moet vervolgens de lijst met regio's opnieuw om de bijgewerkte schrijven regio krijgt.

## <a name="next-steps"></a>Volgende stappen

Meer informatie over de distributie gegevens globaal met DocumentDB in de volgende artikelen:

- [Gegevens globaal met DocumentDB distribueren](documentdb-distribute-data-globally.md)
- [Consistentie niveaus](documentdb-consistency-levels.md)
- [De werking van doorvoer met meerdere regio 's](documentdb-manage.md#how-throughput-works-with-multiple-regions)
- [Regio's met behulp van de Azure portal toevoegen](documentdb-portal-global-replication.md)

[regions]: https://azure.microsoft.com/regions/ 
