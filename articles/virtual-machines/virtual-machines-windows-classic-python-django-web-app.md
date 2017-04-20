<properties
    pageTitle="Python web app met Django | Microsoft Azure"
    description="Deze zelfstudie leert u hoe u voor het hosten van een website Django gebaseerde op Azure met Windows Server 2012 R2 Datacenter virtuele machines met het implementatiemodel klassieke."
    services="virtual-machines-windows"
    documentationCenter="python"
    authors="huguesv"
    manager="wpickett"
    editor=""
    tags="azure-service-management"/>


<tags 
    ms.service="virtual-machines-windows" 
    ms.workload="web" 
    ms.tgt_pltfrm="vm-windows" 
    ms.devlang="python" 
    ms.topic="article" 
    ms.date="08/04/2015" 
    ms.author="huvalo"/>


# <a name="django-hello-world-web-application-on-a-windows-server-vm"></a>De webtoepassing Django Hallo allemaal op een Windows Server VM

> [AZURE.SELECTOR]
- [Windows](virtual-machines-windows-classic-python-django-web-app.md)
- [Mac/Linux](virtual-machines-linux-python-django-web-app.md)

<br>

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]
 

Deze zelfstudie wordt beschreven hoe voor het hosten van een website Django gebaseerde op Microsoft Azure met een Windows Server virtuele machine. Deze zelfstudie wordt ervan uitgegaan dat u hebt geen ervaring met Azure. Na het voltooien van deze zelfstudie, hebt u een Django-toepassing omhoog en worden uitgevoerd in de cloud.

U leert hoe u:

* Een Azure virtuele machines aan host Django instellen. Terwijl deze zelfstudie wordt uitgelegd hoe u hiervoor onder Windows Server, kan hetzelfde ook worden uitgevoerd met een Linux VM gehost in Azure wordt aangegeven.
* Een nieuwe Django-toepassing maken van Windows.

Door deze zelfstudie te volgen, stelt u een eenvoudige Hallo allemaal-webtoepassing. De toepassing wordt in een Azure virtuele machines worden gehost.

Schermafbeelding van de voltooide toepassing weergegeven volgende.

![Een browservenster weergeven van de pagina van de wereld Hallo op Azure][1]

[AZURE.INCLUDE [create-account-and-vms-note](../../includes/create-account-and-vms-note.md)]

## <a name="creating-and-configuring-an-azure-virtual-machine-to-host-django"></a>Maken en configureren van een Azure virtuele machines aan host Django

1. Volg de instructies opgegeven [hier](virtual-machines-windows-classic-tutorial.md) een Azure virtuele machine van de verdeling van Windows Server 2012 R2 Datacenter maken.

1. Geef de instructie om Azure naar verkeer poort 80 vanaf het web naar poort 80 op de virtuele machine:
 - Ga naar de zojuist gemaakte VM in de portal van Azure klassieke en klik op het tabblad **EINDPUNTEN** .
 - Klik op de knop **toevoegen** onder aan het scherm.
    ![eindpunt toevoegen](./media/virtual-machines-windows-classic-python-django-web-app/django-helloworld-addendpoint.png)

 - Open het **TCP** -protocol is **openbare poort 80** als **privé poort 80**.
![][port80]
1. Klik op het tabblad **DASHBOARD** op **verbinding maken met** om **Extern bureaublad** te extern Meld u aan bij de zojuist gemaakte Azure virtuele machine gebruiken.  

**Belangrijke opmerking:** Alle onderstaande instructies wordt ervan uitgegaan dat u de virtuele machine correct aangemeld en opdrachten er in plaats van uw lokale computer wordt uitgebracht.

## <a id="setup"> </a>Python, Django, WFastCGI installeren

**Notitie:** Om te downloaden met Internet Explorer mogelijk moet u IE ESC-instellingen configureren (begindatum/Projectnaamadministratief hulpmiddelen voor/Server Manager/lokale Server, klik op **Verbeterde beveiliging van IE**, uitgeschakeld).

1. Installeer de meest recente Python 2.7 of 3.4 vanaf [python.org][].
1. Installeer de wfastcgi en django-pakketten pip gebruiken.

    Gebruik de volgende opdracht voor Python 2.7.

        c:\python27\scripts\pip install wfastcgi
        c:\python27\scripts\pip install django

    Gebruik de volgende opdracht voor Python 3.4.

        c:\python34\scripts\pip install wfastcgi
        c:\python34\scripts\pip install django

## <a name="installing-iis-with-fastcgi"></a>Installeren van IIS met FastCGI

1. IIS met FastCGI-ondersteuning installeert.  Dit kan enige tijd duren om uit te voeren.

        start /wait %windir%\System32\PkgMgr.exe /iu:IIS-WebServerRole;IIS-WebServer;IIS-CommonHttpFeatures;IIS-StaticContent;IIS-DefaultDocument;IIS-DirectoryBrowsing;IIS-HttpErrors;IIS-HealthAndDiagnostics;IIS-HttpLogging;IIS-LoggingLibraries;IIS-RequestMonitor;IIS-Security;IIS-RequestFiltering;IIS-HttpCompressionStatic;IIS-WebServerManagementTools;IIS-ManagementConsole;WAS-WindowsActivationService;WAS-ProcessModel;WAS-NetFxEnvironment;WAS-ConfigurationAPI;IIS-CGI

