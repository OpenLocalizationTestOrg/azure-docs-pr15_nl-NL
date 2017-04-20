<properties
    pageTitle="Rollen voor het web en werknemer van PHP | Microsoft Azure"
    description="Een handleiding voor het maken van PHP web en werknemer rollen in een Azure-cloudservice en de runtime PHP configureren."
    services=""
    documentationCenter="php"
    authors="rmcmurray"
    manager="wpickett"
    editor=""/>

<tags
    ms.service="cloud-services"
    ms.workload="tbd"
    ms.tgt_pltfrm="na"
    ms.devlang="PHP"
    ms.topic="article"
    ms.date="08/11/2016"
    ms.author="robmcm"/>

#<a name="how-to-create-php-web-and-worker-roles"></a>Het maken van PHP web en werknemer rollen

## <a name="overview"></a>Overzicht

Deze handleiding leert u hoe u rollen voor het web of werknemer van PHP maken in een Windows-ontwikkelomgeving, kiest u een specifieke versie van PHP van de beschikbare "ingebouwde" versies wijzigen van de configuratie PHP, extensies inschakelen en ten slotte implementeren naar Azure. Hierin wordt ook het configureren van een webpagina of werknemer rol als u wilt gebruiken, een PHP-runtime (met aangepaste configuratie en uitbreidingen) die u opgeeft.

## <a name="what-are-php-web-and-worker-roles"></a>Wat zijn de rollen web en werknemer PHP?

Azure biedt drie modellen voor het uitvoeren van toepassingen berekenen: Azure App-Service, Azure virtuele Machines en Azure-Cloudservices. Alle drie modellen ondersteunen PHP. Cloudservices, waaronder web en werknemer rollen, biedt *platform als een service (PaaS)*. Binnen een cloudservice biedt een Webrol een speciale webserver van Internet Information Services (IIS) naar de front-webtoepassingen host. Asynchroon, langdurige of doorlopende taken onafhankelijk van interactie van de gebruiker of invoer kan worden uitgevoerd door een rol werknemer.

Zie [hostingprovider opties op Azure berekenen](./cloud-services/cloud-services-choose-me.md)voor meer informatie over deze opties.

## <a name="download-the-azure-sdk-for-php"></a>Downloaden van de Azure SDK voor PHP

De [SDK van Azure voor PHP] bestaat uit meerdere onderdelen. In dit artikel worden twee van deze gebruiken: Azure PowerShell en de Azure emulators. Deze twee onderdelen kunnen worden geïnstalleerd via het installatieprogramma van Microsoft Web Platform. Lees [hoe u installeren en configureren van Azure PowerShell](powershell-install-configure.md)voor meer informatie.

## <a name="create-a-cloud-services-project"></a>Maak een Cloud Services-project

De eerste stap bij het maken van een PHP web of werknemer rol is het opzetten van een project Azure-Service. een project Azure-Service fungeert als logische container voor web en werknemer rollen en bevat de [definitie van de service (.csdef)] en [service te configureren (.cscfg)] bestanden van het project.

Maak een nieuw project van Azure-Service, Azure PowerShell uitvoeren als beheerder en voer de volgende opdracht uit:

    PS C:\>New-AzureServiceProject myProject

Deze opdracht maakt een nieuwe map (`myProject`) waaraan u rollen web en werknemer kunt toevoegen.

## <a name="add-php-web-or-worker-roles"></a>Rollen voor het web of werknemer van PHP toevoegen

Als u wilt een PHP Webrol aan een project toevoegen, voert u de volgende opdracht in de hoofdmap van het project:

    PS C:\myProject> Add-AzurePHPWebRole roleName

Gebruik deze opdracht voor een rol werknemer:

    PS C:\myProject> Add-AzurePHPWorkerRole roleName

> [AZURE.NOTE] De `roleName` parameter is optioneel. Als dit wordt weggelaten, wordt de naam van de functie automatisch gegenereerd. De eerste Webrol gemaakt worden `WebRole1`, de tweede worden `WebRole2`, enzovoort. De eerste werknemer rol gemaakt worden `WorkerRole1`, de tweede worden `WorkerRole2`, enzovoort.

## <a name="specify-the-built-in-php-version"></a>Geef de ingebouwde PHP-versie

Wanneer u een PHP web of werknemer rol aan een project toevoegt, worden de projectbestanden configuratie gewijzigd zodat PHP wordt geïnstalleerd op elk exemplaar van het web of werknemer van uw toepassing wanneer deze wordt geïmplementeerd. Als u wilt zien van de versie van PHP die al dan niet standaard wordt geïnstalleerd, voert u de volgende opdracht uit:

    PS C:\myProject> Get-AzureServiceProjectRoleRuntime

