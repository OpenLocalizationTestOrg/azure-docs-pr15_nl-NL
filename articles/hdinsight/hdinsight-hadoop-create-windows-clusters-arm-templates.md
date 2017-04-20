<properties
   pageTitle="Maken op basis van Windows Hadoop clusters in met sjablonen resourcemanager Azure HDInsight | Microsoft Azure"
    description="Informatie over het maken van clusters voor Azure HDInsight Azure resourcemanager sjablonen gebruiken."
   services="hdinsight"
   documentationCenter=""
   tags="azure-portal"
   authors="mumian"
   manager="jhubbard"
   editor="cgronlun"/>

<tags
   ms.service="hdinsight"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data"
   ms.date="10/19/2016"
   ms.author="jgao"/>

# <a name="create-windows-based-hadoop-clusters-in-hdinsight-using-azure-resource-manager-templates"></a>Maken op basis van Windows Hadoop clusters in HDInsight Azure resourcemanager sjablonen gebruiken

[AZURE.INCLUDE [selector](../../includes/hdinsight-selector-create-clusters.md)]

Leer hoe u HDInsight clusters met Azure resourcemanager sjablonen maken. Zie [Deploy een toepassing met Azure resourcemanager sjabloon](../resource-group-template-deploy.md)voor meer informatie. Voor het maken van andere cluster hulpprogramma's en functies klikt u op het tabblad Selecteer boven aan deze pagina of Zie [methoden voor het maken van Cluster](hdinsight-provision-clusters.md#cluster-creation-methods).

##<a name="prerequisites"></a>Vereisten:

[AZURE.INCLUDE [delete-cluster-warning](../../includes/hdinsight-delete-cluster-warning.md)]


Voordat u de instructies in dit artikel, hebt u het volgende:

- [Azure-abonnement](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).
- Azure PowerShell of Azure CLI

    [AZURE.INCLUDE [use-latest-version](../../includes/hdinsight-use-latest-powershell-and-cli.md)] 

### <a name="access-control-requirements"></a>Vereisten voor het beheer van Access

[AZURE.INCLUDE [access-control](../../includes/hdinsight-access-control-requirements.md)]

## <a name="resource-manager-templates"></a>Resourcemanager sjablonen

Resourcemanager sjabloon kunt u gemakkelijk HDInsight clusters, hun afhankelijke bronnen (zoals het standaardaccount opslag) en andere resources (zoals Azure SQL-Database voor het gebruik van Apache Sqoop) maken voor uw toepassing een eenmalige, gecoördineerde betrekking heeft. In de sjabloon die u kunt de resources die nodig zijn voor de toepassing definiëren en implementatie parameters voor het invoeren van waarden voor verschillende omgevingen opgeeft. De sjabloon bestaat uit JSON en expressies die u gebruiken kunt om samen te stellen van de waarden voor de implementatie.

