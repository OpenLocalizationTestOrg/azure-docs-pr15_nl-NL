<properties
    pageTitle="Een WebApp met MSDeploy met hostname en ssl-certificaat implementeren"
    description="Een sjabloon Azure Resource Manager gebruiken om te implementeren van een web-app met MSDeploy en instellen van aangepaste hostname en een SSL-certificaat"
    services="app-service\web"
    manager="erikre"
    documentationCenter=""
    authors="jodehavi"
    />

<tags
    ms.service="app-service-web"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="05/31/2016"
    ms.author="john.dehavilland"/>

# <a name="deploy-a-web-app-with-msdeploy-custom-hostname-and-ssl-certificate"></a>Een WebApp met MSDeploy, aangepaste hostname en SSL-certificaat implementeren

Deze handleiding helpt bij het maken van een end-to-end-implementatie voor een Azure-Web-App, gebruikmaken van MSDeploy, evenals een aangepaste hostname en een SSL-certificaat toevoegen aan de sjabloon ARM.

Zie voor meer informatie over het maken van sjablonen, [Authoring Azure resourcemanager sjablonen](../resource-group-authoring-templates.md).

###<a name="create-sample-application"></a>Voorbeeldtoepassing maken

U implementeren een ASP.NET-webtoepassing. De eerste stap is het opzetten van een eenvoudige webtoepassing (of kunt u een bestaande eigenschap te gebruiken: in dat geval kunt u deze stap overslaan).

Open Visual Studio-2015 en kies Bestand > Nieuw Project. Kies in het dialoogvenster dat wordt weergegeven Web > ASP.NET-webtoepassing. Klik onder sjablonen Kies Web en kiest u de sjabloon MVC. Selecteer _het verificatietype wijzigen_ naar _Geen verificatie_. Dit is alleen op de voorbeeldtoepassing zo eenvoudig mogelijk te maken.

U hebt nu een eenvoudige ASP.Net web-app wilt gebruiken als onderdeel van uw implementatieproces.

###<a name="create-msdeploy-package"></a>MSDeploy-pakket maken

Volgende stap is het opzetten van het pakket om te implementeren van de web-app naar Azure. Klik hiertoe uw project opslaan en voer de volgende handelingen uit vanaf de opdrachtregel:

    msbuild yourwebapp.csproj /t:Package /p:PackageLocation="path\to\package.zip"

Hiermee maakt u een ZIP-pakket onder de map PackageLocation. De toepassing is nu klaar om te worden geïmplementeerd, waarin u kunt nu bouwen van een sjabloon Azure resourcemanager dat doen.

###<a name="create-arm-template"></a>Op ARM sjabloon maken
Eerst, laten we beginnen met een basissjabloon van ARM dat een webtoepassing en een hostingprovider plannen (Let die parameters en variabelen niet kort te houden weergegeven worden) wilt maken.

    {
        "name": "[parameters('appServicePlanName')]",
        "type": "Microsoft.Web/serverfarms",
        "location": "[resourceGroup().location]",
        "apiVersion": "2014-06-01",
        "dependsOn": [ ],
        "tags": {
            "displayName": "appServicePlan"
        },
        "properties": {
            "name": "[parameters('appServicePlanName')]",
            "sku": "[parameters('appServicePlanSKU')]",
            "workerSize": "[parameters('appServicePlanWorkerSize')]",
            "numberOfWorkers": 1
        }
    },
    {
        "name": "[variables('webAppName')]",
        "type": "Microsoft.Web/sites",
        "location": "[resourceGroup().location]",
        "apiVersion": "2015-08-01",
        "dependsOn": [
            "[concat('Microsoft.Web/serverfarms/', parameters('appServicePlanName'))]"
        ],
        "tags": {
            "[concat('hidden-related:', resourceGroup().id, '/providers/Microsoft.Web/serverfarms/', parameters('appServicePlanName'))]": "Resource",
            "displayName": "webApp"
        },
        "properties": {
            "name": "[variables('webAppName')]",
            "serverFarmId": "[resourceId('Microsoft.Web/serverfarms/', parameters('appServicePlanName'))]"
        }
    }

