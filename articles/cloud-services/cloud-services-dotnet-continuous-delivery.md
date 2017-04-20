<properties
    pageTitle="Continue bezorging voor cloud services met TFS in Azure | Microsoft Azure"
    description="Informatie over het instellen van continue bezorging voor Azure cloud-apps. Voorbeelden van de code voor de opdrachtregel instructies MSBuild en PowerShell-scripts."
    services="cloud-services"
    documentationCenter=""
    authors="TomArcher"
    manager="douge"
    editor=""/>

<tags
    ms.service="cloud-services"
    ms.workload="tbd"
    ms.tgt_pltfrm="na"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="07/30/2016"
    ms.author="tarcher"/>

# <a name="continuous-delivery-for-cloud-services-in-azure"></a>Continue bezorging voor Cloudservices in Azure wordt aangegeven

Het proces wordt beschreven in dit artikel leest u hoe voor het instellen van continue bezorging voor Azure cloud-apps. Deze procedure kunt u automatisch pakketten maken en implementeren in het pakket Azure na elke code inchecken. Het pakket opbouwen-proces beschreven in dit artikel is gelijk aan de opdracht **pakket** in Visual Studio en de publicatie stappen komen overeen met de opdracht **publiceren** in Visual Studio.
Het artikel behandelt de methoden die u gebruikt voor het maken van een server opbouwen MSBuild opdrachtregel instructies en Windows PowerShell-scripts en u ziet ook hoe u desgewenst configureert Visual Studio Team Foundation Server - Team samenstellen definities de MSBuild opdrachten en de PowerShell-scripts gebruiken. Het proces kan worden aangepast voor uw opbouwomgeving en Azure doel-omgevingen.

U kunt ook Visual Studio Team Services, een versie van TFS die wordt gehost in Azure wordt aangegeven, eenvoudiger hiervoor gebruiken. Zie [Continue bezorging bij Azure door gebruik van Visual Studio Team Services][]voor meer informatie.

Voordat u begint, moet u de toepassing Visual Studio publiceren.
Dit zorgt ervoor dat alle de resources zijn alleen beschikbaar en geïnitialiseerd wanneer u probeert om de publicatieproces te automatiseren.

## <a name="1-configure-the-build-server"></a>1: de Server opbouwen configureren

Voordat u een Azure-pakket maken kunt met behulp van MSBuild, moet u de vereiste software en hulpprogramma's voor installeren op de server opbouwen.

Visual Studio is niet moeten worden geïnstalleerd op de server opbouwen. Als u gebruiken Team Foundation bouwen-Service wilt voor het beheren van uw server opbouwen, volgt u de instructies in de servicedocumentatie [Team Foundation maken][] .

