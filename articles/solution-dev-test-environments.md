<properties
   pageTitle="Ontwikkel- en -omgevingen | Microsoft Azure"
   description="Leer hoe u Azure resourcemanager sjablonen gebruiken om snel en consistent maken en verwijderen van ontwikkeling en testen van omgevingen."
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
   ms.date="01/22/2016"
   ms.author="tomfitz"/>

# <a name="development-and-test-environments-in-microsoft-azure"></a>Ontwikkel- en -omgevingen in Microsoft Azure

Aangepaste toepassingen zijn meestal geïmplementeerd in meerdere ontwikkeling en testen omgevingen voor implementatie op productie. Wanneer omgevingen on-premises worden gemaakt, zijn bronnen aangeschaft of toegewezen aan elke omgeving voor elke toepassing. De omgevingen bevatten vaak verschillende fysieke of virtuele machines met specifieke configuraties die zijn geïmplementeerd handmatig of met complexe automatische scripts. Implementaties vaak uur duren en inconsistente configuraties het resultaat in omgevingen.

## <a name="scenario"></a>Scenario ##

Wanneer u ontwikkeling en testomgevingen in Microsoft Azure inrichten, betaalt u alleen voor de bronnen die u gebruikt.  In dit artikel wordt uitgelegd dat u snel en consistente u kunt maken, behouden, en ontwikkeling verwijderen en testen omgevingen met Azure resourcemanager sjablonen en parameterbestanden, zoals hieronder.

![Scenario](./media/solution-dev-test-environments/scenario.png)

Drie ontwikkeling en testen omgevingen worden hierboven weergegeven.  Elk heeft webtoepassing en SQL-database resources, die zijn opgegeven in een sjabloonbestand.  De namen van de aanvraag en de database in elke omgeving zijn verschillende en zijn opgegeven in unieke parameterbestanden voor elke omgeving.  

Als u niet bekend met Azure resourcemanager concepten bent, het raadzaam dat u het artikel [Azure resourcemanager overzicht](azure-resource-manager/resource-group-overview.md) lezen voordat u lezen in dit artikel.

U kunt eerst Doorloop de stappen in dit artikel vermelde zonder het lezen van een van de artikelen waarnaar wordt verwezen naar snel enkele ervaring met Azure resourcemanager sjablonen. Nadat u eenmaal hebt zijn de stappen doorlopen, u kunt wel krijg antwoord op de meeste vragen die zijn ontstaan uw eerste tijd via door te experimenteren verdere met de stappen en lees de artikelen waarnaar wordt verwezen.

## <a name="plan-azure-resource-use"></a>Gebruik van Azure bronnen plannen
Nadat u het ontwerp van een hoog niveau voor uw toepassing hebt, kunt u definiëren:

- Welke Azure resources uw toepassing bevat. U kunt uw toepassing maken en het dashboard implementeren als een Azure-Web-App met een Azure SQL-Database.  U kunt uw toepassing in de virtuele machines met PHP en MySQL IIS en SQL Server of andere componenten kan maken. Het artikel [Azure App-Service, Cloudservices, en virtuele Machines vergelijking]( app-service-web/choose-web-site-cloud-service-vm.md) kunt u bepalen welke Azure bronnen die u mogelijk wilt gebruiken voor uw toepassing.
- Welke serviceniveauvereisten voor het niveau zoals beschikbaarheid, beveiliging en schaal die voldoen aan uw toepassing.

## <a name="download-an-existing-template"></a>Een bestaande sjabloon te downloaden
Een sjabloon Azure resourcemanager bepaalt alle Azure bronnen die gebruikmaakt van uw toepassing. Diverse sjablonen bestaande reeds dat u kunt rechtstreeks in de portal Azure implementeren of downloaden, wijzigen en opslaan in een systeem voor bronbeheer gebruikt met uw toepassingscode.  Hiermee voert u de onderstaande stappen om een bestaande sjabloon te downloaden.