## <a name="creating-a-new-django-application"></a>Een nieuwe Django-toepassing maken

1.  Uit *C:\inetpub\wwwroot*, voert u de volgende opdracht uit om een nieuwe Django project te maken:

    Gebruik de volgende opdracht voor Python 2.7.

        C:\Python27\Scripts\django-admin.exe startproject helloworld

    Gebruik de volgende opdracht voor Python 3.4.

        C:\Python34\Scripts\django-admin.exe startproject helloworld

    ![Het resultaat van de opdracht Nieuw AzureService](./media/virtual-machines-windows-classic-python-django-web-app/django-helloworld-cmd-new-azure-service.png)

1.  De opdracht **django-beheerders** genereert een basisstructuur voor Django gebaseerde websites:

  -   **helloworld\manage.PY** helpt u bij het hostingprovider starten en stoppen uw website Django gebaseerde hosten
  -   **helloworld\helloworld\settings.PY** bevat Django-instellingen voor uw toepassing.
  -   **helloworld\helloworld\urls.PY** bevat de code toewijzing tussen elke url en de weergave.

1.  Maak een nieuw bestand met de naam **views.py** in de adreslijst *C:\inetpub\wwwroot\helloworld\helloworld* . Hiermee wordt de weergave die worden weergegeven door de pagina 'Hallo allemaal' bevatten. Start uw editor en voer de volgende gegevens:

        from django.http import HttpResponse
        def home(request):
            html = "<html><body>Hello World!</body></html>"
            return HttpResponse(html)

1.  De inhoud van het bestand urls.py vervangen door het volgende.

        from django.conf.urls import patterns, url
        urlpatterns = patterns('',
            url(r'^$', 'helloworld.views.home', name='home'),
        )

## <a name="configuring-iis"></a>IIS configureren

1. De sectie handlers in de globale applicationhost.config ontgrendelen.  Hiermee schakelt u het gebruik van de handler python in uw web.config.

        %windir%\system32\inetsrv\appcmd unlock config -section:system.webServer/handlers

1. WFastCGI inschakelen.  Hiermee wordt een toepassing toevoegen aan de globale applicationhost.config die naar uw Python interpreter uitvoerbare en het script wfastcgi.py verwijst.

    Python 2.7:

        c:\python27\scripts\wfastcgi-enable

    Python 3.4:

        c:\python34\scripts\wfastcgi-enable

1. Een bestand web.config in *C:\inetpub\wwwroot\helloworld*maken.  De waarde van de `scriptProcessor` kenmerk moet overeenkomen met de uitvoer van de vorige stap.  Zie de pagina voor [wfastcgi][] op pypi voor meer aan wfastcgi-instellingen.

    Python 2.7:

        <configuration>
          <appSettings>
            <add key="WSGI_HANDLER" value="django.core.handlers.wsgi.WSGIHandler()" />
            <add key="PYTHONPATH" value="C:\inetpub\wwwroot\helloworld" />
            <add key="DJANGO_SETTINGS_MODULE" value="helloworld.settings" />
          </appSettings>
          <system.webServer>
            <handlers>
                <add name="Python FastCGI" path="*" verb="*" modules="FastCgiModule" scriptProcessor="C:\Python27\python.exe|C:\Python27\Lib\site-packages\wfastcgi.pyc" resourceType="Unspecified" />
            </handlers>
          </system.webServer>
        </configuration>

    Python 3.4:

        <configuration>
          <appSettings>
            <add key="WSGI_HANDLER" value="django.core.handlers.wsgi.WSGIHandler()" />
            <add key="PYTHONPATH" value="C:\inetpub\wwwroot\helloworld" />
            <add key="DJANGO_SETTINGS_MODULE" value="helloworld.settings" />
          </appSettings>
          <system.webServer>
            <handlers>
                <add name="Python FastCGI" path="*" verb="*" modules="FastCgiModule" scriptProcessor="C:\Python34\python.exe|C:\Python34\Lib\site-packages\wfastcgi.py" resourceType="Unspecified" />
            </handlers>
          </system.webServer>
        </configuration>

1. De locatie van de IIS standaardwebsite zodat deze verwijzen naar de map met django project bijwerken.

        %windir%\system32\inetsrv\appcmd set vdir "Default Web Site/" -physicalPath:"C:\inetpub\wwwroot\helloworld"

1. Tot slot laad de pagina met webonderdelen in uw browser.

![Een browservenster weergeven van de pagina van de wereld Hallo op Azure][1]


## <a name="shutting-down-your-azure-virtual-machine"></a>Uw Azure virtuele machines afsluiten

Wanneer u klaar bent met deze zelfstudie, afsluiten en/of de zojuist gemaakte Azure virtuele machines als bronnen vrij te maken voor andere zelfstudies en wilt voorkomen dat Azure gebruikskosten verwijderen.

[1]: ./media/virtual-machines-windows-classic-python-django-web-app/django-helloworld-browser-azure.png

[port80]: ./media/virtual-machines-windows-classic-python-django-web-app/django-helloworld-port80.png

[Web Platform Installer]: http://www.microsoft.com/web/downloads/platform.aspx
[Python.org]: https://www.python.org/downloads/
[wfastcgi]: https://pypi.python.org/pypi/wfastcgi