Vervolgens moet u voor het wijzigen van de resource web app om een geneste MSDeploy resource te zetten. Hierdoor kunt u om te verwijzen naar het pakket eerder hebt gemaakt, waarbij wordt aangegeven Azure resourcemanager MSDeploy gebruiken om te implementeren van het pakket naar de WebApp Azure. Hierna ziet u de resource Microsoft.Web/sites met de geneste MSDeploy resource:

    {
        "name": "[variables('webAppName')]",
        "type": "Microsoft.Web/sites",
        "location": "[resourceGroup().location]",
        "apiVersion": "2015-08-01",
        "dependsOn": [
            "[concat('Microsoft.Web/serverfarms/', parameters('appServicePlanName'))]"
        ],
        "tags": {
            "[concat('hidden-related:', resourceGroup().id, '/providers/Microsoft.Web/serverfarms/', parameters('appServicePlanName'))]": "Resource",
            "displayName": "webApp"
        },
        "properties": {
            "name": "[variables('webAppName')]",
            "serverFarmId": "[resourceId('Microsoft.Web/serverfarms/', parameters('appServicePlanName'))]"
        },
        "resources": [
            {
                "name": "MSDeploy",
                "type": "extensions",
                "location": "[resourceGroup().location]",
                "apiVersion": "2015-08-01",
                "dependsOn": [
                    "[concat('Microsoft.Web/sites/', variables('webAppName'))]"
                ],
                "tags": {
                    "displayName": "webDeploy"
                },
                "properties": {
                    "packageUri": "[concat(parameters('_artifactsLocation'), '/', parameters('webDeployPackageFolder'), '/', parameters('webDeployPackageFileName'), parameters('_artifactsLocationSasToken'))]",
                    "dbType": "None",
                    "connectionString": "",
                    "setParameters": {
                        "IIS Web Application Name": "[variables('webAppName')]"
                    }
                }
            }
        ]
    }

Nu ziet u de resource MSDeploy heeft die een **packageUri** -eigenschap die wordt als volgt gedefinieerd:

    "packageUri": "[concat(parameters('_artifactsLocation'), '/', parameters('webDeployPackageFolder'), '/', parameters('webDeployPackageFileName'), parameters('_artifactsLocationSasToken'))]"

Deze **packageUri** gaat de opslagruimte account-uri die verwijst naar het account opslag waarnaar u uploaden wordt uw zip pakket aan. De Manager van de Resource Azure maken gebruik van [Access handtekeningen gedeeld](../storage/storage-dotnet-shared-access-signature-part-1.md) om op te halen het pakket omlaag lokaal van de opslag-account wanneer u de sjabloon implementeert. Dit proces wordt uitgevoerd via een PowerShell-script die het pakket uploaden en de API voor het beheer van Azure als u wilt maken van de toetsen aanroepen vereist en geef in de sjabloon als parameters (*_artifactsLocation* en *_artifactsLocationSasToken*). U moet parameters definiëren voor de map en bestandsnaam het pakket wordt geüpload naar onder de container opslag.

Vervolgens moet u in een andere geneste resource voor het instellen van de bindingen hostname als u wilt gebruikmaken van een aangepast domein toevoegen. Moet u eerst om ervoor te zorgen dat u eigenaar bent van de hostnaam en stelt u dit te worden geverifieerd door Azure dat u eigenaar bent van het - Zie [een aangepaste domeinnaam in Azure App-Service configureren](web-sites-custom-domain-name.md). Wanneer u dat hebt gedaan, kunt u de volgende handelingen uit kunt toevoegen aan uw sjabloon onder de sectie Microsoft.Web/sites resource:

    {
        "apiVersion": "2015-08-01",
        "type": "hostNameBindings",
        "name": "www.yourcustomdomain.com",
        "dependsOn": [
            "[concat('Microsoft.Web/sites/', variables('webAppName'))]"
        ],
        "properties": {
            "domainId": null,
            "hostNameType": "Verified",
            "siteName": "variables('webAppName')"
        }
    }

