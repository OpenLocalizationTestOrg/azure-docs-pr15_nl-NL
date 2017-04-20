<properties
   pageTitle="Windows PowerShell-Scripts publiceren naar ontwikkelaar en testomgevingen met | Microsoft Azure"
   description="Informatie over het gebruik van Windows PowerShell-scripts vanuit Visual Studio publiceren naar ontwikkelen en testen van omgevingen."
   services="visual-studio-online"
   documentationCenter="na"
   authors="TomArcher"
   manager="douge"
   editor="" />
<tags
   ms.service="multiple"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="multiple"
   ms.date="08/15/2016"
   ms.author="tarcher" />

# <a name="using-windows-powershell-scripts-to-publish-to-dev-and-test-environments"></a>Windows PowerShell-scripts gebruiken om te publiceren op ontwikkelaar en test omgevingen

Als u een webtoepassing in Visual Studio maken, kunt u een Windows PowerShell-script die u later gebruiken kunt om te automatiseren van het publiceren van uw website kunt Azure als een Web-App in Azure App Service of een virtuele machine genereren. U kunt bewerken en het Windows PowerShell-script in de editor Visual Studio verder aan uw eisen uitbreiden of het script integreren met bestaande opbouwen, testen en het publiceren van scripts.

Met deze scripts, kunt u aangepaste versies (ook wel bekend als een ontwikkelaar en test omgevingen) van uw site voor tijdelijk gebruik inrichten. U kunt bijvoorbeeld een bepaalde versie van uw website op een Azure virtuele machines of op het tijdelijk opslaan slot op een website voor het uitvoeren van een test-suite, reproduceren een fout, test een oplossing bug, proefabonnement een voorgestelde wijziging of instellen van een aangepaste omgeving voor een demo of presentatie instellen. Nadat u een script die uw project publiceert hebt gemaakt, kunt u opnieuw identieke omgevingen maken door het script opnieuw uit te voeren naar wens of het script uitvoeren met uw eigen opbouwen van uw webtoepassing om een aangepaste omgeving voor het testen van te maken.

## <a name="what-you-need"></a>Wat u nodig hebt

