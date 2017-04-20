<properties 
   pageTitle="Maken van DNS-zones en sets vastleggen in Azure DNS met de SDK .NET | Microsoft Azure" 
   description="Het maken van DNS-zones en record wordt ingesteld in Azure DNS met behulp van de .NET SDK." 
   services="dns" 
   documentationCenter="na" 
   authors="jtuliani" 
   manager="carmonm" 
   editor=""/>

<tags
   ms.service="dns"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services" 
   ms.date="09/19/2016"
   ms.author="jtuliani"/>


# <a name="create-dns-zones-and-record-sets-using-the-net-sdk"></a>DNS-zones en record sets met de SDK .NET maken

U kunt bewerkingen als u wilt maken, verwijderen of DNS-zones, record sets en records bijwerken met behulp van DNS SDK met .NET DNS-beheer bibliotheek kunt automatiseren. Een volledige Visual Studio-project is beschikbaar [hier.](https://www.microsoft.com/en-us/download/details.aspx?id=47268&WT.mc_id=DX_MVP4025064&e6b34bbe-475b-1abd-2c51-b5034bcdd6d2=True)

## <a name="create-a-service-principal-account"></a>Een principal service-account maken

Toegang tot het Azure resources wordt meestal verleend via een speciale rekening in plaats van uw eigen gebruikersreferenties. Deze speciale accounts worden accounts 'service principal' genoemd. Als u wilt gebruiken het project van de steekproef Azure DNS-SDK, moet u eerst een principal service-account maken en de juiste machtigingen toewijzen.

1. Volg [deze instructies](../resource-group-authenticate-service-principal.md) om te maken van een principal serviceaccount (het project van de steekproef Azure DNS-SDK wordt ervan uitgegaan wachtwoord gebaseerde verificatie.)

2. Een resourcegroep maken ([Hier ziet u hoe](../azure-portal/resource-group-portal.md)).

3. Azure RBAC de hoofdsom serviceaccount 'DNS-Zone Inzender' om machtigingen te verlenen aan de resourcegroep gebruiken ([Hier ziet u hoe](../active-directory/role-based-access-control-configure.md).)

4. Als het project van de steekproef Azure DNS-SDK gebruikt, als volgt het 'program.cs'-bestand bewerken:
    * Voeg de juiste waarden voor de tenantId, clientId (ook wel een account-ID), geheim (principal accountwachtwoord service) en subscriptionId zoals gebruikt in stap 1.
    * Voer de naam van de resource-groep gekozen in stap 2.
    * Voer de naam van een DNS-zone van uw keuze.

## <a name="nuget-packages-and-namespace-declarations"></a>NuGet-pakketten en naamruimtedeclaraties

Als u wilt de SDK van Azure DNS .NET gebruiken, moet u het pakket van **Azure DNS-beheer bibliotheek** NuGet en andere vereiste Azure-pakketten installeren.
 
1. Open een project of een nieuw project in **Visual Studio**. 

2. Ga naar **Extra** **>** **NuGet Package Manager** **>** **NuGet-pakketten voor oplossing... beheren**. 

3. Klik op **Bladeren**, activeert u het vakje **voorlopige versie opnemen** en typ **Microsoft.Azure.Management.Dns** in het zoekvak.

4. Selecteer het pakket en klik op **installeren** om deze aan uw project Visual Studio toe.
 
5. Herhaal de bovenstaande ook de volgende pakketten installeren proces: **Microsoft.Rest.ClientRuntime.Azure.Authentication** en **Microsoft.Azure.Management.ResourceManager**.

## <a name="add-namespace-declarations"></a>Naamruimtedeclaraties toevoegen

De volgende naamruimtedeclaraties toevoegen

    using Microsoft.Rest.Azure.Authentication;
    using Microsoft.Azure.Management.Dns;
    using Microsoft.Azure.Management.Dns.Models;

## <a name="initialize-the-dns-management-client"></a>Initialisatie van de DNS-beheer-client

De *DnsManagementClient* bevat de methoden en eigenschappen die nodig zijn voor het beheer van DNS-zones en recordsets.  De volgende code zich aanmelden bij de hoofdsom serviceaccount en maakt u een object DnsManagementClient.

    // Build the service credentials and DNS management client
    var serviceCreds = await ApplicationTokenProvider.LoginSilentAsync(tenantId, clientId, secret);
    var dnsClient = new DnsManagementClient(serviceCreds);
    dnsClient.SubscriptionId = subscriptionId;

## <a name="create-or-update-a-dns-zone"></a>Maken of bijwerken van een DNS-zone

Als u wilt maken van een DNS-zone, wordt eerst een object "Zone" gemaakt voor de DNS-zone parameters bevatten. Omdat de DNS-zones niet zijn gekoppeld aan een bepaalde regio, is de locatie ingesteld op 'global'. In dit voorbeeld wordt een [Azure resourcemanager 'tag'](https://azure.microsoft.com/updates/organize-your-azure-resources-with-tags/) ook toegevoegd aan de zone.

Werkelijk maken of bijwerken van de zone in Azure DNS, wordt het zone-object waarin de zone parameters doorgegeven aan de methode *DnsManagementClient.Zones.CreateOrUpdateAsyc* .

>[AZURE.NOTE] DnsManagementClient ondersteunt bewerking drie manieren: synchroon ('CreateOrUpdate'), asynchroon ('CreateOrUpdateAsync'), of asynchroon met toegang tot het HTTP-antwoord ('CreateOrUpdateWithHttpMessagesAsync').  U kunt een van deze modi, afhankelijk van uw behoeften toepassing.

Azure DNS ondersteunt optimistische gelijktijdigheid, [Etags](dns-getstarted-create-dnszone.md)genoemd. In dit voorbeeld precisie "*" kop geeft voor het 'If-None-Match' Azure DNS voor het maken van een DNS-zone, als er nog niet bestaat.  De oproep mislukt als een zone met de opgegeven naam al in de opgegeven resourcegroep bestaat.

    // Create zone parameters
    var dnsZoneParams = new Zone("global"); // All DNS zones must have location = "global"
    
    // Create a Azure Resource Manager 'tag'.  This is optional.  You can add multiple tags
    dnsZoneParams.Tags = new Dictionary<string, string>();
    dnsZoneParams.Tags.Add("dept", "finance");
    
    // Create the actual zone.
    // Note: Uses 'If-None-Match *' ETAG check, so will fail if the zone exists already.
    // Note: For non-async usage, call dnsClient.Zones.CreateOrUpdate(resourceGroupName, zoneName, dnsZoneParams, null, "*")
    // Note: For getting the http response, call dnsClient.Zones.CreateOrUpdateWithHttpMessagesAsync(resourceGroupName, zoneName, dnsZoneParams, null, "*")
    var dnsZone = await dnsClient.Zones.CreateOrUpdateAsync(resourceGroupName, zoneName, dnsZoneParams, null, "*");

## <a name="create-dns-record-sets-and-records"></a>DNS-record sets en records maken

DNS-records worden beheerd als een recordset. Een recordset is een set records met dezelfde naam en recordtype binnen een zone.  De naam van de recordset is ten opzichte van de naam van de zone, niet de FQDN-naam.

Als u wilt maken of bijwerken van een record instellen, een 'RecordSet"parameters object wordt gemaakt en doorgegeven aan *DnsManagementClient.RecordSets.CreateOrUpdateAsync*. Als u met de DNS-zones, zijn er zijn drie manieren bewerking: synchroon ('CreateOrUpdate'), asynchroon ('CreateOrUpdateAsync'), of asynchroon met toegang tot het HTTP-antwoord ('CreateOrUpdateWithHttpMessagesAsync').

Als u met de DNS-zones, bewerkingen op record sets zijn onder meer ondersteuning voor optimistische gelijktijdigheid.  In dit voorbeeld aangezien 'If-Match' noch 'If-None-Match' zijn opgegeven, de recordset altijd gemaakt.  In dit gesprek overschrijft eventuele bestaande record instellen met dezelfde naam en recordtype in deze DNS-zone.

    // Create record set parameters
    var recordSetParams = new RecordSet();
    recordSetParams.TTL = 3600;

    // Add records to the record set parameter object.  In this case, we'll add a record of type 'A'
    recordSetParams.ARecords = new List<ARecord>();
    recordSetParams.ARecords.Add(new ARecord("1.2.3.4"));

    // Add metadata to the record set.  Similar to Azure Resource Manager tags, this is optional and you can add multiple metadata name/value pairs
    recordSetParams.Metadata = new Dictionary<string, string>();
    recordSetParams.Metadata.Add("user", "Mary");

    // Create the actual record set in Azure DNS
    // Note: no ETAG checks specified, will overwrite existing record set if one exists
    var recordSet = await dnsClient.RecordSets.CreateOrUpdateAsync(resourceGroupName, zoneName, recordSetName, RecordType.A, recordSetParams);

## <a name="get-zones-and-record-sets"></a>Zones opvragen en record sets weergegeven

De methoden *DnsManagementClient.Zones.Get* en *DnsManagementClient.RecordSets.Get* ophalen respectievelijk afzonderlijke zones en record sets. RecordSets worden aangeduid met het type, naam en de zone- en resourcekalenders groep aanwezig zijn in. Zones worden aangeduid met hun naam en de resourcegroep die aanwezig zijn in.

    var recordSet = dnsClient.RecordSets.Get(resourceGroupName, zoneName, recordSetName, RecordType.A);
    
## <a name="update-an-existing-record-set"></a>Een bestaande recordset bijwerken

Als u wilt bijwerken van een bestaande groep van de DNS-record, eerst de recordset ophalen en klik vervolgens de recordset inhoud bijwerken, moet u vervolgens de wijziging dient.  In dit voorbeeld wordt de Etag uit de set opgehaalde record in de parameter If-Match opgeven. De oproep mislukt als een gelijktijdige bewerking de record die is ingesteld in de tussentijd is gewijzigd.

    var recordSet = dnsClient.RecordSets.Get(resourceGroupName, zoneName, recordSetName, RecordType.A);

    // Add a new record to the local object.  Note that records in a record set must be unique/distinct
    recordSet.ARecords.Add(new ARecord("5.6.7.8"));

    // Update the record set in Azure DNS
    // Note: ETAG check specified, update will be rejected if the record set has changed in the meantime
    recordSet = await dnsClient.RecordSets.CreateOrUpdateAsync(resourceGroupName, zoneName, recordSetName, RecordType.A, recordSet, recordSet.Etag);

## <a name="list-zones-and-record-sets"></a>Lijst met zones en record sets weergegeven

Aan de lijst zones, voert u de methoden *DnsManagementClient.Zones.List...* , die ondersteuning vermelding op alle zones in een bepaalde groep of alle zones in een bepaald Azure abonnement (via resourcegroepen.) Als u wilt opnemen sets lijst, *DnsManagementClient.RecordSets.List...* methoden, die ondersteuning bieden voor een lijst met alle record sets in een bepaalde zone of alleen die record sets van een specifiek type gebruiken.

Houd er rekening mee bij weergave van zones en recordsets die het resultaat is mogelijk worden gepagineerd.  Het volgende voorbeeld wordt getoond hoe de pagina's met resultaten doorlopen. (Een kunstmatig kleine paginaformaat van '2' wordt gebruikt om te paginering afdwingen; in Word Web App deze parameter moet worden weggelaten en de grootte van de pagina standaard gebruikt.)

    // Note: in this demo, we'll use a very small page size (2 record sets) to demonstrate paging
    // In practice, to improve performance you would use a large page size or just use the system default
    int recordSets = 0;
    var page = await dnsClient.RecordSets.ListAllInResourceGroupAsync(resourceGroupName, zoneName, "2");
    recordSets += page.Count();

    while (page.NextPageLink != null)
    {
        page = await dnsClient.RecordSets.ListAllInResourceGroupNextAsync(page.NextPageLink);
        recordSets += page.Count();
    }

## <a name="next-steps"></a>Volgende stappen

Download het [Azure DNS .NET SDK steekproef project](https://www.microsoft.com/en-us/download/details.aspx?id=47268&WT.mc_id=DX_MVP4025064&e6b34bbe-475b-1abd-2c51-b5034bcdd6d2=True), waaronder verdere voorbeelden van het gebruik van de Azure DNS .NET SDK, inclusief voorbeelden voor andere DNS-recordtypen.
