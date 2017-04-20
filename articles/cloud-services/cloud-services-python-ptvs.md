<properties
    pageTitle="Python web en werknemer rollen met Visual Studio | Microsoft Azure"
    description="Overzicht van het gebruik van Python Tools voor Visual Studio Azure cloudservices met inbegrip van web rollen en werknemer rollen maken."
    services="cloud-services"
    documentationCenter="python"
    authors="thraka"
    manager="timlt"
    editor=""/>

<tags
    ms.service="cloud-services"
    ms.workload="tbd"
    ms.tgt_pltfrm="na"
    ms.devlang="python"
    ms.topic="hero-article"
    ms.date="08/03/2016"
    ms.author="adegeo"/>


# <a name="python-web-and-worker-roles-with-python-tools-for-visual-studio"></a>Python web en werknemer rollen met Python Tools voor Visual Studio

Dit artikel bevat een overzicht van het gebruik van Python web en werknemer rollen met [Python Tools for Visual Studio][]. Hier leert u hoe u het gebruik van Visual Studio maken en implementeren van een eenvoudige Cloudservice die wordt gebruikt Python.

## <a name="prerequisites"></a>Vereisten voor

 - Visual Studio 2013 of 2015
 - [Python Tools voor Visual Studio][] (PTVS)
 - [Azure SDK-hulpmiddelen voor tegenover 2015][] [Azure SDK voor voor tegenover 2013][] of
 - [Python 2.7 32-bits][] of [Python 3.5 32-bits][]

[AZURE.INCLUDE [create-account-and-websites-note](../../includes/create-account-and-websites-note.md)]

## <a name="what-are-python-web-and-worker-roles"></a>Wat zijn de rollen web en werknemer Python?

Azure biedt drie modellen voor het uitvoeren van toepassingen berekenen: [Web Apps-functie in Azure App Service][execution model-web sites], [Azure virtuele Machines][execution model-vms], en [Azure-Cloudservices][execution model-cloud services]. Alle drie modellen ondersteunen Python. Cloudservices, waaronder web en werknemer rollen, bieden *Platform als een Service (PaaS)*. Binnen een cloudservice biedt een Webrol een speciale Internet Information Services (IIS)-webserver voor host front-webtoepassingen, terwijl de rol van een werknemer asynchroon, langdurige of doorlopende taken onafhankelijk van interactie van de gebruiker of invoer kan worden uitgevoerd.

Zie voor meer informatie [Wat is een Cloudservice?].

> [AZURE.NOTE]*Looking maken van een eenvoudige website?*
Als uw scenario heeft betrekking op alleen een eenvoudige website front, kunt u overwegen de lightweight Web Apps-functie in Azure App-Service. U kunt eenvoudig upgraden naar een Cloudservice, zoals uw website in omvang groeit, en uw vereisten voor wijzigen. Zie het <a href="/develop/python/">Python Developer Center</a> voor artikelen waarin de ontwikkeling van de functie Web Apps in Azure App-Service.
<br />


## <a name="project-creation"></a>Maken van projecten

U kunt in Visual Studio **Azure-Cloudservice** in het dialoogvenster **Nieuw Project** onder **Python**selecteren.

![Dialoogvenster voor de nieuwe Project](./media/cloud-services-python-ptvs/new-project-cloud-service.png)

Klik in de wizard Azure-Cloudservice, kunt u nieuwe web en werknemer rollen.

![Dialoogvenster van Azure Cloud-Service](./media/cloud-services-python-ptvs/new-service-wizard.png)

De werknemer rolsjabloon wordt geleverd met standaardtekst code verbinding maken met een Azure opslag-account of Azure Service Bus.

![Cloud Service-oplossing](./media/cloud-services-python-ptvs/worker.png)

U kunt rollen web of werknemer toevoegen aan een bestaande cloudservice op elk gewenst moment.  U kunt bestaande projecten in uw oplossing toevoegen of nieuwe bestanden maken.

![De opdracht rol toevoegen](./media/cloud-services-python-ptvs/add-new-or-existing-role.png)

Uw cloudservice kan rollen geïmplementeerd in verschillende talen bevatten.  U kunt bijvoorbeeld een Python Webrol geïmplementeerd met Django, met Python of met C# werknemer rollen hebben.  U kunt eenvoudig communiceren tussen uw rollen met Service Bus of opslag wachtrijen.

## <a name="install-python-on-the-cloud-service"></a>Python installeren op de cloudservice

>[AZURE.WARNING] De setup-scripts die worden geïnstalleerd met Visual Studio (op het moment dat in dit artikel voor het laatst werd bijgewerkt) werken niet. Dit onderwerp vindt een tijdelijke oplossing.

