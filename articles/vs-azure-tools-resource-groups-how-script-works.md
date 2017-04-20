<properties
    pageTitle="Overzicht van de Azure resourcegroep project-implementatiescript | Microsoft Azure"
    description="Beschrijving van de werking van de PowerShell-script in het project resourcegroep Azure-implementatie."
    services="visual-studio-online"
    documentationCenter="na"
    authors="tfitzmac"
    manager="timlt"
    editor="" />

 <tags
    ms.service="azure-resource-manager"
    ms.devlang="multiple"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="na"
    ms.date="07/26/2016"
    ms.author="tomfitz" />

# <a name="overview-of-the-azure-resource-group-project-deployment-script"></a>Overzicht van de implementatiescript van de resourcegroep Azure-project

Implementatie van Azure resourcegroep projecten kunt gebruiken om fase en implementeren van bestanden en andere onderdelen op Azure. Wanneer u een implementatieproject van Azure resourcemanager in Visual Studio maakt, wordt een PowerShell-script genoemd **Deploy-AzureResourceGroup.ps1** wordt toegevoegd aan het project. In dit onderwerp vindt meer informatie over wat dit script doet en het uitvoeren van deze binnen en buiten Visual Studio.

## <a name="what-does-the-script-do"></a>Wat is het script?

Het script Deploy-AzureResourceGroup.ps1 biedt twee dingen die belangrijk voor de implementatie-werkstroom zijn.

- Alle bestanden of de onderdelen die u nodig hebt voor de sjabloonimplementatie uploaden
- De sjabloon implementeren

Het eerste gedeelte van het script uploads de bestanden en de onderdelen voor implementatie en de laatste cmdlet in het script implementeren daadwerkelijk de sjabloon. Bijvoorbeeld als een virtuele machine worden geconfigureerd met een script moet, uploadt de implementatiescript eerst veilig configuratiescript van de bij een account Azure opslag. Hierdoor beschikbaar naar Azure resourcemanager voor het configureren van de virtuele machine tijdens het inrichten.

Omdat niet alle sjabloon implementaties extra onderdelen die moeten worden geüpload hebt nodig, wordt een parameter veranderen met de naam van *uploadArtifacts* wordt geëvalueerd. Als alle onderdelen worden geüpload moeten, stelt u de schakeloptie *uploadArtifacts* bij het aanroepen van het script. Houd er rekening mee dat de belangrijkste sjabloonbestand en parameterbestand hoeft te worden geüpload. Alleen andere bestanden, zoals configuratiescripts, genest implementatie sjablonen en toepassingsbestanden moeten worden geüpload.

## <a name="detailed-script-description"></a>Gedetailleerde script beschrijving

Hier volgt een beschrijving van wat select secties van het Doe Deploy AzureResourceGroup.ps1 Azure PowerShell-script.

>[AZURE.NOTE] Hierin versie 1.0 van het script Deploy-AzureResourceGroup.ps1.

