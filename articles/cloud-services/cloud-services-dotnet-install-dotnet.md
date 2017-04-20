<properties
   pageTitle=".NET installeren op een rol van de Service Cloud | Microsoft Azure"
   description="Dit artikel wordt beschreven hoe u handmatig installeert .NET framework op Cloud Service Web en werknemer rollen"
   services="cloud-services"
   documentationCenter=".net"
   authors="thraka"
   manager="timlt"
   editor=""/>

<tags
   ms.service="cloud-services"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="08/10/2016"
   ms.author="adegeo"/>

# <a name="install-net-on-a-cloud-service-role"></a>.NET installeren op een rol Cloud-Service 

In dit artikel wordt beschreven hoe u .NET framework installeert op Cloud Service Web en werknemer rollen. U kunt deze stappen .NET 4.6.1 installeren op de Azure Gast OS familie 4. Zie [Azure Gast OS loslaat en SDK compatibiliteit Matrix](cloud-services-guestos-update-matrix.md)voor de meest recente informatie over Gast OS versies.

Het proces van het .NET installeren op uw web en werknemer rollen heeft betrekking op inclusief het .NET-installatiepakket als onderdeel van uw Project Cloud en het installatieprogramma starten als onderdeel van de functie opstarten taken.  

## <a name="add-the-net-installer-to-your-project"></a>Het installatieprogramma van .NET toevoegen aan uw project
- Download de het installatieprogramma van web voor .NET framework u wilt installeren
    - [.NET 4.6.1 Installer met webonderdelen](http://go.microsoft.com/fwlink/?LinkId=671729)
- Voor een Webrol
  1. In **Solution Explorer**onder In de **rollen** in de cloud service project Klik met de rechtermuisknop op uw rol en selecteert u **toevoegen > nieuwe map**. De *opslaglocatie* van een map maken
  2. Klik met de rechtermuisknop op de map **bin** en selecteer **toevoegen > bestaand Item**. Selecteer het installatieprogramma van .NET en deze toevoegen aan de map bin.
- Voor een rol werknemer
  1. Klik met de rechtermuisknop op uw rol en selecteert u **toevoegen > bestaand Item**. Selecteer het installatieprogramma van .NET en deze toevoegen aan de rol. 

Bestanden toegevoegd op deze manier naar de map rol inhoud automatisch worden toegevoegd aan het pakket cloud-service en geïmplementeerd op een consistente locatie op de virtuele machine. Herhaal deze procedure voor alle web en werknemer rollen in uw-Cloudservice, zodat alle functies een kopie van het installatieprogramma zijn.

> [AZURE.NOTE] Zelfs als uw toepassing is bedoeld voor .NET 4.6, moet u .NET 4.6.1 installeren op uw rol Cloudservice. Het besturingssysteem van Azure Gast bevat updates [3098779](https://support.microsoft.com/kb/3098779) en [3097997](https://support.microsoft.com/kb/3097997). .NET 4.6 boven aan deze updates installeren kan problemen veroorzaken wanneer u zich uw .NET-toepassingen, zodat u rechtstreeks .NET 4.6.1 in plaats van .NET 4.6 moet installeren. Zie [KB 3118750](https://support.microsoft.com/kb/3118750)voor meer informatie.

![Rol-inhoud met installatiebestanden][1]

## <a name="define-startup-tasks-for-your-roles"></a>Opstarttaken voor uw rollen definiëren
Opstarttaken kunt u bewerkingen uitvoeren voordat u een rol wordt gestart. Installatie van .NET Framework als onderdeel van de taak opstarten zorgt ervoor dat dat het kader is geïnstalleerd voordat u een van de toepassingscode van uw wordt uitgevoerd. Zie voor meer informatie over het opstarten taken: [Opstarttaken uitvoeren in Azure wordt aangegeven](cloud-services-startup-tasks.md). 

1. Voeg de volgende naar het bestand *ServiceDefinition.csdef* onder het knooppunt **WebRole** of **WorkerRole** voor alle rollen:
    
    ```xml
    <LocalResources>
      <LocalStorage name="NETFXInstall" sizeInMB="1024" cleanOnRoleRecycle="false" />
    </LocalResources>    
    <Startup>
      <Task commandLine="install.cmd" executionContext="elevated" taskType="simple">
        <Environment>
          <Variable name="PathToNETFXInstall">
            <RoleInstanceValue xpath="/RoleEnvironment/CurrentInstance/LocalResources/LocalResource[@name='NETFXInstall']/@path" />
          </Variable>
          <Variable name="ComputeEmulatorRunning">
            <RoleInstanceValue xpath="/RoleEnvironment/Deployment/@emulated" />
          </Variable>
        </Environment>
      </Task>
    </Startup>
    ```

    De bovenstaande configuratie wordt de console opdracht *install.cmd* uitvoeren met beheerdersbevoegdheden zodat deze .NET framework kunt installeren. De configuratie wordt ook een LocalStorage gemaakt met de naam *NETFXInstall*. Het opstartscript wordt de map temp gebruik van deze resource lokale opslag, zodat het installatieprogramma van .NET framework wordt gedownload en geïnstalleerd vanaf deze resource ingesteld. Het is belangrijk om in te stellen van de grootte van deze resource ten minste 1024 MB om ervoor te zorgen dat het kader correct kan worden geïnstalleerd. Zie voor meer informatie over het starten van taken: [algemene Cloudservice opstarttaken](cloud-services-startup-tasks-common.md) 

2. Een bestand **install.cmd** maken en deze toevoegen aan alle rollen met de rechtermuisknop op de rol en selecteren **toevoegen > bestaand Item...**. Zodat alle rollen nu het installatiebestand .NET, evenals de install.cmd-bestand hebt.
    
    ![Rol-inhoud met alle bestanden][2]

    > [AZURE.NOTE] Gebruik een teksteditor zoals Kladblok om dit bestand te maken. Als u Visual Studio gebruiken om te maken van een tekstbestand en wijzig de naam aan de 'cmd' het bestand nog steeds een UTF-8-Byte Order Mark kan bevatten en de eerste regel van het script actief zijn ingevoegd om een fout. Als u Visual Studio gebruiken om te maken het bestand verlaten een REM (Opmerking) toevoegen aan de eerste regel van het bestand, zodat deze wordt genegeerd wanneer uitvoert. 

3. Het volgende script toevoegen aan het bestand **install.cmd** :

    ```
    REM Set the value of netfx to install appropriate .NET Framework. 
    REM ***** To install .NET 4.5.2 set the variable netfx to "NDP452" *****
    REM ***** To install .NET 4.6 set the variable netfx to "NDP46" *****
    REM ***** To install .NET 4.6.1 set the variable netfx to "NDP461" *****
    REM ***** To install .NET 4.6.2 set the variable netfx to "NDP462" *****
    set netfx="NDP461"
    
    REM ***** Set script start timestamp *****
    set timehour=%time:~0,2%
    set timestamp=%date:~-4,4%%date:~-10,2%%date:~-7,2%-%timehour: =0%%time:~3,2%
    set "log=install.cmd started %timestamp%."
    
    REM ***** Exit script if running in Emulator *****
    if %ComputeEmulatorRunning%=="true" goto exit
    
    REM ***** Needed to correctly install .NET 4.6.1, otherwise you may see an out of disk space error *****
    set TMP=%PathToNETFXInstall%
    set TEMP=%PathToNETFXInstall%
    
    REM ***** Setup .NET filenames and registry keys *****
    if %netfx%=="NDP462" goto NDP462
    if %netfx%=="NDP461" goto NDP461
    if %netfx%=="NDP46" goto NDP46
        set "netfxinstallfile=NDP452-KB2901954-Web.exe"
        set netfxregkey="0x5cbf5"
        goto logtimestamp
    
    :NDP46
    set "netfxinstallfile=NDP46-KB3045560-Web.exe"
    set netfxregkey="0x6004f"
    goto logtimestamp
    
    :NDP461
    set "netfxinstallfile=NDP461-KB3102438-Web.exe"
    set netfxregkey="0x6040e"
    goto logtimestamp
    
    :NDP462
    set "netfxinstallfile=NDP462-KB3151802-Web.exe"
    set netfxregkey="0x60632"
    
    :logtimestamp
    REM ***** Setup LogFile with timestamp *****
    md "%PathToNETFXInstall%\log"
    set startuptasklog="%PathToNETFXInstall%log\startuptasklog-%timestamp%.txt"
    set netfxinstallerlog="%PathToNETFXInstall%log\NetFXInstallerLog-%timestamp%"
    echo %log% >> %startuptasklog%
    echo Logfile generated at: %startuptasklog% >> %startuptasklog%
    echo TMP set to: %TMP% >> %startuptasklog%
    echo TEMP set to: %TEMP% >> %startuptasklog%
    
    REM ***** Check if .NET is installed *****
    echo Checking if .NET (%netfx%) is installed >> %startuptasklog%
    set /A netfxregkeydecimal=%netfxregkey%
    set foundkey=0
    FOR /F "usebackq skip=2 tokens=1,2*" %%A in (`reg query "HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\NET Framework Setup\NDP\v4\Full" /v Release 2^>nul`) do @set /A foundkey=%%C
    echo Minimum required key: %netfxregkeydecimal% -- found key: %foundkey% >> %startuptasklog%
    if %foundkey% GEQ %netfxregkeydecimal% goto installed
    
    REM ***** Installing .NET *****
    echo Installing .NET with commandline: start /wait %~dp0%netfxinstallfile% /q /serialdownload /log %netfxinstallerlog%  /chainingpackage "CloudService Startup Task" >> %startuptasklog%
    start /wait %~dp0%netfxinstallfile% /q /serialdownload /log %netfxinstallerlog% /chainingpackage "CloudService Startup Task" >> %startuptasklog% 2>>&1
    if %ERRORLEVEL%== 0 goto installed
        echo .NET installer exited with code %ERRORLEVEL% >> %startuptasklog%   
        if %ERRORLEVEL%== 3010 goto restart
        if %ERRORLEVEL%== 1641 goto restart
        echo .NET (%netfx%) install failed with Error Code %ERRORLEVEL%. Further logs can be found in %netfxinstallerlog% >> %startuptasklog%
    
    :restart
    echo Restarting to complete .NET (%netfx%) installation >> %startuptasklog%
    EXIT /B %ERRORLEVEL%
    
    :installed
    echo .NET (%netfx%) is installed >> %startuptasklog%
    
    :end
    echo install.cmd completed: %date:~-4,4%%date:~-10,2%%date:~-7,2%-%timehour: =0%%time:~3,2% >> %startuptasklog%
    
    :exit
    EXIT /B 0
    ```
        
    Het script voor installatie wordt gecontroleerd of de opgegeven .NET framework-versie al is geïnstalleerd op de computer via query's in het register. Als de .NET-versie niet is geïnstalleerd en de .net Web Installer wordt gestart. Om u te helpen oplossen met eventuele problemen het script wordt Meld u aan alle activiteit naar een bestand met de naam *startuptasklog-(currentdatetime) .txt* die zijn opgeslagen in de lokale opslag *InstallLogs* .

    > [AZURE.NOTE] Het script nog steeds wordt beschreven hoe u bij het installeren van .NET 4.5.2 of .NET 4.6 voor bedrijfscontinuïteit. Er is niet nodig .NET 4.5.2 handmatig installeren als deze nog beschikbaar zijn op Azure Gast OS. In plaats van de installatie van .NET 4.6 moet u rechtstreeks .NET 4.6.1 vanwege [KB 3118750](https://support.microsoft.com/kb/3118750)installeren.
      

## <a name="configure-diagnostics-to-transfer-the-startup-task-logs-to-blob-storage"></a>Diagnostische hulpprogramma's om over te brengen van de logboeken aan de taak opstarten om blob storage configureren 
U kunt Diagnostisch hulpprogramma Azure om over te brengen van de logboekbestanden die zijn gegenereerd door het opstartscript of het installatieprogramma van .NET met blob storage configureren om te vereenvoudigen installeren problemen oplossen. Met deze methode kunt u de logboeken weergeven door gewoon de logboekbestanden van blobopslag downloaden en hoeven te extern bureaublad aan de rol.

Diagnostische hulpprogramma's openen de *diagnostics.wadcfgx* configureren en de volgende handelingen uit onder het knooppunt **mappen** toevoegen: 

```xml 
<DataSources>
 <DirectoryConfiguration containerName="netfx-install">
  <LocalResource name="NETFXInstall" relativePath="log"/>
 </DirectoryConfiguration>
</DataSources>
```

Hiermee configureert azure diagnostische hulpprogramma's om alle bestanden in de map *logboek* onder de resource *NETFXInstall* overbrengen naar het account van de opslagruimte diagnostische gegevens in de container van de blob *netfx installeren* .

## <a name="deploying-your-service"></a>Uw service implementeren 
Wanneer u implementeert in uw service wordt de opstarttaken uitvoeren en .NET framework installeren als dit nog niet is gebeurd. Uw rollen worden weergegeven in de bezet staat terwijl het kader wordt geïnstalleerd en zelfs mogelijk opnieuw als de installatie van framework vereist. 

## <a name="additional-resources"></a>Aanvullende informatie

- [Installatie van .NET Framework][]
- [Hoe u: bepalen welke .NET Framework-versies zijn geïnstalleerd][]
- [.NET Framework installaties probleemoplossing][]

[Hoe u: bepalen welke .NET Framework-versies zijn geïnstalleerd]: https://msdn.microsoft.com/library/hh925568.aspx
[Installatie van .NET Framework]: https://msdn.microsoft.com/library/5a4x27ek.aspx
[.NET Framework installaties probleemoplossing]: https://msdn.microsoft.com/library/hh925569.aspx

<!--Image references-->
[1]: ./media/cloud-services-dotnet-install-dotnet/rolecontentwithinstallerfiles.png
[2]: ./media/cloud-services-dotnet-install-dotnet/rolecontentwithallfiles.png

 