Een sjabloon resourcemanager voor het maken van een cluster HDInsight en de afhankelijke opslag van Azure-account kunt u vinden in [Bijlage-A](#appx-a-arm-template). Gebruik een teksteditor aan de sjabloon opslaan in een bestand op uw werkstation. Hier leert u hoe u de sjabloon met verschillende hulpmiddelen bellen.

Zie voor meer informatie over resourcemanager-sjabloon

- [Auteur Azure resourcemanager sjablonen](../resource-group-authoring-templates.md)
- [Een toepassing met Azure resourcemanager sjabloon implementeren](../resource-group-template-deploy.md)


## <a name="deploy-with-powershell"></a>Met PowerShell implementeren

De volgende procedure wordt gemaakt van een cluster HDInsight.

**Een cluster met resourcemanager sjabloon implementeren**

1. Sla het bestand json in [bijlage A](#appx-a-arm-template) bij uw werkstation.
2. Stel de parameters indien nodig.
3. De sjabloon met het volgende PowerShell-script uitvoeren:

        ####################################
        # Set these variables
        ####################################
        #region - used for creating Azure service names
        $nameToken = "<Enter an Alias>" 
        $templateFile = "C:\HDITutorials-ARM\hdinsight-arm-template.json"
        #endregion

        ####################################
        # Service names and varialbes
        ####################################
        #region - service names
        $namePrefix = $nameToken.ToLower() + (Get-Date -Format "MMdd")

        $resourceGroupName = $namePrefix + "rg"
        $hdinsightClusterName = $namePrefix + "hdi"
        $defaultStorageAccountName = $namePrefix + "store"
        $defaultBlobContainerName = $hdinsightClusterName

        $location = "East US 2"

        $armDeploymentName = $namePrefix
        #endregion

        ####################################
        # Connect to Azure
        ####################################
        #region - Connect to Azure subscription
        Write-Host "`nConnecting to your Azure subscription ..." -ForegroundColor Green
        try{Get-AzureRmContext}
        catch{Login-AzureRmAccount}
        #endregion

        # Create a resource group
        New-AzureRmResourceGroup -Name $resourceGroupName -Location $Location

        # Create cluster and the dependent storage accounge
        $parameters = @{clusterName="$hdinsightClusterName";clusterStorageAccountName="$defaultStorageAccountName"}

        New-AzureRmResourceGroupDeployment `
            -Name $armDeploymentName `
            -ResourceGroupName $resourceGroupName `
            -TemplateFile $templateFile `
            -TemplateParameterObject $parameters

        # List cluster
        Get-AzureRmHDInsightCluster -ResourceGroupName $resourceGroupName -ClusterName $hdinsightClusterName 

    De PowerShell-script configureert alleen de clusternaam van de en de naam van het opslag-account.  U kunt andere waarden in de sjabloon resourcemanager instellen. 
    
Zie voor meer informatie [met PowerShell distribueren](../resource-group-template-deploy.md#deploy-with-powershell).

## <a name="deploy-with-azure-cli"></a>Met Azure CLI implementeren

In het onderstaande voorbeeld wordt een cluster en de afhankelijke opslag-account en de container door te bellen van een sjabloon resourcemanager gemaakt:

    azure login
    azure config mode arm
    azure group create -n hdi1229rg -l "East US 2"
    azure group deployment create "hdi1229rg" "hdi1229" --template-file "C:\HDITutorials-ARM\hdinsight-arm-template.json" -p "{\"clusterName\":{\"value\":\"hdi1229win\"},\"clusterStorageAccountName\":{\"value\":\"hdi1229store\"},\"location\":{\"value\":\"East US 2\"},\"clusterLoginPassword\":{\"value\":\"Pass@word1\"}}"





## <a name="deploy-with-rest-api"></a>Implementeren met REST API

Zie [implementeren met de REST API](../resource-group-template-deploy.md#deploy-with-the-rest-api).

## <a name="deploy-with-visual-studio"></a>Met Visual Studio implementeren

U kunt met Visual Studio, een resource groepsproject maken en het dashboard implementeren naar Azure via de gebruikersinterface. Selecteert u het type bronnen die u wilt opnemen in uw project en deze resources worden automatisch toegevoegd aan resourcemanager sjabloon. Het project bevat ook een PowerShell-script als u wilt implementeren van de sjabloon.

Zie voor een introductie over het gebruik van Visual Studio met resourcegroepen, [maken en implementeren van Azure resourcegroepen tot en met Visual Studio](../vs-azure-tools-resource-groups-deployment-projects-create-deploy.md).

##<a name="next-steps"></a>Volgende stappen
U kunt op verschillende manieren om te maken van een cluster HDInsight hebt geleerd in dit artikel. Meer informatie raadpleegt u de volgende artikelen:


- Zie [Deploy resources gebruik .NET-bibliotheken en een sjabloon](../virtual-machines/virtual-machines-windows-csharp-template.md)voor een voorbeeld van de implementatie van bronnen via de bibliotheek .NET-client.
- Zie voor een uitgebreide voorbeeld van een toepassing implementeren, [inrichten en implementeren van microservices volgens plan in Azure wordt aangegeven](../app-service-web/app-service-deploy-complex-application-predictably.md).
- Zie voor hulp bij uw-oplossing implementeert in verschillende omgevingen, [ontwikkeling en testomgevingen in Microsoft Azure wordt aangegeven](../solution-dev-test-environments.md).
- Zie voor meer informatie over de secties van de sjabloon Azure resourcemanager, [ontwerpfuncties sjablonen](../resource-group-authoring-templates.md).
- Zie [functies van de sjabloon](../resource-group-template-functions.md)voor een lijst met de functies die u in een sjabloon Azure resourcemanager gebruiken kunt.



##<a name="appx-a-resource-manager-template"></a>Resourcemanager toepassingX A: sjabloon

De volgende Azure Resource Manager-sjabloon maakt een cluster Hadoop op basis van Windows met het account afhankelijke Azure opslag.

    {
        "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
        "contentVersion": "1.0.0.0",
    "parameters": {
        "location": {
        "type": "string",
        "defaultValue": "East US 2",
        "allowedValues": [
            "Central US",
            "North Europe",
            "East US",
            "East US 2",
            "North Central US",
            "South Central US",
            "West US",
            "North Europe",
            "West Europe",
            "East Asia",
            "Southeast Asia",
            "Japan East",
            "Japan West",
            "Brizil South",
            "Australia East",
            "Australia Southeast",
            "Central India"
        ],
        "metadata": {
            "description": "The location where all azure resources will be deployed."
        }
        },
        "clusterName": {
        "type": "string",
        "metadata": {
            "description": "The name of the HDInsight cluster to create."
        }
        },
        "clusterLoginUserName": {
        "type": "string",
        "defaultValue": "admin",
        "metadata": {
            "description": "These credentials can be used to submit jobs to the cluster and to log into cluster dashboards."
        }
        },
        "clusterLoginPassword": {
        "type": "securestring",
        "metadata": {
            "description": "The password for the cluster login."
        }
        },
        "clusterStorageAccountName": {
        "type": "string",
        "metadata": {
            "description": "The name of the storage account to be created and be used as the cluster's storage."
        }
        },
        "clusterStorageType": {
        "type": "string",
        "defaultValue": "Standard_LRS",
        "allowedValues": [
            "Standard_LRS",
            "Standard_GRS",
            "Standard_ZRS"
        ]
        },
        "clusterWorkerNodeCount": {
        "type": "int",
        "defaultValue": 4,
        "metadata": {
            "description": "The number of nodes in the HDInsight cluster."
        }
        }
    },
        "variables": {},
        "resources": [
            {
            "name": "[parameters('clusterStorageAccountName')]",
            "type": "Microsoft.Storage/storageAccounts",
            "location": "[parameters('location')]",
            "apiVersion": "2015-05-01-preview",
            "dependsOn": [],
            "tags": {},
            "properties": {
                "accountType": "[parameters('clusterStorageType')]"
            }
            },
            {
            "name": "[parameters('clusterName')]",
            "type": "Microsoft.HDInsight/clusters",
            "location": "[parameters('location')]",
            "apiVersion": "2015-03-01-preview",
            "dependsOn": [
                "[concat('Microsoft.Storage/storageAccounts/',parameters('clusterStorageAccountName'))]"
            ],
            "tags": {},
            "properties": {
                "clusterVersion": "3.2",
                "osType": "Windows",
                "clusterDefinition": {
                "kind": "hadoop",
                "configurations": {
                    "gateway": {
                    "restAuthCredential.isEnabled": true,
                    "restAuthCredential.username": "[parameters('clusterLoginUserName')]",
                    "restAuthCredential.password": "[parameters('clusterLoginPassword')]"
                    }
                }
                },
                "storageProfile": {
                "storageaccounts": [
                    {
                    "name": "[concat(parameters('clusterStorageAccountName'),'.blob.core.windows.net')]",
                    "isDefault": true,
                    "container": "[parameters('clusterName')]",
                    "key": "[listKeys(resourceId('Microsoft.Storage/storageAccounts', parameters('clusterStorageAccountName')), '2015-05-01-preview').key1]"
                    }
                ]
                },
                "computeProfile": {
                "roles": [
                    {
                    "name": "headnode",
                    "targetInstanceCount": "1",
                    "hardwareProfile": {
                        "vmSize": "Large"
                    }
                    },
                    {
                    "name": "workernode",
                    "targetInstanceCount": "[parameters('clusterWorkerNodeCount')]",
                    "hardwareProfile": {
                        "vmSize": "Large"
                    }
                    }
                ]
                }
            }
            }
        ],
        "outputs": {
            "cluster": {
            "type": "object",
            "value": "[reference(resourceId('Microsoft.HDInsight/clusters',parameters('clusterName')))]"
            }
        }
    }

