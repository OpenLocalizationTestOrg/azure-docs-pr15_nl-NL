<properties
   pageTitle="Algemene Azure-implementatie oplossen | Microsoft Azure"
   description="Beschrijving van het oplossen van veelvoorkomende fouten in wanneer u resources dashboard implementeren naar Azure met Azure Resource Manager."
   services="azure-resource-manager"
   documentationCenter=""
   tags="top-support-issue"
   authors="tfitzmac"
   manager="timlt"
   editor="tysonn"
   keywords="implementatie is opgetreden en azure-implementatie implementeren naar azure"/>

<tags
   ms.service="azure-resource-manager"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="10/24/2016"
   ms.author="tomfitz"/>

# <a name="troubleshoot-common-azure-deployment-errors-with-azure-resource-manager"></a>Algemene oplossen Azure-implementatie met Azure Resource Manager

Dit onderwerp wordt beschreven hoe u sommige gemene kunt oplossen Azure-implementatie-fouten die kunnen optreden. Als u meer informatie over wat er fout gegaan met uw implementatie is nodig hebt, raadpleegt u eerst [de bewerkingen implementatie weergeven](resource-manager-troubleshoot-deployments-portal.md) en keert u terug naar dit artikel voor hulp bij het oplossen van de fout. De foutcode die u ziet een lijst met de secties in dit onderwerp.

## <a name="invalid-template"></a>Ongeldige sjabloon

Deze fout kan resulteren uit verschillende soorten fouten. 

### <a name="syntax-error"></a>Van de syntaxisfout

Als u een foutbericht wordt weergegeven dat de validatie is mislukt sjabloon ontvangt, wellicht u een probleem syntaxis in uw sjabloon. 

    Code=InvalidTemplate 
    Message=Deployment template validation failed

Deze fout zich gemakkelijk omdat sjabloonexpressies ingewikkeld kunnen zijn. De naamtoewijzing voor de volgende voor een account opslag bevat bijvoorbeeld één set met haken, drie functies drie sets met haakjes, één set met enkele aanhalingstekens en één eigenschap:

    "name": "[concat('storage', uniqueString(resourceGroup().id))]",

Als u de syntaxis van de overeenkomende niet opgeeft, wordt in de sjabloon een waarde die is anders dan uw bedoeling levert.

Wanneer u dit type fout ontvangt, zorgvuldig door de expressiesyntaxis van de. Kunt u overwegen een JSON-editor zoals [Visual Studio](vs-azure-tools-resource-groups-deployment-projects-create-deploy.md) of [Visual Studio-Code](resource-manager-vs-code.md)die u over syntaxisfouten waarschuwen kunt. 

### <a name="incorrect-segment-lengths"></a>Onjuiste segment lengte berekend

Een andere ongeldige sjabloon fout treedt op wanneer de naam van de resource niet de juiste indeling is.

    Code=InvalidTemplate
    Message=Deployment template validation failed: 'The template resource {resource-name}' 
    for type {resource-type} has incorrect segment lengths.

Een resource voor het niveau van hoofdmap moet één minder segment van de naam dan in het resourcetype. Elk segment is door een slash onderscheiden. In het volgende voorbeeld wordt het type heeft twee segmenten en de naam heeft één segment, zodat u een **geldige naam**.

    {
      "type": "Microsoft.Web/serverfarms",
      "name": "myHostingPlanName",
      ...
    }

Maar het volgende voorbeeld is **niet de naam van een geldige** omdat er hetzelfde aantal segmenten als het type.

    {
      "type": "Microsoft.Web/serverfarms",
      "name": "appPlan/myHostingPlanName",
      ...
    }

Hebben hetzelfde aantal segmenten voor onderliggende resources, het type en de naam. Dit aantal segmenten relevant is omdat de volledige naam en type voor de onderliggende bevat de naam van bovenliggende en type. De volledige naam heeft daarom nog steeds één minder segment dan het volledige type. 

    "resources": [
        {
            "type": "Microsoft.KeyVault/vaults",
            "name": "contosokeyvault",
            ...
            "resources": [
                {
                    "type": "secrets",
                    "name": "appPassword",
                    ...
                }
            ]
        }
    ]

Aan de segmenten rechts kan lastig met resourcemanager typen die zijn toegepast op resource providers zijn. Bijvoorbeeld, een resource vergrendelen toepassen op een website, is een type met vier segmenten vereist. De naam is er daarom drie segmenten:

    {
        "type": "Microsoft.Web/sites/providers/locks",
        "name": "[concat(variables('siteName'),'/Microsoft.Authorization/MySiteLock')]",
        ...
    }

