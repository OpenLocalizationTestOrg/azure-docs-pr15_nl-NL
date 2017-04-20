<properties
   pageTitle="Resources met PowerShell en sjabloon implementeren | Microsoft Azure"
   description="Azure resourcemanager en Azure PowerShell gebruiken om te implementeren van een resources naar Azure. De resources worden gedefinieerd in een sjabloon resourcemanager."
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

# <a name="deploy-resources-with-resource-manager-templates-and-azure-powershell"></a>Resources met resourcemanager sjablonen en Azure PowerShell implementeren

> [AZURE.SELECTOR]
- [PowerShell](resource-group-template-deploy.md)
- [Azure CLI](resource-group-template-deploy-cli.md)
- [Portal](resource-group-template-deploy-portal.md)
- [REST API](resource-group-template-deploy-rest.md)

Dit onderwerp wordt uitgelegd hoe u uw resources implementeren naar Azure met Azure PowerShell met resourcemanager sjablonen.  

> [AZURE.TIP] Voor hulp bij het oplossen van een fout fouten tijdens de implementatie, raadpleegt u:
>
> - [Implementatie-bewerkingen met Azure PowerShell weergeven](resource-manager-troubleshoot-deployments-powershell.md) voor meer informatie over het aanvragen van informatie waarmee u oplossen de fout
> - [Problemen oplossen met veelvoorkomende fouten bij de implementatie van Azure met Azure resourcemanager-informatiebronnen](resource-manager-common-deployment-errors.md) voor meer informatie over het oplossen van veelvoorkomende implementatiefouten in

