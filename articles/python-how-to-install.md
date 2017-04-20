<properties
    pageTitle="Python en de SDK - Azure installeren"
    description="Informatie over het installeren van Python en de SDK voor gebruik met Azure."
    services=""
    documentationCenter="python"
    authors="lmazuel"
    manager="wpickett"
    editor=""/>

<tags
    ms.service="multiple"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="python"
    ms.topic="article"
    ms.date="09/06/2016"
    ms.author="lmazuel"/>

# <a name="installing-python-and-the-sdk"></a>Installatie van Python en de SDK

Python kunt u eenvoudig instellen in Windows en zijn vooraf geïnstalleerd op de Mac, Linux en [Bash voor Windows](https://msdn.microsoft.com/commandline/wsl/about). Deze handleiding helpt u bij de installatie- en uw computer gereed voor gebruik met Azure aan.

## <a name="whats-in-the-python-azure-sdk"></a>Wat staat er in de Python Azure SDK?

De SDK Azure voor Python bevat onderdelen waarmee u kunt de ontwikkeling, implementatie en Python toepassingen voor Azure beheren. De SDK Azure voor Python bevat de volgende gegevens op:

* **Management bibliotheken**. Deze bibliotheken class bieden een interface Azure bronnen, zoals virtuele machines-opslag accounts beheren.

* **Runtime-bibliotheken**. Deze bibliotheken class bieden een interface voor toegang tot Azure functies, zoals opslagruimte en service bus.

## <a name="which-python-and-which-version-to-use"></a>Welke Python en welke versie u wilt gebruiken

Er zijn verschillende versies van Python interpreters beschikbaar: voorbeelden hiervan zijn:

* CPython - de standaard- en meest gebruikte Python interpreter
* PyPy - snelle, compatibele alternatieve implementatie naar CPython
* IronPython - Python programma waarmee het wordt uitgevoerd op .net/CLR
* Jython - Python programma waarmee het wordt uitgevoerd op de Java Virtual Machine

**CPython** v2.7 of v3.3 + en PyPy 5.4.0 worden getest en worden ondersteund voor de SDK van de Azure Python.

## <a name="where-to-get-python"></a>Waar moet Python krijgen?

Er zijn verschillende manieren om CPython:

* Rechtstreeks vanuit [www.python.org][]
* Vanuit een vertrouwde distro zoals [www.continuum.io][], [www.enthought.com][] of [www.activestate.com][]
* Maken van bron!

Tenzij u een specifieke nodig hebt, wordt aangeraden de eerste twee opties.

## <a name="sdk-installation-on-windows-linux-and-macos-client-libraries-only"></a>SDK installatie op Windows, Linux en Mac OS (alleen clientbibliotheken)

Als u al Python is geïnstalleerd, kunt u pip gebruiken voor het installeren van een bundel van alle clientbibliotheken die de in uw bestaande Python 2.7 of Python 3.3 +-omgeving. Hiermee wordt de pakketten downloaden uit de [Python pakket Index][] (PyPI).

Mogelijk moet u beheerdersrechten:

- Linux en Mac OS, gebruikt u de `sudo` opdracht: `sudo pip install azure-mgmt-compute`.
- Windows: open de PowerShell-/ opdrachtprompt als beheerder

U kunt afzonderlijk elke bibliotheek voor elke Azure-service installeren:

```console
   $ pip install azure-batch          # Install the latest Batch runtime library
   $ pip install azure-mgmt-scheduler # Install the latest Storage management library
```

Voorbeeld-pakketten kunnen worden geïnstalleerd met de `--pre` vlag:

```console
   $ pip install --pre azure-mgmt-compute # will install only the latest Compute Management library
```

U kunt ook een reeks Azure bibliotheken installeren in een ononderbroken lijn met de `azure` metagegevens-pakket. Aangezien niet alle pakketten in dit metagegevens-pakket zijn gepubliceerd ofwel nog, de `azure` metagegevens-pakket zich nog in de Preview-versie. Echter kunnen de core-pakketten van code kwaliteit/volledigheid perspectieven worden beschouwd, "stabiele" op dit moment
- Deze officieel heet als zodanig gesynchroniseerd met andere talen zo snel mogelijk. We hebben geen tot typt u vervolgens op verder grote wijzigingen plannen.

Aangezien het een preview-versie, u moet gebruiken de `--pre` vlag:

```console
   $ pip install --pre azure
```
   
of rechtstreeks

```console
   $ pip install azure==2.0.0rc6
```

## <a name="getting-more-packages"></a>Meer-pakketten ophalen

De [Python pakket Index][] (PyPI) heeft een uitgebreide selectie van Python bibliotheken.  Als u ervoor kiest u een Distro installeert, hebt u al meest interessant bits voor verschillende scenario's van webontwikkeling naar technische Computing.


## <a name="python-tools-for-visual-studio"></a>Python Tools voor Visual Studio

[Python Tools voor Visual Studio][] (PTVS) is een gratis/OSS-invoegtoepassing van Microsoft, die tegenover in een volwaardige Python IDE verandert:

![How-naar-installatie-python-ptvs](./media/python-how-to-install/how-to-install-python-ptvs.png)

Gebruik van PTVS is optioneel, maar wordt aanbevolen zoals u kunt wel Python en Web Project/oplossing ondersteuning, foutopsporing, profielen, interactieve venster, sjabloon bewerken en Intellisense.

PTVS ook eenvoudig te implementeren naar Microsoft Azure met ondersteuning voor implementatie naar [Cloud Services][] en [Websites][].

PTVS werkt met uw bestaande Visual Studio-2013 of 2015 installatie.  Zie voor documentatie, downloads en discussies, [Python Tools voor Visual Studio].  

## <a name="python-azure-scenarios-for-linux-and-macos"></a>Python Azure scenario's voor Linux en Mac OS

Voor Linux of Mac OS, belangrijkste Azure scenario's die worden ondersteund:

1. Azure Services gebruiken met behulp van de clientbibliotheken voor Python

2. Uw app uitgevoerd in een VM Linux

3. Ontwikkelen en publiceren naar cijfer met Azure-Websites

Het eerste scenario kunt u uitgebreide WebApps die van de mogelijkheden van de Azure PaaS zoals [blobopslag][], [wachtrij opslag][], [tabelopslag][] enzovoort via Pythonic aanvullende inhoud voor de Azure REST API's gebruikmaken van de auteur. Deze werkt identiek voor Windows, Mac en Linux.  U kunt ook deze clientbibliotheken gebruiken van uw lokale development-computer of een Linux VM waarop Azure.

Voor het scenario VM u gewoon een VM Linux van uw keuze (Ubuntu, CentOS, Suse) starten en uitvoeren/beheren wat u tevreden bent.  U kunt bijvoorbeeld [IPython][] REPL/notitieblok uitvoeren op uw Mac-Windows/Linux-computer en uw browser naar een Linux of Windows meerdere processors VM uitvoeren van de IPython Engine op Azure verwijzen. Zie de zelfstudie [IPython notitieblok op Azure][] voor meer informatie.

Voor informatie over hoe u setup een VM Linux, raadpleegt u de zelfstudie [maken een virtuele Machine uitgevoerd Linux][] .

Cijfer implementatie gebruikt, kunt u een webtoepassing Python ontwikkelen en publiceren naar een Website Azure op een ander besturingssysteem.  Wanneer u uw bibliotheek naar Azure drukt, wordt er automatisch een virtuele omgeving gemaakt en pip uw vereiste pakketten installeren.

Zie voor meer informatie over het ontwikkelen en publiceren van Azure Websites, de zelfstudies voor [Websites maken met Django][], [Websites maken met flessen][]en [Websites maken met kolf][]. Zie voor meer algemene informatie over het gebruik van een WSGI-compatibele framework [Python configureren met Azure-Websites][].


## <a name="additional-software-and-resources"></a>Extra Software en bronnen:

* [Azure SDK voor Python ReadTheDocs](http://azure-sdk-for-python.readthedocs.io/en/latest/)
* [Azure SDK voor Python Github](https://github.com/Azure/azure-sdk-for-python)
* [Officiële Azure voorbeelden voor Python](https://azure.microsoft.com/documentation/samples/?platform=python)
* [Continue Analytics Python verdeling][]
* [Enthought Python verdeling][]
* [ActiveState Python verdeling][]
* [SciPy - een reeks wetenschappelijke Python bibliotheken][]
* [NumPy - een diatheek numerieke waarden voor Python][]
* [Django Project - een volwaardige web framework/CMS][]
* [IPython - een geavanceerde REPL/notitieblok voor Python][]
* [IPython notitieblok op Azure][]
* [Python Tools voor Visual Studio op GitHub][]
* [Python Developer Center](/develop/python/)

[Continue Analytics Python verdeling]: http://continuum.io
[Enthought Python verdeling]: http://www.enthought.com
[ActiveState Python verdeling]: http://www.activestate.com
[www.Python.org]: http://www.python.org
[www.continuum.IO]: http://continuum.io
[www.enthought.com]: http://www.enthought.com
[www.ActiveState.com]: http://www.activestate.com
[SciPy - een reeks wetenschappelijke Python bibliotheken]: http://www.scipy.org
[NumPy - een diatheek numerieke waarden voor Python]: http://www.numpy.org
[Django Project - een volwaardige web framework/CMS]: http://www.djangoproject.com
[IPython - een geavanceerde REPL/notitieblok voor Python]: http://ipython.org
[IPython]: http://ipython.org
[IPython notitieblok op Azure]: virtual-machines-linux-jupyter-notebook.md
[Cloudservices]: cloud-services-python-ptvs.md
[Websites]: web-sites-python-ptvs-django-mysql.md
[Python Tools voor Visual Studio]: http://aka.ms/ptvs
[Python Tools voor Visual Studio op GitHub]: https://github.com/microsoft/ptvs
[Python pakket Index]: http://pypi.python.org/pypi
[Microsoft Azure SDK for Python 2.7]: http://go.microsoft.com/fwlink/?LinkId=254281
[Microsoft Azure SDK for Python 3.4]: http://go.microsoft.com/fwlink/?LinkID=516990
[Setting up a Linux VM via the Azure portal]: create-and-configure-opensuse-vm-in-portal.md
[How to use the Azure Command-Line Interface]: crossplat-cmd-tools.md
[Een virtuele Machine waarop Linux maken]: virtual-machines-linux-quick-create-cli.md
[Websites maken met Django]: web-sites-python-create-deploy-django-app.md
[Websites maken met flessen]: web-sites-python-create-deploy-bottle-app.md
[Websites maken met kolf]: web-sites-python-create-deploy-flask-app.md
[Python configureren met Azure-Websites]: web-sites-python-configure.md
[-tabelopslag]: storage-python-how-to-use-table-storage.md
[wachtrij opslag]: storage-python-how-to-use-queue-storage.md
[-blobopslag]: storage-python-how-to-use-blob-storage.md