### <a name="copy-index-is-not-expected"></a>Kopie index wordt niet naar verwachting

Deze fout **InvalidTemplate** optreedt wanneer u het element **kopie** hebt toegepast op een deel van de sjabloon die met dit element niet ondersteunt. U kunt alleen de kopie-element toepassen op een resourcetype. U kunt geen kopie toepassen op een eigenschap binnen een resourcetype. Zoals u kopie toepassen op een virtuele machine, maar u kunt deze niet toepassen op de schijven OS voor een virtuele machine. In sommige gevallen kunt u een resource onderliggende converteren naar een bovenliggende resource om te maken van een lus kopiëren. Zie [meerdere exemplaren van resources in Azure resourcemanager maken](resource-group-create-multiple.md)voor meer informatie over het gebruik van kopiëren.

### <a name="parameter-is-not-valid"></a>Parameter is niet geldig

Als de sjabloon toegestane waarden voor een parameter worden en u een waarde die is niet op een van deze waarden opgeeft, ontvangt u een bericht dat vergelijkbaar is met het volgende foutbericht weergegeven:

    Code=InvalidTemplate; 
    Message=Deployment template validation failed: 'The provided value {parameter vaule} 
    for the template parameter {parameter name} is not valid. The parameter value is not 
    part of the allowed value(s)

Dubbele Controleer de toegestane waarden in de sjabloon en geef een tijdens de implementatie.

## <a name="not-found-or-resource-not-found"></a>Niet gevonden (of resource is niet gevonden)

Wanneer uw sjabloon de naam van een resource die niet worden omgezet bevat, wordt een foutbericht zoals:

    Code=NotFound;
    Message=Cannot find ServerFarm with name exampleplan.

Als u probeert te implementeren van de ontbrekende resource in de sjabloon, controleert u of u moet een afhankelijkheid toevoegen. Resourcemanager optimaliseert implementatie door te maken van resources parallel, indien mogelijk. Als u een resource moet worden geïmplementeerd na een andere resource, moet u het element **dependsOn** gebruiken in uw sjabloon een afhankelijkheid maken op de andere resource. Bijvoorbeeld als u een web-app wordt geïmplementeerd, moet het abonnement dat App aanwezig. Als u hebt opgegeven dat de web-app is afhankelijk van het App-abonnement, resourcemanager Hiermee maakt u beide resources op hetzelfde moment. U foutbericht een waarin staat dat niet de App Service abonnement resource wordt gevonden, omdat deze nog niet bestaat tijdens een poging om het instellen van een eigenschap in de web-app. U kunt deze fout voorkomen door in te stellen van de afhankelijkheid in de WebApp.

    {
      "apiVersion": "2015-08-01",
      "type": "Microsoft.Web/sites",
      ...
      "dependsOn": [
        "[variables('hostingPlanName')]"
      ],
      ...
    }
 