De sjabloon kan een lokaal bestand of een extern bestand die beschikbaar zijn via een URI zijn. Wanneer uw sjabloon bevindt zich in een opslag-account, kunt u het beperken van toegang aan de sjabloon en een gedeelde toegangstoken handtekening (SA's) tijdens implementatie verstrekken.

## <a name="quick-steps-to-deployment"></a>Snelle stappen voor implementatie

In dit artikel worden alle verschillende opties die beschikbaar zijn voor u tijdens de implementatie. Echter hoeft vaak u alleen twee eenvoudige opdrachten. Als u wilt snel aan de slag met implementatie, gebruikt u de volgende opdrachten:

    New-AzureRmResourceGroup -Name ExampleResourceGroup -Location "West US"
    New-AzureRmResourceGroupDeployment -Name ExampleDeployment -ResourceGroupName ExampleResourceGroup -TemplateFile <PathToTemplate> -TemplateParameterFile <PathToParameterFile>

Meer informatie over opties voor implementatie die mogelijk geschikter uw scenario, gaat u verder lezen in dit artikel.

[AZURE.INCLUDE [resource-manager-deployments](../includes/resource-manager-deployments.md)]

## <a name="deploy-with-powershell"></a>Met PowerShell implementeren

1. Meld u aan bij uw Azure-account.

        Add-AzureRmAccount

     Een overzicht van uw account wordt geretourneerd.

        Environment : AzureCloud
        Account     : someone@example.com
        ...

2. Als u meerdere abonnementen hebt, vindt u de abonnements-ID die u wilt gebruiken voor implementatie met de opdracht **Set-AzureRmContext** . 

        Set-AzureRmContext -SubscriptionID <YourSubscriptionId>

3. Meestal wanneer een nieuwe sjabloon wordt geïmplementeerd, wilt u maken van een resourcegroep aanduiding van de resources. Als u een bestaande resourcegroep die u implementeren wilt naar hebt, kunt u deze stap overslaan en die resourcegroep gebruiken. 

     Als u wilt een resourcegroep maken, Geef een naam en locatie voor de resourcegroep. U moet een locatie voor de resourcegroep omdat de resourcegroep metagegevens over de resources worden opgeslagen. Voor naleving redenen wilt u mogelijk opgeven waarin die metagegevens is opgeslagen. In het algemeen, is het raadzaam dat u opgeeft dat een locatie waar de meeste uw resources zich bevinden. Het gebruik van dezelfde locatie, kan uw sjabloon vereenvoudigen.

        New-AzureRmResourceGroup -Name ExampleResourceGroup -Location "West US"
   
     Een overzicht van de nieuwe resourcegroep wordt geretourneerd.
   
        ResourceGroupName : ExampleResourceGroup
        Location          : westus
        ProvisioningState : Succeeded
        Tags              :
        Permissions       :
             Actions  NotActions
             =======  ==========
             *
        ResourceId        : /subscriptions/######/resourceGroups/ExampleResourceGroup

4. Voordat u uw implementatie uitvoert, kunt u uw implementatie-instellingen valideren. De cmdlet **Test-AzureRmResourceGroupDeployment** kunt u problemen zoeken voor het maken van de werkelijke resources. Het volgende voorbeeld ziet u hoe u een implementatie valideren.

        Test-AzureRmResourceGroupDeployment -ResourceGroupName ExampleResourceGroup -TemplateFile <PathToTemplate>

5. Als u wilt implementeren resources aan uw resourcegroep, voert u de opdracht **Nieuw AzureRmResourceGroupDeployment** en geef de benodigde parameters. De parameters bevatten een naam voor de implementatie, de naam van uw resourcegroep, het pad of de URL naar de sjabloon die u hebt gemaakt en eventuele andere parameters die u nodig hebt voor uw scenario. Als de parameter **modus** niet opgeeft is, wordt de standaardwaarde van **incrementele** gebruikt. Als u wilt uitvoeren op een volledige implementatie, stel **modus** op **voltooid**. Wees voorzichtig met het gebruik van de volledige modus kunt u per ongeluk bronnen die zich niet in uw sjabloon verwijderen.

     Als u wilt een lokale sjabloon implementeert, gebruikt u de parameter **sjabloonbestand** :

        New-AzureRmResourceGroupDeployment -Name ExampleDeployment -ResourceGroupName ExampleResourceGroup -TemplateFile <PathToTemplate>

     Als u wilt een externe sjabloon implementeert, **TemplateUri** -parameter te gebruiken:

        New-AzureRmResourceGroupDeployment -Name ExampleDeployment -ResourceGroupName ExampleResourceGroup -TemplateUri <LinkToTemplate>
   
     Hebt u de volgende opties voor het leveren van parameterwaarden: 
   
     1. Inline parameters gebruiken.

            New-AzureRmResourceGroupDeployment -Name ExampleDeployment -ResourceGroupName ExampleResourceGroup -TemplateFile <PathToTemplate> -myParameterName "parameterValue"

     2. Gebruik een parameter-object.

            $parameters = @{"<ParameterName>"="<Parameter Value>"}
            New-AzureRmResourceGroupDeployment -Name ExampleDeployment -ResourceGroupName ExampleResourceGroup -TemplateFile <PathToTemplate> -TemplateParameterObject $parameters

     3. Gebruik een lokale parameterbestand. Zie voor informatie over het sjabloonbestand [Parameter-bestand](#parameter-file).

            New-AzureRmResourceGroupDeployment -Name ExampleDeployment -ResourceGroupName ExampleResourceGroup -TemplateFile <PathToTemplate> -TemplateParameterFile <PathToParameterFile>

     4. Een externe parameterbestand gebruiken. Zie voor informatie over het sjabloonbestand [Parameter-bestand](#parameter-file). 

            New-AzureRmResourceGroupDeployment -Name ExampleDeployment -ResourceGroupName ExampleResourceGroup -TemplateUri <LinkToTemplate> -TemplateParameterUri <LinkToParameterFile>

        Wanneer u een externe parameterbestand hebt gebruikt, u kunt geen doorgeven andere waarden beide inline of vanuit een lokaal bestand. Zie de [Parameter prioriteit](#parameter-precendence)voor meer informatie.

     Nadat de resources zijn geïmplementeerd, ziet u een overzicht van de implementatie.

        DeploymentName    : ExampleDeployment
        ResourceGroupName : ExampleResourceGroup
        ProvisioningState : Succeeded
        Timestamp         : 4/14/2015 7:00:27 PM
        Mode              : Incremental
        ...

     Als uw sjabloon een parameter met dezelfde naam als een van de parameters in de PowerShell-opdracht bevat, wordt u gevraagd om een waarde voor die parameter. De parameter van de sjabloon bevat de postfix **FromTemplate**. Een parameter wordt bijvoorbeeld **ResourceGroupName** naam in het sjabloon in strijd met de parameter **ResourceGroupName** in de cmdlet [New-AzureRmResourceGroupDeployment](https://msdn.microsoft.com/library/azure/mt679003.aspx) . U wordt gevraagd naar een waarde opgeven voor **ResourceGroupNameFromTemplate**. In het algemeen, voorkomt u deze verwarring door de naam van parameters niet met dezelfde naam als parameters gebruikt voor implementatie bewerkingen.

6. Als u meer informatie over de implementatie waarmee u eventuele fouten corrigeren implementatie wilt, voert u de parameter **DeploymentDebugLogLevel** mogelijk registreren. U kunt opgeven dat de inhoud van de aanvraag, antwoord van inhoud of beide worden vastgelegd met de implementatie-bewerking.

        New-AzureRmResourceGroupDeployment -Name ExampleDeployment -DeploymentDebugLogLevel All -ResourceGroupName ExampleResourceGroup -TemplateFile <PathOrLinkToTemplate>
        
     Zie voor meer informatie over het gebruik van dit inhoudstype foutopsporing om op te lossen implementaties [Probleemoplossing resource groep implementaties met Azure PowerShell](resource-manager-troubleshoot-deployments-powershell.md).

## <a name="deploy-template-from-storage-with-sas-token"></a>Sjabloon van opslagruimte met SA's token implementeren

U kunt uw sjablonen toevoegen aan een opslag-account en een koppeling naar deze tijdens de implementatie met een token SA's.

> [AZURE.IMPORTANT] Door de onderstaande stappen, is de blob met de sjabloon die toegankelijk zijn voor alleen de eigenaar van het account. De blob is echter toegankelijk voor iedereen met die URI wanneer u een token SA's voor de blob maakt. Als u een andere gebruiker onderschept de URI, kan die gebruiker voor toegang tot de sjabloon. Een token SA's is een goede manier voor het beperken van toegang tot uw sjablonen, maar u moet gevoelige gegevens, zoals wachtwoorden niet rechtstreeks in de sjabloon opnemen.

### <a name="add-private-template-to-storage-account"></a>Persoonlijke sjabloon toevoegen aan opslag-account

De volgende stappen uit een account opslag voor sjablonen instellen:

1. Een resourcegroep maken.

        New-AzureRmResourceGroup -Name ManageGroup -Location "West US"

2. Maak een account opslag. De naam van het opslag-account moet uniek zijn voor Azure, dus Geef uw eigen naam op voor het account.

        New-AzureRmStorageAccount -ResourceGroupName ManageGroup -Name storagecontosotemplates -Type Standard_LRS -Location "West US"

3. Hiermee stelt u de huidige opslag-account.

        Set-AzureRmCurrentStorageAccount -ResourceGroupName ManageGroup -Name storagecontosotemplates

4. Een container maken. De machtiging is ingesteld op **uit** wat betekent dat de container is alleen beschikbaar voor de eigenaar bent.

        New-AzureStorageContainer -Name templates -Permission Off
        
5. De sjabloon toevoegen aan de container.

        Set-AzureStorageBlobContent -Container templates -File c:\Azure\Templates\azuredeploy.json
        
### <a name="provide-sas-token-during-deployment"></a>Token SA's bieden tijdens de implementatie

Als u wilt implementeren een persoonlijke sjabloon in een opslag-account, een token SA's ophalen en deze opnemen in de URI voor de sjabloon.

1. Als u het huidige opslag-account hebt gewijzigd, stelt u de huidige opslag-account aan met uw sjablonen.

        Set-AzureRmCurrentStorageAccount -ResourceGroupName ManageGroup -Name storagecontosotemplates

2. Maak een token SA's met leesmachtigingen en een verlooptijd om toegang te beperken. Hiermee kunt u de volledige URI van de sjabloon die het token SA's inclusief ophalen.

        $templateuri = New-AzureStorageBlobSASToken -Container templates -Blob azuredeploy.json -Permission r -ExpiryTime (Get-Date).AddHours(2.0) -FullUri

3. De sjabloon implementeren doordat de URI die het token SA's bevat.

        New-AzureRmResourceGroupDeployment -ResourceGroupName ExampleResourceGroup -TemplateUri $templateuri

Zie de [gekoppelde sjablonen met Azure Resource Manager gebruiken](resource-group-linked-templates.md)voor een voorbeeld van het gebruik van een token SA's met gekoppelde sjablonen.

[AZURE.INCLUDE [resource-manager-parameter-file](../includes/resource-manager-parameter-file.md)]

## <a name="parameter-precedence"></a>Prioriteit van regels voor parameter

U kunt parameters inline en een lokale parameterbestand in dezelfde implementatie bewerking. U kunt bijvoorbeeld opgeven van enkele waarden in de lokale parameterbestand en andere waarden inline toevoegen tijdens de implementatie. Als u waarden voor een parameter in zowel de lokale parameter bestands- en de inline opgeven, wordt de waarde inline voorrang.

U kunt inline parameters echter niet gebruiken met een externe parameterbestand. Wanneer u een parameterbestand in de parameter **TemplateParameterUri** opgeeft, worden alle inline parameters worden genegeerd. U kunt alle parameterwaarden in het externe bestand moet opgeven. Als uw sjabloon een gevoelige waarde die u niet opnemen in de parameterbestand bevat, die waarde toevoegen aan een belangrijke kluis en overzicht van de belangrijkste kluis in uw parameterbestand externe, of dynamisch bieden alle parameter waarden inline.

Voor meer informatie over het gebruik van een verwijzing KeyVault voor geven secure waarden, raadpleegt u [geven secure waarden tijdens de implementatie](resource-manager-keyvault-parameter.md).

## <a name="next-steps"></a>Volgende stappen
- Zie [Deploy resources gebruik .NET-bibliotheken en een sjabloon](virtual-machines/virtual-machines-windows-csharp-template.md)voor een voorbeeld van de implementatie van bronnen via de bibliotheek .NET-client.
- Als u wilt definiëren parameters in een sjabloon, raadpleegt u [ontwerpfuncties sjablonen](resource-group-authoring-templates.md#parameters).
- Zie voor hulp bij uw-oplossing implementeert in verschillende omgevingen, [ontwikkeling en testomgevingen in Microsoft Azure wordt aangegeven](solution-dev-test-environments.md).