1.  Op de server opbouwen, installeert u de [.NET Framework 4.5.2][], inclusief MSBuild.
2.  Installeer de meest recente [Azure Authoring hulpprogramma's voor .NET](https://azure.microsoft.com/develop/net/).
3.  Installeer de [Azure bibliotheken voor .NET](http://go.microsoft.com/fwlink/?LinkId=623519).
4.  Kopieer het bestand Microsoft.WebApplication.targets van een Visual Studio-installatie op de server opbouwen.

    Klik op een computer met Visual Studio is geïnstalleerd, in dit bestand zich bevindt in de map C:\\programma Files(x86)\\MSBuild\\Microsoft\\VisualStudio\\v14.0\\WebApplications. U moet deze kopiëren naar dezelfde map op de server opbouwen.
5.  Installeer de [Azure Tools voor Visual Studio](https://www.visualstudio.com/features/azure-tools-vs.aspx).

## <a name="2-build-a-package-using-msbuild-commands"></a>2: een pakket met MSBuild opdrachten maken

In deze sectie wordt beschreven hoe een MSBuild-opdracht een Azure-pakket genereert maken. Deze stap uitvoeren op de server opbouwen om te controleren of alles correct is geconfigureerd en dat de opdracht MSBuild wat u wilt is laten doen. U kunt deze opdrachtregel toevoegen aan bestaande opbouwen scripts op de server opbouwen of u kunt de opdrachtregel in een definitie TFS maken, zoals wordt beschreven in de volgende sectie. Zie voor meer informatie over opdrachtregelparameters en MSBuild [MSBuild opdrachtregel verwijzing](https://msdn.microsoft.com/library/ms164311%28v=vs.140%29.aspx).

1.  Als Visual Studio is geïnstalleerd op de server opbouwen, zoeken en kiest u **Visual Studio opdracht vragen** in de **Visual Studio Tools** map in Windows.

    Als Visual Studio niet is geïnstalleerd op de server opbouwen, open een opdrachtprompt en zorg ervoor dat MSBuild.exe toegankelijk is op het pad. MSBuild is geïnstalleerd met .NET Framework in het pad % WINDIR %\\Microsoft.NET\\Framework\\*versie*. Typ bijvoorbeeld de volgende opdracht uit als u wilt toevoegen aan de omgevingsvariabele PATH MSBuild.exe als er .NET Framework 4 is geïnstalleerd, bij de opdrachtprompt:

        set PATH=%PATH%;"C:\Windows\Microsoft.NET\Framework\v4.0.30319"

2.  Bij de opdrachtprompt, navigeer naar de map met het Azure projectbestand dat u wilt maken.

3.  MSBuild uitvoeren met de/target: publiceren optie zoals in het volgende voorbeeld:

        MSBuild /target:Publish

    Deze optie kunt afgekort /t: publiceren. De optie /t:Publish in MSBuild moet niet verwarren met de opdrachten publiceren beschikbaar in Visual Studio wanneer u de SDK Azure is geïnstalleerd. De /t: optie alleen-versies van de Azure-pakketten publiceren. Deze implementeert geen de pakketten zoals de opdrachten publiceren in Visual Studio doen.

    Desgewenst kunt u de naam van het project als een parameter MSBuild opgeven. Als u niet opgeeft, wordt de huidige map wordt gebruikt. Zie voor meer informatie over de opdrachtregelopties MSBuild, [MSBuild opdrachtregel verwijzing](https://msdn.microsoft.com/library/ms164311%28v=vs.140%29.aspx).

4.  Zoek de uitvoer. Deze opdracht maakt standaard een map ten opzichte van de hoofdmap voor het project, zoals *ProjectDir*\\opslaglocatie\\*configuratie*\\app.publish\\. Wanneer u een Azure project maakt, genereert u twee bestanden, het pakketbestand zelf en de bijbehorende configuratiebestand:

    -   Project.cspkg
    -   ServiceConfiguration. *TargetProfile*.cscfg

    Standaard elk Azure project bevat één configuratie servicebestand (.cscfg-bestand) voor lokale (foutopsporing)-versies en een andere voor de cloud (staging- of)-versies, maar u kunt toevoegen of verwijderen van service configuratiebestanden naar wens. Als u een pakket in Visual Studio samenstelt, krijgt u welke configuratiebestand service om op te nemen samen met het pakket.

5.  Geef het configuratiebestand service. Als u een pakket samenstelt met behulp van MSBuild, kan het configuratiebestand lokale service standaard is opgenomen. Als u wilt opnemen in een andere service configuratiebestand, stelt u de eigenschap TargetProfile van de opdracht MSBuild, zoals in het volgende voorbeeld:

        MSBuild /t:Publish /p:TargetProfile=Cloud

6.  Geef de locatie voor de uitvoer. Het pad instellen met behulp van de /p:PublishDir =*map* \\ optie, inclusief het afsluitende backslash-scheidingsteken, zoals in het volgende voorbeeld:

        MSBuild /target:Publish /p:PublishDir=\\myserver\drops\

    Zodra u hebt opgesteld en een juiste MSBuild opdrachtregel als u wilt uw projecten maken en deze combineren in een pakket met Azure getest, kunt u deze opdrachtregel toevoegen aan uw scripts opbouwen. Als uw server opbouwen gebruikmaakt van aangepaste scripts, wordt dit proces afhankelijk van de details van uw aangepaste opbouwen-proces. Als u TFS als een opbouwomgeving gebruikt, kunt u de instructies in de volgende stap het bouwen van Azure-pakket toevoegen aan uw opbouwen proces volgen.

## <a name="3-build-a-package-using-tfs-team-build"></a>3: maken van een pakket met TFS Team samenstellen

Als u hebt Team Foundation Server (TFS) is ingesteld als een controller opbouwen en de server opbouwen instellen als een TFS opbouwen machine en klikt u desgewenst een geautomatiseerde opbouwen kunt instellen voor uw Azure-pakket. Zie voor informatie over het instellen en gebruiken van Team Foundation-server als een systeem opbouwen [schaal uw systeem opbouwen][]. Met name de volgende procedure wordt ervan uitgegaan dat u de server is geconfigureerd opbouwen zoals is beschreven in [Deploy en configureren van een server opbouwen][], en dat u een teamproject hebt gemaakt een cloud-service-project in het teamproject gemaakt.

Als u wilt configureren TFS om te maken van Azure-pakketten, moet u de volgende stappen uitvoeren:

1.  Kies **Team Explorer**in Visual Studio op de computer, in het menu Beeld of kiest u Ctrl +\\, Ctrl + M. Vouw het knooppunt **genereert** in het Team Verkenner-venster of kiest u de pagina **genereert** en kies **Nieuwe definitie maken**.

    ![Optie voor nieuwe definitie maken][0]

2.  Kies het tabblad **Trigger** en geef de gewenste voorwaarden voor als u wilt dat het pakket worden opgebouwd. Bijvoorbeeld opgeven **Continue integratie** om te maken van het pakket wanneer een bron besturingselement inchecken plaatsvindt.

3.  Kies het tabblad **Instellingen voor gegevensbron** en controleer of uw projectmap wordt weergegeven in de kolom **Bron besturingselement map** en de status **actief**is.

4.  Kies het tabblad **Standaardwaarden maken** en controleer onder opbouwen controller, of de naam van de server opbouwen.  Ook, kies de optie **kopie uitvoer naar de volgende decoratieve map maken** en de locatie van de gewenste decoratieve opgeven.

5.  Kies het tabblad **proces** . Klik op het tabblad proces kiest u de standaardsjabloon, onder **maken**, kiest u het project als deze nog niet is geselecteerd en vouwen van de sectie **Geavanceerd** in de sectie voor het **maken** van het raster.

6.  Kies **MSBuild argumenten**en de juiste MSBuild-opdrachtregelargumenten instellen, zoals beschreven in stap 2. Voer bijvoorbeeld **/t: publiceren /p:PublishDir =\\\\MijnServer\\worden\\ ** voor bouwen van een pakket en de pakketbestanden kopiëren naar de locatie \\ \\MijnServer\\worden\\:

    ![MSBuild argumenten][2]

    **Notitie:** De bestanden kopiëren naar een openbare share vergemakkelijkt handmatig implementeren de pakketten vanaf uw computer ontwikkeling.

5.  Het succes van uw stap opbouwen testen door te schakelen in een wijziging aan uw project, of de rij van een nieuwe build. Als u wilt de wachtrij plaatsen een nieuwe build, in de Verkenner Team met de rechtermuisknop op **Alle definities van maken,** en kies vervolgens **Wachtrij nieuwe maken**.

## <a name="4-publish-a-package-using-a-powershell-script"></a>4: een pakket met een PowerShell-Script publiceren

In deze sectie wordt beschreven hoe een Windows PowerShell-script dat de uitvoer van Cloud-app-pakket publiceert op Azure optionele parameters gebruiken om te maken. Dit script kan worden aangeroepen nadat de build stap in uw aangepaste opbouwen automatisering. Dit kan ook worden aangeroepen vanuit de werkstroomactiviteiten processjabloon in Visual Studio TFS Team samenstellen.

1.  Installeren van de [Azure PowerShell-cmdlets][] (v0.6.1 of hoger).
    Tijdens de cmdlet setup-fase wilt installeren als een module. Houd er rekening mee dat deze officieel ondersteunde versie Hiermee vervangt u de oudere versie beschikbaar via CodePlex, hoewel de vorige versies zijn 2.x.x genummerd.

2.  Azure PowerShell via het menu Start te starten of pagina. Als u op deze manier start, worden de Azure PowerShell-cmdlets geladen.

3.  Controleren of de PowerShell-cmdlets zijn geladen door in te voeren de gedeeltelijke opdracht bij de prompt PowerShell `Get-Azure` en drukt u op de Tab-toets voor de voltooiing van instructie.

    Als u meermaals op de Tab-toets drukt, ziet u verschillende Azure PowerShell-opdrachten.

4.  Controleer of dat u verbinding met uw Azure-abonnement maken kunt door de informatie over uw abonnement uit de .publishsettings-bestand importeren.

    `Import-AzurePublishSettingsFile c:\scripts\WindowsAzure\default.publishsettings`

    Voer vervolgens de opdracht

    `Get-AzureSubscription`

    Hiermee worden gegevens over uw abonnement. Controleer of alles klopt.

4.  Sla de script-sjabloon gegeven aan het eind van dit artikel naar de scriptmap als c:\\scripts\\WindowsAzure\\**PublishCloudService.ps1**.

5.  Raadpleeg de sectie parameters van het script. Toevoegen of wijzigen van de standaardwaarden. Deze waarden kunnen altijd worden overschreven door doorgeven in expliciete parameters.

6.  Controleer of er zijn geldig cloud-service en opslag accounts gemaakt in uw abonnement die kan worden gericht door het script publiceren. Het opslag-account (blobopslag) wordt gebruikt om te uploaden en de implementatie-pakket en config-bestand tijdelijk op te slaan terwijl de implementatie wordt gemaakt.

    -   Als u wilt een nieuwe cloudservice hebt gemaakt, kunt u dit script belt of het gebruik van de [Azure klassieke portal](http://go.microsoft.com/fwlink/?LinkID=213885). De naam van de cloud-service wordt gebruikt als een voorvoegsel in een volledig gekwalificeerde domeinnaam en dus deze moet uniek zijn.

            New-AzureService -ServiceName "mytestcloudservice" -Location "North Central US" -Label "mytestcloudservice"

    -   Om een nieuwe opslag-account maken, kunt u dit script belt of het gebruik van de [Azure klassieke portal](http://go.microsoft.com/fwlink/?LinkID=213885). De naam van het opslag-account wordt gebruikt als een voorvoegsel in een volledig gekwalificeerde domeinnaam en dus deze moet uniek zijn. U kunt proberen met dezelfde naam als de cloudservice.

            New-AzureStorageAccount -ServiceName "mytestcloudservice" -Location "North Central US" -Label "mytestcloudservice"

7.  Het script bellen rechtstreeks vanuit Azure PowerShell, of op wire dit script aan de host opbouwen automatisering na het pakket opbouwen.

    >[AZURE.IMPORTANT] Het script wordt altijd verwijderen of vervangen van uw bestaande implementaties al dan niet standaard als ze zijn gevonden. Dit is nodig om in te schakelen continue bezorging bij automatisering geen gebruiker waarin u wordt gevraagd indien mogelijk is.

    **Voorbeeldscenario 1:** continue implementatie naar de testomgeving van een service:

        PowerShell c:\scripts\windowsazure\PublishCloudService.ps1 -environment Staging -serviceName mycloudservice -storageAccountName mystoragesaccount -packageLocation c:\drops\app.publish\ContactManager.Azure.cspkg -cloudConfigLocation c:\drops\app.publish\ServiceConfiguration.Cloud.cscfg -subscriptionDataFile c:\scripts\default.publishsettings

    Dit is meestal opgevolgd door test verificatie en een omwisselen VIP uitvoeren. Het omwisselen VIP kan plaatsvinden via de [portal van Azure klassieke](http://go.microsoft.com/fwlink/?LinkID=213885) of met behulp van de cmdlet verplaatsen-implementatie.

    **Voorbeeldscenario 2:** continue implementatie naar de productieomgeving van een speciale test-service

        PowerShell c:\scripts\windowsazure\PublishCloudService.ps1 -environment Production -enableDeploymentUpgrade 1 -serviceName mycloudservice -storageAccountName mystorageaccount -packageLocation c:\drops\app.publish\ContactManager.Azure.cspkg -cloudConfigLocation c:\drops\app.publish\ServiceConfiguration.Cloud.cscfg -subscriptionDataFile c:\scripts\default.publishsettings

    **Extern bureaublad:**

    Als u extern bureaublad is ingeschakeld in uw Azure project die u moet extra eenmalige stappen om te zorgen dat het juiste certificaat van de Cloud-Service wordt geüpload naar alle cloudservices gericht door dit script uitvoeren.

    Zoek het certificaat vingerafdruk waarden door uw rollen verwacht. De waarden vingerafdruk zijn zichtbaar in de sectie certificaten van het bestand in de cloud-config (dat wil zeggen ServiceConfiguration.Cloud.cscfg). Het is ook zichtbaar in het dialoogvenster configuratie voor extern bureaublad in Visual Studio wanneer u opties en het geselecteerde certificaat weergeven.

        <Certificates>
              <Certificate name="Microsoft.WindowsAzure.Plugins.RemoteAccess.PasswordEncryption" thumbprint="C33B6C432C25581601B84C80F86EC2809DC224E8" thumbprintAlgorithm="sha1" />
        </Certificates>

    Upload extern bureaublad certificaten als een eenmalige instellingsstap met de volgende cmdlet-script:

        Add-AzureCertificate -serviceName <CLOUDSERVICENAME> -certToDeploy (get-item cert:\CurrentUser\MY\<THUMBPRINT>)

    Bijvoorbeeld:

        Add-AzureCertificate -serviceName 'mytestcloudservice' -certToDeploy (get-item cert:\CurrentUser\MY\C33B6C432C25581601B84C80F86EC2809DC224E8

    U kunt ook het certificaatbestand PFX met persoonlijke sleutel exporteren en certificaten uploaden naar elke doel-cloudservice met behulp van de [Azure klassieke portal](http://go.microsoft.com/fwlink/?LinkID=213885). 
    Lees de volgende artikel voor meer informatie: [http://msdn.microsoft.com/library/windowsazure/gg443832.aspx]-[].

    **Implementatie versus verwijderen implementatie - upgrade\> nieuwe implementatie**

    Het script wordt standaard een Upgrade implementatie uitvoeren ($enableDeploymentUpgrade = 1) wanneer er geen parameter wordt doorgegeven- of de waarde 1 expliciet is verstreken. Voor één exemplaren heeft dit het voordeel van minder tijd dan een volledige implementatie in beslag neemt. Voor exemplaren die opgevolgd beschikbaarheid die volgt, ook het voordeel moeten heeft van sommige gevallen uitvoeren terwijl u anderen verlaten worden bijgewerkt (lopen uw domein bijwerken), plus de VIP wordt niet verwijderd.

    Implementatie van de upgrade kan worden uitgeschakeld in het script ($enableDeploymentUpgrade = 0) of door te geven *- enableDeploymentUpgrade 0* als een parameter, waarin het script gedrag naar eerst Verwijder eventuele bestaande implementatie en maak vervolgens een nieuwe implementatie wijzigt.

    >[AZURE.IMPORTANT] Het script wordt altijd verwijderen of vervangen van uw bestaande implementaties al dan niet standaard als ze zijn gevonden. Dit is nodig om in te schakelen continue bezorging bij automatisering geen gebruiker/operator waarin u wordt gevraagd indien mogelijk is.

## <a name="5-publish-a-package-using-tfs-team-build"></a>5: publiceren van een pakket met TFS Team samenstellen

Deze eventueel maakt verbinding TFS Team samenstellen aan het script is gemaakt in stap 4, waarin de publicatie van de build pakket naar Azure verwerkt. Dit houdt in dat de sjabloon die worden gebruikt door de definitie van uw opbouwen, zodat deze wordt uitgevoerd de activiteit in een publiceren aan het einde van de werkstroom wijzigen. De activiteit publiceren, wordt uw PowerShell-opdracht doorgeven in parameters van het bouwen uitgevoerd. Uitvoer van de MSBuild-doelen en publiceren van script worden de eigenschappen in de opbouwuitvoer standaard.

1.  Bewerk de definitie maken die verantwoordelijk is voor doorlopende implementeren.

2.  Selecteer het tabblad **proces** .

3.  Volg [deze instructies](http://msdn.microsoft.com/library/dd647551.aspx) als u wilt toevoegen van een project activiteit voor de sjabloon opbouwen-proces, downloadt de standaardsjabloon, toe te voegen aan het project en schakelt u dit. Geef de opbouwen proces-sjabloon een nieuwe naam, bijvoorbeeld AzureBuildProcessTemplate.

3.  Ga terug naar het tabblad **proces** en **Details weergeven** om weer te geven van een lijst met beschikbare opbouwen processjablonen gebruiken. Kies de knop **Nieuw...** en Ga naar het project dat u zojuist hebt toegevoegd en ingecheckt. Zoek de sjabloon die u zojuist hebt gemaakt en kies **OK**.

4.  Open de geselecteerde processjabloon om te bewerken. U kunt rechtstreeks in de werkstroomontwerper of in de XML-editor voor gebruik met de XAML openen.

5.  De volgende lijst met nieuwe argumenten als afzonderlijk item op het tabblad van de argumenten van de werkstroomontwerper toevoegen. Alle argumenten moet richting = In en typt u = tekenreeks. Deze worden gebruikt op stroom parameters van de definitie opbouwen in de werkstroom, die vervolgens gebruikt ophalen om te bellen van het script publiceren.

        SubscriptionName
        StorageAccountName
        CloudConfigLocation
        PackageLocation
        Environment
        SubscriptionDataFileLocation
        PublishScriptLocation
        ServiceName

    ![Lijst met argumenten][3]

    De bijbehorende XAML ziet er zo uit:

        <Activity  _ />
          <x:Members>
            <x:Property Name="BuildSettings" Type="InArgument(mtbwa:BuildSettings)" />
            <x:Property Name="TestSpecs" Type="InArgument(mtbwa:TestSpecList)" />
            <x:Property Name="BuildNumberFormat" Type="InArgument(x:String)" />
            <x:Property Name="CleanWorkspace" Type="InArgument(mtbwa:CleanWorkspaceOption)" />
            <x:Property Name="RunCodeAnalysis" Type="InArgument(mtbwa:CodeAnalysisOption)" />
            <x:Property Name="SourceAndSymbolServerSettings" Type="InArgument(mtbwa:SourceAndSymbolServerSettings)" />
            <x:Property Name="AgentSettings" Type="InArgument(mtbwa:AgentSettings)" />
            <x:Property Name="AssociateChangesetsAndWorkItems" Type="InArgument(x:Boolean)" />
            <x:Property Name="CreateWorkItem" Type="InArgument(x:Boolean)" />
            <x:Property Name="DropBuild" Type="InArgument(x:Boolean)" />
            <x:Property Name="MSBuildArguments" Type="InArgument(x:String)" />
            <x:Property Name="MSBuildPlatform" Type="InArgument(mtbwa:ToolPlatform)" />
            <x:Property Name="PerformTestImpactAnalysis" Type="InArgument(x:Boolean)" />
            <x:Property Name="CreateLabel" Type="InArgument(x:Boolean)" />
            <x:Property Name="DisableTests" Type="InArgument(x:Boolean)" />
            <x:Property Name="GetVersion" Type="InArgument(x:String)" />
            <x:Property Name="PrivateDropLocation" Type="InArgument(x:String)" />
            <x:Property Name="Verbosity" Type="InArgument(mtbw:BuildVerbosity)" />
            <x:Property Name="Metadata" Type="mtbw:ProcessParameterMetadataCollection" />
            <x:Property Name="SupportedReasons" Type="mtbc:BuildReason" />
            <x:Property Name="SubscriptionName" Type="InArgument(x:String)" />
            <x:Property Name="StorageAccountName" Type="InArgument(x:String)" />
            <x:Property Name="CloudConfigLocation" Type="InArgument(x:String)" />
            <x:Property Name="PackageLocation" Type="InArgument(x:String)" />
            <x:Property Name="Environment" Type="InArgument(x:String)" />
            <x:Property Name="SubscriptionDataFileLocation" Type="InArgument(x:String)" />
            <x:Property Name="PublishScriptLocation" Type="InArgument(x:String)" />
            <x:Property Name="ServiceName" Type="InArgument(x:String)" />
          </x:Members>

          <this:Process.MSBuildArguments>

6.  Een nieuwe reeks toevoegen aan het einde van uitvoeren op Agent:

    1.  Begin met het toevoegen van een activiteit If-instructie om te controleren voor een geldige scriptbestand. De voorwaarde op deze waarde ingesteld:

            Not String.IsNullOrEmpty(PublishScriptLocation)

    2.  In de Then geval van de instructie als, een nieuwe sequentie-activiteit toevoegen. De weergavenaam 'Start publiceren' instellen

    3.  Met het starten publiceren sequentie nog steeds is geselecteerd, voegt u de volgende lijst met nieuwe variabelen als afzonderlijke lijn items in het tabblad variabelen van de werkstroomontwerper. Alle variabelen type variabele moeten hebben = tekenreeks en Scope = begindatum publiceren. Deze worden gebruikt op stroom parameters van de definitie opbouwen in de werkstroom, die vervolgens gebruikt ophalen om te bellen van het script publiceren.

        -   SubscriptionDataFilePath, van het type tekenreeks

        -   PublishScriptFilePath, van het type tekenreeks

            ![Nieuwe variabelen][4]

    4.  Als u TFS 2012 of eerder gebruikt, moet u de activiteit van een ConvertWorkspaceItem toevoegen aan het begin van de nieuwe volgorde. Als u TFS 2013 of hoger gebruikt, moet u de activiteit van een GetLocalPath toevoegen aan het begin van de nieuwe volgorde. Voor een ConvertWorkspaceItem, stel de eigenschappen als volgt: richting = ServerToLocal, weergavenaam = 'Converteren publiceert script bestandsnaam', invoer = PublishScriptLocation, resultaat = PublishScriptFilePath, werkruimte = 'Werkruimte'. Stel de eigenschap IncomingPath naar 'PublishScriptLocation' en het resultaat om te 'PublishScriptFilePath' voor een activiteit in een GetLocalPath. Deze activiteit converteert het pad naar het script publiceren van TFS serverlocaties (indien beschikbaar) naar een pad standaard lokale schijf.

    5.  Als u TFS 2012 of eerder gebruikt, kunt u een andere ConvertWorkspaceItem-activiteit toevoegen aan het einde van de nieuwe volgorde. Richting = ServerToLocal, weergavenaam = 'Converteren abonnement filename', invoer = SubscriptionDataFileLocation, resultaat = SubscriptionDataFilePath, werkruimte = 'Werkruimte'. Als u TFS 2013 of hoger gebruikt, voegt u een andere GetLocalPath toe. IncomingPath = 'SubscriptionDataFileLocation', en resultaat = 'SubscriptionDataFilePath'.

    6.  Een activiteit InvokeProcess toevoegen aan het einde van de nieuwe volgorde.
        Deze activiteit oproepen PowerShell.exe van de argumenten doorgegeven door de definitie maken.

        1.  Argumenten String.Format = ("-bestand""{0}" "- servicenaam {1} - storageAccountName {2} - packageLocation""{3}" "- cloudConfigLocation""{4}" "- subscriptionDataFile""{5}" "- selectedSubscription {6}-omgeving""{7}" "", PublishScriptFilePath, servicenaam, StorageAccountName, PackageLocation, CloudConfigLocation, SubscriptionDataFilePath, SubscriptionName, omgeving)

        2.  Weergavenaam = Execute script publiceren

        3.  FileName = "PowerShell" (inclusief de aanhalingstekens)

        4.  OutputEncoding = System.Text.Encoding.GetEncoding(System.Globalization.CultureInfo.InstalledUICulture.TextInfo.OEMCodePage)

    7.  Stel de waarde van het tekstvak naar 'gegevens' in het tekstvak van de sectie **Standaarduitvoer verwerken** van de InvokeProcess. Dit is een variabele voor de opslag van de standaard uitvoergegevens.

    8.  Een activiteit WriteBuildMessage vlak onder de sectie **Standaarduitvoer verwerken** toevoegen. De urgentie = 'Microsoft.TeamFoundation.Build.Client.BuildMessageImportance.High' en het bericht = 'gegevens'. Dit zorgt ervoor dat de standaard uitvoer van het script wordt naar de opbouwuitvoer krijgen geschreven.

    9.  Stel in het tekstvak van de sectie **Verwerken fout uitvoer** van de InvokeProcess te 'gegevens' de waarde van het tekstvak. Dit is een variabele voor de opslag van de standaardfout-gegevens.

    10. Een activiteit WriteBuildError vlak onder de sectie **Verwerken fout uitvoer** toevoegen. Stel het bericht = 'gegevens'. Dit zorgt ervoor dat de standaard-fouten van het script wordt naar de uitvoer van de fout opbouwen krijgen geschreven.

    11. Corrigeer eventuele fouten, aangegeven met blauwe uitroepteken markeringen. Plaats de muisaanwijzer op de markeringen uitroepteken om een tip over de fout. Sla de werkstroom als u wilt wissen fouten.

    Het uiteindelijke resultaat van de werkstroomactiviteiten publiceren ziet er als volgt in de ontwerpfunctie voor:

    ![Werkstroomactiviteiten][5]

    Het uiteindelijke resultaat van de werkstroomactiviteiten publiceren ziet er als volgt in XAML:

        <If Condition="[Not String.IsNullOrEmpty(PublishScriptLocation)]" sap2010:WorkflowViewState.IdRef="If_1">
            <If.Then>
              <Sequence DisplayName="Start Publish" sap2010:WorkflowViewState.IdRef="Sequence_4">
                <Sequence.Variables>
                  <Variable x:TypeArguments="x:String" Name="SubscriptionDataFilePath" />
                  <Variable x:TypeArguments="x:String" Name="PublishScriptFilePath" />
                </Sequence.Variables>
                <mtbwa:ConvertWorkspaceItem DisplayName="Convert publish script filename" sap2010:WorkflowViewState.IdRef="ConvertWorkspaceItem_1" Input="[PublishScriptLocation]" Result="[PublishScriptFilePath]" Workspace="[Workspace]" />
                <mtbwa:ConvertWorkspaceItem DisplayName="Convert subscription filename" sap2010:WorkflowViewState.IdRef="ConvertWorkspaceItem_2" Input="[SubscriptionDataFileLocation]" Result="[SubscriptionDataFilePath]" Workspace="[Workspace]" />
                <mtbwa:InvokeProcess Arguments="[String.Format(&quot; -File &quot;&quot;{0}&quot;&quot; -serviceName {1}&#xD;&#xA;            -storageAccountName {2} -packageLocation &quot;&quot;{3}&quot;&quot;&#xD;&#xA;            -cloudConfigLocation &quot;&quot;{4}&quot;&quot; -subscriptionDataFile &quot;&quot;{5}&quot;&quot;&#xD;&#xA;            -selectedSubscription {6} -environment &quot;&quot;{7}&quot;&quot;&quot;,&#xD;&#xA;            PublishScriptFilePath, ServiceName, StorageAccountName,&#xD;&#xA;            PackageLocation, CloudConfigLocation,&#xD;&#xA;            SubscriptionDataFilePath, SubscriptionName, Environment)]" DisplayName="'Execute Publish Script'" FileName="[PowerShell]" sap2010:WorkflowViewState.IdRef="InvokeProcess_1">
                  <mtbwa:InvokeProcess.ErrorDataReceived>
                    <ActivityAction x:TypeArguments="x:String">
                      <ActivityAction.Argument>
                        <DelegateInArgument x:TypeArguments="x:String" Name="data" />
                      </ActivityAction.Argument>
                      <mtbwa:WriteBuildError Message="{x:Null}" sap2010:WorkflowViewState.IdRef="WriteBuildError_1" />
                    </ActivityAction>
                  </mtbwa:InvokeProcess.ErrorDataReceived>
                  <mtbwa:InvokeProcess.OutputDataReceived>
                    <ActivityAction x:TypeArguments="x:String">
                      <ActivityAction.Argument>
                        <DelegateInArgument x:TypeArguments="x:String" Name="data" />
                      </ActivityAction.Argument>
                      <mtbwa:WriteBuildMessage sap2010:WorkflowViewState.IdRef="WriteBuildMessage_2" Importance="[Microsoft.TeamFoundation.Build.Client.BuildMessageImportance.High]" Message="[data]" mva:VisualBasic.Settings="Assembly references and imported namespaces serialized as XML namespaces" />
                    </ActivityAction>
                  </mtbwa:InvokeProcess.OutputDataReceived>
                </mtbwa:InvokeProcess>
              </Sequence>
            </If.Then>
          </If>
        </Sequence>


7.  Sla de sjabloon werkstroom voor het proces van opbouwen en inchecken dit bestand.

8.  De definitie van de build bewerken (sluit als deze nog is geopend), en selecteer de knop **Nieuw** als u de nieuwe sjabloon in de lijst met sjablonen proces nog niet kan zien.

9.  Stel de parameter eigenschapswaarden in de sectie Diversen als volgt:

    1.  CloudConfigLocation ='c:\\worden\\app.publish\\ServiceConfiguration.Cloud.cscfg' *Deze waarde wordt afgeleid van: ($PublishDir)ServiceConfiguration.Cloud.cscfg*

    2.  PackageLocation = ' c:\\worden\\app.publish\\ContactManager.Azure.cspkg' *Deze waarde wordt afgeleid van: ($PublishDir)($ProjectName) .cspkg*

    3.  PublishScriptLocation = ' c:\\scripts\\WindowsAzure\\PublishCloudService.ps1'

    4.  Servicenaam = 'mycloudservicename' *Gebruik de juiste cloud-servicenaam hier*

    5.  Omgeving = 'Tijdelijke'

    6.  StorageAccountName = 'mystorageaccountname' *Gebruik het juiste opslagaccountnaam hier*

    7.  SubscriptionDataFileLocation = ' c:\\scripts\\WindowsAzure\\Subscription.xml'

    8.  SubscriptionName = 'default'

    ![Eigenschap parameterwaarden][6]

10. Sla de wijzigingen op de definitie maken.

11. Wachtrij een opbouwen uitvoeren van zowel het pakket maken en publiceren. Als u een trigger ingesteld op doorlopend integratie hebt, kunt u dit gedrag wordt uitgevoerd op elke inchecken.

### <a name="publishcloudserviceps1-script-template"></a>PublishCloudService.ps1 script sjabloon

```
Param(  $serviceName = "",
        $storageAccountName = "",
        $packageLocation = "",
        $cloudConfigLocation = "",
        $environment = "Staging",
        $deploymentLabel = "ContinuousDeploy to $servicename",
        $timeStampFormat = "g",
        $alwaysDeleteExistingDeployments = 1,
        $enableDeploymentUpgrade = 1,
        $selectedsubscription = "default",
        $subscriptionDataFile = ""
     )


function Publish()
{
    $deployment = Get-AzureDeployment -ServiceName $serviceName -Slot $slot -ErrorVariable a -ErrorAction silentlycontinue
    if ($a[0] -ne $null)
    {
        Write-Output "$(Get-Date -f $timeStampFormat) - No deployment is detected. Creating a new deployment. "
    }
    #check for existing deployment and then either upgrade, delete + deploy, or cancel according to $alwaysDeleteExistingDeployments and $enableDeploymentUpgrade boolean variables
    if ($deployment.Name -ne $null)
    {
        switch ($alwaysDeleteExistingDeployments)
        {
            1
            {
                switch ($enableDeploymentUpgrade)
                {
                    1  #Update deployment inplace (usually faster, cheaper, won't destroy VIP)
                    {
                        Write-Output "$(Get-Date -f $timeStampFormat) - Deployment exists in $servicename.  Upgrading deployment."
                        UpgradeDeployment
                    }
                    0  #Delete then create new deployment
                    {
                        Write-Output "$(Get-Date -f $timeStampFormat) - Deployment exists in $servicename.  Deleting deployment."
                        DeleteDeployment
                        CreateNewDeployment

                    }
                } # switch ($enableDeploymentUpgrade)
            }
            0
            {
                Write-Output "$(Get-Date -f $timeStampFormat) - ERROR: Deployment exists in $servicename.  Script execution cancelled."
                exit
            }
        } #switch ($alwaysDeleteExistingDeployments)
    } else {
            CreateNewDeployment
    }
}

function CreateNewDeployment()
{
    write-progress -id 3 -activity "Creating New Deployment" -Status "In progress"
    Write-Output "$(Get-Date -f $timeStampFormat) - Creating New Deployment: In progress"

    $opstat = New-AzureDeployment -Slot $slot -Package $packageLocation -Configuration $cloudConfigLocation -label $deploymentLabel -ServiceName $serviceName

    $completeDeployment = Get-AzureDeployment -ServiceName $serviceName -Slot $slot
    $completeDeploymentID = $completeDeployment.deploymentid

    write-progress -id 3 -activity "Creating New Deployment" -completed -Status "Complete"
    Write-Output "$(Get-Date -f $timeStampFormat) - Creating New Deployment: Complete, Deployment ID: $completeDeploymentID"

    StartInstances
}

function UpgradeDeployment()
{
    write-progress -id 3 -activity "Upgrading Deployment" -Status "In progress"
    Write-Output "$(Get-Date -f $timeStampFormat) - Upgrading Deployment: In progress"

    # perform Update-Deployment
    $setdeployment = Set-AzureDeployment -Upgrade -Slot $slot -Package $packageLocation -Configuration $cloudConfigLocation -label $deploymentLabel -ServiceName $serviceName -Force

    $completeDeployment = Get-AzureDeployment -ServiceName $serviceName -Slot $slot
    $completeDeploymentID = $completeDeployment.deploymentid

    write-progress -id 3 -activity "Upgrading Deployment" -completed -Status "Complete"
    Write-Output "$(Get-Date -f $timeStampFormat) - Upgrading Deployment: Complete, Deployment ID: $completeDeploymentID"
}

function DeleteDeployment()
{

    write-progress -id 2 -activity "Deleting Deployment" -Status "In progress"
    Write-Output "$(Get-Date -f $timeStampFormat) - Deleting Deployment: In progress"

    #WARNING - always deletes with force
    $removeDeployment = Remove-AzureDeployment -Slot $slot -ServiceName $serviceName -Force

    write-progress -id 2 -activity "Deleting Deployment: Complete" -completed -Status $removeDeployment
    Write-Output "$(Get-Date -f $timeStampFormat) - Deleting Deployment: Complete"

}

function StartInstances()
{
    write-progress -id 4 -activity "Starting Instances" -status "In progress"
    Write-Output "$(Get-Date -f $timeStampFormat) - Starting Instances: In progress"

    $deployment = Get-AzureDeployment -ServiceName $serviceName -Slot $slot
    $runstatus = $deployment.Status

    if ($runstatus -ne 'Running')
    {
        $run = Set-AzureDeployment -Slot $slot -ServiceName $serviceName -Status Running
    }
    $deployment = Get-AzureDeployment -ServiceName $serviceName -Slot $slot
    $oldStatusStr = @("") * $deployment.RoleInstanceList.Count

    while (-not(AllInstancesRunning($deployment.RoleInstanceList)))
    {
        $i = 1
        foreach ($roleInstance in $deployment.RoleInstanceList)
        {
            $instanceName = $roleInstance.InstanceName
            $instanceStatus = $roleInstance.InstanceStatus

            if ($oldStatusStr[$i - 1] -ne $roleInstance.InstanceStatus)
            {
                $oldStatusStr[$i - 1] = $roleInstance.InstanceStatus
                Write-Output "$(Get-Date -f $timeStampFormat) - Starting Instance '$instanceName': $instanceStatus"
            }

            write-progress -id (4 + $i) -activity "Starting Instance '$instanceName'" -status "$instanceStatus"
            $i = $i + 1
        }

        sleep -Seconds 1

        $deployment = Get-AzureDeployment -ServiceName $serviceName -Slot $slot
    }

    $i = 1
    foreach ($roleInstance in $deployment.RoleInstanceList)
    {
        $instanceName = $roleInstance.InstanceName
        $instanceStatus = $roleInstance.InstanceStatus

        if ($oldStatusStr[$i - 1] -ne $roleInstance.InstanceStatus)
        {
            $oldStatusStr[$i - 1] = $roleInstance.InstanceStatus
            Write-Output "$(Get-Date -f $timeStampFormat) - Starting Instance '$instanceName': $instanceStatus"
        }

        $i = $i + 1
    }

    $deployment = Get-AzureDeployment -ServiceName $serviceName -Slot $slot
    $opstat = $deployment.Status

    write-progress -id 4 -activity "Starting Instances" -completed -status $opstat
    Write-Output "$(Get-Date -f $timeStampFormat) - Starting Instances: $opstat"
}

function AllInstancesRunning($roleInstanceList)
{
    foreach ($roleInstance in $roleInstanceList)
    {
        if ($roleInstance.InstanceStatus -ne "ReadyRole")
        {
            return $false
        }
    }

    return $true
}

#configure powershell with Azure 1.7 modules
Import-Module Azure

#configure powershell with publishsettings for your subscription
$pubsettings = $subscriptionDataFile
Import-AzurePublishSettingsFile $pubsettings
Set-AzureSubscription -CurrentStorageAccountName $storageAccountName -SubscriptionName $selectedsubscription
Select-AzureSubscription $selectedsubscription

#set remaining environment variables for Azure cmdlets
$subscription = Get-AzureSubscription $selectedsubscription
$subscriptionname = $subscription.subscriptionname
$subscriptionid = $subscription.subscriptionid
$slot = $environment

#main driver - publish & write progress to activity log
Write-Output "$(Get-Date -f $timeStampFormat) - Azure Cloud Service deploy script started."
Write-Output "$(Get-Date -f $timeStampFormat) - Preparing deployment of $deploymentLabel for $subscriptionname with Subscription ID $subscriptionid."

Publish

$deployment = Get-AzureDeployment -slot $slot -serviceName $servicename
$deploymentUrl = $deployment.Url

Write-Output "$(Get-Date -f $timeStampFormat) - Created Cloud Service with URL $deploymentUrl."
Write-Output "$(Get-Date -f $timeStampFormat) - Azure Cloud Service deploy script finished."
```

## <a name="next-steps"></a>Volgende stappen

Als u wilt foutopsporing op afstand inschakelen wanneer u een doorlopend bezorging gebruikt, Zie [inschakelen bij gebruik van continue bezorging publiceren naar Azure foutopsporing op afstand](cloud-services-virtual-machines-dotnet-continuous-delivery-remote-debugging.md).

  [Continue bezorging bij Azure met behulp van Visual Studio teamservices]: cloud-services-continuous-delivery-use-vso.md  
  [Team Foundation opbouwen-Service]: https://msdn.microsoft.com/library/ee259687.aspx
  [.NET Framework 4]: https://www.microsoft.com/download/details.aspx?id=17851
  [.NET Framework 4.5]: https://www.microsoft.com/download/details.aspx?id=30653
  [.NET framework 4.5.2]: https://www.microsoft.com/download/details.aspx?id=42643
  [De schaal van uw systeem opbouwen aanpassen]: https://msdn.microsoft.com/library/dd793166.aspx
  [Implementeren en configureren van een server opbouwen]: https://msdn.microsoft.com/library/ms181712.aspx
  [Azure PowerShell-cmdlets]: powershell-install-configure.md
  [the .publishsettings file]: https://manage.windowsazure.com/download/publishprofile.aspx?wa=wsignin1.0
  [0]: ./media/cloud-services-dotnet-continuous-delivery/tfs-01bc.png
  [2]: ./media/cloud-services-dotnet-continuous-delivery/tfs-02.png
  [3]: ./media/cloud-services-dotnet-continuous-delivery/common-task-tfs-03.png
  [4]: ./media/cloud-services-dotnet-continuous-delivery/common-task-tfs-04.png
  [5]: ./media/cloud-services-dotnet-continuous-delivery/common-task-tfs-05.png
  [6]: ./media/cloud-services-dotnet-continuous-delivery/common-task-tfs-06.png
