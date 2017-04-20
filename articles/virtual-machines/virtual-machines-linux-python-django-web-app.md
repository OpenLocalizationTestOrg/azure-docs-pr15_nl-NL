<properties 
    pageTitle="Python web app met Django op Linux | Microsoft Azure" 
    description="Leer hoe u een Django gebaseerde webtoepassing op Azure met een Linux virtuele machine hosten." 
    services="virtual-machines-linux" 
    documentationCenter="python" 
    authors="huguesv" 
    manager="wpickett" 
    editor=""
    tags="azure-resource-manager"/>

<tags 
    ms.service="virtual-machines-linux" 
    ms.workload="web" 
    ms.tgt_pltfrm="vm-linux" 
    ms.devlang="python" 
    ms.topic="article" 
    ms.date="11/17/2015" 
    ms.author="huvalo"/>
    
# <a name="django-hello-world-web-application-on-a-linux-vm"></a>De webtoepassing Django Hallo allemaal op een VM Linux

> [AZURE.SELECTOR]
- [Windows](virtual-machines-windows-classic-python-django-web-app.md)
- [Mac/Linux](virtual-machines-linux-python-django-web-app.md)

<br>

Deze zelfstudie wordt beschreven hoe voor het hosten van een website Django gebaseerde op Microsoft Azure met een VM Linux. Deze zelfstudie wordt ervan uitgegaan dat u hebt geen ervaring met Azure. Na het voltooien van deze handleiding, hebt u een Django-toepassing omhoog en worden uitgevoerd in de cloud.

U leert hoe u:

* Een Azure virtuele machines aan host Django-instelling. Terwijl deze zelfstudie wordt uitgelegd hoe u hiervoor onder **Linux**, kan hetzelfde ook worden uitgevoerd met een Windows Server VM gehost in Azure wordt aangegeven. 
* Een nieuwe Django-toepassing maken van Linux.

Door deze zelfstudie te volgen, stelt u een eenvoudige Hallo allemaal-webtoepassing. De toepassing wordt in een Azure virtuele machines worden gehost.

Schermafbeelding van de voltooide toepassing is hieronder:

![Een browservenster weergeven van de pagina van de wereld Hallo op Azure](./media/virtual-machines-linux-python-django-web-app/mac-linux-django-helloworld-browser.png)

[AZURE.INCLUDE [create-account-and-vms-note](../../includes/create-account-and-vms-note.md)]

## <a name="creating-and-configuring-an-azure-virtual-machine-to-host-django"></a>Maken en configureren van een Azure virtuele machines aan host Django

1. Volg de instructies opgegeven [hier](virtual-machines-linux-quick-create-portal.md) een Azure virtuele machine van de verdeling *Ubuntu Server 14.04 LTS* maken.  Als u liever, kunt u in plaats van de openbare sleutel SSH-wachtwoordverificatie.

1. Bewerken van de beveiligingsgroep van netwerk zodat binnenkomende HTTP-verkeer poort 80 volgens de instructies [hier](../virtual-network/virtual-networks-create-nsg-arm-pportal.md).

1. Uw nieuwe virtuele machine heeft een volledig gekwalificeerde domeinnaam (FQDN) geen standaard.  U kunt maken door de instructies te volgen [hier](virtual-machines-linux-portal-create-fqdn.md).  Deze stap is optioneel als u wilt deze zelfstudie te voltooien.

## <a id="setup"> </a>Instellen van de ontwikkelomgeving

**Notitie:** Als u moet installeren Python of wilt de Client-bibliotheken gebruiken, raadpleegt u de [Installatiehandleiding voor Python](../python-how-to-install.md).

De Ubuntu Linux VM al wordt geleverd met Python 2.7 vooraf geïnstalleerd, maar deze geen Apache of django is geïnstalleerd.  Volg deze stappen om de verbinding maken met uw VM en installeer Apache and django.

1.  Start een nieuw **Terminal** -venster.
    
1.  Voer de volgende opdracht verbinding maken met de VM Azure.  Als u een FQDN niet zelf hebt gemaakt, kunt u verbinding maken met het openbare IP-adres in het overzicht in de portal van Azure klassieke VM weergegeven.

        $ ssh yourusername@yourVmUrl

1.  Voer de volgende opdrachten voor het installeren van django:

        $ sudo apt-get install python-setuptools python-pip
        $ sudo pip install django

1.  Voer de volgende opdracht Apache met rest-wsgi installeren:

        $ sudo apt-get install apache2 libapache2-mod-wsgi


## <a name="creating-a-new-django-application"></a>Een nieuwe Django-toepassing maken

1.  Open het **Terminal** -venster die u hebt gebruikt in de vorige sectie om te ssh in uw VM.
    
1.  Voer de volgende opdrachten om een nieuwe Django project te maken:

        $ cd /var/www
        $ sudo django-admin.py startproject helloworld

    Het script **django-admin.py** genereert een basisstructuur voor Django gebaseerde websites:
    -   **HelloWorld/Manage.PY** helpt u bij het hostingprovider starten en stoppen uw website Django gebaseerde hosten
    -   **HelloWorld/HelloWorld/Settings.PY** bevat Django-instellingen voor uw toepassing.
    -   **HelloWorld/HelloWorld/URLs.PY** bevat de code toewijzing tussen elke url en de weergave.

1.  Maak een nieuw bestand met de naam **views.py** in de adreslijst **/var/www/helloworld/helloworld** . Hiermee wordt de weergave die worden weergegeven door de pagina 'Hallo allemaal' bevatten. Start uw editor en voer de volgende gegevens:
        
        from django.http import HttpResponse
        def home(request):
            html = "<html><body>Hello World!</body></html>"
            return HttpResponse(html)

1.  Nu de inhoud van het bestand **urls.py** vervangen door het volgende:

        from django.conf.urls import patterns, url
        urlpatterns = patterns('',
            url(r'^$', 'helloworld.views.home', name='home'),
        )


## <a name="setting-up-apache"></a>Bij het instellen van Apache

1.  Maak een Apache virtuele host configuratie bestand **/etc/apache2/sites-available/helloworld.conf**. Stelt u de inhoud op de volgende handelingen uit en *yourVmName* vervangen door de feitelijke naam van de computer die u (voor voorbeeld *pyubuntu gebruikt*).

        <VirtualHost *:80>
        ServerName yourVmName
        </VirtualHost>
        WSGIScriptAlias / /var/www/helloworld/helloworld/wsgi.py
        WSGIPythonPath /var/www/helloworld

1.  Schakelen van de site met de volgende opdracht uit:

        $ sudo a2ensite helloworld

1.  Opnieuw Apache starten met de volgende opdracht uit:

        $ sudo service apache2 reload

1.  Tot slot laadt u de pagina met webonderdelen in uw browser:

    ![Een browservenster weergeven van de pagina van de wereld Hallo op Azure](./media/virtual-machines-linux-python-django-web-app/mac-linux-django-helloworld-browser.png)


## <a name="shutting-down-your-azure-virtual-machine"></a>Uw Azure virtuele machines afsluiten

Wanneer u klaar bent met deze zelfstudie, afsluiten/verwijderen of de zojuist gemaakte Azure virtuele machine bronnen vrij te maken voor andere zelfstudies en te voorkomen dat Azure gebruikskosten.
