<properties
    pageTitle="Continue-integratie in de VS Team Services met Azure resourcegroep projecten | Microsoft Azure"
    description="Wordt beschreven hoe continue integratie in Visual Studio Team Services instellen met behulp van Azure resourcegroep implementatieprojecten in Visual Studio."
    services="visual-studio-online"
    documentationCenter="na"
    authors="mlearned"
    manager="erickson-doug"
    editor="" />

 <tags
    ms.service="azure-resource-manager"
    ms.devlang="multiple"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="na"
    ms.date="08/01/2016"
    ms.author="mlearned" />

# <a name="continuous-integration-in-visual-studio-team-services-using-azure-resource-group-deployment-projects"></a>Continue-integratie in Visual Studio Team-mailservices via Azure resourcegroep implementatieprojecten

Als u wilt implementeren een Azure-sjabloon, moet u uitvoeren van taken die u moet leest u over de verschillende fasen: opbouwen, Test, kopiëren naar Azure (ook wel 'Tijdelijke' genoemd), en implementeren van sjabloon.  Er zijn twee verschillende manieren hiervoor in Visual Studio Team Services (VS Team Services). Beide methoden bieden dezelfde resultaten, dus kies degene die het beste past bij uw werkstroom.

-   Één stap toevoegen aan de definitie van uw opbouwen die wordt uitgevoerd de PowerShell-script die opgenomen in de resourcegroep Azure-implementatie-project (Deploy-AzureResourceGroup.ps1). Het script onderdelen kopieert en vervolgens implementeert de sjabloon.
-   Voeg dat meerdere tegenover Team Services ontwikkelen stappen voor elke uitvoering van een taak fase.

In dit artikel wordt beschreven hoe u met de eerste optie (gebruik een definitie opbouwen de PowerShell-script uitvoeren). Een voordeel van deze optie is dat het script gebruikt door ontwikkelaars in Visual Studio hetzelfde script die wordt gebruikt door tegenover Team Services. Deze procedure wordt ervan uitgegaan dat u al een Visual Studio-implementatie-project ingecheckt in de VS Team Services.

## <a name="copy-artifacts-to-azure"></a>Onderdelen naar Azure kopiëren 

Ongeacht het scenario, hebt u een onderdelen die nodig zijn voor de sjabloonimplementatie van de, moet u resourcemanager Azure toegang geven tot deze. Deze onderdelen kunnen u de volgende bestanden:

-   Geneste sjablonen
-   Configuratiescripts en DSC scripts
-   Binaire bestanden van toepassing

### <a name="nested-templates-and-configuration-scripts"></a>Geneste sjablonen en configuratiescripts
Wanneer u de sjablonen die is verstrekt door Visual Studio gebruiken (of ingebouwd met Visual Studio fragmenten), de PowerShell-script fasen niet alleen de onderdelen, deze ook de URI voor de resources voor verschillende implementaties parameterizes. Het script en vervolgens wordt gekopieerd van de onderdelen aan een beveiligde container in Azure wordt aangegeven, Hiermee maakt u een token SA's voor dat onderdeel en vervolgens doorgeven dat informatie over de sjabloonimplementatie. Zie [de sjabloonimplementatie van een maken](https://msdn.microsoft.com/library/azure/dn790564.aspx) voor meer informatie over geneste sjablonen.

## <a name="set-up-continuous-deployment-in-vs-team-services"></a>Continue implementatie in tegenover Team Services instellen

De PowerShell-script om in te bellen tegenover Team Services, moet u de definitie van uw opbouwen bijwerken. Kort gezegd zijn zijn de stappen: 

1.  De definitie van de build bewerken.
1.  Azure autorisatie in tegenover Team Services instellen.
1.  Voeg een stap voor het maken van Azure PowerShell dat verwijst naar de PowerShell-script in het project resourcegroep Azure-implementatie.
1.  Stel de waarde van de parameter *- ArtifactsStagingDirectory* voor gebruik met een project die is ingebouwd in de VS Team Services.

### <a name="detailed-walkthrough"></a>Gedetailleerd overzicht

De volgende stappen doorlopen die u stapsgewijze instructies voor het configureren van continue implementatie in de VS Team Services 

1.  De definitie van uw tegenover Team Services opbouwen bewerken en voeg een stap van de build Azure PowerShell toe. Kies de definitie opbouwen onder de categorie **definities bouwen** en kies vervolgens de koppeling **bewerken** .

    ![][0]

