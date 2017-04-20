<properties
   pageTitle="Resources met Azure CLI en sjabloon implementeren | Microsoft Azure"
   description="Azure resourcemanager en Azure CLI gebruiken om te implementeren van een resources naar Azure. De resources worden gedefinieerd in een sjabloon resourcemanager."
   services="azure-resource-manager"
   documentationCenter="na"
   authors="tfitzmac"
   manager="timlt"
   editor="tysonn"/>

<tags
   ms.service="azure-resource-manager"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="08/15/2016"
   ms.author="tomfitz"/>

# <a name="deploy-resources-with-resource-manager-templates-and-azure-cli"></a>Resources met resourcemanager sjablonen en Azure CLI implementeren

> [AZURE.SELECTOR]
- [PowerShell](resource-group-template-deploy.md)
- [Azure CLI](resource-group-template-deploy-cli.md)
- [Portal](resource-group-template-deploy-portal.md)
- [REST API](resource-group-template-deploy-rest.md)

Dit onderwerp wordt uitgelegd hoe u uw resources implementeren naar Azure met Azure CLI met resourcemanager sjablonen.  

> [AZURE.TIP] Voor hulp bij het oplossen van een fout fouten tijdens de implementatie, raadpleegt u:
>
> - [Implementatie-bewerkingen met Azure CLI weergeven](resource-manager-troubleshoot-deployments-cli.md) voor meer informatie over het aanvragen van informatie waarmee u oplossen de fout
> - [Problemen oplossen met veelvoorkomende fouten bij de implementatie van Azure met Azure resourcemanager-informatiebronnen](resource-manager-common-deployment-errors.md) voor meer informatie over het oplossen van veelvoorkomende implementatiefouten in