1. Bladeren in bestaande sjablonen in de bibliotheek van de GitHub [Azure Quickstart sjablonen](https://github.com/Azure/azure-quickstart-templates/) . Klik in de lijst ziet u een map "[201-web-app-sql-database](https://github.com/Azure/azure-quickstart-templates/tree/master/201-web-app-sql-database)". Aangezien veel aangepaste toepassingen zijn opgenomen in een webtoepassing en SQL-database, wordt deze sjabloon kunt u meer informatie over het gebruik van sjablonen gebruikt als voorbeeld in de rest van dit artikel. Het is buiten het bereik van dit artikel volledig uitleg over alles deze sjabloon wordt gemaakt en geconfigureerd, maar als u van plan bent om deze te gebruiken maken werkelijke omgevingen in uw organisatie, wilt u deze beter te begrijpen Lees het artikel [inrichten een web-app met een SQL-Database](app-service-web/app-service-web-arm-with-sql-database-provision.md) .
Opmerking: dit artikel is geschreven voor de December 2015-versie van de sjabloon [201-web-app-sql-database](https://github.com/Azure/azure-quickstart-templates/tree/3f24f7b7e1e377538d1d548eaa6eab2851a21810/201-web-app-sql-database) . De koppelingen onder die verwijzen naar de sjabloon en parameterbestanden naar deze versie van de sjabloon.
2. Klik op het bestand [azuredeploy.json](https://github.com/Azure/azure-quickstart-templates/tree/3f24f7b7e1e377538d1d548eaa6eab2851a21810/201-web-app-sql-database/azuredeploy.json) in de map 201-web-app-sql-database om weer te geven van de inhoud ervan. Dit is het sjabloonbestand resourcemanager Azure. 
3. Klik op de knop '[onbewerkte](https://raw.githubusercontent.com/Azure/azure-quickstart-templates/3f24f7b7e1e377538d1d548eaa6eab2851a21810/201-web-app-sql-database/azuredeploy.json)' in de weergavemodus. 
4. Met de muis, selecteert u de inhoud van dit bestand en sla deze op uw computer als een bestand met de naam "TestApp1-Template.json." 
5. Bekijk de sjablooninhoud en u ziet het volgende:
 - **Resources** gedeelte: in deze sectie wordt gedefinieerd welke typen Azure resources die zijn gemaakt door deze sjabloon. Deze sjabloon maakt tussen andere resourcetypen [Azure Web App](app-service-web/app-service-web-overview.md) en [Azure SQL Database](sql-database/sql-database-technical-overview.md) -resources. Als u wilt uitvoeren en web en SQL-servers in virtuele machines beheren, kunt u de "[iis-2vm-sql-1vm](https://github.com/Azure/azure-quickstart-templates/tree/master/iis-2vm-sql-1vm)" of "[licht-app](https://github.com/Azure/azure-quickstart-templates/tree/master/lamp-app)" sjablonen, maar de instructies in dit artikel zijn gebaseerd op de sjabloon [201-web-app-sql-database](https://github.com/Azure/azure-quickstart-templates/tree/3f24f7b7e1e377538d1d548eaa6eab2851a21810/201-web-app-sql-database) .
 - **Parameters** gedeelte: in deze sectie definieert de parameters die elke resource kan worden geconfigureerd met. Enkele van de parameters opgegeven in de sjabloon bevatten "defaultValue" eigenschappen, en andere niet. Wanneer u implementeert Azure resources met een sjabloon, moet u waarden opgeven voor alle parameters waarvoor geen defaultValue eigenschappen die zijn opgegeven in de sjabloon.  Als u geen waarden voor parameters met defaultValue eigenschappen opgeeft, wordt de waarde die is opgegeven voor de parameter defaultValue in de sjabloon gebruikt.

Een sjabloon bepaalt welke Azure resources worden gemaakt en de parameters elke resource kan worden geconfigureerd met. U kunt meer informatie over sjablonen en hoe u bij het ontwerpen van uw eigen door het lezen van het artikel [Aanbevolen procedures voor het ontwerpen van Azure resourcemanager sjablonen](best-practices-resource-manager-design-templates.md) .

## <a name="download-and-customize-an-existing-parameter-file"></a>Downloaden en aanpassen van een bestaand parameterbestand

Hoewel kunt u beter de *dezelfde* Azure resources die zijn gemaakt in elke omgeving, kunt u de configuratie van de resources worden *verschillende* in elke omgeving.  Dit is waar parameterbestanden komen. Maak parameterbestanden met unieke waarden in elke omgeving de onderstaande stappen uit.   

1. De inhoud van het bestand [azuredeploy.parameters.json](https://github.com/Azure/azure-quickstart-templates/tree/3f24f7b7e1e377538d1d548eaa6eab2851a21810/201-web-app-sql-database/azuredeploy.parameters.json) bekijken in de map 201-web-app-sql-database. Dit is de parameter-bestand voor het sjabloonbestand die u hebt opgeslagen in de vorige sectie. 
2. Klik op de knop '[onbewerkte](https://raw.githubusercontent.com/Azure/azure-quickstart-templates/3f24f7b7e1e377538d1d548eaa6eab2851a21810/201-web-app-sql-database/azuredeploy.parameters.json)' in de weergavemodus. 
3. Met de muis, selecteert u de inhoud van dit bestand en sla deze op drie afzonderlijke bestanden op uw computer met de volgende namen:
 - TestApp1-Parameters-Development.json
 - TestApp1-Parameters-Test.json
 - TestApp1-Parameters-Pre-Production.json

3. Met een tekst of de JSON-editor, de ontwikkeling omgeving parameter-bestand wordt gebruikt, die u hebt gemaakt in stap 3, worden vervangen door de waarden in de lijst aan de rechterkant van de parameterwaarden in het bestand met de *waarden* in de lijst aan de rechterkant van de **parameters** onder bewerken: 
 - **sitenaam**: *TestApp1DevApp*
 - **hostingPlanName**: *TestApp1DevPlan*
 - **siteLocation**: *Central US*
 - **servernaam**: *testapp1devsrv*
 - **serverLocation**: *Central US*
 - **administratorLogin**: *testapp1Admin*
 - **administratorLoginPassword**: *vervangen door uw wachtwoord*
 - **databasenaam**: *testapp1devdb*

4. De omgeving parameter testbestand u hebt gemaakt in stap 3, vervangen met behulp van een tekst of de JSON-editor, worden bewerken de de waarden in de lijst aan de rechterkant van de parameterwaarden in het bestand met de *waarden* aan de rechterkant van de **parameters** van hieronder vermeld:
 - **sitenaam**: *TestApp1TestApp*
 - **hostingPlanName**: *TestApp1TestPlan*
 - **siteLocation**: *Central US*
 - **servernaam**: *testapp1testsrv*
 - **serverLocation**: *Central US*
 - **administratorLogin**: *testapp1Admin*
 - **administratorLoginPassword**: *vervangen door uw wachtwoord*
 - **databasenaam**: *testapp1testdb*

5. Met een tekst of de JSON-editor, bewerk het preproductie parameterbestand dat u hebt gemaakt in stap 3.  Vervangen door de volledige inhoud van het bestand wat hieronder is:

        {
          "$schema" : "http://schema.management.azure.com/schemas/2015-01-01/deploymentParameters.json#",
          "contentVersion" : "1.0.0.0",
          "parameters" : {
        "administratorLogin" : {
          "value" : "testApp1Admin"
        },
        "administratorLoginPassword" : {
          "value" : "replace with your password"
        },
        "databaseName" : {
          "value" : "testapp1preproddb"
        },
        "hostingPlanName" : {
          "value" : "TestApp1PreProdPlan"
        },
        "serverLocation" : {
          "value" : "Central US"
        },
        "serverName" : {
          "value" : "testapp1preprodsrv"
        },
        "siteLocation" : {
          "value" : "Central US"
        },
        "siteName" : {
          "value" : "TestApp1PreProdApp"
        },
        "sku" : {
          "value" : "Standard"
        },
            "requestedServiceObjectiveName" : {
              "value" : "S1"
        }
          }
        }

In het bovenstaande bestand oude productieparameters de **sku** en **requestedServiceObjectiveName** parameters zijn *toegevoegd*, terwijl ze niet zijn toegevoegd in de ontwikkeling en Test parameters-bestanden. Dit is omdat er standaardwaarden die zijn opgegeven voor deze parameters in de sjabloon en in de omgevingen ontwikkeling en Test de standaardwaarden worden gebruikt, maar de voorbereid omgeving niet-standaard waarden voor deze parameters worden gebruikt.

De reden niet-standaard waarden worden gebruikt voor deze parameters in de oude productieomgeving is om te testen van de waarden voor deze parameters die u misschien liever voor uw productieomgeving, zodat ze kunnen ook worden getest.  Deze alle parameters betrekking tot de Azure [hostingprovider Web App-abonnementen](https://azure.microsoft.com/pricing/details/app-service/), of **sku** en Azure [SQL-Database](https://azure.microsoft.com/pricing/details/sql-database/)of **requestedServiceObjectiveName** die worden gebruikt door de toepassing.  Andere SKU's en doelstelling servicenamen hebben verschillende kosten en functies en ondersteuning voor verschillende service niveau aan de doelstellingen.

De onderstaande tabel bevat de standaardwaarden voor deze parameters zijn opgegeven in de sjabloon en de waarden die worden gebruikt in plaats van de standaardwaarden in het oude productieparameters-bestand.

| Parameter | Standaardwaarde | Parameterwaarde-bestand |
|---|---|---|
| **SKU** | Vrij te geven | Standaard |
| **requestedServiceObjectiveName** | S0 | S1 |

## <a name="create-environments"></a>Omgevingen maken
Alle Azure resources moeten worden gemaakt in een [Resourcegroep Azure](azure-resource-manager/resource-group-overview.md). Resourcegroepen kunnen u Azure resources groeperen, zodat ze gezamenlijk kunnen worden beheerd.  [Machtigingen](./active-directory/role-based-access-built-in-roles.md) kunnen worden toegewezen aan resourcegroepen dat bepaalde personen binnen uw organisatie kunt maken, wijzigen, verwijderen of ze en de resources daarin weergeven.  Waarschuwingen en factureringsgegevens voor resources in de groep Resource kunnen worden weergegeven in de [Portal van Azure](https://portal.azure.com). Resourcegroepen worden gemaakt in een Azure [regio](https://azure.microsoft.com/regions/).  In dit artikel worden alle resources in de regio Central US gemaakt. Wanneer u begint met het maken van de werkelijke omgevingen, kiest u de regio die het beste aan uw behoeften. 

Resourcegroepen voor elke omgeving met een van de onderstaande methoden maken.  Alle methoden krijgt hetzelfde resultaat.

###<a name="azure-command-line-interface-cli"></a>Azure opdrachtregelinterface)

Zorg ervoor dat u hebt de CLI [geïnstalleerd](xplat-cli-install.md) op een Windows, OS X, of Linux-computer, en die u [verbonden](xplat-cli-connect.md) uw [Azure AD-account hebt](./active-directory/active-directory-how-subscriptions-associated-directory.md) (ook wel een account voor werk of school) aan uw Azure-abonnement. Typ de opdracht hieronder de resourcegroep voor de ontwikkelomgeving maken vanaf de opdrachtregel CLI.

    azure group create "TestApp1-Development" "Central US"

De opdracht retourneert het volgende als deze is uitgevoerd:

    info:    Executing command group create
    + Getting resource group TestApp1-Development
    + Creating resource group TestApp1-Development
    info:    Created resource group TestApp1-Development
    data:    Id:                  /subscriptions/uuuuuuuu-vvvv-wwww-xxxx-yyyy-zzzzzzzzzzzz/resourceGroups/TestApp1-Development
    data:    Name:                TestApp1-Development
    data:    Location:            centralus
    data:    Provisioning State:  Succeeded
    data:    Tags: null
    data:
    info:    group create command OK

Als de resourcegroep voor de testomgeving, typt u de opdracht hieronder:

    azure group create "TestApp1-Test" "Central US"

Als de resourcegroep voor de oude productieomgeving, typt u de opdracht hieronder:

    azure group create "TestApp1-Pre-Production" "Central US"

###<a name="powershell"></a>PowerShell

Zorg ervoor dat u Azure PowerShell 1.01 hebt of hoger geïnstalleerd op een Windows-computer en uw [Azure AD-account](./active-directory/active-directory-how-subscriptions-associated-directory.md) (ook wel een account voor werk of school) zijn verbonden met uw abonnement als gedetailleerde in het artikel [installeren en configureren van Azure PowerShell](powershell-install-configure.md) . Typ de opdracht hieronder de resourcegroep voor de ontwikkelomgeving maken vanaf een opdrachtprompt PowerShell.

    New-AzureRmResourceGroup -Name TestApp1-Development -Location "Central US"

De opdracht retourneert het volgende als deze is uitgevoerd:

    ResourceGroupName : TestApp1-Development
    Location          : centralus
    ProvisioningState : Succeeded
    Tags              :
    ResourceId        : /subscriptions/uuuuuuuu-vvvv-wwww-xxxx-yyyy-zzzzzzzzzzzz/resourceGroups/TestApp1-Development

Als de resourcegroep voor de testomgeving, typt u de opdracht hieronder:

    New-AzureRmResourceGroup -Name TestApp1-Test -Location "Central US"

Als de resourcegroep voor de oude productieomgeving, typt u de opdracht hieronder:

    New-AzureRmResourceGroup -Name TestApp1-Pre-Production -Location "Central US"

###<a name="azure-portal"></a>Azure-portal

1. Meld u aan bij de [portal van Azure](https://portal.azure.com) met een account [Azure AD](./active-directory/active-directory-how-subscriptions-associated-directory.md) (ook wel een werk of school genoemd). Klik op Nieuw--> Management--> resourcegroep "TestApp1-Development" Voer in het naamvak van Resource-groep, selecteert u uw abonnement en "Centraal US" selecteert in het vak Resource groep locatie, zoals in de onderstaande afbeelding.
   ![Portal](./media/solution-dev-test-environments/rgcreate.png)
2. Klik op de knop maken als u wilt maken van het resourceveld groep.
3. Klik op Bladeren, schuif omlaag in de lijst aan resourcegroepen en klik op resourcegroepen, zoals hieronder wordt weergegeven.
   ![Portal](./media/solution-dev-test-environments/rgbrowse.png) 
4. Nadat u hebt geklikt op resourcegroepen ziet u het blad Resource-groepen met uw nieuwe resourcegroep.
   ![Portal](./media/solution-dev-test-environments/rgview.png)
5. Maken van de TestApp1-toets en TestApp1-Pre-productieve resource groepen dezelfde manier als die u de resourcegroep TestApp1-ontwikkeling hierboven hebt gemaakt.

##<a name="deploy-resources-to-environments"></a>Resources implementeren naar omgevingen

Azure resources dashboard implementeren naar de resourcegroepen voor elke omgeving met het sjabloonbestand voor de oplossing en de parameterbestanden voor elke omgeving gebruikmaakt van een van de onderstaande methoden.  Beide methoden krijgt hetzelfde resultaat.

###<a name="azure-command-line-interface-cli"></a>Azure opdrachtregelinterface)

Typ de opdracht hieronder om te implementeren van resources aan de resourcegroep dat u hebt gemaakt voor de ontwikkelomgeving [pad] vervangen door het pad naar de bestanden die u hebt opgeslagen in de vorige stappen vanaf de opdrachtregel CLI.

    azure group deployment create -g TestApp1-Development -n Deployment1 -f [path]TestApp1-Template.json -e [path]TestApp1-Parameters-Development.json 

Na het zien van een bericht 'Wachten op implementatie om te voltooien' voor een paar minuten, retourneert de opdracht het volgende als deze is uitgevoerd:

    info:    Executing command group deployment create
    + Initializing template configurations and parameters
    + Creating a deployment
    info:    Created template deployment "Deployment1"
    + Waiting for deployment to complete
    data:    DeploymentName     : Deployment1
    data:    ResourceGroupName  : TestApp1-Development
    data:    ProvisioningState  : Succeeded
    data:    Timestamp          : XXXX-XX-XXT20:20:23.5202316Z
    data:    Mode               : Incremental
    data:    Name                           Type          Value
    data:    -----------------------------  ------------  ----------------------------
    data:    siteName                       String        TestApp1DevApp
    data:    hostingPlanName                String        TestApp1DevPlan
    data:    siteLocation                   String        Central US
    data:    sku                            String        Free
    data:    workerSize                     String        0
    data:    serverName                     String        testapp1devsrv
    data:    serverLocation                 String        Central US
    data:    administratorLogin             String        testapp1Admin
    data:    administratorLoginPassword     SecureString  undefined
    data:    databaseName                   String        testapp1devdb
    data:    collation                      String        SQL_Latin1_General_CP1_CI_AS
    data:    edition                        String        Standard
    data:    maxSizeBytes                   String        1073741824
    data:    requestedServiceObjectiveName  String        S0
    info:    group deployment create command OKx

Als de opdracht niet slaagt, foutberichten oplossen en probeer het opnieuw.  Algemene problemen gebruikt parameterwaarden die niet aan Azure naming resourcebeperkingen houden. Andere tips voor probleemoplossing vindt u in het artikel [Probleemoplossing resource groep implementaties in Azure wordt aangegeven](./resource-manager-troubleshoot-deployments-cli.md) .

Typ de opdracht hieronder om te implementeren van resources aan de resourcegroep dat u hebt gemaakt voor de testomgeving, [pad] vervangen door het pad naar de bestanden die u hebt opgeslagen in de vorige stappen vanaf de opdrachtregel CLI.

    azure group deployment create -g TestApp1-Test -n Deployment1 -f [path]TestApp1-Template.json -e [path]TestApp1-Parameters-Test.json

Typ de opdracht hieronder om te implementeren van resources aan de resourcegroep dat u hebt gemaakt voor de oude productieomgeving, [pad] vervangen door het pad naar de bestanden die u hebt opgeslagen in de vorige stappen vanaf de opdrachtregel CLI.

    azure group deployment create -g TestApp1-Pre-Production -n Deployment1 -f [path]TestApp1-Template.json -e [path]TestApp1-Parameters-Pre-Production.json
  
###<a name="powershell"></a>PowerShell

Vanaf een opdrachtprompt Azure PowerShell (versie 1.01 of hoger), typt u de opdracht hieronder om te implementeren van resources aan de resourcegroep dat u hebt gemaakt voor de ontwikkelomgeving [pad] vervangen door het pad naar de bestanden die u hebt opgeslagen in de vorige stappen.

    New-AzureRmResourceGroupDeployment -ResourceGroupName TestApp1-Development -TemplateFile [path]TestApp1-Template.json -TemplateParameterFile [path]TestApp1-Parameters-Development.json -Name Deployment1 

Na een knipperende cursor zien voor een paar minuten, resulteert de opdracht in het volgende als deze is uitgevoerd:

    DeploymentName    : Deployment1
    ResourceGroupName : TestApp1-Development
    ProvisioningState : Succeeded
    Timestamp         : XX/XX/XXXX 2:44:48 PM
    Mode              : Incremental
    TemplateLink      : 
    Parameters        : 
                        Name             Type                       Value     
                        ===============  =========================  ==========
                        siteName         String                     TestApp1DevApp
                        hostingPlanName  String                     TestApp1DevPlan
                        siteLocation     String                     Central US
                        sku              String                     Free      
                        workerSize       String                     0         
                        serverName       String                     testapp1devsrv
                        serverLocation   String                     Central US
                        administratorLogin  String                     testapp1Admin
                        administratorLoginPassword  SecureString                         
                        databaseName     String                     testapp1devdb
                        collation        String                     SQL_Latin1_General_CP1_CI_AS
                        edition          String                     Standard  
                        maxSizeBytes     String                     1073741824
                        requestedServiceObjectiveName  String                     S0        
                        
    Outputs           :

  Als de opdracht niet slaagt, foutberichten oplossen en probeer het opnieuw.  Algemene problemen gebruikt parameterwaarden die niet aan Azure naming resourcebeperkingen houden. Andere tips voor probleemoplossing vindt u in het artikel [Probleemoplossing resource groep implementaties in Azure wordt aangegeven](./resource-manager-troubleshoot-deployments-powershell.md) .

  Vanuit een PowerShell-opdrachtprompt, typt u de opdracht hieronder om te implementeren van resources aan de resourcegroep dat u hebt gemaakt voor de testomgeving, [pad] vervangen door het pad naar de bestanden die u hebt opgeslagen in de vorige stappen.

    New-AzureRmResourceGroupDeployment -ResourceGroupName TestApp1-Test -TemplateFile [path]TestApp1-Template.json -TemplateParameterFile [path]TestApp1-Parameters-Test.json -Name Deployment1

  Vanuit een PowerShell-opdrachtprompt, typt u de opdracht hieronder om te implementeren van resources aan de resourcegroep dat u hebt gemaakt voor de oude productieomgeving, [pad] vervangen door het pad naar de bestanden die u hebt opgeslagen in de vorige stappen.

    New-AzureRmResourceGroupDeployment -ResourceGroupName TestApp1-Pre-Production -TemplateFile [path]TestApp1-Template.json -TemplateParameterFile [path]TestApp1-Parameters-Pre-Production.json -Name Deployment1

De sjabloon en parameter bestanden kunnen worden versienummer en onderhouden met uw toepassingscode in een besturingselement brone-mailsysteem.  U kunt ook de bovenstaande script bestanden en deze opslaan met uw code ook opdrachten opslaan.

> [AZURE.NOTE] U kunt de sjabloon implementeren naar Azure rechtstreeks door te klikken op de knop 'Implementeren naar Azure' op het artikel [inrichten een Web-App met een SQL-Database](https://azure.microsoft.com/documentation/templates/201-web-app-sql-database/) .  Mogelijk nuttige dit voor meer informatie over sjablonen, maar doen dus niet kunt u versie, bewerken en opslaan van uw sjabloon en parameter waarden met uw toepassingscode, zodat u meer details over deze methode is niet beschreven in dit artikel.

## <a name="maintain-environments"></a>Voor het behoud van omgevingen
Overal in de ontwikkeling, kan de configuratie van de Azure bronnen in de verschillende omgevingen worden inconsistent gewijzigd per ongeluk of opzettelijk.  Hiermee kan leiden tot onnodige problemen oplossen en probleemoplossing tijdens de ontwikkelingscyclus van toepassing.

1. De omgevingen wijzigen door het openen van de [Azure-portal](https://portal.azure.com). 
2. Meld u aan bij deze met hetzelfde account die u gebruikt om de bovenstaande stappen te voltooien. 
3. Zo wordt weergegeven in de afbeelding onder, klik op Bladeren--> resourcegroepen (mogelijk moet u schuiven om te zien resourcegroepen).
   ![Portal](./media/solution-dev-test-environments/rgbrowse.png)
4. Nadat u hebt geklikt op resourcegroepen in de bovenstaande afbeelding, ziet u het blad Resource-groepen en de drie resourcegroepen die u in een vorige stap hebt gemaakt, zoals wordt weergegeven in de onderstaande afbeelding. Klik op de resourcegroep TestApp1 ontwikkelen en ziet u het blad waarin de resources die zijn gemaakt door de sjabloon in de groep implementatie van een TestApp1 ontwikkeling resource dat u in een vorige stap hebt voltooid.  De resource TestApp1DevApp Web App verwijderen door TestApp1DevApp in het blad TestApp1 Development resources-groep te klikken en vervolgens op Delete te klikken in het blad TestApp1DevApp Web-app.
   ![Portal](./media/solution-dev-test-environments/portal2.png)
5. Klik op "Ja" wanneer de portal u gevraagd of u zeker weet dat u wilt verwijderen van de resource. Het TestApp1-informatiebron groep blad te sluiten en opnieuw te openen wordt u deze nu weergegeven zonder de Web-app die u zojuist hebt verwijderd.  De inhoud van de resourcegroep is nu anders dan deze moeten worden. Verder kunt u experimenteren meerdere resources verwijdert uit meerdere resourcegroepen of zelfs wijzigt configuratie-instellingen voor een deel van de resources. In plaats van met behulp van de Azure portal verwijderen van een resource uit een resourcegroep, kunt u de opdracht PowerShell [Verwijderen-AzureResource](https://msdn.microsoft.com/library/azure/dn757676.aspx) of de of "azure resource" verwijderopdracht vanaf de CLI voor het uitvoeren van dezelfde taak.
6. Als u alle bronnen en configuratie die zijn in de resourcegroepen terug naar de stand die deze moeten worden in, opnieuw implementeren de omgevingen aan de resourcegroepen met de dezelfde opdrachten die u hebt gebruikt in de sectie [Deploy resources voor omgevingen](#deploy-resources-to-environments) , maar vervangen "Deployment1" met "Deployment2."
7.  Zoals u in de sectie Samenvatting van het blad TestApp1-ontwikkeling in de afbeelding wordt weergegeven in stap 4, ziet u dat de Web-app die u in de portal in de vorige stap hebt verwijderd opnieuw bestaat, zoals doen van andere bronnen u hebt gekozen om te verwijderen. Als u de configuratie van de resources hebt gewijzigd, kunt u ook dat ze hebt ingesteld terug naar de waarden in de parameterbestanden te controleren. Een van de voordelen van het inzetten van uw omgevingen met Azure resourcemanager sjablonen is dat u eenvoudig opnieuw de omgevingen terug naar een bekende staat op elk gewenst moment implementeren kunt.
8. Als u op de tekst onder 'Laatste implementatie' in de onderstaande afbeelding klikt, ziet u een blade waarin de implementatie-geschiedenis voor de resourcegroep.  Aangezien u de naam 'Deployment1' voor de eerste implementatie en "Deployment2" voor de tweede implementatie gebruikt, hebt u twee vermeldingen.  Klikken op een implementatie, wordt een blade waarin de resultaten voor elke implementatie weergegeven.
  ![Portal](./media/solution-dev-test-environments/portal3.png)



## <a name="delete-environments"></a>Omgevingen verwijderen
Zodra u klaar met een omgeving bent, wilt u deze verwijderen zodat u niet oplopen gebruikskosten voor Azure resources die u niet langer gebruikt.  Het staat nog gemakkelijker dan het maken van deze verwijderen door omgevingen.  In de voorgaande stappen, afzonderlijke Azure-resourcegroepen zijn gemaakt voor elke omgeving en vervolgens resources zijn geïmplementeerd in de resourcegroepen. 

Verwijder de omgevingen met een van de onderstaande methoden.  Alle methoden krijgt hetzelfde resultaat.

### <a name="azure-cli"></a>Azure CLI

Bij de opdrachtprompt CLI, typt u het volgende:

    azure group delete "TestApp1-Development"

Wanneer u wordt gevraagd, voert u y en druk op enter om de ontwikkelomgeving en alle bijbehorende informatiebronnen te verwijderen. Na een paar minuten retourneert de opdracht het volgende:

    info:    group delete command OK

Typ de volgende handelingen uit als u wilt verwijderen van de resterende omgevingen CLI de opdrachtprompt:

    azure group delete "TestApp1-Test"
    azure group delete "TestApp1-Pre-Production"
  
### <a name="powershell"></a>PowerShell

Typ de opdracht hieronder om de resourcegroep en de inhoud ervan te verwijderen uit de opdrachtprompt met Azure PowerShell (versie 1.01 of hoger).    

    Remove-AzureRmResourceGroup -Name TestApp1-Development

Wanneer u wordt gevraagd of u zeker weet dat u wilt verwijderen van de resourcegroep, typt u y, gevolgd door de enter-toets.

Typ het volgende als u wilt verwijderen van de resterende omgevingen:

    Remove-AzureRmResourceGroup -Name TestApp1-Test
    Remove-AzureRmResourceGroup -Name TestApp1-Pre-Production

### <a name="azure-portal"></a>Azure-portal

1. Blader in de portal Azure naar resourcegroepen zoals in de voorgaande stappen. 
2. Selecteer de resourcegroep TestApp1-ontwikkeling en klik vervolgens op verwijderen in het blad van de groep TestApp1 Development resources. Een nieuwe blade wordt weergegeven. Voer de naam van de resource-groep en klik op de knop verwijderen.
![Portal](./media/solution-dev-test-environments/rgdelete.png)
3. Verwijderen van de TestApp1-toets en TestApp1-Pre-productieve resource groepen dezelfde manier als die u de resourcegroep TestApp1-ontwikkeling verwijderd.

Ongeacht de methode die u gebruikt, de resourcegroepen en alle bronnen die ze zich niet langer bevinden en u wordt niet meer billing onkosten maken voor de resources.  

Als u wilt minimaliseren van de Azure taken die resource gebruik uitgaven tijdens de ontwikkeling van toepassingen die u [Azure automatisering](automation/automation-intro.md) gebruiken kunt om te plannen:

- Virtuele machines Stop aan het einde van elke dag en start u ze aan het begin van elke dag.
- Hele omgevingen aan het einde van elke dag verwijderen en opnieuw te maken aan het begin van elke dag.
 
Nu dat u hebt ervaring hoe makkelijk het is te maken, onderhouden, en ontwikkeling verwijderen en testen omgevingen, kunt u meer informatie gedaan over wat u zojuist hebt door verder te experimenteren met de bovenstaande stappen en de aanduidingen in dit artikel lezen.

## <a name="next-steps"></a>Volgende stappen

- [Beheer van de gemachtigde](./active-directory/role-based-access-control-configure.md) naar verschillende bronnen op elke omgeving door Microsoft Azure AD-groepen of gebruikers toewijzen aan een specifieke rol die u hebt de mogelijkheid een subset van bewerkingen op Azure resources uitvoeren.
- [Labels toewijzen](resource-group-using-tags.md) aan de resourcegroepen voor elke omgeving en/of de afzonderlijke resources. U kunt een markering 'Omgeving' toevoegen aan uw resourcegroepen en stel de waarde zodat ze overeenkomen met de namen van uw omgeving. Tags kunnen zijn met name handig als u wilt organiseren van resources voor facturering of management.
- Waarschuwingen en facturering voor resource groep resources in de [portal van Azure](https://portal.azure.com)bewaken.