De uitvoer van de bovenstaande opdracht ziet er ongeveer wat wordt weergegeven onder uit. In dit voorbeeld de `IsDefault` vlag is ingesteld op `true` voor PHP 5.3.17, die aangeeft dat deze de standaardversie PHP is geïnstalleerd.

    Runtime Version     PackageUri                      IsDefault
    ------- -------     ----------                      ---------
    Node 0.6.17         http://nodertncu.blob.core...   False
    Node 0.6.20         http://nodertncu.blob.core...   True
    Node 0.8.4          http://nodertncu.blob.core...   False
    IISNode 0.1.21      http://nodertncu.blob.core...   True
    Cache 1.8.0         http://nodertncu.blob.core...   True
    PHP 5.3.17          http://nodertncu.blob.core...   True
    PHP 5.4.0           http://nodertncu.blob.core...   False

U kunt PHP runtime-versie instellen op een van de PHP-versies die worden weergegeven. Als u bijvoorbeeld de PHP-versie instellen (voor een rol met de naam `roleName`) te 5.4.0, gebruikt u de volgende opdracht uit:

    PS C:\myProject> Set-AzureServiceProjectRole roleName php 5.4.0

> [AZURE.NOTE] Beschikbare PHP versies kunnen in de toekomst veranderen.

## <a name="customize-the-built-in-php-runtime"></a>De ingebouwde PHP runtime aanpassen

Hebt u volledige controle over de configuratie van de PHP runtime die wordt geïnstalleerd wanneer u de stappen hierboven, met inbegrip van wijziging van `php.ini` -instellingen en inschakelen van uitbreidingen.

U kunt de ingebouwde PHP runtime aanpassen door de volgende stappen uit:

1. Een nieuwe map, met de naam toevoegen `php`, aan de `bin` map van uw Webrol. Voor een rol werknemer, moet u deze naar de hoofdmap van de functie toevoegen.
2. In de `php` map, maakt u een andere map genaamd `ext`. Zet een `.dll` extensiebestanden (bijvoorbeeld `php_mongo.dll`) die u wilt inschakelen in deze map.
3. Toevoegen een `php.ini` van het bestand in de `php` map. Een aangepaste extensies inschakelen en stelt u alle PHP richtlijnen in dit bestand. Als u wilt schakelen bijvoorbeeld `display_errors` op en schakel de `php_mongo.dll` extensie, de inhoud van uw `php.ini` bestand zou als volgt:

        display_errors=On
        extension=php_mongo.dll

> [AZURE.NOTE] Instellingen die u niet expliciet instellen in de `php.ini` bestand dat u opgeeft van wordt automatisch worden ingesteld op de standaardwaarden. Houd er rekening mee dat u een volledige kunt toevoegen echter `php.ini` bestand.

## <a name="use-your-own-php-runtime"></a>Uw eigen runtime PHP gebruiken
In sommige gevallen, in plaats van een ingebouwde PHP runtime selecteren en te configureren als hierboven, wilt u mogelijk de runtime van uw eigen PHP bieden. U kunt bijvoorbeeld de dezelfde PHP runtime gebruiken in een webpagina of werknemer rol die u in uw ontwikkelomgeving gebruiken. Dit gemakkelijker om ervoor te zorgen dat de toepassing gedrag in uw productieomgeving niet wijzigen.

### <a name="configure-a-web-role-to-use-your-own-php-runtime"></a>Een Webrol als u wilt gebruiken van uw eigen runtime PHP configureren

Als u wilt configureren een Webrol als u wilt gebruiken, een PHP runtime die u opgeeft, als volgt te werk:

1. Een project Azure-Service maken en toevoegen van een rol van de web PHP, zoals eerder is beschreven in dit onderwerp.
2. Maak een `php` map in de `bin` map die is in de hoofdmap van de Webrol van uw, en voegt u uw PHP-runtime (alle binaire bestanden, configuratiebestanden, submappen, enzovoort) toe aan de `php` map.
3. (OPTIONEEL) Als uw runtime PHP gebruikmaakt van de [Microsoft-stuurprogramma's voor PHP voor SQL Server][sqlsrv drivers], moet u uw Webrol om te kunnen installeren van [SQL Server Native Client 2012] configureren[ sql native client] wanneer deze is ingericht. Klik hiertoe toevoegen het [installatieprogramma van sqlncli.msi x64] naar de `bin` map in de hoofdmap van de Webrol van uw. Het opstartscript beschreven in de volgende stap wordt het installatieprogramma van stilte uitgevoerd wanneer de rol is ingericht. Als de Microsoft-Drivers uw runtime PHP niet voor PHP voor SQL Server gebruikt, kunt u de volgende regel verwijderen uit het script wordt weergegeven in de volgende stap:

        msiexec /i sqlncli.msi /qn IACCEPTSQLNCLILICENSETERMS=YES

4. Een taak opstarten waarmee u kunt [Internet Information Services (IIS configureren)] definieert[ iis.net] naar uw runtime PHP gebruiken voor het verwerken van aanvragen voor `.php` pagina's. Klik hiertoe openen de `setup_web.cmd` bestand (in de `bin` bestand van de hoofdmap van de Webrol van uw) in een teksteditor en de inhoud ervan vervangen door het volgende script:

        @ECHO ON
        cd "%~dp0"

        if "%EMULATED%"=="true" exit /b 0

        msiexec /i sqlncli.msi /qn IACCEPTSQLNCLILICENSETERMS=YES

        SET PHP_FULL_PATH=%~dp0php\php-cgi.exe
        SET NEW_PATH=%PATH%;%RoleRoot%\base\x86

        %WINDIR%\system32\inetsrv\appcmd.exe set config -section:system.webServer/fastCgi /+"[fullPath='%PHP_FULL_PATH%',maxInstances='12',idleTimeout='60000',activityTimeout='3600',requestTimeout='60000',instanceMaxRequests='10000',protocol='NamedPipe',flushNamedPipe='False']" /commit:apphost
        %WINDIR%\system32\inetsrv\appcmd.exe set config -section:system.webServer/fastCgi /+"[fullPath='%PHP_FULL_PATH%'].environmentVariables.[name='PATH',value='%NEW_PATH%']" /commit:apphost
        %WINDIR%\system32\inetsrv\appcmd.exe set config -section:system.webServer/fastCgi /+"[fullPath='%PHP_FULL_PATH%'].environmentVariables.[name='PHP_FCGI_MAX_REQUESTS',value='10000']" /commit:apphost
        %WINDIR%\system32\inetsrv\appcmd.exe set config -section:system.webServer/handlers /+"[name='PHP',path='*.php',verb='GET,HEAD,POST',modules='FastCgiModule',scriptProcessor='%PHP_FULL_PATH%',resourceType='Either',requireAccess='Script']" /commit:apphost
        %WINDIR%\system32\inetsrv\appcmd.exe set config -section:system.webServer/fastCgi /"[fullPath='%PHP_FULL_PATH%'].queueLength:50000"

5. Uw toepassingsbestanden toevoegen naar de hoofdmap van de Webrol van uw. Dit is de hoofdmap van de webserver.

6. Publiceren uw toepassing zoals is beschreven in de onderstaande van de sectie voor [uw toepassing publiceren](#how-to-publish-your-application) .

> [AZURE.NOTE] De `download.ps1` script (in de `bin` map van de hoofdmap van de Webrol) kunnen worden verwijderd nadat u de stappen voor het gebruik van uw eigen runtime PHP hierboven is beschreven.

### <a name="configure-a-worker-role-to-use-your-own-php-runtime"></a>Een rol werknemer als u wilt gebruiken van uw eigen runtime PHP configureren

Als u wilt configureren een rol werknemer als u wilt gebruiken, een PHP runtime die u opgeeft, als volgt te werk:

1. Een project Azure-Service maken en toevoegen van een rol van de werknemer PHP, zoals eerder is beschreven in dit onderwerp.
2. Maak een `php` map in de hoofdmap van de rol van de werknemer, en uw PHP-runtime (alle binaire bestanden, configuratiebestanden, submappen, enzovoort) toevoegen aan de `php` map.
3. (OPTIONEEL) Als uw runtime PHP [Microsoft-stuurprogramma's voor PHP voor SQL Server]gebruikt[sqlsrv drivers], moet u uw rol werknemer om te kunnen installeren van [SQL Server Native Client 2012] configureren[ sql native client] wanneer deze is ingericht. Klik hiertoe toevoegen het [installatieprogramma van sqlncli.msi x64] naar de hoofdmap van de rol van de werknemer. Het opstartscript beschreven in de volgende stap wordt het installatieprogramma van stilte uitgevoerd wanneer de rol is ingericht. Als de Microsoft-Drivers uw runtime PHP niet voor PHP voor SQL Server gebruikt, kunt u de volgende regel verwijderen uit het script wordt weergegeven in de volgende stap:

        msiexec /i sqlncli.msi /qn IACCEPTSQLNCLILICENSETERMS=YES

4. Een taak opstarten die worden toegevoegd definiëren uw `php.exe` uitvoerbare aan van de rol van de werknemer omgevingsvariabele PATH wanneer de rol is ingericht. Klik hiertoe openen de `setup_worker.cmd` -bestand (in de hoofdmap van de rol van de werknemer) in een teksteditor en de inhoud ervan vervangen door het volgende script:

        @echo on

        cd "%~dp0"

        echo Granting permissions for Network Service to the web root directory...
        icacls ..\ /grant "Network Service":(OI)(CI)W
        if %ERRORLEVEL% neq 0 goto error
        echo OK

        if "%EMULATED%"=="true" exit /b 0

        msiexec /i sqlncli.msi /qn IACCEPTSQLNCLILICENSETERMS=YES

        setx Path "%PATH%;%~dp0php" /M

        if %ERRORLEVEL% neq 0 goto error

        echo SUCCESS
        exit /b 0

        :error

        echo FAILED
        exit /b -1

5. Uw toepassingsbestanden naar de hoofdmap van de rol van uw werknemer toevoegen.

6. Publiceren uw toepassing zoals is beschreven in de onderstaande van de sectie voor [uw toepassing publiceren](#how-to-publish-your-application) .

## <a name="run-your-application-in-the-compute-and-storage-emulators"></a>Uw toepassing uitvoeren in de emulators berekeningscluster en opslag

De Azure emulators vindt u een lokale omgeving waarin u uw Azure-toepassing testen kunt voordat u deze in de cloud implementeert. Er zijn enkele verschillen tussen de emulators en de Azure-omgeving. Zie informatie over dit beter [gebruiken de emulator Azure opslag voor ontwikkelen en testen](./storage/storage-use-emulator.md).

Houd er rekening mee dat u PHP geïnstalleerd lokaal om het gebruik van de emulator berekeningscluster nodig hebt. De emulator berekeningscluster wordt uw lokale PHP-installatie gebruiken voor het uitvoeren van de toepassing.

Als u wilt uw project uitvoeren in de emulators, voer de volgende opdracht uit de hoofdmap van uw project:

    PS C:\MyProject> Start-AzureEmulator

Hier ziet u uitvoer ongeveer als volgt:

    Creating local package...
    Starting Emulator...
    Role is running at http://127.0.0.1:81
    Started

U kunt uw toepassing uitgevoerd in de emulator door een webbrowser te openen en te bladeren naar het lokale adres dat wordt weergegeven in de uitvoer zien (`http://127.0.0.1:81` in het bovenstaande voorbeeld-uitvoer).

Als u wilt stoppen met het emulators, voert u deze opdracht uit:

    PS C:\MyProject> Stop-AzureEmulator

## <a name="publish-your-application"></a>Uw toepassing publiceren

Als u wilt uw toepassing publiceren, moet u eerst importeren uw publicatie-instellingen via de cmdlet [Importeren-AzurePublishSettingsFile](https://msdn.microsoft.com/library/azure/dn790370.aspx) . Vervolgens kunt u uw toepassing publiceren met behulp van de cmdlet [Publiceren-AzureServiceProject](https://msdn.microsoft.com/library/azure/dn495166.aspx) . Zie voor informatie over het aanmelden, [installeren en configureren van Azure PowerShell](powershell-install-configure.md).

## <a name="next-steps"></a>Volgende stappen

Zie het [PHP Developer Center](/develop/php/)voor meer informatie.

[Azure SDK voor PHP]: /develop/php/common-tasks/download-php-sdk/
[install ps and emulators]: http://go.microsoft.com/fwlink/p/?linkid=320376&clcid=0x409
[definitie van de service (.csdef)]: http://msdn.microsoft.com/library/windowsazure/ee758711.aspx
[configuratie van de service (.cscfg)]: http://msdn.microsoft.com/library/windowsazure/ee758710.aspx
[iis.net]: http://www.iis.net/
[sql native client]: http://msdn.microsoft.com/sqlserver/aa937733.aspx
[sqlsrv drivers]: http://php.net/sqlsrv
[SQLNCLI.msi x64 installer]: http://go.microsoft.com/fwlink/?LinkID=239648
