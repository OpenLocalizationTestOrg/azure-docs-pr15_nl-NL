<properties
    pageTitle="Machine Learning-werkruimte met Azure resourcemanager sjabloon implementeren | Microsoft Azure"
    description="Het implementeren van een werkruimte voor Azure Machine Learning met Azure resourcemanager-sjabloon"
    services="machine-learning"
    documentationCenter=""
    authors="ahgyger"
    manager="haining"
    editor="garye"/>

<tags
    ms.service="machine-learning"
    ms.workload="data-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/23/2016"
    ms.author="ahgyger"/>
# <a name="deploy-machine-learning-workspace-using-azure-resource-manager"></a>Machine Learning-werkruimte met Azure resourcemanager implementeren

## <a name="introduction"></a>Inleiding
Met onderling verbonden onderdelen met een validatie implementeren en probeer opnieuw om in een Azure resourcemanager implementatiesjabloon u tijd bespaart doordat u een scalable manier om te gebruiken. Als u wilt Azure Machine Learning werkruimten hebt ingesteld, bijvoorbeeld, moet u eerst een Azure opslag-account configureren en vervolgens uw werkruimte te implementeren. Stel handmatig om voor honderden werkruimten. Alternatief gemakkelijker is naar een resourcemanager Azure-sjabloon gebruiken om een Azure Machine Learning-werkruimte en alle bijbehorende afhankelijkheden te implementeren. In dit artikel worden behandeld, dit proces stapsgewijze. Zie [overzicht van de Azure resourcemanager](../azure-resource-manager/resource-group-overview.md)voor een overzicht van Azure resourcemanager.

## <a name="step-by-step-create-a-machine-learning-workspace"></a>Stapsgewijze instructies: een Machine Learning-werkruimte maken
We wordt een Azure resourcegroep maken en vervolgens een nieuw Azure opslag-account en een nieuwe Azure Machine Learning-werkruimte met een sjabloon resourcemanager implementeren. Zodra de implementatie voltooid is, wordt we belangrijke informatie over werkruimten die zijn gemaakt (de primaire sleutel, de workspaceID en de URL naar de werkruimte) afgedrukt.

### <a name="create-an-azure-resource-manager-template"></a>Een resourcemanager Azure-sjabloon maken
Een Machine Learning-werkruimte vereist een Azure opslag-account voor de opslag van de gegevensset die is gekoppeld aan dit.
De volgende sjabloon wordt gebruikt voor de naam van de resourcegroep aan de opslagaccountnaam genereren en de naam van de werkruimte.  Ook wordt de naam van het opslag-account als een eigenschap bij het maken van de werkruimte.

```
{
    "contentVersion": "1.0.0.0",
    "$schema": "http://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "variables": {
        "namePrefix": "[resourceGroup().name]",
        "location": "[resourceGroup().location]",
        "mlVersion": "2016-04-01",
        "stgVersion": "2015-06-15",
        "storageAccountName": "[concat(variables('namePrefix'),'stg')]",
        "mlWorkspaceName": "[concat(variables('namePrefix'),'mlwk')]",
        "mlResourceId": "[resourceId('Microsoft.MachineLearning/workspaces', variables('mlWorkspaceName'))]",
        "stgResourceId": "[resourceId('Microsoft.Storage/storageAccounts', variables('storageAccountName'))]",
        "storageAccountType": "Standard_LRS"
    },
    "resources": [
        {
            "apiVersion": "[variables('stgVersion')]",
            "name": "[variables('storageAccountName')]",
            "type": "Microsoft.Storage/storageAccounts",
            "location": "[variables('location')]",
            "properties": {
                "accountType": "[variables('storageAccountType')]"
            }
        },
        {
            "apiVersion": "[variables('mlVersion')]",
            "type": "Microsoft.MachineLearning/workspaces",
            "name": "[variables('mlWorkspaceName')]",
            "location": "[variables('location')]",
            "dependsOn": ["[variables('stgResourceId')]"],
            "properties": {
                "UserStorageAccountId": "[variables('stgResourceId')]"
            }
        }
    ],
    "outputs": {
        "mlWorkspaceObject": {"type": "object", "value": "[reference(variables('mlResourceId'), variables('mlVersion'))]"},
        "mlWorkspaceToken": {"type": "string", "value": "[listWorkspaceKeys(variables('mlResourceId'), variables('mlVersion')).primaryToken]"},
        "mlWorkspaceWorkspaceID": {"type": "string", "value": "[reference(variables('mlResourceId'), variables('mlVersion')).WorkspaceId]"},
        "mlWorkspaceWorkspaceLink": {"type": "string", "value": "[concat('https://studio.azureml.net/Home/ViewWorkspace/', reference(variables('mlResourceId'), variables('mlVersion')).WorkspaceId)]"}
    }
}

```
Deze sjabloon opslaan als bestand mlworkspace.json onder c:\temp\.