Tot slot moet u een andere bovenste niveau resource, Microsoft.Web/certificates toevoegen. Deze resource uw SSL-certificaat moet bevatten en aanwezig op hetzelfde niveau als uw WebApp en hostingprovider plannen.

    {
        "name": "[parameters('certificateName')]",
        "apiVersion": "2014-04-01",
        "type": "Microsoft.Web/certificates",
        "location": "[resourceGroup().location]",
        "properties": {
            "pfxBlob": "pfx base64 blob",
            "password": "some pass"
        }
    }

U moet een geldig SSL-certificaat hebben de instelling van deze resource. Nadat u die geldig certificaat hebt moet u het extraheren van de bytes pfx als een tekenreeks base64. Eén optie worden geëxtraheerd dit is het gebruik van de volgende PowerShell-opdracht:

    $fileContentBytes = get-content 'C:\path\to\cert.pfx' -Encoding Byte

    [System.Convert]::ToBase64String($fileContentBytes) | Out-File 'pfx-bytes.txt'

Kunt u vervolgens doorgeven dit als een parameter uw sjabloon ARM-implementatie.

De sjabloon ARM is nu klaar.

###<a name="deploy-template"></a>Sjabloon implementeren

De laatste stappen zijn om deze helemaal naar een volledige end-to-end-implementatie. Kunt u implementatie gemakkelijker dat kunt u gebruikmaken van het **Deploy AzureResourceGroup.ps1** PowerShell-script die is toegevoegd wanneer u een resourcegroep Azure-project in Visual Studio om u te helpen maakt met het uploaden van alle onderdelen vereist in de sjabloon. Dit moet u een opslag-account dat u wilt gebruiken tijd vooraf hebt gemaakt. In dit voorbeeld ik heb gemaakt een gedeelde opslag-account voor de package.zip om te worden geüpload. Het script maken gebruik van AzCopy het pakket uploaden naar de opslag-account. U bent in uw maplocatie onderdeel en het script wordt automatisch alle bestanden in die map te uploaden naar de container benoemde opslag. Na het bellen van Deploy-AzureResourceGroup.ps1 die u moet past u de SSL-Bindingen om toe te wijzen van de aangepaste hostname met uw SSL-certificaat.

De volledige implementatie bellen van de Deploy ziet u het volgende PowerShell-AzureResourceGroup.ps1:

    #Set resource group name
    $rgName = "Name-of-resource-group"

    #call deploy-azureresourcegroup script to deploy web app

    .\Deploy-AzureResourceGroup.ps1 -ResourceGroupLocation "East US" `
                                    -ResourceGroupName $rgName `
                                    -UploadArtifacts `
                                    -StorageAccountName "name-of-storage-acct-for-package" `
                                    -StorageAccountResourceGroupName "resource-group-name-storage-acct" `
                                    -TemplateFile "web-app-deploy.json" `
                                    -TemplateParametersFile "web-app-deploy-parameters.json" `
                                    -ArtifactStagingDirectory "C:\path\to\packagefolder\"

    #update web app to bind ssl certificate to hostname. This has to be done after creation above.

    $cert = Get-PfxCertificate -FilePath C:\path\to\certificate.pfx

    $ar = Get-AzureRmResource -Name nameofwebsite -ResourceGroupName $rgName -ResourceType Microsoft.Web/sites -ApiVersion 2014-11-01

    $props = $ar.Properties

    $props.HostNameSslStates[2].'SslState' = 1
    $props.HostNameSslStates[2].'thumbprint' = $cert.Thumbprint
    $props.hostNameSslStates[2].'toUpdate' = $true

    Set-AzureRmResource -ApiVersion 2014-11-01 -Name nameofwebsite -ResourceGroupName $rgName -ResourceType Microsoft.Web/sites -PropertyObject $props

Nu uw toepassing moet zijn geïmplementeerd en u moet kunnen blader er naartoe via https://www.yourcustomdomain.com
