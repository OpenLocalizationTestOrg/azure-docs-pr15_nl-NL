<properties 
    pageTitle="Azure PowerShell met resourcemanager | Microsoft Azure" 
    description="Inleiding tot Azure PowerShell gebruiken voor meerdere resources te implementeren als een resourcegroep naar Azure." 
    services="azure-resource-manager" 
    documentationCenter="" 
    authors="tfitzmac" 
    manager="timlt" 
    editor="tysonn"/>

<tags 
    ms.service="azure-resource-manager" 
    ms.workload="multiple" 
    ms.tgt_pltfrm="powershell" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="08/18/2016" 
    ms.author="tomfitz"/>

# <a name="using-azure-powershell-with-azure-resource-manager"></a>Via Azure PowerShell met Azure resourcemanager

> [AZURE.SELECTOR]
- [Portal](azure-portal/resource-group-portal.md) 
- [Azure CLI](xplat-cli-azure-resource-manager.md)
- [Azure PowerShell](powershell-azure-resource-manager.md)
- [REST API](resource-manager-rest-api.md)


Azure resourcemanager implementeert een modern aanpak naar Azure resources levenscyclus van het besturingselement. In plaats van maken en beheren van afzonderlijke resources, begint u door een complete oplossing, zoals een blog, een Fotogalerie, een SharePoint-portal of een wiki imagining. Een sjabloon--een declaratieve weergave van de oplossing--kunt u een resourcegroep waarin alle van de bronnen die u nodig hebt voor de ondersteuning van de oplossing definiëren. Vervolgens implementeren en beheren van die resourcegroep als een logische eenheid. 

In deze zelfstudie leert u hoe u met Azure Resource Manager Azure PowerShell gebruiken. Deze begeleidt u bij het proces van een oplossing implementeert, en werken met deze oplossing. U kunt Azure PowerShell en een sjabloon resourcemanager implementeren:

- SQL server - voor het hosten van de database
- SQL-database - voor de opslag van de gegevens
- Firewallregels - toe te staan dat de web-app verbinding maken met de database
- App serviceplan - voor het definiëren van de mogelijkheden en de kosten van de web-app
- Website - voor het uitvoeren van de web-app
- Web config - voor het opslaan van de verbindingsreeks in de database 
- Waarschuwingsregels - voor prestaties en fouten controleren
- Inzicht in de App - voor schaalinstellingen voor auto

## <a name="prerequisites"></a>Vereisten voor

Als u wilt deze zelfstudie hebt voltooid, hebt u het volgende nodig:

- Een Azure-account
  + U kunt [een Azure-account gratis openen](/pricing/free-trial/?WT.mc_id=A261C142F): U tegoeden krijgt u kunt uitproberen betaalde Azure services en zelfs nadat ze gebruikt afgerond kunt u het account en gebruik vrij te geven Azure services, zoals Websites. Uw creditcard nooit brengt, tenzij u expliciet uw instellingen wijzigen en vragen om te betalen.
  
  + U kunt [MSDN abonnee voordelen activeren](/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A261C142F): uw MSDN-abonnement kunt u tegoeden elke maand die u voor betaalde Azure-services gebruiken kunt.
- Azure PowerShell 1.0. Zie voor informatie over deze release en hoe u het kunt installeren, [het installeren en configureren van Azure PowerShell](powershell-install-configure.md).

Deze zelfstudie is bedoeld voor beginners PowerShell, maar deze wordt ervan uitgegaan dat u de basisbegrippen, zoals modules, cmdlets en sessies kennen.

## <a name="get-help-for-cmdlets"></a>Hulp krijgen voor cmdlets

Als u gedetailleerde help voor elke-cmdlet waarmee u in deze zelfstudie te zien, gebruikt u de cmdlet Get-Help. 

    Get-Help <cmdlet-name> -Detailed

Typ bijvoorbeeld het volgende als u help voor de cmdlet Get-AzureRmResource:

    Get-Help Get-AzureRmResource -Detailed

Als u een lijst met cmdlets in de module Resources met een help-samenvatting, typ: 

    Get-Command -Module AzureRM.Resources | Get-Help | Format-Table Name, Synopsis

