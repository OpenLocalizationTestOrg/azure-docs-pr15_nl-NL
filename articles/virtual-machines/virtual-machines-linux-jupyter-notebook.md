<properties
    pageTitle="Maken van een notitieblok Jupyter/IPython | Microsoft Azure"
    description="Informatie over het implementeren van het notitieblok Jupyter/IPython op een Linux virtuele machine die zijn gemaakt met het model resource manager implementatie in Azure wordt aangegeven."
    services="virtual-machines-linux"
    documentationCenter="python"
    authors="crwilcox"
    manager="wpickett"
    editor=""
    tags="azure-service-management,azure-resource-manager"/>

<tags
    ms.service="virtual-machines-linux"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-linux"
    ms.devlang="python"
    ms.topic="article"
    ms.date="11/10/2015"
    ms.author="crwilcox"/>

# <a name="jupyter-notebook-on-azure"></a>Jupyter notitieblok op Azure

Het [Jupyter project](http://jupyter.org), voorheen het [IPython project](http://ipython.org), bevat een verzameling hulpprogramma's voor wetenschappelijk computing met krachtige interactieve houders die code worden uitgevoerd met het maken van een live rekenkundige document combineren. Deze notitieblokbestanden kunnen bevatten willekeurige tekst, wiskundige formules, invoer code, resultaten, afbeeldingen, video's en andere afbeeldingstypen media dat een modern webbrowser kan weergeven is. Of u absoluut geen ervaring met Python hebt en wilt deze informatie over in een omgeving leuke, interactieve of enkele ernstige parallel/technische computing doen, is het notitieblok Jupyter een uitstekende keus.

![Schermafbeelding](./media/virtual-machines-linux-jupyter-notebook/ipy-notebook-spectral.png) SciPy gebruiken en Matplotlib-pakketten te analyseren de structuur van het opnemen van een geluid.


## <a name="jupyter-two-ways-azure-notebooks-or-custom-deployment"></a>Jupyter twee manieren: De Azure notitieblokken of aangepaste implementatie
Azure biedt een service die u om [snel te Jupyter gebruiken gebruiken kunt ](http://blogs.technet.com/b/machinelearning/archive/2015/07/24/introducing-jupyter-notebooks-in-azure-ml-studio.aspx).  Met behulp van de Azure-Service voor notitieblok, kunt u snel toegang krijgen tot de Jupyter web toegankelijke interface naar scalable rekenkracht met de kracht van Python en de vele bibliotheken.  Aangezien de installatie wordt uitgevoerd door de service, de gebruikers hebben toegang tot deze resources zonder dat u voor beheer en configuratie door de gebruiker worden weergegeven.

Als het notitieblok-service niet voor uw scenario werkt gaat u verder te lezen in dit artikel waarin wordt wordt uitgelegd hoe u het notitieblok Jupyter op Microsoft Azure implementeren met Linux virtuele machines (VMs).

[AZURE.INCLUDE [create-account-and-vms-note](../../includes/create-account-and-vms-note.md)]

## <a name="create-and-configure-a-vm-on-azure"></a>Maken en configureren van een VM op Azure

De eerste stap is het opzetten van een virtuele machine (VM) op Azure uitgevoerd.
Deze VM is een volledige besturingssysteem wordt uitgevoerd in de cloud en wordt gebruikt voor het uitvoeren van het notitieblok Jupyter. Azure kan worden uitgevoerd Linux- en Windows virtuele machines is en aan bod komen de instellingen van Jupyter op beide soorten virtuele machines.

### <a name="create-a-linux-vm-and-open-a-port-for-jupyter"></a>Maak een VM Linux en een poort voor Jupyter openen

Volg de instructies opgegeven [hier] [ portal-vm-linux] een virtuele machine van de verdeling *Ubuntu* maken. In deze zelfstudie wordt Ubuntu Server 14.04 LTS. We gaan ervan uit dat de gebruiker naam *azureuser*.

Nadat de virtuele machine implementeert nodig we om een regel op de beveiligingsgroep van netwerk te openen.  Ga naar **Beveiligingsgroepen netwerk** van de Azure-portal en open het tabblad voor de beveiligingsgroep die overeenkomt met uw VM. U nodig hebt voor het toevoegen van een regel voor binnenkomende verbindingen met de volgende instellingen: **TCP** voor het protocol **\*** voor de bronpoort (openbaar) en **9999** voor de doelpoort (persoonlijk).

![Schermafbeelding](./media/virtual-machines-linux-jupyter-notebook/azure-add-endpoint.png)

Terwijl u in uw netwerk-beveiligingsgroep, klik op **Netwerk-Interfaces** en noteer het **Openbare IP-adres** als dit nodig is om te verbinden met uw VM in de volgende stap.

## <a name="install-required-software-on-the-vm"></a>Vereiste software installeren op de VM

Het notitieblok Jupyter op onze VM uitgevoerd, moeten we eerst Jupyter en de bijbehorende afhankelijkheden installeren. Verbinding maken met uw linux vm gebruiken ssh en de gebruikersnaam en wachtwoord koppelen u ervoor hebt gekozen wanneer u de vm hebt gemaakt. In deze zelfstudie we stopverf gebruiken en verbinding maken van Windows.

### <a name="installing-jupyter-on-ubuntu"></a>Installatie van Jupyter op Ubuntu
Installeer Anaconda, een populaire gegevens wetenschappelijke python-verdeling met een van de koppelingen van [Continue analyseprogramma](https://www.continuum.io/downloads).  Vanaf het schrijven van dit document, de onderstaande koppelingen zijn optimaal snel aan de datum-versies.

#### <a name="anaconda-installs-for-linux"></a>Anaconda-installaties voor Linux
<table>
  <th>Python 3.4</th>
  <th>Python 2.7</th>
  <tr>
    <td>
        <a href='https://3230d63b5fc54e62148e-c95ac804525aac4b6dba79b00b39d1d3.ssl.cf1.rackcdn.com/Anaconda3-2.3.0-Linux-x86_64.sh'>64-bits</href>
    </td>
    <td>
        <a href='https://3230d63b5fc54e62148e-c95ac804525aac4b6dba79b00b39d1d3.ssl.cf1.rackcdn.com/Anaconda-2.3.0-Linux-x86_64.sh'>64-bits</href>
    </td>
  </tr>
  <tr>
    <td>
        <a href='https://3230d63b5fc54e62148e-c95ac804525aac4b6dba79b00b39d1d3.ssl.cf1.rackcdn.com/Anaconda3-2.3.0-Linux-x86.sh'>32-bits</href>
    </td>
    <td>
        <a href='https://3230d63b5fc54e62148e-c95ac804525aac4b6dba79b00b39d1d3.ssl.cf1.rackcdn.com/Anaconda-2.3.0-Linux-x86.sh'>32-bits</href>
    </td>  
  </tr>
</table>


#### <a name="installing-anaconda3-230-64-bit-on-ubuntu"></a>Installatie van Anaconda3 2.3.0 64-bits op Ubuntu
Als u bijvoorbeeld is dit hoe u Anaconda op Ubuntu kunt installeren

    # install anaconda
    cd ~
    mkdir -p anaconda
    cd anaconda/
    curl -O https://3230d63b5fc54e62148e-c95ac804525aac4b6dba79b00b39d1d3.ssl.cf1.rackcdn.com/Anaconda3-2.3.0-Linux-x86_64.sh
    sudo bash Anaconda3-2.3.0-Linux-x86_64.sh -b -f -p /anaconda3

    # clean up home directory
    cd ..
    rm -rf anaconda/

    # Update Jupyter to the latest install and generate its config file
    sudo /anaconda3/bin/conda install jupyter -y
    /anaconda3/bin/jupyter-notebook --generate-config


![Schermafbeelding](./media/virtual-machines-linux-jupyter-notebook/anaconda-install.png)


### <a name="configuring-jupyter-and-using-ssl"></a>Jupyter configureren en gebruiken van SSL
Na de installatie van moeten we even de tijd om de configuratiebestanden voor dit Jupyter. Als u nog steeds ondervindt dreigen met het configureren van Jupyter is het wellicht handig om de [Jupyter documentatie voor het uitvoeren van een notitieblok-Server](http://jupyter-notebook.readthedocs.org/en/latest/public_server.html)te bekijken.

Volgende we `cd` naar de profielmap onze SSL-certificaat maken en bewerken van het configuratiebestand profielen.

Gebruik de volgende opdracht uit op Linux.

    cd ~/.jupyter

Gebruik de volgende opdracht uit om te maken van het SSL-certificaat (Linux en Windows).

    openssl req -x509 -nodes -days 365 -newkey rsa:1024 -keyout mycert.pem -out mycert.pem

Houd er rekening mee dat omdat u een zelfondertekend SSL-certificaat, wanneer u verbinding maakt met het notitieblok van uw browser geeft wel een beveiligingswaarschuwing weergegeven.  Voor gebruik in langdurige productieomgeving, wordt u wilt gebruiken, een goed ondertekend certificaat dat is gekoppeld aan uw organisatie.  Aangezien Certificaatbeheer buiten het bereik van deze demo valt, zullen we nu een zelfondertekend certificaat blijven.

Naast een certificaat, moet u ook een wachtwoord om te voorkomen dat uw notitieblok niet-geautoriseerd gebruik opgeven.  Vanwege de beveiliging gebruikt Jupyter gecodeerde wachtwoorden in de configuratiebestand, dus moet u eerst uw wachtwoord versleutelen.  IPython biedt een hulpprogramma hiervoor; Voer de volgende opdracht opdrachtprompt.

    /anaconda3/bin/python -c "import IPython;print(IPython.lib.passwd())"

Hiermee vraagt u om een wachtwoord en bevestiging en klikt u vervolgens het wachtwoord wordt afgedrukt. Opmerking dit voor de volgende stap.

    Enter password:
    Verify password:
    sha1:b86e933199ad:a02e9592e59723da722.. (elided the rest for security)

Vervolgens we bewerken van het profiel configuratiebestand, dat wil zeggen de `jupyter_notebook_config.py` bestand in de map die u bezig bent.  Opmerking dat dit bestand niet bestaat kan, net als dat het geval maken.  Dit bestand heeft een aantal velden en al dan niet standaard zijn alle toegelicht af.  U kunt dit bestand niet openen met elke teksteditor van uw eigen smaak en moet u ervoor zorgen dat er ten minste de volgende inhoud. **Zorg ervoor dat u voor het vervangen van de c.NotebookApp.password in config met de sha1 uit de vorige stap**.

    c = get_config()

    # You must give the path to the certificate file.
    c.NotebookApp.certfile = u'/home/azureuser/.jupyter/mycert.pem'

    # Create your own password as indicated above
    c.NotebookApp.password = u'sha1:b86e933199ad:a02e9592e5 etc... '

    # Network and browser details. We use a fixed port (9999) so it matches
    # our Azure setup, where we've allowed traffic on that port
    c.NotebookApp.ip = '*'
    c.NotebookApp.port = 9999
    c.NotebookApp.open_browser = False

### <a name="run-the-jupyter-notebook"></a>Het notitieblok Jupyter uitvoeren

We zijn op dit moment kunt beginnen met het notitieblok Jupyter. Ga hiervoor naar de map die u wilt opslaan van notitieblokken in en de server Jupyter notitieblok starten met de volgende opdracht uit.

    /anaconda3/bin/jupyter-notebook

U moet nu uw notitieblok Jupyter op het adres dat toegang tot `https://[PUBLIC-IP-ADDRESS]:9999`.

Wanneer u uw notitieblok voor het eerst opent, wordt de aanmeldingspagina gevraagd om uw wachtwoord. En wanneer u zich hebt aangemeld, ziet u het 'Jupyter notitieblok Dashboard', namelijk het beheercentrum ontrole voor alle bewerkingen van het notitieblok.  Op deze pagina kunt u nieuwe notitieblokken maken en vervolgens opent bestaande ideeën.

![Schermafbeelding](./media/virtual-machines-linux-jupyter-notebook/jupyter-tree-view.png)

### <a name="using-the-jupyter-notebook"></a>Het notitieblok Jupyter

Als u op de knop **Nieuw** klikt, ziet u de volgende pagina van de openen.

![Schermafbeelding](./media/virtual-machines-linux-jupyter-notebook/jupyter-untitled-notebook.png)

Het gebied dat is gemarkeerd met een `In []:` prompt is het invoergebied en hier kunt u een geldige code, Python typen en deze wordt uitgevoerd als u raakt `Shift-Enter` of klik op het pictogram "Afspelen" (de rechts wijzende driehoek op de werkbalk).

## <a name="a-powerful-paradigm-live-computational-documents-with-rich-media"></a>Een krachtige model: rekenkundige documenten met rijke media live

Het notitieblok zelf het is belangrijk zeer natuurlijke voor iedereen die is gebruikt Python en een tekstverwerkingsprogramma omdat dit een combinatie van beide in de volgende manieren is: u kunt blokken Python code uitvoeren, maar u kunt ook laten notities en andere tekst door te wijzigen van de stijl van een cel van 'Code' naar 'Korting' met de vervolgkeuzelijst op de werkbalk.

Jupyter is veel meer dan een tekstverwerkingsprogramma als deze kan het mengen van berekeningen en uitgebreide media (tekst, afbeeldingen, video en vrijwel die een modern webbrowser kunt weergeven). U kunt combineren, tekst, code, video's en meer!

![Schermafbeelding](./media/virtual-machines-linux-jupyter-notebook/jupyter-editing-experience.png)

En met de kracht van Python van groot aantal bronnen met uitstekende bibliotheken voor wetenschappelijke en technische computing, in de volgende schermafbeelding, dat een eenvoudige berekening kan worden uitgevoerd met net zo gemakkelijk als een analyse van complexe netwerk, alles in één omgeving.

In dit model mengen van de kracht van het moderne web met live berekening biedt vele mogelijkheden en is ideaal voor de cloud; het notitieblok kan worden gebruikt:

* Werken als een rekenkundige Kladblok experimentele opnemen op een probleem.

* Resultaten delen met collega's in rekenkundige formulier 'live' of in papier indelingen (HTML, PDF-bestand).

* Als u wilt verspreiden en presenteren live lesmateriaal waarbij u gebruikmaakt van berekening, zodat de leerlingen/studenten onmiddellijk kunnen experimenteren met de reële code, wijzigen en opnieuw interactief uit te voeren.

* Te leveren "uitvoerbare papier" dat de resultaten van onderzoek in een manier die kan worden direct gereproduceerd presenteren, gevalideerd en uitgebreide door anderen.

* Als een platform voor samenwerking computing: meerdere gebruikers kunnen aanmelden bij de hetzelfde notitieblok server om een live rekenkundige sessie te delen.


Als u naar de IPython bron code [opslagplaats][]gaat, vindt u een hele adreslijst met voorbeelden van het notitieblok waarin u kunt downloaden en klik vervolgens experimenteren met op uw eigen Azure Jupyter VM.  Gewoon downloaden de `.ipynb` bestanden van de site en uploadt u ze naar het dashboard van uw notitieblok Azure VM (of kunt u deze rechtstreeks in de VM downloaden).

## <a name="conclusion"></a>Sluiten

Het notitieblok Jupyter biedt een krachtige interface voor interactief toegang tot de macht van het systeem Python op Azure.  Deze bedekt een breed scala van gebruik gevallen, met inbegrip van eenvoudige verkennen en te leren Python, gegevensanalyse en visualisatie, simulaties en parallelle computing. De resulterende notitieblokdocumenten bevatten een volledig overzicht van de berekeningen die worden uitgevoerd en kunnen worden gedeeld met andere gebruikers Jupyter.  Het notitieblok Jupyter kunnen worden gebruikt als een lokale toepassing, maar deze is ideaal voor cloud-implementaties op Azure

De gewenste basisfuncties van Jupyter zijn ook beschikbaar in Visual Studio via de [Python Tools voor Visual Studio][] (PTVS). PTVS is een gratis en open source invoegtoepassing van Microsoft die Visual Studio in een geavanceerde Python ontwikkelomgeving waarin een geavanceerde editor met IntelliSense foutopsporing, verandert profielbeleid en parallelle computing integratie.

## <a name="next-steps"></a>Volgende stappen

Zie het [Python Developer Center](/develop/python/)voor meer informatie.

[portal-vm-linux]: https://azure.microsoft.com/en-us/documentation/articles/virtual-machines-linux-tutorial-portal-rm/
[opslagplaats]: https://github.com/ipython/ipython
[Python Tools voor Visual Studio]: http://aka.ms/ptvs