De sjabloon kan een lokaal bestand of een extern bestand die beschikbaar zijn via een URI zijn. Wanneer uw sjabloon bevindt zich in een opslag-account, kunt u het beperken van toegang aan de sjabloon en een gedeelde toegangstoken handtekening (SA's) tijdens implementatie verstrekken.

## <a name="quick-steps-to-deployment"></a>Snelle stappen voor implementatie

In dit artikel worden alle verschillende opties die beschikbaar zijn voor u tijdens de implementatie. Echter hoeft vaak u alleen twee eenvoudige opdrachten. Als u wilt snel aan de slag met implementatie, gebruikt u de volgende opdrachten:

    azure group create -n ExampleResourceGroup -l "West US"
    azure group deployment create -f <PathToTemplate> -e <PathToParameterFile> -g ExampleResourceGroup -n ExampleDeployment

Meer informatie over opties voor implementatie die mogelijk geschikter uw scenario, gaat u verder lezen in dit artikel.

[AZURE.INCLUDE [resource-manager-deployments](../includes/resource-manager-deployments.md)]

## <a name="deploy-with-azure-cli"></a>Met Azure CLI implementeren

Als u niet eerder hebt gebruikt Azure CLI met resourcemanager, leest u het [gebruik van de Azure CLI voor Mac, Linux, en Windows met Azure Resource Management](xplat-cli-azure-resource-manager.md).

1. Meld u aan bij uw Azure-account. Na het opgeven van uw referenties, resultaat de opdracht het van uw aanmelding.

        azure login
  
        ...
        info:    login command OK

2. Als u meerdere abonnementen hebt, vindt u de abonnements-id die u wilt gebruiken voor implementatie.

        azure account set <YourSubscriptionNameOrId>

3. Ga naar de resourcemanager Azure-module. U ontvangt een bevestiging van de nieuwe modus.

        azure config mode arm
   
        info:     New mode is arm

4. Als u een bestaande resourcegroep niet hebt, kunt u een resourcegroep maken. Geef de naam van de resourcegroep en de locatie die u nodig voor uw oplossing hebt. U moet een locatie voor de resourcegroep omdat de resourcegroep metagegevens over de resources worden opgeslagen. Voor naleving redenen wilt u mogelijk opgeven waarin die metagegevens is opgeslagen. In het algemeen, is het raadzaam dat u opgeeft dat een locatie waar de meeste uw resources zich bevinden. Het gebruik van dezelfde locatie, kan uw sjabloon vereenvoudigen.

        azure group create -n ExampleResourceGroup -l "West US"

     Een overzicht van de nieuwe resourcegroep wordt geretourneerd.
   
        info:    Executing command group create
        + Getting resource group ExampleResourceGroup
        + Creating resource group ExampleResourceGroup
        info:    Created resource group ExampleResourceGroup
        data:    Id:                  /subscriptions/####/resourceGroups/ExampleResourceGroup
        data:    Name:                ExampleResourceGroup
        data:    Location:            westus
        data:    Provisioning State:  Succeeded
        data:    Tags:
        data:
        info:    group create command OK

5. Valideer de implementatie voordat deze wordt uitgevoerd door de opdracht **azure groep sjabloon valideren** uit te voeren. Wanneer u test de implementatie, bieden u parameters exact in zoals u zou doen bij het uitvoeren van de implementatie (weergegeven in de volgende stap).

        azure group template validate -f <PathToTemplate> -p "{\"ParameterName\":{\"value\":\"ParameterValue\"}}" -g ExampleResourceGroup

5. Als u wilt implementeren resources aan de resourcegroep, voer de volgende opdracht en geef de benodigde parameters. De parameters bevatten een naam voor de implementatie, de naam van uw resourcegroep, het pad of de URL naar de sjabloon en alle andere parameters die u nodig hebt voor uw scenario. 
   
     Hebt u de volgende drie opties voor het leveren van parameterwaarden: 

     1. Inline-parameters gebruiken en een lokale sjabloon. Elke parameter is in de indeling: `"ParameterName": { "value": "ParameterValue" }`. Het volgende voorbeeld ziet u de parameters met escape-tekens.

            azure group deployment create -f <PathToTemplate> -p "{\"ParameterName\":{\"value\":\"ParameterValue\"}}" -g ExampleResourceGroup -n ExampleDeployment

     2. Inline-parameters gebruiken en een koppeling naar een sjabloon.

            azure group deployment create --template-uri <LinkToTemplate> -p "{\"ParameterName\":{\"value\":\"ParameterValue\"}}" -g ExampleResourceGroup -n ExampleDeployment

     3. Gebruik een parameterbestand. Zie voor informatie over het sjabloonbestand [Parameter-bestand](#parameter-file).
    
            azure group deployment create -f <PathToTemplate> -e <PathToParameterFile> -g ExampleResourceGroup -n ExampleDeployment

     Nadat de resources zijn geïmplementeerd via een van de bovenstaande drie methoden, ziet u een overzicht van de implementatie.
  
        info:    Executing command group deployment create
        + Initializing template configurations and parameters
        + Creating a deployment
        ...
        info:    group deployment create command OK

     Als u wilt uitvoeren op een volledige implementatie, stel **modus** op **voltooid**. Wees voorzichtig met het gebruik van deze modus kunt u per ongeluk bronnen die zich niet in uw sjabloon verwijderen.

        azure group deployment create --mode Complete -f <PathToTemplate> -e <PathToParameterFile> -g ExampleResourceGroup -n ExampleDeployment

6. Als u vastleggen aanvullende informatie over de implementatie waarmee u eventuele fouten corrigeren implementatie wilt, voert u de parameter **Foutopsporing-instelling** . U kunt opgeven dat de inhoud van de aanvraag, antwoord van inhoud of beide worden vastgelegd met de implementatie-bewerking.

        azure group deployment create --debug-setting All -f <PathToTemplate> -e <PathToParameterFile> -g ExampleResourceGroup -n ExampleDeployment

## <a name="deploy-template-from-storage-with-sas-token"></a>Sjabloon van opslagruimte met SA's token implementeren

U kunt uw sjablonen toevoegen aan een opslag-account en een koppeling naar deze tijdens de implementatie met een token SA's.

> [AZURE.IMPORTANT] Door de onderstaande stappen, is de blob met de sjabloon die toegankelijk zijn voor alleen de eigenaar van het account. De blob is echter toegankelijk voor iedereen met die URI wanneer u een token SA's voor de blob maakt. Als u een andere gebruiker onderschept de URI, kan die gebruiker voor toegang tot de sjabloon. Een token SA's is een goede manier voor het beperken van toegang tot uw sjablonen, maar u moet gevoelige gegevens, zoals wachtwoorden niet rechtstreeks in de sjabloon opnemen.

### <a name="add-private-template-to-storage-account"></a>Persoonlijke sjabloon toevoegen aan opslag-account

De volgende stappen uit een account opslag voor sjablonen instellen:

1. Een resourcegroep maken.

        azure group create -n "ManageGroup" -l "westus"

2. Maak een account opslag. De naam van het opslag-account moet uniek zijn voor Azure, dus Geef uw eigen naam op voor het account.

        azure storage account create -g ManageGroup -l "westus" --sku-name LRS --kind Storage storagecontosotemplates

3. Variabelen voor de opslag-account en de sleutel instellen.

        export AZURE_STORAGE_ACCOUNT=storagecontosotemplates
        export AZURE_STORAGE_ACCESS_KEY={storage_account_key}

4. Een container maken. De machtiging is ingesteld op **uit** wat betekent dat de container is alleen beschikbaar voor de eigenaar bent.

        azure storage container create --container templates -p Off 
        
4. De sjabloon toevoegen aan de container.

        azure storage blob upload --container templates -f c:\Azure\Templates\azuredeploy.json
        
### <a name="provide-sas-token-during-deployment"></a>Token SA's bieden tijdens de implementatie

Als u wilt implementeren een persoonlijke sjabloon in een opslag-account, een token SA's ophalen en deze opnemen in de URI voor de sjabloon.

1. Maak een token SA's met leesmachtigingen en een verlooptijd om toegang te beperken. Stel de verlooptijd toe te staan dat voldoende tijd in beslag de implementatie. Hiermee kunt u de volledige URI van de sjabloon die het token SA's inclusief ophalen.

        expiretime=$(date -I'minutes' --date "+30 minutes")
        fullurl=$(azure storage blob sas create --container templates --blob azuredeploy.json --permissions r --expiry $expiretimetime --json  | jq ".url")

2. De sjabloon implementeren doordat de URI die het token SA's bevat.

        azure group deployment create --template-uri $fullurl -g ExampleResourceGroup

Zie de [gekoppelde sjablonen met Azure Resource Manager gebruiken](resource-group-linked-templates.md)voor een voorbeeld van het gebruik van een token SA's met gekoppelde sjablonen.

[AZURE.INCLUDE [resource-manager-parameter-file](../includes/resource-manager-parameter-file.md)]

## <a name="next-steps"></a>Volgende stappen
- Zie [Deploy resources gebruik .NET-bibliotheken en een sjabloon](virtual-machines/virtual-machines-windows-csharp-template.md)voor een voorbeeld van de implementatie van bronnen via de bibliotheek .NET-client.
- Als u wilt definiëren parameters in een sjabloon, raadpleegt u [ontwerpfuncties sjablonen](resource-group-authoring-templates.md#parameters).
- Zie voor hulp bij uw-oplossing implementeert in verschillende omgevingen, [ontwikkeling en testomgevingen in Microsoft Azure wordt aangegeven](solution-dev-test-environments.md).
- Voor meer informatie over het gebruik van een verwijzing KeyVault voor geven secure waarden, raadpleegt u [geven secure waarden tijdens de implementatie](resource-manager-keyvault-parameter.md).