De uitvoer ziet er ongeveer het volgende fragment uit:

    Name                                   Synopsis
    ----                                   --------
    Find-AzureRmResource                   Searches for resources using the specified parameters.
    Find-AzureRmResourceGroup              Searches for resource group using the specified parameters.
    Get-AzureRmADGroup                     Filters active directory groups.
    Get-AzureRmADGroupMember               Get a group members.
    ...

Als u volledige help voor een cmdlet, typt u een opdracht met de notatie:

    Get-Help <cmdlet-name> -Full
  
## <a name="login-to-your-azure-account"></a>Meld u aan bij uw Azure-account

Voordat u aan uw oplossing werkt, moet u Meld u aan bij uw account.

Als u wilt aanmelden bij uw Azure-account, gebruikt u de cmdlet **Toevoegen-AzureRmAccount** .

    Add-AzureRmAccount

De cmdlet wordt u gevraagd de aanmeldingsreferenties voor uw Azure-account. Na het aanmelden, downloaden het uw accountinstellingen, zodat ze beschikbaar voor Azure PowerShell zijn. 

De accountinstellingen zijn verlopen, dus moet u deze af en toe te vernieuwen. Als u wilt de accountinstellingen vernieuwen, opnieuw **Toevoegen-AzureRmAccount** uitvoeren. 

>[AZURE.NOTE] De modules resourcemanager toevoegen-AzureRmAccount vereist. Een bestand publiceren instellingen is niet voldoende.     

Als u meer dan één abonnement hebt, vindt u de abonnements-id die u wilt gebruiken voor implementatie met de cmdlet **Set-AzureRmContext** .

    Set-AzureRmContext -SubscriptionID <YourSubscriptionId>

## <a name="create-a-resource-group"></a>Een resourcegroep maken

Voordat de resources aan uw abonnement wordt geïmplementeerd, moet u een resourcegroep die uit de bronnen bestaat. 

Als u wilt een resourcegroep maken, gebruikt u de cmdlet **New-AzureRmResourceGroup** .

De opdracht gebruikt de parameter **naam** een naam voor de resourcegroep en de parameter **locatie** om op te geven van de locatie te geven. Op basis van wat we in de vorige sectie hebt gevonden, we "West VS" gebruiken voor de locatie.

    New-AzureRmResourceGroup -Name TestRG1 -Location "West US"
    
De uitvoer is vergelijkbaar met:

    ResourceGroupName : TestRG1
    Location          : westus
    ProvisioningState : Succeeded
    Tags              :
    ResourceId        : /subscriptions/{guid}/resourceGroups/TestRG1

De resourcegroep is gemaakt.

## <a name="deploy-your-solution"></a>Uw oplossing implementeren