1.  Een nieuwe **Azure PowerShell** opbouwen stap toevoegen aan de definitie opbouwen en kies vervolgens **toevoegen bouwen stap...** knop.

    ![][1]

1.  Kies de categorie **Deploy taak** , selecteert u de taak **Azure PowerShell** en kies vervolgens de knop **toevoegen** .

    ![][2]

1.  Kies de stap van de build **Azure PowerShell** en vul de waarden wilt opzoeken.

    1.  Als u al een eindpunt Azure-service is toegevoegd aan de VS Team Services, kiest u het abonnement in de vervolgkeuzelijst van **Azure-abonnement** op keuzelijst met invoervak en gaat u verder met het volgende gedeelte. 

        Als u geen een eindpunt Azure-service in de VS Team Services, moet u een toe te voegen. Deze subsectie gaat u in het proces. Als uw Azure-account gebruikt een Microsoft-account (zoals Hotmail), moet u de volgende stappen uit om de verificatie van een Service Principal.

    1.  Kies op de koppeling **beheren** naast het **Azure-abonnement** het vak vervolgkeuzelijst.

        ![][3]

    1. Kies **Azure** in de **Nieuwe Service-eindpunt** vervolgkeuzelijst.

        ![][4]

    1.  Selecteer in het dialoogvenster **Azure-abonnement toevoegen** de optie **Service Principal** .

        ![][5]

    1.  Uw op Azure-abonnementsgegevens toevoegen aan het dialoogvenster **Azure-abonnement toevoegen** . U moet opgeven van de volgende items:
        -   Abonnements-Id
        -   De naam van abonnement
        -   Service Principal-Id
        -   Service Principal-toets
        -   Tenant-Id

    1.  Een naam van uw keuze toevoegen aan het vak van de naam van **abonnement** . Deze waarde wordt later in de vervolgkeuzelijst van **Azure-abonnement** op lijst in de VS Team Services weergegeven. 

    1.  Als u niet uw Azure abonnements-ID weet, kunt u een van de volgende opdrachten gebruiken om dit te krijgen.
        
        Gebruik voor PowerShell-scripts:

        `Get-AzureRmSubscription`

        Gebruik voor Azure CLI:

        `azure account show`
    

    1.  Als u een Service Principal-ID, volgt Service Principal-toets vast en Tenant-ID, u de procedure in [Active Directory maken-toepassing en service principal met behulp van portal](resource-group-create-service-principal-portal.md) of [een service principal met Azure resourcemanager verifiëren](resource-group-authenticate-service-principal.md).

    1.  De Service Principal-ID, Service Principal sleutel en Tenant-ID-waarden toevoegen aan het dialoogvenster **Azure-abonnement toevoegen** en kies vervolgens de knop **OK** .

        U hebt nu een geldige Service Principal gebruik van de Azure PowerShell-script uitvoeren.

1.  De definitie van de build bewerken en kies de stap van de build **Azure PowerShell** . Selecteer het abonnement in de vervolgkeuzelijst van **Azure-abonnement** op keuzelijst. (Als het abonnement dat niet wordt weergegeven, kies de knop **vernieuwen in** naast de koppeling **beheren** ). 

    ![][8]

1.  Geef het pad op de Deploy AzureResourceGroup.ps1 PowerShell-script. Klik hiertoe klikt u op de knop weglatingsteken (...) naast het vak **Scriptpad** , navigeer naar de Deploy AzureResourceGroup.ps1 PowerShell-script in de map **Scripts** van uw project, selecteert u deze en kies vervolgens de knop **OK** . 

    ![][9]

1. Nadat u het script, moet u het pad naar het script bijwerken zodat deze wordt uitgevoerd vanuit het Build.StagingDirectory (de dezelfde map die is ingesteld op *ArtifactsLocation* ). U kunt dit doen door toe te voegen "$(Build.StagingDirectory)/"naar het begin van het scriptpad.

    ![][10]