### <a name="deploy-the-resource-group-based-on-the-template"></a>De resourcegroep, gebaseerd op de sjabloon implementeren
* Open PowerShell
* Modules voor Azure resourcemanager en beheer van Azure-Service installeren  

```
# Install the Azure Resource Manager modules from the PowerShell Gallery (press “A”)
Install-Module AzureRM -Scope CurrentUser

# Install the Azure Service Management modules from the PowerShell Gallery (press “A”)
Install-Module Azure -Scope CurrentUser
```

   Deze stappen download en installeer het nodig zijn voor de overige stappen modules. Dit hoeft te worden uitgevoerd in de omgeving waarin u de PowerShell-opdrachten uitvoert.   

* Azure verifiëren  

```
# Authenticate (enter your credentials in the pop-up window)
Add-AzureRmAccount
```
Deze stap moet worden herhaald voor elke sessie. Als geverifieerd, kan uw abonnementsgegevens moet worden weergegeven.

![Azure-Account][1]

Nu we hebben toegang tot Azure, kunnen we de resourcegroep maken.

* Een resourcegroep maken

```
$rg = New-AzureRmResourceGroup -Name "uniquenamerequired523" -Location "South Central US"
$rg
```

Controleer of dat de resourcegroep juist is ingericht. **ProvisioningState** moet worden 'is geslaagd."
De naam van de resource-groep wordt gebruikt door de sjabloon voor het genereren van de naam van het opslag-account. De opslagaccountnaam moet tussen 3 en 24 tekens bevatten en gebruiken van getallen en alleen kleine letters.

![Resourcegroep][2]

* Met de implementatie van de groep resource, een nieuwe Machine Learning-werkruimte implementeren.

```
# Create a Resource Group, TemplateFile is the location of the JSON template.
$rgd = New-AzureRmResourceGroupDeployment -Name "demo" -TemplateFile "C:\temp\mlworkspace.json" -ResourceGroupName $rg.ResourceGroupName
```

Wanneer de implementatie is voltooid, is het ingewikkelde aan access-eigenschappen van de werkruimte die u geïmplementeerd. U kunt bijvoorbeeld de primaire sleutel Token openen.

```
# Access Azure ML Workspace Token after its deployment.
$rgd.Outputs.mlWorkspaceToken.Value
```

Een andere manier om op te halen tokens van bestaande werkruimte is via de opdracht Roep-AzureRmResourceAction. U kunt bijvoorbeeld een lijst met de primaire en secundaire tokens van alle werkruimten.

```  
# List the primary and secondary tokens of all workspaces
Get-AzureRmResource |? { $_.ResourceType -Like "*MachineLearning/workspaces*"} |% { Invoke-AzureRmResourceAction -ResourceId $_.ResourceId -Action listworkspacekeys -Force}  
```
Nadat de werkruimte is ingericht, kunt u ook veel Azure Machine Learning Studio taken gebruik van de [PowerShell-Module voor Azure Machine Learning](http://aka.ms/amlps)automatiseren.

## <a name="next-steps"></a>Volgende stappen 
* Meer informatie over [Een presentatie Azure resourcemanager sjablonen](../resource-group-authoring-templates.md). 
* Bekijk de [Azure Quickstart sjablonen opslagplaats](https://github.com/Azure/azure-quickstart-templates)hebben. 
* Bekijk deze video over [Resourcemanager Azure](https://channel9.msdn.com/Events/Ignite/2015/C9-39). 
 
<!--Image references-->
[1]: ../media/machine-learning-deploy-with-resource-manager-template/azuresubscription.png
[2]: ../media/machine-learning-deploy-with-resource-manager-template/resourcegroupprovisioning.png


<!--Link references-->