In dit onderwerp wordt u niet hoe u uw sjabloon maken of de structuur van de sjabloon bespreken weergegeven. Zie [Azure resourcemanager Authoring sjablonen](resource-group-authoring-templates.md) en [Resourcemanager sjabloon Stapsgewijze instructies](resource-manager-template-walkthrough.md)voor die gegevens. U kunt de vooraf gedefinieerde sjabloon [inrichten een Web-App met een SQL-Database](https://azure.microsoft.com/documentation/templates/201-web-app-sql-database/) van [Azure Quickstart sjablonen](https://azure.microsoft.com/documentation/templates/)gaat implementeren.

U hebt uw resourcegroep en u hebt uw sjabloon, zodat u bent nu klaar om te implementeren van de infrastructuur gedefinieerd in de sjabloon aan de resourcegroep. U implementeren resources met de cmdlet **New-AzureRmResourceGroupDeployment** . De sjabloon bevat veel standaardwaarden, die we gebruiken zodat u niet hoeft te bieden waarden voor deze parameters. De syntaxis van de eenvoudige ziet er zo:

    New-AzureRmResourceGroupDeployment -ResourceGroupName TestRG1 -administratorLogin exampleadmin -TemplateUri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/201-web-app-sql-database/azuredeploy.json 

U geeft de resourcegroep en de locatie van de sjabloon. Als de sjabloon een lokaal bestand is, kunt u de parameter **- Sjabloonbestand** gebruiken en geef het pad naar de sjabloon. U kunt instellen dat de **-modus** -parameter voor **Stapsgewijze** of **voltooid**. Standaard voert resourcemanager u een incrementele update tijdens de implementatie; daarom niet essentiële om in te stellen **-modus** waarop **incrementele**. Als u wilt weten over de verschillen tussen deze implementatiemodi, raadpleegt u [een toepassing met Azure resourcemanager sjabloon Deploy](resource-group-template-deploy.md). 

###<a name="dynamic-template-parameters"></a>Dynamische Sjabloonparameters

Als u bekend met PowerShell bent, weet u dat u door de beschikbare parameters voor een cmdlet bladeren kunt door een minteken (-) te typen en vervolgens op de TAB-toets te drukken. Deze functionaliteit werkt ook met parameters die u in uw sjabloon definieert. Zodra u de sjabloonnaam te typen, de cmdlet ophaalt van de sjabloon parseert u deze en wordt de Sjabloonparameters aan de opdracht dynamisch. Dit kunt u heel gemakkelijk kunt u de sjabloon parameterwaarden opgeven.

Wanneer u de opdracht invoert, wordt u gevraagd om het ontbrekende parameter is vereist, **administratorLoginPassword**. En, wanneer u het wachtwoord typt, de secure tekenreekswaarde wordt verborgen. Deze strategie sluit u het risico aan te bieden een wachtwoord in tekst zonder opmaak.

    cmdlet New-AzureRmResourceGroupDeployment at command pipeline position 1
    Supply values for the following parameters:
    (Type !? for Help.)
    administratorLoginPassword: ********

Als de sjabloon bevat een parameter met een naam die overeenkomt met een van de parameters in de opdracht om te implementeren van de sjabloon (zoals een parameter met de naam **ResourceGroupName** in de sjabloon die hetzelfde als de parameter **ResourceGroupName** in de cmdlet [New-AzureRmResourceGroupDeployment is](https://msdn.microsoft.com/library/azure/mt679003.aspx) inclusief), wordt u gevraagd om een waarde voor een parameter met de postfix **FromTemplate** (zoals **ResourceGroupNameFromTemplate**). In het algemeen, voorkomt u deze verwarring door de naam van parameters niet met dezelfde naam als parameters gebruikt voor implementatie bewerkingen.

De opdracht wordt uitgevoerd en geeft als resultaat berichten, terwijl de resources worden gemaakt. Uiteindelijk, raadpleegt u het resultaat van de implementatie.

    DeploymentName    : azuredeploy
    ResourceGroupName : TestRG1
    ProvisioningState : Succeeded
    Timestamp         : 4/11/2016 7:26:11 PM
    Mode              : Incremental
    TemplateLink      :
                Uri            : https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/201-web-app-sql-database/azuredeploy.json
                ContentVersion : 1.0.0.0
    Parameters        :
                Name             Type                       Value
                ===============  =========================  ==========
                skuName          String                     F1
                skuCapacity      Int                        1
                administratorLogin  String                  exampleadmin
                administratorLoginPassword  SecureString
                databaseName     String                     sampledb
                collation        String                     SQL_Latin1_General_CP1_CI_AS
                edition          String                     Basic
                maxSizeBytes     String                     1073741824
                requestedServiceObjectiveName  String       Basic

    Outputs           :
                Name             Type                       Value
                ===============  =========================  ==========
                siteUri          String                     websites5wdai7p2k2g4.azurewebsites.net
                sqlSvrFqdn       String                     sqlservers5wdai7p2k2g4.database.windows.net
                    
    DeploymentDebugLogLevel :

In slechts een paar stappen we hebben gemaakt en de resources die zijn vereist voor een complexe website geïmplementeerd. 

### <a name="log-debug-information"></a>Logboekgegevens voor foutopsporing

Wanneer u een sjabloon implementeert, kunt u meer informatie over de vergaderverzoeken en antwoorden kunt vastleggen door op te geven van de parameter **- DeploymentDebugLogLevel** als **Nieuw AzureRmResourceGroupDeployment**actief. Deze informatie kunt u synchronisatiefouten oplossen voor implementatie. De standaardwaarde is **geen** wat betekent er geen aanvraag dat of antwoord inhoud zijn vastgelegd. Logboekregistratie van de inhoud van de aanvraag, antwoord of beide, kunt u opgeven.  Zie voor meer informatie over probleemoplossing implementaties en vastleggen van gegevens voor foutopsporing, [Probleemoplossing resource groep implementaties met Azure PowerShell](resource-manager-troubleshoot-deployments-powershell.md). Het volgende voorbeeld Logboeken de inhoud van de aanvraag en reactie inhoud voor de implementatie.

    New-AzureRmResourceGroupDeployment -ResourceGroupName TestRG1 -DeploymentDebugLogLevel All -TemplateUri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/201-web-app-sql-database/azuredeploy.json 

> [AZURE.NOTE] Wanneer u de parameter DeploymentDebugLogLevel, Overweeg zorgvuldig het type gegevens dat u tijdens de implementatie doorgeeft. Door logboekregistratie informatie over de aanvraag of antwoord waarbij u potentieel gevoelige gegevens die zijn opgehaald door de implementatie-bewerkingen. 


## <a name="get-information-about-your-resource-groups"></a>Informatie over uw resourcegroepen

Nadat een resourcegroep is gemaakt, kunt u de cmdlets in de module resourcemanager voor het beheren van uw resourcegroepen.

- Als u een resourcegroep in uw abonnement, gebruikt u de cmdlet **Get-AzureRmResourceGroup** :

        Get-AzureRmResourceGroup -ResourceGroupName TestRG1
    
    Welke geeft als resultaat de volgende informatie:

        ResourceGroupName : TestRG1
        Location          : westus
        ProvisioningState : Succeeded
        Tags              :
        ResourceId        : /subscriptions/{guid}/resourceGroups/TestRG
        
        ...

    Als u de naam van een resource-groep niet opgeeft, resultaat de cmdlet van de resourcegroepen in uw abonnement.

- Als u de resources in de resourcegroep, gebruikt u de cmdlet **Zoeken-AzureRmResource** en de bijbehorende **ResourceGroupNameContains** -parameter. Zonder parameters krijgt zoeken-AzureRmResource alle resources in uw Azure-abonnement.

        Find-AzureRmResource -ResourceGroupNameContains TestRG1
    
     Welke geeft als resultaat een lijst met bronnen indeling zoals:
        
        Name              : sqlservers5wdai7p2k2g4
        ResourceId        : /subscriptions/{guid}/resourceGroups/TestRG1/providers/Microsoft.Sql/servers/sqlservers5wdai7p2k2g4
        ResourceName      : sqlservers5wdai7p2k2g4
        ResourceType      : Microsoft.Sql/servers
        Kind              : v2.0
        ResourceGroupName : TestRG1
        Location          : westus
        SubscriptionId    : {guid}
        Tags              : {System.Collections.Hashtable}
        ...
            
- U kunt labels logisch organiseren van de resources in uw abonnement en resources met de **Zoeken-AzureRmResource** en **Zoeken-AzureRmResourceGroup** cmdlets ophalen.

        Find-AzureRmResource -TagName displayName -TagValue Website

        Name              : webSites5wdai7p2k2g4
        ResourceId        : /subscriptions/{guid}/resourceGroups/TestRG1/providers/Microsoft.Web/sites/webSites5wdai7p2k2g4
        ResourceName      : webSites5wdai7p2k2g4
        ResourceType      : Microsoft.Web/sites
        ResourceGroupName : TestRG1
        Location          : westus
        SubscriptionId    : {guid}
                
      There is much more you can do with tags. For more information, see [Using tags to organize your Azure resources](resource-group-using-tags.md).

## <a name="add-to-a-resource-group"></a>Toevoegen aan een resourcegroep

Als u wilt een resource toevoegen aan de resourcegroep, kunt u de cmdlet **New-AzureRmResource** . Echter een resource toevoegt zo mogelijk toekomstige verwarring omdat de nieuwe resource niet in uw sjabloon bestaat. Als u de oude sjabloon opnieuw geïmplementeerd, zou u een onvoltooide-oplossing implementeert. Als u vaak implementeert, vindt u deze naar de nieuwe resource toevoegen aan uw sjabloon en deze opnieuw te implementeren begrip.

## <a name="move-a-resource"></a>Een resource verplaatsen

U kunt bestaande resources verplaatsen naar een nieuwe resourcegroep. Zie voor voorbeelden [Resources verplaatsen naar nieuwe resourcegroep-abonnement](resource-group-move-resources.md).

## <a name="export-template"></a>Sjabloon exporteren

Voor een bestaande resourcegroep (geïmplementeerd via PowerShell of een van de andere methoden zoals de portal), kunt u de sjabloon resourcemanager voor de resourcegroep weergeven. De sjabloon exporteren biedt twee voordelen:

1. Omdat alle de infrastructuur van is gedefinieerd in de sjabloon, kunt u eenvoudig toekomstige implementaties van de oplossing automatiseren.

2. U kunt vertrouwd te raken met de sjabloonsyntaxis van de door te zoeken op de JavaScript Object notatie (JSON) die uw oplossing vertegenwoordigt.

Via de PowerShell, kunt u genereren van een sjabloon die de huidige status van uw resourcegroep vertegenwoordigt of ophalen van de sjabloon die is gebruikt voor een bepaalde implementatie.

Exporteren van de sjabloon voor een resourcegroep is handig als u wijzigingen hebt aangebracht in een resourcegroep, en moet de JSON-weergave van de huidige status ophalen. De gegenereerde sjabloon bevat echter alleen een minimum aantal parameters en er geen variabelen. De meeste van de waarden in de sjabloon zijn hard gecodeerde. Voordat de gegenereerde sjabloon wordt geïmplementeerd, wilt u mogelijk meer van de waarden converteren naar parameters, zodat u de implementatie van verschillende omgevingen kunt aanpassen.

Exporteren van de sjabloon voor een bepaalde implementatie is handig wanneer u de werkelijke sjabloon die is gebruikt moet voor het implementeren van resources weergeven. De sjabloon bevat alle parameters en variabelen die is gedefinieerd voor de oorspronkelijke implementatie. Echter als iemand in uw organisatie wijzigingen heeft aangebracht in de resourcegroep buiten de waarde die is gedefinieerd in de sjabloon, deze sjabloon niet staat voor de huidige status van het resourceveld groep.

> [AZURE.NOTE] De functie van de sjabloon exporteren in de proefversie van is en niet alle typen momenteel ondersteund exporteren van een sjabloon. Tijdens een poging om het exporteren van een sjabloon, ziet u mogelijk een fout die aangeeft dat enkele bronnen zijn niet geëxporteerd. Indien nodig kunt u deze resources handmatig definiëren in uw sjabloon na downloaden.

###<a name="export-template-from-resource-group"></a>Sjabloon van resourcegroep exporteren

Als u wilt weergeven op de sjabloon voor een resourcegroep, voert u de cmdlet **Exporteren-AzureRmResourceGroup** .

    Export-AzureRmResourceGroup -ResourceGroupName TestRG1 -Path c:\Azure\Templates\Downloads\TestRG1.json
    
###<a name="download-template-from-deployment"></a>Sjabloon downloaden van implementatie

Als u wilt downloaden van de gebruikte sjabloon voor een bepaalde implementatie, voert u de cmdlet **Opslaan-AzureRmResourceGroupDeploymentTemplate** .

    Save-AzureRmResourceGroupDeploymentTemplate -DeploymentName azuredeploy -ResourceGroupName TestRG1 -Path c:\Azure\Templates\Downloads\azuredeploy.json

## <a name="delete-resources-or-resource-group"></a>Resources of resourcegroep verwijderen

- Als een resource uit de resourcegroep wilt verwijderen, gebruikt u de cmdlet **Verwijderen-AzureRmResource** . Deze cmdlet Hiermee verwijdert u de resource, maar de resourcegroep echter niet worden verwijderd.

    Deze opdracht Hiermee verwijdert u de website TestSite uit de resourcegroep TestRG1.

        Remove-AzureRmResource -Name TestSite -ResourceGroupName TestRG1 -ResourceType "Microsoft.Web/sites" -ApiVersion 2015-08-01

- Een als resourcegroep wilt verwijderen, gebruikt u de cmdlet **Verwijderen-AzureRmResourceGroup** . Deze cmdlet Hiermee verwijdert u de resourcegroep en de bijbehorende bronnen.

        Remove-AzureRmResourceGroup -Name TestRG1
        
    U wordt gevraagd het verwijderen te bevestigen.
        
        Confirm
        Are you sure you want to remove resource group 'TestRG1'
        [Y] Yes  [N] No  [S] Suspend  [?] Help (default is "Y"): Y

## <a name="deployment-script"></a>Implementatiescript

De eerdere implementatie voorbeelden in dit onderwerp alleen de afzonderlijke cmdlets weergegeven die nodig zijn voor het implementeren van resources op Azure. Het volgende voorbeeld ziet u een implementatiescript waarbij de resourcegroep gemaakt en implementeren van de resources.

    <#
      .SYNOPSIS
      Deploys a template to Azure
      
      .DESCRIPTION
      Deploys an Azure Resource Manager template

      .PARAMETER subscriptionId
      The subscription id where the template will be deployed.

      .PARAMETER resourceGroupName
      The resource group where the template will be deployed. Can be the name of an existing or a new resource group.

      .PARAMETER resourceGroupLocation
      Optional, a resource group location. If specified, will try to create a new resource group in this location. If not specified, assumes resource group is existing.

      .PARAMETER deploymentName
      The deployment name.

      .PARAMETER templateFilePath
      Optional, path to the template file. Defaults to template.json.

      .PARAMETER parametersFilePath
      Optional, path to the parameters file. Defaults to parameters.json. If file is not found, will prompt for parameter values based on template.
    #>

    param(
      [Parameter(Mandatory=$True)]
      [string]
      $subscriptionId,

      [Parameter(Mandatory=$True)]
      [string]
      $resourceGroupName,

      [string]
      $resourceGroupLocation,

      [Parameter(Mandatory=$True)]
      [string]
      $deploymentName,

      [string]
      $templateFilePath = "template.json",

      [string]
      $parametersFilePath = "parameters.json"
    )

    #******************************************************************************
    # Script body
    # Execution begins here 
    #******************************************************************************
    $ErrorActionPreference = "Stop"

    # sign in
    Write-Host "Logging in...";
    Add-AzureRmAccount;

    # select subscription
    Write-Host "Selecting subscription '$subscriptionId'";
    Set-AzureRmContext -SubscriptionID $subscriptionId;

    #Create or check for existing resource group
    $resourceGroup = Get-AzureRmResourceGroup -Name $resourceGroupName -ErrorAction SilentlyContinue
    if(!$resourceGroup)
    {
      Write-Host "Resource group '$resourceGroupName' does not exist. To create a new resource group, please enter a location.";
      if(!$resourceGroupLocation) {
        $resourceGroupLocation = Read-Host "resourceGroupLocation";
      }
      Write-Host "Creating resource group '$resourceGroupName' in location '$resourceGroupLocation'";
      New-AzureRmResourceGroup -Name $resourceGroupName -Location $resourceGroupLocation
    }
    else{
      Write-Host "Using existing resource group '$resourceGroupName'";
    }

    # Start the deployment
    Write-Host "Starting deployment...";
    if(Test-Path $parametersFilePath) {
      New-AzureRmResourceGroupDeployment -ResourceGroupName $resourceGroupName -TemplateFile $templateFilePath -TemplateParameterFile $parametersFilePath;
    } else {
      New-AzureRmResourceGroupDeployment -ResourceGroupName $resourceGroupName -TemplateFile $templateFilePath;
    }

## <a name="next-steps"></a>Volgende stappen

- Zie voor meer informatie over het maken van sjablonen van de Resource Manager, [Authoring Azure resourcemanager sjablonen](./resource-group-authoring-templates.md).
- Meer informatie over het implementeren van sjablonen, raadpleegt u [een toepassing met Azure resourcemanager sjabloon Deploy](./resource-group-template-deploy.md).
- Zie voor een gedetailleerd voorbeeld van de implementatie van een project, [Deploy microservices volgens plan in Azure wordt aangegeven](app-service-web/app-service-deploy-complex-application-predictably.md).
- Zie voor meer informatie over het oplossen van een distributie die is mislukt, [Probleemoplossing resource groep implementaties in Azure wordt aangegeven](./resource-manager-troubleshoot-deployments-powershell.md).