U ziet ook deze fout wanneer de resource in een andere resourcegroep dan de tabel die wordt geïmplementeerd in bestaat. In dat geval de [resourceId functie](resource-group-template-functions.md#resourceid) gebruiken om de volledig gekwalificeerde naam van de resource.

    "properties": {
        "name": "[parameters('siteName')]",
        "serverFarmId": "[resourceId('plangroup', 'Microsoft.Web/serverfarms', parameters('hostingPlanName'))]"
    } 

Als u probeert te gebruiken van de [verwijzing](resource-group-template-functions.md#referenc) of een [listKeys](resource-group-template-functions.md#listkeys) functies met een resource dat kan worden opgelost, ontvangt u het volgende foutbericht weergegeven:

    Code=ResourceNotFound;
    Message=The Resource 'Microsoft.Storage/storageAccounts/{storage name}' under resource 
    group {resource group name} was not found.

Zoekt u een expressie met de functie **verwijzing** . Controleer of de betreffende parameterwaarden juist zijn.

## <a name="storage-account-already-exists-or-already-taken"></a>Opslag account al bestaat (of die u al hebt gemaakt)

U moet een naam voor de resource die uniek is voor alle Azure opgeven voor opslag-accounts. Als u een unieke naam niet opgeeft, wordt een foutbericht zoals:

    Code=StorageAccountAlreadyTaken 
    Message=The storage account named mystorage is already taken.

U kunt een unieke naam voor het samenvoegen van uw naamgevingsconventie met het resultaat van de functie [uniqueString](resource-group-template-functions.md#uniquestring) maken.

    "name": "[concat('contosostorage', uniqueString(resourceGroup().id))]",
    "type": "Microsoft.Storage/storageAccounts",

Als u een account opslagruimte met dezelfde naam als een bestaand account voor de opslag van uw abonnement implementeren, maar een andere locatie biedt, ontvangt u een foutbericht waarin wordt aangegeven dat de opslag-account al bestaat in een andere locatie. Verwijder de bestaande opslag-account of bieden dezelfde locatie als het bestaande opslag-account.

## <a name="account-name-invalid"></a>Ongeldige accountnaam

U ziet de fout **AccountNameInvalid** wanneer u probeert te Geef een naam met een account opslag tekens verboden. Opslag accountnamen moeten tussen 3 en 24 tekens bevatten en gebruiken van getallen en alleen kleine letters.

## <a name="no-registered-provider-found"></a>Geen geregistreerde provider gevonden

Wanneer de resource wordt geïmplementeerd, wordt de volgende foutcode en het bericht:

    Code: NoRegisteredProviderFound
    Message: No registered resource provider found for location {ocation} 
    and API version {api-version} for type {resource-type}.

Dit foutbericht drie oorzaken hebben:

1. Locatie niet ondersteund voor het resourcetype
2. API-versie niet ondersteund voor het resourcetype
3. De resource-provider is niet geregistreerd voor uw abonnement

Het foutbericht geeft u suggesties voor de ondersteunde locaties en API-versies. U kunt uw sjabloon wijzigen in een van de gesuggereerde waarde wordt weergegeven. De meeste providers zijn geregistreerde automatisch door de Azure-portal of de opdrachtregel die u gebruikt, maar niet alle. Als u een bepaalde resource-provider voordat u niet hebt gebruikt, moet u mogelijk registreren die provider. U kunt meer informatie over resource-providers via PowerShell of Azure CLI ontdekken.

### <a name="powershell"></a>PowerShell

Als u wilt uw registratiestatus wordt weergegeven, gebruikt u **Get-AzureRmResourceProvider**.

    Get-AzureRmResourceProvider -ListAvailable

Als u wilt een provider hebt geregistreerd, **Register-AzureRmResourceProvider** gebruiken en geef de naam van de resource-provider die u wilt registreren.

    Register-AzureRmResourceProvider -ProviderNamespace Microsoft.Cdn

Als u de ondersteunde locaties voor een bepaald type resource, gebruikt u:

    ((Get-AzureRmResourceProvider -ProviderNamespace Microsoft.Web).ResourceTypes | Where-Object ResourceTypeName -eq sites).Locations

Als u de ondersteunde API-versies voor een bepaald type resource, gebruikt u:

    ((Get-AzureRmResourceProvider -ProviderNamespace Microsoft.Web).ResourceTypes | Where-Object ResourceTypeName -eq sites).ApiVersions

### <a name="azure-cli"></a>Azure CLI

Als u wilt zien of de provider is geregistreerd, gebruikt u de `azure provider list` opdracht.

    azure provider list

Als u wilt een resource-provider hebt geregistreerd, gebruikt u de `azure provider register` opdracht en geef de *naamruimte* te registreren.

    azure provider register Microsoft.Cdn

Als u wilt zien van de ondersteunde locaties en API-versies voor een resource-provider, gebruiken:

    azure provider show -n Microsoft.Compute --json > compute.json

## <a name="operation-not-allowed"></a>Bewerking is niet toegestaan

U hebt problemen wanneer implementatie groter is dan een quotum, die per resourcegroep, abonnementen, accounts en andere bereiken kan zijn. Bijvoorbeeld: uw abonnement kan worden geconfigureerd Beperk het aantal cores voor een regio. Als u probeert te implementeren van een virtuele machine met meer cores dan de toegestane bedrag, ontvangt u een foutmelding aan dat het quotum is overschreden.
Zie [Azure-abonnement en limieten van de service, quota's en beperkingen](azure-subscription-service-limits.md)voor quotum voor volledige informatie.

Als u wilt onderzoeken van uw abonnement quota voor cores, kunt u de `azure vm list-usage` command in de CLI Azure. Het volgende voorbeeld ziet u dat het quotum voor core voor een gratis proefabonnement-account 4 is:

    azure vm list-usage

Welke geeft als resultaat:

    info:    Executing command vm list-usage
    Location: westus
    data:    Name   Unit   CurrentValue  Limit
    data:    -----  -----  ------------  -----
    data:    Cores  Count  0             4
    info:    vm list-usage command OK

Als u een sjabloon die meer dan vier cores in de regio West Amerikaans maakt implementeert, krijgt u een implementatie-foutbericht die lijkt op:

    Code=OperationNotAllowed
    Message=Operation results in exceeding quota limits of Core. 
    Maximum allowed: 4, Current in use: 4, Additional requested: 2.

Of in PowerShell, kunt u de cmdlet **Get-AzureRmVMUsage** gebruiken.

    Get-AzureRmVMUsage

Welke geeft als resultaat:

    ...
    CurrentValue : 0
    Limit        : 4
    Name         : {
                     "value": "cores",
                     "localizedValue": "Total Regional Cores"
                   }
    Unit         : null
    ...

In deze gevallen moet u Ga naar de portal en een Ondersteuningsprobleem met de als u wilt verhogen van de quota voor de regio waar u naartoe wilt implementeren voor uw bestand.

> [AZURE.NOTE] Denk eraan dat voor de resourcegroepen het quotum voor de afzonderlijke regio's niet voor het hele abonnement. Als u nodig hebt om te implementeren 30 cores in West VS, hebt u om te vragen om 30 resourcemanager cores in West VS. Als u nodig hebt om te implementeren 30 cores in een van de regio's waartoe u toegang hebt, vraagt u 30 resourcemanager cores in alle regio's.

## <a name="invalid-content-link"></a>Koppeling met een ongeldige inhoud

Wanneer u het foutbericht ontvangt:

    Code=InvalidContentLink
    Message=Unable to download deployment content from ...

U hebt waarschijnlijk geprobeerd om te koppelen aan een geneste sjabloon die niet beschikbaar is. Controleer de URI die u voor de geneste sjabloon hebt opgegeven. Als de sjabloon in een opslag-account bestaat, zorg er dan voor dat de URI toegankelijk is. Mogelijk moet u een token SA's doorgeven. Zie [gekoppelde sjablonen met Azure Resource Manager gebruiken](resource-group-linked-templates.md)voor meer informatie.

## <a name="authorization-failed"></a>Verificatie mislukt

U mogelijk een foutbericht ontvangt tijdens de implementatie, omdat de account of service principal poging om het implementeren van de bronnen geen toegang heeft tot deze acties uitvoeren. Azure Active Directory kunt u of uw beheerder om te bepalen welke identiteiten toegang tot welke bronnen in grote mate van precisie van formules. Bijvoorbeeld als uw account is toegewezen aan de rol van de lezer, bent u niet kunnen maken van resources. In dat geval ziet u een foutbericht met de mededeling dat de verificatie is mislukt.

Zie [Toegangsbeheer voor Azure Role-Based](./active-directory/role-based-access-control-configure.md)voor meer informatie over Rolgebaseerd toegangsbeheer.

Naast het Rolgebaseerd toegangsbeheer, kunnen uw implementatie-acties worden beperkt door beleidsregels voor het abonnement. Via het beleid, kan de beheerder conventies voor alle resources die zijn geïmplementeerd in het abonnement afdwingen. Een beheerder kan bijvoorbeeld vereisen dat de waarde van een bepaald label is opgegeven voor een resourcetype. Als u voldoen niet aan de beleidsvereisten, kunt u een foutbericht ontvangt tijdens implementatie. Zie voor meer informatie over het beleid [Beleid voor het beheren van resources en toegang](resource-manager-policy.md).

## <a name="troubleshooting-virtual-machines"></a>Probleemoplossing virtuele machines

| Fout | Artikelen |
| -------- | ----------- |
| Aangepast script extensie fouten | [Windows-VM extensie fouten](./virtual-machines/virtual-machines-windows-extensions-troubleshoot.md)<br />of<br />[Linux VM extensie fouten](./virtual-machines/virtual-machines-linux-extensions-troubleshoot.md) |
| Afbeelding van de OS inrichting van fouten | [Nieuwe Windows VM fouten](./virtual-machines/virtual-machines-windows-troubleshoot-deployment-new-vm.md)<br />of<br />[Nieuwe Linux VM fouten](./virtual-machines/virtual-machines-linux-troubleshoot-deployment-new-vm.md ) |
| Fouten | [Windows-VM fouten](./virtual-machines/virtual-machines-windows-allocation-failure.md)<br />of<br />[Linux VM fouten](./virtual-machines/virtual-machines-linux-allocation-failure.md) |
| Secure Shell (SSH) fouten tijdens een poging om verbinding te maken | [Beveiligde Shell verbindingen voor Linux VM](./virtual-machines/virtual-machines-linux-troubleshoot-ssh-connection.md) |
| Verbinding maken met de toepassing wordt uitgevoerd VM fouten | [Toepassing op Windows VM wordt uitgevoerd](./virtual-machines/virtual-machines-windows-troubleshoot-app-connection.md)<br />of<br />[Toepassing op een VM Linux wordt uitgevoerd](./virtual-machines/virtual-machines-linux-troubleshoot-app-connection.md) |
| Verbinding met extern bureaublad fouten | [Verbindingen met extern bureaublad voor Windows VM](./virtual-machines/virtual-machines-windows-troubleshoot-rdp-connection.md) |
| Fouten opgelost door opnieuw te implementeren | [VM implementeren naar nieuwe Azure knooppunt](./virtual-machines/virtual-machines-windows-redeploy-to-new-node.md) |
| Cloud service fouten | [Cloud-implementatie serviceproblemen](./cloud-services/cloud-services-troubleshoot-deployment-problems.md) |

## <a name="troubleshooting-other-services"></a>Problemen met andere services oplossen

De volgende tabel is niet een volledige lijst met onderwerpen voor probleemoplossing voor Azure. In plaats daarvan de nadruk op met betrekking tot het implementeren of te configureren van resources. Als u hulp bij het oplossen van problemen runtime-resource nodig hebt, raadpleegt u de documentatie van deze Azure-service.

| Service | Artikel |
| -------- | -------- |
| Automatisering | [Tips voor probleemoplossing voor veelvoorkomende fouten in Azure automatisering](./automation/automation-troubleshooting-automation-errors.md) |
| Azure stapel | [Microsoft Azure stapel problemen oplossen](./azure-stack/azure-stack-troubleshooting.md) |
| Azure stapel | [WebApps en Azure stapel](./azure-stack/azure-stack-webapps-troubleshoot-known-issues.md ) |
| Gegevens Factory | [Problemen met gegevens Factory oplossen](./data-factory/data-factory-troubleshoot.md) |
| Service stof | [Veelvoorkomende problemen oplossen van problemen als u services op Azure-Service stof implementeren](./service-fabric/service-fabric-diagnostics-troubleshoot-common-scenarios.md) |
| Herstel van de site | [Controleren en problemen met beveiliging voor virtuele machines en fysieke servers](./site-recovery/site-recovery-monitoring-and-troubleshooting.md) |
| Opslag | [Bewaken, diagnosticeren en problemen met Microsoft Azure Storage](./storage/storage-monitoring-diagnosing-troubleshooting.md) |
| StorSimple | [Problemen met de implementatie van de StorSimple apparaat oplossen](./storsimple/storsimple-troubleshoot-deployment.md) |
| SQL-Database | [Problemen met verbinding oplossen met Azure SQL-Database](./sql-database/sql-database-troubleshoot-common-connection-issues.md) |
| SQL datawarehouse | [Azure SQL datawarehouse probleemoplossing](./sql-data-warehouse/sql-data-warehouse-troubleshoot.md) |

## <a name="understand-when-a-deployment-is-ready"></a>Meer informatie over wanneer een implementatie is gereed

Wanneer alle providers van implementatie succes retourneren, Azure resourcemanager success rapporten in een implementatie. Dit bericht betekent niet per se echter dat uw resourcegroep "actief is en klaar voor uw gebruikers." Een implementatie wellicht bijvoorbeeld upgrades downloaden, wachten op niet-sjabloon resources of complexe scripts installeren. Resourcemanager weet niet wanneer deze taken worden voltooid, omdat ze zijn niet activiteiten die een provider wordt bijgehouden. In deze gevallen kan het enige tijd voordat de resources gereed voor gebruik van de echte zijn zijn. Hierdoor, moet u zou verwachten dat de implementatiestatus is geslaagd enige tijd voordat uw implementatie kan worden gebruikt.

U kunt voorkomen dat Azure bevestiging implementatie, echter door te maken van een aangepast script voor de aangepaste sjabloon. Het script weet hoe u de hele implementatie voor de gereedheid van de hele systeem bewaken en retourneert succes alleen wanneer gebruikers met de volledige implementatie werken kunnen. Als u wilt Zorg ervoor dat is uw extensie het laatst wilt uitvoeren, voert u de eigenschap **dependsOn** in uw sjabloon.

## <a name="next-steps"></a>Volgende stappen

- Zie voor meer informatie over het controleren van acties, [controle bewerkingen met bronbeheer](resource-group-audit.md).
- Zie voor meer informatie over acties om de fouten tijdens de implementatie, [implementatie-bewerkingen weergeven](resource-manager-troubleshoot-deployments-portal.md).