- Azure SDK 2.3 of later. Zie [Visual Studio-Downloads](http://go.microsoft.com/fwlink/?LinkID=624384) voor meer informatie.

U hoeft niet de SDK Azure te genereren van de scripts voor webprojecten. Deze functie is voor webprojecten, niet web rollen in cloudservices.

- Azure PowerShell 0.7.4 of hoger. Lees [hoe u installeren en configureren van Azure PowerShell](powershell-install-configure.md) voor meer informatie.

- [Windows PowerShell 3.0](http://go.microsoft.com/?linkid=9811175) of hoger.

## <a name="additional-tools"></a>Extra hulpmiddelen

Aanvullende hulpmiddelen en bronnen voor het werken met PowerShell in Visual Studio voor de ontwikkeling van Azure zijn beschikbaar. Zie [PowerShell-hulpprogramma's voor Visual Studio](http://go.microsoft.com/fwlink/?LinkId=404012).

## <a name="generating-the-publish-scripts"></a>De scripts publiceren genereren

U kunt de scripts publiceren voor een virtuele machine waarop uw website wanneer u een nieuw project door de volgende [deze instructies](./virtual-machines/virtual-machines-windows-classic-web-app-visual-studio.md)te maken genereren. U kunt ook [genereren scripts voor WebApps publiceren in Azure App Service](./app-service-web/web-sites-dotnet-get-started.md).

## <a name="scripts-that-visual-studio-generates"></a>Scripts die Visual Studio gegenereerd

Visual Studio genereert een oplossing op gebruikersniveau map genaamd **PublishScripts** met twee Windows PowerShell-bestanden, een script publiceren voor uw VM of website en een module met functies die u in de scripts gebruiken kunt. Visual Studio gegenereerd ook een bestand in de indeling van JSON waarmee de details van het project dat u wilt implementeren.

### <a name="windows-powershell-publish-script"></a>Windows PowerShell publiceren script

Het script publiceren bevat specifieke Publiceren stappen voor het implementeren naar een website of virtuele machine. Visual Studio biedt syntaxis van de kleuren voor de ontwikkeling van de Windows PowerShell. Help voor de functies is beschikbaar en kunt u de functies in het script aanpassen aan uw vereisten voor veranderende vrij bewerken.

### <a name="windows-powershell-module"></a>Windows PowerShell-module

De Windows PowerShell-module die Visual Studio gegenereerd bevat functies die het script publiceren wordt gebruikt. Deze zijn Azure PowerShell-functies en niet moeten worden gewijzigd. Lees [hoe u installeren en configureren van Azure PowerShell](powershell-install-configure.md) voor meer informatie.

### <a name="json-configuration-file"></a>Configuratiebestand JSON

De JSON-bestand is gemaakt in de map **configuraties** en configuratiegegevens waarmee wordt opgegeven precies welke bronnen die u wilt implementeren naar Azure bevat. De naam van het bestand dat Visual Studio gegenereerd is project-naam-WAWS-dev.json als u een website of project naam-VM-dev.json gemaakt als u een virtuele machine hebt gemaakt. Hier volgt een voorbeeld van een JSON-configuratiebestand die wordt gegenereerd wanneer u een website hebt gemaakt. De meeste van de waarden zijn geen uitleg. De websitenaam van de wordt gegenereerd door Azure, zodat deze mogelijk niet overeenkomen met de naam van uw project.

```
{
"environmentSettings": {
"webSite": {
"name": "WebApplication26632",
"location": "West US"
},
"databases": [
{
"connectionStringName": "DefaultConnection",
"databaseName": "WebApplication26632_db",
"serverName": "YourDatabaseServerName",
"user": "sqluser2",
"password": "",
"edition": "",
"size": "",
"collation": "",
"location": "West US"
}
]
}
}
```
Wanneer u een virtuele machine maakt, ziet het configuratiebestand JSON er ongeveer als volgt te werk. Houd er rekening mee dat een cloudservice als een container voor de virtuele machine wordt gemaakt. De virtuele machine bevat de gebruikelijke eindpunten voor web access via HTTP en HTTPS, evenals de eindpunten voor het Web implementeert, waarmee u de publiceren naar de website van uw lokale computer, extern bureaublad en Windows PowerShell.

```
{
"environmentSettings": {
"cloudService": {
"name": "myusernamevm1",
"affinityGroup": "",
"location": "West US",
"virtualNetwork": "",
"subnet": "",
"availabilitySet": "",
"virtualMachine": {
"name": "myusernamevm1",
"vhdImage": "a699494373c04fc0bc8f2bb1389d6106__Win2K8R2SP1-Datacenter-201403.01-en.us-127GB.vhd",
"size": "Small",
"user": "vmuser1",
"password": "",
"enableWebDeployExtension": true,
"endpoints": [
{
"name": "Http",
"protocol": "TCP",
"publicPort": "80",
"privatePort": "80"
},
{
"name": "Https",
"protocol": "TCP",
"publicPort": "443",
"privatePort": "443"
},
{
"name": "WebDeploy",
"protocol": "TCP",
"publicPort": "8172",
"privatePort": "8172"
},
{
"name": "Remote Desktop",
"protocol": "TCP",
"publicPort": "3389",
"privatePort": "3389"
},
{
"name": "Powershell",
"protocol": "TCP",
"publicPort": "5986",
"privatePort": "5986"
}
]
}
},
"databases": [
{
"connectionStringName": "",
"databaseName": "",
"serverName": "",
"user": "",
"password": ""
}
],
"webDeployParameters": {
"iisWebApplicationName": "Default Web Site"
}
}
}
```

U kunt bewerken dat de JSON-configuratie als u wilt wijzigen wat er gebeurt wanneer het uitvoeren van scripts publiceren. De `cloudService` en `virtualMachine` secties zijn vereist, maar u kunt verwijderen de `databases` sectie als u deze niet nodig hebt. De eigenschappen die leeg zijn in de standaard-configuratiebestand die Visual Studio gegenereerd zijn optioneel. items die waarden in het standaardconfiguratiebestand bevatten zijn vereist.

Als u een website waarop implementatieomgevingen met meerdere (sleuven genoemd) in plaats van een enkele productielocatie in Azure hebt, kunt u de naam van de slot naam van de website kunt opnemen in het configuratiebestand JSON. Bijvoorbeeld als u een website hebt die heeft een naam met **Mijn site** en een slot voor dit met de naam **test** vervolgens de URI is Mijn site-test.cloudapp.net, maar de juiste naam in het configuratiebestand wordt mysite(test). U kunt dit alleen doen als de website en sleuven al in uw abonnement. Als ze niet bestaat, de website maken door het script zonder op te geven van de slot, en vervolgens de slot in de [portal van Azure klassieke](http://go.microsoft.com/fwlink/?LinkID=213885)maken en daarna het script uitvoeren met de websitenaam van de gewijzigde. Zie [tijdelijke omgevingen voor web-apps in Azure-Service voor App instellen](./app-service-web/web-sites-staged-publishing.md)voor meer informatie over de voor-WebApps implementatie-elementen.

## <a name="how-to-run-the-publish-scripts"></a>Het uitvoeren van scripts publiceren

Als u een Windows PowerShell-script voordat u nooit hebt uitgevoerd, moet u eerst het beleid kan worden uitgevoerd om in te schakelen uitvoeren van scripts instellen. Dit is een beveiligingsfunctie om te voorkomen dat gebruikers Windows PowerShell-scripts wordt uitgevoerd als ze vatbaar voor schadelijke software of virussen waarbij u gebruikmaakt van het uitvoeren van scripts.

### <a name="run-the-script"></a>Het script uitvoeren

1. Maak het pakket Web implementeren voor uw project. Een pakket Web implementeren is een gecomprimeerd archief (ZIP-bestand) met bestanden die u wilt kopiëren naar uw website of virtuele machine. U kunt pakketten Web implementeren in Visual Studio maken voor een webtoepassing.

![Maken van Web pakket implementeren](./media/vs-azure-tools-publishing-using-powershell-scripts/IC767885.png)

Zie voor meer informatie [hoe: Web implementatiepakket maken in Visual Studio](https://msdn.microsoft.com/library/dd465323.aspx). U kunt ook het maken van uw pakket Web implementeren automatiseren zoals is beschreven in de sectie **aanpassen en de scripts publiceren uitbreiden** verderop in dit onderwerp.

1. Open het contextmenu voor het script in **Solution Explorer**en kiest u **openen met filter wissen**.

1. Als dit de eerste keer dat u Windows PowerShell-scripts op deze computer hebt uitgevoerd, open een opdrachtpromptvenster met beheerdersbevoegdheden en typt u de volgende opdracht uit:

`Set-ExecutionPolicy RemoteSigned`

1. Meld u aan bij Azure met behulp van de volgende opdracht uit.

`Add-AzureAccount`

Wanneer u wordt gevraagd, Voer uw gebruikersnaam en wachtwoord.

Houd er rekening mee dat wanneer u het script automatiseren, deze methode aan te bieden Azure referenties werkt niet. In plaats daarvan, moet u het bestand .publishsettings referenties op te geven. Één keer alleen u gebruikt de opdracht **Get-AzurePublishSettingsFile** naar het bestand downloaden via Azure, en daarna **Importeren-AzurePublishSettingsFile** gebruiken om het bestand te importeren. Zie voor gedetailleerde informatie kunt [installeren en configureren van Azure PowerShell](powershell-install-configure.md).

1. (Optioneel) Als u maken van Azure resources zoals de VM, database en website wilt zonder dat uw webtoepassing te publiceren, gebruikt u de opdracht **Publiceren-WebApplication.ps1** met de **-configuratie** argument ingesteld op het configuratiebestand JSON. Het configuratiebestand JSON deze opdrachtregel gebruikt om te bepalen welke bronnen die u wilt maken. Omdat hierin worden de standaardinstellingen voor andere opdrachtregelargumenten gebruikt, wordt gemaakt van de resources, maar niet de webtoepassing publiceren. Het gedeelte-uitgebreid optie vindt u meer informatie over wat er gebeurt.

`Publish-WebApplication.ps1 -Verbose –Configuration C:\Path\WebProject-WAWS-dev.json`

1. Gebruik de opdracht **Publiceren-WebApplication.ps1** zoals weergegeven in een van de volgende voorbeelden het script starten en het publiceren van uw webtoepassing. Als u de standaardinstellingen voor een van de andere argumenten, zoals de naam van het abonnement overschrijven, publiceren pakketnaam, VM referenties of referenties voor de database wilt, kunt u deze parameters. Gebruik de **– uitgebreid** optie voor meer informatie over de voortgang van het publicatieproces.

```
Publish-WebApplication.ps1 –Configuration C:\Path\WebProject-WAWS-dev-json `
–SubscriptionName Contoso `
-WebDeployPackage C:\Documents\Azure\ADWebApp.zip `
-DatabaseServerPassword @{Name="dbServerName";Password="adminPassword"} `
-Verbose
```

Als u een virtuele machine maakt, wordt de opdracht ziet eruit als volgt te werk. In dit voorbeeld ziet u ook de referenties voor meerdere databases opgeven. Voor de virtuele machines die deze scripts maakt, is het SSL-certificaat niet uit een vertrouwde basiscertificeringsinstantie. Daarom moet u de optie **– AllowUntrusted** gebruiken.

```
Publish-WebApplication.ps1 `
-Configuration C:\Path\ADVM-VM-test.json `
-SubscriptionName Contoso `
-WebDeployPackage C:\Path\ADVM.zip `
-AllowUntrusted `
-VMPassword @{name = "vmUserName"; password = "YourPasswordHere"} `
-DatabaseServerPassword @{Name="server1";Password="adminPassword1"}, @{Name="server2";Password="adminPassword2"} `
-Verbose
```

Het script databases kunt maken, maar er geen databaseservers gemaakt. Als u maken van een database-server wilt, kunt u de functie **Nieuw AzureSqlDatabaseServer** in de Azure module.

## <a name="customizing-and-extending-the-publish-scripts"></a>Aanpassen en uitbreiden van de scripts publiceren

U kunt de publiceren script en JSON configuratiebestand aanpassen. De functies in de Windows PowerShell-module **AzureWebAppPublishModule.psm1** niet moeten worden gewijzigd. Als u alleen wilt opgeven van een andere database of enkele eigenschappen van de virtuele machine wijzigen, de JSON configuratie-bestand bewerken. Als u de functionaliteit van het script maken en testen van uw project automatiseren uitbreidt wilt, kunt u de functie stubs implementeren in **Publiceren-WebApplication.ps1**.

Als u wilt automatiseren met het samenstellen van uw project, code die MSBuild naar belt toevoegen `New-WebDeployPackage` zoals in dit codevoorbeeld. Het pad naar de opdracht MSBuild is niet hetzelfde zijn afhankelijk van de versie van Visual Studio die u hebt geïnstalleerd. Als u het juiste pad, kunt u de functie **Get-MSBuildCmd**, zoals in dit voorbeeld.

### <a name="to-automate-building-your-project"></a>Automatiseren met het samenstellen van uw project

1. Toevoegen de `$ProjectFile` parameter in de sectie globale parameter.

```
[Parameter(Mandatory = $false)]
  [ValidateScript({Test-Path $_ -PathType Leaf})]
  [String]
  $ProjectFile,
```

1. Kopieer de functie `Get-MSBuildCmd` in een scriptbestand.

```
function Get-MSBuildCmd
{
        process
{

             $path =  Get-ChildItem "HKLM:\SOFTWARE\Microsoft\MSBuild\ToolsVersions\" |
                                   Sort-Object {[double]$_.PSChildName} -Descending |
                                   Select-Object -First 1 |
                                   Get-ItemProperty -Name MSBuildToolsPath |
                                   Select -ExpandProperty MSBuildToolsPath
       
            $path = (Join-Path -Path $path -ChildPath 'msbuild.exe')

        return Get-Item $path
    }
}
```

1. Vervang `New-WebDeployPackage` met de volgende code en vervang de tijdelijke aanduidingen in de regel het bouwen van `$msbuildCmd`. Deze code is voor Visual Studio-2015. Als u Visual Studio 2013 gebruikt, wijzigt u de eigenschap **VisualStudioVersion** onder naar `12.0`.

```
function New-WebDeployPackage
{
    #Write a function to build and package your web application
      
#To build your web application, use MsBuild.exe. For help, see MSBuild Command-Line Reference at: http://go.microsoft.com/fwlink/?LinkId=391339
      
Write-VerboseWithTime 'Build-WebDeployPackage: Start'
      
$msbuildCmd = '"{0}" "{1}" /T:Rebuild;Package /P:VisualStudioVersion=14.0 /p:OutputPath="{2}\MSBuildOutputPath" /flp:logfile=msbuild.log,v=d' -f (Get-MSBuildCmd), $ProjectFile, $scriptDirectory
      
Write-VerboseWithTime ('Build-WebDeployPackage: ' + $msbuildCmd)
      
#Start execution of the build command
$job = Start-Process cmd.exe -ArgumentList('/C "' + $msbuildCmd + '"') -WindowStyle Normal -Wait -PassThru
      
if ($job.ExitCode -ne 0)
{
throw('MsBuild exited with an error. ExitCode:' + $job.ExitCode)
}

#Obtain the project name
$projectName = (Get-Item $ProjectFile).BaseName
      
#Construct the path to web deploy zip package
$DeployPackageDir =  '.\MSBuildOutputPath\_PublishedWebsites\{0}_Package\{0}.zip' -f $projectName
      
      
#Get the full path for the web deploy zip package. This is required for MSDeploy to work
$WebDeployPackage = Resolve-Path –LiteralPath $DeployPackageDir
      
Write-VerboseWithTime 'Build-WebDeployPackage: End'
      
return $WebDeployPackage
}
```

1. Bellen de `New-WebDeployPackage` functie voordat u deze regel: `$Config = Read-ConfigFile $Configuration` voor WebApps of `$Config = Read-ConfigFile $Configuration -HasWebDeployPackage:([Bool]$WebDeployPackage)` voor virtuele machines.

```
if($ProjectFile)
{
$WebDeployPackage = New-WebDeployPackage
}
```

1. Roepen de aangepast script van de opdrachtregel doorgeven met de `$Project` argument, zoals in het volgende voorbeeld-opdrachtregel.

```
.\Publish-WebApplicationVM.ps1 -Configuration .\Configurations\WebApplication5-VM-dev.json `
-ProjectFile ..\WebApplication5\WebApplication5.csproj `
-VMPassword @{Name="VMUser";Password="Test.123"} `
-AllowUntrusted `
-Verbose
```

Als u wilt automatiseren testen van de toepassing, code toevoegen aan `Test-WebApplication`. Zorg ervoor dat u Verwijder de opmerkingen bij de regels in **Publiceren WebApplication.ps1** waar deze functies worden genoemd. Als u een implementatie niet opgeeft, kunt u uw project met Visual Studio handmatig maakt en voer het script publiceren publiceren naar Azure.

## <a name="publishing-function-summary"></a>Publicerende functie samenvatting

Voor informatie over functies die u bij de opdrachtprompt van Windows PowerShell gebruiken kunt, gebruikt u de opdracht `Get-Help function-name`. De help bevat parameter help en voorbeelden. De dezelfde help-informatie is ook in de bronbestanden script, **AzureWebAppPublishModule.psm1** en **Publiceren-WebApplication.ps1**. Het script en help zijn gelokaliseerd in de Visual Studio-taal.

**AzureWebAppPublishModule**

|Functienaam|Beschrijving|
|---|---|
|Toevoegen AzureSQLDatabase|Hiermee maakt u een nieuwe Azure SQL-database.|
|Toevoegen AzureSQLDatabases|Hiermee maakt u Azure SQL-databases van de waarden in de JSON configuation-bestand dat Visual Studio wordt gegenereerd.|
|Toevoegen AzureVM|Hiermee maakt u een Azure virtuele machine en geeft als resultaat de URL van de geïmplementeerd VM. De functie stelt de vereisten en vervolgens wordt de functie **Nieuw-AzureVM** (Azure module) als u wilt maken van een nieuwe virtuele machine.|
|Toevoegen AzureVMEndpoints|Nieuwe invoer eindpunten toevoegt aan een virtuele machine en geeft als resultaat de virtuele machine met het nieuwe eindpunt.|
|Toevoegen AzureVMStorage|Hiermee maakt u een nieuw Azure opslag-account in het huidige abonnement. De naam van het account begint met "devtest", gevolgd door een unieke, alfanumerieke tekenreeks. De functie retourneert de naam van het nieuwe opslag-account. Moet u een locatie of een groep affiniteit voor de nieuwe opslag-account.|
|Toevoegen AzureWebsite|Hiermee maakt een website met de opgegeven naam en locatie. Deze functie wordt de functie **Nieuw AzureWebsite** in de Azure module. Als het abonnement dat niet al een website met de opgegeven naam bevat, wordt deze functie wordt gemaakt van de website en een website retourneert. Anders is het resultaat `$null`.|
|Back-up-abonnement|Slaat het huidige Azure abonnement in de `$Script:originalSubscription` variabele in script bereik. Deze functie wordt het huidige Azure abonnement opgeslagen (zoals verkregen doordat `Get-AzureSubscription -Current`) en de opslag-account en het abonnement dat is gewijzigd door dit script (die zijn opgeslagen in de variabele `$UserSpecifiedSubscription`) en de opslag-account, in script bereik. Door de waarden op te slaan, kunt u een functie, zoals `Restore-Subscription`, om te zetten van de oorspronkelijke account voor het abonnement en opslag van huidige naar de huidige status als de huidige status is gewijzigd.|
|Zoeken naar AzureVM|De opgegeven Azure virtuele machine krijgt.|
|Indeling-DevTestMessageWithTime|Voegt toe de datum en tijd aan een bericht. Deze functie is bedoeld voor berichten die naar de fout en uitgebreid-streams geschreven.|
|Get-AzureSQLDatabaseConnectionString|Samengevoegd naast een verbindingsreeks verbinding maken met een Azure SQL-database.|
|Get-AzureVMStorage|Retourneert de naam van de eerste opslag rekening met het naampatroon dat in de "devtest*" (hoofdlettergevoelig) in de opgegeven locatie of affiniteit-groep. Als de "devtest*" opslag account niet overeenkomen met de locatie of affiniteit groep, de functie deze genegeerd. U moet een locatie of een groep affiniteit opgeven.|
|Get-MSDeployCmd|Geeft als resultaat een opdracht kiezen om het hulpprogramma MsDeploy.exe uitvoeren.|
|Nieuwe AzureVMEnvironment|Vindt u maakt of een virtuele machine in het abonnement dat overeenkomt met de waarden in het configuratiebestand JSON.|
|Publiceren WebPackage|Gebruik MsDeploy.exe en een web publiceren-pakket. ZIP-bestand resources implementeren naar een website. Deze functie niet uitvoer gegenereerd. Als de oproep door naar MSDeploy.exe mislukt, wordt met de functie een uitzondering genereert. Als u meer gedetailleerde uitvoer, gebruikt de **-uitgebreide** optie.|
|Publiceren WebPackageToVM|Wordt gecontroleerd met de betreffende parameterwaarden en vervolgens wordt de functie **Publiceren WebPackage** .|
|Alleen-ConfigFile|Het configuratiebestand JSON is gevalideerd en geeft als resultaat een hashtabel van geselecteerde waarden.|
|Herstellen-abonnement|Hiermee stelt u het huidige abonnement naar het oorspronkelijke abonnement opnieuw.|
|Test-AzureModule|Geeft als resultaat `$true` als de geïnstalleerde Azure moduleversie niet 0.7.4 wordt of hoger. Geeft als resultaat `$false` als de module niet is geïnstalleerd of een eerdere versie. Deze functie heeft geen parameters.|
|Test-AzureModuleVersion|Geeft als resultaat `$true` als de versie van de Azure module 0.7.4 is of hoger. Geeft als resultaat `$false` als de module niet is geïnstalleerd of een eerdere versie. Deze functie heeft geen parameters.|
|Test-HttpsUrl|Converteert de invoer-URL naar een object System.Uri. Geeft als resultaat `$True` als de URL absoluut is en bijbehorende schema https is. Geeft als resultaat `$false` als de URL relatief is, de kleurenschema niet HTTPS wordt of de ingevoerde tekenreeks kan niet worden geconverteerd naar een URL.|
|Test-lid|Geeft als resultaat `$true` als een eigenschap of methode deel van het object uitmaakt. Anders, is het resultaat `$false`.|
|Schrijven-ErrorWithTime|Hiermee worden voorafgegaan door de huidige tijd een foutbericht wordt weergegeven. Deze functie wordt de functie **Format-DevTestMessageWithTime** om de tijd voordat u het bericht schrijft naar de stream fout Voeg.|
|Schrijven-HostWithTime|Een bericht schrijft voor het hostprogramma (**Write-Host**) voorafgegaan door de huidige tijd. Het effect van het schrijven naar het hostprogramma varieert. De meeste programma's die host Windows PowerShell schrijven deze berichten naar standaard uitvoer.|
|Schrijven-VerboseWithTime|Wordt een uitgebreide voorafgegaan door de huidige tijd. Omdat deze **Schrijven uitgebreid**u belt, verschijnt het bericht alleen als het script wordt uitgevoerd met de parameter **uitgebreid** of als de voorkeur **VerbosePreference** is ingesteld op **Doorgaan**.|

**Publiceren webtoepassing**

|Functienaam|Beschrijving|
|---|---|
|Nieuwe AzureWebApplicationEnvironment|Hiermee maakt u Azure resources, zoals een website of virtuele machine.|
|Nieuwe WebDeployPackage|Deze functie is niet geïmplementeerd. In deze functie voor het maken van uw project, kunt u opdrachten toevoegen.|
|Publiceren AzureWebApplication|Een webtoepassing Azure publiceert.|
|Publiceren webtoepassing|Hiermee maakt u en implementeren van Web Apps, virtuele machines SQL-databases en opslag-accounts voor een Visual Studio-web-project.|
|Test-webtoepassing|Deze functie is niet geïmplementeerd. U kunt opdrachten toevoegen aan deze functie om uw toepassing te testen.|

## <a name="next-steps"></a>Volgende stappen

Meer informatie over de PowerShell-scripts door te lezen [uitvoeren van scripts met Windows PowerShell](https://technet.microsoft.com/library/bb978526.aspx) en andere Azure PowerShell-scripts, in het [Script Center](https://azure.microsoft.com/documentation/scripts/)zien.