1.  Parameters die nodig zijn voor Azure resourcemanager implementatieproject declareren. Sommige parameters hebben standaardwaarden die zijn ingesteld wanneer het project is gemaakt. U kunt deze standaardwaarden in het script wijzigen of andere parameterwaarden toevoegen voordat u het script uitvoeren.

    ```
    Param(
      [string] [Parameter(Mandatory=$true)] $ResourceGroupLocation,
      [string] $ResourceGroupName = 'AzureResourceGroup1',
      [switch] $UploadArtifacts,
      [string] $StorageAccountName,
      [string] $StorageAccountResourceGroupName,
      [string] $StorageContainerName = $ResourceGroupName.ToLowerInvariant() + '-stageartifacts',
      [string] $TemplateFile = '..\Templates\azuredeploy.json',
      [string] $TemplateParametersFile = '..\Templates\azuredeploy.parameters.json',
      [string] $ArtifactStagingDirectory = '..\bin\Debug\staging',
      [string] $AzCopyPath = '..\Tools\AzCopy.exe',
      [string] $DSCSourceFolder = '..\DSC'
    )
    ```

  	|Parameter|Beschrijving|
  	|---|---|
  	|$ResourceGroupLocation|Het land of de gegevens centreren locatie voor de resourcegroep, zoals **West Amerikaans** of **Oost-Azië**.|
  	|$ResourceGroupName|De naam van de Azure resourcegroep.|
  	|$UploadArtifacts|Een binaire waarde die aangeeft of onderdelen moeten worden geüpload naar Azure van uw systeem.|
  	|$StorageAccountName|De naam van uw account Azure opslag is waar uw onderdelen worden geüpload.|
  	|$StorageAccountResourceGroupName|De naam van de Azure resourcegroep met het account opslag.|
  	|$StorageContainerName|De naam van de container opslag is gebruikt voor het uploaden van onderdelen.|
  	|$TemplateFile|Het pad naar de implementatie-bestand (`<app name>.json`) in uw project Azure resourceveld groep.|
  	|$TemplateParametersFile|Het pad naar het parameterbestand (`<app name>.parameters.json`) in uw project Azure resourceveld groep.|
  	|$ArtifactStagingDirectory|Het pad op de computer waarop onderdelen lokaal, met inbegrip van de hoofdmap van de PowerShell-script bevindt zijn. Dit pad kan zijn absolute of ten opzichte van de scriptlocatie.|
  	|$AzCopyPath|Het pad waar de ZIP-bestanden, met inbegrip van de hoofdmap van de PowerShell-script in het hulpmiddel AzCopy.exe wordt gekopieerd. Dit pad kan zijn absolute of ten opzichte van de scriptlocatie.|
  	|$DSCSourceFolder|Het pad naar de map DSC (gewenst staat configuratie) bron, met inbegrip van de hoofdmap van de PowerShell-script. Dit pad kan zijn absolute of ten opzichte van de scriptlocatie. Zie [Inleiding tot de extensie Azure PowerShell DSC (gewenst staat configuratie)](http://blogs.msdn.com/b/powershell/archive/2014/08/07/introducing-the-azure-powershell-dsc-desired-state-configuration-extension.aspx), indien van toepassing, voor meer informatie.|

1.  Controleer of onderdelen moeten worden geüpload naar Azure. Als dat niet zo is, gaat u verder met stap 11. Anders wordt de volgende stappen uitvoeren.

1.  Alle variabelen met relatieve paden converteren naar absolute paden. Bijvoorbeeld, zoals wijzigen een pad `..\Tools\AzCopy.exe` naar `C:\YourFolder\Tools\AzCopy.exe`. Bovendien geïnitialiseerd de variabelen *ArtifactsLocationName* en *ArtifactsLocationSasTokenName* op null. *ArtifactsLocation* en *SaSToken* mogelijk parameters aan de sjabloon. Als de waarden null na lezen in het parameterbestand zijn, wordt het script gegenereerd waarden voor deze.

    De hulpmiddelen Azure gebruik de parameter waarden *_artifactsLocation* en *_artifactsLocationSasToken* in de sjabloon voor het beheren van onderdelen. Als de PowerShell-script parameters met namen worden gevonden, maar de parameterwaarden zijn niet wordt opgegeven, wordt het script de onderdelen uploads en retourneert, passende waarden voor de parameters. Deze waarna ze hebt doorgegeven aan de cmdlet via `@OptionsParameters`.

  	|Variabele|Beschrijving|
  	|---|---|
  	|ArtifactsLocationName|Het pad naar waar de Azure onderdelen zich bevinden.|
  	|ArtifactsLocationSasTokenName|De token SA's (Access-handtekening gedeeld)-naam die wordt gebruikt door het script om Service Bus te verifiëren. Zie [Access handtekening-verificatie gedeeld met Service Bus](service-bus-shared-access-signature-authentication.md) voor meer informatie.|

    ```
    if ($UploadArtifacts) {
    # Convert relative paths to absolute paths if needed
    $AzCopyPath = [System.IO.Path]::Combine($PSScriptRoot, $AzCopyPath)
    $ArtifactStagingDirectory = [System.IO.Path]::Combine($PSScriptRoot, $ArtifactStagingDirectory)
    $DSCSourceFolder = [System.IO.Path]::Combine($PSScriptRoot, $DSCSourceFolder)

    Set-Variable ArtifactsLocationName '_artifactsLocation' -Option ReadOnly
    Set-Variable ArtifactsLocationSasTokenName '_artifactsLocationSasToken' -Option ReadOnly

    $OptionalParameters.Add($ArtifactsLocationName, $null)
    $OptionalParameters.Add($ArtifactsLocationSasTokenName, $null)
    ```

1.  Deze sectie Hiermee wordt gecontroleerd of de <app name>. parameters.json-bestand (de "parameterbestand genoemd ') is een bovenliggend knooppunt benoemde **parameters** (in de `else` blok). Anders heeft geen bovenliggende knooppunt. De notatie is acceptabel.
    
    ```
    if ($JsonParameters -eq $null) {
            $JsonParameters = $JsonContent
        }
        else {
            $JsonParameters = $JsonContent.parameters
        }
    ```

1.  De collectie met parameters die JSON doorlopen. Als een parameterwaarde is toegewezen aan *_artifactsLocation* of *_artifactsLocationSasToken*, stelt u de variabele *$OptionalParameters* met deze waarden. Hiermee voorkomt u dat het script per ongeluk overschrijven geen parameterwaarden die u opgeeft.

    ```
    $JsonParameters | Get-Member -Type NoteProperty | ForEach-Object {
        $ParameterValue = $JsonParameters | Select-Object -ExpandProperty $_.Name

        if ($_.Name -eq $ArtifactsLocationName -or $_.Name -eq $ArtifactsLocationSasTokenName) {
            $OptionalParameters[$_.Name] = $ParameterValue.value
        }
    }
    ```

1.  Lees het opslag accountsleutel en de context voor de opslag account resource voor het opslaan van de onderdelen voor implementatie.

    ```
    $StorageAccountKey = (Get-AzureRMStorageAccountKey -ResourceGroupName $StorageAccountResourceGroupName -Name $StorageAccountName).Key1

    $StorageAccountContext = (Get-AzureRmStorageAccount -ResourceGroupName $StorageAccountResourceGroupName -Name $StorageAccountName).Context
    ```

1.  Als u PowerShell DSC gebruikt voor het configureren van een virtuele machine, moet de extensie DSC de onderdelen in een enkel zip-bestand. Ja, maak een ZIP-archief voor de configuratie DSC. Controleer hiertoe als $DSCSourceFolder bestaat. Als een DSC-configuratie bestaat, verwijdert u deze en maak een nieuwe gecomprimeerde bestand dsc.zip genoemd.

    ```
    # Create DSC configuration archive
    if (Test-Path $DSCSourceFolder) {
    Add-Type -Assembly System.IO.Compression.FileSystem
        $ArchiveFile = Join-Path $ArtifactStagingDirectory "dsc.zip"
        Remove-Item -Path $ArchiveFile -ErrorAction SilentlyContinue
        [System.IO.Compression.ZipFile]::CreateFromDirectory($DSCSourceFolder, $ArchiveFile)
    }
    ```

1.  Als er geen pad voor Azure onderdelen is opgegeven in het bestand Parameters, stelt u een pad naar de PowerShell-script om te gebruiken bij het uploaden van onderdelen. Klik hiertoe door een pad met een combinatie van de opslag van de account eindpunt pad plus de naam van de container opslag te maken. Vervolgens moet u het bestand Parameters bijgewerkt met deze nieuwe pad.

    ```
    # Generate the value for artifacts location if it is not provided in the parameter file
    $ArtifactsLocation = $OptionalParameters[$ArtifactsLocationName]
    if ($ArtifactsLocation -eq $null) {
        $ArtifactsLocation = $StorageAccountContext.BlobEndPoint + $StorageContainerName
        $OptionalParameters[$ArtifactsLocationName] = $ArtifactsLocation
    }
    ```

1.  Gebruik het **AzCopy** hulpprogramma (in de map **hulpmiddelen** van uw project resourcegroep Azure-implementatie) kunt u bestanden vanuit uw lokale opslag decoratieve pad kopiëren naar uw online opslag van Azure-account. Als deze stap is mislukt, sluit u het script af omdat de implementatie, waarschijnlijk mislukt zonder de vereiste onderdelen.

    ```
    # Use AzCopy to copy files from the local storage drop path to the storage account container
    & $AzCopyPath """$ArtifactStagingDirectory""", $ArtifactsLocation, "/DestKey:$StorageAccountKey", "/S", "/Y", "/Z:$env:LocalAppData\Microsoft\Azure\AzCopy\$ResourceGroupName"
    if ($LASTEXITCODE -ne 0) { return }
    ```

1.  Als een token SA's voor de locatie van de onderdelen is niet beschikbaar in het bestand Parameters, maakt u een tijdelijke alleen-lezen toegang tot de online opslag container. Vervolgens doorgeven dat token SA's kunnen de opdrachtregel als een 'optionalParameter." Houd er rekening mee dat eventuele parameters die op de opdrachtregel worden doorgegeven voorrang op waarden die beschikbaar zijn in het parameterbestand.

    ```
    # Generate the value for artifacts location SAS token if it is not provided in the parameter file
    $ArtifactsLocationSasToken = $OptionalParameters[$ArtifactsLocationSasTokenName]
    if ($ArtifactsLocationSasToken -eq $null) {
       # Create a SAS token for the storage container - this gives temporary read-only access to the container
       $ArtifactsLocationSasToken = New-AzureStorageContainerSASToken -Container $StorageContainerName -Context $StorageAccountContext -Permission r -ExpiryTime (Get-Date).AddHours(4)
       $ArtifactsLocationSasToken = ConvertTo-SecureString $ArtifactsLocationSasToken -AsPlainText -Force
       $OptionalParameters[$ArtifactsLocationSasTokenName] = $ArtifactsLocationSasToken
    }
    ```

1.  Maak de resourcegroep als deze nog niet bestaat en kijk of het bestand sjabloon en parameters voor eventuele fouten met gegevensvalidatie die voorkomen de implementatie van het mislukken dat.

    ```
    # Create or update the resource group using the specified template file and template parameters file
    New-AzureRMResourceGroup -Name $ResourceGroupName -Location $ResourceGroupLocation -Verbose -Force -ErrorAction Stop

    Test-AzureRmResourceGroupDeployment -ResourceGroupName $ResourceGroupName -TemplateFile $TemplateFile -TemplateParameterFile $TemplateParametersFile @OptionalParameters -ErrorAction Stop
    ```

1. Tot slot implementeren de sjabloon. Deze code maakt een unieke naam voor de implementatie met een tijdstempel.

    ```
    New-AzureRMResourceGroupDeployment -Name ((Get-ChildItem $TemplateFile).BaseName + '-' + ((Get-Date).ToUniversalTime()).ToString('MMdd-HHmm')) `
        -ResourceGroupName $ResourceGroupName `
        -TemplateFile $TemplateFile `
        -TemplateParameterFile $TemplateParametersFile `
        @OptionalParameters `
        -Force -Verbose
    ```

## <a name="deploy-the-resource-group"></a>De resourcegroep implementeren

### <a name="to-deploy-the-resource-group-in-visual-studio"></a>De resourcegroep in Visual Studio implementeren

1. Kies op het snelmenu van het project Azure resourcegroep **Deploy** > **Nieuwe implementatie**.

    ![][0]

1. In het dialoogvenster **Deploy aan resourcegroep** op een bestaande resourcegroep kiezen in de vervolgkeuzelijst om te implementeren naar of kiest ** &lt;nieuw... &gt;** een nieuwe resourcegroep maken.

    ![][1]

1. Als u wordt gevraagd, voert u een resource groepsnaam en de locatie in het dialoogvenster **Resourcegroep maken** en kies vervolgens de knop **maken** .

    ![][2]

1. Kies de knop **Bewerken van Parameters** om het dialoogvenster **Parameters bewerken** weergeven en voer vervolgens een ontbrekende parameterwaarden.

    ![][3]

    >[AZURE.NOTE] Als alle vereiste parameters waarden nodig hebt, wordt dit dialoogvenster automatisch weergegeven wanneer u implementeert.

    ![][4]

1. Als u klaar bent met parameterwaarden invoeren, klikt u op de knop **Opslaan** en kies vervolgens de knop **Deploy** .

    De implementatiescript (Deploy-AzureResourceGroup.ps1) wordt uitgevoerd en de sjabloon, samen met alle onderdelen, implementeren in Azure.

### <a name="to-deploy-the-resource-group-by-using-powershell"></a>De resourcegroep implementatie via PowerShell

Als u het script uitvoeren wilt zonder de opdracht Visual Studio implementeren en UI, in het snelmenu voor het script, kiest u als volgt te **openen met filter wissen**.

![][5]


## <a name="command-deployment-examples"></a>De opdracht implementatie voorbeelden

### <a name="deploy-using-default-values"></a>Met standaardwaarden implementeren

In dit voorbeeld wordt getoond hoe de met de parameter standaardwaarden script uitvoeren. (Omdat de locatieparameter geen een standaardwaarde bevat, hebt u kunnen.)

`.\Deploy-AzureResourceGroup.ps1 -ResourceGroupLocation eastus`

### <a name="deploy-overriding-the-default-values"></a>Implementeer de standaardwaarden overschrijven

In dit voorbeeld ziet hoe u het script uitvoeren om bestanden van sjabloon en parameters die van de standaardwaarden verschillen implementeren.

```
.\Deploy-AzureResourceGroup.ps1 -ResourceGroupLocation eastus –TemplateFile ..\templates\AnotherTemplate.json –TemplateParametersFile ..\templates\AnotherTemplate.parameters.json
```

### <a name="deploy-using-uploadartifacts-for-staging"></a>Met behulp van UploadArtifacts voor tijdelijke implementeren

In dit voorbeeld ziet hoe u het script uitvoeren om onderdelen uit de releasemap uploaden en implementeren van niet-standaard sjablonen.

```
.\Deploy-AzureResourceGroup.ps1 -StorageAccountName 'mystorage' -StorageAccountResourceGroupName 'Default-Storage-EastUS' -ResourceGroupName 'myResourceGroup' -ResourceGroupLocation 'eastus' -TemplateFile '..\templates\windowsvirtualmachine.json' -TemplateParametersFile '..\templates\windowsvirtualmachine.parameters.json' -UploadArtifacts -ArtifactStagingDirectory ..\bin\release\staging
```

In dit voorbeeld ziet hoe u het script uitvoeren in een Azure PowerShell-taak in Visual Studio Online.

```
$(Build.StagingDirectory)/AzureResourceGroup1/Scripts/Deploy-AzureResourceGroup.ps1 -StorageAccountName 'mystorage' -StorageAccountResourceGroupName 'Default-Storage-EastUS' -ResourceGroupName 'myResourceGroup' -ResourceGroupLocation 'eastus' -TemplateFile '..\templates\windowsvirtualmachine.json' -TemplateParametersFile '..\templates\windowsvirtualmachine.parameters.json' -UploadArtifacts -ArtifactStagingDirectory $(Build.StagingDirectory)
```

## <a name="next-steps"></a>Volgende stappen
Lees meer over Azure resourcemanager [Azure resourcemanager overzicht](azure-resource-manager/resource-group-overview.md).

Zie voor meer voorbeelden van het werken met Azure resourcegroep projecten [distribueren en beheren van Azure resources](https://github.com/Microsoft/HealthClinic.biz/wiki/Deploy-and-manage-Azure-resources) uit de [HealthClinic.biz](https://github.com/Microsoft/HealthClinic.biz) 2015 verbinding maken met [demo](https://blogs.msdn.microsoft.com/visualstudio/2015/12/08/connectdemos-2015-healthclinic-biz/). Zie voor meer QuickStart uit de demo HealthClinic.biz [Azure ontwikkelaars hulpmiddelen voor QuickStart](https://github.com/Microsoft/HealthClinic.biz/wiki/Azure-Developer-Tools-Quickstarts).

[0]: ./media/vs-azure-tools-resource-groups-how-script-works/deploy1c.png
[1]: ./media/vs-azure-tools-resource-groups-how-script-works/deploy2bc.png
[2]: ./media/vs-azure-tools-resource-groups-how-script-works/deploy3bc.png
[3]: ./media/vs-azure-tools-resource-groups-how-script-works/deploy4bc.png
[4]: ./media/vs-azure-tools-resource-groups-how-script-works/deploy5c.png
[5]: ./media/vs-azure-tools-resource-groups-how-script-works/deploy6c.png
