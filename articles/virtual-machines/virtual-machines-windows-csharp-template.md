<properties
    pageTitle="Een VM met C# en een sjabloon resourcemanager implementeren | Microsoft Azure"
    description="Leer hoe u hoe u C# en een resourcemanager-sjabloon met een VM Azure implementeren."
    services="virtual-machines-windows"
    documentationCenter=""
    authors="davidmu1"
    manager="timlt"
    editor="tysonn"
    tags="azure-resource-manager"/>

<tags
    ms.service="virtual-machines-windows"
    ms.workload="na"
    ms.tgt_pltfrm="vm-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/10/2016"
    ms.author="davidmu"/>

# <a name="deploy-an-azure-virtual-machine-using-c-and-a-resource-manager-template"></a>Een Azure virtuele machines met C# en een sjabloon resourcemanager implementeren

Via resourcegroepen en sjablonen, bent u alle bronnen samen die ondersteuning bieden voor uw toepassing beheren. In dit artikel leest u hoe u Visual Studio en C# gebruikt voor verificatie instellen, maakt u een sjabloon, en vervolgens implementeren Azure resources met behulp van de sjabloon die u hebt gemaakt.

Moet u eerst om ervoor te zorgen dat u deze instellingsstappen hebt gedaan:

- [Visual Studio](http://msdn.microsoft.com/library/dd831853.aspx) installeren
- Controleer of de installatie van [Windows Management Framework 3.0](http://www.microsoft.com/download/details.aspx?id=34595) of [Windows Management Framework 4.0](http://www.microsoft.com/download/details.aspx?id=40855)
- Een [verificatietoken](../resource-group-authenticate-service-principal.md) ophalen
- Een resourcegroep met [Azure PowerShell](../resource-group-template-deploy.md) [Azure CLI](../resource-group-template-deploy-cli.md)of [Azure-portal](../resource-group-template-deploy-portal.md)maken.

Het duurt ongeveer 30 minuten moet u deze stappen doen.
    
## <a name="step-1-create-the-visual-studio-project-the-template-file-and-the-parameters-file"></a>Stap 1: Het project Visual Studio, het sjabloonbestand en het parameterbestand maken

### <a name="create-the-template-file"></a>Het sjabloonbestand maken

Een sjabloon Azure resourcemanager kunt mogelijk te implementeren en Azure resources samen beheren. De sjabloon is een JSON-beschrijving van de resources en de bijbehorende implementatie parameters.

Visual Studio, volgt u deze stappen:

1. Klik op **bestand** > **nieuwe** > **Project**.

2. In **sjablonen** > **Visual C#**, selecteer **Console-toepassing**, voert u de naam en locatie van het project en klik vervolgens op **OK**.

3. Met de rechtermuisknop op de naam van het project in Solution Explorer, klikt u op **toevoegen** > **Nieuw Item**.

4. Klik op Web, selecteert u JSON-bestand, *VirtualMachineTemplate.json* invoeren voor de naam en klik vervolgens op **toevoegen**.

5. Toevoegen in de openen en vierkant haakje van het bestand VirtualMachineTemplate.json, het vereiste schema-element en de vereiste contentVersion-element:

        {
          "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json",
          "contentVersion": "1.0.0.0",
        }

6. [Parameters](../resource-group-authoring-templates.md#parameters) zijn niet altijd vereist, maar ze zelf een formule invoerwaarden wanneer de sjabloon wordt geïmplementeerd. Het element parameters en de onderliggende elementen na het element contentVersion toevoegen:

        {
          "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json",
          "contentVersion": "1.0.0.0",
          "parameters": {
            "adminUserName": { "type": "string" },
            "adminPassword": { "type": "securestring" }
          },
        }

7. [Variabelen](../resource-group-authoring-templates.md#variables) kunnen worden gebruikt in een sjabloon kunt u waarden die regelmatig veranderen of waarden die moeten worden gemaakt op basis van een combinatie van parameterwaarden opgeven. Het element variabelen na de sectie parameters toevoegen:

        {
          "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json",
          "contentVersion": "1.0.0.0",
          "parameters": {
            "adminUsername": { "type": "string" },
            "adminPassword": { "type": "securestring" }
          },
          "variables": {
            "vnetID":"[resourceId('Microsoft.Network/virtualNetworks','myvn1')]",
            "subnetRef": "[concat(variables('vnetID'),'/subnets/mysn1')]"  
          },
        }

8. [Resources](../resource-group-authoring-templates.md#resources) , zoals de virtuele machine, het virtuele netwerk en het account opslag worden vervolgens gedefinieerd in de sjabloon. De sectie bronnen na de sectie variabelen toevoegen:

        {
          "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json",
          "contentVersion": "1.0.0.0",
          "parameters": {
            "adminUsername": { "type": "string" },
            "adminPassword": { "type": "securestring" }
          },
          "variables": {
            "vnetID":"[resourceId('Microsoft.Network/virtualNetworks','myvn1')]",
            "subnetRef": "[concat(variables('vnetID'),'/subnets/mysn1')]"
          },
          "resources": [
            {
              "type": "Microsoft.Storage/storageAccounts",
              "name": "mystorage1",
              "apiVersion": "2015-06-15",
              "location": "[resourceGroup().location]",
              "properties": { "accountType": "Standard_LRS" }
            },
            {
              "apiVersion": "2016-03-30",
              "type": "Microsoft.Network/publicIPAddresses",
              "name": "myip1",
              "location": "[resourceGroup().location]",
              "properties": {
                "publicIPAllocationMethod": "Dynamic",
                "dnsSettings": { "domainNameLabel": "mydns1" }
              }
            },
            {
              "apiVersion": "2016-03-30",
              "type": "Microsoft.Network/virtualNetworks",
              "name": "myvnet1",
              "location": "[resourceGroup().location]",
              "properties": {
                "addressSpace": { "addressPrefixes": [ "10.0.0.0/16" ] },
                "subnets": [ {
                  "name": "mysn1",
                  "properties": { "addressPrefix": "10.0.0.0/24" }
                } ]
              }
            },
            {
              "apiVersion": "2016-03-30",
              "type": "Microsoft.Network/networkInterfaces",
              "name": "mync1",
              "location": "[resourceGroup().location]",
              "dependsOn": [
                "Microsoft.Network/publicIPAddresses/myip1",
                "Microsoft.Network/virtualNetworks/myvn1"
              ],
              "properties": {
                "ipConfigurations": [ {
                  "name": "ipconfig1",
                  "properties": {
                    "privateIPAllocationMethod": "Dynamic",
                    "publicIPAddress": {
                      "id": "[resourceId('Microsoft.Network/publicIPAddresses', 'myip1')]"
                    },
                    "subnet": { "id": "[variables('subnetRef')]" }
                  }
                } ]
              }
            },
            {
              "apiVersion": "2016-03-30",
              "type": "Microsoft.Compute/virtualMachines",
              "name": "myvm1",
              "location": "[resourceGroup().location]",
              "dependsOn": [
                "Microsoft.Network/networkInterfaces/mync1",
                "Microsoft.Storage/storageAccounts/mystorage1"
              ],
              "properties": {
                "hardwareProfile": { "vmSize": "Standard_A1" },
                "osProfile": {
                  "computerName": "myvm1",
                  "adminUsername": "[parameters('adminUsername')]",
                  "adminPassword": "[parameters('adminPassword')]"
                },
                "storageProfile": {
                  "imageReference": {
                    "publisher": "MicrosoftWindowsServer",
                    "offer": "WindowsServer",
                    "sku": "2012-R2-Datacenter",
                    "version" : "latest"
                  },
                  "osDisk": {
                    "name": "myosdisk1",
                    "vhd": {
                      "uri": "https://mystorage1.blob.core.windows.net/vhds/myosdisk1.vhd"
                    },
                    "caching": "ReadWrite",
                    "createOption": "FromImage"
                  }
                },
                "networkProfile": {
                  "networkInterfaces" : [ {
                    "id": "[resourceId('Microsoft.Network/networkInterfaces','mync1')]"
                  } ]
                }
              }
            } ]
          }
      
9. Sla het sjabloonbestand die u hebt gemaakt.

### <a name="create-the-parameters-file"></a>Het parameterbestand maken

Waarden voor de resourceparameters die zijn gedefinieerd in de sjabloon wilt opgeven, maakt u een parameterbestand met de waarden die worden gebruikt wanneer de sjabloon wordt geïmplementeerd. Visual Studio, volgt u deze stappen:

1. Met de rechtermuisknop op de naam van het project in Solution Explorer, klikt u op **toevoegen** > **Nieuw Item**.

2. Klik op Web, selecteert u JSON-bestand, *Parameters.json* invoeren voor de naam en klik vervolgens op **toevoegen**.

3. Open het bestand parameters.json en klikt u vervolgens de inhoud van deze JSON toevoegen:

        {
          "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json",
          "contentVersion": "1.0.0.0",
          "parameters": {
            "adminUserName": { "value": "mytestacct1" },
            "adminPassword": { "value": "mytestpass1" }
          }
        }

    >[AZURE.NOTE] In dit artikel wordt gemaakt van een virtuele machine met een versie van het besturingssysteem Windows Server. Zie voor meer informatie over het selecteren van de andere afbeeldingen, [navigeren en selecteer Azure virtuele machines afbeeldingen met Windows PowerShell en de CLI Azure](virtual-machines-linux-cli-ps-findimage.md).

4. Sla het parameterbestand die u hebt gemaakt.

## <a name="step-2-install-the-libraries"></a>Stap 2: Installeer de bibliotheken

NuGet-pakketten zijn de eenvoudigste manier om het installeren van de bibliotheken die u moet deze zelfstudie hebt voltooid. Moet u de bibliotheek van Azure Resource Management en Azure Active Directory-verificatie Library de resources maken. Als u deze bibliotheken in Visual Studio, volgt u deze stappen:

1. Met de rechtermuisknop op de naam van het project in de Solution Explorer op **NuGet-pakketten beheren**en klik vervolgens op Bladeren.

2. Type, *Active Directory* in het zoekvak, klikt u op **installeren** voor de bibliotheek van Active Directory-verificatie-pakket en volg de instructies voor het installeren van het pakket.

4. Selecteer boven aan de pagina, **Opnemen voorlopige versie**. Type *Microsoft.Azure.Management.ResourceManager* in het zoekvak, klikt u op **installeren** voor de Microsoft Azure Resource Management bibliotheken en volg de instructies voor het installeren van het pakket.

U bent nu klaar om te beginnen met het gebruik van de bibliotheken voor het maken van uw toepassing.

## <a name="step-3-create-the-credentials-that-are-used-to-authenticate-requests"></a>Stap 3: De referenties die worden gebruikt voor verificatie van aanvragen maken

De Azure Active Directory-toepassing is gemaakt en de verificatie-bibliotheek is geïnstalleerd. Nu opmaken u de toepassingsgegevens in referenties die worden gebruikt voor verificatie van aanvragen naar de resourcemanager van Azure.

1. Open het bestand Program.cs voor het project dat u hebt gemaakt en voegt u deze met instructies naar het begin van het bestand:

        using Microsoft.Azure;
        using Microsoft.IdentityModel.Clients.ActiveDirectory;
        using Microsoft.Azure.Management.ResourceManager;
        using Microsoft.Azure.Management.ResourceManager.Models;
        using Microsoft.Rest;
        using System.IO;

2.  Deze methode toevoegen aan de klas programma om het token die nodig is om te maken van de referenties:

        private static async Task<AuthenticationResult> GetAccessTokenAsync()
        {
          var cc = new ClientCredential("{client-id}", "{client-secret}");
          var context = new AuthenticationContext("https://login.windows.net/{tenant-id}");
          var token = await context.AcquireTokenAsync("https://management.azure.com/", cc);
          if (token == null)
          {
            throw new InvalidOperationException("Could not get the token.");
          }
          return token;
        }

    {Client-id} vervangen door de id van de Azure Active Directory-toepassing, {client-geheim} met de access-sleutel van de AD-toepassing en {tenant-id} aan de tenant-id voor uw abonnement. U vindt de id van de tenant door Get-AzureRmSubscription uit te voeren. U kunt de toegangstoets vinden met behulp van de Azure-portal.

3. Als u wilt de referenties hebt gemaakt, moet u deze code toevoegen met de methode hoofdtabblad in het bestand Program.cs:

        var token = GetAccessTokenAsync();
        var credential = new TokenCredentials(token.Result.AccessToken);

4. Sla het bestand Program.cs.

## <a name="step-4-deploy-the-template"></a>Stap 4: De sjabloon implementeren

In deze stap geeft u de resourcegroep die u eerder hebt gemaakt, maar u ook een resourcegroep met de [ResourceGroup](https://msdn.microsoft.com/library/azure/microsoft.azure.management.resources.models.resourcegroup.aspx) en de klassen [ResourceManagementClient](https://msdn.microsoft.com/library/azure/microsoft.azure.management.resources.resourcemanagementclient.aspx) kan maken.

1. Variabelen toevoegen aan de methode Hoofdgegeven van het programma-klasse om op te geven van de namen van de bronnen die u eerder hebt gemaakt, de naam van de implementatie en uw abonnement-id:

        var groupName = "resource group name";
        var subscriptionId = "subsciption id";
        var deploymentName = "deployment name";

    Vervang de waarde van de groepsnaam door de naam van het resourceveld groep. Vervang de waarde van deploymentName door de naam die u wilt gebruiken voor de implementatie. U vindt de id van het abonnement door Get-AzureRmSubscription uit te voeren.

2. Deze methode toevoegen aan de klas programma implementeren van de bronnen aan de resourcegroep met behulp van de sjabloon die u hebt gedefinieerd:

        public static async Task<DeploymentExtended> CreateTemplateDeploymentAsync(
          TokenCredentials credential,
          string groupName,
          string deploymentName,
          string subscriptionId)
        {
          Console.WriteLine("Creating the template deployment...");
          var deployment = new Deployment();
          deployment.Properties = new DeploymentProperties
          {
            Mode = DeploymentMode.Incremental,
            Template = File.ReadAllText("..\\..\\VirtualMachineTemplate.json"),
            Parameters = File.ReadAllText("..\\..\\Parameters.json")
          };
          var resourceManagementClient = new ResourceManagementClient(credential) 
            { SubscriptionId = subscriptionId };
          return await resourceManagementClient.Deployments.CreateOrUpdateAsync(
            groupName,
            deploymentName,
            deployment);
        }

    Als u implementeren van de sjabloon een opslag-account wilt, kunt u de eigenschap sjabloon vervangen door de eigenschap TemplateLink.

3. Als u wilt bellen de methode die u zojuist hebt toegevoegd, moet u deze code toevoegen met de methode Hoofdgegeven:

        var dpResult = CreateTemplateDeploymentAsync(
          credential,
          groupName,
          deploymentName,
          subscriptionId);
        Console.WriteLine(dpResult.Result.Properties.ProvisioningState);
        Console.ReadLine();

## <a name="step-5-delete-the-resources"></a>Stap 5: De resources verwijderen

Omdat u worden berekend voor resources die worden gebruikt in Azure wordt aangegeven, maar het is altijd een goede gewoonte om het verwijderen van resources die niet meer nodig zijn. U hoeft niet te elke resource afzonderlijk verwijderen uit een resourcegroep. Verwijder de resourcegroep en alle bijbehorende resources worden automatisch verwijderd.

1.  De als resourcegroep wilt verwijderen, door deze methode te toevoegen aan de klas programma:

        public static async void DeleteResourceGroupAsync(
          TokenCredentials credential,
          string groupName,
          string subscriptionId)
        {
          Console.WriteLine("Deleting resource group...");
          var resourceManagementClient = new ResourceManagementClient(credential)
            { SubscriptionId = subscriptionId };
          await resourceManagementClient.ResourceGroups.DeleteAsync(groupName);
        }

2.  Als u wilt bellen de methode die u zojuist hebt toegevoegd, moet u deze code toevoegen met de methode Hoofdgegeven:

        DeleteResourceGroupAsync(
          credential,
          groupName,
          subscriptionId);
        Console.ReadLine();

##<a name="step-6-run-the-console-application"></a>Stap 6: De consoletoepassing wordt uitgevoerd

1.  Voer de consoletoepassing en klikt u op **starten** in Visual Studio, meld u aan bij Azure AD met dezelfde referenties dat u met uw abonnement gebruikt.

2.  Druk op **Enter** nadat de geaccepteerde-status wordt weergegeven.

    Het moet over vijf minuten duren voordat deze consoletoepassing om uit te voeren volledig vanaf begin om te voltooien. Als u op Enter om te beginnen u resources verwijdert, kunt u een paar minuten om te controleren of het maken van de resources in de portal van Azure voordat u ze verwijderen kunt volgen.

3. Als u wilt zien van de status van de bronnen, gaat u naar de controlelogboeken bijhouden in de portal van Azure:

    ![Controlelogboeken bijhouden in Azure-portal bladeren](./media/virtual-machines-windows-csharp-template/crpportal.png)

## <a name="next-steps"></a>Volgende stappen

- Als er problemen met de implementatie zijn, wordt een volgende stap zou nagaan [Probleemoplossing resource groep implementaties met Azure-portal](../resource-manager-troubleshoot-deployments-portal.md).
- Informatie over het beheren van de virtuele machine die u aan de hand van [beheren virtuele machines met Azure resourcemanager en PowerShell](virtual-machines-windows-csharp-manage.md)hebt gemaakt.