1.  Voer de volgende parameters (in een ononderbroken lijn) in het vak **Scriptargumenten** . Als u het script in Visual Studio uitvoeren, kunt u zien hoe tegenover de parameters in **het uitvoervenster** gebruikt. U kunt dit gebruiken als uitgangspunt voor het instellen van de betreffende parameterwaarden in de stap opbouwen.

  	| Parameter | Beschrijving|
  	|---|---|
  	| -ResourceGroupLocation           | De waarde voor geografische locatie waar de resourcegroep zich bevindt, zoals **eastus** of **' Oost Amerikaans '**. (Enkele aanhalingstekens toevoegen als er een spatie in de naam). Zie [Azure regio's](https://azure.microsoft.com/en-us/regions/) voor meer informatie.|                                                                                                                                                                                                                              |
  	| -ResourceGroupName               | De naam van de resourcegroep die wordt gebruikt voor deze implementatie.|                                                                                                                                                                                                                                                                                                                                                                                                                |
  	| -UploadArtifacts                 | Deze parameter, geeft indien aanwezig is, dat onderdelen moeten worden geüpload naar Azure uit het lokale systeem. Alleen moet u deze schakeloptie instellen als uw sjabloonimplementatie is vereist voor extra onderdelen die u wilt fase met de PowerShell-script (zoals configuratiescripts of geneste sjablonen).                                                                                                                                                                 |
  	| -StorageAccountName              | De naam van de opslag-account gebruikt om te fase onderdelen voor deze installatie. Deze parameter is vereist alleen als u bent onderdelen naar Azure kopiëren. Dit account opslag wordt niet automatisch gemaakt door de implementatie, moet deze al bestaan.|                                                                                                                                                                                                                     |
  	| -StorageAccountResourceGroupName | De naam van de resourcegroep die is gekoppeld aan het account opslag. Deze parameter is vereist alleen als u bent onderdelen naar Azure kopiëren.|                                                                                                                                                                                                                                                                                                                               |
  	| -Sjabloonbestand                    | Het pad naar het sjabloonbestand in het project resourcegroep Azure-implementatie. Als u wilt zorgen voor meer flexibiliteit, een pad te gebruiken voor deze parameter die is ten opzichte van de locatie van de PowerShell-script in plaats van een absoluut pad.|
  	| -TemplateParametersFile          | Het pad naar het parameterbestand in het project resourcegroep Azure-implementatie. Als u wilt zorgen voor meer flexibiliteit, een pad te gebruiken voor deze parameter die is ten opzichte van de locatie van de PowerShell-script in plaats van een absoluut pad.|
  	| -ArtifactStagingDirectory        | Deze parameter kunt de PowerShell script weet in welke map van waaruit de binaire bestanden van het project moeten worden gekopieerd. Deze waarde negeert u de standaardwaarde die wordt gebruikt door de PowerShell-script. Voor tegenover Team Services gebruiken, stelt u de waarde in op: - ArtifactStagingDirectory $(Build.StagingDirectory)                                                                                                                                                                                              |

    Hier volgt een voorbeeld in script argumenten (de lijn is gestopt voor leesbaarheid):

    ``` 
    -ResourceGroupName 'MyGroup' -ResourceGroupLocation 'eastus' -TemplateFile '..\templates\azuredeploy.json' 
    -TemplateParametersFile '..\templates\azuredeploy.parameters.json' -UploadArtifacts -StorageAccountName 'mystorageacct' 
    –StorageAccountResourceGroupName 'Default-Storage-EastUS' -ArtifactStagingDirectory '$(Build.StagingDirectory)' 
    ```

    Wanneer u klaar bent, het vak **Scriptargumenten** moet er dan ongeveer als volgt te werk.

    ![][11]

1.  Nadat u hebt toegevoegd dat alle vereiste items met de PowerShell Azure stap maken, kiest u de **wachtrij** opbouwen, knop voor het maken van het project. Het scherm **maken** ziet u de uitvoer van de PowerShell-script.

## <a name="next-steps"></a>Volgende stappen

Lees [Azure resourcemanager overzicht](azure-resource-manager/resource-group-overview.md) voor meer informatie over Azure resourcemanager en Azure resourcegroepen.


[0]: ./media/vs-azure-tools-resource-groups-ci-in-vsts/walkthrough1.png
[1]: ./media/vs-azure-tools-resource-groups-ci-in-vsts/walkthrough2.png
[2]: ./media/vs-azure-tools-resource-groups-ci-in-vsts/walkthrough3.png
[3]: ./media/vs-azure-tools-resource-groups-ci-in-vsts/walkthrough4.png
[4]: ./media/vs-azure-tools-resource-groups-ci-in-vsts/walkthrough5.png
[5]: ./media/vs-azure-tools-resource-groups-ci-in-vsts/walkthrough6.png
[8]: ./media/vs-azure-tools-resource-groups-ci-in-vsts/walkthrough9.png
[9]: ./media/vs-azure-tools-resource-groups-ci-in-vsts/walkthrough10.png
[10]: ./media/vs-azure-tools-resource-groups-ci-in-vsts/walkthrough11b.png
[11]: ./media/vs-azure-tools-resource-groups-ci-in-vsts/walkthrough12.png