Het belangrijkste probleem met de setup-scripts zijn dat ze niet python installeert. Eerste stap definieert u twee [opstarttaken](cloud-services-startup-tasks.md) in het bestand [ServiceDefinition.csdef](cloud-services-model-and-package.md#servicedefinitioncsdef) . De eerste taak (**PrepPython.ps1**) downloadt en installeert de runtime Python. De tweede taak (**PipInstaller.ps1**) wordt uitgevoerd pip als u wilt installeren, moet u wellicht afhankelijk.

De onderstaande scripts zijn geschreven Python 3.5 doelgroepen. Als u wilt gebruiken van de versie 2.x van python, het **PYTHON2** variabele bestand ingesteld op **op** voor de twee opstarttaken bij en de taak runtime: `<Variable name="PYTHON2" value="<mark>on</mark>" />`.


```xml
<Startup>

  <Task executionContext="elevated" taskType="simple" commandLine="bin\ps.cmd PrepPython.ps1">
    <Environment>
      <Variable name="EMULATED">
        <RoleInstanceValue xpath="/RoleEnvironment/Deployment/@emulated" />
      </Variable>
      <Variable name="PYTHON2" value="off" />
    </Environment>
  </Task>

  <Task executionContext="elevated" taskType="simple" commandLine="bin\ps.cmd PipInstaller.ps1">
    <Environment>
      <Variable name="EMULATED">
        <RoleInstanceValue xpath="/RoleEnvironment/Deployment/@emulated" />
      </Variable>
      <Variable name="PYTHON2" value="off" />
    </Environment>
    
  </Task>

</Startup>
```

De variabelen **PYTHON2** en **PYPATH** moet worden toegevoegd aan de taak van werknemer opstarten. De variabele **PYPATH** wordt alleen gebruikt als de **PYTHON2** -variabele is ingesteld op **op**.

```xml
<Runtime>
  <Environment>
    <Variable name="EMULATED">
      <RoleInstanceValue xpath="/RoleEnvironment/Deployment/@emulated" />
    </Variable>
    <Variable name="PYTHON2" value="off" />
    <Variable name="PYPATH" value="%SystemDrive%\Python27" />
  </Environment>
  <EntryPoint>
    <ProgramEntryPoint commandLine="bin\ps.cmd LaunchWorker.ps1" setReadyOnProcessStart="true" />
  </EntryPoint>
</Runtime>
```

#### <a name="sample-servicedefinitioncsdef"></a>Voorbeeld van de ServiceDefinition.csdef

```xml
<?xml version="1.0" encoding="utf-8"?>
<ServiceDefinition name="AzureCloudServicePython" xmlns="http://schemas.microsoft.com/ServiceHosting/2008/10/ServiceDefinition" schemaVersion="2015-04.2.6">
  <WorkerRole name="WorkerRole1" vmsize="Small">
    <ConfigurationSettings>
      <Setting name="Microsoft.WindowsAzure.Plugins.Diagnostics.ConnectionString" />
      <Setting name="Python2" />
    </ConfigurationSettings>
    <Startup>
      <Task executionContext="elevated" taskType="simple" commandLine="bin\ps.cmd PrepPython.ps1">
        <Environment>
          <Variable name="EMULATED">
            <RoleInstanceValue xpath="/RoleEnvironment/Deployment/@emulated" />
          </Variable>
          <Variable name="PYTHON2" value="off" />
        </Environment>
      </Task>
      <Task executionContext="elevated" taskType="simple" commandLine="bin\ps.cmd PipInstaller.ps1">
        <Environment>
          <Variable name="EMULATED">
            <RoleInstanceValue xpath="/RoleEnvironment/Deployment/@emulated" />
          </Variable>
          <Variable name="PYTHON2" value="off" />
        </Environment>
      </Task>
    </Startup>
    <Runtime>
      <Environment>
        <Variable name="EMULATED">
          <RoleInstanceValue xpath="/RoleEnvironment/Deployment/@emulated" />
        </Variable>
        <Variable name="PYTHON2" value="off" />
        <Variable name="PYPATH" value="%SystemDrive%\Python27" />
      </Environment>
      <EntryPoint>
        <ProgramEntryPoint commandLine="bin\ps.cmd LaunchWorker.ps1" setReadyOnProcessStart="true" />
      </EntryPoint>
    </Runtime>
    <Imports>
      <Import moduleName="RemoteAccess" />
      <Import moduleName="RemoteForwarder" />
    </Imports>
  </WorkerRole>
</ServiceDefinition>
```



Maak vervolgens de bestanden **PrepPython.ps1** en **PipInstaller.ps1** in de **. / bin** map van uw rol.

#### <a name="preppythonps1"></a>PrepPython.ps1

Dit script installeert python. Als de **PYTHON2** omgeving-variabele is ingesteld op **op** vervolgens Python 2.7 worden geïnstalleerd, anders Python 3.5 wordt geïnstalleerd.

```powershell
$is_emulated = $env:EMULATED -eq "true"
$is_python2 = $env:PYTHON2 -eq "on"
$nl = [Environment]::NewLine

if (-not $is_emulated){
    Write-Output "Checking if python is installed...$nl"
    if ($is_python2) {
        & "${env:SystemDrive}\Python27\python.exe"  -V | Out-Null
    }
    else {
        py -V | Out-Null
    }

    if (-not $?) {

        $url = "https://www.python.org/ftp/python/3.5.2/python-3.5.2-amd64.exe"
        $outFile = "${env:TEMP}\python-3.5.2-amd64.exe"

        if ($is_python2) {
            $url = "https://www.python.org/ftp/python/2.7.12/python-2.7.12.amd64.msi"
            $outFile = "${env:TEMP}\python-2.7.12.amd64.msi"
        }
        
        Write-Output "Not found, downloading $url to $outFile$nl"
        Invoke-WebRequest $url -OutFile $outFile
        Write-Output "Installing$nl"

        if ($is_python2) {
            Start-Process msiexec.exe -ArgumentList "/q", "/i", "$outFile", "ALLUSERS=1" -Wait
        }
        else {
            Start-Process "$outFile" -ArgumentList "/quiet", "InstallAllUsers=1" -Wait
        }

        Write-Output "Done$nl"
    }
    else {
        Write-Output "Already installed"
    }
}
```

#### <a name="pipinstallerps1"></a>PipInstaller.ps1

Dit script pip belt en alle de afhankelijkheden in het bestand **requirements.txt** is geïnstalleerd. Als de **PYTHON2** omgeving-variabele is ingesteld op **op** vervolgens Python 2.7 wordt gebruikt, anders Python 3.5 wordt gebruikt.

```powershell
$is_emulated = $env:EMULATED -eq "true"
$is_python2 = $env:PYTHON2 -eq "on"
$nl = [Environment]::NewLine

if (-not $is_emulated){
    Write-Output "Checking if requirements.txt exists$nl"
    if (Test-Path ..\requirements.txt) {
        Write-Output "Found. Processing pip$nl"

        if ($is_python2) {
            & "${env:SystemDrive}\Python27\python.exe" -m pip install -r ..\requirements.txt
        }
        else {
            py -m pip install -r ..\requirements.txt
        }

        Write-Output "Done$nl"
    }
    else {
        Write-Output "Not found$nl"
    }
}
```

#### <a name="modify-launchworkerps1"></a>LaunchWorker.ps1 wijzigen

>[AZURE.NOTE] Voor een project **werknemer rol** is **LauncherWorker.ps1** bestand vereist voor het uitvoeren van het opstartbestand. Het opstartbestand wordt in project **Webrol** in plaats daarvan gedefinieerd in de Projecteigenschappen van het.

De **bin\LaunchWorker.ps1** oorspronkelijk hiervoor een groot aantal voorbereidingen is gemaakt, maar dit echt niet werkt. De inhoud van dat bestand vervangen door het volgende script.

Het bestand **worker.py** dit script gebeld vanaf uw project python. Als de **PYTHON2** omgeving-variabele is ingesteld op **op** vervolgens Python 2.7 wordt gebruikt, anders Python 3.5 wordt gebruikt.

```powershell
$is_emulated = $env:EMULATED -eq "true"
$is_python2 = $env:PYTHON2 -eq "on"
$nl = [Environment]::NewLine

if (-not $is_emulated)
{
    Write-Output "Running worker.py$nl"

    if ($is_python2) {
        cd..
        iex "$env:PYPATH\python.exe worker.py"
    }
    else {
        cd..
        iex "py worker.py"
    }
}
else
{
    Write-Output "Running (EMULATED) worker.py$nl"

    # Customize to your local dev environment

    if ($is_python2) {
        cd..
        iex "$env:PYPATH\python.exe worker.py"
    }
    else {
        cd..
        iex "py worker.py"
    }
}
```

#### <a name="pscmd"></a>PS.cmd

De sjablonen voor Visual Studio moeten hebt gemaakt op een bestand **ps.cmd** in de **. / bin** map. Deze shellscript ziet u de bovenstaande PowerShell Wikkel scripts en biedt logboekregistratie op basis van de naam van de PowerShell-Wikkel genoemd. Als dit bestand niet is gemaakt, moet u hier ziet wat erin moet zijn. 

```bat
@echo off

cd /D %~dp0

if not exist "%DiagnosticStore%\LogFiles" mkdir "%DiagnosticStore%\LogFiles"
%SystemRoot%\System32\WindowsPowerShell\v1.0\powershell.exe -ExecutionPolicy Unrestricted -File %* >> "%DiagnosticStore%\LogFiles\%~n1.txt" 2>> "%DiagnosticStore%\LogFiles\%~n1.err.txt"
```



## <a name="run-locally"></a>Lokaal uitvoeren

Als u uw cloud service-project als het opstartproject instellen en druk op F5, is de cloudservice wordt uitgevoerd in de lokale Azure emulator.

Hoewel PTVS ondersteunt starten in de emulator, voor foutopsporing in (bijvoorbeeld onderbrekingspunten) niet werkt.

Voor foutopsporing op het web en werknemer rollen, kunt u de rol-project instellen als het opstartproject en die in plaats daarvan foutopsporing.  U kunt ook meerdere opstarten projecten instellen.  Met de rechtermuisknop op de oplossing en selecteer vervolgens **Opstarten projecten instellen**.

![Opstartopties voor Project oplossing](./media/cloud-services-python-ptvs/startup.png)

## <a name="publish-to-azure"></a>Publiceren naar Azure

Als u wilt publiceren, met de rechtermuisknop op het project cloud-service in de oplossing en selecteer vervolgens **publiceren**.

![Microsoft Azure publiceren aanmelden](./media/cloud-services-python-ptvs/publish-sign-in.png)

Voer de wizard. Als u wilt, kunt u extern bureaublad inschakelen. Extern bureaublad is handig wanneer u moet fouten opsporen in een ander nummer.

Wanneer u klaar bent met de configuratie van de instellingen, klik op **publiceren**.

Sommige voortgang wordt weergegeven in het uitvoervenster en klik vervolgens ziet u het venster Microsoft Azure activiteitenlogboek.

![Microsoft Azure Log activiteitvenster](./media/cloud-services-python-ptvs/publish-activity-log.png)

Implementatie worden enkele minuten duren om uit te voeren en vervolgens uw web-en/of werknemer rollen wordt uitgevoerd op Azure!

### <a name="investigate-logs"></a>Logboeken onderzoeken

Nadat de cloud-service virtuele machine wordt gestart en Python geïnstalleerd, kunt u de logboeken om te zoeken foutberichten bekijken. Deze logboeken zich bevinden in de **C:\Resources\Directory\\{rol} \LogFiles** map. **PrepPython.err.txt** heeft ten minste één fout in deze uit wanneer het script probeert aan te melden als Python wordt geïnstalleerd en **PipInstaller.err.txt** over een verouderde versie van pip klacht mogelijk detecteren.

## <a name="next-steps"></a>Volgende stappen

Zie de documentatie PTVS voor meer gedetailleerde informatie over het werken met web en werknemer rollen in Python Tools voor Visual Studio:

- [Cloud serviceprojecten][]

Zie de volgende artikelen voor meer informatie over het gebruik van Azure services van uw web en werknemer rollen, zoals met Azure Storage of Service Bus.

- [BLOB-Service][]
- [Tabel-Service][]
- [Wachtrijservice][]
- [Service Bus wachtrijen][]
- [Service Bus onderwerpen][]


<!--Link references-->

[Wat is een Cloudservice?]: cloud-services-choose-me.md
[execution model-web sites]: ../app-service-web/app-service-web-overview.md
[execution model-vms]: ../virtual-machines/virtual-machines-windows-about.md
[execution model-cloud services]: cloud-services-choose-me.md
[Python Developer Center]: /develop/python/

[BLOB-Service]: ../storage/storage-python-how-to-use-blob-storage.md
[Wachtrijservice]: ../storage/storage-python-how-to-use-queue-storage.md
[Tabel-Service]: ../storage/storage-python-how-to-use-table-storage.md
[Service Bus wachtrijen]: ../service-bus-messaging/service-bus-python-how-to-use-queues.md
[Service Bus onderwerpen]: ../service-bus-messaging/service-bus-python-how-to-use-topics-subscriptions.md


<!--External Link references-->

[Python Tools voor Visual Studio]: http://aka.ms/ptvs
[Python Tools for Visual Studio Documentation]: http://aka.ms/ptvsdocs
[Cloud serviceprojecten]: http://go.microsoft.com/fwlink/?LinkId=624028
[Azure SDK hulpprogramma's voor tegenover 2013]: http://go.microsoft.com/fwlink/?LinkId=323510
[Azure SDK hulpprogramma's voor tegenover 2015]: http://go.microsoft.com/fwlink/?LinkId=518003
[Python 2.7 32-bits]: https://www.python.org/downloads/
[Python 3.5 32-bits]: https://www.python.org/downloads/
