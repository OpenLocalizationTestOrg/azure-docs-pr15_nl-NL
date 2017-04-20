<properties 
    pageTitle="Python configureren met Azure App Service WebApps" 
    description="Deze zelfstudie worden opties voor het schrijven en configureren van een eenvoudige webserver Gateway Interface (WSGI) compatibele Python toepassing op Azure App Service Web Apps." 
    services="app-service" 
    documentationCenter="python" 
    tags="python"
    authors="huguesv" 
    manager="wpickett" 
    editor=""/>

<tags 
    ms.service="app-service" 
    ms.workload="na" 
    ms.tgt_pltfrm="na" 
    ms.devlang="python" 
    ms.topic="article" 
    ms.date="02/26/2016" 
    ms.author="huvalo"/>




# <a name="configuring-python-with-azure-app-service-web-apps"></a>Python configureren met Azure App Service WebApps

Deze zelfstudie worden opties voor het schrijven en configureren van een eenvoudige Web Server Gateway Interface (WSGI) compatibele Python toepassing op [Azure App Service Web Apps](http://go.microsoft.com/fwlink/?LinkId=529714).

Deze aanvullende functies van cijfer implementatie, zoals virtuele omgeving en de installatie van pakket met requirements.txt wordt beschreven.


## <a name="bottle-django-or-flask"></a>Flessen, Django of kolf?

De Azure Marketplace bevat sjablonen voor de flessen, Django en kolf kaders. Als u uw eerste web-app in Azure App Service ontwikkelt, of u niet bekend met cijfer bent, raden we u aan een van deze zelfstudies, waaronder Stapsgewijze instructies voor het samenstellen van een toepassing werken vanuit de galerie met cijfer implementatie van Windows of Mac:

- [WebApps maken met flessen](web-sites-python-create-deploy-bottle-app.md)
- [WebApps maken met Django](web-sites-python-create-deploy-django-app.md)
- [WebApps maken met kolf](web-sites-python-create-deploy-flask-app.md)


## <a name="web-app-creation-on-azure-portal"></a>Web-app maken op Azure-Portal

Deze zelfstudie wordt ervan uitgegaan van een bestaand Azure-abonnement en toegang tot de Portal Azure.

Als u een bestaande web-app niet hebt, kunt u een van de [Azure-Portal](https://portal.azure.com)maken.  Klik op de knop Nieuw in de linkerbovenhoek en klik op **Web + Mobile** > **WebApp**.

## <a name="git-publishing"></a>Cijfer publiceren

Cijfer publicatie voor uw nieuwe WebApp volgens de instructies op de [Lokale cijfer implementatie naar Azure App-Service](app-service-deploy-local-git.md)configureren. In deze zelfstudie wordt cijfer kunt maken, beheren en publiceren van onze Python web-app naar Azure App-Service.

Zodra het cijfer publicatie is ingesteld, wordt een cijfer opslagplaats gemaakt en dat is gekoppeld aan uw web-app. De URL van de bibliotheek wordt weergegeven en voortaan kan worden gebruikt om gegevens uit de lokale ontwikkelomgeving push in de cloud. Als u wilt publiceren toepassingen via cijfer, Controleer of dat een cijfer-client ook is geïnstalleerd en gebruik de instructies om de inhoud van uw web-app aan App-Azure-Service.


## <a name="application-overview"></a>Overzicht van de toepassing

In de volgende secties worden de volgende bestanden gemaakt. Ze moeten worden teruggezet in de hoofdmap van de bibliotheek cijfer.

    app.py
    requirements.txt
    runtime.txt
    web.config
    ptvs_virtualenv_proxy.py


## <a name="wsgi-handler"></a>WSGI-Handler

WSGI is een Python-standaard beschreven door [PEP 3333](http://www.python.org/dev/peps/pep-3333/) interface tussen de webserver en Python definiëren. Deze biedt een gestandaardiseerde interface voor het schrijven van verschillende webtoepassingen en kaders met Python. Populaire Python web kaders gebruik vandaag WSGI. Azure App Service Web-Apps hebt die u ondersteuning voor deze kaders; Daarnaast kunnen ervaren gebruikers zelfs schrijven hun eigen zo lang maken als de aangepaste handler de richtlijnen van de specificatie WSGI volgt.

Hier volgt een voorbeeld van een `app.py` die een aangepaste handler wordt gedefinieerd:

    def wsgi_app(environ, start_response):
        status = '200 OK'
        response_headers = [('Content-type', 'text/plain')]
        start_response(status, response_headers)
        response_body = 'Hello World'
        yield response_body.encode()

    if __name__ == '__main__':
        from wsgiref.simple_server import make_server

        httpd = make_server('localhost', 5555, wsgi_app)
        httpd.serve_forever()

U kunt uitvoeren van deze toepassing lokaal met `python app.py`, bladert u naar `http://localhost:5555` in uw webbrowser.


## <a name="virtual-environment"></a>Virtuele omgeving

Hoewel het bovenstaande voorbeeld-app niet alle externe-pakketten vereist, is het mogelijk dat uw toepassing worden enkele moeten.

Implementatie van Azure cijfer ondersteunt externe pakketafhankelijkheden beheren, zodat het maken van virtuele omgevingen.

Als een requirements.txt Azure vastgesteld in de hoofdmap van de bibliotheek, wordt automatisch een virtuele omgeving met de naam gemaakt `env`. Dit gebeurt alleen op de eerste implementatie of tijdens een implementatie na de geselecteerde Python runtime is gewijzigd.

U waarschijnlijk wilt maken van een virtuele omgeving lokaal voor de ontwikkeling, maar deze niet opnemen in uw bibliotheek cijfer.


## <a name="package-management"></a>Pakket Management

Pakketten vermeld in requirements.txt wordt automatisch in de virtuele omgeving met pip worden geïnstalleerd. Dit gebeurt er in elke implementatie, maar pip installatie wordt overslaan als een pakket al is geïnstalleerd.

Voorbeeld `requirements.txt`:

    azure==0.8.4


## <a name="python-version"></a>Python versie

[AZURE.INCLUDE [web-sites-python-customizing-runtime](../../includes/web-sites-python-customizing-runtime.md)]

Voorbeeld `runtime.txt`:

    python-2.7


## <a name="webconfig"></a>Web.config

U moet een web.config-bestand als u wilt opgeven hoe de server moet verwerken van aanvragen maken.

Houd er rekening mee dat als er een bestand web.x.y.config in uw bibliotheek, waar x.y overeenkomt met de geselecteerde Python runtime en Azure wordt automatisch kopieert u het juiste bestand als web.config.

De volgende voorbeelden van web.config, is afhankelijk van een virtuele omgeving proxy-script, waarin wordt beschreven in het volgende gedeelte.  Ze werken met de WSGI-handler die wordt gebruikt in het voorbeeld `app.py` hierboven.

Voorbeeld `web.config` voor Python 2.7:

    <?xml version="1.0"?>
    <configuration>
      <appSettings>
        <add key="WSGI_ALT_VIRTUALENV_HANDLER" value="app.wsgi_app" />
        <add key="WSGI_ALT_VIRTUALENV_ACTIVATE_THIS"
             value="D:\home\site\wwwroot\env\Scripts\activate_this.py" />
        <add key="WSGI_HANDLER"
             value="ptvs_virtualenv_proxy.get_virtualenv_handler()" />
        <add key="PYTHONPATH" value="D:\home\site\wwwroot" />
      </appSettings>
      <system.web>
        <compilation debug="true" targetFramework="4.0" />
      </system.web>
      <system.webServer>
        <modules runAllManagedModulesForAllRequests="true" />
        <handlers>
          <remove name="Python27_via_FastCGI" />
          <remove name="Python34_via_FastCGI" />
          <add name="Python FastCGI"
               path="handler.fcgi"
               verb="*"
               modules="FastCgiModule"
               scriptProcessor="D:\Python27\python.exe|D:\Python27\Scripts\wfastcgi.py"
               resourceType="Unspecified"
               requireAccess="Script" />
        </handlers>
        <rewrite>
          <rules>
            <rule name="Static Files" stopProcessing="true">
              <conditions>
                <add input="true" pattern="false" />
              </conditions>
            </rule>
            <rule name="Configure Python" stopProcessing="true">
              <match url="(.*)" ignoreCase="false" />
              <conditions>
                <add input="{REQUEST_URI}" pattern="^/static/.*" ignoreCase="true" negate="true" />
              </conditions>
              <action type="Rewrite"
                      url="handler.fcgi/{R:1}"
                      appendQueryString="true" />
            </rule>
          </rules>
        </rewrite>
      </system.webServer>
    </configuration>


Voorbeeld `web.config` voor Python 3.4:

    <?xml version="1.0"?>
    <configuration>
      <appSettings>
        <add key="WSGI_ALT_VIRTUALENV_HANDLER" value="app.wsgi_app" />
        <add key="WSGI_ALT_VIRTUALENV_ACTIVATE_THIS"
             value="D:\home\site\wwwroot\env\Scripts\python.exe" />
        <add key="WSGI_HANDLER"
             value="ptvs_virtualenv_proxy.get_venv_handler()" />
        <add key="PYTHONPATH" value="D:\home\site\wwwroot" />
      </appSettings>
      <system.web>
        <compilation debug="true" targetFramework="4.0" />
      </system.web>
      <system.webServer>
        <modules runAllManagedModulesForAllRequests="true" />
        <handlers>
          <remove name="Python27_via_FastCGI" />
          <remove name="Python34_via_FastCGI" />
          <add name="Python FastCGI"
               path="handler.fcgi"
               verb="*"
               modules="FastCgiModule"
               scriptProcessor="D:\Python34\python.exe|D:\Python34\Scripts\wfastcgi.py"
               resourceType="Unspecified"
               requireAccess="Script" />
        </handlers>
        <rewrite>
          <rules>
            <rule name="Static Files" stopProcessing="true">
              <conditions>
                <add input="true" pattern="false" />
              </conditions>
            </rule>
            <rule name="Configure Python" stopProcessing="true">
              <match url="(.*)" ignoreCase="false" />
              <conditions>
                <add input="{REQUEST_URI}" pattern="^/static/.*" ignoreCase="true" negate="true" />
              </conditions>
              <action type="Rewrite" url="handler.fcgi/{R:1}" appendQueryString="true" />
            </rule>
          </rules>
        </rewrite>
      </system.webServer>
    </configuration>


Statische bestanden wordt verwerkt door de webserver rechtstreeks, zonder tussenkomst Python code voor verbeterde prestaties.

De locatie van de statische bestanden op schijf moet overeenkomen met de locatie in de URL in de voorbeelden hierboven. Dit betekent dat een aanvraag voor `http://pythonapp.azurewebsites.net/static/site.css` wordt het bestand op schijf bij dienen `\static\site.css`.

`WSGI_ALT_VIRTUALENV_HANDLER`is waar u de handler WSGI opgeven. In de bovenstaande voorbeelden wordt er `app.wsgi_app` omdat de handler een functie met de naam is `wsgi_app` in `app.py` in de hoofdmap.

`PYTHONPATH`kan zo worden aangepast, maar als u alle uw afhankelijkheden in de virtuele omgeving door het opgeven van deze in requirements.txt hebt geïnstalleerd, moet u niet mag wijzigen.


## <a name="virtual-environment-proxy"></a>Virtuele omgeving-Proxy

Het volgende script wordt gebruikt voor het ophalen van de handler WSGI, de virtuele omgeving en log fouten activeren. Dit is bedoeld om algemene en gebruikt zonder wijzigingen.

Inhoud van `ptvs_virtualenv_proxy.py`:

     # ############################################################################
     #
     # Copyright (c) Microsoft Corporation. 
     #
     # This source code is subject to terms and conditions of the Apache License, Version 2.0. A 
     # copy of the license can be found in the License.html file at the root of this distribution. If 
     # you cannot locate the Apache License, Version 2.0, please send an email to 
     # vspython@microsoft.com. By using this source code in any fashion, you are agreeing to be bound 
     # by the terms of the Apache License, Version 2.0.
     #
     # You must not remove this notice, or any other, from this software.
     #
     # ###########################################################################

    import datetime
    import os
    import sys
    import traceback

    if sys.version_info[0] == 3:
        def to_str(value):
            return value.decode(sys.getfilesystemencoding())

        def execfile(path, global_dict):
            """Execute a file"""
            with open(path, 'r') as f:
                code = f.read()
            code = code.replace('\r\n', '\n') + '\n'
            exec(code, global_dict)
    else:
        def to_str(value):
            return value.encode(sys.getfilesystemencoding())

    def log(txt):
        """Logs fatal errors to a log file if WSGI_LOG env var is defined"""
        log_file = os.environ.get('WSGI_LOG')
        if log_file:
            f = open(log_file, 'a+')
            try:
                f.write('%s: %s' % (datetime.datetime.now(), txt))
            finally:
                f.close()

    ptvsd_secret = os.getenv('WSGI_PTVSD_SECRET')
    if ptvsd_secret:
        log('Enabling ptvsd ...\n')
        try:
            import ptvsd
            try:
                ptvsd.enable_attach(ptvsd_secret)
                log('ptvsd enabled.\n')
            except: 
                log('ptvsd.enable_attach failed\n')
        except ImportError:
            log('error importing ptvsd.\n');

    def get_wsgi_handler(handler_name):
        if not handler_name:
            raise Exception('WSGI_ALT_VIRTUALENV_HANDLER env var must be set')
    
        if not isinstance(handler_name, str):
            handler_name = to_str(handler_name)
    
        module_name, _, callable_name = handler_name.rpartition('.')
        should_call = callable_name.endswith('()')
        callable_name = callable_name[:-2] if should_call else callable_name
        name_list = [(callable_name, should_call)]
        handler = None
        last_tb = ''

        while module_name:
            try:
                handler = __import__(module_name, fromlist=[name_list[0][0]])
                last_tb = ''
                for name, should_call in name_list:
                    handler = getattr(handler, name)
                    if should_call:
                        handler = handler()
                break
            except ImportError:
                module_name, _, callable_name = module_name.rpartition('.')
                should_call = callable_name.endswith('()')
                callable_name = callable_name[:-2] if should_call else callable_name
                name_list.insert(0, (callable_name, should_call))
                handler = None
                last_tb = ': ' + traceback.format_exc()
    
        if handler is None:
            raise ValueError('"%s" could not be imported%s' % (handler_name, last_tb))
    
        return handler

    activate_this = os.getenv('WSGI_ALT_VIRTUALENV_ACTIVATE_THIS')
    if not activate_this:
        raise Exception('WSGI_ALT_VIRTUALENV_ACTIVATE_THIS is not set')

    def get_virtualenv_handler():
        log('Activating virtualenv with %s\n' % activate_this)
        execfile(activate_this, dict(__file__=activate_this))

        log('Getting handler %s\n' % os.getenv('WSGI_ALT_VIRTUALENV_HANDLER'))
        handler = get_wsgi_handler(os.getenv('WSGI_ALT_VIRTUALENV_HANDLER'))
        log('Got handler: %r\n' % handler)
        return handler

    def get_venv_handler():
        log('Activating venv with executable at %s\n' % activate_this)
        import site
        sys.executable = activate_this
        old_sys_path, sys.path = sys.path, []
    
        site.main()
    
        sys.path.insert(0, '')
        for item in old_sys_path:
            if item not in sys.path:
                sys.path.append(item)

        log('Getting handler %s\n' % os.getenv('WSGI_ALT_VIRTUALENV_HANDLER'))
        handler = get_wsgi_handler(os.getenv('WSGI_ALT_VIRTUALENV_HANDLER'))
        log('Got handler: %r\n' % handler)
        return handler


## <a name="customize-git-deployment"></a>Cijfer implementatie aanpassen

[AZURE.INCLUDE [web-sites-python-customizing-runtime](../../includes/web-sites-python-customizing-deployment.md)]


## <a name="troubleshooting---package-installation"></a>Problemen oplossen bij - pakketinstallatie

[AZURE.INCLUDE [web-sites-python-troubleshooting-package-installation](../../includes/web-sites-python-troubleshooting-package-installation.md)]


## <a name="troubleshooting---virtual-environment"></a>Probleemoplossing - virtuele omgeving

[AZURE.INCLUDE [web-sites-python-troubleshooting-virtual-environment](../../includes/web-sites-python-troubleshooting-virtual-environment.md)]

## <a name="next-steps"></a>Volgende stappen

Zie het [Python Developer Center](/develop/python/)voor meer informatie.

>[AZURE.NOTE] Als u aan de slag met Azure App Service wilt voordat u zich registreert voor een Azure-account, gaat u naar de [App-Service probeert](http://go.microsoft.com/fwlink/?LinkId=523751), waar u direct een tijdelijk starter in de browser in de App-Service maken kunt. Geen creditcards vereist; geen verplichtingen.

## <a name="whats-changed"></a>Wat er gewijzigd
* Zie voor een handleiding voor het wijzigen van Websites naar App Service: [Azure App-Service en de invloed op bestaande Azure-Services](http://go.microsoft.com/fwlink/?LinkId=529714)





 
